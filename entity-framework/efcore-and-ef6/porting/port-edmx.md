---
title: EF6'dan EF Core'a taşıma - EDMX tabanlı bir modeli taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997417"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>EF6 EDMX tabanlı bir modeli EF core'a taşıma

EF Core modelleri için EDMX dosya biçimini desteklemiyor. Bu modeller bağlantı noktası için en iyi seçenektir veritabanından uygulamanız için yeni bir kod tabanlı modeli oluşturmak için.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet paketi yüklemesi

Yükleme `Microsoft.EntityFrameworkCore.Tools` NuGet paketi.

## <a name="regenerate-the-model"></a>Modeli yeniden oluşturun

Ayrıca, varolan bir veritabanını temel alan bir model oluşturmak için artık ters mühendislik işlevlerini kullanabilirsiniz.

Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın (araçları –> NuGet Paket Yöneticisi Paket Yöneticisi konsolu –>). Bkz: [Paket Yöneticisi Konsolu (Visual Studio)](../../core/miscellaneous/cli/powershell.md) tablolar vb. kümesini iskele komut seçenekleri için.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Örneğin, bir model, SQL Server LocalDB örneğini blog veritabanından iskele komutu aşağıdadır.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>EF6 modeli kaldırma

Artık uygulamanızı EF6 modeli kaldırmak isteyeceksiniz.

EF Core ve EF6 kullanılan yan yana aynı uygulamada olması gibi yüklü EF6 NuGet paketini (EntityFramework) bırakmak uygundur. Uygulamanızın tüm alanlarda EF6'ı kullanmayı planlayan değildir, ancak sonra paket kaldırılıyor dikkat etmeniz gereken kod parçaları derleme hatalarını vermek yardımcı olur.

## <a name="update-your-code"></a>Kodunuzu güncelleştirin

Bu noktada, birkaç adresleme derleme hataları ve EF6 ve EF Core arasındaki davranış değişiklikleri, etkileyip etkilemeyeceğini görmek için gözden geçirme kod değil.

## <a name="test-the-port"></a>Bağlantı testi

Yalnızca uygulamanızı derleyen çünkü başarıyla EF Core için unity'nin anlamına gelmez. Tüm alanları davranışı değişikliklerin hiçbiri olumsuz uygulamanızı etkilemiş emin olmak için uygulamanızı test etmek gerekir.

> [!TIP]
> Bkz: [var olan bir veritabanı ile ASP.NET Core üzerinde EF Core ile çalışmaya başlama](xref:core/get-started/aspnetcore/existing-db) var olan bir veritabanı ile çalışma konusunda ek bir başvuru 
