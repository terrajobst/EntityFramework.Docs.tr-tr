---
title: İlişkiler, gezinti özellikleri ve yabancı anahtarlar - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: a98c1bf798a8a6d2c748408d7363d5f884e7e6e9
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490551"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>İlişkiler, gezinti özellikleri ve yabancı anahtarlar
Bu konu, Entity Framework varlıklar arasındaki ilişkilerin nasıl yönettiğine bir genel bakış sağlar. Ayrıca ilişkileri eşleyin ve düzenleme hakkında rehberlik sağlar.

## <a name="relationships-in-ef"></a>EF ilişkileri

İlişkisel veritabanları, tablolar arasında ilişki (ilişkilendirmeleri olarak da bilinir), yabancı anahtarlar tanımlanır. Yabancı anahtar (FK) bir sütun veya kurmak ve iki tablodaki veriler arasında bir bağlantı zorlamak için kullanılan sütunlar bileşimidir. Genellikle üç türde ilişki vardır: bire bir, bire çok ve çok-çok. Bir-çok ilişki, yabancı anahtar birçok ilişki sonunu temsil eden tabloda tanımlanır. Çoktan çoğa ilişki ile ilgili iki tablodan yabancı anahtarlar birincil anahtarı oluşur (bir birleşim veya birleşim tablo olarak adlandırılır) üçüncü bir tablo tanımlama ilgilidir. Bire bir ilişki, yabancı anahtar olarak ayrıca birincil anahtar görevi görür ve ayrı yabancı anahtar sütunu yok ya da tablo için yoktur.

Aşağıdaki görüntüde katılan iki tablo-çok ilişkisini gösterir. **Kurs** tablodur bağımlı tablo içerdiği için **DepartmentID** bağlantı sütunu **departmanı** tablo.

![Bölüm ve kursu tabloları](~/ef6/media/database2.png)

Varlık Çerçevesi'nde varlığın diğer varlıklarla ilişki veya ilişki ile ilgili olabilir. Her ilişki varlık türü ve tür (bir, sıfır veya bir veya birçok) Bu ilişki iki varlıktaki için Çokluk tanımlayan iki ucu içerir. İlişki hangi son ilişkisinde bir asıl rolüdür tanımlayan bir başvuru kısıtlamasını tarafından yönetilebilir ve bağımlı rol olduğu.

Gezinti özellikleri iki varlık türleri arasındaki ilişkiyi gitmek için bir yol sağlar. Her nesne içinde katılan her ilişki için bir gezinti özelliği olabilir. Gezinti özellikleri gidin ve bir başvuru nesnesi döndürerek her iki yönde ilişkileri yönetmenize olanak sağlar (çeşitlilik tek ise ya da sıfır veya bir) veya (çeşitlilik birçok ise) bir koleksiyon. Ayrıca tek yönlü gezinme her ikisini de değil ve ilişkisine katılıyor türleri yalnızca birinde gezinme özelliği bu durumda tanımlamak seçebilirsiniz.

Bir veritabanındaki yabancı anahtarlar eşleyen modelinde özellikler eklemek için önerilir. Dahil edilen yabancı anahtar özellikleri ile oluşturabilir veya bağımlı bir nesne üzerinde yabancı anahtar değerini değiştirerek bir ilişki. Bu tür bir ilişki, yabancı anahtar ilişkilendirmesi adı verilir. Yabancı anahtarlar kullanarak bağlantısı kesilmiş varlıklar ile çalışırken daha da önemlidir. Not, söz konusu olduğunda 1-1 veya 1-0 ile çalışma... 1 ilişki ayrı yabancı anahtar sütunu yok, birincil anahtar özelliği yabancı anahtar olarak davranır ve modeldeki her zaman dahildir.

Yabancı anahtar sütunları, modelde yer almaz, ilişki bilgilerini bağımsız bir nesne yönetilir. Yabancı anahtar özellikleri yerine nesne başvuruları arasında ilişkileri izlenir. Bu tür bir ilişkilendirme olarak adlandırılan bir *bağımsız ilişkilendirme*. Değiştirmek için en yaygın yolu bir *bağımsız ilişkilendirme* ilişkilendirmesine katılan her varlık için oluşturulan gezinti özelliklerini değiştirmek için.

Modelinizde ilişkilendirmeleri birini veya ikisini türleri kullanmayı da tercih edebilirsiniz. Yalnızca yabancı anahtarları içeren bir birleştirme tablo bağlı saf bir çoktan çoğa ilişki varsa, ancak, EF bağımsız ilişkilendirme böyle çoktan çoğa ilişki yönetmek için kullanırsınız.   

Aşağıdaki görüntüde, Entity Framework Designer ile oluşturulmuş bir kavramsal model gösterilmektedir. Model-çok ilişkide yer alan iki varlık içerir. Her iki varlık Gezinti özellikleri vardır. **Kurs** bağımlı varlık ve **DepartmentID** tanımlı yabancı anahtar özelliği.

![Gezinti özellikleri içeren bölüm ve kurs tablolar](~/ef6/media/relationshipefdesigner.png)

Aşağıdaki kod parçacığı, Code First ile oluşturulan aynı modelin gösterir.

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
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>Yapılandırma veya ilişkileri eşleme

Bu sayfanın geri kalanını erişmek ve ilişkileri kullanarak verileri işlemek nasıl etkinleştireceğinizi de açıklar. Modelinizde ilişkileri ayarlama hakkında daha fazla bilgi için şu sayfalara bakın.

-   İlişkiler Code First yapılandırmak için bkz [veri ek açıklamaları](~/ef6/modeling/code-first/data-annotations.md) ve [Fluent API'si – ilişkileri](~/ef6/modeling/code-first/fluent/relationships.md).
-   Entity Framework Designer kullanarak ilişkilerini yapılandırmak için bkz [EF Designer ilişkilerle](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Oluşturma ve ilişkileri değiştirme

İçinde bir *yabancı anahtar ilişkilendirmesi*, ilişki ile bağımlı bir nesnenin durumu değiştiğinde bir `EntityState.Unchanged` durumu değişiklikleri `EntityState.Modified`. Bağımsız bir ilişkide, ilişkinin değiştirme bağımlı nesne durumunu güncelleştirmez.

Aşağıdaki örnekler, yabancı anahtar özellikler ve gezinti özellikleri ilgili nesneleri ilişkilendirmek için nasıl kullanılacağını gösterir. Yabancı anahtar ilişkilerini değiştirme, oluşturmak veya ilişkileri değiştirmek için her iki yöntem kullanabilirsiniz. Bağımsız ilişkilerini, yabancı anahtar özelliği kullanılamaz.

- Yeni bir değer aşağıdaki örnekte olduğu gibi bir yabancı anahtar özelliğine atayarak.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- Aşağıdaki kod bir ilişki, yabancı anahtarı ayarlayarak kaldırır **null**. Yabancı anahtar özelliği null olması gerektiğini unutmayın.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > Başvuru (Bu örnekte, kurs nesnesi) eklenmiş durumda ise, SaveChanges çağrılana kadar başvuru gezinti özelliği yeni bir nesnenin anahtar değerleriyle eşitlenmez. Nesne bağlamı kaydedilmeden kadar eklenen nesneler için kalıcı anahtarlar içermediğinden eşitleme gerçekleşmez. Yeni nesneler ilişkisi hemen sonra tam olarak eşitlenmiş olması gerekir, aşağıdaki yöntemlerin birini kullanın.

- Yeni bir nesne bir gezinti özelliğine atayarak. Aşağıdaki kod bir kurs arasında bir ilişki oluşturur ve bir `department`. Nesneleri bağlamına ekliyse `course` de eklenir `department.Courses` koleksiyonu ve yabancı karşılık gelen anahtar özellik üzerinde `course` nesne departmanı anahtar özellik değerine ayarlanır.  
  ``` csharp
  course.Department = department;
  ```

- İlişkiyi silmek için gezinme özelliğini ayarlamak `null`. Entity Framework, .NET 4.0 tabanlı ile çalışıyorsanız, ilgili uç, null olarak ayarlamadan önce yüklü olması gerekir. Örneğin:   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  Entity Framework, .NET 4.5 üzerinde temel alınan 5.0 ile başlatma, ilişki null ilgili uç yüklemeden ayarlayabilirsiniz. Ayrıca, aşağıdaki yöntemi kullanarak null değeri ayarlayabilirsiniz.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- Silme veya bir varlık koleksiyonu bir nesne ekleme. Örneğin, türü bir nesne ekleyebilirsiniz `Course` için `department.Courses` koleksiyonu. Bu işlem, belirli bir arasında bir ilişki oluşturur **kurs** ve belirli bir `department`. Nesneler üzerinde eklenmiş içerik, departman başvuru ve yabancı anahtar özelliği varsa **kurs** nesne ayarlanacak uygun `department`.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- Kullanarak `ChangeRelationshipState` iki varlık nesnesi arasında belirtilen ilişki durumunu değiştirmek için yöntemi. Bu yöntem N katmanlı uygulamalar ile çalışırken en yaygın olarak kullanılır ve bir *bağımsız ilişkilendirme* (yabancı anahtar ilişkilendirmesi ile kullanılamaz). Ayrıca, bu yöntemi kullanmak için düşürmeli aşağı `ObjectContext`aşağıdaki örnekte gösterildiği gibi.  
Aşağıdaki örnekte, eğitmenlerini ve dersleri arasında bir çoktan çoğa ilişki yoktur. Çağırma `ChangeRelationshipState` yöntemi ve geçirme `EntityState.Added` parametresi, sağlar `SchoolContext` iki nesne bir ilişki eklendiğini bildirin:
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  (Yalnızca ekleme) güncelleştiriyorsanız unutmayın bir ilişki eski ilişkiyi yeni bir tane ekledikten sonra silmeniz gerekir:

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Gezinti özellikleri ve yabancı anahtarlar arasındaki değişiklikleri eşitleme

Yukarıda anlatılan yöntemlerden birini kullanarak bağlamına iliştirilemez. nesneler arasındaki ilişkiyi değiştirdiğinizde, yabancı anahtarlar, başvuruları ve koleksiyonları eşitlemek Entity Framework gerekir. Entity Framework bu eşitleme (olarak da bilinen ilişki düzeltmesi yan yana) proxy'si ile POCO varlık için otomatik olarak yönetir. Daha fazla bilgi için [proxy ile çalışmayı](~/ef6/fundamentals/proxies.md).

POCO varlık olmayan proxy'si kullanıyorsanız, emin olmanız gerekir **DetectChanges** bağlamındaki ilişkili nesneleri eşitlenecek yöntemi çağrılır. Aşağıdaki API'ları otomatik olarak tetikleyen Not bir **DetectChanges** çağırın.

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
-   Yürütülen bir LINQ Sorgu karşı bir `DbSet`

## <a name="loading-related-objects"></a>Yükleme ile ilgili nesneler

En sık kullandığınız Entity Framework içinde tanımlanmış ilişki tarafından döndürülen varlık ilgili varlıkları yükleme için gezinme özelliklerini kullanın. Daha fazla bilgi için [ilgili nesneler Yükleniyor](~/ef6/querying/related-data.md).

> [!NOTE]
> Bağımlı bir nesne bir ilgili uç yüklediğinizde bir yabancı anahtar ilişkilendirmesine, ilgili nesneyi göre şu anda bellekte olmadığını bağımlı yabancı anahtar değeri olarak yüklenecektir:

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

Bağımsız bir ilişkide, şu anda veritabanında yabancı anahtar değere göre bir bağımlı nesne ilgili sonuna sorgulanır. Ancak, ilişkisi değiştirildi ve bağımlı nesne noktalarında nesne bağlamında, Entity Framework yüklenen farklı bir asıl nesneye başvuru özelliği olarak bir ilişki oluşturmak deneyecek, istemcide tanımlanır.

## <a name="managing-concurrency"></a>Eşzamanlılığı yönetme

Yabancı anahtar hem bağımsız ilişkilendirmeleri eşzamanlılık denetimlerinin varlık anahtarları ve modelde tanımlanan diğer varlık özellikleri temel alır. Bir model oluşturmak için EF Designer'ı kullanırken, ayarlayın `ConcurrencyMode` özniteliğini **sabit** özelliği için eşzamanlılık denetlenmesi gerektiğini belirtmek için. Bir modeli tanımlamak için Code First kullanarak kullanınl `ConcurrencyCheck` denetlenmesi için eşzamanlılık istediğiniz özellikleri ek açıklama. Code First ile çalışırken de kullanabilirsiniz `TimeStamp` özelliği için eşzamanlılık denetlenmesi gerektiğini belirtmek için ek açıklama. Belirli bir sınıf içinde yalnızca bir zaman damgası özelliği olabilir. Kod, bu özellik ilk veritabanı NULL olmayan bir alana eşlemeleri.

Her zaman yabancı anahtar ilişkilendirmesi eşzamanlılık denetimi ve çözümleme katılacak varlıklar ile çalışırken kullanmanızı öneririz.

Daha fazla bilgi için [eşzamanlılık çakışmalarını işleme](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Anahtarları örtüşen ile çalışma

Bazı özellikler anahtarında da varlıktaki başka bir anahtarın parçası olduğu bileşik anahtarlar çakışan anahtarlar var. Çakışan bir anahtarı bağımsız ilişkilendirmesine sahip olamaz. Anahtarları örtüşen içeren bir yabancı anahtar ilişkilendirmesi değiştirmek için nesne başvuruları kullanmak yerine, yabancı anahtar değerlerini değiştirmenizi öneririz.
