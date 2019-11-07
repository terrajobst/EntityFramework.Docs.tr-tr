---
title: Ayrı geçişler projesi kullanma-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0c08855db77470d28e23f9ef1d147497dfcdff83
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655552"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="616d4-102">Ayrı geçişler projesi kullanma</span><span class="sxs-lookup"><span data-stu-id="616d4-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="616d4-103">Geçişlerinizi `DbContext`sahip olandan farklı bir derlemede depolamak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="616d4-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="616d4-104">Bu stratejiyi Ayrıca birden çok geçiş kümesini (örneğin, geliştirme için bir diğeri de yayından yayın yükseltmeleri için başka bir şekilde) sürdürmek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="616d4-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="616d4-105">Bunu yapmak için...</span><span class="sxs-lookup"><span data-stu-id="616d4-105">To do this...</span></span>

1. <span data-ttu-id="616d4-106">Yeni bir sınıf kitaplığı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="616d4-106">Create a new class library.</span></span>

2. <span data-ttu-id="616d4-107">DbContext derlemenizin başvurusunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="616d4-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="616d4-108">Geçişler ve model anlık görüntü dosyalarını sınıf kitaplığına taşıyın.</span><span class="sxs-lookup"><span data-stu-id="616d4-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="616d4-109">Zaten geçiş yaptıysanız, DbContext 'i içeren projede bir tane oluşturun ve taşıyın.</span><span class="sxs-lookup"><span data-stu-id="616d4-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="616d4-110">Bu önemlidir çünkü geçişler derlemesi var olan bir geçiş içermiyorsa, Add-Migration komutu DbContext 'i bulamaz.</span><span class="sxs-lookup"><span data-stu-id="616d4-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="616d4-111">Geçişler derlemesini yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="616d4-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="616d4-112">Başlangıç derlemesinden geçiş derlemenizin başvurusunu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="616d4-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="616d4-113">Bu, döngüsel bağımlılığa neden olursa, sınıf kitaplığının çıkış yolunu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="616d4-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="616d4-114">Her şeyi doğru yaptıysanız, projeye yeni geçişler ekleyebilmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="616d4-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="616d4-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="616d4-115">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="616d4-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="616d4-116">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
