---
title: EF6'dan EF Core'a taşıma - EDMX Tabanlı Model Taşıma - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416925"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="44f51-102">EF6 EDMX Tabanlı Modeli EF Core'a Taşıma</span><span class="sxs-lookup"><span data-stu-id="44f51-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="44f51-103">EF Core, modeller için EDMX dosya biçimini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="44f51-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="44f51-104">Bu modelleri taşımanın en iyi seçeneği, uygulamanız için veritabanından yeni bir kod tabanlı model oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="44f51-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="44f51-105">EF Core NuGet paketlerini yükleyin</span><span class="sxs-lookup"><span data-stu-id="44f51-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="44f51-106">NuGet `Microsoft.EntityFrameworkCore.Tools` paketini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="44f51-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="44f51-107">Modeli yeniden oluşturma</span><span class="sxs-lookup"><span data-stu-id="44f51-107">Regenerate the model</span></span>

<span data-ttu-id="44f51-108">Artık varolan veritabanınızı temel alan bir model oluşturmak için ters mühendislik işlevini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44f51-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="44f51-109">Paket Yöneticisi Konsolu'nda (Araçlar –> NuGet Paket Yöneticisi –> Paket Yöneticisi Konsolu) aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="44f51-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="44f51-110">Tabloların bir alt kümesini iskelek için komut seçenekleri için [Paket Yöneticisi Konsolu 'na (Visual Studio)](../../core/miscellaneous/cli/powershell.md) bakın.</span><span class="sxs-lookup"><span data-stu-id="44f51-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="44f51-111">Örneğin, SQL Server LocalDB örneğiniz üzerindeki Blog veritabanından bir modeli iskeleoluşturma komutu burada dır.</span><span class="sxs-lookup"><span data-stu-id="44f51-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="44f51-112">EF6 modelini kaldırma</span><span class="sxs-lookup"><span data-stu-id="44f51-112">Remove EF6 model</span></span>

<span data-ttu-id="44f51-113">Şimdi EF6 modelini uygulamanızdan kaldırırsınız.</span><span class="sxs-lookup"><span data-stu-id="44f51-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="44f51-114">EF Core ve EF6 aynı uygulamada yan yana kullanılabildiği için EF6 NuGet paketini (EntityFramework) yüklü bırakmak iyidir.</span><span class="sxs-lookup"><span data-stu-id="44f51-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="44f51-115">Ancak, UYGULAMANIZIN herhangi bir alanında EF6'yı kullanmak istemiyorsanız, paketi kaldırmanız dikkat gerektiren kod parçaları üzerinde derleme hataları na yardımcı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="44f51-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="44f51-116">Kodunuzu güncelleştirin</span><span class="sxs-lookup"><span data-stu-id="44f51-116">Update your code</span></span>

<span data-ttu-id="44f51-117">Bu noktada, EF6 ve EF Core arasındaki davranış değişikliklerinin sizi etkileyip etkilemeyeceğini görmek için derleme hatalarını ele alma ve kodu gözden geçirme meselesidir.</span><span class="sxs-lookup"><span data-stu-id="44f51-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="44f51-118">Bağlantı noktasını test edin</span><span class="sxs-lookup"><span data-stu-id="44f51-118">Test the port</span></span>

<span data-ttu-id="44f51-119">Uygulamanızın derlenmiş olması, uygulamanın ef core'a başarıyla iletilmiş olduğu anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="44f51-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="44f51-120">Davranış değişikliklerinin hiçbirinin uygulamanızı olumsuz etkilemediğinden emin olmak için uygulamanızın tüm alanlarını test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="44f51-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
