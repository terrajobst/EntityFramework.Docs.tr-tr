---
title: Entity Framework 6-EF6 ' e genel bakış
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656176"
---
# <a name="entity-framework-6"></a><span data-ttu-id="b3bb2-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b3bb2-102">Entity Framework 6</span></span>
<span data-ttu-id="b3bb2-103">Entity Framework 6 (EF6), çok sayıda özelliği geliştirme ve sanallaştırmaya sahip .NET için bir, ve test edilmiş nesne ilişkisel Eşleyici (O/RM).</span><span class="sxs-lookup"><span data-stu-id="b3bb2-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="b3bb2-104">Bir O/RM olarak, EF6, ilişkisel ve nesne odaklı çalışma LDS arasındaki aksaklığın uyuşmazlığını azaltır, geliştiricilerin, bir yandan, türü kesin belirlenmiş .NET nesnelerini uygulamanın etki alanı ve genellikle yazması gereken veri erişimi "sıhhi tesisat" kodunun büyük bir bölümü için gereksinimi ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="b3bb2-105">EF6 birçok popüler O/RM özelliği uygular:</span><span class="sxs-lookup"><span data-stu-id="b3bb2-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="b3bb2-106">Herhangi bir EF türüne bağımlı olmayan [poco](xref:ef6/resources/glossary#poco) varlık sınıflarının eşlemesi</span><span class="sxs-lookup"><span data-stu-id="b3bb2-106">Mapping of [POCO](xref:ef6/resources/glossary#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="b3bb2-107">Otomatik değişiklik izleme</span><span class="sxs-lookup"><span data-stu-id="b3bb2-107">Automatic change tracking</span></span>
- <span data-ttu-id="b3bb2-108">Kimlik çözümlemesi ve Iş birimi</span><span class="sxs-lookup"><span data-stu-id="b3bb2-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="b3bb2-109">Eager, geç ve açık yükleme</span><span class="sxs-lookup"><span data-stu-id="b3bb2-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="b3bb2-110">LINQ kullanılarak kesin belirlenmiş sorguların çevirisi [(dil Ile tümleşik sorgu)](https://aka.ms/AA6hsvu)</span><span class="sxs-lookup"><span data-stu-id="b3bb2-110">Translation of strongly-typed queries using [LINQ (Language INtegrated Query)](https://aka.ms/AA6hsvu)</span></span>
- <span data-ttu-id="b3bb2-111">Aşağıdakiler için destek dahil zengin eşleme özellikleri:</span><span class="sxs-lookup"><span data-stu-id="b3bb2-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="b3bb2-112">Bire bir, bire çok ve çoktan çoğa ilişkiler</span><span class="sxs-lookup"><span data-stu-id="b3bb2-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="b3bb2-113">Devralma (her hiyerarşi için tablo, tür başına tablo ve somut sınıf başına tablo)</span><span class="sxs-lookup"><span data-stu-id="b3bb2-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="b3bb2-114">Karmaşık türler</span><span class="sxs-lookup"><span data-stu-id="b3bb2-114">Complex types</span></span>
  - <span data-ttu-id="b3bb2-115">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="b3bb2-115">Stored procedures</span></span>
- <span data-ttu-id="b3bb2-116">Varlık modelleri oluşturmak için görsel tasarımcı.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="b3bb2-117">Kod yazarak varlık modelleri oluşturmaya yönelik bir "Code First" deneyimi.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="b3bb2-118">Modeller, mevcut veritabanlarından oluşturulup ardından el ile düzenlenebilir ya da sıfırdan oluşturulup yeni veritabanları oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="b3bb2-119">WPF ve WinForms ile ASP.NET dahil olmak üzere .NET Framework uygulama modelleriyle ve Veri bağlamada tümleştirme.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="b3bb2-120">SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, vb. ' e bağlanmak için ADO.NET ve sayısız [sağlayıcıya](xref:ef6/fundamentals/providers/index) dayalı veritabanı bağlantısı.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-120">Database connectivity based on ADO.NET and numerous [providers](xref:ef6/fundamentals/providers/index) available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="b3bb2-121">EF6 veya EF Core kullanmalıdır mi?</span><span class="sxs-lookup"><span data-stu-id="b3bb2-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="b3bb2-122">EF Core, EF6 'in çok benzer özelliklerine ve avantajlarına sahip Entity Framework daha modern, hafif ve genişletilebilir bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="b3bb2-123">EF Core, tüm bir yeniden yazma ve EF6 içinde mevcut olmayan birçok yeni özelliği içerir, ancak yine de EF6 'nin en gelişmiş eşleme özelliklerinden bazılarını da içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="b3bb2-124">Özellik kümesi gereksinimlerle eşleşiyorsa yeni uygulamalarda EF Core kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="b3bb2-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) bu seçimi daha ayrıntılı bir şekilde inceler.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedxrefef6get-started"></a>[<span data-ttu-id="b3bb2-126">Başlarken</span><span class="sxs-lookup"><span data-stu-id="b3bb2-126">Get Started</span></span>](xref:ef6/get-started)

<span data-ttu-id="b3bb2-127">Varlıklarınızın EntityFramework NuGet paketini ekleyin veya [Visual Studio için Entity Framework Tools](https://aka.ms/AA6i8c5)' ü yüklemek.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-127">Add the EntityFramework NuGet package to your project or install the [Entity Framework Tools for Visual Studio](https://aka.ms/AA6i8c5).</span></span> <span data-ttu-id="b3bb2-128">Daha sonra EF6 ' dan en iyi şekilde emin olmanıza yardımcı olması için videoları izleyin, öğreticileri okuyun ve gelişmiş belgeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="b3bb2-129">Son Entity Framework sürümler</span><span class="sxs-lookup"><span data-stu-id="b3bb2-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="b3bb2-130">Bu, Entity Framework 6 ' nın en son sürümü için, aynı zamanda geçmiş yayınlar için de geçerli olan belgelerdir.</span><span class="sxs-lookup"><span data-stu-id="b3bb2-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="b3bb2-131">EF sürümlerinin ve tanıtıldıkları özelliklerin tamamı için yenilikler ve [Geçmiş yayınlar](xref:ef6/what-is-new/past-releases) [hakkında bilgi edinin](xref:ef6/what-is-new/index) .</span><span class="sxs-lookup"><span data-stu-id="b3bb2-131">Check out [What's New](xref:ef6/what-is-new/index) and [Past Releases](xref:ef6/what-is-new/past-releases) for a complete list of EF releases and the features they introduced.</span></span>
