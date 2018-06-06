---
title: Geçişler - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: dd164125c053497af94773011127853ad10d27a6
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754515"
---
<a name="migrations"></a>Geçişleri
==========
Geçişler veritabanında var olan verileri korurken EF çekirdek modeli ile eşitlenmiş tutmak için veritabanının artımlı olarak şema değişiklikleri uygulamak için bir yol sağlar.

<a name="creating-the-database"></a>Veritabanı oluşturuluyor
---------------------
Sonra [ilk modelinizi tanımlanan][1], veritabanı oluşturmak için zamanı. Bunu yapmak için bir başlangıç geçiş ekleyin.
Yükleme [EF çekirdek Araçları] [ 2] ve uygun komutunu çalıştırın.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Üç dosya projenizi altına eklenir **geçişler** dizini:

* **00000000000000_InitialCreate.cs**--ana geçişler dosya. Geçiş uygulamak gerekli işlemleri içerir (içinde `Up()`) ve dönmek için (içinde `Down()`).
* **00000000000000_InitialCreate.Designer.cs**--geçişler meta veri dosyası. EF tarafından kullanılan bilgileri içerir.
* **MyContextModelSnapshot.cs**--anlık görüntüsünü, geçerli model. Sonraki geçiş eklenirken nelerin değiştiğini belirlemek için kullanılır.

Dosya adında zaman damgası, bunları değişiklikleri ilerleyişini görebilmeleri kronolojik olarak sıralanan tutmaya yardımcı olur.

> [!TIP]
> Geçişler dosyaları taşıyın ve kendi ad alanı değiştirmek boş. Yeni geçişler son geçiş eşdüzeyi olarak oluşturulur.

Ardından, geçiş şema oluşturmak için veritabanına uygulayın.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Başka bir geçiş ekleme
------------------------
EF çekirdek modelinizi değişiklikleri yaptıktan sonra veritabanı şemasını eşitlenmemiş olacaktır. Güncel duruma getirmek için başka bir geçiş ekleyin. Geçiş adı gibi bir yürütme ileti bir sürüm denetim sisteminde kullanılabilir. Müşteri incelemelerini ürünlerin kaydedilecek değişiklik yaptıysanız, örneğin, aşağıdakine benzer seçmem *AddProductReviews*.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Geçiş iskele kurulmuş sonra doğruluk için gözden geçirin ve doğru şekilde uygulamak için gerekli herhangi bir ek işlem ekleyin. Örneğin, geçişinizi aşağıdaki işlemleri içerebilir:

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

Bu işlemler veritabanı şeması uyumlu yaparken, mevcut müşteri adları korumak yok. Daha iyi hale getirmek için aşağıdaki gibi yeniden.

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
> Bir işlem iskele kurulmuş zaman yeni bir geçiş ekleme (bir sütunun düşürülmesine gibi) veri kaybı ile sonuçlanabilir sizi uyarır. Özellikle bu geçişler doğruluğu için gözden geçirdiğinizden emin olun.

Geçiş uygun komutu kullanılarak veritabanına uygulayın.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Bir geçiş kaldırma
--------------------
Bazen, bir geçiş ekleyebilir ve uygulamadan önce EF çekirdek modelinizi ek değişiklikler yapmak gereksinim unutmayın.
Son geçiş kaldırmak için bu komutu kullanın.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Kaldırıldıktan sonra ek model değişiklikleri yapın ve yeniden ekleyin.

<a name="reverting-a-migration"></a>Bir geçiş dönüştürme
---------------------
Zaten bir geçiş (ya da birkaç geçişler) veritabanı ancak dönmek için gerek uyguladıysanız, geçişleri uygulayın, ancak geri almak istediğiniz geçiş adını belirtmek için aynı komutu kullanabilirsiniz.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Boş geçişleri
----------------
Bazen bir geçiş modeli değişiklik yapmadan eklemek yararlıdır. Bu durumda, yeni bir geçiş ekleme boş bir tane oluşturur. EF çekirdek modeli doğrudan ilişkili olmayan işlemleri gerçekleştirmek için bu geçiş özelleştirebilirsiniz.
Bu şekilde yönetmek isteyebilirsiniz bazı noktalar şunlardır:

* Tam metin araması
* İşlevler
* Saklı yordamlar
* Tetikleyiciler
* Görünümler
* VS.

<a name="generating-a-sql-script"></a>SQL komut dosyası oluşturma
-----------------------
Ne zaman, geçişler hata ayıklama veya üretim veritabanına dağıtırken, bir SQL komut dosyası oluşturmak kullanışlıdır. Komut dosyası sonra daha doğruluk bakımından gözden ve üretim veritabanını gereksinimlerine uyacak şekilde ayarlanmış. Komut dosyası da bir dağıtım teknolojisi ile birlikte kullanılabilir. Temel komut aşağıdaki gibidir.

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Bu komut için birkaç seçeneğiniz vardır.

**Gelen** geçiş komut dosyasını çalıştırmadan önce veritabanına uygulanan son geçiş olması gerekir. Geçiş uygulanan belirtebilmeniz `0` (varsayılan değer budur).

**İçin** geçiş komut dosyasını çalıştırdıktan sonra veritabanına uygulanacak son geçiş olur. Bu son geçiş projenizdeki varsayılan olarak.

Bir **ıdempotent** betik isteğe bağlı olarak oluşturulabilir. Bunlar zaten veritabanına uygulanmamış bu betik yalnızca geçişler geçerlidir. Tam olarak ne son geçiş veritabanına uygulanan bilmiyorsanız, yararlı, ya da her farklı bir geçiş olabilir birden fazla veritabanı için dağıtıyorsanız budur.

<a name="applying-migrations-at-runtime"></a>Çalışma zamanında uygulanan geçişleri
------------------------------
Bazı uygulamalar başlatma sırasında çalışma zamanında geçişleri uygulayın veya ilk çalıştırmak isteyebilirsiniz. Kullanarak bunu `Migrate()` yöntemi.

Uyarı, bu yaklaşım herkes için değil. Yerel bir veritabanı olan uygulamalar için harika olsa da, çoğu uygulamayı SQL komut dosyaları oluşturma gibi daha sağlam Dağıtım stratejisi gerektirir.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> Çağrı yok `EnsureCreated()` önce `Migrate()`. `EnsureCreated()` neden şema oluşturmak için geçiş atlar `Migrate()` başarısız.

> [!NOTE]
> Bu yöntem üstünde derlemeler `IMigrator` daha Gelişmiş senaryolar için kullanılan hizmet. Kullanım `DbContext.GetService<IMigrator>()` erişmek için.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
