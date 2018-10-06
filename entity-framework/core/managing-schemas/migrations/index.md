---
title: Geçişleri - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 5ae06a4342a556936dc44c5bf6622814eaad4733
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834753"
---
<a name="migrations"></a>Geçişleri
==========

Bir veri modeli, geliştirme sırasında değiştirir ve veritabanı ile eşitlenmemiş alır. Veritabanını bırakın ve EF modeli eşleşen yeni bir tane oluşturun sağlar, ancak bu yordamı veri kaybıyla sonuçlanır. EF Core geçişleri özelliği, veritabanındaki mevcut verileri korurken uygulamanın veri modeli ile eşitlenmiş saklamak için veritabanı şemasına artımlı olarak güncelleştirmek için bir yol sağlar.

Geçişler, komut satırı araçları ve aşağıdaki görevlerde size yardımcı API'leri içerir:

* [Bir geçiş oluşturmak](#create-a-migration). Bir model değişiklik kümesini eşitlemek için veritabanını güncellemek kod oluşturur.
* [Veritabanını güncellemek](#update-the-database). Veritabanı şemasını güncelleştirmek için geçişler için geçerlidir.
* [Geçiş kodu özelleştirme](#customize-migration-code). Bazen takıma veya değiştirilecek oluşturulan kodu gerekiyor.
* [Bir geçiş Kaldır](#remove-a-migration). Oluşturulan kodun silin.
* [Bir geçiş geri](#revert-a-migration). Veritabanı değişiklikleri geri al.
* [SQL komut dosyaları üret](#generate-sql-scripts). Bir üretim veritabanını güncelleştirmek için veya geçiş kodu sorunlarını gidermek için bir komut dosyası gerekebilir.
* [Geçişler, çalışma zamanında uygulama](#apply-migrations-at-runtime). Tasarım zamanı güncelleştirmeleri ve betiklerin çalıştırılmasını en iyi seçenekleri değilken çağrı `Migrate()` yöntemi.

<a name="install-the-tools"></a>Araçları yükleme
-----------------

Yükleme [komut satırı araçları](xref:core/miscellaneous/cli/index):
* Visual Studio için öneririz [Paket Yöneticisi konsolu Araçları](xref:core/miscellaneous/cli/powershell).
* Diğer geliştirme ortamlarında seçin [.NET Core CLI Araçları](xref:core/miscellaneous/cli/dotnet).

<a name="create-a-migration"></a>Bir geçiş oluşturun
------------------

Sonra [ilk modelinizi tanımlanan](xref:core/modeling/index), veritabanı oluşturma zamanı geldi. Bir başlangıç geçiş eklemek için aşağıdaki komutu çalıştırın.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Üç dosyayı projenize altında eklenir **geçişler** dizini:

* **00000000000000_InitialCreate.cs**--ana geçişleri dosya. Geçiş uygulamak gerekli işlemleri içerir (içinde `Up()`) ve dönmek için (içinde `Down()`).
* **00000000000000_InitialCreate.Designer.cs**--geçişleri meta veri dosyası. EF tarafından kullanılan bilgileri içerir.
* **MyContextModelSnapshot.cs**--bir anlık görüntü geçerli modelinizin. Sonraki geçiş eklerken nelerin değiştiğini belirlemek için kullanılır.

Zaman damgası dosya değişiklikleri ilerleyişini gördüğünüz şekilde kronolojik olarak sıralanan kalmalarını yardımcı olur.

> [!TIP]
> Geçişleri dosyalarını taşıma ve kendi ad alanı değiştirmek ücretsizdir. Yeni geçiş son geçiş bir eşdüzeyi olarak oluşturulur.

<a name="update-the-database"></a>Veritabanını Güncelleştir
-------------------

Ardından, geçiş veritabanına şema oluşturmak için geçerlidir.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="customize-migration-code"></a>Geçiş kodu özelleştirme
------------------------

EF Core modelinizi değişiklikleri yaptıktan sonra veritabanı şemasını eşitlenmemiş olabilir. Bu en güncel duruma getirmek için başka bir geçiş ekleyin. Geçiş adı gibi bir işleme iletisi, bir sürüm denetim sisteminde kullanılabilir. Örneğin, bir ad gibi seçebilir *AddProductReviews* incelemeleri için yeni bir varlık sınıfı değişiklikse.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

(Bunun için oluşturulan iskele kurulmuş kodu) geçiş başladıktan sonra doğruluğunu kodu gözden geçirin ve eklemek, kaldırmak veya doğru uygulamak için gerekli işlemleri değiştirin.

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

Bu işlemler veritabanı şeması uyumlu yaparken, mevcut müşteri adları korumak yok. Daha iyi hale getirmek için şu şekilde yeniden.

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
> Bir işlem (bir sütunun düşürülmesine gibi) veri kaybına neden olabilir, geçiş iskele oluşturma işlemi konusunda sizi uyarır. Bu uyarıyı görürseniz, doğruluk geçiş kodunu gözden geçirmek özellikle emin olun.

Geçiş uygun komutu kullanılarak veritabanına uygulanır.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a>Boş geçişleri

Bazen model değişiklik yapmadan bir geçiş eklemek yararlıdır. Bu durumda, yeni bir geçiş ekleme kodu dosyaları ile boş sınıflar oluşturur. EF Core modele doğrudan ilişkili olmayan işlemleri gerçekleştirmek için bu geçiş özelleştirebilirsiniz. Bu şekilde yönetmek için isteyebileceğiniz bazı işlemler şunlardır:

* Tam metin araması
* İşlevler
* Saklı yordamlar
* Tetikleyiciler
* Görünümler

<a name="remove-a-migration"></a>Bir geçiş Kaldır
------------------
Bazen bir geçiş ekleyin ve uygulamadan önce EF Core modelinizi ek değişiklikler yapmanız gerekiyorsa farkında olun. Son geçiş kaldırmak için bu komutu kullanın.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Yükseltme kaldırdıktan sonra ek model değişiklikleri yapın ve yeniden ekleyin.

<a name="revert-a-migration"></a>Bir geçiş geri döndür
------------------
Zaten bir geçiş (veya birkaç geçişler) veritabanı, ancak gereksinim dönmek için uyguladığınız, geçişleri geçerli, ancak geri almak istediğiniz geçiş adını belirtmek için aynı komutu kullanabilirsiniz.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="generate-sql-scripts"></a>SQL komut dosyaları üret
--------------------
Ne zaman geçiş hata ayıklama veya bunları üretim veritabanı için dağıtma, bir SQL betiği oluşturmak kullanışlıdır. Betik daha sonra daha fazla doğruluk gözden ve bir üretim veritabanının gereksinimlerinize uyacak şekilde ayarlanmış. Betik, bir dağıtım teknolojisi ile birlikte de kullanılabilir. Temel komut aşağıdaki gibidir.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Bu komut için birkaç seçenek vardır.

**Gelen** geçiş betiği çalıştırmadan önce veritabanına uygulanan son geçiş olmalıdır. Hiçbir geçiş uygulandıysa belirtin `0` (varsayılan değer budur).

**İçin** geçiş için veritabanı, betiği çalıştırdıktan sonra uygulanacak son geçiş olur. Bu, son geçiş, projenizdeki varsayar.

Bir **ıdempotent** betik isteğe bağlı olarak oluşturulabilir. Bunlar zaten veritabanına sorgularınızda uygulanmamış bu betik yalnızca geçişleri geçerlidir. Ne son geçiş veritabanına uygulanan tam olarak emin değilseniz yaradı veya her farklı bir geçiş olabilir. birden fazla veritabanına dağıtıyorsanız, budur.

<a name="apply-migrations-at-runtime"></a>Çalışma zamanında geçişleri Uygula
---------------------------
Bazı uygulamalar, geçişleri çalışma zamanında başlatma sırasında uygulama veya ilk çalıştırma isteyebilirsiniz. Bunu yapmak `Migrate()` yöntemi.

Bu yöntem, üst kısmındaki yapılar `IMigrator` daha Gelişmiş senaryolar için kullanılan hizmet. Kullanım `DbContext.GetService<IMigrator>()` erişmek için.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> * Bu yaklaşım herkese uygun değildir. Yerel veritabanı içeren uygulamalar için mükemmel olmakla birlikte, çoğu uygulama SQL betikleri oluşturma gibi daha güçlü Dağıtım stratejisi gerektirir.
> * Remove() çağırmayın `EnsureCreated()` önce `Migrate()`. `EnsureCreated()` neden olan şema oluşturmaya geçişleri atlar `Migrate()` başarısız.

<a name="next-steps"></a>Sonraki adımlar
----------

Daha fazla bilgi için bkz. <xref:core/miscellaneous/cli/index>.