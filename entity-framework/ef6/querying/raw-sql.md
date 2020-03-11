---
title: Ham SQL sorguları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417097"
---
# <a name="raw-sql-queries"></a>Ham SQL Sorguları
Entity Framework, varlık sınıflarınız ile LINQ kullanarak sorgulama yapmanıza olanak sağlar. Ancak, ham SQL kullanarak doğrudan veritabanına karşı sorguları çalıştırmak istediğiniz zamanlar olabilir. Bu, saklı yordamların çağrılmasını içerir. Bu, şu anda saklı yordamlara eşlemeyi desteklemeyen Code First modeller için yararlı olabilir. Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

## <a name="writing-sql-queries-for-entities"></a>Varlıklar için SQL sorguları yazma  

DbSet üzerindeki SqlQuery metodu, varlık örnekleri döndürecek ham bir SQL sorgusunun yazılmasına izin verir. Döndürülen nesneler, LINQ sorgusu tarafından döndürüldüler olduğu gibi bağlam tarafından izlenir. Örnek:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

LINQ sorguları için olduğu gibi, sonuçlar numaralandırılıncaya kadar sorgu yürütülmez — Yukarıdaki örnekte, ToList çağrısıyla yapılır.  

Her iki nedenden dolayı ham SQL sorguları yazıldığında dikkatli olunmalıdır. İlk olarak, yalnızca istenen türden gerçekten varlık döndüren varlıkların döndürüldüğünden emin olmak için sorgunun yazılması gerekir. Örneğin, devralma gibi özellikleri kullanırken yanlış CLR türünde varlıklar oluşturacak bir sorgu yazmak kolaydır.  

İkinci olarak, bazı ham SQL sorgusu türleri, özellikle SQL ekleme saldırıları etrafında olası güvenlik riskleri sunar. Bu tür saldırılara karşı koruma sağlamak için Sorgunuzdaki parametreleri doğru şekilde kullandığınızdan emin olun.  

### <a name="loading-entities-from-stored-procedures"></a>Saklı yordamlardan varlık yükleme  

Saklı yordamın sonuçlarından varlıkları yüklemek için DbSet. SqlQuery ' i kullanabilirsiniz. Örneğin, aşağıdaki kod dbo 'yı çağırır. Veritabanında Getbloglar yordamı:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

Ayrıca, aşağıdaki sözdizimini kullanarak parametreleri saklı yordama geçirebilirsiniz:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Varlık dışı türler için SQL sorguları yazma  

İlkel türler de dahil olmak üzere herhangi bir türün örnek döndüren bir SQL sorgusu, veritabanı sınıfında SqlQuery yöntemi kullanılarak oluşturulabilir. Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Nesneler bir varlık türünün örnekleri olsa bile, veritabanındaki SqlQuery 'den döndürülen sonuçlar hiçbir şekilde bağlam tarafından izlenmeyecektir.  

## <a name="sending-raw-commands-to-the-database"></a>Ham komutları veritabanına gönderme  

Sorgu olmayan komutlar veritabanı üzerinde ExecuteSqlCommand yöntemi kullanılarak veritabanına gönderilebilir. Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Veritabanında veri yüklenmeden veya yeniden yüklenene kadar, ExecuteSqlCommand kullanılarak veritabanındaki verilerde yapılan tüm değişikliklerin bağlam halinde donuk olduğunu unutmayın.  

### <a name="output-parameters"></a>Çıktı Parametreleri  

Çıkış parametreleri kullanılıyorsa, sonuçlar tamamen okunana kadar bu değerler kullanılamaz. Bu, DbDataReader 'ın temel davranışının nedeni, daha fazla ayrıntı için [DataReader kullanarak veri alma](https://go.microsoft.com/fwlink/?LinkID=398589) konusuna bakın.  
