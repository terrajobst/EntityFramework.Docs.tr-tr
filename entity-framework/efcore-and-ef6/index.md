---
title: EF Core ve EF6 Karşılaştırması
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4609ecbc9e24d8a359694d256523c64141b5ff62
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002762"
---
# <a name="compare-ef-core--ef6"></a>EF Core ve EF6 Karşılaştırması

Entity Framework, Entity Framework Çekirdek ve Entity Framework 6 iki farklı sürümü vardır.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) bir yıllardır özellikleri ve sabitlemeyi ile denenmiş ve test edilmiş veri erişim teknolojisidir. Bu ilk 2008'de .NET Framework 3.5 SP1 ve Visual Studio 2008 SP1'in bir parçası olarak yayımlanmıştır. Sevk olarak EF4.1 yayın başlayarak [EntityFramework NuGet paketi](https://www.nuget.org/packages/EntityFramework/) -NuGet.org en popüler paketleri şu anda biri.

EF6 desteklenen bir ürün olarak devam eder ve hata düzeltmeleri ve görünmesi biraz zaman için küçük geliştirmeleri görmeye devam edersiniz.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Çekirdek (EF temel), Entity Framework'ün hafif, genişletilebilir ve platformlar arası bir sürümüdür. EF çekirdek birçok geliştirmeleri ve EF6 ile karşılaştırıldığında yeni özellikleri sunar. Aynı anda EF çekirdek yeni bir kod temel ve EF6 olarak olarak olgun ' dir.

EF çekirdek geliştirici deneyimi EF6 tutar ve EF çekirdek EF6 kullanmış olan terimleri bilgili eşitleyerek şekilde en üst düzey API'leri aynı de kalır. Aynı anda EF çekirdek tamamen yeni bir çekirdek bileşenleri kümesi üzerinde oluşturulmuştur. Başka bir deyişle, EF çekirdek otomatik olarak EF6 tüm özellikleri devralmaz. Bu özelliklerden bazıları, gelecek sürümlerde gösterir, ancak daha az kullanılan diğer özellikleri EF çekirdek uygulanan değil.

Yeni, genişletilebilir ve basit çekirdek bize EF6 (örneğin, diğer anahtarları, toplu güncelleştirmeler ve LINQ sorgularını karma istemci/veritabanı hesaplanmasında) uygulanacak değil EF çekirdek bazı özellikleri eklemek de izin.
