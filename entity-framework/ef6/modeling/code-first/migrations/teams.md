---
title: Takım ortamları - EF6 Code First geçişleri
author: divega
ms.date: 2016-10-23
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
ms.openlocfilehash: 42f52e63fd6cfc1f02d6a721594f4a161eea9a7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997305"
---
# <a name="code-first-migrations-in-team-environments"></a>Takım ortamları Code First geçişleri
> [!NOTE]
> Bu makalede, temel senaryolarda Code First Migrations nasıl kullanılacağını varsayar. Sizin sonra okumak ihtiyacınız olacak [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) devam etmeden önce.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Bir kahve, tüm bu makaleyi okumak gerekli

Ekip ortamlarında iki geliştiriciler kendi yerel kod temelinde geçişleri oluşturduğunuzda çoğunlukla geçişleri birleştirme etrafında sorunlardır. Bunları çözmek için adımları oldukça basit olsa da, bunlar geçişleri nasıl çalıştığına ilişkin sağlam bir anlama sahip olmanızı gerektirir. Lütfen yoksa yalnızca sonuna kadar İleri atlayabilirsiniz: başarılı olduğundan emin olmak için tüm makaleyi okumak için zaman ayırın.

## <a name="some-general-guidelines"></a>Bazı genel yönergeleri

Biz birden fazla geliştirici tarafından oluşturulan birleştirme geçişleri yönetmek nasıl ayrıntılı olarak incelemeden önce başarı için ayarlamak için bazı genel yönergeleri aşağıda verilmiştir.

### <a name="each-team-member-should-have-a-local-development-database"></a>Her ekip üyesi, yerel geliştirme veritabanı olmalıdır

Geçişleri kullanır  **\_ \_MigrationsHistory** hangi geçişleri veritabanına uygulanmış depolamak üzere tablo. Birden çok geliştirici aynı veritabanını hedeflediğinizden çalışırken farklı geçişler oluşturma varsa (ve bu nedenle paylaşma bir  **\_ \_MigrationsHistory** tablo) geçişleri gittiğini çok kafanız alın.

Elbette, geçişler oluşturan olmayan takım üyeleriniz varsa, bunları Geliştirme Merkezi veritabanını paylaşan sahip hiçbir sorun yoktur.

### <a name="avoid-automatic-migrations"></a>Otomatik geçişleri kaçının

Alt satır otomatik geçişleri başlangıçta takım ortamlarda iyi görünür ancak gerçekte yalnızca çalışmıyor değil. Neden – okumaya devam edin, bilmek istiyorsunuz değilse, ardından, sonraki bölüme atlayabilirsiniz.

Otomatik geçişleri kod dosyaları (kod tabanlı geçişler) oluşturmaya gerek kalmadan geçerli model eşleşecek şekilde güncelleştirildi, veritabanı şemasını sahip olmanızı sağlar. Yalnızca kimse tarafından kullanılmadı ve hiçbir zaman herhangi bir kod tabanlı geçişler oluşturulan otomatik geçişleri çok iyi bir ekip ortamında çalışır. Sorun otomatik geçişleri sınırlıdır ve işlemlerinin sayısı işlemez – özellik/sütununu yeniden adlandırır, başka bir tabloya veri taşıma, vs. Bu senaryolar, düştüğünden işlemek için kod tabanlı geçişler oluşturmak (ve iskele kurulmuş kod düzenleme) otomatik geçişleri tarafından işlenen değişiklikleri arasında karma. Bu, yakın üzerinde olanaksız kılar iki geliştiriciler geçişlerde iade ettiğinizde, değişiklikleri birleştirmek için.

## <a name="screencasts"></a>Ekran kayıtları

Bu makaleyi okuyun bir yayını daha yerine izleyin, bu makalede aynı içeriğe aşağıdaki iki video kapsar.

### <a name="video-one-migrations---under-the-hood"></a>Bir video: "Başlık altında - geçişler"

[Bu yayını](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) geçişleri nasıl izlediği kapsar ve modeli hakkında bilgi modeli değişikliklerini algılamak için kullanır.

### <a name="video-two-migrations---team-environments"></a>Video iki: "Geçişler - takım ortamları"

Önceki video kavramları güncelleştirmelerle [bu yayını](http://channel9.msdn.com/blogs/ef/migrations-team-environments) takım ortamını ve bunları çözmek nasıl ortaya çıkan sorunları ele alır.

## <a name="understanding-how-migrations-works"></a>Geçişleri nasıl çalıştığını anlama

Anahtar başarıyla bir ekip ortamında migrations'ı kullanma, temel bir geçişi nasıl izlediği anlama ve modeli değişikliklerini algılamak için modelle ilgili bilgiler kullanır.

### <a name="the-first-migration"></a>İlk geçiş

İlk geçiş projenizi için eklediğinizde, aşağıdaki gibi çalıştırın **Ekle geçiş ilk** Paket Yöneticisi konsolunda. Bu komut gerçekleştiren üst düzey adımlar aşağıda gösterilir.

![FirstMigration](~/ef6/media/firstmigration.png)

Geçerli model kodunuzdan (1) olarak hesaplanır. Gerekli veritabanı nesnelerini ardından model farkları (2) hesaplanan – yalnızca kullanır, bu ilk geçiş modeli olduğundan, karşılaştırma için boş bir model farklılık gösterir. Visual Studio çözümünüzü (4) daha sonra eklenen gerekli geçiş kodu (3) derlemek için kod Oluşturucu için gerekli değişiklikleri geçirilir.

Ana kod dosyasında depolanan gerçek geçiş kodu yanı sıra, geçişleri de bazı ek arka plan kod dosyaları oluşturur. Bu dosyalar, geçişleri tarafından kullanılan meta verilerdir ve düzenleme bir şey değildir. Bu dosyalar geçiş oluşturulduğu sırada model anlık görüntüsünü içeren bir kaynak dosyası (.resx) biridir. Sonraki adımda bunu nasıl kullanıldığını görürsünüz.

Bu noktada, büyük olasılıkla çalıştıracağınız **veritabanını Güncelleştir** değişikliklerinizi veritabanına uygulayın ve ardından uygulamanızın diğer alanlar'ı uygulama hakkında.

### <a name="subsequent-migrations"></a>Sonraki geçişleri

Daha sonra geri dönün ve modelinize bazı değişiklikler yapmanız – örneğimizde ekleyeceğiz bir **Url** özelliğini **Blog**. Bir komut gibi ardından vermek **Ekle geçiş AddUrl** karşılık gelen veritabanı uygulamak için bir geçiş iskelesini değişir. Bu komut gerçekleştiren üst düzey adımlar aşağıda gösterilir.

![SecondMigration](~/ef6/media/secondmigration.png)

Aynı geçen seferki gibi geçerli model koddan (1) olarak hesaplanır. Ancak bu kez var. mevcut geçişleri önceki modelde en son geçiş (2) alınır böylece Diff (3) gerekli veritabanı değişikliklerini bulmak için bu iki modeli olan ve işlemini'ı önceki gibi tamamlar.

Aynı işlemi projeye eklemek daha tüm geçişler için kullanılır.

### <a name="why-bother-with-the-model-snapshot"></a>Neden model anlık görüntü rahatsız?

Neden EF modeli anlık görüntü ile – rahatsız değil yalnızca görünüm neden, veritabanını merak ediyor olabilirsiniz. Bu durumda, okumaya devam edin. Ardından ilgilenen değilseniz, bu bölümü atlayabilirsiniz.

Pek çok EF modeli anlık görüntü etrafında tutar vardır:

-   EF modeli kayma veritabanına sağlar. Bu değişiklik veritabanına doğrudan yapılamaz veya değişiklik yapmak için geçişler iskele kurulan kodu değiştirebilirsiniz. Bu uygulamada örnekleri birkaç şunlardır:
    -   Bir eklenen ve güncelleştirilen bir veya daha fazla tablolara sütun eklemek istediğiniz, ancak bu sütunların EF modele dahil etmek istemediğiniz. Geçişler veritabanını görünüyorsa bir geçiş iskele kurulmuş her zaman bu sütunları kaldırın sürekli isteriz. Model anlık görüntüyü kullanarak EF her zaman sadece modeline yasal değişiklikleri algılar.
    -   Güncelleştirmeler için bazı günlük dahil etmek için kullanılan bir saklı yordam gövde metni değiştirmek istediğinizde. Bu saklı yordamı, veritabanı geçişleri görünüyorsa, sürekli olarak deneyin ve geri EF bekliyor tanımına sıfırlama. Modeli anlık görüntü kullanarak EF yalnızca şimdiye kadar EF modeli yordamda şeklini değiştirdiğinizde saklı yordamı değiştirmek için kodunun iskelesini.
    -   Bu aynı ilkeler geçerlidir ek dizinleri ekleme, veritabanınızda ek tablolar da dahil olmak üzere, tablo, vb. üzerinde yer alan bir veritabanı görünümü EF eşleme.
-   EF modeli, veritabanı şeklini daha fazlasını içerir. Modelin tamamı modeliniz ve bunların tablolar ve sütunlar için nasıl eşleneceğine sınıfları ve özellikleri hakkındaki bilgilere bakmak geçişlerini sağlar. Bu bilgiler, geçişler, iskele oluşturulduğunu kodda daha akıllı olmasını sağlar. Örneğin, bir özellik geçişleri için eşlenen sütunun adını değiştirirseniz, aynı özellik – yalnızca veritabanı şeması varsa, sağlayan yapılamaz olmasını görüyorsunuz tarafından yeniden adlandırma algılayabilir. 

## <a name="what-causes-issues-in-team-environments"></a>Hangi sorunları takım ortamları neden olur

Bir uygulama üzerinde çalışan tek bir geliştirici olduğunda önceki bölümde çalışır harika iş akışı kapsamında. Modele değişiklik yapmadan yalnızca kişi olması durumunda da iyi bir ekip ortamında çalışır. Bu senaryoda modeli değişiklik, geçişler oluşturabilir ve bunları, kaynak denetimine gönderebilirsiniz. Diğer geliştiriciler değişikliklerinizi eşitleyin ve çalıştırma **veritabanını Güncelleştir** uygulanan şema değişiklikleri olması.

EF modeli değişiklikleri yaptıktan ve aynı anda kaynak denetimine gönderme birden fazla Geliştirici sahip ortaya çıkan sorunları başlayın. Ne EF eksik yerel geçiş son eşitlenmiş olduğundan, başka bir geliştirici kaynak denetimine gönderdiği geçişler ile birleştirmek için birinci sınıf bir yoludur.

## <a name="an-example-of-a-merge-conflict"></a>Bir birleştirme çakışması örneği

İlk böyle bir birleştirme çakışması somut bir örneğe göz atalım. Biz üzerinde daha önce inceledik örneğiyle devam edeceğiz. Başlangıç olarak işaret şimdi önceki bölümde değişiklikleri iade özgün geliştirici tarafından varsayılır. Bunlar kod için temel değişiklik olarak iki geliştiriciler izleriz.

EF modeli ve bir dizi değişiklik'ya kadar geçerli geçişleri izleriz. Bir başlangıç noktası için aşağıdaki grafikte gösterildiği gibi kaynak denetim deposu için hem geliştiriciler eşitlediyseniz.

![StartingPoint](~/ef6/media/startingpoint.png)

Geliştirici \#1 ve geliştirici \#2 artık yapar bazı değişiklikleri kendi yerel kodda EF modeli temel. Geliştirici \#1 ekler bir **derecelendirme** özelliğini **Blog** – ve oluşturan bir **AddRating** veritabanına değişiklikleri uygulamak için geçiş. Geliştirici \#2 ekler bir **okuyucular** özelliğini **Blog** – ve karşılık gelen oluşturur **AddReaders** geçiş. Her iki geliştiriciler çalıştırma **veritabanını Güncelleştir**, değişiklikleri kendi yerel veritabanlarına uygulayın ve ardından uygulama geliştirme devam edin.

> [!NOTE]
> Geçişleri bizim grafiğini temsil eden için bir zaman damgası ile öneki AddReaders geçiş'geliştiriciden \#2 geliştiriciden AddRating geçişten sonra gelen \#1. Olmadığını Geliştirici \#1 veya \#takım ya da bunları sonraki bölümde bakacağız birleştirme işlemi çalışmanın sorunlara fark 2 oluşturulan geçiş ilk yapar.

![LocalChanges](~/ef6/media/localchanges.png)

Bir geliştiricinin lucky gündür \#1 gelişmelerden önce yaptıkları değişiklikleri göndermek için. Bunlar, depo eşitlenmiş olduğundan başka hiç kimse iade olduğundan, herhangi bir birleştirme işlemi yapmadan bunlar yalnızca değişikliklerini gönderebilirsiniz.

![Gönder](~/ef6/media/submit.png)

Geliştirici zamanı artık \#göndermek için 2. Bunlar, bu nedenle lucky değildir. Kullanıcılar eşitlenmiş olduğundan başkası değişiklikler gönderdi olduğundan, birleştirme ve değişiklikleri çekmek gerekir. Kaynak denetim sistemi, büyük olasılıkla çok basit olduğundan kod düzeyinde değişiklikleri otomatik olarak birleştirmek mümkün olacaktır. Geliştirici durumunu \#2 yerel aşağıdaki grafikte gösterilen eşitlemeden sonra depo. 

![Çekme](~/ef6/media/pull.png)

Şu anda Geliştirici aşama \#2 çalıştırabilirsiniz **veritabanını Güncelleştir** hangi algılayacak yeni **AddRating** geçiş (hangi henüz uygulanmadığını Geliştirici \#2 veritabanı) ve uygulayın. Artık **derecelendirme** sütun eklenir **blogları** tablo ve veritabanı modeli ile eşitlenmiş.

Ancak birkaç sorun vardır:

1.  Ancak **veritabanını Güncelleştir** uygulanacak **AddRating** geçiş, da yükseltmek bir uyarı: *bekleyen değişiklikler olduğundan, geçerli model eşleştirilecek veritabanı güncelleştirilemiyor ve Otomatik geçişi devre dışı bırakıldı...*
    Model anlık görüntü son geçişin depolanan sorunudur (**AddReader**) eksik **derecelendirme** özelliği **Blog** (modelinin bir parçası değildi beri olduğunda geçiş oluşturuldu). Kod öncelikle son geçiş modelinde geçerli model eşleşmiyor ve uyarı meydana getirir algılar.
2.  Uygulamayı çalıştıran InvalidOperationException belirten sonuçlanır "*veritabanı oluşturulduktan sonra 'BloggingContext' bağlam yedekleme modeli değişti. ... Veritabanını güncellemek için Code First Migrations'ı kullanmayı deneyin"*
    Yeniden son geçişin depolanan modeli anlık görüntü geçerli model eşleşmeyen bir sorundur.
3.  Son olarak, biz çalışmasını beklediğiniz **Ekle geçiş** (veritabanına uygulamak için herhangi bir değişiklik olduğundan) boş bir geçiş artık üretir. Ancak geçişleri dışlanmaz çünkü geçerli model biri son geçiş (eksik olduğu **derecelendirme** özelliği) da aslında başka iskele **AddColumn** içinde eklemekiçinçağrı**Derecelendirme** sütun. Doğal olarak, bu geçiş sırasında başarısız olacağı **veritabanını Güncelleştir** çünkü **derecelendirme** sütun zaten mevcut.

## <a name="resolving-the-merge-conflict"></a>Birleştirme çakışması çözümleme

Güzel bir haberimiz var geçişleri nasıl çalıştığına ilişkin bir anlayış olması kaydıyla, birleştirme el ile – dağıtılacak çok zor olmasıdır. Bu bölüme atlandı devam varsa bunu... Üzgünüz, geri dönün ve makalenin geri kalanında okumanız gerekir!

İki seçenek, bir anlık görüntü olarak doğru geçerli modeli olan boş bir geçiş oluşturmak için en kolay yoldur. İkinci seçenek doğru olması için son geçiş anlık güncelleştirme, model anlık görüntü. İkinci seçenek biraz daha zordur ve her senaryoda kullanılamaz, ancak fazladan bir geçiş ekleme içermeyen ayrıca temizleyici olmasıdır.

### <a name="option-1-add-a-blank-merge-migration"></a>1. seçenek: boş 'merge' geçiş Ekle

Bu seçenek yalnızca en son geçiş sahip emin olmak amacıyla boş bir geçiş oluşturduğumuz doğru model anlık görüntü depolanan içinde.

Bu seçenek bağımsız olarak, kimin son geçiş oluşturulan kullanılabilir. Biz aşağıdaki Geliştirici örnekte \#2, birleştirme care sürüyor ve son geçiş oluşturmak için gerçekleştiği. Ancak aynı adımları kullanılabilir Geliştirici \#1 oluşturulan son geçiş. Adımlar, birden çok geçiş ilgili – biz yalnızca iki basit tutmak için arıyorsunuz durumunda da geçerlidir.

Aşağıdaki işlem, kaynak denetiminden eşitlenmesi gereken değişiklikleri olması fark uygulanmasından itibaren bu yaklaşım için kullanılabilir.

1.  Tüm bekleyen model değişiklikleri yerel kod tabanınızın bir geçiş için yazılmış emin olun. Bu adım, boş bir geçiş oluşturmak için zaman geldiğinde yasal değişiklikleri kaçırmayın sağlar.
2.  Kaynak denetimi ile eşitleyin.
3.  Çalıştırma **veritabanını Güncelleştir** diğer geliştiriciler iade herhangi bir yeni geçişler uygulamak için.
    **
    *Not: *** tüm uyarılar veritabanını Güncelleştir komutundan elde etmezsiniz sonra hiçbir yeni geçiş diğer geliştiricilerden vardı ve herhangi ek birleştirme gerçekleştirmek için gerek yoktur.*
4.  Çalıştırma **Ekle geçiş &lt;çekme\_bir\_adı&gt; – IgnoreChanges** (örneğin, **Ekle geçiş birleştirme – IgnoreChanges**). Bu (geçerli modelinkiyle anlık görüntüsünü dahil) tüm meta verileriyle bir geçiş oluşturur, ancak geçerli model için son geçişlerin anlık karşılaştırılırken algıladığı herhangi bir değişiklik göz ardı eder (boş alma anlamı **yukarı** ve **Aşağı** yöntemi).
5.  Geliştirmeye devam veya kaynak denetimi (birim Elbette testleri sonra) gönderin.

İşte Geliştirici durumunu \#2 yerel kod bu yaklaşım kullandıktan sonra temel.

![MergeMigration](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>2. seçenek: son geçiş modeli anlık güncelleştirme

Bu seçenek seçeneği 1 çok benzer, ancak ek boş geçişi – çünkü kaldırır, ek bir kod dosyalarında çözüm isteyen şimdi yüz tanıma.

**Bu yaklaşım yalnızca en son geçiş, yalnızca yerel kod tabanınızın var ve (örneğin, son geçiş birleştirme yapan kullanıcı tarafından oluşturulmuşsa) henüz kaynak denetimine gönderilmedi uygulanabilirdir**. Diğer geliştiricilerin zaten kendi geliştirme veritabanı – veya hatta uyguladınız geçişleri meta verilerini düzenleme daha da kötüsü üretim veritabanına – uygulanan beklenmeyen etkilere neden olabilir. İşlem sırasında son geçişi yerel veritabanımızda yer geri ve güncelleştirilmiş meta verileriyle yeniden uygulamak için ekleyeceğiz.

Yalnızca son geçiş gereksinimlerini yerel kod tabanı sayısı veya devam geçişleri sırasını kısıtlama yoktur olması. Birden çok geçiş birden çok farklı geliştiricilerden olabilir ve aynı adımları uygulayın: biz yalnızca iki basit tutmak için arıyorsunuz.

Aşağıdaki işlem, kaynak denetiminden eşitlenmesi gereken değişiklikleri olması fark uygulanmasından itibaren bu yaklaşım için kullanılabilir.

1.  Tüm bekleyen model değişiklikleri yerel kod tabanınızın bir geçiş için yazılmış emin olun. Bu adım, boş bir geçiş oluşturmak için zaman geldiğinde yasal değişiklikleri kaçırmayın sağlar.
2.  Kaynak denetimi ile eşitleyin.
3.  Çalıştırma **veritabanını Güncelleştir** diğer geliştiriciler iade herhangi bir yeni geçişler uygulamak için.
    **
    *Not: *** tüm uyarılar veritabanını Güncelleştir komutundan elde etmezsiniz sonra hiçbir yeni geçiş diğer geliştiricilerden vardı ve herhangi ek birleştirme gerçekleştirmek için gerek yoktur.*
4.  Çalıştırma **veritabanını güncelleştir – TargetMigration &lt;ikinci\_son\_geçiş&gt;**  (biz aşağıdaki örnekte bu olacaktır **-veritabanı – güncelleştirme TargetMigration AddRating**). Bu veritabanını yedeklemek için ikinci durumunu rolleri son geçişi – etkili bir şekilde 'beklemediğiniz uygulanan' veritabanından son geçiş.
    **
    *Not: *** olduğundan meta veriler de depolanan meta veriler geçişin düzenlemek güvenli hale getirmek için bu adım gereklidir \_ \_MigrationsHistoryTable veritabanı. Yalnızca son geçiş yalnızca yerel kodunuzda temel varsa bu seçeneği kullanmalısınız nedeni budur. Diğer veritabanlarının uygulanan son geçiş olsaydı geri alma ve meta verileri güncelleştirmek için son geçiş yeniden uygulamanız gerekir.* 
5.  Çalıştırma **Ekle geçiş &lt;tam\_adı\_dahil olmak üzere\_zaman damgası\_,\_son\_geçiş** &gt; (örnekte Biz başlangıcındaki bu aşağıdakine benzer olacaktır **Ekle geçiş 201311062215252\_AddReaders**).
    **
    *Not: *** geçişleri bilebilmesi yeni bir yapı iskelesini oluşturmak yerine var olan geçiş düzenlemek istediğiniz zaman damgası eklemeniz gerekir.*
    Bu, geçerli model eşleşecek şekilde son geçiş için meta verileri güncelleştirir. Komut tamamlandıktan, ancak tam olarak istediğiniz zaman aşağıdaki uyarıyı alırsınız. "*Yalnızca geçiş ' 201311062215252 Tasarımcısı kodunu\_AddReaders yeniden iskele kurulmuş. Tüm geçiş yeniden iskelesini kullanın - Force parametresini. "*
6.  Çalıştırma **veritabanını Güncelleştir** güncelleştirilmiş meta verilerin en son geçiş yeniden uygulamak için.
7.  Geliştirmeye devam veya kaynak denetimi (birim Elbette testleri sonra) gönderin.

İşte Geliştirici durumunu \#2 yerel kod bu yaklaşım kullandıktan sonra temel.

![UpdatedMetadata](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Özet

Bir ekip ortamında Code First Migrations'ı kullanırken, bazı zorluklar vardır. Ancak, temel bir anlayışa geçişleri nasıl çalıştığını ve birleştirme çakışmalarını çözmek için basit yaklaşımlardan kolaylaştırır bu zorluklarının üstesinden gelin.

Temel sorunu, en son geçişin depolanan yanlış meta verilerdir. Bu ilk kod geçerli model ve veritabanı şeması eşleşmeyen yanlış algılamak için ve sonraki geçişin yanlış kodunun iskelesini neden olur. Bu durum, doğru modeli ile boş bir geçiş oluşturmak ya da son geçiş meta verilerde güncelleştirme üstesinden gelebilir.
