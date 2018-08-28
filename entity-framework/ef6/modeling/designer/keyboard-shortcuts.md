---
title: Entity Framework Designer klavye kısayolları - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: 70c9705956b58f4d00908dd9cca6ad0e0a078fc6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997769"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Entity Framework Designer klavye kısayolları
Bu sayfada Visual Studio için Entity Framework Araçları, çeşitli ekranlar kullanılabilen klavye kısayolları listesini sağlar.

## <a name="adonet-entity-data-model-wizard"></a>ADO.NET varlık veri modeli Sihirbazı

### <a name="step-one-choose-model-contents"></a>Bir adım: Model içeriğinin seçin

![WizardOne](~/ef6/media/wizardone.png)

| Kısayol  | Eylem                                                     | Notlar                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **Alt + n** | Sonraki ekrana Taşı                                        | Model içeriğinin tüm seçimleri için kullanılamaz. |
| **Alt + f** | Sihirbazı tamamlayın                                              | Model içeriğinin tüm seçimleri için kullanılamaz. |
| **Alt + w** | "Ne model içereceği?" odağını Değiştir bölme. |                                                     |

### <a name="step-two-choose-your-connection"></a>İki. adım: Bağlantınızı seçin

![WizardTwo](~/ef6/media/wizardtwo.png)

| Kısayol  | Eylem                                                     | Notlar                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **Alt + n** | Sonraki ekrana Taşı                                        |                                                         |
| **Alt + p** | Önceki ekrana Taşı                                    |                                                         |
| **Alt + w** | "Ne model içereceği?" odağını Değiştir bölme. |                                                         |
| **Alt + c** | "Bağlantı özellikleri" penceresini açın                    | Yeni bir veritabanı bağlantısı tanımını sağlar. |
| **Alt + e** | Bağlantı dizesinden hassas verileri Dışla          |                                                         |
| **Alt + ı** | Bağlantı dizesini hassas verileri eklemek            |                                                         |
| **Alt + s** | "App.Config dosyasında bağlantı ayarlarını Kaydet" seçeneğini değiştirir |                                                         |

### <a name="step-three-choose-your-version"></a>Adım üç: Sürümünüzü seçin

![WizardThree](~/ef6/media/wizardthree.png)

| Kısayol  | Eylem                                             | Notlar                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **Alt + n** | Sonraki ekrana Taşı                                |                                                                                       |
| **Alt + p** | Önceki ekrana Taşı                            |                                                                                       |
| **Alt + w** | Entity Framework sürüm seçimi odağını Değiştir | Entity Framework farklı bir sürümünü kullanmak için projeye belirtmek için izin verir. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Dört. adım: Veritabanı nesneleri ve ayarları seçin

![WizardFour](~/ef6/media/wizardfour.png)

| Kısayol  | Eylem                                                                                    | Notlar                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **Alt + f** | Sihirbazı tamamlayın                                                                             |                                                                     |
| **Alt + p** | Önceki ekrana Taşı                                                                   |                                                                     |
| **Alt + w** | Odağı veritabanı nesne Seçim Bölmesi                                            | Geriye doğru olması için veritabanı nesneleri belirterek mühendislik izin verir.    |
| **Alt + s** | İki durumlu "olarak çeviren Pluralize veya oluşturulan nesne adlarını singularize" seçeneği                       |                                                                     |
| **Alt + k** | "Modelde yabancı anahtar sütunu Ekle" seçeneğini değiştirir                              | Model içeriğinin tüm seçimleri için kullanılamaz.                 |
| **Alt + ı** | "İçeri aktarma saklı yordamları ve işlevleri varlık modele seçili" seçeneğini değiştirir | Model içeriğinin tüm seçimleri için kullanılamaz.                 |
| **Alt + m** | "Modeli Namespace" metin alanı için anahtarları odağı                                        | Model içeriğinin tüm seçimleri için kullanılamaz.                 |
| **alanı** | Öğede Seçimi Değiştir                                                               | Alt öğe varsa, tüm alt öğeleri de açılıp |
| **Sol**  | Alt ağacını Daralt                                                                       |                                                                     |
| **sağ** | Alt ağacı genişletin                                                                         |                                                                     |
| **Ayarlama**    | Önceki öğeyle ağacında gidin                                                      |                                                                     |
| **Aşağı**  | Ağacında sonraki öğeye gidin                                                          |                                                                     |

## <a name="ef-designer-surface"></a>EF Tasarımcı yüzeyi

![DesignerSurface](~/ef6/media/designersurface.png)

| Kısayol                                                                                | Eylem                      | Notlar                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Alan/girin**                                                                         | Seçimi Değiştir            | Odağı olan nesne üzerindeki seçimi değiştirir.                                                                                                                                                                                         |
| **ESC**                                                                                 | Seçimi iptal et            | Geçerli seçimi iptal eder.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Tümünü Seç                  | Tasarım yüzeyinde tüm şekiller seçer.                                                                                                                                                                                       |
| **Yukarı Ok**                                                                            | Yukarı Taşı                     | Seçili varlık bir kılavuz artırma yukarı taşır. <br/> Listede, önceki eşdüzey alt alan için taşır.                                                                                                                            |
| **Aşağı ok**                                                                          | Aşağı Taşı                   | Seçili varlık bir kılavuz artırma aşağı taşır. <br/> Listede, sonraki eşdüzey alt alan için taşır.                                                                                                                              |
| **Sol Ok**                                                                          | Sola Taşı                   | Seçili varlık sol tek bir kılavuzda artırma taşır. <br/> Listede, önceki eşdüzey alt alan için taşır.                                                                                                                          |
| **Sağ ok**                                                                         | Sağa Taşı                  | Seçili varlık sağa bir kılavuz artırma taşır. <br/> Listede, sonraki eşdüzey alt alan için taşır.                                                                                                                             |
| **Shift + Sol Ok**                                                                  | Sol boyutu şekli             | Seçilen varlığın genişliğini bir kılavuz artışla azaltır.                                                                                                                                                                     |
| **SHIFT + sağ ok**                                                                 | Boyutunu, şeklini sağ            | Bir kılavuz artışla seçilen varlığın genişliğini artırır.                                                                                                                                                                   |
| **Giriş**                                                                                | İlk eş                  | Odağı ve eş düzeyde tasarım yüzeyinde ilk nesneye seçimi.                                                                                                                                         |
| **Son**                                                                                 | Son eş                   | Odağı ve eş düzeyde tasarım yüzeyinde son nesne seçimi.                                                                                                                                          |
| **Ctrl + Home**                                                                         | İlk eş (odağı)          | İlk eş ancak odak taşıyıp seçimi yerine odağı aynıdır.                                                                                                                                                          |
| **CTRL + End**                                                                          | Son eş (odağı)           | Son aynı eş ancak odak taşıyıp seçimi yerine odak taşır.                                                                                                                                                           |
| **sekmesi**                                                                                 | Sonraki eş                   | Odağı ve sonraki nesne eş düzeyde tasarım yüzeyinde seçimi.                                                                                                                                          |
| **Shift + Sekme**                                                                           | Önceki eş               | Odağı ve eş düzeyde tasarım yüzeyinde önceki Nesne Seçimi.                                                                                                                                      |
| **Alt + Ctrl + sekme**                                                                        | Sonraki eş (odağı)           | Sonraki aynı eş ancak odak taşıyıp seçimi yerine odak taşır.                                                                                                                                                           |
| **Alt + Ctrl + Shift + Sekme**                                                                  | Önceki eş (odağı)       | Önceki eş ancak odak taşıyıp seçimi yerine odağı aynıdır.                                                                                                                                                       |
| **&lt;**                                                                                | Ascend                      | Sonraki tasarım yüzeyi bir düzey hiyerarşinin üst düzeylerindeki nesnede taşır. Bu şeklin hiyerarşideki yukarıda şekil olup olmadığını (diğer bir deyişle, nesne doğrudan tasarım yüzeyinde yerleştirilir), diyagram seçilir. |
| **&gt;**                                                                                | Düzen                     | Tasarım yüzeyi bir düzey altındaki hiyerarşideki bir sonraki kapsanan nesnesinde taşır. Kapsanan nesne yoksa, bir İşlemsiz budur.                                                                              |
| **CTRL + &lt;**                                                                         | (Odağı) ascend              | Aynı komutu, odağı seçimi olmadan ancak ascend.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Düzen (odağı)             | Aynı komutu, odağı seçimi olmadan ancak düzen.                                                                                                                                                                         |
| **SHIFT + End**                                                                         | Bağlı izleyin         | Bu varlık için bağlı olan bir varlığa yönelik bir varlıktan taşır.                                                                                                                                                               |
| **DEL**                                                                                 | Sil                      | Bir nesne veya bağlayıcı diyagramdan silin.                                                                                                                                                                                     |
| **Bileşenleri**                                                                                 | Ekleme                      | "Skaler Özellikler" bölme başlığındaki ya da bir özellik seçildiğinde yeni bir özellik için bir varlık ekler.                                                                                                           |
| **Sayfa Yukarı**                                                                               | Kaydırma diyagram           | Şu anda görünür tasarım yüzeyine yüksekliğini % 75 oranında eşit halinde, tasarım yüzeyine kaydırır.                                                                                                                    |
| **Sayfa aşağı**                                                                             | Aşağı kaydırma diyagramı         | Tasarım yüzeyinde, aşağı kaydırır.                                                                                                                                                                                                    |
| **SHIFT + Aşağı Pg**                                                                     | Sağa kaydırma diyagramı        | Tasarım yüzeyine sağa kaydırır.                                                                                                                                                                                            |
| **SHIFT + Yukarı Pg**                                                                       | Sola kaydırma diyagramı         | Tasarım yüzeyine Sola kaydırır.                                                                                                                                                                                             |
| **F2**                                                                                  | Düzenleme moduna girin             | Bir metin denetimi için düzenleme moduna girmek için standart klavye kısayolu'ni kullanın.                                                                                                                                                               |
| **Shift + F10**                                                                         | Kısayol menüsünü göster       | Seçilen öğenin kısayol menüsünü görüntülemek için standart klavye kısayolu'ni kullanın.                                                                                                                                                          |
| **Sol tıklama control + Shift + fare**  <br/> **Denetim + Shift + fare tekerleğini ilet**  | Anlam yakınlaştırma            | Fare işaretçisi altındaki diyagram görünümü alanının yakınlaştırır.                                                                                                                                                                 |
| **Sağ control + Shift + fare tıklatın** <br/> **Denetim + Shift + fare tekerleğini geriye doğru** | Anlam uzaklaştırma           | Fare işaretçisi altındaki diyagram görünümünün alanından uzaklaştırılır. İçin geçerli diyagram merkezi çok uzaktaysa uzaklaştırabilir, diyagram yeniden ortalar.                                                                          |
| **Control + üst karakter + '+'** <br/> **Control + fare tekerleğini ilet**                        | Yakınlaştır                     | Diyagram görünümü Merkezi'nde yakınlaştırır.                                                                                                                                                                                         |
| **Control + üst karakter + '-'** <br/> **Control + fare tekerleğini geriye doğru**                       | Uzaklaştır                    | Diyagram görünümü tıklandı alanından uzaklaştırılır. İçin geçerli diyagram merkezi çok uzaktaysa uzaklaştırabilir, diyagram yeniden ortalar.                                                                                            |
| **Kontrol + Shift + farenin sol düğmesiyle dikdörtgen çizme**                  | Yakınlaştırma alanının                   | Seçtiğiniz alan ortalanmış yakınlaştırır. Denetim + SHIFT tuşlarını basılı tuttuğunuzda, imleç yakınlaştırmak için alanını tanımlamak izin veren Büyüteç, değiştiğini görürsünüz.                        |
| **Bağlam menüsü tuşu + 'M '**                                                              | Eşleme ayrıntıları penceresini açma | Seçilen varlık eşlemelerini düzenlemek için eşleşme Ayrıntıları penceresi açılır                                                                                                                                                               |

## <a name="mapping-details-window"></a>Eşleme Ayrıntıları penceresi

![MappingDetailsShortcuts](~/ef6/media/mappingdetailsshortcuts.png)

| Kısayol                  | Eylem         | Notlar                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **sekmesi**                   | Anahtar bağlamı | Ana pencere alanını ve sol taraftaki araç çubuğu arasında geçiş yapar                                                                     |
| **Ok tuşları**            | Gezinti     | Ana pencere alanını Sütunlar arasında satır veya sağ ve sol yukarı ve Aşağı Taşı. Sol araç çubuğundaki düğmeler arasındaki taşıyın. |
| **Girin** <br/> **alanı** | Seçim         | Sol taraftaki araç çubuğunda bir düğmeyi seçtiğinde.                                                                                          |
| **Alt + Aşağı Ok**      | Listeyi Aç      | Açılan listeden sahip bir hücre seçildiğinde açılan bir liste.                                                                     |
| **Girin**                 | Liste seçin    | Açılan listeden bir öğe seçer.                                                                                               |
| **ESC**                   | Liste Kapat     | Açılan listeden kapatır.                                                                                                              |

## <a name="visual-studio-navigation"></a>Visual Studio Gezinti

Varlık çerçevesi de sağlayan bir dizi özel klavye kısayollarını eşlenmiş olan eylem (hiçbir kısayolları varsayılan olarak eşleştirilir). Bu özel kısayollar oluşturmak için Araçlar menüsünde sonra Seçenekleri'ni tıklatın.  Ortam altında klavye seçin.  İstediğiniz komutu seçin, kısayol "kısayol tuşlarına basın" metin kutusuna girin ve tıklayın kadar ortadaki listesinde aşağıya ilerleyin atayın. Olası kısayolları aşağıdaki gibidir:

| Kısayol                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
