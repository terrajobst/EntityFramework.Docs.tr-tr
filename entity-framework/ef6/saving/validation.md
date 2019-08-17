---
title: Doğrulama-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 457af0c32f0fe4804dbfe6e348664efb1af517c9
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565369"
---
# <a name="data-validation"></a><span data-ttu-id="47213-102">Veri doğrulama</span><span class="sxs-lookup"><span data-stu-id="47213-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="47213-103">**EF 4.1 yalnızca** sonraki sürümler-bu sayfada açıklanan özellikler, API 'ler vb. Entity Framework 4,1 ' de tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="47213-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="47213-104">Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz</span><span class="sxs-lookup"><span data-stu-id="47213-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="47213-105">Bu sayfadaki içerik, özgün olarak Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)) tarafından yazılmış bir makaleden uyarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="47213-105">The content on this page is adapted from an article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="47213-106">Entity Framework, istemci tarafı doğrulama için bir kullanıcı arabirimine veya sunucu tarafı doğrulaması için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="47213-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="47213-107">Önce kodu kullanırken, daha fazla açıklama veya Fluent API yapılandırma kullanarak doğrulama belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="47213-108">Kod içinde ek doğrulamalar ve daha karmaşık bir şekilde belirlenebilir ve önce, modelinizde önce koddan, modelden önce veya önce modelinizden çekip önlenebilir.</span><span class="sxs-lookup"><span data-stu-id="47213-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="47213-109">Model</span><span class="sxs-lookup"><span data-stu-id="47213-109">The model</span></span>

<span data-ttu-id="47213-110">Basit bir sınıf çiftiyle doğrulamaları göstereceğim: Blog ve gönderi.</span><span class="sxs-lookup"><span data-stu-id="47213-110">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="47213-111">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="47213-111">Data Annotations</span></span>

<span data-ttu-id="47213-112">Code First, `System.ComponentModel.DataAnnotations` kod ilk sınıflarını yapılandırmanın bir yolu olarak derlemeden ek açıklamaları kullanır.</span><span class="sxs-lookup"><span data-stu-id="47213-112">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="47213-113">Bu ek açıklamalar arasında `Required`, `MaxLength` ve `MinLength`gibi kurallar sağlayan olanlardır.</span><span class="sxs-lookup"><span data-stu-id="47213-113">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="47213-114">Birçok .NET istemci uygulaması da bu ek açıklamaları tanır, örneğin, ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="47213-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="47213-115">Bu ek açıklamalarla hem istemci tarafı hem de sunucu tarafı doğrulaması elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="47213-116">Örneğin, blog title özelliğini gerekli bir özellik olacak şekilde zorlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="47213-117">Uygulamada ek kod veya biçimlendirme değişikliği olmadan, mevcut bir MVC uygulaması, özelliği ve ek açıklama adlarını kullanarak dinamik olarak bir ileti oluşturarak istemci tarafı doğrulaması gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="47213-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Şekil 1](~/ef6/media/figure01.png)

<span data-ttu-id="47213-119">Bu oluşturma görünümünün geri gönder yönteminde, yeni blogu veritabanına kaydetmek için Entity Framework kullanılır, ancak uygulama bu koda ulaşmadan önce MVC 'nin istemci tarafı doğrulaması tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="47213-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="47213-120">İstemci tarafı doğrulaması, bir madde işareti değildir, ancak.</span><span class="sxs-lookup"><span data-stu-id="47213-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="47213-121">Kullanıcılar, tarayıcı özelliklerini etkileyebilir veya daha kötü bir şekilde kötü, Kullanıcı arabirimi doğrulamaları önlemek için bazı karmaşık kery kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="47213-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="47213-122">Ancak Entity Framework `Required` ek açıklamayı da tanıyacak ve doğrulayacaktır.</span><span class="sxs-lookup"><span data-stu-id="47213-122">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="47213-123">Bunu sınamanın basit bir yolu, MVC 'nin istemci tarafı doğrulama özelliğini devre dışı bırakmanız.</span><span class="sxs-lookup"><span data-stu-id="47213-123">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="47213-124">Bunu, MVC uygulamasının Web. config dosyasında yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-124">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="47213-125">AppSettings bölümünde ClientValidationEnabled için bir anahtar bulunur.</span><span class="sxs-lookup"><span data-stu-id="47213-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="47213-126">Bu anahtarın false olarak ayarlanması, Kullanıcı arabiriminin doğrulamaları gerçekleştirmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="47213-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="47213-127">İstemci tarafı doğrulaması devre dışı bırakılsa bile, uygulamanızda aynı yanıtı alırsınız.</span><span class="sxs-lookup"><span data-stu-id="47213-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="47213-128">"Başlık alanı gereklidir" hata iletisi, daha önce olduğu gibi görüntülenecektir.</span><span class="sxs-lookup"><span data-stu-id="47213-128">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="47213-129">Bunun dışında, sunucu tarafı doğrulamanın bir sonucu olacaktır.</span><span class="sxs-lookup"><span data-stu-id="47213-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="47213-130">Entity Framework, `Required` ek açıklama üzerinde (veritabanına gönderilmek üzere bir `INSERT` komut oluşturmak için bile, hatta geri dönmezler) ve iletiyi görüntüleyen MVC 'ye hatayı döndürecek şekilde doğrulama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="47213-130">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="47213-131">Akıcı API</span><span class="sxs-lookup"><span data-stu-id="47213-131">Fluent API</span></span>

<span data-ttu-id="47213-132">Aynı istemci tarafı & sunucu tarafı doğrulamasını almak için, kod açıklamaları yerine önce kod Fluent API kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-132">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="47213-133">Kullanmak `Required`yerine, bunu bir MaxLength doğrulaması kullanarak göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="47213-133">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="47213-134">Kod, ilk olarak sınıftan model oluşturuyor olduğundan, akıcı API konfigürasyonları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="47213-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="47213-135">DbContext sınıfının ' Onmodeloluþturma metodunu geçersiz kılarak, konfigürasyonları ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-135">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="47213-136">BloggerName özelliğinin 10 karakterden uzun olmadığını belirten bir yapılandırma aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="47213-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="47213-137">Akıcı API yapılandırmalarına dayalı olarak oluşturulan doğrulama hataları, Kullanıcı arabirimine otomatik olarak ulaşmaz, ancak kodda yakalayıp buna uygun şekilde yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="47213-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="47213-138">Entity Framework, uygulamanın BlogController sınıfında, bu doğrulama hatasını yakalayan ve bu, 10 karakterlik en büyük değeri aşan bir BloggerName ile blog kaydetmeye çalıştığında oluşan bazı özel durum işleme hata kodu.</span><span class="sxs-lookup"><span data-stu-id="47213-138">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="47213-139">Doğrulama, tarafından `ModelState.AddModelError` kullanılan ek kodun neden kullanıldığı, otomatik olarak görünüme geri geçirilmiyor.</span><span class="sxs-lookup"><span data-stu-id="47213-139">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="47213-140">Bu, hata ayrıntılarının görünümü bir görünüme yapmasını sağlar ve ardından hatayı görüntülemek için `ValidationMessageFor` HtmlHelper 'ı kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="47213-140">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="47213-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="47213-141">IValidatableObject</span></span>

<span data-ttu-id="47213-142">`IValidatableObject`, içinde bulunan `System.ComponentModel.DataAnnotations`bir arabirimdir.</span><span class="sxs-lookup"><span data-stu-id="47213-142">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="47213-143">Entity Framework API 'sinin bir parçası olmasa da, Entity Framework sınıflarınızda sunucu tarafı doğrulama için bundan yararlanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="47213-144">`IValidatableObject`Entity Framework SaveChanges `Validate` sırasında çağıracağını belirten bir yöntem sağlar veya kendi sınıflarını doğrulamak istediğiniz zaman çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-144">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="47213-145">Ve gibi yapılandırma ve `MaxLength` tek bir alanda doğrulama gerçekleştirme. `Required`</span><span class="sxs-lookup"><span data-stu-id="47213-145">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="47213-146">`Validate` Yönteminde, örneğin iki alanı karşılaştıran daha karmaşık mantığa sahip olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-146">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="47213-147">Aşağıdaki örnekte, `Blog` sınıfı uygulamak `IValidatableObject` için genişletilmiştir, `Title` ve `BloggerName` eşleşmemiş bir kural sağlayın.</span><span class="sxs-lookup"><span data-stu-id="47213-147">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

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

<span data-ttu-id="47213-148">Oluşturucu, `string` hata`string`iletisini ve doğrulamayla ilişkili üye adlarını temsil eden bir dizi öğeleri temsil eden bir kullanır. `ValidationResult`</span><span class="sxs-lookup"><span data-stu-id="47213-148">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="47213-149">Bu doğrulama hem `Title` `BloggerName`hem de ' yi denetlediğinde, her iki özellik adı da döndürülür.</span><span class="sxs-lookup"><span data-stu-id="47213-149">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="47213-150">Akıcı API tarafından belirtilen doğrulamanın aksine, bu doğrulama sonucu görünüm tarafından tanınacaktır ve hatayı `ModelState` eklemek için daha önce kullandığım özel durum işleyicisi gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="47213-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="47213-151">İçinde her iki özellik adını da `ValidationResult`ayarlayacağım, MVC htmlyardımcıları bu özelliklerden her ikisi için de hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="47213-151">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Şekil 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="47213-153">DbContext. ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="47213-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="47213-154">`DbContext`adlı `ValidateEntity`geçersiz kılınabilir bir yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="47213-154">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="47213-155">' İ çağırdığınızda `SaveChanges`Entity Framework, durumu olmayan `Unchanged`önbelleğindeki her varlık için bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="47213-155">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="47213-156">Doğrulama mantığını doğrudan buraya yerleştirebilir veya bu yöntemi, örneğin `Blog.Validate` önceki bölüme eklenen yöntemi çağırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-156">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="47213-157">Aşağıda, posta başlığının henüz kullanılmadığından `ValidateEntity` emin olmak için yeni `Post`s 'yi doğrulayan bir geçersiz kılma örneği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="47213-157">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="47213-158">İlk olarak varlığın bir gönderi olup olmadığını ve durumunun eklenip eklenmediğini kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="47213-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="47213-159">Bu durumda, aynı başlığa sahip bir gönderi olup olmadığını görmek için veritabanına bakar.</span><span class="sxs-lookup"><span data-stu-id="47213-159">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="47213-160">Zaten var olan bir gönderi varsa yeni `DbEntityValidationResult` bir oluşturma oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="47213-160">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="47213-161">`DbEntityValidationResult`tek bir `DbEntityEntry` varlık `ICollection<DbValidationErrors>` için bir ve bir barındırır.</span><span class="sxs-lookup"><span data-stu-id="47213-161">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="47213-162">Bu yöntemin başlangıcında, bir `DbEntityValidationResult` örneği oluşturulur ve sonra bulunan tüm hatalar `ValidationErrors` koleksiyonuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="47213-162">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="47213-163">Doğrulamayı açıkça tetikleme</span><span class="sxs-lookup"><span data-stu-id="47213-163">Explicitly triggering validation</span></span>

<span data-ttu-id="47213-164">' A çağrı `SaveChanges` , bu makalede ele alınan tüm doğrulamaları tetikler.</span><span class="sxs-lookup"><span data-stu-id="47213-164">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="47213-165">Ancak uygulamanız `SaveChanges`gerekmez.</span><span class="sxs-lookup"><span data-stu-id="47213-165">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="47213-166">Uygulamanızda başka bir yerde doğrulama yapmayı tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="47213-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="47213-167">`DbContext.GetValidationErrors`, ek açıklamalar veya akıcı API tarafından tanımlanan doğrulamaları, içinde `IValidatableObject` oluşturulan doğrulamayı (örneğin, `Blog.Validate`) ve `DbContext.ValidateEntity` yöntemde gerçekleştirilen doğrulamaları tetikler.</span><span class="sxs-lookup"><span data-stu-id="47213-167">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="47213-168">Aşağıdaki kod, geçerli bir `GetValidationErrors` `DbContext`örneği üzerinde çağracaktır.</span><span class="sxs-lookup"><span data-stu-id="47213-168">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="47213-169">`ValidationErrors`, varlık türüne `DbEntityValidationResult`göre gruplandırılır.</span><span class="sxs-lookup"><span data-stu-id="47213-169">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="47213-170">Kod, ilk `DbEntityValidationResult`olarak yöntemi tarafından döndürülen öğeleri ve sonra her `DbValidationError` bir içinde yinelenir.</span><span class="sxs-lookup"><span data-stu-id="47213-170">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="47213-171">Doğrulama kullanırken dikkat edilecek diğer noktalar</span><span class="sxs-lookup"><span data-stu-id="47213-171">Other considerations when using validation</span></span>

<span data-ttu-id="47213-172">Entity Framework doğrulaması kullanırken göz önünde bulundurmanız gereken birkaç farklı noktası aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="47213-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="47213-173">Doğrulama sırasında yavaş yükleme devre dışı bırakıldı</span><span class="sxs-lookup"><span data-stu-id="47213-173">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="47213-174">EF, eşlenmemiş özelliklerde veri açıklamalarını doğrular (veritabanındaki bir sütunla eşlenmemiş Özellikler)</span><span class="sxs-lookup"><span data-stu-id="47213-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="47213-175">Doğrulama, sırasında `SaveChanges`değişiklikler saptandıktan sonra gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="47213-175">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="47213-176">Doğrulama sırasında değişiklik yaparsanız değişiklik izleyicinize bildirimde bulunan sorumluluk sizin sorumluluğunuzdadır</span><span class="sxs-lookup"><span data-stu-id="47213-176">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="47213-177">`DbUnexpectedValidationException`doğrulama sırasında hata oluşursa oluşturulur</span><span class="sxs-lookup"><span data-stu-id="47213-177">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="47213-178">Entity Framework model (maksimum uzunluk, gerekli, vb.), sınıflarınızda hiç bir veri ek açıklaması olmasa bile doğrulamaya neden olur ve/veya modelinizi oluşturmak için EF tasarımcısını kullandıysanız</span><span class="sxs-lookup"><span data-stu-id="47213-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="47213-179">Öncelik kuralları:</span><span class="sxs-lookup"><span data-stu-id="47213-179">Precedence rules:</span></span>
  - <span data-ttu-id="47213-180">Akıcı API çağrıları karşılık gelen veri açıklamalarını geçersiz kılar</span><span class="sxs-lookup"><span data-stu-id="47213-180">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="47213-181">Yürütme sırası:</span><span class="sxs-lookup"><span data-stu-id="47213-181">Execution order:</span></span>
  - <span data-ttu-id="47213-182">Özellik doğrulaması tür doğrulamasından önce oluşur</span><span class="sxs-lookup"><span data-stu-id="47213-182">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="47213-183">Tür doğrulaması yalnızca özellik doğrulaması başarılı olursa gerçekleşir</span><span class="sxs-lookup"><span data-stu-id="47213-183">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="47213-184">Bir özellik karmaşıksa, doğrulama işlemi de şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="47213-184">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="47213-185">Karmaşık tür özelliklerinde özellik düzeyinde doğrulama</span><span class="sxs-lookup"><span data-stu-id="47213-185">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="47213-186">Karmaşık tür üzerinde doğrulama da dahil olmak üzere `IValidatableObject` karmaşık türde tür düzeyinde doğrulama</span><span class="sxs-lookup"><span data-stu-id="47213-186">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="47213-187">Özet</span><span class="sxs-lookup"><span data-stu-id="47213-187">Summary</span></span>

<span data-ttu-id="47213-188">Entity Framework 'deki doğrulama API 'SI MVC 'de istemci tarafı doğrulaması ile çok daha fazla oynatılır, ancak istemci tarafı doğrulamaya güvenmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="47213-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="47213-189">Entity Framework, kod ilk akıcı API ile uyguladığınız veri açıklamaları veya yapılandırmaların sunucu tarafında doğrulama işlemini ele alır.</span><span class="sxs-lookup"><span data-stu-id="47213-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="47213-190">Ayrıca, `IValidatableObject` arabirimini kullanırken veya `DbContext.ValidateEntity` yöntemine dokunarak davranışı özelleştirmek için birçok genişletilebilirlik noktası da gördünüz.</span><span class="sxs-lookup"><span data-stu-id="47213-190">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="47213-191">Ve bu son iki, doğrulama Code First, model First veya Database First `DbContext`iş akışını, kavramsal modelinizi anlatmak üzere kullanıp kullandığına göre kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="47213-191">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
