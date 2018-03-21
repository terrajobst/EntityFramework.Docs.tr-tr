---
title: "Birden çok sağlayıcı - EF çekirdek ile geçişleri"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a>Birden çok sağlayıcı ile geçişleri
==================================
[EF çekirdek Araçları] [ 1] yalnızca geçişler etkin sağlayıcı için iskele. Bazı durumlarda, ancak, birden fazla sağlayıcı (örneğin, Microsoft SQL Server ve SQLite) ile sizin DbContext kullanmak isteyebilirsiniz. Bu geçiş ile işlemek için iki yolu vardır. İki bulundurabilirsiniz geçirilmesi--her sağlayıcısı--veya birleştirme bunları tek bir ayarlamak için bir tane hem de çalışabilir.

<a name="two-migration-sets"></a>İki geçiş kümeleri
------------------
İlk yaklaşım, her bir model değişikliği için iki geçiş oluşturur.

Yapmanın bir yolu her bir geçiş kümesi yerleştirilecek budur [ayrı bir derleme içindeki] [ 2] ve iki geçiş ekleme arasında etkin sağlayıcısı (ve geçişler derleme) el ile geçiş yapın.

Araçlar ile çalışmayı kolaylaştırır başka bir yaklaşım active sağlayıcısı geçersiz kılar ve DbContext öğesinden türetilen yeni bir tür oluşturmaktır. Bu tür tasarım sırasında kullanılan zaman eklerken veya geçişler uygulanıyor.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Her bir geçiş kümesi kendi DbContext türleri kullandığından, bu yaklaşım ayrı geçişler derlemeyi kullanarak gerektirmez.

Yeni geçiş eklenirken içerik türlerini belirtin.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Son eşdüzey olarak oluşturulduğundan sonraki geçişler için çıktı dizini belirtmeniz gerekmez.

<a name="one-migration-set"></a>Bir geçiş ayarlama
-----------------
Geçişler iki kümesi sahip hoşlanmıyorsanız, el ile bunları hem sağlayıcılarına uygulanabilir tek bir kümesine birleştirebilirsiniz.

Bir sağlayıcı, anlamıyor tüm ek açıklamaları yok sayar beri ek açıklamaları bulunabilir. Örneğin, hem Microsoft SQL Server hem de SQLite ile çalışır birincil bir anahtar sütunu şöyle görünebilir.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

İşlemleri yalnızca bir sağlayıcısında uygulanabilir (veya farklı sağlayıcılar arasında oldukları varsa) kullanmak `ActiveProvider` hangi sağlayıcısı etkin olup özelliği.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
