---
title: DbSets 'leri tanımlama-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419096"
---
# <a name="defining-dbsets"></a><span data-ttu-id="430b2-102">DbSets 'leri tanımlama</span><span class="sxs-lookup"><span data-stu-id="430b2-102">Defining DbSets</span></span>
<span data-ttu-id="430b2-103">Code First iş akışıyla geliştirilirken, veritabanı ile oturumunuzu temsil eden ve modelinizdeki her bir tür için bir DbSet sunan türetilmiş bir DbContext tanımlarsınız.</span><span class="sxs-lookup"><span data-stu-id="430b2-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="430b2-104">Bu konu, DbSet özelliklerini tanımlayabilmeniz için çeşitli yollar içerir.</span><span class="sxs-lookup"><span data-stu-id="430b2-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="430b2-105">DbSet özelliklerine sahip DbContext</span><span class="sxs-lookup"><span data-stu-id="430b2-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="430b2-106">Code First örneklerde gösterilen yaygın durum, modelinizin varlık türleri için ortak otomatik DbSet özelliklerine sahip bir DbContext 'e sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="430b2-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="430b2-107">Örnek:</span><span class="sxs-lookup"><span data-stu-id="430b2-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="430b2-108">Code First modunda kullanıldığında, bu, blogların ve gönderilerin varlık türleri olarak yapılandırılmasını ve bu kaynaklardan erişilebilen diğer türleri yapılandırmasını sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="430b2-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="430b2-109">Ayrıca, DbContext de ilgili DbSet 'in bir örneğini ayarlamak için bu özelliklerin her biri için ayarlayıcısını otomatik olarak çağırır.</span><span class="sxs-lookup"><span data-stu-id="430b2-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="430b2-110">Idbset özellikleriyle DbContext</span><span class="sxs-lookup"><span data-stu-id="430b2-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="430b2-111">Bir arabirim kullanarak küme özelliklerinizi bildirmek için daha yararlı olduğu gibi, ne tür bir veya Fakes oluştururken olduğu gibi durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="430b2-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="430b2-112">Bu gibi durumlarda, ıdbset arabirimi DbSet yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="430b2-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="430b2-113">Örnek:</span><span class="sxs-lookup"><span data-stu-id="430b2-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="430b2-114">Bu bağlam, küme özellikleri için DbSet sınıfını kullanan bağlamla tam olarak aynı şekilde çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="430b2-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="430b2-115">Salt okuma kümesi özelliklerine sahip DbContext</span><span class="sxs-lookup"><span data-stu-id="430b2-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="430b2-116">DbSet veya ıdbset özelliklerine ilişkin ortak ayarlayıcıları göstermek istemiyorsanız, bunun yerine salt okunurdur özellikler oluşturabilir ve küme örneklerini kendiniz oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="430b2-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="430b2-117">Örnek:</span><span class="sxs-lookup"><span data-stu-id="430b2-117">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

<span data-ttu-id="430b2-118">DbContext 'in set yönteminden döndürülen DbSet örneğini önbelleğe aldığından, bu özelliklerin her çağrılışında aynı örneği döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="430b2-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="430b2-119">Code First için varlık türlerinin bulunması, ortak alıcıları ve ayarlayıcıları olan özellikler için yaptığı gibi, burada aynı şekilde çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="430b2-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
