---
title: Varlık durumları - EF6 ile çalışma
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: e5f9ca4aa41e64141fa63a1e5fcf4d4775d67d24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899658"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="f80ed-102">Varlık durumları ile çalışma</span><span class="sxs-lookup"><span data-stu-id="f80ed-102">Working with entity states</span></span>
<span data-ttu-id="f80ed-103">Bu konuda, ekleme ve varlıklar için bir bağlam eklemek ve Entity Framework SaveChanges sırasında bunları nasıl işlediğini ele alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="f80ed-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="f80ed-104">Entity Framework için bir bağlam bağlı, ancak bağlantısı kesilmiş veya N-katmanlı senaryolarda durum varlıklarınızı bilmeniz EF sağlayabilirsiniz varlıkların durumunu olmalıdır izleme üstlenir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="f80ed-105">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="f80ed-106">Varlık durumları ve SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f80ed-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="f80ed-107">Bir varlık beş durumlardan birinde EntityState numaralandırmasıyla tanımlanan olabilir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="f80ed-108">Bu durumlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="f80ed-108">These states are:</span></span>  

- <span data-ttu-id="f80ed-109">Eklenen: Varlık bağlam tarafından izlenen ancak henüz veritabanında mevcut değil</span><span class="sxs-lookup"><span data-stu-id="f80ed-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="f80ed-110">Değiştirilmemiş: Varlık bağlam tarafından izlenen ve veritabanında var ve özellik değerlerini veritabanındaki değerlerinin değişmedi</span><span class="sxs-lookup"><span data-stu-id="f80ed-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="f80ed-111">Değiştirilen: Varlık bağlam tarafından izleniyor ve veritabanında var ve bazı veya tüm özellik değerleri değiştirilmiş</span><span class="sxs-lookup"><span data-stu-id="f80ed-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="f80ed-112">Silinen: Varlık bağlam tarafından izleniyor ve veritabanında var, ancak silinmek üzere veritabanından SaveChanges bir sonraki zamana işaretlendi</span><span class="sxs-lookup"><span data-stu-id="f80ed-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="f80ed-113">Ayrılmış: Varlık bağlam tarafından izlenmiyor</span><span class="sxs-lookup"><span data-stu-id="f80ed-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="f80ed-114">SaveChanges farklı durumlarda varlıklar için farklı şeyler yapar:</span><span class="sxs-lookup"><span data-stu-id="f80ed-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="f80ed-115">Değiştirilmemiş varlıkları SaveChanges tarafından kullanılmayan.</span><span class="sxs-lookup"><span data-stu-id="f80ed-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="f80ed-116">Güncelleştirmeleri değişmemiş durumda varlıklar için veritabanına gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="f80ed-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="f80ed-117">Eklenen varlıkları veritabanına eklenir ve ardından değişmedi haline SaveChanges döndürür.</span><span class="sxs-lookup"><span data-stu-id="f80ed-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="f80ed-118">Değiştirilmiş varlıklar veritabanında güncelleştirilen ve ardından değişmedi haline SaveChanges döndürür.</span><span class="sxs-lookup"><span data-stu-id="f80ed-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="f80ed-119">Silinen varlıklar veritabanından silinir ve ardından bağlamdan ayrılır.</span><span class="sxs-lookup"><span data-stu-id="f80ed-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="f80ed-120">Aşağıdaki örnekler bir varlık veya varlık graf durumunu değiştirilebilir yollarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="f80ed-121">Bağlam için yeni bir varlık ekleme</span><span class="sxs-lookup"><span data-stu-id="f80ed-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="f80ed-122">Yeni bir varlık üzerinde olan DB Ekle yöntemi çağırarak bağlamına eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="f80ed-123">Bu varlık eklenen durumuna, bunun veritabanına SaveChanges adlı bir sonraki açışınızda eklenir, yani koyar.</span><span class="sxs-lookup"><span data-stu-id="f80ed-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="f80ed-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f80ed-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="f80ed-125">Bağlam için yeni bir varlık eklemek için başka bir yol için eklenen durumunu değiştirmektir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="f80ed-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f80ed-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="f80ed-127">Son olarak, zaten izlenmekte olan başka bir varlık kadar takma tarafından yeni bir varlık bağlamına ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f80ed-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="f80ed-128">Bu, başka bir varlık koleksiyon gezinme özelliği için yeni bir varlık ekleme veya yeni varlığa işaret edecek şekilde başka bir varlık başvurusu gezinti özelliğini ayarlayarak olabilir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="f80ed-129">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f80ed-129">For example:</span></span>  

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

<span data-ttu-id="f80ed-130">Henüz bu yeni varlıklar ardından izlenen, tüm eklenen varlık olmayan diğer varlıklara başvurular varsa, bu örnekler için Not bağlamına da eklenir ve SaveChanges adlı bir sonraki zamandan veritabanına eklenir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="f80ed-131">Var olan bir varlığa bağlamına ekleme</span><span class="sxs-lookup"><span data-stu-id="f80ed-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="f80ed-132">' İniz varsa zaten bildiğiniz bir varlık veritabanı ancak bağlam üzerinde olan DB Attach yöntemi kullanarak varlık izlemek için size daha sonra şu anda bağlam tarafından izlenmiyor bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="f80ed-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="f80ed-133">Varlık bağlamı değişmemiş durumda olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f80ed-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="f80ed-134">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f80ed-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f80ed-135">SaveChanges ekli varlık herhangi bir değişiklik yapmadan çağrılırsa hiçbir değişiklik veritabanına yapılan olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f80ed-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="f80ed-136">Varlık durumda olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="f80ed-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="f80ed-137">Var olan bir varlığa bağlamına eklemek için başka bir Unchanged olarak kendi durumunu değiştirmek için yoludur.</span><span class="sxs-lookup"><span data-stu-id="f80ed-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="f80ed-138">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f80ed-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f80ed-139">Bu örneklerin her ikisi için iliştirilmekte varlık değil henüz izlenen diğer varlıklara başvurular varsa, ardından bu yeni varlıklar ayrıca bağlı değişmeden durumunda içeriği dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f80ed-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="f80ed-140">Değiştirilen varlık içeriği ancak mevcut bir ekleme</span><span class="sxs-lookup"><span data-stu-id="f80ed-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="f80ed-141">' İniz varsa zaten bildiğiniz bir varlık veritabanında ancak varlık ekleme ve değiştirme için durumunu ayarlamak için bağlam söyleyebilirsiniz sonra hangi değişiklikler yapılmıştır bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="f80ed-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="f80ed-142">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f80ed-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f80ed-143">Değiştirilen için durum değiştirdiğinizde varlığın tüm özellikleri değiştirilmiş olarak işaretlenir ve SaveChanges çağrıldığında tüm özellik değerlerini veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="f80ed-144">İliştirilmekte varlık değil henüz izlenen diğer varlıklara başvurular varsa, ardından bu yeni varlıklar bağlı değişmeden durumunda içeriği unutmayın; bunlar otomatik olarak değişiklik yapılamaz.</span><span class="sxs-lookup"><span data-stu-id="f80ed-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="f80ed-145">Değiştirilen işaretlenmiş olması gereken birden fazla varlık olduğunda durumu bunların her biri için ayrı ayrı ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="f80ed-146">İzlenen varlık durumu değiştirme</span><span class="sxs-lookup"><span data-stu-id="f80ed-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="f80ed-147">Kendi girişinde durum özelliğini ayarlayarak zaten izlenmekte olan varlık durumunu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f80ed-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="f80ed-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f80ed-148">For example:</span></span>  

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

<span data-ttu-id="f80ed-149">Ekleme veya iliştirme zaten izlenen bir varlık için arama da varlık durumu değişikliği için kullanılabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f80ed-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="f80ed-150">Örneğin, Attach Added durumda olan bir varlık için çağırma durumuna Unchanged olarak değişecektir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="f80ed-151">Ekleme veya güncelleştirme deseni</span><span class="sxs-lookup"><span data-stu-id="f80ed-151">Insert or update pattern</span></span>  

<span data-ttu-id="f80ed-152">Yeni (bir veritabanı insert sonucu olarak) bir varlık eklemek veya mevcut olarak bir varlık eklemek ve (veritabanı güncelleştirmesindeki kaynaklanan) değiştirilmiş olarak işaretleyin için bazı uygulamalar için yaygın bir düzen olan birincil anahtar değerine bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="f80ed-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="f80ed-153">Örneğin, oluşturulan veritabanı tamsayı birincil anahtarları kullanırken anahtarla bir sıfır olarak yeni bir varlık ile mevcut olarak bir sıfır olmayan anahtarına sahip bir varlık değerlendirilecek yaygındır.</span><span class="sxs-lookup"><span data-stu-id="f80ed-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="f80ed-154">Bu düzen, birincil anahtar değeri üzerinde bir denetimi göre varlık durumu ayarlayarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="f80ed-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="f80ed-155">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="f80ed-155">For example:</span></span>  

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

<span data-ttu-id="f80ed-156">Değiştirilen için durum değiştirdiğinizde varlığın tüm özellikleri değiştirilmiş olarak işaretlenir ve SaveChanges çağrıldığında tüm özellik değerlerini veritabanına gönderilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f80ed-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
