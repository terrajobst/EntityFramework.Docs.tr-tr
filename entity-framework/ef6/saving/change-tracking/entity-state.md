---
title: Varlık durumları - EF6 ile çalışma
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: c1dde7810d1dfa8a73e6bd2cf091b24be6b865d8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490679"
---
# <a name="working-with-entity-states"></a>Varlık durumları ile çalışma
Bu konuda, ekleme ve varlıklar için bir bağlam eklemek ve Entity Framework SaveChanges sırasında bunları nasıl işlediğini ele alınacaktır.
Entity Framework için bir bağlam bağlı, ancak bağlantısı kesilmiş veya N-katmanlı senaryolarda durum varlıklarınızı bilmeniz EF sağlayabilirsiniz varlıkların durumunu olmalıdır izleme üstlenir.
Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

## <a name="entity-states-and-savechanges"></a>Varlık durumları ve SaveChanges

Bir varlık beş durumlardan birinde EntityState numaralandırmasıyla tanımlanan olabilir. Bu durumlar şunlardır:  

- Eklenen: Varlık bağlam tarafından izlenen ancak henüz veritabanında mevcut değil  
- Değiştirilmemiş: Varlık bağlam tarafından izlenen ve veritabanında var ve özellik değerlerini veritabanındaki değerlerinin değişmedi  
- Değiştirilen: Varlık bağlam tarafından izleniyor ve veritabanında var ve bazı veya tüm özellik değerleri değiştirilmiş  
- Silinen: Varlık bağlam tarafından izleniyor ve veritabanında var, ancak silinmek üzere veritabanından SaveChanges bir sonraki zamana işaretlendi  
- Ayrılmış: Varlık bağlam tarafından izlenmiyor  

SaveChanges farklı durumlarda varlıklar için farklı şeyler yapar:  

- Değiştirilmemiş varlıkları SaveChanges tarafından kullanılmayan. Güncelleştirmeleri değişmemiş durumda varlıklar için veritabanına gönderilmez.  
- Eklenen varlıkları veritabanına eklenir ve ardından değişmedi haline SaveChanges döndürür.  
- Değiştirilmiş varlıklar veritabanında güncelleştirilen ve ardından değişmedi haline SaveChanges döndürür.  
- Silinen varlıklar veritabanından silinir ve ardından bağlamdan ayrılır.  

Aşağıdaki örnekler bir varlık veya varlık graf durumunu değiştirilebilir yollarını gösterir.  

## <a name="adding-a-new-entity-to-the-context"></a>Bağlam için yeni bir varlık ekleme  

Yeni bir varlık üzerinde olan DB Ekle yöntemi çağırarak bağlamına eklenebilir.
Bu varlık eklenen durumuna, bunun veritabanına SaveChanges adlı bir sonraki açışınızda eklenir, yani koyar.
Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Bağlam için yeni bir varlık eklemek için başka bir yol için eklenen durumunu değiştirmektir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Son olarak, zaten izlenmekte olan başka bir varlık kadar takma tarafından yeni bir varlık bağlamına ekleyebilirsiniz.
Bu, başka bir varlık koleksiyon gezinme özelliği için yeni bir varlık ekleme veya yeni varlığa işaret edecek şekilde başka bir varlık başvurusu gezinti özelliğini ayarlayarak olabilir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Henüz bu yeni varlıklar ardından izlenen, tüm eklenen varlık olmayan diğer varlıklara başvurular varsa, bu örnekler için Not bağlamına da eklenir ve SaveChanges adlı bir sonraki zamandan veritabanına eklenir.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Var olan bir varlığa bağlamına ekleme  

' İniz varsa zaten bildiğiniz bir varlık veritabanı ancak bağlam üzerinde olan DB Attach yöntemi kullanarak varlık izlemek için size daha sonra şu anda bağlam tarafından izlenmiyor bulunmaktadır. Varlık bağlamı değişmemiş durumda olacaktır. Örneğin:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

SaveChanges ekli varlık herhangi bir değişiklik yapmadan çağrılırsa hiçbir değişiklik veritabanına yapılan olduğunu unutmayın. Varlık durumda olmasıdır.  

Var olan bir varlığa bağlamına eklemek için başka bir Unchanged olarak kendi durumunu değiştirmek için yoludur. Örneğin:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Bu örneklerin her ikisi için iliştirilmekte varlık değil henüz izlenen diğer varlıklara başvurular varsa, ardından bu yeni varlıklar ayrıca bağlı değişmeden durumunda içeriği dikkat edin.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Değiştirilen varlık içeriği ancak mevcut bir ekleme  

' İniz varsa zaten bildiğiniz bir varlık veritabanında ancak varlık ekleme ve değiştirme için durumunu ayarlamak için bağlam söyleyebilirsiniz sonra hangi değişiklikler yapılmıştır bulunmaktadır.
Örneğin:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

Değiştirilen için durum değiştirdiğinizde varlığın tüm özellikleri değiştirilmiş olarak işaretlenir ve SaveChanges çağrıldığında tüm özellik değerlerini veritabanına gönderilir.  

İliştirilmekte varlık değil henüz izlenen diğer varlıklara başvurular varsa, ardından bu yeni varlıklar bağlı değişmeden durumunda içeriği unutmayın; bunlar otomatik olarak değişiklik yapılamaz.
Değiştirilen işaretlenmiş olması gereken birden fazla varlık olduğunda durumu bunların her biri için ayrı ayrı ayarlamanız gerekir.  

## <a name="changing-the-state-of-a-tracked-entity"></a>İzlenen varlık durumu değiştirme  

Kendi girişinde durum özelliğini ayarlayarak zaten izlenmekte olan varlık durumunu değiştirebilirsiniz. Örneğin:  

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

Ekleme veya iliştirme zaten izlenen bir varlık için arama da varlık durumu değişikliği için kullanılabileceğini unutmayın. Örneğin, Attach Added durumda olan bir varlık için çağırma durumuna Unchanged olarak değişecektir.  

## <a name="insert-or-update-pattern"></a>Ekleme veya güncelleştirme deseni  

Yeni (bir veritabanı insert sonucu olarak) bir varlık eklemek veya mevcut olarak bir varlık eklemek ve (veritabanı güncelleştirmesindeki kaynaklanan) değiştirilmiş olarak işaretleyin için bazı uygulamalar için yaygın bir düzen olan birincil anahtar değerine bağlı olarak.
Örneğin, oluşturulan veritabanı tamsayı birincil anahtarları kullanırken anahtarla bir sıfır olarak yeni bir varlık ile mevcut olarak bir sıfır olmayan anahtarına sahip bir varlık değerlendirilecek yaygındır.
Bu düzen, birincil anahtar değeri üzerinde bir denetimi göre varlık durumu ayarlayarak gerçekleştirilebilir. Örneğin:  

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

Değiştirilen için durum değiştirdiğinizde varlığın tüm özellikleri değiştirilmiş olarak işaretlenir ve SaveChanges çağrıldığında tüm özellik değerlerini veritabanına gönderilir unutmayın.  
