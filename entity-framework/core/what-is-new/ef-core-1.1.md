---
title: EF Core 1,1 ' deki yenilikler-EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656005"
---
# <a name="new-features-in-ef-core-11"></a>EF Core 1,1 ' deki yeni özellikler

## <a name="modeling"></a>Modelleme

### <a name="field-mapping"></a>Alan eşleme

Bir özellik için bir destek alanı yapılandırmanıza izin verir. Bu, salt okuma özellikleri veya bir özellik yerine get/set yöntemlerine sahip veriler için yararlı olabilir.

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>SQL Server bellek için Iyileştirilmiş tablolarla eşleme

Bir varlığın eşlendiği tablonun bellek için iyileştirilmiş olduğunu belirtebilirsiniz. Modelinize dayalı bir veritabanı oluşturmak ve korumak için EF Core kullanılırken (geçişlerle veya `Database.EnsureCreated()`), bu varlıklar için bellek için iyileştirilmiş bir tablo oluşturulacaktır.

## <a name="change-tracking"></a>Change tracking

### <a name="additional-change-tracking-apis-from-ef6"></a>EF6 'ten ek değişiklik izleme API 'Leri

`Reload`, `GetModifiedProperties`, `GetDatabaseValues` vb.

## <a name="query"></a>Sorgu

### <a name="explicit-loading"></a>Açık yükleme

Daha önce veritabanından yüklenmiş bir varlık üzerinde bir gezinti özelliği popülasyonu tetiklemeniz sağlar.

### <a name="dbsetfind"></a>DbSet. Find

, Birincil anahtar değerine göre bir varlığı getirmek için kolay bir yol sağlar.

## <a name="other"></a>Diğer

### <a name="connection-resiliency"></a>Bağlantı dayanıklılığı

Başarısız veritabanı komutlarını otomatik olarak yeniden dener. Bu, özellikle, geçici hataların yaygın olduğu SQL Azure bağlantı olduğunda faydalıdır.

### <a name="simplified-service-replacement"></a>Basitleştirilmiş hizmet değişimi

EF 'in kullandığı iç Hizmetleri değiştirmeyi kolaylaştırır.
