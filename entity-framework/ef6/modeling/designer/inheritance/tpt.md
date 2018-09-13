---
title: Tasarımcı TPT devralma - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489459"
---
# <a name="designer-tpt-inheritance"></a>Tasarımcı TPT devralma
Bu adım adım kılavuzda, Entity Framework Designer (EF Designer) kullanarak modelinize tablo başına tür (TPT) devralma uygulamak gösterilmektedir. Tablo başına tür devralma devralınan özellikler ve devralma hiyerarşisinde her türü için anahtar özellikler için verileri korumak için veritabanında ayrı bir tabloda kullanır.

Bu örnekte biz eşler **kurs** (temel türü), **OnlineCourse** (kursuna türetilir) ve **OnsiteCourse** (türetilen **Kurs**) aynı ada sahip tablolar için varlıklar. Biz bir model veritabanından oluşturacak ve ardından TPT devralma uygulamak için modeli alter.

Ayrıca ilk Model başlatabilir ve sonra da modelden veritabanı oluşturun. EF Designer TPT stratejisi varsayılan olarak kullanır ve bu nedenle herhangi bir devralma modeli tabloları ayırmak için eşleştirilecek.

## <a name="other-inheritance-options"></a>Diğer devralma seçenekleri

Tablo-başına-hiyerarşi (TPH) devralma hangi bir veritabanında tablo varlık türleri bir devralma hiyerarşisindeki tüm verileri korumak için kullanılan başka bir türdür.  Varlık Tasarımcısı tablo başına hiyerarşi kalıtım eşlemek hakkında daha fazla bilgi için bkz: [EF Designer TPH devralma](~/ef6/modeling/designer/inheritance/tph.md). 

Devralma (TPC) ve karma devralma modelleri Entity Framework çalışma zamanı tarafından desteklenir ancak EF tasarımcı tarafından desteklenmeyen türü,'somut başına tablo unutmayın. TPC veya karma devralma kullanmak istiyorsanız, iki seçeneğiniz vardır: Code First kullanın veya el ile EDMX dosyasını düzenleyin. EDMX ile çalışmayı tercih ederseniz eşleme Ayrıntıları penceresi "Güvenli Mod'da" yerleştirilir ve eşlemelerini değiştirmek için tasarımcı kullanmanız mümkün olmayacaktır.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio'nun en son sürümü.
- [School örnek veritabanını](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projesi kurun

-   Visual Studio 2012'yi açın.
-   Seçin **File -&gt; yeni -&gt; proje**
-   Sol bölmede **Visual C\#** ve ardından **konsol** şablonu.
-   Girin **TPTDBFirstSample** adı.
-   Seçin **Tamam**.

## <a name="create-a-model"></a>Model oluşturma

-   Çözüm Gezgini'nde projeye sağ tıklayıp seçin **Ekle -&gt; yeni öğe**.
-   Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.
-   Girin **TPTModel.edmx** dosya adı ve ardından **Ekle**.
-   Choose Model Contents iletişim kutusunda seçim ** Oluştur veritabanı ** kutusuna ve ardından **sonraki**.
-   Tıklayın **yeni bağlantı**.
    Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.
    Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda, tablolar düğümünü seçin **departmanı**, **kurs OnlineCourse ve OnsiteCourse** tablolar.
-   **Son**'a tıklayın.

Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir. Veritabanı nesnelerinizi seçin iletişim kutusunda seçilen tüm nesneler, modele eklenir.

## <a name="implement-table-per-type-inheritance"></a>Tablo başına tür devralma uygulama

-   Tasarım yüzeyinde, sağ **OnlineCourse** varlık türü **özellikleri**.
-   İçinde **özellikleri** penceresinde ayarlanmış temel tür özelliği **kurs**.
-   Sağ **OnsiteCourse** varlık türü **özellikleri**.
-   İçinde **özellikleri** penceresinde ayarlanmış temel tür özelliği **kurs**.
-   Arasındaki ilişkiyi (satır) sağ **OnlineCourse** ve **kurs** varlık türleri.
    Seçin **modelden silmek**.
-   Arasındaki ilişkiyi sağ **OnsiteCourse** ve **kurs** varlık türleri.
    Seçin **modelden silmek**.

Şimdi biz silecek **CourseID** özelliğinden **OnlineCourse** ve **OnsiteCourse** bu sınıfların devraldığı **CourseID** gelen **kurs** temel türü.

-   Sağ **CourseID** özelliği **OnlineCourse** varlık türü ve ardından **modelden silmek**.
-   Sağ **CourseID** özelliği **OnsiteCourse** varlık türü ve ardından **modelden Sil**
-   Tablo başına tür devralma artık uygulanır.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Kullanım modeli

Açık **Program.cs** dosya nerede **ana** yöntemi tanımlanır. Aşağıdaki kodu yapıştırın **ana** işlevi. Kod üç sorguları yürütür. İlk sorgu geri getirir tüm **kursları** belirtilen departmana ilgili. İkinci sorgu kullanan **OfType** döndürülecek yöntemi **OnlineCourses** belirtilen departmana ilgili. Üçüncü sorgunun döndürdüğü **OnsiteCourses**.

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
