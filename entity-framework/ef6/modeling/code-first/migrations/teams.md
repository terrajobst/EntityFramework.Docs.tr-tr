---
title: Ekip ortamlarında Code First Migrations-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
ms.openlocfilehash: b3c4c35d636caf4ddd251dd78e026587abc57d42
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182609"
---
# <a name="code-first-migrations-in-team-environments"></a>Ekip ortamlarında Code First Migrations
> [!NOTE]
> Bu makalede temel senaryolarda Code First Migrations kullanmayı bildiğiniz varsayılmaktadır. Bunu yapmazsanız devam etmeden önce [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) okumanız gerekir.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Kahve, bu makalenin tamamını okumanız gerekir

Ekip ortamlarındaki sorunlar, iki geliştirici yerel kod tabanında geçişler oluştururken geçişleri birleştirme konusunda büyük bir yerdedir. Bunları çözmeye yönelik adımlar oldukça basittir, ancak geçiş işlemlerinin nasıl çalıştığına ilişkin katı bir anlama sahip olmanızı gerektirir. Lütfen sonuna kadar atlayın; başarılı olduğunuzdan emin olmak için makalenin tamamını okumaya devam edin.

## <a name="some-general-guidelines"></a>Bazı genel yönergeler

Birden çok geliştirici tarafından oluşturulan birleştirme geçişlerinin nasıl yönetileceğini öğrenmek için, bu işlemin başarılı olup olmadığını ayarlamak için bazı genel yönergeler aşağıda verilmiştir.

### <a name="each-team-member-should-have-a-local-development-database"></a>Her takım üyesinin yerel bir geliştirme veritabanına sahip olması gerekir

Geçişler, veritabanına uygulanan geçişlerin depolanması için **\_ @ no__t-2MigrationsHistory** tablosunu kullanır. Aynı veritabanını hedefleyecek şekilde birden çok geliştirici varsa farklı geçişler oluşturduysanız ( **\_ @ no__t-2MigrationsHistory** tablosu paylaşma) geçişler çok karıştırılır.

Tabii ki, geçiş üretmeyen ekip üyeleriniz varsa, bunların merkezi bir geliştirme veritabanını paylaşmasına gerek yoktur.

### <a name="avoid-automatic-migrations"></a>Otomatik geçişlere engel

Alt çizgi, otomatik geçişlerin takım ortamlarında iyi şekilde bakmasıdır, ancak gerçekte yalnızca çalışmaz. Nedenini, okumayı koruyun – yoksa, sonraki bölüme atlayabilirsiniz.

Otomatik geçişler, kod dosyaları üretmesine gerek kalmadan, veritabanı şemanızın geçerli modelle eşleşecek şekilde güncelleştirilmesini sağlar (kod tabanlı geçişler). Yalnızca kullandıysanız ve kod tabanlı geçişler oluşturmadıysa otomatik geçişler bir ekip ortamında oldukça iyi çalışır. Bu sorun, otomatik geçişlerin sınırlı olduğu ve bir dizi işlemin (özellik/sütun yeniden adlandırmaları, verileri başka bir tabloya taşıma vb.) sınırlandırılmasıdır. Bu senaryoları işlemek için, otomatik geçişler tarafından işlenen değişiklikler arasında karma olan kod tabanlı geçişler (ve yapı iskelesi kodunu düzenleyerek) sonlandırın. Bu, iki geliştirici geçişi iade edildiğinde değişiklikleri birleştirme imkanını neredeyse olanaksız hale getirir.

## <a name="screencasts"></a>Tasarlandı

Bu makaleyi okuduğunuzdan bir ekran koruyucu izlemeyi tercih ediyorsanız, aşağıdaki iki video bu makaleyle aynı içeriği kapsar.

### <a name="video-one-migrations---under-the-hood"></a>Video One: "Geçişler-"

[Bu ekran kaydı](https://channel9.msdn.com/blogs/ef/migrations-under-the-hood) , geçişlerin, model değişikliklerini algılamak için model hakkındaki bilgileri nasıl izlediğini ve kullandığını ele alır.

### <a name="video-two-migrations---team-environments"></a>Video Iki: "Geçişler-ekip ortamları"

Önceki videodaki kavramlar üzerinde oluşturma, [Bu ekran](https://channel9.msdn.com/blogs/ef/migrations-team-environments) görüntüsü bir ekip ortamında oluşan sorunları ve bunları nasıl çözebileceğini ele alır.

## <a name="understanding-how-migrations-works"></a>Geçişlerin nasıl çalıştığını anlama

Bir ekip ortamında geçişleri başarıyla kullanmaya yönelik anahtar, geçişlerin nasıl izlediği ve model değişikliklerini algılamak için model hakkındaki bilgileri nasıl izlediğini anlayabiliyor.

### <a name="the-first-migration"></a>İlk geçiş

Projenize ilk geçişi eklediğinizde, Paket Yöneticisi konsolunda **önce geçiş Ekle** gibi bir şey çalıştırırsınız. Bu komutun gerçekleştirdiği üst düzey adımlar aşağıda verilmiştir.

![İlk geçiş](~/ef6/media/firstmigration.png)

Geçerli model, kodunuzda (1) hesaplanır. Gerekli veritabanı nesneleri daha sonra model tarafından hesaplanır (2). bu ilk geçiş olduğundan, model farklı bir şekilde karşılaştırma için yalnızca boş bir model kullanır. Gerekli değişiklikler kod oluşturucusuna geçirilir ve daha sonra Visual Studio çözümünüze (4) eklenen gerekli geçiş kodunu (3) oluşturur.

Ana kod dosyasında depolanan gerçek geçiş koduna ek olarak geçişler de bazı ek arka plan kod dosyaları oluşturur. Bu dosyalar, geçişler tarafından kullanılan ve düzenlemeniz gereken bir şey olmayan meta verilerlerdir. Bu dosyalardan biri, geçişin oluşturulduğu sırada modelin anlık görüntüsünü içeren bir kaynak dosyasıdır (. resx). Bu, bir sonraki adımda nasıl kullanıldığını görürsünüz.

Bu noktada, değişiklikleri veritabanına uygulamak için büyük olasılıkla **Update-Database** ' i çalıştırıp uygulamanızın diğer bölgelerini uygulamaya gidebilirsiniz.

### <a name="subsequent-migrations"></a>Sonraki geçişler

Daha sonra modelinize geri dönüp modelinizde bazı değişiklikler yaparsınız. bizim örneğimizde, **blogda**bir **URL** özelliği ekleyeceğiz. Ardından, karşılık gelen veritabanı değişikliklerini uygulamak üzere bir geçişe geçiş yapmak için **Add-Migration AddUrl** gibi bir komut verirsiniz. Bu komutun gerçekleştirdiği üst düzey adımlar aşağıda verilmiştir.

![İkinci geçiş](~/ef6/media/secondmigration.png)

Son kez olduğu gibi, geçerli model koddan hesaplanır (1). Ancak, bu kez, önceki modelin en son geçişten (2) alınması için mevcut geçişler vardır. Bu iki model, gerekli veritabanı değişikliklerini (3) bulmak için dağıtılır ve sonra işlemin daha önce olduğu gibi tamamlanır.

Bu işlem, projeye eklediğiniz diğer geçişler için de kullanılır.

### <a name="why-bother-with-the-model-snapshot"></a>Model anlık görüntüsüne neden neden olsıther?

Model anlık görüntüsüne neden olduğunu merak ediyor olabilirsiniz; bu nedenle veritabanına bakmasınız. Öyleyse, okumaya devam edin. İlgilenmiyorsanız, bu bölümü atlayabilirsiniz.

Bir dizi nedenden dolayı model anlık görüntüsünün etrafında devam eder:

-   Veritabanınızın EF modelinden ft 'e erişmesine izin verir. Bu değişiklikler doğrudan veritabanında yapılabilir veya değişiklikleri yapmak için geçişlerinizin scafkatlama kodunu değiştirebilirsiniz. İşte bu uygulamada birkaç örnek aşağıda verilmiştir:
    -   Bir veya daha fazla tabloünize ekli ve güncelleştirilmiş bir sütunu eklemek istiyorsunuz, ancak bu sütunları EF modeline eklemek istemezsiniz. Geçişler veritabanına bakıyorsa, bir geçişi her kullandığınızda sürekli olarak bu sütunları bırakmaya çalışır. Model anlık görüntüsünü kullanarak, EF yalnızca modeldeki meşru değişiklikleri algılar.
    -   Güncelleştirmeler için kullanılan saklı yordamın gövdesini, bazı günlükleri içerecek şekilde değiştirmek istiyorsunuz. Geçişler veritabanından bu saklı yordama bakıyorsa, bu işlemi sürekli olarak dener ve EF 'in beklediği tanıma yeniden sıfırlar. Model anlık görüntüsünü kullanarak EF yalnızca, EF modelindeki yordamın şeklini değiştirirken saklı yordamı değiştirmek için bir kod kullanacaktır.
    -   Aynı ilkeler, veritabanınıza ek tablolar dahil olmak üzere ek dizinler eklemek, EF 'i tablo üzerinde yer alan bir veritabanı görünümüyle eşleştirmek için de geçerlidir.
-   EF modeli yalnızca veritabanının şeklinin fazlasını içerir. Modelin tamamının olması, geçiş geçişlerinin modelinizdeki Özellikler ve sınıflar hakkındaki bilgilere ve bunların sütunlarına ve tablolarla nasıl eşlenmesine izin verir. Bu bilgiler, geçişlerinin, dolandırıcıların bulduğu kodda daha akıllı olmasına olanak sağlar. Örneğin, bir özelliğin geçişlerle eşlendiği sütunun adını değiştirirseniz, yalnızca veritabanı şemasına sahipseniz, bu özelliğin aynı özelliği olduğunu görerek yeniden adlandırma işlemi algılayabilir. 

## <a name="what-causes-issues-in-team-environments"></a>Takım ortamlarında sorunlara neden olur

Önceki bölümde ele alınan iş akışı, bir uygulama üzerinde çalışan tek bir geliştirici olduğunuzda harika bir şekilde çalışır. Ayrıca, modelde değişiklik yapan tek kişiyseniz, bir ekip ortamında da iyi bir şekilde çalışacaktır. Bu senaryoda model değişiklikleri yapabilir, geçişler oluşturabilir ve bunları kaynak denetimize gönderebilirsiniz. Diğer geliştiriciler değişiklikleri eşitleyebilir ve şema değişikliklerinin uygulanmasını sağlamak için **Update-Database** ' i çalıştırabilir.

Çok sayıda geliştirici, EF modelinde değişiklik yaparken ve kaynak denetimine aynı anda gönderim yaptığınızda sorun oluşur. En son eşitleme işleminden bu yana başka bir geliştiricinin kaynak denetimine gönderdiği geçişlerle yerel geçişlerinizi birleştirmenin ilk sınıf yolu yoktur.

## <a name="an-example-of-a-merge-conflict"></a>Birleştirme çakışması örneği

İlk olarak, bu tür birleştirme çakışmasına ait somut bir örneğe bakalım. Daha önce bakdığımız örnekle devam edeceğiz. Başlangıç noktası olarak, önceki bölümdeki değişikliklerin özgün geliştirici tarafından iade edilmiş olduğunu varsayalım. Kod tabanında değişiklik yaptıkları için iki geliştirici izliyoruz.

EF modelini ve geçişleri bir dizi değişiklik aracılığıyla izliyoruz. Bir başlangıç noktası için, her iki geliştirici aşağıdaki grafikte gösterildiği gibi kaynak denetimi deposu ile eşitlenir.

![Başlangıç noktası](~/ef6/media/startingpoint.png)

Geliştirici \#1 ve geliştirici \#2 artık yerel kod tabanında EF modelinde bazı değişiklikler yapar. Geliştirici \#1 **Blog** 'A bir **Derecelendirme** özelliği ekler ve değişiklikleri veritabanına uygulamak için bir **addderecelendirme** geçişi oluşturur. Geliştirici \#2 **Blog** 'A bir **Okuyucular** özelliği ekler ve ilgili **addokuyucular** geçişini oluşturur. Her iki geliştirici da, değişiklikleri yerel veritabanlarına uygulamak için **Update-Database**' i çalıştırır ve ardından uygulamayı geliştirmeye devam eder.

> [!NOTE]
> Geçişlere bir zaman damgası eklenir, bu nedenle grafiğimiz geliştirici \#2 ' den gelen Addokuyucular geçişinin, geliştirici \#1 ' den sonra gelen Addderecelendirme geçişinden sonra geldiğini temsil eder. Geliştirici \#1 veya \#2 ' nin oluşturulduğu, geçiş öncelikle bir takımda çalışma sorunlarından herhangi bir farklılık yapmaz ya da bir sonraki bölümde bakacağımız birleştirme süreciyle sonuçlanır.

![Yerel değişiklikler](~/ef6/media/localchanges.png)

Bu, geliştirici @no__t için bir Backy günüdür. Depolamalarını eşitlediği için başka hiç kimse iade ettiğinden, herhangi bir birleştirme gerçekleştirmeden yalnızca kendi değişikliklerini gönderebilirler.

![Gönder](~/ef6/media/submit.png)

Şimdi geliştirici \#2 ' nin göndermesi zaman alabilir. Bunlar bu kadar Lucky değildir. Eşitlendikleri tarihten sonra başka birisi tarafından gönderilmiş olduğundan, değişiklikler ve birleştirme işlemini çekmeleri gerekir. Kaynak denetim sistemi, büyük olasılıkla, değişiklikler çok basittir çünkü kod düzeyinde otomatik olarak birleştirebilecektir. Geliştirici \#2 ' nin yerel deposu, eşitlemeden sonra bu durum aşağıdaki grafikte gösterilmiştir. 

![Çekme](~/ef6/media/pull.png)

Bu aşama geliştirici \#2, yeni **Addderecelendirme** geçişini algılayacak (Geliştirici \#2 veritabanına uygulanmamış) ve uygulamayı sağlayacak olan **Update-Database** komutunu çalıştırabilir. Artık **Derecelendirme** sütunu **Bloglar** tablosuna eklenir ve veritabanı modelle eşitlenmiş durumda.

Ancak birkaç sorun vardır:

1.  **Update-Database** , **addderecelendirme** geçişini uygulayabilse de, bir uyarı da tetiklecektir: *Bekleyen değişiklikler olduğundan ve otomatik geçiş devre dışı olduğundan, veritabanı geçerli modelle eşleşecek şekilde güncelleştirilemiyor...*
    Bu sorun, son geçişte (**Addreader**) depolanan model anlık görüntüsünün, **blogdaki** **Derecelendirme** özelliğinin (geçiş oluşturulduğunda modelin bir parçası olmadığı için) eksik olması olabilir. Code First, son geçiş içindeki modelin geçerli modelle eşleştiğini algılar ve uyarıyı yükseltir.
2.  Uygulamanın çalıştırılması, veritabanının oluşturulmasından bu yana " *' BloggingContext ' bağlamını yedekleyen modelin değiştiğini belirten bir InvalidOperationException ile sonuçlanır. Veritabanını güncelleştirmek için Code First Migrations kullanmayı düşünün... "*
    Bu sorun, son geçişte depolanan model anlık görüntüsünün geçerli modelle eşleşmez.
3.  Son olarak, şimdi de, çalışma **geçişinin** daha sonra boş bir geçiş oluşturması beklenir (veritabanına uygulanacak bir değişiklik olmadığı için). Ancak geçişler geçerli modeli son geçişten ( **Derecelendirme** özelliği eksik) ile karşılaştırdığından, **Derecelendirme** sütununa eklemek Için başka bir **AddColumn** çağrısını gerçekten bir şekilde ele alacak. Tabii ki, bu geçiş **Update-Database** sırasında başarısız olur çünkü **Derecelendirme** sütunu zaten var.

## <a name="resolving-the-merge-conflict"></a>Birleştirme çakışmasını çözme

İyi haber, geçişlerin nasıl çalıştığına ilişkin daha fazla bilgiye sahip olmanız kaydıyla, birleştirmeye el ile ilgilenmek çok zor değildir. Bu nedenle, bu bölümde daha önce atladıysanız... Ne yazık ki geri dönüp önce makalenin geri kalanını okumanız gerekir!

İki seçenek vardır. Bu, en kolay bir anlık görüntü olarak doğru geçerli modele sahip boş bir geçiş oluşturmadır. İkinci seçenek, son geçişte anlık görüntüyü doğru model anlık görüntüsüne sahip olacak şekilde güncelleştirmedir. İkinci seçenek biraz daha zordur ve her senaryoda kullanılamaz, ancak ek bir geçiş eklemeyi içermediğinden de temizleyici olur.

### <a name="option-1-add-a-blank-merge-migration"></a>Seçenek 1: Boş bir ' Merge ' geçişi ekleyin

Bu seçenekte, yalnızca en son geçişin doğru model anlık görüntüsüne sahip olduğundan emin olmak amacıyla yalnızca bir boş geçiş oluşturacağız.

Bu seçenek, son geçişi kimin oluşturmuş olduğunuza bakılmaksızın kullanılabilir. Aşağıda, geliştirici takip ediyoruz \#2 birleştirme işlemini gerçekleştirerek son geçişi oluşturma gerçekleşirken. Ancak, geliştirici \#1 son geçişi oluşturduysa Bu adımlar kullanılabilir. Bu adımlar, birden çok geçiş söz konusu olduğunda da geçerlidir. Bu, en basit durumda tutulması için yalnızca iki tane bakıyorduk.

Aşağıdaki işlem, kaynak denetiminden eşitlenmesi gereken değişiklikler olduğunu fark ettiğiniz zamandan itibaren bu yaklaşım için kullanılabilir.

1.  Yerel kod tabanınız içindeki bekleyen model değişikliklerinin bir geçişe yazıldığından emin olun. Bu adım, boş geçiş oluşturmak için zaman geldiğinde herhangi bir yasal değişikliği kaçırmamasını sağlar.
2.  Kaynak denetimiyle eşitleyin.
3.  Diğer geliştiricilerin denetlediği yeni geçişleri uygulamak için **Update-Database** ' i çalıştırın.
    **_Note:_** *Update-Database komutundan herhangi bir uyarı alamazsanız, diğer geliştiricilerden yeni geçiş yoktu ve daha fazla birleştirme gerçekleştirmeye gerek yoktur.*
4.  **Add-migration @no__t çalıştırın-1pick @ no__t-2A @ no__t-3name @ no__t-4 – ıgnorechanges** (örneğin, **Add-Migration Merge – ıgnorechanges**). Bu, tüm meta veriler (geçerli modelin anlık görüntüsü dahil) ile bir geçiş oluşturur, ancak geçerli model son geçişlerde anlık görüntüyle karşılaştırılırken algıladığı tüm değişiklikleri yoksayar (boş bir **yukarı** ve **aşağı** yöntemi alırsınız).
5.  Geliştirmeye devam edin veya kaynak denetimine gönderim yapın (kendi birim testlerinizi çalıştırdıktan sonra).

Bu yaklaşım kullanıldıktan sonra geliştirici \#2 ' in yerel kod tabanı 'nın durumu aşağıda verilmiştir.

![Birleştirme geçişi](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Seçenek 2: Son geçişte model anlık görüntüsünü güncelleştirme

Bu seçenek, 1. seçeneğe çok benzer ancak daha fazla boş geçiş de kaldırılır. bu nedenle, çözümünde ek kod dosyaları istiyor.

**Bu yaklaşım yalnızca, en son geçişin yalnızca yerel kod tabanınız varsa ve henüz kaynak denetimine gönderilmediyse (örneğin, birleştirme yapan kullanıcı tarafından son geçiş oluşturulduysa) uygulanabilir**. Diğer geliştiricilerin geliştirme veritabanına daha önce uygulamış olabileceği veya daha kötü bir üretim veritabanına uygulanan geçişlerin meta verilerini düzenlemekte, beklenmeyen yan etkilere neden olabilir. İşlem sırasında, yerel veritabanımızda son geçişi geri almak ve güncelleştirilmiş meta veriler ile yeniden uygulamamız istenir.

Son geçişin yalnızca yerel kod tabanında olması gerekir, ancak devam eden geçişlerin sayısı veya sırası için hiçbir kısıtlama yoktur. Birden çok farklı geliştiriciden birden çok geçiş olabilir ve aynı adımlar geçerlidir. Bu, en basit durumda tutulması için yalnızca iki tane bakıyorduk.

Aşağıdaki işlem, kaynak denetiminden eşitlenmesi gereken değişiklikler olduğunu fark ettiğiniz zamandan itibaren bu yaklaşım için kullanılabilir.

1.  Yerel kod tabanınız içindeki bekleyen model değişikliklerinin bir geçişe yazıldığından emin olun. Bu adım, boş geçiş oluşturmak için zaman geldiğinde herhangi bir yasal değişikliği kaçırmamasını sağlar.
2.  Kaynak denetimiyle eşitleyin.
3.  Diğer geliştiricilerin denetlediği yeni geçişleri uygulamak için **Update-Database** ' i çalıştırın.
    **_Note:_** *Update-Database komutundan herhangi bir uyarı alamazsanız, diğer geliştiricilerden yeni geçiş yoktu ve daha fazla birleştirme gerçekleştirmeye gerek yoktur.*
4.  **Update-Database – TargetMigration &lt; saniye @ no__t-2last @ no__t-3migration @ no__t-4** ' ü çalıştırın (bunun için aşağıdaki örnekte **Update-Database – Targetmigration addderecelendirmesi**) bulunur. Bu, veritabanını ikinci son geçişin durumuna geri doğru şekilde, son geçiş ' i veritabanından son geçişin uygulanmasını sağlar.
    **_Notun_** *bu adım, meta verilerin, veritabanının \_ @ no__t-2MigrationsHistoryTable ' da depolanmasından sonra geçişin meta verilerini düzenlemesini güvenli hale getirmek için gereklidir. Bu nedenle, yalnızca son geçiş yalnızca yerel kod tabanınız ise bu seçeneği kullanmanız gerekir. Diğer veritabanlarına en son geçiş uygulanmışsa, verileri güncelleştirmek için geri almanız ve son geçişi yeniden uygulamanız gerekir.* 
5.  **Add-migration &lt;full @ no__t-2name @ no__t-3' i @ no__t-4timestamp @ no__t-5of @ no__t-6last @ no__t-7migration**&gt; (Bu örnekte, bunu takip ettiğimiz örnekte, **Add-Migration 201311062215252 @ no__ gibi bir şey olacaktır) çalıştırın. t-10Addokuyucular**).
    **_Notun_** *Geçişlerin yeni bir yükseltme yerine var olan geçişi düzenlemek istediğinizi bilmesi için zaman damgasını dahil etmeniz gerekir.*
    Bu işlem, son geçişin meta verilerini geçerli modelle eşleşecek şekilde güncelleştirir. Komut tamamlandığında aşağıdaki uyarıyı alırsınız, ancak tam olarak istediğiniz şeydir. "*Yalnızca ' 201311062215252 @ no__t-1Addokuyucular ' geçişinin tasarımcı kodu yeniden tanılandı. Geçişin tamamını yeniden kullanmak için-zorlama parametresini kullanın. "*
6.  Güncelleştirilmiş meta verilerle en son geçişi yeniden uygulamak için **Update-Database** ' i çalıştırın.
7.  Geliştirmeye devam edin veya kaynak denetimine gönderim yapın (kendi birim testlerinizi çalıştırdıktan sonra).

Bu yaklaşım kullanıldıktan sonra geliştirici \#2 ' in yerel kod tabanı 'nın durumu aşağıda verilmiştir.

![Güncelleştirilmiş meta veriler](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Özet

Code First Migrations bir ekip ortamında kullanırken bazı sorunlar vardır. Ancak, geçişlerin nasıl çalıştığını ve birleştirme çakışmalarını çözmek için bazı basit yaklaşımların bu zorlukları aşmak kolaylaşır.

Temel sorun, en son geçişte depolanan hatalı meta veriler. Bunun nedeni, geçerli modelin ve veritabanı şemasının eşleşip eşleşmediği ve sonraki geçişte yanlış koda sahip bir şekilde algılanmamasını Code First neden olur. Bu durum, doğru modelle boş bir geçiş üretilerek veya en son geçişte meta verileri güncelleştirerek aşılır.
