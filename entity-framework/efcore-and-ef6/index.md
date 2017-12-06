---
title: "EF çekirdek & EF6 karşılaştırma"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a>EF çekirdek & EF6 karşılaştırma

Entity Framework, Entity Framework Çekirdek ve Entity Framework 6 iki sürümü vardır.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) bir yıllardır özellikleri ve sabitlemeyi ile denenmiş ve test edilmiş veri erişim teknolojisidir. Bu ilk 2008'de .NET Framework 3.5 SP1 ve Visual Studio 2008 SP1'in bir parçası olarak yayımlanmıştır. Sevk olarak EF4.1 yayın başlayarak [EntityFramework NuGet paketi](https://www.nuget.org/packages/EntityFramework/) -NuGet.org şu anda en popüler paketi.

EF6 desteklenen bir ürün olarak devam eder ve hata düzeltmeleri ve görünmesi biraz zaman için küçük geliştirmeleri görmeye devam edersiniz.

## <a name="entity-framework-core"></a>Entity Framework Çekirdek

Entity Framework Çekirdek (EF temel), Entity Framework'ün hafif, genişletilebilir ve platformlar arası bir sürümüdür. EF çekirdek birçok geliştirmeleri ve EF6 ile karşılaştırıldığında yeni özellikleri sunar. Aynı anda EF çekirdek yeni bir kod temel ve EF6 olarak olarak olgun ' dir.

EF çekirdek geliştirici deneyimi EF6 tutar ve EF çekirdek EF6 kullanmış olan terimleri bilgili eşitleyerek şekilde en üst düzey API'leri aynı de kalır. Aynı anda EF çekirdek tamamen yeni bir çekirdek bileşenleri kümesi üzerinde oluşturulmuştur. Başka bir deyişle, EF çekirdek otomatik olarak EF6 tüm özellikleri devralmaz. Bu özelliklerden bazıları daha az yaygın olarak kullanılan gelecek sürümlerde (örneğin, yavaş yükleniyor ve bağlantı dayanıklılığı), diğer özellikleri EF çekirdek uygulanmadı gösterir.

Yeni, genişletilebilir ve basit çekirdek bize EF6 (örneğin, alternatif anahtarlarını ve LINQ sorgularını karma istemci/veritabanı hesaplanmasında) uygulanacak değil EF çekirdek bazı özellikleri eklemek de izin.
