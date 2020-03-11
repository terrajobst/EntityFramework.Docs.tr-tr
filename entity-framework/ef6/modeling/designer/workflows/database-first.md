---
title: Database First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418359"
---
# <a name="database-first"></a>Database First
Bu video ve adım adım yönergeler, Entity Framework kullanarak Database First geliştirmeye yönelik bir giriş sağlar. Database First, varolan bir veritabanından bir modele ters mühendislik yapmanıza olanak sağlar. Model bir EDMX dosyasında (. edmx uzantılı) depolanır ve Entity Framework Designer görüntülenebilir ve düzenlenebilir. Uygulamanızda etkileşimde bulunan sınıflar, EDMX dosyasından otomatik olarak oluşturulur.

## <a name="watch-the-video"></a>Videoyu izleme
Bu videoda Entity Framework kullanarak Database First geliştirmeye bir giriş sunulmaktadır. Database First, varolan bir veritabanından bir modele ters mühendislik yapmanıza olanak sağlar. Model bir EDMX dosyasında (. edmx uzantılı) depolanır ve Entity Framework Designer görüntülenebilir ve düzenlenebilir. Uygulamanızda etkileşimde bulunan sınıflar, EDMX dosyasından otomatik olarak oluşturulur.

**Sunulma ölçütü**: [Rowa Miller](https://romiller.com/)

**Video**: [wmv](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için en az Visual Studio 2010 veya Visual Studio 2012 yüklü olmalıdır.

Visual Studio 2010 kullanıyorsanız, [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) ' in yüklü olması gerekir.

 

## <a name="1-create-an-existing-database"></a>1. var olan bir veritabanını oluşturun

Genellikle, var olan bir veritabanını hedeflerken zaten oluşturulur, ancak bu izlenecek yol için, erişmek üzere bir veritabanı oluşturulması gerekir.

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:

-   Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.
-   Visual Studio 2012 kullanıyorsanız, [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) veritabanı oluşturursunuz.

 

Şimdi veritabanını oluşturalım.

-   Visual Studio’yu açın
-   **&gt; Sunucu Gezgini görüntüle**
-   Veri bağlantıları ' na sağ tıklayın **&gt; bağlantı ekle...**
-   Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak Microsoft SQL Server seçmeniz gerekir

    ![Veri Kaynağı Seç](~/ef6/media/selectdatasource.png)

-   Hangisini yüklediğinize bağlı olarak LocalDB 'ye veya SQL Express 'e bağlanın ve veritabanı adı olarak **Databasefirst. blog** yazın

    ![SQL Express bağlantı DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB bağlantısı DF](~/ef6/media/localdbconnectiondf.png)

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
```

## <a name="2-create-the-application"></a>2. uygulamayı oluşturun

Şeyleri basit tutmak için, veri erişimi gerçekleştirmek üzere Database First kullanan temel bir konsol uygulaması oluşturacağız:

-   Visual Studio’yu açın
-   **Dosya-&gt; yeni&gt; projesi...**
-   Sol taraftaki menüden ve **konsol uygulamasından** **Windows** ' u seçin
-   Ad olarak **Databasefirstsample** yazın
-   **Tamam**’ı seçin

 

## <a name="3-reverse-engineer-model"></a>3. tersine mühendislik modeli

Modelimizi oluşturmak için Visual Studio 'nun bir parçası olarak dahil edilen Entity Framework Designer kullanacağız.

-   **Proje-&gt; yeni öğe Ekle...**
-   Sol menüden **verileri** seçin ve ardından **ADO.net varlık veri modeli**
-   Ad olarak **BloggingModel** girin ve **Tamam 'a** tıklayın
-   Bu, **varlık veri modeli Sihirbazı 'nı** başlatır
-   **Veritabanından oluştur** ' u seçin ve **İleri** ' ye tıklayın.

    ![Sihirbaz 1. adım](~/ef6/media/wizardstep1.png)

-   İlk bölümde oluşturduğunuz veritabanına bağlantıyı seçin, bağlantı dizesinin adı olarak **BloggingContext** girin ve **İleri** ' ye tıklayın.

    ![Sihirbaz 2. adım](~/ef6/media/wizardstep2.png)

-   Tüm tabloları içeri aktarmak için ' tablolar ' seçeneğinin yanındaki onay kutusuna tıklayın ve ' son ' a tıklayın

    ![Sihirbaz 3. adım](~/ef6/media/wizardstep3.png)

 

Tersine mühendislik işlemi tamamlandıktan sonra, yeni model projenize eklenir ve Entity Framework Designer görüntülemeniz için açılır. Veritabanına yönelik bağlantı ayrıntılarına sahip bir App. config dosyası da projenize eklenmiştir.

![Ilk model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010 ' de ek adımlar

Visual Studio 2010 ' de çalışıyorsanız, en son Entity Framework sürümüne yükseltmek için izlemeniz gereken bazı ek adımlar vardır. ' Nin, kullanımı çok daha kolay olan gelişmiş bir API yüzeyine erişim sağladığından ve en son hata düzeltmelerinin yanı sıra yükseltme önemlidir.

İlk olarak, Entity Framework NuGet 'den en son sürümü alması gerekir.

-   **Proje –&gt; NuGet Paketlerini Yönet...** * **NuGet Paketlerini Yönet...** seçeneğine sahipseniz
    [NuGet 'in en son sürümünü](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklemelisiniz*
-   **Çevrimiçi** sekmesini seçin
-   **EntityFramework** paketini seçin
-   **Install** 'a tıklayın

Bundan sonra, Entity Framework sonraki sürümlerinde tanıtılan DbContext API 'sini kullanan kodu oluşturmak için modelimizi değiştirmemiz gerekiyor.

-   EF Designer 'daki modelinizin boş bir noktasına sağ tıklayıp **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.
-   Sol menüden **çevrimiçi şablonlar** ' ı seçin ve **DbContext** ' i arayın
-   **C\#IÇIN EF 5. x DbContext oluşturucusunu** seçin, ad olarak **BloggingModel** girin ve **Ekle** ' ye tıklayın.

    ![DbContext şablonu](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. verileri okuma & yazma

Artık bir modelimiz olduğuna göre, bazı verilere erişmek için bunu kullanmanın zamanı. Verilere erişmek için kullanacağınız sınıflar, EDMX dosyasına bağlı olarak sizin için otomatik olarak oluşturulur.

*Bu ekran görüntüsü Visual Studio 2012 ' den, Visual Studio 2010 kullanıyorsanız BloggingModel.tt ve BloggingModel.Context.tt dosyaları, EDMX dosyasının altında iç içe değil doğrudan projenizin altında olacaktır.*

![Oluşturulan sınıflar DF](~/ef6/media/generatedclassesdf.png)

 

Ana yöntemi aşağıda gösterildiği gibi Program.cs ' de uygulayın. Bu kod, bağlamımız yeni bir örnek oluşturur ve yeni bir blog eklemek için onu kullanır. Daha sonra, bir LINQ sorgusu kullanarak, veritabanındaki tüm blogları başlık sırasına göre sıralanmış olarak alır.

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
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a>5. veritabanı değişiklikleriyle ilgilenme

Artık veritabanı şemanızda bazı değişiklikler yapmanız zaman alabilir. bu değişiklikleri yaparken, bu değişiklikleri yansıtacak şekilde modelimizi de güncelleştirmeniz gerekir.

İlk adım, veritabanı şemasında bazı değişiklikler yapmak için kullanılır. Şemaya bir kullanıcı tablosu ekleyeceğiz.

-   Sunucu Gezgini ve **Yeni sorgu** ' yı seçin **.**
-   Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Artık şema güncellendiğinden, modelin bu değişikliklerle güncelleştirilmesi zaman atalım.

-   EF Designer 'daki modelinizin boş bir noktasına sağ tıklayın ve ' veritabanını güncelleştir... ' seçeneğini belirleyin, bu işlem güncelleştirme sihirbazını başlatır
-   Güncelleştirme sihirbazının Ekle sekmesinde tablolar ' ın yanındaki kutuyu işaretleyin, bundan sonra şemadan yeni tablolar eklemek istiyoruz.
    *Yenile sekmesi, modelde, güncelleştirme sırasında değişiklikler için denetlenecek tüm mevcut tabloları gösterir. Silme sekmeleri şemadan kaldırılan tüm tabloları gösterir ve güncelleştirmenin bir parçası olarak modelden de kaldırılır. Bu iki sekmede bulunan bilgiler otomatik olarak algılanır ve yalnızca bilgilendirme amacıyla sağlanır, hiçbir ayarı değiştiremezsiniz.*

    ![Sihirbazı Yenile](~/ef6/media/refreshwizard.png)

-   Güncelleştirme sihirbazında son ' a tıklayın

 

Model artık veritabanına eklediğimiz Kullanıcı tablosuyla eşleşen yeni bir kullanıcı varlığı içerecek şekilde güncelleştirilir.

![Model güncelleştirildi](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Özet

Bu kılavuzda, var olan bir veritabanını temel alan EF tasarımcısında bir model oluşturmamızı sağlayan Database First geliştirmeyi inceledik. Daha sonra bu modeli veritabanından bazı verileri okumak ve yazmak için kullandık. Son olarak, modeli veritabanı şemasında yaptığımız değişiklikleri yansıtacak şekilde güncelleştirdik.
