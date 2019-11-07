---
title: Devralma (Ilişkisel veritabanı)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 381d1878007bb78b359eb49649f4356f1e5eb04a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655639"
---
# <a name="inheritance-relational-database"></a>Devralma (İlişkisel Veritabanı)

> [!NOTE]  
> Bu bölümdeki yapılandırma genel olarak ilişkisel veritabanları için geçerlidir. Burada gösterilen uzantı yöntemleri, bir ilişkisel veritabanı sağlayıcısı yüklediğinizde (paylaşılan *Microsoft. EntityFrameworkCore. ilişkisel* paketi nedeniyle) kullanılabilir hale gelir.

EF modelindeki devralma, varlık sınıflarında devralmanın veritabanında nasıl temsil edileceğini denetlemek için kullanılır.

> [!NOTE]  
> Şu anda EF Core ' de yalnızca hiyerarşi başına tablo (TPH) düzeni uygulanır. Tablo/tür (TPT) ve tablo başına somut-tür (TPC) gibi diğer yaygın desenler henüz kullanılamamaktadır.

## <a name="conventions"></a>Kurallar

Kural gereği, devralma, hiyerarşi başına tablo (TPH) düzeni kullanılarak eşleştirilir. TPH, hiyerarşideki tüm türlerin verilerini depolamak için tek bir tablo kullanır. Bir Ayrıştırıcı sütunu, her bir satırın hangi tür temsil ettiğini belirlemek için kullanılır.

EF Core, yalnızca modele açıkça iki veya daha fazla devralınmış tür varsa devralma ayarı yapılır (daha fazla ayrıntı için bkz. [Devralma](../inheritance.md) ).

Aşağıda, bir basit devralma senaryosunu ve TPH deseninin kullanıldığı bir ilişkisel veritabanı tablosunda depolanan verileri gösteren bir örnek verilmiştir. *Ayrıştırıcı* sütunu, her satırda hangi *Blog* türünün depolandığını tanımlar.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![görüntü](_static/inheritance-tph-data.png)

>[!NOTE]
> Veritabanı sütunları, TPH eşlemesi kullanılırken gerektiğinde otomatik olarak null yapılabilir hale getirilir.

## <a name="data-annotations"></a>Veri Açıklamaları

Devralma yapılandırmak için veri ek açıklamalarını kullanamazsınız.

## <a name="fluent-api"></a>Akıcı API

Ayrıştırıcı sütununun adını ve türünü ve hiyerarşideki her türü tanımlamak için kullanılan değerleri yapılandırmak için Floent API 'sini kullanabilirsiniz.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Ayrıştırıcı özelliğini yapılandırma

Yukarıdaki örneklerde, ayrıştırıcı hiyerarşinin temel varlığında bir [Gölge Özellik](xref:core/modeling/shadow-properties) olarak oluşturulur. Modeldeki bir özellik olduğu için, diğer özellikler gibi yapılandırılabilir. Örneğin, varsayılan, kural olarak ayrıştırıcı kullanılıyorsa en fazla uzunluğu ayarlamak için:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Ayrıştırıcı, varlığınızda gerçek bir CLR özelliğine de eşleştirilebilir. Örneğin:

```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Bu iki şeyi birlikte birleştirmek, Ayrıştırıcıyı gerçek bir özellik ile eşleştirmek ve yapılandırmak mümkündür:

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
