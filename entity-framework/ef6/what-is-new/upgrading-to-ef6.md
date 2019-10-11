---
title: Entity Framework 6 ' ya yükseltme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 4395a9c117a6cf38e7fc08f11ee689d6fffa6fed
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182107"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="bd9ba-102">Entity Framework 6 ' ya yükseltme</span><span class="sxs-lookup"><span data-stu-id="bd9ba-102">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="bd9ba-103">EF 'in önceki sürümlerinde kod, bir NuGet paketinde gönderilen .NET Framework ve bant dışı (OOB) kitaplıklarının (birincil olarak EntityFramework. dll) bir parçası olarak sevk edilen çekirdek kitaplıklar (birincil olarak System. Data. Entity. dll) arasına bölünür.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-103">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="bd9ba-104">EF6, kodu çekirdek kitaplıklarından alır ve OOB kitaplıklarına ekler.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-104">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="bd9ba-105">Bu, EF 'in açık kaynak haline getirilme ve .NET Framework farklı bir hızda gelişebilmesini sağlamak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-105">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="bd9ba-106">Bunun sonucu olarak, uygulamaların taşınan türler için yeniden oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-106">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="bd9ba-107">Bu, EF 4,1 ve üzeri sürümlerde olduğu gibi DbContext kullanan uygulamalar için basit olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-107">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="bd9ba-108">ObjectContext 'in kullanıldığı ancak hala zor olmayan uygulamalar için biraz daha fazla iş gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-108">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="bd9ba-109">Mevcut bir uygulamayı EF6 'e yükseltmek için yapmanız gereken öğelerin bir denetim listesi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-109">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="bd9ba-110">1. EF6 NuGet paketini yükler</span><span class="sxs-lookup"><span data-stu-id="bd9ba-110">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="bd9ba-111">Yeni Entity Framework 6 çalışma zamanına yükseltmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-111">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="bd9ba-112">Projenize sağ tıklayın ve **NuGet Paketlerini Yönet..** . seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-112">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="bd9ba-113">**Çevrimiçi** sekmesinde **EntityFramework** ' ü seçin ve ardından **yükler** ' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-113">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="bd9ba-114">EntityFramework NuGet paketinin önceki bir sürümü yüklüyse, bunu EF6 sürümüne yükseltir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-114">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="bd9ba-115">Alternatif olarak, paket yöneticisi konsolundan aşağıdaki komutu çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="bd9ba-115">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="bd9ba-116">2. System. Data. Entity. dll ' ye bütünleştirilmiş kod başvurularının kaldırıldığından emin olun</span><span class="sxs-lookup"><span data-stu-id="bd9ba-116">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="bd9ba-117">EF6 NuGet paketinin yüklenmesi, sizin için projenizden System. Data. Entity başvurularını otomatik olarak kaldırmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-117">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="bd9ba-118">3. EF Designer (EDMX) modellerini EF 6. x kod üretimi kullanacak şekilde değiştirin</span><span class="sxs-lookup"><span data-stu-id="bd9ba-118">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="bd9ba-119">EF Designer ile oluşturulmuş modelleriniz varsa, EF6 uyumlu kod üretmek için kod oluşturma şablonlarını güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-119">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="bd9ba-120">Şu anda yalnızca Visual Studio 2012 ve 2013 için kullanılabilen EF 6. x DbContext Oluşturucu şablonları mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-120">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="bd9ba-121">Mevcut kod oluşturma şablonlarını silin.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-121">Delete existing code-generation templates.</span></span> <span data-ttu-id="bd9ba-122">Bu dosyalar genellikle **@no__t -1edmx_file_name\>.tt** ve **\<edmx_file_adı @ no__t-5 olarak adlandırılır. Context.tt** ve Çözüm Gezgini içindeki edmx dosyanızın altına yerleştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-122">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="bd9ba-123">Çözüm Gezgini şablonları seçebilir ve silmek için **del** tuşuna basabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-123">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="bd9ba-124">Web sitesi projelerinde, şablonlar edmx dosyanızın altına yerleştirmeyecektir, ancak Çözüm Gezgini yanında listelenir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-124">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="bd9ba-125">VB.NET projelerinde, iç içe şablon dosyalarını görebilmeniz için ' tüm dosyaları göster ' i etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-125">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="bd9ba-126">Uygun EF 6. x kod oluşturma şablonunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-126">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="bd9ba-127">Ortamınızı EF tasarımcısında açın, tasarım yüzeyine sağ tıklayıp **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-127">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="bd9ba-128">DbContext API 'SI kullanıyorsanız (önerilir), bu durumda **EF 6. x DbContext Oluşturucu** **veri** sekmesinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-128">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="bd9ba-129">Visual Studio 2012 kullanıyorsanız, bu şablonun olması için EF 6 araçlarını yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-129">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="bd9ba-130">Ayrıntılar için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md) .</span><span class="sxs-lookup"><span data-stu-id="bd9ba-130">See [Get Entity Framework](~/ef6/fundamentals/install.md) for details.</span></span>  

    - <span data-ttu-id="bd9ba-131">ObjectContext API 'sini kullanıyorsanız **çevrimiçi** sekmesini seçmeniz ve **EF 6. x EntityObject Generator**araması yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-131">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="bd9ba-132">Kod oluşturma şablonlarına herhangi bir özelleştirme uyguladıysanız, bunları güncelleştirilmiş şablonlara yeniden uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-132">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="bd9ba-133">4. Kullanılmakta olan tüm çekirdek EF türleri için ad alanlarını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="bd9ba-133">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="bd9ba-134">DbContext ve Code First türleri için ad alanları değişmemiştir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-134">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="bd9ba-135">Bu, EF 4,1 veya sonrasını kullanan birçok uygulama için herhangi bir şeyi değiştirmeniz gerekmediği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-135">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="bd9ba-136">Daha önce System. Data. Entity. dll içinde bulunan ObjectContext gibi türler yeni ad alanlarına taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-136">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="bd9ba-137">Bu, EF6 'e göre derlemek için *using* veya *Import* yönergelerinden güncelleştirmeniz gerekebilecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-137">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="bd9ba-138">Ad alanı değişikliklerinin genel kuralı, System. Data. \* içindeki herhangi bir tür System. Data. Entity. Core. \* öğesine taşınır.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-138">The general rule for namespace changes is that any type in System.Data.\* is moved to System.Data.Entity.Core.\*.</span></span> <span data-ttu-id="bd9ba-139">Diğer bir deyişle **Entity. Core** 'u eklemeniz yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-139">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="bd9ba-140">System. Data 'dan sonra.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-140">after System.Data.</span></span> <span data-ttu-id="bd9ba-141">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bd9ba-141">For example:</span></span>

- <span data-ttu-id="bd9ba-142">System. Data. EntityException = > System. Data. **Entity. Core**. EntityException</span><span class="sxs-lookup"><span data-stu-id="bd9ba-142">System.Data.EntityException => System.Data.**Entity.Core**.EntityException</span></span>  
- <span data-ttu-id="bd9ba-143">System. Data. Objects. ObjectContext = > System. Data. **Entity. Core**. Objects. ObjectContext</span><span class="sxs-lookup"><span data-stu-id="bd9ba-143">System.Data.Objects.ObjectContext => System.Data.**Entity.Core**.Objects.ObjectContext</span></span>  
- <span data-ttu-id="bd9ba-144">System. Data. Objects. DataClasses. RelationshipManager = > System. Data. **Entity. Core**. Objects. DataClasses. RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="bd9ba-144">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core**.Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="bd9ba-145">Bu türler, çoğu DbContext tabanlı uygulama için doğrudan kullanılmadığından *çekirdek* ad uzayındadır.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-145">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="bd9ba-146">System. Data. Entity. dll ' nin parçası olan bazı türler yaygın olarak ve doğrudan DbContext tabanlı uygulamalar için kullanılır ve bu nedenle *çekirdek* ad alanlarına taşınamaz.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-146">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="bd9ba-147">Bunlar:</span><span class="sxs-lookup"><span data-stu-id="bd9ba-147">These are:</span></span>

- <span data-ttu-id="bd9ba-148">System. Data. EntityState = > System. Data. **Varlık**. EntityState</span><span class="sxs-lookup"><span data-stu-id="bd9ba-148">System.Data.EntityState => System.Data.**Entity**.EntityState</span></span>  
- <span data-ttu-id="bd9ba-149">System. Data. Objects. DataClasses. EdmFunctionAttribute = > System. Data. **Entity. DbFunctionAttribute**</span><span class="sxs-lookup"><span data-stu-id="bd9ba-149">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="bd9ba-150">Bu sınıf yeniden adlandırıldı; Eski adlı bir sınıf hala var ve işe yaramıştır, ancak artık eski olarak işaretlendi.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-150">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="bd9ba-151">System. Data. Objects. EntityFunctions = > System. Data. **Entity. DbFunctions**</span><span class="sxs-lookup"><span data-stu-id="bd9ba-151">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="bd9ba-152">Bu sınıf yeniden adlandırıldı; Eski ada sahip bir sınıf hala var ve çalışmaktadır, ancak artık eski olarak işaretlendi.)</span><span class="sxs-lookup"><span data-stu-id="bd9ba-152">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="bd9ba-153">Uzamsal sınıflar (örneğin, Dbcoğrafya, DbGeometry) System. Data. uzamsal = > System. Data öğesinden taşınmıştır. **Varlık**. Uzay</span><span class="sxs-lookup"><span data-stu-id="bd9ba-153">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity**.Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="bd9ba-154">System. Data ad alanındaki bazı türler, EF derlemesi olmayan System. Data. dll ' dir.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-154">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="bd9ba-155">Bu türler taşınmadı ve bu nedenle ad alanları değişmeden kalır.</span><span class="sxs-lookup"><span data-stu-id="bd9ba-155">These types have not moved and so their namespaces remain unchanged.</span></span>
