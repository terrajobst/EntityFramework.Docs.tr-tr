---
title: Bağlantı dizelerini ve modelleri - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: afb13998a6482b3e7a7e250892854ab7599f109d
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912069"
---
# <a name="connection-strings-and-models"></a>Bağlantı dizelerini ve modeller
Bu konuda, Entity Framework için kullanılacak veritabanı bağlantıyı nasıl bulur ve nasıl değiştirebileceğinizi kapsar. Code First ve EF Designer ile oluşturulan modeller Bu konu başlığında ele alınmaktadır.  

Genellikle bir Entity Framework uygulamasını DbContext türetilmiş bir sınıf kullanır. Bu türetilmiş sınıf oluşturucular bir denetim temel DbContext sınıfına çağırmak:  

- Nasıl bağlamı bir database—i.e nasıl bir bağlantı dizesi bulundu ve kullanılan bağlanmak  
- Bağlam kullanıp kullanmayacağını Code First kullanarak bir model hesaplamak veya EF Designer ile oluşturulan bir modeli yüklenemiyor  
- Ek Gelişmiş Seçenekler  

DbContext oluşturucular yollardan bazılarını kullanılabilir aşağıdaki parçalarını gösterir.  

## <a name="use-code-first-with-connection-by-convention"></a>Code First bağlantı ile kural olarak kullanın  

Uygulamanızda herhangi bir yapılandırma yapmadıysanız, parametresiz bir oluşturucu üzerinde DbContext sonra çağırma kuralı tarafından oluşturulmuş bir veritabanı bağlantısı ile Code First modunda çalışacak şekilde DbContext neden olur. Örneğin:  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

Bu örnekte DbContext, türetilmiş bağlam class—Demo.EF.BloggingContext—as veritabanı adı tam ad alanı adını kullanır ve SQL Express veya LocalDB kullanarak bu veritabanı için bir bağlantı dizesi oluşturur. Her ikisi de yüklüyse, SQL Express kullanılır.  

Visual Studio 2010, SQL Express varsayılan ve Visual Studio 2012 tarafından içerir ve daha sonra LocalDB içerir. Yükleme sırasında hangi veritabanı sunucusu kullanılabilir EntityFramework NuGet paketi denetler. Kural gereği bağlantı oluştururken Code First kullanan varsayılan veritabanı sunucusu ayarlayarak NuGet paketini daha sonra yapılandırma dosyasını güncelleştirin. SQL Express çalışıyorsa kullanılır. SQL Express kullanılabilir değilse, ardından LocalDB varsayılan olarak bunun yerine kaydedilir. Varsayılan bağlantı üreteci için bir ayar varsa yapılandırma dosyasına hiçbir değişiklik yapılmaz.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Code First bağlantıyla kuralı ve belirtilen veritabanı adı kullanın  

Uygulamanızda herhangi bir yapılandırma yapmadıysanız, kullanmak istediğiniz veritabanı adıyla'da DbContext dize yapılandırıcının çağrılmasıyla veritabanına kural tarafından oluşturulan bir veritabanı bağlantısı ile Code First modunda çalışacak şekilde DbContext neden olur Bu adı. Örneğin:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

Bu örnekte DbContext "BloggingDatabase" adlı veritabanı kullanır ve (Visual Studio 2010 ile yüklü), SQL Express veya LocalDB (Visual Studio 2012 ile yüklenen) kullanarak bu veritabanı için bir bağlantı dizesi oluşturur. Her ikisi de yüklüyse, SQL Express kullanılır.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Code First app.config/web.config dosyasındaki bağlantı dizesi ile kullanma  

App.config veya web.config dosyanızda bir bağlantı dizesi koymak isteyebilirsiniz. Örneğin:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

SQL Express ya da LocalDB dışındaki bir veritabanı sunucusunu kullanmak üzere DbContext bildirmek için kolay bir yol budur; Yukarıdaki örnek bir SQL Server Compact Edition veritabanını belirtir.  

Bağlantı dizesinin adını bağlamınızın (ile veya ad alanı nitelik olmadan) ad eşleşiyorsa parametresiz kullanıldığında, ardından bunu DbContext tarafından bulunur. Bağlantı dizesi adı Bağlamınızı adından farklıysa, bağlantı dizesi adı DbContext oluşturucusuna geçirerek Code First modunda bu bağlantıyı kullanmak için DbContext söyleyebilirsiniz. Örneğin:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Alternatif olarak, form kullanabilirsiniz "name =\<bağlantı dizesi adı\>" DbContext oluşturucuya geçirilen bir dize. Örneğin:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Bu formu açık bağlantı dizesini yapılandırma dosyanızda bulunan beklediğiniz kolaylaştırır. Belirtilen ada sahip bir bağlantı dizesi bulunmazsa, bir özel durum oluşturulur.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>İlk app.config/web.config dosyasındaki bağlantı dizesi ile veritabanı/modeli  

EF Designer ile oluşturulan modeller, modelinizi zaten var ve uygulama çalıştığında koddan oluşturulan değil, Code First farklılık gösterir. Model, genellikle, projenizdeki bir EDMX dosyası olarak bulunmaktadır.  

Tasarımcı için app.config veya web.config dosyanızda bir EF bağlantı dizesini ekleyeceksiniz. EDMX dosyanız bilgileri bulma hakkında bilgiler içerir, bu bağlantı dizesini özeldir. Örneğin:  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

EF Designer DbContext DbContext oluşturucuya bağlantı dizesi adı geçirerek bu bağlantıyı kullanmak için bildiren kod ayrıca oluşturur. Örneğin:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext bilir varolan modeli (yerine koddan hesaplamak için Code First kullanarak) yüklemek için bağlantı dizesini kullanmak için modelin ayrıntılarını içeren bir EF bağlantı dizesi olduğundan.  

## <a name="other-dbcontext-constructor-options"></a>Diğer DbContext Oluşturucusu seçenekleri  

DbContext sınıfı diğer oluşturucular ve bazı daha gelişmiş senaryoları etkinleştirmek kullanım düzenlerini içerir. Bunlardan bazıları şunlardır:  

- DbModelBuilder sınıfı bir DbContext örneği örnekleme olmadan bir Code First modelini oluşturmak için kullanabilirsiniz. Bunun sonucu olarak bir DbModel nesnesidir. DbContext örneğinizi oluşturmak hazır olduğunuzda, ardından bu DbModel nesne bir DbContext oluşturucular geçirebilirsiniz.  
- DbContext için yalnızca veritabanı veya bağlantı dizesi adı yerine tam bağlantı dizesi geçirebilirsiniz. Varsayılan olarak, bu bağlantı dizesi ile System.Data.SqlClient sağlayıcısı kullanılır; Bu, farklı bir uygulama IConnectionFactory bağlam üzerine ayarlayarak değiştirilebilir. Database.DefaultConnectionFactory.  
- DbContext oluşturucusuna geçirerek, varolan bir DbConnection nesnesini kullanabilirsiniz. Bağlantı nesnesi EntityConnection, örneğini, belirtilen bağlantı modeli kullanılan yerine hesaplamak modeli kullanarak kodu ilk kez. Nesneyi başka bir tür örneği olup olmadığını — Örneğin, SqlConnection — bağlamı için Code First modunu kullanır.  
- Mevcut bir bağlamı sarmalama DbContext oluşturmak için bir DbContext oluşturucuya varolan bir Objectcontext'e geçirebilirsiniz. Bu ObjectContext kullanan, ancak bazı bölümleri uygulamanın DbContext yararlanmak istediğiniz var olan uygulamalar için kullanılabilir.  
