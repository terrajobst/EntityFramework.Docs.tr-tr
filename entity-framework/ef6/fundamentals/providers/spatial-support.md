---
title: Uzamsal türler için sağlayıcı desteği-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181591"
---
# <a name="provider-support-for-spatial-types"></a>Uzamsal türler için sağlayıcı desteği
Entity Framework, Dbcoğrafya veya DbGeometry sınıfları aracılığıyla uzamsal verilerle çalışmayı destekler. Bu sınıflar, Entity Framework sağlayıcısı tarafından sunulan veritabanına özgü işlevleri kullanır. Tüm sağlayıcılar uzamsal verileri desteklemez ve bu, uzamsal tür derlemelerinin yüklenmesi gibi ek önkoşullara sahip olabilir. Uzamsal türler için sağlayıcı desteği hakkında daha fazla bilgi aşağıda verilmiştir.  

Bir uygulamada uzamsal türlerin nasıl kullanılacağına ilişkin ek bilgiler, biri Code First Database First veya Model First için olmak üzere iki izlenecek yol içinde bulunabilir:  

- [Code First uzamsal veri türleri](~/ef6/modeling/code-first/data-types/spatial.md)  
- [EF tasarımcısında uzamsal veri türleri](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>Uzamsal türleri destekleyen EF yayınları  

Uzamsal türler için destek EF5 içinde tanıtılmıştır. Ancak, EF5 uzamsal türler yalnızca uygulama .NET 4,5 üzerinde hedeflerse ve çalıştırıldığında desteklenir.  

EF6 uzamsal türlerinden başlamak, hem .NET 4 hem de .NET 4,5 'yi hedefleyen uygulamalar için desteklenir.  

## <a name="ef-providers-that-support-spatial-types"></a>Uzamsal türleri destekleyen EF sağlayıcıları  

### <a name="ef5"></a>EF5  

EF5 için Entity Framework sağlayıcılar, bu şekilde desteklenen uzamsal türleri destekler:  

- Microsoft SQL Server sağlayıcı  
    - Bu sağlayıcı, EF5 bir parçası olarak gönderilir.  
    - Bu sağlayıcı, yüklenmesi gerekebilecek bazı ek alt düzey kitaplıklara bağımlıdır. Ayrıntılar için aşağıya bakın.  
- [Oracle için Devart dotConnect](https://www.devart.com/dotconnect/oracle/)  
    - Bu, Devart 'den bir üçüncü taraf sağlayıcıdır.  

Uzamsal türleri destekleyen bir EF5 sağlayıcısı biliyorsanız, lütfen bu kişiye ulaşın ve bu listeye eklemek isteriz.  

### <a name="ef6"></a>EF6  

EF6 için Entity Framework sağlayıcılar, bu şekilde desteklenen uzamsal türleri destekler:  

- Microsoft SQL Server sağlayıcı  
    - Bu sağlayıcı, EF6 bir parçası olarak gönderilir.  
    - Bu sağlayıcı, yüklenmesi gerekebilecek bazı ek alt düzey kitaplıklara bağımlıdır. Ayrıntılar için aşağıya bakın.  
- [Oracle için Devart dotConnect](https://www.devart.com/dotconnect/oracle/)  
    - Bu, Devart 'den bir üçüncü taraf sağlayıcıdır.  

Uzamsal türleri destekleyen bir EF6 sağlayıcısı biliyorsanız, lütfen bu kişiye ulaşın ve bu listeye eklemek isteriz.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Microsoft SQL Server ile uzamsal türler için Önkoşullar  

SQL Server uzamsal destek, alt düzey, SQL Server özgü tür Sqlcoğrafya ve SqlGeometry ' ne bağlıdır. Bu türler, Microsoft. SqlServer. Types. dll derlemesinde bulunur ve bu derleme EF 'in bir parçası olarak veya .NET Framework bir parçası olarak sevk edilmez.  

Visual Studio yüklendiğinde, genellikle SQL Server bir sürümünü de yükler ve bu, Microsoft. SqlServer. Types. dll ' nin yüklenmesini içerir.  

Uzamsal türleri kullanmak istediğiniz makinede SQL Server yüklü değilse veya uzamsal türler SQL Server yüklemesinden dışlanmazsa, bunları el ile yüklemeniz gerekir. Türler, Microsoft SQL Server Özellik paketinin bir parçası olan `SQLSysClrTypes.msi` kullanılarak yüklenebilir. Uzamsal türler sürüme özgü SQL Server, bu nedenle Microsoft Indirme merkezi 'nde ["SQL Server Özellik paketi" için arama](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) yapmanızı öneririz, sonra kullanacağınız SQL Server sürümüne karşılık gelen seçeneği seçin ve indirin.
