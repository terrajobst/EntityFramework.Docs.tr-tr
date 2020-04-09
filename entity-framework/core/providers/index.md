---
title: Veritabanı Sağlayıcıları - EF Core
author: ajcvickers
ms.date: 12/17/2019
uid: core/providers/index
ms.openlocfilehash: daf2e06c76ed55213243f5728548fdfd4be0e5e2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417827"
---
# <a name="database-providers"></a>Veritabanı Sağlayıcıları

Entity Framework Core, veritabanı sağlayıcıları adı verilen eklenti kitaplıkları aracılığıyla birçok farklı veritabanına erişebilir.

## <a name="current-providers"></a>Mevcut sağlayıcılar

> [!IMPORTANT]  
> EF Core sağlayıcıları çeşitli kaynaklar tarafından oluşturulmuştür. Tüm sağlayıcılar [Entity Framework Core Project'in](https://github.com/aspnet/EntityFrameworkCore)bir parçası olarak korunmuyor. Bir sağlayıcı düşünürken, kalite, lisanslama, destek, vb. değerlendirdiğinizden emin olun. Ayrıca, ayrıntılı sürüm uyumluluk bilgileri için her sağlayıcının belgelerini gözden geçirdiğinizden emin olun.

> [!IMPORTANT]  
> EF Core sağlayıcıları genellikle küçük sürümler arasında çalışır, ancak ana sürümler arasında çalışmaz. Örneğin, EF Core 2.1 için yayımlanan bir sağlayıcı EF Core 2.2 ile çalışmalıdır, ancak EF Core 3.0 ile çalışmaz. 

| NuGet Paketi                                                                                                        | Desteklenen veritabanı motorları | Bakımcı / Satıcı                                                           | Notlar / Gereksinimler | Sürüm için üretilmiştir | Yararlı bağlantılar                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2012'den itibaren    | [EF Çekirdek Projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [Dokümanlar](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7'den itibaren         | [EF Çekirdek Projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [Dokümanlar](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | EF Core bellek veritabanı | [EF Çekirdek Projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | [Sınırlamalar](xref:core/miscellaneous/testing/in-memory)                 | 3.1               | [Dokümanlar](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | Azure Cosmos DB SQL API    | [EF Çekirdek Projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | 3.1               | [Dokümanlar](xref:core/providers/cosmos/index)                                                                                                                                                           |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Npgsql Geliştirme Ekibi](https://github.com/npgsql)                          |                      | 3.1               | [Dokümanlar](https://www.npgsql.org/efcore/index.html)                                                                                                                                                   |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Vakfı Projesi](https://github.com/PomeloFoundation)              |                      | 3.1               | [Benioku](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5'in itibaren            | [DevArt](https://www.devart.com/)                                             | Ödenen                 | 3,0               | [Dokümanlar](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle DB 9.2.0.4 itibaren  | [DevArt](https://www.devart.com/)                                             | Ödenen                 | 3,0               | [Dokümanlar](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0'dan itibaren     | [DevArt](https://www.devart.com/)                                             | Ödenen                 | 3,0               | [Dokümanlar](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3'ün itibaren           | [DevArt](https://www.devart.com/)                                             | Ödenen                 | 3,0               | [Dokümanlar](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [DosyaBağlamCore](https://www.nuget.org/packages/FileContextCore/)                                                   | Verileri dosyalarda depolar       | [Morris Janatzek](https://github.com/morrisjdev)                              | Geliştirme amacıyla | 3,0               | [Benioku](https://github.com/morrisjdev/FileContextCore/blob/master/README.md)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Microsoft Access dosyaları     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | 2,2               | [Benioku](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2,2               | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | 2,2               | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 ve 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | 2,2               | [Dokümanlar](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [Teradata.EntityFrameworkCore](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                         | Teradata Veritabanı 16.10'dan itibaren | [Teradata](https://downloads.teradata.com/download/connectivity/net-data-provider-for-teradata) | | 2,2               |[Web sitesi](https://www.nuget.org/packages/Teradata.EntityFrameworkCore/)                                                                                                                            |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 ve 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | 2.1               | [Wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | Progress OpenEdge          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | 2.1               | [Benioku](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [MySQL projesi](https://dev.mysql.com) (Oracle)                               |                      | 2.1               | [Dokümanlar](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)                             | Oracle DB 11.2 itibaren     | [Oracle](https://www.oracle.com/technetwork/topics/dotnet/)                   |                      | 2.1               | [Web sitesi](https://www.oracle.com/technetwork/topics/dotnet/)                                                                                                                                       |
| [ıbm. EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2, Informix              | [IBM](https://ibm.com)                                                        | Windows sürümü      | 2,0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [ıbm. EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | Linux sürümü        | 2,0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [ıbm. EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2, Informix              | [IBM](https://ibm.com)                                                        | macOS sürümü        | 2,0               | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT Sunucusu               | [Pomelo Vakfı Projesi](https://github.com/PomeloFoundation)              | Yalnızca ön sürüm      | 1.1               | [Benioku](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |

## <a name="adding-a-database-provider-to-your-application"></a>Uygulamanıza veritabanı sağlayıcısı ekleme

EF Core için çoğu veritabanı sağlayıcısı NuGet paketleri olarak dağıtılır ve aşağıdaki gibi yüklenebilir:

## <a name="net-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package provider_package_name
```

## <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
install-package provider_package_name
```

***

Yüklendikten sonra, bir bağımlılık enjeksiyon `DbContext`kapsayıcısı kullanıyorsanız, `OnConfiguring` sağlayıcıyı yöntemde veya `AddDbContext` yöntemde yapılandırmış olursunuz.
Örneğin, aşağıdaki satır, SQL Server sağlayıcısını geçirilen bağlantı dizesiyle yapılandırır:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Veritabanı sağlayıcıları, belirli veritabanlarına özgü işlevselliği etkinleştirmek için EF Core'u genişletebilir.
Bazı kavramlar çoğu veritabanlarında yaygındır ve birincil EF Core bileşenlerine dahildir.
Bu tür kavramlar, LINQ'daki sorguları, hareketleri ifade etmeyi ve veritabanından yüklendikten sonra nesnelerdeki değişiklikleri izlemeyi içerir.
Bazı kavramlar belirli bir sağlayıcıya özgür.
Örneğin, SQL Server sağlayıcısı bellek için optimize edilmiş tabloları (SQL Server'a özgü bir özellik) [yapılandırmanıza](xref:core/providers/sql-server/memory-optimized-tables) olanak tanır.
Diğer kavramlar sağlayıcılar sınıfına özgüdir.
Örneğin, ilişkisel veritabanları için EF Core sağlayıcıları, tablo ve sütun eşlemelerini, yabancı anahtar kısıtlamalarını vb. yapılandırmak için API'ler sağlayan ortak `Microsoft.EntityFrameworkCore.Relational` kitaplığı temel alır. Sağlayıcılar genellikle NuGet paketleri olarak dağıtılır.

> [!IMPORTANT]  
> EF Core'un yeni bir yama sürümü yayımlandığında, genellikle paketgüncelleştirmelerini `Microsoft.EntityFrameworkCore.Relational` içerir.
> İlişkisel bir veritabanı sağlayıcısı eklediğinizde, bu paket uygulamanızın geçişli bir bağımlılığı haline gelir.
> Ancak birçok sağlayıcı EF Core'dan bağımsız olarak serbest bırakılır ve bu paketin yeni yama sürümüne bağlı olarak güncelleştirilemeyebilir.
> Tüm hata düzeltmelerini aldığınızdan emin olmak için, uygulamanızın doğrudan bağımlılığı `Microsoft.EntityFrameworkCore.Relational` olarak yama sürümünü eklemeniz önerilir.
