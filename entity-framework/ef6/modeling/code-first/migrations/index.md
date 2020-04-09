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
# <a name="code-first-migrations"></a><span data-ttu-id="f0a64-102">Kod İlk Geçişler</span><span class="sxs-lookup"><span data-stu-id="f0a64-102">Code First Migrations</span></span>
<span data-ttu-id="f0a64-103">Code First Migrations, Önce Kod iş akışını kullanıyorsanız, uygulamanızın veritabanı şemasını geliştirmenin önerilen yoludur.</span><span class="sxs-lookup"><span data-stu-id="f0a64-103">Code First Migrations is the recommended way to evolve your application's database schema if you are using the Code First workflow.</span></span> <span data-ttu-id="f0a64-104">Geçişler, şuna izin veren bir araç kümesi sağlar:</span><span class="sxs-lookup"><span data-stu-id="f0a64-104">Migrations provide a set of tools that allow:</span></span>

1. <span data-ttu-id="f0a64-105">EF modelinizde çalışan bir başlangıç veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f0a64-105">Create an initial database that works with your EF model</span></span>
2. <span data-ttu-id="f0a64-106">EF modelinizde yaptığınız değişiklikleri izlemek için geçiş oluşturma</span><span class="sxs-lookup"><span data-stu-id="f0a64-106">Generating migrations to keep track of changes you make to your EF model</span></span>
2. <span data-ttu-id="f0a64-107">Veritabanınızı bu değişikliklerden haberdar edin</span><span class="sxs-lookup"><span data-stu-id="f0a64-107">Keep your database up to date with those changes</span></span>

<span data-ttu-id="f0a64-108">Aşağıdaki izlenecek yol, Varlık Çerçevesi'ndeki İlk Geçişler Kodu'na genel bir bakış sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-108">The following walkthrough will provide an overview Code First Migrations in Entity Framework.</span></span> <span data-ttu-id="f0a64-109">Tüm gözden geçiriyi tamamlayabilir veya ilgilendiğiniz konuya atlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-109">You can either complete the entire walkthrough or skip to the topic you are interested in.</span></span> <span data-ttu-id="f0a64-110">Aşağıdaki konular ele alınmıştır:</span><span class="sxs-lookup"><span data-stu-id="f0a64-110">The following topics are covered:</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="f0a64-111">İlk Model & Veritabanı Oluşturma</span><span class="sxs-lookup"><span data-stu-id="f0a64-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="f0a64-112">Geçişleri kullanmaya başlamadan önce bir projeye ve çalışmak için Bir İlk Kod modeline ihtiyacımız vardır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="f0a64-113">Bu izlenebilir için biz kanonik **Blog** ve **Post** modeli kullanmak için gidiyoruz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="f0a64-114">Yeni **migrationsDemo** Console uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="f0a64-114">Create a new **MigrationsDemo** Console application</span></span>
-   <span data-ttu-id="f0a64-115">**EntityFramework** NuGet paketinin en son sürümünü projeye ekleyin</span><span class="sxs-lookup"><span data-stu-id="f0a64-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="f0a64-116">**Araçlar&gt; – Kütüphane&gt; Paket Yöneticisi – Package Manager Konsolu**</span><span class="sxs-lookup"><span data-stu-id="f0a64-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="f0a64-117">**Install-Package EntityFramework** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="f0a64-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="f0a64-118">Aşağıda gösterilen kodu içeren bir **Model.cs** dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f0a64-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="f0a64-119">Bu kod, etki alanı modelimizi oluşturan tek bir **Blog** sınıfımızı ve EF Code First bağlamımız olan **bir BlogContext** sınıfımızı tanımlar</span><span class="sxs-lookup"><span data-stu-id="f0a64-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="f0a64-120">Artık bir modelimiz olduğuna göre veri erişimi gerçekleştirmek için kullanma nın zamanı gelmiştir.</span><span class="sxs-lookup"><span data-stu-id="f0a64-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="f0a64-121">Program.cs **dosyayı** aşağıda gösterilen kodla güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="f0a64-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="f0a64-122">Uygulamanızı çalıştırın ve sizin için bir **MigrationsCodeDemo.BlogContext** veritabanı oluşturulduğunu göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-122">Run your application and you will see that a **MigrationsCodeDemo.BlogContext** database is created for you.</span></span>

    ![Veritabanı LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="f0a64-124">Geçişleri Etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f0a64-124">Enabling Migrations</span></span>

<span data-ttu-id="f0a64-125">Modelimizde daha fazla değişiklik yapma zamanı.</span><span class="sxs-lookup"><span data-stu-id="f0a64-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="f0a64-126">Blog sınıfına bir Url özelliği tanıyalım.</span><span class="sxs-lookup"><span data-stu-id="f0a64-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="f0a64-127">Uygulamayı yeniden çalıştıracak olursanız, veritabanı oluşturulduğundan *beri 'BlogContext' bağlamını destekleyen modelin değiştiğini belirten bir GeçersizOperasyonİst alırsınız. Veritabanını güncelleştirmek için Kod İlk Geçişleri 'ni kullanmayı düşünün (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="f0a64-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="f0a64-128">Özel olarak da anlaşılacağı gibi, Kod İlk Geçişler kullanmaya başlama zamanı.</span><span class="sxs-lookup"><span data-stu-id="f0a64-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="f0a64-129">İlk adım, bağlamımız için geçişleri mümkün kılmaktır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-129">The first step is to enable migrations for our context.</span></span>

-   <span data-ttu-id="f0a64-130">Paket Yöneticisi Konsolunda **Geçişleri Etkinleştir** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="f0a64-130">Run the **Enable-Migrations** command in Package Manager Console</span></span>

    <span data-ttu-id="f0a64-131">Bu komut projemize bir **Geçişler** klasörü ekledi.</span><span class="sxs-lookup"><span data-stu-id="f0a64-131">This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="f0a64-132">Bu yeni klasör iki dosya içerir:</span><span class="sxs-lookup"><span data-stu-id="f0a64-132">This new folder contains two files:</span></span>

-   <span data-ttu-id="f0a64-133">**Yapılandırma sınıfı.**</span><span class="sxs-lookup"><span data-stu-id="f0a64-133">**The Configuration class.**</span></span> <span data-ttu-id="f0a64-134">Bu sınıf, Geçişler'in bağlamınız için nasıl bir şekilde nasıl bir şekilde nasıl bir şekilde nasıl bir şekilde uygun olduğunu yapılandırmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="f0a64-134">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="f0a64-135">Bu izlenecek yol için varsayılan yapılandırmayı kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="f0a64-135">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="f0a64-136">*Projenizde yalnızca tek bir Code First bağlamı olduğundan, Geçişleri Etkinleştir medenleri bu yapılandırmanın geçerli olduğu bağlam türünü otomatik olarak doldurdu.*</span><span class="sxs-lookup"><span data-stu-id="f0a64-136">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>
-   <span data-ttu-id="f0a64-137">**Başlangıç Geçişi Oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="f0a64-137">**An InitialCreate migration**.</span></span> <span data-ttu-id="f0a64-138">Bu geçiş, geçişleri etkinleştirmeden önce Code First'in bizim için bir veritabanı oluşturmasına sahip olduğumuz için oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="f0a64-138">This migration was generated because we already had Code First create a database for us, before we enabled migrations.</span></span> <span data-ttu-id="f0a64-139">Bu iskelegeçişindeki kod, veritabanında zaten oluşturulmuş nesneleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f0a64-139">The code in this scaffolded migration represents the objects that have already been created in the database.</span></span> <span data-ttu-id="f0a64-140">Bizim durumumuzda bir **BlogId** ve **Ad** sütunları ile **Blog** tablodur.</span><span class="sxs-lookup"><span data-stu-id="f0a64-140">In our case that is the **Blog** table with a **BlogId** and **Name** columns.</span></span> <span data-ttu-id="f0a64-141">Dosya adı, sipariş vermeye yardımcı olacak bir zaman damgası içerir.</span><span class="sxs-lookup"><span data-stu-id="f0a64-141">The filename includes a timestamp to help with ordering.</span></span>
    <span data-ttu-id="f0a64-142">*Veritabanı zaten oluşturulmasaydı bu InitialCreate geçişi projeye eklenmezdi. Bunun yerine, bu tabloları oluşturmak için kod Ekleme-Geçiş dediğimiz ilk kez yeni bir geçiş iskele olacaktır.*</span><span class="sxs-lookup"><span data-stu-id="f0a64-142">*If the database had not already been created this InitialCreate migration would not have been added to the project. Instead, the first time we call Add-Migration the code to create these tables would be scaffolded to a new migration.*</span></span>

### <a name="multiple-models-targeting-the-same-database"></a><span data-ttu-id="f0a64-143">Aynı Veritabanını Hedefleyen Birden Çok Model</span><span class="sxs-lookup"><span data-stu-id="f0a64-143">Multiple Models Targeting the Same Database</span></span>

<span data-ttu-id="f0a64-144">EF6'dan önceki sürümleri kullanırken, veritabanının şemasını oluşturmak/yönetmek için yalnızca bir Code First modeli kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f0a64-144">When using versions prior to EF6, only one Code First model could be used to generate/manage the schema of a database.</span></span> <span data-ttu-id="f0a64-145">Bu, hangi girişlerin \*\* \_ \_\*\* hangi modele ait olduğunu belirlemenin hiçbir yolu olmayan veritabanı başına tek bir MigrationsHistory tablosunun sonucudur.</span><span class="sxs-lookup"><span data-stu-id="f0a64-145">This is the result of a single **\_\_MigrationsHistory** table per database with no way to identify which entries belong to which model.</span></span>

<span data-ttu-id="f0a64-146">EF6 ile **başlayarak, Yapılandırma** sınıfı bir **ContextKey** özelliği içerir.</span><span class="sxs-lookup"><span data-stu-id="f0a64-146">Starting with EF6, the **Configuration** class includes a **ContextKey** property.</span></span> <span data-ttu-id="f0a64-147">Bu, her Code First modeli için benzersiz bir tanımlayıcı görevi görür.</span><span class="sxs-lookup"><span data-stu-id="f0a64-147">This acts as a unique identifier for each Code First model.</span></span> <span data-ttu-id="f0a64-148">\*\* \_ \_MigrationsHistory\*\* tablosundaki karşılık gelen sütun, birden çok modelden girişlerin tabloyu paylaşmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-148">A corresponding column in the **\_\_MigrationsHistory** table allows entries from multiple models to share the table.</span></span> <span data-ttu-id="f0a64-149">Varsayılan olarak, bu özellik bağlamınızın tam nitelikli adına ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-149">By default, this property is set to the fully qualified name of your context.</span></span>

## <a name="generating--running-migrations"></a><span data-ttu-id="f0a64-150">& Çalıştırma Geçişleri Oluşturma</span><span class="sxs-lookup"><span data-stu-id="f0a64-150">Generating & Running Migrations</span></span>

<span data-ttu-id="f0a64-151">Kod İlk Geçişler aşina olacak iki birincil komutları vardır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-151">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="f0a64-152">**Add-Migration,** son geçiş oluşturulduğundan beri modelinizde yaptığınız değişikliklere göre bir sonraki geçişi şekillendirecek</span><span class="sxs-lookup"><span data-stu-id="f0a64-152">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="f0a64-153">**Update-Database** veritabanına bekleyen geçişleri uygular</span><span class="sxs-lookup"><span data-stu-id="f0a64-153">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="f0a64-154">Eklediğimiz yeni Url özelliğinin icabına bakmak için bir göç inşa etmeliyiz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-154">We need to scaffold a migration to take care of the new Url property we have added.</span></span> <span data-ttu-id="f0a64-155">**Add-Migration** komutu bize bu göçler bir isim vermek için izin verir, sadece bizim **AddBlogUrl**çağıralım.</span><span class="sxs-lookup"><span data-stu-id="f0a64-155">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogUrl**.</span></span>

-   <span data-ttu-id="f0a64-156">Paket Yöneticisi Konsolunda **Ekle-Geçiş AddBlogUrl** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="f0a64-156">Run the **Add-Migration AddBlogUrl** command in Package Manager Console</span></span>
-   <span data-ttu-id="f0a64-157">**Migrations** klasöründe artık yeni bir **AddBlogUrl** geçişi var.</span><span class="sxs-lookup"><span data-stu-id="f0a64-157">In the **Migrations** folder we now have a new **AddBlogUrl** migration.</span></span> <span data-ttu-id="f0a64-158">Geçiş dosya adı, sipariş eve yardımcı olmak için bir zaman damgası ile önceden düzeltilir</span><span class="sxs-lookup"><span data-stu-id="f0a64-158">The migration filename is pre-fixed with a timestamp to help with ordering</span></span>

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

<span data-ttu-id="f0a64-159">Şimdi bu göç ekini yapabilir veya ekleyebiliriz ama her şey oldukça iyi görünüyor.</span><span class="sxs-lookup"><span data-stu-id="f0a64-159">We could now edit or add to this migration but everything looks pretty good.</span></span> <span data-ttu-id="f0a64-160">Bu geçişi veritabanına uygulamak için **Update-Database'i** kullanalım.</span><span class="sxs-lookup"><span data-stu-id="f0a64-160">Let’s use **Update-Database** to apply this migration to the database.</span></span>

-   <span data-ttu-id="f0a64-161">Paket Yöneticisi Konsolunda **Update-Database** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="f0a64-161">Run the **Update-Database** command in Package Manager Console</span></span>
-   <span data-ttu-id="f0a64-162">Kod İlk Geçişler, **Geçişler** klasörümüzdeki geçişleri veritabanına uygulananlarla karşılaştırır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-162">Code First Migrations will compare the migrations in our **Migrations** folder with the ones that have been applied to the database.</span></span> <span data-ttu-id="f0a64-163">**Bu AddBlogUrl** geçiş uygulanması gerektiğini göreceksiniz, ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f0a64-163">It will see that the **AddBlogUrl** migration needs to be applied, and run it.</span></span>

<span data-ttu-id="f0a64-164">**MigrationsDemo.BlogContext** veritabanı artık **Bloglar** tablosuna **Url** sütununu içerecek şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="f0a64-164">The **MigrationsDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

## <a name="customizing-migrations"></a><span data-ttu-id="f0a64-165">Geçişleri Özelleştirme</span><span class="sxs-lookup"><span data-stu-id="f0a64-165">Customizing Migrations</span></span>

<span data-ttu-id="f0a64-166">Şimdiye kadar herhangi bir değişiklik yapmadan bir göç oluşturduk ve çalıştırdık.</span><span class="sxs-lookup"><span data-stu-id="f0a64-166">So far we’ve generated and run a migration without making any changes.</span></span> <span data-ttu-id="f0a64-167">Şimdi varsayılan olarak oluşturulan kodu düzenlemeye bakalım.</span><span class="sxs-lookup"><span data-stu-id="f0a64-167">Now let’s look at editing the code that gets generated by default.</span></span>

-   <span data-ttu-id="f0a64-168">Bizim model bazı daha fazla değişiklik yapmak için zamanı, **Blog** sınıfına yeni bir **Derecelendirme** özelliği ekleyelim</span><span class="sxs-lookup"><span data-stu-id="f0a64-168">It’s time to make some more changes to our model, let’s add a new **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

-   <span data-ttu-id="f0a64-169">Ayrıca yeni bir **Post** sınıfı ekleyelim</span><span class="sxs-lookup"><span data-stu-id="f0a64-169">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="f0a64-170">Ayrıca **Blog** ve **Post** arasındaki ilişkinin diğer ucunu oluşturmak için **Blog** sınıfına bir **Gönderiler** koleksiyonu ekleyeceğiz</span><span class="sxs-lookup"><span data-stu-id="f0a64-170">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="f0a64-171">Kod İlk Geçişler bizim için göç en iyi tahmin iskele izin vermek için **Ekle-Geçiş** komutunu kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="f0a64-171">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span> <span data-ttu-id="f0a64-172">Bu göçe **AddPostClass**diyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-172">We’re going to call this migration **AddPostClass**.</span></span>

-   <span data-ttu-id="f0a64-173">Paket Yöneticisi Konsolunda **Ekle-Geçiş EklePostClass** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f0a64-173">Run the **Add-Migration AddPostClass** command in Package Manager Console.</span></span>

<span data-ttu-id="f0a64-174">Kod İlk Göçler bu değişiklikleri iskele oldukça iyi bir iş yaptı, ama değiştirmek isteyebilirsiniz bazı şeyler vardır:</span><span class="sxs-lookup"><span data-stu-id="f0a64-174">Code First Migrations did a pretty good job of scaffolding these changes, but there are some things we might want to change:</span></span>

1.  <span data-ttu-id="f0a64-175">İlk olarak, **Posts.Title** sütununa benzersiz bir dizin ekleyelim (Aşağıdaki koda satır 22 & 29'a ekleme).</span><span class="sxs-lookup"><span data-stu-id="f0a64-175">First up, let’s add a unique index to **Posts.Title** column (Adding in line 22 & 29 in the code below).</span></span>
2.  <span data-ttu-id="f0a64-176">Ayrıca geçersiz **bloglar.Rating** sütunu ekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-176">We’re also adding a non-nullable **Blogs.Rating** column.</span></span> <span data-ttu-id="f0a64-177">Tabloda varolan herhangi bir veri varsa, yeni sütun için veri türünün CLR varsayılanı atanır (Derecelendirme, yani **0**olur).</span><span class="sxs-lookup"><span data-stu-id="f0a64-177">If there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="f0a64-178">Ancak, **Bloglar** tablosundaki varolan satırların iyi bir derecelendirmeyle başlaması için **varsayılan değeri 3** olarak belirtmek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-178">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
    <span data-ttu-id="f0a64-179">(Aşağıdaki kodun 24. satırında belirtilen varsayılan değeri görebilirsiniz)</span><span class="sxs-lookup"><span data-stu-id="f0a64-179">(You can see the default value specified on line 24 of the code below)</span></span>

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

<span data-ttu-id="f0a64-180">Düzenlenen geçişimiz kullanıma hazır, bu nedenle veritabanını güncel duruma getirmek için **Update-Database'i** kullanalım.</span><span class="sxs-lookup"><span data-stu-id="f0a64-180">Our edited migration is ready to go, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="f0a64-181">Bu kez ,İlk Geçişler Kodu'nun çalıştırdığı SQL'i görebilmeniz için **Verbose** bayrağını belirtelim.</span><span class="sxs-lookup"><span data-stu-id="f0a64-181">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="f0a64-182">Paket Yöneticisi Konsolunda **Update-Database –Verbose** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f0a64-182">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="data-motion--custom-sql"></a><span data-ttu-id="f0a64-183">Veri Hareketi / Özel SQL</span><span class="sxs-lookup"><span data-stu-id="f0a64-183">Data Motion / Custom SQL</span></span>

<span data-ttu-id="f0a64-184">Şimdiye kadar herhangi bir veriyi değiştirmeyen veya taşımayan geçiş işlemlerine baktık, şimdi bazı verileri hareket ettirmesi gereken bir şeye bakalım.</span><span class="sxs-lookup"><span data-stu-id="f0a64-184">So far we have looked at migration operations that don’t change or move any data, now let’s look at something that needs to move some data around.</span></span> <span data-ttu-id="f0a64-185">Veri hareketi için henüz yerel bir destek yoktur, ancak komut dosyamızın herhangi bir noktasında bazı rasgele SQL komutları çalıştırabiliriz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-185">There is no native support for data motion yet, but we can run some arbitrary SQL commands at any point in our script.</span></span>

-   <span data-ttu-id="f0a64-186">Modelimize **Bir Post.Abstract** özelliği ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="f0a64-186">Let’s add a **Post.Abstract** property to our model.</span></span> <span data-ttu-id="f0a64-187">Daha sonra, **İçerik** sütununun başından itibaren bazı metinleri kullanarak varolan gönderiler için **Özet'i** önceden dolduracağız.</span><span class="sxs-lookup"><span data-stu-id="f0a64-187">Later, we’re going to pre-populate the **Abstract** for existing posts using some text from the start of the **Content** column.</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="f0a64-188">Kod İlk Geçişler bizim için göç en iyi tahmin iskele izin vermek için **Ekle-Geçiş** komutunu kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="f0a64-188">We'll use the **Add-Migration** command to let Code First Migrations scaffold its best guess at the migration for us.</span></span>

-   <span data-ttu-id="f0a64-189">Paket Yöneticisi Konsolunda **Ekle-Geçiş AddPostAbstract** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f0a64-189">Run the **Add-Migration AddPostAbstract** command in Package Manager Console.</span></span>
-   <span data-ttu-id="f0a64-190">Oluşturulan geçiş şema değişiklikleri ilgilenir ama biz de her yazı için içerik ilk 100 karakter kullanarak **Özet** sütun önceden doldurmak istiyorum.</span><span class="sxs-lookup"><span data-stu-id="f0a64-190">The generated migration takes care of the schema changes but we also want to pre-populate the **Abstract** column using the first 100 characters of content for each post.</span></span> <span data-ttu-id="f0a64-191">Bunu, SQL'e bırakarak ve sütun eklendikten sonra bir **UPDATE** deyimi çalıştırarak yapabiliriz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-191">We can do this by dropping down to SQL and running an **UPDATE** statement after the column is added.</span></span>
    <span data-ttu-id="f0a64-192">(Aşağıdaki kodda 12. satıra ekleme)</span><span class="sxs-lookup"><span data-stu-id="f0a64-192">(Adding in line 12 in the code below)</span></span>

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

<span data-ttu-id="f0a64-193">Düzenlenen geçişimiz iyi görünüyor, bu nedenle veritabanını güncel duruma getirmek için **Update-Database'i** kullanalım.</span><span class="sxs-lookup"><span data-stu-id="f0a64-193">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="f0a64-194">SQL'in veritabanına karşı çalıştırıldığını görebilmemiz için **Verbose** bayrağını belirtiriz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-194">We’ll specify the **–Verbose** flag so that we can see the SQL being run against the database.</span></span>

-   <span data-ttu-id="f0a64-195">Paket Yöneticisi Konsolunda **Update-Database –Verbose** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f0a64-195">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="migrate-to-a-specific-version-including-downgrade"></a><span data-ttu-id="f0a64-196">Belirli Bir Sürüme Geçiş (Düşürme Dahil)</span><span class="sxs-lookup"><span data-stu-id="f0a64-196">Migrate to a Specific Version (Including Downgrade)</span></span>

<span data-ttu-id="f0a64-197">Şimdiye kadar her zaman en son geçiş yükselttik, ancak yükseltme / belirli bir geçiş için downgrade istediğiniz zamanlar olabilir.</span><span class="sxs-lookup"><span data-stu-id="f0a64-197">So far we have always upgraded to the latest migration, but there may be times when you want upgrade/downgrade to a specific migration.</span></span>

<span data-ttu-id="f0a64-198">Veritabanımızı **AddBlogUrl** geçişimizi çalıştırdıktan sonra içinde olduğu duruma geçirmek istediğimizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="f0a64-198">Let’s say we want to migrate our database to the state it was in after running our **AddBlogUrl** migration.</span></span> <span data-ttu-id="f0a64-199">Bu geçişe indirgenmek için **–TargetMigration** anahtarını kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-199">We can use the **–TargetMigration** switch to downgrade to this migration.</span></span>

-   <span data-ttu-id="f0a64-200">**Update-Database çalıştırın –TargetMigration: AddBlogUrl** komutu Package Manager Console'da.</span><span class="sxs-lookup"><span data-stu-id="f0a64-200">Run the **Update-Database –TargetMigration: AddBlogUrl** command in Package Manager Console.</span></span>

<span data-ttu-id="f0a64-201">Bu **komut, AddBlogAbstract** ve **AddPostClass** geçişlerimiz için Aşağı komut dosyasını çalıştıracaktır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-201">This command will run the Down script for our **AddBlogAbstract** and **AddPostClass** migrations.</span></span>

<span data-ttu-id="f0a64-202">Boş bir veritabanına geri dönmek **istiyorsanız, Update-Database -TargetMigration: $InitialDatabase** komutunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-202">If you want to roll all the way back to an empty database then you can use the **Update-Database –TargetMigration: $InitialDatabase** command.</span></span>

## <a name="getting-a-sql-script"></a><span data-ttu-id="f0a64-203">SQL Script alma</span><span class="sxs-lookup"><span data-stu-id="f0a64-203">Getting a SQL Script</span></span>

<span data-ttu-id="f0a64-204">Başka bir geliştirici bu değişiklikleri makinelerinde isterse, biz değişikliklerimizi kaynak denetimine kontrol ettiğimizde senkronize edebilirler.</span><span class="sxs-lookup"><span data-stu-id="f0a64-204">If another developer wants these changes on their machine they can just sync once we check our changes into source control.</span></span> <span data-ttu-id="f0a64-205">Yeni geçişlerimizi yaptıktan sonra değişiklikleri yerel olarak çalıştırmak için Update-Database komutunu çalıştırabilirler.</span><span class="sxs-lookup"><span data-stu-id="f0a64-205">Once they have our new migrations they can just run the Update-Database command to have the changes applied locally.</span></span> <span data-ttu-id="f0a64-206">Ancak bu değişiklikleri bir test sunucusuna itmek ve sonunda üretim yapmak istersek, muhtemelen DBA'mıza teslim edebileceğimiz bir SQL komut dosyası isteriz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-206">However if we want to push these changes out to a test server, and eventually production, we probably want a SQL script we can hand off to our DBA.</span></span>

-   <span data-ttu-id="f0a64-207">**Update-Database** komutunu çalıştırın, ancak bu kez değişikliklerin uygulanmak yerine bir komut dosyasına yazılması için **-Script** bayrağını belirtin.</span><span class="sxs-lookup"><span data-stu-id="f0a64-207">Run the **Update-Database** command but this time specify the **–Script** flag so that changes are written to a script rather than applied.</span></span> <span data-ttu-id="f0a64-208">Komut dosyasını oluşturmak için bir kaynak ve hedef geçişi de belirteceğiz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-208">We’ll also specify a source and target migration to generate the script for.</span></span> <span data-ttu-id="f0a64-209">Boş bir veritabanından **($InitialDatabase)** en son sürüme (geçiş **AddPostAbstract)** gitmek için bir komut dosyası istiyorum.</span><span class="sxs-lookup"><span data-stu-id="f0a64-209">We want a script to go from an empty database (**$InitialDatabase**) to the latest version (migration **AddPostAbstract**).</span></span>
    <span data-ttu-id="f0a64-210">*Bir hedef geçişi belirtmezseniz, Geçişler hedef olarak en son geçişi kullanır. Kaynak geçişleri belirtmezseniz, Geçişler veritabanının geçerli durumunu kullanır.*</span><span class="sxs-lookup"><span data-stu-id="f0a64-210">*If you don’t specify a target migration, Migrations will use the latest migration as the target. If you don't specify a source migrations, Migrations will use the current state of the database.*</span></span>
-   <span data-ttu-id="f0a64-211">**Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** komutunu Paket Yöneticisi Konsolunda çalıştırın</span><span class="sxs-lookup"><span data-stu-id="f0a64-211">Run the **Update-Database -Script -SourceMigration: $InitialDatabase -TargetMigration: AddPostAbstract** command in Package Manager Console</span></span>

<span data-ttu-id="f0a64-212">Kod İlk Geçişler geçiş ardışık çalışır, ancak değişiklikleri gerçekten uygulamak yerine bunları sizin için bir .sql dosyasına yazar.</span><span class="sxs-lookup"><span data-stu-id="f0a64-212">Code First Migrations will run the migration pipeline but instead of actually applying the changes it will write them out to a .sql file for you.</span></span> <span data-ttu-id="f0a64-213">Komut dosyası oluşturulduktan sonra, visual studio'da sizin için açılır ve görüntülemeniz veya kaydetmeniz için hazır hale gelir.</span><span class="sxs-lookup"><span data-stu-id="f0a64-213">Once the script is generated, it is opened for you in Visual Studio, ready for you to view or save.</span></span>

### <a name="generating-idempotent-scripts"></a><span data-ttu-id="f0a64-214">İktidarlı Komut Dosyaları Oluşturma</span><span class="sxs-lookup"><span data-stu-id="f0a64-214">Generating Idempotent Scripts</span></span>

<span data-ttu-id="f0a64-215">EF6 ile başlayarak, belirtirseniz **–SourceMigration $InitialDatabase** sonra oluşturulan komut dosyası 'idempotent' olacaktır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-215">Starting with EF6, if you specify **–SourceMigration $InitialDatabase** then the generated script will be ‘idempotent’.</span></span> <span data-ttu-id="f0a64-216">Idempotent komut dosyaları en son sürüme herhangi bir sürümü şu anda bir veritabanı yükseltebilirsiniz (veya kullanırsanız belirtilen sürümü **–TargetMigration).**</span><span class="sxs-lookup"><span data-stu-id="f0a64-216">Idempotent scripts can upgrade a database currently at any version to the latest version (or the specified version if you use **–TargetMigration**).</span></span> <span data-ttu-id="f0a64-217">Oluşturulan komut \*\* \_ \_dosyası, MigrationsHistory\*\* tablosunu denetlemek ve yalnızca daha önce uygulanmamış değişiklikleri uygulamak için mantık içerir.</span><span class="sxs-lookup"><span data-stu-id="f0a64-217">The generated script includes logic to check the **\_\_MigrationsHistory** table and only apply changes that haven't been previously applied.</span></span>

## <a name="automatically-upgrading-on-application-startup-migratedatabasetolatestversion-initializer"></a><span data-ttu-id="f0a64-218">Uygulama Başlatmada Otomatik Yükseltme (MigrateDatabaseToLatestVersion Initializer)</span><span class="sxs-lookup"><span data-stu-id="f0a64-218">Automatically Upgrading on Application Startup (MigrateDatabaseToLatestVersion Initializer)</span></span>

<span data-ttu-id="f0a64-219">Uygulamanızı dağıtıyorsanız, uygulama başlatıldığında veritabanını otomatik olarak yükseltmesini (bekleyen geçişleri uygulayarak) isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-219">If you are deploying your application you may want it to automatically upgrade the database (by applying any pending migrations) when the application launches.</span></span> <span data-ttu-id="f0a64-220">Bunu, **GeçirveriToLatestVersion** veritabanı baş harflerini kaydederek yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-220">You can do this by registering the **MigrateDatabaseToLatestVersion** database initializer.</span></span> <span data-ttu-id="f0a64-221">Veritabanı baş harflerini yalnızca veritabanının doğru şekilde kurulum olduğundan emin olmak için kullanılan bazı mantık içerir.</span><span class="sxs-lookup"><span data-stu-id="f0a64-221">A database initializer simply contains some logic that is used to make sure the database is setup correctly.</span></span> <span data-ttu-id="f0a64-222">Bu mantık, bağlam ın uygulama işlemi **(AppDomain)** içinde ilk kez kullanılmasıyla çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-222">This logic is run the first time the context is used within the application process (**AppDomain**).</span></span>

<span data-ttu-id="f0a64-223">Bağlamı (Satır 14) kullanmadan önce BlogContext için **GeçirVeritabanıToLatestVersion** başlatlayıcısını ayarlamak için **Program.cs** dosyasını güncelleştirebiliriz.</span><span class="sxs-lookup"><span data-stu-id="f0a64-223">We can update the **Program.cs** file, as shown below, to set the **MigrateDatabaseToLatestVersion** initializer for BlogContext before we use the context (Line 14).</span></span> <span data-ttu-id="f0a64-224">**System.Data.Entity** ad alanı (Satır 5) için bir dekullanarak ifade eklemeniz gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="f0a64-224">Note that you also need to add a using statement for the **System.Data.Entity** namespace (Line 5).</span></span>

<span data-ttu-id="f0a64-225">*Bu başlatanın bir örneğini oluşturduğumuzda bağlam türünü **(BlogContext)** ve geçişyapılandırmasını **(Yapılandırma)** belirtmemiz gerekir - Geçişler yapılandırması, Geçişleri etkinleştirdiğimizde **Geçişler** klasörümüze eklenen sınıftır.*</span><span class="sxs-lookup"><span data-stu-id="f0a64-225">*When we create an instance of this initializer we need to specify the context type (**BlogContext**) and the migrations configuration (**Configuration**) - the migrations configuration is the class that got added to our **Migrations** folder when we enabled Migrations.*</span></span>

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

<span data-ttu-id="f0a64-226">Şimdi uygulamamız çalıştığında, ilk olarak hedeflenen veritabanının güncel olup olmadığını denetleyecek ve değilse bekleyen geçişleri uygulayacaktır.</span><span class="sxs-lookup"><span data-stu-id="f0a64-226">Now whenever our application runs it will first check if the database it is targeting is up-to-date, and apply any pending migrations if it is not.</span></span>
