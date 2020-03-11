---
title: Geçişler geçmiş tablosunu özelleştirme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: eb19f367611a86f685557a6741a5f2f0bad6b718
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418981"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="442e1-102">Geçişler geçmiş tablosunu özelleştirme</span><span class="sxs-lookup"><span data-stu-id="442e1-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="442e1-103">**Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="442e1-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="442e1-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="442e1-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="442e1-105">Bu makalede temel senaryolarda Code First Migrations kullanmayı bildiğiniz varsayılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="442e1-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="442e1-106">Bunu yapmazsanız devam etmeden önce [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) okumanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="442e1-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="442e1-107">Geçişler geçmiş tablosu nedir?</span><span class="sxs-lookup"><span data-stu-id="442e1-107">What is Migrations History Table?</span></span>

<span data-ttu-id="442e1-108">Geçişler geçmiş tablosu, veritabanına uygulanan geçişler hakkındaki ayrıntıları depolamak için Code First Migrations tarafından kullanılan bir tablodur.</span><span class="sxs-lookup"><span data-stu-id="442e1-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="442e1-109">Varsayılan olarak, veritabanındaki tablonun adı \_\_MigrationHistory ' dir ve veritabanına ilk geçiş uygulanırken oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="442e1-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration to the database.</span></span> <span data-ttu-id="442e1-110">Entity Framework 5 ' te, uygulama Microsoft SQL Server veritabanını kullanıyorsa bu tablo bir sistem tablosu idi.</span><span class="sxs-lookup"><span data-stu-id="442e1-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="442e1-111">Bu, Entity Framework 6 ' da değişmiştir ve geçişler geçmiş tablosu artık bir sistem tablosu olarak işaretlenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="442e1-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="442e1-112">Geçişleri geçmiş tablosunu neden özelleştirsin?</span><span class="sxs-lookup"><span data-stu-id="442e1-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="442e1-113">Geçişler geçmiş tablosunun yalnızca Code First Migrations tarafından kullanılması ve el ile değiştirilmesi geçişleri kesebilir.</span><span class="sxs-lookup"><span data-stu-id="442e1-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="442e1-114">Ancak bazen varsayılan yapılandırma uygun değildir ve tablonun özelleştirilmesi gerekir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="442e1-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="442e1-115">3 bir<sup>RD</sup> parti geçişi sağlayıcısını etkinleştirmek için sütunların adlarını ve/veya modellerini değiştirmeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="442e1-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="442e1-116">Tablonun adını değiştirmek istiyorsunuz</span><span class="sxs-lookup"><span data-stu-id="442e1-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="442e1-117">\_\_MigrationHistory tablosu için varsayılan olmayan bir şema kullanmanız gerekir</span><span class="sxs-lookup"><span data-stu-id="442e1-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="442e1-118">İçeriğin belirli bir sürümü için ek verileri depolamanız ve bu nedenle tabloya ek bir sütun eklemeniz gerekir</span><span class="sxs-lookup"><span data-stu-id="442e1-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="442e1-119">Önlem kelimeleri</span><span class="sxs-lookup"><span data-stu-id="442e1-119">Words of precaution</span></span>

<span data-ttu-id="442e1-120">Geçiş geçmişi tablosunun değiştirilmesi güçlüdür, ancak bunu yapmak için dikkatli olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="442e1-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="442e1-121">EF Runtime Şu anda özelleştirilmiş geçişler geçmiş tablosunun çalışma zamanıyla uyumlu olup olmadığını denetlemiyor.</span><span class="sxs-lookup"><span data-stu-id="442e1-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="442e1-122">Uygulamanız değilse, çalışma zamanında kesintiye uğramayabilir veya öngörülemeyen yollarla davranabilir.</span><span class="sxs-lookup"><span data-stu-id="442e1-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="442e1-123">Bu, veritabanı başına birden çok bağlam kullandığınızda daha da çok önemlidir. Bu durumda birden çok bağlam, geçişler hakkındaki bilgileri depolamak için aynı geçiş geçmişi tablosunu kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="442e1-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="442e1-124">Geçişler geçmiş tablosu nasıl özelleştirilir?</span><span class="sxs-lookup"><span data-stu-id="442e1-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="442e1-125">Başlamadan önce, geçişleri geçmiş tablosunu yalnızca ilk geçişi uygulamadan önce özelleştirebileceğinizi bilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="442e1-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="442e1-126">Şimdi koda.</span><span class="sxs-lookup"><span data-stu-id="442e1-126">Now, to the code.</span></span>

<span data-ttu-id="442e1-127">İlk olarak, System. Data. Entity. geçişler. history. Geçmişcontext sınıfından türetilmiş bir sınıf oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="442e1-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="442e1-128">Geçmişten bağlam sınıfı DbContext sınıfından türetilir, böylece geçişler geçmiş tablosunu yapılandırmak Fluent API ile EF modellerini yapılandırmaya çok benzer.</span><span class="sxs-lookup"><span data-stu-id="442e1-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="442e1-129">Yalnızca Onmodeloluşturma yöntemini geçersiz kılmanız ve tabloyu yapılandırmak için Fluent API kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="442e1-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="442e1-130">Genellikle EF modellerini yapılandırdığınızda, taban ' i çağırmanız gerekmez. DbContext. Onmodeloluşturuluyor () boş gövde içerdiğinden, geçersiz kılınan Onmodeloluþturma yönteminden Onmodeloluşturuluyor ().</span><span class="sxs-lookup"><span data-stu-id="442e1-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="442e1-131">Geçişler geçmiş tablosu yapılandırılırken bu durum böyle değildir.</span><span class="sxs-lookup"><span data-stu-id="442e1-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="442e1-132">Bu durumda Onmodeloluþturma () geçersiz kılmanızda yapılacak ilk şey aslında temel çağırmalıdır. Onmodeloluşturuluyor ().</span><span class="sxs-lookup"><span data-stu-id="442e1-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="442e1-133">Bu, geçişleri geçmiş tablosunu varsayılan şekilde yapılandırır, daha sonra geçersiz kılma yönteminde ince ayar.</span><span class="sxs-lookup"><span data-stu-id="442e1-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="442e1-134">Geçişleri geçmiş tablosunu yeniden adlandırmak ve "admin" adlı özel bir şemaya koymak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="442e1-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="442e1-135">Ayrıca, DBA 'nın MigrationID sütununu geçiş\_KIMLIĞI olarak yeniden adlandırmanızı istersiniz.</span><span class="sxs-lookup"><span data-stu-id="442e1-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span> <span data-ttu-id="442e1-136"> Bu, aşağıdaki bir türeme bağlamından türetilen sınıfı oluşturarak bunu elde edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="442e1-136"> You could achieve this by creating the following class derived from HistoryContext:</span></span>

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

<span data-ttu-id="442e1-137">Özel bir [ıncode tabanlı yapılandırma](https://msdn.com/data/jj680699)yoluyla kaydederek özel bir ın,</span><span class="sxs-lookup"><span data-stu-id="442e1-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

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

<span data-ttu-id="442e1-138">Bu çok önemlidir.</span><span class="sxs-lookup"><span data-stu-id="442e1-138">That’s pretty much it.</span></span> <span data-ttu-id="442e1-139">Artık Paket Yöneticisi konsoluna, Enable-geçişleri, Add-Migration ve finally Update-Database ' e gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="442e1-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="442e1-140">Bu, geçmişe yönelik bağlam türetilmiş sınıfında belirttiğiniz ayrıntılara göre yapılandırılmış bir geçiş geçmişi tablosu veritabanına eklenmesine neden olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="442e1-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Database](~/ef6/media/database.png)
