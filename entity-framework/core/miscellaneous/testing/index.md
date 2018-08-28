---
title: Entity Framework - EF Core kullanarak bileşenleri test
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997843"
---
# <a name="testing"></a>Sınama

Asıl veritabanı g/ç işlemleri yükü olmadan gerçek veritabanına bağlanıyor benzeyen bir şey kullanarak bileşenleri test isteyebilirsiniz.

Bunu yapmak için iki ana seçeneğiniz vardır:
 * [SQLite bellek içi modda](sqlite.md) , ilişkisel bir veritabanı gibi davranan bir sağlayıcısına verimli testleri yazmanızı sağlar.
 * [Inmemory sağlayıcı](in-memory.md) en az bir bağımlılığı yoktur, ancak her zaman bir ilişkisel veritabanı gibi davranmaz basit bir sağlayıcıdır.
