---
title: Entity Framework 6-EF6 ' e genel bakış
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416334"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6), çok sayıda özelliği geliştirme ve sanallaştırmaya sahip .NET için bir, ve test edilmiş nesne ilişkisel Eşleyici (O/RM).

Bir O/RM olarak, EF6 ilişkisel ve nesne odaklı çalışma LDS arasındaki empedance uyuşmazlığını azaltır, geliştiricilerin uygulamanın etki alanını temsil eden, kesin türü belirtilmiş .NET nesnelerini kullanarak ilişkisel veritabanlarında etkileşime geçen uygulamaları yazmasına ve genellikle yazmaları gereken veri erişimi "sıhhi tesisat" kodunun büyük bir bölümü için ihtiyaç duymasını ortadan kaldırmaya olanak tanır.

EF6 birçok popüler O/RM özelliği uygular:
- Herhangi bir EF türüne bağımlı olmayan [poco](xref:ef6/resources/glossary#poco) varlık sınıflarının eşlemesi
- Otomatik değişiklik izleme
- Kimlik çözümlemesi ve Iş birimi
- Eager, geç ve açık yükleme
- LINQ kullanılarak kesin belirlenmiş sorguların çevirisi [(dil Ile tümleşik sorgu)](https://aka.ms/AA6hsvu)
- Aşağıdakiler için destek dahil zengin eşleme özellikleri:
  - Bire bir, bire çok ve çoktan çoğa ilişkiler
  - Devralma (her hiyerarşi için tablo, tür başına tablo ve somut sınıf başına tablo)
  - Karmaşık türler
  - Saklı yordamlar
- Varlık modelleri oluşturmak için görsel tasarımcı.
- Kod yazarak varlık modelleri oluşturmaya yönelik bir "Code First" deneyimi.
- Modeller, mevcut veritabanlarından oluşturulup ardından el ile düzenlenebilir ya da sıfırdan oluşturulup yeni veritabanları oluşturmak için kullanılabilir.
- WPF ve WinForms ile ASP.NET dahil olmak üzere .NET Framework uygulama modelleriyle ve Veri bağlamada tümleştirme.
- SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, vb. ' e bağlanmak için ADO.NET ve sayısız [sağlayıcıya](xref:ef6/fundamentals/providers/index) dayalı veritabanı bağlantısı.

## <a name="should-i-use-ef6-or-ef-core"></a>EF6 veya EF Core kullanmalıdır mi?

EF Core, EF6 'in çok benzer özelliklerine ve avantajlarına sahip Entity Framework daha modern, hafif ve genişletilebilir bir sürümüdür.
EF Core, tüm bir yeniden yazma ve EF6 içinde mevcut olmayan birçok yeni özelliği içerir, ancak yine de EF6 'nin en gelişmiş eşleme özelliklerinden bazılarını da içermemelidir.
Özellik kümesi gereksinimlerle eşleşiyorsa yeni uygulamalarda EF Core kullanmayı düşünün.
[Compare EF Core &AMP; EF6](xref:efcore-and-ef6/index) bu seçimi daha ayrıntılı bir şekilde inceler.

## <a name="get-started"></a>[Başlarken](xref:ef6/get-started)

Varlıklarınızın EntityFramework NuGet paketini ekleyin veya [Visual Studio için Entity Framework Tools](https://aka.ms/AA6i8c5)' ü yüklemek. Daha sonra EF6 ' dan en iyi şekilde emin olmanıza yardımcı olması için videoları izleyin, öğreticileri okuyun ve gelişmiş belgeleri izleyin.

## <a name="past-entity-framework-versions"></a>Son Entity Framework sürümler

Bu, Entity Framework 6 ' nın en son sürümü için, aynı zamanda geçmiş yayınlar için de geçerli olan belgelerdir.
EF sürümlerinin ve tanıtıldıkları özelliklerin tamamı için yenilikler ve [Geçmiş yayınlar](xref:ef6/what-is-new/past-releases) [hakkında bilgi edinin](xref:ef6/what-is-new/index) .
