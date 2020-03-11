---
title: Sabit listesi desteği-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419110"
---
# <a name="enum-support---code-first"></a>Sabit listesi desteği-Code First
> [!NOTE]
> **Yalnızca EF5** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 5 ' te sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

Bu video ve adım adım yönergeler, Entity Framework Code First ile numaralandırma türlerini nasıl kullanacağınızı gösterir. Ayrıca, bir LINQ sorgusunda numaralandırmaları nasıl kullanacağınızı gösterir.

Bu izlenecek yol, yeni bir veritabanı oluşturmak için Code First kullanacaktır, ancak aynı zamanda [Code First kullanarak var olan bir veritabanına eşleyebilirsiniz](~/ef6/modeling/code-first/workflows/existing-database.md).

Enum desteği Entity Framework 5 ' te tanıtılmıştı. Numaralandırmalar, uzamsal veri türleri ve tablo değerli işlevler gibi yeni özellikleri kullanmak için, .NET Framework 4,5 ' i hedeflemelidir. Visual Studio 2012, .NET 4,5 'i varsayılan olarak hedefler.

Entity Framework, bir numaralandırma aşağıdaki temel türlere sahip olabilir: **byte**, **Int16**, **Int32**, **Int64** veya **SByte**.

## <a name="watch-the-video"></a>Videoyu izleme
Bu videoda, Entity Framework Code First ile numaralandırma türlerinin nasıl kullanılacağı gösterilmektedir. Ayrıca, bir LINQ sorgusunda numaralandırmaları nasıl kullanacağınızı gösterir.

**Sunduğu**: Julia Kornich

**Video**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>Önkoşullar

Bu yönergeyi tamamlamak için Visual Studio 2012, Ultimate, Premium, Professional veya Web Express sürümünün yüklü olması gerekir.

 

## <a name="set-up-the-project"></a>Projeyi ayarlama

1.  Visual Studio 2012'yi açın
2.  **Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje** ' ye tıklayın.
3.  Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol** şablonunu seçin
4.  Projenin adı olarak **Enumcodefırst** girin ve **Tamam 'a** tıklayın

## <a name="define-a-new-model-using-code-first"></a>Code First kullanarak yeni bir model tanımlama

Code First geliştirme kullanılırken, genellikle kavramsal (etki alanı) modelinizi tanımlayan .NET Framework sınıfları yazarak başlarsınız. Aşağıdaki kod departman sınıfını tanımlar.

Kod ayrıca DepartmentNames sabit listesini de tanımlar. Varsayılan olarak, sabit listesi **int** türündedir. Departman sınıfındaki Name özelliği DepartmentNames türüdür.

Program.cs dosyasını açın ve aşağıdaki sınıf tanımlarını yapıştırın.

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
 

## <a name="define-the-dbcontext-derived-type"></a>DbContext türetilmiş türünü tanımlayın

Varlıkları tanımlamaya ek olarak, DbContext 'ten türeten bir sınıf tanımlamanız ve DbSet&lt;TEntity&gt; özelliklerini kullanıma sunabilmeniz gerekir. DbSet&lt;TEntity&gt; özellikleri, bağlamın modele hangi türleri dahil etmek istediğinizi bilmesini sağlar.

DbContext türetilmiş türünün bir örneği, çalışma zamanı sırasında varlık nesnelerini yönetir, bu da nesneleri bir veritabanındaki verilerle doldurmayı, değişiklik izlemeyi ve kalıcı verileri veritabanına getirmeyi içerir.

DbContext ve DbSet türleri EntityFramework derlemesinde tanımlanmıştır. EntityFramework NuGet paketini kullanarak bu DLL 'ye bir başvuru ekleyeceğiz.

1.  Çözüm Gezgini, proje adına sağ tıklayın.
2.  **NuGet Paketlerini Yönet...** seçeneğini belirleyin.
3.  NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin.
4.  **Install** 'a tıklayın

EntityFramework derlemesine ek olarak System. ComponentModel. Dataaçıklamalarda ve System. Data. Entity Derlemeleriyle ilgili başvurular da eklenir.

Program.cs dosyasının en üstüne aşağıdaki using ifadesini ekleyin:

``` csharp
using System.Data.Entity;
```

Program.cs içinde bağlam tanımını ekleyin. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>Kalıcı ve veri alma

Main yönteminin tanımlandığı Program.cs dosyasını açın. Aşağıdaki kodu Main işlevine ekleyin. Kod, içeriğe yeni bir departman nesnesi ekler. Daha sonra verileri kaydeder. Kod ayrıca, adın DepartmentNames. Ingilizce olduğu bir departmanı döndüren bir LINQ sorgusu yürütür.

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

Uygulamayı derleyin ve çalıştırın. Program aşağıdaki çıktıyı üretir:

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>Oluşturulan veritabanını görüntüleme

Uygulamayı ilk kez çalıştırdığınızda, Entity Framework sizin için bir veritabanı oluşturur. Visual Studio 2012 'nin yüklü olduğu için, veritabanı LocalDB örneğinde oluşturulacak. Entity Framework, varsayılan olarak, türetilmiş bağlamın tam adından sonra veritabanını ( **Enumcodefırst. EnumTestContext**olan bu örnek için) adlandırır. Mevcut veritabanının sonraki zamanları kullanılacaktır.  

Veritabanı oluşturulduktan sonra modelinizde herhangi bir değişiklik yaparsanız, veritabanı şemasını güncelleştirmek için Code First Migrations kullanmanız gerekir. Geçişleri kullanma örneği için bkz. [Code First yeni bir veritabanı](~/ef6/modeling/code-first/workflows/new-database.md) .

Veritabanını ve verileri görüntülemek için aşağıdakileri yapın:

1.  Visual Studio 2012 ana menüsünde **görünüm** -&gt; **SQL Server Nesne Gezgini**' yı seçin.
2.  LocalDB sunucular listesinde yoksa, **SQL Server** sağ fare düğmesine tıklayın ve **Ekle SQL Server** ' yi seçin ve LocalDB örneğine bağlanmak Için varsayılan **Windows kimlik doğrulamasını** kullanın
3.  LocalDB düğümünü Genişlet
4.  Yeni veritabanını görmek ve **bölüm** tablosu notuna göz atarak, bu Code First sabit listesi türüyle eşleşen bir tablo oluşturmadığından, **veritabanları** klasörünün sarmalamayı kaldırın
5.  Verileri görüntülemek için tabloya sağ tıklayıp **verileri görüntüle** ' yi seçin.

## <a name="summary"></a>Özet

Bu izlenecek yolda, Entity Framework Code First ile numaralandırma türlerini nasıl kullanacağınızı inceledik. 
