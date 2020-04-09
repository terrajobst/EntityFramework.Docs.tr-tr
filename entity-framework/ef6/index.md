---
title: Varlık Çerçevesine Genel Bakış 6 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416334"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6), uzun yıllar boyunca özellik geliştirme ve sabitleme özelliğine sahip .NET için denenmiş ve test edilmiş nesne ilişkisel mapper (O/RM) dir.

O/RM olarak EF6, ilişkisel ve nesne yönelimli dünyalar arasındaki empedans uyumsuzluğu azaltarak, geliştiricilerin uygulamanın etki alanını temsil eden güçlü bir şekilde yazılan .NET nesneleri kullanarak ilişkisel veritabanlarında depolanan verilerle etkileşime giren uygulamalar yazmalarını ve genellikle yazmaları gereken veri erişim "plumbing" kodunun büyük bir kısmına olan ihtiyacı ortadan kaldırır.

EF6 birçok popüler O/RM özelliğini uygular:
- Herhangi bir EF türüne bağlı olmayan [POCO](xref:ef6/resources/glossary#poco) varlık sınıflarının haritalaması
- Otomatik değişiklik izleme
- Kimlik çözümlemesi ve Çalışma Birimi
- Istekli, tembel ve açık yükleme
- [LINQ (Dil Geçersiz Sorgu)](https://aka.ms/AA6hsvu) kullanarak güçlü bir şekilde yazılan sorguların çevirisi
- Destek de dahil olmak üzere zengin haritalama yetenekleri:
  - Bire bir, bire çok ve çok-çok ilişkiler
  - Kalıtım (hiyerarşi başına tablo, tür başına tablo ve beton sınıf başına tablo)
  - Karmaşık türler
  - Saklı yordamlar
- Varlık modelleri oluşturmak için görsel bir tasarımcı.
- Kod yazarak varlık modelleri oluşturmak için bir "Önce Kod" deneyimi.
- Modeller varolan veritabanlarından oluşturulabilir ve sonra elden düzenlenebilir veya sıfırdan oluşturulabilir ve yeni veritabanları oluşturmak için kullanılabilir.
- ASP.NET dahil olmak üzere .NET Framework uygulama modelleri ile ve veri bağlama yoluyla WPF ve WinForms ile tümleştirme.
- SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 vb. bağlanabilen ADO.NET ve çok sayıda [sağlayıcıya](xref:ef6/fundamentals/providers/index) dayalı veritabanı bağlantısı

## <a name="should-i-use-ef6-or-ef-core"></a>EF6 veya EF Core kullanmalı mıyım?

EF Core, Varlık Çerçevesi'nin EF6'ya çok benzer yetenekleri ve avantajları olan daha modern, hafif ve genişletilebilir bir versiyonudur.
EF Core tam bir yeniden yazma ve EF6 mevcut olmayan birçok yeni özellik içerir, aynı zamanda hala BAZı EF6 en gelişmiş haritalama yetenekleri yoksun olsa da.
Özellik kümesi gereksinimlerinize uygunsa, yeni uygulamalarda EF Core'u kullanmayı düşünün.
[EF Core & EF6](xref:efcore-and-ef6/index) karşılaştırın bu seçimi daha ayrıntılı olarak inceler.

## <a name="get-started"></a>[Başlayın](xref:ef6/get-started)

EntityFramework NuGet paketini projenize ekleyin veya [Visual Studio için Entity Framework Tools'u](https://aka.ms/AA6i8c5)yükleyin. Ardından videoları izleyin, eğitimleri okuyun ve EF6'dan en iyi şekilde yararlanmak için gelişmiş belgeler okuyun.

## <a name="past-entity-framework-versions"></a>Geçmiş Varlık Çerçeve Sürümleri

Bu, Entity Framework 6'nın en son sürümü için yapılan belgelerdir, ancak çoğu geçmiş sürümler için de geçerlidir.
EF sürümlerinin ve sundukları özelliklerin tam listesi için [Neler in New](xref:ef6/what-is-new/index) ve Past [Releases'e](xref:ef6/what-is-new/past-releases) göz atın.
