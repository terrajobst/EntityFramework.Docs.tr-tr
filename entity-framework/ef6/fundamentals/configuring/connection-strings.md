---
title: Bağlantı dizeleri ve modeller-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419299"
---
# <a name="connection-strings-and-models"></a>Bağlantı dizeleri ve modeller
Bu konu, hangi veritabanı bağlantısının kullanılacağını ve bunu nasıl değiştirebildiğini Entity Framework nasıl keşfedeceğini ele alır. Code First ve EF Designer ile oluşturulan modeller bu konuda ele alınmıştır.  

Genellikle bir Entity Framework uygulama DbContext 'ten türetilmiş bir sınıfı kullanır. Bu türetilmiş sınıf, denetlemek için Base DbContext sınıfındaki oluşturuculardan birini çağırır:  

- Bağlamın bir veritabanına bağlanması — yani bir bağlantı dizesinin nasıl bulunduğu/kullanıldığı  
- Bağlamın Code First kullanarak model hesaplama veya EF Designer ile oluşturulmuş bir modeli yükleme kullanılacağını belirtir  
- Ek Gelişmiş Seçenekler  

Aşağıdaki parçalar, DbContext oluşturucuların kullanılabileceği bazı yolları göstermektedir.  

## <a name="use-code-first-with-connection-by-convention"></a>Kurala göre bağlantı ile Code First kullanma  

Uygulamanızda başka bir yapılandırma yapmadıysanız, parametresiz oluşturucuyu DbContext üzerinde çağırmak, DbContext 'in kural tarafından oluşturulan bir veritabanı bağlantısıyla Code First modunda çalışmasına neden olur. Örnek:  

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

Bu örnekte, DbContext türetilmiş bağlam sınıfınızın ad alanını (demo. EF. BloggingContext) veritabanı adı olarak kullanır ve SQL Express veya LocalDB kullanarak bu veritabanı için bir bağlantı dizesi oluşturur. Her ikisi de yüklüyse, SQL Express kullanılacaktır.  

Visual Studio 2010, varsayılan olarak SQL Express 'i içerir ve Visual Studio 2012 ve sonraki sürümleri LocalDB içerir. Yükleme sırasında, EntityFramework NuGet paketi hangi veritabanı sunucusunun kullanılabilir olduğunu denetler. NuGet paketi, bir bağlantı kuralı oluştururken Code First kullandığı varsayılan veritabanı sunucusunu ayarlayarak yapılandırma dosyasını güncelleştirir. SQL Express çalışıyorsa, kullanılacaktır. SQL Express kullanılamıyorsa, LocalDB bunun yerine varsayılan olarak kaydedilir. Varsayılan bağlantı fabrikası için zaten bir ayar varsa yapılandırma dosyasında değişiklik yapılmaz.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Kurala göre bağlantı ve belirtilen veritabanı adı ile Code First kullanın  

Uygulamanızda başka herhangi bir yapılandırma yapmadıysanız, kullanmak istediğiniz veritabanı adıyla DbContext üzerinde dize oluşturucusunu çağırmak, DbContext 'in kural tarafından oluşturulan bir veritabanı bağlantısıyla Code First modunda çalışmasına neden olur. Bu ad. Örnek:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

Bu örnekte, DbContext veritabanı adı olarak "BloggingDatabase" kullanır ve SQL Express (Visual Studio 2010 ile yüklenir) veya LocalDB (Visual Studio 2012 ile yüklenir) kullanarak bu veritabanı için bir bağlantı dizesi oluşturur. Her ikisi de yüklüyse, SQL Express kullanılacaktır.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>App. config/Web. config dosyasında bağlantı dizesiyle Code First kullanın  

App. config veya Web. config dosyanıza bir bağlantı dizesi koymak için seçim yapabilirsiniz. Örnek:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Bu, DbContext 'in SQL Express veya LocalDB dışında bir veritabanı sunucusunu kullanmasını söylemek için kolay bir yoldur — yukarıdaki örnek bir SQL Server Compact Edition veritabanı belirtir.  

Bağlantı dizesinin adı, bağlamınızın adı ile eşleşiyorsa (ad alanı nitelemesiz ya da olmadan), parametresiz Oluşturucu kullanıldığında DbContext tarafından bulunur. Bağlantı dizesi adı bağlamınızın adından farklıysa, DbContext 'in bu bağlantıyı Code First modunda kullanmasını, bağlantı dizesi adını DbContext oluşturucusuna geçirerek söyleyebilirsiniz. Örnek:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Alternatif olarak, DbContext oluşturucusuna geçirilen dize için "ad =\<bağlantı dizesi adı\>" biçimini kullanabilirsiniz. Örnek:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Bu form, bağlantı dizesinin yapılandırma dosyanızda bulunabilmesini beklediğinizi açık hale getirir. Verilen ada sahip bir bağlantı dizesi bulunmazsa bir özel durum oluşturulur.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>App. config/Web. config dosyasında bağlantı dizesiyle veritabanı/Model First  

EF Designer ile oluşturulan modeller modelinizdeki Code First farklıdır ve uygulama çalışırken koddan oluşturulmaz. Model genellikle projenizde bir EDMX dosyası olarak mevcuttur.  

Tasarımcı, App. config veya Web. config dosyanıza bir EF bağlantı dizesi ekler. Bu bağlantı dizesi, EDMX dosyasında bilgilerin nasıl bulunacağı hakkında bilgi içermesi için özeldir. Örnek:  

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

EF Designer Ayrıca, bağlantı dizesi adını DbContext oluşturucusuna geçirerek, DbContext 'in bu bağlantıyı kullanmasını söyleyen kodu oluşturur. Örnek:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext, bağlantı dizesi kullanılacak modelin ayrıntılarını içeren bir EF bağlantı dizesi olduğundan, var olan modeli (koddan hesaplamak için Code First kullanmak yerine) yüklemeyi bilir.  

## <a name="other-dbcontext-constructor-options"></a>Diğer DbContext Oluşturucu seçenekleri  

DbContext sınıfı, daha gelişmiş senaryolar sağlayan diğer oluşturucular ve kullanım düzenlerini içerir. Bunlardan bazıları şunlardır:  

- DbModelBuilder sınıfını bir DbContext örneği örneklemeden bir Code First modeli oluşturmak için kullanabilirsiniz. Bunun sonucu bir DbModel nesnesidir. Daha sonra DbContext örneğinizi oluşturmaya hazırsanız bu DbModel nesnesini DbContext oluşturucularından birine geçirebilirsiniz.  
- Tam bir bağlantı dizesini yalnızca veritabanı veya bağlantı dizesi adı yerine DbContext 'e geçirebilirsiniz. Varsayılan olarak, bu bağlantı dizesi System. Data. SqlClient sağlayıcısıyla birlikte kullanılır; Bu, IConnectionFactory 'in farklı bir uygulamasına bağlam üzerine ayarlanarak değiştirilebilir. Database. DefaultConnectionFactory.  
- Var olan bir DbConnection nesnesini DbContext oluşturucusuna geçirerek kullanabilirsiniz. Bağlantı nesnesi EntityConnection 'un bir örneği ise, bağlantıda belirtilen model Code First kullanarak bir modeli hesaplamak yerine kullanılacaktır. Nesne başka bir türün örneğidir — Örneğin, SqlConnection — daha sonra bağlam, Code First modu için kullanacaktır.  
- Var olan bağlamı sarmalama oluşturmak için var olan bir ObjectContext 'i bir DbContext oluşturucusuna geçirebilirsiniz. Bu, ObjectContext kullanan mevcut uygulamalar için ve uygulamanın bazı bölümlerinde DbContext 'in avantajlarından yararlanmak için kullanılabilir.  
