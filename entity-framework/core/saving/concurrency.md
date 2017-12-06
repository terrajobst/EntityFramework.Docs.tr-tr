---
title: "EF çekirdek eşzamanlılık - işleme"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a>Eşzamanlılık işleme

Ardından bir özelliği bir eşzamanlılık belirteci olarak yapılandırılmışsa, EF başka bir kullanıcı o değerini veritabanında değişiklikler kayda kaydedilirken değiştirdi denetleyin.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) github'da.

## <a name="how-concurrency-handling-works-in-ef-core"></a>Eşzamanlılık işleme EF çekirdek nasıl çalışır?

Eşzamanlılık işleme Entity Framework Çekirdek nasıl çalıştığını ayrıntılı bir açıklaması için bkz: [eşzamanlılık belirteçleri](../modeling/concurrency.md).

## <a name="resolving-concurrency-conflicts"></a>Eşzamanlılık çakışmalarını çözme

Bir eşzamanlılık çakışması çözümleme geçerli kullanıcı veritabanında yapılan değişikliklerle bekleyen değişiklikler birleştirmek için bir algoritma kullanmayı içerir. Tam bir yaklaşım uygulamanız göre değişir, ancak ortak bir yaklaşım kullanıcıya değerleri görüntüler ve bunları doğru değerleri veritabanında depolanan karar sahip olmaktır.

**Değerleri bir eşzamanlılık çakışması çözümlenmesine yardımcı olmak kullanılabilir üç kümesi vardır.**

* **Geçerli değerler** uygulama veritabanına yazmaya çalışıyordu değerlerdir.

* **Özgün değerler** tüm düzenlemeleri yapılmadan önce ilk olarak veritabanından alınan değerlerdir.

* **Veritabanı değerleri** şu anda veritabanında depolanan değerler.

Bir eşzamanlılık çakışması işlemek için catch bir `DbUpdateConcurrencyException` sırasında `SaveChanges()`, kullanın `DbUpdateConcurrencyException.Entries` etkilenen varlıklar için yeni bir değişiklik kümesini hazırlamak ve ardından yeniden deneyin `SaveChanges()` işlemi.

Aşağıdaki örnekte, `Person.FirstName` ve `Person.LastName` Kurulum eşzamanlılık belirteci olarak değiştirilebilir. Var olan bir `// TODO:` burada dahil veritabanına kaydedilecek değer seçmek için uygulama belirli mantığını konumda açıklama.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
