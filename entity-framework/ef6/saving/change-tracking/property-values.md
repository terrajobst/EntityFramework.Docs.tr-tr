---
title: Özellik değerleri - EF6 ile çalışma
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: afde503bb4ed15fcf83a57053541cd5da8c89835
ms.sourcegitcommit: 50521b4a2f71139e6a7210a69ac73da582ef46cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67416671"
---
# <a name="working-with-property-values"></a>Özellik değerleri ile çalışma
Çoğunlukla Entity Framework izleme durumu, özgün değer ve varlık örneklerinizin özelliklerin geçerli değerleri ilgileniriz. Ancak, bazı durumlarda, görüntülemek veya değiştirmek EF olan özellikler hakkında bilgi için istediğiniz - gibi bağlantısız senaryolar - olabilir. Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

Entity Framework, izlenen bir varlığın her bir özellik için iki değeri izler. Geçerli değer olduğunda, adından da anlaşılacağı gibi varlıktaki özelliğinin geçerli değeri. Özgün değer varlık veritabanından sorgulanan veya bağlamına bağlı özelliği olan değerdir.  

Özellik değerleri ile çalışmak için iki genel mekanizma vardır:  

- Tek bir özellik değeri, özellik yöntemi kullanarak türü kesin belirlenmiş bir biçimde alınabilir.  
- Bir varlığın tüm özellikleri için değer DbPropertyValues nesnede okunabilir. DbPropertyValues ardından okuyup ayarlamak özellik değerlerini izin vermek için bir sözlük benzeri nesnesi olarak görev yapar. Değerlerin DbPropertyValues nesnesindeki başka bir DbPropertyValues nesne değerleri veya başka bir kopyasını varlık veya basit veri aktarım nesnesini (DTO) gibi diğer bazı nesne değerleri ayarlayabilirsiniz.  

Aşağıdaki bölümlerde yukarıdaki mekanizmalardan ikisi de örnekleri gösterir.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Alma ve tek bir özellik geçerli ya da orijinal değerini ayarlama  

Aşağıdaki örnek nasıl bir özelliğin geçerli değerini okunabilir ve ardından yeni bir değere ayarlayın gösterir:  

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

OriginalValue özelliği yerine CurrentValue özelliği okuma veya özgün değer ayarlamak için kullanın.  

Özellik adı belirtmek için kullanılan bir dize olduğunda döndürülen değer "nesne" yazıldığını unutmayın. Bir lambda ifadesi kullanılıyorsa, diğer taraftan, döndürülen değerin türü kesin.  

Özellik değeri gibi bu ayarı yalnızca özellik yeni değer eski değerinden farklı ise değiştirilmiş olarak işaretler.  

Bir özellik değeri, bu şekilde ayarlandığında bile AutoDetectChanges kapalı değişikliği otomatik olarak algılanır.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Alma ve eşlenmemiş bir özelliğin geçerli değerini ayarlama  

Veritabanına eşlenmemiş bir özelliğinin geçerli değeri ayrıca okuyabilirsiniz. Eşlenmemiş bir özelliği örneği, bir Blog RssLink özelliği olabilir. Bu değer, üzerinde BlogId göre hesaplanır ve bu nedenle veritabanında depolanan gerekmez. Örneğin:  

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

Özelliğin bir Ayarlayıcısı sunarsa geçerli değeri de ayarlayabilirsiniz.  

Entity Framework doğrulama eşlenmemiş özelliklerinin gerçekleştirirken eşlenmemiş özelliklerin değerlerini okuma yararlı olur. Aynı nedenden dolayı geçerli değerleri okuyun ve bağlam tarafından şu anda izlenmiyor varlıkların özelliklerini ayarlayın. Örneğin:  

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

Özgün değerlerine bağlam tarafından izlenmiyor varlıkların özelliklerini veya eşlenmemiş özellikler için kullanılabilir olmadığını unutmayın.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Bir özelliği değiştirildi olarak işaretlenmiş olup olmadığını denetleme  

Aşağıdaki örnek, tek bir özellik değiştirilmiş olarak işaretlenmiş olup olmadığını denetlemek gösterilmektedir:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Değiştirilen özelliklerin değerlerini SaveChanges çağrıldığında güncelleştirmeleri veritabanına gönderilir.  

##  <a name="marking-a-property-as-modified"></a>Bir özelliği değiştirildi olarak işaretleme  

Aşağıdaki örnekte, değiştirilmiş olarak işaretlemek için tek bir özellik zorlamak gösterilmektedir:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Bir özellik olması için bir güncelleştirme gönderme özelliği için veritabanı özelliğinin geçerli değeri, özgün değeri aynı olsa bile SaveChanges çağrıldığında değiştirilmiş kuvvetleri olarak işaretleniyor.  

Değiştirilmiş olarak işaretlenmiş sonra değiştirilmesi değil tek bir özellik sıfırlamak şu anda mümkün değildir. Sorun gelecekteki bir sürümde desteklemeyi planlıyoruz budur.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Geçerli ve özgün veritabanı değerlerini bir varlığın tüm özellikleri okuma  

Aşağıdaki örnek, geçerli değerler, özgün değerler ve değerlerin gerçekten bir varlığın eşlenen tüm özellikler için veritabanında okuma gösterilmektedir.  

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

Geçerli değerler şu anda entity öğesinin özellikleri içeren değerlerdir. Varlığı sorgulandığında veritabanından okunan değerleri özgün değerlerdir. Şu anda veritabanında depolanan gibi veritabanı değerlerini değerlerdir. Veritabanı değerlerini alma veritabanındaki değerlerin eşzamanlı düzenlemek için veritabanı başka bir kullanıcı tarafından yapılmadığını gibi varlık sorgulandı beri değişmiş olabilir durumlarda kullanışlıdır.  

## <a name="setting-current-or-original-values-from-another-object"></a>Geçerli ya da orijinal değerleri başka bir nesneden ayarlama  

İzlenen bir varlığın geçerli ya da orijinal değerleri başka bir nesneden değerleri kopyalayarak güncelleştirilebilir. Örneğin:  

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

Yukarıdaki kodu çalıştıran verecektir:  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Bu teknik bazen bir varlık hizmeti çağrısı veya istemci n katmanlı bir uygulama olarak alınan değerlerle güncelleştirirken kullanılır. Varlık adları eşleşen özelliklere sahip olduğu sürece, varlık olarak aynı türde olacak şekilde kullanılan nesne olmadığını unutmayın. Yukarıdaki örnekte, BlogDTO örneği, orijinal değerleri güncelleştirmek üzere kullanılır.  

Diğer nesneden kopyaladığınız değerleri farklı şekilde ayarlanan özellikler değiştirilmiş olarak işaretlenir unutmayın.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Geçerli ya da orijinal değerleri sözlükten ayarlama  

İzlenen bir varlığın geçerli ya da orijinal değerleri sözlüğü ya da başka bir veri yapısına değerlerini kopyalayarak güncelleştirilebilir. Örneğin:  

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

OriginalValues özelliği CurrentValues özelliği yerine özgün değerleri ayarlamak için kullanın.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Geçerli ya da orijinal değerleri özelliğini kullanarak bir sözlükten ayarlama  

CurrentValues veya yukarıda da gösterildiği gibi OriginalValues kullanmaya alternatif her bir özellik değerini ayarlamak için özellik yöntemini kullanmaktır. Karmaşık özelliklerin değerlerini ayarlamak, ihtiyacınız olduğunda bu tercih olabilir. Örneğin:  

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

Karmaşık özellikler yukarıdaki örnekte noktalı adları kullanılarak erişilebilir. Karmaşık özelliklerine erişmek kullanabileceğiniz diğer yöntemler, özellikle karmaşık özellikler hakkında bu konunun ilerleyen bölümlerinde iki bölümlere bakın.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Geçerli, özgün ya da veritabanı değerlerini içeren bir kopyalanan nesne oluşturma  

DbPropertyValues nesne OriginalValues, CurrentValues döndürülen veya GetDatabaseValues varlık bir kopyasını oluşturmak için kullanılabilir. Bu kopya oluşturmak için kullanılan DbPropertyValues nesnesinden özellik değerlerini içerir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Döndürülen nesne varlık değildir ve bağlam tarafından izlenmiyor unutmayın. Döndürülen nesne ilişkileri diğer nesnelere ayarlamak da yok.  

Kopyalanan nesne veritabanına özellikle veri bağlama için belirli bir türden nesneleri içeren bir kullanıcı Arabirimi kullanıldığı yerin eş zamanlı güncelleştirmelerin ilgili sorunları çözmek için yararlı olabilir.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Alma ve karmaşık özelliklerin geçerli ya da özgün değerlerini ayarlama  

Karmaşık bir nesnenin tamamını değerini okuma ve özellik yöntemi kullanarak basit bir özellik için olabilir set olabilir. Buna ek olarak, karmaşık bir nesne ve söz konusu nesne veya iç içe bir nesneyi okuma ya da ayarlanmış özelliği detaya gidebilirsiniz. Bazı örnekler şunlardır:  

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

OriginalValue özelliği yerine CurrentValue özelliği almak ve özgün değeri ayarlamak için kullanın.  

Karmaşık bir özelliğe erişmek için özelliği veya ComplexProperty yöntemi kullanılabileceğini unutmayın. Ancak, ek özellik ile karmaşık nesne incelemek istediğiniz veya ComplexProperty çağırır ComplexProperty yöntemi kullanılmalıdır.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Karmaşık özelliklerine erişmek için DbPropertyValues kullanma  

Ne zaman geçerli özgün almak için CurrentValues, OriginalValues veya GetDatabaseValues kullanın veya veritabanı değerlerini bir varlık için herhangi bir karmaşık özelliklerin değerlerini iç içe geçmiş DbPropertyValues nesneler olarak döndürülür. Bu nesneleri can iç içe ardından karmaşık bir nesnenin değerlerini almak için kullanılır. Örneğin, aşağıdaki yöntemi değerleri herhangi bir karmaşık özellikler ve iç içe geçmiş karmaşık özellikler dahil olmak üzere tüm özelliklerinin değerleri yazdırır.  

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

Tüm geçerli özellik değerleri print yöntemi için şu şekilde çağrılır:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
