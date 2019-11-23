---
title: WinForms ile veri bağlama-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 4b3eee20ff238864b94ef4edfb97c1bae0713300
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181795"
---
# <a name="databinding-with-winforms"></a>WinForms ile veri bağlama
Bu adım adım izlenecek yol, POCO türlerinin bir "ana-ayrıntı" formunda pencere formları (WinForms) denetimlerine nasıl bağlanacağını gösterir. Uygulama, nesneleri veritabanındaki verilerle doldurmak, değişiklikleri izlemek ve verileri veritabanına kalıcı hale getirmek için Entity Framework kullanır.

Model bire çok ilişkiye katılan iki tür tanımlar: kategori (asıl\\ana) ve ürün (bağımlı\\ayrıntısı). Daha sonra, Visual Studio Araçları modelde tanımlanan türleri WinForms denetimlerine bağlamak için kullanılır. WinForms veri bağlama çerçevesi ilgili nesneler arasında gezinmeyi sağlar: ana görünümdeki satırları seçme, ayrıntı görünümünün karşılık gelen alt verilerle güncelleştirilmesine neden olur.

Bu izlenecek yolda ekran görüntüleri ve kod listeleri Visual Studio 2013 alınmıştır ancak Visual Studio 2012 veya Visual Studio 2010 ile bu yönergeyi tamamlayabilirsiniz.

## <a name="pre-requisites"></a>Önkoşulların önkoşulları

Bu izlenecek yolu tamamlamak için Visual Studio 2013, Visual Studio 2012 veya Visual Studio 2010 yüklü olmalıdır.

Visual Studio 2010 kullanıyorsanız, NuGet ' i de yüklemelisiniz. Daha fazla bilgi için bkz. [NuGet 'ı yükleme](https://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Uygulamayı oluşturma

-   Visual Studio 'Yu aç
-   **Dosya-&gt; yeni&gt; projesi....**
-   Sağ bölmedeki sol bölmedeki ve **Windows FormsApplication** penceresinde **Windows** ' u seçin
-   Ad olarak **Winformyüzthefsample** girin
-   **Tamam 'ı** seçin

## <a name="install-the-entity-framework-nuget-package"></a>Entity Framework NuGet paketini yükler

-   Çözüm Gezgini, **Winformyüzthefsample** projesine sağ tıklayın
-   **NuGet Paketlerini Yönet...** seçeneğini belirleyin.
-   NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin
-   **Install** 'a tıklayın  
    > [!NOTE]
    > EntityFramework derlemesine ek olarak, System. ComponentModel. Dataaçıklamalarda bir başvuru de eklenir. Projenin System. Data. Entity başvurusu varsa, EntityFramework paketi yüklendiğinde kaldırılır. System. Data. Entity derlemesi artık Entity Framework 6 uygulamaları için kullanılmıyor.

## <a name="implementing-ilistsource-for-collections"></a>Koleksiyonlar için ISource uygulama

Windows Forms kullanılırken sıralama ile iki yönlü veri bağlamayı etkinleştirmek için koleksiyon özelliklerinin ISource arabirimini uygulaması gerekir. Bunu yapmak için ObservableCollection ISource işlevselliği eklemek üzere genişletireceğiz.

-   Projeye bir **ObservableListSource** sınıfı ekleyin:
    -   Proje adına sağ tıklayın
    -   **&gt; yeni öğe Ekle** ' yi seçin
    -   Sınıf adı için **Class** ' ı seçin ve **ObservableListSource** girin
-   Varsayılan olarak oluşturulan kodu aşağıdaki kodla değiştirin:

*Bu sınıf iki yönlü veri bağlamayı ve sıralamayı mümkün kılmaktadır. Sınıf, ObservableCollection&lt;T&gt; öğesinden türetilir ve ISource 'un açık bir uygulamasını ekler. ISource 'un GetList () yöntemi, ObservableCollection ile eşitlenmiş bir IBindingList uygulamasını döndürecek şekilde uygulanır. ToBindingList tarafından oluşturulan IBindingList uygulamasının sıralamayı destekler. ToBindingList Extension yöntemi EntityFramework derlemesinde tanımlanmıştır.*

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

## <a name="define-a-model"></a>Model tanımlama

Bu izlenecek yolda, Code First veya EF tasarımcısını kullanarak bir model uygulamayı seçebilirsiniz. Aşağıdaki iki bölümden birini doldurun.

### <a name="option-1-define-a-model-using-code-first"></a>Seçenek 1: Code First kullanarak model tanımlama

Bu bölümde, Code First kullanarak bir modelin ve ilişkili veritabanının nasıl oluşturulacağı gösterilmektedir. Bir sonraki bölüme atlayın (**seçenek 2: Database First kullanarak bir model tanımlayın)** , bir VERITABANıNDAN, EF Designer kullanarak modelinize ters mühendislik uygulamak için Database First kullanmayı tercih ediyorsanız

Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız.

-   Projeye yeni bir **ürün** sınıfı ekleyin
-   Varsayılan olarak oluşturulan kodu aşağıdaki kodla değiştirin:

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

-   Projeye bir **Kategori** sınıfı ekleyin.
-   Varsayılan olarak oluşturulan kodu aşağıdaki kodla değiştirin:

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

Varlıkları tanımlamaya ek olarak, **DbContext** 'ten türeten bir sınıf tanımlamanız ve **Dbset&lt;TEntity&gt;** özelliklerini kullanıma sunabilmeniz gerekir. **Dbset** özellikleri bağlamın modele hangi türleri dahil etmek istediğinizi bilmesini sağlar. **DbContext** ve **Dbset** türleri EntityFramework derlemesinde tanımlanmıştır.

DbContext türetilmiş türünün bir örneği, çalışma zamanı sırasında varlık nesnelerini yönetir, bu da nesneleri bir veritabanındaki verilerle doldurmayı, değişiklik izlemeyi ve kalıcı verileri veritabanına getirmeyi içerir.

-   Projeye yeni bir **Productcontext** sınıfı ekleyin.
-   Varsayılan olarak oluşturulan kodu aşağıdaki kodla değiştirin:

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

    ![Veritabanı Oluştur](~/ef6/media/createdatabase.png)

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

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

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

EF, modelden T4 şablonları kullanarak kod oluşturur. Visual Studio ile birlikte gelen veya Visual Studio galerisinden indirilen şablonlar genel amaçlı kullanım amaçlıdır. Bu, bu şablonlardan oluşturulan varlıkların basit ICollection&lt;T&gt; özelliklerine sahip olduğu anlamına gelir. Ancak, veri bağlama yaparken ISource 'u uygulayan koleksiyon özelliklerinin olması tercih edilir. Bu nedenle, yukarıdaki ObservableListSource sınıfını oluşturuyoruz ve artık bu sınıftan kullanımı sağlamak için şablonları değiştirebiliyoruz.

-   **Çözüm Gezgini** açın ve **ProductModel. edmx** dosyasını bulun
-   ProductModel. edmx dosyasının altında iç içe olacak **ProductModel.tt** dosyasını bulun

    ![Ürün modeli şablonu](~/ef6/media/productmodeltemplate.png)

-   Visual Studio Düzenleyicisi 'nde açmak için ProductModel.tt dosyasına çift tıklayın
-   "**ICollection**" sözcüğünün iki yinelemesini "**ObservableListSource**" ile bulur ve değiştirin. Bunlar yaklaşık olarak 296 ve 484 satırlarında bulunur.
-   İlk "**HashSet**" oluşumunu "**ObservableListSource**" ile bulur ve değiştirin. Bu oluşum yaklaşık satır 50 ' de bulunur. Kodda daha sonra bulunan ikinci diyez kümesi **oluşumunu değiştirmeyin.**
-   ProductModel.tt dosyasını kaydedin. Bu, varlıkların kodunun yeniden üretilmesine neden olmalıdır. Kod otomatik olarak yeniden oluşturmaz, ProductModel.tt 'e sağ tıklayın ve "özel araç Çalıştır" seçeneğini belirleyin.

Artık Category.cs dosyasını (ProductModel.tt altında iç içe yerleştirilmiş) açarsanız, Products koleksiyonunun **ObservableListSource&lt;ürün&gt;** türünde olduğunu görmeniz gerekir.

Projeyi derleyin.

## <a name="lazy-loading"></a>Geç yükleme

**Product** sınıfında **category** sınıfı ve **category** özelliğindeki **Products** özelliği gezinti özellikleridir. Entity Framework, gezinti özellikleri iki varlık türü arasında bir ilişkiye gitmek için bir yol sağlar.

EF, gezinti özelliğine ilk kez eriştiğinizde ilgili varlıkları veritabanından otomatik olarak yükleme seçeneği sunar. Bu yükleme türü (yavaş yükleme olarak adlandırılır) ile, her gezinti özelliğine ilk kez eriştiğinizde, içerik bağlamda değilse veritabanına karşı ayrı bir sorgu yürütülecektir.

POCO varlık türleri kullanılırken, EF, çalışma zamanı sırasında türetilmiş proxy türlerinin örneklerini oluşturarak ve sonra yükleme kancasını eklemek için sınıflarınızda sanal özellikleri geçersiz kılarak geç yüklemeye erişir. İlgili nesnelerin geç yüklemesini almak için, gezinti özelliği alıcıları ' ı **Public** ve **Virtual** (Visual Basic) olarak bildirmeniz ve sınıfınız **mühürlenmemelidir** **(Visual Basic** içinde**NotOverridable** ). Database First gezinti özelliklerinin kullanılması, yavaş yüklemeyi etkinleştirmek için otomatik olarak sanal hale getirilir. Code First bölümünde, aynı nedenden dolayı gezinti özelliklerini sanal hale getirmeyi seçtik

## <a name="bind-object-to-controls"></a>Nesneyi denetimlere bağlama

Bu WinForms uygulaması için modelde tanımlanan sınıfları veri kaynağı olarak ekleyin.

-   Ana menüden **Proje-&gt; yeni veri kaynağı Ekle...** öğesini seçin.
    (Visual Studio 2010 ' de, **veri&gt; yeni veri kaynağı Ekle...** ' i seçmeniz gerekir.)
-   Veri kaynağı türü seçin penceresinde, **nesne** ' yi seçin ve **İleri** ' ye tıklayın.
-   Veri nesnelerini seçin iletişim kutusunda, **Winformyüzthefsample** 'ın iki kez katlamayı kaldırın ve **Kategori** ' yi seçin. Bu, ürün veri kaynağını seçmek zorunda kalmaz.

    ![Veri Kaynağı](~/ef6/media/datasource.png)

-   Son ' a tıklayın **.**
    Veri kaynakları penceresi gösterilmiyorsa, **diğer Windows&gt; veri kaynaklarını&gt; görüntüle** ' yi seçin.
-   Veri kaynakları penceresi otomatik olarak gizlenmemesi için Sabitle simgesine basın. Pencere zaten görünür durumdaysa Yenile düğmesine vurması gerekebilir.

    ![Veri kaynağı 2](~/ef6/media/datasource2.png)

-   Çözüm Gezgini, tasarımcıda ana formu açmak için **Form1.cs** dosyasına çift tıklayın.
-   **Kategori** veri kaynağını seçin ve form üzerine sürükleyin. Varsayılan olarak, tasarımcıya yeni bir DataGridView (**Categorydatagridview**) ve gezinti araç çubuğu denetimleri eklenir. Bu denetimler, ayrıca oluşturulan BindingSource (**Categorybindingsource**) ve bağlama Gezgini (**categorybindingnavigator**) bileşenlerine bağımlıdır.
-   **Categorydatagridview**üzerinde sütunları düzenleyin. **CategoryID** sütununu salt okunurdur olarak ayarlamak istiyoruz. Verileri kaydettikten sonra **CategoryID** özelliğinin değeri veritabanı tarafından oluşturulur.
    -   DataGridView denetimine sağ tıklayıp sütunları Düzenle... seçeneğini belirleyin.
    -   CategoryID sütununu seçin ve ReadOnly değerini true olarak ayarlayın
    -   Tamam 'a basın
-   Kategori veri kaynağı altında products ' ı seçin ve forma sürükleyin. ProductDataGridView ve productBindingSource forma eklenir.
-   ProductDataGridView üzerinde sütunları düzenleyin. CategoryID ve category sütunlarını gizlemek ve ProductID 'yi salt okunurdur olarak ayarlamak istiyoruz. Verileri kaydettikten sonra ProductID özelliğinin değeri veritabanı tarafından oluşturulur.
    -   DataGridView denetimine sağ tıklayıp **Sütunları Düzenle...** seçeneğini belirleyin.
    -   **ProductID** sütununu seçin ve **ReadOnly** **değerini true**olarak ayarlayın.
    -   **CategoryID** sütununu seçin ve **Kaldır** düğmesine basın. **Kategori** sütunuyla aynısını yapın.
    -   Tuşuna **Tamam**.

    Şimdiye kadar, tasarımcı 'daki BindingSource bileşenleriyle DataGridView denetimlerimizi ilişkilendirdik. Sonraki bölümde, categoryBindingSource. DataSource öğesini DbContext tarafından izlenmekte olan varlıkların koleksiyonuna ayarlamak için arkasındaki koda kod ekleyeceğiz. Kategori altındaki ürünleri sürüklediğimiz ve bıraktığı zaman, WinForms, productsBindingSource. DataSource özelliğini Products BindingSource ve productsBindingSource. DataMember özelliğine ayarlamamız gerekir. Bu bağlama nedeniyle, yalnızca şu anda seçili olan kategoriye ait ürünler productDataGridView içinde görüntülenir.
-   Sağ fare düğmesine tıklayıp **etkin**' i seçerek gezinti araç çubuğundaki **Kaydet** düğmesini etkinleştirin.

    ![Form 1 Tasarımcısı](~/ef6/media/form1-designer.png)

-   Düğmeye çift tıklayarak Kaydet düğmesine ait olay işleyicisini ekleyin. Bu işlem olay işleyicisini ekler ve sizi form için arka plan koduna getirir. **Categorybindinggezgintorsaveıtem\_Click** olay işleyicisi bir sonraki bölüme eklenecektir.

## <a name="add-the-code-that-handles-data-interaction"></a>Veri etkileşimini Işleyen kodu ekleyin

Artık veri erişimi gerçekleştirmek için ProductContext kullanmak üzere kodu ekleyeceğiz. Ana form penceresi için kodu aşağıda gösterildiği gibi güncelleştirin.

Kod, ProductContext 'in uzun süre çalışan bir örneğini bildirir. Verileri sorgulamak ve veritabanına kaydetmek için ProductContext nesnesi kullanılır. ProductContext örneğindeki Dispose () yöntemi, geçersiz kılınan OnClosing yönteminden çağırılır. Kod açıklamaları kodun ne yaptığını hakkında ayrıntılar sağlar.

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

-   Uygulamayı derleyin ve çalıştırın ve işlevleri test edebilirsiniz.

    ![Kaydetmeden önce form 1](~/ef6/media/form1beforesave.png)

-   Mağaza oluşturulduktan sonra oluşturulan anahtarların ekranda gösterilmesi gösterilmektedir.

    ![Kaydettikten sonra 1 formu](~/ef6/media/form1aftersave.png)

-   Code First kullandıysanız, sizin için bir **Winformyüzthefsample. ProductContext** veritabanının oluşturulduğunu da görürsünüz.

    ![Sunucu Nesne Gezgini](~/ef6/media/serverobjexplorer.png)
