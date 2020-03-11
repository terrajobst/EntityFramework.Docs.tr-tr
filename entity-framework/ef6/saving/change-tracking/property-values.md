---
title: Özellik değerleriyle çalışma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: d8a18182754980d79b71df3f227b30c4ce40366f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416969"
---
# <a name="working-with-property-values"></a><span data-ttu-id="f25b2-102">Özellik değerleriyle çalışma</span><span class="sxs-lookup"><span data-stu-id="f25b2-102">Working with property values</span></span>
<span data-ttu-id="f25b2-103">Çoğu bölüm için Entity Framework durum, özgün değerler ve varlık örneklerinizin özelliklerinin güncel değerlerini takip eder.</span><span class="sxs-lookup"><span data-stu-id="f25b2-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="f25b2-104">Ancak, bağlantısı kesik olan senaryolar gibi bazı durumlar olabilir. Bu bilgiler, EF 'in özelliklerle ilgili olduğunu göstermek veya değiştirmek istediğiniz yerdir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="f25b2-105">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="f25b2-106">Entity Framework, izlenen bir varlığın her özelliği için iki değeri izler.</span><span class="sxs-lookup"><span data-stu-id="f25b2-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="f25b2-107">Geçerli değer, ad olarak, varlıktaki özelliğin geçerli değerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="f25b2-108">Özgün değer, özelliğin varlık veritabanından sorgulandığında veya bağlama eklenmiş olduğu değerdir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="f25b2-109">Özellik değerleriyle çalışmak için iki genel mekanizma vardır:</span><span class="sxs-lookup"><span data-stu-id="f25b2-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="f25b2-110">Tek bir özelliğin değeri, özellik yöntemi kullanılarak kesin olarak belirlenmiş bir şekilde elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="f25b2-111">Bir varlığın tüm özelliklerinin değerleri, DbPropertyValues nesnesine okunabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="f25b2-112">Daha sonra DbPropertyValues, özellik değerlerinin okunve ayarlamaya olanak tanımak için sözlük benzeri bir nesne gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="f25b2-113">Bir DbPropertyValues nesnesindeki değerler, başka bir DbPropertyValues nesnesindeki değerlerden veya varlığın başka bir kopyası veya basit bir veri aktarımı nesnesi (DTO) gibi diğer bir nesne içindeki değerlerden ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="f25b2-114">Aşağıdaki bölümlerde, yukarıdaki mekanizmaların her ikisini de kullanma örnekleri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="f25b2-115">Tek bir özelliğin geçerli veya orijinal değerini alma ve ayarlama</span><span class="sxs-lookup"><span data-stu-id="f25b2-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="f25b2-116">Aşağıdaki örnekte, bir özelliğin geçerli değerinin nasıl okun, sonra yeni bir değere nasıl ayarlanacağı gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="f25b2-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="f25b2-117">Özgün değeri okumak veya ayarlamak için CurrentValue özelliği yerine OriginalValue özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="f25b2-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="f25b2-118">Özellik adını belirtmek için bir dize kullanıldığında döndürülen değerin "Object" olarak yazılmış olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f25b2-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="f25b2-119">Diğer taraftan, bir lambda ifadesi kullanılırsa döndürülen değer kesin olarak yazılır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="f25b2-120">Özellik değerinin bu şekilde ayarlanması, yeni değer eski değerden farklıysa özelliği yalnızca değiştirilen olarak işaretler.</span><span class="sxs-lookup"><span data-stu-id="f25b2-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="f25b2-121">Bu şekilde bir özellik değeri ayarlandığında, otomatik DetectChanges kapalı olsa bile değişiklik otomatik olarak algılanır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="f25b2-122">Eşlenmemiş bir özelliğin geçerli değerini alma ve ayarlama</span><span class="sxs-lookup"><span data-stu-id="f25b2-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="f25b2-123">Veritabanına eşlenmemiş bir özelliğin geçerli değeri de okunabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="f25b2-124">Eşlenmemiş özelliğe bir örnek, blogda bir RssLink özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="f25b2-125">Bu değer blogID temelinde hesaplanabilir ve bu nedenle veritabanında depolanması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f25b2-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="f25b2-126">Örnek:</span><span class="sxs-lookup"><span data-stu-id="f25b2-126">For example:</span></span>  

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

<span data-ttu-id="f25b2-127">Özellik bir ayarlayıcı kullanıma sunarsa geçerli değer de ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="f25b2-128">Eşlenmemiş özelliklerin değerlerini okumak, eşlenmemiş özelliklerin Entity Framework doğrulanması gerçekleştirilirken faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="f25b2-129">Aynı nedenden dolayı geçerli değerler, şu anda bağlam tarafından izlenmeyen varlıkların özellikleri için okunabilir ve ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="f25b2-130">Örnek:</span><span class="sxs-lookup"><span data-stu-id="f25b2-130">For example:</span></span>  

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

<span data-ttu-id="f25b2-131">Özgün değerlerin eşlenmemiş özellikler için veya bağlam tarafından izlenmeyen varlıkların özellikleri için kullanılamayacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f25b2-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="f25b2-132">Bir özelliğin değiştirilmiş olarak işaretlenip işaretlenmediği denetleniyor</span><span class="sxs-lookup"><span data-stu-id="f25b2-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="f25b2-133">Aşağıdaki örnekte, tek bir özelliğin değiştirilmiş olarak işaretlenip işaretlenmediğini nasıl denetleneceği gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="f25b2-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="f25b2-134">Değiştirilen özelliklerin değerleri, SaveChanges çağrıldığında veritabanına güncelleştirmeler olarak gönderilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="f25b2-135">Bir özelliği değiştirilmiş olarak işaretleme</span><span class="sxs-lookup"><span data-stu-id="f25b2-135">Marking a property as modified</span></span>  

<span data-ttu-id="f25b2-136">Aşağıdaki örnekte, tek bir özelliğin değiştirilme olarak işaretlenme nasıl zorlanacağı gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="f25b2-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="f25b2-137">Özelliğin geçerli değeri özgün değeri ile aynı olsa bile, bir özelliği değiştirilmiş olarak işaretlemek, SaveChanges çağrıldığında özelliğin veritabanına gönderilmesini zorlar.</span><span class="sxs-lookup"><span data-stu-id="f25b2-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="f25b2-138">Tek bir özelliğin değiştirilmiş olarak işaretlendikten sonra değiştirilmemesi için sıfırlanması mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="f25b2-139">Bu, gelecekteki sürümlerde desteklemeyi planladığımız bir şeydir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="f25b2-140">Bir varlığın tüm özellikleri için geçerli, orijinal ve veritabanı değerlerini okuma</span><span class="sxs-lookup"><span data-stu-id="f25b2-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="f25b2-141">Aşağıdaki örnekte, bir varlığın eşlenen tüm özellikleri için geçerli değerlerin, özgün değerlerin ve gerçekten veritabanındaki değerlerin nasıl okunacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

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

<span data-ttu-id="f25b2-142">Geçerli değerler, varlık özelliklerinin Şu anda içerdiği değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="f25b2-143">Özgün değerler, varlık sorgulandığında veritabanından okunan değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="f25b2-144">Veritabanı değerleri şu anda veritabanında depolandıkları değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="f25b2-145">Veritabanı değerlerinin alınması, varlığın veritabanına yönelik eşzamanlı bir düzenleme başka bir kullanıcı tarafından yapıldığında olduğu gibi, veritabanındaki değerler değiştirildiğinde yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="f25b2-146">Geçerli veya orijinal değerleri başka bir nesneden ayarlama</span><span class="sxs-lookup"><span data-stu-id="f25b2-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="f25b2-147">İzlenen bir varlığın geçerli veya orijinal değerleri başka bir nesneden değerler kopyalanarak güncellenebilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="f25b2-148">Örnek:</span><span class="sxs-lookup"><span data-stu-id="f25b2-148">For example:</span></span>  

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

<span data-ttu-id="f25b2-149">Yukarıdaki kodu çalıştırmak şu şekilde yazdırılır:</span><span class="sxs-lookup"><span data-stu-id="f25b2-149">Running the code above will print out:</span></span>  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="f25b2-150">Bu teknik bazen bir varlık, bir hizmet çağrısından alınan değerlerle veya n katmanlı bir uygulamadaki bir istemciden güncelleştirilirken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="f25b2-151">Kullanılan nesnenin, adları varlıkla eşleşen özelliklere sahip olduğu sürece varlıkla aynı türde olması gerekmez,.</span><span class="sxs-lookup"><span data-stu-id="f25b2-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="f25b2-152">Yukarıdaki örnekte, özgün değerleri güncelleştirmek için bir BlogDTO örneği kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="f25b2-153">Yalnızca diğer nesneden kopyalandıklarında farklı değerlere ayarlanmış özelliklerin değiştirilmiş olarak işaretleneceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f25b2-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="f25b2-154">Sözlükten geçerli veya orijinal değerleri ayarlama</span><span class="sxs-lookup"><span data-stu-id="f25b2-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="f25b2-155">İzlenen bir varlığın geçerli veya orijinal değerleri, bir sözlükten veya başka bir veri yapısından değerler kopyalanarak güncellenebilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="f25b2-156">Örnek:</span><span class="sxs-lookup"><span data-stu-id="f25b2-156">For example:</span></span>  

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

<span data-ttu-id="f25b2-157">Özgün değerleri ayarlamak için CurrentValues özelliği yerine OriginalValues özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="f25b2-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="f25b2-158">Özelliği kullanarak bir sözlükten geçerli veya orijinal değerleri ayarlama</span><span class="sxs-lookup"><span data-stu-id="f25b2-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="f25b2-159">Yukarıda gösterildiği gibi CurrentValues veya OriginalValues kullanmanın bir alternatifi, her bir özelliğin değerini ayarlamak için Property metodunu kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="f25b2-160">Bu, karmaşık özelliklerin değerlerini ayarlamanız gerektiğinde tercih edilebilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="f25b2-161">Örnek:</span><span class="sxs-lookup"><span data-stu-id="f25b2-161">For example:</span></span>  

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

<span data-ttu-id="f25b2-162">Yukarıdaki örnekte, karmaşık özelliklere, noktalı adlar kullanılarak erişilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="f25b2-163">Karmaşık özelliklere erişmenin diğer yolları için, özellikle karmaşık özellikler hakkında bu konunun ilerleyen bölümlerindeki iki bölüme bakın.</span><span class="sxs-lookup"><span data-stu-id="f25b2-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="f25b2-164">Geçerli, orijinal veya veritabanı değerlerini içeren kopyalanmış nesne oluşturma</span><span class="sxs-lookup"><span data-stu-id="f25b2-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="f25b2-165">CurrentValues, OriginalValues veya GetDatabaseValues 'tan döndürülen DbPropertyValues nesnesi varlığın bir kopyasını oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="f25b2-166">Bu kopya, onu oluşturmak için kullanılan DbPropertyValues nesnesinden özellik değerlerini içerecektir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="f25b2-167">Örnek:</span><span class="sxs-lookup"><span data-stu-id="f25b2-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="f25b2-168">Döndürülen nesnenin varlık değil olduğunu ve bağlam tarafından izlenmediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f25b2-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="f25b2-169">Döndürülen nesnenin aynı zamanda diğer nesnelere ayarlanmış hiçbir ilişkisi yok.</span><span class="sxs-lookup"><span data-stu-id="f25b2-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="f25b2-170">Kopyalanmış nesne, özellikle de belirli bir türdeki nesnelere veri bağlamayı içeren bir kullanıcı arabiriminin kullanıldığı eşzamanlı güncelleştirmelerle ilgili sorunları çözmek için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="f25b2-171">Karmaşık özelliklerin geçerli veya orijinal değerlerini alma ve ayarlama</span><span class="sxs-lookup"><span data-stu-id="f25b2-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="f25b2-172">Tüm karmaşık bir nesnenin değeri, basit bir özellik için olduğu gibi, Property yöntemi kullanılarak okunabilir ve ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="f25b2-173">Ayrıca, karmaşık nesnenin detayına gidebilir ve bu nesnenin özelliklerini okuyabilir veya ayarlayabilirsiniz, hatta iç içe geçmiş bir nesne.</span><span class="sxs-lookup"><span data-stu-id="f25b2-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="f25b2-174">İşte bazı örnekler:</span><span class="sxs-lookup"><span data-stu-id="f25b2-174">Here are some examples:</span></span>  

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

<span data-ttu-id="f25b2-175">Özgün bir değer almak veya ayarlamak için CurrentValue özelliği yerine OriginalValue özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="f25b2-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="f25b2-176">Özelliğin veya ComplexProperty yönteminin karmaşık bir özelliğe erişmek için kullanılabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f25b2-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="f25b2-177">Ancak, karmaşık nesneye ek özellik veya ComplexProperty çağrıları ile detaya gitmek istiyorsanız ComplexProperty yöntemi kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="f25b2-178">Karmaşık özelliklere erişmek için DbPropertyValues kullanma</span><span class="sxs-lookup"><span data-stu-id="f25b2-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="f25b2-179">Bir varlık için geçerli, orijinal veya veritabanı değerlerini almak üzere CurrentValues, OriginalValues veya GetDatabaseValues kullandığınızda, herhangi bir karmaşık özellik değeri iç içe geçmiş DbPropertyValues nesneleri olarak döndürülür.</span><span class="sxs-lookup"><span data-stu-id="f25b2-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="f25b2-180">Bu iç içe geçmiş nesneler daha sonra karmaşık nesnenin değerlerini almak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f25b2-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="f25b2-181">Örneğin, aşağıdaki yöntem, herhangi bir karmaşık özellik ve iç içe geçmiş karmaşık özellikler dahil olmak üzere tüm özelliklerin değerlerini yazdıracaktır.</span><span class="sxs-lookup"><span data-stu-id="f25b2-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

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

<span data-ttu-id="f25b2-182">Tüm geçerli özellik değerlerini yazdırmak için yöntem şöyle çağrılır:</span><span class="sxs-lookup"><span data-stu-id="f25b2-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
