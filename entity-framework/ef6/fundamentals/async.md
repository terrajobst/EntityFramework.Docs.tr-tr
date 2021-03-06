---
title: Async sorgusu ve Save-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 0642dc13e7aa3906fa1495031c62701fc16f0192
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417982"
---
# <a name="async-query-and-save"></a>Zaman uyumsuz sorgu ve Kaydet
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

EF6, zaman uyumsuz sorgu için destek sunmuştur ve .NET 4,5 ' de tanıtılan [zaman uyumsuz ve await anahtar sözcüklerini](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) kullanarak kaydeder. Tüm uygulamalar asynchrony 'den faydalanabilir olsa da, uzun süre çalışan, ağ veya g/ç ile bağlantılı görevleri gerçekleştirirken istemci yanıt hızını ve sunucu ölçeklenebilirliğini artırmak için kullanılabilir.

## <a name="when-to-really-use-async"></a>Zaman uyumsuz olarak ne zaman kullanılır?

Bu izlenecek yol, zaman uyumsuz kavramları, zaman uyumsuz ve zaman uyumlu program yürütme arasındaki farkı gözlemlemeye olanak verecek şekilde sağlamaktır. Bu izlenecek yol, zaman uyumsuz programlama avantajlarının sağladığı önemli senaryolardan herhangi birini göstermeye yönelik değildir.

Zaman uyumsuz programlama öncelikle, yönetilen bir iş parçacığından işlem süresi gerektirmeyen bir işlemi beklediği sırada başka çalışma yapmak için geçerli yönetilen iş parçacığını (.NET kodu çalıştıran iş parçacığı) boşaltmaya odaklanır. Örneğin, veritabanı altyapısı bir sorguyu işliyor, .NET kodu tarafından yapılacak bir şey yoktur.

İstemci uygulamalarında (WinForms, WPF vb.) geçerli iş parçacığı, zaman uyumsuz işlem gerçekleştirilirken Kullanıcı arabirimini yanıt verecek şekilde korumak için kullanılabilir. Sunucu uygulamalarında (ASP.NET vb.), iş parçacığı diğer gelen istekleri işlemek için kullanılabilir-Bu, bellek kullanımını azaltabilir ve/veya sunucunun verimini artırabilir.

Zaman uyumsuz kullanan çoğu uygulama, dikkat çekici bir avantaja sahip olmayacaktır ve hatta büyük olasılıkla olabilir. Test, profil oluşturma ve ortak anlamda, zaman uyumsuz olarak belirli senaryonuza uygulamadan önce etkisini ölçmek için kullanın.

Zaman uyumsuz hakkında bilgi edinmek için daha fazla kaynak aşağıda verilmiştir:

-   [Brandon Bray, .NET 4,5 ' de Async/Await öğesine genel bakış](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   MSDN kitaplığındaki [zaman uyumsuz programlama](https://msdn.microsoft.com/library/hh191443.aspx) sayfaları
-   [Zaman uyumsuz kullanarak ASP.NET Web uygulamaları oluşturma](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (daha fazla sunucu aktarım hızı içerir)

## <a name="create-the-model"></a>Modeli oluşturma

Modelimizi oluşturmak ve veritabanını oluşturmak için [Code First iş akışını](~/ef6/modeling/code-first/workflows/new-database.md) kullanacağız, ancak zaman uyumsuz Işlevi EF Designer ile oluşturulanlar dahil olmak üzere tüm EF modelleriyle çalışır.

-   Bir konsol uygulaması oluşturun ve bunu **AsyncDemo** olarak çağırın
-   EntityFramework NuGet paketini ekleme
    -   Çözüm Gezgini, **AsyncDemo** projesine sağ tıklayın
    -   **NuGet Paketlerini Yönet...** seçeneğini belirleyin.
    -   NuGet Paketlerini Yönet iletişim kutusunda **çevrimiçi** sekmesini seçin ve **EntityFramework** paketini seçin
    -   **Install** 'a tıklayın
-   Aşağıdaki uygulamayla bir **model.cs** sınıfı ekleyin

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

 

## <a name="create-a-synchronous-program"></a>Zaman uyumlu program oluşturma

Artık bir EF modelimiz olduğuna göre, bazı veri erişimi gerçekleştirmek için onu kullanan bir kod yazalım.

-   **Program.cs** içeriğini aşağıdaki kodla değiştirin

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

Bu kod, veritabanına yeni bir **Blog** kaydeden ve sonra tüm **blogları** veritabanından alıp **konsola**yazdıran **performdatabaseoperations** yöntemini çağırır. Bundan sonra program, günün bir teklifini **konsola**yazar.

Kod zaman uyumlu olduğundan, programı çalıştırdığımızda aşağıdaki yürütme akışını gözlemlebiliriz:

1.  **SaveChanges** yeni **blogunuzu** veritabanına göndermeye başlıyor
2.  **SaveChanges** tamamlandı
3.  Tüm **blogların** sorgusu veritabanına gönderiliyor
4.  Sorgu dönüşleri ve sonuçlar **konsola** yazılır
5.  Günün teklifi **konsola** yazılır

![Eşitleme çıkışı](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Zaman uyumsuz hale getirme

Programımızın çalışır duruma getirdiğimiz ve bu aşamada, yeni Async ve await anahtar sözcüklerini kullanmaya başlayabiliriz. Program.cs için aşağıdaki değişiklikleri yaptık

1.  2\. satır: **System. Data. Entity** ad alanı için using metodu, EF Async Extension yöntemlerine bize erişim sağlar.
2.  Satır 4: **System. Threading. Tasks** ad alanı için using ifadesinin **görev** türünü kullanmamızı sağlar.
3.  Satır 12 & 18: **Performsomedatabaseoperations** (12. satır) ilerlemesini izleyen bir görev olarak yakalıyoruz ve sonra programa yönelik tüm işler yapıldıktan sonra bu görevin tamamlanması için program yürütmesini engelliyor (satır 18).
4.  25. satır: **zaman uyumsuz** olarak işaretlenecek ve bir **görev**döndüren **performsomedatabaseoperations** 'ı güncelleştirdik.
5.  Satır 35: Şu anda SaveChanges 'un zaman uyumsuz sürümünü çağırıyor ve tamamlanmasını bekliyor.
6.  Satır 42: artık ToList 'in zaman uyumsuz sürümünü çağırıyor ve sonuç üzerinde bekleniyor.

System. Data. Entity ad alanındaki kullanılabilir uzantı yöntemlerinin kapsamlı bir listesi için, QueryableExtensions sınıfına bakın. *Ayrıca, using deyimlerine "System. Data. Entity kullanma" eklemeniz gerekecektir.*

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

Artık kod zaman uyumsuz olduğuna göre, programı çalıştırdığımızda farklı bir yürütme akışını gözlemlebiliriz:

1. **SaveChanges** yeni **blogunuzu** veritabanına göndermeye başlıyor  
    *Komut veritabanına gönderildikten sonra, geçerli yönetilen iş parçacığında daha fazla işlem süresi gerekmez. **Performdatabaseoperations** yöntemi döndürür (yürütmeyi bitirmemiş olsa da) ve Main yönteminde Program Flow devam eder.*
2. **Günün teklifi konsola yazılır**  
    *Main yönteminde yapılacak başka bir iş olmadığından, veritabanı işlemi tamamlanana kadar, yönetilen iş parçacığı bekleme çağrısında engellenir. Tamamlandıktan sonra, **Performdatabaseoperations** 'imizin geri kalanı yürütülür.*
3.  **SaveChanges** tamamlandı  
4.  Tüm **blogların** sorgusu veritabanına gönderiliyor  
    *Bu durumda, sorgu veritabanında işlendiği sırada yönetilen iş parçacığı başka bir iş yapmak için ücretsizdir. Diğer tüm yürütme tamamlandığından iş parçacığı de yalnızca bekleme çağrısında durabilir.*
5.  Sorgu dönüşleri ve sonuçlar **konsola** yazılır  

![Zaman uyumsuz çıkış](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Kalkış

Artık EF 'in zaman uyumsuz yöntemlerinin ne kadar kolay olduğunu gördük. Zaman uyumsuz 'nin avantajları basit bir konsol uygulamasıyla çok açık olmayabilir, ancak uzun süreli veya ağ bağlantılı etkinliklerin uygulamayı engelleyebileceği veya çok sayıda iş parçacığına neden olabileceği durumlarda aynı stratejiler uygulanabilir. bellek parmak izini artırın.
