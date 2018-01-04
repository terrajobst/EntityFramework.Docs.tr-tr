---
title: "Birden çok proje - EF çekirdek ile geçişleri"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
<a name="using-a-separate-project"></a><span data-ttu-id="f8b25-102">Ayrı bir proje kullanma</span><span class="sxs-lookup"><span data-stu-id="f8b25-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="f8b25-103">Bir içeren daha farklı bir derleme, geçişler depolamak isteyebilirsiniz, `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f8b25-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="f8b25-104">Sürüm yayın yükseltmeleri için birden çok kümesini geçişler, örneğin korumak için bu strateji, biri geliştirme diğeri için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f8b25-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="f8b25-105">Bunu yapmak için...</span><span class="sxs-lookup"><span data-stu-id="f8b25-105">To do this...</span></span>

1. <span data-ttu-id="f8b25-106">Yeni bir sınıf kitaplığı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f8b25-106">Create a new class library.</span></span>

2. <span data-ttu-id="f8b25-107">DbContext derlemesine başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f8b25-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="f8b25-108">Sınıf Kitaplığı'na geçiş ve model anlık görüntü dosyaları taşıyın.</span><span class="sxs-lookup"><span data-stu-id="f8b25-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="f8b25-109">Eklemediniz, bir DbContext projeye ekleyin, sonra taşıyın.</span><span class="sxs-lookup"><span data-stu-id="f8b25-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="f8b25-110">Geçişler derleme yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="f8b25-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="f8b25-111">Geçişler derlemenizi başlangıç derlemesinden bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f8b25-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="f8b25-112">Bu döngüsel bağımlılığa yol açıyorsa, sınıf kitaplığı çıkış yolu güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="f8b25-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="f8b25-113">Her şeyin doğru şekilde kaldırdıysanız, projeye yeni geçişler eklemeniz mümkün olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="f8b25-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
