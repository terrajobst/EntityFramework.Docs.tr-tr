---
title: DbContext - EF6 ile çalışma
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
caps.latest.revision: 3
ms.openlocfilehash: 865c9883ce25f405a173791df4e46b98550cd41f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912048"
---
# <a name="working-with-dbcontext"></a>DbContext ile çalışma

Entity Framework sorgulama, ekleme, güncelleştirme ve .NET nesneleri kullanarak verileri silme olarak kullanabilmek için öncelikle gerekir [Model oluşturma](~/ef6/modeling/index.md) varlıkları ve modelinizi bir veritabanındaki tablolar için tanımlanan ilişkileri eşler.

Bir modeli oluşturduktan sonra uygulamanızı etkileşim birincil sınıf, `System.Data.Entity.DbContext` (genellikle bağlam sınıfını adlandırılır). Bir Modeli'ne için ilişkili bir DbContext kullanabilirsiniz:
- Yazma ve sorgular yürütme   
- Sorgu sonuçları varlık nesnesi depolanabildiği
- Bu nesnelere yapılan değişiklikleri izleme
- Nesne değişikliklerini geri veritabanında kalıcı hale
- Nesneleri bellekte bir UI denetimine bağlama

Bu sayfa, bağlam sınıfını yönetme hakkında rehberlik sağlar.  

## <a name="defining-a-dbcontext-derived-class"></a>DbContext türetilmiş bir sınıf tanımlama  

Bağlamı ile çalışmak için önerilen yöntem bağlamında belirtilen varlık koleksiyonları temsil eden olan DB özellikleri gösterir ve DbContext türetilen bir sınıf tanımlamaktır. EF Designer ile çalışıyorsanız, bağlam sizin için oluşturulur. Code First ile çalışıyorsanız, tipik olarak bağlam kendi yazdığınız.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Bir bağlam aldıktan sonra sorgulama, ekleme (kullanarak `Add` veya `Attach` yöntemleri) veya kaldırma (kullanarak `Remove`) bağlamında bu özellikleri aracılığıyla varlıklar. Erişim bir `DbSet` bir bağlam nesnesi özelliği belirtilen türün tüm varlıklarını döndüren başlayan bir sorgu temsil eder. Yalnızca bir özellik erişim sorgu yürütülmez unutmayın. Bir sorgu yürütüldüğü zaman:  

- Tarafından numaralandırılmış bir `foreach` (C#) veya `For Each` deyimi (Visual Basic).  
- Bir toplama işlemi tarafından gibi numaralandırılana `ToArray`, `ToDictionary`, veya `ToList`.  
- Gibi LINQ işleçleri `First` veya `Any` en dıştaki sorgunun parçası belirtilir.  
- Aşağıdaki yöntemlerden birini çağrılır: `Load` genişletme yöntemi `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, ve `DbSet<T>.Find`, belirtilen anahtara sahip bir varlık zaten yüklenmiş bir bağlamda bulunamadı.  

## <a name="lifetime"></a>Ömür  

Örneği oluşturulduğunda ve örnek ya da elden atık olarak toplanmış veya sona erer bağlamı ömrünü başlar. Kullanım **kullanarak** bağlam bloğunun sonunda çıkarılması için denetimleri tüm kaynakları istiyorsanız. Kullanırken **kullanarak**, derleyici bir try/finally bloğu otomatik olarak oluşturur ve dispose işlemini çağırır **son** blok.  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Bağlam ömrü hakkında karar verirken bazı genel yönergeler şunlardır:  

- Web uygulamaları ile çalışırken, istek başına bir bağlam örneği kullanın.  
- Windows Presentation Foundation (WPF) veya Windows Forms ile çalışırken, bir formda bir bağlam örneği kullanın. Bu değişiklik izleme işlevselliği kullanmanıza olanak tanır, bağlam sağlar.  
- Bağlam örneği bir bağımlılık ekleme kapsayıcısını tarafından oluşturduysanız, genellikle bağlamı atmayı kapsayıcının sorumluluğundadır.
- Uygulama kodunda bağlamı oluşturduysanız, artık gerekli olmadığında bağlamında atmayı unutmayın.  
- Uzun süre çalışan bağlamı ile çalışırken aşağıdakileri göz önünde bulundurun:  
    - Daha fazla nesne ve başvurularını belleğe yük bağlamı bellek tüketimi hızlı bir şekilde artırabilirsiniz. Bu durum, performans sorunlarına neden olabilir.  
    - Bağlam, iş parçacığı açısından güvenli değildir, bu nedenle, iş üzerindeki eşzamanlı olarak yapılması birden çok iş parçacığı arasında Paylaşılmaması gerekiyor.
    - Bir özel durum bağlamı kurtarılamaz bir duruma gelmesine neden olursa, tüm uygulama sonlandırabilir.  
    - Verilerin ne zaman sorgulanabilir ve güncelleştirilmiş saat arasındaki boşluk büyüdükçe eşzamanlılık ile ilgili bir sorunla karşılaşırsanız çalıştırma olasılığını artırın.  

## <a name="connections"></a>Bağlantıları  

Varsayılan olarak, içerik veritabanı bağlantılarını yönetir. Bağlamı açar ve gerektiğinde bağlantıları kapatır. Örneğin, içeriği bir sorgu yürütmek için bir bağlantı açar ve tüm sonuç kümelerindeki işlendiğinde ardından bağlantıyı kapatır.  

Bağlantıyı açar ve kapandığında üzerinde daha fazla denetime sahip olmasını istediğiniz zaman durumlar vardır. Örneğin, SQL Server Compact ile çalışırken, genellikle veritabanı ayrı açık bağlantı ömrü boyunca uygulama performansını korumak için önerilir. Kullanarak bu işlemi el ile yönetebilirsiniz `Connection` özelliği.  
