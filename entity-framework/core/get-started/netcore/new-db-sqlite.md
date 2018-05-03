---
title: .NET Core - yeni veritabanı - EF Çekirdeğinde Başlarken
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Entity Framework Çekirdek kullanarak .NET Core'u kullanmaya başlama
keywords: .NET core, Entity Framework Çekirdek, VS kodu, Visual Studio kodu, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: fcace3c0f259b1a456d9ca1086e6a1549c070d57
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>.NET Core konsol uygulaması EF Çekirdeğinde ile yeni bir veritabanı ile çalışmaya başlama

Bu kılavuzda, Entity Framework Çekirdek kullanarak bir SQLite veritabanı karşı temel veri erişimi gerçekleştirdiği bir .NET Core konsol uygulaması oluşturacaksınız. Geçişler, modelden veritabanı oluşturmak için kullanır. Bkz: [ASP.NET Core - yeni veritabanı](xref:core/get-started/aspnetcore/new-db) ASP.NET Core MVC kullanarak bir Visual Studio sürümü.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) github'da.

## <a name="prerequisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gerekir:
* .NET Core destekleyen bir işletim sistemi.
* [.NET Core SDK](https://www.microsoft.com/net/core) 2.0 (yönergeleri çok az değişiklik yapılması açısından önceki bir sürümüyle bir uygulama oluşturmak için kullanılır ancak kullanılabilir).

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Yeni bir `ConsoleApp.SQLite` projenizi ve kullanmak için klasör `dotnet` .NET Core uygulamayla doldurmak için komutu.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Entity Framework Çekirdek yükleyin

EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin. Bu kılavuzda SQLite kullanılır. Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).

* Microsoft.EntityFrameworkCore.Sqlite ve Microsoft.EntityFrameworkCore.Design yükleyin

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* El ile düzenleme `ConsoleApp.SQLite.csproj` Microsoft.EntityFrameworkCore.Tools.DotNet için bir DotNetCliToolReference eklemek için:

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

`ConsoleApp.SQLite.csproj` Şimdi aşağıdakileri içermelidir:

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 Not: Yukarıdaki kullanılan sürüm numaralarını yayımlama aynı anda doğru.

*  Çalıştırma `dotnet restore` yeni paketleri yüklemek için.

## <a name="create-the-model"></a>Model oluşturma

Modelinizi olun bağlamını ve varlık sınıfları tanımlar.

* Yeni bir *Model.cs* dosya aşağıdaki içeriğe sahip.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

İpucu: Gerçek bir uygulamada, her sınıf ayrı bir dosyaya koymak ve bağlantı dizesini yapılandırma dosyasındaki yerleştirin. Öğretici basit tutmak için biz her şeyi bir dosyaya koymak.

## <a name="create-the-database"></a>Veritabanı oluşturma

Bir modeli eğittikten sonra kullanabileceğiniz [geçişler](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) bir veritabanı oluşturmak için.

* Çalıştırma `dotnet ef migrations add InitialCreate` bir geçişin iskelesini kurun ve tablo modeli için ilk kümesini oluşturmak için.
* Çalıştırma `dotnet ef database update` veritabanına yeni geçiş uygulanacak. Bu komut, geçişler uygulamadan önce veritabanı oluşturur.

> [!NOTE]  
> Göreli yollar SQLite ile kullanırken, uygulamanın ana derleme göreli yol olacaktır. Bu örnekte, ana ikili dosyadır `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, SQLite veritabanı olacak şekilde `bin/Debug/netcoreapp2.0/blogging.db`.

## <a name="use-your-model"></a>Modelinizi kullanın

* Açık *Program.cs* ve içeriğini aşağıdaki kodla değiştirin:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Uygulamayı test etme:

  `dotnet run`

  Bir blog veritabanına kaydedilir ve tüm Web günlüklerini ayrıntılarını konsolunda görüntülenir.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Model değiştirme:

- Modelinize değişiklik yaparsanız, kullanabileceğiniz `dotnet ef migrations add` yeni iskele komutu [geçiş](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) karşılık gelen şema değişikliklerini veritabanına. Gerekli kurulmuş kod iade (ve gerekli değişiklikleri yaptıktan sonra), kullanabileceğiniz `dotnet ef database update` veritabanına değişiklikleri uygulamak için komutu.
- EF kullanan bir `__EFMigrationsHistory` hangi geçişleri veritabanına zaten uygulandı izlemek için veritabanı tablosunda.
- SQLite içinde SQLite sınırlamaları nedeniyle tüm geçişler (şema değişiklikleri) desteklemez. Bkz: [SQLite sınırlamalar](../../providers/sqlite/limitations.md). Yeni geliştirme projeleri için veritabanını silmek ve yeni bir tane oluşturmak yerine modelinizi değiştiğinde geçişleri kullanmaya göz önünde bulundurun.

## <a name="additional-resources"></a>Ek Kaynaklar

* [.NET core - yeni veritabanı SQLite ile](xref:core/get-started/netcore/new-db-sqlite) -platformlar arası konsol EF öğretici.
* [ASP.NET Core Mac veya Linux MVC giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [ASP.NET Core Visual Studio ile MVC giriş](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Visual Studio kullanarak ASP.NET Core ve Entity Framework Core ile çalışmaya başlama](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
