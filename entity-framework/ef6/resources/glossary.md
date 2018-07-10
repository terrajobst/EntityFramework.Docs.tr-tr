---
title: Entity Framework Sözlüğü - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
caps.latest.revision: 3
ms.openlocfilehash: 8a06cfb2dbe79364c3f5cc21ecb32fd60d239e8a
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914108"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="18400-102">Entity Framework Sözlüğü</span><span class="sxs-lookup"><span data-stu-id="18400-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="18400-103">İlk kod</span><span class="sxs-lookup"><span data-stu-id="18400-103">Code First</span></span>
<span data-ttu-id="18400-104">Kod kullanarak bir Entity Framework modelini oluşturma.</span><span class="sxs-lookup"><span data-stu-id="18400-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="18400-105">Model hedefleyebilir ve var olan bir veritabanını veya yeni bir veritabanı.</span><span class="sxs-lookup"><span data-stu-id="18400-105">The model can target and existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="18400-106">Bağlam</span><span class="sxs-lookup"><span data-stu-id="18400-106">Context</span></span>
<span data-ttu-id="18400-107">Sorgu ve veri kaydetmenize olanak sağlayan, veritabanı ile bir oturumu temsil eden sınıf.</span><span class="sxs-lookup"><span data-stu-id="18400-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="18400-108">Bir bağlam DbContext veya ObjectContext sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="18400-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="18400-109">Kuralı (ilk kodu)</span><span class="sxs-lookup"><span data-stu-id="18400-109">Convention (Code First)</span></span>
<span data-ttu-id="18400-110">Sınıflarınızı modelden şeklini çıkarsamak için Entity Framework kullanan bir kural.</span><span class="sxs-lookup"><span data-stu-id="18400-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="18400-111">İlk veritabanı</span><span class="sxs-lookup"><span data-stu-id="18400-111">Database First</span></span>
<span data-ttu-id="18400-112">EF Designer kullanarak bir Entity Framework modeli oluşturma, varolan bir veritabanını hedefler.</span><span class="sxs-lookup"><span data-stu-id="18400-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="18400-113">İstekli yükleme</span><span class="sxs-lookup"><span data-stu-id="18400-113">Eager loading</span></span>
<span data-ttu-id="18400-114">Burada bir varlık türü için sorgu ayrıca ilgili varlıkları sorgunun bir parçası yükler ilgili veri yükleme deseni.</span><span class="sxs-lookup"><span data-stu-id="18400-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="18400-115">EF Designer</span><span class="sxs-lookup"><span data-stu-id="18400-115">EF Designer</span></span>
<span data-ttu-id="18400-116">Bir görsel tasarımcı Visual Studio'da, kutuları ve satırları kullanarak bir Entity Framework modelini oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="18400-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="18400-117">Varlık</span><span class="sxs-lookup"><span data-stu-id="18400-117">Entity</span></span>
<span data-ttu-id="18400-118">Bir sınıf veya müşteriler, ürünler ve siparişler gibi uygulama verilerini temsil eden nesne.</span><span class="sxs-lookup"><span data-stu-id="18400-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="18400-119">Varlık Veri Modeli</span><span class="sxs-lookup"><span data-stu-id="18400-119">Entity Data Model</span></span>
<span data-ttu-id="18400-120">Varlıklar ve aralarındaki ilişkiler açıklanmıştır modeli.</span><span class="sxs-lookup"><span data-stu-id="18400-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="18400-121">EF kullanan EDM kavramsal modelinde açıklamak için geliştirici programları.</span><span class="sxs-lookup"><span data-stu-id="18400-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="18400-122">EDM Dr tarafından sunulan bir varlık ilişkisi modeli oluşturur. Peter Chen.</span><span class="sxs-lookup"><span data-stu-id="18400-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="18400-123">EDM başlangıçta Microsoft gelen Geliştirici ve sunucu teknolojileri paketi arasında ortak veri modeli olma birincil amacı ile geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="18400-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="18400-124">EDM de OData protokolünün bir parçası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="18400-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="18400-125">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="18400-125">Explicit loading</span></span>
<span data-ttu-id="18400-126">Bir API çağrısı yaparak ilgili nesneler burada yüklenen ilgili veri yükleme deseni.</span><span class="sxs-lookup"><span data-stu-id="18400-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="18400-127">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="18400-127">Fluent API</span></span>
<span data-ttu-id="18400-128">Code First modelini yapılandırmak için kullanılan bir API.</span><span class="sxs-lookup"><span data-stu-id="18400-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="18400-129">Yabancı anahtar ilişkilendirmesi</span><span class="sxs-lookup"><span data-stu-id="18400-129">Foreign key association</span></span>
<span data-ttu-id="18400-130">Yabancı anahtar temsil eden bir özellik (yani ürünün CategoryID özelliği içerir) bağımlı varlık sınıfında burada dahil varlıklar arasında ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="18400-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity (i.e. the Product contains a CategoryId property).</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="18400-131">İlişki tanımlama</span><span class="sxs-lookup"><span data-stu-id="18400-131">Identifying relationship</span></span>
<span data-ttu-id="18400-132">Asıl varlığın birincil anahtarı bağımlı varlığın birincil anahtarının bir parçası olduğu bir ilişki.</span><span class="sxs-lookup"><span data-stu-id="18400-132">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="18400-133">Bu tür ilişkiyi bağımlı varlık asıl varlık var olamaz.</span><span class="sxs-lookup"><span data-stu-id="18400-133">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="18400-134">Bağımsız ilişkilendirme</span><span class="sxs-lookup"><span data-stu-id="18400-134">Independent association</span></span>
<span data-ttu-id="18400-135">Varlıklar arasında ilişkilendirme burada yabancı anahtar (yani bir ürün sınıfı kategori ancak hiç CategoryID özelliği arasında bir ilişki içeriyor) bağımlı varlık sınıfına temsil eden bir özellik yok.</span><span class="sxs-lookup"><span data-stu-id="18400-135">An association between entities where there is no property representing the foreign key in the class of the dependent entity (i.e. a Product class contains a relationship to Category but no CategoryId property).</span></span> <span data-ttu-id="18400-136">Entity Framework, bağımsız bir nesne, bu ilişkiyi izlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="18400-136">Entity Framework will use an independent object to track this relationship.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="18400-137">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="18400-137">Lazy loading</span></span>
<span data-ttu-id="18400-138">Gezinti özelliğine erişinceye burada ilgili nesneleri otomatik olarak yüklenir, ilgili veri yükleme deseni.</span><span class="sxs-lookup"><span data-stu-id="18400-138">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="18400-139">İlk model</span><span class="sxs-lookup"><span data-stu-id="18400-139">Model First</span></span>
<span data-ttu-id="18400-140">EF Designer kullanarak bir Entity Framework modeli oluşturma, ardından yeni bir veritabanı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="18400-140">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="18400-141">Gezinme özelliği</span><span class="sxs-lookup"><span data-stu-id="18400-141">Navigation property</span></span>
<span data-ttu-id="18400-142">Başka bir varlığa başvuran bir varlığın bir özelliği (yani ürün kategorisi gezinti özelliği içerir ve kategorisi ürünleri gezinme özelliğini içerir).</span><span class="sxs-lookup"><span data-stu-id="18400-142">A property of an entity that references another entity (i.e. Product contains a Category navigation property and Category contains a Products navigation property).</span></span>

## <a name="poco"></a><span data-ttu-id="18400-143">POCO</span><span class="sxs-lookup"><span data-stu-id="18400-143">POCO</span></span>
<span data-ttu-id="18400-144">Düz eski CLR nesnesi kısaltması.</span><span class="sxs-lookup"><span data-stu-id="18400-144">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="18400-145">Basit kullanıcı sınıf herhangi bir çerçeveyi ile bağımlılık yok.</span><span class="sxs-lookup"><span data-stu-id="18400-145">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="18400-146">EF, bağlamında bir EntityObject türemiyor bir varlık sınıfı her arabirimlerini uygular veya EF tanımlı herhangi bir öznitelik taşır.</span><span class="sxs-lookup"><span data-stu-id="18400-146">In the context of EF, a an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="18400-147">Kalıcılık çerçevesinden bağımsız çalışabildiğinden böyle bir varlık sınıfları ayrıca "Kalıcılık ignorant" olduğu söylenir.</span><span class="sxs-lookup"><span data-stu-id="18400-147">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="18400-148">İlişki ters</span><span class="sxs-lookup"><span data-stu-id="18400-148">Relationship inverse</span></span>
<span data-ttu-id="18400-149">Örneğin, ürün bir ilişki karşı sonu. Kategori ve kategorisi. Ürün.</span><span class="sxs-lookup"><span data-stu-id="18400-149">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="18400-150">Kendi kendine varlık izleme</span><span class="sxs-lookup"><span data-stu-id="18400-150">Self-tracking entity</span></span>
<span data-ttu-id="18400-151">N katmanlı geliştirmeye yardımcı olan kod oluşturma şablondan oluşturulmuş bir varlık.</span><span class="sxs-lookup"><span data-stu-id="18400-151">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="18400-152">Tablo başına somut türünü (TPC)</span><span class="sxs-lookup"><span data-stu-id="18400-152">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="18400-153">Veritabanındaki tabloda ayırmak için bir yöntem devralma hiyerarşisinde girildiği soyut olmayan her eşleme eşlenir.</span><span class="sxs-lookup"><span data-stu-id="18400-153">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="18400-154">Tablo-başına-hiyerarşi (TPH)</span><span class="sxs-lookup"><span data-stu-id="18400-154">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="18400-155">Hiyerarşideki tüm türleri aynı veritabanındaki tabloda burada eşlendiğine devralma eşleme yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="18400-155">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="18400-156">Sütunları her satırda ne tür tanımlamak için kullanılan bir ayrıştırıcı ilişkilidir.</span><span class="sxs-lookup"><span data-stu-id="18400-156">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="18400-157">Tablo başına tür (TPT)</span><span class="sxs-lookup"><span data-stu-id="18400-157">Table-per-type (TPT)</span></span>
<span data-ttu-id="18400-158">Burada hiyerarşideki tüm türlerin genel özelliklerini veritabanında aynı tabloya eşlenir, ancak her türü için benzersiz özellikler için ayrı bir tabloda eşlenen devralma eşleme yöntemidir.</span><span class="sxs-lookup"><span data-stu-id="18400-158">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="18400-159">Bulma türü</span><span class="sxs-lookup"><span data-stu-id="18400-159">Type discovery</span></span>
<span data-ttu-id="18400-160">Entity Framework modelinin bir parçası olması gereken türleri tanımlama işlemi.</span><span class="sxs-lookup"><span data-stu-id="18400-160">The process of identifying the types that should be part of an Entity Framework model.</span></span>
