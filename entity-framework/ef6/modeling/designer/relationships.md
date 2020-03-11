---
title: İlişkiler-EF Tasarımcısı-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418247"
---
# <a name="relationships---ef-designer"></a>İlişkiler-EF Tasarımcısı
> [!NOTE]
> Bu sayfada, AŞV tasarımcısını kullanarak modelinizde ilişkiler ayarlama hakkında bilgi sağlanır. EF 'teki ilişkiler ve ilişkileri kullanarak verilere erişme ve verileri işleme hakkında genel bilgi için bkz. [ilişkiler & gezinti özellikleri](~/ef6/fundamentals/relationships.md).

İlişkilendirmeler bir modeldeki varlık türleri arasındaki ilişkileri tanımlar. Bu konu başlığı, Entity Framework Designer (EF Designer) ile ilişkilerin nasıl eşleneceğini gösterir. Aşağıdaki görüntüde, EF Designer ile çalışırken kullanılan ana pencereler gösterilmektedir.

![EF Tasarımcısı](~/ef6/media/efdesigner.png)

> [!NOTE]
> Kavramsal model oluşturduğunuzda, eşlenmemiş varlıklar ve ilişkilendirmeler hakkında uyarılar Hata Listesi görünebilir. Bu uyarıları yoksayabilirsiniz çünkü veritabanını modelden oluşturmayı seçtiğinizde hatalar kaybolur.

## <a name="associations-overview"></a>İlişkilendirmelere genel bakış

Ortamınızı EF Designer kullanarak tasarladığınızda, bir. edmx dosyası modelinizi temsil eder. . Edmx dosyasında, bir **ilişkilendirme** öğesi iki varlık türü arasında bir ilişki tanımlar. Bir ilişki, ilişkiye dahil olan varlık türlerini ve ilişkinin her ucunda çoğulluk olarak bilinen varlık türlerinin olası sayısını belirtmelidir. Bir ilişki ucunun çoğulluğu bir (1), sıfır veya bir (0.. 1) veya çok (\*) bir değere sahip olabilir. Bu bilgiler iki alt **End** öğesinde belirtilir.

Çalışma zamanında, bir ilişkilendirmenin bir sonundaki varlık türü örneklerine, gezinti özellikleri veya yabancı anahtarlar aracılığıyla erişilebilir (varlıklarınızda yabancı anahtarlar açığa çıkarmak istiyorsanız). Yabancı anahtarlar kullanıma sunulduğunda, varlıklar arasındaki ilişki bir **ReferentialConstraint** öğesi ( **Association** öğesinin bir alt öğesi) ile yönetilir. Varlıklarınızda ilişkiler için her zaman yabancı anahtarlar oluşturmanız önerilir.

> [!NOTE]
> Çoka çok (\*:\*), varlıklara yabancı anahtarlar ekleyemezsiniz. \*:\* ilişkisinde, ilişki bilgileri bağımsız bir nesne ile yönetilir.

CSDL öğeleri (**ReferentialConstraint**, **ilişkilendirme**vb.) hakkında daha fazla bilgi için bkz. [csdl belirtimi](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Ilişki oluşturma ve silme

EF Designer ile bir ilişki oluşturmak. edmx dosyasının model içeriğini günceller. İlişki oluşturduktan sonra ilişkilendirme için eşlemeler oluşturmanız gerekir (Bu konunun ilerleyen kısımlarında açıklanmıştır).

> [!NOTE]
> Bu bölümde, modelinize arasında bir ilişki oluşturmak istediğiniz varlıkları zaten eklemiş olduğunuz varsayılır.

### <a name="to-create-an-association"></a>Bir ilişki oluşturmak için

1.  Tasarım yüzeyinde boş bir alana sağ tıklayın, **Yeni Ekle**' nin üzerine gelin ve **ilişkilendirme seç...** öğesini seçin.
2.  İlişkilendirme **Ekle** iletişim kutusunda ilişkilendirmenin ayarlarını girin.

    ![Ilişki Ekle](~/ef6/media/addassociation.png)

    > [!NOTE]
    >  **Gezinti özelliğini **temizleyerek ve varlık onay kutularına **&lt;varlık türü adı&gt; yabancı anahtar özellikleri **ekleyerek, ilişkilendirmenin sonundaki varlıklara gezinti özellikleri veya yabancı anahtar özellikleri eklememe seçeneğini belirleyebilirsiniz. Yalnızca bir gezinti özelliği eklerseniz, ilişki yalnızca bir yönde Traversable olacaktır. Hiçbir gezinti özelliği yoksa, ilişkinin sonunda varlıklara erişebilmek için yabancı anahtar özellikleri eklemeyi seçmeniz gerekir.
    
3.   **Tamam**' a tıklayın.

### <a name="to-delete-an-association"></a>Bir ilişkilendirmeyi silmek için

Bir ilişkilendirmeyi silmek için aşağıdakilerden birini yapın:

-   EF Designer yüzeyinde ilişkiye sağ tıklayın ve **Sil**' i seçin.

- Veya

-   Bir veya daha fazla ilişki seçin ve DELETE tuşuna basın.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Varlıklarınızda yabancı anahtar özellikleri ekleyin (başvurusal kısıtlamalar)

Varlıklarınızda ilişkiler için her zaman yabancı anahtarlar oluşturmanız önerilir. Entity Framework bir özelliğin bir ilişki için yabancı anahtar görevi göreceğini belirlemek için bir başvuru kısıtlaması kullanır.

Bir ilişki oluştururken ***&lt;varlık türü adı&gt; varlık onay kutusuna yabancı anahtar özellikleri ekleme*** ' yi denetlediyseniz, bu başvuru kısıtlaması sizin için eklenmiştir.

Bir başvuru kısıtlaması eklemek veya düzenlemek için EF tasarımcısını kullandığınızda, EF Designer,. edmx dosyasının CSDL içeriğindeki bir **ReferentialConstraint** öğesi ekler veya değiştirir.

-   Düzenlemek istediğiniz ilişkiye çift tıklayın.
     **Başvurusal kısıtlama** iletişim kutusu görüntülenir.
-    **Asıl** açılan listesinden, başvuru kısıtlamasındaki asıl varlığı seçin.
    Varlığın anahtar özellikleri iletişim kutusundaki **asıl anahtar** listesine eklenir.
-    **Bağımlı** aşağı açılan listesinden, başvuru kısıtlamasındaki bağımlı varlığı seçin.
-   Bağımlı anahtarı olan her bir asıl anahtar için, **bağımlı anahtar** sütunundaki açılan listelerden karşılık gelen bir bağımlı anahtar seçin.

    ![Ref kısıtlaması](~/ef6/media/refconstraint.png)

-    **Tamam**' a tıklayın.

## <a name="create-and-edit-association-mappings"></a>Ilişki eşlemeleri oluşturma ve düzenleme

Bir ilişkilendirmenin, EF Designer 'ın **eşleme ayrıntıları** penceresinde veritabanına nasıl eşlendiğini belirtebilirsiniz.

> [!NOTE]
> Yalnızca bir başvuru kısıtlaması belirtilmemiş ilişkilerin ayrıntılarını eşleyebilirsiniz. Bir başvuru kısıtlaması belirtilmişse, varlığa bir yabancı anahtar özelliği dahil edilir ve yabancı anahtarın hangi sütuna eşlendiğini denetlemek için varlık için eşleme ayrıntılarını kullanabilirsiniz.

### <a name="create-an-association-mapping"></a>İlişki eşlemesi oluşturma

-   Tasarım yüzeyinde bir ilişkiye sağ tıklayın ve **Tablo eşleme**' yi seçin.
    Bu, ilişkilendirme eşlemesini **eşleme ayrıntıları** penceresinde görüntüler.
-    **Tablo veya Görünüm Ekle**' ye tıklayın.
    Depolama modelindeki tüm tabloları içeren bir açılır liste görüntülenir.
-   İlişkilendirmenin eşolacağı tabloyu seçin.
     **Eşleme ayrıntıları** penceresi, ilişkinin her iki ucunu ve her **uçta**varlık türü için anahtar özelliklerini görüntüler.
-   Her anahtar özelliği için, **sütun** alanına tıklayın ve özelliğin eşolacağı sütunu seçin.

    ![Eşleme ayrıntıları 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>İlişki eşlemesini düzenleme

-   Tasarım yüzeyinde bir ilişkiye sağ tıklayın ve **Tablo eşleme**' yi seçin.
    Bu, ilişkilendirme eşlemesini **eşleme ayrıntıları** penceresinde görüntüler.
-    **Tablo adı&gt;&lt;Için haritalar **' a tıklayın.
    Depolama modelindeki tüm tabloları içeren bir açılır liste görüntülenir.
-   İlişkilendirmenin eşolacağı tabloyu seçin.
     **Eşleme ayrıntıları** penceresi, ilişkinin her iki ucunu ve her uçta varlık türü için anahtar özelliklerini görüntüler.
-   Her anahtar özelliği için, **sütun** alanına tıklayın ve özelliğin eşolacağı sütunu seçin.

## <a name="edit-and-delete-navigation-properties"></a>Gezinti özelliklerini Düzenle ve Sil

Gezinti özellikleri, bir modeldeki ilişkilendirmenin sonundaki varlıkları bulmak için kullanılan kısayol özellikleridir. İki varlık türü arasında bir ilişki oluşturduğunuzda, gezinti özellikleri oluşturulabilir.

#### <a name="to-edit-navigation-properties"></a>Gezinti özelliklerini düzenlemek için

-   EF Designer yüzeyinde bir gezinti özelliği seçin.
    Gezinti özelliği hakkında bilgi, Visual Studio **özellikleri** penceresinde görüntülenir.
-    **Özellikler** penceresindeki özellik ayarlarını değiştirin.

#### <a name="to-delete-navigation-properties"></a>Gezinti özelliklerini silmek için

-   Kavramsal modeldeki varlık türlerinde yabancı anahtarlar gösterilmeyerek, bir gezinti özelliğinin silinmesi karşılık gelen ilişkilendirmeyi yalnızca bir yönde Traversable veya Traversable değil.
-   EF Designer yüzeyinde bir gezinti özelliğine sağ tıklayın ve **Sil**' i seçin.
