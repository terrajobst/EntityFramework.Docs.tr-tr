---
title: Tasarımcı Kodu Oluşturma Şablonları - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: e4e99a86e7c273682c85eba06042af9a2a837d12
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78418678"
---
# <a name="designer-code-generation-templates"></a>Tasarımcı Kodu Oluşturma Şablonları
Varlık Çerçeve Tasarımcısı'nı kullanarak bir model oluşturduğunuzda, sınıflarınız ve türetilmiş bağlam ınız sizin için otomatik olarak oluşturulur. Varsayılan kod oluşturmaek olarak oluşturulan kodu özelleştirmek için kullanılabilecek şablonlar bir dizi de sağlar. Bu şablonlar, gerekirse şablonları özelleştirmenize olanak tanıyan T4 Metin Şablonları olarak sağlanır.

Varsayılan olarak oluşturulan kod, modelinizi oluşturduğunuz Visual Studio sürümüne bağlıdır:

-   Visual Studio 2012 & 2013'te oluşturulan modeller basit POCO varlık sınıfları ve basitleştirilmiş DbContext'dan türeyen bir bağlam oluşturacaktır.
-   Visual Studio 2010'da oluşturulan modeller EntityObject'ten ve ObjectContext'dan türeyen bir bağlamdan türeyen varlık sınıfları oluşturur.
  > [!NOTE]    
  > Modelinizi ekledikten sonra DbContext Generator şablonuna geçmenizi öneririz.

Bu sayfa kullanılabilir şablonları kapsar ve ardından modelinize şablon eklemek için yönergeler sağlar.

## <a name="available-templates"></a>Kullanılabilir Şablonlar

Aşağıdaki şablonlar Varlık Çerçevesi ekibi tarafından sağlanır:

### <a name="dbcontext-generator"></a>DbContext Jeneratör

Bu şablon, basit POCO varlık sınıfları ve EF6 kullanarak DbContext türetilen bir bağlam oluşturur.
Aşağıda listelenen diğer şablonlardan birini kullanmak için bir nedeniniz yoksa, bu önerilen şablondur.
Visual Studio'nun (Visual Studio 2013) son sürümlerini kullanıyorsanız varsayılan olarak aldığınız kod oluşturma şablonudur: Yeni bir model oluşturduğunuzda bu şablon varsayılan olarak kullanılır ve T4 dosyaları (.tt) .edmx dosyanızın altına yer alır.

#### <a name="older-versions-of-visual-studio"></a>Visual Studio'nun eski sürümleri
- **Görsel Stüdyo 2012:** **EF 6.x DbContextGenerator** şablonlarını almak için Visual **Studio için** en son Varlık Çerçeve Araçlarını yüklemeniz gerekir - daha fazla bilgi için [Varlık Çerçevesi](~/ef6/fundamentals/install.md) Sayfasına bakın.
- **Görsel Stüdyo 2010:** **EF 6.x DbContextGenerator** şablonları Visual Studio 2010 için kullanılamaz.

#### <a name="dbcontext-generator-for-ef-5x"></a>EF 5.x için DbContext Jeneratör

EntityFramework NuGet paketinin eski bir sürümünü kullanıyorsanız (5 ana sürümü olan bir sürüm) **EF 5.x DbContext Generator** şablonu kullanmanız gerekir.

Visual Studio 2013 veya 2012 kullanıyorsanız bu şablon zaten yüklenmiş.

Visual Studio 2010 kullanıyorsanız, Visual Studio Gallery'den indirmek için şablonu eklerken **Çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak şablonu doğrudan Visual Studio Gallery'den önceden yükleyebilirsiniz. Şablonlar Visual Studio'nun sonraki sürümlerinde yer aldığı için galerideki sürümler yalnızca Visual Studio 2010'a yüklenebilir.

- [C için EF 5.x DbContext Jeneratör #](https://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [C# Web Siteleri için EF 5.x DbContext Jeneratör](https://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [VB.NET için EF 5.x DbContext Jeneratör](https://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [VB.NET Web Siteleri için EF 5.x DbContext Jeneratör](https://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>EF 4.x için DbContext Jeneratör

EntityFramework NuGet paketinin eski bir sürümünü kullanıyorsanız (4 ana sürümü olan bir sürüm) **EF 4.x DbContext Generator** şablonu kullanmanız gerekir. Bu, şablonu eklerken **Çevrimiçi** sekmesinde bulunabilir veya şablonu doğrudan Visual Studio Gallery'den önceden yükleyebilirsiniz.

- [C için EF 4.x DbContext Jeneratör #](https://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [C# Web Siteleri için EF 4.x DbContext Jeneratör](https://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [VB.NET için EF 4.x DbContext Jeneratör](https://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [VB.NET Web Siteleri için EF 4.x DbContext Jeneratör](https://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>EntityObject Jeneratör

Bu şablon, EntityObject'ten türeyen varlık sınıfları ve ObjectContext'dan türeyen bir bağlam oluşturur.

> [!NOTE]
> DbContext Jeneratör'ü kullanmayı düşünün

DbContext Generator şimdi yeni uygulamalar için önerilen şablondur. DbContext Generator basit DbContext API yararlanır. EntityObject Generator varolan uygulamaları desteklemek için kullanılabilir olmaya devam ediyor.

**Visual Studio 2010, 2012 &amp; 2013**

Visual Studio Gallery'den indirmek için şablonu eklerken **Çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak şablonu doğrudan Visual Studio Gallery'den önceden yükleyebilirsiniz.

- [C için EF 6.x EntityObject Generator #](https://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [C# Web Siteleri için EF 6.x EntityObject Generator](https://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [EF 6.x EntityObject Generator for VB.NET](https://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [VB.NET Web Siteleri için EF 6.x EntityObject Generator](https://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**EF 5.x için EntityObject Generator**


Visual Studio 2012 veya 2013 kullanıyorsanız, Visual Studio Gallery'den indirmek için şablonu eklerken **Çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak şablonu doğrudan Visual Studio Gallery'den önceden yükleyebilirsiniz. Şablonlar Visual Studio 2010'a dahil olduğundan galerideki sürümler yalnızca Visual Studio 2012 &amp; 2013'e yüklenebilir.

- [C için EF 5.x EntityObject Generator #](https://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [C# Web Siteleri için EF 5.x EntityObject Generator](https://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [EF 5.x EntityObject Generator for VB.NET](https://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [VB.NET Web Siteleri için EF 5.x EntityObject Generator](https://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Şablonu yönetmeye gerek kalmadan ObjectContext kod oluşturma yı istiyorsanız [EntityObject kod oluşturma'ya geri dönebilirsiniz.](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)

Visual Studio 2010 kullanıyorsanız bu şablon zaten yüklenmiş. Visual Studio 2010'da yeni bir model oluşturursanız, bu şablon varsayılan olarak kullanılır, ancak .tt dosyaları projenize dahil edilmez. Şablonu özelleştirmek istiyorsanız, şablonu projenize eklemeniz gerekir.

### <a name="self-tracking-entities-ste-generator"></a>Kendinden Takip Eden Varlıklar (STE) Jeneratör

Bu şablon, Öz İzleme Varlık sınıfları ve ObjectContext türetilen bir bağlam oluşturur. Bir EF uygulamasında, varlıklardaki değişiklikleri izlemekten bir bağlam sorumludur. Ancak, N-Tier senaryolarında bağlam, varlıkları değiştiren katmanda kullanılamayabilir. Kendi kendini izleyen varlıklar, herhangi bir katmandaki değişiklikleri izlemenize yardımcı olur. Daha fazla bilgi için, [Kendi Kendini İzleme Varlıkları'na](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md)bakın.

> [!NOTE]
> STE Şablonu Önerilmez

Artık yeni uygulamalarda STE şablonu kullanmanızı önermiyoruz, varolan uygulamaları desteklemek için kullanılabilir olmaya devam ediyor. N-Tier senaryoları için önerdiğimiz diğer seçenekler için [bağlantısı kesilen varlıklar makalesini](~/ef6/fundamentals/disconnected-entities/index.md) ziyaret edin.

> [!NOTE]
> STE şablonunun EF 6.x sürümü yoktur.


> [!NOTE]
> STE şablonunun Visual Studio 2013 sürümü yoktur.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Visual Studio 2012 kullanıyorsanız, Visual Studio Gallery'den indirmek için şablonu eklerken **Çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak şablonu doğrudan Visual Studio Gallery'den önceden yükleyebilirsiniz. Şablonlar Visual Studio 2010'a dahil olduğundan galerideki sürümler yalnızca Visual Studio 2012'ye yüklenebilir.

- [C için EF 5.x STE Jeneratör #](https://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [C# Web Siteleri için EF 5.x STE Jeneratör](https://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [VB.NET için EF 5.x STE Jeneratör](https://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [VB.NET Web Siteleri için EF 5.x STE Jeneratör](https://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Görsel Stüdyo 2010**

Visual Studio 2010 kullanıyorsanız bu şablon zaten yüklenmiş.

### <a name="poco-entity-generator"></a>POCO Varlık Jeneratör

Bu şablon, POCO varlık sınıfları ve ObjectContext'dan türeyen bir bağlam oluşturur

> [!NOTE]
> DbContext Jeneratör'ü kullanmayı düşünün

DbContext Generator şimdi yeni uygulamalarda POCO sınıfları oluşturmak için önerilen şablondur. DbContext Generator yeni DbContext API yararlanır ve basit POCO sınıfları üretebilir. POCO Entity Generator mevcut uygulamaları desteklemek için kullanılabilir olmaya devam etmektedir.

> [!NOTE]
> STE şablonunun EF 5.x veya EF 6.x sürümü yoktur.

> [!NOTE]
> POCO şablonunun Visual Studio 2013 sürümü yoktur.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

Visual Studio Gallery'den indirmek için şablonu eklerken **Çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak şablonu doğrudan Visual Studio Gallery'den önceden yükleyebilirsiniz.

- [C için EF 4.x POCO Jeneratör #](https://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [C# Web Siteleri için EF 4.x POCO Jeneratör](https://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [VB.NET için EF 4.x POCO Jeneratör](https://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [VB.NET Web Siteleri için EF 4.x POCO Jeneratör](https://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>"Web Siteleri" Şablonları nelerdir

"Web Siteleri" şablonları (örneğin, **C\# Web Siteleri için EF 5.x DbContext Generator)** Dosya üzerinden oluşturulan Web Sitesi projelerinde kullanılmak üzere **- Yeni -&gt; &gt; Web Sitesi...**. Bunlar, standart şablonları kullanan **Dosya&gt; -&gt; Yeni - Project...** aracılığıyla oluşturulan Web Uygulamalarından farklıdır. Visual Studio'daki öğe şablonu sistemi bunları gerektirdiğinden ayrı şablonlar sağlarız.

## <a name="using-a-template"></a>Şablon Kullanma

Kod oluşturma şablonu kullanmaya başlamak için, EF Designer'da tasarım yüzeyinde boş bir noktaya sağ tıklayın ve **Kod Oluşturma Öğesi Ekle'yi seçin...** seçeneğini belirleyin.

![Kod Gen Öğesi Ekle](~/ef6/media/add-code-gen-item.png)

Kullanmak istediğiniz şablonu zaten yüklediyseniz (veya Visual Studio'ya dahil edildi), şablon sol menüden **Kod** veya **Veri** bölümü altında kullanılabilir.

![Yüklendi](~/ef6/media/installed.png)

Şablon zaten yüklü değilseniz, sol menüden **Çevrimiçi'yi** seçin ve istediğiniz şablonu arayın.

![Arama](~/ef6/media/search.png) 

Visual Studio 2012 kullanıyorsanız, yeni .tt dosyaları .edmx dosyasının altına yer alır.*

> [!NOTE]
> Visual Studio 2012'de oluşturulan modelleriçin varsayılan kod oluşturma için kullanılan şablonları silmeniz gerekir, aksi takdirde yinelenen sınıflar ve bağlam oluşturulur. Varsayılan dosyalar ** &lt;model&gt;adı .tt** ve ** &lt;model adı&gt;.context.tt.** 

![VS2012 Şablonları](~/ef6/media/vs2012-templates.png)

Visual Studio 2010 kullanıyorsanız, tt dosyaları doğrudan projenize eklenir.  

![VS2010 Şablonları](~/ef6/media/vs2010-templates.png)
