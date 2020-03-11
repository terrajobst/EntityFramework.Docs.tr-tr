---
title: Tasarımcı TPT devralma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418212"
---
# <a name="designer-tpt-inheritance"></a>Tasarımcı TPT devralma
Bu adım adım izlenecek yol, Entity Framework Designer (EF Designer) kullanarak modelinizde tablo başına (TPT) devralmayı nasıl uygulayacağınızı gösterir. Tablo başına tür devralma, devralma hiyerarşisindeki her bir türün devralınmamış özellikleri ve anahtar özellikleri için verileri korumak üzere veritabanında ayrı bir tablo kullanır.

Bu kılavuzda, **kursu** (temel tür), **onlinekursu** (kursa türetiliyor) ve **Onsitekursu** ( **kursa**türetilir) varlıklarıyla aynı adlara sahip tablolara eşliyoruz. Veritabanından bir model oluşturacak ve ardından modeli TPT devralmayı uygulayacak şekilde değiştirecek.

Ayrıca Model First başlatabilir ve sonra veritabanını modelden oluşturabilirsiniz. EF Designer, varsayılan olarak TPT stratejisini kullanır ve böylece modeldeki Devralınanlar ayrı tablolarla eşleştirilir.

## <a name="other-inheritance-options"></a>Diğer devralma seçenekleri

Hiyerarşi başına tablo (TPH), bir devralma hiyerarşisindeki tüm varlık türlerinin verilerini korumak için bir veritabanı tablosunun kullanıldığı başka bir devralma türüdür.  Entity Desisgner hiyerarşi başına devralmayı, ile eşleme hakkında bilgi için bkz. [EF Designer TPH devralma](~/ef6/modeling/designer/inheritance/tph.md). 

Somut tür devralma (TPC) ve karışık devralma modellerinin tablo başına Entity Framework çalışma zamanı tarafından desteklendiğini ancak EF Designer tarafından desteklenmediğini unutmayın. TPC veya karışık devralma kullanmak istiyorsanız iki seçeneğiniz vardır: Code First kullanın veya EDMX dosyasını el ile düzenleyin. EDMX dosyası ile çalışmayı seçerseniz, eşleme ayrıntıları penceresi "güvenli mod" a yerleştirilir ve bu eşlemeleri değiştirmek için tasarımcıyı kullanamazsınız.

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio 'nun son sürümü.
- [Okul örnek veritabanı](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projeyi ayarlama

-   Visual Studio 2012'yi açın.
-   **Dosya&gt; yeni&gt; proje** ' yi seçin
-   Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin.
-   Ad olarak **Tptdbfirstsample** girin.
-    **Tamam ' ı**seçin.

## <a name="create-a-model"></a>Model oluşturma

-   Çözüm Gezgini projeye sağ tıklayın ve **Ekle-&gt; yeni öğe**' yi seçin.
-   Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.
-   Dosya adı için **Tptmodel. edmx** yazın ve ardından **Ekle**' ye tıklayın.
-   Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.
-    **Yeni bağlantı**' ya tıklayın.
    Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.
    Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusundaki tablolar düğümünün altında **departmanı**, **kursu, onlinekursu ve onsitekursu** tablolarını seçin.
-    **Son**' a tıklayın.

Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir. Veritabanı nesnelerinizi seçin iletişim kutusunda seçtiğiniz tüm nesneler modele eklenir.

## <a name="implement-table-per-type-inheritance"></a>Tablo türü devralma Uygula

-   Tasarım yüzeyinde **onlinekurs** varlık türüne sağ tıklayın ve **Özellikler**' i seçin.
-   **Özellikler** penceresinde, temel tür özelliğini **Kurs**olarak ayarlayın.
-   **Onsitekursu** varlık türüne sağ tıklayın ve **Özellikler**' i seçin.
-   **Özellikler** penceresinde, temel tür özelliğini **Kurs**olarak ayarlayın.
-   **Onlinekurs** ve **Kurs** varlık türleri arasındaki ilişkiye (satır) sağ tıklayın.
    **Modelden Sil**' i seçin.
-   **Onsitekursu** ve **Kurs** varlık türleri arasındaki ilişkiye sağ tıklayın.
    **Modelden Sil**' i seçin.

Artık, bu sınıflar **Kurs** temel türünden **CourseID** 'Yi devraldığı için **Onlinekursu** ve **onsitekursu** ' ndan kurs **Seid** özelliğini silecağız.

-   **Onlinekurs** varlık türünün **Kurs Seid** özelliğine sağ tıklayın ve sonra **Modelden Sil**' i seçin.
-   **Onsitekursu** varlık türünün **CourseID** özelliğine sağ tıklayın ve sonra **Modelden Sil** ' i seçin.
-   Tablo türü devralma artık uygulandı.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Modeli kullanma

**Main** yönteminin tanımlandığı **program.cs** dosyasını açın. Aşağıdaki kodu **Main** işlevine yapıştırın. Kod üç sorguyu yürütür. İlk sorgu belirtilen departmanla ilgili tüm **kursları** geri getirir. İkinci sorgu, belirtilen departmanla ilgili **Onlinekurslar** döndürmek için **OfType** metodunu kullanır. Üçüncü sorgu **Onsitekursu**döndürür.

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
