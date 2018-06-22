---
title: EF çekirdek 1.1 - EF çekirdek yenilikler nelerdir?
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 74c1033cab2704bdbb9fa4d3ce111df1f1c29418
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054288"
---
# <a name="new-features-in-ef-core-11"></a>EF çekirdek 1.1 yeni özellikler

## <a name="modelling"></a>Model oluşturma
### <a name="field-mapping"></a>Alan eşleme
Bir özellik için bir yedekleme alanını yapılandırmanıza izin verir. Bu salt okunur özellikler veya bir özellik yerine Get/Set yöntemleri sahip veriler için yararlı olabilir.
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Bellek için iyileştirilmiş tablolar SQL Server ile eşleme
Bir varlık eşlenmiş tablosu bellek için iyileştirilmiş olduğunu belirtebilirsiniz. Ne zaman EF çekirdek bir veritabanını oluşturmak ve korumak için tabanlı kullanarak modelinize göre (geçişler biriyle veya `Database.EnsureCreated()`), bir bellek için iyileştirilmiş tablo bu varlıklar için oluşturulur.

## <a name="change-tracking"></a>Change tracking
### <a name="additional-change-tracking-apis-from-ef6"></a>Ek değişiklik EF6 API izleme
Gibi `Reload`, `GetModifiedProperties`, `GetDatabaseValues` vs.

## <a name="query"></a>Sorgu
### <a name="explicit-loading"></a>Açık yükleniyor
Veritabanından daha önce yüklenen bir varlık bir gezinti özelliği popülasyonunu tetiklemek sağlar.
### <a name="dbsetfind"></a>DbSet.Find
Birincil anahtar değerine göre varlık almak için kolay bir yol sağlar.

## <a name="other"></a>Diğer
### <a name="connection-resiliency"></a>Bağlantı dayanıklılığı
Veritabanı komutları otomatik olarak yeniden denemeler başarısız oldu. Bu özellikle yararlı olduğunda SQL Azure, geçici hataları nerede ortak bağlantı.
### <a name="simplified-service-replacement"></a>Basitleştirilmiş hizmet değiştirme
EF kullanan iç Hizmetleri değiştirmek kolaylaştırır.
