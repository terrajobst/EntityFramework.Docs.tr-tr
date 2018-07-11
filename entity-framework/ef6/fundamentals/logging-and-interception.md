---
title: Günlüğe kaydetme ve veritabanı işlemleri - EF6 kesintiye
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
caps.latest.revision: 3
ms.openlocfilehash: 1d0e953309f3c81a2941d6850e169aaa31ae8de0
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949288"
---
# <a name="logging-and-intercepting-database-operations"></a>Günlüğe kaydetme ve veritabanı işlemleri kesintiye
> [!NOTE]
> **EF6 ve sonraki sürümler yalnızca** -özellikler, API'ler, bu sayfada açıklanan vb., Entity Framework 6'da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bazı veya tüm bilgileri geçerli değildir.  

Entity Framework veritabanı için bir komutu, bu komut gönderir. herhangi bir zamanda Entity Framework 6 ile başlayarak, uygulama kodu tarafından yakalanabilir. SQL günlüğü için en yaygın olarak kullanılır, ancak değiştirmek veya komutu durdurun için de kullanılabilir.  

Özellikle, EF içerir:  

- LINQ to SQL'de DataContext.Log benzer bağlamı için bir günlük özelliği  
- İçerik ve'ına gönderilen çıktı biçimlendirme özelleştirmek için bir mekanizma  
- Daha fazla denetim/esnekliğini durdurma için alt düzey yapı taşları  

## <a name="context-log-property"></a>İçerik günlüğü özelliği  

DbContext.Database.Log özelliği bir dize alır herhangi bir yöntem için temsilci olarak ayarlanabilir. En yaygın olarak ayarlayarak bu TextWriter "Yazma" yöntemi için herhangi bir TextWriter ile kullanılır. Geçerli bağlam tarafından oluşturulan tüm SQL yazıcının için günlüğe kaydedilir. Örneğin, aşağıdaki kod, konsola SQL başlar.SSH:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Bu bağlamı dikkat edin. Database.Log Console.Write için ayarlanır. Tüm SQL konsolunda oturum için gereken budur.  

### <a name="example-output"></a>Örnek çıktı  

Biz bazı çıkış görebilmeniz için bazı basit sorgu/ekleme/güncelleştirme kod ekleyelim:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

Bu, aşağıdaki çıktıyı üretir:  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

(Bu veritabanı sıfırlamaları çoktan olmuş varsayarsak çıkış olduğunu unutmayın. "Olacaktır sonra veritabanı başlatma zaten olmayan oluştuysa tüm iş geçişler gösteren çok daha fazla çıkış olup olmadığını denetleyin veya yeni bir veritabanı oluşturmak için arka planda gerçekleştirir.)  

### <a name="what-gets-logged"></a>Oturum?  

Günlük özelliği aşağıdakilerin tümü ayarlandığında kaydedilir:  

- SQL komutları tüm farklı türleri için. Örneğin:  
    - Normal LINQ sorguları, eSQL sorgular ve ham sorgularından SqlQuery gibi yöntemleri dahil olmak üzere sorgular  
    - Ekleme, güncelleştirme ve silme SaveChanges bir parçası olarak oluşturulan  
    - İlişki sorgular tarafından Gecikmeli yükleme oluşturulanların gibi yükleniyor  
- Parametreler  
- Bir komut olup olmadığını zaman uyumsuz olarak yürütülüyor  
- Komut yürütme başlatıldığında belirten bir zaman damgası  
- Komut başarıyla tamamlandı, özel bir durum yaratarak veya, zaman uyumsuz başarısız olsun veya olmasın, iptal edildi  
- Sonuç değeri bazı göstergesi  
- Bu komutu yürütmek için geçen süre yaklaşık miktarı. Bu sonuç nesnesi geri almak için komut göndermesini saat olduğuna dikkat edin. Sonuçları okuma süresi içermez.  

Yukarıdaki örnek çıktısına baktığınızda, her oturum dört komuttan biri şunlardır:  

- Bağlam çağrısından kaynaklanan sorgu. Blogs.First  
    - ToString çağrılabilir Iqueryable SQL alma ToString yöntemini beri bu sorgu için öğrenmeniz gerekir bildirimi "First" sağlamaz  
- Yavaş-yüklenmesini blog sonuçlanan sorgu. Gönderiler  
    - Bildirimi anahtar değeri geç hangi yükleme için parametre ayrıntılarını olmuyor.  
    - Varsayılan olmayan değerlere ayarlanır özellikleri parametresinin günlüğe kaydedilir. Örneğin, boyut özelliği yalnızca sıfır olup olmadığını gösterilir.  
- İki komuttan SaveChangesAsync kaynaklanan; bir güncelleştirme sonrası başlık, yeni bir gönderi eklemek bir ekleme için diğer değiştirmek için  
    - FK ve başlık özelliklerini parametre ayrıntılarını dikkat edin.  
    - Bu komutları yürütülürken zaman uyumsuz olarak dikkat edin.  

### <a name="logging-to-different-places"></a>Farklı konumlara günlüğe kaydetme  

Günlüğe kaydetme için yukarıda da gösterildiği gibi konsolun çok kolaydır. Bellek, dosya, vb. için farklı kullanarak oturum kolaydır, [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

LINQ ile SQL, LINQ to SQL günlüğü özelliği gerçek TextWriter nesnesinde (örneğin, Console.Out) sırasında bir dize (örneğin kabul eden bir yönteme günlük özelliği ayarlanmış EF ayarlı görebilirsiniz biliyorsanız Console.Write veya Console.Out.Write). Bunun nedeni TextWriter'dan EF dizeleri için bir havuz olarak davranıp herhangi bir temsilci kabul ederek ayırmaktır. Örneğin, bazı günlüğe kaydetme çerçevesi zaten var ve bir günlük yöntemi tanımlar Imagine şu şekilde:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Bu gibi bu EF günlük özelliğine ölçekledikçe:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

### <a name="result-logging"></a>Sonucu günlüğe kaydetme  

Komut veritabanına gönderilmeden önce varsayılan Günlükçü komut metni (SQL), parametreler ve "Executing" satırın zaman damgası ile kaydeder. Geçen süreyi içeren bir "tamamlandı" oturum aşağıdaki komutun yürütülmesi satırıdır.  

Zaman uyumsuz görev gerçekten tamamlandıktan, başarısız veya iptal kadar zaman uyumsuz komutları için "tamamlandı" satırı kaydedilmez unutmayın.  

"Tamamlandı" satırı komut ve yürütme başarılı olsun veya olmasın türüne bağlı olarak farklı bilgiler içerir.  

#### <a name="successful-execution"></a>Başarılı yürütme  

Çıkış başarıyla tamamlanması komutlar için "x sonucuyla ms içinde tamamlandı:" sonucu neydi bazı göstergesini tarafından izlenen. Bir veri okuyucu sonuç komutlarında türünü göstergesidir [dbdatareader öğesine dönüştürülemedi](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) döndürdü. Güncelleştirme gibi bir tamsayı değeri döndüren komutlar için gösterilen sonuç gösterilen komutu bu tamsayıdır.  

#### <a name="failed-execution"></a>Başarısız yürütme  

Bir özel durum ile başarısız komutları için çıkış özel durumdan ileti içerir. Örneğin, SqlQuery için var olan bir tablo sorgusu kullanarak günlük sonucunda aşağıdakine benzer çıktıyı verir:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

#### <a name="canceled-execution"></a>Yürütme işlemi iptal edildi  

Bu iptal etme denemesi yapıldığında, temel alınan ADO.NET sağlayıcısı genellikle yaptığı olduğundan burada görev iptal zaman uyumsuz komutları için sonuç bir özel durum ile başarısız olabilir. Bu gerçekleşmez ve görev düzgün bir şekilde iptal çıktısı aşağıdakine benzer görünecektir:  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Günlük içeriği değiştirme ve biçimlendirme  

### <a name="databaselogformatter"></a>DatabaseLogFormatter  

Özelliğin sağlar Database.Log perde DatabaseLogFormatter nesnesinin kullanın. Bu nesne (aşağıya bakın) IDbCommandInterceptor uygulama dizeleri ve bir DbContext kabul eden bir temsilci etkili bir şekilde bağlar. Bu, DatabaseLogFormatter yöntemlerde önce ve sonra komutların yürütülmesini EF tarafından çağrılır, anlamına gelir. Bu DatabaseLogFormatter yöntemleri toplayın ve günlük çıktısı biçimlendirmek ve temsilciye gönderebilirsiniz.  

### <a name="customizing-databaselogformatter"></a>DatabaseLogFormatter özelleştirme  

Günlüğe kaydedilenler ve nasıl biçimlendirildiğini değiştirme, DatabaseLogFormatter türetir ve uygun yöntemleri geçersiz kılan bir yeni sınıf oluşturarak gerçekleştirilebilir. Geçersiz kılmak için en yaygın yöntemler şunlardır:  

- LogCommand – bu komutları nasıl bunlar yürütülmeden önce günlüğe kaydedildiğini değiştirmek için geçersiz. Varsayılan olarak, her parametre için LogParameter LogCommand çağırır; geçersiz kılma aynı yapın veya bunun yerine parametreleri farklı şekilde işlemek tercih edebilirsiniz.  
- LogResult – bir komutun yürütülmesini sonucu nasıl kaydedilir değiştirmek için geçersiz kılar.  
- LogParameter – bu biçimlendirme ve parametre günlük içeriğini değiştirmek için geçersiz.  

Örneğin, her komut veritabanına gönderilmeden önce tek bir satır oturum istedik varsayalım. Bu iki geçersiz kılma ile yapılabilir:  

- Biçimlendirme ve tek satırlık bir SQL yazma LogCommand geçersiz kıl  
- Hiçbir şey yapmıyor LogResult geçersiz kılar.  

Kod aşağıdaki gibi görünür:

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

Oturum çıktı yapılandırılmış yazma temsilciye gönderir yazma yönteminin yalnızca çağrı çıktı.  

(Bu kodu satır sonları alıyormuş kaldırılmasını yalnızca örnek olarak yaptığı unutmayın. Bu büyük olasılıkla iyi karmaşık SQL görüntülemek için çalışmaz.)  

### <a name="setting-the-databaselogformatter"></a>DatabaseLogFormatter ayarlama  

Yeni bir DatabaseLogFormatter sınıf oluşturulduktan sonra EF ile kayıtlı olması gerekir. Bu yapılır kod tabanlı yapılandırma kullanarak. Buna koysalar bu aynı bütünleştirilmiş kodun DbContext sınıfınıza olarak DbConfiguration türeyen yeni bir sınıf oluşturursunuz ve ardından bu yeni sınıfın oluşturucusunda SetDatabaseLogFormatter arama anlamına gelir. Örneğin:  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>Yeni DatabaseLogFormatter kullanma  

Bu yeni DatabaseLogFormatter artık Database.Log ayarlanmış herhangi bir zamanda kullanılır. Bu nedenle, bölüm 1 ' kodu çalıştıran artık aşağıdaki çıktıda sonuçlanır:  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Durdurma yapı taşları  

Şu ana kadar EF tarafından oluşturulan SQL oturum DbContext.Database.Log kullanmayı inceledik. Ancak bu kodu görece basit bir cephe gerçekten daha genel durdurma için bazı alt düzey yapı taşları üzerinden.  

### <a name="interception-interfaces"></a>Durdurma arabirimleri  

Durdurma kodu durdurma arabirimleri kavramı üretilmiştir. Bu arabirimler IDbInterceptor devralır ve EF bazı eylem gerçekleştirdiğinde çağrılan yöntemlere tanımlayın. Amaç, bir arabirim başına geçirilmesinin nesne türü olmasını sağlamaktır. Örneğin, önce EF ExecuteNonQuery, ExecuteScalar, ExecuteReader ve ilişkili yöntemleri çağrıda çağrılan yöntemlere IDbCommandInterceptor arabirimi tanımlar. Benzer şekilde, bu işlemlerin her biri tamamlandığında çağrılan yöntemlere arabirimi tanımlar. Yukarıda inceledik DatabaseLogFormatter sınıfı komutları için bu arabirimi uygular.  

### <a name="the-interception-context"></a>Durdurma bağlamı  

Herhangi bir dinleyiciyi arabirimleri üzerinde tanımlanan yöntemler, bakarak her çağrı DbInterceptionContext türünde bir nesne veya bazı tür bu gibi DbCommandInterceptionContext türetildiğine emin görünen\<\>. Bu nesne EF sürüyor eylem hakkında bağlamsal bilgiler içerir. Örneğin, bir DbContext adına yapılan eylemi, DbContext DbInterceptionContext dahildir. Benzer şekilde, zaman uyumsuz olarak yürütülen komutlar için üzerinde DbCommandInterceptionContext Isasync bayrağı ayarlanır.  

### <a name="result-handling"></a>Sonuç işleme  

DbCommandInterceptionContext\< \> sınıfı sonucu, OriginalResult, özel durum ve özgün özel durum olarak adlandırılan bir özelliklerini içerir. Bu özellikler için null/sıfır işlemi yürütülmeden önce çağrılan durdurma yöntemlere yapılan çağrılar için ayarlanan — diğer bir deyişle, için... Yürütme yöntemleri. İşlemi yürütülür ve başarılı, işlemin sonucunu için sonuç ve OriginalResult ayarlanır. Bu değerleri işlemi yürütüldükten sonra çağrılan durdurma yöntemlere sonra gözlenecek — diğer bir deyişle,... Yürütülen yöntemler. İşlemi oluşturursa, benzer şekilde, daha sonra özel durum ve özgün özel durum özellikleri ayarlanır.  

#### <a name="suppressing-execution"></a>Yürütme gizleme  

Komutu yürüttüğünü önce bir dinleyiciyi sonucu özelliği ayarlar varsa (birinde... Yürütme yöntemleri) sonra EF gerçekten komutu yürütmek çalışmaz, ancak bunun yerine yalnızca sonuç kümesini kullanacak. Diğer bir deyişle, dinleyiciyi de komutu yürütmeye bastır ancak komutu yürüttüğünü gibi devam EF sahip.  

Bu nasıl kullanılabileceğine ilişkin bir örnek, bir sarmalama sağlayıcısıyla, toplu işleme komut geleneksel olarak Tamamlandı ' dir. Dinleyiciyi komut daha sonra yürütmek için bir toplu iş olarak saklayacağından, ancak "komutu normal olarak yürütülen EF görünen". Toplu işleme uygulamak için bu birden fazla gerektiriyor, ancak durdurma sonucu değiştirme nasıl kullanılabileceğine ilişkin bir örnek budur unutmayın.  

Yürütme ayrıca gizlenebilir birinde özel durum özelliğini ayarlayarak... Yürütme yöntemleri. Bu, EF yürütme işlemi, belirtilen özel durum tarafından doğrulayamadı gibi devam etmesine neden olur. Bu, uygulamanın çökmesine neden neden olabilir ancak geçici bir özel durum veya EF tarafından işlenen bazı bir durum olabilir. Örneğin, bu test ortamlarında komut yürütme başarısız olduğunda uygulamanın davranışını test etmek için kullanılabilir.  

#### <a name="changing-the-result-after-execution"></a>Yürütmeden sonra sonucu değiştirme  

Komut yürütüldükten sonra bir dinleyiciyi sonucu özelliği ayarlar varsa (birinde... Yöntemleri yürütülür) EF işlemi gerçekte döndürülen sonuç yerine değişen sonucu kullanır. Komut yürütüldükten sonra özel durum özelliği bir dinleyiciyi ayarlar, işlemi özel durum gibi benzer şekilde, ardından EF kümesi özel durum oluşturur.  

Bir dinleyiciyi, ayrıca özel durum özelliği hiçbir özel durum olduğunu göstermek için null olarak ayarlayabilirsiniz. Yürütme işlemi başarısız oldu, ancak EF işlemi başarılı olursa devam etmek için dinleyiciyi istediği, bu yararlı olabilir. Bu genellikle ayrıca EF devam olarak çalışmak için bazı sonuç değeri olan sonuç ayarını içerir.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult ve özgün özel durum  

EF bir işlemi yürütüldükten sonra yürütme bir özel durumla başarısız olursa, yürütme başarısız olmadı, sonuç ve OriginalResult özellikleri ya da özel durum ve özgün özel durum özelliklerini ayarlar.  

OriginalResult ve özgün özel durum özellikleri salt okunurdur ve yalnızca EF tarafından bir işlemi yürütüldükten sonra ayarlanır. Bu özellikler dinleyicileri tarafından ayarlanamaz. Başka bir deyişle, dinleyiciyi herhangi bir özel durum veya diğer bazı dinleyiciyi gerçek özel durumun aksine tarafından ayarlanan sonuç ya da işlemi çalıştırıldığında oluşan sonuç arasında ayırt edebilir.  

### <a name="registering-interceptors"></a>Kesiciler kaydediliyor  

Bir veya daha fazla durdurma arabirimleri uygulayan bir sınıf oluşturulduktan sonra DbInterception sınıfını kullanarak EF ile kaydedilebilir. Örneğin:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Kesiciler, DbConfiguration kod tabanlı yapılandırma mekanizmasını kullanma uygulama etki alanı düzeyinde de kaydedilebilir.  

### <a name="example-logging-to-nlog"></a>Örnek: NLog için günlüğe kaydetme  

Şimdi tüm bunları bir örnek kullanarak, bir araya IDbCommandInterceptor ve [NLog](http://nlog-project.org/) için:  

- Günlüğe bir uyarı olmayan-zaman uyumsuz olarak yürütülür herhangi bir komutun  
- Yürütüldüğünde oluşturan herhangi bir komut için hata günlüğü  

Yukarıda da gösterildiği gibi kaydedilmelidir günlük kaydı yapar sınıfı şöyledir:  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

Bir komut uyumsuz olmayan yürütülürken bulmak ve bir komut yürütülürken bir hata olduğunda bulmak için bu kodu durdurma bağlam nasıl kullandığına dikkat edin.  
