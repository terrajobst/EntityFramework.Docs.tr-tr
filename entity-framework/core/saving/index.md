---
title: Veri Kaydetme - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417550"
---
# <a name="saving-data"></a>Verileri Kaydetme

Her bağlam örneğinin veritabanına yazılması gereken değişiklikleri izlemekten sorumlu bir `ChangeTracker` örneği vardır. Varlık sınıflarınızın örneklerinde değişiklik yaptığınızda, bu değişiklikler veritabanına kaydedilir `ChangeTracker` ve 'yi `SaveChanges`aradiğinizde veritabanına yazılır. Veritabanı sağlayıcısı, değişiklikleri veritabanına özgü işlemlere (örneğin, `INSERT` `UPDATE`ve ilişkisel `DELETE` bir veritabanı için komutlar) çevirmekten sorumludur.
