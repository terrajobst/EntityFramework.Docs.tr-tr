---
title: "Veritabanı sağlayıcıları - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>Veritabanı sağlayıcıları

Entity Framework Çekirdek sayıda farklı veritabanına erişmek için kullanılacak EF izin vermek için bir sağlayıcı modeli kullanır. Bazı kavramları çoğu veritabanları için ortaktır ve birincil EF çekirdek bileşenler dahil edilir. Nesnelerdeki değişiklikleri kez tacking veritabanından yüklenmeden ve bu tür kavramları LINQ, işlemler, sorgularda ifade içerir. Bazı kavramları, belirli bir sağlayıcıya özgü. Örneğin, SQL Server sağlayıcısı bellek için iyileştirilmiş tablolar (özelliği SQL Server'a özgü) yapılandırmanıza olanak sağlar. Diğer kavramlar sağlayıcıları sınıfına özeldir. Örneğin, ilişkisel veritabanları için EF çekirdek sağlayıcılar ortak yapı `Microsoft.EntityFrameworkCore.Relational` kitaplığı yapılandırmak için tablo ve sütun eşlemelerini, yabancı anahtar kısıtlamaları, vb. API'ler sağlar.

EF çekirdek sağlayıcılar çeşitli kaynakları tarafından oluşturulur. Tüm sağlayıcılar Entity Framework Çekirdek projenin bir parçası olarak korunur. Bir üçüncü taraf sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun.
