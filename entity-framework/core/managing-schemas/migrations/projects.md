---
title: Birden çok proje - EF Core ile geçişleri
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 30a6afad1488e74ce2585be3d780186311379a97
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447150"
---
<a name="using-a-separate-project"></a><span data-ttu-id="f9c1e-102">Ayrı proje kullanma</span><span class="sxs-lookup"><span data-stu-id="f9c1e-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="f9c1e-103">Geçiş tek içeren daha farklı bir derleme depolamak isteyebilirsiniz, `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f9c1e-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="f9c1e-104">Sürüm yayın yükseltmeleri için birden fazla geçişler, örneğin korumak için bu strateji, geliştirme için diğeri de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f9c1e-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="f9c1e-105">Bunu yapmak için...</span><span class="sxs-lookup"><span data-stu-id="f9c1e-105">To do this...</span></span>

1. <span data-ttu-id="f9c1e-106">Yeni bir sınıf kitaplığı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f9c1e-106">Create a new class library.</span></span>

2. <span data-ttu-id="f9c1e-107">DbContext derlemenizin bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f9c1e-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="f9c1e-108">Model anlık görüntü dosyaları ve geçişleri Sınıf Kitaplığı'na taşıyın.</span><span class="sxs-lookup"><span data-stu-id="f9c1e-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="f9c1e-109">Mevcut hiçbir geçiş varsa, bir DbContext içeren projede ardından taşıyın.</span><span class="sxs-lookup"><span data-stu-id="f9c1e-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="f9c1e-110">Bu önemlidir, çünkü geçişleri derleme var olan bir geçiş içermiyorsa geçiş Ekle komutunu DbContext bulamıyor.</span><span class="sxs-lookup"><span data-stu-id="f9c1e-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="f9c1e-111">Geçişleri derleme yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="f9c1e-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="f9c1e-112">Başlangıç derlemesinden, geçişler derlemesine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f9c1e-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="f9c1e-113">Bu, döngüsel bağımlılığa neden olursa, sınıf kitaplığı çıkış yolunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="f9c1e-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="f9c1e-114">Her şeyin doğru şekilde kaldırdıysanız, projeye yeni bir geçiş eklemek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="f9c1e-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
