---
title: Ham SQL Sorguları - EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417672"
---
# <a name="raw-sql-queries"></a>Ham SQL Sorguları

Entity Framework Core, ilişkisel bir veritabanıyla çalışırken ham SQL sorgularına düşmenizi sağlar. İstediğiniz sorgu LINQ kullanılarak ifade edilenemiyorsa Ham SQL sorguları yararlıdır. Bir LINQ sorgusu kullanarak verimsiz bir SQL sorgusu neden olduğunda Ham SQL sorguları da kullanılır. Raw SQL sorguları, normal varlık türlerini veya modelinizin bir parçası olan [anahtarsız varlık türlerini](xref:core/modeling/keyless-entity-types) döndürebilir.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) GitHub'da görüntüleyebilirsiniz.

## <a name="basic-raw-sql-queries"></a>Temel ham SQL sorguları

Ham bir `FromSqlRaw` SQL sorgusunu temel alan bir LINQ sorgusu başlatmak için uzantı yöntemini kullanabilirsiniz. `FromSqlRaw`yalnızca doğrudan sorgu köklerinde `DbSet<>`kullanılabilir.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

Raw SQL sorguları depolanan yordamı yürütmek için kullanılabilir.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Parametreleri geçirme

> [!WARNING]
> **Her zaman ham SQL sorguları için parametreleştirme kullanın**
>
> Kullanıcı tarafından sağlanan değerleri ham bir SQL sorgusuna sokarken, SQL enjeksiyon saldırılarından kaçınmak için dikkatli olunmalıdır. Bu tür değerlerin geçersiz karakterler içermediğini doğrulamanın yanı sıra, her zaman SQL metninden ayrı değerleri gönderen parametrelendirmekullanın.
>
> Özellikle, kullanıcı tarafından sağlanan doğrulanmış olmayan değerlere`$""`sahip, hiçbir zaman bir sıkıştırılmış `FromSqlRaw` `ExecuteSqlRaw`veya enterpolasyonlu dizeyi () hiçbir zaman geçemez. Ve `FromSqlInterpolated` `ExecuteSqlInterpolated` yöntemler, string enterpolasyon sözdizimini SQL enjeksiyon saldırılarına karşı koruyacak şekilde kullanmaolanağı sağlar.

Aşağıdaki örnek, SQL sorgu dizesi bir parametre yer tutucusu ekleyerek ve ek bir bağımsız değişken sağlayarak depolanan yordamı tek bir parametre geçer. Bu sözdizimi sözdizimi gibi `String.Format` görünse de, sağlanan `DbParameter` değer a'ya sarılır ve `{0}` yer tutucunun belirtildiği yere oluşturulan parametre adı eklenir.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated`benzer `FromSqlRaw` dir, ancak dize enterpolasyon sözdizimini kullanmanıza olanak sağlar. Gibi `FromSqlRaw`, `FromSqlInterpolated` sadece sorgu kökleri kullanılabilir. Önceki örnekte olduğu gibi, değer a `DbParameter` dönüştürülür ve SQL enjeksiyonuna karşı savunmasız değildir.

> [!NOTE]
> Önce sürüm 3.0 `FromSqlRaw` `FromSqlInterpolated` ve iki overloads adlı `FromSql`edildi . Daha fazla bilgi için [önceki sürümler bölümüne](#previous-versions)bakın.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

Ayrıca bir DbParameter oluşturup parametre değeri olarak da sağlayabilirsiniz. Dize yer tutucusu yerine normal bir SQL parametre `FromSqlRaw` yer tutucukullanıldığından, güvenli bir şekilde kullanılabilir:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw`SQL sorgu dizesinde adlandırılmış parametreleri kullanmanıza olanak tanır, bu da depolanan bir yordamın isteğe bağlı parametreleri olduğunda yararlıdır:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>LINQ ile beste

LINQ işleçlerini kullanarak ilk ham SQL sorgusunun üstüne oluşturabilirsiniz. EF Core bunu alt sorgu olarak ele alacak ve veritabanında üzerinde oluşturacaktır. Aşağıdaki örnekte, Tablo Değeri Olan İşlev'den (TVF) seçim yapan ham bir SQL sorgusu kullanır. Ve sonra filtreleme ve sıralama yapmak için LINQ kullanarak üzerine oluşturur.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

Yukarıdaki sorgu aşağıdaki SQL oluşturur:

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>İlgili verileri dahil etme

Yöntem, `Include` diğer LINQ sorgusunda olduğu gibi ilgili verileri de eklemek için kullanılabilir:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

EF Core, sağlanan SQL'i bir alt sorgu olarak ele alacağı için, LINQ ile beste yapmak için ham SQL sorgunuzun birleştirilebilir olmasını gerektirir. Üzerinde oluşturulabilen SQL sorguları `SELECT` anahtar sözcükile başlar. Ayrıca, SQL geçirilen gibi bir alt sorgu üzerinde geçerli olmayan herhangi bir karakter veya seçenek içermemelidir:

- İzleyen bir yarı kolon
- SQL Server'da, sorgu düzeyi ipucu (örneğin, `OPTION (HASH JOIN)`)
- `ORDER BY` SQL Server'da, `OFFSET 0` `TOP 100 PERCENT` `SELECT` yan tümcede OR ile kullanılmayan bir yan tümce

SQL Server, depolanan yordam çağrıları üzerinde bir araya gelmekiçin izin vermez, bu nedenle böyle bir çağrıya ek sorgu işleçleri uygulama girişimi geçersiz SQL'e neden olur. EF `AsEnumerable` `AsAsyncEnumerable` Core'un `FromSqlRaw` `FromSqlInterpolated` depolanan bir yordam üzerinde oluşturmaya çalışmadığından emin olmak için hemen sonra veya yöntem kullanın.

## <a name="change-tracking"></a>Değişiklik İzleme

`FromSqlRaw` Veya `FromSqlInterpolated` yöntemleri kullanan sorgular, EF Core'daki diğer LINQ sorguları yla aynı değişiklik izleme kurallarını izler. Örneğin, sorgu varlık türlerini projeleri ise, sonuçlar varsayılan olarak izlenir.

Aşağıdaki örnek, Tablo Değeri Olan İşlev'den (TVF) seçim yapan ham bir SQL sorgusu `AsNoTracking`kullanır, ardından aşağıdaki çağrıyla izlemeyi değiştirir:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Sınırlamalar

Ham SQL sorgularını kullanırken dikkat edilmesi gereken birkaç sınırlama vardır:

- SQL sorgusu, varlık türünün tüm özellikleri için veri döndürmelidir.
- Sonuç kümesindeki sütun adları, özelliklerin eşlenen sütun adlarıyla eşleşmelidir. Bu davranışın EF6'dan farklı olduğunu unutmayın. EF6, ham SQL sorguları için sütun eşleme özelliğini göz ardı etti ve sonuç kümesi sütun adları özellik adlarıyla eşleşmek zorunda kaldı.
- SQL sorgusu ilgili verileri içeremez. Ancak, çoğu durumda ilgili verileri döndürmek için `Include` işleci kullanarak sorgunun üstüne oluşturabilirsiniz (bkz. ilgili verileri dahil [etme).](#including-related-data)

## <a name="previous-versions"></a>Önceki sürümler

EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`. Amaç enterpolasyonlu dize yöntemini aramakken yanlışlıkla ham dize yöntemini aramak kolaydı. Yanlışlıkla yanlış aşırı yükleme çağrısı, sorguların olması gerekirken parametrelendirilememelerine neden olabilir.
