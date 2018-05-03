---
title: Paket Yöneticisi Konsolu (Visual Studio) - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
<a name="ef-core-package-manager-console-tools"></a>EF çekirdek Paket Yöneticisi konsolu araçları
=====================================
NuGet kullanarak Visual Studio içinde EF çekirdek Paket Yöneticisi Konsolu (PMC) araçları çalıştırmak [Paket Yöneticisi Konsolu][2].
Bu araçlar, .NET Framework ve .NET Core projeleri ile çalışır.

> [!TIP]
> Visual Studio kullanmıyor musunuz? [EF çekirdek komut satırı araçları] [ 1] platformlar arası ve bir komut istemi içinde çalıştırma.

<a name="installing-the-tools"></a>Araçları yükleme
--------------------
EF çekirdek Paket Yöneticisi konsolu araçları Microsoft.EntityFrameworkCore.Tools NuGet paketini yükleyerek yükleyin.
İçinde aşağıdaki komutu çalıştırarak yükleyebilirsiniz [Paket Yöneticisi Konsolu][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Her şeyin doğru şekilde çalışan, bu komutu çalıştırmak yapabiliyor olmanız gerekir:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Başlangıç projeniz .NET standart hedefliyorsa [arası hedef desteklenen çerçevelerden] [ 3] araçları kullanmadan önce.

> [!IMPORTANT]
> Kullanıyorsanız, **Evrensel Windows** veya **Xamarin**, .NET standart bir sınıf kitaplığı'na EF kodunuzu taşıyın ve [arası hedef desteklenen çerçevelerden] [ 3] araçları kullanmadan önce. Sınıf kitaplığı başlangıç projesi olarak belirtin.

<a name="using-the-tools"></a>Araçlarını kullanma
---------------
Bir komut çağırma her iki proje ilgili vardır:

Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir. Hedef Proje Varsayılanları için **varsayılan proje** Paket Yöneticisi Konsolu'nda seçili ancak kullanılarak da belirtilebilir proje parametresi.

Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir. Bir varsayılan olarak **başlangıç projesi olarak ayarla** Çözüm Gezgini'nde. Bu, - StartupProject parametresi kullanılarak da belirtilebilir.

Ortak Parametreler:

|                           |                             |
|:--------------------------|:----------------------------|
| -Context \<dize >        | Kullanılacak DbContext.       |
| -Proje \<dize >        | Kullanmak için proje.         |
| -StartupProject \<dize > | Kullanılacak başlangıç projesi. |
| -Verbose                  | Ayrıntılı çıktıyı göster.        |

Bir komutla ilgili Yardım bilgilerini göstermek için PowerShell'ın kullanın `Get-Help` komutu.

> [!TIP]
> Bağlam, proje ve StartupProject parametreleri sekmesi genişletme destekler.

> [!TIP]
> Ayarlama **env:ASPNETCORE_ENVIRONMENT** ASP.NET Core ortam belirtmek için çalıştırmadan önce.

<a name="commands"></a>Komutlar
--------

### <a name="add-migration"></a>Add-Migration

Yeni bir geçiş ekler.

Parametreler:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Ad*** \<dize >             | Geçiş adı.                                                                                       |
| <nobr>-OutputDir \<dize ></nobr> | Kullanılacak dizin (ve alt ad alanı). Proje dizininin göreli yollardır. Varsayılan olarak "Geçiş". |

> [!NOTE]
> Parametrelerde **kalın** gereklidir ve olanları içinde *italik* konumsal şunlardır.

### <a name="drop-database"></a>Veritabanını silme

Veritabanı bırakır.

Parametreler:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Hangi veritabanı olduğundan bırakılamaz, ancak bırakma yok gösterir. |

### <a name="get-dbcontext"></a>Get-DbContext

DbContext türü hakkındaki bilgileri alır.

### <a name="remove-migration"></a>Remove-geçiş

Son geçiş kaldırır.

Parametreler:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Veritabanına uyguladıysanız geçişi geri alın. |

### <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.

Parametreler:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Bağlantı*** \<dize ></nobr> | Veritabanı bağlantı dizesi.                                                           |
| ***-Sağlayıcısı*** \<dize >                | Kullanılacak sağlayıcısı. (Örn. Microsoft.EntityFrameworkCore.SqlServer)                              |
| -OutputDir \<dize >                     | Dosyaları yerleştirmek için dizin. Proje dizininin göreli yollardır.                      |
| -ContextDir \<dize >                    | DbContext dosyasını yerleştirmek için dizin. Proje dizininin göreli yollardır.             |
| -Context \<dize >                       | Oluşturulacak DbContext adı.                                                           |
| -Şemaları \<String [] >                     | Varlık türleri oluşturmak için tabloları şemalar.                                              |
| -Tabloları \<String [] >                      | Varlık türleri için oluşturmak üzere tablolara.                                                         |
| -DataAnnotations                         | Öznitelikler, model (mümkün olduğunda) yapılandırmak için kullanın. Atlanırsa, yalnızca fluent API kullanılır. |
| -UseDatabaseNames                        | Doğrudan veritabanından tablo ve sütun adları kullanın.                                           |
| -Force                                   | Var olan dosyaların üzerine yazar.                                                                        |

### <a name="script-migration"></a>Komut dosyası geçişi

Bir SQL betiği geçişleri oluşturur.

Parametreler:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-From* \<dize > | Başlangıç geçiş. Varsayılan ayar: 0 (ilk veritabanı).      |
| *-Çok* \<dize >   | Bitiş geçiş. Son geçiş varsayılan olarak ayarlanır.              |
| -Idempotent       | Bir veritabanında hiçbir geçiş sırasında kullanılan bir komut dosyası oluşturur. |
| -Çıktı \<dize > | Sonucu yazmak için dosya.                                   |

> [!TIP]
> Kime, ve çıkış parametreleri sekmesi genişletme desteği.

### <a name="update-database"></a>Update-Database

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Geçiş* \<dize ></nobr> | Hedef geçiş. '0 'ise, tüm geçişler geri alınacak. Son geçiş varsayılan olarak ayarlanır. |

> [!TIP]
> Geçiş parametre sekmesini genişletme destekler.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
