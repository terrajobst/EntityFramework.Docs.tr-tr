---
title: .NET Framework - mevcut veritabanı - EF Core üzerinde çalışmaya başlama
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: b9e079f88dd35016407b19bb627f8bd46edb3d4c
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447163"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>.NET Framework ile varolan bir veritabanını EF Core ile çalışmaya başlama

Bu öğreticide, Entity Framework kullanarak bir Microsoft SQL Server veritabanında temel veri erişimi gerçekleştirdiği bir konsol uygulaması oluşturun. Bir Entity Framework modelini tarafından ters mühendislik mevcut bir veritabanı oluşturun.

[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>Önkoşullar

* [Visual Studio 2017 sürüm 15.7 veya üzeri](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a>Günlük veritabanı oluşturma

Bu öğreticide bir **blog** LocalDb örneğinde var olan veritabanının veritabanı. Zaten oluşturduysanız **blog** veritabanı başka bir öğreticinin bir parçası olarak, bu adımı atlayın.

* Visual Studio'yu Aç

* **Araçlar > veritabanına bağlan...**

* Seçin **Microsoft SQL Server** tıklatıp **devam et**

* Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**

* Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**

* Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**

* Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**

* Sorgu Düzenleyicisi'ne aşağıdaki betiği kopyalayın

* Sorgu düzenleyicisini sağ tıklayıp **Yürüt**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Açık Visual Studio 2017

* **Dosya > Yeni > Proje...**

* Sol menüden **yüklü > Visual C# > Windows Masaüstü**

* Seçin **konsol uygulaması (.NET Framework)** proje şablonu

* Emin olun projenizin hedeflediği **.NET Framework 4.6.1** veya üzeri

* Projeyi adlandırın *ConsoleApp.ExistingDb* tıklatıp **Tamam**

## <a name="install-entity-framework"></a>Entity Framework'ü yükleme

EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin. Bu öğreticide, SQL Server kullanır. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.

Sonraki adımda, veritabanı tersine için bazı Entity Framework araçları kullanın. Bu nedenle de araçları paketini yükleyin.

* `Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.

## <a name="reverse-engineer-the-model"></a>Ters mühendislik modeli

Artık mevcut bir veritabanını temel alan EF modeli oluşturma zamanı geldi.

* **Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**

* Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> Varlıklar için ekleyerek oluşturmak üzere tablolara belirtebilirsiniz `-Tables` bağımsız değişkeni için yukarıdaki komutu. Örneğin, `-Tables Blog,Post`.

Oluşturulan varlık sınıfları ters mühendislik süreci (`Blog` ve `Post`) ve türetilmiş bir içerik (`BloggingContext`) var olan veritabanı şemasını temel alan.

Varlık sınıfları, sorgulama ve kaydetme verilerini temsil eden basit C# nesneleridir. İşte `Blog` ve `Post` varlık sınıfları:

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> Yavaş yükleniyor etkinleştirmek için Gezinti özellikleri yapabilirsiniz `virtual` (Blog.Post ve Post.Blog).

İçerik veritabanı ile bir oturumu temsil eder. Bu, sorgulama ve varlık sınıflarının örneklerini kaydetmek için kullanabileceğiniz yöntemleri vardır.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>Kullanım modeli

Şimdi, veri erişimi gerçekleştirdiği modeli kullanabilirsiniz.

* Açık *Program.cs*

* Dosyanın içeriğini aşağıdaki kodla değiştirin.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* Hata ayıklama > hata ayıklama olmadan Başlat

  Bir blog veritabanına kaydedilir ve daha sonra tüm blogları ayrıntılarını konsola yazdırılır görürsünüz.

  ![görüntü](_static/output-existing-db.png)

## <a name="next-steps"></a>Sonraki adımlar

Bağlam ve varlık sınıfları iskelesinin nasıl kurulacağını hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
* [Entity Framework Core başvuru - .NET CLI araçları](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Entity Framework Core araçları başvurusu - Paket Yöneticisi Konsolu](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)