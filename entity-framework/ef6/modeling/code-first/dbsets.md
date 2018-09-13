---
title: EF6 DbSets - tanımlama
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489004"
---
# <a name="defining-dbsets"></a>DbSets tanımlama
Code First iş akışıyla geliştirirken, veritabanı, oturumla temsil eder ve bir olan DB modelinizdeki her türü için ortaya çıkaran türetilmiş bir DbContext tanımlayın. Bu konu olan DB özelliklerini tanımlamak farklı yöntemleri kapsar.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext olan DB özellikleri  

Code First örneklerde gösterilen ortak durum modelinizin varlık türleri için genel otomatik olan DB özelliklere sahip bir DbContext sağlamaktır. Örneğin:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Code First modunda kullanıldığında, bu blog ve gönderi varlık türleri yanı sıra diğer türleri bu erişilebilir yapılandırma olarak yapılandıracaksınız. Buna ek olarak DbContext otomatik olarak ayarlayıcı uygun olan DB örneğini ayarlamak için bu özelliklerden her biri için çağırır.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext IDbSet özelliklere sahip  

Ne zaman oluşturma veya mocks fakes, bir arabirim kullanarak kümesi özelliklerinizi bildirmek daha kullanışlı olduğu gibi durumlar vardır. Bu gibi durumlarda IDbSet arabirimi olan DB yerine kullanılabilir. Örneğin:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Bu içerik kümesinin özelliklerini olan DB sınıfını kullanan bağlamı tam olarak aynı şekilde çalışır.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext salt okunur özelliklerini ayarlama  

Genel ayarlayıcılar olan DB veya IDbSet özelliklerinizi için kullanıma sunmak istemiyorsanız bunun yerine salt okunur özellikler oluşturabilir ve kümesi örneklerine kendiniz oluşturun. Örneğin:  

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

DbContext her çağrıldığında bu özelliklerden her biri aynı örnek döndürülecektir böylece kümesi yönteminden döndürülen olan DB örneğini önbelleğe unutmayın.  

Bulma için Code First burada aynı şekilde çalışır varlık türleri olarak özellikleri ile genel alıcılar ve ayarlayıcılar için yapar.  
