---
title: İlişkiler-EF Core
description: Entity Framework Core kullanırken varlık türleri arasında ilişkiler yapılandırma
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 6d68e813cec6c989e8e4cb848f8740489645c65c
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77051413"
---
# <a name="relationships"></a>İlişkiler

Bir ilişki, iki varlığın birbirleriyle ilişkisini tanımlar. İlişkisel bir veritabanında bu, yabancı anahtar kısıtlaması tarafından temsil edilir.

> [!NOTE]  
> Bu makaledeki örneklerin çoğu, kavramları göstermek için bire çok ilişki kullanır. Bire bir ve çoktan çoğa ilişkilerin örnekleri için makalenin sonundaki [diğer Ilişki desenleri](#other-relationship-patterns) bölümüne bakın.

## <a name="definition-of-terms"></a>Koşulların tanımı

İlişkileri anlatmak için kullanılan bazı terimler vardır

* **Bağımlı varlık:** Bu, yabancı anahtar özelliklerini içeren varlıktır. Bazen ilişkinin ' Child ' olarak da adlandırılır.

* **Asıl varlık:** Bu, birincil/alternatif anahtar özelliklerini içeren varlıktır. Bazen ilişkinin ' Parent ' olarak adlandırılır.

* **Asıl anahtar:** Asıl varlığı benzersiz bir şekilde tanımlayan Özellikler. Bu, birincil anahtar veya alternatif bir anahtar olabilir.

* **Yabancı anahtar:** İlgili varlık için asıl anahtar değerlerini depolamak için kullanılan bağımlı varlıktaki Özellikler.

* **Gezinti özelliği:** Sorumlu ve/veya bağımlı varlık üzerinde tanımlanan, ilgili varlığa başvuran bir özellik.

  * **Koleksiyon gezinti özelliği:** Birçok ilgili varlığa başvurular içeren bir gezinti özelliği.

  * **Başvuru gezinti özelliği:** Tek bir ilgili varlığa başvuruyu tutan bir gezinti özelliği.

  * **Ters gezinme özelliği:** Belirli bir gezinti özelliği tartışırken, bu terim ilişkinin diğer ucundaki gezinti özelliğine başvurur.
  
* **Kendine başvuran ilişki:** Bağımlı ve asıl varlık türlerinin aynı olduğu bir ilişki.

Aşağıdaki kod, `Blog` ve `Post` arasında bir-çok ilişkisi gösterir

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Full)]

* bağımlı varlık `Post`

* Asıl varlık `Blog`

* `Blog.BlogId` asıl anahtardır (Bu durumda, alternatif bir anahtar yerine birincil anahtardır)

* yabancı anahtar `Post.BlogId`

* `Post.Blog` bir başvuru gezinti özelliğidir

* bir koleksiyon gezinti özelliği `Blog.Posts`

* `Post.Blog`, `Blog.Posts` ters gezinti özelliğidir (ve tam tersi)

## <a name="conventions"></a>Kurallar

Varsayılan olarak, bir tür üzerinde bulunan bir gezinti özelliği olduğunda bir ilişki oluşturulur. Bir özellik, işaret ettiği türün geçerli veritabanı sağlayıcısı tarafından skalar bir tür olarak eşleştirilemez olması halinde bir gezinti özelliği olarak değerlendirilir.

> [!NOTE]  
> Kural tarafından keşfedilen ilişkiler her zaman asıl varlığın birincil anahtarını hedefler. Alternatif bir anahtarı hedeflemek için, akıcı API kullanılarak ek yapılandırma gerçekleştirilmesi gerekir.

### <a name="fully-defined-relationships"></a>Tam olarak tanımlanmış ilişkiler

İlişkiler için en yaygın model, ilişkinin her iki ucunda ve bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği için tanımlanmış gezinti özelliklerine sahip değildir.

* İki tür arasında bir dizi gezinti özelliği bulunursa, bu, aynı ilişkinin ters gezinme özellikleri olarak yapılandırılır.

* Bağımlı varlık, bu desenlerden biriyle eşleşen bir ada sahip bir özellik içeriyorsa, yabancı anahtar olarak yapılandırılır:
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Full&highlight=6,15-16)]

Bu örnekte, ilişkiyi yapılandırmak için vurgulanan Özellikler kullanılacaktır.

> [!NOTE]
> Özellik birincil anahtar ise veya asıl anahtarla uyumlu olmayan bir türde ise, yabancı anahtar olarak yapılandırılmaz.

> [!NOTE]
> EF Core 3,0 önce, asıl anahtar özelliği ile tam olarak aynı adlı özellik [yabancı anahtar olarak da eşleştirildiği](https://github.com/aspnet/EntityFrameworkCore/issues/13274) için

### <a name="no-foreign-key-property"></a>Yabancı anahtar özelliği yok

Bağımlı varlık sınıfında tanımlanmış yabancı anahtar özelliği olması önerilse de, bu gerekli değildir. Yabancı anahtar özelliği bulunmazsa, bağımlı tür üzerinde hiçbir gezinti yoksa, `<navigation property name><principal key property name>` veya `<principal entity name><principal key property name>` bir [gölge yabancı anahtar özelliği](shadow-properties.md) tanıtılacaktır.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=6,15)]

Bu örnekte, ön bekleyen gezinti adının gereksiz olması nedeniyle gölge yabancı anahtarı `BlogId`.

> [!NOTE]
> Aynı ada sahip bir özellik zaten varsa, gölge Özellik adı bir sayıyla sonlenir.

### <a name="single-navigation-property"></a>Tek gezinti özelliği

Tek bir gezinti özelliği (ters gezinme olmadan ve yabancı anahtar özelliği olmadan) dahil olmak üzere, kural tarafından tanımlanan bir ilişkiye sahip olmak için yeterli değildir. Tek bir gezinti özelliğine ve yabancı anahtar özelliğine de sahip olabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=OneNavigation&highlight=6)]

### <a name="limitations"></a>Sınırlamalar

İki tür arasında tanımlı birden çok gezinti özelliği olduğunda (diğer bir deyişle, birbirine işaret eden tek bir bir çift gezintiden daha fazla), gezinti özellikleriyle temsil edilen ilişkiler belirsizdir. Belirsizliği çözümlemek için bunları el ile yapılandırmanız gerekir.

### <a name="cascade-delete"></a>Basamaklı silme

Kurala göre, Cascade DELETE, isteğe bağlı ilişkiler için gerekli ilişkiler ve *Clientsetnull* için *Cascade* olarak ayarlanacak. *Cascade* , bağımlı varlıkların de silindiği anlamına gelir. *Clientsetnull* , belleğe yüklenmeyen bağımlı varlıkların değişmeden kalacağından, el ile silinmesi ya da geçerli bir sorumlu varlığına işaret edecek şekilde güncelleştirilmeleri anlamına gelir. Belleğe yüklenen varlıklar için EF Core yabancı anahtar özelliklerini null olarak ayarlamaya çalışacaktır.

Gerekli ve isteğe bağlı ilişkiler arasındaki fark için [gerekli ve Isteğe bağlı ilişkiler](#required-and-optional-relationships) bölümüne bakın.

Farklı silme davranışları ve kural tarafından kullanılan varsayılanlar hakkında daha fazla ayrıntı için bkz. [Cascade Delete](../saving/cascade-delete.md) .

## <a name="manual-configuration"></a>El ile yapılandırma

### <a name="fluent-apitabfluent-api"></a>[Akıcı API](#tab/fluent-api)

Akıcı API 'de bir ilişki yapılandırmak için, ilişkiyi oluşturan gezinti özelliklerini tanımlayarak başlacaksınız. `HasOne` veya `HasMany`, üzerinde yapılandırmaya başmakta olduğunuz varlık türünde gezinti özelliğini tanımlar. Daha sonra ters gezintiyi belirlemek için `WithOne` veya `WithMany` bir çağrı zincirleyebilirsiniz. `HasOne`/`WithOne` başvuru gezinti özellikleri için kullanılır ve koleksiyon gezinti özellikleri için `HasMany`/`WithMany` kullanılır.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=8-10)]

### <a name="data-annotationstabdata-annotations"></a>[Veri açıklamaları](#tab/data-annotations)

Bağımlı ve birincil varlıklarda gezinti özelliklerinin nasıl kullandığını yapılandırmak için veri açıklamalarını kullanabilirsiniz. Bu, genellikle iki varlık türü arasında birden fazla gezinti özelliği olduğunda yapılır.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?name=InverseProperty&highlight=20,23)]

> [!NOTE]
> İlişkinin gerekilliğini etkilemek için yalnızca bağımlı varlıktaki özelliklerde [Required] kullanabilirsiniz. [Gerekli] asıl varlıktaki gezinmede genellikle yok sayılır, ancak varlığın bağımlı bir duruma gelmesine neden olabilir.

> [!NOTE]
> `[ForeignKey]` ve `[InverseProperty]` veri ek açıklamaları `System.ComponentModel.DataAnnotations.Schema` ad alanında kullanılabilir. `[Required]`, `System.ComponentModel.DataAnnotations` ad alanında kullanılabilir.

---

### <a name="single-navigation-property"></a>Tek gezinti özelliği

Yalnızca bir gezinti özelliğine sahipseniz, `WithOne` ve `WithMany`Parametresiz aşırı yüklemeleri vardır. Bu, ilişkinin diğer ucunda kavramsal olarak bir başvuru veya koleksiyon olduğunu gösterir, ancak varlık sınıfında hiçbir gezinti özelliği dahil değildir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?name=OneNavigation&highlight=8-10)]

### <a name="foreign-key"></a>Yabancı anahtar

#### <a name="fluent-api-simple-keytabfluent-api-simple-key"></a>[Akıcı API (basit anahtar)](#tab/fluent-api-simple-key)

Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için Floent API 'sini kullanabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?name=ForeignKey&highlight=11)]

#### <a name="fluent-api-composite-keytabfluent-api-composite-key"></a>[Akıcı API (bileşik anahtar)](#tab/fluent-api-composite-key)

Belirli bir ilişki için bileşik yabancı anahtar özellikleri olarak kullanılması gereken özellikleri yapılandırmak için akıcı API 'YI kullanabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?name=CompositeForeignKey&highlight=13)]

#### <a name="data-annotations-simple-keytabdata-annotations-simple-key"></a>[Veri açıklamaları (basit anahtar)](#tab/data-annotations-simple-key)

Belirli bir ilişki için yabancı anahtar özelliği olarak kullanılması gereken özelliği yapılandırmak için veri açıklamalarını kullanabilirsiniz. Bu, genellikle yabancı anahtar özelliği kural tarafından keşfedildiğinde yapılır:

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?name=ForeignKey&highlight=17)]

> [!TIP]  
> `[ForeignKey]` ek açıklaması, ilişkide herhangi bir gezinti özelliğine eklenebilir. Bağımlı varlık sınıfında gezinti özelliğine gitmesinin gerekli değildir.

> [!NOTE]
> Gezinti özelliğinde `[ForeignKey]` kullanılarak belirtilen özelliğin bağımlı tür olması gerekmez. Bu durumda, belirtilen ad bir gölge yabancı anahtar oluşturmak için kullanılacaktır.

---

#### <a name="shadow-foreign-key"></a>Gölge yabancı anahtar

Bir gölge özelliği yabancı anahtar olarak yapılandırmak için `HasForeignKey(...)` dize aşırı yüklemesini kullanabilirsiniz (daha fazla bilgi için bkz. [gölge özellikleri](shadow-properties.md) ). Gölge özelliğini, yabancı anahtar olarak kullanmadan önce modele açıkça eklememiz önerilir (aşağıda gösterildiği gibi).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs?name=ShadowForeignKey&highlight=10,16)]

#### <a name="foreign-key-constraint-name"></a>Yabancı anahtar kısıtlama adı

Kural gereği, bir ilişkisel veritabanını hedeflerken, yabancı anahtar kısıtlamaları FK_<dependent type name> _<principal type name>_ <foreign key property name>olarak adlandırılır. Bileşik yabancı anahtarlar için <foreign key property name> yabancı anahtar özellik adlarının alt çizgiyle ayrılmış bir listesi haline gelir.

Kısıtlama adını aşağıdaki şekilde de yapılandırabilirsiniz:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ConstraintName.cs?name=ConstraintName&highlight=6-7)]

### <a name="without-navigation-property"></a>Gezinti özelliği olmadan

Bir gezinti özelliği sağlamanız gerekmez. İlişkinin yalnızca bir tarafında yabancı anahtar sağlayabilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?name=NoNavigation&highlight=8-11)]

### <a name="principal-key"></a>Asıl anahtar

Yabancı anahtarın birincil anahtar dışında bir özelliğe başvurmasına isterseniz, ilişkinin asıl anahtar özelliğini yapılandırmak için Floent API 'sini kullanabilirsiniz. Asıl anahtar olarak yapılandırdığınız özellik otomatik olarak [alternatif bir anahtar](alternate-keys.md)olarak ayarlar.

#### <a name="simple-keytabsimple-key"></a>[Basit anahtar](#tab/simple-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

#### <a name="composite-keytabcomposite-key"></a>[Bileşik anahtar](#tab/composite-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=CompositePrincipalKey&highlight=11)]

> [!WARNING]  
> Asıl anahtar özelliklerini belirttiğiniz sıra, yabancı anahtar için belirtildikleri sırayla eşleşmelidir.

---

### <a name="required-and-optional-relationships"></a>Gerekli ve isteğe bağlı ilişkiler

İlişkinin gerekli veya isteğe bağlı olup olmadığını yapılandırmak için Floent API 'sini kullanabilirsiniz. Sonuç olarak bu, yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığını denetler. Bu, büyük bir gölge durumu yabancı anahtar kullanırken faydalıdır. Varlık sınıfınızda yabancı anahtar özelliği varsa, ilişkinin gereklik durumu yabancı anahtar özelliğinin gerekli veya isteğe bağlı olup olmadığına göre belirlenir (daha fazla bilgi için bkz. [gerekli ve Isteğe bağlı özellikler](required-optional.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=6)]

> [!NOTE]
> `IsRequired(false)` çağrısı, aksi belirtilmedikçe yabancı anahtar özelliğini de isteğe bağlı hale getirir.

### <a name="cascade-delete"></a>Basamaklı silme

Belirli bir ilişki için basamaklı silme davranışını açıkça yapılandırmak için Floent API 'sini kullanabilirsiniz.

Her seçeneğe ilişkin ayrıntılı bir tartışma için bkz. [Cascade Delete](../saving/cascade-delete.md) .

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=6)]

## <a name="other-relationship-patterns"></a>Diğer ilişki desenleri

### <a name="one-to-one"></a>Bire bir

Bir ilişkinin her iki tarafında da başvuru gezintisi özelliği vardır. Tek-çok ilişkilerle aynı kurallara uyar, ancak her sorumluyla yalnızca bir bağımlı ilişki olduğundan emin olmak için yabancı anahtar özelliğinde benzersiz bir dizin sunulmuştur.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=OneToOne&highlight=6,15-16)]

> [!NOTE]  
> EF, bir yabancı anahtar özelliği algılama özelliğine göre bağımlı olacak varlıklardan birini seçer. Bağımlı olarak yanlış varlık seçilirse, bunu düzeltmek için akıcı API 'yi kullanabilirsiniz.

Akıcı API ile ilişkiyi yapılandırırken `HasOne` ve `WithOne` yöntemlerini kullanırsınız.

Yabancı anahtarı yapılandırırken, bağımlı varlık türünü belirtmeniz gerekir-aşağıdaki listede `HasForeignKey` için belirtilen genel parametreyi görürsünüz. Bire çok ilişkisinde, başvuru gezinmesi olan varlığın bağımlı olduğunu ve koleksiyonun asıl öğe olduğunu unutmayın. Ancak bu, bire bir ilişkide değildir, bu nedenle açıkça tanımlanması gerekir.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>Çok-çok

JOIN tablosunu temsil eden bir varlık sınıfı olmayan çoktan çoğa ilişkiler henüz desteklenmemektedir. Bununla birlikte, JOIN tablosuna bir varlık sınıfı ekleyerek ve iki ayrı bire çok ilişkiyi eşleyerek çoktan çoğa ilişkiyi temsil edebilirsiniz.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11-14,16-19,39-46)]
