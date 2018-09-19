---
title: Numaralandırma desteği - EF6 öncelikle - kod
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283726"
---
# <a name="enum-support---code-first"></a>Numaralandırma desteği - kod önce
> [!NOTE]
> **EF5 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 5'te kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Bu video ve adım adım izlenecek yol, numaralandırma türleri ile Entity Framework Code First işlemi gösterilir. Ayrıca, bir LINQ Sorgu numaralandırmalar kullanmayı gösterir.

Bu izlenecek yolda Code First yeni bir veritabanı oluşturmak için kullanır, ancak ayrıca [mevcut bir veritabanına eşlemek için Code First](~/ef6/modeling/code-first/workflows/existing-database.md).

Numaralandırma desteği, Entity Framework 5'te tanıtıldı. Numaralandırmalar, uzamsal veri türleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için .NET Framework 4.5 hedeflemesi gerekir. Visual Studio 2012, varsayılan olarak .NET 4.5 hedefler.

Varlık Çerçevesi'nde bir numaralandırma aşağıdaki temel alınan türler sahip olabilir: **bayt**, **Int16**, **Int32**, **Int64** , veya **SByte**.

## <a name="watch-the-video"></a>Videoyu izleyin
Bu video, numaralandırma türleri ile Entity Framework Code First kullanma işlemini gösterir. Ayrıca, bir LINQ Sorgu numaralandırmalar kullanmayı gösterir.

**Tarafından sunulan**: Julia Kornich

**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Ön koşullar

Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümü, bu izlenecek yolu tamamlamak için yüklü olması gerekir.

 

## <a name="set-up-the-project"></a>Projesi kurun

1.  Visual Studio 2012'yi açın
2.  Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**
3.  Sol bölmede **Visual C\#** ve ardından **konsol** şablonu
4.  Girin **EnumCodeFirst** tıklayın ve proje adı olarak **Tamam**

## <a name="define-a-new-model-using-code-first"></a>Code First kullanarak yeni bir modeli tanımlamak

Code First geliştirme kullanırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan bir .NET Framework sınıf yazarak başlayın. Aşağıdaki kod, departman sınıfı tanımlar.

Kod ayrıca DepartmentNames sabit listesi tanımlar. Varsayılan olarak, sabit listesidir. **int** türü. Departman sınıfı adı özelliği DepartmentNames türüdür.

Program.cs dosyasını açın ve aşağıdaki sınıf tanımları yapıştırın.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
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

EntityFramework derlemeye ek olarak System.ComponentModel.DataAnnotations ve System.Data.Entity derlemelere başvuruları de eklendiğini unutmayın.

Program.cs dosyasının en üstüne aşağıdakileri ekleyin using deyimi:

``` csharp
using System.Data.Entity;
```

Program.cs içinde içerik tanımı ekleyin. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Kalıcı hale getirmek ve veri alma

Main yöntemi tanımlandığı Program.cs dosyasını açın. Ana işlevine aşağıdaki kodu ekleyin. Kod bağlamı için yeni bir bölüm nesnesi ekler. Ardından, verileri kaydeder. Kod ayrıca bir bölüm adı DepartmentNames.English olduğu döndüren LINQ sorgusu yürütür.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Derleme ve uygulamayı çalıştırın. Program şu çıktıyı üretir:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Oluşturulan veritabanı görünümü

Uygulamayı ilk kez çalıştırdığınızda, Entity Framework sizin için bir veritabanı oluşturur. Biz Visual Studio 2012 yüklü olduğundan LocalDB örneğinde bir veritabanı oluşturulur. Varsayılan olarak Entity Framework veritabanı türetilmiş bağlamı tam olarak nitelenmiş adını sonra adları (örneğin, bu **EnumCodeFirst.EnumTestContext**). Varolan bir veritabanını kullanılacak sonraki saatleri.  

Veritabanı oluşturulduktan sonra modelinize herhangi bir değişiklik yaparsanız, Code First Migrations veritabanı şemasını güncelleştirmek için kullanmanızı unutmayın. Bkz: [yeni veritabanına Code First](~/ef6/modeling/code-first/workflows/new-database.md) geçişleri kullanma örneği için.

Veritabanını ve verileri görüntülemek için aşağıdakileri yapın:

1.  Visual Studio 2012 ana menüde **görünümü**  - &gt; **SQL Server Nesne Gezgini**.
2.  LocalDB sunucuları listesinde değilse, sağ fare düğmesine tıklayın **SQL Server** seçip **SQL Server Ekle** varsayılan **Windows kimlik doğrulaması** bağlanmak için LocalDB örneğini
3.  LocalDB düğümünü genişletin
4.  Unfold **veritabanları** göz atın ve yeni veritabanı görmek için klasör **departmanı** Code First sabit listesi türü eşleyen bir tablo oluşturmaz unutmayın, tablo
5.  Verileri görüntülemek, tablonun sağ tıklayın ve seçmek için **verileri görüntüleme**

## <a name="summary"></a>Özet

Bu izlenecek yolda Numaralandırma türleri ile Entity Framework Code First kullanma inceledik. 
