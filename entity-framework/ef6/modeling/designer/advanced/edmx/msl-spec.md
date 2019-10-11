---
title: MSL belirtimi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182562"
---
# <a name="msl-specification"></a><span data-ttu-id="fb111-102">MSL Belirtimi</span><span class="sxs-lookup"><span data-stu-id="fb111-102">MSL Specification</span></span>
<span data-ttu-id="fb111-103">Eşleme belirtim dili (MSL), bir Entity Framework uygulamasının kavramsal modeli ve depolama modeli arasındaki eşlemeyi açıklayan XML tabanlı bir dildir.</span><span class="sxs-lookup"><span data-stu-id="fb111-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="fb111-104">Entity Framework uygulamasında, eşleme meta verileri, derleme zamanında bir. msl dosyasından (MSL 'te yazılmıştır) yüklenir.</span><span class="sxs-lookup"><span data-stu-id="fb111-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="fb111-105">Entity Framework, sorguları, verileri depoya özel komutlara dönüştürmek için çalışma zamanında eşleme meta verilerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb111-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="fb111-106">Entity Framework Designer (EF Designer), eşleme bilgilerini tasarım zamanında bir. edmx dosyasında depolar.</span><span class="sxs-lookup"><span data-stu-id="fb111-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="fb111-107">Derleme zamanında Entity Desisgner, çalışma zamanında Entity Framework gereken. msl dosyasını oluşturmak için bir. edmx dosyasındaki bilgileri kullanır</span><span class="sxs-lookup"><span data-stu-id="fb111-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="fb111-108">MSL 'de başvurulan tüm kavramsal veya depolama modeli türlerinin adları, ilgili ad alanı adlarıyla nitelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="fb111-109">Kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. [csdl belirtimi](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="fb111-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="fb111-110">Depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz. [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="fb111-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="fb111-111">MSL 'nin sürümleri, XML ad alanları ile farklılaştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="fb111-112">MSL sürümü</span><span class="sxs-lookup"><span data-stu-id="fb111-112">MSL Version</span></span> | <span data-ttu-id="fb111-113">XML ad alanı</span><span class="sxs-lookup"><span data-stu-id="fb111-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="fb111-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="fb111-114">MSL v1</span></span>      | <span data-ttu-id="fb111-115">urn: schemas-microsoft-com: Windows: Storage: eşleme: CS</span><span class="sxs-lookup"><span data-stu-id="fb111-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="fb111-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="fb111-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="fb111-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="fb111-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="fb111-118">Alias öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-118">Alias Element (MSL)</span></span>

<span data-ttu-id="fb111-119">Eşleme belirtim dili (MSL) içindeki **diğer ad** öğesi, kavramsal model ve depolama modeli ad alanları için diğer adları tanımlamak üzere kullanılan mapping öğesinin bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="fb111-120">MSL 'de başvurulan tüm kavramsal veya depolama modeli türlerinin adları, ilgili ad alanı adlarıyla nitelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="fb111-121">Kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. şema öğesi (CSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="fb111-122">Depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz. şema öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="fb111-123">**Alias** öğesi alt öğeleri içeremez.</span><span class="sxs-lookup"><span data-stu-id="fb111-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-124">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-124">Applicable Attributes</span></span>

<span data-ttu-id="fb111-125">Aşağıdaki tabloda, **diğer ad** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="fb111-126">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-126">Attribute Name</span></span> | <span data-ttu-id="fb111-127">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-127">Is Required</span></span> | <span data-ttu-id="fb111-128">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="fb111-129">**Anahtar**</span><span class="sxs-lookup"><span data-stu-id="fb111-129">**Key**</span></span>        | <span data-ttu-id="fb111-130">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-130">Yes</span></span>         | <span data-ttu-id="fb111-131">**Değer** özniteliği tarafından belirtilen ad alanı için diğer ad.</span><span class="sxs-lookup"><span data-stu-id="fb111-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="fb111-132">**Değer**</span><span class="sxs-lookup"><span data-stu-id="fb111-132">**Value**</span></span>      | <span data-ttu-id="fb111-133">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-133">Yes</span></span>         | <span data-ttu-id="fb111-134">**Anahtar** öğesi değerinin bir diğer adı olduğu ad alanı.</span><span class="sxs-lookup"><span data-stu-id="fb111-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="fb111-135">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-135">Example</span></span>

<span data-ttu-id="fb111-136">Aşağıdaki örnek, kavramsal modelde tanımlanan türler için `c` diğer adını tanımlayan bir **diğer ad** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a><span data-ttu-id="fb111-137">AssociationEnd öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="fb111-138">Kavramsal modeldeki bir varlık türünün değiştirme işlevleri, temel alınan veritabanındaki Saklı yordamlarla eşlendiğinde, eşleme belirtim dili (MSL) içindeki **Associationend** öğesi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb111-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="fb111-139">Bir değişiklik saklı yordamı değeri bir ilişkilendirme özelliğinde tutulan bir parametre alırsa, **Associationend** öğesi özellik değerini parametresine Eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="fb111-140">Daha fazla bilgi için aşağıdaki örnekte bakın.</span><span class="sxs-lookup"><span data-stu-id="fb111-140">For more information, see the example below.</span></span>

<span data-ttu-id="fb111-141">Varlık türlerinin değişiklik işlevlerini saklı yordamlara eşleme hakkında daha fazla bilgi için bkz. ModificationFunctionMapping öğesi (MSL) ve Izlenecek yol: Bir varlığı Saklı yordamlarla eşleme.</span><span class="sxs-lookup"><span data-stu-id="fb111-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="fb111-142">**Associationend** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="fb111-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-144">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-144">Applicable Attributes</span></span>

<span data-ttu-id="fb111-145">Aşağıdaki tabloda, **Associationend** öğesi için geçerli olan öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="fb111-146">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-146">Attribute Name</span></span>     | <span data-ttu-id="fb111-147">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-147">Is Required</span></span> | <span data-ttu-id="fb111-148">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="fb111-149">**AssociationSet**</span></span> | <span data-ttu-id="fb111-150">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-150">Yes</span></span>         | <span data-ttu-id="fb111-151">Eşlenmekte olan ilişkilendirmenin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="fb111-152">**From**</span><span class="sxs-lookup"><span data-stu-id="fb111-152">**From**</span></span>           | <span data-ttu-id="fb111-153">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-153">Yes</span></span>         | <span data-ttu-id="fb111-154">Eşlenen ilişkiye karşılık gelen Gezinti özelliğinin **FromRole** özniteliğinin değeri.</span><span class="sxs-lookup"><span data-stu-id="fb111-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="fb111-155">Daha fazla bilgi için bkz. NavigationProperty öğesi (CSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="fb111-156">**To**</span><span class="sxs-lookup"><span data-stu-id="fb111-156">**To**</span></span>             | <span data-ttu-id="fb111-157">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-157">Yes</span></span>         | <span data-ttu-id="fb111-158">Eşlenen ilişkiye karşılık gelen Gezinti özelliğinin **ToRole** özniteliğinin değeri.</span><span class="sxs-lookup"><span data-stu-id="fb111-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="fb111-159">Daha fazla bilgi için bkz. NavigationProperty öğesi (CSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="fb111-160">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-160">Example</span></span>

<span data-ttu-id="fb111-161">Aşağıdaki kavramsal model varlık türünü göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fb111-161">Consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

<span data-ttu-id="fb111-162">Ayrıca aşağıdaki saklı yordamı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fb111-162">Also consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

<span data-ttu-id="fb111-163">@No__t-0 varlığının güncelleştirme işlevini bu saklı yordama eşlemek için, **DepartmentID** parametresine bir değer sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb111-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="fb111-164">@No__t-0 değeri varlık türündeki bir özelliğe karşılık gelmiyor; eşlemesi burada gösterilen bağımsız bir ilişkide bulunur:</span><span class="sxs-lookup"><span data-stu-id="fb111-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

<span data-ttu-id="fb111-165">Aşağıdaki kod, **FK @ no__t-3Kursu @ no__t-4Department** Association 'ın **DepartmentID** özelliğini **updatekurs** saklı yordamına (' ın update Işlevinin) eşlemek için kullanılan **associationend** öğesini gösterir. **Kurs** varlık türü eşlendi):</span><span class="sxs-lookup"><span data-stu-id="fb111-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="fb111-166">AssociationSetMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="fb111-167">Eşleme belirtim dili (MSL) içindeki **Associationsetmapping** öğesi, temel veritabanındaki kavramsal model ve tablo sütunlarındaki bir ilişki arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="fb111-168">Kavramsal modeldeki ilişkilendirmeler, özellikleri temel veritabanında birincil ve yabancı anahtar sütunlarını temsil eden türlerdir.</span><span class="sxs-lookup"><span data-stu-id="fb111-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="fb111-169">**Associationsetmapping** öğesi, veritabanındaki ilişkilendirme türü özellikleri ve sütunları arasındaki eşlemeleri tanımlamak Için Iki endproperty öğesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="fb111-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="fb111-170">Koşul öğesiyle bu eşlemelere koşullar yerleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb111-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="fb111-171">Veritabanında bulunan saklı yordamlara ilişkiler için INSERT, Update ve DELETE işlevlerini, ModificationFunctionMapping öğesiyle eşleyin.</span><span class="sxs-lookup"><span data-stu-id="fb111-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="fb111-172">QueryView öğesinde bir Entity SQL dizesi kullanarak ilişkilendirmeler ve tablo sütunları arasında salt okunurdur eşlemeler tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="fb111-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-173">Kavramsal modeldeki bir ilişki için bir başvuru kısıtlaması tanımlanmışsa, ilişkilendirmenin bir **Associationsetmapping** öğesiyle eşlenmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fb111-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="fb111-174">Başvuru kısıtlaması olan bir ilişki için bir **Associationsetmapping** öğesi varsa, **associationsetmapping** öğesinde tanımlanan eşlemeler yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="fb111-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="fb111-175">Daha fazla bilgi için bkz. ReferentialConstraint öğesi (CSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="fb111-176">**Associationsetmapping** öğesi aşağıdaki alt öğelere sahip olabilir</span><span class="sxs-lookup"><span data-stu-id="fb111-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="fb111-177">QueryView (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="fb111-178">EndProperty (sıfır veya iki)</span><span class="sxs-lookup"><span data-stu-id="fb111-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="fb111-179">Koşul (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="fb111-180">ModificationFunctionMapping (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-181">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-181">Applicable Attributes</span></span>

<span data-ttu-id="fb111-182">Aşağıdaki tabloda, **Associationsetmapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="fb111-183">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-183">Attribute Name</span></span>     | <span data-ttu-id="fb111-184">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-184">Is Required</span></span> | <span data-ttu-id="fb111-185">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb111-186">**Name**</span></span>           | <span data-ttu-id="fb111-187">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-187">Yes</span></span>         | <span data-ttu-id="fb111-188">Eşlenmekte olan kavramsal model ilişkilendirme kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="fb111-189">**'Ta**</span><span class="sxs-lookup"><span data-stu-id="fb111-189">**TypeName**</span></span>       | <span data-ttu-id="fb111-190">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-190">No</span></span>          | <span data-ttu-id="fb111-191">Eşlenmekte olan kavramsal model ilişki türünün ad alanı nitelenmiş adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="fb111-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="fb111-192">**StoreEntitySet**</span></span> | <span data-ttu-id="fb111-193">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-193">No</span></span>          | <span data-ttu-id="fb111-194">Eşlenmekte olan tablonun adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="fb111-195">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-195">Example</span></span>

<span data-ttu-id="fb111-196">Aşağıdaki örnek, kavramsal modelde **FK @ no__t-2Kursu @ no__t-3Department** Association 'ın veritabanındaki **Kurs** tablosuna eşlendiği bir **associationsetmapping** öğesini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="fb111-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="fb111-197">İlişki türü özellikleri ve tablo sütunları arasındaki eşlemeler alt **Endproperty** öğelerinde belirtilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

## <a name="complexproperty-element-msl"></a><span data-ttu-id="fb111-198">ComplexProperty öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="fb111-199">Eşleme belirtim dili (MSL) içindeki bir **ComplexProperty** öğesi, bir kavramsal model varlık türü ve temel alınan veritabanındaki tablo sütunlarındaki karmaşık tür özelliği arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="fb111-200">Özellik sütun eşlemeleri alt ScalarProperty öğelerinde belirtilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="fb111-201">**ComplexType** özelliği öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-202">ScalarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="fb111-203">**Complexözelliği** (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="fb111-204">ComplextTypeMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="fb111-205">Koşul (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-206">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-206">Applicable Attributes</span></span>

<span data-ttu-id="fb111-207">Aşağıdaki tabloda, **ComplexProperty** öğesi için geçerli olan öznitelikler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="fb111-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="fb111-208">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-208">Attribute Name</span></span> | <span data-ttu-id="fb111-209">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-209">Is Required</span></span> | <span data-ttu-id="fb111-210">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb111-211">**Name**</span></span>       | <span data-ttu-id="fb111-212">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-212">Yes</span></span>         | <span data-ttu-id="fb111-213">Eşlenmekte olan kavramsal modeldeki bir varlık türünün karmaşık özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="fb111-214">**'Ta**</span><span class="sxs-lookup"><span data-stu-id="fb111-214">**TypeName**</span></span>   | <span data-ttu-id="fb111-215">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-215">No</span></span>          | <span data-ttu-id="fb111-216">Kavramsal model özelliği türünün ad alanı nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="fb111-217">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-217">Example</span></span>

<span data-ttu-id="fb111-218">Aşağıdaki örnek, okul modelini temel alır.</span><span class="sxs-lookup"><span data-stu-id="fb111-218">The following example is based on the School model.</span></span> <span data-ttu-id="fb111-219">Kavramsal modele aşağıdaki karmaşık tür eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="fb111-219">The following complex type has been added to the conceptual model:</span></span>

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

<span data-ttu-id="fb111-220">**Kişi** varlık türünün **LastName** ve **FirstName** özellikleri, bir karmaşık özellikle değiştirildi, **ad**:</span><span class="sxs-lookup"><span data-stu-id="fb111-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

<span data-ttu-id="fb111-221">Aşağıdaki MSL, **ad** özelliğini temel alınan veritabanındaki sütunlara eşlemek Için kullanılan **ComplexProperty** öğesini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fb111-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="fb111-222">ComplexTypeMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="fb111-223">Eşleme belirtim dili (MSL) içindeki **Complextypemapping** öğesi, resultmapping öğesinin bir alt öğesidir ve aşağıdaki durumlarda temel alınan veritabanındaki bir işlev içeri aktarma ile kavramsal modeldeki bir saklı yordam arasındaki eşlemeyi tanımlar doğru:</span><span class="sxs-lookup"><span data-stu-id="fb111-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="fb111-224">İşlev içeri aktarma kavramsal bir karmaşık tür döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="fb111-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="fb111-225">Saklı yordam tarafından döndürülen sütunların adları, karmaşık türdeki özelliklerin adlarıyla tam olarak eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="fb111-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="fb111-226">Varsayılan olarak, bir saklı yordam tarafından döndürülen sütunlar ve karmaşık bir tür arasındaki eşleme, sütun ve özellik adlarını temel alır.</span><span class="sxs-lookup"><span data-stu-id="fb111-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="fb111-227">Sütun adları tam olarak özellik adlarıyla eşleşmezse, eşlemeyi tanımlamak için **Complextypemapping** öğesini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb111-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="fb111-228">Varsayılan eşlemenin bir örneği için bkz. FunctionImportMapping öğesi (MSL).</span><span class="sxs-lookup"><span data-stu-id="fb111-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="fb111-229">**Complextypemapping** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-230">ScalarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-231">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-231">Applicable Attributes</span></span>

<span data-ttu-id="fb111-232">Aşağıdaki tabloda, **Complextypemapping** öğesi için geçerli olan öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="fb111-233">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-233">Attribute Name</span></span> | <span data-ttu-id="fb111-234">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-234">Is Required</span></span> | <span data-ttu-id="fb111-235">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="fb111-236">**'Ta**</span><span class="sxs-lookup"><span data-stu-id="fb111-236">**TypeName**</span></span>   | <span data-ttu-id="fb111-237">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-237">Yes</span></span>         | <span data-ttu-id="fb111-238">Eşlenmekte olan karmaşık türün ad alanı nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="fb111-239">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-239">Example</span></span>

<span data-ttu-id="fb111-240">Aşağıdaki saklı yordamı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fb111-240">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="fb111-241">Ayrıca, aşağıdaki kavramsal model karmaşık türünü de göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fb111-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="fb111-242">Önceki karmaşık türün örneklerini döndüren bir işlev içeri aktarması oluşturmak için, saklı yordamın ve varlık türünün döndürdüğü sütunlar arasındaki eşlemenin bir **Complextypemapping** öğesinde tanımlanması gerekir:</span><span class="sxs-lookup"><span data-stu-id="fb111-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a><span data-ttu-id="fb111-243">Condition öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-243">Condition Element (MSL)</span></span>

<span data-ttu-id="fb111-244">Eşleme belirtim dili (MSL) içindeki **koşul** öğesi, kavramsal model ve temel alınan veritabanı arasındaki eşlemelerle ilgili koşullar koyar.</span><span class="sxs-lookup"><span data-stu-id="fb111-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="fb111-245">Bir XML düğümü içinde tanımlanan eşleme, alt **koşul** öğelerinde belirtilen tüm koşullar karşılanıyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="fb111-246">Aksi takdirde, eşleme geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="fb111-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="fb111-247">Örneğin, bir MappingFragment öğesi bir veya daha fazla **Condition** alt öğesi Içeriyorsa, **mappingfragment** düğümü içinde tanımlanan eşleme yalnızca alt **koşul** öğelerinin tüm koşulları karşılanıyorsa geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="fb111-248">Her koşul, bir **ada** ( **ad** özniteliği tarafından belirtilen bir kavramsal model varlığı özelliğinin adı) veya **sütunadı** ( **ColumnName** özniteliği tarafından belirtilen veritabanında bir sütunun adı) uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="fb111-249">**Ad** özniteliği ayarlandığında, koşul bir varlık özelliği değeri ile denetlenir.</span><span class="sxs-lookup"><span data-stu-id="fb111-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="fb111-250">**ColumnName** özniteliği ayarlandığında, koşul bir sütun değerine göre denetlenir.</span><span class="sxs-lookup"><span data-stu-id="fb111-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="fb111-251">**Condition** öğesinde **Name** veya **ColumnName** özniteliğinden yalnızca biri belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-252">**Condition** öğesi bir FunctionImportMapping öğesi içinde kullanıldığında, yalnızca **Name** özniteliği uygulanabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="fb111-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="fb111-253">**Condition** öğesi aşağıdaki öğelerin bir alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="fb111-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="fb111-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="fb111-255">Complexözelliği</span><span class="sxs-lookup"><span data-stu-id="fb111-255">ComplexProperty</span></span>
-   <span data-ttu-id="fb111-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="fb111-256">EntitySetMapping</span></span>
-   <span data-ttu-id="fb111-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="fb111-257">MappingFragment</span></span>
-   <span data-ttu-id="fb111-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="fb111-258">EntityTypeMapping</span></span>

<span data-ttu-id="fb111-259">**Condition** öğesinin hiç alt öğesi olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb111-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-260">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-260">Applicable Attributes</span></span>

<span data-ttu-id="fb111-261">Aşağıdaki tabloda, **koşul** öğesi için geçerli olan öznitelikler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="fb111-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="fb111-262">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-262">Attribute Name</span></span> | <span data-ttu-id="fb111-263">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-263">Is Required</span></span> | <span data-ttu-id="fb111-264">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-265">**Tation**</span><span class="sxs-lookup"><span data-stu-id="fb111-265">**ColumnName**</span></span> | <span data-ttu-id="fb111-266">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-266">No</span></span>          | <span data-ttu-id="fb111-267">Koşulu değerlendirmek için değeri kullanılan tablo sütununun adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="fb111-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="fb111-268">**IsNull**</span></span>     | <span data-ttu-id="fb111-269">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-269">No</span></span>          | <span data-ttu-id="fb111-270">**True** veya **false**.</span><span class="sxs-lookup"><span data-stu-id="fb111-270">**True** or **False**.</span></span> <span data-ttu-id="fb111-271">Değer **true** ise ve sütun değeri **null**ise veya değer **false** ise ve sütun değeri **null**değilse, koşul true olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="fb111-272">Aksi takdirde, koşul false olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="fb111-273">**IsNull** ve **Value** öznitelikleri aynı anda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="fb111-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="fb111-274">**Değer**</span><span class="sxs-lookup"><span data-stu-id="fb111-274">**Value**</span></span>      | <span data-ttu-id="fb111-275">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-275">No</span></span>          | <span data-ttu-id="fb111-276">Sütun değerinin karşılaştırıldığı değer.</span><span class="sxs-lookup"><span data-stu-id="fb111-276">The value with which the column value is compared.</span></span> <span data-ttu-id="fb111-277">Değerler aynıysa, koşul true olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="fb111-278">Aksi takdirde, koşul false olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="fb111-279">**IsNull** ve **Value** öznitelikleri aynı anda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="fb111-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="fb111-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb111-280">**Name**</span></span>       | <span data-ttu-id="fb111-281">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-281">No</span></span>          | <span data-ttu-id="fb111-282">Koşulu değerlendirmek için kullanılan kavramsal model varlığı özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="fb111-283">**Condition** öğesi bir FunctionImportMapping öğesi içinde kullanılıyorsa bu öznitelik geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="fb111-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="fb111-284">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-284">Example</span></span>

<span data-ttu-id="fb111-285">Aşağıdaki örnek, **durum** öğelerini **mappingfragment** öğelerinin alt öğesi olarak gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="fb111-286">**HireDate** null olmadığında ve **kayıttarihi** null olduğunda, veriler **SchoolModel. eğitmen** türü Ile **kişi** tablosunun **PersonID** ve **HireDate** sütunları arasında eşlenir.</span><span class="sxs-lookup"><span data-stu-id="fb111-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="fb111-287">**Kayıttarihi** null olmadığında ve **HireDate** null olduğunda, veriler **SchoolModel. öğrenci** türü Ile **kişi** tablosunun **PersonID** ve **kayıt** sütunları arasında eşlenir.</span><span class="sxs-lookup"><span data-stu-id="fb111-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a><span data-ttu-id="fb111-288">DeleteFunction öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="fb111-289">Eşleme belirtim dili (MSL) içindeki **deletefunction** öğesi, kavramsal modeldeki Delete işlevini, temel veritabanında bulunan bir saklı yordama eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="fb111-290">Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="fb111-291">Daha fazla bilgi için bkz. Işlev öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-292">Bir varlık türünün ekleme, güncelleştirme veya silme işlemlerinin üçünü saklı yordamlara eşleştirmez, çalışma zamanında yürütülürse ve bir UpdateException oluşturulursa eşlenmemiş işlemler başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="fb111-293">EntityTypeMapping 'a uygulanan DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="fb111-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="fb111-294">EntityTypeMapping öğesine uygulandığında, **Deletefunction** öğesi kavramsal modeldeki bir varlık türünün Delete işlevini bir saklı yordama eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="fb111-295">Bir **Entitytypemapping** öğesine uygulandığında **deletefunction** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="fb111-296">AssociationEnd (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="fb111-297">Complexözelliği (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="fb111-298">ScarlarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="fb111-299">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-299">Applicable Attributes</span></span>

<span data-ttu-id="fb111-300">Aşağıdaki tabloda, bir **Entitytypemapping** öğesine uygulandığında **deletefunction** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="fb111-301">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-301">Attribute Name</span></span>            | <span data-ttu-id="fb111-302">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-302">Is Required</span></span> | <span data-ttu-id="fb111-303">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-304">**Ifadelerini**</span><span class="sxs-lookup"><span data-stu-id="fb111-304">**FunctionName**</span></span>          | <span data-ttu-id="fb111-305">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-305">Yes</span></span>         | <span data-ttu-id="fb111-306">Delete işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="fb111-307">Saklı yordam, depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="fb111-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="fb111-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="fb111-309">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-309">No</span></span>          | <span data-ttu-id="fb111-310">Etkilenen satır sayısını döndüren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="fb111-311">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-311">Example</span></span>

<span data-ttu-id="fb111-312">Aşağıdaki örnek, okul modeline dayalıdır ve **kişi** varlık türünün Delete Işlevini **deleteperson** saklı yordamına eşleyen **deletefunction** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="fb111-313">**Deleteperson** saklı yordamı depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="fb111-314">AssociationSetMapping 'e uygulanan DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="fb111-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="fb111-315">AssociationSetMapping öğesine uygulandığında, **Deletefunction** öğesi kavramsal modeldeki bir ilişkinin Delete işlevini bir saklı yordama eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="fb111-316">**Deletefunction** öğesi, **associationsetmapping** öğesine uygulandığında aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="fb111-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="fb111-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="fb111-318">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-318">Applicable Attributes</span></span>

<span data-ttu-id="fb111-319">Aşağıdaki tablo, **Associationsetmapping** öğesine uygulandığında **deletefunction** öğesine uygulanabilen öznitelikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="fb111-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="fb111-320">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-320">Attribute Name</span></span>            | <span data-ttu-id="fb111-321">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-321">Is Required</span></span> | <span data-ttu-id="fb111-322">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-323">**Ifadelerini**</span><span class="sxs-lookup"><span data-stu-id="fb111-323">**FunctionName**</span></span>          | <span data-ttu-id="fb111-324">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-324">Yes</span></span>         | <span data-ttu-id="fb111-325">Delete işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="fb111-326">Saklı yordam, depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="fb111-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="fb111-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="fb111-328">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-328">No</span></span>          | <span data-ttu-id="fb111-329">Etkilenen satır sayısını döndüren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="fb111-330">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-330">Example</span></span>

<span data-ttu-id="fb111-331">Aşağıdaki örnek, okul modeline dayalıdır ve **courseeğitmen** ilişkilendirmesinin Delete Işlevini **deletecourseeğitmen** saklı yordamına eşlemek için kullanılan **deletefunction** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="fb111-332">**Deletecourseeğitmen** saklı yordamı, depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a><span data-ttu-id="fb111-333">EndProperty öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="fb111-334">Eşleme belirtim dili (MSL) içindeki **Endproperty** öğesi, kavramsal model ilişkilendirmesinin bir End veya bir değiştirme işlevi ile temel alınan veritabanı arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="fb111-335">Özellik sütunu eşlemesi bir alt ScalarProperty öğesinde belirtildi.</span><span class="sxs-lookup"><span data-stu-id="fb111-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="fb111-336">Bir **Endproperty** öğesi kavramsal model ilişkisinin sonuna yönelik eşlemeyi tanımlamak için kullanıldığında, bir associationsetmapping öğesinin alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="fb111-337">Bir kavramsal model ilişkisinin değiştirme işlevi için eşlemeyi tanımlamak üzere **Endproperty** öğesi kullanıldığında, bir ınsertfunction öğesinin veya deletefunction öğesinin alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="fb111-338">**Endproperty** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-339">ScalarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-340">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-340">Applicable Attributes</span></span>

<span data-ttu-id="fb111-341">Aşağıdaki tabloda, **Endproperty** öğesi için geçerli olan öznitelikler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="fb111-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="fb111-342">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-342">Attribute Name</span></span> | <span data-ttu-id="fb111-343">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-343">Is Required</span></span> | <span data-ttu-id="fb111-344">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="fb111-345">Name</span><span class="sxs-lookup"><span data-stu-id="fb111-345">Name</span></span>           | <span data-ttu-id="fb111-346">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-346">Yes</span></span>         | <span data-ttu-id="fb111-347">Eşlenmekte olan ilişki ucunun adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="fb111-348">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-348">Example</span></span>

<span data-ttu-id="fb111-349">Aşağıdaki örnek, kavramsal modeldeki **FK @ no__t-2Kursu @ no__t-3Department** Association öğesinin veritabanındaki **Kurs** tablosuyla eşlendiği bir **associationsetmapping** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="fb111-350">İlişki türü özellikleri ve tablo sütunları arasındaki eşlemeler alt **Endproperty** öğelerinde belirtilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a><span data-ttu-id="fb111-351">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-351">Example</span></span>

<span data-ttu-id="fb111-352">Aşağıdaki örnek, bir ilişkilendirmenin (**Courseeğitmen**) INSERT ve DELETE işlevlerini temel alınan veritabanındaki saklı yordamlara eşleyen **endproperty** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="fb111-353">İle eşlenen işlevler depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-353">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="fb111-354">EntityContainerMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="fb111-355">Eşleme belirtim dili (MSL) içindeki **Entitycontainermapping** öğesi, kavramsal modeldeki varlık kapsayıcısını depolama modelindeki varlık kapsayıcısına eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="fb111-356">**Entitycontainermapping** öğesi, Mapping öğesinin bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="fb111-357">**Entitycontainermapping** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb111-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb111-358">EntitySetMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="fb111-359">AssociationSetMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="fb111-360">FunctionImportMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-361">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-361">Applicable Attributes</span></span>

<span data-ttu-id="fb111-362">Aşağıdaki tabloda **Entitycontainermapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="fb111-363">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-363">Attribute Name</span></span>            | <span data-ttu-id="fb111-364">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-364">Is Required</span></span> | <span data-ttu-id="fb111-365">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="fb111-366">**StorageModelContainer**</span></span> | <span data-ttu-id="fb111-367">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-367">Yes</span></span>         | <span data-ttu-id="fb111-368">Eşlenmekte olan depolama modeli varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="fb111-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="fb111-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="fb111-370">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-370">Yes</span></span>         | <span data-ttu-id="fb111-371">Eşlenmekte olan kavramsal model varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="fb111-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="fb111-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="fb111-373">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-373">No</span></span>          | <span data-ttu-id="fb111-374">**True** veya **false**.</span><span class="sxs-lookup"><span data-stu-id="fb111-374">**True** or **False**.</span></span> <span data-ttu-id="fb111-375">**Yanlışsa**, güncelleştirme görünümleri oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="fb111-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="fb111-376">Veriler başarıyla geri dönüş yaptığından, geçersiz olabilecek bir salt okuma eşlemi varsa, bu öznitelik **false** olarak ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb111-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="fb111-377">Varsayılan değer **true**'dur.</span><span class="sxs-lookup"><span data-stu-id="fb111-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="fb111-378">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-378">Example</span></span>

<span data-ttu-id="fb111-379">Aşağıdaki örnek, **SchoolModelEntities** kapsayıcısını (kavramsal model varlık kapsayıcısı) **SchoolModelStoreContainer** kapsayıcısına (depolama modeli varlığı) eşleyen bir **entitycontainermapping** öğesini gösterir kapsayıcı):</span><span class="sxs-lookup"><span data-stu-id="fb111-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="fb111-380">EntitySetMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="fb111-381">Eşleme belirtim dili (MSL) içindeki **EntitySetMapping** öğesi, bir kavramsal model varlığındaki tüm türleri, depolama modelindeki varlık kümelerine göre eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="fb111-382">Kavramsal modeldeki bir varlık, aynı türde (ve türetilmiş türler) varlık örnekleri için mantıksal bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="fb111-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="fb111-383">Depolama modelinde ayarlanan bir varlık, temel alınan veritabanında bir tabloyu veya görünümü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="fb111-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="fb111-384">Kavramsal model varlık kümesi, **EntitySetMapping** öğesinin **Name** özniteliğinin değeri ile belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="fb111-385">Eşlenen tablo veya görünüm her bir alt MappingFragment öğesinde veya **EntitySetMapping** öğesinin kendisinde **storeentityset** özniteliği tarafından belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="fb111-386">**EntitySetMapping** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-387">EntityTypeMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="fb111-388">QueryView (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="fb111-389">MappingFragment (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-390">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-390">Applicable Attributes</span></span>

<span data-ttu-id="fb111-391">Aşağıdaki tabloda **EntitySetMapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="fb111-392">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-392">Attribute Name</span></span>           | <span data-ttu-id="fb111-393">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-393">Is Required</span></span> | <span data-ttu-id="fb111-394">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb111-395">**Name**</span></span>                 | <span data-ttu-id="fb111-396">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-396">Yes</span></span>         | <span data-ttu-id="fb111-397">Eşlenmekte olan kavramsal model varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="fb111-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="fb111-398">**TypeName** **1**</span></span>       | <span data-ttu-id="fb111-399">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-399">No</span></span>          | <span data-ttu-id="fb111-400">Eşlenmekte olan kavramsal model varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="fb111-401">**Storeentityset** **1**</span><span class="sxs-lookup"><span data-stu-id="fb111-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="fb111-402">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-402">No</span></span>          | <span data-ttu-id="fb111-403">Eşlenmekte olan depolama modeli varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="fb111-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="fb111-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="fb111-405">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-405">No</span></span>          | <span data-ttu-id="fb111-406">Yalnızca ayrı satırların döndürülüp döndürülmediğine bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb111-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="fb111-407">Bu öznitelik **true**olarak ayarlanırsa, EntityContainerMapping öğesinin **generateupdateviews** özniteliği **false**olarak ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb111-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="fb111-408">**1** **TypeName** ve **storeentityset** öznitelikleri, tek bir varlık türünü tek bir tabloya eşlemek için entitytypemapping ve mappingfragment alt öğelerinin yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="fb111-409">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-409">Example</span></span>

<span data-ttu-id="fb111-410">Aşağıdaki örnek, kavramsal modelin **Kurslar** varlık kümesindeki üç türü (temel tür ve iki türetilmiş tür) eşleyen bir **EntitySetMapping** öğesini gösterir ve temel alınan veritabanında üç farklı tabloya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fb111-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="fb111-411">Tablolar her **Mappingfragment** öğesinde **storeentityset** özniteliği tarafından belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="fb111-412">EntityTypeMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="fb111-413">Eşleme belirtim dili (MSL) içindeki **Entitytypemapping** öğesi, kavramsal modeldeki bir varlık türü ve temel alınan veritabanındaki tablolar veya görünümler arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="fb111-414">Kavramsal model varlık türleri ve temel alınan veritabanı tabloları ya da görünümleri hakkında daha fazla bilgi için bkz. EntityType öğesi (CSDL) ve EntitySet öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="fb111-415">Eşlenmekte olan kavramsal model varlık türü **Entitytypemapping** öğesinin **TypeName** özniteliğiyle belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="fb111-416">Eşlenmekte olan tablo veya görünüm, alt MappingFragment öğesinin **Storeentityset** özniteliği tarafından belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="fb111-417">ModificationFunctionMapping alt öğesi, varlık türlerinin INSERT, Update veya delete işlevlerini veritabanındaki saklı yordamlara eşlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="fb111-418">**Entitytypemapping** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-419">MappingFragment (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="fb111-420">ModificationFunctionMapping (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="fb111-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="fb111-421">ScalarProperty</span></span>
-   <span data-ttu-id="fb111-422">Koşul</span><span class="sxs-lookup"><span data-stu-id="fb111-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-423">**Mappingfragment** ve **ModificationFunctionMapping** öğeleri aynı anda **entitytypemapping** öğesinin alt öğesi olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb111-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="fb111-424">**Scalarproperty** ve **Condition** öğeleri yalnızca bir FunctionImportMapping öğesi içinde kullanıldığında **entitytypemapping** öğesinin alt öğeleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-425">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-425">Applicable Attributes</span></span>

<span data-ttu-id="fb111-426">Aşağıdaki tabloda **Entitytypemapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="fb111-427">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-427">Attribute Name</span></span> | <span data-ttu-id="fb111-428">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-428">Is Required</span></span> | <span data-ttu-id="fb111-429">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-430">**'Ta**</span><span class="sxs-lookup"><span data-stu-id="fb111-430">**TypeName**</span></span>   | <span data-ttu-id="fb111-431">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-431">Yes</span></span>         | <span data-ttu-id="fb111-432">Eşlenmekte olan kavramsal model varlık türünün ad alanı nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="fb111-433">Tür soyut veya türetilmiş bir tür ise, değer `IsOfType(Namespace-qualified_type_name)` olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb111-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="fb111-434">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-434">Example</span></span>

<span data-ttu-id="fb111-435">Aşağıdaki örnek iki alt **Entitytypemapping** öğesi olan bir EntitySetMapping öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="fb111-436">İlk **Entitytypemapping** öğesinde, **SchoolModel. Person** varlık türü **kişi** tablosuna eşlenir.</span><span class="sxs-lookup"><span data-stu-id="fb111-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="fb111-437">İkinci **Entitytypemapping** öğesinde, **SchoolModel. Person** türünün güncelleştirme işlevselliği veritabanında bir saklı yordam olan **UpdatePerson**ile eşleştirilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="fb111-438">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-438">Example</span></span>

<span data-ttu-id="fb111-439">Sonraki örnek, kök türünün soyut olduğu bir tür hiyerarşisinin eşlemesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="fb111-440">**TypeName** öznitelikleri için `IsOfType` sözdiziminin kullanımını göz önünde edin.</span><span class="sxs-lookup"><span data-stu-id="fb111-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="fb111-441">FunctionImportMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="fb111-442">Eşleme belirtim dili (MSL) içindeki **FunctionImportMapping** öğesi, kavramsal modeldeki bir işlev içeri aktarması ile temel alınan veritabanındaki bir saklı yordam veya işlev arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="fb111-443">İşlev içeri aktarmaları kavramsal modelde bildirilmelidir ve saklı yordamlar depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="fb111-444">Daha fazla bilgi için bkz. FunctionImport öğesi (CSDL) ve Işlev öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-445">Varsayılan olarak, bir işlev içeri aktarma bir kavramsal model varlık türü veya karmaşık tür döndürürse, temel saklı yordam tarafından döndürülen sütunların adları kavramsal model türündeki özelliklerin adlarıyla tam olarak eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="fb111-446">Sütun adları Özellik adlarıyla tam olarak eşleşmiyorsa, eşlemenin bir ResultMapping öğesinde tanımlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb111-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="fb111-447">**FunctionImportMapping** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-448">ResultMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-449">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-449">Applicable Attributes</span></span>

<span data-ttu-id="fb111-450">Aşağıdaki tabloda **FunctionImportMapping** öğesi için geçerli olan öznitelikler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="fb111-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="fb111-451">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-451">Attribute Name</span></span>         | <span data-ttu-id="fb111-452">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-452">Is Required</span></span> | <span data-ttu-id="fb111-453">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-454">**Işleviçeaktarmaadı**</span><span class="sxs-lookup"><span data-stu-id="fb111-454">**FunctionImportName**</span></span> | <span data-ttu-id="fb111-455">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-455">Yes</span></span>         | <span data-ttu-id="fb111-456">Eşlenmekte olan kavramsal modelde işlev içeri aktarma işleminin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="fb111-457">**Ifadelerini**</span><span class="sxs-lookup"><span data-stu-id="fb111-457">**FunctionName**</span></span>       | <span data-ttu-id="fb111-458">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-458">Yes</span></span>         | <span data-ttu-id="fb111-459">Eşlenen depolama modelindeki işlevin ad alanı nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="fb111-460">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-460">Example</span></span>

<span data-ttu-id="fb111-461">Aşağıdaki örnek, okul modelini temel alır.</span><span class="sxs-lookup"><span data-stu-id="fb111-461">The following example is based on the School model.</span></span> <span data-ttu-id="fb111-462">Depolama modelinde aşağıdaki işlevi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fb111-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="fb111-463">Kavramsal modelde bu işlevi içeri aktarmayı da göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fb111-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="fb111-464">Aşağıdaki örnek, yukarıdaki işlev ve işlev içeri aktarmayı birbirlerine eşlemek için kullanılan bir **FunctionImportMapping** öğesi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fb111-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="fb111-465">Insertfunction öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="fb111-466">Eşleme belirtim dili (MSL) içindeki **ınsertfunction** öğesi, kavramsal modeldeki INSERT işlevini temel veritabanındaki bir saklı yordama eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="fb111-467">Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="fb111-468">Daha fazla bilgi için bkz. Işlev öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-469">Bir varlık türünün ekleme, güncelleştirme veya silme işlemlerinin üçünü saklı yordamlara eşleştirmez, çalışma zamanında yürütülürse ve bir UpdateException oluşturulursa eşlenmemiş işlemler başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="fb111-470">**Insertfunction** öğesi ModificationFunctionMapping öğesinin bir alt öğesi olabilir ve entitytypemapping öğesine veya associationsetmapping öğesine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fb111-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="fb111-471">EntityTypeMapping 'a uygulanan ınsertfunction</span><span class="sxs-lookup"><span data-stu-id="fb111-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="fb111-472">EntityTypeMapping öğesine uygulandığında, **ınsertfunction** öğesi kavramsal modeldeki bir varlık türünün INSERT işlevini bir saklı yordama eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="fb111-473">Bir **Entitytypemapping** öğesine uygulandığında **ınsertfunction** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="fb111-474">AssociationEnd (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="fb111-475">Complexözelliği (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="fb111-476">ResultBinding (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="fb111-477">ScarlarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="fb111-478">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-478">Applicable Attributes</span></span>

<span data-ttu-id="fb111-479">Aşağıdaki tabloda, bir **Entitytypemapping** öğesine uygulandığında **ınsertfunction** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="fb111-480">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-480">Attribute Name</span></span>            | <span data-ttu-id="fb111-481">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-481">Is Required</span></span> | <span data-ttu-id="fb111-482">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-483">**Ifadelerini**</span><span class="sxs-lookup"><span data-stu-id="fb111-483">**FunctionName**</span></span>          | <span data-ttu-id="fb111-484">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-484">Yes</span></span>         | <span data-ttu-id="fb111-485">INSERT işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="fb111-486">Saklı yordam, depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="fb111-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="fb111-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="fb111-488">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-488">No</span></span>          | <span data-ttu-id="fb111-489">Etkilenen satır sayısını döndüren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="fb111-490">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-490">Example</span></span>

<span data-ttu-id="fb111-491">Aşağıdaki örnek, okul modeline dayalıdır ve kişi varlık türünün INSERT işlevini **ınsertperson** saklı yordamına eşlemek Için kullanılan **ınsertfunction** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="fb111-492">**Insertperson** saklı yordamı depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="fb111-493">AssociationSetMapping 'e uygulanan ınsertfunction</span><span class="sxs-lookup"><span data-stu-id="fb111-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="fb111-494">AssociationSetMapping öğesine uygulandığında, **ınsertfunction** öğesi kavramsal modeldeki bir ilişkinin INSERT işlevini bir saklı yordama eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="fb111-495">**Insertsetmapping öğesine uygulandığında ınsertfunction** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="fb111-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="fb111-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="fb111-497">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-497">Applicable Attributes</span></span>

<span data-ttu-id="fb111-498">Aşağıdaki tablo, **Associationsetmapping** öğesine uygulandığında **ınsertfunction** öğesine uygulanabilen öznitelikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="fb111-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="fb111-499">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-499">Attribute Name</span></span>            | <span data-ttu-id="fb111-500">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-500">Is Required</span></span> | <span data-ttu-id="fb111-501">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-502">**Ifadelerini**</span><span class="sxs-lookup"><span data-stu-id="fb111-502">**FunctionName**</span></span>          | <span data-ttu-id="fb111-503">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-503">Yes</span></span>         | <span data-ttu-id="fb111-504">INSERT işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="fb111-505">Saklı yordam, depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="fb111-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="fb111-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="fb111-507">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-507">No</span></span>          | <span data-ttu-id="fb111-508">Etkilenen satır sayısını döndüren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="fb111-509">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-509">Example</span></span>

<span data-ttu-id="fb111-510">Aşağıdaki örnek, okul modeline dayalıdır ve **courseeğitmen** ilişkilendirmesinin INSERT Işlevini **ınsertcourseeğitmen** saklı yordamına eşlemek için kullanılan **ınsertfunction** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="fb111-511">**Insertcourseeğitmen** saklı yordamı, depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a><span data-ttu-id="fb111-512">Mapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="fb111-513">Eşleme belirtimi dili (MSL) içindeki **Mapping** öğesi, kavramsal modelde tanımlanan nesneleri bir veritabanına (bir depolama modelinde açıklandığı gibi) eşlemek için bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="fb111-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="fb111-514">Daha fazla bilgi için bkz. CSDL belirtimi ve SSDL belirtimi.</span><span class="sxs-lookup"><span data-stu-id="fb111-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="fb111-515">**Mapping** öğesi, bir eşleme belirtiminin kök öğesidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="fb111-516">Eşleme belirtimleri için XML ad alanı https://schemas.microsoft.com/ado/2009/11/mapping/cs ' dır.</span><span class="sxs-lookup"><span data-stu-id="fb111-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="fb111-517">Mapping öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):</span><span class="sxs-lookup"><span data-stu-id="fb111-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="fb111-518">Diğer ad (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="fb111-519">EntityContainerMapping (tam olarak bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="fb111-520">MSL 'de başvurulan kavramsal ve depolama modeli türlerinin adları, ilgili ad alanı adlarıyla nitelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="fb111-521">Kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. şema öğesi (CSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="fb111-522">Depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz. şema öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="fb111-523">MSL 'de kullanılan ad alanları için diğer adlar diğer ad öğesiyle tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-524">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-524">Applicable Attributes</span></span>

<span data-ttu-id="fb111-525">Aşağıdaki tabloda, **Mapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="fb111-526">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-526">Attribute Name</span></span> | <span data-ttu-id="fb111-527">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-527">Is Required</span></span> | <span data-ttu-id="fb111-528">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="fb111-529">**Boşlu**</span><span class="sxs-lookup"><span data-stu-id="fb111-529">**Space**</span></span>      | <span data-ttu-id="fb111-530">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-530">Yes</span></span>         | <span data-ttu-id="fb111-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="fb111-531">**C-S**.</span></span> <span data-ttu-id="fb111-532">Bu sabit bir değerdir ve değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="fb111-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="fb111-533">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-533">Example</span></span>

<span data-ttu-id="fb111-534">Aşağıdaki örnek, okul modelinin bir bölümünü temel alan bir **Mapping** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="fb111-535">Okul modeli hakkında daha fazla bilgi için bkz. hızlı başlangıç (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="fb111-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="fb111-536">MappingFragment öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="fb111-537">Eşleme belirtim dili (MSL) içindeki **Mappingfragment** öğesi, bir kavramsal model varlık türünün özellikleri ve veritabanındaki bir tablo veya görünüm arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="fb111-538">Kavramsal model varlık türleri ve temel alınan veritabanı tabloları ya da görünümleri hakkında daha fazla bilgi için bkz. EntityType öğesi (CSDL) ve EntitySet öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="fb111-539">**Mappingfragment** entitytypemapping öğesinin ya da EntitySetMapping öğesinin bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="fb111-540">**Mappingfragment** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-541">ComplexType (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="fb111-542">ScalarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="fb111-543">Koşul (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-544">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-544">Applicable Attributes</span></span>

<span data-ttu-id="fb111-545">Aşağıdaki tabloda, **Mappingfragment** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="fb111-546">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-546">Attribute Name</span></span>          | <span data-ttu-id="fb111-547">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-547">Is Required</span></span> | <span data-ttu-id="fb111-548">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="fb111-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="fb111-550">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-550">Yes</span></span>         | <span data-ttu-id="fb111-551">Eşlenmekte olan tablonun veya görünümün adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="fb111-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="fb111-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="fb111-553">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-553">No</span></span>          | <span data-ttu-id="fb111-554">Yalnızca ayrı satırların döndürülüp döndürülmediğine bağlı olarak **doğru** veya **yanlış** .</span><span class="sxs-lookup"><span data-stu-id="fb111-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="fb111-555">Bu öznitelik **true**olarak ayarlanırsa, EntityContainerMapping öğesinin **generateupdateviews** özniteliği **false**olarak ayarlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb111-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="fb111-556">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-556">Example</span></span>

<span data-ttu-id="fb111-557">Aşağıdaki örnek bir **Entitytypemapping** öğesinin alt öğesi olarak bir **mappingfragment** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="fb111-558">Bu örnekte, kavramsal modeldeki **Kurs** türünün özellikleri, veritabanındaki **Kurs** tablosunun sütunlarına eşlenir.</span><span class="sxs-lookup"><span data-stu-id="fb111-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="fb111-559">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-559">Example</span></span>

<span data-ttu-id="fb111-560">Aşağıdaki örnek bir **EntitySetMapping** öğesinin alt öğesi olarak bir **mappingfragment** öğesi gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="fb111-561">Yukarıdaki örnekte olduğu gibi, kavramsal modeldeki **Kurs** türünün özellikleri, veritabanındaki **Kurs** tablosunun sütunlarına eşlenir.</span><span class="sxs-lookup"><span data-stu-id="fb111-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="fb111-562">ModificationFunctionMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="fb111-563">Eşleme belirtim dili (MSL) içindeki **ModificationFunctionMapping** öğesi, kavramsal model varlık türünün INSERT, Update ve DELETE işlevlerini temel alınan veritabanındaki saklı yordamlar olarak eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="fb111-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="fb111-564">**ModificationFunctionMapping** öğesi Ayrıca, kavramsal modeldeki çok-çok İlişkilendirmelerin INSERT ve DELETE işlevlerini temel alınan veritabanındaki Saklı yordamlarla de eşleyebilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="fb111-565">Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="fb111-566">Daha fazla bilgi için bkz. Işlev öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-567">Bir varlık türünün ekleme, güncelleştirme veya silme işlemlerinin üçünü saklı yordamlara eşleştirmez, çalışma zamanında yürütülürse ve bir UpdateException oluşturulursa eşlenmemiş işlemler başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="fb111-568">Bir devralma hiyerarşisindeki bir varlığın değişiklik işlevleri Saklı yordamlarla eşlenmişse, hiyerarşideki tüm türlerin değiştirme işlevlerinin Saklı yordamlarla eşlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb111-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="fb111-569">**ModificationFunctionMapping** öğesi entitytypemapping öğesinin bir alt öğesi veya associationsetmapping öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="fb111-570">**ModificationFunctionMapping** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-571">DeleteFunction (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="fb111-572">Insertfunction (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="fb111-573">UpdateFunction (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="fb111-574">**ModificationFunctionMapping** öğesi için geçerli bir öznitelik yok.</span><span class="sxs-lookup"><span data-stu-id="fb111-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="fb111-575">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-575">Example</span></span>

<span data-ttu-id="fb111-576">Aşağıdaki örnekte, okul modelinde ayarlanan **kişiler** varlığı için varlık kümesi eşleştirmesi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fb111-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="fb111-577">**Kişi** varlık türü için sütun eşlemenin yanı sıra, **kişi** türünün INSERT, Update ve DELETE işlevlerinin eşlemesi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="fb111-578">İle eşlenen işlevler depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-578">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="fb111-579">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-579">Example</span></span>

<span data-ttu-id="fb111-580">Aşağıdaki örnekte, okul modelinde **Kurs** için bir ilişki kümesi eşlemesi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fb111-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="fb111-581">**Courseeğitmen** ilişkilendirmesinin sütun eşlemesine ek olarak, **courseeğitmen** ilişkilendirmesinin INSERT ve DELETE işlevlerinin eşleştirmesi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="fb111-582">İle eşlenen işlevler depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-582">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="fb111-583">QueryView öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="fb111-584">Eşleme belirtim dili (MSL) içindeki **QueryView** öğesi, kavramsal modeldeki bir varlık türü veya ilişkilendirme ile temel alınan veritabanındaki bir tablo arasındaki salt okunurdur eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="fb111-585">Eşleme, depolama modeline göre değerlendirilen bir Entity SQL sorgusuyla tanımlanır ve sonuç kümesini kavramsal modeldeki bir varlık veya ilişkilendirme açısından ifade edersiniz.</span><span class="sxs-lookup"><span data-stu-id="fb111-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="fb111-586">Sorgu görünümleri salt okunurdur, sorgu görünümleri tarafından tanımlanan türleri güncelleştirmek için Standart güncelleştirme komutlarını kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="fb111-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="fb111-587">Değiştirme işlevlerini kullanarak bu türlerde güncelleştirmeler yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb111-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="fb111-588">Daha fazla bilgi için bkz. nasıl yapılır: Değiştirme Işlevlerini Saklı yordamlarla eşleyin.</span><span class="sxs-lookup"><span data-stu-id="fb111-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-589">**QueryView** öğesinde, **GroupBy**, Grup toplamaları veya gezinti özellikleri içeren Entity SQL ifadeleri desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="fb111-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="fb111-590">**QueryView** öğesi EntitySetMapping öğesinin bir alt öğesi ya da associationsetmapping öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="fb111-591">Önceki durumda, sorgu görünümü kavramsal modeldeki bir varlık için salt okunurdur eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="fb111-592">İkinci durumda, sorgu görünümü kavramsal modeldeki bir ilişki için salt okunurdur eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-593">**Associationsetmapping** öğesi başvuru kısıtlaması olan bir ilişkilendirme için Ise, **associationsetmapping** öğesi yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="fb111-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="fb111-594">Daha fazla bilgi için bkz. ReferentialConstraint öğesi (CSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="fb111-595">**QueryView** öğesinin herhangi bir alt öğesi olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb111-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-596">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-596">Applicable Attributes</span></span>

<span data-ttu-id="fb111-597">Aşağıdaki tabloda **QueryView** öğesine uygulanabilen öznitelikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fb111-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="fb111-598">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-598">Attribute Name</span></span> | <span data-ttu-id="fb111-599">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-599">Is Required</span></span> | <span data-ttu-id="fb111-600">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-601">**'Ta**</span><span class="sxs-lookup"><span data-stu-id="fb111-601">**TypeName**</span></span>   | <span data-ttu-id="fb111-602">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-602">No</span></span>          | <span data-ttu-id="fb111-603">Sorgu görünümü tarafından eşlenmekte olan kavramsal model türünün adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="fb111-604">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-604">Example</span></span>

<span data-ttu-id="fb111-605">Aşağıdaki örnek, **QueryView** öğesini **EntitySetMapping** öğesinin bir alt öğesi olarak gösterir ve okul modelindeki **Departman** varlık türü için bir sorgu görünümü eşlemesi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

<span data-ttu-id="fb111-606">Sorgu yalnızca depolama modelindeki **Departman** türünde üyelerin bir alt kümesini döndürdüğünden, okul modelindeki **Departman** türü bu eşlemeye göre aşağıdaki gibi değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fb111-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a><span data-ttu-id="fb111-607">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-607">Example</span></span>

<span data-ttu-id="fb111-608">Sonraki örnek, bir **Associationsetmapping** öğesinin alt öğesi olarak **QueryView** öğesini gösterir ve okul modelinde `FK_Course_Department` ilişkilendirmesi için salt okuma eşlemesini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fb111-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a><span data-ttu-id="fb111-609">Açıklamalar</span><span class="sxs-lookup"><span data-stu-id="fb111-609">Comments</span></span>

<span data-ttu-id="fb111-610">Aşağıdaki senaryoları etkinleştirmek için sorgu görünümleri tanımlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fb111-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="fb111-611">Kavramsal modelde, depolama modelindeki varlığın tüm özelliklerini içermeyen bir varlık tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="fb111-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="fb111-612">Bu, varsayılan değerlere sahip olmayan ve **null** değerleri desteklemeyen özellikler içerir.</span><span class="sxs-lookup"><span data-stu-id="fb111-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="fb111-613">Depolama modelindeki hesaplanan sütunları kavramsal modeldeki varlık türlerinin özelliklerine eşleyin.</span><span class="sxs-lookup"><span data-stu-id="fb111-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="fb111-614">Kavramsal modeldeki varlıkları bölümlemek için kullanılan koşulların eşitlik temelinde olmadığı bir eşleme tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="fb111-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="fb111-615">**Koşul** öğesini kullanarak koşullu eşleme belirttiğinizde, sağlanan koşulun belirtilen değere eşit olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb111-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="fb111-616">Daha fazla bilgi için bkz. koşul öğesi (MSL).</span><span class="sxs-lookup"><span data-stu-id="fb111-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="fb111-617">Depolama modelindeki sütunu, kavramsal modeldeki birden çok türe eşleyin.</span><span class="sxs-lookup"><span data-stu-id="fb111-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="fb111-618">Birden çok türü aynı tabloyla eşleyin.</span><span class="sxs-lookup"><span data-stu-id="fb111-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="fb111-619">Kavramsal modelde ilişkisel şemadaki yabancı anahtarları temel alan ilişkilendirmeler tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="fb111-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="fb111-620">Kavramsal modeldeki özelliklerin değerini ayarlamak için özel iş mantığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="fb111-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="fb111-621">Örneğin, veri kaynağındaki "T" dize değerini, kavramsal modelde bir Boole değeri olan **true**değerine eşleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb111-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="fb111-622">Sorgu sonuçları için koşullu filtreler tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="fb111-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="fb111-623">Kavramsal modeldeki veriler üzerinde depolama modelinden daha az kısıtlama zorlayın.</span><span class="sxs-lookup"><span data-stu-id="fb111-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="fb111-624">Örneğin, bir özelliği, eşlendiği sütun **null**değerleri desteklemediğinden bile kavramsal modelde null yapılabilir hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb111-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="fb111-625">Varlıkların sorgu görünümlerini tanımlarken aşağıdaki noktalar geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="fb111-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="fb111-626">Sorgu görünümleri salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="fb111-626">Query views are read-only.</span></span> <span data-ttu-id="fb111-627">Yalnızca değiştirme işlevlerini kullanarak varlıklara güncelleştirmeler yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb111-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="fb111-628">Bir sorgu görünümü ile bir varlık türü tanımladığınızda, tüm ilgili varlıkları sorgu görünümlerine göre de tanımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb111-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="fb111-629">İlişkisel şemadaki bir bağlantı tablosunu temsil eden depolama modelindeki bir varlıkla çoktan çoğa bir ilişki eşlediğinizde, bu bağlantı tablosunun **Associationsetmapping** öğesinde bir **QueryView** öğesi tanımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb111-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="fb111-630">Sorgu görünümleri bir tür hiyerarşisindeki tüm türler için tanımlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fb111-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="fb111-631">Bunu aşağıdaki yöntemlerle yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fb111-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="fb111-632">Hiyerarşide tüm varlık türlerinin bir birleşimini döndüren tek bir Entity SQL sorgusu belirten tek bir **QueryView** öğesi ile.</span><span class="sxs-lookup"><span data-stu-id="fb111-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="fb111-633">Belirli bir koşula bağlı olarak hiyerarşide belirli bir varlık türünü döndürmek için CASE işlecini kullanan tek bir Entity SQL sorgusu belirten tek bir **QueryView** öğesi ile.</span><span class="sxs-lookup"><span data-stu-id="fb111-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="fb111-634">Hiyerarşide belirli bir tür için ek bir **QueryView** öğesi ile.</span><span class="sxs-lookup"><span data-stu-id="fb111-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="fb111-635">Bu durumda, her bir görünüm için varlık türünü belirtmek üzere **QueryView** öğesinin **TypeName** özniteliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="fb111-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="fb111-636">Bir sorgu görünümü tanımlandığında, **EntitySetMapping** öğesinde **storagesetname** özniteliğini belirtemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb111-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="fb111-637">Bir sorgu görünümü tanımlandığında, **EntitySetMapping**öğesi **özellik** eşlemeleri de içeremez.</span><span class="sxs-lookup"><span data-stu-id="fb111-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="fb111-638">ResultBinding öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="fb111-639">Eşleme belirtimi dili (MSL) içindeki **Resultbinding** öğesi, varlık türü değiştirme işlevleri içindeki saklı yordamlarla eşlendiğinde, saklı yordamlar tarafından döndürülen sütun değerlerini kavramsal modelde varlık özelliklerine eşler. temel alınan veritabanı.</span><span class="sxs-lookup"><span data-stu-id="fb111-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="fb111-640">Örneğin, bir kimlik sütununun değeri bir INSERT saklı yordamı tarafından döndürüldüğünde, **Resultbinding** öğesi döndürülen değeri kavramsal modeldeki bir varlık türü özelliğine eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="fb111-641">**Resultbinding** öğesi, ınsertfunction öğesinin veya updatefunction öğesinin alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="fb111-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="fb111-642">**Resultbinding** öğesinin herhangi bir alt öğesi olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb111-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-643">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-643">Applicable Attributes</span></span>

<span data-ttu-id="fb111-644">Aşağıdaki tabloda, **Resultbinding** öğesi için geçerli olan öznitelikler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="fb111-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="fb111-645">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-645">Attribute Name</span></span> | <span data-ttu-id="fb111-646">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-646">Is Required</span></span> | <span data-ttu-id="fb111-647">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb111-648">**Name**</span></span>       | <span data-ttu-id="fb111-649">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-649">Yes</span></span>         | <span data-ttu-id="fb111-650">Eşlenmekte olan kavramsal modeldeki varlık özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="fb111-651">**Tation**</span><span class="sxs-lookup"><span data-stu-id="fb111-651">**ColumnName**</span></span> | <span data-ttu-id="fb111-652">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-652">Yes</span></span>         | <span data-ttu-id="fb111-653">Eşlenen sütunun adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="fb111-654">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-654">Example</span></span>

<span data-ttu-id="fb111-655">Aşağıdaki örnek, okul modeline dayalıdır ve **kişi** varlık türünün INSERT Işlevini **ınsertperson** saklı yordamına eşlemek Için kullanılan bir **ınsertfunction** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="fb111-656">( **Insertperson** saklı yordamı aşağıda gösterilmektedir ve depolama modelinde bildirilmiştir.) Bir **Resultbinding** öğesi, saklı yordamın (**newpersonıd**) döndürdüğü bir sütun değerini bir varlık türü özelliğine (**PersonID**) eşlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fb111-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

<span data-ttu-id="fb111-657">Aşağıdaki Transact-SQL, **ınsertperson** saklı yordamını açıklar:</span><span class="sxs-lookup"><span data-stu-id="fb111-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a><span data-ttu-id="fb111-658">ResultMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="fb111-659">Eşleme belirtim dili (MSL) içindeki **Resultmapping** öğesi, aşağıdaki doğru olduğunda, kavramsal modelde bir işlev içeri aktarma ve temel alınan veritabanındaki saklı yordam arasındaki eşlemeyi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="fb111-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="fb111-660">İşlev içeri aktarma bir kavramsal model varlık türü veya karmaşık tür döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="fb111-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="fb111-661">Saklı yordam tarafından döndürülen sütunların adları, varlık türü veya karmaşık türdeki özelliklerin adlarıyla tam olarak eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="fb111-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="fb111-662">Varsayılan olarak, bir saklı yordam tarafından döndürülen sütunlar ve varlık türü veya karmaşık tür arasındaki eşleme, sütun ve özellik adlarını temel alır.</span><span class="sxs-lookup"><span data-stu-id="fb111-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="fb111-663">Sütun adları Özellik adlarıyla tam olarak eşleşmiyorsa, eşlemeyi tanımlamak için **Resultmapping** öğesini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb111-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="fb111-664">Varsayılan eşlemenin bir örneği için bkz. FunctionImportMapping öğesi (MSL).</span><span class="sxs-lookup"><span data-stu-id="fb111-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="fb111-665">**Resultmapping** öğesi FunctionImportMapping öğesinin bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="fb111-666">**Resultmapping** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-667">EntityTypeMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="fb111-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="fb111-668">ComplexTypeMapping</span></span>

<span data-ttu-id="fb111-669">**Resultmapping** öğesi için geçerli bir öznitelik yok.</span><span class="sxs-lookup"><span data-stu-id="fb111-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="fb111-670">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-670">Example</span></span>

<span data-ttu-id="fb111-671">Aşağıdaki saklı yordamı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fb111-671">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="fb111-672">Ayrıca, aşağıdaki kavramsal model varlık türünü de göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fb111-672">Also consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

<span data-ttu-id="fb111-673">Önceki varlık türünün örneklerini döndüren bir işlev içeri aktarması oluşturmak için, saklı yordamın ve varlık türünün döndürdüğü sütunlar arasındaki eşlemenin bir **Resultmapping** öğesinde tanımlanması gerekir:</span><span class="sxs-lookup"><span data-stu-id="fb111-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="fb111-674">ScalarProperty öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="fb111-675">Eşleme belirtim dili (MSL) içindeki **Scalarproperty** öğesi bir kavramsal model varlık türü, karmaşık tür veya bir tablo sütunuyla ilişkili bir özelliği veya temel alınan veritabanındaki saklı yordam parametresini eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="fb111-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="fb111-676">Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="fb111-677">Daha fazla bilgi için bkz. Işlev öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="fb111-678">**Scalarproperty** öğesi aşağıdaki öğelerin bir alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="fb111-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="fb111-679">MappingFragment</span></span>
-   <span data-ttu-id="fb111-680">Insertfunction</span><span class="sxs-lookup"><span data-stu-id="fb111-680">InsertFunction</span></span>
-   <span data-ttu-id="fb111-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="fb111-681">UpdateFunction</span></span>
-   <span data-ttu-id="fb111-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="fb111-682">DeleteFunction</span></span>
-   <span data-ttu-id="fb111-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="fb111-683">EndProperty</span></span>
-   <span data-ttu-id="fb111-684">Complexözelliği</span><span class="sxs-lookup"><span data-stu-id="fb111-684">ComplexProperty</span></span>
-   <span data-ttu-id="fb111-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="fb111-685">ResultMapping</span></span>

<span data-ttu-id="fb111-686">**Mappingfragment**, **ComplexProperty**veya **endproperty** öğesinin bir alt öğesi olarak, **scalarproperty** öğesi kavramsal modeldeki bir özelliği veritabanındaki bir sütuna eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="fb111-687">**Insertfunction**, **Updatefunction**veya **deletefunction** öğesinin bir alt öğesi olarak, **scalarproperty** öğesi kavramsal modeldeki bir özelliği bir saklı yordam parametresine Eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="fb111-688">**Scalarproperty** öğesinin herhangi bir alt öğesi olamaz.</span><span class="sxs-lookup"><span data-stu-id="fb111-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-689">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-689">Applicable Attributes</span></span>

<span data-ttu-id="fb111-690">**Scalarproperty** öğesi için uygulanan öznitelikler, öğesinin rolüne bağlı olarak farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="fb111-691">Aşağıdaki **tabloda, bir** kavramsal model özelliğini veritabanındaki bir sütuna eşlemek için kullanılan özellikler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="fb111-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="fb111-692">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-692">Attribute Name</span></span> | <span data-ttu-id="fb111-693">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-693">Is Required</span></span> | <span data-ttu-id="fb111-694">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="fb111-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb111-695">**Name**</span></span>       | <span data-ttu-id="fb111-696">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-696">Yes</span></span>         | <span data-ttu-id="fb111-697">Eşlenmekte olan kavramsal model özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="fb111-698">**Tation**</span><span class="sxs-lookup"><span data-stu-id="fb111-698">**ColumnName**</span></span> | <span data-ttu-id="fb111-699">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-699">Yes</span></span>         | <span data-ttu-id="fb111-700">Eşlenmekte olan tablo sütununun adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="fb111-701">Aşağıdaki tabloda, bir kavramsal model özelliğini saklı yordam parametresine eşlemek için kullanıldığında **Scalarproperty** öğesi için geçerli olan öznitelikler açıklanmaktadır:</span><span class="sxs-lookup"><span data-stu-id="fb111-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="fb111-702">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-702">Attribute Name</span></span>    | <span data-ttu-id="fb111-703">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-703">Is Required</span></span> | <span data-ttu-id="fb111-704">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="fb111-705">**Name**</span></span>          | <span data-ttu-id="fb111-706">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-706">Yes</span></span>         | <span data-ttu-id="fb111-707">Eşlenmekte olan kavramsal model özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="fb111-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="fb111-708">**ParameterName**</span></span> | <span data-ttu-id="fb111-709">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-709">Yes</span></span>         | <span data-ttu-id="fb111-710">Eşlenmekte olan parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="fb111-711">**Sürüm**</span><span class="sxs-lookup"><span data-stu-id="fb111-711">**Version**</span></span>       | <span data-ttu-id="fb111-712">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-712">No</span></span>          | <span data-ttu-id="fb111-713">**Geçerli** veya **orijinal** değerin, özelliğin orijinal değerinin eşzamanlılık denetimleri için kullanılıp kullanılmayacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="fb111-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="fb111-714">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-714">Example</span></span>

<span data-ttu-id="fb111-715">Aşağıdaki örnek, iki şekilde kullanılan **Scalarproperty** öğesini göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="fb111-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="fb111-716">**Kişi** varlık türünün özelliklerini **kişi**tablosunun sütunlarına eşlemek için.</span><span class="sxs-lookup"><span data-stu-id="fb111-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="fb111-717">**Kişi** varlık türünün özelliklerini **UpdatePerson** saklı yordamının parametreleriyle eşlemek için.</span><span class="sxs-lookup"><span data-stu-id="fb111-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="fb111-718">Saklı yordamlar depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-718">The stored procedures are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="fb111-719">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-719">Example</span></span>

<span data-ttu-id="fb111-720">Sonraki örnekte, bir kavramsal model ilişkisinin INSERT ve DELETE işlevlerini veritabanındaki Saklı yordamlarla eşlemek için kullanılan **Scalarproperty** öğesi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fb111-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="fb111-721">Saklı yordamlar depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-721">The stored procedures are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a><span data-ttu-id="fb111-722">UpdateFunction öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="fb111-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="fb111-723">Eşleme belirtim dili (MSL) içindeki **Updatefunction** öğesi, kavramsal modeldeki bir varlık türünün Update işlevini temel alınan veritabanında bulunan bir saklı yordama eşler.</span><span class="sxs-lookup"><span data-stu-id="fb111-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="fb111-724">Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="fb111-725">Daha fazla bilgi için bkz. Işlev öğesi (SSDL).</span><span class="sxs-lookup"><span data-stu-id="fb111-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="fb111-726">Bir varlık türünün ekleme, güncelleştirme veya silme işlemlerinin üçünü saklı yordamlara eşleştirmez, çalışma zamanında yürütülürse ve bir UpdateException oluşturulursa eşlenmemiş işlemler başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="fb111-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="fb111-727">**Updatefunction** öğesi ModificationFunctionMapping öğesinin bir alt öğesi olabilir ve entitytypemapping öğesine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fb111-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="fb111-728">**Updatefunction** öğesi aşağıdaki alt öğelere sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="fb111-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="fb111-729">AssociationEnd (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="fb111-730">Complexözelliği (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="fb111-731">ResultBinding (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="fb111-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="fb111-732">ScarlarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="fb111-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="fb111-733">Uygulanabilir öznitelikler</span><span class="sxs-lookup"><span data-stu-id="fb111-733">Applicable Attributes</span></span>

<span data-ttu-id="fb111-734">Aşağıdaki tablo **Updatefunction** öğesine uygulanabilen öznitelikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="fb111-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="fb111-735">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="fb111-735">Attribute Name</span></span>            | <span data-ttu-id="fb111-736">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="fb111-736">Is Required</span></span> | <span data-ttu-id="fb111-737">Value</span><span class="sxs-lookup"><span data-stu-id="fb111-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fb111-738">**Ifadelerini**</span><span class="sxs-lookup"><span data-stu-id="fb111-738">**FunctionName**</span></span>          | <span data-ttu-id="fb111-739">Evet</span><span class="sxs-lookup"><span data-stu-id="fb111-739">Yes</span></span>         | <span data-ttu-id="fb111-740">Update işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="fb111-741">Saklı yordam, depolama modelinde bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fb111-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="fb111-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="fb111-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="fb111-743">Hayır</span><span class="sxs-lookup"><span data-stu-id="fb111-743">No</span></span>          | <span data-ttu-id="fb111-744">Etkilenen satır sayısını döndüren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="fb111-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="fb111-745">Örnek</span><span class="sxs-lookup"><span data-stu-id="fb111-745">Example</span></span>

<span data-ttu-id="fb111-746">Aşağıdaki örnek, okul modeline dayalıdır ve **kişi** varlık türünün güncelleştirme Işlevini **UpdatePerson** saklı yordamına eşlemek için kullanılan **updatefunction** öğesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb111-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="fb111-747">**UpdatePerson** saklı yordamı depolama modelinde bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fb111-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
