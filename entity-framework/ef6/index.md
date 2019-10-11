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
# <a name="entity-framework-6"></a><span data-ttu-id="4d3e3-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="4d3e3-102">Entity Framework 6</span></span>
<span data-ttu-id="4d3e3-103">Entity Framework 6 (EF6), çok sayıda özelliği geliştirme ve sanallaştırmaya sahip .NET için bir, ve test edilmiş nesne ilişkisel Eşleyici (O/RM).</span><span class="sxs-lookup"><span data-stu-id="4d3e3-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="4d3e3-104">Bir O/RM olarak, EF6, ilişkisel ve nesne odaklı çalışma LDS arasındaki aksaklığın uyuşmazlığını azaltır, geliştiricilerin, bir yandan, türü kesin belirlenmiş .NET nesnelerini uygulamanın etki alanı ve genellikle yazması gereken veri erişimi "sıhhi tesisat" kodunun büyük bir bölümü için gereksinimi ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="4d3e3-105">EF6 birçok popüler O/RM özelliği uygular:</span><span class="sxs-lookup"><span data-stu-id="4d3e3-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="4d3e3-106">Herhangi bir EF türüne bağımlı olmayan [poco](~/ef6/resources/glossary.md#poco) varlık sınıflarının eşlemesi</span><span class="sxs-lookup"><span data-stu-id="4d3e3-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="4d3e3-107">Otomatik değişiklik izleme</span><span class="sxs-lookup"><span data-stu-id="4d3e3-107">Automatic change tracking</span></span>
- <span data-ttu-id="4d3e3-108">Kimlik çözümlemesi ve Iş birimi</span><span class="sxs-lookup"><span data-stu-id="4d3e3-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="4d3e3-109">Eager, geç ve açık yükleme</span><span class="sxs-lookup"><span data-stu-id="4d3e3-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="4d3e3-110">LINQ kullanılarak kesin belirlenmiş sorguların çevirisi (dil ile tümleşik sorgu)</span><span class="sxs-lookup"><span data-stu-id="4d3e3-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="4d3e3-111">Aşağıdakiler için destek dahil zengin eşleme özellikleri:</span><span class="sxs-lookup"><span data-stu-id="4d3e3-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="4d3e3-112">Bire bir, bire çok ve çoktan çoğa ilişkiler</span><span class="sxs-lookup"><span data-stu-id="4d3e3-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="4d3e3-113">Devralma (her hiyerarşi için tablo, tür başına tablo ve somut sınıf başına tablo)</span><span class="sxs-lookup"><span data-stu-id="4d3e3-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="4d3e3-114">Karmaşık türler</span><span class="sxs-lookup"><span data-stu-id="4d3e3-114">Complex types</span></span>
  - <span data-ttu-id="4d3e3-115">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="4d3e3-115">Stored procedures</span></span>
- <span data-ttu-id="4d3e3-116">Varlık modelleri oluşturmak için görsel tasarımcı.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="4d3e3-117">Kod yazarak varlık modelleri oluşturmaya yönelik bir "Code First" deneyimi.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="4d3e3-118">Modeller, mevcut veritabanlarından oluşturulup ardından el ile düzenlenebilir ya da sıfırdan oluşturulup yeni veritabanları oluşturmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="4d3e3-119">WPF ve WinForms ile ASP.NET dahil olmak üzere .NET Framework uygulama modelleriyle ve Veri bağlamada tümleştirme.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="4d3e3-120">SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, vb. ' e bağlanmak için ADO.NET ve sayısız sağlayıcıya dayalı veritabanı bağlantısı.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="4d3e3-121">EF6 veya EF Core kullanmalıdır mi?</span><span class="sxs-lookup"><span data-stu-id="4d3e3-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="4d3e3-122">EF Core, EF6 'in çok benzer özelliklerine ve avantajlarına sahip Entity Framework daha modern, hafif ve genişletilebilir bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="4d3e3-123">EF Core, tüm bir yeniden yazma ve EF6 içinde mevcut olmayan birçok yeni özelliği içerir, ancak yine de EF6 'nin en gelişmiş eşleme özelliklerinden bazılarını da içermemelidir.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="4d3e3-124">Özellik kümesi gereksinimlerle eşleşiyorsa yeni uygulamalarda EF Core kullanmayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="4d3e3-125">[Compare EF Core &AMP; EF6](xref:efcore-and-ef6/index) bu seçimi daha ayrıntılı bir şekilde inceler.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="4d3e3-126">Başlarken</span><span class="sxs-lookup"><span data-stu-id="4d3e3-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="4d3e3-127">Varlıklarınızın EntityFramework NuGet paketini ekleyin veya Visual Studio için Entity Framework Tools ' ü yüklemek.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="4d3e3-128">Daha sonra EF6 ' dan en iyi şekilde emin olmanıza yardımcı olması için videoları izleyin, öğreticileri okuyun ve gelişmiş belgeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="4d3e3-129">Son Entity Framework sürümler</span><span class="sxs-lookup"><span data-stu-id="4d3e3-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="4d3e3-130">Bu, Entity Framework 6 ' nın en son sürümü için, aynı zamanda geçmiş yayınlar için de geçerli olan belgelerdir.</span><span class="sxs-lookup"><span data-stu-id="4d3e3-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="4d3e3-131">EF sürümlerinin ve tanıtıldıkları özelliklerin tamamı için yenilikler ve [Geçmiş yayınlar](~/ef6/what-is-new/past-releases.md) [hakkında bilgi edinin](~/ef6/what-is-new/index.md) .</span><span class="sxs-lookup"><span data-stu-id="4d3e3-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
