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
# <a name="stored-procedures-with-multiple-result-sets"></a>Birden çok sonuç kümesiyle saklı yordamlar
Bazen saklı yordamlar kullanılırken, birden fazla sonuç kümesi döndürihtiyacınız olacaktır. Bu senaryo, tek bir ekran oluşturmak için gereken veritabanı gidiş dönüş sayısını azaltmak için yaygın olarak kullanılır. EF5 ' dan önce, Entity Framework saklı yordamın çağrılmasına izin verir, ancak yalnızca ilk sonuç kümesini çağıran koda döndürür.

Bu makalede, Entity Framework ' deki bir saklı yordamdan birden fazla sonuç kümesine erişmek için kullanabileceğiniz iki yol gösterilir. Yalnızca kod kullanan ve hem kod hem de EF Tasarımcısı ve yalnızca EF Designer ile birlikte çalışarak çalıştırılan bir tane. Bu için araç ve API desteği, Entity Framework gelecek sürümlerinde iyileştirmelidir.

## <a name="model"></a>Model

Bu makaledeki örneklerde, bir blogun birçok gönderiye sahip olduğu ve bir gönderisinin tek bir blog 'a ait olduğu temel bir blog ve gönderi modeli kullanılır. Veritabanında, şunun gibi tüm blogları ve gönderileri döndüren bir saklı yordam kullanacağız:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Birden çok sonuç kümesine kodla erişme

Saklı yordamımızı yürütmek için ham bir SQL komutu vermek üzere kullanım kodu yürütüyoruz. Bu yaklaşımın avantajı, hem Code First hem de EF Designer ile birlikte çalışabildir.

Çalışan birden çok sonuç kümesi almak için, ıobjectcontextadapter arabirimini kullanarak ObjectContext API 'sine bırakması gerekir.

Bir ObjectContext olduktan sonra, saklı yordamımızın sonuçlarını, normal şekilde, bu şekilde izlenebilirler ve kullanılabilecek varlıklara dönüştürmek için Çevir yöntemini kullanabiliriz. Aşağıdaki kod örneği bu eylemi gösterir.

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

Çeviri yöntemi, yordamı, bir EntitySet adını ve bir birleştirme seçeneğini yürütüyoruz, aldığımız okuyucuyu kabul eder. EntitySet adı, türetilmiş bağlamınızın DbSet özelliği ile aynı olacaktır. MergeOption numaralandırması, bellekte aynı varlık zaten varsa sonuçların nasıl işlendiğini denetler.

Burada, NextResult 'ı çağırmadan önce blogların koleksiyonunu yineliyoruz. ilk sonuç kümesi bir sonraki sonuç kümesine geçmeden önce tüketilmesi gerektiğinden Yukarıdaki kod verilmesinden önemlidir.

İki çevirme yöntemi çağrıldıktan sonra, blog ve gönderi varlıkları diğer varlıklarıyla aynı şekilde işlenir ve bu nedenle değiştirilebilir veya silinebilir ve normal şekilde kaydedilebilir.

>[!NOTE]
> EF, çeviri yöntemini kullanarak varlıkları oluşturduğunda, hiçbir eşleme almaz. Bu, sonuç kümesindeki sütun adlarını sınıflarınızda Özellik adlarıyla eşleştirecek.

>[!NOTE]
> Yavaş yükleme özelliği etkinse, blog varlıklarından birindeki gönderi özelliğine eriştiğinizde EF, tümünü zaten yüklemiş olduğumuz halde tüm gönderileri geç yüklemek için veritabanına bağlanır. Bunun nedeni, EF 'in tüm gönderilerinizi mi yüklediklerini yoksa ya da veritabanında daha fazla olup olmadığını bilemez. Bundan kaçınmak isterseniz, geç yüklemeyi devre dışı bırakmanız gerekir.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>EDMX içinde yapılandırılmış birden çok sonuç kümesi

>[!NOTE]
> EDMX 'te birden çok sonuç kümesi yapılandırabilmek için .NET Framework 4,5 ' i hedeflemelidir. .NET 4,0 ' i hedefliyorsanız, önceki bölümde gösterilen kod tabanlı yöntemi kullanabilirsiniz.

EF Designer kullanıyorsanız, modelinizi, döndürülecek farklı sonuç kümelerini bilmesi için de değiştirebilirsiniz. Her şey, araçları birden çok sonuç kümesi olmadan önce bilmeniz, bu nedenle edmx dosyasını el ile düzenlemeniz gerekir. Bu dosya gibi edmx dosyasını düzenleme işi çalışır, ancak aynı zamanda, VS 'deki modelin doğrulanmasını de keser. Bu nedenle modelinizi doğrulamanız durumunda her zaman hata alırsınız.

-   Bunu yapmak için, tek bir sonuç kümesi sorgusuyla yaptığınız gibi modelinize saklı yordamı eklemeniz gerekir.
-   Bunu yaptıktan sonra modelinize sağ tıklayıp **birlikte Aç** ' ı seçmeniz gerekir. sonra **XML**

    ![Farklı aç](~/ef6/media/openas.png)

Modeli XML olarak açtıktan sonra, aşağıdaki adımları gerçekleştirmeniz gerekir:

-   Modelinizde karmaşık tür ve işlev içeri aktarmayı bulun:

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

 

-   Karmaşık türü kaldır
-   İşlev içeri aktarmayı, varlıklarınıza eşlenecek şekilde güncelleştirin, bu durumda aşağıdaki gibi görünür:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Bu, modele saklı yordamın iki koleksiyon döndürmeyeceğini, blog girişlerinden birini ve post girişlerinden birini söyler.

-   İşlev eşleme öğesini bul:

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

-   Sonuç eşlemesini, döndürülmekte olan her varlık için bir ile değiştirin; Örneğin, aşağıdaki gibi:

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

Sonuç kümelerinin, varsayılan olarak oluşturulan bir gibi karmaşık türlerle eşleşmesi de mümkündür. Bunu yapmak için, bunları kaldırmak yerine yeni bir karmaşık tür oluşturun ve Yukarıdaki örneklerde varlık adlarını kullandığınız her yerde karmaşık türleri kullanın.

Bu eşlemeler değiştirildikten sonra modeli kaydedebilir ve saklı yordamı kullanmak için aşağıdaki kodu çalıştırabilirsiniz:

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
> Modelinize ilişkin edmx dosyasını el ile düzenlerseniz, modeli veritabanından yeniden oluşturduysanız üzerine yazılır.

## <a name="summary"></a>Özet

Burada, birden çok sonuç kümesine Entity Framework kullanarak erişmenin iki farklı yöntemi gösterildik. Her ikisi de durumunuza ve tercihlerinize göre eşit oranda geçerlidir ve koşullarınız için en iyi görünen birini seçmeniz gerekir. Birden çok sonuç kümesi desteğinin, Entity Framework gelecek sürümlerinde iyileştirilen ve bu belgedeki adımların gerçekleştirilmesi artık gerekli olmayacak şekilde planlanacaktır.
