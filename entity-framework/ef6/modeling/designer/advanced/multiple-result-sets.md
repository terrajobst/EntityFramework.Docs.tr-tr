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
# <a name="stored-procedures-with-multiple-result-sets"></a>Birden çok sonuç kümesi saklı yordamlar
Bazen kullanarak saklı yordamları birden fazla sonuç döndürmesi gerekir ayarlanır. Bu senaryo veritabanı sayısını azaltmak için yaygın olarak kullanılan gidiş dönüş tek bir ekran oluşturmak için gereklidir. Entity Framework EF5 önce çağrılacak saklı yordamı çalıştırmasına olanak tanır ancak yalnızca ilk sonuç çağrıldığı koda kümesini döndürür.

Bu makale, birden fazla sonuç kümesi saklı yordam, Entity Framework erişmek için kullanabileceğiniz iki yolu gösterir. Yalnızca kod kullanır ve her iki koduyla önce çalışır ve EF Designer diğeri, yalnızca EF Designer ile çalışır. Gelecekte araçları ve API desteği Entity Framework sürümlerini geliştirmek.

## <a name="model"></a>Model

Bu makaledeki örneklerde temel bir Blog kullanın ve tek bir blog gönderileri modeli blog birçok gönderileri ve bir gönderi sahip olduğu ait. Tüm blog ve gönderi şuna benzeyen döndürür veritabanında bir saklı yordam kullanacağız:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Birden çok sonuç erişme koduyla ayarlar

Size sunduğumuz saklı yordamı yürütmek için ham SQL komutu için kullanmak kod yürütebilir. Bu yaklaşımın avantajı, her iki kod ile ilk çalışıyor olması ve EF Designer.

Birden çok sonuç almak için çalışma IObjectContextAdapter arabirimini kullanarak ObjectContext API'sı tarafından açılan ihtiyacımız ayarlar.

Biz bir Objectcontext'e tamamladıktan sonra Translate yöntemi izlenen ve EF normal olarak kullanılan varlıklar bizim saklı yordamının sonuçları küçültmesini kullanabiliriz. Aşağıdaki kod örneği, bu eylem gösterir.

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

Translate yöntemi biz yordamı, bir EntitySet adı ve bir MergeOption yürütüldüğünde, aldık okuyucunun kabul eder. EntitySet adı türetilmiş Bağlamınızı olan DB özelliği ile aynı olacaktır. Bellekte aynı varlık zaten varsa, sonuçları nasıl işleneceğini MergeOption enum denetler.

Burada ilk sonuç kümesi için sonraki sonuç kümesini taşımadan önce kullanılması gerekir çünkü biz NextResult, yukarıdaki kod verilen önemli budur çağırmadan önce biz blogları, koleksiyonda gezin.

İki Çevir sonra yöntemleri sonra Blog ve gönderi varlıkların herhangi bir varlık aynı şekilde EF tarafından izlenir ve değiştirilebilir veya silindi ve normal olarak kaydedilen çağrılır.

>[!NOTE]
> Translate yöntemi kullanarak varlıkları oluşturduğunda EF hiçbir eşlemeyle dikkate almaz. Yalnızca sonuç sınıflarınızı üzerinde özellik adları ile kümesini sütun adları aynı olacaktır.

>[!NOTE]
> Blog varlıklardan biri gönderileri özelliği erişim etkinse, yavaş yükleniyor varsa zaten tümünü yüklendik olsa da ardından EF gevşek tüm gönderileri yüklemek için veritabanına bağlanmanızı. EF tüm gönderileri yüklü veya olup olmadığını veritabanında daha fazla biliyor olmasıdır. Ardından, bu durumu önlemek istiyorsanız yavaş yükleniyor devre dışı bırakmanız gerekir.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Yapılandırılmış EDMX ile birden çok sonuç kümesi

>[!NOTE]
> .NET Framework 4. 5'i EDMX içinde birden çok sonuç kümesi yapılandırmak için hedeflemesi gerekir. .NET 4.0 hedefliyorsanız, önceki bölümde gösterilen kod tabanlı yöntemi kullanabilirsiniz.

EF Designer kullanıyorsanız, böylece döndürülecek farklı sonuç kümeleri hakkında bilmesi modelinizi değiştirebilirsiniz. Edmx dosyasını el ile düzenlemek ihtiyacınız olacak şekilde elle araçları birden çok sonuç değil önce bilmeniz gereken bir şey kullanan, ayarlayın. Bir edmx dosyası bu çalışır, ancak aynı zamanda VS'de modelin doğrulama keser gibi düzenleme. Bu nedenle, modelinizi doğrulama her zaman hata almazsınız.

-   Bunu yapmak için tek bir sonuç kümesi sorguda yaptığınız gibi modelinize saklı yordamı eklemeniz gerekir.
-   Modelinize göre sağ tıklatın ve seçin için ihtiyaç duyduğunuz sonra bunu aldıktan sonra **birlikte Aç...** ardından **Xml**

    ![OpenAs](~/ef6/media/openas.png)

XML olarak aşağıdaki adımları gerçekleştirmeniz gereken sonra açılan modeli olduğunda:

-   Karmaşık tür ve işlevi içeri aktarma, modelinizde bulun:

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

 

-   Karmaşık tür Kaldır
-   İşlev güncelleştirin, böylece bunun aşağıdaki gibi görünür durumda varlıklarınızı eşlenir:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Bu saklı yordamı blog girişleri, diğeri post girişlerinin iki koleksiyonu döndürür, modeli bildirir.

-   İşlev eşleme öğesi bulun:

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

-   Biri, aşağıdakiler gibi döndürülen her varlık için sonuç eşleme değiştirin:

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

Sonuç kümeleri varsayılan olarak oluşturulan gibi karmaşık türler eşlemek mümkündür. Bunu yapmak için yeni bir karmaşık türü, bunları kaldırma yerine oluşturun ve karmaşık türler Yukarıdaki örneklerde varlık adları kullanmışsınız her yerde kullanın.

Bu eşlemeler değiştirilmiş bir kez daha sonra modeli kaydedin ve saklı yordamı kullanmak için şu kodu yürütün:

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
> Edmx dosyasını modeliniz için el ile düzenlerseniz, hiç olmadığı kadar veritabanı modelden oluşturursanız üzerine yazılacak.

## <a name="summary"></a>Özet

Burada birden çok sonuç erişmenin iki farklı yöntemle ayarlar Entity Framework kullanarak göstermiştir. Her ikisi de durumunuza göre eşit olarak geçerlidir ve tercihleri ve koşullarınıza için en iyi görünüyor birini seçmeniz gerekir. Birden çok sonuç kümesi olacak desteği gelecekte Entity Framework sürümlerini geliştirildi ve bu belgedeki adımları gerçekleştirmeden artık gerekli olacağını planlanmaktadır.
