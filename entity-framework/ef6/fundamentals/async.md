---
title: Zaman uyumsuz sorgulama ve Kaydet - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: de702365251fd05c423c8590ccaefa7d8542ad02
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058766"
---
# <a name="async-query-and-save"></a>Zaman uyumsuz sorgulama ve kaydedin
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

EF6 kullanarak kaydetmek ve zaman uyumsuz sorgu için destek sunmuştur [async ve await anahtar sözcükleri](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) .NET 4. 5 ' sunulmuştur. Tüm uygulamalar için faaliyetler avantajlı olabilir ancak uzun süre çalışan, ağ veya miyim/O-bağlı görevler ele alırken istemci yanıt verme becerisini ve sunucu ölçeklenebilirliğini geliştirmek için kullanılabilir.

## <a name="when-to-really-use-async"></a>Zaman uyumsuz gerçekten kullanan

Bu kılavuzun amacı, zaman uyumsuz ve zaman uyumlu programın yürütülmesi arasındaki farkı görmek kolay bir şekilde zaman uyumsuz kavramları tanıtan sağlamaktır. Bu izlenecek yol herhangi birini senaryoları göstermek için zaman uyumsuz programlama avantajları burada tasarlanmamıştır.

Zaman uyumsuz programlama öncelikle herhangi bir işlem süresi gerektirmeyen bir işlem için beklediği sırada başka işleri yapmak için geçerli yönetilen iş parçacığı (iş parçacığı çalışan .NET kodu) boşaltma üzerinde yönetilen bir iş parçacığından odaklanır. Örneğin, veritabanı motoru sorgu işlenirken bir şey yok .NET kodu tarafından yapılacak.

İstemci uygulamalarında (WinForms, WPF vb.), geçerli iş parçacığı UI esnek, zaman uyumsuz işlem gerçekleştirildiği sırada tutmak için kullanılabilir. Diğer gelen istekleri - iş parçacığı kullanılabilir sunucu uygulamalarında (ASP.NET, vb.) bu bellek kullanımını azaltmak ve/veya sunucusunun aktarım hızını artırın.

Zaman uyumsuz kullanarak çoğu uygulamada belirgin faydalı olacaktır ve hatta zarar. Testler, profil oluşturma ve sağduyunuzu yapmadan önce zaman uyumsuz belirli senaryonuza etkisini ölçmek için kullanın.

Zaman uyumsuz hakkında bilgi edinmek için daha fazla bazı kaynaklar aşağıda verilmiştir:

-   [.NET 4.5 içinde async/await Brandon Bray'nın genel bakış](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Zaman uyumsuz programlama](https://msdn.microsoft.com/library/hh191443.aspx) MSDN Kitaplığı'nda sayfaları
-   [ASP.NET Web uygulamaları kullanarak Async yapı nasıl](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (artan sunucusu verimliliği gösterimini içerir)

## <a name="create-the-model"></a>Modeli oluşturma

Kullanacağız [kod ilk iş akışınızı](~/ef6/modeling/code-first/workflows/new-database.md) modelimizi oluşturun ve zaman uyumsuz işlevleri ile EF Designer oluşturulanlar dahil olmak üzere tüm EF modelleri ile çalışır ancak veritabanı oluşturmak için.

-   Bir konsol uygulaması oluşturun ve adlandırın **AsyncDemo**
-   EntityFramework NuGet paketi ekleme
    -   Çözüm Gezgini'nde sağ **AsyncDemo** proje
    -   Seçin **NuGet paketlerini Yönet...**
    -   NuGet paketlerini Yönet iletişim kutusunda, seçmek **çevrimiçi** sekmesini **EntityFramework** paket
    -   Tıklayın **yükleyin**
-   Ekleme bir **Model.cs** aşağıdaki uygulama ile sınıfı

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
        }

        public class Blog
        {
            public int BlogId { get; set; }
            public string Name { get; set; }

            public virtual List<Post> Posts { get; set; }
        }

        public class Post
        {
            public int PostId { get; set; }
            public string Title { get; set; }
            public string Content { get; set; }

            public int BlogId { get; set; }
            public virtual Blog Blog { get; set; }
        }
    }
```

 

## <a name="create-a-synchronous-program"></a>Zaman uyumlu bir program oluşturma

EF modeli sahibiz, bazı veri erişimi gerçekleştirdiği kullanan biraz kod yazalım.

-   Öğesinin içeriğini değiştirin **Program.cs** aşağıdaki kodla

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

Bu kod **PerformDatabaseOperations** yeni kaydedileceği yöntemi **Blog** veritabanına ve ardından alır tüm **blogları** veritabanından ve bunlara yazdırır **Konsol**. Bundan sonra programı için günün bir teklif Yazar **konsol**.

Kod zaman uyumlu olduğundan, biz programını çalıştırdığınızda, biz aşağıdaki yürütme akış görebilirsiniz:

1.  **SaveChanges** yeni göndermeye başlar **Blog** veritabanı
2.  **SaveChanges** tamamlar
3.  Tüm sorgu **blogları** veritabanına gönderilir
4.  Sorgu döndürür ve sonuçları yazılır **Konsolu**
5.  Günün yazılır **Konsolu**

![Eşitleme çıkışı](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Zaman uyumsuz hale getirme

Programımız çalıştırmaya sahibiz, biz yeni async ve await anahtar sözcükleri yapmadan başlayabilirsiniz. Program.cs'ye aşağıdaki değişiklikleri yaptık

1.  2. satır: Using deyimi için **System.Data.Entity** ad alanı sağladığı erişim için EF zaman uyumsuz genişletme yöntemleri.
2.  4. satırı: Using deyimi için **System.Threading.Tasks** ad alanı kullanılacak bize sağlar **görev** türü.
3.  12 & 18 satırı: Biz ilerleme durumunu izleyen Görev yakalama **PerformSomeDatabaseOperations** (satır, 12) ve sonra bu program yürütme engelleme görev tam bir kez tüm işler için program (18. satır) gerçekleştirilir.
4.  25 satırı: Güncelleştirme yaptıklarımız **PerformSomeDatabaseOperations** olarak işaretlenecek **zaman uyumsuz** ve dönüş bir **görev**.
5.  35 satırı: Biz artık SaveChanges zaman uyumsuz sürümü çağırma ve onun tamamlanmasını bekleme.
6.  Satırı 42: Biz, artık ToList ve sonucunu bekleyen zaman uyumsuz sürümü numarasını arıyoruz.

System.Data.Entity ad alanında kullanılabilir uzantı yöntemlerini kapsamlı bir listesi için QueryableExtensions sınıfına bakın. *Ayrıca "System.Data.Entity kullanma" eklemek kullanarak, gerekir deyimleri.*

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

Kod uyumsuz olduğuna göre biz programını çalıştırdığınızda, biz farklı yürütme akış görebilirsiniz:

1.  **SaveChanges** yeni göndermeye başlar **Blog** veritabanına *komutu artık işlem saati geçerli bir yönetilen iş parçacığı üzerinde gerekli veritabanı gönderildikten sonra. **PerformDatabaseOperations** yöntemi döndürür (Bu yürütme işlemi tamamlanmadı olsa bile) ve ana yöntem program akışında devam eder.*
2.  **Günün konsoluna yazılan**
    *Main yöntemine yapmak için daha fazla iş olduğundan, yönetilen iş parçacığı bekleme engellenir veritabanı işlemi tamamlanana kadar çağırın. İşlem tamamlandıktan sonra geri kalanında bizim **PerformDatabaseOperations** * yürütülür.
3.  **SaveChanges** tamamlar
4.  Tüm sorgu **blogları** veritabanına gönderilen *yeniden yönetilen iş parçacığı veritabanında sorgu işlenirken başka işleri yapmak ücretsizdir. Diğer tüm yürütme tamamlandı olduğundan, iş parçacığı yalnızca bekleme çağrıda ancak durdurulur.*
5.  Sorgu döndürür ve sonuçları yazılır **Konsolu**

![Zaman uyumsuz çıkış](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Takeaway

Artık hale getirmek için ne kadar kolay olduğunu gördük EF'ın zaman uyumsuz yöntemleri kullanın. Zaman uyumsuz avantajları ile basit bir konsol uygulaması oldukça belirgin olmayabilir rağmen bu aynı stratejiler burada uzun süreli veya ağa bağlı etkinlik Aksi takdirde uygulama önleyebilir, veya çok sayıda iş parçacığı neden durumlarda uygulanabilir bellek Ayak izi artırın.
