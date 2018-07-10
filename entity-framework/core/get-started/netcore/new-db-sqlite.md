---
title: Üzerinde .NET Core - yeni veritabanı - EF Core kullanmaya başlama
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: .NET Core kullanarak Entity Framework Core ile çalışmaya başlama
keywords: .NET core, Entity Framework Core, VS kodu, Visual Studio kodu, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911508"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>EF Core üzerinde .NET Core konsol uygulaması ile yeni bir veritabanı ile çalışmaya başlama

Bu kılavuzda, Entity Framework Core kullanan bir SQLite veritabanında veri erişimi gerçekleştirdiği bir .NET Core konsol uygulaması oluşturacaksınız. Modelden veritabanı oluşturmaya geçişleri kullanın. Bkz: [ASP.NET Core - yeni veritabanı](xref:core/get-started/aspnetcore/new-db) ASP.NET Core MVC kullanarak bir Visual Studio sürümü için.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) GitHub üzerinde.

## <a name="prerequisites"></a>Önkoşullar

[.NET Core SDK'sı](https://www.microsoft.com/net/core) 2.1

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Yeni bir konsol projesi oluşturun:

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a>Entity Framework Core yükleme

EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin. Bu izlenecek yolda SQLite kullanır. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

* Microsoft.EntityFrameworkCore.Sqlite ve Microsoft.EntityFrameworkCore.Design yükleyin

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Çalıştırma `dotnet restore` yeni paketlerini yükleyin.

## <a name="create-the-model"></a>Model oluşturma

Modelinizi olun bağlamını ve varlık sınıfları tanımlar.

* Yeni bir *Model.cs* dosya aşağıdaki içeriğe sahip.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

İpucu: Gerçek bir uygulamada, ayrı bir dosyada her sınıf koyun ve bağlantı dizesini yapılandırma dosyası yerleştirin. Bu öğreticide basit tutmak için her şeyi bir dosyada yer alır.

## <a name="create-the-database"></a>Veritabanı oluşturma

Bir modeli oluşturduktan sonra kullandığınız [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.

* Çalıştırma `dotnet ef migrations add InitialCreate` geçiş iskelesini ve tablo modeli için başlangıç kümesi oluşturun.
* Çalıştırma `dotnet ef database update` veritabanına yeni geçiş uygulamak için. Bu komut, veritabanı geçişleri uygulamadan önce oluşturur.

*Blogging.db** SQLite DB proje dizininde olduğu.

## <a name="use-your-model"></a>Modelinizi kullanın

* Açık *Program.cs* ve içeriğini aşağıdaki kodla değiştirin:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Uygulamayı test edin:

  `dotnet run`

  Bir blog veritabanına kaydedilir ve tüm blogları ayrıntılarını konsolda görüntülenir.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Model değiştiriliyor:

- Modelinize değişiklik yaparsanız, kullanabileceğiniz `dotnet ef migrations add` yeni iskele komut [geçiş](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) karşılık gelen şema değişikliklerini veritabanına. Gerekli iskele kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `dotnet ef database update` veritabanına değişiklikleri uygulamak için komutu.
- EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulanmış izlemek için veritabanı tablosunda.
- SQLite sınırlamaları nedeniyle tüm geçişler (şema değişiklikleri) içinde SQLite desteklemez. Bkz: [SQLite sınırlamaları](../../providers/sqlite/limitations.md). Yeni geliştirme projeleri için veritabanı bırakmadan yeni bir tane oluşturmak yerine ve modelinizi değiştiğinde migrations'ı kullanma göz önünde bulundurun.

## <a name="additional-resources"></a>Ek Kaynaklar

* [.NET core - yeni veritabanı SQLite ile](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.
* [MVC Mac veya Linux'ta ASP.NET Core'a giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Visual Studio ile MVC ASP.NET Core'a giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Visual Studio kullanarak ASP.NET Core ve Entity Framework Core ile çalışmaya başlama](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
