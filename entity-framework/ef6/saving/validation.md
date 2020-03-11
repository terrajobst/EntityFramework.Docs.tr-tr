---
title: Doğrulama-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 2c5e6f1b3f60862124bafcac42e8859a7591f8e6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416962"
---
# <a name="data-validation"></a><span data-ttu-id="3748a-102">Veri Doğrulama</span><span class="sxs-lookup"><span data-stu-id="3748a-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="3748a-103">**EF 4.1 yalnızca** sonraki sürümler-bu sayfada açıklanan özellikler, API 'ler vb. Entity Framework 4,1 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="3748a-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="3748a-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz</span><span class="sxs-lookup"><span data-stu-id="3748a-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="3748a-105">Bu sayfadaki içerik, başlangıçta Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)) tarafından yazılmış bir makaleden uyarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3748a-105">The content on this page is adapted from an article originally written by Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)).</span></span>

<span data-ttu-id="3748a-106">Entity Framework, istemci tarafı doğrulama için bir kullanıcı arabirimine veya sunucu tarafı doğrulaması için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3748a-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="3748a-107">Önce kodu kullanırken, daha fazla açıklama veya Fluent API yapılandırma kullanarak doğrulama belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="3748a-108">Kod içinde ek doğrulamalar ve daha karmaşık bir şekilde belirlenebilir ve önce, modelinizde önce koddan, modelden önce veya önce modelinizden çekip önlenebilir.</span><span class="sxs-lookup"><span data-stu-id="3748a-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="3748a-109">Model</span><span class="sxs-lookup"><span data-stu-id="3748a-109">The model</span></span>

<span data-ttu-id="3748a-110">Basit bir sınıf çiftiyle doğrulamaları göstereceğim: blog ve gönderi.</span><span class="sxs-lookup"><span data-stu-id="3748a-110">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public DateTime DateCreated { get; set; }
    public string Content { get; set; }
    public int BlogId { get; set; }
    public ICollection<Comment> Comments { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="3748a-111">Veri Açıklamaları</span><span class="sxs-lookup"><span data-stu-id="3748a-111">Data Annotations</span></span>

<span data-ttu-id="3748a-112">Code First, kod ilk sınıflarını yapılandırmanın bir yolu olarak `System.ComponentModel.DataAnnotations` derlemesindeki ek açıklamaları kullanır.</span><span class="sxs-lookup"><span data-stu-id="3748a-112">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="3748a-113">Bu ek açıklamalar arasında `Required`, `MaxLength` ve `MinLength`gibi kurallar sağlayan olanlardır.</span><span class="sxs-lookup"><span data-stu-id="3748a-113">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="3748a-114">Birçok .NET istemci uygulaması da bu ek açıklamaları tanır, örneğin, ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3748a-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="3748a-115">Bu ek açıklamalarla hem istemci tarafı hem de sunucu tarafı doğrulaması elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="3748a-116">Örneğin, blog title özelliğini gerekli bir özellik olacak şekilde zorlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="3748a-117">Uygulamada ek kod veya biçimlendirme değişikliği olmadan, mevcut bir MVC uygulaması, özelliği ve ek açıklama adlarını kullanarak dinamik olarak bir ileti oluşturarak istemci tarafı doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="3748a-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Şekil 1](~/ef6/media/figure01.png)

<span data-ttu-id="3748a-119">Bu oluşturma görünümünün geri gönder yönteminde, yeni blogu veritabanına kaydetmek için Entity Framework kullanılır, ancak uygulama bu koda ulaşmadan önce MVC 'nin istemci tarafı doğrulaması tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="3748a-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="3748a-120">İstemci tarafı doğrulaması, bir madde işareti değildir, ancak.</span><span class="sxs-lookup"><span data-stu-id="3748a-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="3748a-121">Kullanıcılar, tarayıcı özelliklerini etkileyebilir veya daha kötü bir şekilde kötü, Kullanıcı arabirimi doğrulamaları önlemek için bazı karmaşık kery kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="3748a-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="3748a-122">Ancak Entity Framework Ayrıca `Required` ek açıklamayı tanıyacak ve doğrulayacaktır.</span><span class="sxs-lookup"><span data-stu-id="3748a-122">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="3748a-123">Bunu sınamanın basit bir yolu, MVC 'nin istemci tarafı doğrulama özelliğini devre dışı bırakmanız.</span><span class="sxs-lookup"><span data-stu-id="3748a-123">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="3748a-124">Bunu, MVC uygulamasının Web. config dosyasında yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-124">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="3748a-125">AppSettings bölümünde ClientValidationEnabled için bir anahtar bulunur.</span><span class="sxs-lookup"><span data-stu-id="3748a-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="3748a-126">Bu anahtarın false olarak ayarlanması, Kullanıcı arabiriminin doğrulamaları gerçekleştirmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="3748a-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="3748a-127">İstemci tarafı doğrulaması devre dışı bırakılsa bile, uygulamanızda aynı yanıtı alırsınız.</span><span class="sxs-lookup"><span data-stu-id="3748a-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="3748a-128">"Başlık alanı gereklidir" hata iletisi, daha önce olduğu gibi görüntülenecektir.</span><span class="sxs-lookup"><span data-stu-id="3748a-128">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="3748a-129">Bunun dışında, sunucu tarafı doğrulamanın bir sonucu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="3748a-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="3748a-130">Entity Framework, `Required` ek açıklamasında (veritabanına gönderilmek üzere bir `INSERT` komutu oluşturmak için bile, hatta geri dönmezler) ve iletiyi görüntüleyen MVC 'ye hatayı döndürecek şekilde doğrulamayı gerçekleştirecek.</span><span class="sxs-lookup"><span data-stu-id="3748a-130">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3748a-131">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="3748a-131">Fluent API</span></span>

<span data-ttu-id="3748a-132">Aynı istemci tarafı & sunucu tarafı doğrulamasını almak için, kod açıklamaları yerine önce kod Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-132">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="3748a-133">`Required`kullanmak yerine, bunu bir MaxLength doğrulaması kullanarak göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="3748a-133">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="3748a-134">Kod, ilk olarak sınıftan model oluşturuyor olduğundan, akıcı API konfigürasyonları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="3748a-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="3748a-135">DbContext sınıfının ' Onmodeloluþturma metodunu geçersiz kılarak, konfigürasyonları ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-135">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="3748a-136">BloggerName özelliğinin 10 karakterden uzun olmadığını belirten bir yapılandırma aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3748a-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
    }
}
```

<span data-ttu-id="3748a-137">Akıcı API yapılandırmalarına dayalı olarak oluşturulan doğrulama hataları, Kullanıcı arabirimine otomatik olarak ulaşmaz, ancak kodda yakalayıp buna uygun şekilde yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="3748a-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="3748a-138">Entity Framework, uygulamanın BlogController sınıfında, bu doğrulama hatasını yakalayan ve bu, 10 karakterlik en büyük değeri aşan bir BloggerName ile blog kaydetmeye çalıştığında oluşan bazı özel durum işleme hata kodu.</span><span class="sxs-lookup"><span data-stu-id="3748a-138">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

``` csharp
[HttpPost]
public ActionResult Edit(int id, Blog blog)
{
    try
    {
        db.Entry(blog).State = EntityState.Modified;
        db.SaveChanges();
        return RedirectToAction("Index");
    }
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

<span data-ttu-id="3748a-139">Doğrulama, `ModelState.AddModelError` kullanan ek kodun neden kullanıldığından, otomatik olarak görünüme geri geçirilir.</span><span class="sxs-lookup"><span data-stu-id="3748a-139">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="3748a-140">Bu, hata ayrıntılarının görünümü bir görünüme yapmasını sağlar ve ardından hatayı görüntülemek için HtmlHelper `ValidationMessageFor` kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="3748a-140">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="3748a-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="3748a-141">IValidatableObject</span></span>

<span data-ttu-id="3748a-142">`IValidatableObject`, `System.ComponentModel.DataAnnotations`bulunan bir arabirimdir.</span><span class="sxs-lookup"><span data-stu-id="3748a-142">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="3748a-143">Entity Framework API 'sinin bir parçası olmasa da, Entity Framework sınıflarınızda sunucu tarafı doğrulama için bundan yararlanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="3748a-144">`IValidatableObject`, Entity Framework SaveChanges sırasında çağıracak bir `Validate` yöntemi sağlar veya kendi sınıflarını doğrulamak istediğiniz her zaman çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-144">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="3748a-145">`Required` ve `MaxLength` gibi yapılandırmalara tek bir alanda doğrulama işlemi yapılır.</span><span class="sxs-lookup"><span data-stu-id="3748a-145">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="3748a-146">`Validate` yönteminde, örneğin iki alanı karşılaştıran daha karmaşık mantığa sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-146">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="3748a-147">Aşağıdaki örnekte, `Blog` sınıfı `IValidatableObject` uygulamak için genişletilmiştir ve sonra `Title` ve `BloggerName` eşleşemez bir kural sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="3748a-147">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

``` csharp
public class Blog : IValidatableObject
{
    public int Id { get; set; }

    [Required]
    public string Title { get; set; }

    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }

    public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
    {
        if (Title == BloggerName)
        {
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

<span data-ttu-id="3748a-148">`ValidationResult` Oluşturucu, hata iletisini ve doğrulamayla ilişkili üye adlarını temsil eden `string`s dizisini temsil eden bir `string` alır.</span><span class="sxs-lookup"><span data-stu-id="3748a-148">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="3748a-149">Bu doğrulama hem `Title` hem de `BloggerName`denetlediğinden, her iki özellik adı da döndürülür.</span><span class="sxs-lookup"><span data-stu-id="3748a-149">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="3748a-150">Akıcı API tarafından sağlanmış olan doğrulamanın aksine, bu doğrulama sonucu görünüm tarafından tanınacaktır ve daha önce bir hatayı `ModelState` eklemek için kullandığım özel durum işleyicisi gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="3748a-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="3748a-151">`ValidationResult`her iki özellik adını da ayarlayacağım, MVC Htmlyardımcıları bu özelliklerin her ikisi için de hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="3748a-151">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Şekil 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="3748a-153">DbContext. ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="3748a-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="3748a-154">`DbContext`, `ValidateEntity`adlı geçersiz kılınabilir bir yönteme sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3748a-154">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="3748a-155">`SaveChanges`çağırdığınızda Entity Framework, durumu `Unchanged`olan önbelleğindeki her varlık için bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="3748a-155">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="3748a-156">Doğrulama mantığını doğrudan buraya yerleştirebilirsiniz veya bu yöntemi, örneğin, önceki bölümde eklenen `Blog.Validate` yöntemini çağırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-156">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="3748a-157">Aşağıda, posta başlığının henüz kullanılmadığından emin olmak için yeni `Post`s 'yi doğrulayan `ValidateEntity` bir geçersiz kılma örneği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3748a-157">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="3748a-158">İlk olarak varlığın bir gönderi olup olmadığını ve durumunun eklenip eklenmediğini kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="3748a-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="3748a-159">Bu durumda, aynı başlığa sahip bir gönderi olup olmadığını görmek için veritabanına bakar.</span><span class="sxs-lookup"><span data-stu-id="3748a-159">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="3748a-160">Zaten var olan bir gönderi varsa yeni bir `DbEntityValidationResult` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3748a-160">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="3748a-161">tek bir varlık için bir `DbEntityEntry` ve bir `ICollection<DbValidationErrors>` `DbEntityValidationResult`.</span><span class="sxs-lookup"><span data-stu-id="3748a-161">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="3748a-162">Bu yöntemin başlangıcında, bir `DbEntityValidationResult` örneği oluşturulur ve sonra bulunan tüm hatalar `ValidationErrors` koleksiyonuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="3748a-162">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
                        "Post title must be unique."));
        }
    }

    if (result.ValidationErrors.Count > 0)
    {
        return result;
    }
    else
    {
        return base.ValidateEntity(entityEntry, items);
    }
}
```

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="3748a-163">Doğrulamayı açıkça tetikleme</span><span class="sxs-lookup"><span data-stu-id="3748a-163">Explicitly triggering validation</span></span>

<span data-ttu-id="3748a-164">`SaveChanges` çağrısı, bu makalede ele alınan tüm doğrulamaları tetikler.</span><span class="sxs-lookup"><span data-stu-id="3748a-164">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="3748a-165">Ancak `SaveChanges`bilmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3748a-165">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="3748a-166">Uygulamanızda başka bir yerde doğrulama yapmayı tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3748a-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="3748a-167">`DbContext.GetValidationErrors`, ek açıklamalar veya akıcı API tarafından tanımlanan doğrulamaları, `IValidatableObject` oluşturulan doğrulamayı (örneğin, `Blog.Validate`) ve `DbContext.ValidateEntity` yönteminde gerçekleştirilen doğrulamaları tetikleyecektir.</span><span class="sxs-lookup"><span data-stu-id="3748a-167">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="3748a-168">Aşağıdaki kod, `DbContext`geçerli örneğinde `GetValidationErrors` çağıracaktır.</span><span class="sxs-lookup"><span data-stu-id="3748a-168">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="3748a-169">`ValidationErrors` varlık türüne göre `DbEntityValidationResult`.</span><span class="sxs-lookup"><span data-stu-id="3748a-169">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="3748a-170">Kod, ilk olarak yöntemi tarafından döndürülen `DbEntityValidationResult`ve ardından içindeki her `DbValidationError` üzerinden yinelenir.</span><span class="sxs-lookup"><span data-stu-id="3748a-170">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="3748a-171">Doğrulama kullanırken dikkat edilecek diğer noktalar</span><span class="sxs-lookup"><span data-stu-id="3748a-171">Other considerations when using validation</span></span>

<span data-ttu-id="3748a-172">Entity Framework doğrulaması kullanırken göz önünde bulundurmanız gereken birkaç farklı noktası aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3748a-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="3748a-173">Doğrulama sırasında yavaş yükleme devre dışı bırakıldı</span><span class="sxs-lookup"><span data-stu-id="3748a-173">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="3748a-174">EF, eşlenmemiş özelliklerde veri açıklamalarını doğrular (veritabanındaki bir sütunla eşlenmemiş Özellikler)</span><span class="sxs-lookup"><span data-stu-id="3748a-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="3748a-175">`SaveChanges`sırasında değişiklikler saptandıktan sonra doğrulama gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="3748a-175">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="3748a-176">Doğrulama sırasında değişiklik yaparsanız değişiklik izleyicinize bildirimde bulunan sorumluluk sizin sorumluluğunuzdadır</span><span class="sxs-lookup"><span data-stu-id="3748a-176">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="3748a-177">doğrulama sırasında hata oluşursa `DbUnexpectedValidationException` atılır</span><span class="sxs-lookup"><span data-stu-id="3748a-177">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="3748a-178">Entity Framework model (maksimum uzunluk, gerekli, vb.), sınıflarınızda hiç bir veri ek açıklaması olmasa bile doğrulamaya neden olur ve/veya modelinizi oluşturmak için EF tasarımcısını kullandıysanız</span><span class="sxs-lookup"><span data-stu-id="3748a-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="3748a-179">Öncelik kuralları:</span><span class="sxs-lookup"><span data-stu-id="3748a-179">Precedence rules:</span></span>
  - <span data-ttu-id="3748a-180">Akıcı API çağrıları karşılık gelen veri açıklamalarını geçersiz kılar</span><span class="sxs-lookup"><span data-stu-id="3748a-180">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="3748a-181">Yürütme sırası:</span><span class="sxs-lookup"><span data-stu-id="3748a-181">Execution order:</span></span>
  - <span data-ttu-id="3748a-182">Özellik doğrulaması tür doğrulamasından önce oluşur</span><span class="sxs-lookup"><span data-stu-id="3748a-182">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="3748a-183">Tür doğrulaması yalnızca özellik doğrulaması başarılı olursa gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="3748a-183">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="3748a-184">Bir özellik karmaşıksa, doğrulama işlemi de şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="3748a-184">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="3748a-185">Karmaşık tür özelliklerinde özellik düzeyinde doğrulama</span><span class="sxs-lookup"><span data-stu-id="3748a-185">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="3748a-186">Karmaşık tür üzerinde `IValidatableObject` doğrulaması da dahil olmak üzere, karmaşık türde düzey doğrulaması yazın</span><span class="sxs-lookup"><span data-stu-id="3748a-186">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="3748a-187">Özet</span><span class="sxs-lookup"><span data-stu-id="3748a-187">Summary</span></span>

<span data-ttu-id="3748a-188">Entity Framework 'deki doğrulama API 'SI MVC 'de istemci tarafı doğrulaması ile çok daha fazla oynatılır, ancak istemci tarafı doğrulamaya güvenmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3748a-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="3748a-189">Entity Framework, kod ilk akıcı API ile uyguladığınız veri açıklamaları veya yapılandırmaların sunucu tarafında doğrulama işlemini ele alır.</span><span class="sxs-lookup"><span data-stu-id="3748a-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="3748a-190">Ayrıca, `IValidatableObject` arabirimini kullanıp `DbContext.ValidateEntity` yöntemine dokunarak davranışı özelleştirmek için birçok genişletilebilirlik noktası da gördünüz.</span><span class="sxs-lookup"><span data-stu-id="3748a-190">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="3748a-191">Ve bu son iki yol `DbContext`aracılığıyla, kavramsal modelinizi anlatmak için Code First, Model First veya Database First iş akışını kullanmanıza bakılmaksızın kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3748a-191">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
