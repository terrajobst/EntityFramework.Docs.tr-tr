---
title: Entity Framework 6-EF6 ' e genel bakış
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 9561a7c4b645896cb4e248cb094c6954ed4bcdf1
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181424"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6), çok sayıda özelliği geliştirme ve sanallaştırmaya sahip .NET için bir, ve test edilmiş nesne ilişkisel Eşleyici (O/RM).

Bir O/RM olarak, EF6, ilişkisel ve nesne odaklı çalışma LDS arasındaki aksaklığın uyuşmazlığını azaltır, geliştiricilerin, bir yandan, türü kesin belirlenmiş .NET nesnelerini uygulamanın etki alanı ve genellikle yazması gereken veri erişimi "sıhhi tesisat" kodunun büyük bir bölümü için gereksinimi ortadan kaldırır.

EF6 birçok popüler O/RM özelliği uygular:
- Herhangi bir EF türüne bağımlı olmayan [poco](~/ef6/resources/glossary.md#poco) varlık sınıflarının eşlemesi
- Otomatik değişiklik izleme
- Kimlik çözümlemesi ve Iş birimi
- Eager, geç ve açık yükleme
- LINQ kullanılarak kesin belirlenmiş sorguların çevirisi (dil ile tümleşik sorgu)
- Aşağıdakiler için destek dahil zengin eşleme özellikleri:
  - Bire bir, bire çok ve çoktan çoğa ilişkiler
  - Devralma (her hiyerarşi için tablo, tür başına tablo ve somut sınıf başına tablo)
  - Karmaşık türler
  - Saklı yordamlar
- Varlık modelleri oluşturmak için görsel tasarımcı.
- Kod yazarak varlık modelleri oluşturmaya yönelik bir "Code First" deneyimi.
- Modeller, mevcut veritabanlarından oluşturulup ardından el ile düzenlenebilir ya da sıfırdan oluşturulup yeni veritabanları oluşturmak için kullanılabilir.
- WPF ve WinForms ile ASP.NET dahil olmak üzere .NET Framework uygulama modelleriyle ve Veri bağlamada tümleştirme.
- SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, vb. ' e bağlanmak için ADO.NET ve sayısız sağlayıcıya dayalı veritabanı bağlantısı.

## <a name="should-i-use-ef6-or-ef-core"></a>EF6 veya EF Core kullanmalıdır mi?

EF Core, EF6 'in çok benzer özelliklerine ve avantajlarına sahip Entity Framework daha modern, hafif ve genişletilebilir bir sürümüdür.
EF Core, tüm bir yeniden yazma ve EF6 içinde mevcut olmayan birçok yeni özelliği içerir, ancak yine de EF6 'nin en gelişmiş eşleme özelliklerinden bazılarını da içermemelidir.
Özellik kümesi gereksinimlerle eşleşiyorsa yeni uygulamalarda EF Core kullanmayı düşünün.
[Compare EF Core &AMP; EF6](xref:efcore-and-ef6/index) bu seçimi daha ayrıntılı bir şekilde inceler.

## <a name="get-startedef6get-startedmd"></a>[Başlarken](~/ef6/get-started.md)

Varlıklarınızın EntityFramework NuGet paketini ekleyin veya Visual Studio için Entity Framework Tools ' ü yüklemek. Daha sonra EF6 ' dan en iyi şekilde emin olmanıza yardımcı olması için videoları izleyin, öğreticileri okuyun ve gelişmiş belgeleri izleyin.

## <a name="past-entity-framework-versions"></a>Son Entity Framework sürümler

Bu, Entity Framework 6 ' nın en son sürümü için, aynı zamanda geçmiş yayınlar için de geçerli olan belgelerdir.
EF sürümlerinin ve tanıtıldıkları özelliklerin tamamı için yenilikler ve [Geçmiş yayınlar](~/ef6/what-is-new/past-releases.md) [hakkında bilgi edinin](~/ef6/what-is-new/index.md) .
