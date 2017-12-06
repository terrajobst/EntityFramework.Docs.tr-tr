---
title: "ASP.NET Core - var olan veritabanı - EF Çekirdeğinde Başlarken"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Varolan bir veritabanını ile ASP.NET Core EF Çekirdeğinde ile çalışmaya başlama

> [!IMPORTANT]  
> [.NET Core SDK](https://www.microsoft.com/net/download/core) artık `project.json` veya Visual Studio 2015. Herkes .NET Core geliştirme yapılması önerilir [project.json için csproj geçirmek](https://docs.microsoft.com/dotnet/articles/core/migration/) ve [Visual Studio 2017](https://www.visualstudio.com/downloads/).

Bu kılavuzda, Entity Framework kullanarak temel veri erişimi gerçekleştirdiği bir ASP.NET Core MVC uygulaması oluşturacaksınız. Varolan bir veritabanını temel alan bir Entity Framework modelini oluşturmak için ters mühendislik kullanır.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) github'da.

## <a name="prerequisites"></a>Önkoşullar

Bu izlenecek yolu tamamlamak için aşağıdaki önkoşullar gerekir:

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) bu iş yükleri ile:
  * **ASP.NET ve web geliştirme** (altında **Web ve bulut**)
  * **.NET core platformlar arası geliştirme** (altında **diğer Toolsets**)
* [.NET 2.0 SDK çekirdek](https://www.microsoft.com/net/download/core).
* [Blog veritabanı](#blogging-database)

### <a name="blogging-database"></a>Blog veritabanı

Bu öğretici kullanan bir **blog** veritabanı, yerel veritabanı örneğinde var olan veritabanı olarak.

> [!TIP]  
> Zaten oluşturduysanız **blog** veritabanı başka bir öğretici bir parçası olarak, bu adımı atlayabilirsiniz.

* Açık Visual Studio
* **Araçlar -> veritabanına bağlan...**
* Seçin **Microsoft SQL Server** tıklatıp **devam et**
* Girin **(localdb) \mssqllocaldb** olarak **sunucu adı**
* Girin **ana** olarak **veritabanı adı** tıklatıp **Tamam**
* Ana veritabanı artık altında görüntülenen **veri bağlantıları** içinde **Sunucu Gezgini**
* Veritabanına sağ tıklayın **Sunucu Gezgini** seçip **yeni sorgu**
* Aşağıda, sorgu Düzenleyicisi'ne komut dosyasını kopyalayın
* Üzerinde sorgu Düzenleyicisi'ni sağ tıklatın ve seçin **çalıştırma**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

* Visual Studio 2017 Aç
* **Dosya -> Yeni Proje ->...**
* Sol menüden seçin **yüklü şablonları -> Visual C# -> Web ->**
* Seçin **ASP.NET çekirdek Web uygulaması (.NET Core)** proje şablonu
* Girin **EFGetStarted.AspNetCore.ExistingDb** tıklatın ve adı olarak **Tamam**
* Bekle **çekirdek yeni bir ASP.NET Web uygulaması** görünmesi iletişim
* Altında **ASP.NET Core şablonları 2.0** seçin **Web uygulaması (Model-View-Controller)**
* Emin **kimlik doğrulaması** ayarlanır **doğrulaması yok**
* **Tamam**’a tıklayın.

## <a name="install-entity-framework"></a>Entity Framework'ü yükleme

EF çekirdek kullanmak için hedeflemek istediğiniz veritabanı sağlayıcı(lar) için paketini yükleyin. Bu kılavuz, SQL Server kullanır. Kullanılabilir sağlayıcılar listesi için bkz: [veritabanı sağlayıcıları](../../providers/index.md).

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**

* Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Veritabanından bir model oluşturmak için size bazı Entity Framework araçları kullanarak. Böylece biz de araçları paketini yükleyecek:

* Çalıştırma`Install-Package Microsoft.EntityFrameworkCore.Tools`

Daha sonra denetleyicileri ve görünümleri oluşturmak için size bazı ASP.NET Core İskele araçlarını kullanma. Böylece biz de bu tasarım paket yükleyecek:

* Çalıştırma`Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="reverse-engineer-your-model"></a>Tersine mühendislik modelinizi

Şimdi, varolan bir veritabanını temel alan EF modeli oluşturmak için zaman yapılır.

* **Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu**
* Varolan bir veritabanından bir model oluşturmak için aşağıdaki komutu çalıştırın:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Belirten bir hata alırsanız, `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, ardından kapatın ve Visual Studio'yu yeniden açın.

> [!TIP]  
> Varlıklar için ekleyerek oluşturmak istediğiniz tabloları belirtebilirsiniz `-Tables` yukarıdaki komut bağımsız değişkeni. Örneğin `-Tables Blog,Post`.

Ters mühendislik işlemi oluşturulan sınıflar (`Blog.cs` & `Post.cs`) ve türetilmiş bir içerik (`BloggingContext.cs`) var olan veritabanı şemasını temel alan.

 Sınıflar, sorgulama kaydetme ve verilerini temsil eden basit C# nesneleridir.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 Bağlam veritabanı oturumla temsil eder ve sorgu ve varlık sınıfları kaydetmenize olanak sağlar.

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

## <a name="register-your-context-with-dependency-injection"></a>İçeriğiniz bağımlılık ekleme ile kaydetme

Bağımlılık ekleme kavramı ASP.NET Core taşımaktadır. Hizmetler (gibi `BloggingContext`) uygulama başlatma sırasında bağımlılık ekleme ile kaydedilir. Bu Hizmetleri (örneğin, MVC denetleyicileri) gerektiren bileşenler daha sonra bu hizmetlere Oluşturucu parametreleri veya özellikleri yoluyla sağlanır. Bağımlılık ekleme hakkında daha fazla bilgi için bkz: [bağımlılık ekleme](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) makale ASP.NET sitesi üzerinde.

### <a name="remove-inline-context-configuration"></a>Satır içi içeriği yapılandırmasını Kaldır

ASP.NET çekirdek yapılandırması genellikle içinde gerçekleştirilir **haline**. Bu desene uyacak şekilde biz veritabanı sağlayıcısı yapılandırmasını taşınır **haline**.

* Açık`Models\BloggingContext.cs`
* Silme `OnConfiguring(...)` yöntemi

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Bağımlılık ekleme tarafından bağlamına geçirilecek yapılandırma sağlayacak aşağıdaki oluşturucuyu ekleyin

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Kaydolun ve içeriğiniz haline içinde yapılandırın

Bizim MVC denetleyicileri yapmak için sırayla kullanımı `BloggingContext` bir hizmet olarak kaydetmek için kalacaklarını.

* Açık **haline**
* Aşağıdakileri ekleyin `using` dosyasının başındaki deyimleri

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Artık biz kullanarak `AddDbContext(...)` bir hizmet olarak kaydetmek için yöntem.
* Bulun `ConfigureServices(...)` yöntemi
* Bağlam bir hizmet olarak kaydetmek için aşağıdaki kodu ekleyin

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> Gerçek bir uygulamada genellikle bir yapılandırma dosyasında bağlantı dizesini koyabilirsiniz. Basitleştirmek amacıyla, biz bu kodda tanımlıyorsanız. Daha fazla bilgi için bkz: [bağlantı dizeleri](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Bir denetleyici oluşturma

Ardından, yapı iskelesi bizim projesinde etkinleştirme.

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**
* Seçin **tam bağımlılıkları** tıklatıp **Ekle**
* ' Ndaki yönergeleri yoksayabilirsiniz `ScaffoldingReadMe.txt` açar dosyası

Askılama özelliğinin etkinleştirildiğinden, bir denetleyici için iskele `Blog` varlık.

* Sağ **denetleyicileri** klasöründe **Çözüm Gezgini** seçip **Ekle -> denetleyicisi...**
* Seçin **görünümleri ile MVC Entity Framework kullanarak denetleyicisi** tıklatıp **Tamam**
* Ayarlama **Model sınıfı** için **Blog** ve **veri bağlamı sınıfı** için **BloggingContext**
* Tıklatın **Ekle**

## <a name="run-the-application"></a>Uygulamayı çalıştırın

Şimdi eylemde görmek için uygulamayı çalıştırabilirsiniz.

* **Hata ayıklama, hata ayıklama olmadan Başlat ->**
* Uygulama oluşturma ve bir web tarayıcısında Aç
* Gidin`/Blogs`
* Tıklatın **Yeni Oluştur**
* Girin bir **Url** tıklatın ve yeni blog **oluştur**

![görüntü](_static/create.png)

![görüntü](_static/index-existing-db.png)
