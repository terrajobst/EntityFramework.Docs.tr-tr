---
title: İşlem işleme hataları - EF6 işleme
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: a22a651851bc46e2bf1fe652b3b9a921ec22b70b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996843"
---
# <a name="handling-transaction-commit-failures"></a>İşlem işleme hatalarını işleme
> [!NOTE]
> **EF6.1 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 6.1 kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

6.1 bir parçası olarak yeni bir bağlantı dayanıklılığı özelliği için EF sunuyoruz: algılamak ve geçici bağlantı hataları hareket işleme bildirim zaman etkileyen otomatik olarak kurtarmak yeteneği. Senaryo tam ayrıntılarını en iyi blog gönderisinde açıklanan [SQL veritabanı bağlantısı ve Teklik sorunu](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  Özet olarak, bir işlem kaydı sırasında bir özel durum harekete geçirildiğinde, iki olası nedeni vardır senaryodur:  

1. İşlem yürütme sunucuda başarısız oldu
2. İşlem işleme sunucu üzerinde başarılı ancak bir bağlantı sorunu başarı bildirimi, istemciye ulaşmasını önleyen  

İlk durum uygulama veya kullanıcıya durumda, işlemi yeniden deneyebilirsiniz, ancak ikinci durum oluştuğunda deneme kaçınılmalıdır ve uygulama otomatik olarak geri yüklenemiyor. Sınama uygulama eyleminin doğru kurs seçemezsiniz bir özel durum işleme sırasında bildirildi gerçek nedeni neydi algılama özelliğini olmasıdır. İşlemin başarılı olduğunu ve doğru kurs eyleminin şeffaf bir şekilde ele, veritabanı ile bir kez daha denetleyin EF EF 6.1 yeni özelliği sağlar.  

## <a name="using-the-feature"></a>Özelliğini kullanma  

Özelliği etkinleştirmek için bir çağrı ekleyin [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) oluşturucusunun içinde **DbConfiguration**. Alışık olduğunuz **DbConfiguration**, bkz: [kod tabanlı yapılandırma](~/ef6/fundamentals/configuring/code-based.md). Bu özellik, işlem gerçekten sunucuda geçici bir hata nedeniyle başarısız oldu durumda yardımcı otomatik deneme EF6 içinde kullanıma sunduk ile birlikte kullanılabilir:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a>İşlemler nasıl izlenir  

Bu özellik etkinleştirildiğinde, EF otomatik olarak yeni bir tablo adlı veritabanına ekler **__Transactions**. Yeni bir satır, bir işlem tarafından EF oluşturulur ve işleme sırasında bir işlem hatası oluşursa, satır varlığını denetlenir her zaman bu tabloda eklenir.  

EF artık ihtiyaç duyulmayan zaman çizelgesinden satırlar ayıklamak üzere en iyi çaba İlkesi ne yapacağını olsa da, uygulamanın erken ve tablo bazı durumlarda elle temizleme gerekebilir. Bu nedenle çıkılıyorsa tablo büyüyebilir.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Önceki sürümlerle işleme hatalarını işlemek nasıl

Önce EF 6.1 yoktu EF ürün hatalarını işleme mekanizması. EF6 önceki sürümleri için uygulanabilir bu durumla ilgilenmek için çeşitli yollar vardır:  

### <a name="option-1---do-nothing"></a>1. seçenek - hiçbir şey yapma  

İşlem işleme sırasında bağlantı hatası olasılığını düşük olduğundan aslında bu durum ortaya çıkarsa, yalnızca başarısız olmasına, uygulamanız için kabul edilebilir olabilir.  

## <a name="option-2---use-the-database-to-reset-state"></a>2. seçenek - durumunu sıfırlamak için veritabanını kullan  

1. Geçerli DbContext AT  
2. Yeni bir DbContext oluşturma ve uygulama durumunu veritabanından geri yükleme  
3. Son işlemi başarıyla tamamlanmamış, kullanıcıyı bilgilendirmeniz  

## <a name="option-3---manually-track-the-transaction"></a>3. seçenek - el ile işlem izleme  

1. İzlenen olmayan tablo işlemlerin durumunu izlemek için kullanılan veritabanı ekleyin.  
2. Her işlem başındaki tabloya bir satır ekleyin.  
3. Yürütme sırasında bağlantı başarısız olursa, veritabanı karşılık gelen satırı varlığını denetleyin.  
    - Satır varsa, işlemin başarıyla yürütüldü gibi normal olarak devam eder  
    - Satır yoksa, geçerli işlemi yeniden denemek için bir yürütme stratejisi kullanın.  
4. Kaydetme başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırı silin.  

[Bu blog gönderisini](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) SQL Azure üzerinde bu işlemi gerçekleştirmek için örnek kod içerir.  
