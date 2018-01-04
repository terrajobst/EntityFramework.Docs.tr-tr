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
<a name="using-a-separate-project"></a>Ayrı bir proje kullanma
========================
Bir içeren daha farklı bir derleme, geçişler depolamak isteyebilirsiniz, `DbContext`. Sürüm yayın yükseltmeleri için birden çok kümesini geçişler, örneğin korumak için bu strateji, biri geliştirme diğeri için de kullanabilirsiniz.

Bunu yapmak için...

1. Yeni bir sınıf kitaplığı oluşturun.

2. DbContext derlemesine başvuru ekleyin.

3. Sınıf Kitaplığı'na geçiş ve model anlık görüntü dosyaları taşıyın.
   * Eklemediniz, bir DbContext projeye ekleyin, sonra taşıyın.

4. Geçişler derleme yapılandırın:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Geçişler derlemenizi başlangıç derlemesinden bir başvuru ekleyin.
   * Bu döngüsel bağımlılığa yol açıyorsa, sınıf kitaplığı çıkış yolu güncelleştirin:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Her şeyin doğru şekilde kaldırdıysanız, projeye yeni geçişler eklemeniz mümkün olması gerekir.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
