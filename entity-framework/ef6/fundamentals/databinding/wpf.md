---
title: Veri bağlama WPF - EF6 ile
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: ae399f9f3d1bae2c446b552247bd3af3ca5a2cf9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48575671"
---
# <a name="databinding-with-wpf"></a>Veri bağlama WPF ile
Bu adım adım kılavuzda, POCO türleri "ana öğe-ayrıntı" formunda WPF denetimleri bağlama işlemi gösterilmektedir. Uygulama, verileri veritabanından nesnelerle doldurmak, değişiklikleri izlemek ve veritabanına verileri kalıcı hale getirmek için Entity Framework API'leri kullanır.

Model-çok ilişkisini katılan iki tür tanımlar: **kategori** (asıl\\ana) ve **ürün** (bağımlı\\ayrıntı). Ardından, Visual Studio Araçları, WPF denetimleri için modelde tanımlanan türleri bağlamak için kullanılır. WPF veri bağlama çerçeve ilgili nesneler arasında gezinme sağlar: ana görünümünde satırları seçme ile ilgili alt verileri güncelleştirmek ayrıntılı görünüm neden olur.

Bu izlenecek yolda kod listeleri ve ekran görüntüleri, Visual Studio 2013'ten alınır ancak bu kılavuzda Visual Studio 2012 veya Visual Studio 2010 ile tamamlayabilirsiniz.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>WPF veri kaynakları oluşturmak için 'Object' seçeneğini kullanın.

Kullanılması için kullandığımız Entity Framework'ın önceki sürümüyle **veritabanı** temel EF Designer ile oluşturulan bir model üzerinde yeni bir veri kaynağı oluşturma seçeneği. Tasarımcı ObjectContext türetilmiş bir bağlam ve EntityObject türetilen varlık sınıfları oluşturur, çünkü bu değildi. Veritabanı seçeneğini kullanarak bu API yüzeyi ile etkileşim kurmak için en iyi kod yazmanıza yardımcı olacaktır.

Visual Studio 2012 ve Visual Studio 2013 için EF tasarımcıları basit POCO varlık sınıfları birlikte DbContext türeyen bir bağlam oluşturur. Visual Studio 2010 ile bu anlatımın ilerleyen kısımlarında açıklandığı gibi DbContext kullanan kod oluşturma şablonuna takas öneririz.

DbContext API yüzeyi kullanırken kullanmalısınız **nesne** seçenek yeni bir veri kaynağı oluşturulurken bu örneklerde gösterildiği gibi.

Gerekirse, [ObjectContext göre kod oluşturma için geri](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) EF Designer ile oluşturulan modeller için.

## <a name="pre-requisites"></a>Ön koşullar

Visual Studio 2013 olması gerekir, bu izlenecek yolu tamamlamak için Visual Studio 2012 veya Visual Studio 2010 'un yüklü.

Visual Studio 2010 kullanıyorsanız, aynı zamanda NuGet yüklemeniz gerekir. Daha fazla bilgi için [NuGet yükleme](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Uygulama oluşturma

-   Visual Studio'yu Aç
-   **Dosya -&gt; yeni -&gt; proje...**
-   Seçin **Windows** sol bölmede ve **WPFApplication** sağ bölmede
-   Girin **WPFwithEFSample** adı
-   Seçin **Tamam**

## <a name="install-the-entity-framework-nuget-package"></a>Entity Framework NuGet paketini yükle

-   Çözüm Gezgini'nde sağ **WinFormswithEFSample** proje
-   Seçin **NuGet paketlerini Yönet...**
-   NuGet paketlerini Yönet iletişim kutusunda, seçmek **çevrimiçi** sekmesini **EntityFramework** paket
-   Tıklayın **yükleyin**  
    >[!NOTE]
    > EntityFramework derlemeye ek olarak bir başvuru System.ComponentModel.DataAnnotations de eklenir. Projeyi bir başvuru System.Data.Entity varsa, EntityFramework paketi yüklendiğinde, sonra da kaldırılacak. System.Data.Entity derleme için Entity Framework 6 uygulamaları artık kullanılmamaktadır.

## <a name="define-a-model"></a>Bir modeli tanımlamak

Code First veya EF Designer kullanarak bir model uygulamak aşağıdakileri yapabilirsiniz, bu izlenecek yolda seçtiniz. İki bölümden birini tamamlayın.

### <a name="option-1-define-a-model-using-code-first"></a>1. seçenek: Code First kullanarak bir Model tanımlayın

Bu bölümde, bir model ve Code First kullanarak ilişkili veritabanı oluşturma işlemi gösterilmektedir. Sonraki bölümde Atla (**seçeneği 2: veritabanı ilk kullanarak bir modeli tanımlamak)** modelinizi EF designer kullanarak bir veritabanından, bunun yerine veritabanı ilk tersine çevirmek için kullanacağınız, mühendislik

Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın.

-   Yeni bir sınıfa eklemek **WPFwithEFSample:**
    -   Proje adına sağ tıklayın
    -   Seçin **ekleme**, ardından **yeni öğe**
    -   Seçin **sınıfı** girin **ürün** sınıfı adı
-   Değiştirin **ürün** sınıf tanımını aşağıdaki kod ile:

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

**Ürünleri** özelliği **kategori** sınıfı ve **kategori** özelliği **ürün** Gezinti özellikleri sınıfı bulunur. Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türleri arasındaki bir ilişki gitmek için bir yol sağlar.

Varlıklar tanımlayan yanı sıra olan DB kullanıma sunar ve DbContext türeyen bir sınıfı tanımlamanız gereken&lt;TEntity&gt; özellikleri. Olan DB&lt;TEntity&gt; özellikleri modele dahil etmek istediğiniz hangi türlerin bilmeniz bağlam olanak tanır.

Nesneleri bir veritabanındaki verilerle doldurma içeren çalışma zamanı sırasında varlık nesnesi DbContext türetilmiş türün bir örneğini yönetir, izleme ve kalıcı veri veritabanına değiştirin.

-   Yeni bir **ProductContext** projeye aşağıdaki tanımıyla sınıfı:

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

### <a name="option-2-define-a-model-using-database-first"></a>2. seçenek: Veritabanı ilk kullanarak bir model tanımlayın

Bu bölümde, modelinizi EF designer kullanarak bir veritabanındaki veritabanı ilk tersine mühendislik için nasıl kullanılacağını gösterir. Önceki bölümde tamamladıysanız (**1. seçenek: Code First kullanarak bir modeli tanımlamak)**, ardından bu bölümü atlayın ve gidebilirsiniz **yavaş Yükleniyor** bölümü.

#### <a name="create-an-existing-database"></a>Mevcut bir veritabanı oluşturun

Genellikle zaten oluşturulur varolan bir veritabanını hedeflerken, ancak bu kılavuz erişmek için bir veritabanı oluşturmak ihtiyacımız var.

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:

-   Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.
-   Visual Studio 2012 kullanıyorsanız sonra, oluşturduğunuz bir [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) veritabanı.

Yeni bir ubuntu ve veritabanı oluşturun.

-   **Görünüm -&gt; Sunucu Gezgini**
-   Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**
-   Sunucu gezgininden veritabanına bağlamadıysanız önce Microsoft SQL Server veri kaynağı olarak seçmeniz gerekir

    ![Veri kaynağını Değiştir](~/ef6/media/changedatasource.png)

-   LocalDB veya hangisinin bağlı olarak, yüklü, SQL Express için bağlanın ve girin **ürünleri** veritabanı adı

    ![Bağlantı LocalDB Ekle](~/ef6/media/addconnectionlocaldb.png)

    ![Bağlantı Express Ekle](~/ef6/media/addconnectionexpress.png)

-   Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**

    ![Veritabanı oluşturma](~/ef6/media/createdatabase.png)

-   Yeni veritabanı sunucu Gezgini'nde artık görünecek üzerinde sağ tıklayıp **yeni sorgu**
-   Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**

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

#### <a name="reverse-engineer-model"></a>Ters mühendislik modeli

Modelimiz oluşturmak için Entity Framework Visual Studio'nun bir parçası olarak dahil olan Designer'ın, kullanan oluşturacağız.

-   **Takım projesi -&gt; Yeni Öğe Ekle...**
-   Seçin **veri** sol menüden ardından **ADO.NET varlık veri modeli**
-   Girin **ProductModel** tıklayın ve adı olarak **Tamam**
-   Böylece **varlık veri modeli Sihirbazı**
-   Seçin **veritabanından Oluştur** tıklatıp **İleri**

    ![Model içeriğinin seçin](~/ef6/media/choosemodelcontents.png)

-   İlk bölümde oluşturduğunuz veritabanı bağlantısı seçin, girin **ProductContext** tıklayın ve bağlantı dizesi adı olarak **İleri**

    ![Bağlantınızı seçin](~/ef6/media/chooseyourconnection.png)

-   'Tüm tabloları Al ve 'Son' tablolar' yanındaki onay kutusuna tıklayın.

    ![Nesnelerinizi seçin](~/ef6/media/chooseyourobjects.png)

Ters mühendislik işlemi tamamlandıktan sonra yeni modeli projenize eklenir ve Entity Framework Tasarımcısı'nda görüntülemeniz için açıldı. Bir App.config dosyası ayrıca veritabanı bağlantı ayrıntıları ile projenize eklendi.

#### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010'daki ek adımlar

Ardından Visual Studio 2010'da çalışıyorsanız EF designer EF6 kod oluşturmayı kullan güncelleştirmeniz gerekir.

-   Modelinizin EF Tasarımcısı'nda boş bir nokta üzerinde sağ tıklayıp **kod oluşturma öğesi Ekle...**
-   Seçin **çevrimiçi şablonlar** arayın ve sol menüden **DbContext**
-   Seçin **EF 6.x DbContext Oluşturucu c\#,** girin **ProductsModel** adıyla ve Ekle'ye tıklayın

#### <a name="updating-code-generation-for-data-binding"></a>Kod oluşturma için veri bağlamayı güncelleştirme

EF modelinizden T4 şablonlarını kullanarak kod oluşturur. Şablonları Visual Studio ile birlikte gelen veya Visual Studio Galerisi'nden indirilir, genel amaçlı kullanım için tasarlanmıştır. Bu bu şablonlardan oluşturulan varlıkları basit ICollection olduğu anlamına gelir&lt;T&gt; özellikleri. Ancak, veri yaparken binding WPF kullanarak bunu kullanmak için tercih edilir **ObservableCollection** için koleksiyon özelliklerini, WPF koleksiyonlarına yapılan değişiklikleri izlemek için. Bu amaçla ObservableCollection kullanmak üzere değiştirmek için yapacağız.

-   Açık **Çözüm Gezgini** ve bulma **ProductModel.edmx** dosyası
-   Bulma **ProductModel.tt** ProductModel.edmx dosyayı içe dosyası

    ![WPF ürün modeli şablonu](~/ef6/media/wpfproductmodeltemplate.png)

-   Visual Studio Düzenleyicisi'nde açmak için ProductModel.tt dosyaya çift tıklayın
-   Bul ve Değiştir iki örneği "**ICollection**"ile"**ObservableCollection**". Bu yaklaşık satırlar 296 ve 484 yer alır.
-   Bul ve Değiştir ilk geçtiği "**HashSet**"ile"**ObservableCollection**". Bu durum, yaklaşık 50 satırında bulunur. **Sağlamadığı** HashSet kod içinde bulunan ikinci oluşum değiştirin.
-   Bul ve Değiştir yalnızca oluşumunu "**System.Collections.Generic**"ile"**System.Collections.ObjectModel**". Bu işlem yaklaşık satırında 424 de bulunur.
-   ProductModel.tt dosyayı kaydedin. Bu, kodu yeniden oluşturulması varlıklar için neden olmaz. Kodu otomatik olarak yeniden oluşturmaz, ProductModel.tt üzerinde sağ tıklayın ve "Özel aracı çalıştır" ı seçin.

ProductModel.tt altında iç içe) Category.cs dosyasını (artık açın ardından ürünleri koleksiyon türlerinin olduğunu görmeniz gerekir **ObservableCollection&lt;ürün&gt;**.

Projeyi derle.

## <a name="lazy-loading"></a>Yavaş yükleniyor

**Ürünleri** özelliği **kategori** sınıfı ve **kategori** özelliği **ürün** Gezinti özellikleri sınıfı bulunur. Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türleri arasındaki bir ilişki gitmek için bir yol sağlar.

EF ilgili varlıkları veritabanından otomatik olarak, gezinti özelliği ilk kez yüklenirken bir seçeneği sunar. Bu (Gecikmeli yükleme olarak adlandırılır) yükleme türüyle içeriği değil zaten bir bağlamda ise, her gezinti özelliği ilk kez ayrı bir sorgu veritabanına karşı yürütülecek olduğunu unutmayın.

EF yavaş yükleniyor POCO varlık türleri kullanırken, çalışma zamanı sırasında proxy türetilen türlerin örneklerini oluşturarak ve sonra da, sınıflarda yükleme kanca eklemek için sanal özelliklerini geçersiz kılma ulaşır. İlgili nesnelerin yavaş yükleniyor almak için özellik alıcıları olarak Gezinti bildirmelidir **genel** ve **sanal** (**Overridable** Visual Basic'te), ve, sınıfı olmalıdır **korumalı** (**NotOverridable** Visual Basic'te). Kullanarak veritabanı ilk Gezinti özellikleri otomatik olarak yavaş yükleniyor etkinleştirmek için sanal hale getirilir. Gezinti özellikleri aynı nedenden dolayı sanal yapmak seçtik kod Birinci bölümde

## <a name="bind-object-to-controls"></a>Denetimlerine nesne bağlama

Modeldeki veri kaynağı için bu WPF uygulaması olarak tanımlanan sınıflar ekleyin.

-   Çift **MainWindow.xaml** ana formu açmak için Çözüm Gezgini'nde
-   Ana menüden **proje -&gt; yeni veri kaynağı Ekle...**
    (Visual Studio 2010'da seçmeniz gerekir. **veri -&gt; yeni veri kaynağı Ekle...** )
-   Bir veri kaynağı Typewindow Seç seçin **nesne** tıklatıp **İleri**
-   Veri nesneleri iletişim seçin unfold **WPFwithEFSample** seçin ve iki kez **kategorisi**  
    *Seçilecek gerek yoktur **ürün** veri kaynağı için üzerinden alır çünkü **ürün**'s özelliği **kategori** veri kaynağı*  

    ![Veri nesnelerini seçin](~/ef6/media/selectdataobjects.png)

-   Tıklayın **son.**
-   Veri kaynakları penceresi yanındaki MainWindow.xaml penceresi açıldığında *veri kaynakları penceresinden görüntülenmiyor, seçin **görünümü -&gt; diğer Windows -&gt; veri kaynakları***
-   Raptiye simgesini, böylece veri kaynakları penceresi otomatik gizleme tuşuna basın. Pencere zaten görülebiliyorsa yenile düğmesine tıklama gerekebilir.

    ![Data Sources](~/ef6/media/datasources.png)

-   Seçin **kategori** veri kaynağı ve formda sürükleyin.

Biz bu kaynağı sürüklendiğinde şu oldu:

-   **CategoryViewSource** kaynak ve **categoryDataGrid** için XAML denetimi eklendi 
-   Üst kılavuz öğesi DataContext özelliği ayarlandı "{StaticResource **categoryViewSource** }". **CategoryViewSource** bağlama kaynağı için dış kaynak görür\\üst kılavuz öğesi. İç kılavuz öğeleri ardından üst kılavuz ("{bağlama}" categoryDataGrid'ın ItemsSource özelliği ayarlı) bir DataContext değeri devralacak.

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

## <a name="adding-a-details-grid"></a>Ayrıntılar Kılavuz ekleme

Biz kategorilerini şimdi görüntülemek için bir kılavuz olduğuna göre ilişkili ürünleri görüntülemek için ayrıntıları kılavuz ekleyin.

-   Seçin **ürünleri** özelliği altındaki **kategori** veri kaynağı ve formda sürükleyin.
    -   **CategoryProductsViewSource** kaynak ve **productDataGrid** kılavuz için XAML eklendi
    -   Bu kaynak için bağlama yolunu ürünler için ayarlanır.
    -   WPF veri bağlama çerçevesi sağlar seçilen kategoriye ilgili yalnızca ürünleri gösterilmediğini **productDataGrid**
-   Araç kutusundan sürükleyin **düğmesi** formu açın. Ayarlama **adı** özelliğini **buttonSave** ve **içerik** özelliğini **Kaydet**.

Form şuna benzer görünmelidir:

![Tasarımcı](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Veri etkileşimini işleyen kodu ekleyin

Bu, ana pencereyi bazı olay işleyicileri ekleme zamanı geldi.

-   XAML penceresinde tıklayarak  **&lt;penceresi** öğesi, ana pencereyi seçer
-   İçinde **özellikleri** penceresi seçin **olayları** sağ üst kısımdaki ardından metin kutusunun sağ tarafındaki çift **Loaded** etiketi

    ![Ana pencere özellikleri](~/ef6/media/mainwindowproperties.png)

-   Ayrıca ekleyin **tıklayın** için olay **Kaydet** Tasarımcısı'nda Kaydet düğmesine çift tıklayarak düğmesi. 

Bu, arka plan kod için form getirir, biz artık veri erişimi gerçekleştirdiği ProductContext kullanmak için kodu düzenleme. Kodu MainWindow için aşağıda gösterildiği gibi güncelleştirin.

Kod bir uzun süre çalışan örneğini bildirir **ProductContext**. **ProductContext** nesnesi, sorgu ve veri veritabanına kaydetmek için kullanılır. **Dispose()** üzerinde **ProductContext** örneği adlı sonra geçersiz kılınan'den **OnClosing** yöntemi. Kod açıklamaları ne yaptığını hakkında ayrıntılar sağlanmaktadır.

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

## <a name="test-the-wpf-application"></a>WPF uygulamayı test etme

-   Derleme ve uygulamayı çalıştırın. Code First kullanılan sonra göreceksiniz bir **WPFwithEFSample.ProductContext** veritabanı sizin için oluşturulur.
-   Alt kılavuzunda üst kılavuz ve ürün adlarını bir kategori adı girin *birincil anahtarı veritabanı tarafından oluşturulmuş olduğu için hiçbir şey kimliği sütunlarında girmeyin*

    ![Yeni kategorilerini ve ürünler ile ana penceresi](~/ef6/media/screen1.png)

-   Tuşuna **Kaydet** verileri veritabanına kaydetmek için düğme

DbContext'ın çağrısından sonra **SaveChanges()**, kimlikleri oluşturulan veritabanı değerleri ile doldurulur. Biz denir çünkü **Refresh()** sonra **SaveChanges()** **DataGrid** denetimleri, yeni değerleri ile güncelleştirilir.

![Ana pencere doldurulmuş kimlikleri](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Ek Kaynaklar

Koleksiyonları kullanarak WPF verilerini bağlama hakkında daha fazla bilgi için bkz. [bu konuda](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) WPF belgelerinde.  
