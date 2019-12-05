---
title: Veritabanı sağlayıcıları-EF Core
author: rowanmiller
ms.date: 02/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/providers/index
ms.openlocfilehash: 7acc00c5e78ac5bc82f6d55d8b74682ff5f5a799
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74823980"
---
# <a name="database-providers"></a>Veritabanı Sağlayıcıları

Entity Framework Core, veritabanı sağlayıcıları adlı eklenti kitaplıkları aracılığıyla birçok farklı veritabanına erişebilir.

## <a name="current-providers"></a>Geçerli sağlayıcılar

> [!IMPORTANT]  
> EF Core sağlayıcıları çeşitli kaynaklardan oluşturulmuştur. Tüm sağlayıcılar [Entity Framework Core projesinin](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunmaz. Bir sağlayıcıyı değerlendirirken, gereksinimlerinizi karşıladığından emin olmak için kalite, lisanslama, destek vb. değerlendirdiğinizden emin olun. Ayrıca, ayrıntılı sürüm uyumluluk bilgileri için her sağlayıcının belgelerini gözden geçirdiğinizden emin olun.

| NuGet paketi                                                                                                        | Desteklenen veritabanı motorları | Bakımcı/satıcı                                                           | Notlar/gereksinimler | Yararlı bağlantılar                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2012 sürümleri    | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [belgeler](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft. EntityFrameworkCore. SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3,7 sonraki sürümler         | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [belgeler](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft. EntityFrameworkCore. InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | Bellek içi veritabanı EF Core | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Yalnızca test için     | [belgeler](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft. EntityFrameworkCore. Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | Azure Cosmos DB SQL API    | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [belgeler](xref:core/providers/cosmos/index)                                                                                                                                                           |
| [Npgsql. EntityFrameworkCore. PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Npgsql geliştirme ekibi](https://github.com/npgsql)                          |                      | [belgeler](https://www.npgsql.org/efcore/index.html)                                                                                                                                                   |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation projesi](https://github.com/PomeloFoundation)              |                      | [Benioku](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo. EntityFrameworkCore. MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT sunucusu               | [Pomelo Foundation projesi](https://github.com/PomeloFoundation)              | Yalnızca ön sürüm      | [Benioku](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql. EntityFrameworkCore. Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2,5 ve 3. x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | [belgeler](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2,5 ve 3. x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [MySql. Data. EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [MySQL projesi](https://dev.mysql.com) (Oracle)                               |                      | [belgeler](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle. EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11,2 sürümleri     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   | Ön sürüm           | [Websitesi](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [IBM. EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | DB2, Informix              | [IBM](https://ibm.com)                                                        | Windows sürümü      | [lenemeyen](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM. EntityFrameworkCore-LNX](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | DB2, Informix              | [IBM](https://ibm.com)                                                        | Linux sürümü        | [lenemeyen](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM. EntityFrameworkCore-OSX](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | DB2, Informix              | [IBM](https://ibm.com)                                                        | macOS sürümü        | [lenemeyen](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [EntityFrameworkCore. Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Microsoft Access dosyaları     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | [Benioku](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore. OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | Progress OpenEdge          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | [Benioku](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Devart. Data. Oracle. EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 sürümleri  | [DevArt](https://www.devart.com/)                                             | Ücretli                 | [belgeler](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8,0 sonraki sürümler     | [DevArt](https://www.devart.com/)                                             | Ücretli                 | [belgeler](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart. Data. SQLite. EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 ve sonraki sürümler           | [DevArt](https://www.devart.com/)                                             | Ücretli                 | [belgeler](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 ve sonraki sürümler            | [DevArt](https://www.devart.com/)                                             | Ücretli                 | [belgeler](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [FileContextCore](https://www.nuget.org/packages/FileContextCore/)                                 | Verileri dosyalarda depolar            | [MORRIS, Ocatzek](https://github.com/morrisjdev)                                             | Geliştirme amacıyla                 | [Benioku](https://github.com/morrisjdev/FileContextCore/blob/master/README.md)                                                                                                                                              |

## <a name="adding-a-database-provider-to-your-application"></a>Uygulamanıza veritabanı sağlayıcısı ekleme

EF Core için veritabanı sağlayıcılarının çoğu NuGet paketleri olarak dağıtılır ve aşağıdaki gibi yüklenebilir:

## <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package provider_package_name
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

Yüklendikten sonra, `OnConfiguring` yönteminde veya bir bağımlılık ekleme kapsayıcısı kullanıyorsanız `AddDbContext` yönteminde `DbContext`sağlayıcıyı yapılandıracaksınız.
Örneğin, aşağıdaki satır, SQL Server sağlayıcıyı geçilen bağlantı dizesiyle yapılandırır:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Veritabanı sağlayıcıları, belirli veritabanlarına özgü işlevleri etkinleştirmek için EF Core genişletebilir.
Bazı kavramlar çoğu veritabanı için ortaktır ve birincil EF Core bileşenlerine dahildir.
Bu tür kavramlar, LINQ, işlemler ve veritabanından yüklendikten sonra nesnelerde yapılan değişiklikleri izlemeye yönelik sorguları içerir.
Bazı kavramlar belirli bir sağlayıcıya özgüdür.
Örneğin, SQL Server sağlayıcı [bellek için iyileştirilmiş tabloları yapılandırmanıza](xref:core/providers/sql-server/memory-optimized-tables) olanak tanır (SQL Server özgü bir özellik).
Diğer kavramlar bir sağlayıcı sınıfına özeldir.
Örneğin, ilişkisel veritabanlarına yönelik EF Core sağlayıcılar, tablo ve sütun eşlemelerini yapılandırma, yabancı anahtar kısıtlamaları vb. için API 'Ler sağlayan ortak `Microsoft.EntityFrameworkCore.Relational` kitaplığı üzerinde oluşturur. Sağlayıcılar genellikle NuGet paketleri olarak dağıtılır.

> [!IMPORTANT]  
> EF Core yeni bir Patch sürümü yayınlandığında, genellikle `Microsoft.EntityFrameworkCore.Relational` paketine yönelik güncelleştirmeleri içerir.
> İlişkisel bir veritabanı sağlayıcısı eklediğinizde, bu paket uygulamanızın geçişli bir bağımlılığı haline gelir.
> Ancak pek çok sağlayıcı EF Core bağımsız olarak yayımlanır ve bu paketin daha yeni yama sürümüne bağlı olarak güncelleştirilmemiş olabilir.
> Tüm hata düzeltmelerini aldığınızdan emin olmak için, `Microsoft.EntityFrameworkCore.Relational` Patch sürümünü uygulamanızın doğrudan bağımlılığı olarak eklemeniz önerilir.
