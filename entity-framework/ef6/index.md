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
# <a name="entity-framework-6"></a><span data-ttu-id="960f2-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="960f2-102">Entity Framework 6</span></span>
<span data-ttu-id="960f2-103">Entity Framework 6 (EF6), uzun yıllar boyunca özellik geliştirme ve sabitleme özelliğine sahip .NET için denenmiş ve test edilmiş nesne ilişkisel mapper (O/RM) dir.</span><span class="sxs-lookup"><span data-stu-id="960f2-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="960f2-104">O/RM olarak EF6, ilişkisel ve nesne yönelimli dünyalar arasındaki empedans uyumsuzluğu azaltarak, geliştiricilerin uygulamanın etki alanını temsil eden güçlü bir şekilde yazılan .NET nesneleri kullanarak ilişkisel veritabanlarında depolanan verilerle etkileşime giren uygulamalar yazmalarını ve genellikle yazmaları gereken veri erişim "plumbing" kodunun büyük bir kısmına olan ihtiyacı ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="960f2-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="960f2-105">EF6 birçok popüler O/RM özelliğini uygular:</span><span class="sxs-lookup"><span data-stu-id="960f2-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="960f2-106">Herhangi bir EF türüne bağlı olmayan [POCO](xref:ef6/resources/glossary#poco) varlık sınıflarının haritalaması</span><span class="sxs-lookup"><span data-stu-id="960f2-106">Mapping of [POCO](xref:ef6/resources/glossary#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="960f2-107">Otomatik değişiklik izleme</span><span class="sxs-lookup"><span data-stu-id="960f2-107">Automatic change tracking</span></span>
- <span data-ttu-id="960f2-108">Kimlik çözümlemesi ve Çalışma Birimi</span><span class="sxs-lookup"><span data-stu-id="960f2-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="960f2-109">Istekli, tembel ve açık yükleme</span><span class="sxs-lookup"><span data-stu-id="960f2-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="960f2-110">[LINQ (Dil Geçersiz Sorgu)](https://aka.ms/AA6hsvu) kullanarak güçlü bir şekilde yazılan sorguların çevirisi</span><span class="sxs-lookup"><span data-stu-id="960f2-110">Translation of strongly-typed queries using [LINQ (Language INtegrated Query)](https://aka.ms/AA6hsvu)</span></span>
- <span data-ttu-id="960f2-111">Destek de dahil olmak üzere zengin haritalama yetenekleri:</span><span class="sxs-lookup"><span data-stu-id="960f2-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="960f2-112">Bire bir, bire çok ve çok-çok ilişkiler</span><span class="sxs-lookup"><span data-stu-id="960f2-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="960f2-113">Kalıtım (hiyerarşi başına tablo, tür başına tablo ve beton sınıf başına tablo)</span><span class="sxs-lookup"><span data-stu-id="960f2-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="960f2-114">Karmaşık türler</span><span class="sxs-lookup"><span data-stu-id="960f2-114">Complex types</span></span>
  - <span data-ttu-id="960f2-115">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="960f2-115">Stored procedures</span></span>
- <span data-ttu-id="960f2-116">Varlık modelleri oluşturmak için görsel bir tasarımcı.</span><span class="sxs-lookup"><span data-stu-id="960f2-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="960f2-117">Kod yazarak varlık modelleri oluşturmak için bir "Önce Kod" deneyimi.</span><span class="sxs-lookup"><span data-stu-id="960f2-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="960f2-118">Modeller varolan veritabanlarından oluşturulabilir ve sonra elden düzenlenebilir veya sıfırdan oluşturulabilir ve yeni veritabanları oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="960f2-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="960f2-119">ASP.NET dahil olmak üzere .NET Framework uygulama modelleri ile ve veri bağlama yoluyla WPF ve WinForms ile tümleştirme.</span><span class="sxs-lookup"><span data-stu-id="960f2-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="960f2-120">SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 vb. bağlanabilen ADO.NET ve çok sayıda [sağlayıcıya](xref:ef6/fundamentals/providers/index) dayalı veritabanı bağlantısı</span><span class="sxs-lookup"><span data-stu-id="960f2-120">Database connectivity based on ADO.NET and numerous [providers](xref:ef6/fundamentals/providers/index) available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="960f2-121">EF6 veya EF Core kullanmalı mıyım?</span><span class="sxs-lookup"><span data-stu-id="960f2-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="960f2-122">EF Core, Varlık Çerçevesi'nin EF6'ya çok benzer yetenekleri ve avantajları olan daha modern, hafif ve genişletilebilir bir versiyonudur.</span><span class="sxs-lookup"><span data-stu-id="960f2-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="960f2-123">EF Core tam bir yeniden yazma ve EF6 mevcut olmayan birçok yeni özellik içerir, aynı zamanda hala BAZı EF6 en gelişmiş haritalama yetenekleri yoksun olsa da.</span><span class="sxs-lookup"><span data-stu-id="960f2-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="960f2-124">Özellik kümesi gereksinimlerinize uygunsa, yeni uygulamalarda EF Core'u kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="960f2-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="960f2-125">[EF Core & EF6](xref:efcore-and-ef6/index) karşılaştırın bu seçimi daha ayrıntılı olarak inceler.</span><span class="sxs-lookup"><span data-stu-id="960f2-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-started"></a>[<span data-ttu-id="960f2-126">Başlayın</span><span class="sxs-lookup"><span data-stu-id="960f2-126">Get Started</span></span>](xref:ef6/get-started)

<span data-ttu-id="960f2-127">EntityFramework NuGet paketini projenize ekleyin veya [Visual Studio için Entity Framework Tools'u](https://aka.ms/AA6i8c5)yükleyin.</span><span class="sxs-lookup"><span data-stu-id="960f2-127">Add the EntityFramework NuGet package to your project or install the [Entity Framework Tools for Visual Studio](https://aka.ms/AA6i8c5).</span></span> <span data-ttu-id="960f2-128">Ardından videoları izleyin, eğitimleri okuyun ve EF6'dan en iyi şekilde yararlanmak için gelişmiş belgeler okuyun.</span><span class="sxs-lookup"><span data-stu-id="960f2-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="960f2-129">Geçmiş Varlık Çerçeve Sürümleri</span><span class="sxs-lookup"><span data-stu-id="960f2-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="960f2-130">Bu, Entity Framework 6'nın en son sürümü için yapılan belgelerdir, ancak çoğu geçmiş sürümler için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="960f2-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="960f2-131">EF sürümlerinin ve sundukları özelliklerin tam listesi için [Neler in New](xref:ef6/what-is-new/index) ve Past [Releases'e](xref:ef6/what-is-new/past-releases) göz atın.</span><span class="sxs-lookup"><span data-stu-id="960f2-131">Check out [What's New](xref:ef6/what-is-new/index) and [Past Releases](xref:ef6/what-is-new/past-releases) for a complete list of EF releases and the features they introduced.</span></span>
