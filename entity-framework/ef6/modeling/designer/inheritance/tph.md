---
title: Tasarımcı TPH devralma - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 1eb935414b20d6e93e9d470ccc845bc13626ed3a
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250850"
---
# <a name="designer-tph-inheritance"></a>Tasarımcı TPH devralma
Bu adım adım kılavuzda, Entity Framework Designer (EF Designer) ile kavramsal modeldeki tablo başına hiyerarşi (TPH) devralma uygulamak gösterilmektedir. TPH devralma, varlık türleri bir devralma hiyerarşisindeki tüm verileri korumak için bir veritabanı tablosu kullanır.

Bu izlenecek yolda biz kişi tablo üç varlık türü için eşler: kişi (temel türü), (türetilen kişiden) Öğrenci ve Eğitmen (türetilen kişiden). Biz kavramsal model veritabanı (veritabanı ilk) oluşturacak ve ardından EF Designer kullanarak TPH devralma uygulamak için modeli alter.

İlk modeli kullanarak bir TPH devralma eşlemek mümkündür, ancak karmaşık olan kendi veritabanı oluşturma iş akışı yazma etmesi gerekir. Bu iş akışını ardından atadığınız **veritabanı oluşturma iş akışı** EF Designer özelliği. Daha kolay bir alternatif, Code First kullanmaktır.

## <a name="other-inheritance-options"></a>Diğer devralma seçenekleri

Tablo başına tür (TPT), veritabanı ayrı tablolarda Devralmada rol oynayan varlıkları eşlenen devralma başka bir türdür.  Tablo başına tür devralma EF Designer ile eşlemek hakkında daha fazla bilgi için bkz: [EF Designer TPT devralma](~/ef6/modeling/designer/inheritance/tpt.md).

Tablo başına somut tür devralma (TPC) ve karma devralma modelleri Entity Framework çalışma zamanı tarafından desteklenir ancak EF tasarımcı tarafından desteklenmiyor. TPC veya karma devralma kullanmak istiyorsanız, iki seçeneğiniz vardır: Code First kullanın veya el ile EDMX dosyasını düzenleyin. EDMX ile çalışmayı tercih ederseniz eşleme Ayrıntıları penceresi "Güvenli Mod'da" yerleştirilir ve eşlemelerini değiştirmek için tasarımcı kullanmanız mümkün olmayacaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio'nun en son sürümü.
- [School örnek veritabanını](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projesi kurun

-   Visual Studio 2012'yi açın.
-   Seçin **File -&gt; yeni -&gt; proje**
-   Sol bölmede **Visual C\#** ve ardından **konsol** şablonu.
-   Girin **TPHDBFirstSample** adı.
-   Seçin **Tamam**.

## <a name="create-a-model"></a>Model oluşturma

-   Çözüm Gezgini'nde proje adına sağ tıklayın ve seçin **Ekle -&gt; yeni öğe**.
-   Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.
-   Girin **TPHModel.edmx** dosya adı ve ardından **Ekle**.
-   Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki**.
-   Tıklayın **yeni bağlantı**.
    Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.
    Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda, tablolar düğümünü seçin **kişi** tablo.
-   **Son**'a tıklayın.

Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir. Veritabanı nesnelerinizi seçin iletişim kutusunda seçilen tüm nesneler, modele eklenir.

Diğer bir deyişle nasıl **kişi** veritabanında tablo görünür.

![Kişi tablosu](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Tablo başına hiyerarşi devralma uygulama

**Kişi** tablolu **ayrıştırıcı** sütunu iki değerden birine sahip olabilir: "Öğrenci" ve "Eğitmen". Değerine bağlı olarak **kişi** tablo için eşleştirilir **Öğrenci** varlık veya **Eğitmen** varlık. **Kişi** tablo de iki sütun vardır **İşeAlmaTarihi** ve **EnrollmentDate**, olması gereken **boş değer atanabilir** bir kişi olamayacağı için bir Öğrenci ve aynı zamanda bir eğitmen (en az değil, bu izlenecek yol).

### <a name="add-new-entities"></a>Yeni varlık Ekle

-   Yeni varlık ekleyin.
    Bunu yapmak için Entity Framework Designer'ın tasarım yüzeyinde boş bir alanı sağ tıklatın ve seçin **Add -&gt;varlık**.
-   Tür **Eğitmen** için **varlık adı** seçip **kişi** için aşağı açılan listeden **temel türü**.
-   **Tamam**'ı tıklatın.
-   Başka bir yeni varlık ekleyin. Tür **Öğrenci** için **varlık adı** seçip **kişi** için aşağı açılan listeden **temel türü**.

İki yeni varlık türleri tasarım yüzeyine eklendi. Bir ok işaret yeni varlık türlerinden **kişi** varlık türü; gösterir **kişi** yeni varlık türleri için temel türdür.

-   Sağ **İşeAlmaTarihi** özelliği **kişi** varlık. Seçin **Kes** (veya Ctrl-X anahtarını kullanın).
-   Sağ **Eğitmen** varlık ve select **Yapıştır** (veya Ctrl-V anahtarı kullanın).
-   Sağ **İşeAlmaTarihi** özelliğini tıklatın ve **özellikleri**.
-   İçinde **özellikleri** penceresinde **Nullable** özelliğini **false**.
-   Sağ **EnrollmentDate** özelliği **kişi** varlık. Seçin **Kes** (veya Ctrl-X anahtarını kullanın).
-   Sağ **Öğrenci** varlık ve select **yapıştırın (veya anahtar kullan Ctrl-V).**
-   Seçin **EnrollmentDate** özelliği ve kümesi **Nullable** özelliğini **false**.
-   Seçin **kişi** varlık türü. İçinde **özellikleri** penceresinde kendi **soyut** özelliğini **true**.
-   Silme **ayrıştırıcı** özelliğinden **kişi**. Silinmesi nedeni aşağıdaki bölümde açıklanmıştır.

### <a name="map-the-entities"></a>Varlıkları Eşle

-   Sağ **Eğitmen** seçip **Tablo eşleme.**
    Eşleme Ayrıntıları penceresinde Eğitmen varlığı seçilidir.
-   Tıklayın **&lt;bir tablo veya Görünüm Ekle&gt;** içinde **eşleşme ayrıntıları** penceresi.
    **&lt;Bir tablo veya Görünüm Ekle&gt;** alan görünümler seçilen varlığın hangi eşlenebilir için veya tablo açılır listesini olur.
-   Seçin **kişi** aşağı açılan listeden.
-   **Eşleşme ayrıntıları** penceresi varsayılan sütun eşlemelerini ve isteğe bağlı bir koşul eklemek için güncelleştirilmiştir.
-   Tıklayarak  **&lt;koşul Ekle&gt;**.
    **&lt;Koşul Ekle&gt;** alan koşulları ayarlanabilir sütun açılan listesini duruma gelir.
-   Seçin **ayrıştırıcı** aşağı açılan listeden.
-   İçinde **işleci** sütununun **eşleşme ayrıntıları** penceresinde açılan listeden =.
-   İçinde **değeri/özellik** sütununa, **Eğitmen**. Nihai sonucu şu şekilde görünmelidir:

    ![Eşleme ayrıntıları](~/ef6/media/mappingdetails2.png)

-   Bu adımı yineleyin **Öğrenci** varlık türü, ancak yapma eşittir koşulu **Öğrenci** değeri.  
    *İstedik kaldırmak için neden **ayrıştırıcı** özelliğidir, çünkü bir tablo sütunu birden çok kez eşlenemez. Bu nedenle de özellik eşlemesi kullanılamaz bu sütun koşullu eşlemek için kullanılacaktır. Bu kullanılabilir her ikisi için de bir koşul kullanıyorsa tek yolu bir **Is Null** veya **Is Not Null** karşılaştırma.*

Tablo başına hiyerarşi devralma artık uygulanır.

![Son TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Kullanım modeli

Açık **Program.cs** dosya nerede **ana** yöntemi tanımlanır. Aşağıdaki kodu yapıştırın **ana** işlevi. Kod üç sorguları yürütür. İlk sorgu geri getirir tüm **kişi** nesneleri. İkinci sorgu kullanan **OfType** döndürülecek yöntemi **Eğitmen** nesneleri. Üçüncü sorgunun kullanan **OfType** döndürülecek yöntemi **Öğrenci** nesneleri.

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
