---
title: Tasarımcı kodu oluşturma şablonları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 56e00fa2-f9f0-48b3-8006-f8266ca7e74b
ms.openlocfilehash: e4e99a86e7c273682c85eba06042af9a2a837d12
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418678"
---
# <a name="designer-code-generation-templates"></a>Tasarımcı kodu oluşturma şablonları
Entity Framework Designer kullanarak bir model oluşturduğunuzda, sınıflarınız ve türetilmiş bağlam sizin için otomatik olarak oluşturulur. Varsayılan kod oluşturmaya ek olarak, oluşturulan kodu özelleştirmek için kullanılabilecek bir dizi şablon da sunuyoruz. Bu şablonlar, T4 Metin şablonları olarak sağlanır ve gerekirse şablonları özelleştirmenize olanak tanır.

Varsayılan olarak oluşturulan kod, modelinizi oluşturduğunuz Visual Studio sürümüne bağlıdır:

-   Visual Studio 2012 & 2013 ' de oluşturulan modeller basit POCO varlık sınıfları ve Basitleştirilmiş DbContext 'ten türetilen bir bağlam oluşturacak.
-   Visual Studio 2010 ' de oluşturulan modeller, EntityObject öğesinden türeten ve ObjectContext 'ten türetilen bir içerikten türetilmiş varlık sınıfları oluşturur.
  > [!NOTE]    
  > Modelinize eklendikten sonra DbContext Oluşturucu şablonuna geçiş yapmanızı öneririz.

Bu sayfa, kullanılabilir şablonları kapsamakta ve modelinize şablon eklemeye ilişkin yönergeler sağlar.

## <a name="available-templates"></a>Kullanılabilir şablonlar

Aşağıdaki şablonlar Entity Framework ekibi tarafından sağlanır:

### <a name="dbcontext-generator"></a>DbContext Oluşturucu

Bu şablon basit POCO varlık sınıfları ve EF6 kullanarak DbContext 'ten türeten bir bağlam oluşturacak.
Aşağıda listelenen diğer şablonlardan birini kullanma nedeniniz yoksa, bu önerilen şablondur.
Ayrıca, Visual Studio 'nun son sürümlerini (Visual Studio 2013 onda) kullanıyorsanız, varsayılan olarak aldığınız kod oluşturma şablonudur: yeni bir model oluşturduğunuzda, bu şablon varsayılan olarak kullanılır ve T4 dosyaları (. tt). edmx dosyanızın altına eklenir.

#### <a name="older-versions-of-visual-studio"></a>Visual Studio 'nun eski sürümleri
- **Visual Studio 2012:** **EF 6. x DbContextGenerator** şablonlarını almak Için, **Visual Studio için** en son Entity Framework Tools yüklemeniz gerekir-daha fazla bilgi için [Get Entity Framework](~/ef6/fundamentals/install.md) sayfasına bakın.
- **Visual Studio 2010:** **EF 6. x DbContextGenerator** şablonları, Visual Studio 2010 için kullanılamaz.

#### <a name="dbcontext-generator-for-ef-5x"></a>EF 5. x için DbContext Oluşturucu

EntityFramework NuGet paketinin eski bir sürümünü kullanıyorsanız (biri 5 ' in ana sürümü ile), **EF 5. x DbContext Generator** şablonunu kullanmanız gerekir.

Visual Studio 2013 veya 2012 kullanıyorsanız bu şablon zaten yüklüdür.

Visual Studio 2010 kullanıyorsanız, Visual Studio galerisinden indirmek üzere şablonu eklerken **çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak, şablonu doğrudan Visual Studio Galerisi 'nden daha önce yükleyebilirsiniz. Şablonlar, Visual Studio 'nun sonraki sürümlerinde yer aldığı için galerideki sürümler yalnızca Visual Studio 2010 ' ye yüklenebilir.

- [İçin EF 5. x DbContext GeneratorC#](https://visualstudiogallery.msdn.microsoft.com/da740968-02f9-42a9-9ee4-1a9a06d896a2)
- [Web siteleri için C# EF 5. x DbContext Generator](https://visualstudiogallery.msdn.microsoft.com/5d01a981-91b8-492c-b42c-c771c3f31e03)
- [VB.NET için EF 5. x DbContext Generator](https://visualstudiogallery.msdn.microsoft.com/875c882d-333e-455a-8dae-5353510527dd?src=featured)
- [VB.NET Web siteleri için EF 5. x DbContext Generator](https://visualstudiogallery.msdn.microsoft.com/d4d7d4cd-c2d0-43e6-8944-12f6ff8f2614)

#### <a name="dbcontext-generator-for-ef-4x"></a>EF 4. x için DbContext Oluşturucu

EntityFramework NuGet paketinin eski bir sürümünü kullanıyorsanız (biri 4 ' ün ana sürümü ile), **EF 4. x DbContext Generator** şablonunu kullanmanız gerekir. Bu, şablon eklenirken **çevrimiçi** sekmede bulunabilir veya şablonu doğrudan Visual Studio galerisinden yükleyebilirsiniz.

- [İçin EF 4. x DbContext OluşturucuC#](https://visualstudiogallery.msdn.microsoft.com/7812b04c-db36-4817-8a84-e73c452410a2)
- [Web siteleri için C# EF 4. x DbContext Oluşturucu](https://visualstudiogallery.msdn.microsoft.com/de0e9bc6-e86a-4448-8a2e-a1260a53203e)
- [VB.NET için EF 4. x DbContext Generator](https://visualstudiogallery.msdn.microsoft.com/73679ae5-e358-4e76-a538-c7b5e04ac073)
- [VB.NET Web siteleri için EF 4. x DbContext Generator](https://visualstudiogallery.msdn.microsoft.com/86f5a660-306e-4831-840c-2e4ee7474a92)

### <a name="entityobject-generator"></a>EntityObject üreticisi

Bu şablon, EntityObject öğesinden türetilen varlık sınıfları ve ObjectContext 'ten türetilen bir bağlam oluşturur.

> [!NOTE]
> DbContext oluşturucuyu kullanmayı düşünün

DbContext Oluşturucu artık yeni uygulamalar için önerilen şablondur. DbContext Oluşturucu, daha basit DbContext API özelliğinden yararlanır. EntityObject üreticisi mevcut uygulamaları desteklemek için kullanılabilir olmaya devam eder.

**Visual Studio 2010, 2012 &amp; 2013**

Visual Studio galerisinden indirmek üzere şablonu eklerken **çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak, şablonu doğrudan Visual Studio Galerisi 'nden daha önce yükleyebilirsiniz.

- [İçin EF 6. x EntityObject OluşturucusuC#](https://visualstudiogallery.msdn.microsoft.com/66612113-549c-4a9e-a14a-f629ceb3f89a)
- [Web siteleri için C# EF 6. x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/076140f3-6dbe-451f-a0e0-16b6d2bd8996)
- [VB.NET için EF 6. x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/ff479d55-2c85-43c5-a4d6-21cd659435ea)
- [VB.NET Web siteleri için EF 6. x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/668e2b92-c142-4da2-8e60-866c6346fc6a)

**EF 5. x için EntityObject üreticisi**


Visual Studio 2012 veya 2013 kullanıyorsanız, Visual Studio galerisinden indirmek üzere şablonu eklerken **çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak, şablonu doğrudan Visual Studio Galerisi 'nden daha önce yükleyebilirsiniz. Şablonlar Visual Studio 2010 ' ye eklendiğinden, galerideki sürümler yalnızca Visual Studio 2012 &amp; 2013 ' ye yüklenebilir.

- [İçin EF 5. x EntityObject OluşturucusuC#](https://visualstudiogallery.msdn.microsoft.com/1da40393-b5ec-404a-a000-6a7e6e911339)
- [Web siteleri için C# EF 5. x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/94b48556-fcf0-4b9b-8615-20f9066ae9ac)
- [VB.NET için EF 5. x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/92c0129e-40dc-488c-a836-7e30846dfb30)
- [VB.NET Web siteleri için EF 5. x EntityObject Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/5dd7f75c-8c98-4eb7-b4bc-06f0d0b03b41)

Yalnızca bir şablonu düzenlemeye gerek kalmadan ObjectContext kod üretimini istiyorsanız [EntityObject kod oluşturmaya döndürebilirsiniz](~/ef6/modeling/designer/codegen/legacy-objectcontext.md).

Visual Studio 2010 kullanıyorsanız bu şablon zaten yüklüdür. Visual Studio 2010 'de yeni bir model oluşturursanız bu şablon varsayılan olarak kullanılır ancak. tt dosyaları projenize dahil edilmez. Şablonu özelleştirmek istiyorsanız projenize eklemeniz gerekir.

### <a name="self-tracking-entities-ste-generator"></a>Kendi kendine Izleme varlıkları (STE) Oluşturucusu

Bu şablon, kendini Izleyen varlık sınıfları ve ObjectContext 'ten türetilen bir bağlam oluşturacak. EF uygulamasında, varlıklarda yapılan değişiklikleri izlemekten bir bağlam sorumludur. Ancak, N katmanlı senaryolarda bağlam, varlıkları değiştiren katmanda bulunmayabilir. Kendi kendine izleme varlıkları herhangi bir katmandaki değişiklikleri izlemenize yardımcı olur. Daha fazla bilgi için bkz. [kendi kendine Izleme varlıkları](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/index.md).

> [!NOTE]
> STE şablonu önerilmez

Artık yeni uygulamalarda STE şablonunu kullanmayı önermeyiz, var olan uygulamaları desteklemeye devam ediyor. N katmanlı senaryolar için önerdiğimiz diğer seçeneklere yönelik [bağlı olmayan varlıklar makalesini](~/ef6/fundamentals/disconnected-entities/index.md) ziyaret edin.

> [!NOTE]
> STE şablonunun EF 6. x sürümü yoktur.


> [!NOTE]
> STE şablonunun Visual Studio 2013 sürümü yok.

### <a name="visual-studio-2012"></a>Visual Studio 2012

Visual Studio 2012 kullanıyorsanız, Visual Studio galerisinden indirmek üzere şablonu eklerken **çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak, şablonu doğrudan Visual Studio Galerisi 'nden daha önce yükleyebilirsiniz. Şablonlar Visual Studio 2010 'ye eklendiğinden, galerideki sürümler yalnızca Visual Studio 2012 ' ye yüklenebilir.

- [İçin EF 5. x STE OluşturucusuC#](https://visualstudiogallery.msdn.microsoft.com/a3ac10a5-9365-4096-bb58-d9a1ba71db8f)
- [Web siteleri için C# EF 5. x Ste Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/1b55ab82-eeb4-47ba-8d35-3c7c8b5f5a8c)
- [VB.NET için EF 5. x STE Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/1ba8c6a3-44e9-4e1f-b21e-596f3168474b)
- [VB.NET Web siteleri için EF 5. x STE Oluşturucusu](https://visualstudiogallery.msdn.microsoft.com/a9fd5f0a-9af4-4e32-9c09-0e057072152e)

#### <a name="visual-studio-2010"></a>Visual Studio 2010 * *

Visual Studio 2010 kullanıyorsanız bu şablon zaten yüklüdür.

### <a name="poco-entity-generator"></a>POCO varlık Oluşturucu

Bu şablon POCO varlık sınıfları ve ObjectContext 'ten türetilen bir bağlam oluşturacak

> [!NOTE]
> DbContext oluşturucuyu kullanmayı düşünün

DbContext Oluşturucu artık yeni uygulamalarda POCO sınıfları oluşturmak için önerilen şablondur. DbContext Oluşturucu yeni DbContext API özelliğinden yararlanır ve daha basit POCO sınıfları oluşturabilir. POCO varlık Oluşturucu mevcut uygulamaları desteklemek için kullanılabilir olmaya devam eder.

> [!NOTE]
> STE şablonunun EF 5. x veya EF 6. x sürümü yoktur.

> [!NOTE]
> POCO şablonunun Visual Studio 2013 sürümü yok.

#### <a name="visual-studio-2012-amp-visual-studio-2010"></a>Visual Studio 2012 &amp; Visual Studio 2010

Visual Studio galerisinden indirmek üzere şablonu eklerken **çevrimiçi** sekmesini seçmeniz gerekir. Alternatif olarak, şablonu doğrudan Visual Studio Galerisi 'nden daha önce yükleyebilirsiniz.

- [EF 4. x POCO OluşturucuC#](https://visualstudiogallery.msdn.microsoft.com/23df0450-5677-4926-96cc-173d02752313)
- [Web siteleri için C# EF 4. x poco Oluşturucu](https://visualstudiogallery.msdn.microsoft.com/fe568da5-aa1a-4178-a2a5-48813c707a7f)
- [VB.NET için EF 4. x POCO Oluşturucu](https://visualstudiogallery.msdn.microsoft.com/53ecbded-8936-4299-ab04-1e44e5489752)
- [VB.NET Web siteleri için EF 4. x POCO Oluşturucu](https://visualstudiogallery.msdn.microsoft.com/463c5aca-05ad-4cdb-910b-2e4f83269e34)

### <a name="what-are-the-web-sites-templates"></a>"Web siteleri" şablonları nelerdir?

"Web siteleri" şablonları (örneğin, **C\# Web siteleri Için EF 5. x DbContext Generator**), **File-&gt; New-&gt; Web sitesi**aracılığıyla oluşturulan Web sitesi projelerinde kullanım içindir.... Bunlar, standart şablonları kullanan **dosya&gt; New-&gt; Project..** . aracılığıyla oluşturulan Web uygulamalarından farklıdır. Visual Studio 'daki öğe şablonu sistemi için gerekli olan ayrı şablonlar sunuyoruz.

## <a name="using-a-template"></a>Şablon kullanma

Kod oluşturma şablonunu kullanmaya başlamak için, EF Designer 'daki tasarım yüzeyinde boş bir noktaya sağ tıklayın ve **kod oluşturma öğesi Ekle...** seçeneğini belirleyin.

![Kod genel öğesi Ekle](~/ef6/media/add-code-gen-item.png)

Kullanmak istediğiniz şablonu (veya Visual Studio 'ya dahil) zaten yüklediyseniz, sol taraftaki menüdeki **kod** veya **veri** bölümünde kullanılabilir olacaktır.

![Yüklendi](~/ef6/media/installed.png)

Şablonunuz zaten yüklü değilse, sol taraftaki menüden **çevrimiçi** ' i seçin ve istediğiniz şablonu arayın.

![Arama](~/ef6/media/search.png) 

Visual Studio 2012 kullanıyorsanız, yeni. tt dosyaları. edmx dosyası altında iç içe alınacaktır. *

> [!NOTE]
> Visual Studio 2012 ' de oluşturulan modeller için, varsayılan kod üretimi için kullanılan şablonları silmeniz gerekir, aksi halde yinelenen sınıflar ve bağlam oluşturulur. Varsayılan dosyalar **&lt;model adı&gt;. tt** ve **&lt;model adı&gt;. Context.tt**. 

![VS2012 şablonları](~/ef6/media/vs2012-templates.png)

Visual Studio 2010 kullanıyorsanız, TT dosyaları projenize doğrudan eklenir.  

![VS2010 şablonları](~/ef6/media/vs2010-templates.png)
