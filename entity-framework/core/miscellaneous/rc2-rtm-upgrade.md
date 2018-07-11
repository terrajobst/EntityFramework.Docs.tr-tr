---
title: 1.0 RC2 - RTM için çekirdek EF yükseltme EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 9561eac253517188251fece9a03f434482246051
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949063"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>EF Core 1.0 RC2'den RTM'ye yükseltme

Bu makale 1.0.0 RC2 paketler ile oluşturulmuş bir uygulama taşıma konusunda rehberlik yapmaktadır RTM.

## <a name="package-versions"></a>Paket sürümü

Bir uygulamaya normal olarak yüklenir üst düzey paketlerin adlarını RC2 ile RTM arasında değişmemiştir.

**Yüklü paketleri RTM sürümüne yükseltme yapmanız gerekir:**

* Çalışma zamanı paketlerini (örneğin, `Microsoft.EntityFrameworkCore.SqlServer`) değiştirildi `1.0.0-rc2-final` için `1.0.0`.

* `Microsoft.EntityFrameworkCore.Tools` Paket değiştirildi `1.0.0-preview1-final` için `1.0.0-preview2-final`. Araç hala yayın öncesi olduğuna dikkat edin.

## <a name="existing-migrations-may-need-maxlength-added"></a>Mevcut geçişleri eklenen maxLength gerekebilir

RC2'deki gibi bir geçiş sütun tanımında baktığı `table.Column<string>(nullable: true)` ve sütun uzunluğu depolarız geçiş arkasındaki kodda bazı meta verilerinde aranabilir. RTM sürümündeki uzunluğu iskele kurulmuş kodu artık dahil `table.Column<string>(maxLength: 450, nullable: true)`.

RTM kullanılmadan önce iskele kurulmuş mevcut tüm geçişler olmayacaktır `maxLength` bağımsız değişken belirtilmedi. Bu veritabanı tarafından desteklenen en fazla uzunluk kullanılacak anlamına gelir (`nvarchar(max)` SQL Server). Bu bazı sütunları, ancak bir anahtar, yabancı anahtar parçası olan sütunlar için iyi olabilir veya dizin uzunluğu en fazla içerecek şekilde güncelleştirilmesi gerekir. Kural gereği, 450 uzunluğu en fazla kullanılan anahtarları, yabancı anahtarlar ve sütunlar dizine var. Ardından model içinde açıkça bir uzunluk yapılandırdıysanız, bu uzunluğu yerine kullanmanız gerekir.

**ASP.NET kimlik**

Bu değişiklik, ASP.NET Identity kullanan ve bir önceden oluşturulmuş projeleri etkiler-RTM proje şablonu. Veritabanını oluşturmak için kullanılan bir geçiş proje şablonu içerir. Bu geçiş, maksimum uzunluğunu belirtmek üzere düzenlenmelidir `256` şu sütunlar için.

*  **AspNetRoles**

    * Ad

    * NormalizedName

*  **AspNetUsers**

   * E-posta

   * NormalizedEmail

   * NormalizedUserName

   * UserName

Bir veritabanına ilk geçiş uygulandığında hata bu değişikliği yapmak için aşağıdaki özel duruma neden olur.

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET core: "içeri aktarmalar" project.json kaldırın.

.NET Core RC2 ile hedefleyen, eklemek için gereken `imports` project.json .NET Standard desteklemediğinden EF Core'nın bağımlılıkları bazıları için geçici bir çözüm olarak için. Bunlar artık kaldırılabilir.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> 1.0 sürümünden itibaren RTM [.NET Core SDK'sı](https://www.microsoft.com/net/download/core) artık `project.json` veya Visual Studio 2015'i kullanarak .NET Core uygulamaları geliştirmek. Öneririz [Project.json'dan csproj'a geçirilmesi geçirme](https://docs.microsoft.com/dotnet/articles/core/migration/). Visual Studio kullanıyorsanız, yükseltme öneririz [Visual Studio 2017](https://www.visualstudio.com/downloads/).

## <a name="uwp-add-binding-redirects"></a>UWP: bağlama yeniden yönlendirmeleri ekleyin

Evrensel Windows Platformu (UWP) projeleri şu hata sonuçlarında EF komutları çalıştırmak çalışılıyor:

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

UWP projesi için bağlama yeniden yönlendirmelerini el ile eklemeniz gerekir. Adlı bir dosya oluşturun `App.config` projesinde kök klasörü ve için doğru derleme sürümlerini yeniden yönlendirmeleri ekleyin.

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
