---
title: CSDL belirtimi - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: f5bf0dc75a8195e9af979c9e044f36171f46c9b7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490525"
---
# <a name="csdl-specification"></a><span data-ttu-id="57253-102">CSDL belirtimi</span><span class="sxs-lookup"><span data-stu-id="57253-102">CSDL Specification</span></span>
<span data-ttu-id="57253-103">Kavramsal şema tanım dili (CSDL) varlıklar, ilişkileri ve kavramsal bir modeli verilerle bir uygulama olun işlevlerini açıklayan bir XML tabanlı bir dilidir.</span><span class="sxs-lookup"><span data-stu-id="57253-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="57253-104">Bu kavramsal model Entity Framework veya WCF Veri Hizmetleri tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="57253-105">CSDL ile açıklanan meta veri varlıkları ve bir veri kaynağına kavramsal modelde tanımlı ilişkiler eşlemek için Entity Framework tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="57253-106">Daha fazla bilgi için [SSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) ve [MSL belirtimi](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="57253-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="57253-107">CSDL, varlık veri modeli, Entity Framework'ün uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="57253-108">Bir Entity Framework uygulamasında, kavramsal model meta verilerini (CSDL içinde yazılan) .csdl dosyasından System.Data.Metadata.Edm.EdmItemCollection bir örneğine yüklenir ve yöntemleri kullanılarak erişilebilir System.Data.Metadata.Edm.MetadataWorkspace sınıfı.</span><span class="sxs-lookup"><span data-stu-id="57253-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="57253-109">Varlık çerçevesi kavramsal model meta veri kaynağına özgü komutlar kavramsal modeline karşı sorgular çevirmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="57253-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="57253-110">EF Designer, tasarım zamanında bir .edmx dosyası içinde kavramsal model bilgileri depolar.</span><span class="sxs-lookup"><span data-stu-id="57253-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="57253-111">Oluşturma zamanında EF Designer Entity Framework tarafından çalışma zamanında gereken .csdl dosyası oluşturmak için bir .edmx dosyası içinde bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="57253-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="57253-112">CSDL sürümleri, XML ad alanları tarafından ayrılır.</span><span class="sxs-lookup"><span data-stu-id="57253-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="57253-113">CSDL sürümü</span><span class="sxs-lookup"><span data-stu-id="57253-113">CSDL Version</span></span> | <span data-ttu-id="57253-114">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="57253-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="57253-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="57253-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="57253-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="57253-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="57253-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="57253-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="57253-118">Association öğesinde (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-118">Association Element (CSDL)</span></span>

<span data-ttu-id="57253-119">Bir **ilişkilendirme** öğe iki varlık türleri arasındaki bir ilişkiyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="57253-120">İlişkilendirmesine katılan varlık türleri ve varlık türleri çeşitliliği bilinen ilişkinin her iki ucunda olası sayısını belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="57253-121">Bir ilişkilendirme end'ün çoğulluğunun bir değer bir (1) sıfır veya bir (0..1) ya da birden çok olabilir (\*).</span><span class="sxs-lookup"><span data-stu-id="57253-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="57253-122">Bu bilgiler, iki alt son öğe belirtilir.</span><span class="sxs-lookup"><span data-stu-id="57253-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="57253-123">Bir varlık türünde gösteriliyorsa varlık türü örneklerinin bir ilişkilendirmenin bir ucunda Gezinti özellikleri veya yabancı anahtarlar erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="57253-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="57253-124">Bir uygulamada belirli bir ilişki varlık türleri örnekleri arasında bir ilişki örneğini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="57253-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="57253-125">İlişkilendirme örnekleri mantıksal olarak bir ilişki kümesi içinde gruplandırılır.</span><span class="sxs-lookup"><span data-stu-id="57253-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="57253-126">Bir **ilişkilendirme** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-127">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-128">Bitiş (tam olarak 2 öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="57253-129">Referentialconstraint'teki (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="57253-130">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-131">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-131">Applicable Attributes</span></span>

<span data-ttu-id="57253-132">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="57253-133">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-133">Attribute Name</span></span> | <span data-ttu-id="57253-134">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-134">Is Required</span></span> | <span data-ttu-id="57253-135">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="57253-136">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-136">**Name**</span></span>       | <span data-ttu-id="57253-137">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-137">Yes</span></span>         | <span data-ttu-id="57253-138">İlişkilendirmenin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-139">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="57253-140">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-141">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-142">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-142">Example</span></span>

<span data-ttu-id="57253-143">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** yabancı anahtarlar üzerinde karşılaştıklarını değil, ilişki **müşteri** ve  **Sipariş** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="57253-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="57253-144">**Çoğulluk** değerler her **son** ilişkisini gösteren diğer birçok **siparişler** ile ilişkili bir **müşteri**, ancak yalnızca bir **müşteri** ile ilişkili bir **sipariş**.</span><span class="sxs-lookup"><span data-stu-id="57253-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="57253-145">Ayrıca, **OnDelete** öğesi gösterir tüm **siparişler** belirli bir ilgili **müşteri** ve içine yüklenen ObjectContext silinecek varsa **müşteri** silinir.</span><span class="sxs-lookup"><span data-stu-id="57253-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="57253-146">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** yabancı anahtarlar üzerinde kullanıma sunması olduğunda ilişkilendirme **müşteri** ve  **Sipariş** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="57253-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="57253-147">Kullanıma sunulan yabancı anahtarlar ile varlıklar arasında ilişki ile yönetilen bir **Referentialconstraint'teki** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="57253-148">Karşılık gelen bir AssociationSetMapping öğesi bu ilişkiyi veri kaynağına eşlemek gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="57253-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="57253-149">AssociationSet öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="57253-150">**AssociationSet** kavramsal şema tanım dili (CSDL) öğesinde, aynı türün ilişkilendirme örnekleri için mantıksal bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="57253-151">Bir veri kaynağına eşlenebilecek bir gruplandırma ilişkilendirme örnekleri için bir tanımı bir ilişki kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="57253-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="57253-152">**AssociationSet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-153">Belgeleri (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="57253-154">Bitiş (tam olarak iki öğe gerekli)</span><span class="sxs-lookup"><span data-stu-id="57253-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="57253-155">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="57253-156">**İlişkilendirme** özniteliği bir ilişki kümesi içeren bir ilişki türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="57253-157">Bir ilişki kümesi sonunu yapmak varlık kümeleri ile tam olarak iki alt belirtilen **son** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="57253-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-158">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-158">Applicable Attributes</span></span>

<span data-ttu-id="57253-159">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="57253-160">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-160">Attribute Name</span></span>  | <span data-ttu-id="57253-161">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-161">Is Required</span></span> | <span data-ttu-id="57253-162">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-163">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-163">**Name**</span></span>        | <span data-ttu-id="57253-164">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-164">Yes</span></span>         | <span data-ttu-id="57253-165">Varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-165">The name of the entity set.</span></span> <span data-ttu-id="57253-166">Değerini **adı** öznitelik değeri ile aynı olamaz **ilişkilendirme** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57253-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="57253-167">**İlişkilendirme**</span><span class="sxs-lookup"><span data-stu-id="57253-167">**Association**</span></span> | <span data-ttu-id="57253-168">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-168">Yes</span></span>         | <span data-ttu-id="57253-169">İlişkilendirme ayarlanmış ilişkilendirme tam olarak nitelenmiş adını örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="57253-170">İlişkilendirme ilişki kümesi ile aynı ad alanında olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-171">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="57253-172">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-173">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-174">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-174">Example</span></span>

<span data-ttu-id="57253-175">Aşağıdaki örnekte gösterildiği bir **EntityContainer** iki öğe **AssociationSet** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="57253-176">CollectionType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="57253-177">**CollectionType** kavramsal şema tanım dili (CSDL) öğesinde bir işlev parametresi veya işlevin dönüş türü, bir koleksiyon olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="57253-178">**CollectionType** parametresi ReturnType (işlev) öğesi veya alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="57253-179">Toplama türünü kullanarak belirtilebilir **türü** özniteliği veya şu alt öğelerden biri:</span><span class="sxs-lookup"><span data-stu-id="57253-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="57253-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="57253-180">**CollectionType**</span></span>
-   <span data-ttu-id="57253-181">referenceType</span><span class="sxs-lookup"><span data-stu-id="57253-181">ReferenceType</span></span>
-   <span data-ttu-id="57253-182">RowType</span><span class="sxs-lookup"><span data-stu-id="57253-182">RowType</span></span>
-   <span data-ttu-id="57253-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="57253-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="57253-184">Bir koleksiyonun türü ile her ikisi de belirtilirse model doğrulama değil **türü** özniteliğini ve bir alt öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="57253-185">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-185">Applicable Attributes</span></span>

<span data-ttu-id="57253-186">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **CollectionType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="57253-187">Unutmayın **DefaultValue**, **MaxLength**, **FixedLength**, **duyarlık**, **ölçek**,  **Unicode**, ve **harmanlama** öznitelikleri koleksiyonlarına geçerli yalnızca **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="57253-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="57253-188">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-188">Attribute Name</span></span>                                                          | <span data-ttu-id="57253-189">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-189">Is Required</span></span> | <span data-ttu-id="57253-190">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-191">**Türü**</span><span class="sxs-lookup"><span data-stu-id="57253-191">**Type**</span></span>                                                                | <span data-ttu-id="57253-192">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-192">No</span></span>          | <span data-ttu-id="57253-193">Koleksiyonun türü.</span><span class="sxs-lookup"><span data-stu-id="57253-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="57253-194">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="57253-194">**Nullable**</span></span>                                                            | <span data-ttu-id="57253-195">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-195">No</span></span>          | <span data-ttu-id="57253-196">**Doğru** (varsayılan değer) veya **False** bağlı olup olmadığını özelliği null değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="57253-197">> CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="57253-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="57253-198">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="57253-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="57253-199">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-199">No</span></span>          | <span data-ttu-id="57253-200">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="57253-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="57253-201">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="57253-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="57253-202">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-202">No</span></span>          | <span data-ttu-id="57253-203">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="57253-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="57253-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="57253-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="57253-205">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-205">No</span></span>          | <span data-ttu-id="57253-206">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="57253-207">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="57253-207">**Precision**</span></span>                                                           | <span data-ttu-id="57253-208">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-208">No</span></span>          | <span data-ttu-id="57253-209">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="57253-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="57253-210">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="57253-210">**Scale**</span></span>                                                               | <span data-ttu-id="57253-211">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-211">No</span></span>          | <span data-ttu-id="57253-212">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="57253-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="57253-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="57253-213">**SRID**</span></span>                                                                | <span data-ttu-id="57253-214">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-214">No</span></span>          | <span data-ttu-id="57253-215">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="57253-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="57253-216">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="57253-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="57253-217">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="57253-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="57253-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="57253-218">**Unicode**</span></span>                                                             | <span data-ttu-id="57253-219">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-219">No</span></span>          | <span data-ttu-id="57253-220">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="57253-221">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="57253-221">**Collation**</span></span>                                                           | <span data-ttu-id="57253-222">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-222">No</span></span>          | <span data-ttu-id="57253-223">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="57253-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="57253-224">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **CollectionType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="57253-225">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-226">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-227">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-227">Example</span></span>

<span data-ttu-id="57253-228">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. bir **CollectionType** işlevi bir koleksiyonu döndürdüğünü belirtmek için öğe **kişi** varlık türleri ( ilebelirtilen**ElementType** özniteliği).</span><span class="sxs-lookup"><span data-stu-id="57253-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="57253-229">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğe (belirtilmiş **RowType** öğesi).</span><span class="sxs-lookup"><span data-stu-id="57253-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="57253-230">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. **CollectionType** işlevi parametre olarak bir koleksiyonunu kabul belirtmek için öğe **departmanı** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="57253-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="57253-231">ComplexType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="57253-232">A **ComplexType** öğe tanımlar oluşan bir veri yapısı **EdmSimpleType** özellikleri veya diğer karmaşık türler.</span><span class="sxs-lookup"><span data-stu-id="57253-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="57253-233">Bir karmaşık türü bir varlık türünün veya başka bir karmaşık türü bir özelliği olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="57253-234">Verileri bir karmaşık tür tanımlar, bir karmaşık türü bir varlık türüne benzerdir.</span><span class="sxs-lookup"><span data-stu-id="57253-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="57253-235">Ancak, karmaşık türlerden ve varlık türleri arasındaki bazı temel farklar vardır:</span><span class="sxs-lookup"><span data-stu-id="57253-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="57253-236">Karmaşık türler kimlikleri (veya anahtarlara) yoksa ve bu nedenle bağımsız olarak var olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="57253-237">Karmaşık türler yalnızca varlık türleri veya diğer karmaşık türler özellikleri olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="57253-238">Karmaşık türler ilişkilendirmeler katılamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="57253-239">Ne bir ilişki sonu bir karmaşık türü olabilir ve bu nedenle karmaşık türler için Gezinti özellikleri tanımlanamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="57253-240">Her bir karmaşık tür skaler özellikleri ayarlanabilir ancak bir karmaşık tür özelliği bir null değer olamaz null.</span><span class="sxs-lookup"><span data-stu-id="57253-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="57253-241">A **ComplexType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-242">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-243">Özellik (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="57253-244">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="57253-245">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ComplexType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="57253-246">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="57253-247">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-247">Is Required</span></span> | <span data-ttu-id="57253-248">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-249">Ad</span><span class="sxs-lookup"><span data-stu-id="57253-249">Name</span></span>                                                                                                           | <span data-ttu-id="57253-250">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-250">Yes</span></span>         | <span data-ttu-id="57253-251">Karmaşık tür adı.</span><span class="sxs-lookup"><span data-stu-id="57253-251">The name of the complex type.</span></span> <span data-ttu-id="57253-252">Karmaşık bir tür adını başka bir ad ile aynı olamaz karmaşık tür, varlık türünün veya model kapsamında olan ilişkisi.</span><span class="sxs-lookup"><span data-stu-id="57253-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="57253-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="57253-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="57253-254">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-254">No</span></span>          | <span data-ttu-id="57253-255">Tanımlanmakta olan karmaşık türün temel türü başka bir karmaşık tür adı.</span><span class="sxs-lookup"><span data-stu-id="57253-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="57253-256">> Bu öznitelik CSDL v1'de geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="57253-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="57253-257">Karmaşık türleri için devralma, bu sürümde desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="57253-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="57253-258">Özet</span><span class="sxs-lookup"><span data-stu-id="57253-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="57253-259">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-259">No</span></span>          | <span data-ttu-id="57253-260">**Doğru** veya **False** (varsayılan değer) karmaşık türü soyut bir tür olup olmamasına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="57253-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="57253-261">> Bu öznitelik CSDL v1'de geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="57253-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="57253-262">Karmaşık türler bu sürümde, soyut türlerin olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="57253-263">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ComplexType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="57253-264">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-265">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-266">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-266">Example</span></span>

<span data-ttu-id="57253-267">Aşağıdaki örnek, bir karmaşık tür gösterir **adresi**, ile **EdmSimpleType** özellikleri **StreetAddress**, **Şehir**,  **Eyaletveİl**, **Ülke**, ve **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="57253-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="57253-268">Karmaşık tür tanımlamak için **adresi** (yukarıda) bir varlık türünün bir özellik olarak, özellik türü varlık tür tanımında bildirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="57253-269">Aşağıdaki örnekte gösterildiği **adresi** özelliği üzerinde bir varlık türü karmaşık bir tür olarak (**yayımcı**):</span><span class="sxs-lookup"><span data-stu-id="57253-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="57253-270">DefiningExpression öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="57253-271">**DefiningExpression** kavramsal şema tanım dili (CSDL) öğesinde kavramsal modelde bir işlevi tanımlayan bir varlık SQL ifadesi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="57253-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="57253-272">Doğrulama amacıyla bir **DefiningExpression** rasgele içerik öğesi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="57253-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="57253-273">Ancak, Entity Framework bağlanamazsa özel durum çalışma zamanında bir **DefiningExpression** öğesi geçerli varlık SQL içermiyor.</span><span class="sxs-lookup"><span data-stu-id="57253-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="57253-274">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-274">Applicable Attributes</span></span>

<span data-ttu-id="57253-275">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **DefiningExpression** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="57253-276">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-277">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="57253-278">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-278">Example</span></span>

<span data-ttu-id="57253-279">Aşağıdaki örnekte bir **DefiningExpression** bir kitap yayımlandığı tarihten sonra geçen yıl sayısını döndüren bir işlev tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="57253-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="57253-280">İçeriği **DefiningExpression** öğesi varlık SQL yazılır.</span><span class="sxs-lookup"><span data-stu-id="57253-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="57253-281">Bağımlı öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="57253-282">**Bağımlı** kavramsal şema tanım dili (CSDL) öğesinde Referentialconstraint'teki öğesi bir alt öğesidir ve başvuru kısıtlamasını bağımlı sonuna tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="57253-283">A **Referentialconstraint'teki** öğe ilişkisel bir veritabanındaki bir başvuru bütünlüğü kısıtlaması benzer işlevselliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="57253-284">Bir veritabanı tablosundan bir sütuna (veya sütun) başka bir tablonun birincil anahtarı başvurabilirsiniz aynı şekilde, başka bir varlık türünün Varlık anahtarı bir varlık türünün bir özelliği (veya Özellikler) başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="57253-285">Başvurulan varlık türü olarak adlandırılır *birincil ucu* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="57253-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="57253-286">Birincil ucu başvuran varlık türü olarak adlandırılan *bağımlı son* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="57253-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="57253-287">**PropertyRef** öğeleri birincil ucu hangi anahtarları başvuru belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="57253-288">**Bağımlı** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-289">PropertyRef (bir veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="57253-290">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-291">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-291">Applicable Attributes</span></span>

<span data-ttu-id="57253-292">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **bağımlı** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="57253-293">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-293">Attribute Name</span></span> | <span data-ttu-id="57253-294">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-294">Is Required</span></span> | <span data-ttu-id="57253-295">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="57253-296">**Rol**</span><span class="sxs-lookup"><span data-stu-id="57253-296">**Role**</span></span>       | <span data-ttu-id="57253-297">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-297">Yes</span></span>         | <span data-ttu-id="57253-298">Varlık türü bağımlı ucundaki ilişkinin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-299">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **bağımlı** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="57253-300">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-301">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-302">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-302">Example</span></span>

<span data-ttu-id="57253-303">Aşağıdaki örnekte gösterildiği bir **Referentialconstraint'teki** öğe tanımının bir parçası olarak kullanılan **yayınladığı** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="57253-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="57253-304">**Publisherıd** özelliği **kitap** varlık türü başvuru kısıtlamasını bağımlı sonuna yapar.</span><span class="sxs-lookup"><span data-stu-id="57253-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="57253-305">Belge öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="57253-306">**Belgeleri** kavramsal şema tanım dili (CSDL) öğesinde, bir üst öğe içinde tanımlanan bir nesneyle ilgili bilgileri sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="57253-307">Bir .edmx dosyası içinde olduğunda **belgeleri** öğesi EF Designer (örneğin, bir varlık, ilişkilendirme veya özellik), tasarım yüzeyindeki bir nesnenin içeriğini görünür bir öğenin alt öğesi olan **belgeleri**  öğesi Visual Studio'da görünür **özellikleri** pencere nesnesi için.</span><span class="sxs-lookup"><span data-stu-id="57253-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="57253-308">**Belgeleri** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-309">**Özet**: üst öğenin kısa bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="57253-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="57253-310">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-310">(zero or one element)</span></span>
-   <span data-ttu-id="57253-311">**LongDescription**: üst öğenin kapsamlı bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="57253-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="57253-312">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-312">(zero or one element)</span></span>
-   <span data-ttu-id="57253-313">Ek açıklama öğeleri.</span><span class="sxs-lookup"><span data-stu-id="57253-313">Annotation elements.</span></span> <span data-ttu-id="57253-314">(sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-315">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-315">Applicable Attributes</span></span>

<span data-ttu-id="57253-316">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **belgeleri** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="57253-317">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-318">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="57253-319">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-319">Example</span></span>

<span data-ttu-id="57253-320">Aşağıdaki örnekte gösterildiği **belgeleri** EntityType öğesinin alt öğesi olarak öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="57253-321">CSDL aşağıdaki kod parçacığında bir .edmx içerik, dosya, içeriğini **özeti** ve **LongDescription** öğeleri, Visual Studio'da görüneceği **özellikleri** tıkladığınızda penceresi `Customer` varlık türü.</span><span class="sxs-lookup"><span data-stu-id="57253-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="57253-322">Bitiş öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-322">End Element (CSDL)</span></span>

<span data-ttu-id="57253-323">**Son** kavramsal şema tanım dili (CSDL) öğesinde Association öğesinde veya AssociationSet bir öğenin bir alt olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="57253-324">Her durumda, rolü **son** öğe farklı ve uygun öznitelikler farklıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="57253-325">Bir ilişkilendirme öğesinin alt öğesi olarak bitiş öğesi</span><span class="sxs-lookup"><span data-stu-id="57253-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="57253-326">Bir **son** öğesi (alt öğesi olarak **ilişkilendirme** öğesi) bir ilişki sonu ve bu ilişkilendirmeyi sonunda bulunabilir varlık türü örneklerinin varlık türü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="57253-327">İlişkilendirme ucu ilişkilendirme bir parçası olarak tanımlanır; bir ilişkiyi tam olarak iki ilişkilendirme ucu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="57253-328">Bir varlık türünde gösteriliyorsa varlık türü örneklerinin bir ilişkilendirmenin bir ucunda Gezinti özellikleri veya yabancı anahtarlar erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="57253-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="57253-329">Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-330">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-331">OnDelete (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="57253-332">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="57253-333">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-333">Applicable Attributes</span></span>

<span data-ttu-id="57253-334">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="57253-335">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-335">Attribute Name</span></span>   | <span data-ttu-id="57253-336">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-336">Is Required</span></span> | <span data-ttu-id="57253-337">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-338">**Türü**</span><span class="sxs-lookup"><span data-stu-id="57253-338">**Type**</span></span>         | <span data-ttu-id="57253-339">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-339">Yes</span></span>         | <span data-ttu-id="57253-340">İlişkilendirmenin bir ucunda varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="57253-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="57253-341">**Rol**</span><span class="sxs-lookup"><span data-stu-id="57253-341">**Role**</span></span>         | <span data-ttu-id="57253-342">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-342">No</span></span>          | <span data-ttu-id="57253-343">İlişki sonu için bir ad.</span><span class="sxs-lookup"><span data-stu-id="57253-343">A name for the association end.</span></span> <span data-ttu-id="57253-344">Adı sağlanmazsa, ilişki uç varlık türünün adı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="57253-345">**Çokluk**</span><span class="sxs-lookup"><span data-stu-id="57253-345">**Multiplicity**</span></span> | <span data-ttu-id="57253-346">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-346">Yes</span></span>         | <span data-ttu-id="57253-347">**1**, **0..1**, veya **\*** ilişkilendirme sonunda olabilir bir varlık türü örneklerinin sayısına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="57253-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="57253-348">**1** bu tam olarak bir varlık türü örneği var ilişkisi sonuna belirten.</span><span class="sxs-lookup"><span data-stu-id="57253-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="57253-349">**0..1** sıfır veya bir varlık türü örneklerini ilişkilendirme sonunda bulunduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="57253-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="57253-350">**\*** sıfır, bir veya daha fazla varlık türü örneklerini ilişkilendirme sonunda bulunduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="57253-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-351">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="57253-352">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-353">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="57253-354">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-354">Example</span></span>

<span data-ttu-id="57253-355">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="57253-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="57253-356">**Çoğulluk** değerler her **son** ilişkisini gösteren diğer birçok **siparişler** ile ilişkili bir **müşteri**, ancak yalnızca bir **müşteri** ile ilişkili bir **sipariş**.</span><span class="sxs-lookup"><span data-stu-id="57253-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="57253-357">Ayrıca, **OnDelete** öğesi gösteren tüm **siparişler** belirli bir ilgili **müşteri** ve içine yüklendi ObjectContext olacaktır Silinen if **müşteri** silinir.</span><span class="sxs-lookup"><span data-stu-id="57253-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="57253-358">AssociationSet öğesinin bir alt öğesi olarak bitiş öğesi</span><span class="sxs-lookup"><span data-stu-id="57253-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="57253-359">**Son** öğesi bir ilişki kümesi ucunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="57253-360">**AssociationSet** öğesi iki içermelidir **son** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="57253-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="57253-361">İçinde yer alan bilgileri bir **son** öğesi, bir veri kaynağı olarak ayarlanmış bir ilişkilendirme eşlemesini kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="57253-362">Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-363">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-364">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="57253-365">Ek açıklama öğelerinin diğer tüm alt öğeleri sonra görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="57253-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="57253-366">Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="57253-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="57253-367">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-367">Applicable Attributes</span></span>

<span data-ttu-id="57253-368">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="57253-369">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-369">Attribute Name</span></span> | <span data-ttu-id="57253-370">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-370">Is Required</span></span> | <span data-ttu-id="57253-371">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="57253-372">**EntitySet**</span></span>  | <span data-ttu-id="57253-373">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-373">Yes</span></span>         | <span data-ttu-id="57253-374">Adını **EntitySet** üst ucunu tanımlayan öğe **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="57253-375">**EntitySet** öğesi tanımlanmış, üst ile aynı varlık kapsayıcıda **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="57253-376">**Rol**</span><span class="sxs-lookup"><span data-stu-id="57253-376">**Role**</span></span>       | <span data-ttu-id="57253-377">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-377">No</span></span>          | <span data-ttu-id="57253-378">Son ilişkilendirmenin adı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="57253-378">The name of the association set end.</span></span> <span data-ttu-id="57253-379">Varsa **rol** özniteliği kullanılmazsa, ilişki sonu Ayarla adını varlık kümesinin adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="57253-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="57253-380">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="57253-381">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-382">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="57253-383">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-383">Example</span></span>

<span data-ttu-id="57253-384">Aşağıdaki örnekte gösterildiği bir **EntityContainer** iki öğe **AssociationSet** öğeleri, her iki **son** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="57253-385">EntityContainer öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="57253-386">**EntityContainer** kavramsal şema tanım dili (CSDL) öğesinde, varlık kümelerini ve ilişki Setleri işlevi içeri aktarmalar için mantıksal bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="57253-387">Kavramsal model varlık kapsayıcısı depolama modelinin varlık kapsayıcısı Entitycontainermapping'indeki öğesi ile eşlenir.</span><span class="sxs-lookup"><span data-stu-id="57253-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="57253-388">Bir depolama modelinin varlık kapsayıcısı veritabanının yapısını açıklar: Varlık kümeleri açıklayan tablolar, ilişki Setleri, yabancı anahtar kısıtlamaları açıklar ve işlevi alır, bir veritabanındaki saklı yordamlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="57253-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="57253-389">Bir **EntityContainer** öğesi sıfır veya bir belge öğeleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="57253-390">Varsa bir **belgeleri** öğe varsa, tüm gelmelidir **EntitySet**, **AssociationSet**, ve **Functionımport** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="57253-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="57253-391">Bir **EntityContainer** öğesi (listelenen sırayla) sıfır veya daha fazla şu alt öğelerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="57253-392">EntitySet</span></span>
-   <span data-ttu-id="57253-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="57253-393">AssociationSet</span></span>
-   <span data-ttu-id="57253-394">Functionımport</span><span class="sxs-lookup"><span data-stu-id="57253-394">FunctionImport</span></span>
-   <span data-ttu-id="57253-395">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="57253-395">Annotation elements</span></span>

<span data-ttu-id="57253-396">Genişletebileceğiniz bir **EntityContainer** başka bir deponun içeriğini içerecek şekilde öğesi **EntityContainer** aynı ad alanı içinde olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="57253-397">Başka bir deponun içeriğini içerecek şekilde **EntityContainer**, başvuru olarak **EntityContainer** öğesini ayarlayın **Extends** adınaözniteliği **EntityContainer** dahil etmek istediğiniz öğe.</span><span class="sxs-lookup"><span data-stu-id="57253-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="57253-398">Tüm alt öğeleri dahil **EntityContainer** öğesi başvuru alt öğeleri olarak kabul edilir **EntityContainer** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-399">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-399">Applicable Attributes</span></span>

<span data-ttu-id="57253-400">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **kullanma** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="57253-401">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-401">Attribute Name</span></span> | <span data-ttu-id="57253-402">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-402">Is Required</span></span> | <span data-ttu-id="57253-403">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="57253-404">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-404">**Name**</span></span>       | <span data-ttu-id="57253-405">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-405">Yes</span></span>         | <span data-ttu-id="57253-406">Varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="57253-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="57253-407">**Genişletir**</span><span class="sxs-lookup"><span data-stu-id="57253-407">**Extends**</span></span>    | <span data-ttu-id="57253-408">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-408">No</span></span>          | <span data-ttu-id="57253-409">Aynı ad alanı içinde başka bir varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="57253-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-410">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityContainer** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="57253-411">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-412">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-413">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-413">Example</span></span>

<span data-ttu-id="57253-414">Aşağıdaki örnekte gösterildiği bir **EntityContainer** üç varlık setleri ve iki ilişki Setleri tanımlayan öğe.</span><span class="sxs-lookup"><span data-stu-id="57253-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="57253-415">Entityset'in öğe (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="57253-416">**EntitySet** kavramsal şema tanım dili elemanıdır örnekleri bir varlık türünün ve bu varlık türünden türetilmiş herhangi bir tür örnekleri için mantıksal kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="57253-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="57253-417">Bir varlık türü ve bir varlık kümesi arasındaki ilişki, ilişkisel bir veritabanındaki bir tabloda bir satırı arasındaki ilişkiye benzer.</span><span class="sxs-lookup"><span data-stu-id="57253-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="57253-418">Bir satır gibi ilgili veri kümesi bir varlık türü tanımlar ve bir tablo gibi bir varlık kümesini bu tanımı örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="57253-419">Bir varlık kümesini, böylece bir veri kaynağında ilgili veri yapıları için eşlenebilir varlık türü örneklerini gruplamak için bir yapı sağlar.</span><span class="sxs-lookup"><span data-stu-id="57253-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="57253-420">Bir özel varlık türü için birden fazla varlık tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="57253-421">EF Designer türü başına birden çok varlık kümeleri içeren kavramsal modeller desteklemez.</span><span class="sxs-lookup"><span data-stu-id="57253-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="57253-422">**EntitySet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-423">Belge öğesi (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="57253-424">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-425">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-425">Applicable Attributes</span></span>

<span data-ttu-id="57253-426">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntitySet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="57253-427">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-427">Attribute Name</span></span> | <span data-ttu-id="57253-428">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-428">Is Required</span></span> | <span data-ttu-id="57253-429">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-430">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-430">**Name**</span></span>       | <span data-ttu-id="57253-431">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-431">Yes</span></span>         | <span data-ttu-id="57253-432">Varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="57253-433">**entityType**</span><span class="sxs-lookup"><span data-stu-id="57253-433">**EntityType**</span></span> | <span data-ttu-id="57253-434">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-434">Yes</span></span>         | <span data-ttu-id="57253-435">Varlık türü için varlık kümesi tam olarak nitelenmiş adını örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-436">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntitySet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="57253-437">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-438">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-439">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-439">Example</span></span>

<span data-ttu-id="57253-440">Aşağıdaki örnekte gösterildiği bir **EntityContainer** üç öğeyle **EntitySet** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="57253-441">Tür (MEST) başına birden çok varlık kümesi tanımlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="57253-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="57253-442">Aşağıdaki örnek, bir varlık kapsayıcısı için iki varlık kümeleri tanımlar **kitap** varlık türü:</span><span class="sxs-lookup"><span data-stu-id="57253-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="57253-443">EntityType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="57253-444">**EntityType** öğesi, bir müşteri ya da kavramsal modelde sırası gibi üst düzey bir kavram yapısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="57253-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="57253-445">Bir şablon için bir varlık türü olduğu varlık türleri bir uygulama örneği.</span><span class="sxs-lookup"><span data-stu-id="57253-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="57253-446">Her şablon, aşağıdaki bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="57253-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="57253-447">Benzersiz bir ad.</span><span class="sxs-lookup"><span data-stu-id="57253-447">A unique name.</span></span> <span data-ttu-id="57253-448">(Gerekli)</span><span class="sxs-lookup"><span data-stu-id="57253-448">(Required.)</span></span>
-   <span data-ttu-id="57253-449">Bir veya daha fazla özellikleri tarafından tanımlanan bir varlık anahtarı.</span><span class="sxs-lookup"><span data-stu-id="57253-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="57253-450">(Gerekli)</span><span class="sxs-lookup"><span data-stu-id="57253-450">(Required.)</span></span>
-   <span data-ttu-id="57253-451">Veri içeren özellikleri.</span><span class="sxs-lookup"><span data-stu-id="57253-451">Properties for containing data.</span></span> <span data-ttu-id="57253-452">(İsteğe bağlı.)</span><span class="sxs-lookup"><span data-stu-id="57253-452">(Optional.)</span></span>
-   <span data-ttu-id="57253-453">Bir ilişkilendirmenin bir uçtan diğerine Gezinti diğer ucuna izin Gezinti özellikleri.</span><span class="sxs-lookup"><span data-stu-id="57253-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="57253-454">(İsteğe bağlı.)</span><span class="sxs-lookup"><span data-stu-id="57253-454">(Optional.)</span></span>

<span data-ttu-id="57253-455">Bir uygulamada (örneğin, belirli müşteri veya sipariş) belirli bir nesnesi bir varlık türünün bir örneği temsil eder.</span><span class="sxs-lookup"><span data-stu-id="57253-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="57253-456">Bir varlık türünün her örneğinin bir varlık kümesi içinde benzersiz Varlık anahtarı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="57253-457">İki varlık türü örnekleri yalnızca aynı türde oldukları ve bunların varlık anahtarları aynı değerleri eşit olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="57253-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="57253-458">Bir **EntityType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-459">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-460">Anahtar (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="57253-461">Özellik (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="57253-462">NavigationProperty (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="57253-463">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-464">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-464">Applicable Attributes</span></span>

<span data-ttu-id="57253-465">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntityType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="57253-466">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="57253-467">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-467">Is Required</span></span> | <span data-ttu-id="57253-468">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-469">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="57253-470">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-470">Yes</span></span>         | <span data-ttu-id="57253-471">Varlık türü adı.</span><span class="sxs-lookup"><span data-stu-id="57253-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="57253-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="57253-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="57253-473">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-473">No</span></span>          | <span data-ttu-id="57253-474">Tanımlanmakta olan varlık türünün temel türü başka bir varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="57253-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="57253-475">**Özet**</span><span class="sxs-lookup"><span data-stu-id="57253-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="57253-476">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-476">No</span></span>          | <span data-ttu-id="57253-477">**Doğru** veya **False**varlık türü soyut bir tür olup bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="57253-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="57253-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="57253-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="57253-479">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-479">No</span></span>          | <span data-ttu-id="57253-480">**Doğru** veya **False** varlık türü bir açık varlık türü olup olmamasına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="57253-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="57253-481">> **OpenType** özniteliktir yalnızca ADO.NET Data Services ile kullanılan kavramsal modellerde tanımlanan varlık türleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="57253-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="57253-482">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="57253-483">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-484">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-485">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-485">Example</span></span>

<span data-ttu-id="57253-486">Aşağıdaki örnekte gösterildiği bir **EntityType** üç öğeyle **özelliği** öğeleri ve iki **NavigationProperty** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="57253-487">EnumType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="57253-488">**EnumType** öğesi bir listeden seçimli türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="57253-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="57253-489">Bir **EnumType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-490">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-491">Üye (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="57253-492">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-493">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-493">Applicable Attributes</span></span>

<span data-ttu-id="57253-494">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EnumType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="57253-495">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-495">Attribute Name</span></span>     | <span data-ttu-id="57253-496">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-496">Is Required</span></span> | <span data-ttu-id="57253-497">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-498">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-498">**Name**</span></span>           | <span data-ttu-id="57253-499">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-499">Yes</span></span>         | <span data-ttu-id="57253-500">Varlık türü adı.</span><span class="sxs-lookup"><span data-stu-id="57253-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="57253-501">**Isflags**</span><span class="sxs-lookup"><span data-stu-id="57253-501">**IsFlags**</span></span>        | <span data-ttu-id="57253-502">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-502">No</span></span>          | <span data-ttu-id="57253-503">**Doğru** veya **False**sabit listesi türü bayrakları bir dizi kullanılıp kullanılamayacağı bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="57253-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="57253-504">Varsayılan değer **yanlış.**.</span><span class="sxs-lookup"><span data-stu-id="57253-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="57253-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="57253-505">**UnderlyingType**</span></span> | <span data-ttu-id="57253-506">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-506">No</span></span>          | <span data-ttu-id="57253-507">**Edm.Byte**, **Edm.Int16**, **EDM.Int32**, **EDM.Int64** veya **Edm.SByte** tür değerleri aralığı tanımlama.</span><span class="sxs-lookup"><span data-stu-id="57253-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="57253-508">Sabit listesi öğeleri listesinin temel alınan türü varsayılandır **EDM.Int32.**.</span><span class="sxs-lookup"><span data-stu-id="57253-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-509">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EnumType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="57253-510">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-511">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-512">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-512">Example</span></span>

<span data-ttu-id="57253-513">Aşağıdaki örnekte gösterildiği bir **EnumType** üç öğeyle **üye** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="57253-514">Function öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-514">Function Element (CSDL)</span></span>

<span data-ttu-id="57253-515">**İşlevi** kavramsal şema tanım dili (CSDL) öğesinde tanımlayın veya kavramsal modeldeki işlevleri bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="57253-516">Bir işlev DefiningExpression öğesi kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="57253-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="57253-517">A **işlevi** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-518">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-519">Parametre (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="57253-520">DefiningExpression (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="57253-521">ReturnType (işlev) (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="57253-522">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="57253-523">Dönüş türü için bir işlev ile birlikte belirtilmelidir **ReturnType** (işlev) öğesi veya **ReturnType** özniteliği (aşağıya bakın), ancak ikisine birden değil.</span><span class="sxs-lookup"><span data-stu-id="57253-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="57253-524">Olası dönüş türleri herhangi EdmSimpleType, varlık türü, karmaşık tür, satır türü veya başvuru türü (veya bu tür bir koleksiyonu) var.</span><span class="sxs-lookup"><span data-stu-id="57253-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-525">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-525">Applicable Attributes</span></span>

<span data-ttu-id="57253-526">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **işlevi** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="57253-527">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-527">Attribute Name</span></span> | <span data-ttu-id="57253-528">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-528">Is Required</span></span> | <span data-ttu-id="57253-529">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="57253-530">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-530">**Name**</span></span>       | <span data-ttu-id="57253-531">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-531">Yes</span></span>         | <span data-ttu-id="57253-532">İşlevin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-532">The name of the function.</span></span>          |
| <span data-ttu-id="57253-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="57253-533">**ReturnType**</span></span> | <span data-ttu-id="57253-534">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-534">No</span></span>          | <span data-ttu-id="57253-535">İşlev tarafından döndürülen tür.</span><span class="sxs-lookup"><span data-stu-id="57253-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-536">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **işlevi** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="57253-537">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-538">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-539">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-539">Example</span></span>

<span data-ttu-id="57253-540">Aşağıdaki örnekte bir **işlevi** bir eğitmen işe alındım beri yıl sayısını döndüren bir işlev tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="57253-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="57253-541">Functionımport öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="57253-542">**Functionımport** kavramsal şema tanım dili (CSDL) öğesinde tanımlanan veri kaynağındaki ancak nesnelere kavramsal model aracılığıyla kullanılabilir olan bir işlevi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="57253-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="57253-543">Örneğin, depolama modelinin Function öğesinde, bir veritabanında bir saklı yordam temsil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="57253-544">A **Functionımport** kavramsal model öğesinde karşılık gelen işlev bir Entity Framework uygulamasını temsil eder ve depolama modeli işleve Functionımportmapping element kullanılarak eşlendi.</span><span class="sxs-lookup"><span data-stu-id="57253-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="57253-545">Uygulamada işlev çağrıldığında, karşılık gelen saklı yordamı veritabanında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="57253-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="57253-546">**Functionımport** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-547">Belgeleri (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="57253-548">Parametre (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="57253-549">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="57253-550">ReturnType (Functionımport) (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="57253-551">Bir **parametre** öğesi, işlev kabul eden her parametre için tanımlanmış olması.</span><span class="sxs-lookup"><span data-stu-id="57253-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="57253-552">Dönüş türü için bir işlev ile birlikte belirtilmelidir **ReturnType** (Functionımport) öğesi veya **ReturnType** özniteliği (aşağıya bakın), ancak ikisine birden değil.</span><span class="sxs-lookup"><span data-stu-id="57253-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="57253-553">Dönüş türü değeri koleksiyonu EdmSimpleType, EntityType veya ComplexType olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-554">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-554">Applicable Attributes</span></span>

<span data-ttu-id="57253-555">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **Functionımport** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="57253-556">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-556">Attribute Name</span></span>   | <span data-ttu-id="57253-557">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-557">Is Required</span></span> | <span data-ttu-id="57253-558">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-559">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-559">**Name**</span></span>         | <span data-ttu-id="57253-560">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-560">Yes</span></span>         | <span data-ttu-id="57253-561">İçeri aktarılan bir işlevin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="57253-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="57253-562">**ReturnType**</span></span>   | <span data-ttu-id="57253-563">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-563">No</span></span>          | <span data-ttu-id="57253-564">İşlevinin döndürdüğü türü.</span><span class="sxs-lookup"><span data-stu-id="57253-564">The type that the function returns.</span></span> <span data-ttu-id="57253-565">İşlev bir değer döndürmezse bu özniteliği kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="57253-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="57253-566">Aksi takdirde, değeri, ComplexType, EntityType veya EDMSimpleType koleksiyonu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="57253-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="57253-567">**EntitySet**</span></span>    | <span data-ttu-id="57253-568">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-568">No</span></span>          | <span data-ttu-id="57253-569">Varlık koleksiyonu işlevi döndürürse, türleri, değerini **EntitySet** koleksiyona ait olduğu varlık kümesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="57253-570">Aksi takdirde, **EntitySet** özniteliği değil kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="57253-571">**Iscomposable**</span><span class="sxs-lookup"><span data-stu-id="57253-571">**IsComposable**</span></span> | <span data-ttu-id="57253-572">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-572">No</span></span>          | <span data-ttu-id="57253-573">Değer ayarlanmışsa true, işlev birleştirilebilir (tablo değerli işlev) ve bir LINQ Sorgu kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="57253-574">Varsayılan değer **false**.</span><span class="sxs-lookup"><span data-stu-id="57253-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="57253-575">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **Functionımport** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="57253-576">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-577">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-578">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-578">Example</span></span>

<span data-ttu-id="57253-579">Aşağıdaki örnekte gösterildiği bir **Functionımport** öğesi, bir parametre kabul eden ve varlık türleri koleksiyonunu döndürür:</span><span class="sxs-lookup"><span data-stu-id="57253-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="57253-580">Anahtar öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-580">Key Element (CSDL)</span></span>

<span data-ttu-id="57253-581">**Anahtarı** öğesi EntityType öğesi bir alt öğesidir ve tanımlayan bir *Varlık anahtarı* (bir özellik veya bir varlık türünün kimliğini belirlemek özellikler kümesi).</span><span class="sxs-lookup"><span data-stu-id="57253-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="57253-582">Bir varlık anahtarı oluşturan özelliklerini tasarım zamanında seçilir.</span><span class="sxs-lookup"><span data-stu-id="57253-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="57253-583">Varlık anahtar özelliklerin değerlerini çalışma zamanında ayarlama varlık içinde bir varlık türü örneği benzersiz şekilde tanımlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="57253-584">Bir varlık kümesindeki örneklerin benzersizliği garanti etmek için bir varlık anahtarı oluşturan özellikleri seçilmelidir.</span><span class="sxs-lookup"><span data-stu-id="57253-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="57253-585">**Anahtar** öğesi bir veya daha fazla özellik bir varlık türünün başvurarak Varlık anahtarı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="57253-586">**Anahtar** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="57253-587">PropertyRef (bir veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="57253-588">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-589">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-589">Applicable Attributes</span></span>

<span data-ttu-id="57253-590">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **anahtar** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="57253-591">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-592">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="57253-593">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-593">Example</span></span>

<span data-ttu-id="57253-594">Aşağıdaki örnekte adlı bir varlık türü tanımlayan **kitap**.</span><span class="sxs-lookup"><span data-stu-id="57253-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="57253-595">Varlık anahtarı başvurarak tanımlanan **ISBN** varlık türü özelliği.</span><span class="sxs-lookup"><span data-stu-id="57253-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="57253-596">**ISBN** özelliği olduğundan Varlık anahtarı için iyi bir seçim bir kitap tanıtan bir Uluslararası Standart Kitap sayısı (ISBN).</span><span class="sxs-lookup"><span data-stu-id="57253-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="57253-597">Aşağıdaki örnek, bir varlık türü gösterir (**Yazar**) iki özelliklerini içeren bir varlık anahtarına sahip **adı** ve **adresi**.</span><span class="sxs-lookup"><span data-stu-id="57253-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="57253-598">Kullanarak **adı** ve **adresi** çünkü aynı adlı iki yazarları aynı adresindeki live olasılığının düşük olduğu varlık için makul bir seçim anahtardır.</span><span class="sxs-lookup"><span data-stu-id="57253-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="57253-599">Ancak, bu seçenek bir varlık anahtarı için bir varlık kümesindeki benzersiz varlık anahtarları kesinlikle garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="57253-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="57253-600">Bir özellik gibi ekleme **AuthorId**, benzersiz şekilde tanımlamak için kullanılabilir bir yazar önerilen bu durumda.</span><span class="sxs-lookup"><span data-stu-id="57253-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="57253-601">Üye öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-601">Member Element (CSDL)</span></span>

<span data-ttu-id="57253-602">**Üye** öğesi EnumType öğesi bir alt öğesidir ve numaralandırılmış tür üyesi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-603">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-603">Applicable Attributes</span></span>

<span data-ttu-id="57253-604">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **Functionımport** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="57253-605">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-605">Attribute Name</span></span> | <span data-ttu-id="57253-606">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-606">Is Required</span></span> | <span data-ttu-id="57253-607">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-608">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-608">**Name**</span></span>       | <span data-ttu-id="57253-609">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-609">Yes</span></span>         | <span data-ttu-id="57253-610">Üyenin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="57253-611">**Değer**</span><span class="sxs-lookup"><span data-stu-id="57253-611">**Value**</span></span>      | <span data-ttu-id="57253-612">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-612">No</span></span>          | <span data-ttu-id="57253-613">Üyenin değeri.</span><span class="sxs-lookup"><span data-stu-id="57253-613">The value of the member.</span></span> <span data-ttu-id="57253-614">Varsayılan olarak 0 değeri ilk üye sahiptir ve art arda gelen her Numaralandırıcı değeri 1 azaltılır.</span><span class="sxs-lookup"><span data-stu-id="57253-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="57253-615">Aynı değerlere sahip birden fazla üye var olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-616">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **Functionımport** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="57253-617">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-618">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-619">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-619">Example</span></span>

<span data-ttu-id="57253-620">Aşağıdaki örnekte gösterildiği bir **EnumType** üç öğeyle **üye** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="57253-621">NavigationProperty öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="57253-622">A **NavigationProperty** öğe bir ilişki sonu bir başvuru sağlayan bir gezinti özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="57253-623">Özellik öğesi ile tanımlanan özelliklerin aksine, gezinti özellikleri veri özelliklerini ve Şekil tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="57253-624">İki varlık türleri arasındaki ilişkiyi gitmek için bir yol sağlarlar.</span><span class="sxs-lookup"><span data-stu-id="57253-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="57253-625">Gezinti özellikleri her iki varlık türlerini ilişkilendirme ucunda isteğe bağlı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="57253-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="57253-626">İlişkilendirme sonunda bir varlık türünün gezinme özelliğinin tanımlarsanız, ilişkinin diğer ucundaki varlık türünün gezinme özelliğinin tanımlamak zorunda değil.</span><span class="sxs-lookup"><span data-stu-id="57253-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="57253-627">Bir gezinti özelliği tarafından döndürülen veri türü Uzak ilişkilendirme bitiş'ün çoğulluğunun tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="57253-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="57253-628">Örneğin, bir gezinti özelliği varsayalım **OrdersNavProp**, bulunuyor. bir **müşteri** varlık türü ve arasında bir-çok ilişkisi gider **müşteri** ve  **Sipariş**.</span><span class="sxs-lookup"><span data-stu-id="57253-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="57253-629">Gezinti özelliği için Uzak ilişkilendirme end çok çeşitlilik olduğundan (\*), kendi veri türüne koleksiyonudur (, **sipariş**).</span><span class="sxs-lookup"><span data-stu-id="57253-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="57253-630">Benzer şekilde, bir gezinti özelliği, **CustomerNavProp**, bulunuyor **sipariş** varlık türü, veri türü olacak **müşteri** uzak uç çokluğu olduğundan bir (1).</span><span class="sxs-lookup"><span data-stu-id="57253-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="57253-631">A **NavigationProperty** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-632">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-633">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-634">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-634">Applicable Attributes</span></span>

<span data-ttu-id="57253-635">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **NavigationProperty** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="57253-636">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-636">Attribute Name</span></span>   | <span data-ttu-id="57253-637">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-637">Is Required</span></span> | <span data-ttu-id="57253-638">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-639">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-639">**Name**</span></span>         | <span data-ttu-id="57253-640">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-640">Yes</span></span>         | <span data-ttu-id="57253-641">Gezinme özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="57253-642">**İlişki**</span><span class="sxs-lookup"><span data-stu-id="57253-642">**Relationship**</span></span> | <span data-ttu-id="57253-643">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-643">Yes</span></span>         | <span data-ttu-id="57253-644">Modeli kapsamında olmayan bir ilişkilendirmenin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="57253-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="57253-645">**ToRole**</span></span>       | <span data-ttu-id="57253-646">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-646">Yes</span></span>         | <span data-ttu-id="57253-647">Gezinti biteceği ilişki sonu.</span><span class="sxs-lookup"><span data-stu-id="57253-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="57253-648">Değerini **ToRole** özniteliği bir değer ile aynı olmalıdır **rol** (ilişki ucu öğesinde tanımlanan) ilişkilendirme uçlarından tanımlanan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="57253-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="57253-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="57253-649">**FromRole**</span></span>     | <span data-ttu-id="57253-650">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-650">Yes</span></span>         | <span data-ttu-id="57253-651">Gezinti başlar ilişki sonu.</span><span class="sxs-lookup"><span data-stu-id="57253-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="57253-652">Değerini **FromRole** özniteliği bir değer ile aynı olmalıdır **rol** (ilişki ucu öğesinde tanımlanan) ilişkilendirme uçlarından tanımlanan öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="57253-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-653">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **NavigationProperty** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="57253-654">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-655">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-656">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-656">Example</span></span>

<span data-ttu-id="57253-657">Aşağıdaki örnek, bir varlık türü tanımlar. (**kitap**) iki Gezinti özellikleri ile (**yayınladığı** ve **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="57253-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="57253-658">OnDelete öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="57253-659">**OnDelete** kavramsal şema tanım dili (CSDL) öğesinde bir ilişkilendirme bağlı davranışını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="57253-660">Varsa **eylem** özniteliği **Cascade** ilişkilendirme bir ucunda, ilişkinin diğer ucundaki varlık türleri varlık türü ilk uca silindiğinde silinir ilgili.</span><span class="sxs-lookup"><span data-stu-id="57253-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="57253-661">İki varlık türleri arasındaki ilişkiyi bir birincil anahtar birincil anahtar ilişkidir sonra yüklenen bir bağımlı nesne bağımsız olarak, ilişkinin diğer ucundaki sorumlusu nesnesi silindiğinde silinir **OnDelete** belirtimi.</span><span class="sxs-lookup"><span data-stu-id="57253-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="57253-662">**OnDelete** öğesi yalnızca bir uygulama çalışma zamanı davranışını etkiler; veri kaynağı davranışını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="57253-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="57253-663">Veri kaynağında tanımlanan davranışı uygulamada tanımlanan davranışı ile aynı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="57253-664">Bir **OnDelete** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-665">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-666">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-667">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-667">Applicable Attributes</span></span>

<span data-ttu-id="57253-668">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **OnDelete** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="57253-669">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-669">Attribute Name</span></span> | <span data-ttu-id="57253-670">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-670">Is Required</span></span> | <span data-ttu-id="57253-671">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-672">**Eylem**</span><span class="sxs-lookup"><span data-stu-id="57253-672">**Action**</span></span>     | <span data-ttu-id="57253-673">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-673">Yes</span></span>         | <span data-ttu-id="57253-674">**Art arda** veya **hiçbiri**.</span><span class="sxs-lookup"><span data-stu-id="57253-674">**Cascade** or **None**.</span></span> <span data-ttu-id="57253-675">Varsa **Cascade**, bağımlı varlık türleri, asıl varlık türü silindiğinde silinir.</span><span class="sxs-lookup"><span data-stu-id="57253-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="57253-676">Varsa **hiçbiri**, asıl varlık türü silindiğinde bağımlı varlık türleri silinmeyecek.</span><span class="sxs-lookup"><span data-stu-id="57253-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-677">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="57253-678">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-679">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-680">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-680">Example</span></span>

<span data-ttu-id="57253-681">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="57253-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="57253-682">**OnDelete** öğesi gösteren tüm **siparişler** belirli bir ilgili **müşteri** ve ObjectContext yüklenen zaman silinecek  **Müşteri** silinir.</span><span class="sxs-lookup"><span data-stu-id="57253-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="57253-683">Parametre öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="57253-684">**Parametre** kavramsal şema tanım dili (CSDL) öğesinde bir alt Functionımport öğesi veya Function öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="57253-685">Functionımport öğesi uygulama</span><span class="sxs-lookup"><span data-stu-id="57253-685">FunctionImport Element Application</span></span>

<span data-ttu-id="57253-686">A **parametre** öğesi (alt öğesi olarak **Functionımport** öğesi) CSDL içinde bildirilen işlev içeri aktarmalar için giriş ve çıkış parametrelerini tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="57253-687">**Parametre** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-688">Belgeleri (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="57253-689">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="57253-690">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-690">Applicable Attributes</span></span>

<span data-ttu-id="57253-691">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="57253-692">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-692">Attribute Name</span></span> | <span data-ttu-id="57253-693">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-693">Is Required</span></span> | <span data-ttu-id="57253-694">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-695">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-695">**Name**</span></span>       | <span data-ttu-id="57253-696">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-696">Yes</span></span>         | <span data-ttu-id="57253-697">Parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="57253-698">**Türü**</span><span class="sxs-lookup"><span data-stu-id="57253-698">**Type**</span></span>       | <span data-ttu-id="57253-699">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-699">Yes</span></span>         | <span data-ttu-id="57253-700">Parametre türü.</span><span class="sxs-lookup"><span data-stu-id="57253-700">The parameter type.</span></span> <span data-ttu-id="57253-701">Değer olmalıdır bir **EDMSimpleType** veya modeli kapsamında olan bir karmaşık türü.</span><span class="sxs-lookup"><span data-stu-id="57253-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="57253-702">**Modu**</span><span class="sxs-lookup"><span data-stu-id="57253-702">**Mode**</span></span>       | <span data-ttu-id="57253-703">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-703">No</span></span>          | <span data-ttu-id="57253-704">**İçinde**, **kullanıma**, veya **Inout** parametresi bir giriş, çıkış ve giriş/çıkış parametresi olmasına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="57253-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="57253-705">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="57253-705">**MaxLength**</span></span>  | <span data-ttu-id="57253-706">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-706">No</span></span>          | <span data-ttu-id="57253-707">Parametresinin uzunluğu izin verilen en fazla.</span><span class="sxs-lookup"><span data-stu-id="57253-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="57253-708">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="57253-708">**Precision**</span></span>  | <span data-ttu-id="57253-709">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-709">No</span></span>          | <span data-ttu-id="57253-710">Parametre duyarlılığı.</span><span class="sxs-lookup"><span data-stu-id="57253-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="57253-711">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="57253-711">**Scale**</span></span>      | <span data-ttu-id="57253-712">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-712">No</span></span>          | <span data-ttu-id="57253-713">Parametre ölçeği.</span><span class="sxs-lookup"><span data-stu-id="57253-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="57253-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="57253-714">**SRID**</span></span>       | <span data-ttu-id="57253-715">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-715">No</span></span>          | <span data-ttu-id="57253-716">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="57253-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="57253-717">Uzamsal tür parametreleri yalnızca için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="57253-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="57253-718">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="57253-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-719">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="57253-720">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-721">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="57253-722">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-722">Example</span></span>

<span data-ttu-id="57253-723">Aşağıdaki örnekte gösterildiği bir **Functionımport** bir öğeyle **parametre** alt öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="57253-724">İşlev bir giriş parametresi kabul eden ve varlık türleri koleksiyonunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="57253-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="57253-725">İşlev öğe uygulama</span><span class="sxs-lookup"><span data-stu-id="57253-725">Function Element Application</span></span>

<span data-ttu-id="57253-726">A **parametre** öğesi (alt öğesi olarak **işlevi** öğesi) kavramsal modelde tanımlı ya da bildirilen işlevler için parametreleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="57253-727">**Parametre** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-728">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="57253-729">CollectionType (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="57253-730">ReferenceType (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="57253-731">RowType (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="57253-732">Yalnızca biri **CollectionType**, **ReferenceType**, veya **RowType** öğeleri alt öğesi olabilir bir **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="57253-733">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="57253-734">Ek açıklama öğelerinin diğer tüm alt öğeleri sonra görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="57253-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="57253-735">Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="57253-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="57253-736">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-736">Applicable Attributes</span></span>

<span data-ttu-id="57253-737">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="57253-738">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-738">Attribute Name</span></span>   | <span data-ttu-id="57253-739">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-739">Is Required</span></span> | <span data-ttu-id="57253-740">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-741">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-741">**Name**</span></span>         | <span data-ttu-id="57253-742">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-742">Yes</span></span>         | <span data-ttu-id="57253-743">Parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="57253-744">**Türü**</span><span class="sxs-lookup"><span data-stu-id="57253-744">**Type**</span></span>         | <span data-ttu-id="57253-745">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-745">No</span></span>          | <span data-ttu-id="57253-746">Parametre türü.</span><span class="sxs-lookup"><span data-stu-id="57253-746">The parameter type.</span></span> <span data-ttu-id="57253-747">Bir parametre aşağıdaki türleri (veya bu türler) herhangi biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="57253-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="57253-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="57253-749">varlık türü</span><span class="sxs-lookup"><span data-stu-id="57253-749">entity type</span></span> <br/> <span data-ttu-id="57253-750">Karmaşık tür</span><span class="sxs-lookup"><span data-stu-id="57253-750">complex type</span></span> <br/> <span data-ttu-id="57253-751">Satır türü</span><span class="sxs-lookup"><span data-stu-id="57253-751">row type</span></span> <br/> <span data-ttu-id="57253-752">başvuru türü</span><span class="sxs-lookup"><span data-stu-id="57253-752">reference type</span></span>                             |
| <span data-ttu-id="57253-753">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="57253-753">**Nullable**</span></span>     | <span data-ttu-id="57253-754">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-754">No</span></span>          | <span data-ttu-id="57253-755">**Doğru** (varsayılan değer) veya **False** özelliğine sahip olup olmadığına bağlı olarak bir **null** değeri.</span><span class="sxs-lookup"><span data-stu-id="57253-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="57253-756">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="57253-756">**DefaultValue**</span></span> | <span data-ttu-id="57253-757">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-757">No</span></span>          | <span data-ttu-id="57253-758">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="57253-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="57253-759">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="57253-759">**MaxLength**</span></span>    | <span data-ttu-id="57253-760">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-760">No</span></span>          | <span data-ttu-id="57253-761">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="57253-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="57253-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="57253-762">**FixedLength**</span></span>  | <span data-ttu-id="57253-763">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-763">No</span></span>          | <span data-ttu-id="57253-764">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="57253-765">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="57253-765">**Precision**</span></span>    | <span data-ttu-id="57253-766">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-766">No</span></span>          | <span data-ttu-id="57253-767">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="57253-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="57253-768">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="57253-768">**Scale**</span></span>        | <span data-ttu-id="57253-769">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-769">No</span></span>          | <span data-ttu-id="57253-770">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="57253-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="57253-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="57253-771">**SRID**</span></span>         | <span data-ttu-id="57253-772">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-772">No</span></span>          | <span data-ttu-id="57253-773">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="57253-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="57253-774">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="57253-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="57253-775">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="57253-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="57253-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="57253-776">**Unicode**</span></span>      | <span data-ttu-id="57253-777">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-777">No</span></span>          | <span data-ttu-id="57253-778">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="57253-779">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="57253-779">**Collation**</span></span>    | <span data-ttu-id="57253-780">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-780">No</span></span>          | <span data-ttu-id="57253-781">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="57253-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="57253-782">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="57253-783">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-784">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="57253-785">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-785">Example</span></span>

<span data-ttu-id="57253-786">Aşağıdaki örnekte gösterildiği bir **işlevi** kullanan tek öğe **parametresi** bir işlev parametresi tanımlamak için alt öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a><span data-ttu-id="57253-787">Asıl öğe (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="57253-788">**Asıl** kavramsal şema tanım dili (CSDL) öğesinde Referentialconstraint'teki öğesine bir başvuru kısıtlamasının birincil ucu tanımlayan bir alt öğedir.</span><span class="sxs-lookup"><span data-stu-id="57253-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="57253-789">A **Referentialconstraint'teki** öğe ilişkisel bir veritabanındaki bir başvuru bütünlüğü kısıtlaması benzer işlevselliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="57253-790">Bir veritabanı tablosundan bir sütuna (veya sütun) başka bir tablonun birincil anahtarı başvurabilirsiniz aynı şekilde, başka bir varlık türünün Varlık anahtarı bir varlık türünün bir özelliği (veya Özellikler) başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="57253-791">Başvurulan varlık türü olarak adlandırılır *birincil ucu* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="57253-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="57253-792">Birincil ucu başvuran varlık türü olarak adlandırılan *bağımlı son* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="57253-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="57253-793">**PropertyRef** öğeleri hangi anahtarları bağımlı uç tarafından başvurulan belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="57253-794">**Asıl** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-795">PropertyRef (bir veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="57253-796">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-797">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-797">Applicable Attributes</span></span>

<span data-ttu-id="57253-798">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **asıl** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="57253-799">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-799">Attribute Name</span></span> | <span data-ttu-id="57253-800">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-800">Is Required</span></span> | <span data-ttu-id="57253-801">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="57253-802">**Rol**</span><span class="sxs-lookup"><span data-stu-id="57253-802">**Role**</span></span>       | <span data-ttu-id="57253-803">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-803">Yes</span></span>         | <span data-ttu-id="57253-804">İlişkisinin birincil ucu ' varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="57253-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-805">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **asıl** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="57253-806">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-807">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-808">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-808">Example</span></span>

<span data-ttu-id="57253-809">Aşağıdaki örnekte gösterildiği bir **Referentialconstraint'teki** tanımının bir parçası olan öğeyi **yayınladığı** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="57253-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="57253-810">**Kimliği** özelliği **yayımcı** varlık türü başvuru kısıtlamasını birincil ucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="57253-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="57253-811">Özellik öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-811">Property Element (CSDL)</span></span>

<span data-ttu-id="57253-812">**Özelliği** kavramsal şema tanım dili (CSDL) öğesinde bir alt öğe EntityType, ComplexType öğesi veya RowType öğesinin olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="57253-813">EntityType ve ComplexType öğesi uygulamalar</span><span class="sxs-lookup"><span data-stu-id="57253-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="57253-814">**Özellik** öğeleri (alt öğeleri olarak **EntityType** veya **ComplexType** öğeleri) şekil ve bir varlık türü örneği veya karmaşık tür örneği içeren veri özelliklerini tanımlar .</span><span class="sxs-lookup"><span data-stu-id="57253-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="57253-815">Kavramsal modelde özellikleri, bir sınıf üzerinde tanımlanan özelliklere benzer.</span><span class="sxs-lookup"><span data-stu-id="57253-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="57253-816">Bir sınıfındaki özellikleri sınıfı şeklini tanımlamak ve nesneler hakkında bilgi taşımak aynı şekilde, kavramsal model özelliklerinde bir varlık türü şeklini tanımlamak ve varlık türü örnekleri hakkında bilgi taşımak.</span><span class="sxs-lookup"><span data-stu-id="57253-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="57253-817">**Özelliği** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-818">Belge öğesi (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="57253-819">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="57253-820">Aşağıdaki özellikleri uygulanabilir bir **özelliği** öğesi: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **duyarlık**, **ölçek**, **Unicode**, **harmanlama**,  **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="57253-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="57253-821">Modelleri veri deposunda özellik değerlerinin nasıl depolandığını konusunda bilgiler sağlayan XML öznitelikleridir.</span><span class="sxs-lookup"><span data-stu-id="57253-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="57253-822">Modelleri yalnızca türünün özelliklerine uygulanabilir **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="57253-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="57253-823">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-823">Applicable Attributes</span></span>

<span data-ttu-id="57253-824">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="57253-825">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-825">Attribute Name</span></span>                                                         | <span data-ttu-id="57253-826">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-826">Is Required</span></span> | <span data-ttu-id="57253-827">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-828">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-828">**Name**</span></span>                                                               | <span data-ttu-id="57253-829">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-829">Yes</span></span>         | <span data-ttu-id="57253-830">Özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="57253-831">**Türü**</span><span class="sxs-lookup"><span data-stu-id="57253-831">**Type**</span></span>                                                               | <span data-ttu-id="57253-832">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-832">Yes</span></span>         | <span data-ttu-id="57253-833">Özellik değerinin türü.</span><span class="sxs-lookup"><span data-stu-id="57253-833">The type of the property value.</span></span> <span data-ttu-id="57253-834">Özellik değeri türü olmalıdır bir **EDMSimpleType** veya modeli kapsamında olmayan (bir tam ad tarafından gösterilen) bir karmaşık türü.</span><span class="sxs-lookup"><span data-stu-id="57253-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="57253-835">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="57253-835">**Nullable**</span></span>                                                           | <span data-ttu-id="57253-836">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-836">No</span></span>          | <span data-ttu-id="57253-837">**Doğru** (varsayılan değer) veya <strong>False</strong> bağlı olup olmadığını özelliği null değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="57253-838">> CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="57253-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="57253-839">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="57253-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="57253-840">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-840">No</span></span>          | <span data-ttu-id="57253-841">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="57253-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="57253-842">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="57253-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="57253-843">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-843">No</span></span>          | <span data-ttu-id="57253-844">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="57253-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="57253-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="57253-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="57253-846">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-846">No</span></span>          | <span data-ttu-id="57253-847">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="57253-848">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="57253-848">**Precision**</span></span>                                                          | <span data-ttu-id="57253-849">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-849">No</span></span>          | <span data-ttu-id="57253-850">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="57253-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="57253-851">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="57253-851">**Scale**</span></span>                                                              | <span data-ttu-id="57253-852">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-852">No</span></span>          | <span data-ttu-id="57253-853">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="57253-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="57253-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="57253-854">**SRID**</span></span>                                                               | <span data-ttu-id="57253-855">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-855">No</span></span>          | <span data-ttu-id="57253-856">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="57253-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="57253-857">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="57253-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="57253-858">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="57253-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="57253-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="57253-859">**Unicode**</span></span>                                                            | <span data-ttu-id="57253-860">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-860">No</span></span>          | <span data-ttu-id="57253-861">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="57253-862">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="57253-862">**Collation**</span></span>                                                          | <span data-ttu-id="57253-863">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-863">No</span></span>          | <span data-ttu-id="57253-864">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="57253-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="57253-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="57253-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="57253-866">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-866">No</span></span>          | <span data-ttu-id="57253-867">**Hiçbiri** (varsayılan değer) veya **sabit**.</span><span class="sxs-lookup"><span data-stu-id="57253-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="57253-868">Değer ayarlanmışsa **sabit**, iyimser eşzamanlılık denetimlerini özellik değeri kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="57253-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="57253-869">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="57253-870">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-871">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="57253-872">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-872">Example</span></span>

<span data-ttu-id="57253-873">Aşağıdaki örnekte gösterildiği bir **EntityType** üç öğeyle **özelliği** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="57253-874">Aşağıdaki örnekte gösterildiği bir **ComplexType** beş öğe **özelliği** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="57253-875">RowType öğesinin uygulama</span><span class="sxs-lookup"><span data-stu-id="57253-875">RowType Element Application</span></span>

<span data-ttu-id="57253-876">**Özellik** öğeleri (alt olarak bir **RowType** öğe) şekil ve geçirilen veya model tanımlı bir işlevden döndürülen verilerin özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="57253-877">**Özelliği** öğesi şu alt öğelerden tam olarak birine sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="57253-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="57253-878">CollectionType</span></span>
-   <span data-ttu-id="57253-879">referenceType</span><span class="sxs-lookup"><span data-stu-id="57253-879">ReferenceType</span></span>
-   <span data-ttu-id="57253-880">RowType</span><span class="sxs-lookup"><span data-stu-id="57253-880">RowType</span></span>

<span data-ttu-id="57253-881">**Özelliği** öğesi sayı alt ek açıklama öğeleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="57253-882">Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="57253-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="57253-883">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-883">Applicable Attributes</span></span>

<span data-ttu-id="57253-884">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="57253-885">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-885">Attribute Name</span></span>                                                     | <span data-ttu-id="57253-886">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-886">Is Required</span></span> | <span data-ttu-id="57253-887">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-888">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-888">**Name**</span></span>                                                           | <span data-ttu-id="57253-889">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-889">Yes</span></span>         | <span data-ttu-id="57253-890">Özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="57253-891">**Türü**</span><span class="sxs-lookup"><span data-stu-id="57253-891">**Type**</span></span>                                                           | <span data-ttu-id="57253-892">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-892">Yes</span></span>         | <span data-ttu-id="57253-893">Özellik değerinin türü.</span><span class="sxs-lookup"><span data-stu-id="57253-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="57253-894">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="57253-894">**Nullable**</span></span>                                                       | <span data-ttu-id="57253-895">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-895">No</span></span>          | <span data-ttu-id="57253-896">**Doğru** (varsayılan değer) veya **False** bağlı olup olmadığını özelliği null değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="57253-897">> CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="57253-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="57253-898">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="57253-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="57253-899">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-899">No</span></span>          | <span data-ttu-id="57253-900">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="57253-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="57253-901">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="57253-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="57253-902">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-902">No</span></span>          | <span data-ttu-id="57253-903">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="57253-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="57253-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="57253-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="57253-905">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-905">No</span></span>          | <span data-ttu-id="57253-906">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="57253-907">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="57253-907">**Precision**</span></span>                                                      | <span data-ttu-id="57253-908">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-908">No</span></span>          | <span data-ttu-id="57253-909">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="57253-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="57253-910">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="57253-910">**Scale**</span></span>                                                          | <span data-ttu-id="57253-911">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-911">No</span></span>          | <span data-ttu-id="57253-912">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="57253-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="57253-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="57253-913">**SRID**</span></span>                                                           | <span data-ttu-id="57253-914">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-914">No</span></span>          | <span data-ttu-id="57253-915">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="57253-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="57253-916">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="57253-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="57253-917">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="57253-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="57253-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="57253-918">**Unicode**</span></span>                                                        | <span data-ttu-id="57253-919">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-919">No</span></span>          | <span data-ttu-id="57253-920">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="57253-921">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="57253-921">**Collation**</span></span>                                                      | <span data-ttu-id="57253-922">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-922">No</span></span>          | <span data-ttu-id="57253-923">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="57253-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="57253-924">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="57253-925">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-926">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="57253-927">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-927">Example</span></span>

<span data-ttu-id="57253-928">Aşağıdaki örnekte gösterildiği **özelliği** model tanımlı bir işlevin dönüş türü şeklini tanımlamak için kullanılan öğeleri.</span><span class="sxs-lookup"><span data-stu-id="57253-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="57253-929">PropertyRef öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="57253-930">**PropertyRef** kavramsal şema tanım dili (CSDL) öğesinde başvuran bir özelliği bir varlık türünün özelliğini aşağıdaki rollerden biri gerçekleştirecek belirtmek için:</span><span class="sxs-lookup"><span data-stu-id="57253-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="57253-931">(Bir özellik veya bir varlık türünün kimliğini belirlemek özellikler kümesi) varlığın anahtarının bir parçası.</span><span class="sxs-lookup"><span data-stu-id="57253-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="57253-932">Bir veya daha fazla **PropertyRef** öğeleri, bir varlığın anahtarı tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="57253-933">Başvuru kısıtlamasını bağımlı veya asıl bitiş olayı.</span><span class="sxs-lookup"><span data-stu-id="57253-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="57253-934">**PropertyRef** öğesi yalnızca alt öğeleri olarak ek açıklama öğelerinin (sıfır veya daha fazla) olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="57253-935">Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.</span><span class="sxs-lookup"><span data-stu-id="57253-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="57253-936">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-936">Applicable Attributes</span></span>

<span data-ttu-id="57253-937">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **PropertyRef** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="57253-938">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-938">Attribute Name</span></span> | <span data-ttu-id="57253-939">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-939">Is Required</span></span> | <span data-ttu-id="57253-940">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="57253-941">**Ad**</span><span class="sxs-lookup"><span data-stu-id="57253-941">**Name**</span></span>       | <span data-ttu-id="57253-942">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-942">Yes</span></span>         | <span data-ttu-id="57253-943">Başvurulan özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="57253-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-944">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **PropertyRef** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="57253-945">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-946">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-947">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-947">Example</span></span>

<span data-ttu-id="57253-948">Aşağıdaki örnek bir varlık türü tanımlar (**kitap**).</span><span class="sxs-lookup"><span data-stu-id="57253-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="57253-949">Varlık anahtarı başvurarak tanımlanan **ISBN** varlık türü özelliği.</span><span class="sxs-lookup"><span data-stu-id="57253-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="57253-950">Sonraki örnekte, iki **PropertyRef** öğeleri ki belirtmek için kullanılan özellikler (**kimliği** ve **Publisherıd**) olan bir başvuru birincil ve bağımlı uçlarından kısıtlama.</span><span class="sxs-lookup"><span data-stu-id="57253-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="57253-951">ReferenceType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="57253-952">**ReferenceType** kavramsal şema tanım dili (CSDL) öğesinde bir varlık türü bir başvuru belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="57253-953">**ReferenceType** aşağıdaki öğelerin bir alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="57253-954">ReturnType (işlev)</span><span class="sxs-lookup"><span data-stu-id="57253-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="57253-955">Parametre</span><span class="sxs-lookup"><span data-stu-id="57253-955">Parameter</span></span>
-   <span data-ttu-id="57253-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="57253-956">CollectionType</span></span>

<span data-ttu-id="57253-957">**ReferenceType** öğesi, bir parametre veya dönüş türü bir işlev için tanımlarken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="57253-958">A **ReferenceType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-959">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-960">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-961">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-961">Applicable Attributes</span></span>

<span data-ttu-id="57253-962">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ReferenceType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="57253-963">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-963">Attribute Name</span></span> | <span data-ttu-id="57253-964">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-964">Is Required</span></span> | <span data-ttu-id="57253-965">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="57253-966">**Türü**</span><span class="sxs-lookup"><span data-stu-id="57253-966">**Type**</span></span>       | <span data-ttu-id="57253-967">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-967">Yes</span></span>         | <span data-ttu-id="57253-968">Başvurulan varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="57253-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-969">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReferenceType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="57253-970">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-971">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-972">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-972">Example</span></span>

<span data-ttu-id="57253-973">Aşağıdaki örnekte gösterildiği **ReferenceType** öğesi alt öğesi olarak kullanılan bir **parametre** başvuruyu kabul eden bir model tanımlı işlev öğesinde bir **kişi** varlık türü:</span><span class="sxs-lookup"><span data-stu-id="57253-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="57253-974">Aşağıdaki örnekte gösterildiği **ReferenceType** öğesi alt öğesi olarak kullanılan bir **ReturnType** (işlev) öğesi için bir başvuru döndürür model tanımlı bir işlevdeki bir **kişi**varlık türü:</span><span class="sxs-lookup"><span data-stu-id="57253-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="57253-975">Referentialconstraint'teki öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="57253-976">A **Referentialconstraint'teki** kavramsal şema tanım dili (CSDL) öğesinde bir başvuru bütünlüğü kısıtlaması ilişkisel bir veritabanındaki benzer işlevselliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="57253-977">Bir veritabanı tablosundan bir sütuna (veya sütun) başka bir tablonun birincil anahtarı başvurabilirsiniz aynı şekilde, başka bir varlık türünün Varlık anahtarı bir varlık türünün bir özelliği (veya Özellikler) başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="57253-978">Başvurulan varlık türü olarak adlandırılır *birincil ucu* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="57253-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="57253-979">Birincil ucu başvuran varlık türü olarak adlandırılan *bağımlı son* kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="57253-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="57253-980">Bir varlık türü üzerinde sunulan bir yabancı anahtar üzerindeki başka bir varlık türü, bir özellik başvuruyorsa **Referentialconstraint'teki** öğe iki varlık türleri arasındaki ilişkiyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="57253-981">Çünkü **Referentialconstraint'teki** öğesi sağlar nasıl iki varlık türleri hakkında bilgi ilişkilidir, karşılık gelen hiçbir AssociationSetMapping öğe eşleme belirtimi dili (MSL) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="57253-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="57253-982">Kullanıma sunulan bir yabancı anahtarları yoksa iki varlık türleri arasındaki ilişkiyi karşılık gelen olmalıdır **AssociationSetMapping** ilişkilendirme bilgileri veri kaynağına eşlemek için öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="57253-983">Bir varlık türünde, yabancı anahtar açık değilse **Referentialconstraint'teki** öğesi yalnızca bir varlık türü ve başka bir varlık türü arasında birincil anahtar birincil anahtar kısıtlaması tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="57253-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="57253-984">A **Referentialconstraint'teki** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-985">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-986">Asıl (tam olarak bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="57253-987">Bağımlı (tam olarak bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="57253-988">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-989">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-989">Applicable Attributes</span></span>

<span data-ttu-id="57253-990">**Referentialconstraint'teki** öğesi herhangi bir sayıda ek açıklama öznitelikleri (özel XML öznitelikleri) olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="57253-991">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-992">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="57253-993">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-993">Example</span></span>

<span data-ttu-id="57253-994">Aşağıdaki örnekte gösterildiği bir **Referentialconstraint'teki** öğe tanımının bir parçası olarak kullanılan **yayınladığı** ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="57253-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="57253-995">ReturnType (işlev) öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="57253-996">**ReturnType** kavramsal şema tanım dili (CSDL) (işlev) öğesinde bir Function öğesinde tanımlanan bir işlev için dönüş türü belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="57253-997">Bir işlevin dönüş türü de belirtilebilir bir **ReturnType** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57253-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="57253-998">Dönüş türleri herhangi olabilir **EdmSimpleType**, varlık türü, karmaşık tür, satır türü, başvuru türü veya bu tür bir koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="57253-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="57253-999">Bir işlevin dönüş türü ile belirtilebilir **türü** özniteliği **ReturnType** (işlev) öğesi veya şu alt öğelerden biri ile:</span><span class="sxs-lookup"><span data-stu-id="57253-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="57253-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="57253-1000">CollectionType</span></span>
-   <span data-ttu-id="57253-1001">referenceType</span><span class="sxs-lookup"><span data-stu-id="57253-1001">ReferenceType</span></span>
-   <span data-ttu-id="57253-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="57253-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="57253-1003">İşlev dönüş türü ile her ikisini de belirtirseniz, bir model doğrulama değil **türü** özniteliği **ReturnType** (işlev) öğe ve alt öğelerden biri.</span><span class="sxs-lookup"><span data-stu-id="57253-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="57253-1004">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-1004">Applicable Attributes</span></span>

<span data-ttu-id="57253-1005">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ReturnType** (işlev) öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="57253-1006">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-1006">Attribute Name</span></span> | <span data-ttu-id="57253-1007">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-1007">Is Required</span></span> | <span data-ttu-id="57253-1008">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="57253-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="57253-1009">**ReturnType**</span></span> | <span data-ttu-id="57253-1010">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1010">No</span></span>          | <span data-ttu-id="57253-1011">İşlev tarafından döndürülen tür.</span><span class="sxs-lookup"><span data-stu-id="57253-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-1012">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReturnType** (işlev) öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="57253-1013">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-1014">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-1015">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-1015">Example</span></span>

<span data-ttu-id="57253-1016">Aşağıdaki örnekte bir **işlevi** bir kitap, yazdırma rolünüzün yıl sayısını döndüren bir işlev tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="57253-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="57253-1017">Dönüş türü tarafından belirtilen Not **türü** özniteliği bir **ReturnType** (işlev) öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="57253-1018">ReturnType (Functionımport) öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="57253-1019">**ReturnType** kavramsal şema tanım dili (CSDL) (Functionımport) öğesinde bir Functionımport öğesinde tanımlanan bir işlev için dönüş türü belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="57253-1020">Bir işlevin dönüş türü de belirtilebilir bir **ReturnType** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57253-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="57253-1021">Dönüş türleri varlık türü, karmaşık tür herhangi bir koleksiyon olabilir veya **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="57253-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="57253-1022">Bir işlevin dönüş türü belirtilmiş **türü** özniteliği **ReturnType** (Functionımport) öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-1023">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-1023">Applicable Attributes</span></span>

<span data-ttu-id="57253-1024">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ReturnType** (Functionımport) öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="57253-1025">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-1025">Attribute Name</span></span> | <span data-ttu-id="57253-1026">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-1026">Is Required</span></span> | <span data-ttu-id="57253-1027">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-1028">**Türü**</span><span class="sxs-lookup"><span data-stu-id="57253-1028">**Type**</span></span>       | <span data-ttu-id="57253-1029">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1029">No</span></span>          | <span data-ttu-id="57253-1030">İşlevinin döndürdüğü türü.</span><span class="sxs-lookup"><span data-stu-id="57253-1030">The type that the function returns.</span></span> <span data-ttu-id="57253-1031">Değer, ComplexType, EntityType veya EDMSimpleType koleksiyonu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="57253-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="57253-1032">**EntitySet**</span></span>  | <span data-ttu-id="57253-1033">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1033">No</span></span>          | <span data-ttu-id="57253-1034">Varlık koleksiyonu işlevi döndürürse, türleri, değerini **EntitySet** koleksiyona ait olduğu varlık kümesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="57253-1035">Aksi takdirde, **EntitySet** özniteliği değil kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-1036">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReturnType** (Functionımport) öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="57253-1037">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-1038">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-1039">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-1039">Example</span></span>

<span data-ttu-id="57253-1040">Aşağıdaki örnekte bir **Functionımport** kitaplardan ve yayımcılar döndürür.</span><span class="sxs-lookup"><span data-stu-id="57253-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="57253-1041">İşlev iki sonucu kümesi ve bu nedenle iki döndürmediğine dikkat edin **ReturnType** (Functionımport) öğeleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="57253-1042">RowType öğesinin (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="57253-1043">A **RowType** kavramsal şema tanım dili (CSDL) öğesinde, bir parametre veya kavramsal modelde tanımlı bir işlevin dönüş türü olarak adlandırılmamış bir yapı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="57253-1044">A **RowType** aşağıdaki öğeleri alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="57253-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="57253-1045">CollectionType</span></span>
-   <span data-ttu-id="57253-1046">Parametre</span><span class="sxs-lookup"><span data-stu-id="57253-1046">Parameter</span></span>
-   <span data-ttu-id="57253-1047">ReturnType (işlev)</span><span class="sxs-lookup"><span data-stu-id="57253-1047">ReturnType (Function)</span></span>

<span data-ttu-id="57253-1048">A **RowType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-1049">Özellik (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="57253-1049">Property (one or more)</span></span>
-   <span data-ttu-id="57253-1050">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="57253-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-1051">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-1051">Applicable Attributes</span></span>

<span data-ttu-id="57253-1052">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **RowType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="57253-1053">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-1054">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="57253-1055">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-1055">Example</span></span>

<span data-ttu-id="57253-1056">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğe (belirtilmiş **RowType** öğesi).</span><span class="sxs-lookup"><span data-stu-id="57253-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="57253-1057">Şema öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="57253-1058">**Şema** öğenin kavramsal model tanımı kök öğesidir.</span><span class="sxs-lookup"><span data-stu-id="57253-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="57253-1059">Bu nesneleri, işlevleri ve kavramsal bir modeli olun kapsayıcıları için tanımlar içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="57253-1060">**Şema** öğesi sıfır veya daha fazla şu alt öğelerden birini içerebilir:</span><span class="sxs-lookup"><span data-stu-id="57253-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="57253-1061">kullanma</span><span class="sxs-lookup"><span data-stu-id="57253-1061">Using</span></span>
-   <span data-ttu-id="57253-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="57253-1062">EntityContainer</span></span>
-   <span data-ttu-id="57253-1063">entityType</span><span class="sxs-lookup"><span data-stu-id="57253-1063">EntityType</span></span>
-   <span data-ttu-id="57253-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="57253-1064">EnumType</span></span>
-   <span data-ttu-id="57253-1065">İlişkilendirme</span><span class="sxs-lookup"><span data-stu-id="57253-1065">Association</span></span>
-   <span data-ttu-id="57253-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="57253-1066">ComplexType</span></span>
-   <span data-ttu-id="57253-1067">İşlev</span><span class="sxs-lookup"><span data-stu-id="57253-1067">Function</span></span>

<span data-ttu-id="57253-1068">A **şema** öğesi sıfır veya bir ek açıklama öğeleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="57253-1069">**İşlevi** öğesi ve ek açıklama öğeleri yalnızca izin CSDL v2'de ve sonraki sürümler.</span><span class="sxs-lookup"><span data-stu-id="57253-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="57253-1070">**Şema** öğesini kullanan **Namespace** kavramsal modelde varlık türü, karmaşık türü ve ilişkisi nesneleri için ad alanını tanımlayan öznitelik.</span><span class="sxs-lookup"><span data-stu-id="57253-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="57253-1071">Ad alanı içinde hiçbir iki nesnenin aynı ada sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="57253-1072">Ad alanları, birden çok yayılabilir **şema** öğelerini ve birden çok .csdl dosyaları.</span><span class="sxs-lookup"><span data-stu-id="57253-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="57253-1073">Kavramsal model ad alanı XML ad alanından farklıdır **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="57253-1074">Kavramsal model ad alanı (tarafından tanımlandığı gibi **Namespace** özniteliği) ilişki türleri varlık türleri ve karmaşık türler için mantıksal bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="57253-1075">XML ad alanı (tarafından belirtilen **xmlns** özniteliği), bir **şema** öğedir alt öğeleri ve öznitelikleri için varsayılan ad alanı **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="57253-1076">XML ad alanları formun http://schemas.microsoft.com/ado/YYYY/MM/edm (burada YYYY ve MM yıl ve ay sırasıyla temsil eder) CSDL için ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="57253-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="57253-1077">Bu forma sahip ad alanları, özel öğeleri ve öznitelikleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-1078">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-1078">Applicable Attributes</span></span>

<span data-ttu-id="57253-1079">Aşağıdaki tabloda öznitelikleri açıklar uygulanabilir **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="57253-1080">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-1080">Attribute Name</span></span> | <span data-ttu-id="57253-1081">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-1081">Is Required</span></span> | <span data-ttu-id="57253-1082">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="57253-1083">**Namespace**</span></span>  | <span data-ttu-id="57253-1084">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1084">Yes</span></span>         | <span data-ttu-id="57253-1085">Kavramsal modelin ad alanı.</span><span class="sxs-lookup"><span data-stu-id="57253-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="57253-1086">Değerini **Namespace** özniteliği, bir türün tam adı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="57253-1087">Örneğin, bir **EntityType** adlı *müşteri* Simple.Example.Model ad alanınıza ve sonra tam adı olduğunu **EntityType** olduğu SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="57253-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="57253-1088">Aşağıdaki dize değeri olarak kullanılamaz **Namespace** özniteliği: **sistem**, **geçici**, veya **Edm**.</span><span class="sxs-lookup"><span data-stu-id="57253-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="57253-1089">Değeri **Namespace** öznitelik değeri olarak aynı olamaz **Namespace** SSDL şeması öğesindeki özniteliği.</span><span class="sxs-lookup"><span data-stu-id="57253-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="57253-1090">**Diğer ad**</span><span class="sxs-lookup"><span data-stu-id="57253-1090">**Alias**</span></span>      | <span data-ttu-id="57253-1091">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1091">No</span></span>          | <span data-ttu-id="57253-1092">Ad alanı adı yerine kullanılan tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="57253-1093">Örneğin, bir **EntityType** adlı *müşteri* Simple.Example.Model ad alanı ve değerini **diğer** özniteliği *modeli*, tam nitelikli adı olarak Model.Customer kullanabilirsiniz **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="57253-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="57253-1094">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="57253-1095">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-1096">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-1097">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-1097">Example</span></span>

<span data-ttu-id="57253-1098">Aşağıdaki örnekte gösterildiği bir **şema** öğesini içeren bir **EntityContainer** öğesinde, iki **EntityType** öğeleri ve bir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="57253-1099">TypeRef öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="57253-1100">**TypeRef** kavramsal şema tanım dili (CSDL) öğesinde türü adlı varolan bir başvuru sağlar.</span><span class="sxs-lookup"><span data-stu-id="57253-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="57253-1101">**TypeRef** bir işlev bir parametre veya dönüş türü bir koleksiyon olduğunu belirtmek için kullanılan CollectionType öğesinin alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="57253-1102">A **TypeRef** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="57253-1103">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="57253-1104">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-1105">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-1105">Applicable Attributes</span></span>

<span data-ttu-id="57253-1106">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **TypeRef** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="57253-1107">Unutmayın **DefaultValue**, **MaxLength**, **FixedLength**, **duyarlık**, **ölçek**,  **Unicode**, ve **harmanlama** öznitelikleri için uygun yalnızca **EDMSimpleTypes**.</span><span class="sxs-lookup"><span data-stu-id="57253-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="57253-1108">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="57253-1109">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-1109">Is Required</span></span> | <span data-ttu-id="57253-1110">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-1111">**Türü**</span><span class="sxs-lookup"><span data-stu-id="57253-1111">**Type**</span></span>                                                           | <span data-ttu-id="57253-1112">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1112">No</span></span>          | <span data-ttu-id="57253-1113">Başvurulan tür adı.</span><span class="sxs-lookup"><span data-stu-id="57253-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="57253-1114">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="57253-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="57253-1115">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1115">No</span></span>          | <span data-ttu-id="57253-1116">**Doğru** (varsayılan değer) veya **False** bağlı olup olmadığını özelliği null değeri olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="57253-1117">> CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="57253-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="57253-1118">**defaultValue**</span><span class="sxs-lookup"><span data-stu-id="57253-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="57253-1119">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1119">No</span></span>          | <span data-ttu-id="57253-1120">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="57253-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="57253-1121">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="57253-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="57253-1122">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1122">No</span></span>          | <span data-ttu-id="57253-1123">Özellik değeri en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="57253-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="57253-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="57253-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="57253-1125">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1125">No</span></span>          | <span data-ttu-id="57253-1126">**Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="57253-1127">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="57253-1127">**Precision**</span></span>                                                      | <span data-ttu-id="57253-1128">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1128">No</span></span>          | <span data-ttu-id="57253-1129">Özellik değerinin kesinliği.</span><span class="sxs-lookup"><span data-stu-id="57253-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="57253-1130">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="57253-1130">**Scale**</span></span>                                                          | <span data-ttu-id="57253-1131">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1131">No</span></span>          | <span data-ttu-id="57253-1132">Özellik değerinin ölçek.</span><span class="sxs-lookup"><span data-stu-id="57253-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="57253-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="57253-1133">**SRID**</span></span>                                                           | <span data-ttu-id="57253-1134">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1134">No</span></span>          | <span data-ttu-id="57253-1135">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="57253-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="57253-1136">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="57253-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="57253-1137">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="57253-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="57253-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="57253-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="57253-1139">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1139">No</span></span>          | <span data-ttu-id="57253-1140">**Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="57253-1141">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="57253-1141">**Collation**</span></span>                                                      | <span data-ttu-id="57253-1142">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1142">No</span></span>          | <span data-ttu-id="57253-1143">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="57253-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="57253-1144">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **CollectionType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="57253-1145">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-1146">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-1147">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-1147">Example</span></span>

<span data-ttu-id="57253-1148">Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. **TypeRef** öğesi (alt öğesi olarak bir **CollectionType** öğesi) işlev koleksiyonunu kabul belirtmek için  **Departman** varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="57253-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="57253-1149">Öğesi (CSDL) kullanma</span><span class="sxs-lookup"><span data-stu-id="57253-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="57253-1150">**Kullanma** kavramsal şema tanım dili (CSDL) öğe farklı bir ad alanında bulunan bir kavramsal model içeriğini içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="57253-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="57253-1151">Değerini ayarlayarak **Namespace** özniteliği başvurabilirsiniz varlık türleri ve karmaşık türler başka bir kavramsal modelde tanımlı ilişki türleri için.</span><span class="sxs-lookup"><span data-stu-id="57253-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="57253-1152">Birden fazla **kullanma** öğesi alt öğesi olabilir bir **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="57253-1153">**Kullanma** CSDL öğesinde tıpkı çalışmaz bir **kullanarak** bir programlama dili deyimi.</span><span class="sxs-lookup"><span data-stu-id="57253-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="57253-1154">Bir ad alanı ile alarak bir **kullanarak** deyimi bir programlama dili, özgün ad alanındaki nesneleri etkilemez.</span><span class="sxs-lookup"><span data-stu-id="57253-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="57253-1155">CSDL bir içeri aktarılan ad alanı özgün ad alanında bir varlık türünden türetilmiş bir varlık türü içerebilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="57253-1156">Bu, özgün ad alanında bildirilen varlık kümeleri etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="57253-1157">**Kullanma** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="57253-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="57253-1158">Belgeleri (izin verilen sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="57253-1159">Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="57253-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="57253-1160">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="57253-1160">Applicable Attributes</span></span>

<span data-ttu-id="57253-1161">Aşağıdaki tabloda öznitelikleri açıklar uygulanabilir **kullanma** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="57253-1162">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="57253-1162">Attribute Name</span></span> | <span data-ttu-id="57253-1163">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="57253-1163">Is Required</span></span> | <span data-ttu-id="57253-1164">Değer</span><span class="sxs-lookup"><span data-stu-id="57253-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="57253-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="57253-1165">**Namespace**</span></span>  | <span data-ttu-id="57253-1166">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1166">Yes</span></span>         | <span data-ttu-id="57253-1167">İçeri aktarılan ad alanının adı.</span><span class="sxs-lookup"><span data-stu-id="57253-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="57253-1168">**Diğer ad**</span><span class="sxs-lookup"><span data-stu-id="57253-1168">**Alias**</span></span>      | <span data-ttu-id="57253-1169">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1169">Yes</span></span>         | <span data-ttu-id="57253-1170">Ad alanı adı yerine kullanılan tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="57253-1171">Bu öznitelik gerekli olsa da, bunu değil, ad alanı adı yerine nesne adlarını nitelemek için kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="57253-1172">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **kullanma** öğesi.</span><span class="sxs-lookup"><span data-stu-id="57253-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="57253-1173">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="57253-1174">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="57253-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="57253-1175">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-1175">Example</span></span>

<span data-ttu-id="57253-1176">Aşağıdaki örnek, gösterir **kullanma** öğesi bir ad alanı içeri aktarmak için kullanılan başka bir yerde tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="57253-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="57253-1177">Unutmayın ad alanı için **şema** gösterilen öğe `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="57253-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="57253-1178">`Address` Özelliği `Publisher` **EntityType** tanımlanan karmaşık bir türdür `ExtendedBooksModel` ad alanı (içeri **kullanma** öğesi).</span><span class="sxs-lookup"><span data-stu-id="57253-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="57253-1179">Ek açıklama öznitelikleri (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="57253-1180">Ek açıklama özniteliklerinde kavramsal şema tanım dili (CSDL) kavramsal model özel XML öznitelikleri ' dir.</span><span class="sxs-lookup"><span data-stu-id="57253-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="57253-1181">Geçerli XML yapısına sahip olmaya ek olarak, aşağıdaki ek açıklama özniteliklerinin doğru olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="57253-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="57253-1182">Ek açıklama öznitelikleri CSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="57253-1183">Ek açıklama birden fazla öznitelik verilen bir CSDL öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="57253-1184">Her iki ek açıklama özniteliklerin tam adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="57253-1185">Ek açıklama öznitelikleri, kavramsal modeldeki öğeleri hakkında ek meta verilerini sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="57253-1186">Ek açıklama öğesinde bulunan meta veriler, çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="57253-1187">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-1187">Example</span></span>

<span data-ttu-id="57253-1188">Aşağıdaki örnekte gösterildiği bir **EntityType** bir ek açıklama özniteliği olan öğe (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="57253-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="57253-1189">Örnek ayrıca varlık türü öğeye uygulanan bir ek açıklama öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="57253-1189">The example also shows an annotation element applied to the entity type element.</span></span>

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
 

<span data-ttu-id="57253-1190">Aşağıdaki kod ek açıklama özniteliği meta verilerini alır ve konsola yazar:</span><span class="sxs-lookup"><span data-stu-id="57253-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="57253-1191">Yukarıdaki kod olduğunu varsayar `School.csdl` dosya projenin çıkış dizinine ve aşağıdaki eklediğiniz `Imports` ve `Using` projenize ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="57253-1192">Ek açıklama öğelerinin (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="57253-1193">Özel XML öğeleri kavramsal modeldeki kavramsal şema tanım dili (CSDL) içinde ek açıklama öğeleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="57253-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="57253-1194">Geçerli XML yapısına sahip olmaya ek olarak, aşağıdaki ek açıklama öğeleri doğru olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="57253-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="57253-1195">Ek açıklama öğelerinin CSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="57253-1196">Birden çok ek açıklama öğesi, belirli bir CSDL öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="57253-1197">Her iki ek açıklama öğelerinin tam adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="57253-1198">Diğer tüm alt öğeleri verilen CSDL öğenin sonra ek açıklama öğelerinin görünmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="57253-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="57253-1199">Ek açıklama öğelerinin kavramsal modeldeki öğeleri hakkında ek meta verilerini sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="57253-1200">.NET Framework sürüm 4 ile başlayarak, ek açıklama öğesinde bulunan meta veriler çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="57253-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="57253-1201">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-1201">Example</span></span>

<span data-ttu-id="57253-1202">Aşağıdaki örnekte gösterildiği bir **EntityType** öğesi ile bir ek açıklama öğesi (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="57253-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="57253-1203">Örnek ayrıca varlık türü öğeye uygulanan bir ek açıklama özniteliği gösterir.</span><span class="sxs-lookup"><span data-stu-id="57253-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

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
 

<span data-ttu-id="57253-1204">Aşağıdaki kod, ek açıklama öğesinde bulunan meta verileri alır ve konsola yazar:</span><span class="sxs-lookup"><span data-stu-id="57253-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="57253-1205">Yukarıdaki kod School.csdl dosya projenin çıkış dizinine olduğunu ve aşağıdaki eklediğinizi varsayar `Imports` ve `Using` projenize ifadeleri:</span><span class="sxs-lookup"><span data-stu-id="57253-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="57253-1206">Kavramsal Model türleri (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="57253-1207">Kavramsal şema tanım dili (CSDL) adı verilen soyut temel veri türleri kümesi destekler **EDMSimpleTypes**, kavramsal modelde özellikleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="57253-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="57253-1208">**EDMSimpleTypes** proxy'ler ilgili daha fazla bilgi için depolama veya barındırma ortamında desteklenen temel veri türleri.</span><span class="sxs-lookup"><span data-stu-id="57253-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="57253-1209">Aşağıdaki tabloda, CSDL tarafından desteklenen temel veri türlerini listeler.</span><span class="sxs-lookup"><span data-stu-id="57253-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="57253-1210">Tabloda ayrıca her uygulanan özellikleri listeler **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="57253-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="57253-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="57253-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="57253-1212">Açıklama</span><span class="sxs-lookup"><span data-stu-id="57253-1212">Description</span></span>                                                | <span data-ttu-id="57253-1213">Geçerli modelleri</span><span class="sxs-lookup"><span data-stu-id="57253-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="57253-1214">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="57253-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="57253-1215">İkili veriler içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="57253-1216">MaxLength, FixedLength, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="57253-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="57253-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="57253-1218">Değeri içeren **true** veya **false**.</span><span class="sxs-lookup"><span data-stu-id="57253-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="57253-1219">Boş değer atanabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="57253-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="57253-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="57253-1221">İmzalanmamış 8 bit tam sayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="57253-1222">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1223">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="57253-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="57253-1224">Tarih ve saati temsil eder.</span><span class="sxs-lookup"><span data-stu-id="57253-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="57253-1225">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="57253-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="57253-1227">Bir tarih ve saat olarak GMT'den dakikalar içinde bir uzaklık içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="57253-1228">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="57253-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="57253-1230">Sabit kesinlik ve ölçek ile sayısal bir değer içeriyor.</span><span class="sxs-lookup"><span data-stu-id="57253-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="57253-1231">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="57253-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="57253-1233">Kayan nokta ile 15 basamaklı duyarlık sayı içerir</span><span class="sxs-lookup"><span data-stu-id="57253-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="57253-1234">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="57253-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="57253-1236">Kayan noktalı sayı 7 basamaklı duyarlık içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="57253-1237">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1238">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="57253-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="57253-1239">16 baytlık benzersiz bir tanımlayıcı içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="57253-1240">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="57253-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="57253-1242">İşaretli 16 bit tam sayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="57253-1243">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1244">**EDM.Int32**</span><span class="sxs-lookup"><span data-stu-id="57253-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="57253-1245">İşaretli 32-bit tamsayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="57253-1246">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1247">**EDM.Int64**</span><span class="sxs-lookup"><span data-stu-id="57253-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="57253-1248">Bir 64-bit işaretli tamsayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="57253-1249">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="57253-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="57253-1251">İşaretli 8 bit tam sayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="57253-1252">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="57253-1253">**Edm.String**</span></span>                   | <span data-ttu-id="57253-1254">Karakter verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1254">Contains character data.</span></span>                                   | <span data-ttu-id="57253-1255">Varsayılan Unicode, FixedLength, MaxLength, harmanlaması, boş değer atanabilir, duyarlık</span><span class="sxs-lookup"><span data-stu-id="57253-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="57253-1256">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="57253-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="57253-1257">Günün bir saati içerir.</span><span class="sxs-lookup"><span data-stu-id="57253-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="57253-1258">Duyarlık, null, varsayılan</span><span class="sxs-lookup"><span data-stu-id="57253-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="57253-1259">**Edm.Geography**</span><span class="sxs-lookup"><span data-stu-id="57253-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="57253-1260">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="57253-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="57253-1262">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="57253-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="57253-1264">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1265">**Edm.GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="57253-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="57253-1266">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="57253-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="57253-1268">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="57253-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="57253-1270">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="57253-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="57253-1272">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="57253-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="57253-1274">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1275">**Edm.Geometry**</span><span class="sxs-lookup"><span data-stu-id="57253-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="57253-1276">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="57253-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="57253-1278">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="57253-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="57253-1280">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="57253-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="57253-1282">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="57253-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="57253-1284">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="57253-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="57253-1286">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="57253-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="57253-1288">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="57253-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="57253-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="57253-1290">Boş değer atanabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="57253-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="57253-1291">Modelleri (CSDL)</span><span class="sxs-lookup"><span data-stu-id="57253-1291">Facets (CSDL)</span></span>

<span data-ttu-id="57253-1292">Kavramsal şema tanım dili (CSDL), modelleri özelliklerini varlık türlerinde ve karmaşık türlerde kısıtlamaları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="57253-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="57253-1293">Modelleri, XML öznitelikleri aşağıdaki CSDL öğeleri olarak görünür:</span><span class="sxs-lookup"><span data-stu-id="57253-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="57253-1294">Özellik</span><span class="sxs-lookup"><span data-stu-id="57253-1294">Property</span></span>
-   <span data-ttu-id="57253-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="57253-1295">TypeRef</span></span>
-   <span data-ttu-id="57253-1296">Parametre</span><span class="sxs-lookup"><span data-stu-id="57253-1296">Parameter</span></span>

<span data-ttu-id="57253-1297">Aşağıdaki tabloda CSDL desteklenen özellikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="57253-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="57253-1298">Tüm özellikleri isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="57253-1298">All facets are optional.</span></span> <span data-ttu-id="57253-1299">Aşağıda listelenen bazı modeller bir kavramsal modelde bir veritabanı oluşturma varlık çerçevesi tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="57253-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="57253-1300">Kavramsal modelde veri türleri hakkında daha fazla bilgi için bkz: kavramsal Model türleri (CSDL).</span><span class="sxs-lookup"><span data-stu-id="57253-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="57253-1301">modeli</span><span class="sxs-lookup"><span data-stu-id="57253-1301">Facet</span></span>               | <span data-ttu-id="57253-1302">Açıklama</span><span class="sxs-lookup"><span data-stu-id="57253-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="57253-1303">Uygulandığı öğe:</span><span class="sxs-lookup"><span data-stu-id="57253-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="57253-1304">Veritabanı oluşturmak için kullanılan</span><span class="sxs-lookup"><span data-stu-id="57253-1304">Used for the database generation</span></span> | <span data-ttu-id="57253-1305">Çalışma zamanı tarafından kullanılan</span><span class="sxs-lookup"><span data-stu-id="57253-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="57253-1306">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="57253-1306">**Collation**</span></span>       | <span data-ttu-id="57253-1307">Harmanlama dizisi (veya sıralama) yapılırken kullanılacak karşılaştırma gerçekleştirme ve özellik değerleri üzerinde işlem sıralama belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="57253-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="57253-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="57253-1309">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1309">Yes</span></span>                              | <span data-ttu-id="57253-1310">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1310">No</span></span>                  |
| <span data-ttu-id="57253-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="57253-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="57253-1312">Özelliğinin değeri için iyimser eşzamanlılık denetimlerinin kullanılması gerektiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="57253-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="57253-1313">Tüm **EDMSimpleType** özellikleri</span><span class="sxs-lookup"><span data-stu-id="57253-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="57253-1314">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1314">No</span></span>                               | <span data-ttu-id="57253-1315">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1315">Yes</span></span>                 |
| <span data-ttu-id="57253-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="57253-1316">**Default**</span></span>         | <span data-ttu-id="57253-1317">Örnek oluşturma sırasında herhangi bir değer sağlanmazsa, özelliğin varsayılan değerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="57253-1318">Tüm **EDMSimpleType** özellikleri</span><span class="sxs-lookup"><span data-stu-id="57253-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="57253-1319">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1319">Yes</span></span>                              | <span data-ttu-id="57253-1320">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1320">Yes</span></span>                 |
| <span data-ttu-id="57253-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="57253-1321">**FixedLength**</span></span>     | <span data-ttu-id="57253-1322">Özellik değerinin uzunluğu değişebilir olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="57253-1323">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="57253-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="57253-1324">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1324">Yes</span></span>                              | <span data-ttu-id="57253-1325">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1325">No</span></span>                  |
| <span data-ttu-id="57253-1326">**maxLength**</span><span class="sxs-lookup"><span data-stu-id="57253-1326">**MaxLength**</span></span>       | <span data-ttu-id="57253-1327">Özellik değeri en büyük uzunluğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="57253-1328">**Edm.Binary**, **Edm.String**</span><span class="sxs-lookup"><span data-stu-id="57253-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="57253-1329">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1329">Yes</span></span>                              | <span data-ttu-id="57253-1330">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1330">No</span></span>                  |
| <span data-ttu-id="57253-1331">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="57253-1331">**Nullable**</span></span>        | <span data-ttu-id="57253-1332">Özelliğine sahip olup olmadığını belirten bir **null** değeri.</span><span class="sxs-lookup"><span data-stu-id="57253-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="57253-1333">Tüm **EDMSimpleType** özellikleri</span><span class="sxs-lookup"><span data-stu-id="57253-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="57253-1334">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1334">Yes</span></span>                              | <span data-ttu-id="57253-1335">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1335">Yes</span></span>                 |
| <span data-ttu-id="57253-1336">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="57253-1336">**Precision**</span></span>       | <span data-ttu-id="57253-1337">Tür özellikleri için **ondalık**, bir özellik değeri olabilir basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="57253-1338">Tür özellikleri için **zaman**, **DateTime**, ve **DateTimeOffset**, özellik değerinin saniye kesirli kısmını için basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="57253-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="57253-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="57253-1340">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1340">Yes</span></span>                              | <span data-ttu-id="57253-1341">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1341">No</span></span>                  |
| <span data-ttu-id="57253-1342">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="57253-1342">**Scale**</span></span>           | <span data-ttu-id="57253-1343">Özellik değeri ondalık noktasının sağındaki basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="57253-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="57253-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="57253-1345">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1345">Yes</span></span>                              | <span data-ttu-id="57253-1346">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1346">No</span></span>                  |
| <span data-ttu-id="57253-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="57253-1347">**SRID**</span></span>            | <span data-ttu-id="57253-1348">Uzamsal sistem başvuru sistemi kimliğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="57253-1349">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="57253-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="57253-1350">**Edm.Geography Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="57253-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="57253-1351">Hayır</span><span class="sxs-lookup"><span data-stu-id="57253-1351">No</span></span>                               | <span data-ttu-id="57253-1352">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1352">Yes</span></span>                 |
| <span data-ttu-id="57253-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="57253-1353">**Unicode**</span></span>         | <span data-ttu-id="57253-1354">Özellik değeri Unicode olarak mi depolanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="57253-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="57253-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="57253-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="57253-1356">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1356">Yes</span></span>                              | <span data-ttu-id="57253-1357">Evet</span><span class="sxs-lookup"><span data-stu-id="57253-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="57253-1358">Veritabanı Oluştur Sihirbazı'nı bir veritabanı kavramsal bir modeli oluşturulurken değerini tanıyacağınız **StoreGeneratedPattern** özniteliği bir **özelliği** aşağıdaki ise öğe ad alanı: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="57253-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="57253-1359">Özniteliği için desteklenen değerler şunlardır: **kimlik** ve **hesaplanan**.</span><span class="sxs-lookup"><span data-stu-id="57253-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="57253-1360">Değerini **kimlik** veritabanında oluşturulan bir kimlik değeri olan bir veritabanı sütunu üretecektir.</span><span class="sxs-lookup"><span data-stu-id="57253-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="57253-1361">Değerini **hesaplanan** veritabanında hesaplanan bir değere sahip bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="57253-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="57253-1362">Örnek</span><span class="sxs-lookup"><span data-stu-id="57253-1362">Example</span></span>

<span data-ttu-id="57253-1363">Aşağıdaki örnek, bir varlık türünün özelliklerine uygulanan özellikleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="57253-1363">The following example shows facets applied to the properties of an entity type:</span></span>

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
