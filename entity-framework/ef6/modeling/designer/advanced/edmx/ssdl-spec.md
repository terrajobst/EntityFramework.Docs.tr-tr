---
title: SSDL belirtimi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418727"
---
# <a name="ssdl-specification"></a>SSDL Belirtimi
Mağaza şeması tanım dili (SSDL), bir Entity Framework uygulamasının depolama modelini tanımlayan XML tabanlı bir dildir.

Entity Framework uygulamasında, depolama modeli meta verileri bir. ssdl dosyasından (SSDL dilinde yazılmıştır) System. Data. Metadata. Edm. StoreItemCollection örneğine yüklenir ve içindeki yöntemler kullanılarak erişilebilir. System. Data. Metadata. Edm. MetadataWorkspace sınıfı. Entity Framework, sorguları, verileri depolamak için kavramsal modele dönüştürmek üzere depolama modeli meta verilerini kullanır.

Entity Framework Designer (EF Designer), tasarım zamanında depolama modeli bilgilerini bir. edmx dosyasında depolar. Derleme zamanında Entity Desisgner, çalışma zamanında Entity Framework gereken. ssdl dosyasını oluşturmak için bir. edmx dosyasındaki bilgileri kullanır.

SSDL sürümleri, XML ad alanları ile farklılaştırılabilir.

| SSDL sürümü | XML ad alanı                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Association öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki bir **ilişkilendirme** öğesi, temel alınan veritabanındaki bir yabancı anahtar kısıtlamasına katılan tablo sütunlarını belirtir. İki gerekli alt uç öğesi, ilişkinin sonunda tabloları ve her uçta çoğulluğu belirler. İsteğe bağlı bir ReferentialConstraint öğesi, ilişkilendirmenin ve katılan sütunların asıl ve bağımlı uçlarını belirtir. **ReferentialConstraint** öğesi yoksa, ilişkilendirmenin sütun eşlemelerini belirtmek Için bir associationsetmapping öğesi kullanılmalıdır.

**Association** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir)
-   Son (tam olarak iki)
-   ReferentialConstraint (sıfır veya bir)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **ilişkilendirme** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Ad**       | Yes         | Temel alınan veritabanında karşılık gelen yabancı anahtar kısıtlamasının adı. |

> [!NOTE]
> **İlişkilendirme** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasına katılan sütunları belirtmek Için **ReferentialConstraint** öğesi kullanan bir **ilişkilendirme** öğesi gösterir:

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

## <a name="associationset-element-ssdl"></a>AssociationSet öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **AssociationSet** öğesi, temel alınan veritabanındaki iki tablo arasındaki yabancı anahtar kısıtlamasını temsil eder. Yabancı anahtar kısıtlamasına katılan tablo sütunları bir Ilişkilendirme öğesinde belirtilir. Belirli bir **AssociationSet** öğesine karşılık gelen **Association** öğesi **AssociationSet** öğesinin **Association** özniteliğinde belirtildi.

SSDL Association kümeleri, bir AssociationSetMapping öğesi tarafından CSDL ilişkilendirme kümelerine eşlenir. Ancak, belirli bir CSDL ilişki kümesi için CSDL ilişkilendirmesi bir ReferentialConstraint öğesi kullanılarak tanımlanmışsa, karşılık gelen bir **Associationsetmapping** öğesi gerekli değildir. Bu durumda, bir **Associationsetmapping** öğesi varsa, tanımladığı eşlemeler **ReferentialConstraint** öğesi tarafından geçersiz kılınır.

**AssociationSet** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir)
-   End (sıfır veya iki)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **AssociationSet** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı  | Gereklidir | Değer                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Ad**        | Yes         | İlişki kümesinin temsil ettiği yabancı anahtar kısıtlamasının adı.                          |
| **Kaldırma** | Yes         | Yabancı anahtar kısıtlamasına katılan sütunları tanımlayan ilişkilendirmenin adı. |

> [!NOTE]
> **AssociationSet** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, temel veritabanında `FK_CustomerOrders` yabancı anahtar kısıtlamasını temsil eden bir **AssociationSet** öğesi gösterir:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>CollectionType öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **CollectionType** öğesi, bir işlevin dönüş türünün bir koleksiyon olduğunu belirtir. **CollectionType** öğesi ReturnType öğesinin bir alt öğesidir. Koleksiyon türü RowType alt öğesi kullanılarak belirtilir:

> [!NOTE]
> **CollectionType** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, işlevin bir satır koleksiyonu döndüreceğini belirtmek için bir **CollectionType** öğesi kullanan bir işlevi gösterir.

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

## <a name="commandtext-element-ssdl"></a>CommandText öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **CommandText** öğesi, bir işlev öğesinin, veritabanında yürütülen bir SQL ifadesini tanımlamanızı sağlayan bir alt öğesidir. **CommandText** öğesi, veritabanında saklı yordama benzer işlevler eklemenize olanak tanır, ancak **CommandText** öğesini depolama modelinde tanımlarsınız.

**CommandText** öğesi alt öğeleri içeremez. **CommandText** öğesinin gövdesi, temel alınan veritabanı için GEÇERLI bir SQL bildirisi olmalıdır.

**CommandText** öğesi için geçerli bir öznitelik yok.

### <a name="example"></a>Örnek

Aşağıdaki örnek, alt **CommandText** öğesi olan bir **Function** öğesini gösterir. **Updateproductınorder** işlevini, kavramsal modele aktararak ObjectContext üzerinde bir yöntem olarak kullanıma sunun.  

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

## <a name="definingquery-element-ssdl"></a>DefiningQuery öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **Definingquery** öğesi, doğrudan temel VERITABANıNDA bir SQL ifadesini yürütmanızı sağlar. **Definingquery** öğesi yaygın olarak bir veritabanı görünümü gibi kullanılır, ancak görünüm veritabanı yerine depolama modelinde tanımlanır. Bir **Definingquery** öğesinde tanımlanan görünüm, bir EntitySetMapping öğesi aracılığıyla kavramsal modeldeki bir varlık türüne eşlenebilir. Bu eşlemeler salt okunurdur.  

Aşağıdaki SSDL sözdizimi, bir **EntitySet** 'in ardından, görünümü almak için kullanılan bir sorgu Içeren **definingquery** öğesiyle ilgili bildirimi gösterir.

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

Görünümler üzerinde okuma yazma senaryolarını etkinleştirmek için Entity Framework saklı yordamları kullanabilirsiniz. Veri alma ve saklı yordamlara göre değişiklik işleme için temel tablo olarak bir veri kaynağı görünümü veya Entity SQL görünümü kullanabilirsiniz.

Microsoft SQL Server Compact 3,5 ' i hedeflemek için **Definingquery** öğesini kullanabilirsiniz. SQL Server Compact 3,5, saklı yordamları desteklemediğinden, **Definingquery** öğesiyle benzer işlevleri de uygulayabilirsiniz. Yararlı olabilecek başka bir yerde, programlama dilinde kullanılan veri türleri ve veri kaynağının bir uyuşmazlığını aşmak için saklı yordamlar oluşturma işlemi yer alabilir. Belirli bir parametre kümesini alan bir **Definingquery** yazabilir ve ardından farklı bir parametre kümesiyle (örneğin, verileri silen bir saklı yordam) çağrı yapabilirsiniz.

## <a name="dependent-element-ssdl"></a>Bağımlı öğe (SSDL)

Depo şeması tanım dili (SSDL) içindeki **bağımlı** öğe, bir yabancı anahtar kısıtlamasının bağımlı sonunu tanımlayan ReferentialConstraint öğesinin bir alt öğesidir (başvuru kısıtlaması olarak da bilinir). **Bağımlı** öğe, birincil anahtar sütununa (veya sütunlara) başvuran bir tablodaki sütunu (veya sütunları) belirtir. **Propertyref** öğeleri hangi sütunlara başvurulduğunu belirtir. Principal öğesi, **bağımlı** öğede belirtilen sütunların başvurduğu birincil anahtar sütunlarını belirtir.

**Bağımlı** öğe aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   PropertyRef (bir veya daha fazla)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda **bağımlı** öğeye uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rol**       | Yes         | Karşılık gelen End öğesinin **rol** özniteliğiyle aynı değer (kullanılıyorsa); Aksi takdirde, başvuran sütununu içeren tablonun adı. |

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **bağımlı** öğeye uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasına katılan sütunları belirtmek Için **ReferentialConstraint** öğesi kullanan bir ilişkilendirme öğesi gösterir. **Bağımlı** öğe, kısıtlamanın bağımlı sonu olarak **Order** tablosunun **CustomerID** sütununu belirtir.

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

## <a name="documentation-element-ssdl"></a>Documentation öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **Belgeler** öğesi, bir üst öğede tanımlanan bir nesne hakkında bilgi sağlamak için kullanılabilir.

**Belge** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   **Özet**: üst öğenin kısa bir açıklaması. (sıfır veya bir öğe)
-   **LongDescription**: üst öğenin kapsamlı bir açıklaması. (sıfır veya bir öğe)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

**Belge** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir EntityType öğesinin alt öğesi olarak **documentation** öğesini göstermektedir.

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

## <a name="end-element-ssdl"></a>End öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **bitiş** öğesi, temel veritabanında yabancı anahtar kısıtlamasının bir sonundaki tablo ve satır sayısını belirtir. **End** öğesi Association öğesinin ya da AssociationSet öğesinin bir alt öğesi olabilir. Her durumda, olası alt öğeler ve ilgili öznitelikler farklıdır.

### <a name="end-element-as-a-child-of-the-association-element"></a>Ilişki öğesinin bir alt öğesi olarak end öğesi

Bir **End** öğesi ( **Association** öğesinin bir alt öğesi olarak), bir yabancı anahtar kısıtlamasının sonunda **tür** ve **çokluk** öznitelikleri sırasıyla tablo ve satır sayısını belirtir. Yabancı anahtar kısıtlamasının uçları bir SSDL ilişkisinin parçası olarak tanımlanır; SSDL Association 'ın tam olarak iki ucu olmalıdır.

Bir **End** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   OnDelete (sıfır veya bir öğe)
-   Ek açıklama öğeleri (sıfır veya daha fazla öğe)

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, bir **ilişkilendirme** öğesinin alt öğesi olduğunda, **End** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Tür**         | Yes         | Yabancı anahtar kısıtlamasının sonundaki SSDL varlık kümesinin tam adı.                                                                                                                                                                                                                                                                                          |
| **Rol**         | Hayır          | Karşılık gelen ReferentialConstraint öğesinin Principal veya Dependent öğesinde **rol** özniteliğinin değeri (kullanılıyorsa).                                                                                                                                                                                                                                             |
| **Çokluk** | Yes         | **1**, **0.. 1**veya yabancı anahtar kısıtlamasının sonunda olabilecek satır sayısına göre **\*** . <br/> **1** yabancı anahtar kısıtlama ucunda tam olarak bir satır olduğunu gösterir. <br/> **0.. 1** yabancı anahtar kısıtlaması sonunda sıfır veya bir satırın bulunduğunu gösterir. <br/> **\*** , yabancı anahtar kısıtlaması sonunda sıfır, bir veya daha fazla satırın bulunduğunu belirtir. |

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **End** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

#### <a name="example"></a>Örnek

Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasını tanımlayan bir **ilişkilendirme** öğesi gösterir. Her **bitiş** öğesinde belirtilen **çokluk** değerleri, **Orders** tablosundaki birçok satırın **Customers** tablosundaki bir satırla ilişkilendirilemeyeceğini gösterir, ancak **Customers** tablosundaki yalnızca bir satır **Orders** tablosundaki bir satırla ilişkilendirilebilir. Ayrıca, **OnDelete** öğesi, **Customers** tablosundaki satır silindiğinde, **müşteriler** tablosundaki belirli bir satıra başvuran **Orders** tablosundaki tüm satırların silineceğini gösterir.

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Öğeyi AssociationSet öğesinin bir alt öğesi olarak bitir

**End** öğesi ( **AssociationSet** öğesinin bir alt öğesi olarak), temel alınan veritabanında yabancı anahtar kısıtlamasının bir sonundaki tabloyu belirtir.

Bir **End** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

#### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, bir **AssociationSet** öğesinin alt öğesi olduğunda **End** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **Di**  | Yes         | Yabancı anahtar kısıtlamasının sonundaki SSDL varlık kümesinin adı.                                      |
| **Rol**       | Hayır          | Karşılık gelen Ilişkilendirme öğesinin bir **End** öğesinde belirtilen **rol** özniteliklerinden birinin değeri. |

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **End** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

#### <a name="example"></a>Örnek

Aşağıdaki örnek, iki **End** öğesiyle bir **AssociationSet** öğesi olan bir **EntityContainer** öğesi gösterir:

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

## <a name="entitycontainer-element-ssdl"></a>EntityContainer öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki bir **EntityContainer** öğesi, bir Entity Framework uygulamasında temel alınan veri kaynağının yapısını AÇıKLAR: SSDL varlık kümeleri (EntitySet öğelerinde tanımlanan) bir veritabanındaki tabloları temsil eder, SSDL varlık türleri (EntityType öğelerinde tanımlanmıştır) bir tablodaki satırları temsil eder ve ilişki kümeleri (AssociationSet öğelerinde tanımlanmıştır) bir veritabanındaki yabancı anahtar kısıtlamalarını temsil eder. Bir depolama modeli varlık kapsayıcısı, EntityContainerMapping öğesi aracılığıyla bir kavramsal model varlığı kapsayıcısına eşlenir.

Bir **EntityContainer** öğesi sıfır veya bir belge öğesine sahip olabilir. Bir **belge** öğesi mevcutsa, diğer tüm alt öğelerin önüne gelmelidir.

Bir **EntityContainer** öğesi aşağıdaki alt öğeleri sıfır veya daha fazla içerebilir (listelenen sırayla):

-   Di
-   AssociationSet
-   Ek açıklama öğeleri

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **EntityContainer** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Ad**       | Yes         | Varlık kapsayıcısının adı. Bu ad nokta (.) içeremez. |

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **EntityContainer** öğesine uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, iki varlık kümesini ve bir ilişki kümesini tanımlayan bir **EntityContainer** öğesini gösterir. Varlık türü ve ilişkilendirme türü adlarının kavramsal model ad alanı adı tarafından nitelenmiş olduğunu unutmayın.

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

## <a name="entityset-element-ssdl"></a>EntitySet öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki bir **EntitySet** öğesi, temel alınan veritabanında bir tabloyu veya görünümü temsil eder. SSDL içindeki bir EntityType öğesi tablo veya görünümdeki bir satırı temsil eder. Bir **EntitySet** öğesinin **EntityType** özniteliği, bir SSDL varlık kümesindeki satırları temsil eden özel SSDL varlık türünü belirtir. Bir CSDL varlık kümesi ve bir SSDL varlık kümesi arasındaki eşleme bir EntitySetMapping öğesinde belirtildi.

**EntitySet** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   DefiningQuery (sıfır veya bir öğe)
-   Ek açıklama öğeleri

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **EntitySet** öğesine uygulanabilen öznitelikler açıklanmaktadır.

> [!NOTE]
> Bazı öznitelikler (burada listelenmemiş) **Mağaza** diğer adıyla nitelenmeyebilir. Bu öznitelikler bir model güncelleştirilirken model Güncelleştirme Sihirbazı tarafından kullanılır.

| Öznitelik adı | Gereklidir | Değer                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Ad**       | Yes         | Varlık kümesinin adı.                                                              |
| **EntityType** | Yes         | Varlık kümesinin örnek içerdiği varlık türünün tam adı. |
| **Manızı**     | Hayır          | Veritabanı şeması.                                                                     |
| **Tablo**      | Hayır          | Veritabanı tablosu.                                                                      |

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **EntitySet** öğesine uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, iki **EntitySet** öğesine ve bir **AssociationSet** öğesine sahip bir **EntityContainer** öğesi gösterir:

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

## <a name="entitytype-element-ssdl"></a>EntityType öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki bir **EntityType** öğesi, alttaki veritabanının bir tablosundaki veya görünümündeki bir satırı temsil eder. SSDL içindeki bir EntitySet öğesi, satırların oluştuğu tabloyu veya görünümü temsil eder. Bir **EntitySet** öğesinin **EntityType** özniteliği, bir SSDL varlık kümesindeki satırları temsil eden özel SSDL varlık türünü belirtir. Bir SSDL varlık türü ve bir CSDL varlık türü arasındaki eşleme bir EntityTypeMapping öğesinde belirtildi.

**EntityType** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir öğe)
-   Anahtar (sıfır veya bir öğe)
-   Ek açıklama öğeleri

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **EntityType** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**       | Yes         | Varlık türünün adı. Bu değer genellikle varlık türünün bir satırı temsil ettiği tablonun adı ile aynıdır. Bu değer, nokta (.) içeremez. |

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) **EntityType** öğesine uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, iki özelliği olan bir **EntityType** öğesi gösterir:

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

## <a name="function-element-ssdl"></a>Function öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **işlev** öğesi, temel alınan veritabanında bulunan bir saklı yordamı belirtir.

**Function** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir)
-   Parametre (sıfır veya daha fazla)
-   CommandText (sıfır veya bir)
-   ReturnType (sıfır veya daha fazla)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

İşlevin dönüş türü, **ReturnType** öğesi ya da **ReturnType** özniteliğiyle belirtilmelidir, ancak her ikisine birden değil.

Depolama modelinde belirtilen saklı yordamlar, bir uygulamanın kavramsal modeline aktarılabilir. Daha fazla bilgi için bkz. [Saklı yordamlarla sorgulama](~/ef6/modeling/designer/stored-procedures/query.md). **İşlev** öğesi, depolama modelindeki özel işlevleri tanımlamak için de kullanılabilir.  

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **işlev** öğesine uygulanabilen öznitelikler açıklanmaktadır.

> [!NOTE]
> Bazı öznitelikler (burada listelenmemiş) **Mağaza** diğer adıyla nitelenmeyebilir. Bu öznitelikler bir model güncelleştirilirken model Güncelleştirme Sihirbazı tarafından kullanılır.

| Öznitelik adı             | Gereklidir | Değer                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                   | Yes         | Saklı yordamın adı.                                                                                                                                                                                  |
| **'Indaki**             | Hayır          | Saklı yordamın dönüş türü.                                                                                                                                                                           |
| **Birleşik**              | Hayır          | Saklı yordam bir toplama değeri döndürürse **true** ; Aksi halde **yanlış**.                                                                                                                                  |
| **Yerleik**                | Hayır          | İşlev yerleşik bir<sup>1</sup> Işlevse **true** ; Aksi halde **yanlış**.                                                                                                                                  |
| **StoreFunctionName**      | Hayır          | Saklı yordamın adı.                                                                                                                                                                                  |
| **NiladicFunction**        | Hayır          | İşlev bir niladic<sup>2</sup> **işlevliyse doğru** ; Aksi takdirde **false** .                                                                                                                                   |
| **IsComposable**           | Hayır          | İşlev birleştirilebilir<sup>3</sup> Işlevse **doğru** ; Aksi takdirde **false** .                                                                                                                                |
| **Parametertypesemantiği** | Hayır          | İşlev aşırı yüklerini çözümlemek için kullanılan tür semantiğini tanımlayan sabit listesi. Sabit listesi, işlev tanımı başına sağlayıcı bildiriminde tanımlanmıştır. Varsayılan değer **Allowwimplicitconversion**' dir. |
| **Manızı**                 | Hayır          | Saklı yordamın tanımlandığı şemanın adı.                                                                                                                                                   |

<sup>1</sup> yerleşik bir işlev, veritabanında tanımlanan bir işlevdir. Depolama modelinde tanımlanan işlevler hakkında daha fazla bilgi için, bkz. CommandText öğesi (SSDL).

<sup>2</sup> bir nilasel işlevi, parametre içermeyen ve çağrıldığında parantez gerektirmeyen bir işlevdir.

<sup>3</sup> iki işlev, bir işlevin çıktısı diğer işlevin girişi olduğunda birleştirilebilir.

> [!NOTE]
> **İşlev** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek **Updateorderquantity** saklı yordamına karşılık gelen bir **Function** öğesini gösterir. Saklı yordam iki parametreyi kabul eder ve bir değer döndürmez.

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

## <a name="key-element-ssdl"></a>Anahtar öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **anahtar** öğe, temel alınan veritabanındaki bir tablonun birincil anahtarını temsil eder. **Anahtar** , bir, tablodaki bir satırı temsil eden bir EntityType öğesinin alt öğesidir. Birincil anahtar, **EntityType** öğesinde tanımlanmış bir veya daha fazla özellik öğesine başvurarak **anahtar** öğesinde tanımlanmıştır.

**Anahtar** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   PropertyRef (bir veya daha fazla)
-   Ek açıklama öğeleri

**Anahtar** öğesi için geçerli bir öznitelik yok.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir özelliğe başvuran bir anahtar içeren bir **EntityType** öğesi gösterir:

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

## <a name="ondelete-element-ssdl"></a>OnDelete öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **OnDelete** öğesi, bir yabancı anahtar kısıtlamasına katılan bir satır silindiğinde veritabanı davranışını yansıtır. Eylem **Cascade**olarak ayarlandıysa, silinmekte olan bir satıra başvuran satırlar da silinir. Eylem **none**olarak ayarlandıysa, silinmekte olan bir satıra başvuran satırlar da silinmez. **OnDelete** öğesi bir End öğesinin alt öğesidir.

**OnDelete** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **OnDelete** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Eylem**     | Yes         | **Cascade** veya **none**. ( **Kısıtlanmış** değer geçerli ancak **none**ile aynı davranışa sahiptir.) |

> [!NOTE]
> **OnDelete** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasını tanımlayan bir **ilişkilendirme** öğesi gösterir. **OnDelete** öğesi, **Customers** tablosundaki satır silindiğinde, **müşteriler** tablosundaki belirli bir satıra başvuran **Orders** tablosundaki tüm satırların silineceğini gösterir.

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

## <a name="parameter-element-ssdl"></a>Parameter öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **Parameter** öğesi, bir işlev öğesinin bir alt öğesidir ve veritabanında saklı yordam için parametreleri belirtir.

**Parameter** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   Belgeler (sıfır veya bir)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **parametre** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**       | Yes         | Parametrenin adı.                                                                                                                                                                                                      |
| **Tür**       | Yes         | Parametre türü.                                                                                                                                                                                                             |
| **Modundaysa**       | Hayır          | Parametresinin bir giriş, çıkış veya giriş/çıkış parametresi olup olmadığına bağlı olarak, **içinde**, **Out**veya **InOut** .                                                                                                                |
| **'In**  | Hayır          | Parametrenin uzunluk üst sınırı.                                                                                                                                                                                            |
| **Duyarlılık**  | Hayır          | Parametrenin duyarlığı.                                                                                                                                                                                                 |
| **Ölçeklendirme**      | Hayır          | Parametresinin ölçeği.                                                                                                                                                                                                     |
| **SRıD**       | Hayır          | Uzamsal sistem başvuru tanımlayıcısı. Yalnızca uzamsal türlerin parametreleri için geçerlidir. Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> **Parametre** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, giriş parametrelerini belirten iki **parametre** öğesi olan bir **Function** öğesini gösterir:

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

## <a name="principal-element-ssdl"></a>Principal öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **Principal** öğesi, bir yabancı anahtar kısıtlamasının (başvuru kısıtlaması olarak da bilinir) ana bitimi tanımlayan ReferentialConstraint öğesinin bir alt öğesidir. **Principal** öğesi, başka bir sütun (veya sütun) tarafından başvurulan bir tablodaki birincil anahtar sütununu (veya sütunları) belirtir. **Propertyref** öğeleri hangi sütunlara başvurulduğunu belirtir. Dependent öğesi, **Principal** öğesinde belirtilen birincil anahtar sütunlarına başvuruda bulunan sütunları belirtir.

**Principal** öğesi aşağıdaki alt öğelere sahip olabilir (listelenen sırayla):

-   PropertyRef (bir veya daha fazla)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Principal** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rol**       | Yes         | Karşılık gelen End öğesinin **rol** özniteliğiyle aynı değer (kullanılıyorsa); Aksi takdirde, başvurulan sütununu içeren tablonun adı. |

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği), **Principal** öğesine uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasına katılan sütunları belirtmek Için **ReferentialConstraint** öğesi kullanan bir ilişkilendirme öğesi gösterir. **Principal** öğesi, kısıtlamanın asıl sonu olarak **Müşteri** tablosunun **CustomerID** sütununu belirtir.

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

## <a name="property-element-ssdl"></a>Property öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **özellik** öğesi, temel alınan veritabanındaki bir tablodaki sütunu temsil eder. **Özellik** öğeleri, bir tablodaki satırları temsil eden EntityType öğelerinin alt öğeleridir. Bir **EntityType** öğesinde tanımlanan her **özellik** öğesi bir sütunu temsil eder.

Bir **özellik** öğesinin herhangi bir alt öğesi olamaz.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **özellik** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                  | Yes         | Karşılık gelen sütunun adı.                                                                                                                                                                                           |
| **Tür**                  | Yes         | Karşılık gelen sütunun türü.                                                                                                                                                                                           |
| **Yapılamaz**              | Hayır          | **True** (varsayılan değer) veya false değeri, karşılık gelen sütunun null değere sahip olup olmadığına bağlı olarak **yanlış** .                                                                                                                  |
| **Değerinin**          | Hayır          | Karşılık gelen sütunun varsayılan değeri.                                                                                                                                                                                  |
| **'In**             | Hayır          | Karşılık gelen sütunun uzunluk üst sınırı.                                                                                                                                                                                 |
| **FixedLength**           | Hayır          | Karşılık gelen sütun değerinin sabit uzunluklu bir dize olarak depolanıp saklanmayacağı seçeneğe bağlı olarak **doğru** veya **yanlış** .                                                                                                              |
| **Duyarlılık**             | Hayır          | Karşılık gelen sütunun duyarlığı.                                                                                                                                                                                      |
| **Ölçeklendirme**                 | Hayır          | Karşılık gelen sütunun ölçeği.                                                                                                                                                                                          |
| **Unicode**               | Hayır          | Karşılık gelen sütun değerinin bir Unicode dize olarak saklanıp saklanmayacağı **doğru** veya **yanlış** .                                                                                                                   |
| **Mediğinden**             | Hayır          | Veri kaynağında kullanılacak harmanlama sırasını belirten bir dize.                                                                                                                                                   |
| **SRıD**                  | Hayır          | Uzamsal sistem başvuru tanımlayıcısı. Yalnızca uzamsal türlerin özellikleri için geçerlidir. Daha fazla bilgi için bkz. [srid](https://en.wikipedia.org/wiki/SRID) ve [srid (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | Hayır          | **Hiçbiri**, **kimlik** (karşılık gelen sütun değeri veritabanında oluşturulan bir kimlik ise) veya **hesaplanmışsa** (karşılık gelen sütun değeri veritabanında hesaplanmışsa). RowType özellikleri için geçerli değil. |

> [!NOTE]
> **Özellik** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, iki alt **özellik** öğesi olan bir **EntityType** öğesi gösterir:

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

## <a name="propertyref-element-ssdl"></a>PropertyRef öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **Propertyref** öğesi, özelliğin aşağıdaki rollerden birini gerçekleştireceğini göstermek Için bir EntityType öğesinde tanımlanan bir özelliğe başvurur:

-   **EntityType** 'ın temsil ettiği tablonun birincil anahtarının bir parçası olun. Bir veya daha fazla **Propertyref** öğesi, birincil anahtar tanımlamak için kullanılabilir. Daha fazla bilgi için bkz. Key öğesi.
-   Bir başvuru kısıtlamasının bağımlı veya birincil sonu olun. Daha fazla bilgi için bkz. ReferentialConstraint öğesi.

**Propertyref** öğesi yalnızca aşağıdaki alt öğelere sahip olabilir:

-   Belgeler (sıfır veya bir)
-   Ek açıklama öğeleri

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, **Propertyref** öğesine uygulanabilen öznitelikler açıklanmaktadır.

| Öznitelik adı | Gereklidir | Değer                                |
|:---------------|:------------|:-------------------------------------|
| **Ad**       | Yes         | Başvurulan özelliğin adı. |

> [!NOTE]
> **Propertyref** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler CSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir **EntityType** öğesinde tanımlanan bir özelliğe başvurarak birincil anahtar tanımlamak için kullanılan bir **propertyref** öğesini gösterir.

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

## <a name="referentialconstraint-element-ssdl"></a>ReferentialConstraint öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **ReferentialConstraint** öğesi, temel veritabanında bir yabancı anahtar kısıtlamasını (başvurusal bütünlük kısıtlaması olarak da bilinir) temsil eder. Kısıtlamanın asıl ve bağımlı uçları, sırasıyla asıl ve bağımlı alt öğeler tarafından belirtilir. Principal ve Dependent erimine katılan sütunlara PropertyRef öğeleriyle başvurulur.

**ReferentialConstraint** öğesi Association öğesinin isteğe bağlı bir alt öğesidir. **İlişki** öğesinde belirtilen yabancı anahtar kısıtlamasını eşlemek Için bir **ReferentialConstraint** öğesi kullanılmazsa, bunu yapmak için bir associationsetmapping öğesi kullanılmalıdır.

**ReferentialConstraint** öğesi aşağıdaki alt öğelere sahip olabilir:

-   Belgeler (sıfır veya bir)
-   Asıl (tam olarak bir)
-   Bağımlı (tam olarak bir)
-   Ek açıklama öğeleri (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

**ReferentialConstraint** öğesine herhangi bir sayıda ek açıklama özniteliği (özel XML özniteliği) uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, **FK\_CustomerOrders** yabancı anahtar kısıtlamasına katılan sütunları belirtmek Için **ReferentialConstraint** öğesi kullanan bir **ilişkilendirme** öğesi gösterir:

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

## <a name="returntype-element-ssdl"></a>ReturnType öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **ReturnType** öğesi, bir **işlev** öğesinde tanımlanan bir işlev için dönüş türünü belirtir. Bir işlev dönüş türü, bir **ReturnType** özniteliğiyle de belirtilebilir.

İşlevin dönüş türü, **tür** özniteliği veya **ReturnType** öğesiyle belirtilir.

**ReturnType** öğesi aşağıdaki alt öğelere sahip olabilir:

- CollectionType (bir)  

> [!NOTE]
> Herhangi bir sayıda ek açıklama özniteliği (özel XML öznitelikleri) **ReturnType** öğesine uygulanabilir. Ancak, özel öznitelikler SSDL için ayrılan herhangi bir XML ad alanına ait olamaz. İki özel öznitelik için tam nitelikli adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir satır koleksiyonu döndüren bir **işlevi** kullanır.

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


## <a name="rowtype-element-ssdl"></a>RowType öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **RowType** öğesi, depoda tanımlanan bir işlev için dönüş türü olarak adlandırılmamış bir yapı tanımlar.

**RowType** öğesi, **CollectionType** öğesinin alt öğesidir:

**RowType** öğesi aşağıdaki alt öğelere sahip olabilir:

- Özellik (bir veya daha fazla)  

### <a name="example"></a>Örnek

Aşağıdaki örnek, işlevin satır koleksiyonunu ( **RowType** öğesinde belirtilen şekilde) döndürdüğünü belirtmek Için bir **CollectionType** öğesi kullanan bir saklama işlevi gösterir.


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

## <a name="schema-element-ssdl"></a>Schema öğesi (SSDL)

Depo şeması tanım dili (SSDL) içindeki **şema** öğesi, bir depolama modeli tanımının kök öğesidir. Bir depolama modeli oluşturan nesneler, işlevler ve kapsayıcılar için tanımlar içerir.

**Şema** öğesi aşağıdaki alt öğeleri sıfır veya daha fazla içerebilir:

-   Kaldırma
-   entityType
-   'Indaki
-   İşlev

**Şema** öğesi, bir depolama modelindeki varlık türü ve ilişkilendirme nesneleri için ad alanını tanımlamak üzere **Namespace** özniteliğini kullanır. Bir ad alanı içinde, iki nesne aynı ada sahip olamaz.

Depolama modeli ad alanı, **şema** öğesinin XML ad alanından farklıdır. Bir depolama modeli ad alanı ( **ad alanı** özniteliğiyle tanımlandığı gibi) varlık türleri ve ilişkilendirme türleri için bir mantıksal kapsayıcıdır. Bir **şema** öğesinin XML ad alanı ( **xmlns** özniteliğiyle gösterilir), alt öğeler ve **şema** öğesinin öznitelikleri için varsayılan ad alanıdır. Form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl XML ad alanları (YYYY ve MM, sırasıyla bir yılı ve ayı temsil eder) SSDL için ayrılmıştır. Özel öğeler ve öznitelikler bu forma sahip ad alanlarında olamaz.

### <a name="applicable-attributes"></a>Uygulanabilir öznitelikler

Aşağıdaki tabloda, özniteliklerin **şema** öğesine uygulanabileceğini açıklanmaktadır.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Uzayına**             | Yes         | Depolama modelinin ad alanı. **Ad alanı** özniteliğinin değeri, bir türün tam nitelikli adını biçimlendirmek için kullanılır. Örneğin, *Müşteri* adlı bir **EntityType** , ExampleModel. Store ad alanında ise, **EntityType** 'ın tam adı örnek model. Store. Customer olur. <br/> Şu dizeler **ad alanı** özniteliği değeri olarak kullanılamaz: **System**, **geçici**veya **EDM**. **Ad alanı** özniteliği DEĞERI, csdl şeması öğesindeki **Namespace** özniteliğinin değeri ile aynı olamaz. |
| **Ek**                 | Hayır          | Ad alanı adı yerine kullanılan tanımlayıcı. Örneğin, *Müşteri* adlı bir **EntityType** , ExampleModel ' de yer alıyorsa. Depo ad alanı ve **diğer ad** özniteliğinin değeri *Storagemodel*ise storagemodel. Customer öğesini EntityType 'ın tam adı olarak kullanabilirsiniz **.**                                                                                                                                                                                                                                                                                    |
| **Sağlayıcı**              | Yes         | Veri sağlayıcı.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Yes         | Sağlayıcıya döndürülecek sağlayıcı bildirimini gösteren bir belirteç. Belirteç için biçim tanımlanmadı. Belirtecin değerleri sağlayıcı tarafından tanımlanır. SQL Server sağlayıcısı bildirim belirteçleri hakkında daha fazla bilgi için bkz. SqlClient Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir **EntityContainer** öğesi, iki **EntityType** öğesi ve bir **ilişkilendirme** öğesi içeren bir **şema** öğesi gösterir.

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

## <a name="annotation-attributes"></a>Ek açıklama öznitelikleri

Depo şeması tanım dili (SSDL) içindeki ek açıklama öznitelikleri, depolama modelindeki öğeler hakkında ek meta veriler sağlayan depolama modelinde özel XML öznitelikleridir. Geçerli XML yapısına sahip olmanın yanı sıra, ek açıklama öznitelikleri için aşağıdaki kısıtlamalar geçerlidir:

-   Ek açıklama öznitelikleri, SSDL için ayrılan XML ad alanı içinde olmamalıdır.
-   İki ek açıklama özniteliğinin tam nitelikli adları aynı olmamalıdır.

Verilen bir SSDL öğesine birden çok ek açıklama özniteliği uygulanabilir. Ek açıklama öğelerinde içerilen meta verilere, System. Data. Metadata. Edm ad alanındaki sınıflar kullanılarak çalışma zamanında erişilebilir.

### <a name="example"></a>Örnek

Aşağıdaki örnek, **OrderID** özelliğine uygulanan bir Annotation özniteliği olan bir EntityType öğesi gösterir. Örnek ayrıca **EntityType** öğesine eklenen bir ek açıklama öğesi gösterir.

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

## <a name="annotation-elements-ssdl"></a>Ek açıklama öğeleri (SSDL)

Depo şeması tanım dili (SSDL) içindeki ek açıklama öğeleri depolama modeli hakkında ek meta veriler sağlayan depolama modelinde özel XML öğeleridir. Geçerli XML yapısına ek olarak, aşağıdaki kısıtlamalar ek açıklama öğeleri için geçerlidir:

-   Ek açıklama öğeleri, SSDL için ayrılan XML ad alanı içinde olmamalıdır.
-   İki ek açıklama öğesinin tam nitelikli adları aynı olmamalıdır.
-   Ek açıklama öğeleri, belirli bir SSDL öğesinin tüm diğer alt öğelerinden sonra gelmelidir.

Birden fazla ek açıklama öğesi, verili bir SSDL öğesinin alt öğesi olabilir. .NET Framework sürüm 4 ' te başlayarak, ek açıklama öğelerinde içerilen meta verilere System. Data. Metadata. Edm ad alanındaki sınıflar kullanılarak çalışma zamanında erişilebilir.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir Annotation öğesi (**CustomElement**) olan bir EntityType öğesi gösterir. Örnek, **OrderID** özelliğine uygulanan bir ek açıklama özniteliği de gösterir.

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

## <a name="facets-ssdl"></a>Modeller (SSDL)

Mağaza şeması tanım dili (SSDL) içindeki modeller Özellik öğelerinde belirtilen sütun türlerinde kısıtlamaları temsil eder. **Özellikler, özellik** öğelerinde XML öznitelikleri olarak uygulanır.

Aşağıdaki tabloda, SSDL içinde desteklenen modeller açıklanmaktadır:

| Kısıtlayan           | Açıklama                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Mediğinden**   | Özelliğin değerlerinde karşılaştırma ve sıralama işlemleri gerçekleştirirken kullanılacak harmanlama sırasını (veya sıralama sırasını) belirtir.                                                                                                             |
| **FixedLength** | Sütun değerinin uzunluğunun değişebileceğini belirtir.                                                                                                                                                                                                  |
| **'In**   | Sütun değerinin uzunluk üst sınırını belirtir.                                                                                                                                                                                                           |
| **Duyarlılık**   | **Decimal**türü özellikler için, bir özellik değerinin sahip olduğu basamak sayısını belirtir. **Time**, **DateTime**ve **DateTimeOffset**türündeki özellikler için, sütun değeri saniyelik kesirli kısmının basamak sayısını belirtir. |
| **Ölçeklendirme**       | Sütun değeri için ondalık noktanın sağ tarafındaki basamak sayısını belirtir.                                                                                                                                                                      |
| **Unicode**     | Sütun değerinin Unicode olarak depolandığını belirtir.                                                                                                                                                                                                    |
