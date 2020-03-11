---
title: Oluşturulan özellikler için açık değerleri ayarlama-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: 43c4ab3c2a60645cdeff2a6cc40ce979f832f2fd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417571"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Oluşturulan özellikler için açık değerler ayarlanıyor

Oluşturulan özellik, varlık eklendiğinde ve/veya güncelleştirilirken değeri oluşturulan (EF veya Database) bir özelliktir. Daha fazla bilgi için bkz. [üretilen Özellikler](../modeling/generated-properties.md) .

Oluşturulmuş bir özellik için, oluşturulması yerine açık bir değer ayarlamak istediğiniz durumlar olabilir.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/) GitHub ' da görebilirsiniz.

## <a name="the-model"></a>Model

Bu makalede kullanılan model tek bir `Employee` varlığı içerir.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Ekleme sırasında açık bir değer kaydetme

`Employee.EmploymentStarted` özelliği, yeni varlıklar için veritabanı tarafından oluşturulan değerlere sahip olacak şekilde yapılandırılır (varsayılan değer kullanılarak).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Aşağıdaki kod veritabanına iki çalışan ekler.

* İlki için, `Employee.EmploymentStarted` özelliğine hiçbir değer atanmaz, bu nedenle `DateTime`için CLR varsayılan değerine ayarlanır.
* İkincisi, `1-Jan-2000`açık bir değer belirledik.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Çıktı, veritabanının ilk çalışan için bir değer üretdiği ve ikincisi için açık değer kullanıldığını gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>SQL Server KIMLIK sütunlarına açık değerler

Kurala göre `Employee.EmployeeId` özelliği bir depo `IDENTITY` sütunu olarak oluşturulur.

Çoğu durumda, yukarıda gösterilen yaklaşım anahtar özellikleri için çalışacaktır. Ancak, bir SQL Server `IDENTITY` sütununa açık değerler eklemek için, `SaveChanges()`çağrılmadan önce `IDENTITY_INSERT` el ile etkinleştirmeniz gerekir.

> [!NOTE]  
> Kapsamımızda SQL Server sağlayıcısı içinde otomatik olarak bunu yapması için bir [özellik isteği](https://github.com/aspnet/EntityFramework/issues/703) sunuyoruz.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Çıktı, sağlanan kimliklerin veritabanına kaydedildiğini gösterir.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Güncelleştirme sırasında açık bir değer ayarlama

`Employee.LastPayRaise` özelliği, güncelleştirmeler sırasında veritabanı tarafından oluşturulan değerlere sahip olacak şekilde yapılandırılmıştır.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Varsayılan olarak, güncelleştirme sırasında oluşturulacak şekilde yapılandırılmış bir özellik için açık bir değer kaydetmeye çalışırsanız, EF Core bir özel durum oluşturur. Bunu önlemek için, alt düzey meta veri API 'sine aşağı doğru açmanız ve `AfterSaveBehavior` (yukarıda gösterildiği gibi) ayarlamanız gerekir.

> [!NOTE]  
> **EF Core 2,0 değişiklikleri:** Önceki sürümlerde, sonra Kaydet davranışı `IsReadOnlyAfterSave` bayrağıyla denetlenir. Bu bayrak kullanımdan kaldırılmıştır ve `AfterSaveBehavior`tarafından değiştirildi.

Ayrıca, `UPDATE` işlemleri sırasında `LastPayRaise` sütunu için değerler oluşturmak üzere veritabanında bir tetikleyici de vardır.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Aşağıdaki kod, veritabanında iki çalışanın maaşını artırır.

* İlki için, `Employee.LastPayRaise` özelliğine hiçbir değer atanmaz, bu nedenle null olarak ayarlanır.
* İkincisi için bir hafta önce açık bir değer belirledik (ödeme yapını geri alıyorsunuz).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Çıktı, veritabanının ilk çalışan için bir değer üretdiği ve ikincisi için açık değer kullanıldığını gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
