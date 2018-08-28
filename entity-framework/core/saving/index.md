---
title: EF Core - veri kaydetme
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998186"
---
# <a name="saving-data"></a>Verileri Kaydetme

Bağlam örneğinin her bir `ChangeTracker` veritabanına yazılması gereken değişiklikleri izlemek için sorumlu. Bu değişiklikler kaydedilir varlık sınıfları örneklerine değişiklik olarak `ChangeTracker` ve çağırdığınızda ardından veritabanına yazılan `SaveChanges`. Değişiklikleri veritabanına özel işlemler çevirmek için veritabanı sağlayıcısı sorumludur (örneğin, `INSERT`, `UPDATE`, ve `DELETE` komutları ilişkisel bir veritabanı için).
