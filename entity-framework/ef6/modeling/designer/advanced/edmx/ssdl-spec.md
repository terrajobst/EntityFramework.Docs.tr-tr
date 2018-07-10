---
title: SSDL belirtimi - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
caps.latest.revision: 3
ms.openlocfilehash: a9977c80d9a9401afdcad2284a705bcb28790fb8
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914476"
---
# <a name="ssdl-specification"></a><span data-ttu-id="097ba-102">SSDL belirtimi</span><span class="sxs-lookup"><span data-stu-id="097ba-102">SSDL Specification</span></span>
<span data-ttu-id="097ba-103">Store şeması tanım dili (SSDL) açıklayan bir varlık çerçevesi uygulamasına depolama modelinin bir XML tabanlı bir dildir.</span><span class="sxs-lookup"><span data-stu-id="097ba-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="097ba-104">Bir Entity Framework uygulamasında, depolama model meta verilerini (SSDL içinde yazılan) .ssdl dosyasından System.Data.Metadata.Edm.StoreItemCollection bir örneğine yüklenir ve yöntemleri kullanılarak erişilebilir System.Data.Metadata.Edm.MetadataWorkspace sınıfı.</span><span class="sxs-lookup"><span data-stu-id="097ba-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="097ba-105">Entity Framework depolama model meta veri deposu özgü komutlar için kavramsal modeline karşı sorgular çevirmek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="097ba-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="097ba-106">Entity Framework Designer (EF Designer), tasarım zamanında bir .edmx dosyası içinde depolama model bilgileri depolar.</span><span class="sxs-lookup"><span data-stu-id="097ba-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="097ba-107">Derleme sırasında varlık Tasarımcısı Entity Framework tarafından çalışma zamanında gereken .ssdl dosyası oluşturmak için bir .edmx dosyası içinde bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="097ba-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="097ba-108">SSDL sürümleri, XML ad alanları tarafından ayrılır.</span><span class="sxs-lookup"><span data-stu-id="097ba-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="097ba-109">SSDL sürümü</span><span class="sxs-lookup"><span data-stu-id="097ba-109">SSDL Version</span></span> | <span data-ttu-id="097ba-110">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="097ba-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="097ba-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="097ba-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="097ba-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="097ba-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="097ba-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="097ba-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="097ba-114">Association öğesinde (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-114">Association Element (SSDL)</span></span>

<span data-ttu-id="097ba-115">Bir **ilişkilendirme** depo şeması tanım dili (SSDL) öğesinde bir yabancı anahtar kısıtlaması temel alınan veritabanında katılan tablo sütunları belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="097ba-116">İki gerekli alt End öğesi, ilişkilendirmenin bir ucunda tabloları ve her iki ucunda çoğulluk belirtin.</span><span class="sxs-lookup"><span data-stu-id="097ba-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="097ba-117">İsteğe bağlı bir Referentialconstraint'teki öğe katılan sütunlar yanı sıra, ilişkilendirmenin birincil ve bağımlı sonunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="097ba-118">Hayır ise **Referentialconstraint'teki** öğe varsa, AssociationSetMapping öğenin ilişkilendirme için sütun eşleşmelerini belirtmek için kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="097ba-119">**İlişkilendirme** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-120">Belgeleri (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="097ba-121">Bitiş (tam olarak iki)</span><span class="sxs-lookup"><span data-stu-id="097ba-121">End (exactly two)</span></span>
-   <span data-ttu-id="097ba-122">Referentialconstraint'teki (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="097ba-123">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-124">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-124">Applicable Attributes</span></span>

<span data-ttu-id="097ba-125">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="097ba-126">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-126">Attribute Name</span></span> | <span data-ttu-id="097ba-127">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-127">Is Required</span></span> | <span data-ttu-id="097ba-128">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-129">**Ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-129">**Name**</span></span>       | <span data-ttu-id="097ba-130">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-130">Yes</span></span>         | <span data-ttu-id="097ba-131">Karşılık gelen yabancı anahtar kısıtlaması temel alınan veritabanında adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-132">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="097ba-133">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-134">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-135">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-135">Example</span></span>

<span data-ttu-id="097ba-136">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** kullanan öğesi bir **Referentialconstraint'teki** katılmak sütunları belirlemek için öğe **FK\_CustomerOrders**  yabancı anahtar kısıtlaması:</span><span class="sxs-lookup"><span data-stu-id="097ba-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a><span data-ttu-id="097ba-137">AssociationSet öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="097ba-138">**AssociationSet** depo şeması tanım dili (SSDL) öğesinde temel alınan veritabanında iki tablo arasında bir yabancı anahtar kısıtlaması temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="097ba-139">Yabancı anahtar kısıtlaması katılan tablo sütunları bir ilişkilendirme öğesinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="097ba-140">**İlişkilendirme** karşılık gelen öğe bir verilen **AssociationSet** öğesi belirtilen **ilişkilendirme** özniteliği **AssociationSet**  öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="097ba-141">SSDL ilişki Setleri CSDL ilişki setleri için AssociationSetMapping öğeyi göre eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="097ba-142">CSDL ilişkilendirme belirli bir CSDL ilişkilendirmesi için ayarlanmış, ancak karşılık gelen bir Referentialconstraint'teki öğesi tarafından tanımlanan **AssociationSetMapping** öğesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="097ba-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="097ba-143">Bu durumda, bir **AssociationSetMapping** öğe varsa, tanımladığı eşlemeleri tarafından geçersiz kılınır **Referentialconstraint'teki** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="097ba-144">**AssociationSet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-145">Belgeleri (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="097ba-146">Bitiş (sıfır veya iki)</span><span class="sxs-lookup"><span data-stu-id="097ba-146">End (zero or two)</span></span>
-   <span data-ttu-id="097ba-147">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-148">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-148">Applicable Attributes</span></span>

<span data-ttu-id="097ba-149">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="097ba-150">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-150">Attribute Name</span></span>  | <span data-ttu-id="097ba-151">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-151">Is Required</span></span> | <span data-ttu-id="097ba-152">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-153">**Ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-153">**Name**</span></span>        | <span data-ttu-id="097ba-154">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-154">Yes</span></span>         | <span data-ttu-id="097ba-155">Yabancı anahtar kısıtlaması ilişkiyi temsil kümesi adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="097ba-156">**İlişkilendirme**</span><span class="sxs-lookup"><span data-stu-id="097ba-156">**Association**</span></span> | <span data-ttu-id="097ba-157">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-157">Yes</span></span>         | <span data-ttu-id="097ba-158">Yabancı anahtar kısıtlaması katılan sütunlar tanımlayan ilişkilendirmenin adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-159">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="097ba-160">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-161">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-162">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-162">Example</span></span>

<span data-ttu-id="097ba-163">Aşağıdaki örnekte gösterildiği bir **AssociationSet** temsil eden öğe `FK_CustomerOrders` temel alınan veritabanında yabancı anahtar kısıtlaması:</span><span class="sxs-lookup"><span data-stu-id="097ba-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="097ba-164">CollectionType öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="097ba-165">**CollectionType** depo şeması tanım dili (SSDL) öğesinde, bir işlevin dönüş türü bir koleksiyon olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="097ba-166">**CollectionType** ReturnType öğesinin bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="097ba-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="097ba-167">Toplama türünü RowType alt öğesi kullanılarak belirtilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="097ba-168">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **CollectionType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="097ba-169">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-170">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-171">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-171">Example</span></span>

<span data-ttu-id="097ba-172">Aşağıdaki örnek, kullanan bir işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="097ba-173">CommandText öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="097ba-174">**CommandText** depo şeması tanım dili (SSDL) öğesinde veritabanına yürütülen bir SQL deyimi tanımlamanıza olanak tanıyan Function öğesi alt öğesi olan.</span><span class="sxs-lookup"><span data-stu-id="097ba-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="097ba-175">**CommandText** öğesi veritabanında bir saklı yordam benzer işlevler eklemenize olanak sağlar, ancak tanımladığınız **CommandText** depolama modelinde öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="097ba-176">**CommandText** öğesi alt öğeleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="097ba-177">Gövdesi **CommandText** öğe, temel alınan veritabanı için geçerli bir SQL deyimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="097ba-178">Hiçbir öznitelik geçerli olan **CommandText** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-179">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-179">Example</span></span>

<span data-ttu-id="097ba-180">Aşağıdaki örnekte gösterildiği bir **işlevi** bir alt öğe **CommandText** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="097ba-181">Kullanıma sunma **UpdateProductInOrder** işlev ObjectContext üzerinde bir yöntem olarak kavramsal modele içeri aktararak.</span><span class="sxs-lookup"><span data-stu-id="097ba-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="097ba-182">DefiningQuery öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="097ba-183">**DefiningQuery** depo şeması tanım dili (SSDL) öğesinde doğrudan temel alınan veritabanında bir SQL deyimi yürütme olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="097ba-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="097ba-184">**DefiningQuery** öğesi, bir veritabanı görünümü gibi yaygın olarak kullanılır, ancak görünüm veritabanı yerine depolama modelinde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="097ba-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="097ba-185">İçinde tanımlanan görünüm bir **DefiningQuery** öğesi, bir varlık türü kavramsal modelde EntitySetMapping öğenin üzerinden eşlenebilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="097ba-186">Bu eşlemeler salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="097ba-186">These mappings are read-only.</span></span>  

<span data-ttu-id="097ba-187">Bildirimi aşağıdaki SSDL sözdizimini gösterir bir **EntitySet** ardından **DefiningQuery** görünümü almak için kullanılan bir sorgu içeren öğe.</span><span class="sxs-lookup"><span data-stu-id="097ba-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

<span data-ttu-id="097ba-188">Saklı yordamlar, varlık Çerçevesi'nde görünümlerini okuma-yazma senaryolarını etkinleştirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="097ba-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="097ba-189">Veri alma ve saklı yordamlar tarafından işleme değişiklik için temel tablo bir veri kaynağı görünümü ya varlık SQL görünümü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="097ba-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="097ba-190">Kullanabileceğiniz **DefiningQuery** hedeflenecek Microsoft SQL Server Compact 3.5 öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="097ba-191">SQL Server Compact 3.5 saklı yordamları desteklemez ancak benzer işlevselliği olan uygulayabilirsiniz **DefiningQuery** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="097ba-192">Burada da yararlı olabilir başka bir programlama dili ve bu veri kaynağının kullanılan veri türleri arasında bir uyuşmazlık üstesinden gelmek için saklı yordamlar oluşturma yerdir.</span><span class="sxs-lookup"><span data-stu-id="097ba-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="097ba-193">Yazabileceğiniz küçük bir **DefiningQuery** , belirli bir parametre kümesi alır ve daha sonra farklı bir dizi parametrenin, örneğin, verilerin bir saklı yordam olan bir saklı yordam çağırır.</span><span class="sxs-lookup"><span data-stu-id="097ba-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="097ba-194">Bağımlı öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="097ba-195">**Bağımlı** depo şeması tanım dili (SSDL) öğesinde (başvurusal Kısıt olarak da bilinir) bir yabancı anahtar kısıtlamasını bağımlı sonuna tanımlayan Referentialconstraint'teki öğeye bir alt öğedir.</span><span class="sxs-lookup"><span data-stu-id="097ba-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="097ba-196">**Bağımlı** öğesi, bir birincil anahtar sütunu (veya sütun) başvuran bir tablodaki sütuna (veya sütun) belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="097ba-197">**PropertyRef** elementleri hangi sütunların başvurulur.</span><span class="sxs-lookup"><span data-stu-id="097ba-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="097ba-198">Asıl öğe içinde belirtilen sütun tarafından başvurulan birincil anahtar sütunları belirtir **bağımlı** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="097ba-199">**Bağımlı** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-200">PropertyRef (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="097ba-201">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-202">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-202">Applicable Attributes</span></span>

<span data-ttu-id="097ba-203">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **bağımlı** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="097ba-204">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-204">Attribute Name</span></span> | <span data-ttu-id="097ba-205">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-205">Is Required</span></span> | <span data-ttu-id="097ba-206">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-207">**Rol**</span><span class="sxs-lookup"><span data-stu-id="097ba-207">**Role**</span></span>       | <span data-ttu-id="097ba-208">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-208">Yes</span></span>         | <span data-ttu-id="097ba-209">Aynı değer olarak **rol** (kullanılıyorsa) karşılık gelen son öğe öznitelik; Aksi takdirde, tablonun adını içeren başvuru sütunu.</span><span class="sxs-lookup"><span data-stu-id="097ba-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-210">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **bağımlı** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="097ba-211">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="097ba-212">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-213">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-213">Example</span></span>

<span data-ttu-id="097ba-214">Aşağıdaki örnek, kullanan bir ilişkilendirme öğenin gösterir. bir **Referentialconstraint'teki** katılmak sütunları belirlemek için öğe **FK\_CustomerOrders** yabancı anahtar kısıtlama.</span><span class="sxs-lookup"><span data-stu-id="097ba-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="097ba-215">**Bağımlı** öğesi belirtir **CustomerID** sütununun **sipariş** kısıtlamasının bağımlı ucu olarak tablo.</span><span class="sxs-lookup"><span data-stu-id="097ba-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a><span data-ttu-id="097ba-216">Belge öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="097ba-217">**Belgeleri** depo şeması tanım dili (SSDL) öğesinde, bir üst öğe içinde tanımlanan bir nesneyle ilgili bilgileri sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="097ba-218">**Belgeleri** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-219">**Özet**: üst öğenin kısa bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="097ba-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="097ba-220">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="097ba-220">(zero or one element)</span></span>
-   <span data-ttu-id="097ba-221">**LongDescription**: üst öğenin kapsamlı bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="097ba-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="097ba-222">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="097ba-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-223">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-223">Applicable Attributes</span></span>

<span data-ttu-id="097ba-224">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **belgeleri** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="097ba-225">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="097ba-226">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-227">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-227">Example</span></span>

<span data-ttu-id="097ba-228">Aşağıdaki örnekte gösterildiği **belgeleri** EntityType öğesinin alt öğesi olarak öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a><span data-ttu-id="097ba-229">Bitiş öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-229">End Element (SSDL)</span></span>

<span data-ttu-id="097ba-230">**Son** depo şeması tanım dili (SSDL) öğesinde, temel alınan veritabanında tablo ve bir ucunda bulunan bir yabancı anahtar kısıtlaması satır sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="097ba-231">**Son** Association öğesinde veya AssociationSet bir öğenin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="097ba-232">Her durumda olası alt öğeler ve uygun öznitelikler farklıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="097ba-233">Bir ilişkilendirme öğesinin alt öğesi olarak bitiş öğesi</span><span class="sxs-lookup"><span data-stu-id="097ba-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="097ba-234">Bir **son** öğesi (alt öğesi olarak **ilişkilendirme** öğe) tablo ve sonunda bir yabancı anahtar kısıtlaması, satır sayısını belirtir **türü** ve **Çoğulluk** sırasıyla öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="097ba-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="097ba-235">Bir yabancı anahtar kısıtlaması ucunda SSDL ilişkilendirme bir parçası olarak tanımlanır; tam olarak iki ucu SSDL ilişki olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="097ba-236">Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-237">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="097ba-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="097ba-238">OnDelete (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="097ba-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="097ba-239">Ek açıklama öğelerinin (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="097ba-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="097ba-240">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-240">Applicable Attributes</span></span>

<span data-ttu-id="097ba-241">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="097ba-242">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-242">Attribute Name</span></span>   | <span data-ttu-id="097ba-243">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-243">Is Required</span></span> | <span data-ttu-id="097ba-244">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-245">**Türü**</span><span class="sxs-lookup"><span data-stu-id="097ba-245">**Type**</span></span>         | <span data-ttu-id="097ba-246">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-246">Yes</span></span>         | <span data-ttu-id="097ba-247">Yabancı anahtar kısıtlaması sonunda SSDL varlık kümesini tam adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="097ba-248">**Rol**</span><span class="sxs-lookup"><span data-stu-id="097ba-248">**Role**</span></span>         | <span data-ttu-id="097ba-249">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-249">No</span></span>          | <span data-ttu-id="097ba-250">Değerini **rol** (kullanılıyorsa) karşılık gelen Referentialconstraint'teki öğe sorumlusu veya bağımlı öğesindeki özniteliği.</span><span class="sxs-lookup"><span data-stu-id="097ba-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="097ba-251">**Çokluk**</span><span class="sxs-lookup"><span data-stu-id="097ba-251">**Multiplicity**</span></span> | <span data-ttu-id="097ba-252">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-252">Yes</span></span>         | <span data-ttu-id="097ba-253">**1**, **0..1**, veya **\*** yabancı anahtar kısıtlaması sonunda olabilir satır sayısına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="097ba-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="097ba-254">**1** tam olarak bir satır var. yabancı anahtar kısıtlaması sonunda gösterir.</span><span class="sxs-lookup"><span data-stu-id="097ba-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="097ba-255">**0..1** belirten sıfır veya bir satır yabancı anahtar kısıtlaması sonunda yok.</span><span class="sxs-lookup"><span data-stu-id="097ba-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="097ba-256">**\*** sıfır, bir veya daha fazla satır yabancı anahtar kısıtlaması sonunda bulunduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="097ba-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-257">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="097ba-258">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="097ba-259">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="097ba-260">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-260">Example</span></span>

<span data-ttu-id="097ba-261">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **FK\_CustomerOrders** yabancı anahtar kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="097ba-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="097ba-262">**Çoğulluk** her belirtilen değerleri **son** öğe belirtmek, bu sayıda satır **siparişler** tabloda bir satır ile ilişkili olabilir **müşteriler**  tablosu, ancak yalnızca tek bir satırda **müşteriler** tabloda bir satır ile ilişkili olabilir **siparişler** tablo.</span><span class="sxs-lookup"><span data-stu-id="097ba-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="097ba-263">Ayrıca, **OnDelete** öğesi gösteren tüm satırları **siparişler** belirli bir satır başvurusu tablo **müşteriler** tablo silinecek, satır **müşteriler** tablo silinir.</span><span class="sxs-lookup"><span data-stu-id="097ba-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="097ba-264">AssociationSet öğesinin bir alt öğesi olarak bitiş öğesi</span><span class="sxs-lookup"><span data-stu-id="097ba-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="097ba-265">**Son** öğesi (alt öğesi olarak **AssociationSet** öğesi) temel alınan veritabanında bir tablo bir ucunda bulunan bir yabancı anahtar kısıtlaması belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="097ba-266">Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-267">Belgeleri (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="097ba-268">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="097ba-269">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-269">Applicable Attributes</span></span>

<span data-ttu-id="097ba-270">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **AssociationSet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="097ba-271">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-271">Attribute Name</span></span> | <span data-ttu-id="097ba-272">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-272">Is Required</span></span> | <span data-ttu-id="097ba-273">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="097ba-274">**EntitySet**</span></span>  | <span data-ttu-id="097ba-275">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-275">Yes</span></span>         | <span data-ttu-id="097ba-276">Yabancı anahtar kısıtlaması sonunda SSDL varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="097ba-277">**Rol**</span><span class="sxs-lookup"><span data-stu-id="097ba-277">**Role**</span></span>       | <span data-ttu-id="097ba-278">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-278">No</span></span>          | <span data-ttu-id="097ba-279">Değerin aşağıdakilerden biri **rol** birinde belirtilen öznitelikler **son** karşılık gelen Association öğesinde öğesidir.</span><span class="sxs-lookup"><span data-stu-id="097ba-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-280">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="097ba-281">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="097ba-282">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="097ba-283">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-283">Example</span></span>

<span data-ttu-id="097ba-284">Aşağıdaki örnekte gösterildiği bir **EntityContainer** öğesi ile bir **AssociationSet** iki öğe **son** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="097ba-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="097ba-285">EntityContainer öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="097ba-286">Bir **EntityContainer** depo şeması tanım dili (SSDL) öğesinde bir Entity Framework uygulamasında temel alınan veri kaynağının yapısını açıklar: SSDL varlık kümeleri (EntitySet öğesinde tanımlanan) temsil eden tablolarında bir veritabanı, bir tablodaki satırlar SSDL varlık türleri (EntityType öğesinde tanımlanan) temsil eder ve ilişki setleri (AssociationSet öğesinde tanımlı) bir veritabanındaki yabancı anahtar kısıtlamaları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="097ba-287">Depolama modelinin varlık kapsayıcısı bir kavramsal model varlık kapsayıcısı Entitycontainermapping'indeki öğesi üzerinden eşlenir.</span><span class="sxs-lookup"><span data-stu-id="097ba-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="097ba-288">Bir **EntityContainer** öğesi sıfır veya bir belge öğeleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="097ba-289">Varsa bir **belgeleri** öğe varsa, bunu diğer tüm alt öğelerden önce gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="097ba-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="097ba-290">Bir **EntityContainer** öğesi (listelenen sırayla) sıfır veya daha fazla şu alt öğelerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="097ba-291">EntitySet</span></span>
-   <span data-ttu-id="097ba-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="097ba-292">AssociationSet</span></span>
-   <span data-ttu-id="097ba-293">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="097ba-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-294">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-294">Applicable Attributes</span></span>

<span data-ttu-id="097ba-295">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntityContainer** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="097ba-296">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-296">Attribute Name</span></span> | <span data-ttu-id="097ba-297">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-297">Is Required</span></span> | <span data-ttu-id="097ba-298">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="097ba-299">**Ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-299">**Name**</span></span>       | <span data-ttu-id="097ba-300">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-300">Yes</span></span>         | <span data-ttu-id="097ba-301">Varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-301">The name of the entity container.</span></span> <span data-ttu-id="097ba-302">Bu ad, nokta (..) içeremez.</span><span class="sxs-lookup"><span data-stu-id="097ba-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-303">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityContainer** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="097ba-304">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-305">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-306">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-306">Example</span></span>

<span data-ttu-id="097ba-307">Aşağıdaki örnekte gösterildiği bir **EntityContainer** iki varlık kümeleri ve bir ilişki kümesi tanımlayan öğe.</span><span class="sxs-lookup"><span data-stu-id="097ba-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="097ba-308">Varlık türü ve ilişkisi tür adları kavramsal model ad alanı adı tarafından yetkili olan unutmayın.</span><span class="sxs-lookup"><span data-stu-id="097ba-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a><span data-ttu-id="097ba-309">Entityset'in öğe (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="097ba-310">Bir **EntitySet** depo şeması tanım dili (SSDL) öğesinde bir tablo veya Görünüm temel alınan veritabanında temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="097ba-311">SSDL EntityType öğesinin tablo veya Görünüm bir sırayı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="097ba-312">**EntityType** özniteliği bir **EntitySet** öğesi bir SSDL varlık kümesindeki satırları gösteren belirli SSDL varlık türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="097ba-313">CSDL varlık kümesi ve bir SSDL varlık kümesi arasındaki eşleme bir EntitySetMapping öğesinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="097ba-314">**EntitySet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-315">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="097ba-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="097ba-316">DefiningQuery (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="097ba-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="097ba-317">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="097ba-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-318">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-318">Applicable Attributes</span></span>

<span data-ttu-id="097ba-319">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntitySet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="097ba-320">Bazı öznitelikler (burada listelenmeyen) ile nitelenebilir **depolamak** diğer adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="097ba-321">Bu öznitelikler bir modeli güncelleştirme güncelleştirme modeli Sihirbazı tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="097ba-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="097ba-322">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-322">Attribute Name</span></span> | <span data-ttu-id="097ba-323">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-323">Is Required</span></span> | <span data-ttu-id="097ba-324">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-325">**Ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-325">**Name**</span></span>       | <span data-ttu-id="097ba-326">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-326">Yes</span></span>         | <span data-ttu-id="097ba-327">Varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="097ba-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="097ba-328">**EntityType**</span></span> | <span data-ttu-id="097ba-329">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-329">Yes</span></span>         | <span data-ttu-id="097ba-330">Varlık türü için varlık kümesi tam olarak nitelenmiş adını örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="097ba-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="097ba-331">**Şema**</span><span class="sxs-lookup"><span data-stu-id="097ba-331">**Schema**</span></span>     | <span data-ttu-id="097ba-332">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-332">No</span></span>          | <span data-ttu-id="097ba-333">Veritabanı şeması.</span><span class="sxs-lookup"><span data-stu-id="097ba-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="097ba-334">**Tablo**</span><span class="sxs-lookup"><span data-stu-id="097ba-334">**Table**</span></span>      | <span data-ttu-id="097ba-335">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-335">No</span></span>          | <span data-ttu-id="097ba-336">Veritabanı tablosu.</span><span class="sxs-lookup"><span data-stu-id="097ba-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="097ba-337">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntitySet** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="097ba-338">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-339">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-340">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-340">Example</span></span>

<span data-ttu-id="097ba-341">Aşağıdaki örnekte gösterildiği bir **EntityContainer** sahip iki öğe **EntitySet** öğeleri ve bir **AssociationSet** öğesi:</span><span class="sxs-lookup"><span data-stu-id="097ba-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="097ba-342">EntityType öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="097ba-343">Bir **EntityType** depo şeması tanım dili (SSDL) öğesinde bir tablo veya temel alınan veritabanına görünümünü bir satır temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="097ba-344">Bir EntitySet öğedeki SSDL tablo veya Görünüm satırları oluşabilen temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="097ba-345">**EntityType** özniteliği bir **EntitySet** öğesi bir SSDL varlık kümesindeki satırları gösteren belirli SSDL varlık türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="097ba-346">CSDL varlık türü ile bir SSDL varlık türü arasında bir eşleme bir EntityTypeMapping öğesinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="097ba-347">**EntityType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-348">Belgeleri (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="097ba-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="097ba-349">Anahtar (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="097ba-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="097ba-350">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="097ba-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-351">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-351">Applicable Attributes</span></span>

<span data-ttu-id="097ba-352">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntityType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="097ba-353">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-353">Attribute Name</span></span> | <span data-ttu-id="097ba-354">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-354">Is Required</span></span> | <span data-ttu-id="097ba-355">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-356">**Ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-356">**Name**</span></span>       | <span data-ttu-id="097ba-357">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-357">Yes</span></span>         | <span data-ttu-id="097ba-358">Varlık türü adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-358">The name of the entity type.</span></span> <span data-ttu-id="097ba-359">Bu değer genellikle varlık türü bir satırı temsil eden tablosunun adı ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="097ba-360">Bu değer, herhangi bir nokta (.) içerebilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-361">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="097ba-362">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-363">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-364">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-364">Example</span></span>

<span data-ttu-id="097ba-365">Aşağıdaki örnekte gösterildiği bir **EntityType** sahip iki özellik öğe:</span><span class="sxs-lookup"><span data-stu-id="097ba-365">The following example shows an **EntityType** element with two properties:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a><span data-ttu-id="097ba-366">Function öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-366">Function Element (SSDL)</span></span>

<span data-ttu-id="097ba-367">**İşlevi** depo şeması tanım dili (SSDL) öğesinde, temel alınan veritabanında var olan bir saklı yordam belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="097ba-368">**İşlevi** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-369">Belgeleri (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="097ba-370">Parametre (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="097ba-371">CommandText (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="097ba-372">ReturnType (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="097ba-373">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="097ba-374">Dönüş türü için bir işlev ile birlikte belirtilmelidir **ReturnType** öğesi veya **ReturnType** özniteliği (aşağıya bakın), ancak ikisine birden değil.</span><span class="sxs-lookup"><span data-stu-id="097ba-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="097ba-375">Depolama modelde belirtilen saklı yordamlar, bir uygulamanın kavramsal modele içeri aktarılabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="097ba-376">Daha fazla bilgi için [depolanan yordamlarla sorgulama](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="097ba-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="097ba-377">**İşlevi** öğe ayrıca depolama modelinde özel işlevleri tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-378">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-378">Applicable Attributes</span></span>

<span data-ttu-id="097ba-379">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **işlevi** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="097ba-380">Bazı öznitelikler (burada listelenmeyen) ile nitelenebilir **depolamak** diğer adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="097ba-381">Bu öznitelikler bir modeli güncelleştirme güncelleştirme modeli Sihirbazı tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="097ba-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="097ba-382">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-382">Attribute Name</span></span>             | <span data-ttu-id="097ba-383">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-383">Is Required</span></span> | <span data-ttu-id="097ba-384">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-385">**Ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-385">**Name**</span></span>                   | <span data-ttu-id="097ba-386">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-386">Yes</span></span>         | <span data-ttu-id="097ba-387">Saklı yordamın adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="097ba-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="097ba-388">**ReturnType**</span></span>             | <span data-ttu-id="097ba-389">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-389">No</span></span>          | <span data-ttu-id="097ba-390">Saklı yordamın dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="097ba-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="097ba-391">**Toplama**</span><span class="sxs-lookup"><span data-stu-id="097ba-391">**Aggregate**</span></span>              | <span data-ttu-id="097ba-392">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-392">No</span></span>          | <span data-ttu-id="097ba-393">**True** saklı yordamı bir toplam değer; döndürür, aksi takdirde **False**.</span><span class="sxs-lookup"><span data-stu-id="097ba-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="097ba-394">**Yerleşik**</span><span class="sxs-lookup"><span data-stu-id="097ba-394">**BuiltIn**</span></span>                | <span data-ttu-id="097ba-395">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-395">No</span></span>          | <span data-ttu-id="097ba-396">**Doğru** yerleşik bir işlev ise<sup>1</sup> işlev; Aksi takdirde **False**.</span><span class="sxs-lookup"><span data-stu-id="097ba-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="097ba-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="097ba-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="097ba-398">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-398">No</span></span>          | <span data-ttu-id="097ba-399">Saklı yordamın adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="097ba-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="097ba-400">**NiladicFunction**</span></span>        | <span data-ttu-id="097ba-401">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-401">No</span></span>          | <span data-ttu-id="097ba-402">**Doğru** işlev bir parametresiz ise<sup>2</sup> işlev; **False** Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="097ba-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="097ba-403">**Iscomposable**</span><span class="sxs-lookup"><span data-stu-id="097ba-403">**IsComposable**</span></span>           | <span data-ttu-id="097ba-404">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-404">No</span></span>          | <span data-ttu-id="097ba-405">**Doğru** birleştirilebilir bir işlev ise<sup>3</sup> işlev; **False** Aksi takdirde.</span><span class="sxs-lookup"><span data-stu-id="097ba-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="097ba-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="097ba-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="097ba-407">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-407">No</span></span>          | <span data-ttu-id="097ba-408">İşlev aşırı yüklemelerinin çözümlemek için kullanılan türü anlamları tanımlayan sabit listesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="097ba-409">Numaralandırma, işlev tanımı başına sağlayıcısı bildirimi içinde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="097ba-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="097ba-410">Varsayılan değer **Allowımplicitconversion**.</span><span class="sxs-lookup"><span data-stu-id="097ba-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="097ba-411">**Şema**</span><span class="sxs-lookup"><span data-stu-id="097ba-411">**Schema**</span></span>                 | <span data-ttu-id="097ba-412">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-412">No</span></span>          | <span data-ttu-id="097ba-413">Saklı yordam tanımlandığı şemasının adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="097ba-414"><sup>1</sup> veritabanında tanımlı bir işlev yerleşik bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="097ba-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="097ba-415">CommandText öğesi (SSDL) depolama modelde tanımlı işlevler hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="097ba-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="097ba-416"><sup>2</sup> parametresiz işlevi, hiçbir parametre kabul eden ve çağrıldığında, parantez gerektirmeyen bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="097ba-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="097ba-417"><sup>3</sup> iki işlev, bir işlevin çıktısı diğer işlevi için giriş olarak birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="097ba-418">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **işlevi** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="097ba-419">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-420">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-421">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-421">Example</span></span>

<span data-ttu-id="097ba-422">Aşağıdaki örnekte gösterildiği bir **işlevi** karşılık gelen öğe **UpdateOrderQuantity** saklı yordamı.</span><span class="sxs-lookup"><span data-stu-id="097ba-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="097ba-423">Saklı yordam, iki parametre kabul eder ve bir değer döndürmez.</span><span class="sxs-lookup"><span data-stu-id="097ba-423">The stored procedure accepts two parameters and does not return a value.</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a><span data-ttu-id="097ba-424">Anahtar öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-424">Key Element (SSDL)</span></span>

<span data-ttu-id="097ba-425">**Anahtar** depo şeması tanım dili (SSDL) öğesinde temel alınan veritabanında bir tablonun birincil anahtarı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="097ba-426">**Anahtar** bir tabloda bir satırı temsil eden EntityType öğenin bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="097ba-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="097ba-427">Birincil anahtarı tanımlanan **anahtarı** öğesi üzerinde tanımlanan bir veya daha fazla özellik öğelerinin başvuru tarafından **EntityType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="097ba-428">**Anahtar** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-429">PropertyRef (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="097ba-430">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="097ba-430">Annotation elements</span></span>

<span data-ttu-id="097ba-431">Hiçbir öznitelik geçerli olan **anahtar** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-432">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-432">Example</span></span>

<span data-ttu-id="097ba-433">Aşağıdaki örnekte gösterildiği bir **EntityType** öğesi bir anahtarla bir özelliğine başvuruyor:</span><span class="sxs-lookup"><span data-stu-id="097ba-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="097ba-434">OnDelete öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="097ba-435">**OnDelete** depo şeması tanım dili (SSDL) öğesinde bir yabancı anahtar kısıtlaması katıldığı bir satır silindiğinde, veritabanı davranışı yansıtır.</span><span class="sxs-lookup"><span data-stu-id="097ba-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="097ba-436">Eylem ayarlanırsa **Cascade**, reddederseniz, silinen satır başvurusu satırlar da silinir.</span><span class="sxs-lookup"><span data-stu-id="097ba-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="097ba-437">Eylem ayarlanırsa **hiçbiri**, sonra da, silinen satır başvurusu satırlar da silinmedi.</span><span class="sxs-lookup"><span data-stu-id="097ba-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="097ba-438">Bir **OnDelete** bir bitiş öğesi alt öğesi bir öğedir.</span><span class="sxs-lookup"><span data-stu-id="097ba-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="097ba-439">Bir **OnDelete** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-440">Belgeleri (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="097ba-441">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-442">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-442">Applicable Attributes</span></span>

<span data-ttu-id="097ba-443">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **OnDelete** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="097ba-444">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-444">Attribute Name</span></span> | <span data-ttu-id="097ba-445">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-445">Is Required</span></span> | <span data-ttu-id="097ba-446">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-447">**Eylem**</span><span class="sxs-lookup"><span data-stu-id="097ba-447">**Action**</span></span>     | <span data-ttu-id="097ba-448">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-448">Yes</span></span>         | <span data-ttu-id="097ba-449">**Art arda** veya **hiçbiri**.</span><span class="sxs-lookup"><span data-stu-id="097ba-449">**Cascade** or **None**.</span></span> <span data-ttu-id="097ba-450">(Değer **kısıtlı** geçerlidir, ancak aynı davranışı sahiptir **hiçbiri**.)</span><span class="sxs-lookup"><span data-stu-id="097ba-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-451">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **OnDelete** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="097ba-452">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-453">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-454">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-454">Example</span></span>

<span data-ttu-id="097ba-455">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **FK\_CustomerOrders** yabancı anahtar kısıtlaması.</span><span class="sxs-lookup"><span data-stu-id="097ba-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="097ba-456">**OnDelete** öğesi gösteren tüm satırları **siparişler** belirli bir satır başvurusu tablo **müşteriler** varsa Tablo silinecek satırda**Müşteriler** tablo silinir.</span><span class="sxs-lookup"><span data-stu-id="097ba-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a><span data-ttu-id="097ba-457">Parametre öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="097ba-458">**Parametre** depo şeması tanım dili (SSDL) öğesinde veritabanında bir saklı yordam parametrelerini belirten Function öğesinin alt öğesi olan.</span><span class="sxs-lookup"><span data-stu-id="097ba-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="097ba-459">**Parametre** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-460">Belgeleri (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="097ba-461">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-462">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-462">Applicable Attributes</span></span>

<span data-ttu-id="097ba-463">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="097ba-464">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-464">Attribute Name</span></span> | <span data-ttu-id="097ba-465">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-465">Is Required</span></span> | <span data-ttu-id="097ba-466">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-467">**Ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-467">**Name**</span></span>       | <span data-ttu-id="097ba-468">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-468">Yes</span></span>         | <span data-ttu-id="097ba-469">Parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="097ba-470">**Türü**</span><span class="sxs-lookup"><span data-stu-id="097ba-470">**Type**</span></span>       | <span data-ttu-id="097ba-471">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-471">Yes</span></span>         | <span data-ttu-id="097ba-472">Parametre türü.</span><span class="sxs-lookup"><span data-stu-id="097ba-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="097ba-473">**Modu**</span><span class="sxs-lookup"><span data-stu-id="097ba-473">**Mode**</span></span>       | <span data-ttu-id="097ba-474">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-474">No</span></span>          | <span data-ttu-id="097ba-475">**İçinde**, **kullanıma**, veya **Inout** parametresi bir giriş, çıkış ve giriş/çıkış parametresi olmasına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="097ba-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="097ba-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="097ba-476">**MaxLength**</span></span>  | <span data-ttu-id="097ba-477">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-477">No</span></span>          | <span data-ttu-id="097ba-478">Parametresinin en büyük uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="097ba-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="097ba-479">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="097ba-479">**Precision**</span></span>  | <span data-ttu-id="097ba-480">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-480">No</span></span>          | <span data-ttu-id="097ba-481">Parametre duyarlılığı.</span><span class="sxs-lookup"><span data-stu-id="097ba-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="097ba-482">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="097ba-482">**Scale**</span></span>      | <span data-ttu-id="097ba-483">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-483">No</span></span>          | <span data-ttu-id="097ba-484">Parametre ölçeği.</span><span class="sxs-lookup"><span data-stu-id="097ba-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="097ba-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="097ba-485">**SRID**</span></span>       | <span data-ttu-id="097ba-486">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-486">No</span></span>          | <span data-ttu-id="097ba-487">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="097ba-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="097ba-488">Uzamsal tür parametreleri yalnızca için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="097ba-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="097ba-489">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="097ba-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-490">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **parametre** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="097ba-491">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-492">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-493">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-493">Example</span></span>

<span data-ttu-id="097ba-494">Aşağıdaki örnekte gösterildiği bir **işlevi** sahip iki öğe **parametre** giriş parametrelerini belirten öğeleri:</span><span class="sxs-lookup"><span data-stu-id="097ba-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a><span data-ttu-id="097ba-495">Asıl öğe (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="097ba-496">**Asıl** depo şeması tanım dili (SSDL) öğesinde (başvurusal Kısıt olarak da bilinir) bir yabancı anahtar kısıtlaması birincil ucu tanımlayan Referentialconstraint'teki öğe için bir alt öğedir.</span><span class="sxs-lookup"><span data-stu-id="097ba-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="097ba-497">**Asıl** başka bir sütuna (veya sütun) Başvurulan tablodaki öğesi belirtir birincil anahtar sütunu (veya sütun).</span><span class="sxs-lookup"><span data-stu-id="097ba-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="097ba-498">**PropertyRef** elementleri hangi sütunların başvurulur.</span><span class="sxs-lookup"><span data-stu-id="097ba-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="097ba-499">Bağımlı öğenin belirtilen birincil anahtar sütunlarını başvuran sütunları belirten **asıl** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="097ba-500">**Asıl** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="097ba-501">PropertyRef (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="097ba-502">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-503">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-503">Applicable Attributes</span></span>

<span data-ttu-id="097ba-504">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **asıl** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="097ba-505">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-505">Attribute Name</span></span> | <span data-ttu-id="097ba-506">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-506">Is Required</span></span> | <span data-ttu-id="097ba-507">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-508">**Rol**</span><span class="sxs-lookup"><span data-stu-id="097ba-508">**Role**</span></span>       | <span data-ttu-id="097ba-509">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-509">Yes</span></span>         | <span data-ttu-id="097ba-510">Aynı değer olarak **rol** (kullanılıyorsa) karşılık gelen son öğe öznitelik; Aksi takdirde, tablonun adını içeren başvurulan sütun.</span><span class="sxs-lookup"><span data-stu-id="097ba-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-511">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **asıl** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="097ba-512">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="097ba-513">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-514">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-514">Example</span></span>

<span data-ttu-id="097ba-515">Aşağıdaki örnek, kullanan bir ilişkilendirme öğenin gösterir. bir **Referentialconstraint'teki** katılmak sütunları belirlemek için öğe **FK\_CustomerOrders** yabancı anahtar kısıtlama.</span><span class="sxs-lookup"><span data-stu-id="097ba-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="097ba-516">**Asıl** öğesi belirtir **CustomerID** sütununun **müşteri** kısıtlamasının birincil ucu olarak tablo.</span><span class="sxs-lookup"><span data-stu-id="097ba-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a><span data-ttu-id="097ba-517">Özellik öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-517">Property Element (SSDL)</span></span>

<span data-ttu-id="097ba-518">**Özelliği** depo şeması tanım dili (SSDL) öğesinde temel alınan veritabanında bir tablodaki bir sütunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="097ba-519">**Özellik** tablodaki satırları temsil eden EntityType öğelerinin alt öğeleridir.</span><span class="sxs-lookup"><span data-stu-id="097ba-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="097ba-520">Her **özelliği** üzerinde tanımlanan öğe bir **EntityType** öğesi bir sütunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="097ba-521">A **özelliği** öğenin tüm alt öğeleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-522">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-522">Applicable Attributes</span></span>

<span data-ttu-id="097ba-523">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="097ba-524">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-524">Attribute Name</span></span>            | <span data-ttu-id="097ba-525">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-525">Is Required</span></span> | <span data-ttu-id="097ba-526">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-527">**Ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-527">**Name**</span></span>                  | <span data-ttu-id="097ba-528">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-528">Yes</span></span>         | <span data-ttu-id="097ba-529">Karşılık gelen sütunun adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="097ba-530">**Türü**</span><span class="sxs-lookup"><span data-stu-id="097ba-530">**Type**</span></span>                  | <span data-ttu-id="097ba-531">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-531">Yes</span></span>         | <span data-ttu-id="097ba-532">Karşılık gelen sütun türü.</span><span class="sxs-lookup"><span data-stu-id="097ba-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="097ba-533">**Boş değer atanabilir**</span><span class="sxs-lookup"><span data-stu-id="097ba-533">**Nullable**</span></span>              | <span data-ttu-id="097ba-534">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-534">No</span></span>          | <span data-ttu-id="097ba-535">**Doğru** (varsayılan değer) veya **False** karşılık gelen sütun null değere sahip olup olmadığına bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="097ba-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="097ba-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="097ba-536">**DefaultValue**</span></span>          | <span data-ttu-id="097ba-537">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-537">No</span></span>          | <span data-ttu-id="097ba-538">Karşılık gelen sütun varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="097ba-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="097ba-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="097ba-539">**MaxLength**</span></span>             | <span data-ttu-id="097ba-540">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-540">No</span></span>          | <span data-ttu-id="097ba-541">Karşılık gelen sütunun maksimum uzunluğu.</span><span class="sxs-lookup"><span data-stu-id="097ba-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="097ba-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="097ba-542">**FixedLength**</span></span>           | <span data-ttu-id="097ba-543">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-543">No</span></span>          | <span data-ttu-id="097ba-544">**Doğru** veya **False** bağlı olup olmadığını karşılık gelen sütun değeri sabit uzunlukta bir dize olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="097ba-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="097ba-545">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="097ba-545">**Precision**</span></span>             | <span data-ttu-id="097ba-546">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-546">No</span></span>          | <span data-ttu-id="097ba-547">Karşılık gelen sütunun duyarlığını.</span><span class="sxs-lookup"><span data-stu-id="097ba-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="097ba-548">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="097ba-548">**Scale**</span></span>                 | <span data-ttu-id="097ba-549">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-549">No</span></span>          | <span data-ttu-id="097ba-550">Karşılık gelen sütunun ölçek.</span><span class="sxs-lookup"><span data-stu-id="097ba-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="097ba-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="097ba-551">**Unicode**</span></span>               | <span data-ttu-id="097ba-552">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-552">No</span></span>          | <span data-ttu-id="097ba-553">**Doğru** veya **False** bağlı olup olmadığını karşılık gelen sütun değeri bir Unicode dize olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="097ba-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="097ba-554">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="097ba-554">**Collation**</span></span>             | <span data-ttu-id="097ba-555">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-555">No</span></span>          | <span data-ttu-id="097ba-556">Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="097ba-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="097ba-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="097ba-557">**SRID**</span></span>                  | <span data-ttu-id="097ba-558">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-558">No</span></span>          | <span data-ttu-id="097ba-559">Sistem uzamsal başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="097ba-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="097ba-560">Yalnızca uzamsal tür özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="097ba-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="097ba-561">Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="097ba-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="097ba-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="097ba-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="097ba-563">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-563">No</span></span>          | <span data-ttu-id="097ba-564">**Hiçbiri**, **kimlik** (karşılık gelen sütun değeri veritabanında oluşturulan bir kimlik ise), veya **hesaplanan** (karşılık gelen sütun değeri veritabanında hesaplanan varsa).</span><span class="sxs-lookup"><span data-stu-id="097ba-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="097ba-565">Değil RowType özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="097ba-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-566">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **özelliği** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="097ba-567">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-568">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-569">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-569">Example</span></span>

<span data-ttu-id="097ba-570">Aşağıdaki örnekte gösterildiği bir **EntityType** iki alt öğe **özelliği** öğeleri:</span><span class="sxs-lookup"><span data-stu-id="097ba-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="097ba-571">PropertyRef öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="097ba-572">**PropertyRef** depo şeması tanım dili (SSDL) öğesinde başvuran bir EntityType öğe üzerinde özellik aşağıdaki rollerden biri gerçekleştirecek belirtmek için tanımlanmış bir özellik:</span><span class="sxs-lookup"><span data-stu-id="097ba-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="097ba-573">Tablonun birincil anahtarı bir parçası olarak, **EntityType** temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="097ba-574">Bir veya daha fazla **PropertyRef** öğeleri, bir birincil anahtar tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="097ba-575">Anahtar öğesi daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="097ba-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="097ba-576">Başvuru kısıtlamasını bağımlı veya asıl sonuna olabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="097ba-577">Referentialconstraint'teki daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="097ba-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="097ba-578">**PropertyRef** öğesi şu alt öğelerden yalnızca olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="097ba-579">Belgeleri (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="097ba-580">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="097ba-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-581">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-581">Applicable Attributes</span></span>

<span data-ttu-id="097ba-582">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **PropertyRef** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="097ba-583">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-583">Attribute Name</span></span> | <span data-ttu-id="097ba-584">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-584">Is Required</span></span> | <span data-ttu-id="097ba-585">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="097ba-586">**Ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-586">**Name**</span></span>       | <span data-ttu-id="097ba-587">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-587">Yes</span></span>         | <span data-ttu-id="097ba-588">Başvurulan özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="097ba-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="097ba-589">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **PropertyRef** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="097ba-590">Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="097ba-591">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-592">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-592">Example</span></span>

<span data-ttu-id="097ba-593">Aşağıdaki örnekte gösterildiği bir **PropertyRef** birincil anahtar tanımlı bir özellik başvurarak tanımlamak için kullanılan öğe bir **EntityType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="097ba-594">Referentialconstraint'teki öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="097ba-595">**Referentialconstraint'teki** depo şeması tanım dili (SSDL) öğesinde (bir başvuru bütünlüğü kısıtlaması olarak da bilinir) bir yabancı anahtar kısıtlaması temel alınan veritabanında temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="097ba-596">Kısıtlamasının birincil ve bağımlı uçları sırasıyla asıl ve bağımlı alt öğeler tarafından belirtilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="097ba-597">Birincil ve bağımlı biter katılan sütunlar PropertyRef öğeleri ile başvurulur.</span><span class="sxs-lookup"><span data-stu-id="097ba-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="097ba-598">**Referentialconstraint'teki** Association öğesinde bir isteğe bağlı alt öğesi bir öğedir.</span><span class="sxs-lookup"><span data-stu-id="097ba-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="097ba-599">Varsa bir **Referentialconstraint'teki** öğesi belirtilen yabancı anahtar kısıtlaması eşlemek için kullanılmaz **ilişkilendirme** öğe, öğe kullanılan, bunu yapmak için bir AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="097ba-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="097ba-600">**Referentialconstraint'teki** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="097ba-601">Belgeleri (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="097ba-602">Asıl (tam olarak bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="097ba-603">Bağımlı (tam olarak bir)</span><span class="sxs-lookup"><span data-stu-id="097ba-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="097ba-604">Ek açıklama öğelerinin (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-605">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-605">Applicable Attributes</span></span>

<span data-ttu-id="097ba-606">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **Referentialconstraint'teki** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="097ba-607">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-608">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-609">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-609">Example</span></span>

<span data-ttu-id="097ba-610">Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** kullanan öğesi bir **Referentialconstraint'teki** katılmak sütunları belirlemek için öğe **FK\_CustomerOrders**  yabancı anahtar kısıtlaması:</span><span class="sxs-lookup"><span data-stu-id="097ba-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a><span data-ttu-id="097ba-611">ReturnType öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="097ba-612">**ReturnType** depo şeması tanım dili (SSDL) öğesinde tanımlanan bir işlev için dönüş türü belirten bir **işlevi** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="097ba-613">Bir işlevin dönüş türü de belirtilebilir bir **ReturnType** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="097ba-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="097ba-614">Bir işlevin dönüş türü belirtilmiş **türü** özniteliği veya **ReturnType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="097ba-615">**ReturnType** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="097ba-616">CollectionType (bir tane)</span><span class="sxs-lookup"><span data-stu-id="097ba-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="097ba-617">Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReturnType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="097ba-618">Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="097ba-619">İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-620">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-620">Example</span></span>

<span data-ttu-id="097ba-621">Aşağıdaki örnekte bir **işlevi** satırları koleksiyonunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="097ba-621">The following example uses a **Function** that returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="097ba-622">RowType öğesinin (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="097ba-623">A **RowType** depo şeması tanım dili (SSDL) öğesinde bir dönüş adlandırılmamış bir yapı tanımlar Mağazası'nda tanımlanan bir işlev türü.</span><span class="sxs-lookup"><span data-stu-id="097ba-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="097ba-624">A **RowType** öğesi alt öğesi olan **CollectionType** öğesi:</span><span class="sxs-lookup"><span data-stu-id="097ba-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="097ba-625">A **RowType** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="097ba-626">Özellik (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="097ba-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="097ba-627">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-627">Example</span></span>

<span data-ttu-id="097ba-628">Aşağıdaki örnek, kullanır depo işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğe (belirtilmiş **RowType** öğesi).</span><span class="sxs-lookup"><span data-stu-id="097ba-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a><span data-ttu-id="097ba-629">Şema öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="097ba-630">**Şema** depo şeması tanım dili (SSDL) öğesinde bir depolama model tanımı kök öğesidir.</span><span class="sxs-lookup"><span data-stu-id="097ba-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="097ba-631">Bu nesneleri, işlevleri ve bir depolama modeli olun kapsayıcılar için tanımları içerir.</span><span class="sxs-lookup"><span data-stu-id="097ba-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="097ba-632">**Şema** öğesi sıfır veya daha fazla şu alt öğelerden birini içerebilir:</span><span class="sxs-lookup"><span data-stu-id="097ba-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="097ba-633">İlişkilendirme</span><span class="sxs-lookup"><span data-stu-id="097ba-633">Association</span></span>
-   <span data-ttu-id="097ba-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="097ba-634">EntityType</span></span>
-   <span data-ttu-id="097ba-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="097ba-635">EntityContainer</span></span>
-   <span data-ttu-id="097ba-636">İşlev</span><span class="sxs-lookup"><span data-stu-id="097ba-636">Function</span></span>

<span data-ttu-id="097ba-637">**Şema** öğesini kullanan **Namespace** depolama modelinde varlık türü ve ilişki nesnesi için ad alanını tanımlayan öznitelik.</span><span class="sxs-lookup"><span data-stu-id="097ba-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="097ba-638">Ad alanı içinde hiçbir iki nesnenin aynı ada sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="097ba-639">Bir depolama model ad alanı XML ad alanından farklıdır **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="097ba-640">Bir depolama model ad alanı (tarafından tanımlandığı gibi **Namespace** özniteliği) varlık türleri ve ilişki türleri için bir mantıksal kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="097ba-641">XML ad alanı (tarafından belirtilen **xmlns** özniteliği), bir **şema** öğedir alt öğeleri ve öznitelikleri için varsayılan ad alanı **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="097ba-642">XML ad alanları formun http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (burada YYYY ve MM yıl ve ay sırasıyla temsil eder) SSDL için ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="097ba-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="097ba-643">Bu forma sahip ad alanları, özel öğeleri ve öznitelikleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="097ba-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="097ba-644">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="097ba-644">Applicable Attributes</span></span>

<span data-ttu-id="097ba-645">Aşağıdaki tabloda öznitelikleri açıklar uygulanabilir **şema** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="097ba-646">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="097ba-646">Attribute Name</span></span>            | <span data-ttu-id="097ba-647">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="097ba-647">Is Required</span></span> | <span data-ttu-id="097ba-648">Değer</span><span class="sxs-lookup"><span data-stu-id="097ba-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="097ba-649">**Namespace**</span></span>             | <span data-ttu-id="097ba-650">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-650">Yes</span></span>         | <span data-ttu-id="097ba-651">Depolama modelinin ad alanı.</span><span class="sxs-lookup"><span data-stu-id="097ba-651">The namespace of the storage model.</span></span> <span data-ttu-id="097ba-652">Değerini **Namespace** özniteliği, bir türün tam adı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="097ba-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="097ba-653">Örneğin, bir **EntityType** adlı *müşteri* ExampleModel.Store ad alanınıza ve sonra tam adı olduğunu **EntityType** olduğu ExampleModel.Store.Customer.</span><span class="sxs-lookup"><span data-stu-id="097ba-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="097ba-654">Aşağıdaki dize değeri olarak kullanılamaz **Namespace** özniteliği: **sistem**, **geçici**, veya **Edm**.</span><span class="sxs-lookup"><span data-stu-id="097ba-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="097ba-655">Değeri **Namespace** öznitelik değeri olarak aynı olamaz **Namespace** CSDL şema öğesindeki özniteliği.</span><span class="sxs-lookup"><span data-stu-id="097ba-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="097ba-656">**Diğer ad**</span><span class="sxs-lookup"><span data-stu-id="097ba-656">**Alias**</span></span>                 | <span data-ttu-id="097ba-657">Hayır</span><span class="sxs-lookup"><span data-stu-id="097ba-657">No</span></span>          | <span data-ttu-id="097ba-658">Ad alanı adı yerine kullanılan tanımlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="097ba-659">Örneğin, bir **EntityType** adlı *müşteri* ExampleModel.Store ad alanı ve değerini **diğer** özniteliği *StorageModel*, tam nitelikli adı olarak StorageModel.Customer kullanabilirsiniz **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="097ba-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="097ba-660">**Sağlayıcı**</span><span class="sxs-lookup"><span data-stu-id="097ba-660">**Provider**</span></span>              | <span data-ttu-id="097ba-661">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-661">Yes</span></span>         | <span data-ttu-id="097ba-662">Veri sağlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="097ba-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="097ba-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="097ba-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="097ba-664">Evet</span><span class="sxs-lookup"><span data-stu-id="097ba-664">Yes</span></span>         | <span data-ttu-id="097ba-665">Sağlayıcıya döndürmek için hangi sağlayıcı bildirimi gösteren bir belirteç.</span><span class="sxs-lookup"><span data-stu-id="097ba-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="097ba-666">Biçim belirteci için tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="097ba-666">No format for the token is defined.</span></span> <span data-ttu-id="097ba-667">Belirteç için değer sağlayıcı tarafından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="097ba-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="097ba-668">SQL Server sağlayıcısı bildirimi belirteçleri hakkında daha fazla bilgi için Entity Framework için SqlClient bakın.</span><span class="sxs-lookup"><span data-stu-id="097ba-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="097ba-669">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-669">Example</span></span>

<span data-ttu-id="097ba-670">Aşağıdaki örnekte gösterildiği bir **şema** öğesini içeren bir **EntityContainer** öğesinde, iki **EntityType** öğeleri ve bir **ilişkilendirme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a><span data-ttu-id="097ba-671">Ek açıklama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="097ba-671">Annotation Attributes</span></span>

<span data-ttu-id="097ba-672">Ek açıklama özniteliklerinde depo şeması tanım dili (SSDL) depolama modelindeki öğeleri hakkında ek meta verileri sağlayan özel XML öznitelikleri depolama modelinde var.</span><span class="sxs-lookup"><span data-stu-id="097ba-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="097ba-673">Geçerli XML yapısına sahip olmaya ek olarak, ek açıklama özniteliklerine yapılan aşağıdaki kısıtlamalar uygulanır:</span><span class="sxs-lookup"><span data-stu-id="097ba-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="097ba-674">Ek açıklama öznitelikleri SSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="097ba-675">Her iki ek açıklama özniteliklerin tam adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="097ba-676">Ek açıklama birden fazla öznitelik verilen bir SSDL öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="097ba-677">Ek açıklama öğesinde bulunan meta veriler, çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-678">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-678">Example</span></span>

<span data-ttu-id="097ba-679">Aşağıdaki örnek, uygulanan bir ek açıklama özniteliği olan bir EntityType öğeyi gösterir. **OrderID** özelliği.</span><span class="sxs-lookup"><span data-stu-id="097ba-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="097ba-680">Örnek ayrıca, eklenen bir ek açıklama öğesi show **EntityType** öğesi.</span><span class="sxs-lookup"><span data-stu-id="097ba-680">The example also show an annotation element added to the **EntityType** element.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="097ba-681">Ek açıklama öğelerinin (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="097ba-682">Depo şeması tanım dili (SSDL) ek açıklama öğesinde depolama modeli hakkında ek meta verileri sağlayan özel XML öğeleri depolama modelinde var.</span><span class="sxs-lookup"><span data-stu-id="097ba-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="097ba-683">Geçerli XML yapısına sahip olmaya ek olarak, ek açıklama öğelerinin aşağıdaki kısıtlamalar uygulanır:</span><span class="sxs-lookup"><span data-stu-id="097ba-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="097ba-684">Ek açıklama öğelerinin SSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="097ba-685">Her iki ek açıklama öğelerinin tam adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="097ba-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="097ba-686">Diğer tüm alt öğeleri verilen SSDL öğenin sonra ek açıklama öğelerinin görünmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="097ba-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="097ba-687">Birden çok ek açıklama öğesi, belirli bir SSDL öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="097ba-688">.NET Framework sürüm 4 ile başlayarak, ek açıklama öğesinde bulunan meta veriler çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="097ba-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="097ba-689">Örnek</span><span class="sxs-lookup"><span data-stu-id="097ba-689">Example</span></span>

<span data-ttu-id="097ba-690">Aşağıdaki örnek, bir ek açıklama öğesi olan bir EntityType öğeyi gösterir (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="097ba-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="097ba-691">Örnek ayrıca uygulanan bir ek açıklama özniteliği gösterir **OrderID** özelliği.</span><span class="sxs-lookup"><span data-stu-id="097ba-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a><span data-ttu-id="097ba-692">Modelleri (SSDL)</span><span class="sxs-lookup"><span data-stu-id="097ba-692">Facets (SSDL)</span></span>

<span data-ttu-id="097ba-693">Depo şeması tanım dili (SSDL), modelleri özelliği öğelerinde belirtilen sütun türleri kısıtlamaları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="097ba-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="097ba-694">Modeller üzerinde XML özniteliği uygulandığından **özelliği** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="097ba-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="097ba-695">Aşağıdaki tabloda SSDL desteklenen özellikleri açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="097ba-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="097ba-696">Modeli</span><span class="sxs-lookup"><span data-stu-id="097ba-696">Facet</span></span>           | <span data-ttu-id="097ba-697">Açıklama</span><span class="sxs-lookup"><span data-stu-id="097ba-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="097ba-698">**Harmanlama**</span><span class="sxs-lookup"><span data-stu-id="097ba-698">**Collation**</span></span>   | <span data-ttu-id="097ba-699">Harmanlama dizisi (veya sıralama) yapılırken kullanılacak karşılaştırma gerçekleştirme ve özellik değerleri üzerinde işlem sıralama belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="097ba-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="097ba-700">**FixedLength**</span></span> | <span data-ttu-id="097ba-701">Sütun değeri uzunluğunu değişebilir olup olmadığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="097ba-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="097ba-702">**MaxLength**</span></span>   | <span data-ttu-id="097ba-703">Sütun değeri en büyük uzunluğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="097ba-704">**Duyarlık**</span><span class="sxs-lookup"><span data-stu-id="097ba-704">**Precision**</span></span>   | <span data-ttu-id="097ba-705">Tür özellikleri için **ondalık**, bir özellik değeri olabilir basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="097ba-706">Tür özellikleri için **zaman**, **DateTime**, ve **DateTimeOffset**, sütun değerinin saniye kesirli kısmını için basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="097ba-707">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="097ba-707">**Scale**</span></span>       | <span data-ttu-id="097ba-708">Sütun değeri ondalık noktasının sağındaki basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="097ba-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="097ba-709">**Unicode**</span></span>     | <span data-ttu-id="097ba-710">Sütun değeri Unicode mi depolanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="097ba-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |