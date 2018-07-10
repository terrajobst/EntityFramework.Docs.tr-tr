---
title: MSL belirtimi - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
caps.latest.revision: 4
ms.openlocfilehash: 7448efc99f9fd9c6cdf930256a26347376fb354c
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912786"
---
# <a name="msl-specification"></a>MSL belirtimi
Eşleme belirtimi dili (MSL) kavramsal model ve depolama modeli bir Entity Framework uygulamasının arasındaki eşlemeyi açıklayan bir XML tabanlı dilidir.

Bir Entity Framework uygulamasında eşleme meta veri (MSL içinde yazılan) .msl dosyasından derleme sırasında yüklenir. Entity Framework eşleme meta veri deposu özgü komutlar için kavramsal modeline karşı sorgular çevirmek için çalışma zamanında kullanır.

Entity Framework Designer (EF Designer), tasarım zamanında bir .edmx dosyası içinde eşleme bilgilerini depolar. Derleme sırasında varlık Tasarımcısı bilgi bir .edmx dosyası içinde Entity Framework tarafından çalışma zamanında gereken .msl dosyası oluşturmak için kullanır

Tüm kavramsal adlarını veya MSL içinde başvurulan depolama model türleri ilgili ad alanı adlarıyla nitelenmelidir. Kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. [CSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz. [SSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

MSL sürümleri, XML ad alanları tarafından ayrılır.

| MSL sürümü | XML Namespace                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: schemas-microsoft-com:windows:storage:mapping:CS |
| MSL v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Diğer ad öğesinde (MSL)

**Diğer** eşleme belirtimi dili (MSL) öğedir kavramsal model ve depolama modeli ad alanları için diğer adlarını tanımlamak için kullanılan eşleme öğesi alt. Tüm kavramsal adlarını veya MSL içinde başvurulan depolama model türleri ilgili ad alanı adlarıyla nitelenmelidir. Şema öğesi (CSDL) kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. Şema öğesi (SSDL) depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz.

**Diğer** öğesi alt öğeleri olamaz.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **diğer** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | Evet         | Tarafından belirtilen ad alanı diğer **değer** özniteliği. |
| **Değer**      | Evet         | Kendisi için bir ad alanı değeri **anahtar** bir diğer ad bir öğedir.     |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **diğer** tanımlayan bir diğer öğe `c`, kavramsal modelde tanımlı türleri için.

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

## <a name="associationend-element-msl"></a>İlişki ucu öğesi (MSL)

**İlişki ucu** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında saklı yordamlar için kavramsal modeldeki bir varlık türünün değiştirilmesi işlevleri eşlendiğinde kullanılır. Saklı yordamın kullandığı bir parametre değeri bir ilişkilendirme özelliğinde tutulan bir değişiklik olursa **ilişki ucu** öğesi parametresi özellik değeri eşler. Daha fazla bilgi için aşağıdaki örnekte bakın.

Saklı yordamlar için değişiklik işlevleri varlık türleri eşleme hakkında daha fazla bilgi için bkz: ModificationFunctionMapping öğesi (MSL) ve izlenecek yol: saklı yordamlar için bir varlık eşlemesi.

**İlişki ucu** öğesi şu alt öğelerden olabilir:

-   ScalarProperty

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ilişki ucu** öğesi.

| Öznitelik adı     | Gereklidir | Değer                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Evet         | Eşlenmekte olan ilişki adı.                                                                                                                                 |
| **Kaynak**           | Evet         | Değerini **FromRole** eşlenmekte olan ilişki için karşılık gelen gezinme özelliğini özniteliği. NavigationProperty öğesi (CSDL) daha fazla bilgi için bkz. |
| **Hedef**             | Evet         | Değerini **ToRole** eşlenmekte olan ilişki için karşılık gelen gezinme özelliğini özniteliği. NavigationProperty öğesi (CSDL) daha fazla bilgi için bkz.   |

### <a name="example"></a>Örnek

Aşağıdaki kavramsal model varlık türünü göz önünde bulundurun:

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

Ayrıca, aşağıdaki depolanan yordamı göz önünde bulundurun:

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

Güncelleştirme işlevini eşlemek için `Course` varlık Bu saklı yordamı için bir değer sağlamanız gerekir **DepartmentID** parametresi. Değeri `DepartmentID` varlık türü; bir özelliğe karşılık gelmiyor olan eşleme burada gösterilen bağımsız bir ilişkide yer alır:

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

Aşağıdaki kodda gösterildiği **ilişki ucu** eşlemek için kullanılan öğe **DepartmentID** özelliği **FK\_kurs\_departmanı** koleksiyonla ilişki **UpdateCourse** saklı yordamını (hangi güncelleştirme işlevini **kurs** varlık türü eşlendi):

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

## <a name="associationsetmapping-element-msl"></a>AssociationSetMapping öğesi (MSL)

**AssociationSetMapping** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında kavramsal model ve tablo sütunları bir ilişkide arasındaki eşlemeyi tanımlar.

Kavramsal modeldeki ilişkileri, birincil ve yabancı anahtar sütunları temel alınan veritabanında özelliklerini temsil türleridir. **AssociationSetMapping** öğe iki EndProperty öğe ilişki türü özellikleri ve sütunları arasındaki eşlemeleri veritabanında tanımlamak için kullanır. Bu eşlemeler koşul öğe ile koşulları yerleştirebilirsiniz. INSERT, update ve delete işlevleri ilişkileri için saklı yordamları veritabanında ModificationFunctionMapping öğesiyle eşlenir. Salt okunur eşlemeleri arasındaki ilişkilendirmeleri ve tablo sütunları bir QueryView öğesinde bir varlık SQL dizesi kullanarak tanımlayın.

> [!NOTE]
> Başvurusal Kısıt bir ilişkisi kavramsal modelde tanımlı ise ilişkilendirme ile eşlenmesi gerekmez bir **AssociationSetMapping** öğesi. Varsa bir **AssociationSetMapping** ilişkilendirme için bir başvuru kısıtlamasını sahip öğe varsa, tanımlanan eşlemeler **AssociationSetMapping** öğesi yok sayılacak. Daha fazla bilgi için Referentialconstraint'teki öğesi (CSDL) bakın.

**AssociationSetMapping** öğesi şu alt öğelerden olabilir

-   QueryView (sıfır veya bir)
-   EndProperty (sıfır veya iki)
-   Koşul (sıfır veya daha fazla)
-   ModificationFunctionMapping (sıfır veya bir)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **AssociationSetMapping** öğesi.

| Öznitelik adı     | Gereklidir | Değer                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Ad**           | Evet         | Eşlenmekte olan kavramsal model ilişki kümesi adı.                      |
| **TypeName**       | Hayır          | Eşlenmekte olan kavramsal model ilişkilendirme türü ad alanıyla nitelenen adı. |
| **StoreEntitySet** | Hayır          | Eşlenmekte olan tablonun adı.                                                 |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **AssociationSetMapping** hangi öğesinde **FK\_kurs\_departmanı** ilişkisi kavramsal modelde ayarlamak için eşlendi **Kurs** veritabanındaki tablo. İlişki türü özellikleri ve tablo sütunları arasındaki eşlemeleri alt belirtilen **EndProperty** öğeleri.

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

## <a name="complexproperty-element-msl"></a>ComplexProperty öğesi (MSL)

A **ComplexProperty** eşleme belirtimi dili (MSL) içindeki öğe bir karmaşık tür özelliği temel alınan veritabanında bir kavramsal model varlık türü ve tablo sütunu arasındaki eşlemeyi tanımlar. Özellik sütun eşlemelerini alt ScalarProperty öğesinde belirtilir.

**ComplexType** özellik öğesi şu alt öğelerden sahip olabilir:

-   ScalarProperty (sıfır veya daha fazla)
-   **ComplexProperty** (sıfır veya daha fazla)
-   ComplextTypeMapping (sıfır veya daha fazla)
-   Koşul (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ComplexProperty** öğesi:

| Öznitelik adı | Gereklidir | Değer                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Eşlenmekte olan kavramsal modeldeki bir varlık türünün karmaşık özelliğin adı. |
| **TypeName**   | Hayır          | Kavramsal model özellik türü ad alanıyla nitelenen adı.                              |

### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modelini temel alıyor. Aşağıdaki karmaşık türü için kavramsal model eklenmiştir:

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

**LastName** ve **FirstName** özelliklerini **kişi** varlık türü karmaşık bir özellik ile değiştirildi **adı**:

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

Aşağıdaki MSL gösterildiği **ComplexProperty** eşlemek için kullanılan öğe **adı** özelliği temel alınan veritabanında sütunlara:

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

## <a name="complextypemapping-element-msl"></a>ComplexTypeMapping öğesi (MSL)

**ComplexTypeMapping** eşleme belirtimi dili (MSL) içindeki öğe ResultMapping öğesinin bir alt öğesidir ve temel kavramsal modeldeki bir işlev içeri aktarma ve bir saklı yordam arasındaki eşlemeyi tanımlar. Veritabanı aşağıdaki doğru olduğunda:

-   İşlev kavramsal karmaşık bir tür döndürür.
-   Saklı yordam tarafından döndürülen sütun adlarını tam olarak karmaşık tür özellikleri adlarını eşleştirin.

Varsayılan olarak, sütunlar arasındaki eşlemeyi bir saklı yordam tarafından döndürülen ve sütun ve özellik adları üzerinde karmaşık bir tür alır. Sütun adları tam olarak eşleşen özellik adlarını, kullanmalısınız **ComplexTypeMapping** eşleme tanımlamak için. Varsayılan eşleme örneği için Functionımportmapping öğesi (MSL) bakın.

**ComplexTypeMapping** öğesi şu alt öğelerden olabilir:

-   ScalarProperty (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ComplexTypeMapping** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **TypeName**   | Evet         | Eşlenmekte olan bir karmaşık tür ad alanıyla nitelenen adı. |

### <a name="example"></a>Örnek

Aşağıdaki saklı yordamı göz önünde bulundurun:

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

Ayrıca, aşağıdaki kavramsal model karmaşık tür göz önünde bulundurun:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Varlık türü tanımlanmalıdır ve sütunlar arasındaki eşlemeyi önceki karmaşık türün örneğini döndüren bir işlev içeri aktarma oluşturabilmek için saklı yordam tarafından döndürülen bir **ComplexTypeMapping** öğesi:

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

## <a name="condition-element-msl"></a>Koşul öğesi (MSL)

**Koşul** eşleme belirtimi dili (MSL) içindeki öğenin kavramsal model ve temel alınan veritabanı arasındaki eşlemeleri üzerinde koşullar yerleştirir. Bir XML düğümü içinde tanımlanan tüm geçerli koşullar, belirtilen alt öğesi olarak eşlemedir **koşul** öğe karşılandığı. Aksi halde, eşleme geçerli değil. Örneğin, bir veya daha fazla MappingFragment öğe içeriyorsa, **koşul** alt öğeleri eşleme içinde tanımlanan **MappingFragment** düğümü yalnızca tüm geçerli olacaktır altkoşulları **Koşul** öğe karşılandığı.

Her koşul için ya da uygulayabilirsiniz bir **adı** (bir kavramsal model varlık özelliği tarafından belirtilen adı **adı** özniteliği), veya bir **ColumnName** (adını bir sütun Belirtilen veritabanı **ColumnName** özniteliği). Zaman **adı** özniteliği, bir varlık özelliğinin değeri koşul denetlenir. Zaman **ColumnName** özniteliği, bir sütun değeri koşul denetlenir. Yalnızca biri **adı** veya **ColumnName** özniteliği belirtilebilir bir **koşul** öğesi.

> [!NOTE]
> Zaman **koşul** öğe Functionımportmapping element içinde yalnızca kullanılan **adı** özniteliği geçerli değil.

**Koşul** aşağıdaki öğelerin bir alt öğesi olabilir:

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

**Koşul** öğesi alt öğe yok olabilir.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **koşul** öğesi:

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | Hayır          | Değeri koşulu değerlendirmek için kullanılabilecek tablo sütununun adı.                                                                                                                                                                                                                   |
| **IsNull**     | Hayır          | **Doğru** veya **False**. Değer ise **True** ve sütun değeri **null**, veya değer ise **False** ve sütun değeri değil **null**, koşul true olduğu . Aksi halde koşul false olur. <br/> **IsNull** ve **değer** özniteliklerine aynı anda kullanılamaz. |
| **Değer**      | Hayır          | Sütun değeri karşılaştırılacağı değeri. Değerler aynıysa koşul true'dur. Aksi halde koşul false olur. <br/> **IsNull** ve **değer** özniteliklerine aynı anda kullanılamaz.                                                                       |
| **Ad**       | Hayır          | Değeri koşulu değerlendirmek için kullanılan kavramsal model varlık özelliğinin adı. <br/> Bu özniteliği geçerli değil, **koşul** öğesi içinde bir Functionımportmapping element kullanılır.                                                                           |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği **koşul** öğeleri alt öğeleri olarak **MappingFragment** öğeleri. Zaman **İşeAlmaTarihi** null değil ve **EnrollmentDate** olan veri arasında null eşlendi **SchoolModel.Instructor** türü ve **Personıd**ve **İşeAlmaTarihi** sütunlarının **kişi** tablo. Zaman **EnrollmentDate** null değil ve **İşeAlmaTarihi** olan veri arasında null eşlendi **SchoolModel.Student** türü ve **Personıd** ve **kayıt** sütunlarının **kişi** tablo.

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

## <a name="deletefunction-element-msl"></a>DeleteFunction öğesi (MSL)

**DeleteFunction** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında bir saklı yordam için bir varlık türünün veya ilişkisi kavramsal modelde silme işlevini eşlemeleri. Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir. Daha fazla bilgi için işlev öğesi (SSDL) bakın.

> [!NOTE]
> Değil eşlerseniz üçünü ekleme, güncelleştirme veya silme işlemleri saklı yordamlar için bir varlık türünün, çalışma zamanında yürütülüyorsa eşlenmemiş işlemleri başarısız olur ve bir UpdateException oluşturulur.

### <a name="deletefunction-applied-to-entitytypemapping"></a>EntityTypeMapping için uygulanan DeleteFunction

EntityTypeMapping öğesine uygulandığında **DeleteFunction** öğesi için bir saklı yordam kavramsal modeldeki bir varlık türünün silme işlevini eşler.

**DeleteFunction** öğesi şu alt öğelerden uygulandığında olabilir bir **EntityTypeMapping** öğesi:

-   İlişki ucu (sıfır veya daha fazla)
-   ComplexProperty (sıfır veya daha fazla)
-   ScarlarProperty (sıfır veya daha fazla)

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **DeleteFunction** için uygulandığında öğesi bir **EntityTypeMapping** öğesi.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Evet         | Silme işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı. Saklı yordam depolama modelinde bildirilmesi gerekir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satırların sayısını veren çıkış parametresinin adı.                                                                               |

#### <a name="example"></a>Örnek

Aşağıdaki örnek Okul modelini temel alan ve gösterir **DeleteFunction** silme işlevini eşleme öğesi **kişi** varlık türüne **DeletePerson** saklı yordam. **DeletePerson** saklı yordam, depolama modelinde bildirilir.

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

### <a name="deletefunction-applied-to-associationsetmapping"></a>AssociationSetMapping için uygulanan DeleteFunction

AssociationSetMapping öğesine uygulandığında **DeleteFunction** öğesi saklı yordama ilişkilendirme kavramsal modeldeki silme işlevini eşlemeleri.

**DeleteFunction** öğesi şu alt öğelerden uygulandığında olabilir **AssociationSetMapping** öğesi:

-   EndProperty

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **DeleteFunction** için uygulandığında öğesi **AssociationSetMapping** öğesi.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Evet         | Silme işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı. Saklı yordam depolama modelinde bildirilmesi gerekir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satırların sayısını veren çıkış parametresinin adı.                                                                               |

#### <a name="example"></a>Örnek

Aşağıdaki örnek Okul modelini temel alan ve gösterir **DeleteFunction** silme işlevini eşlemek için kullanılan öğe **CourseInstructor** ilişkilendirmeye  **DeleteCourseInstructor** saklı yordamı. **DeleteCourseInstructor** saklı yordam, depolama modelinde bildirilir.

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

## <a name="endproperty-element-msl"></a>EndProperty öğesi (MSL)

**EndProperty** eşleme belirtimi dili (MSL) içindeki öğe sona veya değişiklik işlevi bir kavramsal model ilişkisi ve temel alınan veritabanı arasındaki eşlemeyi tanımlar. Özellik sütun eşleme, bir alt ScalarProperty öğesinde belirtilir.

Olduğunda bir **EndProperty** öğe eşleme için kavramsal model ilişki sonu tanımlamak için kullanılır, AssociationSetMapping öğesi bir alt öğesidir. Zaman **EndProperty** öğesi eşleme için kavramsal model ilişki değişikliği işlevi tanımlamak için kullanıldığında, bir InsertFunction DeleteFunction öğesi veya bir alt öğesidir.

**EndProperty** öğesi şu alt öğelerden olabilir:

-   ScalarProperty (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **EndProperty** öğesi:

| Öznitelik adı | Gereklidir | Değer                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Ad           | Evet         | Eşlenmekte olan ilişki sonu adı. |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **AssociationSetMapping** hangi öğesinde **FK\_kurs\_departmanı** ilişkisi kavramsal modelde içineşlenmiş**Kurs** veritabanındaki tablo. İlişki türü özellikleri ve tablo sütunları arasındaki eşlemeleri alt belirtilen **EndProperty** öğeleri.

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

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği **EndProperty** ilişkilendirme INSERT ve delete işlevlerini eşleme öğesi (**CourseInstructor**) temel alınan veritabanında saklı yordamlar. Eşleştirilmiş işlevleri depolama modelinde bildirilir.

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

## <a name="entitycontainermapping-element-msl"></a>Entitycontainermapping'indeki öğesi (MSL)

**Entitycontainermapping'indeki** eşleme belirtimi dili (MSL) içindeki öğe depolama modelinin varlık kapsayıcısı kavramsal modeldeki varlık kapsayıcısı eşler. **Entitycontainermapping'indeki** eşleme öğesi bir alt öğesidir.

**Entitycontainermapping'indeki** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   EntitySetMapping (sıfır veya daha fazla)
-   AssociationSetMapping (sıfır veya daha fazla)
-   Functionımportmapping (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **Entitycontainermapping'indeki** öğesi.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Evet         | Eşlenmekte olan depolama modelinin varlık kapsayıcısının adı.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Evet         | Eşlenmekte olan kavramsal model varlık kapsayıcısının adı.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | Hayır          | **Doğru** veya **False**. Varsa **False**, hiçbir güncelleştirme görünümleri oluşturulur. Bu öznitelik ayarlanmalıdır **False** verileri başarıyla dönmez olabilir çünkü, geçersiz olacak salt okunur bir eşleme varsa. <br/> Varsayılan değer **True**. |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **Entitycontainermapping'indeki** eşleşen öğe **SchoolModelEntities** (kavramsal model varlık kapsayıcısı) kapsayıcıya  **SchoolModelStoreContainer** kapsayıcı (depolama modelinin varlık kapsayıcısı):

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

## <a name="entitysetmapping-element-msl"></a>EntitySetMapping öğesi (MSL)

**EntitySetMapping** eşleme belirtimi dili (MSL) eşlemeleri kavramsal model varlıktaki tüm türleri varlık kümesi içindeki öğe depolama modelinde ayarlar. Bir varlık kavramsal modelde kümesi için bir mantıksal kapsayıcıdır örnekleri varlık aynı türde (ve türetilen türler). Varlık kümesi depolama modelinde, bir tablo veya Görünüm temel alınan veritabanında temsil eder. Kavramsal model varlık kümesini değeri tarafından belirtilen **adı** özniteliği **EntitySetMapping** öğesi. Eşlenen için tablo veya görünüm tarafından belirtilen **StoreEntitySet** her alt MappingFragment öğe veya öznitelik **EntitySetMapping** öğenin kendisinin.

**EntitySetMapping** öğesi şu alt öğelerden olabilir:

-   EntityTypeMapping (sıfır veya daha fazla)
-   QueryView (sıfır veya bir)
-   MappingFragment (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntitySetMapping** öğesi.

| Öznitelik adı           | Gereklidir | Değer                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                 | Evet         | Eşlenmekte olan kavramsal model varlık kümesinin adı.                                                                                                                                                             |
| **TypeName** **1**       | Hayır          | Eşlenmekte olan kavramsal model varlık türünün adı.                                                                                                                                                            |
| **StoreEntitySet** **1** | Hayır          | İçin eşlenen depolama modelinin varlık kümesinin adı.                                                                                                                                                             |
| **MakeColumnsDistinct**  | Hayır          | **Doğru** veya **False** bağlı olarak yalnızca ayrı satırların olup olmadığını döndürülür. <br/> Bu öznitelik ayarlanırsa **True**, **GenerateUpdateViews** Entitycontainermapping'indeki öğesinin özniteliği ayarlanmalıdır **False**. |

 

**1** **TypeName** ve **StoreEntitySet** öznitelikleri eşleme tek bir tabloya bir tek bir varlık türü için EntityTypeMapping ve MappingFragment alt öğeleri yerine kullanılabilir.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntitySetMapping** üç tür (bir taban türü ve iki türetilmiş türler) içinde eşleşen öğe **kursları** üç farklı tabloda için kavramsal modelin varlık kümesi temel alınan veritabanı. Tabloları tarafından belirtilen **StoreEntitySet** her öznitelik **MappingFragment** öğesi.

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

## <a name="entitytypemapping-element-msl"></a>EntityTypeMapping öğesi (MSL)

**EntityTypeMapping** eşleme belirtimi dili (MSL) içinde öğe, temel alınan veritabanında bir varlık türü kavramsal model ve tabloları veya görünümleri arasındaki eşlemeyi tanımlar. EntityType öğesi (CSDL) ve Entityset'in öğe (SSDL) kavramsal model varlık türleri ve temel alınan veritabanı tabloları veya görünümleri hakkında daha fazla bilgi için bkz. Eşlenmekte olan kavramsal model varlık türü tarafından belirtilen **TypeName** özniteliği **EntityTypeMapping** öğesi. Eşlenmekte olan Görünüm ve tablo tarafından belirtilen **StoreEntitySet** alt MappingFragment öğesinin özniteliği.

Alt Öğe Ekle eşlemek için kullanılan ModificationFunctionMapping update veya delete işlevleri veritabanında saklı yordamlar için varlık türleri.

**EntityTypeMapping** öğesi şu alt öğelerden olabilir:

-   MappingFragment (sıfır veya daha fazla)
-   ModificationFunctionMapping (sıfır veya bir)
-   ScalarProperty
-   Koşul

> [!NOTE]
> **MappingFragment** ve **ModificationFunctionMapping** öğeleri alt öğeleri olamaz **EntityTypeMapping** aynı anda öğesi.


> [!NOTE]
> **ScalarProperty** ve **koşul** öğeleri alt öğelerinin yalnızca olabilir **EntityTypeMapping** bir Functionımportmapping element içinde kullanıldığında öğesi.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntityTypeMapping** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **TypeName**   | Evet         | Eşlenmekte olan kavramsal model varlık türü ad alanıyla nitelenen adı. <br/> Tür abstract veya türetilmiş bir tür ise, değer olmalıdır `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir iki alt EntitySetMapping öğeyle gösterir **EntityTypeMapping** öğeleri. İlk **EntityTypeMapping** öğesi **SchoolModel.Person** varlık türü eşlenmiş durumda **kişi** tablo. İkinci **EntityTypeMapping** öğesinde, güncelleştirme işlevleri **SchoolModel.Person** türü bir saklı yordam için eşlenmiş **UpdatePerson**, veritabanındaki .

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

### <a name="example"></a>Örnek

Sonraki örnek, kök türü soyut bir tür hiyerarşisi eşleme gösterir. Kullanımına dikkat edin `IsOfType` söz diziminin **TypeName** öznitelikleri.

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

## <a name="functionimportmapping-element-msl"></a>Functionımportmapping Element (MSL)

**Functionımportmapping** eşleme belirtimi dili (MSL) içindeki öğe, temel alınan veritabanında bir işlev içeri aktarma kavramsal model ve bir saklı yordam veya işlev arasındaki eşlemeyi tanımlar. Kavramsal modelde işlevi içeri aktarmalar bildirilmelidir ve saklı yordamlar depolama modelinde bildirilmesi gerekir. Daha fazla bilgi için Functionımport öğesi (CSDL) ve işlev öğesi (SSDL) bakın.

> [!NOTE]
> Bir işlev içeri aktarma kavramsal model varlık türünün veya karmaşık türü döndürürse, varsayılan olarak, ardından temel alınan bir saklı yordam tarafından döndürülen sütun adlarını tam olarak kavramsal model türündeki özellikleri adları eşleşmelidir. Sütun adlarını tam olarak eşleşen özellik adlarını, eşleme bir ResultMapping öğesinde tanımlanmış olması gerekir.

**Functionımportmapping** öğesi şu alt öğelerden olabilir:

-   ResultMapping (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **Functionımportmapping** öğesi:

| Öznitelik adı         | Gereklidir | Değer                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Evet         | Eşlenmekte olan kavramsal modeldeki işlevi içeri aktarma adı.           |
| **FunctionName**       | Evet         | Eşlenmekte olan depolama modelinde işlevi ad alanıyla nitelenen adı. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modelini temel alıyor. Aşağıdaki işlev depolama modelinde göz önünde bulundurun:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Ayrıca, bu işlev içeri aktarma kavramsal modeldeki göz önünde bulundurun:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

Aşağıdaki örnekte gösterildiği bir **Functionımportmapping** işlev eşleme ve birbirlerine yukarıda içeri aktarma işlevini kullanılan öğe:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>InsertFunction öğesi (MSL)

**InsertFunction** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında bir saklı yordam Ekle işlevi bir varlık türünün veya ilişkisi kavramsal modelde eşleştirir. Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir. Daha fazla bilgi için işlev öğesi (SSDL) bakın.

> [!NOTE]
> Değil eşlerseniz üçünü ekleme, güncelleştirme veya silme işlemleri saklı yordamlar için bir varlık türünün, çalışma zamanında yürütülüyorsa eşlenmemiş işlemleri başarısız olur ve bir UpdateException oluşturulur.

**InsertFunction** öğesi ModificationFunctionMapping öğesinin bir alt öğesi olabilir ve EntityTypeMapping öğesi veya Associationsetmapping'deki öğesine uygulanır.

### <a name="insertfunction-applied-to-entitytypemapping"></a>EntityTypeMapping için uygulanan InsertFunction

EntityTypeMapping öğesine uygulandığında **InsertFunction** öğesi saklı yordama ekleme işlevi kavramsal modeldeki bir varlık türünün eşler.

**InsertFunction** öğesi şu alt öğelerden uygulandığında olabilir bir **EntityTypeMapping** öğesi:

-   İlişki ucu (sıfır veya daha fazla)
-   ComplexProperty (sıfır veya daha fazla)
-   ResultBinding (sıfır veya bir)
-   ScarlarProperty (sıfır veya daha fazla)

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **InsertFunction** uygulandığında öğesi bir **EntityTypeMapping** öğesi.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Evet         | INSERT işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı. Saklı yordam depolama modelinde bildirilmesi gerekir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satır sayısını döndüren çıkış parametresinin adı.                                                                               |

#### <a name="example"></a>Örnek

Aşağıdaki örnek Okul modelini temel alan ve gösterir **InsertFunction** ekleme işlevi için kişi varlık türünün eşlemek için kullanılan öğe **InsertPerson** saklı yordamı. **InsertPerson** saklı yordam, depolama modelinde bildirilir.

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
### <a name="insertfunction-applied-to-associationsetmapping"></a>AssociationSetMapping için uygulanan InsertFunction

AssociationSetMapping öğesine uygulandığında **InsertFunction** öğesi saklı yordama ekleme işlevi kavramsal modeldeki bir ilişkilendirmenin eşler.

**InsertFunction** öğesi şu alt öğelerden uygulandığında olabilir **AssociationSetMapping** öğesi:

-   EndProperty

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **InsertFunction** için uygulandığında öğesi **AssociationSetMapping** öğesi.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Evet         | INSERT işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı. Saklı yordam depolama modelinde bildirilmesi gerekir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satırların sayısını veren çıkış parametresinin adı.                                                                               |

#### <a name="example"></a>Örnek

Aşağıdaki örnek Okul modelini temel alan ve gösterir **InsertFunction** Ekle işlevini eşlemek için kullanılan öğe **CourseInstructor** ilişkilendirmeye  **InsertCourseInstructor** saklı yordamı. **InsertCourseInstructor** saklı yordam, depolama modelinde bildirilir.

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

## <a name="mapping-element-msl"></a>Eşleme öğesi (MSL)

**Eşleme** eşleme belirtimi dili (MSL) içinde öğesi (bir depolama model içinde anlatıldığı gibi) bir veritabanına kavramsal modelde sonuna tanımlanan nesneleri eşleme bilgilerini içerir. CSDL belirtimi ve SSDL belirtimi daha fazla bilgi için bkz.

**Eşleme** öğesi bir eşleme belirtimi için kök öğesidir. XML ad alanı belirtimleri eşleme http://schemas.microsoft.com/ado/2009/11/mapping/cs.

Eşleme öğesi şu alt öğelerden (listelenen sırayla) sahip olabilir:

-   Diğer ad (sıfır veya daha fazla)
-   Entitycontainermapping'indeki (tam olarak bir)

Kavramsal adlarını ve MSL içinde başvurulan depolama model türleri ilgili ad alanı adlarıyla nitelenmelidir. Şema öğesi (CSDL) kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. Şema öğesi (SSDL) depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz. Diğer adlar MSL içinde kullanılan ad alanları için diğer ad öğesinde ile tanımlanabilir.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **eşleme** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Alanı**      | Evet         | **C-S**. Bu, sabit bir değerdir ve değiştirilemez. |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **eşleme** Okul modelinin bir parçası üzerinde temel öğesi. Okul modeli hakkında daha fazla bilgi için Hızlı Başlangıç (Entity Framework) bakın:

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

## <a name="mappingfragment-element-msl"></a>MappingFragment öğesi (MSL)

**MappingFragment** eşleme belirtimi dili (MSL) içindeki öğe kavramsal model varlık türü ve tablo veya Görünüm veritabanında özelliklerini arasındaki eşlemeyi tanımlar. EntityType öğesi (CSDL) ve Entityset'in öğe (SSDL) kavramsal model varlık türleri ve temel alınan veritabanı tabloları veya görünümleri hakkında daha fazla bilgi için bkz. **MappingFragment** EntityTypeMapping EntitySetMapping öğesi veya alt öğesi olabilir.

**MappingFragment** öğesi şu alt öğelerden olabilir:

-   ComplexType (sıfır veya daha fazla)
-   ScalarProperty (sıfır veya daha fazla)
-   Koşul (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **MappingFragment** öğesi.

| Öznitelik adı          | Gereklidir | Değer                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Evet         | Eşlenmekte olan Görünüm ve tablo adı.                                                                                                                                                                           |
| **MakeColumnsDistinct** | Hayır          | **Doğru** veya **False** bağlı olarak yalnızca ayrı satırların olup olmadığını döndürülür. <br/> Bu öznitelik ayarlanırsa **True**, **GenerateUpdateViews** Entitycontainermapping'indeki öğesinin özniteliği ayarlanmalıdır **False**. |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **MappingFragment** öğesi alt öğesi olarak bir **EntityTypeMapping** öğesi. Bu örnekte, özelliklerini **kurs** kavramsal model türünde sütunları eşlendi **kurs** veritabanındaki tablo.

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

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **MappingFragment** öğesi alt öğesi olarak bir **EntitySetMapping** öğesi. Özellikleri yukarıdaki örnekte olduğu gibi **kurs** kavramsal model türünde sütunları eşlendi **kurs** veritabanındaki tablo.

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

## <a name="modificationfunctionmapping-element-msl"></a>ModificationFunctionMapping öğesi (MSL)

**ModificationFunctionMapping** eşleme belirtimi dili (MSL) içinde öğesi eşler ekleme, güncelleştirme ve delete işlevleri kavramsal model varlık türünün temel alınan veritabanında saklı yordamlar. **ModificationFunctionMapping** öğesi, ayrıca harita INSERT ve delete işlevleri çoktan çoğa ilişkilerini kavramsal modelde temel alınan veritabanında saklı yordamlar için. Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir. Daha fazla bilgi için işlev öğesi (SSDL) bakın.

> [!NOTE]
> Değil eşlerseniz üçünü ekleme, güncelleştirme veya silme işlemleri saklı yordamlar için bir varlık türünün, çalışma zamanında yürütülüyorsa eşlenmemiş işlemleri başarısız olur ve bir UpdateException oluşturulur.


> [!NOTE]
> Devralma Hiyerarşisi içindeki bir varlık için değişiklik işlevleri için saklı yordamlar eşlenirse, hiyerarşideki tüm türler için değişiklik işlevleri için saklı yordamlar eşlenmesi gerekir.

**ModificationFunctionMapping** EntityTypeMapping öğenin veya Associationsetmapping'deki öğesi bir alt öğesi olabilir.

**ModificationFunctionMapping** öğesi şu alt öğelerden olabilir:

-   DeleteFunction (sıfır veya bir)
-   InsertFunction (sıfır veya bir)
-   UpdateFunction (sıfır veya bir)

Hiçbir öznitelik geçerli olan **ModificationFunctionMapping** öğesi.

### <a name="example"></a>Örnek

Aşağıdaki örnek, varlık kümesi için eşleme gösterir **kişiler** varlık Okul modelde ayarlayın. Sütun eşlemesi için ek olarak **kişi** varlık türü, eşleme INSERT, update ve delete işlevleri, **kişi** türü gösterilir. Eşleştirilmiş işlevleri depolama modelinde bildirilir.

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

### <a name="example"></a>Örnek

Aşağıdaki örnek, kümesi için eşlemesini ilişkisini gösterir. **CourseInstructor** ilişkilendirme Okul modelde ayarlayın. Sütun eşlemesi için ek olarak **CourseInstructor** ilişkilendirme, INSERT ve delete işlevlerini eşleme **CourseInstructor** ilişkisi gösterilir. Eşleştirilmiş işlevleri depolama modelinde bildirilir.

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
 

 

## <a name="queryview-element-msl"></a>QueryView öğesi (MSL)

**QueryView** eşleme belirtimi dili (MSL) içindeki öğe kavramsal model ve temel alınan veritabanı tablosunda bir varlık türünün veya ilişkilendirme arasında salt okunur bir eşleme tanımlar. Eşleme depolama modelinde değerlendirilen varlık SQL sorgusu ile tanımlanır ve bir varlık veya ilişkisi kavramsal modelde açısından sonuç express. Sorgu görünümleri salt okunur olduğundan, sorgu görünümleri tarafından tanımlanan türleri güncelleştirmek için standart güncelleştirme komutları kullanamazsınız. Değişiklik işlevlerini kullanarak bu tür için güncelleştirmeleri yapabilirsiniz. Daha fazla bilgi için bkz: nasıl yapılır: saklı yordamlar için eşlemesi değişikliği işlevleri.

> [!NOTE]
> İçinde **QueryView** öğenin, içeren varlık SQL deyimleri **GroupBy**, Grup toplamları ya da gezinti özellikleri desteklenmez.

 

**QueryView** EntitySetMapping öğenin veya Associationsetmapping'deki öğesi bir alt öğesi olabilir. En eski bir durumda, sorgu görünümü, kavramsal modeldeki bir varlık için salt okunur eşlemeyi tanımlar. İkinci durumda, sorgu görünümü, kavramsal modeldeki bir ilişkilendirme için bir salt okunur eşleme tanımlar.

> [!NOTE]
> Varsa **AssociationSetMapping** öğedir ilişkilendirmesine sahip bir başvuru kısıtlaması için **AssociationSetMapping** öğesi göz ardı edilir. Daha fazla bilgi için Referentialconstraint'teki öğesi (CSDL) bakın.

**QueryView** öğenin tüm alt öğeleri olamaz.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **QueryView** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **TypeName**   | Hayır          | Sorgu Görünümü tarafından eşleştirilen kavramsal model türünün adı. |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği **QueryView** öğesi alt öğesi olarak **EntitySetMapping** öğesi ve bir sorgu görünümü eşleme tanımlar **departmanı** varlık türünde Okul modeli.

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

Çünkü sorgu yalnızca üyelerin kümesini döndürür **departmanı** türü depolama modelindeki **departmanı** bu gibi eşleme'temel türü Okul modelde değiştirildi:

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

### <a name="example"></a>Örnek

Sonraki örnekte gösterildiği **QueryView** öğesi alt öğesi olarak bir **AssociationSetMapping** öğesi için bir salt okunur eşleme tanımlar `FK_Course_Department` Okul modelinde ilişkilendirme.

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
 
### <a name="comments"></a>Açıklamalar

Aşağıdaki senaryoları etkinleştirmek için sorgu görünümleri tanımlayabilirsiniz:

-   Bir varlık varlığın tüm özellikleri depolama modelinde içermeyen bir kavramsal model tanımlayın. Bu varsayılan değerlere sahip olmayan ve desteklenmeyen özellikleri içerir **null** değerleri.
-   Depolama modelindeki hesaplanmış sütunlar kavramsal modelin varlık türlerini özelliklerine eşlenir.
-   Burada kavramsal modeldeki bölüm varlıkları için kullanılan koşulların eşitlik bakımından dayanmayan eşlemeyi tanımlar. Belirttiğinizde kullanarak koşullu eşleştirme **koşul** öğesi, belirtilen koşula belirtilen değere eşit olmalıdır. Daha fazla bilgi için koşul öğesi (MSL) bakın.
-   Aynı sütun depolama modelindeki birden çok kavramsal modeldeki eşleyin.
-   Birden fazla aynı tabloya eşleyin.
-   İlişkisel şemasında yabancı anahtarlar temel almaz kavramsal modeldeki ilişkileri tanımlayın.
-   Özel iş mantığı kavramsal modelde özelliklerin değerini ayarlamak için kullanın. Örneğin, dize değeri veri kaynağında bir değer için "T" eşleyebilirsiniz **true**, kavramsal modeldeki bir Boole değeri.
-   Sorgu sonuçları için koşullu filtrelerini tanımlayın.
-   Depolama modelinin kavramsal modeldeki veriler üzerinde daha az kısıtlamalarını uygular. Bunu eşlenen sütun desteklemiyor olsa bile, bir özellik kavramsal modelde boş değer atanabilir yapabileceğiniz **null**değerleri.

Varlıkların sorgu görünümleri tanımlarken aşağıdaki maddeler geçerlidir:

-   Sorgu görünümleri salt okunurdur. Yalnızca güncelleştirme, değişiklik işlevlerini kullanarak varlıklara yapabilirsiniz.
-   Bir sorgu görünümü tarafından bir varlık türü tanımladığınızda, ilgili tüm varlıkların sorgu görünümleri tarafından da tanımlamanız gerekir.
-   Çoktan çoğa ilişki bir bağlantı tablosu ilişkisel şema temsil eden depolama modelinin varlık eşlediğinizde, tanımlamalısınız bir **QueryView** öğesinde **AssociationSetMapping** Bu bağlantı tablosu için öğesi.
-   Tüm türlerin tür hiyerarşisi için sorgu görünümleri yeniden tanımlanması gerekir. Bunu aşağıdaki yöntemlerle yapabilirsiniz:
-   -   Tek bir **QueryView** hiyerarşideki tüm varlık türlerini birleşimini döndürür tek bir varlık SQL sorgusu belirten öğe.
    -   Tek bir **QueryView** hiyerarşi içinde bir özel varlık türü döndürmek için büyük/küçük harf işlecini kullanan tek bir varlık SQL sorgusu belirten öğesini, belirli bir koşula dayalı.
    -   Bir ek **QueryView** hiyerarşideki belirli bir tür için öğesi. Bu durumda **TypeName** özniteliği **QueryView** her görünüm için varlık türünü belirtmek için öğesi.
-   Bir sorgu görünümü tanımlandığında belirtemezsiniz **StorageSetName** özniteliği **EntitySetMapping** öğesi.
-   Bir sorgu görünümü tanımlandığında **EntitySetMapping**öğesi de içeremez **özelliği** eşlemeleri.

## <a name="resultbinding-element-msl"></a>ResultBinding öğesi (MSL)

**ResultBinding** eşleme belirtimi dili (MSL) içinde öğesi eşler varlık türü değişikliği işlevleri eşlendiğinde, saklı yordamlar tarafından kavramsal modeldeki varlık özellikleri için saklı döndürülen sütun değerleri yordamları temel alınan veritabanında. Saklı yordamı, bir kimlik sütunu değeri döndürüldüğünde INSERT gibi **ResultBinding** öğesi kavramsal modeldeki bir varlık türü özelliği döndürülen değerin eşleştirir.

**ResultBinding** InsertFunction UpdateFunction öğesi veya alt öğesi olabilir.

**ResultBinding** öğenin tüm alt öğeleri olamaz.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ResultBinding** öğesi:

| Öznitelik adı | Gereklidir | Değer                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Ad**       | Evet         | Eşlenmekte olan kavramsal modeldeki varlık özelliğinin adı. |
| **ColumnName** | Evet         | Eşlenmekte olan sütunun adı.                                          |

### <a name="example"></a>Örnek

Aşağıdaki örnek Okul modelini temel alan ve gösterir bir **InsertFunction** Ekle işlevini eşlemek için kullanılan öğe **kişi** varlık türüne **InsertPerson** saklı yordam. ( **InsertPerson** saklı yordam aşağıda gösterilmiştir ve depolama modelinde bildirilir.) A **ResultBinding** öğesi saklı yordam tarafından döndürülen bir sütun değeri eşlemek için kullanılır (**NewPersonID**) için bir varlık türü özelliği (**Personıd**).

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

Aşağıdaki Transact-SQL açıklar **InsertPerson** saklı yordam:

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

## <a name="resultmapping-element-msl"></a>ResultMapping öğesi (MSL)

**ResultMapping** eşleme belirtimi dili (MSL) içindeki öğe aşağıdaki doğru olduğunda bu kavramsal modeldeki bir işlev içeri aktarma ve bir saklı yordam arasındaki eşleme temel alınan veritabanında tanımlar:

-   İşlev, kavramsal model varlık türünün veya karmaşık türü döndürür.
-   Saklı yordam tarafından döndürülen sütun adlarını özellikleri varlık türünün veya karmaşık tür adlarını tam olarak aynı.

Varsayılan olarak, sütunlar arasındaki eşlemeyi bir saklı yordam tarafından döndürülen ve sütun ve özellik adları bir varlık türünün veya karmaşık türün temel alır. Sütun adları tam olarak eşleşen özellik adlarını, kullanmalısınız **ResultMapping** eşleme tanımlamak için. Varsayılan eşleme örneği için Functionımportmapping öğesi (MSL) bakın.

**ResultMapping** Functionımportmapping element alt öğesi bir öğedir.

**ResultMapping** öğesi şu alt öğelerden olabilir:

-   EntityTypeMapping (sıfır veya daha fazla)
-   ComplexTypeMapping

Hiçbir öznitelik geçerli olan **ResultMapping** öğesi.

### <a name="example"></a>Örnek

Aşağıdaki saklı yordamı göz önünde bulundurun:

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

Ayrıca, aşağıdaki kavramsal model varlık türünü göz önünde bulundurun:

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

Varlık türü tanımlanmalıdır ve sütunlar arasındaki eşlemeyi önceki varlık türünün örneğini döndüren bir işlev içeri aktarma oluşturabilmek için saklı yordam tarafından döndürülen bir **ResultMapping** öğesi:

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

## <a name="scalarproperty-element-msl"></a>ScalarProperty öğesi (MSL)

**ScalarProperty** eşleme belirtimi dili (MSL) içinde öğesi bir tablo sütunu veya saklı yordam parametresi temel alınan veritabanında bir özelliği bir kavramsal model varlık türü, karmaşık tür veya ilişkilendirme eşler.

> [!NOTE]
> Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir. Daha fazla bilgi için işlev öğesi (SSDL) bakın.

**ScalarProperty** aşağıdaki öğelerin bir alt öğesi olabilir:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Bir alt öğesi olarak **MappingFragment**, **ComplexProperty**, veya **EndProperty** öğesi **ScalarProperty** öğesi bir özellik eşlemeleri veritabanı sütununa kavramsal modelde. Bir alt öğesi olarak **InsertFunction**, **UpdateFunction**, veya **DeleteFunction** öğesi **ScalarProperty** öğesi bir özellik eşlemeleri bir saklı yordam parametresi için kavramsal modelde.

**ScalarProperty** öğenin tüm alt öğeleri olamaz.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Uygulanan öznitelikleri **ScalarProperty** öğesi, öğenin rolüne bağlı olarak farklılık gösterir.

Aşağıdaki tabloda, ne zaman geçerli olan öznitelikleri açıklar **ScalarProperty** öğesi, bir veritabanı sütununa kavramsal model özelliğine eşlemek için kullanılır:

| Öznitelik adı | Gereklidir | Değer                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Ad**       | Evet         | Eşlenmekte olan kavramsal model özellik adı. |
| **ColumnName** | Evet         | Eşlenmekte olan tablo sütununun adı.              |

Aşağıdaki tabloda, geçerli olan öznitelikleri açıklar **ScalarProperty** bir saklı yordam parametresi için kavramsal model özelliğine eşlenecek kullanıldığında öğesi:

| Öznitelik adı    | Gereklidir | Değer                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**          | Evet         | Eşlenmekte olan kavramsal model özellik adı.                                                                                 |
| **ParameterName** | Evet         | Eşlenmekte olan parametrenin adı.                                                                                                 |
| **Sürüm**       | Hayır          | **Geçerli** veya **özgün** geçerli veya özelliğin özgün değeri eşzamanlılık denetimlerinin için kullanılması gerekip gerekmediğini bağlı olarak. |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği **ScalarProperty** iki yolla kullanılan öğe:

-   Özelliklerini eşlemek için **kişi** sütunlarını varlık türüne **kişi**tablo.
-   Özelliklerini eşlemek için **kişi** varlık türü parametrelerine **UpdatePerson** saklı yordamı. Saklı yordamları, depolama modelinde bildirilir.

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

### <a name="example"></a>Örnek

Sonraki örnekte gösterildiği **ScalarProperty** INSERT ve delete işlevleri bir kavramsal model ilişki veritabanında saklı yordamlar için kullanılan öğe. Saklı yordamları, depolama modelinde bildirilir.

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

## <a name="updatefunction-element-msl"></a>UpdateFunction öğesi (MSL)

**UpdateFunction** eşleme belirtimi dili (MSL) içindeki öğe temel alınan veritabanında bir saklı yordam kavramsal modeldeki bir varlık türünün güncelleştirme işlevini eşleştirir. Hangi değişiklik işlevleri eşlenmiş saklı yordamlar depolama modelinde bildirilmesi gerekir. Daha fazla bilgi için işlev öğesi (SSDL) bakın.

> [!NOTE]
>  Değil eşlerseniz üçünü ekleme, güncelleştirme veya silme işlemleri saklı yordamlar için bir varlık türünün, çalışma zamanında yürütülüyorsa eşlenmemiş işlemleri başarısız olur ve bir UpdateException oluşturulur.

**UpdateFunction** öğesi ModificationFunctionMapping öğesinin bir alt öğesi olabilir ve EntityTypeMapping öğeye uygulanır.

**UpdateFunction** öğesi şu alt öğelerden olabilir:

-   İlişki ucu (sıfır veya daha fazla)
-   ComplexProperty (sıfır veya daha fazla)
-   ResultBinding (sıfır veya bir)
-   ScarlarProperty (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **UpdateFunction** öğesi.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Evet         | Güncelleştirme işlevi için eşlenmiş saklı yordam ad alanıyla nitelenen adı. Saklı yordam depolama modelinde bildirilmesi gerekir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satırların sayısını veren çıkış parametresinin adı.                                                                               |

### <a name="example"></a>Örnek

Aşağıdaki örnek Okul modelini temel alan ve gösterir **UpdateFunction** güncelleştirme işlevini eşlemek için kullanılan öğe **kişi** varlık türüne **UpdatePerson** saklı yordam. **UpdatePerson** saklı yordam, depolama modelinde bildirilir.

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
