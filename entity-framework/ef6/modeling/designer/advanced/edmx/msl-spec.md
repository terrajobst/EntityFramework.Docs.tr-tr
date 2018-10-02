---
title: MSL belirtimi - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 6bff1f5407bc0546e60b5bee1178be9aa4748bd8
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460143"
---
# <a name="msl-specification"></a><span data-ttu-id="8a89f-102">MSL belirtimi</span><span class="sxs-lookup"><span data-stu-id="8a89f-102">MSL Specification</span></span>
<span data-ttu-id="8a89f-103">Eşleme belirtimi dili (MSL) kavramsal model ve depolama modeli bir Entity Framework uygulamasının arasındaki eşlemeyi açıklayan bir XML tabanlı dilidir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="8a89f-104">Bir Entity Framework uygulamasında eşleme meta veri (MSL içinde yazılan) .msl dosyasından derleme sırasında yüklenir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="8a89f-105">Entity Framework eşleme meta veri deposu özgü komutlar için kavramsal modeline karşı sorgular çevirmek için çalışma zamanında kullanır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="8a89f-106">Entity Framework Designer (EF Designer), tasarım zamanında bir .edmx dosyası içinde eşleme bilgilerini depolar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="8a89f-107">Derleme sırasında varlık Tasarımcısı bilgi bir .edmx dosyası içinde Entity Framework tarafından çalışma zamanında gereken .msl dosyası oluşturmak için kullanır</span><span class="sxs-lookup"><span data-stu-id="8a89f-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="8a89f-108">Tüm kavramsal adlarını veya MSL içinde başvurulan depolama model türleri ilgili ad alanı adlarıyla nitelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="8a89f-109">Kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. [CSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="8a89f-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="8a89f-110">Depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz. [SSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="8a89f-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="8a89f-111">MSL sürümleri, XML ad alanları tarafından ayrılır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="8a89f-112">MSL sürümü</span><span class="sxs-lookup"><span data-stu-id="8a89f-112">MSL Version</span></span> | <span data-ttu-id="8a89f-113">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="8a89f-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="8a89f-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="8a89f-114">MSL v1</span></span>      | <span data-ttu-id="8a89f-115">urn: schemas-microsoft-com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="8a89f-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="8a89f-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="8a89f-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="8a89f-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="8a89f-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="8a89f-118">Diğer ad öğesinde (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-118">Alias Element (MSL)</span></span>

<span data-ttu-id="8a89f-119">**Diğer** eşleme belirtimi dili (MSL) öğedir kavramsal model ve depolama modeli ad alanları için diğer adlarını tanımlamak için kullanılan eşleme öğesi alt.</span><span class="sxs-lookup"><span data-stu-id="8a89f-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="8a89f-120">Tüm kavramsal adlarını veya MSL içinde başvurulan depolama model türleri ilgili ad alanı adlarıyla nitelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="8a89f-121">Şema öğesi (CSDL) kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="8a89f-122">Şema öğesi (SSDL) depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="8a89f-123">**Diğer** öğesi alt öğeleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-124">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-124">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-125">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **diğer** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="8a89f-126">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-126">Attribute Name</span></span> | <span data-ttu-id="8a89f-127">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-127">Is Required</span></span> | <span data-ttu-id="8a89f-128">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="8a89f-129">**Key**</span></span>        | <span data-ttu-id="8a89f-130">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-130">Yes</span></span>         | <span data-ttu-id="8a89f-131">Tarafından belirtilen ad alanı diğer **değer** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8a89f-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="8a89f-132">**Değer**</span><span class="sxs-lookup"><span data-stu-id="8a89f-132">**Value**</span></span>      | <span data-ttu-id="8a89f-133">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-133">Yes</span></span>         | <span data-ttu-id="8a89f-134">Kendisi için bir ad alanı değeri **anahtar** bir diğer ad bir öğedir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="8a89f-135">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-135">Example</span></span>

<span data-ttu-id="8a89f-136">Aşağıdaki örnekte gösterildiği bir **diğer** tanımlayan bir diğer öğe `c`, kavramsal modelde tanımlı türleri için.</span><span class="sxs-lookup"><span data-stu-id="8a89f-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="associationend-element-msl"></a><span data-ttu-id="8a89f-137">İlişki ucu öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="8a89f-138">**İlişki ucu** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında saklı yordamlar için kavramsal modeldeki bir varlık türünün değiştirilmesi işlevleri eşlendiğinde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="8a89f-139">Saklı yordamın kullandığı bir parametre değeri bir ilişkilendirme özelliğinde tutulan bir değişiklik olursa **ilişki ucu** öğesi parametresi özellik değeri eşler.</span><span class="sxs-lookup"><span data-stu-id="8a89f-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="8a89f-140">Daha fazla bilgi için aşağıdaki örnekte bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-140">For more information, see the example below.</span></span>

<span data-ttu-id="8a89f-141">Saklı yordamlar için değişiklik işlevleri varlık türleri eşleme hakkında daha fazla bilgi için bkz: ModificationFunctionMapping öğesi (MSL) ve izlenecek yol: saklı yordamlar için bir varlık eşlemesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="8a89f-142">**İlişki ucu** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="8a89f-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-144">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-144">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-145">Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ilişki ucu** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="8a89f-146">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-146">Attribute Name</span></span>     | <span data-ttu-id="8a89f-147">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-147">Is Required</span></span> | <span data-ttu-id="8a89f-148">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="8a89f-149">**AssociationSet**</span></span> | <span data-ttu-id="8a89f-150">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-150">Yes</span></span>         | <span data-ttu-id="8a89f-151">Eşlenmekte olan ilişki adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="8a89f-152">**Kaynak**</span><span class="sxs-lookup"><span data-stu-id="8a89f-152">**From**</span></span>           | <span data-ttu-id="8a89f-153">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-153">Yes</span></span>         | <span data-ttu-id="8a89f-154">Değerini **FromRole** eşlenmekte olan ilişki için karşılık gelen gezinme özelliğini özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8a89f-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="8a89f-155">NavigationProperty öğesi (CSDL) daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="8a89f-156">**Hedef**</span><span class="sxs-lookup"><span data-stu-id="8a89f-156">**To**</span></span>             | <span data-ttu-id="8a89f-157">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-157">Yes</span></span>         | <span data-ttu-id="8a89f-158">Değerini **ToRole** eşlenmekte olan ilişki için karşılık gelen gezinme özelliğini özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8a89f-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="8a89f-159">NavigationProperty öğesi (CSDL) daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="8a89f-160">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-160">Example</span></span>

<span data-ttu-id="8a89f-161">Aşağıdaki kavramsal model varlık türünü göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a89f-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="8a89f-162">Ayrıca, aşağıdaki depolanan yordamı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a89f-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="8a89f-163">Güncelleştirme işlevini eşlemek için `Course` varlık Bu saklı yordamı için bir değer sağlamanız gerekir **DepartmentID** parametresi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="8a89f-164">Değeri `DepartmentID` varlık türü; bir özelliğe karşılık gelmiyor olan eşleme burada gösterilen bağımsız bir ilişkide yer alır:</span><span class="sxs-lookup"><span data-stu-id="8a89f-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="8a89f-165">Aşağıdaki kodda gösterildiği **ilişki ucu** eşlemek için kullanılan öğe **DepartmentID** özelliği **FK\_kurs\_departmanı** koleksiyonla ilişki **UpdateCourse** saklı yordamını (hangi güncelleştirme işlevini **kurs** varlık türü eşlendi):</span><span class="sxs-lookup"><span data-stu-id="8a89f-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="8a89f-166">AssociationSetMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="8a89f-167">**AssociationSetMapping** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında kavramsal model ve tablo sütunları bir ilişkide arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="8a89f-168">Kavramsal modeldeki ilişkileri, birincil ve yabancı anahtar sütunları temel alınan veritabanında özelliklerini temsil türleridir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="8a89f-169">**AssociationSetMapping** öğe iki EndProperty öğe ilişki türü özellikleri ve sütunları arasındaki eşlemeleri veritabanında tanımlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="8a89f-170">Bu eşlemeler koşul öğe ile koşulları yerleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="8a89f-171">INSERT, update ve delete işlevleri ilişkileri için saklı yordamları veritabanında ModificationFunctionMapping öğesiyle eşlenir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="8a89f-172">Salt okunur eşlemeleri arasındaki ilişkilendirmeleri ve tablo sütunları bir QueryView öğesinde bir varlık SQL dizesi kullanarak tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-173">Başvurusal Kısıt bir ilişkisi kavramsal modelde tanımlı ise ilişkilendirme ile eşlenmesi gerekmez bir **AssociationSetMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="8a89f-174">Varsa bir **AssociationSetMapping** ilişkilendirme için bir başvuru kısıtlamasını sahip öğe varsa, tanımlanan eşlemeler **AssociationSetMapping** öğesi yok sayılacak.</span><span class="sxs-lookup"><span data-stu-id="8a89f-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="8a89f-175">Daha fazla bilgi için Referentialconstraint'teki öğesi (CSDL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="8a89f-176">**AssociationSetMapping** öğesi şu alt öğelerden olabilir</span><span class="sxs-lookup"><span data-stu-id="8a89f-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="8a89f-177">QueryView (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="8a89f-178">EndProperty (sıfır veya iki)</span><span class="sxs-lookup"><span data-stu-id="8a89f-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="8a89f-179">Koşul (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="8a89f-180">ModificationFunctionMapping (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-181">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-181">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-182">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **AssociationSetMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="8a89f-183">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-183">Attribute Name</span></span>     | <span data-ttu-id="8a89f-184">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-184">Is Required</span></span> | <span data-ttu-id="8a89f-185">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-186">**Ad**</span><span class="sxs-lookup"><span data-stu-id="8a89f-186">**Name**</span></span>           | <span data-ttu-id="8a89f-187">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-187">Yes</span></span>         | <span data-ttu-id="8a89f-188">Eşlenmekte olan kavramsal model ilişki kümesi adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="8a89f-189">**typeName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-189">**TypeName**</span></span>       | <span data-ttu-id="8a89f-190">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-190">No</span></span>          | <span data-ttu-id="8a89f-191">Eşlenmekte olan kavramsal model ilişkilendirme türü ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="8a89f-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="8a89f-192">**StoreEntitySet**</span></span> | <span data-ttu-id="8a89f-193">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-193">No</span></span>          | <span data-ttu-id="8a89f-194">Eşlenmekte olan tablonun adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="8a89f-195">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-195">Example</span></span>

<span data-ttu-id="8a89f-196">Aşağıdaki örnekte gösterildiği bir **AssociationSetMapping** hangi öğesinde **FK\_kurs\_departmanı** ilişkisi kavramsal modelde ayarlamak için eşlendi **Kurs** veritabanındaki tablo.</span><span class="sxs-lookup"><span data-stu-id="8a89f-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="8a89f-197">İlişki türü özellikleri ve tablo sütunları arasındaki eşlemeleri alt belirtilen **EndProperty** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="8a89f-198">ComplexProperty öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="8a89f-199">A **ComplexProperty** eşleme belirtimi dili (MSL) içindeki öğe bir karmaşık tür özelliği temel alınan veritabanında bir kavramsal model varlık türü ve tablo sütunu arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="8a89f-200">Özellik sütun eşlemelerini alt ScalarProperty öğesinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="8a89f-201">**ComplexType** özellik öğesi şu alt öğelerden sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-202">ScalarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="8a89f-203">**ComplexProperty** (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="8a89f-204">ComplextTypeMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="8a89f-205">Koşul (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-206">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-206">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-207">Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ComplexProperty** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="8a89f-208">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-208">Attribute Name</span></span> | <span data-ttu-id="8a89f-209">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-209">Is Required</span></span> | <span data-ttu-id="8a89f-210">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-211">**Ad**</span><span class="sxs-lookup"><span data-stu-id="8a89f-211">**Name**</span></span>       | <span data-ttu-id="8a89f-212">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-212">Yes</span></span>         | <span data-ttu-id="8a89f-213">Eşlenmekte olan kavramsal modeldeki bir varlık türünün karmaşık özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="8a89f-214">**typeName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-214">**TypeName**</span></span>   | <span data-ttu-id="8a89f-215">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-215">No</span></span>          | <span data-ttu-id="8a89f-216">Kavramsal model özellik türü ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="8a89f-217">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-217">Example</span></span>

<span data-ttu-id="8a89f-218">Aşağıdaki örnek, okul modelini temel alıyor.</span><span class="sxs-lookup"><span data-stu-id="8a89f-218">The following example is based on the School model.</span></span> <span data-ttu-id="8a89f-219">Aşağıdaki karmaşık türü için kavramsal model eklenmiştir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="8a89f-220">**LastName** ve **FirstName** özelliklerini **kişi** varlık türü karmaşık bir özellik ile değiştirildi **adı**:</span><span class="sxs-lookup"><span data-stu-id="8a89f-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="8a89f-221">Aşağıdaki MSL gösterildiği **ComplexProperty** eşlemek için kullanılan öğe **adı** özelliği temel alınan veritabanında sütunlara:</span><span class="sxs-lookup"><span data-stu-id="8a89f-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="8a89f-222">ComplexTypeMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="8a89f-223">**ComplexTypeMapping** eşleme belirtimi dili (MSL) içindeki öğe ResultMapping öğesinin bir alt öğesidir ve temel kavramsal modeldeki bir işlev içeri aktarma ve bir saklı yordam arasındaki eşlemeyi tanımlar. Veritabanı aşağıdaki doğru olduğunda:</span><span class="sxs-lookup"><span data-stu-id="8a89f-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="8a89f-224">İşlev kavramsal karmaşık bir tür döndürür.</span><span class="sxs-lookup"><span data-stu-id="8a89f-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="8a89f-225">Saklı yordam tarafından döndürülen sütun adlarını tam olarak karmaşık tür özellikleri adlarını eşleştirin.</span><span class="sxs-lookup"><span data-stu-id="8a89f-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="8a89f-226">Varsayılan olarak, sütunlar arasındaki eşlemeyi bir saklı yordam tarafından döndürülen ve sütun ve özellik adları üzerinde karmaşık bir tür alır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="8a89f-227">Sütun adları tam olarak eşleşen özellik adlarını, kullanmalısınız **ComplexTypeMapping** eşleme tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="8a89f-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="8a89f-228">Varsayılan eşleme örneği için Functionımportmapping öğesi (MSL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="8a89f-229">**ComplexTypeMapping** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-230">ScalarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-231">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-231">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-232">Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ComplexTypeMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="8a89f-233">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-233">Attribute Name</span></span> | <span data-ttu-id="8a89f-234">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-234">Is Required</span></span> | <span data-ttu-id="8a89f-235">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="8a89f-236">**typeName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-236">**TypeName**</span></span>   | <span data-ttu-id="8a89f-237">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-237">Yes</span></span>         | <span data-ttu-id="8a89f-238">Eşlenmekte olan bir karmaşık tür ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="8a89f-239">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-239">Example</span></span>

<span data-ttu-id="8a89f-240">Aşağıdaki saklı yordamı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a89f-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="8a89f-241">Ayrıca, aşağıdaki kavramsal model karmaşık tür göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a89f-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="8a89f-242">Varlık türü tanımlanmalıdır ve sütunlar arasındaki eşlemeyi önceki karmaşık türün örneğini döndüren bir işlev içeri aktarma oluşturabilmek için saklı yordam tarafından döndürülen bir **ComplexTypeMapping** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="8a89f-243">Koşul öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-243">Condition Element (MSL)</span></span>

<span data-ttu-id="8a89f-244">**Koşul** eşleme belirtimi dili (MSL) içindeki öğenin kavramsal model ve temel alınan veritabanı arasındaki eşlemeleri üzerinde koşullar yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="8a89f-245">Bir XML düğümü içinde tanımlanan tüm geçerli koşullar, belirtilen alt öğesi olarak eşlemedir **koşul** öğe karşılandığı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="8a89f-246">Aksi halde, eşleme geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="8a89f-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="8a89f-247">Örneğin, bir veya daha fazla MappingFragment öğe içeriyorsa, **koşul** alt öğeleri eşleme içinde tanımlanan **MappingFragment** düğümü yalnızca tüm geçerli olacaktır altkoşulları **Koşul** öğe karşılandığı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="8a89f-248">Her koşul için ya da uygulayabilirsiniz bir **adı** (bir kavramsal model varlık özelliği tarafından belirtilen adı **adı** özniteliği), veya bir **ColumnName** (adını bir sütun Belirtilen veritabanı **ColumnName** özniteliği).</span><span class="sxs-lookup"><span data-stu-id="8a89f-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="8a89f-249">Zaman **adı** özniteliği, bir varlık özelliğinin değeri koşul denetlenir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="8a89f-250">Zaman **ColumnName** özniteliği, bir sütun değeri koşul denetlenir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="8a89f-251">Yalnızca biri **adı** veya **ColumnName** özniteliği belirtilebilir bir **koşul** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-252">Zaman **koşul** öğe Functionımportmapping element içinde yalnızca kullanılan **adı** özniteliği geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="8a89f-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="8a89f-253">**Koşul** aşağıdaki öğelerin bir alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="8a89f-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="8a89f-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="8a89f-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="8a89f-255">ComplexProperty</span></span>
-   <span data-ttu-id="8a89f-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="8a89f-256">EntitySetMapping</span></span>
-   <span data-ttu-id="8a89f-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="8a89f-257">MappingFragment</span></span>
-   <span data-ttu-id="8a89f-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="8a89f-258">EntityTypeMapping</span></span>

<span data-ttu-id="8a89f-259">**Koşul** öğesi alt öğe yok olabilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-260">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-260">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-261">Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **koşul** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="8a89f-262">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-262">Attribute Name</span></span> | <span data-ttu-id="8a89f-263">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-263">Is Required</span></span> | <span data-ttu-id="8a89f-264">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-265">**ColumnName**</span></span> | <span data-ttu-id="8a89f-266">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-266">No</span></span>          | <span data-ttu-id="8a89f-267">Değeri koşulu değerlendirmek için kullanılabilecek tablo sütununun adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="8a89f-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="8a89f-268">**IsNull**</span></span>     | <span data-ttu-id="8a89f-269">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-269">No</span></span>          | <span data-ttu-id="8a89f-270">**Doğru** veya **False**.</span><span class="sxs-lookup"><span data-stu-id="8a89f-270">**True** or **False**.</span></span> <span data-ttu-id="8a89f-271">Değer ise **True** ve sütun değeri **null**, veya değer ise **False** ve sütun değeri değil **null**, koşul true olduğu .</span><span class="sxs-lookup"><span data-stu-id="8a89f-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="8a89f-272">Aksi halde koşul false olur.</span><span class="sxs-lookup"><span data-stu-id="8a89f-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="8a89f-273">**IsNull** ve **değer** özniteliklerine aynı anda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="8a89f-274">**Değer**</span><span class="sxs-lookup"><span data-stu-id="8a89f-274">**Value**</span></span>      | <span data-ttu-id="8a89f-275">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-275">No</span></span>          | <span data-ttu-id="8a89f-276">Sütun değeri karşılaştırılacağı değeri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-276">The value with which the column value is compared.</span></span> <span data-ttu-id="8a89f-277">Değerler aynıysa koşul true'dur.</span><span class="sxs-lookup"><span data-stu-id="8a89f-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="8a89f-278">Aksi halde koşul false olur.</span><span class="sxs-lookup"><span data-stu-id="8a89f-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="8a89f-279">**IsNull** ve **değer** özniteliklerine aynı anda kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="8a89f-280">**Ad**</span><span class="sxs-lookup"><span data-stu-id="8a89f-280">**Name**</span></span>       | <span data-ttu-id="8a89f-281">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-281">No</span></span>          | <span data-ttu-id="8a89f-282">Değeri koşulu değerlendirmek için kullanılan kavramsal model varlık özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="8a89f-283">Bu özniteliği geçerli değil, **koşul** öğesi içinde bir Functionımportmapping element kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="8a89f-284">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-284">Example</span></span>

<span data-ttu-id="8a89f-285">Aşağıdaki örnekte gösterildiği **koşul** öğeleri alt öğeleri olarak **MappingFragment** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="8a89f-286">Zaman **İşeAlmaTarihi** null değil ve **EnrollmentDate** olan veri arasında null eşlendi **SchoolModel.Instructor** türü ve **Personıd**ve **İşeAlmaTarihi** sütunlarının **kişi** tablo.</span><span class="sxs-lookup"><span data-stu-id="8a89f-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="8a89f-287">Zaman **EnrollmentDate** null değil ve **İşeAlmaTarihi** olan veri arasında null eşlendi **SchoolModel.Student** türü ve **Personıd** ve **kayıt** sütunlarının **kişi** tablo.</span><span class="sxs-lookup"><span data-stu-id="8a89f-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="8a89f-288">DeleteFunction öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="8a89f-289">**DeleteFunction** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında bir saklı yordam için bir varlık türünün veya ilişkisi kavramsal modelde silme işlevini eşlemeleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="8a89f-290">Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="8a89f-291">Daha fazla bilgi için işlev öğesi (SSDL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-292">Değil eşlerseniz üçünü ekleme, güncelleştirme veya silme işlemleri saklı yordamlar için bir varlık türünün, çalışma zamanında yürütülüyorsa eşlenmemiş işlemleri başarısız olur ve bir UpdateException oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a89f-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="8a89f-293">EntityTypeMapping için uygulanan DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="8a89f-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="8a89f-294">EntityTypeMapping öğesine uygulandığında **DeleteFunction** öğesi için bir saklı yordam kavramsal modeldeki bir varlık türünün silme işlevini eşler.</span><span class="sxs-lookup"><span data-stu-id="8a89f-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="8a89f-295">**DeleteFunction** öğesi şu alt öğelerden uygulandığında olabilir bir **EntityTypeMapping** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="8a89f-296">İlişki ucu (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="8a89f-297">ComplexProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="8a89f-298">ScarlarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-299">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-299">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-300">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **DeleteFunction** için uygulandığında öğesi bir **EntityTypeMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="8a89f-301">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-301">Attribute Name</span></span>            | <span data-ttu-id="8a89f-302">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-302">Is Required</span></span> | <span data-ttu-id="8a89f-303">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-304">**functionName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-304">**FunctionName**</span></span>          | <span data-ttu-id="8a89f-305">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-305">Yes</span></span>         | <span data-ttu-id="8a89f-306">Silme işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="8a89f-307">Saklı yordam depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="8a89f-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="8a89f-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="8a89f-309">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-309">No</span></span>          | <span data-ttu-id="8a89f-310">Etkilenen satırların sayısını veren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="8a89f-311">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-311">Example</span></span>

<span data-ttu-id="8a89f-312">Aşağıdaki örnek Okul modelini temel alan ve gösterir **DeleteFunction** silme işlevini eşleme öğesi **kişi** varlık türüne **DeletePerson** saklı yordam.</span><span class="sxs-lookup"><span data-stu-id="8a89f-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="8a89f-313">**DeletePerson** saklı yordam, depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="8a89f-314">AssociationSetMapping için uygulanan DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="8a89f-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="8a89f-315">AssociationSetMapping öğesine uygulandığında **DeleteFunction** öğesi saklı yordama ilişkilendirme kavramsal modeldeki silme işlevini eşlemeleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="8a89f-316">**DeleteFunction** öğesi şu alt öğelerden uygulandığında olabilir **AssociationSetMapping** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="8a89f-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="8a89f-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-318">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-318">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-319">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **DeleteFunction** için uygulandığında öğesi **AssociationSetMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="8a89f-320">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-320">Attribute Name</span></span>            | <span data-ttu-id="8a89f-321">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-321">Is Required</span></span> | <span data-ttu-id="8a89f-322">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-323">**functionName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-323">**FunctionName**</span></span>          | <span data-ttu-id="8a89f-324">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-324">Yes</span></span>         | <span data-ttu-id="8a89f-325">Silme işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="8a89f-326">Saklı yordam depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="8a89f-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="8a89f-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="8a89f-328">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-328">No</span></span>          | <span data-ttu-id="8a89f-329">Etkilenen satırların sayısını veren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="8a89f-330">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-330">Example</span></span>

<span data-ttu-id="8a89f-331">Aşağıdaki örnek Okul modelini temel alan ve gösterir **DeleteFunction** silme işlevini eşlemek için kullanılan öğe **CourseInstructor** ilişkilendirmeye  **DeleteCourseInstructor** saklı yordamı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="8a89f-332">**DeleteCourseInstructor** saklı yordam, depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="8a89f-333">EndProperty öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="8a89f-334">**EndProperty** eşleme belirtimi dili (MSL) içindeki öğe sona veya değişiklik işlevi bir kavramsal model ilişkisi ve temel alınan veritabanı arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="8a89f-335">Özellik sütun eşleme, bir alt ScalarProperty öğesinde belirtilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="8a89f-336">Olduğunda bir **EndProperty** öğe eşleme için kavramsal model ilişki sonu tanımlamak için kullanılır, AssociationSetMapping öğesi bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="8a89f-337">Zaman **EndProperty** öğesi eşleme için kavramsal model ilişki değişikliği işlevi tanımlamak için kullanıldığında, bir InsertFunction DeleteFunction öğesi veya bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="8a89f-338">**EndProperty** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-339">ScalarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-340">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-340">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-341">Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **EndProperty** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="8a89f-342">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-342">Attribute Name</span></span> | <span data-ttu-id="8a89f-343">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-343">Is Required</span></span> | <span data-ttu-id="8a89f-344">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="8a89f-345">Ad</span><span class="sxs-lookup"><span data-stu-id="8a89f-345">Name</span></span>           | <span data-ttu-id="8a89f-346">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-346">Yes</span></span>         | <span data-ttu-id="8a89f-347">Eşlenmekte olan ilişki sonu adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="8a89f-348">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-348">Example</span></span>

<span data-ttu-id="8a89f-349">Aşağıdaki örnekte gösterildiği bir **AssociationSetMapping** hangi öğesinde **FK\_kurs\_departmanı** ilişkisi kavramsal modelde içineşlenmiş**Kurs** veritabanındaki tablo.</span><span class="sxs-lookup"><span data-stu-id="8a89f-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="8a89f-350">İlişki türü özellikleri ve tablo sütunları arasındaki eşlemeleri alt belirtilen **EndProperty** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="8a89f-351">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-351">Example</span></span>

<span data-ttu-id="8a89f-352">Aşağıdaki örnekte gösterildiği **EndProperty** ilişkilendirme INSERT ve delete işlevlerini eşleme öğesi (**CourseInstructor**) temel alınan veritabanında saklı yordamlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="8a89f-353">Eşleştirilmiş işlevleri depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="8a89f-354">Entitycontainermapping'indeki öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="8a89f-355">**Entitycontainermapping'indeki** eşleme belirtimi dili (MSL) içindeki öğe depolama modelinin varlık kapsayıcısı kavramsal modeldeki varlık kapsayıcısı eşler.</span><span class="sxs-lookup"><span data-stu-id="8a89f-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="8a89f-356">**Entitycontainermapping'indeki** eşleme öğesi bir alt öğesidir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="8a89f-357">**Entitycontainermapping'indeki** öğesi şu alt öğelerden (listelenen sırayla) olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a89f-358">EntitySetMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="8a89f-359">AssociationSetMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="8a89f-360">Functionımportmapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-361">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-361">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-362">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **Entitycontainermapping'indeki** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="8a89f-363">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-363">Attribute Name</span></span>            | <span data-ttu-id="8a89f-364">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-364">Is Required</span></span> | <span data-ttu-id="8a89f-365">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="8a89f-366">**StorageModelContainer**</span></span> | <span data-ttu-id="8a89f-367">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-367">Yes</span></span>         | <span data-ttu-id="8a89f-368">Eşlenmekte olan depolama modelinin varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="8a89f-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="8a89f-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="8a89f-370">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-370">Yes</span></span>         | <span data-ttu-id="8a89f-371">Eşlenmekte olan kavramsal model varlık kapsayıcısının adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="8a89f-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="8a89f-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="8a89f-373">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-373">No</span></span>          | <span data-ttu-id="8a89f-374">**Doğru** veya **False**.</span><span class="sxs-lookup"><span data-stu-id="8a89f-374">**True** or **False**.</span></span> <span data-ttu-id="8a89f-375">Varsa **False**, hiçbir güncelleştirme görünümleri oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a89f-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="8a89f-376">Bu öznitelik ayarlanmalıdır **False** verileri başarıyla dönmez olabilir çünkü, geçersiz olacak salt okunur bir eşleme varsa.</span><span class="sxs-lookup"><span data-stu-id="8a89f-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="8a89f-377">Varsayılan değer **True**.</span><span class="sxs-lookup"><span data-stu-id="8a89f-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="8a89f-378">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-378">Example</span></span>

<span data-ttu-id="8a89f-379">Aşağıdaki örnekte gösterildiği bir **Entitycontainermapping'indeki** eşleşen öğe **SchoolModelEntities** (kavramsal model varlık kapsayıcısı) kapsayıcıya  **SchoolModelStoreContainer** kapsayıcı (depolama modelinin varlık kapsayıcısı):</span><span class="sxs-lookup"><span data-stu-id="8a89f-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="8a89f-380">EntitySetMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="8a89f-381">**EntitySetMapping** eşleme belirtimi dili (MSL) eşlemeleri kavramsal model varlıktaki tüm türleri varlık kümesi içindeki öğe depolama modelinde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="8a89f-382">Bir varlık kavramsal modelde kümesi için bir mantıksal kapsayıcıdır örnekleri varlık aynı türde (ve türetilen türler).</span><span class="sxs-lookup"><span data-stu-id="8a89f-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="8a89f-383">Varlık kümesi depolama modelinde, bir tablo veya Görünüm temel alınan veritabanında temsil eder.</span><span class="sxs-lookup"><span data-stu-id="8a89f-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="8a89f-384">Kavramsal model varlık kümesini değeri tarafından belirtilen **adı** özniteliği **EntitySetMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="8a89f-385">Eşlenen için tablo veya görünüm tarafından belirtilen **StoreEntitySet** her alt MappingFragment öğe veya öznitelik **EntitySetMapping** öğenin kendisinin.</span><span class="sxs-lookup"><span data-stu-id="8a89f-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="8a89f-386">**EntitySetMapping** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-387">EntityTypeMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="8a89f-388">QueryView (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="8a89f-389">MappingFragment (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-390">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-390">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-391">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntitySetMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="8a89f-392">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-392">Attribute Name</span></span>           | <span data-ttu-id="8a89f-393">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-393">Is Required</span></span> | <span data-ttu-id="8a89f-394">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-395">**Ad**</span><span class="sxs-lookup"><span data-stu-id="8a89f-395">**Name**</span></span>                 | <span data-ttu-id="8a89f-396">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-396">Yes</span></span>         | <span data-ttu-id="8a89f-397">Eşlenmekte olan kavramsal model varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="8a89f-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="8a89f-398">**TypeName** **1**</span></span>       | <span data-ttu-id="8a89f-399">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-399">No</span></span>          | <span data-ttu-id="8a89f-400">Eşlenmekte olan kavramsal model varlık türünün adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="8a89f-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="8a89f-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="8a89f-402">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-402">No</span></span>          | <span data-ttu-id="8a89f-403">İçin eşlenen depolama modelinin varlık kümesinin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="8a89f-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="8a89f-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="8a89f-405">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-405">No</span></span>          | <span data-ttu-id="8a89f-406">**Doğru** veya **False** bağlı olarak yalnızca ayrı satırların olup olmadığını döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8a89f-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="8a89f-407">Bu öznitelik ayarlanırsa **True**, **GenerateUpdateViews** Entitycontainermapping'indeki öğesinin özniteliği ayarlanmalıdır **False**.</span><span class="sxs-lookup"><span data-stu-id="8a89f-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="8a89f-408">**1** **TypeName** ve **StoreEntitySet** öznitelikleri eşleme tek bir tabloya bir tek bir varlık türü için EntityTypeMapping ve MappingFragment alt öğeleri yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="8a89f-409">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-409">Example</span></span>

<span data-ttu-id="8a89f-410">Aşağıdaki örnekte gösterildiği bir **EntitySetMapping** üç tür (bir taban türü ve iki türetilmiş türler) içinde eşleşen öğe **kursları** üç farklı tabloda için kavramsal modelin varlık kümesi temel alınan veritabanı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="8a89f-411">Tabloları tarafından belirtilen **StoreEntitySet** her öznitelik **MappingFragment** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="8a89f-412">EntityTypeMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="8a89f-413">**EntityTypeMapping** eşleme belirtimi dili (MSL) içinde öğe, temel alınan veritabanında bir varlık türü kavramsal model ve tabloları veya görünümleri arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="8a89f-414">EntityType öğesi (CSDL) ve Entityset'in öğe (SSDL) kavramsal model varlık türleri ve temel alınan veritabanı tabloları veya görünümleri hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="8a89f-415">Eşlenmekte olan kavramsal model varlık türü tarafından belirtilen **TypeName** özniteliği **EntityTypeMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="8a89f-416">Eşlenmekte olan Görünüm ve tablo tarafından belirtilen **StoreEntitySet** alt MappingFragment öğesinin özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8a89f-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="8a89f-417">Alt Öğe Ekle eşlemek için kullanılan ModificationFunctionMapping update veya delete işlevleri veritabanında saklı yordamlar için varlık türleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="8a89f-418">**EntityTypeMapping** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-419">MappingFragment (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="8a89f-420">ModificationFunctionMapping (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="8a89f-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="8a89f-421">ScalarProperty</span></span>
-   <span data-ttu-id="8a89f-422">Koşul</span><span class="sxs-lookup"><span data-stu-id="8a89f-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-423">**MappingFragment** ve **ModificationFunctionMapping** öğeleri alt öğeleri olamaz **EntityTypeMapping** aynı anda öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="8a89f-424">**ScalarProperty** ve **koşul** öğeleri alt öğelerinin yalnızca olabilir **EntityTypeMapping** bir Functionımportmapping element içinde kullanıldığında öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-425">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-425">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-426">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntityTypeMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="8a89f-427">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-427">Attribute Name</span></span> | <span data-ttu-id="8a89f-428">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-428">Is Required</span></span> | <span data-ttu-id="8a89f-429">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-430">**typeName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-430">**TypeName**</span></span>   | <span data-ttu-id="8a89f-431">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-431">Yes</span></span>         | <span data-ttu-id="8a89f-432">Eşlenmekte olan kavramsal model varlık türü ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="8a89f-433">Tür abstract veya türetilmiş bir tür ise, değer olmalıdır `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="8a89f-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="8a89f-434">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-434">Example</span></span>

<span data-ttu-id="8a89f-435">Aşağıdaki örnek, bir iki alt EntitySetMapping öğeyle gösterir **EntityTypeMapping** öğeleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="8a89f-436">İlk **EntityTypeMapping** öğesi **SchoolModel.Person** varlık türü eşlenmiş durumda **kişi** tablo.</span><span class="sxs-lookup"><span data-stu-id="8a89f-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="8a89f-437">İkinci **EntityTypeMapping** öğesinde, güncelleştirme işlevleri **SchoolModel.Person** türü bir saklı yordam için eşlenmiş **UpdatePerson**, veritabanındaki .</span><span class="sxs-lookup"><span data-stu-id="8a89f-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="8a89f-438">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-438">Example</span></span>

<span data-ttu-id="8a89f-439">Sonraki örnek, kök türü soyut bir tür hiyerarşisi eşleme gösterir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="8a89f-440">Kullanımına dikkat edin `IsOfType` söz diziminin **TypeName** öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="8a89f-441">Functionımportmapping Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="8a89f-442">**Functionımportmapping** eşleme belirtimi dili (MSL) içindeki öğe, temel alınan veritabanında bir işlev içeri aktarma kavramsal model ve bir saklı yordam veya işlev arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="8a89f-443">Kavramsal modelde işlevi içeri aktarmalar bildirilmelidir ve saklı yordamlar depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="8a89f-444">Daha fazla bilgi için Functionımport öğesi (CSDL) ve işlev öğesi (SSDL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-445">Bir işlev içeri aktarma kavramsal model varlık türünün veya karmaşık türü döndürürse, varsayılan olarak, ardından temel alınan bir saklı yordam tarafından döndürülen sütun adlarını tam olarak kavramsal model türündeki özellikleri adları eşleşmelidir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="8a89f-446">Sütun adlarını tam olarak eşleşen özellik adlarını, eşleme bir ResultMapping öğesinde tanımlanmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="8a89f-447">**Functionımportmapping** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-448">ResultMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-449">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-449">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-450">Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **Functionımportmapping** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="8a89f-451">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-451">Attribute Name</span></span>         | <span data-ttu-id="8a89f-452">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-452">Is Required</span></span> | <span data-ttu-id="8a89f-453">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-454">**FunctionImportName**</span></span> | <span data-ttu-id="8a89f-455">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-455">Yes</span></span>         | <span data-ttu-id="8a89f-456">Eşlenmekte olan kavramsal modeldeki işlevi içeri aktarma adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="8a89f-457">**functionName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-457">**FunctionName**</span></span>       | <span data-ttu-id="8a89f-458">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-458">Yes</span></span>         | <span data-ttu-id="8a89f-459">Eşlenmekte olan depolama modelinde işlevi ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="8a89f-460">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-460">Example</span></span>

<span data-ttu-id="8a89f-461">Aşağıdaki örnek, okul modelini temel alıyor.</span><span class="sxs-lookup"><span data-stu-id="8a89f-461">The following example is based on the School model.</span></span> <span data-ttu-id="8a89f-462">Aşağıdaki işlev depolama modelinde göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a89f-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="8a89f-463">Ayrıca, bu işlev içeri aktarma kavramsal modeldeki göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a89f-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="8a89f-464">Aşağıdaki örnekte gösterildiği bir **Functionımportmapping** işlev eşleme ve birbirlerine yukarıda içeri aktarma işlevini kullanılan öğe:</span><span class="sxs-lookup"><span data-stu-id="8a89f-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="8a89f-465">InsertFunction öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="8a89f-466">**InsertFunction** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında bir saklı yordam Ekle işlevi bir varlık türünün veya ilişkisi kavramsal modelde eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="8a89f-467">Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="8a89f-468">Daha fazla bilgi için işlev öğesi (SSDL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-469">Değil eşlerseniz üçünü ekleme, güncelleştirme veya silme işlemleri saklı yordamlar için bir varlık türünün, çalışma zamanında yürütülüyorsa eşlenmemiş işlemleri başarısız olur ve bir UpdateException oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a89f-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="8a89f-470">**InsertFunction** öğesi ModificationFunctionMapping öğesinin bir alt öğesi olabilir ve EntityTypeMapping öğesi veya Associationsetmapping'deki öğesine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="8a89f-471">EntityTypeMapping için uygulanan InsertFunction</span><span class="sxs-lookup"><span data-stu-id="8a89f-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="8a89f-472">EntityTypeMapping öğesine uygulandığında **InsertFunction** öğesi saklı yordama ekleme işlevi kavramsal modeldeki bir varlık türünün eşler.</span><span class="sxs-lookup"><span data-stu-id="8a89f-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="8a89f-473">**InsertFunction** öğesi şu alt öğelerden uygulandığında olabilir bir **EntityTypeMapping** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="8a89f-474">İlişki ucu (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="8a89f-475">ComplexProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="8a89f-476">ResultBinding (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="8a89f-477">ScarlarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-478">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-478">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-479">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **InsertFunction** uygulandığında öğesi bir **EntityTypeMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="8a89f-480">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-480">Attribute Name</span></span>            | <span data-ttu-id="8a89f-481">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-481">Is Required</span></span> | <span data-ttu-id="8a89f-482">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-483">**functionName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-483">**FunctionName**</span></span>          | <span data-ttu-id="8a89f-484">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-484">Yes</span></span>         | <span data-ttu-id="8a89f-485">INSERT işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="8a89f-486">Saklı yordam depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="8a89f-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="8a89f-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="8a89f-488">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-488">No</span></span>          | <span data-ttu-id="8a89f-489">Etkilenen satır sayısını döndüren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="8a89f-490">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-490">Example</span></span>

<span data-ttu-id="8a89f-491">Aşağıdaki örnek Okul modelini temel alan ve gösterir **InsertFunction** ekleme işlevi için kişi varlık türünün eşlemek için kullanılan öğe **InsertPerson** saklı yordamı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="8a89f-492">**InsertPerson** saklı yordam, depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="8a89f-493">AssociationSetMapping için uygulanan InsertFunction</span><span class="sxs-lookup"><span data-stu-id="8a89f-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="8a89f-494">AssociationSetMapping öğesine uygulandığında **InsertFunction** öğesi saklı yordama ekleme işlevi kavramsal modeldeki bir ilişkilendirmenin eşler.</span><span class="sxs-lookup"><span data-stu-id="8a89f-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="8a89f-495">**InsertFunction** öğesi şu alt öğelerden uygulandığında olabilir **AssociationSetMapping** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="8a89f-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="8a89f-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-497">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-497">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-498">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **InsertFunction** için uygulandığında öğesi **AssociationSetMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="8a89f-499">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-499">Attribute Name</span></span>            | <span data-ttu-id="8a89f-500">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-500">Is Required</span></span> | <span data-ttu-id="8a89f-501">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-502">**functionName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-502">**FunctionName**</span></span>          | <span data-ttu-id="8a89f-503">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-503">Yes</span></span>         | <span data-ttu-id="8a89f-504">INSERT işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="8a89f-505">Saklı yordam depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="8a89f-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="8a89f-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="8a89f-507">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-507">No</span></span>          | <span data-ttu-id="8a89f-508">Etkilenen satırların sayısını veren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="8a89f-509">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-509">Example</span></span>

<span data-ttu-id="8a89f-510">Aşağıdaki örnek Okul modelini temel alan ve gösterir **InsertFunction** Ekle işlevini eşlemek için kullanılan öğe **CourseInstructor** ilişkilendirmeye  **InsertCourseInstructor** saklı yordamı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="8a89f-511">**InsertCourseInstructor** saklı yordam, depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="8a89f-512">Eşleme öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="8a89f-513">**Eşleme** eşleme belirtimi dili (MSL) içinde öğesi (bir depolama model içinde anlatıldığı gibi) bir veritabanına kavramsal modelde sonuna tanımlanan nesneleri eşleme bilgilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="8a89f-514">CSDL belirtimi ve SSDL belirtimi daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="8a89f-515">**Eşleme** öğesi bir eşleme belirtimi için kök öğesidir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="8a89f-516">XML ad alanı belirtimleri eşleme http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="8a89f-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="8a89f-517">Eşleme öğesi şu alt öğelerden (listelenen sırayla) sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="8a89f-518">Diğer ad (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="8a89f-519">Entitycontainermapping'indeki (tam olarak bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="8a89f-520">Kavramsal adlarını ve MSL içinde başvurulan depolama model türleri ilgili ad alanı adlarıyla nitelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="8a89f-521">Şema öğesi (CSDL) kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="8a89f-522">Şema öğesi (SSDL) depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="8a89f-523">Diğer adlar MSL içinde kullanılan ad alanları için diğer ad öğesinde ile tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-524">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-524">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-525">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **eşleme** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="8a89f-526">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-526">Attribute Name</span></span> | <span data-ttu-id="8a89f-527">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-527">Is Required</span></span> | <span data-ttu-id="8a89f-528">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="8a89f-529">**alanı**</span><span class="sxs-lookup"><span data-stu-id="8a89f-529">**Space**</span></span>      | <span data-ttu-id="8a89f-530">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-530">Yes</span></span>         | <span data-ttu-id="8a89f-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="8a89f-531">**C-S**.</span></span> <span data-ttu-id="8a89f-532">Bu, sabit bir değerdir ve değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="8a89f-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="8a89f-533">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-533">Example</span></span>

<span data-ttu-id="8a89f-534">Aşağıdaki örnekte gösterildiği bir **eşleme** Okul modelinin bir parçası üzerinde temel öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="8a89f-535">Okul modeli hakkında daha fazla bilgi için Hızlı Başlangıç (Entity Framework) bakın:</span><span class="sxs-lookup"><span data-stu-id="8a89f-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="8a89f-536">MappingFragment öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="8a89f-537">**MappingFragment** eşleme belirtimi dili (MSL) içindeki öğe kavramsal model varlık türü ve tablo veya Görünüm veritabanında özelliklerini arasındaki eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="8a89f-538">EntityType öğesi (CSDL) ve Entityset'in öğe (SSDL) kavramsal model varlık türleri ve temel alınan veritabanı tabloları veya görünümleri hakkında daha fazla bilgi için bkz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="8a89f-539">**MappingFragment** EntityTypeMapping EntitySetMapping öğesi veya alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="8a89f-540">**MappingFragment** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-541">ComplexType (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="8a89f-542">ScalarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="8a89f-543">Koşul (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-544">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-544">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-545">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **MappingFragment** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="8a89f-546">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-546">Attribute Name</span></span>          | <span data-ttu-id="8a89f-547">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-547">Is Required</span></span> | <span data-ttu-id="8a89f-548">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="8a89f-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="8a89f-550">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-550">Yes</span></span>         | <span data-ttu-id="8a89f-551">Eşlenmekte olan Görünüm ve tablo adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="8a89f-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="8a89f-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="8a89f-553">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-553">No</span></span>          | <span data-ttu-id="8a89f-554">**Doğru** veya **False** bağlı olarak yalnızca ayrı satırların olup olmadığını döndürülür.</span><span class="sxs-lookup"><span data-stu-id="8a89f-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="8a89f-555">Bu öznitelik ayarlanırsa **True**, **GenerateUpdateViews** Entitycontainermapping'indeki öğesinin özniteliği ayarlanmalıdır **False**.</span><span class="sxs-lookup"><span data-stu-id="8a89f-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="8a89f-556">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-556">Example</span></span>

<span data-ttu-id="8a89f-557">Aşağıdaki örnekte gösterildiği bir **MappingFragment** öğesi alt öğesi olarak bir **EntityTypeMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="8a89f-558">Bu örnekte, özelliklerini **kurs** kavramsal model türünde sütunları eşlendi **kurs** veritabanındaki tablo.</span><span class="sxs-lookup"><span data-stu-id="8a89f-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="8a89f-559">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-559">Example</span></span>

<span data-ttu-id="8a89f-560">Aşağıdaki örnekte gösterildiği bir **MappingFragment** öğesi alt öğesi olarak bir **EntitySetMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="8a89f-561">Özellikleri yukarıdaki örnekte olduğu gibi **kurs** kavramsal model türünde sütunları eşlendi **kurs** veritabanındaki tablo.</span><span class="sxs-lookup"><span data-stu-id="8a89f-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="8a89f-562">ModificationFunctionMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="8a89f-563">**ModificationFunctionMapping** eşleme belirtimi dili (MSL) içinde öğesi eşler ekleme, güncelleştirme ve delete işlevleri kavramsal model varlık türünün temel alınan veritabanında saklı yordamlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="8a89f-564">**ModificationFunctionMapping** öğesi, ayrıca harita INSERT ve delete işlevleri çoktan çoğa ilişkilerini kavramsal modelde temel alınan veritabanında saklı yordamlar için.</span><span class="sxs-lookup"><span data-stu-id="8a89f-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="8a89f-565">Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="8a89f-566">Daha fazla bilgi için işlev öğesi (SSDL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-567">Değil eşlerseniz üçünü ekleme, güncelleştirme veya silme işlemleri saklı yordamlar için bir varlık türünün, çalışma zamanında yürütülüyorsa eşlenmemiş işlemleri başarısız olur ve bir UpdateException oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a89f-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="8a89f-568">Devralma Hiyerarşisi içindeki bir varlık için değişiklik işlevleri için saklı yordamlar eşlenirse, hiyerarşideki tüm türler için değişiklik işlevleri için saklı yordamlar eşlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="8a89f-569">**ModificationFunctionMapping** EntityTypeMapping öğenin veya Associationsetmapping'deki öğesi bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="8a89f-570">**ModificationFunctionMapping** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-571">DeleteFunction (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="8a89f-572">InsertFunction (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="8a89f-573">UpdateFunction (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="8a89f-574">Hiçbir öznitelik geçerli olan **ModificationFunctionMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="8a89f-575">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-575">Example</span></span>

<span data-ttu-id="8a89f-576">Aşağıdaki örnek, varlık kümesi için eşleme gösterir **kişiler** varlık Okul modelde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="8a89f-577">Sütun eşlemesi için ek olarak **kişi** varlık türü, eşleme INSERT, update ve delete işlevleri, **kişi** türü gösterilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="8a89f-578">Eşleştirilmiş işlevleri depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="8a89f-579">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-579">Example</span></span>

<span data-ttu-id="8a89f-580">Aşağıdaki örnek, kümesi için eşlemesini ilişkisini gösterir. **CourseInstructor** ilişkilendirme Okul modelde ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="8a89f-581">Sütun eşlemesi için ek olarak **CourseInstructor** ilişkilendirme, INSERT ve delete işlevlerini eşleme **CourseInstructor** ilişkisi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="8a89f-582">Eşleştirilmiş işlevleri depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="8a89f-583">QueryView öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="8a89f-584">**QueryView** eşleme belirtimi dili (MSL) içindeki öğe kavramsal model ve temel alınan veritabanı tablosunda bir varlık türünün veya ilişkilendirme arasında salt okunur bir eşleme tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="8a89f-585">Eşleme depolama modelinde değerlendirilen varlık SQL sorgusu ile tanımlanır ve bir varlık veya ilişkisi kavramsal modelde açısından sonuç express.</span><span class="sxs-lookup"><span data-stu-id="8a89f-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="8a89f-586">Sorgu görünümleri salt okunur olduğundan, sorgu görünümleri tarafından tanımlanan türleri güncelleştirmek için standart güncelleştirme komutları kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="8a89f-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="8a89f-587">Değişiklik işlevlerini kullanarak bu tür için güncelleştirmeleri yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="8a89f-588">Daha fazla bilgi için bkz: nasıl yapılır: saklı yordamlar için eşlemesi değişikliği işlevleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-589">İçinde **QueryView** öğenin, içeren varlık SQL deyimleri **GroupBy**, Grup toplamları ya da gezinti özellikleri desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="8a89f-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="8a89f-590">**QueryView** EntitySetMapping öğenin veya Associationsetmapping'deki öğesi bir alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="8a89f-591">En eski bir durumda, sorgu görünümü, kavramsal modeldeki bir varlık için salt okunur eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="8a89f-592">İkinci durumda, sorgu görünümü, kavramsal modeldeki bir ilişkilendirme için bir salt okunur eşleme tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-593">Varsa **AssociationSetMapping** öğedir ilişkilendirmesine sahip bir başvuru kısıtlaması için **AssociationSetMapping** öğesi göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="8a89f-594">Daha fazla bilgi için Referentialconstraint'teki öğesi (CSDL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="8a89f-595">**QueryView** öğenin tüm alt öğeleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-596">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-596">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-597">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **QueryView** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="8a89f-598">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-598">Attribute Name</span></span> | <span data-ttu-id="8a89f-599">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-599">Is Required</span></span> | <span data-ttu-id="8a89f-600">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-601">**typeName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-601">**TypeName**</span></span>   | <span data-ttu-id="8a89f-602">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-602">No</span></span>          | <span data-ttu-id="8a89f-603">Sorgu Görünümü tarafından eşleştirilen kavramsal model türünün adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="8a89f-604">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-604">Example</span></span>

<span data-ttu-id="8a89f-605">Aşağıdaki örnekte gösterildiği **QueryView** öğesi alt öğesi olarak **EntitySetMapping** öğesi ve bir sorgu görünümü eşleme tanımlar **departmanı** varlık türünde Okul modeli.</span><span class="sxs-lookup"><span data-stu-id="8a89f-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="8a89f-606">Çünkü sorgu yalnızca üyelerin kümesini döndürür **departmanı** türü depolama modelindeki **departmanı** bu gibi eşleme'temel türü Okul modelde değiştirildi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="8a89f-607">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-607">Example</span></span>

<span data-ttu-id="8a89f-608">Sonraki örnekte gösterildiği **QueryView** öğesi alt öğesi olarak bir **AssociationSetMapping** öğesi için bir salt okunur eşleme tanımlar `FK_Course_Department` Okul modelinde ilişkilendirme.</span><span class="sxs-lookup"><span data-stu-id="8a89f-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="8a89f-609">Açıklamalar</span><span class="sxs-lookup"><span data-stu-id="8a89f-609">Comments</span></span>

<span data-ttu-id="8a89f-610">Aşağıdaki senaryoları etkinleştirmek için sorgu görünümleri tanımlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8a89f-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="8a89f-611">Bir varlık varlığın tüm özellikleri depolama modelinde içermeyen bir kavramsal model tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="8a89f-612">Bu varsayılan değerlere sahip olmayan ve desteklenmeyen özellikleri içerir **null** değerleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="8a89f-613">Depolama modelindeki hesaplanmış sütunlar kavramsal modelin varlık türlerini özelliklerine eşlenir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="8a89f-614">Burada kavramsal modeldeki bölüm varlıkları için kullanılan koşulların eşitlik bakımından dayanmayan eşlemeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8a89f-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="8a89f-615">Belirttiğinizde kullanarak koşullu eşleştirme **koşul** öğesi, belirtilen koşula belirtilen değere eşit olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="8a89f-616">Daha fazla bilgi için koşul öğesi (MSL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="8a89f-617">Aynı sütun depolama modelindeki birden çok kavramsal modeldeki eşleyin.</span><span class="sxs-lookup"><span data-stu-id="8a89f-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="8a89f-618">Birden fazla aynı tabloya eşleyin.</span><span class="sxs-lookup"><span data-stu-id="8a89f-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="8a89f-619">İlişkisel şemasında yabancı anahtarlar temel almaz kavramsal modeldeki ilişkileri tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="8a89f-620">Özel iş mantığı kavramsal modelde özelliklerin değerini ayarlamak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="8a89f-621">Örneğin, dize değeri veri kaynağında bir değer için "T" eşleyebilirsiniz **true**, kavramsal modeldeki bir Boole değeri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="8a89f-622">Sorgu sonuçları için koşullu filtrelerini tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="8a89f-623">Depolama modelinin kavramsal modeldeki veriler üzerinde daha az kısıtlamalarını uygular.</span><span class="sxs-lookup"><span data-stu-id="8a89f-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="8a89f-624">Bunu eşlenen sütun desteklemiyor olsa bile, bir özellik kavramsal modelde boş değer atanabilir yapabileceğiniz **null**değerleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="8a89f-625">Varlıkların sorgu görünümleri tanımlarken aşağıdaki maddeler geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="8a89f-626">Sorgu görünümleri salt okunurdur.</span><span class="sxs-lookup"><span data-stu-id="8a89f-626">Query views are read-only.</span></span> <span data-ttu-id="8a89f-627">Yalnızca güncelleştirme, değişiklik işlevlerini kullanarak varlıklara yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="8a89f-628">Bir sorgu görünümü tarafından bir varlık türü tanımladığınızda, ilgili tüm varlıkların sorgu görünümleri tarafından da tanımlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="8a89f-629">Çoktan çoğa ilişki bir bağlantı tablosu ilişkisel şema temsil eden depolama modelinin varlık eşlediğinizde, tanımlamalısınız bir **QueryView** öğesinde **AssociationSetMapping** Bu bağlantı tablosu için öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="8a89f-630">Tüm türlerin tür hiyerarşisi için sorgu görünümleri yeniden tanımlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="8a89f-631">Bunu aşağıdaki yöntemlerle yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8a89f-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="8a89f-632">Tek bir **QueryView** hiyerarşideki tüm varlık türlerini birleşimini döndürür tek bir varlık SQL sorgusu belirten öğe.</span><span class="sxs-lookup"><span data-stu-id="8a89f-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="8a89f-633">Tek bir **QueryView** hiyerarşi içinde bir özel varlık türü döndürmek için büyük/küçük harf işlecini kullanan tek bir varlık SQL sorgusu belirten öğesini, belirli bir koşula dayalı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="8a89f-634">Bir ek **QueryView** hiyerarşideki belirli bir tür için öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="8a89f-635">Bu durumda **TypeName** özniteliği **QueryView** her görünüm için varlık türünü belirtmek için öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="8a89f-636">Bir sorgu görünümü tanımlandığında belirtemezsiniz **StorageSetName** özniteliği **EntitySetMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="8a89f-637">Bir sorgu görünümü tanımlandığında **EntitySetMapping**öğesi de içeremez **özelliği** eşlemeleri.</span><span class="sxs-lookup"><span data-stu-id="8a89f-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="8a89f-638">ResultBinding öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="8a89f-639">**ResultBinding** eşleme belirtimi dili (MSL) içinde öğesi eşler varlık türü değişikliği işlevleri eşlendiğinde, saklı yordamlar tarafından kavramsal modeldeki varlık özellikleri için saklı döndürülen sütun değerleri yordamları temel alınan veritabanında.</span><span class="sxs-lookup"><span data-stu-id="8a89f-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="8a89f-640">Saklı yordamı, bir kimlik sütunu değeri döndürüldüğünde INSERT gibi **ResultBinding** öğesi kavramsal modeldeki bir varlık türü özelliği döndürülen değerin eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="8a89f-641">**ResultBinding** InsertFunction UpdateFunction öğesi veya alt öğesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="8a89f-642">**ResultBinding** öğenin tüm alt öğeleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-643">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-643">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-644">Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ResultBinding** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="8a89f-645">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-645">Attribute Name</span></span> | <span data-ttu-id="8a89f-646">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-646">Is Required</span></span> | <span data-ttu-id="8a89f-647">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-648">**Ad**</span><span class="sxs-lookup"><span data-stu-id="8a89f-648">**Name**</span></span>       | <span data-ttu-id="8a89f-649">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-649">Yes</span></span>         | <span data-ttu-id="8a89f-650">Eşlenmekte olan kavramsal modeldeki varlık özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="8a89f-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-651">**ColumnName**</span></span> | <span data-ttu-id="8a89f-652">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-652">Yes</span></span>         | <span data-ttu-id="8a89f-653">Eşlenmekte olan sütunun adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="8a89f-654">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-654">Example</span></span>

<span data-ttu-id="8a89f-655">Aşağıdaki örnek Okul modelini temel alan ve gösterir bir **InsertFunction** Ekle işlevini eşlemek için kullanılan öğe **kişi** varlık türüne **InsertPerson** saklı yordam.</span><span class="sxs-lookup"><span data-stu-id="8a89f-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="8a89f-656">( **InsertPerson** saklı yordam aşağıda gösterilmiştir ve depolama modelinde bildirilir.) A **ResultBinding** öğesi saklı yordam tarafından döndürülen bir sütun değeri eşlemek için kullanılır (**NewPersonID**) için bir varlık türü özelliği (**Personıd**).</span><span class="sxs-lookup"><span data-stu-id="8a89f-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="8a89f-657">Aşağıdaki Transact-SQL açıklar **InsertPerson** saklı yordam:</span><span class="sxs-lookup"><span data-stu-id="8a89f-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="8a89f-658">ResultMapping öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="8a89f-659">**ResultMapping** eşleme belirtimi dili (MSL) içindeki öğe aşağıdaki doğru olduğunda bu kavramsal modeldeki bir işlev içeri aktarma ve bir saklı yordam arasındaki eşleme temel alınan veritabanında tanımlar:</span><span class="sxs-lookup"><span data-stu-id="8a89f-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="8a89f-660">İşlev, kavramsal model varlık türünün veya karmaşık türü döndürür.</span><span class="sxs-lookup"><span data-stu-id="8a89f-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="8a89f-661">Saklı yordam tarafından döndürülen sütun adlarını özellikleri varlık türünün veya karmaşık tür adlarını tam olarak aynı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="8a89f-662">Varsayılan olarak, sütunlar arasındaki eşlemeyi bir saklı yordam tarafından döndürülen ve sütun ve özellik adları bir varlık türünün veya karmaşık türün temel alır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="8a89f-663">Sütun adları tam olarak eşleşen özellik adlarını, kullanmalısınız **ResultMapping** eşleme tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="8a89f-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="8a89f-664">Varsayılan eşleme örneği için Functionımportmapping öğesi (MSL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="8a89f-665">**ResultMapping** Functionımportmapping element alt öğesi bir öğedir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="8a89f-666">**ResultMapping** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-667">EntityTypeMapping (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="8a89f-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="8a89f-668">ComplexTypeMapping</span></span>

<span data-ttu-id="8a89f-669">Hiçbir öznitelik geçerli olan **ResultMapping** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="8a89f-670">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-670">Example</span></span>

<span data-ttu-id="8a89f-671">Aşağıdaki saklı yordamı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a89f-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="8a89f-672">Ayrıca, aşağıdaki kavramsal model varlık türünü göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8a89f-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="8a89f-673">Varlık türü tanımlanmalıdır ve sütunlar arasındaki eşlemeyi önceki varlık türünün örneğini döndüren bir işlev içeri aktarma oluşturabilmek için saklı yordam tarafından döndürülen bir **ResultMapping** öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="8a89f-674">ScalarProperty öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="8a89f-675">**ScalarProperty** eşleme belirtimi dili (MSL) içinde öğesi bir tablo sütunu veya saklı yordam parametresi temel alınan veritabanında bir özelliği bir kavramsal model varlık türü, karmaşık tür veya ilişkilendirme eşler.</span><span class="sxs-lookup"><span data-stu-id="8a89f-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="8a89f-676">Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="8a89f-677">Daha fazla bilgi için işlev öğesi (SSDL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="8a89f-678">**ScalarProperty** aşağıdaki öğelerin bir alt öğesi olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="8a89f-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="8a89f-679">MappingFragment</span></span>
-   <span data-ttu-id="8a89f-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="8a89f-680">InsertFunction</span></span>
-   <span data-ttu-id="8a89f-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="8a89f-681">UpdateFunction</span></span>
-   <span data-ttu-id="8a89f-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="8a89f-682">DeleteFunction</span></span>
-   <span data-ttu-id="8a89f-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="8a89f-683">EndProperty</span></span>
-   <span data-ttu-id="8a89f-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="8a89f-684">ComplexProperty</span></span>
-   <span data-ttu-id="8a89f-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="8a89f-685">ResultMapping</span></span>

<span data-ttu-id="8a89f-686">Bir alt öğesi olarak **MappingFragment**, **ComplexProperty**, veya **EndProperty** öğesi **ScalarProperty** öğesi bir özellik eşlemeleri veritabanı sütununa kavramsal modelde.</span><span class="sxs-lookup"><span data-stu-id="8a89f-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="8a89f-687">Bir alt öğesi olarak **InsertFunction**, **UpdateFunction**, veya **DeleteFunction** öğesi **ScalarProperty** öğesi bir özellik eşlemeleri bir saklı yordam parametresi için kavramsal modelde.</span><span class="sxs-lookup"><span data-stu-id="8a89f-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="8a89f-688">**ScalarProperty** öğenin tüm alt öğeleri olamaz.</span><span class="sxs-lookup"><span data-stu-id="8a89f-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-689">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-689">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-690">Uygulanan öznitelikleri **ScalarProperty** öğesi, öğenin rolüne bağlı olarak farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="8a89f-691">Aşağıdaki tabloda, ne zaman geçerli olan öznitelikleri açıklar **ScalarProperty** öğesi, bir veritabanı sütununa kavramsal model özelliğine eşlemek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="8a89f-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="8a89f-692">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-692">Attribute Name</span></span> | <span data-ttu-id="8a89f-693">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-693">Is Required</span></span> | <span data-ttu-id="8a89f-694">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="8a89f-695">**Ad**</span><span class="sxs-lookup"><span data-stu-id="8a89f-695">**Name**</span></span>       | <span data-ttu-id="8a89f-696">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-696">Yes</span></span>         | <span data-ttu-id="8a89f-697">Eşlenmekte olan kavramsal model özellik adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="8a89f-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-698">**ColumnName**</span></span> | <span data-ttu-id="8a89f-699">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-699">Yes</span></span>         | <span data-ttu-id="8a89f-700">Eşlenmekte olan tablo sütununun adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="8a89f-701">Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ScalarProperty** bir saklı yordam parametresi için kavramsal model özelliğine eşlenecek kullanıldığında öğesi:</span><span class="sxs-lookup"><span data-stu-id="8a89f-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="8a89f-702">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-702">Attribute Name</span></span>    | <span data-ttu-id="8a89f-703">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-703">Is Required</span></span> | <span data-ttu-id="8a89f-704">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-705">**Ad**</span><span class="sxs-lookup"><span data-stu-id="8a89f-705">**Name**</span></span>          | <span data-ttu-id="8a89f-706">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-706">Yes</span></span>         | <span data-ttu-id="8a89f-707">Eşlenmekte olan kavramsal model özellik adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="8a89f-708">**parameterName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-708">**ParameterName**</span></span> | <span data-ttu-id="8a89f-709">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-709">Yes</span></span>         | <span data-ttu-id="8a89f-710">Eşlenmekte olan parametrenin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="8a89f-711">**Sürüm**</span><span class="sxs-lookup"><span data-stu-id="8a89f-711">**Version**</span></span>       | <span data-ttu-id="8a89f-712">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-712">No</span></span>          | <span data-ttu-id="8a89f-713">**Geçerli** veya **özgün** geçerli veya özelliğin özgün değeri eşzamanlılık denetimlerinin için kullanılması gerekip gerekmediğini bağlı olarak.</span><span class="sxs-lookup"><span data-stu-id="8a89f-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="8a89f-714">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-714">Example</span></span>

<span data-ttu-id="8a89f-715">Aşağıdaki örnekte gösterildiği **ScalarProperty** iki yolla kullanılan öğe:</span><span class="sxs-lookup"><span data-stu-id="8a89f-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="8a89f-716">Özelliklerini eşlemek için **kişi** sütunlarını varlık türüne **kişi**tablo.</span><span class="sxs-lookup"><span data-stu-id="8a89f-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="8a89f-717">Özelliklerini eşlemek için **kişi** varlık türü parametrelerine **UpdatePerson** saklı yordamı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="8a89f-718">Saklı yordamları, depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="8a89f-719">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-719">Example</span></span>

<span data-ttu-id="8a89f-720">Sonraki örnekte gösterildiği **ScalarProperty** INSERT ve delete işlevleri bir kavramsal model ilişki veritabanında saklı yordamlar için kullanılan öğe.</span><span class="sxs-lookup"><span data-stu-id="8a89f-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="8a89f-721">Saklı yordamları, depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="8a89f-722">UpdateFunction öğesi (MSL)</span><span class="sxs-lookup"><span data-stu-id="8a89f-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="8a89f-723">**UpdateFunction** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında bir saklı yordam kavramsal modeldeki bir varlık türünün güncelleştirme işlevini eşleştirir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="8a89f-724">Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="8a89f-725">Daha fazla bilgi için işlev öğesi (SSDL) bakın.</span><span class="sxs-lookup"><span data-stu-id="8a89f-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="8a89f-726">Değil eşlerseniz üçünü ekleme, güncelleştirme veya silme işlemleri saklı yordamlar için bir varlık türünün, çalışma zamanında yürütülüyorsa eşlenmemiş işlemleri başarısız olur ve bir UpdateException oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8a89f-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="8a89f-727">**UpdateFunction** öğesi ModificationFunctionMapping öğesinin bir alt öğesi olabilir ve EntityTypeMapping öğeye uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8a89f-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="8a89f-728">**UpdateFunction** öğesi şu alt öğelerden olabilir:</span><span class="sxs-lookup"><span data-stu-id="8a89f-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="8a89f-729">İlişki ucu (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="8a89f-730">ComplexProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="8a89f-731">ResultBinding (sıfır veya bir)</span><span class="sxs-lookup"><span data-stu-id="8a89f-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="8a89f-732">ScarlarProperty (sıfır veya daha fazla)</span><span class="sxs-lookup"><span data-stu-id="8a89f-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="8a89f-733">Uygun öznitelikler</span><span class="sxs-lookup"><span data-stu-id="8a89f-733">Applicable Attributes</span></span>

<span data-ttu-id="8a89f-734">Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **UpdateFunction** öğesi.</span><span class="sxs-lookup"><span data-stu-id="8a89f-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="8a89f-735">Öznitelik adı</span><span class="sxs-lookup"><span data-stu-id="8a89f-735">Attribute Name</span></span>            | <span data-ttu-id="8a89f-736">Gereklidir</span><span class="sxs-lookup"><span data-stu-id="8a89f-736">Is Required</span></span> | <span data-ttu-id="8a89f-737">Değer</span><span class="sxs-lookup"><span data-stu-id="8a89f-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8a89f-738">**functionName**</span><span class="sxs-lookup"><span data-stu-id="8a89f-738">**FunctionName**</span></span>          | <span data-ttu-id="8a89f-739">Evet</span><span class="sxs-lookup"><span data-stu-id="8a89f-739">Yes</span></span>         | <span data-ttu-id="8a89f-740">Güncelleştirme işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="8a89f-741">Saklı yordam depolama modelinde bildirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="8a89f-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="8a89f-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="8a89f-743">Hayır</span><span class="sxs-lookup"><span data-stu-id="8a89f-743">No</span></span>          | <span data-ttu-id="8a89f-744">Etkilenen satırların sayısını veren çıkış parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="8a89f-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="8a89f-745">Örnek</span><span class="sxs-lookup"><span data-stu-id="8a89f-745">Example</span></span>

<span data-ttu-id="8a89f-746">Aşağıdaki örnek Okul modelini temel alan ve gösterir **UpdateFunction** güncelleştirme işlevini eşlemek için kullanılan öğe **kişi** varlık türüne **UpdatePerson** saklı yordam.</span><span class="sxs-lookup"><span data-stu-id="8a89f-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="8a89f-747">**UpdatePerson** saklı yordam, depolama modelinde bildirilir.</span><span class="sxs-lookup"><span data-stu-id="8a89f-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
