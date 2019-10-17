---
title: Ham SQL sorguları-EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: b7087771f1a9e8ee5e044cfea367d74a0b1c1d35
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445928"
---
# <a name="raw-sql-queries"></a>Ham SQL Sorguları

Entity Framework Core, ilişkisel bir veritabanıyla çalışırken ham SQL sorgularına karşı aşağı açılan bir liste sağlar. Ham SQL sorguları, istediğiniz sorgu LINQ kullanılarak ifade edilebilmesi yararlıdır. Ham SQL sorguları, bir LINQ sorgusunun kullanılması verimsiz bir SQL sorgusuna neden olursa da kullanılır. Ham SQL sorguları, modelinizin bir parçası olan normal varlık türlerini veya [anahtarsız varlık türlerini](xref:core/modeling/keyless-entity-types) döndürebilir.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/) GitHub ' da görebilirsiniz.

## <a name="basic-raw-sql-queries"></a>Temel ham SQL sorguları

Ham SQL sorgusuna dayalı bir LINQ sorgusuna başlamak için `FromSqlRaw` genişletme yöntemini kullanabilirsiniz. `FromSqlRaw` yalnızca sorgu köklerinin üzerinde kullanılabilir, doğrudan `DbSet<>` ' dir.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

Ham SQL sorguları, saklı bir yordamı yürütmek için kullanılabilir.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Parametreleri geçirme

> [!WARNING]
> **Ham SQL sorguları için her zaman Parametreleştirme kullanın**
>
> Bir ham SQL sorgusuna Kullanıcı tarafından sağlanmış herhangi bir değer alındığınızda, SQL ekleme saldırılarından kaçınmak için dikkatli olunması gerekir. Bu tür değerlerin geçersiz karakterler içermediğini doğrulamaya ek olarak, her zaman değerleri SQL metinden ayrı gönderen Parametreleştirme kullanın.
>
> Özellikle, hiç bir birleştirilmiş veya enterpolasyonlu dize (`$""`) `FromSqlRaw` veya `ExecuteSqlRaw` ' ye sahip olmayan kullanıcı tarafından belirtilen değerlerle geçirmez. @No__t-0 ve `ExecuteSqlInterpolated` yöntemleri, SQL ekleme saldırılarına karşı koruma sağlayacak şekilde dize ilişkilendirme sözdiziminin kullanılmasına izin verir.

Aşağıdaki örnek, SQL sorgu dizesinde bir parametre yer tutucusu ekleyerek ve ek bir bağımsız değişken sağlayarak, saklı yordama tek bir parametre geçirir. Bu söz dizimi `String.Format` sözdizimi gibi görünebilir, ancak sağlanan değer bir `DbParameter` ve `{0}` yer tutucusunun belirtildiği oluşturulan parametre adı ile sarmalanır.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated` `FromSqlRaw` ' e benzer ancak dize ilişkilendirme söz dizimini kullanmanıza olanak sağlar. @No__t-0 gibi `FromSqlInterpolated` yalnızca sorgu köklerinin üzerinde kullanılabilir. Önceki örnekte olduğu gibi, değer `DbParameter` ' a dönüştürülür ve SQL ekleme ile ilgili değildir.

> [!NOTE]
> Sürüm 3,0 ' den önce, `FromSqlRaw` ve `FromSqlInterpolated` `FromSql` adlı iki aşırı yüklemedir. Daha fazla bilgi için [önceki sürümler bölümüne](#previous-versions)bakın.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

Ayrıca bir DbParameter oluşturup bunu bir parametre değeri olarak sağlayabilirsiniz. Bir dize yer tutucusu yerine düzenli bir SQL parametre yer tutucusu kullanıldığından, `FromSqlRaw` güvenli bir şekilde kullanılabilir.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw`, SQL sorgu dizesinde adlandırılmış parametreleri kullanmanıza olanak sağlar. Bu, saklı yordam isteğe bağlı parametrelere sahip olduğunda yararlı olur:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>LINQ ile oluşturma

LINQ işleçleri kullanarak ilk ham SQL sorgusunun üzerine oluşturabilirsiniz. EF Core, bunu alt sorgu olarak değerlendirir ve veritabanında oluşturun. Aşağıdaki örnek, bir tablo değerli Işlevden (TVF) seçim yapan ham bir SQL sorgusu kullanır. Daha sonra, filtre uygulamak ve sıralamak için LINQ kullanarak buna bir bileşim yapın.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

Yukarıdaki sorgu aşağıdaki SQL 'i oluşturur:

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>İlgili verileri dahil etme

@No__t-0 yöntemi, diğer herhangi bir LINQ sorgusuyla benzer şekilde ilgili verileri dahil etmek için kullanılabilir:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

EF Core, sağlanan SQL 'in bir alt sorgu olarak davranmasından bu yana, LINQ ile oluşturmak için ham SQL sorgusunun birleştirilebilir olması gerekir. Üzerinde birleştirilebilen SQL sorguları `SELECT` anahtar sözcüğüyle başlar. Ayrıca, geçirilen SQL, bir alt sorgu üzerinde geçerli olmayan herhangi bir karakter veya seçenek içermemelidir, örneğin:

- Sondaki noktalı virgül
- SQL Server, sondaki sorgu düzeyi İpucu (örneğin, `OPTION (HASH JOIN)`)
- SQL Server, `SELECT` yan tümcesinde `OFFSET 0` veya `TOP 100 PERCENT` ile kullanılmayan bir `ORDER BY` yan tümcesi

SQL Server, saklı yordam çağrılarının üzerinde oluşturmaya izin vermez, bu nedenle, bu tür bir çağrıya ek sorgu işleçleri uygulama girişimleri geçersiz SQL 'e neden olur. EF Core bir saklı yordam üzerinden oluşturmaya çalıştığından emin olmak için `FromSqlRaw` veya `FromSqlInterpolated` yöntemlerinden sonra `AsEnumerable` veya `AsAsyncEnumerable` yöntemi kullanın.

## <a name="change-tracking"></a>Değişiklik İzleme

@No__t-0 veya `FromSqlInterpolated` yöntemlerini kullanan sorgular, EF Core ' deki diğer LINQ sorgularıyla tam olarak aynı değişiklik izleme kurallarını izler. Örneğin, sorgu projeleri varlık türlerdir, sonuçlar varsayılan olarak izlenir.

Aşağıdaki örnek, bir tablo değerli Işlevden (TVF) seçim yapan ham bir SQL sorgusu kullanır ve sonra değişiklik izlemeyi devre dışı bırakarak `AsNoTracking`:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Sınırlamalar

Ham SQL sorguları kullanırken dikkat etmeniz için bazı sınırlamalar vardır:

- SQL sorgusu, varlık türünün tüm özellikleri için veri döndürmelidir.
- Sonuç kümesindeki sütun adları, özelliklerin eşlendiği sütun adlarıyla eşleşmelidir. Bu davranışın EF6 öğesinden farklı olduğunu aklınızda edin. EF6, ham SQL sorguları için sütun eşlemesine yoksayılan özelliği ve sonuç kümesi sütun adlarının Özellik adlarıyla eşleşmesi gerekiyordu.
- SQL sorgusu ilgili verileri içeremez. Ancak çoğu durumda, ilgili verileri (bkz. [ilgili verileri dahil](#including-related-data)) döndürmek için `Include` işlecini kullanarak sorgunun üzerine yazabilirsiniz.

## <a name="previous-versions"></a>Önceki sürümler

EF Core sürüm 2,2 ve önceki sürümleri, daha yeni `FromSqlRaw` ve `FromSqlInterpolated` ile aynı şekilde davranmış `FromSql` adlı yöntemin iki aşırı yüküne sahipti. Amaç, enterpolasyonlu dize yöntemini çağırdığınızda ve diğer bir şekilde, ham dize metodunu yanlışlıkla çağırmak kolaydır. Yanlışlıkla yanlış aşırı yükleme çağırmak, sorguların olması gerektiği zaman parametreli hale getirmemelidir.
