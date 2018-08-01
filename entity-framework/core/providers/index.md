---
title: Veritabanı sağlayıcıları - EF Core
author: rowanmiller
ms.author: divega
ms.date: 2/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: f51304a20bab2c80d2d546fc4685da0fa28d5f92
ms.sourcegitcommit: 5c2634c546720902fe01935f4fc031d73aa3ebde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393756"
---
# <a name="database-providers"></a>Veritabanı Sağlayıcıları

Entity Framework Core birçok farklı veritabanı, veritabanı sağlayıcı olarak adlandırılan eklenti kitaplıkları erişebilir.

## <a name="current-providers"></a>Geçerli sağlayıcıları
> [!IMPORTANT]  
> EF Core sağlayıcıları, çeşitli kaynaklardan tarafından oluşturulur. Tüm sağlayıcıları bir parçası olarak korunur [Entity Framework Core projesi](https://github.com/aspnet/EntityFrameworkCore). Bir sağlayıcı dikkate alındığında, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirdiğinizden emin olun. Ayrıca, her sağlayıcının belgelerine ayrıntılı sürüm uyumluluk bilgilerini gözden emin olun.

| NuGet paketi                                                                                                        | Desteklenen her veritabanı motoru | Bakımcı / satıcı                                                           | Notları / gereksinimleri           | Faydalı bağlantılar                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:-------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 ve sonraki sürümler    | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                | [Docs](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 ve sonraki sürümler         | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                | [Docs](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | EF Core bellek içi veritabanı | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Yalnızca test etmek için               | [Docs](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Npgsql geliştirme ekibi](https://github.com/npgsql)                          |                                | [Docs](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation proje](https://github.com/PomeloFoundation)              |                                | [Benioku dosyası](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT sunucusu               | [Pomelo Foundation proje](https://github.com/PomeloFoundation)              | EF Core 1.1 kadar yayın öncesi | [Benioku dosyası](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                 | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                 | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [MySQL proje](http://dev.mysql.com) (Oracle)                                | Yayın öncesi                    | [Docs](https://dev.mysql.com/doc/connector-net/en/)                                                                                                                                                |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 ve 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 | EF Core 2.0 ve sonraki sürümler            | [Docs](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 ve 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           | EF Core 2.0 ve sonraki sürümler            | [Wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [IBM. EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2 Informix              | [IBM](https://ibm.com)                                                        | Windows sürümü                | [blogu](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM. EntityFrameworkCore lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2 Informix              | [IBM](https://ibm.com)                                                        | Linux sürümü                  | [blogu](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM. EntityFrameworkCore osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2 Informix              | [IBM](https://ibm.com)                                                        | macOS sürümü                  | [blogu](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle 9.2.0.4 ve sonraki sürümler     | [DevArt](https://www.devart.com/)                                             | Ücretli                           | [Docs](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 ve üzeri     | [DevArt](https://www.devart.com/)                                             | Ücretli                           | [Docs](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 ve sonraki sürümler           | [DevArt](https://www.devart.com/)                                             | Ücretli                           | [Docs](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 ve sonraki sürümler            | [DevArt](https://www.devart.com/)                                             | Ücretli                           | [Docs](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Microsoft Access dosyası     | [Bubi](https://github.com/bubibubi)                                           | EF Core 2.0, .NET Framework    | [Benioku dosyası](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |

## <a name="future-providers"></a>Gelecekteki sağlayıcıları

### <a name="cosmos-db"></a>Cosmos DB

Biz, Cosmos DB DocumentDB API'si için bir EF Core sağlayıcısı geliştirme. Bu ilk tam belge yönelimli veritabanı sağlayıcısı üretilen olacak ve 2.1 sonra sonraki bir sürümünü tasarımındaki iyileştirmeler bildirmek için bu çalışma alanından dersleri yapacaksınız. Geçerli planın önizlemesini erkenden Sağlayıcısı'nın 2.1 profilleri'nde yayımlamaktır.

### <a name="oracle"></a>Oracle
Oracle .NET ekibi, 2018 üçüncü çeyrek içinde yaklaşık bir birinci taraf sağlayıcısı EF Core 2.0 için yayımlamayı planlama duyurdu. Bkz: kendi [.NET Core ve Entity Framework Core yönünü deyiminin](http://www.oracle.com/technetwork/topics/dotnet/tech-info/odpnet-dotnet-ef-core-sod-4395108.pdf) daha fazla bilgi için.
Lütfen bu sağlayıcı için yayın zaman çizelgesi dahil olmak üzere hakkında herhangi bir sorunuz doğrudan [Oracle topluluk sitesine](https://community.oracle.com/).

Bu arada, EF ekibin ürettiği bir [Oracle veritabanları için örnek EF Core sağlayıcısı](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/OracleProvider). Proje amacı adresi Oracle daha iyi desteklemek için gereken EF Core'nın ilişkisel ve temel işlevleri ve hızlı bir başlangıç yapmak için Microsoft'a ait bir EF Core sağlayıcısı oluşturmak için ancak belirlemenize yardımcı olmak için geliştirmeye yönelik diğer Oracle boşluklar değil Oracle veya üçüncü taraflar tarafından EF Core için sağlayıcıları.

Biz, örnek uygulamada geliştirilmesine katkı dikkate alacaktır. Biz ayrıca Hoş Geldiniz ve EF Core, örneği başlangıç noktası kullanan bir açık kaynak Oracle sağlayıcısı oluşturmak için bir topluluk çaba teşvik edin.

## <a name="adding-a-database-provider-to-your-application"></a>Veritabanı sağlayıcısı uygulamanıza ekleme

EF Core için çoğu veritabanı sağlayıcıları NuGet paketleri olarak dağıtılır. Yani bunlar kullanılarak yüklenebilir `dotnet` aracında komut satırı:

``` console
dotnet add package provider_package_name
```

Veya Visual Studio'da NuGet Paket Yöneticisi konsolu kullanarak:

``` powershell
install-package provider_package_name
```

Sağlayıcı yüklendikten sonra yapılandıracağınız, `DbContext`, ya da içinde `OnConfiguring` yöntemi veya `AddDbContext` bir bağımlılık ekleme kapsayıcısını kullanıyorsanız yöntemi. Örneğin, aşağıdaki satırı geçirilen bağlantı dizesi ile SQL Server sağlayıcısı yapılandırır:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Veritabanı sağlayıcıları belirli veritabanlarına benzersiz işlevselliğini etkinleştirmek için EF Core genişletebilirsiniz. Bazı kavramları çoğu veritabanı için ortak olan ve birincil EF Core bileşenlerinde yer alır. Bu kavramlar, LINQ, işlemleri, sorguların belirtme ve veritabanından yüklenmeden sonra değişiklikleri izleme nesneleri için içerir. Bazı kavramları, belirli bir sağlayıcıya özgü. Örneğin, SQL Server sağlayıcısı sağlar [bellek için iyileştirilmiş tablolar yapılandırma](xref:core/providers/sql-server/memory-optimized-tables) (özgü bir özellik için SQL Server). Diğer kavramlar sağlayıcılarının bir sınıfa özgüdür. Örneğin, ilişkisel veritabanları için EF Core sağlayıcıları üzerinde ortak yapı `Microsoft.EntityFrameworkCore.Relational` kitaplığı, tablo ve sütun eşlemeleri, yabancı anahtar kısıtlamalarını yapılandırma API'leri sağlar. Sağlayıcıları, genellikle NuGet paketleri olarak dağıtılır.

> [!IMPORTANT]  
> EF Core yeni bir düzeltme eki sürümü yayımlandığında, bu genellikle için güncelleştirmeleri içerir `Microsoft.EntityFrameworkCore.Relational` paket. İlişkisel veritabanı sağlayıcısı eklediğinizde, bu paket, uygulamanızın geçişli bir bağımlılık olur. Ancak birçok sağlayıcı EF Core bağımsız olarak yayımlanır ve bu paketin daha yeni düzeltme eki sürümü bağımlı güncelleştirilmeyebilir. Tüm hata düzeltmeleri alırsınız emin olmak için düzeltme eki sürümü eklemeniz önerilir `Microsoft.EntityFrameworkCore.Relational` , uygulamanızın doğrudan bağımlılık olarak.
