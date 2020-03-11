---
title: Tasarımcı varlık bölünmesi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418464"
---
# <a name="designer-entity-splitting"></a>Tasarımcı varlığı bölme
Bu izlenecek yol, bir modeli Entity Framework Designer (EF Designer) ile değiştirerek bir varlık türünün iki tabloya nasıl eşleneceğini gösterir. Tablolar ortak bir anahtar paylaşıyorsa, bir varlığı birden çok tabloya eşleyebilirsiniz. Bir varlık türünü iki tabloya eşlemek için uygulanan kavramlar, bir varlık türünü ikiden fazla tabloya eşlemek için kolayca genişletilir.

Aşağıdaki görüntüde, EF Designer ile çalışırken kullanılan ana pencereler gösterilmektedir.

![EF Tasarımcısı](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2012 veya Visual Studio 2010, Ultimate, Premium, Professional veya Web Express Edition.

## <a name="create-the-database"></a>Veritabanını oluşturma

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösteren bir sürümdür:

-   Visual Studio 2012 kullanıyorsanız, LocalDB veritabanı oluşturursunuz.
-   Visual Studio 2010 kullanıyorsanız, bir SQL Express veritabanı oluşturursunuz.

İlk olarak, tek bir varlıkta birleştirme yapacağımız iki tablo içeren bir veritabanı oluşturacağız.

-   Visual Studio’yu açın
-   **&gt; Sunucu Gezgini görüntüle**
-   Veri bağlantıları ' na sağ tıklayın **&gt; bağlantı ekle...**
-   Sunucu Gezgini bir veritabanına bağlı değilseniz, veri kaynağı olarak **Microsoft SQL Server** seçmeniz gerekir
-   Hangi hangisinin yüklü olduğuna bağlı olarak, LocalDB veya SQL Express 'e bağlanın
-   Veritabanı adı olarak **Entitybölünmesi** girin
-   **Tamam** ' ı seçtiğinizde, yeni bir veritabanı oluşturmak isteyip istemediğiniz sorulur, **Evet** ' i seçin.
-   Yeni veritabanı artık Sunucu Gezgini görüntülenir
-   Visual Studio 2012 kullanıyorsanız
    -   Sunucu Gezgini veritabanında veritabanına sağ tıklayın ve **Yeni sorgu** ' yı seçin.
    -   Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **Yürüt** ' ü seçin.
-   Visual Studio 2010 kullanıyorsanız
    -   **Veri&gt; Transact SQL Düzenleyicisi-&gt; yeni sorgu bağlantısı ' nı seçin...**
    -   Sunucu adı olarak **.\\SQLExpress** girin ve **Tamam 'a** tıklayın
    -   Sorgu düzenleyicisinin en üstündeki açılan listeden **Entitybölünmesi** veritabanını seçin
    -   Aşağıdaki SQL 'i yeni sorguya kopyalayın, ardından sorguya sağ tıklayıp **SQL 'ı Yürüt** ' ü seçin.

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a>Proje oluşturma

-   **Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje**' ye tıklayın.
-   Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol uygulaması** şablonunu seçin.
-   Projenin adı olarak **Mapentitytotablessample** girin ve **Tamam**' a tıklayın.
-   İlk bölümde oluşturulan SQL sorgusunu kaydetmek isteyip istemediğiniz sorulduğunda **Hayır** ' a tıklayın.

## <a name="create-a-model-based-on-the-database"></a>Veritabanını temel alan bir model oluşturma

-   Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe**' ye tıklayın.
-   Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.
-   Dosya adı için **Mapentitytotablesmodel. edmx** girin ve ardından **Ekle**' ye tıklayın.
-   Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından İleri ' ye tıklayın **.**
-   Açılan listeden **Entitybölünmesi** bağlantısını seçin ve **İleri**' ye tıklayın.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda, **tablolar** düğümünün yanındaki kutuyu işaretleyin.
    Bu, tüm tabloları **Entitybölünmesi** veritabanından modele ekler.
-    **Son**' a tıklayın.

Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.

## <a name="map-an-entity-to-two-tables"></a>Bir varlığı Iki tabloya eşleme

Bu adımda **Person ve Person** **Info** tablolarından verileri birleştirmek için **kişi** varlık türünü güncelleştireceğiz.

-    **PersonInfo **varlığının **e-posta** ve **Telefon** özelliklerini seçin ve **CTRL + X** tuşlarına basın.
-    **Kişi **varlığını seçin ve **CTRL + V** tuşlarına basın.
-   Tasarım yüzeyinde, **PersonInfo** varlığını seçin ve klavyede **Sil** düğmesine basın.
-   Modelden Person (kişi) **' ı kaldırmak isteyip istemediğiniz sorulduğunda,** bu **bilgileri** **kişi** varlığıyla eşlemek istiyoruz.

    ![Tabloları sil](~/ef6/media/deletetables.png)

Sonraki adımlarda **eşleme ayrıntıları** penceresi gerekir. Bu pencereyi göremiyorsanız, tasarım yüzeyine sağ tıklayıp **eşleme ayrıntıları**' nı seçin.

-    ** varlık** türünü seçin ve **eşleme ayrıntıları** penceresinde **&lt;tablo Ekle veya &gt;görüntüle** ' ye tıklayın.
-   Açılan listeden **PersonInfo ** seçin.
     **Eşleme ayrıntıları** penceresi varsayılan sütun eşlemeleriyle güncelleştirilir, bunlar senaryolarımız için uygundur.

 **Kişi** varlık türü artık **kişi** ve **PersonInfo** tablolarıyla eşlenir.

![Eşleme 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Modeli kullanma

-   Aşağıdaki kodu Main yöntemine yapıştırın.

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   Uygulamayı derleyin ve çalıştırın.

Aşağıdaki T-SQL deyimleri, bu uygulamayı çalıştırmanın bir sonucu olarak veritabanına karşı yürütüldü. 

-   Aşağıdaki iki **Insert** deyimi, bağlamını yürütmenin sonucu olarak yürütüldü. SaveChanges (). Bunlar, **kişi** varlığındaki verileri alır ve **kişi ile Person** **Info** tabloları arasında böler.

    ![1 ekle](~/ef6/media/insert1.png)

    ![2 Ekle](~/ef6/media/insert2.png)
-   Veritabanındaki kişilerin numaralandırıldığı bir sonuç olarak aşağıdaki **seçim** yürütüldü. **Kişi ve Person** **Info** tablosundaki verileri birleştirir.

    ![Şunu seçin:](~/ef6/media/select.png)
