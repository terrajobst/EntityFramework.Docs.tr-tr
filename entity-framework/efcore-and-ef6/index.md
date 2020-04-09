---
title: EF6 ve EF Core'u karşılaştırın
description: EF6 ve EF Core arasında nasıl seçim yapılacağınıanlatan yönerge.
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: e28c7d0299e5089f56fb0795d00917cfc30f5cf1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419649"
---
# <a name="compare-ef-core--ef6"></a>EF Core ve EF6 Karşılaştırması

## <a name="ef-core"></a>EF Core

Entity Framework Core[(EF Core),](../core/index.md).NET için modern bir nesne-veritabanı mapper'ıdır. LINQ sorgularını, izlemeyi, güncelleştirmeleri ve şema geçişlerini değiştirir.

EF Core, sql server/SQL Azure, SQLite, Azure Cosmos DB, MySQL, PostgreSQL ve daha birçok veritabanı ile bir [veritabanı sağlayıcısı eklenti modeli](../core/providers/index.md)aracılığıyla çalışır.

## <a name="ef6"></a>EF6

Varlık Çerçevesi 6[(EF6),](../ef6/index.md).NET Framework için tasarlanmış ancak .NET Core desteği ile tasarlanmış nesne ilişkisisel bir mapper'dır. EF6 kararlı, desteklenen bir üründür, ancak artık aktif olarak geliştirilmemektedir.

## <a name="feature-comparison"></a>Özellik karşılaştırması

EF Core, EF6'da uygulanmayacak yeni özellikler sunar. Ancak, tüm EF6 özellikleri şu anda EF Core'da uygulanmamaktadır.

Aşağıdaki tablolar, EF Core ve EF6'da bulunan özellikleri karşılaştırın. Bu üst düzey bir karşılaştırmadır ve farklı EF sürümlerinde aynı özellik arasındaki her özelliği listelemiyor veya farklılıkları açıklamaz.

EF Core sütunu, özelliğin ilk göründüğü ürün sürümünü gösterir.

### <a name="creating-a-model"></a>Model oluşturma

| **Özellik**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Temel sınıf eşleme                                   | Evet      | 1.0                                   |
| Parametreleri olan yapıcılar                          |          | 2.1                                   |
| Özellik değeri dönüşümleri                            |          | 2.1                                   |
| Anahtarsız eşlenmiş türler                             |          | 2.1                                   |
| Kurallar                                           | Evet      | 1.0                                   |
| Özel kurallar                                    | Evet      | 1.0 (kısmi; [#214](https://github.com/dotnet/efcore/issues/214)) |
| Veri açıklamaları                                      | Evet      | 1.0                                   |
| Akıcı API                                            | Evet      | 1.0                                   |
| Kalıtım: Hiyerarşi başına tablo (TPH)                | Evet      | 1.0                                   |
| Kalıtım: Tür başına tablo (TPT)                     | Evet      | 5.0 için planlanan ([#2266](https://github.com/dotnet/efcore/issues/2266)) |
| Kalıtım: Beton sınıfına göre tablo (TPC)           | Evet      | 5.0 için streç ([#3170](https://github.com/dotnet/efcore/issues/3170)) <sup>(1)</sup> |
| Gölge durumu özellikleri                               |          | 1.0                                   |
| Alternatif tuşlar                                        |          | 1.0                                   |
| Çok-çok navigasyon                              | Evet      | 5.0 için planlanan ([#19003](https://github.com/dotnet/efcore/issues/19003)) |
| Birleştirme varlık olmadan çok-çok                      | Evet      | Biriktirme listesiüzerinde ([#1368](https://github.com/dotnet/efcore/issues/1368)) |
| Anahtar oluşturma: Veritabanı                              | Evet      | 1.0                                   |
| Anahtar oluşturma: İstemci                                |          | 1.0                                   |
| Karmaşık/sahip olunan türler                                   | Evet      | 2,0                                   |
| Uzamsal veriler                                          | Evet      | 2,2                                   |
| Model biçimi: Kod                                    | Evet      | 1.0                                   |
| Veritabanından model oluşturma: Komut satırı              | Evet      | 1.0                                   |
| Veritabanından modeli güncelleştirme                            | Kısmi  | Biriktirme listesiüzerinde ([#831](https://github.com/dotnet/efcore/issues/831)) |
| Genel sorgu filtreleri                                  |          | 2,0                                   |
| Tablo bölme                                       | Evet      | 2,0                                   |
| Varlık bölme                                      | Evet      | 5.0 için streç ([#620](https://github.com/dotnet/efcore/issues/620)) <sup>(1)</sup> |
| Veritabanı skaler fonksiyon eşleme                      | Kötü     | 2,0                                   |
| Alan eşlemesi                                         |          | 1.1                                   |
| Nullable referans türleri (C# 8.0)                     |          | 3,0                                   |
| Modelin grafiksel görselleştirmesi                      | Evet      | Destek planlanmadı <sup>(2)</sup>     |
| Grafik model düzenleyicisi                                | Evet      | Destek planlanmadı <sup>(2)</sup>     |
| Model biçimi: EDMX (XML)                              | Evet      | Destek planlanmadı <sup>(2)</sup>     |
| Veritabanından model oluşturma: VS sihirbazı                 | Evet      | Destek planlanmadı <sup>(2)</sup>     |

### <a name="querying-data"></a>Veri sorgulama

| **Özellik**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ sorguları                                          | Evet      | 1.0                                   |
| Okunabilir oluşturulan SQL                                | Kötü     | 1.0                                   |
| GroupBy çevirisi                                   | Evet      | 2.1                                   |
| İlgili verilerin yüklenmesi: Istekli                           | Evet      | 1.0                                   |
| İlgili verilerin yüklenmesi: Türemiş türler için istekli yükleme |          | 2.1                                   |
| İlgili verilerin yüklenmesi: Tembel                            | Evet      | 2.1                                   |
| Yükleme ile ilgili veriler: Açık                        | Evet      | 1.1                                   |
| Ham SQL sorguları: Varlık türleri                         | Evet      | 1.0                                   |
| Raw SQL sorguları: Anahtarsız varlık türleri                 | Evet      | 2.1                                   |
| Ham SQL sorguları: LINQ ile beste                  |          | 1.0                                   |
| Açıkça derlenmiş sorgular                           | Kötü     | 2,0                                   |
| foreach bekliyor (C# 8.0)                                |          | 3,0                                   |
| Metin tabanlı sorgu dili (Entity SQL)                | Evet      | Destek planlanmadı <sup>(2)</sup>     |

### <a name="saving-data"></a>Verileri kaydetme

| **Özellik**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| İzlemeyi değiştir: Anlık görüntü                             | Evet      | 1.0                                   |
| İzlemeyi değiştir: Bildirim                         | Evet      | 1.0                                   |
| İzlemeyi değiştir: Yakınlıklar                              | Evet      | 5.0 için birleştirildi ([#10949](https://github.com/dotnet/efcore/issues/10949)) |
| İzlenen duruma erişme                               | Evet      | 1.0                                   |
| İyimser eşzamanlılık                                | Evet      | 1.0                                   |
| İşlemler                                          | Evet      | 1.0                                   |
| İfadelerin toplu olarak gruplanması                                |          | 1.0                                   |
| Depolanan yordam eşleme                              | Evet      | Biriktirme listesiüzerinde ([#245](https://github.com/dotnet/efcore/issues/245)) |
| Bağlantısız grafik alt düzey API'ler                     | Kötü     | 1.0                                   |
| Bağlantısız grafik Uçuça                         |          | 1.0 (kısmi; [#5536](https://github.com/dotnet/efcore/issues/5536)) |

### <a name="other-features"></a>Diğer özellikler

| **Özellik**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Geçişler                                            | Evet      | 1.0                                   |
| Veritabanı oluşturma/silme API'leri                       | Evet      | 1.0                                   |
| Tohum verileri                                             | Evet      | 2.1                                   |
| Bağlantı dayanıklılığı                                 | Evet      | 1.1                                   |
| Durdurucular                                          | Evet      | 3,0                                   |
| Olaylar                                                | Evet      | 3.0 (kısmi; [#626](https://github.com/dotnet/efcore/issues/626)) |
| Basit Günlük (Database.Log)                         | Evet      | 5.0 için birleştirildi ([#1199](https://github.com/dotnet/efcore/issues/1199)) |
| DbContext havuzlama                                     |          | 2,0                                   |

### <a name="database-providers-sup3sup"></a>Veritabanı sağlayıcıları <sup>(3)</sup>

| **Özellik**                                           | **EF6.4**| **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Evet      | 1.0                                   |
| MySQL                                                 | Evet      | 1.0                                   |
| PostgreSQL                                            | Evet      | 1.0                                   |
| Oracle                                                | Evet      | 1.0                                   |
| SQLite                                                | Evet      | 1.0                                   |
| SQL Server Compact                                    | Evet      | 1.0 <sup>(4)</sup>                    |
| DB2                                                   | Evet      | 1.0                                   |
| Firebird                                              | Evet      | 2,0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(4)</sup>                    |
| Azure Cosmos DB                                       |          | 3,0                                   |
| Bellek içi (sınama için)                               |          | 1.0                                   |

<sup>1</sup> Belirli bir sürüm için streç hedeflere ulaşılma olasılığı düşüktür. Ancak, işler yolunda giderse, o zaman onları çekmeye çalışacağız.

<sup>2</sup> BAZı EF6 özellikleri EF Core'da uygulanmaz. Bu özellikler, EF6'nın temel Varlık Veri Modeline (EDM) bağlıdır ve/veya yatırım getirisi nispeten düşük olan karmaşık özelliklerdir. Geri bildirimleri her zaman memnuniyetle karşılarız, ancak EF Core EF6'da mümkün olmayan birçok şeyi sağlarken, EF Core'un EF6'nın tüm özelliklerini desteklemesi tam tersi mümkün değildir.

<sup>Üçüncü</sup> taraflarca uygulanan 3 EF Core veritabanı sağlayıcısı, EF Core'un yeni ana sürümlerine güncellenmesinde gecikebilir. Daha fazla bilgi için [Veritabanı Sağlayıcıları'na](../core/providers/index.md) bakın.

<sup>4</sup> SQL Server Compact ve Jet sağlayıcıları yalnızca .NET Framework üzerinde çalışır (.NET Core'da değil).

### <a name="supported-platforms"></a>Desteklenen platformlar

EF Core 3.1 .NET Core ve .NET Framework üzerinde .NET Standard 2.0'ı kullanarak çalışır. Ancak, EF Core 5.0 .NET Framework üzerinde çalışmaz. Daha fazla bilgi için [Platformlar'a](../core/platforms/index.md) bakın.

EF6.4,.NET Core ve .NET Framework üzerinde çoklu hedefleme yoluyla çalışır.

## <a name="guidance-for-new-applications"></a>Yeni uygulamalar için kılavuz

Uygulama nın yalnızca .NET Framework'de desteklenen bir şeye ihtiyacı olmadığı sürece tüm yeni uygulamalar için .NET [Core'da](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server)EF Core'u kullanın.

## <a name="guidance-for-existing-ef6-applications"></a>Mevcut EF6 uygulamaları için kılavuz

EF Core, EF6'nın yerine düşme değildir. EF6'dan EF Core'a geçmek büyük olasılıkla uygulamanızda değişiklik yapılmasını gerektirecektir.

Bir EF6 uygulamasını .NET Core'a taşınırken:
* Veri erişim kodu kararlıysa ve gelişme olasılığı yoksa veya yeni özelliklere ihtiyaç distemiyorsa EF6 kullanmaya devam edin.
* Veri erişim kodu gelişiyorsa veya uygulamanın yalnızca EF Core'da kullanılabilen yeni özelliklere ihtiyacı varsa, EF Core'a bağlantı noktası.
* EF Core'a taşıma da genellikle performans için yapılır. Ancak, tüm senaryolar daha hızlı değildir, bu nedenle ilk bazı profil oluşturma yok.

Daha fazla bilgi [için EF6'dan EF Core'a](porting/index.md) taşıma ya da taşıma bilgisine bakın.
