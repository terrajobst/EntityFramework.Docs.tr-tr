---
title: Tasarımcı kod oluşturma şablonlar - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: e4e99a86e7c273682c85eba06042af9a2a837d12
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283868"
---
# <a name="designer-code-generation-templates"></a>Tasarımcı kod oluşturma şablonları
Entity Framework Designer kullanarak model oluşturduğunuzda sınıfları ve türetilmiş bağlamı otomatik olarak sizin için oluşturulur. Varsayılan kod oluşturma yanı sıra ayrıca çeşitli oluşturulan kod özelleştirmek için kullanılan şablonları sunuyoruz. Bu şablonlar, gerekirse şablonları özelleştirmenize olanak tanıyacak T4 metin şablonları sağlanır.

Varsayılan olarak oluşturulan kod, modelinizi oluşturmak Visual Studio'nun hangi sürümünün bağlıdır:

-   Visual Studio 2012 ve 2013 içinde oluşturulan modeller basit POCO varlık sınıfları ve Basitleştirilmiş DbContext türeyen bir bağlam oluşturur.
-   Visual Studio 2010'da oluşturulmuş modelleri EntityObject ve ObjectContext türetilen bağlamı öğesinden türetilen varlık sınıfları oluşturur.
  > [!NOTE]    
  > DbContext Oluşturucu şablona modelinizi ekledikten sonra geçiş öneririz.

Bu sayfa, kullanılabilir şablonların kapsar ve ardından modelinize şablonu ekleme yönergeleri sağlar.

## <a name="available-templates"></a>Kullanılabilir şablonlar

Aşağıdaki şablonlar Entity Framework ekibi tarafından sağlanır:

### <a name="dbcontext-generator"></a>DbContext Oluşturucusu

Bu şablon, basit POCO varlık sınıfları ve EF6 kullanarak DbContext türeyen bir bağlam oluşturur.
Aşağıda listelenen diğer şablonlardan birini kullanmak için bir nedeniniz yoksa bu önerilen bir şablonudur.
De size varsayılan (Visual Studio 2013 ve üzeri) Visual Studio'nun en son sürümleri kullanıyorsanız kod kuşak şablonu olabilir: Bu şablon yeni bir modeli oluşturduğunuzda varsayılan olarak kullanılır ve .edmx dosyanızın altında iç içe T4 dosyaları (.tt).

#### <a name="older-versions-of-visual-studio"></a>Visual Studio'nun eski sürümleri
- **Visual Studio 2012:** almak için **EF 6.x DbContextGenerator** şablonları en son yükleme gerekecek **Entity Framework Araçları Visual Studio için** -bkz [varlık alma Framework](~/ef6/fundamentals/install.md) daha fazla bilgi için.
- **Visual Studio 2010:** **EF 6.x DbContextGenerator** şablonları, Visual Studio 2010 için kullanılabilir değildir.

#### <a name="dbcontext-generator-for-ef-5x"></a>EF için DbContext oluşturucuyu 5.x

EntityFramework NuGet paketini (biri bir ana sürüm 5) daha eski bir sürümünü kullanıyorsanız, kullanmanız gerekecektir **EF 5.x DbContext Oluşturucu** şablonu.

Visual Studio 2013 veya 2012 kullanıyorsanız, bu şablon zaten yüklü.

Visual Studio 2010 kullanıyorsanız seçmeniz gerekecek **çevrimiçi** sekmesinde Visual Studio Galerisi'nden yüklemek için şablon eklerken. Alternatif olarak, doğrudan Visual Studio Galerisi önceden şablonu yükleyebilirsiniz. Şablonları Visual Studio'nun sonraki sürümlerinde dahil olmadığından galeri sürümlerinde yalnızca Visual Studio 2010'da yüklenebilir.

- [EF 5.x DbContext oluşturucusu için C#](https://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [C# Web siteleri için EF 5.x DbContext Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [VB.NET için EF 5.x DbContext Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [VB.NET Web siteleri için EF 5.x DbContext Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>EF için DbContext oluşturucuyu 4.x

EntityFramework NuGet paketi (bir ana sürüm 4 ile) daha eski bir sürümünü kullanıyorsanız, kullanmanız gerekecektir **EF 4.x DbContext Oluşturucu** şablonu. Bu bulunabilir **çevrimiçi** sekmesinde şablonu eklerken veya şablon önceden Visual Studio Galerisi doğrudan yükleyebilirsiniz.

- [EF 4.x DbContext oluşturucusu için C#](https://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [C# Web siteleri için EF 4.x DbContext Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [VB.NET için EF 4.x DbContext Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [VB.NET Web siteleri için EF 4.x DbContext Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>EntityObject Oluşturucusu

Bu şablon EntityObject ve ObjectContext türetilen bağlamı öğesinden türetilen varlık sınıfları oluşturur.

> [!NOTE]
> DbContext Oluşturucu kullanmayı düşünün

DbContext Oluşturucusu artık önerilen yeni uygulamalar için şablonudur. DbContext Oluşturucu daha basit bir DbContext API yararlanır. EntityObject Oluşturucu, mevcut uygulamaları desteklemek kullanılabilir olmaya devam eder.

**Visual Studio 2010, 2012 &amp; 2013**

Seçmeniz gerekecek **çevrimiçi** sekmesinde Visual Studio Galerisi'nden yüklemek için şablon eklerken. Alternatif olarak, doğrudan Visual Studio Galerisi önceden şablonu yükleyebilirsiniz.

- [EF 6.x EntityObject oluşturucusu için C#](https://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [C# Web siteleri için EF 6.x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [VB.NET için EF 6.x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [VB.NET Web siteleri için EF 6.x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**EF için EntityObject oluşturucuyu 5.x**


Visual Studio 2012 veya 2013 kullanıyorsanız seçmeniz gerekecek **çevrimiçi** sekmesinde Visual Studio Galerisi'nden yüklemek için şablon eklerken. Alternatif olarak, doğrudan Visual Studio Galerisi önceden şablonu yükleyebilirsiniz. Şablonları Visual Studio 2010'da dahil olmadığından galeri sürümlerinde yalnızca Visual Studio 2012'ye yüklenebilir &amp; 2013.

- [EF 5.x EntityObject oluşturucusu için C#](https://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [C# Web siteleri için EF 5.x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [VB.NET için EF 5.x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [VB.NET Web siteleri için EF 5.x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Yapabilecekleriniz şablonu Düzen gerek kalmadan yalnızca ObjectContext kod oluşturma istiyorsanız [EntityObject kod oluşturma için geri](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Visual Studio 2010 kullanıyorsanız, bu şablon zaten yüklü. Visual Studio 2010'da yeni bir model oluşturursanız, bu şablon, varsayılan ancak .tt dosyaları projenize dahil edilmez tarafından kullanılır. Şablonu özelleştirmek istiyorsanız projenize eklemeniz gerekir.

### <a name="self-tracking-entities-ste-generator"></a>Kendi kendine izleme varlıkları (Yapıştır) Oluşturucusu

Bu şablon Self-Tracking varlık sınıfları ve ObjectContext türeyen bir bağlam oluşturur. EF uygulamada varlıkları değişiklikleri izlemek için bir bağlam sorumludur. Bununla birlikte, N katmanlı senaryolarda bağlamı varlıkları değiştiren katmanda kullanılabilir olmayabilir. Kendi kendine izleme varlıkları tüm katman değişiklikleri izlemenize yardımcı olur. Daha fazla bilgi için [Self-Tracking varlıkları](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> Ste'leri birleştirme şablon önerilmez

Artık yeni uygulamalarda Pıştırarak şablon kullanmanızı öneririz, mevcut uygulamaları desteklemek kullanılabilir olmaya devam eder. Ziyaret [bağlantısı kesilmiş varlıklar makale](~/ef6/fundamentals/disconnected-entities/index.md) diğer seçeneklerin N katmanlı senaryolar için önerilir.

> [!NOTE]
> Ste'leri birleştirme şablonun EF 6.x sürümü yoktur.


> [!NOTE]
> Visual Studio 2013 sürümü Pıştırarak şablonu yoktur.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Visual Studio 2012 kullanıyorsanız seçmeniz gerekecek **çevrimiçi** sekmesinde Visual Studio Galerisi'nden yüklemek için şablon eklerken. Alternatif olarak, doğrudan Visual Studio Galerisi önceden şablonu yükleyebilirsiniz. Şablonları Visual Studio 2010'da dahil olmadığından galeri sürümlerinde yalnızca Visual Studio 2012'de yüklenebilir.

- [EF 5.x Pıştırarak oluşturucusu için C#](https://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [C# Web siteleri için EF 5.x Pıştırarak Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [VB.NET için EF 5.x Pıştırarak Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [VB.NET Web siteleri için EF 5.x Pıştırarak Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010 **

Visual Studio 2010 kullanıyorsanız, bu şablon zaten yüklü.

### <a name="poco-entity-generator"></a>POCO varlık Oluşturucu

Bu şablon POCO varlık sınıfları ve ObjectContext türeyen bir bağlam oluşturur

> [!NOTE]
> DbContext Oluşturucu kullanmayı düşünün

DbContext Oluşturucusu artık önerilen yeni uygulamalar oluşturma POCO sınıfların şablonudur. DbContext Oluşturucusu yeni DbContext API yararlanır ve daha basit POCO sınıfları oluşturabilirsiniz. POCO varlık Oluşturucu, mevcut uygulamaları desteklemek kullanılabilir olmaya devam eder.

> [!NOTE]
> Hiçbir EF yoktur 5.x veya EF 6.x Pıştırarak şablon sürümü.

> [!NOTE]
> Visual Studio 2013 sürümü POCO şablonu yoktur.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

Seçmeniz gerekecek **çevrimiçi** sekmesinde Visual Studio Galerisi'nden yüklemek için şablon eklerken. Alternatif olarak, doğrudan Visual Studio Galerisi önceden şablonu yükleyebilirsiniz.

- [EF 4.x POCO oluşturucusu için C#](https://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [C# Web siteleri için EF 4.x POCO Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [VB.NET için EF 4.x POCO Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [VB.NET Web siteleri için EF 4.x POCO Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>"Web siteleri" şablonları nelerdir

"Web siteleri" şablonları (örneğin, **EF 5.x DbContext Oluşturucu c\# Web siteleri**) aracılığıyla oluşturulan Web sitesi projeleri kullanmak içindir **dosya -&gt; New -&gt; Web sitesi...** . Aracılığıyla oluşturulan Web uygulamaları, farklı bunlar **dosya -&gt; New -&gt; proje...** , standart şablonları kullanın. Visual Studio öğesi şablonu sistemde gerektirdiğinden ayrı şablonlar sunuyoruz.

## <a name="using-a-template"></a>Şablon kullanma

Kod oluşturma şablonu kullanmaya başlamak için EF tasarımcısında tasarım yüzeyinde boş bir nokta sağ tıklayıp **kod oluşturma öğesi Ekle...** .

![Kod Genel öğesi Ekle](~/ef6/media/add-code-gen-item.png)

Şablon zaten yüklediyseniz, kullanmak istediğiniz (veya Visual Studio'ya dahil), ya da altında kullanılabilir olmayacaktır **kod** veya **veri** sol menüden bölümü.

![Yüklü](~/ef6/media/installed.png)

Yüklü şablon zaten yoksa, seçin **çevrimiçi** ve sol menüden istediğiniz şablonu arayın.

![Ara](~/ef6/media/search.png) 

Visual Studio 2012 kullanıyorsanız, .edmx file.* altında yeni .tt dosyaları içe yerleştirilir

> [!NOTE]
> Varsayılan kod oluşturma için kullanılan şablonları silmeniz gerekecektir Visual Studio 2012'de oluşturulan modeller için Aksi halde yinelenen sınıflar ve oluşturulan bağlam gerekir. Varsayılan dosyalar  **&lt;model adı&gt;.tt** ve  **&lt;model adı&gt;. context.tt**. 

![VS2012 şablonları](~/ef6/media/vs2012-templates.png)

Visual Studio 2010 kullanıyorsanız, tt dosyalarını doğrudan projenize eklenir.  

![VS2010 şablonları](~/ef6/media/vs2010-templates.png)
