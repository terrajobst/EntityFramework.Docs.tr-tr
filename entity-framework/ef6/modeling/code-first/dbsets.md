---
title: EF6 DbSets - tanımlama
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
caps.latest.revision: 3
ms.openlocfilehash: 7be31737ea2e0d321113d8f747fa219533087ad2
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912633"
---
# <a name="defining-dbsets"></a><span data-ttu-id="7599c-102">DbSets tanımlama</span><span class="sxs-lookup"><span data-stu-id="7599c-102">Defining DbSets</span></span>
<span data-ttu-id="7599c-103">Code First iş akışıyla geliştirirken, veritabanı oturumla temsil eder ve bir olan DB modelinizdeki her türü için ortaya çıkaran türetilmiş bir DbContext tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="7599c-103">When developing with the Code First workflow you define a derived DbContext that represents you session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="7599c-104">Bu konu olan DB özelliklerini tanımlamak farklı yöntemleri kapsar.</span><span class="sxs-lookup"><span data-stu-id="7599c-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="7599c-105">DbContext olan DB özellikleri</span><span class="sxs-lookup"><span data-stu-id="7599c-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="7599c-106">Code First örneklerde gösterilen ortak durum modelinizin varlık türleri için genel otomatik olan DB özelliklere sahip bir DbContext sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="7599c-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="7599c-107">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7599c-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="7599c-108">Code First modunda kullanıldığında, bu Unicorn, Princess LadyInWaiting ve Castle varlık türleri yanı sıra diğer türleri bu erişilebilir yapılandırma olarak yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="7599c-108">When used in Code First mode, this will configure Unicorn, Princess, LadyInWaiting, and Castle as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="7599c-109">Buna ek olarak DbContext otomatik olarak ayarlayıcı uygun olan DB örneğini ayarlamak için bu özelliklerden her biri için çağırır.</span><span class="sxs-lookup"><span data-stu-id="7599c-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="7599c-110">DbContext IDbSet özelliklere sahip</span><span class="sxs-lookup"><span data-stu-id="7599c-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="7599c-111">Ne zaman oluşturma veya mocks fakes, bir arabirim kullanarak kümesi özelliklerinizi bildirmek daha kullanışlı olduğu gibi durumlarda, bir vardır.</span><span class="sxs-lookup"><span data-stu-id="7599c-111">There a situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="7599c-112">Bu gibi durumlarda IDbSet arabirimi olan DB yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="7599c-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="7599c-113">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7599c-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="7599c-114">Bu içerik kümesinin özelliklerini olan DB sınıfını kullanan bağlamı tam olarak aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="7599c-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="7599c-115">DbContext salt okunur özelliklerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="7599c-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="7599c-116">Genel ayarlayıcılar olan DB veya IDbSet özelliklerinizi için kullanıma sunmak istemiyorsanız bunun yerine salt okunur özellikler oluşturabilir ve kümesi örneklerine kendiniz oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7599c-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="7599c-117">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="7599c-117">For example:</span></span>  

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

<span data-ttu-id="7599c-118">DbContext her çağrıldığında bu özelliklerden her biri aynı örnek döndürülecektir böylece kümesi yönteminden döndürülen olan DB örneğini önbelleğe unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7599c-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="7599c-119">Bulma için Code First burada aynı şekilde çalışır varlık türleri olarak özellikleri ile genel alıcılar ve ayarlayıcılar için yapar.</span><span class="sxs-lookup"><span data-stu-id="7599c-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
