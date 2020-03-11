---
title: Diziler-EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417442"
---
# <a name="sequences"></a>Diziler

> [!NOTE]  
> Diziler, genellikle yalnızca ilişkisel veritabanları tarafından desteklenen bir özelliktir. Cosmos gibi ilişkisel olmayan bir veritabanı kullanıyorsanız, benzersiz değerler oluşturmak için veritabanı belgelerinize bakın.

Bir dizi, veritabanında benzersiz ve sıralı sayısal değerler üretir. Sıralar belirli bir tabloyla ilişkili değildir ve aynı diziden değerler çizmek için birden çok tablo ayarlanabilir.

## <a name="basic-usage"></a>Temel kullanım

Modelde bir sıra ayarlayabilir ve sonra Özellikler için değer oluşturmak üzere bunu kullanabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

Bir dizideki değeri oluşturmak için kullanılan belirli SQL 'in veritabanına özgü olduğunu unutmayın. Yukarıdaki örnek SQL Server çalışır, ancak diğer veritabanlarında başarısız olur. Daha fazla bilgi için belirli veritabanınızın belgelerine başvurun.

## <a name="configuring-sequence-settings"></a>Sıra ayarlarını yapılandırma

Ayrıca, dizinin şeması, başlangıç değeri, artış vb. gibi ek yönlerini de yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
