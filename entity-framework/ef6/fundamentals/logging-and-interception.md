---
title: Günlüğe kaydetme ve veritabanı işlemlerini kesintiye alma-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 35b0284a5ad8b2b732f074589bd458d243312575
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181671"
---
# <a name="logging-and-intercepting-database-operations"></a>Veritabanı işlemlerini günlüğe kaydetme ve önleme
> [!NOTE]
> **Yalnızca EF6** , bu sayfada açıklanan özellikler, API 'ler, vb. Entity Framework 6 ' da sunulmuştur. Önceki bir sürümü kullanıyorsanız, bilgilerin bazıları veya tümü uygulanmaz.  

Entity Framework 6 ' dan başlayarak, her zaman Entity Framework veritabanına bir komut gönderir bu komut uygulama kodu tarafından yakalanabilir. Bu, yaygın olarak SQL günlüğü için kullanılır, ancak komutu değiştirmek veya durdurmak için de kullanılabilir.  

Özel olarak, EF şunları içerir:  

- LINQ to SQL DataContext. log dosyasına benzer bağlam için bir günlük özelliği  
- Günlüğe gönderilen çıktının içeriğini ve biçimlendirmesini özelleştirmek için bir mekanizma  
- Daha fazla denetim/esneklik sağlayan, ele geçirme için düşük düzey yapı taşları  

## <a name="context-log-property"></a>Bağlam günlüğü özelliği  

DbContext. Database. Log özelliği, bir dize alan herhangi bir yöntem için bir temsilciye ayarlanabilir. En yaygın olarak, bu TextWriter 'ın "yazma" yöntemine ayarlanarak herhangi bir TextWriter ile birlikte kullanılır. Geçerli bağlam tarafından oluşturulan tüm SQL bu yazıcıya kaydedilir. Örneğin, aşağıdaki kod SQL 'i konsola kaydeder:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Bağlam olduğuna dikkat edin. Database. log, Console. Write olarak ayarlanır. Bu, SQL 'i konsolunda günlüğe kaydetmek için gereklidir.  

Bazı çıktıları görebilmemiz için bazı basit sorgu/ekleme/güncelleştirme kodu ekleyelim:  

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

Bu, aşağıdaki çıktıyı oluşturur:  

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

(Bu, herhangi bir veritabanı başlatmasının zaten gerçekleştiğini kabul eden çıkış olduğunu unutmayın. Veritabanı başlatma işlemi henüz gerçekleşmiyorsa, tüm çalışma geçişlerinin, yeni bir veritabanını denetlemeye veya oluşturmaya yönelik kapakların altında olduğunu gösteren çok daha fazla çıkış olacaktır.)  

## <a name="what-gets-logged"></a>Ne günlüğe kaydedilir?  

Log özelliği ayarlandığında, aşağıdakilerin tümü günlüğe kaydedilir:  

- Tüm farklı komut türleri için SQL. Örneğin:  
    - Normal LINQ sorguları, eSQL sorguları ve SqlQuery gibi yöntemlerden ham sorgular da dahil olmak üzere sorgular  
    - SaveChanges 'un bir parçası olarak oluşturulan ekler, güncelleştirmeler ve siler  
    - Geç yükleme tarafından oluşturulanlar gibi ilişki yükleme sorguları  
- Parametreler  
- Komutun zaman uyumsuz olarak yürütülüp yürütülmediğini belirtir  
- Komutun ne zaman yürütülmeye başlatıldığını belirten bir zaman damgası  
- Komutun başarıyla tamamlanıp tamamlanmayacağı, bir özel durum oluşturan veya zaman uyumsuz olarak iptal edildiği  
- Sonuç değerinin bir göstergesi  
- Komutu yürütmek için geçen sürenin yaklaşık miktarı. Bu, sonuç nesnesini geri almak için komutunu göndermenin zamanı olduğunu unutmayın. Sonuçların okunması zaman içermez.  

Yukarıdaki örnek çıktıya bakarak günlüğe yazılan dört komutun her biri şu şekilde yapılır:  

- Sorgu bağlama çağrısının sonucu. Ilk blog.  
    - "First", ToString 'in çağrılabilecek bir IQueryable sağlamamasından bu yana, SQL 'i alma ToString yönteminin bu sorgu için çalışmadığına dikkat edin  
- Sorgu, blogun geç yükleme işleminden elde edilir. Nakil  
    - Yavaş yüklemenin gerçekleştiği anahtar değer için parametre ayrıntılarına dikkat edin  
    - Yalnızca varsayılan olmayan değerlere ayarlanan parametrelerin özellikleri günlüğe kaydedilir. Örneğin, size özelliği yalnızca sıfır dışında bir değer olarak gösterilir.  
- SaveChangesAsync 'den kaynaklanan iki komut Bunlardan biri, bir posta başlığını değiştirmek için bir INSERT, diğeri yeni gönderi eklemek için  
    - FK ve title özelliklerine ilişkin parametre ayrıntılarına dikkat edin  
    - Bu komutların zaman uyumsuz olarak yürütülebileceğini unutmayın  

## <a name="logging-to-different-places"></a>Farklı yerlere kaydetme  

Yukarıdaki konsolda gösterildiği gibi, daha kolay bir işlemdir. Ayrıca, farklı tür [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)kullanarak bellek, dosya vb. günlük kaydı da kolaydır.  

LINQ to SQL biliyorsanız, log özelliğinin gerçek TextWriter nesnesine (örneğin, Console. out) ayarlandığını, EF 'teki log özelliğinin ise bir dizeyi kabul eden bir yönteme ayarlandığını LINQ to SQL fark edebilirsiniz (örneğin, , Console. Write veya Console. out. Write). Bunun nedeni, dizeler için havuz görevi gören herhangi bir temsilciyi kabul ederek TextWriter 'dan EF 'e ayrılmalıdır. Örneğin, bir günlük çerçevesini zaten kullandığınızı ve bunun gibi bir günlüğe kaydetme yöntemi tanımladığınızı düşünün:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Bu, EF Log özelliğine şu şekilde erişilebilir:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

## <a name="result-logging"></a>Sonuç günlüğü  

Varsayılan günlükçü komut metnini (SQL), parametreleri ve "yürütülüyor" satırını komut veritabanına gönderilmeden önce bir zaman damgasıyla günlüğe kaydeder. Geçen süreyi içeren "tamamlandı" satırı komutun yürütülmesi için günlüğe kaydedilir.  

Zaman uyumsuz komutlar için "tamamlandı" satırının, zaman uyumsuz görev gerçekten tamamlanana, başarısız olmasına veya iptal edilene kadar günlüğe kaydedilmeyeceğini unutmayın.  

"Tamamlandı" satırı, komutun türüne ve yürütmenin başarılı olup olmadığına bağlı olarak farklı bilgiler içerir.  

### <a name="successful-execution"></a>Başarılı yürütme  

Başarılı bir şekilde tamamlanan komutlar için, çıkış "x MS 'de şu sonuçla tamamlandı:" sözcüğünün ardından sonucun ne olduğuna ilişkin bir bildirim gelir. Bir veri okuyucusu döndüren komutlar için sonuç göstergesi döndürülen [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) türüdür. Gösterilen sonucun yukarıda gösterilen Update komutu gibi bir tamsayı değer döndüren komutlar için bu tamsayıdır.  

### <a name="failed-execution"></a>Yürütme başarısız oldu  

Özel durum oluşturan komutlar için, çıkış özel durumdan gelen iletiyi içerir. Örneğin, var olan bir tabloya karşı sorgulamak için SqlQuery kullanmak şuna benzer bir şekilde günlük çıkışı oluşmasına neden olur:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

### <a name="canceled-execution"></a>Yürütme iptal edildi  

Görevin iptal edildiği zaman uyumsuz komutlar için, bu, temel alınan ADO.NET sağlayıcının genellikle iptal için bir deneme yapıldığında yaptığı durumlar olduğundan, bir özel durumla başarısız olabilir. Bu durum gerçekleşmezse ve görev düzgün şekilde iptal edilirse, çıkış şuna benzer şekilde görünür:  

```console
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Günlük içeriğini ve biçimlendirmeyi değiştirme  

Öğesinin altında Database. Log özelliği bir DatabaseLogFormatter nesnesinin kullanımını sağlar. Bu nesne, dize ve DbContext kabul eden bir temsilciye bir ıdbcommandyakalayıcısı uygulamasını (aşağıya bakın) etkin bir şekilde bağlar. Bu, DatabaseLogFormatter üzerindeki yöntemlerin, komutların EF tarafından yürütülmesinden önce ve sonra çağrıldığı anlamına gelir. Bu DatabaseLogFormatter yöntemleri, günlük çıktısını toplayıp biçimlendirir ve temsilciye gönderir.  

### <a name="customizing-databaselogformatter"></a>DatabaseLogFormatter 'ı özelleştirme  

Günlüğe kaydedilen ve nasıl biçimlendirildiğine yönelik değişiklik, DatabaseLogFormatter 'dan türeyen yeni bir sınıf oluşturularak ve uygun şekilde Yöntemler geçersiz kılınarak elde edilebilir. Geçersiz kılınacak en yaygın yöntemler şunlardır:  

- LogCommand: komutları yürütülmeden önce günlüğe kaydetme şeklini değiştirmek için bunu geçersiz kılın. Varsayılan olarak, LogCommand her parametre için LogParameter çağırır; Bunun yerine, geçersiz kılmanızda veya parametreleri farklı şekilde işleyebilir.  
- LogResult: bir komutu yürütme sonucunun günlüğe nasıl kaydedileceğini değiştirmek için bunu geçersiz kılın.  
- LogParameter: parametre günlüğünün biçimlendirmesini ve içeriğini değiştirmek için bunu geçersiz kılın.  

Örneğin, her komut veritabanına gönderilmeden önce yalnızca tek bir satırı günlüğe kaydetmek istediğinizi varsayalım. Bu, iki geçersiz kılma ile yapılabilir:  

- Tek SQL satırını biçimlendirmek ve yazmak için LogCommand 'i geçersiz kıl  
- Hiçbir şey yapmak için LogResult 'ı geçersiz kılın.  

Kod şuna benzer:

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

Çıktıyı günlüğe kaydetmek için, yapılandırılan yazma temsilcisine çıktı gönderecek olan Write metodunu çağırın.  

(Bu kodun bir örnek olarak satır sonlarının uyarlaması kaldırdığına göz önünde kısınız. Büyük olasılıkla karmaşık SQL görüntülemek için iyi çalışmaz.)  

### <a name="setting-the-databaselogformatter"></a>DatabaseLogFormatter ayarlanıyor  

Yeni bir DatabaseLogFormatter sınıfı oluşturulduktan sonra, EF ile kaydedilmesi gerekir. Bu, kod tabanlı yapılandırma kullanılarak yapılır. Bir kısaca 'de bu, DbContext sınıfınız ile aynı derlemede bulunan DBConfiguration 'dan türetilmiş yeni bir sınıf oluşturma ve ardından bu yeni sınıfın oluşturucusunda setdatabaselogformatter çağırma anlamına gelir. Örneğin:  

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

### <a name="using-the-new-databaselogformatter"></a>Yeni DatabaseLogFormatter 'ı kullanma  

Bu yeni DatabaseLogFormatter artık her zaman veritabanı için kullanılacaktır. log ayarlandı. Bu nedenle, kod 1 ' den kodu çalıştırmak şu çıkışa neden olur:  

```console
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Yakalaşmayı oluşturma blokları  

Şu ana kadar, EF tarafından oluşturulan SQL 'i günlüğe kaydetmek için DbContext. Database. log ' un nasıl kullanılacağını inceledik. Ancak bu kod, daha genel bir ele alma için bazı düşük düzeyli yapı taşları üzerinde görece ince façlşdir.  

### <a name="interception-interfaces"></a>Araya geçirme arabirimleri  

Yakasyon kodu, ele geçirme arabirimleri kavramı etrafında oluşturulmuştur. Bu arabirimler, ıdbyakalayıcıyı sınıfından devralınır ve EF bir işlem gerçekleştirdiğinde çağrılan yöntemleri tanımlar. Amaç, her nesne türü için bir arabirime sahip olmak için kullanılır. Örneğin, ıdbcommandyakalayıcısı arabirimi, bir ExecuteNonQuery, ExecuteReader metodunu, ExecuteReader ve ilgili yöntemlere çağrı yapmadan önce çağrılan yöntemleri tanımlar. Benzer şekilde, arabirim, bu işlemlerin her biri tamamlandığında çağrılan yöntemleri tanımlar. Yukarıda incelediğimiz DatabaseLogFormatter sınıfı bu arabirimi günlüğe kaydetmek için uygular.  

### <a name="the-interception-context"></a>Yakalenme bağlamı  

Herhangi bir dinleyici için tanımlanan yöntemlere bakarak, her çağrıya Dbyakationcontext türünde bir nesne veya Dbcommandyakationcontext\<\>gibi bazı tür bir nesne verilmediği görünür. Bu nesne, EF 'in aldığı eylem hakkında bağlamsal bilgiler içerir. Örneğin, eylem bir DbContext adına götürülüiyorsa DbContext Dbyakationcontext içine dahil edilir. Benzer şekilde, zaman uyumsuz olarak yürütülen komutlar için, Dbcommandyakationcontext üzerinde IsAsync bayrağı ayarlanır.  

### <a name="result-handling"></a>Sonuç işleme  

Dbcommandyakationcontext\<\> sınıfı result, OriginalResult, Exception ve OriginalException adlı bir özellik içerir. Bu özellikler, işlem yürütülmeden önce çağrılan çağrı yöntemlerine yapılan çağrılar için null/sıfır olarak ayarlanır; Yani,.................. Yöntemler yürütülüyor. İşlem yürütülürse ve başarılı olursa Result ve OriginalResult işlemin sonucuna ayarlanır. Bu değerler daha sonra, işlem yürütüldükten sonra çağrılan (...... Çalıştırılan Yöntemler. Benzer şekilde, işlem oluşturursa, Exception ve OriginalException özellikleri ayarlanır.  

#### <a name="suppressing-execution"></a>Yürütmeyi gizleme  

Bir dinleyici, komut yürütülmeden önce sonuç özelliğini ayarlarsa (bir... Yöntemler yürütülüyor), AŞV gerçekten komutu yürütmeye çalışmaz, ancak bunun yerine yalnızca sonuç kümesini kullanacaktır. Diğer bir deyişle, yakalayıcısı komutun yürütülmesini engelleyebilir, ancak komutun yürütüldüğünden EF ile devam eder.  

Bunun nasıl kullanılacağına ilişkin bir örnek, bir sarmalama sağlayıcısıyla geleneksel olarak gerçekleştirilen komut toplu işi örneğidir. Yakalayıcısı, sonraki yürütme için komutu bir toplu iş olarak depolar, ancak komutun normal olarak yürütüldüğünü, "pretend" olması gerekir. Toplu işlem uygulamak için bundan daha fazlasını gerektirdiğini unutmayın, ancak bu durum, ayırma sonucunun nasıl kullanıldığına ilişkin bir örnektir.  

Yürütme aynı zamanda özel durum özelliği bir... ile ayarlanarak de gizlenebilir. Yöntemler yürütülüyor. Bu, işlemin yürütülmesi verilen özel durumu oluşturarak başarısız olmaya devam etmesine neden olur. Bu, kuşkusuz uygulamanın kilitlenmesine neden olabilir, ancak başka bir özel durum veya EF tarafından işlenen diğer özel durumlar da olabilir. Örneğin, bu, komut yürütme başarısız olduğunda bir uygulamanın davranışını test etmek için test ortamlarında kullanılabilir.  

#### <a name="changing-the-result-after-execution"></a>Yürütmeden sonra sonucu değiştirme  

Bir dinleyici, komut yürütüldükten sonra Result özelliğini ayarlarsa (bir... Yürütülen Yöntemler) ardından EF, işlemden aslında döndürülen sonuç yerine değiştirilen sonucu kullanır. Benzer şekilde, bir dinleyici, komut yürütüldükten sonra özel durum özelliğini ayarlarsa EF, işlem özel durumu oluşturdu gibi set özel durumunu oluşturur.  

Bir dinleyici, özel durum oluşturulması gerektiğini belirtmek için özel durum özelliğini null olarak da ayarlayabilir. Bu işlem, işlemin yürütülmesi başarısız olursa, ancak bu işlem başarılı olmuş gibi devam etmek için bu yararlı olabilir. Bu genellikle, sonucun devam ettiği için birlikte çalışmak üzere bir sonuç değeri olması için sonucu ayarlamayı da içerir.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult ve OriginalException  

EF bir işlem çalıştırdıktan sonra, yürütme başarısız olduysa Result ve OriginalResult özelliklerini ayarlar ve yürütme bir özel durumla başarısız olursa, Exception ve OriginalException özellikleri ayarlanır.  

OriginalResult ve OriginalException özellikleri salt okunurdur ve yalnızca bir işlem yürütüldükten sonra EF tarafından ayarlanır. Bu özellikler, yakalayıcılar tarafından ayarlanamaz. Bu, herhangi bir yakacının, bir özel durumun veya işlem yürütüldüğünde oluşan sonucun aksine, başka bir dinleyici tarafından ayarlanan bir özel durum ya da sonuç arasında ayrım yaptığı anlamına gelir.  

### <a name="registering-interceptors"></a>Kayıt yaptırıcılar  

Bir veya daha fazla savunma arabirimini uygulayan bir sınıf oluşturulduktan sonra, Dbyakaısyon sınıfı kullanılarak EF ile kaydedilebilir. Örneğin:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Ayrıca, uygulama etki alanı düzeyinde DbConfiguration kod tabanlı yapılandırma mekanizması kullanılarak da kayıt yapılabilir.  

### <a name="example-logging-to-nlog"></a>Örnek: NLog günlüğüne kaydetme  

Bunu, ıdbcommandyakalayıcısı ve [NLog](https://nlog-project.org/) ' un kullanıldığı bir örneğe bir araya koyalım:  

- Zaman uyumsuz olarak yürütülen her komut için bir uyarı Kaydet  
- Yürütüldüğünde oluşturan herhangi bir komut için bir hata günlüğe kaydet  

Aşağıda gösterildiği gibi kaydedilmesi gereken günlüğe kaydetme sınıfı aşağıda verilmiştir:  

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

Bu kodun, zaman uyumsuz olmayan bir komutun yürütüldüğü ve bir komutun yürütüldüğü bir hata olduğunda keşfedildiğinde bulması için yakalanmış bağlamını nasıl kullandığını fark edin.  
