---
title: Otomatik Code First geçişleri - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: f358a4df04b03399e9e54ffdf0389e43d715af1c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996101"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="eb58e-102">Otomatik Code First geçişleri</span><span class="sxs-lookup"><span data-stu-id="eb58e-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="eb58e-103">Otomatik geçişleri yaptığınız her değişiklik için bir kod dosyası projenize zorunda kalmadan Code First Migrations kullanmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="eb58e-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="eb58e-104">Tüm değişiklikler otomatik olarak uygulanabilir; örneğin sütun yeniden adlandırma kod tabanlı bir geçiş kullanılmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="eb58e-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="eb58e-105">Bu makalede, temel senaryolarda Code First Migrations nasıl kullanılacağını varsayar.</span><span class="sxs-lookup"><span data-stu-id="eb58e-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="eb58e-106">Sizin sonra okumak ihtiyacınız olacak [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="eb58e-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="eb58e-107">Takım ortamları için öneri</span><span class="sxs-lookup"><span data-stu-id="eb58e-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="eb58e-108">Otomatik ve kod tabanlı geçişler aralarına koyabilirsiniz ancak bu takım geliştirme senaryolarda önerilmez.</span><span class="sxs-lookup"><span data-stu-id="eb58e-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="eb58e-109">Kaynak denetimi kullanan geliştiriciler bir ekibin parçası olduğunda, tamamen otomatik geçişleri veya yalnızca kod tabanlı geçişler ya da kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="eb58e-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="eb58e-110">Otomatik geçişleri sınırlamaları kod tabanlı geçişler ekip ortamlarında kullanılmasını öneririz.</span><span class="sxs-lookup"><span data-stu-id="eb58e-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="eb58e-111">Bir ilk Model & veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="eb58e-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="eb58e-112">Biz geçişleri kullanmaya başlamadan önce bir proje ve çalışmak için Code First modeli gerekir.</span><span class="sxs-lookup"><span data-stu-id="eb58e-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="eb58e-113">Bu kılavuz için kurallı kullanacağız **Blog** ve **Post** modeli.</span><span class="sxs-lookup"><span data-stu-id="eb58e-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="eb58e-114">Yeni bir **MigrationsAutomaticDemo** konsol uygulaması</span><span class="sxs-lookup"><span data-stu-id="eb58e-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="eb58e-115">En son sürümünü eklemek **EntityFramework** projeye NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="eb58e-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="eb58e-116">**Araçları –&gt; kitaplık Paket Yöneticisi –&gt; Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="eb58e-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="eb58e-117">Çalıştırma **Install-Package EntityFramework** komutu</span><span class="sxs-lookup"><span data-stu-id="eb58e-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="eb58e-118">Ekleme bir **Model.cs** aşağıda gösterilen kod dosyası.</span><span class="sxs-lookup"><span data-stu-id="eb58e-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="eb58e-119">Bu kod, tek bir tanımlar **Blog** etki alanı modelimizi sağlar sınıfını ve **BlogContext** bizim EF Code First bağlam sınıfı</span><span class="sxs-lookup"><span data-stu-id="eb58e-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

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

-   <span data-ttu-id="eb58e-120">Biz bir modeliniz olduğuna göre veri erişimi gerçekleştirdiği kullanma zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="eb58e-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="eb58e-121">Güncelleştirme **Program.cs** aşağıda gösterilen kod dosyası.</span><span class="sxs-lookup"><span data-stu-id="eb58e-121">Update the **Program.cs** file with the code shown below.</span></span>

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

-   <span data-ttu-id="eb58e-122">Uygulamanızı çalıştırın ve göreceksiniz bir **MigrationsAutomaticCodeDemo.BlogContext** veritabanı sizin için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="eb58e-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="eb58e-124">Geçiş etkinleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="eb58e-124">Enabling Migrations</span></span>

<span data-ttu-id="eb58e-125">Bu, bazı modelimiz için daha fazla değişiklik zamanı geldi.</span><span class="sxs-lookup"><span data-stu-id="eb58e-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="eb58e-126">Şimdi Blog sınıfına URL'si özelliği sunar.</span><span class="sxs-lookup"><span data-stu-id="eb58e-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="eb58e-127">Uygulamayı çalıştırmak için olsaydı yeniden belirten bir InvalidOperationException elde edebileceğiniz *veritabanı oluşturulduktan sonra 'BlogContext' bağlam yedekleme modeli değişti. Veritabanını güncellemek için Code First Migrations'ı kullanmayı deneyin (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](http://go.microsoft.com/fwlink/?LinkId=238269) *).*</span><span class="sxs-lookup"><span data-stu-id="eb58e-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="eb58e-128">Özel durum da anlaşılacağı gibi Code First Migrations'ı kullanmaya başlamak için zaman var.</span><span class="sxs-lookup"><span data-stu-id="eb58e-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="eb58e-129">Otomatik geçişleri kullanmak istediğimizden bunu belirtmek için dağıtacağız **– EnableAutomaticMigrations** geçin.</span><span class="sxs-lookup"><span data-stu-id="eb58e-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="eb58e-130">Çalıştırma **etkinleştir geçişleri – EnableAutomaticMigrations** Paket Yöneticisi konsolu bu komutu bir komutta ekledi bir **geçişler** Projemizin klasörüne.</span><span class="sxs-lookup"><span data-stu-id="eb58e-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="eb58e-131">Bu yeni klasöre bir dosya içerir:</span><span class="sxs-lookup"><span data-stu-id="eb58e-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="eb58e-132">**Yapılandırma sınıfı.**</span><span class="sxs-lookup"><span data-stu-id="eb58e-132">**The Configuration class.**</span></span> <span data-ttu-id="eb58e-133">Bu sınıf geçişleri içeriğiniz için nasıl davranacağını yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="eb58e-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="eb58e-134">Bu kılavuz için yalnızca varsayılan yapılandırmayı kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="eb58e-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="eb58e-135">*Projenizde yalnızca tek bir Code First bağlamı olmadığından, geçişleri etkinleştir otomatik olarak doldurulur Bu yapılandırmanın geçerli bağlam türü.*</span><span class="sxs-lookup"><span data-stu-id="eb58e-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="eb58e-136">İlk otomatik geçiş</span><span class="sxs-lookup"><span data-stu-id="eb58e-136">Your First Automatic Migration</span></span>

<span data-ttu-id="eb58e-137">Code First geçişleri sahibi olacak iki birincil komutu vardır.</span><span class="sxs-lookup"><span data-stu-id="eb58e-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="eb58e-138">**Geçiş** son geçiş oluşturulmasından bu yana modelinize yaptığınız değişikliklere dayalı sonraki geçiş iskelesini</span><span class="sxs-lookup"><span data-stu-id="eb58e-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="eb58e-139">**Veritabanını Güncelleştir** geçişler bekleyen herhangi bir veritabanına uygulanır</span><span class="sxs-lookup"><span data-stu-id="eb58e-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="eb58e-140">Önlemek kullanacağız geçiş Ekle (biz aslında gerekmedikçe) kullanarak ve Code First Migrations'ı otomatik olarak izin vererek odaklanmasına hesaplamak ve değişiklikleri uygulayın.</span><span class="sxs-lookup"><span data-stu-id="eb58e-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="eb58e-141">Kullanalım **veritabanını Güncelleştir** modelimiz için değişiklikleri göndermek için Code First Migrations'ı almak için (yeni **Blog.Ur**l özelliği) veritabanı.</span><span class="sxs-lookup"><span data-stu-id="eb58e-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="eb58e-142">Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="eb58e-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="eb58e-143">**MigrationsAutomaticDemo.BlogContext** veritabanı içerecek şekilde güncelleştirilmiş artık **Url** sütununda **blogları** tablo.</span><span class="sxs-lookup"><span data-stu-id="eb58e-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="eb58e-144">İkinci otomatik geçiş</span><span class="sxs-lookup"><span data-stu-id="eb58e-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="eb58e-145">Başka bir değiştirme ve otomatik olarak değişiklikleri veritabanına bizim için anında iletme Code First Migrations izin olalım.</span><span class="sxs-lookup"><span data-stu-id="eb58e-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="eb58e-146">Ayrıca yeni bir ekleyelim **Post** sınıfı</span><span class="sxs-lookup"><span data-stu-id="eb58e-146">Let's also add a new **Post** class</span></span>

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

-   <span data-ttu-id="eb58e-147">Ayrıca ekleyeceğiz bir **gönderileri** koleksiyona **Blog** arasındaki ilişkinin diğer ucundaki formu için sınıf **Blog** ve **sonrası**</span><span class="sxs-lookup"><span data-stu-id="eb58e-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="eb58e-148">Artık **veritabanını Güncelleştir** güncel veritabanı getirilecek.</span><span class="sxs-lookup"><span data-stu-id="eb58e-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="eb58e-149">Bu süre belirtelim **– ayrıntılı** Code First Migrations'ı çalıştıran SQL görebilmeniz için bayrak.</span><span class="sxs-lookup"><span data-stu-id="eb58e-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="eb58e-150">Çalıştırma **veritabanını güncelleştir – ayrıntılı** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="eb58e-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="eb58e-151">Geçiş dayalı bir kod ekleme</span><span class="sxs-lookup"><span data-stu-id="eb58e-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="eb58e-152">Artık bir şey kod tabanlı bir geçiş için kullanılacak istiyoruz göz atalım.</span><span class="sxs-lookup"><span data-stu-id="eb58e-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="eb58e-153">Ekleyelim bir **derecelendirme** özelliğini **Blog** sınıfı</span><span class="sxs-lookup"><span data-stu-id="eb58e-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="eb58e-154">Biz yalnızca çalıştırabileceğiniz **veritabanını Güncelleştir** bu değişiklikleri veritabanına göndermek için.</span><span class="sxs-lookup"><span data-stu-id="eb58e-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="eb58e-155">Ancak, bir NULL olmayan ekliyoruz **Blogs.Rating** sütun, tablodaki tüm mevcut veriler varsa, yeni bir sütun için veri türünün CLR varsayılan atanır (derecelendirmesi, tamsayı, olacaktır **0**).</span><span class="sxs-lookup"><span data-stu-id="eb58e-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="eb58e-156">Ancak varsayılan değerini belirtmek istediğimiz **3** içinde varolan satırları şekilde **blogları** tablo makul bir derecelendirme ile başlar.</span><span class="sxs-lookup"><span data-stu-id="eb58e-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="eb58e-157">Şimdi biz düzenleyebilmek için kod tabanlı bir geçiş out bu değişikliği yazmak için geçiş Ekle komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="eb58e-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="eb58e-158">**Ekle geçiş** komutu bize bu geçişleri bir ad verin, yalnızca bizim adlandıralım verir **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="eb58e-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="eb58e-159">Çalıştırma **Ekle geçiş AddBlogRating** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="eb58e-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="eb58e-160">İçinde **geçişler** klasör artık sahibiz yeni **AddBlogRating** geçiş.</span><span class="sxs-lookup"><span data-stu-id="eb58e-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="eb58e-161">Geçiş dosya zaman damgası ile sıralama ile yardımcı olmak için önceden sabit.</span><span class="sxs-lookup"><span data-stu-id="eb58e-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="eb58e-162">Bir varsayılan değer 3'ün Blog.Rating (aşağıdaki kod satırını 10) belirtmek için oluşturulan kodu düzenleyelim</span><span class="sxs-lookup"><span data-stu-id="eb58e-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="eb58e-163">*Geçiş de bazı meta veriler yakalanır bir arka plan kod dosyası vardır. Bu meta veriler, bu kod tabanlı geçiş öncesinde gerçekleştirdiğimiz otomatik geçişlerin çoğaltmak Code First Migrations izin verir. Bu, bizim geçişlerini çalıştırmak başka bir geliştirici istiyorsa, ya da bunu uygulamamız dağıtma zamanı geldiğinde önemlidir.*</span><span class="sxs-lookup"><span data-stu-id="eb58e-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

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

<span data-ttu-id="eb58e-164">Bizim düzenlenen geçiş iyi görünüyor, böylece kullanalım **veritabanını Güncelleştir** güncel veritabanı getirilecek.</span><span class="sxs-lookup"><span data-stu-id="eb58e-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="eb58e-165">Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="eb58e-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="eb58e-166">Otomatik geçişleri</span><span class="sxs-lookup"><span data-stu-id="eb58e-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="eb58e-167">Otomatik geçişler için yaptığımız basit değişiklikleri geri dönmek ücretsiz sunmaktayız.</span><span class="sxs-lookup"><span data-stu-id="eb58e-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="eb58e-168">Code First geçişleri her kod tabanlı bir geçiş için arka plan kod dosyasında depolamak meta verileri doğru sırada otomatik ve kod tabanlı geçişler gerçekleştirerek ilgileniriz.</span><span class="sxs-lookup"><span data-stu-id="eb58e-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="eb58e-169">Modelimiz için Post.Abstract özelliği ekleyelim</span><span class="sxs-lookup"><span data-stu-id="eb58e-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="eb58e-170">Kullanabiliriz artık **veritabanını Güncelleştir** bu değişikliği otomatik geçişini kullanarak veritabanına göndermek için Code First Migrations'ı almak için.</span><span class="sxs-lookup"><span data-stu-id="eb58e-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="eb58e-171">Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu.</span><span class="sxs-lookup"><span data-stu-id="eb58e-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="eb58e-172">Özet</span><span class="sxs-lookup"><span data-stu-id="eb58e-172">Summary</span></span>

<span data-ttu-id="eb58e-173">Bu izlenecek yolda göndermeyi otomatik geçişleri kullanmayı öğrendiniz modeli veritabanını değiştirir.</span><span class="sxs-lookup"><span data-stu-id="eb58e-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="eb58e-174">Ayrıca, iskele ve daha fazla denetime ihtiyacınız olduğunda otomatik geçişleri arasında kod tabanlı geçişler çalıştırma de gördünüz.</span><span class="sxs-lookup"><span data-stu-id="eb58e-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
