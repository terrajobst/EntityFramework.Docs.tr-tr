---
title: EF6 'den EF Core taşıma-EDMX tabanlı bir modeli taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: a1c978e141f47a39fff59eb5fc417a63afd37b04
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71148989"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="cc935-102">EF6 EDMX tabanlı bir modelin EF Core taşıma</span><span class="sxs-lookup"><span data-stu-id="cc935-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="cc935-103">EF Core modeller için EDMX dosya biçimini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="cc935-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="cc935-104">Bu modellerin bağlantı noktası için en iyi seçenek, uygulamanız için veritabanından yeni bir kod tabanlı model oluşturmak içindir.</span><span class="sxs-lookup"><span data-stu-id="cc935-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="cc935-105">EF Core NuGet paketlerini yükler</span><span class="sxs-lookup"><span data-stu-id="cc935-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="cc935-106">`Microsoft.EntityFrameworkCore.Tools` NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="cc935-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="cc935-107">Modeli yeniden oluştur</span><span class="sxs-lookup"><span data-stu-id="cc935-107">Regenerate the model</span></span>

<span data-ttu-id="cc935-108">Artık, mevcut veritabanınıza dayalı bir model oluşturmak için ters mühendislik işlevini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cc935-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="cc935-109">Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın (Araçlar – > NuGet Paket Yöneticisi – > Paket Yöneticisi konsolu).</span><span class="sxs-lookup"><span data-stu-id="cc935-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="cc935-110">Tabloların bir alt kümesini kankatlamak için bkz. komut seçenekleri için [Paket Yöneticisi Konsolu (Visual Studio)](../../core/miscellaneous/cli/powershell.md) .</span><span class="sxs-lookup"><span data-stu-id="cc935-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="cc935-111">Örneğin, SQL Server LocalDB örneğinizdeki blog veritabanından bir modeli dolandırın bir komut aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cc935-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="cc935-112">EF6 modelini kaldır</span><span class="sxs-lookup"><span data-stu-id="cc935-112">Remove EF6 model</span></span>

<span data-ttu-id="cc935-113">Artık EF6 modelini uygulamanızdan kaldırırsınız.</span><span class="sxs-lookup"><span data-stu-id="cc935-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="cc935-114">EF Core ve EF6 'nin aynı uygulamada yan yana kullanılabilmesi için, EF6 NuGet paketini (EntityFramework) yüklü bırakmak çok iyidir.</span><span class="sxs-lookup"><span data-stu-id="cc935-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="cc935-115">Ancak, uygulamanızın herhangi bir alanında EF6 kullanmayı hiç bilmiyorsanız, paketin kaldırılması, dikkat edilmesi gereken kod parçalarında derleme hataları sağlamaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="cc935-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="cc935-116">Kodunuzu güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="cc935-116">Update your code</span></span>

<span data-ttu-id="cc935-117">Bu noktada derleme hatalarının adreslenmesi ve kodu gözden geçirmesinden bağımsız olarak, EF6 ve EF Core arasındaki davranış değişikliklerinin sizi etkileyebileceğini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="cc935-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="cc935-118">Bağlantı noktasını test etme</span><span class="sxs-lookup"><span data-stu-id="cc935-118">Test the port</span></span>

<span data-ttu-id="cc935-119">Uygulamanız derlendiğinden, EF Core başarılı bir şekilde bir şekilde bağlantı edildiği anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="cc935-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="cc935-120">Davranış değişikliklerinin hiçbirinin uygulamanızı olumsuz yönde etkilememesini sağlamak için uygulamanızın tüm bölümlerini test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="cc935-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
