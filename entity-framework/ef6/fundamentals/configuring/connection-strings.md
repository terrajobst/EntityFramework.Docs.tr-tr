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
# <a name="connection-strings-and-models"></a><span data-ttu-id="51629-102">Bağlantı dizeleri ve modeller</span><span class="sxs-lookup"><span data-stu-id="51629-102">Connection strings and models</span></span>
<span data-ttu-id="51629-103">Bu konu, hangi veritabanı bağlantısının kullanılacağını ve bunu nasıl değiştirebildiğini Entity Framework nasıl keşfedeceğini ele alır.</span><span class="sxs-lookup"><span data-stu-id="51629-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="51629-104">Code First ve EF Designer ile oluşturulan modeller bu konuda ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="51629-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="51629-105">Genellikle bir Entity Framework uygulama DbContext 'ten türetilmiş bir sınıfı kullanır.</span><span class="sxs-lookup"><span data-stu-id="51629-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="51629-106">Bu türetilmiş sınıf, denetlemek için Base DbContext sınıfındaki oluşturuculardan birini çağırır:</span><span class="sxs-lookup"><span data-stu-id="51629-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="51629-107">Bağlamın bir veritabanına bağlanması — yani bir bağlantı dizesinin nasıl bulunduğu/kullanıldığı</span><span class="sxs-lookup"><span data-stu-id="51629-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="51629-108">Bağlamın Code First kullanarak model hesaplama veya EF Designer ile oluşturulmuş bir modeli yükleme kullanılacağını belirtir</span><span class="sxs-lookup"><span data-stu-id="51629-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="51629-109">Ek Gelişmiş Seçenekler</span><span class="sxs-lookup"><span data-stu-id="51629-109">Additional advanced options</span></span>  

<span data-ttu-id="51629-110">Aşağıdaki parçalar, DbContext oluşturucuların kullanılabileceği bazı yolları göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="51629-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="51629-111">Kurala göre bağlantı ile Code First kullanma</span><span class="sxs-lookup"><span data-stu-id="51629-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="51629-112">Uygulamanızda başka bir yapılandırma yapmadıysanız, parametresiz oluşturucuyu DbContext üzerinde çağırmak, DbContext 'in kural tarafından oluşturulan bir veritabanı bağlantısıyla Code First modunda çalışmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="51629-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="51629-113">Örnek:</span><span class="sxs-lookup"><span data-stu-id="51629-113">For example:</span></span>  

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

<span data-ttu-id="51629-114">Bu örnekte, DbContext türetilmiş bağlam sınıfınızın ad alanını (demo. EF. BloggingContext) veritabanı adı olarak kullanır ve SQL Express veya LocalDB kullanarak bu veritabanı için bir bağlantı dizesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="51629-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="51629-115">Her ikisi de yüklüyse, SQL Express kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="51629-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="51629-116">Visual Studio 2010, varsayılan olarak SQL Express 'i içerir ve Visual Studio 2012 ve sonraki sürümleri LocalDB içerir.</span><span class="sxs-lookup"><span data-stu-id="51629-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="51629-117">Yükleme sırasında, EntityFramework NuGet paketi hangi veritabanı sunucusunun kullanılabilir olduğunu denetler.</span><span class="sxs-lookup"><span data-stu-id="51629-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="51629-118">NuGet paketi, bir bağlantı kuralı oluştururken Code First kullandığı varsayılan veritabanı sunucusunu ayarlayarak yapılandırma dosyasını güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="51629-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="51629-119">SQL Express çalışıyorsa, kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="51629-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="51629-120">SQL Express kullanılamıyorsa, LocalDB bunun yerine varsayılan olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="51629-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="51629-121">Varsayılan bağlantı fabrikası için zaten bir ayar varsa yapılandırma dosyasında değişiklik yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="51629-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="51629-122">Kurala göre bağlantı ve belirtilen veritabanı adı ile Code First kullanın</span><span class="sxs-lookup"><span data-stu-id="51629-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="51629-123">Uygulamanızda başka herhangi bir yapılandırma yapmadıysanız, kullanmak istediğiniz veritabanı adıyla DbContext üzerinde dize oluşturucusunu çağırmak, DbContext 'in kural tarafından oluşturulan bir veritabanı bağlantısıyla Code First modunda çalışmasına neden olur. Bu ad.</span><span class="sxs-lookup"><span data-stu-id="51629-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="51629-124">Örnek:</span><span class="sxs-lookup"><span data-stu-id="51629-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="51629-125">Bu örnekte, DbContext veritabanı adı olarak "BloggingDatabase" kullanır ve SQL Express (Visual Studio 2010 ile yüklenir) veya LocalDB (Visual Studio 2012 ile yüklenir) kullanarak bu veritabanı için bir bağlantı dizesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="51629-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="51629-126">Her ikisi de yüklüyse, SQL Express kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="51629-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="51629-127">App. config/Web. config dosyasında bağlantı dizesiyle Code First kullanın</span><span class="sxs-lookup"><span data-stu-id="51629-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="51629-128">App. config veya Web. config dosyanıza bir bağlantı dizesi koymak için seçim yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51629-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="51629-129">Örnek:</span><span class="sxs-lookup"><span data-stu-id="51629-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="51629-130">Bu, DbContext 'in SQL Express veya LocalDB dışında bir veritabanı sunucusunu kullanmasını söylemek için kolay bir yoldur — yukarıdaki örnek bir SQL Server Compact Edition veritabanı belirtir.</span><span class="sxs-lookup"><span data-stu-id="51629-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="51629-131">Bağlantı dizesinin adı, bağlamınızın adı ile eşleşiyorsa (ad alanı nitelemesiz ya da olmadan), parametresiz Oluşturucu kullanıldığında DbContext tarafından bulunur.</span><span class="sxs-lookup"><span data-stu-id="51629-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="51629-132">Bağlantı dizesi adı bağlamınızın adından farklıysa, DbContext 'in bu bağlantıyı Code First modunda kullanmasını, bağlantı dizesi adını DbContext oluşturucusuna geçirerek söyleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51629-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="51629-133">Örnek:</span><span class="sxs-lookup"><span data-stu-id="51629-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="51629-134">Alternatif olarak, DbContext oluşturucusuna geçirilen dize için "ad =\<bağlantı dizesi adı\>" biçimini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51629-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="51629-135">Örnek:</span><span class="sxs-lookup"><span data-stu-id="51629-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="51629-136">Bu form, bağlantı dizesinin yapılandırma dosyanızda bulunabilmesini beklediğinizi açık hale getirir.</span><span class="sxs-lookup"><span data-stu-id="51629-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="51629-137">Verilen ada sahip bir bağlantı dizesi bulunmazsa bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="51629-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="51629-138">App. config/Web. config dosyasında bağlantı dizesiyle veritabanı/Model First</span><span class="sxs-lookup"><span data-stu-id="51629-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="51629-139">EF Designer ile oluşturulan modeller modelinizdeki Code First farklıdır ve uygulama çalışırken koddan oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="51629-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="51629-140">Model genellikle projenizde bir EDMX dosyası olarak mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="51629-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="51629-141">Tasarımcı, App. config veya Web. config dosyanıza bir EF bağlantı dizesi ekler.</span><span class="sxs-lookup"><span data-stu-id="51629-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="51629-142">Bu bağlantı dizesi, EDMX dosyasında bilgilerin nasıl bulunacağı hakkında bilgi içermesi için özeldir.</span><span class="sxs-lookup"><span data-stu-id="51629-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="51629-143">Örnek:</span><span class="sxs-lookup"><span data-stu-id="51629-143">For example:</span></span>  

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

<span data-ttu-id="51629-144">EF Designer Ayrıca, bağlantı dizesi adını DbContext oluşturucusuna geçirerek, DbContext 'in bu bağlantıyı kullanmasını söyleyen kodu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="51629-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="51629-145">Örnek:</span><span class="sxs-lookup"><span data-stu-id="51629-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="51629-146">DbContext, bağlantı dizesi kullanılacak modelin ayrıntılarını içeren bir EF bağlantı dizesi olduğundan, var olan modeli (koddan hesaplamak için Code First kullanmak yerine) yüklemeyi bilir.</span><span class="sxs-lookup"><span data-stu-id="51629-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="51629-147">Diğer DbContext Oluşturucu seçenekleri</span><span class="sxs-lookup"><span data-stu-id="51629-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="51629-148">DbContext sınıfı, daha gelişmiş senaryolar sağlayan diğer oluşturucular ve kullanım düzenlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="51629-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="51629-149">Bunlardan bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="51629-149">Some of these are:</span></span>  

- <span data-ttu-id="51629-150">DbModelBuilder sınıfını bir DbContext örneği örneklemeden bir Code First modeli oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51629-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="51629-151">Bunun sonucu bir DbModel nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="51629-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="51629-152">Daha sonra DbContext örneğinizi oluşturmaya hazırsanız bu DbModel nesnesini DbContext oluşturucularından birine geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51629-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="51629-153">Tam bir bağlantı dizesini yalnızca veritabanı veya bağlantı dizesi adı yerine DbContext 'e geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51629-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="51629-154">Varsayılan olarak, bu bağlantı dizesi System. Data. SqlClient sağlayıcısıyla birlikte kullanılır; Bu, IConnectionFactory 'in farklı bir uygulamasına bağlam üzerine ayarlanarak değiştirilebilir. Database. DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="51629-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="51629-155">Var olan bir DbConnection nesnesini DbContext oluşturucusuna geçirerek kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51629-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="51629-156">Bağlantı nesnesi EntityConnection 'un bir örneği ise, bağlantıda belirtilen model Code First kullanarak bir modeli hesaplamak yerine kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="51629-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="51629-157">Nesne başka bir türün örneğidir — Örneğin, SqlConnection — daha sonra bağlam, Code First modu için kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="51629-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="51629-158">Var olan bağlamı sarmalama oluşturmak için var olan bir ObjectContext 'i bir DbContext oluşturucusuna geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51629-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="51629-159">Bu, ObjectContext kullanan mevcut uygulamalar için ve uygulamanın bazı bölümlerinde DbContext 'in avantajlarından yararlanmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="51629-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
