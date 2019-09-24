---
title: Yenilikler-EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: bb7038764644682c2149a8a500f342804d01f3d2
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198042"
---
# <a name="whats-new-in-ef6"></a>EF6 'deki yenilikler

En son özellikleri ve en yüksek kararlılığı elde etmenizi sağlamak için en son yayınlanan Entity Framework sürümünü kullanmanızı kesinlikle öneririz.
Bununla birlikte, önceki bir sürümü kullanmanız gerektiğini veya en son yayın öncesi sürümde yeni geliştirmeler yapmak isteyebileceğiniz hakkında fark ediyoruz.
EF 'in belirli sürümlerini yüklemek için bkz. [Get Entity Framework](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>EF 6.3.0

EF 6.3.0 Runtime, Eylül 2019 ' de NuGet 'e yayımlandı. Bu sürümün ana amacı, EF 6 kullanan mevcut uygulamaların .NET Core 3,0 ' e geçirilmesini kolaylaştırmıştı. Topluluk ayrıca çeşitli hata düzeltmeleri ve geliştirmeleri de katkıda bulunur. Ayrıntılar için her 6.3.0 [kilometre taşında](https://github.com/aspnet/EntityFramework6/milestones?state=closed) kapatılan sorunlara bakın. Burada, daha fazla bilgi yer verilmiştir:

- .NET Core 3,0 desteği
  - EntityFramework paketi artık .NET Framework 4. x ' e ek olarak .NET Standard 2,1 ' i hedefliyor
  - Geçişler komutları, işlem dışında yürütülecek ve SDK stili projelerle çalışacak şekilde yeniden yazıldı
- SQL Server HierarchyId desteği
- Roslyn ve NuGet PackageReference ile iyileştirilmiş uyumluluk
- Derlemelerden `ef6.exe` geçişleri etkinleştirmek, eklemek, betik oluşturmak ve uygulamak için yardımcı program eklendi. Bunun yerini alır`migrate.exe`

### <a name="ef-designer-support"></a>EF Designer desteği

Şu anda EF Designer 'ı doğrudan .NET Core veya .NET Standard projelerinde kullanma desteği yoktur. 

Aynı çözümde bir .NET Core 3,0 veya .NET Standard 2,1 projesine, öğeler için EDMX dosyasını ve üretilen sınıfları ve DbContext 'i bağlı dosyalar olarak ekleyerek bu sınırlamaya geçici bir çözüm bulabilirsiniz.

Bağlantılı dosyalar proje dosyasında şöyle görünür:

``` csproj 
&lt;ItemGroup&gt;
  &lt;EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" /&gt;
  &lt;Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" /&gt;
&lt;/ItemGroup&gt;
```

EDMX dosyasının EntityDeploy derleme eylemiyle bağlantılı olduğunu unutmayın. Bu, EF modelinin hedef derlemeye gömülü kaynaklar olarak eklenmesi (veya EDMX 'in meta veri yapıt Işleme ayarına bağlı olarak çıktı klasörüne dosya olarak kopyalamak) için özel bir MSBuild görevi (artık EF 6,3 paketine eklenmiştir). Bu kurulumu nasıl alacağınız hakkında daha fazla bilgi için bkz. [edmx .NET Core örneği](https://aka.ms/EdmxDotNetCoreSample).

## <a name="past-releases"></a>Geçmiş Yayınlar

[Eski yayınlar](past-releases.md) sayfası, tüm önceki EF sürümlerinin ve her yayında tanıtılan ana özelliklerin bir arşivini içerir.
