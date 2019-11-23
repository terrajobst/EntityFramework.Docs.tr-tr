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
# <a name="msl-specification"></a>MSL Belirtimi
Eşleme belirtim dili (MSL), bir Entity Framework uygulamasının kavramsal modeli ve depolama modeli arasındaki eşlemeyi açıklayan XML tabanlı bir dildir.

Entity Framework uygulamasında, eşleme meta verileri, derleme zamanında bir. msl dosyasından (MSL 'te yazılmıştır) yüklenir. Entity Framework, sorguları, verileri depoya özel komutlara dönüştürmek için çalışma zamanında eşleme meta verilerini kullanır.

Entity Framework Designer (EF Designer), eşleme bilgilerini tasarım zamanında bir. edmx dosyasında depolar. Derleme zamanında Entity Desisgner, çalışma zamanında Entity Framework gereken. msl dosyasını oluşturmak için bir. edmx dosyasındaki bilgileri kullanır

MSL 'de başvurulan tüm kavramsal veya depolama modeli türlerinin adları, ilgili ad alanı adlarıyla nitelenmelidir. Kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. [csdl belirtimi](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz. [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

MSL 'nin sürümleri, XML ad alanları ile farklılaştırılabilir.

| MSL sürümü | XML ad alanı                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: schemas-microsoft-com: Windows: Storage: eşleme: CS |
| MSL v2      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| MSL v3      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Alias öğesi (MSL)

Eşleme belirtim dili (MSL) içindeki **diğer ad** öğesi, kavramsal model ve depolama modeli ad alanları için diğer adları tanımlamak üzere kullanılan mapping öğesinin bir alt öğesidir. MSL 'de başvurulan tüm kavramsal veya depolama modeli türlerinin adları, ilgili ad alanı adlarıyla nitelenmelidir. Kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. şema öğesi (CSDL). Depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz. şema öğesi (SSDL).

**Alias** öğesi alt öğeleri içeremez.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **diğer ad** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Anahtar**        | Evet         | **Değer** özniteliği tarafından belirtilen ad alanı için diğer ad. |
| **Değer**      | Evet         | **Anahtar** öğesi değerinin bir diğer adı olduğu ad alanı.     |

### <a name="example"></a>Örnek

Aşağıdaki örnek, kavramsal modelde tanımlanan türler için `c`diğer adı tanımlayan bir **diğer ad** öğesini gösterir.

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

## <a name="associationend-element-msl"></a>AssociationEnd öğesi (MSL)

Kavramsal modeldeki bir varlık türünün değiştirme işlevleri, temel alınan veritabanındaki Saklı yordamlarla eşlendiğinde, eşleme belirtim dili (MSL) içindeki **Associationend** öğesi kullanılır. Bir değişiklik saklı yordamı değeri bir ilişkilendirme özelliğinde tutulan bir parametre alırsa, **Associationend** öğesi özellik değerini parametresine Eşler. Daha fazla bilgi için aşağıdaki örnekte bakın.

Varlık türlerinin değişiklik işlevlerini saklı yordamlara eşleme hakkında daha fazla bilgi için bkz. ModificationFunctionMapping öğesi (MSL) ve Izlenecek yol: bir varlığı saklı yordamlara eşleme.

**Associationend** öğesi aşağıdaki alt öğelere sahip olabilir:

-   ScalarProperty

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Associationend** öğesi için geçerli olan öznitelikler açıklanmaktadır.

| Öznitelik adı     | Gereklidir | Değer                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Evet         | Eşlenmekte olan ilişkilendirmenin adı.                                                                                                                                 |
| **From**           | Evet         | Eşlenen ilişkiye karşılık gelen Gezinti özelliğinin **FromRole** özniteliğinin değeri. Daha fazla bilgi için bkz. NavigationProperty öğesi (CSDL). |
| **To**             | Evet         | Eşlenen ilişkiye karşılık gelen Gezinti özelliğinin **ToRole** özniteliğinin değeri. Daha fazla bilgi için bkz. NavigationProperty öğesi (CSDL).   |

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

Ayrıca aşağıdaki saklı yordamı göz önünde bulundurun:

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

`Course` varlığın güncelleştirme işlevini bu saklı yordama eşlemek için, **DepartmentID** parametresine bir değer sağlamanız gerekir. `DepartmentID` değeri varlık türündeki bir özelliğe karşılık gelmiyor; eşlemesi burada gösterilen bağımsız bir ilişkide bulunur:

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

Aşağıdaki kod, **FK\_kursu\_departmanı** ilişkilendirmesinin **DepartmentID** özelliğini **updatekurs** saklı yordamı ( **Kurs** varlık türünün Update Işlevinin eşlendiği) ile eşlemek için kullanılan **associationend** öğesini gösterir:

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

Eşleme belirtim dili (MSL) içindeki **Associationsetmapping** öğesi, temel veritabanındaki kavramsal model ve tablo sütunlarındaki bir ilişki arasındaki eşlemeyi tanımlar.

Kavramsal modeldeki ilişkilendirmeler, özellikleri temel veritabanında birincil ve yabancı anahtar sütunlarını temsil eden türlerdir. **Associationsetmapping** öğesi, veritabanındaki ilişkilendirme türü özellikleri ve sütunları arasındaki eşlemeleri tanımlamak Için Iki endproperty öğesi kullanır. Koşul öğesiyle bu eşlemelere koşullar yerleştirebilirsiniz. Veritabanında bulunan saklı yordamlara ilişkiler için INSERT, Update ve DELETE işlevlerini, ModificationFunctionMapping öğesiyle eşleyin. QueryView öğesinde bir Entity SQL dizesi kullanarak ilişkilendirmeler ve tablo sütunları arasında salt okunurdur eşlemeler tanımlayın.

> [!NOTE]
> Kavramsal modeldeki bir ilişki için bir başvuru kısıtlaması tanımlanmışsa, ilişkilendirmenin bir **Associationsetmapping** öğesiyle eşlenmesi gerekmez. Başvuru kısıtlaması olan bir ilişki için bir **Associationsetmapping** öğesi varsa, **associationsetmapping** öğesinde tanımlanan eşlemeler yok sayılır. Daha fazla bilgi için bkz. ReferentialConstraint öğesi (CSDL).

**Associationsetmapping** öğesi aşağıdaki alt öğelere sahip olabilir

-   QueryView (sıfır veya bir)
-   EndProperty (sıfır veya iki)
-   Koşul (sıfır veya daha fazla)
-   ModificationFunctionMapping (sıfır veya bir)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Associationsetmapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı     | Gereklidir | Değer                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Ad**           | Evet         | Eşlenmekte olan kavramsal model ilişkilendirme kümesinin adı.                      |
| **'Ta**       | Hayır          | Eşlenmekte olan kavramsal model ilişki türünün ad alanı nitelenmiş adı. |
| **StoreEntitySet** | Hayır          | Eşlenmekte olan tablonun adı.                                                 |

### <a name="example"></a>Örnek

Aşağıdaki örnek, kavramsal modelde ayarlanan **FK\_kursu\_departmanı** Ilişkilendirmesinin veritabanındaki **Kurs** tablosuna eşlendiği bir **associationsetmapping** öğesini gösterir. İlişki türü özellikleri ve tablo sütunları arasındaki eşlemeler alt **Endproperty** öğelerinde belirtilmiştir.

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

Eşleme belirtim dili (MSL) içindeki bir **ComplexProperty** öğesi, bir kavramsal model varlık türü ve temel alınan veritabanındaki tablo sütunlarındaki karmaşık tür özelliği arasındaki eşlemeyi tanımlar. Özellik sütun eşlemeleri alt ScalarProperty öğelerinde belirtilmiştir.

**ComplexType** özelliği öğesi aşağıdaki alt öğelere sahip olabilir:

-   ScalarProperty (sıfır veya daha fazla)
-   **Complexözelliği** (sıfır veya daha fazla)
-   ComplextTypeMapping (sıfır veya daha fazla)
-   Koşul (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **ComplexProperty** öğesi için geçerli olan öznitelikler açıklanmaktadır:

| Öznitelik adı | Gereklidir | Değer                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Eşlenmekte olan kavramsal modeldeki bir varlık türünün karmaşık özelliğinin adı. |
| **'Ta**   | Hayır          | Kavramsal model özelliği türünün ad alanı nitelikli adı.                              |

### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modelini temel alır. Kavramsal modele aşağıdaki karmaşık tür eklenmiştir:

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

**Kişi** varlık türünün **LastName** ve **FirstName** özellikleri, bir karmaşık özellikle değiştirildi, **ad**:

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

Aşağıdaki MSL, **ad** özelliğini temel alınan veritabanındaki sütunlara eşlemek Için kullanılan **ComplexProperty** öğesini göstermektedir:

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

Eşleme belirtim dili (MSL) içindeki **Complextypemapping** öğesi resultmapping öğesinin bir alt öğesidir ve aşağıdakiler doğru olduğunda temel alınan veritabanındaki bir işlev içeri aktarma ile kavramsal modeldeki bir saklı yordam arasındaki eşlemeyi tanımlar:

-   İşlev içeri aktarma kavramsal bir karmaşık tür döndürüyor.
-   Saklı yordam tarafından döndürülen sütunların adları, karmaşık türdeki özelliklerin adlarıyla tam olarak eşleşmez.

Varsayılan olarak, bir saklı yordam tarafından döndürülen sütunlar ve karmaşık bir tür arasındaki eşleme, sütun ve özellik adlarını temel alır. Sütun adları tam olarak özellik adlarıyla eşleşmezse, eşlemeyi tanımlamak için **Complextypemapping** öğesini kullanmanız gerekir. Varsayılan eşlemenin bir örneği için bkz. FunctionImportMapping öğesi (MSL).

**Complextypemapping** öğesi aşağıdaki alt öğelere sahip olabilir:

-   ScalarProperty (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Complextypemapping** öğesi için geçerli olan öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **'Ta**   | Evet         | Eşlenmekte olan karmaşık türün ad alanı nitelikli adı. |

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

Ayrıca, aşağıdaki kavramsal model karmaşık türünü de göz önünde bulundurun:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Önceki karmaşık türün örneklerini döndüren bir işlev içeri aktarması oluşturmak için, saklı yordamın ve varlık türünün döndürdüğü sütunlar arasındaki eşlemenin bir **Complextypemapping** öğesinde tanımlanması gerekir:

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

## <a name="condition-element-msl"></a>Condition öğesi (MSL)

Eşleme belirtim dili (MSL) içindeki **koşul** öğesi, kavramsal model ve temel alınan veritabanı arasındaki eşlemelerle ilgili koşullar koyar. Bir XML düğümü içinde tanımlanan eşleme, alt **koşul** öğelerinde belirtilen tüm koşullar karşılanıyorsa geçerlidir. Aksi takdirde, eşleme geçerli değildir. Örneğin, bir MappingFragment öğesi bir veya daha fazla **Condition** alt öğesi Içeriyorsa, **mappingfragment** düğümü içinde tanımlanan eşleme yalnızca alt **koşul** öğelerinin tüm koşulları karşılanıyorsa geçerli olur.

Her koşul, bir **ada** ( **ad** özniteliği tarafından belirtilen bir kavramsal model varlığı özelliğinin adı) veya **sütunadı** ( **ColumnName** özniteliği tarafından belirtilen veritabanında bir sütunun adı) uygulanabilir. **Ad** özniteliği ayarlandığında, koşul bir varlık özelliği değeri ile denetlenir. **ColumnName** özniteliği ayarlandığında, koşul bir sütun değerine göre denetlenir. **Condition** öğesinde **Name** veya **ColumnName** özniteliğinden yalnızca biri belirtilebilir.

> [!NOTE]
> **Condition** öğesi bir FunctionImportMapping öğesi içinde kullanıldığında, yalnızca **Name** özniteliği uygulanabilir değildir.

**Condition** öğesi aşağıdaki öğelerin bir alt öğesi olabilir:

-   AssociationSetMapping
-   Complexözelliği
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

**Condition** öğesinin hiç alt öğesi olamaz.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **koşul** öğesi için geçerli olan öznitelikler açıklanmaktadır:

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tation** | Hayır          | Koşulu değerlendirmek için değeri kullanılan tablo sütununun adı.                                                                                                                                                                                                                   |
| **IsNull**     | Hayır          | **True** veya **false**. Değer **true** ise ve sütun değeri **null**ise veya değer **false** ise ve sütun değeri **null**değilse, koşul true olur. Aksi takdirde, koşul false olur. <br/> **IsNull** ve **Value** öznitelikleri aynı anda kullanılamaz. |
| **Değer**      | Hayır          | Sütun değerinin karşılaştırıldığı değer. Değerler aynıysa, koşul true olur. Aksi takdirde, koşul false olur. <br/> **IsNull** ve **Value** öznitelikleri aynı anda kullanılamaz.                                                                       |
| **Ad**       | Hayır          | Koşulu değerlendirmek için kullanılan kavramsal model varlığı özelliğinin adı. <br/> **Condition** öğesi bir FunctionImportMapping öğesi içinde kullanılıyorsa bu öznitelik geçerli değildir.                                                                           |

### <a name="example"></a>Örnek

Aşağıdaki örnek, **durum** öğelerini **mappingfragment** öğelerinin alt öğesi olarak gösterir. **HireDate** null olmadığında ve **kayıttarihi** null olduğunda, veriler **SchoolModel. eğitmen** türü Ile **kişi** tablosunun **PersonID** ve **HireDate** sütunları arasında eşlenir. **Kayıttarihi** null olmadığında ve **HireDate** null olduğunda, veriler **SchoolModel. öğrenci** türü Ile **kişi** tablosunun **PersonID** ve **kayıt** sütunları arasında eşlenir.

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

Eşleme belirtim dili (MSL) içindeki **deletefunction** öğesi, kavramsal modeldeki Delete işlevini, temel veritabanında bulunan bir saklı yordama eşler. Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir. Daha fazla bilgi için bkz. Işlev öğesi (SSDL).

> [!NOTE]
> Bir varlık türünün ekleme, güncelleştirme veya silme işlemlerinin üçünü saklı yordamlara eşleştirmez, çalışma zamanında yürütülürse ve bir UpdateException oluşturulursa eşlenmemiş işlemler başarısız olur.

### <a name="deletefunction-applied-to-entitytypemapping"></a>EntityTypeMapping 'a uygulanan DeleteFunction

EntityTypeMapping öğesine uygulandığında, **Deletefunction** öğesi kavramsal modeldeki bir varlık türünün Delete işlevini bir saklı yordama eşler.

Bir **Entitytypemapping** öğesine uygulandığında **deletefunction** öğesi aşağıdaki alt öğelere sahip olabilir:

-   AssociationEnd (sıfır veya daha fazla)
-   Complexözelliği (sıfır veya daha fazla)
-   ScarlarProperty (sıfır veya daha fazla)

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, bir **Entitytypemapping** öğesine uygulandığında **deletefunction** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ifadelerini**          | Evet         | Delete işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı. Saklı yordam, depolama modelinde bildirilmelidir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satır sayısını döndüren çıkış parametresinin adı.                                                                               |

#### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modeline dayalıdır ve **kişi** varlık türünün Delete Işlevini **deleteperson** saklı yordamına eşleyen **deletefunction** öğesini gösterir. **Deleteperson** saklı yordamı depolama modelinde bildirilmiştir.

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

### <a name="deletefunction-applied-to-associationsetmapping"></a>AssociationSetMapping 'e uygulanan DeleteFunction

AssociationSetMapping öğesine uygulandığında, **Deletefunction** öğesi kavramsal modeldeki bir ilişkinin Delete işlevini bir saklı yordama eşler.

**Deletefunction** öğesi, **associationsetmapping** öğesine uygulandığında aşağıdaki alt öğelere sahip olabilir:

-   EndProperty

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tablo, **Associationsetmapping** öğesine uygulandığında **deletefunction** öğesine uygulanabilen öznitelikleri açıklar.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ifadelerini**          | Evet         | Delete işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı. Saklı yordam, depolama modelinde bildirilmelidir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satır sayısını döndüren çıkış parametresinin adı.                                                                               |

#### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modeline dayalıdır ve **courseeğitmen** ilişkilendirmesinin Delete Işlevini **deletecourseeğitmen** saklı yordamına eşlemek için kullanılan **deletefunction** öğesini gösterir. **Deletecourseeğitmen** saklı yordamı, depolama modelinde bildirilmiştir.

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

Eşleme belirtim dili (MSL) içindeki **Endproperty** öğesi, kavramsal model ilişkilendirmesinin bir End veya bir değiştirme işlevi ile temel alınan veritabanı arasındaki eşlemeyi tanımlar. Özellik sütunu eşlemesi bir alt ScalarProperty öğesinde belirtildi.

Bir **Endproperty** öğesi kavramsal model ilişkisinin sonuna yönelik eşlemeyi tanımlamak için kullanıldığında, bir associationsetmapping öğesinin alt öğesidir. Bir kavramsal model ilişkisinin değiştirme işlevi için eşlemeyi tanımlamak üzere **Endproperty** öğesi kullanıldığında, bir ınsertfunction öğesinin veya deletefunction öğesinin alt öğesidir.

**Endproperty** öğesi aşağıdaki alt öğelere sahip olabilir:

-   ScalarProperty (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Endproperty** öğesi için geçerli olan öznitelikler açıklanmaktadır:

| Öznitelik adı | Gereklidir | Değer                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Name           | Evet         | Eşlenmekte olan ilişki ucunun adı. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, kavramsal modeldeki **FK\_kursu\_departmanı** Ilişkilendirmesinin veritabanındaki **Kurs** tablosuyla eşlendiği bir **associationsetmapping** öğesini gösterir. İlişki türü özellikleri ve tablo sütunları arasındaki eşlemeler alt **Endproperty** öğelerinde belirtilmiştir.

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

Aşağıdaki örnek, bir ilişkilendirmenin (**Courseeğitmen**) INSERT ve DELETE işlevlerini temel alınan veritabanındaki saklı yordamlara eşleyen **endproperty** öğesini gösterir. İle eşlenen işlevler depolama modelinde bildirilmiştir.

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

## <a name="entitycontainermapping-element-msl"></a>EntityContainerMapping öğesi (MSL)

Eşleme belirtim dili (MSL) içindeki **Entitycontainermapping** öğesi, kavramsal modeldeki varlık kapsayıcısını depolama modelindeki varlık kapsayıcısına eşler. **Entitycontainermapping** öğesi, Mapping öğesinin bir alt öğesidir.

**Entitycontainermapping** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   EntitySetMapping (sıfır veya daha fazla)
-   AssociationSetMapping (sıfır veya daha fazla)
-   FunctionImportMapping (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **Entitycontainermapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Evet         | Eşlenmekte olan depolama modeli varlık kapsayıcısının adı.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Evet         | Eşlenmekte olan kavramsal model varlık kapsayıcısının adı.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | Hayır          | **True** veya **false**. **Yanlışsa**, güncelleştirme görünümleri oluşturulmaz. Veriler başarıyla geri dönüş yaptığından, geçersiz olabilecek bir salt okuma eşlemi varsa, bu öznitelik **false** olarak ayarlanmalıdır. <br/> Varsayılan değer **Doğru** değeridir. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, **SchoolModelEntities** kapsayıcısını (kavramsal model varlık kapsayıcısı) **SchoolModelStoreContainer** kapsayıcısına (depolama modeli varlık kapsayıcısı) eşleyen bir **entitycontainermapping** öğesi gösterir:

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

Eşleme belirtim dili (MSL) içindeki **EntitySetMapping** öğesi, bir kavramsal model varlığındaki tüm türleri, depolama modelindeki varlık kümelerine göre eşler. Kavramsal modeldeki bir varlık, aynı türde (ve türetilmiş türler) varlık örnekleri için mantıksal bir kapsayıcıdır. Depolama modelinde ayarlanan bir varlık, temel alınan veritabanında bir tabloyu veya görünümü temsil eder. Kavramsal model varlık kümesi, **EntitySetMapping** öğesinin **Name** özniteliğinin değeri ile belirtilir. Eşlenen tablo veya görünüm her bir alt MappingFragment öğesinde veya **EntitySetMapping** öğesinin kendisinde **storeentityset** özniteliği tarafından belirtilir.

**EntitySetMapping** öğesi aşağıdaki alt öğelere sahip olabilir:

-   EntityTypeMapping (sıfır veya daha fazla)
-   QueryView (sıfır veya bir)
-   MappingFragment (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **EntitySetMapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı           | Gereklidir | Değer                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                 | Evet         | Eşlenmekte olan kavramsal model varlık kümesinin adı.                                                                                                                                                             |
| **TypeName** **1**       | Hayır          | Eşlenmekte olan kavramsal model varlık türünün adı.                                                                                                                                                            |
| **Storeentityset** **1** | Hayır          | Eşlenmekte olan depolama modeli varlık kümesinin adı.                                                                                                                                                             |
| **MakeColumnsDistinct**  | Hayır          | Yalnızca ayrı satırların döndürülüp döndürülmediğine bağlı olarak **doğru** veya **yanlış** . <br/> Bu öznitelik **true**olarak ayarlanırsa, EntityContainerMapping öğesinin **generateupdateviews** özniteliği **false**olarak ayarlanmalıdır. |

 

**1** **TypeName** ve **storeentityset** öznitelikleri, tek bir varlık türünü tek bir tabloya eşlemek için entitytypemapping ve mappingfragment alt öğelerinin yerine kullanılabilir.

### <a name="example"></a>Örnek

Aşağıdaki örnek, kavramsal modelin **Kurslar** varlık kümesindeki üç türü (temel tür ve iki türetilmiş tür) eşleyen bir **EntitySetMapping** öğesini gösterir ve temel alınan veritabanında üç farklı tabloya sahiptir. Tablolar her **Mappingfragment** öğesinde **storeentityset** özniteliği tarafından belirtilir.

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

Eşleme belirtim dili (MSL) içindeki **Entitytypemapping** öğesi, kavramsal modeldeki bir varlık türü ve temel alınan veritabanındaki tablolar veya görünümler arasındaki eşlemeyi tanımlar. Kavramsal model varlık türleri ve temel alınan veritabanı tabloları ya da görünümleri hakkında daha fazla bilgi için bkz. EntityType öğesi (CSDL) ve EntitySet öğesi (SSDL). Eşlenmekte olan kavramsal model varlık türü **Entitytypemapping** öğesinin **TypeName** özniteliğiyle belirtilir. Eşlenmekte olan tablo veya görünüm, alt MappingFragment öğesinin **Storeentityset** özniteliği tarafından belirtilir.

ModificationFunctionMapping alt öğesi, varlık türlerinin INSERT, Update veya delete işlevlerini veritabanındaki saklı yordamlara eşlemek için kullanılabilir.

**Entitytypemapping** öğesi aşağıdaki alt öğelere sahip olabilir:

-   MappingFragment (sıfır veya daha fazla)
-   ModificationFunctionMapping (sıfır veya bir)
-   ScalarProperty
-   Koşul

> [!NOTE]
> **Mappingfragment** ve **ModificationFunctionMapping** öğeleri aynı anda **entitytypemapping** öğesinin alt öğesi olamaz.


> [!NOTE]
> **Scalarproperty** ve **Condition** öğeleri yalnızca bir FunctionImportMapping öğesi içinde kullanıldığında **entitytypemapping** öğesinin alt öğeleri olabilir.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **Entitytypemapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **'Ta**   | Evet         | Eşlenmekte olan kavramsal model varlık türünün ad alanı nitelikli adı. <br/> Tür soyut veya türetilmiş bir tür ise, değer `IsOfType(Namespace-qualified_type_name)`olmalıdır. |

### <a name="example"></a>Örnek

Aşağıdaki örnek iki alt **Entitytypemapping** öğesi olan bir EntitySetMapping öğesini gösterir. İlk **Entitytypemapping** öğesinde, **SchoolModel. Person** varlık türü **kişi** tablosuna eşlenir. İkinci **Entitytypemapping** öğesinde, **SchoolModel. Person** türünün güncelleştirme işlevselliği veritabanında bir saklı yordam olan **UpdatePerson**ile eşleştirilir.

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

Sonraki örnek, kök türünün soyut olduğu bir tür hiyerarşisinin eşlemesini gösterir. **TypeName** öznitelikleri için `IsOfType` sözdiziminin kullanımını aklınızda edin.

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

## <a name="functionimportmapping-element-msl"></a>FunctionImportMapping öğesi (MSL)

Eşleme belirtim dili (MSL) içindeki **FunctionImportMapping** öğesi, kavramsal modeldeki bir işlev içeri aktarması ile temel alınan veritabanındaki bir saklı yordam veya işlev arasındaki eşlemeyi tanımlar. İşlev içeri aktarmaları kavramsal modelde bildirilmelidir ve saklı yordamlar depolama modelinde bildirilmelidir. Daha fazla bilgi için bkz. FunctionImport öğesi (CSDL) ve Işlev öğesi (SSDL).

> [!NOTE]
> Varsayılan olarak, bir işlev içeri aktarma bir kavramsal model varlık türü veya karmaşık tür döndürürse, temel saklı yordam tarafından döndürülen sütunların adları kavramsal model türündeki özelliklerin adlarıyla tam olarak eşleşmelidir. Sütun adları Özellik adlarıyla tam olarak eşleşmiyorsa, eşlemenin bir ResultMapping öğesinde tanımlanması gerekir.

**FunctionImportMapping** öğesi aşağıdaki alt öğelere sahip olabilir:

-   ResultMapping (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **FunctionImportMapping** öğesi için geçerli olan öznitelikler açıklanmaktadır:

| Öznitelik adı         | Gereklidir | Değer                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **Işleviçeaktarmaadı** | Evet         | Eşlenmekte olan kavramsal modelde işlev içeri aktarma işleminin adı.           |
| **Ifadelerini**       | Evet         | Eşlenen depolama modelindeki işlevin ad alanı nitelikli adı. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modelini temel alır. Depolama modelinde aşağıdaki işlevi göz önünde bulundurun:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Kavramsal modelde bu işlevi içeri aktarmayı da göz önünde bulundurun:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

Aşağıdaki örnek, yukarıdaki işlev ve işlev içeri aktarmayı birbirlerine eşlemek için kullanılan bir **FunctionImportMapping** öğesi gösterir:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>Insertfunction öğesi (MSL)

Eşleme belirtim dili (MSL) içindeki **ınsertfunction** öğesi, kavramsal modeldeki INSERT işlevini temel veritabanındaki bir saklı yordama eşler. Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir. Daha fazla bilgi için bkz. Işlev öğesi (SSDL).

> [!NOTE]
> Bir varlık türünün ekleme, güncelleştirme veya silme işlemlerinin üçünü saklı yordamlara eşleştirmez, çalışma zamanında yürütülürse ve bir UpdateException oluşturulursa eşlenmemiş işlemler başarısız olur.

**Insertfunction** öğesi ModificationFunctionMapping öğesinin bir alt öğesi olabilir ve entitytypemapping öğesine veya associationsetmapping öğesine uygulanır.

### <a name="insertfunction-applied-to-entitytypemapping"></a>EntityTypeMapping 'a uygulanan ınsertfunction

EntityTypeMapping öğesine uygulandığında, **ınsertfunction** öğesi kavramsal modeldeki bir varlık türünün INSERT işlevini bir saklı yordama eşler.

Bir **Entitytypemapping** öğesine uygulandığında **ınsertfunction** öğesi aşağıdaki alt öğelere sahip olabilir:

-   AssociationEnd (sıfır veya daha fazla)
-   Complexözelliği (sıfır veya daha fazla)
-   ResultBinding (sıfır veya bir)
-   ScarlarProperty (sıfır veya daha fazla)

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, bir **Entitytypemapping** öğesine uygulandığında **ınsertfunction** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ifadelerini**          | Evet         | INSERT işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı. Saklı yordam, depolama modelinde bildirilmelidir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satır sayısını döndüren çıkış parametresinin adı.                                                                               |

#### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modeline dayalıdır ve kişi varlık türünün INSERT işlevini **ınsertperson** saklı yordamına eşlemek Için kullanılan **ınsertfunction** öğesini gösterir. **Insertperson** saklı yordamı depolama modelinde bildirilmiştir.

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
### <a name="insertfunction-applied-to-associationsetmapping"></a>AssociationSetMapping 'e uygulanan ınsertfunction

AssociationSetMapping öğesine uygulandığında, **ınsertfunction** öğesi kavramsal modeldeki bir ilişkinin INSERT işlevini bir saklı yordama eşler.

**Insertsetmapping öğesine uygulandığında ınsertfunction** öğesi aşağıdaki alt öğelere sahip olabilir:

-   EndProperty

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tablo, **Associationsetmapping** öğesine uygulandığında **ınsertfunction** öğesine uygulanabilen öznitelikleri açıklar.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ifadelerini**          | Evet         | INSERT işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı. Saklı yordam, depolama modelinde bildirilmelidir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satır sayısını döndüren çıkış parametresinin adı.                                                                               |

#### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modeline dayalıdır ve **courseeğitmen** ilişkilendirmesinin INSERT Işlevini **ınsertcourseeğitmen** saklı yordamına eşlemek için kullanılan **ınsertfunction** öğesini gösterir. **Insertcourseeğitmen** saklı yordamı, depolama modelinde bildirilmiştir.

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

## <a name="mapping-element-msl"></a>Mapping öğesi (MSL)

Eşleme belirtimi dili (MSL) içindeki **Mapping** öğesi, kavramsal modelde tanımlanan nesneleri bir veritabanına (bir depolama modelinde açıklandığı gibi) eşlemek için bilgiler içerir. Daha fazla bilgi için bkz. CSDL belirtimi ve SSDL belirtimi.

**Mapping** öğesi, bir eşleme belirtiminin kök öğesidir. Eşleme belirtimleri için XML ad alanı https://schemas.microsoft.com/ado/2009/11/mapping/cs.

Mapping öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Diğer ad (sıfır veya daha fazla)
-   EntityContainerMapping (tam olarak bir)

MSL 'de başvurulan kavramsal ve depolama modeli türlerinin adları, ilgili ad alanı adlarıyla nitelenmelidir. Kavramsal model ad alanı adı hakkında daha fazla bilgi için bkz. şema öğesi (CSDL). Depolama modeli ad alanı adı hakkında daha fazla bilgi için bkz. şema öğesi (SSDL). MSL 'de kullanılan ad alanları için diğer adlar diğer ad öğesiyle tanımlanabilir.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Mapping** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **Boşlu**      | Evet         | **C-S**. Bu sabit bir değerdir ve değiştirilemez. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modelinin bir bölümünü temel alan bir **Mapping** öğesi gösterir. Okul modeli hakkında daha fazla bilgi için bkz. hızlı başlangıç (Entity Framework):

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

## <a name="mappingfragment-element-msl"></a>MappingFragment öğesi (MSL)

Eşleme belirtim dili (MSL) içindeki **Mappingfragment** öğesi, bir kavramsal model varlık türünün özellikleri ve veritabanındaki bir tablo veya görünüm arasındaki eşlemeyi tanımlar. Kavramsal model varlık türleri ve temel alınan veritabanı tabloları ya da görünümleri hakkında daha fazla bilgi için bkz. EntityType öğesi (CSDL) ve EntitySet öğesi (SSDL). **Mappingfragment** entitytypemapping öğesinin ya da EntitySetMapping öğesinin bir alt öğesi olabilir.

**Mappingfragment** öğesi aşağıdaki alt öğelere sahip olabilir:

-   ComplexType (sıfır veya daha fazla)
-   ScalarProperty (sıfır veya daha fazla)
-   Koşul (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Mappingfragment** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı          | Gereklidir | Değer                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Evet         | Eşlenmekte olan tablonun veya görünümün adı.                                                                                                                                                                           |
| **MakeColumnsDistinct** | Hayır          | Yalnızca ayrı satırların döndürülüp döndürülmediğine bağlı olarak **doğru** veya **yanlış** . <br/> Bu öznitelik **true**olarak ayarlanırsa, EntityContainerMapping öğesinin **generateupdateviews** özniteliği **false**olarak ayarlanmalıdır. |

### <a name="example"></a>Örnek

Aşağıdaki örnek bir **Entitytypemapping** öğesinin alt öğesi olarak bir **mappingfragment** öğesi gösterir. Bu örnekte, kavramsal modeldeki **Kurs** türünün özellikleri, veritabanındaki **Kurs** tablosunun sütunlarına eşlenir.

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

Aşağıdaki örnek bir **EntitySetMapping** öğesinin alt öğesi olarak bir **mappingfragment** öğesi gösterir. Yukarıdaki örnekte olduğu gibi, kavramsal modeldeki **Kurs** türünün özellikleri, veritabanındaki **Kurs** tablosunun sütunlarına eşlenir.

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

Eşleme belirtim dili (MSL) içindeki **ModificationFunctionMapping** öğesi, kavramsal model varlık türünün INSERT, Update ve DELETE işlevlerini temel alınan veritabanındaki saklı yordamlar olarak eşleştirir. **ModificationFunctionMapping** öğesi Ayrıca, kavramsal modeldeki çok-çok İlişkilendirmelerin INSERT ve DELETE işlevlerini temel alınan veritabanındaki Saklı yordamlarla de eşleyebilir. Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir. Daha fazla bilgi için bkz. Işlev öğesi (SSDL).

> [!NOTE]
> Bir varlık türünün ekleme, güncelleştirme veya silme işlemlerinin üçünü saklı yordamlara eşleştirmez, çalışma zamanında yürütülürse ve bir UpdateException oluşturulursa eşlenmemiş işlemler başarısız olur.


> [!NOTE]
> Bir devralma hiyerarşisindeki bir varlığın değişiklik işlevleri Saklı yordamlarla eşlenmişse, hiyerarşideki tüm türlerin değiştirme işlevlerinin Saklı yordamlarla eşlenmesi gerekir.

**ModificationFunctionMapping** öğesi entitytypemapping öğesinin bir alt öğesi veya associationsetmapping öğesi olabilir.

**ModificationFunctionMapping** öğesi aşağıdaki alt öğelere sahip olabilir:

-   DeleteFunction (sıfır veya bir)
-   Insertfunction (sıfır veya bir)
-   UpdateFunction (sıfır veya bir)

**ModificationFunctionMapping** öğesi için geçerli bir öznitelik yok.

### <a name="example"></a>Örnek

Aşağıdaki örnekte, okul modelinde ayarlanan **kişiler** varlığı için varlık kümesi eşleştirmesi gösterilmektedir. **Kişi** varlık türü için sütun eşlemenin yanı sıra, **kişi** türünün INSERT, Update ve DELETE işlevlerinin eşlemesi gösterilir. İle eşlenen işlevler depolama modelinde bildirilmiştir.

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

Aşağıdaki örnekte, okul modelinde **Kurs** için bir ilişki kümesi eşlemesi gösterilmektedir. **Courseeğitmen** ilişkilendirmesinin sütun eşlemesine ek olarak, **courseeğitmen** ilişkilendirmesinin INSERT ve DELETE işlevlerinin eşleştirmesi gösterilir. İle eşlenen işlevler depolama modelinde bildirilmiştir.

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

Eşleme belirtim dili (MSL) içindeki **QueryView** öğesi, kavramsal modeldeki bir varlık türü veya ilişkilendirme ile temel alınan veritabanındaki bir tablo arasındaki salt okunurdur eşlemeyi tanımlar. Eşleme, depolama modeline göre değerlendirilen bir Entity SQL sorgusuyla tanımlanır ve sonuç kümesini kavramsal modeldeki bir varlık veya ilişkilendirme açısından ifade edersiniz. Sorgu görünümleri salt okunurdur, sorgu görünümleri tarafından tanımlanan türleri güncelleştirmek için Standart güncelleştirme komutlarını kullanamazsınız. Değiştirme işlevlerini kullanarak bu türlerde güncelleştirmeler yapabilirsiniz. Daha fazla bilgi için bkz. nasıl yapılır: değişiklik Işlevlerini saklı yordamlara eşleme.

> [!NOTE]
> **QueryView** öğesinde, **GroupBy**, Grup toplamaları veya gezinti özellikleri içeren Entity SQL ifadeleri desteklenmez.

 

**QueryView** öğesi EntitySetMapping öğesinin bir alt öğesi ya da associationsetmapping öğesi olabilir. Önceki durumda, sorgu görünümü kavramsal modeldeki bir varlık için salt okunurdur eşlemeyi tanımlar. İkinci durumda, sorgu görünümü kavramsal modeldeki bir ilişki için salt okunurdur eşlemeyi tanımlar.

> [!NOTE]
> **Associationsetmapping** öğesi başvuru kısıtlaması olan bir ilişkilendirme için Ise, **associationsetmapping** öğesi yok sayılır. Daha fazla bilgi için bkz. ReferentialConstraint öğesi (CSDL).

**QueryView** öğesinin herhangi bir alt öğesi olamaz.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **QueryView** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **'Ta**   | Hayır          | Sorgu görünümü tarafından eşlenmekte olan kavramsal model türünün adı. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, **QueryView** öğesini **EntitySetMapping** öğesinin bir alt öğesi olarak gösterir ve okul modelindeki **Departman** varlık türü için bir sorgu görünümü eşlemesi tanımlar.

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

Sorgu yalnızca depolama modelindeki **Departman** türünde üyelerin bir alt kümesini döndürdüğünden, okul modelindeki **Departman** türü bu eşlemeye göre aşağıdaki gibi değiştirilmiştir:

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

Sonraki örnek, bir **Associationsetmapping** öğesinin alt öğesi olarak **QueryView** öğesini gösterir ve okul modelinde `FK_Course_Department` ilişkilendirmesi için salt okunurdur eşlemeyi tanımlar.

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

-   Kavramsal modelde, depolama modelindeki varlığın tüm özelliklerini içermeyen bir varlık tanımlayın. Bu, varsayılan değerlere sahip olmayan ve **null** değerleri desteklemeyen özellikler içerir.
-   Depolama modelindeki hesaplanan sütunları kavramsal modeldeki varlık türlerinin özelliklerine eşleyin.
-   Kavramsal modeldeki varlıkları bölümlemek için kullanılan koşulların eşitlik temelinde olmadığı bir eşleme tanımlayın. **Koşul** öğesini kullanarak koşullu eşleme belirttiğinizde, sağlanan koşulun belirtilen değere eşit olması gerekir. Daha fazla bilgi için bkz. koşul öğesi (MSL).
-   Depolama modelindeki sütunu, kavramsal modeldeki birden çok türe eşleyin.
-   Birden çok türü aynı tabloyla eşleyin.
-   Kavramsal modelde ilişkisel şemadaki yabancı anahtarları temel alan ilişkilendirmeler tanımlayın.
-   Kavramsal modeldeki özelliklerin değerini ayarlamak için özel iş mantığını kullanın. Örneğin, veri kaynağındaki "T" dize değerini, kavramsal modelde bir Boole değeri olan **true**değerine eşleyebilirsiniz.
-   Sorgu sonuçları için koşullu filtreler tanımlayın.
-   Kavramsal modeldeki veriler üzerinde depolama modelinden daha az kısıtlama zorlayın. Örneğin, bir özelliği, eşlendiği sütun **null**değerleri desteklemediğinden bile kavramsal modelde null yapılabilir hale getirebilirsiniz.

Varlıkların sorgu görünümlerini tanımlarken aşağıdaki noktalar geçerlidir:

-   Sorgu görünümleri salt okunurdur. Yalnızca değiştirme işlevlerini kullanarak varlıklara güncelleştirmeler yapabilirsiniz.
-   Bir sorgu görünümü ile bir varlık türü tanımladığınızda, tüm ilgili varlıkları sorgu görünümlerine göre de tanımlamanız gerekir.
-   İlişkisel şemadaki bir bağlantı tablosunu temsil eden depolama modelindeki bir varlıkla çoktan çoğa bir ilişki eşlediğinizde, bu bağlantı tablosunun **Associationsetmapping** öğesinde bir **QueryView** öğesi tanımlamanız gerekir.
-   Sorgu görünümleri bir tür hiyerarşisindeki tüm türler için tanımlanmalıdır. Bunu aşağıdaki yöntemlerle yapabilirsiniz:
-   -   Hiyerarşide tüm varlık türlerinin bir birleşimini döndüren tek bir Entity SQL sorgusu belirten tek bir **QueryView** öğesi ile.
    -   Belirli bir koşula bağlı olarak hiyerarşide belirli bir varlık türünü döndürmek için CASE işlecini kullanan tek bir Entity SQL sorgusu belirten tek bir **QueryView** öğesi ile.
    -   Hiyerarşide belirli bir tür için ek bir **QueryView** öğesi ile. Bu durumda, her bir görünüm için varlık türünü belirtmek üzere **QueryView** öğesinin **TypeName** özniteliğini kullanın.
-   Bir sorgu görünümü tanımlandığında, **EntitySetMapping** öğesinde **storagesetname** özniteliğini belirtemezsiniz.
-   Bir sorgu görünümü tanımlandığında, **EntitySetMapping**öğesi **özellik** eşlemeleri de içeremez.

## <a name="resultbinding-element-msl"></a>ResultBinding öğesi (MSL)

Eşleme belirtimi dili (MSL) içindeki **Resultbinding** öğesi, varlık türü değişikliği işlevleri temel alınan veritabanındaki Saklı yordamlarla eşlendiğinde kavramsal modeldeki varlık özelliklerine döndürülen sütun değerlerini eşler. Örneğin, bir kimlik sütununun değeri bir INSERT saklı yordamı tarafından döndürüldüğünde, **Resultbinding** öğesi döndürülen değeri kavramsal modeldeki bir varlık türü özelliğine eşler.

**Resultbinding** öğesi, ınsertfunction öğesinin veya updatefunction öğesinin alt öğesi olabilir.

**Resultbinding** öğesinin herhangi bir alt öğesi olamaz.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Resultbinding** öğesi için geçerli olan öznitelikler açıklanmaktadır:

| Öznitelik adı | Gereklidir | Değer                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Ad**       | Evet         | Eşlenmekte olan kavramsal modeldeki varlık özelliğinin adı. |
| **Tation** | Evet         | Eşlenen sütunun adı.                                          |

### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modeline dayalıdır ve **kişi** varlık türünün INSERT Işlevini **ınsertperson** saklı yordamına eşlemek Için kullanılan bir **ınsertfunction** öğesini gösterir. ( **Insertperson** saklı yordamı aşağıda gösterilmektedir ve depolama modelinde bildirilmiştir.) Bir **Resultbinding** öğesi, saklı yordamın (**newpersonıd**) döndürdüğü bir sütun değerini bir varlık türü özelliğine (**PersonID**) eşlemek için kullanılır.

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

Aşağıdaki Transact-SQL, **ınsertperson** saklı yordamını açıklar:

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

Eşleme belirtim dili (MSL) içindeki **Resultmapping** öğesi, aşağıdaki doğru olduğunda, kavramsal modelde bir işlev içeri aktarma ve temel alınan veritabanındaki saklı yordam arasındaki eşlemeyi tanımlar:

-   İşlev içeri aktarma bir kavramsal model varlık türü veya karmaşık tür döndürüyor.
-   Saklı yordam tarafından döndürülen sütunların adları, varlık türü veya karmaşık türdeki özelliklerin adlarıyla tam olarak eşleşmez.

Varsayılan olarak, bir saklı yordam tarafından döndürülen sütunlar ve varlık türü veya karmaşık tür arasındaki eşleme, sütun ve özellik adlarını temel alır. Sütun adları Özellik adlarıyla tam olarak eşleşmiyorsa, eşlemeyi tanımlamak için **Resultmapping** öğesini kullanmanız gerekir. Varsayılan eşlemenin bir örneği için bkz. FunctionImportMapping öğesi (MSL).

**Resultmapping** öğesi FunctionImportMapping öğesinin bir alt öğesidir.

**Resultmapping** öğesi aşağıdaki alt öğelere sahip olabilir:

-   EntityTypeMapping (sıfır veya daha fazla)
-   ComplexTypeMapping

**Resultmapping** öğesi için geçerli bir öznitelik yok.

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

Ayrıca, aşağıdaki kavramsal model varlık türünü de göz önünde bulundurun:

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

Önceki varlık türünün örneklerini döndüren bir işlev içeri aktarması oluşturmak için, saklı yordamın ve varlık türünün döndürdüğü sütunlar arasındaki eşlemenin bir **Resultmapping** öğesinde tanımlanması gerekir:

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

Eşleme belirtim dili (MSL) içindeki **Scalarproperty** öğesi bir kavramsal model varlık türü, karmaşık tür veya bir tablo sütunuyla ilişkili bir özelliği veya temel alınan veritabanındaki saklı yordam parametresini eşleştirir.

> [!NOTE]
> Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir. Daha fazla bilgi için bkz. Işlev öğesi (SSDL).

**Scalarproperty** öğesi aşağıdaki öğelerin bir alt öğesi olabilir:

-   MappingFragment
-   Insertfunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   Complexözelliği
-   ResultMapping

**Mappingfragment**, **ComplexProperty**veya **endproperty** öğesinin bir alt öğesi olarak, **scalarproperty** öğesi kavramsal modeldeki bir özelliği veritabanındaki bir sütuna eşler. **Insertfunction**, **Updatefunction**veya **deletefunction** öğesinin bir alt öğesi olarak, **scalarproperty** öğesi kavramsal modeldeki bir özelliği bir saklı yordam parametresine Eşler.

**Scalarproperty** öğesinin herhangi bir alt öğesi olamaz.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

**Scalarproperty** öğesi için uygulanan öznitelikler, öğesinin rolüne bağlı olarak farklılık gösterir.

Aşağıdaki **tabloda, bir** kavramsal model özelliğini veritabanındaki bir sütuna eşlemek için kullanılan özellikler açıklanmaktadır:

| Öznitelik adı | Gereklidir | Değer                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Ad**       | Evet         | Eşlenmekte olan kavramsal model özelliğinin adı. |
| **Tation** | Evet         | Eşlenmekte olan tablo sütununun adı.              |

Aşağıdaki tabloda, bir kavramsal model özelliğini saklı yordam parametresine eşlemek için kullanıldığında **Scalarproperty** öğesi için geçerli olan öznitelikler açıklanmaktadır:

| Öznitelik adı    | Gereklidir | Değer                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**          | Evet         | Eşlenmekte olan kavramsal model özelliğinin adı.                                                                                 |
| **ParameterName** | Evet         | Eşlenmekte olan parametrenin adı.                                                                                                 |
| **Sürüm**       | Hayır          | **Geçerli** veya **orijinal** değerin, özelliğin orijinal değerinin eşzamanlılık denetimleri için kullanılıp kullanılmayacağını belirtir. |

### <a name="example"></a>Örnek

Aşağıdaki örnek, iki şekilde kullanılan **Scalarproperty** öğesini göstermektedir:

-   **Kişi** varlık türünün özelliklerini **kişi**tablosunun sütunlarına eşlemek için.
-   **Kişi** varlık türünün özelliklerini **UpdatePerson** saklı yordamının parametreleriyle eşlemek için. Saklı yordamlar depolama modelinde bildirilmiştir.

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

Sonraki örnekte, bir kavramsal model ilişkisinin INSERT ve DELETE işlevlerini veritabanındaki Saklı yordamlarla eşlemek için kullanılan **Scalarproperty** öğesi gösterilmektedir. Saklı yordamlar depolama modelinde bildirilmiştir.

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

Eşleme belirtim dili (MSL) içindeki **Updatefunction** öğesi, kavramsal modeldeki bir varlık türünün Update işlevini temel alınan veritabanında bulunan bir saklı yordama eşler. Değişiklik işlevlerinin eşlendiği saklı yordamlar depolama modelinde bildirilmelidir. Daha fazla bilgi için bkz. Işlev öğesi (SSDL).

> [!NOTE]
>  Bir varlık türünün ekleme, güncelleştirme veya silme işlemlerinin üçünü saklı yordamlara eşleştirmez, çalışma zamanında yürütülürse ve bir UpdateException oluşturulursa eşlenmemiş işlemler başarısız olur.

**Updatefunction** öğesi ModificationFunctionMapping öğesinin bir alt öğesi olabilir ve entitytypemapping öğesine uygulanır.

**Updatefunction** öğesi aşağıdaki alt öğelere sahip olabilir:

-   AssociationEnd (sıfır veya daha fazla)
-   Complexözelliği (sıfır veya daha fazla)
-   ResultBinding (sıfır veya bir)
-   ScarlarProperty (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tablo **Updatefunction** öğesine uygulanabilen öznitelikleri açıklar.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ifadelerini**          | Evet         | Update işlevinin eşlendiği saklı yordamın ad alanı nitelikli adı. Saklı yordam, depolama modelinde bildirilmelidir. |
| **RowsAffectedParameter** | Hayır          | Etkilenen satır sayısını döndüren çıkış parametresinin adı.                                                                               |

### <a name="example"></a>Örnek

Aşağıdaki örnek, okul modeline dayalıdır ve **kişi** varlık türünün güncelleştirme Işlevini **UpdatePerson** saklı yordamına eşlemek için kullanılan **updatefunction** öğesini gösterir. **UpdatePerson** saklı yordamı depolama modelinde bildirilmiştir.

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
