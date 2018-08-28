---
title: Geçişleri geçmiş tablosu - EF6 özelleştirme
author: divega
ms.date: 2016-10-23
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: ad07b1fe97b3dafe789c0315bd555f979fc412e7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996767"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="aa501-102">Geçişleri geçmiş tablosu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="aa501-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="aa501-103">**EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="aa501-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="aa501-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="aa501-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="aa501-105">Bu makalede, temel senaryolarda Code First Migrations nasıl kullanılacağını varsayar.</span><span class="sxs-lookup"><span data-stu-id="aa501-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="aa501-106">Sizin sonra okumak ihtiyacınız olacak [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="aa501-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="aa501-107">Geçişleri geçmiş tablosu nedir?</span><span class="sxs-lookup"><span data-stu-id="aa501-107">What is Migrations History Table?</span></span>

<span data-ttu-id="aa501-108">Geçişleri geçmiş tablosu, Code First Migrations ile veritabanına uygulanan geçişleri ayrıntılarını depolamak için kullanılan bir tablodur.</span><span class="sxs-lookup"><span data-stu-id="aa501-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="aa501-109">Varsayılan olarak veritabanındaki tablo adıdır. \_ \_MigrationHistory ve ilk veritabanı geçiş do uygularken oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="aa501-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration do the database.</span></span> <span data-ttu-id="aa501-110">Uygulama, Microsoft Sql Server veritabanı kullandıysanız Entity Framework 5'te bu tabloda sistem tablosunda oluştu.</span><span class="sxs-lookup"><span data-stu-id="aa501-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="aa501-111">Bu Entity Framework 6 ancak değişti ve geçişleri geçmiş tablosu bir sistem tablosu artık işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="aa501-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="aa501-112">Neden geçişleri geçmiş tablosu özelleştirebilir?</span><span class="sxs-lookup"><span data-stu-id="aa501-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="aa501-113">Geçişleri geçmiş tablosunda yalnızca Code First Migrations tarafından kullanılan beklenir ve el ile değiştirme geçişleri bozabilir.</span><span class="sxs-lookup"><span data-stu-id="aa501-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="aa501-114">Ancak bazen varsayılan yapılandırmayı uygun değil ve tabloda, örneği için özelleştirilmiş olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="aa501-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="aa501-115">Adları ve/veya sütun 3 etkinleştirmek için modelleri değiştirmeniz gereken<sup>rd</sup> taraf geçişleri sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="aa501-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="aa501-116">Tablonun adını değiştirmek istediğiniz</span><span class="sxs-lookup"><span data-stu-id="aa501-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="aa501-117">Varsayılan olmayan şema için kullanmanız gereken \_ \_MigrationHistory tablo</span><span class="sxs-lookup"><span data-stu-id="aa501-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="aa501-118">Belirli bir sürümü bağlam için ek verileri depolamak gerekir ve bu nedenle, başka bir sütuna tabloya eklemeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="aa501-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="aa501-119">Önlem sözcükleri</span><span class="sxs-lookup"><span data-stu-id="aa501-119">Words of precaution</span></span>

<span data-ttu-id="aa501-120">Geçiş geçmişi tablosu değiştirme güçlüdür ancak uzaklaşmasına konusunda dikkatli olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="aa501-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="aa501-121">EF çalışma zamanı şu anda özelleştirilmiş geçişleri geçmiş tablosu çalışma zamanı ile uyumlu olup olmadığını kontrol etmez.</span><span class="sxs-lookup"><span data-stu-id="aa501-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="aa501-122">Uygulamanızı değilse, çalışma zamanında sonu olabilir veya beklenmeyen şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="aa501-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="aa501-123">Bu veritabanı başına birden fazla bağlamı kullanırsanız bu durumda birden fazla bağlamı aynı geçiş geçmiş tablosu geçişleri hakkında bilgi depolamak için kullanabilirsiniz daha da önem taşır.</span><span class="sxs-lookup"><span data-stu-id="aa501-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="aa501-124">Geçişleri geçmiş tablosu özelleştirmek nasıl?</span><span class="sxs-lookup"><span data-stu-id="aa501-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="aa501-125">Başlamadan önce ilk geçiş yalnızca uygulamadan önce geçişler geçmiş tablosu özelleştirebilirsiniz bilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="aa501-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="aa501-126">Artık, kod.</span><span class="sxs-lookup"><span data-stu-id="aa501-126">Now, to the code.</span></span>

<span data-ttu-id="aa501-127">İlk olarak, System.Data.Entity.Migrations.History.HistoryContext sınıfından türetilen bir sınıf oluşturmanız gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="aa501-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="aa501-128">Yapılandırma geçişleri geçmiş tablosu EF modelleri fluent API'si ile yapılandırmak için çok benzer şekilde HistoryContext sınıfı DbContext sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="aa501-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="aa501-129">OnModelCreating yöntemi yok sayın ve tablo yapılandırmak için fluent API'sini kullanmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="aa501-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="aa501-130">Genellikle EF modeli yapılandırırken temel çağrı gerekmez. Geçersiz kılınan OnModelCreating yönteminden DbContext.OnModelCreating() beri OnModelCreating() boş bir gövdeye sahip.</span><span class="sxs-lookup"><span data-stu-id="aa501-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="aa501-131">Bu durum geçişlerini geçmiş tablosu yapılandırırken değildir.</span><span class="sxs-lookup"><span data-stu-id="aa501-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="aa501-132">Bu durumda ilk OnModelCreating() geçersiz kılmada yapılacak şey gerçekten temel çağırmaktır. OnModelCreating().</span><span class="sxs-lookup"><span data-stu-id="aa501-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="aa501-133">Bu geçersiz kılma yöntemi ince ayar varsayılan şekilde geçişleri geçmiş tablosu yapılandıracaksınız.</span><span class="sxs-lookup"><span data-stu-id="aa501-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="aa501-134">Geçişleri geçmişi tabloyu yeniden adlandırma ve "Yönetici" adlı bir özel şemaya koymak istediğiniz varsayalım.</span><span class="sxs-lookup"><span data-stu-id="aa501-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="aa501-135">Ayrıca, DBA geçiş MigrationId sütunu yeniden adlandırmak istediğiniz\_kimliği</span><span class="sxs-lookup"><span data-stu-id="aa501-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span>  <span data-ttu-id="aa501-136">Aşağıdaki HistoryContext türetilmiş sınıf oluşturarak bunu:</span><span class="sxs-lookup"><span data-stu-id="aa501-136">You could achieve this by creating the following class derived from HistoryContext:</span></span>

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

<span data-ttu-id="aa501-137">EF ile kaydederek bu haberdar olmak gerekir, özel HistoryContext hazır olduktan sonra [kod tabanlı yapılandırma](http://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="aa501-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](http://msdn.com/data/jj680699):</span></span>

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

<span data-ttu-id="aa501-138">Tarayıcınızdaki İşte bu kadar.</span><span class="sxs-lookup"><span data-stu-id="aa501-138">That’s pretty much it.</span></span> <span data-ttu-id="aa501-139">Etkinleştir-geçişler, Paket Yöneticisi konsolu gidebilirsiniz artık geçiş Ekle ve son olarak veritabanını güncelleştir.</span><span class="sxs-lookup"><span data-stu-id="aa501-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="aa501-140">Bu, veritabanı geçişleri geçmiş tablosu HistoryContext türetilmiş sınıfınızın belirtilen ayrıntılara göre yapılandırılmış ekleme neden olur.</span><span class="sxs-lookup"><span data-stu-id="aa501-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Veritabanı](~/ef6/media/database.png)
