---
title: İşlemler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: ff12c4e7ace1f1b9e503cb2353bcdd53efd87cce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197901"
---
# <a name="using-transactions"></a>İşlemleri Kullanma

İşlemler birkaç veritabanı işlemin atomik bir şekilde işlenmesine izin verir. İşlem yürütüolduysa, tüm işlemler veritabanına başarıyla uygulanır. İşlem geri alınırsa, hiçbir işlem veritabanına uygulanmaz.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Transactions/) GitHub üzerinde.

## <a name="default-transaction-behavior"></a>Varsayılan işlem davranışı

Varsayılan olarak, veritabanı sağlayıcısı işlemleri destekliyorsa, tek bir çağrıdaki `SaveChanges()` tüm değişiklikler bir işlem içinde uygulanır. Değişikliklerden herhangi biri başarısız olursa, işlem geri alınır ve veritabanına hiçbir değişiklik uygulanmaz. Bu, tamamen `SaveChanges()` başarılı veya bir hata oluşursa veritabanını değiştirilmemiş olarak bırakma garantisi veren anlamına gelir.

Çoğu uygulama için bu varsayılan davranış yeterlidir. İşlemleri yalnızca, uygulama gereksinimleriniz gerekli değilse el ile kontrol etmelisiniz.

## <a name="controlling-transactions"></a>İşlemleri denetleme

İşlemleri başlatmak, yürütmek `DbContext.Database` ve geri almak için API 'yi kullanabilirsiniz. Aşağıdaki örnekte, tek bir `SaveChanges()` işlemde yürütülen iki işlem ve bir LINQ sorgusu gösterilmektedir.

Tüm veritabanı sağlayıcıları işlemleri desteklemez. İşlem API 'Leri çağrıldığında bazı sağlayıcılar bu işlemleri atabilir veya gerektirmez.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Çapraz bağlam işlemi (yalnızca ilişkisel veritabanları)

Ayrıca, bir işlemi birden çok bağlam örneği arasında paylaşabilirsiniz. Bu işlevsellik yalnızca, ilişkisel veritabanlarına özgü olan `DbTransaction` ve `DbConnection`kullanımını gerektirdiğinden ilişkisel bir veritabanı sağlayıcısı kullanılırken kullanılabilir.

Bir işlemi paylaşmak için bağlamların hem a `DbConnection` hem de bir `DbTransaction`paylaşılması gerekir.

### <a name="allow-connection-to-be-externally-provided"></a>Bağlantının dışarıdan sağlanması için izin ver

' Nin `DbConnection` paylaşılması, bir bağlantıyı oluştururken bir bağlama geçirebilmesini gerektirir.

Dışarıdan sağlanmanın en kolay `DbConnection` yolu, bağlamını yapılandırmak ve içeriği dışarıdan oluşturmak `DbContextOptions` ve bağlam `DbContext.OnConfiguring` oluşturucusuna geçirmek için yöntemini kullanmayı durdurmaktır.

> [!TIP]  
> `DbContextOptionsBuilder`, bağlamını yapılandırmak için ' de `DbContext.OnConfiguring` kullandığınız API 'dir, şimdi oluşturmak `DbContextOptions`için bunu dışarıdan kullanacaksınız.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Diğer bir seçenek de kullanmaya `DbContext.OnConfiguring`devam etmesinin yanı sıra içinde `DbContext.OnConfiguring`kullanılan ve daha sonra kullanılan bir `DbConnection` .

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

### <a name="share-connection-and-transaction"></a>Bağlantıyı ve işlemi paylaşma

Artık aynı bağlantıyı paylaşan birden çok bağlam örneği oluşturabilirsiniz. Daha sonra aynı `DbContext.Database.UseTransaction(DbTransaction)` işlemde her iki bağlamı da listeleme için API 'yi kullanın.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Dış DbTransactions kullanma (yalnızca ilişkisel veritabanları)

İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojisi kullanıyorsanız, bu farklı teknolojiler tarafından gerçekleştirilen işlemler arasında bir işlem paylaşmak isteyebilirsiniz.

Aşağıdaki örnek, aynı işlemde bir ADO.NET SqlClient işleminin ve Entity Framework Core işleminin nasıl gerçekleştirileceğini gösterir.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>System. Transactions kullanma

> [!NOTE]  
> Bu özellik EF Core 2,1 ' de yenidir.

Daha büyük bir kapsamda koordine etmeniz gerekiyorsa çevresel işlemler kullanmak mümkündür.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Ayrıca, açık bir işlemde listeleme de mümkündür.

[!code-csharp[Main](../../../samples/core/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>System. Transactions sınırlamaları  

1. EF Core, System. Transactions desteğini uygulamak için veritabanı sağlayıcılarını kullanır. Destek, .NET Framework için ADO.NET sağlayıcıları arasında oldukça yaygın olsa da, API yalnızca .NET Core 'a eklenmiştir ve bu nedenle destek yaygın olarak değildir. Bir sağlayıcı System. Transactions için destek uygulamaz, bu API 'lere yapılan çağrılar tamamen yok sayılır. .NET Core için SqlClient, bunu 2,1 ve sonraki sürümlerde destekler. .NET Core 2,0 için SqlClient, özelliği kullanmayı denerseniz bir özel durum oluşturur. 

   > [!IMPORTANT]  
   > İşlemleri yönetmek için kullanmadan önce API 'nin sağlayıcınızı doğru şekilde davrandığını test etmeniz önerilir. Veri tabanı sağlayıcının bakımınonu ile iletişim kurmanız önerilir. 

2. Sürüm 2,1 itibariyle, .NET Core 'daki System. Transactions uygulamasında dağıtılmış işlemler için destek yoktur, bu nedenle, birden çok kaynak yöneticisi arasında `TransactionScope` işlem `CommittableTransaction` koordine edilemez. 
