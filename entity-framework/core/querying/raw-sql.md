---
title: Ham SQL sorguları - EF çekirdek
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 29b7e20e875bf791a88a92636c1df4bc4e31656b
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="raw-sql-queries"></a>Ham SQL sorguları

Entity Framework Çekirdek, ilişkisel veritabanı ile çalışırken, ham SQL sorguları açılan olanak sağlar. Gerçekleştirmek istediğiniz sorgu LINQ kullanılarak ifade edilemeyen veya bir LINQ Sorgu kullanarak verimsiz SQL veritabanına gönderilen kaynaklanan bu yararlı olabilir.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) github'da.

## <a name="limitations"></a>Sınırlamalar

Ham SQL sorguları kullanırken dikkat edilmesi gereken bazı sınırlamalar vardır:
* SQL sorguları yalnızca dönüş modelinizi parçası olan varlık türleri için kullanılabilir. Bizim biriktirme listesi üzerinde bir geliştirme yoktur [ham SQL sorgularından geçici türleri döndüren etkinleştir](https://github.com/aspnet/EntityFramework/issues/1862).

* SQL sorgusu, varlık veya sorgu türü tüm özelliklerde için veri döndürmesi gerekir.

* Sonuç kümesi içindeki sütun adlarının özellikleri eşlendiği sütun adlarının eşleşmesi gerekir. Bu özelliği/sütun eşlemesi için ham SQL sorguları yeri yoksayıldı EF6 farklı olduğuna dikkat edin ve sonuç kümesi sütunu adları özellik adlarının eşleşmesi gerekiyordu.

* SQL sorgusu, ilgili veri içeremez. Bununla birlikte, çoğu durumda, sorgu kullanarak üstünde oluşturabilirsiniz `Include` ilgili verileri döndürmek için işleci (bkz [ilgili verileri de dahil olmak üzere](#including-related-data)).

* `SELECT` Bu yönteme geçirilen deyimleri genellikle birleştirilebilir olmalıdır: varsa EF çekirdek gereken sunucu üzerindeki Ek sorgu işleçleri değerlendirmek (örneğin sonra uygulanan LINQ işleçleri çevirmek için `FromSql`), sağlanan SQL alt sorgu kabul edilir. Başka bir deyişle, geçirilen SQL herhangi bir karakter veya gibi bir alt sorgu geçerli olmayan seçenekler içermemelidir:
  * sondaki noktalı virgül
  * SQL Server'da sorgu düzeyi sondaki ipucu, örn. `OPTION (HASH JOIN)`
  * SQL Server'da bir `ORDER BY` , eşlik yan tümcesi `TOP 100 PERCENT` içinde `SELECT` yan tümcesi

* SQL deyimleri dışında `SELECT` otomatik birleştirilebilir olmayan tanınır. Sonuç olarak, tam sonuçları saklı yordamlar, her zaman istemciye döndürülen ve sonra LINQ işleçleri uygulanan `FromSql` değerlendirilen bellek içi şunlardır. 

## <a name="basic-raw-sql-queries"></a>Temel ham SQL sorguları

Kullanabileceğiniz *FromSql* ham SQL sorgu temelli bir LINQ Sorgu başlamak için genişletme yöntemi.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Ham SQL sorguları bir saklı yordamı yürütmek için kullanılabilir.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Parametreleri geçirme

SQL kabul eden herhangi bir API'yi gibi ile giriş SQL ekleme saldırısına karşı korumak için herhangi bir kullanıcı Parametreleştirme önemlidir. SQL sorgu dizesinde parametre yer tutucuları içerir ve ek bağımsız değişkenler olarak parametre değerlerini sağlayın. Sağladığınız herhangi bir parametre değeri için otomatik olarak dönüştürülecek bir `DbParameter`.

Aşağıdaki örnek, tek bir parametre saklı yordama geçirir. Bu görünebilir ancak ister `String.Format` sözdizimi, sağlanan değer kaydırılır içindeki bir parametre ve oluşturulan parametre adı eklenen nereye `{0}` yer tutucu belirtildi.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Bu aynı olduğundan sorgu ancak EF çekirdek 2.0 ve üzeri desteklenir dize ilişkilendirme sözdizimini kullanarak:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Ayrıca, bir DbParameter oluşturmak ve parametre değeri olarak sağlayın. Bu adlandırılmış parametreleri SQL sorgu dizesinde kullanmanıza olanak sağlar

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>LINQ ile oluşturma

Ardından SQL sorgusuna üzerinde veritabanında birleştirilebilen, LINQ işleçleri kullanarak ilk ham SQL sorgusu üstünde oluşturabilirsiniz. İle olan birleştirilebilen SQL sorguları `SELECT` anahtar sözcüğü.

Aşağıdaki örnek, bir tablo değerli fonksiyon (TVF) gelen seçer ve ardından oluşturur ham bir SQL sorgusu filtreleme ve sıralama gerçekleştirmek için LINQ kullanarak kullanır.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>İlgili verileri de dahil olmak üzere

LINQ işleçlerle oluşturma sorguda ilgili verileri dahil etmek için kullanılabilir.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Her zaman parametrelemeyi ham SQL sorgular için kullanın:** ham SQL kabul API'leri dize gibi `FromSql` ve `ExecuteSqlCommand` parametre olarak kolayca geçirilecek değerlere izin. Kullanıcı girişini doğrulama yanı sıra, her zaman parametrelemeyi ham bir SQL sorgusu/komutta kullanılan herhangi bir değeri için kullanın. Dize birleştirme SQL yerleştirme saldırılarına karşı korumak için herhangi bir giriş doğrulamak için sorumlu sonra herhangi bir kısmını sorgu dizesini dinamik olarak oluşturmak için kullanıyorsanız.
