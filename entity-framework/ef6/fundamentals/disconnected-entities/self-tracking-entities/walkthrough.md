---
title: Kendi kendine Izlenen varlıklar Izlenecek yol-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 9bd644461f50a7eff1006cb8866ca9a3b08b6b8d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419535"
---
# <a name="self-tracking-entities-walkthrough"></a>Kendi kendine Izlenen varlıkları gözden geçirme
> [!IMPORTANT]
> Artık kendi kendine izleme varlıkları şablonunu kullanmanızı önermiyoruz. Yalnızca var olan uygulamaları desteklemek için kullanılabilir olmaya devam edecektir. Uygulamanız, bağlantılı olmayan grafik grafiklerle çalışmayı gerektiriyorsa, bu, topluluk tarafından daha etkin bir şekilde geliştirilmiş olan ve alt düzey değişiklik izleme API 'Leri kullanılarak özel kod yazma gibi, kendini Izlemeye benzer bir teknoloji olan, [izleyicileri oluşturan varlıklar](https://trackableentities.github.io/)gibi diğer alternatifleri göz önünde bulundurun.

Bu izlenecek yol, bir Windows Communication Foundation (WCF) hizmetinin bir varlık grafiği döndüren bir işlemi kullanıma sunduğunu gösteren senaryoyu gösterir. Daha sonra, bir istemci uygulaması bu grafiği yönetir ve Entity Framework kullanarak bir veritabanına güncelleştirmeleri doğrulayan ve kaydeden bir hizmet işlemine yapılan değişiklikleri gönderir.

Bu yönergeyi tamamlamadan önce, [kendi kendine Izleme varlıkları](index.md) sayfasını okuduğunuzdan emin olun.

Bu izlenecek yol aşağıdaki eylemleri tamamlar:

-   Erişmek için bir veritabanı oluşturur.
-   Modeli içeren bir sınıf kitaplığı oluşturur.
-   Kendi kendini Izleyen varlık Oluşturucu şablonunu değiştirir.
-   Varlık sınıflarını ayrı bir projeye kaydırır.
-   Varlıkları sorgulama ve kaydetme işlemlerini kullanıma sunan bir WCF hizmeti oluşturur.
-   Hizmeti kullanan istemci uygulamaları (konsol ve WPF) oluşturur.

Bu izlenecek yolda Database First kullanacağız, ancak aynı teknikler Model First için de aynı şekilde uygulanır.

## <a name="pre-requisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için, Visual Studio 'nun yeni bir sürümüne ihtiyacınız olacaktır.

## <a name="create-a-database"></a>Veritabanı Oluşturma

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:

-   Visual Studio 2012 kullanıyorsanız, LocalDB veritabanı oluşturursunuz.
-   Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.

Şimdi veritabanını oluşturalım.

-   Visual Studio’yu açın
-   **&gt; Sunucu Gezgini görüntüle**
-   Veri bağlantıları ' na sağ tıklayın **&gt; bağlantı ekle...**
-   Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak **Microsoft SQL Server** seçmeniz gerekir
-   Hangi hangisinin yüklü olduğuna bağlı olarak, LocalDB veya SQL Express 'e bağlanın
-   Veritabanı adı olarak **Stesample** girin
-   **Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.
-   Yeni veritabanı artık Sunucu Gezgini görüntülenir
-   Visual Studio 2012 kullanıyorsanız
    -   Sunucu Gezgini veritabanında veritabanına sağ tıklayın ve **Yeni sorgu** ' yı seçin.
    -   Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.
-   Visual Studio 2010 kullanıyorsanız
    -   **Veri&gt; Transact SQL Düzenleyicisi-&gt; yeni sorgu bağlantısı ' nı seçin...**
    -   Sunucu adı olarak **.\\SQLExpress** girin ve **Tamam 'a** tıklayın
    -   Sorgu Düzenleyicisi 'nin en üstündeki açılan listeden **Stesample** veritabanını seçin
    -   Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **SQL 'ı Yürüt** ' ü seçin.

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

İlk olarak, modeli içine koyabileceğiniz bir proje gerekiyor.

-   **Dosya-&gt; yeni&gt; projesi...**
-   Sol bölmeden ve sonra **sınıf kitaplığı** 'Ndan **Visual C\#** seçin
-   Ad olarak **Stesample** girin ve **Tamam 'a** tıklayın

Şimdi, veritabanımıza erişmek için EF tasarımcısında basit bir model oluşturacağız:

-   **Proje-&gt; yeni öğe Ekle...**
-   Sol bölmedeki **verileri** seçin ve ardından **ADO.net varlık veri modeli**
-   Ad olarak **BloggingModel** girin ve **Tamam 'a** tıklayın
-   **Veritabanından oluştur** ' u seçin ve **İleri** ' ye tıklayın.
-   Önceki bölümde oluşturduğunuz veritabanı için bağlantı bilgilerini girin
-   Bağlantı dizesinin adı olarak **BloggingContext** girin ve **İleri** ' ye tıklayın.
-   **Tablolar** ' ın yanındaki kutuyu Işaretleyin ve **son** ' a tıklayın.

## <a name="swap-to-ste-code-generation"></a>STE kod oluşturmaya değiştirme

Şimdi varsayılan kod oluşturmayı devre dışı bırakmaktan ve kendini Izlemeye yönelik olarak takas etmemiz gerekiyor.

### <a name="if-you-are-using-visual-studio-2012"></a>Visual Studio 2012 kullanıyorsanız

-   **Çözüm Gezgini** 'de **BloggingModel. edmx** ' i genişletin ve **BloggingModel.tt** ve **BloggingModel.Context.tt**
    silin. *Bu, varsayılan kod üretimini devre dışı bırakır*
-   EF Designer yüzeyinde boş bir alana sağ tıklayın ve **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.
-   Sol bölmeden **çevrimiçi** ' i seçin ve **Ste Generator** araması yapın
-   **C\#şablonu Için Ste Generator** ' yı seçin, ad olarak **Stetemplate** girin ve **Ekle** ' ye tıklayın.
-   **STETemplate.tt** ve **STETemplate.Context.tt** dosyaları, BloggingModel. edmx dosyasının altına iç içe eklenir

### <a name="if-you-are-using-visual-studio-2010"></a>Visual Studio 2010 kullanıyorsanız

-   EF Designer yüzeyinde boş bir alana sağ tıklayın ve **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.
-   Sol bölmedeki **kodu** seçin ve ardından **ADO.net kendi kendine izleme varlık Oluşturucu**
-   Ad olarak **Stetemplate** girin ve **Ekle** ' ye tıklayın.
-   **STETemplate.tt** ve **STETemplate.Context.tt** dosyaları projenize doğrudan eklenir

## <a name="move-entity-types-into-separate-project"></a>Varlık türlerini ayrı projeye taşı

Kendi kendini Izleyen varlıkları kullanmak için, istemci uygulamamız, modelinizde oluşturulan varlık sınıflarına erişmesi gerekir. Tüm modeli istemci uygulamasına göstermek istemediğimiz için, varlık sınıflarını ayrı bir projeye taşıyacağız.

İlk adım, mevcut projede varlık sınıfları oluşturmayı durdurmaktır:

-   **Çözüm Gezgini** 'de **STETemplate.tt** öğesine sağ tıklayın ve **Özellikler** ' i seçin
-   **Özellikler** penceresinde **CustomTool** özelliğinden **TextTemplatingFileGenerator** seçimini kaldırın
-   **Çözüm Gezgini** 'de **STETemplate.tt** öğesini genişletin ve içinde iç içe yerleştirilmiş tüm dosyaları silin

Ardından, yeni bir proje ekleyeceğiz ve bu projede varlık sınıfları oluşturacağız

-   **Dosya-&gt; Add-&gt; projesi...**
-   Sol bölmeden ve sonra **sınıf kitaplığı** 'Ndan **Visual C\#** seçin
-   Ad olarak **Stesample. Entities** girin ve **Tamam 'a** tıklayın
-   **Proje-&gt; var olan öğe Ekle...**
-   **Stesample** proje klasörüne gitme
-   Tüm dosyaları görüntülemek için seçin **(\*.\*)**
-   **STETemplate.tt** dosyasını seçin
-   **Ekle** düğmesinin yanındaki aşağı açılan oka tıklayın ve **bağlantı olarak ekle** ' yi seçin.

    ![Bağlı Şablon Ekle](~/ef6/media/addlinkedtemplate.png)

Ayrıca, varlık sınıflarının bağlamla aynı ad alanında oluşturulduğundan emin veriyoruz. Bu, uygulamamız genelinde eklememiz gereken using deyimlerinin sayısını azaltır.

-   **Çözüm Gezgini** bağlantılı **STETemplate.tt** sağ tıklayın ve **Özellikler** ' i seçin
-   **Özellikler** penceresinde, **Stesample** Için **özel araç ad alanını** ayarlayın

STE şablonu tarafından oluşturulan kod, derlemek için **System. Runtime. Serialization** öğesine bir başvuruya sahip olacaktır. Bu kitaplık, serileştirilebilir varlık türlerinde kullanılan WCF **DataContract** ve **DataMember** öznitelikleri için gereklidir.

-   **Çözüm Gezgini** ' deki **Stesample. Entities** projesine sağ tıklayın ve **Başvuru Ekle...** seçeneğini belirleyin.
    -   Visual Studio 2012- **System. Runtime. Serialization** seçeneğinin yanındaki kutuyu Işaretleyin ve **Tamam** ' a tıklayın.
    -   Visual Studio 2010- **System. Runtime. Serialization** öğesini seçin ve **Tamam 'a** tıklayın

Son olarak, içinde bağlamımız bir proje varlık türlerine bir başvuruya sahip olur.

-   **Çözüm Gezgini** ' deki **stesample** projesine sağ tıklayın ve **Başvuru Ekle...** öğesini seçin.
    -   Visual Studio 2012-sol bölmeden **çözüm** ' i seçin, **Stesample. varlıklar** ' ın yanındaki kutuyu işaretleyin ve **Tamam** ' a tıklayın.
    -   Visual Studio 2010- **Projeler** sekmesini seçin, **Stesample. Entities** ' yi seçin ve **Tamam** ' a tıklayın.

>[!NOTE]
> Varlık türlerini ayrı bir projeye taşımaya yönelik başka bir seçenek de şablon dosyasını varsayılan konumundan bağlamak yerine taşımaktır. Bunu yaparsanız, edmx dosyasına (Bu örnekte **..\\BloggingModel. edmx**) göreli yolu sağlamak Için şablonda **InputFile** değişkenini güncelleştirmeniz gerekir.

## <a name="create-a-wcf-service"></a>WCF hizmeti oluşturma

Şimdi verilerimizi açığa çıkarmak için bir WCF hizmeti eklemenin zamanı, projeyi oluşturarak başlayacağız.

-   **Dosya-&gt; Add-&gt; projesi...**
-   Sol bölmeden ve ardından **WCF hizmeti uygulamasından** **Visual C\#** seçin
-   Ad olarak **Stesample. Service** girin ve **Tamam 'a** tıklayın
-   **System. Data. Entity** derlemesine bir başvuru ekleyin
-   **Stesample** ve **Stesample. Entities** projelerine bir başvuru ekleyin

EF bağlantı dizesini, çalışma zamanında bulunduğu için bu projeye kopyalamamız gerekir.

-    **Stesample **projesi için **app. config** dosyasını açın ve **connectionStrings** öğesini kopyalayın
-   **ConnectionString** öğesini, **Stesample. Service** projesindeki **Web. config** dosyasının **yapılandırma** öğesinin bir alt öğesi olarak yapıştırın

Şimdi de gerçek hizmeti uygulama zamanı.

-   **IService1.cs** açın ve içeriğini aşağıdaki kodla değiştirin

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

-   **Service1. svc** açın ve içerikleri aşağıdaki kodla değiştirin

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

## <a name="consume-the-service-from-a-console-application"></a>Hizmeti bir konsol uygulamasından tüketme

Hizmetimizi kullanan bir konsol uygulaması oluşturalım.

-   **Dosya-&gt; yeni&gt; projesi...**
-   Sol bölmeden ve sonra **konsol uygulamasında** **Visual C\#** ' yi seçin.
-   Ad olarak **Stesample. ConsoleTest** girin ve **Tamam 'a** tıklayın
-   **Stesample. Entities** projesine bir başvuru ekleyin

WCF hizmetinize bir hizmet başvurusu gerekiyor

-   **Çözüm Gezgini** ' deki **Stesample. consoletest** projesine sağ tıklayın ve **hizmet başvurusu Ekle...** öğesini seçin.
-   **Keşfet** 'e tıklayın
-   Ad alanı olarak **BloggingService** girin ve **Tamam 'a** tıklayın

Artık hizmeti tüketmek için bazı kodlar yazabiliriz.

-   **Program.cs** açın ve içeriğini aşağıdaki kodla değiştirin.

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

Artık uygulamayı çalışır durumda görmek için çalıştırabilirsiniz.

-   **Çözüm Gezgini** ' deki **Stesample. consoletest** projesine sağ tıklayın ve **Hata Ayıkla-&gt; yeni örnek Başlat** ' ı seçin

Uygulama yürütüldüğünde aşağıdaki çıktıyı görürsünüz.

```console
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

## <a name="consume-the-service-from-a-wpf-application"></a>Bir WPF uygulamasından hizmeti tüketme

Hizmetimizi kullanan bir WPF uygulaması oluşturalım.

-   **Dosya-&gt; yeni&gt; projesi...**
-   Sol bölmeden ve ardından **WPF uygulamasında** **Visual C\#** seçin
-   Ad olarak **Stesample. WPFTest** girin ve **Tamam 'a** tıklayın
-   **Stesample. Entities** projesine bir başvuru ekleyin

WCF hizmetinize bir hizmet başvurusu gerekiyor

-   **Çözüm Gezgini** ' deki **Stesample. wpftest** projesine sağ tıklayın ve **hizmet başvurusu Ekle...** öğesini seçin.
-   **Keşfet** 'e tıklayın
-   Ad alanı olarak **BloggingService** girin ve **Tamam 'a** tıklayın

Artık hizmeti tüketmek için bazı kodlar yazabiliriz.

-   **MainWindow. xaml** ' i açın ve içeriği aşağıdaki kodla değiştirin.

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

-   MainWindow (**MainWindow.xaml.cs**) için arka plan kodunu açın ve içeriği aşağıdaki kodla değiştirin

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

Artık uygulamayı çalışır durumda görmek için çalıştırabilirsiniz.

-   **Çözüm Gezgini** ' deki **Stesample. wpftest** projesine sağ tıklayın ve **Hata Ayıkla-&gt; yeni örnek Başlat** ' ı seçin
-   Ekranı kullanarak verileri işleyebilir ve **Kaydet** düğmesini kullanarak hizmet aracılığıyla kaydedebilirsiniz.

![WPF ana penceresi](~/ef6/media/wpf.png)
