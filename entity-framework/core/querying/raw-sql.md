---
title: Ham SQL sorguları-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813592"
---
# <a name="raw-sql-queries"></a>Ham SQL Sorguları

Entity Framework Core, ilişkisel bir veritabanıyla çalışırken ham SQL sorgularına karşı aşağı açılan bir liste sağlar. Bu, gerçekleştirmek istediğiniz sorgu LINQ kullanılarak ifade edilenemediğini veya bir LINQ sorgusunun kullanılması verimsiz bir SQL sorgusuyla sonuçlıysa yararlı olabilir. Ham SQL sorguları, modelinizin bir parçası olan normal varlık türlerini veya [anahtarsız varlık türlerini](xref:core/modeling/keyless-entity-types) döndürebilir.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) GitHub üzerinde.

## <a name="basic-raw-sql-queries"></a>Temel ham SQL sorguları

Ham SQL sorgusuna dayalı `FromSqlRaw` bir LINQ sorgusuna başlamak için genişletme yöntemini kullanabilirsiniz.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

Ham SQL sorguları, saklı bir yordamı yürütmek için kullanılabilir.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Parametreleri geçirme

> [!WARNING]
> **Ham SQL sorguları için her zaman Parametreleştirme kullanın**
>
> Bir ham SQL sorgusuna Kullanıcı tarafından sağlanmış herhangi bir değer alındığınızda, SQL ekleme saldırılarından kaçınmak için dikkatli olunması gerekir. Bu tür değerlerin geçersiz karakterler içermediğini doğrulamaya ek olarak, her zaman değerleri SQL metinden ayrı gönderen Parametreleştirme kullanın.
>
> Özellikle, hiç bir birleştirilmiş veya enterpolasyonlu dize`$""`() ' i doğrulanmamış kullanıcı tarafından sağlanmış değerlerle `FromSqlRaw` veya `ExecuteSqlRaw`içine geçirmeyin. `FromSqlInterpolated` Ve`ExecuteSqlInterpolated` yöntemleri, dize ilişkilendirme sözdiziminin SQL ekleme saldırılarına karşı korunmasını sağlayan bir biçimde kullanılmasına izin verir.

Aşağıdaki örnek, SQL sorgu dizesinde bir parametre yer tutucusu ekleyerek ve ek bir bağımsız değişken sağlayarak, saklı yordama tek bir parametre geçirir. Bu, söz dizimi gibi `String.Format` görünebilir, ancak sağlanan değer bir `DbParameter` ve `{0}` yer tutucunun belirtildiği yerde oluşturulmuş parametre adına sarmalanır.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

' A alternatif olarak `FromSqlRaw`, dize ilişkilendirmeden `FromSqlInterpolated` güvenli bir şekilde kullanılmasına izin veren ' yi kullanabilirsiniz. Önceki örnekte olduğu gibi, değer bir `DbParameter` öğesine dönüştürülür ve bu nedenle SQL ekleme ile ilgili değildir:

> [!NOTE]
> Sürüm 3,0 ' `FromSqlRaw` den önce ve `FromSqlInterpolated` adında `FromSql`iki aşırı yükleme yapılmıştır. Daha ayrıntılı bilgi için [önceki sürümler bölümüne](#previous-versions) bakın.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Ayrıca bir DbParameter oluşturup bunu bir parametre değeri olarak sağlayabilirsiniz. Bir dize yer tutucusu `FromSqlRaw` yerine normal bir SQL parametre yer tutucusu kullanıldığından, güvenli bir şekilde kullanılabilir:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Bu, bir saklı yordam isteğe bağlı parametrelere sahip olduğunda yararlı olan SQL sorgu dizesinde adlandırılmış parametreleri kullanmanıza olanak sağlar:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>LINQ ile oluşturma

SQL sorgusu veritabanında yer alıyorsa, LINQ işleçlerini kullanarak ilk ham SQL sorgusunun üzerine oluşturabilirsiniz. Üzerinde birleştirilebilen SQL sorguları `SELECT` anahtar sözcüğüyle başlar.

Aşağıdaki örnek, tablo değerli bir Işlevden (TVF) seçen ham bir SQL sorgusu kullanır ve ardından filtreleme ve sıralamayı gerçekleştirmek için LINQ kullanarak buna dayanır.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

Bu, aşağıdaki SQL sorgusunu üretir:

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a>Değişiklik İzleme

`FromSql` Yöntemleri kullanan sorgular, EF Core ' deki diğer herhangi bir LINQ sorgusuyla tam olarak aynı değişiklik izleme kurallarını izler. Örneğin, sorgu projeleri varlık türlerdir, sonuçlar varsayılan olarak izlenir.

Aşağıdaki örnek, bir tablo değerli Işlevden (TVF) seçen ham bir SQL sorgusu kullanır ve sonra yapılan `AsNoTracking`çağrısıyla değişiklik izlemeyi devre dışı bırakır:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>İlgili verileri dahil etme

`Include` Yöntemi, diğer herhangi bir LINQ sorgusuyla olduğu gibi ilgili verileri dahil etmek için kullanılabilir:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

Bunun için ham SQL sorgusunun birleştirilebilir olması gerektiğini unutmayın; Bu, özellikle saklı yordam çağrılarında çalışmaz. [Sınırlamalar](#limitations)bölümünde yer alan notlara bakın.

## <a name="limitations"></a>Sınırlamalar

Ham SQL sorguları kullanırken dikkat etmeniz için bazı sınırlamalar vardır:

* SQL sorgusu, varlık türünün tüm özellikleri için veri döndürmelidir.

* Sonuç kümesindeki sütun adları, özelliklerin eşlendiği sütun adlarıyla eşleşmelidir. Bu, ham SQL sorguları için özellik/sütun eşlemesinin yoksayıldığı ve sonuç kümesi sütun adlarının Özellik adlarıyla eşleşmesi gerekiyordu EF6 öğesinden farklıdır.

* SQL sorgusu ilgili verileri içeremez. Bununla birlikte, çoğu durumda ilgili verileri ( [ilgili verileri dahil olmak](#including-related-data)üzere) döndürmek `Include` için işlecini kullanarak sorgunun üzerine oluşturabilirsiniz.

* `SELECT`Bu yönteme geçirilen deyimler genellikle birleştirilebilir olmalıdır: EF Core sunucuda ek sorgu işleçlerini değerlendirmesi gerekiyorsa (örneğin, yöntemlerden sonra `FromSql` uygulanan LINQ işleçlerini çevirmek için), sağlanan SQL bir alt sorgu olarak kabul edilir. Bu, geçirilen SQL 'in bir alt sorgu üzerinde geçerli olmayan herhangi bir karakter veya seçenek içermemesi gerektiği anlamına gelir; örneğin:
  * sondaki noktalı virgül
  * SQL Server, sondaki sorgu düzeyi İpucu (örneğin, `OPTION (HASH JOIN)`)
  * SQL Server, `ORDER BY` `SELECT` yan tümcelerinde `OFFSET 0` or `TOP 100 PERCENT` içinde olmayan bir yan tümce

* SQL Server, saklı yordam çağrılarının üzerinde oluşturmaya izin vermediğini unutmayın. bu nedenle, bu tür bir çağrıya ek sorgu işleçleri uygulama girişimleri geçersiz SQL sonucu verir. Sorgu işleçleri, istemci değerlendirmesi sonrasında `AsEnumerable()` tanıtılmıştır.

## <a name="previous-versions"></a>Önceki sürümler

EF Core sürüm 2,2 ve önceki sürümlerde, daha yeni `FromSql` `FromSqlRaw` ve ile `FromSqlInterpolated`aynı şekilde davranmış iki aşırı yükleme vardı. Bu, amaç enterpolasyonlu dize yöntemini çağırdığınızda ham dize yöntemini yanlışlıkla çağırmak çok kolay hale getirilir. Bu, sorguların olması gerektiğinde parametreli hale getirmemelidir.
