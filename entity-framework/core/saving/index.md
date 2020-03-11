---
title: Veri kaydetme-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417550"
---
# <a name="saving-data"></a>Verileri Kaydetme

Her bağlam örneği, veritabanına yazılması gereken değişiklikleri saklamaktan sorumlu bir `ChangeTracker` sahiptir. Varlık sınıflarınızın örneklerine değişiklik yaparken, bu değişiklikler `ChangeTracker` kaydedilir ve `SaveChanges`çağırdığınızda veritabanına yazılır. Veritabanı sağlayıcısı, değişiklikleri veritabanına özgü işlemlere çevirmekten sorumludur (örneğin, bir ilişkisel veritabanı için `INSERT`, `UPDATE`ve `DELETE` komutları).
