---
title: EF6 'den EF Core taşıma-kod tabanlı model-EF 'e taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419639"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>EF6 kod tabanlı bir modelin EF Core için taşıma

Tüm uyarıları okuduğunuzda ve bağlantı noktasına hazırsanız, kullanmaya başlamanıza yardımcı olacak bazı yönergeler aşağıda verilmiştir.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet paketlerini yükler

EF Core kullanmak için, kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yüklersiniz. Örneğin, SQL Server hedeflenirken `Microsoft.EntityFrameworkCore.SqlServer`yüklersiniz. Ayrıntılar için bkz. [veritabanı sağlayıcıları](../../core/providers/index.md) .

Geçişleri kullanmayı planlıyorsanız, `Microsoft.EntityFrameworkCore.Tools` paketini de yüklemelisiniz.

EF Core ve EF6 'nin aynı uygulamada yan yana kullanılabilmesi için, EF6 NuGet paketini (EntityFramework) yüklü bırakmak çok iyidir. Ancak, uygulamanızın herhangi bir alanında EF6 kullanmayı hiç bilmiyorsanız, paketin kaldırılması, dikkat edilmesi gereken kod parçalarında derleme hataları sağlamaya yardımcı olur.

## <a name="swap-namespaces"></a>Ad alanlarını değiştirme

EF6 içinde kullandığınız çoğu API 'Ler `System.Data.Entity` ad alanında (ve ilgili alt ad alanlarında) bulunur. İlk kod değişikliği `Microsoft.EntityFrameworkCore` ad alanına takas etmek. Genellikle, türetilmiş bağlam kodu dosyanız ile başlayıp daha sonra, meydana gelen derleme hatalarını adresleyen bir şekilde ele almanız gerekir.

## <a name="context-configuration-connection-etc"></a>Bağlam yapılandırması (bağlantı vb.)

[EF Core uygulamanız Için çalıştığından emin olun](ensure-requirements.md), EF Core bağlanılacak veritabanını algılayarak daha az bir sihirli olur. Türetilmiş bağlamınızda `OnConfiguring` yöntemini geçersiz kılmanız ve veritabanına bağlantıyı kurmak için veritabanı sağlayıcısına özgü API 'yi kullanmanız gerekir.

Çoğu EF6 uygulaması bağlantı dizesini uygulamalar `App/Web.config` dosyasında depolar. EF Core, `ConfigurationManager` API 'sini kullanarak bu bağlantı dizesini okuyabilirsiniz. Bu API 'yi kullanabilmek için `System.Configuration` Framework derlemesine bir başvuru eklemeniz gerekebilir.

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

## <a name="update-your-code"></a>Kodunuzu güncelleştirme

Bu noktada, bu durum derleme hatalarının adreslenmesi ve kodu gözden geçirerek davranış değişikliklerinin sizi etkileyebileceğini göz atalım.

## <a name="existing-migrations"></a>Mevcut geçişler

EF Core için mevcut EF6 geçişlerini bağlantı noktası için uygun bir yol yoktur.

Mümkünse, EF6 ' den önceki tüm geçişlerin veritabanına uygulandığını varsaymak ve sonra EF Core kullanarak şemayı bu noktadan geçirmeye başlamanız en iyisidir. Bunu yapmak için, model EF Core bağlantı kurulduktan sonra bir geçiş eklemek için `Add-Migration` komutunu kullanın. Ardından, tüm kodu, yapı iskelesi geçişinin `Up` ve `Down` yöntemlerinden kaldırırsınız. Sonraki geçişler, ilk geçiş iskele alındığı zaman modeliyle karşılaştırılır.

## <a name="test-the-port"></a>Bağlantı noktasını test etme

Uygulamanız derlendiğinden, EF Core başarılı bir şekilde bir şekilde bağlantı edildiği anlamına gelmez. Davranış değişikliklerinin hiçbirinin uygulamanızı olumsuz yönde etkilememesini sağlamak için uygulamanızın tüm bölümlerini test etmeniz gerekir.
