---
title: Oluşturulan özellikler için açık değerleri ayarlama-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: d6aa9a0a9ce34e09a39026ad7ea9195b6777858c
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197863"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Oluşturulan özellikler için açık değerler ayarlanıyor

Oluşturulan özellik, varlık eklendiğinde ve/veya güncelleştirilirken değeri oluşturulan (EF veya Database) bir özelliktir. Daha fazla bilgi için bkz. [üretilen Özellikler](../modeling/generated-properties.md) .

Oluşturulmuş bir özellik için, oluşturulması yerine açık bir değer ayarlamak istediğiniz durumlar olabilir.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/) GitHub üzerinde.

## <a name="the-model"></a>Model

Bu makalede kullanılan model tek `Employee` bir varlık içerir.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Ekleme sırasında açık bir değer kaydetme

`Employee.EmploymentStarted` Özelliği, yeni varlıklar için veritabanı tarafından oluşturulan değerlere sahip olacak şekilde yapılandırılır (varsayılan değer kullanılarak).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Aşağıdaki kod veritabanına iki çalışan ekler.
* İlki için, `Employee.EmploymentStarted` özelliğe hiçbir değer atanmaz, bu nedenle için `DateTime`clr varsayılan değerine ayarlanır.
* İkincisi için açık bir değer `1-Jan-2000`belirledik.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Çıktı, veritabanının ilk çalışan için bir değer üretdiği ve ikincisi için açık değer kullanıldığını gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>SQL Server KIMLIK sütunlarına açık değerler

Kurala göre özelliği `Employee.EmployeeId` , bir depo tarafından oluşturulan `IDENTITY` sütundur.

Çoğu durumda, yukarıda gösterilen yaklaşım anahtar özellikleri için çalışacaktır. Ancak, SQL Server `IDENTITY` sütununa açık değerler eklemek için çağrılmadan `SaveChanges()`önce el ile etkinleştirmeniz `IDENTITY_INSERT` gerekir.

> [!NOTE]  
> Kapsamımızda SQL Server sağlayıcısı içinde otomatik olarak bunu yapması için bir [özellik isteği](https://github.com/aspnet/EntityFramework/issues/703) sunuyoruz.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Çıktı, sağlanan kimliklerin veritabanına kaydedildiğini gösterir.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Güncelleştirme sırasında açık bir değer ayarlama

`Employee.LastPayRaise` Özelliği, güncelleştirmeler sırasında veritabanı tarafından oluşturulan değerlere sahip olacak şekilde yapılandırılmıştır.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Varsayılan olarak, güncelleştirme sırasında oluşturulacak şekilde yapılandırılmış bir özellik için açık bir değer kaydetmeye çalışırsanız, EF Core bir özel durum oluşturur. Bunu önlemek için, alt düzey meta veri API 'sine aşağı doğru açmanız ve `AfterSaveBehavior` (yukarıda gösterildiği gibi) ayarlamanız gerekir.

> [!NOTE]  
> **EF Core 2,0 değişiklikleri:** Önceki sürümlerde, After-Save davranışı `IsReadOnlyAfterSave` bayrağı aracılığıyla denetlenir. Bu bayrak kullanımdan kaldırılmıştır ve tarafından `AfterSaveBehavior`değiştirildi.

Ayrıca, işlemler sırasında `LastPayRaise` `UPDATE` sütun için değerler oluşturmak üzere veritabanında bir tetikleyici de vardır.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Aşağıdaki kod, veritabanında iki çalışanın maaşını artırır.
* İlki için, hiçbir değer `Employee.LastPayRaise` özelliğe atanmaz, bu nedenle null olarak ayarlanır.
* İkincisi için bir hafta önce açık bir değer belirledik (ödeme yapını geri alıyorsunuz).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Çıktı, veritabanının ilk çalışan için bir değer üretdiği ve ikincisi için açık değer kullanıldığını gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
