---
title: Sorgulamak ve varlıkları - EF6 bulma
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
caps.latest.revision: 3
ms.openlocfilehash: 92467e1a93f576eca627cf7b7d2351054a882c2c
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067553"
---
# <a name="querying-and-finding-entities"></a>Sorgulamak ve varlıkları bulma
Bu konu LINQ ve bulma yöntemini de dahil olmak üzere, Entity Framework kullanarak verileri sorgulamak farklı yöntemleri kapsar. Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

## <a name="finding-entities-using-a-query"></a>Bir sorgu kullanarak varlıkları bulma  

Olan DB ve IDbSet Iqueryable'a uygulayın ve veritabanında bir LINQ sorgusunu yazmak için başlangıç noktası olarak bu nedenle kullanılabilir. LINQ ilgili ayrıntılı bir tartışma için uygun bir yerde değil, ancak birkaç basit bir örnek aşağıdadır:  

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

Olan DB ve IDbSet her zaman veritabanına karşı sorgular oluşturun ve zaten döndürülen varlıklara bağlamında bulunsa bile her zaman veritabanına gidiş dönüş içerecektir unutmayın. Bir sorgu veritabanında yürütüldüğü zaman:  

- Tarafından numaralandırılmış bir **foreach** (C#) veya **her** deyimi (Visual Basic).  
- Bir toplama işlemi tarafından gibi numaralandırılana [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), veya [ToList](https://msdn.microsoft.com/library/bb342261).  
- Gibi LINQ işleçleri [ilk](https://msdn.microsoft.com/library/bb291976) veya [herhangi](https://msdn.microsoft.com/library/bb337697) en dıştaki sorgunun parçası belirtilir.  
- Aşağıdaki yöntem de çağrıldığında: [yük](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) genişletme yöntemini bir olan DB [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx)ve Database.ExecuteSqlCommand.  

Veritabanından sonuçları döndürdüğünde bağlamda mevcut değil bir nesne bağlamına eklenir. Bir nesne bağlamı ise, varolan nesne döndürülür (geçerli ve orijinal nesnenin özelliklerini giriş değerleri **değil** veritabanı değerlerle geçersiz kılınır).  

Bir sorgu gerçekleştirdiğinizde, bağlamına eklenmiş ancak veritabanı henüz kaydedilmedi varlıklar sonuç kümesinin bir parçası olarak döndürülmez. Bir bağlam içinde veri almak için bkz [yerel veri](~/ef6/querying/local-data.md).  

Bir sorgu, veritabanından satır döndürürse, sonuç boş bir koleksiyon olacaktır yerine **null**.  

## <a name="finding-entities-using-primary-keys"></a>Birincil anahtarlar kullanarak varlıkları bulma  

Bulma yöntemini olan DB bağlam tarafından izlenen bir varlığın bulmaya için birincil anahtar değeri kullanır. Varlık bağlamı bulunamazsa sonra bir sorgu veritabanına varlık var. bulmak için gönderilir. Varlık bağlamı veya veritabanında bulunmazsa null döndürülür.  

Bulma, bir sorgu kullanarak iki önemli şekilde farklıdır:  

- Bağlam içinde verilen anahtara sahip bir varlık bulunamıyorsa bir gidiş dönüş veritabanına yalnızca sunulacaktır.  
- Bul Added durumda olan varlıklar döndürür. Bu, bulabilir, bağlamına eklenmiş ancak veritabanı henüz kaydedilmedi varlıkları döndürür.  
### <a name="finding-an-entity-by-primary-key"></a>Bir varlığın birincil anahtara göre bulma  

Aşağıdaki kod bulma bazı kullanımlarını gösterir:  

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

### <a name="finding-an-entity-by-composite-primary-key"></a>Bir varlığın birincil bileşik anahtara göre bulma  

Entity Framework varlıklarınızı birden fazla özelliği oluşan bir anahtar kullanıcının bileşik anahtarlar - olmasını sağlar. Örneğin, belirli bir blog için kullanıcı ayarlarını temsil eden bir BlogSettings varlık olabilir. Bir kullanıcı her zaman sadece yeterli olacağından bir BlogSettings yapabilirsiniz her blog için birincil anahtarı olarak BlogSettings BlogId ve kullanıcı adı olun seçtiniz. Aşağıdaki kod ile BlogId BlogSettings bulmayı dener 3 ve kullanıcı adı = "johndoe1987" =:  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Bileşik anahtarlar olduğunda bileşik anahtarın özelliklerini sıralama belirtmek için ColumnAttribute ya da fluent API'sini kullanmanız gerektiğini unutmayın. Bul çağrısı bu anahtarı form değerler belirtirken kullanmalısınız.  
