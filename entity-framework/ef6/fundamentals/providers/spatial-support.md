---
title: Uzamsal türler için sağlayıcı desteği-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416341"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="dda1d-102">Uzamsal türler için sağlayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="dda1d-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="dda1d-103">Entity Framework, Dbcoğrafya veya DbGeometry sınıfları aracılığıyla uzamsal verilerle çalışmayı destekler.</span><span class="sxs-lookup"><span data-stu-id="dda1d-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="dda1d-104">Bu sınıflar, Entity Framework sağlayıcısı tarafından sunulan veritabanına özgü işlevleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="dda1d-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="dda1d-105">Tüm sağlayıcılar uzamsal verileri desteklemez ve bu, uzamsal tür derlemelerinin yüklenmesi gibi ek önkoşullara sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="dda1d-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="dda1d-106">Uzamsal türler için sağlayıcı desteği hakkında daha fazla bilgi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="dda1d-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="dda1d-107">Bir uygulamada uzamsal türlerin nasıl kullanılacağına ilişkin ek bilgiler, biri Code First Database First veya Model First için olmak üzere iki izlenecek yol içinde bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="dda1d-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="dda1d-108">Code First uzamsal veri türleri</span><span class="sxs-lookup"><span data-stu-id="dda1d-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="dda1d-109">EF tasarımcısında uzamsal veri türleri</span><span class="sxs-lookup"><span data-stu-id="dda1d-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="dda1d-110">Uzamsal türleri destekleyen EF yayınları</span><span class="sxs-lookup"><span data-stu-id="dda1d-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="dda1d-111">Uzamsal türler için destek EF5 içinde tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="dda1d-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="dda1d-112">Ancak, EF5 uzamsal türler yalnızca uygulama .NET 4,5 üzerinde hedeflerse ve çalıştırıldığında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="dda1d-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="dda1d-113">EF6 uzamsal türlerinden başlamak, hem .NET 4 hem de .NET 4,5 'yi hedefleyen uygulamalar için desteklenir.</span><span class="sxs-lookup"><span data-stu-id="dda1d-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="dda1d-114">Uzamsal türleri destekleyen EF sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="dda1d-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="dda1d-115">EF5</span><span class="sxs-lookup"><span data-stu-id="dda1d-115">EF5</span></span>  

<span data-ttu-id="dda1d-116">EF5 için Entity Framework sağlayıcılar, bu şekilde desteklenen uzamsal türleri destekler:</span><span class="sxs-lookup"><span data-stu-id="dda1d-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="dda1d-117">Microsoft SQL Server sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="dda1d-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="dda1d-118">Bu sağlayıcı, EF5 bir parçası olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="dda1d-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="dda1d-119">Bu sağlayıcı, yüklenmesi gerekebilecek bazı ek alt düzey kitaplıklara bağımlıdır. Ayrıntılar için aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="dda1d-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="dda1d-120">Oracle için Devart dotConnect</span><span class="sxs-lookup"><span data-stu-id="dda1d-120">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="dda1d-121">Bu, Devart 'den bir üçüncü taraf sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="dda1d-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="dda1d-122">Uzamsal türleri destekleyen bir EF5 sağlayıcısı biliyorsanız, lütfen bu kişiye ulaşın ve bu listeye eklemek isteriz.</span><span class="sxs-lookup"><span data-stu-id="dda1d-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="dda1d-123">EF6</span><span class="sxs-lookup"><span data-stu-id="dda1d-123">EF6</span></span>  

<span data-ttu-id="dda1d-124">EF6 için Entity Framework sağlayıcılar, bu şekilde desteklenen uzamsal türleri destekler:</span><span class="sxs-lookup"><span data-stu-id="dda1d-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="dda1d-125">Microsoft SQL Server sağlayıcı</span><span class="sxs-lookup"><span data-stu-id="dda1d-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="dda1d-126">Bu sağlayıcı, EF6 bir parçası olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="dda1d-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="dda1d-127">Bu sağlayıcı, yüklenmesi gerekebilecek bazı ek alt düzey kitaplıklara bağımlıdır. Ayrıntılar için aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="dda1d-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="dda1d-128">Oracle için Devart dotConnect</span><span class="sxs-lookup"><span data-stu-id="dda1d-128">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="dda1d-129">Bu, Devart 'den bir üçüncü taraf sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="dda1d-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="dda1d-130">Uzamsal türleri destekleyen bir EF6 sağlayıcısı biliyorsanız, lütfen bu kişiye ulaşın ve bu listeye eklemek isteriz.</span><span class="sxs-lookup"><span data-stu-id="dda1d-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="dda1d-131">Microsoft SQL Server ile uzamsal türler için Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="dda1d-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="dda1d-132">SQL Server uzamsal destek, alt düzey, SQL Server özgü tür Sqlcoğrafya ve SqlGeometry ' ne bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="dda1d-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="dda1d-133">Bu türler, Microsoft. SqlServer. Types. dll derlemesinde bulunur ve bu derleme EF 'in bir parçası olarak veya .NET Framework bir parçası olarak sevk edilmez.</span><span class="sxs-lookup"><span data-stu-id="dda1d-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="dda1d-134">Visual Studio yüklendiğinde, genellikle SQL Server bir sürümünü de yükler ve bu, Microsoft. SqlServer. Types. dll ' nin yüklenmesini içerir.</span><span class="sxs-lookup"><span data-stu-id="dda1d-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="dda1d-135">Uzamsal türleri kullanmak istediğiniz makinede SQL Server yüklü değilse veya uzamsal türler SQL Server yüklemesinden dışlanmazsa, bunları el ile yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="dda1d-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="dda1d-136">Türler, Microsoft SQL Server Özellik paketinin bir parçası olan `SQLSysClrTypes.msi`kullanılarak yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="dda1d-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="dda1d-137">Uzamsal türler sürüme özgü SQL Server, bu nedenle Microsoft Indirme merkezi 'nde ["SQL Server Özellik paketi" için arama](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) yapmanızı öneririz, sonra kullanacağınız SQL Server sürümüne karşılık gelen seçeneği seçin ve indirin.</span><span class="sxs-lookup"><span data-stu-id="dda1d-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
