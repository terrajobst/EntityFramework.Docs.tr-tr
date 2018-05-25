---
title: Oluşturulan özellikleri - EF çekirdek açık değerleri ayarlama
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>Açık değerler için oluşturulan özelliklerini ayarlama

Oluşturulan özelliği (da EF veya veritabanı) değeri oluşturulan bir özelliktir zaman varlık eklendi ve/veya güncelleştirildi. Bkz: [oluşturulan Özellikler](../modeling/generated-properties.md) daha fazla bilgi için.

Oluşturulan bir tane yerine oluşturulan özelliği için açık bir değer ayarlamak istediğiniz durumlar olabilir.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) github'da.

## <a name="the-model"></a>Modeli

Bu makalede kullanılan model tek bir içeren `Employee` varlık.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Açık bir değer Ekle sırasında kaydetme

`Employee.EmploymentStarted` Özelliği (varsayılan bir değer kullanarak) yeni varlıklar için veritabanı tarafından oluşturulan değerler için yapılandırılır.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Aşağıdaki kod iki çalışanlar veritabanına ekler.
* İlk için hiçbir değer atanmış `Employee.EmploymentStarted` , böylece özelliği ayarlamak için CLR varsayılan değere `DateTime`.
* Saniye, biz, açık bir değer kümesi `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Çıktı veritabanı ilk çalışan için bir değer oluşturulur ve bizim açık bir değer saniye kullanılan gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>SQL Server kimlik sütunlara açık değerler

Kural tarafından `Employee.EmployeeId` özelliktir oluşturulan bir mağaza `IDENTITY` sütun.

Çoğu durumlarda, yukarıda gösterilen yaklaşım için anahtar özellikler çalışmaz. Ancak, bir SQL Server'a açık değerler eklemek için `IDENTITY` sütun, el ile etkinleştirmek için ihtiyacınız `IDENTITY_INSERT` çağırmadan önce `SaveChanges()`.

> [!NOTE]  
> Sahip olduğumuz bir [özellik isteği](https://github.com/aspnet/EntityFramework/issues/703) içinde SQL Server sağlayıcısı otomatik olarak bunu yapmak için bizim biriktirme listesi üzerinde.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Sağlanan kimlikleri veritabanına kaydedildi çıktısını gösterir.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Güncelleştirme sırasında açık bir değer ayarlama

`Employee.LastPayRaise` Özelliği güncelleştirmeleri sırasında veritabanı tarafından oluşturulan değerler için yapılandırılır.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Varsayılan olarak, EF çekirdek, güncelleştirme sırasında oluşturulacak şekilde yapılandırılmış bir özelliği için açık bir değer kaydetmeye çalışırken bir özel durum oluşturur. Bunu önlemek için alt düzey meta veriler için API açılır ve ayarlamak gereken `AfterSaveBehavior` (yukarıda gösterildiği gibi).

> [!NOTE]  
> **EF çekirdek 2.0 değişiklikleri:** önceki sürümlerde aracılığıyla sonrası kaydetme davranışını kontrol `IsReadOnlyAfterSave` bayrağı. Bu bayrak geçersiz ve değiştirilmiştir `AfterSaveBehavior`.

Ayrıca bir tetikleyici için değerlerini oluşturmak için veritabanındaki olduğundan `LastPayRaise` sırasında sütun `UPDATE` işlemleri.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Aşağıdaki kod iki çalışanlar veritabanında maaş artırır.
* Hiçbir değer atandığı ilk için `Employee.LastPayRaise` özelliğini ayarlanmış olarak kalır, null.
* İkinci için size bir hafta önce (geri ödeme dating olursa) açık bir değer ayarladınız.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Çıktı veritabanı ilk çalışan için bir değer oluşturulur ve bizim açık bir değer saniye kullanılan gösterir.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
