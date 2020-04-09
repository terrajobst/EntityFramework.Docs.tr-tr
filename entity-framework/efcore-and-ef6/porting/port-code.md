---
title: EF6'dan EF Core'a Taşıma - Kod Tabanlı Model Taşıma - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78419639"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>EF6 Kod Tabanlı Modeli EF Core'a Taşıma

Tüm uyarılar okuduysanız ve bağlantı noktasına gitmeye hazırsanız, başlamanıza yardımcı olacak bazı yönergeler burada dır.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet paketlerini yükleyin

EF Core'u kullanmak için Kullanmak Istediğiniz veritabanı sağlayıcısı için NuGet paketini yüklersiniz. Örneğin, SQL Server'ı hedeflerken. `Microsoft.EntityFrameworkCore.SqlServer` Ayrıntılar için [Veritabanı Sağlayıcıları'na](../../core/providers/index.md) bakın.

Geçişleri kullanmayı planlıyorsanız, `Microsoft.EntityFrameworkCore.Tools` paketi de yüklemeniz gerekir.

EF Core ve EF6 aynı uygulamada yan yana kullanılabildiği için EF6 NuGet paketini (EntityFramework) yüklü bırakmak iyidir. Ancak, UYGULAMANIZIN herhangi bir alanında EF6'yı kullanmak istemiyorsanız, paketi kaldırmanız dikkat gerektiren kod parçaları üzerinde derleme hataları na yardımcı olacaktır.

## <a name="swap-namespaces"></a>Ad boşluklarını değiştirme

EF6'da kullandığınız API'lerin `System.Data.Entity` çoğu ad alanında (ve ilgili alt ad alanlarında) bulunur. İlk kod değişikliği `Microsoft.EntityFrameworkCore` ad alanına geçiş yapmaktır. Genellikle türemiş bağlam kodu dosyanızla başlar ve ardından derleme hatalarını oluştukça ele alınarak buradan çalışırsınız.

## <a name="context-configuration-connection-etc"></a>Bağlam yapılandırması (bağlantı vb.)

[EF Core'un Uygulamanız için Çalışacağını Garanti](ensure-requirements.md)Edin'de açıklandığı gibi, EF Core bağlanmak için veritabanını algılama konusunda daha az sihrivardır. Türemiş bağlamınızdaki `OnConfiguring` yöntemi geçersiz kılmanız ve veritabanına bağlantıyı kurmak için veritabanı sağlayıcısına özgü API kullanmanız gerekir.

ÇOĞU EF6 uygulaması bağlantı dizesini `App/Web.config` uygulamalar dosyasında depolar. EF Core'da, API'yi `ConfigurationManager` kullanarak bu bağlantı dizesini okudunuz. Bu API'yi kullanabilmek için `System.Configuration` çerçeve derlemesine bir başvuru eklemeniz gerekebilir.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a>Kodunuzu güncelleştirin

Bu noktada, davranış değişikliklerinin sizi etkileyip etkilemeyeceğini görmek için derleme hatalarını ele alma ve kodu gözden geçirme meselesidir.

## <a name="existing-migrations"></a>Varolan geçişler

Mevcut EF6 geçişlerini EF Core'a taşımanın uygun bir yolu yoktur.

Mümkünse, EF6'dan önceki tüm geçişlerin veritabanına uygulandığını varsaymak ve daha sonra EF Core kullanarak şemayı bu noktadan geçirerek başlamak en iyisidir. Bunu yapmak için, model `Add-Migration` EF Core'a taşınırken geçiş eklemek için komutu kullanırsınız. Daha sonra iskele geçişive `Up` `Down` yöntemleri tüm kodu kaldırmak istiyorsunuz. Sonraki geçişler, ilk geçiş indiğinde modelle karşılaştırılır.

## <a name="test-the-port"></a>Bağlantı noktasını test edin

Uygulamanızın derlenmiş olması, uygulamanın ef core'a başarıyla iletilmiş olduğu anlamına gelmez. Davranış değişikliklerinin hiçbirinin uygulamanızı olumsuz etkilemediğinden emin olmak için uygulamanızın tüm alanlarını test etmeniz gerekir.
