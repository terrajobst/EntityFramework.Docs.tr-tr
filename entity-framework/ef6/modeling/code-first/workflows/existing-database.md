---
title: Var olan bir veritabanına Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 61980bbd1f236f496a9d4fd92aa52264f1454615
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182626"
---
# <a name="code-first-to-an-existing-database"></a>Var olan bir veritabanına Code First
Bu video ve adım adım yönergeler, var olan bir veritabanını hedefleyen Code First geliştirmeye yönelik bir giriş sağlar. Code First, modelinizi C\# veya VB.Net sınıfları kullanarak tanımlamanızı sağlar. İsteğe bağlı olarak, sınıflarınızda ve özelliklerde öznitelikler kullanılarak veya bir Fluent API kullanarak ek yapılandırma gerçekleştirilebilir.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu video [artık Channel 9 ' da kullanılabilir](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Önkoşulların önkoşulları

Bu izlenecek yolu tamamlamak için **Visual Studio 2012** veya **Visual Studio 2013** yüklü olması gerekir.

Ayrıca, **Visual Studio için Entity Framework Tools** sürüm **6,1** (veya üzeri) yüklü olmalıdır. Entity Framework Tools 'ın en son sürümünü yükleme hakkında bilgi için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md) .

## <a name="1-create-an-existing-database"></a>1. var olan bir veritabanını oluşturun

Genellikle, var olan bir veritabanını hedeflerken zaten oluşturulur, ancak bu izlenecek yol için, erişmek üzere bir veritabanı oluşturulması gerekir.

Şimdi veritabanını oluşturalım.

-   Visual Studio 'Yu aç
-   **&gt; Sunucu Gezgini görüntüle**
-   Veri bağlantıları ' na sağ tıklayın **&gt; bağlantı ekle...**
-   **Sunucu Gezgini** bir veritabanına bağlı değilseniz, veri kaynağı olarak **Microsoft SQL Server** seçmeniz gerekir

    ![Veri Kaynağı Seç](~/ef6/media/selectdatasource.png)

-   LocalDB örneğinize bağlanın ve veritabanı adı olarak **Blog** girin

    ![LocalDB bağlantısı](~/ef6/media/localdbconnection.png)

-   **Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.

    ![Veritabanı oluştur Iletişim kutusu](~/ef6/media/createdatabasedialog.png)

-   Yeni veritabanı artık Sunucu Gezgini görüntülenir, sağ tıklayıp **Yeni sorgu** ' yı seçin.
-   Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.

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

## <a name="2-create-the-application"></a>2. uygulamayı oluşturun

Şeyleri basit tutmak için veri erişimi gerçekleştirmek üzere Code First kullanan temel bir konsol uygulaması oluşturacağız:

-   Visual Studio 'Yu aç
-   **Dosya-&gt; yeni&gt; projesi...**
-   Sol taraftaki menüden ve **konsol uygulamasından** **Windows** ' u seçin
-   Ad olarak **Codefırstexistingdatabasesample** girin
-   **Tamam 'ı** seçin

 

## <a name="3-reverse-engineer-model"></a>3. tersine mühendislik modeli

Veritabanına eşlemek için bazı ilk kod oluşturmamıza yardımcı olmak üzere Visual Studio için Entity Framework Tools kullanacağız. Bu araçlar, isterseniz, el ile de yazabileceğiniz bir kod oluşturuyor.

-   **Proje-&gt; yeni öğe Ekle...**
-   Sol menüden **verileri** seçin ve ardından **ADO.net varlık veri modeli**
-   Ad olarak **BloggingContext** girin ve **Tamam 'a** tıklayın
-   Bu, **varlık veri modeli Sihirbazı 'nı** başlatır
-   **Veritabanından Code First** seçin ve **İleri** ' ye tıklayın.

    ![Sihirbaz bir CFE](~/ef6/media/wizardonecfe.png)

-   İlk bölümde oluşturduğunuz veritabanına bağlantıyı seçin ve **İleri** ' ye tıklayın.

    ![Sihirbaz Iki CFE](~/ef6/media/wizardtwocfe.png)

-   **Tablolar** ' ın yanındaki onay kutusuna tıklayarak tüm tabloları Içeri aktarıp **son** ' a tıklayın.

    ![Sihirbaz üç CFE](~/ef6/media/wizardthreecfe.png)

Tersine mühendislik işlemi tamamlandıktan sonra projeye bir dizi öğe eklendiğinde, nelerin eklendiğine göz atalım.

### <a name="configuration-file"></a>Yapılandırma dosyası

Projeye bir App. config dosyası eklenmiştir, bu dosya mevcut veritabanına yönelik bağlantı dizesini içerir.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Yapılandırma dosyasında başka bazı ayarlar da fark edersiniz. Bunlar, Code First veritabanlarının nerede oluşturulacağını söyleyen varsayılan EF 'dir. Mevcut bir veritabanıyla eşleştirdiğimiz için bu ayar uygulamamızda yok sayılır.*

### <a name="derived-context"></a>Türetilmiş bağlam

Projeye bir **BloggingContext** sınıfı eklendi. Bağlam, veritabanı ile bir oturumu temsil eder ve verileri sorgulamanızı ve kaydetmemizi sağlar.
Bağlam, modelimizin her türü için bir **Dbset&lt;TEntity&gt;** sunar. Ayrıca, varsayılan oluşturucunun **Name =** söz dizimini kullanarak bir temel Oluşturucu çağırdığına de dikkat edin. Bu, bu bağlam için kullanılacak bağlantı dizesinin yapılandırma dosyasından yüklü olması gerektiğini Code First söyler.

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

*Yapılandırma dosyasında bir bağlantı dizesi kullanırken, her zaman **Name =** sözdizimini kullanmanız gerekir. Bu, bağlantı dizesinin mevcut olmamasını sağlar ve Entity Framework kurala göre yeni bir veritabanı oluşturmak yerine oluşturulacak.*

### <a name="model-classes"></a>Model sınıfları

Son olarak, projeye bir **Blog** ve **Post** sınıfı de eklenmiştir. Bunlar, modelimizi oluşturan etki alanı sınıflarıdır. Code First kurallarının mevcut veritabanının yapısıyla hizalanmayan yapılandırmayı belirtmek için sınıflara uygulanan veri ek açıklamalarını görürsünüz. Örneğin, veritabanında en fazla **200** uzunluğuna sahip olduklarından **blog.Name** ve **blog. URL** ' de **StringLength** ek açıklamasını görürsünüz (Code First varsayılan olarak, SQL Server) veritabanı sağlayıcısı tarafından desteklenen en yüksek uzunluğu **(max)** kullanmaktır.

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

## <a name="4-reading--writing-data"></a>4. verileri okuma & yazma

Artık bir modelimiz olduğuna göre, bazı verilere erişmek için bunu kullanmanın zamanı. **Ana** yöntemi aşağıda gösterildiği gibi **program.cs** ' de uygulayın. Bu kod, bağlamımız yeni bir örnek oluşturur ve yeni bir **Blog**eklemek için onu kullanır. Daha sonra, bir LINQ sorgusu kullanarak, veritabanındaki tüm **blogları** **başlık**sırasına göre sıralanmış olarak alır.

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
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Veritabanım değişirse ne olacak?

Veritabanına Code First Sihirbazı, daha sonra ince ayar ve değiştirebileceğiniz bir başlangıç noktası kümesi oluşturmak için tasarlanmıştır. Veritabanı şemanız değişirse, sınıfları el ile düzenleyebilir veya sınıfların üzerine yazmak için başka bir geri mühendis gerçekleştirebilirsiniz.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Mevcut bir veritabanına Code First Migrations kullanma

Mevcut bir veritabanıyla Code First Migrations kullanmak istiyorsanız, bkz. [Code First Migrations var olan bir veritabanına](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Özet

Bu kılavuzda, var olan bir veritabanını kullanarak Code First geliştirmeye baktık. Veritabanına eşlenen ve veri depolamak ve almak için kullanılabilecek bir sınıf kümesine ters mühendislik uygulamak için Visual Studio için Entity Framework Tools kullandık.
