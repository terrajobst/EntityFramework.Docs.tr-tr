---
title: Entity Framework 6 Hızlı genel bakış
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
caps.latest.revision: 5
uid: ef6/index
ms.openlocfilehash: 7bb51ea82640ef29bb376c2320ea29a81eeb175e
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914347"
---
# <a name="entity-framework-6-quick-overview"></a><span data-ttu-id="9125d-102">Entity Framework 6 Hızlı genel bakış</span><span class="sxs-lookup"><span data-stu-id="9125d-102">Entity Framework 6 Quick Overview</span></span>
<span data-ttu-id="9125d-103">Entity Framework 6 (EF6) bir denenmiş ve test edilmiş nesne ilişkisel eşleyicidir (O/RM) .NET için yıllar boyunca özellik geliştirmeleri ve sabitleme ' dir.</span><span class="sxs-lookup"><span data-stu-id="9125d-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="9125d-104">Bir O/RM depolanan ilişkisel veritabanlarının temsil eden kesin .NET nesneleri kullanarak verilerle etkileşim kuran uygulamaları yazmak geliştiricilerin ilişkisel ve nesne yönelimli dünyaları arasında empedans uyuşmazlığı EF6 azaltır. uygulama etki alanı ve genellikle yazmak için ihtiyaç duydukları veri erişim "tesisatı" kodu büyük bir kısmı gereği ortadan kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="9125d-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="9125d-105">EF6 birçok popüler O/RM özellikler uygular:</span><span class="sxs-lookup"><span data-stu-id="9125d-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="9125d-106">Eşleme [POCO](~/ef6/resources/glossary.md#poco) herhangi EF türlerinde bağlı olmayan varlık sınıfları</span><span class="sxs-lookup"><span data-stu-id="9125d-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="9125d-107">Otomatik değişiklik izleme</span><span class="sxs-lookup"><span data-stu-id="9125d-107">Automatic change tracking</span></span>
- <span data-ttu-id="9125d-108">Kimlik çözümlemesi ve iş birimi</span><span class="sxs-lookup"><span data-stu-id="9125d-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="9125d-109">Eager, yavaş ve açık yükleme</span><span class="sxs-lookup"><span data-stu-id="9125d-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="9125d-110">LINQ (dil ile tümleşik sorgu) kullanarak türü kesin belirlenmiş sorgular çevirisi</span><span class="sxs-lookup"><span data-stu-id="9125d-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="9125d-111">Zengin Haritalama özellikleri desteği:</span><span class="sxs-lookup"><span data-stu-id="9125d-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="9125d-112">Bire bir, bire çok ve çoka çok ilişkileri</span><span class="sxs-lookup"><span data-stu-id="9125d-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="9125d-113">Devralma (tablo başına hiyerarşi, her tür tablosu ve tablo somut sınıf başına)</span><span class="sxs-lookup"><span data-stu-id="9125d-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="9125d-114">Karmaşık türler</span><span class="sxs-lookup"><span data-stu-id="9125d-114">Complex types</span></span>
  - <span data-ttu-id="9125d-115">Saklı yordamlar</span><span class="sxs-lookup"><span data-stu-id="9125d-115">Stored procedures</span></span>
- <span data-ttu-id="9125d-116">Varlık modelleri oluşturmak için bir görsel tasarımcı.</span><span class="sxs-lookup"><span data-stu-id="9125d-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="9125d-117">Kod yazarak varlık modelleri oluşturmak için bir "Code First" karşılaşırsınız.</span><span class="sxs-lookup"><span data-stu-id="9125d-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="9125d-118">Modelleri oluşturulan form veritabanlarında olabilir ve daha sonra elle düzenlenerek, ya da bunlar sıfırdan oluşturulabilir ve sonra yeni veritabanları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9125d-118">Models can either be generated form existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="9125d-119">WPF ve WinForms ile veri bağlama aracılığıyla ASP.NET dahil olmak üzere, .NET Framework uygulama modelleri ile tümleştirme.</span><span class="sxs-lookup"><span data-stu-id="9125d-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="9125d-120">ADO.NET ve SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, vb. için bağlanmak kullanılabilen çok sayıda sağlayıcılarını göre veritabanı bağlantısı.</span><span class="sxs-lookup"><span data-stu-id="9125d-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="9125d-121">EF6 veya EF Core kullanmalıyım?</span><span class="sxs-lookup"><span data-stu-id="9125d-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="9125d-122">EF Core çok benzer işlevler ve avantajlar EF6 için'olan Entity Framework'ün daha modern, basit ve Genişletilebilir bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="9125d-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="9125d-123">EF Core tamamını yeniden yazmak ve rağmen yine de bazı EF6'ın en gelişmiş eşleme özellikleri eksik EF6 içinde mevcut olmayan birçok yeni özellikler içerir.</span><span class="sxs-lookup"><span data-stu-id="9125d-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="9125d-124">Özellik kümesini gereksinimlerinize uyan sürece yeni uygulamalarda EF Core kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="9125d-124">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="9125d-125">[EF Core ve EF6 karşılaştırması](xref:efcore-and-ef6/index) bu seçenek daha ayrıntılı inceler.</span><span class="sxs-lookup"><span data-stu-id="9125d-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="9125d-126">Başlarken</span><span class="sxs-lookup"><span data-stu-id="9125d-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="9125d-127">EntityFramework NuGet paketini projenize ekleyin veya Entity Framework Tools for Visual Studio yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9125d-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="9125d-128">Videolar, öğreticiler ve en iyi şekilde EF6 yapmanıza yardımcı olmak için Gelişmiş belgeleri okuyun, sonra izleyin.</span><span class="sxs-lookup"><span data-stu-id="9125d-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="9125d-129">Geçmiş Entity Framework sürümleri</span><span class="sxs-lookup"><span data-stu-id="9125d-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="9125d-130">Çoğu son sürümleri için de geçerlidir ancak Entity Framework 6, en son sürümüne yönelik belgelere budur.</span><span class="sxs-lookup"><span data-stu-id="9125d-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="9125d-131">Kullanıma [yenilikler](~/ef6/what-is-new/index.md) ve [son sürümleri](~/ef6/what-is-new/past-releases.md) EF sürümleri ve bunlar sunulan özelliklerin tam listesi için.</span><span class="sxs-lookup"><span data-stu-id="9125d-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
