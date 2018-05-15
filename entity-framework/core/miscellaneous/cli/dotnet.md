---
title: .NET core CLI - EF çekirdek
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d053d53bd50d2e7d16223c5b4e4009c9bb2298bb
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
<a name="ef-core-net-command-line-tools"></a>EF çekirdek .NET komut satırı araçları
===============================
Platformlar arası uzantısı Entity Framework Çekirdek .NET komut satırı araçları olan **dotnet** parçasıdır komutu, [.NET Core SDK][2].

> [!TIP]
> Öneririz Visual Studio kullanıyorsanız, [PMC Araçları] [ 1] yerine beri sağladıkları daha tümleşik bir deneyim.

<a name="installing-the-tools"></a>Araçları yükleme
--------------------
> [!NOTE]
> .NET Core SDK'sı sürüm 2.1.300 ve daha yeni içerir **dotnet ef** EF çekirdek 2.0 ve sonraki sürümleri ile uyumlu olan komutları. Bu nedenle .NET Core SDK'sı ve EF çekirdeği çalışma zamanı en güncel sürümlerini kullanıyorsanız, herhangi bir yükleme gereklidir ve bu bölümde rest yoksayabilirsiniz.
>
> Diğer taraftan, **dotnet ef** aracı .NET Core SDK sürüm 2.1.300 içerdiği ve daha yeni sürümü 1.0 ve 1.1 EF çekirdek sürümü ile uyumlu değil. .NET Core SDK 2.1.300 sahip bir bilgisayarda bu sürümlerde EF çekirdek kullanan bir proje ile çalışabilirsiniz ya da daha yeni yüklü önce sürüm 2.1.200 de yüklemeniz gerekir ya da SDK'ın eski ve uygulamayı değiştirerek, eski sürümünün kullanacak şekilde yapılandırın,  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) dosya. Bu dosya, normalde çözüm dizini (bir proje üzerinde) bulunmaktadır. Ardından installlation talimatıyla devam edebilirsiniz.

.NET Core SDK önceki sürümleri için aşağıdaki adımları kullanarak EF çekirdek .NET komut satırı araçları yükleyebilirsiniz:

1. Proje dosyasını düzenleyin ve Microsoft.EntityFrameworkCore.Tools.DotNet DotNetCliToolReference öğesi (aşağıya bakın) olarak Ekle
2. Aşağıdaki komutları çalıştırın:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

Elde edilen proje aşağıdakine benzer görünmelidir:

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
> Bir paket başvurusuyla `PrivateAssets="All"` değil sunulan genellikle yalnızca geliştirme sırasında kullanılan paketler için özellikle yararlıdır bu proje başvurusu projelerine anlamına gelir.

Her şeyi doğru kaldırdıysanız, başarılı bir şekilde bir komut isteminde aşağıdaki komutu çalıştırın olmalıdır.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Araçlarını kullanma
---------------
Bir komut çağırma her iki proje ilgili vardır:

Hedef dosyalar eklendiği (veya kaldırılan bazı durumlarda) projesidir. Hedef projeyi projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir <nobr> **--proje** </nobr> seçeneği.

Başlangıç projesi projenizin kodu çalıştırırken araçları tarafından Öykünülen adrestir. Ayrıca projenin geçerli dizinin varsayılan olarak, ancak kullanılarak değiştirilebilir **--başlangıç projesi** seçeneği.

> [!NOTE]
> Örneğin, web uygulamanızın EF çekirdek farklı projede yüklü olan veritabanını güncelleştirme şuna benzer: `dotnet ef database update --project {project-path}` (dizininizden web uygulaması)

Ortak seçenekleri:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | --json                           | JSON çıktısını gösterir.           |
| -c | --bağlam \<DBCONTEXT >           | Kullanılacak DbContext.       |
| -p | --Proje \<Proje >             | Kullanmak için proje.         |
| -s | --başlangıç projesi \<Proje >     | Kullanılacak başlangıç projesi. |
|    | --framework \<FRAMEWORK >         | Hedef çerçevesi.       |
|    | --configuration \<yapılandırma > | Kullanılacak yapılandırma.   |
|    | --çalışma zamanı \<TANIMLAYICISI >          | Çalışma zamanı.         |
| -h | --Yardım                           | Yardım bilgilerini gösterir.      |
| -v | --ayrıntılı                        | Ayrıntılı çıktıyı göster.        |
|    | --renk yok                       | Çıktı renklendirme yok.      |
|    | --önek çıktı                  | Düzeyiyle çıktı öneki.   |


> [!TIP]
> ASP.NET Core ortam belirtmek için ayarlanmış **ASPNETCORE_ENVIRONMENT** çalıştırmadan önce ortam değişkeni.

<a name="commands"></a>Komutlar
--------

### <a name="dotnet-ef-database-drop"></a>DotNet ef veritabanı bırakma

Veritabanı bırakır.

Seçenekler:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | --zorla   | Onaylayın yok.                                           |
|    | --çalıştırma | Hangi veritabanı olduğundan bırakılamaz, ancak bırakma yok gösterir. |

### <a name="dotnet-ef-database-update"></a>DotNet ef veritabanı güncelleştirme

Belirtilen geçiş için veritabanını güncelleştirir.

Bağımsız değişkenler:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| \<GEÇİŞ &GT; | Hedef geçiş. 0 ise, tüm geçişler geri alınacak. Son geçiş varsayılan olarak ayarlanır. |

### <a name="dotnet-ef-dbcontext-info"></a>DotNet ef dbcontext bilgisi

DbContext türü hakkındaki bilgileri alır.

### <a name="dotnet-ef-dbcontext-list"></a>DotNet ef dbcontext listesi

Kullanılabilir DbContext türleri listelenmektedir.

### <a name="dotnet-ef-dbcontext-scaffold"></a>DotNet ef dbcontext iskele

Bir veritabanı için bir DbContext ve varlık türleri iskelesini kurar.

Bağımsız değişkenler:

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| \<BAĞLANTI &GT; | Veritabanı bağlantı dizesi.                              |
| \<SAĞLAYICI &GT;   | Kullanılacak sağlayıcısı. (Örn. Microsoft.EntityFrameworkCore.SqlServer) |

Seçenekler:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | --Veri ek açıklamaları                      | Öznitelikler, model (mümkün olduğunda) yapılandırmak için kullanın. Atlanırsa, yalnızca fluent API kullanılır. |
| -c              | --bağlam \<adı >                       | DbContext adı.                                                                       |
|                 | --bağlam dir \<yolu >                   | DbContext dosyasını yerleştirmek için dizin. Proje dizininin göreli yollardır.             |
| -f              | --zorla                                 | Var olan dosyaların üzerine yazar.                                                                        |
| -o              | --Çıkış dir \<yolu >                    | Dosyaları yerleştirmek için dizin. Proje dizininin göreli yollardır.                      |
|                 | <nobr>--şema \<SCHEMA_NAME >...</nobr> | Varlık türleri oluşturmak için tabloları şemalar.                                              |
| -t              | --Tablo \<TABLE_NAME >...                | Varlık türleri için oluşturmak üzere tablolara.                                                         |
|                 | --adları veritabanı kullan                    | Doğrudan veritabanından tablo ve sütun adları kullanın.                                           |

### <a name="dotnet-ef-migrations-add"></a>DotNet ef geçişler ekleme

Yeni bir geçiş ekler.

Bağımsız değişkenler:

|         |                            |
|:--------|:---------------------------|
| \<ADI &GT; | Geçiş adı. |

Seçenekler:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr>--Çıkış dir \<yolu ></nobr> | Kullanılacak dizin (ve alt ad alanı). Proje dizininin göreli yollardır. Varsayılan olarak "Geçiş". |

### <a name="dotnet-ef-migrations-list"></a>DotNet ef geçişler listesi

Kullanılabilir geçişler listeler.

### <a name="dotnet-ef-migrations-remove"></a>DotNet ef geçişler Kaldır

Son geçiş kaldırır.

Seçenekler:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | --zorla | Veritabanına uyguladıysanız geçişi geri alın. |

### <a name="dotnet-ef-migrations-script"></a>DotNet ef geçişler komut dosyası

Bir SQL betiği geçişleri oluşturur.

Bağımsız değişkenler:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| \<GELEN &GT; | Başlangıç geçiş. Varsayılan ayar: 0 (ilk veritabanı). |
| \<İÇİN &GT;   | Bitiş geçiş. Son geçiş varsayılan olarak ayarlanır.         |

Seçenekler:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | --Çıkış \<dosyası > | Sonucu yazmak için dosya.                                   |
| -i | --ıdempotent     | Bir veritabanında hiçbir geçiş sırasında kullanılan bir komut dosyası oluşturur. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
