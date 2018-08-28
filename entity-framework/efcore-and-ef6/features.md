---
title: EF Core ve EF6 özellik karşılaştırması
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: ed6ce51e7560e19e0d572f3d81cef7cbb310beec
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995341"
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>EF Core ve EF6 özellik karşılaştırması

Aşağıdaki tabloda EF6 ve EF Core ile sunulan özellikler karşılaştırılmaktadır. Bu, üst düzey bir karşılaştırma sağlar ve her özellik listelenemedi veya nasıl aynı works özelliği arasındaki olası farklar ayrıntıları vermek girişimi için tasarlanmıştır.

EF Core sütun, özelliği ilk kez göründü ürün sürümü numarasını içerir.

| **Model Oluşturma**                                  | **EF 6** | **EF Core**                           |
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
| Uzamsal veriler                                          | Evet      |                                       |
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
|                                                       |          |                                       |
| **Verileri Sorgulama**                                     | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Verileri Kaydetme**                                       | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Diğer özellikler**                                    | **EF6**  | **EF Core**                           |
| Geçişleri                                            | Evet      | 1.0                                   |
| Veritabanı oluşturma/silme API'leri                       | Evet      | 1.0                                   |
| Veri kaynağı                                             | Evet      | 2.1                                   |
| Bağlantı dayanıklılığı                                 | Evet      | 1.1                                   |
| Yaşam döngüsü kancaları (olayları, durdurma)                | Evet      |                                       |
| Basit günlüğe kaydetme (Database.Log)                         | Evet      |                                       |
| DbContext havuzu                                     |          | 2,0                                   |
|                                                       |          |                                       |
| **Veritabanı Sağlayıcıları**                                | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Platformlar**                                         | **EF6**  | **EF Core**                           |
| .NET framework (konsol, WinForms, WPF, ASP.NET)      | Evet      | 1.0                                   |
| .NET core (konsol, ASP.NET Core)                     |          | 1.0                                   |
| Mono ve Xamarin                                        |          | 1.0 (Sürüyor)                     |
| UWP                                                   |          | 1.0 (Sürüyor)                     |

<sup>1</sup> olduğundan şu anda bir Ücretli sağlayıcısı. Oracle için ücretsiz bir resmi sağlayıcısı üzerinde çalışılan.
<sup>2</sup> bu sağlayıcı yalnızca .NET Framework (değil, .NET Core) üzerinde çalışır.
