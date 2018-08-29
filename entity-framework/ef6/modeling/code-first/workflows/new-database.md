---
title: Yeni bir veritabanı - EF6 öncelikle kod
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 50c6a4710bc50879304f64e781a46c4836f86882
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152484"
---
# <a name="code-first-to-a-new-database"></a>Yeni bir veritabanına ilk kod
Bu video ve adım adım kılavuz, yeni bir veritabanını hedefleyen Code First geliştirmeye giriş sağlar. Mevcut bir veritabanını hedefleyen bu senaryo içerir ve Code First oluşturur ya da boş bir veritabanı, Code First yeni tablolara ekleyeceksiniz. Kod ilk sağlar, C kullanarak modelinizi tanımlamanızı\# veya VB.Net sınıflar. Ek yapılandırma, sınıfları ve özellikleri ya da fluent API'sini kullanarak özniteliklerini kullanarak isteğe bağlı olarak gerçekleştirilebilir.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda yeni bir veritabanını hedefleyen Code First geliştirmeye giriş sağlar. Mevcut bir veritabanını hedefleyen bu senaryo içerir ve Code First oluşturur ya da boş bir veritabanı, Code First yeni tablolara ekleyeceksiniz. Kod ilk C# veya VB.Net sınıflarını kullanarak modelinizi tanımlamanızı sağlar. Ek yapılandırma, sınıfları ve özellikleri ya da fluent API'sini kullanarak özniteliklerini kullanarak isteğe bağlı olarak gerçekleştirilebilir.

**Tarafından sunulan**: [Rowan Miller](http://romiller.com/)

**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

# <a name="pre-requisites"></a>Ön koşullar

Bu izlenecek yolu tamamlamak için Visual Studio 2012 yüklü veya en az Visual Studio 2010 olması gerekir.

Visual Studio 2010 kullanıyorsanız, aynı zamanda sahip gerekecektir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklü.

## <a name="1-create-the-application"></a>1. Uygulama oluşturma

Örneği basit tutmak için bunu bir veri erişimi gerçekleştirdiği Code First kullanan temel bir konsol uygulaması oluşturmak için dağıtacağız.

-   Visual Studio'yu Aç
-   **Dosya -&gt; yeni -&gt; proje...**
-   Seçin **Windows** sol menüden ve **konsol uygulaması**
-   Girin **CodeFirstNewDatabaseSample** adı
-   Seçin **Tamam**

## <a name="2-create-the-model"></a>2. Model oluşturma

Sınıfları kullanarak basit bir model tanımlayalım. Biz yalnızca bunları Program.cs dosyasındaki ancak ayrı dosyalar ve büyük olasılıkla ayrı proje sınıflarınızı kullanıma ayırırsınız gerçek dünya uygulamasında tanımlayacağınız.

Program.cs Program sınıf tanımında altına aşağıdaki iki sınıf ekleyin.

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

İki Gezinti özellikleri (Blog.Posts ve Post.Blog) sanal yapıyoruz olduğunu fark edeceksiniz. Bu, Entity Framework'ün yavaş Yükleme özelliği sağlar. Yavaş yükleniyor, bunları erişmeye çalışırken bu özellikleri içeriğini otomatik olarak veritabanından yükleneceğini belirten anlamına gelir.

## <a name="3-create-a-context"></a>3. Bir bağlam oluşturma

Artık sorgu ve veri kaydetmek bize izin vererek, veritabanı ile bir oturumu temsil eden bir türetilmiş içeriği tanımlamak için zamanı geldi. System.Data.Entity.DbContext türetilir ve bir türü belirtilmiş olan DB sunan bir bağlam tanımlarız&lt;TEntity&gt; modelimizi her sınıf için.

Şu anda yüzden EntityFramework NuGet paketini eklemek Entity Framework türlerinden kullanılacak başlatılıyor.

-   **Proje –&gt; NuGet paketlerini Yönet...**
    Not: yoksa **NuGet paketlerini Yönet...** seçeneğini yüklemelisiniz [en son NuGet sürümünü](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Seçin **çevrimiçi** sekmesi
-   Seçin **EntityFramework** paket
-   Tıklayın **yükleyin**

Kullanarak bir ekleme deyimi Program.cs dosyasının üst System.Data.Entity için.

``` csharp
using System.Data.Entity;
```

Program.cs sınıfında postanın aşağıdaki türetilmiş bağlam ekleyin.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Ne Program.cs artık içermelidir, tam bir listesi aşağıda verilmiştir.

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

Biz depolamak ve almak veri başlatmak için ihtiyacınız olan tüm kod olmasıdır. Kuşkusuz arka planda geçmeden oldukça bit yoktur ve ilk ancak bir dakika içinde şimdi nasıl çalıştığını görün, biz göz atacağız.

## <a name="4-reading--writing-data"></a>4. Okuma ve yazma veri

Main yöntemi program.cs'ye aşağıda gösterildiği gibi uygulayın. Bu kod, bizim bağlam yeni bir örneğini oluşturur ve yeni bir Blog eklemek için kullanır. Ardından veritabanından başlığa göre alfabetik olarak sıralanmış tüm blogları almak için LINQ sorgusu kullanır.

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

Şimdi uygulamayı çalıştırmak ve test etmek.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a>Verilerim nerede?

Kural gereği DbContext sizin için bir veritabanı oluşturdu.

-   (Visual Studio 2010 ile varsayılan olarak yüklenir) yerel bir SQL Express örneği varsa ardından Code First veritabanı bu örnekte oluşturdu
-   SQL Express kullanılamıyor durumunda Code First çalıştığında ve [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (Visual Studio 2012 ile varsayılan olarak yüklenir)
-   Sonra bu örnekte türetilen bağlamı tam olarak nitelenmiş adını adlı veritabanı **CodeFirstNewDatabaseSample.BloggingContext**

Bunlar yalnızca varsayılan kuralları ve Code First kullandığı veritabanını değiştirmek için çeşitli yolları vardır, daha fazla bilgi kullanılabilir **DbContext Model ve veritabanı bağlantısını nasıl bulur** konu.
Visual Studio'da Sunucu Gezgini kullanarak bu veritabanına bağlanabilir

-   **Görünüm -&gt; Sunucu Gezgini**
-   Sağ tıklayın **veri bağlantıları** seçip **bağlantı ekle...**
-   Sunucu gezgininden veritabanına bağlamadıysanız önce Microsoft SQL Server veri kaynağı olarak seçmeniz gerekir

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   LocalDB veya hangisinin bağlı olarak yüklediğiniz SQL Express için Bağlan

Biz, artık Code First oluşturulan şema inceleyebilirsiniz.

![SchemaInitial](~/ef6/media/schemainitial.png)

Biz tanımlanmış olan DB özelliklerine bakarak tarafından modele dahil edilecek sınıfları dışarı DbContext çalışmıştır. Ardından kod öncelikli kurallar varsayılan kümesini kullanır tablo ve sütun adları belirlemek, veri türlerini belirlemek için birincil anahtarlar vb. bulun. Bu kuralları nasıl kılabilirsiniz Bu izlenecek yolda inceleyeceğiz.

## <a name="5-dealing-with-model-changes"></a>5. Model değişikliklerle ilgilenme

Artık size ayrıca veritabanı şemasını güncelleştirmek için ihtiyacımız olan bu değişiklikler yaptığınızda modelimiz için bazı değişiklikler yapmanız gerekebileceğini zamanı geldi. Bunu yapmak için Code First Migrations veya geçirilmesi için kısa adlı bir özelliği kullanmak için kullanacağız.

Geçişleri bize (yükseltme ve düşürme), veritabanı şemasını açıklayan adımlar sıralı bir dizi olmasını sağlar. Bir geçiş bilinen, bu adımların her biri, değişikliklerin uygulanması için açıklayan bazı kod içerir. 

İlk adım, bizim BloggingContext için Code First Migrations etkinleştirmektir.

-   **Araçlar -&gt; kitaplık Paket Yöneticisi -&gt; Paket Yöneticisi Konsolu**
-   Çalıştırma **etkinleştir geçişleri** komutunu Paket Yöneticisi Konsolu
-   Yeni bir geçiş klasör iki öğe içeren bizim projeye eklendi:
    -   **Configuration.cs** – bu dosya geçişleri geçirmek için kullanacağı ayarları içeren BloggingContext. Bu izlenecek yol için herhangi bir ayarı değiştirmek gerekmez, ancak burada kayıt sağlayıcıları diğer veritabanları için çekirdek veri değişiklikleri ad alanı belirleyebileceğiniz geçişler vb. içinde oluşturulur.
    -   **&lt;zaman damgası&gt;\_InitialCreate.cs** – Bu, ilk geçiş, blog ve gönderi tablolar içeren bir boş bir veritabanı yüklenmesini gerçekleştirilecek veritabanına zaten uygulanmış olan değişiklikleri gösterir . Biz Code First otomatik olarak izin vermesine rağmen bu tablolar bir geçiş dönüştürülmüş geçişler için biz de verdiniz göre bize oluşturun. Kod, ayrıca bu geçiş zaten uygulanmış bizim yerel veritabanında ilk kaydetti. Zaman damgası dosya çubuğunda amacıyla sıralamak için kullanılır.

    Şimdi github'dan modelimiz için değişiklik, Blog sınıfa URL'si özelliği ekleyin:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Çalıştırma **Ekle geçiş AddUrl** Paket Yöneticisi konsolunda komutu.
    Geçiş Ekle komutunu son geçişinizi bu yana değişiklik olup olmadığını denetler ve bulunan herhangi bir değişiklik ile yeni bir geçiş iskele oluşturulduğunu. Biz geçişleri bir ad verebilirsiniz; Bu durumda biz 'AddUrl' geçiş arıyoruz.
    İskele kurulan kodu dbo dize verileri tutabilecek, bir Url sütun eklemek ihtiyacımız diyor. Bloglar tablo. Gerekirse, iskele kurulan kodu düzenleme, ancak bu durumda zorunlu değildir.

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

-   Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu. Bu komut tüm bekleyen geçişleri veritabanına uygulanır. Geçişler yalnızca bizim yeni AddUrl geçiş uygulanacak şekilde bizim InitialCreate geçiş zaten uygulanmıştır.
    İpucu: Kullandığınız **– ayrıntılı** veritabanına karşı yürütülen SQL görmek için Update-veritabanı çağırırken geçin.

Yeni bir Url sütun artık veritabanı blogları tablosuna eklenir:

![SchemaWithUrl](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. Veri ek açıklamaları

Şu ana kadar biz yalnızca kendi varsayılan kuralları kullanarak model Bul EF bildirdiniz, ancak var olan giderek kez zaman göreceğiniz sınıfların kurallarına uygun olmayan ve daha fazla yapılandırmayı gerçekleştirmek ihtiyacımız olacak. Bu iki seçenek vardır; Bu bölümde veri ek açıklamaları ve sonraki bölümde fluent API'si şu konuları inceleyeceğiz.

-   Kullanıcı sınıfı için modelimizi ekleyelim

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Biz de müşterilerimizin türetilmiş bağlamına kümesi eklemeniz gerekir

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Bir geçiş eklemek çalıştığında bir hata bildiren elde edebileceğiniz "*EntityType 'User' olan anahtar tanımlanmadı. Anahtar için bu Entitytype'a tanımlayın."* Kullanıcı adı, kullanıcı için birincil anahtarı olmalıdır olduğunu bilmesinin imkanı EF sahip olduğundan.
-   Yüzden kullanarak bir eklemek veri ek açıklamaları kullanmak oluşturacağız deyimini Program.cs dosyasının üst

```
using System.ComponentModel.DataAnnotations;
```

-   Artık birincil anahtarı olduğunu belirlemek için kullanıcı adı özelliği Not Ekle

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Kullanım **Ekle geçiş AddUser** bu uygulama için bir geçiş iskele komut, veritabanına değiştirir
-   Çalıştırma **veritabanını Güncelleştir** komut yeni geçiş veritabanına uygulamak için

Yeni tablo artık veritabanına eklenir:

![SchemaWithUsers](~/ef6/media/schemawithusers.png)

EF tarafından desteklenen ek açıklamaları tam listesi verilmiştir:

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. Fluent API'si

Önceki bölümde tamamlayabilir veya kurala göre algılandı geçersiz kılmak için veri ek açıklamaları kullanarak incelemiştik. Model yapılandırmak için diğer Code First fluent API'si ile yoludur.

En fazla model yapılandırma yapılabilir basit veri ek açıklamalarını kullanma. Fluent API'si veri ek açıklamaları ayrıca veri açıklamalarla mümkün bazı daha gelişmiş yapılandırma için yapabileceğiniz her şeyi kapsayan bir model yapılandırması belirterek daha gelişmiş bir yoludur. Veri ek açıklamaları ve fluent API'si birlikte kullanılabilir.

Fluent API'sine erişmek için DbContext OnModelCreating yöntemi geçersiz kılın. İstedik User.DisplayName görüntülenecek depolanan sütunu yeniden adlandırmak düşünelim\_adı.

-   Aşağıdaki kod ile BloggingContext üzerinde OnModelCreating yöntemi geçersiz kılın

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

-   Kullanım **Ekle geçiş ChangeDisplayName** bu uygulama için bir geçiş iskele komut, veritabanına değiştirir.
-   Çalıştırma **veritabanını Güncelleştir** veritabanına yeni geçiş uygulamak için komutu.

DisplayName sütun görüntülemek için şimdi adlandırılır\_adı:

![SchemaWithDisplayNameRenamed](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Özet

Bu izlenecek yolda Code First geliştirme kullanarak yeni bir veritabanı incelemiştik. Biz sınıfları kullanarak bir model tanımlı ardından modelin bir veritabanı oluşturun ve veri depolayıp almak için kullanılır. Veritabanı oluşturulduktan Code First Migrations şema gelişerek modelimizi değiştirmek için kullandık. Ayrıca veri ek açıklamaları ve Fluent API'sini kullanarak bir model yapılandırma gördük.
