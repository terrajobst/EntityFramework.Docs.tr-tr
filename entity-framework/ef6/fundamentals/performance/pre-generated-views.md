---
title: Önceden oluşturulan eşleme görünümleri - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 397569ef374cb44d4938f9e201b588a26c408f6e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996478"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="c02f3-102">Önceden oluşturulan eşleme görünümleri</span><span class="sxs-lookup"><span data-stu-id="c02f3-102">Pre-generated mapping views</span></span>
<span data-ttu-id="c02f3-103">Entity Framework, bir sorgu yürütme veya değişiklikleri veri kaynağına kaydetmek için önce bunun veritabanına erişmek için eşleme görünümü kümesi oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="c02f3-104">Bu eşleme görünümler veritabanı soyut bir şekilde temsil eden varlık SQL deyimi bir dizi ve uygulama etki alanı başına önbelleğe alınan meta veriler bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="c02f3-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="c02f3-105">Aynı uygulama etki alanında birden fazla aynı bağlam oluşturursanız, bunları yeniden yerine önbelleğe alınan meta veri eşleme görünümleri yeniden kullanır.</span><span class="sxs-lookup"><span data-stu-id="c02f3-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="c02f3-106">Eşleme görünümü oluşturma ilk sorgu yürütülürken ilişkin genel maliyeti önemli bir parçası olduğundan, Entity Framework, eşleme görünümleri önceden oluşturmak ve bunları derlenmiş projeye dahil etmek sağlar.</span><span class="sxs-lookup"><span data-stu-id="c02f3-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="c02f3-107">Daha fazla bilgi için [başarım düşünceleri (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="c02f3-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools"></a><span data-ttu-id="c02f3-108">EF güç araçları görünümleriyle eşleme oluşturma</span><span class="sxs-lookup"><span data-stu-id="c02f3-108">Generating Mapping Views with the EF Power Tools</span></span>

<span data-ttu-id="c02f3-109">Görünümleri önceden oluşturulacak en kolay yolu kullanmaktır [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span><span class="sxs-lookup"><span data-stu-id="c02f3-109">The easiest way to pre-generate views is to use the [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span></span> <span data-ttu-id="c02f3-110">Güç araçlarının yüklü sonra aşağıda gösterildiği gibi görünümler oluşturmak için bir menü seçeneğine sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="c02f3-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="c02f3-111">İçin **Code First** modelleri DbContext sınıfınızı içeren kod dosyasını sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c02f3-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="c02f3-112">İçin **EF Designer** modelleri EDMX dosyanız sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c02f3-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generateViews](~/ef6/media/generateviews.png)

<span data-ttu-id="c02f3-114">İşlem tamamlandıktan sonra oluşturulan aşağıdakine benzer bir sınıf olması</span><span class="sxs-lookup"><span data-stu-id="c02f3-114">Once the process is finished you will have a class similar to the following generated</span></span>

![generatedViews](~/ef6/media/generatedviews.png)

<span data-ttu-id="c02f3-116">Programını çalıştırdığınızda artık uygulamanızı EF Bu sınıf görünüm gerekli olarak yüklemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="c02f3-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="c02f3-117">Model değişikliklerinizi ve bu sınıf yeniden oluşturmaz varsa EF bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c02f3-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="c02f3-118">Kod - EF6 sonrası görünümleri eşleme oluşturma</span><span class="sxs-lookup"><span data-stu-id="c02f3-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="c02f3-119">Görünümleri oluşturmak için başka bir şekilde EF sağlayan API'ler kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="c02f3-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="c02f3-120">Bu yöntemi kullanırken görünümleri istediğiniz, ancak kendiniz yükleme görünümleri gerekir ancak seri hale getirmek için özgürlüğüne sahipsiniz.</span><span class="sxs-lookup"><span data-stu-id="c02f3-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="c02f3-121">**EF6 ve sonraki sürümler yalnızca** -Bu bölümde gösterilen API'leri, Entity Framework 6'da sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="c02f3-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="c02f3-122">Önceki bir sürümü kullanıyorsanız, bu bilgileri geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="c02f3-123">Görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="c02f3-123">Generating Views</span></span>

<span data-ttu-id="c02f3-124">Görünümlerin System.Data.Entity.Core.Mapping.StorageMappingItemCollection sınıfında apı'lerdir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="c02f3-125">Bir Objectcontext'e MetadataWorkspace kullanarak bir bağlam için bir StorageMappingCollection alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c02f3-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="c02f3-126">Bunu kullanarak erişebilirsiniz sonra yeni DbContext API kullanıyorsanız bu kodda, türetilmiş DbContext dbContext adlı örneği sahibiz, IObjectContextAdapter gibi aşağıda:</span><span class="sxs-lookup"><span data-stu-id="c02f3-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="c02f3-127">StorageMappingItemCollection oluşturduktan sonra erişim GenerateViews ve ComputeMappingHashValue yöntemlerine alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c02f3-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="c02f3-128">İlk yöntem, kapsayıcı eşlemesindeki her görünüm için bir girişi ile bir sözlük oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c02f3-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="c02f3-129">İkinci yöntem tek kapsayıcısı eşlemesine yönelik bir karma değeri hesaplar ve çalışma zamanında doğrulamak için model görünümleri önceden oluşturulan bu yana değişmemiştir kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c02f3-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="c02f3-130">Geçersiz kılmalar iki yöntemden biriyle birden çok kapsayıcı eşlemeleri içeren karmaşık senaryolar için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c02f3-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="c02f3-131">Görünümleri oluştururken GenerateViews yöntemini çağırın ve ardından elde edilen EntitySetBase ve DbMappingView öğrenmek yazın.</span><span class="sxs-lookup"><span data-stu-id="c02f3-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="c02f3-132">ComputeMappingHashValue yöntemi tarafından oluşturulur karmasını depolamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="c02f3-133">Görünümler yükleniyor</span><span class="sxs-lookup"><span data-stu-id="c02f3-133">Loading Views</span></span>

<span data-ttu-id="c02f3-134">Yükleme GenerateViews yöntemi tarafından oluşturulan görünümleri için DbMappingViewCache soyut sınıfından devralan bir sınıf ile EF sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="c02f3-135">Uygulamanız gereken iki yöntem DbMappingViewCache belirtir:</span><span class="sxs-lookup"><span data-stu-id="c02f3-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="c02f3-136">MappingHashValue özelliği ComputeMappingHashValue yöntemi tarafından oluşturulur karmasını döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="c02f3-137">EF olduğunda görünümleri, sormak için devam eden ilk oluşturmak ve bu özellik tarafından döndürülen karma model karma değerini karşılaştırır.</span><span class="sxs-lookup"><span data-stu-id="c02f3-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="c02f3-138">Bunlar eşleşmiyorsa, EF EntityCommandCompilationException bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c02f3-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="c02f3-139">GetView yöntemi bir EntitySetBase kabul eder ve için oluşturulan EntitySql içeren bir DbMappingVIew GenerateViews yöntemi tarafından oluşturulan sözlükteki belirtilen EntitySetBase ilişkilendirilmiş döndürülecek gerekir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="c02f3-140">EF için isterse, ardından GetView olmayan bir görünüm null değeri döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="c02f3-141">Yukarıda, depolamak ve gerekli EntitySql almak için bir yol görüyoruz açıklandığı gibi güç araçları ile oluşturulan DbMappingViewCache bir ayıklayın verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="c02f3-142">Eklediğiniz, DbMappingViewCache EF kullanması için için oluşturulan bağlamı belirtme DbMappingViewCacheTypeAttribute kullanın.</span><span class="sxs-lookup"><span data-stu-id="c02f3-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="c02f3-143">Aşağıdaki kod biz BlogContext MyMappingViewCache sınıfı ile ilişkilendirin.</span><span class="sxs-lookup"><span data-stu-id="c02f3-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="c02f3-144">Daha karmaşık senaryolarda, eşleme görünümü önbellek örnekleri bir eşleme görünümü önbellek fabrikası belirterek sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="c02f3-145">Bu Özet sınıf System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory uygulayarak yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="c02f3-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="c02f3-146">Kullanılan eşleme görünümü önbellek factory örneğini alınan veya StorageMappingItemCollection.MappingViewCacheFactoryproperty kullanarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c02f3-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
