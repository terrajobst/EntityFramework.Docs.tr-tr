---
title: Entity Framework Tasarımcısı - EF6 ObjectContext geri dönülüyor
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
caps.latest.revision: 3
ms.openlocfilehash: 17fa6538cf5e92f59ab72376af96ad65c640a085
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912789"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a><span data-ttu-id="cfd2a-102">Entity Framework Tasarımcısı'nda ObjectContext geri dönülüyor</span><span class="sxs-lookup"><span data-stu-id="cfd2a-102">Reverting to ObjectContext in Entity Framework Designer</span></span>
<span data-ttu-id="cfd2a-103">EF Designer ile oluşturulan bir model Entity Framework'ün önceki sürümüyle ObjectContext türetilmiş bir bağlam ve EntityObject türetilen varlık sınıfları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cfd2a-103">With previous version of Entity Framework a model created with the EF Designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span>

<span data-ttu-id="cfd2a-104">EF4.1 ile başlayan DbContext ve POCO varlık sınıflarından türetme bir bağlamı oluşturan kod oluşturma şablonuna takas önerilir.</span><span class="sxs-lookup"><span data-stu-id="cfd2a-104">Starting with EF4.1 we recommended swapping to a code generation template that generates a context deriving from DbContext and POCO entity classes.</span></span>

<span data-ttu-id="cfd2a-105">Visual Studio 2012'de varsayılan olarak EF Designer ile oluşturulan tüm yeni modeller için oluşturulan DbContext kodunu alın.</span><span class="sxs-lookup"><span data-stu-id="cfd2a-105">In Visual Studio 2012 you get DbContext code generated by default for all new models created with the EF Designer.</span></span> <span data-ttu-id="cfd2a-106">Mevcut modellerde DbContext göre Kod Oluşturucu için takas etmek karar vermediğiniz sürece ObjectContext göre kod üretmek devam eder.</span><span class="sxs-lookup"><span data-stu-id="cfd2a-106">Existing models will continue to generate ObjectContext based code unless you decide to swap to the DbContext based code generator.</span></span>

## <a name="reverting-back-to-objectcontext-code-generation"></a><span data-ttu-id="cfd2a-107">ObjectContext kod oluşturma için geri döndürülüyor</span><span class="sxs-lookup"><span data-stu-id="cfd2a-107">Reverting Back to ObjectContext Code Generation</span></span>

### <a name="1-disable-dbcontext-code-generation"></a><span data-ttu-id="cfd2a-108">1. DbContext kod oluşturmayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="cfd2a-108">1. Disable DbContext Code Generation</span></span>

<span data-ttu-id="cfd2a-109">Nesil DbContext ve POCO türetilen sınıfların, projede iki .tt dosyaları tarafından işlenir, Çözüm Gezgini'nde .edmx dosyasını genişletirseniz, bu dosyaları görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="cfd2a-109">Generation of the derived DbContext and POCO classes is handled by two .tt files in you project, if you expand the .edmx file in solution explorer you will see these files.</span></span> <span data-ttu-id="cfd2a-110">Bu dosyaların her ikisini de projenizden silin.</span><span class="sxs-lookup"><span data-stu-id="cfd2a-110">Delete both of these files from your project.</span></span>

![CodeGenFiles](~/ef6/media/codegenfiles.png)

<span data-ttu-id="cfd2a-112">VB.NET kullanıyorsanız seçmeniz gerekecek **tüm dosyaları göster** iç içe geçmiş dosyaları görmek için düğme.</span><span class="sxs-lookup"><span data-stu-id="cfd2a-112">If you are using VB.NET you will need to select the **Show All Files** button to see the nested files.</span></span>

![Showallfıles](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a><span data-ttu-id="cfd2a-114">2. Yeniden ObjectContext kod oluşturmayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="cfd2a-114">2. Re-Enable ObjectContext Code Generation</span></span>

<span data-ttu-id="cfd2a-115">EF Tasarımcısı'nda model açık sağ tıklayın, boş bir bölüm tasarım yüzeyi ve select **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="cfd2a-115">Open you model in the EF Designer, right-click on a blank section of the design surface and select **Properties**.</span></span>

<span data-ttu-id="cfd2a-116">Özellikler penceresinde değişiklik **kod oluşturma stratejisi** gelen **hiçbiri** için **varsayılan**.</span><span class="sxs-lookup"><span data-stu-id="cfd2a-116">In the Properties window change the **Code Generation Strategy** from **None** to **Default**.</span></span>

![CodeGenStrategy](~/ef6/media/codegenstrategy.png)