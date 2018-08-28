---
title: Tasarlayıcı tablosu bölme - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: 87b6e1bd0374f77dfffab342c659cf4e16c8a337
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994509"
---
# <a name="designer-table-splitting"></a>Bölme tasarlayıcı tablosu
Bu izlenecek yol, birden çok varlık türleri tek bir tabloya bir model Entity Framework Designer (EF Designer) ile değiştirerek eşlemeyle ilgili bilgi gösterir.

Bölme tablosunu kullanmak isteyebileceğiniz nedenlerinden biri nesnelerinizi yüklemek için yükleme yavaş kullanırken bazı özellikler yüklenmesini geciktirme. Ayrı bir varlık çok büyük miktarda veri içeren ve yalnızca gerekli olduğunda, yükleme özelliklerini ayırabilirsiniz.

EF Designer ile çalışırken, kullanılan ana windows aşağıdaki resimde gösterilmektedir.

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio'nun en son sürümü.
- [School örnek veritabanını](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projesi kurun

Bu izlenecek yol, Visual Studio 2012 kullanıyor.

-   Visual Studio 2012'yi açın.
-   Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**.
-   Sol bölmede, Visual C tıklayın\#ve ardından konsol uygulaması şablonu seçin.
-   Girin **TableSplittingSample** tıklayın ve proje adı olarak **Tamam**.

## <a name="create-a-model-based-on-the-school-database"></a>School veritabanını temel alan bir Model oluşturma

-   Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**.
-   Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.
-   Girin **TableSplittingModel.edmx** dosya adı ve ardından **Ekle**.
-   Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki.**
-   Yeni bağlantı tıklayın. Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.
    Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda, düzleştirme **tabloları** düğüm ve onay **kişi** tablo. Bu belirtilen tabloya ekler **Okul** modeli.
-   **Son**'a tıklayın.

Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir. Seçtiğiniz tüm nesneleri **veritabanı nesnelerinizi seçin** iletişim kutusu, modele eklenir.

## <a name="map-two-entities-to-a-single-table"></a>Tek bir tabloya iki varlıkları Eşle

Bu bölümde, bölecek **kişi** iki varlık varlığa ve ardından bunları tek bir tabloya eşleyin.

> [!NOTE]
> **Kişi** varlık büyük miktarda veriler içeren tüm özellikleri içermez; yalnızca örnek olarak kullanılır.

-   Tasarım yüzeyinde boş bir alana sağ tıklayın, fareyle **yeni Ekle**, tıklatıp **varlık**.
    **Yeni varlık** iletişim kutusu görüntülenir.
-   Tür **HireInfo** için **varlık adı** ve **Personıd** için **anahtar özellik** adı.
-   **Tamam**'ı tıklatın.
-   Yeni bir varlık türü oluşturulur ve tasarım yüzeyinde görüntülenir.
-   Seçin **İşeAlmaTarihi** özelliği **kişi** varlık yazın ve ENTER tuşuna **Ctrl + X** anahtarları.
-   Seçin **HireInfo** varlık ve ENTER tuşuna **Ctrl + V** anahtarları.
-   Arasında ilişkilendirme oluşturma **kişi** ve **HireInfo**. Bunu yapmak için tasarım yüzeyinde boş bir alana sağ tıklayın, işaret **yeni Ekle**, tıklatıp **ilişkilendirme**.
-   **Ekleme ilişkilendirme** iletişim kutusu görüntülenir. **PersonHireInfo** adı varsayılan olarak verilir.
-   Çokluk belirtin **1(One)** ilişkinin her iki ucunda.
-   Tuşuna **Tamam**.

Sonraki adım gerektirir **eşleşme ayrıntıları** penceresi. Bu pencere göremiyorsanız, tasarım yüzeyi ve select sağ **eşleşme ayrıntıları**.

-   Seçin **HireInfo** varlık türü ve tıklatın **&lt;bir tablo veya Görünüm Ekle&gt;** içinde **eşleşme ayrıntıları** penceresi.
-   Seçin **kişi** gelen **&lt;bir tablo veya Görünüm Ekle&gt;** alan açılan listesi. Listenin tabloları içeren veya seçilen varlığın hangi eşlenebilir için görüntüler.
    Uygun özellikleri varsayılan olarak eşlenmesi gerekir.

    ![Eşleme](~/ef6/media/mapping.png)

-   Seçin **PersonHireInfo** tasarım yüzeyinde ilişkilendirme.
-   Tasarım yüzeyi ve select ilişkilendirmenin sağ **özellikleri**.
-   İçinde **özellikleri** penceresinde **başvuru kısıtlamalarını** özelliği ve üç nokta düğmesine tıklayın.
-   Seçin **kişi** gelen **asıl** aşağı açılan listesi.
-   Tuşuna **Tamam**.

 

## <a name="use-the-model"></a>Kullanım modeli

-   Main yöntemine aşağıdaki kodu yapıştırın.

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
-   Derleme ve uygulamayı çalıştırın.

Aşağıdaki T-SQL deyimlerini karşı yürütüldü **Okul** sonucunda bu uygulamayı çalıştırmadan veritabanı. 

-   Aşağıdaki **Ekle** bağlam yürütmenin sonucu olarak yürütülmesi. SaveChanges() ve birleştirir verilerinden **kişi** ve **HireInfo** varlıklar

    ![Ekleme](~/ef6/media/insert.png)

-   Aşağıdaki **seçin** bağlam yürütmenin sonucu olarak yürütülmesi. People.FirstOrDefault() ve sütunları eşlenen seçer **kişi**

    ![Select1](~/ef6/media/select1.png)

-   Aşağıdaki **seçin** gezinti özelliği existingPerson.Instructor erişme sonucu olarak çalıştırılan ve yalnızca eşlenen sütun seçer **HireInfo**

    ![Select2](~/ef6/media/select2.png)
