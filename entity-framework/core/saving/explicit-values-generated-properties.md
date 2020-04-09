---
title: Oluşturulan Özellikler için Açık Değerleri Ayarlama - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: 43c4ab3c2a60645cdeff2a6cc40ce979f832f2fd
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417571"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Oluşturulan Özellikler için Açık Değerleri Ayarlama

Oluşturulan özellik, varlık eklendiğinde ve/veya güncelleştirildiğinde değeri (EF veya veritabanı tarafından) oluşturulan bir özelliktir. Daha fazla bilgi için [Oluşturulan Özellikler'e](../modeling/generated-properties.md) bakın.

Oluşturulan bir özellik için açık bir değer ayarlamak istediğiniz durumlar olabilir.

> [!TIP]  
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/) GitHub'da görüntüleyebilirsiniz.

## <a name="the-model"></a>Model

Bu makalede kullanılan model tek `Employee` bir varlık içerir.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Ekleme sırasında açık bir değer kaydetme

Özellik, `Employee.EmploymentStarted` yeni varlıklar için veritabanı tarafından oluşturulan değerlere sahip olacak şekilde yapılandırılır (varsayılan değer kullanılarak).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Aşağıdaki kod veritabanına iki çalışanı ekler.

* İlk olarak, özellik için `Employee.EmploymentStarted` hiçbir değer atanır, bu yüzden CLR `DateTime`varsayılan değeri için ayarlanmış kalır.
* İkincisi için, açık bir değer `1-Jan-2000`belirledik.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Çıktı, veritabanının ilk çalışan için bir değer oluşturduğunu ve ikinci için açık değerimizin kullanıldığını gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>SQL Server IDENTITY sütunlarına açık değerler

Kural olarak `Employee.EmployeeId` özellik bir `IDENTITY` mağaza oluşturulan sütundur.

Çoğu durumda, yukarıda gösterilen yaklaşım önemli özellikler için çalışacaktır. Ancak, bir SQL Server `IDENTITY` sütununa açık değerler eklemek `IDENTITY_INSERT` için, `SaveChanges()`aramadan önce el ile etkinleştirmeniz gerekir.

> [!NOTE]  
> Biriktirme listemizde bunu SQL Server sağlayıcısında otomatik olarak yapmak için bir [özellik isteğimiz](https://github.com/aspnet/EntityFramework/issues/703) var.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Çıktı, verilen kimliklerin veritabanına kaydedildiğini gösterir.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Güncelleştirme sırasında açık bir değer ayarlama

Özellik, `Employee.LastPayRaise` güncelleştirmeler sırasında veritabanı tarafından oluşturulan değerlere sahip olacak şekilde yapılandırılır.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Varsayılan olarak, güncelleştirme sırasında oluşturulacak şekilde yapılandırılan bir özellik için açık bir değer kaydetmeye çalışırsanız, EF Core bir özel durum oluşturur. Bunu önlemek için, alt düzey meta veri API'sine `AfterSaveBehavior` inmeniz ve (yukarıda gösterildiği gibi) ayarlamanız gerekir.

> [!NOTE]  
> **EF Core 2.0'daki değişiklikler:** Önceki sürümlerde kaydsonrası davranış `IsReadOnlyAfterSave` bayrak üzerinden denetlendi. Bu bayrak obsoleted ve yerini `AfterSaveBehavior`.

Ayrıca, işlem sırasında `LastPayRaise` `UPDATE` sütun için değer oluşturmak için veritabanında bir tetikleyici vardır.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Aşağıdaki kod veritabanında iki çalışanın maaş Artar.

* İlk olarak, özellik için `Employee.LastPayRaise` hiçbir değer atanır, bu nedenle null olarak ayarlanır.
* İkincisi için, bir hafta önce (geri maaş zammı kalma) açık bir değer belirledik.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Çıktı, veritabanının ilk çalışan için bir değer oluşturduğunu ve ikinci için açık değerimizin kullanıldığını gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
