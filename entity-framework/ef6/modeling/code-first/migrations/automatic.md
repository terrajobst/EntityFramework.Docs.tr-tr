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
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="7ad42-102">Otomatik Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="7ad42-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="7ad42-103">Otomatik geçişler, yaptığınız her değişiklik için projenizde kod dosyası olmadan Code First Migrations kullanmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="7ad42-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="7ad42-104">Tüm değişiklikler otomatik olarak uygulanmayabilir; Örneğin, sütun yeniden adlandırmaları kod tabanlı geçişin kullanılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="7ad42-105">Bu makalede temel senaryolarda Code First Migrations kullanmayı bildiğiniz varsayılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="7ad42-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="7ad42-106">Bunu yapmazsanız devam etmeden önce [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) okumanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="7ad42-107">Ekip ortamları için öneri</span><span class="sxs-lookup"><span data-stu-id="7ad42-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="7ad42-108">Otomatik ve kod tabanlı geçişleri birbirine dönüştürebilirsiniz, ancak bu, takım geliştirme senaryolarında önerilmez.</span><span class="sxs-lookup"><span data-stu-id="7ad42-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="7ad42-109">Kaynak denetimini kullanan bir geliştirici ekibinin parçasıysa, yalnızca otomatik geçişleri veya yalnızca kod tabanlı geçişleri kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="7ad42-110">Otomatik geçişlerin sınırlamaları verildiğinde, takım ortamlarında kod tabanlı geçişlerde kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="7ad42-111">Ilk model & veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad42-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="7ad42-112">Geçişleri kullanmaya başlamadan önce bir proje ve ile çalışmak için bir Code First modeli gerekir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="7ad42-113">Bu izlenecek yol için kurallı **Blog** ve **gönderi** modelini kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="7ad42-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="7ad42-114">Yeni bir **Migrationsautomaticdemo** konsol uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad42-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="7ad42-115">Projeye **EntityFramework** NuGet paketinin en son sürümünü ekleyin</span><span class="sxs-lookup"><span data-stu-id="7ad42-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="7ad42-116">**Araçlar –&gt; kitaplığı Paket Yöneticisi –&gt; Paket Yöneticisi konsolu**</span><span class="sxs-lookup"><span data-stu-id="7ad42-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="7ad42-117">**Install-Package EntityFramework** komutunu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="7ad42-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="7ad42-118">Aşağıda gösterilen kodla bir **model.cs** dosyası ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ad42-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="7ad42-119">Bu kod, etki alanı modelimizi ve EF Code First bağlamımız bir **BlogContext** sınıfını oluşturan tek bir **Blog** sınıfını tanımlar</span><span class="sxs-lookup"><span data-stu-id="7ad42-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="7ad42-120">Artık bir modelimiz olduğuna göre, veri erişimi gerçekleştirmek için bunu kullanmanın zamanı.</span><span class="sxs-lookup"><span data-stu-id="7ad42-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="7ad42-121">**Program.cs** dosyasını aşağıda gösterilen kodla güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7ad42-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="7ad42-122">Uygulamanızı çalıştırın ve sizin için bir **Migrationsautomaticcodedemo. BlogContext** veritabanının oluşturulduğunu görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7ad42-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![Veritabanı Yereldb](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="7ad42-124">Geçişleri etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="7ad42-124">Enabling Migrations</span></span>

<span data-ttu-id="7ad42-125">Modelinizde daha fazla değişiklik yapmak zaman alabilir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="7ad42-126">Blog sınıfına bir URL özelliği tanıtalım.</span><span class="sxs-lookup"><span data-stu-id="7ad42-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="7ad42-127">Uygulamayı yeniden çalıştırırsanız, *veritabanı oluşturulduktan sonra ' BlogContext ' bağlamının yedeklendiğini belirten bir InvalidOperationException alacaksınız. Veritabanını güncelleştirmek için Code First Migrations kullanmayı düşünün (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="7ad42-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](https://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="7ad42-128">Özel durum önerdiğinde Code First Migrations kullanmaya başlama zamanı.</span><span class="sxs-lookup"><span data-stu-id="7ad42-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="7ad42-129">Otomatik geçişleri kullanmak istiyoruz, **– Enableautomaticgeçişler** anahtarını belirteceğiz.</span><span class="sxs-lookup"><span data-stu-id="7ad42-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="7ad42-130">Paket Yöneticisi konsolundaki **Enable-geçişleri – Enableautomaticgeçişleri** komutunu çalıştırın bu komut, projemizdeki bir **geçiş** klasörü ekledi.</span><span class="sxs-lookup"><span data-stu-id="7ad42-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="7ad42-131">Bu yeni klasör bir dosya içerir:</span><span class="sxs-lookup"><span data-stu-id="7ad42-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="7ad42-132">**Yapılandırma sınıfı.**</span><span class="sxs-lookup"><span data-stu-id="7ad42-132">**The Configuration class.**</span></span> <span data-ttu-id="7ad42-133">Bu sınıf, bağlam için geçişlerin nasıl davranacağını yapılandırmanıza olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="7ad42-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="7ad42-134">Bu kılavuzda, yalnızca varsayılan yapılandırmayı kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="7ad42-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="7ad42-135">*Projenizde yalnızca tek bir Code First bağlamı olduğundan, Enable-geçişler bu yapılandırmanın uygulandığı bağlam türüne otomatik olarak doldurulur.*</span><span class="sxs-lookup"><span data-stu-id="7ad42-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="7ad42-136">Ilk otomatik geçişiniz</span><span class="sxs-lookup"><span data-stu-id="7ad42-136">Your First Automatic Migration</span></span>

<span data-ttu-id="7ad42-137">Code First Migrations, öğrenecek iki birincil komuta sahiptir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="7ad42-138">**Geçiş geçişi** , son geçişin oluşturulmasından bu yana modelinizde yaptığınız değişikliklere dayalı olarak bir sonraki geçişi kankatalacak</span><span class="sxs-lookup"><span data-stu-id="7ad42-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="7ad42-139">**Güncelleştir-veritabanı** bekleyen geçişleri veritabanına uygular</span><span class="sxs-lookup"><span data-stu-id="7ad42-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="7ad42-140">Ekleme geçişi kullanmaktan kaçının (gerçekten gerekli olmadığı durumlar dışında) ve değişiklikleri Code First Migrations otomatik olarak hesaplayıp uygulamanıza odaklanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="7ad42-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="7ad42-141">Değişiklikleri modelinize (yeni **blog. ur**l özelliği) veritabanına göndermek için Code First Migrations almak üzere **Update-Database** ' i kullanalım.</span><span class="sxs-lookup"><span data-stu-id="7ad42-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="7ad42-142">Package Manager konsolunda **Update-Database** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7ad42-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="7ad42-143">**Migrationsautomaticdemo. BlogContext** veritabanı artık **Bloglar** tablosuna **URL** sütununu içerecek şekilde güncelleştirildi.</span><span class="sxs-lookup"><span data-stu-id="7ad42-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="7ad42-144">Ikinci otomatik geçişiniz</span><span class="sxs-lookup"><span data-stu-id="7ad42-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="7ad42-145">Şimdi başka bir değişiklik yapıp Code First Migrations değişiklikleri otomatik olarak veritabanına itelim.</span><span class="sxs-lookup"><span data-stu-id="7ad42-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="7ad42-146">Ayrıca yeni bir **Post** sınıfı ekleyelim</span><span class="sxs-lookup"><span data-stu-id="7ad42-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="7ad42-147">Ayrıca **, blog ve** **gönderi** arasındaki ilişkinin diğer sonunu oluşturmak Için **Blog** sınıfına bir **gönderi** koleksiyonu ekleyeceğiz</span><span class="sxs-lookup"><span data-stu-id="7ad42-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="7ad42-148">Şimdi veritabanını güncel hale getirmek için **Update-Database** kullanın.</span><span class="sxs-lookup"><span data-stu-id="7ad42-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="7ad42-149">Bu süre, Code First Migrations çalışan SQL 'i görebilmeniz için **– verbose** bayrağını belirtlim.</span><span class="sxs-lookup"><span data-stu-id="7ad42-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="7ad42-150">Package Manager konsolundaki **Update-Database – verbose** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7ad42-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="7ad42-151">Kod tabanlı geçiş ekleme</span><span class="sxs-lookup"><span data-stu-id="7ad42-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="7ad42-152">Şimdi, için kod tabanlı bir geçiş kullanmak isteyebileceğiniz bir şeye bakalım.</span><span class="sxs-lookup"><span data-stu-id="7ad42-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="7ad42-153">**Blog** sınıfına bir **Derecelendirme** özelliği ekleyelim</span><span class="sxs-lookup"><span data-stu-id="7ad42-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="7ad42-154">Bu değişiklikleri veritabanına göndermek için yalnızca **Update-Database** ' i çalıştırabiliriz.</span><span class="sxs-lookup"><span data-stu-id="7ad42-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="7ad42-155">Ancak, tablodaki mevcut veriler varsa, null olamayan bir **blog. derecelendirme** sütunu ekledik. Bu, tabloda yeni bir sütun için VERI türünün clr varsayılan değeri atanır (derecelendirme tamdır, yani **0**olur).</span><span class="sxs-lookup"><span data-stu-id="7ad42-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="7ad42-156">Ancak varsayılan olarak **3** değerini belirtmek istiyoruz, bu sayede **blogların** tablosundaki mevcut satırların bir sıra derecelendirmesi ile başlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="7ad42-157">Bunu düzenleyebilmemiz için, bu değişikliği kod tabanlı bir geçişe yazmak üzere Add-Migration komutunu kullanalım.</span><span class="sxs-lookup"><span data-stu-id="7ad42-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="7ad42-158">**Add-Migration** komutu, bu geçişlere bir ad vermemizi sağlar, şimdi **de bizlere**çağrı yapalım.</span><span class="sxs-lookup"><span data-stu-id="7ad42-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="7ad42-159">Paket Yöneticisi konsolunda **Add-Migration AddBlogRating** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7ad42-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="7ad42-160">**Geçişler** klasöründe **artık yeni bir ek geçiş geçişimiz** var.</span><span class="sxs-lookup"><span data-stu-id="7ad42-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="7ad42-161">Geçiş dosya adı, sıralamaya yardımcı olması için zaman damgasıyla önceden düzeltilir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="7ad42-162">Web günlüğü. derecelendirmesi için varsayılan 3 değerini belirtmek üzere oluşturulan kodu düzenleyelim (aşağıdaki kodda satır 10)</span><span class="sxs-lookup"><span data-stu-id="7ad42-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="7ad42-163">*Geçişin Ayrıca bazı meta verileri yakalayan bir arka plan kod dosyası vardır. Bu meta veriler, Code First Migrations Bu kod tabanlı geçişten önce yaptığımız otomatik geçişleri çoğaltmasına izin verir. Başka bir geliştirici geçişlerimizi çalıştırmak isterse veya uygulamamızı dağıtmaya zaman geldiğinde bu önemlidir.*</span><span class="sxs-lookup"><span data-stu-id="7ad42-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="7ad42-164">Düzenlenmiş geçişimiz iyi arıyor, bu nedenle veritabanını güncel hale getirmek için **Update-Database** ' i kullanalım.</span><span class="sxs-lookup"><span data-stu-id="7ad42-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="7ad42-165">Package Manager konsolunda **Update-Database** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7ad42-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="7ad42-166">Otomatik geçişlere geri dön</span><span class="sxs-lookup"><span data-stu-id="7ad42-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="7ad42-167">Artık daha basit değişikliklerimiz için otomatik geçişlere geçiş yapmak ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="7ad42-168">Code First Migrations, otomatik ve kod tabanlı geçişleri, her kod tabanlı geçiş için arka plan kod dosyasında depoladığını temel alarak doğru sırada gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="7ad42-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="7ad42-169">Modelinize bir post. Abstract özelliği ekleyelim</span><span class="sxs-lookup"><span data-stu-id="7ad42-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="7ad42-170">Şimdi **Update-Database** ' i kullanarak, otomatik geçiş kullanarak bu değişikliği veritabanına göndermek için Code First Migrations edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ad42-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="7ad42-171">Package Manager konsolunda **Update-Database** komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7ad42-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="7ad42-172">Özet</span><span class="sxs-lookup"><span data-stu-id="7ad42-172">Summary</span></span>

<span data-ttu-id="7ad42-173">Bu izlenecek yolda, model değişikliklerini veritabanına göndermek için otomatik geçişleri nasıl kullanacağınızı gördünüz.</span><span class="sxs-lookup"><span data-stu-id="7ad42-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="7ad42-174">Ayrıca, daha fazla denetime ihtiyacınız olduğunda otomatik geçişler arasında kod tabanlı geçişler oluşturmayı ve bunları nasıl çalıştıracağınızı da gördünüz.</span><span class="sxs-lookup"><span data-stu-id="7ad42-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
