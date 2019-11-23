---
title: Tablo değerli Işlevler (TVFs)-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 35684196dcd7b708a8feeb1eca3096e8d4e555ec
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182531"
---
# <a name="table-valued-functions-tvfs"></a>Tablo değerli Işlevler (TVFs)
> [!NOTE]
> **Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

Video ve adım adım izlenecek yol, Entity Framework Designer kullanarak tablo değerli işlevlerin (TVFs) nasıl eşlendiğini gösterir. Ayrıca, bir LINQ sorgusundan bir TVF 'nin nasıl çağrılacağını gösterir.

TVFs Şu anda yalnızca Database First iş akışında desteklenmektedir.

TVF desteği Entity Framework sürüm 5 ' te tanıtılmıştı. Tablo değerli işlevler, numaralandırmalar ve uzamsal türler gibi yeni özellikleri kullanmak için .NET Framework 4,5 ' i hedeflemelidir. Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.

TVFs, tek bir anahtar farklılığı olan saklı yordamlara çok benzer: bir TVF 'nin sonucu birleştirilebilir. Bu, bir TVF 'nin sonuçlarının bir LINQ sorgusunda kullanılabileceği anlamına gelir, çünkü saklı yordamın sonuçları.

## <a name="watch-the-video"></a>Videoyu izleyin

**Sunduğu**: Julia Kornich

[Wmv](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Önkoşulların önkoşulları

Bu yönergeyi tamamlamak için şunları yapmanız gerekir:

- [Okul veritabanını](~/ef6/resources/school-database.md)yükler.

- Visual Studio 'nun yeni bir sürümüne sahip

## <a name="set-up-the-project"></a>Projeyi ayarlama

1.  Visual Studio 'Yu aç
2.  **Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.
3.  Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin
4.  Projenin adı olarak **TVF** girin ve **Tamam 'a** tıklayın

## <a name="add-a-tvf-to-the-database"></a>Veritabanına bir TVF ekleyin

-   **Görünüm-&gt; SQL Server Nesne Gezgini** seçin
-   LocalDB sunucu listesinde değilse: **SQL Server** sağ tıklayın ve **Ekle SQL Server** ' yi seçin ve LocalDB sunucusuna bağlanmak Için varsayılan **Windows kimlik doğrulamasını** kullanın
-   LocalDB düğümünü Genişlet
-   Veritabanları düğümü altında, okul veritabanı düğümüne sağ tıklayın ve yeni sorgu ' yı seçin. **..**
-   T-SQL düzenleyicisinde, aşağıdaki TVF tanımını yapıştırın

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

-   T-SQL düzenleyicisinde sağ fare düğmesine tıklayın ve **Yürüt** ' ü seçin.
-   Getstudentgradesforkurs işlevi okul veritabanına eklendi

 

## <a name="create-a-model"></a>Model oluşturma

1.  Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe** ' ye tıklayın.
2.  Sol menüden **verileri** seçin ve ardından **şablonlar** bölmesinde **ADO.net varlık veri modeli** seçin
3.  Dosya adı için **Tvfmodel. edmx** yazın ve ardından **Ekle** ' ye tıklayın.
4.  Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri** ' ye tıklayın.
5.  Sunucu adı metin kutusuna **Yeni bağlantı** gir **(localdb)\\mssqllocaldb** ' ye tıklayın, veritabanı adı için **okul** girin, **Tamam** ' a tıklayın.
6.  Veritabanı nesnelerinizi seçin iletişim kutusunda, **tablolar** düğümü altında, **kişiyi**, **studentgrad**' ı ve **Kurs** tabloları ' nı seçin.
7.   **Saklı yordamlar ve işlevler** , Visual Studio 2012 ' den başlayarak, Entity Desisgner, saklı yordamlarınızı ve işlevlerinizi Toplu içe aktarmanıza olanak sağlayan saklı yordamlar ve işlevler altında bulunan **Getstudentgradesforkurs** işlevini seçin
8.   **Son** ' a tıklayın
9.  Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.  **Veritabanı nesnelerinizi seçin** iletişim kutusunda seçtiğiniz tüm nesneler modele eklenir.
10. Varsayılan olarak, içeri aktarılan her saklı yordamın veya işlevin sonuç şekli, varlık modelinizde otomatik olarak yeni bir karmaşık tür olur. Ancak Getstudentgradesforkurs işlevinin sonuçlarını Studentgrad varlığına eşlemek istiyoruz: tasarım yüzeyine sağ tıklayın ve model **tarayıcısı 'nda model tarayıcısı '** nı seçin, **işlev içeri aktarmalar**' ı seçin ve ardından **Getstudentgradesforkurs** Işlevine çift tıklayarak Işlev içeri aktarmayı Düzenle Iletişim kutusunda **varlık** seçin ve **studentgrad** öğesini seçin.

## <a name="persist-and-retrieve-data"></a>Kalıcı ve veri alma

Main yönteminin tanımlandığı dosyayı açın. Aşağıdaki kodu Main işlevine ekleyin.

Aşağıdaki kod, tablo değerli bir Işlev kullanan bir sorgunun nasıl oluşturulacağını göstermektedir. Sorgu sonuçları, ilgili kurs başlığını ve 3,5 değerine eşit veya daha büyük bir değere sahip olan ilgili öğrencileri içeren anonim bir tür olarak projeler.

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

Uygulamayı derleyin ve çalıştırın. Program aşağıdaki çıktıyı üretir:

```console
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Özet

Bu izlenecek yolda, Entity Framework Designer kullanarak tablo değerli Işlevlerin (TVFs) nasıl eşlendiğini inceledik. Ayrıca, bir LINQ sorgusundan bir TVF çağırma gösterilmiştir.
