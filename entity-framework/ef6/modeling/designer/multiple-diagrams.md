---
title: Model başına birden çok diyagram-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
ms.openlocfilehash: e78b927a147143629ac5b73e23edf439ba6d0264
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418296"
---
# <a name="multiple-diagrams-per-model"></a>Model başına birden çok diyagram
> [!NOTE]
> **Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

Bu video ve sayfa, Entity Framework Designer (EF Designer) kullanarak bir modelin birden çok diyagramlara nasıl bölüneceği gösterir. Modelinize bakmak veya düzenlemek için çok büyük hale geldiğinde bu özelliği kullanmak isteyebilirsiniz.

EF Designer 'ın önceki sürümlerinde, EDMX dosyası başına yalnızca bir diyagrama sahip olabilirsiniz. Visual Studio 2012 ' den itibaren, EDMX dosyanızı birden çok diyagrama bölmek için EF tasarımcısını kullanabilirsiniz.

## <a name="watch-the-video"></a>Videoyu izleme
Bu videoda, Entity Framework Designer (EF Designer) kullanılarak bir modelin birden çok diyagramla nasıl bölüneceği gösterilmektedir. Modelinize bakmak veya düzenlemek için çok büyük hale geldiğinde bu özelliği kullanmak isteyebilirsiniz.

**Sunduğu**: Julia Kornich

**Video**: [wmv](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>EF tasarımcısına genel bakış

EF Designer 'ın Varlık Veri Modeli Sihirbazı 'Nı kullanarak bir model oluşturduğunuzda, bir. edmx dosyası oluşturulur ve çözümünüze eklenir. Bu dosya, varlıklarınızın şeklini ve veritabanına nasıl eşlendiğini tanımlar.

EF Designer aşağıdaki bileşenlerden oluşur:

-   Modeli düzenlemeyle ilgili görsel tasarım yüzeyi. Varlıkları ve ilişkilendirmeleri oluşturabilir, değiştirebilir veya silebilirsiniz.
-   Modelin ağaç görünümlerini sağlayan bir **Model tarayıcı** pencere.  Varlıklar ve ilişkilendirmeleri *\[ModelName\]* klasörünün altında bulunur. Veritabanı tabloları ve kısıtlamaları *\[ModelName\]* altında bulunur. Mağaza klasörü.
-   Eşlemeleri görüntüleme ve düzenlemeyle ilgili bir **eşleme ayrıntıları** penceresi. Varlık türlerini veya ilişkilendirmelerini veritabanı tabloları, sütunları ve saklı yordamlarla eşleyebilirsiniz. 

Varlık Veri Modeli Sihirbazı tamamlandığında görsel tasarım yüzeyi penceresi otomatik olarak açılır. Model tarayıcısı görünür değilse, ana tasarım yüzeyine sağ tıklayıp **model tarayıcısı**' nı seçin.

Aşağıdaki ekran görüntüsünde, EF tasarımcısında açılan bir. edmx dosyası gösterilmektedir. Ekran görüntüsünde görsel tasarım yüzeyi (sol tarafta) ve **Model tarayıcı** pencere (sağdaki) gösterilir.

![EF Designer 2](~/ef6/media/efdesigner2.png)

EF tasarımcısında yapılan bir işlemi geri almak için CTRL-Z ' ye tıklayın.

## <a name="working-with-diagrams"></a>Diyagramlarla çalışma

Varsayılan olarak, EF Designer Diyagram1 adlı bir diyagram oluşturur. Çok sayıda varlık ve ilişkiye sahip bir diyagramınız varsa, bunları mantıksal olarak ayırmak istersiniz. Visual Studio 2012 ' den başlayarak, kavramsal modelinizi birden çok diyagramda görüntüleyebilirsiniz.   

Yeni diyagramlar eklerken Model tarayıcı penceresinde diyagramlar klasörü altında görünürler. Bir diyagramı yeniden adlandırmak için: model tarayıcısı penceresinde diyagramı seçin, ad üzerinde bir kez tıklayın ve yeni adı yazın.  Diyagram adına sağ tıklayıp **Yeniden Adlandır**' ı da seçebilirsiniz.

Diyagram adı, Visual Studio düzenleyicisinde. edmx dosya adının yanında görüntülenir. Örneğin Model1. edmx\[Diyagram1\].

![DiagramName](~/ef6/media/diagramname.png)

Diyagramlar içeriği (varlıkların ve ilişkilerin şekli ve rengi). edmx. Diagram dosyasında depolanır. Bu dosyayı görüntülemek için, Çözüm Gezgini ve. edmx dosyasının katlamayı Kaldır ' ı seçin. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

. Edmx. Diagram dosyasını el ile düzenlememelisiniz, bu dosyanın içeriğinin EF Designer tarafından üzerine yazılabilir.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Varlıkları ve Ilişkilendirmeleri yeni bir diyagrama bölme

Mevcut diyagramda varlıklar seçebilirsiniz (birden çok varlık seçmek için SHIFT tuşunu basılı tutun). Sağ fare düğmesine tıklayın ve **yeni diyagrama taşı**' yı seçin. Yeni Diyagram oluşturulur ve seçilen varlıklar ve ilişkilendirmeleri diyagrama taşınır.

Alternatif olarak, model tarayıcısı 'ndaki diyagramlar klasörüne sağ tıklayıp **Yeni Diyagram Ekle** ' yi seçebilirsiniz. Daha sonra varlıkları model tarayıcıda bulunan varlık türleri klasöründen tasarım yüzeyine sürükleyip bırakabilirsiniz.

Ayrıca, bir diyagramdan (CTRL-X veya CTRL-C tuşlarını kullanarak) varlıkları kesip kopyalayabilir ve diğer üzerinde (Ctrl-V tuşunu kullanarak) yapıştırabilirsiniz. Bir varlığı yapıştırdığınız diyagramda zaten aynı ada sahip bir varlık varsa, yeni bir varlık oluşturulur ve modele eklenir.  Örneğin: Diagram2, departman varlığını içerir. Daha sonra, Diagram2 üzerine başka bir departman yapıştırırsınız. Department1 varlığı oluşturulur ve kavramsal modele eklenir.   

İlgili varlıkları bir diyagrama eklemek için, Rick-varlığa tıklayın ve **Ilişkili Ekle**' yi seçin. Bu, ilgili varlıkların ve ilişkilerin bir kopyasını belirtilen diyagramda oluşturacak.

## <a name="changing-the-color-of-entities"></a>Varlıkların rengini değiştirme

Bir modeli birden çok diyagrama bölmenin yanı sıra varlıklarınızın renklerini de değiştirebilirsiniz.

Rengi değiştirmek için tasarım yüzeyinde bir varlık (veya birden çok varlık) seçin. Sonra sağ fare düğmesine tıklayın ve **Özellikler**' i seçin. Özellikler penceresi, **Fill Color** özelliğini seçin. Rengi geçerli bir renk adı (örneğin, kırmızı) ya da geçerli bir RGB (örneğin, 255, 128, 128) kullanarak belirtin. 

![Renk](~/ef6/media/color.png)

## <a name="summary"></a>Özet

Bu konu başlığında, bir modeli birden çok diyagramınıza bölme ve ayrıca Entity Framework Designer kullanarak bir varlık için farklı bir renk belirtme hakkında baktık. 
