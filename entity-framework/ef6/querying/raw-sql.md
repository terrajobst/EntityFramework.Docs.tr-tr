---
title: Ham SQL sorguları - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 6b00648939ccedffeed09b4e1d6e8d70fa262a36
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490590"
---
# <a name="raw-sql-queries"></a>Ham SQL sorguları
Entity Framework, varlık sınıfları ile LINQ kullanarak sorgulama sağlar. Ancak, kullanarak doğrudan veritabanında ham SQL sorguları çalıştırmak istediğiniz zamanlar olabilir. Bu, şu anda saklı yordamlar için eşleme desteklemeyen Code First modelleri için yararlı olabilir saklı yordam çağırma içerir. Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

## <a name="writing-sql-queries-for-entities"></a>Varlıklar için SQL sorguları yazma  

Varlık örneklerini döndürür yazılacak ham bir SQL sorgusu olan DB SqlQuery yöntemi sağlar. Yalnızca bunlar bir LINQ Sorgu tarafından döndürülen durumunda olduğu gibi döndürülen nesneleri bağlam tarafından izlenir. Örneğin:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Sonuçları numaralandırılır kadar LINQ sorguları olduğu gibi sorgu yürütülmez ise, Not: Yukarıdaki örnekte, bu ToList çağrısıyla yapılır.  

Ham SQL sorguları iki nedenden dolayı yazılan her dikkatli olunması. İlk olarak, yalnızca gerçekten istenen türde olan varlıklar döndürüyor emin olmak için sorgu yazılması gerekir. Örneğin, devralma gibi özellikleri kullanarak CLR türü yanlış olan varlıkları oluşturan bir sorgu yazmak kolaydır.  

İkinci olarak, bazı türleri ham SQL sorgusunun özellikle geçici SQL ekleme saldırılarına karşı olası güvenlik risklerini kullanıma sunar. Bu tür saldırılara karşı korunmak için doğru şekilde sorgu parametreleri kullandığınızdan emin olun.  

### <a name="loading-entities-from-stored-procedures"></a>Saklı yordamlardan varlıklar yükleniyor  

DbSet.SqlQuery varlıklar bir saklı yordam sonuçlarından yüklemek için kullanabilirsiniz. Örneğin, aşağıdaki kod, dbo çağırır. Veritabanı yordamda GetBlogs:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

Ayrıca, aşağıdaki söz dizimini kullanarak bir saklı yordam parametreleri geçirebilirsiniz:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Varlık olmayan türleri için SQL sorguları yazma  

İlkel türler dahil olmak üzere herhangi bir türü örneklerini döndüren bir SQL sorgusu üzerinde veritabanı sınıfı SqlQuery yöntemi kullanılarak oluşturulabilir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Nesneleri bir varlık türü örneklerini olsa bile, SqlQuery döndürülen veritabanı sonuçları hiçbir zaman bağlam tarafından izlenir.  

## <a name="sending-raw-commands-to-the-database"></a>Veritabanına ham komut gönderme  

Sorgu dışı komutları veritabanında ExecuteSqlCommand yöntemi kullanarak veritabanına gönderilir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Varlıkları yüklenen veya veritabanından yeniden kadar ExecuteSqlCommand kullanarak veritabanındaki verileri yapılan tüm değişiklikler bağlamına donuk olduğunu unutmayın.  

### <a name="output-parameters"></a>Çıktı Parametreleri  

Çıktı parametreleri kullandıysanız değerleri sonuçları tamamen okunana kadar kullanılamaz. Bu dbdatareader öğesine dönüştürülemedi arka plandaki davranışı nedeniyle, bkz: [alma verileri kullanarak bir DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) daha fazla ayrıntı için.  
