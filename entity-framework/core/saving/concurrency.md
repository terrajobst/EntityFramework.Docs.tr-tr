---
title: Eşzamanlılık Çakışmalarını Ele Alma - EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: a1d1a5a11d482f9104691aa3c072dbd1c548e9f1
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417592"
---
# <a name="handling-concurrency-conflicts"></a>Eşzamanlılık Çakışmalarını İşleme

> [!NOTE]
> Bu sayfa, eşzamanlıbirimin EF Core'da nasıl çalıştığını ve uygulamanızdaki eşzamanlılık çakışmaları nasıl işleyeceğini belgeletir. Modelinizde eşzamanlılık belirteçleri yapılandırma hakkında ayrıntılar için [Eşzamanlılık Belirteçleri'ne](xref:core/modeling/concurrency) bakın.

> [!TIP]
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) GitHub'da görüntüleyebilirsiniz.

_Veritabanı eşzamanlılığı,_ birden çok işlem veya kullanıcının aynı anda bir veritabanındaki aynı verilere erişteği veya değiştirdiği durumları ifade eder. _Eşzamanlı denetim,_ eşzamanlı değişikliklerin varlığında veri tutarlılığı sağlamak için kullanılan belirli mekanizmaları ifade eder.

EF _Core, eşitleme_veya kilitleme yükü olmadan birden çok sürecin veya kullanıcıların bağımsız olarak değişiklik yapmalarına izin verdiği anlamına gelen iyimser eşzamanlılık denetimini uygular. İdeal durumda, bu değişiklikler birbirine müdahale etmeyecek ve bu nedenle başarılı olmak mümkün olacak. En kötü durum senaryosunda, iki veya daha fazla işlem çakışan değişiklikler yapmaya çalışır ve bunlardan yalnızca biri başarılı olmalıdır.

## <a name="how-concurrency-control-works-in-ef-core"></a>EŞZAMANLıL denetim EF Core'da nasıl çalışır?

Eşzamanlılık belirteçleri olarak yapılandırılan özellikler iyimser eşzamanlılık denetimini uygulamak için `SaveChanges`kullanılır: bir güncelleştirme veya silme işlemi sırasında gerçekleştirildiğinde, veritabanındaki eşzamanlılık belirteci değeri EF Core tarafından okunan özgün değerle karşılaştırılır.

- Değerler eşleşirse, işlem tamamlanabilir.
- Değerler eşleşmiyorsa, EF Core başka bir kullanıcının çakışan bir işlem gerçekleştirdiğini varsayar ve geçerli hareketi iptal eder.

Başka bir kullanıcının geçerli işlemle çakışan bir işlem gerçekleştirdiği durum _eşzamanlılık çakışması_olarak bilinir.

Veritabanı sağlayıcıları eşzamanlılık belirteç değerlerinin karşılaştırmasını uygulamaktan sorumludur.

İlişkisel veritabanlarında EF Core, herhangi `WHERE` `UPDATE` bir veya `DELETE` deyimin yan tümcesindeki eşzamanlılık belirteci değerini denetler. İfadeleri çalıştırdıktan sonra, EF Core etkilenen satır sayısını okur.

Satır lar etkilenmezse, eşzamanlılık çakışması algılanır `DbUpdateConcurrencyException`ve EF Core atar.

Örneğin, eşzamanlılık belirteci `LastName` `Person` olarak yapılandırmak isteyebiliriz. Daha sonra Person üzerindeki herhangi bir güncelleştirme `WHERE` işlemi, maddede eşzamanlılık denetimini içerir:

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını çözme

Önceki örnekle devam edersek, bir kullanıcı bazı değişiklikleri `Person`kaydetmeye çalışırsa, ancak `LastName`başka bir kullanıcı zaten , bir özel durum atılır.

Bu noktada, uygulama yalnızca çakışan değişiklikler nedeniyle güncelleştirmenin başarılı olmadığını kullanıcıya bildirebilir ve devam edebilir. Ancak, kullanıcıdan bu kaydın hala aynı gerçek kişiyi temsil ettiğinden emin olmasını ve işlemi yeniden denemesini isteyebilir.

Bu işlem, _eşzamanlılık çakışması çözme_bir örnektir.

Eşzamanlılık çakışması çözümlenmesi, bekleyen değişiklikleri `DbContext` geçerliden veritabanındaki değerlerle birleştirmeyi içerir. Hangi değerlerin birleştirilmesi uygulamaya göre değişir ve kullanıcı girişi tarafından yönlendirilebilir.

**Eşzamanlılık çakışmasını çözmeye yardımcı olmak için kullanılabilir üç değer kümesi vardır:**

- **Geçerli değerler,** uygulamanın veritabanına yazmaya çalıştığı değerlerdir.
- **Özgün değerler,** herhangi bir değiştirme yapılmadan önce ilk olarak veritabanından alınan değerlerdir.
- **Veritabanı değerleri,** veritabanında depolanan değerlerdir.

Eşzamanlılık çakışmalarını işlemek için genel yaklaşım:

1. Sırasında `DbUpdateConcurrencyException` `SaveChanges`catch .
2. Etkilenen `DbUpdateConcurrencyException.Entries` varlıklar için yeni bir değişiklik kümesi hazırlamak için kullanın.
3. Veritabanındaki geçerli değerleri yansıtacak şekilde eşzamanlılık belirteci orijinal değerlerini yenileyin.
4. Çakışma oluşmayana işlemi yeniden deneyin.

Aşağıdaki örnekte `Person.FirstName` ve `Person.LastName` eşzamanlılık belirteçleri olarak ayarlanır. Kaydedilecek `// TODO:` değeri seçmek için uygulamaya özgü mantık eklediğiniz konumda bir açıklama vardır.

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
