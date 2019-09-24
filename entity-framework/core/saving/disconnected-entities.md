---
title: Bağlantısı kesilen varlıklar-EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 070f2ad396ec21858096c29413ac80bdf8547328
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197804"
---
# <a name="disconnected-entities"></a>Bağlantısı kesilen varlıklar

DbContext örneği veritabanından döndürülen varlıkları otomatik olarak izler. Bu varlıklarda yapılan değişiklikler, SaveChanges çağrıldığında ve veritabanı gerektiği şekilde güncelleniyorsa algılanır. Ayrıntılar için bkz. [temel kaydetme](basic.md) ve [ilgili veriler](related-data.md) .

Ancak, bazen varlıklar bir bağlam örneği kullanılarak sorgulanır ve sonra farklı bir örnek kullanılarak kaydedilir. Bu genellikle varlıkların sorgulandığı, istemciye gönderildiği, değiştirildiği, bir istekte sunucuya geri gönderildiği ve ardından kaydedildiği bir Web uygulaması gibi "bağlantısı kesik" senaryolarda meydana gelir. Bu durumda, ikinci bağlam örneğinin varlıkların yeni (eklenmelidir) mi yoksa mevcut mi (güncelleştirilmesi gerekir) olduğunu bilmeleri gerekir.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) GitHub üzerinde.

> [!TIP]
> EF Core, belirli bir birincil anahtar değeri olan herhangi bir varlığın yalnızca bir örneğini izleyebilir. Bu sorunu önlemenin en iyi yolu, her iş birimi için, içeriğin boş başlaması, kendisine iliştirilmiş varlıklar olması, bu varlıkları kaydettiğinden ve sonra bağlamın atılacağını ve atılmasına yönelik kısa süreli bir bağlam kullanmaktır.

## <a name="identifying-new-entities"></a>Yeni varlıkları tanımlama

### <a name="client-identifies-new-entities"></a>İstemci yeni varlıkları tanımlar

En basit durum, istemcinin, varlığın yeni mi yoksa mevcut mi olduğunu bildirir. Örneğin, genellikle yeni bir varlık ekleme isteği, var olan bir varlığı güncelleştirmek için istekten farklıdır.

Bu bölümün geri kalanında, ekleme veya güncelleştirme yapıp etmeksizin başka bir şekilde belirlenmesi gereken durumlar ele alınmaktadır.

### <a name="with-auto-generated-keys"></a>Otomatik olarak oluşturulan anahtarlarla

Otomatik olarak oluşturulan bir anahtarın değeri, genellikle bir varlığın eklenmesi veya güncelleştirilmesi gerekip gerekmediğini belirlemede kullanılabilir. Anahtar ayarlanmamışsa (diğer bir deyişle, hala CLR varsayılan değeri null, sıfır vb.), varlık yeni ve ekleme gerekiyor olmalıdır. Diğer taraftan, anahtar değeri ayarlandıysa, daha önce kaydedilmiş ve şimdi güncelleştirilmesi gerekiyor olmalıdır. Diğer bir deyişle, anahtarın bir değeri varsa, varlık sorgulanmıştı, istemciye gönderilir ve şimdi güncelleştirilmesini geri gelir.

Varlık türü bilindiğinde, bir ayarı kontrol etmek kolaydır:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Bununla birlikte, EF aynı zamanda herhangi bir varlık türü ve anahtar türü için yerleşik bir yönteme sahiptir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Varlık eklenen durumda olsa bile, varlıklar bağlam tarafından izlendiğinde, anahtarlar hemen ayarlanır. Bu, bir varlık grafiğinde geçiş yaparken ve her biriyle ne yapacağına karar verirken (TrackGraph API 'SI kullanılırken olduğu gibi) yardımcı olur. Anahtar değeri yalnızca varlığı izlemek için herhangi bir çağrı _yapılmadan önce_ burada gösterildiği gibi kullanılmalıdır.

### <a name="with-other-keys"></a>Diğer anahtarlarla

Anahtar değerleri otomatik olarak oluşturulmadığında yeni varlıkları tanımlamak için başka bir mekanizma gerekir. Bunun için iki genel yaklaşım vardır:
 * Varlık için sorgu
 * İstemciden bir bayrak geçirin

Varlığı sorgulamak için Find metodunu kullanmanız yeterlidir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Bir istemciden bayrak geçirmenin tam kodunu göstermek için bu belgenin kapsamı dışındadır. Bir Web uygulamasında, genellikle farklı eylemler için farklı istekler yapılması veya istekte bir durum iletilmesi ve sonra denetleyiciye ayıklanması anlamına gelir.

## <a name="saving-single-entities"></a>Tek varlıkları kaydetme

Bir ekleme veya güncelleştirme gerekip gerekmediğini bilindiğinde, ekleme veya güncelleştirme uygun şekilde kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Ancak, varlık otomatik üretilen anahtar değerlerini kullanıyorsa, her iki durumda da güncelleştirme yöntemi kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Update yöntemi, normal olarak güncelleştirme için varlığı, INSERT değil olarak işaretler. Ancak, varlık otomatik olarak oluşturulmuş bir anahtara sahipse ve anahtar değeri ayarlanmamışsa, bu durumda varlık otomatik olarak ekleme için işaretlenir.

> [!TIP]  
> Bu davranış EF Core 2,0 ' de tanıtılmıştı. Daha önceki sürümlerde, açıkça Ekle veya Güncelleştir ' i seçmek için her zaman gereklidir.

Varlık otomatik olarak oluşturulan anahtarları kullanmıyor ise, uygulamanın varlığın eklenip eklenmeyeceğine veya güncelleştirilmesine karar vermelidir: Örneğin:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Buradaki adımlar şunlardır:
* Find null döndürürse, veritabanı bu KIMLIĞE sahip blogu zaten içermez, bu nedenle ekleme öğesini çağırıyoruz.
* Bul bir varlık döndürürse, veritabanında bulunur ve bağlam artık var olan varlığı izliyor
  * Daha sonra bu varlıktaki tüm özelliklerin değerlerini istemciden gelen değerlere ayarlamak için SetValues kullanılır.
  * SetValues çağrısı, varlığı gerektiği şekilde güncelleştiren şekilde işaretleyecek.

> [!TIP]  
> SetValues yalnızca, izlenen varlıktaki farklı değerlere sahip özellikleri değiştirilmiş olarak işaretlenecektir. Bu, güncelleştirme gönderildiğinde yalnızca gerçekten değiştirilen sütunlar güncelleştirilecek anlamına gelir. (Ve hiçbir şey değiştirilmezse hiçbir güncelleştirme gönderilmez.)

## <a name="working-with-graphs"></a>Grafiklerle çalışma

### <a name="identity-resolution"></a>Kimlik çözümlemesi

Yukarıda belirtildiği gibi, EF Core yalnızca belirli bir birincil anahtar değeri olan herhangi bir varlığın bir örneğini izleyebilir. Grafiklerle çalışırken, grafik ideal bir şekilde oluşturulmalıdır ve bağlam yalnızca bir iş birimi için kullanılmalıdır. Grafik yinelenen öğeler içeriyorsa, birden çok örneği birleştirmek için bir grafik için EF 'e göndermeden önce grafiğin işlenmesi gerekir. Bu, örneklerin çakışan değerler ve ilişkilerin bulunduğu önemsiz olmayabilir, bu nedenle yinelenenleri birleştirme, uygulama ardışık düzeninde çakışma çözümünden kaçınmak için mümkün olan en kısa sürede yapılmalıdır.

### <a name="all-newall-existing-entities"></a>Tüm yeni/tüm mevcut varlıklar

Grafiklerle çalışmaya bir örnek, bir blogu ilişkili gönderilerin koleksiyonuyla birlikte ekleme veya güncelleştirme. Grafikteki tüm varlıkların eklenmesi veya tümünün güncellenmesi gerekiyorsa, işlem yukarıda açıklanan tek varlıklar için aynı olur. Örneğin, aşağıdaki gibi oluşturulan blogların ve gönderilerin bir grafiği:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

şöyle eklenebilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

Eklenecek çağrı blogunu ve eklenecek tüm gönderileri işaretleyecek.

Benzer şekilde, bir grafikteki tüm varlıkların güncelleştirilmesi gerekiyorsa, güncelleştirme kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog ve tüm gönderimler güncellenmeyecek şekilde işaretlenir.

### <a name="mix-of-new-and-existing-entities"></a>Yeni ve mevcut varlıkların karışımı

Otomatik olarak oluşturulan anahtarlarla, grafik ekleme ve güncelleştirme gerektiren varlıkların bir karışımını içerse bile, güncelleştirme her iki ekleme ve güncelleştirme için de kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Güncelleştirme, bir anahtar değer kümesi yoksa, diğer tüm varlıklar güncelleştirme için işaretlenirken grafik, blog veya gönderi içindeki herhangi bir varlığı, ekleme için bir anahtar değeri ayarlanmamışsa işaretler.

Daha önce olduğu gibi, otomatik olarak oluşturulan anahtarlar kullanılmazsa bir sorgu ve bir işlem kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Silmeleri işleme

Genellikle bir varlık yokluğu, silinmesi gerektiği anlamına gelir. Bunu yapmanın bir yolu, varlığın gerçekten silinmesi yerine silinmiş olarak işaretlenmesi için "geçici silme" kullanmaktır. Sonra siler, güncelleştirmelerle aynı olur. Geçici silme, [sorgu filtreleri](xref:core/querying/filters)kullanılarak uygulanabilir.

Doğru silme işlemleri için genel bir model, temel bir grafik farkı olan bir sorgu deseninin uzantısını kullanmaktır. Örneğin:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Dahili, ekleme, Iliştirme ve güncelleştirme, her varlık için eklenen (ekleme), değiştirme (güncelleştirme), değiştirilmemiş (hiçbir şey yapma) veya silinmiş (silmek için) olarak işaretlenme gibi bir belirleme ile grafik çapraz geçişi kullanın. Bu mekanizma, TrackGraph API 'SI aracılığıyla sunulur. Örneğin, istemci bir varlık grafiğini geri gönderdiğinde, her bir varlıkta nasıl işleneceğini gösteren bir bayrak ayarladığını varsayalım. TrackGraph, daha sonra bu bayrağı işlemek için kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

Bayraklar, örneğin basitliği için varlığın bir parçası olarak gösterilir. Genellikle bayraklar bir DTO veya istekte bulunan başka bir durumun parçası olur.
