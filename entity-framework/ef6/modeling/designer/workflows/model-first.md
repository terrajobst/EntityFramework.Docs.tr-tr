---
title: Model First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418107"
---
# <a name="model-first"></a>Model First
Bu video ve adım adım yönergeler, Entity Framework kullanarak Model First geliştirmeye yönelik bir giriş sağlar. Model First, Entity Framework Designer kullanarak yeni bir model oluşturmanızı ve sonra modelden bir veritabanı şeması oluşturmanızı sağlar. Model bir EDMX dosyasında (. edmx uzantılı) depolanır ve Entity Framework Designer görüntülenebilir ve düzenlenebilir. Uygulamanızda etkileşimde bulunan sınıflar, EDMX dosyasından otomatik olarak oluşturulur.

## <a name="watch-the-video"></a>Videoyu izleme
Bu video ve adım adım yönergeler, Entity Framework kullanarak Model First geliştirmeye yönelik bir giriş sağlar. Model First, Entity Framework Designer kullanarak yeni bir model oluşturmanızı ve sonra modelden bir veritabanı şeması oluşturmanızı sağlar. Model bir EDMX dosyasında (. edmx uzantılı) depolanır ve Entity Framework Designer görüntülenebilir ve düzenlenebilir. Uygulamanızda etkileşimde bulunan sınıflar, EDMX dosyasından otomatik olarak oluşturulur.

**Sunulma ölçütü**: [Rowa Miller](https://romiller.com/)

**Video**: [wmv](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için Visual Studio 2010 veya Visual Studio 2012 yüklü olmalıdır.

Visual Studio 2010 kullanıyorsanız, [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) ' in yüklü olması gerekir.

## <a name="1-create-the-application"></a>1. uygulamayı oluşturun

Şeyleri basit tutmak için, veri erişimi gerçekleştirmek üzere Model First kullanan temel bir konsol uygulaması oluşturacağız:

-   Visual Studio’yu açın
-   **Dosya-&gt; yeni&gt; projesi...**
-   Sol taraftaki menüden ve **konsol uygulamasından** **Windows** ' u seçin
-   Ad olarak **Modelfirstsample** girin
-   **Tamam**’ı seçin

## <a name="2-create-model"></a>2. model oluştur

Modelimizi oluşturmak için Visual Studio 'nun bir parçası olarak dahil edilen Entity Framework Designer kullanacağız.

-   **Proje-&gt; yeni öğe Ekle...**
-   Sol menüden **verileri** seçin ve ardından **ADO.net varlık veri modeli**
-   Ad olarak **BloggingModel** girin ve **Tamam**' a tıkladıktan sonra varlık veri modeli Sihirbazı başlatılır
-   **Boş model** ' i seçin ve **son** ' a tıklayın

    ![Boş model oluştur](~/ef6/media/createemptymodel.png)

Entity Framework Designer boş bir modelle açılır. Artık modele varlıklar, Özellikler ve ilişkilendirmeler eklemeye başlayabiliriz.

-   Tasarım yüzeyine sağ tıklayıp **Özellikler** ' i seçin
-   Özellikler penceresi **varlık kapsayıcısı adını** **BloggingContext** olarak değiştirin
    *Bu, sizin için oluşturulacak türetilmiş bağlamın adıdır; bağlam veritabanı ile bir oturumu temsil eder ve verileri sorgulayıp kaydetmenize olanak tanır*
-   Tasarım yüzeyine sağ tıklayıp **yeni&gt; varlık Ekle ' yi seçin...**
-   Varlık adı ve **blogID** olarak anahtar adı olarak blog girin ve **Tamam** ' **a** tıklayın.

    ![Blog varlığı Ekle](~/ef6/media/addblogentity.png)

-   Tasarım yüzeyinde yeni varlığa sağ tıklayın ve **New-&gt; skaler Özellik Ekle**' yi seçin, özelliğin adı olarak **adı** girin.
-   **URL** özelliği eklemek için bu işlemi tekrarlayın.
-   Tasarım yüzeyinde **URL** özelliği ' ne sağ tıklayın ve **Özellikler**' i seçin Özellikler penceresi **null yapılabilir** ayarını **doğru** olarak değiştirin
    bu, *bir blogu URL atamadan veritabanına kaydetmenizi sağlar*
-   Yeni öğrenmeniz gereken teknikleri kullanarak **postid** anahtar özelliğine sahip bir **Post** varlığı ekleyin
-   **Post** varlığına **başlık** ve **içerik** skaler özellikleri ekleyin

Artık birkaç varlık olduğuna göre, aralarında bir ilişki (veya ilişki) ekleme zamanı.

-   Tasarım yüzeyine sağ tıklayıp **yeni&gt; Ilişkilendirmesi Ekle ' yi seçin...**
-   İlişki noktasının bir bitişini bir **tane** **birçok**
    çokluğa sahip olacak şekilde **bloga** ve diğer uç nokta ile blog olarak **gönderin** . *Bu, blogun birçok gönderiye sahip olduğu ve bir gönderiye ait olduğu anlamına gelir*
-   **' Post ' varlığına yabancı anahtar özellikleri ekle** kutusunun işaretli olduğundan emin olun ve **Tamam** ' a tıklayın.

    ![Ilişki ekleme MF](~/ef6/media/addassociationmf.png)

Artık, bir veritabanı oluşturabileceğiniz ve veri okumak ve yazmak için kullanabileceğiniz basit bir modelimiz vardır.

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

## <a name="3-generating-the-database"></a>3. veritabanı oluşturuluyor

Modelimiz verildiğinde, Entity Framework modeli kullanarak veri depolamanıza ve almasına imkan tanıyan bir veritabanı şemasını hesaplayabilirler.

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:

-   Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.
-   Visual Studio 2012 kullanıyorsanız, [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) veritabanı oluşturursunuz.

Şimdi veritabanını oluşturalım.

-   Tasarım yüzeyine sağ tıklayın ve **modelden veritabanı oluştur..** . seçeneğini belirleyin.
-   Yeni bağlantı ' ya tıklayın **...** ve hangi Visual Studio sürümüne bağlı olarak, LocalDB ya da SQL Express 'i belirtin, veritabanı adı olarak **Modelfirst. blog** girin.

    ![LocalDB bağlantısı MF](~/ef6/media/localdbconnectionmf.png)

    ![SQL Express bağlantı MF](~/ef6/media/sqlexpressconnectionmf.png)

-   **Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.
-   **İleri ' yi** seçin ve Entity Framework Designer veritabanı şemasını oluşturmak için bir komut dosyası hesaplar
-   Betik görüntülenirken **son** ' a tıklayın ve betik projenize eklenir ve açılır
-   Komut dosyasına sağ tıklayın ve **Yürüt**' ü seçin, hangi Visual Studio sürümüne bağlı olarak ' ye bağlanacak veritabanını belirtmeniz, LocalDB veya SQL Server Express belirtmeniz istenecektir

## <a name="4-reading--writing-data"></a>4. verileri okuma & yazma

Artık bir modelimiz olduğuna göre, bazı verilere erişmek için bunu kullanmanın zamanı. Verilere erişmek için kullanacağınız sınıflar, EDMX dosyasına bağlı olarak sizin için otomatik olarak oluşturulur.

*Bu ekran görüntüsü Visual Studio 2012 ' den, Visual Studio 2010 kullanıyorsanız BloggingModel.tt ve BloggingModel.Context.tt dosyaları, EDMX dosyasının altında iç içe değil doğrudan projenizin altında olacaktır.*

![Oluşturulan sınıflar](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. model değişiklikleriyle ilgilenme

Artık modelinizde bazı değişiklikler yapma zamanı, bu değişiklikleri yaptığımız için de veritabanı şemasını güncelleştirmemiz gerekiyor.

Modelinize yeni bir kullanıcı varlığı ekleyerek başlayacağız.

-   Anahtarın Özellik türü olarak anahtar adı ve **dize** olarak **Kullanıcı** adı ile yeni bir **Kullanıcı** varlık adı ekleyin

    ![Kullanıcı varlığı Ekle](~/ef6/media/adduserentity.png)

-   Tasarım yüzeyinde **Kullanıcı adı** özelliğine sağ tıklayın ve **Özellikler**' i seçin Özellikler penceresi **MaxLength** ayarını **50** olarak değiştirin
    *Bu, Kullanıcı adı 'nda depolanabilecek verileri 50 karaktere kısıtlar*
-   **Kullanıcı** varlığına **DisplayName** skalar özelliği ekleyin

Şimdi güncelleştirilmiş bir modelimiz var ve veritabanını yeni Kullanıcı varlık türü ile uyumlu olacak şekilde güncelleştirmeye hazırsınız.

-   Tasarım yüzeyine sağ tıklayın ve **modelden veritabanı oluştur...** ' u seçin, Entity Framework güncelleştirilmiş modele göre bir şemayı yeniden oluşturmak için bir komut dosyası hesaplar.
-   **Son**’a tıklayın
-   Var olan DDL betiğinin üzerine yazma ve modelin eşleme ve depolama bölümlerinin yazılmasına ilişkin uyarılar alabilirsiniz, her iki uyarı için de **Evet** ' e tıklayabilirsiniz
-   Veritabanını oluşturmak için güncelleştirilmiş SQL betiği sizin için açıldı  
    *Oluşturulan komut dosyası tüm mevcut tabloları bırakacak ve sonra şemayı sıfırdan yeniden oluşturacak. Bu, yerel geliştirme için çalışabilir, ancak daha önce dağıtılmış bir veritabanına yapılan değişiklikleri göndermek için uygun değildir. Zaten dağıtılmış bir veritabanında değişiklikler yayımlamanız gerekiyorsa, bir geçiş betiği hesaplamak için betiği düzenlemeniz veya bir şema karşılaştırma aracı kullanmanız gerekir.*
-   Komut dosyasına sağ tıklayın ve **Yürüt**' ü seçin, hangi Visual Studio sürümüne bağlı olarak ' ye bağlanacak veritabanını belirtmeniz, LocalDB veya SQL Server Express belirtmeniz istenecektir

## <a name="summary"></a>Özet

Bu kılavuzda, EF tasarımcısında bir model oluşturmamız ve ardından bu modelden bir veritabanı oluşturması için Model First geliştirmeye baktık. Daha sonra, veritabanından bazı verileri okumak ve yazmak için modeli kullandık. Son olarak, modeli güncelleştirdik ve sonra modeliyle eşleşecek şekilde veritabanı şemasını yeniden oluşturacağız.
