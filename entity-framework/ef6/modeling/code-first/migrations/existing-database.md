---
title: Var olan bir veritabanıyla Code First Migrations-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
ms.openlocfilehash: eb7948eafb1322cabcf69b47bd5411f762fe8498
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182577"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Mevcut bir veritabanıyla Code First Migrations
> [!NOTE]
> **EF 4.3 yalnızca** bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 4,1 ' de tanıtılmıştı. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

Bu makalede, Entity Framework tarafından oluşturulmamış mevcut bir veritabanıyla Code First Migrations kullanımı ele alınmaktadır.

> [!NOTE]
> Bu makalede temel senaryolarda Code First Migrations kullanmayı bildiğiniz varsayılmaktadır. Bunu yapmazsanız devam etmeden önce [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) okumanız gerekir.

## <a name="screencasts"></a>Tasarlandı

Bu makaleyi okuduğunuzdan bir ekran koruyucu izlemeyi tercih ediyorsanız, aşağıdaki iki video bu makaleyle aynı içeriği kapsar.

### <a name="video-one-migrations---under-the-hood"></a>Video One: "Geçişler-"

[Bu ekran kaydı](https://channel9.msdn.com/blogs/ef/migrations-under-the-hood) , geçişlerin, model değişikliklerini algılamak için model hakkındaki bilgileri nasıl izlediğini ve kullandığını ele alır.

### <a name="video-two-migrations---existing-databases"></a>Video Iki: "Geçişler-mevcut veritabanları"

Önceki videodaki kavramlar üzerinde oluşturma, [Bu ekran kaydı](https://channel9.msdn.com/blogs/ef/migrations-existing-databases) , var olan bir veritabanıyla geçişleri etkinleştirme ve kullanma konularını ele alır.

## <a name="step-1-create-a-model"></a>1\. adım: Model oluşturma

İlk adımınız, var olan veritabanınızı hedefleyen Code First bir model oluşturmak olacaktır. [Var olan bir veritabanına Code First](~/ef6/modeling/code-first/workflows/existing-database.md) , bunun nasıl yapılacağı hakkında ayrıntılı yönergeler sağlar.

>[!NOTE]
> Modelinizde veritabanı şemasında değişiklik yapılmasını gerektiren değişiklikler yapmadan önce bu konudaki adımların geri kalanını izlemeniz önemlidir. Aşağıdaki adımlar, modelin veritabanı şeması ile eşitlenmiş olmasını gerektirir.

## <a name="step-2-enable-migrations"></a>2\. adım: Geçişleri etkinleştir

Sonraki adım, geçişleri etkinleştirmektir. Bunu, Paket Yöneticisi konsolundaki **geçişi etkinleştir** komutunu çalıştırarak yapabilirsiniz.

Bu komut, çözümünüzde geçişler adlı bir klasör oluşturur ve bunun içinde yapılandırma adlı tek bir sınıf yerleştirir. Yapılandırma sınıfı, uygulamanız için geçişleri yapılandırdığınız yerdir, [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) konusunda daha fazla bilgi edinebilirsiniz.

## <a name="step-3-add-an-initial-migration"></a>3\. adım: İlk geçiş Ekle

Geçişler oluşturulduktan ve yerel veritabanına uygulandıktan sonra, bu değişiklikleri diğer veritabanlarına da uygulamak isteyebilirsiniz. Örneğin, yerel veritabanınız bir test veritabanı olabilir ve son olarak, değişiklikleri bir üretim veritabanına ve/veya diğer geliştiriciler test veritabanlarına de uygulamak isteyebilirsiniz. Bu adım için iki seçenek bulunur ve sizin seçmeniz gereken diğer veritabanlarının şemasının boş olup olmadığı veya şu anda yerel veritabanının şemasıyla eşleşip eşleşmediğini gösterir.

-   **seçenek: Başlangıç noktası olarak mevcut şemayı kullan.** Bu yaklaşımı, daha sonra geçiş yapılacak diğer veritabanları, yerel veritabanınız ile aynı şemaya sahip olacağı durumlarda kullanmanız gerekir. Örneğin, yerel test veritabanınız Şu anda üretim veritabanınızın v1 ile eşleşiyorsa bunu kullanabilir ve daha sonra üretim veritabanınızı v2 'ye güncelleştirmek için bu geçişleri uygularsınız.
-   **Seçenek Iki: Başlangıç noktası olarak boş veritabanını kullan.** Bu yaklaşımı, geçiş için daha sonra uygulanacak diğer veritabanları boş olduğunda (veya henüz yoksa) kullanmanız gerekir. Örneğin, bir test veritabanını kullanarak uygulamanızı geliştirmeye başladıysanız ancak geçişleri kullanmadan, daha sonra bir üretim veritabanını sıfırdan oluşturmak istiyorsanız bu işlemi kullanabilirsiniz.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Seçenek bir: Varolan şemayı başlangıç noktası olarak kullan

Code First Migrations, modeldeki değişiklikleri algılamak için en son geçişte depolanan modelin anlık görüntüsünü kullanır (Bu, ilgili ayrıntılı bilgileri [Takım ortamlarında Code First Migrations](~/ef6/modeling/code-first/migrations/teams.md)bulabilirsiniz). Veritabanlarının zaten geçerli modelin şemasına sahip olduğunu varsaydığımızdan, anlık görüntü olarak geçerli modele sahip boş bir (Hayır-op) geçişi oluşturacağız.

1.  Paket Yöneticisi konsolundaki **Add-Migration ınitialcreate – ıgnorechanges** komutunu çalıştırın. Bu, geçerli modelle anlık görüntü olarak boş bir geçiş oluşturur.
2.  Package Manager konsolunda **Update-Database** komutunu çalıştırın. Bu, ınitialcreate geçişini veritabanına uygular. Gerçek geçiş herhangi bir değişiklik içermediğinden, bu geçişin zaten uygulandığını belirten \_ @ no__t-1MigrationsHistory tablosuna yalnızca bir satır ekler.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>Iki seçenek: Boş veritabanını başlangıç noktası olarak kullan

Bu senaryoda, yerel veritabanımızda zaten mevcut olan tablolar da dahil olmak üzere tüm veritabanını sıfırdan oluşturabilmeniz için geçişlere ihtiyacımız var. Mevcut şemayı oluşturma mantığını içeren bir ınitialcreate geçişi oluşturacağız. Daha sonra var olan veritabanımızın bu geçiş zaten uygulanmış gibi görünmesini sağlayacağız.

1.  Paket Yöneticisi konsolundaki **Add-Migration ınitialcreate** komutunu çalıştırın. Bu, varolan şemayı oluşturmak için bir geçiş oluşturur.
2.  Yeni oluşturulan geçişin up yöntemindeki tüm kodu açıklama. Bu, daha önce var olan tüm tabloları yeniden oluşturmaya gerek kalmadan yerel veritabanına geçişi ' uygulamamıza izin verir.
3.  Package Manager konsolunda **Update-Database** komutunu çalıştırın. Bu, ınitialcreate geçişini veritabanına uygular. Gerçek geçiş herhangi bir değişiklik içermediğinden (bunları geçici olarak belirlediğimiz için), bu geçişin zaten uygulandığını belirten \_ @ no__t-1MigrationsHistory tablosuna yalnızca bir satır ekler.
4.  Up yöntemindeki kodun açıklamasını kaldırın. Bu, geçiş gelecekteki veritabanlarına uygulandığında, yerel veritabanında zaten var olan şemanın geçişler tarafından oluşturulacağı anlamına gelir.

## <a name="things-to-be-aware-of"></a>Dikkat edilmesi gerekenler

Mevcut bir veritabanına yönelik geçişleri kullanırken bilmeniz gereken birkaç nokta vardır.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Varsayılan/hesaplanan adlar varolan şemayla eşleşmeyebilir

Geçişler, bir geçişleri yapılandırırken sütun ve tablo adlarını açıkça belirler. Ancak, geçişler uygulanırken geçiş için varsayılan bir adı hesaplayan diğer veritabanı nesneleri vardır. Bu dizinler ve yabancı anahtar kısıtlamalarını içerir. Mevcut bir şemayı hedeflerken, bu hesaplanan adlar veritabanınızda gerçekten mevcut olan ile eşleşmeyebilir.

Bunun ne zaman farkında olmanız gerektiği hakkında bazı örnekler aşağıda verilmiştir:

**Kullandıysanız ' seçeneği bir: Mevcut şemayı başlangıç noktası olarak kullan: 3. Adım:**

-   Modelinizdeki gelecekteki değişiklikler farklı şekilde adlandırılan veritabanı nesnelerinden birinin değiştirilmesini veya bırakılmasını gerektiriyorsa, doğru adı belirtmek için yapı iskelesi geçişini değiştirmeniz gerekir. Geçişler API 'Leri, bunu yapmanıza olanak sağlayan isteğe bağlı bir ad parametresine sahiptir.
    Örneğin, var olan şemanız, IndexFk @ no__t-0Blogıd adlı bir dizine sahip blogID yabancı anahtar sütunu içeren bir post tablosuna sahip olabilir. Ancak, varsayılan olarak geçişler bu dizinin x @ no__t-0Blogıd olarak adlandırıldığını bekler. Modelinizde bu dizini bırakmaya neden olan bir değişiklik yaparsanız, IndexFk @ no__t-0Blogıd adını belirtmek için scafkatlanmış DropIndex çağrısını değiştirmeniz gerekir.

**' seçeneğini kullandıysanız: Adım 3 ' te boş veritabanını başlangıç noktası olarak kullan:**

-   Geçişler, hatalı adları kullanarak dizin ve yabancı anahtar kısıtlamalarını bırakmaya çalışacağından, ilk geçişin (yani boş bir veritabanına geri döndürülmesi), yerel veritabanınıza karşı çalışma yöntemini çalıştırmaya çalışmak başarısız olabilir. Bu, ilk geçişin up yöntemi kullanılarak diğer veritabanları sıfırdan oluşturulduğundan, yalnızca yerel veritabanınızı etkiler.
    Mevcut yerel veritabanınızı boş bir duruma düşürmek isterseniz, veritabanını bırakarak veya tüm tabloları bırakarak bunu el ile yapmak en kolay yoldur. Bu ilk kez düşürme sonrasında tüm veritabanı nesneleri varsayılan adlarla yeniden oluşturulacaktır, bu nedenle bu sorun kendisini tekrar sunmaz.
-   Modelinizdeki gelecekteki değişiklikler farklı şekilde adlandırılan veritabanı nesnelerinden birinin değiştirilmesini veya bırakılmasını gerektiriyorsa, bu adlar varsayılanlar ile eşleşmediğinden, mevcut yerel veritabanınıza karşı çalışmaz. Ancak, geçişler tarafından seçilen varsayılan adları kullandıklarından ' sıfırdan ' ' sıfırdan ' oluşturulan veritabanlarına karşı çalışır.
    Bu değişiklikleri yerel var olan veritabanınızda el ile yapabilir veya geçiş yapmayı, diğer makinelerde olduğu gibi, veritabanınızı sıfırdan yeniden oluşturmasını sağlayabilirsiniz.
-   İlk geçişinizin up yöntemi kullanılarak oluşturulan veritabanları, dizinler ve yabancı anahtar kısıtlamaları için hesaplanmış varsayılan adlar kullanılacak olduğundan, yerel veritabanından biraz farklı olabilir. Geçiş, yabancı anahtar sütunlarında varsayılan olarak dizin oluşturacak şekilde ek dizinler de alabilir; bu durum özgün yerel veritabanınızda bu durum olmayabilir.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Tüm veritabanı nesneleri modelde temsil edilmez

Modelinize dahil olmayan veritabanı nesneleri geçişler tarafından işlenmeyecektir. Bu, görünümleri, saklı yordamları, izinleri, modelinizin bir parçası olmayan tabloları, ek dizinleri vb. içerebilir.

Bunun ne zaman farkında olmanız gerektiği hakkında bazı örnekler aşağıda verilmiştir:

-   ' Adım 3 ' te seçtiğiniz seçenekten bağımsız olarak, modelinizdeki gelecekteki değişiklikler bu ek nesnelerin değiştirilmesini veya bırakılmasını gerektiriyorsa, bu değişiklikleri yapmak için bu değişiklikleri bilmez. Örneğin, üzerinde ek bir dizin olan bir sütunu bırakırsanız, geçişler dizini bırakmayı bilmez. Bunu, yapı iskelesi geçişine el ile eklemeniz gerekir.
-   ' Seçeneği Iki kullandıysanız: Boş veritabanını başlangıç noktası olarak kullan ', bu ek nesneler ilk geçişinizin up yöntemi tarafından oluşturulmayacak.
    İsterseniz, bu ek nesnelerle ilgilenmek için yukarı ve aşağı yöntemlerini değiştirebilirsiniz. Geçişler API 'sinde, görünümler gibi yerel olarak desteklenmeyen nesneler için, [SQL](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) metodunu kullanarak ham SQL 'i oluşturabilir/bırakabilirsiniz.
