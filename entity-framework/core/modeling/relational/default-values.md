---
title: Varsayılan değerler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655910"
---
# <a name="default-values"></a><span data-ttu-id="ca8ed-102">Varsayılan Değerler</span><span class="sxs-lookup"><span data-stu-id="ca8ed-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="ca8ed-103">Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ca8ed-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ca8ed-104">Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.</span><span class="sxs-lookup"><span data-stu-id="ca8ed-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ca8ed-105">Bir sütunun varsayılan değeri, yeni bir satır eklenirse, ancak sütun için hiçbir değer belirtilmemişse eklenecek değerdir.</span><span class="sxs-lookup"><span data-stu-id="ca8ed-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="ca8ed-106">Kurallar</span><span class="sxs-lookup"><span data-stu-id="ca8ed-106">Conventions</span></span>

<span data-ttu-id="ca8ed-107">Kurala göre, varsayılan bir değer yapılandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="ca8ed-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ca8ed-108">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ca8ed-108">Data Annotations</span></span>

<span data-ttu-id="ca8ed-109">Veri ek açıklamalarını kullanarak varsayılan bir değer ayarlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="ca8ed-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ca8ed-110">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="ca8ed-110">Fluent API</span></span>

<span data-ttu-id="ca8ed-111">Bir özellik için varsayılan değeri belirtmek üzere Floent API 'sini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca8ed-111">You can use the Fluent API to specify the default value for a property.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

<span data-ttu-id="ca8ed-112">Varsayılan değeri hesaplamak için kullanılan bir SQL parçası de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca8ed-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
