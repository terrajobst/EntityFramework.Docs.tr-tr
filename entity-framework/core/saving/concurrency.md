---
title: Eşzamanlılık çakışmalarını işleme-EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: 4d6ff24e58caa0b228e9c1e4313beda78d1025fc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197836"
---
# <a name="handling-concurrency-conflicts"></a>Eşzamanlılık Çakışmalarını İşleme

> [!NOTE]
> Bu sayfa, eşzamanlılık EF Core ' de nasıl çalıştığını ve uygulamanızda eşzamanlılık çakışmalarının nasıl işleneceğini belgeler. Modelinizdeki eşzamanlılık belirteçlerini yapılandırma hakkında ayrıntılı bilgi için bkz. [eşzamanlılık belirteçleri](xref:core/modeling/concurrency) .

> [!TIP]
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) GitHub üzerinde.

_Veritabanı eşzamanlılık_ , birden fazla işlem veya kullanıcının aynı anda bir veritabanındaki verileri erişen veya değiştiren durumlara başvurur. _Eşzamanlılık denetimi_ , eşzamanlı değişiklikler olması halinde veri tutarlılığı sağlamak için kullanılan belirli mekanizmaların anlamına gelir.

EF Core, birden çok işlemin veya kullanıcının değişiklik yapmasına veya kilitlenmeden bağımsız olarak değişiklik yapmasına olanak tanıyan _iyimser eşzamanlılık denetimini_uygular. İdeal durumda, bu değişiklikler birbirini engellemez ve bu nedenle başarılı olur. En kötü durum senaryosunda, iki veya daha fazla işlem çakışan değişiklikler yapmaya çalışacaktır ve bunlardan yalnızca biri başarılı olmalıdır.

## <a name="how-concurrency-control-works-in-ef-core"></a>Eşzamanlılık denetimi nasıl EF Core?

Eşzamanlılık belirteçleri olarak yapılandırılan Özellikler iyimser eşzamanlılık denetimi uygulamak için kullanılır: sırasında `SaveChanges`bir güncelleştirme veya silme işlemi gerçekleştirildiğinde, veritabanındaki eşzamanlılık belirtecinin değeri orijinalle karşılaştırılır EF Core tarafından okunan değer.

- Değerler eşleşiyorsa, işlem tamamlanabilir.
- Değerler eşleşmezse EF Core başka bir kullanıcının çakışan bir işlem gerçekleştirdiğinizi ve geçerli işlemi iptal eder.

Başka bir Kullanıcı, geçerli işlemle çakışan bir işlem gerçekleştirdiğinde _eşzamanlılık çakışması_olarak bilinir.

Veritabanı sağlayıcıları eşzamanlılık belirteci değerlerinin karşılaştırmasını uygulamaktan sorumludur.

İlişkisel veritabanlarında EF Core, herhangi `WHERE` `UPDATE` bir or `DELETE` deyimi yan tümcesindeki eşzamanlılık belirtecinin değeri için bir denetim içerir. Deyimlerini yürüttükten sonra, EF Core etkilenen satır sayısını okur.

Hiçbir satır etkilenmiyorsa, bir eşzamanlılık çakışması algılanır ve EF Core `DbUpdateConcurrencyException`oluşturulur.

Örneğin, ' de `LastName` `Person` bir eşzamanlılık belirteci olacak şekilde yapılandırmak isteyebilirsiniz. Ardından, `WHERE` herkese açık olan herhangi bir güncelleştirme işlemi, yan tümcesine eşzamanlılık denetimi içerecektir:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını çözme

Önceki örnekle devam etmek, bir Kullanıcı bazı değişiklikleri a `Person`'ya kaydetmeye çalışırsa, ancak başka bir kullanıcı tarafından zaten `LastName`değiştirilmişse, bir özel durum oluşturulur.

Bu noktada, uygulama, değişiklikleri çakışan değişiklikler nedeniyle başarılı bir şekilde kullanıcıya bildirebilir ve üzerinde geçiş yapın. Ancak kullanıcıdan bu kaydın aynı gerçek kişiyi temsil ettiğini ve işlemi yeniden denemesini istemek istenebilir.

Bu işlem, _bir eşzamanlılık çakışmasını çözmeye_yönelik bir örnektir.

Eşzamanlılık çakışmasını çözmek, bekleyen değişikliklerin veritabanındaki değerlerle geçerli `DbContext` olan birleştirilmesini içerir. Birleştirilecek değerler uygulamaya göre değişir ve Kullanıcı girişiyle yönlendirilebilir.

**Eşzamanlılık çakışmasını çözmeye yardımcı olmak için üç değer kümesi mevcuttur:**

* **Geçerli değerler** , uygulamanın veritabanına yazmaya çalışan değerlerdir.

* **Orijinal değerler** , hiçbir düzenleme yapılmadan önce veritabanından ilk olarak alınan değerlerdir.

* Veritabanı **değerleri** , veritabanında Şu anda depolanan değerlerdir.

Eşzamanlılık çakışmalarını işlemeye yönelik genel yaklaşım şunlardır:

1. `DbUpdateConcurrencyException` İçinde`SaveChanges`catch.
2. Etkilenen `DbUpdateConcurrencyException.Entries` varlıklar için yeni bir değişiklik kümesi hazırlamak üzere kullanın.
3. Veritabanının geçerli değerlerini yansıtmak için eşzamanlılık belirtecinin orijinal değerlerini yenileyin.
4. Çakışma gerçekleşene kadar işlemi yeniden deneyin.

Aşağıdaki örnekte, `Person.FirstName` ve `Person.LastName` eşzamanlılık belirteçleri olarak ayarlanır. Kaydedilecek değeri seçmek `// TODO:` için uygulamaya özgü mantığı dahil ettiğiniz konumda bir açıklama vardır.

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
