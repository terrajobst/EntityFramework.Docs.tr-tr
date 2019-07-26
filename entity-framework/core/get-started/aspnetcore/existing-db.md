---
title: ASP.NET Core var olan veritabanı ile çalışmaya başlama-EF Core
author: rowanmiller
description: Mevcut bir veritabanıyla ASP.NET Core EF Core kullanmaya başlama
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 6b0ed0a9222644bee31d23234aa27b2084137f4a
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497520"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Mevcut bir veritabanıyla ASP.NET Core EF Core kullanmaya başlama

Bu öğreticide, Entity Framework Core kullanarak temel veri erişimi gerçekleştiren bir ASP.NET Core MVC uygulaması oluşturacaksınız. Bir Entity Framework modeli oluşturmak için var olan bir veritabanına ters mühendislik uygulamalısınız.

[Bu makalenin örneğini GitHub 'Da görüntüleyin](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yazılımı yükler:

* Bu iş yükleriyle [Visual Studio 2017 15,7](https://www.visualstudio.com/downloads/) :
  * **ASP.net ve Web geliştirme** ( **Web & bulutu**altında)
  * **.NET Core platformlar arası geliştirme** ( **diğer araç kümeleri**altında)
* [.NET Core 2.1 SDK'sı](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Blog veritabanı oluşturma

Bu öğretici, LocalDb örneğinizdeki bir **Blog** veritabanını var olan veritabanı olarak kullanır. **Blog** veritabanını başka bir öğreticinin parçası olarak zaten oluşturduysanız, bu adımları atlayın.

* Visual Studio 'Yu aç
* **Araçlar-> veritabanına bağlan...**
* **Microsoft SQL Server** seçin ve **devam** ' a tıklayın
* **Sunucu adı** olarak **(LocalDB) \mssqllocaldb** yazın
* **Asıl** **veritabanı adı** olarak girin ve **Tamam 'a** tıklayın
* Ana veritabanı artık **Sunucu Gezgini** Içindeki **veri bağlantıları** altında görüntülenir
* **Sunucu Gezgini** veritabanında veritabanına sağ tıklayın ve **Yeni sorgu** ' yı seçin.
* Aşağıda listelenen betiği sorgu düzenleyicisine Kopyala
* Sorgu düzenleyicisine sağ tıklayıp **Yürüt** ' ü seçin.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Visual Studio 2017 'yi açın
* **Dosya yeni > projesi >...**
* Soldaki menüden, **Visual C# > Web > yüklü** ' i seçin
* **ASP.NET Core Web uygulaması** proje şablonunu seçin
* Ad olarak **Efgetstarted. AspNetCore. ExistingDb** yazın (kodda daha sonra kullanılan ad alanıyla tam olarak eşleşmelidir) ve **Tamam** ' a tıklayın. 
* **Yeni ASP.NET Core Web uygulaması** iletişim kutusunun görüntülenmesini bekleyin
* Hedef çerçeve açılan menüsünün **.NET Core**olarak ayarlandığından ve sürüm açılan menüsünün **ASP.NET Core 2,1** olarak ayarlandığından emin olun
* **Web uygulaması (Model-View-Controller)** şablonunu seçin
* **Kimlik doğrulamasının** **kimlik doğrulaması yok** olarak ayarlandığından emin olun
*           **Tamam**’a tıklayın.

## <a name="install-entity-framework-core"></a>Entity Framework Core yüklensin

EF Core yüklemek için, hedeflemek istediğiniz EF Core veritabanı sağlayıcılarının paketini yüklersiniz. Kullanılabilir sağlayıcıların listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md). 

Öğretici SQL Server kullandığından, bu öğretici için bir sağlayıcı paketi yüklemenize gerek yoktur. SQL Server sağlayıcısı paketi [Microsoft. AspnetCore. app metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1)'e dahildir.

## <a name="reverse-engineer-your-model"></a>Modelinize ters mühendislik

Şimdi de mevcut veritabanınıza göre EF modeli oluşturma zamanı.

* **Araçlar – > NuGet Paket Yöneticisi – > Paket Yöneticisi konsolu**
* Varolan veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Belirten `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`bir hata alırsanız Visual Studio 'yu kapatıp yeniden açın.

> [!TIP]  
> Yukarıdaki komuta `-Tables` bağımsız değişkenini ekleyerek hangi tabloları için varlık oluşturmak istediğinizi belirtebilirsiniz. Örneğin: `-Tables Blog,Post`.

Tersine mühendislik işlemi, mevcut veritabanının şemasına bağlı`Blog.cs`olarak varlık sınıfları ( & `Post.cs`) ve`BloggingContext.cs`türetilmiş bir bağlam () oluşturdu.

 Varlık sınıfları, sorguladığınız C# ve kaydedilecektir verileri temsil eden basit nesnelerdir. `Blog` Ve`Post` varlık sınıfları şunlardır:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Yavaş yüklemeyi etkinleştirmek için, gezinti özelliklerini `virtual` (blog.Post ve post. blog) yapabilirsiniz.

 Bağlam, veritabanı ile bir oturumu temsil eder ve varlık sınıflarının örneklerini sorgulamanızı ve kaydetmenizi sağlar.

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

## <a name="register-your-context-with-dependency-injection"></a>Bağlama ekleme ile bağlamını kaydetme

Bağımlılık ekleme kavramı ASP.NET Core merkezidir. Hizmetler (gibi `BloggingContext`), uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu hizmetleri gerektiren bileşenler (MVC denetleyicileriniz gibi), daha sonra bu hizmetleri Oluşturucu parametreleri veya özellikleri aracılığıyla sağlıyordu. Bağımlılık ekleme hakkında daha fazla bilgi için ASP.NET sitesindeki [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) makalesine bakın.

### <a name="register-and-configure-your-context-in-startupcs"></a>Startup.cs 'de bağlamını kaydetme ve yapılandırma

MVC denetleyicileri `BloggingContext` için kullanılabilir hale getirmek için bir hizmet olarak kaydedin.

* Açık **Startup.cs**
* Aşağıdaki `using` deyimlerini dosyanın başlangıcına ekleyin

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Artık `AddDbContext(...)` yöntemi bir hizmet olarak kaydetmek için kullanabilirsiniz.
* `ConfigureServices(...)` Yöntemi bulun
* Bağlamı hizmet olarak kaydetmek için aşağıdaki vurgulanmış kodu ekleyin

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> Gerçek bir uygulamada, genellikle bağlantı dizesini bir yapılandırma dosyasına veya ortam değişkenine yerleştirebilirsiniz. Kolaylık sağlaması için, bu öğretici kodu kod içinde tanımlamanızı ister. Daha fazla bilgi için bkz. [bağlantı dizeleri](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Denetleyici ve görünüm oluşturma

* **Çözüm Gezgini** ' de **denetleyiciler** klasörüne sağ tıklayın ve **Ekle-> denetleyicisi ' ni seçin...**
* **Görünümler Ile MVC denetleyicisi ' ni seçin, Entity Framework kullanarak** **Tamam** ' a tıklayın.
* **Model sınıfını** **Blog** ve **veri bağlamı sınıfına** **BloggingContext** olarak ayarla
* **Ekle** 'ye tıklayın

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Artık uygulamayı çalışır durumda görmek için çalıştırabilirsiniz.

* **Hata ayıklama-> hata ayıklama olmadan Başlat**
* Uygulama bir Web tarayıcısında oluşturulup açılıyor
* Gidin `/Blogs`
* **Yeni oluştur** ' a tıklayın
* Yeni blog için bir **URL** girin ve **Oluştur** ' a tıklayın.

  ![sayfası oluşturma](_static/create.png)

  ![Dizin sayfası](_static/index-existing-db.png)

## <a name="next-steps"></a>Sonraki adımlar

Bağlam ve varlık sınıflarının nasıl kullanılacağı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
* [Tersine mühendislik](xref:core/managing-schemas/scaffolding)
* [Entity Framework Core araçları başvurusu-.NET CLı](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Entity Framework Core araçları başvurusu-Paket Yöneticisi konsolu](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
