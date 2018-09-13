---
title: Entity Framework 6 yükseltme
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 2e2dacfe67238bdb7fd1f31f784319049f0f2cb0
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490954"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="d6f51-102">Entity Framework 6 yükseltme</span><span class="sxs-lookup"><span data-stu-id="d6f51-102">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="d6f51-103">EF önceki sürümlerinde .NET Framework bir parçası olarak gönderildiğini çekirdek kitaplıkları (öncelikle System.Data.Entity.dll) ve bir NuGet paketi sevk bant dışı (OOB) kitaplıkları (öncelikle EntityFramework.dll) arasında kod bölünmüş.</span><span class="sxs-lookup"><span data-stu-id="d6f51-103">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="d6f51-104">EF6 çekirdek kitaplıklarından kodu alır ve OOB kitaplıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-104">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="d6f51-105">Bu açık kaynak yapılacak EF izin vermek üzere gerekli ve .NET Framework farklı bir hızda evrim Geçiren mümkün olması için.</span><span class="sxs-lookup"><span data-stu-id="d6f51-105">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="d6f51-106">Bu sonucu, uygulamaları taşınan türlerine karşı yeniden oluşturulması gerekecektir ' dir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-106">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="d6f51-107">Bu DbContext EF 4.1 ve üzeri sevk edildi olarak kullanan uygulamalar için basit olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d6f51-107">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="d6f51-108">Biraz daha fazla iş ObjectContext kullanan uygulamalar için gerekli değildir ancak yine de yapmak sabit değildir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-108">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="d6f51-109">EF6 varolan bir uygulamaya yükseltmek için yapmanız gereken şeyleri listesi aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-109">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="d6f51-110">1. EF6 NuGet paketini yükle</span><span class="sxs-lookup"><span data-stu-id="d6f51-110">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="d6f51-111">Yeni Entity Framework 6 çalışma zamanına yükseltmesini gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-111">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="d6f51-112">Seçin ve proje üzerinde sağ **NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="d6f51-112">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="d6f51-113">Altında **çevrimiçi** sekmesinde **EntityFramework** tıklatıp **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="d6f51-113">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="d6f51-114">EntityFramework NuGet paketi yüklendi önceki bir sürümü bu EF6 yükseltir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-114">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="d6f51-115">Alternatif olarak, Paket Yöneticisi konsolundan aşağıdaki komutu çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="d6f51-115">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="d6f51-116">2. Derleme başvurularını System.Data.Entity.dll kaldırıldığından emin olun</span><span class="sxs-lookup"><span data-stu-id="d6f51-116">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="d6f51-117">EF6 NuGet paketini yükleme otomatik olarak System.Data.Entity tüm başvurular projenizden sizin için kaldırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-117">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="d6f51-118">3. Takas herhangi EF Designer (EDMX) modellerini EF 6.x kod üretimini kullan</span><span class="sxs-lookup"><span data-stu-id="d6f51-118">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="d6f51-119">EF Designer ile oluşturulan herhangi bir model varsa, EF6 uyumlu kod üretmek için kod oluşturma şablonları güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-119">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="d6f51-120">Şu anda yalnızca EF 6.x DbContext Oluşturucu şablonları Visual Studio 2012 ve 2013 için kullanılabilen vardır.</span><span class="sxs-lookup"><span data-stu-id="d6f51-120">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="d6f51-121">Varolan kod üretimi şablonları silin.</span><span class="sxs-lookup"><span data-stu-id="d6f51-121">Delete existing code-generation templates.</span></span> <span data-ttu-id="d6f51-122">Bu dosyalar genellikle adlı  **\<edmx_file_name\>.tt** ve  **\<edmx_file_name\>. Context.tt** ve Çözüm Gezgini'nde edmx dosyanız altında iç içe olamaz.</span><span class="sxs-lookup"><span data-stu-id="d6f51-122">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="d6f51-123">Çözüm Gezgini ve ENTER tuşuna şablon seçebilirsiniz **Del** anahtarını silin.</span><span class="sxs-lookup"><span data-stu-id="d6f51-123">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="d6f51-124">Web sitesi projeleri şablonları değil edmx dosyanız altında iç içe geçmiş, ancak Çözüm Gezgini'nde yanı sıra listede.</span><span class="sxs-lookup"><span data-stu-id="d6f51-124">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="d6f51-125">VB.NET projelerinde 'Tüm iç içe geçmiş şablon dosyalarını görebilmeniz dosyaları Göster' etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-125">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="d6f51-126">Uygun EF 6.x kod kuşak şablonu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d6f51-126">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="d6f51-127">Açık EF Designer modelinizde sağ tasarım yüzeyi ve select **kod oluşturma öğesi Ekle...**</span><span class="sxs-lookup"><span data-stu-id="d6f51-127">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="d6f51-128">DbContext sonra (önerilen) API kullanıyorsanız **EF 6.x DbContext Oluşturucu** altında kullanılabilir olacak **veri** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="d6f51-128">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="d6f51-129">Visual Studio 2012 kullanıyorsanız, bu şablonu EF 6 araçları yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-129">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="d6f51-130">Bkz: [alma Entity Framework](~/ef6/fundamentals/install.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="d6f51-130">See [Get Entity Framework](~/ef6/fundamentals/install.md) for details.</span></span>  

    - <span data-ttu-id="d6f51-131">ObjectContext API kullanıyorsanız sonra seçmeniz gerekecek **çevrimiçi** sekmesinde ve arama **EF 6.x EntityObject Oluşturucu**.</span><span class="sxs-lookup"><span data-stu-id="d6f51-131">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="d6f51-132">Özelleştirmeler için kod oluşturma şablonları uyguladıysanız yeniden güncelleştirilmiş şablonların uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-132">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="d6f51-133">4. Ad alanları için kullanılan her çekirdek EF türünü güncelleştir</span><span class="sxs-lookup"><span data-stu-id="d6f51-133">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="d6f51-134">DbContext ve Code First türleri için ad alanları değişmedi.</span><span class="sxs-lookup"><span data-stu-id="d6f51-134">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="d6f51-135">EF 4.1 kullanan birçok uygulama için başka bir deyişle ya da daha sonra herhangi bir ayarı değiştirmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d6f51-135">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="d6f51-136">Daha önce System.Data.Entity.dll türlerini ObjectContext gibi yeni ad alanları için taşınmıştır.</span><span class="sxs-lookup"><span data-stu-id="d6f51-136">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="d6f51-137">Yani güncelleştirmeniz gerekebilir, *kullanarak* veya *alma* EF6 karşı oluşturmak için yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="d6f51-137">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="d6f51-138">Genel ad değişiklikleri için System.Data.\* herhangi bir türü için System.Data.Entity.Core.\* taşınır kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="d6f51-138">The general rule for namespace changes is that any type in System.Data.\* is moved to System.Data.Entity.Core.\*.</span></span> <span data-ttu-id="d6f51-139">Diğer bir deyişle, yalnızca eklemek **Entity.Core.**</span><span class="sxs-lookup"><span data-stu-id="d6f51-139">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="d6f51-140">System.Data sonra.</span><span class="sxs-lookup"><span data-stu-id="d6f51-140">after System.Data.</span></span> <span data-ttu-id="d6f51-141">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d6f51-141">For example:</span></span>

- <span data-ttu-id="d6f51-142">System.Data.EntityException = > System.Data. **Entity.Core.** EntityException</span><span class="sxs-lookup"><span data-stu-id="d6f51-142">System.Data.EntityException => System.Data.**Entity.Core.** EntityException</span></span>  
- <span data-ttu-id="d6f51-143">System.Data.Objects.ObjectContext = > System.Data. **Entity.Core.** Objects.ObjectContext</span><span class="sxs-lookup"><span data-stu-id="d6f51-143">System.Data.Objects.ObjectContext => System.Data.**Entity.Core.** Objects.ObjectContext</span></span>  
- <span data-ttu-id="d6f51-144">System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core.** Objects.DataClasses.RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="d6f51-144">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core.** Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="d6f51-145">Bu tür bulunan *çekirdek* ad alanları doğrudan çoğu DbContext tabanlı uygulamalar için kullanılmadığından.</span><span class="sxs-lookup"><span data-stu-id="d6f51-145">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="d6f51-146">System.Data.Entity.dll parçası olan bazı türleri hala yaygın olarak ve doğrudan DbContext tabanlı uygulamalar için kullanılır ve bu nedenle içine taşınmamış olan *çekirdek* ad alanları.</span><span class="sxs-lookup"><span data-stu-id="d6f51-146">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="d6f51-147">Bunlar:</span><span class="sxs-lookup"><span data-stu-id="d6f51-147">These are:</span></span>

- <span data-ttu-id="d6f51-148">System.Data.EntityState = > System.Data. **Varlık.** EntityState</span><span class="sxs-lookup"><span data-stu-id="d6f51-148">System.Data.EntityState => System.Data.**Entity.** EntityState</span></span>  
- <span data-ttu-id="d6f51-149">System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**</span><span class="sxs-lookup"><span data-stu-id="d6f51-149">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="d6f51-150">Bu sınıf yeniden adlandırıldı; eski adıyla bir sınıf hala var olduğundan ve çalışır, ancak artık eski olarak işaretlendi.</span><span class="sxs-lookup"><span data-stu-id="d6f51-150">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="d6f51-151">System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**</span><span class="sxs-lookup"><span data-stu-id="d6f51-151">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="d6f51-152">Bu sınıf yeniden adlandırıldı; eski adıyla bir sınıf hala var olduğundan ve çalışır, ancak artık kullanılmayan olarak işaretlenmiş.)</span><span class="sxs-lookup"><span data-stu-id="d6f51-152">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="d6f51-153">Uzamsal sınıfları (örneğin, DbGeography, DbGeometry) System.Data.Spatial taşınmış = > System.Data. **Varlık.** Uzamsal</span><span class="sxs-lookup"><span data-stu-id="d6f51-153">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity.** Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="d6f51-154">System.Data ad alanındaki bazı türleri, EF derleme olmayan System.Data.dll olabilir.</span><span class="sxs-lookup"><span data-stu-id="d6f51-154">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="d6f51-155">Bu tür değil taşıdık ve bu nedenle, ad alanları değişmeden kalır.</span><span class="sxs-lookup"><span data-stu-id="d6f51-155">These types have not moved and so their namespaces remain unchanged.</span></span>
