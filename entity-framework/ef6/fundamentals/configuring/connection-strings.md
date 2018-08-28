---
title: Bağlantı dizelerini ve modelleri - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: dce414ea84f13235691abf0dcadef5c743d90f9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994965"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="a4414-102">Bağlantı dizelerini ve modeller</span><span class="sxs-lookup"><span data-stu-id="a4414-102">Connection strings and models</span></span>
<span data-ttu-id="a4414-103">Bu konuda, Entity Framework için kullanılacak veritabanı bağlantıyı nasıl bulur ve nasıl değiştirebileceğinizi kapsar.</span><span class="sxs-lookup"><span data-stu-id="a4414-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="a4414-104">Code First ve EF Designer ile oluşturulan modeller Bu konu başlığında ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a4414-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="a4414-105">Genellikle bir Entity Framework uygulamasını DbContext türetilmiş bir sınıf kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4414-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="a4414-106">Bu türetilmiş sınıf oluşturucular bir denetim temel DbContext sınıfına çağırmak:</span><span class="sxs-lookup"><span data-stu-id="a4414-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="a4414-107">Bağlam bir veritabanına nasıl bağlanacağını — diğer bir deyişle, nasıl bir bağlantı dizesi bulunamadı/kullanılır</span><span class="sxs-lookup"><span data-stu-id="a4414-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="a4414-108">Bağlam kullanıp kullanmayacağını Code First kullanarak bir model hesaplamak veya EF Designer ile oluşturulan bir modeli yüklenemiyor</span><span class="sxs-lookup"><span data-stu-id="a4414-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="a4414-109">Ek Gelişmiş Seçenekler</span><span class="sxs-lookup"><span data-stu-id="a4414-109">Additional advanced options</span></span>  

<span data-ttu-id="a4414-110">DbContext oluşturucular yollardan bazılarını kullanılabilir aşağıdaki parçalarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a4414-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="a4414-111">Code First bağlantı ile kural olarak kullanın</span><span class="sxs-lookup"><span data-stu-id="a4414-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="a4414-112">Uygulamanızda herhangi bir yapılandırma yapmadıysanız, parametresiz bir oluşturucu üzerinde DbContext sonra çağırma kuralı tarafından oluşturulmuş bir veritabanı bağlantısı ile Code First modunda çalışacak şekilde DbContext neden olur.</span><span class="sxs-lookup"><span data-stu-id="a4414-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="a4414-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a4414-113">For example:</span></span>  

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

<span data-ttu-id="a4414-114">Bu örnekte DbContext, türetilmiş bağlam class—Demo.EF.BloggingContext—as veritabanı adı tam ad alanı adını kullanır ve SQL Express veya LocalDB kullanarak bu veritabanı için bir bağlantı dizesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a4414-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="a4414-115">Her ikisi de yüklüyse, SQL Express kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a4414-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="a4414-116">Visual Studio 2010, SQL Express varsayılan ve Visual Studio 2012 tarafından içerir ve daha sonra LocalDB içerir.</span><span class="sxs-lookup"><span data-stu-id="a4414-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="a4414-117">Yükleme sırasında hangi veritabanı sunucusu kullanılabilir EntityFramework NuGet paketi denetler.</span><span class="sxs-lookup"><span data-stu-id="a4414-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="a4414-118">Kural gereği bağlantı oluştururken Code First kullanan varsayılan veritabanı sunucusu ayarlayarak NuGet paketini daha sonra yapılandırma dosyasını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="a4414-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="a4414-119">SQL Express çalışıyorsa kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a4414-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="a4414-120">SQL Express kullanılabilir değilse, ardından LocalDB varsayılan olarak bunun yerine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a4414-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="a4414-121">Varsayılan bağlantı üreteci için bir ayar varsa yapılandırma dosyasına hiçbir değişiklik yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="a4414-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="a4414-122">Code First bağlantıyla kuralı ve belirtilen veritabanı adı kullanın</span><span class="sxs-lookup"><span data-stu-id="a4414-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="a4414-123">Uygulamanızda herhangi bir yapılandırma yapmadıysanız, kullanmak istediğiniz veritabanı adıyla'da DbContext dize yapılandırıcının çağrılmasıyla veritabanına kural tarafından oluşturulan bir veritabanı bağlantısı ile Code First modunda çalışacak şekilde DbContext neden olur Bu adı.</span><span class="sxs-lookup"><span data-stu-id="a4414-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="a4414-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a4414-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="a4414-125">Bu örnekte DbContext "BloggingDatabase" adlı veritabanı kullanır ve (Visual Studio 2010 ile yüklü), SQL Express veya LocalDB (Visual Studio 2012 ile yüklenen) kullanarak bu veritabanı için bir bağlantı dizesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a4414-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="a4414-126">Her ikisi de yüklüyse, SQL Express kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a4414-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="a4414-127">Code First app.config/web.config dosyasındaki bağlantı dizesi ile kullanma</span><span class="sxs-lookup"><span data-stu-id="a4414-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="a4414-128">App.config veya web.config dosyanızda bir bağlantı dizesi koymak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4414-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="a4414-129">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a4414-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="a4414-130">SQL Express ya da LocalDB dışındaki bir veritabanı sunucusunu kullanmak üzere DbContext bildirmek için kolay bir yol budur; Yukarıdaki örnek bir SQL Server Compact Edition veritabanını belirtir.</span><span class="sxs-lookup"><span data-stu-id="a4414-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="a4414-131">Bağlantı dizesinin adını bağlamınızın (ile veya ad alanı nitelik olmadan) ad eşleşiyorsa parametresiz kullanıldığında, ardından bunu DbContext tarafından bulunur.</span><span class="sxs-lookup"><span data-stu-id="a4414-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="a4414-132">Bağlantı dizesi adı Bağlamınızı adından farklıysa, bağlantı dizesi adı DbContext oluşturucusuna geçirerek Code First modunda bu bağlantıyı kullanmak için DbContext söyleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4414-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="a4414-133">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a4414-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="a4414-134">Alternatif olarak, form kullanabilirsiniz "name =\<bağlantı dizesi adı\>" DbContext oluşturucuya geçirilen bir dize.</span><span class="sxs-lookup"><span data-stu-id="a4414-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="a4414-135">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a4414-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="a4414-136">Bu formu açık bağlantı dizesini yapılandırma dosyanızda bulunan beklediğiniz kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="a4414-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="a4414-137">Belirtilen ada sahip bir bağlantı dizesi bulunmazsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a4414-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="a4414-138">İlk app.config/web.config dosyasındaki bağlantı dizesi ile veritabanı/modeli</span><span class="sxs-lookup"><span data-stu-id="a4414-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="a4414-139">EF Designer ile oluşturulan modeller, modelinizi zaten var ve uygulama çalıştığında koddan oluşturulan değil, Code First farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="a4414-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="a4414-140">Model, genellikle, projenizdeki bir EDMX dosyası olarak bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="a4414-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="a4414-141">Tasarımcı için app.config veya web.config dosyanızda bir EF bağlantı dizesini ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="a4414-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="a4414-142">EDMX dosyanız bilgileri bulma hakkında bilgiler içerir, bu bağlantı dizesini özeldir.</span><span class="sxs-lookup"><span data-stu-id="a4414-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="a4414-143">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a4414-143">For example:</span></span>  

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

<span data-ttu-id="a4414-144">EF Designer DbContext DbContext oluşturucuya bağlantı dizesi adı geçirerek bu bağlantıyı kullanmak için bildiren kod ayrıca oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a4414-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="a4414-145">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a4414-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="a4414-146">DbContext bilir varolan modeli (yerine koddan hesaplamak için Code First kullanarak) yüklemek için bağlantı dizesini kullanmak için modelin ayrıntılarını içeren bir EF bağlantı dizesi olduğundan.</span><span class="sxs-lookup"><span data-stu-id="a4414-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="a4414-147">Diğer DbContext Oluşturucusu seçenekleri</span><span class="sxs-lookup"><span data-stu-id="a4414-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="a4414-148">DbContext sınıfı diğer oluşturucular ve bazı daha gelişmiş senaryoları etkinleştirmek kullanım düzenlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="a4414-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="a4414-149">Bunlardan bazıları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a4414-149">Some of these are:</span></span>  

- <span data-ttu-id="a4414-150">DbModelBuilder sınıfı bir DbContext örneği örnekleme olmadan bir Code First modelini oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4414-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="a4414-151">Bunun sonucu olarak bir DbModel nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="a4414-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="a4414-152">DbContext örneğinizi oluşturmak hazır olduğunuzda, ardından bu DbModel nesne bir DbContext oluşturucular geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4414-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="a4414-153">DbContext için yalnızca veritabanı veya bağlantı dizesi adı yerine tam bağlantı dizesi geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4414-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="a4414-154">Varsayılan olarak, bu bağlantı dizesi ile System.Data.SqlClient sağlayıcısı kullanılır; Bu, farklı bir uygulama IConnectionFactory bağlam üzerine ayarlayarak değiştirilebilir. Database.DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="a4414-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="a4414-155">DbContext oluşturucusuna geçirerek, varolan bir DbConnection nesnesini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4414-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="a4414-156">Bağlantı nesnesi EntityConnection, örneğini, belirtilen bağlantı modeli kullanılan yerine hesaplamak modeli kullanarak kodu ilk kez.</span><span class="sxs-lookup"><span data-stu-id="a4414-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="a4414-157">Nesneyi başka bir tür örneği olup olmadığını — Örneğin, SqlConnection — bağlamı için Code First modunu kullanır.</span><span class="sxs-lookup"><span data-stu-id="a4414-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="a4414-158">Mevcut bir bağlamı sarmalama DbContext oluşturmak için bir DbContext oluşturucuya varolan bir Objectcontext'e geçirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a4414-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="a4414-159">Bu ObjectContext kullanan, ancak bazı bölümleri uygulamanın DbContext yararlanmak istediğiniz var olan uygulamalar için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a4414-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
