---
title: İlişkiler-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655670"
---
# <a name="relationships"></a>İlişkiler

Bir ilişki, iki varlığın birbirleriyle ilişkisini tanımlar. İlişkisel bir veritabanında bu, yabancı anahtar kısıtlaması tarafından temsil edilir.

> [!NOTE]  
> Bu makaledeki örneklerin çoğu, kavramları göstermek için bire çok ilişki kullanır. Bire bir ve çoktan çoğa ilişkilerin örnekleri için makalenin sonundaki [diğer Ilişki desenleri](#other-relationship-patterns) bölümüne bakın.

## <a name="definition-of-terms"></a>Koşulların tanımı

İlişkileri anlatmak için kullanılan bazı terimler vardır

* **Bağımlı varlık:** Bu, yabancı anahtar özelliğini içeren varlıktır. Bazen ilişkinin ' Child ' olarak da adlandırılır.

* **Asıl varlık:** Bu, birincil/alternatif anahtar Özellik (ler) i içeren varlıktır. Bazen ilişkinin ' Parent ' olarak adlandırılır.

* **Yabancı anahtar:** Bağımlı varlıktaki, varlığın ilişkili olduğu asıl anahtar özelliğinin değerlerini depolamak için kullanılan Özellik (ler).

* **Asıl anahtar:** Asıl varlığı benzersiz bir şekilde tanımlayan Özellik (ler). Bu, birincil anahtar veya alternatif bir anahtar olabilir.

* **Gezinti özelliği:** Sorumlu ve/veya bağımlı varlık üzerinde tanımlanan, ilişkili varlıklara başvuru içeren bir özellik.

  * **Koleksiyon gezinti özelliği:** Birçok ilgili varlığa başvurular içeren bir gezinti özelliği.

  * **Başvuru gezinti özelliği:** Tek bir ilgili varlığa başvuruyu tutan bir gezinti özelliği.

  * **Ters gezinme özelliği:** Belirli bir gezinti özelliği tartışırken, bu terim ilişkinin diğer ucundaki gezinti özelliğine başvurur.

Aşağıdaki kod listesi `Blog` ve `Post` arasında bir-çok ilişkisi gösterir

* bağımlı varlık `Post`

* Asıl varlık `Blog`

* yabancı anahtar `Post.BlogId`

* `Blog.BlogId` asıl anahtardır (Bu durumda, alternatif bir anahtar yerine birincil anahtardır)

* `Post.Blog` bir başvuru gezinti özelliğidir

* bir koleksiyon gezinti özelliği `Blog.Posts`

* `Post.Blog`, `Blog.Posts` ters gezinti özelliğidir (ve tam tersi)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Kurallar

Kurala göre, bir tür üzerinde bulunan bir gezinti özelliği olduğunda bir ilişki oluşturulur. Bir özellik, işaret ettiği türün geçerli veritabanı sağlayıcısı tarafından skalar bir tür olarak eşleştirilemez olması halinde bir gezinti özelliği olarak değerlendirilir.

> [!NOTE]  
> Kural tarafından keşfedilen ilişkiler her zaman asıl varlığın birincil anahtarını hedefler. Alternatif bir anahtarı hedeflemek için, akıcı API kullanılarak ek yapılandırma gerçekleştirilmesi gerekir.

### <a name="fully-defined-relationships"></a>Tam olarak tanımlanmış Ilişkiler

İlişkiler için en yaygın model, ilişkinin her iki ucunda ve bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği için tanımlanmış gezinti özelliklerine sahip değildir.

* İki tür arasında bir dizi gezinti özelliği bulunursa, bu, aynı ilişkinin ters gezinme özellikleri olarak yapılandırılır.

* Bağımlı varlık `<primary key property name>`, `<navigation property name><primary key property name>`veya `<principal entity name><primary key property name>` adlı bir özellik içeriyorsa, yabancı anahtar olarak yapılandırılır.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> İki tür arasında tanımlanmış birden çok gezinti özelliği varsa (diğer bir deyişle, birbirine işaret eden birden çok farklı gezginler çifti), kurala göre hiçbir ilişki oluşturulmaz ve Gezinti özellikleri eşleştirin.

### <a name="no-foreign-key-property"></a>Yabancı anahtar özelliği yok

Bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği olması önerilse de, bu gerekli değildir. Yabancı anahtar özelliği bulunmazsa, `<navigation property name><principal key property name>` adıyla bir gölge yabancı anahtar özelliği tanıtılacaktır (daha fazla bilgi için bkz. [Gölge özelliklerine](shadow-properties.md) bakın).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Tek gezinti özelliği

Tek bir gezinti özelliği (ters gezinme olmadan ve yabancı anahtar özelliği olmadan) dahil olmak üzere, kural tarafından tanımlanan bir ilişkiye sahip olmak için yeterli değildir. Tek bir gezinti özelliğine ve yabancı anahtar özelliğine de sahip olabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Basamaklı Silme

Kurala göre, Cascade DELETE, isteğe bağlı ilişkiler için gerekli ilişkiler ve *Clientsetnull* için *Cascade* olarak ayarlanacak. *Cascade* , bağımlı varlıkların de silindiği anlamına gelir. *Clientsetnull* , belleğe yüklenmeyen bağımlı varlıkların değişmeden kalacağından, el ile silinmesi ya da geçerli bir sorumlu varlığına işaret edecek şekilde güncelleştirilmeleri anlamına gelir. Belleğe yüklenen varlıklar için EF Core yabancı anahtar özelliklerini null olarak ayarlamaya çalışacaktır.

Gerekli ve isteğe bağlı ilişkiler arasındaki fark için [gerekli ve Isteğe bağlı ilişkiler](#required-and-optional-relationships) bölümüne bakın.

Farklı silme davranışları ve kural tarafından kullanılan varsayılanlar hakkında daha fazla ayrıntı için bkz. [Cascade Delete](../saving/cascade-delete.md) .

## <a name="data-annotations"></a>Veri Açıklamaları

`[ForeignKey]` ve `[InverseProperty]`ilişkilerini yapılandırmak için kullanılabilecek iki veri ek açıklaması vardır. Bunlar `System.ComponentModel.DataAnnotations.Schema` ad alanında kullanılabilir.

### <a name="foreignkey"></a>[ForeignKey]

Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için veri açıklamalarını kullanabilirsiniz. Bu, genellikle yabancı anahtar özelliği kural tarafından keşfedildiğinde yapılır.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> `[ForeignKey]` ek açıklaması, ilişkide herhangi bir gezinti özelliğine eklenebilir. Bağımlı varlık sınıfında gezinti özelliğine gitmesinin gerekli değildir.

### <a name="inverseproperty"></a>[Evirseproperty]

Bağımlı ve birincil varlıklarda gezinti özelliklerinin nasıl kullandığını yapılandırmak için veri açıklamalarını kullanabilirsiniz. Bu, genellikle iki varlık türü arasında birden fazla gezinti özelliği olduğunda yapılır.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>Akıcı API

Akıcı API 'de bir ilişki yapılandırmak için, ilişkiyi oluşturan gezinti özelliklerini tanımlayarak başlacaksınız. `HasOne` veya `HasMany`, üzerinde yapılandırmaya başmakta olduğunuz varlık türünde gezinti özelliğini tanımlar. Daha sonra ters gezintiyi belirlemek için `WithOne` veya `WithMany` bir çağrı zincirleyebilirsiniz. `HasOne`/`WithOne` başvuru gezinti özellikleri için kullanılır ve koleksiyon gezinti özellikleri için `HasMany`/`WithMany` kullanılır.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Tek gezinti özelliği

Yalnızca bir gezinti özelliğine sahipseniz, `WithOne` ve `WithMany`Parametresiz aşırı yüklemeleri vardır. Bu, ilişkinin diğer ucunda kavramsal olarak bir başvuru veya koleksiyon olduğunu gösterir, ancak varlık sınıfında hiçbir gezinti özelliği dahil değildir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Yabancı anahtar

Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için Floent API 'yi kullanabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

Aşağıdaki kod listesi, bileşik yabancı anahtarın nasıl yapılandırılacağını gösterir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

Bir gölge özelliği yabancı anahtar olarak yapılandırmak için `HasForeignKey(...)` dize aşırı yüklemesini kullanabilirsiniz (daha fazla bilgi için bkz. [gölge özellikleri](shadow-properties.md) ). Gölge özelliğini, yabancı anahtar olarak kullanmadan önce modele açıkça eklememiz önerilir (aşağıda gösterildiği gibi).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Gezinti özelliği olmadan

Bir gezinti özelliği sağlamanız gerekmez. İlişkinin yalnızca bir tarafında yabancı anahtar sağlayabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Asıl anahtar

Yabancı anahtarın birincil anahtar dışında bir özelliğe başvurmasına isterseniz, ilişkinin asıl anahtar özelliğini yapılandırmak için Floent API 'sini kullanabilirsiniz. Asıl anahtar olarak yapılandırdığınız özelliği otomatik olarak alternatif bir anahtar olarak ayarlar (daha fazla bilgi için bkz. [Alternatif anahtarlar](alternate-keys.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

Aşağıdaki kod listesinde bir bileşik asıl anahtarın nasıl yapılandırılacağı gösterilmektedir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> Asıl anahtar özelliklerini belirttiğiniz sıra, yabancı anahtar için belirtildikleri sırayla eşleşmelidir.

### <a name="required-and-optional-relationships"></a>Gerekli ve Isteğe bağlı Ilişkiler

İlişkinin gerekli veya isteğe bağlı olup olmadığını yapılandırmak için Floent API 'sini kullanabilirsiniz. Sonuç olarak bu, yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığını denetler. Bu, büyük bir gölge durumu yabancı anahtar kullanırken faydalıdır. Varlık sınıfınızda yabancı anahtar özelliği varsa, ilişkinin gereklik durumu yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığına göre belirlenir (daha fazla bilgi için bkz. [gerekli ve Isteğe bağlı özellikler](required-optional.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a>Basamaklı Silme

Belirli bir ilişki için basamaklı silme davranışını açıkça yapılandırmak için Floent API 'sini kullanabilirsiniz.

Her seçeneğe ilişkin ayrıntılı bir tartışma için verileri kaydetme bölümündeki [basamaklı silme](../saving/cascade-delete.md) bölümüne bakın.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a>Diğer Ilişki desenleri

### <a name="one-to-one"></a>Bire bir

Bir ilişkinin her iki tarafında da başvuru gezintisi özelliği vardır. Tek-çok ilişkilerle aynı kurallara uyar, ancak her sorumluyla yalnızca bir bağımlı ilişki olduğundan emin olmak için yabancı anahtar özelliğinde benzersiz bir dizin sunulmuştur.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> EF, bir yabancı anahtar özelliği algılama özelliğine göre bağımlı olacak varlıklardan birini seçer. Bağımlı olarak yanlış varlık seçilirse, bunu düzeltmek için akıcı API 'yi kullanabilirsiniz.

Akıcı API ile ilişkiyi yapılandırırken `HasOne` ve `WithOne` yöntemlerini kullanırsınız.

Yabancı anahtarı yapılandırırken, bağımlı varlık türünü belirtmeniz gerekir-aşağıdaki listede `HasForeignKey` için belirtilen genel parametreyi görürsünüz. Bire çok ilişkisinde, başvuru gezinmesi olan varlığın bağımlı olduğunu ve koleksiyonun asıl öğe olduğunu unutmayın. Ancak bu, bire bir ilişkide değildir, bu nedenle açıkça tanımlanması gerekir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>Çoktan çoğa

JOIN tablosunu temsil eden bir varlık sınıfı olmayan çoktan çoğa ilişkiler henüz desteklenmemektedir. Bununla birlikte, JOIN tablosuna bir varlık sınıfı ekleyerek ve iki ayrı bire çok ilişkiyi eşleyerek çoktan çoğa ilişkiyi temsil edebilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
