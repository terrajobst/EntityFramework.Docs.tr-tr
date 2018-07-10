---
title: Eşzamanlılık çakışmalarını - EF6 işleme
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
caps.latest.revision: 3
ms.openlocfilehash: b8608dbb4cadd60ff4ff4984583f8a9d843b3949
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912021"
---
# <a name="handling-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını işleme
İyimser eşzamanlılık açıklamayı kaçırmadığından içerir varlığınız veriler varlık bu yana değişmemiştir umuduyla veritabanına kaydetme girişiminde yüklendi. Gösterilmiş, bir özel durum ve kaydetmeyi yeniden denemeden önce çakışmayı çözmelisiniz sonra veri değişti. Bu konu, Entity Framework, bu tür özel durumların nasıl işleneceğini kapsar. Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

Bu gönderi, iyimser eşzamanlılık tam bir irdelemesi için uygun bir yerde değil. Aşağıdaki bölümler, eşzamanlılık çözümleme biraz bilgi varsayar ve ortak görevler için desenleri gösterir.  

Bu düzenleri birçoğu olun açıklandığı konularda kullanım [özellik değerleri ile çalışma](~/ef6/saving/change-tracking/property-values.md).  

(Burada yabancı anahtarı varlığınız özelliğinde eşlenmedi) bağımsız ilişkilerini kullanırken eşzamanlılık sorunlarını çözme, yabancı anahtar ilişkilerini kullanırken daha çok daha zordur. Uygulamanızdaki eşzamanlılık çözümleme yapmak için kullanacaksanız bu nedenle, yabancı anahtarlar her zaman, varlıklara eşleme önerilir. Aşağıdaki örneklerde, yabancı anahtar ilişkilerini kullandığınız varsayılmıştır.  

Yabancı anahtar ilişkilerini kullanan varlık kaydedilmeye çalışılırken bir iyimser eşzamanlılık özel durum tespit edildiğinde bir DbUpdateConcurrencyException SaveChanges tarafından oluşturulur.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Reload (veritabanı WINS) ile iyimser eşzamanlılık özel durumlar  

Yeniden yükleme yöntemi, artık veritabanında değerlerle varlığın geçerli değerlerin üzerine yazmak için kullanılabilir. Varlık sonra genellikle geri biçimdeki kullanıcıya verilir ve bunların değişiklikleri tekrar yapmanıza ve yeniden kaydetmek denemeniz gerekir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

Bir eşzamanlılık özel durumunu benzetimini yapmak için en iyi yolu, SaveChanges çağrıda bir kesme noktası ayarlayın ve sonra SQL Management Studio gibi başka bir araç kullanarak veritabanında kaydedilmiş bir varlık değiştirme sağlamaktır. Ayrıca, SaveChanges SqlCommand kullanarak doğrudan veritabanını güncellemek için bir satır ekleyebilirsiniz. Örneğin:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

DbUpdateConcurrencyException girişleri yöntemi güncelleştirilemedi varlıkların DbEntityEntry örneği döndürür. (Bu özellik şu anda her zaman eşzamanlılık sorunları için tek bir değer döndürür. Bu genel güncelleştirme özel durumlar için birden çok değer döndürebilir.) Girişler veritabanından yüklenmesi gereken tüm varlıkları almak için bazı durumlar için bir alternatif olabilir ve bunların her biri için çağrı yeniden yükleyin.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>İyimser eşzamanlılık özel durumları istemci WINS çözümleme  

Yeniden kullanan yukarıdaki örnekte veritabanı WINS adlandırılır veya varlıktaki değerler, veritabanından alınan değerlerin tarafından üzerine yazılır olduğundan depolama WINS. Bazen varlıktaki değerlerle veritabanındaki değerlerin üzerine ve bunun tersini yapmak isteyebilirsiniz. Bu, istemci WINS adlandırılır ve geçerli veritabanı değerlerini alma ve bunları varlığın özgün değer olarak ayarlayarak gerçekleştirilebilir. (Bkz [özellik değerleri ile çalışma](~/ef6/saving/change-tracking/property-values.md) geçerli ve orijinal değerleri hakkında bilgi almak için.) Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>İyimser eşzamanlılık özel durumların özel çözümleme  

Bazen, veritabanı şu anda değerler şu anda varlıktaki değerler birleştirmek isteyebilirsiniz. Bu genellikle bazı özel mantığı ya da kullanıcı etkileşimini gerektirir. Örneğin, geçerli değerler veritabanında içeren kullanıcı için bir form sunabilir ve varsayılan çözümlenen değerlerini ayarlayın. Kullanıcı çözümlenen değerleri gereken şekilde ardından düzenleyin ve bunun veritabanına kaydedilir, çözümlenen bu değerleri olur. Bu yapılabilir DbPropertyValues nesneleri kullanarak CurrentValues ve GetDatabaseValues varlığın girişinde döndürdü. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>İyimser eşzamanlılık özel durumların nesneleri kullanarak özel çözümleme  

Yukarıdaki kod, geçerli etrafında geçirme, veritabanı ve çözümlenen değerleri DbPropertyValues örnekleri kullanır. Bazen bu varlığın örneklerinin kullanımı daha kolay olabilir. Bu yapılabilir DbPropertyValues ToObject ve SetValues yöntemlerini kullanma. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
