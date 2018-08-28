---
title: CSDL belirtimi - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 88669cf80f9a792fda7d191d9f6be2b1734691df
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994735"
---
# <a name="csdl-specification"></a>CSDL belirtimi
Kavramsal şema tanım dili (CSDL) varlıklar, ilişkileri ve kavramsal bir modeli verilerle bir uygulama olun işlevlerini açıklayan bir XML tabanlı bir dilidir. Bu kavramsal model Entity Framework veya WCF Veri Hizmetleri tarafından kullanılabilir. CSDL ile açıklanan meta veri varlıkları ve bir veri kaynağına kavramsal modelde tanımlı ilişkiler eşlemek için Entity Framework tarafından kullanılır. Daha fazla bilgi için [SSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) ve [MSL belirtimi](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL, varlık veri modeli, Entity Framework'ün uygulamasıdır.

Bir Entity Framework uygulamasında, kavramsal model meta verilerini (CSDL içinde yazılan) .csdl dosyasından System.Data.Metadata.Edm.EdmItemCollection bir örneğine yüklenir ve yöntemleri kullanılarak erişilebilir System.Data.Metadata.Edm.MetadataWorkspace sınıfı. Varlık çerçevesi kavramsal model meta veri kaynağına özgü komutlar kavramsal modeline karşı sorgular çevirmek için kullanır.

EF Designer, tasarım zamanında bir .edmx dosyası içinde kavramsal model bilgileri depolar. Oluşturma zamanında EF Designer Entity Framework tarafından çalışma zamanında gereken .csdl dosyası oluşturmak için bir .edmx dosyası içinde bilgileri kullanır.

CSDL sürümleri, XML ad alanları tarafından ayrılır.

| CSDL sürümü | XML Namespace                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | http://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Association öğesinde (CSDL)

Bir **ilişkilendirme** öğe iki varlık türleri arasındaki bir ilişkiyi tanımlar. İlişkilendirmesine katılan varlık türleri ve varlık türleri çeşitliliği bilinen ilişkinin her iki ucunda olası sayısını belirtmeniz gerekir. Bir ilişkilendirme end'ün çoğulluğunun bir değer bir (1) sıfır veya bir (0..1) ya da birden çok olabilir (\*). Bu bilgiler, iki alt son öğe belirtilir.

Bir varlık türünde gösteriliyorsa varlık türü örneklerinin bir ilişkilendirmenin bir ucunda Gezinti özellikleri veya yabancı anahtarlar erişilebilir.

Bir uygulamada belirli bir ilişki varlık türleri örnekleri arasında bir ilişki örneğini temsil eder. İlişkilendirme örnekleri mantıksal olarak bir ilişki kümesi içinde gruplandırılır.

Bir **ilişkilendirme** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Bitiş (tam olarak 2 öğe)
-   Referentialconstraint'teki (sıfır veya bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ilişkilendirme** öğesi.

| Öznitelik adı | Gereklidir | Değer                        |
|:---------------|:------------|:-----------------------------|
| **Ad**       | Evet         | İlişkilendirmenin adı. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ilişkilendirme** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** yabancı anahtarlar üzerinde karşılaştıklarını değil, ilişki **müşteri** ve  **Sipariş** varlık türleri. **Çoğulluk** değerler her **son** ilişkisini gösteren diğer birçok **siparişler** ile ilişkili bir **müşteri**, ancak yalnızca bir **müşteri** ile ilişkili bir **sipariş**. Ayrıca, **OnDelete** öğesi gösterir tüm **siparişler** belirli bir ilgili **müşteri** ve içine yüklenen ObjectContext silinecek varsa **müşteri** silinir.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** yabancı anahtarlar üzerinde kullanıma sunması olduğunda ilişkilendirme **müşteri** ve  **Sipariş** varlık türleri. Kullanıma sunulan yabancı anahtarlar ile varlıklar arasında ilişki ile yönetilen bir **Referentialconstraint'teki** öğesi. Karşılık gelen bir AssociationSetMapping öğesi bu ilişkiyi veri kaynağına eşlemek gerekli değildir.

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
 

 

## <a name="associationset-element-csdl"></a>AssociationSet öğesi (CSDL)

**AssociationSet** kavramsal şema tanım dili (CSDL) öğesinde, aynı türün ilişkilendirme örnekleri için mantıksal bir kapsayıcıdır. Bir veri kaynağına eşlenebilecek bir gruplandırma ilişkilendirme örnekleri için bir tanımı bir ilişki kümesi sağlar.  

**AssociationSet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (izin verilen sıfır veya bir öğe)
-   Bitiş (tam olarak iki öğe gerekli)
-   Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)

**İlişkilendirme** özniteliği bir ilişki kümesi içeren bir ilişki türünü belirtir. Bir ilişki kümesi sonunu yapmak varlık kümeleri ile tam olarak iki alt belirtilen **son** öğeleri.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **AssociationSet** öğesi.

| Öznitelik adı  | Gereklidir | Değer                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**        | Evet         | Varlık kümesinin adı. Değerini **adı** öznitelik değeri ile aynı olamaz **ilişkilendirme** özniteliği.                                 |
| **İlişkilendirme** | Evet         | İlişkilendirme ayarlanmış ilişkilendirme tam olarak nitelenmiş adını örneklerini içerir. İlişkilendirme ilişki kümesi ile aynı ad alanında olması gerekir. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **AssociationSet** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityContainer** iki öğe **AssociationSet** öğeleri:

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
 

 

## <a name="collectiontype-element-csdl"></a>CollectionType öğesi (CSDL)

**CollectionType** kavramsal şema tanım dili (CSDL) öğesinde bir işlev parametresi veya işlevin dönüş türü, bir koleksiyon olduğunu belirtir. **CollectionType** parametresi ReturnType (işlev) öğesi veya alt öğesi olabilir. Toplama türünü kullanarak belirtilebilir **türü** özniteliği veya şu alt öğelerden biri:

-   **CollectionType**
-   referenceType
-   RowType
-   TypeRef

> [!NOTE]
> Bir koleksiyonun türü ile her ikisi de belirtilirse model doğrulama değil **türü** özniteliğini ve bir alt öğesi.

 

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **CollectionType** öğesi. Unutmayın **DefaultValue**, **MaxLength**, **FixedLength**, **duyarlık**, **ölçek**,  **Unicode**, ve **harmanlama** öznitelikleri koleksiyonlarına geçerli yalnızca **EDMSimpleTypes**.

| Öznitelik adı                                                          | Gereklidir | Değer                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Türü**                                                                | Hayır          | Koleksiyonun türü.                                                                                                                                                                                                      |
| **Boş değer atanabilir**                                                            | Hayır          | **Doğru** (varsayılan değer) veya **False** bağlı olup olmadığını özelliği null değeri olabilir. <br/> [!NOTE]                                                                                                                 |
| > CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **defaultValue**                                                        | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                               |
| **maxLength**                                                           | Hayır          | Özellik değeri en büyük uzunluğu.                                                                                                                                                                                        |
| **FixedLength**                                                         | Hayır          | **Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.                                                                                                                           |
| **Duyarlık**                                                           | Hayır          | Özellik değerinin kesinliği.                                                                                                                                                                                             |
| **Ölçek**                                                               | Hayır          | Özellik değerinin ölçek.                                                                                                                                                                                                 |
| **SRID**                                                                | Hayır          | Sistem uzamsal başvuru tanımlayıcısı. Yalnızca uzamsal tür özellikleri için geçerlidir.   Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | Hayır          | **Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.                                                                                                                                |
| **Harmanlama**                                                           | Hayır          | Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.                                                                                                                                                    |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **CollectionType** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. bir **CollectionType** işlevi bir koleksiyonu döndürdüğünü belirtmek için öğe **kişi** varlık türleri ( ilebelirtilen**ElementType** özniteliği).

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
 

Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğe (belirtilmiş **RowType** öğesi).

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
 

Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. **CollectionType** işlevi parametre olarak bir koleksiyonunu kabul belirtmek için öğe **departmanı** varlık türleri.

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
 

 

## <a name="complextype-element-csdl"></a>ComplexType öğesi (CSDL)

A **ComplexType** öğe tanımlar oluşan bir veri yapısı **EdmSimpleType** özellikleri veya diğer karmaşık türler.  Bir karmaşık türü bir varlık türünün veya başka bir karmaşık türü bir özelliği olabilir. Verileri bir karmaşık tür tanımlar, bir karmaşık türü bir varlık türüne benzerdir. Ancak, karmaşık türlerden ve varlık türleri arasındaki bazı temel farklar vardır:

-   Karmaşık türler kimlikleri (veya anahtarlara) yoksa ve bu nedenle bağımsız olarak var olamaz. Karmaşık türler yalnızca varlık türleri veya diğer karmaşık türler özellikleri olarak bulunabilir.
-   Karmaşık türler ilişkilendirmeler katılamaz. Ne bir ilişki sonu bir karmaşık türü olabilir ve bu nedenle karmaşık türler için Gezinti özellikleri tanımlanamaz.
-   Her bir karmaşık tür skaler özellikleri ayarlanabilir ancak bir karmaşık tür özelliği bir null değer olamaz null.

A **ComplexType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Özellik (sıfır veya daha fazla öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ComplexType** öğesi.

| Öznitelik adı                                                                                                 | Gereklidir | Değer                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ad                                                                                                           | Evet         | Karmaşık tür adı. Karmaşık bir tür adını başka bir ad ile aynı olamaz karmaşık tür, varlık türünün veya model kapsamında olan ilişkisi. |
| BaseType                                                                                                       | Hayır          | Tanımlanmakta olan karmaşık türün temel türü başka bir karmaşık tür adı. <br/> [!NOTE]                                                                     |
| > Bu öznitelik CSDL v1'de geçerli değildir. Karmaşık türleri için devralma, bu sürümde desteklenmiyor. |             |                                                                                                                                                                                     |
| Özet                                                                                                       | Hayır          | **Doğru** veya **False** (varsayılan değer) karmaşık türü soyut bir tür olup olmamasına bağlı olarak. <br/> [!NOTE]                                                                  |
| > Bu öznitelik CSDL v1'de geçerli değildir. Karmaşık türler bu sürümde, soyut türlerin olamaz.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ComplexType** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir karmaşık tür gösterir **adresi**, ile **EdmSimpleType** özellikleri **StreetAddress**, **Şehir**,  **Eyaletveİl**, **Ülke**, ve **PostalCode**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Karmaşık tür tanımlamak için **adresi** (yukarıda) bir varlık türünün bir özellik olarak, özellik türü varlık tür tanımında bildirmeniz gerekir. Aşağıdaki örnekte gösterildiği **adresi** özelliği üzerinde bir varlık türü karmaşık bir tür olarak (**yayımcı**):

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
 

 

## <a name="definingexpression-element-csdl"></a>DefiningExpression öğesi (CSDL)

**DefiningExpression** kavramsal şema tanım dili (CSDL) öğesinde kavramsal modelde bir işlevi tanımlayan bir varlık SQL ifadesi içeriyor.  

> [!NOTE]
> Doğrulama amacıyla bir **DefiningExpression** rasgele içerik öğesi içerebilir. Ancak, Entity Framework bağlanamazsa özel durum çalışma zamanında bir **DefiningExpression** öğesi geçerli varlık SQL içermiyor.

 

### <a name="applicable-attributes"></a>Uygun öznitelikler

Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **DefiningExpression** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte bir **DefiningExpression** bir kitap yayımlandığı tarihten sonra geçen yıl sayısını döndüren bir işlev tanımlamak için. İçeriği **DefiningExpression** öğesi varlık SQL yazılır.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Bağımlı öğesi (CSDL)

**Bağımlı** kavramsal şema tanım dili (CSDL) öğesinde Referentialconstraint'teki öğesi bir alt öğesidir ve başvuru kısıtlamasını bağımlı sonuna tanımlar. A **Referentialconstraint'teki** öğe ilişkisel bir veritabanındaki bir başvuru bütünlüğü kısıtlaması benzer işlevselliği tanımlar. Bir veritabanı tablosundan bir sütuna (veya sütun) başka bir tablonun birincil anahtarı başvurabilirsiniz aynı şekilde, başka bir varlık türünün Varlık anahtarı bir varlık türünün bir özelliği (veya Özellikler) başvurabilir. Başvurulan varlık türü olarak adlandırılır *birincil ucu* kısıtlaması. Birincil ucu başvuran varlık türü olarak adlandırılan *bağımlı son* kısıtlaması. **PropertyRef** öğeleri birincil ucu hangi anahtarları başvuru belirtmek için kullanılır.

**Bağımlı** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   PropertyRef (bir veya daha fazla öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **bağımlı** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rol**       | Evet         | Varlık türü bağımlı ucundaki ilişkinin adı. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **bağımlı** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **Referentialconstraint'teki** öğe tanımının bir parçası olarak kullanılan **yayınladığı** ilişkilendirme. **Publisherıd** özelliği **kitap** varlık türü başvuru kısıtlamasını bağımlı sonuna yapar.

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
 

 

## <a name="documentation-element-csdl"></a>Belge öğesi (CSDL)

**Belgeleri** kavramsal şema tanım dili (CSDL) öğesinde, bir üst öğe içinde tanımlanan bir nesneyle ilgili bilgileri sağlamak için kullanılabilir. Bir .edmx dosyası içinde olduğunda **belgeleri** öğesi EF Designer (örneğin, bir varlık, ilişkilendirme veya özellik), tasarım yüzeyindeki bir nesnenin içeriğini görünür bir öğenin alt öğesi olan **belgeleri**  öğesi Visual Studio'da görünür **özellikleri** pencere nesnesi için.

**Belgeleri** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   **Özet**: üst öğenin kısa bir açıklaması. (sıfır veya bir öğe)
-   **LongDescription**: üst öğenin kapsamlı bir açıklama. (sıfır veya bir öğe)
-   Ek açıklama öğeleri. (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **belgeleri** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği **belgeleri** EntityType öğesinin alt öğesi olarak öğesi. CSDL aşağıdaki kod parçacığında bir .edmx içerik, dosya, içeriğini **özeti** ve **LongDescription** öğeleri, Visual Studio'da görüneceği **özellikleri** tıkladığınızda penceresi `Customer` varlık türü.

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
 

 

## <a name="end-element-csdl"></a>Bitiş öğesi (CSDL)

**Son** kavramsal şema tanım dili (CSDL) öğesinde Association öğesinde veya AssociationSet bir öğenin bir alt olabilir. Her durumda, rolü **son** öğe farklı ve uygun öznitelikler farklıdır.

### <a name="end-element-as-a-child-of-the-association-element"></a>Bir ilişkilendirme öğesinin alt öğesi olarak bitiş öğesi

Bir **son** öğesi (alt öğesi olarak **ilişkilendirme** öğesi) bir ilişki sonu ve bu ilişkilendirmeyi sonunda bulunabilir varlık türü örneklerinin varlık türü tanımlar. İlişkilendirme ucu ilişkilendirme bir parçası olarak tanımlanır; bir ilişkiyi tam olarak iki ilişkilendirme ucu olması gerekir. Bir varlık türünde gösteriliyorsa varlık türü örneklerinin bir ilişkilendirmenin bir ucunda Gezinti özellikleri veya yabancı anahtarlar erişilebilir.  

Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   OnDelete (sıfır veya bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **ilişkilendirme** öğesi.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Türü**         | Evet         | İlişkilendirmenin bir ucunda varlık türünün adı.                                                                                                                                                                                                                                                                                                                                                         |
| **Rol**         | Hayır          | İlişki sonu için bir ad. Adı sağlanmazsa, ilişki uç varlık türünün adı kullanılır.                                                                                                                                                                                                                                                                                           |
| **Çokluk** | Evet         | **1**, **0..1**, veya **\*** ilişkilendirme sonunda olabilir bir varlık türü örneklerinin sayısına bağlı olarak. <br/> **1** bu tam olarak bir varlık türü örneği var ilişkisi sonuna belirten. <br/> **0..1** sıfır veya bir varlık türü örneklerini ilişkilendirme sonunda bulunduğunu gösterir. <br/> **\*** sıfır, bir veya daha fazla varlık türü örneklerini ilişkilendirme sonunda bulunduğunu gösterir. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** ilişkilendirme. **Çoğulluk** değerler her **son** ilişkisini gösteren diğer birçok **siparişler** ile ilişkili bir **müşteri**, ancak yalnızca bir **müşteri** ile ilişkili bir **sipariş**. Ayrıca, **OnDelete** öğesi gösteren tüm **siparişler** belirli bir ilgili **müşteri** ve içine yüklendi ObjectContext olacaktır Silinen if **müşteri** silinir.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>AssociationSet öğesinin bir alt öğesi olarak bitiş öğesi

**Son** öğesi bir ilişki kümesi ucunu belirtir. **AssociationSet** öğesi iki içermelidir **son** öğeleri. İçinde yer alan bilgileri bir **son** öğesi, bir veri kaynağı olarak ayarlanmış bir ilişkilendirme eşlemesini kullanılır.

Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

> [!NOTE]
> Ek açıklama öğelerinin diğer tüm alt öğeleri sonra görünmelidir. Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.

 

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **AssociationSet** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Evet         | Adını **EntitySet** üst ucunu tanımlayan öğe **AssociationSet** öğesi. **EntitySet** öğesi tanımlanmış, üst ile aynı varlık kapsayıcıda **AssociationSet** öğesi. |
| **Rol**       | Hayır          | Son ilişkilendirmenin adı ayarlayın. Varsa **rol** özniteliği kullanılmazsa, ilişki sonu Ayarla adını varlık kümesinin adı olacaktır.                                                                   |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityContainer** iki öğe **AssociationSet** öğeleri, her iki **son** öğeleri:

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
 

 

## <a name="entitycontainer-element-csdl"></a>EntityContainer öğesi (CSDL)

**EntityContainer** kavramsal şema tanım dili (CSDL) öğesinde, varlık kümelerini ve ilişki Setleri işlevi içeri aktarmalar için mantıksal bir kapsayıcıdır. Kavramsal model varlık kapsayıcısı depolama modelinin varlık kapsayıcısı Entitycontainermapping'indeki öğesi ile eşlenir. Bir depolama modelinin varlık kapsayıcısı veritabanının yapısını açıklar: Varlık kümeleri açıklayan tablolar, ilişki Setleri, yabancı anahtar kısıtlamaları açıklar ve işlevi alır, bir veritabanındaki saklı yordamlar açıklanmaktadır.

Bir **EntityContainer** öğesi sıfır veya bir belge öğeleri olabilir. Varsa bir **belgeleri** öğe varsa, tüm gelmelidir **EntitySet**, **AssociationSet**, ve **Functionımport** öğeleri.

Bir **EntityContainer** öğesi (listelenen sırayla) sıfır veya daha fazla şu alt öğelerden biri olabilir:

-   EntitySet
-   AssociationSet
-   Functionımport
-   Ek açıklama öğeleri

Genişletebileceğiniz bir **EntityContainer** başka bir deponun içeriğini içerecek şekilde öğesi **EntityContainer** aynı ad alanı içinde olmasıdır. Başka bir deponun içeriğini içerecek şekilde **EntityContainer**, başvuru olarak **EntityContainer** öğesini ayarlayın **Extends** adınaözniteliği **EntityContainer** dahil etmek istediğiniz öğe. Tüm alt öğeleri dahil **EntityContainer** öğesi başvuru alt öğeleri olarak kabul edilir **EntityContainer** öğesi.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **kullanma** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Ad**       | Evet         | Varlık kapsayıcısının adı.                               |
| **Genişletir**    | Hayır          | Aynı ad alanı içinde başka bir varlık kapsayıcısının adı. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityContainer** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityContainer** üç varlık setleri ve iki ilişki Setleri tanımlayan öğe.

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
 

 

## <a name="entityset-element-csdl"></a>Entityset'in öğe (CSDL)

**EntitySet** kavramsal şema tanım dili elemanıdır örnekleri bir varlık türünün ve bu varlık türünden türetilmiş herhangi bir tür örnekleri için mantıksal kapsayıcı. Bir varlık türü ve bir varlık kümesi arasındaki ilişki, ilişkisel bir veritabanındaki bir tabloda bir satırı arasındaki ilişkiye benzer. Bir satır gibi ilgili veri kümesi bir varlık türü tanımlar ve bir tablo gibi bir varlık kümesini bu tanımı örneklerini içerir. Bir varlık kümesini, böylece bir veri kaynağında ilgili veri yapıları için eşlenebilir varlık türü örneklerini gruplamak için bir yapı sağlar.  

Bir özel varlık türü için birden fazla varlık tanımlanabilir.

> [!NOTE]
> EF Designer türü başına birden çok varlık kümeleri içeren kavramsal modeller desteklemez.

 

**EntitySet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belge öğesi (izin verilen sıfır veya bir öğe)
-   Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntitySet** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Varlık kümesinin adı.                                                              |
| **entityType** | Evet         | Varlık türü için varlık kümesi tam olarak nitelenmiş adını örneklerini içerir. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntitySet** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityContainer** üç öğeyle **EntitySet** öğeleri:

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
 

Tür (MEST) başına birden çok varlık kümesi tanımlamak mümkündür. Aşağıdaki örnek, bir varlık kapsayıcısı için iki varlık kümeleri tanımlar **kitap** varlık türü:

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
 

 

## <a name="entitytype-element-csdl"></a>EntityType öğesi (CSDL)

**EntityType** öğesi, bir müşteri ya da kavramsal modelde sırası gibi üst düzey bir kavram yapısını temsil eder. Bir şablon için bir varlık türü olduğu varlık türleri bir uygulama örneği. Her şablon, aşağıdaki bilgileri içerir:

-   Benzersiz bir ad. (Gerekli)
-   Bir veya daha fazla özellikleri tarafından tanımlanan bir varlık anahtarı. (Gerekli)
-   Veri içeren özellikleri. (İsteğe bağlı.)
-   Bir ilişkilendirmenin bir uçtan diğerine Gezinti diğer ucuna izin Gezinti özellikleri. (İsteğe bağlı.)

Bir uygulamada (örneğin, belirli müşteri veya sipariş) belirli bir nesnesi bir varlık türünün bir örneği temsil eder. Bir varlık türünün her örneğinin bir varlık kümesi içinde benzersiz Varlık anahtarı olması gerekir.

İki varlık türü örnekleri yalnızca aynı türde oldukları ve bunların varlık anahtarları aynı değerleri eşit olarak kabul edilir.

Bir **EntityType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Anahtar (sıfır veya bir öğe)
-   Özellik (sıfır veya daha fazla öğe)
-   NavigationProperty (sıfır veya daha fazla öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntityType** öğesi.

| Öznitelik adı                                                                                                                                  | Gereklidir | Değer                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Ad**                                                                                                                                        | Evet         | Varlık türü adı.                                                                     |
| **BaseType**                                                                                                                                    | Hayır          | Tanımlanmakta olan varlık türünün temel türü başka bir varlık türünün adı.  |
| **Özet**                                                                                                                                    | Hayır          | **Doğru** veya **False**varlık türü soyut bir tür olup bağlı olarak.                 |
| **OpenType**                                                                                                                                    | Hayır          | **Doğru** veya **False** varlık türü bir açık varlık türü olup olmamasına bağlı olarak. <br/> [!NOTE] |
| > **OpenType** özniteliktir yalnızca ADO.NET Data Services ile kullanılan kavramsal modellerde tanımlanan varlık türleri için geçerlidir. |             |                                                                                                  |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityType** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityType** üç öğeyle **özelliği** öğeleri ve iki **NavigationProperty** öğeleri:

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
 

 

## <a name="enumtype-element-csdl"></a>EnumType öğesi (CSDL)

**EnumType** öğesi bir listeden seçimli türü temsil eder.

Bir **EnumType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Üye (sıfır veya daha fazla öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EnumType** öğesi.

| Öznitelik adı     | Gereklidir | Değer                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**           | Evet         | Varlık türü adı.                                                                                                                                                                  |
| **Isflags**        | Hayır          | **Doğru** veya **False**sabit listesi türü bayrakları bir dizi kullanılıp kullanılamayacağı bağlı olarak. Varsayılan değer **yanlış.**.                                                                     |
| **UnderlyingType** | Hayır          | **Edm.Byte**, **Edm.Int16**, **EDM.Int32**, **EDM.Int64** veya **Edm.SByte** tür değerleri aralığı tanımlama.   Sabit listesi öğeleri listesinin temel alınan türü varsayılandır **EDM.Int32.**. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EnumType** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EnumType** üç öğeyle **üye** öğeleri:

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function öğesi (CSDL)

**İşlevi** kavramsal şema tanım dili (CSDL) öğesinde tanımlayın veya kavramsal modeldeki işlevleri bildirmek için kullanılır. Bir işlev DefiningExpression öğesi kullanılarak tanımlanır.  

A **işlevi** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Parametre (sıfır veya daha fazla öğe)
-   DefiningExpression (sıfır veya bir öğe)
-   ReturnType (işlev) (sıfır veya bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

Dönüş türü için bir işlev ile birlikte belirtilmelidir **ReturnType** (işlev) öğesi veya **ReturnType** özniteliği (aşağıya bakın), ancak ikisine birden değil. Olası dönüş türleri herhangi EdmSimpleType, varlık türü, karmaşık tür, satır türü veya başvuru türü (veya bu tür bir koleksiyonu) var.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **işlevi** öğesi.

| Öznitelik adı | Gereklidir | Değer                              |
|:---------------|:------------|:-----------------------------------|
| **Ad**       | Evet         | İşlevin adı.          |
| **ReturnType** | Hayır          | İşlev tarafından döndürülen tür. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **işlevi** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte bir **işlevi** bir eğitmen işe alındım beri yıl sayısını döndüren bir işlev tanımlamak için.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>Functionımport öğesi (CSDL)

**Functionımport** kavramsal şema tanım dili (CSDL) öğesinde tanımlanan veri kaynağındaki ancak nesnelere kavramsal model aracılığıyla kullanılabilir olan bir işlevi temsil eder. Örneğin, depolama modelinin Function öğesinde, bir veritabanında bir saklı yordam temsil etmek için kullanılabilir. A **Functionımport** kavramsal model öğesinde karşılık gelen işlev bir Entity Framework uygulamasını temsil eder ve depolama modeli işleve Functionımportmapping element kullanılarak eşlendi. Uygulamada işlev çağrıldığında, karşılık gelen saklı yordamı veritabanında yürütülür.

**Functionımport** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (izin verilen sıfır veya bir öğe)
-   Parametre (izin verilen sıfır veya daha fazla öğe)
-   Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)
-   ReturnType (Functionımport) (izin verilen sıfır veya daha fazla öğe)

Bir **parametre** öğesi, işlev kabul eden her parametre için tanımlanmış olması.

Dönüş türü için bir işlev ile birlikte belirtilmelidir **ReturnType** (Functionımport) öğesi veya **ReturnType** özniteliği (aşağıya bakın), ancak ikisine birden değil. Dönüş türü değeri koleksiyonu EdmSimpleType, EntityType veya ComplexType olmalıdır.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **Functionımport** öğesi.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**         | Evet         | İçeri aktarılan bir işlevin adı.                                                                                                                                                                    |
| **ReturnType**   | Hayır          | İşlevinin döndürdüğü türü. İşlev bir değer döndürmezse bu özniteliği kullanmayın. Aksi takdirde, değeri, ComplexType, EntityType veya EDMSimpleType koleksiyonu olması gerekir.        |
| **EntitySet**    | Hayır          | Varlık koleksiyonu işlevi döndürürse, türleri, değerini **EntitySet** koleksiyona ait olduğu varlık kümesi gerekir. Aksi takdirde, **EntitySet** özniteliği değil kullanılmalıdır. |
| **Iscomposable** | Hayır          | Değer ayarlanmışsa true, işlev birleştirilebilir (tablo değerli işlev) ve bir LINQ Sorgu kullanılabilir.  Varsayılan değer **false**.                                                           |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **Functionımport** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **Functionımport** öğesi, bir parametre kabul eden ve varlık türleri koleksiyonunu döndürür:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Anahtar öğesi (CSDL)

**Anahtarı** öğesi EntityType öğesi bir alt öğesidir ve tanımlayan bir *Varlık anahtarı* (bir özellik veya bir varlık türünün kimliğini belirlemek özellikler kümesi). Bir varlık anahtarı oluşturan özelliklerini tasarım zamanında seçilir. Varlık anahtar özelliklerin değerlerini çalışma zamanında ayarlama varlık içinde bir varlık türü örneği benzersiz şekilde tanımlamalıdır. Bir varlık kümesindeki örneklerin benzersizliği garanti etmek için bir varlık anahtarı oluşturan özellikleri seçilmelidir. **Anahtar** öğesi bir veya daha fazla özellik bir varlık türünün başvurarak Varlık anahtarı tanımlar.

**Anahtar** öğesi şu alt öğelerden olabilir:

-   PropertyRef (bir veya daha fazla öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **anahtar** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte adlı bir varlık türü tanımlayan **kitap**. Varlık anahtarı başvurarak tanımlanan **ISBN** varlık türü özelliği.

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
 

**ISBN** özelliği olduğundan Varlık anahtarı için iyi bir seçim bir kitap tanıtan bir Uluslararası Standart Kitap sayısı (ISBN).

Aşağıdaki örnek, bir varlık türü gösterir (**Yazar**) iki özelliklerini içeren bir varlık anahtarına sahip **adı** ve **adresi**.

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
 

Kullanarak **adı** ve **adresi** çünkü aynı adlı iki yazarları aynı adresindeki live olasılığının düşük olduğu varlık için makul bir seçim anahtardır. Ancak, bu seçenek bir varlık anahtarı için bir varlık kümesindeki benzersiz varlık anahtarları kesinlikle garanti etmez. Bir özellik gibi ekleme **AuthorId**, benzersiz şekilde tanımlamak için kullanılabilir bir yazar önerilen bu durumda.

 

## <a name="member-element-csdl"></a>Üye öğesi (CSDL)

**Üye** öğesi EnumType öğesi bir alt öğesidir ve numaralandırılmış tür üyesi tanımlar.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **Functionımport** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Üyenin adı.                                                                                                                                                                  |
| **Değer**      | Hayır          | Üyenin değeri. Varsayılan olarak 0 değeri ilk üye sahiptir ve art arda gelen her Numaralandırıcı değeri 1 azaltılır. Aynı değerlere sahip birden fazla üye var olabilir. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **Functionımport** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EnumType** üç öğeyle **üye** öğeleri:

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>NavigationProperty öğesi (CSDL)

A **NavigationProperty** öğe bir ilişki sonu bir başvuru sağlayan bir gezinti özelliği tanımlar. Özellik öğesi ile tanımlanan özelliklerin aksine, gezinti özellikleri veri özelliklerini ve Şekil tanımlamaz. İki varlık türleri arasındaki ilişkiyi gitmek için bir yol sağlarlar.

Gezinti özellikleri her iki varlık türlerini ilişkilendirme ucunda isteğe bağlı olduğunu unutmayın. İlişkilendirme sonunda bir varlık türünün gezinme özelliğinin tanımlarsanız, ilişkinin diğer ucundaki varlık türünün gezinme özelliğinin tanımlamak zorunda değil.

Bir gezinti özelliği tarafından döndürülen veri türü Uzak ilişkilendirme bitiş'ün çoğulluğunun tarafından belirlenir. Örneğin, bir gezinti özelliği varsayalım **OrdersNavProp**, bulunuyor. bir **müşteri** varlık türü ve arasında bir-çok ilişkisi gider **müşteri** ve  **Sipariş**. Gezinti özelliği için Uzak ilişkilendirme end çok çeşitlilik olduğundan (\*), kendi veri türüne koleksiyonudur (, **sipariş**). Benzer şekilde, bir gezinti özelliği, **CustomerNavProp**, bulunuyor **sipariş** varlık türü, veri türü olacak **müşteri** uzak uç çokluğu olduğundan bir (1).

A **NavigationProperty** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **NavigationProperty** öğesi.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**         | Evet         | Gezinme özelliğinin adı.                                                                                                                                                                                                             |
| **İlişki** | Evet         | Modeli kapsamında olmayan bir ilişkilendirmenin adı.                                                                                                                                                                                |
| **ToRole**       | Evet         | Gezinti biteceği ilişki sonu. Değerini **ToRole** özniteliği bir değer ile aynı olmalıdır **rol** (ilişki ucu öğesinde tanımlanan) ilişkilendirme uçlarından tanımlanan öznitelikleri.       |
| **FromRole**     | Evet         | Gezinti başlar ilişki sonu. Değerini **FromRole** özniteliği bir değer ile aynı olmalıdır **rol** (ilişki ucu öğesinde tanımlanan) ilişkilendirme uçlarından tanımlanan öznitelikleri. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **NavigationProperty** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir varlık türü tanımlar. (**kitap**) iki Gezinti özellikleri ile (**yayınladığı** ve **WrittenBy**):

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
 

 

## <a name="ondelete-element-csdl"></a>OnDelete öğesi (CSDL)

**OnDelete** kavramsal şema tanım dili (CSDL) öğesinde bir ilişkilendirme bağlı davranışını tanımlar. Varsa **eylem** özniteliği **Cascade** ilişkilendirme bir ucunda, ilişkinin diğer ucundaki varlık türleri varlık türü ilk uca silindiğinde silinir ilgili. İki varlık türleri arasındaki ilişkiyi bir birincil anahtar birincil anahtar ilişkidir sonra yüklenen bir bağımlı nesne bağımsız olarak, ilişkinin diğer ucundaki sorumlusu nesnesi silindiğinde silinir **OnDelete** belirtimi.  

> [!NOTE]
> **OnDelete** öğesi yalnızca bir uygulama çalışma zamanı davranışını etkiler; veri kaynağı davranışını etkilemez. Veri kaynağında tanımlanan davranışı uygulamada tanımlanan davranışı ile aynı olması gerekir.

 

Bir **OnDelete** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **OnDelete** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Eylem**     | Evet         | **Art arda** veya **hiçbiri**. Varsa **Cascade**, bağımlı varlık türleri, asıl varlık türü silindiğinde silinir. Varsa **hiçbiri**, asıl varlık türü silindiğinde bağımlı varlık türleri silinmeyecek. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ilişkilendirme** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **CustomerOrders** ilişkilendirme. **OnDelete** öğesi gösteren tüm **siparişler** belirli bir ilgili **müşteri** ve ObjectContext yüklenen zaman silinecek  **Müşteri** silinir.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parametre öğesi (CSDL)

**Parametre** kavramsal şema tanım dili (CSDL) öğesinde bir alt Functionımport öğesi veya Function öğesi olabilir.

### <a name="functionimport-element-application"></a>Functionımport öğesi uygulama

A **parametre** öğesi (alt öğesi olarak **Functionımport** öğesi) CSDL içinde bildirilen işlev içeri aktarmalar için giriş ve çıkış parametrelerini tanımlamak için kullanılır.

**Parametre** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (izin verilen sıfır veya bir öğe)
-   Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **parametre** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Parametrenin adı.                                                                                                                                                                                                      |
| **Türü**       | Evet         | Parametre türü. Değer olmalıdır bir **EDMSimpleType** veya modeli kapsamında olan bir karmaşık türü.                                                                                                             |
| **Modu**       | Hayır          | **İçinde**, **kullanıma**, veya **Inout** parametresi bir giriş, çıkış ve giriş/çıkış parametresi olmasına bağlı olarak.                                                                                                                |
| **maxLength**  | Hayır          | Parametresinin uzunluğu izin verilen en fazla.                                                                                                                                                                                    |
| **Duyarlık**  | Hayır          | Parametre duyarlılığı.                                                                                                                                                                                                 |
| **Ölçek**      | Hayır          | Parametre ölçeği.                                                                                                                                                                                                     |
| **SRID**       | Hayır          | Sistem uzamsal başvuru tanımlayıcısı. Uzamsal tür parametreleri yalnızca için geçerlidir. Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **parametre** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **Functionımport** bir öğeyle **parametre** alt öğesi. İşlev bir giriş parametresi kabul eden ve varlık türleri koleksiyonunu döndürür.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>İşlev öğe uygulama

A **parametre** öğesi (alt öğesi olarak **işlevi** öğesi) kavramsal modelde tanımlı ya da bildirilen işlevler için parametreleri tanımlar.

**Parametre** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   CollectionType (sıfır veya bir öğe)
-   ReferenceType (sıfır veya bir öğe)
-   RowType (sıfır veya bir öğe)

> [!NOTE]
> Yalnızca biri **CollectionType**, **ReferenceType**, veya **RowType** öğeleri alt öğesi olabilir bir **özelliği** öğesi.

 

-   Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)

> [!NOTE]
> Ek açıklama öğelerinin diğer tüm alt öğeleri sonra görünmelidir. Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.

 

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **parametre** öğesi.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**         | Evet         | Parametrenin adı.                                                                                                                                                                                                      |
| **Türü**         | Hayır          | Parametre türü. Bir parametre aşağıdaki türleri (veya bu türler) herhangi biri olabilir: <br/> **EdmSimpleType** <br/> varlık türü <br/> Karmaşık tür <br/> Satır türü <br/> başvuru türü                             |
| **Boş değer atanabilir**     | Hayır          | **Doğru** (varsayılan değer) veya **False** özelliğine sahip olup olmadığına bağlı olarak bir **null** değeri.                                                                                                                          |
| **defaultValue** | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                              |
| **maxLength**    | Hayır          | Özellik değeri en büyük uzunluğu.                                                                                                                                                                                       |
| **FixedLength**  | Hayır          | **Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.                                                                                                                          |
| **Duyarlık**    | Hayır          | Özellik değerinin kesinliği.                                                                                                                                                                                            |
| **Ölçek**        | Hayır          | Özellik değerinin ölçek.                                                                                                                                                                                                |
| **SRID**         | Hayır          | Sistem uzamsal başvuru tanımlayıcısı. Yalnızca uzamsal tür özellikleri için geçerlidir. Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Hayır          | **Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.                                                                                                                               |
| **Harmanlama**    | Hayır          | Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.                                                                                                                                                   |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **parametre** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **işlevi** kullanan tek öğe **parametresi** bir işlev parametresi tanımlamak için alt öğesi.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a>Asıl öğe (CSDL)

**Asıl** kavramsal şema tanım dili (CSDL) öğesinde Referentialconstraint'teki öğesine bir başvuru kısıtlamasının birincil ucu tanımlayan bir alt öğedir. A **Referentialconstraint'teki** öğe ilişkisel bir veritabanındaki bir başvuru bütünlüğü kısıtlaması benzer işlevselliği tanımlar. Bir veritabanı tablosundan bir sütuna (veya sütun) başka bir tablonun birincil anahtarı başvurabilirsiniz aynı şekilde, başka bir varlık türünün Varlık anahtarı bir varlık türünün bir özelliği (veya Özellikler) başvurabilir. Başvurulan varlık türü olarak adlandırılır *birincil ucu* kısıtlaması. Birincil ucu başvuran varlık türü olarak adlandırılan *bağımlı son* kısıtlaması. **PropertyRef** öğeleri hangi anahtarları bağımlı uç tarafından başvurulan belirtmek için kullanılır.

**Asıl** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   PropertyRef (bir veya daha fazla öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **asıl** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rol**       | Evet         | İlişkisinin birincil ucu ' varlık türünün adı. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **asıl** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **Referentialconstraint'teki** tanımının bir parçası olan öğeyi **yayınladığı** ilişkilendirme. **Kimliği** özelliği **yayımcı** varlık türü başvuru kısıtlamasını birincil ucu sağlar.

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
 

 

## <a name="property-element-csdl"></a>Özellik öğesi (CSDL)

**Özelliği** kavramsal şema tanım dili (CSDL) öğesinde bir alt öğe EntityType, ComplexType öğesi veya RowType öğesinin olabilir.

### <a name="entitytype-and-complextype-element-applications"></a>EntityType ve ComplexType öğesi uygulamalar

**Özellik** öğeleri (alt öğeleri olarak **EntityType** veya **ComplexType** öğeleri) şekil ve bir varlık türü örneği veya karmaşık tür örneği içeren veri özelliklerini tanımlar . Kavramsal modelde özellikleri, bir sınıf üzerinde tanımlanan özelliklere benzer. Bir sınıfındaki özellikleri sınıfı şeklini tanımlamak ve nesneler hakkında bilgi taşımak aynı şekilde, kavramsal model özelliklerinde bir varlık türü şeklini tanımlamak ve varlık türü örnekleri hakkında bilgi taşımak.

**Özelliği** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belge öğesi (izin verilen sıfır veya bir öğe)
-   Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)

Aşağıdaki özellikleri uygulanabilir bir **özelliği** öğesi: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **duyarlık**, **ölçek**, **Unicode**, **harmanlama**,  **ConcurrencyMode**. Modelleri veri deposunda özellik değerlerinin nasıl depolandığını konusunda bilgiler sağlayan XML öznitelikleridir.

> [!NOTE]
> Modelleri yalnızca türünün özelliklerine uygulanabilir **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **özelliği** öğesi.

| Öznitelik adı                                                         | Gereklidir | Değer                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                                                               | Evet         | Özelliğin adı.                                                                                                                                                                                                       |
| **Türü**                                                               | Evet         | Özellik değerinin türü. Özellik değeri türü olmalıdır bir **EDMSimpleType** veya modeli kapsamında olmayan (bir tam ad tarafından gösterilen) bir karmaşık türü.                                                 |
| **Boş değer atanabilir**                                                           | Hayır          | **Doğru** (varsayılan değer) veya <strong>False</strong> bağlı olup olmadığını özelliği null değeri olabilir. <br/> [!NOTE]                                                                                                   |
| > CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **defaultValue**                                                       | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                              |
| **maxLength**                                                          | Hayır          | Özellik değeri en büyük uzunluğu.                                                                                                                                                                                       |
| **FixedLength**                                                        | Hayır          | **Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.                                                                                                                          |
| **Duyarlık**                                                          | Hayır          | Özellik değerinin kesinliği.                                                                                                                                                                                            |
| **Ölçek**                                                              | Hayır          | Özellik değerinin ölçek.                                                                                                                                                                                                |
| **SRID**                                                               | Hayır          | Sistem uzamsal başvuru tanımlayıcısı. Yalnızca uzamsal tür özellikleri için geçerlidir. Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Hayır          | **Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.                                                                                                                               |
| **Harmanlama**                                                          | Hayır          | Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | Hayır          | **Hiçbiri** (varsayılan değer) veya **sabit**. Değer ayarlanmışsa **sabit**, iyimser eşzamanlılık denetimlerini özellik değeri kullanılacak.                                                                                  |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **özelliği** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityType** üç öğeyle **özelliği** öğeleri:

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
 

Aşağıdaki örnekte gösterildiği bir **ComplexType** beş öğe **özelliği** öğeleri:

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>RowType öğesinin uygulama

**Özellik** öğeleri (alt olarak bir **RowType** öğe) şekil ve geçirilen veya model tanımlı bir işlevden döndürülen verilerin özelliklerini tanımlar.  

**Özelliği** öğesi şu alt öğelerden tam olarak birine sahip olabilir:

-   CollectionType
-   referenceType
-   RowType

**Özelliği** öğesi sayı alt ek açıklama öğeleri olabilir.

> [!NOTE]
> Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.

 

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **özelliği** öğesi.

| Öznitelik adı                                                     | Gereklidir | Değer                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                                                           | Evet         | Özelliğin adı.                                                                                                                                                                                                       |
| **Türü**                                                           | Evet         | Özellik değerinin türü.                                                                                                                                                                                                 |
| **Boş değer atanabilir**                                                       | Hayır          | **Doğru** (varsayılan değer) veya **False** bağlı olup olmadığını özelliği null değeri olabilir. <br/> [!NOTE]                                                                                                                |
| > CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **defaultValue**                                                   | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                              |
| **maxLength**                                                      | Hayır          | Özellik değeri en büyük uzunluğu.                                                                                                                                                                                       |
| **FixedLength**                                                    | Hayır          | **Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.                                                                                                                          |
| **Duyarlık**                                                      | Hayır          | Özellik değerinin kesinliği.                                                                                                                                                                                            |
| **Ölçek**                                                          | Hayır          | Özellik değerinin ölçek.                                                                                                                                                                                                |
| **SRID**                                                           | Hayır          | Sistem uzamsal başvuru tanımlayıcısı. Yalnızca uzamsal tür özellikleri için geçerlidir. Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Hayır          | **Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.                                                                                                                               |
| **Harmanlama**                                                      | Hayır          | Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.                                                                                                                                                   |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **özelliği** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği **özelliği** model tanımlı bir işlevin dönüş türü şeklini tanımlamak için kullanılan öğeleri.

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
 

 

## <a name="propertyref-element-csdl"></a>PropertyRef öğesi (CSDL)

**PropertyRef** kavramsal şema tanım dili (CSDL) öğesinde başvuran bir özelliği bir varlık türünün özelliğini aşağıdaki rollerden biri gerçekleştirecek belirtmek için:

-   (Bir özellik veya bir varlık türünün kimliğini belirlemek özellikler kümesi) varlığın anahtarının bir parçası. Bir veya daha fazla **PropertyRef** öğeleri, bir varlığın anahtarı tanımlamak için kullanılabilir.
-   Başvuru kısıtlamasını bağımlı veya asıl bitiş olayı.

**PropertyRef** öğesi yalnızca alt öğeleri olarak ek açıklama öğelerinin (sıfır veya daha fazla) olabilir.

> [!NOTE]
> Ek açıklama öğelerinin yalnızca CSDL v2 ve daha sonra izin verilir.

 

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **PropertyRef** öğesi.

| Öznitelik adı | Gereklidir | Değer                                |
|:---------------|:------------|:-------------------------------------|
| **Ad**       | Evet         | Başvurulan özelliğin adı. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **PropertyRef** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek bir varlık türü tanımlar (**kitap**). Varlık anahtarı başvurarak tanımlanan **ISBN** varlık türü özelliği.

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
 

Sonraki örnekte, iki **PropertyRef** öğeleri ki belirtmek için kullanılan özellikler (**kimliği** ve **Publisherıd**) olan bir başvuru birincil ve bağımlı uçlarından kısıtlama.

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
 

 

## <a name="referencetype-element-csdl"></a>ReferenceType öğesi (CSDL)

**ReferenceType** kavramsal şema tanım dili (CSDL) öğesinde bir varlık türü bir başvuru belirtir. **ReferenceType** aşağıdaki öğelerin bir alt öğesi olabilir:

-   ReturnType (işlev)
-   Parametre
-   CollectionType

**ReferenceType** öğesi, bir parametre veya dönüş türü bir işlev için tanımlarken kullanılır.

A **ReferenceType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ReferenceType** öğesi.

| Öznitelik adı | Gereklidir | Değer                                         |
|:---------------|:------------|:----------------------------------------------|
| **Türü**       | Evet         | Başvurulan varlık türünün adı. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReferenceType** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği **ReferenceType** öğesi alt öğesi olarak kullanılan bir **parametre** başvuruyu kabul eden bir model tanımlı işlev öğesinde bir **kişi** varlık türü:

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
 

Aşağıdaki örnekte gösterildiği **ReferenceType** öğesi alt öğesi olarak kullanılan bir **ReturnType** (işlev) öğesi için bir başvuru döndürür model tanımlı bir işlevdeki bir **kişi**varlık türü:

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
 

 

## <a name="referentialconstraint-element-csdl"></a>Referentialconstraint'teki öğesi (CSDL)

A **Referentialconstraint'teki** kavramsal şema tanım dili (CSDL) öğesinde bir başvuru bütünlüğü kısıtlaması ilişkisel bir veritabanındaki benzer işlevselliği tanımlar. Bir veritabanı tablosundan bir sütuna (veya sütun) başka bir tablonun birincil anahtarı başvurabilirsiniz aynı şekilde, başka bir varlık türünün Varlık anahtarı bir varlık türünün bir özelliği (veya Özellikler) başvurabilir. Başvurulan varlık türü olarak adlandırılır *birincil ucu* kısıtlaması. Birincil ucu başvuran varlık türü olarak adlandırılan *bağımlı son* kısıtlaması.

Bir varlık türü üzerinde sunulan bir yabancı anahtar üzerindeki başka bir varlık türü, bir özellik başvuruyorsa **Referentialconstraint'teki** öğe iki varlık türleri arasındaki ilişkiyi tanımlar. Çünkü **Referentialconstraint'teki** öğesi sağlar nasıl iki varlık türleri hakkında bilgi ilişkilidir, karşılık gelen hiçbir AssociationSetMapping öğe eşleme belirtimi dili (MSL) gereklidir. Kullanıma sunulan bir yabancı anahtarları yoksa iki varlık türleri arasındaki ilişkiyi karşılık gelen olmalıdır **AssociationSetMapping** ilişkilendirme bilgileri veri kaynağına eşlemek için öğesi.

Bir varlık türünde, yabancı anahtar açık değilse **Referentialconstraint'teki** öğesi yalnızca bir varlık türü ve başka bir varlık türü arasında birincil anahtar birincil anahtar kısıtlaması tanımlayın.

A **Referentialconstraint'teki** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Asıl (tam olarak bir öğe)
-   Bağımlı (tam olarak bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

**Referentialconstraint'teki** öğesi herhangi bir sayıda ek açıklama öznitelikleri (özel XML öznitelikleri) olabilir. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **Referentialconstraint'teki** öğe tanımının bir parçası olarak kullanılan **yayınladığı** ilişkilendirme.

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
 

 

## <a name="returntype-function-element-csdl"></a>ReturnType (işlev) öğesi (CSDL)

**ReturnType** kavramsal şema tanım dili (CSDL) (işlev) öğesinde bir Function öğesinde tanımlanan bir işlev için dönüş türü belirtir. Bir işlevin dönüş türü de belirtilebilir bir **ReturnType** özniteliği.

Dönüş türleri herhangi olabilir **EdmSimpleType**, varlık türü, karmaşık tür, satır türü, başvuru türü veya bu tür bir koleksiyonu.

Bir işlevin dönüş türü ile belirtilebilir **türü** özniteliği **ReturnType** (işlev) öğesi veya şu alt öğelerden biri ile:

-   CollectionType
-   referenceType
-   RowType

> [!NOTE]
> İşlev dönüş türü ile her ikisini de belirtirseniz, bir model doğrulama değil **türü** özniteliği **ReturnType** (işlev) öğe ve alt öğelerden biri.

 

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ReturnType** (işlev) öğesi.

| Öznitelik adı | Gereklidir | Değer                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | Hayır          | İşlev tarafından döndürülen tür. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReturnType** (işlev) öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte bir **işlevi** bir kitap, yazdırma rolünüzün yıl sayısını döndüren bir işlev tanımlamak için. Dönüş türü tarafından belirtilen Not **türü** özniteliği bir **ReturnType** (işlev) öğesi.

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (Functionımport) öğesi (CSDL)

**ReturnType** kavramsal şema tanım dili (CSDL) (Functionımport) öğesinde bir Functionımport öğesinde tanımlanan bir işlev için dönüş türü belirtir. Bir işlevin dönüş türü de belirtilebilir bir **ReturnType** özniteliği.

Dönüş türleri varlık türü, karmaşık tür herhangi bir koleksiyon olabilir veya **EdmSimpleType**,

Bir işlevin dönüş türü belirtilmiş **türü** özniteliği **ReturnType** (Functionımport) öğesi.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ReturnType** (Functionımport) öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Türü**       | Hayır          | İşlevinin döndürdüğü türü. Değer, ComplexType, EntityType veya EDMSimpleType koleksiyonu olmalıdır.                                                                                      |
| **EntitySet**  | Hayır          | Varlık koleksiyonu işlevi döndürürse, türleri, değerini **EntitySet** koleksiyona ait olduğu varlık kümesi gerekir. Aksi takdirde, **EntitySet** özniteliği değil kullanılmalıdır. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReturnType** (Functionımport) öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte bir **Functionımport** kitaplardan ve yayımcılar döndürür. İşlev iki sonucu kümesi ve bu nedenle iki döndürmediğine dikkat edin **ReturnType** (Functionımport) öğeleri belirtilir.

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>RowType öğesinin (CSDL)

A **RowType** kavramsal şema tanım dili (CSDL) öğesinde, bir parametre veya kavramsal modelde tanımlı bir işlevin dönüş türü olarak adlandırılmamış bir yapı tanımlar.

A **RowType** aşağıdaki öğeleri alt öğesi olabilir:

-   CollectionType
-   Parametre
-   ReturnType (işlev)

A **RowType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Özellik (bir veya daha fazla)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **RowType** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğe (belirtilmiş **RowType** öğesi).

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

## <a name="schema-element-csdl"></a>Şema öğesi (CSDL)

**Şema** öğenin kavramsal model tanımı kök öğesidir. Bu nesneleri, işlevleri ve kavramsal bir modeli olun kapsayıcıları için tanımlar içerir.

**Şema** öğesi sıfır veya daha fazla şu alt öğelerden birini içerebilir:

-   kullanma
-   EntityContainer
-   entityType
-   EnumType
-   İlişkilendirme
-   ComplexType
-   İşlev

A **şema** öğesi sıfır veya bir ek açıklama öğeleri içerebilir.

> [!NOTE]
> **İşlevi** öğesi ve ek açıklama öğeleri yalnızca izin CSDL v2'de ve sonraki sürümler.

 

**Şema** öğesini kullanan **Namespace** kavramsal modelde varlık türü, karmaşık türü ve ilişkisi nesneleri için ad alanını tanımlayan öznitelik. Ad alanı içinde hiçbir iki nesnenin aynı ada sahip olabilir. Ad alanları, birden çok yayılabilir **şema** öğelerini ve birden çok .csdl dosyaları.

Kavramsal model ad alanı XML ad alanından farklıdır **şema** öğesi. Kavramsal model ad alanı (tarafından tanımlandığı gibi **Namespace** özniteliği) ilişki türleri varlık türleri ve karmaşık türler için mantıksal bir kapsayıcıdır. XML ad alanı (tarafından belirtilen **xmlns** özniteliği), bir **şema** öğedir alt öğeleri ve öznitelikleri için varsayılan ad alanı **şema** öğesi. XML ad alanları formun http://schemas.microsoft.com/ado/YYYY/MM/edm (burada YYYY ve MM yıl ve ay sırasıyla temsil eder) CSDL için ayrılmıştır. Bu forma sahip ad alanları, özel öğeleri ve öznitelikleri olamaz.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda öznitelikleri açıklar uygulanabilir **şema** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Evet         | Kavramsal modelin ad alanı. Değerini **Namespace** özniteliği, bir türün tam adı oluşturmak için kullanılır. Örneğin, bir **EntityType** adlı *müşteri* Simple.Example.Model ad alanınıza ve sonra tam adı olduğunu **EntityType** olduğu SimpleExampleModel.Customer. <br/> Aşağıdaki dize değeri olarak kullanılamaz **Namespace** özniteliği: **sistem**, **geçici**, veya **Edm**. Değeri **Namespace** öznitelik değeri olarak aynı olamaz **Namespace** SSDL şeması öğesindeki özniteliği. |
| **Diğer ad**      | Hayır          | Ad alanı adı yerine kullanılan tanımlayıcıdır. Örneğin, bir **EntityType** adlı *müşteri* Simple.Example.Model ad alanı ve değerini **diğer** özniteliği *modeli*, tam nitelikli adı olarak Model.Customer kullanabilirsiniz **EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **şema** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **şema** öğesini içeren bir **EntityContainer** öğesinde, iki **EntityType** öğeleri ve bir **ilişkilendirme** öğesi.

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
 

 

## <a name="typeref-element-csdl"></a>TypeRef öğesi (CSDL)

**TypeRef** kavramsal şema tanım dili (CSDL) öğesinde türü adlı varolan bir başvuru sağlar. **TypeRef** bir işlev bir parametre veya dönüş türü bir koleksiyon olduğunu belirtmek için kullanılan CollectionType öğesinin alt öğesi olabilir.

A **TypeRef** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **TypeRef** öğesi. Unutmayın **DefaultValue**, **MaxLength**, **FixedLength**, **duyarlık**, **ölçek**,  **Unicode**, ve **harmanlama** öznitelikleri için uygun yalnızca **EDMSimpleTypes**.

| Öznitelik adı                                                     | Gereklidir | Değer                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Türü**                                                           | Hayır          | Başvurulan tür adı.                                                                                                                                                                                          |
| **Boş değer atanabilir**                                                       | Hayır          | **Doğru** (varsayılan değer) veya **False** bağlı olup olmadığını özelliği null değeri olabilir. <br/> [!NOTE]                                                                                                                |
| > CSDL v1 içinde bir karmaşık tür özelliği sağlanmalıdır. `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **defaultValue**                                                   | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                              |
| **maxLength**                                                      | Hayır          | Özellik değeri en büyük uzunluğu.                                                                                                                                                                                       |
| **FixedLength**                                                    | Hayır          | **Doğru** veya **False** bağlı olarak sabit uzunlukta bir dize olarak özellik değeri depolanmış.                                                                                                                          |
| **Duyarlık**                                                      | Hayır          | Özellik değerinin kesinliği.                                                                                                                                                                                            |
| **Ölçek**                                                          | Hayır          | Özellik değerinin ölçek.                                                                                                                                                                                                |
| **SRID**                                                           | Hayır          | Sistem uzamsal başvuru tanımlayıcısı. Yalnızca uzamsal tür özellikleri için geçerlidir. Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Hayır          | **Doğru** veya **False** bağlı olarak özellik değeri bir Unicode dize olarak depolanmış.                                                                                                                               |
| **Harmanlama**                                                      | Hayır          | Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.                                                                                                                                                   |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **CollectionType** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, kullanan bir model tanımlı işlev gösterir. **TypeRef** öğesi (alt öğesi olarak bir **CollectionType** öğesi) işlev koleksiyonunu kabul belirtmek için  **Departman** varlık türleri.

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
 

 

## <a name="using-element-csdl"></a>Öğesi (CSDL) kullanma

**Kullanma** kavramsal şema tanım dili (CSDL) öğe farklı bir ad alanında bulunan bir kavramsal model içeriğini içeri aktarır. Değerini ayarlayarak **Namespace** özniteliği başvurabilirsiniz varlık türleri ve karmaşık türler başka bir kavramsal modelde tanımlı ilişki türleri için. Birden fazla **kullanma** öğesi alt öğesi olabilir bir **şema** öğesi.

> [!NOTE]
> **Kullanma** CSDL öğesinde tıpkı çalışmaz bir **kullanarak** bir programlama dili deyimi. Bir ad alanı ile alarak bir **kullanarak** deyimi bir programlama dili, özgün ad alanındaki nesneleri etkilemez. CSDL bir içeri aktarılan ad alanı özgün ad alanında bir varlık türünden türetilmiş bir varlık türü içerebilir. Bu, özgün ad alanında bildirilen varlık kümeleri etkileyebilir.

 

**Kullanma** öğesi şu alt öğelerden olabilir:

-   Belgeleri (izin verilen sıfır veya bir öğe)
-   Ek açıklama öğelerinin (izin verilen sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda öznitelikleri açıklar uygulanabilir **kullanma** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Evet         | İçeri aktarılan ad alanının adı.                                                                                                                                                |
| **Diğer ad**      | Evet         | Ad alanı adı yerine kullanılan tanımlayıcıdır. Bu öznitelik gerekli olsa da, bunu değil, ad alanı adı yerine nesne adlarını nitelemek için kullanılması gerekir. |

 

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **kullanma** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, gösterir **kullanma** öğesi bir ad alanı içeri aktarmak için kullanılan başka bir yerde tanımlanmış. Unutmayın ad alanı için **şema** gösterilen öğe `BooksModel`. `Address` Özelliği `Publisher` **EntityType** tanımlanan karmaşık bir türdür `ExtendedBooksModel` ad alanı (içeri **kullanma** öğesi).

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
 

 

## <a name="annotation-attributes-csdl"></a>Ek açıklama öznitelikleri (CSDL)

Ek açıklama özniteliklerinde kavramsal şema tanım dili (CSDL) kavramsal model özel XML öznitelikleri ' dir. Geçerli XML yapısına sahip olmaya ek olarak, aşağıdaki ek açıklama özniteliklerinin doğru olması gerekir:

-   Ek açıklama öznitelikleri CSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.
-   Ek açıklama birden fazla öznitelik verilen bir CSDL öğesine uygulanabilir.
-   Her iki ek açıklama özniteliklerin tam adları aynı olmamalıdır.

Ek açıklama öznitelikleri, kavramsal modeldeki öğeleri hakkında ek meta verilerini sağlamak için kullanılabilir. Ek açıklama öğesinde bulunan meta veriler, çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityType** bir ek açıklama özniteliği olan öğe (**CustomAttribute**). Örnek ayrıca varlık türü öğeye uygulanan bir ek açıklama öğesi gösterir.

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
 

Aşağıdaki kod ek açıklama özniteliği meta verilerini alır ve konsola yazar:

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
 

Yukarıdaki kod olduğunu varsayar `School.csdl` dosya projenin çıkış dizinine ve aşağıdaki eklediğiniz `Imports` ve `Using` projenize ifadeleri:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Ek açıklama öğelerinin (CSDL)

Özel XML öğeleri kavramsal modeldeki kavramsal şema tanım dili (CSDL) içinde ek açıklama öğeleri şunlardır: Geçerli XML yapısına sahip olmaya ek olarak, aşağıdaki ek açıklama öğeleri doğru olması gerekir:

-   Ek açıklama öğelerinin CSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.
-   Birden çok ek açıklama öğesi, belirli bir CSDL öğesinin bir alt öğesi olabilir.
-   Her iki ek açıklama öğelerinin tam adları aynı olmamalıdır.
-   Diğer tüm alt öğeleri verilen CSDL öğenin sonra ek açıklama öğelerinin görünmesi gerekir.

Ek açıklama öğelerinin kavramsal modeldeki öğeleri hakkında ek meta verilerini sağlamak için kullanılabilir. .NET Framework sürüm 4 ile başlayarak, ek açıklama öğesinde bulunan meta veriler çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityType** öğesi ile bir ek açıklama öğesi (**CustomElement**). Örnek ayrıca varlık türü öğeye uygulanan bir ek açıklama özniteliği gösterir.

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
 

Aşağıdaki kod, ek açıklama öğesinde bulunan meta verileri alır ve konsola yazar:

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
 

Yukarıdaki kod School.csdl dosya projenin çıkış dizinine olduğunu ve aşağıdaki eklediğinizi varsayar `Imports` ve `Using` projenize ifadeleri:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Kavramsal Model türleri (CSDL)

Kavramsal şema tanım dili (CSDL) adı verilen soyut temel veri türleri kümesi destekler **EDMSimpleTypes**, kavramsal modelde özellikleri tanımlar. **EDMSimpleTypes** proxy'ler ilgili daha fazla bilgi için depolama veya barındırma ortamında desteklenen temel veri türleri.

Aşağıdaki tabloda, CSDL tarafından desteklenen temel veri türlerini listeler. Tabloda ayrıca her uygulanan özellikleri listeler **EDMSimpleType**.

| EDMSimpleType                    | Açıklama                                                | Geçerli modelleri                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **Edm.Binary**                   | İkili veriler içerir.                                      | MaxLength, FixedLength, null, varsayılan                                |
| **Edm.Boolean**                  | Değeri içeren **true** veya **false**.                  | Boş değer atanabilir, varsayılan                                                        |
| **Edm.Byte**                     | İmzalanmamış 8 bit tam sayı değeri içerir.                  | Duyarlık, null, varsayılan                                             |
| **Edm.DateTime**                 | Tarih ve saati temsil eder.                                | Duyarlık, null, varsayılan                                             |
| **Edm.DateTimeOffset**           | Bir tarih ve saat olarak GMT'den dakikalar içinde bir uzaklık içerir. | Duyarlık, null, varsayılan                                             |
| **Edm.Decimal**                  | Sabit kesinlik ve ölçek ile sayısal bir değer içeriyor.   | Duyarlık, null, varsayılan                                             |
| **Edm.Double**                   | Kayan nokta ile 15 basamaklı duyarlık sayı içerir   | Duyarlık, null, varsayılan                                             |
| **Edm.Float**                    | Kayan noktalı sayı 7 basamaklı duyarlık içerir.   | Duyarlık, null, varsayılan                                             |
| **Edm.Guid**                     | 16 baytlık benzersiz bir tanımlayıcı içerir.                      | Duyarlık, null, varsayılan                                             |
| **Edm.Int16**                    | İşaretli 16 bit tam sayı değeri içerir.                    | Duyarlık, null, varsayılan                                             |
| **EDM.Int32**                    | İşaretli 32-bit tamsayı değeri içerir.                    | Duyarlık, null, varsayılan                                             |
| **EDM.Int64**                    | Bir 64-bit işaretli tamsayı değeri içerir.                    | Duyarlık, null, varsayılan                                             |
| **Edm.SByte**                    | İşaretli 8 bit tam sayı değeri içerir.                     | Duyarlık, null, varsayılan                                             |
| **Edm.String**                   | Karakter verileri içerir.                                   | Varsayılan Unicode, FixedLength, MaxLength, harmanlaması, boş değer atanabilir, duyarlık |
| **Edm.Time**                     | Günün bir saati içerir.                                    | Duyarlık, null, varsayılan                                             |
| **Edm.Geography**                |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeographyPoint**           |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeographyLineString**      |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeographyPolygon**         |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeographyMultiPoint**      |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeographyMultiLineString** |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeographyMultiPolygon**    |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeographyCollection**      |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.Geometry**                 |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeometryPoint**            |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeometryLineString**       |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeometryPolygon**          |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeometryMultiPoint**       |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeometryMultiLineString**  |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeometryMultiPolygon**     |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |
| **Edm.GeometryCollection**       |                                                            | Boş değer atanabilir, varsayılan, SRID                                                  |

## <a name="facets-csdl"></a>Modelleri (CSDL)

Kavramsal şema tanım dili (CSDL), modelleri özelliklerini varlık türlerinde ve karmaşık türlerde kısıtlamaları temsil eder. Modelleri, XML öznitelikleri aşağıdaki CSDL öğeleri olarak görünür:

-   Özellik
-   TypeRef
-   Parametre

Aşağıdaki tabloda CSDL desteklenen özellikleri açıklar. Tüm özellikleri isteğe bağlıdır. Aşağıda listelenen bazı modeller bir kavramsal modelde bir veritabanı oluşturma varlık çerçevesi tarafından kullanılır.

> [!NOTE]
> Kavramsal modelde veri türleri hakkında daha fazla bilgi için bkz: kavramsal Model türleri (CSDL).

| modeli               | Açıklama                                                                                                                                                                                                                                                   | Uygulandığı öğe:                                                                                                                                                                                                                                                                                                                                                                           | Veritabanı oluşturmak için kullanılan | Çalışma zamanı tarafından kullanılan |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **Harmanlama**       | Harmanlama dizisi (veya sıralama) yapılırken kullanılacak karşılaştırma gerçekleştirme ve özellik değerleri üzerinde işlem sıralama belirtir.                                                                                                               | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Evet                              | Hayır                  |
| **ConcurrencyMode** | Özelliğinin değeri için iyimser eşzamanlılık denetimlerinin kullanılması gerektiğini gösterir.                                                                                                                                                                    | Tüm **EDMSimpleType** özellikleri                                                                                                                                                                                                                                                                                                                                                     | Hayır                               | Evet                 |
| **Default**         | Örnek oluşturma sırasında herhangi bir değer sağlanmazsa, özelliğin varsayılan değerini belirtir.                                                                                                                                                                       | Tüm **EDMSimpleType** özellikleri                                                                                                                                                                                                                                                                                                                                                     | Evet                              | Evet                 |
| **FixedLength**     | Özellik değerinin uzunluğu değişebilir olup olmadığını belirtir.                                                                                                                                                                                                  | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Evet                              | Hayır                  |
| **maxLength**       | Özellik değeri en büyük uzunluğunu belirtir.                                                                                                                                                                                                           | **Edm.Binary**, **Edm.String**                                                                                                                                                                                                                                                                                                                                                       | Evet                              | Hayır                  |
| **Boş değer atanabilir**        | Özelliğine sahip olup olmadığını belirten bir **null** değeri.                                                                                                                                                                                                     | Tüm **EDMSimpleType** özellikleri                                                                                                                                                                                                                                                                                                                                                     | Evet                              | Evet                 |
| **Duyarlık**       | Tür özellikleri için **ondalık**, bir özellik değeri olabilir basamak sayısını belirtir. Tür özellikleri için **zaman**, **DateTime**, ve **DateTimeOffset**, özellik değerinin saniye kesirli kısmını için basamak sayısını belirtir. | **Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**                                                                                                                                                                                                                                                                                                              | Evet                              | Hayır                  |
| **Ölçek**           | Özellik değeri ondalık noktasının sağındaki basamak sayısını belirtir.                                                                                                                                                                      | **Edm.Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Evet                              | Hayır                  |
| **SRID**            | Uzamsal sistem başvuru sistemi kimliğini belirtir. Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **Edm.Geography Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection** | Hayır                               | Evet                 |
| **Unicode**         | Özellik değeri Unicode olarak mi depolanacağını belirtir.                                                                                                                                                                                                    | **Edm.String**                                                                                                                                                                                                                                                                                                                                                                       | Evet                              | Evet                 |

>[!NOTE]
> Veritabanı Oluştur Sihirbazı'nı bir veritabanı kavramsal bir modeli oluşturulurken değerini tanıyacağınız **StoreGeneratedPattern** özniteliği bir **özelliği** aşağıdaki ise öğe ad alanı: http://schemas.microsoft.com/ado/2009/02/edm/annotation. Özniteliği için desteklenen değerler şunlardır: **kimlik** ve **hesaplanan**. Değerini **kimlik** veritabanında oluşturulan bir kimlik değeri olan bir veritabanı sütunu üretecektir. Değerini **hesaplanan** veritabanında hesaplanan bir değere sahip bir sütun oluşturur.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir varlık türünün özelliklerine uygulanan özellikleri gösterir:

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
