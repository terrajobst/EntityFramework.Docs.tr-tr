---
title: Sorgu-EF Designer-EF6 tanımlama
author: divega
ms.date: 10/23/2016
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: b1589dc12ccb50754c2e950932a2d82bc4869f6b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418800"
---
# <a name="defining-query---ef-designer"></a>Query-EF tasarımcısını tanımlama
Bu izlenecek yol, EF Designer kullanarak bir modele bir tanımlama sorgusunun ve ilgili varlık türünün nasıl ekleneceğini gösterir. Bir veritabanı görünümü tarafından sağlananlara benzer işlevsellik sağlamak için genellikle tanımlama sorgusu kullanılır, ancak görünüm, veritabanında değil modelde tanımlanır. Tanımlama sorgusu, bir. edmx dosyasının **Definingquery** öğesinde BELIRTILEN bir SQL ifadesini çalıştırmanıza izin verir. Daha fazla bilgi için, [SSDL belirtiminde](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) **definingquery** bölümüne bakın.

Sorgu tanımlama kullanılırken, modelinizde bir varlık türü de tanımlamanız gerekir. Varlık türü, tanımlama sorgusunun açığa çıkarılan verileri yüzey için kullanılır. Bu varlık türünün karşılaştığı verilerin salt okunurdur.

Parametreli sorgular sorgu tanımlayarak yürütülemez. Ancak veriler, saklı yordamlara veri sunan varlık türünün INSERT, Update ve delete işlevleri eşlenerek güncellenebilir. Daha fazla bilgi için bkz. [Saklı yordamlarla ekleme, güncelleştirme ve silme](~/ef6/modeling/designer/stored-procedures/cud.md).

Bu konu başlığı altında, aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilmektedir.

-   Tanımlama sorgusu ekleme
-   Modele bir varlık türü ekleyin
-   Tanımlama sorgusunu varlık türü ile eşleyin

## <a name="prerequisites"></a>Önkoşullar

Bu kılavuzu tamamlamak için şunlara ihtiyacınız olacak:

- Visual Studio 'nun son sürümü.
- [Okul örnek veritabanı](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Projeyi ayarlama

Bu izlenecek yol, Visual Studio 2012 veya daha yeni bir sürümünü kullanıyor.

-   Visual Studio'yu açın.
-   **Dosya** menüsünde, **Yeni**' nin üzerine gelin ve ardından **Proje**' ye tıklayın.
-   Sol bölmede, **Visual C\#** ' ye tıklayın ve ardından **konsol uygulaması** şablonunu seçin.
-   Projenin adı olarak **Definingquerysample** girin ve **Tamam**' a tıklayın.

 

## <a name="create-a-model-based-on-the-school-database"></a>Okul veritabanına dayalı bir model oluşturma

-   Çözüm Gezgini ' de proje adına sağ tıklayın, **Ekle**' nin üzerine gelin ve ardından **Yeni öğe**' ye tıklayın.
-   Sol menüden **verileri** seçin ve ardından şablonlar bölmesinde **ADO.net varlık veri modeli** öğesini seçin.
-   Dosya adı için **Definingquerymodel. edmx** girin ve ardından **Ekle**' ye tıklayın.
-   Model Içeriğini seçin iletişim kutusunda, **veritabanından oluştur**' u seçin ve ardından **İleri**' ye tıklayın.
-   Yeni bağlantı ' ya tıklayın. Bağlantı özellikleri iletişim kutusunda sunucu adını (örneğin, **(LocalDB)\\mssqllocaldb**) girin, kimlik doğrulama yöntemini seçin, veritabanı adı için **okul** yazın ve ardından **Tamam**' a tıklayın.
    Veri bağlantınızı seçin iletişim kutusu, veritabanı bağlantı ayarınız ile güncelleştirilir.
-   Veritabanı nesnelerinizi seçin iletişim kutusunda **tablolar** düğümünü kontrol edin. Bu, tüm tabloları **okul** modeline ekler.
-    **Son**' a tıklayın.
-   Çözüm Gezgini, **Definingquerymodel. edmx** dosyasına sağ tıklayın ve **birlikte aç...** seçeneğini belirleyin.
-   **XML (metin) Düzenleyicisi**seçin.

    ![XML Düzenleyicisi](~/ef6/media/xmleditor.png)

-   Aşağıdaki iletiyle istenirse **Evet** ' e tıklayın:

    ![Uyarı 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Tanımlama sorgusu ekleme

Bu adımda,. edmx dosyasının SSDL bölümüne bir tanımlama sorgusu ve bir varlık türü eklemek için XML düzenleyicisini kullanacağız. 

-   . Edmx dosyasının SSDL bölümüne bir **EntitySet** öğesi ekleyin (satır 5 ile 13 arasında). Aşağıdakileri belirtin:
    -    **EntitySet** öğesinin yalnızca **ad** ve **EntityType** öznitelikleri belirtilir.
    -   Varlık türünün tam adı **EntityType** özniteliğinde kullanılır.
    -   Yürütülecek SQL açıklaması **Definingquery** öğesinde belirtilmiştir.

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

-   **EntityType** öğesini. edmx öğesinin SSDL bölümüne ekleyin. dosyası aşağıda gösterildiği gibi. Şunlara dikkat edin:
    -   **Ad** özniteliğinin değeri, yukarıdaki **EntitySet** öğesinde **EntityType** özniteliğinin değerine karşılık gelir, ancak varlık türünün tam adı **EntityType** özniteliğinde kullanılır.
    -   Özellik adları, **Definingquery** öğesinde (yukarıda) SQL ifadesinin döndürdüğü sütun adlarına karşılık gelir.
    -   Bu örnekte, varlık anahtarı benzersiz bir anahtar değeri sağlamak için üç özelliklerden oluşur.

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
> Daha sonra **güncelleştirme modeli Sihirbazı** iletişim kutusunu çalıştırırsanız, sorgu tanımlama da dahil olmak üzere depolama modelinde yapılan tüm değişikliklerin üzerine yazılır.

 

## <a name="add-an-entity-type-to-the-model"></a>Modele bir varlık türü ekleyin

Bu adımda, EF tasarımcısını kullanarak varlık türünü kavramsal modele ekleyeceğiz.  Aşağıdakilere göz önünde edin:

-   Varlığın **adı** , yukarıdaki **EntitySet** öğesinde **EntityType** özniteliğinin değerine karşılık gelir.
-   Özellik adları, yukarıdaki **Definingquery** öğesinde SQL ifadesinin döndürdüğü sütun adlarına karşılık gelir.
-   Bu örnekte, varlık anahtarı benzersiz bir anahtar değeri sağlamak için üç özelliklerden oluşur.

Modeli EF tasarımcısında açın.

-   DefiningQueryModel. edmx öğesine çift tıklayın.
-   Aşağıdaki iletiyi **Evet** olarak söyleyin:

    ![Uyarı 2](~/ef6/media/warning2.png)

 

Modelinizi düzenlemekte bir tasarım yüzeyi sağlayan Entity Desisgner görüntülenir.

-   Tasarımcı yüzeyine sağ tıklayın ve yeni-&gt;varlık **Ekle** ' yi seçin **...**
-   **Anahtar özelliği**için varlık adı ve **CourseID** Için **gradereport** belirtin.
-   **Gradereport** varlığına sağ tıklayın ve yeni-&gt; **skalar Özellik** **Ekle** ' yi seçin.
-   Özelliğin varsayılan adını **FirstName**olarak değiştirin.
-   Başka bir skaler özellik ekleyin ve ad için **LastName** öğesini belirtin.
-   Başka bir skaler özellik ekleyin ve ad için bir **sınıf** belirtin.
-   **Özellikler** penceresinde, **sınıf** **türü** özelliğini **Decimal**olarak değiştirin.
-   **FirstName** ve **LastName** özelliklerini seçin.
-   **Özellikler** penceresinde **EntityKey** özellik değerini **true**olarak değiştirin.

Sonuç olarak, aşağıdaki öğeler. edmx dosyasının **csdl** bölümüne eklenmiştir.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Tanımlama sorgusunu varlık türü ile eşleyin

Bu adımda, kavramsal ve depolama varlık türlerini eşlemek için eşleme ayrıntıları penceresini kullanacağız.

-   Tasarım yüzeyinde **Gradereport** varlığına sağ tıklayın ve **Tablo eşleme**' yi seçin.  
    **Eşleme ayrıntıları** penceresi görüntülenir.
-   **&lt;tablo Ekle veya görüntüle&gt;** açılan listesini ( **tablo**s altında bulunur **) seçin.**  
    Kavramsal ve depolama **Gradereport** varlık türü arasındaki varsayılan eşlemeler görüntülenir.  
    ![eşleme Details3](~/ef6/media/mappingdetails.png)

Sonuç olarak, **EntitySetMapping** öğesi. edmx dosyasının Mapping bölümüne eklenir. 

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

 

## <a name="call-the-defining-query-in-your-code"></a>Kodunuzda tanımlama sorgusunu çağırma

Artık, **Gradereport** varlık türünü kullanarak tanımlama sorgusunu çalıştırabilirsiniz. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
