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
# <a name="pre-generated-mapping-views"></a>Önceden oluşturulmuş eşleme görünümleri
Entity Framework bir sorgu yürütmeden veya veri kaynağında değişiklikler kaydedebilmesi için, veritabanına erişmek üzere bir eşleme görünümleri kümesi oluşturması gerekir. Bu eşleme görünümleri, veritabanını soyut bir şekilde temsil eden bir Entity SQL deyimidir ve uygulama etki alanı başına önbelleğe alınan meta verilerin bir parçasıdır. Aynı uygulama etki alanında aynı bağlamın birden çok örneğini oluşturursanız, eşleme görünümlerini yeniden oluşturmak yerine önbelleğe alınmış meta verilerden yeniden kullanacaktır. Eşleme görünümü oluşturma, ilk sorguyu yürütmenin genel maliyetinin önemli bir parçası olduğundan, Entity Framework eşleme görünümlerini önceden oluşturmanızı ve bunları derlenmiş projeye eklemenizi sağlar. Daha fazla bilgi için bkz.  [performans konuları (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>EF Power Tools Community Edition ile eşleme görünümleri oluşturma

Görünümleri önceden oluşturmanın en kolay yolu, [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition)' ı kullanmaktır. Power Tools yüklendikten sonra, görünümler oluşturmak için aşağıdaki gibi bir menü seçeneğine sahip olursunuz.

-   **Code First** modelleri Için DbContext sınıfınızı içeren kod dosyasına sağ tıklayın.
-   **EF Designer** modellerı için edmx dosyanıza sağ tıklayın.

![Görünüm Oluştur](~/ef6/media/generateviews.png)

İşlem tamamlandıktan sonra, aşağıdakine benzer bir sınıfa sahip olursunuz

![oluşturulan görünümler](~/ef6/media/generatedviews.png)

Artık uygulamanızı çalıştırdığınız zaman, görünümleri gereken şekilde yüklemek için bu sınıfı kullanır. Modeliniz değişirse ve bu sınıfı yeniden oluşturmayın, EF bir özel durum oluşturur.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Code-EF6 Onlenlerden eşleme görünümleri oluşturma

Görünümler oluşturmanın diğer yolu, EF 'in sağladığı API 'Leri kullanmaktır. Bu yöntemi kullanırken, görünümleri ancak istediğiniz şekilde serileştirme özgürlüğü vardır ancak görünümleri de yüklemeniz gerekir.

> [!NOTE]
> **Yalnızca EF6** , bu bölümde gösterilen apı 'ler Entity Framework 6 ' da sunulmuştur. Daha önceki bir sürümü kullanıyorsanız bu bilgiler uygulanmaz.

### <a name="generating-views"></a>Görünüm Oluşturma

Görünümler oluşturmak için API 'Ler System. Data. Entity. Core. Mapping. StorageMappingItemCollection sınıfıdır. Bir ObjectContext 'in MetadataWorkspace 'i kullanarak bir bağlam için StorageMappingCollection alabilirsiniz. Daha yeni DbContext API 'sini kullanıyorsanız, aşağıdaki gibi ıobjectcontextadapter kullanarak buna erişebilirsiniz. Bu kodda, dbContext adlı türetilmiş DbContext 'in bir örneği sunuyoruz:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

StorageMappingItemCollection 'ı aldıktan sonra GenerateViews ve ComputeMappingHashValue yöntemlerine erişebilirsiniz.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

İlk yöntem, kapsayıcı eşlemesindeki her görünüm için bir girişi olan bir sözlük oluşturur. İkinci yöntem, tek bir kapsayıcı eşlemesi için bir karma değer hesaplar ve çalışma zamanında, görünümlerin önceden oluşturulmasından bu yana modelin değiştirilmediğini doğrulamak için kullanılır. Birden çok kapsayıcı eşlemelerini kapsayan karmaşık senaryolar için iki yöntemin geçersiz kılmaları sağlanır.

Görünümler oluştururken GenerateViews metodunu çağırıp elde edilen EntitySetBase ve DbMappingView ' u yazacaksınız. Ayrıca, ComputeMappingHashValue yöntemi tarafından oluşturulan karmayı depolamanız gerekecektir.

### <a name="loading-views"></a>Görünümler yükleniyor

GenerateViews yöntemi tarafından oluşturulan görünümleri yüklemek için DbMappingViewCache soyut sınıfından devralan bir sınıf ile EF sağlayabilirsiniz. DbMappingViewCache, uygulamanız gereken iki yöntemi belirtir:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

MappingHashValue özelliği, ComputeMappingHashValue yöntemi tarafından oluşturulan karmayı döndürmelidir. EF, görünümler için sorulacak olduğunda, önce bu özellik tarafından döndürülen karma ile modelin karma değerini oluşturur ve karşılaştırın. Eşleşiyorlarsa, EF bir EntityCommandCompilationException özel durumu oluşturur.

GetView yöntemi bir EntitySetBase kabul eder ve GenerateViews yöntemi tarafından oluşturulan sözlükte verilen EntitySetBase ile ilişkili olan için oluşturulan EntitySql içeren bir DbMappingVIew döndürmeli. EF, sahip olmayan bir görünüm isterse, GetView null döndürmelidir.

Aşağıda açıklandığı gibi güç araçlarıyla oluşturulan DbMappingViewCache öğesinden ayıklama işlemi aşağıdaki gibidir. Bu, içinde EntitySql 'i depolamanın ve almanın bir yolunu görebiliyoruz.

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

EF 'in DbMappingViewCache 'i kullanması için, eklediğiniz bağlamı belirterek DbMappingViewCacheTypeAttribute öğesini kullanın. Aşağıdaki kodda, BlogContext MyMappingViewCache sınıfıyla ilişkilendiririz.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Daha karmaşık senaryolar için eşleme görünümü önbellek örnekleri, bir eşleme görünümü önbellek fabrikası belirtilerek sağlanarak temin edilebilir. Bu, System. Data. Entity. Infrastructure. MappingViews. DbMappingViewCacheFactory olan soyut Class. Kullanılan eşleme görünümü önbellek fabrikası örneği, StorageMappingItemCollection. MappingViewCacheFactoryproperty kullanılarak alınabilir veya ayarlanabilir.
