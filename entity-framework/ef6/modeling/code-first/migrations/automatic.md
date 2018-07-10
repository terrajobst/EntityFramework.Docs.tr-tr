---
title: Otomatik Code First geçişleri - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
caps.latest.revision: 3
ms.openlocfilehash: 1f6ed728271e38d8e65fcf33fd45c74f085d9178
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2018
ms.locfileid: "37912684"
---
# <a name="automatic-code-first-migrations"></a>Otomatik Code First geçişleri
Otomatik geçişleri yaptığınız her değişiklik için bir kod dosyası projenize zorunda kalmadan Code First Migrations kullanmanızı sağlar. Tüm değişiklikler otomatik olarak uygulanabilir; örneğin sütun yeniden adlandırma kod tabanlı bir geçiş kullanılmasını gerektirir.

> [!NOTE]
> Bu makalede, temel senaryolarda Code First Migrations nasıl kullanılacağını varsayar. Sizin sonra okumak ihtiyacınız olacak [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) devam etmeden önce.

## <a name="recommendation-for-team-environments"></a>Takım ortamları için öneri

Otomatik ve kod tabanlı geçişler aralarına koyabilirsiniz ancak bu takım geliştirme senaryolarda önerilmez. Kaynak denetimi kullanan geliştiriciler bir ekibin parçası olduğunda, tamamen otomatik geçişleri veya yalnızca kod tabanlı geçişler ya da kullanmanız gerekir. Otomatik geçişleri sınırlamaları kod tabanlı geçişler ekip ortamlarında kullanılmasını öneririz.

## <a name="building-an-initial-model--database"></a>Bir ilk Model & veritabanı oluşturma

Biz geçişleri kullanmaya başlamadan önce bir proje ve çalışmak için Code First modeli gerekir. Bu kılavuz için kurallı kullanacağız **Blog** ve **Post** modeli.

-   Yeni bir **MigrationsAutomaticDemo** konsol uygulaması
-   En son sürümünü eklemek **EntityFramework** projeye NuGet paketi
    -   **Araçları –&gt; kitaplık Paket Yöneticisi –&gt; Paket Yöneticisi Konsolu**
    -   Çalıştırma **Install-Package EntityFramework** komutu
-   Ekleme bir **Model.cs** aşağıda gösterilen kod dosyası. Bu kod, tek bir tanımlar **Blog** etki alanı modelimizi sağlar sınıfını ve **BlogContext** bizim EF Code First bağlam sınıfı

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

-   Biz bir modeliniz olduğuna göre veri erişimi gerçekleştirdiği kullanma zamanı geldi. Güncelleştirme **Program.cs** aşağıda gösterilen kod dosyası.

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

-   Uygulamanızı çalıştırın ve göreceksiniz bir **MigrationsAutomaticCodeDemo.BlogContext** veritabanı sizin için oluşturulur.

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Geçiş etkinleştiriliyor

Bu, bazı modelimiz için daha fazla değişiklik zamanı geldi.

-   Şimdi Blog sınıfına URL'si özelliği sunar.

``` csharp
    public string Url { get; set; }
```

Uygulamayı çalıştırmak için olsaydı yeniden belirten bir InvalidOperationException elde edebileceğiniz *veritabanı oluşturulduktan sonra 'BlogContext' bağlam yedekleme modeli değişti. Veritabanını güncellemek için Code First Migrations'ı kullanmayı deneyin (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](http://go.microsoft.com/fwlink/?LinkId=238269) *).*

Özel durum da anlaşılacağı gibi Code First Migrations'ı kullanmaya başlamak için zaman var. Otomatik geçişleri kullanmak istediğimizden bunu belirtmek için dağıtacağız **– EnableAutomaticMigrations** geçin.

-   Çalıştırma **etkinleştir geçişleri – EnableAutomaticMigrations** Paket Yöneticisi konsolu bu komutu bir komutta ekledi bir **geçişler** Projemizin klasörüne. Bu yeni klasöre bir dosya içerir:

-   **Yapılandırma sınıfı.** Bu sınıf geçişleri içeriğiniz için nasıl davranacağını yapılandırmanıza olanak sağlar. Bu kılavuz için yalnızca varsayılan yapılandırmayı kullanacağız.
    *Projenizde yalnızca tek bir Code First bağlamı olmadığından, geçişleri etkinleştir otomatik olarak doldurulur Bu yapılandırmanın geçerli bağlam türü.*

 

## <a name="your-first-automatic-migration"></a>İlk otomatik geçiş

Code First geçişleri sahibi olacak iki birincil komutu vardır.

-   **Geçiş** son geçiş oluşturulmasından bu yana modelinize yaptığınız değişikliklere dayalı sonraki geçiş iskelesini
-   **Veritabanını Güncelleştir** geçişler bekleyen herhangi bir veritabanına uygulanır

Önlemek kullanacağız geçiş Ekle (biz aslında gerekmedikçe) kullanarak ve Code First Migrations'ı otomatik olarak izin vererek odaklanmasına hesaplamak ve değişiklikleri uygulayın. Kullanalım **veritabanını Güncelleştir** modelimiz için değişiklikleri göndermek için Code First Migrations'ı almak için (yeni **Blog.Ur**l özelliği) veritabanı.

-   Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu.

**MigrationsAutomaticDemo.BlogContext** veritabanı içerecek şekilde güncelleştirilmiş artık **Url** sütununda **blogları** tablo.

 

## <a name="your-second-automatic-migration"></a>İkinci otomatik geçiş

Başka bir değiştirme ve otomatik olarak değişiklikleri veritabanına bizim için anında iletme Code First Migrations izin olalım.

-   Ayrıca yeni bir ekleyelim **Post** sınıfı

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

-   Ayrıca ekleyeceğiz bir **gönderileri** koleksiyona **Blog** arasındaki ilişkinin diğer ucundaki formu için sınıf **Blog** ve **sonrası**

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Artık **veritabanını Güncelleştir** güncel veritabanı getirilecek. Bu süre belirtelim **– ayrıntılı** Code First Migrations'ı çalıştıran SQL görebilmeniz için bayrak.

-   Çalıştırma **veritabanını güncelleştir – ayrıntılı** Paket Yöneticisi konsolunda komutu.

## <a name="adding-a-code-based-migration"></a>Geçiş dayalı bir kod ekleme

Artık bir şey kod tabanlı bir geçiş için kullanılacak istiyoruz göz atalım.

-   Ekleyelim bir **derecelendirme** özelliğini **Blog** sınıfı

``` csharp
    public int Rating { get; set; }
```

Biz yalnızca çalıştırabileceğiniz **veritabanını Güncelleştir** bu değişiklikleri veritabanına göndermek için. Ancak, bir NULL olmayan ekliyoruz **Blogs.Rating** sütun, tablodaki tüm mevcut veriler varsa, yeni bir sütun için veri türünün CLR varsayılan atanır (derecelendirmesi, tamsayı, olacaktır **0**). Ancak varsayılan değerini belirtmek istediğimiz **3** içinde varolan satırları şekilde **blogları** tablo makul bir derecelendirme ile başlar.
Şimdi biz düzenleyebilmek için kod tabanlı bir geçiş out bu değişikliği yazmak için geçiş Ekle komutunu kullanın. **Ekle geçiş** komutu bize bu geçişleri bir ad verin, yalnızca bizim adlandıralım verir **AddBlogRating**.

-   Çalıştırma **Ekle geçiş AddBlogRating** Paket Yöneticisi konsolunda komutu.
-   İçinde **geçişler** klasör artık sahibiz yeni **AddBlogRating** geçiş. Geçiş dosya zaman damgası ile sıralama ile yardımcı olmak için önceden sabit. Bir varsayılan değer 3'ün Blog.Rating (aşağıdaki kod satırını 10) belirtmek için oluşturulan kodu düzenleyelim

*Geçiş de bazı meta veriler yakalanır bir arka plan kod dosyası vardır. Bu meta veriler, bu kod tabanlı geçiş öncesinde gerçekleştirdiğimiz otomatik geçişlerin çoğaltmak Code First Migrations izin verir. Bu, bizim geçişlerini çalıştırmak başka bir geliştirici istiyorsa, ya da bunu uygulamamız dağıtma zamanı geldiğinde önemlidir.*

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

Bizim düzenlenen geçiş iyi görünüyor, böylece kullanalım **veritabanını Güncelleştir** güncel veritabanı getirilecek.

-   Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu.

## <a name="back-to-automatic-migrations"></a>Otomatik geçişleri

Otomatik geçişler için yaptığımız basit değişiklikleri geri dönmek ücretsiz sunmaktayız. Code First geçişleri her kod tabanlı bir geçiş için arka plan kod dosyasında depolamak meta verileri doğru sırada otomatik ve kod tabanlı geçişler gerçekleştirerek ilgileniriz.

-   Modelimiz için Post.Abstract özelliği ekleyelim

``` csharp
    public string Abstract { get; set; }
```

Kullanabiliriz artık **veritabanını Güncelleştir** bu değişikliği otomatik geçişini kullanarak veritabanına göndermek için Code First Migrations'ı almak için.

-   Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu.

## <a name="summary"></a>Özet

Bu izlenecek yolda göndermeyi otomatik geçişleri kullanmayı öğrendiniz modeli veritabanını değiştirir. Ayrıca, iskele ve daha fazla denetime ihtiyacınız olduğunda otomatik geçişleri arasında kod tabanlı geçişler çalıştırma de gördünüz.
