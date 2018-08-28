---
title: EF Core ve EF6 Karşılaştırması
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997130"
---
# <a name="compare-ef-core--ef6"></a>EF Core ve EF6 Karşılaştırması

Entity Framework, Entity Framework Core ve Entity Framework 6 farklı iki sürümü vardır.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6), yıllar boyunca özellikleri ve sabitleme veri erişimi teknolojisidir. Bu ilk 2008'de, .NET Framework 3.5 SP1 ve Visual Studio 2008 SP1'in bir parçası olarak yayımlanmıştır. Sevk olarak EF4.1 sürüm ile başlayarak [EntityFramework NuGet paketini](https://www.nuget.org/packages/EntityFramework/) -en popüler paketleri NuGet.org üzerinde şu anda biri.

EF6 desteklenen bir ürün olmaya devam eder ve hata düzeltmeleri ve süre için küçük iyileştirmeler görmeye devam edecektir.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core), Entity Framework'ün hafif, genişletilebilir ve platformlar arası bir sürümüdür. EF Core birçok geliştirme ve EF6 ile karşılaştırıldığında yeni özellikleri tanıtır. Aynı zamanda, EF Core bir yeni temel ve EF6 olarak kadar olgun koddur.

EF Core Geliştirici deneyimini EF6'dan tutar ve EF Core EF6 kullanmış istemeyenler için çok tanıdık gelmeli şekilde en üst düzey API'lerinin aynı de kalır. Aynı zamanda, EF Core temel bileşenleri tamamen yeni bir küme üzerinde oluşturulmuştur. Başka bir deyişle, EF Core EF6'dan tüm özelliklerini otomatik olarak devralmaz. Gelecek sürümlerde bu özelliklerin bazıları gösterilir, ancak daha az kullanılan diğer özellikleri de EF Core uygulanan değil.

Yeni, genişletilebilir ve basit çekirdek ayrıca bazı özellikler EF6 ' (örneğin, alternatif anahtarlar, toplu güncelleştirmeler ve karma istemci/veritabanı değerlendirme LINQ sorgularında) gerçekleştirilmez EF Core eklemek etmemizi de sağladı.
