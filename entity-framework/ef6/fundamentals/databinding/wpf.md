---
title: WPF ile veri bağlama-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416598"
---
# <a name="databinding-with-wpf"></a>WPF ile veri bağlama
Bu adım adım kılavuzda, POCO türlerinin bir "ana-ayrıntı" formunda WPF denetimlerine nasıl bağlanacağı gösterilmektedir. Uygulama, nesneleri veritabanındaki verilerle doldurmak, değişiklikleri izlemek ve verileri veritabanına kalıcı hale getirmek için Entity Framework API 'Lerini kullanır.

Model bire çok ilişkiye katılan iki tür tanımlar: **Kategori** (asıl\\ana) ve **ürün** (bağımlı\\ayrıntısı). Daha sonra, Visual Studio Araçları modelde tanımlanan türleri WPF denetimlerine bağlamak için kullanılır. WPF veri bağlama çerçevesi ilgili nesneler arasında gezinmeyi sağlar: ana görünümdeki satırları seçme, ayrıntı görünümünün karşılık gelen alt verilerle güncelleştirilmesine neden olur.

Bu izlenecek yolda ekran görüntüleri ve kod listeleri Visual Studio 2013 alınmıştır ancak Visual Studio 2012 veya Visual Studio 2010 ile bu yönergeyi tamamlayabilirsiniz.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>WPF veri kaynakları oluşturmak için ' nesne ' seçeneğini kullanın

Entity Framework önceki sürümüyle, EF Designer ile oluşturulmuş bir modele dayalı yeni bir veri kaynağı oluştururken **veritabanı** seçeneğini kullanmanızı öneririz. Bunun nedeni, tasarımcının ObjectContext 'ten türetilen ve EntityObject öğesinden türetilen varlık sınıflarından bir bağlam üretmesidir. Veritabanı seçeneğini kullanmak, bu API yüzeyine etkileşimde bulunmak için en iyi kodu yazmanıza yardımcı olur.

Visual Studio 2012 ve Visual Studio 2013 için EF tasarımcıları, basit POCO varlık sınıflarıyla birlikte DbContext 'ten türeten bir bağlam oluşturur. Visual Studio 2010 ile, bu kılavuzda daha sonra açıklandığı gibi DbContext kullanan bir kod oluşturma şablonuna değiştirmeyi öneririz.

DbContext API yüzeyini kullanırken, bu kılavuzda gösterildiği gibi yeni bir veri kaynağı oluştururken **nesne** seçeneğini kullanmanız gerekir.

Gerekirse, EF Designer ile oluşturulan modeller için [ObjectContext tabanlı kod oluşturmaya döndürebilirsiniz](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) .

## <a name="pre-requisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için Visual Studio 2013, Visual Studio 2012 veya Visual Studio 2010 yüklü olmalıdır.

Visual Studio 2010 kullanıyorsanız, NuGet ' i de yüklemelisiniz. Daha fazla bilgi için bkz. [NuGet 'ı yükleme](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Uygulamayı oluşturma

-   Visual Studio’yu açın
-   **Dosya-&gt; yeni&gt; projesi....**
-   Sol bölmedeki **Windows** ve sağ bölmedeki **WPFApplication** seçin
-   Ad olarak **WPFwithEFSample** girin
-    **Tamam 'ı** seçin

## <a name="install-the-entity-framework-nuget-package"></a>Entity Framework NuGet paketini yükler

-   Çözüm Gezgini, **Winformyüzthefsample** projesine sağ tıklayın
-   **NuGet Paketlerini Yönet...** seçeneğini belirleyin.
-   NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin
-   **Install** 'a tıklayın  
    >[!NOTE]
    > EntityFramework derlemesine ek olarak, System. ComponentModel. Dataaçıklamalarda bir başvuru de eklenir. Projenin System. Data. Entity başvurusu varsa, EntityFramework paketi yüklendiğinde kaldırılır. System. Data. Entity derlemesi artık Entity Framework 6 uygulamaları için kullanılmıyor.

## <a name="define-a-model"></a>Model tanımlama

Bu izlenecek yolda, Code First veya EF tasarımcısını kullanarak bir model uygulamayı seçebilirsiniz. Aşağıdaki iki bölümden birini doldurun.

### <a name="option-1-define-a-model-using-code-first"></a>Seçenek 1: Code First kullanarak model tanımlama

Bu bölümde, Code First kullanarak bir modelin ve ilişkili veritabanının nasıl oluşturulacağı gösterilmektedir. Bir sonraki bölüme atlayın (**seçenek 2: Database First kullanarak bir model tanımlayın)** , bir VERITABANıNDAN, EF Designer kullanarak modelinize ters mühendislik uygulamak için Database First kullanmayı tercih ediyorsanız

Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız.

-   WPFwithEFSample için yeni bir sınıf ekleyin **:**
    -   Proje adına sağ tıklayın
    -   **Ekle**' yi ve ardından **Yeni öğe** ' yi seçin
    -   Sınıf adı için **sınıf** ' ı seçin ve **ürün** girin
-    **Ürün** sınıfı tanımını aşağıdaki kodla değiştirin:

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

**Product** sınıfında **category** sınıfı ve **category** özelliğindeki **Products** özelliği gezinti özellikleridir. Entity Framework, gezinti özellikleri iki varlık türü arasında bir ilişkiye gitmek için bir yol sağlar.

Varlıkları tanımlamaya ek olarak, DbContext 'ten türeten bir sınıf tanımlamanız ve DbSet&lt;TEntity&gt; özelliklerini kullanıma sunabilmeniz gerekir. DbSet&lt;TEntity&gt; özellikleri, bağlamın modele hangi türleri dahil etmek istediğinizi bilmesini sağlar.

DbContext türetilmiş türünün bir örneği, çalışma zamanı sırasında varlık nesnelerini yönetir, bu da nesneleri bir veritabanındaki verilerle doldurmayı, değişiklik izlemeyi ve kalıcı verileri veritabanına getirmeyi içerir.

-   Aşağıdaki tanıma sahip projeye yeni bir **Productcontext** sınıfı ekleyin:

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

Projeyi derleyin.

### <a name="option-2-define-a-model-using-database-first"></a>Seçenek 2: Database First kullanarak model tanımlama

Bu bölümde, EF Designer kullanarak bir veritabanından modelinize ters mühendislik uygulamak için Database First nasıl kullanılacağı gösterilmektedir. Önceki bölümü (**1. seçenek: Code First kullanarak model tanımlama)** tamamladıysanız, bu bölümü atlayın ve doğrudan **geç yükleme** bölümüne gidin.

#### <a name="create-an-existing-database"></a>Var olan bir veritabanı oluştur

Genellikle, var olan bir veritabanını hedeflerken zaten oluşturulur, ancak bu izlenecek yol için, erişmek üzere bir veritabanı oluşturulması gerekir.

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:

-   Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.
-   Visual Studio 2012 kullanıyorsanız, [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) veritabanı oluşturursunuz.

Şimdi veritabanını oluşturalım.

-   **&gt; Sunucu Gezgini görüntüle**
-   Veri bağlantıları ' na sağ tıklayın **&gt; bağlantı ekle...**
-   Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak Microsoft SQL Server seçmeniz gerekir

    ![Veri kaynağını Değiştir](~/ef6/media/changedatasource.png)

-   Hangisini yüklediğinize bağlı olarak LocalDB 'ye veya SQL Express 'e bağlanın ve veritabanı adı olarak **ürünleri** girin

    ![Bağlantı LocalDB Ekle](~/ef6/media/addconnectionlocaldb.png)

    ![Bağlantı Express ekleme](~/ef6/media/addconnectionexpress.png)

-   **Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.

    ![Veritabanı Oluşturma](~/ef6/media/createdatabase.png)

-   Yeni veritabanı artık Sunucu Gezgini görüntülenir, sağ tıklayıp **Yeni sorgu** ' yı seçin.
-   Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.

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

#### <a name="reverse-engineer-model"></a>Tersine mühendislik modeli

Modelimizi oluşturmak için Visual Studio 'nun bir parçası olarak dahil edilen Entity Framework Designer kullanacağız.

-   **Proje-&gt; yeni öğe Ekle...**
-   Sol menüden **verileri** seçin ve ardından **ADO.net varlık veri modeli**
-   Ad olarak **ProductModel** girin ve **Tamam 'a** tıklayın
-   Bu, **varlık veri modeli Sihirbazı 'nı** başlatır
-   **Veritabanından oluştur** ' u seçin ve **İleri** ' ye tıklayın.

    ![Model Içeriğini seçin](~/ef6/media/choosemodelcontents.png)

-   İlk bölümde oluşturduğunuz veritabanına bağlantıyı seçin, bağlantı dizesinin adı olarak **Productcontext** girin ve **İleri** ' ye tıklayın.

    ![Bağlantınızı seçin](~/ef6/media/chooseyourconnection.png)

-   Tüm tabloları içeri aktarmak için ' tablolar ' seçeneğinin yanındaki onay kutusuna tıklayın ve ' son ' a tıklayın

    ![Nesnelerinizi seçin](~/ef6/media/chooseyourobjects.png)

Tersine mühendislik işlemi tamamlandıktan sonra, yeni model projenize eklenir ve Entity Framework Designer görüntülemeniz için açılır. Veritabanına yönelik bağlantı ayrıntılarına sahip bir App. config dosyası da projenize eklenmiştir.

#### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010 ' de ek adımlar

Visual Studio 2010 ' de çalışıyorsanız, EF6 kod oluşturmayı kullanmak için EF tasarımcısını güncelleştirmeniz gerekir.

-   EF Designer 'daki modelinizin boş bir noktasına sağ tıklayıp **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.
-   Sol menüden **çevrimiçi şablonlar** ' ı seçin ve **DbContext** ' i arayın
-   **C\#Için EF 6. x DbContext oluşturucusunu seçin,** ad olarak **productsmodel** girin ve Ekle ' ye tıklayın.

#### <a name="updating-code-generation-for-data-binding"></a>Veri bağlama için kod üretimi güncelleştiriliyor

EF, modelden T4 şablonları kullanarak kod oluşturur. Visual Studio ile birlikte gelen veya Visual Studio galerisinden indirilen şablonlar genel amaçlı kullanım amaçlıdır. Bu, bu şablonlardan oluşturulan varlıkların basit ICollection&lt;T&gt; özelliklerine sahip olduğu anlamına gelir. Bununla birlikte, WPF kullanarak veri bağlama yaparken, WPF 'in koleksiyonlarda yapılan değişiklikleri izleyebilmesi için koleksiyon özellikleri için **ObservableCollection** kullanılması tercih edilir. Bu uçta, ObservableCollection kullanmak için şablonları değiştiririz.

-   **Çözüm Gezgini** açın ve **ProductModel. edmx** dosyasını bulun
-   ProductModel. edmx dosyasının altında iç içe olacak **ProductModel.tt** dosyasını bulun

    ![WPF ürün modeli şablonu](~/ef6/media/wpfproductmodeltemplate.png)

-   Visual Studio Düzenleyicisi 'nde açmak için ProductModel.tt dosyasına çift tıklayın
-   "**ICollection**" sözcüğünün iki yinelemesini "**ObservableCollection**" ile bulur ve değiştirin. Bunlar, 296 ve 484 satırlarında yaklaşık olarak bulunur.
-   İlk "**HashSet**" oluşumunu "**ObservableCollection**" ile bulur ve değiştirin. Bu oluşum yaklaşık olarak 50. satırda bulunur. Kodda daha sonra bulunan ikinci diyez kümesi **oluşumunu değiştirmeyin.**
-   Tek "**System. Collections. Generic**" oluşumunu "**System. Collections. ObjectModel**" ile bulur ve değiştirir. Bu yaklaşık 424. satırda bulunur.
-   ProductModel.tt dosyasını kaydedin. Bu, varlıkların kodunun yeniden üretilmesine neden olmalıdır. Kod otomatik olarak yeniden oluşturmaz, ProductModel.tt 'e sağ tıklayın ve "özel araç Çalıştır" seçeneğini belirleyin.

Artık Category.cs dosyasını (ProductModel.tt altında iç içe yerleştirilmiş) açarsanız, Products koleksiyonunun **ObservableCollection&lt;ürün&gt;** türünde olduğunu görmeniz gerekir.

Projeyi derleyin.

## <a name="lazy-loading"></a>Geç yükleme

**Product** sınıfında **category** sınıfı ve **category** özelliğindeki **Products** özelliği gezinti özellikleridir. Entity Framework, gezinti özellikleri iki varlık türü arasında bir ilişkiye gitmek için bir yol sağlar.

EF, gezinti özelliğine ilk kez eriştiğinizde ilgili varlıkları veritabanından otomatik olarak yükleme seçeneği sunar. Bu yükleme türü (yavaş yükleme olarak adlandırılır) ile, her gezinti özelliğine ilk kez eriştiğinizde, içerik bağlamda değilse veritabanına karşı ayrı bir sorgu yürütülecektir.

POCO varlık türleri kullanılırken, EF, çalışma zamanı sırasında türetilmiş proxy türlerinin örneklerini oluşturarak ve sonra yükleme kancasını eklemek için sınıflarınızda sanal özellikleri geçersiz kılarak geç yüklemeye erişir. İlgili nesnelerin geç yüklemesini almak için, gezinti özelliği alıcıları ' ı **Public** ve **Virtual** (Visual Basic) olarak bildirmeniz ve sınıfınız **mühürlenmemelidir** **(Visual Basic** içinde**NotOverridable** ). Database First gezinti özelliklerinin kullanılması, yavaş yüklemeyi etkinleştirmek için otomatik olarak sanal hale getirilir. Code First bölümünde, aynı nedenden dolayı gezinti özelliklerini sanal hale getirmeyi seçtik

## <a name="bind-object-to-controls"></a>Nesneyi denetimlere bağlama

Modelde tanımlanan sınıfları bu WPF uygulaması için veri kaynağı olarak ekleyin.

-   Ana formu açmak için Çözüm Gezgini **MainWindow. xaml** öğesine çift tıklayın
-   Ana menüden **Proje-&gt; yeni veri kaynağı Ekle...** öğesini seçin.
    (Visual Studio 2010 ' de, **veri&gt; yeni veri kaynağı Ekle...** ' i seçmeniz gerekir.)
-   Bir veri kaynağı türü seçin penceresinde, **nesne** ' yi seçin ve **İleri** ' ye tıklayın.
-   Veri nesnelerini seçin iletişim kutusunda, **WPFwithEFSample** iki kez katlamayı kaldırın ve **Kategori** ' yi seçin.  
    ***Ürün veri kaynağını** seçme gereksinimi yoktur, çünkü bu Işlem, **Kategori** veri kaynağındaki **ürünün**özelliği aracılığıyla sunulacaktır*  

    ![Veri nesnelerini seçin](~/ef6/media/selectdataobjects.png)

-   **Son**'a tıklayın.
-   Veri kaynakları penceresi açık değilse, MainWindow. xaml penceresinin yanında veri kaynakları penceresi açılır.  ***&gt; diğer Windows-&gt; veri kaynaklarını görüntüle** ' yi seçin*
-   Veri kaynakları penceresi otomatik olarak gizlenmemesi için Sabitle simgesine basın. Pencere zaten görünür durumdaysa Yenile düğmesine vurması gerekebilir.

    ![Veri Kaynakları](~/ef6/media/datasources.png)

-    **Kategori** veri kaynağını seçin ve form üzerine sürükleyin.

Bu kaynağı sürüklediğiniz sırada şunlar oldu:

-   **Categoryviewsource** kaynağı ve **CATEGORYDATAGRID** denetimi xaml 'ye eklendi 
-   Üst kılavuz öğesindeki DataContext özelliği "{StaticResource **Categoryviewsource** }" olarak ayarlandı. **Categoryviewsource** kaynağı, dış\\üst kılavuz öğesi için bir bağlama kaynağı işlevi görür. İç kılavuz öğeleri daha sonra üst ızgaraya DataContext değerini devralınır (categoryDataGrid 'in ItemId özelliği "{Binding}" olarak ayarlanır)

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

## <a name="adding-a-details-grid"></a>Ayrıntı Kılavuzu ekleme

Şimdi Kategoriler görüntüleyen bir kılavuza sahip olduğumuz ilgili ürünleri göstermek için bir ayrıntı Kılavuzu ekleyelim.

-    **Kategori** veri kaynağı altında **Products** özelliğini seçin ve forma sürükleyin.
    -   **Categoryproductsviewsource** kaynağı ve **PRODUCTDATAGRID** Kılavuzu xaml 'ye eklenir
    -   Bu kaynak için bağlama yolu ürünler olarak ayarlandı
    -   WPF veri bağlama çerçevesi yalnızca seçili kategoriyle ilgili ürünlerin **Productdatagrid** 'de görünmesini sağlar
-   Araç kutusu ' ndan **düğme** ' ye sürükleyin. **Name** özelliğini **Buttonsave** , **içerik** özelliği ise **Save**olarak ayarlayın.

Form şuna benzer görünmelidir:

![Tasarımcı](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Veri etkileşimini Işleyen kod ekleme

Ana pencereye bazı olay işleyicileri ekleme zamanı.

-   XAML penceresinde **&lt;pencere** öğesine tıklayın, bu, ana pencereyi seçer
-   **Özellikler** penceresinde, sağ üst köşedeki **Olaylar** ' ı seçin ve ardından **yüklenen** etiketin sağ tarafındaki metin kutusunu çift tıklatın

    ![Ana pencere özellikleri](~/ef6/media/mainwindowproperties.png)

-   Ayrıca, tasarımcıda Kaydet düğmesine çift tıklayarak **Kaydet** düğmesine ait **Click** olayını da ekleyin. 

Bu, sizi form için arka plan koduna getirir, artık kodu, veri erişimi gerçekleştirmek için ProductContext 'i kullanacak şekilde düzenliyoruz. MainWindow için kodu aşağıda gösterildiği gibi güncelleştirin.

Kod, **Productcontext**'in uzun süre çalışan bir örneğini bildirir. Verileri sorgulamak ve veritabanına kaydetmek için **Productcontext** nesnesi kullanılır. **Productcontext** örneğindeki **Dispose ()** , geçersiz kılınan **OnClosing** yönteminden çağırılır. Kod açıklamaları kodun ne yaptığını hakkında ayrıntılar sağlar.

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

## <a name="test-the-wpf-application"></a>WPF uygulamasını test etme

-   Uygulamayı derleyin ve çalıştırın. Code First kullandıysanız, sizin için bir **WPFwithEFSample. ProductContext** veritabanının oluşturulduğunu görürsünüz.
-   Üst kılavuza bir kategori adı girin ve *birincil anahtar veritabanı tarafından oluşturulduğundan alt kılavuzdaki ürün adları ID sütunlarında hiçbir şey girmeyin*

    ![Yeni kategoriler ve ürünlerle ana pencere](~/ef6/media/screen1.png)

-   Verileri veritabanına kaydetmek için **Kaydet** düğmesine basın

DbContext 'in **SaveChanges ()** çağrısından sonra, kimlikler veritabanı tarafından oluşturulan değerlerle doldurulur. **SaveChanges** () sonrasında **Refresh (** ) çağırdığımız için **DataGrid** denetimleri yeni değerlerle de güncelleştirilir.

![Doldurulan kimlikleri içeren ana pencere](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Ek Kaynaklar

WPF kullanarak koleksiyonlara veri bağlama hakkında daha fazla bilgi edinmek için WPF belgelerindeki [Bu konuya](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) bakın.  
