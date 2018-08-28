---
title: Özel kod öncelikli kurallar - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: 79450790c6d3c8ce7fad209e3946e81d3fad4b75
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995834"
---
# <a name="custom-code-first-conventions"></a>Özel kod öncelikli kurallar
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.

Code First kullanarak modelinizi sınıflardan kuralları kümesi kullanılarak hesaplanır. Varsayılan [kod öncelikli kurallar](~/ef6/modeling/code-first/conventions/built-in.md) şeyler gibi özellik olur bir varlığın birincil anahtarı, bir varlık eşler için tablo ve hangi kesinlik ve ölçek ondalık bir sütun varsayılan olarak sahip adını belirleyin.

Bazen bu varsayılan kuralları modeliniz için ideal değildir ve bunların etrafına veri ek açıklamaları ya da Fluent API'sini kullanarak birçok tek tek varlıklarla yapılandırarak çalışmak zorunda. Özel kod öncelikli kurallar modeliniz için yapılandırma Varsayılanları sağlayan kendi kuralları tanımlamanıza olanak sağlar. Bu kılavuzda, biz özel kuralları ve bunların her biri oluşturma farklı türlerini keşfedin.


## <a name="model-based-conventions"></a>Model tabanlı kuralları

Bu sayfa DbModelBuilder API'si için özel kurallar kapsar. Bu API, çoğu özel kuralları yazmak için yeterli olmalıdır. Ancak, de mevcuttur özelliği, model tabanlı kuralları - oluşturulduktan sonra son modelin yönlendirme kurallarını yazmak için - Gelişmiş senaryolar işlemek için. Daha fazla bilgi için [Model tabanlı kuralları](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Modelimiz

Size sunduğumuz kuralları ile kullanabileceğiniz basit bir model tanımlayarak başlayalım. Aşağıdaki sınıflar, projenize ekleyin.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a>Özel kuralları ile tanışın

Varlık türü için birincil anahtar olmasını anahtar adlı herhangi bir özelliği yapılandıran bir kuralı yazalım.

Kuralları bağlamında OnModelCreating geçersiz kılarak erişilebilir model oluşturucu üzerinde etkindir. ProductContext sınıfı aşağıdaki gibi güncelleştirin:

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

Artık, herhangi bir özelliği modelimizi anahtar adlı olacak kendi parçası ne olursa olsun varlığın birincil anahtarı yapılandırılmış.

Biz de bizim kuralları daha belirli yapılandırma kullanacağız özelliği türüne göre filtreleme yapabilirsiniz:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Bu, varlığın anahtarı, ancak bir tamsayı ise yalnızca birincil anahtarının adlı tüm özelliklerini yapılandırır.

Iskey yöntemi ilgi çekici bir özelliğidir, olmasıdır eklenebilir. Bu, birden çok özellikleri Iskey çağırın ve tüm bileşik anahtarın bir parçası olacak anlamına gelir. Bunun için bir uyarı, bir anahtar için birden çok özellik belirttiğinizde bu özellikler için bir sipariş da belirtmelisiniz ' dir. Bu yöntem gibi aşağıda HasColumnOrder çağırarak yapabilirsiniz:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Bu kod türlerine int anahtar sütunu ve dize adı sütunu oluşan bileşik anahtara sahip modelimizi yapılandıracaksınız. Biz modeli Tasarımcısı'nda görüntülediğinizde şöyle görünebilir:

![compositeKey](~/ef6/media/compositekey.png)

Başka bir özellik kuralları my modeldeki datetime yerine SQL Server datetime2 türüne eşlemek için tüm DateTime özelliklerini yapılandırmak için örneğidir. Bunu aşağıdaki elde:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Kuralı sınıfları

Kuralları tanımlamanın bir başka yolu kuralınızın yalıtılacak kuralı sınıfı kullanmaktır. Bir kuralı sınıf kullanırken System.Data.Entity.ModelConfiguration.Conventions ad alanındaki kuralı sınıfından devralan bir tür oluşturun.

Aşağıdakileri yaparak size daha önce gösterilen datetime2 kuralıyla bir kuralı sınıf oluşturabiliriz:

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

Bu kural, adım adım kılavuzla birlikte takip ediyorsanız, şuna benzeyecektir OnModelCreating kuralları koleksiyona eklediğiniz kullanılacak EF bildirmek için:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Gördüğünüz gibi biz bizim kuralı örneği kuralları koleksiyona ekleyin. Kuralı devralan gruplandırma ve takımlar veya projeleri arasında kuralları paylaşımı kullanışlı bir yol sağlar. Örneğin, bir sınıf kitaplığı ile kuruluşunuzun tüm projeleri, kullanım kuralları ortak bir dizi olabilir.

 

## <a name="custom-attributes"></a>Özel Öznitelikler

Kuralları başka bir harika kullanımı modeli yapılandırırken kullanılacak yeni bir öznitelik etkinleştirmektir. Bunu açıklamak üzere; dize özellikleri Unicode olmayan işaretlemek için kullanabileceğiniz bir öznitelik oluşturalım.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Şimdi, bu öznitelik için modelimizi uygulamak için bir kural oluşturalım:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Bu kural ile herhangi bir veritabanı sütunu anlamına gelir bizim dize özellikleri Unicode olmayan tüm karşılaştırmalar öznitelik nvarchar yerine varchar olarak depolanan ekleyebilirsiniz.

Bir özel durum oluşturur sonra Unicode olmayan tüm karşılaştırmalar özniteliği bir dize özelliği dışında herhangi bir şey üzerinde koyarsanız bu kural hakkında dikkat edilecek bir şey olmasıdır. Bir dize dışında herhangi bir türü IsUnicode yapılandıramıyorsunuz için bunu yapar. Bu durumda, böylece bir dize olmayan şeyi filtreleri sonra kuralınızın daha belirli yapabilirsiniz.

Özel öznitelikler tanımlamak için yukarıdaki kuralı çalışırken kullanmak özellikle öznitelik sınıfından özellikleri kullanmak istediğinizde çok daha kolay olabilir başka bir API yoktur.

Bu örnekte bizim özniteliği güncelleştirme ve şuna benzer şekilde IsUnicode özniteliğin değiştirme kullanacağız:

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

Biz bunu aldıktan sonra bool bizim özniteliği olup olmadığını bir özelliği Unicode olmalıdır kuralı bildirmek için ayarlayabilirsiniz. Biz zaten böyle yapılandırma sınıfın ClrProperty erişerek sahibiz kuralı içinde yapabilirsiniz:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Bu oldukça kolaydır, ancak sahip kullanarak bunu elde etmenin daha birleştiren bir yolu yoktur API kurallarının yöntemi. Sahip yönteminin bir parametresi vardır, Func yazın&lt;PropertyInfo, T&gt; kabul eden PropertyInfo Where aynı yöntemi, bir nesneyi döndürmek için bekleniyordu ancak. Özellik yapılandırılmadı sonra döndürülen nesne null ise, yani özellikleriyle, nerede olduğu gibi kullanıma filtre uygulayabilirsiniz, ancak ayrıca döndürülen nesne yakalamak ve yapılandırma yönteme geçirin, farklıdır. Bu, aşağıdaki gibi çalışır:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Özel özniteliklere sahip kullanmak için tek nedeni olmayan yöntemi, bu yararlıdır, türler veya özellikler yapılandırırken filtre uyguladığınız bir şey hakkında neden gereken herhangi bir yerde.

 

## <a name="configuring-types"></a>Yapılandırma türü

Kadarki tüm müşterilerimizin kuralları özelliklerini silinmiş, ancak türleri modelinizde yapılandırmak için başka bir alan kurallarının API olduğu. Deneyimi şu ana kadar gördük kurallarına benzer, ancak varlık özelliği yerine şu iç yapılandırma seçenekleri düzeyinde olacaktır.

Tür düzeyi kuralları için gerçekten kullanışlı olabilecek şeylerden biri tablo adlandırma kuralı, EF varsayılandan farklı bir var olan bir şema eşlemek için veya farklı bir adlandırma kuralı ile yeni bir veritabanı oluşturmak için değişiyor. Bunu yapmak için öncelikle TypeInfo modelimizi bir türü için kabul edebilir ve dönüş türü için tablo adı ne olmalıdır bir yöntem ihtiyacımız:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Bu yöntem, bir türü alır ve alt çizgi CamelCase yerine küçük kullanan bir dize döndürür. Modelimiz bu ProductCategory sınıfı ürün adlı bir tabloya eşlenmesi anlamına gelir\_öğelerini tire yerine kategorisi.

Bu yöntem sahibiz sonra size, böyle bir kural içinde çağırabilirsiniz:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Bu kural her tür modelimiz bizim GetTableName yönteminden döndürülen tablo adını eşleştirmek için yapılandırır. Bu kural Fluent API'sini kullanarak modeldeki her bir varlık için ToTable metodunu eşdeğerdir.

Bu hakkında dikkat edilecek bir çağırdığınızda ToTable EF herhangi bir tablo adları belirlerken normalde yaptığınız çoğullaştırma olmadan tam tablo adı olarak sağlayan dize süreceğini şeydir. Tablo adı bizim kuralı ürünüdür. Bu yüzden\_kategori yerine ürün\_kategorileri. Bizim kuralına kendimize çoğullaştırma hizmetine bir çağrı yaparak çözebiliriz.

Aşağıdaki kodda kullanacağız [bağımlılık çözümlemesi](~/ef6/fundamentals/configuring/dependency-resolution.md) EF kullandığınız çoğullaştırma hizmet alınacak EF6 içinde eklenen özellik ve pluralize bizim tablo adı.

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> Genel sürümünü GetService System.Data.Entity.Infrastructure.DependencyResolution ad alanındaki bir genişletme yöntemi, kullanarak bir eklemeniz gerekecektir Bağlamınızı kullanmak için deyimi.

### <a name="totable-and-inheritance"></a>ToTable ve devralma

Bir tür açıkça belirli bir tabloda eşlerseniz EF kullanan eşleme stratejisi değiştirebilirsiniz sonra başka bir önemli yönüyle ToTable olmasıdır. Devralma hiyerarşisinde her tür için ToTable çağırırsanız, yukarıda yaptığımız gibi tür adı tablonun adı geçirerek daha sonra varsayılan tablo başına hiyerarşi (TPH) eşleme stratejisi tablo başına tür (TPT) değişir. Bunu açıklamak için en iyi yolu whith somut bir örnek verilmiştir:

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

Varsayılan olarak, veritabanındaki (çalışan) aynı tabloya hem çalışan hem de manager eşlenir. Tablo çalışan ve yöneticilerin ne tür bir örnek, her satır için depolanan söyleyecektir bir ayrıştırıcı sütunu içerir. Hiyerarşi için tek bir tablo olmadığından TPH eşleme budur. Her iki classe üzerinde ToTable çağırırsanız ancak ardından her tür bunun yerine kendi tablo olarak da bilinen her türü kendi tablosunda bulunduğundan TPT eşleştirilir.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Yukarıdaki kodu aşağıdakine benzer bir tablo yapısı için eşler:

![tptExample](~/ef6/media/tptexample.jpg)

Bu durumu önlemek ve birkaç yolla varsayılan TPH eşleme Koru:

1.  Hiyerarşideki her bir türü için aynı tablo adıyla ToTable çağırın.
2.  ToTable yalnızca üzerinde çalışan olacaktır Bizim örneğimizde hiyerarşinin temel sınıfı çağırın.

 

## <a name="execution-order"></a>Yürütme sırası

Bir son WINS şekilde Fluent API'si ile aynı kuralları çalışır. Ne bu yapılandırma aynı seçeneği aynı özelliğin iki kuralları ve WINS yürütülecek sonuncu yazarsanız, anlamına gelir. Örneğin, aşağıdaki kod tüm dizeleri en fazla uzunluğu 500'e ayarlanır ancak ardından 250 uzunluk üst sınırını olmasını modelinde adı olan tüm özellikleri yapılandırıyoruz.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Uzunluk üst sınırı 250'ye ayarlamak için kuralı tüm dizeleri 500'e ayarlayan bir sonra olduğundan, bizim modelinde adı tüm özellikler 250 açıklamaları gibi herhangi diğer dizeler çalışırken, bir MaxLength gerekir, 500 olacaktır. Bu şekilde kurallarını kullanarak anlamına gelir, genel bir kural türleri veya modeli ve ardından geçersiz kılma özellikleri sağlayabilirsiniz bunları farklı alt kümeleri için.

Fluent API'si ve veri ek açıklamaları belirli durumlarda bir yöntemi geçersiz kılmak için de kullanılabilir. Biz Fluent API'si uzunluğu bir özelliği ayarlamak için kullanmışsınız daha belirli Fluent API'si daha genel yapılandırma kuralı kazanacak çünkü yukarıdaki ardından biz bunu önce veya sonra kural, yerleştirebilirsiniz.

 

## <a name="built-in-conventions"></a>Yerleşik kuralları

Özel kurallar tarafından varsayılan kod öncelikli kurallar etkilenebilir, çünkü önce veya sonra başka bir kuralı çalıştırmak için kuralları eklemek yararlı olabilir. Bunu yapmak için türetilmiş bir DbContext üzerinde kuralları koleksiyonu AddBefore ve AddAfter yöntemlerini kullanabilirsiniz. Aşağıdaki kod, oluşturduğumuz daha önce oluşturulmuş önce çalıştıracağı kuralı sınıfı eklersiniz, anahtar bulma kuralı.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Bu geçmeden önce veya sonra yerleşik kurallarını çalıştırmak için gereksinim kuralları eklerken en çok kullanımı olması için yerleşik kuralları listesini burada bulunabilir: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

Ayrıca, modelinize uygulanan istemediğiniz kuralları kaldırabilirsiniz. Bir kuralı kaldırmak için Remove yöntemi kullanın. PluralizingTableNameConvention kaldırmanın bir örnek aşağıda verilmiştir.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
