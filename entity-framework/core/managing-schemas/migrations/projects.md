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
# <a name="using-a-separate-migrations-project"></a>Ayrı geçişler projesi kullanma

Geçişlerinizi `DbContext`sahip olandan farklı bir derlemede depolamak isteyebilirsiniz. Bu stratejiyi Ayrıca birden çok geçiş kümesini (örneğin, geliştirme için bir diğeri de yayından yayın yükseltmeleri için başka bir şekilde) sürdürmek için de kullanabilirsiniz.

Bunu yapmak için...

1. Yeni bir sınıf kitaplığı oluşturun.

2. DbContext derlemenizin başvurusunu ekleyin.

3. Geçişler ve model anlık görüntü dosyalarını sınıf kitaplığına taşıyın.
   > [!TIP]
   > Zaten geçiş yaptıysanız, DbContext 'i içeren projede bir tane oluşturun ve taşıyın.
   > Bu önemlidir çünkü geçişler derlemesi var olan bir geçiş içermiyorsa, Add-Migration komutu DbContext 'i bulamaz.

4. Geçişler derlemesini yapılandırın:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Başlangıç derlemesinden geçiş derlemenizin başvurusunu ekleyin.
   * Bu, döngüsel bağımlılığa neden olursa, sınıf kitaplığının çıkış yolunu güncelleştirin:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Her şeyi doğru yaptıysanız, projeye yeni geçişler ekleyebilmelisiniz.

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
