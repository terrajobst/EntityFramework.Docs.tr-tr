---
title: EF Core 1.1'deki yenilikler - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417519"
---
# <a name="new-features-in-ef-core-11"></a>EF Core 1.1'deki yeni özellikler

## <a name="modeling"></a>Modelleme

### <a name="field-mapping"></a>Alan eşlemesi

Bir özellik için bir destek alanını yapılandırmanızı sağlar. Bu, salt okunur özellikler veya özellik yerine Get/Set yöntemlerine sahip veriler için yararlı olabilir.

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>SQL Server'da Bellek En İyi Duruma Getirilmiş Tablolara Eşleme

Bir varlığın eşlenen tablosunun bellek için en iyi duruma getirilmiş olduğunu belirtebilirsiniz. Modelinize dayalı bir veritabanı oluşturmak ve korumak için EF `Database.EnsureCreated()`Core'u kullanırken (geçişlerle veya geçişlerle), bu varlıklar için bellek için en iyi duruma getirilmiş bir tablo oluşturulur.

## <a name="change-tracking"></a>Değişiklik izleme

### <a name="additional-change-tracking-apis-from-ef6"></a>EF6'dan ek değişiklik izleme API'leri

, `Reload`, `GetModifiedProperties` `GetDatabaseValues` vb. gibi.

## <a name="query"></a>Sorgu

### <a name="explicit-loading"></a>Açık Yükleme

Daha önce veritabanından yüklenen bir varlık üzerinde bir gezinti özelliğinin popülasyonunu tetiklemenizi sağlar.

### <a name="dbsetfind"></a>DbSet.Bul

Bir varlığı birincil anahtar değerine göre getirmenin kolay bir yolunu sağlar.

## <a name="other"></a>Diğer

### <a name="connection-resiliency"></a>Bağlantı dayanıklılığı

Başarısız veritabanı komutlarını otomatik olarak yeniden dener. Bu, geçici hataların yaygın olduğu SQL Azure'a bağlantı verirken özellikle kullanışlıdır.

### <a name="simplified-service-replacement"></a>Basitleştirilmiş hizmet değiştirme

EF'nin kullandığı dahili hizmetlerin değiştirilmesini kolaylaştırır.
