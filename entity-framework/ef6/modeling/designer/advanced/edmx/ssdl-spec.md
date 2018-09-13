---
title: SSDL belirtimi - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: a8b1f844a34c44d283982a52cef3bf80afd7e679
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490356"
---
# <a name="ssdl-specification"></a>SSDL belirtimi
Store şeması tanım dili (SSDL) açıklayan bir varlık çerçevesi uygulamasına depolama modelinin bir XML tabanlı bir dildir.

Bir Entity Framework uygulamasında, depolama model meta verilerini (SSDL içinde yazılan) .ssdl dosyasından System.Data.Metadata.Edm.StoreItemCollection bir örneğine yüklenir ve yöntemleri kullanılarak erişilebilir System.Data.Metadata.Edm.MetadataWorkspace sınıfı. Entity Framework depolama model meta veri deposu özgü komutlar için kavramsal modeline karşı sorgular çevirmek için kullanır.

Entity Framework Designer (EF Designer), tasarım zamanında bir .edmx dosyası içinde depolama model bilgileri depolar. Derleme sırasında varlık Tasarımcısı Entity Framework tarafından çalışma zamanında gereken .ssdl dosyası oluşturmak için bir .edmx dosyası içinde bilgileri kullanır.

SSDL sürümleri, XML ad alanları tarafından ayrılır.

| SSDL sürümü | XML Namespace                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL v2      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Association öğesinde (SSDL)

Bir **ilişkilendirme** depo şeması tanım dili (SSDL) öğesinde bir yabancı anahtar kısıtlaması temel alınan veritabanında katılan tablo sütunları belirtir. İki gerekli alt End öğesi, ilişkilendirmenin bir ucunda tabloları ve her iki ucunda çoğulluk belirtin. İsteğe bağlı bir Referentialconstraint'teki öğe katılan sütunlar yanı sıra, ilişkilendirmenin birincil ve bağımlı sonunu belirtir. Hayır ise **Referentialconstraint'teki** öğe varsa, AssociationSetMapping öğenin ilişkilendirme için sütun eşleşmelerini belirtmek için kullanılmalıdır.

**İlişkilendirme** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir)
-   Bitiş (tam olarak iki)
-   Referentialconstraint'teki (sıfır veya bir)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **ilişkilendirme** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Ad**       | Evet         | Karşılık gelen yabancı anahtar kısıtlaması temel alınan veritabanında adı. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ilişkilendirme** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** kullanan öğesi bir **Referentialconstraint'teki** katılmak sütunları belirlemek için öğe **FK\_CustomerOrders**  yabancı anahtar kısıtlaması:

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

**AssociationSet** depo şeması tanım dili (SSDL) öğesinde temel alınan veritabanında iki tablo arasında bir yabancı anahtar kısıtlaması temsil eder. Yabancı anahtar kısıtlaması katılan tablo sütunları bir ilişkilendirme öğesinde belirtilir. **İlişkilendirme** karşılık gelen öğe bir verilen **AssociationSet** öğesi belirtilen **ilişkilendirme** özniteliği **AssociationSet**  öğesi.

SSDL ilişki Setleri CSDL ilişki setleri için AssociationSetMapping öğeyi göre eşleştirilir. CSDL ilişkilendirme belirli bir CSDL ilişkilendirmesi için ayarlanmış, ancak karşılık gelen bir Referentialconstraint'teki öğesi tarafından tanımlanan **AssociationSetMapping** öğesi gereklidir. Bu durumda, bir **AssociationSetMapping** öğe varsa, tanımladığı eşlemeleri tarafından geçersiz kılınır **Referentialconstraint'teki** öğesi.

**AssociationSet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir)
-   Bitiş (sıfır veya iki)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **AssociationSet** öğesi.

| Öznitelik adı  | Gereklidir | Değer                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Ad**        | Evet         | Yabancı anahtar kısıtlaması ilişkiyi temsil kümesi adı.                          |
| **İlişkilendirme** | Evet         | Yabancı anahtar kısıtlaması katılan sütunlar tanımlayan ilişkilendirmenin adı. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **AssociationSet** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **AssociationSet** temsil eden öğe `FK_CustomerOrders` temel alınan veritabanında yabancı anahtar kısıtlaması:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>CollectionType öğesi (SSDL)

**CollectionType** depo şeması tanım dili (SSDL) öğesinde, bir işlevin dönüş türü bir koleksiyon olduğunu belirtir. **CollectionType** ReturnType öğesinin bir alt öğesidir. Toplama türünü RowType alt öğesi kullanılarak belirtilir:

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **CollectionType** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, kullanan bir işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğesi.

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

**CommandText** depo şeması tanım dili (SSDL) öğesinde veritabanına yürütülen bir SQL deyimi tanımlamanıza olanak tanıyan Function öğesi alt öğesi olan. **CommandText** öğesi veritabanında bir saklı yordam benzer işlevler eklemenize olanak sağlar, ancak tanımladığınız **CommandText** depolama modelinde öğesi.

**CommandText** öğesi alt öğeleri olamaz. Gövdesi **CommandText** öğe, temel alınan veritabanı için geçerli bir SQL deyimi olmalıdır.

Hiçbir öznitelik geçerli olan **CommandText** öğesi.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **işlevi** bir alt öğe **CommandText** öğesi. Kullanıma sunma **UpdateProductInOrder** işlev ObjectContext üzerinde bir yöntem olarak kavramsal modele içeri aktararak.  

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

**DefiningQuery** depo şeması tanım dili (SSDL) öğesinde doğrudan temel alınan veritabanında bir SQL deyimi yürütme olanak tanır. **DefiningQuery** öğesi, bir veritabanı görünümü gibi yaygın olarak kullanılır, ancak görünüm veritabanı yerine depolama modelinde tanımlanır. İçinde tanımlanan görünüm bir **DefiningQuery** öğesi, bir varlık türü kavramsal modelde EntitySetMapping öğenin üzerinden eşlenebilir. Bu eşlemeler salt okunurdur.  

Bildirimi aşağıdaki SSDL sözdizimini gösterir bir **EntitySet** ardından **DefiningQuery** görünümü almak için kullanılan bir sorgu içeren öğe.

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

Saklı yordamlar, varlık Çerçevesi'nde görünümlerini okuma-yazma senaryolarını etkinleştirmek için kullanabilirsiniz. Veri alma ve saklı yordamlar tarafından işleme değişiklik için temel tablo bir veri kaynağı görünümü ya varlık SQL görünümü kullanabilirsiniz.

Kullanabileceğiniz **DefiningQuery** hedeflenecek Microsoft SQL Server Compact 3.5 öğesi. SQL Server Compact 3.5 saklı yordamları desteklemez ancak benzer işlevselliği olan uygulayabilirsiniz **DefiningQuery** öğesi. Burada da yararlı olabilir başka bir programlama dili ve bu veri kaynağının kullanılan veri türleri arasında bir uyuşmazlık üstesinden gelmek için saklı yordamlar oluşturma yerdir. Yazabileceğiniz küçük bir **DefiningQuery** , belirli bir parametre kümesi alır ve daha sonra farklı bir dizi parametrenin, örneğin, verilerin bir saklı yordam olan bir saklı yordam çağırır.

## <a name="dependent-element-ssdl"></a>Bağımlı öğesi (SSDL)

**Bağımlı** depo şeması tanım dili (SSDL) öğesinde (başvurusal Kısıt olarak da bilinir) bir yabancı anahtar kısıtlamasını bağımlı sonuna tanımlayan Referentialconstraint'teki öğeye bir alt öğedir. **Bağımlı** öğesi, bir birincil anahtar sütunu (veya sütun) başvuran bir tablodaki sütuna (veya sütun) belirtir. **PropertyRef** elementleri hangi sütunların başvurulur. Asıl öğe içinde belirtilen sütun tarafından başvurulan birincil anahtar sütunları belirtir **bağımlı** öğesi.

**Bağımlı** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   PropertyRef (bir veya daha fazla)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **bağımlı** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rol**       | Evet         | Aynı değer olarak **rol** (kullanılıyorsa) karşılık gelen son öğe öznitelik; Aksi takdirde, tablonun adını içeren başvuru sütunu. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **bağımlı** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, kullanan bir ilişkilendirme öğenin gösterir. bir **Referentialconstraint'teki** katılmak sütunları belirlemek için öğe **FK\_CustomerOrders** yabancı anahtar kısıtlama. **Bağımlı** öğesi belirtir **CustomerID** sütununun **sipariş** kısıtlamasının bağımlı ucu olarak tablo.

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

## <a name="documentation-element-ssdl"></a>Belge öğesi (SSDL)

**Belgeleri** depo şeması tanım dili (SSDL) öğesinde, bir üst öğe içinde tanımlanan bir nesneyle ilgili bilgileri sağlamak için kullanılabilir.

**Belgeleri** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   **Özet**: üst öğenin kısa bir açıklaması. (sıfır veya bir öğe)
-   **LongDescription**: üst öğenin kapsamlı bir açıklama. (sıfır veya bir öğe)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **belgeleri** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği **belgeleri** EntityType öğesinin alt öğesi olarak öğesi.

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

## <a name="end-element-ssdl"></a>Bitiş öğesi (SSDL)

**Son** depo şeması tanım dili (SSDL) öğesinde, temel alınan veritabanında tablo ve bir ucunda bulunan bir yabancı anahtar kısıtlaması satır sayısını belirtir. **Son** Association öğesinde veya AssociationSet bir öğenin bir alt öğesi olabilir. Her durumda olası alt öğeler ve uygun öznitelikler farklıdır.

### <a name="end-element-as-a-child-of-the-association-element"></a>Bir ilişkilendirme öğesinin alt öğesi olarak bitiş öğesi

Bir **son** öğesi (alt öğesi olarak **ilişkilendirme** öğe) tablo ve sonunda bir yabancı anahtar kısıtlaması, satır sayısını belirtir **türü** ve **Çoğulluk** sırasıyla öznitelikleri. Bir yabancı anahtar kısıtlaması ucunda SSDL ilişkilendirme bir parçası olarak tanımlanır; tam olarak iki ucu SSDL ilişki olmalıdır.

Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   OnDelete (sıfır veya bir öğe)
-   Ek açıklama öğelerinin (sıfır veya daha fazla öğe)

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **ilişkilendirme** öğesi.

| Öznitelik adı   | Gereklidir | Değer                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Türü**         | Evet         | Yabancı anahtar kısıtlaması sonunda SSDL varlık kümesini tam adı.                                                                                                                                                                                                                                                                                          |
| **Rol**         | Hayır          | Değerini **rol** (kullanılıyorsa) karşılık gelen Referentialconstraint'teki öğe sorumlusu veya bağımlı öğesindeki özniteliği.                                                                                                                                                                                                                                             |
| **Çokluk** | Evet         | **1**, **0..1**, veya **\*** yabancı anahtar kısıtlaması sonunda olabilir satır sayısına bağlı olarak. <br/> **1** tam olarak bir satır var. yabancı anahtar kısıtlaması sonunda gösterir. <br/> **0..1** belirten sıfır veya bir satır yabancı anahtar kısıtlaması sonunda yok. <br/> **\*** sıfır, bir veya daha fazla satır yabancı anahtar kısıtlaması sonunda bulunduğunu gösterir. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

#### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **FK\_CustomerOrders** yabancı anahtar kısıtlaması. **Çoğulluk** her belirtilen değerleri **son** öğe belirtmek, bu sayıda satır **siparişler** tabloda bir satır ile ilişkili olabilir **müşteriler**  tablosu, ancak yalnızca tek bir satırda **müşteriler** tabloda bir satır ile ilişkili olabilir **siparişler** tablo. Ayrıca, **OnDelete** öğesi gösteren tüm satırları **siparişler** belirli bir satır başvurusu tablo **müşteriler** tablo silinecek, satır **müşteriler** tablo silinir.

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a>AssociationSet öğesinin bir alt öğesi olarak bitiş öğesi

**Son** öğesi (alt öğesi olarak **AssociationSet** öğesi) temel alınan veritabanında bir tablo bir ucunda bulunan bir yabancı anahtar kısıtlaması belirtir.

Bir **son** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

#### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **son** öğesi alt öğesi olduğunda bir **AssociationSet** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Evet         | Yabancı anahtar kısıtlaması sonunda SSDL varlık kümesinin adı.                                      |
| **Rol**       | Hayır          | Değerin aşağıdakilerden biri **rol** birinde belirtilen öznitelikler **son** karşılık gelen Association öğesinde öğesidir. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **son** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

#### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityContainer** öğesi ile bir **AssociationSet** iki öğe **son** öğeleri:

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

Bir **EntityContainer** depo şeması tanım dili (SSDL) öğesinde bir Entity Framework uygulamasında temel alınan veri kaynağının yapısını açıklar: SSDL varlık kümeleri (EntitySet öğesinde tanımlanan) temsil eden tablolarında bir veritabanı, bir tablodaki satırlar SSDL varlık türleri (EntityType öğesinde tanımlanan) temsil eder ve ilişki setleri (AssociationSet öğesinde tanımlı) bir veritabanındaki yabancı anahtar kısıtlamaları temsil eder. Depolama modelinin varlık kapsayıcısı bir kavramsal model varlık kapsayıcısı Entitycontainermapping'indeki öğesi üzerinden eşlenir.

Bir **EntityContainer** öğesi sıfır veya bir belge öğeleri olabilir. Varsa bir **belgeleri** öğe varsa, bunu diğer tüm alt öğelerden önce gelmelidir.

Bir **EntityContainer** öğesi (listelenen sırayla) sıfır veya daha fazla şu alt öğelerden biri olabilir:

-   EntitySet
-   AssociationSet
-   Ek açıklama öğeleri

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntityContainer** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Ad**       | Evet         | Varlık kapsayıcısının adı. Bu ad, nokta (..) içeremez. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityContainer** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityContainer** iki varlık kümeleri ve bir ilişki kümesi tanımlayan öğe. Varlık türü ve ilişkisi tür adları kavramsal model ad alanı adı tarafından yetkili olan unutmayın.

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

## <a name="entityset-element-ssdl"></a>Entityset'in öğe (SSDL)

Bir **EntitySet** depo şeması tanım dili (SSDL) öğesinde bir tablo veya Görünüm temel alınan veritabanında temsil eder. SSDL EntityType öğesinin tablo veya Görünüm bir sırayı temsil eder. **EntityType** özniteliği bir **EntitySet** öğesi bir SSDL varlık kümesindeki satırları gösteren belirli SSDL varlık türünü belirtir. CSDL varlık kümesi ve bir SSDL varlık kümesi arasındaki eşleme bir EntitySetMapping öğesinde belirtilir.

**EntitySet** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   DefiningQuery (sıfır veya bir öğe)
-   Ek açıklama öğeleri

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntitySet** öğesi.

> [!NOTE]
> Bazı öznitelikler (burada listelenmeyen) ile nitelenebilir **depolamak** diğer adı. Bu öznitelikler bir modeli güncelleştirme güncelleştirme modeli Sihirbazı tarafından kullanılır.

| Öznitelik adı | Gereklidir | Değer                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Varlık kümesinin adı.                                                              |
| **entityType** | Evet         | Varlık türü için varlık kümesi tam olarak nitelenmiş adını örneklerini içerir. |
| **Şema**     | Hayır          | Veritabanı şeması.                                                                     |
| **Tablo**      | Hayır          | Veritabanı tablosu.                                                                      |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntitySet** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityContainer** sahip iki öğe **EntitySet** öğeleri ve bir **AssociationSet** öğesi:

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

Bir **EntityType** depo şeması tanım dili (SSDL) öğesinde bir tablo veya temel alınan veritabanına görünümünü bir satır temsil eder. Bir EntitySet öğedeki SSDL tablo veya Görünüm satırları oluşabilen temsil eder. **EntityType** özniteliği bir **EntitySet** öğesi bir SSDL varlık kümesindeki satırları gösteren belirli SSDL varlık türünü belirtir. CSDL varlık türü ile bir SSDL varlık türü arasında bir eşleme bir EntityTypeMapping öğesinde belirtilir.

**EntityType** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir öğe)
-   Anahtar (sıfır veya bir öğe)
-   Ek açıklama öğeleri

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **EntityType** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Varlık türü adı. Bu değer genellikle varlık türü bir satırı temsil eden tablosunun adı ile aynıdır. Bu değer, herhangi bir nokta (.) içerebilir. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **EntityType** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityType** sahip iki özellik öğe:

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

**İşlevi** depo şeması tanım dili (SSDL) öğesinde, temel alınan veritabanında var olan bir saklı yordam belirtir.

**İşlevi** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir)
-   Parametre (sıfır veya daha fazla)
-   CommandText (sıfır veya bir)
-   ReturnType (sıfır veya daha fazla)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

Dönüş türü için bir işlev ile birlikte belirtilmelidir **ReturnType** öğesi veya **ReturnType** özniteliği (aşağıya bakın), ancak ikisine birden değil.

Depolama modelde belirtilen saklı yordamlar, bir uygulamanın kavramsal modele içeri aktarılabilir. Daha fazla bilgi için [depolanan yordamlarla sorgulama](~/ef6/modeling/designer/stored-procedures/query.md). **İşlevi** öğe ayrıca depolama modelinde özel işlevleri tanımlamak için kullanılabilir.  

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **işlevi** öğesi.

> [!NOTE]
> Bazı öznitelikler (burada listelenmeyen) ile nitelenebilir **depolamak** diğer adı. Bu öznitelikler bir modeli güncelleştirme güncelleştirme modeli Sihirbazı tarafından kullanılır.

| Öznitelik adı             | Gereklidir | Değer                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                   | Evet         | Saklı yordamın adı.                                                                                                                                                                                  |
| **ReturnType**             | Hayır          | Saklı yordamın dönüş türü.                                                                                                                                                                           |
| **Toplama**              | Hayır          | **True** saklı yordamı bir toplam değer; döndürür, aksi takdirde **False**.                                                                                                                                  |
| **Yerleşik**                | Hayır          | **Doğru** yerleşik bir işlev ise<sup>1</sup> işlev; Aksi takdirde **False**.                                                                                                                                  |
| **StoreFunctionName**      | Hayır          | Saklı yordamın adı.                                                                                                                                                                                  |
| **NiladicFunction**        | Hayır          | **Doğru** işlev bir parametresiz ise<sup>2</sup> işlev; **False** Aksi takdirde.                                                                                                                                   |
| **Iscomposable**           | Hayır          | **Doğru** birleştirilebilir bir işlev ise<sup>3</sup> işlev; **False** Aksi takdirde.                                                                                                                                |
| **ParameterTypeSemantics** | Hayır          | İşlev aşırı yüklemelerinin çözümlemek için kullanılan türü anlamları tanımlayan sabit listesi. Numaralandırma, işlev tanımı başına sağlayıcısı bildirimi içinde tanımlanır. Varsayılan değer **Allowımplicitconversion**. |
| **Şema**                 | Hayır          | Saklı yordam tanımlandığı şemasının adı.                                                                                                                                                   |

<sup>1</sup> veritabanında tanımlı bir işlev yerleşik bir işlevdir. CommandText öğesi (SSDL) depolama modelde tanımlı işlevler hakkında daha fazla bilgi için bkz.

<sup>2</sup> parametresiz işlevi, hiçbir parametre kabul eden ve çağrıldığında, parantez gerektirmeyen bir işlevdir.

<sup>3</sup> iki işlev, bir işlevin çıktısı diğer işlevi için giriş olarak birleştirilebilir.

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **işlevi** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **işlevi** karşılık gelen öğe **UpdateOrderQuantity** saklı yordamı. Saklı yordam, iki parametre kabul eder ve bir değer döndürmez.

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

**Anahtar** depo şeması tanım dili (SSDL) öğesinde temel alınan veritabanında bir tablonun birincil anahtarı temsil eder. **Anahtar** bir tabloda bir satırı temsil eden EntityType öğenin bir alt öğesidir. Birincil anahtarı tanımlanan **anahtarı** öğesi üzerinde tanımlanan bir veya daha fazla özellik öğelerinin başvuru tarafından **EntityType** öğesi.

**Anahtar** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   PropertyRef (bir veya daha fazla)
-   Ek açıklama öğeleri

Hiçbir öznitelik geçerli olan **anahtar** öğesi.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityType** öğesi bir anahtarla bir özelliğine başvuruyor:

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

**OnDelete** depo şeması tanım dili (SSDL) öğesinde bir yabancı anahtar kısıtlaması katıldığı bir satır silindiğinde, veritabanı davranışı yansıtır. Eylem ayarlanırsa **Cascade**, reddederseniz, silinen satır başvurusu satırlar da silinir. Eylem ayarlanırsa **hiçbiri**, sonra da, silinen satır başvurusu satırlar da silinmedi. Bir **OnDelete** bir bitiş öğesi alt öğesi bir öğedir.

Bir **OnDelete** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **OnDelete** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Eylem**     | Evet         | **Art arda** veya **hiçbiri**. (Değer **kısıtlı** geçerlidir, ancak aynı davranışı sahiptir **hiçbiri**.) |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **OnDelete** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** tanımlayan öğe **FK\_CustomerOrders** yabancı anahtar kısıtlaması. **OnDelete** öğesi gösteren tüm satırları **siparişler** belirli bir satır başvurusu tablo **müşteriler** varsa Tablo silinecek satırda**Müşteriler** tablo silinir.

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

## <a name="parameter-element-ssdl"></a>Parametre öğesi (SSDL)

**Parametre** depo şeması tanım dili (SSDL) öğesinde veritabanında bir saklı yordam parametrelerini belirten Function öğesinin alt öğesi olan.

**Parametre** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   Belgeleri (sıfır veya bir)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **parametre** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**       | Evet         | Parametrenin adı.                                                                                                                                                                                                      |
| **Türü**       | Evet         | Parametre türü.                                                                                                                                                                                                             |
| **Modu**       | Hayır          | **İçinde**, **kullanıma**, veya **Inout** parametresi bir giriş, çıkış ve giriş/çıkış parametresi olmasına bağlı olarak.                                                                                                                |
| **maxLength**  | Hayır          | Parametresinin en büyük uzunluğu.                                                                                                                                                                                            |
| **Duyarlık**  | Hayır          | Parametre duyarlılığı.                                                                                                                                                                                                 |
| **Ölçek**      | Hayır          | Parametre ölçeği.                                                                                                                                                                                                     |
| **SRID**       | Hayır          | Sistem uzamsal başvuru tanımlayıcısı. Uzamsal tür parametreleri yalnızca için geçerlidir. Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **parametre** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **işlevi** sahip iki öğe **parametre** giriş parametrelerini belirten öğeleri:

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

## <a name="principal-element-ssdl"></a>Asıl öğe (SSDL)

**Asıl** depo şeması tanım dili (SSDL) öğesinde (başvurusal Kısıt olarak da bilinir) bir yabancı anahtar kısıtlaması birincil ucu tanımlayan Referentialconstraint'teki öğe için bir alt öğedir. **Asıl** başka bir sütuna (veya sütun) Başvurulan tablodaki öğesi belirtir birincil anahtar sütunu (veya sütun). **PropertyRef** elementleri hangi sütunların başvurulur. Bağımlı öğenin belirtilen birincil anahtar sütunlarını başvuran sütunları belirten **asıl** öğesi.

**Asıl** öğesi şu alt öğelerden (listelenen sırayla) olabilir:

-   PropertyRef (bir veya daha fazla)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **asıl** öğesi.

| Öznitelik adı | Gereklidir | Değer                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rol**       | Evet         | Aynı değer olarak **rol** (kullanılıyorsa) karşılık gelen son öğe öznitelik; Aksi takdirde, tablonun adını içeren başvurulan sütun. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **asıl** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnek, kullanan bir ilişkilendirme öğenin gösterir. bir **Referentialconstraint'teki** katılmak sütunları belirlemek için öğe **FK\_CustomerOrders** yabancı anahtar kısıtlama. **Asıl** öğesi belirtir **CustomerID** sütununun **müşteri** kısıtlamasının birincil ucu olarak tablo.

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

## <a name="property-element-ssdl"></a>Özellik öğesi (SSDL)

**Özelliği** depo şeması tanım dili (SSDL) öğesinde temel alınan veritabanında bir tablodaki bir sütunu temsil eder. **Özellik** tablodaki satırları temsil eden EntityType öğelerinin alt öğeleridir. Her **özelliği** üzerinde tanımlanan öğe bir **EntityType** öğesi bir sütunu temsil eder.

A **özelliği** öğenin tüm alt öğeleri olamaz.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **özelliği** öğesi.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ad**                  | Evet         | Karşılık gelen sütunun adı.                                                                                                                                                                                           |
| **Türü**                  | Evet         | Karşılık gelen sütun türü.                                                                                                                                                                                           |
| **Boş değer atanabilir**              | Hayır          | **Doğru** (varsayılan değer) veya **False** karşılık gelen sütun null değere sahip olup olmadığına bağlı olarak.                                                                                                                  |
| **defaultValue**          | Hayır          | Karşılık gelen sütun varsayılan değeri.                                                                                                                                                                                  |
| **maxLength**             | Hayır          | Karşılık gelen sütunun maksimum uzunluğu.                                                                                                                                                                                 |
| **FixedLength**           | Hayır          | **Doğru** veya **False** bağlı olup olmadığını karşılık gelen sütun değeri sabit uzunlukta bir dize olarak depolanır.                                                                                                              |
| **Duyarlık**             | Hayır          | Karşılık gelen sütunun duyarlığını.                                                                                                                                                                                      |
| **Ölçek**                 | Hayır          | Karşılık gelen sütunun ölçek.                                                                                                                                                                                          |
| **Unicode**               | Hayır          | **Doğru** veya **False** bağlı olup olmadığını karşılık gelen sütun değeri bir Unicode dize olarak depolanır.                                                                                                                   |
| **Harmanlama**             | Hayır          | Veri kaynağında kullanılacak harmanlama sırasının belirten bir dize.                                                                                                                                                   |
| **SRID**                  | Hayır          | Sistem uzamsal başvuru tanımlayıcısı. Yalnızca uzamsal tür özellikleri için geçerlidir. Daha fazla bilgi için [SRID](http://en.wikipedia.org/wiki/SRID) ve [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | Hayır          | **Hiçbiri**, **kimlik** (karşılık gelen sütun değeri veritabanında oluşturulan bir kimlik ise), veya **hesaplanan** (karşılık gelen sütun değeri veritabanında hesaplanan varsa). Değil RowType özellikleri için geçerlidir. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **özelliği** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **EntityType** iki alt öğe **özelliği** öğeleri:

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

**PropertyRef** depo şeması tanım dili (SSDL) öğesinde başvuran bir EntityType öğe üzerinde özellik aşağıdaki rollerden biri gerçekleştirecek belirtmek için tanımlanmış bir özellik:

-   Tablonun birincil anahtarı bir parçası olarak, **EntityType** temsil eder. Bir veya daha fazla **PropertyRef** öğeleri, bir birincil anahtar tanımlamak için kullanılabilir. Anahtar öğesi daha fazla bilgi için bkz.
-   Başvuru kısıtlamasını bağımlı veya asıl sonuna olabilir. Referentialconstraint'teki daha fazla bilgi için bkz.

**PropertyRef** öğesi şu alt öğelerden yalnızca olabilir:

-   Belgeleri (sıfır veya bir)
-   Ek açıklama öğeleri

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda uygulanabilir öznitelikleri açıklar **PropertyRef** öğesi.

| Öznitelik adı | Gereklidir | Değer                                |
|:---------------|:------------|:-------------------------------------|
| **Ad**       | Evet         | Başvurulan özelliğin adı. |

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **PropertyRef** öğesi. Ancak, özel öznitelikler CSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **PropertyRef** birincil anahtar tanımlı bir özellik başvurarak tanımlamak için kullanılan öğe bir **EntityType** öğesi.

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

## <a name="referentialconstraint-element-ssdl"></a>Referentialconstraint'teki öğesi (SSDL)

**Referentialconstraint'teki** depo şeması tanım dili (SSDL) öğesinde (bir başvuru bütünlüğü kısıtlaması olarak da bilinir) bir yabancı anahtar kısıtlaması temel alınan veritabanında temsil eder. Kısıtlamasının birincil ve bağımlı uçları sırasıyla asıl ve bağımlı alt öğeler tarafından belirtilir. Birincil ve bağımlı biter katılan sütunlar PropertyRef öğeleri ile başvurulur.

**Referentialconstraint'teki** Association öğesinde bir isteğe bağlı alt öğesi bir öğedir. Varsa bir **Referentialconstraint'teki** öğesi belirtilen yabancı anahtar kısıtlaması eşlemek için kullanılmaz **ilişkilendirme** öğe, öğe kullanılan, bunu yapmak için bir AssociationSetMapping.

**Referentialconstraint'teki** öğesi şu alt öğelerden olabilir:

-   Belgeleri (sıfır veya bir)
-   Asıl (tam olarak bir)
-   Bağımlı (tam olarak bir)
-   Ek açıklama öğelerinin (sıfır veya daha fazla)

### <a name="applicable-attributes"></a>Uygun öznitelikler

Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **Referentialconstraint'teki** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **ilişkilendirme** kullanan öğesi bir **Referentialconstraint'teki** katılmak sütunları belirlemek için öğe **FK\_CustomerOrders**  yabancı anahtar kısıtlaması:

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

**ReturnType** depo şeması tanım dili (SSDL) öğesinde tanımlanan bir işlev için dönüş türü belirten bir **işlevi** öğesi. Bir işlevin dönüş türü de belirtilebilir bir **ReturnType** özniteliği.

Bir işlevin dönüş türü belirtilmiş **türü** özniteliği veya **ReturnType** öğesi.

**ReturnType** öğesi şu alt öğelerden olabilir:

- CollectionType (bir tane)  

> [!NOTE]
> Ek açıklama öznitelikleri (özel XML öznitelikleri) herhangi bir sayıda uygulanabilir **ReturnType** öğesi. Ancak, özel öznitelikler SSDL için ayrılmış herhangi bir XML ad alanı için ait olamaz. İki özel öznitelikleri için tam olarak nitelenmiş adlar aynı olamaz.

### <a name="example"></a>Örnek

Aşağıdaki örnekte bir **işlevi** satırları koleksiyonunu döndürür.

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


## <a name="rowtype-element-ssdl"></a>RowType öğesinin (SSDL)

A **RowType** depo şeması tanım dili (SSDL) öğesinde bir dönüş adlandırılmamış bir yapı tanımlar Mağazası'nda tanımlanan bir işlev türü.

A **RowType** öğesi alt öğesi olan **CollectionType** öğesi:

A **RowType** öğesi şu alt öğelerden olabilir:

- Özellik (bir veya daha fazla)  

### <a name="example"></a>Örnek

Aşağıdaki örnek, kullanır depo işlev gösterir. bir **CollectionType** işlev satır koleksiyonunda döndürdüğünü belirtmek için öğe (belirtilmiş **RowType** öğesi).


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

## <a name="schema-element-ssdl"></a>Şema öğesi (SSDL)

**Şema** depo şeması tanım dili (SSDL) öğesinde bir depolama model tanımı kök öğesidir. Bu nesneleri, işlevleri ve bir depolama modeli olun kapsayıcılar için tanımları içerir.

**Şema** öğesi sıfır veya daha fazla şu alt öğelerden birini içerebilir:

-   İlişkilendirme
-   entityType
-   EntityContainer
-   İşlev

**Şema** öğesini kullanan **Namespace** depolama modelinde varlık türü ve ilişki nesnesi için ad alanını tanımlayan öznitelik. Ad alanı içinde hiçbir iki nesnenin aynı ada sahip olabilir.

Bir depolama model ad alanı XML ad alanından farklıdır **şema** öğesi. Bir depolama model ad alanı (tarafından tanımlandığı gibi **Namespace** özniteliği) varlık türleri ve ilişki türleri için bir mantıksal kapsayıcıdır. XML ad alanı (tarafından belirtilen **xmlns** özniteliği), bir **şema** öğedir alt öğeleri ve öznitelikleri için varsayılan ad alanı **şema** öğesi. XML ad alanları formun http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (burada YYYY ve MM yıl ve ay sırasıyla temsil eder) SSDL için ayrılmıştır. Bu forma sahip ad alanları, özel öğeleri ve öznitelikleri olamaz.

### <a name="applicable-attributes"></a>Uygun öznitelikler

Aşağıdaki tabloda öznitelikleri açıklar uygulanabilir **şema** öğesi.

| Öznitelik adı            | Gereklidir | Değer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**             | Evet         | Depolama modelinin ad alanı. Değerini **Namespace** özniteliği, bir türün tam adı oluşturmak için kullanılır. Örneğin, bir **EntityType** adlı *müşteri* ExampleModel.Store ad alanınıza ve sonra tam adı olduğunu **EntityType** olduğu ExampleModel.Store.Customer. <br/> Aşağıdaki dize değeri olarak kullanılamaz **Namespace** özniteliği: **sistem**, **geçici**, veya **Edm**. Değeri **Namespace** öznitelik değeri olarak aynı olamaz **Namespace** CSDL şema öğesindeki özniteliği. |
| **Diğer ad**                 | Hayır          | Ad alanı adı yerine kullanılan tanımlayıcıdır. Örneğin, bir **EntityType** adlı *müşteri* ExampleModel.Store ad alanı ve değerini **diğer** özniteliği *StorageModel*, tam nitelikli adı olarak StorageModel.Customer kullanabilirsiniz **EntityType.**                                                                                                                                                                                                                                                                                    |
| **Sağlayıcı**              | Evet         | Veri sağlayıcısı.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Evet         | Sağlayıcıya döndürmek için hangi sağlayıcı bildirimi gösteren bir belirteç. Biçim belirteci için tanımlanır. Belirteç için değer sağlayıcı tarafından tanımlanır. SQL Server sağlayıcısı bildirimi belirteçleri hakkında daha fazla bilgi için Entity Framework için SqlClient bakın.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Örnek

Aşağıdaki örnekte gösterildiği bir **şema** öğesini içeren bir **EntityContainer** öğesinde, iki **EntityType** öğeleri ve bir **ilişkilendirme** öğesi.

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

## <a name="annotation-attributes"></a>Ek açıklama öznitelikleri

Ek açıklama özniteliklerinde depo şeması tanım dili (SSDL) depolama modelindeki öğeleri hakkında ek meta verileri sağlayan özel XML öznitelikleri depolama modelinde var. Geçerli XML yapısına sahip olmaya ek olarak, ek açıklama özniteliklerine yapılan aşağıdaki kısıtlamalar uygulanır:

-   Ek açıklama öznitelikleri SSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.
-   Her iki ek açıklama özniteliklerin tam adları aynı olmamalıdır.

Ek açıklama birden fazla öznitelik verilen bir SSDL öğesine uygulanabilir. Ek açıklama öğesinde bulunan meta veriler, çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.

### <a name="example"></a>Örnek

Aşağıdaki örnek, uygulanan bir ek açıklama özniteliği olan bir EntityType öğeyi gösterir. **OrderID** özelliği. Örnek ayrıca, eklenen bir ek açıklama öğesi show **EntityType** öğesi.

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

## <a name="annotation-elements-ssdl"></a>Ek açıklama öğelerinin (SSDL)

Depo şeması tanım dili (SSDL) ek açıklama öğesinde depolama modeli hakkında ek meta verileri sağlayan özel XML öğeleri depolama modelinde var. Geçerli XML yapısına sahip olmaya ek olarak, ek açıklama öğelerinin aşağıdaki kısıtlamalar uygulanır:

-   Ek açıklama öğelerinin SSDL için ayrılmış herhangi bir XML ad alanı içinde olmalıdır.
-   Her iki ek açıklama öğelerinin tam adları aynı olmamalıdır.
-   Diğer tüm alt öğeleri verilen SSDL öğenin sonra ek açıklama öğelerinin görünmesi gerekir.

Birden çok ek açıklama öğesi, belirli bir SSDL öğesinin bir alt öğesi olabilir. .NET Framework sürüm 4 ile başlayarak, ek açıklama öğesinde bulunan meta veriler çalışma zamanında System.Data.Metadata.Edm ad alanındaki sınıfları kullanarak erişilebilir.

### <a name="example"></a>Örnek

Aşağıdaki örnek, bir ek açıklama öğesi olan bir EntityType öğeyi gösterir (**CustomElement**). Örnek ayrıca uygulanan bir ek açıklama özniteliği gösterir **OrderID** özelliği.

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

## <a name="facets-ssdl"></a>Modelleri (SSDL)

Depo şeması tanım dili (SSDL), modelleri özelliği öğelerinde belirtilen sütun türleri kısıtlamaları temsil eder. Modeller üzerinde XML özniteliği uygulandığından **özelliği** öğeleri.

Aşağıdaki tabloda SSDL desteklenen özellikleri açıklanmaktadır:

| modeli           | Açıklama                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Harmanlama**   | Harmanlama dizisi (veya sıralama) yapılırken kullanılacak karşılaştırma gerçekleştirme ve özellik değerleri üzerinde işlem sıralama belirtir.                                                                                                             |
| **FixedLength** | Sütun değeri uzunluğunu değişebilir olup olmadığını belirtir.                                                                                                                                                                                                  |
| **maxLength**   | Sütun değeri en büyük uzunluğunu belirtir.                                                                                                                                                                                                           |
| **Duyarlık**   | Tür özellikleri için **ondalık**, bir özellik değeri olabilir basamak sayısını belirtir. Tür özellikleri için **zaman**, **DateTime**, ve **DateTimeOffset**, sütun değerinin saniye kesirli kısmını için basamak sayısını belirtir. |
| **Ölçek**       | Sütun değeri ondalık noktasının sağındaki basamak sayısını belirtir.                                                                                                                                                                      |
| **Unicode**     | Sütun değeri Unicode mi depolanacağını belirtir.                                                                                                                                                                                                    |
