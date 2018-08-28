---
title: Proxy - EF6 ile çalışma
author: divega
ms.date: 2016-10-23
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 7b82dd370e67d1622fc00ff5e5275721d0fc4fe1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997209"
---
# <a name="working-with-proxies"></a><span data-ttu-id="8f3db-102">Proxy ile çalışma</span><span class="sxs-lookup"><span data-stu-id="8f3db-102">Working with proxies</span></span>
<span data-ttu-id="8f3db-103">Entity Framework, genellikle POCO varlık türleri örneklerini oluştururken, varlık için bir proxy görevi gören dinamik olarak üretilen bir türetilmiş türün örneklerini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8f3db-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="8f3db-104">Bu proxy özelliği erişildiğinde eylemlerini otomatik olarak gerçekleştirmek için kancaları eklenecek varlık sanal bazı özelliklerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="8f3db-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="8f3db-105">Örneğin, bu mekanizma, ilişkilerin yavaş yükleniyor desteklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8f3db-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="8f3db-106">Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8f3db-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="8f3db-107">Proxy oluşturma devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="8f3db-107">Disabling proxy creation</span></span>  

<span data-ttu-id="8f3db-108">Bazen Entity Framework proxy örnekleri oluşturmasını önlemek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="8f3db-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="8f3db-109">Örneğin, olmayan proxy'si örneklerini serileştirmek proxy örneklerini serileştirmek daha önemli ölçüde daha kolay olur.</span><span class="sxs-lookup"><span data-stu-id="8f3db-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="8f3db-110">Proxy oluşturma ProxyCreationEnabled bayrağı temizleyerek kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="8f3db-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="8f3db-111">Bunun tek bir yerde bağlamınızın oluşturucusunda ' dir.</span><span class="sxs-lookup"><span data-stu-id="8f3db-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="8f3db-112">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8f3db-112">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="8f3db-113">EF proxy türleri için oluşturmaz unutmayın burada yapmak proxy için bir şey yoktur.</span><span class="sxs-lookup"><span data-stu-id="8f3db-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="8f3db-114">Bu, ayrıca proxy'leri mühürlendi ve/veya sanal özelliğe sahip türler tarafından önleyebilirsiniz olduğunu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="8f3db-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="8f3db-115">Açıkça bir ara sunucu örneğini oluşturma</span><span class="sxs-lookup"><span data-stu-id="8f3db-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="8f3db-116">New işleci kullanarak bir varlık örneği oluşturursanız, bir proxy örneği oluşturulmayacak.</span><span class="sxs-lookup"><span data-stu-id="8f3db-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="8f3db-117">Bu bir sorun olmayabilir ancak (örneğin, yavaş yükleniyor veya proxy değişiklik izleme çalışır böylece) proxy'si örneği oluşturmanız gerekiyorsa ardından olan DB oluşturma yöntemini kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8f3db-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="8f3db-118">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8f3db-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="8f3db-119">Türetilen varlık türünün örneğini oluşturmak istiyorsanız Oluştur genel sürümü kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8f3db-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="8f3db-120">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8f3db-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="8f3db-121">Create yöntemi değil veya oluşturulan varlığın bağlamına ekleme olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="8f3db-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="8f3db-122">Bunu herhangi bir şey yapmak çünkü Create yöntemi yalnızca varlık türünün bir örneği bir varlığın proxy türü oluşturuyorsanız oluşturacağını Not hiçbir değer yoktur.</span><span class="sxs-lookup"><span data-stu-id="8f3db-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="8f3db-123">Örneğin, varlık türü korumalı ve/veya sanal özellikleri yok oluşturma yalnızca varlık türünün bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8f3db-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="8f3db-124">Bir proxy türünden gerçek varlık türü alma</span><span class="sxs-lookup"><span data-stu-id="8f3db-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="8f3db-125">Proxy türlerinin, aşağıdakine benzer şekilde adlara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="8f3db-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="8f3db-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="8f3db-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="8f3db-127">ObjectContext GetObjectType yöntemini kullanarak bu proxy türü için varlık türü bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8f3db-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="8f3db-128">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="8f3db-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="8f3db-129">Türü geçirilmiş GetObjectType bir proxy tür varlık türü değil bir varlık türünün bir örneği hala döndürülen olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="8f3db-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="8f3db-130">Başka bir deyişle, her zaman gerçek varlık türü herhangi diğer tür proxy türü olup olmadığını görmek için denetlemeden almak için bu yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8f3db-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
