---
title: Saklı yordamları - EF6 Sorgu Tasarımcısı
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 04478ea1c8cd43a7ba4ee788e464992af3de7f64
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283907"
---
# <a name="designer-query-stored-procedures"></a>Sorgu Tasarımcısı saklı yordamlar
Bu adım adım bir modele saklı yordamlar içeri aktarmak için Entity Framework Designer (EF Designer) kullanın ve ardından sonuçları almak için içeri aktarılan saklı yordamları çağırmak nasıl gösterir. 

Code First saklı yordamları ve işlevleri için eşleme desteklemediğini unutmayın. Ancak, System.Data.Entity.DbSet.SqlQuery yöntemi kullanarak saklı yordamları ve işlevleri çağırabilir. Örneğin:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio'nun en son sürümü.
- [School örnek veritabanını](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projesi kurun

-   Visual Studio 2012'yi açın.
-   Seçin **File -&gt; yeni -&gt; proje**
-   Sol bölmede **Visual C\#** ve ardından **konsol** şablonu.
-   Girin **EFwithSProcsSample** adı.
-   Seçin **Tamam**.

## <a name="create-a-model"></a>Model oluşturma

-   Çözüm Gezgini'nde projeye sağ tıklayıp **Ekle -&gt; yeni öğe**.
-   Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.
-   Girin **EFwithSProcsModel.edmx** dosya adı ve ardından **Ekle**.
-   Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki**.
-   Tıklayın **yeni bağlantı**.  
    Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.  
    Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda, denetleyin **tabloları** tüm tabloları seçmek için onay kutusu.  
    Ayrıca, altında aşağıdaki saklı yordamları seçin **saklı yordamları ve işlevleri** düğüm: **GetStudentGrades** ve **GetDepartmentName**. 

    ![{1&gt;İçeri Aktar&lt;1}](~/ef6/media/import.jpg)

    *EF Designer Visual Studio 2012'den itibaren saklı yordamlar toplu olarak içeri aktarma destekler. **Alma theentity modeline saklı yordamları ve işlevleri seçili** varsayılan olarak işaretlidir.*
-   **Son**'a tıklayın.

Varsayılan olarak, her bir içeri aktarılan saklı yordam ya da birden fazla sütun döndüren işlev sonucu şeklini otomatik olarak yeni bir karmaşık türü olacaktır. Bu örnekte biz sonuçlarını eşlemek istediğiniz **GetStudentGrades** işlevi **StudentGrade** varlık ve sonuçları **GetDepartmentName** için**hiçbiri** (**hiçbiri** varsayılan değerdir).

Bir işlev alma bir varlık türü döndürmek, karşılık gelen bir saklı yordam tarafından döndürülen sütunlar skaler özellikler döndürülen varlık türünün tam olarak eşleşmelidir. Bir işlev içeri aktarma, ayrıca koleksiyonları basit türler, karmaşık türler veya herhangi bir değer döndürebilir.

-   Tasarım yüzeyi ve select sağ **Model tarayıcı**.
-   İçinde **Model tarayıcı**seçin **işlevi içeri aktarmalar**ve çift tıklatarak **GetStudentGrades** işlevi.
-   İşlev içeri aktarma Düzenle iletişim kutusunda **varlıkları** ve **StudentGrade**.  
    ***İşlevi alma birleştirilebilir** en üstündeki onay kutusu **işlevi içeri aktarmalar** iletişim için birleştirilemeyen işlevler eşlemenize izin. Bu kutuyu işaretlerseniz, yalnızca birleştirilemeyen işlevler (tablo değerli işlevler) görünür **saklı yordam / işlev adı** aşağı açılan listesi. Bu kutuyu işaretleyin değil, yalnızca olmayan işlevler listesinde gösterilir.*

## <a name="use-the-model"></a>Kullanım modeli

Açık **Program.cs** dosya nerede **ana** yöntemi tanımlanır. Ana işlevine aşağıdaki kodu ekleyin.

Kod, iki saklı yordam çağırır: **GetStudentGrades** (döndürür **StudentGrades** için belirtilen *StudentId*) ve **GetDepartmentName** (departman adı çıkış parametresinde döndürür).  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

Derleme ve uygulamayı çalıştırın. Program şu çıktıyı üretir:

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Çıktı Parametreleri
-----------------

Çıktı parametreleri kullandıysanız değerleri sonuçları tamamen okunana kadar kullanılamaz. Bu dbdatareader öğesine dönüştürülemedi arka plandaki davranışı nedeniyle, bkz: [alma verileri kullanarak bir DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) daha fazla ayrıntı için.
