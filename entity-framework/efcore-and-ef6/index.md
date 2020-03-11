---
title: EF6 ve EF Core karşılaştırın
description: EF6 ve EF Core arasında seçim yapma kılavuzu.
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: e28c7d0299e5089f56fb0795d00917cfc30f5cf1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419649"
---
# <a name="compare-ef-core--ef6"></a>EF Core ve EF6 Karşılaştırması

## <a name="ef-core"></a>EF Core

Entity Framework Core ([EF Core](../core/index.md)), .NET için modern bir nesne veritabanıdır. LINQ sorguları, değişiklik izleme, güncelleştirmeler ve şema geçişlerini destekler.

EF Core, SQL Server/SQL Azure, SQLite, Azure Cosmos DB, MySQL, PostgreSQL ve [veritabanı sağlayıcısı eklenti modeli](../core/providers/index.md)aracılığıyla daha birçok veritabanı ile kullanılabilir.

## <a name="ef6"></a>EF6

Entity Framework 6 ([EF6](../ef6/index.md)), .NET Framework için tasarlanan, ancak .NET Core desteğiyle, nesne ilişkisel bir eşleştiricisidir. EF6, kararlı, desteklenen bir üründür, ancak artık etkin bir şekilde geliştirilmiyor.

## <a name="feature-comparison"></a>Özellik karşılaştırması

EF Core, EF6 'de uygulanmayan yeni özellikler sunar. Ancak, EF Core tüm EF6 özellikleri şu anda uygulanmıyor.

Aşağıdaki tablolar EF Core ve EF6 ' de bulunan özellikleri karşılaştırır. Bu, üst düzey bir karşılaştırmadır ve farklı EF sürümlerindeki aynı özellik arasındaki her özelliği veya açıklama farklarını listeetmez.

EF Core sütunu, özelliğin ilk göründüğü ürün sürümünü belirtir.

### <a name="creating-a-model"></a>Model oluşturma

| **Özellik**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Temel sınıf eşleme                                   | Yes      | 1.0                                   |
| Parametrelere sahip oluşturucular                          |          | 2.1                                   |
| Özellik değeri dönüştürmeleri                            |          | 2.1                                   |
| Anahtarı olmayan eşlenmiş türler                             |          | 2.1                                   |
| Kurallar                                           | Yes      | 1.0                                   |
| Özel kurallar                                    | Yes      | 1,0 (kısmi; [#214](https://github.com/dotnet/efcore/issues/214)) |
| Veri açıklamaları                                      | Yes      | 1.0                                   |
| Akıcı API                                            | Yes      | 1.0                                   |
| Devralma: hiyerarşi başına tablo (TPH)                | Yes      | 1.0                                   |
| Devralma: tür başına tablo (TPT)                     | Yes      | 5,0 için planlandı ([#2266](https://github.com/dotnet/efcore/issues/2266)) |
| Devralma: somut sınıf başına tablo (TPC)           | Yes      | 5,0 için uzat ([#3170](https://github.com/dotnet/efcore/issues/3170)) <sup>(1)</sup> |
| Gölge durumu özellikleri                               |          | 1.0                                   |
| Alternatif anahtarlar                                        |          | 1.0                                   |
| Çoktan çoğa gezinmeler                              | Yes      | 5,0 için planlandı ([#19003](https://github.com/dotnet/efcore/issues/19003)) |
| Çok-çok, JOIN varlığı olmadan                      | Yes      | Biriktirme listesi ([#1368](https://github.com/dotnet/efcore/issues/1368)) |
| Anahtar oluşturma: veritabanı                              | Yes      | 1.0                                   |
| Anahtar üretimi: Istemci                                |          | 1.0                                   |
| Karmaşık/sahip türler                                   | Yes      | 2,0                                   |
| Uzamsal veriler                                          | Yes      | 2.2                                   |
| Model biçimi: kod                                    | Yes      | 1.0                                   |
| Veritabanından model oluştur: komut satırı              | Yes      | 1.0                                   |
| Modeli veritabanından Güncelleştir                            | Kısmi  | Biriktirme listesi ([#831](https://github.com/dotnet/efcore/issues/831)) |
| Genel sorgu filtreleri                                  |          | 2,0                                   |
| Tablo bölme                                       | Yes      | 2,0                                   |
| Varlık bölme                                      | Yes      | 5,0 için uzat ([#620](https://github.com/dotnet/efcore/issues/620)) <sup>(1)</sup> |
| Veritabanı skaler işlev eşlemesi                      | Kötü     | 2,0                                   |
| Alan eşleme                                         |          | 1.1                                   |
| Null yapılabilir başvuru türleriC# (8,0)                     |          | 3.0                                   |
| Modelin grafik görselleştirmesi                      | Yes      | Planlanmış destek yok <sup>(2)</sup>     |
| Grafik Model Düzenleyicisi                                | Yes      | Planlanmış destek yok <sup>(2)</sup>     |
| Model biçimi: EDMX (XML)                              | Yes      | Planlanmış destek yok <sup>(2)</sup>     |
| Veritabanından model oluştur: VS Wizard                 | Yes      | Planlanmış destek yok <sup>(2)</sup>     |

### <a name="querying-data"></a>Veri sorgulama

| **Özellik**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ sorguları                                          | Yes      | 1.0                                   |
| Okunabilir oluşturulmuş SQL                                | Kötü     | 1.0                                   |
| GroupBy çevirisi                                   | Yes      | 2.1                                   |
| İlgili verileri yükleme: Eager                           | Yes      | 1.0                                   |
| İlgili verileri yükleme: türetilmiş türler için ekip yükleme |          | 2.1                                   |
| İlgili verileri yükleme: yavaş                            | Yes      | 2.1                                   |
| İlgili verileri yükleme: açık                        | Yes      | 1.1                                   |
| Ham SQL sorguları: varlık türleri                         | Yes      | 1.0                                   |
| Ham SQL sorguları: Keyless varlık türleri                 | Yes      | 2.1                                   |
| Ham SQL sorguları: LINQ ile oluşturma                  |          | 1.0                                   |
| Açıkça derlenmiş sorgular                           | Kötü     | 2,0                                   |
| await foreach (C# 8,0)                                |          | 3.0                                   |
| Metin tabanlı sorgu dili (Entity SQL)                | Yes      | Planlanmış destek yok <sup>(2)</sup>     |

### <a name="saving-data"></a>Verileri kaydetme

| **Özellik**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Değişiklik izleme: anlık görüntü                             | Yes      | 1.0                                   |
| Değişiklik izleme: bildirim                         | Yes      | 1.0                                   |
| Değişiklik izleme: proxy 'Ler                              | Yes      | 5,0 için birleştirildi ([#10949](https://github.com/dotnet/efcore/issues/10949)) |
| İzlenen duruma erişme                               | Yes      | 1.0                                   |
| İyimser eşzamanlılık                                | Yes      | 1.0                                   |
| İşlemler                                          | Yes      | 1.0                                   |
| Deyimlerin toplu işi                                |          | 1.0                                   |
| Saklı yordam eşleme                              | Yes      | Biriktirme listesi ([#245](https://github.com/dotnet/efcore/issues/245)) |
| Bağlantısı kesilmiş grafik alt düzey API 'Ler                     | Kötü     | 1.0                                   |
| Bağlantısı kesilen Graph uçtan uca                         |          | 1,0 (kısmi; [#5536](https://github.com/dotnet/efcore/issues/5536)) |

### <a name="other-features"></a>Diğer özellikler

| **Özellik**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Geçişler                                            | Yes      | 1.0                                   |
| Veritabanı oluşturma/silme API 'Leri                       | Yes      | 1.0                                   |
| Çekirdek verileri                                             | Yes      | 2.1                                   |
| Bağlantı dayanıklılığı                                 | Yes      | 1.1                                   |
| Durdurucular                                          | Yes      | 3.0                                   |
| Olaylar                                                | Yes      | 3,0 (kısmi; [#626](https://github.com/dotnet/efcore/issues/626)) |
| Basit günlüğe kaydetme (Database. log)                         | Yes      | 5,0 için birleştirildi ([#1199](https://github.com/dotnet/efcore/issues/1199)) |
| DbContext havuzu                                     |          | 2,0                                   |

### <a name="database-providers-sup3sup"></a>Veritabanı sağlayıcıları <sup>(3)</sup>

| **Özellik**                                           | **EF 6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Yes      | 1.0                                   |
| MySQL                                                 | Yes      | 1.0                                   |
| PostgreSQL                                            | Yes      | 1.0                                   |
| Oracle                                                | Yes      | 1.0                                   |
| SQLite                                                | Yes      | 1.0                                   |
| SQL Server Compact                                    | Yes      | 1,0 <sup>(4)</sup>                    |
| DB2                                                   | Yes      | 1.0                                   |
| Firebird                                              | Yes      | 2,0                                   |
| Jet (Microsoft Access)                                |          | 2,0 <sup>(4)</sup>                    |
| Azure Cosmos DB                                       |          | 3.0                                   |
| Bellek içi (test için)                               |          | 1.0                                   |

<sup>1</sup> Esnetme hedefi, belirli bir sürüm için ulaşılması olası değildir. Ancak, şeyler düzgün bir şekilde gittiğinizde, bunları içine çekmeye çalışacaktır.

<sup>2</sup> bazı EF6 özellikleri EF Core uygulanmaz. Bu özellikler, EF6's temel Varlık Veri Modeli (EDM) bağımlıdır ve/veya, daha az yatırım getirisi olan karmaşık özelliklerdir. Her zaman geri bildirimde bulunun, ancak EF Core EF6 'te mümkün olmayan birçok şeyi etkinleştirirken, bu, EF Core tüm EF6 özelliklerini desteklemeye uygun değildir.

<sup>3</sup> EF Core üçüncü taraflar tarafından uygulanan veritabanı sağlayıcıları, EF Core yeni ana sürümlerine güncelleştirme sırasında gecikebilir. Daha fazla bilgi için bkz. [veritabanı sağlayıcıları](../core/providers/index.md) .

<sup>4</sup> SQL Server Compact ve Jet sağlayıcıları yalnızca .NET Framework (.NET Core üzerinde değil) üzerinde çalışır.

### <a name="supported-platforms"></a>Desteklenen platformlar

EF Core 3,1, .NET Core ve .NET Framework üzerinde .NET Standard 2,0 ' i kullanarak çalışır. Ancak, EF Core 5,0 .NET Framework çalışmaz. Daha fazla ayrıntı için [platformları](../core/platforms/index.md) inceleyin.

EF 6.4, .NET Core üzerinde çalışır ve Çoklu hedefleme aracılığıyla .NET Framework.

## <a name="guidance-for-new-applications"></a>Yeni uygulamalar için rehberlik

Uygulamanın [yalnızca .NET Framework desteklenen](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server)bir şeyi kullanması gerekmiyorsa, tüm yeni uygulamalar Için .NET Core üzerinde EF Core kullanın.

## <a name="guidance-for-existing-ef6-applications"></a>Mevcut EF6 uygulamaları için rehberlik

EF Core, EF6 için bir bırakma değişikliği değildir. EF6 ' dan EF Core ' a geçiş, büyük olasılıkla uygulamanızda değişiklikler yapılmasını gerektirir.

Bir EF6 uygulamasını .NET Core 'a taşırken:
* Veri erişim kodu kararlı ise veya yeni özelliklere ihtiyaç duymadığı durumlarda EF6 kullanmaya devam edin.
* Veri erişim kodu gelişiyor veya uygulamanın yalnızca EF Core kullanılabilir olan yeni özelliklere ihtiyacı varsa EF Core için bağlantı noktası.
* EF Core taşıma işlemi de genellikle performans için yapılır. Ancak, tüm senaryolar daha hızlıdır, bu nedenle önce profil oluşturma işlemini yapın.

Daha fazla bilgi için bkz. [EF6 'den EF Core 'e taşıma](porting/index.md) .
