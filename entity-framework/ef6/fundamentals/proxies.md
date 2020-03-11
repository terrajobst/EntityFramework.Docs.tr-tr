---
title: Proxy 'ler ile çalışma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419341"
---
# <a name="working-with-proxies"></a><span data-ttu-id="37a2f-102">Proxy ile çalışma</span><span class="sxs-lookup"><span data-stu-id="37a2f-102">Working with proxies</span></span>
<span data-ttu-id="37a2f-103">POCO varlık türlerinin örneklerini oluştururken, Entity Framework genellikle varlık için proxy görevi gören dinamik olarak oluşturulan türetilmiş türün örneklerini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37a2f-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="37a2f-104">Bu proxy, özellik erişildiği zaman otomatik olarak eylem gerçekleştirmeye yönelik kancalar eklemek için varlığın bazı sanal özelliklerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="37a2f-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="37a2f-105">Örneğin, bu mekanizma ilişkilerin geç yüklemesini desteklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="37a2f-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="37a2f-106">Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="37a2f-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="37a2f-107">Proxy oluşturma devre dışı bırakılıyor</span><span class="sxs-lookup"><span data-stu-id="37a2f-107">Disabling proxy creation</span></span>  

<span data-ttu-id="37a2f-108">Bazen Entity Framework proxy örnekleri oluşturmasını engellemek yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="37a2f-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="37a2f-109">Örneğin, proxy olmayan örneklerin serileştirilmesi proxy örneklerinden serileştirilden çok daha kolay.</span><span class="sxs-lookup"><span data-stu-id="37a2f-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="37a2f-110">ProxyCreationEnabled bayrağı temizlenerek proxy oluşturma kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="37a2f-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="37a2f-111">Bunu yapabileceğiniz bir yer, bağlamınızın kurucusudur.</span><span class="sxs-lookup"><span data-stu-id="37a2f-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="37a2f-112">Örnek:</span><span class="sxs-lookup"><span data-stu-id="37a2f-112">For example:</span></span>  

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

<span data-ttu-id="37a2f-113">EF 'in proxy 'nin yapması için hiçbir şey olmadığı türler için proxy oluşturmayacak olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="37a2f-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="37a2f-114">Bu, korumalı ve/veya hiç sanal özelliği olmayan türlere sahip olan proxy 'leri de önlemenize yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="37a2f-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="37a2f-115">Bir ara sunucu örneğini açıkça oluşturma</span><span class="sxs-lookup"><span data-stu-id="37a2f-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="37a2f-116">New işlecini kullanarak bir varlığın örneğini oluşturursanız bir proxy örneği oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="37a2f-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="37a2f-117">Bu bir sorun olmayabilir, ancak bir ara sunucu örneği oluşturmanız gerekiyorsa (örneğin, yavaş yükleme veya ara sunucu değişikliği izlemenin çalışması için), bunu DbSet oluşturma yöntemini kullanarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37a2f-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="37a2f-118">Örnek:</span><span class="sxs-lookup"><span data-stu-id="37a2f-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="37a2f-119">Türetilmiş varlık türünün bir örneğini oluşturmak isterseniz, Create öğesinin genel sürümü kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="37a2f-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="37a2f-120">Örnek:</span><span class="sxs-lookup"><span data-stu-id="37a2f-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="37a2f-121">Oluşturma yönteminin oluşturulan varlığı içeriğine eklemediğini veya ekleyemediğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="37a2f-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="37a2f-122">Oluşturma yönteminin yalnızca varlık türünün bir örneğini oluşturacaksa, varlık için bir proxy türü oluşturmak hiçbir değer yoksa, hiçbir şey yapmamasını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="37a2f-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="37a2f-123">Örneğin, varlık türü mühürlü ve/veya sanal özellikleri yoksa, Oluştur yalnızca varlık türünün bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37a2f-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="37a2f-124">Asıl varlık türünü bir proxy türünden alma</span><span class="sxs-lookup"><span data-stu-id="37a2f-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="37a2f-125">Proxy türleri şuna benzer adlara sahiptir:</span><span class="sxs-lookup"><span data-stu-id="37a2f-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="37a2f-126">System. Data. Entity. DynamicProxy 'Leri. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="37a2f-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="37a2f-127">Bu proxy türünün varlık türünü ObjectContext 'ten GetObjectType metodunu kullanarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37a2f-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="37a2f-128">Örnek:</span><span class="sxs-lookup"><span data-stu-id="37a2f-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="37a2f-129">GetObjectType öğesine geçirilen tür bir proxy türü olmayan bir varlık türü örneğidir, daha sonra varlık türü de döndürülür.</span><span class="sxs-lookup"><span data-stu-id="37a2f-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="37a2f-130">Bu, türün bir proxy türü olup olmadığını görmek için herhangi bir denetim olmadan gerçek varlık türünü almak üzere her zaman bu yöntemi kullanabileceğiniz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="37a2f-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
