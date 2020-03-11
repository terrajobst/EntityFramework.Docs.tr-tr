---
title: Eşzamanlılık çakışmalarını işleme-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419694"
---
# <a name="handling-concurrency-conflicts"></a>Eşzamanlılık Çakışmalarını İşleme
İyimser eşzamanlılık, varlığınızın, varlık yüklenmeden bu yana değiştirilmediğinden verileri veritabanına kaydetmeye yönelik optimizasyonu yapmayı içerir. Verilerin değiştiğini algıladığında, bir özel durum oluşturulur ve yeniden kaydetmeyi denemeden önce çakışmayı çözmeniz gerekir. Bu konu, Entity Framework özel durumların nasıl işleneceğini ele alır. Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

Bu gönderi, iyimser eşzamanlılık hakkında tam bir tartışma için uygun yer değildir. Aşağıdaki bölümler, eşzamanlılık çözümünden oluşan bazı bilgileri varsayar ve ortak görevler için desenleri gösterir.  

Bu desenlerin çoğu, [özellik değerleriyle çalışma](~/ef6/saving/change-tracking/property-values.md)bölümünde ele alınan konuları kullanır.  

Bağımsız ilişkilendirmeler kullanırken eşzamanlılık sorunlarını çözme (yabancı anahtar, varlığınızda bir özellikle eşlenmiyor), yabancı anahtar ilişkilendirmelerini kullanırken çok daha zordur. Bu nedenle, uygulamanızda eşzamanlılık çözümlemesi yapacaksanız, her zaman yabancı anahtarları varlıklarınıza eşlemeniz önerilir. Aşağıdaki örneklerde, yabancı anahtar ilişkilendirmelerini kullandığınız varsayılır.  

Yabancı anahtar ilişkilendirmelerini kullanan bir varlık kaydedilmeye çalışılırken bir iyimser eşzamanlılık özel durumu algılandığında, SaveChanges tarafından bir DbUpdateConcurrencyException oluşturulur.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Yeniden yükleme ile iyimser eşzamanlılık özel durumlarını çözme (veritabanı WINS)  

Yeniden yükleme yöntemi, varlığın geçerli değerlerinin üzerine veritabanında şu değerlerle birlikte yazmak için kullanılabilir. Daha sonra varlık, genellikle kullanıcı tarafından bir formda geri verilir ve değişiklikleri yeniden yapıp yeniden kaydetmeniz gerekir. Örnek:  

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

Eşzamanlılık özel durumunun benzetimini yapmanın iyi bir yolu, SaveChanges çağrısında bir kesme noktası ayarlamak ve sonra SQL Management Studio gibi başka bir aracı kullanarak veritabanına kaydedilen bir varlığı değiştirmektir. Ayrıca, veritabanının SqlCommand 'ı kullanarak doğrudan güncelleştirilmesini sağlamak için SaveChanges 'tan önce bir satır ekleyebilirsiniz. Örnek:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

DbUpdateConcurrencyException üzerindeki Entries yöntemi, güncellenemedi olan varlıkların DbEntityEntry örneklerini döndürür. (Bu özellik şu anda eşzamanlılık sorunları için her zaman tek bir değer döndürür. Genel güncelleştirme özel durumları için birden çok değer döndürebilir.) Bazı durumlar için bir alternatif, veritabanından yeniden yüklenmesi gerekebilecek tüm varlıkların girdilerini almak ve bunların her biri için yeniden yükleme çağırmak olabilir.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>İstemci WINS olarak iyimser eşzamanlılık özel durumlarını çözme  

Yukarıdaki örnek, veritabanındaki değerlerin üzerine yazılacak şekilde, yeniden yükleme kullanan yukarıdaki örnek bazen veritabanı WINS veya mağaza WINS olarak adlandırılır. Bazen bunun tersini yapmak isteyebilirsiniz ve veritabanındaki değerlerin üzerine varlık içinde olan değerleri yazabilirsiniz. Bu bazen istemci WINS olarak adlandırılır ve geçerli veritabanı değerlerini alarak ve varlık için özgün değerler olarak ayarlanarak yapılabilir. (Bkz. geçerli ve orijinal değerler hakkında bilgi için bkz. [özellik değerleriyle çalışma](~/ef6/saving/change-tracking/property-values.md) .) Örneğin:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>İyimser eşzamanlılık özel durumlarının özel çözümlemesi  

Bazen veritabanında Şu anda varlıktaki değerleri birleştirmek isteyebilirsiniz. Bu genellikle bazı özel Logic veya kullanıcı etkileşimini gerektirir. Örneğin, kullanıcıya geçerli değerleri, veritabanındaki değerleri ve çözümlenen değerlerin varsayılan bir kümesini içeren bir form sunabilirsiniz. Daha sonra Kullanıcı çözümlenmiş değerleri gerektiği gibi düzenleyebilir ve veritabanına kaydedilen bu çözümlenmiş değerlerdir. Bu işlem, varlık girişinde CurrentValues ve GetDatabaseValues 'tan döndürülen DbPropertyValues nesneleri kullanılarak yapılabilir. Örnek:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Nesneler kullanılarak iyimser eşzamanlılık özel durumlarının özel çözümlenmesi  

Yukarıdaki kod, geçerli, veritabanı ve çözümlenen değerleri geçirmek için DbPropertyValues örnekleri kullanır. Bazen bunun için varlık türünün örneklerinin kullanılması daha kolay olabilir. Bu, DbPropertyValues 'un ToObject ve SetValues yöntemleri kullanılarak yapılabilir. Örnek:  

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
