---
title: Tasarımcı TPH devralma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418429"
---
# <a name="designer-tph-inheritance"></a>Tasarımcı TPH devralma
Bu adım adım izlenecek yol, Entity Framework Designer (EF Designer) ile kavramsal modelinizde hiyerarşi başına tablo (TPH) devralmanın nasıl uygulanacağını gösterir. TPH devralma bir devralma hiyerarşisindeki tüm varlık türlerinin verilerini korumak için bir veritabanı tablosu kullanır.

Bu kılavuzda kişi tablosunu üç varlık türü ile eşliyoruz: kişi (temel tür), öğrenci (kişiden türetiliyor) ve eğitmen (kişiden türetilir). Veritabanından bir kavramsal model oluşturacağız (Database First) ve ardından modeli, EF tasarımcısını kullanarak TPH devralmayı uygulayacak şekilde değiştirin.

Model First kullanarak bir TPH devrtabanına eşleme yapılabilir, ancak karmaşık olan kendi veritabanı oluşturma iş akışınızı yazmanız gerekir. Daha sonra bu iş akışını EF tasarımcısında **veritabanı oluşturma Iş akışı** özelliğine atamalısınız. Code First kullanmak daha kolay bir alternatiftir.

## <a name="other-inheritance-options"></a>Diğer devralma seçenekleri

Tablo/tür (TPT), veritabanındaki ayrı tabloların devralmaya katılan varlıklara eşlendiği başka bir devralma türüdür.  EF Designer ile tablo türü devralmayı eşleme hakkında daha fazla bilgi için, bkz. [EF Designer TPT devralma](~/ef6/modeling/designer/inheritance/tpt.md).

Tablo başına somut tür devralma (TPC) ve karışık devralma modelleri Entity Framework çalışma zamanı tarafından desteklenir, ancak EF Designer tarafından desteklenmez. TPC veya karışık devralma kullanmak istiyorsanız iki seçeneğiniz vardır: Code First kullanın veya EDMX dosyasını el ile düzenleyin. EDMX dosyası ile çalışmayı seçerseniz, eşleme ayrıntıları penceresi "güvenli mod" a yerleştirilir ve bu eşlemeleri değiştirmek için tasarımcıyı kullanamazsınız.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio 'nun son sürümü.
- [Okul örnek veritabanı](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projeyi ayarlama

-   Visual Studio 2012'yi açın.
-   **Dosya&gt; yeni&gt; proje** ' yi seçin
-   Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin.
-   Ad olarak **Tphdbfirstsample** girin.
-    **Tamam ' ı**seçin.

## <a name="create-a-model"></a>Model oluşturma

-   Çözüm Gezgini ' de proje adına sağ tıklayın ve **Ekle-&gt; yeni öğe**' yi seçin.
-   Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.
-   Dosya adı için **Tphmodel. edmx** girin ve ardından **Ekle**' ye tıklayın.
-   Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.
-    **Yeni bağlantı**' ya tıklayın.
    Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.
    Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusundaki tablolar düğümünün altında **kişi** tablosunu seçin.
-    **Son**' a tıklayın.

Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir. Veritabanı nesnelerinizi seçin iletişim kutusunda seçtiğiniz tüm nesneler modele eklenir.

Bu, **kişi** tablosunun veritabanında nasıl göründüğünü bu şekilde yapar.

![Kişi tablosu](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Tablo-hiyerarşi devralmayı Uygula

 **Kişi** tablosu, iki değerden birine sahip olabilen **ayrıştırıcı** sütununa sahiptir: "öğrenci" ve "eğitmen". **Kişi** tablosunun, **öğrenci** varlığına veya **eğitmen** varlığına eşlendiği değere bağlı olarak. Kişi **tablosunda Ayrıca** , bir kişi aynı anda bir öğrenci ve bir eğitmen (Bu kılavuzda değil) olabileceğinden **null değer atanabilir** olması gereken Iki sütun vardır: **HireDate**  ** ve kayıttarihi.**

### <a name="add-new-entities"></a>Yeni varlık Ekle

-   Yeni bir varlık ekleyin.
    Bunu yapmak için Entity Framework Designer Tasarım yüzeyindeki boş bir alana sağ tıklayın ve **Ekle-&gt;varlık**' ı seçin.
-    **Varlık adı** için **eğitmen** yazın ve **temel tür**için açılan listeden **kişi** ' yı seçin.
-    **Tamam**' a tıklayın.
-   Başka bir yeni varlık ekleyin.  **Varlık adı** için **öğrenci** yazın ve **temel tür**için açılan listeden **kişi** ' yı seçin.

Tasarım yüzeyine iki yeni varlık türü eklenmiştir. Bir ok, yeni varlık türlerinden **kişiye** varlık türüne göre işaret eder; Bu, **kişinin** yeni varlık türleri için temel tür olduğunu gösterir.

-    **Kişi** varlığının **hiredate** özelliğine sağ tıklayın.  **Kes** ' i seçin (veya CTRL + X tuşunu kullanın).
-    **Eğitmen** varlığına sağ tıklayıp **Yapıştır** ' ı seçin (veya CTRL-V anahtarını kullanın).
-    **HireDate** özelliğine sağ tıklayın ve **Özellikler**' i seçin.
-    **Özellikler** penceresinde, **Nullable** özelliğini **false**olarak ayarlayın.
-    **Kişi** varlığının **kayıttarihi** özelliğine sağ tıklayın.  **Kes** ' i seçin (veya CTRL + X tuşunu kullanın).
-    **Öğrenci** varlığına sağ tıklayıp Yapıştır ' ı seçin **(veya CTRL-V anahtarını kullanın).**
-   Kayıttarihi ****  özelliğini seçin ve **Nullable** özelliğini **false**olarak ayarlayın.
-   Varlık türü  **kişiyi** seçin.  **Özellikler** penceresinde, **soyut** özelliğini **doğru**olarak ayarlayın.
-   **Kişiden** **ayrıştırıcı** özelliğini silin. Silinmesi gereken neden aşağıdaki bölümde açıklanmıştır.

### <a name="map-the-entities"></a>Varlıkları eşleme

-    **Eğitmeni** sağ tıklatın ve **Tablo eşleme** ' yi seçin.
    Eğitmen varlığı, eşleme ayrıntıları penceresinde seçilir.
-    **Eşleme ayrıntıları** penceresinde ** tablo eklemek&lt;veya &gt;görüntüleyin** ' e tıklayın.
     **&lt;tablo veya görünüm&gt;**  alanı seçili varlığın eşleştiribileceği tablo veya görünümlerin açılan bir listesi haline gelir.
-   Açılan listeden **kişi** seçin.
-    **Eşleme ayrıntıları** penceresi, varsayılan sütun eşlemeleriyle ve koşul ekleme seçeneği ile güncelleştirilir.
-    **Koşul eklemek&lt;** ' ye tıklayın&gt;.
     **&lt;koşul ekleme&gt;**  alan, koşulların ayarlanbileceği bir aşağı açılan sütun listesi haline gelir.
-   Aşağı açılan listeden **ayrıştırıcı** seçin.
-    **Eşleme ayrıntıları** penceresinin **işleç** sütununda, açılan listeden = seçeneğini belirleyin.
-    **Değer/özellik** sütununda, **eğitmen**yazın. Nihai sonuç şöyle görünmelidir:

    ![Eşleme ayrıntıları](~/ef6/media/mappingdetails2.png)

-    **Öğrenci** varlık türü için bu adımları yineleyin, ancak koşulu **öğrenci** değerine eşit yapın.  
    ***Ayrıştırıcı** özelliğini kaldırmak istediğimiz neden, bir tablo sütununu birden çok kez eşleyemezsiniz. Bu sütun koşullu eşleme için kullanılacaktır, bu nedenle de Özellik eşleme için kullanılamaz. Her ikisi için de kullanılabilecek tek yol, bir koşul **null** kullanıyorsa veya null karşılaştırma **değildir** .*

Hiyerarşi başına tablo devralma işlemi şimdi uygulandı.

![Son TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Modeli kullanma

**Main** yönteminin tanımlandığı **program.cs** dosyasını açın. Aşağıdaki kodu **Main** işlevine yapıştırın. Kod üç sorguyu yürütür. İlk sorgu tüm **kişi** nesnelerini geri getirir. İkinci sorgu, **eğitmen** nesnelerini döndürmek için **OfType** metodunu kullanır. Üçüncü sorgu, **öğrenci** nesnelerini döndürmek için **OfType** metodunu kullanır.

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
