---
title: EF Core kullanarak bileşenleri test etme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: a946387718546f14e1485b4093e6c8046188f62d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197498"
---
# <a name="testing-components-using-ef-core"></a>EF Core kullanarak bileşenleri test etme

Gerçek veritabanı g/ç işlemlerinin ek yükü olmadan, gerçek veritabanına bağlanan bir şeyi kullanarak bileşenleri test etmek isteyebilirsiniz.

Bunu yapmanın iki ana seçeneği vardır:
 * [SQLite bellek içi modu](sqlite.md) , ilişkisel bir veritabanı gibi davranan bir sağlayıcıya karşı etkili testler yazmanızı sağlar.
 * [InMemory sağlayıcısı](in-memory.md) , en az bağımlılığı olan basit bir sağlayıcıdır, ancak her zaman ilişkisel bir veritabanı gibi davranmaz.
