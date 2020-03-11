---
title: DbContext-EF6 ile çalışma
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416324"
---
# <a name="working-with-dbcontext"></a>DbContext ile Çalışma

.NET nesnelerini kullanarak verileri sorgulamak, eklemek, güncelleştirmek ve silmek için, önce modelinizde tanımlanan varlıkları ve ilişkileri bir veritabanındaki tablolarla eşleyen [bir model oluşturmanız](~/ef6/modeling/index.md) gerekir. Entity Framework

Bir modelinize sahip olduğunuzda, uygulamanızın etkileşimde bulunduğu birincil sınıf `System.Data.Entity.DbContext` (genellikle bağlam sınıfı olarak adlandırılır). Bir modelle ilişkili bir DbContext kullanarak şunları yapabilirsiniz:
- Sorguları yazma ve yürütme   
- Sorgu sonuçlarını varlık nesneleri olarak gerçekleştirme
- Bu nesnelerde yapılan değişiklikleri izleme
- Nesne değişikliklerini veritabanında geri Sürdür
- Nesneleri bellekten UI denetimlerine bağlama

Bu sayfa, bağlam sınıfının nasıl yönetilebileceğine ilişkin yönergeler sunar.  

## <a name="defining-a-dbcontext-derived-class"></a>DbContext türetilmiş sınıfı tanımlama  

Bağlam ile çalışmanın önerilen yolu, DbContext 'ten türetilen bir sınıf tanımlamaktır ve bağlamda belirtilen varlıkların koleksiyonlarını temsil eden DbSet özelliklerini kullanıma sunar. EF Designer ile çalışıyorsanız, bağlam sizin için oluşturulur. Code First ile çalışıyorsanız, genellikle bağlamı yazarsınız.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

Bir bağlama sahip olduktan sonra, bu özellikler aracılığıyla bağlam içindeki `Attach` `Add` (`Remove`) veya bağlama () varlıklarını sorgular. Bağlam nesnesindeki bir `DbSet` özelliğine erişilmesi, belirtilen türdeki tüm varlıkları döndüren bir başlangıç sorgusunu temsil eder. Yalnızca bir özelliğe erişmenin sorguyu yürütmeyeceğini unutmayın. Şu durumlarda bir sorgu yürütülür:  

- `foreach` (C#) veya `For Each` (Visual Basic) ifadesiyle numaralandırılır.  
- `ToArray`, `ToDictionary`veya `ToList`gibi bir koleksiyon işlemi tarafından numaralandırılır.  
- `First` veya `Any` gibi LINQ işleçleri sorgunun en dıştaki bölümünde belirtilmiştir.  
- Aşağıdaki yöntemlerden biri çağrılır: belirtilen anahtara sahip bir varlık bağlamda zaten yüklü değilse `Load` uzantı yöntemi, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`ve `DbSet<T>.Find`.  

## <a name="lifetime"></a>Ömür  

Bağlam yaşam süresi, örnek oluşturulduğunda başlar ve örnek atıldığında ya da atık toplanmış olduğunda sona erer. Bağlam denetimlerine ait tüm kaynakların bloğun sonunda atıldığından emin olmak istiyorsanız **kullanarak** kullanın. **Using**kullandığınızda, derleyici otomatik olarak bir try/finally bloğu oluşturur ve **finally** bloğunda Dispose çağırır.  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

Bağlam ömrü üzerinde karar verirken bazı genel yönergeler aşağıda verilmiştir:  

- Web uygulamalarıyla çalışırken, istek başına bir bağlam örneği kullanın.  
- Windows Presentation Foundation (WPF) veya Windows Forms çalışırken, form başına bir bağlam örneği kullanın. Bu, içeriğin sağladığı değişiklik izleme işlevini kullanmanıza olanak sağlar.  
- Bağlam örneği bir bağımlılık ekleme kapsayıcısı tarafından oluşturulduysa, genellikle kapsayıcının, bağlamı atma sorumluluğunda olur.
- Bağlam uygulama kodunda oluşturulduysa, artık gerekli olmadığında bağlamı atmayı unutmayın.  
- Uzun süre çalışan bağlamla çalışırken aşağıdakileri göz önünde bulundurun:  
    - Daha fazla nesne ve başvuruları belleğe yüklerken, içeriğin bellek tüketimi hızla artırılabilir. Bu durum performans sorunlarına neden olabilir.  
    - Bağlam, iş parçacığı açısından güvenli değildir, bu nedenle aynı anda üzerinde çalışan birden çok iş parçacığı arasında paylaşılmamalıdır.
    - Bir özel durum bağlamın kurtarılamaz durumda olmasına neden olursa tüm uygulama sonlandırılabilir.  
    - Eşzamanlılık ile ilgili sorunlara çalıştırmanın olasılığı, verilerin sorgulandığı ve güncelleştirildiği saat arasındaki boşluk olarak artar.  

## <a name="connections"></a>Bağlantılar  

Varsayılan olarak, bağlam veritabanıyla bağlantıları yönetir. Bağlam, gerektiğinde bağlantıları açar ve kapatır. Örneğin, bağlam bir sorguyu yürütmek için bir bağlantı açar ve ardından tüm sonuç kümeleri işlendiğinde bağlantıyı kapatır.  

Bağlantı açılıp kapanışında daha fazla denetime sahip olmak istediğiniz durumlar vardır. Örneğin, SQL Server Compact ile çalışırken, performansı artırmak için uygulamanın kullanım ömrü boyunca veritabanına ayrı bir açık bağlantı sağlamak önerilir. `Connection` özelliğini kullanarak bu işlemi el ile yönetebilirsiniz.  
