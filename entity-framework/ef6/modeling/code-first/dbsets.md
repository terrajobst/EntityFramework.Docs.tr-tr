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
# <a name="defining-dbsets"></a>DbSets 'leri tanımlama
Code First iş akışıyla geliştirilirken, veritabanı ile oturumunuzu temsil eden ve modelinizdeki her bir tür için bir DbSet sunan türetilmiş bir DbContext tanımlarsınız. Bu konu, DbSet özelliklerini tanımlayabilmeniz için çeşitli yollar içerir.  

## <a name="dbcontext-with-dbset-properties"></a>DbSet özelliklerine sahip DbContext  

Code First örneklerde gösterilen yaygın durum, modelinizin varlık türleri için ortak otomatik DbSet özelliklerine sahip bir DbContext 'e sahip değildir. Örnek:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Code First modunda kullanıldığında, bu, blogların ve gönderilerin varlık türleri olarak yapılandırılmasını ve bu kaynaklardan erişilebilen diğer türleri yapılandırmasını sağlayacaktır. Ayrıca, DbContext de ilgili DbSet 'in bir örneğini ayarlamak için bu özelliklerin her biri için ayarlayıcısını otomatik olarak çağırır.  

## <a name="dbcontext-with-idbset-properties"></a>Idbset özellikleriyle DbContext  

Bir arabirim kullanarak küme özelliklerinizi bildirmek için daha yararlı olduğu gibi, ne tür bir veya Fakes oluştururken olduğu gibi durumlar vardır. Bu gibi durumlarda, ıdbset arabirimi DbSet yerine kullanılabilir. Örnek:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Bu bağlam, küme özellikleri için DbSet sınıfını kullanan bağlamla tam olarak aynı şekilde çalışacaktır.  

## <a name="dbcontext-with-read-only-set-properties"></a>Salt okuma kümesi özelliklerine sahip DbContext  

DbSet veya ıdbset özelliklerine ilişkin ortak ayarlayıcıları göstermek istemiyorsanız, bunun yerine salt okunurdur özellikler oluşturabilir ve küme örneklerini kendiniz oluşturabilirsiniz. Örnek:  

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

DbContext 'in set yönteminden döndürülen DbSet örneğini önbelleğe aldığından, bu özelliklerin her çağrılışında aynı örneği döndürmesi gerekir.  

Code First için varlık türlerinin bulunması, ortak alıcıları ve ayarlayıcıları olan özellikler için yaptığı gibi, burada aynı şekilde çalışmaktadır.  
