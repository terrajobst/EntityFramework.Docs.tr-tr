---
title: EF6'dan EF Core'a taşıma - kod tabanlı bir modeli taşıma
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997055"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>EF6 kod tabanlı bir modeli EF core'a taşıma

Tüm uyarılar makaleyi okudunuz ve bağlantı noktası için hazır olursunuz, başlamanıza yardımcı olmak için bazı Kılavuzlar şunlardır.

## <a name="install-ef-core-nuget-packages"></a>EF Core NuGet paketi yüklemesi

EF Core kullanmak için kullanmak istediğiniz veritabanı sağlayıcısı için NuGet paketini yükleyin. Örneğin, SQL Server hedeflenirken yüklemeniz `Microsoft.EntityFrameworkCore.SqlServer`. Bkz: [veritabanı sağlayıcıları](../../core/providers/index.md) Ayrıntılar için.

Geçişleri kullanmayı planladığınıza sonra de yüklemeniz gerekir `Microsoft.EntityFrameworkCore.Tools` paket.

EF Core ve EF6 kullanılan yan yana aynı uygulamada olması gibi yüklü EF6 NuGet paketini (EntityFramework) bırakmak uygundur. Uygulamanızın tüm alanlarda EF6'ı kullanmayı planlayan değildir, ancak sonra paket kaldırılıyor dikkat etmeniz gereken kod parçaları derleme hatalarını vermek yardımcı olur.

## <a name="swap-namespaces"></a>Ad alanlarını değiştirme

EF6 içinde kullanan çoğu API'ler bulunan `System.Data.Entity` ad alanı (ve ilgili alt ad alanları). İlk kod değişikliği için değiştirilecek olan `Microsoft.EntityFrameworkCore` ad alanı. Genellikle türetilmiş bağlam kodu dosyanızla başlatın ve ardından derleme hatası oluşunca adresleme buradan iş.

## <a name="context-configuration-connection-etc"></a>Bağlam içi yapılandırma (bağlantı vs.)

Bölümünde anlatıldığı gibi [olun EF Core olacak iş uygulamanız için](ensure-requirements.md), EF Core olan algılama bağlanmak için veritabanı geçici olarak daha az Sihirli. Geçersiz kılmak ihtiyacınız olacak `OnConfiguring` yöntemi türetilmiş bağlam ve veritabanı bağlantısı kurmak için veritabanı sağlayıcısı özel API'yi kullanın.

EF6 uygulamaların çoğu uygulamalarında bağlantı dizesini depolama `App/Web.config` dosya. Bu bağlantı dizesini kullanarak EF Core okuma `ConfigurationManager` API. Bir başvuru eklemeniz gerekebilir `System.Configuration` bu API'yi kullanabilmek için framework derlemesi.

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

Bu noktada, derleme hataları çözdükten ve davranış değişiklikleri, etkiler, görmek için kod gözden geçirme bir konudur.

## <a name="existing-migrations"></a>Mevcut geçişleri

Gerçekten mevcut EF6 geçişler EF Core için bağlantı noktası için uygun bir yolu yoktur.

Mümkünse, veritabanına EF6'dan önceki tüm geçişler uygulanmış olan ve ardından EF Core kullanarak, şema geçişi başlangıç noktası varsayılır en iyisidir. Bunu yapmak için kullanacağınız `Add-Migration` modeli, EF Core için unity'nin sonra bir geçiş eklemek için komutu. Ardından tüm koddan kaldırın `Up` ve `Down` iskele kurulmuş geçiş yöntemleri. Bu ilk geçiş iskele kurulmuş zaman sonraki geçişleri modele karşılaştıracağız.

## <a name="test-the-port"></a>Bağlantı testi

Yalnızca uygulamanızı derleyen çünkü başarıyla EF Core için unity'nin anlamına gelmez. Tüm alanları davranışı değişikliklerin hiçbiri olumsuz uygulamanızı etkilemiş emin olmak için uygulamanızı test etmek gerekir.
