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
# <a name="code-first-migrations"></a><span data-ttu-id="dbbba-102">Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="dbbba-102">Code First Migrations</span></span>
<span data-ttu-id="dbbba-103">Code First Migrations, Code First iş akışını kullanıyorsanız uygulamanızın veritabanı şemasını geliştirmek için önerilen yoldur.</span><span class="sxs-lookup"><span data-stu-id="dbbba-103">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="dbbba-104">Geçişler, izin veren bir araç kümesi sağlar:</span><span class="sxs-lookup"><span data-stu-id="dbbba-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="dbbba-105">EF modelinizle birlikte çalışarak ilk veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="dbbba-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="dbbba-106">EF modelinizde yaptığınız değişiklikleri izlemek için geçişler oluşturma</span><span class="sxs-lookup"><span data-stu-id="dbbba-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="dbbba-107">Veritabanınızı bu değişikliklerle güncel tutun</span><span class="sxs-lookup"><span data-stu-id="dbbba-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="dbbba-108">Aşağıdaki izlenecek yol, Entity Framework Code First Migrations bir genel bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="dbbba-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="dbbba-109">Tüm yönergeyi tamamlayabilir ya da ilgilendiğiniz konuya atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="dbbba-110">Aşağıdaki konular ele alınmıştır:</span><span class="sxs-lookup"><span data-stu-id="dbbba-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="dbbba-111">Ilk model & veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="dbbba-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="dbbba-112">Geçişleri kullanmaya başlamadan önce bir proje ve ile çalışmak için bir Code First modeli gerekir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="dbbba-113">Bu izlenecek yol için kurallı **Blog** ve **gönderi** modelini kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="dbbba-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="dbbba-114">Yeni bir **Migrationsdemo** konsol uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="dbbba-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="dbbba-115">Projeye **EntityFramework** NuGet paketinin en son sürümünü ekleyin</span><span class="sxs-lookup"><span data-stu-id="dbbba-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="dbbba-116">**Araçlar –&gt; kitaplığı Paket Yöneticisi –&gt; Paket Yöneticisi konsolu**</span><span class="sxs-lookup"><span data-stu-id="dbbba-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="dbbba-117">**Install-Package EntityFramework** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="dbbba-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="dbbba-118">Aşağıda gösterilen kodla bir **model.cs** dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="dbbba-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="dbbba-119">Bu kod, etki alanı modelimizi ve EF Code First bağlamımız bir **BlogContext** sınıfını oluşturan tek bir **Blog** sınıfını tanımlar</span><span class="sxs-lookup"><span data-stu-id="dbbba-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="dbbba-120">Artık bir modelimiz olduğuna göre, veri erişimi gerçekleştirmek için bunu kullanmanın zamanı.</span><span class="sxs-lookup"><span data-stu-id="dbbba-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="dbbba-121">**Program.cs** dosyasını aşağıda gösterilen kodla güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="dbbba-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="dbbba-122">Uygulamanızı çalıştırın ve sizin için bir **Migrationscodedemo. BlogContext** veritabanının oluşturulduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![Veritabanı Yereldb](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="dbbba-124">Geçişleri etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="dbbba-124">Enabling Migrations</span></span>

<span data-ttu-id="dbbba-125">Modelinizde daha fazla değişiklik yapmak zaman alabilir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="dbbba-126">Blog sınıfına bir URL özelliği tanıtalım.</span><span class="sxs-lookup"><span data-stu-id="dbbba-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="dbbba-127">Uygulamayı yeniden çalıştırırsanız, *veritabanı oluşturulduktan sonra ' BlogContext ' bağlamının yedeklendiğini belirten bir InvalidOperationException alacaksınız. Veritabanını güncelleştirmek için Code First Migrations kullanmayı düşünün (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="dbbba-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="dbbba-128">Özel durum önerdiğinde Code First Migrations kullanmaya başlama zamanı.</span><span class="sxs-lookup"><span data-stu-id="dbbba-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="dbbba-129">İlk adım bağlamımız için geçişleri etkinleştirmektir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="dbbba-130">Paket Yöneticisi konsolunda **Etkinleştir-geçişleri** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="dbbba-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="dbbba-131">Bu komut, projenize bir **geçişler** klasörü ekledi.</span><span class="sxs-lookup"><span data-stu-id="dbbba-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="dbbba-132">Bu yeni klasör iki dosya içerir:</span><span class="sxs-lookup"><span data-stu-id="dbbba-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="dbbba-133">**Yapılandırma sınıfı.**</span><span class="sxs-lookup"><span data-stu-id="dbbba-133">**The Configuration class.**</span></span> <span data-ttu-id="dbbba-134">Bu sınıf, bağlam için geçişlerin nasıl davranacağını yapılandırmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="dbbba-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="dbbba-135">Bu kılavuzda, yalnızca varsayılan yapılandırmayı kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="dbbba-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="dbbba-136">*Projenizde yalnızca tek bir Code First bağlamı olduğundan, Enable-geçişler bu yapılandırmanın uygulandığı bağlam türüne otomatik olarak doldurulur.*</span><span class="sxs-lookup"><span data-stu-id="dbbba-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="dbbba-137">**Bir ınitialcreate geçişi**.</span><span class="sxs-lookup"><span data-stu-id="dbbba-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="dbbba-138">Bu geçiş, geçişleri etkinleştirmeden önce bizim için bir veritabanı oluşturmak Code First zaten vardı.</span><span class="sxs-lookup"><span data-stu-id="dbbba-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="dbbba-139">Bu yapı iskelesi geçişi içindeki kod, veritabanında zaten oluşturulmuş olan nesneleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="dbbba-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="dbbba-140">Küçük harfli bir **blogID** ve **ad** sütunları olan **Blog** tablosu.</span><span class="sxs-lookup"><span data-stu-id="dbbba-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="dbbba-141">Dosya adı, sıralamaya yardımcı olacak bir zaman damgası içerir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="dbbba-142">*Veritabanı zaten oluşturulmadıysa, bu ınitialcreate geçişi projeye eklenmemiş olur. Bunun yerine, ekleme geçişi ilk kez bu tabloları oluşturmak için kod yeni bir geçişe iskele olacaktır.*</span><span class="sxs-lookup"><span data-stu-id="dbbba-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="dbbba-143">Aynı veritabanını hedefleyen birden çok model</span><span class="sxs-lookup"><span data-stu-id="dbbba-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="dbbba-144">EF6 ' den önceki sürümleri kullanırken bir veritabanının şemasını oluşturmak/yönetmek için yalnızca bir Code First modeli kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="dbbba-145">Bu, hangi girişlerin hangi modele ait olduğunu belirlemenin hiçbir yolu olmadan veritabanı başına tek bir **\_\_MigrationsHistory** tablosunun sonucudur.</span><span class="sxs-lookup"><span data-stu-id="dbbba-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="dbbba-146">EF6 ile başlayarak, **yapılandırma** sınıfı bir **contextKey** özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="dbbba-147">Bu, her bir Code First modeli için benzersiz bir tanımlayıcı işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="dbbba-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="dbbba-148">**\_\_MigrationsHistory** tablosundaki karşılık gelen bir sütun, birden çok modeldeki girişlerin tabloyu paylaşmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="dbbba-149">Varsayılan olarak, bu özellik, bağlamınızın tam adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="dbbba-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="dbbba-150">Çalıştırılan & geçişleri oluşturma</span><span class="sxs-lookup"><span data-stu-id="dbbba-150">Generating & Running Migrations</span></span>

<span data-ttu-id="dbbba-151">Code First Migrations, öğrenecek iki birincil komuta sahiptir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="dbbba-152">**Geçiş geçişi** , son geçişin oluşturulmasından bu yana modelinizde yaptığınız değişikliklere dayalı olarak bir sonraki geçişi kankatalacak</span><span class="sxs-lookup"><span data-stu-id="dbbba-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="dbbba-153">**Güncelleştir-veritabanı** bekleyen geçişleri veritabanına uygular</span><span class="sxs-lookup"><span data-stu-id="dbbba-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="dbbba-154">Eklediğimiz yeni URL özelliğinden yararlanmak için bir geçişi bir geçişe katdık.</span><span class="sxs-lookup"><span data-stu-id="dbbba-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="dbbba-155">**Add-Migration** komutu bu geçişlere bir ad vermemizi sağlar. yalnızca **Bizaddblogurl**'yi çağıralım.</span><span class="sxs-lookup"><span data-stu-id="dbbba-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="dbbba-156">Paket Yöneticisi konsolunda **Add-Migration AddBlogUrl** komutunu çalıştırma</span><span class="sxs-lookup"><span data-stu-id="dbbba-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="dbbba-157">**Geçişler** klasöründe artık yeni bir **Addblogurl** geçişi var.</span><span class="sxs-lookup"><span data-stu-id="dbbba-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="dbbba-158">Geçiş dosya adı, sıralamaya yardımcı olması için zaman damgasıyla önceden düzeltildi</span><span class="sxs-lookup"><span data-stu-id="dbbba-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="dbbba-159">Şimdi bu geçişe düzenleme veya ekleme yapabiliriz, ancak her şey oldukça iyi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="dbbba-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="dbbba-160">Bu geçişi veritabanına uygulamak için **Update-Database** ' i kullanalım.</span><span class="sxs-lookup"><span data-stu-id="dbbba-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="dbbba-161">Package Manager konsolunda **Update-Database** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="dbbba-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="dbbba-162">Code First Migrations, **geçişleri** klasörünüzdeki geçişleri veritabanına uygulanmış olanlarla karşılaştıracaktır.</span><span class="sxs-lookup"><span data-stu-id="dbbba-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="dbbba-163">**Addblogurl** geçişinin uygulanması gerektiğini görebilir ve bu işlem çalışır.</span><span class="sxs-lookup"><span data-stu-id="dbbba-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="dbbba-164">**Migrationsdemo. BlogContext** veritabanı artık **Bloglar** tablosuna **URL** sütununu içerecek şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="dbbba-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="dbbba-165">Geçişleri özelleştirme</span><span class="sxs-lookup"><span data-stu-id="dbbba-165">Customizing Migrations</span></span>

<span data-ttu-id="dbbba-166">Şimdiye kadar herhangi bir değişiklik yapmadan bir geçiş oluşturmuş ve çalıştırdık.</span><span class="sxs-lookup"><span data-stu-id="dbbba-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="dbbba-167">Şimdi varsayılan olarak oluşturulan kodu düzenleyerek göz atalım.</span><span class="sxs-lookup"><span data-stu-id="dbbba-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="dbbba-168">Modelimizde daha fazla değişiklik yapma zamanı, **Blog** sınıfına yeni bir **Derecelendirme** özelliği ekleyelim</span><span class="sxs-lookup"><span data-stu-id="dbbba-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="dbbba-169">Ayrıca yeni bir **Post** sınıfı ekleyelim</span><span class="sxs-lookup"><span data-stu-id="dbbba-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="dbbba-170">Ayrıca **, blog ve** **gönderi** arasındaki ilişkinin diğer sonunu oluşturmak Için **Blog** sınıfına bir **gönderi** koleksiyonu ekleyeceğiz</span><span class="sxs-lookup"><span data-stu-id="dbbba-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="dbbba-171">Geçiş sırasında en iyi tahmininizi Code First Migrations Scam etmek için **Add-Migration** komutunu kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="dbbba-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="dbbba-172">Bu geçiş **Addpostclass**'ı çağıracağız.</span><span class="sxs-lookup"><span data-stu-id="dbbba-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="dbbba-173">Paket Yöneticisi konsolundaki **Add-Migration AddPostClass** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dbbba-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="dbbba-174">Code First Migrations, bu değişikliklerin etkili bir şekilde sağlam bir işi olduğundan, değiştirmek isteyebileceğiniz bazı şeyler vardır:</span><span class="sxs-lookup"><span data-stu-id="dbbba-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="dbbba-175">İlk olarak, postalarınıza benzersiz bir dizin ekleyelim **. title** sütununa (aşağıdaki kodda 22 & 29 satıra ekleme).</span><span class="sxs-lookup"><span data-stu-id="dbbba-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="dbbba-176">Ayrıca, null olamayan **blogların. derecelendirme** sütununu da ekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="dbbba-177">Tabloda varolan veriler varsa, yeni sütun için veri türünün CLR varsayılanı atanır (derecelendirme tamdır, yani **0**olur).</span><span class="sxs-lookup"><span data-stu-id="dbbba-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="dbbba-178">Ancak varsayılan olarak **3** değerini belirtmek istiyoruz, bu sayede **blogların** tablosundaki mevcut satırların bir sıra derecelendirmesi ile başlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="dbbba-179">(Aşağıdaki kodun 24. satırında belirtilen varsayılan değeri görebilirsiniz)</span><span class="sxs-lookup"><span data-stu-id="dbbba-179">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="dbbba-180">Düzenlenmiş geçişimiz çalışmaya devam eder, bu nedenle veritabanını güncel hale getirmek için **Update-Database** ' i kullanalım.</span><span class="sxs-lookup"><span data-stu-id="dbbba-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="dbbba-181">Bu süre, Code First Migrations çalışan SQL 'i görebilmeniz için **– verbose** bayrağını belirtlim.</span><span class="sxs-lookup"><span data-stu-id="dbbba-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="dbbba-182">Package Manager konsolundaki **Update-Database – verbose** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dbbba-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="dbbba-183">Veri hareketi/özel SQL</span><span class="sxs-lookup"><span data-stu-id="dbbba-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="dbbba-184">Şimdiye kadar herhangi bir veriyi değiştirmeyin veya taşımamış geçiş işlemlerine baktık, şimdi bazı verilerin etrafında taşınması gereken bir şeye bakalım.</span><span class="sxs-lookup"><span data-stu-id="dbbba-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="dbbba-185">Henüz veri hareketi için yerel destek yoktur, ancak betiğimizde herhangi bir noktada bazı rastgele SQL komutları çalıştırabiliriz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="dbbba-186">Modelinize bir **Post. Abstract** özelliği ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="dbbba-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="dbbba-187">Daha sonra, **içerik** sütununun başından itibaren bazı metinleri kullanarak mevcut gönderimler için **Özet** 'i önceden dolduracağız.</span><span class="sxs-lookup"><span data-stu-id="dbbba-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="dbbba-188">Geçiş sırasında en iyi tahmininizi Code First Migrations Scam etmek için **Add-Migration** komutunu kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="dbbba-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="dbbba-189">Paket Yöneticisi konsolunda **Add-Migration AddPostAbstract** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dbbba-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="dbbba-190">Oluşturulan geçiş, şema değişikliklerinden yararlanır, ancak her gönderi için içeriğin ilk 100 karakterini kullanarak **soyut** sütunu önceden doldurmak de istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="dbbba-191">Bunu, SQL 'e bırakarak ve sütun eklendikten sonra bir **Update** ifadesini çalıştırarak yapabiliriz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="dbbba-192">(Aşağıdaki kodda 12. satırda ekleme)</span><span class="sxs-lookup"><span data-stu-id="dbbba-192">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="dbbba-193">Düzenlenmiş geçişimiz iyi arıyor, bu nedenle veritabanını güncel hale getirmek için **Update-Database** ' i kullanalım.</span><span class="sxs-lookup"><span data-stu-id="dbbba-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="dbbba-194">Veritabanına karşı çalışan SQL 'i görebilmemiz için **– verbose** bayrağını belirteceğiz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="dbbba-195">Package Manager konsolundaki **Update-Database – verbose** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dbbba-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="dbbba-196">Belirli bir sürüme geçiş (düşürme dahil)</span><span class="sxs-lookup"><span data-stu-id="dbbba-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="dbbba-197">Şimdiye kadar her zaman en son geçişe yükseltildik, ancak belirli bir geçişe yükseltme/düşürme yapmak istediğiniz zamanlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="dbbba-198">Şimdi, **Addblogurl** geçişimizi çalıştırdıktan sonra veritabanını bulunduğu duruma geçirmek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="dbbba-199">Bu geçişe düşürme sağlamak için **– Targetmigration** anahtarını kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="dbbba-200">Package Manager konsolundaki **Update-Database – TargetMigration: AddBlogUrl** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="dbbba-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="dbbba-201">Bu komut, **AddBlogAbstract** ve **addpostclass** geçişlerimiz için aşağı komut dosyasını çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="dbbba-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="dbbba-202">Bir bütün olarak boş bir veritabanına geri dönmek istiyorsanız, **Update-Database – TargetMigration: $InitialDatabase** komutunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="dbbba-203">SQL betiği alma</span><span class="sxs-lookup"><span data-stu-id="dbbba-203">Getting a SQL Script</span></span>

<span data-ttu-id="dbbba-204">Başka bir geliştirici makinesinde bu değişikliklere istiyorsa, değişiklikleri kaynak denetimine denetliyoruz bir kez daha zaman eşitlenebilir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="dbbba-205">Yeni geçişlerimiz olduktan sonra, değişikliklerin yerel olarak uygulanmasını sağlamak için yalnızca Update-database komutunu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="dbbba-206">Ancak, bu değişiklikleri bir test sunucusuna göndermek istiyoruz ve son olarak üretimde, büyük olasılıkla DBA için bir SQL betiği sunmamız istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="dbbba-207">**Update-Database** komutunu çalıştırın, ancak bu kez, değişikliklerin uygulanması yerine bir betiğe yazılması Için **– Script** bayrağını belirtin.</span><span class="sxs-lookup"><span data-stu-id="dbbba-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="dbbba-208">Ayrıca, komut dosyasını oluşturmak için bir kaynak ve hedef geçişi de belirteceğiz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="dbbba-209">Bir betiğin boş bir veritabanından ( **$InitialDatabase**) en son sürüme (geçiş **Addpostabstract**) gitmesini istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="dbbba-210">*Hedef geçiş belirtmezseniz, geçişler hedef olarak en son geçişi kullanır. Kaynak geçişleri belirtmezseniz, geçişler veritabanının geçerli durumunu kullanacaktır.*</span><span class="sxs-lookup"><span data-stu-id="dbbba-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="dbbba-211">**Güncelleştirme-veritabanı-betiği-SourceMigration: $InitialDatabase-TargetMigration: AddPostAbstract** komutunu, Paket Yöneticisi konsolu 'nda çalıştırın</span><span class="sxs-lookup"><span data-stu-id="dbbba-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="dbbba-212">Code First Migrations, geçiş işlem hattını çalıştıracak ancak değişiklikleri gerçekten uygulamak yerine, sizin için bir. SQL dosyasına yazacak.</span><span class="sxs-lookup"><span data-stu-id="dbbba-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="dbbba-213">Betik oluşturulduktan sonra, Visual Studio 'da sizin için açılır, bu sizin için sizin için açılır.</span><span class="sxs-lookup"><span data-stu-id="dbbba-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="dbbba-214">Idempotent betikleri üretiliyor</span><span class="sxs-lookup"><span data-stu-id="dbbba-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="dbbba-215">EF6 ile başlayarak, **– Sourcemigration $InitialDatabase** belirtirseniz oluşturulan betik ' ıdempotent ' olur.</span><span class="sxs-lookup"><span data-stu-id="dbbba-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="dbbba-216">Idempotent betikleri, şu anda herhangi bir sürümdeki bir veritabanını en son sürüme (ya da **– Targetmigration**kullanıyorsanız, belirtilen sürüme) yükseltebilirler.</span><span class="sxs-lookup"><span data-stu-id="dbbba-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="dbbba-217">Oluşturulan betik, **\_\_MigrationsHistory** tablosunu denetleme mantığını içerir ve yalnızca daha önce uygulanmamış değişiklikleri uygular.</span><span class="sxs-lookup"><span data-stu-id="dbbba-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="dbbba-218">Uygulama başlangıcında otomatik olarak yükseltme (MigrateDatabaseToLatestVersion Başlatıcısı)</span><span class="sxs-lookup"><span data-stu-id="dbbba-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="dbbba-219">Uygulamanızı dağıtıyorsanız, uygulamanın başlatıldığında veritabanını otomatik olarak yükseltmesini (bekleyen geçişler uygulayarak) isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="dbbba-220">Bunu, **Migratedatabasetolatestversion** veritabanı Başlatıcısı ' nı kaydederek yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dbbba-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="dbbba-221">Veritabanı başlatıcısı yalnızca veritabanının doğru şekilde ayarlandığından emin olmak için kullanılan bir mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="dbbba-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="dbbba-222">Bu mantık, içerik uygulama işlemi (**AppDomain**) içinde ilk kez kullanıldığında çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="dbbba-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="dbbba-223">Bağlamı kullanmadan önce BlogContext için **Migratedatabasetolatestversion** başlatıcısı 'nı ayarlamak üzere, aşağıda gösterildiği gibi **program.cs** dosyasını güncelleştirebiliriz (satır 14).</span><span class="sxs-lookup"><span data-stu-id="dbbba-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="dbbba-224">**System. Data. Entity** ad alanı (5. satır) için bir using ifadesini de eklemeniz gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="dbbba-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="dbbba-225">*Bu başlatıcı 'nin bir örneğini oluşturduğumuzda, bağlam türünü (**BlogContext**) ve geçişler yapılandırmasını (**yapılandırma**) belirtmemiz gerekir-geçişleri etkinleştirmemiz durumunda **geçişler klasörünüze eklenen** sınıftır.*</span><span class="sxs-lookup"><span data-stu-id="dbbba-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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

<span data-ttu-id="dbbba-226">Artık uygulamamız her çalıştığında, öncelikle hedeflediği veritabanının güncel olup olmadığını kontrol eder ve yoksa bekleyen geçişleri uygular.</span><span class="sxs-lookup"><span data-stu-id="dbbba-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
