---
title: Entity Framework Designer klavye kısayolları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: c75eafcca0863faa1ad64202e98b61832827377c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418520"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Klavye kısayollarını Entity Framework Designer
Bu sayfa, Visual Studio için Entity Framework Tools çeşitli ekranlarda bulunan klavye kısa larının bir listesini sağlar.

## <a name="adonet-entity-data-model-wizard"></a>ADO.NET Varlık Veri Modeli Sihirbazı

### <a name="step-one-choose-model-contents"></a>Birinci adım: model Içeriğini seçme

![Sihirbaz bir](~/ef6/media/wizardone.png)

| Kısayol  | Eylem                                                     | Notlar                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **Alt + n** | Sonraki ekrana taşı                                        | Tüm model içeriği seçimleri için kullanılamaz. |
| **Alt + f** | Sihirbazı Tamamlama                                              | Tüm model içeriği seçimleri için kullanılamaz. |
| **Alt + w** | Odağı "model neleri içerecek?" olarak değiştirin paneldeki. |                                                     |

### <a name="step-two-choose-your-connection"></a>Ikinci adım: bağlantınızı seçin

![Iki sihirbaz](~/ef6/media/wizardtwo.png)

| Kısayol  | Eylem                                                     | Notlar                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **Alt + n** | Sonraki ekrana taşı                                        |                                                         |
| **Alt + p** | Önceki ekrana git                                    |                                                         |
| **Alt + w** | Odağı "model neleri içerecek?" olarak değiştirin paneldeki. |                                                         |
| **Alt + c** | "Bağlantı özellikleri" penceresini açın                    | Yeni bir veritabanı bağlantısının tanımına izin verir. |
| **Alt + e** | Gizli verileri bağlantı dizesinden çıkar          |                                                         |
| **Alt + ı** | Gizli verileri bağlantı dizesine dahil et            |                                                         |
| **Alt + s** | "Bağlantı ayarlarını App. config içinde Kaydet" seçeneğini değiştirin |                                                         |

### <a name="step-three-choose-your-version"></a>Üçüncü adım: sürümünüzü seçin

![Sihirbazın üçü](~/ef6/media/wizardthree.png)

| Kısayol  | Eylem                                             | Notlar                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **Alt + n** | Sonraki ekrana taşı                                |                                                                                       |
| **Alt + p** | Önceki ekrana git                            |                                                                                       |
| **Alt + w** | Odağı Entity Framework sürüm seçimine geçir | Projede kullanılmak üzere farklı bir Entity Framework sürümünün belirtilmesine izin verir. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>4\. Adım: veritabanı nesnelerinizi ve ayarlarınızı seçme

![Sihirbaz dört](~/ef6/media/wizardfour.png)

| Kısayol  | Eylem                                                                                    | Notlar                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **Alt + f** | Sihirbazı Tamamlama                                                                             |                                                                     |
| **Alt + p** | Önceki ekrana git                                                                   |                                                                     |
| **Alt + w** | Odağı veritabanı nesnesi seçim bölmesine geçir                                            | Tersine mühendislik uygulanacak veritabanı nesnelerinin belirtilmesine izin verir.    |
| **Alt + s** | "Plurlaştır veya SINV üretilen nesne adlarını" seçeneğini değiştirin                       |                                                                     |
| **Alt + k** | "Modeldeki yabancı anahtar sütunlarını dahil et" seçeneğini değiştirin                              | Tüm model içeriği seçimleri için kullanılamaz.                 |
| **Alt + ı** | "Seçili saklı yordamları ve işlevleri varlık modeline aktar" seçeneğini değiştirin | Tüm model içeriği seçimleri için kullanılamaz.                 |
| **Alt + a** | Odağı "model ad alanı" metin alanına geçirir                                        | Tüm model içeriği seçimleri için kullanılamaz.                 |
| **Boşlu** | Öğe üzerinde seçimi aç                                                               | Öğesinde alt öğe varsa, tüm alt öğeler de açılır |
| **Tarafta**  | Alt ağacı Daralt                                                                       |                                                                     |
| **Right** | Alt ağacı Genişlet                                                                         |                                                                     |
| **Ayarlama**    | Ağaçtaki önceki öğeye git                                                      |                                                                     |
| **Kapatılıp**  | Ağaçtaki sonraki öğeye git                                                          |                                                                     |

## <a name="ef-designer-surface"></a>EF Designer yüzeyi

![Tasarımcı yüzeyi](~/ef6/media/designersurface.png)

| Kısayol                                                                                | Eylem                      | Notlar                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Boşluk/ENTER**                                                                         | Seçimi değiştirme            | Odak ile nesnedeki seçimi değiştirir.                                                                                                                                                                                         |
| **Larına**                                                                                 | Seçimi iptal et            | Geçerli seçimi iptal eder.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Tümünü Seç                  | Tasarım yüzeyinde tüm şekilleri seçer.                                                                                                                                                                                       |
| **Yukarı ok**                                                                            | Yukarı taşı                     | Seçili varlığı bir ızgara artışını yukarı kaydırır. <br/> Bir listede ise, önceki eşdüzey alt alanına gider.                                                                                                                            |
| **Aşağı ok**                                                                          | Aşağı taşı                   | Seçili varlığı bir ızgara artışı aşağı kaydırır. <br/> Bir listede yer alıyorsa, sonraki eşdüzey alt alan ' a gider.                                                                                                                              |
| **Sol ok**                                                                          | Sola taşı                   | Seçili varlığı bir ızgara artışını sola kaydırır. <br/> Bir listede ise, önceki eşdüzey alt alanına gider.                                                                                                                          |
| **Sağ ok**                                                                         | Sağa taşı                  | Seçili varlığı bir ızgara artışını sağa kaydırır. <br/> Bir listede yer alıyorsa, sonraki eşdüzey alt alan ' a gider.                                                                                                                             |
| **SHIFT + sol ok**                                                                  | Şekli sola Boyutlandır             | Seçilen varlığın genişliğini bir ızgara artıkında azaltır.                                                                                                                                                                     |
| **SHIFT + sağ ok**                                                                 | Şekil sağa Boyutlandır            | Seçilen varlığın genişliğini bir ızgara artışına göre arttırır.                                                                                                                                                                   |
| **Sayfa**                                                                                | İlk eş                  | Odağı ve seçimi, Tasarım yüzeyinde aynı eş düzeyinde ilk nesneye kaydırır.                                                                                                                                         |
| **Bitiş**                                                                                 | Son eş                   | Odağı ve seçimi, aynı eş düzeyindeki tasarım yüzeyinde son nesneye kaydırır.                                                                                                                                          |
| **Ctrl + Home**                                                                         | İlk eş (odak)          | İlk eş ile aynıdır, ancak odağı ve seçimi taşımak yerine odağı taşır.                                                                                                                                                          |
| **CTRL + END**                                                                          | Son eş (odak)           | Son eş ile aynı, ancak odağı ve seçimi taşımak yerine odağı taşır.                                                                                                                                                           |
| **Sekmesinde**                                                                                 | Sonraki eş                   | Odağı ve seçimi, Tasarım yüzeyinde aynı eş düzeyinde sonraki nesneye kaydırır.                                                                                                                                          |
| **SHIFT + TAB**                                                                           | Önceki eş               | Odağı ve seçimi, Tasarım yüzeyinde aynı eş düzeyinde yer alan önceki nesneye taşınır.                                                                                                                                      |
| **Alt + Ctrl + Sekme**                                                                        | Sonraki eş (odak)           | Sonraki eş ile aynıdır, ancak odağı ve seçimi taşımak yerine odağı taşır.                                                                                                                                                           |
| **Alt + CTRL + SHIFT + TAB**                                                                  | Önceki eş (odak)       | Önceki eş ile aynı, ancak odağı ve seçimi taşımak yerine odağı taşır.                                                                                                                                                       |
| **&lt;**                                                                                | Ascend                      | Tasarım yüzeyinde bir sonraki nesneye, hiyerarşide daha yüksek bir düzeye gider. Hiyerarşide bu şeklin üzerinde hiç şekil yoksa (yani, nesne doğrudan tasarım yüzeyine yerleştirilir) diyagram seçilir. |
| **&gt;**                                                                                | Azalmaz                     | Tasarım yüzeyinde, hiyerarşide bir düzey alttaki bir sonraki içerilen nesneye gider. İçerilen nesne yoksa, bu bir op değildir.                                                                              |
| **CTRL + &lt;**                                                                         | Ascend (odak)              | Ascend komutuyla aynı, ancak odağı seçim olmadan taşıdır.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Descend (odak)             | Azalmaz komutuyla aynıdır, ancak odağı seçim olmadan taşımaktadır.                                                                                                                                                                         |
| **SHIFT + End**                                                                         | Bağlı olarak takip edin         | Bir varlıktan, bu varlığın bağlandığı bir varlığa gider.                                                                                                                                                               |
| **Tuşunun**                                                                                 | Sil                      | Diyagramdan bir nesne veya bağlayıcıyı silin.                                                                                                                                                                                     |
| **Eklentiniz**                                                                                 | Ekle                      | "Skaler Özellikler" bölme üstbilgisi veya bir özelliğin kendisi seçili olduğunda varlığa yeni bir özellik ekler.                                                                                                           |
| **Sayfa yukarı**                                                                               | Diyagramı yukarı kaydır           | Tasarım yüzeyini, şu anda görünen tasarım yüzeyi yüksekliğinin %75 ' luk artışlarla kaydırır.                                                                                                                    |
| **Sayfa aşağı**                                                                             | Diyagramı aşağı kaydır         | Tasarım yüzeyini aşağı kaydırır.                                                                                                                                                                                                    |
| **SHIFT + pg aşağı**                                                                     | Diyagramı sağa kaydır        | Tasarım yüzeyini sağa kaydırır.                                                                                                                                                                                            |
| **SHIFT + pg up**                                                                       | Diyagramı sola kaydır         | Tasarım yüzeyini sola kaydırır.                                                                                                                                                                                             |
| **F2**                                                                                  | Düzenleme moduna gir             | Metin denetimi için düzenleme moduna girmek için standart klavye kısayolu.                                                                                                                                                               |
| **SHIFT + F10**                                                                         | Kısayol menüsünü görüntüle       | Seçili öğenin kısayol menüsünü görüntülemek için standart klavye kısayolu.                                                                                                                                                          |
| **Denetim + Shift + fare sol tıklama**  <br/> **Denetim + SHIFT + MouseWheel ileri**  | Anlamlı yakınlaştırma            | Fare işaretçisinin altındaki diyagram görünümünün alanını yakınlaştırır.                                                                                                                                                                 |
| **Denetim + Shift + fare sağ tıklama** <br/> **Denetim + SHIFT + MouseWheel geriye doğru** | Anlamlı yakınlaştırma           | Fare işaretçisinin altındaki diyagram görünümünün alanından uzaklaşır. Geçerli diyagram Merkezi için çok fazla büyütme yaparken diyagramı yeniden ortalar.                                                                          |
| **Denetim + SHIFT + ' + '** <br/> **Control + MouseWheel ileri**                        | Yakınlaştır                     | Diyagram görünümünün merkezini yakınlaştırın.                                                                                                                                                                                         |
| **Denetim + SHIFT + '-'** <br/> **Denetim + fare tekerleği geriye doğru**                       | Uzaklaştır                    | Diyagram görünümünün tıklanan alanından uzaklaşır. Geçerli diyagram Merkezi için çok fazla büyütme yaparken diyagramı yeniden ortalar.                                                                                            |
| **Ctrl + Shift + sol fare düğmesiyle bir dikdörtgen çiz**                  | Yakınlaştırma alanı                   | Seçtiğiniz alana göre ortalanmış şekilde yakınlaştırın. Denetim + SHIFT tuşlarını basılı tuttuğunuzda, imlecin yakınlaştırılacak alanı tanımlamanızı sağlayan bir Büyüteç Camı olarak değişiklik olduğunu görürsünüz.                        |
| **Bağlam menüsü tuşu + 'M '**                                                              | Eşleme ayrıntıları penceresini aç | Seçili varlık için eşlemeleri düzenlemek üzere eşleme ayrıntıları penceresini açar                                                                                                                                                               |

## <a name="mapping-details-window"></a>Eşleme ayrıntıları penceresi

![Eşleme ayrıntıları kısayolları](~/ef6/media/mappingdetailsshortcuts.png)

| Kısayol                  | Eylem         | Notlar                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **Sekmesinde**                   | Geçiş bağlamı | Ana pencere alanı ve soldaki araç çubuğu arasında geçiş yapar                                                                     |
| **Ok tuşları**            | Gezinme     | Ana pencere alanındaki satırları yukarı ve aşağı doğru veya sağ ve sütunlar arasında hareket ettirin. Sol taraftaki araç çubuğundaki düğmeler arasında geçiş yapın. |
| **Girmesini** <br/> **Boşlu** | Şunu seçin:         | Sol taraftaki araç çubuğunda bir düğme seçer.                                                                                          |
| **Alt + aşağı ok**      | Açık liste      | Aşağı açılan liste içeren bir hücre seçilmişse listeyi aşağı kaydırın.                                                                     |
| **Girmesini**                 | Liste Seç    | Açılan listede bir öğe seçer.                                                                                               |
| **Larına**                   | Listeyi kapat     | Açılan listeyi kapatır.                                                                                                              |

## <a name="visual-studio-navigation"></a>Visual Studio gezintisi

Entity Framework Ayrıca, özel klavye kısayollarına eşlenmiş bir dizi eylem sağlar (varsayılan olarak hiçbir kısayol eşlenmedi). Bu özel kısayolları oluşturmak için, Araçlar menüsüne ve ardından Seçenekler ' e tıklayın.  Ortam altında klavye ' yi seçin.  İstediğiniz komutu seçerek ortadaki listeyi aşağı kaydırın, "kısayol tuşlarına basın" metin kutusuna kısayolu girin ve ata ' ya tıklayın. Olası kısayollar aşağıdaki gibidir:

| Kısayol                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Add. ComplexProperty. ComplexTypes**        |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Addcodegenerationıtem**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Addfunctionımport**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. AddEnumType**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. Association**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. Complexözelliği**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. ComplexType**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. Entity**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. FunctionImport**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. devral**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. NavigationProperty**               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNew. ScalarProperty**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddNewDiagram**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. AddtoDiagram**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Close**                                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Collapse**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ConverttoEnum**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Diagram. CollapseAll**                     |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Diagram. ExpandAll**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Diagram. ExportasImage**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Diagram. LayoutDiagram**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Edit**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. EntityKey**                               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Expand**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. FunctionImportMapping**                   |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. GenerateDatabasefromModel**               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Sayfaydefinition**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Grid. ShowGrid**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Grid. SnaptoGrid**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ıncluderetildi**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MappingDetails**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ModelBrowser**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveDiagramstoSeparateFile**              |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. aşağı**                     |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. Down5**                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. ToBottom**                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. ToTop**                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. up**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MoveProperties. Up5**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. MovetonewDiagram**                        |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Open**                                    |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. yeniden Düzenle. MovetoNewComplexType**           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. yeniden Düzenle. Rename**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. RemovefromDiagram**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Rename**                                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ScalarPropertyFormat. DisplayName**        |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. ScalarPropertyFormat. DisplayNameandType** |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. BaseType**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. Entity**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. Property**                         |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Select. SubType**                          |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. SelectAll**                               |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. SelectAssociation**                       |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Showwindiagram**                           |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Showwınmodelbrowser**                      |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. StoredProcedureMapping**                  |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. TableMapping**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. UpdateModelfromDatabase**                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. Validate**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 10**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 100**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 125**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 150**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 200**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 25**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 300**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 33**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 400**                                |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 50**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 66**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. 75**                                 |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. Custom**                             |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. ZoomIn**                             |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. ZoomOut**                            |
| **OtherContextMenus. MicrosoftDataEntityDesignContext. zoom. ZoomtoFit**                          |
| **View. EntityDataModelBrowser**                                                                |
| **View. EntityDataModelMappingDetails**                                                         |
