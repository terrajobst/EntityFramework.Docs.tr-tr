---
title: Uzamsal türler - EF6 sağlayıcı desteği
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 9c00e82c663daec219fe649a8d889afcc81564f7
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022281"
---
# <a name="provider-support-for-spatial-types"></a>Uzamsal türler için sağlayıcı desteği
Entity Framework DbGeography veya DbGeometry sınıflarıyla uzamsal veri ile çalışma destekler. Bu sınıflar Entity Framework sağlayıcısı tarafından sunulan özel veritabanı işlevleri kullanır. Uzamsal veriler tüm sağlayıcıları destekler ve olmayanlar uzamsal türü derlemeler yüklenmesi gibi ek Önkoşullar var. Uzamsal türler için sağlayıcı desteği hakkında daha fazla bilgi aşağıda verilmiştir.  

Code First, diğer veritabanı ilk veya Model ilk için iki izlenecek uzamsal türler bir uygulamada kullanma hakkında ek bilgiler bulunabilir:  

- [Uzamsal veri türleri kodda önce](~/ef6/modeling/code-first/data-types/spatial.md)  
- [EF Designer uzamsal veri türleri](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>EF uzamsal türler destekleyen yayımlar.  

Uzamsal türler için destek EF5 içinde kullanılmaya başlandı. Ancak, uygulama hedefler ve .NET 4.5 üzerinde çalışır EF5 içinde uzamsal türler yalnızca desteklenir.  

Hem .NET 4 ve .NET 4. 5'i hedefleyen uygulamalar için desteklenen uzamsal türleri EF6 ile'başlatılıyor.  

## <a name="ef-providers-that-support-spatial-types"></a>Uzamsal türlerini destekleyen EF sağlayıcıları  

### <a name="ef5"></a>EF5  

Entity Framework sağlayıcıları için destek uzamsal türler olduğunu farkında duyuyoruz EF5:  

- Microsoft SQL Server sağlayıcısı  
    - Bu sağlayıcı EF5 bir parçası olarak sevk edilir.  
    - Bu sağlayıcı yüklenmesi gereken bazı ek alt düzey kitaplıklarını temel bağlıdır; Ayrıntılar için aşağıya bakın.  
- [Devart dotConnect Oracle için](http://www.devart.com/dotconnect/oracle/)  
    - Bir üçüncü taraf sağlayıcısından Devart budur.  

Uzamsal türler sonra lütfen destekleyen bir EF5 sağlayıcısı biliyorsanız kişi alın ve biz bu listeye eklemek memnuniyet duyacaktır.  

### <a name="ef6"></a>EF6  

Entity Framework sağlayıcıları için destek uzamsal türler olduğunu farkında duyuyoruz EF6:  

- Microsoft SQL Server sağlayıcısı  
    - Bu sağlayıcı EF6 bir parçası olarak sevk edilir.  
    - Bu sağlayıcı yüklenmesi gereken bazı ek alt düzey kitaplıklarını temel bağlıdır; Ayrıntılar için aşağıya bakın.  
- [Devart dotConnect Oracle için](http://www.devart.com/dotconnect/oracle/)  
    - Bir üçüncü taraf sağlayıcısından Devart budur.  

Uzamsal türler sonra lütfen destekleyen bir EF6 sağlayıcısı biliyorsanız kişi alın ve biz bu listeye eklemek memnuniyet duyacaktır.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Microsoft SQL Server uzamsal türler için Önkoşullar  

SQL Server uzamsal destek alt düzey, SQL Server'a özgü SqlGeography ve SqlGeometry türlerine bağlıdır. Bu tür Microsoft.SqlServer.Types.dll derlemede canlı ve bu derlemeye EF bir parçası olarak ya da .NET Framework'ün bir parçası olarak gönderildiğini değil.  

Visual Studio yüklendiğinde, genellikle de bir SQL Server sürümü yükler ve bu Microsoft.SqlServer.Types.dll yüklenmesini içerir.  

SQL Server uzamsal türler kullanmak istediğiniz makinede yüklü değil veya SQL Server yüklemesinden uzamsal türler dışlanan, bunları el ile yüklemeniz gerekir. Türleri kullanılarak yüklenebilir `SQLSysClrTypes.msi`, Microsoft SQL Server özellik Paketi'nin bir parçası değildir. Uzamsal türleridir öneririz, böylece SQL Server sürümü özgü ["SQL Server özellik paketi" için arama](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) Microsoft Download Center'daki sonra seçin ve karşılık gelen seçeneği için kullanacağınız SQL Server sürümünü indirin.
