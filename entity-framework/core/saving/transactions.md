---
title: İşlemler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 390d89398ebfdf015804749e71ff0b61d3f278d3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417557"
---
# <a name="using-transactions"></a>İşlemleri Kullanma

İşlemler, çeşitli veritabanı işlemlerinin atomik bir şekilde işlenmesine izin verir. Hareket işlenirse, tüm işlemler veritabanına başarıyla uygulanır. Hareket geri alınırsa, işlemlerin hiçbiri veritabanına uygulanmaz.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) GitHub'da görüntüleyebilirsiniz.

## <a name="default-transaction-behavior"></a>Varsayılan hareket davranışı

Varsayılan olarak, veritabanı sağlayıcısı hareketleri destekliyorsa, tek `SaveChanges()` bir çağrıdaki tüm değişiklikler bir harekette uygulanır. Değişikliklerden herhangi biri başarısız olursa, hareket geri alınır ve değişikliklerin hiçbiri veritabanına uygulanmaz. Bu, `SaveChanges()` bir hata oluşursa veritabanının tamamen başarılı olması veya veritabanını değiştirilmeden bırakması garanti edilir.

Çoğu uygulama için bu varsayılan davranış yeterlidir. Yalnızca başvuru gereksinimleriniz gerekli yse hareketleri el ile denetlemeniz gerekir.

## <a name="controlling-transactions"></a>Hareketleri denetleme

`DbContext.Database` Hareketleri başlatmak, işlemek ve geri almak için API'yi kullanabilirsiniz. Aşağıdaki örnekte, `SaveChanges()` iki işlem ve tek bir işlemde yürütülmekte olan bir LINQ sorgusu gösterilmektedir.

Tüm veritabanı sağlayıcıları hareketleri desteklemez. Bazı sağlayıcılar, işlem API'leri çağrıldığında atabilir veya işlem olmayabilir.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Bağlamlar arası işlem (yalnızca ilişkisel veritabanları)

Bir hareketi birden çok bağlam örnekleri arasında da paylaşabilirsiniz. Bu işlevsellik yalnızca ilişkisel veritabanlarının `DbTransaction` kullanımını ve `DbConnection`ilişkisel veritabanlarına özgü olmasını gerektirdiğinden ilişkisel bir veritabanı sağlayıcısı kullanırken kullanılabilir.

Bir hareketi paylaşmak için bağlamların hem `DbConnection` a `DbTransaction`hem de bir' yi paylaşması gerekir.

### <a name="allow-connection-to-be-externally-provided"></a>Bağlantının dışarıdan sağlanmasına izin ver

Bir `DbConnection` bağlantıyı oluşturmak için bir bağlama geçiş yeteneği gerektirir.

Dışarıdan sağlanmanın `DbConnection` en kolay yolu, bağlamı `DbContext.OnConfiguring` yapılandırmak ve bunları dışsal olarak `DbContextOptions` oluşturmak ve bağlam oluşturucuya aktarmak için yöntemi kullanmayı durdurmaktır.

> [!TIP]  
> `DbContextOptionsBuilder`bağlamı `DbContext.OnConfiguring` yapılandırmak için kullandığınız API'dir, şimdi oluşturmak `DbContextOptions`için harici olarak kullanacaksınız.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Alternatif olarak kullanmaya `DbContext.OnConfiguring`devam etmek, `DbConnection` ancak kaydedilmiş ve `DbContext.OnConfiguring`daha sonra kullanılan bir kabul etmektir.

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a>Bağlantı ve hareketi paylaşma

Artık aynı bağlantıyı paylaşan birden çok bağlam örneği oluşturabilirsiniz. `DbContext.Database.UseTransaction(DbTransaction)` Ardından, her iki bağlamı da aynı işlemde kullanmak için API'yi kullanın.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Harici DbTransactions kullanma (yalnızca ilişkisel veritabanları)

İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojisi kullanıyorsanız, bu farklı teknolojiler tarafından gerçekleştirilen işlemler arasında bir işlem paylaşmak isteyebilirsiniz.

Aşağıdaki örnek, aynı işlemde bir sqlclient işlemi ve bir Entity Framework Core işlemi ADO.NET nasıl gerçekleştirilini gösterir.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>System.Transactions'ı Kullanma

> [!NOTE]  
> Bu özellik EF Core 2.1'de yenidir.

Daha geniş bir kapsamda koordine olmanız gerekiyorsa ortam hareketlerini kullanmak mümkündür.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Açık bir işlem için kaydolmak da mümkündür.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>System.Transactions Sınırlamaları  

1. EF Core, System.Transactions desteğini uygulamak için veritabanı sağlayıcılarına güvenir. Destek .NET Framework için ADO.NET sağlayıcılar arasında oldukça yaygın olmasına rağmen, API sadece son zamanlarda .NET Core eklenmiştir ve bu nedenle destek kadar yaygın değildir. Bir sağlayıcı System.Transactions için destek uygulamazsa, bu API'lere yapılan çağrıların tamamen yoksayılması mümkündür. .NET Core için SqlClient 2.1'den itibaren destekler. .NET Core 2.0 için SqlClient özelliğini kullanmaya çalışırsanız bir özel durum oluşturur.

   > [!IMPORTANT]  
   > Hareketleri yönetmek için güvenmeden önce API'nin sağlayıcınıza doğru şekilde hareket ettiğini test etmek önerilir. Yoksa veritabanı sağlayıcısının bakıcısına başvurmanız tavsiye edilir.

2. Sürüm 2.1 itibariyle, .NET Core'daki System.Transactions uygulaması dağıtılmış hareketler için `TransactionScope` `CommittableTransaction` destek içermez, bu nedenle birden çok kaynak yöneticisi arasında hareketleri kullanamaz veya koordine edemezsiniz.
