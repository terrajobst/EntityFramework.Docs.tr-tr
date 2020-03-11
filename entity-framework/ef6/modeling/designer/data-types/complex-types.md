---
title: Karmaşık türler-EF Tasarımcısı-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418653"
---
# <a name="complex-types---ef-designer"></a>Karmaşık türler-EF Tasarımcısı
Bu konu, karmaşık türlerin Entity Framework Designer (EF Designer) ile nasıl eşleneceğini ve karmaşık türün özelliklerini içeren varlıkların nasıl sorgulanalınacağını gösterir.

Aşağıdaki görüntüde, EF Designer ile çalışırken kullanılan ana pencereler gösterilmektedir.

![EF Tasarımcısı](~/ef6/media/efdesigner.png)

> [!NOTE]
> Kavramsal model oluşturduğunuzda, eşlenmemiş varlıklar ve ilişkilendirmeler hakkında uyarılar Hata Listesi görünebilir. Bu uyarıları yoksayabilirsiniz çünkü veritabanını modelden oluşturmayı seçtiğinizde hatalar kaybolur.

## <a name="what-is-a-complex-type"></a>Karmaşık tür nedir?

Karmaşık türler, skalar özelliklerin varlıklar içinde düzenlenmesine olanak sağlayan varlık türlerinin skalar olmayan özellikleridir. Varlıklar gibi karmaşık türler de skalar özelliklerden veya diğer karmaşık tür özelliklerinden oluşur.

Karmaşık türleri temsil eden nesnelerle çalışırken, aşağıdakilere dikkat edin:

-   Karmaşık türlerde anahtarlar yoktur ve bu nedenle bağımsız olarak bulunamaz. Karmaşık türler yalnızca varlık türlerinin veya diğer karmaşık türlerin özellikleri olarak bulunabilir.
-   Karmaşık türler ilişkilendirmelere katılamaz ve gezinti özellikleri içeremez.
-   Karmaşık tür özellikleri **null**olamaz. **DbContext. SaveChanges** çağrıldığında ve null karmaşık bir nesneyle karşılaşıldığında **InvalidOperationException **oluşur. Karmaşık nesnelerin skaler özellikleri **null**olabilir.
-   Karmaşık türler diğer karmaşık türlerden devralınabilir.
-   Karmaşık türü bir **sınıf**olarak tanımlamanız gerekir. 
-   EF, **DbContext. DetectChanges** çağrıldığında karmaşık tür nesnesindeki üyelerin değişikliklerini algılar. Entity Framework, aşağıdaki Üyeler çağrıldığında **DetectChanges** öğesini otomatik olarak çağırır: **Dbset. Find**, **dbset. Local**, **Dbset. Remove**, **dbset. Add**, **dbset. Attach**, **DbContext. SaveChanges**, **DbContext. getvalidationerrors**, **DbContext. Entry**, **dbchangetracker. Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Varlığın özelliklerini yeni karmaşık türe yeniden düzenleme

Kavramsal modelinizde zaten bir varlık varsa, bazı özelliklerden karmaşık tür özelliklerine yeniden düzenleme yapmak isteyebilirsiniz.

Tasarımcı yüzeyinde bir varlığın bir veya daha fazla özelliğini (gezinme özellikleri hariç) seçin, sonra sağ tıklayıp yeniden&gt; Düzenle ' yi seçerek **Yeni karmaşık türe taşıyın**.

![Yeniden Düzenleme (Refactor)](~/ef6/media/refactor.png)

**Model tarayıcıya**seçili özelliklere sahip yeni bir karmaşık tür eklenmiştir. Karmaşık türe varsayılan bir ad verilir.

Yeni oluşturulan türün karmaşık bir özelliği seçilen özelliklerin yerini alır. Tüm özellik eşlemeleri korunur.

![Yeniden Düzenle 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Yeni bir karmaşık tür oluşturun

Ayrıca, var olan bir varlığın özelliklerini içermeyen yeni bir karmaşık tür oluşturabilirsiniz.

Model tarayıcısında **karmaşık türler** klasörüne sağ tıklayın, **AddNew karmaşık türü..** . seçeneğine gelin. Alternatif olarak, **karmaşık türler** klasörünü seçip klavyenizdeki **Insert** tuşuna basabilirsiniz.

![Yeni karmaşık tür Ekle](~/ef6/media/addnewcomplextype.png)

Klasöre varsayılan adla yeni bir karmaşık tür eklenir. Artık türüne özellikler ekleyebilirsiniz.

## <a name="add-properties-to-a-complex-type"></a>Karmaşık bir türe özellikler ekleme

Karmaşık bir türün özellikleri skaler türler veya var olan karmaşık türler olabilir. Ancak, karmaşık tür özelliklerinin döngüsel başvuruları olamaz. Örneğin, bir karmaşık tür **onsitecoursedetails** karmaşık türde **onsitecoursedetails**özelliğine sahip olamaz.

Bir özelliği, aşağıda listelenen yollarla karmaşık bir türe ekleyebilirsiniz.

-   Model tarayıcısında bir karmaşık türe sağ tıklayın, **Ekle**' nin üzerine gelin, sonra da **skaler Özellik** veya **karmaşık özellik**' in üzerine gelin ve istediğiniz özellik türünü seçin. Alternatif olarak, bir karmaşık tür seçebilir ve klavyenizdeki **ınsert** tuşuna basabilirsiniz.  

    ![Karmaşık türe özellikler ekleme](~/ef6/media/addpropertiestocomplextype.png)

    Varsayılan bir ada sahip karmaşık türe yeni bir özellik eklenir.

- Veya

-   **EF Designer** yüzeyinde bir varlık özelliğine sağ tıklayın ve **Kopyala**' yı seçin, ardından **model tarayıcısında** karmaşık türe sağ tıklayıp **Yapıştır**' ı seçin.

## <a name="rename-a-complex-type"></a>Karmaşık bir türü yeniden adlandırma

Karmaşık bir türü yeniden adlandırdığınızda, türe yapılan tüm başvurular proje genelinde güncellenir.

-   **Model tarayıcısında**bir karmaşık türe yavaş bir çift tıklayın.
    Ad, düzenleme modunda seçilir ve görüntülenir.

- Veya

-   **Model tarayıcısında** bir karmaşık türe sağ tıklayıp **Yeniden Adlandır**' ı seçin.

- Veya

-   Model tarayıcısında bir karmaşık tür seçin ve F2 tuşuna basın.

- Veya

-   **Model tarayıcısında** karmaşık bir türe sağ tıklayın ve **Özellikler**' i seçin.   **Özellikler** penceresinde adı düzenleyin.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Bir varlığa var olan bir karmaşık türü ekleyin ve özelliklerini tablo sütunlarına eşleyin

1.  Bir varlığa sağ tıklayın, **Yeni Ekle**' nin üzerine gelin ve **karmaşık özellik**' i seçin.
    Varlığa varsayılan adda bir karmaşık tür özelliği eklenir. Varsayılan bir tür (varolan karmaşık türlerden seçilen) özelliğine atanır.
2.  İstenen türü **özellikler** penceresindeki özelliğe atayın.
    Bir varlığa karmaşık bir tür özelliği eklendikten sonra, özelliklerini tablo sütunlarına eşlemeniz gerekir.
3.  Tasarım yüzeyinde veya **model tarayıcısı** bir varlık türüne sağ tıklayın ve **tablo eşlemeleri**' ni seçin.
    Tablo eşlemeleri **eşleme ayrıntıları** penceresinde görüntülenir.
4.   **Haritaları &lt;tablo adı&gt;**  düğümünü genişletin.
    Bir **sütun eşlemeleri** düğümü görüntülenir.
5.   **Sütun eşlemelerini** düğümünü genişletin.
    Tablodaki tüm sütunların bir listesi görüntülenir. Sütun haritasının, **değer/özellik** başlığı altında listelendiği varsayılan Özellikler (varsa).
6.  Eşlemek istediğiniz sütunu seçin ve ardından karşılık gelen **değer/özellik** alanına sağ tıklayın.
    Tüm skaler özelliklerin açılan listesi görüntülenir.
7.  Uygun özelliği seçin.

    ![Eşleme karmaşık türü](~/ef6/media/mapcomplextype.png)

8.  Her tablo sütunu için 6 ve 7. adımları yineleyin.

>[!NOTE]
> Bir sütun eşlemesini silmek için, eşlemek istediğiniz sütunu seçin ve ardından **değer/özellik** alanına tıklayın. Sonra, açılan listeden **Sil** ' i seçin.

## <a name="map-a-function-import-to-a-complex-type"></a>Bir Işlev Içeri aktarmayı karmaşık bir türe eşleyin

İşlev içeri aktarmaları, saklı yordamlara dayalıdır. Bir işlev içeri aktarmayı karmaşık bir türe eşlemek için, karşılık gelen saklı yordam tarafından döndürülen sütunlar sayı olarak karmaşık türün özellikleriyle eşleşmelidir ve özellik türleriyle uyumlu depolama türlerine sahip olmalıdır.

-   Karmaşık bir türe eşlemek istediğiniz içeri aktarılan işleve çift tıklayın.

    ![İşlev Içeri aktarmaları](~/ef6/media/functionimports.png)

-   Yeni işlev içeri aktarma ayarlarını aşağıdaki gibi girin:
    -    **Saklı yordam adı** alanında bir işlev içeri aktarması oluşturduğunuz saklı yordamı belirtin. Bu alan, depolama modelindeki tüm saklı yordamları görüntüleyen bir açılan liste listesidir.
    -   İşlev **içeri aktarma adı** alanında işlev içeri aktarma adını belirtin.
    -   Dönüş türü olarak **karmaşık** seçin ve ardından açılan listeden uygun türü seçerek belirli karmaşık dönüş türünü belirtin.

        ![Işlev Içeri aktarmayı Düzenle](~/ef6/media/editfunctionimport.png)

-    **Tamam**' a tıklayın.
    İşlev içeri aktarma girişi kavramsal modelde oluşturulur.

### <a name="customize-column-mapping-for-function-import"></a>Işlev Içeri aktarması için sütun eşlemeyi özelleştirme

-   Model tarayıcısında içeri aktarma işlevine sağ tıklayın ve **Işlev Içeri aktarma eşlemesi**' ni seçin.
     **Eşleme ayrıntıları** penceresi görünür ve işlev içeri aktarma için varsayılan eşlemeyi gösterir. Oklar, sütun değerleri ve özellik değerleri arasındaki eşlemeleri gösterir. Varsayılan olarak, sütun adlarının karmaşık türün Özellik adlarıyla aynı olduğu varsayılır. Varsayılan sütun adları gri metinde görünür.
-   Gerekirse, sütun adlarını, işlev içeri aktarmaya karşılık gelen saklı yordamın döndürdüğü sütun adlarıyla eşleşecek şekilde değiştirin.

## <a name="delete-a-complex-type"></a>Karmaşık bir türü silme

Karmaşık bir türü sildiğinizde tür kavramsal modelden silinir ve türün tüm örnekleri için eşlemeler silinir. Ancak, türe yapılan başvurular güncellenmez. Örneğin, bir varlığın ComplexType1 türünde karmaşık bir tür özelliği varsa ve ComplexType1 **model tarayıcısında**silinirse, karşılık gelen varlık özelliği güncellenmez. Silinen bir karmaşık türe başvuran bir varlık içerdiğinden model doğrulanmayacak. Entity Desisgner kullanarak silinen karmaşık türlere başvuruları güncelleştirebilir veya silebilirsiniz.

-   Model tarayıcısında karmaşık bir türe sağ tıklayın ve **Sil**' i seçin.

- Veya

-   Model tarayıcısında bir karmaşık tür seçin ve klavyenizdeki DELETE tuşuna basın.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Karmaşık türün özelliklerini Içeren varlıklar için sorgu

Aşağıdaki kod, karmaşık tür özelliği içeren varlık türü nesnelerinin bir koleksiyonunu döndüren bir sorgunun nasıl yürütüleceğini gösterir.

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
