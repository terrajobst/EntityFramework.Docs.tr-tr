---
title: Uzamsal türler - EF6 sağlayıcı desteği
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: ffd22222f59a541d8135d3738d37a7e8f5dc5d7c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489758"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="89301-102">Uzamsal türler için sağlayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="89301-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="89301-103">Entity Framework DbGeography veya DbGeometry sınıflarıyla uzamsal veri ile çalışma destekler.</span><span class="sxs-lookup"><span data-stu-id="89301-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="89301-104">Bu sınıflar Entity Framework sağlayıcısı tarafından sunulan özel veritabanı işlevleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="89301-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="89301-105">Uzamsal veriler tüm sağlayıcıları destekler ve olmayanlar uzamsal türü derlemeler yüklenmesi gibi ek Önkoşullar var.</span><span class="sxs-lookup"><span data-stu-id="89301-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="89301-106">Uzamsal türler için sağlayıcı desteği hakkında daha fazla bilgi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="89301-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="89301-107">Code First, diğer veritabanı ilk veya Model ilk için iki izlenecek uzamsal türler bir uygulamada kullanma hakkında ek bilgiler bulunabilir:</span><span class="sxs-lookup"><span data-stu-id="89301-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="89301-108">Uzamsal veri türleri kodda önce</span><span class="sxs-lookup"><span data-stu-id="89301-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="89301-109">EF Designer uzamsal veri türleri</span><span class="sxs-lookup"><span data-stu-id="89301-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="89301-110">EF uzamsal türler destekleyen yayımlar.</span><span class="sxs-lookup"><span data-stu-id="89301-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="89301-111">Uzamsal türler için destek EF5 içinde kullanılmaya başlandı.</span><span class="sxs-lookup"><span data-stu-id="89301-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="89301-112">Ancak, uygulama hedefler ve .NET 4.5 üzerinde çalışır EF5 içinde uzamsal türler yalnızca desteklenir.</span><span class="sxs-lookup"><span data-stu-id="89301-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="89301-113">Hem .NET 4 ve .NET 4. 5'i hedefleyen uygulamalar için desteklenen uzamsal türleri EF6 ile'başlatılıyor.</span><span class="sxs-lookup"><span data-stu-id="89301-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="89301-114">Uzamsal türlerini destekleyen EF sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="89301-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="89301-115">EF5</span><span class="sxs-lookup"><span data-stu-id="89301-115">EF5</span></span>  

<span data-ttu-id="89301-116">Entity Framework sağlayıcıları için destek uzamsal türler olduğunu farkında duyuyoruz EF5:</span><span class="sxs-lookup"><span data-stu-id="89301-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="89301-117">Microsoft SQL Server sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="89301-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="89301-118">Bu sağlayıcı EF5 bir parçası olarak sevk edilir.</span><span class="sxs-lookup"><span data-stu-id="89301-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="89301-119">Bu sağlayıcı yüklenmesi gereken bazı ek alt düzey kitaplıklarını temel bağlıdır; Ayrıntılar için aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="89301-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="89301-120">Devart dotConnect Oracle için</span><span class="sxs-lookup"><span data-stu-id="89301-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="89301-121">Bir üçüncü taraf sağlayıcısından Devart budur.</span><span class="sxs-lookup"><span data-stu-id="89301-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="89301-122">Uzamsal türler sonra lütfen destekleyen bir EF5 sağlayıcısı biliyorsanız kişi alın ve biz bu listeye eklemek memnuniyet duyacaktır.</span><span class="sxs-lookup"><span data-stu-id="89301-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="89301-123">EF6</span><span class="sxs-lookup"><span data-stu-id="89301-123">EF6</span></span>  

<span data-ttu-id="89301-124">Entity Framework sağlayıcıları için destek uzamsal türler olduğunu farkında duyuyoruz EF6:</span><span class="sxs-lookup"><span data-stu-id="89301-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="89301-125">Microsoft SQL Server sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="89301-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="89301-126">Bu sağlayıcı EF6 bir parçası olarak sevk edilir.</span><span class="sxs-lookup"><span data-stu-id="89301-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="89301-127">Bu sağlayıcı yüklenmesi gereken bazı ek alt düzey kitaplıklarını temel bağlıdır; Ayrıntılar için aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="89301-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="89301-128">Devart dotConnect Oracle için</span><span class="sxs-lookup"><span data-stu-id="89301-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="89301-129">Bir üçüncü taraf sağlayıcısından Devart budur.</span><span class="sxs-lookup"><span data-stu-id="89301-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="89301-130">Uzamsal türler sonra lütfen destekleyen bir EF6 sağlayıcısı biliyorsanız kişi alın ve biz bu listeye eklemek memnuniyet duyacaktır.</span><span class="sxs-lookup"><span data-stu-id="89301-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="89301-131">Microsoft SQL Server uzamsal türler için Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="89301-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="89301-132">SQL Server uzamsal destek alt düzey, SQL Server'a özgü SqlGeography ve SqlGeometry türlerine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="89301-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="89301-133">Bu tür Microsoft.SqlServer.Types.dll derlemede canlı ve bu derlemeye EF bir parçası olarak ya da .NET Framework'ün bir parçası olarak gönderildiğini değil.</span><span class="sxs-lookup"><span data-stu-id="89301-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="89301-134">Visual Studio yüklendiğinde, genellikle de bir SQL Server sürümü yükler ve bu Microsoft.SqlServer.Types.dll yüklenmesini içerir.</span><span class="sxs-lookup"><span data-stu-id="89301-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="89301-135">SQL Server uzamsal türler kullanmak istediğiniz makinede yüklü değil veya SQL Server yüklemesinden uzamsal türler dışlanan, bunları el ile yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="89301-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="89301-136">Türleri kullanılarak yüklenebilir `SQLSysClrTypes.msi`, Microsoft SQL Server özellik Paketi'nin bir parçası değildir.</span><span class="sxs-lookup"><span data-stu-id="89301-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="89301-137">Uzamsal türleridir öneririz, böylece SQL Server sürümü özgü ["SQL Server özellik paketi" için arama](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) Microsoft Download Center'daki sonra seçin ve karşılık gelen seçeneği için kullanacağınız SQL Server sürümünü indirin.</span><span class="sxs-lookup"><span data-stu-id="89301-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
