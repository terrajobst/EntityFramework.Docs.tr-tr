---
title: Göçler - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 190057daed61c58c1f89ee8d775913458e413a50
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136205"
---
# <a name="migrations"></a>Geçişler

Bir veri modeli geliştirme sırasında değişir ve veritabanı ile eşitlenmemiş olur. Veritabanını bırakabilir ve EF'nin modelle eşleşen yeni bir veritabanı oluşturmasına izin verebilirsiniz, ancak bu yordam veri kaybına neden olabilir. EF Core'daki geçişler özelliği, veritabanındaki varolan verileri korurken uygulamanın veri modeliyle eşit tutmak için veritabanı şemasını aşamalı olarak güncelleştirmenin bir yolunu sağlar.

Geçişler, aşağıdaki görevlere yardımcı olan komut satırı araçlarını ve API'leri içerir:

* [Geçiş oluşturun.](#create-a-migration) Veritabanını bir model değişikliği kümesiyle eşitlemek için güncelleştirebilecek kod oluşturun.
* [Veritabanını güncelleştirin.](#update-the-database) Veritabanı şemasını güncelleştirmek için bekleyen geçişleri uygulayın.
* [Geçiş kodunu özelleştirin.](#customize-migration-code) Bazen oluşturulan kodun değiştirilmesi veya desteklenmesi gerekir.
* [Geçişkaldırma.](#remove-a-migration) Oluşturulan kodu silin.
* [Geçişi geri çevirin.](#revert-a-migration) Veritabanı değişikliklerini geri ala.
* [SQL komut dosyaları oluşturun.](#generate-sql-scripts) Üretim veritabanını güncelleştirmek veya geçiş kodunu gidermek için bir komut dosyasına ihtiyacınız olabilir.
* [Geçişleri çalışma zamanında uygulayın.](#apply-migrations-at-runtime) Tasarım zamanı güncelleştirmeleri ve çalışan komut dosyaları en iyi `Migrate()` seçenek olmadığında, yöntemi arayın.

> [!TIP]
> Başlangıç `DbContext` projesinden farklı bir derlemedeyse, [Paket Yöneticisi Konsolu araçlarında](xref:core/miscellaneous/cli/powershell#target-and-startup-project) veya [.NET Core CLI araçlarında](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project)hedef ve başlangıç projelerini açıkça belirtebilirsiniz.

## <a name="install-the-tools"></a>Araçları yükleme

Komut [satırı araçlarını](xref:core/miscellaneous/cli/index)yükleyin:

* Visual Studio için [Paket Yöneticisi Konsolu araçlarını](xref:core/miscellaneous/cli/powershell)öneririz.
* Diğer geliştirme ortamları için [.NET Core CLI araçlarını](xref:core/miscellaneous/cli/dotnet)seçin.

## <a name="create-a-migration"></a>Geçiş oluşturma

[İlk modelinizi tanımladıktan](xref:core/modeling/index)sonra veritabanını oluşturma nın zamanı doldu. İlk geçiş eklemek için aşağıdaki komutu çalıştırın.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add InitialCreate
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration InitialCreate
```

***

**Geçişler** dizininde projenize üç dosya eklenir:

* **XXXXXXXXXXXXXX_InitialCreate.cs**--Ana geçişler dosyası. Geçiş (in) `Up()`uygulamak ve geri almak için (içinde) `Down()`gerekli işlemleri içerir.
* **XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--Geçişler meta veri dosyası. EF tarafından kullanılan bilgileri içerir.
* **MyContextModelSnapshot.cs**-- Geçerli modelinizin anlık görüntüsü. Sonraki geçiş eklenince nelerin değiştiğini belirlemek için kullanılır.

Dosya adındaki zaman damgası, değişikliklerin ilerlemesini görebilmeniz için kronolojik olarak sıralı kalmalarına yardımcı olur.

> [!TIP]
> Geçişler dosyalarını taşımak ve ad alanlarını değiştirmek ücretsizdir. Yeni geçişler son geçişin kardeşleri olarak oluşturulur.

## <a name="update-the-database"></a>Veritabanını güncelleştirme

Ardından, şemayı oluşturmak için geçiş işimdi veritabanına uygulayın.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

## <a name="customize-migration-code"></a>Geçiş kodunu özelleştirme

EF Core modelinizde değişiklik yaptıktan sonra, veritabanı şeması eşitlenmemiş olabilir. Güncel getirmek için başka bir geçiş ekleyin. Geçiş adı, sürüm denetim sisteminde bir ileti iletisi gibi kullanılabilir. Örneğin, değişiklik incelemeler için yeni bir varlık sınıfıysa *AddProductReviews* gibi bir ad seçebilirsiniz.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add AddProductReviews
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration AddProductReviews
```

***

Geçiş iskelesi (bunun için oluşturulan kod) oluşturulduktan sonra, doğruluk için kodu gözden geçirin ve doğru uygulamak için gereken işlemleri ekleyin, kaldırın veya değiştirin.

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

Bu işlemler veritabanı şema uyumlu hale getirmekle birlikte, varolan müşteri adlarını korumaz. Daha iyi hale getirmek için aşağıdaki gibi yeniden yazın.

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
> Geçiş iskelesi işlemi, bir işlemin veri kaybına (sütun düşürme gibi) neden olabileceği konusunda uyarır. Bu uyarıyı görürseniz, doğruluk için geçişler kodunu gözden geçirdiğinizden özellikle emin olun.

Geçişi uygun komutu kullanarak veritabanına uygulayın.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database
```

***

### <a name="empty-migrations"></a>Boş geçişler

Bazen herhangi bir model değişikliği yapmadan bir geçiş eklemek yararlıdır. Bu durumda, yeni bir geçiş eklemek boş sınıfları olan kod dosyaları oluşturur. Bu geçişi, EF Core modeliyle doğrudan ilişkili olmayan işlemleri gerçekleştirmek için özelleştirebilirsiniz. Bu şekilde yönetmek isteyebileceğin bazı şeyler şunlardır:

* Tam Metin Arama
* İşlevler
* Saklı yordamlar
* Tetikleyiciler
* Görünümler

## <a name="remove-a-migration"></a>Geçişi kaldırma

Bazen bir geçiş ekler ve uygulamadan önce EF Core modelinizde ek değişiklikler yapmanız gerektiğini fark elersiniz. Son geçişi kaldırmak için bu komutu kullanın.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations remove
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Remove-Migration
```

***

Geçişi kaldırdıktan sonra, ek model değişiklikleri yapabilir ve yeniden ekleyebilirsiniz.

## <a name="revert-a-migration"></a>Geçişi geri almak

Veritabanına zaten bir geçiş (veya birkaç geçiş) uyguladıysanız, ancak geri almanız gerekiyorsa, geçişleri uygulamak için aynı komutu kullanabilirsiniz, ancak geri almak istediğiniz geçişin adını belirtebilirsiniz.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update LastGoodMigration
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Update-Database LastGoodMigration
```

***

## <a name="generate-sql-scripts"></a>SQL komut dosyaları oluşturma

Geçişlerinizin hata ayıklanması veya bunları bir üretim veritabanına dağıtırken, bir SQL komut dosyası oluşturmak yararlıdır. Komut dosyası daha sonra doğruluk açısından daha fazla gözden geçirilebilir ve bir üretim veritabanının gereksinimlerine uyacak şekilde ayarlanabilir. Komut dosyası, dağıtım teknolojisiyle birlikte de kullanılabilir. Temel komut aşağıdaki gibidir.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

#### <a name="basic-usage"></a>Temel Kullanım
```dotnetcli
dotnet ef migrations script
```

#### <a name="with-from-to-implied"></a>From ile (zımni olarak)
Bu, bu geçişten en son geçişe kadar bir SQL komut dosyası oluşturur.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>From ve To ile
Bu, `from` geçişten belirtilen `to` geçişe kadar bir SQL komut dosyası oluşturur.
```dotnetcli
dotnet ef migrations script 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
Geri alma `from` komut dosyası oluşturmak `to` için daha yeni bir komut dosyası kullanabilirsiniz. *Lütfen olası veri kaybı senaryolarına dikkat edin.*

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

#### <a name="basic-usage"></a>Temel Kullanım
``` powershell
Script-Migration
```

#### <a name="with-from-to-implied"></a>From ile (zımni olarak)
Bu, bu geçişten en son geçişe kadar bir SQL komut dosyası oluşturur.
```powershell
Script-Migration 20190725054716_Add_new_tables
```

#### <a name="with-from-and-to"></a>From ve To ile
Bu, `from` geçişten belirtilen `to` geçişe kadar bir SQL komut dosyası oluşturur.
```powershell
Script-Migration 20190725054716_Add_new_tables 20190829031257_Add_audit_table
```
Geri alma `from` komut dosyası oluşturmak `to` için daha yeni bir komut dosyası kullanabilirsiniz. *Lütfen olası veri kaybı senaryolarına dikkat edin.*

***

Bu komutun birkaç seçeneği vardır.

**Geçiş,** komut dosyasını çalıştırmadan önce veritabanına uygulanan son geçiş olmalıdır. Geçiş uygulanmadıysa, belirtin `0` (bu varsayılandır).

**Geçiş,** komut dosyasını çalıştırdıktan sonra veritabanına uygulanacak son geçiştir. Bu, projenizdeki son geçiş için varsayılandır.

İsteğe bağlı olarak **bir idempotent** komut dosyası oluşturulabilir. Bu komut dosyası, yalnızca veritabanına zaten uygulanmamışsa geçişleri uygular. Veritabanına uygulanan son geçişin ne olduğunu tam olarak bilmiyorsanız veya her biri farklı bir geçişte olabilecek birden çok veritabanına dağıtıyorsanız bu kullanışlıdır.

## <a name="apply-migrations-at-runtime"></a>Geçişleri çalışma zamanında uygulama

Bazı uygulamalar, başlangıç veya ilk çalıştırma sırasında çalışma zamanında geçişuygulamak isteyebilir. Yöntemi kullanarak `Migrate()` bunu yapın.

Bu yöntem, daha gelişmiş `IMigrator` senaryolar için kullanılabilecek hizmetin üstüne oluşturur. Erişmek `myDbContext.GetInfrastructure().GetService<IMigrator>()` için kullan.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * Bu yaklaşım herkes için değil. Yerel veritabanına sahip uygulamalar için harika olsa da, çoğu uygulama SQL komut dosyası oluşturma gibi daha sağlam dağıtım stratejisi gerektirir.
> * Daha önce `Migrate()` `EnsureCreated()` arama. `EnsureCreated()`başarısız olmaya neden `Migrate()` olan şema oluşturmak için Geçişleri atlar.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. <xref:core/miscellaneous/cli/index>.
