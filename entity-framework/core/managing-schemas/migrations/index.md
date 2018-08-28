---
title: Geçişleri - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 4a5d6f3798c7af7597f95cebea1aeb9e5e58d277
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996528"
---
<a name="migrations"></a>Geçişleri
==========
Geçişler, EF Core modeliniz veritabanındaki mevcut verileri korurken eşitleyin veritabanına artımlı olarak şema değişiklikleri uygulamak için bir yol sağlar.

<a name="creating-the-database"></a>Veritabanı oluşturma
---------------------
Sonra [ilk modelinizi tanımlanan][1], veritabanı oluşturma zamanı geldi. Bunu yapmak için bir başlangıç geçiş ekleyin.
Yükleme [EF Core Araçları] [ 2] ve uygun komutu çalıştırın.

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

Ardından, geçiş veritabanına şema oluşturmak için geçerlidir.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Başka bir geçiş ekleniyor
------------------------
EF Core modelinizi değişiklikleri yaptıktan sonra veritabanı şemasını eşitlenmemiş olacaktır. Bu en güncel duruma getirmek için başka bir geçiş ekleyin. Geçiş adı gibi bir işleme iletisi, bir sürüm denetim sisteminde kullanılabilir. Ben müşteri incelemeleri ürünlerin kaydedilecek değişiklikler yaptıysanız, örneğin, aşağıdaki gibi seçmem *AddProductReviews*.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Geçiş iskele kurulmuş sonra doğruluk gözden geçirmeli ve doğru bir şekilde uygulamak için gerekli olan herhangi bir ek işlem ekleyin. Örneğin, geçişinizi aşağıdaki işlemleri içerebilir:

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
> Bir işlem iskele kurulmuş zaman yeni bir geçiş ekleme (bir sütunun düşürülmesine gibi) veri kaybına neden olabilir sizi uyarır. Özellikle bu geçişler için doğruluk gözden geçirdiğinizden emin olun.

Geçiş uygun komutu kullanılarak veritabanına uygulanır.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Bir geçiş kaldırılıyor
--------------------
Bazen bir geçiş ekleyin ve uygulamadan önce EF Core modelinizi ek değişiklikler yapmanız gerekiyorsa farkında olun.
Son geçiş kaldırmak için bu komutu kullanın.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Kaldırdıktan sonra ek model değişiklikleri yapın ve yeniden ekleyin.

<a name="reverting-a-migration"></a>Bir geçiş döndürülüyor
---------------------
Zaten bir geçiş (veya birkaç geçişler) veritabanı, ancak gereksinim dönmek için uyguladığınız, geçişleri geçerli, ancak geri almak istediğiniz geçiş adını belirtmek için aynı komutu kullanabilirsiniz.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Boş geçişleri
----------------
Bazen model değişiklik yapmadan bir geçiş eklemek yararlıdır. Bu durumda, yeni bir geçiş ekleniyor, boş bir tane oluşturur. EF Core modele doğrudan ilişkili olmayan işlemleri gerçekleştirmek için bu geçiş özelleştirebilirsiniz.
Bu şekilde yönetmek için isteyebileceğiniz bazı işlemler şunlardır:

* Tam metin araması
* İşlevler
* Saklı yordamlar
* Tetikleyiciler
* Görünümler
* VS.

<a name="generating-a-sql-script"></a>SQL komut dosyası oluşturuluyor
-----------------------
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

<a name="applying-migrations-at-runtime"></a>Zamanında uygulanan geçişleri
------------------------------
Bazı uygulamalar, geçişleri çalışma zamanında başlatma sırasında uygulama veya ilk çalıştırma isteyebilirsiniz. Bunu yapmak `Migrate()` yöntemi.

Uyarı, bu yaklaşım herkese uygun değildir. Yerel veritabanı içeren uygulamalar için mükemmel olmakla birlikte, çoğu uygulama SQL betikleri oluşturma gibi daha güçlü Dağıtım stratejisi gerektirir.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> Remove() çağırmayın `EnsureCreated()` önce `Migrate()`. `EnsureCreated()` neden olan şema oluşturmaya geçişleri atlar `Migrate()` başarısız.

> [!NOTE]
> Bu yöntem, üst kısmındaki yapılar `IMigrator` daha Gelişmiş senaryolar için kullanılan hizmet. Kullanım `DbContext.GetService<IMigrator>()` erişmek için.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
