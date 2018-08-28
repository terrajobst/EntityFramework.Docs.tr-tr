---
title: Uzamsal - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 39fe54ffe9d0ffd1753844f7bffcd1832d82eec5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994373"
---
# <a name="spatial---ef-designer"></a>Uzamsal - EF Designer
> [!NOTE]
> **EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Video ve adım adım kılavuzu uzamsal türler Entity Framework Designer ile eşleştirme gösterilmektedir. Ayrıca, LINQ sorgusu iki konum arasında bir uzaklık bulmak için nasıl kullanılacağını gösterir.

Bu izlenecek yol modeli ilk yeni bir veritabanı oluşturmak için kullanır, ancak EF Designer ile de kullanılabilir [veritabanı ilk](~/ef6/modeling/designer/workflows/database-first.md) mevcut bir veritabanına eşlemek için iş akışı.

Uzamsal türü desteği, Entity Framework 5'te tanıtıldı. Not uzamsal türü, sabit listeleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için .NET Framework 4.5 hedeflemesi gerekir. Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.

Uzamsal veri türleri kullanmayı da uzamsal desteğine sahip bir varlık çerçevesi sağlayıcısına kullanmanız gerekir. Bkz: [uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md) daha fazla bilgi için.

İki ana uzamsal veri türü vardır: coğrafya ve geometri. Coğrafya veri türü ellipsoidal verilerini depolayan (örneğin, GPS'i enlem ve boylam koordinatlarını). Geometri veri türü (düz) Euclidean koordinat sistemini temsil eder.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda, Entity Framework Designer uzamsal türleriyle eşleme işlemi gösterilmektedir. Ayrıca, LINQ sorgusu iki konum arasında bir uzaklık bulmak için nasıl kullanılacağını gösterir.

**Tarafından sunulan**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Ön koşullar

Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümü, bu izlenecek yolu tamamlamak için yüklü olması gerekir.

## <a name="set-up-the-project"></a>Projesi kurun

1.  Visual Studio 2012'yi açın
2.  Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**
3.  Sol bölmede **Visual C\#** ve ardından **konsol** şablonu
4.  Girin **SpatialEFDesigner** tıklayın ve proje adı olarak **Tamam**

## <a name="create-a-new-model-using-the-ef-designer"></a>Yeni EF Designer kullanarak Model oluşturma

1.  Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**
2.  Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde
3.  Girin **UniversityModel.edmx** dosya adı ve ardından **Ekle**
4.  Varlık veri modeli Sihirbazı sayfasında **boş Model** Choose Model Contents iletişim kutusunda
5.  Tıklayın **son**

Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.

Sihirbaz, aşağıdaki eylemleri gerçekleştirir:

-   Kavramsal model ve depolama modeli bunları arasındaki eşlemeyi tanımlar EnumTestModel.edmx dosyası oluşturur. Oluşturulan meta veri dosyaları bütünleştirilmiş koda gömülü için Yapıt meta veri işleme özelliğini .edmx dosyasını çıkış bütünleştirilmiş kodu Ekle ayarlar.
-   Aşağıdaki derlemelere başvuru ekler: EntityFramework System.ComponentModel.DataAnnotations ve System.Data.Entity.
-   UniversityModel.tt ve UniversityModel.Context.tt dosyaları oluşturur ve bunları altında .edmx dosyasını ekler. Bu T4 şablonu dosyaları .edmx modeli'ndeki varlıkları eşleyen POCO türleri ve türetilen DbContext türünü tanımlayan kodu oluştur

## <a name="add-a-new-entity-type"></a>Yeni bir varlık türü Ekle

1.  Select tasarım yüzeyinde boş bir alana sağ tıklayın **Ekle -&gt; varlık**, yeni varlık iletişim kutusu görünür.
2.  Belirtin **University** türü için ad ve belirtin **UniversityID** için anahtar özellik adı, türü olarak bırakın **Int32**
3.  **Tamam**’a tıklayın.
4.  Varlık sağ tıklayıp **Ekle - yeni&gt; skaler özelliği**
5.  Yeni özelliğe Yeniden Adlandır **adı**
6.  Başka bir skaler bir özellik ekleyin ve yeniden adlandırın **konumu** Özellikler penceresini açın ve yeni özelliğin türünü değiştirme **coğrafi konum**
7.  Modeli kaydedin ve projeyi derleyin
    > [!NOTE]
    > Oluşturma sırasında hata Listesi'nde eşlenmemiş varlıkları ve ilişkileri hakkında uyarılar görünebilir. Veritabanı Modeli'nden seçtikten sonra hatalar kaybolur çünkü bu uyarıları gözardı edebilirsiniz.

## <a name="generate-database-from-model"></a>Modelden veritabanı oluştur

Şimdi biz modelini temel alan bir veritabanı oluşturabilirsiniz.

1.  Varlık Tasarımcısı seçin ve yüzey üzerinde boş bir alanı sağ **Model veritabanından oluştur**
2.  Veritabanı Oluştur Sihirbazı'nın, veri bağlantısı iletişim kutusu Seç'e tıklayın görüntülenen **yeni bağlantı** belirt düğmesine **(localdb)\\ifadesini mssqllocaldb** vesunucuadıiçin **University** tıklayın ve veritabanı için **Tamam**
3.  Yeni bir veritabanı oluşturmak isteyip istemediğinizi soran bir iletişim kutusu açılır, tıklayın **Evet**.
4.  Tıklayın **sonraki** ve oluşturulan DDL bir veritabanı oluşturmak için bir tanım DDL içermeyen Ayarları iletişim kutusu Not ve Özet görüntülenir Veritabanı Oluşturma Sihirbazı'nı veri tanımlama dili (DDL) oluşturur. bir sabit listesi türü eşleyen tablo
5.  Tıklayın **son** Son'a DDL betik yürütülmez.
6.  Veritabanı Oluşturma Sihirbazı'nı aşağıdakileri yapar: açılır **UniversityModel.edmx.sql** T-SQL Düzenleyicisi oluşturur deposu şeması ve eşleme bölümlerini EDMX bağlantı dizesi bilgilerini ekler App.config dosyasına dosya
7.  T-SQL Düzenleyicisi'nde sağ fare düğmesine tıklayıp **yürütme** Connect sunucusu iletişim kutusu görüntülenirse, 2. adımdaki bağlantı bilgilerini girin ve tıklayın **Bağlan**
8.  Oluşturulan şema görüntülemek için SQL Server Nesne Gezgini içinde veritabanı adını sağ tıklatın ve seçin **Yenile**

## <a name="persist-and-retrieve-data"></a>Kalıcı hale getirmek ve veri alma

Main yöntemi tanımlandığı Program.cs dosyasını açın. Ana işlevine aşağıdaki kodu ekleyin.

Kod bağlamı için iki yeni University nesneleri ekler. Uzamsal özellikler DbGeography.FromText yöntemi kullanılarak başlatılır. WellKnownText yöntemine geçirilen olarak temsil edilen Coğrafya noktası. Kodu daha sonra verileri kaydeder. Konumunda belirtilen konuma yakın olduğu University nesnesi döndüren oluşturulur ve yürütülen ardından LINQ Sorgu.

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Derleme ve uygulamayı çalıştırın. Program şu çıktıyı üretir:

```
The closest University to you is: School of Fine Art.
```

Veritabanındaki verileri görüntülemek için SQL Server Nesne Gezgini içinde veritabanı adını sağ tıklatın ve seçin **Yenile**. Tablo ve seçim sağ fare düğmesini tıklatıp **görünüm verilerini**.

## <a name="summary"></a>Özet

Bu izlenecek yolda Entity Framework Designer kullanarak uzamsal türler eşlemeyle ilgili bilgi ve kod içinde uzamsal türler kullanma inceledik. 
