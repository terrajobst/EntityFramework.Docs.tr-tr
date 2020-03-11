---
title: NGen-EF6 ile başlangıç performansını artırma
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
ms.openlocfilehash: 841aec645abdb2a56076d0b70bfb2614b0acafb4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419463"
---
# <a name="improving-startup-performance-with-ngen"></a>NGen ile başlangıç performansını artırma
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

.NET Framework, uygulamaların daha hızlı başlamasını ve ayrıca bazı durumlarda daha az bellek kullanmasını sağlamak için yönetilen uygulamalar ve kitaplıklar için yerel görüntülerin oluşturulmasını destekler. Yerel görüntüler, yönetilen kod derlemeleri uygulama yürütülmeden önce yerel makine yönergeleri içeren dosyalara çevrilirken oluşturulur, .NET JıT (tam zamanında) derleyicisini şu adreste yerel yönergeler oluşturmak zorunda kalmadan uygulama çalışma zamanı.  

Sürüm 6 ' dan önce, EF Runtime 'ın temel kitaplıkları .NET Framework bir parçasıdır ve yerel görüntüler bunlar için otomatik olarak oluşturulmuştur. Sürüm 6 ' dan itibaren, tüm EF çalışma zamanı EntityFramework NuGet paketi ile birleştirilmiştir. Yerel görüntülerin artık benzer sonuçlar elde etmek için NGen. exe komut satırı aracı kullanılarak oluşturulması gerekir.  

Empırik gözlemleri, EF çalışma zamanı derlemelerinin yerel görüntülerinin, uygulama başlatma süresinin 1 ile 3 saniye arasında kesilmesini olduğunu gösterir.  

## <a name="how-to-use-ngenexe"></a>NGen. exe kullanma  

NGen. exe aracının en temel işlevi, bir derleme için yerel görüntüleri ve tüm doğrudan bağımlılıklarını "yüklemektir" (diğer bir deyişle, disk oluşturmak ve kalıcı hale getirmek için). Şunları elde edebilirsiniz:  

1. Yönetici olarak bir komut Istemi penceresi açın.
2. Geçerli çalışma dizinini yerel görüntü oluşturmak istediğiniz derlemelerin konumuyla değiştirin:

   ``` console
   cd <*Assemblies location*>  
   ```

3. İşletim sisteminize ve uygulamanın yapılandırmasına bağlı olarak, 32 bit mimarisi, 64 bit mimarisi veya her ikisi için de yerel görüntüler oluşturmanız gerekebilir.

   32 bit çalıştırması için:

   ``` console
   %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
   ```

   64 bit çalıştırması için:
  
   ``` console
   %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
   ```

> [!TIP]
> Yanlış mimari için yerel görüntülerin oluşturulması çok yaygın bir hata oluşturur. Şüpheli durumda, makinede yüklü olan işletim sistemi için uygulanan tüm mimarilerin yerel görüntülerini oluşturmanız yeterlidir.  

NGen. exe, yüklü yerel görüntüleri kaldırma ve görüntüleme, birden çok görüntünün oluşturulmasını sıraya alma, vb. gibi işlevleri de destekler. Kullanım hakkında daha fazla bilgi için [Ngen. exe belgelerini](https://msdn.microsoft.com/library/6t9t5wcf.aspx)okuyun.  

## <a name="when-to-use-ngenexe"></a>NGen. exe ne zaman kullanılır?  

Bir uygulama için, EF sürüm 6 veya üzerini temel alan bir uygulamada yerel görüntü oluşturmak üzere hangi derlemeler olduğuna karar verirken, aşağıdaki seçenekleri göz önünde bulundurmanız gerekir:  

- **Ana EF çalışma zamanı derlemesi, EntityFramework. dll**: tıpık bir EF tabanlı uygulama, başlangıçta bu derlemeden veya veritabanına ilk erişimi üzerinde önemli miktarda kod yürütür. Sonuç olarak, bu derlemenin yerel görüntülerini oluşturmak, başlangıç performansındaki en büyük kazancı oluşturacaktır.  
- **Uygulamanız tarafından kullanılan herhangi BIR EF sağlayıcı derlemesi**: başlangıç saati, bunların yerel görüntülerini oluşturmaktan biraz daha da yararlanabilir. Örneğin, uygulama SQL Server için EF sağlayıcısını kullanıyorsa, EntityFramework. SqlServer. dll için yerel bir görüntü oluşturmak isteyeceksiniz.  
- **Uygulamanızın derlemeleri ve diğer bağımlılıkları**: [Ngen. exe belgeleri](https://msdn.microsoft.com/library/6t9t5wcf.aspx) , için yerel görüntüler oluşturmak için kullanılan derlemeleri ve güvenlik üzerindeki yerel görüntülerin etkisini, "sabit bağlama" gibi gelişmiş seçenekleri, hata ayıklama ve profil oluşturma senaryolarında yerel görüntüleri kullanma gibi senaryoları, vb. içerir.  

> [!TIP]
> Yerel görüntülerin hem başlangıç performansı hem de uygulamanızın genel performansı ve gerçek gereksinimlere göre karşılaştırıldığı etkileri dikkatle ölçdiğinizden emin olun. Yerel görüntüler genellikle başlangıçtaki performansı artırmaya yardımcı olur ve bazı durumlarda bellek kullanımını azaltırken, tüm senaryolar eşit olarak avantajına sahip olur. Örneğin, kararlı durum yürütmesinde (yani, uygulama tarafından kullanılan tüm yöntemler en az bir kez çağrıldığında), JıT derleyicisi tarafından oluşturulan kod aslında yerel görüntülerden biraz daha iyi performans sağlayabilir.  

## <a name="using-ngenexe-in-a-development-machine"></a>Bir geliştirme makinesinde NGen. exe kullanma  

Geliştirme sırasında .NET JıT derleyicisi, sık değişen kod için en iyi genel zorunluluğunu getirir sunar. EF çalışma zamanı derlemeleri gibi derlenmiş bağımlılıklar için yerel görüntülerin oluşturulması, her yürütmenin başlangıcında birkaç saniyelik bir süreyi izleyerek geliştirme ve test sürecini hızlandırmaya yardımcı olabilir.  

EF çalışma zamanı derlemelerini bulmak için iyi bir yer vardır. Örneğin, SQL Server ile EF 6.0.2 kullanan bir uygulama için, .NET 4,5 veya üstünü hedeflemek için bir komut Istemi penceresine (yönetici olarak açmayı unutmayın) şunu yazabilirsiniz:  

```console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> Bu, SQL Server için doğal sağlayıcının yerel görüntülerini yüklemenin yanı sıra, ana EF çalışma zamanı derlemesinin yerel görüntülerini da yükler. Bu, NGen. exe ' nin EntityFramework. dll ' nin aynı dizinde bulunan EntityFramework. SqlServer. dll derlemesinin doğrudan bağımlılığı algılayabileceğinden, bu işe yarar.  

## <a name="creating-native-images-during-setup"></a>Kurulum sırasında yerel görüntüler oluşturma  

WiX araç seti, bu [nasıl yapılır kılavuzunda](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html)açıklandığı gibi, kurulum sırasında yönetilen derlemeler için yerel görüntülerin oluşturulmasını sıraya almayı destekler. Diğer bir seçenek de NGen. exe komutunu yürütecek özel bir kurulum görevi oluşturmaktır.  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a>Yerel görüntülerin EF için kullanıldığı doğrulanıyor  

Belirli bir uygulamanın, ". nı. dll" veya ". nı. exe" uzantısına sahip yüklü derlemeleri arayarak yerel bir derlemeyi kullandığını doğrulayabilirsiniz. Örneğin, EF 'in ana çalışma zamanı derlemesi için yerel bir görüntü EntityFramework. nı. dll olarak adlandırılacaktır. Bir işlemin yüklü .NET derlemelerini incelemenize yönelik kolay bir yol, [Işlem Gezginini](https://technet.microsoft.com/sysinternals/bb896653)kullanmaktır.  

## <a name="other-things-to-be-aware-of"></a>Bilmeniz gerekenler  

Bütünleştirilmiş **kodun yerel görüntüsünü oluşturmak, derlemeyi [GAC 'de (genel derleme önbelleği)](https://msdn.microsoft.com/library/yf1d93sz.aspx)kaydederek karıştırılmamalıdır**. NGen. exe GAC içinde olmayan derlemelerin görüntülerini oluşturmaya izin verir ve aslında belirli bir EF sürümü kullanan birden çok uygulama aynı yerel görüntüyü paylaşabilir. Windows 8 GAC 'ye yerleştirilmiş derlemeler için otomatik olarak yerel görüntüler oluşturmasının yanı sıra, EF çalışma zamanının uygulamanız ile birlikte dağıtılması en iyi duruma getirilmiştir ve bu, derleme çözümlemesi üzerinde olumsuz bir etkiye sahip olduğundan GAC 'ye kaydolmasını önermiyoruz. diğer yönleri arasında uygulamalarınıza bakım yapma.  
