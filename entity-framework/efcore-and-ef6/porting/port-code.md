---
title: EF6'dan EF Core'a taşıma - kod tabanlı bir modeli taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997055"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="3e0c0-102">EF6 kod tabanlı bir modeli EF core'a taşıma</span><span class="sxs-lookup"><span data-stu-id="3e0c0-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="3e0c0-103">Tüm uyarılar makaleyi okudunuz ve bağlantı noktası için hazır olursunuz, başlamanıza yardımcı olmak için bazı Kılavuzlar şunlardır.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="3e0c0-104">EF Core NuGet paketi yüklemesi</span><span class="sxs-lookup"><span data-stu-id="3e0c0-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="3e0c0-105">EF Core kullanmak için kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="3e0c0-106">Örneğin, SQL Server hedeflenirken yüklemeniz `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="3e0c0-107">Bkz: [veritabanı sağlayıcıları](../../core/providers/index.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="3e0c0-108">Geçişleri kullanmayı planladığınıza sonra de yüklemeniz gerekir `Microsoft.EntityFrameworkCore.Tools` paket.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="3e0c0-109">EF Core ve EF6 kullanılan yan yana aynı uygulamada olması gibi yüklü EF6 NuGet paketini (EntityFramework) bırakmak uygundur.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="3e0c0-110">Uygulamanızın tüm alanlarda EF6'ı kullanmayı planlayan değildir, ancak sonra paket kaldırılıyor dikkat etmeniz gereken kod parçaları derleme hatalarını vermek yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="3e0c0-111">Ad alanlarını değiştirme</span><span class="sxs-lookup"><span data-stu-id="3e0c0-111">Swap namespaces</span></span>

<span data-ttu-id="3e0c0-112">EF6 içinde kullanan çoğu API'ler bulunan `System.Data.Entity` ad alanı (ve ilgili alt ad alanları).</span><span class="sxs-lookup"><span data-stu-id="3e0c0-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="3e0c0-113">İlk kod değişikliği için değiştirilecek olan `Microsoft.EntityFrameworkCore` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="3e0c0-114">Genellikle türetilmiş bağlam kodu dosyanızla başlatın ve ardından derleme hatası oluşunca adresleme buradan iş.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="3e0c0-115">Bağlam içi yapılandırma (bağlantı vs.)</span><span class="sxs-lookup"><span data-stu-id="3e0c0-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="3e0c0-116">Bölümünde anlatıldığı gibi [olun EF Core olacak iş uygulamanız için](ensure-requirements.md), EF Core olan algılama bağlanmak için veritabanı geçici olarak daha az Sihirli.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="3e0c0-117">Geçersiz kılmak ihtiyacınız olacak `OnConfiguring` yöntemi türetilmiş bağlam ve veritabanı bağlantısı kurmak için veritabanı sağlayıcısı özel API'yi kullanın.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="3e0c0-118">EF6 uygulamaların çoğu uygulamalarında bağlantı dizesini depolama `App/Web.config` dosya.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="3e0c0-119">Bu bağlantı dizesini kullanarak EF Core okuma `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="3e0c0-120">Bir başvuru eklemeniz gerekebilir `System.Configuration` bu API'yi kullanabilmek için framework derlemesi.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="3e0c0-121">Kodunuzu güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="3e0c0-121">Update your code</span></span>

<span data-ttu-id="3e0c0-122">Bu noktada, derleme hataları çözdükten ve davranış değişiklikleri, etkiler, görmek için kod gözden geçirme bir konudur.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="3e0c0-123">Mevcut geçişleri</span><span class="sxs-lookup"><span data-stu-id="3e0c0-123">Existing migrations</span></span>

<span data-ttu-id="3e0c0-124">Gerçekten mevcut EF6 geçişler EF Core için bağlantı noktası için uygun bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="3e0c0-125">Mümkünse, veritabanına EF6'dan önceki tüm geçişler uygulanmış olan ve ardından EF Core kullanarak, şema geçişi başlangıç noktası varsayılır en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="3e0c0-126">Bunu yapmak için kullanacağınız `Add-Migration` modeli, EF Core için unity'nin sonra bir geçiş eklemek için komutu.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="3e0c0-127">Ardından tüm koddan kaldırın `Up` ve `Down` iskele kurulmuş geçiş yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="3e0c0-128">Bu ilk geçiş iskele kurulmuş zaman sonraki geçişleri modele karşılaştıracağız.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="3e0c0-129">Bağlantı testi</span><span class="sxs-lookup"><span data-stu-id="3e0c0-129">Test the port</span></span>

<span data-ttu-id="3e0c0-130">Yalnızca uygulamanızı derleyen çünkü başarıyla EF Core için unity'nin anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="3e0c0-131">Tüm alanları davranışı değişikliklerin hiçbiri olumsuz uygulamanızı etkilemiş emin olmak için uygulamanızı test etmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e0c0-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
