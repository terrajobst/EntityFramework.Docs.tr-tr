---
title: EF6 öncelikle - veritabanı
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: b499dea02cbeaa64f6ef87bf89cc739110c8b560
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490928"
---
# <a name="database-first"></a>İlk veritabanı
Bu video ve adım adım izlenecek yol, Entity Framework kullanarak veritabanı First geliştirmeye giriş sağlar. Veritabanı ilk ters mühendislik varolan bir veritabanını modelden sağlar. Model bir EDMX dosyası (.edmx uzantısı) depolanır ve görüntülenebilir ve Entity Framework Tasarımcısı'nda düzenlenebilir. Uygulamanıza etkileşim sınıfları EDMX dosyasından otomatik olarak oluşturulur.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda, Entity Framework kullanarak veritabanı ilk geliştirme tanıtılmaktadır. Veritabanı ilk ters mühendislik varolan bir veritabanını modelden sağlar. Model bir EDMX dosyası (.edmx uzantısı) depolanır ve görüntülenebilir ve Entity Framework Tasarımcısı'nda düzenlenebilir. Uygulamanıza etkileşim sınıfları EDMX dosyasından otomatik olarak oluşturulur.

**Tarafından sunulan**: [Rowan Miller](http://romiller.com/)

**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Ön koşullar

Bu izlenecek yolu tamamlamak için Visual Studio 2012 yüklü veya en az Visual Studio 2010 olması gerekir.

Visual Studio 2010 kullanıyorsanız, aynı zamanda sahip gerekecektir [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklü.

 

## <a name="1-create-an-existing-database"></a>1. Mevcut bir veritabanı oluşturun

Genellikle zaten oluşturulur varolan bir veritabanını hedeflerken, ancak bu kılavuz erişmek için bir veritabanı oluşturmak ihtiyacımız var.

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:

-   Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.
-   Visual Studio 2012 kullanıyorsanız sonra, oluşturduğunuz bir [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) veritabanı.

 

Yeni bir ubuntu ve veritabanı oluşturun.

-   Visual Studio'yu Aç
-   **Görünüm -&gt; Sunucu Gezgini**
-   Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**
-   Sunucu gezgininden veritabanına bağlamadıysanız önce Microsoft SQL Server veri kaynağı olarak seçmeniz gerekir

    ![Veri kaynağını seçin](~/ef6/media/selectdatasource.png)

-   LocalDB veya hangisinin bağlı olarak, yüklü, SQL Express için bağlanın ve girin **DatabaseFirst.Blogging** veritabanı adı

    ![SQL Express bağlantısı SD](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB bağlantı SD](~/ef6/media/localdbconnectiondf.png)

-   Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**

    ![Veritabanı iletişim kutusu oluşturma](~/ef6/media/createdatabasedialog.png)

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
```

## <a name="2-create-the-application"></a>2. Uygulama oluşturma

Örneği basit tutmak için veri erişimi gerçekleştirdiği ilk veritabanı kullanan temel bir konsol uygulaması oluşturmak için oluşturacağız:

-   Visual Studio'yu Aç
-   **Dosya -&gt; yeni -&gt; proje...**
-   Seçin **Windows** sol menüden ve **konsol uygulaması**
-   Girin **DatabaseFirstSample** adı
-   Seçin **Tamam**

 

## <a name="3-reverse-engineer-model"></a>3. Ters mühendislik modeli

Modelimiz oluşturmak için Entity Framework Visual Studio'nun bir parçası olarak dahil olan Designer'ın, kullanan oluşturacağız.

-   **Takım projesi -&gt; Yeni Öğe Ekle...**
-   Seçin **veri** sol menüden ardından **ADO.NET varlık veri modeli**
-   Girin **BloggingModel** tıklayın ve adı olarak **Tamam**
-   Böylece **varlık veri modeli Sihirbazı**
-   Seçin **veritabanından Oluştur** tıklatıp **İleri**

    ![Adım 1'in Sihirbazı](~/ef6/media/wizardstep1.png)

-   İlk bölümde oluşturduğunuz veritabanı bağlantısı seçin, girin **BloggingContext** tıklayın ve bağlantı dizesi adı olarak **İleri**

    ![Sihirbazı adımı 2](~/ef6/media/wizardstep2.png)

-   'Tüm tabloları Al ve 'Son' tablolar' yanındaki onay kutusuna tıklayın.

    ![Sihirbazı adımı 3](~/ef6/media/wizardstep3.png)

 

Ters mühendislik işlemi tamamlandıktan sonra yeni modeli projenize eklenir ve Entity Framework Tasarımcısı'nda görüntülemeniz için açıldı. Bir App.config dosyası ayrıca veritabanı bağlantı ayrıntıları ile projenize eklendi.

![İlk model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010'daki ek adımlar

Visual Studio 2010'da çalışıyorsanız Entity Framework'ün en son sürüme yükseltmek için izlemeniz gereken bazı ek adımlar vardır. Yükseltme önemlidir çünkü en son hata düzeltmeleri yanı sıra kullanmak çok daha kolaydır, geliştirilmiş bir API yüzeyi, erişmenizi sağlar.

Öncelikle, Nuget'ten Entity Framework'ün en son sürümünü almak ihtiyacımız var.

-   **Proje –&gt; NuGet paketlerini Yönet... ** 
     *Yoksa **NuGet paketlerini Yönet... ** yüklemelisiniz seçeneği [en son NuGet sürümünü](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Seçin **çevrimiçi** sekmesi
-   Seçin **EntityFramework** paket
-   Tıklayın **yükleyin**

Ardından, Entity Framework'ün sonraki sürümlerinde sunulan DbContext API kullanır kod üretmek için modelimizi takas etmek ihtiyacımız var.

-   Modelinizin EF Tasarımcısı'nda boş bir nokta üzerinde sağ tıklayıp **kod oluşturma öğesi Ekle...**
-   Seçin **çevrimiçi şablonlar** arayın ve sol menüden **DbContext**
-   EF seçin **5.x DbContext Oluşturucu c\#**, girin **BloggingModel** tıklayın ve adı olarak **Ekle**

    ![DbContext şablonu](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Okuma ve yazma veri

Biz bir modeliniz olduğuna göre bazı verilere erişmek için kullanılacak zaman var. Sınıfları kullanacağız EDMX dosyasını temel alan için kullanılacak erişim verilerini otomatik olarak oluşturuluyor.

*Visual Studio 2010 kullanıyorsanız bu ekran görüntüsü, Visual Studio 2012'den BloggingModel.tt olduğu ve BloggingModel.Context.tt dosyalarını doğrudan projenize altında yerine EDMX dosyasının altında iç içe geçmiş.*

![Oluşturulan sınıflar SD](~/ef6/media/generatedclassesdf.png)

 

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
 

## <a name="5-dealing-with-database-changes"></a>5. Veritabanı değişikliklerini uğraşmanızı

Artık biz de modelimiz, bu değişiklikleri yansıtacak şekilde güncelleştirilecek ihtiyacımız bu değişiklik yaptığınız zaman, bizim veritabanı şeması, bazı değişiklikler yapmanız gerekebileceğini zamanı geldi.

Veritabanı şemasına bazı değişiklikler yapmanız gerekebileceğini ilk adımdır bakın. Bir kullanıcı tablosu için şema eklemek için ekleyeceğiz.

-   Sağ **DatabaseFirst.Blogging** seçin ve veritabanı sunucu Gezgini'nde **yeni sorgu**
-   Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Şema güncelleştirildi, modeli bu değişikliklerle güncelleştirmek için zaman var.

-   Boş bir nokta modelinize EF Designer ve Seç 'güncelleştirme modeli veritabanından...' na sağ tıklayın, bu güncelleştirme Sihirbazı başlatılır
-   Güncelleştirme Sihirbazı onay tabloları yanındaki kutuyu Ekle sekmesinde bu şemadan herhangi bir yeni tablolar eklemek istediğimiz gösterir.
    *Yenileme sekmesi, güncelleştirme sırasında değişiklikleri için denetlenecek modelindeki mevcut tabloların gösterir. Delete sekmeler şemadan kaldırıldı ve modelden güncelleştirmenin bir parçası da kaldırılacak herhangi bir tablo gösterir. Bu iki sekmelerindeki bilgileri otomatik olarak algılanır ve yalnızca bilgilendirme amacıyla sağlanmıştır, herhangi bir ayarı değiştiremezsiniz.*

    ![Yenileme Sihirbazı](~/ef6/media/refreshwizard.png)

-   Güncelleştirme Sihirbazı'nı Son'a tıklayın

 

Model veritabanına ekledik kullanıcılar tablo eşleşen yeni bir kullanıcı varlığı eklemek için güncelleştirilmiştir.

![Güncelleştirilmiş model](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Özet

Bu kılavuzda ilk veritabanı geliştirme incelemiştik olduğu EF tasarımcıda varolan bir veritabanını temel alan bir model oluşturmak olduk. Ardından bu modelin bazı verileri veritabanından okuyup kullandık. Son olarak, model veritabanı şemasına yaptık değişiklikleri yansıtacak şekilde güncelleştirdik.
