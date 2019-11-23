---
title: Tasarımcı CUD saklı yordamları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813555"
---
# <a name="designer-cud-stored-procedures"></a>Tasarımcı CUD saklı yordamları

Bu adım adım izlenecek yol, bir varlık türünün Create\\INSERT, Update ve Delete (CUD) işlemlerinin, Entity Framework Designer (EF Designer) kullanılarak saklı yordamlara nasıl eşleneceğini gösterir.  Varsayılan olarak, Entity Framework CUD işlemleri için SQL deyimlerini otomatik olarak oluşturur, ancak saklı yordamları bu işlemlere de eşleyebilirsiniz.  

Code First, saklı yordamlara veya işlevlere eşlemeyi desteklemediğine unutmayın. Ancak, System. Data. Entity. DbSet. SqlQuery yöntemini kullanarak saklı yordamları veya işlevleri çağırabilirsiniz. Örneğin:

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>CUD Işlemlerini Saklı yordamlarla eşlerken dikkat edilecek noktalar

CUD işlemlerini Saklı yordamlarla eşlerken aşağıdaki noktalar geçerlidir:

- CUD işlemlerinden birini saklı yordamla eşleştirdiğinizde, tümünü eşleyin. Üçü de eşleştirmez, yürütülürse eşlenmemiş işlemler başarısız olur ve bir **UpdateException** oluşturulur.
- Saklı yordamın her parametresini varlık özelliklerine eşlemeniz gerekir.
- Sunucu, ekli satır için birincil anahtar değerini oluşturursa, bu değeri varlığın anahtar özelliğine geri eşlemeniz gerekir. Aşağıdaki örnekte, **ınsertperson** saklı yordamı saklı yordamın sonuç kümesinin bir parçası olarak yeni oluşturulan birincil anahtarı döndürür. Birincil anahtar, EF Designer 'ın **&lt;sonuç bağlamaları ekle&gt;**  özelliği kullanılarak varlık anahtarı (**PersonID**) ile eşleştirilir.
- Saklı yordam çağrıları, kavramsal modeldeki varlıklarla 1:1 eşleştirilir. Örneğin, kavramsal modelinize bir devralma hiyerarşisi uygularsanız ve sonra **üst** (temel) ve **alt** (türetilmiş) varlıklar için CUD saklı yordamlarını eşlediğinizde, **alt** değişiklikleri kaydetmek yalnızca **alt**öğenin saklı yordamlarını çağıracaktır, **üst**öğenin saklı yordam çağrılarını tetiklemez.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio 'nun son sürümü.
- [Okul örnek veritabanı](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projeyi ayarlama

- Visual Studio 2012'yi açın.
- **Dosya&gt; yeni&gt; proje** ' yi seçin
- Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin.
- Ad olarak **Cudsprocssample** girin.
-  **Tamam ' ı**seçin.

## <a name="create-a-model"></a>Model oluşturma

- Çözüm Gezgini ' de proje adına sağ tıklayın ve **Ekle-&gt; yeni öğe**' yi seçin.
- Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.
- Dosya adı için **Cudsprocs. edmx** girin ve ardından **Ekle**' ye tıklayın.
- Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.
-  **Yeni bağlantı**' ya tıklayın. Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.
    Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.
- Veritabanı nesnelerinizi seçin iletişim kutusunda, **tablolar** düğümü altında **kişi** tablosunu seçin.
- Ayrıca, **saklı yordamlar ve işlevler** düğümü altında aşağıdaki saklı yordamları seçin: **deleteperson**, **ınsertperson**ve **UpdatePerson**.
- Visual Studio 2012 ile başlayarak EF Designer, saklı yordamların Toplu içe aktarımını destekler. **Seçilen saklı yordamları ve işlevleri varlık modeline aktar** varsayılan olarak denetlenir. Bu örnekte varlık türlerini ekleyen, güncelleştiren ve silen yordamlar depoladık, bunları içeri aktarmak istemiyorum ve bu onay kutusunun işaretini silecek.

    ![S profili içeri aktar](~/ef6/media/importsprocs.jpg)

-  **Son**' a tıklayın.
    Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan EF Designer, görüntülenir.

## <a name="map-the-person-entity-to-stored-procedures"></a>Kişi varlığını saklı yordamlara eşleyin

- Varlık türü  **kişiye** sağ tıklayın ve **saklı yordam eşlemesi**' ni seçin.
- Saklı yordam eşlemeleri, **eşleme ayrıntıları** penceresinde görünür.
-  **&lt;Işlev ekle&gt;seçin **' e tıklayın.
    Bu alan, kavramsal modeldeki varlık türleriyle eşleştirilabilen depolama modelindeki saklı yordamların açılan bir listesi haline gelir.
    Aşağı açılan listeden  **ınsertperson** ' ı seçin.
- Saklı yordam parametreleri ve varlık özellikleri arasındaki varsayılan eşlemeler görüntülenir. Okların eşleme yönünü gösterdiğine unutmayın: özellik değerleri, saklı yordam parametrelerine sağlanır.
-  **&lt;sonuç bağlama&gt;Ekle **' ye tıklayın.
-  **Newpersonıd**, **ınsertperson** saklı yordamının döndürdüğü parametrenin adı yazın. Baştaki veya sondaki boşlukları yazmayın emin olun.
-  **ENTER**tuşuna basın.
- Varsayılan olarak **Newpersonıd** , **PersonID**varlık anahtarıyla eşleştirilir. Bir okun eşlemenin yönünü gösterir: sonuç sütununun değeri özelliği için sağlanır.

    ![Eşleme ayrıntıları](~/ef6/media/mappingdetails.png)

-  **&lt;işlev&gt;Güncelleştir** ' i tıklatın  sonra oluşturulan açılan listeden **UpdatePerson** ' i seçin.
- Saklı yordam parametreleri ve varlık özellikleri arasındaki varsayılan eşlemeler görüntülenir.
-  **&lt;fonksiyon&gt;Sil** ' i seçin ardından ortaya çıkan açılan listeden **deleteperson** ' ı seçin.
- Saklı yordam parametreleri ve varlık özellikleri arasındaki varsayılan eşlemeler görüntülenir.

 **Kişi** varlık türünün ekleme, güncelleştirme ve silme işlemleri artık Saklı yordamlarla eşlenir.

Saklı yordamlar içeren bir varlığı güncelleştirirken veya silerken eşzamanlılık denetimini etkinleştirmek istiyorsanız, aşağıdaki seçeneklerden birini kullanın:

- Saklı yordamdaki etkilenen satırların sayısını döndürmek ve parametre adının yanındaki onay kutusunu **etkilenen** satırları denetlemek Için bir **output** parametresi kullanın. İşlem çağrıldığında döndürülen değer sıfırsa, bir  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) atılır.
- Eşzamanlılık denetimi için kullanmak istediğiniz özelliğin yanındaki **özgün değeri kullan** onay kutusunu işaretleyin. Bir güncelleştirme denendiğinde, veritabanında ilk olarak okunan özelliğin değeri veritabanına geri veri yazılırken kullanılacaktır. Değer veritabanındaki değerle eşleşmiyorsa, bir **OptimisticConcurrencyException** atılır.

## <a name="use-the-model"></a>Modeli kullanma

**Main** yönteminin tanımlandığı **program.cs** dosyasını açın. Aşağıdaki kodu Main işlevine ekleyin.

Kod yeni bir **kişi** nesnesi oluşturur, sonra nesneyi güncelleştirir ve son olarak nesneyi siler.

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

- Uygulamayı derleyin ve çalıştırın. Program aşağıdaki çıktıyı üretir *

> [!NOTE]
> PersonID, sunucu tarafından otomatik olarak oluşturulur; bu nedenle büyük olasılıkla farklı bir sayı görürsünüz *

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Visual Studio 'nun Ultimate sürümüyle çalışıyorsanız, yürütülen SQL deyimlerini görmek için hata ayıklayıcı ile IntelliTrace kullanabilirsiniz.

![IntelliTrace](~/ef6/media/intellitrace.png)
