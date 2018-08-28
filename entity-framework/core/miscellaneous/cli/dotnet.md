---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995303"
---
<a name="ef-core-net-command-line-tools"></a>EF Core .NET komut satırı araçları
===============================
Entity Framework Core ve .NET komut satırı araçları, platformlar arası bir uzantısı olan **dotnet** parçası olan komut, [.NET Core SDK'sı][2].

> [!TIP]
> Öneririz Visual Studio kullanıyorsanız, [PMC Araçları] [ 1] yerine beri sağladıkları daha tümleşik bir deneyim.

<a name="installing-the-tools"></a>Araçları yükleme
--------------------
> [!NOTE]
> .NET Core SDK'sı sürüm 2.1.300 ve yeni içerir **dotnet ef** EF Core 2.0 ve sonraki sürümler ile uyumlu olan komutları. Bu nedenle .NET Core SDK'sını ve EF Core çalışma zamanı son sürümlerini kullanıyorsanız, herhangi bir yükleme gereklidir ve bu bölümün geri kalanında yoksayabilirsiniz.
>
> Öte yandan, **dotnet ef** aracı .NET Core SDK'sı sürüm 2.1.300 içerdiği ve yeni EF Core 1.0 ve 1.1 sürümüyle uyumlu değil. .NET Core SDK'sı 2.1.300 olan bir bilgisayarda bu sürümlerde EF Core kullanan bir proje ile çalışabilir veya üzerinin yüklü önce sürüm 2.1.200 da yüklemeniz gerekir ya da SDK'ın eski ve değiştirerek, eski sürümünün kullanmak için uygulamayı yapılandırma,  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) dosya. Normalde bu dosya çözüm dizini (bir proje üzerinde) dahildir. Ardından installlation talimatıyla geçebilirsiniz.

.NET Core SDK'sının önceki sürümleri için aşağıdaki adımları kullanarak EF Core .NET komut satırı araçları yükleyebilirsiniz:

1. Proje dosyasını düzenleyin ve Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference öğesi (aşağıya bakın) olarak Ekle
2. Aşağıdaki komutları çalıştırın:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

Elde edilen proje şunun gibi görünmelidir:

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> Bir paket başvurusu ile `PrivateAssets="All"` değilse ifşa genellikle yalnızca geliştirme sırasında kullanılan paketler için özellikle yararlıdır bu projeye başvuran projeler için anlamına gelir.

Her şeyi doğru kaldırdıysanız, başarıyla bir komut isteminde aşağıdaki komutu çalıştırabilirsiniz.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Araçlarını kullanma
---------------
Bir komut çağırma her iki proje kullanılan vardır:

Hedef dosyalar eklenir (veya bazı durumlarda kaldırıldı) projesidir. Hedef proje geçerli dizinde proje için varsayılan olarak, ancak kullanılarak değiştirilebilir <nobr> **--proje** </nobr> seçeneği.

Başlangıç projesi, proje kodunuzun yürütülürken araçları tarafından Öykünülen bir bileşendir. Ayrıca geçerli dizinde proje için varsayılan olarak, ancak kullanılarak değiştirilebilir **--başlangıç projesi** seçeneği.

> [!NOTE]
> Örneğin, web uygulamanızın yüklü farklı bir projede EF Core sahip veritabanını güncelleme şuna benzer: `dotnet ef database update --project {project-path}` (dizininizden web uygulaması)

Genel Seçenekler:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | --json                           | JSON çıktıyı gösterir.           |
| -c | --bağlam \<DBCONTEXT >           | Kullanılacak DbContext.       |
| -p | --Proje \<Proje >             | Kullanmak için proje.         |
| -s | --başlangıç projesi \<Proje >     | Kullanılacak başlangıç projesi. |
|    | --framework \<FRAMEWORK >         | Hedef Framework'ü.       |
|    | --configuration \<yapılandırma > | Kullanılacak yapılandırma.   |
|    | --çalışma zamanı \<TANIMLAYICI >          | Kullanmak için çalışma zamanı.         |
| -h | --Yardım                           | Yardım bilgilerini gösterir.      |
| -v | --verbose                        | Ayrıntılı çıktıyı gösterir.        |
|    | --no-rengi                       | Çıkış renklendirmeye yok.      |
|    | --Çıktı ön eki                  | Düzeyiyle çıkış öneki.   |


> [!TIP]
> ASP.NET Core ortamını belirtmek için ayarlanmış **ASPNETCORE_ENVIRONMENT** çalıştırmadan önce ortam değişkeni.

<a name="commands"></a>Komutlar
--------

### <a name="dotnet-ef-database-drop"></a>DotNet ef veritabanını bırak

Veritabanı bırakır.

Seçenekler:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | --force   | Onay isteme.                                           |
|    | --Prova amaçlı | Hangi veritabanı bırakılan ancak açılan yoksa gösterir. |

### <a name="dotnet-ef-database-update"></a>DotNet ef veritabanı güncelleştirmesi

Belirtilen geçiş veritabanını güncelleştirir.

Bağımsız değişkenleri:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<GEÇİŞ &GT; | Hedef geçiş. 0 ise, tüm geçişler döndürülecek. Son varsayılan olarak geçiş. |

### <a name="dotnet-ef-dbcontext-info"></a>DotNet ef dbcontext bilgileri

DbContext tür bilgilerini alır.

### <a name="dotnet-ef-dbcontext-list"></a>DotNet ef dbcontext listesi

Kullanılabilir DbContext türlerini listeler.

### <a name="dotnet-ef-dbcontext-scaffold"></a>DotNet ef dbcontext iskele

Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.

Bağımsız değişkenleri:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| \<BAĞLANTI &GT; | Veritabanı bağlantı dizesi.                                      |
| \<SAĞLAYICI &GT;   | Kullanılacak sağlayıcı. (örneğin, Microsoft.EntityFrameworkCore.SqlServer) |

Seçenekler:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --Veri ek açıklamaları                      | Öznitelikler, model (mümkünse) yapılandırmak için kullanın. Atlanırsa, yalnızca fluent API'si kullanılır. |
| -c              | --bağlam \<adı >                       | DbContext adı.                                                                       |
|                 | --bağlam dir \<yolu >                   | DbContext dosya yerleştirmek için dizin. Proje dizinine göreli yollardır.             |
| -f              | --force                                 | Varolan dosyaların üzerine yaz.                                                                        |
| -o              | --çıktı dizini \<yolu >                    | Dosyaları yerleştirmek için dizin. Proje dizinine göreli yollardır.                      |
|                 | <nobr>--şema \<SCHEMA_NAME >...</nobr> | Şemaları için varlık türleri oluşturmak için tablo.                                              |
| -t              | --Tablo \<TABLE_NAME >...                | Varlık türleri için oluşturmak üzere tablolara.                                                         |
|                 | --veritabanı adları kullan                    | Doğrudan veritabanından tablo ve sütun adları kullanın.                                           |

### <a name="dotnet-ef-migrations-add"></a>DotNet ef geçişleri Ekle

Yeni bir geçiş ekler.

Bağımsız değişkenleri:

|         |                            |
|:--------|:---------------------------|
| \<ADI &GT; | Geçiş adı. |

Seçenekler:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>--çıktı dizini \<yolu ></nobr> | Kullanılacak dizin (ve alt ad alanı). Proje dizinine göreli yollardır. Varsayılan olarak "Geçişler". |

### <a name="dotnet-ef-migrations-list"></a>DotNet ef geçişleri listesi

Kullanılabilir geçişleri listeler.

### <a name="dotnet-ef-migrations-remove"></a>DotNet ef geçişleri Kaldır

Son geçiş kaldırır.

Seçenekler:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | --force | Veritabanına uygulanmışsa, geçişi geri alın. |

### <a name="dotnet-ef-migrations-script"></a>DotNet ef geçişleri betiği

Bir SQL betiği geçişleri oluşturur.

Bağımsız değişkenleri:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<GELEN &GT; | Başlangıç geçiş. Varsayılan olarak 0 (ilk veritabanı). |
| \<İÇİN &GT;   | Bitiş geçiş. Son varsayılan olarak geçiş.         |

Seçenekler:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | --Çıktı \<dosyası > | Yazma sonucu dosyası.                                   |
| -i | ıdempotent--     | Bir veritabanı herhangi bir geçiş sırasında kullanılan bir komut dosyası oluşturur. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
