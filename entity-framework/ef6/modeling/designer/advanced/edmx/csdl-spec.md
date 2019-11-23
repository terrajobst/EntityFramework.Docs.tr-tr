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
# <a name="csdl-specification"></a>CSDL Belirtimi
Kavramsal şema tanım dili (CSDL), veri temelli bir uygulamanın kavramsal modelini oluşturan varlıkları, ilişkileri ve işlevleri açıklayan XML tabanlı bir dildir. Bu kavramsal model Entity Framework veya WCF Veri Hizmetleri tarafından kullanılabilir. CSDL ile açıklanan meta veriler, kavramsal modelde tanımlanan varlıkları ve ilişkileri bir veri kaynağıyla eşlemek için Entity Framework tarafından kullanılır. Daha fazla bilgi için bkz. [SSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) ve [MSL belirtimi](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL, Entity Framework Varlık Veri Modeli uygulamasıdır.

Entity Framework bir uygulamada, kavramsal model meta verileri bir. csdl dosyasından (CSDL içinde yazılmış) System. Data. Metadata. Edm. EdmItemCollection örneğine yüklenir ve içindeki yöntemler kullanılarak erişilebilir. System. Data. Metadata. Edm. MetadataWorkspace sınıfı. Entity Framework, sorguları kavramsal modele veri kaynağına özgü komutlara dönüştürmek için kavramsal model meta verilerini kullanır.

EF Designer, kavramsal model bilgilerini tasarım zamanında bir. edmx dosyasında depolar. Yapı zamanında EF Designer, çalışma zamanında Entity Framework gereken. csdl dosyasını oluşturmak için bir. edmx dosyasındaki bilgileri kullanır.

CSDL sürümleri, XML ad alanları ile farklılaştırılabilir.

| CSDL sürümü | XML ad alanı                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm |
| CSDL v2      | https://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Association öğesi (CSDL)

**İlişkilendirme** öğesi iki varlık türü arasındaki ilişkiyi tanımlar. Bir ilişki, ilişkiye dahil olan varlık türlerini ve ilişkinin her ucunda çoğulluk olarak bilinen varlık türlerinin olası sayısını belirtmelidir. Bir ilişki ucunun çoğulluğu bir (1), sıfır veya bir (0.. 1) veya çok (\*) bir değere sahip olabilir. Bu bilgiler iki alt End öğesinde belirtilir.

Bir ilişkinin bir sonundaki varlık türü örneklerine, bir varlık türü üzerinde gösterilmeleri durumunda gezinti özellikleri veya yabancı anahtarlar üzerinden erişilebilir.

Bir uygulamada, bir ilişkinin örneği varlık türü örnekleri arasındaki belirli bir ilişkilendirmeyi temsil eder. İlişki örnekleri bir ilişki kümesinde mantıksal olarak gruplandırılır.

Bir **ilişkilendirme** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   End (tam olarak 2 öğe)
-   ReferentialConstraint (sıfır veya bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **ilişkilendirme** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                        |
|:---------------|:------------|:-----------------------------|
| **Ad**       | Evet         | İlişkilendirmenin adı. |

 

> [!NOTE]
> **İlişkilendirme** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, **Müşteri** ve **sipariş** varlık türlerinde yabancı anahtarlar gösterilmediği zaman **CustomerOrders** ilişkilendirmesini tanımlayan bir **ilişki** öğesini gösterir. İlişkilendirmenin her bir **ucunun** **çoğulluğu** değeri, bir **müşteriyle**birçok **siparişin** Ilişkilendirilemeyeceğini gösterir, ancak bir **siparişle**yalnızca bir **Müşteri** ilişkilendirilebilen anlamına gelebilir. Ayrıca, **OnDelete** öğesi belirli bir **müşteriyle** ilgili olan ve ObjectContext 'e yüklenmiş tüm **siparişlerin** , **Müşteri** silinirse silinecek olduğunu gösterir.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

Aşağıdaki örnek, **Müşteri** ve **sipariş** varlık türlerinde yabancı anahtarlar açık olduğunda **CustomerOrders** ilişkilendirmesini tanımlayan bir **ilişki** öğesi gösterir. Yabancı anahtarlar kullanıma sunulduğunda, varlıklar arasındaki ilişki bir **ReferentialConstraint** öğesiyle yönetilir. Bu ilişkilendirmeyi veri kaynağıyla eşlemek için karşılık gelen bir AssociationSetMapping öğesi gerekli değildir.

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

Kavramsal şema tanım dili (CSDL) içindeki **AssociationSet** öğesi, aynı türde ilişki örnekleri için mantıksal bir kapsayıcıdır. Bir ilişki kümesi, ilişki örneklerinin bir veri kaynağıyla eşleştiribilecekleri şekilde gruplandırılması için bir tanım sağlar.  

**AssociationSet** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe izin verilir)
-   End (tam olarak iki öğe gereklidir)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)

**Association** özniteliği bir ilişki kümesinin içerdiği ilişkilendirmenin türünü belirtir. Bir ilişki kümesinin uçlarını oluşturan varlık kümeleri, tam olarak iki alt **End** öğesi ile belirtilir.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **AssociationSet** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı  | Gereklidir | Değer                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**        | Evet         | Varlık kümesinin adı. **Name** özniteliğinin değeri **Association** özniteliğinin değeriyle aynı olamaz.                                 |
| **İlişkilendirme** | Evet         | İlişki kümesinin örnekleri içerdiği ilişkilendirmenin tam nitelikli adı. İlişki, ilişki kümesiyle aynı ad alanında olmalıdır. |

 

> [!NOTE]
> **AssociationSet** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, iki **AssociationSet** öğesiyle bir **EntityContainer** öğesi gösterir:

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

Kavramsal şema tanım dili (CSDL) içindeki **CollectionType** öğesi, bir işlev parametresi veya işlev dönüş türünün bir koleksiyon olduğunu belirtir. **CollectionType** öğesi, Parameter öğesinin veya ReturnType (Function) öğesinin bir alt öğesi olabilir. Koleksiyon türü, **Type** özniteliği ya da aşağıdaki alt öğelerinden biri kullanılarak belirtilebilir:

-   **Türünde**
-   referenceType
-   RowType
-   Değerini

> [!NOTE]
> Bir koleksiyon türünün hem **tür** özniteliği hem de bir alt öğe ile belirtilmesi durumunda model doğrulanmaz.

 

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **CollectionType** öğesine uygulanabilen öznitelikler açıklanmaktadır. **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **UNICODE**ve **harmanlama** özniteliklerinin yalnızca **edmsimpletypes**koleksiyonları için geçerli olduğunu unutmayın.

| Öznitelik adı                                                          | Gereklidir | Değer                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tür**                                                                | Hayır          | Koleksiyonun türü.                                                                                                                                                                                                      |
| **Yapılamaz**                                                            | Hayır          | **True** (varsayılan değer) veya özelliğin NULL değere sahip olmasına bağlı olarak **false** . <br/> [!NOTE]                                                                                                                 |
| CSDL V1 >, karmaşık bir tür özelliği `Nullable="False"`sahip olmalıdır. |             |                                                                                                                                                                                                                                  |
| **Değerinin**                                                        | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                               |
| **'In**                                                           | Hayır          | Özellik değerinin uzunluk üst sınırı.                                                                                                                                                                                        |
| **FixedLength**                                                         | Hayır          | Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                           |
| **Duyarlılık**                                                           | Hayır          | Özellik değerinin duyarlığı.                                                                                                                                                                                             |
| **Ölçek**                                                               | Hayır          | Özellik değerinin ölçeği.                                                                                                                                                                                                 |
| **SRıD**                                                                | Hayır          | Uzamsal sistem başvuru tanımlayıcısı. Yalnızca uzamsal türlerin özellikleri için geçerlidir.   Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | Hayır          | Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                                |
| **Mediğinden**                                                           | Hayır          | Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.                                                                                                                                                    |

 

> [!NOTE]
> **CollectionType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, işlevin bir **kişi** varlık türleri koleksiyonunu ( **ElementType** özniteliğiyle belirtilen şekilde) döndürdüğünü belirtmek için bir **CollectionType** öğesi kullanan model tanımlı bir işlevi gösterir.

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
 

Aşağıdaki örnek, işlevin satır koleksiyonunu ( **RowType** öğesinde belirtilen şekilde) döndürdüğünü belirtmek Için bir **CollectionType** öğesi kullanan model tanımlı bir işlevi gösterir.

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
 

Aşağıdaki örnek, işlevin bir **Departman** varlık türleri koleksiyonu olarak kabul ettiğini belirtmek için **CollectionType** öğesini kullanan model tanımlı bir işlevi gösterir.

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

Bir **complexType** öğesi, **edmsimpletype** özelliklerinden veya diğer karmaşık türlerden oluşan bir veri yapısını tanımlar.  Karmaşık tür bir varlık türünün özelliği ya da başka bir karmaşık tür olabilir. Karmaşık bir tür, verileri tanımlayan bir varlık türüne benzerdir. Ancak, karmaşık türler ve varlık türleri arasında bazı önemli farklılıklar vardır:

-   Karmaşık türlerin kimlikleri (veya anahtarları) yoktur ve bu nedenle bağımsız olarak bulunamaz. Karmaşık türler yalnızca varlık türlerinin veya diğer karmaşık türlerin özellikleri olarak bulunabilir.
-   Karmaşık türler ilişkilendirmelere katılamaz. İlişkinin sonu ne bir karmaşık tür olamaz, bu nedenle gezinti özellikleri karmaşık türler için tanımlanamaz.
-   Karmaşık tür özelliği null değere sahip olamaz, ancak karmaşık bir türdeki skalar özellikler her biri null olarak ayarlanabilir.

Bir **complexType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Özellik (sıfır veya daha fazla öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

Aşağıdaki tabloda, **complexType** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı                                                                                                 | Gereklidir | Değer                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name                                                                                                           | Evet         | Karmaşık türün adı. Karmaşık bir türün adı, modelin kapsamı içinde olan başka bir karmaşık türün, varlık türünün veya ilişkilendirmenin adı ile aynı olamaz. |
| BaseType                                                                                                       | Hayır          | Tanımlanmakta olan karmaşık türün temel türü olan başka bir karmaşık türün adı. <br/> [!NOTE]                                                                     |
| > Bu öznitelik CSDL v1 'de geçerli değildir. Karmaşık türlerin devralınması bu sürümde desteklenmiyor. |             |                                                                                                                                                                                     |
| Özet                                                                                                       | Hayır          | Karmaşık türün soyut bir tür olmasına bağlı olarak **doğru** veya **yanlış** (varsayılan değer). <br/> [!NOTE]                                                                  |
| > Bu öznitelik CSDL v1 'de geçerli değildir. Bu sürümdeki karmaşık türler soyut tür olamaz.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> **ComplexType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, **Edmsimpletype** özellikleri **StreetAddress**, **City**, **stateoreyalet**, **ülke**ve **PostaKodu**olan karmaşık bir tür, **Adres**gösterir.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Karmaşık tür **adresini** (yukarıdaki) bir varlık türünün özelliği olarak tanımlamak için varlık türü tanımında Özellik türünü bildirmeniz gerekir. Aşağıdaki örnek, **Adres** özelliğini bir varlık türü (**Yayımcı**) üzerinde karmaşık bir tür olarak göstermektedir:

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

Kavramsal şema tanım dili (CSDL) içindeki **DefiningExpression** öğesi, kavramsal modeldeki bir işlevi tanımlayan bir Entity SQL ifadesi içerir.  

> [!NOTE]
> Doğrulama amacıyla, bir **DefiningExpression** öğesi rastgele içerik içerebilir. Ancak, bir **DefiningExpression** öğesi geçerli Entity SQL içermiyorsa, Entity Framework çalışma zamanında bir özel durum oluşturur.

 

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

**DefiningExpression** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir kitabın Yayınlandığı tarihten itibaren yıl sayısını döndüren bir işlev tanımlamak için bir **DefiningExpression** öğesi kullanmaktadır. **DefiningExpression** öğesinin içeriği Entity SQL yazılır.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Bağımlı öğe (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **bağımlı** öğe, ReferentialConstraint öğesinin bir alt öğesidir ve bir başvuru kısıtlamasının bağımlı sonunu tanımlar. Bir **ReferentialConstraint** öğesi, ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamasına benzer işlevselliği tanımlar. Bir veritabanı tablosundaki bir sütunun (veya sütunlarının) başka bir tablonun birincil anahtarına başvurmasına benzer şekilde, bir varlık türünün özelliği (veya özellikleri) başka bir varlık türünün varlık anahtarına başvurabilir. Başvurulan varlık türüne kısıtlamanın *asıl sonu* denir. Asıl ucuna başvuran varlık türüne kısıtlamanın *bağımlı sonu* denir. **Propertyref** öğeleri, asıl uca hangi anahtarların başvurulacağını belirtmek için kullanılır.

**Bağımlı** öğe aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   PropertyRef (bir veya daha fazla öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **bağımlı** öğeye uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rol**       | Evet         | İlişkinin bağımlı ucundaki varlık türünün adı. |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **bağımlı** öğeye uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, **PublishedBy** Association tanımının bir parçası olarak kullanılan **ReferentialConstraint** öğesini gösterir. **Kitap** varlık türünün **publisherID** özelliği, başvuru kısıtlamasının bağımlı sonunu oluşturur.

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
 

 

## <a name="documentation-element-csdl"></a>Documentation öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **Belgeler** öğesi, bir üst öğede tanımlanan bir nesne hakkında bilgi sağlamak için kullanılabilir. Bir. edmx dosyasında, **documentation** öğesi EF Designer 'ın tasarım yüzeyinde (bir varlık, ilişkilendirme veya özellik gibi) bir nesne olarak görünen bir öğenin alt öğesi olduğunda, **belge** öğesinin Içeriği nesnenin Visual Studio **özellikleri** penceresinde görünür.

**Belge** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   **Özet**: üst öğenin kısa bir açıklaması. (sıfır veya bir öğe)
-   **LongDescription**: üst öğenin kapsamlı bir açıklaması. (sıfır veya bir öğe)
-   Ek açıklama öğeleri. (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

**Belge** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir EntityType öğesinin alt öğesi olarak **documentation** öğesini göstermektedir. Aşağıdaki kod parçacığı bir. edmx dosyasının CSDL içeriklerinde olsaydı, `Customer` varlık türüne tıkladığınızda **Summary** ve **LongDescription** öğelerinin içeriği Visual Studio **Özellikler** penceresinde görünür.

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
 

 

## <a name="end-element-csdl"></a>End öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **End** öğesi Association öğesinin veya AssociationSet öğesinin bir alt öğesi olabilir. Her durumda, **End** öğesinin rolü farklıdır ve ilgili öznitelikler farklıdır.

### <a name="end-element-as-a-child-of-the-association-element"></a>Ilişki öğesinin bir alt öğesi olarak end öğesi

Bir **End** öğesi ( **Association** öğesinin bir alt öğesi olarak), ilişkinin bir sonundaki varlık türünü ve bir ilişkinin o ucunda bulunabilir varlık türü örneklerinin sayısını tanımlar. İlişki uçları bir ilişkinin parçası olarak tanımlanır; bir ilişkilendirme tam olarak iki ilişkilendirme bitmelidir. Bir ilişkinin bir sonundaki varlık türü örneklerine, bir varlık türü üzerinde gösterilmeleri durumunda gezinti özellikleri veya yabancı anahtarlar üzerinden erişilebilir.  

Bir **End** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   OnDelete (sıfır veya bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, bir **ilişkilendirme** öğesinin alt öğesi olduğunda, **End** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tür**         | Evet         | İlişkinin bir sonundaki varlık türünün adı.                                                                                                                                                                                                                                                                                                                                                         |
| **Rol**         | Hayır          | İlişki ucu için bir ad. Ad sağlanmazsa, ilişki uçtaki varlık türünün adı kullanılacaktır.                                                                                                                                                                                                                                                                                           |
| **Ğunun** | Evet         | **1**, **0.. 1**veya ilişkinin sonunda olabilecek varlık türü örneklerinin sayısına göre **\*** . <br/> **1** ilişki ucunda tam olarak bir varlık türü örneğinin bulunduğunu gösterir. <br/> **0.. 1** , ilişkilendirme ucunda sıfır veya bir varlık türü örneklerinin bulunduğunu gösterir. <br/> **\*** , ilişkilendirme ucunda sıfır, bir veya daha fazla varlık türü örneğinin var olduğunu belirtir. |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **End** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnek, **CustomerOrders** ilişkilendirmesini tanımlayan bir **ilişki** öğesini gösterir. İlişkilendirmenin her bir **ucunun** **çoğulluğu** değeri, bir **müşteriyle**birçok **siparişin** Ilişkilendirilemeyeceğini gösterir, ancak bir **siparişle**yalnızca bir **Müşteri** ilişkilendirilebilen anlamına gelebilir. Ayrıca, **OnDelete** öğesi, **Müşteri** silinirse, belirli bir **müşteriyle** ilgili olan ve ObjectContext 'e yüklenmiş tüm **siparişlerin** silineceğini belirtir.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Öğeyi AssociationSet öğesinin bir alt öğesi olarak bitir

**End** öğesi bir ilişki kümesinin sonunu belirtir. **AssociationSet** öğesi iki **End** öğesi içermelidir. Bir **End** öğesinde yer alan bilgiler bir veri kaynağına yönelik bir ilişki kümesini eşlerken kullanılır.

Bir **End** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

> [!NOTE]
> Ek açıklama öğeleri diğer tüm alt öğelerden sonra gelmelidir. Ek açıklama öğelerine yalnızca CSDL v2 ve sonrasında izin verilir.

 

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, bir **AssociationSet** öğesinin alt öğesi olduğunda **End** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Di**  | Evet         | Üst **AssociationSet** öğesinin bir sonunu tanımlayan **EntitySet** öğesinin adı. **EntitySet** öğesi, üst **AssociationSet** öğesiyle aynı varlık kapsayıcısında tanımlanmalıdır. |
| **Rol**       | Hayır          | İlişki kümesi ucunun adı. **Rol** özniteliği kullanılmazsa, ilişki kümesi ucunun adı varlık kümesinin adı olacaktır.                                                                   |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **End** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnek, her biri iki **End** öğesine sahip iki **AssociationSet** öğesiyle bir **EntityContainer** öğesi gösterir:

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

Kavramsal şema tanım dili (CSDL) içindeki **EntityContainer** öğesi, varlık kümeleri, ilişkilendirme kümeleri ve işlev içeri aktarmaları için bir mantıksal kapsayıcıdır. Kavramsal model varlık kapsayıcısı, EntityContainerMapping öğesi aracılığıyla bir depolama modeli varlık kapsayıcısına eşlenir. Bir depolama modeli varlık kapsayıcısı veritabanının yapısını açıklar: varlık kümeleri tabloları tanımlar, ilişki kümeleri yabancı anahtar kısıtlamalarını tanımlar ve işlev içeri aktarmaları bir veritabanında saklı yordamları açıklar.

Bir **EntityContainer** öğesi sıfır veya bir belge öğesine sahip olabilir. Bir **belge** öğesi varsa, tüm **EntitySet**, **AssociationSet**ve **FunctionImport** öğelerinden önce gelmelidir.

Bir **EntityContainer** öğesi aşağıdaki alt öğeleri sıfır veya daha fazla içerebilir (listelenen sırayla):

-   Di
-   AssociationSet
-   'Unun
-   Ek açıklama öğeleri

Aynı ad alanı içinde olan başka bir **EntityContainer** 'ın içeriğini dahil etmek Için bir **EntityContainer** öğesini genişletebilirsiniz. Başka bir **EntityContainer**'ın içeriğini eklemek için, başvuran **EntityContainer** öğesinde, **Extends** özniteliğinin değerini, dahil etmek istediğiniz **EntityContainer** öğesinin adı olarak ayarlayın. Dahil edilen **EntityContainer** öğesinin tüm alt öğeleri, başvuran **EntityContainer** öğesinin alt öğeleri olarak kabul edilir.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **using** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Ad**       | Evet         | Varlık kapsayıcısının adı.                               |
| **Tekrarlan**    | Hayır          | Aynı ad alanı içindeki başka bir varlık kapsayıcısının adı. |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **EntityContainer** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, üç varlık kümesini ve iki ilişkilendirme kümesini tanımlayan bir **EntityContainer** öğesini gösterir.

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
 

 

## <a name="entityset-element-csdl"></a>EntitySet öğesi (CSDL)

Kavramsal şema tanım dilindeki **EntitySet** öğesi bir varlık türü örnekleri ve bu varlık türünden türetilmiş herhangi bir türün örnekleri için mantıksal bir kapsayıcıdır. Bir varlık türü ve bir varlık kümesi arasındaki ilişki, ilişkisel veritabanındaki bir satır ve tablo arasındaki ilişkiye benzer. Bir satır gibi, bir varlık türü ilgili verilerin bir kümesini tanımlar ve bir tablo gibi bir varlık kümesi bu tanımın örneklerini içerir. Bir varlık kümesi, bir veri kaynağındaki ilgili veri yapılarına eşleştiribilecekleri şekilde, varlık türü örneklerinin gruplandırılması için bir yapı sağlar.  

Belirli bir varlık türü için birden fazla varlık kümesi tanımlanmış olabilir.

> [!NOTE]
> EF Designer, tür başına birden çok varlık kümesi içeren kavramsal modelleri desteklemez.

 

**EntitySet** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Documentation öğesi (sıfır veya bir öğe izin verilir)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **EntitySet** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Varlık kümesinin adı.                                                              |
| **EntityType** | Evet         | Varlık kümesinin örnek içerdiği varlık türünün tam adı. |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **EntitySet** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, üç **EntitySet** öğesiyle bir **EntityContainer** öğesi gösterir:

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
 

Tür başına birden çok varlık kümesi tanımlamak mümkündür (MEST). Aşağıdaki örnek, **kitap** varlık türü için iki varlık kümesiyle bir varlık kapsayıcısını tanımlar:

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

**EntityType** öğesi, bir müşteri veya sipariş gibi bir üst düzey kavramın bir kavramsal modelde yapısını temsil eder. Varlık türü, bir uygulamadaki varlık türü örnekleri için bir şablondur. Her şablon aşağıdaki bilgileri içerir:

-   Benzersiz bir ad. (Gerekli.)
-   Bir veya daha fazla özellik tarafından tanımlanan bir varlık anahtarı. (Gerekli.)
-   Veri içeren Özellikler. (İsteğe bağlı.)
-   Bir ilişkinin bir sonundan diğer uçtan gezintiye izin veren gezinti özellikleri. (İsteğe bağlı.)

Bir uygulamada, varlık türünün bir örneği belirli bir nesneyi (örneğin, belirli bir müşteri veya sipariş) temsil eder. Bir varlık türünün her örneğinin bir varlık kümesi içinde benzersiz bir varlık anahtarı olmalıdır.

İki varlık türü örneği, yalnızca aynı türde olmaları durumunda ve varlık anahtarlarının değerleri aynı ise eşit olarak değerlendirilir.

Bir **EntityType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Anahtar (sıfır veya bir öğe)
-   Özellik (sıfır veya daha fazla öğe)
-   NavigationProperty (sıfır veya daha fazla öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **EntityType** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı                                                                                                                                  | Gereklidir | Değer                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Ad**                                                                                                                                        | Evet         | Varlık türünün adı.                                                                     |
| **BaseType**                                                                                                                                    | Hayır          | Tanımlanmakta olan varlık türünün temel türü olan başka bir varlık türünün adı.  |
| **Soyut**                                                                                                                                    | Hayır          | Varlık türünün soyut bir tür olmasına bağlı olarak **true** veya **false**.                 |
| **OpenType**                                                                                                                                    | Hayır          | Varlık türünün açık bir varlık türü olup olmadığına bağlı olarak **doğru** veya **yanlış** . <br/> [!NOTE] |
| **OpenType** özniteliği > yalnızca ADO.NET Data Services ile kullanılan kavramsal modellerde tanımlanan varlık türleri için geçerlidir. |             |                                                                                                  |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **EntityType** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, üç **özellik** öğesi ve iki **NavigationProperty** öğesi olan bir **EntityType** öğesi gösterir:

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

**EnumType** öğesi, numaralandırılmış bir türü temsil eder.

Bir **EnumType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Üye (sıfır veya daha fazla öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **EnumType** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı     | Gereklidir | Değer                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**           | Evet         | Varlık türünün adı.                                                                                                                                                                  |
| **IsFlags**        | Hayır          | Sabit listesi türünün bir bayrak kümesi olarak kullanılıp kullanılamayacağını bağlı olarak **true** veya **false**. Varsayılan değer false şeklindedir **.**                                                                     |
| **UnderlyingType** | Hayır          | **Edm. Byte**, **Edm. Int16**, **Edm. Int32**, **Edm. Int64** veya **Edm. SByte** , türün değer aralığını tanımlar.   Numaralandırma öğelerinin varsayılan temel alınan türü **Edm. Int32**' dir.. |

 

> [!NOTE]
> **EnumType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, üç **üye** öğesi olan bir **EnumType** öğesini göstermektedir:

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **işlev** öğesi, kavramsal modeldeki işlevleri tanımlamak veya bildirmek için kullanılır. Bir işlev DefiningExpression öğesi kullanılarak tanımlanır.  

Bir **Function** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Parametre (sıfır veya daha fazla öğe)
-   DefiningExpression (sıfır veya bir öğe)
-   ReturnType (Işlev) (sıfır veya bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

Bir işlev için dönüş türü, **ReturnType** (Function) öğesi ya da **ReturnType** özniteliğiyle (aşağıya bakın) belirtilmelidir, ancak her ikisine birden değil. Olası dönüş türleri, herhangi bir EdmSimpleType, varlık türü, karmaşık tür, satır türü veya başvuru türüdür (veya bu türlerden birinin bir koleksiyonu).

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **işlev** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                              |
|:---------------|:------------|:-----------------------------------|
| **Ad**       | Evet         | İşlevin adı.          |
| **'Indaki** | Hayır          | İşlev tarafından döndürülen tür. |

 

> [!NOTE]
> **İşlev** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir eğitmenin işe alındığı tarihten itibaren yıl sayısını döndüren bir işlevi tanımlamak için bir **Function** öğesi kullanmaktadır.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>FunctionImport öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **FunctionImport** öğesi, veri kaynağında tanımlanan ancak kavramsal model aracılığıyla nesneler için kullanılabilen bir işlevi temsil eder. Örneğin, depolama modelindeki bir Işlev öğesi bir veritabanında saklı yordamı temsil etmek için kullanılabilir. Kavramsal modeldeki bir **FunctionImport** öğesi, bir Entity Framework uygulamasındaki karşılık gelen işlevi temsil eder ve FunctionImportMapping öğesi kullanılarak depolama modeli işlevine eşlenir. İşlev uygulamada çağrıldığında, karşılık gelen saklı yordam veritabanında yürütülür.

**FunctionImport** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe izin verilir)
-   Parametre (sıfır veya daha fazla öğeye izin verilir)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)
-   ReturnType (FunctionImport) (sıfır veya daha fazla öğeye izin verilir)

İşlevin kabul ettiği her parametre için bir **parametre** öğesi tanımlanmalıdır.

Bir işlev için dönüş türü, **ReturnType** (FunctionImport) öğesi ya da **ReturnType** özniteliği (aşağıya bakın) ile belirtilmelidir, ancak her ikisine birden değil. Dönüş türü değeri EdmSimpleType, EntityType veya ComplexType koleksiyonu olmalıdır.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **FunctionImport** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**         | Evet         | İçeri aktarılan işlevin adı.                                                                                                                                                                    |
| **'Indaki**   | Hayır          | İşlevin döndürdüğü tür. İşlev bir değer döndürmezse bu özniteliği kullanmayın. Aksi takdirde, değer ComplexType, EntityType veya EDMSimpleType bir koleksiyon olmalıdır.        |
| **Di**    | Hayır          | İşlev bir varlık türleri koleksiyonu döndürürse, **EntitySet** 'in değeri koleksiyonun ait olduğu varlık kümesi olmalıdır. Aksi takdirde, **EntitySet** özniteliği kullanılmamalıdır. |
| **IsComposable** | Hayır          | Değer true olarak ayarlanırsa, işlev birleştirilebilir (tablo değerli Işlev) ve bir LINQ sorgusunda kullanılabilir.  Varsayılan değer **false**'dur.                                                           |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **FunctionImport** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir parametre kabul eden ve bir varlık türleri koleksiyonu döndüren bir **FunctionImport** öğesi gösterir:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Anahtar öğesi (CSDL)

**Anahtar** öğesi EntityType öğesinin bir alt öğesidir ve bir *varlık anahtarı* (bir özellik veya kimliği belirleyen bir varlık türünün özellikler kümesi) tanımlar. Bir varlık anahtarını oluşturan Özellikler tasarım zamanında seçilir. Varlık anahtarı özelliklerinin değerleri, çalışma zamanında bir varlık kümesi içindeki bir varlık türü örneğini benzersiz şekilde tanımlamalıdır. Bir varlık kümesindeki örneklerin benzersizlik garantisi sağlamak için bir varlık anahtarı oluşturan Özellikler seçilmelidir. **Anahtar** öğesi bir varlık türünün bir veya daha fazla özelliğe başvurarak bir varlık anahtarını tanımlar.

**Anahtar** öğesi aşağıdaki alt öğelere sahip olabilir:

-   PropertyRef (bir veya daha fazla öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

**Anahtar** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, **Book**adlı bir varlık türünü tanımlar. Varlık anahtarı, varlık türünün **ISBN** özelliğine başvurarak tanımlanır.

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
 

Uluslararası bir standart defter numarası (ıSBN) bir kitabı benzersiz bir şekilde tanımladığından, **ISBN** özelliği varlık anahtarı için iyi bir seçimdir.

Aşağıdaki örnek, iki özellik, **ad** ve **adresten**oluşan bir varlık anahtarı olan bir varlık türü (**Yazar**) gösterir.

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
 

Aynı ada sahip iki yazar aynı adreste yaşılamadığından, varlık anahtarı için **ad** ve **Adres** kullanımı makul bir seçimdir. Ancak, bir varlık anahtarı için bu seçenek bir varlık kümesindeki benzersiz varlık anahtarlarını kesinlikle garanti etmez. Bir yazarı benzersiz bir şekilde tanımlamak için kullanılabilecek **AuthorId**gibi bir özelliğin eklenmesi bu durumda önerilir.

 

## <a name="member-element-csdl"></a>Üye öğesi (CSDL)

**Üye** öğesi EnumType öğesinin bir alt öğesidir ve numaralandırılmış türün bir üyesini tanımlar.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **FunctionImport** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Üyenin adı.                                                                                                                                                                  |
| **Değer**      | Hayır          | Üyenin değeri. Varsayılan olarak, ilk üye 0 değerine sahiptir ve art arda her bir Numaralandırıcı değeri 1 artırılır. Aynı değere sahip birden çok üye bulunabilir. |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **FunctionImport** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, üç **üye** öğesi olan bir **EnumType** öğesini göstermektedir:

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>NavigationProperty öğesi (CSDL)

Bir **NavigationProperty** öğesi, bir ilişkilendirmenin diğer sonuna bir başvuru sağlayan bir gezinti özelliği tanımlar. Özellik öğesiyle tanımlanan özelliklerden farklı olarak, gezinti özellikleri verilerin şeklini ve özelliklerini tanımlamaz. İki varlık türü arasındaki bir ilişkilendirmede gezinmek için bir yol sağlar.

Gezinti özelliklerinin, bir ilişkinin sonundaki her iki varlık türü üzerinde isteğe bağlı olduğunu unutmayın. Bir ilişkinin sonundaki bir varlık türünde bir gezinti özelliği tanımlarsanız, ilişkilendirmenin diğer sonundaki varlık türünde bir gezinti özelliği tanımlamanız gerekmez.

Bir gezinti özelliği tarafından döndürülen veri türü, uzak ilişki ucunun çoğulluğu tarafından belirlenir. Örneğin **, bir** **Müşteri** varlık türünde bir gezinti özelliği olduğunu varsayalım ve **Müşteri** ile **sipariş**arasında bire çok ilişkilendirmeyi gider. Gezinti özelliği için uzak ilişki ucunun çokluğun çok sayıda (\*) olduğundan, veri türü bir koleksiyon ( **sıra**) olur. Benzer şekilde, bir gezinti özelliği olan **CustomerNavProp**, **sipariş** varlık türünde mevcutsa, uzak uçtaki çoğulluğu bir (1) olduğundan veri türü **Müşteri** olur.

Bir **NavigationProperty** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **NavigationProperty** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**         | Evet         | Navigation özelliğinin adı.                                                                                                                                                                                                             |
| **İlişkiye** | Evet         | Modelin kapsamı içinde olan bir ilişkilendirmenin adı.                                                                                                                                                                                |
| **ToRole**       | Evet         | Gezinmede uçların bittiği son. **ToRole** özniteliğinin değeri, ilişki uçlarından birinde tanımlanan **rol** özniteliklerinden birinin değeriyle aynı olmalıdır (associationend öğesinde tanımlanır).       |
| **FromRole**     | Evet         | Gezintinin başladığı ilişkinin sonu. **FromRole** özniteliğinin değeri, ilişki uçlarından birinde tanımlanan **rol** özniteliklerinden birinin değeriyle aynı olmalıdır (associationend öğesinde tanımlanır). |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **NavigationProperty** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, iki gezinti özelliği (**PublishedBy** ve **WrittenBy**) ile bir varlık türü (**Book**) tanımlar:

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

Kavramsal şema tanım dili (CSDL) içindeki **OnDelete** öğesi bir ilişkilendirme ile bağlantılı davranışı tanımlar. **Eylem** özniteliği bir ilişkinin bir ucunda **Cascade** olarak ayarlandıysa, ilk uçtaki varlık türü silindiğinde, ilişkilendirmenin diğer ucundaki ilgili varlık türleri silinir. İki varlık türü arasındaki ilişki birincil anahtardan birincil anahtar ilişkisi ise, ilişkilendirmenin diğer ucundaki asıl nesne **OnDelete** belirtimine bakılmaksızın silindiğinde, yüklenmiş bir bağımlı nesne silinir.  

> [!NOTE]
> **OnDelete** öğesi yalnızca bir uygulamanın çalışma zamanı davranışını etkiler; veri kaynağındaki davranışı etkilemez. Veri kaynağında tanımlanan davranış, uygulamada tanımlanan davranışla aynı olmalıdır.

 

**OnDelete** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **OnDelete** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Eylem**     | Evet         | **Cascade** veya **none**. Eğer **basamakla**, bağımlı varlık türleri asıl varlık türü silindiğinde silinir. **Hiçbiri**yoksa, asıl varlık türü silindiğinde bağımlı varlık türleri silinmez. |

 

> [!NOTE]
> **İlişkilendirme** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, **CustomerOrders** ilişkilendirmesini tanımlayan bir **ilişki** öğesini gösterir. **OnDelete** öğesi, **Müşteri** silindiğinde, belirli bir **müşteriyle** Ilgili ve ObjectContext 'e yüklenmiş tüm **siparişlerin** silineceğini belirtir.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **Parameter** öğesi, FunctionImport öğesinin veya Function öğesinin bir alt öğesi olabilir.

### <a name="functionimport-element-application"></a>FunctionImport öğe uygulaması

Bir **parametre** öğesi ( **FunctionImport** öğesinin bir alt Öğesı olarak), csdl içinde belirtilen işlev içeri aktarmaları için giriş ve çıkış parametrelerini tanımlamak üzere kullanılır.

**Parameter** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe izin verilir)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **parametre** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Parametrenin adı.                                                                                                                                                                                                      |
| **Tür**       | Evet         | Parametre türü. Değer, bir **Edmsimpletype** veya modelin kapsamı içinde olan karmaşık bir tür olmalıdır.                                                                                                             |
| **Modundaysa**       | Hayır          | Parametresinin bir giriş, çıkış veya giriş/çıkış parametresi olup olmadığına bağlı olarak, **içinde**, **Out**veya **InOut** .                                                                                                                |
| **'In**  | Hayır          | Parametrenin izin verilen en fazla uzunluğu.                                                                                                                                                                                    |
| **Duyarlılık**  | Hayır          | Parametrenin duyarlığı.                                                                                                                                                                                                 |
| **Ölçek**      | Hayır          | Parametresinin ölçeği.                                                                                                                                                                                                     |
| **SRıD**       | Hayır          | Uzamsal sistem başvuru tanımlayıcısı. Yalnızca uzamsal türlerin parametreleri için geçerlidir. Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> **Parametre** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnek, bir bir **parametre** alt öğesi olan bir **FunctionImport** öğesi gösterir. İşlev bir giriş parametresini kabul eder ve varlık türlerinin bir koleksiyonunu döndürür.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>İşlev öğesi uygulaması

Bir **parametre** öğesi ( **Function** öğesinin bir alt öğesi olarak) kavramsal modelde tanımlanan veya belirtilen işlevlere yönelik parametreleri tanımlar.

**Parameter** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   CollectionType (sıfır veya bir öğe)
-   ReferenceType (sıfır veya bir öğe)
-   RowType (sıfır veya bir öğe)

> [!NOTE]
> **CollectionType**, **ReferenceType**veya **RowType** öğelerinden yalnızca biri bir **özellik** öğesinin alt öğesi olabilir.

 

-   Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)

> [!NOTE]
> Ek açıklama öğeleri diğer tüm alt öğelerden sonra gelmelidir. Ek açıklama öğelerine yalnızca CSDL v2 ve sonrasında izin verilir.

 

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **parametre** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**         | Evet         | Parametrenin adı.                                                                                                                                                                                                      |
| **Tür**         | Hayır          | Parametre türü. Bir parametre aşağıdaki türlerden herhangi biri olabilir (veya bu türlerin koleksiyonları): <br/> **EdmSimpleType** <br/> entity type <br/> complex type <br/> satır türü <br/> başvuru türü                             |
| **Yapılamaz**     | Hayır          | **True** (varsayılan değer) veya özelliğin **null** değere sahip olmasına bağlı olarak **false** .                                                                                                                          |
| **Değerinin** | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                              |
| **'In**    | Hayır          | Özellik değerinin uzunluk üst sınırı.                                                                                                                                                                                       |
| **FixedLength**  | Hayır          | Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                          |
| **Duyarlılık**    | Hayır          | Özellik değerinin duyarlığı.                                                                                                                                                                                            |
| **Ölçek**        | Hayır          | Özellik değerinin ölçeği.                                                                                                                                                                                                |
| **SRıD**         | Hayır          | Uzamsal sistem başvuru tanımlayıcısı. Yalnızca uzamsal türlerin özellikleri için geçerlidir. Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Hayır          | Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                               |
| **Mediğinden**    | Hayır          | Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.                                                                                                                                                   |

 

> [!NOTE]
> **Parametre** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnek, bir işlev parametresi tanımlamak için bir **parametre** alt öğesi kullanan bir **Function** öğesini gösterir.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Principal öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **asıl** öğe, bir başvuru kısıtlamasının asıl bitimi tanımlayan ReferentialConstraint öğesinin bir alt öğesidir. Bir **ReferentialConstraint** öğesi, ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamasına benzer işlevselliği tanımlar. Bir veritabanı tablosundaki bir sütunun (veya sütunlarının) başka bir tablonun birincil anahtarına başvurmasına benzer şekilde, bir varlık türünün özelliği (veya özellikleri) başka bir varlık türünün varlık anahtarına başvurabilir. Başvurulan varlık türüne kısıtlamanın *asıl sonu* denir. Asıl ucuna başvuran varlık türüne kısıtlamanın *bağımlı sonu* denir. **Propertyref** öğeleri, bağımlı uç tarafından hangi anahtarlara başvurulduğunu belirtmek için kullanılır.

**Principal** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   PropertyRef (bir veya daha fazla öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Principal** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rol**       | Evet         | İlişkinin asıl ucundaki varlık türünün adı. |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **Principal** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, **PublishedBy** Association tanımının bir parçası olan **ReferentialConstraint** öğesini gösterir. **Yayımcı** varlık türünün **ID** özelliği, başvuru kısıtlamasının asıl sonunu oluşturur.

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
 

 

## <a name="property-element-csdl"></a>Property öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **özellik** öğesi EntityType öğesi, complexType öğesi ya da RowType öğesinin bir alt öğesi olabilir.

### <a name="entitytype-and-complextype-element-applications"></a>EntityType ve ComplexType öğesi uygulamaları

**Özellik** öğeleri ( **EntityType** veya **complexType** öğelerinin alt öğeleri olarak) bir varlık türü örneği veya karmaşık tür örneğinin içereceği verilerin şeklini ve özelliklerini tanımlar. Kavramsal modeldeki özellikler, bir sınıfta tanımlanan özelliklerle benzerdir. Bir sınıftaki özellikler, sınıfın şeklini tanımlar ve nesneler hakkında bilgi taşıyan bir kavramsal modeldeki Özellikler bir varlık türünün şeklini tanımlar ve varlık türü örnekleri hakkında bilgi taşır.

**Property** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Documentation öğesi (sıfır veya bir öğe izin verilir)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)

Aşağıdaki modeller bir **özellik** öğesine uygulanabilir: **null atanabilir**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **UNICODE**, **harmanlama**, **ConcurrencyMode**. Modeller, özellik değerlerinin veri deposunda nasıl depolandığı hakkında bilgi sağlayan XML öznitelikleridir.

> [!NOTE]
> Modeller yalnızca **Edmsimpletype**türünde özelliklere uygulanabilir.

 

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **özellik** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı                                                         | Gereklidir | Değer                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                                                               | Evet         | Özelliğin adı.                                                                                                                                                                                                       |
| **Tür**                                                               | Evet         | Özellik değerinin türü. Özellik değeri türünün, modelin kapsamındaki bir **Edmsimpletype** veya karmaşık bir tür olması gerekir (tam olarak nitelenmiş bir ad ile belirtilir).                                                 |
| **Yapılamaz**                                                           | Hayır          | **True** (varsayılan değer) veya özelliğin NULL değere sahip olmasına bağlı olarak <strong>false</strong> . <br/> [!NOTE]                                                                                                   |
| CSDL v1 'de > karmaşık bir tür özelliği `Nullable="False"`sahip olmalıdır. |             |                                                                                                                                                                                                                                 |
| **Değerinin**                                                       | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                              |
| **'In**                                                          | Hayır          | Özellik değerinin uzunluk üst sınırı.                                                                                                                                                                                       |
| **FixedLength**                                                        | Hayır          | Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                          |
| **Duyarlılık**                                                          | Hayır          | Özellik değerinin duyarlığı.                                                                                                                                                                                            |
| **Ölçek**                                                              | Hayır          | Özellik değerinin ölçeği.                                                                                                                                                                                                |
| **SRıD**                                                               | Hayır          | Uzamsal sistem başvuru tanımlayıcısı. Yalnızca uzamsal türlerin özellikleri için geçerlidir. Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Hayır          | Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                               |
| **Mediğinden**                                                          | Hayır          | Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | Hayır          | **Hiçbiri** (varsayılan değer) veya **sabit**. Değer **fixed**olarak ayarlandıysa, iyimser eşzamanlılık denetimlerinde Özellik değeri kullanılacaktır.                                                                                  |

 

> [!NOTE]
> **Özellik** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnek, üç **özellik** öğesi olan bir **EntityType** öğesi gösterir:

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
 

Aşağıdaki örnek, beş **özellik** öğesi Içeren bir **complexType** öğesi gösterir:

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>RowType öğe uygulaması

**Özellik** öğeleri ( **RowType** öğesinin alt öğeleri olarak), model tanımlı bir işlevden geçirilecek veya döndürülen verilerin şeklini ve özelliklerini tanımlar.  

**Property** öğesi aşağıdaki alt öğelerden tam olarak birine sahip olabilir:

-   Türünde
-   referenceType
-   RowType

**Property** öğesi herhangi bir sayıda alt ek açıklama öğesine sahip olabilir.

> [!NOTE]
> Ek açıklama öğelerine yalnızca CSDL v2 ve sonrasında izin verilir.

 

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **özellik** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı                                                     | Gereklidir | Değer                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                                                           | Evet         | Özelliğin adı.                                                                                                                                                                                                       |
| **Tür**                                                           | Evet         | Özellik değerinin türü.                                                                                                                                                                                                 |
| **Yapılamaz**                                                       | Hayır          | **True** (varsayılan değer) veya özelliğin NULL değere sahip olmasına bağlı olarak **false** . <br/> [!NOTE]                                                                                                                |
| CSDL v1 'de > karmaşık bir tür özelliği `Nullable="False"`sahip olmalıdır. |             |                                                                                                                                                                                                                                 |
| **Değerinin**                                                   | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                              |
| **'In**                                                      | Hayır          | Özellik değerinin uzunluk üst sınırı.                                                                                                                                                                                       |
| **FixedLength**                                                    | Hayır          | Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                          |
| **Duyarlılık**                                                      | Hayır          | Özellik değerinin duyarlığı.                                                                                                                                                                                            |
| **Ölçek**                                                          | Hayır          | Özellik değerinin ölçeği.                                                                                                                                                                                                |
| **SRıD**                                                           | Hayır          | Uzamsal sistem başvuru tanımlayıcısı. Yalnızca uzamsal türlerin özellikleri için geçerlidir. Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Hayır          | Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                               |
| **Mediğinden**                                                      | Hayır          | Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.                                                                                                                                                   |

 

> [!NOTE]
> **Özellik** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

#### <a name="example"></a>Örnek

Aşağıdaki örnek, model tanımlı bir işlevin dönüş türünün şeklini tanımlamak için kullanılan **özellik** öğelerini gösterir.

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

Kavramsal şema tanım dili (CSDL) içindeki **Propertyref** öğesi, özelliğin aşağıdaki rollerden birini gerçekleştireceğini göstermek için bir varlık türünün özelliğine başvurur:

-   Varlığın anahtarının bir parçası (bir özellik veya kimliği tespit eden bir varlık türünün özellikler kümesi). Bir veya daha fazla **Propertyref** öğesi bir varlık anahtarı tanımlamak için kullanılabilir.
-   Bir başvuru kısıtlamasının bağımlı veya birincil sonu.

**Propertyref** öğesi yalnızca ek açıklama öğelerine (sıfır veya daha fazla) alt öğe olarak sahip olabilir.

> [!NOTE]
> Ek açıklama öğelerine yalnızca CSDL v2 ve sonrasında izin verilir.

 

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Propertyref** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                |
|:---------------|:------------|:-------------------------------------|
| **Ad**       | Evet         | Başvurulan özelliğin adı. |

 

> [!NOTE]
> **Propertyref** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnekte bir varlık türü (**Book**) tanımlanmaktadır. Varlık anahtarı, varlık türünün **ISBN** özelliğine başvurarak tanımlanır.

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
 

Sonraki örnekte, iki Özellik (**ID** ve **publisherID**) bir başvuru kısıtlamasının asıl ve bağımlı uçları olduğunu göstermek için iki **propertyref** öğesi kullanılır.

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

Kavramsal şema tanım dili (CSDL) içindeki **ReferenceType** öğesi bir varlık türüne yönelik bir başvuru belirtir. **ReferenceType** öğesi aşağıdaki öğelerin bir alt öğesi olabilir:

-   ReturnType (Işlev)
-   Parametre
-   Türünde

Bir işlev için bir parametre veya dönüş türü tanımlarken, **ReferenceType** öğesi kullanılır.

Bir **ReferenceType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **ReferenceType** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                         |
|:---------------|:------------|:----------------------------------------------|
| **Tür**       | Evet         | Başvurulmakta olan varlık türünün adı. |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **ReferenceType** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir **kişi** varlık türü başvurusunu kabul eden model tanımlı bir Işlevde bir **parametre** öğesinin alt öğesi olarak kullanılan **ReferenceType** öğesini göstermektedir:

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
 

Aşağıdaki örnek, bir **kişi** varlık türüne başvuru döndüren model tanımlı bir Işlevde bir **ReturnType** (Function) öğesinin alt öğesi olarak kullanılan **ReferenceType** öğesini göstermektedir:

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
 

 

## <a name="referentialconstraint-element-csdl"></a>ReferentialConstraint öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki bir **ReferentialConstraint** öğesi, ilişkisel bir veritabanındaki başvurusal bütünlük kısıtlamasına benzer işlevselliği tanımlar. Bir veritabanı tablosundaki bir sütunun (veya sütunlarının) başka bir tablonun birincil anahtarına başvurmasına benzer şekilde, bir varlık türünün özelliği (veya özellikleri) başka bir varlık türünün varlık anahtarına başvurabilir. Başvurulan varlık türüne kısıtlamanın *asıl sonu* denir. Asıl ucuna başvuran varlık türüne kısıtlamanın *bağımlı sonu* denir.

Bir varlık türünde sunulan bir yabancı anahtar, başka bir varlık türündeki bir özelliğe başvuruyorsa, **ReferentialConstraint** öğesi iki varlık türü arasında bir ilişki tanımlar. **ReferentialConstraint** öğesi iki varlık türünün birbiriyle nasıl ilişkili olduğu hakkında bilgi sağladığından, eşleme belirtim DILINDE (MSL) karşılık gelen bir associationsetmapping öğesi gerekli değildir. Yabancı anahtarları açığa çıkarmayan iki varlık türü arasındaki ilişki, ilişki bilgilerini veri kaynağıyla eşlemek için karşılık gelen bir **Associationsetmapping** öğesine sahip olmalıdır.

Bir varlık türünde yabancı anahtar gösterilmediği takdirde, **ReferentialConstraint** öğesi varlık türü ve başka bir varlık türü arasında yalnızca birincil anahtar-birincil anahtar kısıtlaması tanımlayabilir.

Bir **ReferentialConstraint** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Asıl (tam olarak bir öğe)
-   Bağımlı (tam olarak bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

**ReferentialConstraint** öğesinde herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) bulunabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, **PublishedBy** Association tanımının bir parçası olarak kullanılan **ReferentialConstraint** öğesini gösterir.

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
 

 

## <a name="returntype-function-element-csdl"></a>ReturnType (Işlev) öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **ReturnType** (işlev) öğesi, bir işlev öğesinde tanımlanan bir işlev için dönüş türünü belirtir. Bir işlev dönüş türü, bir **ReturnType** özniteliğiyle de belirtilebilir.

Dönüş türleri herhangi bir **Edmsimpletype**, varlık türü, karmaşık tür, satır türü, başvuru türü veya bu türlerden birinin bir koleksiyonu olabilir.

Bir işlevin dönüş türü, **ReturnType** (Function) öğesinin **tür** özniteliğiyle ya da aşağıdaki alt öğelerinden biri ile belirtilebilir:

-   Türünde
-   referenceType
-   RowType

> [!NOTE]
> Yalnızca **ReturnType** (Function) öğesinin **Type** özniteliği ve alt öğelerinden biri ile bir işlev dönüş türü belirtirseniz bir model doğrulanmaz.

 

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **ReturnType** (Function) öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                              |
|:---------------|:------------|:-----------------------------------|
| **'Indaki** | Hayır          | İşlev tarafından döndürülen tür. |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **ReturnType** (Function) öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir kitabın yazdırıldığınız yıl sayısını döndüren bir işlev tanımlamak için bir **işlev** öğesi kullanır. Dönüş türünün bir **ReturnType** (Function) öğesinin **Type** özniteliğiyle belirtildiğine unutmayın.

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (FunctionImport) öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **ReturnType** (FunctionImport) öğesi, bir FunctionImport öğesinde tanımlanan bir işlevin dönüş türünü belirtir. Bir işlev dönüş türü, bir **ReturnType** özniteliğiyle de belirtilebilir.

Dönüş türleri herhangi bir varlık türü, karmaşık tür veya **Edmsimpletype**koleksiyonu olabilir,

İşlevin dönüş türü, **ReturnType** (FunctionImport) öğesinin **tür** özniteliğiyle belirtilir.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **ReturnType** (FunctionImport) öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tür**       | Hayır          | İşlevin döndürdüğü tür. Değer, ComplexType, EntityType veya EDMSimpleType bir koleksiyon olmalıdır.                                                                                      |
| **Di**  | Hayır          | İşlev bir varlık türleri koleksiyonu döndürürse, **EntitySet** 'in değeri koleksiyonun ait olduğu varlık kümesi olmalıdır. Aksi takdirde, **EntitySet** özniteliği kullanılmamalıdır. |

 

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **ReturnType** (FunctionImport) öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, kitap ve yayımcılar döndüren bir **FunctionImport** kullanır. İşlevin iki sonuç kümesi döndürdüğüne ve bu nedenle iki **ReturnType** (FunctionImport) öğesi belirtildiğine unutmayın.

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>RowType öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **RowType** öğesi, kavramsal modelde tanımlanan bir işlev için parametre veya dönüş türü olarak adlandırılmamış bir yapı tanımlar.

**RowType** öğesi aşağıdaki öğelerin alt öğesi olabilir:

-   Türünde
-   Parametre
-   ReturnType (Işlev)

**RowType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Özellik (bir veya daha fazla)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

**RowType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, işlevin satır koleksiyonunu ( **RowType** öğesinde belirtilen şekilde) döndürdüğünü belirtmek Için bir **CollectionType** öğesi kullanan model tanımlı bir işlevi gösterir.

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

**Şema** öğesi, bir kavramsal model tanımının kök öğesidir. Kavramsal modeli oluşturan nesneler, işlevler ve kapsayıcılar için tanımlar içerir.

**Şema** öğesi aşağıdaki alt öğeleri sıfır veya daha fazla içerebilir:

-   Kullanarak
-   'Indaki
-   entityType
-   EnumType
-   Kaldırma
-   Türündedir
-   İşlev

**Şema** öğesi sıfır veya bir ek açıklama öğesi içerebilir.

> [!NOTE]
> **Function** öğesi ve Annotation ÖĞELERINE yalnızca csdl v2 ve üzeri sürümlerde izin verilir.

 

**Şema** öğesi, kavramsal bir modeldeki varlık türü, karmaşık tür ve ilişkilendirme nesneleri için ad alanını tanımlamak üzere **Namespace** özniteliğini kullanır. Bir ad alanı içinde, iki nesne aynı ada sahip olamaz. Ad alanları, birden çok **şema** öğesine ve birden çok. csdl dosyasına yayılabilir.

Kavramsal model ad alanı, **şema** öğesinin XML ad alanından farklıdır. Kavramsal model ad alanı ( **ad alanı** özniteliği tarafından tanımlandığı gibi) varlık türleri, karmaşık türler ve ilişkilendirme türleri için bir mantıksal kapsayıcıdır. Bir **şema** öğesinin XML ad alanı ( **xmlns** özniteliğiyle gösterilir), alt öğeler ve **şema** öğesinin öznitelikleri için varsayılan ad alanıdır. Form https://schemas.microsoft.com/ado/YYYY/MM/edm XML ad alanları (YYYY ve MM, sırasıyla bir yılı ve ayı temsil eder) CSDL için ayrılmıştır. Özel öğeler ve öznitelikler bu forma sahip ad alanlarında olamaz.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, özniteliklerin **şema** öğesine uygulanabileceğini açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Evet         | Kavramsal modelin ad alanı. **Ad alanı** özniteliğinin değeri, bir türün tam nitelikli adını biçimlendirmek için kullanılır. Örneğin, *Müşteri* adlı bir **EntityType** basit. example. model ad alanında ise, **EntityType** 'ın tam adı simpleexamplemodel. Customer olur. <br/> Şu dizeler **ad alanı** özniteliği değeri olarak kullanılamaz: **System**, **geçici**veya **EDM**. **Namespace** özniteliğinin DEĞERI, SSDL şema öğesindeki **Namespace** özniteliğinin değeri ile aynı olamaz. |
| **Diğer ad**      | Hayır          | Ad alanı adı yerine kullanılan tanımlayıcı. Örneğin, *Müşteri* adlı bir **EntityType** basit. example. model ad alanı ve **diğer ad** özniteliğinin değeri *Modelise*model. Customer ' i EntityType 'ın tam adı olarak kullanabilirsiniz **.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> **Şema** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir **EntityContainer** öğesi, iki **EntityType** öğesi ve bir **ilişkilendirme** öğesi içeren bir **şema** öğesi gösterir.

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
 

 

## <a name="typeref-element-csdl"></a>TypeRef öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **TypeRef** öğesi, var olan bir adlandırılmış türe başvuru sağlar. **TypeRef** öğesi, bir işlevin bir parametre ya da dönüş türü olarak bir koleksiyon olduğunu belirtmek Için kullanılan CollectionType öğesinin bir alt öğesi olabilir.

**TypeRef** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **TypeRef** öğesine uygulanabilen öznitelikler açıklanmaktadır. **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **UNICODE**ve **harmanlama** özniteliklerinin yalnızca **edmsimpletypes**için geçerli olduğunu unutmayın.

| Öznitelik adı                                                     | Gereklidir | Değer                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tür**                                                           | Hayır          | Başvurulmakta olan türün adı.                                                                                                                                                                                          |
| **Yapılamaz**                                                       | Hayır          | **True** (varsayılan değer) veya özelliğin NULL değere sahip olmasına bağlı olarak **false** . <br/> [!NOTE]                                                                                                                |
| CSDL v1 'de > karmaşık bir tür özelliği `Nullable="False"`sahip olmalıdır. |             |                                                                                                                                                                                                                                 |
| **Değerinin**                                                   | Hayır          | Özelliğin varsayılan değeri.                                                                                                                                                                                              |
| **'In**                                                      | Hayır          | Özellik değerinin uzunluk üst sınırı.                                                                                                                                                                                       |
| **FixedLength**                                                    | Hayır          | Özellik değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                          |
| **Duyarlılık**                                                      | Hayır          | Özellik değerinin duyarlığı.                                                                                                                                                                                            |
| **Ölçek**                                                          | Hayır          | Özellik değerinin ölçeği.                                                                                                                                                                                                |
| **SRıD**                                                           | Hayır          | Uzamsal sistem başvuru tanımlayıcısı. Yalnızca uzamsal türlerin özellikleri için geçerlidir. Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Hayır          | Özellik değerinin bir Unicode dize olarak saklanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                                               |
| **Mediğinden**                                                      | Hayır          | Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.                                                                                                                                                   |

 

> [!NOTE]
> **CollectionType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, işlevin bir **Departman** varlık türleri koleksiyonunu kabul ettiğini belirtmek için **TypeRef** öğesini (bir **CollectionType** öğesinin alt öğesi olarak) kullanan model tanımlı bir işlevi gösterir.

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
 

 

## <a name="using-element-csdl"></a>Using öğesi (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki **using** öğesi, farklı bir ad alanında bulunan bir kavramsal modelin içeriğini içeri aktarır. **Ad alanı** özniteliğinin değerini ayarlayarak, başka bir kavramsal modelde tanımlanan varlık türlerine, karmaşık türlere ve ilişkilendirme türlerine başvurabilirsiniz. Birden fazla **using** öğesi bir **şema** öğesinin alt öğesi olabilir.

> [!NOTE]
> CSDL içindeki **using** öğesi, programlama dilinde bir **using** ifadesiyle tam olarak çalışmaz. Bir programlama dilinde **using** ifadesiyle bir ad alanı içe aktararak, özgün ad alanındaki nesneleri etkilemeyin. CSDL 'de, içeri aktarılan bir ad alanı özgün ad alanındaki bir varlık türünden türetilmiş bir varlık türü içerebilir. Bu, özgün ad alanında belirtilen varlık kümelerini etkileyebilir.

 

**Using** öğesi aşağıdaki alt öğelere sahip olabilir:

-   Belgeler (sıfır veya bir öğe izin verilir)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe izin verilir)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **using** ögesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Evet         | İçeri aktarılan ad alanının adı.                                                                                                                                                |
| **Diğer ad**      | Evet         | Ad alanı adı yerine kullanılan tanımlayıcı. Bu öznitelik gerekli olsa da, nesne adlarını nitelemek için ad alanı adı yerine kullanılması gerekli değildir. |

 

> [!NOTE]
> **Using** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

 

### <a name="example"></a>Örnek

Aşağıdaki örnek, başka bir yerde tanımlanmış bir ad alanını içeri aktarmak için kullanılan **using** öğesini gösterir. Gösterilen **şema** öğesi için ad alanının `BooksModel`olduğunu unutmayın. `Publisher`**EntityType** 'daki `Address` özelliği `ExtendedBooksModel` ad alanında tanımlanan karmaşık bir türdür ( **using** öğesiyle içeri aktarılır).

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
 

 

## <a name="annotation-attributes-csdl"></a>Ek açıklama öznitelikleri (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki ek açıklama öznitelikleri, kavramsal modelde özel XML öznitelikleridir. Geçerli XML yapısına ek olarak, aşağıdaki ek açıklama özniteliklerinin doğru olması gerekir:

-   Ek açıklama öznitelikleri, CSDL için ayrılan XML ad alanı içinde olmamalıdır.
-   Verilen bir CSDL öğesine birden fazla ek açıklama özniteliği uygulanabilir.
-   İki ek açıklama özniteliğinin tam nitelikli adları aynı olmamalıdır.

Ek açıklama öznitelikleri, kavramsal bir modeldeki öğeler hakkında ek meta veriler sağlamak için kullanılabilir. Ek açıklama öğelerinde içerilen meta verilere, System. Data. Metadata. Edm ad alanındaki sınıflar kullanılarak çalışma zamanında erişilebilir.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir ek açıklama özniteliği (**CustomAttribute**) Ile bir **EntityType** öğesi gösterir. Örnek ayrıca varlık türü öğesine uygulanan bir ek açıklama öğesi gösterir.

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
 

Aşağıdaki kod, ek açıklama özniteliğinde meta verileri alır ve konsola yazar:

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
 

Yukarıdaki kod `School.csdl` dosyasının projenin çıkış dizininde olduğunu ve projenize aşağıdaki `Imports` ve `Using` deyimlerini eklediğinizi varsayar:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Ek açıklama öğeleri (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki ek açıklama öğeleri, kavramsal modeldeki özel XML öğeleridir. Geçerli XML yapısına ek olarak, aşağıdaki ek açıklama öğelerinin doğru olması gerekir:

-   Ek açıklama öğeleri, CSDL için ayrılan hiçbir XML ad alanı içinde olmamalıdır.
-   Birden fazla ek açıklama öğesi, verili bir CSDL öğesinin alt öğesi olabilir.
-   İki ek açıklama öğesinin tam nitelikli adları aynı olmamalıdır.
-   Ek açıklama öğeleri, belirli bir CSDL öğesinin tüm diğer alt öğelerinden sonra görünmelidir.

Ek açıklama öğeleri, kavramsal bir modeldeki öğeler hakkında ek meta veriler sağlamak için kullanılabilir. .NET Framework sürüm 4 ' te başlayarak, ek açıklama öğelerinde içerilen meta verilere System. Data. Metadata. Edm ad alanındaki sınıflar kullanılarak çalışma zamanında erişilebilir.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir ek açıklama öğesi (**CustomElement**) Ile bir **EntityType** öğesi gösterir. Örnek ayrıca varlık türü öğesine uygulanan bir ek açıklama özniteliği gösterir.

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
 

Aşağıdaki kod, ek açıklama öğesindeki meta verileri alır ve konsola yazar:

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
 

Yukarıdaki kod, okul. csdl dosyasının projenin çıkış dizininde olduğunu ve projenize aşağıdaki `Imports` ve `Using` deyimlerini eklediğinizi varsayar:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Kavramsal model türleri (CSDL)

Kavramsal şema tanım dili (CSDL), kavramsal bir modeldeki özellikleri tanımlayan **Edmsimpletypes**adlı bir soyut temel veri türleri kümesini destekler. **Edmsimpletypes** , depolama veya barındırma ortamında desteklenen temel veri türleri için proxy 'lardır.

Aşağıdaki tabloda, CSDL tarafından desteklenen temel veri türleri listelenmiştir. Tablo, her **Edmsimpletype**öğesine uygulanabilen modelleri de listeler.

| EDMSimpleType                    | Açıklama                                                | Uygulanabilir modeller                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **EDM. Binary**                   | İkili verileri içerir.                                      | MaxLength, FixedLength, Nullable, varsayılan                                |
| **EDM. Boolean**                  | **True** veya **false**değerini içerir.                  | Null yapılabilir, varsayılan                                                        |
| **EDM. Byte**                     | İşaretsiz 8 bit tamsayı değeri içerir.                  | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. DateTime**                 | Bir tarih ve saati temsil eder.                                | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. DateTimeOffset**           | GMT cinsinden dakika cinsinden bir tarih ve saat içerir. | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. Decimal**                  | Sabit duyarlığa ve ölçeğe sahip sayısal bir değer içerir.   | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. Double**                   | 15 basamaklı duyarlık içeren bir kayan nokta numarası içerir   | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. float**                    | 7 basamaklı duyarlık içeren bir kayan nokta numarası içerir.   | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. Guid**                     | 16 baytlık benzersiz bir tanımlayıcı içerir.                      | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. Int16**                    | İşaretli 16 bit tamsayı değeri içerir.                    | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. Int32**                    | İmzalı 32 bitlik bir tamsayı değeri içerir.                    | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. Int64**                    | İmzalı 64 bitlik bir tamsayı değeri içerir.                    | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. SByte**                    | İşaretli 8 bit tamsayı değeri içerir.                     | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. String**                   | Karakter verisi içerir.                                   | Unicode, FixedLength, MaxLength, harmanlama, duyarlık, Nullable, varsayılan |
| **EDM. Time**                     | Günün saatini içerir.                                    | Duyarlılık, null yapılabilir, varsayılan                                             |
| **EDM. Coğrafya**                |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. Geographyıpoint**           |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. Geographyılinestring**      |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. GeographyPolygon**         |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. Geographyımultipoint**      |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. Geographyımultilinestring** |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. GeographyMultiPolygon**    |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. Geographrivcollection**      |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. Geometry**                 |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. GeometryPoint**            |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. GeometryLineString**       |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. GeometryPolygon**          |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. GeometryMultiPoint**       |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. GeometryMultiLineString**  |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. GeometryMultiPolygon**     |                                                            | Null yapılabilir, varsayılan, SRID                                                  |
| **EDM. GeometryCollection**       |                                                            | Null yapılabilir, varsayılan, SRID                                                  |

## <a name="facets-csdl"></a>Modeller (CSDL)

Kavramsal şema tanım dili (CSDL) içindeki modeller varlık türlerinin ve karmaşık türlerin özelliklerindeki kısıtlamaları temsil eder. Modeller aşağıdaki CSDL öğelerinde XML öznitelikleri olarak görünür:

-   Özellik
-   Değerini
-   Parametre

Aşağıdaki tabloda, CSDL 'de desteklenen modeller açıklanmaktadır. Tüm modeller isteğe bağlıdır. Aşağıda listelenen bazı modeller kavramsal bir modelden veritabanı oluştururken Entity Framework tarafından kullanılır.

> [!NOTE]
> Kavramsal modeldeki veri türleri hakkında daha fazla bilgi için bkz. kavramsal model türleri (CSDL).

| Kısıtlayan               | Açıklama                                                                                                                                                                                                                                                   | Şunlara uygulanacaktır:                                                                                                                                                                                                                                                                                                                                                                           | Veritabanı oluşturma için kullanılır | Çalışma zamanı tarafından kullanılan |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **Mediğinden**       | Özelliğin değerlerinde karşılaştırma ve sıralama işlemleri gerçekleştirirken kullanılacak harmanlama sırasını (veya sıralama sırasını) belirtir.                                                                                                               | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Evet                              | Hayır                  |
| **ConcurrencyMode** | Özellik değerinin iyimser eşzamanlılık denetimleri için kullanılması gerektiğini belirtir.                                                                                                                                                                    | Tüm **Edmsimpletype** özellikleri                                                                                                                                                                                                                                                                                                                                                     | Hayır                               | Evet                 |
| **Default**         | Örnek oluşturma sırasında hiçbir değer sağlanmadığında özelliğin varsayılan değerini belirtir.                                                                                                                                                                       | Tüm **Edmsimpletype** özellikleri                                                                                                                                                                                                                                                                                                                                                     | Evet                              | Evet                 |
| **FixedLength**     | Özellik değerinin uzunluğunun değişebileceğini belirtir.                                                                                                                                                                                                  | **Edm. Binary**, **Edm. String**                                                                                                                                                                                                                                                                                                                                                       | Evet                              | Hayır                  |
| **'In**       | Özellik değerinin uzunluk üst sınırını belirtir.                                                                                                                                                                                                           | **Edm. Binary**, **Edm. String**                                                                                                                                                                                                                                                                                                                                                       | Evet                              | Hayır                  |
| **Yapılamaz**        | Özelliğin **null** değere sahip olup olmayacağını belirtir.                                                                                                                                                                                                     | Tüm **Edmsimpletype** özellikleri                                                                                                                                                                                                                                                                                                                                                     | Evet                              | Evet                 |
| **Duyarlılık**       | **Decimal**türü özellikler için, bir özellik değerinin sahip olduğu basamak sayısını belirtir. **Time**, **DateTime**ve **DateTimeOffset**türündeki özellikler için, özellik değerinin saniyenin kısmi bölümü için basamak sayısını belirtir. | **Edm. DateTime**, **Edm. DateTimeOffset**, **Edm. Decimal**, **Edm. Time**                                                                                                                                                                                                                                                                                                              | Evet                              | Hayır                  |
| **Ölçek**           | Özellik değeri için ondalık noktanın sağ tarafındaki basamak sayısını belirtir.                                                                                                                                                                      | **EDM. Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Evet                              | Hayır                  |
| **SRıD**            | Uzamsal sistem başvurusu sistem KIMLIĞINI belirtir. Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **EDM. coğrafya, Edm. Geographyıpoint, Edm. Geographyılinestring, Edm. GeographyPolygon, Edm. Geographyımultipoint, Edm. Geographyımultilinestring, Edm. GeographyMultiPolygon, Edm. Geographyıcollection, Edm. Geometry, Edm. GeometryPoint EDM. GeometryLineString, Edm. GeometryPolygon, Edm. GeometryMultiPoint, Edm. GeometryMultiLineString, Edm. GeometryMultiPolygon, Edm. GeometryCollection** | Hayır                               | Evet                 |
| **Unicode**         | Özellik değerinin Unicode olarak depolandığını belirtir.                                                                                                                                                                                                    | **EDM. String**                                                                                                                                                                                                                                                                                                                                                                       | Evet                              | Evet                 |

>[!NOTE]
> Kavramsal bir modelden veritabanı oluştururken, veritabanı oluştur Sihirbazı, bir **özellik** öğesi üzerinde **StoreGeneratedPattern** özniteliğinin değerini şu ad alanında ise tanır: https://schemas.microsoft.com/ado/2009/02/edm/annotation. Öznitelik için desteklenen değerler **Identity** ve **hesaplandı**. **Kimlik** değeri, veritabanında oluşturulan kimlik değeri ile bir veritabanı sütunu oluşturur. **Hesaplanan** değeri, veritabanında hesaplanan bir değere sahip bir sütun oluşturur.

### <a name="example"></a>Örnek

Aşağıdaki örnek bir varlık türünün özelliklerine uygulanan modelleri gösterir:

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
