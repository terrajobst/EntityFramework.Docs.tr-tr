---
title: Önceden oluşturulmuş eşleme görünümleri-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419393"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="4c196-102">Önceden oluşturulmuş eşleme görünümleri</span><span class="sxs-lookup"><span data-stu-id="4c196-102">Pre-generated mapping views</span></span>
<span data-ttu-id="4c196-103">Entity Framework bir sorgu yürütmeden veya veri kaynağında değişiklikler kaydedebilmesi için, veritabanına erişmek üzere bir eşleme görünümleri kümesi oluşturması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c196-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="4c196-104">Bu eşleme görünümleri, veritabanını soyut bir şekilde temsil eden bir Entity SQL deyimidir ve uygulama etki alanı başına önbelleğe alınan meta verilerin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c196-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="4c196-105">Aynı uygulama etki alanında aynı bağlamın birden çok örneğini oluşturursanız, eşleme görünümlerini yeniden oluşturmak yerine önbelleğe alınmış meta verilerden yeniden kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="4c196-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="4c196-106">Eşleme görünümü oluşturma, ilk sorguyu yürütmenin genel maliyetinin önemli bir parçası olduğundan, Entity Framework eşleme görünümlerini önceden oluşturmanızı ve bunları derlenmiş projeye eklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="4c196-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span><span data-ttu-id="4c196-107"> Daha fazla bilgi için bkz.  [performans konuları (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="4c196-107"> For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a><span data-ttu-id="4c196-108">EF Power Tools Community Edition ile eşleme görünümleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="4c196-108">Generating Mapping Views with the EF Power Tools Community Edition</span></span>

<span data-ttu-id="4c196-109">Görünümleri önceden oluşturmanın en kolay yolu, [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition)' ı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="4c196-109">The easiest way to pre-generate views is to use the [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span></span> <span data-ttu-id="4c196-110">Power Tools yüklendikten sonra, görünümler oluşturmak için aşağıdaki gibi bir menü seçeneğine sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="4c196-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="4c196-111">**Code First** modelleri Için DbContext sınıfınızı içeren kod dosyasına sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4c196-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="4c196-112">**EF Designer** modellerı için edmx dosyanıza sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4c196-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![Görünüm Oluştur](~/ef6/media/generateviews.png)

<span data-ttu-id="4c196-114">İşlem tamamlandıktan sonra, aşağıdakine benzer bir sınıfa sahip olursunuz</span><span class="sxs-lookup"><span data-stu-id="4c196-114">Once the process is finished you will have a class similar to the following generated</span></span>

![oluşturulan görünümler](~/ef6/media/generatedviews.png)

<span data-ttu-id="4c196-116">Artık uygulamanızı çalıştırdığınız zaman, görünümleri gereken şekilde yüklemek için bu sınıfı kullanır.</span><span class="sxs-lookup"><span data-stu-id="4c196-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="4c196-117">Modeliniz değişirse ve bu sınıfı yeniden oluşturmayın, EF bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4c196-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="4c196-118">Code-EF6 Onlenlerden eşleme görünümleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="4c196-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="4c196-119">Görünümler oluşturmanın diğer yolu, EF 'in sağladığı API 'Leri kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="4c196-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="4c196-120">Bu yöntemi kullanırken, görünümleri ancak istediğiniz şekilde serileştirme özgürlüğü vardır ancak görünümleri de yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c196-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="4c196-121">**Yalnızca EF6** , bu bölümde gösterilen apı 'ler Entity Framework 6 ' da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="4c196-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="4c196-122">Daha önceki bir sürümü kullanıyorsanız bu bilgiler uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="4c196-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="4c196-123">Görünüm Oluşturma</span><span class="sxs-lookup"><span data-stu-id="4c196-123">Generating Views</span></span>

<span data-ttu-id="4c196-124">Görünümler oluşturmak için API 'Ler System. Data. Entity. Core. Mapping. StorageMappingItemCollection sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="4c196-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="4c196-125">Bir ObjectContext 'in MetadataWorkspace 'i kullanarak bir bağlam için StorageMappingCollection alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c196-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="4c196-126">Daha yeni DbContext API 'sini kullanıyorsanız, aşağıdaki gibi ıobjectcontextadapter kullanarak buna erişebilirsiniz. Bu kodda, dbContext adlı türetilmiş DbContext 'in bir örneği sunuyoruz:</span><span class="sxs-lookup"><span data-stu-id="4c196-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="4c196-127">StorageMappingItemCollection 'ı aldıktan sonra GenerateViews ve ComputeMappingHashValue yöntemlerine erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c196-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="4c196-128">İlk yöntem, kapsayıcı eşlemesindeki her görünüm için bir girişi olan bir sözlük oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4c196-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="4c196-129">İkinci yöntem, tek bir kapsayıcı eşlemesi için bir karma değer hesaplar ve çalışma zamanında, görünümlerin önceden oluşturulmasından bu yana modelin değiştirilmediğini doğrulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4c196-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="4c196-130">Birden çok kapsayıcı eşlemelerini kapsayan karmaşık senaryolar için iki yöntemin geçersiz kılmaları sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4c196-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="4c196-131">Görünümler oluştururken GenerateViews metodunu çağırıp elde edilen EntitySetBase ve DbMappingView ' u yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4c196-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="4c196-132">Ayrıca, ComputeMappingHashValue yöntemi tarafından oluşturulan karmayı depolamanız gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="4c196-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="4c196-133">Görünümler yükleniyor</span><span class="sxs-lookup"><span data-stu-id="4c196-133">Loading Views</span></span>

<span data-ttu-id="4c196-134">GenerateViews yöntemi tarafından oluşturulan görünümleri yüklemek için DbMappingViewCache soyut sınıfından devralan bir sınıf ile EF sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c196-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="4c196-135">DbMappingViewCache, uygulamanız gereken iki yöntemi belirtir:</span><span class="sxs-lookup"><span data-stu-id="4c196-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="4c196-136">MappingHashValue özelliği, ComputeMappingHashValue yöntemi tarafından oluşturulan karmayı döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c196-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="4c196-137">EF, görünümler için sorulacak olduğunda, önce bu özellik tarafından döndürülen karma ile modelin karma değerini oluşturur ve karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="4c196-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="4c196-138">Eşleşiyorlarsa, EF bir EntityCommandCompilationException özel durumu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4c196-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="4c196-139">GetView yöntemi bir EntitySetBase kabul eder ve GenerateViews yöntemi tarafından oluşturulan sözlükte verilen EntitySetBase ile ilişkili olan için oluşturulan EntitySql içeren bir DbMappingVIew döndürmeli.</span><span class="sxs-lookup"><span data-stu-id="4c196-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="4c196-140">EF, sahip olmayan bir görünüm isterse, GetView null döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="4c196-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="4c196-141">Aşağıda açıklandığı gibi güç araçlarıyla oluşturulan DbMappingViewCache öğesinden ayıklama işlemi aşağıdaki gibidir. Bu, içinde EntitySql 'i depolamanın ve almanın bir yolunu görebiliyoruz.</span><span class="sxs-lookup"><span data-stu-id="4c196-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

<span data-ttu-id="4c196-142">EF 'in DbMappingViewCache 'i kullanması için, eklediğiniz bağlamı belirterek DbMappingViewCacheTypeAttribute öğesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="4c196-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="4c196-143">Aşağıdaki kodda, BlogContext MyMappingViewCache sınıfıyla ilişkilendiririz.</span><span class="sxs-lookup"><span data-stu-id="4c196-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="4c196-144">Daha karmaşık senaryolar için eşleme görünümü önbellek örnekleri, bir eşleme görünümü önbellek fabrikası belirtilerek sağlanarak temin edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4c196-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="4c196-145">Bu, System. Data. Entity. Infrastructure. MappingViews. DbMappingViewCacheFactory olan soyut Class.</span><span class="sxs-lookup"><span data-stu-id="4c196-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="4c196-146">Kullanılan eşleme görünümü önbellek fabrikası örneği, StorageMappingItemCollection. MappingViewCacheFactoryproperty kullanılarak alınabilir veya ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="4c196-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
