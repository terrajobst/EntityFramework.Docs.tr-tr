---
title: Numaralandırma desteği - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: a94a497e8c5b3213dd7eb4215de90164d437507d
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250641"
---
# <a name="enum-support---ef-designer"></a>Numaralandırma desteği - EF Designer
> [!NOTE]
> **EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Bu video ve adım adım izlenecek yol, Entity Framework Designer ile numaralandırma türleri kullanmayı gösterir. Ayrıca, bir LINQ Sorgu numaralandırmalar kullanmayı gösterir.

Bu izlenecek yol modeli ilk yeni bir veritabanı oluşturmak için kullanır, ancak EF Designer ile de kullanılabilir [veritabanı ilk](~/ef6/modeling/designer/workflows/database-first.md) mevcut bir veritabanına eşlemek için iş akışı.

Numaralandırma desteği, Entity Framework 5'te tanıtıldı. Numaralandırmalar, uzamsal veri türleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için .NET Framework 4.5 hedeflemesi gerekir. Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.

Varlık Çerçevesi'nde bir numaralandırma aşağıdaki temel alınan türler sahip olabilir: **bayt**, **Int16**, **Int32**, **Int64** , veya **SByte**.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda, Entity Framework Designer ile numaralandırma türleri kullanmayı gösterir. Ayrıca, bir LINQ Sorgu numaralandırmalar kullanmayı gösterir.

**Tarafından sunulan**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Ön koşullar

Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümü, bu izlenecek yolu tamamlamak için yüklü olması gerekir.

## <a name="set-up-the-project"></a>Projesi kurun

1.  Visual Studio 2012'yi açın
2.  Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**
3.  Sol bölmede **Visual C\#** ve ardından **konsol** şablonu
4.  Girin **EnumEFDesigner** tıklayın ve proje adı olarak **Tamam**

## <a name="create-a-new-model-using-the-ef-designer"></a>Yeni EF Designer kullanarak Model oluşturma

1.  Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**
2.  Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde
3.  Girin **EnumTestModel.edmx** dosya adı ve ardından **Ekle**
4.  Varlık veri modeli Sihirbazı sayfasında **boş Model** Choose Model Contents iletişim kutusunda
5.  Tıklayın **son**

Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.

Sihirbaz, aşağıdaki eylemleri gerçekleştirir:

-   Kavramsal model ve depolama modeli bunları arasındaki eşlemeyi tanımlar EnumTestModel.edmx dosyası oluşturur. Oluşturulan meta veri dosyaları bütünleştirilmiş koda gömülü için Yapıt meta veri işleme özelliğini .edmx dosyasını çıkış bütünleştirilmiş kodu Ekle ayarlar.
-   Aşağıdaki derlemelere başvuru ekler: EntityFramework System.ComponentModel.DataAnnotations ve System.Data.Entity.
-   EnumTestModel.tt ve EnumTestModel.Context.tt dosyaları oluşturur ve bunları altında .edmx dosyasını ekler. Bu T4 şablonu dosyaları .edmx modeli'ndeki varlıkları eşleyen POCO türleri ve türetilen DbContext türünü tanımlayan bir kod oluşturur.

## <a name="add-a-new-entity-type"></a>Yeni bir varlık türü Ekle

1.  Select tasarım yüzeyinde boş bir alana sağ tıklayın **Ekle -&gt; varlık**, yeni varlık iletişim kutusu görünür.
2.  Belirtin **departmanı** türü için ad ve belirtin **DepartmentID** için anahtar özellik adı, türü olarak bırakın **Int32**
3.  **Tamam**’a tıklayın.
4.  Varlık sağ tıklayıp **Ekle - yeni&gt; skaler özelliği**
5.  Yeni özelliğe Yeniden Adlandır **adı**
6.  Yeni özelliğin türünü değiştirmek **Int32** (varsayılan olarak, yeni özellik dize türüdür) türünü değiştirmek için Özellikler penceresini açmak ve değiştirmek için Type özelliği **Int32**
7.  Başka bir skaler bir özellik ekleyin ve yeniden adlandırın **bütçe**, türe çeviremezsiniz **ondalık**

## <a name="add-an-enum-type"></a>Enum Türü Ekle

1.  Entity Framework Tasarımcısı'nda ad özelliği sağ tıklayın, **enum için Dönüştür**

    ![Numaralandırmaya Dönüştür](~/ef6/media/converttoenum.png)

2.  İçinde **sabit listesi Ekle** iletişim kutusuna **DepartmentNames** Enum tür adı için temel alınan türe çeviremezsiniz **Int32**, ve ardından türü aşağıdaki üyeleri Ekle: İngilizce, Matematik ve ekonomik

    ![Enum Türü Ekle](~/ef6/media/addenumtype.png)

3.  Tuşuna **Tamam**
4.  Modeli kaydedin ve projeyi derleyin
    > [!NOTE]
    > Oluşturma sırasında hata Listesi'nde eşlenmemiş varlıkları ve ilişkileri hakkında uyarılar görünebilir. Veritabanı Modeli'nden seçtikten sonra hatalar kaybolur çünkü bu uyarıları gözardı edebilirsiniz.

Özellikler penceresinde bakarsanız, Name özelliğinin türü için değiştirildiğini fark edeceksiniz **DepartmentNames** ve yeni eklenen numaralandırma türüne türlerini listesine eklendi.

Model tarayıcı penceresine geçiş yapıyorsanız, türü, sabit listesi türleri düğüme de eklendiğini görürsünüz.

![Model tarayıcısı](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Ayrıca yeni sabit listesi türleri bu pencereden sağ fare düğmesini tıklatıp seçerek ekleyebileceğiniz **Enum Türü Ekle**. Türü oluşturulduktan sonra türleri listesinde görünür ve bir özellik ile ilişkilendirin olacaktır

## <a name="generate-database-from-model"></a>Modelden veritabanı oluştur

Şimdi biz modelini temel alan bir veritabanı oluşturabilirsiniz.

1.  Varlık Tasarımcısı seçin ve yüzey üzerinde boş bir alanı sağ **Model veritabanından oluştur**
2.  Veritabanı Oluştur Sihirbazı'nın, veri bağlantısı iletişim kutusu Seç'e tıklayın görüntülenen **yeni bağlantı** belirt düğmesine **(localdb)\\ifadesini mssqllocaldb** vesunucuadıiçin **EnumTest** tıklayın ve veritabanı için **Tamam**
3.  Yeni bir veritabanı oluşturmak isteyip istemediğinizi soran bir iletişim kutusu açılır, tıklayın **Evet**.
4.  Tıklayın **sonraki** ve oluşturulan DDL bir veritabanı oluşturmak için bir tanım DDL içermeyen Ayarları iletişim kutusu Not ve Özet görüntülenir Veritabanı Oluşturma Sihirbazı'nı veri tanımlama dili (DDL) oluşturur. bir sabit listesi türü eşleyen tablo
5.  Tıklayın **son** Son'a DDL betik yürütülmez.
6.  Veritabanı Oluşturma Sihirbazı'nı aşağıdakileri yapar: açılır **EnumTest.edmx.sql** T-SQL Düzenleyicisi oluşturur deposu şeması ve eşleme bölümlerini EDMX bağlantı dizesi bilgilerini ekler App.config dosyasına dosya
7.  T-SQL Düzenleyicisi'nde sağ fare düğmesine tıklayıp **yürütme** Connect sunucusu iletişim kutusu görüntülenirse, 2. adımdaki bağlantı bilgilerini girin ve tıklayın **Bağlan**
8.  Oluşturulan şema görüntülemek için SQL Server Nesne Gezgini içinde veritabanı adını sağ tıklatın ve seçin **Yenile**

## <a name="persist-and-retrieve-data"></a>Kalıcı hale getirmek ve veri alma

Main yöntemi tanımlandığı Program.cs dosyasını açın. Ana işlevine aşağıdaki kodu ekleyin. Kod bağlamı için yeni bir bölüm nesnesi ekler. Ardından, verileri kaydeder. Kod ayrıca bir bölüm adı DepartmentNames.English olduğu döndüren LINQ sorgusu yürütür.

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Derleme ve uygulamayı çalıştırın. Program şu çıktıyı üretir:

```
DepartmentID: 1 Name: English
```

Veritabanındaki verileri görüntülemek için SQL Server Nesne Gezgini içinde veritabanı adını sağ tıklatın ve seçin **Yenile**. Tablo ve seçim sağ fare düğmesini tıklatıp **görünüm verilerini**.

## <a name="summary"></a>Özet

Bu izlenecek yolda Numaralandırma türleri Entity Framework Designer kullanarak eşleme ve numaralandırmalar kodda kullanma inceledik. 
