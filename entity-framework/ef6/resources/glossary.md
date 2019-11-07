---
title: Entity Framework sözlüğü-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: df0da4a68b3d2c882d9673417ee5fe335eccae2b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656152"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="fcb0e-102">Entity Framework sözlüğü</span><span class="sxs-lookup"><span data-stu-id="fcb0e-102">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="fcb0e-103">Code First</span><span class="sxs-lookup"><span data-stu-id="fcb0e-103">Code First</span></span>
<span data-ttu-id="fcb0e-104">Kodu kullanarak Entity Framework modeli oluşturma.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-104">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="fcb0e-105">Model, var olan bir veritabanını veya yeni bir veritabanını hedefleyebilir.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-105">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="fcb0e-106">Bağlam</span><span class="sxs-lookup"><span data-stu-id="fcb0e-106">Context</span></span>
<span data-ttu-id="fcb0e-107">Veritabanı ile bir oturumu temsil eden ve verileri sorgulamanızı ve kaydetmenizi sağlayan bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-107">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="fcb0e-108">Bağlam DbContext veya ObjectContext sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-108">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="fcb0e-109">Kural (Code First)</span><span class="sxs-lookup"><span data-stu-id="fcb0e-109">Convention (Code First)</span></span>
<span data-ttu-id="fcb0e-110">Sınıflarınızda modelinizin şeklini çıkarsmak için Entity Framework tarafından kullanılan bir kural.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-110">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="fcb0e-111">Database First</span><span class="sxs-lookup"><span data-stu-id="fcb0e-111">Database First</span></span>
<span data-ttu-id="fcb0e-112">Var olan bir veritabanını hedefleyen EF tasarımcısını kullanarak Entity Framework modeli oluşturma.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-112">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="fcb0e-113">Ekip yükleme</span><span class="sxs-lookup"><span data-stu-id="fcb0e-113">Eager loading</span></span>
<span data-ttu-id="fcb0e-114">Bir varlık türü için bir sorgunun aynı zamanda ilgili varlıkları sorgunun bir parçası olarak yüklediği ilgili verileri yükleme kalıbı.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-114">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="fcb0e-115">EF Tasarımcısı</span><span class="sxs-lookup"><span data-stu-id="fcb0e-115">EF Designer</span></span>
<span data-ttu-id="fcb0e-116">Kutuları ve çizgileri kullanarak bir Entity Framework modeli oluşturmanıza olanak tanıyan Visual Studio 'da görsel tasarımcı.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-116">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="fcb0e-117">Varlık</span><span class="sxs-lookup"><span data-stu-id="fcb0e-117">Entity</span></span>
<span data-ttu-id="fcb0e-118">Müşteriler, ürünler ve siparişler gibi uygulama verilerini temsil eden bir sınıf veya nesne.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-118">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="fcb0e-119">Varlık Veri Modeli</span><span class="sxs-lookup"><span data-stu-id="fcb0e-119">Entity Data Model</span></span>
<span data-ttu-id="fcb0e-120">Varlıkları ve aralarındaki ilişkileri açıklayan bir model.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-120">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="fcb0e-121">EF, geliştirici programlarının yanındaki kavramsal modeli anlatmak için EDM kullanır.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-121">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="fcb0e-122">EDM derlemeleri Dr. Peter Chen tarafından tanıtılan varlık Ilişki modelinde.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-122">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="fcb0e-123">EDM, ilk olarak Microsoft 'un bir geliştirici ve sunucu teknolojileri paketi genelinde ortak veri modeli olma konusunda birincil hedefle geliştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-123">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="fcb0e-124">EDM, OData protokolünün bir parçası olarak da kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-124">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="fcb0e-125">Açık yükleme</span><span class="sxs-lookup"><span data-stu-id="fcb0e-125">Explicit loading</span></span>
<span data-ttu-id="fcb0e-126">Bir API çağırarak ilgili nesnelerin yüklendiği ilgili verileri yükleme kalıbı.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-126">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="fcb0e-127">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="fcb0e-127">Fluent API</span></span>
<span data-ttu-id="fcb0e-128">Code First modeli yapılandırmak için kullanılabilen bir API.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-128">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="fcb0e-129">Yabancı anahtar ilişkilendirmesi</span><span class="sxs-lookup"><span data-stu-id="fcb0e-129">Foreign key association</span></span>
<span data-ttu-id="fcb0e-130">Bağımlı varlığın sınıfına, yabancı anahtarı temsil eden bir özelliğin dahil olduğu varlıklar arasındaki ilişki.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-130">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="fcb0e-131">Örneğin, ürün bir CategoryID özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-131">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="fcb0e-132">İlişki tanımlama</span><span class="sxs-lookup"><span data-stu-id="fcb0e-132">Identifying relationship</span></span>
<span data-ttu-id="fcb0e-133">Asıl varlığın birincil anahtarının bağımlı varlığın birincil anahtarının bir parçası olduğu bir ilişki.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-133">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="fcb0e-134">Bu tür bir ilişkide, bağımlı varlık asıl varlık olmadan bulunamaz.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-134">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="fcb0e-135">Bağımsız ilişkilendirme</span><span class="sxs-lookup"><span data-stu-id="fcb0e-135">Independent association</span></span>
<span data-ttu-id="fcb0e-136">Bağımlı varlık sınıfındaki yabancı anahtarı temsil eden özellik olmayan varlıklar arasındaki ilişki.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-136">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="fcb0e-137">Örneğin, bir ürün sınıfı kategori ile ilişki içerir ancak CategoryID özelliği yoktur.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-137">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="fcb0e-138">Entity Framework, ilişkinin durumunu iki ilişkilendirme sonunda varlıkların durumundan bağımsız olarak izler.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-138">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="fcb0e-139">Geç yükleme</span><span class="sxs-lookup"><span data-stu-id="fcb0e-139">Lazy loading</span></span>
<span data-ttu-id="fcb0e-140">Bir gezinti özelliğine erişildiğinde ilgili nesnelerin otomatik olarak yüklendiği ilgili verileri yükleme kalıbı.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-140">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="fcb0e-141">Model First</span><span class="sxs-lookup"><span data-stu-id="fcb0e-141">Model First</span></span>
<span data-ttu-id="fcb0e-142">Yeni bir veritabanı oluşturmak için kullanılan EF tasarımcısını kullanarak Entity Framework modeli oluşturma.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-142">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="fcb0e-143">Gezinti özelliği</span><span class="sxs-lookup"><span data-stu-id="fcb0e-143">Navigation property</span></span>
<span data-ttu-id="fcb0e-144">Başka bir varlığa başvuran bir varlığın özelliği.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-144">A property of an entity that references another entity.</span></span> <span data-ttu-id="fcb0e-145">Örneğin, ürün bir kategori gezinti özelliği içerir ve kategori bir ürünler gezinti özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-145">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="fcb0e-146">POCO</span><span class="sxs-lookup"><span data-stu-id="fcb0e-146">POCO</span></span>
<span data-ttu-id="fcb0e-147">Düz eski CLR nesnesi kısaltması.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-147">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="fcb0e-148">Hiçbir çerçeveye bağımlılığı olmayan basit bir Kullanıcı sınıfı.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-148">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="fcb0e-149">EF bağlamında, EntityObject sınıfından türemeyen bir varlık sınıfı, herhangi bir arabirim uygular veya EF içinde tanımlanan öznitelikleri taşır.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-149">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="fcb0e-150">Kalıcılık çerçevesinden ayrılan bu tür varlık sınıfları, "Kalıcılık Ignorant" olarak da kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-150">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="fcb0e-151">İlişki ters</span><span class="sxs-lookup"><span data-stu-id="fcb0e-151">Relationship inverse</span></span>
<span data-ttu-id="fcb0e-152">Bir ilişkinin ters sonu (örneğin, ürün). Kategori ve kategori. Ürünüyle.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-152">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="fcb0e-153">Kendini izleyen varlık</span><span class="sxs-lookup"><span data-stu-id="fcb0e-153">Self-tracking entity</span></span>
<span data-ttu-id="fcb0e-154">N katmanlı geliştirmeye yardımcı olan kod oluşturma şablonundan oluşturulmuş bir varlık.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-154">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="fcb0e-155">Tablo başına somut tür (TPC)</span><span class="sxs-lookup"><span data-stu-id="fcb0e-155">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="fcb0e-156">Hiyerarşideki her özet olmayan türün veritabanındaki ayrı tabloyla eşlendiği devralmayı eşleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-156">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="fcb0e-157">Hiyerarşi başına tablo (TPH)</span><span class="sxs-lookup"><span data-stu-id="fcb0e-157">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="fcb0e-158">Hiyerarşideki tüm türlerin veritabanındaki aynı tabloyla eşlendiği devralmayı eşleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-158">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="fcb0e-159">Her bir satırın ilişkilendirildiği türü belirlemek için bir Ayrıştırıcı sütunu kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-159">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="fcb0e-160">Tür başına tablo (TPT)</span><span class="sxs-lookup"><span data-stu-id="fcb0e-160">Table-per-type (TPT)</span></span>
<span data-ttu-id="fcb0e-161">Hiyerarşideki tüm türlerin ortak özelliklerinin veritabanındaki aynı tabloyla eşlendiği devralmayı eşleme yöntemi, ancak her bir türe özgü özellikler ayrı bir tabloya eşlenir.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-161">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="fcb0e-162">Tür keşfi</span><span class="sxs-lookup"><span data-stu-id="fcb0e-162">Type discovery</span></span>
<span data-ttu-id="fcb0e-163">Bir Entity Framework modelinin parçası olması gereken türleri tanımlama işlemi.</span><span class="sxs-lookup"><span data-stu-id="fcb0e-163">The process of identifying the types that should be part of an Entity Framework model.</span></span>
