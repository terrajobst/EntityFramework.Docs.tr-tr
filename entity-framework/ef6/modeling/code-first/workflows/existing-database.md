---
title: Mevcut bir veritabanına - EF6 öncelikle kod
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
caps.latest.revision: 3
ms.openlocfilehash: 47581a7ae9ff534d26ce82365bcbe2b254247495
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912726"
---
# <a name="code-first-to-an-existing-database"></a>Mevcut bir veritabanına ilk kod
Bu video ve adım adım izlenecek yol, var olan bir veritabanını hedefleyen Code First geliştirmeye giriş sağlar. Kod ilk sağlar, C kullanarak modelinizi tanımlamanızı\# veya VB.Net sınıflar. İsteğe bağlı olarak ek yapılandırma, sınıfları ve özellikleri ya da fluent API'sini kullanarak öznitelikleri kullanılarak gerçekleştirilebilir.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu video [Channel 9 sunuldu](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Ön koşullar

Sahip olması gerekir **Visual Studio 2012** veya **Visual Studio 2013** Bu izlenecek yolu tamamlamak için yüklü.

Ayrıca sürüm gerekir **6.1** (veya üzeri), **Visual Studio için Entity Framework Tools** yüklü. Bkz: [alma Entity Framework](~/ef6/fundamentals/install.md) Entity Framework Araçları'nın en son sürümü yükleme hakkında bilgi.

## <a name="1-create-an-existing-database"></a>1. Mevcut bir veritabanı oluşturun

Genellikle zaten oluşturulur varolan bir veritabanını hedeflerken, ancak bu kılavuz erişmek için bir veritabanı oluşturmak ihtiyacımız var.

Yeni bir ubuntu ve veritabanı oluşturun.

-   Visual Studio'yu Aç
-   **Görünüm -&gt; Sunucu Gezgini**
-   Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**
-   Bir veritabanına bağlamadıysanız **Sunucu Gezgini** seçmeniz gerekir önce **Microsoft SQL Server** veri kaynağı

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   LocalDB Örneğinize bağlanmak ve girin **blog** veritabanı adı

    ![LocalDBConnection](~/ef6/media/localdbconnection.png)

-   Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   Yeni veritabanı sunucu Gezgini'nde artık görünecek üzerinde sağ tıklayıp **yeni sorgu**
-   Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a>2. Uygulama oluşturma

Örneği basit tutmak için yapı veri erişimi gerçekleştirdiği Code First kullanan temel bir konsol uygulaması oluşturacağız:

-   Visual Studio'yu Aç
-   **Dosya -&gt; yeni -&gt; proje...**
-   Seçin **Windows** sol menüden ve **konsol uygulaması**
-   Girin **CodeFirstExistingDatabaseSample** adı
-   Seçin **Tamam**

 

## <a name="3-reverse-engineer-model"></a>3. Ters mühendislik modeli

Visual Studio için Entity Framework Araçları, veritabanı için ilk kod oluşturmamızı sağlayacak yararlanması oluşturacağız. Bu araçlar, yalnızca tercih ederseniz, ayrıca, el ile yazabilirsiniz kod oluşturma.

-   **Takım projesi -&gt; Yeni Öğe Ekle...**
-   Seçin **veri** sol menüden ardından **ADO.NET varlık veri modeli**
-   Girin **BloggingContext** tıklayın ve adı olarak **Tamam**
-   Böylece **varlık veri modeli Sihirbazı**
-   Seçin **Code First veritabanından** tıklatıp **İleri**

    ![WizardOneCFE](~/ef6/media/wizardonecfe.png)

-   İlk bölümde oluşturduğunuz veritabanı bağlantısı seçin ve tıklayın **İleri**

    ![WizardTwoCFE](~/ef6/media/wizardtwocfe.png)

-   Yanındaki onay kutusuna tıklayın **tabloları** tüm tabloları içeri aktarma ve tıklayın **son**

    ![WizardThreeCFE](~/ef6/media/wizardthreecfe.png)

Öğe sayısı ters mühendislik işlemi tamamlandıktan sonra projeye şimdi eklenecek ne eklenmiş bir göz atalım.

### <a name="configuration-file"></a>Yapılandırma dosyası

Bir App.config dosyası eklendi projeye, bu dosya, var olan veritabanına bağlantı dizesi içerir.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Diğer ayarlarında bir yapılandırma dosyası çok fark edeceksiniz, Code First veritabanları oluşturma yeri bildirme varsayılan EF ayarları şunlardır. Bu ayar yoksayılacak varolan bir veritabanına uygulamamız içinde eşleyeceğiniz olduğundan.*

### <a name="derived-context"></a>Türetilen içerik

A **BloggingContext** sınıfı projeye eklendi. Bağlam, sorgu ve veri kaydetmek bize izin vererek, veritabanı ile bir oturumu temsil eder.
İçerik sunan bir **olan DB&lt;TEntity&gt;**  modelimiz, her tür için. Varsayılan Oluşturucu kullanarak temel oluşturucu çağrısı da fark edeceksiniz **adı =** söz dizimi. Bu kod ilk bu bağlam için kullanılacak bağlantı dizesini yapılandırma dosyasından yüklenmesi gerektiğini bildirir.

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

*Her zaman kullanmalısınız **adı =** yapılandırma dosyasında bir bağlantı dizesi kullanırken söz dizimi. Bu bağlantı dizesi, mevcut değilse, Entity Framework ardından oluşturmaz sağlar kurala göre yeni bir veritabanı oluşturmak yerine.*

### <a name="model-classes"></a>Model sınıfları

Son olarak, bir **Blog** ve **Post** sınıfı da projeye eklendi. Modelimiz yapmak alan sınıfları şunlardır. Burada kod öncelikli kurallar var olan veritabanının yapısını hizalama değil, yapılandırmayı belirtmek için veri sınıfları için uygulanan ek görürsünüz. Örneğin, gördüğünüz **StringLength** üzerindeki ek açıklama **Blog.Name** ve **Blog.Url** bunlar en fazla olduğundan **200** içinde Veritabanı (veritabanı sağlayıcısı tarafından - desteklenen izin verilen en fazla uzunluk kullanmak için Code First varsayılandır **nvarchar(max)** SQL Server).

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a>4. Okuma ve yazma veri

Biz bir modeliniz olduğuna göre bazı verilere erişmek için kullanılacak zaman var. Uygulama **ana** yönteminde **Program.cs** aşağıda gösterildiği gibi. Bu kod, bizim bağlam yeni bir örneğini oluşturur ve yeni bir eklemek için kullandığı **Blog**. Tüm almak için bir LINQ Sorgu kullanıyorsa **blogları** göre alfabetik olarak sıralanmış veritabanından **başlık**.

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
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Veritabanım varsa ne değiştirir?

Code First Veritabanı Sihirbazı'nı, ardından ince ve değiştirebileceği sınıf bir başlangıç noktası kümesi oluşturmak üzere tasarlanmıştır. Veritabanı şemanızı değişirse el ile düzenleme sınıfları veya sınıflar üzerine yazmak için başka bir ters mühendislik gerçekleştirin.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Varolan bir veritabanına Code First geçişleri kullanma

Var olan bir veritabanına Code First Migrations'ı kullanmak istiyorsanız, bkz. [var olan bir veritabanına Code First Migrations](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Özet

Bu izlenecek yolda Code First geliştirme varolan bir veritabanını kullanma incelemiştik. Ters mühendislik veritabanına eşlediğiniz ve veri depolayıp almak için kullanılabilir sınıf kümesi için Entity Framework Araçları Visual Studio için kullanılır.
