---
title: SSDL belirtimi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182542"
---
# <a name="ssdl-specification"></a><span data-ttu-id="64583-102">SSDL Belirtimi</span><span class="sxs-lookup"><span data-stu-id="64583-102">SSDL Specification</span></span>
<span data-ttu-id="64583-103">Mağaza şeması tanım dili (SSDL), bir Entity Framework uygulamasının depolama modelini tanımlayan XML tabanlı bir dildir.</span><span class="sxs-lookup"><span data-stu-id="64583-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="64583-104">Entity Framework uygulamasında, depolama modeli meta verileri bir. ssdl dosyasından (SSDL dilinde yazılmıştır) System. Data. Metadata. Edm. StoreItemCollection örneğine yüklenir ve içindeki yöntemler kullanılarak erişilebilir. System. Data. Metadata. Edm. MetadataWorkspace sınıfı.</span><span class="sxs-lookup"><span data-stu-id="64583-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="64583-105">Entity Framework, sorguları, verileri depolamak için kavramsal modele dönüştürmek üzere depolama modeli meta verilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="64583-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="64583-106">Entity Framework Designer (EF Designer), tasarım zamanında depolama modeli bilgilerini bir. edmx dosyasında depolar.</span><span class="sxs-lookup"><span data-stu-id="64583-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="64583-107">Derleme zamanında Entity Desisgner, çalışma zamanında Entity Framework gereken. ssdl dosyasını oluşturmak için bir. edmx dosyasındaki bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="64583-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="64583-108">SSDL sürümleri, XML ad alanları ile farklılaştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="64583-109">SSDL sürümü</span><span class="sxs-lookup"><span data-stu-id="64583-109">SSDL Version</span></span> | <span data-ttu-id="64583-110">XML ad alanı</span><span class="sxs-lookup"><span data-stu-id="64583-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="64583-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="64583-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="64583-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="64583-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="64583-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="64583-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="64583-114">Association öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-114">Association Element (SSDL)</span></span>

<span data-ttu-id="64583-115">Depo şeması tanım dili (SSDL) içindeki bir **ilişkilendirme** öğesi, temel alınan veritabanındaki bir yabancı anahtar kısıtlamasına katılan tablo sütunlarını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="64583-116">İki gerekli alt uç öğesi, ilişkinin sonunda tabloları ve her uçta çoğulluğu belirler.</span><span class="sxs-lookup"><span data-stu-id="64583-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="64583-117">İsteğe bağlı bir ReferentialConstraint öğesi, ilişkilendirmenin ve katılan sütunların asıl ve bağımlı uçlarını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="64583-118">**ReferentialConstraint** öğesi yoksa, ilişkilendirmenin sütun eşlemelerini belirtmek Için bir associationsetmapping öğesi kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="64583-119">**Association** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-120">Belgeler (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="64583-121">Son (tam olarak iki)</span><span class="sxs-lookup"><span data-stu-id="64583-121">End (exactly two)</span></span>
-   <span data-ttu-id="64583-122">ReferentialConstraint (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="64583-123">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-124">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-124">Applicable Attributes</span></span>

<span data-ttu-id="64583-125">Aşağıdaki tabloda **ilişkilendirme** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="64583-126">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-126">Attribute Name</span></span> | <span data-ttu-id="64583-127">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-127">Is Required</span></span> | <span data-ttu-id="64583-128">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="64583-129">**Ad**</span><span class="sxs-lookup"><span data-stu-id="64583-129">**Name**</span></span>       | <span data-ttu-id="64583-130">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-130">Yes</span></span>         | <span data-ttu-id="64583-131">Temel alınan veritabanında karşılık gelen yabancı anahtar kısıtlamasının adı.</span><span class="sxs-lookup"><span data-stu-id="64583-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-132">**İlişkilendirme** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="64583-133">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-134">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-135">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-135">Example</span></span>

<span data-ttu-id="64583-136">Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasına katılan sütunları belirtmek Için **ReferentialConstraint** öğesi kullanan bir **ilişkilendirme** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="64583-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="64583-137">AssociationSet öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="64583-138">Depo şeması tanım dili (SSDL) içindeki **AssociationSet** öğesi, temel alınan veritabanındaki iki tablo arasındaki yabancı anahtar kısıtlamasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="64583-139">Yabancı anahtar kısıtlamasına katılan tablo sütunları bir Ilişkilendirme öğesinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="64583-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="64583-140">Belirli bir **AssociationSet** öğesine karşılık gelen **Association** öğesi **AssociationSet** öğesinin **Association** özniteliğinde belirtildi.</span><span class="sxs-lookup"><span data-stu-id="64583-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="64583-141">SSDL Association kümeleri, bir AssociationSetMapping öğesi tarafından CSDL ilişkilendirme kümelerine eşlenir.</span><span class="sxs-lookup"><span data-stu-id="64583-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="64583-142">Ancak, belirli bir CSDL ilişki kümesi için CSDL ilişkilendirmesi bir ReferentialConstraint öğesi kullanılarak tanımlanmışsa, karşılık gelen bir **Associationsetmapping** öğesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="64583-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="64583-143">Bu durumda, bir **Associationsetmapping** öğesi varsa, tanımladığı eşlemeler **ReferentialConstraint** öğesi tarafından geçersiz kılınır.</span><span class="sxs-lookup"><span data-stu-id="64583-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="64583-144">**AssociationSet** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-145">Belgeler (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="64583-146">End (sıfır veya iki)</span><span class="sxs-lookup"><span data-stu-id="64583-146">End (zero or two)</span></span>
-   <span data-ttu-id="64583-147">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-148">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-148">Applicable Attributes</span></span>

<span data-ttu-id="64583-149">Aşağıdaki tabloda **AssociationSet** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="64583-150">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-150">Attribute Name</span></span>  | <span data-ttu-id="64583-151">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-151">Is Required</span></span> | <span data-ttu-id="64583-152">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-153">**Ad**</span><span class="sxs-lookup"><span data-stu-id="64583-153">**Name**</span></span>        | <span data-ttu-id="64583-154">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-154">Yes</span></span>         | <span data-ttu-id="64583-155">İlişki kümesinin temsil ettiği yabancı anahtar kısıtlamasının adı.</span><span class="sxs-lookup"><span data-stu-id="64583-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="64583-156">**İlişkilendirme**</span><span class="sxs-lookup"><span data-stu-id="64583-156">**Association**</span></span> | <span data-ttu-id="64583-157">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-157">Yes</span></span>         | <span data-ttu-id="64583-158">Yabancı anahtar kısıtlamasına katılan sütunları tanımlayan ilişkilendirmenin adı.</span><span class="sxs-lookup"><span data-stu-id="64583-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-159">**AssociationSet** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="64583-160">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-161">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-162">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-162">Example</span></span>

<span data-ttu-id="64583-163">Aşağıdaki örnek, temel veritabanında `FK_CustomerOrders` yabancı anahtar kısıtlamasını temsil eden bir **AssociationSet** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="64583-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="64583-164">CollectionType öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="64583-165">Depo şeması tanım dili (SSDL) içindeki **CollectionType** öğesi, bir işlevin dönüş türünün bir koleksiyon olduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="64583-166">**CollectionType** öğesi ReturnType öğesinin bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="64583-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="64583-167">Koleksiyon türü RowType alt öğesi kullanılarak belirtilir:</span><span class="sxs-lookup"><span data-stu-id="64583-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="64583-168">**CollectionType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="64583-169">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-170">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-171">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-171">Example</span></span>

<span data-ttu-id="64583-172">Aşağıdaki örnek, işlevin bir satır koleksiyonu döndüreceğini belirtmek için bir **CollectionType** öğesi kullanan bir işlevi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="64583-173">CommandText öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="64583-174">Depo şeması tanım dili (SSDL) içindeki **CommandText** öğesi, bir işlev öğesinin, veritabanında yürütülen bir SQL ifadesini tanımlamanızı sağlayan bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="64583-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="64583-175">**CommandText** öğesi, veritabanında saklı yordama benzer işlevler eklemenize olanak tanır, ancak **CommandText** öğesini depolama modelinde tanımlarsınız.</span><span class="sxs-lookup"><span data-stu-id="64583-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="64583-176">**CommandText** öğesi alt öğeleri içeremez.</span><span class="sxs-lookup"><span data-stu-id="64583-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="64583-177">**CommandText** öğesinin gövdesi, temel alınan veritabanı için GEÇERLI bir SQL bildirisi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="64583-178">**CommandText** öğesi için geçerli bir öznitelik yok.</span><span class="sxs-lookup"><span data-stu-id="64583-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="64583-179">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-179">Example</span></span>

<span data-ttu-id="64583-180">Aşağıdaki örnek, alt **CommandText** öğesi olan bir **Function** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="64583-181">**Updateproductınorder** işlevini, kavramsal modele aktararak ObjectContext üzerinde bir yöntem olarak kullanıma sunun.</span><span class="sxs-lookup"><span data-stu-id="64583-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="64583-182">DefiningQuery öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="64583-183">Depo şeması tanım dili (SSDL) içindeki **Definingquery** öğesi, doğrudan temel VERITABANıNDA bir SQL ifadesini yürütmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="64583-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="64583-184">**Definingquery** öğesi yaygın olarak bir veritabanı görünümü gibi kullanılır, ancak görünüm veritabanı yerine depolama modelinde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="64583-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="64583-185">Bir **Definingquery** öğesinde tanımlanan görünüm, bir EntitySetMapping öğesi aracılığıyla kavramsal modeldeki bir varlık türüne eşlenebilir.</span><span class="sxs-lookup"><span data-stu-id="64583-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="64583-186">Bu eşlemeler salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="64583-186">These mappings are read-only.</span></span>  

<span data-ttu-id="64583-187">Aşağıdaki SSDL sözdizimi, bir **EntitySet** 'in ardından, görünümü almak için kullanılan bir sorgu Içeren **definingquery** öğesiyle ilgili bildirimi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="64583-188">Görünümler üzerinde okuma yazma senaryolarını etkinleştirmek için Entity Framework saklı yordamları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="64583-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="64583-189"> Veri alma ve saklı yordamlara göre değişiklik işleme için temel tablo olarak bir veri kaynağı görünümü veya Entity SQL görünümü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="64583-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="64583-190">Microsoft SQL Server Compact 3,5 ' i hedeflemek için **Definingquery** öğesini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="64583-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="64583-191">SQL Server Compact 3,5, saklı yordamları desteklemediğinden, **Definingquery** öğesiyle benzer işlevleri de uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="64583-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="64583-192">Yararlı olabilecek başka bir yerde, programlama dilinde kullanılan veri türleri ve veri kaynağının bir uyuşmazlığını aşmak için saklı yordamlar oluşturma işlemi yer alabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="64583-193">Belirli bir parametre kümesini alan bir **Definingquery** yazabilir ve ardından farklı bir parametre kümesiyle (örneğin, verileri silen bir saklı yordam) çağrı yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="64583-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="64583-194">Bağımlı öğe (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="64583-195">Depo şeması tanım dili (SSDL) içindeki **bağımlı** öğe, bir yabancı anahtar kısıtlamasının bağımlı sonunu tanımlayan ReferentialConstraint öğesinin bir alt öğesidir (başvuru kısıtlaması olarak da bilinir).</span><span class="sxs-lookup"><span data-stu-id="64583-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="64583-196">**Bağımlı** öğe, birincil anahtar sütununa (veya sütunlara) başvuran bir tablodaki sütunu (veya sütunları) belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="64583-197">**Propertyref** öğeleri hangi sütunlara başvurulduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="64583-198">Principal öğesi, **bağımlı** öğede belirtilen sütunların başvurduğu birincil anahtar sütunlarını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="64583-199">**Bağımlı** öğe aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-200">PropertyRef (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="64583-201">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-202">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-202">Applicable Attributes</span></span>

<span data-ttu-id="64583-203">Aşağıdaki tabloda **bağımlı** öğeye uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="64583-204">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-204">Attribute Name</span></span> | <span data-ttu-id="64583-205">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-205">Is Required</span></span> | <span data-ttu-id="64583-206">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-207">**Rol**</span><span class="sxs-lookup"><span data-stu-id="64583-207">**Role**</span></span>       | <span data-ttu-id="64583-208">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-208">Yes</span></span>         | <span data-ttu-id="64583-209">Karşılık gelen End öğesinin **rol** özniteliğiyle aynı değer (kullanılıyorsa); Aksi takdirde, başvuran sütununu içeren tablonun adı.</span><span class="sxs-lookup"><span data-stu-id="64583-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-210">Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **bağımlı** öğeye uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="64583-211">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="64583-212">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-213">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-213">Example</span></span>

<span data-ttu-id="64583-214">Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasına katılan sütunları belirtmek Için **ReferentialConstraint** öğesi kullanan bir ilişkilendirme öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="64583-215">**Bağımlı** öğe, kısıtlamanın bağımlı sonu olarak **Order** tablosunun **CustomerID** sütununu belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="64583-216">Documentation öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="64583-217">Depo şeması tanım dili (SSDL) içindeki **Belgeler** öğesi, bir üst öğede tanımlanan bir nesne hakkında bilgi sağlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="64583-218">**Belge** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-219">**Özet**: üst öğenin kısa bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="64583-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="64583-220">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="64583-220">(zero or one element)</span></span>
-   <span data-ttu-id="64583-221">**LongDescription**: üst öğenin kapsamlı bir açıklaması.</span><span class="sxs-lookup"><span data-stu-id="64583-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="64583-222">(sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="64583-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-223">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-223">Applicable Attributes</span></span>

<span data-ttu-id="64583-224">**Belge** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="64583-225">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="64583-226">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-227">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-227">Example</span></span>

<span data-ttu-id="64583-228">Aşağıdaki örnek, bir EntityType öğesinin alt öğesi olarak **documentation** öğesini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="64583-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="64583-229">End öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-229">End Element (SSDL)</span></span>

<span data-ttu-id="64583-230">Depo şeması tanım dili (SSDL) içindeki **bitiş** öğesi, temel veritabanında yabancı anahtar kısıtlamasının bir sonundaki tablo ve satır sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="64583-231">**End** öğesi Association öğesinin ya da AssociationSet öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="64583-232">Her durumda, olası alt öğeler ve ilgili öznitelikler farklıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="64583-233">Ilişki öğesinin bir alt öğesi olarak end öğesi</span><span class="sxs-lookup"><span data-stu-id="64583-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="64583-234">Bir **End** öğesi ( **Association** öğesinin bir alt öğesi olarak), bir yabancı anahtar kısıtlamasının sonunda **tür** ve **çokluk** öznitelikleri sırasıyla tablo ve satır sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="64583-235">Yabancı anahtar kısıtlamasının uçları bir SSDL ilişkisinin parçası olarak tanımlanır; SSDL Association 'ın tam olarak iki ucu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="64583-236">Bir **End** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-237">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="64583-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="64583-238">OnDelete (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="64583-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="64583-239">Ek açıklama öğeleri (sıfır veya daha fazla öğe)</span><span class="sxs-lookup"><span data-stu-id="64583-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="64583-240">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-240">Applicable Attributes</span></span>

<span data-ttu-id="64583-241">Aşağıdaki tabloda, bir **ilişkilendirme** öğesinin alt öğesi olduğunda, **End** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="64583-242">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-242">Attribute Name</span></span>   | <span data-ttu-id="64583-243">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-243">Is Required</span></span> | <span data-ttu-id="64583-244">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-245">**Tür**</span><span class="sxs-lookup"><span data-stu-id="64583-245">**Type**</span></span>         | <span data-ttu-id="64583-246">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-246">Yes</span></span>         | <span data-ttu-id="64583-247">Yabancı anahtar kısıtlamasının sonundaki SSDL varlık kümesinin tam adı.</span><span class="sxs-lookup"><span data-stu-id="64583-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="64583-248">**Rol**</span><span class="sxs-lookup"><span data-stu-id="64583-248">**Role**</span></span>         | <span data-ttu-id="64583-249">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-249">No</span></span>          | <span data-ttu-id="64583-250">Karşılık gelen ReferentialConstraint öğesinin Principal veya Dependent öğesinde **rol** özniteliğinin değeri (kullanılıyorsa).</span><span class="sxs-lookup"><span data-stu-id="64583-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="64583-251">**Ğunun**</span><span class="sxs-lookup"><span data-stu-id="64583-251">**Multiplicity**</span></span> | <span data-ttu-id="64583-252">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-252">Yes</span></span>         | <span data-ttu-id="64583-253">**1**, **0.. 1**veya yabancı anahtar kısıtlamasının sonunda olabilecek satır sayısına göre **\*** .</span><span class="sxs-lookup"><span data-stu-id="64583-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="64583-254">**1** yabancı anahtar kısıtlama ucunda tam olarak bir satır olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="64583-255">**0.. 1** yabancı anahtar kısıtlaması sonunda sıfır veya bir satırın bulunduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="64583-256">**\*** , yabancı anahtar kısıtlaması sonunda sıfır, bir veya daha fazla satırın bulunduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-257">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **End** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="64583-258">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="64583-259">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="64583-260">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-260">Example</span></span>

<span data-ttu-id="64583-261">Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasını tanımlayan bir **ilişkilendirme** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="64583-262">Her **bitiş** öğesinde belirtilen **çokluk** değerleri, **Orders** tablosundaki birçok satırın **Customers** tablosundaki bir satırla ilişkilendirilemeyeceğini gösterir, ancak **Customers** tablosundaki yalnızca bir satır **Orders** tablosundaki bir satırla ilişkilendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="64583-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="64583-263">Ayrıca, **OnDelete** öğesi, **Customers** tablosundaki satır silindiğinde, **müşteriler** tablosundaki belirli bir satıra başvuran **Orders** tablosundaki tüm satırların silineceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="64583-264">Öğeyi AssociationSet öğesinin bir alt öğesi olarak bitir</span><span class="sxs-lookup"><span data-stu-id="64583-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="64583-265">**End** öğesi ( **AssociationSet** öğesinin bir alt öğesi olarak), temel alınan veritabanında yabancı anahtar kısıtlamasının bir sonundaki tabloyu belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="64583-266">Bir **End** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-267">Belgeler (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="64583-268">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="64583-269">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-269">Applicable Attributes</span></span>

<span data-ttu-id="64583-270">Aşağıdaki tabloda, bir **AssociationSet** öğesinin alt öğesi olduğunda **End** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="64583-271">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-271">Attribute Name</span></span> | <span data-ttu-id="64583-272">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-272">Is Required</span></span> | <span data-ttu-id="64583-273">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-274">**Di**</span><span class="sxs-lookup"><span data-stu-id="64583-274">**EntitySet**</span></span>  | <span data-ttu-id="64583-275">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-275">Yes</span></span>         | <span data-ttu-id="64583-276">Yabancı anahtar kısıtlamasının sonundaki SSDL varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="64583-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="64583-277">**Rol**</span><span class="sxs-lookup"><span data-stu-id="64583-277">**Role**</span></span>       | <span data-ttu-id="64583-278">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-278">No</span></span>          | <span data-ttu-id="64583-279">Karşılık gelen Ilişkilendirme öğesinin bir **End** öğesinde belirtilen **rol** özniteliklerinden birinin değeri.</span><span class="sxs-lookup"><span data-stu-id="64583-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-280">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **End** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="64583-281">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="64583-282">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="64583-283">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-283">Example</span></span>

<span data-ttu-id="64583-284">Aşağıdaki örnek, iki **End** öğesiyle bir **AssociationSet** öğesi olan bir **EntityContainer** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="64583-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="64583-285">EntityContainer öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="64583-286">Depo şeması tanım dili (SSDL) içindeki bir **EntityContainer** öğesi, bir Entity Framework uygulamasında temel alınan veri kaynağının yapısını AÇıKLAR: SSDL varlık kümeleri (EntitySet öğelerinde tanımlanan) bir veritabanındaki tabloları temsil eder, SSDL varlık türleri (EntityType öğelerinde tanımlanmıştır) bir tablodaki satırları temsil eder ve ilişki kümeleri (AssociationSet öğelerinde tanımlanmıştır) bir veritabanındaki yabancı anahtar kısıtlamalarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="64583-287">Bir depolama modeli varlık kapsayıcısı, EntityContainerMapping öğesi aracılığıyla bir kavramsal model varlığı kapsayıcısına eşlenir.</span><span class="sxs-lookup"><span data-stu-id="64583-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="64583-288">Bir **EntityContainer** öğesi sıfır veya bir belge öğesine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="64583-289">Bir **belge** öğesi mevcutsa, diğer tüm alt öğelerin önüne gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="64583-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="64583-290">Bir **EntityContainer** öğesi aşağıdaki alt öğeleri sıfır veya daha fazla içerebilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-291">Di</span><span class="sxs-lookup"><span data-stu-id="64583-291">EntitySet</span></span>
-   <span data-ttu-id="64583-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="64583-292">AssociationSet</span></span>
-   <span data-ttu-id="64583-293">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="64583-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-294">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-294">Applicable Attributes</span></span>

<span data-ttu-id="64583-295">Aşağıdaki tabloda, **EntityContainer** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="64583-296">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-296">Attribute Name</span></span> | <span data-ttu-id="64583-297">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-297">Is Required</span></span> | <span data-ttu-id="64583-298">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="64583-299">**Ad**</span><span class="sxs-lookup"><span data-stu-id="64583-299">**Name**</span></span>       | <span data-ttu-id="64583-300">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-300">Yes</span></span>         | <span data-ttu-id="64583-301">Varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="64583-301">The name of the entity container.</span></span> <span data-ttu-id="64583-302">Bu ad nokta (.) içeremez.</span><span class="sxs-lookup"><span data-stu-id="64583-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-303">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **EntityContainer** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="64583-304">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-305">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-306">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-306">Example</span></span>

<span data-ttu-id="64583-307">Aşağıdaki örnek, iki varlık kümesini ve bir ilişki kümesini tanımlayan bir **EntityContainer** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="64583-308">Varlık türü ve ilişkilendirme türü adlarının kavramsal model ad alanı adı tarafından nitelenmiş olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="64583-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="64583-309">EntitySet öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="64583-310">Depo şeması tanım dili (SSDL) içindeki bir **EntitySet** öğesi, temel alınan veritabanında bir tabloyu veya görünümü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="64583-311">SSDL içindeki bir EntityType öğesi tablo veya görünümdeki bir satırı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="64583-312">Bir **EntitySet** öğesinin **EntityType** özniteliği, bir SSDL varlık kümesindeki satırları temsil eden özel SSDL varlık türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="64583-313">Bir CSDL varlık kümesi ve bir SSDL varlık kümesi arasındaki eşleme bir EntitySetMapping öğesinde belirtildi.</span><span class="sxs-lookup"><span data-stu-id="64583-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="64583-314">**EntitySet** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-315">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="64583-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="64583-316">DefiningQuery (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="64583-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="64583-317">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="64583-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-318">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-318">Applicable Attributes</span></span>

<span data-ttu-id="64583-319">Aşağıdaki tabloda, **EntitySet** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="64583-320">Bazı öznitelikler (burada listelenmemiş) **Mağaza** diğer adıyla nitelenmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="64583-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="64583-321">Bu öznitelikler bir model güncelleştirilirken model Güncelleştirme Sihirbazı tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64583-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="64583-322">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-322">Attribute Name</span></span> | <span data-ttu-id="64583-323">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-323">Is Required</span></span> | <span data-ttu-id="64583-324">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-325">**Ad**</span><span class="sxs-lookup"><span data-stu-id="64583-325">**Name**</span></span>       | <span data-ttu-id="64583-326">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-326">Yes</span></span>         | <span data-ttu-id="64583-327">Varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="64583-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="64583-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="64583-328">**EntityType**</span></span> | <span data-ttu-id="64583-329">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-329">Yes</span></span>         | <span data-ttu-id="64583-330">Varlık kümesinin örnek içerdiği varlık türünün tam adı.</span><span class="sxs-lookup"><span data-stu-id="64583-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="64583-331">**Şema**</span><span class="sxs-lookup"><span data-stu-id="64583-331">**Schema**</span></span>     | <span data-ttu-id="64583-332">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-332">No</span></span>          | <span data-ttu-id="64583-333">Veritabanı şeması.</span><span class="sxs-lookup"><span data-stu-id="64583-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="64583-334">**Tablo**</span><span class="sxs-lookup"><span data-stu-id="64583-334">**Table**</span></span>      | <span data-ttu-id="64583-335">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-335">No</span></span>          | <span data-ttu-id="64583-336">Veritabanı tablosu.</span><span class="sxs-lookup"><span data-stu-id="64583-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="64583-337">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **EntitySet** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="64583-338">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-339">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-340">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-340">Example</span></span>

<span data-ttu-id="64583-341">Aşağıdaki örnek, iki **EntitySet** öğesine ve bir **AssociationSet** öğesine sahip bir **EntityContainer** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="64583-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="64583-342">EntityType öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="64583-343">Depo şeması tanım dili (SSDL) içindeki bir **EntityType** öğesi, alttaki veritabanının bir tablosundaki veya görünümündeki bir satırı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="64583-344">SSDL içindeki bir EntitySet öğesi, satırların oluştuğu tabloyu veya görünümü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="64583-345">Bir **EntitySet** öğesinin **EntityType** özniteliği, bir SSDL varlık kümesindeki satırları temsil eden özel SSDL varlık türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="64583-346">Bir SSDL varlık türü ve bir CSDL varlık türü arasındaki eşleme bir EntityTypeMapping öğesinde belirtildi.</span><span class="sxs-lookup"><span data-stu-id="64583-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="64583-347">**EntityType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-348">Belgeler (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="64583-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="64583-349">Anahtar (sıfır veya bir öğe)</span><span class="sxs-lookup"><span data-stu-id="64583-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="64583-350">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="64583-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-351">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-351">Applicable Attributes</span></span>

<span data-ttu-id="64583-352">Aşağıdaki tabloda, **EntityType** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="64583-353">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-353">Attribute Name</span></span> | <span data-ttu-id="64583-354">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-354">Is Required</span></span> | <span data-ttu-id="64583-355">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-356">**Ad**</span><span class="sxs-lookup"><span data-stu-id="64583-356">**Name**</span></span>       | <span data-ttu-id="64583-357">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-357">Yes</span></span>         | <span data-ttu-id="64583-358">Varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="64583-358">The name of the entity type.</span></span> <span data-ttu-id="64583-359">Bu değer genellikle varlık türünün bir satırı temsil ettiği tablonun adı ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="64583-360">Bu değer, nokta (.) içeremez.</span><span class="sxs-lookup"><span data-stu-id="64583-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-361">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **EntityType** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="64583-362">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-363">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-364">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-364">Example</span></span>

<span data-ttu-id="64583-365">Aşağıdaki örnek, iki özelliği olan bir **EntityType** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="64583-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="64583-366">Function öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-366">Function Element (SSDL)</span></span>

<span data-ttu-id="64583-367">Depo şeması tanım dili (SSDL) içindeki **işlev** öğesi, temel alınan veritabanında bulunan bir saklı yordamı belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="64583-368">**Function** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-369">Belgeler (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="64583-370">Parametre (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="64583-371">CommandText (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="64583-372">ReturnType (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="64583-373">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="64583-374">İşlevin dönüş türü, **ReturnType** öğesi ya da **ReturnType** özniteliğiyle belirtilmelidir, ancak her ikisine birden değil.</span><span class="sxs-lookup"><span data-stu-id="64583-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="64583-375">Depolama modelinde belirtilen saklı yordamlar, bir uygulamanın kavramsal modeline aktarılabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="64583-376">Daha fazla bilgi için bkz. [Saklı yordamlarla sorgulama](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="64583-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="64583-377">**İşlev** öğesi, depolama modelindeki özel işlevleri tanımlamak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="64583-378">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-378">Applicable Attributes</span></span>

<span data-ttu-id="64583-379">Aşağıdaki tabloda, **işlev** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="64583-380">Bazı öznitelikler (burada listelenmemiş) **Mağaza** diğer adıyla nitelenmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="64583-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="64583-381">Bu öznitelikler bir model güncelleştirilirken model Güncelleştirme Sihirbazı tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64583-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="64583-382">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-382">Attribute Name</span></span>             | <span data-ttu-id="64583-383">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-383">Is Required</span></span> | <span data-ttu-id="64583-384">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-385">**Ad**</span><span class="sxs-lookup"><span data-stu-id="64583-385">**Name**</span></span>                   | <span data-ttu-id="64583-386">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-386">Yes</span></span>         | <span data-ttu-id="64583-387">Saklı yordamın adı.</span><span class="sxs-lookup"><span data-stu-id="64583-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="64583-388">**'Indaki**</span><span class="sxs-lookup"><span data-stu-id="64583-388">**ReturnType**</span></span>             | <span data-ttu-id="64583-389">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-389">No</span></span>          | <span data-ttu-id="64583-390">Saklı yordamın dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="64583-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="64583-391">**Birleşik**</span><span class="sxs-lookup"><span data-stu-id="64583-391">**Aggregate**</span></span>              | <span data-ttu-id="64583-392">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-392">No</span></span>          | <span data-ttu-id="64583-393">Saklı yordam bir toplama değeri döndürürse **true** ; Aksi halde **yanlış**.</span><span class="sxs-lookup"><span data-stu-id="64583-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="64583-394">**Yerleik**</span><span class="sxs-lookup"><span data-stu-id="64583-394">**BuiltIn**</span></span>                | <span data-ttu-id="64583-395">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-395">No</span></span>          | <span data-ttu-id="64583-396">İşlev yerleşik bir<sup>1</sup> Işlevse **true** ; Aksi halde **yanlış**.</span><span class="sxs-lookup"><span data-stu-id="64583-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="64583-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="64583-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="64583-398">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-398">No</span></span>          | <span data-ttu-id="64583-399">Saklı yordamın adı.</span><span class="sxs-lookup"><span data-stu-id="64583-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="64583-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="64583-400">**NiladicFunction**</span></span>        | <span data-ttu-id="64583-401">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-401">No</span></span>          | <span data-ttu-id="64583-402">İşlev bir niladic<sup>2</sup> **işlevliyse doğru** ; Aksi takdirde **false** .</span><span class="sxs-lookup"><span data-stu-id="64583-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="64583-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="64583-403">**IsComposable**</span></span>           | <span data-ttu-id="64583-404">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-404">No</span></span>          | <span data-ttu-id="64583-405">İşlev birleştirilebilir<sup>3</sup> Işlevse **doğru** ; Aksi takdirde **false** .</span><span class="sxs-lookup"><span data-stu-id="64583-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="64583-406">**Parametertypesemantiği**</span><span class="sxs-lookup"><span data-stu-id="64583-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="64583-407">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-407">No</span></span>          | <span data-ttu-id="64583-408">İşlev aşırı yüklerini çözümlemek için kullanılan tür semantiğini tanımlayan sabit listesi.</span><span class="sxs-lookup"><span data-stu-id="64583-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="64583-409">Sabit listesi, işlev tanımı başına sağlayıcı bildiriminde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="64583-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="64583-410">Varsayılan değer **Allowwimplicitconversion**' dir.</span><span class="sxs-lookup"><span data-stu-id="64583-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="64583-411">**Şema**</span><span class="sxs-lookup"><span data-stu-id="64583-411">**Schema**</span></span>                 | <span data-ttu-id="64583-412">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-412">No</span></span>          | <span data-ttu-id="64583-413">Saklı yordamın tanımlandığı şemanın adı.</span><span class="sxs-lookup"><span data-stu-id="64583-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="64583-414"><sup>1</sup> yerleşik bir işlev, veritabanında tanımlanan bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="64583-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="64583-415">Depolama modelinde tanımlanan işlevler hakkında daha fazla bilgi için, bkz. CommandText öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="64583-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="64583-416"><sup>2</sup> bir nilasel işlevi, parametre içermeyen ve çağrıldığında parantez gerektirmeyen bir işlevdir.</span><span class="sxs-lookup"><span data-stu-id="64583-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="64583-417"><sup>3</sup> iki işlev, bir işlevin çıktısı diğer işlevin girişi olduğunda birleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="64583-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="64583-418">**İşlev** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="64583-419">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-420">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-421">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-421">Example</span></span>

<span data-ttu-id="64583-422">Aşağıdaki örnek **Updateorderquantity** saklı yordamına karşılık gelen bir **Function** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="64583-423">Saklı yordam iki parametreyi kabul eder ve bir değer döndürmez.</span><span class="sxs-lookup"><span data-stu-id="64583-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="64583-424">Anahtar öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-424">Key Element (SSDL)</span></span>

<span data-ttu-id="64583-425">Depo şeması tanım dili (SSDL) içindeki **anahtar** öğe, temel alınan veritabanındaki bir tablonun birincil anahtarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="64583-426">**Anahtar** , bir, tablodaki bir satırı temsil eden bir EntityType öğesinin alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="64583-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="64583-427">Birincil anahtar, **EntityType** öğesinde tanımlanmış bir veya daha fazla özellik öğesine başvurarak **anahtar** öğesinde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="64583-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="64583-428">**Anahtar** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-429">PropertyRef (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="64583-430">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="64583-430">Annotation elements</span></span>

<span data-ttu-id="64583-431">**Anahtar** öğesi için geçerli bir öznitelik yok.</span><span class="sxs-lookup"><span data-stu-id="64583-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="64583-432">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-432">Example</span></span>

<span data-ttu-id="64583-433">Aşağıdaki örnek, bir özelliğe başvuran bir anahtar içeren bir **EntityType** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="64583-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="64583-434">OnDelete öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="64583-435">Depo şeması tanım dili (SSDL) içindeki **OnDelete** öğesi, bir yabancı anahtar kısıtlamasına katılan bir satır silindiğinde veritabanı davranışını yansıtır.</span><span class="sxs-lookup"><span data-stu-id="64583-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="64583-436">Eylem **Cascade**olarak ayarlandıysa, silinmekte olan bir satıra başvuran satırlar da silinir.</span><span class="sxs-lookup"><span data-stu-id="64583-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="64583-437">Eylem **none**olarak ayarlandıysa, silinmekte olan bir satıra başvuran satırlar da silinmez.</span><span class="sxs-lookup"><span data-stu-id="64583-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="64583-438">**OnDelete** öğesi bir End öğesinin alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="64583-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="64583-439">**OnDelete** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-440">Belgeler (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="64583-441">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-442">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-442">Applicable Attributes</span></span>

<span data-ttu-id="64583-443">Aşağıdaki tabloda, **OnDelete** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="64583-444">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-444">Attribute Name</span></span> | <span data-ttu-id="64583-445">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-445">Is Required</span></span> | <span data-ttu-id="64583-446">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-447">**Eylem**</span><span class="sxs-lookup"><span data-stu-id="64583-447">**Action**</span></span>     | <span data-ttu-id="64583-448">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-448">Yes</span></span>         | <span data-ttu-id="64583-449">**Cascade** veya **none**.</span><span class="sxs-lookup"><span data-stu-id="64583-449">**Cascade** or **None**.</span></span> <span data-ttu-id="64583-450">( **Kısıtlanmış** değer geçerli ancak **none**ile aynı davranışa sahiptir.)</span><span class="sxs-lookup"><span data-stu-id="64583-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-451">**OnDelete** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="64583-452">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-453">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-454">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-454">Example</span></span>

<span data-ttu-id="64583-455">Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasını tanımlayan bir **ilişkilendirme** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="64583-456">**OnDelete** öğesi, **Customers** tablosundaki satır silindiğinde, **müşteriler** tablosundaki belirli bir satıra başvuran **Orders** tablosundaki tüm satırların silineceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="64583-457">Parameter öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="64583-458">Depo şeması tanım dili (SSDL) içindeki **Parameter** öğesi, bir işlev öğesinin bir alt öğesidir ve veritabanında saklı yordam için parametreleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="64583-459">**Parameter** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-460">Belgeler (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="64583-461">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-462">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-462">Applicable Attributes</span></span>

<span data-ttu-id="64583-463">Aşağıdaki tabloda, **parametre** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="64583-464">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-464">Attribute Name</span></span> | <span data-ttu-id="64583-465">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-465">Is Required</span></span> | <span data-ttu-id="64583-466">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-467">**Ad**</span><span class="sxs-lookup"><span data-stu-id="64583-467">**Name**</span></span>       | <span data-ttu-id="64583-468">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-468">Yes</span></span>         | <span data-ttu-id="64583-469">Parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="64583-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="64583-470">**Tür**</span><span class="sxs-lookup"><span data-stu-id="64583-470">**Type**</span></span>       | <span data-ttu-id="64583-471">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-471">Yes</span></span>         | <span data-ttu-id="64583-472">Parametre türü.</span><span class="sxs-lookup"><span data-stu-id="64583-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="64583-473">**Modundaysa**</span><span class="sxs-lookup"><span data-stu-id="64583-473">**Mode**</span></span>       | <span data-ttu-id="64583-474">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-474">No</span></span>          | <span data-ttu-id="64583-475">Parametresinin bir giriş, çıkış veya giriş/çıkış parametresi olup olmadığına bağlı olarak, **içinde**, **Out**veya **InOut** .</span><span class="sxs-lookup"><span data-stu-id="64583-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="64583-476">**'In**</span><span class="sxs-lookup"><span data-stu-id="64583-476">**MaxLength**</span></span>  | <span data-ttu-id="64583-477">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-477">No</span></span>          | <span data-ttu-id="64583-478">Parametrenin uzunluk üst sınırı.</span><span class="sxs-lookup"><span data-stu-id="64583-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="64583-479">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="64583-479">**Precision**</span></span>  | <span data-ttu-id="64583-480">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-480">No</span></span>          | <span data-ttu-id="64583-481">Parametrenin duyarlığı.</span><span class="sxs-lookup"><span data-stu-id="64583-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="64583-482">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="64583-482">**Scale**</span></span>      | <span data-ttu-id="64583-483">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-483">No</span></span>          | <span data-ttu-id="64583-484">Parametresinin ölçeği.</span><span class="sxs-lookup"><span data-stu-id="64583-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="64583-485">**SRıD**</span><span class="sxs-lookup"><span data-stu-id="64583-485">**SRID**</span></span>       | <span data-ttu-id="64583-486">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-486">No</span></span>          | <span data-ttu-id="64583-487">Uzamsal sistem başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="64583-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="64583-488">Yalnızca uzamsal türlerin parametreleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="64583-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="64583-489">Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="64583-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-490">**Parametre** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="64583-491">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-492">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-493">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-493">Example</span></span>

<span data-ttu-id="64583-494">Aşağıdaki örnek, giriş parametrelerini belirten iki **parametre** öğesi olan bir **Function** öğesini gösterir:</span><span class="sxs-lookup"><span data-stu-id="64583-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="64583-495">Principal öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="64583-496">Depo şeması tanım dili (SSDL) içindeki **Principal** öğesi, bir yabancı anahtar kısıtlamasının (başvuru kısıtlaması olarak da bilinir) ana bitimi tanımlayan ReferentialConstraint öğesinin bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="64583-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="64583-497">**Principal** öğesi, başka bir sütun (veya sütun) tarafından başvurulan bir tablodaki birincil anahtar sütununu (veya sütunları) belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="64583-498">**Propertyref** öğeleri hangi sütunlara başvurulduğunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="64583-499">Dependent öğesi, **Principal** öğesinde belirtilen birincil anahtar sütunlarına başvuruda bulunan sütunları belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="64583-500">**Principal** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="64583-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="64583-501">PropertyRef (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="64583-502">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-503">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-503">Applicable Attributes</span></span>

<span data-ttu-id="64583-504">Aşağıdaki tabloda, **Principal** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="64583-505">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-505">Attribute Name</span></span> | <span data-ttu-id="64583-506">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-506">Is Required</span></span> | <span data-ttu-id="64583-507">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-508">**Rol**</span><span class="sxs-lookup"><span data-stu-id="64583-508">**Role**</span></span>       | <span data-ttu-id="64583-509">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-509">Yes</span></span>         | <span data-ttu-id="64583-510">Karşılık gelen End öğesinin **rol** özniteliğiyle aynı değer (kullanılıyorsa); Aksi takdirde, başvurulan sütununu içeren tablonun adı.</span><span class="sxs-lookup"><span data-stu-id="64583-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-511">Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **Principal** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="64583-512">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="64583-513">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-514">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-514">Example</span></span>

<span data-ttu-id="64583-515">Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasına katılan sütunları belirtmek Için **ReferentialConstraint** öğesi kullanan bir ilişkilendirme öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="64583-516">**Principal** öğesi, kısıtlamanın asıl sonu olarak **Müşteri** tablosunun **CustomerID** sütununu belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="64583-517">Property öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-517">Property Element (SSDL)</span></span>

<span data-ttu-id="64583-518">Depo şeması tanım dili (SSDL) içindeki **özellik** öğesi, temel alınan veritabanındaki bir tablodaki sütunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="64583-519">**Özellik** öğeleri, bir tablodaki satırları temsil eden EntityType öğelerinin alt öğeleridir.</span><span class="sxs-lookup"><span data-stu-id="64583-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="64583-520">Bir **EntityType** öğesinde tanımlanan her **özellik** öğesi bir sütunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="64583-521">Bir **özellik** öğesinin herhangi bir alt öğesi olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-522">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-522">Applicable Attributes</span></span>

<span data-ttu-id="64583-523">Aşağıdaki tabloda, **özellik** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="64583-524">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-524">Attribute Name</span></span>            | <span data-ttu-id="64583-525">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-525">Is Required</span></span> | <span data-ttu-id="64583-526">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-527">**Ad**</span><span class="sxs-lookup"><span data-stu-id="64583-527">**Name**</span></span>                  | <span data-ttu-id="64583-528">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-528">Yes</span></span>         | <span data-ttu-id="64583-529">Karşılık gelen sütunun adı.</span><span class="sxs-lookup"><span data-stu-id="64583-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="64583-530">**Tür**</span><span class="sxs-lookup"><span data-stu-id="64583-530">**Type**</span></span>                  | <span data-ttu-id="64583-531">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-531">Yes</span></span>         | <span data-ttu-id="64583-532">Karşılık gelen sütunun türü.</span><span class="sxs-lookup"><span data-stu-id="64583-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="64583-533">**Yapılamaz**</span><span class="sxs-lookup"><span data-stu-id="64583-533">**Nullable**</span></span>              | <span data-ttu-id="64583-534">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-534">No</span></span>          | <span data-ttu-id="64583-535">**True** (varsayılan değer) veya false değeri, karşılık gelen sütunun null değere sahip olup olmadığına bağlı olarak **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="64583-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="64583-536">**Değerinin**</span><span class="sxs-lookup"><span data-stu-id="64583-536">**DefaultValue**</span></span>          | <span data-ttu-id="64583-537">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-537">No</span></span>          | <span data-ttu-id="64583-538">Karşılık gelen sütunun varsayılan değeri.</span><span class="sxs-lookup"><span data-stu-id="64583-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="64583-539">**'In**</span><span class="sxs-lookup"><span data-stu-id="64583-539">**MaxLength**</span></span>             | <span data-ttu-id="64583-540">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-540">No</span></span>          | <span data-ttu-id="64583-541">Karşılık gelen sütunun uzunluk üst sınırı.</span><span class="sxs-lookup"><span data-stu-id="64583-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="64583-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="64583-542">**FixedLength**</span></span>           | <span data-ttu-id="64583-543">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-543">No</span></span>          | <span data-ttu-id="64583-544">Karşılık gelen sütun değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="64583-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="64583-545">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="64583-545">**Precision**</span></span>             | <span data-ttu-id="64583-546">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-546">No</span></span>          | <span data-ttu-id="64583-547">Karşılık gelen sütunun duyarlığı.</span><span class="sxs-lookup"><span data-stu-id="64583-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="64583-548">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="64583-548">**Scale**</span></span>                 | <span data-ttu-id="64583-549">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-549">No</span></span>          | <span data-ttu-id="64583-550">Karşılık gelen sütunun ölçeği.</span><span class="sxs-lookup"><span data-stu-id="64583-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="64583-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="64583-551">**Unicode**</span></span>               | <span data-ttu-id="64583-552">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-552">No</span></span>          | <span data-ttu-id="64583-553">Karşılık gelen sütun değerinin bir Unicode dize olarak saklanıp saklanmayacağı **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="64583-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="64583-554">**Mediğinden**</span><span class="sxs-lookup"><span data-stu-id="64583-554">**Collation**</span></span>             | <span data-ttu-id="64583-555">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-555">No</span></span>          | <span data-ttu-id="64583-556">Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.</span><span class="sxs-lookup"><span data-stu-id="64583-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="64583-557">**SRıD**</span><span class="sxs-lookup"><span data-stu-id="64583-557">**SRID**</span></span>                  | <span data-ttu-id="64583-558">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-558">No</span></span>          | <span data-ttu-id="64583-559">Uzamsal sistem başvuru tanımlayıcısı.</span><span class="sxs-lookup"><span data-stu-id="64583-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="64583-560">Yalnızca uzamsal türlerin özellikleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="64583-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="64583-561">Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="64583-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="64583-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="64583-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="64583-563">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-563">No</span></span>          | <span data-ttu-id="64583-564">**Hiçbiri**, **kimlik** (karşılık gelen sütun değeri veritabanında oluşturulan bir kimlik ise) veya **hesaplanmışsa** (karşılık gelen sütun değeri veritabanında hesaplanmışsa).</span><span class="sxs-lookup"><span data-stu-id="64583-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="64583-565">RowType özellikleri için geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="64583-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-566">**Özellik** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="64583-567">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-568">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-569">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-569">Example</span></span>

<span data-ttu-id="64583-570">Aşağıdaki örnek, iki alt **özellik** öğesi olan bir **EntityType** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="64583-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="64583-571">PropertyRef öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="64583-572">Depo şeması tanım dili (SSDL) içindeki **Propertyref** öğesi, özelliğin aşağıdaki rollerden birini gerçekleştireceğini göstermek Için bir EntityType öğesinde tanımlanan bir özelliğe başvurur:</span><span class="sxs-lookup"><span data-stu-id="64583-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="64583-573">**EntityType** 'ın temsil ettiği tablonun birincil anahtarının bir parçası olun.</span><span class="sxs-lookup"><span data-stu-id="64583-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="64583-574">Bir veya daha fazla **Propertyref** öğesi, birincil anahtar tanımlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="64583-575">Daha fazla bilgi için bkz. Key öğesi.</span><span class="sxs-lookup"><span data-stu-id="64583-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="64583-576">Bir başvuru kısıtlamasının bağımlı veya birincil sonu olun.</span><span class="sxs-lookup"><span data-stu-id="64583-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="64583-577">Daha fazla bilgi için bkz. ReferentialConstraint öğesi.</span><span class="sxs-lookup"><span data-stu-id="64583-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="64583-578">**Propertyref** öğesi yalnızca aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="64583-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="64583-579">Belgeler (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="64583-580">Ek açıklama öğeleri</span><span class="sxs-lookup"><span data-stu-id="64583-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-581">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-581">Applicable Attributes</span></span>

<span data-ttu-id="64583-582">Aşağıdaki tabloda, **Propertyref** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="64583-583">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-583">Attribute Name</span></span> | <span data-ttu-id="64583-584">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-584">Is Required</span></span> | <span data-ttu-id="64583-585">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="64583-586">**Ad**</span><span class="sxs-lookup"><span data-stu-id="64583-586">**Name**</span></span>       | <span data-ttu-id="64583-587">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-587">Yes</span></span>         | <span data-ttu-id="64583-588">Başvurulan özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="64583-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="64583-589">**Propertyref** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="64583-590">Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="64583-591">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-592">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-592">Example</span></span>

<span data-ttu-id="64583-593">Aşağıdaki örnek, bir **EntityType** öğesinde tanımlanan bir özelliğe başvurarak birincil anahtar tanımlamak için kullanılan bir **propertyref** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="64583-594">ReferentialConstraint öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="64583-595">Depo şeması tanım dili (SSDL) içindeki **ReferentialConstraint** öğesi, temel veritabanında bir yabancı anahtar kısıtlamasını (başvurusal bütünlük kısıtlaması olarak da bilinir) temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="64583-596">Kısıtlamanın asıl ve bağımlı uçları, sırasıyla asıl ve bağımlı alt öğeler tarafından belirtilir.</span><span class="sxs-lookup"><span data-stu-id="64583-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="64583-597">Principal ve Dependent erimine katılan sütunlara PropertyRef öğeleriyle başvurulur.</span><span class="sxs-lookup"><span data-stu-id="64583-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="64583-598">**ReferentialConstraint** öğesi Association öğesinin isteğe bağlı bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="64583-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="64583-599">**İlişki** öğesinde belirtilen yabancı anahtar kısıtlamasını eşlemek Için bir **ReferentialConstraint** öğesi kullanılmazsa, bunu yapmak için bir associationsetmapping öğesi kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="64583-600">**ReferentialConstraint** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="64583-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="64583-601">Belgeler (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="64583-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="64583-602">Asıl (tam olarak bir)</span><span class="sxs-lookup"><span data-stu-id="64583-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="64583-603">Bağımlı (tam olarak bir)</span><span class="sxs-lookup"><span data-stu-id="64583-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="64583-604">Ek açıklama öğeleri (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-605">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-605">Applicable Attributes</span></span>

<span data-ttu-id="64583-606">**ReferentialConstraint** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="64583-607">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-608">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-609">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-609">Example</span></span>

<span data-ttu-id="64583-610">Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasına katılan sütunları belirtmek Için **ReferentialConstraint** öğesi kullanan bir **ilişkilendirme** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="64583-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="64583-611">ReturnType öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="64583-612">Depo şeması tanım dili (SSDL) içindeki **ReturnType** öğesi, bir **işlev** öğesinde tanımlanan bir işlev için dönüş türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="64583-613">Bir işlev dönüş türü, bir **ReturnType** özniteliğiyle de belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="64583-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="64583-614">İşlevin dönüş türü, **tür** özniteliği veya **ReturnType** öğesiyle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="64583-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="64583-615">**ReturnType** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="64583-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="64583-616">CollectionType (bir)</span><span class="sxs-lookup"><span data-stu-id="64583-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="64583-617">Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **ReturnType** öğesine uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="64583-618">Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="64583-619">İki özel öznitelik için tam nitelikli adlar aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="64583-620">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-620">Example</span></span>

<span data-ttu-id="64583-621">Aşağıdaki örnek, bir satır koleksiyonu döndüren bir **işlevi** kullanır.</span><span class="sxs-lookup"><span data-stu-id="64583-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="64583-622">RowType öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="64583-623">Depo şeması tanım dili (SSDL) içindeki **RowType** öğesi, depoda tanımlanan bir işlev için dönüş türü olarak adlandırılmamış bir yapı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="64583-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="64583-624">**RowType** öğesi, **CollectionType** öğesinin alt öğesidir:</span><span class="sxs-lookup"><span data-stu-id="64583-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="64583-625">**RowType** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="64583-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="64583-626">Özellik (bir veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="64583-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="64583-627">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-627">Example</span></span>

<span data-ttu-id="64583-628">Aşağıdaki örnek, işlevin satır koleksiyonunu ( **RowType** öğesinde belirtilen şekilde) döndürdüğünü belirtmek Için bir **CollectionType** öğesi kullanan bir saklama işlevi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="64583-629">Schema öğesi (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="64583-630">Depo şeması tanım dili (SSDL) içindeki **şema** öğesi, bir depolama modeli tanımının kök öğesidir.</span><span class="sxs-lookup"><span data-stu-id="64583-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="64583-631">Bir depolama modeli oluşturan nesneler, işlevler ve kapsayıcılar için tanımlar içerir.</span><span class="sxs-lookup"><span data-stu-id="64583-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="64583-632">**Şema** öğesi aşağıdaki alt öğeleri sıfır veya daha fazla içerebilir:</span><span class="sxs-lookup"><span data-stu-id="64583-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="64583-633">Kaldırma</span><span class="sxs-lookup"><span data-stu-id="64583-633">Association</span></span>
-   <span data-ttu-id="64583-634">entityType</span><span class="sxs-lookup"><span data-stu-id="64583-634">EntityType</span></span>
-   <span data-ttu-id="64583-635">'Indaki</span><span class="sxs-lookup"><span data-stu-id="64583-635">EntityContainer</span></span>
-   <span data-ttu-id="64583-636">İşlev</span><span class="sxs-lookup"><span data-stu-id="64583-636">Function</span></span>

<span data-ttu-id="64583-637">**Şema** öğesi, bir depolama modelindeki varlık türü ve ilişkilendirme nesneleri için ad alanını tanımlamak üzere **Namespace** özniteliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="64583-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="64583-638">Bir ad alanı içinde, iki nesne aynı ada sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="64583-639">Depolama modeli ad alanı, **şema** öğesinin XML ad alanından farklıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="64583-640">Bir depolama modeli ad alanı ( **ad alanı** özniteliğiyle tanımlandığı gibi) varlık türleri ve ilişkilendirme türleri için bir mantıksal kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="64583-641">Bir **şema** öğesinin XML ad alanı ( **xmlns** özniteliğiyle gösterilir), alt öğeler ve **şema** öğesinin öznitelikleri için varsayılan ad alanıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="64583-642">Form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl XML ad alanları (YYYY ve MM, sırasıyla bir yılı ve ayı temsil eder) SSDL için ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="64583-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="64583-643">Özel öğeler ve öznitelikler bu forma sahip ad alanlarında olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="64583-644">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="64583-644">Applicable Attributes</span></span>

<span data-ttu-id="64583-645">Aşağıdaki tabloda, özniteliklerin **şema** öğesine uygulanabileceğini açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="64583-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="64583-646">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="64583-646">Attribute Name</span></span>            | <span data-ttu-id="64583-647">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="64583-647">Is Required</span></span> | <span data-ttu-id="64583-648">Değer</span><span class="sxs-lookup"><span data-stu-id="64583-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="64583-649">**Namespace**</span></span>             | <span data-ttu-id="64583-650">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-650">Yes</span></span>         | <span data-ttu-id="64583-651">Depolama modelinin ad alanı.</span><span class="sxs-lookup"><span data-stu-id="64583-651">The namespace of the storage model.</span></span> <span data-ttu-id="64583-652">**Ad alanı** özniteliğinin değeri, bir türün tam nitelikli adını biçimlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="64583-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="64583-653">Örneğin, *Müşteri* adlı bir **EntityType** , ExampleModel. Store ad alanında ise, **EntityType** 'ın tam adı örnek model. Store. Customer olur.</span><span class="sxs-lookup"><span data-stu-id="64583-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="64583-654">Şu dizeler **ad alanı** özniteliği değeri olarak kullanılamaz: **System**, **geçici**veya **EDM**.</span><span class="sxs-lookup"><span data-stu-id="64583-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="64583-655">**Ad alanı** özniteliği DEĞERI, csdl şeması öğesindeki **Namespace** özniteliğinin değeri ile aynı olamaz.</span><span class="sxs-lookup"><span data-stu-id="64583-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="64583-656">**Diğer ad**</span><span class="sxs-lookup"><span data-stu-id="64583-656">**Alias**</span></span>                 | <span data-ttu-id="64583-657">Hayır</span><span class="sxs-lookup"><span data-stu-id="64583-657">No</span></span>          | <span data-ttu-id="64583-658">Ad alanı adı yerine kullanılan tanımlayıcı.</span><span class="sxs-lookup"><span data-stu-id="64583-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="64583-659">Örneğin, *Müşteri* adlı bir **EntityType** , ExampleModel ' de yer alıyorsa. Depo ad alanı ve **diğer ad** özniteliğinin değeri *Storagemodel*ise storagemodel. Customer öğesini EntityType 'ın tam adı olarak kullanabilirsiniz **.**</span><span class="sxs-lookup"><span data-stu-id="64583-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="64583-660">**Sağlayıcı**</span><span class="sxs-lookup"><span data-stu-id="64583-660">**Provider**</span></span>              | <span data-ttu-id="64583-661">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-661">Yes</span></span>         | <span data-ttu-id="64583-662">Veri sağlayıcı.</span><span class="sxs-lookup"><span data-stu-id="64583-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="64583-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="64583-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="64583-664">Evet</span><span class="sxs-lookup"><span data-stu-id="64583-664">Yes</span></span>         | <span data-ttu-id="64583-665">Sağlayıcıya döndürülecek sağlayıcı bildirimini gösteren bir belirteç.</span><span class="sxs-lookup"><span data-stu-id="64583-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="64583-666">Belirteç için biçim tanımlanmadı.</span><span class="sxs-lookup"><span data-stu-id="64583-666">No format for the token is defined.</span></span> <span data-ttu-id="64583-667">Belirtecin değerleri sağlayıcı tarafından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="64583-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="64583-668">SQL Server sağlayıcısı bildirim belirteçleri hakkında daha fazla bilgi için bkz. SqlClient Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="64583-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="64583-669">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-669">Example</span></span>

<span data-ttu-id="64583-670">Aşağıdaki örnek, bir **EntityContainer** öğesi, iki **EntityType** öğesi ve bir **ilişkilendirme** öğesi içeren bir **şema** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

## <a name="annotation-attributes"></a><span data-ttu-id="64583-671">Ek açıklama öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="64583-671">Annotation Attributes</span></span>

<span data-ttu-id="64583-672">Depo şeması tanım dili (SSDL) içindeki ek açıklama öznitelikleri, depolama modelindeki öğeler hakkında ek meta veriler sağlayan depolama modelinde özel XML öznitelikleridir.</span><span class="sxs-lookup"><span data-stu-id="64583-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="64583-673">Geçerli XML yapısına sahip olmanın yanı sıra, ek açıklama öznitelikleri için aşağıdaki kısıtlamalar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="64583-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="64583-674">Ek açıklama öznitelikleri, SSDL için ayrılan XML ad alanı içinde olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="64583-675">İki ek açıklama özniteliğinin tam nitelikli adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="64583-676">Verilen bir SSDL öğesine birden çok ek açıklama özniteliği uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="64583-677">Ek açıklama öğelerinde içerilen meta verilere, System. Data. Metadata. Edm ad alanındaki sınıflar kullanılarak çalışma zamanında erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="64583-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="64583-678">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-678">Example</span></span>

<span data-ttu-id="64583-679">Aşağıdaki örnek, **OrderID** özelliğine uygulanan bir Annotation özniteliği olan bir EntityType öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="64583-680">Örnek ayrıca **EntityType** öğesine eklenen bir ek açıklama öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="64583-681">Ek açıklama öğeleri (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="64583-682">Depo şeması tanım dili (SSDL) içindeki ek açıklama öğeleri depolama modeli hakkında ek meta veriler sağlayan depolama modelinde özel XML öğeleridir.</span><span class="sxs-lookup"><span data-stu-id="64583-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="64583-683">Geçerli XML yapısına ek olarak, aşağıdaki kısıtlamalar ek açıklama öğeleri için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="64583-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="64583-684">Ek açıklama öğeleri, SSDL için ayrılan XML ad alanı içinde olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="64583-685">İki ek açıklama öğesinin tam nitelikli adları aynı olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="64583-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="64583-686">Ek açıklama öğeleri, belirli bir SSDL öğesinin tüm diğer alt öğelerinden sonra gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="64583-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="64583-687">Birden fazla ek açıklama öğesi, verili bir SSDL öğesinin alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="64583-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="64583-688">.NET Framework sürüm 4 ' te başlayarak, ek açıklama öğelerinde içerilen meta verilere System. Data. Metadata. Edm ad alanındaki sınıflar kullanılarak çalışma zamanında erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="64583-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="64583-689">Örnek</span><span class="sxs-lookup"><span data-stu-id="64583-689">Example</span></span>

<span data-ttu-id="64583-690">Aşağıdaki örnek, bir Annotation öğesi (**CustomElement**) olan bir EntityType öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="64583-691">Örnek, **OrderID** özelliğine uygulanan bir ek açıklama özniteliği de gösterir.</span><span class="sxs-lookup"><span data-stu-id="64583-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="64583-692">Modeller (SSDL)</span><span class="sxs-lookup"><span data-stu-id="64583-692">Facets (SSDL)</span></span>

<span data-ttu-id="64583-693">Mağaza şeması tanım dili (SSDL) içindeki modeller Özellik öğelerinde belirtilen sütun türlerinde kısıtlamaları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="64583-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="64583-694">**Özellikler, özellik** öğelerinde XML öznitelikleri olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="64583-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="64583-695">Aşağıdaki tabloda, SSDL içinde desteklenen modeller açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="64583-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="64583-696">Kısıtlayan</span><span class="sxs-lookup"><span data-stu-id="64583-696">Facet</span></span>           | <span data-ttu-id="64583-697">Açıklama</span><span class="sxs-lookup"><span data-stu-id="64583-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="64583-698">**Mediğinden**</span><span class="sxs-lookup"><span data-stu-id="64583-698">**Collation**</span></span>   | <span data-ttu-id="64583-699">Özelliğin değerlerinde karşılaştırma ve sıralama işlemleri gerçekleştirirken kullanılacak harmanlama sırasını (veya sıralama sırasını) belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="64583-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="64583-700">**FixedLength**</span></span> | <span data-ttu-id="64583-701">Sütun değerinin uzunluğunun değişebileceğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="64583-702">**'In**</span><span class="sxs-lookup"><span data-stu-id="64583-702">**MaxLength**</span></span>   | <span data-ttu-id="64583-703">Sütun değerinin uzunluk üst sınırını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="64583-704">**Duyarlılık**</span><span class="sxs-lookup"><span data-stu-id="64583-704">**Precision**</span></span>   | <span data-ttu-id="64583-705">**Decimal**türü özellikler için, bir özellik değerinin sahip olduğu basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="64583-706">**Time**, **DateTime**ve **DateTimeOffset**türündeki özellikler için, sütun değeri saniyelik kesirli kısmının basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="64583-707">**Ölçek**</span><span class="sxs-lookup"><span data-stu-id="64583-707">**Scale**</span></span>       | <span data-ttu-id="64583-708">Sütun değeri için ondalık noktanın sağ tarafındaki basamak sayısını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="64583-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="64583-709">**Unicode**</span></span>     | <span data-ttu-id="64583-710">Sütun değerinin Unicode olarak depolandığını belirtir.</span><span class="sxs-lookup"><span data-stu-id="64583-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
