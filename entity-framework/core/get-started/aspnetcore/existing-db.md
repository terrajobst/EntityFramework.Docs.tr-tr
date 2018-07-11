---
title: Üzerinde ASP.NET Core - mevcut veritabanı - EF Core kullanmaya başlama
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949159"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Mevcut bir veritabanı ile ASP.NET Core üzerinde EF Core ile çalışmaya başlama

Bu kılavuzda, Entity Framework kullanarak basit veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturacaksınız. Tersine mühendislik, varolan bir veritabanını temel alan bir Entity Framework modelini oluşturmak için kullanır.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) GitHub üzerinde.

## <a name="prerequisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gereklidir:

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) bu iş yükleri ile:
  * **ASP.NET ve web geliştirme** (altında **Web ve bulut**)
  * **.NET core çoklu platform geliştirme** (altında **diğer araç takımları**)
* [.NET core 2.0 SDK'sı](https://www.microsoft.com/net/download/core).
* [Günlük veritabanı](#blogging-database)

### <a name="blogging-database"></a>Günlük veritabanı

Bu öğreticide bir **blog** LocalDb örneğiniz mevcut veritabanı olarak veritabanı.

> [!TIP]  
> Zaten oluşturduysanız **blog** veritabanı başka bir öğreticinin bir parçası olarak, bu adımı atlayabilirsiniz.

* Visual Studio'yu Aç
* **Araçlar -> veritabanına bağlan...**
* Seçin **Microsoft SQL Server** tıklatıp **devam et**
* Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**
* Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**
* Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**
* Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**
* Sorgu Düzenleyicisi'ne, aşağıda listelenen betiği kopyalayın
* Sorgu düzenleyicisini sağ tıklayıp **Yürüt**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Açık Visual Studio 2017
* **Dosya -> Yeni -> Proje...**
* Sol menüden **yüklü şablonları -> Visual C# ' -> Web ->**
* Seçin **ASP.NET Core Web uygulaması (.NET Core)** proje şablonu
* Girin **EFGetStarted.AspNetCore.ExistingDb** tıklayın ve adı olarak **Tamam**
* Bekle **yeni ASP.NET Core Web uygulaması** görüntülenecek iletişim
* Altında **ASP.NET Core şablonları 2.0** seçin **Web uygulaması (Model-View-Controller)**
* Emin **kimlik doğrulaması** ayarlanır **kimlik doğrulaması yok**
* **Tamam**’a tıklayın.

## <a name="install-entity-framework"></a>Entity Framework'ü yükleme

EF Core kullanmak için hedeflemek istediğiniz veritabanı şu sağlayıcı(lar) için paketi yükleyin. Bu izlenecek yol, SQL Server kullanır. Kullanılabilir sağlayıcılar listesi için bkz. [veritabanı sağlayıcıları](../../providers/index.md).

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* `Install-Package Microsoft.EntityFrameworkCore.SqlServer`'i çalıştırın.

Veritabanından bir model oluşturmak için size bazı Entity Framework Tools kullanıyor olacaksınız. Bu nedenle Araçları paketi de yüklenir:

* `Install-Package Microsoft.EntityFrameworkCore.Tools`'i çalıştırın.

Daha sonra denetleyicileri ve görünümleri oluşturmak için size bazı ASP.NET Core yapı İskelesi araçları kullanarak. Bu nedenle bu tasarım paketi de yüklenir:

* `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`'i çalıştırın.

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

 Varlık sınıfları, sorgulama ve kaydetme verilerini temsil eden basit C# nesneleridir.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 İçerik veritabanı ile bir oturumu temsil eder ve sorgu ve varlık sınıflarının örneklerini kaydetmenize olanak tanır.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
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

### <a name="remove-inline-context-configuration"></a>Satır içi içeriği yapılandırmasını Kaldır

ASP.NET Core, yapılandırma genellikle içinde gerçekleştirilen **Startup.cs**. Bu deseni için uygun, biz veritabanı sağlayıcısı için yapılandırma taşınacağı **Startup.cs**.

* Açık `Models\BloggingContext.cs`
* Silme `OnConfiguring(...)` yöntemi

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Bağımlılık ekleme tarafından bağlamına geçirilecek yapılandırma izin veren aşağıdaki oluşturucuyu ekleyin

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Kaydetme ve Bağlamınızı Startup.cs içinde yapılandırma

Bizim MVC denetleyicileri yapmak için sırayla kullanım `BloggingContext` hizmet olarak kaydetmek için kullanacağız.

* Açık **Startup.cs**
* Aşağıdaki `using` deyimlerini dosyanın başında

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Kullanabiliriz artık `AddDbContext(...)` hizmet olarak kaydetmek için yöntemi.
* Bulun `ConfigureServices(...)` yöntemi
* Bağlam bir hizmet olarak kaydetmek için aşağıdaki kodu ekleyin

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> Gerçek bir uygulamada bir yapılandırma dosyasında bağlantı dizesi genellikle koyabilirsiniz. Basitleştirmek amacıyla, biz bunu kodda tanımlarsınız. Daha fazla bilgi için [bağlantı dizeleri](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Bir denetleyici oluşturma

Ardından, biz yapı iskelesi projemizdeki tıklatmalarını sağlarsınız.

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**
* Seçin **tam bağımlılıkları** tıklatıp **Ekle**
* Yönergeleri yoksayabilirsiniz `ScaffoldingReadMe.txt` açılan dosya

Yapı iskelesi etkin, bir denetleyici için iskelesini oluşturabilirsiniz `Blog` varlık.

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**
* Seçin **MVC denetleyici Entity Framework kullanarak görünümler ile** tıklatıp **Tamam**
* Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfının** için **BloggingContext**
* Tıklayın **Ekle**

## <a name="run-the-application"></a>Uygulamayı çalıştırın

Şimdi nasıl çalıştığını görmek için uygulamayı çalıştırabilirsiniz.

* **Hata ayıklama, hata ayıklama olmadan Başlat ->**
* Uygulama oluşturacak ve bir web tarayıcısında Aç
* Gidin `/Blogs`
* Tıklayın **Yeni Oluştur**
* Girin bir **Url** tıklayın ve yeni blog için **oluştur**

![görüntü](_static/create.png)

![görüntü](_static/index-existing-db.png)
