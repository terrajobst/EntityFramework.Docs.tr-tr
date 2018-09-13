---
title: Tablo değerli işlev (Tvf) - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 6130e6bd550497509f3dcc0242077046d12c601a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489329"
---
# <a name="table-valued-functions-tvfs"></a>Tablo değerli işlev (Tvf)
> [!NOTE]
> **EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Video ve adım adım izlenecek yol, Entity Framework Designer kullanarak tablo değerli işlev (Tvf) map işlemi gösterilmektedir. Ayrıca, bir LINQ Sorgu bir TVF çağırmak nasıl gösterir.

Tvf şu anda yalnızca veritabanı ilk iş akışında desteklenir.

TVF destek Entity Framework 5 sürümü kullanıma sunulmuştur. Tablo değerli işlevler, sabit listeleri ve .NET Framework 4.5 hedeflemelidir uzamsal türler gibi yeni özellikleri kullanmak için dikkat edin. Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.

Tvf da önemli bir fark saklı yordamlar için oldukça benzerdir: bir TVF sonucunu birleştirilebilir. Bir LINQ Sorgu sonuçlarını bir saklı yordam işleyemez bir TVF sonuçlardan kullanılabilir anlamına gelir.

## <a name="watch-the-video"></a>Videoyu izleyin

**Tarafından sunulan**: Julia Kornich

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Ön koşullar

Bu izlenecek yolu tamamlamak için şunları yapmanız:

- Yükleme [School veritabanını](~/ef6/resources/school-database.md).

- Visual Studio'nun yeni bir sürümü yüklü

## <a name="set-up-the-project"></a>Projesi kurun

1.  Visual Studio'yu Aç
2.  Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**
3.  Sol bölmede **Visual C\#** ve ardından **konsol** şablonu
4.  Girin **TVF** tıklayın ve proje adı olarak **Tamam**

## <a name="add-a-tvf-to-the-database"></a>Veritabanına bir TVF ekleme

-   Seçin **görünümü -&gt; SQL Server Nesne Gezgini**
-   LocalDB sunucuları listesinde değilse: sağ **SQL Server** seçip **SQL Server Ekle** varsayılan **Windows kimlik doğrulaması** LocalDB sunucusuna bağlanmak için
-   LocalDB düğümünü genişletin
-   Veritabanı düğümü altında School veritabanı düğümünü sağ tıklatın ve seçin **yeni sorgu...**
-   T-SQL Düzenleyicisi'nde aşağıdaki TVF tanımını yapıştırın

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   T-SQL Düzenleyicisi sağ fare düğmesine tıklayıp **Yürüt**
-   School veritabanını GetStudentGradesForCourse işlevi eklenir

 

## <a name="create-a-model"></a>Model oluşturma

1.  Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**
2.  Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** içinde **şablonları** bölmesi
3.  Girin **TVFModel.edmx** dosya adı ve ardından **Ekle**
4.  Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **İleri**
5.  Tıklayın **yeni bağlantı** Enter **(localdb)\\ifadesini mssqllocaldb** Enter sunucu adı metin kutusunda **Okul** adına tıklayın veritabanı için **Tamam**
6.  Choose, veritabanı nesneleri iletişim kutusunun altında **tabloları** düğümünü **kişi**, **StudentGrade**, ve **kurs** tabloları
7.  Seçin **GetStudentGradesForCourse** işlevi bulunan altında **saklı yordamları ve işlevleri** düğümü Not, Visual Studio 2012 ile başlayarak, varlık Tasarımcısı sayesinde toplu içeri aktarma Saklı yordamları ve işlevleri
8.  Tıklayın **son**
9.  Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir. Seçtiğiniz tüm nesneleri **veritabanı nesnelerinizi seçin** iletişim kutusu, modele eklenir.
10. Varsayılan olarak, her bir içeri aktarılan saklı yordam veya işlev sonucu şeklini otomatik olarak yeni bir karmaşık tür varlık modelinizdeki olur. Ancak biz StudentGrade varlığa GetStudentGradesForCourse işlevinin sonuçlarını eşlemek istediğiniz: tasarım yüzeyi ve select sağ **Model tarayıcı** modeli tarayıcıda, seçin **işlevi içeri aktarmalar**ve çift tıklatarak **GetStudentGradesForCourse** işlevi içinde düzenleme işlevi Al iletişim kutusunda seçim **varlıkları** ve **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Kalıcı hale getirmek ve veri alma

Main yöntemi tanımlandığı dosyasını açın. Ana işlevine aşağıdaki kodu ekleyin.

Aşağıdaki kod, bir tablo değerli işlev kullanan bir sorgu oluşturmak nasıl gösterir. Sorgu sonuçları 3.5 eşit veya daha büyük bir sınıf ile ilgili öğrencilere ve ilgili kurs başlık içeren bir anonim tür içine yansıtıyor.

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

Derleme ve uygulamayı çalıştırın. Program şu çıktıyı üretir:

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Özet

Bu izlenecek yolda Entity Framework Designer kullanarak tablo değerli işlev (Tvf) map nasıl incelemiştik. Ayrıca, bir LINQ Sorgu bir TVF çağırmak nasıl gösterilmiştir.
