---
title: Entity Framework 6 yükseltme
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 711f1940080de27bd23cb8f641a5c7f2711dd65b
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182013"
---
# <a name="upgrading-to-entity-framework-6"></a>Entity Framework 6 yükseltme

EF önceki sürümlerinde .NET Framework bir parçası olarak gönderildiğini çekirdek kitaplıkları (öncelikle System.Data.Entity.dll) ve bir NuGet paketi sevk bant dışı (OOB) kitaplıkları (öncelikle EntityFramework.dll) arasında kod bölünmüş. EF6 çekirdek kitaplıklarından kodu alır ve OOB kitaplıkları içerir. Bu açık kaynak yapılacak EF izin vermek üzere gerekli ve .NET Framework farklı bir hızda evrim Geçiren mümkün olması için. Bu sonucu, uygulamaları taşınan türlerine karşı yeniden oluşturulması gerekecektir ' dir.

Bu DbContext EF 4.1 ve üzeri sevk edildi olarak kullanan uygulamalar için basit olmalıdır. Biraz daha fazla iş ObjectContext kullanan uygulamalar için gerekli değildir ancak yine de yapmak sabit değildir.

EF6 varolan bir uygulamaya yükseltmek için yapmanız gereken şeyleri listesi aşağıda verilmiştir.

## <a name="1-install-the-ef6-nuget-package"></a>1. EF6 NuGet paketini yükle

Yeni Entity Framework 6 çalışma zamanına yükseltmesini gerekir.

1. Seçin ve proje üzerinde sağ **NuGet paketlerini Yönet...**  
2. Altında **çevrimiçi** sekmesinde **EntityFramework** tıklatıp **yükleyin**  
   > [!NOTE]
   > EntityFramework NuGet paketi yüklendi önceki bir sürümü bu EF6 yükseltir.

Alternatif olarak, Paket Yöneticisi konsolundan aşağıdaki komutu çalıştırabilirsiniz:

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a>2. Derleme başvurularını System.Data.Entity.dll kaldırıldığından emin olun

EF6 NuGet paketini yükleme otomatik olarak System.Data.Entity tüm başvurular projenizden sizin için kaldırmanız gerekir.

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a>3. Takas herhangi EF Designer (EDMX) modellerini EF 6.x kod üretimini kullan

EF Designer ile oluşturulan herhangi bir model varsa, EF6 uyumlu kod üretmek için kod oluşturma şablonları güncelleştirmeniz gerekir.

> [!NOTE]
> Şu anda yalnızca EF 6.x DbContext Oluşturucu şablonları Visual Studio 2012 ve 2013 için kullanılabilen vardır.

1. Varolan kod üretimi şablonları silin. Bu dosyalar genellikle adlı  **\<edmx_file_name\>.tt** ve  **\<edmx_file_name\>. Context.tt** ve Çözüm Gezgini'nde edmx dosyanız altında iç içe olamaz. Çözüm Gezgini ve ENTER tuşuna şablon seçebilirsiniz **Del** anahtarını silin.  
   > [!NOTE]
   > Web sitesi projeleri şablonları değil edmx dosyanız altında iç içe geçmiş, ancak Çözüm Gezgini'nde yanı sıra listede.  

   > [!NOTE]
   > VB.NET projelerinde 'Tüm iç içe geçmiş şablon dosyalarını görebilmeniz dosyaları Göster' etkinleştirmeniz gerekir.
2. Uygun EF 6.x kod kuşak şablonu ekleyin. Açık EF Designer modelinizde sağ tasarım yüzeyi ve select **kod oluşturma öğesi Ekle...**
    - DbContext sonra (önerilen) API kullanıyorsanız **EF 6.x DbContext Oluşturucu** altında kullanılabilir olacak **veri** sekmesi.  
      > [!NOTE]
      > Visual Studio 2012 kullanıyorsanız, bu şablonu EF 6 araçları yüklemeniz gerekir. Bkz: [alma Entity Framework](~/ef6/fundamentals/install.md) Ayrıntılar için.  

    - ObjectContext API kullanıyorsanız sonra seçmeniz gerekecek **çevrimiçi** sekmesinde ve arama **EF 6.x EntityObject Oluşturucu**.  
3. Özelleştirmeler için kod oluşturma şablonları uyguladıysanız yeniden güncelleştirilmiş şablonların uygulamanız gerekir.

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a>4. Ad alanları için kullanılan her çekirdek EF türünü güncelleştir

DbContext ve Code First türleri için ad alanları değişmedi. EF 4.1 kullanan birçok uygulama için başka bir deyişle ya da daha sonra herhangi bir ayarı değiştirmek gerekmez.

Daha önce System.Data.Entity.dll türlerini ObjectContext gibi yeni ad alanları için taşınmıştır. Yani güncelleştirmeniz gerekebilir, *kullanarak* veya *alma* EF6 karşı oluşturmak için yönergeleri.

Genel ad değişiklikleri için System.Data.* herhangi bir türü için System.Data.Entity.Core.* taşınır kuralıdır. Diğer bir deyişle, yalnızca eklemek **Entity.Core.** System.Data sonra. Örneğin:

- System.Data.EntityException = > System.Data. **Entity.Core**. EntityException  
- System.Data.Objects.ObjectContext = > System.Data. **Entity.Core**. Objects.ObjectContext  
- System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core**. Objects.DataClasses.RelationshipManager  

Bu tür bulunan *çekirdek* ad alanları doğrudan çoğu DbContext tabanlı uygulamalar için kullanılmadığından. System.Data.Entity.dll parçası olan bazı türleri hala yaygın olarak ve doğrudan DbContext tabanlı uygulamalar için kullanılır ve bu nedenle içine taşınmamış olan *çekirdek* ad alanları. Bunlar:

- System.Data.EntityState = > System.Data. **Varlık**. EntityState  
- System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**  
  > [!NOTE]
  > Bu sınıf yeniden adlandırıldı; eski adıyla bir sınıf hala var olduğundan ve çalışır, ancak artık eski olarak işaretlendi.  
- System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**  
  > [!NOTE]
  > Bu sınıf yeniden adlandırıldı; eski adıyla bir sınıf hala var olduğundan ve çalışır, ancak artık kullanılmayan olarak işaretlenmiş.)  
- Uzamsal sınıfları (örneğin, DbGeography, DbGeometry) System.Data.Spatial taşınmış = > System.Data. **Varlık**. Uzamsal

> [!NOTE]
> System.Data ad alanındaki bazı türleri, EF derleme olmayan System.Data.dll olabilir. Bu tür değil taşıdık ve bu nedenle, ad alanları değişmeden kalır.
