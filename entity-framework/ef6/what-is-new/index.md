---
title: Yenilikler - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: e0367aeefd682434bf520301776bcff4f0e72e06
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136133"
---
# <a name="whats-new-in-ef6"></a>EF6'daki yenilikler

En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için Entity Framework'ün en son yayımlanan sürümünü kullanmanızı şiddetle öneririz.
Ancak, önceki bir sürümü kullanmanız gerekebileceğini veya en son sürüm öncesi yeni geliştirmelerle denemeler yapmak isteyebileceğinibiliyoruz.
EF'nin belirli sürümlerini yüklemek için Varlık [Çerçevesi'ni al'a](~/ef6/fundamentals/install.md)bakın.

## <a name="ef-640"></a>EF 6.4.0

EF 6.4.0 çalışma süresi Aralık 2019'da NuGet'de yayınlandı. EF 6.4'ün birincil hedefi, EF 6.3'te teslim edilen özellikleri ve senaryoları parlatmaktır. Github'daki [önemli düzeltmelerin listesine](https://github.com/dotnet/ef6/milestone/14?closed=1) bakın.

## <a name="ef-630"></a>EF 6.3.0

EF 6.3.0 çalışma süresi Eylül 2019'da NuGet'de yayınlandı. Bu sürümün temel amacı, EF 6'yı .NET Core 3.0'a geçiren varolan uygulamaların geçişini kolaylaştırmaktı. Topluluk ayrıca çeşitli hata düzeltmeleri ve geliştirmeler katkıda bulunmuştur. Ayrıntılar için her 6.3.0 [kilometre taşında](https://github.com/aspnet/EntityFramework6/milestones?state=closed) kapatılan sorunlara bakın. Burada daha önemli olanlardan bazıları şunlardır:

- .NET Core 3.0 desteği
  - EntityFramework paketi artık .NET Framework 4.x'e ek olarak .NET Standard 2.1'i hedeflemektedir.
  - Bu, EF 6.3'ün çapraz platform olduğu ve Linux ve macOS gibi Windows'un yanı sıra diğer işletim sistemlerinde de desteklenerek desteklendirilen anlamına gelir.
  - Geçiş komutları, işlem dışında yürütülmesi ve SDK tarzı projelerle çalışmak üzere yeniden yazıldı.
- SQL Server HierarchyId desteği.
- Roslyn ve NuGet PackageReference ile geliştirilmiş uyumluluk.
- Derlemelerden geçişleri etkinleştirmek, eklemek, komut dosyası oluşturmak ve uygulamak için yardımcı program eklendi. `ef6.exe` Bu, `migrate.exe`.

### <a name="ef-designer-support"></a>EF tasarımcı desteği

Şu anda EF tasarımcısını doğrudan .NET Core veya .NET Standard projelerinde veya SDK tarzı bir .NET Framework projesinde kullanmak için destek yoktur. 

EdMX dosyasını ve varlıklar ve DbContext için oluşturulan sınıfları aynı çözüme bağlı bir .NET Core 3.0 veya .NET Standard 2.1 projesine bağlı dosyalar olarak ekleyerek bu sınırlamayı çözebilirsiniz.

Bağlı dosyalar proje dosyasında aşağıdaki gibi görünür:

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

EDMX dosyasının EntityDeploy yapı eylemiyle bağlantılı olduğunu unutmayın. Bu özel bir MSBuild görev (şimdi EF 6.3 paketine dahil) gömülü kaynaklar olarak hedef derleme içine EF modeli ekleyerek ilgilenir (veya EDMX Metadata Artifact Processing ayarına bağlı olarak çıktı klasöründe dosya olarak kopyalama). Bu kurulumu nasıl yaptırılabildiğini öğrenmek için [EDMX .NET Core örneğimize](https://aka.ms/EdmxDotNetCoreSample)bakın.

Uyarı: "Gerçek" .edmx dosyasını tanımlayan eski stilin (yani SDK tarzı olmayan) .NET Framework projesinin .sln dosyasının içindeki bağlantıyı tanımlayan projeden _önce_ geldiğinden emin olun. Aksi takdirde, tasarımcıda .edmx dosyasını açtığınızda, "Varlık Çerçevesi proje için şu anda belirtilen hedef çerçevede kullanılamaz" hata iletisini görürsünüz. Projenin hedef çerçevesini değiştirebilir veya XmlEditor'da modeli değiştirebilirsiniz".

## <a name="past-releases"></a>Geçmiş Yayınlar

[Geçmiş Sürümler](past-releases.md) sayfası, EF'nin önceki tüm sürümlerinin ve her sürümde tanıtılan önemli özelliklerin bir arşivini içerir.
