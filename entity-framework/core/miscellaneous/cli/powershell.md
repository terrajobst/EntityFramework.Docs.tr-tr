---
title: Paket Yöneticisi Konsolu (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.openlocfilehash: 6a3594a3535f8de30ec1898fd21cfcbe272f216b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997940"
---
<a name="ef-core-package-manager-console-tools"></a>EF Core Paket Yöneticisi konsolu araçları
=====================================
NuGet kullanarak EF Core Paket Yöneticisi Konsolu (PMC) araçları Visual Studio içinde çalıştırma [Paket Yöneticisi Konsolu][2].
Bu araçlar, .NET Framework hem de .NET Core projeleriyle çalışır.

> [!TIP]
> Visual Studio kullanmıyor musunuz? [EF Core komut satırı araçları] [ 1] platformlar arası ve bir komut istemi içinde çalıştırın.

<a name="installing-the-tools"></a>Araçları yükleme
--------------------
EF Core Paket Yöneticisi konsolu araçları Microsoft.EntityFrameworkCore.Tools NuGet paketini yükleyerek yükleyin.
İçinde aşağıdaki komutu çalıştırarak yükleyebilirsiniz [Paket Yöneticisi Konsolu][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Her şeyin düzgün çalıştığı, bu komutu çalıştırmak kullanabilirsiniz:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Başlangıç projeniz .NET Standard hedefliyorsa [desteklenen framework çapraz hedefleme] [ 3] Araçları'nı kullanmadan önce.

> [!IMPORTANT]
> Kullanıyorsanız **Evrensel Windows** veya **Xamarin**, EF kodunuzu .NET Standard Sınıf Kitaplığı'na taşıyın ve [desteklenen framework çapraz hedefleme] [ 3] Araçları'nı kullanmadan önce. Sınıf kitaplığı, başlangıç projesi olarak belirtin.

<a name="using-the-tools"></a>Araçlarını kullanma
---------------
Bir komut çağırma her iki proje kullanılan vardır:

Hedef dosyalar eklenir (veya bazı durumlarda kaldırıldı) projesidir. Hedef Proje Varsayılanları **varsayılan proje** Paket Yöneticisi Konsolu'nda seçildi, ancak kullanılarak da belirtilebilir proje parametresi.

Başlangıç projesi, proje kodunuzun yürütülürken araçları tarafından Öykünülen bir bileşendir. İçin varsayılan olarak **başlangıç projesi olarak ayarla** Çözüm Gezgini'nde. Bu, - StartupProject parametresi kullanılarak da belirtilebilir.

Ortak parametreleri:

|                           |                             |
|:--------------------------|:----------------------------|
| -Bağlam \<dizesi >        | Kullanılacak DbContext.       |
| -Project \<dizesi >        | Kullanmak için proje.         |
| -StartupProject \<dizesi > | Kullanılacak başlangıç projesi. |
| -Verbose                  | Ayrıntılı çıktıyı gösterir.        |

Bir komut hakkında Yardım bilgilerini görüntülemek için PowerShell'in kullanın `Get-Help` komutu.

> [!TIP]
> Bağlam, proje ve StartupProject parametreleri sekme genişletmeyi destekler.

> [!TIP]
> Ayarlama **env:ASPNETCORE_ENVIRONMENT** ASP.NET Core ortamını belirtmek için çalıştırmadan önce.

<a name="commands"></a>Komutlar
--------

### <a name="add-migration"></a>Geçiş Ekle

Yeni bir geçiş ekler.

Parametreler:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Ad*** \<dizesi >             | Geçiş adı.                                                                                       |
| <nobr>-OutputDir \<dizesi ></nobr> | Kullanılacak dizin (ve alt ad alanı). Proje dizinine göreli yollardır. Varsayılan olarak "Geçişler". |

> [!NOTE]
> Parametrelerinde **kalın** gereklidir ve olanları içinde *italik* konumsal olan.

### <a name="drop-database"></a>Veritabanını silme

Veritabanı bırakır.

Parametreler:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Hangi veritabanı bırakılan ancak açılan yoksa gösterir. |

### <a name="get-dbcontext"></a>Get-DbContext

DbContext tür bilgilerini alır.

### <a name="remove-migration"></a>Remove-geçiş

Son geçiş kaldırır.

Parametreler:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Veritabanına uygulanmışsa, geçişi geri alın. |

### <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.

Parametreler:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Bağlantı*** \<dizesi ></nobr> | Veritabanı bağlantı dizesi.                                                           |
| ***-Provider*** \<dizesi >                | Kullanılacak sağlayıcı. (örneğin, Microsoft.EntityFrameworkCore.SqlServer)                      |
| -OutputDir \<dizesi >                     | Dosyaları yerleştirmek için dizin. Proje dizinine göreli yollardır.                      |
| -ContextDir \<dizesi >                    | DbContext dosya yerleştirmek için dizin. Proje dizinine göreli yollardır.             |
| -Bağlam \<dizesi >                       | Oluşturulacak DbContext adı.                                                           |
| -Şemaları \<String [] >                     | Şemaları için varlık türleri oluşturmak için tablo.                                              |
| -Tablolar \<String [] >                      | Varlık türleri için oluşturmak üzere tablolara.                                                         |
| -DataAnnotations                         | Öznitelikler, model (mümkünse) yapılandırmak için kullanın. Atlanırsa, yalnızca fluent API'si kullanılır. |
| -UseDatabaseNames                        | Doğrudan veritabanından tablo ve sütun adları kullanın.                                           |
| -Force                                   | Varolan dosyaların üzerine yaz.                                                                        |

### <a name="script-migration"></a>Geçiş betiği

Bir SQL betiği geçişleri oluşturur.

Parametreler:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-From* \<dizesi > | Başlangıç geçiş. Varsayılan olarak 0 (ilk veritabanı).      |
| *-Çok* \<dizesi >   | Bitiş geçiş. Son varsayılan olarak geçiş.              |
| -Bir kez etkili       | Bir veritabanı herhangi bir geçiş sırasında kullanılan bir komut dosyası oluşturur. |
| -Çıkış \<dizesi > | Yazma sonucu dosyası.                                   |

> [!TIP]
> Kime, ve çıkış parametreleri sekme genişletmeyi destekler.

### <a name="update-database"></a>Veritabanını Güncelleştir

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Geçiş* \<dizesi ></nobr> | Hedef geçiş. '0 'ise, tüm geçişler döndürülecek. Son varsayılan olarak geçiş. |

> [!TIP]
> Geçiş parametresi sekme genişletmeyi destekler.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
