---
title: NGen - EF6 ile başlangıç performansı iyileştirme
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
caps.latest.revision: 4
ms.openlocfilehash: cffd2deea3148a16ed704d1e5e7b365eda06f72b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949051"
---
# <a name="improving-startup-performance-with-ngen"></a>NGen ile başlangıç performansı iyileştirme
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

.NET Framework yönetilen uygulamalar için yerel görüntüleri oluşturulmasını destekler ve daha az bellek kitaplıkları uygulamaları daha hızlı başlayın sağlayacak bir yol olarak ve bazı durumlarda da kullanabilirsiniz. Yerel görüntüler uygulama yürütülmeden önce yerel makine yönergelerine içeren dosyaları yerel yönergelerini oluşturmak zorunda kalmadan .NET JIT (Just-ın-Time) derleyicinin yoğun yönetilen kod derlemelerine çevirerek oluşturulur Uygulama çalışma zamanı.  

Sürüm 6 önce EF çalışma zamanının çekirdek kitaplıkları .NET Framework'ün bir parçası olan ve yerel görüntü için bunları otomatik olarak oluşturulmadı. Sürüm 6 ile başlayarak tüm EF çalışma zamanı EntityFramework NuGet paket birleştirilmiştir. Yerel görüntüler olması için şimdi oluşturulacak benzer sonuçlar elde etmek için NGen.exe komut satırı aracını kullanarak.  

EF çalışma zamanı derlemeleri, yerel görüntüler uygulama başlangıç zamanı 1-3 saniye arasında daraltabilirsiniz görgül gözlemler gösterir.  

## <a name="how-to-use-ngenexe"></a>NGen.exe kullanma  

En temel NGen.exe Aracı'nın "yüklemek için" işlevidir (diğer bir deyişle, oluşturmak ve diske kalıcı hale getirmek için) bir derleme ve tüm doğrudan bağımlılıkları için yerel görüntüler. İşte, nasıl gerçekleştirebilirsiniz:  

1. Yönetici olarak bir komut istemi penceresi açın  
2. Geçerli çalışma dizini için yerel görüntüleri oluşturmak istediğiniz derlemelerin konumunu değiştirin:  

  ``` console
    cd <*Assemblies location*>  
  ```
3. İşletim sisteminiz ve uygulama yapılandırmasına bağlı olarak, 32 bit mimari, 64 bit mimari için veya her ikisi de için yerel görüntüler Oluştur gerekebilir.  

    32 çalıştırma bit için:  
  ``` console
    %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
  ```
    64 çalıştırma bit için:
  ``` console
    %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
  ```

> [!TIP]
> Yanlış mimarisi için yerel görüntüler oluşturuluyor çok yaygın bir hata var. Zaman şüpheli yeterlidir makinede yüklü olan işletim sistemi için geçerli tüm mimariler için yerel görüntüleri oluşturun.  

NGen.exe, birden çok resimler, vb. nesil queuing yüklü yerel görüntüleri görüntüleme ve kaldırma gibi diğer işlevleri de destekler. Kullanımı daha fazla ayrıntı için okuma [NGen.exe belgeleri](https://msdn.microsoft.com/library/6t9t5wcf.aspx).  

## <a name="when-to-use-ngenexe"></a>NGen.exe kullanıldığı durumlar  

EF 6 veya üzeri bir sürümü üzerinde tabanlı bir uygulama için yerel görüntüleri oluşturmak için hangi derlemelerin karar vermek için söz konusu olduğunda, aşağıdaki seçenekleri dikkate almanız gerekir:  

- **Ana EF çalışma zamanı derlemesi EntityFramework.dll**: genel bir temel EF uygulama yürütür veritabanında önemli miktarda kod başlangıç veya ilk erişimini, bu derlemedeki. Sonuç olarak, bu derlemenin yerel görüntüler oluşturma başlangıç performansı büyük kazanç oluşturur.  
- **Uygulamanız tarafından kullanılan tüm EF sağlayıcı derlemesi**: başlangıç zamanı da avantajını biraz oluşturmasını bu yerel görüntüler. Örneğin, uygulamanın EF sağlayıcısı için SQL Server kullanıyorsa, yerel görüntü için EntityFramework.SqlServer.dll oluşturmak isteyeceksiniz.  
- **Uygulamanızın derlemeleri ve diğer bağımlılıklar**: [NGen.exe belgeleri](https://msdn.microsoft.com/library/6t9t5wcf.aspx) için yerel görüntüler ve güvenlik, yerel görüntüler etkisini oluşturmak için hangi derlemelerin seçmeye yönelik genel ölçüt kapsar Gelişmiş Seçenekleri "sıkı bağlama" gibi hata ayıklama ve profil oluşturma senaryoları, vb. yerel görüntüleri kullanan gibi senaryoları.  

> [!TIP]
> Dikkatli bir şekilde hem başlangıç performansını hem de uygulamanızın genel performansını yerel görüntüleri kullanan etkilerini ölçmek ve karşılaştırmak içinse karşı Gerçek gereksinimler emin olun. Yerel görüntüler genellikle performans dökümünü başlangıç artırmak ve bazı durumlarda bellek kullanımını azaltmaya yardımcı olur, ancak tüm senaryolarda eşit yararlı olacaktır. (Diğer bir deyişle, uygulama tarafından kullanılan tüm yöntemler en az bir kez çağrılabilir sonra), kararlı bir duruma yürütülmesine JIT derleyicisi tarafından üretilen kod aslında yerel görüntülerden biraz daha iyi performans sağlayabilir.  

## <a name="using-ngenexe-in-a-development-machine"></a>NGen.exe bir geliştirme makinesinde kullanma  

Geliştirme sırasında .NET JIT Derleyici sık değişen kod için en iyi genel artırabilen sunar. EF çalışma zamanı derlemeleri gibi derlenmiş bağımlılıkları için yerel görüntüler oluşturuluyor, geliştirme ve test birkaç saniye her yürütme başlangıcında kesme göre hızlandırın yardımcı olabilir.  

Çözüm için NuGet paket konumu EF çalışma zamanı derlemeleri bulmak için iyi bir yerdir. Örneğin, SQL Server ile EF 6.0.2 kullanma ve .NET 4.5 veya üzeri hedefleyen bir uygulama için aşağıdaki komut istemi penceresinde yazabilirsiniz (yönetici olarak açın unutmayın):  

``` console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Bu, yerel görüntüler için SQL Server EF sağlayıcısı yükleme de varsayılan olarak yerel görüntüler için ana EF çalışma zamanı derlemesi yüklenir olgu yararlanır. NGen.exe EntityFramework.dll aynı dizinde bulunan EntityFramework.SqlServer.dll bütünleştirilmiş kodun doğrudan bir bağımlılık olduğunu algılamak için bu çalışır.  

## <a name="creating-native-images-during-setup"></a>Kurulum sırasında yerel görüntüler oluşturma  

WiX araç destekler queuing yerel görüntülerin Kurulum sırasında oluşturma Yönetilen derlemeler için bu konuda açıklandığı gibi [yapılır kılavuzunda](http://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html). Başka bir alternatif NGen.exe komutu yürüterek bir özel kurulum görevi oluşturmaktır.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Yerel görüntüler için EF kullanıldığını doğrulanıyor  

Uzantı yüklenen derlemeler için bakarak belirli bir uygulamanın yerel bir derleme kullandığından emin olun ". ni.dll"veya". ni.exe". Örneğin, bir doğal görüntü EF'ın ana çalışma zamanı derlemesi için EntityFramework.ni.dll çağrılmaz. Bir işlemin yüklenen .NET derlemeleri incelemek için kolay bir yol kullanmaktır [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).  

## <a name="other-things-to-be-aware-of"></a>Dikkat edilmesi gereken diğer noktalar  

**Bir derlemenin yerel görüntüsünü oluşturma karıştırılmamalıdır derlemesinde kaydetme ile [GAC'ye (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**. NGen.exe derlemeleri GAC'ye olmayan görüntülerini oluşturma sağlar ve EF belirli bir sürümünü kullanan birden çok uygulama aynı yerel görüntü aslında paylaşabilirsiniz. Windows 8 GAC'de yerleştirilen derlemeler için yerel görüntüleri otomatik olarak oluşturmanız mümkün olsa da EF çalışma zamanı yanı sıra, uygulamanızın dağıtılması için optimize edilmiştir ve bu derleme çözümlemesine üzerinde olumsuz bir etkiye sahip olduğundan GAC'ye kaydetme önerilmez ve uygulamalarınızı diğer yönleri arasında hizmet.  
