---
title: Oluşturulan özellikler - EF Core açık değerlerini ayarlama
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: 00abef4d1208400ff68ced0a241b98b8dc9be5c0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997859"
---
# <a name="setting-explicit-values-for-generated-properties"></a>İçin oluşturulan özellikleri açık değerler ayarlama

Oluşturulan özellik değeri (veya EF veya veritabanı) oluşturulan bir özelliktir ne zaman varlık eklendi ve güncellendi. Bkz: [üretilen özellikleri](../modeling/generated-properties.md) daha fazla bilgi için.

Oluşturulan iki yerine oluşturulan özellik için açık bir değer ayarlamak için istediğiniz durumlar olabilir.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) GitHub üzerinde.

## <a name="the-model"></a>Model

Bu makalede kullanılan modeli içeren tek bir `Employee` varlık.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Açık bir değer ekleme sırasında kaydediliyor

`Employee.EmploymentStarted` Özelliği yeni varlıklar (varsayılan değer kullanarak) için veritabanı tarafından oluşturulan değerleri içerecek şekilde yapılandırılmıştır.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Aşağıdaki kod, iki çalışan veritabanına ekler.
* Herhangi bir değer atanır ilki için `Employee.EmploymentStarted` kaldığı için özelliği ayarlamak için CLR varsayılan değerine `DateTime`.
* Saniye, biz, açık bir değer ayarladığınız `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Çıktı, veritabanı için ilk çalışan bir değer oluşturulur ve bizim açık değer saniye kullanıldı gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>SQL Server kimlik sütunlara açık değerler

Kural gereği `Employee.EmployeeId` özelliktir oluşturulan bir depo `IDENTITY` sütun.

Çoğu durumlar için yukarıda gösterilen bir yaklaşım için anahtar özellikler çalışır. Ancak, bir SQL Server'a açık değerler eklemek için `IDENTITY` sütununda, gereksinim el ile etkinleştirmek `IDENTITY_INSERT` çağırmadan önce `SaveChanges()`.

> [!NOTE]  
> Sahip olduğumuz bir [özellik isteği](https://github.com/aspnet/EntityFramework/issues/703) şirket içinde SQL Server sağlayıcısı otomatik olarak bunu yapmak için çalışıyoruz.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Çıktı, sağlanan kimlikleri veritabanına kaydedilmiş olduğunu gösterir.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Güncelleştirme sırasında açık bir değer ayarlama

`Employee.LastPayRaise` Özellik güncelleştirmeleri sırasında veritabanı tarafından oluşturulan değerleri içerecek şekilde yapılandırılır.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Varsayılan olarak EF Core, güncelleştirme sırasında oluşturulacak yapılandırılmış bir özelliği için açık bir değer kaydetmeye çalışırken bir özel durum oluşturur. Bunu önlemek için alt düzey meta veriler için API açılan menü ve ayarlamak gereken `AfterSaveBehavior` (yukarıda gösterildiği gibi).

> [!NOTE]  
> **EF Core 2.0 içindeki değişiklikleri:** önceki sürümlerde aracılığıyla sonrası kaydetme davranışını kontrol `IsReadOnlyAfterSave` bayrağı. Bu bayrağı geçersiz kılınmış ve yerine `AfterSaveBehavior`.

Ayrıca bir tetikleyici için değerler oluşturmak için veritabanındaki olduğundan `LastPayRaise` sırasında sütun `UPDATE` operations.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Aşağıdaki kod, veritabanı iki çalışanların maaş artırır.
* Herhangi bir değer atanır ilki için `Employee.LastPayRaise` özelliğini ayarla kalır, null.
* Açık bir değer, bir hafta önce (geri ödeme ilk artırma) saniye için ayarladık.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Çıktı, veritabanı için ilk çalışan bir değer oluşturulur ve bizim açık değer saniye kullanıldı gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
