---
title: Yayın planlamasını EF Core
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/release_planning.md
ms.openlocfilehash: c60040aa4acc33ba8b5a54b619539b275690f70e
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125398"
---
# <a name="release-planning-process"></a>Yayın planlama işlemi

Genellikle belirli bir yayına gitmek için belirli özellikleri nasıl seçtiğimiz hakkında sorular sunuyoruz.
Kapsamımız, tamamen yayın planlarına çevrilmez.
EF6 ' deki bir özelliğin varlığı, özelliğin EF Core uygulanması gerektiği anlamına da otomatik olarak gelmez.

Bir yayını planlamak için izlediğimiz tüm süreci ayrıntılandırıyor.
Çoğu, belirli özellikleri, fırsatları ve öncelikleri ele alır ve işlemin kendisi de her yayında gelişir.
Bununla birlikte, bir sonraki adımda nelerin çalışacağına karar verirken yanıt vermeye çalışmamız gereken yaygın soruları özetleyebiliriz:

1. **Kaç geliştirici özelliği kullanacağınızı ve uygulamalarını ya da deneyimlerini ne kadar daha iyi hale getireceğinizi düşünüyorsunuz?** Bu soruyu yanıtlamak için birçok kaynaktan geri bildirim topladık. sorunların açıklamaları ve oyları bu kaynaklardan biridir.

2. **Bu özelliği henüz uygulamadığımızda kişilerin kullanabileceği geçici çözümler nelerdir?** Örneğin, birçok geliştirici, yerel çoktan çoğa desteğin olmaması için bir JOIN tablosunu eşleyebilir. Kuşkusuz, tüm geliştiriciler bunu yapmak istememiz, ancak pek çok can ve kararımız bir faktör olarak sayılır.

3. **Bu özelliğin uygulanması, diğer özellikleri uygulamaya yaklaştırmak üzere EF Core mimarisini geliştirsin mi?** Diğer özellikler için yapı taşları görevi gören özellikleri tercih ederiz. Örneğin, özellik paketi varlıkları çoktan çoğa destekten ve varlık oluşturucularının yavaş yükleme desteğimizi etkinleştirmemize yardımcı olabilir.

4. **Özellik bir genişletilebilirlik noktası mi?** Geliştiricilerin kendi davranışlarını ve eksik işlevleri dengelamalarını sağladığından normal Özellikler üzerinde genişletilebilirlik noktalarını tercih ederiz.

5. **Diğer ürünlerle birlikte kullanıldığında özelliğin sinerjiden bahsederek denemelerini özelliği nedir?** .NET Core, Visual Studio 'nun en son sürümü, Microsoft Azure vb. gibi diğer ürünlerle EF Core kullanma deneyimini etkinleştiren veya önemli ölçüde iyileştiren Özellikler önerilir.

6. **Bir özellik üzerinde çalışmak için kullanılabilecek kişilerin becerileri nelerdir ve bu kaynaklardan en iyi şekilde yararlanın?** EF ekibinin ve topluluk Katkılarımızın her üyesi farklı alanlarda farklı deneyim düzeylerine sahiptir, bu nedenle uygun şekilde planlamanız gerekir. "Her türlü BT destesi", GroupBy çevirileri veya çoktan çoğa gibi belirli bir özellik üzerinde çalışması için pratik olmasa bile.

Daha önce belirtildiği gibi, süreç her yayında gelişir.
Gelecekte, yayın planlarımıza giriş sağlamak üzere topluluk üyeleri için daha fazla fırsat eklemeyi deneyeceğiz.
Örneğin, özelliklerin taslak tasarımlarını ve yayın planının kendisini incelemeyi kolaylaştırmak istiyoruz.

## <a name="backlog"></a>Biriktirme

Sorun izleyicimizin [kapsam kilometre taşı](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) , Someday üzerinde çalışmayı umduğumuz sorunları içeriyor ya da topluluktan birisinin başka bir kişi tarafından üstesinden geldiğini düşünüyoruz.
Müşteriler bu sorunlara yorum ve oyları göndermek için hoş geldiniz.
Bu sorunlardan herhangi biriyle çalışmayı isteyen katkıda bulunanlar, ilk olarak bu sorunların nasıl yaklaşımıyla ilgili bir tartışma başlatmaları önerilir.

EF Core belirli bir sürümünde, belirli bir özellik üzerinde çalışacağımız hiçbir garanti yoktur.
Tüm yazılım projelerinde, öncelikler, yayın zamanlamaları ve kullanılabilir kaynaklar herhangi bir noktada değişebilir.
Ancak belirli bir zaman çerçevesinde bir sorunu çözmeyi düşünüyorsanız, bunu kapsam kilometre taşı yerine bir yayın kilometre taşına atayacağız.

Bu sorunu çözmek için planlamadığımızda, büyük olasılıkla bir sorunu kapatacağız.
Ancak bununla ilgili yeni bilgiler aldığımızda, daha önce kapattığınız bir sorunu yeniden düşünebiliriz.
