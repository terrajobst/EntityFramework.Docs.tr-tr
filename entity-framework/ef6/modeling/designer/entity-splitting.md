---
title: Tasarımcı varlık bölme - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 214561f0a0381bced3ceae0b6acfcd45f5dd65c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995625"
---
# <a name="designer-entity-splitting"></a>Tasarımcı varlık bölme
Bu izlenecek yol, bir model Entity Framework Designer (EF Designer) ile değiştirerek, iki tabloya bir varlık türü eşlemeyle ilgili bilgi gösterir. Tablolar, bir ortak anahtar paylaştığınızda bir varlık için birden çok tablo eşleyebilirsiniz. İki tabloya bir varlık türü eşlemek için uygulanacak kavramları kolayca ikiden fazla tablolara eşleme bir varlık türü için genişletilir.

EF Designer ile çalışırken, kullanılan ana windows aşağıdaki resimde gösterilmektedir.

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Önkoşullar

Visual Studio 2012 veya Visual Studio 2010, Ultimate, Premium, Professional veya Web Express sürümü.

## <a name="create-the-database"></a>Veritabanı oluşturma

Visual Studio ile yüklenen veritabanı sunucusu, yüklediğiniz Visual Studio sürümüne bağlı olarak farklılık gösterir:

-   Visual Studio 2012 kullanıyorsanız, bir LocalDB veritabanına oluşturmayı.
-   Visual Studio 2010 kullanıyorsanız, SQL Express veritabanı oluşturursunuz.

İlk iki tablolarla birleştirmek için tek bir varlığa kullanacağız bir veritabanı oluşturacağız.

-   Visual Studio'yu Aç
-   **Görünüm -&gt; Sunucu Gezgini**
-   Sağ tıklayın **veri bağlantıları -&gt; bağlantı ekle...**
-   Sunucu gezgininden veritabanına bağlamadıysanız seçmeniz gerekir önce **Microsoft SQL Server** veri kaynağı
-   LocalDB veya hangisinin bağlı olarak yüklediğiniz SQL Express için Bağlan
-   Girin **EntitySplitting** veritabanı adı
-   Seçin **Tamam** ve bir yeni bir veritabanı oluşturmak istiyorsanız istenir **Evet**
-   Yeni veritabanı şimdi sunucu Gezgini'nde görünür.
-   Visual Studio 2012 kullanıyorsanız
    -   Sunucu Gezgini veritabanı üzerinde sağ tıklayıp **yeni sorgu**
    -   Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **Yürüt**
-   Visual Studio 2010 kullanıyorsanız
    -   Seçin **veri -&gt; Transact - SQL Düzenleyicisi&gt; yeni bağlantı...**
    -   Girin **.\\ SQLEXPRESS** tıklayın ve sunucu adı olarak **Tamam**
    -   Seçin **EntitySplitting** veritabanı açılır menüden aşağı sorgu Düzenleyicisi'ni üstünde
    -   Yeni bir sorguda aşağıdaki SQL kopyalayın, sonra sağ tıklatın ve sorgu **SQL Yürüt**

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

-   Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**.
-   Sol bölmede **Visual C\#** ve ardından **konsol uygulaması** şablonu.
-   Girin **MapEntityToTablesSample** tıklayın ve proje adı olarak **Tamam**.
-   Tıklayın **Hayır** ilk bölümde oluşturduğunuz SQL sorguyu kaydetmek isteyip istemediğiniz sorulduğunda.

## <a name="create-a-model-based-on-the-database"></a>Veritabanını temel alan bir Model oluşturma

-   Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**.
-   Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.
-   Girin **MapEntityToTablesModel.edmx** dosya adı ve ardından **Ekle**.
-   Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki.**
-   Seçin **EntitySplitting** açılan bağlantı ve **sonraki**.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda yanındaki kutuyu işaretleyin **tabloları** düğümü.
    Bu tablodan tüm ekler **EntitySplitting** veritabanı modeli.
-   **Son**'a tıklayın.

Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.

## <a name="map-an-entity-to-two-tables"></a>Bir varlık için iki tablo eşleme

Bu adımda güncelleştireceğiz **kişi** alınan verileri birleştirmek varlık türü **kişi** ve **PersonInfo** tablolar.

-   Seçin **e-posta** ve **telefon** özelliklerini ** PersonInfo ** varlık ve ENTER tuşuna **Ctrl + X** anahtarları.
-   Seçin ** kişi ** varlık ve ENTER tuşuna **Ctrl + V** anahtarları.
-   Tasarım yüzeyinde seçin **PersonInfo** varlık ve ENTER tuşuna **Sil** klavyedeki düğmesi.
-   Tıklayın **Hayır** kaldırmak isteyip istemediğiniz sorulduğunda **PersonInfo** tablo hakkında eşlemek üzere duyuyoruz modelden **kişi** varlık.

    ![DeleteTables](~/ef6/media/deletetables.png)

Sonraki adımlar gerektiren **eşleşme ayrıntıları** penceresi. Bu pencere göremiyorsanız, tasarım yüzeyi ve select sağ **eşleşme ayrıntıları**.

-   Seçin **kişi** varlık türü ve tıklatın **&lt;bir tablo veya Görünüm Ekle&gt;** içinde **eşleşme ayrıntıları** penceresi.
-   Seçin ** PersonInfo ** aşağı açılan listeden.
    **Eşleşme ayrıntıları** penceresi, varsayılan sütun eşlemelerini ile güncelleştirilir, bunlar bizim senaryomuz için bir sakınca yoktur.

**Kişi** varlık türü için eşlenmiş artık **kişi** ve **PersonInfo** tablolar.

![Mapping2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Kullanım modeli

-   Main yöntemine aşağıdaki kodu yapıştırın.

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

-   Derleme ve uygulamayı çalıştırın.

Bu uygulamayı çalıştıran sonucunda veritabanında aşağıdaki T-SQL deyimlerini yürütüldü. 

-   Aşağıdaki iki **Ekle** deyimleri yürütülen bağlam yürütmenin sonucu olarak. SaveChanges(). Verilerden aldıkları **kişi** varlık ve arasında bölmek **kişi** ve **PersonInfo** tablolar.

    ![Insert1](~/ef6/media/insert1.png)

    ![Insert2](~/ef6/media/insert2.png)
-   Aşağıdaki **seçin** veritabanında kişiler numaralandırma sonucu olarak yürütülmesi. Verileri birleştirir **kişi** ve **PersonInfo** tablo.

    ![Seçim](~/ef6/media/select.png)
