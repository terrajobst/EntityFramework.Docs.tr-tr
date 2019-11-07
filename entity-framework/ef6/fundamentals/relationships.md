---
title: İlişkiler, gezinti özellikleri ve yabancı anahtarlar-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: cc7160f2d0ab7ac0c6009f820441c88590cacfaf
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655873"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>İlişkiler, gezinti özellikleri ve yabancı anahtarlar
Bu konu, varlıklar arasındaki ilişkileri nasıl yönettiğini Entity Framework bir genel bakış sunar. Ayrıca, ilişkilerin eşlenme ve işleme hakkında bazı yönergeler de sağlar.

## <a name="relationships-in-ef"></a>EF 'teki ilişkiler

İlişkisel veritabanlarında, tablolar arasındaki ilişkiler (ilişkilendirmeler olarak da bilinir) yabancı anahtarlar aracılığıyla tanımlanır. Yabancı anahtar (FK), iki tablodaki veriler arasında bağlantı kurmak ve zorlamak için kullanılan bir sütun veya sütun birleşimidir. Genellikle üç tür ilişki vardır: bire bir, bire çok ve çoktan çoğa. Bire çok ilişkisinde, yabancı anahtar ilişkinin birçok sonunu temsil eden tabloda tanımlanmıştır. Çoktan çoğa ilişki, birincil anahtarı ilgili tablolardaki yabancı anahtarlardan oluşan bir üçüncü tablo (kavşak veya birleşim tablosu olarak adlandırılır) tanımlamayı içerir. Bire bir ilişkide, birincil anahtar ek olarak yabancı anahtar olarak davranır ve her iki tablo için ayrı bir yabancı anahtar sütunu yoktur.

Aşağıdaki görüntüde bire çok ilişkisine katılan iki tablo gösterilmektedir. **Kurs** tablosu, **bölüm** tablosuna bağlayan **DepartmentID** sütununu içerdiğinden bağımlı tablodur.

![Departman ve kurs tabloları](~/ef6/media/database2.png)

Entity Framework, bir varlık bir ilişki veya ilişki aracılığıyla diğer varlıklarla ilişkili olabilir. Her ilişki, söz konusu ilişkideki iki varlık için varlık türünü ve türün çokluğunu (bir, sıfır-veya-bir ya da çok) tanımlayan iki ucu içerir. İlişki, ilişkinin bir asıl rol olduğunu ve bağımlı bir rol olduğunu açıklayan bir başvuru kısıtlaması tarafından yönetilebilir.

Gezinti özellikleri iki varlık türü arasındaki bir ilişkilendirmeyi gezinmek için bir yol sağlar. Her nesnenin katıldığı her ilişki için bir gezinti özelliği olabilir. Gezinti özellikleri, bir başvuru nesnesi (çoğulluk bir veya sıfır ya da-bir) ya da bir koleksiyon (çokluk çok ise) döndüren her iki yönde ilişkilerde gezinmeniz ve bunları yönetmenize olanak tanır. Tek yönlü bir gezinmenin de tercih edebilirsiniz. Bu durumda, gezinti özelliğini yalnızca ilişkiye katılan ve her ikisi üzerinde değil, yalnızca birinde tanımladığınız türlerden birinde tanımlarsınız.

Modeldeki yabancı anahtarlarla eşlenen özellikleri eklemek önerilir. Yabancı anahtar özellikleri dahil olmak üzere, bağımlı bir nesne üzerindeki yabancı anahtar değerini değiştirerek bir ilişki oluşturabilir veya değiştirebilirsiniz. Bu tür bir ilişkiye yabancı anahtar ilişkilendirmesi denir. Bağlantısı kesilmiş varlıklarla çalışırken yabancı anahtarların kullanılması daha da önemlidir. 1 ile 1 arasında veya 1 ile 0 arasında çalışırken. 1 ilişkiler, ayrı bir yabancı anahtar sütunu yoktur, birincil anahtar özelliği yabancı anahtar olarak davranır ve her zaman modele dahil edilir.

Yabancı anahtar sütunları modele dahil edilmediğinde, ilişkilendirme bilgileri bağımsız bir nesne olarak yönetilir. İlişkiler, yabancı anahtar özellikleri yerine nesne başvuruları aracılığıyla izlenir. Bu tür bir ilişkilendirme *bağımsız bir ilişkilendirme*olarak adlandırılır. *Bağımsız bir ilişkilendirmeyi* değiştirmek için en yaygın yol, ilişkilendirmede yer alan her varlık için oluşturulan gezinti özelliklerini değiştirmektir.

Modelinizde bir veya her iki tür ilişkilendirmeyi kullanmayı seçebilirsiniz. Ancak, yalnızca yabancı anahtarlar içeren bir JOIN tablosu tarafından bağlanan saf çoktan çoğa ilişkiye sahipseniz, EF bu çok-çok ilişkisini yönetmek için bağımsız bir ilişki kullanır.   

Aşağıdaki görüntüde Entity Framework Designer ile oluşturulmuş kavramsal bir model gösterilmektedir. Model bire çok ilişkisine katılan iki varlık içerir. Her iki varlık de gezinti özelliklerine sahiptir. **Kurs** , bağımlı varlıktır ve **DepartmentID** yabancı anahtar özelliği tanımlanmış.

![Gezinti özelliklerine sahip departman ve kurs tabloları](~/ef6/media/relationshipefdesigner.png)

Aşağıdaki kod parçacığı, Code First ile oluşturulmuş modeli gösterir.

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>İlişkileri yapılandırma veya eşleme

Bu sayfanın geri kalanında ilişkiler kullanılarak verilere erişme ve verileri işleme konuları ele alınmaktadır. Modelinizde ilişkiler ayarlama hakkında daha fazla bilgi için aşağıdaki sayfalara bakın.

-   Code First ilişkilerini yapılandırmak için, bkz. [veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md) ve [akıcı API – ilişkiler](~/ef6/modeling/code-first/fluent/relationships.md).
-   Entity Framework Designer kullanarak ilişkileri yapılandırmak için, bkz. [EF Designer Ile ilişkiler](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>İlişki oluşturma ve değiştirme

Bir *yabancı anahtar ilişkisinde*, ilişkiyi değiştirdiğinizde, bağımlı nesnenin durumu, `EntityState.Unchanged` durumu `EntityState.Modified`olarak değişir. Bağımsız bir ilişkide ilişki değiştirildiğinde bağımlı nesnenin durumu güncellemez.

Aşağıdaki örneklerde, ilişkili nesneleri ilişkilendirmek için yabancı anahtar özelliklerinin ve gezinti özelliklerinin nasıl kullanılacağı gösterilmektedir. Yabancı anahtar ilişkilendirmeleriyle, ilişkileri değiştirmek, oluşturmak veya değiştirmek için her iki yöntemi de kullanabilirsiniz. Bağımsız İlişkilendirmelerde, yabancı anahtar özelliğini kullanamazsınız.

- Aşağıdaki örnekte olduğu gibi, yabancı anahtar özelliğine yeni bir değer atayarak.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- Aşağıdaki kod, yabancı anahtarı **null**olarak ayarlayarak bir ilişkiyi kaldırır. Yabancı anahtar özelliğinin null yapılabilir olması gerektiğini unutmayın.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > Başvuru eklenen durumundaysa (Bu örnekte, kurs nesnesi), SaveChanges çağrılmadan önce başvuru gezintisi özelliği yeni bir nesnenin anahtar değerleriyle eşitlenmez. Nesne bağlamı, kaydedilmeden eklenen nesneler için kalıcı anahtarlar içermediğinden eşitleme gerçekleşmez. İlişkiyi ayarladığınız anda yeni nesneleri tamamen eşitlenmiş olması gerekiyorsa, aşağıdaki yöntemlerden birini kullanın. *

- Bir gezinti özelliğine yeni bir nesne atayarak. Aşağıdaki kod, kurs ile `department`arasında bir ilişki oluşturur. Nesneler bağlama eklenirse, `course` `department.Courses` koleksiyonuna de eklenir ve `course` nesnesindeki karşılık gelen yabancı anahtar özelliği departmanın anahtar özellik değerine ayarlanır.  
  ``` csharp
  course.Department = department;
  ```

- İlişkiyi silmek için, gezinti özelliğini `null`olarak ayarlayın. .NET 4,0 tabanlı Entity Framework çalışıyorsanız, null olarak ayarlamadan önce ilgili ucun yüklenmesi gerekir. Örneğin:   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  .NET 4,5 ' i temel alan Entity Framework 5,0 ' den başlayarak, ilişkili bitişi yüklemeden ilişkiyi null olarak ayarlayabilirsiniz. Ayrıca, aşağıdaki yöntemi kullanarak geçerli değeri null olarak ayarlayabilirsiniz.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- Bir varlık koleksiyonundaki bir nesneyi silerek veya ekleyerek. Örneğin, `department.Courses` koleksiyonuna `Course` türünde bir nesne ekleyebilirsiniz. Bu işlem, belirli bir **Kurs** ve belirli bir `department`arasında bir ilişki oluşturur. Nesneler içeriğe eklenmişse, **Kurs** nesnesindeki departman başvurusu ve yabancı anahtar özelliği uygun `department`ayarlanır.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- İki varlık nesnesi arasında belirtilen ilişkinin durumunu değiştirmek için `ChangeRelationshipState` yöntemini kullanarak. Bu yöntem, N katmanlı uygulamalarla ve *bağımsız bir ilişkilendirmede* (bir yabancı anahtar ilişkisiyle birlikte kullanılamaz) çalışırken yaygın olarak kullanılır. Ayrıca, bu yöntemi kullanmak için aşağıdaki örnekte gösterildiği gibi, `ObjectContext`için öğesini açmalısınız.  
Aşağıdaki örnekte, Eğitmenler ve kurslar arasında çoktan çoğa ilişki vardır. `ChangeRelationshipState` yöntemini çağırarak ve `EntityState.Added` parametresini geçirerek, `SchoolContext` iki nesne arasında bir ilişki eklendiğini bilmesini sağlar:
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  Bir ilişki güncelleştiriyorsanız (yalnızca eklemeyi değil), yenisini ekledikten sonra eski ilişkiyi silmeniz gerektiğini unutmayın:

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Yabancı anahtarlar ve gezinti özellikleri arasındaki değişiklikleri eşitleme

Yukarıda açıklanan yöntemlerden birini kullanarak bağlama eklenmiş nesnelerin ilişkisini değiştirdiğinizde Entity Framework yabancı anahtarları, başvuruları ve koleksiyonları eşitlenmiş halde tutmaları gerekir. Entity Framework, proxy 'leri olan POCO varlıklarının bu eşitlemesini (ilişki çözme olarak da bilinir) otomatik olarak yönetir. Daha fazla bilgi için bkz. [proxy Ile çalışma](~/ef6/fundamentals/proxies.md).

Proxy 'siz POCO varlıklarını kullanıyorsanız, bağlam içindeki ilişkili nesneleri eşitlemeniz için **DetectChanges** yönteminin çağrıldığından emin olmanız gerekir. Aşağıdaki API 'Lerin bir **DetectChanges** çağrısını otomatik olarak tetikleyeceğini unutmayın.

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   `DbSet` bir LINQ sorgusu yürütme

## <a name="loading-related-objects"></a>İlgili nesneler yükleniyor

Entity Framework, tanımlı ilişki tarafından döndürülen varlıkla ilişkili varlıkları yüklemek için genellikle gezinti özelliklerini kullanırsınız. Daha fazla bilgi için bkz. [Ilgili nesneleri yükleme](~/ef6/querying/related-data.md).

> [!NOTE]
> Yabancı anahtar ilişkisinde, bağımlı bir nesnenin ilgili bir sonunu yüklediğinizde ilgili nesne, şu anda bellekte olan bağımlı anahtarın yabancı anahtar değerine göre yüklenir:

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

Bağımsız bir ilişkide, bağımlı bir nesnenin ilgili ucu, şu anda veritabanında olan yabancı anahtar değerine göre sorgulanır. Ancak, ilişki değiştirildiyse ve bağımlı nesne üzerindeki başvuru özelliği, nesne bağlamına yüklenen farklı bir Principal nesnesine işaret ediyorsa, Entity Framework istemci üzerinde tanımlanan bir ilişki oluşturmayı dener.

## <a name="managing-concurrency"></a>Eşzamanlılık yönetimi

Hem yabancı anahtar hem de bağımsız İlişkilendirmelerde eşzamanlılık denetimleri, modelde tanımlanan varlık anahtarlarına ve diğer varlık özelliklerine göre yapılır. Bir model oluşturmak için EF tasarımcısını kullanırken, özelliğinin eşzamanlılık için denetlenmesi gerektiğini belirtmek üzere `ConcurrencyMode` özniteliğini **fixed** olarak ayarlayın. Bir modeli tanımlamak için Code First kullanırken, eşzamanlılık için denetlenmesini istediğiniz özelliklerde `ConcurrencyCheck` ek açıklamasını kullanın. Code First ile çalışırken, özelliğin eşzamanlılık için denetlenmesi gerektiğini belirtmek için `TimeStamp` ek açıklamasını de kullanabilirsiniz. Belirli bir sınıfta yalnızca bir zaman damgası özelliğine sahip olabilirsiniz. Code First, bu özelliği veritabanında null olmayan bir alana eşleştirir.

Eşzamanlılık denetimi ve çözümüne katılan varlıklarla çalışırken her zaman yabancı anahtar ilişkilendirmesini kullanmanızı öneririz.

Daha fazla bilgi için bkz. [eşzamanlılık çakışmalarını işleme](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Çakışan anahtarlarla çalışma

Çakışan anahtarlar, anahtardaki bazı özelliklerin aynı zamanda varlıktaki başka bir anahtarın parçası olduğu bileşik anahtarlardır. Bağımsız bir ilişkide çakışan bir anahtarınız olamaz. Çakışan anahtarlar içeren bir yabancı anahtar ilişkilendirmesini değiştirmek için, nesne başvurularını kullanmak yerine yabancı anahtar değerlerini değiştirmenizi öneririz.
