---
title: "EDMX tabanlı modeli taşıma EF çekirdek - EF6 bağlantı noktası oluşturma"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 1880d5ce49d4c9cb891d0e8fb230770bb44799e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="19571-102">Bir EF6 EDMX tabanlı modeli EF çekirdek için bağlantı noktası oluşturma</span><span class="sxs-lookup"><span data-stu-id="19571-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="19571-103">EF çekirdek EDMX dosya biçimi modelleri için desteklemez.</span><span class="sxs-lookup"><span data-stu-id="19571-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="19571-104">Bu modeller bağlantı noktası için en iyi seçenektir veritabanından uygulamanız için yeni bir kod tabanlı model oluşturulamadı.</span><span class="sxs-lookup"><span data-stu-id="19571-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="19571-105">EF Core NuGet paketi yüklemesi</span><span class="sxs-lookup"><span data-stu-id="19571-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="19571-106">Yükleme `Microsoft.EntityFrameworkCore.Tools` NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="19571-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="19571-107">Modeli yeniden oluştur</span><span class="sxs-lookup"><span data-stu-id="19571-107">Regenerate the model</span></span>

<span data-ttu-id="19571-108">Ters mühendislik işlevselliği artık mevcut veritabanını temel alan bir model oluşturmak için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19571-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="19571-109">Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın (araçları –> NuGet Paket Yöneticisi Paket Yöneticisi konsolu –>).</span><span class="sxs-lookup"><span data-stu-id="19571-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="19571-110">Bkz: [Paket Yöneticisi Konsolu (Visual Studio)](../../core/miscellaneous/cli/powershell.md) komut seçenekleri tablolar vb. kümesini iskele kurmak için.</span><span class="sxs-lookup"><span data-stu-id="19571-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="19571-111">Örneğin, bir modeli, SQL Server yerel veritabanı örneğinde blog veritabanından iskele için komutu aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="19571-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="19571-112">EF6 modeli kaldırma</span><span class="sxs-lookup"><span data-stu-id="19571-112">Remove EF6 model</span></span>

<span data-ttu-id="19571-113">Artık EF6 modeli uygulamanızdan kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19571-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="19571-114">EF çekirdek ve EF6 kullanılan yan yana aynı uygulamada olabildiği kadar yüklü EF6 NuGet paketi (EntityFramework) bırakmak uygundur.</span><span class="sxs-lookup"><span data-stu-id="19571-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="19571-115">Uygulamanızın tüm alanlarda EF6 kullanmayı planlayan değil, ancak paket kaldırma dikkat etmeniz gereken kod parçalarını derleme hataları vermek yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="19571-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="19571-116">Kodunuzu güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="19571-116">Update your code</span></span>

<span data-ttu-id="19571-117">Bu noktada sağlasa da adresleme derleme hataları ve EF6 EF çekirdek arasındaki davranış değişiklikleri, etkileyip etkilemeyeceğini görmek için gözden geçirme kodu değil.</span><span class="sxs-lookup"><span data-stu-id="19571-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="19571-118">Bağlantı testi</span><span class="sxs-lookup"><span data-stu-id="19571-118">Test the port</span></span>

<span data-ttu-id="19571-119">Yalnızca uygulamanızı derler çünkü EF çekirdek başarıyla bağlantı noktalı anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="19571-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="19571-120">Test davranış değişiklikleri hiçbiri olumsuz uygulamanızı etkilenen olduğundan emin olmak için uygulamanızın tüm alanları gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="19571-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="19571-121">Bkz: [var olan bir veritabanı ile ASP.NET Core EF Çekirdeğinde ile çalışmaya başlama](xref:core/get-started/aspnetcore/existing-db) var olan bir veritabanı, çalışma konusunda ek bir başvuru için</span><span class="sxs-lookup"><span data-stu-id="19571-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
