---
title: Ayrı geçişler projesi kullanma-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0082b0af2905fe9e5c3c6509516f622c9d4f8370
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812038"
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

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
