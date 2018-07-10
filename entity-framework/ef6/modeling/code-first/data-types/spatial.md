---
title: Uzamsal - EF6 öncelikle - kod
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
caps.latest.revision: 3
ms.openlocfilehash: 03557820bc689d8e7389a1f84121b7eeaa904df1
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914262"
---
# <a name="spatial---code-first"></a>Uzamsal - Code First
> [!NOTE]
> **EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Video ve adım adım izlenecek yol, Entity Framework Code First uzamsal türleriyle eşleme işlemi gösterilmektedir. Ayrıca, LINQ sorgusu iki konum arasında bir uzaklık bulmak için nasıl kullanılacağını gösterir.

Bu izlenecek yolda Code First yeni bir veritabanı oluşturmak için kullanır, ancak ayrıca [var olan bir veritabanına ilk kod](~/ef6/modeling/code-first/workflows/existing-database.md).

Uzamsal türü desteği, Entity Framework 5'te tanıtıldı. Not uzamsal türü, sabit listeleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için .NET Framework 4.5 hedeflemesi gerekir. Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.

Uzamsal veri türleri kullanmayı da uzamsal desteğine sahip bir varlık çerçevesi sağlayıcısına kullanmanız gerekir. Bkz: [uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md) daha fazla bilgi için.

İki ana uzamsal veri türü vardır: coğrafya ve geometri. Coğrafya veri türü ellipsoidal verilerini depolayan (örneğin, GPS'i enlem ve boylam koordinatlarını). Geometri veri türü (düz) Euclidean koordinat sistemini temsil eder.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda, Entity Framework Code First uzamsal türleriyle eşleme işlemi gösterilmektedir. Ayrıca, LINQ sorgusu iki konum arasında bir uzaklık bulmak için nasıl kullanılacağını gösterir.

**Tarafından sunulan**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Ön koşullar

Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümü, bu izlenecek yolu tamamlamak için yüklü olması gerekir.

## <a name="set-up-the-project"></a>Projesi kurun

1.  Visual Studio 2012'yi açın
2.  Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**
3.  Sol bölmede **Visual C\#** ve ardından **konsol** şablonu
4.  Girin **SpatialCodeFirst** tıklayın ve proje adı olarak **Tamam**

## <a name="define-a-new-model-using-code-first"></a>Code First kullanarak yeni bir modeli tanımlamak

Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın. Aşağıdaki kod, University sınıfı tanımlar.

University DbGeography tür konumu özelliğine sahiptir. DbGeography türünü kullanmak üzere System.Data.Entity derlemeye bir başvuruda bulunun ve ayrıca using deyimi System.Data.Spatial eklemeniz gerekir.

Program.cs dosyasını açın ve aşağıdakini yapıştırın dosyasının en üstüne using deyimlerini:

``` csharp
using System.Data.Spatial;
```

Program.cs dosyasına aşağıdaki University sınıf tanımını ekleyin.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a>Tanımlama DbContext türetilmiş türü

Varlıklar tanımlayan yanı sıra olan DB kullanıma sunar ve DbContext türeyen bir sınıfı tanımlamanız gereken&lt;TEntity&gt; özellikleri. Olan DB&lt;TEntity&gt; özellikleri modele dahil etmek istediğiniz hangi türlerin bilmeniz bağlam olanak tanır.

Nesneleri bir veritabanındaki verilerle doldurma içeren çalışma zamanı sırasında varlık nesnesi DbContext türetilmiş türün bir örneğini yönetir, izleme ve kalıcı veri veritabanına değiştirin.

DbContext ve olan DB türleri EntityFramework derlemede tanımlanır. EntityFramework NuGet paketini kullanarak bu DLL'ye bir başvuru ekleyeceğiz.

1.  Çözüm Gezgini'nde proje adına sağ tıklayın.
2.  Seçin **NuGet paketlerini Yönet...**
3.  NuGet paketlerini Yönet iletişim kutusunda, seçmek **çevrimiçi** sekmesini **EntityFramework** paket.
4.  Tıklayın **yükleyin**

EntityFramework derleme yanı sıra System.ComponentModel.DataAnnotations derlemesine bir başvuru da eklendiğini unutmayın.

Program.cs dosyasının en üstüne aşağıdakileri ekleyin using deyimi:

``` csharp
using System.Data.Entity;
```

Program.cs içinde içerik tanımı ekleyin. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Kalıcı hale getirmek ve veri alma

Main yöntemi tanımlandığı Program.cs dosyasını açın. Ana işlevine aşağıdaki kodu ekleyin.

Kod bağlamı için iki yeni University nesneleri ekler. Uzamsal özellikler DbGeography.FromText yöntemi kullanılarak başlatılır. WellKnownText yöntemine geçirilen olarak temsil edilen Coğrafya noktası. Kodu daha sonra verileri kaydeder. Konumunda belirtilen konuma yakın olduğu University nesnesi döndüren oluşturulur ve yürütülen ardından LINQ Sorgu.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
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

## <a name="view-the-generated-database"></a>Oluşturulan veritabanı görünümü

Uygulamayı ilk kez çalıştırdığınızda, Entity Framework sizin için bir veritabanı oluşturur. Biz Visual Studio 2012 yüklü olduğundan LocalDB örneğinde bir veritabanı oluşturulur. Varsayılan olarak Entity Framework veritabanı türetilmiş bağlamı tam olarak nitelenmiş adını sonra adları (Bu örnekte, **SpatialCodeFirst.UniversityContext**). Varolan bir veritabanını kullanılacak sonraki saatleri.  

Veritabanı oluşturulduktan sonra modelinize herhangi bir değişiklik yaparsanız, Code First Migrations veritabanı şemasını güncelleştirmek için kullanmanızı unutmayın. Bkz: [yeni veritabanına Code First](~/ef6/modeling/code-first/workflows/new-database.md) geçişleri kullanma örneği için.

Veritabanını ve verileri görüntülemek için aşağıdakileri yapın:

1.  Visual Studio 2012 ana menüde **görünümü**  - &gt; **SQL Server Nesne Gezgini**.
2.  LocalDB sunucuları listesinde değilse, sağ fare düğmesine tıklayın **SQL Server** seçip **SQL Server Ekle** varsayılan **Windows kimlik doğrulaması** bağlanmak için LocalDB örneğini
3.  LocalDB düğümünü genişletin
4.  Unfold **veritabanları** göz atın ve yeni veritabanı görmek için klasör **üniversiteler** tablo
5.  Verileri görüntülemek, tablonun sağ tıklayın ve seçmek için **verileri görüntüleme**

## <a name="summary"></a>Özet

Bu izlenecek yolda uzamsal türler ile Entity Framework Code First kullanma inceledik. 
