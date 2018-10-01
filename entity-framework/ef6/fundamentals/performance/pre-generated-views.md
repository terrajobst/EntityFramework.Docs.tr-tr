---
title: Önceden oluşturulan eşleme görünümleri - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459532"
---
# <a name="pre-generated-mapping-views"></a>Önceden oluşturulan eşleme görünümleri
Entity Framework, bir sorgu yürütme veya değişiklikleri veri kaynağına kaydetmek için önce bunun veritabanına erişmek için eşleme görünümü kümesi oluşturmanız gerekir. Bu eşleme görünümler veritabanı soyut bir şekilde temsil eden varlık SQL deyimi bir dizi ve uygulama etki alanı başına önbelleğe alınan meta veriler bir parçasıdır. Aynı uygulama etki alanında birden fazla aynı bağlam oluşturursanız, bunları yeniden yerine önbelleğe alınan meta veri eşleme görünümleri yeniden kullanır. Eşleme görünümü oluşturma ilk sorgu yürütülürken ilişkin genel maliyeti önemli bir parçası olduğundan, Entity Framework, eşleme görünümleri önceden oluşturmak ve bunları derlenmiş projeye dahil etmek sağlar. Daha fazla bilgi için [başarım düşünceleri (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>EF güç araçları Community Edition görünümleriyle eşleme oluşturma

Görünümleri önceden oluşturulacak en kolay yolu kullanmaktır [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition). Güç araçlarının yüklü sonra aşağıda gösterildiği gibi görünümler oluşturmak için bir menü seçeneğine sahip olursunuz.

-   İçin **Code First** modelleri DbContext sınıfınızı içeren kod dosyasını sağ tıklayın.
-   İçin **EF Designer** modelleri EDMX dosyanız sağ tıklayın.

![Görünümler oluşturma](~/ef6/media/generateviews.png)

İşlem tamamlandıktan sonra oluşturulan aşağıdakine benzer bir sınıf olması

![üretilmiş görünümleri](~/ef6/media/generatedviews.png)

Programını çalıştırdığınızda artık uygulamanızı EF Bu sınıf görünüm gerekli olarak yüklemek için kullanır. Model değişikliklerinizi ve bu sınıf yeniden oluşturmaz varsa EF bir özel durum oluşturur.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Kod - EF6 sonrası görünümleri eşleme oluşturma

Görünümleri oluşturmak için başka bir şekilde EF sağlayan API'ler kullanmaktır. Bu yöntemi kullanırken görünümleri istediğiniz, ancak kendiniz yükleme görünümleri gerekir ancak seri hale getirmek için özgürlüğüne sahipsiniz.

> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -Bu bölümde gösterilen API'leri, Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bu bilgileri geçerli değildir.

### <a name="generating-views"></a>Görünüm oluşturma

Görünümlerin System.Data.Entity.Core.Mapping.StorageMappingItemCollection sınıfında apı'lerdir. Bir Objectcontext'e MetadataWorkspace kullanarak bir bağlam için bir StorageMappingCollection alabilirsiniz. Bunu kullanarak erişebilirsiniz sonra yeni DbContext API kullanıyorsanız bu kodda, türetilmiş DbContext dbContext adlı örneği sahibiz, IObjectContextAdapter gibi aşağıda:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

StorageMappingItemCollection oluşturduktan sonra erişim GenerateViews ve ComputeMappingHashValue yöntemlerine alabilirsiniz.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

İlk yöntem, kapsayıcı eşlemesindeki her görünüm için bir girişi ile bir sözlük oluşturur. İkinci yöntem tek kapsayıcısı eşlemesine yönelik bir karma değeri hesaplar ve çalışma zamanında doğrulamak için model görünümleri önceden oluşturulan bu yana değişmemiştir kullanılır. Geçersiz kılmalar iki yöntemden biriyle birden çok kapsayıcı eşlemeleri içeren karmaşık senaryolar için sağlanır.

Görünümleri oluştururken GenerateViews yöntemini çağırın ve ardından elde edilen EntitySetBase ve DbMappingView öğrenmek yazın. ComputeMappingHashValue yöntemi tarafından oluşturulur karmasını depolamak gerekir.

### <a name="loading-views"></a>Görünümler yükleniyor

Yükleme GenerateViews yöntemi tarafından oluşturulan görünümleri için DbMappingViewCache soyut sınıfından devralan bir sınıf ile EF sağlayabilir. Uygulamanız gereken iki yöntem DbMappingViewCache belirtir:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

MappingHashValue özelliği ComputeMappingHashValue yöntemi tarafından oluşturulur karmasını döndürmesi gerekir. EF olduğunda görünümleri, sormak için devam eden ilk oluşturmak ve bu özellik tarafından döndürülen karma model karma değerini karşılaştırır. Bunlar eşleşmiyorsa, EF EntityCommandCompilationException bir özel durum oluşturur.

GetView yöntemi bir EntitySetBase kabul eder ve için oluşturulan EntitySql içeren bir DbMappingVIew GenerateViews yöntemi tarafından oluşturulan sözlükteki belirtilen EntitySetBase ilişkilendirilmiş döndürülecek gerekir. EF için isterse, ardından GetView olmayan bir görünüm null değeri döndürmelidir.

Yukarıda, depolamak ve gerekli EntitySql almak için bir yol görüyoruz açıklandığı gibi güç araçları ile oluşturulan DbMappingViewCache bir ayıklayın verilmiştir.

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

Eklediğiniz, DbMappingViewCache EF kullanması için için oluşturulan bağlamı belirtme DbMappingViewCacheTypeAttribute kullanın. Aşağıdaki kod biz BlogContext MyMappingViewCache sınıfı ile ilişkilendirin.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Daha karmaşık senaryolarda, eşleme görünümü önbellek örnekleri bir eşleme görünümü önbellek fabrikası belirterek sağlanabilir. Bu Özet sınıf System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory uygulayarak yapılabilir. Kullanılan eşleme görünümü önbellek factory örneğini alınan veya StorageMappingItemCollection.MappingViewCacheFactoryproperty kullanarak ayarlayın.
