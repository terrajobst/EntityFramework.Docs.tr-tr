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
# <a name="customizing-the-migrations-history-table"></a>Geçişler geçmiş tablosunu özelleştirme
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

> [!NOTE]
> Bu makalede temel senaryolarda Code First Migrations kullanmayı bildiğiniz varsayılmaktadır. Bunu yapmazsanız devam etmeden önce [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) okumanız gerekir.

## <a name="what-is-migrations-history-table"></a>Geçişler geçmiş tablosu nedir?

Geçişler geçmiş tablosu, veritabanına uygulanan geçişler hakkındaki ayrıntıları depolamak için Code First Migrations tarafından kullanılan bir tablodur. Varsayılan olarak, veritabanındaki tablonun adı \_\_MigrationHistory ' dir ve veritabanına ilk geçiş uygulanırken oluşturulur. Entity Framework 5 ' te, uygulama Microsoft SQL Server veritabanını kullanıyorsa bu tablo bir sistem tablosu idi. Bu, Entity Framework 6 ' da değişmiştir ve geçişler geçmiş tablosu artık bir sistem tablosu olarak işaretlenmemiştir.

## <a name="why-customize-migrations-history-table"></a>Geçişleri geçmiş tablosunu neden özelleştirsin?

Geçişler geçmiş tablosunun yalnızca Code First Migrations tarafından kullanılması ve el ile değiştirilmesi geçişleri kesebilir. Ancak bazen varsayılan yapılandırma uygun değildir ve tablonun özelleştirilmesi gerekir, örneğin:

-   3 bir<sup>RD</sup> parti geçişi sağlayıcısını etkinleştirmek için sütunların adlarını ve/veya modellerini değiştirmeniz gerekir
-   Tablonun adını değiştirmek istiyorsunuz
-   \_\_MigrationHistory tablosu için varsayılan olmayan bir şema kullanmanız gerekir
-   İçeriğin belirli bir sürümü için ek verileri depolamanız ve bu nedenle tabloya ek bir sütun eklemeniz gerekir

## <a name="words-of-precaution"></a>Önlem kelimeleri

Geçiş geçmişi tablosunun değiştirilmesi güçlüdür, ancak bunu yapmak için dikkatli olmanız gerekir. EF Runtime Şu anda özelleştirilmiş geçişler geçmiş tablosunun çalışma zamanıyla uyumlu olup olmadığını denetlemiyor. Uygulamanız değilse, çalışma zamanında kesintiye uğramayabilir veya öngörülemeyen yollarla davranabilir. Bu, veritabanı başına birden çok bağlam kullandığınızda daha da çok önemlidir. Bu durumda birden çok bağlam, geçişler hakkındaki bilgileri depolamak için aynı geçiş geçmişi tablosunu kullanabilir.

## <a name="how-to-customize-migrations-history-table"></a>Geçişler geçmiş tablosu nasıl özelleştirilir?

Başlamadan önce, geçişleri geçmiş tablosunu yalnızca ilk geçişi uygulamadan önce özelleştirebileceğinizi bilmeniz gerekir. Şimdi koda.

İlk olarak, System. Data. Entity. geçişler. history. Geçmişcontext sınıfından türetilmiş bir sınıf oluşturmanız gerekir. Geçmişten bağlam sınıfı DbContext sınıfından türetilir, böylece geçişler geçmiş tablosunu yapılandırmak Fluent API ile EF modellerini yapılandırmaya çok benzer. Yalnızca Onmodeloluşturma yöntemini geçersiz kılmanız ve tabloyu yapılandırmak için Fluent API kullanmanız gerekir.

>[!NOTE]
> Genellikle EF modellerini yapılandırdığınızda, taban ' i çağırmanız gerekmez. DbContext. Onmodeloluşturuluyor () boş gövde içerdiğinden, geçersiz kılınan Onmodeloluþturma yönteminden Onmodeloluşturuluyor (). Geçişler geçmiş tablosu yapılandırılırken bu durum böyle değildir. Bu durumda Onmodeloluþturma () geçersiz kılmanızda yapılacak ilk şey aslında temel çağırmalıdır. Onmodeloluşturuluyor (). Bu, geçişleri geçmiş tablosunu varsayılan şekilde yapılandırır, daha sonra geçersiz kılma yönteminde ince ayar.

Geçişleri geçmiş tablosunu yeniden adlandırmak ve "admin" adlı özel bir şemaya koymak istediğinizi varsayalım. Ayrıca, DBA 'nın MigrationID sütununu geçiş\_KIMLIĞI olarak yeniden adlandırmanızı istersiniz.  Bu, aşağıdaki bir türeme bağlamından türetilen sınıfı oluşturarak bunu elde edebilirsiniz:

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

Özel bir [ıncode tabanlı yapılandırma](https://msdn.com/data/jj680699)yoluyla kaydederek özel bir ın,

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

Bu çok önemlidir. Artık Paket Yöneticisi konsoluna, Enable-geçişleri, Add-Migration ve finally Update-Database ' e gidebilirsiniz. Bu, geçmişe yönelik bağlam türetilmiş sınıfında belirttiğiniz ayrıntılara göre yapılandırılmış bir geçiş geçmişi tablosu veritabanına eklenmesine neden olmalıdır.

![Database](~/ef6/media/database.png)
