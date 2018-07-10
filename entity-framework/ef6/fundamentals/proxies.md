---
title: Proxy - EF6 ile çalışma
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
caps.latest.revision: 3
ms.openlocfilehash: 4632e246d28a3cd53dabe5ac76e44f4538739abc
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912066"
---
# <a name="working-with-proxies"></a>Proxy ile çalışma
Entity Framework, genellikle POCO varlık türleri örneklerini oluştururken, varlık için bir proxy görevi gören dinamik olarak üretilen bir türetilmiş türün örneklerini oluşturur. Bu proxy özelliği erişildiğinde eylemlerini otomatik olarak gerçekleştirmek için kancaları eklenecek varlık sanal bazı özelliklerini geçersiz kılar. Örneğin, bu mekanizma, ilişkilerin yavaş yükleniyor desteklemek için kullanılır. Bu konuda gösterilen teknikleri Code First ve EF Designer ile oluşturulan modeller için eşit oranda geçerlidir.  

## <a name="disabling-proxy-creation"></a>Proxy oluşturma devre dışı bırakma  

Bazen Entity Framework proxy örnekleri oluşturmasını önlemek yararlıdır. Örneğin, olmayan proxy'si örneklerini serileştirmek proxy örneklerini serileştirmek daha önemli ölçüde daha kolay olur. Proxy oluşturma ProxyCreationEnabled bayrağı temizleyerek kapatılabilir. Bunun tek bir yerde bağlamınızın oluşturucusunda ' dir. Örneğin:  

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

EF proxy türleri için oluşturmaz unutmayın burada yapmak proxy için bir şey yoktur. Bu, ayrıca proxy'leri mühürlendi ve/veya sanal özelliğe sahip türler tarafından önleyebilirsiniz olduğunu anlamına gelir.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Açıkça bir ara sunucu örneğini oluşturma  

New işleci kullanarak bir varlık örneği oluşturursanız, bir proxy örneği oluşturulmayacak. Bu bir sorun olmayabilir ancak (örneğin, yavaş yükleniyor veya proxy değişiklik izleme çalışır böylece) proxy'si örneği oluşturmanız gerekiyorsa ardından olan DB oluşturma yöntemini kullanarak bunu yapabilirsiniz. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

Türetilen varlık türünün örneğini oluşturmak istiyorsanız Oluştur genel sürümü kullanılabilir. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Create yöntemi değil veya oluşturulan varlığın bağlamına ekleme olduğunu unutmayın.  

Bunu herhangi bir şey yapmak çünkü Create yöntemi yalnızca varlık türünün bir örneği bir varlığın proxy türü oluşturuyorsanız oluşturacağını Not hiçbir değer yoktur. Örneğin, varlık türü korumalı ve/veya sanal özellikleri yok oluşturma yalnızca varlık türünün bir örneğini oluşturur.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Bir proxy türünden gerçek varlık türü alma  

Proxy türlerinin, aşağıdakine benzer şekilde adlara sahiptir:  

System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

ObjectContext GetObjectType yöntemini kullanarak bu proxy türü için varlık türü bulabilirsiniz. Örneğin:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Türü geçirilmiş GetObjectType bir proxy tür varlık türü değil bir varlık türünün bir örneği hala döndürülen olduğunu unutmayın. Başka bir deyişle, her zaman gerçek varlık türü herhangi diğer tür proxy türü olup olmadığını görmek için denetlemeden almak için bu yöntemi kullanabilirsiniz.  
