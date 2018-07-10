---
title: Karmaşık türler - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
caps.latest.revision: 3
ms.openlocfilehash: 6d0744921085afdafa35d137d276c54738cdd465
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912696"
---
# <a name="complex-types---ef-designer"></a>Karmaşık türler - EF Designer
Bu konuda, karmaşık türün özelliklerini içeren varlıklar için sorgulama ve karmaşık türler Entity Framework Designer (EF Designer) ile eşlemek nasıl gösterir.

EF Designer ile çalışırken, kullanılan ana windows aşağıdaki resimde gösterilmektedir.

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> Kavramsal model oluşturduğunuzda, hata Listesi'nde eşlenmemiş varlıkları ve ilişkileri hakkında uyarılar görünebilir. Modelden veritabanını oluşturmak seçtikten sonra hatalar kaybolur çünkü bu uyarıları gözardı edebilirsiniz.

## <a name="what-is-a-complex-type"></a>Karmaşık bir tür nedir

Skaler olmayan özelliklerinin skaler özellikler varlıkları düzenlenmesine olanak sağlayan varlık türler karmaşık türleridir. Varlıklar gibi karmaşık türler, skaler özellikler veya diğer karmaşık tür özellikleri oluşur.

Karmaşık türleri temsil eden nesneleri ile çalışırken, aşağıdakilere dikkat edin:

-   Karmaşık türler, anahtarları yoksa ve bu nedenle bağımsız olarak var olamaz. Karmaşık türler yalnızca varlık türleri veya diğer karmaşık türler özellikleri olarak bulunabilir.
-   Karmaşık türler ilişkilendirmeler katılamaz ve gezinti özellikleri içeremez.
-   Karmaşık tür özellikleri olamaz **null**. Bir ** InvalidOperationException ** gerçekleşir, **DbContext.SaveChanges** çağrılır ve karmaşık bir null Nesne karşılaşıldı. Skaler özellikleri karmaşık nesneler olabilir **null**.
-   Karmaşık türler, diğer karmaşık türlerden devralamaz.
-   Karmaşık tür olarak tanımlamalıdır bir **sınıfı**. 
-   EF üyeleri bir karmaşık tür nesne üzerindeki değişiklikleri algılar, **DbContext.DetectChanges** çağrılır. Varlık çerçevesi çağıran **DetectChanges** otomatik olarak, aşağıdaki üyeleri çağrılır: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Bir varlığın özelliklerini yeni karmaşık tür yeniden düzenleyin.

Bir varlık, kavramsal modelde zaten varsa, bazı özellikleri bir karmaşık tür özellikte yeniden isteyebilirsiniz.

Tasarımcı yüzeyinde birini seçin veya daha fazla özellik (Gezinti özellikleri hariç), bir varlığın sonra sağ tıklayıp **yeniden düzenleme -&gt; taşımak için yeni karmaşık tür**.

![Yeniden Düzenleme (Refactor)](~/ef6/media/refactor.png)

Seçili özelliklere sahip yeni bir karmaşık türü eklenir **Model tarayıcı**. Karmaşık tür bir varsayılan ad verilir.

Yeni oluşturulan türü karmaşık bir özellik seçili nesnenin özelliklerini değiştirir. Tüm özellik eşlemeleri korunur.

![Refactor2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Yeni bir karmaşık tür oluşturma

Var olan bir varlığa özelliklerini içermeyen yeni bir karmaşık türü de oluşturabilirsiniz.

Sağ **karmaşık türler** klasör modeli tarayıcıda işaret **AddNew karmaşık türü...** . Alternatif olarak, seçebileceğiniz **karmaşık türler** klasörü ve ENTER tuşuna **Ekle** klavyenizde anahtar.

![AddNewComplextype](~/ef6/media/addnewcomplextype.png)

Yeni bir karmaşık türü bir varsayılan ada sahip klasör eklenir. Bu gibi durumlarda, özellikleri artık türüne ekleyebilirsiniz.

## <a name="add-properties-to-a-complex-type"></a>Karmaşık bir türü özellikleri ekleyin

Karmaşık bir tür özelliklerinin skaler türler veya var olan bir karmaşık türler olabilir. Ancak, karmaşık tür özellikleri döngüsel başvurulara sahip olamaz. Örneğin, bir karmaşık tür **OnsiteCourseDetails** karmaşık türün bir özellik olamaz **OnsiteCourseDetails**.

Aşağıda listelenen şekilde karmaşık bir tür için bir özelliği ekleyebilirsiniz.

-   Model tarayıcı karmaşık tür sağ tıklatın, **Ekle**, gelin **skaler özelliği** veya **karmaşık özellik**, ardından istenen özellik türünü seçin. Alternatif olarak, bir karmaşık türü seçin ve sonra basın **Ekle** klavyenizde anahtar.  

    ![AddPropertiestoComplexType](~/ef6/media/addpropertiestocomplextype.png)

    Yeni bir özellik varsayılan ada sahip karmaşık tür eklenir.

- OR-

-   Bir varlık özelliği sağ tıklayın **EF Designer** seçin ve yüzey **kopyalama**, karmaşık türü ardından sağ tıklayarak **Model tarayıcı** seçip  **Yapıştırma**.

## <a name="rename-a-complex-type"></a>Karmaşık bir tür yeniden adlandır

Karmaşık bir tür yeniden adlandırdığınızda, tüm başvuruları türüne proje boyunca güncelleştirilir.

-   Yavaş bir karmaşık tür çift **Model tarayıcı**.
    Ad seçilir ve buna düzenleme modu.

- OR-

-   Karmaşık tür sağ **Model tarayıcı** seçip **Yeniden Adlandır**.

- OR-

-   Model tarayıcıda bir karmaşık türü seçin ve F2 tuşuna basın.

- OR-

-   Karmaşık tür sağ **Model tarayıcı** seçip **özellikleri**. Adını düzenleyin **özellikleri** penceresi.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Var olan bir karmaşık türü bir varlığa eklemek ve tablo sütunları özellikleriyle eşleyin

1.  Bir varlık sağ tıklatın, **yeni Ekle**seçip **karmaşık özellik**.
    Varsayılan ada sahip bir karmaşık tür özelliği varlık eklenir. Varsayılan türü (seçtiğiniz var olan bir karmaşık türlerinden) özelliğine atanır.
2.  İstenen türde özelliğine atayın **özellikleri** penceresi.
    Bir varlığa bir karmaşık tür özelliği ekledikten sonra özelliklerini tablo sütunları eşlemeniz gerekir.
3.  Tasarım yüzeyinde veya bir varlık türüne sağ **Model tarayıcı** seçip **Tablo eşlemeleri**.
    Tablo eşlemeleri görüntülenen **eşleşme ayrıntıları** penceresi.
4.  Genişletin **eşlendiği &lt;tablo adı&gt;**  düğümü.
    A **sütun eşlemelerini** düğümü görüntülenir.
5.  Genişletin **sütun eşlemelerini** düğümü.
    Tablodaki tüm sütunlar listesi görüntülenir. Varsayılan özellikleri (varsa) hangi sütunları eşlemeniz altında listelenen **değeri/özellik** başlığı.
6.  Eşlemek istediğiniz sütunu seçin ve ardından ilgili sağ tıklayarak **değeri/özellik** alan.
    Tüm skaler özellikler aşağı açılan listesi görüntülenir.
7.  Uygun özelliğini seçin.

    ![MapComplexType](~/ef6/media/mapcomplextype.png)

8.  Her bir tablo sütunu için 6 ve 7. adımları tekrarlayın.

>[!NOTE]
> Sütun eşlemesini silmek için eşleme ve ardından istediğiniz sütunu seçin **değeri/özellik** alan. Ardından, **Sil** aşağı açılan listeden.

## <a name="map-a-function-import-to-a-complex-type"></a>Harita karmaşık bir tür için bir işlev içeri aktarma

İşlev içeri aktarmalar saklı yordamlara dayanır. Karmaşık bir tür için bir işlev içeri aktarma eşlemek için karşılık gelen bir saklı yordam tarafından döndürülen sütun numarası karmaşık türü özelliklerini eşleşmelidir ve özellik türleri ile uyumlu depolama türleri olması gerekir.

-   Karmaşık bir türü eşlemek istediğiniz içeri aktarılan bir işlevin çift tıklayın.

    ![FunctionImports](~/ef6/media/functionimports.png)

-   Yeni işlev içeri aktarma ayarlarını aşağıdaki gibi doldurun:
    -   Bir işlev alma oluşturma saklı yordamı belirtin **saklı yordam adı** alan. Depolama modelinde tüm saklı yordamları görüntüleyen bir açılır liste alandır.
    -   İşlev alma adını **işlev adı alma** alan.
    -   Seçin **karmaşık** dönüş olarak yazın ve ardından uygun türde aşağı açılan listeden seçim yaparak karmaşık belirli dönüş türü belirtin.

        ![EditFunctionImport](~/ef6/media/editfunctionimport.png)

-   **Tamam**'ı tıklatın.
    Kavramsal modelde işlev içeri aktarma girişi oluşturulur.

### <a name="customize-column-mapping-for-function-import"></a>Sütun eşleme işlev içeri aktarma için özelleştirme

-   Model tarayıcıdaki işlev içeri aktarma sağ tıklayıp **alma işlev eşleme Mapping**.
    **Eşleme ayrıntılarını** penceresi görüntülenir ve işlev içeri aktarma için varsayılan eşlemeyi gösterir. Oklar sütun değerleri ve özellik değerlerini arasındaki eşlemeleri gösterir. Varsayılan olarak, sütun adlarını karmaşık türünün özellik adlarıyla aynı olduğu varsayılır. Varsayılan sütun adlarını gri metin olarak görüntülenir.
-   Gerekirse, sütun adlarını işlevi alma işlemi karşılık gelen saklı yordam tarafından döndürülen sütun adlarını eşleştirmek için değiştirin.

## <a name="delete-a-complex-type"></a>Karmaşık bir tür Sil

Karmaşık bir tür sildiğinizde, kavramsal modelden türü silinir ve eşlemeleri türünün tüm örnekleri için silinir. Ancak, başvuru türü için güncelleştirilmez. Örneğin, bir karmaşık tür özelliği ComplexType1 türünde bir varlık varsa ve ComplexType1 silinir **Model tarayıcı**, ilgili varlık özelliği güncelleştirilmez. Silinen bir karmaşık tür başvuran bir varlık içerdiğinden model doğrulama değil. Güncelleştirmesi veya varlık Tasarımcısı kullanarak silinen karmaşık tür başvuruları silin.

-   Model tarayıcı karmaşık tür sağ tıklayıp **Sil**.

- OR-

-   Model tarayıcıda bir karmaşık türü seçin ve klavyenizde Delete tuşuna basın.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Karmaşık türün özelliklerini içeren varlıklar için sorgu

Aşağıdaki kodu içeren bir karmaşık tür özelliği türü nesneleri varlık koleksiyonunu döndüren bir sorgu yürütme gösterilmektedir.

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
