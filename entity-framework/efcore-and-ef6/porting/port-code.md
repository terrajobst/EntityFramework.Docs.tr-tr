---
title: Kod tabanlı bir Model taşıma EF çekirdek - EF6 bağlantı noktası oluşturma
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054285"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>Kod tabanlı bir EF6 modeli EF çekirdek için bağlantı noktası oluşturma

Tüm uyarılar okuduğunuz ve bağlantı noktasına hazır, başlamanıza yardımcı olacak bazı yönergeler şunlardır.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet paketi yüklemesi

EF çekirdek kullanmak için kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin. Örneğin, SQL Server hedefleme yüklemeniz `Microsoft.EntityFrameworkCore.SqlServer`. Bkz: [veritabanı sağlayıcıları](../../core/providers/index.md) Ayrıntılar için.

Geçişler kullanmayı planladığınıza sonra de yüklemeniz gerekir `Microsoft.EntityFrameworkCore.Tools` paket.

EF çekirdek ve EF6 kullanılan yan yana aynı uygulamada olabildiği kadar yüklü EF6 NuGet paketi (EntityFramework) bırakmak uygundur. Uygulamanızın tüm alanlarda EF6 kullanmayı planlayan değil, ancak paket kaldırma dikkat etmeniz gereken kod parçalarını derleme hataları vermek yardımcı olur.

## <a name="swap-namespaces"></a>Ad alanlarını değiştirme

İçinde EF6 kullanan çoğu API'leri bulunan `System.Data.Entity` ad alanı (ve ilgili alt ad alanları). İlk kod için değiştirilecek değişikliktir `Microsoft.EntityFrameworkCore` ad alanı. Genellikle türetilmiş bağlam kod dosyanızla başlatın ve sonra bunlar ortaya çıktığında derleme hataları adresleme buradan, iş.

## <a name="context-configuration-connection-etc"></a>Bağlam yapılandırma (bağlantı vs.)

Bölümünde açıklandığı gibi [olun EF çekirdek olacak iş uygulamanız için](ensure-requirements.md), EF çekirdek bağlanmak için veritabanı algılama geçici daha az Sihirli sahiptir. Geçersiz kılma gerekecek `OnConfiguring` yöntemi türetilmiş bağlamı ve veritabanı bağlantısı kurmak için veritabanı sağlayıcısı belirli API kullanın.

Çoğu EF6 uygulamaları bağlantı dizesi uygulamalarda depolamak `App/Web.config` dosya. Bu bağlantı dizesini kullanarak okunur EF çekirdek `ConfigurationManager` API. Bir başvuru eklemeniz gerekebilir `System.Configuration` framework derleme bu API kullanmanız mümkün olmayacaktır.

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

Bu noktada, derleme hataları adresleme ve davranış değişiklikleri, etkiler, görmek için kod gözden geçirme bir konudur.

## <a name="existing-migrations"></a>Varolan geçişleri

Gerçekten var olan EF6 geçişler EF çekirdek için bağlantı noktası için uygun bir yolu yoktur.

Mümkünse, varsayar EF6 önceki tüm geçişleri veritabanına uygulanmış olan ve ardından EF çekirdek kullanarak şema değerinden geçirme başlangıç noktası en iyisidir. Bunu yapmak için kullanacağınız `Add-Migration` EF çekirdek modeli bağlantı noktası kurulmuş bir kez bir geçiş eklemek için komutu. Ardından tüm koddan kaldırın `Up` ve `Down` kurulmuş geçiş yöntemleri. İlk geçişin kuruldu olduğunda sonraki geçişler modele karşılaştırın.

## <a name="test-the-port"></a>Bağlantı testi

Yalnızca uygulamanızı derler çünkü EF çekirdek başarıyla bağlantı noktalı anlamına gelmez. Test davranış değişiklikleri hiçbiri olumsuz uygulamanızı etkilenen olduğundan emin olmak için uygulamanızın tüm alanları gerekecektir.
