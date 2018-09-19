---
title: EF6 öncelikle - model
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: d429d5ea590b22c77f3f7f0bcfbd5dfc0a3e0049
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283881"
---
# <a name="model-first"></a>İlk model
Bu video ve adım adım izlenecek yol, Entity Framework kullanarak Model First geliştirmeye giriş sağlar. Model, Entity Framework Designer kullanarak yeni bir model oluşturmak ve ardından bir veritabanı şema modeli oluşturmak öncelikle sağlar. Model bir EDMX dosyası (.edmx uzantısı) depolanır ve görüntülenebilir ve Entity Framework Tasarımcısı'nda düzenlenebilir. Uygulamanıza etkileşim sınıfları EDMX dosyasından otomatik olarak oluşturulur.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu video ve adım adım izlenecek yol, Entity Framework kullanarak Model First geliştirmeye giriş sağlar. Model, Entity Framework Designer kullanarak yeni bir model oluşturmak ve ardından bir veritabanı şema modeli oluşturmak öncelikle sağlar. Model bir EDMX dosyası (.edmx uzantısı) depolanır ve görüntülenebilir ve Entity Framework Tasarımcısı'nda düzenlenebilir. Uygulamanıza etkileşim sınıfları EDMX dosyasından otomatik olarak oluşturulur.

**Tarafından sunulan**: [Rowan Miller](http://romiller.com/)

**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Ön koşullar

Bu izlenecek yolu tamamlamak için Visual Studio 2012 yüklü veya Visual Studio 2010 olması gerekir.

Visual Studio 2010 kullanıyorsanız, aynı zamanda sahip gerekecektir [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) yüklü.

## <a name="1-create-the-application"></a>1. Uygulama oluşturma

Örneği basit tutmak için veri erişimi gerçekleştirdiği ilk modeli kullanan bir temel bir konsol uygulamasının oluşturacağız:

-   Visual Studio'yu Aç
-   **Dosya -&gt; yeni -&gt; proje...**
-   Seçin **Windows** sol menüden ve **konsol uygulaması**
-   Girin **ModelFirstSample** adı
-   Seçin **Tamam**

## <a name="2-create-model"></a>2. Model oluşturma

Modelimiz oluşturmak için Entity Framework Visual Studio'nun bir parçası olarak dahil olan Designer'ın, kullanan oluşturacağız.

-   **Takım projesi -&gt; Yeni Öğe Ekle...**
-   Seçin **veri** sol menüden ardından **ADO.NET varlık veri modeli**
-   Girin **BloggingModel** tıklayın ve adı olarak **Tamam**, bu varlık veri modeli Sihirbazı başlatır
-   Seçin **boş Model** tıklatıp **son**

    ![Boş Model oluşturma](~/ef6/media/createemptymodel.png)

Entity Framework Designer boş bir model ile açılır. Artık modele varlıklar, özellikleri ve ilişkileri ekleme başlayabilirsiniz.

-   Tasarım yüzeyi ve select sağ **özellikleri**
-   Özellikler penceresinde değişiklik **varlık kapsayıcı adı** için **BloggingContext**
    *sizin için bağlam oluşturulacak türetilmiş bağlamı adıdır Sorgu ve veri kaydetmek bize izin vererek, veritabanı ile bir oturumu temsil eder*
-   Tasarım yüzeyi ve select sağ **Ekle - yeni&gt; varlık...**
-   Girin **Blog** varlık adı olarak ve **BlogId** tıklatın ve anahtar adını olarak **Tamam**

    ![Blog varlık ekleme](~/ef6/media/addblogentity.png)

-   Yeni varlık tasarım yüzeyi ve select üzerinde sağ tıklayın **Ekle - yeni&gt; skaler özelliği**, girin **adı** özelliğinin adı.
-   Eklemek için bu işlemi tekrarlamanız bir **Url** özelliği.
-   Sağ **Url** tasarım yüzeyi ve seçme özelliği **özellikleri**, Özellikler penceresinde değişiklik **Nullable** ayarını **True** 
     *Bu Blog, bir Url atamadan veritabanına kaydetme olanak tanır*
-   Yalnızca modellerin, teknikleri kullanarak bir **Post** varlıkla bir **PostId** anahtar özelliği
-   Ekleme **başlık** ve **içerik** skaler özelliklerine **Post** varlık

Birkaç varlık sahibiz, bunlar arasında bir ilişkilendirme (veya ilişki) ekleme zamanı geldi.

-   Tasarım yüzeyi ve select sağ **Ekle - yeni&gt; ilişki...**
-   İlişkinin bir ucu üzerine olun **Blog** çokluğunu ile **bir** ve diğer uç noktasına **Post** çokluğunu ile **birçok** 
     *Bu Blog birçok gönderileri ve bir gönderi bir Bloga ait olduğu anlamına gelir*
-   Olun **'Posta' varlığa yabancı anahtar özelliklerini eklemek** kutusunda denetlenir ve tıklayın **Tamam**

    ![İlişkilendirme MF ekleyin](~/ef6/media/addassociationmf.png)

Artık size bir veritabanından oluşturmak ve veri okuma ve yazma için kullanan basit bir model var.

![İlk model](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010'daki ek adımlar

Visual Studio 2010'da çalışıyorsanız Entity Framework'ün en son sürüme yükseltmek için izlemeniz gereken bazı ek adımlar vardır. Yükseltme önemlidir çünkü en son hata düzeltmeleri yanı sıra kullanmak çok daha kolaydır, geliştirilmiş bir API yüzeyi, erişmenizi sağlar.

Öncelikle, Nuget'ten Entity Framework'ün en son sürümünü almak ihtiyacımız var.

-   **Proje –&gt; NuGet paketlerini Yönet... ** 
     *Yoksa **NuGet paketlerini Yönet... ** yüklemelisiniz seçeneği [en son NuGet sürümünü](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Seçin **çevrimiçi** sekmesi
-   Seçin **EntityFramework** paket
-   Tıklayın **yükleyin**

Ardından, Entity Framework'ün sonraki sürümlerinde sunulan DbContext API kullanır kod üretmek için modelimizi takas etmek ihtiyacımız var.

-   Modelinizin EF Tasarımcısı'nda boş bir nokta üzerinde sağ tıklayıp **kod oluşturma öğesi Ekle...**
-   Seçin **çevrimiçi şablonlar** arayın ve sol menüden **DbContext**
-   EF seçin **5.x DbContext Oluşturucu c\#**, girin **BloggingModel** tıklayın ve adı olarak **Ekle**

    ![DbContext şablonu](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. Veritabanı oluşturma

Modelimiz göz önünde bulundurulduğunda, Entity Framework modelini kullanarak veri depolama ve alma için bize izin veren bir veritabanı şeması hesaplayabilirsiniz.

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:

-   Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.
-   Visual Studio 2012 kullanıyorsanız sonra, oluşturduğunuz bir [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) veritabanı.

Yeni bir ubuntu ve veritabanı oluşturun.

-   Tasarım yüzeyi ve select sağ **Model veritabanından oluştur...**
-   Tıklayın **yeni bağlantı...** LocalDB veya Visual Studio sürümüne bağlı olarak kullandığınız SQL Express belirtin girin **ModelFirst.Blogging** veritabanı adı.

    ![LocalDB bağlantı MF](~/ef6/media/localdbconnectionmf.png)

    ![SQL Express bağlantısı MF](~/ef6/media/sqlexpressconnectionmf.png)

-   Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**
-   Seçin **sonraki** ve Entity Framework Designer veritabanı şeması oluşturmak için bir betik hesaplar
-   Betik görüntülendikten sonra tıklayın **son** ve komut dosyası projenize eklenir ve açılır
-   Sağ tıklatın ve betik **yürütme**, LocalDB veritabanına bağlanmak belirtip için istenir veya SQL Server Express, kullanmakta olduğunuz Visual Studio sürümüne bağlı olarak

## <a name="4-reading--writing-data"></a>4. Okuma ve yazma veri

Biz bir modeliniz olduğuna göre bazı verilere erişmek için kullanılacak zaman var. Sınıfları kullanacağız EDMX dosyasını temel alan için kullanılacak erişim verilerini otomatik olarak oluşturuluyor.

*Visual Studio 2010 kullanıyorsanız bu ekran görüntüsü, Visual Studio 2012'den BloggingModel.tt olduğu ve BloggingModel.Context.tt dosyalarını doğrudan projenize altında yerine EDMX dosyasının altında iç içe geçmiş.*

![Oluşturulan sınıflar](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. Model değişikliklerle ilgilenme

Artık size ayrıca veritabanı şemasını güncelleştirmek için ihtiyacımız olan bu değişiklikler yaptığınızda modelimiz için bazı değişiklikler yapmanız gerekebileceğini zamanı geldi.

Modelimiz için yeni bir kullanıcı varlığı ekleyerek başlayacağız.

-   Yeni bir **kullanıcı** varlık adı ile **kullanıcıadı** anahtar adı olarak ve **dize** anahtar özellik türü olarak

    ![Kullanıcı varlığı ekleyin](~/ef6/media/adduserentity.png)

-   Sağ **kullanıcıadı** tasarım yüzeyi ve seçme özelliği **özellikleri**, Özellikler penceresinde değişiklik **MaxLength** ayarını **50 ** 
     *Bu kullanıcı adı 50 karakterden depolanabilir veri sınırlar*
-   Ekleme bir **DisplayName** skaler özelliğe **kullanıcı** varlık

Şimdi güncelleştirilmiş bir model vardır ve müşterilerimize yeni kullanıcının varlık türü uyum sağlayacak şekilde veritabanını güncelleştirmek hazırız.

-   Tasarım yüzeyi ve select sağ **Model veritabanından oluştur...** , Entity Framework, güncelleştirilmiş model temelinde bir şema yeniden oluşturmak için bir betik hesaplar.
-   Tıklayın **son**
-   Mevcut DDL betik ve model eşlemesi ve depolama bölümleri hakkında uyarılar alırsınız, tıklayın **Evet** hem bu uyarılar için
-   Veritabanını oluşturmak için güncelleştirilmiş SQL betiği açılır  
    *Oluşturulan kodun tüm varolan tablolardan bırakın ve şema sıfırdan yeniden. Bu, yerel geliştirme için çalışabilir ancak zaten dağıtılmış bir veritabanına değişiklikleri gönderme için uygulanabilir değil. Zaten dağıtılmış bir veritabanına değişiklikleri yayımlamak gerekiyorsa, betiği düzenleyin veya bir geçiş öncesinde bir betik hesaplamak için bir şema karşılaştırma aracını kullanmak gerekir.*
-   Sağ tıklatın ve betik **yürütme**, LocalDB veritabanına bağlanmak belirtip için istenir veya SQL Server Express, kullanmakta olduğunuz Visual Studio sürümüne bağlı olarak

## <a name="summary"></a>Özet

Model ilk geliştirme için incelemiştik bu kılavuzda, hangi EF Tasarımcısı'nda bir model oluşturmak ve ardından bir veritabanı o Modeli'nden olduk. Ardından model okuma ve bazı verileri veritabanından yazma kullandık. Son olarak, biz güncelleştirilmiş model ve veritabanı şeması modeli eşleşecek şekilde yeniden oluşturulur.
