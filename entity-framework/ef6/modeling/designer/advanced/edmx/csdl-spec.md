---
title: CSDL belirtimi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182592"
---
# <a name="csdl-specification"></a><span data-ttu-id="fb50d-102">CSDL Belirtimi</span><span class="sxs-lookup"><span data-stu-id="fb50d-102">CSDL Specification</span></span>
<span data-ttu-id="fb50d-103">Kavramsal şema tanım dili (CSDL), veri temelli bir uygulamanın kavramsal modelini oluşturan varlıkları, ilişkileri ve işlevleri açıklayan XML tabanlı bir dildir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="fb50d-104">Bu kavramsal model Entity Framework veya WCF Veri Hizmetleri tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="fb50d-105">CSDL ile açıklanan meta veriler, kavramsal modelde tanımlanan varlıkları ve ilişkileri bir veri kaynağıyla eşlemek için Entity Framework tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="fb50d-106">Daha fazla bilgi için bkz. [SSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) ve [MSL belirtimi](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="fb50d-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="fb50d-107">CSDL, Entity Framework Varlık Veri Modeli uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="fb50d-108">Entity Framework bir uygulamada, kavramsal model meta verileri bir. csdl dosyasından (CSDL içinde yazılmış) System. Data. Metadata. Edm. EdmItemCollection örneğine yüklenir ve içindeki yöntemler kullanılarak erişilebilir. System. Data. Metadata. Edm. MetadataWorkspace sınıfı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="fb50d-109">Entity Framework, sorguları kavramsal modele veri kaynağına özgü komutlara dönüştürmek için kavramsal model meta verilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="fb50d-110">EF Designer, kavramsal model bilgilerini tasarım zamanında bir. edmx dosyasında depolar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="fb50d-111">Yapı zamanında EF Designer, çalışma zamanında Entity Framework gereken. csdl dosyasını oluşturmak için bir. edmx dosyasındaki bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="fb50d-112">CSDL sürümleri, XML ad alanları ile farklılaştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="fb50d-113">CSDL sürümü</span><span class="sxs-lookup"><span data-stu-id="fb50d-113">CSDL Version</span></span> | <span data-ttu-id="fb50d-114">XML ad alanı</span><span class="sxs-lookup"><span data-stu-id="fb50d-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="fb50d-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="fb50d-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="fb50d-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="fb50d-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="fb50d-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="fb50d-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="fb50d-118">Association öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-118">Association Element (CSDL)</span></span>

<span data-ttu-id="fb50d-119">**İlişkilendirme** öğesi iki varlık türü arasındaki ilişkiyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="fb50d-120">Bir ilişki, ilişkiye dahil olan varlık türlerini ve ilişkinin her ucunda çoğulluk olarak bilinen varlık türlerinin olası sayısını belirtmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="fb50d-121">Bir ilişki ucunun çoğulluğu bir (1), sıfır veya bir (0.. 1) veya çok (\*) bir değere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="fb50d-122">Bu bilgiler iki alt End öğesinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="fb50d-123">Bir ilişkinin bir sonundaki varlık türü örneklerine, bir varlık türü üzerinde gösterilmeleri durumunda gezinti özellikleri veya yabancı anahtarlar üzerinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="fb50d-124">Bir uygulamada, bir ilişkinin örneği varlık türü örnekleri arasındaki belirli bir ilişkilendirmeyi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fb50d-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="fb50d-125">İlişki örnekleri bir ilişki kümesinde mantıksal olarak gruplandırılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="fb50d-126">Bir **ilişkilendirme** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-127">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-128">End (tam olarak 2 öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="fb50d-129">ReferentialConstraint (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-130">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-131">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-131">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-132">Aşağıdaki tabloda **ilişkilendirme** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="fb50d-133">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-133">Attribute Name</span></span> | <span data-ttu-id="fb50d-134">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-134">Is Required</span></span> | <span data-ttu-id="fb50d-135">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="fb50d-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-136">**Name**</span></span>       | <span data-ttu-id="fb50d-137">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-137">Yes</span></span>         | <span data-ttu-id="fb50d-138">İlişkilendirmenin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-139">**İlişkilendirme** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="fb50d-140">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-141">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-142">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-142">Example</span></span>

<span data-ttu-id="fb50d-143">Aşağıdaki örnek, **Müşteri** ve **sipariş** varlık türlerinde yabancı anahtarlar gösterilmediği zaman **CustomerOrders** ilişkilendirmesini tanımlayan bir **ilişki** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="fb50d-144">İlişkilendirmenin her bir **ucunun** **çoğulluğu** değeri, bir **müşteriyle**birçok **siparişin** Ilişkilendirilemeyeceğini gösterir, ancak bir **siparişle**yalnızca bir **Müşteri** ilişkilendirilebilen anlamına gelebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="fb50d-145">Ayrıca, **OnDelete** öğesi belirli bir **müşteriyle** ilgili olan ve ObjectContext 'e yüklenmiş tüm **siparişlerin** , **Müşteri** silinirse silinecek olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="fb50d-146">Aşağıdaki örnek, **Müşteri** ve **sipariş** varlık türlerinde yabancı anahtarlar açık olduğunda **CustomerOrders** ilişkilendirmesini tanımlayan bir **ilişki** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="fb50d-147">Yabancı anahtarlar kullanıma sunulduğunda, varlıklar arasındaki ilişki bir **ReferentialConstraint** öğesiyle yönetilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="fb50d-148">Bu ilişkilendirmeyi veri kaynağıyla eşlemek için karşılık gelen bir AssociationSetMapping öğesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="fb50d-149">AssociationSet öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="fb50d-150">Kavramsal şema tanım dili (CSDL) içindeki **AssociationSet** öğesi, aynı türde ilişki örnekleri için mantıksal bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="fb50d-151">Bir ilişki kümesi, ilişki örneklerinin bir veri kaynağıyla eşleştiribilecekleri şekilde gruplandırılması için bir tanım sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="fb50d-152">**AssociationSet** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-153">Belgeler (sıfır veya bir öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="fb50d-154">End (tam olarak iki öğe gereklidir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="fb50d-155">Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="fb50d-156">**Association** özniteliği bir ilişki kümesinin içerdiği ilişkilendirmenin türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="fb50d-157">Bir ilişki kümesinin uçlarını oluşturan varlık kümeleri, tam olarak iki alt **End** öğesi ile belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-158">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-158">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-159">Aşağıdaki tabloda **AssociationSet** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="fb50d-160">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-160">Attribute Name</span></span>  | <span data-ttu-id="fb50d-161">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-161">Is Required</span></span> | <span data-ttu-id="fb50d-162">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-163">**Name**</span></span>        | <span data-ttu-id="fb50d-164">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-164">Yes</span></span>         | <span data-ttu-id="fb50d-165">Varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-165">The name of the entity set.</span></span> <span data-ttu-id="fb50d-166">**Name** özniteliğinin değeri **Association** özniteliğinin değeriyle aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="fb50d-167">**İlişkilendirme**</span><span class="sxs-lookup"><span data-stu-id="fb50d-167">**Association**</span></span> | <span data-ttu-id="fb50d-168">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-168">Yes</span></span>         | <span data-ttu-id="fb50d-169">İlişki kümesinin örnekleri içerdiği ilişkilendirmenin tam nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="fb50d-170">İlişki, ilişki kümesiyle aynı ad alanında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-171">**AssociationSet** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="fb50d-172">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-173">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-174">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-174">Example</span></span>

<span data-ttu-id="fb50d-175">Aşağıdaki örnek, iki **AssociationSet** öğesiyle bir **EntityContainer** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="fb50d-176">CollectionType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="fb50d-177">Kavramsal şema tanım dili (CSDL) içindeki **CollectionType** öğesi, bir işlev parametresi veya işlev dönüş türünün bir koleksiyon olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="fb50d-178">**CollectionType** öğesi, Parameter öğesinin veya ReturnType (Function) öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="fb50d-179">Koleksiyon türü, **Type** özniteliği ya da aşağıdaki alt öğelerinden biri kullanılarak belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="fb50d-180">**Türünde**</span><span class="sxs-lookup"><span data-stu-id="fb50d-180">**CollectionType**</span></span>
-   <span data-ttu-id="fb50d-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="fb50d-181">ReferenceType</span></span>
-   <span data-ttu-id="fb50d-182">RowType</span><span class="sxs-lookup"><span data-stu-id="fb50d-182">RowType</span></span>
-   <span data-ttu-id="fb50d-183">Değerini</span><span class="sxs-lookup"><span data-stu-id="fb50d-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-184">Bir koleksiyon türünün hem **tür** özniteliği hem de bir alt öğe ile belirtilmesi durumunda model doğrulanmaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-185">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-185">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-186">Aşağıdaki tabloda, **CollectionType** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="fb50d-187">**DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **UNICODE**ve **harmanlama** özniteliklerinin yalnızca **edmsimpletypes**koleksiyonları için geçerli olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fb50d-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="fb50d-188">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-188">Attribute Name</span></span>                                                          | <span data-ttu-id="fb50d-189">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-189">Is Required</span></span> | <span data-ttu-id="fb50d-190">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-191">**Tür**</span><span class="sxs-lookup"><span data-stu-id="fb50d-191">**Type**</span></span>                                                                | <span data-ttu-id="fb50d-192">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-192">No</span></span>          | <span data-ttu-id="fb50d-193">Koleksiyonun türü.</span><span class="sxs-lookup"><span data-stu-id="fb50d-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="fb50d-194">**Yapılamaz**</span><span class="sxs-lookup"><span data-stu-id="fb50d-194">**Nullable**</span></span>                                                            | <span data-ttu-id="fb50d-195">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-195">No</span></span>          | <span data-ttu-id="fb50d-196">**True** (varsayılan değer) veya özelliğin NULL değere sahip olmasına bağlı olarak **false** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="fb50d-197">CSDL V1 >, karmaşık bir tür özelliği `Nullable="False"` olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="fb50d-198">**Değerinin**</span><span class="sxs-lookup"><span data-stu-id="fb50d-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="fb50d-199">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-199">No</span></span>          | <span data-ttu-id="fb50d-200">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="fb50d-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="fb50d-201">**'In**</span><span class="sxs-lookup"><span data-stu-id="fb50d-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="fb50d-202">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-202">No</span></span>          | <span data-ttu-id="fb50d-203">Özellik değerinin uzunluk üst sınırı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="fb50d-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="fb50d-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="fb50d-205">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-205">No</span></span>          | <span data-ttu-id="fb50d-206">Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="fb50d-207">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="fb50d-207">**Precision**</span></span>                                                           | <span data-ttu-id="fb50d-208">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-208">No</span></span>          | <span data-ttu-id="fb50d-209">Özellik değerinin duyarlığı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="fb50d-210">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="fb50d-210">**Scale**</span></span>                                                               | <span data-ttu-id="fb50d-211">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-211">No</span></span>          | <span data-ttu-id="fb50d-212">Özellik değerinin ölçeği.</span><span class="sxs-lookup"><span data-stu-id="fb50d-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="fb50d-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="fb50d-213">**SRID**</span></span>                                                                | <span data-ttu-id="fb50d-214">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-214">No</span></span>          | <span data-ttu-id="fb50d-215">Uzamsal sistem başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="fb50d-216">Yalnızca uzamsal türlerin özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="fb50d-217">   Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="fb50d-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="fb50d-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="fb50d-218">**Unicode**</span></span>                                                             | <span data-ttu-id="fb50d-219">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-219">No</span></span>          | <span data-ttu-id="fb50d-220">Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="fb50d-221">**Mediğinden**</span><span class="sxs-lookup"><span data-stu-id="fb50d-221">**Collation**</span></span>                                                           | <span data-ttu-id="fb50d-222">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-222">No</span></span>          | <span data-ttu-id="fb50d-223">Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="fb50d-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="fb50d-224">**CollectionType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="fb50d-225">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-226">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-227">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-227">Example</span></span>

<span data-ttu-id="fb50d-228">Aşağıdaki örnek, işlevin bir **kişi** varlık türleri koleksiyonunu ( **ElementType** özniteliğiyle belirtilen şekilde) döndürdüğünü belirtmek için bir **CollectionType** öğesi kullanan model tanımlı bir işlevi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="fb50d-229">Aşağıdaki örnek, işlevin satır koleksiyonunu ( **RowType** öğesinde belirtilen şekilde) döndürdüğünü belirtmek Için bir **CollectionType** öğesi kullanan model tanımlı bir işlevi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="fb50d-230">Aşağıdaki örnek, işlevin bir **Departman** varlık türleri koleksiyonu olarak kabul ettiğini belirtmek için **CollectionType** öğesini kullanan model tanımlı bir işlevi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="fb50d-231">ComplexType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="fb50d-232">Bir **complexType** öğesi, **edmsimpletype** özelliklerinden veya diğer karmaşık türlerden oluşan bir veri yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="fb50d-233">  Karmaşık tür bir varlık türünün özelliği ya da başka bir karmaşık tür olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="fb50d-234">Karmaşık bir tür, verileri tanımlayan bir varlık türüne benzerdir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="fb50d-235">Ancak, karmaşık türler ve varlık türleri arasında bazı önemli farklılıklar vardır:</span><span class="sxs-lookup"><span data-stu-id="fb50d-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="fb50d-236">Karmaşık türlerin kimlikleri (veya anahtarları) yoktur ve bu nedenle bağımsız olarak bulunamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="fb50d-237">Karmaşık türler yalnızca varlık türlerinin veya diğer karmaşık türlerin özellikleri olarak bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="fb50d-238">Karmaşık türler ilişkilendirmelere katılamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="fb50d-239">İlişkinin sonu ne bir karmaşık tür olamaz, bu nedenle gezinti özellikleri karmaşık türler için tanımlanamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="fb50d-240">Karmaşık tür özelliği null değere sahip olamaz, ancak karmaşık bir türdeki skalar özellikler her biri null olarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="fb50d-241">Bir **complexType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-242">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-243">Özellik (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="fb50d-244">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="fb50d-245">Aşağıdaki tabloda, **complexType** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="fb50d-246">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="fb50d-247">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-247">Is Required</span></span> | <span data-ttu-id="fb50d-248">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-249">Name</span><span class="sxs-lookup"><span data-stu-id="fb50d-249">Name</span></span>                                                                                                           | <span data-ttu-id="fb50d-250">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-250">Yes</span></span>         | <span data-ttu-id="fb50d-251">Karmaşık türün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-251">The name of the complex type.</span></span> <span data-ttu-id="fb50d-252">Karmaşık bir türün adı, modelin kapsamı içinde olan başka bir karmaşık türün, varlık türünün veya ilişkilendirmenin adı ile aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="fb50d-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="fb50d-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="fb50d-254">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-254">No</span></span>          | <span data-ttu-id="fb50d-255">Tanımlanmakta olan karmaşık türün temel türü olan başka bir karmaşık türün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="fb50d-256">> Bu öznitelik CSDL v1 'de geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="fb50d-257">Karmaşık türlerin devralınması bu sürümde desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="fb50d-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="fb50d-258">Özet</span><span class="sxs-lookup"><span data-stu-id="fb50d-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="fb50d-259">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-259">No</span></span>          | <span data-ttu-id="fb50d-260">Karmaşık türün soyut bir tür olmasına bağlı olarak **doğru** veya **yanlış** (varsayılan değer).</span><span class="sxs-lookup"><span data-stu-id="fb50d-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="fb50d-261">> Bu öznitelik CSDL v1 'de geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="fb50d-262">Bu sürümdeki karmaşık türler soyut tür olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="fb50d-263">**ComplexType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="fb50d-264">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-265">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-266">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-266">Example</span></span>

<span data-ttu-id="fb50d-267">Aşağıdaki örnek, **Edmsimpletype** özellikleri **StreetAddress**, **City**, **stateoreyalet**, **ülke**ve **PostaKodu**olan karmaşık bir tür, **Adres**gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="fb50d-268">Karmaşık tür **adresini** (yukarıdaki) bir varlık türünün özelliği olarak tanımlamak için varlık türü tanımında Özellik türünü bildirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="fb50d-269">Aşağıdaki örnek, **Adres** özelliğini bir varlık türü (**Yayımcı**) üzerinde karmaşık bir tür olarak göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="fb50d-270">DefiningExpression öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="fb50d-271">Kavramsal şema tanım dili (CSDL) içindeki **DefiningExpression** öğesi, kavramsal modeldeki bir işlevi tanımlayan bir Entity SQL ifadesi içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="fb50d-272">Doğrulama amacıyla, bir **DefiningExpression** öğesi rastgele içerik içerebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="fb50d-273">Ancak, bir **DefiningExpression** öğesi geçerli Entity SQL içermiyorsa, Entity Framework çalışma zamanında bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-274">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-274">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-275">**DefiningExpression** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="fb50d-276">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-277">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="fb50d-278">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-278">Example</span></span>

<span data-ttu-id="fb50d-279">Aşağıdaki örnek, bir kitabın Yayınlandığı tarihten itibaren yıl sayısını döndüren bir işlev tanımlamak için bir **DefiningExpression** öğesi kullanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="fb50d-280">**DefiningExpression** öğesinin içeriği Entity SQL yazılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="fb50d-281">Bağımlı öğe (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="fb50d-282">Kavramsal şema tanım dili (CSDL) içindeki **bağımlı** öğe, ReferentialConstraint öğesinin bir alt öğesidir ve bir başvuru kısıtlamasının bağımlı sonunu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="fb50d-283">Bir **ReferentialConstraint** öğesi, ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamasına benzer işlevselliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="fb50d-284">Bir veritabanı tablosundaki bir sütunun (veya sütunlarının) başka bir tablonun birincil anahtarına başvurmasına benzer şekilde, bir varlık türünün özelliği (veya özellikleri) başka bir varlık türünün varlık anahtarına başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="fb50d-285">Başvurulan varlık türüne kısıtlamanın *asıl sonu* denir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="fb50d-286">Asıl ucuna başvuran varlık türüne kısıtlamanın *bağımlı sonu* denir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="fb50d-287">**Propertyref** öğeleri, asıl uca hangi anahtarların başvurulacağını belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="fb50d-288">**Bağımlı** öğe aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-289">PropertyRef (bir veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="fb50d-290">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-291">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-291">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-292">Aşağıdaki tabloda **bağımlı** öğeye uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="fb50d-293">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-293">Attribute Name</span></span> | <span data-ttu-id="fb50d-294">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-294">Is Required</span></span> | <span data-ttu-id="fb50d-295">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="fb50d-296">**Rol**</span><span class="sxs-lookup"><span data-stu-id="fb50d-296">**Role**</span></span>       | <span data-ttu-id="fb50d-297">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-297">Yes</span></span>         | <span data-ttu-id="fb50d-298">İlişkinin bağımlı ucundaki varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-299">Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **bağımlı** öğeye uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="fb50d-300">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-301">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-302">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-302">Example</span></span>

<span data-ttu-id="fb50d-303">Aşağıdaki örnek, **PublishedBy** Association tanımının bir parçası olarak kullanılan **ReferentialConstraint** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="fb50d-304">**Kitap** varlık türünün **publisherID** özelliği, başvuru kısıtlamasının bağımlı sonunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="fb50d-305">Documentation öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="fb50d-306">Kavramsal şema tanım dili (CSDL) içindeki **Belgeler** öğesi, bir üst öğede tanımlanan bir nesne hakkında bilgi sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="fb50d-307">Bir. edmx dosyasında, **Belgeler** öğesi EF Designer 'ın tasarım yüzeyinde (bir varlık, ilişkilendirme veya özellik gibi) bir nesne olarak görünen bir öğenin alt öğesi olduğunda, **belge** öğesinin içeriği şöyle görünür Nesne için Visual Studio **özellikleri** penceresi.</span><span class="sxs-lookup"><span data-stu-id="fb50d-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="fb50d-308">**Belge** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-309">**Özet**: Üst öğenin kısa bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="fb50d-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="fb50d-310">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-310">(zero or one element)</span></span>
-   <span data-ttu-id="fb50d-311">**LongDescription**: Üst öğenin kapsamlı bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="fb50d-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="fb50d-312">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-312">(zero or one element)</span></span>
-   <span data-ttu-id="fb50d-313">Ek açıklama öğeleri.</span><span class="sxs-lookup"><span data-stu-id="fb50d-313">Annotation elements.</span></span> <span data-ttu-id="fb50d-314">(sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-315">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-315">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-316">**Belge** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="fb50d-317">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-318">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="fb50d-319">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-319">Example</span></span>

<span data-ttu-id="fb50d-320">Aşağıdaki örnek, bir EntityType öğesinin alt öğesi olarak **documentation** öğesini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="fb50d-321">Aşağıdaki kod parçacığı bir. edmx dosyasının CSDL içeriklerinde olsaydı, `Customer` varlık türüne tıkladığınızda **Summary** ve **LongDescription** öğelerinin içeriği Visual Studio **Özellikler** penceresinde görünür.</span><span class="sxs-lookup"><span data-stu-id="fb50d-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="fb50d-322">End öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-322">End Element (CSDL)</span></span>

<span data-ttu-id="fb50d-323">Kavramsal şema tanım dili (CSDL) içindeki **End** öğesi Association öğesinin veya AssociationSet öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="fb50d-324">Her durumda, **End** öğesinin rolü farklıdır ve ilgili öznitelikler farklıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="fb50d-325">Ilişki öğesinin bir alt öğesi olarak end öğesi</span><span class="sxs-lookup"><span data-stu-id="fb50d-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="fb50d-326">Bir **End** öğesi ( **Association** öğesinin bir alt öğesi olarak), ilişkinin bir sonundaki varlık türünü ve bir ilişkinin o ucunda bulunabilir varlık türü örneklerinin sayısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="fb50d-327">İlişki uçları bir ilişkinin parçası olarak tanımlanır; bir ilişkilendirme tam olarak iki ilişkilendirme bitmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="fb50d-328">Bir ilişkinin bir sonundaki varlık türü örneklerine, bir varlık türü üzerinde gösterilmeleri durumunda gezinti özellikleri veya yabancı anahtarlar üzerinden erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="fb50d-329">Bir **End** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-330">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-331">OnDelete (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-332">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-333">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-333">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-334">Aşağıdaki tabloda, bir **ilişkilendirme** öğesinin alt öğesi olduğunda, **End** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="fb50d-335">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-335">Attribute Name</span></span>   | <span data-ttu-id="fb50d-336">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-336">Is Required</span></span> | <span data-ttu-id="fb50d-337">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-338">**Tür**</span><span class="sxs-lookup"><span data-stu-id="fb50d-338">**Type**</span></span>         | <span data-ttu-id="fb50d-339">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-339">Yes</span></span>         | <span data-ttu-id="fb50d-340">İlişkinin bir sonundaki varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="fb50d-341">**Rol**</span><span class="sxs-lookup"><span data-stu-id="fb50d-341">**Role**</span></span>         | <span data-ttu-id="fb50d-342">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-342">No</span></span>          | <span data-ttu-id="fb50d-343">İlişki ucu için bir ad.</span><span class="sxs-lookup"><span data-stu-id="fb50d-343">A name for the association end.</span></span> <span data-ttu-id="fb50d-344">Ad sağlanmazsa, ilişki uçtaki varlık türünün adı kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="fb50d-345">**Ğunun**</span><span class="sxs-lookup"><span data-stu-id="fb50d-345">**Multiplicity**</span></span> | <span data-ttu-id="fb50d-346">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-346">Yes</span></span>         | <span data-ttu-id="fb50d-347">**1**, **0.. 1**veya ilişki sonunda olabilecek varlık türü örneklerinin sayısına bağlı olarak **\*** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="fb50d-348">**1** ilişki ucunda tam olarak bir varlık türü örneğinin bulunduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="fb50d-349">**0.. 1** , ilişkilendirme ucunda sıfır veya bir varlık türü örneklerinin bulunduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="fb50d-350">**\*** , ilişkilendirme ucunda sıfır, bir veya daha fazla varlık türü örneğinin var olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-351">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **End** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="fb50d-352">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-353">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="fb50d-354">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-354">Example</span></span>

<span data-ttu-id="fb50d-355">Aşağıdaki örnek, **CustomerOrders** ilişkilendirmesini tanımlayan bir **ilişki** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="fb50d-356">İlişkilendirmenin her bir **ucunun** **çoğulluğu** değeri, bir **müşteriyle**birçok **siparişin** Ilişkilendirilemeyeceğini gösterir, ancak bir **siparişle**yalnızca bir **Müşteri** ilişkilendirilebilen anlamına gelebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="fb50d-357">Ayrıca, **OnDelete** öğesi, **Müşteri** silinirse, belirli bir **müşteriyle** ilgili olan ve ObjectContext 'e yüklenmiş tüm **siparişlerin** silineceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="fb50d-358">Öğeyi AssociationSet öğesinin bir alt öğesi olarak bitir</span><span class="sxs-lookup"><span data-stu-id="fb50d-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="fb50d-359">**End** öğesi bir ilişki kümesinin sonunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="fb50d-360">**AssociationSet** öğesi iki **End** öğesi içermelidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="fb50d-361">Bir **End** öğesinde yer alan bilgiler bir veri kaynağına yönelik bir ilişki kümesini eşlerken kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="fb50d-362">Bir **End** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-363">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-364">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-365">Ek açıklama öğeleri diğer tüm alt öğelerden sonra gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="fb50d-366">Ek açıklama öğelerine yalnızca CSDL v2 ve sonrasında izin verilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-367">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-367">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-368">Aşağıdaki tabloda, bir **AssociationSet** öğesinin alt öğesi olduğunda **End** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="fb50d-369">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-369">Attribute Name</span></span> | <span data-ttu-id="fb50d-370">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-370">Is Required</span></span> | <span data-ttu-id="fb50d-371">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-372">**Di**</span><span class="sxs-lookup"><span data-stu-id="fb50d-372">**EntitySet**</span></span>  | <span data-ttu-id="fb50d-373">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-373">Yes</span></span>         | <span data-ttu-id="fb50d-374">Üst **AssociationSet** öğesinin bir sonunu tanımlayan **EntitySet** öğesinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="fb50d-375">**EntitySet** öğesi, üst **AssociationSet** öğesiyle aynı varlık kapsayıcısında tanımlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="fb50d-376">**Rol**</span><span class="sxs-lookup"><span data-stu-id="fb50d-376">**Role**</span></span>       | <span data-ttu-id="fb50d-377">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-377">No</span></span>          | <span data-ttu-id="fb50d-378">İlişki kümesi ucunun adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-378">The name of the association set end.</span></span> <span data-ttu-id="fb50d-379">**Rol** özniteliği kullanılmazsa, ilişki kümesi ucunun adı varlık kümesinin adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="fb50d-380">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **End** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="fb50d-381">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-382">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="fb50d-383">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-383">Example</span></span>

<span data-ttu-id="fb50d-384">Aşağıdaki örnek, her biri iki **End** öğesine sahip iki **AssociationSet** öğesiyle bir **EntityContainer** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="fb50d-385">EntityContainer öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="fb50d-386">Kavramsal şema tanım dili (CSDL) içindeki **EntityContainer** öğesi, varlık kümeleri, ilişkilendirme kümeleri ve işlev içeri aktarmaları için bir mantıksal kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="fb50d-387">Kavramsal model varlık kapsayıcısı, EntityContainerMapping öğesi aracılığıyla bir depolama modeli varlık kapsayıcısına eşlenir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="fb50d-388">Bir depolama modeli varlık kapsayıcısı veritabanının yapısını açıklar: varlık kümeleri tabloları tanımlar, ilişki kümeleri yabancı anahtar kısıtlamalarını tanımlar ve işlev içeri aktarmaları bir veritabanında saklı yordamları açıklar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="fb50d-389">Bir **EntityContainer** öğesi sıfır veya bir belge öğesine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="fb50d-390">Bir **belge** öğesi varsa, tüm **EntitySet**, **AssociationSet**ve **FunctionImport** öğelerinden önce gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="fb50d-391">Bir **EntityContainer** öğesi aşağıdaki alt öğeleri sıfır veya daha fazla içerebilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-392">Di</span><span class="sxs-lookup"><span data-stu-id="fb50d-392">EntitySet</span></span>
-   <span data-ttu-id="fb50d-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="fb50d-393">AssociationSet</span></span>
-   <span data-ttu-id="fb50d-394">'Unun</span><span class="sxs-lookup"><span data-stu-id="fb50d-394">FunctionImport</span></span>
-   <span data-ttu-id="fb50d-395">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="fb50d-395">Annotation elements</span></span>

<span data-ttu-id="fb50d-396">Aynı ad alanı içinde olan başka bir **EntityContainer** 'ın içeriğini dahil etmek Için bir **EntityContainer** öğesini genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="fb50d-397">Başka bir **EntityContainer**'ın içeriğini eklemek için, başvuran **EntityContainer** öğesinde, **Extends** özniteliğinin değerini, dahil etmek istediğiniz **EntityContainer** öğesinin adı olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="fb50d-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="fb50d-398">Dahil edilen **EntityContainer** öğesinin tüm alt öğeleri, başvuran **EntityContainer** öğesinin alt öğeleri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-399">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-399">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-400">Aşağıdaki tabloda, **using** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="fb50d-401">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-401">Attribute Name</span></span> | <span data-ttu-id="fb50d-402">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-402">Is Required</span></span> | <span data-ttu-id="fb50d-403">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="fb50d-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-404">**Name**</span></span>       | <span data-ttu-id="fb50d-405">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-405">Yes</span></span>         | <span data-ttu-id="fb50d-406">Varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="fb50d-407">**Tekrarlan**</span><span class="sxs-lookup"><span data-stu-id="fb50d-407">**Extends**</span></span>    | <span data-ttu-id="fb50d-408">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-408">No</span></span>          | <span data-ttu-id="fb50d-409">Aynı ad alanı içindeki başka bir varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-410">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **EntityContainer** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="fb50d-411">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-412">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-413">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-413">Example</span></span>

<span data-ttu-id="fb50d-414">Aşağıdaki örnek, üç varlık kümesini ve iki ilişkilendirme kümesini tanımlayan bir **EntityContainer** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="fb50d-415">EntitySet öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="fb50d-416">Kavramsal şema tanım dilindeki **EntitySet** öğesi bir varlık türü örnekleri ve bu varlık türünden türetilmiş herhangi bir türün örnekleri için mantıksal bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="fb50d-417">Bir varlık türü ve bir varlık kümesi arasındaki ilişki, ilişkisel veritabanındaki bir satır ve tablo arasındaki ilişkiye benzer.</span><span class="sxs-lookup"><span data-stu-id="fb50d-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="fb50d-418">Bir satır gibi, bir varlık türü ilgili verilerin bir kümesini tanımlar ve bir tablo gibi bir varlık kümesi bu tanımın örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="fb50d-419">Bir varlık kümesi, bir veri kaynağındaki ilgili veri yapılarına eşleştiribilecekleri şekilde, varlık türü örneklerinin gruplandırılması için bir yapı sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="fb50d-420">Belirli bir varlık türü için birden fazla varlık kümesi tanımlanmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-421">EF Designer, tür başına birden çok varlık kümesi içeren kavramsal modelleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="fb50d-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="fb50d-422">**EntitySet** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-423">Documentation öğesi (sıfır veya bir öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="fb50d-424">Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-425">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-425">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-426">Aşağıdaki tabloda, **EntitySet** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="fb50d-427">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-427">Attribute Name</span></span> | <span data-ttu-id="fb50d-428">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-428">Is Required</span></span> | <span data-ttu-id="fb50d-429">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-430">**Name**</span></span>       | <span data-ttu-id="fb50d-431">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-431">Yes</span></span>         | <span data-ttu-id="fb50d-432">Varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="fb50d-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="fb50d-433">**EntityType**</span></span> | <span data-ttu-id="fb50d-434">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-434">Yes</span></span>         | <span data-ttu-id="fb50d-435">Varlık kümesinin örnek içerdiği varlık türünün tam adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-436">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **EntitySet** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="fb50d-437">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-438">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-439">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-439">Example</span></span>

<span data-ttu-id="fb50d-440">Aşağıdaki örnek, üç **EntitySet** öğesiyle bir **EntityContainer** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="fb50d-441">Tür başına birden çok varlık kümesi tanımlamak mümkündür (MEST).</span><span class="sxs-lookup"><span data-stu-id="fb50d-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="fb50d-442">Aşağıdaki örnek, **kitap** varlık türü için iki varlık kümesiyle bir varlık kapsayıcısını tanımlar:</span><span class="sxs-lookup"><span data-stu-id="fb50d-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="fb50d-443">EntityType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="fb50d-444">**EntityType** öğesi, bir müşteri veya sipariş gibi bir üst düzey kavramın bir kavramsal modelde yapısını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fb50d-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="fb50d-445">Varlık türü, bir uygulamadaki varlık türü örnekleri için bir şablondur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="fb50d-446">Her şablon aşağıdaki bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="fb50d-447">Benzersiz bir ad.</span><span class="sxs-lookup"><span data-stu-id="fb50d-447">A unique name.</span></span> <span data-ttu-id="fb50d-448">(Gerekli.)</span><span class="sxs-lookup"><span data-stu-id="fb50d-448">(Required.)</span></span>
-   <span data-ttu-id="fb50d-449">Bir veya daha fazla özellik tarafından tanımlanan bir varlık anahtarı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="fb50d-450">(Gerekli.)</span><span class="sxs-lookup"><span data-stu-id="fb50d-450">(Required.)</span></span>
-   <span data-ttu-id="fb50d-451">Veri içeren Özellikler.</span><span class="sxs-lookup"><span data-stu-id="fb50d-451">Properties for containing data.</span></span> <span data-ttu-id="fb50d-452">(İsteğe bağlı.)</span><span class="sxs-lookup"><span data-stu-id="fb50d-452">(Optional.)</span></span>
-   <span data-ttu-id="fb50d-453">Bir ilişkinin bir sonundan diğer uçtan gezintiye izin veren gezinti özellikleri.</span><span class="sxs-lookup"><span data-stu-id="fb50d-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="fb50d-454">(İsteğe bağlı.)</span><span class="sxs-lookup"><span data-stu-id="fb50d-454">(Optional.)</span></span>

<span data-ttu-id="fb50d-455">Bir uygulamada, varlık türünün bir örneği belirli bir nesneyi (örneğin, belirli bir müşteri veya sipariş) temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fb50d-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="fb50d-456">Bir varlık türünün her örneğinin bir varlık kümesi içinde benzersiz bir varlık anahtarı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="fb50d-457">İki varlık türü örneği, yalnızca aynı türde olmaları durumunda ve varlık anahtarlarının değerleri aynı ise eşit olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="fb50d-458">Bir **EntityType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-459">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-460">Anahtar (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-461">Özellik (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="fb50d-462">NavigationProperty (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="fb50d-463">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-464">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-464">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-465">Aşağıdaki tabloda, **EntityType** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="fb50d-466">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="fb50d-467">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-467">Is Required</span></span> | <span data-ttu-id="fb50d-468">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="fb50d-470">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-470">Yes</span></span>         | <span data-ttu-id="fb50d-471">Varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="fb50d-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="fb50d-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="fb50d-473">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-473">No</span></span>          | <span data-ttu-id="fb50d-474">Tanımlanmakta olan varlık türünün temel türü olan başka bir varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="fb50d-475">**Soyut**</span><span class="sxs-lookup"><span data-stu-id="fb50d-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="fb50d-476">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-476">No</span></span>          | <span data-ttu-id="fb50d-477">Varlık türünün soyut bir tür olmasına bağlı olarak **true** veya **false**.</span><span class="sxs-lookup"><span data-stu-id="fb50d-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="fb50d-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="fb50d-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="fb50d-479">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-479">No</span></span>          | <span data-ttu-id="fb50d-480">Varlık türünün açık bir varlık türü olup olmadığına bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="fb50d-481">**OpenType** özniteliği > yalnızca ADO.NET Data Services ile kullanılan kavramsal modellerde tanımlanan varlık türleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="fb50d-482">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **EntityType** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="fb50d-483">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-484">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-485">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-485">Example</span></span>

<span data-ttu-id="fb50d-486">Aşağıdaki örnek, üç **özellik** öğesi ve iki **NavigationProperty** öğesi olan bir **EntityType** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="fb50d-487">EnumType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="fb50d-488">**EnumType** öğesi, numaralandırılmış bir türü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fb50d-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="fb50d-489">Bir **EnumType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-490">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-491">Üye (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="fb50d-492">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-493">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-493">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-494">Aşağıdaki tabloda, **EnumType** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="fb50d-495">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-495">Attribute Name</span></span>     | <span data-ttu-id="fb50d-496">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-496">Is Required</span></span> | <span data-ttu-id="fb50d-497">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-498">**Name**</span></span>           | <span data-ttu-id="fb50d-499">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-499">Yes</span></span>         | <span data-ttu-id="fb50d-500">Varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="fb50d-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="fb50d-501">**IsFlags**</span></span>        | <span data-ttu-id="fb50d-502">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-502">No</span></span>          | <span data-ttu-id="fb50d-503">Sabit listesi türünün bir bayrak kümesi olarak kullanılıp kullanılamayacağını bağlı olarak **true** veya **false**.</span><span class="sxs-lookup"><span data-stu-id="fb50d-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="fb50d-504">Varsayılan değer false şeklindedir **.**</span><span class="sxs-lookup"><span data-stu-id="fb50d-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="fb50d-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="fb50d-505">**UnderlyingType**</span></span> | <span data-ttu-id="fb50d-506">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-506">No</span></span>          | <span data-ttu-id="fb50d-507">**Edm. Byte**, **Edm. Int16**, **Edm. Int32**, **Edm. Int64** veya **Edm. SByte** , türün değer aralığını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="fb50d-508">  Numaralandırma öğelerinin varsayılan temel alınan türü **Edm. Int32**' dir..</span><span class="sxs-lookup"><span data-stu-id="fb50d-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-509">**EnumType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="fb50d-510">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-511">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-512">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-512">Example</span></span>

<span data-ttu-id="fb50d-513">Aşağıdaki örnek, üç **üye** öğesi olan bir **EnumType** öğesini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="fb50d-514">Function öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-514">Function Element (CSDL)</span></span>

<span data-ttu-id="fb50d-515">Kavramsal şema tanım dili (CSDL) içindeki **işlev** öğesi, kavramsal modeldeki işlevleri tanımlamak veya bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="fb50d-516">Bir işlev DefiningExpression öğesi kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="fb50d-517">Bir **Function** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-518">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-519">Parametre (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="fb50d-520">DefiningExpression (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-521">ReturnType (Işlev) (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-522">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="fb50d-523">Bir işlev için dönüş türü, **ReturnType** (Function) öğesi ya da **ReturnType** özniteliğiyle (aşağıya bakın) belirtilmelidir, ancak her ikisine birden değil.</span><span class="sxs-lookup"><span data-stu-id="fb50d-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="fb50d-524">Olası dönüş türleri, herhangi bir EdmSimpleType, varlık türü, karmaşık tür, satır türü veya başvuru türüdür (veya bu türlerden birinin bir koleksiyonu).</span><span class="sxs-lookup"><span data-stu-id="fb50d-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-525">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-525">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-526">Aşağıdaki tabloda, **işlev** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="fb50d-527">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-527">Attribute Name</span></span> | <span data-ttu-id="fb50d-528">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-528">Is Required</span></span> | <span data-ttu-id="fb50d-529">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="fb50d-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-530">**Name**</span></span>       | <span data-ttu-id="fb50d-531">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-531">Yes</span></span>         | <span data-ttu-id="fb50d-532">İşlevin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-532">The name of the function.</span></span>          |
| <span data-ttu-id="fb50d-533">**'Indaki**</span><span class="sxs-lookup"><span data-stu-id="fb50d-533">**ReturnType**</span></span> | <span data-ttu-id="fb50d-534">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-534">No</span></span>          | <span data-ttu-id="fb50d-535">İşlev tarafından döndürülen tür.</span><span class="sxs-lookup"><span data-stu-id="fb50d-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-536">**İşlev** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="fb50d-537">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-538">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-539">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-539">Example</span></span>

<span data-ttu-id="fb50d-540">Aşağıdaki örnek, bir eğitmenin işe alındığı tarihten itibaren yıl sayısını döndüren bir işlevi tanımlamak için bir **Function** öğesi kullanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="fb50d-541">FunctionImport öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="fb50d-542">Kavramsal şema tanım dili (CSDL) içindeki **FunctionImport** öğesi, veri kaynağında tanımlanan ancak kavramsal model aracılığıyla nesneler için kullanılabilen bir işlevi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fb50d-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="fb50d-543">Örneğin, depolama modelindeki bir Işlev öğesi bir veritabanında saklı yordamı temsil etmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="fb50d-544">Kavramsal modeldeki bir **FunctionImport** öğesi, bir Entity Framework uygulamasındaki karşılık gelen işlevi temsil eder ve FunctionImportMapping öğesi kullanılarak depolama modeli işlevine eşlenir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="fb50d-545">İşlev uygulamada çağrıldığında, karşılık gelen saklı yordam veritabanında yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fb50d-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="fb50d-546">**FunctionImport** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-547">Belgeler (sıfır veya bir öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="fb50d-548">Parametre (sıfır veya daha fazla öğeye izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="fb50d-549">Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="fb50d-550">ReturnType (FunctionImport) (sıfır veya daha fazla öğeye izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="fb50d-551">İşlevin kabul ettiği her parametre için bir **parametre** öğesi tanımlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="fb50d-552">Bir işlev için dönüş türü, **ReturnType** (FunctionImport) öğesi ya da **ReturnType** özniteliği (aşağıya bakın) ile belirtilmelidir, ancak her ikisine birden değil.</span><span class="sxs-lookup"><span data-stu-id="fb50d-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="fb50d-553">Dönüş türü değeri EdmSimpleType, EntityType veya ComplexType koleksiyonu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-554">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-554">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-555">Aşağıdaki tabloda, **FunctionImport** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="fb50d-556">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-556">Attribute Name</span></span>   | <span data-ttu-id="fb50d-557">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-557">Is Required</span></span> | <span data-ttu-id="fb50d-558">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-559">**Name**</span></span>         | <span data-ttu-id="fb50d-560">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-560">Yes</span></span>         | <span data-ttu-id="fb50d-561">İçeri aktarılan işlevin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="fb50d-562">**'Indaki**</span><span class="sxs-lookup"><span data-stu-id="fb50d-562">**ReturnType**</span></span>   | <span data-ttu-id="fb50d-563">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-563">No</span></span>          | <span data-ttu-id="fb50d-564">İşlevin döndürdüğü tür.</span><span class="sxs-lookup"><span data-stu-id="fb50d-564">The type that the function returns.</span></span> <span data-ttu-id="fb50d-565">İşlev bir değer döndürmezse bu özniteliği kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="fb50d-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="fb50d-566">Aksi takdirde, değer ComplexType, EntityType veya EDMSimpleType bir koleksiyon olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="fb50d-567">**Di**</span><span class="sxs-lookup"><span data-stu-id="fb50d-567">**EntitySet**</span></span>    | <span data-ttu-id="fb50d-568">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-568">No</span></span>          | <span data-ttu-id="fb50d-569">İşlev bir varlık türleri koleksiyonu döndürürse, **EntitySet** 'in değeri koleksiyonun ait olduğu varlık kümesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="fb50d-570">Aksi takdirde, **EntitySet** özniteliği kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="fb50d-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="fb50d-571">**IsComposable**</span></span> | <span data-ttu-id="fb50d-572">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-572">No</span></span>          | <span data-ttu-id="fb50d-573">Değer true olarak ayarlanırsa, işlev birleştirilebilir (tablo değerli Işlev) ve bir LINQ sorgusunda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="fb50d-574">  Varsayılan değer **false**'dur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="fb50d-575">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **FunctionImport** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="fb50d-576">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-577">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-578">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-578">Example</span></span>

<span data-ttu-id="fb50d-579">Aşağıdaki örnek, bir parametre kabul eden ve bir varlık türleri koleksiyonu döndüren bir **FunctionImport** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="fb50d-580">Anahtar öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-580">Key Element (CSDL)</span></span>

<span data-ttu-id="fb50d-581">**Anahtar** öğesi EntityType öğesinin bir alt öğesidir ve bir *varlık anahtarı* (bir özellik veya kimliği belirleyen bir varlık türünün özellikler kümesi) tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="fb50d-582">Bir varlık anahtarını oluşturan Özellikler tasarım zamanında seçilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="fb50d-583">Varlık anahtarı özelliklerinin değerleri, çalışma zamanında bir varlık kümesi içindeki bir varlık türü örneğini benzersiz şekilde tanımlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="fb50d-584">Bir varlık kümesindeki örneklerin benzersizlik garantisi sağlamak için bir varlık anahtarı oluşturan Özellikler seçilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="fb50d-585">**Anahtar** öğesi bir varlık türünün bir veya daha fazla özelliğe başvurarak bir varlık anahtarını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="fb50d-586">**Anahtar** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb50d-587">PropertyRef (bir veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="fb50d-588">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-589">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-589">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-590">**Anahtar** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="fb50d-591">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-592">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="fb50d-593">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-593">Example</span></span>

<span data-ttu-id="fb50d-594">Aşağıdaki örnek, **Book**adlı bir varlık türünü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="fb50d-595">Varlık anahtarı, varlık türünün **ISBN** özelliğine başvurarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="fb50d-596">Uluslararası bir standart defter numarası (ıSBN) bir kitabı benzersiz bir şekilde tanımladığından, **ISBN** özelliği varlık anahtarı için iyi bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="fb50d-597">Aşağıdaki örnek, iki özellik, **ad** ve **adresten**oluşan bir varlık anahtarı olan bir varlık türü (**Yazar**) gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="fb50d-598">Aynı ada sahip iki yazar aynı adreste yaşılamadığından, varlık anahtarı için **ad** ve **Adres** kullanımı makul bir seçimdir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="fb50d-599">Ancak, bir varlık anahtarı için bu seçenek bir varlık kümesindeki benzersiz varlık anahtarlarını kesinlikle garanti etmez.</span><span class="sxs-lookup"><span data-stu-id="fb50d-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="fb50d-600">Bir yazarı benzersiz bir şekilde tanımlamak için kullanılabilecek **AuthorId**gibi bir özelliğin eklenmesi bu durumda önerilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="fb50d-601">Üye öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-601">Member Element (CSDL)</span></span>

<span data-ttu-id="fb50d-602">**Üye** öğesi EnumType öğesinin bir alt öğesidir ve numaralandırılmış türün bir üyesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-603">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-603">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-604">Aşağıdaki tabloda, **FunctionImport** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="fb50d-605">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-605">Attribute Name</span></span> | <span data-ttu-id="fb50d-606">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-606">Is Required</span></span> | <span data-ttu-id="fb50d-607">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-608">**Name**</span></span>       | <span data-ttu-id="fb50d-609">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-609">Yes</span></span>         | <span data-ttu-id="fb50d-610">Üyenin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="fb50d-611">**Değer**</span><span class="sxs-lookup"><span data-stu-id="fb50d-611">**Value**</span></span>      | <span data-ttu-id="fb50d-612">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-612">No</span></span>          | <span data-ttu-id="fb50d-613">Üyenin değeri.</span><span class="sxs-lookup"><span data-stu-id="fb50d-613">The value of the member.</span></span> <span data-ttu-id="fb50d-614">Varsayılan olarak, ilk üye 0 değerine sahiptir ve art arda her bir Numaralandırıcı değeri 1 artırılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="fb50d-615">Aynı değere sahip birden çok üye bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-616">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **FunctionImport** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="fb50d-617">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-618">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-619">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-619">Example</span></span>

<span data-ttu-id="fb50d-620">Aşağıdaki örnek, üç **üye** öğesi olan bir **EnumType** öğesini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="fb50d-621">NavigationProperty öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="fb50d-622">Bir **NavigationProperty** öğesi, bir ilişkilendirmenin diğer sonuna bir başvuru sağlayan bir gezinti özelliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="fb50d-623">Özellik öğesiyle tanımlanan özelliklerden farklı olarak, gezinti özellikleri verilerin şeklini ve özelliklerini tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="fb50d-624">İki varlık türü arasındaki bir ilişkilendirmede gezinmek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="fb50d-625">Gezinti özelliklerinin, bir ilişkinin sonundaki her iki varlık türü üzerinde isteğe bağlı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fb50d-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="fb50d-626">Bir ilişkinin sonundaki bir varlık türünde bir gezinti özelliği tanımlarsanız, ilişkilendirmenin diğer sonundaki varlık türünde bir gezinti özelliği tanımlamanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fb50d-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="fb50d-627">Bir gezinti özelliği tarafından döndürülen veri türü, uzak ilişki ucunun çoğulluğu tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="fb50d-628">Örneğin **, bir** **Müşteri** varlık türünde bir gezinti özelliği olduğunu varsayalım ve **Müşteri** ile **sipariş**arasında bire çok ilişkilendirmeyi gider.</span><span class="sxs-lookup"><span data-stu-id="fb50d-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="fb50d-629">Gezinti özelliği için uzak ilişki ucunun çokluya çok sayıda (\*) olduğundan, veri türü bir koleksiyon ( **sıra**) olur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="fb50d-630">Benzer şekilde, bir gezinti özelliği olan **CustomerNavProp**, **sipariş** varlık türünde mevcutsa, uzak uçtaki çoğulluğu bir (1) olduğundan veri türü **Müşteri** olur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="fb50d-631">Bir **NavigationProperty** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-632">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-633">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-634">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-634">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-635">Aşağıdaki tabloda, **NavigationProperty** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="fb50d-636">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-636">Attribute Name</span></span>   | <span data-ttu-id="fb50d-637">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-637">Is Required</span></span> | <span data-ttu-id="fb50d-638">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-639">**Name**</span></span>         | <span data-ttu-id="fb50d-640">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-640">Yes</span></span>         | <span data-ttu-id="fb50d-641">Gezinti özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="fb50d-642">**İlişkiye**</span><span class="sxs-lookup"><span data-stu-id="fb50d-642">**Relationship**</span></span> | <span data-ttu-id="fb50d-643">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-643">Yes</span></span>         | <span data-ttu-id="fb50d-644">Modelin kapsamı içinde olan bir ilişkilendirmenin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="fb50d-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="fb50d-645">**ToRole**</span></span>       | <span data-ttu-id="fb50d-646">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-646">Yes</span></span>         | <span data-ttu-id="fb50d-647">Gezinmede uçların bittiği son.</span><span class="sxs-lookup"><span data-stu-id="fb50d-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="fb50d-648">**ToRole** özniteliğinin değeri, ilişki uçlarından birinde tanımlanan **rol** özniteliklerinden birinin değeriyle aynı olmalıdır (associationend öğesinde tanımlanır).</span><span class="sxs-lookup"><span data-stu-id="fb50d-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="fb50d-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="fb50d-649">**FromRole**</span></span>     | <span data-ttu-id="fb50d-650">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-650">Yes</span></span>         | <span data-ttu-id="fb50d-651">Gezintinin başladığı ilişkinin sonu.</span><span class="sxs-lookup"><span data-stu-id="fb50d-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="fb50d-652">**FromRole** özniteliğinin değeri, ilişki uçlarından birinde tanımlanan **rol** özniteliklerinden birinin değeriyle aynı olmalıdır (associationend öğesinde tanımlanır).</span><span class="sxs-lookup"><span data-stu-id="fb50d-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-653">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **NavigationProperty** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="fb50d-654">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-655">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-656">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-656">Example</span></span>

<span data-ttu-id="fb50d-657">Aşağıdaki örnek, iki gezinti özelliği (**PublishedBy** ve **WrittenBy**) ile bir varlık türü (**Book**) tanımlar:</span><span class="sxs-lookup"><span data-stu-id="fb50d-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="fb50d-658">OnDelete öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="fb50d-659">Kavramsal şema tanım dili (CSDL) içindeki **OnDelete** öğesi bir ilişkilendirme ile bağlantılı davranışı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="fb50d-660">**Eylem** özniteliği bir ilişkinin bir ucunda **Cascade** olarak ayarlandıysa, ilk uçtaki varlık türü silindiğinde, ilişkilendirmenin diğer ucundaki ilgili varlık türleri silinir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="fb50d-661">İki varlık türü arasındaki ilişki birincil anahtardan birincil anahtar ilişkisi ise, ilişkilendirmenin diğer ucundaki asıl nesne **OnDelete** belirtimine bakılmaksızın silindiğinde, yüklenmiş bir bağımlı nesne silinir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="fb50d-662">**OnDelete** öğesi yalnızca bir uygulamanın çalışma zamanı davranışını etkiler; veri kaynağındaki davranışı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="fb50d-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="fb50d-663">Veri kaynağında tanımlanan davranış, uygulamada tanımlanan davranışla aynı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="fb50d-664">**OnDelete** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-665">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-666">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-667">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-667">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-668">Aşağıdaki tabloda, **OnDelete** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="fb50d-669">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-669">Attribute Name</span></span> | <span data-ttu-id="fb50d-670">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-670">Is Required</span></span> | <span data-ttu-id="fb50d-671">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-672">**Eylem**</span><span class="sxs-lookup"><span data-stu-id="fb50d-672">**Action**</span></span>     | <span data-ttu-id="fb50d-673">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-673">Yes</span></span>         | <span data-ttu-id="fb50d-674">**Cascade** veya **none**.</span><span class="sxs-lookup"><span data-stu-id="fb50d-674">**Cascade** or **None**.</span></span> <span data-ttu-id="fb50d-675">Eğer **basamakla**, bağımlı varlık türleri asıl varlık türü silindiğinde silinir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="fb50d-676">**Hiçbiri**yoksa, asıl varlık türü silindiğinde bağımlı varlık türleri silinmez.</span><span class="sxs-lookup"><span data-stu-id="fb50d-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-677">**İlişkilendirme** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="fb50d-678">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-679">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-680">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-680">Example</span></span>

<span data-ttu-id="fb50d-681">Aşağıdaki örnek, **CustomerOrders** ilişkilendirmesini tanımlayan bir **ilişki** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="fb50d-682">**OnDelete** öğesi, **Müşteri** silindiğinde, belirli bir **müşteriyle** Ilgili ve ObjectContext 'e yüklenmiş tüm **siparişlerin** silineceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="fb50d-683">Parameter öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="fb50d-684">Kavramsal şema tanım dili (CSDL) içindeki **Parameter** öğesi, FunctionImport öğesinin veya Function öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="fb50d-685">FunctionImport öğe uygulaması</span><span class="sxs-lookup"><span data-stu-id="fb50d-685">FunctionImport Element Application</span></span>

<span data-ttu-id="fb50d-686">Bir **parametre** öğesi ( **FunctionImport** öğesinin bir alt Öğesı olarak), csdl içinde belirtilen işlev içeri aktarmaları için giriş ve çıkış parametrelerini tanımlamak üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="fb50d-687">**Parameter** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-688">Belgeler (sıfır veya bir öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="fb50d-689">Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-690">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-690">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-691">Aşağıdaki tabloda **parametre** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="fb50d-692">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-692">Attribute Name</span></span> | <span data-ttu-id="fb50d-693">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-693">Is Required</span></span> | <span data-ttu-id="fb50d-694">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-695">**Name**</span></span>       | <span data-ttu-id="fb50d-696">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-696">Yes</span></span>         | <span data-ttu-id="fb50d-697">Parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="fb50d-698">**Tür**</span><span class="sxs-lookup"><span data-stu-id="fb50d-698">**Type**</span></span>       | <span data-ttu-id="fb50d-699">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-699">Yes</span></span>         | <span data-ttu-id="fb50d-700">Parametre türü.</span><span class="sxs-lookup"><span data-stu-id="fb50d-700">The parameter type.</span></span> <span data-ttu-id="fb50d-701">Değer, bir **Edmsimpletype** veya modelin kapsamı içinde olan karmaşık bir tür olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="fb50d-702">**Modundaysa**</span><span class="sxs-lookup"><span data-stu-id="fb50d-702">**Mode**</span></span>       | <span data-ttu-id="fb50d-703">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-703">No</span></span>          | <span data-ttu-id="fb50d-704">Parametresinin bir giriş, çıkış veya giriş/çıkış parametresi olup olmadığına bağlı olarak, **içinde**, **Out**veya **InOut** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="fb50d-705">**'In**</span><span class="sxs-lookup"><span data-stu-id="fb50d-705">**MaxLength**</span></span>  | <span data-ttu-id="fb50d-706">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-706">No</span></span>          | <span data-ttu-id="fb50d-707">Parametrenin izin verilen en fazla uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="fb50d-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="fb50d-708">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="fb50d-708">**Precision**</span></span>  | <span data-ttu-id="fb50d-709">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-709">No</span></span>          | <span data-ttu-id="fb50d-710">Parametrenin duyarlığı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="fb50d-711">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="fb50d-711">**Scale**</span></span>      | <span data-ttu-id="fb50d-712">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-712">No</span></span>          | <span data-ttu-id="fb50d-713">Parametresinin ölçeği.</span><span class="sxs-lookup"><span data-stu-id="fb50d-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="fb50d-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="fb50d-714">**SRID**</span></span>       | <span data-ttu-id="fb50d-715">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-715">No</span></span>          | <span data-ttu-id="fb50d-716">Uzamsal sistem başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="fb50d-717">Yalnızca uzamsal türlerin parametreleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="fb50d-718">Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb50d-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-719">**Parametre** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="fb50d-720">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-721">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="fb50d-722">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-722">Example</span></span>

<span data-ttu-id="fb50d-723">Aşağıdaki örnek, bir bir **parametre** alt öğesi olan bir **FunctionImport** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="fb50d-724">İşlev bir giriş parametresini kabul eder ve varlık türlerinin bir koleksiyonunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="fb50d-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="fb50d-725">İşlev öğesi uygulaması</span><span class="sxs-lookup"><span data-stu-id="fb50d-725">Function Element Application</span></span>

<span data-ttu-id="fb50d-726">Bir **parametre** öğesi ( **Function** öğesinin bir alt öğesi olarak) kavramsal modelde tanımlanan veya belirtilen işlevlere yönelik parametreleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="fb50d-727">**Parameter** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-728">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="fb50d-729">CollectionType (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="fb50d-730">ReferenceType (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="fb50d-731">RowType (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-732">**CollectionType**, **ReferenceType**veya **RowType** öğelerinden yalnızca biri bir **özellik** öğesinin alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="fb50d-733">Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-734">Ek açıklama öğeleri diğer tüm alt öğelerden sonra gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="fb50d-735">Ek açıklama öğelerine yalnızca CSDL v2 ve sonrasında izin verilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-736">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-736">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-737">Aşağıdaki tabloda **parametre** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="fb50d-738">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-738">Attribute Name</span></span>   | <span data-ttu-id="fb50d-739">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-739">Is Required</span></span> | <span data-ttu-id="fb50d-740">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-741">**Name**</span></span>         | <span data-ttu-id="fb50d-742">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-742">Yes</span></span>         | <span data-ttu-id="fb50d-743">Parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="fb50d-744">**Tür**</span><span class="sxs-lookup"><span data-stu-id="fb50d-744">**Type**</span></span>         | <span data-ttu-id="fb50d-745">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-745">No</span></span>          | <span data-ttu-id="fb50d-746">Parametre türü.</span><span class="sxs-lookup"><span data-stu-id="fb50d-746">The parameter type.</span></span> <span data-ttu-id="fb50d-747">Bir parametre aşağıdaki türlerden herhangi biri olabilir (veya bu türlerin koleksiyonları):</span><span class="sxs-lookup"><span data-stu-id="fb50d-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="fb50d-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="fb50d-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="fb50d-749">entity type</span><span class="sxs-lookup"><span data-stu-id="fb50d-749">entity type</span></span> <br/> <span data-ttu-id="fb50d-750">complex type</span><span class="sxs-lookup"><span data-stu-id="fb50d-750">complex type</span></span> <br/> <span data-ttu-id="fb50d-751">satır türü</span><span class="sxs-lookup"><span data-stu-id="fb50d-751">row type</span></span> <br/> <span data-ttu-id="fb50d-752">başvuru türü</span><span class="sxs-lookup"><span data-stu-id="fb50d-752">reference type</span></span>                             |
| <span data-ttu-id="fb50d-753">**Yapılamaz**</span><span class="sxs-lookup"><span data-stu-id="fb50d-753">**Nullable**</span></span>     | <span data-ttu-id="fb50d-754">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-754">No</span></span>          | <span data-ttu-id="fb50d-755">**True** (varsayılan değer) veya özelliğin **null** değere sahip olmasına bağlı olarak **false** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="fb50d-756">**Değerinin**</span><span class="sxs-lookup"><span data-stu-id="fb50d-756">**DefaultValue**</span></span> | <span data-ttu-id="fb50d-757">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-757">No</span></span>          | <span data-ttu-id="fb50d-758">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="fb50d-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="fb50d-759">**'In**</span><span class="sxs-lookup"><span data-stu-id="fb50d-759">**MaxLength**</span></span>    | <span data-ttu-id="fb50d-760">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-760">No</span></span>          | <span data-ttu-id="fb50d-761">Özellik değerinin uzunluk üst sınırı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="fb50d-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="fb50d-762">**FixedLength**</span></span>  | <span data-ttu-id="fb50d-763">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-763">No</span></span>          | <span data-ttu-id="fb50d-764">Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="fb50d-765">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="fb50d-765">**Precision**</span></span>    | <span data-ttu-id="fb50d-766">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-766">No</span></span>          | <span data-ttu-id="fb50d-767">Özellik değerinin duyarlığı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="fb50d-768">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="fb50d-768">**Scale**</span></span>        | <span data-ttu-id="fb50d-769">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-769">No</span></span>          | <span data-ttu-id="fb50d-770">Özellik değerinin ölçeği.</span><span class="sxs-lookup"><span data-stu-id="fb50d-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="fb50d-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="fb50d-771">**SRID**</span></span>         | <span data-ttu-id="fb50d-772">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-772">No</span></span>          | <span data-ttu-id="fb50d-773">Uzamsal sistem başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="fb50d-774">Yalnızca uzamsal türlerin özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="fb50d-775">Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb50d-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="fb50d-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="fb50d-776">**Unicode**</span></span>      | <span data-ttu-id="fb50d-777">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-777">No</span></span>          | <span data-ttu-id="fb50d-778">Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="fb50d-779">**Mediğinden**</span><span class="sxs-lookup"><span data-stu-id="fb50d-779">**Collation**</span></span>    | <span data-ttu-id="fb50d-780">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-780">No</span></span>          | <span data-ttu-id="fb50d-781">Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="fb50d-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="fb50d-782">**Parametre** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="fb50d-783">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-784">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="fb50d-785">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-785">Example</span></span>

<span data-ttu-id="fb50d-786">Aşağıdaki örnek, bir işlev parametresi tanımlamak için bir **parametre** alt öğesi kullanan bir **Function** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="fb50d-787">Principal öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="fb50d-788">Kavramsal şema tanım dili (CSDL) içindeki **asıl** öğe, bir başvuru kısıtlamasının asıl bitimi tanımlayan ReferentialConstraint öğesinin bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="fb50d-789">Bir **ReferentialConstraint** öğesi, ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamasına benzer işlevselliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="fb50d-790">Bir veritabanı tablosundaki bir sütunun (veya sütunlarının) başka bir tablonun birincil anahtarına başvurmasına benzer şekilde, bir varlık türünün özelliği (veya özellikleri) başka bir varlık türünün varlık anahtarına başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="fb50d-791">Başvurulan varlık türüne kısıtlamanın *asıl sonu* denir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="fb50d-792">Asıl ucuna başvuran varlık türüne kısıtlamanın *bağımlı sonu* denir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="fb50d-793">**Propertyref** öğeleri, bağımlı uç tarafından hangi anahtarlara başvurulduğunu belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="fb50d-794">**Principal** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-795">PropertyRef (bir veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="fb50d-796">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-797">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-797">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-798">Aşağıdaki tabloda, **Principal** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="fb50d-799">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-799">Attribute Name</span></span> | <span data-ttu-id="fb50d-800">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-800">Is Required</span></span> | <span data-ttu-id="fb50d-801">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="fb50d-802">**Rol**</span><span class="sxs-lookup"><span data-stu-id="fb50d-802">**Role**</span></span>       | <span data-ttu-id="fb50d-803">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-803">Yes</span></span>         | <span data-ttu-id="fb50d-804">İlişkinin asıl ucundaki varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-805">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **Principal** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="fb50d-806">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-807">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-808">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-808">Example</span></span>

<span data-ttu-id="fb50d-809">Aşağıdaki örnek, **PublishedBy** Association tanımının bir parçası olan **ReferentialConstraint** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="fb50d-810">**Yayımcı** varlık türünün **ID** özelliği, başvuru kısıtlamasının asıl sonunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="fb50d-811">Property öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-811">Property Element (CSDL)</span></span>

<span data-ttu-id="fb50d-812">Kavramsal şema tanım dili (CSDL) içindeki **özellik** öğesi EntityType öğesi, complexType öğesi ya da RowType öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="fb50d-813">EntityType ve ComplexType öğesi uygulamaları</span><span class="sxs-lookup"><span data-stu-id="fb50d-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="fb50d-814">**Özellik** öğeleri ( **EntityType** veya **complexType** öğelerinin alt öğeleri olarak) bir varlık türü örneği veya karmaşık tür örneğinin içereceği verilerin şeklini ve özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="fb50d-815">Kavramsal modeldeki özellikler, bir sınıfta tanımlanan özelliklerle benzerdir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="fb50d-816">Bir sınıftaki özellikler, sınıfın şeklini tanımlar ve nesneler hakkında bilgi taşıyan bir kavramsal modeldeki Özellikler bir varlık türünün şeklini tanımlar ve varlık türü örnekleri hakkında bilgi taşır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="fb50d-817">**Property** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-818">Documentation öğesi (sıfır veya bir öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="fb50d-819">Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="fb50d-820">Aşağıdaki modeller bir **özellik** öğesine uygulanabilir: **Null yapılabilir**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **UNICODE**, **harmanlama**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="fb50d-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="fb50d-821">Modeller, özellik değerlerinin veri deposunda nasıl depolandığı hakkında bilgi sağlayan XML öznitelikleridir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-822">Modeller yalnızca **Edmsimpletype**türünde özelliklere uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-823">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-823">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-824">Aşağıdaki tabloda, **özellik** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="fb50d-825">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-825">Attribute Name</span></span>                                                         | <span data-ttu-id="fb50d-826">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-826">Is Required</span></span> | <span data-ttu-id="fb50d-827">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-828">**Name**</span></span>                                                               | <span data-ttu-id="fb50d-829">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-829">Yes</span></span>         | <span data-ttu-id="fb50d-830">Özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="fb50d-831">**Tür**</span><span class="sxs-lookup"><span data-stu-id="fb50d-831">**Type**</span></span>                                                               | <span data-ttu-id="fb50d-832">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-832">Yes</span></span>         | <span data-ttu-id="fb50d-833">Özellik değerinin türü.</span><span class="sxs-lookup"><span data-stu-id="fb50d-833">The type of the property value.</span></span> <span data-ttu-id="fb50d-834">Özellik değeri türünün, modelin kapsamındaki bir **Edmsimpletype** veya karmaşık bir tür olması gerekir (tam olarak nitelenmiş bir ad ile belirtilir).</span><span class="sxs-lookup"><span data-stu-id="fb50d-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="fb50d-835">**Yapılamaz**</span><span class="sxs-lookup"><span data-stu-id="fb50d-835">**Nullable**</span></span>                                                           | <span data-ttu-id="fb50d-836">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-836">No</span></span>          | <span data-ttu-id="fb50d-837">**True** (varsayılan değer) veya özelliğin NULL değere sahip olmasına bağlı olarak <strong>false</strong> .</span><span class="sxs-lookup"><span data-stu-id="fb50d-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="fb50d-838">CSDL v1 'de > karmaşık bir tür özelliği @no__t olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="fb50d-839">**Değerinin**</span><span class="sxs-lookup"><span data-stu-id="fb50d-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="fb50d-840">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-840">No</span></span>          | <span data-ttu-id="fb50d-841">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="fb50d-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="fb50d-842">**'In**</span><span class="sxs-lookup"><span data-stu-id="fb50d-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="fb50d-843">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-843">No</span></span>          | <span data-ttu-id="fb50d-844">Özellik değerinin uzunluk üst sınırı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="fb50d-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="fb50d-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="fb50d-846">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-846">No</span></span>          | <span data-ttu-id="fb50d-847">Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="fb50d-848">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="fb50d-848">**Precision**</span></span>                                                          | <span data-ttu-id="fb50d-849">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-849">No</span></span>          | <span data-ttu-id="fb50d-850">Özellik değerinin duyarlığı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="fb50d-851">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="fb50d-851">**Scale**</span></span>                                                              | <span data-ttu-id="fb50d-852">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-852">No</span></span>          | <span data-ttu-id="fb50d-853">Özellik değerinin ölçeği.</span><span class="sxs-lookup"><span data-stu-id="fb50d-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="fb50d-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="fb50d-854">**SRID**</span></span>                                                               | <span data-ttu-id="fb50d-855">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-855">No</span></span>          | <span data-ttu-id="fb50d-856">Uzamsal sistem başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="fb50d-857">Yalnızca uzamsal türlerin özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="fb50d-858">Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb50d-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="fb50d-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="fb50d-859">**Unicode**</span></span>                                                            | <span data-ttu-id="fb50d-860">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-860">No</span></span>          | <span data-ttu-id="fb50d-861">Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="fb50d-862">**Mediğinden**</span><span class="sxs-lookup"><span data-stu-id="fb50d-862">**Collation**</span></span>                                                          | <span data-ttu-id="fb50d-863">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-863">No</span></span>          | <span data-ttu-id="fb50d-864">Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="fb50d-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="fb50d-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="fb50d-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="fb50d-866">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-866">No</span></span>          | <span data-ttu-id="fb50d-867">**Hiçbiri** (varsayılan değer) veya **sabit**.</span><span class="sxs-lookup"><span data-stu-id="fb50d-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="fb50d-868">Değer **fixed**olarak ayarlandıysa, iyimser eşzamanlılık denetimlerinde Özellik değeri kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="fb50d-869">**Özellik** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="fb50d-870">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-871">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="fb50d-872">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-872">Example</span></span>

<span data-ttu-id="fb50d-873">Aşağıdaki örnek, üç **özellik** öğesi olan bir **EntityType** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="fb50d-874">Aşağıdaki örnek, beş **özellik** öğesi Içeren bir **complexType** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="fb50d-875">RowType öğe uygulaması</span><span class="sxs-lookup"><span data-stu-id="fb50d-875">RowType Element Application</span></span>

<span data-ttu-id="fb50d-876">**Özellik** öğeleri ( **RowType** öğesinin alt öğeleri olarak), model tanımlı bir işlevden geçirilecek veya döndürülen verilerin şeklini ve özelliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="fb50d-877">**Property** öğesi aşağıdaki alt öğelerden tam olarak birine sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="fb50d-878">Türünde</span><span class="sxs-lookup"><span data-stu-id="fb50d-878">CollectionType</span></span>
-   <span data-ttu-id="fb50d-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="fb50d-879">ReferenceType</span></span>
-   <span data-ttu-id="fb50d-880">RowType</span><span class="sxs-lookup"><span data-stu-id="fb50d-880">RowType</span></span>

<span data-ttu-id="fb50d-881">**Property** öğesi herhangi bir sayıda alt ek açıklama öğesine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-882">Ek açıklama öğelerine yalnızca CSDL v2 ve sonrasında izin verilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-883">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-883">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-884">Aşağıdaki tabloda, **özellik** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="fb50d-885">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-885">Attribute Name</span></span>                                                     | <span data-ttu-id="fb50d-886">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-886">Is Required</span></span> | <span data-ttu-id="fb50d-887">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-888">**Name**</span></span>                                                           | <span data-ttu-id="fb50d-889">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-889">Yes</span></span>         | <span data-ttu-id="fb50d-890">Özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="fb50d-891">**Tür**</span><span class="sxs-lookup"><span data-stu-id="fb50d-891">**Type**</span></span>                                                           | <span data-ttu-id="fb50d-892">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-892">Yes</span></span>         | <span data-ttu-id="fb50d-893">Özellik değerinin türü.</span><span class="sxs-lookup"><span data-stu-id="fb50d-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="fb50d-894">**Yapılamaz**</span><span class="sxs-lookup"><span data-stu-id="fb50d-894">**Nullable**</span></span>                                                       | <span data-ttu-id="fb50d-895">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-895">No</span></span>          | <span data-ttu-id="fb50d-896">**True** (varsayılan değer) veya özelliğin NULL değere sahip olmasına bağlı olarak **false** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="fb50d-897">CSDL v1 'de > karmaşık bir tür özelliği, @no__t sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="fb50d-898">**Değerinin**</span><span class="sxs-lookup"><span data-stu-id="fb50d-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="fb50d-899">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-899">No</span></span>          | <span data-ttu-id="fb50d-900">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="fb50d-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="fb50d-901">**'In**</span><span class="sxs-lookup"><span data-stu-id="fb50d-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="fb50d-902">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-902">No</span></span>          | <span data-ttu-id="fb50d-903">Özellik değerinin uzunluk üst sınırı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="fb50d-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="fb50d-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="fb50d-905">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-905">No</span></span>          | <span data-ttu-id="fb50d-906">Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="fb50d-907">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="fb50d-907">**Precision**</span></span>                                                      | <span data-ttu-id="fb50d-908">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-908">No</span></span>          | <span data-ttu-id="fb50d-909">Özellik değerinin duyarlığı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="fb50d-910">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="fb50d-910">**Scale**</span></span>                                                          | <span data-ttu-id="fb50d-911">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-911">No</span></span>          | <span data-ttu-id="fb50d-912">Özellik değerinin ölçeği.</span><span class="sxs-lookup"><span data-stu-id="fb50d-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="fb50d-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="fb50d-913">**SRID**</span></span>                                                           | <span data-ttu-id="fb50d-914">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-914">No</span></span>          | <span data-ttu-id="fb50d-915">Uzamsal sistem başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="fb50d-916">Yalnızca uzamsal türlerin özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="fb50d-917">Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb50d-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="fb50d-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="fb50d-918">**Unicode**</span></span>                                                        | <span data-ttu-id="fb50d-919">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-919">No</span></span>          | <span data-ttu-id="fb50d-920">Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="fb50d-921">**Mediğinden**</span><span class="sxs-lookup"><span data-stu-id="fb50d-921">**Collation**</span></span>                                                      | <span data-ttu-id="fb50d-922">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-922">No</span></span>          | <span data-ttu-id="fb50d-923">Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="fb50d-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="fb50d-924">**Özellik** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="fb50d-925">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-926">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="fb50d-927">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-927">Example</span></span>

<span data-ttu-id="fb50d-928">Aşağıdaki örnek, model tanımlı bir işlevin dönüş türünün şeklini tanımlamak için kullanılan **özellik** öğelerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="fb50d-929">PropertyRef öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="fb50d-930">Kavramsal şema tanım dili (CSDL) içindeki **Propertyref** öğesi, özelliğin aşağıdaki rollerden birini gerçekleştireceğini göstermek için bir varlık türünün özelliğine başvurur:</span><span class="sxs-lookup"><span data-stu-id="fb50d-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="fb50d-931">Varlığın anahtarının bir parçası (bir özellik veya kimliği tespit eden bir varlık türünün özellikler kümesi).</span><span class="sxs-lookup"><span data-stu-id="fb50d-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="fb50d-932">Bir veya daha fazla **Propertyref** öğesi bir varlık anahtarı tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="fb50d-933">Bir başvuru kısıtlamasının bağımlı veya birincil sonu.</span><span class="sxs-lookup"><span data-stu-id="fb50d-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="fb50d-934">**Propertyref** öğesi yalnızca ek açıklama öğelerine (sıfır veya daha fazla) alt öğe olarak sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-935">Ek açıklama öğelerine yalnızca CSDL v2 ve sonrasında izin verilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-936">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-936">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-937">Aşağıdaki tabloda, **Propertyref** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="fb50d-938">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-938">Attribute Name</span></span> | <span data-ttu-id="fb50d-939">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-939">Is Required</span></span> | <span data-ttu-id="fb50d-940">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="fb50d-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb50d-941">**Name**</span></span>       | <span data-ttu-id="fb50d-942">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-942">Yes</span></span>         | <span data-ttu-id="fb50d-943">Başvurulan özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-944">**Propertyref** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="fb50d-945">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-946">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-947">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-947">Example</span></span>

<span data-ttu-id="fb50d-948">Aşağıdaki örnekte bir varlık türü (**Book**) tanımlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="fb50d-949">Varlık anahtarı, varlık türünün **ISBN** özelliğine başvurarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="fb50d-950">Sonraki örnekte, iki Özellik (**ID** ve **publisherID**) bir başvuru kısıtlamasının asıl ve bağımlı uçları olduğunu göstermek için iki **propertyref** öğesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="fb50d-951">ReferenceType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="fb50d-952">Kavramsal şema tanım dili (CSDL) içindeki **ReferenceType** öğesi bir varlık türüne yönelik bir başvuru belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="fb50d-953">**ReferenceType** öğesi aşağıdaki öğelerin bir alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="fb50d-954">ReturnType (Işlev)</span><span class="sxs-lookup"><span data-stu-id="fb50d-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="fb50d-955">Parametre</span><span class="sxs-lookup"><span data-stu-id="fb50d-955">Parameter</span></span>
-   <span data-ttu-id="fb50d-956">Türünde</span><span class="sxs-lookup"><span data-stu-id="fb50d-956">CollectionType</span></span>

<span data-ttu-id="fb50d-957">Bir işlev için bir parametre veya dönüş türü tanımlarken, **ReferenceType** öğesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="fb50d-958">Bir **ReferenceType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-959">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-960">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-961">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-961">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-962">Aşağıdaki tabloda, **ReferenceType** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="fb50d-963">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-963">Attribute Name</span></span> | <span data-ttu-id="fb50d-964">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-964">Is Required</span></span> | <span data-ttu-id="fb50d-965">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="fb50d-966">**Tür**</span><span class="sxs-lookup"><span data-stu-id="fb50d-966">**Type**</span></span>       | <span data-ttu-id="fb50d-967">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-967">Yes</span></span>         | <span data-ttu-id="fb50d-968">Başvurulmakta olan varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-969">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **ReferenceType** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="fb50d-970">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-971">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-972">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-972">Example</span></span>

<span data-ttu-id="fb50d-973">Aşağıdaki örnek, bir **kişi** varlık türü başvurusunu kabul eden model tanımlı bir Işlevde bir **parametre** öğesinin alt öğesi olarak kullanılan **ReferenceType** öğesini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="fb50d-974">Aşağıdaki örnek, bir **kişi** varlık türüne başvuru döndüren model tanımlı bir Işlevde bir **ReturnType** (Function) öğesinin alt öğesi olarak kullanılan **ReferenceType** öğesini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="fb50d-975">ReferentialConstraint öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="fb50d-976">Kavramsal şema tanım dili (CSDL) içindeki bir **ReferentialConstraint** öğesi, ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamasına benzer işlevselliği tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="fb50d-977">Bir veritabanı tablosundaki bir sütunun (veya sütunlarının) başka bir tablonun birincil anahtarına başvurmasına benzer şekilde, bir varlık türünün özelliği (veya özellikleri) başka bir varlık türünün varlık anahtarına başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="fb50d-978">Başvurulan varlık türüne kısıtlamanın *asıl sonu* denir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="fb50d-979">Asıl ucuna başvuran varlık türüne kısıtlamanın *bağımlı sonu* denir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="fb50d-980">Bir varlık türünde sunulan bir yabancı anahtar, başka bir varlık türündeki bir özelliğe başvuruyorsa, **ReferentialConstraint** öğesi iki varlık türü arasında bir ilişki tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="fb50d-981">**ReferentialConstraint** öğesi iki varlık türünün birbiriyle nasıl ilişkili olduğu hakkında bilgi sağladığından, eşleme belirtim DILINDE (MSL) karşılık gelen bir associationsetmapping öğesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="fb50d-982">Yabancı anahtarları açığa çıkarmayan iki varlık türü arasındaki ilişki, ilişki bilgilerini veri kaynağıyla eşlemek için karşılık gelen bir **Associationsetmapping** öğesine sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="fb50d-983">Bir varlık türünde yabancı anahtar gösterilmediği takdirde, **ReferentialConstraint** öğesi varlık türü ve başka bir varlık türü arasında yalnızca birincil anahtar-birincil anahtar kısıtlaması tanımlayabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="fb50d-984">Bir **ReferentialConstraint** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-985">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-986">Asıl (tam olarak bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="fb50d-987">Bağımlı (tam olarak bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="fb50d-988">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-989">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-989">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-990">**ReferentialConstraint** öğesinde herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="fb50d-991">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-992">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="fb50d-993">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-993">Example</span></span>

<span data-ttu-id="fb50d-994">Aşağıdaki örnek, **PublishedBy** Association tanımının bir parçası olarak kullanılan **ReferentialConstraint** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="fb50d-995">ReturnType (Işlev) öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="fb50d-996">Kavramsal şema tanım dili (CSDL) içindeki **ReturnType** (işlev) öğesi, bir işlev öğesinde tanımlanan bir işlev için dönüş türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="fb50d-997">Bir işlev dönüş türü, bir **ReturnType** özniteliğiyle de belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="fb50d-998">Dönüş türleri herhangi bir **Edmsimpletype**, varlık türü, karmaşık tür, satır türü, başvuru türü veya bu türlerden birinin bir koleksiyonu olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="fb50d-999">Bir işlevin dönüş türü, **ReturnType** (Function) öğesinin **tür** özniteliğiyle ya da aşağıdaki alt öğelerinden biri ile belirtilebilir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="fb50d-1000">Türünde</span><span class="sxs-lookup"><span data-stu-id="fb50d-1000">CollectionType</span></span>
-   <span data-ttu-id="fb50d-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="fb50d-1001">ReferenceType</span></span>
-   <span data-ttu-id="fb50d-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="fb50d-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-1003">Yalnızca **ReturnType** (Function) öğesinin **Type** özniteliği ve alt öğelerinden biri ile bir işlev dönüş türü belirtirseniz bir model doğrulanmaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-1004">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-1004">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-1005">Aşağıdaki tabloda, **ReturnType** (Function) öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="fb50d-1006">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-1006">Attribute Name</span></span> | <span data-ttu-id="fb50d-1007">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-1007">Is Required</span></span> | <span data-ttu-id="fb50d-1008">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="fb50d-1009">**'Indaki**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1009">**ReturnType**</span></span> | <span data-ttu-id="fb50d-1010">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1010">No</span></span>          | <span data-ttu-id="fb50d-1011">İşlev tarafından döndürülen tür.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-1012">Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **ReturnType** (Function) öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="fb50d-1013">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-1014">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-1015">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-1015">Example</span></span>

<span data-ttu-id="fb50d-1016">Aşağıdaki örnek, bir kitabın yazdırıldığınız yıl sayısını döndüren bir işlev tanımlamak için bir **işlev** öğesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="fb50d-1017">Dönüş türünün bir **ReturnType** (Function) öğesinin **Type** özniteliğiyle belirtildiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="fb50d-1018">ReturnType (FunctionImport) öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="fb50d-1019">Kavramsal şema tanım dili (CSDL) içindeki **ReturnType** (FunctionImport) öğesi, bir FunctionImport öğesinde tanımlanan bir işlevin dönüş türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="fb50d-1020">Bir işlev dönüş türü, bir **ReturnType** özniteliğiyle de belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="fb50d-1021">Dönüş türleri herhangi bir varlık türü, karmaşık tür veya **Edmsimpletype**koleksiyonu olabilir,</span><span class="sxs-lookup"><span data-stu-id="fb50d-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="fb50d-1022">İşlevin dönüş türü, **ReturnType** (FunctionImport) öğesinin **tür** özniteliğiyle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-1023">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-1023">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-1024">Aşağıdaki tabloda, **ReturnType** (FunctionImport) öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="fb50d-1025">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-1025">Attribute Name</span></span> | <span data-ttu-id="fb50d-1026">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-1026">Is Required</span></span> | <span data-ttu-id="fb50d-1027">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-1028">**Tür**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1028">**Type**</span></span>       | <span data-ttu-id="fb50d-1029">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1029">No</span></span>          | <span data-ttu-id="fb50d-1030">İşlevin döndürdüğü tür.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1030">The type that the function returns.</span></span> <span data-ttu-id="fb50d-1031">Değer, ComplexType, EntityType veya EDMSimpleType bir koleksiyon olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="fb50d-1032">**Di**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1032">**EntitySet**</span></span>  | <span data-ttu-id="fb50d-1033">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1033">No</span></span>          | <span data-ttu-id="fb50d-1034">İşlev bir varlık türleri koleksiyonu döndürürse, **EntitySet** 'in değeri koleksiyonun ait olduğu varlık kümesi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="fb50d-1035">Aksi takdirde, **EntitySet** özniteliği kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-1036">Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **ReturnType** (FunctionImport) öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="fb50d-1037">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-1038">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-1039">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-1039">Example</span></span>

<span data-ttu-id="fb50d-1040">Aşağıdaki örnek, kitap ve yayımcılar döndüren bir **FunctionImport** kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="fb50d-1041">İşlevin iki sonuç kümesi döndürdüğüne ve bu nedenle iki **ReturnType** (FunctionImport) öğesi belirtildiğine unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="fb50d-1042">RowType öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="fb50d-1043">Kavramsal şema tanım dili (CSDL) içindeki **RowType** öğesi, kavramsal modelde tanımlanan bir işlev için parametre veya dönüş türü olarak adlandırılmamış bir yapı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="fb50d-1044">**RowType** öğesi aşağıdaki öğelerin alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="fb50d-1045">Türünde</span><span class="sxs-lookup"><span data-stu-id="fb50d-1045">CollectionType</span></span>
-   <span data-ttu-id="fb50d-1046">Parametre</span><span class="sxs-lookup"><span data-stu-id="fb50d-1046">Parameter</span></span>
-   <span data-ttu-id="fb50d-1047">ReturnType (Işlev)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1047">ReturnType (Function)</span></span>

<span data-ttu-id="fb50d-1048">**RowType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-1049">Özellik (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1049">Property (one or more)</span></span>
-   <span data-ttu-id="fb50d-1050">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-1051">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-1051">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-1052">**RowType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="fb50d-1053">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-1054">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="fb50d-1055">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-1055">Example</span></span>

<span data-ttu-id="fb50d-1056">Aşağıdaki örnek, işlevin satır koleksiyonunu ( **RowType** öğesinde belirtilen şekilde) döndürdüğünü belirtmek Için bir **CollectionType** öğesi kullanan model tanımlı bir işlevi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="fb50d-1057">Şema öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="fb50d-1058">**Şema** öğesi, bir kavramsal model tanımının kök öğesidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="fb50d-1059">Kavramsal modeli oluşturan nesneler, işlevler ve kapsayıcılar için tanımlar içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="fb50d-1060">**Şema** öğesi aşağıdaki alt öğeleri sıfır veya daha fazla içerebilir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="fb50d-1061">Kullanarak</span><span class="sxs-lookup"><span data-stu-id="fb50d-1061">Using</span></span>
-   <span data-ttu-id="fb50d-1062">'Indaki</span><span class="sxs-lookup"><span data-stu-id="fb50d-1062">EntityContainer</span></span>
-   <span data-ttu-id="fb50d-1063">entityType</span><span class="sxs-lookup"><span data-stu-id="fb50d-1063">EntityType</span></span>
-   <span data-ttu-id="fb50d-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="fb50d-1064">EnumType</span></span>
-   <span data-ttu-id="fb50d-1065">Kaldırma</span><span class="sxs-lookup"><span data-stu-id="fb50d-1065">Association</span></span>
-   <span data-ttu-id="fb50d-1066">Türündedir</span><span class="sxs-lookup"><span data-stu-id="fb50d-1066">ComplexType</span></span>
-   <span data-ttu-id="fb50d-1067">İşlev</span><span class="sxs-lookup"><span data-stu-id="fb50d-1067">Function</span></span>

<span data-ttu-id="fb50d-1068">**Şema** öğesi sıfır veya bir ek açıklama öğesi içerebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-1069">**Function** öğesi ve Annotation ÖĞELERINE yalnızca csdl v2 ve üzeri sürümlerde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="fb50d-1070">**Şema** öğesi, kavramsal bir modeldeki varlık türü, karmaşık tür ve ilişkilendirme nesneleri için ad alanını tanımlamak üzere **Namespace** özniteliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="fb50d-1071">Bir ad alanı içinde, iki nesne aynı ada sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="fb50d-1072">Ad alanları, birden çok **şema** öğesine ve birden çok. csdl dosyasına yayılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="fb50d-1073">Kavramsal model ad alanı, **şema** öğesinin XML ad alanından farklıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="fb50d-1074">Kavramsal model ad alanı ( **ad alanı** özniteliği tarafından tanımlandığı gibi) varlık türleri, karmaşık türler ve ilişkilendirme türleri için bir mantıksal kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="fb50d-1075">Bir **şema** öğesinin XML ad alanı ( **xmlns** özniteliğiyle gösterilir), alt öğeler ve **şema** öğesinin öznitelikleri için varsayılan ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="fb50d-1076">@No__t-0 (YYYY ve MM, sırasıyla bir yılı ve ayı temsil eder) biçiminde olan XML ad alanları CSDL için ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="fb50d-1077">Özel öğeler ve öznitelikler bu forma sahip ad alanlarında olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-1078">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-1078">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-1079">Aşağıdaki tabloda, özniteliklerin **şema** öğesine uygulanabileceğini açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="fb50d-1080">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-1080">Attribute Name</span></span> | <span data-ttu-id="fb50d-1081">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-1081">Is Required</span></span> | <span data-ttu-id="fb50d-1082">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1083">**Namespace**</span></span>  | <span data-ttu-id="fb50d-1084">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1084">Yes</span></span>         | <span data-ttu-id="fb50d-1085">Kavramsal modelin ad alanı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="fb50d-1086">**Ad alanı** özniteliğinin değeri, bir türün tam nitelikli adını biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="fb50d-1087">Örneğin, *Müşteri* adlı bir **EntityType** basit. example. model ad alanında ise, **EntityType** 'ın tam adı simpleexamplemodel. Customer olur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="fb50d-1088">Şu dizeler **ad alanı** özniteliği için değer olarak kullanılamaz: **Sistem**, **geçici**veya **EDM**.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="fb50d-1089">**Namespace** özniteliğinin DEĞERI, SSDL şema öğesindeki **Namespace** özniteliğinin değeri ile aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="fb50d-1090">**Diğer ad**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1090">**Alias**</span></span>      | <span data-ttu-id="fb50d-1091">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1091">No</span></span>          | <span data-ttu-id="fb50d-1092">Ad alanı adı yerine kullanılan tanımlayıcı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="fb50d-1093">Örneğin, *Müşteri* adlı bir **EntityType** basit. example. model ad alanı ve **diğer ad** özniteliğinin değeri *Modelise*model. Customer ' i EntityType 'ın tam adı olarak kullanabilirsiniz **.**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="fb50d-1094">**Şema** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="fb50d-1095">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-1096">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-1097">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-1097">Example</span></span>

<span data-ttu-id="fb50d-1098">Aşağıdaki örnek, bir **EntityContainer** öğesi, iki **EntityType** öğesi ve bir **ilişkilendirme** öğesi içeren bir **şema** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="fb50d-1099">TypeRef öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="fb50d-1100">Kavramsal şema tanım dili (CSDL) içindeki **TypeRef** öğesi, var olan bir adlandırılmış türe başvuru sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="fb50d-1101">**TypeRef** öğesi, bir işlevin bir parametre ya da dönüş türü olarak bir koleksiyon olduğunu belirtmek Için kullanılan CollectionType öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="fb50d-1102">**TypeRef** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb50d-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb50d-1103">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="fb50d-1104">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-1105">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-1105">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-1106">Aşağıdaki tabloda, **TypeRef** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="fb50d-1107">**DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **UNICODE**ve **harmanlama** özniteliklerinin yalnızca **edmsimpletypes**için geçerli olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="fb50d-1108">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="fb50d-1109">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-1109">Is Required</span></span> | <span data-ttu-id="fb50d-1110">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-1111">**Tür**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1111">**Type**</span></span>                                                           | <span data-ttu-id="fb50d-1112">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1112">No</span></span>          | <span data-ttu-id="fb50d-1113">Başvurulmakta olan türün adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="fb50d-1114">**Yapılamaz**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="fb50d-1115">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1115">No</span></span>          | <span data-ttu-id="fb50d-1116">**True** (varsayılan değer) veya özelliğin NULL değere sahip olmasına bağlı olarak **false** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="fb50d-1117">CSDL v1 'de > karmaşık bir tür özelliği, @no__t sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="fb50d-1118">**Değerinin**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="fb50d-1119">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1119">No</span></span>          | <span data-ttu-id="fb50d-1120">Özelliğin varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="fb50d-1121">**'In**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="fb50d-1122">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1122">No</span></span>          | <span data-ttu-id="fb50d-1123">Özellik değerinin uzunluk üst sınırı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="fb50d-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="fb50d-1125">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1125">No</span></span>          | <span data-ttu-id="fb50d-1126">Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="fb50d-1127">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1127">**Precision**</span></span>                                                      | <span data-ttu-id="fb50d-1128">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1128">No</span></span>          | <span data-ttu-id="fb50d-1129">Özellik değerinin duyarlığı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="fb50d-1130">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1130">**Scale**</span></span>                                                          | <span data-ttu-id="fb50d-1131">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1131">No</span></span>          | <span data-ttu-id="fb50d-1132">Özellik değerinin ölçeği.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="fb50d-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1133">**SRID**</span></span>                                                           | <span data-ttu-id="fb50d-1134">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1134">No</span></span>          | <span data-ttu-id="fb50d-1135">Uzamsal sistem başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="fb50d-1136">Yalnızca uzamsal türlerin özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="fb50d-1137">Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb50d-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="fb50d-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="fb50d-1139">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1139">No</span></span>          | <span data-ttu-id="fb50d-1140">Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb50d-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="fb50d-1141">**Mediğinden**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1141">**Collation**</span></span>                                                      | <span data-ttu-id="fb50d-1142">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1142">No</span></span>          | <span data-ttu-id="fb50d-1143">Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="fb50d-1144">**CollectionType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="fb50d-1145">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-1146">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-1147">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-1147">Example</span></span>

<span data-ttu-id="fb50d-1148">Aşağıdaki örnek, işlevin bir **Departman** varlık türleri koleksiyonunu kabul ettiğini belirtmek için **TypeRef** öğesini (bir **CollectionType** öğesinin alt öğesi olarak) kullanan model tanımlı bir işlevi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="fb50d-1149">Using öğesi (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="fb50d-1150">Kavramsal şema tanım dili (CSDL) içindeki **using** öğesi, farklı bir ad alanında bulunan bir kavramsal modelin içeriğini içeri aktarır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="fb50d-1151">**Ad alanı** özniteliğinin değerini ayarlayarak, başka bir kavramsal modelde tanımlanan varlık türlerine, karmaşık türlere ve ilişkilendirme türlerine başvurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="fb50d-1152">Birden fazla **using** öğesi bir **şema** öğesinin alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-1153">CSDL içindeki **using** öğesi, programlama dilinde bir **using** ifadesiyle tam olarak çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="fb50d-1154">Bir programlama dilinde **using** ifadesiyle bir ad alanı içe aktararak, özgün ad alanındaki nesneleri etkilemeyin.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="fb50d-1155">CSDL 'de, içeri aktarılan bir ad alanı özgün ad alanındaki bir varlık türünden türetilmiş bir varlık türü içerebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="fb50d-1156">Bu, özgün ad alanında belirtilen varlık kümelerini etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="fb50d-1157">**Using** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb50d-1158">Belgeler (sıfır veya bir öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="fb50d-1159">Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb50d-1160">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb50d-1160">Applicable Attributes</span></span>

<span data-ttu-id="fb50d-1161">Aşağıdaki tabloda, **using** ögesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="fb50d-1162">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb50d-1162">Attribute Name</span></span> | <span data-ttu-id="fb50d-1163">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb50d-1163">Is Required</span></span> | <span data-ttu-id="fb50d-1164">Value</span><span class="sxs-lookup"><span data-stu-id="fb50d-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1165">**Namespace**</span></span>  | <span data-ttu-id="fb50d-1166">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1166">Yes</span></span>         | <span data-ttu-id="fb50d-1167">İçeri aktarılan ad alanının adı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="fb50d-1168">**Diğer ad**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1168">**Alias**</span></span>      | <span data-ttu-id="fb50d-1169">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1169">Yes</span></span>         | <span data-ttu-id="fb50d-1170">Ad alanı adı yerine kullanılan tanımlayıcı.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="fb50d-1171">Bu öznitelik gerekli olsa da, nesne adlarını nitelemek için ad alanı adı yerine kullanılması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="fb50d-1172">**Using** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="fb50d-1173">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="fb50d-1174">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="fb50d-1175">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-1175">Example</span></span>

<span data-ttu-id="fb50d-1176">Aşağıdaki örnek, başka bir yerde tanımlanmış bir ad alanını içeri aktarmak için kullanılan **using** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="fb50d-1177">Gösterilen **şema** öğesi için ad alanının `BooksModel` olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="fb50d-1178">@No__t-1**EntityType** 'daki `Address` özelliği, `ExtendedBooksModel` ad alanında tanımlanan karmaşık bir türdür ( **using** öğesiyle içeri aktarılır).</span><span class="sxs-lookup"><span data-stu-id="fb50d-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="fb50d-1179">Ek açıklama öznitelikleri (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="fb50d-1180">Kavramsal şema tanım dili (CSDL) içindeki ek açıklama öznitelikleri, kavramsal modelde özel XML öznitelikleridir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="fb50d-1181">Geçerli XML yapısına ek olarak, aşağıdaki ek açıklama özniteliklerinin doğru olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="fb50d-1182">Ek açıklama öznitelikleri, CSDL için ayrılan XML ad alanı içinde olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="fb50d-1183">Verilen bir CSDL öğesine birden fazla ek açıklama özniteliği uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="fb50d-1184">İki ek açıklama özniteliğinin tam nitelikli adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="fb50d-1185">Ek açıklama öznitelikleri, kavramsal bir modeldeki öğeler hakkında ek meta veriler sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="fb50d-1186">Ek açıklama öğelerinde içerilen meta verilere, System. Data. Metadata. Edm ad alanındaki sınıflar kullanılarak çalışma zamanında erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="fb50d-1187">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-1187">Example</span></span>

<span data-ttu-id="fb50d-1188">Aşağıdaki örnek, bir ek açıklama özniteliği (**CustomAttribute**) Ile bir **EntityType** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="fb50d-1189">Örnek ayrıca varlık türü öğesine uygulanan bir ek açıklama öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="fb50d-1190">Aşağıdaki kod, ek açıklama özniteliğinde meta verileri alır ve konsola yazar:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="fb50d-1191">Yukarıdaki kod `School.csdl` dosyasının projenin çıkış dizininde olduğunu ve aşağıdaki `Imports` ve `Using` deyimlerini projenize eklediğinizi varsayar:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="fb50d-1192">Ek açıklama öğeleri (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="fb50d-1193">Kavramsal şema tanım dili (CSDL) içindeki ek açıklama öğeleri, kavramsal modeldeki özel XML öğeleridir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="fb50d-1194">Geçerli XML yapısına ek olarak, aşağıdaki ek açıklama öğelerinin doğru olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="fb50d-1195">Ek açıklama öğeleri, CSDL için ayrılan hiçbir XML ad alanı içinde olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="fb50d-1196">Birden fazla ek açıklama öğesi, verili bir CSDL öğesinin alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="fb50d-1197">İki ek açıklama öğesinin tam nitelikli adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="fb50d-1198">Ek açıklama öğeleri, belirli bir CSDL öğesinin tüm diğer alt öğelerinden sonra görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="fb50d-1199">Ek açıklama öğeleri, kavramsal bir modeldeki öğeler hakkında ek meta veriler sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="fb50d-1200">.NET Framework sürüm 4 ' te başlayarak, ek açıklama öğelerinde içerilen meta verilere System. Data. Metadata. Edm ad alanındaki sınıflar kullanılarak çalışma zamanında erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="fb50d-1201">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-1201">Example</span></span>

<span data-ttu-id="fb50d-1202">Aşağıdaki örnek, bir ek açıklama öğesi (**CustomElement**) Ile bir **EntityType** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="fb50d-1203">Örnek ayrıca varlık türü öğesine uygulanan bir ek açıklama özniteliği gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="fb50d-1204">Aşağıdaki kod, ek açıklama öğesindeki meta verileri alır ve konsola yazar:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="fb50d-1205">Yukarıdaki kod, okul. csdl dosyasının projenin çıkış dizininde olduğunu ve aşağıdaki `Imports` ve `Using` deyimlerini projenize eklediğinizi varsayar:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="fb50d-1206">Kavramsal model türleri (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="fb50d-1207">Kavramsal şema tanım dili (CSDL), kavramsal bir modeldeki özellikleri tanımlayan **Edmsimpletypes**adlı bir soyut temel veri türleri kümesini destekler.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="fb50d-1208">**Edmsimpletypes** , depolama veya barındırma ortamında desteklenen temel veri türleri için proxy 'lardır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="fb50d-1209">Aşağıdaki tabloda, CSDL tarafından desteklenen temel veri türleri listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="fb50d-1210">Tablo, her **Edmsimpletype**öğesine uygulanabilen modelleri de listeler.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="fb50d-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="fb50d-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="fb50d-1212">Açıklama</span><span class="sxs-lookup"><span data-stu-id="fb50d-1212">Description</span></span>                                                | <span data-ttu-id="fb50d-1213">Uygulanabilir modeller</span><span class="sxs-lookup"><span data-stu-id="fb50d-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="fb50d-1214">**EDM. Binary**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="fb50d-1215">İkili verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="fb50d-1216">MaxLength, FixedLength, Nullable, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="fb50d-1217">**EDM. Boolean**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="fb50d-1218">**True** veya **false**değerini içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="fb50d-1219">Null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="fb50d-1220">**EDM. Byte**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="fb50d-1221">İşaretsiz 8 bit tamsayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="fb50d-1222">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="fb50d-1224">Bir tarih ve saati temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="fb50d-1225">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1226">**EDM. DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="fb50d-1227">GMT cinsinden dakika cinsinden bir tarih ve saat içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="fb50d-1228">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1229">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="fb50d-1230">Sabit duyarlığa ve ölçeğe sahip sayısal bir değer içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="fb50d-1231">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1232">**EDM. Double**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="fb50d-1233">15 basamaklı duyarlık içeren bir kayan nokta numarası içerir</span><span class="sxs-lookup"><span data-stu-id="fb50d-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="fb50d-1234">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="fb50d-1236">7 basamaklı duyarlık içeren bir kayan nokta numarası içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="fb50d-1237">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1238">**EDM. Guid**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="fb50d-1239">16 baytlık benzersiz bir tanımlayıcı içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="fb50d-1240">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="fb50d-1242">İşaretli 16 bit tamsayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="fb50d-1243">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1244">**EDM. Int32**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="fb50d-1245">İmzalı 32 bitlik bir tamsayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="fb50d-1246">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1247">**EDM. Int64**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="fb50d-1248">İmzalı 64 bitlik bir tamsayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="fb50d-1249">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="fb50d-1251">İşaretli 8 bit tamsayı değeri içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="fb50d-1252">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1253">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1253">**Edm.String**</span></span>                   | <span data-ttu-id="fb50d-1254">Karakter verisi içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1254">Contains character data.</span></span>                                   | <span data-ttu-id="fb50d-1255">Unicode, FixedLength, MaxLength, harmanlama, duyarlık, Nullable, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="fb50d-1256">**EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="fb50d-1257">Günün saatini içerir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="fb50d-1258">Duyarlılık, null yapılabilir, varsayılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="fb50d-1259">**EDM. Coğrafya**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="fb50d-1260">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1261">**EDM. Geographyıpoint**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="fb50d-1262">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1263">**EDM. Geographyılinestring**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="fb50d-1264">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1265">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="fb50d-1266">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1267">**EDM. Geographyımultipoint**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="fb50d-1268">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1269">**EDM. Geographyımultilinestring**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="fb50d-1270">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1271">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="fb50d-1272">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1273">**EDM. Geographrivcollection**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="fb50d-1274">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="fb50d-1276">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1277">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="fb50d-1278">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1279">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="fb50d-1280">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1281">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="fb50d-1282">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1283">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="fb50d-1284">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1285">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="fb50d-1286">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1287">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="fb50d-1288">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="fb50d-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="fb50d-1290">Null yapılabilir, varsayılan, SRID</span><span class="sxs-lookup"><span data-stu-id="fb50d-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="fb50d-1291">Modeller (CSDL)</span><span class="sxs-lookup"><span data-stu-id="fb50d-1291">Facets (CSDL)</span></span>

<span data-ttu-id="fb50d-1292">Kavramsal şema tanım dili (CSDL) içindeki modeller varlık türlerinin ve karmaşık türlerin özelliklerindeki kısıtlamaları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="fb50d-1293">Modeller aşağıdaki CSDL öğelerinde XML öznitelikleri olarak görünür:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="fb50d-1294">Özellik</span><span class="sxs-lookup"><span data-stu-id="fb50d-1294">Property</span></span>
-   <span data-ttu-id="fb50d-1295">Değerini</span><span class="sxs-lookup"><span data-stu-id="fb50d-1295">TypeRef</span></span>
-   <span data-ttu-id="fb50d-1296">Parametre</span><span class="sxs-lookup"><span data-stu-id="fb50d-1296">Parameter</span></span>

<span data-ttu-id="fb50d-1297">Aşağıdaki tabloda, CSDL 'de desteklenen modeller açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="fb50d-1298">Tüm modeller isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1298">All facets are optional.</span></span> <span data-ttu-id="fb50d-1299">Aşağıda listelenen bazı modeller kavramsal bir modelden veritabanı oluştururken Entity Framework tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="fb50d-1300">Kavramsal modeldeki veri türleri hakkında daha fazla bilgi için bkz. kavramsal model türleri (CSDL).</span><span class="sxs-lookup"><span data-stu-id="fb50d-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="fb50d-1301">Kısıtlayan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1301">Facet</span></span>               | <span data-ttu-id="fb50d-1302">Açıklama</span><span class="sxs-lookup"><span data-stu-id="fb50d-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="fb50d-1303">Şunlara uygulanacaktır:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="fb50d-1304">Veritabanı oluşturma için kullanılır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1304">Used for the database generation</span></span> | <span data-ttu-id="fb50d-1305">Çalışma zamanı tarafından kullanılan</span><span class="sxs-lookup"><span data-stu-id="fb50d-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="fb50d-1306">**Mediğinden**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1306">**Collation**</span></span>       | <span data-ttu-id="fb50d-1307">Özelliğin değerlerinde karşılaştırma ve sıralama işlemleri gerçekleştirirken kullanılacak harmanlama sırasını (veya sıralama sırasını) belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="fb50d-1308">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="fb50d-1309">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1309">Yes</span></span>                              | <span data-ttu-id="fb50d-1310">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1310">No</span></span>                  |
| <span data-ttu-id="fb50d-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="fb50d-1312">Özellik değerinin iyimser eşzamanlılık denetimleri için kullanılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="fb50d-1313">Tüm **Edmsimpletype** özellikleri</span><span class="sxs-lookup"><span data-stu-id="fb50d-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="fb50d-1314">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1314">No</span></span>                               | <span data-ttu-id="fb50d-1315">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1315">Yes</span></span>                 |
| <span data-ttu-id="fb50d-1316">**Varsayılan**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1316">**Default**</span></span>         | <span data-ttu-id="fb50d-1317">Örnek oluşturma sırasında hiçbir değer sağlanmadığında özelliğin varsayılan değerini belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="fb50d-1318">Tüm **Edmsimpletype** özellikleri</span><span class="sxs-lookup"><span data-stu-id="fb50d-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="fb50d-1319">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1319">Yes</span></span>                              | <span data-ttu-id="fb50d-1320">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1320">Yes</span></span>                 |
| <span data-ttu-id="fb50d-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1321">**FixedLength**</span></span>     | <span data-ttu-id="fb50d-1322">Özellik değerinin uzunluğunun değişebileceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="fb50d-1323">**Edm. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="fb50d-1324">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1324">Yes</span></span>                              | <span data-ttu-id="fb50d-1325">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1325">No</span></span>                  |
| <span data-ttu-id="fb50d-1326">**'In**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1326">**MaxLength**</span></span>       | <span data-ttu-id="fb50d-1327">Özellik değerinin uzunluk üst sınırını belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="fb50d-1328">**Edm. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="fb50d-1329">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1329">Yes</span></span>                              | <span data-ttu-id="fb50d-1330">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1330">No</span></span>                  |
| <span data-ttu-id="fb50d-1331">**Yapılamaz**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1331">**Nullable**</span></span>        | <span data-ttu-id="fb50d-1332">Özelliğin **null** değere sahip olup olmayacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="fb50d-1333">Tüm **Edmsimpletype** özellikleri</span><span class="sxs-lookup"><span data-stu-id="fb50d-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="fb50d-1334">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1334">Yes</span></span>                              | <span data-ttu-id="fb50d-1335">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1335">Yes</span></span>                 |
| <span data-ttu-id="fb50d-1336">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1336">**Precision**</span></span>       | <span data-ttu-id="fb50d-1337">**Decimal**türü özellikler için, bir özellik değerinin sahip olduğu basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="fb50d-1338">**Time**, **DateTime**ve **DateTimeOffset**türündeki özellikler için, özellik değerinin saniyenin kısmi bölümü için basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="fb50d-1339">**Edm. DateTime**, **Edm. DateTimeOffset**, **Edm. Decimal**, **Edm. Time**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="fb50d-1340">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1340">Yes</span></span>                              | <span data-ttu-id="fb50d-1341">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1341">No</span></span>                  |
| <span data-ttu-id="fb50d-1342">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1342">**Scale**</span></span>           | <span data-ttu-id="fb50d-1343">Özellik değeri için ondalık noktanın sağ tarafındaki basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="fb50d-1344">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="fb50d-1345">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1345">Yes</span></span>                              | <span data-ttu-id="fb50d-1346">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1346">No</span></span>                  |
| <span data-ttu-id="fb50d-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1347">**SRID**</span></span>            | <span data-ttu-id="fb50d-1348">Uzamsal sistem başvurusu sistem KIMLIĞINI belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="fb50d-1349">Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb50d-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="fb50d-1350">**EDM. coğrafya, Edm. Geographyıpoint, Edm. Geographyılinestring, Edm. GeographyPolygon, Edm. Geographyımultipoint, Edm. Geographyımultilinestring, Edm. GeographyMultiPolygon, Edm. Geographyıcollection, Edm. Geometry, Edm. GeometryPoint EDM. GeometryLineString, Edm. GeometryPolygon, Edm. GeometryMultiPoint, Edm. GeometryMultiLineString, Edm. GeometryMultiPolygon, Edm. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="fb50d-1351">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb50d-1351">No</span></span>                               | <span data-ttu-id="fb50d-1352">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1352">Yes</span></span>                 |
| <span data-ttu-id="fb50d-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1353">**Unicode**</span></span>         | <span data-ttu-id="fb50d-1354">Özellik değerinin Unicode olarak depolandığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="fb50d-1355">**EDM. String**</span><span class="sxs-lookup"><span data-stu-id="fb50d-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="fb50d-1356">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1356">Yes</span></span>                              | <span data-ttu-id="fb50d-1357">Evet</span><span class="sxs-lookup"><span data-stu-id="fb50d-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="fb50d-1358">Kavramsal bir modelden veritabanı oluştururken, veritabanı oluştur Sihirbazı, **StoreGeneratedPattern** özniteliğinin değerini şu ad alanında ise, bir **özellik** öğesi üzerinde algılar: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="fb50d-1359">Öznitelik için desteklenen değerler **Identity** ve **hesaplandı**.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="fb50d-1360">**Kimlik** değeri, veritabanında oluşturulan kimlik değeri ile bir veritabanı sütunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="fb50d-1361">**Hesaplanan** değeri, veritabanında hesaplanan bir değere sahip bir sütun oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fb50d-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="fb50d-1362">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb50d-1362">Example</span></span>

<span data-ttu-id="fb50d-1363">Aşağıdaki örnek bir varlık türünün özelliklerine uygulanan modelleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb50d-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
