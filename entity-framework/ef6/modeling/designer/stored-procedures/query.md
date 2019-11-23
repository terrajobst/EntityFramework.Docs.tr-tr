---
title: Tasarımcı sorgusu saklı yordamları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182476"
---
# <a name="designer-query-stored-procedures"></a>Tasarımcı sorgusu saklı yordamları
Bu adım adım izlenecek yol, saklı yordamları bir modele aktarmak için Entity Framework Designer (EF Designer) kullanmayı ve ardından sonuçları almak için içeri aktarılan saklı yordamları çağırmayı gösterir. 

Code First, saklı yordamlara veya işlevlere eşlemeyi desteklemediğine unutmayın. Ancak, System. Data. Entity. DbSet. SqlQuery yöntemini kullanarak saklı yordamları veya işlevleri çağırabilirsiniz. Örneğin:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio 'nun son sürümü.
- [Okul örnek veritabanı](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projeyi ayarlama

-   Visual Studio 2012'yi açın.
-   **Dosya&gt; yeni&gt; proje** ' yi seçin
-   Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin.
-   Ad olarak **Efwithsprocssample** girin.
-    **Tamam ' ı**seçin.

## <a name="create-a-model"></a>Model oluşturma

-   Çözüm Gezgini projeye sağ tıklayın ve **Ekle-&gt; yeni öğe**' yi seçin.
-   Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.
-   Dosya adı için **Efwithsprocsmodel. edmx** yazın ve ardından **Ekle**' ye tıklayın.
-   Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.
-    **Yeni bağlantı**' ya tıklayın.  
    Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.  
    Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda tablolar onay kutusunu işaretleyerek tüm **tabloları seçin** .  
    Ayrıca, **saklı yordamlar ve işlevler** düğümü altında aşağıdaki saklı yordamları seçin: **Getstudentnotlar** ve **GetDepartmentName**. 

    ![{1&gt;İçeri Aktar&lt;1}](~/ef6/media/import.jpg)

    *Visual Studio 2012 ile başlayarak EF Designer, saklı yordamların Toplu içe aktarımını destekler. **Seçilen saklı yordamları ve Işlevleri Içeri aktar varlık modeline** varsayılan olarak işaretlidir.*
-    **Son**' a tıklayın.

Varsayılan olarak, içeri aktarılan her saklı yordamın veya işlevin birden fazla sütunu döndüren sonuç şekli otomatik olarak yeni bir karmaşık tür olur. Bu örnekte, **Getstudentnotlar** Işlevinin sonuçlarını **Studentgrad** varlığına ve **GetDepartmentName** sonuçlarını **none** (**hiçbiri** varsayılan değer) olarak eşlemek istiyoruz.

Bir işlev içeri aktarma işleminin bir varlık türü döndürmesi için, karşılık gelen saklı yordam tarafından döndürülen sütunlar döndürülen varlık türünün skaler özellikleriyle tam olarak eşleşmelidir. Bir işlev içeri aktarması Ayrıca basit türler, karmaşık türler veya hiçbir değer için Koleksiyonlar döndürebilir.

-   Tasarım yüzeyine sağ tıklayıp **model tarayıcısı**' nı seçin.
-   **Model tarayıcısı**' nda **işlev içeri aktarmalar**' ı seçin ve ardından **getstudentnotlar** işlevine çift tıklayın.
-   Işlev Içeri aktarmayı Düzenle iletişim kutusunda **varlıklar** seçin ve **Studentgrad**' ı seçin.  
    *İşlev içeri **aktarmaları** iletişim kutusunun üst kısmındaki **işlev içe aktarma birleştirilebilir** onay kutusu, birleştirilebilir işlevlere eşlemenizi sağlar. Bu kutuyu işaretleyin, **saklı yordam/Işlev adı** açılır listesinde yalnızca birleştirilebilir Işlevler (tablo değerli işlevler) görüntülenir. Bu kutuyu denetlemeyin, listede yalnızca birleştirilemeyen işlevler gösterilir.*

## <a name="use-the-model"></a>Modeli kullanma

**Main** yönteminin tanımlandığı **program.cs** dosyasını açın. Aşağıdaki kodu Main işlevine ekleyin.

Kod, iki saklı yordamı çağırır: **Getstudentnotlar** (belirtilen *studentitıd*Için **studentnotlar** ' ı döndürür) ve **GetDepartmentName** (çıkış parametresindeki departmanın adını döndürür).  

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

Uygulamayı derleyin ve çalıştırın. Program aşağıdaki çıktıyı üretir:

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Çıktı Parametreleri
-----------------

Çıkış parametreleri kullanılıyorsa, sonuçlar tamamen okunana kadar bu değerler kullanılamaz. Bu, DbDataReader 'ın temel davranışının nedeni, daha fazla ayrıntı için [DataReader kullanarak veri alma](https://go.microsoft.com/fwlink/?LinkID=398589) konusuna bakın.
