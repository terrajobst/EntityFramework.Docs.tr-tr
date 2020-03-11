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
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="280cf-102">Eşzamanlılık Çakışmalarını İşleme</span><span class="sxs-lookup"><span data-stu-id="280cf-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="280cf-103">İyimser eşzamanlılık, varlığınızın, varlık yüklenmeden bu yana değiştirilmediğinden verileri veritabanına kaydetmeye yönelik optimizasyonu yapmayı içerir.</span><span class="sxs-lookup"><span data-stu-id="280cf-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="280cf-104">Verilerin değiştiğini algıladığında, bir özel durum oluşturulur ve yeniden kaydetmeyi denemeden önce çakışmayı çözmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="280cf-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="280cf-105">Bu konu, Entity Framework özel durumların nasıl işleneceğini ele alır.</span><span class="sxs-lookup"><span data-stu-id="280cf-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="280cf-106">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="280cf-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="280cf-107">Bu gönderi, iyimser eşzamanlılık hakkında tam bir tartışma için uygun yer değildir.</span><span class="sxs-lookup"><span data-stu-id="280cf-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="280cf-108">Aşağıdaki bölümler, eşzamanlılık çözümünden oluşan bazı bilgileri varsayar ve ortak görevler için desenleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="280cf-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="280cf-109">Bu desenlerin çoğu, [özellik değerleriyle çalışma](~/ef6/saving/change-tracking/property-values.md)bölümünde ele alınan konuları kullanır.</span><span class="sxs-lookup"><span data-stu-id="280cf-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="280cf-110">Bağımsız ilişkilendirmeler kullanırken eşzamanlılık sorunlarını çözme (yabancı anahtar, varlığınızda bir özellikle eşlenmiyor), yabancı anahtar ilişkilendirmelerini kullanırken çok daha zordur.</span><span class="sxs-lookup"><span data-stu-id="280cf-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="280cf-111">Bu nedenle, uygulamanızda eşzamanlılık çözümlemesi yapacaksanız, her zaman yabancı anahtarları varlıklarınıza eşlemeniz önerilir.</span><span class="sxs-lookup"><span data-stu-id="280cf-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="280cf-112">Aşağıdaki örneklerde, yabancı anahtar ilişkilendirmelerini kullandığınız varsayılır.</span><span class="sxs-lookup"><span data-stu-id="280cf-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="280cf-113">Yabancı anahtar ilişkilendirmelerini kullanan bir varlık kaydedilmeye çalışılırken bir iyimser eşzamanlılık özel durumu algılandığında, SaveChanges tarafından bir DbUpdateConcurrencyException oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="280cf-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="280cf-114">Yeniden yükleme ile iyimser eşzamanlılık özel durumlarını çözme (veritabanı WINS)</span><span class="sxs-lookup"><span data-stu-id="280cf-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="280cf-115">Yeniden yükleme yöntemi, varlığın geçerli değerlerinin üzerine veritabanında şu değerlerle birlikte yazmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="280cf-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="280cf-116">Daha sonra varlık, genellikle kullanıcı tarafından bir formda geri verilir ve değişiklikleri yeniden yapıp yeniden kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="280cf-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="280cf-117">Örnek:</span><span class="sxs-lookup"><span data-stu-id="280cf-117">For example:</span></span>  

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

<span data-ttu-id="280cf-118">Eşzamanlılık özel durumunun benzetimini yapmanın iyi bir yolu, SaveChanges çağrısında bir kesme noktası ayarlamak ve sonra SQL Management Studio gibi başka bir aracı kullanarak veritabanına kaydedilen bir varlığı değiştirmektir.</span><span class="sxs-lookup"><span data-stu-id="280cf-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="280cf-119">Ayrıca, veritabanının SqlCommand 'ı kullanarak doğrudan güncelleştirilmesini sağlamak için SaveChanges 'tan önce bir satır ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="280cf-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="280cf-120">Örnek:</span><span class="sxs-lookup"><span data-stu-id="280cf-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="280cf-121">DbUpdateConcurrencyException üzerindeki Entries yöntemi, güncellenemedi olan varlıkların DbEntityEntry örneklerini döndürür.</span><span class="sxs-lookup"><span data-stu-id="280cf-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="280cf-122">(Bu özellik şu anda eşzamanlılık sorunları için her zaman tek bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="280cf-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="280cf-123">Genel güncelleştirme özel durumları için birden çok değer döndürebilir.) Bazı durumlar için bir alternatif, veritabanından yeniden yüklenmesi gerekebilecek tüm varlıkların girdilerini almak ve bunların her biri için yeniden yükleme çağırmak olabilir.</span><span class="sxs-lookup"><span data-stu-id="280cf-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="280cf-124">İstemci WINS olarak iyimser eşzamanlılık özel durumlarını çözme</span><span class="sxs-lookup"><span data-stu-id="280cf-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="280cf-125">Yukarıdaki örnek, veritabanındaki değerlerin üzerine yazılacak şekilde, yeniden yükleme kullanan yukarıdaki örnek bazen veritabanı WINS veya mağaza WINS olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="280cf-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="280cf-126">Bazen bunun tersini yapmak isteyebilirsiniz ve veritabanındaki değerlerin üzerine varlık içinde olan değerleri yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="280cf-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="280cf-127">Bu bazen istemci WINS olarak adlandırılır ve geçerli veritabanı değerlerini alarak ve varlık için özgün değerler olarak ayarlanarak yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="280cf-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="280cf-128">(Bkz. geçerli ve orijinal değerler hakkında bilgi için bkz. [özellik değerleriyle çalışma](~/ef6/saving/change-tracking/property-values.md) .) Örneğin:</span><span class="sxs-lookup"><span data-stu-id="280cf-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="280cf-129">İyimser eşzamanlılık özel durumlarının özel çözümlemesi</span><span class="sxs-lookup"><span data-stu-id="280cf-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="280cf-130">Bazen veritabanında Şu anda varlıktaki değerleri birleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="280cf-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="280cf-131">Bu genellikle bazı özel Logic veya kullanıcı etkileşimini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="280cf-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="280cf-132">Örneğin, kullanıcıya geçerli değerleri, veritabanındaki değerleri ve çözümlenen değerlerin varsayılan bir kümesini içeren bir form sunabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="280cf-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="280cf-133">Daha sonra Kullanıcı çözümlenmiş değerleri gerektiği gibi düzenleyebilir ve veritabanına kaydedilen bu çözümlenmiş değerlerdir.</span><span class="sxs-lookup"><span data-stu-id="280cf-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="280cf-134">Bu işlem, varlık girişinde CurrentValues ve GetDatabaseValues 'tan döndürülen DbPropertyValues nesneleri kullanılarak yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="280cf-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="280cf-135">Örnek:</span><span class="sxs-lookup"><span data-stu-id="280cf-135">For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="280cf-136">Nesneler kullanılarak iyimser eşzamanlılık özel durumlarının özel çözümlenmesi</span><span class="sxs-lookup"><span data-stu-id="280cf-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="280cf-137">Yukarıdaki kod, geçerli, veritabanı ve çözümlenen değerleri geçirmek için DbPropertyValues örnekleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="280cf-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="280cf-138">Bazen bunun için varlık türünün örneklerinin kullanılması daha kolay olabilir.</span><span class="sxs-lookup"><span data-stu-id="280cf-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="280cf-139">Bu, DbPropertyValues 'un ToObject ve SetValues yöntemleri kullanılarak yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="280cf-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="280cf-140">Örnek:</span><span class="sxs-lookup"><span data-stu-id="280cf-140">For example:</span></span>  

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
