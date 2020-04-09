---
title: Bağlantısız Varlıklar - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
uid: core/saving/disconnected-entities
ms.openlocfilehash: 421531e68ac98c0553938f1c24892701f22fef3c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417599"
---
# <a name="disconnected-entities"></a>Bağlantısı kesilen varlıklar

DbContext örneği, veritabanından döndürülen varlıkları otomatik olarak izler. SaveChanges çağrıldığında bu varlıklarda yapılan değişiklikler algılanır ve veritabanı gerektiği gibi güncelleştirilir. Ayrıntılar için [Temel Kaydet](basic.md) ve [İlgili Verilere](related-data.md) bakın.

Ancak, bazen varlıklar tek bir bağlam örneği kullanılarak sorgulanır ve sonra farklı bir örnek kullanılarak kaydedilir. Bu genellikle varlıkların sorgulandığı, istemciye gönderildiği, değiştirildiği, istekte sunucuya geri gönderildiği ve sonra kaydedildiği bir web uygulaması gibi "bağlantısı kesilmiş" senaryolarda gerçekleşir. Bu durumda, ikinci bağlam örneğinin varlıkların yeni mi yoksa varolan mı (güncelleştirilmelidir) olup olmadığını bilmesi gerekir.

<!-- markdownlint-disable MD028 -->
> [!TIP]
> Bu makalenin [örneğini](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Disconnected/) GitHub'da görüntüleyebilirsiniz.

> [!TIP]
> EF Core, belirli bir birincil anahtar değeri olan herhangi bir varlığın yalnızca bir örneğini izleyebilir. Bu bir sorun olmasını önlemenin en iyi yolu, her bir çalışma birimi için bağlamın boş başlaması, ona bağlı varlıkların olması, bu varlıkların kaydedilmesi ve bağlamın bertaraf edilip atılması gibi kısa ömürlü bir bağlam kullanmaktır.
<!-- markdownlint-enable MD028 -->

## <a name="identifying-new-entities"></a>Yeni varlıkların tanımlanması

### <a name="client-identifies-new-entities"></a>İstemci yeni varlıkları tanımlar

Ele almak için en basit durumda istemci varlığı yeni veya varolan olup olmadığını sunucuya bildirir. Örneğin, genellikle yeni bir varlık ekleme isteği varolan bir varlığı güncelleştirme isteğifarklıdır.

Bu bölümün geri kalanı, eklemek veya güncelleştirmek için başka bir şekilde belirlemek için gerekli durumlarda kapsar.

### <a name="with-auto-generated-keys"></a>Otomatik oluşturulan anahtarlarla

Otomatik olarak oluşturulan anahtarın değeri genellikle bir varlığın eklenmesi veya güncelleştirilmesi gerekip gerekmediğini belirlemek için kullanılabilir. Anahtar ayarlanmadıysa (diğer bir deyişle, hala null, sıfır, vb CLR varsayılan değerine sahip), o zaman varlık yeni olmalı ve ekleme ihtiyacı vardır. Diğer taraftan, anahtar değeri ayarlanmışsa, daha önce kaydedilmiş olması gerekir ve şimdi güncelleştirilmesi gerekir. Başka bir deyişle, anahtarın bir değeri varsa, varlık sorgulandı, istemciye gönderildi ve şimdi güncellenmek üzere geri geldi.

Varlık türü bilindiğinde ayarlanmamış anahtarı denetlemek kolaydır:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewSimple)]

Ancak, EF de herhangi bir varlık türü ve anahtar türü için bunu yapmak için yerleşik bir şekilde vardır:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Varlıklar bağlam tarafından izlenir izlenmez, varlık Eklenen durumda olsa bile anahtarlar ayarlanır. Bu, varlıkların bir grafiğini dolaşırken ve TrackGraph API'sını kullanırken olduğu gibi her biriyle ne yapacağınız konusunda karar verirken yardımcı olur. Anahtar değeri, varlığı izlemek için herhangi bir arama yapılmadan _önce_ yalnızca burada gösterilen şekilde kullanılmalıdır.

### <a name="with-other-keys"></a>Diğer tuşlarla

Anahtar değerler otomatik olarak oluşturulmadığında yeni varlıkları tanımlamak için başka bir mekanizma gerekir. Bunun iki genel yaklaşımı vardır:

* Varlık için sorgu
* İstemciden bayrak geçir

Varlığı sorgulamak için Bul yöntemini kullanman:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Bir istemciden bayrak geçirmek için tam kodu göstermek için bu belgenin kapsamı dışındadır. Bir web uygulamasında, genellikle farklı eylemler için farklı isteklerde bulunmak veya istekteki bazı durumları geçmek ve denetleyicide ayıklamak anlamına gelir.

## <a name="saving-single-entities"></a>Tek varlıkları kaydetme

Bir ekleme veya güncelleştirme gerekip gerekmediği biliniyorsa, Ekle veya Güncelleştir uygun şekilde kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Ancak, varlık otomatik olarak oluşturulan anahtar değerlerini kullanıyorsa, güncelleştirme yöntemi her iki durum için de kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Güncelleştirme yöntemi normalde ekleme değil, güncelleştirme için varlık işaretler. Ancak, varlığın otomatik olarak oluşturulan bir anahtarı varsa ve anahtar değeri ayarlanmadıysa, varlık eklemek için otomatik olarak işaretlenir.

> [!TIP]  
> Bu davranış EF Core 2.0'da sunulmuştur. Önceki sürümler için her zaman açıkça Ekle veya Güncelleştir'i seçmek gerekir.

Varlık otomatik olarak oluşturulan anahtarları kullanmıyorsa, uygulama varlığın eklenmesi mi yoksa güncelleştirilmesi mi gerektiğine karar vermelidir: Örneğin:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Burada adımlar şunlardır:

* Bul döndürür null dönerse, veritabanı zaten bu kimliği içeren blog içermez, bu yüzden ekleme için işaretle diyoruz.
* Find bir varlığı döndürürse, veritabanında var olur ve bağlam şimdi varolan varlığı takip ediyor
  * Daha sonra, bu varlıktaki tüm özelliklerin değerlerini istemciden gelenlere ayarlamak için SetValues'i kullanırız.
  * SetValues çağrısı, gerektiğinde güncelleştirilecek varlığı işaretler.

> [!TIP]  
> SetDeğerleri yalnızca izlenen varlıktakilerle farklı değerlere sahip özellikleri değiştirilmiş olarak işaretler. Bu, güncelleştirme gönderildiğinde yalnızca gerçekten değiştirilen sütunların güncelleştirileceği anlamına gelir. (Ve hiçbir şey değişmediyse, o zaman hiçbir güncelleştirme gönderilmez.)

## <a name="working-with-graphs"></a>Grafiklerle çalışma

### <a name="identity-resolution"></a>Kimlik çözünürlüğü

Yukarıda belirtildiği gibi, EF Core yalnızca belirli bir birincil anahtar değeri olan herhangi bir varlığın bir örneğini izleyebilir. Grafiklerle çalışırken grafik ideal olarak bu değişmezin korunması ve bağlamın yalnızca bir çalışma birimi için kullanılması gibi oluşturulmalıdır. Grafik yinelemeler içeriyorsa, birden çok örneği tek bir örnekte birleştirmek için grafiği EF'ye göndermeden önce işlemek gerekir. Örneklerin çakışan değerler eve sahip olduğu bu önemsiz olmayabilir, bu nedenle çakışma çözümünü önlemek için uygulama ardışık bilgisayarınızda yinelenenleri en kısa sürede birleştirme yapılmalıdır.

### <a name="all-newall-existing-entities"></a>Tüm yeni/tüm mevcut varlıklar

Grafiklerle çalışmanın bir örneği, bir blogu ilişkili gönderikoleksiyonuyla birlikte eklemek veya güncelleştirmektir. Grafikteki tüm varlıklar eklenmelidir veya tümü güncelleştirilmelidir, işlem tek varlıklar için yukarıda açıklandığı gibi aynıdır. Örneğin, aşağıdaki gibi oluşturulan blogların ve gönderilerin grafiği:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

şu şekilde eklenebilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertGraph)]

Ekle'ye çağrı, blogu ve eklenecek tüm gönderileri işaretler.

Aynı şekilde, grafikteki tüm varlıkların güncellenmesi gerekiyorsa, Güncelleştirme kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#UpdateGraph)]

Blog ve tüm gönderileri güncellenmek üzere işaretlenecektir.

### <a name="mix-of-new-and-existing-entities"></a>Yeni ve mevcut varlıkların karışımı

Otomatik olarak oluşturulan anahtarlarla, grafik ekleme ve güncelleştirme gerektiren varlıkların bir karışımını içerse bile, Güncelleştirme hem ekler hem de güncelleştirmeler için yeniden kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Güncelleştirme, önemli bir değer kümesi yoksa, diğer tüm varlıklar güncelleştirme için işaretlenmiş ise ekleme için grafik, blog veya gönderideki herhangi bir varlığı işaretler.

Daha önce olduğu gibi, otomatik olarak oluşturulan anahtarları kullanmadığınızda, bir sorgu ve bazı işleme kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>İşleme siler

Silme, genellikle bir varlığın yokluğu silinmesi gerektiği anlamına geldiğinden, işlemek zor olabilir. Bununla başa çıkmanın bir yolu, varlığın gerçekten silinmek yerine silinmiş olarak işaretlendiğini "yumuşak silmeler" kullanmaktır. Siler sonra güncelleştirmeleri aynı olur. Yumuşak [silmesorgu filtreleri](xref:core/querying/filters)kullanılarak uygulanabilir.

Gerçek siler için, ortak bir desen aslında bir grafik diff ne gerçekleştirmek için sorgu deseni bir uzantısı kullanmaktır. Örneğin:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>Trackgraph

Dahili olarak, Ekle, Ekle ve Güncelleştir, her varlık için eklenen (eklemek için), Değiştirilen (güncelleştirmek için), Değiştirilmeden (hiçbir şey yapmamak) veya Silinmiş (silinecek) olarak işaretlenip işaretlenmemesi gerektiği konusunda yapılan bir kararlılıkla grafik-traversal kullanın. Bu mekanizma TrackGraph API ile ortaya çıkarır. Örneğin, istemci varlıkların bir grafik geri gönderdiğinde, nasıl işlenmesi gerektiğini belirten her varlık üzerinde bazı bayrak ayarlar varsayalım. TrackGraph daha sonra bu bayrağı işlemek için kullanılabilir:

[!code-csharp[Main](../../../samples/core/Saving/Disconnected/Sample.cs#TrackGraph)]

Bayraklar yalnızca örneğin basitliği için varlığın bir parçası olarak gösterilir. Genellikle bayraklar, isteğe dahil edilmiş bir DTO'nun veya başka bir durum durumunun bir parçası olur.
