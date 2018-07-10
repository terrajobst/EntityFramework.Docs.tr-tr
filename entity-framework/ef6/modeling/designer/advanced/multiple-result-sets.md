---
title: Saklı yordamları birden çok sonuç kümesi - EF6 ile
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
caps.latest.revision: 3
ms.openlocfilehash: 68d544b0c553868ad1ff36cd24db19cff08db073
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912858"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="c48c8-102">Birden çok sonuç kümesi saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="c48c8-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="c48c8-103">Bazen kullanarak saklı yordamları birden fazla sonuç döndürmesi gerekir ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="c48c8-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="c48c8-104">Bu senaryo veritabanı sayısını azaltmak için yaygın olarak kullanılan gidiş dönüş tek bir ekran oluşturmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="c48c8-105">Entity Framework EF5 önce çağrılacak saklı yordamı çalıştırmasına olanak tanır ancak yalnızca ilk sonuç çağrıldığı koda kümesini döndürür.</span><span class="sxs-lookup"><span data-stu-id="c48c8-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="c48c8-106">Bu makale, birden fazla sonuç kümesi saklı yordam, Entity Framework erişmek için kullanabileceğiniz iki yolu gösterir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="c48c8-107">Yalnızca kod kullanır ve her iki koduyla önce çalışır ve EF Designer diğeri, yalnızca EF Designer ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="c48c8-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="c48c8-108">Gelecekte araçları ve API desteği Entity Framework sürümlerini geliştirmek.</span><span class="sxs-lookup"><span data-stu-id="c48c8-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="c48c8-109">Model</span><span class="sxs-lookup"><span data-stu-id="c48c8-109">Model</span></span>

<span data-ttu-id="c48c8-110">Bu makaledeki örneklerde temel bir Blog kullanın ve tek bir blog gönderileri modeli blog birçok gönderileri ve bir gönderi sahip olduğu ait.</span><span class="sxs-lookup"><span data-stu-id="c48c8-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="c48c8-111">Tüm blog ve gönderi şuna benzeyen döndürür veritabanında bir saklı yordam kullanacağız:</span><span class="sxs-lookup"><span data-stu-id="c48c8-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="c48c8-112">Birden çok sonuç erişme koduyla ayarlar</span><span class="sxs-lookup"><span data-stu-id="c48c8-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="c48c8-113">Size sunduğumuz saklı yordamı yürütmek için ham SQL komutu için kullanmak kod yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="c48c8-114">Bu yaklaşımın avantajı, her iki kod ile ilk çalışıyor olması ve EF Designer.</span><span class="sxs-lookup"><span data-stu-id="c48c8-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="c48c8-115">Birden çok sonuç almak için çalışma IObjectContextAdapter arabirimini kullanarak ObjectContext API'sı tarafından açılan ihtiyacımız ayarlar.</span><span class="sxs-lookup"><span data-stu-id="c48c8-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="c48c8-116">Biz bir Objectcontext'e tamamladıktan sonra Translate yöntemi izlenen ve EF normal olarak kullanılan varlıklar bizim saklı yordamının sonuçları küçültmesini kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="c48c8-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="c48c8-117">Aşağıdaki kod örneği, bu eylem gösterir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-117">The following code sample demonstrates this in action.</span></span>

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

<span data-ttu-id="c48c8-118">Translate yöntemi biz yordamı, bir EntitySet adı ve bir MergeOption yürütüldüğünde, aldık okuyucunun kabul eder.</span><span class="sxs-lookup"><span data-stu-id="c48c8-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="c48c8-119">EntitySet adı türetilmiş Bağlamınızı olan DB özelliği ile aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c48c8-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="c48c8-120">Bellekte aynı varlık zaten varsa, sonuçları nasıl işleneceğini MergeOption enum denetler.</span><span class="sxs-lookup"><span data-stu-id="c48c8-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="c48c8-121">Burada ilk sonuç kümesi için sonraki sonuç kümesini taşımadan önce kullanılması gerekir çünkü biz NextResult, yukarıdaki kod verilen önemli budur çağırmadan önce biz blogları, koleksiyonda gezin.</span><span class="sxs-lookup"><span data-stu-id="c48c8-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="c48c8-122">İki Çevir sonra yöntemleri sonra Blog ve gönderi varlıkların herhangi bir varlık aynı şekilde EF tarafından izlenir ve değiştirilebilir veya silindi ve normal olarak kaydedilen çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c48c8-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="c48c8-123">Translate yöntemi kullanarak varlıkları oluşturduğunda EF hiçbir eşlemeyle dikkate almaz.</span><span class="sxs-lookup"><span data-stu-id="c48c8-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="c48c8-124">Yalnızca sonuç sınıflarınızı üzerinde özellik adları ile kümesini sütun adları aynı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c48c8-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="c48c8-125">Blog varlıklardan biri gönderileri özelliği erişim etkinse, yavaş yükleniyor varsa zaten tümünü yüklendik olsa da ardından EF gevşek tüm gönderileri yüklemek için veritabanına bağlanmanızı.</span><span class="sxs-lookup"><span data-stu-id="c48c8-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="c48c8-126">EF tüm gönderileri yüklü veya olup olmadığını veritabanında daha fazla biliyor olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="c48c8-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="c48c8-127">Ardından, bu durumu önlemek istiyorsanız yavaş yükleniyor devre dışı bırakmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="c48c8-128">Yapılandırılmış EDMX ile birden çok sonuç kümesi</span><span class="sxs-lookup"><span data-stu-id="c48c8-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="c48c8-129">.NET Framework 4. 5'i EDMX içinde birden çok sonuç kümesi yapılandırmak için hedeflemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="c48c8-130">.NET 4.0 hedefliyorsanız, önceki bölümde gösterilen kod tabanlı yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c48c8-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="c48c8-131">EF Designer kullanıyorsanız, böylece döndürülecek farklı sonuç kümeleri hakkında bilmesi modelinizi değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c48c8-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="c48c8-132">Edmx dosyasını el ile düzenlemek ihtiyacınız olacak şekilde elle araçları birden çok sonuç değil önce bilmeniz gereken bir şey kullanan, ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c48c8-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="c48c8-133">Bir edmx dosyası bu çalışır, ancak aynı zamanda VS'de modelin doğrulama keser gibi düzenleme.</span><span class="sxs-lookup"><span data-stu-id="c48c8-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="c48c8-134">Bu nedenle, modelinizi doğrulama her zaman hata almazsınız.</span><span class="sxs-lookup"><span data-stu-id="c48c8-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="c48c8-135">Bunu yapmak için tek bir sonuç kümesi sorguda yaptığınız gibi modelinize saklı yordamı eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="c48c8-136">Modelinize göre sağ tıklatın ve seçin için ihtiyaç duyduğunuz sonra bunu aldıktan sonra **birlikte Aç...**</span><span class="sxs-lookup"><span data-stu-id="c48c8-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="c48c8-137">ardından **Xml**</span><span class="sxs-lookup"><span data-stu-id="c48c8-137">then **Xml**</span></span>

    ![OpenAs](~/ef6/media/openas.png)

<span data-ttu-id="c48c8-139">XML olarak aşağıdaki adımları gerçekleştirmeniz gereken sonra açılan modeli olduğunda:</span><span class="sxs-lookup"><span data-stu-id="c48c8-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="c48c8-140">Karmaşık tür ve işlevi içeri aktarma, modelinizde bulun:</span><span class="sxs-lookup"><span data-stu-id="c48c8-140">Find the complex type and function import in your model:</span></span>

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

 

-   <span data-ttu-id="c48c8-141">Karmaşık tür Kaldır</span><span class="sxs-lookup"><span data-stu-id="c48c8-141">Remove the complex type</span></span>
-   <span data-ttu-id="c48c8-142">İşlev güncelleştirin, böylece bunun aşağıdaki gibi görünür durumda varlıklarınızı eşlenir:</span><span class="sxs-lookup"><span data-stu-id="c48c8-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="c48c8-143">Bu saklı yordamı blog girişleri, diğeri post girişlerinin iki koleksiyonu döndürür, modeli bildirir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="c48c8-144">İşlev eşleme öğesi bulun:</span><span class="sxs-lookup"><span data-stu-id="c48c8-144">Find the function mapping element:</span></span>

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

-   <span data-ttu-id="c48c8-145">Biri, aşağıdakiler gibi döndürülen her varlık için sonuç eşleme değiştirin:</span><span class="sxs-lookup"><span data-stu-id="c48c8-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

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

<span data-ttu-id="c48c8-146">Sonuç kümeleri varsayılan olarak oluşturulan gibi karmaşık türler eşlemek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="c48c8-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="c48c8-147">Bunu yapmak için yeni bir karmaşık türü, bunları kaldırma yerine oluşturun ve karmaşık türler Yukarıdaki örneklerde varlık adları kullanmışsınız her yerde kullanın.</span><span class="sxs-lookup"><span data-stu-id="c48c8-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="c48c8-148">Bu eşlemeler değiştirilmiş bir kez daha sonra modeli kaydedin ve saklı yordamı kullanmak için şu kodu yürütün:</span><span class="sxs-lookup"><span data-stu-id="c48c8-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

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
> <span data-ttu-id="c48c8-149">Edmx dosyasını modeliniz için el ile düzenlerseniz, hiç olmadığı kadar veritabanı modelden oluşturursanız üzerine yazılacak.</span><span class="sxs-lookup"><span data-stu-id="c48c8-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="c48c8-150">Özet</span><span class="sxs-lookup"><span data-stu-id="c48c8-150">Summary</span></span>

<span data-ttu-id="c48c8-151">Burada birden çok sonuç erişmenin iki farklı yöntemle ayarlar Entity Framework kullanarak göstermiştir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="c48c8-152">Her ikisi de durumunuza göre eşit olarak geçerlidir ve tercihleri ve koşullarınıza için en iyi görünüyor birini seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="c48c8-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="c48c8-153">Birden çok sonuç kümesi olacak desteği gelecekte Entity Framework sürümlerini geliştirildi ve bu belgedeki adımları gerçekleştirmeden artık gerekli olacağını planlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c48c8-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
