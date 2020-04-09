---
title: WPF ile Veri Bağlama - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639139"
---
> [!IMPORTANT]
> **Bu belge sadece .NET Framework'de WPF için geçerlidir**
>
> Bu belge, .NET Framework üzerinde WPF için veri bağlama açıklar. Yeni .NET Core projeleri için Varlık Çerçevesi 6 yerine [EF Core'u](/ef/core) kullanmanızı öneririz. EF Core'da veri bağlama için gerekli belgeler [Sayı #778'da](https://github.com/dotnet/EntityFramework.Docs/issues/778)izlenir.

# <a name="databinding-with-wpf"></a>WPF ile veri bağlama
Bu adım adım gözden geçirme, POCO türlerinin WPF denetimlerine nasıl "ana ayrıntı" biçiminde bağlanılabildiğini gösterir. Uygulama, nesneleri veritabanındaki verilerle doldurmak, değişiklikleri izlemek ve verileri veritabanında kalıcı olarak sürdürmek için Varlık Çerçeve API'lerini kullanır.

Model, bir-çok ilişkisine katılan iki tür **Category** tanımlar:\\Kategori (asıl asıl) ve **Ürün** (bağımlı\\ayrıntı). Daha sonra, Visual Studio araçları wpf denetimleri için modelde tanımlanan türleri bağlamak için kullanılır. WPF veri bağlama çerçevesi ilgili nesneler arasında gezinmeyi sağlar: ana görünümdeki satırları seçmek ayrıntı görünümünün ilgili alt verilerle güncelleştirilmesine neden olur.

Bu iznin ekran görüntüleri ve kod listeleri Visual Studio 2013'ten alınmıştır ancak Visual Studio 2012 veya Visual Studio 2010 ile bu walkthrough'u tamamlayabilirsiniz.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>WPF Veri Kaynakları Oluşturmak için 'Nesne' Seçeneğini Kullanma

Varlık Çerçevesi'nin önceki sürümünde, EF Tasarımcısı ile oluşturulan bir modele dayalı yeni bir Veri Kaynağı oluştururken **Veritabanı** seçeneğini kullanmanızı öneririz. Bunun nedeni, tasarımcının ObjectContext'tan türetilen bir bağlam ve EntityObject'ten türetilen varlık sınıfları oluşturmasıdır. Veritabanı seçeneğini kullanmak, bu API yüzeyi ile etkileşim için en iyi kodu yazmanıza yardımcı olur.

Visual Studio 2012 ve Visual Studio 2013 için EF Designers basit POCO varlık sınıfları ile birlikte DbContext türetilen bir bağlam oluşturur. Visual Studio 2010 ile, bu iznin ilerleyen saatlerinde açıklandığı gibi DbContext kullanan bir kod oluşturma şablonuna geçiş yapmanızı öneririz.

DbContext API yüzeyini kullanırken, bu izbarada gösterildiği gibi yeni bir Veri Kaynağı oluştururken **Nesne** seçeneğini kullanmanız gerekir.

Gerekirse, EF Designer ile oluşturulan modeller için [ObjectContext tabanlı kod oluşturma](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) geri dönebilirsiniz.

## <a name="pre-requisites"></a>Önkoşullar

Bu walkthrough tamamlamak için Visual Studio 2013, Visual Studio 2012 veya Visual Studio 2010 yüklü olması gerekir.

Visual Studio 2010 kullanıyorsanız, NuGet'i de yüklemeniz gerekir. Daha fazla bilgi için [NuGet'i Yükleme'ye](https://docs.microsoft.com/nuget/install-nuget-client-tools)bakın.  

## <a name="create-the-application"></a>Uygulamayı Oluştur

-   Visual Studio’yu açın
-   **Dosya&gt; -&gt; Yeni - Proje....**
-   Sol bölmede **Windows'u** ve sağ bölmede **WPFApplication'ı** seçin
-    **WPFwithEFSample'ı** ad olarak girin
-    **Tamam'ı** seçin

## <a name="install-the-entity-framework-nuget-package"></a>Varlık Çerçevesi NuGet paketini yükleyin

-   Solution Explorer'da **WinFormswithEFSample** projesine sağ tıklayın
-   **NuGet Paketlerini Yönet'i seçin...**
-   NuGet Paketlerini Yönet iletişim **kutusunda, Çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin
-   **Yükle'yi** tıklatın  
    >[!NOTE]
    > EntityFramework derlemesine ek olarak System.ComponentModel.DataAnnotations'a bir başvuru da eklenir. Projenin System.Data.Entity'e bir başvurusu varsa, EntityFramework paketi yüklendiğinde kaldırılır. System.Data.Entity derlemesi artık Entity Framework 6 uygulamaları için kullanılmaz.

## <a name="define-a-model"></a>Model Tanımla

Bu izne Code First veya EF Designer kullanarak bir model uygulamayı seçebilirsiniz. Aşağıdaki iki bölümden birini tamamlayın.

### <a name="option-1-define-a-model-using-code-first"></a>Seçenek 1: Önce Kodu Kullanarak Model Tanımlayın

Bu bölümde, Önce Kod'u kullanarak bir modelin ve ilişkili veritabanının nasıl oluşturulutamamolduğu gösterilmektedir. MODELINIZI EF tasarımcısını kullanarak bir veritabanından tersine mühendislik yapmak için Önce Veritabanı'nı kullanarak bir model tanımlayın **(Seçenek 2: Önce Veritabanı'nı kullanarak bir model tanımlayın)**

Kod İlk geliştirme kullanırken genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız.

-    **WPFwithEFSample'a** yeni bir sınıf ekleyin:
    -   Proje adına sağ tıklayın
    -   **Ekle'yi**seçin, ardından **Yeni Öğe**
    -   **Sınıf'ı** seçin ve sınıf adı için **Ürün** girin
-    **Ürün** sınıf tanımını aşağıdaki kodla değiştirin:

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

**Ürün** sınıfındaki **Kategori** sınıfı ve **Kategori** özelliğindeki **Ürünler** özelliği gezinti özellikleridir. Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türü arasındaki ilişkiyi gezinmenin bir yolunu sağlar.

Varlıkları tanımlamaya ek olarak, DbContext'dan türetilen ve DbSet&lt;TEntity&gt; özelliklerini ortaya çıkaran bir sınıf tanımlamanız gerekir. DbSet&lt;TEntity&gt; özellikleri, içeriğe modele hangi türleri eklemek istediğinizi bildirin.

DbContext türetilmiş türünün bir örneği, nesneleri veritabanındaki verilerle doldurmayı, izlemeyi değiştirmeyi ve verileri veritabanına kalıcı olarak dahil etmeyi içeren çalışma süresi sırasında varlık nesnelerini yönetir.

-   Projeye aşağıdaki tanımla yeni bir **ProductContext** sınıfı ekleyin:

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

Projeyi derle.

### <a name="option-2-define-a-model-using-database-first"></a>Seçenek 2: Önce Veritabanı'nı kullanarak bir model tanımlayın

Bu bölümde, MODELINIZI EF tasarımcısını kullanarak bir veritabanından tersine mühendislik yapmak için Veritabanı İlk'in nasıl kullanılacağı gösterilmektedir. Önceki bölümü tamamladıysanız **(Seçenek 1: Önce Kod'u kullanarak bir model tanımlayın)** sonra bu bölümü atlayın ve doğrudan **Tembel Yükleme** bölümüne gidin.

#### <a name="create-an-existing-database"></a>Varolan Veritabanı Oluşturma

Genellikle varolan bir veritabanını hedefliyorsanız zaten oluşturulur, ancak bu izlenecek yol için erişmek için bir veritabanı oluşturmamız gerekir.

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklıdır:

-   Visual Studio 2010 kullanıyorsanız bir SQL Express veritabanı oluşturuyor olacaksınız.
-   Visual Studio 2012 kullanıyorsanız, bir [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) veritabanı oluşturuyor olacaksınız.

Devam edelim ve veritabanını oluşturalım.

-   **Görünüm&gt; - Sunucu Gezgini**
-   **Veri Bağlantıları -&gt; Bağlantı Ekle'ye** sağ tıklayın...
-   Veri kaynağı olarak Microsoft SQL Server'ı seçmeniz gerekirken, sunucu gezgininden bir veritabanına bağlanmadıysanız

    ![Veri Kaynağını Değiştir](~/ef6/media/changedatasource.png)

-   Hangisini yüklediğinize bağlı olarak LocalDB veya SQL Express'e bağlanın ve **Ürünleri** veritabanı adı olarak girin

    ![Bağlantı Ekle LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Bağlantı Ekspresi Ekle](~/ef6/media/addconnectionexpress.png)

-   **Tamam'ı** seçin ve yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet'i** seçin

    ![Veritabanı Oluşturma](~/ef6/media/createdatabase.png)

-   Yeni veritabanı artık Server Explorer'da görünecek, üzerine sağ tıklanacak ve **Yeni Sorgu'yu** seçecektir
-   Aşağıdaki SQL'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayın ve **Yürüt''''ün**

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a>Ters Mühendis Modeli

Modelimizi oluşturmak için Visual Studio'nun bir parçası olarak yer alan Entity Framework Designer'ı kullanacağız.

-   **Proje&gt; - Yeni Öğe Ekle...**
-   Sol menüden **Veri'yi** seçin ve ardından **Varlık Veri Modeli ADO.NET**
-   Ürün **Model'i** ad olarak girin ve **Tamam'ı** tıklatın
-   Bu, **Varlık Veri Modeli Sihirbazı'nı** başlattı
-   **Veritabanından Oluştur'u** seçin ve **İleri'yi** tıklatın

    ![Model İçeriklerini Seçin](~/ef6/media/choosemodelcontents.png)

-   İlk bölümde oluşturduğunuz veritabanına bağlantıyı seçin, bağlantı dizesinin adı olarak **ProductContext'ı** girin ve **İleri'yi** tıklatın

    ![Bağlantınızı Seçin](~/ef6/media/chooseyourconnection.png)

-   Tüm tabloları almak için 'Tablolar'ın yanındaki onay kutusunu tıklatın ve 'Finish' düğmesini tıklatın

    ![Nesnelerinizi Seçin](~/ef6/media/chooseyourobjects.png)

Ters mühendislik işlemi tamamlandıktan sonra yeni model projenize eklenir ve Entity Framework Designer'da görüntülemeniz için açılır. Veritabanının bağlantı ayrıntılarıyla birlikte projenize bir App.config dosyası da eklendi.

#### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010'da Ek Adımlar

Visual Studio 2010'da çalışıyorsanız, EF6 kod oluşturmayı kullanmak için EF tasarımcısını güncellemeniz gerekir.

-   EF Designer'da modelinizin boş bir noktasına sağ tıklayın ve **Kod Oluşturma Öğesi Ekle'yi seçin...**
-   Sol menüden **Çevrimiçi Şablonlar'ı** seçin ve **DbContext'ı** arayın
-   **C\#için EF 6.x DbContext Jeneratörünü seçin,** ürün adı olarak **ProductsModel'i** girin ve Ekle'yi tıklatın

#### <a name="updating-code-generation-for-data-binding"></a>Veri bağlama için kod oluşturma yı güncelleştirme

EF, T4 şablonlarını kullanarak modelinizden kod oluşturur. Visual Studio ile gönderilen veya Visual Studio galerisinden indirilen şablonlar genel amaçlı kullanım için tasarlanmıştır. Bu, bu şablonlardan oluşturulan varlıkların basit&lt;ICollection&gt; T özelliklerine sahip olduğu anlamına gelir. Ancak, WPF kullanarak veri bağlama yaparken, WPF'nin koleksiyonlarda yapılan değişiklikleri takip edebilmesi için toplama özellikleri için **ObservableCollection'ı** kullanmak istenir. Bu amaçla, observableCollection kullanmak için şablonları değiştirmek olacaktır.

-   Solution **Explorer'ı** açın ve **ProductModel.edmx** dosyasını bulun
-   ProductModel.edmx dosyasının altına yer alacak **ProductModel.tt** dosyayı bulun

    ![WPF Ürün Modeli Şablonu](~/ef6/media/wpfproductmodeltemplate.png)

-   Visual Studio editöründe açmak için ProductModel.tt dosyasına çift tıklayın
-   "**ICollection**" un iki olayını "**ObservableCollection**" ile bulun ve değiştirin. Bunlar yaklaşık olarak 296 ve 484.
-   "**HashSet**" in ilk oluşumunu "**ObservableCollection**" ile bulun ve değiştirin. Bu oluşum yaklaşık 50 satırında yer alır. **Do not** Daha sonra kodda bulunan HashSet'in ikinci oluşumunu değiştirmeyin.
-   "**System.Collections.Generic**" in tek oluşumunu "**System.Collections.ObjectModel**" ile bulun ve değiştirin. Bu yaklaşık 424 hattında yer almaktadır.
-   ProductModel.tt dosyasını kaydedin. Bu, varlıkların yeniden oluşturulması için kodun neden olmalıdır. Kod otomatik olarak yenilenmezse, ProductModel.tt sağ tıklayın ve "Özel Aracı Çalıştır"ı seçin.

Şimdi Category.cs dosyayı açarsanız (ProductModel.tt altında iç içe dir) o zaman Ürünler koleksiyonunda **ObservableCollection&lt;&gt;Ürün**türüne sahip olduğunu görmeniz gerekir.

Projeyi derle.

## <a name="lazy-loading"></a>Tembel Yükleme

**Ürün** sınıfındaki **Kategori** sınıfı ve **Kategori** özelliğindeki **Ürünler** özelliği gezinti özellikleridir. Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türü arasındaki ilişkiyi gezinmenin bir yolunu sağlar.

EF, gezinti özelliğine ilk erişimenizden itibaren ilgili varlıkları veritabanından otomatik olarak yükleme seçeneği sunar. Bu tür yüklemelerle (tembel yükleme olarak adlandırılır), her gezinti özelliğine ilk kez erişirdiğinizde, içerik bağlamında değilse veritabanına karşı ayrı bir sorgu yürütüleceğini unutmayın.

POCO varlık türlerini kullanırken, EF çalışma süresi sırasında türetilmiş proxy türlerinin örnekleri oluşturarak ve ardından yükleme kancasını eklemek için sınıflarınızdaki sanal özellikleri geçersiz kılarak tembel yükleme elde eder. İlgili nesnelerin tembel cereyan etmesini sağlamak için, gezinti özelliği nial **ve** **sanal** (Visual Basic'te**geçersiz kılınabilir)** olarak beyan etmeniz gerekir ve sınıf **mühürlenmemelidir** (Visual Basic'te**Geçersiz kılınmaz).** Veritabanı kullanırken İlk gezinme özellikleri otomatik olarak tembel yükleme sağlamak için sanal yapılır. Kod İlk bölümünde biz aynı nedenle navigasyon özellikleri sanal yapmak için seçti

## <a name="bind-object-to-controls"></a>Nesneyi Denetimlere Bağlama

Modelde tanımlanan sınıfları bu WPF uygulaması için veri kaynağı olarak ekleyin.

-   Ana formu açmak için Solution Explorer'da **MainWindow.xaml'a** çift tıklayın
-   Ana menüden **Project -&gt; Yeni Veri Kaynağı Ekle'yi** seçin ...
    (Visual Studio 2010'da Veri - **Yeni Veri Kaynağı&gt; Ekle...**)
-   Veri Kaynağı Yazı Penceresini Seç'te **Nesne'yi** seçin ve **İleri'yi** tıklatın
-   Veri Nesnelerini Seç iletişim kutusunda, **WPFwithEFSample'ı** iki kez açın ve **Kategori'yi** seçin  
    ***Ürün** veri kaynağını seçmenize gerek yoktur, çünkü ürüne **Kategori**veri kaynağındaki **Category** Ürünün özelliği üzerinden ulaşacağız*  

    ![Veri Nesnelerini Seçin](~/ef6/media/selectdataobjects.png)

-   **Son**'a tıklayın.
-   Veri Kaynakları penceresi MainWindow.xaml penceresinin yanında açılır *Veri Kaynakları penceresi görünmüyorsa, **Görünüm -&gt; Diğer Windows-&gt; Veri Kaynakları** *
-   Pin simgesine bastığımda, Veri Kaynakları penceresi otomatik gizlemez. Pencere zaten görünüyorsa yenileme düğmesine basmanız gerekebilir.

    ![Veri Kaynakları](~/ef6/media/datasources.png)

-    **Kategori** veri kaynağını seçin ve forma sürükleyin.

Bu kaynağı sürüklediğimizde şunlar oldu:

-   **CategoryViewSource** kaynak ve **kategoriDataGrid** kontrolü XAML eklendi 
-   Ana Izgara öğesindeki DataContext özelliği "{StaticResource **categoryViewSource** }" olarak ayarlandı.**categoryViewSource** kaynağı dış\\üst Izgara öğesi için bağlayıcı bir kaynak olarak hizmet vermektedir. İç Izgara öğeleri daha sonra ana Kılavuz'dan Veri Bağlamı değerini devralır (CategoryDataGrid'in ItemsSource özelliği "{Binding}" olarak ayarlanır)

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a>Ayrıntı Izgara ekleme

Kategorileri görüntülemek için bir ızgaramız olduğuna göre, ilişkili Ürünleri görüntülemek için bir ayrıntı ızgarası ekleyelim.

-    **Kategori** veri kaynağının altından **Ürünler** özelliğini seçin ve forma sürükleyin.
    -   **CategoryProductsViewSource** kaynak ve **ürünDataGrid** ızgara XAML eklenir
    -   Bu kaynak için bağlayıcı yol Ürünler olarak ayarlanır
    -   WPF veri bağlama çerçevesi, yalnızca seçili Kategoriile ilgili Ürünlerin **productDataGrid'de** gösterilmesini sağlar
-   Araç Kutusundan **Düğmeyi** forma sürükleyin. **Ad** özelliğini **kaydet düğmesine** ve **Kaydet'e İçerik** özelliğini ayarla. **Save**

Form buna benzer olmalıdır:

![Tasarımcı](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Veri Etkileşimini İşleyen Kod Ekleme

Ana pencereye bazı olay işleyicileri eklemenin zamanı doldu.

-   XAML penceresinde, ** &lt;Pencere** öğesini tıklatın, bu ana pencereyi seçer
-   **Özellikler** penceresinde sağ üstteki **Etkinlikler'i** seçin ve **ardından Yüklenen** etiketin sağındaki metin kutusunu çift tıklatın

    ![Ana Pencere Özellikleri](~/ef6/media/mainwindowproperties.png)

-   Ayrıca **tasarımcıda** Kaydet düğmesini çift tıklatarak **Kaydet** düğmesine tıklayın olayını ekleyin. 

Bu form için arkasındaki kod getiriyor, şimdi veri erişimi gerçekleştirmek için ProductContext kullanmak için kodu edeceğiz. MainWindow kodunu aşağıda gösterildiği gibi güncelleştirin.

Kod, **ProductContext'ın**uzun soluklu bir örneğini bildirir. **ProductContext** nesnesi verileri sorgulamak ve veritabanına kaydetmek için kullanılır. **ProductContext** örneğindeki **Elden Çıkarma()** daha sonra geçersiz kılınan **OnClosing** yönteminden çağrılır.Kod açıklamaları, kodun ne yaptığı hakkında ayrıntılı bilgi sağlar.

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a>WPF Uygulamasını Test Edin

-   Uygulamayı derle ve çalıştır. Önce Kod'u kullandıysanız, sizin için bir **WPFwithEFSample.ProductContext** veritabanı oluşturulduğunu görürsünüz.
-   *Birincil anahtar veritabanı tarafından oluşturulduğundan,* üst ızgaraya bir kategori adı ve alt ızgaraya ürün adları girin Kimlik sütunlarında hiçbir şey girmeyin

    ![Yeni kategoriler ve ürünlerle Ana Pencere](~/ef6/media/screen1.png)

-   Verileri veritabanına kaydetmek için **Kaydet** düğmesine basın

DbContext'S **SaveChanges()** için aramadan sonra, d's veritabanı oluşturulan değerleri ile doldurulur. **SaveChanges()** den sonra **Yenile()** adını verdiğimiz için **DataGrid** denetimleri yeni değerlerle de güncelleştirilir.

![Doldurulan doldurulmuş doldurulmuş ana pencere](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Ek Kaynaklar

WPF kullanarak koleksiyonlara veri bağlama hakkında daha fazla bilgi edinmek için [bu konuya](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) WPF belgelerinde bakın.  
