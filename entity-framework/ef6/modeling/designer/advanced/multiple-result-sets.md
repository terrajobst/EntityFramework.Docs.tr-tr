---
title: Birden çok sonuç kümesi ile saklı yordamlar-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418706"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="9cfa0-102">Birden çok sonuç kümesiyle saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="9cfa0-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="9cfa0-103">Bazen saklı yordamlar kullanılırken, birden fazla sonuç kümesi döndürihtiyacınız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="9cfa0-104">Bu senaryo, tek bir ekran oluşturmak için gereken veritabanı gidiş dönüş sayısını azaltmak için yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span><span data-ttu-id="9cfa0-105"> EF5 ' dan önce, Entity Framework saklı yordamın çağrılmasına izin verir, ancak yalnızca ilk sonuç kümesini çağıran koda döndürür.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-105"> Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="9cfa0-106">Bu makalede, Entity Framework ' deki bir saklı yordamdan birden fazla sonuç kümesine erişmek için kullanabileceğiniz iki yol gösterilir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="9cfa0-107">Yalnızca kod kullanan ve hem kod hem de EF Tasarımcısı ve yalnızca EF Designer ile birlikte çalışarak çalıştırılan bir tane.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="9cfa0-108">Bu için araç ve API desteği, Entity Framework gelecek sürümlerinde iyileştirmelidir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="9cfa0-109">Model</span><span class="sxs-lookup"><span data-stu-id="9cfa0-109">Model</span></span>

<span data-ttu-id="9cfa0-110">Bu makaledeki örneklerde, bir blogun birçok gönderiye sahip olduğu ve bir gönderisinin tek bir blog 'a ait olduğu temel bir blog ve gönderi modeli kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="9cfa0-111">Veritabanında, şunun gibi tüm blogları ve gönderileri döndüren bir saklı yordam kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="9cfa0-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="9cfa0-112">Birden çok sonuç kümesine kodla erişme</span><span class="sxs-lookup"><span data-stu-id="9cfa0-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="9cfa0-113">Saklı yordamımızı yürütmek için ham bir SQL komutu vermek üzere kullanım kodu yürütüyoruz.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="9cfa0-114">Bu yaklaşımın avantajı, hem Code First hem de EF Designer ile birlikte çalışabildir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="9cfa0-115">Çalışan birden çok sonuç kümesi almak için, ıobjectcontextadapter arabirimini kullanarak ObjectContext API 'sine bırakması gerekir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="9cfa0-116">Bir ObjectContext olduktan sonra, saklı yordamımızın sonuçlarını, normal şekilde, bu şekilde izlenebilirler ve kullanılabilecek varlıklara dönüştürmek için Çevir yöntemini kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="9cfa0-117">Aşağıdaki kod örneği bu eylemi gösterir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-117">The following code sample demonstrates this in action.</span></span>

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

<span data-ttu-id="9cfa0-118">Çeviri yöntemi, yordamı, bir EntitySet adını ve bir birleştirme seçeneğini yürütüyoruz, aldığımız okuyucuyu kabul eder.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="9cfa0-119">EntitySet adı, türetilmiş bağlamınızın DbSet özelliği ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="9cfa0-120">MergeOption numaralandırması, bellekte aynı varlık zaten varsa sonuçların nasıl işlendiğini denetler.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="9cfa0-121">Burada, NextResult 'ı çağırmadan önce blogların koleksiyonunu yineliyoruz. ilk sonuç kümesi bir sonraki sonuç kümesine geçmeden önce tüketilmesi gerektiğinden Yukarıdaki kod verilmesinden önemlidir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="9cfa0-122">İki çevirme yöntemi çağrıldıktan sonra, blog ve gönderi varlıkları diğer varlıklarıyla aynı şekilde işlenir ve bu nedenle değiştirilebilir veya silinebilir ve normal şekilde kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="9cfa0-123">EF, çeviri yöntemini kullanarak varlıkları oluşturduğunda, hiçbir eşleme almaz.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="9cfa0-124">Bu, sonuç kümesindeki sütun adlarını sınıflarınızda Özellik adlarıyla eşleştirecek.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="9cfa0-125">Yavaş yükleme özelliği etkinse, blog varlıklarından birindeki gönderi özelliğine eriştiğinizde EF, tümünü zaten yüklemiş olduğumuz halde tüm gönderileri geç yüklemek için veritabanına bağlanır.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="9cfa0-126">Bunun nedeni, EF 'in tüm gönderilerinizi mi yüklediklerini yoksa ya da veritabanında daha fazla olup olmadığını bilemez.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="9cfa0-127">Bundan kaçınmak isterseniz, geç yüklemeyi devre dışı bırakmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="9cfa0-128">EDMX içinde yapılandırılmış birden çok sonuç kümesi</span><span class="sxs-lookup"><span data-stu-id="9cfa0-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="9cfa0-129">EDMX 'te birden çok sonuç kümesi yapılandırabilmek için .NET Framework 4,5 ' i hedeflemelidir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="9cfa0-130">.NET 4,0 ' i hedefliyorsanız, önceki bölümde gösterilen kod tabanlı yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="9cfa0-131">EF Designer kullanıyorsanız, modelinizi, döndürülecek farklı sonuç kümelerini bilmesi için de değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="9cfa0-132">Her şey, araçları birden çok sonuç kümesi olmadan önce bilmeniz, bu nedenle edmx dosyasını el ile düzenlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="9cfa0-133">Bu dosya gibi edmx dosyasını düzenleme işi çalışır, ancak aynı zamanda, VS 'deki modelin doğrulanmasını de keser.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="9cfa0-134">Bu nedenle modelinizi doğrulamanız durumunda her zaman hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="9cfa0-135">Bunu yapmak için, tek bir sonuç kümesi sorgusuyla yaptığınız gibi modelinize saklı yordamı eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="9cfa0-136">Bunu yaptıktan sonra modelinize sağ tıklayıp **birlikte Aç** ' ı seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="9cfa0-137">sonra **XML**</span><span class="sxs-lookup"><span data-stu-id="9cfa0-137">then **Xml**</span></span>

    ![Farklı aç](~/ef6/media/openas.png)

<span data-ttu-id="9cfa0-139">Modeli XML olarak açtıktan sonra, aşağıdaki adımları gerçekleştirmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="9cfa0-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="9cfa0-140">Modelinizde karmaşık tür ve işlev içeri aktarmayı bulun:</span><span class="sxs-lookup"><span data-stu-id="9cfa0-140">Find the complex type and function import in your model:</span></span>

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   <span data-ttu-id="9cfa0-141">Karmaşık türü kaldır</span><span class="sxs-lookup"><span data-stu-id="9cfa0-141">Remove the complex type</span></span>
-   <span data-ttu-id="9cfa0-142">İşlev içeri aktarmayı, varlıklarınıza eşlenecek şekilde güncelleştirin, bu durumda aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="9cfa0-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="9cfa0-143">Bu, modele saklı yordamın iki koleksiyon döndürmeyeceğini, blog girişlerinden birini ve post girişlerinden birini söyler.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="9cfa0-144">İşlev eşleme öğesini bul:</span><span class="sxs-lookup"><span data-stu-id="9cfa0-144">Find the function mapping element:</span></span>

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   <span data-ttu-id="9cfa0-145">Sonuç eşlemesini, döndürülmekte olan her varlık için bir ile değiştirin; Örneğin, aşağıdaki gibi:</span><span class="sxs-lookup"><span data-stu-id="9cfa0-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

<span data-ttu-id="9cfa0-146">Sonuç kümelerinin, varsayılan olarak oluşturulan bir gibi karmaşık türlerle eşleşmesi de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="9cfa0-147">Bunu yapmak için, bunları kaldırmak yerine yeni bir karmaşık tür oluşturun ve Yukarıdaki örneklerde varlık adlarını kullandığınız her yerde karmaşık türleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="9cfa0-148">Bu eşlemeler değiştirildikten sonra modeli kaydedebilir ve saklı yordamı kullanmak için aşağıdaki kodu çalıştırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9cfa0-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> <span data-ttu-id="9cfa0-149">Modelinize ilişkin edmx dosyasını el ile düzenlerseniz, modeli veritabanından yeniden oluşturduysanız üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="9cfa0-150">Özet</span><span class="sxs-lookup"><span data-stu-id="9cfa0-150">Summary</span></span>

<span data-ttu-id="9cfa0-151">Burada, birden çok sonuç kümesine Entity Framework kullanarak erişmenin iki farklı yöntemi gösterildik.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="9cfa0-152">Her ikisi de durumunuza ve tercihlerinize göre eşit oranda geçerlidir ve koşullarınız için en iyi görünen birini seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="9cfa0-153">Birden çok sonuç kümesi desteğinin, Entity Framework gelecek sürümlerinde iyileştirilen ve bu belgedeki adımların gerçekleştirilmesi artık gerekli olmayacak şekilde planlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="9cfa0-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
