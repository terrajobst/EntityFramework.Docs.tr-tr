---
title: Otomatik Code First Migrations-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419002"
---
# <a name="automatic-code-first-migrations"></a>Otomatik Code First Migrations
Otomatik geçişler, yaptığınız her değişiklik için projenizde kod dosyası olmadan Code First Migrations kullanmanıza olanak sağlar. Tüm değişiklikler otomatik olarak uygulanmayabilir; Örneğin, sütun yeniden adlandırmaları kod tabanlı geçişin kullanılmasını gerektirir.

> [!NOTE]
> Bu makalede temel senaryolarda Code First Migrations kullanmayı bildiğiniz varsayılmaktadır. Bunu yapmazsanız devam etmeden önce [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) okumanız gerekir.

## <a name="recommendation-for-team-environments"></a>Ekip ortamları için öneri

Otomatik ve kod tabanlı geçişleri birbirine dönüştürebilirsiniz, ancak bu, takım geliştirme senaryolarında önerilmez. Kaynak denetimini kullanan bir geliştirici ekibinin parçasıysa, yalnızca otomatik geçişleri veya yalnızca kod tabanlı geçişleri kullanmanız gerekir. Otomatik geçişlerin sınırlamaları verildiğinde, takım ortamlarında kod tabanlı geçişlerde kullanılması önerilir.

## <a name="building-an-initial-model--database"></a>Ilk model & veritabanı oluşturma

Geçişleri kullanmaya başlamadan önce bir proje ve ile çalışmak için bir Code First modeli gerekir. Bu izlenecek yol için kurallı **Blog** ve **gönderi** modelini kullanacağız.

-   Yeni bir **Migrationsautomaticdemo** konsol uygulaması oluşturma
-   Projeye **EntityFramework** NuGet paketinin en son sürümünü ekleyin
    -   **Araçlar –&gt; kitaplığı Paket Yöneticisi –&gt; Paket Yöneticisi konsolu**
    -   **Install-Package EntityFramework** komutunu çalıştırın
-   Aşağıda gösterilen kodla bir **model.cs** dosyası ekleyin. Bu kod, etki alanı modelimizi ve EF Code First bağlamımız bir **BlogContext** sınıfını oluşturan tek bir **Blog** sınıfını tanımlar

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   Artık bir modelimiz olduğuna göre, veri erişimi gerçekleştirmek için bunu kullanmanın zamanı. **Program.cs** dosyasını aşağıda gösterilen kodla güncelleştirin.

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   Uygulamanızı çalıştırın ve sizin için bir **Migrationsautomaticcodedemo. BlogContext** veritabanının oluşturulduğunu görürsünüz.

    ![Veritabanı Yereldb](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Geçişleri etkinleştirme

Modelinizde daha fazla değişiklik yapmak zaman alabilir.

-   Blog sınıfına bir URL özelliği tanıtalım.

``` csharp
    public string Url { get; set; }
```

Uygulamayı yeniden çalıştırırsanız, *veritabanı oluşturulduktan sonra ' BlogContext ' bağlamının yedeklendiğini belirten bir InvalidOperationException alacaksınız. Veritabanını güncelleştirmek için Code First Migrations kullanmayı düşünün (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

Özel durum önerdiğinde Code First Migrations kullanmaya başlama zamanı. Otomatik geçişleri kullanmak istiyoruz, **– Enableautomaticgeçişler** anahtarını belirteceğiz.

-   Paket Yöneticisi konsolundaki **Enable-geçişleri – Enableautomaticgeçişleri** komutunu çalıştırın bu komut, projemizdeki bir **geçiş** klasörü ekledi. Bu yeni klasör bir dosya içerir:

-   **Yapılandırma sınıfı.** Bu sınıf, bağlam için geçişlerin nasıl davranacağını yapılandırmanıza olanak tanır. Bu kılavuzda, yalnızca varsayılan yapılandırmayı kullanacağız.
    *Projenizde yalnızca tek bir Code First bağlamı olduğundan, Enable-geçişler bu yapılandırmanın uygulandığı bağlam türüne otomatik olarak doldurulur.*

 

## <a name="your-first-automatic-migration"></a>Ilk otomatik geçişiniz

Code First Migrations, öğrenecek iki birincil komuta sahiptir.

-   **Geçiş geçişi** , son geçişin oluşturulmasından bu yana modelinizde yaptığınız değişikliklere dayalı olarak bir sonraki geçişi kankatalacak
-   **Güncelleştir-veritabanı** bekleyen geçişleri veritabanına uygular

Ekleme geçişi kullanmaktan kaçının (gerçekten gerekli olmadığı durumlar dışında) ve değişiklikleri Code First Migrations otomatik olarak hesaplayıp uygulamanıza odaklanıyoruz. Değişiklikleri modelinize (yeni **blog. ur**l özelliği) veritabanına göndermek için Code First Migrations almak üzere **Update-Database** ' i kullanalım.

-   Package Manager konsolunda **Update-Database** komutunu çalıştırın.

**Migrationsautomaticdemo. BlogContext** veritabanı artık **Bloglar** tablosuna **URL** sütununu içerecek şekilde güncelleştirildi.

 

## <a name="your-second-automatic-migration"></a>Ikinci otomatik geçişiniz

Şimdi başka bir değişiklik yapıp Code First Migrations değişiklikleri otomatik olarak veritabanına itelim.

-   Ayrıca yeni bir **Post** sınıfı ekleyelim

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   Ayrıca **, blog ve** **gönderi** arasındaki ilişkinin diğer sonunu oluşturmak Için **Blog** sınıfına bir **gönderi** koleksiyonu ekleyeceğiz

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Şimdi veritabanını güncel hale getirmek için **Update-Database** kullanın. Bu süre, Code First Migrations çalışan SQL 'i görebilmeniz için **– verbose** bayrağını belirtlim.

-   Package Manager konsolundaki **Update-Database – verbose** komutunu çalıştırın.

## <a name="adding-a-code-based-migration"></a>Kod tabanlı geçiş ekleme

Şimdi, için kod tabanlı bir geçiş kullanmak isteyebileceğiniz bir şeye bakalım.

-   **Blog** sınıfına bir **Derecelendirme** özelliği ekleyelim

``` csharp
    public int Rating { get; set; }
```

Bu değişiklikleri veritabanına göndermek için yalnızca **Update-Database** ' i çalıştırabiliriz. Ancak, tablodaki mevcut veriler varsa, null olamayan bir **blog. derecelendirme** sütunu ekledik. Bu, tabloda yeni bir sütun için VERI türünün clr varsayılan değeri atanır (derecelendirme tamdır, yani **0**olur). Ancak varsayılan olarak **3** değerini belirtmek istiyoruz, bu sayede **blogların** tablosundaki mevcut satırların bir sıra derecelendirmesi ile başlaması gerekir.
Bunu düzenleyebilmemiz için, bu değişikliği kod tabanlı bir geçişe yazmak üzere Add-Migration komutunu kullanalım. **Add-Migration** komutu, bu geçişlere bir ad vermemizi sağlar, şimdi **de bizlere**çağrı yapalım.

-   Paket Yöneticisi konsolunda **Add-Migration AddBlogRating** komutunu çalıştırın.
-   **Geçişler** klasöründe **artık yeni bir ek geçiş geçişimiz** var. Geçiş dosya adı, sıralamaya yardımcı olması için zaman damgasıyla önceden düzeltilir. Web günlüğü. derecelendirmesi için varsayılan 3 değerini belirtmek üzere oluşturulan kodu düzenleyelim (aşağıdaki kodda satır 10)

*Geçişin Ayrıca bazı meta verileri yakalayan bir arka plan kod dosyası vardır. Bu meta veriler, Code First Migrations Bu kod tabanlı geçişten önce yaptığımız otomatik geçişleri çoğaltmasına izin verir. Başka bir geliştirici geçişlerimizi çalıştırmak isterse veya uygulamamızı dağıtmaya zaman geldiğinde bu önemlidir.*

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

Düzenlenmiş geçişimiz iyi arıyor, bu nedenle veritabanını güncel hale getirmek için **Update-Database** ' i kullanalım.

-   Package Manager konsolunda **Update-Database** komutunu çalıştırın.

## <a name="back-to-automatic-migrations"></a>Otomatik geçişlere geri dön

Artık daha basit değişikliklerimiz için otomatik geçişlere geçiş yapmak ücretsizdir. Code First Migrations, otomatik ve kod tabanlı geçişleri, her kod tabanlı geçiş için arka plan kod dosyasında depoladığını temel alarak doğru sırada gerçekleştirir.

-   Modelinize bir post. Abstract özelliği ekleyelim

``` csharp
    public string Abstract { get; set; }
```

Şimdi **Update-Database** ' i kullanarak, otomatik geçiş kullanarak bu değişikliği veritabanına göndermek için Code First Migrations edinebilirsiniz.

-   Package Manager konsolunda **Update-Database** komutunu çalıştırın.

## <a name="summary"></a>Özet

Bu izlenecek yolda, model değişikliklerini veritabanına göndermek için otomatik geçişleri nasıl kullanacağınızı gördünüz. Ayrıca, daha fazla denetime ihtiyacınız olduğunda otomatik geçişler arasında kod tabanlı geçişler oluşturmayı ve bunları nasıl çalıştıracağınızı da gördünüz.
