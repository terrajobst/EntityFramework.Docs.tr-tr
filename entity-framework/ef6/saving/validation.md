---
title: Doğrulama-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 4162c2eb60109459c799da7cf4c1a9c8e84548b6
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182125"
---
# <a name="data-validation"></a>Veri Doğrulama
> [!NOTE]
> **EF 4.1 yalnızca** sonraki sürümler-bu sayfada açıklanan özellikler, API 'ler vb. Entity Framework 4,1 ' de tanıtılmıştı. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz

Bu sayfadaki içerik, başlangıçta Julie Lerman ([https://thedatafarm.com](http://thedatafarm.com)) tarafından yazılmış bir makaleden uyarlanmıştır.

Entity Framework, istemci tarafı doğrulama için bir kullanıcı arabirimine veya sunucu tarafı doğrulaması için kullanılabilir. Önce kodu kullanırken, daha fazla açıklama veya Fluent API yapılandırma kullanarak doğrulama belirtebilirsiniz. Kod içinde ek doğrulamalar ve daha karmaşık bir şekilde belirlenebilir ve önce, modelinizde önce koddan, modelden önce veya önce modelinizden çekip önlenebilir.

## <a name="the-model"></a>Model

Basit bir sınıf çiftiyle doğrulamaları göstereceğim: Blog ve gönderi.

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

## <a name="data-annotations"></a>Veri Açıklamaları

Code First, kod ilk sınıflarını yapılandırmanın bir yolu olarak `System.ComponentModel.DataAnnotations` derlemesindeki ek açıklamaları kullanır. Bu ek açıklamalar arasında `Required`, `MaxLength` ve `MinLength` gibi kurallar sağlayan olanlardır. Birçok .NET istemci uygulaması da bu ek açıklamaları tanır, örneğin, ASP.NET MVC. Bu ek açıklamalarla hem istemci tarafı hem de sunucu tarafı doğrulaması elde edebilirsiniz. Örneğin, blog title özelliğini gerekli bir özellik olacak şekilde zorlayabilirsiniz.

``` csharp
[Required]
public string Title { get; set; }
```

Uygulamada ek kod veya biçimlendirme değişikliği olmadan, mevcut bir MVC uygulaması, özelliği ve ek açıklama adlarını kullanarak dinamik olarak bir ileti oluşturarak istemci tarafı doğrulaması gerçekleştirir.

![Şekil 1](~/ef6/media/figure01.png)

Bu oluşturma görünümünün geri gönder yönteminde, yeni blogu veritabanına kaydetmek için Entity Framework kullanılır, ancak uygulama bu koda ulaşmadan önce MVC 'nin istemci tarafı doğrulaması tetiklenir.

İstemci tarafı doğrulaması, bir madde işareti değildir, ancak. Kullanıcılar, tarayıcı özelliklerini etkileyebilir veya daha kötü bir şekilde kötü, Kullanıcı arabirimi doğrulamaları önlemek için bazı karmaşık kery kullanabilir. Aynı zamanda Entity Framework, `Required` ek açıklamasını tanır ve doğrular.

Bunu sınamanın basit bir yolu, MVC 'nin istemci tarafı doğrulama özelliğini devre dışı bırakmanız. Bunu, MVC uygulamasının Web. config dosyasında yapabilirsiniz. AppSettings bölümünde ClientValidationEnabled için bir anahtar bulunur. Bu anahtarın false olarak ayarlanması, Kullanıcı arabiriminin doğrulamaları gerçekleştirmesini engeller.

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

İstemci tarafı doğrulaması devre dışı bırakılsa bile, uygulamanızda aynı yanıtı alırsınız. "Başlık alanı gereklidir" hata iletisi, daha önce olduğu gibi görüntülenecektir. Bunun dışında, sunucu tarafı doğrulamanın bir sonucu olacaktır. Entity Framework, `Required` ek açıklamasında (veritabanına gönderilmek üzere bir `INSERT` komutu oluşturmak için bile, hatta hata vermezler) ve iletiyi görüntüleyen MVC 'ye hatayı döndürecek şekilde doğrulamayı gerçekleştirecek.

## <a name="fluent-api"></a>Akıcı API

Aynı istemci tarafı & sunucu tarafı doğrulamasını almak için, kod açıklamaları yerine önce kod Fluent API kullanabilirsiniz. @No__t-0 kullanmak yerine, bunu bir MaxLength doğrulaması kullanarak göstereceğiz.

Kod, ilk olarak sınıftan model oluşturuyor olduğundan, akıcı API konfigürasyonları uygulanır. DbContext sınıfının ' Onmodeloluþturma metodunu geçersiz kılarak, konfigürasyonları ekleyebilirsiniz. BloggerName özelliğinin 10 karakterden uzun olmadığını belirten bir yapılandırma aşağıda verilmiştir.

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

Akıcı API yapılandırmalarına dayalı olarak oluşturulan doğrulama hataları, Kullanıcı arabirimine otomatik olarak ulaşmaz, ancak kodda yakalayıp buna uygun şekilde yanıt verebilir.

Entity Framework, uygulamanın BlogController sınıfında, bu doğrulama hatasını yakalayan ve bu, 10 karakterlik en büyük değeri aşan bir BloggerName ile blog kaydetmeye çalıştığında oluşan bazı özel durum işleme hata kodu.

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

Doğrulama, `ModelState.AddModelError` kullanan ek kodun neden kullanıldığı, otomatik olarak görünüme geri geçirilmiyor. Bu, hata ayrıntılarının, hatayı görüntülemek için `ValidationMessageFor` HtmlHelper kullanacağı görünüme yapmasını sağlar.

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

`IValidatableObject`, `System.ComponentModel.DataAnnotations` ' de bulunan bir arabirimdir. Entity Framework API 'sinin bir parçası olmasa da, Entity Framework sınıflarınızda sunucu tarafı doğrulama için bundan yararlanmaya devam edebilirsiniz. `IValidatableObject`, Entity Framework SaveChanges sırasında çağıracağını belirten bir `Validate` yöntemi sağlar veya kendi sınıflarını doğrulamak istediğiniz zaman çağırabilirsiniz.

@No__t-0 ve `MaxLength` gibi konfigürasyonlar tek bir alanda doğrulama gerçekleştirir. @No__t-0 yönteminde, örneğin iki alanı karşılaştıran daha karmaşık mantığa sahip olabilirsiniz.

Aşağıdaki örnekte, `Blog` sınıfı `IValidatableObject` ' i uygulamak üzere genişletilmiştir ve sonra `Title` ve `BloggerName` ' ün eşleşemez bir kural sağlamalıdır.

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

@No__t-0 Oluşturucusu, hata iletisini ve doğrulamayla ilişkili üye adlarını temsil eden bir `string`s dizisini temsil eden bir `string` alır. Bu doğrulama hem `Title` hem de `BloggerName` ' i denetlediğinden, her iki özellik adı da döndürülür.

Akıcı API tarafından sağlanmış olan doğrulamanın aksine, bu doğrulama sonucu görünüm tarafından tanınacaktır ve daha önce hatayı eklemek için kullandığım özel durum işleyicisi `ModelState` gereksizdir. @No__t-0 ' da her iki özellik adını ayarlayacağım, MVC Htmlyardımcıları bu özelliklerin her ikisi için de hata iletisini görüntüler.

![Şekil 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext. ValidateEntity

`DbContext` `ValidateEntity` adlı geçersiz kılınabilir bir yönteme sahip. @No__t-0 ' ı çağırdığınızda Entity Framework, durumu `Unchanged` olmayan önbelleğindeki her varlık için bu yöntemi çağırır. Doğrulama mantığını doğrudan buraya yerleştirebilir veya bir önceki bölümde eklenen `Blog.Validate` yöntemini çağırmak için bu yöntemi kullanabilirsiniz.

Aşağıda, posta başlığının henüz kullanılmadığından emin olmak için yeni `Post`s ' i doğrulayan `ValidateEntity` geçersiz kılma örneği verilmiştir. İlk olarak varlığın bir gönderi olup olmadığını ve durumunun eklenip eklenmediğini kontrol eder. Bu durumda, aynı başlığa sahip bir gönderi olup olmadığını görmek için veritabanına bakar. Zaten var olan bir gönderi varsa, yeni bir `DbEntityValidationResult` oluşturulur.

`DbEntityValidationResult`, tek bir varlık için bir `DbEntityEntry` ve `ICollection<DbValidationErrors>` ' i barındırır. Bu yöntemin başlangıcında, bir `DbEntityValidationResult` örneği oluşturulur ve sonra bulunan tüm hatalar `ValidationErrors` koleksiyonuna eklenir.

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

## <a name="explicitly-triggering-validation"></a>Doğrulamayı açıkça tetikleme

@No__t-0 ' A yapılan bir çağrı, bu makalede ele alınan tüm doğrulamaları tetikler. Ancak `SaveChanges` ' a güvenmeniz gerekmez. Uygulamanızda başka bir yerde doğrulama yapmayı tercih edebilirsiniz.

`DbContext.GetValidationErrors`, ek açıklamalar veya akıcı API tarafından tanımlanan doğrulamaları, `IValidatableObject` ' de (örneğin, `Blog.Validate`) oluşturulan doğrulamayı ve `DbContext.ValidateEntity` yönteminde gerçekleştirilen doğrulamaları tetikleyecektir.

Aşağıdaki kod, `DbContext` ' in geçerli örneğinde `GetValidationErrors` ' a çağrı yapılır. `ValidationErrors` ' ın varlık türüne göre `DbEntityValidationResult` ' e gruplanırlar. Kod, ilk olarak yöntemi tarafından döndürülen `DbEntityValidationResult`s ve sonra içinde her `DbValidationError` üzerinden yinelenir.

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

## <a name="other-considerations-when-using-validation"></a>Doğrulama kullanırken dikkat edilecek diğer noktalar

Entity Framework doğrulaması kullanırken göz önünde bulundurmanız gereken birkaç farklı noktası aşağıda verilmiştir:

- Doğrulama sırasında yavaş yükleme devre dışı bırakıldı
- EF, eşlenmemiş özelliklerde veri açıklamalarını doğrular (veritabanındaki bir sütunla eşlenmemiş Özellikler)
- @No__t-0 sırasında değişiklikler algılandıktan sonra doğrulama gerçekleştirilir. Doğrulama sırasında değişiklik yaparsanız değişiklik izleyicinize bildirimde bulunan sorumluluk sizin sorumluluğunuzdadır
- doğrulama sırasında hata oluşursa, `DbUnexpectedValidationException` atılır
- Entity Framework model (maksimum uzunluk, gerekli, vb.), sınıflarınızda hiç bir veri ek açıklaması olmasa bile doğrulamaya neden olur ve/veya modelinizi oluşturmak için EF tasarımcısını kullandıysanız
- Öncelik kuralları:
  - Akıcı API çağrıları karşılık gelen veri açıklamalarını geçersiz kılar
- Yürütme sırası:
  - Özellik doğrulaması tür doğrulamasından önce oluşur
  - Tür doğrulaması yalnızca özellik doğrulaması başarılı olursa gerçekleşir
- Bir özellik karmaşıksa, doğrulama işlemi de şunları içerir:
  - Karmaşık tür özelliklerinde özellik düzeyinde doğrulama
  - Karmaşık tür üzerinde `IValidatableObject` doğrulaması da dahil olmak üzere, karmaşık türde düzey doğrulaması yazın

## <a name="summary"></a>Özet

Entity Framework 'deki doğrulama API 'SI MVC 'de istemci tarafı doğrulaması ile çok daha fazla oynatılır, ancak istemci tarafı doğrulamaya güvenmeniz gerekmez. Entity Framework, kod ilk akıcı API ile uyguladığınız veri açıklamaları veya yapılandırmaların sunucu tarafında doğrulama işlemini ele alır.

Ayrıca, `IValidatableObject` arabirimini kullanıp kullanmayacağınızı veya `DbContext.ValidateEntity` yöntemine dokunarak davranışı özelleştirmek için birçok genişletilebilirlik noktası da gördünüz. Ve `DbContext` ' ı kullanarak, kavramsal modelinizi anlatmak için Code First, Model First veya Database First iş akışını kullanmanıza bakılmaksızın, doğrulamanın bu son iki yolu vardır.
