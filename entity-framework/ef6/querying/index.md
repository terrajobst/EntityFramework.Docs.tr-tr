---
title: Varlıkları Sorgulama ve Bulma - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417170"
---
# <a name="querying-and-finding-entities"></a>Varlıkları Sorgulama ve Bulma
Bu konu, LINQ ve Bul yöntemi de dahil olmak üzere Varlık Çerçevesi'ni kullanarak veri sorgulayabildiğiniz çeşitli yolları kapsar. Bu konuda gösterilen teknikler, Code First ve EF Designer ile oluşturulan modeller için eşit olarak uygulanır.  

## <a name="finding-entities-using-a-query"></a>Sorgu kullanarak varlıkları bulma  

DbSet ve IDbSet iQueryable uygulamak ve böylece veritabanına karşı bir LINQ sorgu yazmak için başlangıç noktası olarak kullanılabilir. Bu LINQ derinlemesine bir tartışma için uygun bir yer değildir, ancak burada basit örnekler bir çift şunlardır:  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

DbSet ve IDbSet'in her zaman veritabanına karşı sorgular oluşturduğunu ve döndürülen varlıklar bağlamında zaten mevcut olsa bile veritabanına gidiş-dönüş içereceğini unutmayın. Aşağıdaki saatlerde veritabanına karşı bir sorgu yürütülür:  

- Bir **foreach** (C#) veya **For Each** (Visual Basic) deyimi ile numaralandırılır.  
- [ToArray,](https://msdn.microsoft.com/library/bb298736) [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)veya [ToList](https://msdn.microsoft.com/library/bb342261)gibi bir toplama işlemi tarafından numaralandırılır.  
- [İlk](https://msdn.microsoft.com/library/bb291976) veya [Any](https://msdn.microsoft.com/library/bb337697) gibi LINQ işleçleri sorgunun en dış kısmında belirtilir.  
- Aşağıdaki yöntemler denir: DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)ve Database.ExecuteSqlCommand yük [uzantısı](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) yöntemi.  

Sonuçlar veritabanından döndürüldüğünde, bağlamiçinde bulunmayan nesneler içeriğe eklenir. Bir nesne zaten bağlam içindeyse, varolan nesne döndürülür (nesnenin girişteki özelliklerinin geçerli ve özgün değerleri veritabanı değerleriyle birlikte üzerine **yazılmaz).**  

Bir sorgu gerçekleştirdiğinizde, içeriğe eklenen ancak henüz veritabanına kaydedilmemiş varlıklar, sonuç kümesinin bir parçası olarak döndürülmez. Bağlamdaki verileri almak için [Bkz. Yerel Veriler.](~/ef6/querying/local-data.md)  

Bir sorgu veritabanından satır döndürmezse, sonuç **null**yerine boş bir koleksiyon olur.  

## <a name="finding-entities-using-primary-keys"></a>Birincil anahtarları kullanarak varlıkları bulma  

DbSet'te Bul yöntemi, bağlam tarafından izlenen bir varlığı bulmaya çalışmak için birincil anahtar değerini kullanır. Varlık bağlamda bulunmazsa, veritabanına bir sorgu gönderilir ve orada bulunan varlığı bulur. Varlık bağlamda veya veritabanında bulunmazsa null döndürülür.  

Bul, sorgukullanmanın iki önemli şekilde kullanılmasından farklıdır:  

- Veritabanına gidiş-dönüş yalnızca verilen anahtara sahip varlık bağlamında bulunmazsa yapılır.  
- Find, Eklenen durumdaki varlıkları döndürecek. Diğer bir zamanda, Bul, içeriğe eklenen ancak henüz veritabanına kaydedilmemiş varlıkları döndürecektir.  
### <a name="finding-an-entity-by-primary-key"></a>Birincil anahtara göre bir varlık bulma  

Aşağıdaki kod Bul'un bazı kullanımlarını gösterir:  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a>Bileşik birincil anahtarla bir varlık bulma  

Entity Framework, varlıklarınızın bileşik anahtarlara sahip olmasını sağlar - bu birden fazla özellikten oluşan bir anahtardır. Örneğin, belirli bir blogun kullanıcı ayarlarını temsil eden bir BlogAyarları kuruluşunuz olabilir. Çünkü bir kullanıcı sadece hiç her blog için bir BlogSettings olurdu BlogAyarlar birincil anahtarı BlogId ve Kullanıcı Adı bir arada yapmak için seçebilirsiniz. Aşağıdaki kod BlogAyarları BlogId = 3 ve Kullanıcı Adı = "johndoe1987" ile bulmaya çalışır:  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Bileşik tuşlarınız olduğunda, bileşik anahtarın özellikleri için bir sıralama belirtmek için ColumnAttribute veya akıcı API kullanmanız gerektiğini unutmayın. Anahtarı oluşturan değerleri belirtirken Bul çağrısı bu sırayı kullanmalıdır.  
