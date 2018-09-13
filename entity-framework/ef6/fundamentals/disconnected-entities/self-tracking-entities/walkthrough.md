---
title: Varlıkları izlenecek yol - EF6 Self izleme
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: d89c452410d34bea71e8220aae141c3bfca3e1ce
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490282"
---
# <a name="self-tracking-entities-walkthrough"></a>Kendi kendine izleme varlıkları gözden geçirme
> [!IMPORTANT]
> Artık, self tracking varlıkları şablon kullanmanızı öneririz. Bu yalnızca var olan uygulamaları desteklemek kullanılabilir olmaya devam edecek. Uygulamanızın çalışma bağlantısı kesilmiş varlıklar grafikleriyle gerektiriyorsa, başka alternatifler gibi düşünün [izlenebilir varlıkları](http://trackableentities.github.io/), Self-Tracking-daha etkin bir şekilde tarafından geliştirilen varlıklara benzer bir teknoloji olan Topluluk veya alt düzey değişiklik API'leri izleme kullanarak özel kod yazma.

Bu izlenecek yol, bir Windows Communication Foundation (WCF) hizmeti bir varlık grafikte döndüren bir işlem kullanıma sunan bir senaryo gösterir. Ardından, bir istemci uygulaması, bu grafik yönetir ve doğrular ve Entity Framework kullanarak bir veritabanına güncelleştirmeleri kaydeden bir hizmet işlemi için yapılan değişiklikleri gönderir.

Bu izlenecek yolda tamamlamadan önce okuduğunuzdan emin olun [Self-Tracking varlıkları](index.md) sayfası.

Bu kılavuz, aşağıdaki eylemleri gerçekleştirir:

-   Erişmek için bir veritabanı oluşturur.
-   Modeli içeren bir sınıf kitaplığı oluşturur.
-   Takasları Self-Tracking varlık Oluşturucu şablon.
-   Varlık sınıfları için ayrı bir proje taşır.
-   Sorgulamak ve varlıkları kaydetmek için operations sunan bir WCF Hizmeti oluşturur.
-   İstemci hizmeti kullanmak uygulamaları (konsolu ve WPF) oluşturur.

Veritabanı ilk Bu izlenecek yolda kullanacağız ancak aynı teknikleri eşit ilk modeli için geçerlidir.

## <a name="pre-requisites"></a>Ön koşullar

Bu izlenecek yolu tamamlamak için Visual Studio'nun yeni bir sürümü gerekir.

## <a name="create-a-database"></a>Bir veritabanı oluşturun

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:

-   Visual Studio 2012 kullanıyorsanız, bir LocalDB veritabanına oluşturmayı.
-   Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.

Yeni bir ubuntu ve veritabanı oluşturun.

-   Visual Studio'yu Aç
-   **Görünüm -&gt; Sunucu Gezgini**
-   Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**
-   Sunucu gezgininden veritabanına bağlamadıysanız seçmeniz gerekir önce **Microsoft SQL Server** veri kaynağı
-   LocalDB veya hangisinin bağlı olarak yüklediğiniz SQL Express için Bağlan
-   Girin **STESample** veritabanı adı
-   Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**
-   Yeni veritabanı şimdi sunucu Gezgini'nde görünür.
-   Visual Studio 2012 kullanıyorsanız
    -   Sunucu Gezgini veritabanı üzerinde sağ tıklayıp **yeni sorgu**
    -   Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**
-   Visual Studio 2010 kullanıyorsanız
    -   Seçin **veri -&gt; Transact - SQL Düzenleyicisi&gt; yeni bağlantı...**
    -   Girin **.\\ SQLEXPRESS** tıklayın ve sunucu adı olarak **Tamam**
    -   Seçin **STESample** veritabanı açılır menüden aşağı sorgu Düzenleyicisi'ni üstünde
    -   Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **SQL Yürüt**

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

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a>Model oluşturma

Öncelikle, bir proje modeli yerleştirmek için ihtiyacımız var.

-   **Dosya -&gt; yeni -&gt; proje...**
-   Seçin **Visual C\#**  sol bölmeden ardından **sınıf kitaplığı**
-   Girin **STESample** tıklayın ve adı olarak **Tamam**

Basit bir model bizim veritabanına erişmek için EF Tasarımcısı'nda şimdi oluşturacağız:

-   **Takım projesi -&gt; Yeni Öğe Ekle...**
-   Seçin **veri** sol bölmeden ardından **ADO.NET varlık veri modeli**
-   Girin **BloggingModel** tıklayın ve adı olarak **Tamam**
-   Seçin **veritabanından Oluştur** tıklatıp **İleri**
-   Önceki bölümde oluşturduğunuz veritabanı için bağlantı bilgilerini girin
-   Girin **BloggingContext** tıklayın ve bağlantı dizesi adı olarak **İleri**
-   Yanındaki kutuyu işaretleyin **tabloları** tıklatıp **son**

## <a name="swap-to-ste-code-generation"></a>Ste'leri birleştirme kod oluşturma için takas etme

Artık varsayılan kod oluşturma ve değiştirme Self-Tracking varlıklara devre dışı bırakmak ihtiyacımız var.

### <a name="if-you-are-using-visual-studio-2012"></a>Visual Studio 2012 kullanıyorsanız

-   Genişletin **BloggingModel.edmx** içinde **Çözüm Gezgini** ve silme **BloggingModel.tt** ve **BloggingModel.Context.tt** 
     *Bu durum, varsayılan kod oluşturma devre dışı bırakır*
-   EF Designer seçin ve yüzey üzerinde boş bir alana sağ **kod oluşturma öğesi Ekle...**
-   Seçin **çevrimiçi** arayın ve sol bölmedeki **Pıştırarak Oluşturucusu**
-   Seçin **Pıştırarak Oluşturucu c\#**  şablon girin **STETemplate** tıklayın ve adı olarak **Ekle**
-   **STETemplate.tt** ve **STETemplate.Context.tt** dosyaları BloggingModel.edmx dosya altında iç içe geçmiş eklendi

### <a name="if-you-are-using-visual-studio-2010"></a>Visual Studio 2010 kullanıyorsanız

-   EF Designer seçin ve yüzey üzerinde boş bir alana sağ **kod oluşturma öğesi Ekle...**
-   Seçin **kod** sol bölmeden ardından **ADO.NET Self-Tracking varlık Oluşturucu**
-   Girin **STETemplate** tıklayın ve adı olarak **Ekle**
-   **STETemplate.tt** ve **STETemplate.Context.tt** dosyalarını doğrudan projenize eklendi

## <a name="move-entity-types-into-separate-project"></a>Varlık türleri ayrı projeye Taşı

Self-Tracking varlıkları kullanmak için istemci uygulamamız bizim modelden üretilen varlık sınıflarının erişimi gerekir. İstemci uygulamasına modelin tamamını göstermek istemediğiniz için varlık sınıflarını ayrı bir projeye taşımak için ekleyeceğiz.

İlk adım, varolan projede, varlık sınıfları oluşturma önlemektir:

-   Sağ **STETemplate.tt** içinde **Çözüm Gezgini** seçip **özellikleri**
-   İçinde **özellikleri** penceresi Temizle **TextTemplatingFileGenerator** gelen **CustomTool** özelliği
-   Genişletin **STETemplate.tt** içinde **Çözüm Gezgini** ve bunun altında iç içe geçmiş tüm dosyaları silin

Ardından, yeni bir proje ekleyin ve varlık sınıfları oluşturmak için kullanacağız

-   **Dosya -&gt; Ekle -&gt; proje...**
-   Seçin **Visual C\#**  sol bölmeden ardından **sınıf kitaplığı**
-   Girin **STESample.Entities** tıklayın ve adı olarak **Tamam**
-   **Takım projesi -&gt; varolan öğeyi Ekle...**
-   Gidin **STESample** proje klasörü
-   Görüntülemek için seçin **tüm dosyalar (\*.\*)**
-   Seçin **STETemplate.tt** dosyası
-   Yanındaki aşağı açılan oka tıklayın **Ekle** düğmesini tıklatın ve seçin **bağlantı olarak Ekle**

    ![Bağlantılı şablonu Ekle](~/ef6/media/addlinkedtemplate.png)

Bunu varlık sınıflarının ad bağlamı olarak oluşturulmasını emin olmak için dağıtacağız. Bu, yalnızca using deyimlerini eklemek için uygulama genelinde ihtiyacımız sayısını azaltır.

-   Bağlantılı üzerinde sağ **STETemplate.tt** içinde **Çözüm Gezgini** seçip **özellikleri**
-   İçinde **özellikleri** penceresi kümesi **özel aracı Namespace** için **STESample**

Ste'leri birleştirme şablon tarafından oluşturulan kodu başvuru gerekir **System.Runtime.Serialization** derlemek için. Bu kitaplık için WCF gerekli **DataContract** ve **DataMember** seri hale getirilebilir varlık türleri üzerinde kullanılan öznitelikler.

-   Sağ tıklayın **STESample.Entities** projesi **Çözüm Gezgini** seçip **Başvuru Ekle...**
    -   Visual Studio 2012'de - yanındaki kutuyu işaretleyin **System.Runtime.Serialization** tıklatıp **Tamam**
    -   Visual Studio 2010'da - seçin **System.Runtime.Serialization** tıklatıp **Tamam**

Son olarak, bizim bağlamda projeyle varlık türleri başvuru gerekir.

-   Sağ tıklayın **STESample** projesi **Çözüm Gezgini** seçip **Başvuru Ekle...**
    -   Visual Studio 2012'de - seçin **çözüm** yanındaki onay kutusunu sol bölmeden **STESample.Entities** tıklatıp **Tamam**
    -   Visual Studio 2010'da - seçin **projeleri** sekmesinde **STESample.Entities** tıklatıp **Tamam**

>[!NOTE]
> Varlık türleri ayrı bir projeye taşımak için başka bir seçenek, varsayılan konumundan bağlama yerine şablon dosyası taşımaktır. Bunu yaparsanız, güncelleştirmeniz gerekecektir **InputFile** edmx dosyasının göreli yolunu sağlamak için şablonu değişken (Bu örnekte olacaktır **.. \\BloggingModel.edmx**).

## <a name="create-a-wcf-service"></a>Bir WCF hizmeti oluşturma

Verilerimizi kullanıma sunmak için bir WCF hizmeti ekleme zamanı artık projesi oluşturarak başlayacağız.

-   **Dosya -&gt; Ekle -&gt; proje...**
-   Seçin **Visual C\#**  sol bölmeden ardından **WCF hizmeti uygulaması**
-   Girin **STESample.Service** tıklayın ve adı olarak **Tamam**
-   Bir başvuru ekleyin **System.Data.Entity** derleme
-   Bir başvuru ekleyin **STESample** ve **STESample.Entities** projeleri

Çalışma zamanında bulunur, böylece bu projede EF bağlantı dizesini kopyalayın oluşturmamız gerekir.

-   Açık **App.Config** dosya için ** STESample ** proje ve kopyalama **connectionStrings** öğesi
-   Yapıştırma **connectionStrings** öğesi alt öğesi olarak **yapılandırma** öğesinin **Web.Config** dosyası **STESample.Service** proje

Artık gerçek hizmeti uygulama zamanı geldi.

-   Açık **Iservice1.cs** ve içeriğini aşağıdaki kodla değiştirin.

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   Açık **service1.svc'yi** ve içeriğini aşağıdaki kodla değiştirin.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a>Konsol uygulamasından hizmetini kullanma

Hizmetimiz kullanan bir konsol uygulaması oluşturalım.

-   **Dosya -&gt; yeni -&gt; proje...**
-   Seçin **Visual C\#**  sol bölmeden ardından **konsol uygulaması**
-   Girin **STESample.ConsoleTest** tıklayın ve adı olarak **Tamam**
-   Bir başvuru ekleyin **STESample.Entities** proje

WCF hizmetimiz bir hizmet başvurusu ihtiyacımız

-   Sağ **STESample.ConsoleTest** projesi **Çözüm Gezgini** seçip **hizmet Başvurusu Ekle...**
-   Tıklayın **keşfedin**
-   Girin **BloggingService** tıklayın ve ad alanı olarak **Tamam**

Şimdi biz hizmeti kullanmak için bazı kod yazabilirsiniz.

-   Açık **Program.cs** ve içeriğini aşağıdaki kodla değiştirin.

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.

-   Sağ **STESample.ConsoleTest** projesi **Çözüm Gezgini** seçip **hata ayıklama -&gt; yeni örnek Başlat**

Uygulama yürütüldüğünde, aşağıdaki çıktıyı görürsünüz.

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a>Bir WPF uygulamasından hizmetini kullanma

Hizmetimiz kullanan bir WPF uygulaması oluşturalım.

-   **Dosya -&gt; yeni -&gt; proje...**
-   Seçin **Visual C\#**  sol bölmeden ardından **WPF uygulaması**
-   Girin **STESample.WPFTest** tıklayın ve adı olarak **Tamam**
-   Bir başvuru ekleyin **STESample.Entities** proje

WCF hizmetimiz bir hizmet başvurusu ihtiyacımız

-   Sağ **STESample.WPFTest** projesi **Çözüm Gezgini** seçip **hizmet Başvurusu Ekle...**
-   Tıklayın **keşfedin**
-   Girin **BloggingService** tıklayın ve ad alanı olarak **Tamam**

Şimdi biz hizmeti kullanmak için bazı kod yazabilirsiniz.

-   Açık **MainWindow.xaml** ve içeriğini aşağıdaki kodla değiştirin.

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   Arka plan kod için MainWindow açın (**MainWindow.xaml.cs**) ve içeriğini aşağıdaki kodla değiştirin.

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.

-   Sağ **STESample.WPFTest** projesi **Çözüm Gezgini** seçip **hata ayıklama -&gt; yeni örnek Başlat**
-   Ekran'ı kullanarak verileri işlemek ve yoluyla kullanarak hizmet kaydetmeden **Kaydet** düğmesi

![WPF ana penceresi](~/ef6/media/wpf.png)
