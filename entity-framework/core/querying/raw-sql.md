---
title: Ham SQL sorguları-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: b0c9ba1bb452e47e8348d000e3f7b88cc2730d8e
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149309"
---
# <a name="raw-sql-queries"></a>Ham SQL sorguları

Entity Framework Core, ilişkisel bir veritabanıyla çalışırken ham SQL sorgularına karşı aşağı açılan bir liste sağlar. Bu, gerçekleştirmek istediğiniz sorgu LINQ kullanılarak ifade edilenemediğini veya bir LINQ sorgusunun kullanılması verimsiz SQL sorgularının oluşmasına neden olduğunda yararlı olabilir. Ham SQL sorguları, modelinizde bir parçası olan EF Core 2,1 ve [anahtarsız varlık türlerinden](xref:core/modeling/keyless-entity-types) başlayarak varlık türlerini veya döndürebilir.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) GitHub üzerinde.

## <a name="basic-raw-sql-queries"></a>Temel ham SQL sorguları

Bir ham SQL sorgusuna dayalı bir LINQ sorgusuna başlamak için *fromsql* Extension yöntemini kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Ham SQL sorguları, saklı bir yordamı yürütmek için kullanılabilir.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Parametreleri geçirme

SQL 'i kabul eden herhangi bir API 'de olduğu gibi, bir SQL ekleme saldırısına karşı korunmak için herhangi bir kullanıcı girişini parametreleştirmek önemlidir. SQL sorgu dizesinde parametre yer tutucuları dahil edebilir ve ardından parametre değerlerini ek bağımsız değişkenler olarak sağlayabilirsiniz. Sağladığınız herhangi bir parametre değeri otomatik olarak bir `DbParameter`olarak dönüştürülür.

Aşağıdaki örnek, saklı yordama tek bir parametre geçirir. Bu, söz dizimi gibi `String.Format` görünebilir, ancak sağlanan değer bir parametreye sarmalanır ve `{0}` yer tutucunun belirtildiği yerde oluşturulan parametre adı eklenir.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Bu sorgu, ancak EF Core 2,0 ve üzeri sürümlerde desteklenen dize ilişkilendirme sözdizimi kullanılarak aynıdır:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Ayrıca bir DbParameter oluşturup bunu bir parametre değeri olarak sağlayabilirsiniz:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Bu, bir saklı yordam isteğe bağlı parametrelere sahip olduğunda yararlı olan SQL sorgu dizesinde adlandırılmış parametreleri kullanmanıza olanak sağlar:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>LINQ ile oluşturma

SQL sorgusu veritabanında yer alıyorsa, LINQ işleçlerini kullanarak ilk ham SQL sorgusunun üzerine oluşturabilirsiniz. Üzerinde birleştirilebilen SQL sorguları `SELECT` anahtar sözcüğüyle başlar.

Aşağıdaki örnek, tablo değerli bir Işlevden (TVF) seçen ham bir SQL sorgusu kullanır ve ardından filtreleme ve sıralamayı gerçekleştirmek için LINQ kullanarak buna dayanır.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a>Değişiklik İzleme

Kullanan sorgular, `FromSql()` EF Core ' deki diğer LINQ sorgularıyla tam olarak aynı değişiklik izleme kurallarını izler. Örneğin, sorgu projeleri varlık türlerdir, sonuçlar varsayılan olarak izlenir.  

Aşağıdaki örnek, bir tablo değerli Işlevden (TVF) seçim yapan ham bir SQL sorgusu kullanır ve sonra yapılan çağrısıyla değişiklik izlemeyi devre dışı bırakır. AsNoTracking ():

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>İlgili verileri dahil etme

`Include()` Yöntemi, diğer herhangi bir LINQ sorgusuyla olduğu gibi ilgili verileri dahil etmek için kullanılabilir:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>Sınırlamalar

Ham SQL sorguları kullanırken dikkat etmeniz için bazı sınırlamalar vardır:

* SQL sorgusu, varlığın veya sorgu türünün tüm özellikleri için veri döndürmelidir.

* Sonuç kümesindeki sütun adları, özelliklerin eşlendiği sütun adlarıyla eşleşmelidir. Bu, ham SQL sorguları için özellik/sütun eşlemesinin yoksayıldığı ve sonuç kümesi sütun adlarının Özellik adlarıyla eşleşmesi gerekiyordu EF6 öğesinden farklıdır.

* SQL sorgusu ilgili verileri içeremez. Bununla birlikte, çoğu durumda ilgili verileri ( [ilgili verileri dahil olmak](#including-related-data)üzere) döndürmek `Include` için işlecini kullanarak sorgunun üzerine oluşturabilirsiniz.

* `SELECT`Bu yönteme geçirilen deyimler genellikle birleştirilebilir olmalıdır: EF Core sunucuda ek sorgu işleçlerini değerlendirmesi gerekiyorsa (örneğin, daha sonra `FromSql`uygulanan LINQ işleçlerini çevirmek için), sağlanan SQL bir alt sorgu olarak değerlendirilir. Bu, geçirilen SQL 'in bir alt sorgu üzerinde geçerli olmayan herhangi bir karakter veya seçenek içermemesi gerektiği anlamına gelir; örneğin:
  * sondaki noktalı virgül
  * SQL Server, sondaki sorgu düzeyi İpucu (örneğin, `OPTION (HASH JOIN)`)
  * SQL Server, `ORDER BY` `SELECT` yan tümcelerinde `OFFSET 0` or `TOP 100 PERCENT` içinde olmayan bir yan tümce

* Dışındaki `SELECT` SQL deyimleri otomatik olarak birleştirilemeyen olarak tanınır. Sonuç olarak, saklı yordamların tam sonuçları istemciye her zaman döndürülür ve tüm LINQ işleçleri bellek içinde değerlendirildikten sonra `FromSql` uygulanır.

> [!WARNING]  
> **Ham SQL sorguları için her zaman Parametreleştirme kullanın:** Kullanıcı girişini doğrulamaya ek olarak, ham bir SQL sorgusunda/komutunda kullanılan değerler için her zaman Parametreleştirme kullanın. `FromSql` Ve gibi ham bir SQL dizesini kabul eden API 'ler `ExecuteSqlCommand` ve değerlerin kolayca parametre olarak geçirilmesini sağlar. FormattableString `ExecuteSqlCommand` kabul eden ve ' nin `FromSql` aşırı yüklemeleri, SQL ekleme saldırılarına karşı korumaya yardımcı olacak şekilde dize ilişkilendirme sözdiziminin kullanılmasına da imkan tanır. 
> 
> Sorgu dizesinin herhangi bir bölümünü dinamik olarak derlemek için dize birleştirme veya ilişkilendirme kullanıyorsanız veya bu girişleri dinamik SQL olarak yürütemeyen deyimlere veya saklı yordamlara geçirmeniz durumunda, herhangi bir girişin doğrulanması sizin sorumluluğunuzdadır SQL ekleme saldırılarına karşı koruma.
