---
title: Doğrulama - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 65639b0f91f54ee2cd1336f6b6cd4caf45ede680
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251030"
---
# <a name="data-validation"></a><span data-ttu-id="b0346-102">Veri doğrulama</span><span class="sxs-lookup"><span data-stu-id="b0346-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="b0346-103">**EF4.1 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 4.1 içinde kullanıma sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="b0346-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="b0346-104">Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri uygulanmaz</span><span class="sxs-lookup"><span data-stu-id="b0346-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="b0346-105">Bu sayfadaki içeriğin gelen uyarlanmış ve Julie Lerman tarafından başlangıçta yazılı makalesi ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="b0346-105">The content on this page is adapted from and article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="b0346-106">Entity Framework ile istemci tarafı doğrulama için bir kullanıcı arabirimi için besleyebilecek veya sunucu tarafı doğrulama için kullanılan doğrulama özellikleri çok çeşitli sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0346-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="b0346-107">Kod ilk kez kullanırken, ek açıklama ya da fluent API'sini yapılandırmaları kullanarak doğrulamaları belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0346-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="b0346-108">Ek doğrulama ve daha karmaşık kod içinde belirtilebilir ve modelinizi koddan önce hails olmadığını çalışacak ilk model veya ilk veritabanı.</span><span class="sxs-lookup"><span data-stu-id="b0346-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="b0346-109">Model</span><span class="sxs-lookup"><span data-stu-id="b0346-109">The model</span></span>

<span data-ttu-id="b0346-110">Sınıfları basit çiftiyle doğrulamaları kazandırabileceğinizi göstereceğiz: Blog ve gönderi.</span><span class="sxs-lookup"><span data-stu-id="b0346-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
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
## <a name="data-annotations"></a><span data-ttu-id="b0346-111">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="b0346-111">Data Annotations</span></span>

<span data-ttu-id="b0346-112">Kod ek açıklamaları System.ComponentModel.DataAnnotations bütünleştirilmiş koddan kod ilk sınıfları yapılandırma bir anlamına gelir ilk kullanır.</span><span class="sxs-lookup"><span data-stu-id="b0346-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="b0346-113">Bu ek açıklamalar arasında gerekli MinLength ve MaxLength gibi kuralları sağlayan bağlantılardır.</span><span class="sxs-lookup"><span data-stu-id="b0346-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="b0346-114">.NET istemci uygulama sayısı Ayrıca bu ek açıklamaları, örneğin, ASP.NET MVC tanır.</span><span class="sxs-lookup"><span data-stu-id="b0346-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="b0346-115">Hem istemci tarafı ve sunucu tarafı doğrulama bu ek açıklamalar ile elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0346-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="b0346-116">Örneğin, Blog başlık özelliği gerekli bir özellik olacak şekilde zorlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0346-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="b0346-117">Hiçbir ek kod veya biçimlendirme değişikliklerini uygulama, var olan bir MVC uygulaması dinamik olarak bile özellik ve ek açıklama adları kullanarak bir ileti oluşturma, istemci tarafı doğrulama gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="b0346-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Şekil 1](~/ef6/media/figure01.png)

<span data-ttu-id="b0346-119">İletide back yöntemi bu Oluştur görünümünün, Entity Framework, yeni blog veritabanına kaydetmek için kullanılır, ancak uygulama kodu ulaşmadan önce MVC'ın istemci tarafı doğrulama tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="b0346-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="b0346-120">İstemci tarafı doğrulama ancak madde işareti kavram değildir.</span><span class="sxs-lookup"><span data-stu-id="b0346-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="b0346-121">Kullanıcılar tarayıcılarını özelliklerinin etkileyebilir veya daha da kötüsü henüz bir bilgisayar korsanının bazı aldatmaca UI doğrulamaları önlemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0346-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="b0346-122">Ancak Entity Framework ayrıca gerekli ek açıklama tanıması ve doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="b0346-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="b0346-123">Bunu test etmek için basit bir yol, MVC'ın istemci tarafı doğrulama özelliği devre dışı bırakmaktır.</span><span class="sxs-lookup"><span data-stu-id="b0346-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="b0346-124">MVC uygulamanın web.config dosyasında, bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0346-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="b0346-125">AppSettings bölümünde ClientValidationEnabled için bir anahtarı yok.</span><span class="sxs-lookup"><span data-stu-id="b0346-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="b0346-126">Bu anahtarı false olarak ayarlandığında, UI doğrulamaları gerçekleştirmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="b0346-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="b0346-127">Devre dışı bile istemci tarafı doğrulama ile uygulamanızda aynı yanıtı alırsınız.</span><span class="sxs-lookup"><span data-stu-id="b0346-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="b0346-128">Hata iletisi "Başlık alanı gerekiyor" olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="b0346-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="b0346-129">Artık sunucu tarafı doğrulama sonucu olacak dışında.</span><span class="sxs-lookup"><span data-stu-id="b0346-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="b0346-130">Entity Framework gerçekleştirecek doğrulama gerekli ek açıklamayı (Bu da derleme ve veritabanına göndermek için INSERT komutu rahatsız önce) ve hata iletisini görüntüler için MVC döndürür.</span><span class="sxs-lookup"><span data-stu-id="b0346-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b0346-131">Fluent API'si</span><span class="sxs-lookup"><span data-stu-id="b0346-131">Fluent API</span></span>

<span data-ttu-id="b0346-132">Kod kullandığınız aynı istemci tarafı ve sunucu almak için ilk'ın fluent API'si ek açıklamaları yerine tarafında doğrulama.</span><span class="sxs-lookup"><span data-stu-id="b0346-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="b0346-133">Yerine gerekli kullanımı, ı, MaxLength doğrulama kullanarak bu göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="b0346-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="b0346-134">Kod ilk modeli sınıfları oluşturma gibi Fluent API'si yapılandırmaları uygulanır.</span><span class="sxs-lookup"><span data-stu-id="b0346-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="b0346-135">DbContext sınıfı OnModelCreating yöntemini geçersiz kılarak yapılandırmaları ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="b0346-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="b0346-136">BloggerName özelliği 10 karakterden daha uzun olabileceğini belirten yapılandırması aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b0346-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="b0346-137">Doğrulama hataları durum Fluent API'si yapılandırmalarına göre otomatik olarak ulaşma kullanıcı Arabirimi, ancak bu kodu ve ardından yanıt uygun şekilde yakalamaz.</span><span class="sxs-lookup"><span data-stu-id="b0346-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="b0346-138">Entity Framework 10 karakter üst sınırını aşıyor bir BloggerName ile blog kaydetmeye çalıştığında doğrulama hata yakalayan uygulamanın BlogController sınıfı hata kodunu aşağıda verilmiştir; bazı özel durum işleme.</span><span class="sxs-lookup"><span data-stu-id="b0346-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

<span data-ttu-id="b0346-139">Doğrulama otomatik olarak geri ModelState.AddModelError kullanan ek kod kullanılan neden olan görünümün geçirilen değil.</span><span class="sxs-lookup"><span data-stu-id="b0346-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="b0346-140">Bu, hata ayrıntılarını, ardından hata görüntülenecek ValidationMessageFor Htmlhelper kullanacağı görünümde yapmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0346-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="b0346-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="b0346-141">IValidatableObject</span></span>

<span data-ttu-id="b0346-142">IValidatableObject System.ComponentModel.DataAnnotations içinde yer alan bir arabirimdir.</span><span class="sxs-lookup"><span data-stu-id="b0346-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="b0346-143">Entity Framework API parçası değildir ancak yine, sunucu tarafı doğrulama için Entity Framework sınıflarınızdaki yararlanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0346-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="b0346-144">Entity Framework SaveChanges veya sırasında çağıracak bir doğrulama yöntemi sınıfları doğrulamak için istediğiniz zaman kendiniz çağırabilirsiniz IValidatableObject sağlar.</span><span class="sxs-lookup"><span data-stu-id="b0346-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="b0346-145">Gibi gerekli yapılandırmaları ve MaxLength validaton tek bir alan üzerinde gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="b0346-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="b0346-146">Doğrulama yöntemi, daha karmaşık mantık iki alan karşılaştırma olabilir.</span><span class="sxs-lookup"><span data-stu-id="b0346-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="b0346-147">Aşağıdaki örnekte, Blog sınıfı IValidatableObject uygulayın ve ardından başlık ve BloggerName eşleşemez bir kural sağlamak için genişletilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b0346-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

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
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

<span data-ttu-id="b0346-148">ValidationResult oluşturucusu, hata iletisi ve doğrulama ile ilişkili olan üye adlarından temsil eden bir dize dizisi temsil eden bir dize alır.</span><span class="sxs-lookup"><span data-stu-id="b0346-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="b0346-149">Bu doğrulama başlığı hem BloggerName denetler olduğundan, her iki özellik adları döndürülür.</span><span class="sxs-lookup"><span data-stu-id="b0346-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="b0346-150">Fluent API'si tarafından sağlanan doğrulama aksine bu doğrulama sonucu görünüm tarafından tanınır ve ben daha önce ModelState ekleme hatası kullanılacak özel durum işleyicisi gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="b0346-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="b0346-151">Her iki özellik adları ValidationResult ayarlandığından, MVC HtmlHelpers hem de bu özellikler hata iletisini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="b0346-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Şekil 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="b0346-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="b0346-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="b0346-154">DbContext ValidateEntity adlı geçersiz kılınabilir bir yöntemi vardır.</span><span class="sxs-lookup"><span data-stu-id="b0346-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="b0346-155">SaveChanges çağırdığınızda, Entity Framework bu yöntemi her varlık durumu değişmeden değil, önbellek için çağırır.</span><span class="sxs-lookup"><span data-stu-id="b0346-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="b0346-156">Örneğin, önceki bölümde eklediğiniz Blog.Validate yöntemini çağırmak için bu yöntem doğrudan ve hatta burada kullanımda Doğrulama mantığı koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0346-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="b0346-157">Posta başlık zaten kullanılmadığını emin olmak için yeni gönderileri onaylar ValidateEntity bir geçersiz kılma örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="b0346-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="b0346-158">İlk durumuna eklenir ve varlık bir gönderi olup olmadığını görmek için denetler.</span><span class="sxs-lookup"><span data-stu-id="b0346-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="b0346-159">Ardından bu durumda, veritabanında zaten bir gönderi başlığıyla aynı olup olmadığını arar.</span><span class="sxs-lookup"><span data-stu-id="b0346-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="b0346-160">Daha sonra postayı zaten varsa, yeni DbEntityValidationResult oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b0346-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="b0346-161">Bir DbEntityEntry ve tek bir varlık için ICollection, DbValidationErrors DbEntityValidationResult barındırır.</span><span class="sxs-lookup"><span data-stu-id="b0346-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="b0346-162">Bu yöntemin başında bir DbEntityValidationResult örneği ve sonra bulunan herhangi bir hata, ValidationErrors koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="b0346-162">At the start of this method, a  DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="b0346-163">Açıkça doğrulama tetikleme</span><span class="sxs-lookup"><span data-stu-id="b0346-163">Explicitly triggering validation</span></span>

<span data-ttu-id="b0346-164">Bu makalede ele alınan doğrulamaları tüm SaveChanges çağrısı tetikler.</span><span class="sxs-lookup"><span data-stu-id="b0346-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="b0346-165">Ancak, üzerinde SaveChanges yararlanmayı gerekmez.</span><span class="sxs-lookup"><span data-stu-id="b0346-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="b0346-166">Başka bir yerde uygulamanızda doğrulamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0346-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="b0346-167">Tüm doğrulamaları, ek açıklamalar ya da Fluent API'si tarafından tanımlanan, doğrulama IValidatableObject (örneğin, Blog.Validate) içinde oluşturulan ve DbContext.ValidateEntity içinde gerçekleştirilen doğrulamaları DbContext.GetValidationErrors tetikler yöntem.</span><span class="sxs-lookup"><span data-stu-id="b0346-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="b0346-168">Aşağıdaki kod geçerli bir DbContext örneğinde GetValidationErrors çağırır.</span><span class="sxs-lookup"><span data-stu-id="b0346-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="b0346-169">ValidationErrors DbValidationRestuls varlık türüne göre gruplandırılır.</span><span class="sxs-lookup"><span data-stu-id="b0346-169">ValidationErrors are grouped by entity type into DbValidationRestuls.</span></span> <span data-ttu-id="b0346-170">Kod ilk yöntem tarafından döndürülen DbValidationResults ve ardından her ValidationError içinde aracılığıyla yinelenir.</span><span class="sxs-lookup"><span data-stu-id="b0346-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="b0346-171">Doğrulama kullanmayla ilgili diğer konular</span><span class="sxs-lookup"><span data-stu-id="b0346-171">Other considerations when using validation</span></span>

<span data-ttu-id="b0346-172">Entity Framework doğrulama kullanırken dikkate alınması gereken bazı noktalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b0346-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="b0346-173">Yavaş yükleme sırasında doğrulama devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="b0346-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="b0346-174">EF eşlenen özelliklerini (veritabanı sütununa eşlenmemiş) veri ek açıklamalarını doğrular.</span><span class="sxs-lookup"><span data-stu-id="b0346-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="b0346-175">Değişiklikleri sırasında SaveChanges algılandıktan sonra doğrulama gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="b0346-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="b0346-176">Doğrulama sırasında değişiklik yaparsanız, değişiklik İzleyici bildirmek için sizin sorumluluğunuzdur.</span><span class="sxs-lookup"><span data-stu-id="b0346-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="b0346-177">Doğrulama sırasında bir hata oluşursa DbUnexpectedValidationException oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b0346-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="b0346-178">Entity Framework modelini (gereken maksimum uzunluktan, vb.) içeren modellerin sınıflarınızı veri ek açıklamalarını değildir ve/veya EF Designer modelinizi oluşturmak için kullanılan olsa bile, doğrulama, neden olur.</span><span class="sxs-lookup"><span data-stu-id="b0346-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="b0346-179">Öncelik kuralları:</span><span class="sxs-lookup"><span data-stu-id="b0346-179">Precedence rules:</span></span>
    -   <span data-ttu-id="b0346-180">Fluent API çağrılarının karşılık gelen veri ek açıklamalarını geçersiz kıl</span><span class="sxs-lookup"><span data-stu-id="b0346-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="b0346-181">Yürütme sırası:</span><span class="sxs-lookup"><span data-stu-id="b0346-181">Execution order:</span></span>
    -   <span data-ttu-id="b0346-182">Özellik doğrulamasını tür doğrulaması önce gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="b0346-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="b0346-183">Özelliği doğrulama başarılı olursa, tür doğrulaması yalnızca oluşur</span><span class="sxs-lookup"><span data-stu-id="b0346-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="b0346-184">Karmaşık bir özellik varsa, doğrulama de içerir:</span><span class="sxs-lookup"><span data-stu-id="b0346-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="b0346-185">Karmaşık tür özellikleri özellik düzeyi doğrulama</span><span class="sxs-lookup"><span data-stu-id="b0346-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="b0346-186">Karmaşık tür IValidatableObject doğrulama dahil olmak üzere, karmaşık tür düzeyinde doğrulama türü</span><span class="sxs-lookup"><span data-stu-id="b0346-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="b0346-187">Özet</span><span class="sxs-lookup"><span data-stu-id="b0346-187">Summary</span></span>

<span data-ttu-id="b0346-188">İstemci tarafı doğrulama MVC ile Entity Framework API doğrulama çok düzgün bir şekilde yürütülür, ancak istemci tarafı doğrulamasını güvenmek zorunda değilsiniz.</span><span class="sxs-lookup"><span data-stu-id="b0346-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="b0346-189">Entity Framework DataAnnotations ya da kod ilk Fluent API'si ile uyguladığınız yapılandırmaları için sunucu tarafında doğrulama ölçeklendirilmesini.</span><span class="sxs-lookup"><span data-stu-id="b0346-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="b0346-190">Ayrıca, genişletilebilirlik noktaları IValidatableObject arabirimini kullanın ya da dokunun DbContet.ValidateEntity yöntemin davranışını özelleştirmek için birçok de gördünüz.</span><span class="sxs-lookup"><span data-stu-id="b0346-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="b0346-191">Ve Code First, Model ilk ya da veritabanı ilk iş akışının kavramsal model tanımlamak için kullandığınız olup olmadığını doğrulama bu son iki çeşit DbContext kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b0346-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
