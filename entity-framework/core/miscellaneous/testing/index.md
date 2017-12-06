---
title: "Entity Framework - EF çekirdek kullanarak bileşenleri test"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a>Test etme

Asıl veritabanını g/ç işlemleri yükü olmadan gerçek veritabanına bağlanma benzeyen bir şey kullanarak bileşenleri test etmek isteyebilirsiniz.

Bunu yapmak için iki ana seçeneğiniz vardır:
 * [SQLite bellek içi modu](sqlite.md) ilişkisel veritabanı gibi davranan bir sağlayıcısına verimli testleri yazmanızı sağlar.
 * [Inmemory sağlayıcısı](in-memory.md) en az bağımlılıklara sahiptir, ancak her zaman bir ilişkisel veritabanı gibi davranır olmayan basit bir sağlayıcıdır.
