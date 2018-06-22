---
title: Veritabanı şemalarını - EF çekirdek yönetme
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26054741"
---
# <a name="managing-database-schemas"></a>Veritabanı şemalarını yönetme
EF çekirdek EF çekirdek modeli ve veritabanı şeması eşitlenmiş şekilde kalmasının iki birincil yolu sağlar. İkisi arasında seçmek için EF çekirdek modelinizi veya veritabanı şeması gerçekte kaynak olup olmadığına karar vermek.

Gerçekte kaynağı olarak EF çekirdek modelinizi istiyorsanız kullanın [geçişler][1]. EF çekirdek modelinizi değişiklik gibi EF çekirdek modeliyle uyumlu kalmayacak şekilde bu yaklaşım karşılık gelen şema değişiklikleri veritabanınıza artımlı olarak uygulanır.

Kullanım [ters mühendislik] [ 2] gerçekte kaynağı olarak veritabanı şemanızı istiyorsanız. Bu yaklaşım, bir DbContext ve varlık türü tersine veritabanı şemanızı EF çekirdek modeline mühendislik tarafından iskelesi olanak sağlar.

> [!NOTE]
> [Oluşturmak ve API bırakma] [ 3] veritabanı şeması EF çekirdek modelden de oluşturabilirsiniz. Ancak, bunlar öncelikli olarak sınama, prototipi oluşturulurken ve veritabanını silmek kabul edilebilir olduğu diğer senaryolar için değildir.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
