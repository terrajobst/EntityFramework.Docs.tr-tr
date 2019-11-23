---
title: Uzamsal-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182657"
---
# <a name="spatial---code-first"></a>Uzamsal-Code First
> [!NOTE]
> **Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

Video ve adım adım izlenecek yol, uzamsal türlerin Entity Framework Code First nasıl eşleneceğini gösterir. Ayrıca, iki konum arasındaki mesafeyi bulmak için bir LINQ sorgusunun nasıl kullanılacağını gösterir.

Bu izlenecek yol, yeni bir veritabanı oluşturmak için Code First kullanır, ancak [Code First mevcut bir veritabanına](~/ef6/modeling/code-first/workflows/existing-database.md)de kullanabilirsiniz.

Uzamsal tür desteği Entity Framework 5 ' te tanıtılmıştı. Uzamsal tür, numaralandırmalar ve tablo değerli işlevler gibi yeni özellikleri kullanmak için, .NET Framework 4,5 ' i hedeflemelidir. Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.

Uzamsal veri türlerini kullanmak için, uzamsal destek içeren bir Entity Framework sağlayıcısı da kullanmanız gerekir. Daha fazla bilgi için bkz. [uzamsal türler için sağlayıcı desteği](~/ef6/fundamentals/providers/spatial-support.md) .

İki ana uzamsal veri türü vardır: Coğrafya ve geometri. Coğrafya veri türü ellipsoidal verilerini depolar (örneğin, GPS Enlem ve boylam koordinatları). Geometri veri türü, Euclidean (düz) koordinat sistemini temsil eder.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu videoda, Entity Framework Code First ile uzamsal türlerin nasıl eşleneceğini gösterilmektedir. Ayrıca, iki konum arasındaki mesafeyi bulmak için bir LINQ sorgusunun nasıl kullanılacağını gösterir.

**Sunduğu**: Julia Kornich

**Video**: [wmv](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>Önkoşulların önkoşulları

Bu yönergeyi tamamlamak için Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümünün yüklü olması gerekir.

## <a name="set-up-the-project"></a>Projeyi ayarlama

1.  Visual Studio 2012'yi açın
2.  **Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.
3.  Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin
4.  Projenin adı olarak **SpatialCodeFirst** girin ve **Tamam 'a** tıklayın

## <a name="define-a-new-model-using-code-first"></a>Code First kullanarak yeni bir model tanımlama

Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız. Aşağıdaki kod University sınıfını tanımlar.

Üniversitenin, Dbcoğrafya türünün Location özelliği vardır. Dbcoğrafya türünü kullanmak için System. Data. Entity derlemesine bir başvuru eklemeniz ve ayrıca System. Data. uzamsal using ifadesini de eklemeniz gerekir.

Program.cs dosyasını açın ve aşağıdaki using deyimlerini dosyanın en üstüne yapıştırın:

``` csharp
using System.Data.Spatial;
```

Aşağıdaki üniversite sınıfı tanımını Program.cs dosyasına ekleyin.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a>DbContext türetilmiş türünü tanımlayın

Varlıkları tanımlamaya ek olarak, DbContext 'ten türeten bir sınıf tanımlamanız ve DbSet&lt;TEntity&gt; özelliklerini kullanıma sunabilmeniz gerekir. DbSet&lt;TEntity&gt; özellikleri, bağlamın modele hangi türleri dahil etmek istediğinizi bilmesini sağlar.

DbContext türetilmiş türünün bir örneği, çalışma zamanı sırasında varlık nesnelerini yönetir, bu da nesneleri bir veritabanındaki verilerle doldurmayı, değişiklik izlemeyi ve kalıcı verileri veritabanına getirmeyi içerir.

DbContext ve DbSet türleri EntityFramework derlemesinde tanımlanmıştır. EntityFramework NuGet paketini kullanarak bu DLL 'ye bir başvuru ekleyeceğiz.

1.  Çözüm Gezgini, proje adına sağ tıklayın.
2.  **NuGet Paketlerini Yönet...** seçeneğini belirleyin.
3.  NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin.
4.  **Install** 'a tıklayın

EntityFramework derlemesine ek olarak, System. ComponentModel. Dataek açıklama derlemesine bir başvuru de eklenir.

Program.cs dosyasının en üstüne aşağıdaki using ifadesini ekleyin:

``` csharp
using System.Data.Entity;
```

Program.cs içinde bağlam tanımını ekleyin. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>Kalıcı ve veri alma

Main yönteminin tanımlandığı Program.cs dosyasını açın. Aşağıdaki kodu Main işlevine ekleyin.

Kod, içeriğe iki yeni üniversite nesnesi ekler. Uzamsal Özellikler Dbcoğrafya. FromText yöntemi kullanılarak başlatılır. Wellknown olarak temsil edilen Coğrafya noktası yöntemine geçirilir. Kod daha sonra verileri kaydeder. Daha sonra, konumu belirtilen konuma en yakın olan bir üniversite nesnesi döndüren LINQ sorgusu oluşturulur ve yürütülür.

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

Uygulamayı derleyin ve çalıştırın. Program aşağıdaki çıktıyı üretir:

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>Oluşturulan veritabanını görüntüleme

Uygulamayı ilk kez çalıştırdığınızda, Entity Framework sizin için bir veritabanı oluşturur. Visual Studio 2012 'nin yüklü olduğu için, veritabanı LocalDB örneğinde oluşturulacak. Varsayılan olarak Entity Framework, türetilmiş bağlamın tam adı (Bu örnekte **SpatialCodeFirst. Üniversıtycontext**) sonra veritabanını adlandırır. Mevcut veritabanının sonraki zamanları kullanılacaktır.  

Veritabanı oluşturulduktan sonra modelinizde herhangi bir değişiklik yaparsanız, veritabanı şemasını güncelleştirmek için Code First Migrations kullanmanız gerekir. Geçişleri kullanma örneği için bkz. [Code First yeni bir veritabanı](~/ef6/modeling/code-first/workflows/new-database.md) .

Veritabanını ve verileri görüntülemek için aşağıdakileri yapın:

1.  Visual Studio 2012 ana menüsünde **görünüm** -&gt; **SQL Server Nesne Gezgini**' yı seçin.
2.  LocalDB sunucular listesinde yoksa, **SQL Server** sağ fare düğmesine tıklayın ve **Ekle SQL Server** ' yi seçin ve LocalDB örneğine bağlanmak Için varsayılan **Windows kimlik doğrulamasını** kullanın
3.  LocalDB düğümünü Genişlet
4.  Yeni veritabanını görmek ve **üniversiteler** tablosuna gitmek için **veritabanları** klasörünün sarmalamayı kaldırın
5.  Verileri görüntülemek için tabloya sağ tıklayıp **verileri görüntüle** ' yi seçin.

## <a name="summary"></a>Özet

Bu kılavuzda, Entity Framework Code First ile uzamsal türleri nasıl kullanacağınızı inceledik. 
