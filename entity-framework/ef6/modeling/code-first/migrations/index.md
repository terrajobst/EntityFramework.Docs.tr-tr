---
title: Code First geçişleri - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
caps.latest.revision: 3
ms.openlocfilehash: 5c7431985e2e404060197615bf281fcf3b318403
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914290"
---
# <a name="code-first-migrations"></a>Code First geçişleri
Code First geçişleri kod ilk iş akışınızı kullanıyorsa uygulamanın veritabanı şemasını gelişmek için önerilen yoldur. Geçişleri sağlayan araçlar sağlar:

1. EF modelinizi çalışır bir başlangıç veritabanı oluşturma
2. EF modelinizi yaptığınız değişiklikleri izlemek için geçişler oluşturuluyor
2. Veritabanınızı bu değişiklikler ile güncel tutun

Aşağıdaki örneklerde Entity Framework Code First Migrations genel bakış sağlar. Tüm izlenecek yolu tamamlamak veya ilgilendiğiniz konusuna atlayabilirsiniz. Aşağıdaki konular ele alınmaktadır:

## <a name="building-an-initial-model--database"></a>Bir ilk Model & veritabanı oluşturma

Biz geçişleri kullanmaya başlamadan önce bir proje ve çalışmak için Code First modeli gerekir. Bu kılavuz için kurallı kullanacağız **Blog** ve **Post** modeli.

-   Yeni bir **MigrationsDemo** konsol uygulaması
-   En son sürümünü eklemek **EntityFramework** projeye NuGet paketi
    -   **Araçları –&gt; kitaplık Paket Yöneticisi –&gt; Paket Yöneticisi Konsolu**
    -   Çalıştırma **Install-Package EntityFramework** komutu
-   Ekleme bir **Model.cs** aşağıda gösterilen kod dosyası. Bu kod, tek bir tanımlar **Blog** etki alanı modelimizi sağlar sınıfını ve **BlogContext** bizim EF Code First bağlam sınıfı

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

-   Biz bir modeliniz olduğuna göre veri erişimi gerçekleştirdiği kullanma zamanı geldi. Güncelleştirme **Program.cs** aşağıda gösterilen kod dosyası.

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

-   Uygulamanızı çalıştırın ve göreceksiniz bir **MigrationsCodeDemo.BlogContext** veritabanı sizin için oluşturulur.

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Geçiş etkinleştiriliyor

Bu, bazı modelimiz için daha fazla değişiklik zamanı geldi.

-   Şimdi Blog sınıfına URL'si özelliği sunar.

``` csharp
    public string Url { get; set; }
```

Uygulamayı çalıştırmak için olsaydı yeniden belirten bir InvalidOperationException elde edebileceğiniz *veritabanı oluşturulduktan sonra 'BlogContext' bağlam yedekleme modeli değişti. Veritabanını güncellemek için Code First Migrations'ı kullanmayı deneyin (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](http://go.microsoft.com/fwlink/?LinkId=238269) *).*

Özel durum da anlaşılacağı gibi Code First Migrations'ı kullanmaya başlamak için zaman var. İlk adım, geçişler için sunduğumuz bağlam etkinleştirmektir.

-   Çalıştırma **etkinleştir geçişleri** komutunu Paket Yöneticisi Konsolu

    Bu komut eklemiştir bir **geçişler** Projemizin klasörüne. Bu yeni klasörü, iki dosya içerir:

-   **Yapılandırma sınıfı.** Bu sınıf geçişleri içeriğiniz için nasıl davranacağını yapılandırmanıza olanak sağlar. Bu kılavuz için yalnızca varsayılan yapılandırmayı kullanacağız.
    *Projenizde yalnızca tek bir Code First bağlamı olmadığından, geçişleri etkinleştir otomatik olarak doldurulur Bu yapılandırmanın geçerli bağlam türü.*
-   **InitialCreate geçiş**. Bu geçiş, çünkü zaten Code First geçişleri etkinleştirdik önce bize bir veritabanı oluşturma vardı oluşturuldu. İskele kurulmuş bu geçiş kodu zaten veritabanında oluşturulan nesneleri gösterir. Bu örnekte, **Blog** ile tablo bir **BlogId** ve **adı** sütunları. Dosya adı ile sıralama yardımcı olmak için bir zaman damgası içerir.
    *Veritabanı değil zaten oluşturulmuş varsa bu InitialCreate geçiş projeye eklenmemiş. Bunun yerine, bu tablolar oluşturmak için kod ekleme geçiş diyoruz ilk kez yeni bir geçiş için iskele kurulmuş.*

### <a name="multiple-models-targeting-the-same-database"></a>Aynı veritabanında hedefleyen birden çok modeli

EF6 önceki sürümler kullanırken, yalnızca bir Code First modeli, bir veritabanı şeması oluşturma/yönetmek için kullanılabilir. Bu tek bir sonucudur  **\_ \_MigrationsHistory** hangi girişlerin hangi modele ait belirlemenin bir yolu olan veritabanı başına tablo.

EF6'ile başlayan **yapılandırma** sınıfı içeren bir **contextInfo yüklenemedi** özelliği. Bu, her bir Code First modeli için benzersiz bir tanımlayıcı olarak görev yapar. Karşılık gelen bir sütunda  **\_ \_MigrationsHistory** tablo girişleri tablo paylaşmak için birden çok modeli sağlar. Varsayılan olarak, bu özellik, bağlamı tam adına ayarlanır.

## <a name="generating--running-migrations"></a>Oluşturma ve geçişler çalıştırma

Code First geçişleri sahibi olacak iki birincil komutu vardır.

-   **Geçiş** son geçiş oluşturulmasından bu yana modelinize yaptığınız değişikliklere dayalı sonraki geçiş iskelesini
-   **Veritabanını Güncelleştir** geçişler bekleyen herhangi bir veritabanına uygulanır

Yeni Url özelliği ekledik halletmeniz için bir geçiş iskele ihtiyacımız var. **Ekle geçiş** komutu bize bu geçişleri bir ad verin, yalnızca bizim adlandıralım verir **AddBlogUrl**.

-   Çalıştırma **Ekle geçiş AddBlogUrl** komutunu Paket Yöneticisi Konsolu
-   İçinde **geçişler** klasör artık sahibiz yeni **AddBlogUrl** geçiş. Geçiş dosya zaman damgası ile sıralama ile yardımcı olmak için önceden sabittir

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

Biz artık düzenleyebilir veya bu geçiş ancak her şeyi oldukça iyi görünüyor ekleyebilirsiniz. Kullanalım **veritabanını Güncelleştir** bu geçiş veritabanına uygulamak için.

-   Çalıştırma **veritabanını Güncelleştir** komutunu Paket Yöneticisi Konsolu
-   Code First geçişleri geçişlerde karşılaştırma bizim **geçişler** klasör fiyatlarla veritabanına uygulanır. Panoyu göremeyecek **AddBlogUrl** uygulanan ve çalıştırmak geçirilmesi gerekiyor.

**MigrationsDemo.BlogContext** veritabanı içerecek şekilde güncelleştirilmiş artık **Url** sütununda **blogları** tablo.

## <a name="customizing-migrations"></a>Geçişlerini özelleştirme

Şu ana kadar biz oluşturulur ve herhangi bir değişiklik yapmadan bir geçiş çalıştırın. Artık varsayılan olarak oluşturulan kod düzenleme göz atalım.

-   Modelimiz için daha fazla bazı değişiklikler yapmak için yeni bir ekleyelim zamanı **derecelendirme** özelliğini **Blog** sınıfı

``` csharp
    public int Rating { get; set; }
```

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

Kullanacağız **Ekle geçiş** bizim için geçiş sırasında en iyi tahmin iskelesini Code First Migrations'ı izin vermek için komutu. Bu geçiş çağrısı yapacağız **AddPostClass**.

-   Çalıştırma **Ekle geçiş AddPostClass** Paket Yöneticisi konsolunda komutu.

Code First geçişleri yaptığınız değişikliklerin iskele kurma özelliği, oldukça iyi bir iş, ancak biz değiştirmek isteyebileceğiniz bazı şeyler vardır:

1.  İlk yedeklemeniz, benzersiz bir dizin ekleyelim **Posts.Title** sütun (22 & aşağıdaki kodda 29 satırında ekleme).
2.  Ayrıca atanamayan bir ekliyoruz **Blogs.Rating** sütun. Tablodaki tüm mevcut veriler varsa, yeni bir sütun için veri türünün CLR varsayılan atanır (derecelendirmesi, tamsayı, olacaktır **0**). Ancak varsayılan değerini belirtmek istediğimiz **3** içinde varolan satırları şekilde **blogları** tablo makul bir derecelendirme ile başlar.
    (Aşağıdaki kod satırını 24 belirtilen varsayılan değerin görebilirsiniz)

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

Bizim düzenlenen geçiş gidin, bu nedenle kullanalım desteklemeye hazır olup **veritabanını Güncelleştir** güncel veritabanı getirilecek. Bu süre belirtelim **– ayrıntılı** Code First Migrations'ı çalıştıran SQL görebilmeniz için bayrak.

-   Çalıştırma **veritabanını güncelleştir – ayrıntılı** Paket Yöneticisi konsolunda komutu.

## <a name="data-motion--custom-sql"></a>Veri hareketi / özel SQL

Şu ana kadar bazı verileri taşımak için gereken değiştirin veya herhangi bir veri artık geçelim işlemleri sırasında bir sorun ara geçiş inceledik. Yerel desteği yoktur veri hareketi için henüz ancak biz bazı rastgele SQL komutlarını betiğimizi içinde herhangi bir noktada çalıştırabilirsiniz.

-   Ekleyelim bir **Post.Abstract** modelimizi özelliği. Önceden doldurmak için daha sonra ekleyeceğiz **soyut** metin başlangıcından kullanarak mevcut gönderileri **içerik** sütun.

``` csharp
    public string Abstract { get; set; }
```

Kullanacağız **Ekle geçiş** bizim için geçiş sırasında en iyi tahmin iskelesini Code First Migrations'ı izin vermek için komutu.

-   Çalıştırma **Ekle geçiş AddPostAbstract** Paket Yöneticisi konsolunda komutu.
-   Oluşturulan geçiş şema değişikliklerini üstlenir ancak biz de önceden doldurmak istiyorum **soyut** ilk 100 karakter içerik için her bir gönderi kullanarak sütun. Bu SQL ve çalışan bırakarak tarafından yapabiliriz bir **güncelleştirme** sütunu eklendikten sonra deyimi.
    (Aşağıdaki kod satırında 12 ekleme)

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

Bizim düzenlenen geçiş iyi görünüyor, böylece kullanalım **veritabanını Güncelleştir** güncel veritabanı getirilecek. Biz belirtirsiniz **– ayrıntılı** biz veritabanına karşı çalıştırılan SQL görebilmeniz için bayrak.

-   Çalıştırma **veritabanını güncelleştir – ayrıntılı** Paket Yöneticisi konsolunda komutu.

## <a name="migrate-to-a-specific-version-including-downgrade"></a>(Sürüm Düşürme dahil), belirli bir sürüme geçirme

Şu ana kadar en son geçiş için her zaman yükselttik, ancak Yükseltme/indirgeme belirli bir geçiş istediğinizde zamanlar olabilir.

Veritabanımızdaki olduğu içinde çalıştırdıktan sonra duruma geçirmek istediğimiz varsayalım bizim **AddBlogUrl** geçiş. Kullanabiliriz **– TargetMigration** düşürmek için bu geçiş anahtarı.

-   Çalıştırma **veritabanını güncelleştir – TargetMigration: AddBlogUrl** Paket Yöneticisi konsolunda komutu.

Bu komut için aşağı betiği çalıştırır bizim **AddBlogAbstract** ve **AddPostClass** geçişler.

Tüm kullanıma sunmak istiyorsanız boş bir veritabanı için yedekleme sonra kullanabileceğiniz **veritabanını güncelleştir – TargetMigration: $InitialDatabase** komutu.

## <a name="getting-a-sql-script"></a>SQL komut dosyası alma

Başka bir geliştiricinin kendi makinesinde bu değişiklikleri istiyorsa, biz bizim değişiklikleri kaynak denetimine iade sonra bunlar yalnızca eşitleyebilirsiniz. Bunlar bizim yeni geçişleri oluşturduktan sonra yalnızca yerel olarak uygulanan değişiklikler için veritabanını Güncelleştir komutunu çalıştırabilirsiniz. Bir test sunucusu ve üretim için sonuçta bu değişiklikleri dışına istiyoruz, ancak büyük olasılıkla bir SQL betiği biz kapatmak için sunduğumuz DBA dağıtabilir istiyoruz.

-   Çalıştırma **veritabanını Güncelleştir** komut ancak bu kez **– betik** değişiklikleri bir komut dosyasına yazılmış yerine böylece uygulanan bayrak. Biz de için komut dosyası oluşturmak için bir kaynak ve hedef geçiş belirteceksiniz. Boş bir veritabanından gitmek için bir betik istiyoruz (**$InitialDatabase**) en son sürüme (geçiş **AddPostAbstract**).
    *Bir hedef geçiş belirtmezseniz, geçişler son geçiş hedef olarak kullanın. Kaynak geçişleri belirtmezseniz, veritabanının geçerli durumu geçişleri kullanır.*
-   Çalıştırma **veritabanını güncelleştir-- SourceMigration betik: $InitialDatabase - TargetMigration: AddPostAbstract** komutunu Paket Yöneticisi Konsolu

Code First geçişleri geçiş ardışık düzeni çalışacak ancak değişiklikleri uygulamak yerine, bunları .sql dosya sizin için yazamadı. Komut dosyası oluşturulduktan sonra Visual Studio, görüntülemek veya kaydetmek hazır açılır.

### <a name="generating-idempotent-scripts"></a>Idempotent betikleri oluşturma

Belirtirseniz EF6'ile başlayan **– SourceMigration $InitialDatabase** oluşturulan betiği 'ıdempotent' olmayacaktır. Idempotent betikleri herhangi bir sürümü şu anda bir veritabanını en son sürüme yükseltebilirsiniz (veya belirtilen sürümü kullanırsanız **– TargetMigration**). Oluşturulan kodun denetlemek için mantığı içerir  **\_ \_MigrationsHistory** tablo ve yalnızca daha önce sorgularınızda uygulanmamış değişiklikleri uygulayın.

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a>Uygulama başlangıcında (MigrateDatabaseToLatestVersion Başlatıcı) otomatik olarak yükseltme

Uygulamanızı dağıtıyorsanız (geçişleri beklemedeki uygulayarak) veritabanı otomatik olarak yükseltmek istediğiniz, uygulamayı başlatır. Kaydederek bunu yapabilirsiniz **MigrateDatabaseToLatestVersion** veritabanı Başlatıcı. Bir veritabanı başlatıcısı, yalnızca veritabanı doğru şekilde kurulduğundan emin olmak için kullanılan bazı mantığını içerir. Bu mantık bağlamı içinde uygulama işlemi kullanılan ilk kez çalıştırılan (**AppDomain**).

Biz güncelleştirebilirsiniz **Program.cs** ayarlamak için aşağıda gösterildiği gibi dosya **MigrateDatabaseToLatestVersion** (satır 14) bağlam kullandığımız önce BlogContext için Başlatıcı. Aynı zamanda bir kullanmadan eklemeniz gerektiğini unutmayın bildirimi **System.Data.Entity** ad alanı (Satır 5).

*Biz ihtiyacımız bağlam türünü belirtmek için bu Başlatıcı örneğini oluşturduğunuzda (**BlogContext**) ve geçişleri yapılandırma (**yapılandırma**)-geçişleri yapılandırma aldı sınıfıdır eklenen bizim **geçişler** klasörüne geçişleri etkinleştirdik.*

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
                Database.SetInitializer(new MigrateDatabaseToLatestVersion\<BlogContext, Configuration>());

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

Artık her veritabanı, hedefliyor, ilk denetleyecek bizim çalıştıracağını güncel olduğundan ve yüklü değilse bekleyen tüm geçişler.
