---
title: Kod İlk Geçişler - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: e5a91af73bab9d45b0f1f4242ce503c6b6f407f6
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78418964"
---
# <a name="code-first-migrations"></a>Kod İlk Geçişler
Code First Migrations, Önce Kod iş akışını kullanıyorsanız, uygulamanızın veritabanı şemasını geliştirmenin önerilen yoludur. Geçişler, şuna izin veren bir araç kümesi sağlar:

1. EF modelinizde çalışan bir başlangıç veritabanı oluşturma
2. EF modelinizde yaptığınız değişiklikleri izlemek için geçiş oluşturma
2. Veritabanınızı bu değişikliklerden haberdar edin

Aşağıdaki izlenecek yol, Varlık Çerçevesi'ndeki İlk Geçişler Kodu'na genel bir bakış sağlayacaktır. Tüm gözden geçiriyi tamamlayabilir veya ilgilendiğiniz konuya atlayabilirsiniz. Aşağıdaki konular ele alınmıştır:

## <a name="building-an-initial-model--database"></a>İlk Model & Veritabanı Oluşturma

Geçişleri kullanmaya başlamadan önce bir projeye ve çalışmak için Bir İlk Kod modeline ihtiyacımız vardır. Bu izlenebilir için biz kanonik **Blog** ve **Post** modeli kullanmak için gidiyoruz.

-   Yeni **migrationsDemo** Console uygulaması oluşturma
-   **EntityFramework** NuGet paketinin en son sürümünü projeye ekleyin
    -   **Araçlar&gt; – Kütüphane&gt; Paket Yöneticisi – Package Manager Konsolu**
    -   **Install-Package EntityFramework** komutunu çalıştırın
-   Aşağıda gösterilen kodu içeren bir **Model.cs** dosyası ekleyin. Bu kod, etki alanı modelimizi oluşturan tek bir **Blog** sınıfımızı ve EF Code First bağlamımız olan **bir BlogContext** sınıfımızı tanımlar

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsDemo
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

-   Artık bir modelimiz olduğuna göre veri erişimi gerçekleştirmek için kullanma nın zamanı gelmiştir. Program.cs **dosyayı** aşağıda gösterilen kodla güncelleştirin.

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsDemo
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

-   Uygulamanızı çalıştırın ve sizin için bir **MigrationsCodeDemo.BlogContext** veritabanı oluşturulduğunu göreceksiniz.

    ![Veritabanı LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Geçişleri Etkinleştirme

Modelimizde daha fazla değişiklik yapma zamanı.

-   Blog sınıfına bir Url özelliği tanıyalım.

``` csharp
    public string Url { get; set; }
```

Uygulamayı yeniden çalıştıracak olursanız, veritabanı oluşturulduğundan *beri 'BlogContext' bağlamını destekleyen modelin değiştiğini belirten bir GeçersizOperasyonİst alırsınız. Veritabanını güncelleştirmek için Kod İlk Geçişleri 'ni kullanmayı düşünün (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

Özel olarak da anlaşılacağı gibi, Kod İlk Geçişler kullanmaya başlama zamanı. İlk adım, bağlamımız için geçişleri mümkün kılmaktır.

-   Paket Yöneticisi Konsolunda **Geçişleri Etkinleştir** komutunu çalıştırın

    Bu komut projemize bir **Geçişler** klasörü ekledi. Bu yeni klasör iki dosya içerir:

-   **Yapılandırma sınıfı.** Bu sınıf, Geçişler'in bağlamınız için nasıl bir şekilde nasıl bir şekilde nasıl bir şekilde nasıl bir şekilde uygun olduğunu yapılandırmanızı sağlar. Bu izlenecek yol için varsayılan yapılandırmayı kullanacağız.
    *Projenizde yalnızca tek bir Code First bağlamı olduğundan, Geçişleri Etkinleştir medenleri bu yapılandırmanın geçerli olduğu bağlam türünü otomatik olarak doldurdu.*
-   **Başlangıç Geçişi Oluşturma**. Bu geçiş, geçişleri etkinleştirmeden önce Code First'in bizim için bir veritabanı oluşturmasına sahip olduğumuz için oluşturuldu. Bu iskelegeçişindeki kod, veritabanında zaten oluşturulmuş nesneleri temsil eder. Bizim durumumuzda bir **BlogId** ve **Ad** sütunları ile **Blog** tablodur. Dosya adı, sipariş vermeye yardımcı olacak bir zaman damgası içerir.
    *Veritabanı zaten oluşturulmasaydı bu InitialCreate geçişi projeye eklenmezdi. Bunun yerine, bu tabloları oluşturmak için kod Ekleme-Geçiş dediğimiz ilk kez yeni bir geçiş iskele olacaktır.*

### <a name="multiple-models-targeting-the-same-database"></a>Aynı Veritabanını Hedefleyen Birden Çok Model

EF6'dan önceki sürümleri kullanırken, veritabanının şemasını oluşturmak/yönetmek için yalnızca bir Code First modeli kullanılabilir. Bu, hangi girişlerin ** \_ \_** hangi modele ait olduğunu belirlemenin hiçbir yolu olmayan veritabanı başına tek bir MigrationsHistory tablosunun sonucudur.

EF6 ile **başlayarak, Yapılandırma** sınıfı bir **ContextKey** özelliği içerir. Bu, her Code First modeli için benzersiz bir tanımlayıcı görevi görür. ** \_ \_MigrationsHistory** tablosundaki karşılık gelen sütun, birden çok modelden girişlerin tabloyu paylaşmasına olanak tanır. Varsayılan olarak, bu özellik bağlamınızın tam nitelikli adına ayarlanır.

## <a name="generating--running-migrations"></a>& Çalıştırma Geçişleri Oluşturma

Kod İlk Geçişler aşina olacak iki birincil komutları vardır.

-   **Add-Migration,** son geçiş oluşturulduğundan beri modelinizde yaptığınız değişikliklere göre bir sonraki geçişi şekillendirecek
-   **Update-Database** veritabanına bekleyen geçişleri uygular

Eklediğimiz yeni Url özelliğinin icabına bakmak için bir göç inşa etmeliyiz. **Add-Migration** komutu bize bu göçler bir isim vermek için izin verir, sadece bizim **AddBlogUrl**çağıralım.

-   Paket Yöneticisi Konsolunda **Ekle-Geçiş AddBlogUrl** komutunu çalıştırın
-   **Migrations** klasöründe artık yeni bir **AddBlogUrl** geçişi var. Geçiş dosya adı, sipariş eve yardımcı olmak için bir zaman damgası ile önceden düzeltilir

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogUrl : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Blogs", "Url", c => c.String());
            }

            public override void Down()
            {
                DropColumn("dbo.Blogs", "Url");
            }
        }
    }
```

Şimdi bu göç ekini yapabilir veya ekleyebiliriz ama her şey oldukça iyi görünüyor. Bu geçişi veritabanına uygulamak için **Update-Database'i** kullanalım.

-   Paket Yöneticisi Konsolunda **Update-Database** komutunu çalıştırın
-   Kod İlk Geçişler, **Geçişler** klasörümüzdeki geçişleri veritabanına uygulananlarla karşılaştırır. **Bu AddBlogUrl** geçiş uygulanması gerektiğini göreceksiniz, ve çalıştırın.

**MigrationsDemo.BlogContext** veritabanı artık **Bloglar** tablosuna **Url** sütununu içerecek şekilde güncelleştirildi.

## <a name="customizing-migrations"></a>Geçişleri Özelleştirme

Şimdiye kadar herhangi bir değişiklik yapmadan bir göç oluşturduk ve çalıştırdık. Şimdi varsayılan olarak oluşturulan kodu düzenlemeye bakalım.

-   Bizim model bazı daha fazla değişiklik yapmak için zamanı, **Blog** sınıfına yeni bir **Derecelendirme** özelliği ekleyelim

``` csharp
    public int Rating { get; set; }
```

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

-   Ayrıca **Blog** ve **Post** arasındaki ilişkinin diğer ucunu oluşturmak için **Blog** sınıfına bir **Gönderiler** koleksiyonu ekleyeceğiz

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Kod İlk Geçişler bizim için göç en iyi tahmin iskele izin vermek için **Ekle-Geçiş** komutunu kullanacağız. Bu göçe **AddPostClass**diyeceğiz.

-   Paket Yöneticisi Konsolunda **Ekle-Geçiş EklePostClass** komutunu çalıştırın.

Kod İlk Göçler bu değişiklikleri iskele oldukça iyi bir iş yaptı, ama değiştirmek isteyebilirsiniz bazı şeyler vardır:

1.  İlk olarak, **Posts.Title** sütununa benzersiz bir dizin ekleyelim (Aşağıdaki koda satır 22 & 29'a ekleme).
2.  Ayrıca geçersiz **bloglar.Rating** sütunu ekliyoruz. Tabloda varolan herhangi bir veri varsa, yeni sütun için veri türünün CLR varsayılanı atanır (Derecelendirme, yani **0**olur). Ancak, **Bloglar** tablosundaki varolan satırların iyi bir derecelendirmeyle başlaması için **varsayılan değeri 3** olarak belirtmek istiyoruz.
    (Aşağıdaki kodun 24. satırında belirtilen varsayılan değeri görebilirsiniz)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostClass : DbMigration
        {
            public override void Up()
            {
                CreateTable(
                    "dbo.Posts",
                    c => new
                        {
                            PostId = c.Int(nullable: false, identity: true),
                            Title = c.String(maxLength: 200),
                            Content = c.String(),
                            BlogId = c.Int(nullable: false),
                        })
                    .PrimaryKey(t => t.PostId)
                    .ForeignKey("dbo.Blogs", t => t.BlogId, cascadeDelete: true)
                    .Index(t => t.BlogId)
                    .Index(p => p.Title, unique: true);

                AddColumn("dbo.Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropIndex("dbo.Posts", new[] { "Title" });
                DropIndex("dbo.Posts", new[] { "BlogId" });
                DropForeignKey("dbo.Posts", "BlogId", "dbo.Blogs");
                DropColumn("dbo.Blogs", "Rating");
                DropTable("dbo.Posts");
            }
        }
    }
```

Düzenlenen geçişimiz kullanıma hazır, bu nedenle veritabanını güncel duruma getirmek için **Update-Database'i** kullanalım. Bu kez ,İlk Geçişler Kodu'nun çalıştırdığı SQL'i görebilmeniz için **Verbose** bayrağını belirtelim.

-   Paket Yöneticisi Konsolunda **Update-Database –Verbose** komutunu çalıştırın.

## <a name="data-motion--custom-sql"></a>Veri Hareketi / Özel SQL

Şimdiye kadar herhangi bir veriyi değiştirmeyen veya taşımayan geçiş işlemlerine baktık, şimdi bazı verileri hareket ettirmesi gereken bir şeye bakalım. Veri hareketi için henüz yerel bir destek yoktur, ancak komut dosyamızın herhangi bir noktasında bazı rasgele SQL komutları çalıştırabiliriz.

-   Modelimize **Bir Post.Abstract** özelliği ekleyelim. Daha sonra, **İçerik** sütununun başından itibaren bazı metinleri kullanarak varolan gönderiler için **Özet'i** önceden dolduracağız.

``` csharp
    public string Abstract { get; set; }
```

Kod İlk Geçişler bizim için göç en iyi tahmin iskele izin vermek için **Ekle-Geçiş** komutunu kullanacağız.

-   Paket Yöneticisi Konsolunda **Ekle-Geçiş AddPostAbstract** komutunu çalıştırın.
-   Oluşturulan geçiş şema değişiklikleri ilgilenir ama biz de her yazı için içerik ilk 100 karakter kullanarak **Özet** sütun önceden doldurmak istiyorum. Bunu, SQL'e bırakarak ve sütun eklendikten sonra bir **UPDATE** deyimi çalıştırarak yapabiliriz.
    (Aşağıdaki kodda 12. satıra ekleme)

``` csharp
    namespace MigrationsDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddPostAbstract : DbMigration
        {
            public override void Up()
            {
                AddColumn("dbo.Posts", "Abstract", c => c.String());

                Sql("UPDATE dbo.Posts SET Abstract = LEFT(Content, 100) WHERE Abstract IS NULL");
            }

            public override void Down()
            {
                DropColumn("dbo.Posts", "Abstract");
            }
        }
    }
```

Düzenlenen geçişimiz iyi görünüyor, bu nedenle veritabanını güncel duruma getirmek için **Update-Database'i** kullanalım. SQL'in veritabanına karşı çalıştırıldığını görebilmemiz için **Verbose** bayrağını belirtiriz.

-   Paket Yöneticisi Konsolunda **Update-Database –Verbose** komutunu çalıştırın.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Belirli Bir Sürüme Geçiş (Düşürme Dahil)

Şimdiye kadar her zaman en son geçiş yükselttik, ancak yükseltme / belirli bir geçiş için downgrade istediğiniz zamanlar olabilir.

Veritabanımızı **AddBlogUrl** geçişimizi çalıştırdıktan sonra içinde olduğu duruma geçirmek istediğimizi varsayalım. Bu geçişe indirgenmek için **–TargetMigration** anahtarını kullanabiliriz.

-   **Update-Database çalıştırın –TargetMigration: AddBlogUrl** komutu Package Manager Console'da.

Bu **komut, AddBlogAbstract** ve **AddPostClass** geçişlerimiz için Aşağı komut dosyasını çalıştıracaktır.

Boş bir veritabanına geri dönmek **istiyorsanız, Update-Database -TargetMigration: $InitialDatabase** komutunu kullanabilirsiniz.

## <a name="getting-a-sql-script"></a>SQL Script alma

Başka bir geliştirici bu değişiklikleri makinelerinde isterse, biz değişikliklerimizi kaynak denetimine kontrol ettiğimizde senkronize edebilirler. Yeni geçişlerimizi yaptıktan sonra değişiklikleri yerel olarak çalıştırmak için Update-Database komutunu çalıştırabilirler. Ancak bu değişiklikleri bir test sunucusuna itmek ve sonunda üretim yapmak istersek, muhtemelen DBA'mıza teslim edebileceğimiz bir SQL komut dosyası isteriz.

-   **Update-Database** komutunu çalıştırın, ancak bu kez değişikliklerin uygulanmak yerine bir komut dosyasına yazılması için **-Script** bayrağını belirtin. Komut dosyasını oluşturmak için bir kaynak ve hedef geçişi de belirteceğiz. Boş bir veritabanından **($InitialDatabase)** en son sürüme (geçiş **AddPostAbstract)** gitmek için bir komut dosyası istiyorum.
    *Bir hedef geçişi belirtmezseniz, Geçişler hedef olarak en son geçişi kullanır. Kaynak geçişleri belirtmezseniz, Geçişler veritabanının geçerli durumunu kullanır.*
-   **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** komutunu Paket Yöneticisi Konsolunda çalıştırın

Kod İlk Geçişler geçiş ardışık çalışır, ancak değişiklikleri gerçekten uygulamak yerine bunları sizin için bir .sql dosyasına yazar. Komut dosyası oluşturulduktan sonra, visual studio'da sizin için açılır ve görüntülemeniz veya kaydetmeniz için hazır hale gelir.

### <a name="generating-idempotent-scripts"></a>İktidarlı Komut Dosyaları Oluşturma

EF6 ile başlayarak, belirtirseniz **–SourceMigration $InitialDatabase** sonra oluşturulan komut dosyası 'idempotent' olacaktır. Idempotent komut dosyaları en son sürüme herhangi bir sürümü şu anda bir veritabanı yükseltebilirsiniz (veya kullanırsanız belirtilen sürümü **–TargetMigration).** Oluşturulan komut ** \_ \_dosyası, MigrationsHistory** tablosunu denetlemek ve yalnızca daha önce uygulanmamış değişiklikleri uygulamak için mantık içerir.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Uygulama Başlatmada Otomatik Yükseltme (MigrateDatabaseToLatestVersion Initializer)

Uygulamanızı dağıtıyorsanız, uygulama başlatıldığında veritabanını otomatik olarak yükseltmesini (bekleyen geçişleri uygulayarak) isteyebilirsiniz. Bunu, **GeçirveriToLatestVersion** veritabanı baş harflerini kaydederek yapabilirsiniz. Veritabanı baş harflerini yalnızca veritabanının doğru şekilde kurulum olduğundan emin olmak için kullanılan bazı mantık içerir. Bu mantık, bağlam ın uygulama işlemi **(AppDomain)** içinde ilk kez kullanılmasıyla çalıştırılır.

Bağlamı (Satır 14) kullanmadan önce BlogContext için **GeçirVeritabanıToLatestVersion** başlatlayıcısını ayarlamak için **Program.cs** dosyasını güncelleştirebiliriz. **System.Data.Entity** ad alanı (Satır 5) için bir dekullanarak ifade eklemeniz gerektiğini unutmayın.

*Bu başlatanın bir örneğini oluşturduğumuzda bağlam türünü **(BlogContext)** ve geçişyapılandırmasını **(Yapılandırma)** belirtmemiz gerekir - Geçişler yapılandırması, Geçişleri etkinleştirdiğimizde **Geçişler** klasörümüze eklenen sınıftır.*

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Data.Entity;
    using MigrationsDemo.Migrations;

    namespace MigrationsDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                Database.SetInitializer(new MigrateDatabaseToLatestVersion<BlogContext, Configuration>());

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

Şimdi uygulamamız çalıştığında, ilk olarak hedeflenen veritabanının güncel olup olmadığını denetleyecek ve değilse bekleyen geçişleri uygulayacaktır.
