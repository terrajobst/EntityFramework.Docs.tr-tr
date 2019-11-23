---
title: Enum desteği-EF Tasarımcısı-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182516"
---
# <a name="enum-support---ef-designer"></a>Enum desteği-EF Tasarımcısı
> [!NOTE]
> **Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

Bu video ve adım adım izlenecek yol, Entity Framework Designer numaralandırma türlerini nasıl kullanacağınızı gösterir. Ayrıca, bir LINQ sorgusunda numaralandırmaları nasıl kullanacağınızı gösterir.

Bu izlenecek yol, yeni bir veritabanı oluşturmak için Model First kullanır, ancak aynı zamanda, var olan bir veritabanına eşlemek için [Database First](~/ef6/modeling/designer/workflows/database-first.md) iş AKıŞıYLA birlikte EF Designer da kullanılabilir.

Enum desteği Entity Framework 5 ' te tanıtılmıştı. Numaralandırmalar, uzamsal veri türleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için, .NET Framework 4,5 ' i hedeflemelidir. Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.

Entity Framework, bir numaralandırma aşağıdaki temel türlere sahip olabilir: **byte**, **Int16**, **Int32**, **Int64** veya **SByte**.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda, Entity Framework Designer ile numaralandırma türlerinin nasıl kullanılacağı gösterilmektedir. Ayrıca, bir LINQ sorgusunda numaralandırmaları nasıl kullanacağınızı gösterir.

**Sunduğu**: Julia Kornich

**Video**: [wmv](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Önkoşulların önkoşulları

Bu yönergeyi tamamlamak için Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümünün yüklü olması gerekir.

## <a name="set-up-the-project"></a>Projeyi ayarlama

1.  Visual Studio 2012'yi açın
2.  **Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.
3.  Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin
4.  Projenin adı olarak **Trmefdesigner** girin ve **Tamam 'a** tıklayın

## <a name="create-a-new-model-using-the-ef-designer"></a>EF tasarımcısını kullanarak yeni model oluşturma

1.  Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe** ' ye tıklayın.
2.  Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** seçin
3.  Dosya adı için **Enumtestmodel. edmx** girin ve ardından **Ekle** ' ye tıklayın.
4.  Varlık Veri Modeli Sihirbazı sayfasında, model Içeriklerini Seç iletişim kutusunda **boş model** ' i seçin.
5.  **Son** ' a tıklayın

Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.

Sihirbaz aşağıdaki eylemleri gerçekleştirir:

-   Kavramsal modeli, depolama modelini ve aralarındaki eşlemeyi tanımlayan EnumTestModel. edmx dosyasını oluşturur. Oluşturulan meta veri dosyalarının derlemeye katıştırılması için. edmx dosyasının meta veri yapıt Işleme özelliğini çıktı derlemesine katıştırmak üzere ayarlar.
-   Aşağıdaki derlemelere bir başvuru ekler: EntityFramework, System. ComponentModel. Dataaçıklamalarda ve System. Data. Entity.
-   EnumTestModel.tt ve EnumTestModel.Context.tt dosyalarını oluşturur ve. edmx dosyasının altına ekler. Bu T4 şablon dosyaları,. edmx modelindeki varlıklarla eşlenen DbContext türetilmiş türünü ve POCO türlerini tanımlayan kodu oluşturur.

## <a name="add-a-new-entity-type"></a>Yeni varlık türü Ekle

1.  Tasarım yüzeyinde boş bir alana sağ tıklayın, **Ekle-&gt; varlık**' ı seçin, yeni varlık iletişim kutusu görüntülenir
2.  Tür adı için **departmanı** belirtin ve anahtar özellik adı için **DepartmentID** belirtin, türü **Int32** olarak bırakın
3.  **Tamam**’a tıklayın.
4.  Varlığa sağ tıklayın ve **New-&gt; skaler özelliği Ekle** ' yi seçin
5.  Yeni özelliği **ad** olarak yeniden adlandır
6.  Yeni özelliğin türünü **Int32** olarak değiştirin (varsayılan olarak, yeni özellik dize türündedir) ve türü değiştirmek için Özellikler penceresi açın ve Type özelliğini **Int32** olarak değiştirin
7.  Başka bir skaler özellik ekleyin ve **bütçeye**yeniden adlandırın, türü **Decimal** olarak değiştirin

## <a name="add-an-enum-type"></a>Sabit listesi türü Ekle

1.  Entity Framework Designer, ad özelliğine sağ tıklayın, **sabit listesine Dönüştür** ' ü seçin

    ![Sabit listesine Dönüştür](~/ef6/media/converttoenum.png)

2.  Numaralandırma **Ekle** iletişim kutusunda, enum türü adı için **DepartmentNames** yazın, temel alınan türü **Int32**olarak değiştirin ve ardından aşağıdaki üyeleri türüne ekleyin: İngilizce, matematik ve ekonomiku

    ![Sabit listesi türü Ekle](~/ef6/media/addenumtype.png)

3.  **Tamam** 'a basın
4.  Modeli kaydetme ve projeyi derleme
    > [!NOTE]
    > Oluşturduğunuzda, eşlenmemiş varlıklar ve ilişkilendirmeler hakkında uyarılar Hata Listesi görünebilir. Bu uyarıları yoksayabilirsiniz çünkü veritabanını modelden oluşturmayı seçmemiz gerekir, ancak hatalar kaybolur.

Özellikler penceresi görüyorsanız, ad özelliğinin türünün **DepartmentNames** olarak değiştirildiğini ve yeni eklenen sabit listesi türünün tür listesine eklendiğini fark edeceksiniz.

Model tarayıcı penceresine geçiş yaparsanız, türün de enum Types düğümüne eklendiğini görürsünüz.

![Model tarayıcı](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Ayrıca, sağ fare düğmesine tıklayıp **sabit listesi türü Ekle**' yi seçerek bu pencereden yeni enum türleri ekleyebilirsiniz. Tür oluşturulduktan sonra, tür listesinde görünür ve bir özellikle ilişkilendirebileceksiniz

## <a name="generate-database-from-model"></a>Modelden veritabanı oluştur

Artık modeli temel alan bir veritabanı oluşturabilirsiniz.

1.  Entity Desisgner yüzeyinde boş bir alana sağ tıklayın ve **modelden veritabanı oluştur** ' u seçin.
2.  Veritabanı oluşturma Sihirbazı ' nın veri bağlantısını seçin Iletişim kutusu görüntülenir **Yeni bağlantı** düğmesine tıklayın (LocalDB) sunucu adı ve veritabanı Için **enumtest** için **Mssqllocaldb\\** ve ardından **Tamam** ' a tıklayın.
3.  Yeni bir veritabanı oluşturmak isteyip istemediğinizi soran bir iletişim kutusu açılır ve **Evet**' e tıklayın.
4.  **İleri** ' ye tıklayın ve veritabanı oluşturma Sihirbazı bir veritabanı oluşturmak için veri tanımlama DILI (ddl) oluşturur oluşturulan DDL, Özet ve ayarlar iletişim kutusu ' nda, DDL 'nin numaralandırma türüyle eşleşen bir tablo tanımı içermediğini belirten bir açıklama içermez.
5.  Son **' a** tıklayarak DDL betiğini yürütmez.
6.  Veritabanı oluşturma Sihirbazı şunları yapar: **Enumtest. edmx** dosyasını açar. T-SQL DÜZENLEYICISINDE, edmx dosyasının mağaza şeması ve eşleme bölümleri, bağlantı dizesi bilgilerini App. config dosyasına ekler
7.  T-SQL Düzenleyicisi ' nde sağ fare düğmesine tıklayın ve sunucuya Bağlan iletişim kutusunu **Çalıştır** ' ı seçin. Adım 2 ' den bağlantı bilgilerini girin ve **Bağlan** ' a tıklayın.
8.  Oluşturulan şemayı görüntülemek için SQL Server Nesne Gezgini veritabanı adına sağ tıklayın ve **Yenile** ' yi seçin.

## <a name="persist-and-retrieve-data"></a>Kalıcı ve veri alma

Main yönteminin tanımlandığı Program.cs dosyasını açın. Aşağıdaki kodu Main işlevine ekleyin. Kod, içeriğe yeni bir departman nesnesi ekler. Daha sonra verileri kaydeder. Kod ayrıca, adın DepartmentNames. Ingilizce olduğu bir departmanı döndüren bir LINQ sorgusu yürütür.

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

Uygulamayı derleyin ve çalıştırın. Program aşağıdaki çıktıyı üretir:

```console
DepartmentID: 1 Name: English
```

Veritabanındaki verileri görüntülemek için SQL Server Nesne Gezgini veritabanı adına sağ tıklayın ve **Yenile**' yi seçin. Ardından, tablodaki sağ fare düğmesine tıklayın ve **verileri görüntüle**' yi seçin.

## <a name="summary"></a>Özet

Bu kılavuzda, Entity Framework Designer kullanarak numaralandırma türlerini nasıl eşleyeceğinizi ve kod içinde Numaralandırmaların nasıl kullanılacağını inceledik. 
