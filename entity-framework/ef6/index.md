---
title: Genel Bakış - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 5218f3f8149b15cb5ad3a14779dfa9c23eb71a0e
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152500"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) bir denenmiş ve test edilmiş nesne ilişkisel eşleyicidir (O/RM) .NET için yıllar boyunca özellik geliştirmeleri ve sabitleme ' dir.

Bir O/RM depolanan ilişkisel veritabanlarının temsil eden kesin .NET nesneleri kullanarak verilerle etkileşim kuran uygulamaları yazmak geliştiricilerin ilişkisel ve nesne yönelimli dünyaları arasında empedans uyuşmazlığı EF6 azaltır. uygulama etki alanı ve genellikle yazmak için ihtiyaç duydukları veri erişim "tesisatı" kodu büyük bir kısmı gereği ortadan kaldırılır.

EF6 birçok popüler O/RM özellikler uygular:
- Eşleme [POCO](~/ef6/resources/glossary.md#poco) herhangi EF türlerinde bağlı olmayan varlık sınıfları
- Otomatik değişiklik izleme
- Kimlik çözümlemesi ve iş birimi
- Eager, yavaş ve açık yükleme
- LINQ (dil ile tümleşik sorgu) kullanarak türü kesin belirlenmiş sorgular çevirisi
- Zengin Haritalama özellikleri desteği:
  - Bire bir, bire çok ve çoka çok ilişkileri
  - Devralma (tablo başına hiyerarşi, her tür tablosu ve tablo somut sınıf başına)
  - Karmaşık türler
  - Saklı yordamlar
- Varlık modelleri oluşturmak için bir görsel tasarımcı.
- Kod yazarak varlık modelleri oluşturmak için bir "Code First" karşılaşırsınız.
- Modellerini ya da mevcut veritabanlarından oluşturulan ve daha sonra elle düzenlenerek olabilir veya bunlar sıfırdan oluşturulabilir ve sonra yeni veritabanları oluşturmak için kullanılır.
- WPF ve WinForms ile veri bağlama aracılığıyla ASP.NET dahil olmak üzere, .NET Framework uygulama modelleri ile tümleştirme.
- ADO.NET ve SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, vb. için bağlanmak kullanılabilen çok sayıda sağlayıcılarını göre veritabanı bağlantısı.

## <a name="should-i-use-ef6-or-ef-core"></a>EF6 veya EF Core kullanmalıyım?

EF Core çok benzer işlevler ve avantajlar EF6 için'olan Entity Framework'ün daha modern, basit ve Genişletilebilir bir sürümüdür.
EF Core tamamını yeniden yazmak ve rağmen yine de bazı EF6'ın en gelişmiş eşleme özellikleri eksik EF6 içinde mevcut olmayan birçok yeni özellikler içerir.
Özellik kümesini gereksinimlerinize uyan sürece yeni uygulamalarda EF Core kullanmanızı öneririz.
[EF Core ve EF6 karşılaştırması](xref:efcore-and-ef6/index) bu seçenek daha ayrıntılı inceler.

## <a name="get-startedef6get-startedmd"></a>[Başlarken](~/ef6/get-started.md)

EntityFramework NuGet paketini projenize ekleyin veya Entity Framework Tools for Visual Studio yükleyin. Videolar, öğreticiler ve en iyi şekilde EF6 yapmanıza yardımcı olmak için Gelişmiş belgeleri okuyun, sonra izleyin.

## <a name="past-entity-framework-versions"></a>Geçmiş Entity Framework sürümleri

Çoğu son sürümleri için de geçerlidir ancak Entity Framework 6, en son sürümüne yönelik belgelere budur.
Kullanıma [yenilikler](~/ef6/what-is-new/index.md) ve [son sürümleri](~/ef6/what-is-new/past-releases.md) EF sürümleri ve bunlar sunulan özelliklerin tam listesi için.
