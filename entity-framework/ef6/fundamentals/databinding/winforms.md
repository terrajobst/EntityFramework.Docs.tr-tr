---
title: WinForms - EF6 ile veri bağlama
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 8da5bf653221b7919abb89d6d33bc8ed172828a4
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490161"
---
# <a name="databinding-with-winforms"></a>WinForms ile veri bağlama
Bu adım adım kılavuzda, POCO türleri "ana öğe-ayrıntı" formunda (WinForms) Windows Formları denetimleri bağlama işlemi gösterilmektedir. Uygulama, verileri veritabanından nesnelerle doldurmak, değişiklikleri izlemek ve veritabanına verileri kalıcı hale getirmek için Entity Framework kullanır.

Model-çok ilişkisini katılan iki tür tanımlar: Kategori (asıl\\ana) ve ürün (bağımlı\\ayrıntı). Ardından, Visual Studio Araçları, WinForms denetimlere modelde tanımlanan türleri bağlamak için kullanılır. İlgili nesneler arasında gezinmeyi WinForms Veri bağlama çerçeve sağlar: ana görünümünde satırları seçme ile ilgili alt verileri güncelleştirmek ayrıntılı görünüm neden olur.

Bu izlenecek yolda kod listeleri ve ekran görüntüleri, Visual Studio 2013'ten alınır ancak bu kılavuzda Visual Studio 2012 veya Visual Studio 2010 ile tamamlayabilirsiniz.

## <a name="pre-requisites"></a>Ön koşullar

Visual Studio 2013 olması gerekir, bu izlenecek yolu tamamlamak için Visual Studio 2012 veya Visual Studio 2010 'un yüklü.

Visual Studio 2010 kullanıyorsanız, aynı zamanda NuGet yüklemeniz gerekir. Daha fazla bilgi için [NuGet yükleme](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Uygulama oluşturma

-   Visual Studio'yu Aç
-   **Dosya -&gt; yeni -&gt; proje...**
-   Seçin **Windows** sol bölmede ve **Windows FormsApplication** sağ bölmede
-   Girin **WinFormswithEFSample** adı
-   Seçin **Tamam**

## <a name="install-the-entity-framework-nuget-package"></a>Entity Framework NuGet paketini yükle

-   Çözüm Gezgini'nde sağ **WinFormswithEFSample** proje
-   Seçin **NuGet paketlerini Yönet...**
-   NuGet paketlerini Yönet iletişim kutusunda, seçmek **çevrimiçi** sekmesini **EntityFramework** paket
-   Tıklayın **yükleyin**  
    > [!NOTE]
    > EntityFramework derlemeye ek olarak bir başvuru System.ComponentModel.DataAnnotations de eklenir. Projeyi bir başvuru System.Data.Entity varsa, EntityFramework paketi yüklendiğinde, sonra da kaldırılacak. System.Data.Entity derleme için Entity Framework 6 uygulamaları artık kullanılmamaktadır.

## <a name="implementing-ilistsource-for-collections"></a>IListSource koleksiyonlar için uygulama

Koleksiyon Özellikleri, Windows Forms kullanırken sıralama ile iki yönlü veri bağlamayı etkinleştirmek için IListSource arabirimini uygulamalıdır. Bunu yapmak için IListSource işlevselliği eklemek için ObservableCollection genişletmek için kullanacağız.

-   Ekleme bir **ObservableListSource** projeye sınıfı:
    -   Proje adına sağ tıklayın
    -   Seçin **Ekle -&gt; yeni öğe**
    -   Seçin **sınıfı** girin **ObservableListSource** sınıfı adı
-   Aşağıdaki kod ile varsayılan olarak oluşturulan kodu değiştirin:

*Bu sınıf, bağlama yanı sıra sıralama çift yönlü veri sağlar. ObservableCollection sınıfı türetilen&lt;T&gt; ve IListSource açık uygulaması ekler. IListSource GetList() yöntemi ObservableCollection ile eşitlenmiş durumda kalmasını IBindingList uygulamaya geri dönmek için uygulanır. Sıralama ToBindingList tarafından oluşturulan IBindingList uygulanmasını destekler. ToBindingList uzantı yöntemi, EntityFramework derlemede tanımlandı.*

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Bir modeli tanımlamak

Code First veya EF Designer kullanarak bir model uygulamak aşağıdakileri yapabilirsiniz, bu izlenecek yolda seçtiniz. İki bölümden birini tamamlayın.

### <a name="option-1-define-a-model-using-code-first"></a>1. seçenek: Code First kullanarak bir Model tanımlayın

Bu bölümde, bir model ve Code First kullanarak ilişkili veritabanı oluşturma işlemi gösterilmektedir. Sonraki bölümde Atla (**seçeneği 2: veritabanı ilk kullanarak bir modeli tanımlamak)** modelinizi EF designer kullanarak bir veritabanından, bunun yerine veritabanı ilk tersine çevirmek için kullanacağınız, mühendislik

Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın.

-   Yeni bir **ürün** projesine sınıfı
-   Aşağıdaki kod ile varsayılan olarak oluşturulan kodu değiştirin:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   Ekleme bir **kategori** projeye sınıfı.
-   Aşağıdaki kod ile varsayılan olarak oluşturulan kodu değiştirin:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

Varlıklar tanımlayan ek olarak, türetilen bir sınıf tanımlamanız gereken **DbContext** ve ortaya çıkaran **olan DB&lt;TEntity&gt;**  özellikleri. **Olan DB** özellikleri modele dahil etmek istediğiniz hangi türlerin bilmeniz bağlam olanak tanır. **DbContext** ve **olan DB** türleri EntityFramework derlemesinde tanımlanmıştır.

Nesneleri bir veritabanındaki verilerle doldurma içeren çalışma zamanı sırasında varlık nesnesi DbContext türetilmiş türün bir örneğini yönetir, izleme ve kalıcı veri veritabanına değiştirin.

-   Yeni bir **ProductContext** projeye sınıfı.
-   Aşağıdaki kod ile varsayılan olarak oluşturulan kodu değiştirin:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
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

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

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

EF modelinizden T4 şablonlarını kullanarak kod oluşturur. Şablonları Visual Studio ile birlikte gelen veya Visual Studio Galerisi'nden indirilir, genel amaçlı kullanım için tasarlanmıştır. Bu bu şablonlardan oluşturulan varlıkları basit ICollection olduğu anlamına gelir&lt;T&gt; özellikleri. Ancak, veri bağlama yaparken IListSource uygulayan koleksiyon özellikleri sağlamak için tercih edilir. Bu nedenle yukarıdaki ObservableListSource sınıfı oluşturduk ve artık hale getirmek için şablonları değiştirme kullanacağız, bu sınıfı kullanın.

-   Açık **Çözüm Gezgini** ve bulma **ProductModel.edmx** dosyası
-   Bulma **ProductModel.tt** ProductModel.edmx dosyayı içe dosyası

    ![Ürün Model şablonu](~/ef6/media/productmodeltemplate.png)

-   Visual Studio Düzenleyicisi'nde açmak için ProductModel.tt dosyaya çift tıklayın
-   Bul ve Değiştir iki örneği "**ICollection**"ile"**ObservableListSource**". Bu yaklaşık satırlar 296 ve 484 yer alır.
-   Bul ve Değiştir ilk geçtiği "**HashSet**"ile"**ObservableListSource**". Bu durum, yaklaşık 50 satırında bulunur. **Sağlamadığı** HashSet kod içinde bulunan ikinci oluşum değiştirin.
-   ProductModel.tt dosyayı kaydedin. Bu, kodu yeniden oluşturulması varlıklar için neden olmaz. Kodu otomatik olarak yeniden oluşturmaz, ProductModel.tt üzerinde sağ tıklayın ve "Özel aracı çalıştır" ı seçin.

ProductModel.tt altında iç içe) Category.cs dosyasını (artık açın ardından ürünleri koleksiyon türlerinin olduğunu görmeniz gerekir **ObservableListSource&lt;ürün&gt;**.

Projeyi derle.

## <a name="lazy-loading"></a>Yavaş yükleniyor

**Ürünleri** özelliği **kategori** sınıfı ve **kategori** özelliği **ürün** Gezinti özellikleri sınıfı bulunur. Varlık Çerçevesi'nde, gezinti özellikleri iki varlık türleri arasındaki bir ilişki gitmek için bir yol sağlar.

EF ilgili varlıkları veritabanından otomatik olarak, gezinti özelliği ilk kez yüklenirken bir seçeneği sunar. Bu (Gecikmeli yükleme olarak adlandırılır) yükleme türüyle içeriği değil zaten bir bağlamda ise, her gezinti özelliği ilk kez ayrı bir sorgu veritabanına karşı yürütülecek olduğunu unutmayın.

EF yavaş yükleniyor POCO varlık türleri kullanırken, çalışma zamanı sırasında proxy türetilen türlerin örneklerini oluşturarak ve sonra da, sınıflarda yükleme kanca eklemek için sanal özelliklerini geçersiz kılma ulaşır. İlgili nesnelerin yavaş yükleniyor almak için özellik alıcıları olarak Gezinti bildirmelidir **genel** ve **sanal** (**Overridable** Visual Basic'te), ve, sınıfı olmalıdır **korumalı** (**NotOverridable** Visual Basic'te). Kullanarak veritabanı ilk Gezinti özellikleri otomatik olarak yavaş yükleniyor etkinleştirmek için sanal hale getirilir. Gezinti özellikleri aynı nedenden dolayı sanal yapmak seçtik kod Birinci bölümde

## <a name="bind-object-to-controls"></a>Denetimlerine nesne bağlama

Aplikace WinForms için veri kaynağı olarak modelde tanımlı sınıfları ekleyin.

-   Ana menüden **proje -&gt; yeni veri kaynağı Ekle...**
    (Visual Studio 2010'da seçmeniz gerekir. **veri -&gt; yeni veri kaynağı Ekle...** )
-   Bir veri kaynağı türü penceresi Seç seçin **nesne** tıklatıp **İleri**
-   Veri nesneleri iletişim seçin unfold **WinFormswithEFSample** seçin ve iki kez **kategori** ürün veri kaynağı seçmek için gerek yoktur çünkü biz için ürünün alırsınız Kategori veri kaynağı özelliği.

    ![veri kaynağı](~/ef6/media/datasource.png)

-   Tıklayın **son.** 
     *Veri kaynakları penceresinden görüntülenmiyor, seçin *** görünümü -&gt; diğer Windows -&gt; veri kaynakları**
-   Raptiye simgesini, böylece veri kaynakları penceresi otomatik gizleme tuşuna basın. Pencere zaten görülebiliyorsa yenile düğmesine tıklama gerekebilir.

    ![Veri kaynağı 2](~/ef6/media/datasource2.png)

-   Çözüm Gezgini'nde çift tıklayarak **Form1.cs** dosyayı ana form Tasarımcısı'nda açın.
-   Seçin **kategori** veri kaynağı ve formda sürükleyin. Varsayılan olarak, yeni DataGridView (**categoryDataGridView**) ve gezinti araç çubuğu denetimleri tasarımcıya eklendi. Bu denetimler için BindingSource bağlıdır (**categoryBindingSource**) ve Gezgin bağlama (**categoryBindingNavigator**) bileşenleri de oluşturulur.
-   Sütunları düzenleme **categoryDataGridView**. Ayarlamak istediğiniz **CategoryID** sütun salt okunur. Değeri **CategoryID** özelliği, biz verileri kaydettikten sonra veritabanı tarafından oluşturulur.
    -   DataGridView denetimine sağ tıklayın ve sütunları Düzenle...
    -   CategoryID sütunu seçin ve salt okunur True olarak ayarlayın.
    -   Tamam'a basın
-   Kategori veri kaynağı gezintisinde ürünleri seçin ve formda sürükleyin. ProductDataGridView ve productBindingSource formuna eklenir.
-   ProductDataGridView sütunlarda düzenleyin. Kullanıcı, Categoryıd'si ve Kategori sütunları gizleyin ve ProductID salt okunur olarak ayarlamak istiyoruz. ProductID özelliğinin değeri, biz verileri kaydettikten sonra veritabanı tarafından oluşturulur.
    -   DataGridView denetimi sağ tıklayıp **sütunları Düzenle...** .
    -   Seçin **ProductID** sütun ve kümesi **salt okunur** için **True**.
    -   Seçin **CategoryID** sütun ve ENTER tuşuna **Kaldır** düğmesi. İle aynı yapmak **kategori** sütun.
    -   Tuşuna **Tamam**.

    Şu ana kadar biz Tasarımcısı'nda BindingSource bileşenleri ile bizim DataGridView denetimi ilişkili. Sonraki bölümde kod arkasındaki kodda şu anda DbContext tarafından izlenen varlık koleksiyonunu categoryBindingSource.DataSource koymak için ekleyeceğiz. Ne zaman biz sürüklediğiniz ve bırakılan ürün kategorisi, WinForms gezintisinde sürdü ürünleri categoryBindingSource ve productsBindingSource.DataMember özelliğini productsBindingSource.DataSource özelliğini ayarlama dikkat edin. Bu bağlama nedeniyle şu anda seçili kategoriye ait olan ürünleri productDataGridView içinde görüntülenir.
-   Etkinleştirme **Kaydet** sağ fare düğmesine tıklayıp seçerek gezinti araç çubuğunda **etkin**.

    ![1 Form Tasarımcısı](~/ef6/media/form1-designer.png)

-   Kayıt için olay işleyicisi ekleme düğmesi düğmesine çift tıklayın. Olay işleyicisi eklemek ve form için arka plan kod için bilgisayarınızı getirin. Kodu **categoryBindingNavigatorSaveItem\_tıklayın** olay işleyicisi, sonraki bölümde eklenir.

## <a name="add-the-code-that-handles-data-interaction"></a>Veri etkileşimini işleyen kodu ekleyin

Veri erişimi gerçekleştirdiği ProductContext kullanmak için kodu artık ekleyeceğiz. Kodu ana form penceresi, aşağıda gösterildiği gibi güncelleştirin.

Kod ProductContext uzun süre çalışan bir örneğini bildirir. ProductContext nesnesi, sorgu ve veri veritabanına kaydetmek için kullanılır. Dispose() yöntemini ProductContext örneğinde geçersiz kılınan OnClosing yönteminden sonra çağrılır. Kod açıklamaları ne yaptığını hakkında ayrıntılar sağlanmaktadır.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a>Windows Forms uygulamayı test etme

-   Derleme ve çalıştırma ve uygulama işlevini test edebilirsiniz.

    ![Form 1 önce Kaydet](~/ef6/media/form1beforesave.png)

-   Kaydettikten sonra ekranda oluşturulan depolama anahtarları gösterilmektedir.

    ![Form 1 sonra Kaydet](~/ef6/media/form1aftersave.png)

-   Code First kullanılan sonra da göreceksiniz bir **WinFormswithEFSample.ProductContext** veritabanı sizin için oluşturulur.

    ![Server Nesne Gezgini](~/ef6/media/serverobjexplorer.png)
