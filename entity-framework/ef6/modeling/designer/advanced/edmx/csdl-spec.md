---
title: CSDL belirtimi - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
caps.latest.revision: 3
ms.openlocfilehash: 0ece73a19fe7ea244905bccb728ab2a104c5179f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912864"
---
# <a name="csdl-specification"></a><span data-ttu-id="77b7a-102">CSDL belirtimi</span><span class="sxs-lookup"><span data-stu-id="77b7a-102">CSDL Specification</span></span>
<span data-ttu-id="77b7a-103">Kavramsal şema tanım dili (CSDL) varlıklar, ilişkileri ve kavramsal bir modeli verilerle bir uygulama olun işlevlerini açıklayan bir XML tabanlı bir dilidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="77b7a-104">Bu kavramsal model Entity Framework veya WCF Veri Hizmetleri tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="77b7a-105">CSDL ile açıklanan meta veri varlıkları ve bir veri kaynağına kavramsal modelde tanımlı ilişkiler eşlemek için Entity Framework tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="77b7a-106">Daha fazla bilgi için [SSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) ve [MSL belirtimi](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="77b7a-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="77b7a-107">CSDL, varlık veri modeli, Entity Framework'ün uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="77b7a-108">Bir Entity Framework uygulamasında, kavramsal model meta verilerini (CSDL içinde yazılan) .csdl dosyasından System.Data.Metadata.Edm.EdmItemCollection bir örneğine yüklenir ve yöntemleri kullanılarak erişilebilir System.Data.Metadata.Edm.MetadataWorkspace sınıfı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="77b7a-109">Varlık çerçevesi kavramsal model meta veri kaynağına özgü komutlar kavramsal modeline karşı sorgular çevirmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="77b7a-110">EF Designer, tasarım zamanında bir .edmx dosyası içinde kavramsal model bilgileri depolar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="77b7a-111">Oluşturma zamanında EF Designer Entity Framework tarafından çalışma zamanında gereken .csdl dosyası oluşturmak için bir .edmx dosyası içinde bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="77b7a-112">CSDL sürümleri, XML ad alanları tarafından ayrılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="77b7a-113">CSDL sürümü</span><span class="sxs-lookup"><span data-stu-id="77b7a-113">CSDL Version</span></span> | <span data-ttu-id="77b7a-114">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="77b7a-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="77b7a-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="77b7a-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="77b7a-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="77b7a-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="77b7a-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="77b7a-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="77b7a-118">Association öğesinde (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-118">Association Element (CSDL)</span></span>

<span data-ttu-id="77b7a-119">Bir **ilişkilendirme** öğe iki varlık türleri arasındaki bir ilişkiyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="77b7a-120">İlişkilendirmesine katılan varlık türleri ve varlık türleri çeşitliliği bilinen ilişkinin her iki ucunda olası sayısını belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="77b7a-121">Bir ilişkilendirme end'ün çoğulluğunun bir değer bir (1) sıfır veya bir (0..1) ya da birden çok olabilir (\*).</span><span class="sxs-lookup"><span data-stu-id="77b7a-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="77b7a-122">Bu bilgiler, iki alt son öğe belirtilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="77b7a-123">Bir varlık türünde gösteriliyorsa varlık türü örneklerinin bir ilişkilendirmenin bir ucunda Gezinti özellikleri veya yabancı anahtarlar erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="77b7a-124">Bir uygulamada belirli bir ilişki varlık türleri örnekleri arasında bir ilişki örneğini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="77b7a-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="77b7a-125">İlişkilendirme örnekleri mantıksal olarak bir ilişki kümesi içinde gruplandırılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="77b7a-126">Bir **ilişkilendirme** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-127">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-128">Bitiş (tam olarak 2 öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="77b7a-129">Referentialconstraint'teki (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-130">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-131">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-131">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-132">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="77b7a-133">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-133">Attribute Name</span></span> | <span data-ttu-id="77b7a-134">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-134">Is Required</span></span> | <span data-ttu-id="77b7a-135">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="77b7a-136">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-136">**Name**</span></span>       | <span data-ttu-id="77b7a-137">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-137">Yes</span></span>         | <span data-ttu-id="77b7a-138">İlişkilendirmenin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-139">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="77b7a-140">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-141">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-142">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-142">Example</span></span>

<span data-ttu-id="77b7a-143">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** yabancı anahtarlar üzerinde karşılaştıklarını değil, ilişki **müşteri** ve  **Sipariş** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="77b7a-144">**Çoğulluk** değerler her **son** ilişkisini gösteren diğer birçok **siparişler** ile ilişkili bir **müşteri**, ancak yalnızca bir **müşteri** ile ilişkili bir **sipariş**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="77b7a-145">Ayrıca, **OnDelete** öğesi gösterir tüm **siparişler** belirli bir ilgili **müşteri** ve içine yüklenen ObjectContext silinecek varsa **müşteri** silinir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="77b7a-146">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** yabancı anahtarlar üzerinde kullanıma sunması olduğunda ilişkilendirme **müşteri** ve  **Sipariş** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="77b7a-147">Kullanıma sunulan yabancı anahtarlar ile varlıklar arasında ilişki ile yönetilen bir **Referentialconstraint'teki** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="77b7a-148">Karşılık gelen bir AssociationSetMapping öğesi bu ilişkiyi veri kaynağına eşlemek gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="77b7a-149">AssociationSet öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="77b7a-150">**AssociationSet** kavramsal şema tanım dili (CSDL) öğesinde, aynı türün ilişkilendirme örnekleri için mantıksal bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="77b7a-151">Bir veri kaynağına eşlenebilecek bir gruplandırma ilişkilendirme örnekleri için bir tanımı bir ilişki kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="77b7a-152">**AssociationSet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-153">Belgeleri (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="77b7a-154">Bitiş (tam olarak iki öğe gerekli)</span><span class="sxs-lookup"><span data-stu-id="77b7a-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="77b7a-155">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="77b7a-156">**İlişkilendirme** özniteliği bir ilişki kümesi içeren bir ilişki türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="77b7a-157">Bir ilişki kümesi sonunu yapmak varlık kümeleri ile tam olarak iki alt belirtilen **son** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-158">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-158">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-159">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="77b7a-160">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-160">Attribute Name</span></span>  | <span data-ttu-id="77b7a-161">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-161">Is Required</span></span> | <span data-ttu-id="77b7a-162">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-163">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-163">**Name**</span></span>        | <span data-ttu-id="77b7a-164">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-164">Yes</span></span>         | <span data-ttu-id="77b7a-165">Varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-165">The name of the entity set.</span></span> <span data-ttu-id="77b7a-166">Değerini **adı** öznitelik değeri ile aynı olamaz **ilişkilendirme** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="77b7a-167">**İlişkilendirme**</span><span class="sxs-lookup"><span data-stu-id="77b7a-167">**Association**</span></span> | <span data-ttu-id="77b7a-168">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-168">Yes</span></span>         | <span data-ttu-id="77b7a-169">İlişkilendirme ayarlanmış ilişkilendirme tam olarak nitelenmiş adını örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="77b7a-170">İlişkilendirme ilişki kümesi ile aynı ad alanında olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-171">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="77b7a-172">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-173">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-174">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-174">Example</span></span>

<span data-ttu-id="77b7a-175">Aşağıdaki örnekte gösterildiği bir **EntityContainer** iki öğe **AssociationSet** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="77b7a-176">CollectionType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="77b7a-177">**CollectionType** kavramsal şema tanım dili (CSDL) öğesinde bir işlev parametresi veya işlevin dönüş türü, bir koleksiyon olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="77b7a-178">**CollectionType** parametresi ReturnType (işlev) öğesi veya alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="77b7a-179">Toplama türünü kullanarak belirtilebilir **türü** özniteliği veya şu alt öğelerden biri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="77b7a-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="77b7a-180">**CollectionType**</span></span>
-   <span data-ttu-id="77b7a-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="77b7a-181">ReferenceType</span></span>
-   <span data-ttu-id="77b7a-182">RowType</span><span class="sxs-lookup"><span data-stu-id="77b7a-182">RowType</span></span>
-   <span data-ttu-id="77b7a-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="77b7a-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-184">Bir koleksiyonun türü ile her ikisi de belirtilirse model doğrulama değil **türü** özniteliğini ve bir alt öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-185">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-185">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-186">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **CollectionType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="77b7a-187">Unutmayın **DefaultValue**, **MaxLength**, **FixedLength**, **duyarlık**, **ölçek**,  **Unicode**, ve **harmanlama** öznitelikleri koleksiyonlarına geçerli yalnızca **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="77b7a-188">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-188">Attribute Name</span></span>                                                          | <span data-ttu-id="77b7a-189">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-189">Is Required</span></span> | <span data-ttu-id="77b7a-190">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-191">**Türü**</span><span class="sxs-lookup"><span data-stu-id="77b7a-191">**Type**</span></span>                                                                | <span data-ttu-id="77b7a-192">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-192">No</span></span>          | <span data-ttu-id="77b7a-193">Koleksiyonun türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="77b7a-194">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="77b7a-194">**Nullable**</span></span>                                                            | <span data-ttu-id="77b7a-195">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-195">No</span></span>          | <span data-ttu-id="77b7a-196">**Doğru** (varsayılan değer) veya **False** bağlı olup olmadığını özelliği null değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="77b7a-197">> CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="77b7a-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="77b7a-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="77b7a-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="77b7a-199">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-199">No</span></span>          | <span data-ttu-id="77b7a-200">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="77b7a-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="77b7a-202">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-202">No</span></span>          | <span data-ttu-id="77b7a-203">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="77b7a-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="77b7a-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="77b7a-205">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-205">No</span></span>          | <span data-ttu-id="77b7a-206">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="77b7a-207">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="77b7a-207">**Precision**</span></span>                                                           | <span data-ttu-id="77b7a-208">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-208">No</span></span>          | <span data-ttu-id="77b7a-209">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="77b7a-210">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="77b7a-210">**Scale**</span></span>                                                               | <span data-ttu-id="77b7a-211">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-211">No</span></span>          | <span data-ttu-id="77b7a-212">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="77b7a-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="77b7a-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="77b7a-213">**SRID**</span></span>                                                                | <span data-ttu-id="77b7a-214">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-214">No</span></span>          | <span data-ttu-id="77b7a-215">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="77b7a-216">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="77b7a-217">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="77b7a-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="77b7a-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="77b7a-218">**Unicode**</span></span>                                                             | <span data-ttu-id="77b7a-219">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-219">No</span></span>          | <span data-ttu-id="77b7a-220">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="77b7a-221">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="77b7a-221">**Collation**</span></span>                                                           | <span data-ttu-id="77b7a-222">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-222">No</span></span>          | <span data-ttu-id="77b7a-223">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="77b7a-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="77b7a-224">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **CollectionType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="77b7a-225">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-226">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-227">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-227">Example</span></span>

<span data-ttu-id="77b7a-228">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. bir **CollectionType** işlevi bir koleksiyonu döndürdüğünü belirtmek için öğe **kişi** varlık türleri ( ilebelirtilen**ElementType** özniteliği).</span><span class="sxs-lookup"><span data-stu-id="77b7a-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="77b7a-229">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğe (belirtilmiş **RowType** öğesi).</span><span class="sxs-lookup"><span data-stu-id="77b7a-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="77b7a-230">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. **CollectionType** işlevi parametre olarak bir koleksiyonunu kabul belirtmek için öğe **departmanı** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="77b7a-231">ComplexType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="77b7a-232">A **ComplexType** öğe tanımlar oluşan bir veri yapısı **EdmSimpleType** özellikleri veya diğer karmaşık türler.</span><span class="sxs-lookup"><span data-stu-id="77b7a-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="77b7a-233">Bir karmaşık türü bir varlık türünün veya başka bir karmaşık türü bir özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="77b7a-234">Verileri bir karmaşık tür tanımlar, bir karmaşık türü bir varlık türüne benzerdir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="77b7a-235">Ancak, karmaşık türlerden ve varlık türleri arasındaki bazı temel farklar vardır:</span><span class="sxs-lookup"><span data-stu-id="77b7a-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="77b7a-236">Karmaşık türler kimlikleri (veya anahtarlara) yoksa ve bu nedenle bağımsız olarak var olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="77b7a-237">Karmaşık türler yalnızca varlık türleri veya diğer karmaşık türler özellikleri olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="77b7a-238">Karmaşık türler ilişkilendirmeler katılamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="77b7a-239">Ne bir ilişki sonu bir karmaşık türü olabilir ve bu nedenle karmaşık türler için Gezinti özellikleri tanımlanamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="77b7a-240">Her bir karmaşık tür skaler özellikleri ayarlanabilir ancak bir karmaşık tür özelliği bir null değer olamaz null.</span><span class="sxs-lookup"><span data-stu-id="77b7a-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="77b7a-241">A **ComplexType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-242">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-243">Özellik (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="77b7a-244">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="77b7a-245">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ComplexType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="77b7a-246">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="77b7a-247">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-247">Is Required</span></span> | <span data-ttu-id="77b7a-248">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-249">Ad</span><span class="sxs-lookup"><span data-stu-id="77b7a-249">Name</span></span>                                                                                                           | <span data-ttu-id="77b7a-250">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-250">Yes</span></span>         | <span data-ttu-id="77b7a-251">Karmaşık tür adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-251">The name of the complex type.</span></span> <span data-ttu-id="77b7a-252">Karmaşık bir tür adını başka bir ad ile aynı olamaz karmaşık tür, varlık türünün veya model kapsamında olan ilişkisi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="77b7a-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="77b7a-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="77b7a-254">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-254">No</span></span>          | <span data-ttu-id="77b7a-255">Tanımlanmakta olan karmaşık türün temel türü başka bir karmaşık tür adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="77b7a-256">> Bu öznitelik CSDL v1'de geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="77b7a-257">Karmaşık türleri için devralma, bu sürümde desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="77b7a-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="77b7a-258">Özet</span><span class="sxs-lookup"><span data-stu-id="77b7a-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="77b7a-259">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-259">No</span></span>          | <span data-ttu-id="77b7a-260">**Doğru** veya **False** (varsayılan değer) karmaşık türü soyut bir tür olup olmamasına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="77b7a-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="77b7a-261">> Bu öznitelik CSDL v1'de geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="77b7a-262">Karmaşık türler bu sürümde, soyut türlerin olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="77b7a-263">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ComplexType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="77b7a-264">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-265">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-266">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-266">Example</span></span>

<span data-ttu-id="77b7a-267">Aşağıdaki örnek, bir karmaşık tür gösterir **adresi**, ile **EdmSimpleType** özellikleri **StreetAddress**, **Şehir**,  **Eyaletveİl**, **Ülke**, ve **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="77b7a-268">Karmaşık tür tanımlamak için **adresi** (yukarıda) bir varlık türünün bir özellik olarak, özellik türü varlık tür tanımında bildirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="77b7a-269">Aşağıdaki örnekte gösterildiği **adresi** özelliği üzerinde bir varlık türü karmaşık bir tür olarak (**yayımcı**):</span><span class="sxs-lookup"><span data-stu-id="77b7a-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="77b7a-270">DefiningExpression öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="77b7a-271">**DefiningExpression** kavramsal şema tanım dili (CSDL) öğesinde kavramsal modelde bir işlevi tanımlayan bir varlık SQL ifadesi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="77b7a-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="77b7a-272">Doğrulama amacıyla bir **DefiningExpression** rasgele içerik öğesi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="77b7a-273">Ancak, Entity Framework bağlanamazsa özel durum çalışma zamanında bir **DefiningExpression** öğesi geçerli varlık SQL içermiyor.</span><span class="sxs-lookup"><span data-stu-id="77b7a-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-274">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-274">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-275">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **DefiningExpression** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="77b7a-276">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-277">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="77b7a-278">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-278">Example</span></span>

<span data-ttu-id="77b7a-279">Aşağıdaki örnekte bir **DefiningExpression** bir kitap yayımlandığı tarihten sonra geçen yıl sayısını döndüren bir işlev tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="77b7a-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="77b7a-280">İçeriği **DefiningExpression** öğesi varlık SQL yazılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="77b7a-281">Bağımlı öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="77b7a-282">**Bağımlı** kavramsal şema tanım dili (CSDL) öğesinde Referentialconstraint'teki öğesi bir alt öğesidir ve başvuru kısıtlamasını bağımlı sonuna tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="77b7a-283">A **Referentialconstraint'teki** öğe ilişkisel bir veritabanındaki bir başvuru bütünlüğü kısıtlaması benzer işlevselliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="77b7a-284">Bir veritabanı tablosundan bir sütuna (veya sütun) başka bir tablonun birincil anahtarı başvurabilirsiniz aynı şekilde, başka bir varlık türünün Varlık anahtarı bir varlık türünün bir özelliği (veya Özellikler) başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="77b7a-285">Başvurulan varlık türü olarak adlandırılır *birincil ucu* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="77b7a-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="77b7a-286">Birincil ucu başvuran varlık türü olarak adlandırılan *bağımlı son* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="77b7a-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="77b7a-287">**PropertyRef** öğeleri birincil ucu hangi anahtarları başvuru belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="77b7a-288">**Bağımlı** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-289">PropertyRef (bir veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="77b7a-290">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-291">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-291">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-292">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **bağımlı** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="77b7a-293">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-293">Attribute Name</span></span> | <span data-ttu-id="77b7a-294">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-294">Is Required</span></span> | <span data-ttu-id="77b7a-295">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="77b7a-296">**Rol**</span><span class="sxs-lookup"><span data-stu-id="77b7a-296">**Role**</span></span>       | <span data-ttu-id="77b7a-297">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-297">Yes</span></span>         | <span data-ttu-id="77b7a-298">Varlık türü bağımlı ucundaki ilişkinin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-299">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **bağımlı** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="77b7a-300">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-301">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-302">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-302">Example</span></span>

<span data-ttu-id="77b7a-303">Aşağıdaki örnekte gösterildiği bir **Referentialconstraint'teki** öğe tanımının bir parçası olarak kullanılan **yayınladığı** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="77b7a-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="77b7a-304">**Publisherıd** özelliği **kitap** varlık türü başvuru kısıtlamasını bağımlı sonuna yapar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="77b7a-305">Belge öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="77b7a-306">**Belgeleri** kavramsal şema tanım dili (CSDL) öğesinde, bir üst öğe içinde tanımlanan bir nesneyle ilgili bilgileri sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="77b7a-307">Bir .edmx dosyası içinde olduğunda **belgeleri** öğesi EF Designer (örneğin, bir varlık, ilişkilendirme veya özellik), tasarım yüzeyindeki bir nesnenin içeriğini görünür bir öğenin alt öğesi olan **belgeleri**  öğesi Visual Studio'da görünür **özellikleri** pencere nesnesi için.</span><span class="sxs-lookup"><span data-stu-id="77b7a-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="77b7a-308">**Belgeleri** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-309">**Özet**: üst öğenin kısa bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="77b7a-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="77b7a-310">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-310">(zero or one element)</span></span>
-   <span data-ttu-id="77b7a-311">**LongDescription**: üst öğenin kapsamlı bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="77b7a-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="77b7a-312">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-312">(zero or one element)</span></span>
-   <span data-ttu-id="77b7a-313">Ek açıklama öğeleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-313">Annotation elements.</span></span> <span data-ttu-id="77b7a-314">(sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-315">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-315">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-316">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **belgeleri** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="77b7a-317">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-318">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="77b7a-319">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-319">Example</span></span>

<span data-ttu-id="77b7a-320">Aşağıdaki örnekte gösterildiği **belgeleri** EntityType öğesinin alt öğesi olarak öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="77b7a-321">CSDL aşağıdaki kod parçacığında bir .edmx içerik, dosya, içeriğini **özeti** ve **LongDescription** öğeleri, Visual Studio'da görüneceği **özellikleri** tıkladığınızda penceresi `Customer` varlık türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="77b7a-322">Bitiş öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-322">End Element (CSDL)</span></span>

<span data-ttu-id="77b7a-323">**Son** kavramsal şema tanım dili (CSDL) öğesinde Association öğesinde veya AssociationSet bir öğenin bir alt olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="77b7a-324">Her durumda, rolü **son** öğe farklı ve uygun öznitelikler farklıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="77b7a-325">Bir ilişkilendirme öğesinin alt öğesi olarak bitiş öğesi</span><span class="sxs-lookup"><span data-stu-id="77b7a-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="77b7a-326">Bir **son** öğesi (alt öğesi olarak **ilişkilendirme** öğesi) bir ilişki sonu ve bu ilişkilendirmeyi sonunda bulunabilir varlık türü örneklerinin varlık türü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="77b7a-327">İlişkilendirme ucu ilişkilendirme bir parçası olarak tanımlanır; bir ilişkiyi tam olarak iki ilişkilendirme ucu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="77b7a-328">Bir varlık türünde gösteriliyorsa varlık türü örneklerinin bir ilişkilendirmenin bir ucunda Gezinti özellikleri veya yabancı anahtarlar erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="77b7a-329">Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-330">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-331">OnDelete (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-332">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-333">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-333">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-334">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="77b7a-335">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-335">Attribute Name</span></span>   | <span data-ttu-id="77b7a-336">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-336">Is Required</span></span> | <span data-ttu-id="77b7a-337">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-338">**Türü**</span><span class="sxs-lookup"><span data-stu-id="77b7a-338">**Type**</span></span>         | <span data-ttu-id="77b7a-339">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-339">Yes</span></span>         | <span data-ttu-id="77b7a-340">İlişkilendirmenin bir ucunda varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="77b7a-341">**Rol**</span><span class="sxs-lookup"><span data-stu-id="77b7a-341">**Role**</span></span>         | <span data-ttu-id="77b7a-342">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-342">No</span></span>          | <span data-ttu-id="77b7a-343">İlişki sonu için bir ad.</span><span class="sxs-lookup"><span data-stu-id="77b7a-343">A name for the association end.</span></span> <span data-ttu-id="77b7a-344">Adı sağlanmazsa, ilişki uç varlık türünün adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="77b7a-345">**Çokluk**</span><span class="sxs-lookup"><span data-stu-id="77b7a-345">**Multiplicity**</span></span> | <span data-ttu-id="77b7a-346">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-346">Yes</span></span>         | <span data-ttu-id="77b7a-347">**1**, **0..1**, veya **\*** ilişkilendirme sonunda olabilir bir varlık türü örneklerinin sayısına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="77b7a-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="77b7a-348">**1** bu tam olarak bir varlık türü örneği var ilişkisi sonuna belirten.</span><span class="sxs-lookup"><span data-stu-id="77b7a-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="77b7a-349">**0..1** sıfır veya bir varlık türü örneklerini ilişkilendirme sonunda bulunduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="77b7a-350">**\*** sıfır, bir veya daha fazla varlık türü örneklerini ilişkilendirme sonunda bulunduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-351">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="77b7a-352">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-353">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="77b7a-354">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-354">Example</span></span>

<span data-ttu-id="77b7a-355">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="77b7a-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="77b7a-356">**Çoğulluk** değerler her **son** ilişkisini gösteren diğer birçok **siparişler** ile ilişkili bir **müşteri**, ancak yalnızca bir **müşteri** ile ilişkili bir **sipariş**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="77b7a-357">Ayrıca, **OnDelete** öğesi gösteren tüm **siparişler** belirli bir ilgili **müşteri** ve içine yüklendi ObjectContext olacaktır Silinen if **müşteri** silinir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="77b7a-358">AssociationSet öğesinin bir alt öğesi olarak bitiş öğesi</span><span class="sxs-lookup"><span data-stu-id="77b7a-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="77b7a-359">**Son** öğesi bir ilişki kümesi ucunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="77b7a-360">**AssociationSet** öğesi iki içermelidir **son** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="77b7a-361">İçinde yer alan bilgileri bir **son** öğesi, bir veri kaynağı olarak ayarlanmış bir ilişkilendirme eşlemesini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="77b7a-362">Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-363">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-364">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-365">Ek açıklama öğelerinin diğer tüm alt öğeleri sonra görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="77b7a-366">Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-367">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-367">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-368">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="77b7a-369">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-369">Attribute Name</span></span> | <span data-ttu-id="77b7a-370">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-370">Is Required</span></span> | <span data-ttu-id="77b7a-371">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="77b7a-372">**EntitySet**</span></span>  | <span data-ttu-id="77b7a-373">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-373">Yes</span></span>         | <span data-ttu-id="77b7a-374">Adını **EntitySet** üst ucunu tanımlayan öğe **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="77b7a-375">**EntitySet** öğesi tanımlanmış, üst ile aynı varlık kapsayıcıda **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="77b7a-376">**Rol**</span><span class="sxs-lookup"><span data-stu-id="77b7a-376">**Role**</span></span>       | <span data-ttu-id="77b7a-377">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-377">No</span></span>          | <span data-ttu-id="77b7a-378">Son ilişkilendirmenin adı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="77b7a-378">The name of the association set end.</span></span> <span data-ttu-id="77b7a-379">Varsa **rol** özniteliği kullanılmazsa, ilişki sonu Ayarla adını varlık kümesinin adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="77b7a-380">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="77b7a-381">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-382">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="77b7a-383">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-383">Example</span></span>

<span data-ttu-id="77b7a-384">Aşağıdaki örnekte gösterildiği bir **EntityContainer** iki öğe **AssociationSet** öğeleri, her iki **son** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="77b7a-385">EntityContainer öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="77b7a-386">**EntityContainer** kavramsal şema tanım dili (CSDL) öğesinde, varlık kümelerini ve ilişki Setleri işlevi içeri aktarmalar için mantıksal bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="77b7a-387">Kavramsal model varlık kapsayıcısı depolama modelinin varlık kapsayıcısı Entitycontainermapping'indeki öğesi ile eşlenir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="77b7a-388">Bir depolama modelinin varlık kapsayıcısı veritabanının yapısını açıklar: Varlık kümeleri açıklayan tablolar, ilişki Setleri, yabancı anahtar kısıtlamaları açıklar ve işlevi alır, bir veritabanındaki saklı yordamlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="77b7a-389">Bir **EntityContainer** öğesi sıfır veya bir belge öğeleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="77b7a-390">Varsa bir **belgeleri** öğe varsa, tüm gelmelidir **EntitySet**, **AssociationSet**, ve **Functionımport** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="77b7a-391">Bir **EntityContainer** öğesi (listelenen sırayla) sıfır veya daha fazla şu alt öğelerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="77b7a-392">EntitySet</span></span>
-   <span data-ttu-id="77b7a-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="77b7a-393">AssociationSet</span></span>
-   <span data-ttu-id="77b7a-394">Functionımport</span><span class="sxs-lookup"><span data-stu-id="77b7a-394">FunctionImport</span></span>
-   <span data-ttu-id="77b7a-395">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="77b7a-395">Annotation elements</span></span>

<span data-ttu-id="77b7a-396">Genişletebileceğiniz bir **EntityContainer** başka bir deponun içeriğini içerecek şekilde öğesi **EntityContainer** aynı ad alanı içinde olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="77b7a-397">Başka bir deponun içeriğini içerecek şekilde **EntityContainer**, başvuru olarak **EntityContainer** öğesini ayarlayın **Extends** adınaözniteliği **EntityContainer** dahil etmek istediğiniz öğe.</span><span class="sxs-lookup"><span data-stu-id="77b7a-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="77b7a-398">Tüm alt öğeleri dahil **EntityContainer** öğesi başvuru alt öğeleri olarak kabul edilir **EntityContainer** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-399">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-399">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-400">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **kullanma** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="77b7a-401">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-401">Attribute Name</span></span> | <span data-ttu-id="77b7a-402">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-402">Is Required</span></span> | <span data-ttu-id="77b7a-403">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="77b7a-404">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-404">**Name**</span></span>       | <span data-ttu-id="77b7a-405">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-405">Yes</span></span>         | <span data-ttu-id="77b7a-406">Varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="77b7a-407">**Genişletir**</span><span class="sxs-lookup"><span data-stu-id="77b7a-407">**Extends**</span></span>    | <span data-ttu-id="77b7a-408">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-408">No</span></span>          | <span data-ttu-id="77b7a-409">Aynı ad alanı içinde başka bir varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-410">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityContainer** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="77b7a-411">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-412">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-413">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-413">Example</span></span>

<span data-ttu-id="77b7a-414">Aşağıdaki örnekte gösterildiği bir **EntityContainer** üç varlık setleri ve iki ilişki Setleri tanımlayan öğe.</span><span class="sxs-lookup"><span data-stu-id="77b7a-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="77b7a-415">Entityset'in öğe (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="77b7a-416">**EntitySet** kavramsal şema tanım dili elemanıdır örnekleri bir varlık türünün ve bu varlık türünden türetilmiş herhangi bir tür örnekleri için mantıksal kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="77b7a-417">Bir varlık türü ve bir varlık kümesi arasındaki ilişki, ilişkisel bir veritabanındaki bir tabloda bir satırı arasındaki ilişkiye benzer.</span><span class="sxs-lookup"><span data-stu-id="77b7a-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="77b7a-418">Bir satır gibi ilgili veri kümesi bir varlık türü tanımlar ve bir tablo gibi bir varlık kümesini bu tanımı örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="77b7a-419">Bir varlık kümesini, böylece bir veri kaynağında ilgili veri yapıları için eşlenebilir varlık türü örneklerini gruplamak için bir yapı sağlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="77b7a-420">Bir özel varlık türü için birden fazla varlık tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-421">EF Designer türü başına birden çok varlık kümeleri içeren kavramsal modeller desteklemez.</span><span class="sxs-lookup"><span data-stu-id="77b7a-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="77b7a-422">**EntitySet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-423">Belge öğesi (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="77b7a-424">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-425">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-425">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-426">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntitySet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="77b7a-427">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-427">Attribute Name</span></span> | <span data-ttu-id="77b7a-428">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-428">Is Required</span></span> | <span data-ttu-id="77b7a-429">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-430">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-430">**Name**</span></span>       | <span data-ttu-id="77b7a-431">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-431">Yes</span></span>         | <span data-ttu-id="77b7a-432">Varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="77b7a-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="77b7a-433">**EntityType**</span></span> | <span data-ttu-id="77b7a-434">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-434">Yes</span></span>         | <span data-ttu-id="77b7a-435">Varlık türü için varlık kümesi tam olarak nitelenmiş adını örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-436">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntitySet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="77b7a-437">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-438">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-439">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-439">Example</span></span>

<span data-ttu-id="77b7a-440">Aşağıdaki örnekte gösterildiği bir **EntityContainer** üç öğeyle **EntitySet** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

<span data-ttu-id="77b7a-441">Tür (MEST) başına birden çok varlık kümesi tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="77b7a-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="77b7a-442">Aşağıdaki örnek, bir varlık kapsayıcısı için iki varlık kümeleri tanımlar **kitap** varlık türü:</span><span class="sxs-lookup"><span data-stu-id="77b7a-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="77b7a-443">EntityType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="77b7a-444">**EntityType** öğesi, bir müşteri ya da kavramsal modelde sırası gibi üst düzey bir kavram yapısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="77b7a-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="77b7a-445">Bir şablon için bir varlık türü olduğu varlık türleri bir uygulama örneği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="77b7a-446">Her şablon, aşağıdaki bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="77b7a-447">Benzersiz bir ad.</span><span class="sxs-lookup"><span data-stu-id="77b7a-447">A unique name.</span></span> <span data-ttu-id="77b7a-448">(Gerekli)</span><span class="sxs-lookup"><span data-stu-id="77b7a-448">(Required.)</span></span>
-   <span data-ttu-id="77b7a-449">Bir veya daha fazla özellikleri tarafından tanımlanan bir varlık anahtarı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="77b7a-450">(Gerekli)</span><span class="sxs-lookup"><span data-stu-id="77b7a-450">(Required.)</span></span>
-   <span data-ttu-id="77b7a-451">Veri içeren özellikleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-451">Properties for containing data.</span></span> <span data-ttu-id="77b7a-452">(İsteğe bağlı.)</span><span class="sxs-lookup"><span data-stu-id="77b7a-452">(Optional.)</span></span>
-   <span data-ttu-id="77b7a-453">Bir ilişkilendirmenin bir uçtan diğerine Gezinti diğer ucuna izin Gezinti özellikleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="77b7a-454">(İsteğe bağlı.)</span><span class="sxs-lookup"><span data-stu-id="77b7a-454">(Optional.)</span></span>

<span data-ttu-id="77b7a-455">Bir uygulamada (örneğin, belirli müşteri veya sipariş) belirli bir nesnesi bir varlık türünün bir örneği temsil eder.</span><span class="sxs-lookup"><span data-stu-id="77b7a-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="77b7a-456">Bir varlık türünün her örneğinin bir varlık kümesi içinde benzersiz Varlık anahtarı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="77b7a-457">İki varlık türü örnekleri yalnızca aynı türde oldukları ve bunların varlık anahtarları aynı değerleri eşit olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="77b7a-458">Bir **EntityType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-459">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-460">Anahtar (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-461">Özellik (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="77b7a-462">NavigationProperty (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="77b7a-463">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-464">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-464">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-465">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntityType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="77b7a-466">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="77b7a-467">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-467">Is Required</span></span> | <span data-ttu-id="77b7a-468">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-469">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="77b7a-470">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-470">Yes</span></span>         | <span data-ttu-id="77b7a-471">Varlık türü adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="77b7a-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="77b7a-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="77b7a-473">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-473">No</span></span>          | <span data-ttu-id="77b7a-474">Tanımlanmakta olan varlık türünün temel türü başka bir varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="77b7a-475">**Özet**</span><span class="sxs-lookup"><span data-stu-id="77b7a-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="77b7a-476">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-476">No</span></span>          | <span data-ttu-id="77b7a-477">**Doğru** veya **False**varlık türü soyut bir tür olup bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="77b7a-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="77b7a-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="77b7a-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="77b7a-479">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-479">No</span></span>          | <span data-ttu-id="77b7a-480">**Doğru** veya **False** varlık türü bir açık varlık türü olup olmamasına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="77b7a-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="77b7a-481">> **OpenType** özniteliktir yalnızca ADO.NET Data Services ile kullanılan kavramsal modellerde tanımlanan varlık türleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="77b7a-482">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="77b7a-483">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-484">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-485">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-485">Example</span></span>

<span data-ttu-id="77b7a-486">Aşağıdaki örnekte gösterildiği bir **EntityType** üç öğeyle **özelliği** öğeleri ve iki **NavigationProperty** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="77b7a-487">EnumType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="77b7a-488">**EnumType** öğesi bir listeden seçimli türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="77b7a-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="77b7a-489">Bir **EnumType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-490">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-491">Üye (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="77b7a-492">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-493">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-493">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-494">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EnumType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="77b7a-495">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-495">Attribute Name</span></span>     | <span data-ttu-id="77b7a-496">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-496">Is Required</span></span> | <span data-ttu-id="77b7a-497">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-498">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-498">**Name**</span></span>           | <span data-ttu-id="77b7a-499">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-499">Yes</span></span>         | <span data-ttu-id="77b7a-500">Varlık türü adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="77b7a-501">**Isflags**</span><span class="sxs-lookup"><span data-stu-id="77b7a-501">**IsFlags**</span></span>        | <span data-ttu-id="77b7a-502">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-502">No</span></span>          | <span data-ttu-id="77b7a-503">**Doğru** veya **False**sabit listesi türü bayrakları bir dizi kullanılıp kullanılamayacağı bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="77b7a-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="77b7a-504">Varsayılan değer **yanlış.**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="77b7a-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="77b7a-505">**UnderlyingType**</span></span> | <span data-ttu-id="77b7a-506">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-506">No</span></span>          | <span data-ttu-id="77b7a-507">**Edm.Byte**, **Edm.Int16**, **EDM.Int32**, **EDM.Int64** veya **Edm.SByte** tür değerleri aralığı tanımlama.</span><span class="sxs-lookup"><span data-stu-id="77b7a-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="77b7a-508">Sabit listesi öğeleri listesinin temel alınan türü varsayılandır **EDM.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-509">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EnumType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="77b7a-510">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-511">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-512">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-512">Example</span></span>

<span data-ttu-id="77b7a-513">Aşağıdaki örnekte gösterildiği bir **EnumType** üç öğeyle **üye** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="77b7a-514">Function öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-514">Function Element (CSDL)</span></span>

<span data-ttu-id="77b7a-515">**İşlevi** kavramsal şema tanım dili (CSDL) öğesinde tanımlayın veya kavramsal modeldeki işlevleri bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="77b7a-516">Bir işlev DefiningExpression öğesi kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="77b7a-517">A **işlevi** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-518">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-519">Parametre (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="77b7a-520">DefiningExpression (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-521">ReturnType (işlev) (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-522">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="77b7a-523">Dönüş türü için bir işlev ile birlikte belirtilmelidir **ReturnType** (işlev) öğesi veya **ReturnType** özniteliği (aşağıya bakın), ancak ikisine birden değil.</span><span class="sxs-lookup"><span data-stu-id="77b7a-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="77b7a-524">Olası dönüş türleri herhangi EdmSimpleType, varlık türü, karmaşık tür, satır türü veya başvuru türü (veya bu tür bir koleksiyonu) var.</span><span class="sxs-lookup"><span data-stu-id="77b7a-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-525">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-525">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-526">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **işlevi** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="77b7a-527">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-527">Attribute Name</span></span> | <span data-ttu-id="77b7a-528">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-528">Is Required</span></span> | <span data-ttu-id="77b7a-529">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="77b7a-530">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-530">**Name**</span></span>       | <span data-ttu-id="77b7a-531">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-531">Yes</span></span>         | <span data-ttu-id="77b7a-532">İşlevin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-532">The name of the function.</span></span>          |
| <span data-ttu-id="77b7a-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="77b7a-533">**ReturnType**</span></span> | <span data-ttu-id="77b7a-534">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-534">No</span></span>          | <span data-ttu-id="77b7a-535">İşlev tarafından döndürülen tür.</span><span class="sxs-lookup"><span data-stu-id="77b7a-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-536">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **işlevi** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="77b7a-537">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-538">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-539">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-539">Example</span></span>

<span data-ttu-id="77b7a-540">Aşağıdaki örnekte bir **işlevi** bir eğitmen işe alındım beri yıl sayısını döndüren bir işlev tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="77b7a-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="77b7a-541">Functionımport öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="77b7a-542">**Functionımport** kavramsal şema tanım dili (CSDL) öğesinde tanımlanan veri kaynağındaki ancak nesnelere kavramsal model aracılığıyla kullanılabilir olan bir işlevi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="77b7a-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="77b7a-543">Örneğin, depolama modelinin Function öğesinde, bir veritabanında bir saklı yordam temsil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="77b7a-544">A **Functionımport** kavramsal model öğesinde karşılık gelen işlev bir Entity Framework uygulamasını temsil eder ve depolama modeli işleve Functionımportmapping element kullanılarak eşlendi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="77b7a-545">Uygulamada işlev çağrıldığında, karşılık gelen saklı yordamı veritabanında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="77b7a-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="77b7a-546">**Functionımport** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-547">Belgeleri (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="77b7a-548">Parametre (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="77b7a-549">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="77b7a-550">ReturnType (Functionımport) (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="77b7a-551">Bir **parametre** öğesi, işlev kabul eden her parametre için tanımlanmış olması.</span><span class="sxs-lookup"><span data-stu-id="77b7a-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="77b7a-552">Dönüş türü için bir işlev ile birlikte belirtilmelidir **ReturnType** (Functionımport) öğesi veya **ReturnType** özniteliği (aşağıya bakın), ancak ikisine birden değil.</span><span class="sxs-lookup"><span data-stu-id="77b7a-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="77b7a-553">Dönüş türü değeri koleksiyonu EdmSimpleType, EntityType veya ComplexType olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-554">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-554">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-555">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **Functionımport** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="77b7a-556">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-556">Attribute Name</span></span>   | <span data-ttu-id="77b7a-557">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-557">Is Required</span></span> | <span data-ttu-id="77b7a-558">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-559">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-559">**Name**</span></span>         | <span data-ttu-id="77b7a-560">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-560">Yes</span></span>         | <span data-ttu-id="77b7a-561">İçeri aktarılan bir işlevin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="77b7a-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="77b7a-562">**ReturnType**</span></span>   | <span data-ttu-id="77b7a-563">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-563">No</span></span>          | <span data-ttu-id="77b7a-564">İşlevinin döndürdüğü türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-564">The type that the function returns.</span></span> <span data-ttu-id="77b7a-565">İşlev bir değer döndürmezse bu özniteliği kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="77b7a-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="77b7a-566">Aksi takdirde, değeri, ComplexType, EntityType veya EDMSimpleType koleksiyonu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="77b7a-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="77b7a-567">**EntitySet**</span></span>    | <span data-ttu-id="77b7a-568">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-568">No</span></span>          | <span data-ttu-id="77b7a-569">Varlık koleksiyonu işlevi döndürürse, türleri, değerini **EntitySet** koleksiyona ait olduğu varlık kümesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="77b7a-570">Aksi takdirde, **EntitySet** özniteliği değil kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="77b7a-571">**Iscomposable**</span><span class="sxs-lookup"><span data-stu-id="77b7a-571">**IsComposable**</span></span> | <span data-ttu-id="77b7a-572">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-572">No</span></span>          | <span data-ttu-id="77b7a-573">Değer ayarlanmışsa true, işlev birleştirilebilir (tablo değerli işlev) ve bir LINQ Sorgu kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="77b7a-574">Varsayılan değer **false**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="77b7a-575">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **Functionımport** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="77b7a-576">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-577">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-578">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-578">Example</span></span>

<span data-ttu-id="77b7a-579">Aşağıdaki örnekte gösterildiği bir **Functionımport** öğesi, bir parametre kabul eden ve varlık türleri koleksiyonunu döndürür:</span><span class="sxs-lookup"><span data-stu-id="77b7a-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="77b7a-580">Anahtar öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-580">Key Element (CSDL)</span></span>

<span data-ttu-id="77b7a-581">**Anahtarı** öğesi EntityType öğesi bir alt öğesidir ve tanımlayan bir *Varlık anahtarı* (bir özellik veya bir varlık türünün kimliğini belirlemek özellikler kümesi).</span><span class="sxs-lookup"><span data-stu-id="77b7a-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="77b7a-582">Bir varlık anahtarı oluşturan özelliklerini tasarım zamanında seçilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="77b7a-583">Varlık anahtar özelliklerin değerlerini çalışma zamanında ayarlama varlık içinde bir varlık türü örneği benzersiz şekilde tanımlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="77b7a-584">Bir varlık kümesindeki örneklerin benzersizliği garanti etmek için bir varlık anahtarı oluşturan özellikleri seçilmelidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="77b7a-585">**Anahtar** öğesi bir veya daha fazla özellik bir varlık türünün başvurarak Varlık anahtarı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="77b7a-586">**Anahtar** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="77b7a-587">PropertyRef (bir veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="77b7a-588">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-589">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-589">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-590">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **anahtar** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="77b7a-591">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-592">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="77b7a-593">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-593">Example</span></span>

<span data-ttu-id="77b7a-594">Aşağıdaki örnekte adlı bir varlık türü tanımlayan **kitap**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="77b7a-595">Varlık anahtarı başvurarak tanımlanan **ISBN** varlık türü özelliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="77b7a-596">**ISBN** özelliği olduğundan Varlık anahtarı için iyi bir seçim bir kitap tanıtan bir Uluslararası Standart Kitap sayısı (ISBN).</span><span class="sxs-lookup"><span data-stu-id="77b7a-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="77b7a-597">Aşağıdaki örnek, bir varlık türü gösterir (**Yazar**) iki özelliklerini içeren bir varlık anahtarına sahip **adı** ve **adresi**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

<span data-ttu-id="77b7a-598">Kullanarak **adı** ve **adresi** çünkü aynı adlı iki yazarları aynı adresindeki live olasılığının düşük olduğu varlık için makul bir seçim anahtardır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="77b7a-599">Ancak, bu seçenek bir varlık anahtarı için bir varlık kümesindeki benzersiz varlık anahtarları kesinlikle garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="77b7a-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="77b7a-600">Bir özellik gibi ekleme **AuthorId**, benzersiz şekilde tanımlamak için kullanılabilir bir yazar önerilen bu durumda.</span><span class="sxs-lookup"><span data-stu-id="77b7a-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="77b7a-601">Üye öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-601">Member Element (CSDL)</span></span>

<span data-ttu-id="77b7a-602">**Üye** öğesi EnumType öğesi bir alt öğesidir ve numaralandırılmış tür üyesi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-603">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-603">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-604">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **Functionımport** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="77b7a-605">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-605">Attribute Name</span></span> | <span data-ttu-id="77b7a-606">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-606">Is Required</span></span> | <span data-ttu-id="77b7a-607">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-608">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-608">**Name**</span></span>       | <span data-ttu-id="77b7a-609">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-609">Yes</span></span>         | <span data-ttu-id="77b7a-610">Üyenin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="77b7a-611">**Değer**</span><span class="sxs-lookup"><span data-stu-id="77b7a-611">**Value**</span></span>      | <span data-ttu-id="77b7a-612">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-612">No</span></span>          | <span data-ttu-id="77b7a-613">Üyenin değeri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-613">The value of the member.</span></span> <span data-ttu-id="77b7a-614">Varsayılan olarak 0 değeri ilk üye sahiptir ve art arda gelen her Numaralandırıcı değeri 1 azaltılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="77b7a-615">Aynı değerlere sahip birden fazla üye var olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-616">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **Functionımport** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="77b7a-617">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-618">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-619">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-619">Example</span></span>

<span data-ttu-id="77b7a-620">Aşağıdaki örnekte gösterildiği bir **EnumType** üç öğeyle **üye** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="77b7a-621">NavigationProperty öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="77b7a-622">A **NavigationProperty** öğe bir ilişki sonu bir başvuru sağlayan bir gezinti özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="77b7a-623">Özellik öğesi ile tanımlanan özelliklerin aksine, gezinti özellikleri veri özelliklerini ve Şekil tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="77b7a-624">İki varlık türleri arasındaki ilişkiyi gitmek için bir yol sağlarlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="77b7a-625">Gezinti özellikleri her iki varlık türlerini ilişkilendirme ucunda isteğe bağlı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="77b7a-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="77b7a-626">İlişkilendirme sonunda bir varlık türünün gezinme özelliğinin tanımlarsanız, ilişkinin diğer ucundaki varlık türünün gezinme özelliğinin tanımlamak zorunda değil.</span><span class="sxs-lookup"><span data-stu-id="77b7a-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="77b7a-627">Bir gezinti özelliği tarafından döndürülen veri türü Uzak ilişkilendirme bitiş'ün çoğulluğunun tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="77b7a-628">Örneğin, bir gezinti özelliği varsayalım **OrdersNavProp**, bulunuyor. bir **müşteri** varlık türü ve arasında bir-çok ilişkisi gider **müşteri** ve  **Sipariş**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="77b7a-629">Gezinti özelliği için Uzak ilişkilendirme end çok çeşitlilik olduğundan (\*), kendi veri türüne koleksiyonudur (, **sipariş**).</span><span class="sxs-lookup"><span data-stu-id="77b7a-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="77b7a-630">Benzer şekilde, bir gezinti özelliği, **CustomerNavProp**, bulunuyor **sipariş** varlık türü, veri türü olacak **müşteri** uzak uç çokluğu olduğundan bir (1).</span><span class="sxs-lookup"><span data-stu-id="77b7a-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="77b7a-631">A **NavigationProperty** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-632">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-633">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-634">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-634">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-635">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **NavigationProperty** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="77b7a-636">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-636">Attribute Name</span></span>   | <span data-ttu-id="77b7a-637">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-637">Is Required</span></span> | <span data-ttu-id="77b7a-638">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-639">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-639">**Name**</span></span>         | <span data-ttu-id="77b7a-640">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-640">Yes</span></span>         | <span data-ttu-id="77b7a-641">Gezinme özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="77b7a-642">**İlişki**</span><span class="sxs-lookup"><span data-stu-id="77b7a-642">**Relationship**</span></span> | <span data-ttu-id="77b7a-643">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-643">Yes</span></span>         | <span data-ttu-id="77b7a-644">Modeli kapsamında olmayan bir ilişkilendirmenin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="77b7a-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="77b7a-645">**ToRole**</span></span>       | <span data-ttu-id="77b7a-646">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-646">Yes</span></span>         | <span data-ttu-id="77b7a-647">Gezinti biteceği ilişki sonu.</span><span class="sxs-lookup"><span data-stu-id="77b7a-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="77b7a-648">Değerini **ToRole** özniteliği bir değer ile aynı olmalıdır **rol** (ilişki ucu öğesinde tanımlanan) ilişkilendirme uçlarından tanımlanan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="77b7a-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="77b7a-649">**FromRole**</span></span>     | <span data-ttu-id="77b7a-650">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-650">Yes</span></span>         | <span data-ttu-id="77b7a-651">Gezinti başlar ilişki sonu.</span><span class="sxs-lookup"><span data-stu-id="77b7a-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="77b7a-652">Değerini **FromRole** özniteliği bir değer ile aynı olmalıdır **rol** (ilişki ucu öğesinde tanımlanan) ilişkilendirme uçlarından tanımlanan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-653">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **NavigationProperty** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="77b7a-654">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-655">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-656">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-656">Example</span></span>

<span data-ttu-id="77b7a-657">Aşağıdaki örnek, bir varlık türü tanımlar. (**kitap**) iki Gezinti özellikleri ile (**yayınladığı** ve **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="77b7a-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="77b7a-658">OnDelete öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="77b7a-659">**OnDelete** kavramsal şema tanım dili (CSDL) öğesinde bir ilişkilendirme bağlı davranışını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="77b7a-660">Varsa **eylem** özniteliği **Cascade** ilişkilendirme bir ucunda, ilişkinin diğer ucundaki varlık türleri varlık türü ilk uca silindiğinde silinir ilgili.</span><span class="sxs-lookup"><span data-stu-id="77b7a-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="77b7a-661">İki varlık türleri arasındaki ilişkiyi bir birincil anahtar birincil anahtar ilişkidir sonra yüklenen bir bağımlı nesne bağımsız olarak, ilişkinin diğer ucundaki sorumlusu nesnesi silindiğinde silinir **OnDelete** belirtimi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="77b7a-662">**OnDelete** öğesi yalnızca bir uygulama çalışma zamanı davranışını etkiler; veri kaynağı davranışını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="77b7a-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="77b7a-663">Veri kaynağında tanımlanan davranışı uygulamada tanımlanan davranışı ile aynı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="77b7a-664">Bir **OnDelete** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-665">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-666">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-667">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-667">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-668">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **OnDelete** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="77b7a-669">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-669">Attribute Name</span></span> | <span data-ttu-id="77b7a-670">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-670">Is Required</span></span> | <span data-ttu-id="77b7a-671">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-672">**Eylem**</span><span class="sxs-lookup"><span data-stu-id="77b7a-672">**Action**</span></span>     | <span data-ttu-id="77b7a-673">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-673">Yes</span></span>         | <span data-ttu-id="77b7a-674">**Art arda** veya **hiçbiri**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-674">**Cascade** or **None**.</span></span> <span data-ttu-id="77b7a-675">Varsa **Cascade**, bağımlı varlık türleri, asıl varlık türü silindiğinde silinir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="77b7a-676">Varsa **hiçbiri**, asıl varlık türü silindiğinde bağımlı varlık türleri silinmeyecek.</span><span class="sxs-lookup"><span data-stu-id="77b7a-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-677">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="77b7a-678">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-679">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-680">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-680">Example</span></span>

<span data-ttu-id="77b7a-681">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="77b7a-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="77b7a-682">**OnDelete** öğesi gösteren tüm **siparişler** belirli bir ilgili **müşteri** ve ObjectContext yüklenen zaman silinecek  **Müşteri** silinir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="77b7a-683">Parametre öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="77b7a-684">**Parametre** kavramsal şema tanım dili (CSDL) öğesinde bir alt Functionımport öğesi veya Function öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="77b7a-685">Functionımport öğesi uygulama</span><span class="sxs-lookup"><span data-stu-id="77b7a-685">FunctionImport Element Application</span></span>

<span data-ttu-id="77b7a-686">A **parametre** öğesi (alt öğesi olarak **Functionımport** öğesi) CSDL içinde bildirilen işlev içeri aktarmalar için giriş ve çıkış parametrelerini tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="77b7a-687">**Parametre** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-688">Belgeleri (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="77b7a-689">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-690">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-690">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-691">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="77b7a-692">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-692">Attribute Name</span></span> | <span data-ttu-id="77b7a-693">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-693">Is Required</span></span> | <span data-ttu-id="77b7a-694">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-695">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-695">**Name**</span></span>       | <span data-ttu-id="77b7a-696">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-696">Yes</span></span>         | <span data-ttu-id="77b7a-697">Parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="77b7a-698">**Türü**</span><span class="sxs-lookup"><span data-stu-id="77b7a-698">**Type**</span></span>       | <span data-ttu-id="77b7a-699">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-699">Yes</span></span>         | <span data-ttu-id="77b7a-700">Parametre türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-700">The parameter type.</span></span> <span data-ttu-id="77b7a-701">Değer olmalıdır bir **EDMSimpleType** veya modeli kapsamında olan bir karmaşık türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="77b7a-702">**Modu**</span><span class="sxs-lookup"><span data-stu-id="77b7a-702">**Mode**</span></span>       | <span data-ttu-id="77b7a-703">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-703">No</span></span>          | <span data-ttu-id="77b7a-704">**İçinde**, **kullanıma**, veya **Inout** parametresi bir giriş, çıkış ve giriş/çıkış parametresi olmasına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="77b7a-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="77b7a-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-705">**MaxLength**</span></span>  | <span data-ttu-id="77b7a-706">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-706">No</span></span>          | <span data-ttu-id="77b7a-707">Parametresinin uzunluğu izin verilen en fazla.</span><span class="sxs-lookup"><span data-stu-id="77b7a-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="77b7a-708">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="77b7a-708">**Precision**</span></span>  | <span data-ttu-id="77b7a-709">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-709">No</span></span>          | <span data-ttu-id="77b7a-710">Parametre duyarlılığı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="77b7a-711">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="77b7a-711">**Scale**</span></span>      | <span data-ttu-id="77b7a-712">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-712">No</span></span>          | <span data-ttu-id="77b7a-713">Parametre ölçeği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="77b7a-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="77b7a-714">**SRID**</span></span>       | <span data-ttu-id="77b7a-715">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-715">No</span></span>          | <span data-ttu-id="77b7a-716">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="77b7a-717">Uzamsal tür parametreleri yalnızca için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="77b7a-718">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="77b7a-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-719">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="77b7a-720">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-721">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="77b7a-722">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-722">Example</span></span>

<span data-ttu-id="77b7a-723">Aşağıdaki örnekte gösterildiği bir **Functionımport** bir öğeyle **parametre** alt öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="77b7a-724">İşlev bir giriş parametresi kabul eden ve varlık türleri koleksiyonunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="77b7a-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="77b7a-725">İşlev öğe uygulama</span><span class="sxs-lookup"><span data-stu-id="77b7a-725">Function Element Application</span></span>

<span data-ttu-id="77b7a-726">A **parametre** öğesi (alt öğesi olarak **işlevi** öğesi) kavramsal modelde tanımlı ya da bildirilen işlevler için parametreleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="77b7a-727">**Parametre** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-728">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="77b7a-729">CollectionType (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="77b7a-730">ReferenceType (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="77b7a-731">RowType (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-732">Yalnızca biri **CollectionType**, **ReferenceType**, veya **RowType** öğeleri alt öğesi olabilir bir **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="77b7a-733">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-734">Ek açıklama öğelerinin diğer tüm alt öğeleri sonra görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="77b7a-735">Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-736">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-736">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-737">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="77b7a-738">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-738">Attribute Name</span></span>   | <span data-ttu-id="77b7a-739">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-739">Is Required</span></span> | <span data-ttu-id="77b7a-740">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-741">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-741">**Name**</span></span>         | <span data-ttu-id="77b7a-742">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-742">Yes</span></span>         | <span data-ttu-id="77b7a-743">Parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="77b7a-744">**Türü**</span><span class="sxs-lookup"><span data-stu-id="77b7a-744">**Type**</span></span>         | <span data-ttu-id="77b7a-745">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-745">No</span></span>          | <span data-ttu-id="77b7a-746">Parametre türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-746">The parameter type.</span></span> <span data-ttu-id="77b7a-747">Bir parametre aşağıdaki türleri (veya bu türler) herhangi biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="77b7a-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="77b7a-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="77b7a-749">varlık türü</span><span class="sxs-lookup"><span data-stu-id="77b7a-749">entity type</span></span> <br/> <span data-ttu-id="77b7a-750">Karmaşık tür</span><span class="sxs-lookup"><span data-stu-id="77b7a-750">complex type</span></span> <br/> <span data-ttu-id="77b7a-751">Satır türü</span><span class="sxs-lookup"><span data-stu-id="77b7a-751">row type</span></span> <br/> <span data-ttu-id="77b7a-752">başvuru türü</span><span class="sxs-lookup"><span data-stu-id="77b7a-752">reference type</span></span>                             |
| <span data-ttu-id="77b7a-753">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="77b7a-753">**Nullable**</span></span>     | <span data-ttu-id="77b7a-754">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-754">No</span></span>          | <span data-ttu-id="77b7a-755">**Doğru** (varsayılan değer) veya **False** özelliğine sahip olup olmadığına bağlı olarak bir **null** değeri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="77b7a-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="77b7a-756">**DefaultValue**</span></span> | <span data-ttu-id="77b7a-757">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-757">No</span></span>          | <span data-ttu-id="77b7a-758">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="77b7a-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-759">**MaxLength**</span></span>    | <span data-ttu-id="77b7a-760">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-760">No</span></span>          | <span data-ttu-id="77b7a-761">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="77b7a-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="77b7a-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-762">**FixedLength**</span></span>  | <span data-ttu-id="77b7a-763">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-763">No</span></span>          | <span data-ttu-id="77b7a-764">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="77b7a-765">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="77b7a-765">**Precision**</span></span>    | <span data-ttu-id="77b7a-766">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-766">No</span></span>          | <span data-ttu-id="77b7a-767">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="77b7a-768">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="77b7a-768">**Scale**</span></span>        | <span data-ttu-id="77b7a-769">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-769">No</span></span>          | <span data-ttu-id="77b7a-770">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="77b7a-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="77b7a-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="77b7a-771">**SRID**</span></span>         | <span data-ttu-id="77b7a-772">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-772">No</span></span>          | <span data-ttu-id="77b7a-773">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="77b7a-774">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="77b7a-775">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="77b7a-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="77b7a-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="77b7a-776">**Unicode**</span></span>      | <span data-ttu-id="77b7a-777">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-777">No</span></span>          | <span data-ttu-id="77b7a-778">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="77b7a-779">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="77b7a-779">**Collation**</span></span>    | <span data-ttu-id="77b7a-780">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-780">No</span></span>          | <span data-ttu-id="77b7a-781">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="77b7a-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="77b7a-782">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="77b7a-783">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-784">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="77b7a-785">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-785">Example</span></span>

<span data-ttu-id="77b7a-786">Aşağıdaki örnekte gösterildiği bir **işlevi** kullanan tek öğe **parametresi** bir işlev parametresi tanımlamak için alt öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a><span data-ttu-id="77b7a-787">Asıl öğe (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="77b7a-788">**Asıl** kavramsal şema tanım dili (CSDL) öğesinde Referentialconstraint'teki öğesine bir başvuru kısıtlamasının birincil ucu tanımlayan bir alt öğedir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="77b7a-789">A **Referentialconstraint'teki** öğe ilişkisel bir veritabanındaki bir başvuru bütünlüğü kısıtlaması benzer işlevselliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="77b7a-790">Bir veritabanı tablosundan bir sütuna (veya sütun) başka bir tablonun birincil anahtarı başvurabilirsiniz aynı şekilde, başka bir varlık türünün Varlık anahtarı bir varlık türünün bir özelliği (veya Özellikler) başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="77b7a-791">Başvurulan varlık türü olarak adlandırılır *birincil ucu* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="77b7a-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="77b7a-792">Birincil ucu başvuran varlık türü olarak adlandırılan *bağımlı son* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="77b7a-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="77b7a-793">**PropertyRef** öğeleri hangi anahtarları bağımlı uç tarafından başvurulan belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="77b7a-794">**Asıl** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-795">PropertyRef (bir veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="77b7a-796">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-797">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-797">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-798">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **asıl** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="77b7a-799">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-799">Attribute Name</span></span> | <span data-ttu-id="77b7a-800">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-800">Is Required</span></span> | <span data-ttu-id="77b7a-801">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="77b7a-802">**Rol**</span><span class="sxs-lookup"><span data-stu-id="77b7a-802">**Role**</span></span>       | <span data-ttu-id="77b7a-803">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-803">Yes</span></span>         | <span data-ttu-id="77b7a-804">İlişkisinin birincil ucu ' varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-805">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **asıl** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="77b7a-806">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-807">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-808">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-808">Example</span></span>

<span data-ttu-id="77b7a-809">Aşağıdaki örnekte gösterildiği bir **Referentialconstraint'teki** tanımının bir parçası olan öğeyi **yayınladığı** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="77b7a-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="77b7a-810">**Kimliği** özelliği **yayımcı** varlık türü başvuru kısıtlamasını birincil ucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="77b7a-811">Özellik öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-811">Property Element (CSDL)</span></span>

<span data-ttu-id="77b7a-812">**Özelliği** kavramsal şema tanım dili (CSDL) öğesinde bir alt öğe EntityType, ComplexType öğesi veya RowType öğesinin olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="77b7a-813">EntityType ve ComplexType öğesi uygulamalar</span><span class="sxs-lookup"><span data-stu-id="77b7a-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="77b7a-814">**Özellik** öğeleri (alt öğeleri olarak **EntityType** veya **ComplexType** öğeleri) şekil ve bir varlık türü örneği veya karmaşık tür örneği içeren veri özelliklerini tanımlar .</span><span class="sxs-lookup"><span data-stu-id="77b7a-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="77b7a-815">Kavramsal modelde özellikleri, bir sınıf üzerinde tanımlanan özelliklere benzer.</span><span class="sxs-lookup"><span data-stu-id="77b7a-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="77b7a-816">Bir sınıfındaki özellikleri sınıfı şeklini tanımlamak ve nesneler hakkında bilgi taşımak aynı şekilde, kavramsal model özelliklerinde bir varlık türü şeklini tanımlamak ve varlık türü örnekleri hakkında bilgi taşımak.</span><span class="sxs-lookup"><span data-stu-id="77b7a-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="77b7a-817">**Özelliği** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-818">Belge öğesi (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="77b7a-819">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="77b7a-820">Aşağıdaki özellikleri uygulanabilir bir **özelliği** öğesi: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **duyarlık**, **ölçek**, **Unicode**, **harmanlama**,  **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="77b7a-821">Modelleri veri deposunda özellik değerlerinin nasıl depolandığını konusunda bilgiler sağlayan XML öznitelikleridir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-822">Modelleri yalnızca türünün özelliklerine uygulanabilir **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-823">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-823">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-824">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="77b7a-825">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-825">Attribute Name</span></span>                                                         | <span data-ttu-id="77b7a-826">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-826">Is Required</span></span> | <span data-ttu-id="77b7a-827">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-828">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-828">**Name**</span></span>                                                               | <span data-ttu-id="77b7a-829">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-829">Yes</span></span>         | <span data-ttu-id="77b7a-830">Özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="77b7a-831">**Türü**</span><span class="sxs-lookup"><span data-stu-id="77b7a-831">**Type**</span></span>                                                               | <span data-ttu-id="77b7a-832">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-832">Yes</span></span>         | <span data-ttu-id="77b7a-833">Özellik değerinin türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-833">The type of the property value.</span></span> <span data-ttu-id="77b7a-834">Özellik değeri türü olmalıdır bir **EDMSimpleType** veya modeli kapsamında olmayan (bir tam ad tarafından gösterilen) bir karmaşık türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="77b7a-835">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="77b7a-835">**Nullable**</span></span>                                                           | <span data-ttu-id="77b7a-836">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-836">No</span></span>          | <span data-ttu-id="77b7a-837">**Doğru** (varsayılan değer) veya <strong>False</strong> bağlı olup olmadığını özelliği null değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="77b7a-838">> CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="77b7a-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="77b7a-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="77b7a-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="77b7a-840">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-840">No</span></span>          | <span data-ttu-id="77b7a-841">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="77b7a-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="77b7a-843">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-843">No</span></span>          | <span data-ttu-id="77b7a-844">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="77b7a-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="77b7a-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="77b7a-846">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-846">No</span></span>          | <span data-ttu-id="77b7a-847">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="77b7a-848">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="77b7a-848">**Precision**</span></span>                                                          | <span data-ttu-id="77b7a-849">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-849">No</span></span>          | <span data-ttu-id="77b7a-850">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="77b7a-851">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="77b7a-851">**Scale**</span></span>                                                              | <span data-ttu-id="77b7a-852">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-852">No</span></span>          | <span data-ttu-id="77b7a-853">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="77b7a-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="77b7a-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="77b7a-854">**SRID**</span></span>                                                               | <span data-ttu-id="77b7a-855">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-855">No</span></span>          | <span data-ttu-id="77b7a-856">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="77b7a-857">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="77b7a-858">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="77b7a-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="77b7a-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="77b7a-859">**Unicode**</span></span>                                                            | <span data-ttu-id="77b7a-860">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-860">No</span></span>          | <span data-ttu-id="77b7a-861">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="77b7a-862">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="77b7a-862">**Collation**</span></span>                                                          | <span data-ttu-id="77b7a-863">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-863">No</span></span>          | <span data-ttu-id="77b7a-864">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="77b7a-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="77b7a-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="77b7a-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="77b7a-866">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-866">No</span></span>          | <span data-ttu-id="77b7a-867">**Hiçbiri** (varsayılan değer) veya **sabit**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="77b7a-868">Değer ayarlanmışsa **sabit**, iyimser eşzamanlılık denetimlerini özellik değeri kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="77b7a-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="77b7a-869">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="77b7a-870">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-871">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="77b7a-872">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-872">Example</span></span>

<span data-ttu-id="77b7a-873">Aşağıdaki örnekte gösterildiği bir **EntityType** üç öğeyle **özelliği** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="77b7a-874">Aşağıdaki örnekte gösterildiği bir **ComplexType** beş öğe **özelliği** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="77b7a-875">RowType öğesinin uygulama</span><span class="sxs-lookup"><span data-stu-id="77b7a-875">RowType Element Application</span></span>

<span data-ttu-id="77b7a-876">**Özellik** öğeleri (alt olarak bir **RowType** öğe) şekil ve geçirilen veya model tanımlı bir işlevden döndürülen verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="77b7a-877">**Özelliği** öğesi şu alt öğelerden tam olarak birine sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="77b7a-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="77b7a-878">CollectionType</span></span>
-   <span data-ttu-id="77b7a-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="77b7a-879">ReferenceType</span></span>
-   <span data-ttu-id="77b7a-880">RowType</span><span class="sxs-lookup"><span data-stu-id="77b7a-880">RowType</span></span>

<span data-ttu-id="77b7a-881">**Özelliği** öğesi sayı alt ek açıklama öğeleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-882">Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-883">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-883">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-884">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="77b7a-885">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-885">Attribute Name</span></span>                                                     | <span data-ttu-id="77b7a-886">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-886">Is Required</span></span> | <span data-ttu-id="77b7a-887">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-888">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-888">**Name**</span></span>                                                           | <span data-ttu-id="77b7a-889">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-889">Yes</span></span>         | <span data-ttu-id="77b7a-890">Özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="77b7a-891">**Türü**</span><span class="sxs-lookup"><span data-stu-id="77b7a-891">**Type**</span></span>                                                           | <span data-ttu-id="77b7a-892">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-892">Yes</span></span>         | <span data-ttu-id="77b7a-893">Özellik değerinin türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="77b7a-894">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="77b7a-894">**Nullable**</span></span>                                                       | <span data-ttu-id="77b7a-895">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-895">No</span></span>          | <span data-ttu-id="77b7a-896">**Doğru** (varsayılan değer) veya **False** bağlı olup olmadığını özelliği null değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="77b7a-897">> CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="77b7a-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="77b7a-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="77b7a-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="77b7a-899">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-899">No</span></span>          | <span data-ttu-id="77b7a-900">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="77b7a-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="77b7a-902">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-902">No</span></span>          | <span data-ttu-id="77b7a-903">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="77b7a-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="77b7a-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="77b7a-905">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-905">No</span></span>          | <span data-ttu-id="77b7a-906">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="77b7a-907">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="77b7a-907">**Precision**</span></span>                                                      | <span data-ttu-id="77b7a-908">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-908">No</span></span>          | <span data-ttu-id="77b7a-909">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="77b7a-910">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="77b7a-910">**Scale**</span></span>                                                          | <span data-ttu-id="77b7a-911">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-911">No</span></span>          | <span data-ttu-id="77b7a-912">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="77b7a-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="77b7a-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="77b7a-913">**SRID**</span></span>                                                           | <span data-ttu-id="77b7a-914">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-914">No</span></span>          | <span data-ttu-id="77b7a-915">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="77b7a-916">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="77b7a-917">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="77b7a-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="77b7a-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="77b7a-918">**Unicode**</span></span>                                                        | <span data-ttu-id="77b7a-919">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-919">No</span></span>          | <span data-ttu-id="77b7a-920">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="77b7a-921">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="77b7a-921">**Collation**</span></span>                                                      | <span data-ttu-id="77b7a-922">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-922">No</span></span>          | <span data-ttu-id="77b7a-923">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="77b7a-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="77b7a-924">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="77b7a-925">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-926">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="77b7a-927">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-927">Example</span></span>

<span data-ttu-id="77b7a-928">Aşağıdaki örnekte gösterildiği **özelliği** model tanımlı bir işlevin dönüş türü şeklini tanımlamak için kullanılan öğeleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="77b7a-929">PropertyRef öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="77b7a-930">**PropertyRef** kavramsal şema tanım dili (CSDL) öğesinde başvuran bir özelliği bir varlık türünün özelliğini aşağıdaki rollerden biri gerçekleştirecek belirtmek için:</span><span class="sxs-lookup"><span data-stu-id="77b7a-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="77b7a-931">(Bir özellik veya bir varlık türünün kimliğini belirlemek özellikler kümesi) varlığın anahtarının bir parçası.</span><span class="sxs-lookup"><span data-stu-id="77b7a-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="77b7a-932">Bir veya daha fazla **PropertyRef** öğeleri, bir varlığın anahtarı tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="77b7a-933">Başvuru kısıtlamasını bağımlı veya asıl bitiş olayı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="77b7a-934">**PropertyRef** öğesi yalnızca alt öğeleri olarak ek açıklama öğelerinin (sıfır veya daha fazla) olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-935">Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-936">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-936">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-937">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **PropertyRef** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="77b7a-938">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-938">Attribute Name</span></span> | <span data-ttu-id="77b7a-939">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-939">Is Required</span></span> | <span data-ttu-id="77b7a-940">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="77b7a-941">**Ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-941">**Name**</span></span>       | <span data-ttu-id="77b7a-942">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-942">Yes</span></span>         | <span data-ttu-id="77b7a-943">Başvurulan özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-944">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **PropertyRef** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="77b7a-945">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-946">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-947">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-947">Example</span></span>

<span data-ttu-id="77b7a-948">Aşağıdaki örnek bir varlık türü tanımlar (**kitap**).</span><span class="sxs-lookup"><span data-stu-id="77b7a-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="77b7a-949">Varlık anahtarı başvurarak tanımlanan **ISBN** varlık türü özelliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="77b7a-950">Sonraki örnekte, iki **PropertyRef** öğeleri ki belirtmek için kullanılan özellikler (**kimliği** ve **Publisherıd**) olan bir başvuru birincil ve bağımlı uçlarından kısıtlama.</span><span class="sxs-lookup"><span data-stu-id="77b7a-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="77b7a-951">ReferenceType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="77b7a-952">**ReferenceType** kavramsal şema tanım dili (CSDL) öğesinde bir varlık türü bir başvuru belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="77b7a-953">**ReferenceType** aşağıdaki öğelerin bir alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="77b7a-954">ReturnType (işlev)</span><span class="sxs-lookup"><span data-stu-id="77b7a-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="77b7a-955">Parametre</span><span class="sxs-lookup"><span data-stu-id="77b7a-955">Parameter</span></span>
-   <span data-ttu-id="77b7a-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="77b7a-956">CollectionType</span></span>

<span data-ttu-id="77b7a-957">**ReferenceType** öğesi, bir parametre veya dönüş türü bir işlev için tanımlarken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="77b7a-958">A **ReferenceType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-959">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-960">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-961">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-961">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-962">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ReferenceType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="77b7a-963">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-963">Attribute Name</span></span> | <span data-ttu-id="77b7a-964">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-964">Is Required</span></span> | <span data-ttu-id="77b7a-965">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="77b7a-966">**Türü**</span><span class="sxs-lookup"><span data-stu-id="77b7a-966">**Type**</span></span>       | <span data-ttu-id="77b7a-967">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-967">Yes</span></span>         | <span data-ttu-id="77b7a-968">Başvurulan varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-969">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReferenceType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="77b7a-970">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-971">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-972">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-972">Example</span></span>

<span data-ttu-id="77b7a-973">Aşağıdaki örnekte gösterildiği **ReferenceType** öğesi alt öğesi olarak kullanılan bir **parametre** başvuruyu kabul eden bir model tanımlı işlev öğesinde bir **kişi** varlık türü:</span><span class="sxs-lookup"><span data-stu-id="77b7a-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="77b7a-974">Aşağıdaki örnekte gösterildiği **ReferenceType** öğesi alt öğesi olarak kullanılan bir **ReturnType** (işlev) öğesi için bir başvuru döndürür model tanımlı bir işlevdeki bir **kişi**varlık türü:</span><span class="sxs-lookup"><span data-stu-id="77b7a-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="77b7a-975">Referentialconstraint'teki öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="77b7a-976">A **Referentialconstraint'teki** kavramsal şema tanım dili (CSDL) öğesinde bir başvuru bütünlüğü kısıtlaması ilişkisel bir veritabanındaki benzer işlevselliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="77b7a-977">Bir veritabanı tablosundan bir sütuna (veya sütun) başka bir tablonun birincil anahtarı başvurabilirsiniz aynı şekilde, başka bir varlık türünün Varlık anahtarı bir varlık türünün bir özelliği (veya Özellikler) başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="77b7a-978">Başvurulan varlık türü olarak adlandırılır *birincil ucu* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="77b7a-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="77b7a-979">Birincil ucu başvuran varlık türü olarak adlandırılan *bağımlı son* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="77b7a-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="77b7a-980">Bir varlık türü üzerinde sunulan bir yabancı anahtar üzerindeki başka bir varlık türü, bir özellik başvuruyorsa **Referentialconstraint'teki** öğe iki varlık türleri arasındaki ilişkiyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="77b7a-981">Çünkü **Referentialconstraint'teki** öğesi sağlar nasıl iki varlık türleri hakkında bilgi ilişkilidir, karşılık gelen hiçbir AssociationSetMapping öğe eşleme belirtimi dili (MSL) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="77b7a-982">Kullanıma sunulan bir yabancı anahtarları yoksa iki varlık türleri arasındaki ilişkiyi karşılık gelen olmalıdır **AssociationSetMapping** ilişkilendirme bilgileri veri kaynağına eşlemek için öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="77b7a-983">Bir varlık türünde, yabancı anahtar açık değilse **Referentialconstraint'teki** öğesi yalnızca bir varlık türü ve başka bir varlık türü arasında birincil anahtar birincil anahtar kısıtlaması tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="77b7a-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="77b7a-984">A **Referentialconstraint'teki** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-985">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-986">Asıl (tam olarak bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="77b7a-987">Bağımlı (tam olarak bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="77b7a-988">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-989">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-989">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-990">**Referentialconstraint'teki** öğesi herhangi bir sayıda ek açıklama öznitelikleri (özel XML öznitelikleri) olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="77b7a-991">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-992">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="77b7a-993">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-993">Example</span></span>

<span data-ttu-id="77b7a-994">Aşağıdaki örnekte gösterildiği bir **Referentialconstraint'teki** öğe tanımının bir parçası olarak kullanılan **yayınladığı** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="77b7a-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="77b7a-995">ReturnType (işlev) öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="77b7a-996">**ReturnType** kavramsal şema tanım dili (CSDL) (işlev) öğesinde bir Function öğesinde tanımlanan bir işlev için dönüş türü belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="77b7a-997">Bir işlevin dönüş türü de belirtilebilir bir **ReturnType** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="77b7a-998">Dönüş türleri herhangi olabilir **EdmSimpleType**, varlık türü, karmaşık tür, satır türü, başvuru türü veya bu tür bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="77b7a-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="77b7a-999">Bir işlevin dönüş türü ile belirtilebilir **türü** özniteliği **ReturnType** (işlev) öğesi veya şu alt öğelerden biri ile:</span><span class="sxs-lookup"><span data-stu-id="77b7a-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="77b7a-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="77b7a-1000">CollectionType</span></span>
-   <span data-ttu-id="77b7a-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="77b7a-1001">ReferenceType</span></span>
-   <span data-ttu-id="77b7a-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="77b7a-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-1003">İşlev dönüş türü ile her ikisini de belirtirseniz, bir model doğrulama değil **türü** özniteliği **ReturnType** (işlev) öğe ve alt öğelerden biri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-1004">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-1004">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-1005">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ReturnType** (işlev) öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="77b7a-1006">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-1006">Attribute Name</span></span> | <span data-ttu-id="77b7a-1007">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-1007">Is Required</span></span> | <span data-ttu-id="77b7a-1008">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="77b7a-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1009">**ReturnType**</span></span> | <span data-ttu-id="77b7a-1010">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1010">No</span></span>          | <span data-ttu-id="77b7a-1011">İşlev tarafından döndürülen tür.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-1012">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReturnType** (işlev) öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="77b7a-1013">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-1014">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-1015">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-1015">Example</span></span>

<span data-ttu-id="77b7a-1016">Aşağıdaki örnekte bir **işlevi** bir kitap, yazdırma rolünüzün yıl sayısını döndüren bir işlev tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="77b7a-1017">Dönüş türü tarafından belirtilen Not **türü** özniteliği bir **ReturnType** (işlev) öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="77b7a-1018">ReturnType (Functionımport) öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="77b7a-1019">**ReturnType** kavramsal şema tanım dili (CSDL) (Functionımport) öğesinde bir Functionımport öğesinde tanımlanan bir işlev için dönüş türü belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="77b7a-1020">Bir işlevin dönüş türü de belirtilebilir bir **ReturnType** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="77b7a-1021">Dönüş türleri varlık türü, karmaşık tür herhangi bir koleksiyon olabilir veya **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="77b7a-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="77b7a-1022">Bir işlevin dönüş türü belirtilmiş **türü** özniteliği **ReturnType** (Functionımport) öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-1023">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-1023">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-1024">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ReturnType** (Functionımport) öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="77b7a-1025">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-1025">Attribute Name</span></span> | <span data-ttu-id="77b7a-1026">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-1026">Is Required</span></span> | <span data-ttu-id="77b7a-1027">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-1028">**Türü**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1028">**Type**</span></span>       | <span data-ttu-id="77b7a-1029">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1029">No</span></span>          | <span data-ttu-id="77b7a-1030">İşlevinin döndürdüğü türü.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1030">The type that the function returns.</span></span> <span data-ttu-id="77b7a-1031">Değer, ComplexType, EntityType veya EDMSimpleType koleksiyonu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="77b7a-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1032">**EntitySet**</span></span>  | <span data-ttu-id="77b7a-1033">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1033">No</span></span>          | <span data-ttu-id="77b7a-1034">Varlık koleksiyonu işlevi döndürürse, türleri, değerini **EntitySet** koleksiyona ait olduğu varlık kümesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="77b7a-1035">Aksi takdirde, **EntitySet** özniteliği değil kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-1036">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReturnType** (Functionımport) öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="77b7a-1037">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-1038">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-1039">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-1039">Example</span></span>

<span data-ttu-id="77b7a-1040">Aşağıdaki örnekte bir **Functionımport** kitaplardan ve yayımcılar döndürür.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="77b7a-1041">İşlev iki sonucu kümesi ve bu nedenle iki döndürmediğine dikkat edin **ReturnType** (Functionımport) öğeleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="77b7a-1042">RowType öğesinin (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="77b7a-1043">A **RowType** kavramsal şema tanım dili (CSDL) öğesinde, bir parametre veya kavramsal modelde tanımlı bir işlevin dönüş türü olarak adlandırılmamış bir yapı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="77b7a-1044">A **RowType** aşağıdaki öğeleri alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="77b7a-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="77b7a-1045">CollectionType</span></span>
-   <span data-ttu-id="77b7a-1046">Parametre</span><span class="sxs-lookup"><span data-stu-id="77b7a-1046">Parameter</span></span>
-   <span data-ttu-id="77b7a-1047">ReturnType (işlev)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1047">ReturnType (Function)</span></span>

<span data-ttu-id="77b7a-1048">A **RowType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-1049">Özellik (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1049">Property (one or more)</span></span>
-   <span data-ttu-id="77b7a-1050">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-1051">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-1051">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-1052">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **RowType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="77b7a-1053">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-1054">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="77b7a-1055">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-1055">Example</span></span>

<span data-ttu-id="77b7a-1056">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğe (belirtilmiş **RowType** öğesi).</span><span class="sxs-lookup"><span data-stu-id="77b7a-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a><span data-ttu-id="77b7a-1057">Şema öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="77b7a-1058">**Şema** öğenin kavramsal model tanımı kök öğesidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="77b7a-1059">Bu nesneleri, işlevleri ve kavramsal bir modeli olun kapsayıcıları için tanımlar içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="77b7a-1060">**Şema** öğesi sıfır veya daha fazla şu alt öğelerden birini içerebilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="77b7a-1061">Kullanma</span><span class="sxs-lookup"><span data-stu-id="77b7a-1061">Using</span></span>
-   <span data-ttu-id="77b7a-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="77b7a-1062">EntityContainer</span></span>
-   <span data-ttu-id="77b7a-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="77b7a-1063">EntityType</span></span>
-   <span data-ttu-id="77b7a-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="77b7a-1064">EnumType</span></span>
-   <span data-ttu-id="77b7a-1065">İlişkilendirme</span><span class="sxs-lookup"><span data-stu-id="77b7a-1065">Association</span></span>
-   <span data-ttu-id="77b7a-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="77b7a-1066">ComplexType</span></span>
-   <span data-ttu-id="77b7a-1067">İşlev</span><span class="sxs-lookup"><span data-stu-id="77b7a-1067">Function</span></span>

<span data-ttu-id="77b7a-1068">A **şema** öğesi sıfır veya bir ek açıklama öğeleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-1069">**İşlevi** öğesi ve ek açıklama öğeleri yalnızca izin CSDL v2'de ve sonraki sürümler.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="77b7a-1070">**Şema** öğesini kullanan **Namespace** kavramsal modelde varlık türü, karmaşık türü ve ilişkisi nesneleri için ad alanını tanımlayan öznitelik.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="77b7a-1071">Ad alanı içinde hiçbir iki nesnenin aynı ada sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="77b7a-1072">Ad alanları, birden çok yayılabilir **şema** öğelerini ve birden çok .csdl dosyaları.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="77b7a-1073">Kavramsal model ad alanı XML ad alanından farklıdır **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="77b7a-1074">Kavramsal model ad alanı (tarafından tanımlandığı gibi **Namespace** özniteliği) ilişki türleri varlık türleri ve karmaşık türler için mantıksal bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="77b7a-1075">XML ad alanı (tarafından belirtilen **xmlns** özniteliği), bir **şema** öğedir alt öğeleri ve öznitelikleri için varsayılan ad alanı **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="77b7a-1076">XML ad alanları formun http://schemas.microsoft.com/ado/YYYY/MM/edm (burada YYYY ve MM yıl ve ay sırasıyla temsil eder) CSDL için ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="77b7a-1077">Bu forma sahip ad alanları, özel öğeleri ve öznitelikleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-1078">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-1078">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-1079">Aşağıdaki tabloda öznitelikleri açıklar uygulanabilir **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="77b7a-1080">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-1080">Attribute Name</span></span> | <span data-ttu-id="77b7a-1081">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-1081">Is Required</span></span> | <span data-ttu-id="77b7a-1082">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1083">**Namespace**</span></span>  | <span data-ttu-id="77b7a-1084">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1084">Yes</span></span>         | <span data-ttu-id="77b7a-1085">Kavramsal modelin ad alanı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="77b7a-1086">Değerini **Namespace** özniteliği, bir türün tam adı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="77b7a-1087">Örneğin, bir **EntityType** adlı *müşteri* Simple.Example.Model ad alanınıza ve sonra tam adı olduğunu **EntityType** olduğu SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="77b7a-1088">Aşağıdaki dize değeri olarak kullanılamaz **Namespace** özniteliği: **sistem**, **geçici**, veya **Edm**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="77b7a-1089">Değeri **Namespace** öznitelik değeri olarak aynı olamaz **Namespace** SSDL şeması öğesindeki özniteliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="77b7a-1090">**Diğer ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1090">**Alias**</span></span>      | <span data-ttu-id="77b7a-1091">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1091">No</span></span>          | <span data-ttu-id="77b7a-1092">Ad alanı adı yerine kullanılan tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="77b7a-1093">Örneğin, bir **EntityType** adlı *müşteri* Simple.Example.Model ad alanı ve değerini **diğer** özniteliği *modeli*, tam nitelikli adı olarak Model.Customer kullanabilirsiniz **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="77b7a-1094">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="77b7a-1095">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-1096">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-1097">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-1097">Example</span></span>

<span data-ttu-id="77b7a-1098">Aşağıdaki örnekte gösterildiği bir **şema** öğesini içeren bir **EntityContainer** öğesinde, iki **EntityType** öğeleri ve bir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="77b7a-1099">TypeRef öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="77b7a-1100">**TypeRef** kavramsal şema tanım dili (CSDL) öğesinde türü adlı varolan bir başvuru sağlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="77b7a-1101">**TypeRef** bir işlev bir parametre veya dönüş türü bir koleksiyon olduğunu belirtmek için kullanılan CollectionType öğesinin alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="77b7a-1102">A **TypeRef** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="77b7a-1103">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="77b7a-1104">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-1105">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-1105">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-1106">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **TypeRef** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="77b7a-1107">Unutmayın **DefaultValue**, **MaxLength**, **FixedLength**, **duyarlık**, **ölçek**,  **Unicode**, ve **harmanlama** öznitelikleri için uygun yalnızca **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="77b7a-1108">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="77b7a-1109">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-1109">Is Required</span></span> | <span data-ttu-id="77b7a-1110">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-1111">**Türü**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1111">**Type**</span></span>                                                           | <span data-ttu-id="77b7a-1112">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1112">No</span></span>          | <span data-ttu-id="77b7a-1113">Başvurulan tür adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="77b7a-1114">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="77b7a-1115">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1115">No</span></span>          | <span data-ttu-id="77b7a-1116">**Doğru** (varsayılan değer) veya **False** bağlı olup olmadığını özelliği null değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="77b7a-1117">> CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="77b7a-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="77b7a-1119">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1119">No</span></span>          | <span data-ttu-id="77b7a-1120">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="77b7a-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="77b7a-1122">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1122">No</span></span>          | <span data-ttu-id="77b7a-1123">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="77b7a-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="77b7a-1125">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1125">No</span></span>          | <span data-ttu-id="77b7a-1126">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="77b7a-1127">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1127">**Precision**</span></span>                                                      | <span data-ttu-id="77b7a-1128">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1128">No</span></span>          | <span data-ttu-id="77b7a-1129">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="77b7a-1130">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1130">**Scale**</span></span>                                                          | <span data-ttu-id="77b7a-1131">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1131">No</span></span>          | <span data-ttu-id="77b7a-1132">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="77b7a-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1133">**SRID**</span></span>                                                           | <span data-ttu-id="77b7a-1134">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1134">No</span></span>          | <span data-ttu-id="77b7a-1135">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="77b7a-1136">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="77b7a-1137">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="77b7a-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="77b7a-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="77b7a-1139">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1139">No</span></span>          | <span data-ttu-id="77b7a-1140">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="77b7a-1141">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1141">**Collation**</span></span>                                                      | <span data-ttu-id="77b7a-1142">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1142">No</span></span>          | <span data-ttu-id="77b7a-1143">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="77b7a-1144">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **CollectionType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="77b7a-1145">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-1146">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-1147">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-1147">Example</span></span>

<span data-ttu-id="77b7a-1148">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. **TypeRef** öğesi (alt öğesi olarak bir **CollectionType** öğesi) işlev koleksiyonunu kabul belirtmek için  **Departman** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="77b7a-1149">Öğesi (CSDL) kullanma</span><span class="sxs-lookup"><span data-stu-id="77b7a-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="77b7a-1150">**Kullanma** kavramsal şema tanım dili (CSDL) öğe farklı bir ad alanında bulunan bir kavramsal model içeriğini içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="77b7a-1151">Değerini ayarlayarak **Namespace** özniteliği başvurabilirsiniz varlık türleri ve karmaşık türler başka bir kavramsal modelde tanımlı ilişki türleri için.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="77b7a-1152">Birden fazla **kullanma** öğesi alt öğesi olabilir bir **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-1153">**Kullanma** CSDL öğesinde tıpkı çalışmaz bir **kullanarak** bir programlama dili deyimi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="77b7a-1154">Bir ad alanı ile alarak bir **kullanarak** deyimi bir programlama dili, özgün ad alanındaki nesneleri etkilemez.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="77b7a-1155">CSDL bir içeri aktarılan ad alanı özgün ad alanında bir varlık türünden türetilmiş bir varlık türü içerebilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="77b7a-1156">Bu, özgün ad alanında bildirilen varlık kümeleri etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="77b7a-1157">**Kullanma** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="77b7a-1158">Belgeleri (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="77b7a-1159">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="77b7a-1160">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="77b7a-1160">Applicable Attributes</span></span>

<span data-ttu-id="77b7a-1161">Aşağıdaki tabloda öznitelikleri açıklar uygulanabilir **kullanma** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="77b7a-1162">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="77b7a-1162">Attribute Name</span></span> | <span data-ttu-id="77b7a-1163">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="77b7a-1163">Is Required</span></span> | <span data-ttu-id="77b7a-1164">Değer</span><span class="sxs-lookup"><span data-stu-id="77b7a-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1165">**Namespace**</span></span>  | <span data-ttu-id="77b7a-1166">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1166">Yes</span></span>         | <span data-ttu-id="77b7a-1167">İçeri aktarılan ad alanının adı.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="77b7a-1168">**Diğer ad**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1168">**Alias**</span></span>      | <span data-ttu-id="77b7a-1169">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1169">Yes</span></span>         | <span data-ttu-id="77b7a-1170">Ad alanı adı yerine kullanılan tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="77b7a-1171">Bu öznitelik gerekli olsa da, bunu değil, ad alanı adı yerine nesne adlarını nitelemek için kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="77b7a-1172">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **kullanma** öğesi.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="77b7a-1173">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="77b7a-1174">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="77b7a-1175">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-1175">Example</span></span>

<span data-ttu-id="77b7a-1176">Aşağıdaki örnek, gösterir **kullanma** öğesi bir ad alanı içeri aktarmak için kullanılan başka bir yerde tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="77b7a-1177">Unutmayın ad alanı için **şema** gösterilen öğe `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="77b7a-1178">`Address` Özelliği `Publisher` **EntityType** tanımlanan karmaşık bir türdür `ExtendedBooksModel` ad alanı (içeri **kullanma** öğesi).</span><span class="sxs-lookup"><span data-stu-id="77b7a-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="77b7a-1179">Ek açıklama öznitelikleri (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="77b7a-1180">Ek açıklama özniteliklerinde kavramsal şema tanım dili (CSDL) kavramsal model özel XML öznitelikleri ' dir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="77b7a-1181">Geçerli XML yapısına sahip olmaya ek olarak, aşağıdaki ek açıklama özniteliklerinin doğru olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="77b7a-1182">Ek açıklama öznitelikleri CSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="77b7a-1183">Ek açıklama birden fazla öznitelik verilen bir CSDL öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="77b7a-1184">Her iki ek açıklama özniteliklerin tam adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="77b7a-1185">Ek açıklama öznitelikleri, kavramsal modeldeki öğeleri hakkında ek meta verilerini sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="77b7a-1186">Ek açıklama öğesinde bulunan meta veriler, çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="77b7a-1187">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-1187">Example</span></span>

<span data-ttu-id="77b7a-1188">Aşağıdaki örnekte gösterildiği bir **EntityType** bir ek açıklama özniteliği olan öğe (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="77b7a-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="77b7a-1189">Örnek ayrıca varlık türü öğeye uygulanan bir ek açıklama öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="77b7a-1190">Aşağıdaki kod ek açıklama özniteliği meta verilerini alır ve konsola yazar:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="77b7a-1191">Yukarıdaki kod olduğunu varsayar `School.csdl` dosya projenin çıkış dizinine ve aşağıdaki eklediğiniz `Imports` ve `Using` projenize ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="77b7a-1192">Ek açıklama öğelerinin (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="77b7a-1193">Özel XML öğeleri kavramsal modeldeki kavramsal şema tanım dili (CSDL) içinde ek açıklama öğeleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="77b7a-1194">Geçerli XML yapısına sahip olmaya ek olarak, aşağıdaki ek açıklama öğeleri doğru olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="77b7a-1195">Ek açıklama öğelerinin CSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="77b7a-1196">Birden çok ek açıklama öğesi, belirli bir CSDL öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="77b7a-1197">Her iki ek açıklama öğelerinin tam adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="77b7a-1198">Diğer tüm alt öğeleri verilen CSDL öğenin sonra ek açıklama öğelerinin görünmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="77b7a-1199">Ek açıklama öğelerinin kavramsal modeldeki öğeleri hakkında ek meta verilerini sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="77b7a-1200">.NET Framework sürüm 4 ile başlayarak, ek açıklama öğesinde bulunan meta veriler çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="77b7a-1201">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-1201">Example</span></span>

<span data-ttu-id="77b7a-1202">Aşağıdaki örnekte gösterildiği bir **EntityType** öğesi ile bir ek açıklama öğesi (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="77b7a-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="77b7a-1203">Örnek ayrıca varlık türü öğeye uygulanan bir ek açıklama özniteliği gösterir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="77b7a-1204">Aşağıdaki kod, ek açıklama öğesinde bulunan meta verileri alır ve konsola yazar:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="77b7a-1205">Yukarıdaki kod School.csdl dosya projenin çıkış dizinine olduğunu ve aşağıdaki eklediğinizi varsayar `Imports` ve `Using` projenize ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="77b7a-1206">Kavramsal Model türleri (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="77b7a-1207">Kavramsal şema tanım dili (CSDL) adı verilen soyut temel veri türleri kümesi destekler **EDMSimpleTypes**, kavramsal modelde özellikleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="77b7a-1208">**EDMSimpleTypes** proxy'ler ilgili daha fazla bilgi için depolama veya barındırma ortamında desteklenen temel veri türleri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="77b7a-1209">Aşağıdaki tabloda, CSDL tarafından desteklenen temel veri türlerini listeler.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="77b7a-1210">Tabloda ayrıca her uygulanan özellikleri listeler **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="77b7a-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="77b7a-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="77b7a-1212">Açıklama</span><span class="sxs-lookup"><span data-stu-id="77b7a-1212">Description</span></span>                                                | <span data-ttu-id="77b7a-1213">Geçerli modelleri</span><span class="sxs-lookup"><span data-stu-id="77b7a-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="77b7a-1214">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="77b7a-1215">İkili veriler içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="77b7a-1216">MaxLength, FixedLength, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="77b7a-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="77b7a-1218">Değeri içeren **true** veya **false**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="77b7a-1219">Boş değer atanabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="77b7a-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="77b7a-1221">İmzalanmamış 8 bit tam sayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="77b7a-1222">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1223">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="77b7a-1224">Tarih ve saati temsil eder.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="77b7a-1225">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="77b7a-1227">Bir tarih ve saat olarak GMT'den dakikalar içinde bir uzaklık içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="77b7a-1228">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="77b7a-1230">Sabit kesinlik ve ölçek ile sayısal bir değer içeriyor.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="77b7a-1231">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="77b7a-1233">Kayan nokta ile 15 basamaklı duyarlık sayı içerir</span><span class="sxs-lookup"><span data-stu-id="77b7a-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="77b7a-1234">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="77b7a-1236">Kayan noktalı sayı 7 basamaklı duyarlık içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="77b7a-1237">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1238">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="77b7a-1239">16 baytlık benzersiz bir tanımlayıcı içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="77b7a-1240">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="77b7a-1242">İşaretli 16 bit tam sayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="77b7a-1243">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1244">**EDM.Int32**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="77b7a-1245">İşaretli 32-bit tamsayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="77b7a-1246">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1247">**EDM.Int64**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="77b7a-1248">Bir 64-bit işaretli tamsayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="77b7a-1249">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="77b7a-1251">İşaretli 8 bit tam sayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="77b7a-1252">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1253">**Edm.String**</span></span>                   | <span data-ttu-id="77b7a-1254">Karakter verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1254">Contains character data.</span></span>                                   | <span data-ttu-id="77b7a-1255">Varsayılan Unicode, FixedLength, MaxLength, harmanlaması, boş değer atanabilir, duyarlık</span><span class="sxs-lookup"><span data-stu-id="77b7a-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="77b7a-1256">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="77b7a-1257">Günün bir saati içerir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="77b7a-1258">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="77b7a-1259">**Edm.Geography**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="77b7a-1260">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="77b7a-1262">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="77b7a-1264">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1265">**Edm.GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="77b7a-1266">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="77b7a-1268">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="77b7a-1270">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="77b7a-1272">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="77b7a-1274">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1275">**Edm.Geometry**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="77b7a-1276">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="77b7a-1278">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="77b7a-1280">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="77b7a-1282">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="77b7a-1284">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="77b7a-1286">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="77b7a-1288">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="77b7a-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="77b7a-1290">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="77b7a-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="77b7a-1291">Modelleri (CSDL)</span><span class="sxs-lookup"><span data-stu-id="77b7a-1291">Facets (CSDL)</span></span>

<span data-ttu-id="77b7a-1292">Kavramsal şema tanım dili (CSDL), modelleri özelliklerini varlık türlerinde ve karmaşık türlerde kısıtlamaları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="77b7a-1293">Modelleri, XML öznitelikleri aşağıdaki CSDL öğeleri olarak görünür:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="77b7a-1294">Özellik</span><span class="sxs-lookup"><span data-stu-id="77b7a-1294">Property</span></span>
-   <span data-ttu-id="77b7a-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="77b7a-1295">TypeRef</span></span>
-   <span data-ttu-id="77b7a-1296">Parametre</span><span class="sxs-lookup"><span data-stu-id="77b7a-1296">Parameter</span></span>

<span data-ttu-id="77b7a-1297">Aşağıdaki tabloda CSDL desteklenen özellikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="77b7a-1298">Tüm özellikleri isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1298">All facets are optional.</span></span> <span data-ttu-id="77b7a-1299">Aşağıda listelenen bazı modeller bir kavramsal modelde bir veritabanı oluşturma varlık çerçevesi tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="77b7a-1300">Kavramsal modelde veri türleri hakkında daha fazla bilgi için bkz: kavramsal Model türleri (CSDL).</span><span class="sxs-lookup"><span data-stu-id="77b7a-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="77b7a-1301">Modeli</span><span class="sxs-lookup"><span data-stu-id="77b7a-1301">Facet</span></span>               | <span data-ttu-id="77b7a-1302">Açıklama</span><span class="sxs-lookup"><span data-stu-id="77b7a-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="77b7a-1303">Uygulandığı öğe:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="77b7a-1304">Veritabanı oluşturmak için kullanılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1304">Used for the database generation</span></span> | <span data-ttu-id="77b7a-1305">Çalışma zamanı tarafından kullanılan</span><span class="sxs-lookup"><span data-stu-id="77b7a-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="77b7a-1306">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1306">**Collation**</span></span>       | <span data-ttu-id="77b7a-1307">Harmanlama dizisi (veya sıralama) yapılırken kullanılacak karşılaştırma gerçekleştirme ve özellik değerleri üzerinde işlem sıralama belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="77b7a-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="77b7a-1309">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1309">Yes</span></span>                              | <span data-ttu-id="77b7a-1310">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1310">No</span></span>                  |
| <span data-ttu-id="77b7a-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="77b7a-1312">Özelliğinin değeri için iyimser eşzamanlılık denetimlerinin kullanılması gerektiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="77b7a-1313">Tüm **EDMSimpleType** özellikleri</span><span class="sxs-lookup"><span data-stu-id="77b7a-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="77b7a-1314">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1314">No</span></span>                               | <span data-ttu-id="77b7a-1315">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1315">Yes</span></span>                 |
| <span data-ttu-id="77b7a-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1316">**Default**</span></span>         | <span data-ttu-id="77b7a-1317">Örnek oluşturma sırasında herhangi bir değer sağlanmazsa, özelliğin varsayılan değerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="77b7a-1318">Tüm **EDMSimpleType** özellikleri</span><span class="sxs-lookup"><span data-stu-id="77b7a-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="77b7a-1319">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1319">Yes</span></span>                              | <span data-ttu-id="77b7a-1320">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1320">Yes</span></span>                 |
| <span data-ttu-id="77b7a-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1321">**FixedLength**</span></span>     | <span data-ttu-id="77b7a-1322">Özellik değerinin uzunluğu değişebilir olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="77b7a-1323">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="77b7a-1324">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1324">Yes</span></span>                              | <span data-ttu-id="77b7a-1325">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1325">No</span></span>                  |
| <span data-ttu-id="77b7a-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1326">**MaxLength**</span></span>       | <span data-ttu-id="77b7a-1327">Özellik değeri en büyük uzunluğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="77b7a-1328">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="77b7a-1329">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1329">Yes</span></span>                              | <span data-ttu-id="77b7a-1330">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1330">No</span></span>                  |
| <span data-ttu-id="77b7a-1331">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1331">**Nullable**</span></span>        | <span data-ttu-id="77b7a-1332">Özelliğine sahip olup olmadığını belirten bir **null** değeri.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="77b7a-1333">Tüm **EDMSimpleType** özellikleri</span><span class="sxs-lookup"><span data-stu-id="77b7a-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="77b7a-1334">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1334">Yes</span></span>                              | <span data-ttu-id="77b7a-1335">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1335">Yes</span></span>                 |
| <span data-ttu-id="77b7a-1336">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1336">**Precision**</span></span>       | <span data-ttu-id="77b7a-1337">Tür özellikleri için **ondalık**, bir özellik değeri olabilir basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="77b7a-1338">Tür özellikleri için **zaman**, **DateTime**, ve **DateTimeOffset**, özellik değerinin saniye kesirli kısmını için basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="77b7a-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="77b7a-1340">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1340">Yes</span></span>                              | <span data-ttu-id="77b7a-1341">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1341">No</span></span>                  |
| <span data-ttu-id="77b7a-1342">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1342">**Scale**</span></span>           | <span data-ttu-id="77b7a-1343">Özellik değeri ondalık noktasının sağındaki basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="77b7a-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="77b7a-1345">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1345">Yes</span></span>                              | <span data-ttu-id="77b7a-1346">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1346">No</span></span>                  |
| <span data-ttu-id="77b7a-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1347">**SRID**</span></span>            | <span data-ttu-id="77b7a-1348">Uzamsal sistem başvuru sistemi kimliğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="77b7a-1349">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="77b7a-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="77b7a-1350">**Edm.Geography Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="77b7a-1351">Hayır</span><span class="sxs-lookup"><span data-stu-id="77b7a-1351">No</span></span>                               | <span data-ttu-id="77b7a-1352">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1352">Yes</span></span>                 |
| <span data-ttu-id="77b7a-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1353">**Unicode**</span></span>         | <span data-ttu-id="77b7a-1354">Özellik değeri Unicode olarak mi depolanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="77b7a-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="77b7a-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="77b7a-1356">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1356">Yes</span></span>                              | <span data-ttu-id="77b7a-1357">Evet</span><span class="sxs-lookup"><span data-stu-id="77b7a-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="77b7a-1358">Veritabanı Oluştur Sihirbazı'nı bir veritabanı kavramsal bir modeli oluşturulurken değerini tanıyacağınız **StoreGeneratedPattern** özniteliği bir **özelliği** aşağıdaki ise öğe ad alanı: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="77b7a-1359">Özniteliği için desteklenen değerler şunlardır: **kimlik** ve **hesaplanan**.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="77b7a-1360">Değerini **kimlik** veritabanında oluşturulan bir kimlik değeri olan bir veritabanı sütunu üretecektir.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="77b7a-1361">Değerini **hesaplanan** veritabanında hesaplanan bir değere sahip bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="77b7a-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="77b7a-1362">Örnek</span><span class="sxs-lookup"><span data-stu-id="77b7a-1362">Example</span></span>

<span data-ttu-id="77b7a-1363">Aşağıdaki örnek, bir varlık türünün özelliklerine uygulanan özellikleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="77b7a-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
