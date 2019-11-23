---
title: Yeni bir veritabanına Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182565"
---
# <a name="code-first-to-a-new-database"></a>Yeni bir veritabanına Code First
Bu video ve adım adım yönergeler, yeni bir veritabanını hedefleyen Code First geliştirmeye yönelik bir giriş sağlar. Bu senaryo, mevcut olmayan bir veritabanının hedeflenmesini ve Code First oluşturulacağını ve Code First yeni tablolar ekleyecek boş bir veritabanı içerir. Code First, modelinizi C\# veya VB.Net sınıfları kullanarak tanımlamanızı sağlar. Ek yapılandırma, isteğe bağlı olarak sınıflarınızda ve özelliklerde öznitelikler kullanılarak veya bir Fluent API kullanılarak gerçekleştirilebilir.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda yeni bir veritabanını hedefleyen Code First geliştirmeye yönelik bir giriş sunulmaktadır. Bu senaryo, mevcut olmayan bir veritabanının hedeflenmesini ve Code First oluşturulacağını ve Code First yeni tablolar ekleyecek boş bir veritabanı içerir. Code First, veya VB.Net sınıfları kullanarak C# modelinizi tanımlamanızı sağlar. Ek yapılandırma, isteğe bağlı olarak sınıflarınızda ve özelliklerde öznitelikler kullanılarak veya bir Fluent API kullanılarak gerçekleştirilebilir.

**Sunulma ölçütü**: [Rowa Miller](https://romiller.com/)

**Video**: [wmv](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

## <a name="pre-requisites"></a>Önkoşulların önkoşulları

Bu izlenecek yolu tamamlamak için en az Visual Studio 2010 veya Visual Studio 2012 yüklü olmalıdır.

Visual Studio 2010 kullanıyorsanız, [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) ' in yüklü olması gerekir.

## <a name="1-create-the-application"></a>1. uygulamayı oluşturun

Şeyleri basit tutmak için veri erişimi gerçekleştirmek üzere Code First kullanan temel bir konsol uygulaması oluşturacağız.

-   Visual Studio 'Yu aç
-   **Dosya-&gt; yeni&gt; projesi...**
-   Sol taraftaki menüden ve **konsol uygulamasından** **Windows** ' u seçin
-   Ad olarak **Codefırstnewdatabasesample** girin
-   **Tamam 'ı** seçin

## <a name="2-create-the-model"></a>2. model oluşturma

Sınıfları kullanarak çok basit bir model tanımlayalim. Bunları Program.cs dosyasında, ancak gerçek bir dünya uygulamasında tanımladıktan sonra, sınıflarınızı ayrı dosyalara ve potansiyel olarak ayrı bir projeye böyorsunuz.

Program.cs içindeki program sınıfı tanımının altında aşağıdaki iki sınıfı ekleyin.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

İki gezinti özelliğini (blog. Post ve post. blog) sanal hale getiriyoruz. Bu, Entity Framework yavaş yükleme özelliğini sunar. Yavaş yükleme, bu özelliklerin içeriğinin, erişmeye çalıştığınızda veritabanından otomatik olarak yükleneceğini gösterir.

## <a name="3-create-a-context"></a>3. bağlam oluşturma

Şimdi, veritabanı ile bir oturumu temsil eden ve verileri sorgulayıp kaydedebileceğimizi sağlayan bir türetilmiş bağlam tanımlama zamanı. System. Data. Entity. DbContext öğesinden türeten bir bağlam tanımladık ve modelimizin her bir sınıfı için tür bir DbSet&lt;TEntity&gt; kullanıma sunuyor.

Artık Entity Framework türleri kullanmaya başladık, bu yüzden EntityFramework NuGet paketini eklememiz gerekiyor.

-   **Proje –&gt; NuGet Paketlerini Yönet...**
    Note: **NuGet Paketlerini Yönet...** [en son NuGet sürümünü](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklemelisiniz.
-   **Çevrimiçi** sekmesini seçin
-   **EntityFramework** paketini seçin
-   **Install** 'a tıklayın

Program.cs 'in en üstünde System. Data. Entity için bir using açıklaması ekleyin.

``` csharp
using System.Data.Entity;
```

Program.cs ' deki Post sınıfının altında aşağıdaki türetilmiş bağlamı ekleyin.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

İşte Program.cs 'in neleri içermesi gerektiğine ilişkin ayrıntılı bir liste aşağıda verilmiştir.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

Bu, veri depolamayı ve almayı başlatmak için gerekli olan tüm kodlarda bulunur. Arka planda oldukça bir bit olduğu açıktır, ancak kısa bir süre içinde göz atacağız, ancak önce bunu eylemde görlim.

## <a name="4-reading--writing-data"></a>4. verileri okuma & yazma

Ana yöntemi aşağıda gösterildiği gibi Program.cs ' de uygulayın. Bu kod, bağlamımız yeni bir örnek oluşturur ve yeni bir blog eklemek için onu kullanır. Daha sonra, bir LINQ sorgusu kullanarak, veritabanındaki tüm blogları başlık sırasına göre sıralanmış olarak alır.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Şimdi uygulamayı çalıştırabilir ve test edebilirsiniz.

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Verilerim nerede?

Kurala göre DbContext sizin için bir veritabanı oluşturdu.

-   Yerel bir SQL Express örneği varsa (Visual Studio 2010 ile varsayılan olarak yüklenir) Code First, bu örnekte veritabanını oluşturmuştur
-   SQL Express kullanılamazsa Code First, [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) 'yi dener ve kullanır (Visual Studio 2012 ile varsayılan olarak yüklenir)
-   Veritabanı, türetilmiş bağlamın tam adından sonra, **Codefırstnewdatabasesample. BloggingContext** ile adlandırılır

Bunlar yalnızca varsayılan kurallardır ve Code First kullanan veritabanını değiştirmek için çeşitli yollar vardır. **DbContext 'In model ve veritabanı bağlantısını nasıl bulduğu hakkında** daha fazla bilgi bulabilirsiniz.
Visual Studio 'da Sunucu Gezgini kullanarak bu veritabanına bağlanabilirsiniz

-   **&gt; Sunucu Gezgini görüntüle**
-   **Veri bağlantıları** ' na sağ tıklayın ve **bağlantı ekle...** seçeneğini belirleyin.
-   Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak Microsoft SQL Server seçmeniz gerekir

    ![Veri Kaynağı Seç](~/ef6/media/selectdatasource.png)

-   Hangi hangisinin yüklü olduğuna bağlı olarak, LocalDB veya SQL Express 'e bağlanın

Artık Code First oluşturulan şemayı inceleyebilirsiniz.

![Ilk şema](~/ef6/media/schemainitial.png)

DbContext, tanımladığımız DbSet özelliklerine bakarak modele hangi sınıfların dahil edileceğini çalıştı. Daha sonra tablo ve sütun adlarını belirleme, veri türlerini belirleme, birincil anahtarları bulma vb. için varsayılan Code First kuralları kümesini kullanır. Bu izlenecek yolda daha sonra bu kuralları nasıl geçersiz kılabileceğiniz hakkında bakacağız.

## <a name="5-dealing-with-model-changes"></a>5. model değişiklikleriyle ilgilenme

Artık modelinizde bazı değişiklikler yapma zamanı, bu değişiklikleri yaptığımız için de veritabanı şemasını güncelleştirmemiz gerekiyor. Bunu yapmak için Code First Migrations veya kısa geçişler adlı bir özellik kullanacağız.

Geçişler, veritabanı Şemamızı yükseltme (ve düşürme) hakkında sıralı bir adım kümesine sahip olmamızı sağlar. Geçiş olarak bilinen bu adımların her biri, uygulanacak değişiklikleri açıklayan bir kod içerir. 

İlk adım, BloggingContext için Code First Migrations etkinleştirmektir.

-   **Araçlar-&gt; kitaplığı Paket Yöneticisi-&gt; Paket Yöneticisi konsolu**
-   Paket Yöneticisi konsolunda **Etkinleştir-geçişleri** komutunu çalıştırın
-   Projenize iki öğe içeren yeni bir geçişler klasörü eklenmiştir:
    -   **Configuration.cs** – bu dosya geçişleri BloggingContext geçirmek için kullanılacak ayarları içerir. Bu izlenecek yol için herhangi bir şeyi değiştirmemiz gerekmez, ancak burada çekirdek verileri belirtebileceğiniz, diğer veritabanları için sağlayıcıları kaydedebileceğiniz, geçişlerin de oluşturulduğu ad alanını değiştiren yer alan yer verilmiştir.
    -   **&lt;zaman damgası&gt;\_InitialCreate.cs** – bu ilk geçişinizin olması, veritabanını, blogların ve gönderi tablolarının bulunduğu boş bir veritabanı olmasını sağlamak üzere veritabanına uygulanmış olan değişiklikleri temsil eder. Code First, artık bu tabloları bir geçişe dönüştürüldüğünü kabul ettiğimiz için bu tabloları otomatik olarak oluşturmamıza izin veririz. Bu geçişin zaten uygulanmış olduğu yerel veritabanımızda Code First de kaydedilir. Dosya adının zaman damgası, sıralama amacıyla kullanılır.

    Şimdi modelinizde bir değişiklik yapalim, blog sınıfına bir URL özelliği ekleyin:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Paket Yöneticisi konsolundaki **Add-Migration AddUrl** komutunu çalıştırın.
    Ekle-geçiş komutu, son geçişinizin sonrasında yapılan değişiklikleri denetler ve bulunan değişikliklerle yeni bir geçiş gerçekleştirir. Geçişlerde bir ad verebiliriz; Bu durumda, ' AddUrl ' geçişini arıyoruz.
    Yapı iskelesi kodu, dize verilerini tutan, dbo 'ya bir URL sütunu eklememiz gerektiğini bildiriyor. Bloglar tablosu. Gerekirse, yapı iskelesi kodunu düzenleyebiliyoruz ancak bu durumda gerekli değildir.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   Package Manager konsolunda **Update-Database** komutunu çalıştırın. Bu komut, bekleyen tüm geçişleri veritabanına uygular. Yalnızca yeni AddUrl geçişimizi uygulamak için, ınitialcreate geçişimiz zaten uygulandı.
    İpucu: veritabanına göre yürütülmekte olan SQL 'i görmek için Update-Database ' i çağırırken **– verbose** anahtarını kullanabilirsiniz.

Yeni URL sütunu artık veritabanındaki Bloglar tablosuna eklenir:

![URL Ile şema](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. veri ek açıklamaları

Şu ana kadar, en az bir deyişle, varsayılan kurallarını kullanarak modeli keşfeder, ancak sınıflarımızın kuralları takip etmemesi ve daha fazla yapılandırma gerçekleştirebilmemiz gerekir. Bunun için iki seçenek vardır; Bu bölümdeki veri ek açıklamalarına ve ardından sonraki bölümde Fluent API bakacağız.

-   Modelinize bir Kullanıcı sınıfı ekleyelim

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Ayrıca, türetilmiş bağlamımız için bir küme eklememiz gerekir

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Bir geçiş eklemeye çalıştık, "*EntityType ' kullanıcısının" tanımlı anahtarı olmadığını belirten bir hata alırız. Bu EntityType için anahtarı tanımlayın. "* EF, bu kullanıcı adını bilmenin bir yolu olmadığından Kullanıcı için birincil anahtar olmalıdır.
-   Şimdi veri ek açıklamalarını kullanacağız. bu nedenle, Program.cs üst kısmına bir using ifadesini eklememiz gerekir

```csharp
using System.ComponentModel.DataAnnotations;
```

-   Şimdi, birincil anahtar olduğunu belirlemek için username özelliğine not ekleyin

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Bu değişiklikleri veritabanına uygulamak için geçişi bir geçişe bağlamak üzere **Add-Migration AddUser** komutunu kullanın
-   Veritabanına yeni geçişi uygulamak için **Update-Database** komutunu çalıştırın

Yeni tablo artık veritabanına eklenir:

![Kullanıcılar Ile şema](~/ef6/media/schemawithusers.png)

EF tarafından desteklenen ek açıklamaların tam listesi:

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute özniteliğine](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [Ters çevir Sepropertyattribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. floent API 'SI

Önceki bölümde, kurala göre algılanıp algılanmadığını tamamlamak veya geçersiz kılmak için veri açıklamalarını kullanma hakkında baktık. Modeli yapılandırmanın diğer yolu Code First Fluent API kullanmaktır.

Çoğu model yapılandırması, basit veri açıklamaları kullanılarak yapılabilir. Fluent API, veri ek açıklamalarıyla ilgili bazı gelişmiş yapılandırmaya ek olarak veri ek açıklamalarının yapabileceği her şeyi içeren model yapılandırması belirtmenin daha gelişmiş bir yoludur. Veri ek açıklamaları ve Fluent API birlikte kullanılabilir.

Fluent API erişmek için DbContext içinde Onmodeloluþturma yöntemini geçersiz kılarsınız. \_adı göstermek için User. DisplayName 'in içinde depolandığı sütunu yeniden adlandırmanızı istiyoruz.

-   Aşağıdaki kodla BloggingContext üzerinde Onmodeloluþturma yöntemini geçersiz kılın

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   Bu değişiklikleri veritabanına uygulamak için geçişi bir geçişe bağlamak üzere **Add-Migration ChangeDisplayName** komutunu kullanın.
-   Yeni geçişi veritabanına uygulamak için **Update-Database** komutunu çalıştırın.

DisplayName sütunu artık\_adı görüntülenecek şekilde yeniden adlandırıldı:

![Görünen adı olan şema yeniden adlandırıldı](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Özet

Bu izlenecek yolda, yeni bir veritabanı kullanarak Code First geliştirmeye baktık. Sınıfları kullanarak bir model tanımladık ve bu modeli bir veritabanı oluşturmak ve verileri depolamak ve almak için kullandınız. Veritabanı oluşturulduktan sonra, modelimiz gibi şemayı değiştirmek için Code First Migrations kullandınız. Ayrıca, veri ek açıklamalarını ve akıcı API 'YI kullanarak bir modelin nasıl yapılandırılacağını da gördük.
