---
title: Bağlantısı kesilmiş varlıkları - EF çekirdek
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: dae6265fe2b9dd2cd5da78dc69d081950f374436
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754398"
---
# <a name="disconnected-entities"></a>Bağlantısı kesilmiş varlıklar

DbContext örnek veritabanı tarafından döndürülen varlıkları otomatik olarak izler. Bu varlıklar için yapılan değişiklikler sonra SaveChanges olarak adlandırılır ve veritabanı gerektiğinde güncelleştirileceği zaman algılanacak. Bkz: [temel Kaydet](basic.md) ve [ilgili verileri](related-data.md) Ayrıntılar için.

Ancak, bazen varlıklar bir bağlam örneğini kullanarak ve farklı bir örneği kullanılarak kaydedilmiş seçmeleri istenir. Bu genellikle "bağlantısız" senaryolarda burada varlıkları sorgulanan, istemciye gönderilen, değişiklik, bir sunucuya geri gönderilen ve sonra kaydedilmiş bir web uygulaması gibi gerçekleşir. Bu durumda, ikinci bağlam (güncelleştirilmelidir) mevcut veya yeni varlıklardır olup olmadığını bilmek (eklenecek) gereksinimlerini örneği.

> [!TIP]  
> Bu makalenin görüntüleyebilirsiniz [örnek](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) github'da.

> [!TIP]
> EF çekirdek yalnızca bir örneğini belirtilen birincil anahtar değerine sahip herhangi bir varlığa izleyebilirsiniz. Bu bağlamda boş başlatır gibi kısa süreli bir bağlam her--iş birimi için kullanmak üzere bir sorun olduğu durumda önlemek için en iyi yolu ekli varlıklar bu varlıkların ve bağlam atıldı ve atılan kaydeder sahiptir.

## <a name="identifying-new-entities"></a>Yeni varlıklar tanımlama

### <a name="client-identifies-new-entities"></a>İstemci yeni varlıklar tanımlar

İstemci sunucuya varlık yeni veya mevcut olup olmadığını bildirir uğraşmanız basit durumdur. Örneğin, genellikle yeni bir varlık eklemek için var olan bir varlığı güncelleştirmek için istekten farklı isteğidir.

Bu bölüm geri kalanı durumları kapsayan Burada, diğer herhangi bir yolla eklemek veya güncelleştirmek karar vermek gerekli.

### <a name="with-auto-generated-keys"></a>Otomatik olarak oluşturulan anahtarlarla

Otomatik olarak oluşturulan bir anahtarın değeri, genellikle bir varlık eklenmesi veya güncelleştirilmesi gerekip gerekmediğini belirlemek için kullanılabilir. Anahtar olmayan varsa varlık yeni olmalıdır ve gereken ekleme (yani hala bulunduğu CLR varsayılan değeri null, sıfır, vb.), ayarlanmış bırakıldı. Diğer taraftan, anahtar değeri ayarlarsanız sonra zaten daha önce kaydedilmiş gerekir ve şimdi güncelleştirilmesi gerekiyor. Diğer bir deyişle, anahtarı varsa, bir değer sonra varlık, istemciye gönderilen sorgulandı ve şimdi güncelleştirilmesi döndürülmesini.

Varlık türü bilinen bir unset anahtarı için denetimi kolaydır:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Ancak, EF ayrıca herhangi bir varlık türü ve anahtar türü için bunu yapmanın yerleşik bir yolu vardır:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Varlık bağlamı tarafından izlenen hemen varlık Added durumunda olsa bile anahtarları ayarlanır. Bu varlıkları ve her biri, bu tür olduğu gibi TrackGraph API'si kullanılırken yapmanız gerekenler karar grafiği çapraz geçiş yapan zaman yardımcı olur. Anahtar değeri yalnızca aşağıda gösterildiği şekilde kullanılmalıdır _önce_ varlık izlemek için bir çağrı yapılır.

### <a name="with-other-keys"></a>Diğer anahtarları

Başka bir mekanizma anahtarı değerlerini otomatik olarak oluşturulmaz yeni varlıklar belirlemek için gereklidir. Bu iki genel yaklaşım vardır:
 * Varlık için sorgu
 * İstemciden bir bayrak geçirin

Varlık için sorgu için hemen Find yöntemi kullanın:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Bir istemciden bir bayrak geçirme için tam kod göstermek için bu belgenin kapsamı dışındadır. Bir web uygulamasını, genellikle farklı eylemler için farklı istekleri yapan veya istek bazı durumda geçirme ardından denetleyicide ayıklanıyor anlamına gelir.

## <a name="saving-single-entities"></a>Tek varlıkları kaydetme

Bir INSERT veya update gereklidir ve sonra uygun şekilde ekleme veya güncelleştirme kullanılabilir olup olmadığını bilinen ise:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Bununla birlikte, varlık otomatik olarak oluşturulan anahtar değerlerini kullanıyorsa, güncelleştirme yöntemi için her iki durumda kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Update yöntemi güncelleştirmesi değil Ekle varlık normal olarak işaretler. Bununla birlikte, varlık otomatik olarak oluşturulan bir anahtar varsa ve varlık bunun yerine otomatik olarak için işaretlenmiş sonra herhangi bir anahtar değer ayarlandı ekleyin.

> [!TIP]  
> Bu davranış EF çekirdek 2. 0'sunulmuştur. Önceki sürümleri için her zaman açık olarak ekleme veya güncelleştirme seçmek gereklidir.

Varlık anahtarları otomatik olarak oluşturulan kullanmayan sonra uygulama varlık eklenen veya güncelleştirilen karar vermeniz gerekir: Örneğin:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Burada adımlar şunlardır:
* Bul döndürür diyoruz şekilde veritabanı zaten bu Kimliğe sahip blog içermiyor. ardından null eklerseniz, ekleme için işaretleyin.
* Bul bir varlık döndürürse, veritabanında var ve bağlam şimdi varolan varlık izleme
  * Ardından SetValues istemciden gelen değişiklikleri bu varlık üzerinde tüm özelliklerinin değerlerini ayarlamak için kullanırız.
  * SetValues çağrısı gerektiğinde güncelleştirilecek varlık işaretler.

> [!TIP]  
> Yalnızca SetValues izlenen varlık de için farklı değerlere sahip özellikleri değiştirilemez olarak işaretlenmesine neden olur. Bu güncelleştirme gönderildiğinde, aslında değişmiş olan sütunları güncelleştirilecek anlamına gelir. (Ve hiçbir şey değiştiyse, ardından güncelleştirme hiç gönderilecek.)

## <a name="working-with-graphs"></a>Grafikleri ile çalışma

### <a name="identity-resolution"></a>Kimlik çözümleme

Yukarıda belirtildiği gibi EF çekirdek yalnızca bir örneğini belirtilen birincil anahtar değerine sahip herhangi bir varlığa izleyebilirsiniz. Grafiklerle çalışırken grafiği ideal olarak bu değişmeyen saklanır ve bağlam yalnızca bir birim çalışma için kullanılması gereken şekilde oluşturulmalıdır. Ardından grafiği çoğaltmaları içeriyorsa, bu grafiği tek birden çok örneği birleştirilecek EF göndermeden işlemek gerekli olacaktır. Bu yani sağlamlaştırmak çoğaltmaları mümkün olan en kısa sürede çakışma çözümü önlemek için uygulama ardışık düzeninizde yapılmalıdır örnekleri çakışan değerler ve ilişkileri, sahip olduğu Önemsiz olmayabilir.

### <a name="all-newall-existing-entities"></a>Tüm yeni/tüm var olan varlıkları

Grafikleri ile çalışmaya ilişkin bir örnek ekleme veya blog ilişkili gönderileri kendi koleksiyonunu birlikte güncelleştiriliyor. Grafik alanındaki tüm varlıkları eklenecek ya da tüm güncelleştirilmesi, ardından aynı tek varlıklar için yukarıda açıklandığı gibi işlemidir. Örneğin, bir grafik bloglar ve şöyle oluşturulan gönderileri:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

Bu gibi eklenebilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

Ekle çağrısı blog ve eklenecek tüm gönderileri işaretler.

Bir grafik alanındaki tüm varlıkları güncelleştirilmesi gerekiyorsa, benzer şekilde, daha sonra güncelleştirme kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog ve tüm gönderileri güncelleştirilmesi için işaretlenir.

### <a name="mix-of-new-and-existing-entities"></a>Yeni ve var olan varlıkları karışımı

Grafik ekleme gerektiren varlıkları ve güncelleştirme gerektirenler bir karışımını içeriyorsa bile otomatik olarak oluşturulan anahtarlarla güncelleştirme yeniden ekler ve güncelleştirmeler için kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Güncelleştirme grafiği, blog veya diğer tüm varlıkları güncelleştirmesi işaretlenmiş sırada bir anahtar değer kümesi yoksa, ekleme için post herhangi bir varlığa işaretler.

Olarak daha önce otomatik olarak oluşturulan anahtarları kullanmadığınızda bir sorgu ve bazı işleme kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Siler işleme

Delete itibaren işlemek hassas olabilir, silinmesi gerektiğini genellikle bir varlık olmaması anlamına gelir. Bu işlem için bir yol varlık gerçekte silinmesini yerine silinmiş olarak işaretlenmiş şekilde "yumuşak siler" kullanmaktır. Siler ve ardından güncelleştirmeleri ile aynı olur. Kullanarak yazılım siler uygulanabilir [sorgu filtreleri](xref:core/querying/filters).

Silme işlemi için true, genel bir desen uzantı sorgu deseni temelde grafik diff nedir gerçekleştirmek için kullanmaktır Örneğin:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Dahili olarak, Ekle, ekleme ve güncelleştirme olup olmadığını, (eklemek için) eklenen (güncelleştirmek için) değiştirilen işaretlenmelidir ilişkin her bir varlık için Unchanged yapılan belirleme ile grafik geçişi kullanın (hiçbir şey yapma), veya silinen (silmek için). Bu mekanizma TrackGraph API sunulur. Örneğin, istemci bir grafik varlıkların geri gönderdiğinde, nasıl işleneceğini belirten her varlığın bazı bayrağını ayarlar varsayalım. TrackGraph sonra bu bayrak işlemek için kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Bayrakları yalnızca varlık örneğin kolaylık olması için bir parçası olarak gösterilir. Genelde bayrakları bir DTO veya istekte bulunan başka bir duruma parçası olacaktır.
