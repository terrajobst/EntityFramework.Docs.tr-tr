---
title: Code First geçişleri - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 36591d8f-36e1-4835-8a51-90f34f633d1e
ms.openlocfilehash: 216f850fb906cfc4b68eae76ae11ff167ed835ea
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993390"
---
# <a name="code-first-migrations"></a><span data-ttu-id="6f895-102">Code First geçişleri</span><span class="sxs-lookup"><span data-stu-id="6f895-102">Code First Migrations</span></span>
<span data-ttu-id="6f895-103">Code First geçişleri kod ilk iş akışınızı kullanıyorsa uygulamanın veritabanı şemasını gelişmek için önerilen yoldur.</span><span class="sxs-lookup"><span data-stu-id="6f895-103">Code First Migrations is the recommended way to evolve you application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="6f895-104">Geçişleri sağlayan araçlar sağlar:</span><span class="sxs-lookup"><span data-stu-id="6f895-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="6f895-105">EF modelinizi çalışır bir başlangıç veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="6f895-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="6f895-106">EF modelinizi yaptığınız değişiklikleri izlemek için geçişler oluşturuluyor</span><span class="sxs-lookup"><span data-stu-id="6f895-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="6f895-107">Veritabanınızı bu değişiklikler ile güncel tutun</span><span class="sxs-lookup"><span data-stu-id="6f895-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="6f895-108">Aşağıdaki örneklerde Entity Framework Code First Migrations genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="6f895-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="6f895-109">Tüm izlenecek yolu tamamlamak veya ilgilendiğiniz konusuna atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f895-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="6f895-110">Aşağıdaki konular ele alınmaktadır:</span><span class="sxs-lookup"><span data-stu-id="6f895-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="6f895-111">Bir ilk Model & veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="6f895-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="6f895-112">Biz geçişleri kullanmaya başlamadan önce bir proje ve çalışmak için Code First modeli gerekir.</span><span class="sxs-lookup"><span data-stu-id="6f895-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="6f895-113">Bu kılavuz için kurallı kullanacağız **Blog** ve **Post** modeli.</span><span class="sxs-lookup"><span data-stu-id="6f895-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="6f895-114">Yeni bir **MigrationsDemo** konsol uygulaması</span><span class="sxs-lookup"><span data-stu-id="6f895-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="6f895-115">En son sürümünü eklemek **EntityFramework** projeye NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="6f895-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="6f895-116">**Araçları –&gt; kitaplık Paket Yöneticisi –&gt; Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="6f895-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="6f895-117">Çalıştırma **Install-Package EntityFramework** komutu</span><span class="sxs-lookup"><span data-stu-id="6f895-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="6f895-118">Ekleme bir **Model.cs** aşağıda gösterilen kod dosyası.</span><span class="sxs-lookup"><span data-stu-id="6f895-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="6f895-119">Bu kod, tek bir tanımlar **Blog** etki alanı modelimizi sağlar sınıfını ve **BlogContext** bizim EF Code First bağlam sınıfı</span><span class="sxs-lookup"><span data-stu-id="6f895-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="6f895-120">Biz bir modeliniz olduğuna göre veri erişimi gerçekleştirdiği kullanma zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="6f895-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="6f895-121">Güncelleştirme **Program.cs** aşağıda gösterilen kod dosyası.</span><span class="sxs-lookup"><span data-stu-id="6f895-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="6f895-122">Uygulamanızı çalıştırın ve göreceksiniz bir **MigrationsCodeDemo.BlogContext** veritabanı sizin için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6f895-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="6f895-124">Geçiş etkinleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="6f895-124">Enabling Migrations</span></span>

<span data-ttu-id="6f895-125">Bu, bazı modelimiz için daha fazla değişiklik zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="6f895-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="6f895-126">Şimdi Blog sınıfına URL'si özelliği sunar.</span><span class="sxs-lookup"><span data-stu-id="6f895-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="6f895-127">Uygulamayı çalıştırmak için olsaydı yeniden belirten bir InvalidOperationException elde edebileceğiniz *veritabanı oluşturulduktan sonra 'BlogContext' bağlam yedekleme modeli değişti. Veritabanını güncellemek için Code First Migrations'ı kullanmayı deneyin (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](http://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="6f895-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="6f895-128">Özel durum da anlaşılacağı gibi Code First Migrations'ı kullanmaya başlamak için zaman var.</span><span class="sxs-lookup"><span data-stu-id="6f895-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="6f895-129">İlk adım, geçişler için sunduğumuz bağlam etkinleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="6f895-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="6f895-130">Çalıştırma **etkinleştir geçişleri** komutunu Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="6f895-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="6f895-131">Bu komut eklemiştir bir **geçişler** Projemizin klasörüne.</span><span class="sxs-lookup"><span data-stu-id="6f895-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="6f895-132">Bu yeni klasörü, iki dosya içerir:</span><span class="sxs-lookup"><span data-stu-id="6f895-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="6f895-133">**Yapılandırma sınıfı.**</span><span class="sxs-lookup"><span data-stu-id="6f895-133">**The Configuration class.**</span></span> <span data-ttu-id="6f895-134">Bu sınıf geçişleri içeriğiniz için nasıl davranacağını yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6f895-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="6f895-135">Bu kılavuz için yalnızca varsayılan yapılandırmayı kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="6f895-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="6f895-136">*Projenizde yalnızca tek bir Code First bağlamı olmadığından, geçişleri etkinleştir otomatik olarak doldurulur Bu yapılandırmanın geçerli bağlam türü.*</span><span class="sxs-lookup"><span data-stu-id="6f895-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="6f895-137">**InitialCreate geçiş**.</span><span class="sxs-lookup"><span data-stu-id="6f895-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="6f895-138">Bu geçiş, çünkü zaten Code First geçişleri etkinleştirdik önce bize bir veritabanı oluşturma vardı oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="6f895-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="6f895-139">İskele kurulmuş bu geçiş kodu zaten veritabanında oluşturulan nesneleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="6f895-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="6f895-140">Bu örnekte, **Blog** ile tablo bir **BlogId** ve **adı** sütunları.</span><span class="sxs-lookup"><span data-stu-id="6f895-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="6f895-141">Dosya adı ile sıralama yardımcı olmak için bir zaman damgası içerir.</span><span class="sxs-lookup"><span data-stu-id="6f895-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="6f895-142">*Veritabanı değil zaten oluşturulmuş varsa bu InitialCreate geçiş projeye eklenmemiş. Bunun yerine, bu tablolar oluşturmak için kod ekleme geçiş diyoruz ilk kez yeni bir geçiş için iskele kurulmuş.*</span><span class="sxs-lookup"><span data-stu-id="6f895-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="6f895-143">Aynı veritabanında hedefleyen birden çok modeli</span><span class="sxs-lookup"><span data-stu-id="6f895-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="6f895-144">EF6 önceki sürümler kullanırken, yalnızca bir Code First modeli, bir veritabanı şeması oluşturma/yönetmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6f895-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="6f895-145">Bu tek bir sonucudur  **\_ \_MigrationsHistory** hangi girişlerin hangi modele ait belirlemenin bir yolu olan veritabanı başına tablo.</span><span class="sxs-lookup"><span data-stu-id="6f895-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="6f895-146">EF6'ile başlayan **yapılandırma** sınıfı içeren bir **contextInfo yüklenemedi** özelliği.</span><span class="sxs-lookup"><span data-stu-id="6f895-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="6f895-147">Bu, her bir Code First modeli için benzersiz bir tanımlayıcı olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="6f895-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="6f895-148">Karşılık gelen bir sütunda  **\_ \_MigrationsHistory** tablo girişleri tablo paylaşmak için birden çok modeli sağlar.</span><span class="sxs-lookup"><span data-stu-id="6f895-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="6f895-149">Varsayılan olarak, bu özellik, bağlamı tam adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="6f895-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="6f895-150">Oluşturma ve geçişler çalıştırma</span><span class="sxs-lookup"><span data-stu-id="6f895-150">Generating & Running Migrations</span></span>

<span data-ttu-id="6f895-151">Code First geçişleri sahibi olacak iki birincil komutu vardır.</span><span class="sxs-lookup"><span data-stu-id="6f895-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="6f895-152">**Geçiş** son geçiş oluşturulmasından bu yana modelinize yaptığınız değişikliklere dayalı sonraki geçiş iskelesini</span><span class="sxs-lookup"><span data-stu-id="6f895-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="6f895-153">**Veritabanını Güncelleştir** geçişler bekleyen herhangi bir veritabanına uygulanır</span><span class="sxs-lookup"><span data-stu-id="6f895-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="6f895-154">Yeni Url özelliği ekledik halletmeniz için bir geçiş iskele ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="6f895-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="6f895-155">**Ekle geçiş** komutu bize bu geçişleri bir ad verin, yalnızca bizim adlandıralım verir **AddBlogUrl**.</span><span class="sxs-lookup"><span data-stu-id="6f895-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="6f895-156">Çalıştırma **Ekle geçiş AddBlogUrl** komutunu Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="6f895-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="6f895-157">İçinde **geçişler** klasör artık sahibiz yeni **AddBlogUrl** geçiş.</span><span class="sxs-lookup"><span data-stu-id="6f895-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="6f895-158">Geçiş dosya zaman damgası ile sıralama ile yardımcı olmak için önceden sabittir</span><span class="sxs-lookup"><span data-stu-id="6f895-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="6f895-159">Biz artık düzenleyebilir veya bu geçiş ancak her şeyi oldukça iyi görünüyor ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f895-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="6f895-160">Kullanalım **veritabanını Güncelleştir** bu geçiş veritabanına uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="6f895-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="6f895-161">Çalıştırma **veritabanını Güncelleştir** komutunu Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="6f895-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="6f895-162">Code First geçişleri geçişlerde karşılaştırma bizim **geçişler** klasör fiyatlarla veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6f895-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="6f895-163">Panoyu göremeyecek **AddBlogUrl** uygulanan ve çalıştırmak geçirilmesi gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="6f895-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="6f895-164">**MigrationsDemo.BlogContext** veritabanı içerecek şekilde güncelleştirilmiş artık **Url** sütununda **blogları** tablo.</span><span class="sxs-lookup"><span data-stu-id="6f895-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="6f895-165">Geçişlerini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="6f895-165">Customizing Migrations</span></span>

<span data-ttu-id="6f895-166">Şu ana kadar biz oluşturulur ve herhangi bir değişiklik yapmadan bir geçiş çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6f895-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="6f895-167">Artık varsayılan olarak oluşturulan kod düzenleme göz atalım.</span><span class="sxs-lookup"><span data-stu-id="6f895-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="6f895-168">Modelimiz için daha fazla bazı değişiklikler yapmak için yeni bir ekleyelim zamanı **derecelendirme** özelliğini **Blog** sınıfı</span><span class="sxs-lookup"><span data-stu-id="6f895-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="6f895-169">Ayrıca yeni bir ekleyelim **Post** sınıfı</span><span class="sxs-lookup"><span data-stu-id="6f895-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="6f895-170">Ayrıca ekleyeceğiz bir **gönderileri** koleksiyona **Blog** arasındaki ilişkinin diğer ucundaki formu için sınıf **Blog** ve **sonrası**</span><span class="sxs-lookup"><span data-stu-id="6f895-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="6f895-171">Kullanacağız **Ekle geçiş** bizim için geçiş sırasında en iyi tahmin iskelesini Code First Migrations'ı izin vermek için komutu.</span><span class="sxs-lookup"><span data-stu-id="6f895-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="6f895-172">Bu geçiş çağrısı yapacağız **AddPostClass**.</span><span class="sxs-lookup"><span data-stu-id="6f895-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="6f895-173">Çalıştırma **Ekle geçiş AddPostClass** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="6f895-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="6f895-174">Code First geçişleri yaptığınız değişikliklerin iskele kurma özelliği, oldukça iyi bir iş, ancak biz değiştirmek isteyebileceğiniz bazı şeyler vardır:</span><span class="sxs-lookup"><span data-stu-id="6f895-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="6f895-175">İlk yedeklemeniz, benzersiz bir dizin ekleyelim **Posts.Title** sütun (22 & aşağıdaki kodda 29 satırında ekleme).</span><span class="sxs-lookup"><span data-stu-id="6f895-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="6f895-176">Ayrıca atanamayan bir ekliyoruz **Blogs.Rating** sütun.</span><span class="sxs-lookup"><span data-stu-id="6f895-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="6f895-177">Tablodaki tüm mevcut veriler varsa, yeni bir sütun için veri türünün CLR varsayılan atanır (derecelendirmesi, tamsayı, olacaktır **0**).</span><span class="sxs-lookup"><span data-stu-id="6f895-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="6f895-178">Ancak varsayılan değerini belirtmek istediğimiz **3** içinde varolan satırları şekilde **blogları** tablo makul bir derecelendirme ile başlar.</span><span class="sxs-lookup"><span data-stu-id="6f895-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="6f895-179">(Aşağıdaki kod satırını 24 belirtilen varsayılan değerin görebilirsiniz)</span><span class="sxs-lookup"><span data-stu-id="6f895-179">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="6f895-180">Bizim düzenlenen geçiş gidin, bu nedenle kullanalım desteklemeye hazır olup **veritabanını Güncelleştir** güncel veritabanı getirilecek.</span><span class="sxs-lookup"><span data-stu-id="6f895-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="6f895-181">Bu süre belirtelim **– ayrıntılı** Code First Migrations'ı çalıştıran SQL görebilmeniz için bayrak.</span><span class="sxs-lookup"><span data-stu-id="6f895-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="6f895-182">Çalıştırma **veritabanını güncelleştir – ayrıntılı** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="6f895-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="6f895-183">Veri hareketi / özel SQL</span><span class="sxs-lookup"><span data-stu-id="6f895-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="6f895-184">Şu ana kadar bazı verileri taşımak için gereken değiştirin veya herhangi bir veri artık geçelim işlemleri sırasında bir sorun ara geçiş inceledik.</span><span class="sxs-lookup"><span data-stu-id="6f895-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="6f895-185">Yerel desteği yoktur veri hareketi için henüz ancak biz bazı rastgele SQL komutlarını betiğimizi içinde herhangi bir noktada çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f895-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="6f895-186">Ekleyelim bir **Post.Abstract** modelimizi özelliği.</span><span class="sxs-lookup"><span data-stu-id="6f895-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="6f895-187">Önceden doldurmak için daha sonra ekleyeceğiz **soyut** metin başlangıcından kullanarak mevcut gönderileri **içerik** sütun.</span><span class="sxs-lookup"><span data-stu-id="6f895-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="6f895-188">Kullanacağız **Ekle geçiş** bizim için geçiş sırasında en iyi tahmin iskelesini Code First Migrations'ı izin vermek için komutu.</span><span class="sxs-lookup"><span data-stu-id="6f895-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="6f895-189">Çalıştırma **Ekle geçiş AddPostAbstract** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="6f895-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="6f895-190">Oluşturulan geçiş şema değişikliklerini üstlenir ancak biz de önceden doldurmak istiyorum **soyut** ilk 100 karakter içerik için her bir gönderi kullanarak sütun.</span><span class="sxs-lookup"><span data-stu-id="6f895-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="6f895-191">Bu SQL ve çalışan bırakarak tarafından yapabiliriz bir **güncelleştirme** sütunu eklendikten sonra deyimi.</span><span class="sxs-lookup"><span data-stu-id="6f895-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="6f895-192">(Aşağıdaki kod satırında 12 ekleme)</span><span class="sxs-lookup"><span data-stu-id="6f895-192">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="6f895-193">Bizim düzenlenen geçiş iyi görünüyor, böylece kullanalım **veritabanını Güncelleştir** güncel veritabanı getirilecek.</span><span class="sxs-lookup"><span data-stu-id="6f895-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="6f895-194">Biz belirtirsiniz **– ayrıntılı** biz veritabanına karşı çalıştırılan SQL görebilmeniz için bayrak.</span><span class="sxs-lookup"><span data-stu-id="6f895-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="6f895-195">Çalıştırma **veritabanını güncelleştir – ayrıntılı** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="6f895-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="6f895-196">(Sürüm Düşürme dahil), belirli bir sürüme geçirme</span><span class="sxs-lookup"><span data-stu-id="6f895-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="6f895-197">Şu ana kadar en son geçiş için her zaman yükselttik, ancak Yükseltme/indirgeme belirli bir geçiş istediğinizde zamanlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="6f895-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="6f895-198">Veritabanımızdaki olduğu içinde çalıştırdıktan sonra duruma geçirmek istediğimiz varsayalım bizim **AddBlogUrl** geçiş.</span><span class="sxs-lookup"><span data-stu-id="6f895-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="6f895-199">Kullanabiliriz **– TargetMigration** düşürmek için bu geçiş anahtarı.</span><span class="sxs-lookup"><span data-stu-id="6f895-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="6f895-200">Çalıştırma **veritabanını güncelleştir – TargetMigration: AddBlogUrl** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="6f895-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="6f895-201">Bu komut için aşağı betiği çalıştırır bizim **AddBlogAbstract** ve **AddPostClass** geçişler.</span><span class="sxs-lookup"><span data-stu-id="6f895-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="6f895-202">Tüm kullanıma sunmak istiyorsanız boş bir veritabanı için yedekleme sonra kullanabileceğiniz **veritabanını güncelleştir – TargetMigration: $InitialDatabase** komutu.</span><span class="sxs-lookup"><span data-stu-id="6f895-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="6f895-203">SQL komut dosyası alma</span><span class="sxs-lookup"><span data-stu-id="6f895-203">Getting a SQL Script</span></span>

<span data-ttu-id="6f895-204">Başka bir geliştiricinin kendi makinesinde bu değişiklikleri istiyorsa, biz bizim değişiklikleri kaynak denetimine iade sonra bunlar yalnızca eşitleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f895-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="6f895-205">Bunlar bizim yeni geçişleri oluşturduktan sonra yalnızca yerel olarak uygulanan değişiklikler için veritabanını Güncelleştir komutunu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f895-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="6f895-206">Bir test sunucusu ve üretim için sonuçta bu değişiklikleri dışına istiyoruz, ancak büyük olasılıkla bir SQL betiği biz kapatmak için sunduğumuz DBA dağıtabilir istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="6f895-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="6f895-207">Çalıştırma **veritabanını Güncelleştir** komut ancak bu kez **– betik** değişiklikleri bir komut dosyasına yazılmış yerine böylece uygulanan bayrak.</span><span class="sxs-lookup"><span data-stu-id="6f895-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="6f895-208">Biz de için komut dosyası oluşturmak için bir kaynak ve hedef geçiş belirteceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6f895-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="6f895-209">Boş bir veritabanından gitmek için bir betik istiyoruz (**$InitialDatabase**) en son sürüme (geçiş **AddPostAbstract**).</span><span class="sxs-lookup"><span data-stu-id="6f895-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="6f895-210">*Bir hedef geçiş belirtmezseniz, geçişler son geçiş hedef olarak kullanın. Kaynak geçişleri belirtmezseniz, veritabanının geçerli durumu geçişleri kullanır.*</span><span class="sxs-lookup"><span data-stu-id="6f895-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="6f895-211">Çalıştırma **veritabanını güncelleştir-- SourceMigration betik: $InitialDatabase - TargetMigration: AddPostAbstract** komutunu Paket Yöneticisi Konsolu</span><span class="sxs-lookup"><span data-stu-id="6f895-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="6f895-212">Code First geçişleri geçiş ardışık düzeni çalışacak ancak değişiklikleri uygulamak yerine, bunları .sql dosya sizin için yazamadı.</span><span class="sxs-lookup"><span data-stu-id="6f895-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="6f895-213">Komut dosyası oluşturulduktan sonra Visual Studio, görüntülemek veya kaydetmek hazır açılır.</span><span class="sxs-lookup"><span data-stu-id="6f895-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="6f895-214">Idempotent betikleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6f895-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="6f895-215">Belirtirseniz EF6'ile başlayan **– SourceMigration $InitialDatabase** oluşturulan betiği 'ıdempotent' olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="6f895-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="6f895-216">Idempotent betikleri herhangi bir sürümü şu anda bir veritabanını en son sürüme yükseltebilirsiniz (veya belirtilen sürümü kullanırsanız **– TargetMigration**).</span><span class="sxs-lookup"><span data-stu-id="6f895-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="6f895-217">Oluşturulan kodun denetlemek için mantığı içerir  **\_ \_MigrationsHistory** tablo ve yalnızca daha önce sorgularınızda uygulanmamış değişiklikleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="6f895-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="6f895-218">Uygulama başlangıcında (MigrateDatabaseToLatestVersion Başlatıcı) otomatik olarak yükseltme</span><span class="sxs-lookup"><span data-stu-id="6f895-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="6f895-219">Uygulamanızı dağıtıyorsanız (geçişleri beklemedeki uygulayarak) veritabanı otomatik olarak yükseltmek istediğiniz, uygulamayı başlatır.</span><span class="sxs-lookup"><span data-stu-id="6f895-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="6f895-220">Kaydederek bunu yapabilirsiniz **MigrateDatabaseToLatestVersion** veritabanı Başlatıcı.</span><span class="sxs-lookup"><span data-stu-id="6f895-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="6f895-221">Bir veritabanı başlatıcısı, yalnızca veritabanı doğru şekilde kurulduğundan emin olmak için kullanılan bazı mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="6f895-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="6f895-222">Bu mantık bağlamı içinde uygulama işlemi kullanılan ilk kez çalıştırılan (**AppDomain**).</span><span class="sxs-lookup"><span data-stu-id="6f895-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="6f895-223">Biz güncelleştirebilirsiniz **Program.cs** ayarlamak için aşağıda gösterildiği gibi dosya **MigrateDatabaseToLatestVersion** (satır 14) bağlam kullandığımız önce BlogContext için Başlatıcı.</span><span class="sxs-lookup"><span data-stu-id="6f895-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="6f895-224">Aynı zamanda bir kullanmadan eklemeniz gerektiğini unutmayın bildirimi **System.Data.Entity** ad alanı (Satır 5).</span><span class="sxs-lookup"><span data-stu-id="6f895-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="6f895-225">*Biz ihtiyacımız bağlam türünü belirtmek için bu Başlatıcı örneğini oluşturduğunuzda (**BlogContext**) ve geçişleri yapılandırma (**yapılandırma**)-geçişleri yapılandırma aldı sınıfıdır eklenen bizim **geçişler** klasörüne geçişleri etkinleştirdik.*</span><span class="sxs-lookup"><span data-stu-id="6f895-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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

<span data-ttu-id="6f895-226">Artık her veritabanı, hedefliyor, ilk denetleyecek bizim çalıştıracağını güncel olduğundan ve yüklü değilse bekleyen tüm geçişler.</span><span class="sxs-lookup"><span data-stu-id="6f895-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
