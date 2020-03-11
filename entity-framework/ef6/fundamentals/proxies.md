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
# <a name="working-with-proxies"></a>Proxy ile çalışma
POCO varlık türlerinin örneklerini oluştururken, Entity Framework genellikle varlık için proxy görevi gören dinamik olarak oluşturulan türetilmiş türün örneklerini oluşturur. Bu proxy, özellik erişildiği zaman otomatik olarak eylem gerçekleştirmeye yönelik kancalar eklemek için varlığın bazı sanal özelliklerini geçersiz kılar. Örneğin, bu mekanizma ilişkilerin geç yüklemesini desteklemek için kullanılır. Bu konu başlığında gösterilen teknikler Code First ve EF Designer ile oluşturulan modellere eşit olarak uygulanır.  

## <a name="disabling-proxy-creation"></a>Proxy oluşturma devre dışı bırakılıyor  

Bazen Entity Framework proxy örnekleri oluşturmasını engellemek yararlı olur. Örneğin, proxy olmayan örneklerin serileştirilmesi proxy örneklerinden serileştirilden çok daha kolay. ProxyCreationEnabled bayrağı temizlenerek proxy oluşturma kapatılabilir. Bunu yapabileceğiniz bir yer, bağlamınızın kurucusudur. Örnek:  

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

EF 'in proxy 'nin yapması için hiçbir şey olmadığı türler için proxy oluşturmayacak olduğunu unutmayın. Bu, korumalı ve/veya hiç sanal özelliği olmayan türlere sahip olan proxy 'leri de önlemenize yol açabilir.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Bir ara sunucu örneğini açıkça oluşturma  

New işlecini kullanarak bir varlığın örneğini oluşturursanız bir proxy örneği oluşturulmaz. Bu bir sorun olmayabilir, ancak bir ara sunucu örneği oluşturmanız gerekiyorsa (örneğin, yavaş yükleme veya ara sunucu değişikliği izlemenin çalışması için), bunu DbSet oluşturma yöntemini kullanarak yapabilirsiniz. Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

Türetilmiş varlık türünün bir örneğini oluşturmak isterseniz, Create öğesinin genel sürümü kullanılabilir. Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Oluşturma yönteminin oluşturulan varlığı içeriğine eklemediğini veya ekleyemediğini unutmayın.  

Oluşturma yönteminin yalnızca varlık türünün bir örneğini oluşturacaksa, varlık için bir proxy türü oluşturmak hiçbir değer yoksa, hiçbir şey yapmamasını unutmayın. Örneğin, varlık türü mühürlü ve/veya sanal özellikleri yoksa, Oluştur yalnızca varlık türünün bir örneğini oluşturur.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Asıl varlık türünü bir proxy türünden alma  

Proxy türleri şuna benzer adlara sahiptir:  

System. Data. Entity. DynamicProxy 'Leri. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Bu proxy türünün varlık türünü ObjectContext 'ten GetObjectType metodunu kullanarak bulabilirsiniz. Örnek:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

GetObjectType öğesine geçirilen tür bir proxy türü olmayan bir varlık türü örneğidir, daha sonra varlık türü de döndürülür. Bu, türün bir proxy türü olup olmadığını görmek için herhangi bir denetim olmadan gerçek varlık türünü almak üzere her zaman bu yöntemi kullanabileceğiniz anlamına gelir.  
