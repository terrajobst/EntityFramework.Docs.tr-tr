---
title: EF Core 1,0 RC2 'den RTM 'ye yükseltme EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 779caad7883d13684b389dab7515be44bc42e1ef
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655812"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>EF Core 1,0 RC2 'den RTM 'ye yükseltme

Bu makalede RC2 paketleri ile derlenmiş bir uygulamayı 1.0.0 RTM 'ye taşımaya yönelik rehberlik sunulmaktadır.

## <a name="package-versions"></a>Paket sürümleri

Genellikle bir uygulamaya yüklediğiniz en üst düzey paketlerin adları RC2 ve RTM arasında değişmemelidir.

**Yüklü paketleri RTM sürümlerine yükseltmeniz gerekir:**

* Çalışma zamanı paketleri (örneğin, `Microsoft.EntityFrameworkCore.SqlServer`) `1.0.0-rc2-final` `1.0.0`olarak değiştirildi.

* `Microsoft.EntityFrameworkCore.Tools` paketi `1.0.0-preview1-final` `1.0.0-preview2-final`olarak değiştirildi. Araç araçlarının hala ön sürüm olduğunu unutmayın.

## <a name="existing-migrations-may-need-maxlength-added"></a>Mevcut geçişlerde maxLength 'in eklenmesi gerekebilir

RC2 'de, bir geçişte sütun tanımı `table.Column<string>(nullable: true)` gibi, sütunun uzunluğu ise geçişin arkasındaki kodda depoladığımız bazı meta verilerde aranmıştı. RTM 'de, uzunluk artık scafkatli kod `table.Column<string>(maxLength: 450, nullable: true)`dahil edilmiştir.

RTM kullanılmadan önce iskele alınan mevcut geçişler, `maxLength` bağımsız değişkenine sahip olmaz. Bu, veritabanı tarafından desteklenen uzunluk üst sınırının kullanılacağı anlamına gelir (SQL Server`nvarchar(max)`). Bu, bazı sütunlarda iyi olabilir, ancak bir anahtarın, yabancı anahtarın veya dizinin parçası olan sütunların en fazla uzunluk içerecek şekilde güncellenmesi gerekir. Kurala göre, 450, anahtar, yabancı anahtar ve dizinli sütunlar için kullanılan en büyük uzunluktadır. Modelde bir uzunluğu açık olarak yapılandırdıysanız, bunun yerine bu uzunluğu kullanmanız gerekir.

### <a name="aspnet-identity"></a>ASP.NET Kimlik

Bu değişiklik ASP.NET Identity kullanan projeleri etkiler ve bir pre-RTM proje şablonundan oluşturulmuştur. Proje şablonu, veritabanını oluşturmak için kullanılan bir geçiş içerir. Bu geçiş, aşağıdaki sütunlar için en fazla `256` uzunluğunu belirtmek üzere düzenlenmelidir.

* **AspNetRoles**
  * Name
  * NormalizedName
* **AspNetUsers**
  * E-posta
  * NormalizedEmail
  * NormalizedUserName
  * UserName

Bir veritabanına ilk geçiş uygulandığında, bu değişikliğin başarısız olması aşağıdaki özel duruma neden olur.

``` Console
System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.
```

## <a name="net-core-remove-imports-in-projectjson"></a>.NET Core: Project. JSON içinde "Imports" öğesini kaldır

RC2 ile .NET Core 'u hedefliyorsanız, bazı EF Core bağımlılıklarının .NET Standard desteklememesine yönelik geçici bir geçici çözüm olarak Project. json ' a `imports` eklemeniz gerekir. Bunlar artık kaldırılabilirler.

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
> Sürüm 1,0 RTM itibariyle, [.NET Core SDK](https://www.microsoft.com/net/download/core) artık `project.json` desteklememektedir veya Visual Studio 2015 kullanarak .NET Core uygulamaları geliştiriyor. [Project. JSON 'dan csproj 'a geçiş](https://docs.microsoft.com/dotnet/articles/core/migration/)yapmanızı öneririz. Visual Studio kullanıyorsanız, [Visual studio 2017](https://www.visualstudio.com/downloads/)sürümüne yükseltmenizi öneririz.

## <a name="uwp-add-binding-redirects"></a>UWP: bağlama yeniden yönlendirmeleri ekleme

Evrensel Windows Platformu (UWP) projelerinde EF komutlarının çalıştırılmaya çalışılması aşağıdaki hatayla sonuçlanır:

```output
System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.
```

UWP projesine el ile bağlama yeniden yönlendirmeleri eklemeniz gerekir. Proje kök klasöründe `App.config` adlı bir dosya oluşturun ve doğru derleme sürümlerine yeniden yönlendirmeler ekleyin.

```xml
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
