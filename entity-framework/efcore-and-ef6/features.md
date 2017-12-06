---
title: "EF çekirdek & EF6 özellik karşılaştırması"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: 696ff2c8ec788c08880ecb3b07e10dc081b0323b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>EF çekirdek ve EF6 özellik karşılaştırması

Aşağıdaki tabloda EF çekirdek ve EF6 kullanılabilen özellikleri karşılaştırılır. Üst düzey bir karşılaştırma verin ve her özellik listesi veya nasıl aynı works özellik arasındaki olası farklar ayrıntıları verin girişimi için tasarlanmıştır.

EF çekirdek sütun, özelliği ilk görünen ürün sürümü sayısını içerir.

| **Model oluşturma** |**EF 6** |**EF çekirdek** |
|-|-|-|
| Temel sınıf eşleme                         | Evet | 1.0 |
| Kurallar                                 | Evet | 1.0 |
| Özel kuralları                          | Evet | 1.0 (kısmi) |
| Veri açıklamaları                            | Evet | 1.0 |
| Fluent API'si                                  | Evet | 1.0 |
| Devralma: Tablo her hiyerarşi (TPH)      | Evet | 1.0 |
| Devralma: Tablo türü (birleştirilmiş TPT) başına           | Evet |     |
| Devralma: Tablo başına somut sınıfı (TPC) | Evet |     |
| Gölge durumu özellikleri                     |     | 1.0 |
| Alternatif anahtarları                              |     | 1.0 |
| Çok-çok birleştirme varlık olmadan            | Evet |     |
| Anahtar oluşturma: veritabanı                    | Evet | 1.0 |
| Anahtar oluşturma: istemci                      |     | 1.0 |
| Karmaşık ve ait türleri                         | Evet | 2.0 |
| Uzamsal veriler                                | Evet |     |
| Modelin grafik Görselleştirme            | Evet |     |
| Grafik model Düzenleyicisi                      | Evet |     |
| Model biçimi: kod                          | Evet | 1.0 |
| Model biçimi: EDMX (XML)                    | Evet |     |
| Modeli veritabanından oluşturun: komut satırı    | Evet | 1.0 |
| Modeli veritabanından oluşturun: VS Sihirbazı       | Evet |     |
| Modeli veritabanından güncelleştir                  | Kısmi | |
| Genel sorgu filtreleri                        |     | 2.0 |
| Tablo bölme                             | Evet | 2.0 |
| Varlık bölme                            | Evet |     |
| Veritabanı skaler işlev eşleme            | Kötü | 2.0 |
| Alan eşleme                               |     | 1.1 |
| | | |
| **Veri sorgulama** |**EF6** |**EF çekirdek** |
| LINQ sorguları                                | Evet | 1.0 (Sürüyor karmaşık sorgular için) |
| Okunabilir oluşturulan SQL                      | Kötü | 1.0 |
| Karma istemci/sunucu değerlendirme              |     | 1.0 |
| İlgili verileri yükleniyor: istekli                 | Evet | 1.0 |
| İlgili verileri yükleniyor: geç                  | Evet |     |
| İlgili verileri yükleniyor: açık              | Evet | 1.1 |
| Ham SQL sorguları: Model türleri                | Evet | 1.0 |
| Ham SQL sorguları: model olmayan türleri            | Evet |     |
| Ham SQL sorguları: LINQ ile oluşturma        |     | 1.0 |
| Açıkça derlenmiş sorguları                 | Kötü | 2.0 |
| | | |
| **Verileri kaydetme** |**EF6** |**EF çekirdek** |
| Değişiklik izleme: anlık görüntü                   | Evet | 1.0 |
| Değişiklik izleme: bildirim               | Evet | 1.0 |
| İzlenen durum erişme                     | Evet | 1.0 |
| İyimser eşzamanlılık                      | Evet | 1.0 |
| İşlemler                                | Evet | 1.0 |
| Deyimleri toplu işleme                      |     | 1.0 |
| Saklı yordam                            | Evet |     |
| Bağlantısı kesilmiş alt düzey API'leri grafiği           | Kötü | 1.0 |
| Bağlantısı kesilmiş grafik uçtan uca               |     | 1.0 (kısmi) |
| | | |
| **Diğer özellikler** |**EF6** |**EF çekirdek** |
| Geçişleri                                  | Evet | 1.0 |
| Veritabanı oluşturma/silme API'leri             | Evet | 1.0 |
| Çekirdek verileri                                   | Evet |     |
| Bağlantı dayanıklılığı                       | Evet | 1.1 |
| Yaşam döngüsü kancaları (olaylar, kişiler tarafından ele)      | Evet |     |
| DbContext havuzu                           |     | 2.0 |
| | | |
| **Veritabanı sağlayıcıları** |**EF6**|**EF çekirdek** |
| SQL Server                                  | Evet | 1.0 |
| MySQL                                       | Evet | 1.0 |
| PostgreSQL                                  | Evet | 1.0 |
| Oracle                                      | Evet | 1.0 (yalnızca ücretli<sup>(1)</sup>) |
| SQLite                                      | Evet | 1.0 |
| SQL Compact                                 | Evet | 1.0 <sup>(2)</sup> |
| DB2                                         | Evet |     |
| (Sınama) bellek içi                      |     | 1.0 |
| | | |
| **Platformlar** |**EF6** |**EF çekirdek** |
| .NET framework (konsol, WinForms, WPF, ASP.NET) | Evet | 1.0 |
| .NET core (konsolu, ASP.NET çekirdek)           |     | 1.0 |
| Mono & Xamarin                              |     | 1.0 (Sürüyor) |
| UWP                                         |     | 1.0 (Sürüyor) |

<sup>1</sup> Oracle için boş bir resmi sağlayıcısı üzerinde çalışılan.
<sup>2</sup> SQL Server Compact sağlayıcısı, yalnızca .NET Framework (.NET çekirdek değildir) üzerinde çalışır.
