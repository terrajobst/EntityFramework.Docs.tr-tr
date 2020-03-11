---
title: Özel Code First kuralları-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419229"
---
# <a name="custom-code-first-conventions"></a>Özel Code First kuralları
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.

Code First kullanılırken, modelinizle bir dizi kural kullanarak sınıfınızdan hesaplanır. Varsayılan [Code First kuralları](~/ef6/modeling/code-first/conventions/built-in.md) , hangi özelliğin bir varlığın birincil anahtarı, bir varlığın eşlendiği tablo adı ve bir ondalık sütunu için varsayılan olarak hangi duyarlık ve ölçekleme olacağını belirleyen şeyleri belirlemektir.

Bazen bu varsayılan kurallar modelinize uygun değildir ve veri açıklamalarını veya akıcı API 'YI kullanarak çok sayıda varlığı yapılandırarak onları geçici olarak çözebilirsiniz. Özel Code First kuralları modelinize yönelik yapılandırma Varsayılanları sağlayan kendi kurallarınızı tanımlamanızı sağlar. Bu kılavuzda, farklı özel kural türlerini ve bunların her birini oluşturmayı inceleyeceğiz.


## <a name="model-based-conventions"></a>Model tabanlı kurallar

Bu sayfa, özel kurallar için DbModelBuilder API 'sini içerir. Bu API, çoğu özel kuralı yazmak için yeterli olmalıdır. Bununla birlikte, gelişmiş senaryoları işlemek için, oluşturulduktan sonra son modeli düzenleyen model tabanlı kuralları yazma özelliği de vardır. Daha fazla bilgi için bkz. [model tabanlı kurallar](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Modelimiz

Kılavuzlarımızla birlikte kullanabilmemiz için basit bir model tanımlayarak başlayalım. Aşağıdaki sınıfları projenize ekleyin.

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

 

## <a name="introducing-custom-conventions"></a>Özel kurallara giriş

Anahtar adlı herhangi bir özelliği, varlık türü için birincil anahtar olacak şekilde yapılandıran bir kural yazalım.

Kurallar, bağlamda Onmodeloluþturma geçersiz kılınarak erişilebilen model Oluşturucu üzerinde etkinleştirilir. ProductContext sınıfını aşağıdaki gibi güncelleştirin:

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

Şimdi, modelinizdeki anahtar adlı bir özellik, bölümünün parçası olan her bir varlığın birincil anahtarı olarak yapılandırılacaktır.

Ayrıca, yapılandırdığımız Özellik türü üzerine filtreleyerek kurallarımızı daha belirgin hale yapabiliriz:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Bu, anahtar adlı tüm özellikleri varlığının birincil anahtarı olacak şekilde yapılandırır, ancak yalnızca bir tamsayıdır.

IsKey yönteminin ilginç bir özelliği eklenebilir. Yani, IsKey öğesini birden çok özelliklerde çağırdığınızda ve hepsi bir bileşik anahtarın parçası haline gelirse. Bunun için bir desteklenmediği uyarısıyla, bir anahtar için birden çok özellik belirttiğinizde, bu özellikler için de bir sıra belirtmeniz gerekir. Bunu aşağıdaki gibi Hasccolumnorder metodunu çağırarak yapabilirsiniz:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Bu kod, modelimizin türlerini, int anahtar sütununu ve dize adı sütununu içeren bileşik bir anahtara sahip olacak şekilde yapılandırır. Modeli tasarımcıda görüntüleyebilmemiz şu şekilde görünür:

![bileşik anahtar](~/ef6/media/compositekey.png)

Özellik kurallarına başka bir örnek, modelinmdeki tüm DateTime özelliklerini, DateTime yerine SQL Server datetime2 türüyle eşlenecek şekilde yapılandırmaktır. Bunu aşağıdakiler ile elde edebilirsiniz:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Kural sınıfları

Kuralları tanımlamanın bir diğer yolu, kurallarınızı kapsüllemek için bir kural sınıfı kullanmaktır. Bir kural sınıfı kullanırken, System. Data. Entity. ModelConfiguration. Convention ad alanındaki kural sınıfından devralan bir tür oluşturursunuz.

Daha önce aşağıdakileri yaparak, datetime2 kuralına sahip bir kural sınıfı oluşturuyoruz:

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

Bu kuralı kullanmak için EF 'in bu kuralı kullanmasını söylemek için, bu yönergeyi Onmodeloluþturma 'daki kurallara göre eklersiniz. Bu, izlenecek yol aşağıdaki gibi görünür:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Gördüğünüz gibi, kural koleksiyonu için kuralımız bir örnek ekleyeceğiz. Kurallardan devralma, takımlar veya projeler arasında kuralları gruplandırmanın ve paylaşmanın kolay bir yolunu sağlar. Örneğin, tüm kuruluşların projelerinin kullandığı ortak bir kural kümesine sahip bir sınıf kitaplığına sahip olabilirsiniz.

 

## <a name="custom-attributes"></a>Özel Öznitelikler

Kuralların diğer harika kullanımı, bir modeli yapılandırırken yeni özniteliklerin kullanılmasını olanaklı hale kullanmaktır. Bunu göstermek için, dize özelliklerini Unicode olmayan olarak işaretlemek üzere kullandığımız bir öznitelik oluşturalım.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Şimdi bu özniteliği modelinize uygulamak için bir kural oluşturalım:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Bu kural ile Unicode olmayan özniteliğini dize özelliklerinden herhangi birine ekleyebiliriz. Bu, veritabanındaki sütunun nvarchar yerine varchar olarak depolanacağı anlamına gelir.

Bu kural hakkında daha fazla bilgi için, Unicode olmayan özniteliğini dize özelliği dışındaki herhangi bir yere yerleştirirseniz bir özel durum oluşturur. Bu, bir dize dışında herhangi bir türde ısunıcode 'u yapılandıramadığı için bunu yapar. Bu durumda, bir dize olmayan herhangi bir şeyi filtreleyerek, daha sonra, kuralınızın daha özel olmasını sağlayabilirsiniz.

Yukarıdaki kural özel öznitelikler tanımlamak için çalışırken, özellikle öznitelik sınıfından özellikleri kullanmak istediğinizde, daha kolay olabilecek başka bir API vardır.

Bu örnekte, öznitemizi güncelleyeceğiz ve bunu bir ıunıcode özniteliğiyle değiştiririz, bu nedenle şöyle görünür:

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

Bunu yaptıktan sonra, bir özelliğin Unicode olması gerekip gerekmediğini bildirmek için öznitemize bir bool ayarlayabiliriz. Bunu şu şekilde yapılandırma sınıfının ClrProperty öğesine erişmekte olduğumuz kurala göre yapabiliriz:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Bu çok kolaydır, ancak kuralları API 'nin HAVING metodunu kullanarak elde etmenin daha kısa bir yolu vardır. HAVING yöntemi Func&lt;PropertyInfo, T&gt; türünde bir parametreye sahiptir ve bu, WHERE yöntemiyle aynı şekilde PropertyInfo kabul eder, ancak bir nesne döndürmesi beklenir. Döndürülen nesne null ise, özelliği yapılandırılmaz, bu, özellikleri burada olduğu gibi filtreleyebilirsiniz, ancak döndürülen nesneyi de yakalayıp yapılandırma yöntemine iletmektir çünkü farklı olur. Bu, aşağıdaki gibi çalışmaktadır:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Özel öznitelikler HAVING metodunu kullanmanın tek nedeni olmadığından, türlerinizi veya özellikleri yapılandırırken filtrelemediğiniz bir şey hakkında neden olmanız gerektiği her yerde yararlı olur.

 

## <a name="configuring-types"></a>Türleri yapılandırma

Şimdiye kadar tüm kurallarımızda özellikler için olduğundan, modelinizdeki türleri yapılandırmak için kurallar API 'sinin başka bir alanı vardır. Deneyim şimdiye kadar görtiğimiz kurallara benzer, ancak yapılandırma içindeki Seçenekler özellik düzeyi yerine varlıkta olacaktır.

Tür düzeyi kuralları için gerçekten yararlı olabilecek olan işlemlerden biri, bir veya daha fazla farklı adlandırma kuralına sahip olan mevcut bir şemayla eşlemek için tablo adlandırma kuralını değiştiriyor. Bunu yapmak için ilk olarak modelinizdeki bir türün TypeInfo kabul edebilecek ve bu tür için tablo adının ne olması gerektiğini döndüren bir yönteme ihtiyaç duyarsınız:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Bu yöntem bir tür alır ve CamelCase yerine alt çizgi ile küçük harf kullanan bir dize döndürür. Modelimizde bu, ProductCategory sınıfının ProductCategories yerine ürün\_kategorisi adlı bir tabloyla eşlenecek anlamına gelir.

Bu yönteme ulaştıktan sonra, bunu şöyle çağırabiliriz:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Bu kural, modelimizin her türünü, GetTableName yönteminden döndürülen tablo adıyla eşlenecek şekilde yapılandırır. Bu kural, akıcı API kullanılarak modeldeki her varlık için ToTable yöntemini çağırmaya eşdeğerdir.

Bunun için bir şey, ToTable EF 'i çağırdığınızda tablo adlarını belirlerken normalde yapacağından emin olmak için, tam tablo adı olarak sağladığınız dizeyi doğru bir şekilde alır. Kuralımız tablo adının ürün\_kategorileri yerine ürün\_kategorisi olması neden olur. Çoğullaştırma hizmeti kendimize ' e bir çağrı yaparak kurallarımızda bunu çözebiliriz.

Aşağıdaki kodda, EF 'in kullandığı ve tablo adınızla plmış olan çoğullaştırma hizmetini almak için EF6 ' de eklenen [bağımlılık çözümleme](~/ef6/fundamentals/configuring/dependency-resolution.md) özelliğini kullanacağız.

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
> GetService 'in genel sürümü System. Data. Entity. Infrastructure. DependencyResolution ad alanındaki bir genişletme yöntemidir, bunu kullanabilmeniz için bağlamına bir using ifadesini eklemeniz gerekir.

### <a name="totable-and-inheritance"></a>ToTable ve devralma

ToTable 'ın diğer önemli bir yönü, bir türü belirli bir tabloyla açıkça eşleştirdiğinizde, EF 'in kullanacağı eşleme stratejisini değiştirebilirsiniz. Bir devralma hiyerarşisindeki her tür için ToTable çağrısı yaparsanız, tür adını yukarıda yaptığımız gibi tablonun adı olarak geçirerek, varsayılan hiyerarşi başına (TPH) eşleme stratejisini tür başına tablo (TPT) olarak değiştirirsiniz. Bunu tanımlamanın en iyi yolu somut bir örnektir.

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

Varsayılan olarak, hem çalışan hem de yönetici veritabanında aynı tablo (çalışanlar) ile eşlenir. Tabloda, her bir satırda ne tür bir örnek depolandığını söyleyen bir Ayrıştırıcı sütunu olan çalışanlar ve yöneticiler bulunur. Bu, hiyerarşi için tek bir tablo olduğundan, bu TPH eşlemedir. Ancak, her iki ClassID üzerinde de ToTable ' ı çağırırsanız her bir türün kendi tablosu olduğu için, her tür kendi tablosuyla eşlenir (TPT olarak da bilinir).

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Yukarıdaki kod, aşağıdakine benzer bir tablo yapısına eşlenir:

![TPT örneği](~/ef6/media/tptexample.jpg)

Bunu ortadan kaldırabilirsiniz ve varsayılan TPH eşlemesini birkaç yolla koruyabilirsiniz:

1.  Hiyerarşideki her tür için aynı tablo adına sahip ToTable öğesini çağırın.
2.  Yalnızca hiyerarşinin temel sınıfında, örneğin Employee olacak şekilde ToTable ' ı çağırın.

 

## <a name="execution-order"></a>Yürütme sırası

Kurallar, akıcı API ile aynı olan son bir WINS biçiminde çalışır. Bunun anlamı, aynı özellikte aynı seçeneği yapılandıran iki kural yazarsanız ve ardından WINS 'i yürütmek için son bir şeydir. Örnek olarak, aşağıdaki kodda tüm dizelerin uzunluk üst sınırı 500 olarak ayarlanır ancak modeldeki ad adlı tüm özellikler en fazla 250 uzunluğunda olacak şekilde yapılandırılır.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

En fazla uzunluğu 250 olarak ayarlayan kural, tüm dizeleri 500 ' ye ayarlayan bir kural olduğundan, modelimizin adı adlı tüm özellikler, açıklamalar gibi diğer dizeler 500 olacak şekilde bir 250 MaxLength 'e sahip olur. Bu şekilde kuralların kullanılması, modelinizdeki türler veya özellikler için genel bir kural sağlayabilmeniz ve bunları farklı alt kümeler için overide.

Akıcı API ve veri ek açıklamaları, belirli durumlarda bir kuralı geçersiz kılmak için de kullanılabilir. Yukarıdaki örneğimizde, bir özelliğin maksimum uzunluğunu ayarlamak için akıcı API kullandığımızda, daha fazla sayıda akıcı API daha genel yapılandırma kuralını kazanacağından, bunu kurala göre veya sonra koyabiliriz.

 

## <a name="built-in-conventions"></a>Yerleşik kurallar

Özel kurallar varsayılan Code First kurallarından etkilendiğinden, başka bir kural öncesinde veya sonrasında çalıştırılacak kurallar eklemek yararlı olabilir. Bunu yapmak için, türetilmiş DbContext 'teki kural koleksiyonunun AddBefore ve Addadfter yöntemlerini kullanabilirsiniz. Aşağıdaki kod, daha önce oluşturduğumuz kural sınıfını ekleyerek yerleşik anahtar bulma kuralına göre çalışacaktır.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Bu, yerleşik kurallardan önce veya sonra çalıştırılması gereken kuralları eklerken en çok kullanılan bir deyişle, yerleşik kuralların bir listesi burada bulunabilir: [System. Data. Entity. ModelConfiguration. kurallara ad alanı](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).

Ayrıca, modelinize uygulanmasını istemediğiniz kuralları da kaldırabilirsiniz. Bir kuralı kaldırmak için Remove metodunu kullanın. PluralizingTableNameConvention öğesinin kaldırılmasına bir örnek aşağıda verilmiştir.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
