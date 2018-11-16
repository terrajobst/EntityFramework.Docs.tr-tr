---
title: Entity Framework 6 ve Entity Framework Core karşılaştırın
description: Entity Framework 6 ile Entity Framework Core arasında seçim yapma konusunda rehberlik sağlar.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: d5fe9b388707f653fdeb2d6a5daa7215ced71c1d
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688725"
---
# <a name="compare-ef-core--ef6"></a>EF Core ve EF6 Karşılaştırması

Varlık, nesne ilişkisel eşleyicidir (O/RM) için .NET çerçevesidir. Bu makalede iki sürüm karşılaştırır: Entity Framework Core ve Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6), veri erişimi teknolojisidir. İlk 2008, .NET Framework 3.5 SP1 ve Visual Studio 2008 SP1'in bir parçası olarak yayımlanmıştır. Sevk olarak 4.1 sürüm ile başlayarak [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet paketi. EF6 çalışan .NET Framework 4.x, yalnızca Windows üzerinde çalıştığı anlamına gelir. 

EF6 desteklenen bir ürün olmaya devam eder ve hata düzeltmeleri ve küçük iyileştirmeler görmeye devam edecektir.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) 2016'da ilk yayımlanan EF6 tamamını yeniden yazmak ' dir. Bir ana olan Nuget paketlerinde sunulur [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/). EF Core .NET Core veya .NET Framework üzerinde çalışan platformlar arası bir üründür.

EF Core için EF6 benzer bir geliştirici deneyimi sağlamak için tasarlanmıştır. En üst düzey API'lerinin aynı kalır. böylece, EF Core EF6 kullanmış geliştiricilerinin aşina devam edebilir.

## <a name="feature-comparison"></a>Özellik karşılaştırması

EF Core EF6 içinde uygulanan gerekmez yeni özellikler sunar (gibi [alternatif anahtarlar](xref:core/modeling/alternate-keys), [toplu güncelleştirmeler](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements), ve [istemci/veritabanı değerlendirme LINQ sorgularında karma](xref:core/querying/client-eval). Ancak, yeni bir kod temeli olduğundan, bu da EF6 olan bazı özellikler eksik.

Aşağıdaki tabloda EF6 ve EF Core ile kullanılabilen özellikleri karşılaştırın. Üst düzey bir karşılaştırma ve değil her özelliğin liste veya aynı özellikte farklı EF sürümleri arasındaki farkları açıklamaktadır.

EF Core sütunu, özelliği ilk kez göründü ürün sürümü gösterir.

### <a name="creating-a-model"></a>Model oluşturma

| **Özelliği**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Temel sınıf eşleme                                   | Evet      | 1.0                                   |
| Parametreli oluşturucular                          |          | 2.1                                   |
| Özellik değer dönüştürmeleri                            |          | 2.1                                   |
| Eşlenmiş türlerdeki anahtar olmadan (sorgu türleri)               |          | 2.1                                   |
| Kurallar                                           | Evet      | 1.0                                   |
| Özel kuralları                                    | Evet      | 1.0 (kısmi)                         |
| Veri açıklamaları                                      | Evet      | 1.0                                   |
| Fluent API'si                                            | Evet      | 1.0                                   |
| Devralma: Tablo başına hiyerarşi (TPH)                | Evet      | 1.0                                   |
| Devralma: Tablo başına tür (TPT)                     | Evet      |                                       |
| Devralma: Tablo başına somut sınıf (TPC)           | Evet      |                                       |
| Gölge durumu özellikleri                               |          | 1.0                                   |
| Alternatif anahtarlar                                        |          | 1.0                                   |
| Varlık çok-çok olmadan katılın                      | Evet      |                                       |
| Anahtar oluşturma: veritabanı                              | Evet      | 1.0                                   |
| Anahtar oluşturma: istemci                                |          | 1.0                                   |
| Karmaşık ve ait türleri                                   | Evet      | 2,0                                   |
| Uzamsal veriler                                          | Evet      | 2.2                                   |
| Modelin grafik görselleştirmesi                      | Evet      |                                       |
| Grafik model Düzenleyicisi                                | Evet      |                                       |
| Model biçimi: kod                                    | Evet      | 1.0                                   |
| Model biçimi: EDMX (XML)                              | Evet      |                                       |
| Model veritabanından oluştur: komut satırı              | Evet      | 1.0                                   |
| Model veritabanından oluşturma: VS Sihirbazı                 | Evet      |                                       |
| Veritabanından bir güncelleştirme modeli                            | Kısmi  |                                       |
| Genel sorgu filtreleri                                  |          | 2,0                                   |
| Tablo bölme                                       | Evet      | 2,0                                   |
| Varlık bölme                                      | Evet      |                                       |
| Veritabanı skaler bir işlev eşlemesi                      | Kötü     | 2,0                                   |
| Alan eşleme                                         |          | 1.1                                   |

### <a name="querying-data"></a>Verileri sorgulama

| **Özelliği**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ sorguları                                          | Evet      | 1.0 (devam eden karmaşık sorgular için) |
| Okunabilir oluşturulan SQL                                | Kötü     | 1.0                                   |
| Karma istemci/sunucu değerlendirme                        |          | 1.0                                   |
| GroupBy çeviri                                   | Evet      | 2.1                                   |
| İlgili verileri yükleme: istekli                           | Evet      | 1.0                                   |
| İlgili verileri yükleme: Eager için yükleme türetilmiş türler |          | 2.1                                   |
| İlgili verileri yükleme: yavaş                            | Evet      | 2.1                                   |
| İlgili verileri yükleme: açık                        | Evet      | 1.1                                   |
| Ham SQL sorguları: varlık türleri                         | Evet      | 1.0                                   |
| Ham SQL sorguları: varlık olmayan türleri (sorgu türleri)       | Evet      | 2.1                                   |
| Ham SQL sorguları: LINQ ile oluşturma                  |          | 1.0                                   |
| Açıkça derlenmiş sorgular                           | Kötü     | 2,0                                   |
| Metin tabanlı sorgu dili (varlık SQL)                | Evet      |                                       |

### <a name="saving-data"></a>Verileri kaydetme

| **Özelliği**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Değişiklik izleme: anlık görüntü                             | Evet      | 1.0                                   |
| Değişiklik izleme: bildirim                         | Evet      | 1.0                                   |
| Değişiklik izleme: proxy                              | Evet      |                                       |
| İzlenen durumuna erişme                               | Evet      | 1.0                                   |
| İyimser eşzamanlılık                                | Evet      | 1.0                                   |
| İşlemler                                          | Evet      | 1.0                                   |
| Toplu işleme deyimleri                                |          | 1.0                                   |
| Saklı yordam eşlemesi                              | Evet      |                                       |
| Bağlantısı kesilen alt düzey API'ler grafiği                     | Kötü     | 1.0                                   |
| Bağlantısı kesilmiş graf uçtan uca                         |          | 1.0 (kısmi)                         |

### <a name="other-features"></a>Diğer özellikler

| **Özelliği**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Geçişler                                            | Evet      | 1.0                                   |
| Veritabanı oluşturma/silme API'leri                       | Evet      | 1.0                                   |
| Veri kaynağı                                             | Evet      | 2.1                                   |
| Bağlantı dayanıklılığı                                 | Evet      | 1.1                                   |
| Yaşam döngüsü kancaları (olayları, durdurma)                | Evet      |                                       |
| Basit günlüğe kaydetme (Database.Log)                         | Evet      |                                       |
| DbContext havuzu                                     |          | 2,0                                   |

### <a name="database-providers"></a>Veritabanı sağlayıcıları

| **Özelliği**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Evet      | 1.0                                   |
| MySQL                                                 | Evet      | 1.0                                   |
| PostgreSQL                                            | Evet      | 1.0                                   |
| Oracle                                                | Evet      | 1.0 <sup>(1)</sup>                    |
| SQLite                                                | Evet      | 1.0                                   |
| SQL Server Compact                                    | Evet      | 1.0 <sup>(2)</sup>                    |
| DB2                                                   | Evet      | 1.0                                   |
| Firebird                                              | Evet      | 2,0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(2)</sup>                    |
| (Sınama) bellek                               |          | 1.0                                   |

<sup>1</sup> şu anda bir Ücretli sağlayıcısı yok, Oracle için kullanılabilir. Oracle için ücretsiz bir resmi sağlayıcısı üzerinde çalışılan.

<sup>2</sup> yalnızca SQL Server Compact ve Jet sağlayıcıları iş .NET Framework (değil, .NET Core).

### <a name="net-implementations"></a>.NET uygulamaları

| **Özelliği**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| .NET framework (konsol, WinForms, WPF, ASP.NET)      | Evet      | 1.0                                   |
| .NET core (konsol, ASP.NET Core)                     |          | 1.0                                   |
| Mono ve Xamarin                                        |          | 1.0 (Sürüyor)                     |
| UWP                                                   |          | 1.0 (Sürüyor)                     |

## <a name="guidance-for-new-applications"></a>Yeni uygulamalar için yönergeler

Aşağıdaki koşulların her ikisi de doğruysa EF Core için yeni bir uygulama kullanmayı deneyin:
* Uygulamanın, .NET Core yeteneklerini gerekir. Daha fazla bilgi için [sunucu uygulamaları için .NET Core ve .NET Framework arasında seçim](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core uygulamasını özelliklerin tümünü destekler. İstenen özellik eksik olursa denetleyin [EF Core yol haritası](xref:core/what-is-new/roadmap) gelecekte desteklemeye yönelik planlar olup olmadığını öğrenin. 

Aşağıdaki koşulların her ikisi de doğruysa EF6 kullanarak göz önünde bulundurun:
* Uygulama, Windows ve .NET Framework 4.0 veya sonraki sürümü üzerinde çalıştırılır.
* EF6 uygulamasını özelliklerin tümünü destekler.

## <a name="guidance-for-existing-ef6-applications"></a>EF6 uygulamalarınız için yönergeler

EF Core temel değişiklikler nedeniyle değişiklik yapmak için yeterli bir neden olmadıkça EF6 uygulamanın EF core'a taşıma önermiyoruz. EF Core için yeni özellikleri kullanmak için taşımak istiyorsanız, kendi sınırlamaları dikkate emin olun. Daha fazla bilgi için [EF6'dan EF Core'a taşıma](porting/index.md). **EF6'dan EF core'a taşıma yükseltme değerinden daha fazla bağlantı noktasıdır.** 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için belgelere bakın:
* <xref:core/index>
* <xref:ef6/index>
