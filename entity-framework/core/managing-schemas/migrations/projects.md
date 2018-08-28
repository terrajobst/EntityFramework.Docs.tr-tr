---
title: Birden çok proje - EF Core ile geçişleri
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997993"
---
<a name="using-a-separate-project"></a><span data-ttu-id="79522-102">Ayrı proje kullanma</span><span class="sxs-lookup"><span data-stu-id="79522-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="79522-103">Geçiş tek içeren daha farklı bir derleme depolamak isteyebilirsiniz, `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="79522-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="79522-104">Sürüm yayın yükseltmeleri için birden fazla geçişler, örneğin korumak için bu strateji, geliştirme için diğeri de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="79522-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="79522-105">Bunu yapmak için...</span><span class="sxs-lookup"><span data-stu-id="79522-105">To do this...</span></span>

1. <span data-ttu-id="79522-106">Yeni bir sınıf kitaplığı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="79522-106">Create a new class library.</span></span>

2. <span data-ttu-id="79522-107">DbContext derlemenizin bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="79522-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="79522-108">Model anlık görüntü dosyaları ve geçişleri Sınıf Kitaplığı'na taşıyın.</span><span class="sxs-lookup"><span data-stu-id="79522-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="79522-109">Mevcut hiçbir geçiş varsa, bir DbContext içeren projede ardından taşıyın.</span><span class="sxs-lookup"><span data-stu-id="79522-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="79522-110">Bu önemlidir, çünkü geçişleri derleme var olan bir geçiş içermiyorsa geçiş Ekle komutunu DbContext bulamıyor.</span><span class="sxs-lookup"><span data-stu-id="79522-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="79522-111">Geçişleri derleme yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="79522-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="79522-112">Başlangıç derlemesinden, geçişler derlemesine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="79522-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="79522-113">Bu, döngüsel bağımlılığa neden olursa, sınıf kitaplığı çıkış yolunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="79522-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="79522-114">Her şeyin doğru şekilde kaldırdıysanız, projeye yeni bir geçiş eklemek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="79522-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
