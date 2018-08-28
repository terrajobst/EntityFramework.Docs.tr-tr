---
title: Üzerinde ASP.NET Core - mevcut veritabanı - EF Core kullanmaya başlama
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 79a73e38fdc9c4268c21de66571d6272f33e9457
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997042"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Mevcut bir veritabanı ile ASP.NET Core üzerinde EF Core ile çalışmaya başlama

Bu öğreticide, Entity Framework Core kullanarak basit veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturun. Entity Framework modelini oluşturmak için varolan bir veritabanını mühendisi ters.

[Bu makaledeki örnek Github'da görüntüle](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yazılımları yükleyin:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) bu iş yükleri ile:
  * **ASP.NET ve web geliştirme** (altında **Web ve bulut**)
  * **.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)
* [.NET core SDK'sını 2.1](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Günlük veritabanı oluşturma

Bu öğreticide bir **blog** LocalDb örneğiniz mevcut veritabanı olarak veritabanı. Zaten oluşturduysanız **blog** veritabanı başka bir öğreticinin bir parçası olarak, bu adımı atlayın.

* Visual Studio'yu Aç
* **Araçlar -> veritabanına bağlan...**
* Seçin **Microsoft SQL Server** tıklatıp **devam et**
* Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**
* Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**
* Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**
* Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**
* Sorgu Düzenleyicisi'ne aşağıdaki betiği kopyalayın
* Sorgu düzenleyicisini sağ tıklayıp **Yürüt**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Açık Visual Studio 2017
* **Dosya > Yeni > Proje...**
* Sol menüden **yüklü > Visual C# > Web**
* Seçin **ASP.NET Core Web uygulaması** proje şablonu
* Girin **EFGetStarted.AspNetCore.ExistingDb** tıklayın ve adı olarak **Tamam**
* Bekle **yeni ASP.NET Core Web uygulaması** görüntülenecek iletişim
* Hedef framework açılan ayarlandığından emin olun **.NET Core**, ve sürüm açılan kümesine **ASP.NET Core 2.1**
* Seçin **Web uygulaması (Model-View-Controller)** şablonu
* Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**
* **Tamam**’a tıklayın.

## <a name="install-entity-framework-core"></a>Entity Framework Core yükleme

EF Core yüklemek için hedeflemek istediğiniz EF Core veritabanı sağlayıcı(lar) için paketi yükleyin. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md). 

Bu öğreticide, öğretici, SQL Server kullandığından sağlayıcı paketi yüklemeniz gerekmez. SQL Server sağlayıcı paketi eklenmiştir [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="reverse-engineer-your-model"></a>Modelinizi tersine mühendislik

Artık, mevcut bir veritabanını temel alan EF modeli oluşturma zamanı geldi.

* **Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**
* Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Belirten bir hata alırsanız `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, ardından kapatın ve Visual Studio'yu yeniden açın.

> [!TIP]  
> Varlıklar için ekleyerek oluşturmak istediğiniz tabloları belirtebilirsiniz `-Tables` bağımsız değişkeni için yukarıdaki komutu. Örneğin, `-Tables Blog,Post`.

Oluşturulan varlık sınıfları ters mühendislik süreci (`Blog.cs` & `Post.cs`) ve türetilmiş bir içerik (`BloggingContext.cs`) var olan veritabanı şemasını temel alan.

 Varlık sınıfları, sorgulama ve kaydetme verilerini temsil eden basit C# nesneleridir. İşte `Blog` ve `Post` varlık sınıfları:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Yavaş yükleniyor etkinleştirmek için Gezinti özellikleri yapabilirsiniz `virtual` (Blog.Post ve Post.Blog).

 İçerik veritabanı ile bir oturumu temsil eder ve sorgu ve varlık sınıflarının örneklerini kaydetmenize olanak tanır.

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a>Bağlamınızı bağımlılık ekleme ile kaydetme

ASP.NET Core için bağımlılık ekleme kavramının taşımaktadır. Hizmetler (gibi `BloggingContext`) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetler (örneğin, MVC denetleyicileri) gerektiren bileşenler sonra hizmetlerin Oluşturucu parametresi veya özellikleri aracılığıyla sağlanır. Bağımlılık ekleme hakkında daha fazla bilgi için bkz. [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) ASP.NET sitesinde makalesi.

### <a name="register-and-configure-your-context-in-startupcs"></a>Kaydetme ve Bağlamınızı Startup.cs içinde yapılandırma

Yapmak `BloggingContext` MVC denetleyicileri için kullanılabilir bir hizmet olarak kaydedin.

* Açık **Startup.cs**
* Aşağıdaki `using` deyimlerini dosyanın başında

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Artık `AddDbContext(...)` hizmet olarak kaydetmek için yöntemi.
* Bulun `ConfigureServices(...)` yöntemi
* Bağlam bir hizmet olarak kaydetmek için aşağıdaki vurgulanmış kodu ekleyin

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> Gerçek bir uygulamada genellikle bir yapılandırma dosyası veya ortam değişkenine bağlantı dizesini koyabilirsiniz. Basitleştirmek amacıyla, Bu öğretici, kod içinde tanımlamanıza sahiptir. Daha fazla bilgi için [bağlantı dizeleri](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Bir denetleyici ve görünümler oluşturma

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**
* Seçin **MVC denetleyici Entity Framework kullanarak görünümler ile** tıklatıp **Tamam**
* Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfının** için **BloggingContext**
* Tıklayın **Ekle**

## <a name="run-the-application"></a>Uygulamayı çalıştırın

Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.

* **Hata ayıklama, hata ayıklama olmadan Başlat ->**
* Uygulama oluşturur ve bir web tarayıcısında açılır
* Gidin `/Blogs`
* Tıklayın **Yeni Oluştur**
* Girin bir **Url** tıklayın ve yeni blog için **oluştur**

![görüntü](_static/create.png)

![görüntü](_static/index-existing-db.png)
