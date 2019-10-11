---
title: EF Core kullanarak bileşenleri test etme EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 8de7df80ce91c4d94133a96d759dd552d0ba1884
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181303"
---
# <a name="testing-components-using-ef-core"></a>EF Core kullanarak bileşenleri test etme

Gerçek veritabanı g/ç işlemlerinin ek yükü olmadan, gerçek veritabanına bağlanan bir şeyi kullanarak bileşenleri test etmek isteyebilirsiniz.

Bunu yapmanın iki ana seçeneği vardır:
 * [SQLite bellek içi modu](sqlite.md) , ilişkisel bir veritabanı gibi davranan bir sağlayıcıya karşı etkili testler yazmanızı sağlar.
 * [InMemory sağlayıcısı](in-memory.md) , en az bağımlılığı olan basit bir sağlayıcıdır, ancak her zaman ilişkisel bir veritabanı gibi davranmaz.
