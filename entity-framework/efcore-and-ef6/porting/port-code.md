---
title: EF6'dan EF Core'a Taşıma - Kod Tabanlı Model Taşıma - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419639"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="61b8d-102">EF6 Kod Tabanlı Modeli EF Core'a Taşıma</span><span class="sxs-lookup"><span data-stu-id="61b8d-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="61b8d-103">Tüm uyarılar okuduysanız ve bağlantı noktasına gitmeye hazırsanız, başlamanıza yardımcı olacak bazı yönergeler burada dır.</span><span class="sxs-lookup"><span data-stu-id="61b8d-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="61b8d-104">EF Core NuGet paketlerini yükleyin</span><span class="sxs-lookup"><span data-stu-id="61b8d-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="61b8d-105">EF Core'u kullanmak için Kullanmak Istediğiniz veritabanı sağlayıcısı için NuGet paketini yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="61b8d-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="61b8d-106">Örneğin, SQL Server'ı hedeflerken. `Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="61b8d-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="61b8d-107">Ayrıntılar için [Veritabanı Sağlayıcıları'na](../../core/providers/index.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="61b8d-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="61b8d-108">Geçişleri kullanmayı planlıyorsanız, `Microsoft.EntityFrameworkCore.Tools` paketi de yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="61b8d-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="61b8d-109">EF Core ve EF6 aynı uygulamada yan yana kullanılabildiği için EF6 NuGet paketini (EntityFramework) yüklü bırakmak iyidir.</span><span class="sxs-lookup"><span data-stu-id="61b8d-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="61b8d-110">Ancak, UYGULAMANIZIN herhangi bir alanında EF6'yı kullanmak istemiyorsanız, paketi kaldırmanız dikkat gerektiren kod parçaları üzerinde derleme hataları na yardımcı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="61b8d-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="61b8d-111">Ad boşluklarını değiştirme</span><span class="sxs-lookup"><span data-stu-id="61b8d-111">Swap namespaces</span></span>

<span data-ttu-id="61b8d-112">EF6'da kullandığınız API'lerin `System.Data.Entity` çoğu ad alanında (ve ilgili alt ad alanlarında) bulunur.</span><span class="sxs-lookup"><span data-stu-id="61b8d-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="61b8d-113">İlk kod değişikliği `Microsoft.EntityFrameworkCore` ad alanına geçiş yapmaktır.</span><span class="sxs-lookup"><span data-stu-id="61b8d-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="61b8d-114">Genellikle türemiş bağlam kodu dosyanızla başlar ve ardından derleme hatalarını oluştukça ele alınarak buradan çalışırsınız.</span><span class="sxs-lookup"><span data-stu-id="61b8d-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="61b8d-115">Bağlam yapılandırması (bağlantı vb.)</span><span class="sxs-lookup"><span data-stu-id="61b8d-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="61b8d-116">[EF Core'un Uygulamanız için Çalışacağını Garanti](ensure-requirements.md)Edin'de açıklandığı gibi, EF Core bağlanmak için veritabanını algılama konusunda daha az sihrivardır.</span><span class="sxs-lookup"><span data-stu-id="61b8d-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="61b8d-117">Türemiş bağlamınızdaki `OnConfiguring` yöntemi geçersiz kılmanız ve veritabanına bağlantıyı kurmak için veritabanı sağlayıcısına özgü API kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="61b8d-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="61b8d-118">ÇOĞU EF6 uygulaması bağlantı dizesini `App/Web.config` uygulamalar dosyasında depolar.</span><span class="sxs-lookup"><span data-stu-id="61b8d-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="61b8d-119">EF Core'da, API'yi `ConfigurationManager` kullanarak bu bağlantı dizesini okudunuz.</span><span class="sxs-lookup"><span data-stu-id="61b8d-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="61b8d-120">Bu API'yi kullanabilmek için `System.Configuration` çerçeve derlemesine bir başvuru eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="61b8d-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="61b8d-121">Kodunuzu güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="61b8d-121">Update your code</span></span>

<span data-ttu-id="61b8d-122">Bu noktada, davranış değişikliklerinin sizi etkileyip etkilemeyeceğini görmek için derleme hatalarını ele alma ve kodu gözden geçirme meselesidir.</span><span class="sxs-lookup"><span data-stu-id="61b8d-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="61b8d-123">Varolan geçişler</span><span class="sxs-lookup"><span data-stu-id="61b8d-123">Existing migrations</span></span>

<span data-ttu-id="61b8d-124">Mevcut EF6 geçişlerini EF Core'a taşımanın uygun bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="61b8d-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="61b8d-125">Mümkünse, EF6'dan önceki tüm geçişlerin veritabanına uygulandığını varsaymak ve daha sonra EF Core kullanarak şemayı bu noktadan geçirerek başlamak en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="61b8d-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="61b8d-126">Bunu yapmak için, model `Add-Migration` EF Core'a taşınırken geçiş eklemek için komutu kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="61b8d-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="61b8d-127">Daha sonra iskele geçişive `Up` `Down` yöntemleri tüm kodu kaldırmak istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="61b8d-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="61b8d-128">Sonraki geçişler, ilk geçiş indiğinde modelle karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="61b8d-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="61b8d-129">Bağlantı noktasını test edin</span><span class="sxs-lookup"><span data-stu-id="61b8d-129">Test the port</span></span>

<span data-ttu-id="61b8d-130">Uygulamanızın derlenmiş olması, uygulamanın ef core'a başarıyla iletilmiş olduğu anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="61b8d-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="61b8d-131">Davranış değişikliklerinin hiçbirinin uygulamanızı olumsuz etkilemediğinden emin olmak için uygulamanızın tüm alanlarını test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="61b8d-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
