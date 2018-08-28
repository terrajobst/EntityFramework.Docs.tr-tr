---
title: EF Core 1.1 - EF Core yenilikleri
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 9f8f2d46f967c7d8ec4f8ea410e51531dfe3ca7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995441"
---
# <a name="new-features-in-ef-core-11"></a>EF Core 1.1 yeni özellikler

## <a name="modelling"></a>Modelleme
### <a name="field-mapping"></a>Alan eşleme
Bir özellik için destek alanı yapılandırmanıza olanak sağlar. Bu salt okunur özellikler veya Get/Set yöntemleri yerine bir özellik olan veriler için kullanışlı olabilir.
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>SQL Server bellek için iyileştirilmiş tablolara eşleme
Bir varlığa eşlenmiş tablosu bellek için iyileştirilmiş olduğunu belirtebilirsiniz. Ne zaman EF Core oluşturmak ve bir veritabanını korumak için tabanlı kullanarak modelinize göre (geçişleri ile veya `Database.EnsureCreated()`), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulur.

## <a name="change-tracking"></a>Change tracking
### <a name="additional-change-tracking-apis-from-ef6"></a>Ek değişiklik EF6'dan API'leri izleme
Gibi `Reload`, `GetModifiedProperties`, `GetDatabaseValues` vs.

## <a name="query"></a>Sorgu
### <a name="explicit-loading"></a>Açık yükleme
Bir gezinme özelliği veritabanından daha önce yüklenen bir varlıkta popülasyonunu tetiklemek sağlar.
### <a name="dbsetfind"></a>DbSet.Find
Birincil anahtar değerini alan bir varlık getirmek için kolay bir yol sağlar.

## <a name="other"></a>Diğer
### <a name="connection-resiliency"></a>Bağlantı dayanıklılığı
Veritabanı komutları otomatik olarak yeniden deneme başarısız oldu. Bu özellikle yararlı olduğunda SQL Azure, geçici hataları nerede genel bağlantı.
### <a name="simplified-service-replacement"></a>Basitleştirilmiş hizmet değiştirme
EF kullanan iç Hizmetleri değiştirilecek kolaylaştırır.
