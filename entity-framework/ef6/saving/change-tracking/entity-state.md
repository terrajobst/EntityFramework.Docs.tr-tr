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
# <a name="working-with-entity-states"></a><span data-ttu-id="dd912-102">Varlık durumlarıyla çalışma</span><span class="sxs-lookup"><span data-stu-id="dd912-102">Working with entity states</span></span>
<span data-ttu-id="dd912-103">Bu konu, bir bağlama nasıl varlık ekleneceğini ve ekleneceğini ve Entity Framework SaveChanges sırasında bunların nasıl işlem yapılacağını ele almaktadır.</span><span class="sxs-lookup"><span data-stu-id="dd912-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="dd912-104">Entity Framework, bir içeriğe bağlıyken varlıkların durumunu izlemenin bir bölümünü ele alır, ancak bağlantısı kesik veya N katmanlı senaryolarda, varlıklarınızın varlıklarınızın hangi durumlarına sahip olduğunu bilmesini sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd912-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="dd912-105">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="dd912-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="dd912-106">Varlık durumları ve SaveChanges</span><span class="sxs-lookup"><span data-stu-id="dd912-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="dd912-107">Varlık, EntityState numaralandırması tarafından tanımlanan beş durumdan birinde olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd912-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="dd912-108">Bu durumlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="dd912-108">These states are:</span></span>  

- <span data-ttu-id="dd912-109">Eklendi: varlık bağlam tarafından izleniyor ancak veritabanında henüz yok</span><span class="sxs-lookup"><span data-stu-id="dd912-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="dd912-110">Değiştirilmemiş: varlık bağlam tarafından izleniyor ve veritabanında var ve özellik değerleri veritabanındaki değerlerden değişmemiştir</span><span class="sxs-lookup"><span data-stu-id="dd912-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="dd912-111">Değiştirilen: varlık bağlam tarafından izleniyor ve veritabanında var ve özellik değerlerinin bazıları veya tümü değiştirildi</span><span class="sxs-lookup"><span data-stu-id="dd912-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="dd912-112">Silindi: varlık bağlam tarafından izleniyor ve veritabanında var, ancak SaveChanges bir sonraki sefer çağrıldığında veritabanından silinmek üzere işaretlendi</span><span class="sxs-lookup"><span data-stu-id="dd912-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="dd912-113">Ayrılmış: varlık bağlam tarafından izlenmiyor</span><span class="sxs-lookup"><span data-stu-id="dd912-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="dd912-114">SaveChanges farklı durumlardaki varlıklar için farklı şeyler yapar:</span><span class="sxs-lookup"><span data-stu-id="dd912-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="dd912-115">Değiştirilmemiş varlıklar SaveChanges 'e dokunmaz.</span><span class="sxs-lookup"><span data-stu-id="dd912-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="dd912-116">Güncelleştirmeler, değiştirilmemiş durumdaki varlıklar için veritabanına gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="dd912-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="dd912-117">Eklenen varlıklar veritabanına eklenir ve ardından SaveChanges döndürüldüğünde değişmeden hale gelir.</span><span class="sxs-lookup"><span data-stu-id="dd912-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="dd912-118">Değiştirilen varlıklar veritabanında güncelleştirilir ve SaveChanges döndürüldüğünde değiştirilmez.</span><span class="sxs-lookup"><span data-stu-id="dd912-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="dd912-119">Silinen varlıklar veritabanından silinir ve sonra bağlamdan ayrılır.</span><span class="sxs-lookup"><span data-stu-id="dd912-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="dd912-120">Aşağıdaki örneklerde bir varlık veya bir varlık grafiğinin durumunun değiştirilebileceği yollar gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="dd912-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="dd912-121">Bağlama yeni bir varlık ekleniyor</span><span class="sxs-lookup"><span data-stu-id="dd912-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="dd912-122">DbSet üzerinde Add yöntemi çağırarak içeriğe yeni bir varlık eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="dd912-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="dd912-123">Bu, varlığı eklenen duruma geçirir, yani bu, SaveChanges 'in bir sonraki çağrılışında veritabanına eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="dd912-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="dd912-124">Örnek:</span><span class="sxs-lookup"><span data-stu-id="dd912-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="dd912-125">İçeriğine yeni bir varlık eklemenin başka bir yolu da, durumunu eklendi olarak değiştirkullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="dd912-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="dd912-126">Örnek:</span><span class="sxs-lookup"><span data-stu-id="dd912-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="dd912-127">Son olarak, daha önce izlenmekte olan başka bir varlığa bağlanarak içeriğe yeni bir varlık ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd912-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="dd912-128">Bu, yeni varlığı başka bir varlığın koleksiyon gezintisi özelliğine ekleyerek veya başka bir varlığın başvuru gezintisi özelliğini yeni varlığa işaret etmek üzere ayarlayarak olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd912-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="dd912-129">Örnek:</span><span class="sxs-lookup"><span data-stu-id="dd912-129">For example:</span></span>  

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

<span data-ttu-id="dd912-130">Eklenmekte olan varlık henüz izlenmeyen diğer varlıklara başvurular içeriyorsa, bu yeni varlıkların de bağlamına ekleneceğini ve SaveChanges 'in bir sonraki çağrılışında veritabanına eklenebileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="dd912-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="dd912-131">Varolan bir varlığı bağlama iliştirme</span><span class="sxs-lookup"><span data-stu-id="dd912-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="dd912-132">Veritabanında zaten var olduğunu bildiğiniz ancak şu anda bağlam tarafından izlenmekte olan bir varlığınız varsa, DbSet üzerinde Attach metodunu kullanarak varlığı izlemek için bağlama söyleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd912-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="dd912-133">Varlık bağlamda değiştirilmemiş durumda olacaktır.</span><span class="sxs-lookup"><span data-stu-id="dd912-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="dd912-134">Örnek:</span><span class="sxs-lookup"><span data-stu-id="dd912-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="dd912-135">SaveChanges, eklenen varlığın başka bir düzenlemesi yapılmadan çağrılırsa veritabanına hiçbir değişiklik yapılmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="dd912-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="dd912-136">Bunun nedeni, varlığın Unchanged durumda olması.</span><span class="sxs-lookup"><span data-stu-id="dd912-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="dd912-137">Var olan bir varlığı bağlama eklemek için başka bir yöntem de durumunu değiştirilmemiş olarak değiştirir.</span><span class="sxs-lookup"><span data-stu-id="dd912-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="dd912-138">Örnek:</span><span class="sxs-lookup"><span data-stu-id="dd912-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="dd912-139">Bu örneklerin her ikisi için de iliştirilmekte olan varlık henüz izlenmeyen diğer varlıklara başvurular içeriyorsa, bu yeni varlıkların de Unchanged durumunda içeriğe eklendiği unutulmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="dd912-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="dd912-140">Var olan ancak değiştirilen bir varlığı içeriğe ekleme</span><span class="sxs-lookup"><span data-stu-id="dd912-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="dd912-141">Veritabanında zaten var olan ancak hangi değişikliklerin yapıldığını bildiğiniz bir varlığınız varsa, bağlamı eklemek ve durumunu değiştirilme olarak ayarlamak için bağlam söyleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd912-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="dd912-142">Örnek:</span><span class="sxs-lookup"><span data-stu-id="dd912-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="dd912-143">Durumu değiştirildi olarak değiştirdiğinizde, varlığın tüm özellikleri değiştirilmiş olarak işaretlenir ve SaveChanges çağrıldığında tüm özellik değerleri veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="dd912-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="dd912-144">İliştirilmekte olan varlık henüz izlenmeyen diğer varlıklara başvurular içeriyorsa, bu yeni varlıkların, değiştirilmemiş durumdaki bir içeriğe eklendiği, otomatik olarak değiştirilme yapılmayacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="dd912-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="dd912-145">Değiştirilmiş olarak işaretlenmesi gereken birden çok varlık varsa, bu varlıkların her biri için durumu ayrı ayrı ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="dd912-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="dd912-146">İzlenen bir varlığın durumunu değiştirme</span><span class="sxs-lookup"><span data-stu-id="dd912-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="dd912-147">Girdisinde durum özelliğini ayarlayarak zaten izlenmekte olan bir varlığın durumunu değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd912-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="dd912-148">Örnek:</span><span class="sxs-lookup"><span data-stu-id="dd912-148">For example:</span></span>  

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

<span data-ttu-id="dd912-149">Zaten izlenen bir varlık için Add veya Attach çağrısı, varlık durumunu değiştirmek için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd912-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="dd912-150">Örneğin, şu anda eklenmiş durumdaki bir varlık için Iliştirme çağrısı, durumunu değiştirilmemiş olarak değiştirecek.</span><span class="sxs-lookup"><span data-stu-id="dd912-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="dd912-151">Ekleme veya güncelleştirme stili</span><span class="sxs-lookup"><span data-stu-id="dd912-151">Insert or update pattern</span></span>  

<span data-ttu-id="dd912-152">Bazı uygulamalar için ortak bir model, yeni olarak bir varlık eklemektir (veritabanı eklemeye yol açar) ya da bir varlığı mevcut olarak ekler ve birincil anahtarın değerine bağlı olarak değiştirilmiş olarak işaretler (bir veritabanı güncelleştirmesine yol açar).</span><span class="sxs-lookup"><span data-stu-id="dd912-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="dd912-153">Örneğin, veritabanı tarafından oluşturulan tamsayı birincil anahtarları kullanılırken, bir varlığı, sıfır olmayan bir anahtarla yeni bir varlık ve var olan sıfır olmayan bir anahtarla değerlendirmek yaygındır.</span><span class="sxs-lookup"><span data-stu-id="dd912-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="dd912-154">Bu model, birincil anahtar değerinin bir denetimine göre varlık durumu ayarlanarak elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="dd912-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="dd912-155">Örnek:</span><span class="sxs-lookup"><span data-stu-id="dd912-155">For example:</span></span>  

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

<span data-ttu-id="dd912-156">Durumu değiştirildi olarak değiştirdiğinizde varlığın tüm özellikleri değiştirilmiş olarak işaretlenir ve SaveChanges çağrıldığında tüm özellik değerlerinin veritabanına gönderileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="dd912-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
