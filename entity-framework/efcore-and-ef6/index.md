---
title: Entity Framework 6 ve Entity Framework Core karşılaştırın
description: Entity Framework 6 ve Entity Framework Core arasında seçim yapma hakkında rehberlik sağlar.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: ad0807a3cfd62c6c09a97df1a45134db7a538623
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149056"
---
# <a name="compare-ef-core--ef6"></a>EF Core ve EF6 Karşılaştırması

Entity Framework, .NET için nesne ilişkisel Eşleyici (O/RM). Bu makalede iki sürüm karşılaştırılır: Entity Framework 6 ve Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6), denenmemiş ve test edilmiş bir veri erişim teknolojisidir. İlk olarak 2008 ' de .NET Framework 3,5 SP1 ve Visual Studio 2008 SP1 'in bir parçası olarak yayımlanmıştır. 4,1 sürümünden başlayarak [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet paketi olarak sevk edildi. EF6 .NET Framework 4. x ve .NET Core üzerinde 3,0 ve sonraki sürümlerde çalışır.

EF6 desteklenen bir ürün olmaya devam eder ve hata düzeltmelerini ve küçük iyileştirmeleri görmeye devam edecektir.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core), ilk olarak 2016 ' de yayınlanan EF6 'in tamamen yeniden yazma işlemi olur. Bu, ana diğeri [Microsoft. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/)olan NuGet paketlerinde gelir. EF Core .NET Core üzerinde çalışan platformlar arası bir üründür.

EF Core, EF6 benzer bir geliştirici deneyimi sağlamak için tasarlanmıştır. En üst düzey API 'lerin çoğu aynı kalır, bu nedenle EF Core, EF6 kullanan geliştiricilere tanıdık gelecektir.

## <a name="feature-comparison"></a>Özellik karşılaştırması

EF Core, EF6 içinde uygulanmayan yeni özellikler sunar ( [Alternatif anahtarlar](xref:core/modeling/alternate-keys), [toplu güncelleştirmeler](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements)ve [LINQ sorgularında karışık istemci/veritabanı değerlendirmesi](xref:core/querying/client-eval)gibi). Ancak, yeni bir kod tabanı olduğundan, EF6 'in sahip olduğu bazı özellikler de yoktur.

Aşağıdaki tablolar EF Core ve EF6 ' de bulunan özellikleri karşılaştırır. Bu, üst düzey bir karşılaştırmalardır ve farklı EF sürümlerindeki aynı özellik arasındaki her özelliği veya açıklama farklarını listeetmez.

EF Core sütunu, özelliğin ilk göründüğü ürün sürümünü belirtir.

### <a name="creating-a-model"></a>Model oluşturma

| **Özelliği**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Temel sınıf eşleme                                   | Evet      | 1.0                                   |
| Parametrelere sahip oluşturucular                          |          | 2.1                                   |
| Özellik değeri dönüştürmeleri                            |          | 2.1                                   |
| Anahtarı olmayan eşlenmiş türler                             |          | 2.1                                   |
| Kurallar                                           | Evet      | 1.0                                   |
| Özel kurallar                                    | Evet      | 1,0 (kısmi)                         |
| Veri açıklamaları                                      | Evet      | 1.0                                   |
| Akıcı API                                            | Evet      | 1.0                                   |
| Devralmayı Hiyerarşi başına tablo (TPH)                | Evet      | 1.0                                   |
| Devralmayı Tür başına tablo (TPT)                     | Evet      |                                       |
| Devralmayı Somut sınıf başına tablo (TPC)           | Evet      |                                       |
| Gölge durumu özellikleri                               |          | 1.0                                   |
| Alternatif anahtarlar                                        |          | 1.0                                   |
| Çok-çok, JOIN varlığı olmadan                      | Evet      |                                       |
| Anahtar oluşturma: Veritabanı                              | Evet      | 1.0                                   |
| Anahtar oluşturma: İstemci                                |          | 1.0                                   |
| Karmaşık/sahip türler                                   | Evet      | 2,0                                   |
| Uzamsal veriler                                          | Evet      | 2.2                                   |
| Modelin grafik görselleştirmesi                      | Evet      |                                       |
| Grafik Model Düzenleyicisi                                | Evet      |                                       |
| Model biçimi: Kod                                    | Evet      | 1.0                                   |
| Model biçimi: EDMX (XML)                              | Evet      |                                       |
| Veritabanından model oluştur: Komut satırı              | Evet      | 1.0                                   |
| Veritabanından model oluştur: VS Sihirbazı                 | Evet      |                                       |
| Modeli veritabanından Güncelleştir                            | Kısmi  |                                       |
| Genel sorgu filtreleri                                  |          | 2,0                                   |
| Tablo bölme                                       | Evet      | 2,0                                   |
| Varlık bölme                                      | Evet      |                                       |
| Veritabanı skaler işlev eşlemesi                      | Kötü     | 2,0                                   |
| Alan eşleme                                         |          | 1.1                                   |
| Null yapılabilir başvuru türleriC# (8,0)                     |          | 3.0                                   |

### <a name="querying-data"></a>Verileri sorgulama

| **Özelliği**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ sorguları                                          | Evet      | 1,0 (karmaşık sorgular için devam ediyor) |
| Okunabilir oluşturulmuş SQL                                | Kötü     | 1.0                                   |
| GroupBy çevirisi                                   | Evet      | 2.1                                   |
| İlgili veriler yükleniyor: Eager                           | Evet      | 1.0                                   |
| İlgili veriler yükleniyor: Türetilmiş türler için ekip yükleme |          | 2.1                                   |
| İlgili veriler yükleniyor: Yavaş                            | Evet      | 2.1                                   |
| İlgili veriler yükleniyor: Anlaşılır                        | Evet      | 1.1                                   |
| Ham SQL sorguları: Varlık türleri                         | Evet      | 1.0                                   |
| Ham SQL sorguları: Keyless varlık türleri                 | Evet      | 2.1                                   |
| Ham SQL sorguları: LINQ ile oluşturma                  |          | 1.0                                   |
| Açıkça derlenmiş sorgular                           | Kötü     | 2,0                                   |
| Metin tabanlı sorgu dili (Entity SQL)                | Evet      |                                       |
| await foreach (C# 8,0)                                |          | 3.0                                   |

### <a name="saving-data"></a>Verileri kaydetme

| **Özelliği**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Değişiklik izleme: Anlık Görüntü                             | Evet      | 1.0                                   |
| Değişiklik izleme: Bildirim                         | Evet      | 1.0                                   |
| Değişiklik izleme: Proxy'ler                              | Evet      |                                       |
| İzlenen duruma erişme                               | Evet      | 1.0                                   |
| İyimser eşzamanlılık                                | Evet      | 1.0                                   |
| İşlemler                                          | Evet      | 1.0                                   |
| Deyimlerin toplu işi                                |          | 1.0                                   |
| Saklı yordam eşleme                              | Evet      |                                       |
| Bağlantısı kesilmiş grafik alt düzey API 'Ler                     | Kötü     | 1.0                                   |
| Bağlantısı kesilen Graph uçtan uca                         |          | 1,0 (kısmi)                         |

### <a name="other-features"></a>Diğer özellikler

| **Özelliği**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Geçişler                                            | Evet      | 1.0                                   |
| Veritabanı oluşturma/silme API 'Leri                       | Evet      | 1.0                                   |
| Çekirdek verileri                                             | Evet      | 2.1                                   |
| Bağlantı dayanıklılığı                                 | Evet      | 1.1                                   |
| Yaşam döngüsü kancaları (olaylar, yakalar)                | Evet      |                                       |
| Basit günlüğe kaydetme (Database. log)                         | Evet      |                                       |
| DbContext havuzu                                     |          | 2,0                                   |

### <a name="database-providers"></a>Veritabanı sağlayıcıları

| **Özelliği**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Evet      | 1.0                                   |
| MySQL                                                 | Evet      | 1.0                                   |
| PostgreSQL                                            | Evet      | 1.0                                   |
| Oracle                                                | Evet      | 1.0                                   |
| SQLite                                                | Evet      | 1.0                                   |
| SQL Server Compact                                    | Evet      | 1,0 <sup>(1)</sup>                    |
| DB2                                                   | Evet      | 1.0                                   |
| Firebird                                              | Evet      | 2,0                                   |
| Jet (Microsoft Access)                                |          | 2,0 <sup>(1)</sup>                    |
| Cosmos DB                                             |          | 3.0                                   |
| Bellek içi (test için)                               |          | 1.0                                   |

<sup>1</sup> SQL Server Compact ve Jet sağlayıcıları yalnızca .NET Framework (.NET Core üzerinde değil) üzerinde çalışır.

### <a name="net-implementations"></a>.NET uygulamaları

| **Özelliği**                                           | **EF6**            | **EF Core**                           |
|:------------------------------------------------------|:-------------------|:--------------------------------------|
| .NET Framework                                        | Evet                | 1,0 (3,0 ' de kaldırılır)                  |
| .NET Core                                             | Evet (6,3 'e eklendi) | 1.0                                   |
| Mono & Xamarin                                        |                    | 1,0 (devam ediyor)                     |
| UWP                                                   |                    | 1,0 (devam ediyor)                     |

## <a name="guidance-for-new-applications"></a>Yeni uygulamalar için rehberlik

Aşağıdaki koşulların her ikisi de doğruysa yeni bir uygulama için EF Core kullanmayı düşünün:
* Uygulamanın .NET Core 'un özelliklerine ihtiyacı vardır. Daha fazla bilgi için bkz. [sunucu uygulamaları için .NET Core ve .NET Framework seçme](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core, uygulamanın gerektirdiği tüm özellikleri destekler. İstenen bir özellik eksikse, daha sonra bunu desteklemeye yönelik planlar olup olmadığını öğrenmek için [EF Core yol haritasını](xref:core/what-is-new/roadmap) denetleyin.

Aşağıdaki koşulların her ikisi de doğruysa EF6 kullanmayı düşünün:
* Uygulama Windows ve .NET Framework 4,0 veya üzeri sürümlerde çalışır.
* EF6, uygulamanın gerektirdiği tüm özellikleri destekler.

## <a name="guidance-for-existing-ef6-applications"></a>Mevcut EF6 uygulamaları için rehberlik

EF Core temel değişiklikler nedeniyle, değişikliği yapmak için etkileyici bir neden olmadıkça bir EF6 uygulamasının EF Core taşınmasını önermiyoruz. Yeni özellikleri kullanmak için EF Core taşımak istiyorsanız, onun sınırlamalarını farkında olduğunuzdan emin olun. Daha fazla bilgi için bkz. [EF6 'dan EF Core 'e taşıma](porting/index.md). **EF6 öğesinden EF Core taşıma işlemi, yükseltenden daha fazla bağlantı noktasıdır.**

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için belgelere bakın:
* <xref:core/index>
* <xref:ef6/index>
