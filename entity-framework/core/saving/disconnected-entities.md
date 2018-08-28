---
title: Bağlantısı kesilmiş varlıklar - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 51367d2619b1943c300f8954123f70b909ad96e7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994405"
---
# <a name="disconnected-entities"></a>Bağlantısı kesilmiş varlıklar

DbContext örneği veritabanından döndürülen varlıkları otomatik olarak izler. Bu varlıklar için yapılan değişiklikler SaveChanges çağrılır ve gerektiği şekilde veritabanını güncelleştirilecek algılanır. Bkz: [temel Kaydet](basic.md) ve [ilgili verileri](related-data.md) Ayrıntılar için.

Ancak, bazen varlıklar bir bağlam örneğini kullanma ve ardından farklı bir örneği kullanılarak kaydedilmiş sorgulanır. Bu genellikle "bağlantısız" senaryolarında burada varlıkları sorgulanan, istemciye gönderilen, değişiklik, bir istek sunucuya geri gönderilen ve ardından kaydedilebilir bir web uygulaması gibi gerçekleşir. Bu durumda, ikinci bağlam (güncelleştirme) mevcut veya yeni varlıklar olup olmadığını bilmek (eklenmesi gereken) gereksinimlerini örneği.

> [!TIP]  
> Bu makalenin görüntüleyebileceğiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) GitHub üzerinde.

> [!TIP]
> EF Core, yalnızca belirli bir birincil anahtar değeri ile herhangi bir varlık örneği takip edebilirsiniz. Bu bağlam boş olarak başlar, kısa süreli bir bağlam her birim iş için kullanılacak bir sorun olduğu durumda önlemek için en iyi yolu olarak bağlanmış varlıklar kişilikleri ve bu bağlam atıldı ve atılan kaydeder sahiptir.

## <a name="identifying-new-entities"></a>Yeni varlıklar tanımlayan

### <a name="client-identifies-new-entities"></a>İstemci yeni varlıklar tanımlayan

İstemci sunucuya varlık yeni veya mevcut olup olmadığını bildirir uğraşmanız basit durumdur. Örneğin, genellikle yeni bir varlık eklemek için mevcut bir varlığı güncelleştirmek için istekte farklı isteğidir.

Bu bölümün geri kalanında durumları kapsayan burada da eklemek veya güncelleştirmek başka bir şekilde karar vermek gerekli.

### <a name="with-auto-generated-keys"></a>Otomatik olarak oluşturulan anahtarları

Otomatik olarak oluşturulan bir anahtarın değeri, genellikle bir varlık eklenmesi veya güncelleştirilmesi gerekip gerekmediğini belirlemek için kullanılabilir. Anahtarı ayarlanmamış olması halinde (diğer bir deyişle, hala CLR varsayılan değeri null, sıfır, vb.) olan varlık yeni ve gereken ekleme. Öte yandan, anahtar değerini ayarlarsanız sonra zaten daha önce kaydedilmiş olması gerekir ve artık güncelleştirilmesi gerekiyor. Diğer bir deyişle, anahtar varsa, bir değer ve varlık, istemciye gönderilen sorgulandı dönüp kodu artık güncelleştirilecek.

Varlık türü bilinen bir unset anahtarı için denetimi daha kolaydır:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Ancak, EF ayrıca herhangi bir varlık türü ve anahtar türü için bunu yapmak için yerleşik bir yolu vardır:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Varlıkları bağlam tarafından izlenen hemen sonra varlık Added durumda olsa bile anahtarları ayarlanır. Bu, bir grafik varlıkları ve her biri, bu tür olduğu gibi TrackGraph API'sini kullanarak ne yapılması gerektiğine karar verme geçiş sırasında yardımcı olur. Anahtar değeri yalnızca burada gösterilen şekilde kullanılmalıdır _önce_ varlık izlemek için bir çağrı yapılır.

### <a name="with-other-keys"></a>İle diğer anahtarlar

Başka bir mekanizma, anahtar değerlerini otomatik olarak oluşturulmaz yeni varlıklar belirlemek için gereklidir. Bu iki genel yaklaşım vardır:
 * Varlık için sorgu
 * İstemciden bir bayrak geçirin

Varlık için sorgu için hemen bulma yöntemi kullanın:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Bir istemciden bir bayrak geçirmek için tam kod göstermek için bu belgenin kapsamı dışındadır. Bir web uygulamasında bu genellikle farklı eylemler için farklı istek yapma veya bazı istek durumuna geçirmeden sonra denetleyicide ayıklama anlamına gelir.

## <a name="saving-single-entities"></a>Tek varlık kaydediliyor

Bir ekleme veya güncelleştirme gerektiği ve ardından uygun şekilde ekleme veya güncelleştirme kullanılabilir olup olmadığını bilinen varsa:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Ancak, anahtar değerlerini otomatik olarak oluşturulan varlık kullanıyorsa, güncelleştirme yöntemi için her iki durumda kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Güncelleştirme yöntemi, update, INSERT değil varlığın normal olarak işaretler. Ancak, varlık otomatik olarak oluşturulmuş bir anahtar varsa ve varlık bunun yerine otomatik olarak için işaretlenmiş sonra anahtar değer ayarlandı ekleyin.

> [!TIP]  
> Bu davranış EF Core 2.0 sürümünde kullanıma sunulmuştur. Önceki sürümler için her zaman açıkça ekleme veya güncelleştirme seçmek gereklidir.

Varlık anahtarları otomatik olarak oluşturulan kullanmayan sonra uygulamanın varlık eklenmesi veya güncelleştirilmesi karar vermeniz gerekir: Örnek:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Buradaki adımları şunlardır:
* Bul döndürür dediğimiz bu nedenle veritabanı zaten bu Kimliğe sahip blog içermiyor sonra null eklerseniz ekleme için işaretleyin.
* Bul varlığın döndürürse, veritabanında var ve bağlam artık mevcut varlık izleme
  * Ardından SetValues Bu istemciden gelen değişiklikleri varlığa tüm özelliklerin değerlerini ayarlamak için kullanırız.
  * Gerektiği şekilde güncelleştirilecek varlık SetValues çağrı işaretler.

> [!TIP]  
> Yalnızca SetValues izlenen varlık öğesindekilerle farklı değerlere sahip özellikleri değiştirilmiş olarak işaretlenmesine neden olur. Bu, gerçekten değişmiş olan sütunları güncelleştirme gönderildiğinde, güncelleştirilecek anlamına gelir. (Ve hiçbir şey değişmediyse, ardından hiçbir güncelleştirme hiç gönderilir.)

## <a name="working-with-graphs"></a>Grafikler ile çalışma

### <a name="identity-resolution"></a>Kimlik çözümlemesi

Yukarıda belirtildiği gibi EF Core yalnızca belirli bir birincil anahtar değeri ile herhangi bir varlık örneği takip edebilirsiniz. Grafiklerle çalışırken graf ideal olarak bu sabit tutulur ve içeriği tek bir birim iş için kullanılması gereken şekilde oluşturulmalıdır. Ardından grafiğin, yinelenenleri içeriyorsa, grafik EF birden çok örneği tek bir araya getirmek için göndermeden önce işlemek için gerekli olacaktır. Bu şekilde yinelemeleri sağlamlaştırmak olabildiğince çabuk çakışma önlemek için uygulama ardışık düzeninizde yapılmalıdır örnekleri çakışan değerler ve ilişkileri, sahip olduğu Önemsiz olmayabilir.

### <a name="all-newall-existing-entities"></a>Tüm yeni/tüm var olan varlıkları

Grafikler ile çalışmaya ilişkin bir örnek ekleme veya blog ilişkili gönderileri kendi koleksiyonunu birlikte güncelleştiriliyor. Graftaki tüm varlıkları eklenmesi gereken ya da tüm güncelleştirilmelidir ardından aynı tek varlıklar için yukarıda açıklandığı gibi işlemidir. Örneğin, bir grafiğini blog ve gönderi şu şekilde oluşturulur:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

şu şekilde eklenebilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

Eklenecek çağrı blog ve tüm gönderileri eklenecek işaretler.

Bir grafikteki tüm varlıkları güncelleştirilmesi gerekiyorsa, benzer şekilde, daha sonra güncelleştirme kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

Güncelleştirilecek blog ve tüm gönderileri işaretlenir.

### <a name="mix-of-new-and-existing-entities"></a>Yeni ve var olan varlıkları karışımı

Graf ekleme gerektiren varlıkları ve bu güncelleştirme gerektiren bir karışımını içeriyorsa bile otomatik olarak oluşturulan anahtarları ile güncelleştirme yeniden eklemeler ve güncelleştirmeler için kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Güncelleştirme, grafik, blog veya post, diğer tüm varlıklar için güncelleştirme olarak işaretlenir ancak bir anahtar değer kümesi olmaması durumunda ekleme için herhangi bir varlık olarak işaretler.

Olarak daha önce otomatik olarak oluşturulan anahtarları, kullanılmadığında bir sorgu ve bazı kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>İşleme siler

Delete beri işlemek zor olabilir, silinmesi gerektiğini genellikle bir varlığın olmaması anlamına gelir. Bu ayarı kullanarak çıkılacağını bir varlık gerçekten silinmesini yerine silinmiş olarak işaretlenmiş şekilde "geçici silme" kullanmaktır. Siler ve ardından güncelleştirmeleri ile aynı olur. Geçici silme kullanarak uygulanabilir [sorgu filtreleri](xref:core/querying/filters).

Silme işlemi için true, aslında graf farkı nedir gerçekleştirmek için sorgu deseninin bir uzantı kullanmak için yaygın bir düzen tutulmasıdır. Örneğin:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Dahili olarak, Ekle, ekleme ve güncelleştirme olup olmadığını onu (eklemek için) eklenen (güncelleştirmek için) değiştirilen işaretlenmelidir seçeceğine her varlık için Unchanged yapılan bir belirleme ile grafik çapraz geçişi kullanın (hiçbir şey yapma), veya silinen (silmek için). Bu mekanizma TrackGraph API aracılığıyla kullanıma sunulur. Örneğin, istemci bir grafik varlıkları geri gönderdiğinde, her varlığın nasıl işleneceğini belirten bazı bayrağını ayarlar varsayalım. TrackGraph, ardından bu bayrağı işlemek için kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Bayraklar yalnızca varlık örneğin kolaylık olması için bir parçası olarak gösterilir. Genelde bayrakları bir DTO veya istekte bulunan başka bir duruma parçası olacaktır.
