---
title: Varlıkları sorgulama ve bulma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417170"
---
# <a name="querying-and-finding-entities"></a>Varlıkları sorgulama ve bulma
Bu konu, LINQ ve Find yöntemi de dahil olmak üzere Entity Framework kullanarak verileri sorgulamak için kullanabileceğiniz çeşitli yolları ele almaktadır. Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

## <a name="finding-entities-using-a-query"></a>Sorgu kullanarak varlıkları bulma  

DbSet ve ıdbset IQueryable uygular ve bu nedenle veritabanına karşı bir LINQ sorgusu yazmak için başlangıç noktası olarak kullanılabilir. Bu, LINQ 'ın derinlemesine bir tartışması için uygun yer değildir, ancak birkaç basit örnek aşağıda verilmiştir:  

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

DbSet ve ıdbset 'in veritabanına göre her zaman sorgu oluşturdığına ve varlıklar bağlamda zaten mevcut olsa bile her zaman veritabanına gidiş dönüş içereceği unutulmamalıdır. Veritabanına karşı bir sorgu yürütülür:  

- **Foreach** (C#) veya **her** bir (Visual Basic) ifadesi tarafından numaralandırılır.  
- [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary)veya [ToList](https://msdn.microsoft.com/library/bb342261)gibi bir koleksiyon işlemi tarafından numaralandırılır.  
- Sorgunun en dıştaki bölümünde [ilki](https://msdn.microsoft.com/library/bb291976) veya [Any](https://msdn.microsoft.com/library/bb337697) gibi LINQ işleçleri belirtilmiştir.  
- Aşağıdaki yöntemler çağrılır: bir DbSet, [DbEntityEntry. Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)ve Database. executesqlcommand üzerindeki [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) genişletme yöntemi.  

Veritabanından sonuçlar döndürüldüğünde, bağlamda mevcut olmayan nesneler bağlamına iliştirilir. Bir nesne zaten bağlamda varsa, varolan nesne döndürülür (nesne içindeki özelliklerin geçerli ve orijinal **değerlerinin, veritabanı değerleriyle üzerine yazılmaz** ).  

Bir sorgu gerçekleştirdiğinizde, içeriğe eklenmiş ancak henüz veritabanına kaydedilmemiş varlıklar sonuç kümesinin bir parçası olarak döndürülmez. Bağlamdaki verileri almak için bkz. [yerel veriler](~/ef6/querying/local-data.md).  

Sorgu veritabanından hiçbir satır döndürürse, sonuç **null**yerine boş bir koleksiyon olur.  

## <a name="finding-entities-using-primary-keys"></a>Birincil anahtarları kullanarak varlıkları bulma  

DbSet üzerindeki Find yöntemi, bağlam tarafından izlenen bir varlık bulmayı denemek için birincil anahtar değerini kullanır. Varlık bağlamda bulunmazsa, varlığı bulmak için veritabanına bir sorgu gönderilir. Varlık bağlamda veya veritabanında bulunmazsa null döndürülür.  

Find, iki önemli şekilde sorgu kullanmaktan farklıdır:  

- Veritabanına gidiş dönüş, yalnızca verilen anahtara sahip varlık bağlamda bulunmazsa yapılır.  
- Bul, eklenen durumdaki varlıkları döndürür. Diğer bir deyişle, bul, içeriğe eklenmiş ancak henüz veritabanına kaydedilmemiş olan varlıkları döndürür.  
### <a name="finding-an-entity-by-primary-key"></a>Birincil anahtara göre bir varlık bulma  

Aşağıdaki kodda bazı bul kullanımları gösterilmektedir:  

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

### <a name="finding-an-entity-by-composite-primary-key"></a>Bileşik birincil anahtara göre varlık bulma  

Entity Framework, varlıklarınızın bileşik anahtarlara sahip olmasına olanak sağlar. Bu, birden fazla özellikten oluşan bir anahtardır. Örneğin, belirli bir blog için Kullanıcı ayarlarını temsil eden bir BlogSettings varlığına sahip olabilirsiniz. Bir Kullanıcı her blog için yalnızca bir BlogSettings sahibi olacağından, BlogSettings 'ın birincil anahtarını blogID ve Kullanıcı adının bir birleşimini yapmayı tercih edebilirsiniz. Aşağıdaki kod, blogID = 3 ve username = "johndoe1987" ile BlogSettings bulmayı dener:  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Bileşik anahtarlarınız varsa, bileşik anahtarın özelliklerine yönelik bir sıralama belirtmek için ColumnAttribute veya Fluent API kullanmanız gerektiğini unutmayın. Bu anahtarı oluşturan değerleri belirtirken bulma çağrısının bu sırayı kullanması gerekir.  
