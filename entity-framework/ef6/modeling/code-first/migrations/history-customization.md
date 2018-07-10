---
title: Geçişleri geçmiş tablosu - EF6 özelleştirme
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
caps.latest.revision: 3
ms.openlocfilehash: 3805d99be6d37d037096f5c5fb69fd30197c1e91
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914039"
---
# <a name="customizing-the-migrations-history-table"></a>Geçişleri geçmiş tablosu özelleştirme
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

> [!NOTE]
> Bu makalede, temel senaryolarda Code First Migrations nasıl kullanılacağını varsayar. Sizin sonra okumak ihtiyacınız olacak [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) devam etmeden önce.

## <a name="what-is-migrations-history-table"></a>Geçişleri geçmiş tablosu nedir?

Geçişleri geçmiş tablosu, Code First Migrations ile veritabanına uygulanan geçişleri ayrıntılarını depolamak için kullanılan bir tablodur. Varsayılan olarak veritabanındaki tablo adıdır. \_ \_MigrationHistory ve ilk veritabanı geçiş do uygularken oluşturulur. Uygulama, Microsoft Sql Server veritabanı kullandıysanız Entity Framework 5'te bu tabloda sistem tablosunda oluştu. Bu Entity Framework 6 ancak değişti ve geçişleri geçmiş tablosu bir sistem tablosu artık işaretlenir.

## <a name="why-customize-migrations-history-table"></a>Neden geçişleri geçmiş tablosu özelleştirebilir?

Geçişleri geçmiş tablosunda yalnızca Code First Migrations tarafından kullanılan beklenir ve el ile değiştirme geçişleri bozabilir. Ancak bazen varsayılan yapılandırmayı uygun değil ve tabloda, örneği için özelleştirilmiş olması gerekir:

-   Adları ve/veya sütun 3 etkinleştirmek için modelleri değiştirmeniz gereken<sup>rd</sup> taraf geçişleri sağlayıcısı
-   Tablonun adını değiştirmek istediğiniz
-   Varsayılan olmayan şema için kullanmanız gereken \_ \_MigrationHistory tablo
-   Belirli bir sürümü bağlam için ek verileri depolamak gerekir ve bu nedenle, başka bir sütuna tabloya eklemeniz gerekir

## <a name="words-of-precaution"></a>Önlem sözcükleri

Geçiş geçmişi tablosu değiştirme güçlüdür ancak uzaklaşmasına konusunda dikkatli olmanız gerekir. EF çalışma zamanı şu anda özelleştirilmiş geçişleri geçmiş tablosu çalışma zamanı ile uyumlu olup olmadığını kontrol etmez. Uygulamanızı değilse, çalışma zamanında sonu olabilir veya beklenmeyen şekilde davranır. Bu veritabanı başına birden fazla bağlamı kullanırsanız bu durumda birden fazla bağlamı aynı geçiş geçmiş tablosu geçişleri hakkında bilgi depolamak için kullanabilirsiniz daha da önem taşır.

## <a name="how-to-customize-migrations-history-table"></a>Geçişleri geçmiş tablosu özelleştirmek nasıl?

Başlamadan önce ilk geçiş yalnızca uygulamadan önce geçişler geçmiş tablosu özelleştirebilirsiniz bilmeniz gerekir. Artık, kod.

İlk olarak, System.Data.Entity.Migrations.History.HistoryContext sınıfından türetilen bir sınıf oluşturmanız gerekecektir. Yapılandırma geçişleri geçmiş tablosu EF modelleri fluent API'si ile yapılandırmak için çok benzer şekilde HistoryContext sınıfı DbContext sınıfından türetilir. OnModelCreating yöntemi yok sayın ve tablo yapılandırmak için fluent API'sini kullanmak yeterlidir.

>[!NOTE]
> Genellikle EF modeli yapılandırırken temel çağrı gerekmez. Geçersiz kılınan OnModelCreating yönteminden DbContext.OnModelCreating() beri OnModelCreating() boş bir gövdeye sahip. Bu durum geçişlerini geçmiş tablosu yapılandırırken değildir. Bu durumda ilk OnModelCreating() geçersiz kılmada yapılacak şey gerçekten temel çağırmaktır. OnModelCreating(). Bu geçersiz kılma yöntemi ince ayar varsayılan şekilde geçişleri geçmiş tablosu yapılandıracaksınız.

Geçişleri geçmişi tabloyu yeniden adlandırma ve "Yönetici" adlı bir özel şemaya koymak istediğiniz varsayalım. Ayrıca, DBA geçiş MigrationId sütunu yeniden adlandırmak istediğiniz\_kimliği  Aşağıdaki HistoryContext türetilmiş sınıf oluşturarak bunu:

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

EF ile kaydederek bu haberdar olmak gerekir, özel HistoryContext hazır olduktan sonra [kod tabanlı yapılandırma](http://msdn.com/data/jj680699):

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

Tarayıcınızdaki İşte bu kadar. Etkinleştir-geçişler, Paket Yöneticisi konsolu gidebilirsiniz artık geçiş Ekle ve son olarak veritabanını güncelleştir. Bu, veritabanı geçişleri geçmiş tablosu HistoryContext türetilmiş sınıfınızın belirtilen ayrıntılara göre yapılandırılmış ekleme neden olur.

![Veritabanı](~/ef6/media/database.png)
