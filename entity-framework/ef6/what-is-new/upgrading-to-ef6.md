---
title: Entity Framework 6 ' ya yükseltme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 4395a9c117a6cf38e7fc08f11ee689d6fffa6fed
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419656"
---
# <a name="upgrading-to-entity-framework-6"></a>Entity Framework 6 ' ya yükseltme

EF 'in önceki sürümlerinde kod, bir NuGet paketinde gönderilen .NET Framework ve bant dışı (OOB) kitaplıklarının (birincil olarak EntityFramework. dll) bir parçası olarak sevk edilen çekirdek kitaplıklar (birincil olarak System. Data. Entity. dll) arasına bölünür. EF6, kodu çekirdek kitaplıklarından alır ve OOB kitaplıklarına ekler. Bu, EF 'in açık kaynak haline getirilme ve .NET Framework farklı bir hızda gelişebilmesini sağlamak için gereklidir. Bunun sonucu olarak, uygulamaların taşınan türler için yeniden oluşturulması gerekir.

Bu, EF 4,1 ve üzeri sürümlerde olduğu gibi DbContext kullanan uygulamalar için basit olmalıdır. ObjectContext 'in kullanıldığı ancak hala zor olmayan uygulamalar için biraz daha fazla iş gerekir.

Mevcut bir uygulamayı EF6 'e yükseltmek için yapmanız gereken öğelerin bir denetim listesi aşağıda verilmiştir.

## <a name="1-install-the-ef6-nuget-package"></a>1. EF6 NuGet paketini yüklemesi

Yeni Entity Framework 6 çalışma zamanına yükseltmeniz gerekir.

1. Projenize sağ tıklayın ve **NuGet Paketlerini Yönet..** . seçeneğini belirleyin.  
2. **Çevrimiçi** sekmesinde **EntityFramework** ' ü seçin ve ardından **yükler** ' e tıklayın.  
   > [!NOTE]
   > EntityFramework NuGet paketinin önceki bir sürümü yüklüyse, bunu EF6 sürümüne yükseltir.

Alternatif olarak, paket yöneticisi konsolundan aşağıdaki komutu çalıştırabilirsiniz:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. System. Data. Entity. dll ' ye bütünleştirilmiş kod başvurularının kaldırıldığından emin olun

EF6 NuGet paketinin yüklenmesi, sizin için projenizden System. Data. Entity başvurularını otomatik olarak kaldırmalıdır.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. EF Designer (EDMX) modellerini EF 6. x kod oluşturma kullanacak şekilde değiştirin

EF Designer ile oluşturulmuş modelleriniz varsa, EF6 uyumlu kod üretmek için kod oluşturma şablonlarını güncelleştirmeniz gerekir.

> [!NOTE]
> Şu anda yalnızca Visual Studio 2012 ve 2013 için kullanılabilen EF 6. x DbContext Oluşturucu şablonları mevcuttur.

1. Mevcut kod oluşturma şablonlarını silin. Bu dosyalar genellikle **\<edmx_file_name\>. tt** ve **\<edmx_file_name\>olarak adlandırılır. Context.tt** ve Çözüm Gezgini içindeki edmx dosyanızın altına yerleştirilmiş. Çözüm Gezgini şablonları seçebilir ve silmek için **del** tuşuna basabilirsiniz.  
   > [!NOTE]
   > Web sitesi projelerinde, şablonlar edmx dosyanızın altına yerleştirmeyecektir, ancak Çözüm Gezgini yanında listelenir.  

   > [!NOTE]
   > VB.NET projelerinde, iç içe şablon dosyalarını görebilmeniz için ' tüm dosyaları göster ' i etkinleştirmeniz gerekir.
2. Uygun EF 6. x kod oluşturma şablonunu ekleyin. Ortamınızı EF tasarımcısında açın, tasarım yüzeyine sağ tıklayıp **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.
    - DbContext API 'SI kullanıyorsanız (önerilir), bu durumda **EF 6. x DbContext Oluşturucu** **veri** sekmesinde kullanılabilir.  
      > [!NOTE]
      > Visual Studio 2012 kullanıyorsanız, bu şablonun olması için EF 6 araçlarını yüklemeniz gerekir. Ayrıntılar için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md) .  

    - ObjectContext API 'sini kullanıyorsanız **çevrimiçi** sekmesini seçmeniz ve **EF 6. x EntityObject Generator**araması yapmanız gerekir.  
3. Kod oluşturma şablonlarına herhangi bir özelleştirme uyguladıysanız, bunları güncelleştirilmiş şablonlara yeniden uygulamanız gerekir.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. kullanılan tüm çekirdek EF türleri için ad alanlarını güncelleştirin

DbContext ve Code First türleri için ad alanları değişmemiştir. Bu, EF 4,1 veya sonrasını kullanan birçok uygulama için herhangi bir şeyi değiştirmeniz gerekmediği anlamına gelir.

Daha önce System. Data. Entity. dll içinde bulunan ObjectContext gibi türler yeni ad alanlarına taşınmıştır. Bu, EF6 'e göre derlemek için *using* veya *Import* yönergelerinden güncelleştirmeniz gerekebilecek anlamına gelir.

Ad alanı değişikliklerinin genel kuralı, System. Data. * içindeki herhangi bir tür System. Data. Entity. Core. * öğesine taşınır. Diğer bir deyişle **Entity. Core** 'u eklemeniz yeterlidir. System. Data 'dan sonra. Örnek:

- System. Data. EntityException = > System. Data. **Entity. Core**. EntityException  
- System. Data. Objects. ObjectContext = > System. Data. **Entity. Core**. Objects. ObjectContext  
- System. Data. Objects. DataClasses. RelationshipManager = > System. Data. **Entity. Core**. Objects. DataClasses. RelationshipManager  

Bu türler, çoğu DbContext tabanlı uygulama için doğrudan kullanılmadığından *çekirdek* ad uzayındadır. System. Data. Entity. dll ' nin parçası olan bazı türler yaygın olarak ve doğrudan DbContext tabanlı uygulamalar için kullanılır ve bu nedenle *çekirdek* ad alanlarına taşınamaz. Bunlar:

- System. Data. EntityState = > System. Data. **Varlık**. EntityState  
- System. Data. Objects. DataClasses. EdmFunctionAttribute = > System. Data. **Entity. DbFunctionAttribute**  
  > [!NOTE]
  > Bu sınıf yeniden adlandırıldı; Eski adlı bir sınıf hala var ve işe yaramıştır, ancak artık eski olarak işaretlendi.  
- System. Data. Objects. EntityFunctions = > System. Data. **Entity. DbFunctions**  
  > [!NOTE]
  > Bu sınıf yeniden adlandırıldı; Eski ada sahip bir sınıf hala var ve çalışmaktadır, ancak artık eski olarak işaretlendi.)  
- Uzamsal sınıflar (örneğin, Dbcoğrafya, DbGeometry) System. Data. uzamsal = > System. Data öğesinden taşınmıştır. **Varlık**. Uzay

> [!NOTE]
> System. Data ad alanındaki bazı türler, EF derlemesi olmayan System. Data. dll ' dir. Bu türler taşınmadı ve bu nedenle ad alanları değişmeden kalır.
