---
title: Veritabanı Şemalarını Yönetme - EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416307"
---
# <a name="managing-database-schemas"></a>Veritabanı Şemalarını Yönetme

EF Core, EF Core modelinizi ve veritabanı şemazınızı eşit tutmak için iki temel yol sağlar. İkisi arasında seçim yapmak için, EF Core modelinizin mi yoksa veritabanı şemazınızın gerçeğin kaynağı mı olduğuna karar verin.

EF Core modelinizin gerçeğin kaynağı olmasını istiyorsanız, [Geçişler'i][1]kullanın. EF Core modelinizde değişiklik yaptığınızda, bu yaklaşım, EF Core modelinizle uyumlu kalması için veritabanınıza karşılık gelen şema değişikliklerini aşamalı olarak uygular.

Veritabanı şemanızın gerçeğin kaynağı olmasını istiyorsanız [Ters Mühendislik'i][2] kullanın. Bu yaklaşım, veritabanı şemanızı bir EF Core modeline dönüştürerek bir DbContext ve varlık türü sınıflarını iskeleye dönüştürmenizi sağlar.

> [!NOTE]
> [OLUŞTURMA ve bırakma API'leri,][3] EF Core modelinizden veritabanı şeasını da oluşturabilir. Ancak, bunlar öncelikle veritabanını bırakmanın kabul edilebilir olduğu sınama, prototip oluşturma ve diğer senaryolar içindir.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
