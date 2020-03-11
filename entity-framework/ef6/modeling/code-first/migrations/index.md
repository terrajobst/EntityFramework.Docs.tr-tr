---
title: Code First Migrations-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: e5a91af73bab9d45b0f1f4242ce503c6b6f407f6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418964"
---
# <a name="code-first-migrations"></a>Code First Migrations
Code First Migrations, Code First iş akışını kullanıyorsanız uygulamanızın veritabanı şemasını geliştirmek için önerilen yoldur. Geçişler, izin veren bir araç kümesi sağlar:

1. EF modelinizle birlikte çalışarak ilk veritabanı oluşturma
2. EF modelinizde yaptığınız değişiklikleri izlemek için geçişler oluşturma
2. Veritabanınızı bu değişikliklerle güncel tutun

Aşağıdaki izlenecek yol, Entity Framework Code First Migrations bir genel bakış sağlar. Tüm yönergeyi tamamlayabilir ya da ilgilendiğiniz konuya atlayabilirsiniz. Aşağıdaki konular ele alınmıştır:

## <a name="building-an-initial-model--database"></a>Ilk model & veritabanı oluşturma

Geçişleri kullanmaya başlamadan önce bir proje ve ile çalışmak için bir Code First modeli gerekir. Bu izlenecek yol için kurallı **Blog** ve **gönderi** modelini kullanacağız.

-   Yeni bir **Migrationsdemo** konsol uygulaması oluşturma
-   Projeye **EntityFramework** NuGet paketinin en son sürümünü ekleyin
    -   **Araçlar –&gt; kitaplığı Paket Yöneticisi –&gt; Paket Yöneticisi konsolu**
    -   **Install-Package EntityFramework** komutunu çalıştırın
-   Aşağıda gösterilen kodla bir **model.cs** dosyası ekleyin. Bu kod, etki alanı modelimizi ve EF Code First bağlamımız bir **BlogContext** sınıfını oluşturan tek bir **Blog** sınıfını tanımlar

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

-   Artık bir modelimiz olduğuna göre, veri erişimi gerçekleştirmek için bunu kullanmanın zamanı. **Program.cs** dosyasını aşağıda gösterilen kodla güncelleştirin.

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

-   Uygulamanızı çalıştırın ve sizin için bir **Migrationscodedemo. BlogContext** veritabanının oluşturulduğunu görürsünüz.

    ![Veritabanı Yereldb](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Geçişleri etkinleştirme

Modelinizde daha fazla değişiklik yapmak zaman alabilir.

-   Blog sınıfına bir URL özelliği tanıtalım.

``` csharp
    public string Url { get; set; }
```

Uygulamayı yeniden çalıştırırsanız, *veritabanı oluşturulduktan sonra ' BlogContext ' bağlamının yedeklendiğini belirten bir InvalidOperationException alacaksınız. Veritabanını güncelleştirmek için Code First Migrations kullanmayı düşünün (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

Özel durum önerdiğinde Code First Migrations kullanmaya başlama zamanı. İlk adım bağlamımız için geçişleri etkinleştirmektir.

-   Paket Yöneticisi konsolunda **Etkinleştir-geçişleri** komutunu çalıştırın

    Bu komut, projenize bir **geçişler** klasörü ekledi. Bu yeni klasör iki dosya içerir:

-   **Yapılandırma sınıfı.** Bu sınıf, bağlam için geçişlerin nasıl davranacağını yapılandırmanıza olanak tanır. Bu kılavuzda, yalnızca varsayılan yapılandırmayı kullanacağız.
    *Projenizde yalnızca tek bir Code First bağlamı olduğundan, Enable-geçişler bu yapılandırmanın uygulandığı bağlam türüne otomatik olarak doldurulur.*
-   **Bir ınitialcreate geçişi**. Bu geçiş, geçişleri etkinleştirmeden önce bizim için bir veritabanı oluşturmak Code First zaten vardı. Bu yapı iskelesi geçişi içindeki kod, veritabanında zaten oluşturulmuş olan nesneleri temsil eder. Küçük harfli bir **blogID** ve **ad** sütunları olan **Blog** tablosu. Dosya adı, sıralamaya yardımcı olacak bir zaman damgası içerir.
    *Veritabanı zaten oluşturulmadıysa, bu ınitialcreate geçişi projeye eklenmemiş olur. Bunun yerine, ekleme geçişi ilk kez bu tabloları oluşturmak için kod yeni bir geçişe iskele olacaktır.*

### <a name="multiple-models-targeting-the-same-database"></a>Aynı veritabanını hedefleyen birden çok model

EF6 ' den önceki sürümleri kullanırken bir veritabanının şemasını oluşturmak/yönetmek için yalnızca bir Code First modeli kullanılabilir. Bu, hangi girişlerin hangi modele ait olduğunu belirlemenin hiçbir yolu olmadan veritabanı başına tek bir **\_\_MigrationsHistory** tablosunun sonucudur.

EF6 ile başlayarak, **yapılandırma** sınıfı bir **contextKey** özelliği içerir. Bu, her bir Code First modeli için benzersiz bir tanımlayıcı işlevi görür. **\_\_MigrationsHistory** tablosundaki karşılık gelen bir sütun, birden çok modeldeki girişlerin tabloyu paylaşmasına izin verir. Varsayılan olarak, bu özellik, bağlamınızın tam adına ayarlanır.

## <a name="generating--running-migrations"></a>Çalıştırılan & geçişleri oluşturma

Code First Migrations, öğrenecek iki birincil komuta sahiptir.

-   **Geçiş geçişi** , son geçişin oluşturulmasından bu yana modelinizde yaptığınız değişikliklere dayalı olarak bir sonraki geçişi kankatalacak
-   **Güncelleştir-veritabanı** bekleyen geçişleri veritabanına uygular

Eklediğimiz yeni URL özelliğinden yararlanmak için bir geçişi bir geçişe katdık. **Add-Migration** komutu bu geçişlere bir ad vermemizi sağlar. yalnızca **Bizaddblogurl**'yi çağıralım.

-   Paket Yöneticisi konsolunda **Add-Migration AddBlogUrl** komutunu çalıştırma
-   **Geçişler** klasöründe artık yeni bir **Addblogurl** geçişi var. Geçiş dosya adı, sıralamaya yardımcı olması için zaman damgasıyla önceden düzeltildi

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

Şimdi bu geçişe düzenleme veya ekleme yapabiliriz, ancak her şey oldukça iyi görünüyor. Bu geçişi veritabanına uygulamak için **Update-Database** ' i kullanalım.

-   Package Manager konsolunda **Update-Database** komutunu çalıştırın
-   Code First Migrations, **geçişleri** klasörünüzdeki geçişleri veritabanına uygulanmış olanlarla karşılaştıracaktır. **Addblogurl** geçişinin uygulanması gerektiğini görebilir ve bu işlem çalışır.

**Migrationsdemo. BlogContext** veritabanı artık **Bloglar** tablosuna **URL** sütununu içerecek şekilde güncelleştirildi.

## <a name="customizing-migrations"></a>Geçişleri özelleştirme

Şimdiye kadar herhangi bir değişiklik yapmadan bir geçiş oluşturmuş ve çalıştırdık. Şimdi varsayılan olarak oluşturulan kodu düzenleyerek göz atalım.

-   Modelimizde daha fazla değişiklik yapma zamanı, **Blog** sınıfına yeni bir **Derecelendirme** özelliği ekleyelim

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

-   Ayrıca **, blog ve** **gönderi** arasındaki ilişkinin diğer sonunu oluşturmak Için **Blog** sınıfına bir **gönderi** koleksiyonu ekleyeceğiz

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Geçiş sırasında en iyi tahmininizi Code First Migrations Scam etmek için **Add-Migration** komutunu kullanacağız. Bu geçiş **Addpostclass**'ı çağıracağız.

-   Paket Yöneticisi konsolundaki **Add-Migration AddPostClass** komutunu çalıştırın.

Code First Migrations, bu değişikliklerin etkili bir şekilde sağlam bir işi olduğundan, değiştirmek isteyebileceğiniz bazı şeyler vardır:

1.  İlk olarak, postalarınıza benzersiz bir dizin ekleyelim **. title** sütununa (aşağıdaki kodda 22 & 29 satıra ekleme).
2.  Ayrıca, null olamayan **blogların. derecelendirme** sütununu da ekliyoruz. Tabloda varolan veriler varsa, yeni sütun için veri türünün CLR varsayılanı atanır (derecelendirme tamdır, yani **0**olur). Ancak varsayılan olarak **3** değerini belirtmek istiyoruz, bu sayede **blogların** tablosundaki mevcut satırların bir sıra derecelendirmesi ile başlaması gerekir.
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

Düzenlenmiş geçişimiz çalışmaya devam eder, bu nedenle veritabanını güncel hale getirmek için **Update-Database** ' i kullanalım. Bu süre, Code First Migrations çalışan SQL 'i görebilmeniz için **– verbose** bayrağını belirtlim.

-   Package Manager konsolundaki **Update-Database – verbose** komutunu çalıştırın.

## <a name="data-motion--custom-sql"></a>Veri hareketi/özel SQL

Şimdiye kadar herhangi bir veriyi değiştirmeyin veya taşımamış geçiş işlemlerine baktık, şimdi bazı verilerin etrafında taşınması gereken bir şeye bakalım. Henüz veri hareketi için yerel destek yoktur, ancak betiğimizde herhangi bir noktada bazı rastgele SQL komutları çalıştırabiliriz.

-   Modelinize bir **Post. Abstract** özelliği ekleyelim. Daha sonra, **içerik** sütununun başından itibaren bazı metinleri kullanarak mevcut gönderimler için **Özet** 'i önceden dolduracağız.

``` csharp
    public string Abstract { get; set; }
```

Geçiş sırasında en iyi tahmininizi Code First Migrations Scam etmek için **Add-Migration** komutunu kullanacağız.

-   Paket Yöneticisi konsolunda **Add-Migration AddPostAbstract** komutunu çalıştırın.
-   Oluşturulan geçiş, şema değişikliklerinden yararlanır, ancak her gönderi için içeriğin ilk 100 karakterini kullanarak **soyut** sütunu önceden doldurmak de istiyoruz. Bunu, SQL 'e bırakarak ve sütun eklendikten sonra bir **Update** ifadesini çalıştırarak yapabiliriz.
    (Aşağıdaki kodda 12. satırda ekleme)

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

Düzenlenmiş geçişimiz iyi arıyor, bu nedenle veritabanını güncel hale getirmek için **Update-Database** ' i kullanalım. Veritabanına karşı çalışan SQL 'i görebilmemiz için **– verbose** bayrağını belirteceğiz.

-   Package Manager konsolundaki **Update-Database – verbose** komutunu çalıştırın.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>Belirli bir sürüme geçiş (düşürme dahil)

Şimdiye kadar her zaman en son geçişe yükseltildik, ancak belirli bir geçişe yükseltme/düşürme yapmak istediğiniz zamanlar olabilir.

Şimdi, **Addblogurl** geçişimizi çalıştırdıktan sonra veritabanını bulunduğu duruma geçirmek istiyoruz. Bu geçişe düşürme sağlamak için **– Targetmigration** anahtarını kullanabiliriz.

-   Package Manager konsolundaki **Update-Database – TargetMigration: AddBlogUrl** komutunu çalıştırın.

Bu komut, **AddBlogAbstract** ve **addpostclass** geçişlerimiz için aşağı komut dosyasını çalıştırır.

Bir bütün olarak boş bir veritabanına geri dönmek istiyorsanız, **Update-Database – TargetMigration: $InitialDatabase** komutunu kullanabilirsiniz.

## <a name="getting-a-sql-script"></a>SQL betiği alma

Başka bir geliştirici makinesinde bu değişikliklere istiyorsa, değişiklikleri kaynak denetimine denetliyoruz bir kez daha zaman eşitlenebilir. Yeni geçişlerimiz olduktan sonra, değişikliklerin yerel olarak uygulanmasını sağlamak için yalnızca Update-database komutunu çalıştırabilir. Ancak, bu değişiklikleri bir test sunucusuna göndermek istiyoruz ve son olarak üretimde, büyük olasılıkla DBA için bir SQL betiği sunmamız istiyoruz.

-   **Update-Database** komutunu çalıştırın, ancak bu kez, değişikliklerin uygulanması yerine bir betiğe yazılması Için **– Script** bayrağını belirtin. Ayrıca, komut dosyasını oluşturmak için bir kaynak ve hedef geçişi de belirteceğiz. Bir betiğin boş bir veritabanından ( **$InitialDatabase**) en son sürüme (geçiş **Addpostabstract**) gitmesini istiyoruz.
    *Hedef geçiş belirtmezseniz, geçişler hedef olarak en son geçişi kullanır. Kaynak geçişleri belirtmezseniz, geçişler veritabanının geçerli durumunu kullanacaktır.*
-   **Güncelleştirme-veritabanı-betiği-SourceMigration: $InitialDatabase-TargetMigration: AddPostAbstract** komutunu, Paket Yöneticisi konsolu 'nda çalıştırın

Code First Migrations, geçiş işlem hattını çalıştıracak ancak değişiklikleri gerçekten uygulamak yerine, sizin için bir. SQL dosyasına yazacak. Betik oluşturulduktan sonra, Visual Studio 'da sizin için açılır, bu sizin için sizin için açılır.

### <a name="generating-idempotent-scripts"></a>Idempotent betikleri üretiliyor

EF6 ile başlayarak, **– Sourcemigration $InitialDatabase** belirtirseniz oluşturulan betik ' ıdempotent ' olur. Idempotent betikleri, şu anda herhangi bir sürümdeki bir veritabanını en son sürüme (ya da **– Targetmigration**kullanıyorsanız, belirtilen sürüme) yükseltebilirler. Oluşturulan betik, **\_\_MigrationsHistory** tablosunu denetleme mantığını içerir ve yalnızca daha önce uygulanmamış değişiklikleri uygular.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Uygulama başlangıcında otomatik olarak yükseltme (MigrateDatabaseToLatestVersion Başlatıcısı)

Uygulamanızı dağıtıyorsanız, uygulamanın başlatıldığında veritabanını otomatik olarak yükseltmesini (bekleyen geçişler uygulayarak) isteyebilirsiniz. Bunu, **Migratedatabasetolatestversion** veritabanı Başlatıcısı ' nı kaydederek yapabilirsiniz. Veritabanı başlatıcısı yalnızca veritabanının doğru şekilde ayarlandığından emin olmak için kullanılan bir mantığı içerir. Bu mantık, içerik uygulama işlemi (**AppDomain**) içinde ilk kez kullanıldığında çalıştırılır.

Bağlamı kullanmadan önce BlogContext için **Migratedatabasetolatestversion** başlatıcısı 'nı ayarlamak üzere, aşağıda gösterildiği gibi **program.cs** dosyasını güncelleştirebiliriz (satır 14). **System. Data. Entity** ad alanı (5. satır) için bir using ifadesini de eklemeniz gerektiğini unutmayın.

*Bu başlatıcı 'nin bir örneğini oluşturduğumuzda, bağlam türünü (**BlogContext**) ve geçişler yapılandırmasını (**yapılandırma**) belirtmemiz gerekir-geçişleri etkinleştirmemiz durumunda **geçişler klasörünüze eklenen** sınıftır.*

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

Artık uygulamamız her çalıştığında, öncelikle hedeflediği veritabanının güncel olup olmadığını kontrol eder ve yoksa bekleyen geçişleri uygular.
