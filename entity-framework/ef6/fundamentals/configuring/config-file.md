---
title: Yapılandırma dosyası ayarlarının - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
caps.latest.revision: 3
ms.openlocfilehash: 24faed6bdae6cc4b31808dc5bbebddb94128d0d3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912141"
---
# <a name="configuration-file-settings"></a>Yapılandırma dosyası ayarları
Entity Framework çeşitli ayarlar yapılandırma dosyasından belirtilmesine olanak sağlar. Genel 'kuralı yapılandırmanız üzerinde' İlkesi EF izler: Bu yayında tartışılan tüm ayarları varsayılan bir davranışa sahip, yalnızca varsayılan artık gereksinimlerinizi sağladığında ayarını değiştirme hakkında endişe etmeniz gerekir.  

## <a name="a-code-based-alternative"></a>Bir kod tabanlı alternatif  

Tüm bu ayarlar, kod kullanarak da uygulanabilir. İçinde kullanıma sunduk EF6 başlangıç [kod tabanlı yapılandırma](code-based.md), uygulama kodu yapılandırma merkezi bir yolu sağlar. EF6 önce yapılandırma hala koddan uygulanabilir ancak farklı alanları yapılandırmak için çeşitli API'leri kullanmanız gerekir. Dağıtım sırasında kodu güncelleştirmeden kolayca değiştirilmesi için bu ayarları yapılandırma dosyası seçeneği sağlar.

## <a name="the-entity-framework-configuration-section"></a>Entity Framework yapılandırma bölümü  

EF4.1 ile başlangıç bağlamı kullanarak veritabanı Başlatıcı ayarlayabilirsiniz **appSettings** yapılandırma dosyasının. EF 4.3 içinde özel sunduk **entityFramework** yeni ayarları işlemek için bölüm. Entity Framework hala eski biçimi kullanılarak ayarlanan veritabanı başlatıcılar algılar, ancak mümkün olduğunca yeni biçime geçmeniz önerilir.

**EntityFramework** EntityFramework NuGet paketi yüklendiğinde bölümünde projenizin yapılandırma dosyasına otomatik olarak eklendiğinden.  

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

## <a name="connection-strings"></a>Bağlantı dizeleri  

[Bu sayfa](~/ef6/fundamentals/configuring/connection-strings.md) yapılandırma dosyasında bağlantı dizeleri dahil olmak üzere daha fazla ayrıntı Entity Framework veritabanı kullanılmak üzere nasıl belirlediğini sağlar.  

Bağlantı dizeleri, standart go **connectionStrings** öğesi ve gerekli olmayan **entityFramework** bölümü.  

İlk göre kod modelleri normal ADO.NET bağlantı dizesi kullanır. Örneğin:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

EF Designer modelleri kullanım özel EF bağlantı dizelerini temel. Örneğin:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Kod tabanlı yapılandırma türü (EF6 sonrası)  

EF6 ile başlayarak, kullanılacak EF için DbConfiguration belirtebilirsiniz [kod tabanlı yapılandırma](code-based.md) uygulamanızdaki. Çoğu durumda EF, DbConfiguration otomatik olarak bulmak gibi bu ayarı belirtmeniz gerekmez. Ne zaman yapılandırma dosyanızda DbConfiguration belirtmeniz gerekebilir, Ayrıntılar için bkz. **taşıma DbConfiguration** bölümünü [kod tabanlı yapılandırma](code-based.md).  

DbConfiguration türünü belirlemek için derleme nitelikli tür adı belirtin. **codeConfigurationType** öğesi.  

> [!NOTE]
> Bir bütünleştirilmiş kod adı türü bulunan derleme ardından virgül tarafından izlenen ad alanı tam adıdır. İsteğe bağlı olarak yapabilecekleriniz de derleme sürümü, kültürü ve genel anahtar belirtecini belirtin.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>EF veritabanı sağlayıcıları (EF6 sonrası)  

EF6 önce veritabanı sağlayıcısı Entity Framework özgü bölümlerini çekirdek ADO.NET sağlayıcısı bir parçası olarak dahil edilmesi gerekti. EF6 ile başlayarak, EF belirli kısımlarını artık yönetilir ve ayrı olarak kayıtlı.  

Normalde kendiniz sağlayıcılarını kaydetme gerekmez. Yüklediğinizde bu genellikle sağlayıcı tarafından gerçekleştirilir.  

Sağlayıcılar dahil ederek kayıtlı bir **sağlayıcısı** öğesi altında **sağlayıcıları** alt kısmında **entityFramework** bölümü. Sağlayıcı girişi için gerekli iki öznitelikleri vardır:  

- **Invariantname** temel ADO.NET sağlayıcısı tanımlayan bu EF sağlayıcısı hedefleri  
- **tür** EF sağlayıcı uygulaması derleme nitelikli tür adı  

> [!NOTE]
> Bir bütünleştirilmiş kod adı türü bulunan derleme ardından virgül tarafından izlenen ad alanı tam adıdır. İsteğe bağlı olarak yapabilecekleriniz de derleme sürümü, kültürü ve genel anahtar belirtecini belirtin.  

Örneğin, Entity Framework yüklediğinizde, varsayılan SQL Server sağlayıcıyı kaydetmek için oluşturulan girişi aşağıda verilmiştir.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Dinleyiciler (EF6.1 ve sonraki sürümler)  

Kesiciler, EF6.1 ile başlayan yapılandırma dosyasında kaydedebilirsiniz. Kesiciler, EF açılış bağlantıları, vb. veritabanı sorguları, yürütme gibi bazı işlemleri gerçekleştirirken ek mantık çalıştırmanıza olanak tanır.  

Kesiciler dahil ederek kayıtlı bir **dinleyiciyi** öğesi altında **dinleyicileri** alt kısmında **entityFramework** bölümü. Örneğin, aşağıdaki yapılandırma yerleşik kaydeder **DatabaseLogger** tüm veritabanı işlemleri konsola oturum dinleyiciyi.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Günlük veritabanı işlemleri bir dosyaya (EF6.1 ve sonraki sürümler)  

Günlük bir sorunla ilgili hataları ayıklamak yardımcı olmak için mevcut bir uygulamaya eklemek istediğiniz yapılandırma dosyası aracılığıyla dinleyicileri kaydetme özellikle yararlı olur. **DatabaseLogger** Oluşturucu parametresi olarak dosya adı sağlayarak dosyaya günlük kaydı destekler.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Varsayılan olarak bu günlük dosyası, uygulama her başlatıldığında yeni bir dosya ile üzerine yazılmasına neden olur. Bunun yerine'ına eklenecek dosya zaten varsa kullanın aşağıdakine benzer:  

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

Hakkında daha fazla bilgi için **DatabaseLogger** ve dinleyicileri kayıt, blog gönderisine bakın [EF 6.1: derlemeden günlüğünü açma](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Kod ilk varsayılan bağlantı üretecini  

Yapılandırma bölümü, Code First bağlam için kullanılacak bir veritabanı bulmak için kullanacağı varsayılan bağlantı üretecini belirtmenizi sağlar. Bağlantı dizesi yapılandırma dosyasının bir bağlam için eklendiğinde varsayılan bağlantı üretecini yalnızca kullanılır.  

EF NuGet paketi yüklendiğinde varsayılan bağlantı üretecini SQL Express LocalDB, hangisinin bağlı olarak, yüklü olduğu için veya işaret eden kaydedildi.  

Bağlantı üreteci ayarlamak için derleme nitelikli tür adı belirtin. **deafultConnectionFactory** öğesi.  

> [!NOTE]
> Bir bütünleştirilmiş kod adı türü bulunan derleme ardından virgül tarafından izlenen ad alanı tam adıdır. İsteğe bağlı olarak yapabilecekleriniz de derleme sürümü, kültürü ve genel anahtar belirtecini belirtin.  

Kendi varsayılan bağlantı üretecini ayarlamanın bir örnek aşağıda verilmiştir:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

Yukarıdaki örnekte, parametresiz bir oluşturucu özel fabrika gerektirir. Gerekirse, oluşturucu parametresi kullanarak belirtebilirsiniz **parametreleri** öğesi.  

Örneğin, varlık Çerçevesi'nde bulunan SqlCeConnectionFactory, oluşturucusuna bir sağlayıcının değişmez adı sağlamanız gerekir. Sağlayıcının değişmez adı kullanmak istediğiniz SQL Compact sürümünü tanımlar. Aşağıdaki yapılandırma, SQL Compact sürüm 4. 0'ın varsayılan olarak kullanılacak bağlamları neden olur.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Varsayılan bağlantı üretecini ayarlamazsanız, Code First işaret SqlConnectionFactory kullanan `.\SQLEXPRESS`. SqlConnectionFactory, ayrıca bağlantı dizesi bölümleri geçersiz kılmak izin veren bir oluşturucusu vardır. Başka bir SQL Server örneğini kullanmak istiyorsanız `.\SQLEXPRESS` sunucusu için bu oluşturucu kullanabilirsiniz.  

Aşağıdaki yapılandırmayı kullanmak Code First neden **MyDatabaseServer** için ayarlanmış bir açık bir bağlantı dizesi yok bağlamı.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Varsayılan olarak, oluşturucu bağımsız değişkenleri dize türünde olduğu varsayılır. Bunu değiştirmek için type özniteliği kullanabilirsiniz.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Veritabanı başlatıcıları  

Veritabanı başlatıcılar bağlam içi olarak yapılandırılır. Kullanılarak yapılandırma dosyasında ayarlanabilir **bağlam** öğesi. Bu öğesi derleme nitelenmiş adı yapılandırılmakta içeriği tanımlamak için kullanır.  

Varsayılan olarak, Code First bağlamları Createdatabaseıfnotexists Başlatıcı kullanmak üzere yapılandırılmış. Var olan bir **disableDatabaseInitialization** özniteliği **bağlam** veritabanı başlatma devre dışı bırakmak için kullanılabilir öğe.  

Örneğin, aşağıdaki yapılandırma veritabanı başlatma da MyAssembly.dll tanımlanan Blogging.BlogContext bağlamı için devre dışı bırakır.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Kullanabileceğiniz **databaseInitializer** özel bir başlatıcı ayarlamak için öğesi.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Oluşturucu parametresi varsayılan bağlantı fabrikaları aynı sözdizimini kullanır.  

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

Entity Framework'e dahil edilen genel veritabanı başlatıcılar birini yapılandırabilirsiniz. **Türü** özniteliği, genel türler için .NET Framework biçimi kullanır.  

Örneğin, Code First Migrations'ı kullanıyorsanız, veritabanı kullanılarak otomatik olarak geçirilecek yapılandırabilirsiniz `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` Başlatıcı.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
