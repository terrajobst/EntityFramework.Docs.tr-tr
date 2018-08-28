---
title: EF Core - veritabanı şemalarını yönetme
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994391"
---
# <a name="managing-database-schemas"></a>Veritabanı Şemalarını Yönetme
EF Core EF Core model ve veritabanı şemanızı eşitlenmiş durumda tutma iki birincil yöntem sağlar. İki tür arasında seçmek için EF Core modelinizi veya veritabanı şeması gerçeklik kaynağı olup olmadığına karar vermek.

EF Core modelinizin gerçeklik kaynağı olmasını istediğiniz kullanırsanız [geçişler][1]. EF Core modelinizi yaptığınız gibi EF Core modeliyle uyumlu kalmayacak şekilde bu yaklaşım karşılık gelen şema değişiklikleri veritabanınıza artımlı olarak uygulanır.

Kullanım [ters mühendislik] [ 2] gerçeklik kaynağı olarak veritabanı şemanızı istiyorsanız. Bu yaklaşım, veritabanı Şemanızda bir EF Core modeline mühendislik ters tarafından bir DbContext ve varlık türü sınıfları iskelesini olanak tanır.

> [!NOTE]
> [API oluşturma ve bırakma] [ 3] veritabanı şeması EF Core modelinizden de oluşturabilirsiniz. Ancak, öncelikli olarak sınama, prototip oluşturma ve veritabanı bırakılırken kabul edilebilir olduğu diğer senaryolar için değildirler.


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
