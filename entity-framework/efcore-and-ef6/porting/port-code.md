---
title: Kod tabanlı bir Model taşıma EF çekirdek - EF6 bağlantı noktası oluşturma
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054285"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="a44ba-102">Kod tabanlı bir EF6 modeli EF çekirdek için bağlantı noktası oluşturma</span><span class="sxs-lookup"><span data-stu-id="a44ba-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="a44ba-103">Tüm uyarılar okuduğunuz ve bağlantı noktasına hazır, başlamanıza yardımcı olacak bazı yönergeler şunlardır.</span><span class="sxs-lookup"><span data-stu-id="a44ba-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="a44ba-104">EF Core NuGet paketi yüklemesi</span><span class="sxs-lookup"><span data-stu-id="a44ba-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="a44ba-105">EF çekirdek kullanmak için kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="a44ba-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="a44ba-106">Örneğin, SQL Server hedefleme yüklemeniz `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="a44ba-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="a44ba-107">Bkz: [veritabanı sağlayıcıları](../../core/providers/index.md) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="a44ba-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="a44ba-108">Geçişler kullanmayı planladığınıza sonra de yüklemeniz gerekir `Microsoft.EntityFrameworkCore.Tools` paket.</span><span class="sxs-lookup"><span data-stu-id="a44ba-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="a44ba-109">EF çekirdek ve EF6 kullanılan yan yana aynı uygulamada olabildiği kadar yüklü EF6 NuGet paketi (EntityFramework) bırakmak uygundur.</span><span class="sxs-lookup"><span data-stu-id="a44ba-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="a44ba-110">Uygulamanızın tüm alanlarda EF6 kullanmayı planlayan değil, ancak paket kaldırma dikkat etmeniz gereken kod parçalarını derleme hataları vermek yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="a44ba-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="a44ba-111">Ad alanlarını değiştirme</span><span class="sxs-lookup"><span data-stu-id="a44ba-111">Swap namespaces</span></span>

<span data-ttu-id="a44ba-112">İçinde EF6 kullanan çoğu API'leri bulunan `System.Data.Entity` ad alanı (ve ilgili alt ad alanları).</span><span class="sxs-lookup"><span data-stu-id="a44ba-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="a44ba-113">İlk kod için değiştirilecek değişikliktir `Microsoft.EntityFrameworkCore` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="a44ba-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="a44ba-114">Genellikle türetilmiş bağlam kod dosyanızla başlatın ve sonra bunlar ortaya çıktığında derleme hataları adresleme buradan, iş.</span><span class="sxs-lookup"><span data-stu-id="a44ba-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="a44ba-115">Bağlam yapılandırma (bağlantı vs.)</span><span class="sxs-lookup"><span data-stu-id="a44ba-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="a44ba-116">Bölümünde açıklandığı gibi [olun EF çekirdek olacak iş uygulamanız için](ensure-requirements.md), EF çekirdek bağlanmak için veritabanı algılama geçici daha az Sihirli sahiptir.</span><span class="sxs-lookup"><span data-stu-id="a44ba-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="a44ba-117">Geçersiz kılma gerekecek `OnConfiguring` yöntemi türetilmiş bağlamı ve veritabanı bağlantısı kurmak için veritabanı sağlayıcısı belirli API kullanın.</span><span class="sxs-lookup"><span data-stu-id="a44ba-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="a44ba-118">Çoğu EF6 uygulamaları bağlantı dizesi uygulamalarda depolamak `App/Web.config` dosya.</span><span class="sxs-lookup"><span data-stu-id="a44ba-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="a44ba-119">Bu bağlantı dizesini kullanarak okunur EF çekirdek `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="a44ba-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="a44ba-120">Bir başvuru eklemeniz gerekebilir `System.Configuration` framework derleme bu API kullanmanız mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="a44ba-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="a44ba-121">Kodunuzu güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="a44ba-121">Update your code</span></span>

<span data-ttu-id="a44ba-122">Bu noktada, derleme hataları adresleme ve davranış değişiklikleri, etkiler, görmek için kod gözden geçirme bir konudur.</span><span class="sxs-lookup"><span data-stu-id="a44ba-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="a44ba-123">Varolan geçişleri</span><span class="sxs-lookup"><span data-stu-id="a44ba-123">Existing migrations</span></span>

<span data-ttu-id="a44ba-124">Gerçekten var olan EF6 geçişler EF çekirdek için bağlantı noktası için uygun bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="a44ba-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="a44ba-125">Mümkünse, varsayar EF6 önceki tüm geçişleri veritabanına uygulanmış olan ve ardından EF çekirdek kullanarak şema değerinden geçirme başlangıç noktası en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="a44ba-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="a44ba-126">Bunu yapmak için kullanacağınız `Add-Migration` EF çekirdek modeli bağlantı noktası kurulmuş bir kez bir geçiş eklemek için komutu.</span><span class="sxs-lookup"><span data-stu-id="a44ba-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="a44ba-127">Ardından tüm koddan kaldırın `Up` ve `Down` kurulmuş geçiş yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="a44ba-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="a44ba-128">İlk geçişin kuruldu olduğunda sonraki geçişler modele karşılaştırın.</span><span class="sxs-lookup"><span data-stu-id="a44ba-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="a44ba-129">Bağlantı testi</span><span class="sxs-lookup"><span data-stu-id="a44ba-129">Test the port</span></span>

<span data-ttu-id="a44ba-130">Yalnızca uygulamanızı derler çünkü EF çekirdek başarıyla bağlantı noktalı anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="a44ba-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="a44ba-131">Test davranış değişiklikleri hiçbiri olumsuz uygulamanızı etkilenen olduğundan emin olmak için uygulamanızın tüm alanları gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="a44ba-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
