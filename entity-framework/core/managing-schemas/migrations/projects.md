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
<a name="using-a-separate-project"></a>Ayrı proje kullanma
========================
Geçiş tek içeren daha farklı bir derleme depolamak isteyebilirsiniz, `DbContext`. Sürüm yayın yükseltmeleri için birden fazla geçişler, örneğin korumak için bu strateji, geliştirme için diğeri de kullanabilirsiniz.

Bunu yapmak için...

1. Yeni bir sınıf kitaplığı oluşturun.

2. DbContext derlemenizin bir başvuru ekleyin.

3. Model anlık görüntü dosyaları ve geçişleri Sınıf Kitaplığı'na taşıyın.
   > [!TIP]
   > Mevcut hiçbir geçiş varsa, bir DbContext içeren projede ardından taşıyın. Bu önemlidir, çünkü geçişleri derleme var olan bir geçiş içermiyorsa geçiş Ekle komutunu DbContext bulamıyor.

4. Geçişleri derleme yapılandırın:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Başlangıç derlemesinden, geçişler derlemesine bir başvuru ekleyin.
   * Bu, döngüsel bağımlılığa neden olursa, sınıf kitaplığı çıkış yolunu güncelleştirin:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Her şeyin doğru şekilde kaldırdıysanız, projeye yeni bir geçiş eklemek mümkün olmayacaktır.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
