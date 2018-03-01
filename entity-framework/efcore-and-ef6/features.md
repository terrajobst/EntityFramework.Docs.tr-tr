---
title: "EF çekirdek & EF6 özellik karşılaştırması"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: 3f05fbe53439826a4e1e1b188a7c03951dc109ec
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>EF çekirdek ve EF6 özellik karşılaştırması

Aşağıdaki tabloda EF çekirdek ve EF6 kullanılabilen özellikleri karşılaştırılır. Üst düzey bir karşılaştırma verin ve her özellik listesi veya nasıl aynı works özellik arasındaki olası farklar ayrıntıları verin girişimi için tasarlanmıştır.

EF çekirdek sütun, özelliği ilk görünen ürün sürümü sayısını içerir.

| **Model Oluşturma**                                  | **EF 6** | **EF çekirdek**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Temel sınıf eşleme                                   | Evet      | 1.0                                   |
| Parametreli oluşturucular                          |          | 2.1                                   |
| Özellik değeri dönüşümleri                            |          | 2.1                                   |
| (Sorgu türleri) anahtar ile eşlenen türleri               |          | 2.1                                   |
| Kurallar                                           | Evet      | 1.0                                   |
| Özel kuralları                                    | Evet      | 1.0 (kısmi)                         |
| Veri açıklamaları                                      | Evet      | 1.0                                   |
| Fluent API'si                                            | Evet      | 1.0                                   |
| Devralma: Tablo her hiyerarşi (TPH)                | Evet      | 1.0                                   |
| Devralma: Tablo türü (birleştirilmiş TPT) başına                     | Evet      |                                       |
| Devralma: Tablo başına somut sınıfı (TPC)           | Evet      |                                       |
| Gölge durumu özellikleri                               |          | 1.0                                   |
| Alternatif anahtarları                                        |          | 1.0                                   |
| Çok-çok birleştirme varlık olmadan                      | Evet      |                                       |
| Anahtar oluşturma: veritabanı                              | Evet      | 1.0                                   |
| Anahtar oluşturma: istemci                                |          | 1.0                                   |
| Karmaşık ve ait türleri                                   | Evet      | 2,0                                   |
| Uzamsal veriler                                          | Evet      |                                       |
| Modelin grafik Görselleştirme                      | Evet      |                                       |
| Grafik model Düzenleyicisi                                | Evet      |                                       |
| Model biçimi: kod                                    | Evet      | 1.0                                   |
| Model biçimi: EDMX (XML)                              | Evet      |                                       |
| Modeli veritabanından oluşturun: komut satırı              | Evet      | 1.0                                   |
| Modeli veritabanından oluşturun: VS Sihirbazı                 | Evet      |                                       |
| Modeli veritabanından güncelleştir                            | Kısmi  |                                       |
| Genel sorgu filtreleri                                  |          | 2,0                                   |
| Tablo bölme                                       | Evet      | 2,0                                   |
| Varlık bölme                                      | Evet      |                                       |
| Veritabanı skaler işlev eşleme                      | Kötü     | 2,0                                   |
| Alan eşleme                                         |          | 1.1                                   |
|                                                       |          |                                       |
| **Verileri Sorgulama**                                     | **EF6**  | **EF çekirdek**                           |
| LINQ sorguları                                          | Evet      | 1.0 (Sürüyor karmaşık sorgular için) |
| Okunabilir oluşturulan SQL                                | Kötü     | 1.0                                   |
| Karma istemci/sunucu değerlendirme                        |          | 1.0                                   |
| GroupBy çevirisi                                   | Evet      | 2.1                                   |
| İlgili verileri yükleniyor: istekli                           | Evet      | 1.0                                   |
| İlgili verileri yükleniyor: için yükleme Eager türetilmiş türler |          | 2.1                                   |
| İlgili verileri yükleniyor: geç                            | Evet      | 2.1                                   |
| İlgili verileri yükleniyor: açık                        | Evet      | 1.1                                   |
| Ham SQL sorguları: varlık türleri                         | Evet      | 1.0                                   |
| Ham SQL sorguları: varlık olmayan türleri (örneğin, sorgu türleri)  | Evet      | 2.1                                   |
| Ham SQL sorguları: LINQ ile oluşturma                  |          | 1.0                                   |
| Açıkça derlenmiş sorguları                           | Kötü     | 2,0                                   |
| Metin tabanlı sorgu dili (örn. varlık SQL)           | 1.0      |                                       |
|                                                       |          |                                       |
| **Verileri Kaydetme**                                       | **EF6**  | **EF çekirdek**                           |
| Değişiklik izleme: anlık görüntü                             | Evet      | 1.0                                   |
| Değişiklik izleme: bildirim                         | Evet      | 1.0                                   |
| Değişiklik izleme: proxy'leri                              | Evet      |                                       |
| İzlenen durum erişme                               | Evet      | 1.0                                   |
| İyimser eşzamanlılık                                | Evet      | 1.0                                   |
| İşlemler                                          | Evet      | 1.0                                   |
| Deyimleri toplu işleme                                |          | 1.0                                   |
| Saklı yordam eşleme                              | Evet      |                                       |
| Bağlantısı kesilmiş alt düzey API'leri grafiği                     | Kötü     | 1.0                                   |
| Bağlantısı kesilmiş grafik uçtan uca                         |          | 1.0 (kısmi)                         |
|                                                       |          |                                       |
| **Diğer özellikler**                                    | **EF6**  | **EF çekirdek**                           |
| Geçişleri                                            | Evet      | 1.0                                   |
| Veritabanı oluşturma/silme API'leri                       | Evet      | 1.0                                   |
| Çekirdek verileri                                             | Evet      | 2.1                                   |
| Bağlantı dayanıklılığı                                 | Evet      | 1.1                                   |
| Yaşam döngüsü kancaları (olaylar, kişiler tarafından ele)                | Evet      |                                       |
| Basit günlüğe kaydetme (örneğin Database.Log)                    | Evet      |                                       |
| DbContext havuzu                                     |          | 2,0                                   |
|                                                       |          |                                       |
| **Veritabanı Sağlayıcıları**                                | **EF6**  | **EF çekirdek**                           |
| SQL Server                                            | Evet      | 1.0                                   |
| MySQL                                                 | Evet      | 1.0                                   |
| PostgreSQL                                            | Evet      | 1.0                                   |
| Oracle                                                | Evet      | 1.0 <sup>(1)</sup>                    |
| SQLite                                                | Evet      | 1.0                                   |
| SQL Server Compact                                    | Evet      | 1.0 <sup>(2)</sup>                    |
| DB2                                                   | Evet      | 1.0                                   |
| Firebird                                              | Evet      | 2,0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(2)</sup>                    |
| (Sınama) bellek içi                               |          | 1.0                                   |
|                                                       |          |                                       |
| **Platforms**                                         | **EF6**  | **EF çekirdek**                           |
| .NET framework (konsol, WinForms, WPF, ASP.NET)      | Evet      | 1.0                                   |
| .NET core (konsolu, ASP.NET çekirdek)                     |          | 1.0                                   |
| Mono & Xamarin                                        |          | 1.0 (Sürüyor)                     |
| UWP                                                   |          | 1.0 (Sürüyor)                     |

<sup>1</sup> olduğundan şu anda bir Ücretli sağlayıcısı. Oracle için boş bir resmi sağlayıcısı üzerinde çalışılan.
<sup>2</sup> bu sağlayıcı yalnızca .NET Framework (değil, .NET Core) üzerinde çalışır.
