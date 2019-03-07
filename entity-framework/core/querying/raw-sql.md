---
title: Ham SQL sorguları - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: ad7ac3099cfd4c49b88acfbbff61f2af9294b6ec
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463249"
---
# <a name="raw-sql-queries"></a>Ham SQL sorguları

Entity Framework Core, ilişkisel bir veritabanı ile çalışırken, ham SQL sorguları açılan olanak tanır. Gerçekleştirmek istediğiniz sorguyu LINQ kullanılarak ifade edilemediğinde ya da verimsiz SQL sorgularında LINQ sorgusu kullanarak kaynaklanan bu yararlı olabilir. Ham SQL sorguları varlık türleri veya EF Core 2.1 ile başlayan döndürebilir [sorgu türü](xref:core/modeling/query-types) modelinizin bir parçası.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

## <a name="basic-raw-sql-queries"></a>Temel ham SQL sorguları

Kullanabileceğiniz *SQL* ham SQL sorgu temelli bir LINQ Sorgu başlamak için genişletme yöntemi.

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

SQL kabul eden bir API ile gibi SQL ekleme saldırısına karşı korumak için tüm kullanıcı parametre haline getirmek önemlidir. SQL sorgu dizesi parametresi yer tutucular içerir ve sonra ek bağımsız değişkenler olarak parametre değerlerini sağlayın. Sağladığınız parametre değerlerini otomatik olarak dönüştürülür bir `DbParameter`.

Aşağıdaki örnek, bir saklı yordam için tek bir parametre geçirir. Bu görünebilir ancak ister `String.Format` söz dizimi, sağlanan değer kaydırılır bir parametre ve oluşturulan parametre adı eklenen nerede `{0}` yer tutucu belirtildi.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Aynıdır, ancak EF Core 2.0 ve üzeri sürümlerde desteklenir. dize ilişkilendirme sözdizimini kullanarak sorgu:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Ayrıca, bir DbParameter oluşturun ve parametre değeri olarak sağlayın. Bu, SQL sorgu dizesinde adlandırılmış parametreler kullanmanıza olanak sağlar.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>LINQ ile oluşturma

Ardından SQL sorgusuna üzerinde veritabanında kullanılıp kullanılamayacağı, LINQ işleçleri kullanarak ilk ham SQL sorgusunun üstüne oluşturabilirsiniz. Üzerinde oluşan SQL sorguları ile başlayan `SELECT` anahtar sözcüğü.

Aşağıdaki örnek filtreleme ve sıralama gerçekleştirmek için LINQ kullanarak bir Table-Valued işlev (TVF) öğesinden seçer ve ardından ölçeklemesini ham bir SQL sorgusu kullanır.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a>Değişiklik İzleme

Sorgular kullanan `FromSql()` EF Core, diğer bir LINQ sorgusu olarak kuralları izleme tam aynı değişikliği izleyin. Örneğin, sorgu varlık türleri projeleri sonuçları varsayılan olarak izlenir.  

Aşağıdaki örnek, bir Table-Valued işlev (TVF) öğesinden seçer ham bir SQL sorgusu kullanır ve ardından değişiklik izleme çağrısıyla devre dışı bırakır. AsNoTracking():

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>İlgili verileri de dahil olmak üzere

`Include()` Yöntemi, herhangi bir LINQ Sorgu ile olduğu gibi ilgili verileri dahil etmek için kullanılabilir:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>Sınırlamalar

Ham SQL sorguları kullanırken dikkat edilmesi gereken bazı sınırlamalar vardır:

* SQL sorgusu, veri varlığı veya sorguyu türü tüm özelliklerde için döndürmesi gerekir.

* Sonuç kümesi sütun adları, özellikler için eşlenen sütun adları eşleşmelidir. Bu özellik/sütun eşlemesi için ham SQL sorguları burada yoksayıldı EF6 farklı olduğuna dikkat edin ve sonuç kümesi sütun adları, özellik adlarının eşleşmesi gerekiyordu.

* SQL sorgusu, ilgili verileri içeremez. Ancak, çoğu durumda, üzerinde sorgu kullanarak oluşturabileceğiniz `Include` ilgili verileri döndürmek için işleci (bkz [ilgili veriler dahil olmak üzere](#including-related-data)).

* `SELECT` Bu yönteme geçirilen deyimler genellikle birleştirilebilir olması gerekir: EF Core ek sorgu işleçleri sunucusunda değerlendirilecek gerekip gerekmediğini (örneğin, LINQ işleçleri çevirmek için uygulanan sonra `FromSql`), sağlanan SQL alt sorgu kabul edilir. Bu, geçirilen SQL herhangi bir karakter veya gibi geçerli bir alt sorgu olmayan seçenekleri içermemelidir anlamına gelir:
  * sondaki noktalı virgül
  * SQL Server'da izleyen bir sorgu düzeyi İpucu (örneğin, `OPTION (HASH JOIN)`)
  * SQL Server'da bir `ORDER BY` , eşlik yan tümcesi `TOP 100 PERCENT` içinde `SELECT` yan tümcesi

* SQL deyimleri dışında `SELECT` otomatik birleştirilebilir olmayan tanınır. Sonuç olarak, saklı yordamları tam sonuçları her zaman istemciye döndürülen ve sonra herhangi bir LINQ işlecini uygulandı `FromSql` değerlendirilen bellek içi olduğu.

> [!WARNING]  
> **Her zaman için ham SQL sorguları Parametreleştirme kullanın:** Kullanıcı girişini doğrulama ek olarak, her zaman Parametreleştirme ham bir SQL sorgu/komutta kullanılan herhangi bir değeri için kullanın. Ham SQL kabul API'leri gibi dize `FromSql` ve `ExecuteSqlCommand` değerleri kolayca parametre olarak geçirilmesine izin verin. Overloads biri `FromSql` ve `ExecuteSqlCommand` FormattableString kabul dize ilişkilendirme syntaxt SQL ekleme saldırılarına karşı korunmasına yardımcı olan bir şekilde kullanarak da izin. 
> 
> Dize birleştirme veya ilişkilendirme dinamik olarak herhangi bir sorgu dizesi bölümünü kullanarak veya deyimleri ya da bu girişleri dinamik SQL yürütebilir saklı yordamlar için kullanıcı girişi geçirerek, ardından herhangi bir giriş doğrulamak için sorumlu olursunuz SQL ekleme saldırılarına karşı koruyun.
