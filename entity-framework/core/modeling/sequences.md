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
# <a name="sequences"></a><span data-ttu-id="db3c2-102">Diziler</span><span class="sxs-lookup"><span data-stu-id="db3c2-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="db3c2-103">Diziler, genellikle yalnızca ilişkisel veritabanları tarafından desteklenen bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="db3c2-103">Sequences are a feature typically supported only by relational databases.</span></span> <span data-ttu-id="db3c2-104">Cosmos gibi ilişkisel olmayan bir veritabanı kullanıyorsanız, benzersiz değerler oluşturmak için veritabanı belgelerinize bakın.</span><span class="sxs-lookup"><span data-stu-id="db3c2-104">If you're using a non-relational database such as Cosmos, check your database documentation on generating unique values.</span></span>

<span data-ttu-id="db3c2-105">Bir dizi, veritabanında benzersiz ve sıralı sayısal değerler üretir.</span><span class="sxs-lookup"><span data-stu-id="db3c2-105">A sequence generates unique, sequential numeric values in the database.</span></span> <span data-ttu-id="db3c2-106">Sıralar belirli bir tabloyla ilişkili değildir ve aynı diziden değerler çizmek için birden çok tablo ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="db3c2-106">Sequences are not associated with a specific table, and multiple tables can be set up to draw values from the same sequence.</span></span>

## <a name="basic-usage"></a><span data-ttu-id="db3c2-107">Temel kullanım</span><span class="sxs-lookup"><span data-stu-id="db3c2-107">Basic usage</span></span>

<span data-ttu-id="db3c2-108">Modelde bir sıra ayarlayabilir ve sonra Özellikler için değer oluşturmak üzere bunu kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="db3c2-108">You can set up a sequence in the model, and then use it to generate values for properties:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

<span data-ttu-id="db3c2-109">Bir dizideki değeri oluşturmak için kullanılan belirli SQL 'in veritabanına özgü olduğunu unutmayın. Yukarıdaki örnek SQL Server çalışır, ancak diğer veritabanlarında başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="db3c2-109">Note that the specific SQL used to generate a value from a sequence is database-specific; the above example works on SQL Server but will fail on other databases.</span></span> <span data-ttu-id="db3c2-110">Daha fazla bilgi için belirli veritabanınızın belgelerine başvurun.</span><span class="sxs-lookup"><span data-stu-id="db3c2-110">Consult your specific database's documentation for more information.</span></span>

## <a name="configuring-sequence-settings"></a><span data-ttu-id="db3c2-111">Sıra ayarlarını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="db3c2-111">Configuring sequence settings</span></span>

<span data-ttu-id="db3c2-112">Ayrıca, dizinin şeması, başlangıç değeri, artış vb. gibi ek yönlerini de yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="db3c2-112">You can also configure additional aspects of the sequence, such as its schema, start value, increment, etc.:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
