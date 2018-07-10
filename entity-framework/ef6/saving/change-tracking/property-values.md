---
title: Özellik değerleri - EF6 ile çalışma
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
caps.latest.revision: 3
ms.openlocfilehash: 07b71c9efe4e1fc3fd25a52c9cfb25f61e92f859
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912120"
---
# <a name="working-with-property-values"></a><span data-ttu-id="ff4a9-102">Özellik değerleri ile çalışma</span><span class="sxs-lookup"><span data-stu-id="ff4a9-102">Working with property values</span></span>
<span data-ttu-id="ff4a9-103">Çoğunlukla Entity Framework izleme durumu, özgün değer ve varlık örneklerinizin özelliklerin geçerli değerleri ilgileniriz.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="ff4a9-104">Ancak, bazı durumlarda, görüntülemek veya değiştirmek EF olan özellikler hakkında bilgi için istediğiniz - gibi bağlantısız senaryolar - olabilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="ff4a9-105">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="ff4a9-106">Entity Framework, izlenen bir varlığın her bir özellik için iki değeri izler.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="ff4a9-107">Geçerli değer olduğunda, adından da anlaşılacağı gibi varlıktaki özelliğinin geçerli değeri.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="ff4a9-108">Özgün değer varlık veritabanından sorgulanan veya bağlamına bağlı özelliği olan değerdir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="ff4a9-109">Özellik değerleri ile çalışmak için iki genel mekanizma vardır:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="ff4a9-110">Tek bir özellik değeri, özellik yöntemi kullanarak türü kesin belirlenmiş bir biçimde alınabilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="ff4a9-111">Bir varlığın tüm özellikleri için değer DbPropertyValues nesnede okunabilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="ff4a9-112">DbPropertyValues ardından okuyup ayarlamak özellik değerlerini izin vermek için bir sözlük benzeri nesnesi olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="ff4a9-113">Değerlerin DbPropertyValues nesnesindeki başka bir DbPropertyValues nesne değerleri veya başka bir kopyasını varlık veya basit veri aktarım nesnesini (DTO) gibi diğer bazı nesne değerleri ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="ff4a9-114">Aşağıdaki bölümlerde yukarıdaki mekanizmalardan ikisi de örnekleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="ff4a9-115">Alma ve tek bir özellik geçerli ya da orijinal değerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="ff4a9-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="ff4a9-116">Aşağıdaki örnek nasıl bir özelliğin geçerli değerini okunabilir ve ardından yeni bir değere ayarlayın gösterir:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="ff4a9-117">OriginalValue özelliği yerine CurrentValue özelliği okuma veya özgün değer ayarlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="ff4a9-118">Özellik adı belirtmek için kullanılan bir dize olduğunda döndürülen değer "nesne" yazıldığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="ff4a9-119">Bir lambda ifadesi kullanılıyorsa, diğer taraftan, döndürülen değerin türü kesin.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="ff4a9-120">Özellik değeri gibi bu ayarı yalnızca özellik yeni değer eski değerinden farklı ise değiştirilmiş olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="ff4a9-121">Bir özellik değeri, bu şekilde ayarlandığında bile AutoDetectChanges kapalı değişikliği otomatik olarak algılanır.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="ff4a9-122">Alma ve eşlenmemiş bir özelliğin geçerli değerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="ff4a9-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="ff4a9-123">Veritabanına eşlenmemiş bir özelliğinin geçerli değeri ayrıca okuyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="ff4a9-124">Eşlenmemiş bir özelliği örneği, bir Blog RssLink özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="ff4a9-125">Bu değer, üzerinde BlogId göre hesaplanır ve bu nedenle veritabanında depolanan gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="ff4a9-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

<span data-ttu-id="ff4a9-127">Özelliğin bir Ayarlayıcısı sunarsa geçerli değeri de ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="ff4a9-128">Entity Framework doğrulama eşlenmemiş özelliklerinin gerçekleştirirken eşlenmemiş özelliklerin değerlerini okuma yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="ff4a9-129">Aynı nedenden dolayı geçerli değerleri okuyun ve bağlam tarafından şu anda izlenmiyor varlıkların özelliklerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="ff4a9-130">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-130">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="ff4a9-131">Özgün değerlerine bağlam tarafından izlenmiyor varlıkların özelliklerini veya eşlenmemiş özellikler için kullanılabilir olmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="ff4a9-132">Bir özelliği değiştirildi olarak işaretlenmiş olup olmadığını denetleme</span><span class="sxs-lookup"><span data-stu-id="ff4a9-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="ff4a9-133">Aşağıdaki örnek, tek bir özellik değiştirilmiş olarak işaretlenmiş olup olmadığını denetlemek gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="ff4a9-134">Değiştirilen özelliklerin değerlerini SaveChanges çağrıldığında güncelleştirmeleri veritabanına gönderilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="ff4a9-135">Bir özelliği değiştirildi olarak işaretleme</span><span class="sxs-lookup"><span data-stu-id="ff4a9-135">Marking a property as modified</span></span>  

<span data-ttu-id="ff4a9-136">Aşağıdaki örnekte, değiştirilmiş olarak işaretlemek için tek bir özellik zorlamak gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="ff4a9-137">Bir özellik olması için bir güncelleştirme gönderme özelliği için veritabanı özelliğinin geçerli değeri, özgün değeri aynı olsa bile SaveChanges çağrıldığında değiştirilmiş kuvvetleri olarak işaretleniyor.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="ff4a9-138">Değiştirilmiş olarak işaretlenmiş sonra değiştirilmesi değil tek bir özellik sıfırlamak şu anda mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="ff4a9-139">Sorun gelecekteki bir sürümde desteklemeyi planlıyoruz budur.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="ff4a9-140">Geçerli ve özgün veritabanı değerlerini bir varlığın tüm özellikleri okuma</span><span class="sxs-lookup"><span data-stu-id="ff4a9-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="ff4a9-141">Aşağıdaki örnek, geçerli değerler, özgün değerler ve değerlerin gerçekten bir varlığın eşlenen tüm özellikler için veritabanında okuma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

<span data-ttu-id="ff4a9-142">Geçerli değerler şu anda entity öğesinin özellikleri içeren değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="ff4a9-143">Varlığı sorgulandığında veritabanından okunan değerleri özgün değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="ff4a9-144">Şu anda veritabanında depolanan gibi veritabanı değerlerini değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="ff4a9-145">Veritabanı değerlerini alma veritabanındaki değerlerin eşzamanlı düzenlemek için veritabanı başka bir kullanıcı tarafından yapılmadığını gibi varlık sorgulandı beri değişmiş olabilir durumlarda kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="ff4a9-146">Geçerli ya da orijinal değerleri başka bir nesneden ayarlama</span><span class="sxs-lookup"><span data-stu-id="ff4a9-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="ff4a9-147">İzlenen bir varlığın geçerli ya da orijinal değerleri başka bir nesneden değerleri kopyalayarak güncelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="ff4a9-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-148">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

<span data-ttu-id="ff4a9-149">Yukarıdaki kodu çalıştıran verecektir:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-149">Running the code above will print out:</span></span>  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="ff4a9-150">Bu teknik bazen bir varlık hizmeti çağrısı veya istemci n katmanlı bir uygulama olarak alınan değerlerle güncelleştirirken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="ff4a9-151">Varlık adları eşleşen özelliklere sahip olduğu sürece, varlık olarak aynı türde olacak şekilde kullanılan nesne olmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="ff4a9-152">Yukarıdaki örnekte, BlogDTO örneği, orijinal değerleri güncelleştirmek üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="ff4a9-153">Diğer nesneden kopyaladığınız değerleri farklı şekilde ayarlanan özellikler değiştirilmiş olarak işaretlenir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="ff4a9-154">Geçerli ya da orijinal değerleri sözlükten ayarlama</span><span class="sxs-lookup"><span data-stu-id="ff4a9-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="ff4a9-155">İzlenen bir varlığın geçerli ya da orijinal değerleri sözlüğü ya da başka bir veri yapısına değerlerini kopyalayarak güncelleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="ff4a9-156">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-156">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

<span data-ttu-id="ff4a9-157">OriginalValues özelliği CurrentValues özelliği yerine özgün değerleri ayarlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="ff4a9-158">Geçerli ya da orijinal değerleri özelliğini kullanarak bir sözlükten ayarlama</span><span class="sxs-lookup"><span data-stu-id="ff4a9-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="ff4a9-159">CurrentValues veya yukarıda da gösterildiği gibi OriginalValues kullanmaya alternatif her bir özellik değerini ayarlamak için özellik yöntemini kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="ff4a9-160">Karmaşık özelliklerin değerlerini ayarlamak, ihtiyacınız olduğunda bu tercih olabilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="ff4a9-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-161">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

<span data-ttu-id="ff4a9-162">Karmaşık özellikler yukarıdaki örnekte noktalı adları kullanılarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="ff4a9-163">Karmaşık özelliklerine erişmek kullanabileceğiniz diğer yöntemler, özellikle karmaşık özellikler hakkında bu konunun ilerleyen bölümlerinde iki bölümlere bakın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="ff4a9-164">Geçerli, özgün ya da veritabanı değerlerini içeren bir kopyalanan nesne oluşturma</span><span class="sxs-lookup"><span data-stu-id="ff4a9-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="ff4a9-165">DbPropertyValues nesne OriginalValues, CurrentValues döndürülen veya GetDatabaseValues varlık bir kopyasını oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="ff4a9-166">Bu kopya oluşturmak için kullanılan DbPropertyValues nesnesinden özellik değerlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="ff4a9-167">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="ff4a9-168">Döndürülen nesne varlık değildir ve bağlam tarafından izlenmiyor unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="ff4a9-169">Döndürülen nesne ilişkileri diğer nesnelere ayarlamak da yok.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="ff4a9-170">Kopyalanan nesne veritabanına özellikle veri bağlama için belirli bir türden nesneleri içeren bir kullanıcı Arabirimi kullanıldığı yerin eş zamanlı güncelleştirmelerin ilgili sorunları çözmek için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="ff4a9-171">Alma ve karmaşık özelliklerin geçerli ya da özgün değerlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="ff4a9-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="ff4a9-172">Karmaşık bir nesnenin tamamını değerini okuma ve özellik yöntemi kullanarak basit bir özellik için olabilir set olabilir.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="ff4a9-173">Buna ek olarak, karmaşık bir nesne ve söz konusu nesne veya iç içe bir nesneyi okuma ya da ayarlanmış özelliği detaya gidebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="ff4a9-174">Bazı örnekler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-174">Here are some examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

<span data-ttu-id="ff4a9-175">OriginalValue özelliği yerine CurrentValue özelliği almak ve özgün değeri ayarlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="ff4a9-176">Karmaşık bir özelliğe erişmek için özelliği veya ComplexProperty yöntemi kullanılabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="ff4a9-177">Ancak, ek özellik ile karmaşık nesne incelemek istediğiniz veya ComplexProperty çağırır ComplexProperty yöntemi kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="ff4a9-178">Karmaşık özelliklerine erişmek için DbPropertyValues kullanma</span><span class="sxs-lookup"><span data-stu-id="ff4a9-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="ff4a9-179">Ne zaman geçerli özgün almak için CurrentValues, OriginalValues veya GetDatabaseValues kullanın veya veritabanı değerlerini bir varlık için herhangi bir karmaşık özelliklerin değerlerini iç içe geçmiş DbPropertyValues nesneler olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="ff4a9-180">Bu nesneleri can iç içe ardından karmaşık bir nesnenin değerlerini almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="ff4a9-181">Örneğin, aşağıdaki yöntemi değerleri herhangi bir karmaşık özellikler ve iç içe geçmiş karmaşık özellikler dahil olmak üzere tüm özelliklerinin değerleri yazdırır.</span><span class="sxs-lookup"><span data-stu-id="ff4a9-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

<span data-ttu-id="ff4a9-182">Tüm geçerli özellik değerleri print yöntemi için şu şekilde çağrılır:</span><span class="sxs-lookup"><span data-stu-id="ff4a9-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
