---
title: "Veritabanı sağlayıcıları - EF çekirdek"
author: rowanmiller
ms.author: divega
ms.date: 2/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 520afe85af5a2eacbfc2764fdc0a8addb78c07ab
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="database-providers"></a>Veritabanı sağlayıcıları

Entity Framework Çekirdek birçok farklı veritabanı veritabanı sağlayıcı olarak adlandırılan eklenti kitaplıkları erişebilirsiniz.

## <a name="current-providers"></a>Geçerli sağlayıcıları
> [!IMPORTANT]  
> EF çekirdek sağlayıcılar çeşitli kaynakları tarafından oluşturulur. Tüm sağlayıcılar parçası olarak korunur [Entity Framework Çekirdek proje](https://github.com/aspnet/EntityFrameworkCore). Bir sağlayıcı değerlendirirken, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirmek emin olun. Ayrıca her sağlayıcının belgelerine ayrıntılı sürüm uyumluluk bilgilerini gözden emin olun.

| NuGet paketi                                                                                                     | Desteklenen her veritabanı motoru | Bakımcı / satıcı                                                           | Notlar / gereksinimleri           | Yararlı bağlantılar                                                                                                                                                              |
|:------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:-------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer) | SQL Server 2008 veya sonraki sürümleri    | [EF çekirdek proje](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                | [Belgeleri](xref:core/providers/sql-server/index)                                                                                                                              |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)       | SQLite 3.7 veya sonraki sürümleri         | [EF çekirdek proje](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                | [Belgeleri](xref:core/providers/sqlite/index)                                                                                                                                  |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)   | EF çekirdek bellek içi veritabanı | [EF çekirdek proje](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Yalnızca test etmek için               | [Belgeleri](xref:core/providers/in-memory/index)                                                                                                                               |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)   | PostgreSQL                 | [Npgsql geliştirme ekibi](https://github.com/npgsql)                          |                                | [Belgeleri](http://www.npgsql.org/efcore/index.html)                                                                                                                           |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)               | MySQL, MariaDB             | [Pomelo Foundation Project](https://github.com/PomeloFoundation)              |                                | [Benioku dosyası](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                      |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)               | MyCAT sunucu               | [Pomelo Foundation Project](https://github.com/PomeloFoundation)              | EF çekirdek 1.1 kadar yayın öncesi | [Benioku dosyası](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                      |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)   | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                 | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                            |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)   | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                 | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                            |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                   | MySQL                      | [MySQL proje](http://dev.mysql.com) (Oracle)                                | Yayın öncesi                    | [Belgeleri](https://dev.mysql.com/doc/connector-net/en/)                                                                                                                       |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                | Firebird 2.5 ve 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           | EF çekirdek 2.0 veya sonraki sürümleri            | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                            |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                 | Db2, Informix              | [IBM](https://ibm.com)                                                        | EF kadar 1.1, Windows çekirdek     | [SSS](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Instructions_for_downloading_and_using_DB2_NET_Core_provider_package) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                         | Db2, Informix              | [IBM](https://ibm.com)                                                        | 1.1, Linux EF kadar çekirdek       | [SSS](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Instructions_for_downloading_and_using_DB2_NET_Core_provider_package) |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                            | Oracle 9.2.0.4 veya sonraki sürümleri     | [DevArt](https://www.devart.com/)                                             | Ücretli                           | [Belgeleri](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                    |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                    | PostgreSQL 8.0 veya sonraki sürümleri     | [DevArt](https://www.devart.com/)                                             | Ücretli                           | [Belgeleri](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                            | SQLite 3'ten başlayarak           | [DevArt](https://www.devart.com/)                                             | Ücretli                           | [Belgeleri](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                    |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                              | MySQL 5'ten başlayarak            | [DevArt](https://www.devart.com/)                                             | Ücretli                           | [Belgeleri](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                     |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                | Microsoft Access dosyaları     | [Bubi](https://github.com/bubibubi)                                           | EF çekirdek 2.0, .NET Framework    | [Benioku dosyası](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                  |

## <a name="future-providers"></a>Gelecekteki sağlayıcıları

### <a name="cosmos-db"></a>Cosmos DB

Biz EF çekirdek sağlayıcısı Cosmos DB DocumentDB API geliştirme. Bu sorundan ürettiğini ilk tam belge yönelimli veritabanı sağlayıcısı olacaktır ve sonraki sürüm 2.1 sonra tasarımındaki geliştirmeler bildirmek için bu alıştırmada gelen learnings geçiyor. Geçerli planınız sağlayıcı erken bir önizlemesini 2.1 zaman çerçevesinde yayımlamaktır.

### <a name="oracle"></a>Oracle
Oracle .NET ekibi 2018 üçüncü çeyrek içinde yaklaşık EF çekirdek 2.0 için birinci taraf sağlayıcısı yayımlamayı planlama bildirme. Bkz: kendi [.NET Core ve Entity Framework Çekirdek yönünü deyiminin](http://www.oracle.com/technetwork/topics/dotnet/tech-info/odpnet-dotnet-ef-core-sod-4395108.pdf) daha fazla bilgi için.
Lütfen yayın zaman çizelgesi için de dahil olmak üzere, bu sağlayıcı hakkında herhangi bir sorunuz doğrudan [Oracle topluluk sitesi](https://community.oracle.com/).

Bu sırada, EF takım üretilen bir [Oracle veritabanları için örnek EF çekirdek sağlayıcısı](https://github.com/aspnet/EntityFrameworkCore/blob/dev/samples/OracleProvider/README.md). Microsoft tarafından sahip olunan bir EF çekirdek sağlayıcısı oluşturmak için ancak tanımlamaya yardımcı olmak için Oracle daha iyi desteklemek için adres gereken EF çekirdek'ın ilişkisel ve temel işlevleri ve başkalarından için diğer Oracle geliştirme boşluklar proje amacı değil Oracle ya da üçüncü taraflar tarafından EF çekirdek için sağlayıcıları.

Biz örnek uygulama geliştirilmesine katkıda göz önüne alır. Biz Hoş Geldiniz da EF çekirdek, örnek bir başlangıç noktası olarak kullanmak için bir açık kaynak Oracle sağlayıcısı oluşturmak için bir topluluk çaba teşvik edin.

## <a name="adding-a-database-provider-to-your-application"></a>Uygulamanız için bir veritabanı sağlayıcısı ekleme

Çoğu veritabanı sağlayıcısı EF çekirdek için NuGet paket olarak dağıtılır. Bunlar kullanılarak yüklenebilir yani `dotnet` komut satırı aracı:

``` console
dotnet add package provider_package_name
```

Veya Visual Studio'da NuGet Paket Yöneticisi konsolu kullanarak:

``` powershell
install-package provider_package_name
```

Bir kez yüklendikten sonra sağlayıcısında yapılandırır, `DbContext`, her iki içinde `OnConfiguring` yöntemi veya `AddDbContext` bir bağımlılık ekleme kapsayıcısını kullanıyorsanız yöntemi. Örneğin Aşağıdaki satırı geçirilen bağlantı dizesi ile SQL Server sağlayıcısı yapılandırır:

``` csharp
  optionsBuilder.UseSqlServer("Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Veritabanı sağlayıcıları EF belirli veritabanlarına benzersiz işlevselliğini etkinleştirmek için çekirdek genişletebilirsiniz. Bazı kavramları çoğu veritabanları için ortaktır ve birincil EF çekirdek bileşenler dahil edilir. Bu tür kavramları LINQ, işlemler, sorguları belirtme ve veritabanından yüklenmeden sonra değişiklikleri izleme nesneleri için içerir. Bazı kavramları, belirli bir sağlayıcıya özgü. Örneğin, SQL Server sağlayıcısı sayesinde [bellek için iyileştirilmiş tablolar yapılandırma](xref:core/providers/sql-server/memory-optimized-tables) (özelliği SQL Server'a özgü). Diğer kavramlar sağlayıcıları sınıfına özeldir. Örneğin, ilişkisel veritabanları için EF çekirdek sağlayıcılar ortak yapı `Microsoft.EntityFrameworkCore.Relational` kitaplığı yapılandırmak için tablo ve sütun eşlemelerini, yabancı anahtar kısıtlamaları, vb. API'ler sağlar. Sağlayıcılar genellikle NuGet paket olarak dağıtılır.

> [!IMPORTANT]  
> EF çekirdek yeni bir düzeltme eki sürümü yayımlandığında, bu genellikle için güncelleştirmeleri içerir `Microsoft.EntityFrameworkCore.Relational` paket. İlişkisel veritabanı sağlayıcısı eklediğinizde, bu paket, uygulamanızın geçişli bir bağımlılık haline gelir. Ancak birçok sağlayıcısı EF çekirdek bağımsız olarak yayımlanmıştır ve düzeltme eki sürümü, paketin üzerinde bağımlı güncelleştirilmeyebilir. Tüm hata düzeltmeleri alırsınız emin olmak için düzeltme eki sürümü eklemeniz önerilir `Microsoft.EntityFrameworkCore.Relational` uygulamanızın doğrudan bağımlılık olarak.
