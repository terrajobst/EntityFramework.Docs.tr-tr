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
# <a name="data-validation"></a>Veri doğrulama
> [!NOTE]
> **EF4.1 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 4.1 içinde kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri uygulanmaz

Bu sayfadaki içeriğin gelen uyarlanmış ve Julie Lerman tarafından başlangıçta yazılı makalesi ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework ile istemci tarafı doğrulama için bir kullanıcı arabirimi için besleyebilecek veya sunucu tarafı doğrulama için kullanılan doğrulama özellikleri çok çeşitli sağlar. Kod ilk kez kullanırken, ek açıklama ya da fluent API'sini yapılandırmaları kullanarak doğrulamaları belirtebilirsiniz. Ek doğrulama ve daha karmaşık kod içinde belirtilebilir ve modelinizi koddan önce hails olmadığını çalışacak ilk model veya ilk veritabanı.

## <a name="the-model"></a>Model

Sınıfları basit çiftiyle doğrulamaları kazandırabileceğinizi göstereceğiz: Blog ve gönderi.

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
## <a name="data-annotations"></a>Veri ek açıklamaları

Kod ek açıklamaları System.ComponentModel.DataAnnotations bütünleştirilmiş koddan kod ilk sınıfları yapılandırma bir anlamına gelir ilk kullanır. Bu ek açıklamalar arasında gerekli MinLength ve MaxLength gibi kuralları sağlayan bağlantılardır. .NET istemci uygulama sayısı Ayrıca bu ek açıklamaları, örneğin, ASP.NET MVC tanır. Hem istemci tarafı ve sunucu tarafı doğrulama bu ek açıklamalar ile elde edebilirsiniz. Örneğin, Blog başlık özelliği gerekli bir özellik olacak şekilde zorlayabilirsiniz.

``` csharp
    [Required]
    public string Title { get; set; }
```

Hiçbir ek kod veya biçimlendirme değişikliklerini uygulama, var olan bir MVC uygulaması dinamik olarak bile özellik ve ek açıklama adları kullanarak bir ileti oluşturma, istemci tarafı doğrulama gerçekleştirir.

![Şekil 1](~/ef6/media/figure01.png)

İletide back yöntemi bu Oluştur görünümünün, Entity Framework, yeni blog veritabanına kaydetmek için kullanılır, ancak uygulama kodu ulaşmadan önce MVC'ın istemci tarafı doğrulama tetiklenir.

İstemci tarafı doğrulama ancak madde işareti kavram değildir. Kullanıcılar tarayıcılarını özelliklerinin etkileyebilir veya daha da kötüsü henüz bir bilgisayar korsanının bazı aldatmaca UI doğrulamaları önlemek için kullanabilirsiniz. Ancak Entity Framework ayrıca gerekli ek açıklama tanıması ve doğrulayın.

Bunu test etmek için basit bir yol, MVC'ın istemci tarafı doğrulama özelliği devre dışı bırakmaktır. MVC uygulamanın web.config dosyasında, bunu yapabilirsiniz. AppSettings bölümünde ClientValidationEnabled için bir anahtarı yok. Bu anahtarı false olarak ayarlandığında, UI doğrulamaları gerçekleştirmesini engeller.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

Devre dışı bile istemci tarafı doğrulama ile uygulamanızda aynı yanıtı alırsınız. Hata iletisi "Başlık alanı gerekiyor" olarak görüntülenir. Artık sunucu tarafı doğrulama sonucu olacak dışında. Entity Framework gerçekleştirecek doğrulama gerekli ek açıklamayı (Bu da derleme ve veritabanına göndermek için INSERT komutu rahatsız önce) ve hata iletisini görüntüler için MVC döndürür.

## <a name="fluent-api"></a>Fluent API'si

Kod kullandığınız aynı istemci tarafı ve sunucu almak için ilk'ın fluent API'si ek açıklamaları yerine tarafında doğrulama. Yerine gerekli kullanımı, ı, MaxLength doğrulama kullanarak bu göstereceğiz.

Kod ilk modeli sınıfları oluşturma gibi Fluent API'si yapılandırmaları uygulanır. DbContext sınıfı OnModelCreating yöntemini geçersiz kılarak yapılandırmaları ekleyebilir. BloggerName özelliği 10 karakterden daha uzun olabileceğini belirten yapılandırması aşağıda verilmiştir.

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

Doğrulama hataları durum Fluent API'si yapılandırmalarına göre otomatik olarak ulaşma kullanıcı Arabirimi, ancak bu kodu ve ardından yanıt uygun şekilde yakalamaz.

Entity Framework 10 karakter üst sınırını aşıyor bir BloggerName ile blog kaydetmeye çalıştığında doğrulama hata yakalayan uygulamanın BlogController sınıfı hata kodunu aşağıda verilmiştir; bazı özel durum işleme.

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

Doğrulama otomatik olarak geri ModelState.AddModelError kullanan ek kod kullanılan neden olan görünümün geçirilen değil. Bu, hata ayrıntılarını, ardından hata görüntülenecek ValidationMessageFor Htmlhelper kullanacağı görünümde yapmak sağlar.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject System.ComponentModel.DataAnnotations içinde yer alan bir arabirimdir. Entity Framework API parçası değildir ancak yine, sunucu tarafı doğrulama için Entity Framework sınıflarınızdaki yararlanabilirsiniz. Entity Framework SaveChanges veya sırasında çağıracak bir doğrulama yöntemi sınıfları doğrulamak için istediğiniz zaman kendiniz çağırabilirsiniz IValidatableObject sağlar.

Gibi gerekli yapılandırmaları ve MaxLength validaton tek bir alan üzerinde gerçekleştirin. Doğrulama yöntemi, daha karmaşık mantık iki alan karşılaştırma olabilir.

Aşağıdaki örnekte, Blog sınıfı IValidatableObject uygulayın ve ardından başlık ve BloggerName eşleşemez bir kural sağlamak için genişletilmiştir.

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

ValidationResult oluşturucusu, hata iletisi ve doğrulama ile ilişkili olan üye adlarından temsil eden bir dize dizisi temsil eden bir dize alır. Bu doğrulama başlığı hem BloggerName denetler olduğundan, her iki özellik adları döndürülür.

Fluent API'si tarafından sağlanan doğrulama aksine bu doğrulama sonucu görünüm tarafından tanınır ve ben daha önce ModelState ekleme hatası kullanılacak özel durum işleyicisi gereksizdir. Her iki özellik adları ValidationResult ayarlandığından, MVC HtmlHelpers hem de bu özellikler hata iletisini görüntüler.

![Şekil 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

DbContext ValidateEntity adlı geçersiz kılınabilir bir yöntemi vardır. SaveChanges çağırdığınızda, Entity Framework bu yöntemi her varlık durumu değişmeden değil, önbellek için çağırır. Örneğin, önceki bölümde eklediğiniz Blog.Validate yöntemini çağırmak için bu yöntem doğrudan ve hatta burada kullanımda Doğrulama mantığı koyabilirsiniz.

Posta başlık zaten kullanılmadığını emin olmak için yeni gönderileri onaylar ValidateEntity bir geçersiz kılma örneği aşağıda verilmiştir. İlk durumuna eklenir ve varlık bir gönderi olup olmadığını görmek için denetler. Ardından bu durumda, veritabanında zaten bir gönderi başlığıyla aynı olup olmadığını arar. Daha sonra postayı zaten varsa, yeni DbEntityValidationResult oluşturulur.

Bir DbEntityEntry ve tek bir varlık için ICollection, DbValidationErrors DbEntityValidationResult barındırır. Bu yöntemin başında bir DbEntityValidationResult örneği ve sonra bulunan herhangi bir hata, ValidationErrors koleksiyona eklenir.

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

## <a name="explicitly-triggering-validation"></a>Açıkça doğrulama tetikleme

Bu makalede ele alınan doğrulamaları tüm SaveChanges çağrısı tetikler. Ancak, üzerinde SaveChanges yararlanmayı gerekmez. Başka bir yerde uygulamanızda doğrulamak isteyebilirsiniz.

Tüm doğrulamaları, ek açıklamalar ya da Fluent API'si tarafından tanımlanan, doğrulama IValidatableObject (örneğin, Blog.Validate) içinde oluşturulan ve DbContext.ValidateEntity içinde gerçekleştirilen doğrulamaları DbContext.GetValidationErrors tetikler yöntem.

Aşağıdaki kod geçerli bir DbContext örneğinde GetValidationErrors çağırır. ValidationErrors DbValidationRestuls varlık türüne göre gruplandırılır. Kod ilk yöntem tarafından döndürülen DbValidationResults ve ardından her ValidationError içinde aracılığıyla yinelenir.

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

## <a name="other-considerations-when-using-validation"></a>Doğrulama kullanmayla ilgili diğer konular

Entity Framework doğrulama kullanırken dikkate alınması gereken bazı noktalar şunlardır:

-   Yavaş yükleme sırasında doğrulama devre dışı bırakıldı.
-   EF eşlenen özelliklerini (veritabanı sütununa eşlenmemiş) veri ek açıklamalarını doğrular.
-   Değişiklikleri sırasında SaveChanges algılandıktan sonra doğrulama gerçekleştirilir. Doğrulama sırasında değişiklik yaparsanız, değişiklik İzleyici bildirmek için sizin sorumluluğunuzdur.
-   Doğrulama sırasında bir hata oluşursa DbUnexpectedValidationException oluşturulur.
-   Entity Framework modelini (gereken maksimum uzunluktan, vb.) içeren modellerin sınıflarınızı veri ek açıklamalarını değildir ve/veya EF Designer modelinizi oluşturmak için kullanılan olsa bile, doğrulama, neden olur.
-   Öncelik kuralları:
    -   Fluent API çağrılarının karşılık gelen veri ek açıklamalarını geçersiz kıl
-   Yürütme sırası:
    -   Özellik doğrulamasını tür doğrulaması önce gerçekleşir.
    -   Özelliği doğrulama başarılı olursa, tür doğrulaması yalnızca oluşur
-   Karmaşık bir özellik varsa, doğrulama de içerir:
    -   Karmaşık tür özellikleri özellik düzeyi doğrulama
    -   Karmaşık tür IValidatableObject doğrulama dahil olmak üzere, karmaşık tür düzeyinde doğrulama türü

## <a name="summary"></a>Özet

İstemci tarafı doğrulama MVC ile Entity Framework API doğrulama çok düzgün bir şekilde yürütülür, ancak istemci tarafı doğrulamasını güvenmek zorunda değilsiniz. Entity Framework DataAnnotations ya da kod ilk Fluent API'si ile uyguladığınız yapılandırmaları için sunucu tarafında doğrulama ölçeklendirilmesini.

Ayrıca, genişletilebilirlik noktaları IValidatableObject arabirimini kullanın ya da dokunun DbContet.ValidateEntity yöntemin davranışını özelleştirmek için birçok de gördünüz. Ve Code First, Model ilk ya da veritabanı ilk iş akışının kavramsal model tanımlamak için kullandığınız olup olmadığını doğrulama bu son iki çeşit DbContext kullanılabilir.
