---
title: Geçişler-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: ffa9a34f13ab29f0ba93f9fd1f469398630604ce
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005454"
---
<a name="migrations"></a>Geçişler
==========

Geliştirme sırasında bir veri modeli değişir ve veritabanı ile eşitlenmemiş olur. Veritabanını bırakabilir ve EF ile eşleşen yeni bir tane oluşturmasını sağlayabilirsiniz, ancak bu yordam veri kaybına neden olur. EF Core geçiş özelliği, veritabanı şemasını, veritabanında var olan verileri korurken uygulamanın veri modeliyle eşitlenmiş halde tutmak için artımlı olarak güncellemek üzere bir yol sağlar.

Geçişler, aşağıdaki görevlerle yardım eden komut satırı araçlarını ve API 'Leri içerir:

* [Geçiş oluşturun](#create-a-migration). Veritabanını bir model değişikliği kümesiyle eşitlenecek şekilde güncelleştiren bir kod oluşturun.
* [Veritabanını güncelleştirin](#update-the-database). Veritabanı şemasını güncelleştirmek için bekleyen geçişleri uygulayın.
* [Geçiş kodunu özelleştirin](#customize-migration-code). Bazen oluşturulan kodun değiştirilmesi veya takıma alınması gerekir.
* [Bir geçişi kaldırın](#remove-a-migration). Oluşturulan kodu silin.
* [Bir geçişi döndürür](#revert-a-migration). Veritabanı değişikliklerini geri al.
* [SQL betikleri oluşturun](#generate-sql-scripts). Bir üretim veritabanını güncelleştirmek veya geçiş kodu sorunlarını gidermek için bir betiğe gerek duyabilirsiniz.
* [Çalışma zamanında geçişleri uygulayın](#apply-migrations-at-runtime). Tasarım zamanı güncelleştirmeleri ve çalıştırma betikleri en iyi seçenek olmadığında, `Migrate()` yöntemi çağırın.

> [!TIP]
> Başlangıç projesinden farklı bir derlemede [](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project) ise,hedefvebaşlangıçprojelerinipaketYöneticisikonsolaraçlarıveya.NETCoreCLIaraçları'ndaaçıkça`DbContext` belirtebilirsiniz. [](xref:core/miscellaneous/cli/powershell#target-and-startup-project)

<a name="install-the-tools"></a>Araçları yükler
-----------------

[Komut satırı araçlarını](xref:core/miscellaneous/cli/index)yükler:
* Visual Studio için [Paket Yöneticisi konsol araçları](xref:core/miscellaneous/cli/powershell)önerilir.
* Diğer geliştirme ortamları için [.NET Core CLI araçları](xref:core/miscellaneous/cli/dotnet)' nı seçin.

<a name="create-a-migration"></a>Geçiş oluşturma
------------------

[İlk modelinizi tanımladıktan](xref:core/modeling/index)sonra, veritabanının oluşturulması zaman alabilir. İlk geçiş eklemek için aşağıdaki komutu çalıştırın.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

**Geçiş** dizini altında projenize üç dosya eklenir:

* **XXXXXXXXXXXXXX_InitialCreate. cs**--ana geçişler dosyası. Geçişi uygulamak için gereken işlemleri (içinde `Up()`) ve döndürür (içinde `Down()`).
* **XXXXXXXXXXXXXX_InitialCreate. Designer. cs**--geçişler meta verileri dosyası. EF tarafından kullanılan bilgileri içerir.
* **MyContextModelSnapshot.cs**--geçerli modelinize ait bir anlık görüntü. Sonraki geçiş eklenirken nelerin değiştiğini belirlemek için kullanılır.

Dosya adında zaman damgası, değişikliklerin ilerlemesini görebileceğiniz şekilde kronolojik olarak sıralanmasına yardımcı olur.

> [!TIP]
> Geçiş dosyalarını taşımak ve ad alanını değiştirmek ücretsizdir. Yeni geçişler, son geçişin eşdüzey öğeleri olarak oluşturulur.

<a name="update-the-database"></a>Veritabanını güncelleştirme
-------------------

Sonra, şemayı oluşturmak için geçiş veritabanını veritabanına uygulayın.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a>Geçiş kodunu özelleştirme
------------------------

EF Core modelinizde değişiklik yaptıktan sonra veritabanı şeması eşitlenmemiş olabilir. Bunu güncel hale getirmek için başka bir geçiş ekleyin. Geçiş adı, bir sürüm denetim sisteminde bir COMMIT iletisi gibi kullanılabilir. Örneğin, gözden geçirmeler için değişiklik yeni bir varlık sınıfı ise *Addproductincelemeleri* gibi bir ad seçebilirsiniz.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Geçiş, daha sonra (bunun için oluşturulan kod) bir kez alındıktan sonra, kodu doğruluk için gözden geçirin ve doğru uygulamak için gereken tüm işlemleri ekleyin, kaldırın veya değiştirin.

Örneğin, bir geçiş aşağıdaki işlemleri içerebilir:

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

Bu işlemler veritabanı şemasını uyumlu hale getirir, ancak mevcut müşteri adlarını korumaz. Daha iyi hale getirmek için aşağıdaki gibi yeniden yazın.

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> Geçiş yapı iskelesi işlemi, bir işlem veri kaybına neden olabileceği zaman uyarır (bir sütunu bırakma gibi). Uyarı görürseniz, özellikle de geçişler için geçiş kodunu gözden geçirdiğinizden emin olun.

Uygun komutu kullanarak geçişi veritabanına uygulayın.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a>Boş geçişler

Bazen herhangi bir model değişikliği yapmadan bir geçiş eklemek yararlı olabilir. Bu durumda, yeni bir geçiş eklemek boş sınıflarla kod dosyaları oluşturur. Bu geçişi, EF Core modeliyle doğrudan bağlantılı olmayan işlemleri gerçekleştirmek için özelleştirebilirsiniz. Bu şekilde yönetmek isteyebileceğiniz bazı şeyler şunlardır:

* Tam metin arama
* İşlevler
* Saklı yordamlar
* Tetikleyiciler
* Görünümler

<a name="remove-a-migration"></a>Geçiş kaldırma
------------------
Bazen bir geçiş ekleyip EF Core modelinizde uygulamadan önce ek değişiklikler yapmanız gerektiğini fark edebilirsiniz. Son geçişi kaldırmak için bu komutu kullanın.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Geçiş kaldırıldıktan sonra, ek model değişiklikleri yapıp yeniden ekleyebilirsiniz.

<a name="revert-a-migration"></a>Geçişi döndürür
------------------
Veritabanına zaten bir geçiş (veya birkaç geçiş) uyguladıysanız ancak geri döndürmeniz gerekiyorsa, geçişleri uygulamak için aynı komutu kullanabilirsiniz, ancak geri almak istediğiniz geçişin adını belirtebilirsiniz.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a>SQL betikleri oluştur
--------------------
Geçişlerinizi hata ayıkladığınızda veya bir üretim veritabanına dağıttığınızda, bir SQL betiği oluşturmak yararlı olur. Betik daha sonra doğruluk açısından daha fazla incelenebilir ve bir üretim veritabanının ihtiyaçlarına uyacak şekilde ayarlanabilir. Betik Ayrıca bir dağıtım teknolojisiyle birlikte kullanılabilir. Temel komut aşağıdaki gibidir.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Bu komut için birkaç seçenek vardır.

Komut dosyası çalıştırılmadan önce geçiş **işleminden** önce veritabanına uygulanan son geçiş yapılmalıdır. Hiçbir geçiş uygulanmışsa, (bu varsayılandır) `0` belirtin.

**-** Geçiş, komut dosyası çalıştırıldıktan sonra veritabanına uygulanacak son geçiştir. Bu, varsayılan olarak projenizdeki son geçişe göre yapılır.

İsteğe bağlı olarak **ıdempotent** betiği oluşturulabilir. Bu betik yalnızca, veritabanına henüz uygulanmadıklarında geçişler uygular. Bu, veritabanına en son geçişin ne olduğunu tam olarak bilmiyorsanız veya her biri farklı bir geçişte olabilecek birden çok veritabanına dağıtım yapıyorsanız kullanışlıdır.

<a name="apply-migrations-at-runtime"></a>Çalışma zamanında geçişleri Uygula
---------------------------
Bazı uygulamalar, başlangıç veya ilk çalıştırma sırasında çalışma zamanında geçiş uygulamak isteyebilir. `Migrate()` Yöntemini kullanarak bunu yapın.

Bu yöntem, daha gelişmiş senaryolar için `IMigrator` kullanılabilen hizmetin üst kısmında oluşturulur. Erişmek `myDbContext.GetInfrastructure().GetService<IMigrator>()` için kullanın.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * Bu yaklaşım herkes için değildir. Yerel bir veritabanına sahip uygulamalar için harika olsa da, çoğu uygulama SQL betikleri oluşturma gibi daha güçlü dağıtım stratejisi gerektirir.
> * Daha önce `EnsureCreated()` `Migrate()`çağırmayın. `EnsureCreated()`Şemayı oluşturmak için geçişleri atlar, bu da başarısız `Migrate()` olmasına neden olur.

<a name="next-steps"></a>Sonraki adımlar
----------

Daha fazla bilgi için bkz. <xref:core/miscellaneous/cli/index>.
