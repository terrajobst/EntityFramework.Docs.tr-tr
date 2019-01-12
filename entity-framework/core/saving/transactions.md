---
title: İşlemler - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 4c50d6694c6678678c0af8defe2601abee923af1
ms.sourcegitcommit: 5f11a5fa5d2cde81a4e4d0d5c3a60aa74b83cbd4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54226198"
---
# <a name="using-transactions"></a>İşlemleri kullanma

İşlemler atomik bir şekilde işlenecek çeşitli veritabanı işlemlerini izin verin. İşlem, varsa, tüm işlemleri veritabanına başarıyla uygulanır. İşlem geri alınır, hiçbiri işlemleri veritabanına uygulanır.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) GitHub üzerinde.

## <a name="default-transaction-behavior"></a>Varsayılan işlem davranışı

Varsayılan olarak, veritabanı sağlayıcısı işlemleri, destekliyorsa yapılan tüm değişiklikler tek bir çağrıda `SaveChanges()` bir işlemde uygulanır. Değişikliklerden herhangi birini başarısız olursa, işlem geri alınır ve değişikliklerin hiçbiri veritabanına uygulanır. Diğer bir deyişle `SaveChanges()` tamamen başarılı ya da bir hata oluşursa, üzerinde değişiklik yapılmadan veritabanını bırak garanti edilir.

Bu varsayılan davranışı, çoğu uygulama için yeterlidir. Uygulama gereksinimlerinize gerekli bulduğunuz, yalnızca el ile işlem denetlemesi gerekir.

## <a name="controlling-transactions"></a>İşlemleri denetleme

Kullanabileceğiniz `DbContext.Database` işleme başlamak için API ve geri alma işlemleri. Aşağıdaki örnek iki gösterir `SaveChanges()` işlemler ve bir LINQ Sorgu tek bir işlemde yürütülmekte.

Veritabanı sağlayıcıları tüm işlemleri destekler. Bazı sağlayıcıları atabilir veya İşlemsiz işlem API'ler çağrıldığında kullanılacak.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a>Bağlam içi işlem (yalnızca ilişkisel veritabanları)

Ayrıca, bir işlem birden çok içerik örneğine paylaşabilirsiniz. Bu işlev yalnızca kullanımı gerektirdiğinden, ilişkisel veritabanı sağlayıcısı kullanırken kullanılabilir `DbTransaction` ve `DbConnection`, ilişkisel veritabanlarına belirli olduğu.

Bir işlem paylaşmak için bağlamı hem de paylaşmanız gerekir bir `DbConnection` ve `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Harici olarak sağlanan bağlantıya izin ver

Paylaşımı bir `DbConnection` oluştururken bu bağlantı bir bağlamına geçirme özelliği gerektirir.

İzin vermek için en kolay yolu `DbConnection` durdurmak için dışarıdan sağlanması için olan `DbContext.OnConfiguring` bağlam yapılandırmak ve harici olarak oluşturmak için gereken yöntemini `DbContextOptions` ve bunları bağlam oluşturucuya geçirin.

> [!TIP]  
> `DbContextOptionsBuilder` içinde kullanılan API `DbContext.OnConfiguring` bağlam yapılandırmak için artık bunu harici olarak oluşturmak için kullanacaksanız `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Kullanmaya devam etmek için bir alternatifidir `DbContext.OnConfiguring`, ancak kabul bir `DbConnection` kaydedildi ve kullanılan `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Paylaşım bağlantı ve işlem

Artık aynı bağlantıyı paylaşan birden çok içerik örneği oluşturabilirsiniz. Ardından `DbContext.Database.UseTransaction(DbTransaction)` kaydolunamadı aynı işlem içinde her iki bağlamları API.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Dış DbTransactions (yalnızca ilişkisel veritabanları) kullanma

İlişkisel bir veritabanına erişmek için birden çok veri erişim teknolojileri kullanıyorsanız, bir işlem bu farklı teknolojiler tarafından gerçekleştirilen işlemler arasında paylaşmak isteyebilirsiniz.

Aşağıdaki örnek, aynı işlemde bir ADO.NET SqlClient işlem ve Entity Framework Core işlemi gerçekleştirmek nasıl gösterir.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>System.Transactions kullanma

> [!NOTE]  
> Bu özellik, EF Core 2.1 içinde yeni bir özelliktir.

Daha büyük bir kapsama arasında koordine etmeniz gerekiyorsa, ortam işlemleri kullanmak mümkündür.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

Açık bir işleme mümkündür.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a>System.Transactions sınırlamaları  

1. EF Core System.Transactions için destek uygulamak için veritabanı sağlayıcıları kullanır. Destek .NET Framework için ADO.NET sağlayıcıları arasında oldukça sık kullanılan olsa da, API .NET Core için yalnızca bir son eklendi ve bu nedenle desteği gibi yaygın değildir. Bir sağlayıcı System.Transactions desteği uygulamaz, bu API'lere giden çağrıların tamamen yoksayılacak mümkündür. .NET Core için SqlClient 2.1 ve üzeri desteklemiyor. Bu özelliği kullanmayı denerseniz, .NET Core 2.0 için SqlClient bir özel durum oluşturur. 

   > [!IMPORTANT]  
   > Üzerinde işlemleri yönetmek için kullanan önce API sağlayıcınız ile düzgün şekilde davranan test önerilir. Veritabanı sağlayıcısı Bakımcı kullanmıyorsa başvurmaları. 

2. 2.1 sürümü itibarıyla, .NET Core System.Transactions uygulamasında dağıtılmış işlemler için destek içermez; bu nedenle kullanamazsınız `TransactionScope` veya `CommittableTransaction` işlemler arasında birden çok kaynak yöneticileri koordine etmek için. 
