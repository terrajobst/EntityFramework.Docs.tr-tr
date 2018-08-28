---
title: EF6'dan EF Core'a taşıma - EDMX tabanlı bir modeli taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997417"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="94ee7-102">EF6 EDMX tabanlı bir modeli EF core'a taşıma</span><span class="sxs-lookup"><span data-stu-id="94ee7-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="94ee7-103">EF Core modelleri için EDMX dosya biçimini desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="94ee7-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="94ee7-104">Bu modeller bağlantı noktası için en iyi seçenektir veritabanından uygulamanız için yeni bir kod tabanlı modeli oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="94ee7-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="94ee7-105">EF Core NuGet paketi yüklemesi</span><span class="sxs-lookup"><span data-stu-id="94ee7-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="94ee7-106">Yükleme `Microsoft.EntityFrameworkCore.Tools` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="94ee7-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="94ee7-107">Modeli yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="94ee7-107">Regenerate the model</span></span>

<span data-ttu-id="94ee7-108">Ayrıca, varolan bir veritabanını temel alan bir model oluşturmak için artık ters mühendislik işlevlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="94ee7-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="94ee7-109">Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın (araçları –> NuGet Paket Yöneticisi Paket Yöneticisi konsolu –>).</span><span class="sxs-lookup"><span data-stu-id="94ee7-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="94ee7-110">Bkz: [Paket Yöneticisi Konsolu (Visual Studio)](../../core/miscellaneous/cli/powershell.md) tablolar vb. kümesini iskele komut seçenekleri için.</span><span class="sxs-lookup"><span data-stu-id="94ee7-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="94ee7-111">Örneğin, bir model, SQL Server LocalDB örneğini blog veritabanından iskele komutu aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="94ee7-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="94ee7-112">EF6 modeli kaldırma</span><span class="sxs-lookup"><span data-stu-id="94ee7-112">Remove EF6 model</span></span>

<span data-ttu-id="94ee7-113">Artık uygulamanızı EF6 modeli kaldırmak isteyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="94ee7-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="94ee7-114">EF Core ve EF6 kullanılan yan yana aynı uygulamada olması gibi yüklü EF6 NuGet paketini (EntityFramework) bırakmak uygundur.</span><span class="sxs-lookup"><span data-stu-id="94ee7-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="94ee7-115">Uygulamanızın tüm alanlarda EF6'ı kullanmayı planlayan değildir, ancak sonra paket kaldırılıyor dikkat etmeniz gereken kod parçaları derleme hatalarını vermek yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="94ee7-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="94ee7-116">Kodunuzu güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="94ee7-116">Update your code</span></span>

<span data-ttu-id="94ee7-117">Bu noktada, birkaç adresleme derleme hataları ve EF6 ve EF Core arasındaki davranış değişiklikleri, etkileyip etkilemeyeceğini görmek için gözden geçirme kod değil.</span><span class="sxs-lookup"><span data-stu-id="94ee7-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="94ee7-118">Bağlantı testi</span><span class="sxs-lookup"><span data-stu-id="94ee7-118">Test the port</span></span>

<span data-ttu-id="94ee7-119">Yalnızca uygulamanızı derleyen çünkü başarıyla EF Core için unity'nin anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="94ee7-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="94ee7-120">Tüm alanları davranışı değişikliklerin hiçbiri olumsuz uygulamanızı etkilemiş emin olmak için uygulamanızı test etmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="94ee7-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="94ee7-121">Bkz: [var olan bir veritabanı ile ASP.NET Core üzerinde EF Core ile çalışmaya başlama](xref:core/get-started/aspnetcore/existing-db) var olan bir veritabanı ile çalışma konusunda ek bir başvuru</span><span class="sxs-lookup"><span data-stu-id="94ee7-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
