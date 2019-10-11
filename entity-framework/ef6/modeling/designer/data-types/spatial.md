---
title: Uzamsal-EF Tasarımcısı-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182511"
---
# <a name="spatial---ef-designer"></a>Uzamsal-EF Tasarımcısı
> [!NOTE]
> **Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

Video ve adım adım izlenecek yol, Entity Framework Designer uzamsal türlerin nasıl eşleneceğini gösterir. Ayrıca, iki konum arasındaki mesafeyi bulmak için bir LINQ sorgusunun nasıl kullanılacağını gösterir.

Bu izlenecek yol, yeni bir veritabanı oluşturmak için Model First kullanır, ancak aynı zamanda, var olan bir veritabanına eşlemek için [Database First](~/ef6/modeling/designer/workflows/database-first.md) iş AKıŞıYLA birlikte EF Designer da kullanılabilir.

Uzamsal tür desteği Entity Framework 5 ' te tanıtılmıştı. Uzamsal tür, numaralandırmalar ve tablo değerli işlevler gibi yeni özellikleri kullanmak için, .NET Framework 4,5 ' i hedeflemelidir. Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.

Uzamsal veri türlerini kullanmak için, uzamsal destek içeren bir Entity Framework sağlayıcısı da kullanmanız gerekir. Daha fazla bilgi için bkz. [uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md) .

İki ana uzamsal veri türü vardır: Coğrafya ve geometri. Coğrafya veri türü ellipsoidal verilerini depolar (örneğin, GPS Enlem ve boylam koordinatları). Geometri veri türü, Euclidean (düz) koordinat sistemini temsil eder.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda, Entity Framework Designer uzamsal türlerin nasıl eşleneceğini gösterir. Ayrıca, iki konum arasındaki mesafeyi bulmak için bir LINQ sorgusunun nasıl kullanılacağını gösterir.

**Sunulma ölçütü**: Julia Kornich

**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Önkoşulların önkoşulları

Bu yönergeyi tamamlamak için Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümünün yüklü olması gerekir.

## <a name="set-up-the-project"></a>Projeyi ayarlama

1.  Visual Studio 2012 'yi açın
2.  **Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.
3.  Sol bölmede, **Visual C @ no__t-1**' e tıklayın ve ardından **konsol** şablonunu seçin
4.  Projenin adı olarak **Spatialefdesigner** girin ve **Tamam 'a** tıklayın

## <a name="create-a-new-model-using-the-ef-designer"></a>EF tasarımcısını kullanarak yeni model oluşturma

1.  Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe** ' ye tıklayın.
2.  Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** seçin
3.  Dosya adı için **Üniversıtymodel. edmx** girin ve ardından **Ekle** ' ye tıklayın.
4.  Varlık Veri Modeli Sihirbazı sayfasında, model Içeriklerini Seç iletişim kutusunda **boş model** ' i seçin.
5.  **Son** ' a tıklayın

Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.

Sihirbaz aşağıdaki eylemleri gerçekleştirir:

-   Kavramsal modeli, depolama modelini ve aralarındaki eşlemeyi tanımlayan EnumTestModel. edmx dosyasını oluşturur. Oluşturulan meta veri dosyalarının derlemeye katıştırılması için. edmx dosyasının meta veri yapıt Işleme özelliğini çıktı derlemesine katıştırmak üzere ayarlar.
-   Aşağıdaki derlemelere bir başvuru ekler: EntityFramework, System. ComponentModel. Dataaçıklamalarda ve System. Data. Entity.
-   UniversityModel.tt ve UniversityModel.Context.tt dosyalarını oluşturur ve. edmx dosyasının altına ekler. Bu T4 şablon dosyaları,. edmx modelindeki varlıklarla eşlenen DbContext türetilmiş türünü ve POCO türlerini tanımlayan kodu oluşturur

## <a name="add-a-new-entity-type"></a>Yeni varlık türü Ekle

1.  Tasarım yüzeyinde boş bir alana sağ tıklayın, **Add-&gt; varlığı**' i seçin, yeni varlık iletişim kutusu görüntülenir
2.  Tür adı olarak **University** belirtin ve anahtar özellik adı Için **üniverkimliği** belirtin, türü **Int32** olarak bırakın
3.  **Tamam**’a tıklayın.
4.  Varlığa sağ tıklayın ve **New-&gt; skaler Özellik Ekle** ' yi seçin
5.  Yeni özelliği **ad** olarak yeniden adlandır
6.  Başka bir skaler özellik ekleyin ve **konuma** yeniden adlandırın Özellikler penceresi açın ve yeni özelliğin türünü **Coğrafya** olarak değiştirin
7.  Modeli kaydetme ve projeyi derleme
    > [!NOTE]
    > Oluşturduğunuzda, eşlenmemiş varlıklar ve ilişkilendirmeler hakkında uyarılar Hata Listesi görünebilir. Bu uyarıları yoksayabilirsiniz çünkü veritabanını modelden oluşturmayı seçmemiz gerekir, ancak hatalar kaybolur.

## <a name="generate-database-from-model"></a>Modelden veritabanı oluştur

Artık modeli temel alan bir veritabanı oluşturabilirsiniz.

1.  Entity Desisgner yüzeyinde boş bir alana sağ tıklayın ve **modelden veritabanı oluştur** ' u seçin.
2.  Veritabanı Oluştur sihirbazının veri bağlantısını seçin Iletişim kutusu görüntülenir **Yeni bağlantı** düğmesine tıklayın **(localdb) \\mssqllocaldb** , veritabanı Için sunucu adı ve **Üniversitesi** ve **Tamam 'a tıklayın.**
3.  Yeni bir veritabanı oluşturmak isteyip istemediğinizi soran bir iletişim kutusu açılır ve **Evet**' e tıklayın.
4.  **İleri** ' ye tıklayın ve veritabanı oluşturma Sihirbazı bir veritabanı oluşturmak için veri tanımlama DILI (ddl) oluşturur oluşturulan DDL, Özet ve ayarlar iletişim kutusu IÇINDE, DDL 'nin bu bir tablo için bir tanım içermediğini sabit listesi türü
5.  Son **' a** tıklayarak DDL betiğini yürütmez.
6.  Veritabanı oluşturma Sihirbazı şunları yapar: **Üniverbir. edmx** dosyasını açar. T-SQL DÜZENLEYICISINDE, edmx dosyasının mağaza şeması ve eşleme bölümleri, bağlantı dizesi bilgilerini App. config dosyasına ekler
7.  T-SQL Düzenleyicisi ' nde sağ fare düğmesine tıklayın ve sunucuya Bağlan iletişim kutusunu **Çalıştır** ' ı seçin. Adım 2 ' den bağlantı bilgilerini girin ve **Bağlan** ' a tıklayın.
8.  Oluşturulan şemayı görüntülemek için SQL Server Nesne Gezgini veritabanı adına sağ tıklayın ve **Yenile** ' yi seçin.

## <a name="persist-and-retrieve-data"></a>Kalıcı ve veri alma

Main yönteminin tanımlandığı Program.cs dosyasını açın. Aşağıdaki kodu Main işlevine ekleyin.

Kod, içeriğe iki yeni üniversite nesnesi ekler. Uzamsal Özellikler Dbcoğrafya. FromText yöntemi kullanılarak başlatılır. Wellknown olarak temsil edilen Coğrafya noktası yöntemine geçirilir. Kod daha sonra verileri kaydeder. Daha sonra, konumu belirtilen konuma en yakın olan bir üniversite nesnesi döndüren LINQ sorgusu oluşturulur ve yürütülür.

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

Uygulamayı derleyin ve çalıştırın. Program aşağıdaki çıktıyı üretir:

```console
The closest University to you is: School of Fine Art.
```

Veritabanındaki verileri görüntülemek için SQL Server Nesne Gezgini veritabanı adına sağ tıklayın ve **Yenile**' yi seçin. Ardından, tablodaki sağ fare düğmesine tıklayın ve **verileri görüntüle**' yi seçin.

## <a name="summary"></a>Özet

Bu izlenecek yolda, Entity Framework Designer kullanarak uzamsal türlerin nasıl eşlendiğini ve koddaki uzamsal türlerin nasıl kullanılacağını inceledik. 
