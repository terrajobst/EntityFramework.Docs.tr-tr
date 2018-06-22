---
title: EDMX tabanlı modeli taşıma EF çekirdek - EF6 bağlantı noktası oluşturma
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812696"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Bir EF6 EDMX tabanlı modeli EF çekirdek için bağlantı noktası oluşturma

EF çekirdek EDMX dosya biçimi modelleri için desteklemez. Bu modeller bağlantı noktası için en iyi seçenektir veritabanından uygulamanız için yeni bir kod tabanlı model oluşturulamadı.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet paketi yüklemesi

Yükleme `Microsoft.EntityFrameworkCore.Tools` NuGet paketi.

## <a name="regenerate-the-model"></a>Modeli yeniden oluştur

Ters mühendislik işlevselliği artık mevcut veritabanını temel alan bir model oluşturmak için de kullanabilirsiniz.

Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın (araçları –> NuGet Paket Yöneticisi Paket Yöneticisi konsolu –>). Bkz: [Paket Yöneticisi Konsolu (Visual Studio)](../../core/miscellaneous/cli/powershell.md) komut seçenekleri tablolar vb. kümesini iskele kurmak için.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Örneğin, bir modeli, SQL Server yerel veritabanı örneğinde blog veritabanından iskele için komutu aşağıdadır.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>EF6 modeli kaldırma

Artık EF6 modeli uygulamanızdan kaldırabilirsiniz.

EF çekirdek ve EF6 kullanılan yan yana aynı uygulamada olabildiği kadar yüklü EF6 NuGet paketi (EntityFramework) bırakmak uygundur. Uygulamanızın tüm alanlarda EF6 kullanmayı planlayan değil, ancak paket kaldırma dikkat etmeniz gereken kod parçalarını derleme hataları vermek yardımcı olur.

## <a name="update-your-code"></a>Kodunuzu güncelleştirin

Bu noktada sağlasa da adresleme derleme hataları ve EF6 EF çekirdek arasındaki davranış değişiklikleri, etkileyip etkilemeyeceğini görmek için gözden geçirme kodu değil.

## <a name="test-the-port"></a>Bağlantı testi

Yalnızca uygulamanızı derler çünkü EF çekirdek başarıyla bağlantı noktalı anlamına gelmez. Test davranış değişiklikleri hiçbiri olumsuz uygulamanızı etkilenen olduğundan emin olmak için uygulamanızın tüm alanları gerekecektir.

> [!TIP]
> Bkz: [var olan bir veritabanı ile ASP.NET Core EF Çekirdeğinde ile çalışmaya başlama](xref:core/get-started/aspnetcore/existing-db) var olan bir veritabanı, çalışma konusunda ek bir başvuru için 
