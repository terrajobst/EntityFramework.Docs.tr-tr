---
title: İşlem işleme başarısızlıklarını işleme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 27e75e6a1919ee2300fe76cfcdf67cceaad887b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417372"
---
# <a name="handling-transaction-commit-failures"></a>İşlem işleme başarısızlıklarını işleme
> [!NOTE]
> **EF 6.1 yalnızca** sonraki sürümler-bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6,1 ' de tanıtılmıştı. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

6,1 kapsamında, EF için yeni bir bağlantı dayanıklılığı özelliği sunuyoruz: geçici bağlantı arızaları işlem yürütmelerinin bildirimini etkilediği zaman otomatik olarak algılama ve kurtarma özelliği. Senaryonun tüm ayrıntıları en iyi şekilde [SQL veritabanı bağlantısı ve en etkili sorun sorunu](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)konusunda açıklanmıştır.  Özet bölümünde senaryo, işlem sırasında bir özel durum ortaya çıktığında iki olası neden vardır:  

1. Sunucuda işlem işleme başarısız oldu
2. İşlem, sunucuda başarılı bir şekilde tamamlandı, ancak bir bağlantı sorunu, Başarı bildiriminin istemciye ulaşmasını engelledi  

İlk durum uygulama olduğunda veya Kullanıcı işlemi yeniden deneyebilir, ancak ikinci durum oluştuğunda yeniden denemeler önlenmiş olmalıdır ve uygulama otomatik olarak kurtarabilir. Bu zorluk, kayıt sırasında bir özel durumun ne kadar gerçek olduğunu tespit etme yeteneği olmadan, uygulamanın doğru işlem planını seçemediğinden emin değildir. EF 6,1 ' deki yeni özellik, işlem başarılı olursa ve doğru şekilde eyleme geçmek için EF 'in veritabanına iki kez işaret etmesine olanak tanır.  

## <a name="using-the-feature"></a>Özelliğini kullanma  

İhtiyacınız olan özelliği etkinleştirmek için, **DBConfiguration**' ın oluşturucusunda [settransactionhandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) ' a bir çağrı ekleyin. **DBConfiguration**hakkında bilginiz yoksa bkz. [kod tabanlı yapılandırma](~/ef6/fundamentals/configuring/code-based.md). Bu özellik, EF6 ' de tanıtıldığımız otomatik yeniden denemeler ile birlikte kullanılabilir ve bu durum, geçici bir hata nedeniyle işlemin gerçekten sunucuda işleme amaması durumunda yardımcı olur:  

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

## <a name="how-transactions-are-tracked"></a>İşlemler nasıl izlenir?  

Özellik etkinleştirildiğinde, EF otomatik olarak **__Transactions**adlı veritabanına yeni bir tablo ekler. Her bir işlem EF tarafından oluşturulduğunda bu tabloya yeni bir satır eklenir ve kayıt sırasında bir işlem hatası oluşursa bu satır varlık için denetlenir.  

EF, artık gerek duyulmadığında tablodaki satırları ayıklamak için en iyi çaba yapabilse de, uygulama zamanından önce çıktığında tablo büyüyebilir ve bu nedenle, bazı durumlarda tabloyu el ile temizlemeniz gerekebilir.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Önceki sürümlerle işleme başarısızlıklarını işleme

EF 6,1 ' den önce EF ürününde işleme başarısızlıklarını işlemeye yönelik bir mekanizma yoktu. Önceki EF6 sürümlerine uygulanabilecek bu durumla ilgilenmenin birkaç yolu vardır:  

* Seçenek 1-hiçbir şey yapma  

  İşlem işleme sırasında bağlantı hatası olasılığı düşük olduğundan, bu durum aslında gerçekleşirse uygulamanızın başarısız olması için kabul edilebilir hale gelebilir.  

* Seçenek 2-durumu sıfırlamak için veritabanını kullanma  

  1. Geçerli DbContext 'i at  
  2. Yeni bir DbContext oluşturun ve uygulamanızın durumunu veritabanından geri yükleyin  
  3. Kullanıcıya son işlemin başarıyla tamamlanmamış olabileceğini bildirin  

* Seçenek 3-işlemi el Ile izleme  

  1. İşlemlerin durumunu izlemek için kullanılan veritabanına izlenmeyen bir tablo ekleyin.  
  2. Her bir işlemin başındaki tabloya bir satır ekleyin.  
  3. Kayıt sırasında bağlantı başarısız olursa, veritabanında karşılık gelen satırın varolup olmadığını kontrol edin.  
     - Satır mevcutsa, işlem başarıyla yürütüldüğü için normal olarak devam edin  
     - Satır yoksa, geçerli işlemi yeniden denemek için bir yürütme stratejisi kullanın.  
  4. Kayıt başarılı olursa, tablonun büyümesini önlemek için karşılık gelen satırı silin.  

[Bu blog gönderisi](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) , SQL Azure için bu kodu yerine getirmeye yönelik örnek kod içerir.  
