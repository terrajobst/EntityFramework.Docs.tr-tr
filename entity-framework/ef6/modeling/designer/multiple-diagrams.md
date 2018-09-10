---
title: Model - EF6 başına birden çok diyagramı
author: divega
ms.date: 2016-10-23
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: e23bf92ce3199b2162ca9a42bd0f797375179d77
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250965"
---
# <a name="multiple-diagrams-per-model"></a>Model başına birden çok diyagramı
> [!NOTE]
> **EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Bu video ve sayfa, Entity Framework Designer (EF Designer) kullanarak birden çok diyagramı bir model bölme gösterilmektedir. Modelinizi görüntülemek veya düzenlemek için çok büyük olduğunda bu özelliği kullanmak isteyebilirsiniz.

EF Designer'ın önceki sürümlerinde EDMX dosyasının yalnızca bir diyagram sahip olabilir. Visual Studio 2012'den itibaren birden çok diyagramı EDMX dosyanız bölmek için EF tasarımcısını kullanabilirsiniz.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda, Entity Framework Designer (EF Designer) kullanarak birden çok diyagramı bir model bölme işlemi gösterilmektedir. Modelinizi görüntülemek veya düzenlemek için çok büyük olduğunda bu özelliği kullanmak isteyebilirsiniz.

**Tarafından sunulan**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>EF Designer genel bakış

EF Designer varlık veri modeli Sihirbazı kullanarak model oluşturduğunuzda, bir .edmx dosyası oluşturulur ve çözümünüze eklenir. Bu dosya, varlıklarınızı ve bunların veritabanına nasıl eşleneceğine şeklini tanımlar.

EF Designer aşağıdaki bileşenlerden oluşur:

-   Modelin düzenleme için bir görsel tasarım yüzeyi. Oluşturmak, değiştirmek veya varlıkları ve ilişkileri silin.
-   A **Model tarayıcı** pencere modelin ağaç görünümlerini sağlar.  Varlıkların ve ilişkilerini altında bulunan *\[ModelName\]* klasör. Veritabanı tabloları ve kısıtlamalar altında bulunan  *\[ModelName\]*. Store klasör.
-   A **eşleşme ayrıntıları** görüntülemeyi ve düzenlemeyi eşlemeleri için penceresi. Veritabanı tabloları, sütunları ve saklı yordamlar için varlık türleri veya ilişkileri eşleyebilirsiniz. 

Varlık veri modeli Sihirbazı tamamlandığında, görsel tasarım yüzeyi penceresi otomatik olarak açılır. Model tarayıcı görünür değilse, temel bir tasarım yüzeyi ve select sağ **Model tarayıcı**.

Aşağıdaki ekran görüntüsünde EF tasarımcıda açık bir .edmx dosyası gösterir. Ekran görüntüsünde gösterilmektedir (sağdan sola) görsel tasarım yüzeyi ve **Model tarayıcı** penceresi (sağına doğru).

![EF Designer 2](~/ef6/media/efdesigner2.png)

EF tasarımcıda yapılan bir işlemin geri alınması için Ctrl-Z'a tıklayın.

## <a name="working-with-diagrams"></a>Diyagramları ile çalışma

Varsayılan olarak EF Designer Diyagram1 adlı bir diyagram oluşturur. Bir diyagram ile çok sayıda varlıkları ve ilişkileri varsa, bunları mantıksal olarak ayırmak istediğiniz en gibi. Visual Studio 2012'den itibaren birden çok diyagramlarında kavramsal model görüntüleyebilirsiniz.   

Yeni Diyagram Ekle gibi Model tarayıcı penceresinde diyagramları klasörü altında görünür. Bir diyagram yeniden adlandırılamadı: Model tarayıcı penceresinde diyagramı seçin, bir kez adına tıklayın ve yeni adı yazın.  Ayrıca diyagram adını sağ tıklatın ve seçin **Yeniden Adlandır**.

Diyagram adını, Visual Studio düzenleyicisinde .edmx dosyası adının yanında görüntülenir. Örneğin Model1.edmx\[Diyagram1\].

![DiagramName](~/ef6/media/diagramname.png)

(Şekil ve renktir varlıkları ve ilişkileri) diyagramları içeriği depolanır. edmx.diagram dosya. Bu dosyayı görüntülemek için Çözüm Gezgini'nde seçin ve .edmx dosyasını aç. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

Değil düzenlemeniz gerekir. edmx.diagram dosyasını el ile belki EF Designer tarafından üzerine bu dosyanın içeriği.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Varlıkları ve ilişkileri içine yeni bir diyagramda bölme

Varlıkları (tutun birden çok varlık seçmek için Shift) var diyagramında seçebilirsiniz. Sağ fare düğmesine tıklayıp **yeni diyagrama Taşı**. Yeni Diyagram oluşturulur ve seçili varlıkların ve ilişkilerini diyagrama taşınır.

Alternatif olarak, Model tarayıcıda diyagramları klasörü sağ tıklatın ve seçin **yeni diyagramı ekleyin.** Ardından, sürükleyin ve Model tarayıcı varlık türleri klasöründe tasarım yüzeyine gezintisinde varlıkları bırakın.

Ayrıca kesme veya da (Ctrl-X veya Ctrl-C tuşlarını kullanarak) varlıkları bir diyagramdan kopyalayın ve yapıştırın diğer (Ctrl-V anahtarını kullanarak). İçine bir varlık zaten yapıştırmakta olduğunuz diyagram aynı ada sahip bir varlık içeriyorsa, yeni bir varlık oluşturulur ve modele eklenir.  Örneğin: bölüm varlık Diyagram2'yi içerir. Ardından, başka bir bölüme üzerinde Diyagram2'yi yapıştırın. Department1 varlık oluşturulur ve kavramsal modele eklenir.   

Bir diyagramda ilişkili varlıkları içermesi için rick tıklamayla varlık ve seçin **dahil ilgili**. Bu, ilgili varlıkları ve ilişkileri bir kopyasını belirtilen şemada yapar.

## <a name="changing-the-color-of-entities"></a>Varlıkları rengini değiştirme

Birden çok diyagramı bir model bölme yanı sıra, varlıklarınızın renkleri de değiştirebilirsiniz.

Rengini değiştirmek için tasarım yüzeyinde bir varlık (veya birden çok varlık) seçin. Ardından, sağ fare düğmesine tıklayıp **özellikleri**. Özellikler penceresinde seçin **dolgu rengi** özelliği. Geçerli renk adı (örneğin, Red) ya da geçerli bir RGB (örneğin, 255, 128, 128) kullanarak rengi belirtin. 

![Renk](~/ef6/media/color.png)

## <a name="summary"></a>Özet

Bu konudaki birden çok diyagramı bir model bölme ve Entity Framework Designer kullanarak bir varlık için farklı bir renkte belirleme inceledik. 
