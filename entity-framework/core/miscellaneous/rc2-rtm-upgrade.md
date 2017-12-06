---
title: "Çekirdek 1.0 RC2 - RTM için EF yükseltme EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 7a1d85949a5f9e1ad7efdbf585a608d815e8ce63
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>RTM'ye EF çekirdek 1.0 RC2 ' yükseltme

Bu makalede 1.0.0 RC2 paketler ile oluşturulan bir uygulamayı taşıma için kılavuz sağlar RTM.

## <a name="package-versions"></a>Paketi sürümleri

Genellikle bir uygulamayı yükler en üst düzey paketleri adlarını RC2 ve RTM arasında değişmemiştir.

**Yüklü paketler RTM sürümüne yükseltme yapmanız gerekir:**

* Çalışma zamanı paketleri (örneğin `Microsoft.EntityFrameworkCore.SqlServer`) değiştirildi `1.0.0-rc2-final` için `1.0.0`.

* `Microsoft.EntityFrameworkCore.Tools` Paket değiştirildi `1.0.0-preview1-final` için `1.0.0-preview2-final`. Araç hala yayın öncesi olduğuna dikkat edin.

## <a name="existing-migrations-may-need-maxlength-added"></a>Varolan geçişleri eklenen maxLength gerekebilir

Gibi bir geçiş sütun tanımında RC2 içinde arama `table.Column<string>(nullable: true)` ve sütun uzunluğu depolarız geçiş arkasındaki kodda bazı meta verilerde arama. RTM içinde uzunluğu kurulmuş kod artık dahil `table.Column<string>(maxLength: 450, nullable: true)`.

RTM kullanılmadan önce iskele kurulmuş tüm mevcut geçişler sahip olmamak `maxLength` belirtilen bağımsız değişken. Bu veritabanı tarafından desteklenen en fazla uzunluk kullanılacak anlamına gelir (`nvarchar(max)` SQL Server üzerinde). Bu bazı sütunları, ancak bir anahtar, yabancı anahtar parçası olan sütunlar için ince olabilir veya dizin en fazla içerecek şekilde güncelleştirilmesi gerekir. Kurala göre 450 en fazla uzunluk yabancı anahtarlar anahtarları için kullanılan sütun dizini ise. Ardından bir uzunluk modelde yapılandırdıysanız bu uzunluğu yerine kullanmanız gerekir.

**ASP.NET kimliği**

Bu değişiklik öncesi oluşturulmuş ve ASP.NET Identity kullanan projeleri etkiler-proje şablonu RTM. Proje şablonu veritabanı oluşturmak için kullanılan bir geçiş içerir. En büyük uzunluğunu belirtmek için bu geçiş düzenlenmesi `256` şu sütunlar için.

*  **AspNetRoles**

    * Ad

    * NormalizedName

*  **AspNetUsers**

   * E-posta

   * NormalizedEmail

   * NormalizedUserName

   * Kullanıcı adı

Bir veritabanına ilk geçiş uygulandığında hata bu değişikliği yapmak için aşağıdaki özel durum neden olur.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: "Imports" project.json kaldırın.

.NET Core RC2 ile hedefleme, eklemek için gereken `imports` .NET standart desteklemediğinden EF çekirdek'ın bağımlılıkları bazıları için geçici bir çözüm olarak project.json için. Bunlar artık kaldırılabilir.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

## <a name="uwp-add-binding-redirects"></a>UWP: bağlama yeniden yönlendirmeleri ekleyin

Evrensel Windows Platformu (UWP) projeleri sonuçlarını aşağıdaki hata EF komutları çalıştırma denemesi:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

UWP projesini bağlama yeniden yönlendirmeleri el ile eklemeniz gerekir. Adlı bir dosya oluşturun `App.config` projesinde kök klasör ve doğru derleme sürümlerini yeniden yönlendirmeleri ekleyin.

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
