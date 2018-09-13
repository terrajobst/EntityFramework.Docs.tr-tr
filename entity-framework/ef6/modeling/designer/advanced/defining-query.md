---
title: Sorgu - EF Designer - EF6 tanımlama
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489485"
---
# <a name="defining-query---ef-designer"></a>Sorgu - EF Designer tanımlama
Bu kılavuzda bir tanımlama ekleneceği gösterilmektedir. sorgu ve karşılık gelen bir varlığın EF Designer kullanarak bir modele yazın. Tanımlayan bir sorgu için bir veritabanı görünümü tarafından sağlanan benzer işlevselliği sağlamak için yaygın olarak kullanılır, ancak görünüm modeli, veritabanı tanımlanır. Tanımlayan bir sorgu içinde belirtilen bir SQL deyimi yürütme sağlar **DefiningQuery** bir .edmx dosyası öğesidir. Daha fazla bilgi için **DefiningQuery** içinde [SSDL belirtimi](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Tanımlama sorguları kullanırken, ayrıca bir varlık türü modelinizde tanımlamak vardır. Varlık türü tanımlayan sorgu tarafından kullanıma sunulan veri yüzey için kullanılır. Bu varlık türü ortaya veri salt okunur olduğunu unutmayın.

Parametreli sorgular sorguları tanımlama olarak yürütülemiyor. Ancak, veri, INSERT, update ve delete işlevleri varlık türünün bu yüzeyleri veriler için saklı yordamlar eşleyerek güncelleştirilebilir. Daha fazla bilgi için [INSERT, Update ve Delete saklı yordamlarla](~/ef6/modeling/designer/stored-procedures/cud.md).

Bu konu aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir.

-   Tanımlayan bir sorgu Ekle
-   Modele bir varlık türü Ekle
-   Varlık türü tanımlayan sorgu eşleme

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio'nun en son sürümü.
- [School örnek veritabanını](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projesi kurun

Bu izlenecek yol, Visual Studio 2012 veya daha yeni kullanıyor.

-   Visual Studio'yu açın.
-   Üzerinde **dosya** menüsünde **yeni**ve ardından **proje**.
-   Sol bölmede **Visual C\#** ve ardından **konsol uygulaması** şablonu.
-   Girin **DefiningQuerySample** tıklayın ve proje adı olarak **Tamam**.

 

## <a name="create-a-model-based-on-the-school-database"></a>School veritabanını temel alan bir Model oluşturma

-   Çözüm Gezgini'nde proje adına sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**.
-   Seçin **veri** seçin ve soldaki menüden **ADO.NET varlık veri modeli** Şablonlar bölmesinde.
-   Girin **DefiningQueryModel.edmx** dosya adı ve ardından **Ekle**.
-   Choose Model Contents iletişim kutusunda **veritabanından Oluştur**ve ardından **sonraki**.
-   Yeni bağlantı tıklayın. Bağlantı Özellikleri iletişim kutusuna sunucu adını girin (örneğin, **(localdb)\\ifadesini mssqllocaldb**) seçin kimlik doğrulama yöntemi, tür **Okul** veritabanı adı ve ardından tıklayın **Tamam**.
    Veri bağlantınızı seçin iletişim kutusunda, veritabanı bağlantı ayarı ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda, denetleyin **tabloları** düğümü. Bu, tüm tablolara ekler **Okul** modeli.
-   **Son**'a tıklayın.
-   Çözüm Gezgini'nde sağ **DefiningQueryModel.edmx** seçin ve dosya **birlikte Aç...** .
-   Seçin **XML (metin) Düzenleyicisi**.

    ![XML Düzenleyicisi](~/ef6/media/xmleditor.png)

-   Tıklayın **Evet** şu ileti ile istenirse:

    ![Uyarı 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Tanımlayan bir sorgu Ekle

Bir tanımlama eklemek için XML Düzenleyicisi'ni kullanacağız Bu adımda sorgulamak ve bir varlık .edmx dosyasını SSDL bölümünü yazın. 

-   Ekleme bir **EntitySet** SSDL bölümüne .edmx dosyasını (Satır 5 13'ya kadar geçerli) öğesi. Aşağıdakileri belirtin:
    -   Yalnızca **adı** ve **EntityType** özniteliklerini **EntitySet** öğe belirtilir.
    -   Varlık türü tam adı kullanılıyor **EntityType** özniteliği.
    -   Çalıştırılacak SQL deyimi belirtilen **DefiningQuery** öğesi.

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   Ekleme **EntityType** .edmx SSDL bölümüne öğesi. Aşağıdaki dosya gösterildiği gibi. Şunlara dikkat edin:
    -   Değerini **adı** öznitelik değerine karşılık gelen **EntityType** özniteliğini **EntitySet** öğesi yukarıdaki rağmen tam adı varlık türü kullanılan **EntityType** özniteliği.
    -   Özellik adlarını SQL deyiminin döndürdüğü sütun adlarına karşılık gelen **DefiningQuery** öğesi (yukarıda).
    -   Bu örnekte, varlık anahtarı benzersiz bir anahtar değeri sağlamak için üç özelliklerinden oluşur.

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> Daha sonra çalıştırdığınızda **güncelleştirme modeli Sihirbazı** iletişim kutusunda, sorguları tanımlama dahil olmak üzere depolama modelinde yapılan değişiklikleri yazılır.

 

## <a name="add-an-entity-type-to-the-model"></a>Modele bir varlık türü Ekle

Bu adımda EF Designer kullanarak kavramsal modele varlık türü ekleyeceğiz.  Şunlara dikkat edin:

-   **Adı** varlığı değerine karşılık gelen **EntityType** özniteliğini **EntitySet** yukarıdaki öğesi.
-   Özellik adlarını SQL deyiminin döndürdüğü sütun adlarına karşılık gelen **DefiningQuery** yukarıdaki öğesi.
-   Bu örnekte, varlık anahtarı benzersiz bir anahtar değeri sağlamak için üç özelliklerinden oluşur.

Model EF Designer ile açın.

-   DefiningQueryModel.edmx çift tıklayın.
-   Söyleyin **Evet** aşağıdaki iletisi:

    ![Uyarı 2](~/ef6/media/warning2.png)

 

Modelinizi düzenleme için bir tasarım yüzeyi sağlar, varlık Tasarımcısı görüntülenir.

-   Tasarımcı yüzeyi ve select sağ **yeni Ekle**-&gt;**varlık...** .
-   Belirtin **GradeReport** varlık adı için ve **CourseID** için **anahtar özellik**.
-   Sağ **GradeReport** varlık ve select **yeni Ekle** - &gt; **skaler özelliği**.
-   Özellik için varsayılan adı değiştirmek **FirstName**.
-   Başka bir skaler bir özellik ekleyin ve belirtin **LastName** adı.
-   Başka bir skaler bir özellik ekleyin ve belirtin **sınıf** adı.
-   İçinde **özellikleri** penceresinde değişiklik **sınıf**'s **türü** özelliğini **ondalık**.
-   Seçin **FirstName** ve **LastName** özellikleri.
-   İçinde **özellikleri** penceresinde değişiklik **EntityKey** özellik değerini **True**.

Sonuç olarak, aşağıdaki öğeleri eklenme **CSDL** .edmx dosyasını bölümü.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Varlık türü tanımlayan sorgu eşleme

Bu adımda, depolama varlık türleri ve eşleşme Ayrıntıları penceresi kavramsal eşlemek için kullanacağız.

-   Sağ **GradeReport** tasarım yüzeyi ve select varlıkta **Tablo eşleme**.  
    **Eşleşme ayrıntıları** penceresi görüntülenir.
-   Seçin **GradeReport** gelen **&lt;bir tablo veya Görünüm Ekle&gt;** açılır liste (altında bulunan **tablo**s).  
    Varsayılan kavramsal arasındaki eşlemeleri ve depolama **GradeReport** varlık türü görüntülenir.  
    ![Details3 eşleme](~/ef6/media/mappingdetails.png)

Sonuç olarak, **EntitySetMapping** öğesi .edmx dosyası için eşleme bölümüne eklenir. 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   Uygulamayı derleyin.

 

## <a name="call-the-defining-query-in-your-code"></a>Kodunuzda tanımlayan sorgu çağırın

Kullanarak artık tanımlayan sorgu yürütebilirsiniz **GradeReport** varlık türü. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
