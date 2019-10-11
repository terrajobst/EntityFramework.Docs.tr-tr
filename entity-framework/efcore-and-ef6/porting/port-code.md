---
title: EF6 'den EF Core taşıma-kod tabanlı model-EF 'e taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181223"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="2c589-102">EF6 kod tabanlı bir modelin EF Core için taşıma</span><span class="sxs-lookup"><span data-stu-id="2c589-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="2c589-103">Tüm uyarıları okuduğunuzda ve bağlantı noktasına hazırsanız, kullanmaya başlamanıza yardımcı olacak bazı yönergeler aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2c589-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="2c589-104">EF Core NuGet paketlerini yükler</span><span class="sxs-lookup"><span data-stu-id="2c589-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="2c589-105">EF Core kullanmak için, kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="2c589-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="2c589-106">Örneğin, SQL Server hedeflenirken, `Microsoft.EntityFrameworkCore.SqlServer` ' ı yüklersiniz.</span><span class="sxs-lookup"><span data-stu-id="2c589-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="2c589-107">Ayrıntılar için bkz. [veritabanı sağlayıcıları](../../core/providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="2c589-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="2c589-108">Geçişleri kullanmayı planlıyorsanız, `Microsoft.EntityFrameworkCore.Tools` paketini de yüklemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="2c589-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="2c589-109">EF Core ve EF6 'nin aynı uygulamada yan yana kullanılabilmesi için, EF6 NuGet paketini (EntityFramework) yüklü bırakmak çok iyidir.</span><span class="sxs-lookup"><span data-stu-id="2c589-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="2c589-110">Ancak, uygulamanızın herhangi bir alanında EF6 kullanmayı hiç bilmiyorsanız, paketin kaldırılması, dikkat edilmesi gereken kod parçalarında derleme hataları sağlamaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="2c589-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="2c589-111">Ad alanlarını değiştirme</span><span class="sxs-lookup"><span data-stu-id="2c589-111">Swap namespaces</span></span>

<span data-ttu-id="2c589-112">EF6 içinde kullandığınız çoğu API 'Ler `System.Data.Entity` ad alanında (ve ilgili alt ad alanlarında) bulunur.</span><span class="sxs-lookup"><span data-stu-id="2c589-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="2c589-113">İlk kod değişikliği `Microsoft.EntityFrameworkCore` ad alanına takas etmek.</span><span class="sxs-lookup"><span data-stu-id="2c589-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="2c589-114">Genellikle, türetilmiş bağlam kodu dosyanız ile başlayıp daha sonra, meydana gelen derleme hatalarını adresleyen bir şekilde ele almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c589-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="2c589-115">Bağlam yapılandırması (bağlantı vb.)</span><span class="sxs-lookup"><span data-stu-id="2c589-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="2c589-116">[EF Core uygulamanız Için çalıştığından emin olun](ensure-requirements.md), EF Core bağlanılacak veritabanını algılayarak daha az bir sihirli olur.</span><span class="sxs-lookup"><span data-stu-id="2c589-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="2c589-117">Türetilmiş bağlamınızda `OnConfiguring` yöntemini geçersiz kılmanız ve veritabanına bağlantıyı kurmak için veritabanı sağlayıcısına özgü API 'yi kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c589-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="2c589-118">Çoğu EF6 uygulaması bağlantı dizesini uygulamalar `App/Web.config` dosyasında depolar.</span><span class="sxs-lookup"><span data-stu-id="2c589-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="2c589-119">EF Core, `ConfigurationManager` API 'sini kullanarak bu bağlantı dizesini okuyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c589-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="2c589-120">Bu API 'yi kullanabilmeniz için `System.Configuration` çerçeve derlemesine bir başvuru eklemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="2c589-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="2c589-121">Kodunuzu güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="2c589-121">Update your code</span></span>

<span data-ttu-id="2c589-122">Bu noktada, bu durum derleme hatalarının adreslenmesi ve kodu gözden geçirerek davranış değişikliklerinin sizi etkileyebileceğini göz atalım.</span><span class="sxs-lookup"><span data-stu-id="2c589-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="2c589-123">Mevcut geçişler</span><span class="sxs-lookup"><span data-stu-id="2c589-123">Existing migrations</span></span>

<span data-ttu-id="2c589-124">EF Core için mevcut EF6 geçişlerini bağlantı noktası için uygun bir yol yoktur.</span><span class="sxs-lookup"><span data-stu-id="2c589-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="2c589-125">Mümkünse, EF6 ' den önceki tüm geçişlerin veritabanına uygulandığını varsaymak ve sonra EF Core kullanarak şemayı bu noktadan geçirmeye başlamanız en iyisidir.</span><span class="sxs-lookup"><span data-stu-id="2c589-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="2c589-126">Bunu yapmak için, model EF Core bağlantı kurulduktan sonra bir geçiş eklemek için `Add-Migration` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="2c589-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="2c589-127">Daha sonra, `Up` ' dan ve `Down` ' den, yapı iskelesi geçişinin tüm kodunu kaldırırsınız.</span><span class="sxs-lookup"><span data-stu-id="2c589-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="2c589-128">Sonraki geçişler, ilk geçiş iskele alındığı zaman modeliyle karşılaştırılır.</span><span class="sxs-lookup"><span data-stu-id="2c589-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="2c589-129">Bağlantı noktasını test etme</span><span class="sxs-lookup"><span data-stu-id="2c589-129">Test the port</span></span>

<span data-ttu-id="2c589-130">Uygulamanız derlendiğinden, EF Core başarılı bir şekilde bir şekilde bağlantı edildiği anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="2c589-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="2c589-131">Davranış değişikliklerinin hiçbirinin uygulamanızı olumsuz yönde etkilememesini sağlamak için uygulamanızın tüm bölümlerini test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2c589-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
