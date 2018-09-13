---
title: Eşzamanlılık çakışmalarını - EF6 işleme
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489160"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="3e576-102">Eşzamanlılık çakışmalarını işleme</span><span class="sxs-lookup"><span data-stu-id="3e576-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="3e576-103">İyimser eşzamanlılık açıklamayı kaçırmadığından içerir varlığınız veriler varlık bu yana değişmemiştir umuduyla veritabanına kaydetme girişiminde yüklendi.</span><span class="sxs-lookup"><span data-stu-id="3e576-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="3e576-104">Gösterilmiş, bir özel durum ve kaydetmeyi yeniden denemeden önce çakışmayı çözmelisiniz sonra veri değişti.</span><span class="sxs-lookup"><span data-stu-id="3e576-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="3e576-105">Bu konu, Entity Framework, bu tür özel durumların nasıl işleneceğini kapsar.</span><span class="sxs-lookup"><span data-stu-id="3e576-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="3e576-106">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="3e576-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="3e576-107">Bu gönderi, iyimser eşzamanlılık tam bir irdelemesi için uygun bir yerde değil.</span><span class="sxs-lookup"><span data-stu-id="3e576-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="3e576-108">Aşağıdaki bölümler, eşzamanlılık çözümleme biraz bilgi varsayar ve ortak görevler için desenleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="3e576-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="3e576-109">Bu düzenleri birçoğu olun açıklandığı konularda kullanım [özellik değerleri ile çalışma](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="3e576-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="3e576-110">(Burada yabancı anahtarı varlığınız özelliğinde eşlenmedi) bağımsız ilişkilerini kullanırken eşzamanlılık sorunlarını çözme, yabancı anahtar ilişkilerini kullanırken daha çok daha zordur.</span><span class="sxs-lookup"><span data-stu-id="3e576-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="3e576-111">Uygulamanızdaki eşzamanlılık çözümleme yapmak için kullanacaksanız bu nedenle, yabancı anahtarlar her zaman, varlıklara eşleme önerilir.</span><span class="sxs-lookup"><span data-stu-id="3e576-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="3e576-112">Aşağıdaki örneklerde, yabancı anahtar ilişkilerini kullandığınız varsayılmıştır.</span><span class="sxs-lookup"><span data-stu-id="3e576-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="3e576-113">Yabancı anahtar ilişkilerini kullanan varlık kaydedilmeye çalışılırken bir iyimser eşzamanlılık özel durum tespit edildiğinde bir DbUpdateConcurrencyException SaveChanges tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3e576-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="3e576-114">Reload (veritabanı WINS) ile iyimser eşzamanlılık özel durumlar</span><span class="sxs-lookup"><span data-stu-id="3e576-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="3e576-115">Yeniden yükleme yöntemi, artık veritabanında değerlerle varlığın geçerli değerlerin üzerine yazmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3e576-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="3e576-116">Varlık sonra genellikle geri biçimdeki kullanıcıya verilir ve bunların değişiklikleri tekrar yapmanıza ve yeniden kaydetmek denemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3e576-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="3e576-117">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3e576-117">For example:</span></span>  

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

<span data-ttu-id="3e576-118">Bir eşzamanlılık özel durumunu benzetimini yapmak için en iyi yolu, SaveChanges çağrıda bir kesme noktası ayarlayın ve sonra SQL Management Studio gibi başka bir araç kullanarak veritabanında kaydedilmiş bir varlık değiştirme sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="3e576-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="3e576-119">Ayrıca, SaveChanges SqlCommand kullanarak doğrudan veritabanını güncellemek için bir satır ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3e576-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="3e576-120">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3e576-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="3e576-121">DbUpdateConcurrencyException girişleri yöntemi güncelleştirilemedi varlıkların DbEntityEntry örneği döndürür.</span><span class="sxs-lookup"><span data-stu-id="3e576-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="3e576-122">(Bu özellik şu anda her zaman eşzamanlılık sorunları için tek bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="3e576-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="3e576-123">Bu genel güncelleştirme özel durumlar için birden çok değer döndürebilir.) Girişler veritabanından yüklenmesi gereken tüm varlıkları almak için bazı durumlar için bir alternatif olabilir ve bunların her biri için çağrı yeniden yükleyin.</span><span class="sxs-lookup"><span data-stu-id="3e576-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="3e576-124">İyimser eşzamanlılık özel durumları istemci WINS çözümleme</span><span class="sxs-lookup"><span data-stu-id="3e576-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="3e576-125">Yeniden kullanan yukarıdaki örnekte veritabanı WINS adlandırılır veya varlıktaki değerler, veritabanından alınan değerlerin tarafından üzerine yazılır olduğundan depolama WINS.</span><span class="sxs-lookup"><span data-stu-id="3e576-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="3e576-126">Bazen varlıktaki değerlerle veritabanındaki değerlerin üzerine ve bunun tersini yapmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3e576-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="3e576-127">Bu, istemci WINS adlandırılır ve geçerli veritabanı değerlerini alma ve bunları varlığın özgün değer olarak ayarlayarak gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="3e576-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="3e576-128">(Bkz [özellik değerleri ile çalışma](~/ef6/saving/change-tracking/property-values.md) geçerli ve orijinal değerleri hakkında bilgi almak için.) Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3e576-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="3e576-129">İyimser eşzamanlılık özel durumların özel çözümleme</span><span class="sxs-lookup"><span data-stu-id="3e576-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="3e576-130">Bazen, veritabanı şu anda değerler şu anda varlıktaki değerler birleştirmek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3e576-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="3e576-131">Bu genellikle bazı özel mantığı ya da kullanıcı etkileşimini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3e576-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="3e576-132">Örneğin, geçerli değerler veritabanında içeren kullanıcı için bir form sunabilir ve varsayılan çözümlenen değerlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3e576-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="3e576-133">Kullanıcı çözümlenen değerleri gereken şekilde ardından düzenleyin ve bunun veritabanına kaydedilir, çözümlenen bu değerleri olur.</span><span class="sxs-lookup"><span data-stu-id="3e576-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="3e576-134">Bu yapılabilir DbPropertyValues nesneleri kullanarak CurrentValues ve GetDatabaseValues varlığın girişinde döndürdü.</span><span class="sxs-lookup"><span data-stu-id="3e576-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="3e576-135">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3e576-135">For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="3e576-136">İyimser eşzamanlılık özel durumların nesneleri kullanarak özel çözümleme</span><span class="sxs-lookup"><span data-stu-id="3e576-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="3e576-137">Yukarıdaki kod, geçerli etrafında geçirme, veritabanı ve çözümlenen değerleri DbPropertyValues örnekleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="3e576-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="3e576-138">Bazen bu varlığın örneklerinin kullanımı daha kolay olabilir.</span><span class="sxs-lookup"><span data-stu-id="3e576-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="3e576-139">Bu yapılabilir DbPropertyValues ToObject ve SetValues yöntemlerini kullanma.</span><span class="sxs-lookup"><span data-stu-id="3e576-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="3e576-140">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3e576-140">For example:</span></span>  

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
