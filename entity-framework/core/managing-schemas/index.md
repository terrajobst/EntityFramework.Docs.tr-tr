---
title: Veritabanı şemalarını yönetme-EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416307"
---
# <a name="managing-database-schemas"></a>Veritabanı Şemalarını Yönetme

EF Core, EF Core modelinizi ve veritabanı şemanızı eşitlenmiş halde tutmanın iki temel yolunu sağlar. İkisi arasında seçim yapmak için EF Core modelinizin veya veritabanı şemasının Truth kaynağı olup olmadığına karar verin.

EF Core modelinizin Truth kaynağı olmasını istiyorsanız, [geçişleri][1]kullanın. EF Core modelinizde değişiklik yaparken bu yaklaşım, ilgili şema değişikliklerini, EF Core modelinizle uyumlu kalacak şekilde veritabanınıza uygular.

Veritabanı şemanızın Truth kaynağı olmasını istiyorsanız [ters mühendislik][2] kullanın. Bu yaklaşım, veritabanı şemanızı bir EF Core modeline ters mühendislik yaparak bir DbContext ve varlık türü sınıflarını dolandırıcılara katmanızı sağlar.

> [!NOTE]
> [Oluşturma ve bırakma API 'leri][3] , EF Core modelinizdeki veritabanı şemasını da oluşturabilir. Bununla birlikte, bunlar birincil olarak test, prototip oluşturma ve veritabanını bırakma için kabul edilebilir olan diğer senaryolar içindir.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
