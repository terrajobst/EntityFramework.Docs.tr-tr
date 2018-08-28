---
title: Üzerinde .NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
description: .NET Core kullanarak Entity Framework Core ile çalışmaya başlama
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993698"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>EF Core üzerinde .NET Core konsol uygulaması ile yeni bir veritabanı ile çalışmaya başlama

Bu öğreticide, Entity Framework Core kullanan bir SQLite veritabanında veri erişimi gerçekleştirdiği bir .NET Core konsol uygulaması oluşturacaksınız. Modelden veritabanı oluşturmaya geçişleri kullanın. Bkz: [ASP.NET Core - yeni veritabanı](xref:core/get-started/aspnetcore/new-db) ASP.NET Core MVC kullanarak bir Visual Studio sürümü için.

Bu makaledeki örnek Github'da görüntüleyin] (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Önkoşullar

* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Yeni bir konsol projesi oluşturun:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a>Entity Framework Core yükleme

EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin. Bu izlenecek yolda SQLite kullanır. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

* Microsoft.EntityFrameworkCore.Sqlite ve Microsoft.EntityFrameworkCore.Design yükleyin

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* Çalıştırma `dotnet restore` yeni paketlerini yükleyin.

## <a name="create-the-model"></a>Model oluşturma

Modelinizi olun bağlamını ve varlık sınıfları tanımlar.

* Yeni bir *Model.cs* dosya aşağıdaki içeriğe sahip.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

İpucu: Gerçek bir uygulamada, ayrı bir dosyada her sınıf koyun ve bağlantı dizesini bir yapılandırma dosyası veya ortam değişkeninde yerleştirin. Bu öğreticide basit tutmak için her şeyi bir dosyada yer alır.

## <a name="create-the-database"></a>Veritabanı oluşturma

Bir modeli oluşturduktan sonra kullandığınız [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.

* Çalıştırma `dotnet ef migrations add InitialCreate` geçiş iskelesini ve tablo modeli için başlangıç kümesi oluşturun.
* Çalıştırma `dotnet ef database update` veritabanına yeni geçiş uygulamak için. Bu komut, veritabanı geçişleri uygulamadan önce oluşturur.

*Blogging.db** SQLite DB proje dizininde olduğu.

## <a name="use-the-model"></a>Kullanım modeli

* Açık *Program.cs* ve içeriğini aşağıdaki kodla değiştirin:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Uygulamayı test edin:

  `dotnet run`

  Bir blog veritabanına kaydedilir ve tüm blogları ayrıntılarını konsolda görüntülenir.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Model değiştiriliyor:

- Modele değişiklik yaparsanız, kullanabileceğiniz `dotnet ef migrations add` yeni iskele komut [geçiş](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations). Gerekli iskele kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `dotnet ef database update` şema uygulamak için komut, veritabanına değiştirir.
- EF Core kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.
- SQLite veritabanı altyapısı, çoğu ilişkisel veritabanı tarafından desteklenen belirli şema değişiklikleri desteklemez. Örneğin, `DropColumn` işlemi desteklenmiyor. EF Core geçişleri bu işlemler için kod oluşturur. Ancak, bunları bir veritabanı için geçerli veya bir komut dosyası oluşturmayı denerseniz, EF Core özel durum oluşturur. Bkz: [SQLite sınırlamaları](../../providers/sqlite/limitations.md). Yeni geliştirme projeleri için veritabanı bırakmadan yeni bir tane oluşturmak yerine ve model değiştiğinde migrations'ı kullanma göz önünde bulundurun.

## <a name="additional-resources"></a>Ek Kaynaklar

* [MVC Mac veya Linux'ta ASP.NET Core'a giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Visual Studio ile MVC ASP.NET Core'a giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Visual Studio kullanarak ASP.NET Core ve Entity Framework Core ile çalışmaya başlama](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
