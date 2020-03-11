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
# <a name="working-with-property-values"></a>Özellik değerleriyle çalışma
Çoğu bölüm için Entity Framework durum, özgün değerler ve varlık örneklerinizin özelliklerinin güncel değerlerini takip eder. Ancak, bağlantısı kesik olan senaryolar gibi bazı durumlar olabilir. Bu bilgiler, EF 'in özelliklerle ilgili olduğunu göstermek veya değiştirmek istediğiniz yerdir. Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

Entity Framework, izlenen bir varlığın her özelliği için iki değeri izler. Geçerli değer, ad olarak, varlıktaki özelliğin geçerli değerini gösterir. Özgün değer, özelliğin varlık veritabanından sorgulandığında veya bağlama eklenmiş olduğu değerdir.  

Özellik değerleriyle çalışmak için iki genel mekanizma vardır:  

- Tek bir özelliğin değeri, özellik yöntemi kullanılarak kesin olarak belirlenmiş bir şekilde elde edilebilir.  
- Bir varlığın tüm özelliklerinin değerleri, DbPropertyValues nesnesine okunabilir. Daha sonra DbPropertyValues, özellik değerlerinin okunve ayarlamaya olanak tanımak için sözlük benzeri bir nesne gibi davranır. Bir DbPropertyValues nesnesindeki değerler, başka bir DbPropertyValues nesnesindeki değerlerden veya varlığın başka bir kopyası veya basit bir veri aktarımı nesnesi (DTO) gibi diğer bir nesne içindeki değerlerden ayarlanabilir.  

Aşağıdaki bölümlerde, yukarıdaki mekanizmaların her ikisini de kullanma örnekleri gösterilmektedir.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Tek bir özelliğin geçerli veya orijinal değerini alma ve ayarlama  

Aşağıdaki örnekte, bir özelliğin geçerli değerinin nasıl okun, sonra yeni bir değere nasıl ayarlanacağı gösterilmektedir:  

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

Özgün değeri okumak veya ayarlamak için CurrentValue özelliği yerine OriginalValue özelliğini kullanın.  

Özellik adını belirtmek için bir dize kullanıldığında döndürülen değerin "Object" olarak yazılmış olduğunu unutmayın. Diğer taraftan, bir lambda ifadesi kullanılırsa döndürülen değer kesin olarak yazılır.  

Özellik değerinin bu şekilde ayarlanması, yeni değer eski değerden farklıysa özelliği yalnızca değiştirilen olarak işaretler.  

Bu şekilde bir özellik değeri ayarlandığında, otomatik DetectChanges kapalı olsa bile değişiklik otomatik olarak algılanır.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Eşlenmemiş bir özelliğin geçerli değerini alma ve ayarlama  

Veritabanına eşlenmemiş bir özelliğin geçerli değeri de okunabilir. Eşlenmemiş özelliğe bir örnek, blogda bir RssLink özelliği olabilir. Bu değer blogID temelinde hesaplanabilir ve bu nedenle veritabanında depolanması gerekmez. Örnek:  

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

Özellik bir ayarlayıcı kullanıma sunarsa geçerli değer de ayarlanabilir.  

Eşlenmemiş özelliklerin değerlerini okumak, eşlenmemiş özelliklerin Entity Framework doğrulanması gerçekleştirilirken faydalıdır. Aynı nedenden dolayı geçerli değerler, şu anda bağlam tarafından izlenmeyen varlıkların özellikleri için okunabilir ve ayarlanabilir. Örnek:  

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

Özgün değerlerin eşlenmemiş özellikler için veya bağlam tarafından izlenmeyen varlıkların özellikleri için kullanılamayacağını unutmayın.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Bir özelliğin değiştirilmiş olarak işaretlenip işaretlenmediği denetleniyor  

Aşağıdaki örnekte, tek bir özelliğin değiştirilmiş olarak işaretlenip işaretlenmediğini nasıl denetleneceği gösterilmektedir:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Değiştirilen özelliklerin değerleri, SaveChanges çağrıldığında veritabanına güncelleştirmeler olarak gönderilir.  

##  <a name="marking-a-property-as-modified"></a>Bir özelliği değiştirilmiş olarak işaretleme  

Aşağıdaki örnekte, tek bir özelliğin değiştirilme olarak işaretlenme nasıl zorlanacağı gösterilmektedir:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Özelliğin geçerli değeri özgün değeri ile aynı olsa bile, bir özelliği değiştirilmiş olarak işaretlemek, SaveChanges çağrıldığında özelliğin veritabanına gönderilmesini zorlar.  

Tek bir özelliğin değiştirilmiş olarak işaretlendikten sonra değiştirilmemesi için sıfırlanması mümkün değildir. Bu, gelecekteki sürümlerde desteklemeyi planladığımız bir şeydir.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Bir varlığın tüm özellikleri için geçerli, orijinal ve veritabanı değerlerini okuma  

Aşağıdaki örnekte, bir varlığın eşlenen tüm özellikleri için geçerli değerlerin, özgün değerlerin ve gerçekten veritabanındaki değerlerin nasıl okunacağı gösterilmektedir.  

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

Geçerli değerler, varlık özelliklerinin Şu anda içerdiği değerlerdir. Özgün değerler, varlık sorgulandığında veritabanından okunan değerlerdir. Veritabanı değerleri şu anda veritabanında depolandıkları değerlerdir. Veritabanı değerlerinin alınması, varlığın veritabanına yönelik eşzamanlı bir düzenleme başka bir kullanıcı tarafından yapıldığında olduğu gibi, veritabanındaki değerler değiştirildiğinde yararlı olabilir.  

## <a name="setting-current-or-original-values-from-another-object"></a>Geçerli veya orijinal değerleri başka bir nesneden ayarlama  

İzlenen bir varlığın geçerli veya orijinal değerleri başka bir nesneden değerler kopyalanarak güncellenebilir. Örnek:  

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

Yukarıdaki kodu çalıştırmak şu şekilde yazdırılır:  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Bu teknik bazen bir varlık, bir hizmet çağrısından alınan değerlerle veya n katmanlı bir uygulamadaki bir istemciden güncelleştirilirken kullanılır. Kullanılan nesnenin, adları varlıkla eşleşen özelliklere sahip olduğu sürece varlıkla aynı türde olması gerekmez,. Yukarıdaki örnekte, özgün değerleri güncelleştirmek için bir BlogDTO örneği kullanılır.  

Yalnızca diğer nesneden kopyalandıklarında farklı değerlere ayarlanmış özelliklerin değiştirilmiş olarak işaretleneceğini unutmayın.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Sözlükten geçerli veya orijinal değerleri ayarlama  

İzlenen bir varlığın geçerli veya orijinal değerleri, bir sözlükten veya başka bir veri yapısından değerler kopyalanarak güncellenebilir. Örnek:  

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

Özgün değerleri ayarlamak için CurrentValues özelliği yerine OriginalValues özelliğini kullanın.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Özelliği kullanarak bir sözlükten geçerli veya orijinal değerleri ayarlama  

Yukarıda gösterildiği gibi CurrentValues veya OriginalValues kullanmanın bir alternatifi, her bir özelliğin değerini ayarlamak için Property metodunu kullanmaktır. Bu, karmaşık özelliklerin değerlerini ayarlamanız gerektiğinde tercih edilebilir. Örnek:  

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

Yukarıdaki örnekte, karmaşık özelliklere, noktalı adlar kullanılarak erişilir. Karmaşık özelliklere erişmenin diğer yolları için, özellikle karmaşık özellikler hakkında bu konunun ilerleyen bölümlerindeki iki bölüme bakın.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Geçerli, orijinal veya veritabanı değerlerini içeren kopyalanmış nesne oluşturma  

CurrentValues, OriginalValues veya GetDatabaseValues 'tan döndürülen DbPropertyValues nesnesi varlığın bir kopyasını oluşturmak için kullanılabilir. Bu kopya, onu oluşturmak için kullanılan DbPropertyValues nesnesinden özellik değerlerini içerecektir. Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Döndürülen nesnenin varlık değil olduğunu ve bağlam tarafından izlenmediğini unutmayın. Döndürülen nesnenin aynı zamanda diğer nesnelere ayarlanmış hiçbir ilişkisi yok.  

Kopyalanmış nesne, özellikle de belirli bir türdeki nesnelere veri bağlamayı içeren bir kullanıcı arabiriminin kullanıldığı eşzamanlı güncelleştirmelerle ilgili sorunları çözmek için yararlı olabilir.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Karmaşık özelliklerin geçerli veya orijinal değerlerini alma ve ayarlama  

Tüm karmaşık bir nesnenin değeri, basit bir özellik için olduğu gibi, Property yöntemi kullanılarak okunabilir ve ayarlanabilir. Ayrıca, karmaşık nesnenin detayına gidebilir ve bu nesnenin özelliklerini okuyabilir veya ayarlayabilirsiniz, hatta iç içe geçmiş bir nesne. İşte bazı örnekler:  

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

Özgün bir değer almak veya ayarlamak için CurrentValue özelliği yerine OriginalValue özelliğini kullanın.  

Özelliğin veya ComplexProperty yönteminin karmaşık bir özelliğe erişmek için kullanılabileceğini unutmayın. Ancak, karmaşık nesneye ek özellik veya ComplexProperty çağrıları ile detaya gitmek istiyorsanız ComplexProperty yöntemi kullanılmalıdır.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Karmaşık özelliklere erişmek için DbPropertyValues kullanma  

Bir varlık için geçerli, orijinal veya veritabanı değerlerini almak üzere CurrentValues, OriginalValues veya GetDatabaseValues kullandığınızda, herhangi bir karmaşık özellik değeri iç içe geçmiş DbPropertyValues nesneleri olarak döndürülür. Bu iç içe geçmiş nesneler daha sonra karmaşık nesnenin değerlerini almak için kullanılabilir. Örneğin, aşağıdaki yöntem, herhangi bir karmaşık özellik ve iç içe geçmiş karmaşık özellikler dahil olmak üzere tüm özelliklerin değerlerini yazdıracaktır.  

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

Tüm geçerli özellik değerlerini yazdırmak için yöntem şöyle çağrılır:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
