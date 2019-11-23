---
title: Tasarımcı tablosu bölme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921778"
---
# <a name="designer-table-splitting"></a>Tasarımcı tablosu bölme
Bu izlenecek yol, bir modeli Entity Framework Designer (EF Designer) ile değiştirerek birden çok varlık türünün tek bir tabloya nasıl eşlendiğini gösterir.

Tablo bölmeyi kullanmak isteyebileceğiniz bir nedenden dolayı, nesnelerinizi yüklemek için yavaş yükleme kullandığınızda bazı özelliklerin yüklenmesi ertelenebilir. Büyük miktarda veri içerebilen özellikleri ayrı bir varlıkta ayırabilir ve yalnızca gerektiğinde yükleyebilirsiniz.

Aşağıdaki görüntüde, EF Designer ile çalışırken kullanılan ana pencereler gösterilmektedir.

![EF Tasarımcısı](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio 'nun son sürümü.
- [Okul örnek veritabanı](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projeyi ayarlama

Bu izlenecek yol, Visual Studio 2012 ' i kullanıyor.

-   Visual Studio 2012'yi açın.
-   **Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje**' ye tıklayın.
-   Sol bölmede, Visual C\#' ye tıklayın ve ardından konsol uygulaması şablonunu seçin.
-   Projenin adı olarak **Tablespttingsample** girin ve **Tamam**' a tıklayın.

## <a name="create-a-model-based-on-the-school-database"></a>Okul veritabanına dayalı bir model oluşturma

-   Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe**' ye tıklayın.
-   Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.
-   Dosya adı için **TableSplittingModel. edmx** girin ve ardından **Ekle**' ye tıklayın.
-   Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından İleri ' ye tıklayın **.**
-   Yeni bağlantı ' ya tıklayın. Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.
    Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda **tablo** düğümünü katlayın ve **kişi** tablosunu kontrol edin. Bu, belirtilen tabloyu **okul** modeline ekler.
-    **Son**' a tıklayın.

Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.  **Veritabanı nesnelerinizi seçin** iletişim kutusunda seçtiğiniz tüm nesneler modele eklenir.

## <a name="map-two-entities-to-a-single-table"></a>Iki varlığı tek bir tabloyla eşleyin

Bu bölümde, **kişi** varlığını iki varlığa bölecektir ve sonra bunları tek bir tabloyla eşleştirirsiniz.

> [!NOTE]
> **Kişi** varlığı, büyük miktarda veri içerebilen herhangi bir özellik içermez; yalnızca örnek olarak kullanılır.

-   Tasarım yüzeyinde boş bir alana sağ tıklayın, **Yeni Ekle**' nin üzerine gelin ve **varlık**' a tıklayın.
     **Yeni varlık** iletişim kutusu görüntülenir.
-   **Anahtar özellik** adı için **varlık adı** ve **PersonID** için **hireınfo** yazın.
-    **Tamam**' a tıklayın.
-   Tasarım yüzeyinde yeni bir varlık türü oluşturulur ve görüntülenir.
-    **Kişi** varlık türünün **hiredate** özelliğini seçin ve **CTRL + X** tuşlarına basın.
-   **Hireınfo** varlığını seçin ve **CTRL + V** tuşlarına basın.
-   **Kişi** ve **hireınfo**arasında bir ilişki oluşturun. Bunu yapmak için tasarım yüzeyinde boş bir alana sağ tıklayın, **Yeni Ekle**' nin üzerine gelin ve **ilişkilendirme**' ye tıklayın.
-    **Ilişki ekle** iletişim kutusu görüntülenir. **Personhireınfo** adı varsayılan olarak verilir.
-   İlişkinin her iki ucunda çeşitlilik **1 (bir)** belirtin.
-   Tuşuna **Tamam**.

Sonraki adım için **eşleme ayrıntıları** penceresi gerekir. Bu pencereyi göremiyorsanız, tasarım yüzeyine sağ tıklayıp **eşleme ayrıntıları**' nı seçin.

-    ** varlık** türünü seçin ve **eşleme ayrıntıları** penceresinde **&lt;tablo ekleme veya&gt; görüntüleme** ' ye tıklayın.
-   **&lt;bir tablo eklemek veya &gt;** alan açılır listesinden **kişi** ' yi seçin. Liste, seçilen varlığın eşleştiribileceği tabloları veya görünümleri içerir.
    Uygun özellikler varsayılan olarak eşlenmelidir.

    ![Eşleme](~/ef6/media/mapping.png)

-   Tasarım yüzeyinde **Personhireınfo** ilişkilendirmesini seçin.
-   Tasarım yüzeyinde ilişkiye sağ tıklayın ve **Özellikler**' i seçin.
-   **Özellikler** penceresinde **başvurusal kısıtlamalar** özelliğini seçin ve üç nokta düğmesine tıklayın.
-   **Sorumlu** açılan listesinden **kişi** ' yi seçin.
-   Tuşuna **Tamam**.

 

## <a name="use-the-model"></a>Modeli kullanma

-   Aşağıdaki kodu Main yöntemine yapıştırın.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   Uygulamayı derleyin ve çalıştırın.

Aşağıdaki T-SQL deyimleri, bu uygulamayı çalıştırmanın bir sonucu olarak **okul** veritabanına karşı yürütüldü. 

-   Aşağıdaki **ekleme** bağlam yürütmenin sonucu olarak yürütüldü. SaveChanges () ve **kişi** ve **hireınfo** varlıklarındaki verileri birleştirir

    ![Ekle](~/ef6/media/insert.png)

-   Aşağıdaki **seçim** , bağlam yürütmenin sonucu olarak yürütüldü. Kişiler. FirstOrDefault () ve yalnızca **kişiyle** eşleştirilmiş sütunları seçer

    ![1 seçin](~/ef6/media/select1.png)

-   Şu **Select** , Existingperson. eğitmeni gezinti özelliğine erişmenin ve yalnızca **hireınfo** ile eşleştirilmiş sütunları seçen bir sonuç olarak yürütüldü

    ![2 seçin](~/ef6/media/select2.png)
