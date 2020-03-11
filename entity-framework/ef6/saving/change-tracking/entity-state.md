---
title: Varlık durumlarıyla çalışma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419701"
---
# <a name="working-with-entity-states"></a>Varlık durumlarıyla çalışma
Bu konu, bir bağlama nasıl varlık ekleneceğini ve ekleneceğini ve Entity Framework SaveChanges sırasında bunların nasıl işlem yapılacağını ele almaktadır.
Entity Framework, bir içeriğe bağlıyken varlıkların durumunu izlemenin bir bölümünü ele alır, ancak bağlantısı kesik veya N katmanlı senaryolarda, varlıklarınızın varlıklarınızın hangi durumlarına sahip olduğunu bilmesini sağlayabilirsiniz.
Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

## <a name="entity-states-and-savechanges"></a>Varlık durumları ve SaveChanges

Varlık, EntityState numaralandırması tarafından tanımlanan beş durumdan birinde olabilir. Bu durumlar şunlardır:  

- Eklendi: varlık bağlam tarafından izleniyor ancak veritabanında henüz yok  
- Değiştirilmemiş: varlık bağlam tarafından izleniyor ve veritabanında var ve özellik değerleri veritabanındaki değerlerden değişmemiştir  
- Değiştirilen: varlık bağlam tarafından izleniyor ve veritabanında var ve özellik değerlerinin bazıları veya tümü değiştirildi  
- Silindi: varlık bağlam tarafından izleniyor ve veritabanında var, ancak SaveChanges bir sonraki sefer çağrıldığında veritabanından silinmek üzere işaretlendi  
- Ayrılmış: varlık bağlam tarafından izlenmiyor  

SaveChanges farklı durumlardaki varlıklar için farklı şeyler yapar:  

- Değiştirilmemiş varlıklar SaveChanges 'e dokunmaz. Güncelleştirmeler, değiştirilmemiş durumdaki varlıklar için veritabanına gönderilmez.  
- Eklenen varlıklar veritabanına eklenir ve ardından SaveChanges döndürüldüğünde değişmeden hale gelir.  
- Değiştirilen varlıklar veritabanında güncelleştirilir ve SaveChanges döndürüldüğünde değiştirilmez.  
- Silinen varlıklar veritabanından silinir ve sonra bağlamdan ayrılır.  

Aşağıdaki örneklerde bir varlık veya bir varlık grafiğinin durumunun değiştirilebileceği yollar gösterilmektedir.  

## <a name="adding-a-new-entity-to-the-context"></a>Bağlama yeni bir varlık ekleniyor  

DbSet üzerinde Add yöntemi çağırarak içeriğe yeni bir varlık eklenebilir.
Bu, varlığı eklenen duruma geçirir, yani bu, SaveChanges 'in bir sonraki çağrılışında veritabanına eklenecektir.
Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

İçeriğine yeni bir varlık eklemenin başka bir yolu da, durumunu eklendi olarak değiştirkullanmaktır. Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Son olarak, daha önce izlenmekte olan başka bir varlığa bağlanarak içeriğe yeni bir varlık ekleyebilirsiniz.
Bu, yeni varlığı başka bir varlığın koleksiyon gezintisi özelliğine ekleyerek veya başka bir varlığın başvuru gezintisi özelliğini yeni varlığa işaret etmek üzere ayarlayarak olabilir. Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Eklenmekte olan varlık henüz izlenmeyen diğer varlıklara başvurular içeriyorsa, bu yeni varlıkların de bağlamına ekleneceğini ve SaveChanges 'in bir sonraki çağrılışında veritabanına eklenebileceğini unutmayın.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Varolan bir varlığı bağlama iliştirme  

Veritabanında zaten var olduğunu bildiğiniz ancak şu anda bağlam tarafından izlenmekte olan bir varlığınız varsa, DbSet üzerinde Attach metodunu kullanarak varlığı izlemek için bağlama söyleyebilirsiniz. Varlık bağlamda değiştirilmemiş durumda olacaktır. Örnek:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

SaveChanges, eklenen varlığın başka bir düzenlemesi yapılmadan çağrılırsa veritabanına hiçbir değişiklik yapılmadığını unutmayın. Bunun nedeni, varlığın Unchanged durumda olması.  

Var olan bir varlığı bağlama eklemek için başka bir yöntem de durumunu değiştirilmemiş olarak değiştirir. Örnek:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Bu örneklerin her ikisi için de iliştirilmekte olan varlık henüz izlenmeyen diğer varlıklara başvurular içeriyorsa, bu yeni varlıkların de Unchanged durumunda içeriğe eklendiği unutulmamalıdır.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Var olan ancak değiştirilen bir varlığı içeriğe ekleme  

Veritabanında zaten var olan ancak hangi değişikliklerin yapıldığını bildiğiniz bir varlığınız varsa, bağlamı eklemek ve durumunu değiştirilme olarak ayarlamak için bağlam söyleyebilirsiniz.
Örnek:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

Durumu değiştirildi olarak değiştirdiğinizde, varlığın tüm özellikleri değiştirilmiş olarak işaretlenir ve SaveChanges çağrıldığında tüm özellik değerleri veritabanına gönderilir.  

İliştirilmekte olan varlık henüz izlenmeyen diğer varlıklara başvurular içeriyorsa, bu yeni varlıkların, değiştirilmemiş durumdaki bir içeriğe eklendiği, otomatik olarak değiştirilme yapılmayacağını unutmayın.
Değiştirilmiş olarak işaretlenmesi gereken birden çok varlık varsa, bu varlıkların her biri için durumu ayrı ayrı ayarlamanız gerekir.  

## <a name="changing-the-state-of-a-tracked-entity"></a>İzlenen bir varlığın durumunu değiştirme  

Girdisinde durum özelliğini ayarlayarak zaten izlenmekte olan bir varlığın durumunu değiştirebilirsiniz. Örnek:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Zaten izlenen bir varlık için Add veya Attach çağrısı, varlık durumunu değiştirmek için de kullanılabilir. Örneğin, şu anda eklenmiş durumdaki bir varlık için Iliştirme çağrısı, durumunu değiştirilmemiş olarak değiştirecek.  

## <a name="insert-or-update-pattern"></a>Ekleme veya güncelleştirme stili  

Bazı uygulamalar için ortak bir model, yeni olarak bir varlık eklemektir (veritabanı eklemeye yol açar) ya da bir varlığı mevcut olarak ekler ve birincil anahtarın değerine bağlı olarak değiştirilmiş olarak işaretler (bir veritabanı güncelleştirmesine yol açar).
Örneğin, veritabanı tarafından oluşturulan tamsayı birincil anahtarları kullanılırken, bir varlığı, sıfır olmayan bir anahtarla yeni bir varlık ve var olan sıfır olmayan bir anahtarla değerlendirmek yaygındır.
Bu model, birincil anahtar değerinin bir denetimine göre varlık durumu ayarlanarak elde edilebilir. Örnek:  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

Durumu değiştirildi olarak değiştirdiğinizde varlığın tüm özellikleri değiştirilmiş olarak işaretlenir ve SaveChanges çağrıldığında tüm özellik değerlerinin veritabanına gönderileceğini unutmayın.  
