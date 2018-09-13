---
title: İlişkiler - EF Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490689"
---
# <a name="relationships---ef-designer"></a>İlişkiler - EF Designer
> [!NOTE]
> Bu sayfa, ilişkileri EF Designer kullanarak modelinizdeki ayarlama hakkında bilgi sağlar. EF ve erişmek ve ilişkileri kullanarak verileri işlemek nasıl ilişkiler hakkında genel bilgi için bkz. [ilişkileri ve gezinti özellikleri](~/ef6/fundamentals/relationships.md).

Bir modeldeki varlık türleri arasındaki ilişkileri ilişkileri tanımlayın. Bu konuda, Entity Framework Designer (EF Designer) ilişkilerini eşleme gösterilmektedir. EF Designer ile çalışırken, kullanılan ana windows aşağıdaki resimde gösterilmektedir.

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> Kavramsal model oluşturduğunuzda, hata Listesi'nde eşlenmemiş varlıkları ve ilişkileri hakkında uyarılar görünebilir. Modelden veritabanını oluşturmak seçtikten sonra hatalar kaybolur çünkü bu uyarıları gözardı edebilirsiniz.

## <a name="associations-overview"></a>İlişkilendirmeleri genel bakış

EF Designer kullanarak modelinizi tasarlarken, bir .edmx dosyası modelinizi temsil eder. .Edmx dosyası içinde bir **ilişkilendirme** öğe iki varlık türleri arasındaki bir ilişkiyi tanımlar. İlişkilendirmesine katılan varlık türleri ve varlık türleri çeşitliliği bilinen ilişkinin her iki ucunda olası sayısını belirtmeniz gerekir. Bir ilişkilendirme end'ün çoğulluğunun bir değer bir (1) sıfır veya bir (0..1) ya da birden çok olabilir (\*). Bu bilgiler, iki alt belirtilen **son** öğeleri.

Ürün (varlıklarınızı yabancı anahtarları kullanıma sunmak isterseniz) çalışma zamanında, varlık türü örneklerinin bir ilişkilendirmenin bir ucunda Gezinti özellikleri veya yabancı anahtarlar erişilebilir. Kullanıma sunulan yabancı anahtarlar ile varlıklar arasında ilişki ile yönetilen bir **Referentialconstraint'teki** öğesi (alt öğesi **ilişkilendirme** öğesi). Yabancı anahtar ilişkileri için her zaman içinde varlıklarınızı kullanıma önerilir.

> [!NOTE]
> Çoktan çoğa içinde (\*:\*) varlıkları için yabancı anahtarlar ekleyemezsiniz. İçinde bir \*:\* ilişki, ilişki bilgilerini, bağımsız bir nesne ile yönetilir.

CSDL öğeleri hakkında bilgi için (**Referentialconstraint'teki**, **ilişkilendirme**, vb.) görmek [CSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Oluşturma ve ilişkileri silme

EF Designer güncelleştirmeleri ile ilişkilendirme .edmx dosyasını modeli içeriğini oluşturma. Bir ilişkilendirme oluşturduktan sonra eşlemeleri (Bu konunun ilerleyen bölümlerinde açıklanmıştır) ilişkisi oluşturmanız gerekir.

> [!NOTE]
> Bu bölümde, modeldeki arasında bir ilişki oluşturmak istediğiniz varlıkları zaten eklenmiş varsayılır.

### <a name="to-create-an-association"></a>Bir ilişkilendirme oluşturmak için

1.  Tasarım yüzeyinde boş bir alana sağ tıklayın, fareyle **yeni Ekle**seçip **ilişki...** .
2.  İlişkilendirmeyi ayarlarını doldurun **ekleme ilişkilendirme** iletişim.

    ![İlişkilendirme ekleyin](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Gezinti özellikleri veya yabancı anahtar özelliklerini ilişkilendirmenin bir ucunda varlıklara temizleyerek eklememeyi seçebilirsiniz ** gezinti özelliği ** ve ** yabancı anahtar özellikleri &lt;varlık türü adı&gt; varlık ** onay kutuları. Yalnızca bir gezinti özelliği eklerseniz, ilişki sadece tek yöndedir traversable olacaktır. Gezinti özelliği eklerseniz, ilişkilendirmenin bir ucunda varlıklara erişmek için yabancı anahtar özelliklerini eklemek seçmeniz gerekir.
    
3.  **Tamam**'ı tıklatın.

### <a name="to-delete-an-association"></a>Bir ilişkiyi silmek için

Bir ilişkilendirme aşağıdakilerden birini yapın silmek için:

-   EF Tasarımcı yüzeyi ve select ilişkilendirmenin sağ **Sil**.

- OR-

-   Bir veya daha fazla ilişkilendirmesi'ni seçin ve DELETE tuşuna basın.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Yabancı anahtar özelliklerini varlıklarınızı (başvuru kısıtlamalarını) içerir.

Yabancı anahtar ilişkileri için her zaman içinde varlıklarınızı kullanıma önerilir. Entity Framework başvurusal Kısıt bir ilişki için yabancı anahtar olarak davranan bir özelliği tanımlamak için kullanır.

İşaretlediyseniz ***yabancı anahtar özellikleri &lt;varlık türü adı&gt; varlık*** onay kutusu ilişki oluştururken bu başvuru kısıtlamasını sizin için eklendi.

EF Designer eklemek veya bir başvuru kısıtlamasını düzenlemek için EF Designer'ı kullandığınızda, ekler veya değiştirir bir **Referentialconstraint'teki** .edmx dosyasını CSDL içeriğini öğesinde.

-   Düzenlemek istediğiniz ilişkilendirme çift tıklayın.
    **Başvuru kısıtlamasını** iletişim kutusu görüntülenir.
-   Gelen **asıl** aşağı açılan listesinde, başvuru kısıtlamasındaki asıl varlığı seçin.
    Varlığın anahtar özellikler eklenir **sorumlusu anahtarı** iletişim kutusunda listesi.
-   Gelen **bağımlı** aşağı açılan listesinde, başvuru kısıtlamasındaki bağımlı varlığı seçin.
-   Bağımlı bir anahtara sahip her asıl anahtarı için aşağı açılan listelerden karşılık gelen bir bağımlı anahtarı seçin. **bağımlı anahtarı** sütun.

    ![Başvuru kısıtlaması](~/ef6/media/refconstraint.png)

-   **Tamam**'ı tıklatın.

## <a name="create-and-edit-association-mappings"></a>Oluşturma ve ilişkilendirme eşlemelerini düzenleme

Bir ilişkilendirme veritabanına eşlemelerini nasıl belirtebilirsiniz **eşleşme ayrıntıları** EF Designer'ın penceresi.

> [!NOTE]
> Yalnızca belirtilen başvurusal Kısıt olmayan ilişkilendirmeleri ayrıntılarını eşleyebilirsiniz. Başvurusal Kısıt belirtilirse yabancı anahtar özellik varlıkta bulunan ve varlık yabancı anahtarı hangi sütunun eşlendiği denetlemek için eşleşme ayrıntıları kullanabilirsiniz.

### <a name="create-an-association-mapping"></a>Bir ilişkilendirme eşlemesi oluşturma

-   İlişkilendirme tasarım yüzeyi ve seçin, sağ **Tablo eşleme**.
    Bu ilişkilendirme eşlemede görüntüler **eşleme ayrıntılarını** penceresi.
-   Tıklayın **bir tablo veya Görünüm Ekle**.
    Depolama modelinde tüm tabloları içeren bir açılır liste görünür.
-   İlişkilendirme eşler tabloyu seçin.
    **Eşleşme ayrıntıları** pencere görüntüler her iki ucunda da ilişki ve varlık türü için anahtar özellikler her **son**.
-   Her bir anahtar özellik için tıklatın **sütun** alan ve özelliği eşlemek istediğiniz sütunu seçin.

    ![Eşleme ayrıntıları 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Bir ilişkilendirme eşlemeyi Düzenle

-   İlişkilendirme tasarım yüzeyi ve seçin, sağ **Tablo eşleme**.
    Bu ilişkilendirme eşlemede görüntüler **eşleme ayrıntılarını** penceresi.
-   Tıklayın **eşlendiği &lt;tablo adı&gt;**.
    Depolama modelinde tüm tabloları içeren bir açılır liste görünür.
-   İlişkilendirme eşler tabloyu seçin.
    **Eşleşme ayrıntıları** penceresi, her iki ucunda da ilişki ve varlık türü için anahtar özellikler her sonunda görüntüler.
-   Her bir anahtar özellik için tıklatın **sütun** alan ve özelliği eşlemek istediğiniz sütunu seçin.

## <a name="edit-and-delete-navigation-properties"></a>Düzenle ve Sil Gezinti özellikleri

Gezinti özellikleri bir modelde ilişkilendirme ucunda varlıkları bulmak için kullanılan kısayol özelliklerdir. İki varlık türleri arasındaki ilişkiyi oluşturduğunuzda, gezinti özellikleri oluşturulabilir.

#### <a name="to-edit-navigation-properties"></a>Gezinti özelliklerini düzenlemek için

-   EF Designer yüzeyinde bir gezinti özelliği seçin.
    Gezinti özelliği hakkında bilgi, Visual Studio'da görüntülenir **özellikleri** penceresi.
-   Özellik ayarlarını değiştirme **özellikleri** penceresi.

#### <a name="to-delete-navigation-properties"></a>Gezinti özellikleri silmek için

-   Yabancı anahtarlar kavramsal modelin varlık türlerini gösterilmediğinden, bir gezinti özelliği silme karşılık gelen ilişki traversable tek bir yönde veya değil traversable hiç kalmasına neden olabilir.
-   EF Tasarımcı yüzeyi ve select Gezinti özelliğindeki sağ **Sil**.
