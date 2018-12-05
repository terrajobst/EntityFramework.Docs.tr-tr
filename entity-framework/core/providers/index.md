---
title: Veritabanı sağlayıcıları - EF Core
author: rowanmiller
ms.date: 02/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/providers/index
ms.openlocfilehash: 8e695b4360367edb055e73499d2522bd695fa315
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857448"
---
# <a name="database-providers"></a>Veritabanı Sağlayıcıları

Entity Framework Core birçok farklı veritabanı, veritabanı sağlayıcı olarak adlandırılan eklenti kitaplıkları erişebilir.

## <a name="current-providers"></a>Geçerli sağlayıcıları
> [!IMPORTANT]  
> EF Core sağlayıcıları, çeşitli kaynaklardan tarafından oluşturulur. Tüm sağlayıcıları bir parçası olarak korunur [Entity Framework Core projesi](https://github.com/aspnet/EntityFrameworkCore). Bir sağlayıcı dikkate alındığında, kalite, lisans, destek, gereksinimlerinizi karşıladıklarından emin olmak için vb. değerlendirdiğinizden emin olun. Ayrıca, her sağlayıcının belgelerine ayrıntılı sürüm uyumluluk bilgilerini gözden emin olun.

| NuGet paketi                                                                                                        | Desteklenen her veritabanı motoru | Bakımcı / satıcı                                                           | Notları / gereksinimleri | Faydalı bağlantılar                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 ve sonraki sürümler    | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [Docs](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 ve sonraki sürümler         | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                      | [Docs](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | EF Core bellek içi veritabanı | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Yalnızca test etmek için     | [Docs](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos)          | Azure Cosmos DB SQL API    | [EF Core projesi](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Yalnızca önizleme         | [blogu](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/)                                                                                         |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)        | PostgreSQL                 | [Npgsql geliştirme ekibi](https://github.com/npgsql)                          |                      | [Docs](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation proje](https://github.com/PomeloFoundation)              |                      | [Benioku dosyası](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT sunucusu               | [Pomelo Foundation proje](https://github.com/PomeloFoundation)              | Yalnızca yayın öncesi      | [Benioku dosyası](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4,0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3.5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework       | [Wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Microsoft Access dosyası     | [Bubi](https://github.com/bubibubi)                                           | .NET Framework       | [Benioku dosyası](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [MySQL proje](http://dev.mysql.com) (Oracle)                                |                      | [Docs](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)                                                                                                         |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 ve 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 |                      | [Docs](https://github.com/cincuranet/FirebirdSql.Data.FirebirdClient/blob/master/Provider/docs/entity-framework-core.md)                                                                           |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 ve 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           |                      | [Wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [IBM. EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | Db2 Informix              | [IBM](https://ibm.com)                                                        | Windows sürümü      | [blogu](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM. EntityFrameworkCore lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | Db2 Informix              | [IBM](https://ibm.com)                                                        | Linux sürümü        | [blogu](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM. EntityFrameworkCore osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | Db2 Informix              | [IBM](https://ibm.com)                                                        | macOS sürümü        | [blogu](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [EntityFrameworkCore.OpenEdge](https://www.nuget.org/packages/EntityFrameworkCore.OpenEdge/)                         | İlerleme OpenEdge          | [Alex Wiese](https://github.com/alexwiese)                                    |                      | [Benioku dosyası](https://github.com/alexwiese/EntityFrameworkCore.OpenEdge/blob/master/README.md)                                                                                                          |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle 9.2.0.4 ve sonraki sürümler     | [DevArt](https://www.devart.com/)                                             | Ücretli                 | [Docs](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 ve üzeri     | [DevArt](https://www.devart.com/)                                             | Ücretli                 | [Docs](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 ve sonraki sürümler           | [DevArt](https://www.devart.com/)                                             | Ücretli                 | [Docs](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 ve sonraki sürümler            | [DevArt](https://www.devart.com/)                                             | Ücretli                 | [Docs](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |

## <a name="future-providers"></a>Gelecekteki sağlayıcıları

### <a name="cosmos-db"></a>Cosmos DB

Biz bir EF Core sağlayıcısı Cosmos DB SQL API geliştirme.
Bu ilk tam belge yönelimli veritabanı sağlayıcısı üretilen olacak ve EF Core ve ilişkisel olmayan büyük olasılıkla diğer sağlayıcılar'ın gelecek sürümlerini tasarımındaki iyileştirmeler bildirmek için bu çalışma alanından dersleri yapacaksınız.
Bir önizleme kullanılabilir [NuGet galerisinde](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos).

### <a name="oracle"></a>Oracle
Oracle .NET ekibi, 2018 üçüncü çeyrek içinde yaklaşık bir birinci taraf sağlayıcısı EF Core için yayımlamayı planlama duyurdu. Bkz: kendi [.NET Core ve Entity Framework Core yönünü deyiminin](http://www.oracle.com/technetwork/topics/dotnet/tech-info/odpnet-dotnet-ef-core-sod-4395108.pdf) daha fazla bilgi için.
Lütfen bu sağlayıcı için yayın zaman çizelgesi dahil olmak üzere hakkında herhangi bir sorunuz doğrudan [Oracle topluluk sitesine](https://community.oracle.com/).

Bu arada, EF ekibin ürettiği bir [Oracle veritabanları için örnek EF Core sağlayıcısı](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/OracleProvider).
Proje amacı, Microsoft tarafından sahip olunan bir EF Core sağlayıcısı üretmez sağlamaktır.
Boşlukları adresine Oracle daha iyi desteklemek için ihtiyacımız EF Core'nın ilişkisel ve temel işlevselliği için projeyi Başladık.
Oracle veya Üçüncü taraflardan jumpstart EF Core için Oracle sağlayıcıları geliştirilmesini de yardımcı olmalıdır.

Biz, örnek uygulamada geliştirilmesine katkı dikkate alacaktır.
Biz ayrıca Hoş Geldiniz ve EF Core, örneği başlangıç noktası kullanan bir açık kaynak Oracle sağlayıcısı oluşturmak için bir topluluk çaba teşvik edin.

## <a name="adding-a-database-provider-to-your-application"></a>Veritabanı sağlayıcısı uygulamanıza ekleme

EF Core için çoğu veritabanı sağlayıcıları NuGet paketleri olarak dağıtılır. Yani bunlar kullanılarak yüklenebilir `dotnet` aracında komut satırı:

``` console
dotnet add package provider_package_name
```

Veya Visual Studio'da NuGet Paket Yöneticisi konsolu kullanarak:

``` powershell
install-package provider_package_name
```

Sağlayıcı yüklendikten sonra yapılandıracağınız, `DbContext`, ya da içinde `OnConfiguring` yöntemi veya `AddDbContext` bir bağımlılık ekleme kapsayıcısını kullanıyorsanız yöntemi.
Örneğin, aşağıdaki satırı geçirilen bağlantı dizesi ile SQL Server sağlayıcısı yapılandırır:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Veritabanı sağlayıcıları belirli veritabanlarına benzersiz işlevselliğini etkinleştirmek için EF Core genişletebilirsiniz.
Bazı kavramları çoğu veritabanı için ortak olan ve birincil EF Core bileşenlerinde yer alır.
Bu kavramlar, LINQ, işlemleri, sorguların belirtme ve veritabanından yüklenmeden sonra değişiklikleri izleme nesneleri için içerir.
Bazı kavramları, belirli bir sağlayıcıya özgü.
Örneğin, SQL Server sağlayıcısı sağlar [bellek için iyileştirilmiş tablolar yapılandırma](xref:core/providers/sql-server/memory-optimized-tables) (özgü bir özellik için SQL Server).
Diğer kavramlar sağlayıcılarının bir sınıfa özgüdür.
Örneğin, ilişkisel veritabanları için EF Core sağlayıcıları üzerinde ortak yapı `Microsoft.EntityFrameworkCore.Relational` kitaplığı, tablo ve sütun eşlemeleri, yabancı anahtar kısıtlamalarını yapılandırma API'leri sağlar. Sağlayıcıları, genellikle NuGet paketleri olarak dağıtılır.

> [!IMPORTANT]  
> EF Core yeni bir düzeltme eki sürümü yayımlandığında, bu genellikle için güncelleştirmeleri içerir `Microsoft.EntityFrameworkCore.Relational` paket.
> İlişkisel veritabanı sağlayıcısı eklediğinizde, bu paket, uygulamanızın geçişli bir bağımlılık olur.
> Ancak birçok sağlayıcı EF Core bağımsız olarak yayımlanır ve bu paketin daha yeni düzeltme eki sürümü bağımlı güncelleştirilmeyebilir.
> Tüm hata düzeltmeleri alırsınız emin olmak için düzeltme eki sürümü eklemeniz önerilir `Microsoft.EntityFrameworkCore.Relational` , uygulamanızın doğrudan bağımlılık olarak.
