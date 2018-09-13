---
title: Code First Migrations ile varolan bir veritabanını - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f0cc4f93-67dd-4664-9753-0a9f913814db
ms.openlocfilehash: 77370ec7d922b8324b924a0b4aca3e58f5ec6066
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490577"
---
# <a name="code-first-migrations-with-an-existing-database"></a>Code First Migrations ile mevcut bir veritabanı
> [!NOTE]
> **EF4.3 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb. Entity Framework 4.1 içinde kullanıma sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Bu makale, Entity Framework tarafından oluşturulmadıysa var olan veritabanı ile Code First Migrations'ı kullanarak kapsar.

> [!NOTE]
> Bu makalede, temel senaryolarda Code First Migrations nasıl kullanılacağını varsayar. Sizin sonra okumak ihtiyacınız olacak [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) devam etmeden önce.

## <a name="screencasts"></a>Ekran kayıtları

Bu makaleyi okuyun bir yayını daha yerine izleyin, bu makalede aynı içeriğe aşağıdaki iki video kapsar.

### <a name="video-one-migrations---under-the-hood"></a>Bir video: "Başlık altında - geçişler"

[Bu yayını](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) geçişleri nasıl izlediği kapsar ve modeli hakkında bilgi modeli değişikliklerini algılamak için kullanır.

### <a name="video-two-migrations---existing-databases"></a>Video iki: "Geçişler - var olan veritabanlarını"

Önceki video kavramları güncelleştirmelerle [bu yayını](http://channel9.msdn.com/blogs/ef/migrations-existing-databases) etkinleştirmek ve var olan bir veritabanı geçişlerini kullanmak nasıl etkinleştireceğinizi de açıklar.

## <a name="step-1-create-a-model"></a>1. adım: bir model oluşturma

İlk adımınız mevcut veritabanını hedefleyen Code First bir model oluşturmak olacaktır. [Var olan bir veritabanına ilk kod](~/ef6/modeling/code-first/workflows/existing-database.md) konu bunun nasıl yapılacağı hakkında ayrıntılı kılavuz sağlar.

>[!NOTE]
> Veritabanı şema değişiklikleri gerektirecek modeldeki herhangi bir değişiklik yapmadan önce bu konudaki adımları izlemeden daha önemlidir. Model eşitlenmiş olması için aşağıdaki adımları gerektirir veritabanı şeması.

## <a name="step-2-enable-migrations"></a>2. adım: Migrations'ı etkinleştirme

Sonraki adım, geçiş etkinleştirmektir. Çalıştırarak bunu yapabilirsiniz **etkinleştir geçişleri** Paket Yöneticisi konsolunda komutu.

Bu komut çözümünüzde geçişleri adlı bir klasör oluşturun ve tek bir sınıf adlı yapılandırma içinde yerleştirin. Yapılandırma geçişleri uygulamanız için daha fazla bilgi bulabilirsiniz yapılandırdığınız sınıftır [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) konu.

## <a name="step-3-add-an-initial-migration"></a>3. adım: bir başlangıç geçiş Ekle

Geçiş oluşturulur ve uygulanan sonra bunlar uygulamak isteyebilirsiniz yerel veritabanı diğer veritabanlarına değişir. Örneğin, yerel veritabanınızı bir test veritabanı olabilir ve sonuçta da üretim veritabanına değişiklikleri uygulamak isteyebilirsiniz ve/veya diğer geliştiriciler veritabanları test edin. Bu adım için iki seçenek vardır ve diğer veritabanlarını şemasını boş veya şu anda yerel veritabanı şeması eşleşen olup olmadığını seçmeniz gerekir bir bağlıdır.

-   **Bir seçenek: var olan bir şema, başlangıç noktası olarak kullanın.** Yerel veritabanınızı şu anda olduğu gibi geçişleri gelecekte uygulanacak diğer veritabanları aynı şemaya sahip olduğunda bu yaklaşımı kullanmanız. Örneğin, bu yerel test veritabanınız şu anda v1 üretim veritabanınız ile eşleşen ve daha sonra bu geçişlerini üretim veritabanınızı v2 için güncelleştirme uygulanır kullanabilirsiniz.
-   **İki seçenek: boş veritabanı, başlangıç noktası olarak kullanın.** Geçişleri gelecekte uygulanacak diğer veritabanlarına boş (veya henüz var olmaması olduğunda), bu yaklaşımı kullanmanız gerekir. Örneğin, bir test veritabanı kullanarak uygulamanızı geliştirmeye başladı, ancak geçişleri ve kullanmadan daha sonra bir üretim veritabanının sıfırdan oluşturmak isteyeceksiniz bu kullanabilirsiniz.

### <a name="option-one-use-existing-schema-as-a-starting-point"></a>Bir seçenek: var olan şema başlangıç noktası olarak kullanın.

Code First geçişleri modeline değişikliklerini algılamak için modelinin en son geçiş depolanan anlık görüntüsünü kullanır (Bu konuda hakkında ayrıntılı bilgi bulabilirsiniz [Code First Migrations ekip ortamlarında](~/ef6/modeling/code-first/migrations/teams.md)). Veritabanları, geçerli model şeması zaten sahip olduğunuzu varsaymaktadır kullanacağız olduğundan, bir anlık görüntü olarak geçerli modeli olan bir boş (İşlemsiz) geçiş oluşturacağız.

1.  Çalıştırma **Ekle geçiş InitialCreate – IgnoreChanges** Paket Yöneticisi konsolunda komutu. Bu boş bir geçiş geçerli modeliyle anlık görüntü olarak oluşturur.
2.  Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu. Bu, InitialCreate geçiş veritabanına uygulanır. Gerçek geçiş herhangi bir değişiklik içermiyor olduğundan, yalnızca bir satır için ekleyeceksiniz \_ \_bu geçiş zaten uygulanmış olan tablo MigrationsHistory belirten.

### <a name="option-two-use-empty-database-as-a-starting-point"></a>İki seçenek: boş veritabanı bir başlangıç noktası olarak kullanın.

Bu senaryoda sıfırdan – bizim yerel veritabanında mevcut olan tablolar da dahil olmak üzere tüm veritabanı oluşturabilmeniz için geçişler ihtiyacımız var. Varolan şema oluşturmak için mantığı içeren bir InitialCreate geçiş oluşturmak için ekleyeceğiz. Ardından bu geçiş zaten uygulanmış gibi görünmesini bizim var olan veritabanı oluşturacağız.

1.  Çalıştırma **Ekle geçiş InitialCreate** Paket Yöneticisi konsolunda komutu. Bu var olan bir şema oluşturmak için bir geçiş oluşturur.
2.  Yeni oluşturulan geçiş yukarı yönteminde tüm kod açıklama satırı yapın. Bu, bize 'geçiş için yerel veritabanı zaten mevcut olan tüm tabloları vb. yeniden çalışmadan uygulamak ' olanak tanır.
3.  Çalıştırma **veritabanını Güncelleştir** Paket Yöneticisi konsolunda komutu. Bu, InitialCreate geçiş veritabanına uygulanır. Gerçek geçiş içermediğinden değişiklikleri (biz geçici olarak bunları açıklamalı nedeniyle), genellikle yalnızca bir satır ekler \_ \_bu geçiş zaten uygulanmış olan tablo MigrationsHistory belirten.
4.  Yukarı yöntemindeki kod un açıklama. Başka bir deyişle, bu geçiş gelecekteki veritabanlarına uygulandığında, yerel veritabanında zaten varolduğu şema geçişleri tarafından oluşturulur.

## <a name="things-to-be-aware-of"></a>Dikkat edilmesi gereken noktalar

Geçişleri var olan bir veritabanında kullanırken bilmeniz gereken birkaç nokta vardır.

### <a name="defaultcalculated-names-may-not-match-existing-schema"></a>Varsayılan ve hesaplanan adları mevcut şema eşleşmiyor olabilir

Geçişleri açıkça belirten sütun ve tabloların adlarını, bir geçiş iskele oluşturulduğunu olduğunda. Ancak, geçişler hesaplar için bir varsayılan ad geçişlerin uygularken diğer veritabanı nesnelerini vardır. Bu, dizinleri ve yabancı anahtar kısıtlamaları içerir. Var olan bir şema hedeflenirken Bu hesaplanan adları ne veritabanınızda gerçekten var eşleşmeyebilir.

Bu durumun farkında olması gerektiğinde bazı örnekleri şunlardır:

**Kullandıysanız ' seçeneği bir: var olan şema başlangıç noktası olarak kullanın. ' adım 3:**

-   Değiştirmek veya farklı şekilde adlandırılmış veritabanı nesnelerinden birine bırakarak, modelinize ileride yapılacak değişikliklerin ihtiyacınız varsa, doğru adı belirtmek için iskele kurulmuş geçiş değiştirmeniz gerekir. Geçişleri API'leri, bunu yapmanızı sağlayan isteğe bağlı adı parametresi var.
    Örneğin, var olan şemanızı IndexFk adlı bir dizin olan bir BlogId yabancı anahtar sütunu içeren bir gönderi tablo olabilir\_BlogId. Varsayılan olarak bu dizin IX adlandırılması geçişleri ancak beklediğiniz\_BlogId. Bu dizini bırakma sonucunda modeldeki bir değişiklik yaparsanız, IndexFk belirtmek için iskele kurulmuş DropIndex çağrısı değiştirmeniz gerekecektir\_BlogId adı.

**Kullandıysanız ' seçeneği iki: bir başlangıç noktası olarak boş veritabanını kullan ' adım 3:**

-   Geçişleri dizinleri ve yanlış adları kullanarak yabancı anahtar kısıtlamalarını dener (yani, boş bir veritabanı için geri alma olduğu gibi) ilk geçiş aşağı yöntemi yerel veritabanınızda çalıştırılmaya çalışılırken başarısız olabilir. Diğer veritabanlarının ilk geçiş yukarı yöntemi kullanarak sıfırdan oluşturulacağından bu yalnızca yerel veritabanınızı etkiler.
    Var olan yerel veritabanınızı boş bir duruma düşürmek istiyorsanız bunu el ile veritabanını silmek veya tüm tabloları bırakarak yapmak oldukça kolaydır. Varsayılan adlarla tüm veritabanı nesnelerinin oluşturulması bu ilk Sürüm Düşürme sonra bu nedenle bu sorunu kendisi yeniden sunacaktır değil.
-   Modelinize ileride yapılacak değişikliklerin değiştirmek veya farklı şekilde adlandırılmış veritabanı nesnelerinden birine bırakarak gerektiriyorsa, varsayılan adları eşleşmeyecektir olduğundan bu var olan yerel veritabanınızda – çalışmaz. Ancak, geçişleri tarafından seçmiş varsayılan adları kullanmış olduğundan, 'sıfırdan' oluşturulan veritabanlarına karşı çalışır.
    Ya da bu değişiklikleri el ile yerel mevcut veritabanınızı yapmak veya diğer makinelere olacak şekilde veritabanınızı – sıfırdan yeniden geçişleri depolamayı düşünün.
-   İlk geçişinizi yukarı yöntemi kullanılarak oluşturulan veritabanları dizinler için hesaplanan varsayılan adlar beri yerel veritabanından biraz farklı olabilir ve yabancı anahtar kısıtlamalarını kullanılacaktır. Ayrıca ek dizinler bitirebilirsiniz gibi geçişleri dizinleri yabancı anahtar sütunlarına varsayılan olarak oluşturacaktır – bu durumda, özgün yerel veritabanınızı olmuş olabilir.

### <a name="not-all-database-objects-are-represented-in-the-model"></a>Tüm veritabanı nesneleri modelinde temsil edilir

Modelinizin bir parçası olmayan veritabanı nesneleri tarafından geçişleri işlenmedi. Bu görünümler, saklı yordamlar, izinler, model, ek dizinleri vb. parçası olmayan tablolar içerebilir.

Bu durumun farkında olması gerektiğinde bazı örnekleri şunlardır:

-   Seçenek bağımsız olarak, modelinize ileride yapılacak değişikliklerin değiştirme ya da geçişler bu değişiklikleri yapmak için oynatacaklarını bilmez bu ek nesneleri bırakarak kullanmanız gerekiyorsa 'Adım 3' olarak seçtiniz. Örneğin, ek dizin içeren bir sütun sürüklerseniz, dizini silmek için geçişler bilmez. İskele kurulmuş geçişi bu el ile eklemeniz gerekir.
-   Kullandıysanız ' seçeneği iki: bir başlangıç noktası olarak boş veritabanını kullan ', bu ek nesneleri ilk geçişinizi yukarı yöntemi tarafından oluşturulmaz.
    Yukarı değiştirebilir ve istediğiniz yöntemleri bu ek nesneleri, halletmeniz için. Geçişleri API'sindeki – – görünümleri gibi yerel olarak desteklenmeyen nesneler için kullanabileceğiniz [Sql](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.sql.aspx) oluşturma bunları bırakma için ham SQL çalıştırmak için yöntemi.
