---
title: Tasarımcı CUD saklı yordamları - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 7a3176e1057816dd11ced5fc545aa3baa672bd03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993895"
---
# <a name="designer-cud-stored-procedures"></a>Tasarımcı CUD saklı yordamlar
Bu adım adım kılavuzda oluşturma eşlemeyle ilgili bilgi göster\\ekleme, güncelleştirme ve silme (CUD) işlemleri saklı yordamlara Entity Framework Designer (EF Designer) kullanarak bir varlık türü.  Varsayılan olarak Entity Framework CUD işlemleri için SQL deyimleri otomatik olarak oluşturur, ancak saklı yordamlar bu işlemleri için eşleyebilirsiniz.  

Code First saklı yordamları ve işlevleri için eşleme desteklemediğini unutmayın. Ancak, System.Data.Entity.DbSet.SqlQuery yöntemi kullanarak saklı yordamları ve işlevleri çağırabilir. Örneğin:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Saklı yordamları CUD işlemleri için eşleme durumlarda dikkat edilmesi gerekenler

CUD işlemleri için saklı yordamlar eşlerken aşağıdaki maddeler geçerlidir: 

-   Saklı yordama CUD işlemleri birini eşliyorsanız, bunların tümünün eşleyin. Üç eşlemeyin eşlenmemiş işlemleri yürütüldü ve bir başarısız olur **UpdateException** oluşturulur.
-   Saklı yordamın her parametre varlık özelliklerine eşlemeniz gerekir.
-   Sunucu eklenen satır için birincil anahtar değeri oluşturursa, bu değer varlığın anahtar özelliğine eşlemeniz gerekir. Aşağıdaki örnekte **InsertPerson** saklı yordam, yeni oluşturulan birincil anahtarı saklı yordamın sonuç kümesinin bir parçası döndürür. Varlık anahtarı için birincil anahtarı eşlenir (**Personıd**) kullanarak **&lt;sonuç bağlaması Ekle&gt;** EF Designer özelliğidir.
-   Saklı yordam, kavramsal modeldeki varlıklar ile eşleştirilmiş 1:1 çağrılarıdır. Örneğin, kavramsal model ve ardından harita Devralma Hiyerarşisi uygularsanız CUD yordamları için depolanan **üst** (Temel) ve **alt** kaydetme(türetilmiş)varlıklar**Alt** değişiklikleri yalnızca çağıracaktır **alt**saklı yordamları, değil tetikleyecek **üst**yordamları çağrıları depolanan.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio'nun en son sürümü.
- [School örnek veritabanını](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projesi kurun

-   Visual Studio 2012'yi açın.
-   Seçin **File -&gt; yeni -&gt; proje**
-   Sol bölmede **Visual C\#** ve ardından **konsol** şablonu.
-   Girin **CUDSProcsSample** adı.
-   Seçin **Tamam**.

## <a name="create-a-model"></a>Model oluşturma

-   Çözüm Gezgini'nde proje adına sağ tıklayın ve seçin **Ekle -&gt; yeni öğe**.
-   Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.
-   Girin **CUDSProcs.edmx** dosya adı ve ardından **Ekle**.
-   Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki**.
-   Tıklayın **yeni bağlantı**. Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.
    Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.
-   Choose, veritabanı nesneleri iletişim kutusunun altında **tabloları** düğümünü **kişi** tablo.
-   Ayrıca, altında aşağıdaki saklı yordamları seçin **saklı yordamları ve işlevleri** düğüm: **DeletePerson**, **InsertPerson**, ve **UpdatePerson** . 
-   EF Designer Visual Studio 2012'den itibaren saklı yordamlar toplu olarak içeri aktarma destekler. **Almak, varlık modele saklı yordamları ve işlevleri seçilen** varsayılan olarak işaretlidir. Bu örnekte biz, ekleme, güncelleştirme ve silme varlık türleri saklı yordamları olduğundan, biz bunları almak istiyor musunuz ve bu onay kutusunun işaretini kaldırın. 

    ![ImportSProcs](~/ef6/media/importsprocs.jpg)

-   **Son**'a tıklayın.
    Modelinizi düzenleme için bir tasarım yüzeyi sağlar, EF Designer görüntülenir.

## <a name="map-the-person-entity-to-stored-procedures"></a>Kişi varlığı eşlemek için saklı yordamlar

-   Sağ **kişi** varlık türü **saklı yordam eşlemesi**.
-   Saklı yordam eşlemeleri görünür **eşleşme ayrıntıları** penceresi.
-   Tıklayın  **&lt;seçin işlevi Ekle&gt;**.
    Alan, kavramsal modeldeki varlık türleri için eşlenen depolama modelinin saklı yordamları, aşağı açılan listesini duruma gelir.
    Seçin **InsertPerson** aşağı açılan listeden.
-   Varsayılan eşlemeleri saklı yordam parametreleri ve varlık özellikleri arasında görünür. Not oklar eşleme yönü belirtir: özellik değerleri, saklı yordam parametreleri için sağlanır.
-   Tıklayın  **&lt;sonuç bağlaması Ekle&gt;**.
-   Tür **NewPersonID**, tarafından döndürülen parametrenin adı **InsertPerson** saklı yordamı. Başında veya sonunda boşluk olmayan yazdığınızdan emin olun.
-   Tuşuna **girin**.
-   Varsayılan olarak, **NewPersonID** varlık anahtara eşlenen **Personıd**. Bir ok eşleme yönünü belirtir Not: sonuç sütunun değeri özelliği sağlandı.

    ![MappingDetails](~/ef6/media/mappingdetails.png)

-   Tıklayın **&lt;güncelleştirme işlevini seçin&gt;** seçip **UpdatePerson** elde edilen aşağı açılan listeden.
-   Varsayılan eşlemeleri saklı yordam parametreleri ve varlık özellikleri arasında görünür.
-   Tıklayın **&lt;silme işlevi seçin&gt;** seçip **DeletePerson** elde edilen aşağı açılan listeden.
-   Varsayılan eşlemeleri saklı yordam parametreleri ve varlık özellikleri arasında görünür.

Ekleme, güncelleştirme ve silme işlemlerini **kişi** varlık türü artık saklı yordamlarla eşlenen.

Eşzamanlılık güncelleştirmeye veya silmeye saklı yordamlara sahip bir varlık denetleme etkinleştirmek istiyorsanız, aşağıdaki seçeneklerden birini kullanın:

-   Kullanım bir **çıkış** sayısını döndürmek için parametre etkilenen satırlar saklı yordam ve onay **satırlardan etkilenen parametresi** parametre adının yanındaki onay kutusunu. İşlem çağrıldığında, döndürülen değer sıfır ise bir [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) oluşturulur.
-   Denetleme **kullanın, özgün değer** uyumluluğunu denetlemek için kullanmak istediğiniz bir özellik yanındaki onay kutusunu. Bir güncelleştirme çalışırken, veritabanına yazma verileri yedeklediğinizde veritabanından ilk olarak okundu özelliğinin değeri kullanılır. Değeri veritabanındaki değerle eşleşmiyorsa bir **OptimisticConcurrencyException** oluşturulur.

## <a name="use-the-model"></a>Kullanım modeli

Açık **Program.cs** dosya nerede **ana** yöntemi tanımlanır. Ana işlevine aşağıdaki kodu ekleyin.

Yeni bir kod oluşturur **kişi** nesnesi daha sonra nesneyi güncelleştirir ve son olarak nesneyi siler.         

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

-   Derleme ve uygulamayı çalıştırın. Program şu çıktıyı üretir. *
    >[!NOTE]
> Sunucu tarafından otomatik olarak oluşturulan Personıd için büyük olasılıkla farklı sayı * görürsünüz

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Visual Studio Ultimate sürümü ile çalışıyorsanız, yürütülen SQL deyimleri görmek için IntelliTrace hata ayıklayıcısı ile kullanabilirsiniz.

![IntelliTrace](~/ef6/media/intellitrace.png)
