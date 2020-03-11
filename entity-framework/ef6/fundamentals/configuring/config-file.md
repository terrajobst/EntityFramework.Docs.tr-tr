---
title: Yapılandırma dosyası ayarları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417975"
---
# <a name="configuration-file-settings"></a>Yapılandırma dosyası ayarları
Entity Framework, yapılandırma dosyasından bir dizi ayar belirtilmesini sağlar. Genel EF ' de bir ' yapılandırma üzerinden kural ' prensibi: bu gönderide ele alınan tüm ayarların varsayılan bir davranışı vardır; yalnızca varsayılan değer gereksinimlerinizi karşılamıyorsa ayarı değiştirme konusunda endişelenmeniz gerekir.  

## <a name="a-code-based-alternative"></a>Kod tabanlı alternatif  

Bu ayarların tümü kod kullanılarak da uygulanabilir. EF6 ' den itibaren, koddan yapılandırma uygulamanın merkezi bir yolunu sağlayan [kod tabanlı yapılandırma](code-based.md)ekledik. EF6 ' dan önce yapılandırma yine koddan uygulanabilir, ancak farklı bölgeleri yapılandırmak için çeşitli API 'Ler kullanmanız gerekir. Yapılandırma dosyası seçeneği, kodunuzun güncelleştirilmesi gerekmeden dağıtım sırasında bu ayarların kolayca değiştirilmesine izin verir.

## <a name="the-entity-framework-configuration-section"></a>Entity Framework yapılandırma bölümü  

EF 4.1 ile başlayarak, yapılandırma dosyasının **appSettings** bölümünü kullanarak bir bağlam için veritabanı başlatıcısı ayarlayabilirsiniz. EF 4,3 ' de, yeni ayarları işlemek için özel **EntityFramework** bölümünü kullanıma sunduk. Entity Framework, eski biçimi kullanarak veritabanı başlatıcıları kümesini tanımaya devam eder, ancak mümkün olduğunda yeni biçime geçmeyi öneririz.

EntityFramework NuGet paketini yüklediğinizde **EntityFramework** bölümü projenizin yapılandırma dosyasına otomatik olarak eklenmiştir.  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a>Bağlantı Dizeleri  

[Bu sayfada](~/ef6/fundamentals/configuring/connection-strings.md) , yapılandırma dosyasındaki bağlantı dizeleri de dahil olmak üzere, kullanılacak veritabanını Entity Framework nasıl belirlediği hakkında daha fazla bilgi verilmektedir.  

Bağlantı dizeleri standart **ConnectionString** öğesine gider ve **EntityFramework** bölümünü gerektirmez.  

Code First tabanlı modeller normal ADO.NET bağlantı dizelerini kullanır. Örnek:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

EF Designer tabanlı modeller özel EF bağlantı dizeleri kullanır. Örnek:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient;
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Kod tabanlı yapılandırma türü (EF6 Onsürümleri)  

EF6 ile başlayarak, uygulamanızda [kod tabanlı yapılandırma](code-based.md) için kullanmak üzere, EF Için DBConfiguration belirtebilirsiniz. Çoğu durumda, EF otomatik olarak DbConfiguration 'ı bulacaktır bu ayarı belirtmeniz gerekmez. Yapılandırma dosyanızda DbConfiguration 'ı ne zaman belirtmeniz gerektiği hakkında ayrıntılı bilgi için [kod tabanlı yapılandırmanın](code-based.md) **hareketli DBConfiguration** bölümüne bakın.  

Bir DbConfiguration türü ayarlamak için **Codeconfigurationtype** öğesinde derleme nitelikli tür adını belirtirsiniz.  

> [!NOTE]
> Bütünleştirilmiş kod nitelikli adı ad alanı nitelenmiş addır, ardından virgül ve türün bulunduğu derleme. İsteğe bağlı olarak, derleme sürümünü, kültürü ve ortak anahtar belirtecini de belirtebilirsiniz.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>EF veritabanı sağlayıcıları (EF6 sürümleri)  

EF6 'ten önce, bir veritabanı sağlayıcısının Entity Framework özgü bölümlerinin çekirdek ADO.NET sağlayıcısı 'nın bir parçası olarak eklenmesi gerekiyordu. EF6 ile başlayarak, EF 'e özgü parçalar artık yönetilir ve ayrı olarak kaydedilir.  

Normalde, sağlayıcıları kendiniz kaydetmeniz gerekmez. Bu işlem genellikle sağlayıcı tarafından yüklendiğinde yapılır.  

Sağlayıcılar, **EntityFramework** bölümünün **sağlayıcılar** alt bölümü altına bir **sağlayıcı** öğesi eklenerek kaydedilir. Sağlayıcı girişi için iki gerekli öznitelik vardır:  

- **InvariantName** , bu EF sağlayıcının hedeflediği Core ADO.net sağlayıcısını tanımlar  
- **tür** , EF sağlayıcı uygulamasının derleme nitelikli tür adıdır  

> [!NOTE]
> Bütünleştirilmiş kod nitelikli adı ad alanı nitelenmiş addır, ardından virgül ve türün bulunduğu derleme. İsteğe bağlı olarak, derleme sürümünü, kültürü ve ortak anahtar belirtecini de belirtebilirsiniz.  

Bir örnek olarak, Entity Framework yüklediğinizde varsayılan SQL Server sağlayıcıyı kaydetmek için oluşturulan girdidir.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Yakalayıcılar (EF 6.1 ve sonraki sürümler)  

EF 6.1 ile başlayarak, yapılandırma dosyasına dinleyici kaydedebilirsiniz. Yakalayıcılar, veritabanı sorguları yürütme, bağlantılar açma vb. gibi belirli işlemleri gerçekleştirdiğinde, ek mantık çalıştırmanızı sağlar.  

Dinleyici oluşturma, **EntityFramework** **bölümünün alt bölümünün** altına bir **yakalayıcısı** öğesi eklenerek kaydedilir. Örneğin, aşağıdaki yapılandırma, tüm veritabanı işlemlerini konsola kaydedecek yerleşik **Databasegünlükçü** yakalayıcısını kaydeder.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Veritabanı Işlemlerini bir dosyaya kaydetme (EF 6.1 ve sonraki sürümler)  

Bir sorunu ayıklamanıza yardımcı olması için, mevcut bir uygulamaya günlük eklemek istediğinizde özellikle de yapılandırma dosyası aracılığıyla kayıt yaptırıcılar yararlı olur. **Databasegünlükçü** , dosya adını bir oluşturucu parametresi olarak sağlayarak bir dosyaya günlük kaydını destekler.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Bu, varsayılan olarak, uygulama her başlatıldığında günlük dosyasının üzerine yazılmasına neden olur. Bunun yerine, zaten varsa günlük dosyasına ekleyin, örneğin:  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

**Databasegünlükçü** ve kayıt yaptırıcılar hakkında daha fazla bilgi için, bkz. Web günlüğü gönderi [aşv 6,1: yeniden derleme olmadan günlüğe kaydetmeyi açma](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Code First varsayılan bağlantı fabrikası  

Yapılandırma bölümü, Code First bir bağlam için kullanılacak veritabanını bulmak için kullanması gereken varsayılan bir bağlantı fabrikası belirtmenize olanak tanır. Varsayılan bağlantı fabrikası yalnızca bir bağlam için yapılandırma dosyasına bir bağlantı dizesi eklendiğinde kullanılır.  

EF NuGet paketini yüklediğinizde, hangisinin yüklü olduğuna bağlı olarak SQL Express veya LocalDB 'ye işaret eden bir varsayılan bağlantı fabrikası kayıtlı olmalıdır.  

Bir bağlantı fabrikası ayarlamak için, **Defaultconnectionfactory** öğesinde derleme nitelikli tür adını belirtirsiniz.  

> [!NOTE]
> Bütünleştirilmiş kod nitelikli adı ad alanı nitelenmiş addır, ardından virgül ve türün bulunduğu derleme. İsteğe bağlı olarak, derleme sürümünü, kültürü ve ortak anahtar belirtecini de belirtebilirsiniz.  

Kendi varsayılan bağlantı fabrikanızı ayarlamaya bir örnek aşağıda verilmiştir:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

Yukarıdaki örnek, özel fabrikasının parametresiz bir oluşturucuya sahip olmasını gerektirir. Gerekirse, **Parameters** öğesini kullanarak Oluşturucu parametreleri belirtebilirsiniz.  

Örneğin, Entity Framework dahil olan SqlCeConnectionFactory, oluşturucuya bir sağlayıcı sabit adı sağlamanız gerekir. Sağlayıcı sabit adı, kullanmak istediğiniz SQL Compact sürümünü tanımlar. Aşağıdaki yapılandırma, bağlamların SQL Compact sürüm 4,0 ' i varsayılan olarak kullanmasına neden olur.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Varsayılan bir bağlantı fabrikası ayarlamazsanız, Code First `.\SQLEXPRESS`işaret eden SqlConnectionFactory 'yi kullanır. SqlConnectionFactory ayrıca bağlantı dizesinin parçalarını geçersiz kılmanızı sağlayan bir oluşturucuya sahiptir. `.\SQLEXPRESS` dışında SQL Server bir örnek kullanmak istiyorsanız, bu oluşturucuyu sunucuyu ayarlamak için kullanabilirsiniz.  

Aşağıdaki yapılandırma, Code First bir açık bağlantı dizesi kümesi olmayan bağlamlarda **MyDatabaseServer** 'ı kullanmasına neden olur.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Varsayılan olarak, Oluşturucu bağımsız değişkenlerinin dize türünde olduğu varsayılır. Bunu değiştirmek için Type özniteliğini kullanabilirsiniz.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Veritabanı başlatıcıları  

Veritabanı başlatıcıları bağlam temelinde yapılandırılır. **Bağlam** öğesi kullanılarak yapılandırma dosyasında ayarlanamazlar. Bu öğe, yapılandırılmakta olan bağlamı tanımlamak için derleme nitelikli adını kullanır.  

Varsayılan olarak, Code First bağlamları CreateDatabaseIfNotExists başlatıcısı 'nı kullanacak şekilde yapılandırılır. **Bağlam** öğesinde, veritabanı başlatmasını devre dışı bırakmak için kullanılabilecek bir **disabledatabaseınitialization** özniteliği vardır.  

Örneğin, aşağıdaki yapılandırma MyAssembly. dll içinde tanımlanan blog. BlogContext bağlamı için veritabanını başlatmayı devre dışı bırakır.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Özel bir başlatıcı ayarlamak için **Databaseınitializer** öğesini kullanabilirsiniz.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Oluşturucu parametreleri varsayılan bağlantı fabrikaları ile aynı sözdizimini kullanır.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

Entity Framework bulunan genel veritabanı başlatıcılarının birini yapılandırabilirsiniz. **Tür** özniteliği genel türler için .NET Framework biçimini kullanır.  

Örneğin, Code First Migrations kullanıyorsanız, veritabanını `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` başlatıcısı kullanılarak otomatik olarak geçirilecek şekilde yapılandırabilirsiniz.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
