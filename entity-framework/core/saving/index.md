---
title: EF Core - veri kaydetme
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 97d7f1248a8d0adeed9714619c1364fa8f9822db
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949401"
---
# <a name="saving-data"></a>Verileri Kaydetme

Bağlam örneğinin her bir `ChangeTracker` veritabanına yazılması gereken değişiklikleri izlemek için sorumlu. Bu değişiklikler kaydedilir varlık sınıfları örneklerine değişiklik olarak `ChangeTracker` ve çağırdığınızda ardından veritabanına yazılan `SaveChanges`. Değişiklikleri veritabanına özel işlemler çevirmek için veritabanı sağlayıcısı sorumludur (örneğin, `INSERT`, `UPDATE`, ve `DELETE` komutları ilişkisel bir veritabanı için).
