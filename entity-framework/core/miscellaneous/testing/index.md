---
title: EF Core kullanarak bileşenleri test etme EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655793"
---
# <a name="testing-components-using-ef-core"></a>EF Core kullanarak bileşenleri test etme

Gerçek veritabanı g/ç işlemlerinin ek yükü olmadan, gerçek veritabanına bağlanan bir şeyi kullanarak bileşenleri test etmek isteyebilirsiniz.

Bunu yapmanın iki ana seçeneği vardır:

* [SQLite bellek içi modu](sqlite.md) , ilişkisel bir veritabanı gibi davranan bir sağlayıcıya karşı etkili testler yazmanızı sağlar.
* [InMemory sağlayıcısı](in-memory.md) , en az bağımlılığı olan basit bir sağlayıcıdır, ancak her zaman ilişkisel bir veritabanı gibi davranmaz.
