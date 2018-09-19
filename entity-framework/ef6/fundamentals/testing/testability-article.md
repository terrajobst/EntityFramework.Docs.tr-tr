---
title: Test Edilebilirlik ve Entity Framework 4.0
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: aec177438004fd255bef85a5e5047cf6b5a6f782
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284050"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="723ff-102">Test Edilebilirlik ve Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="723ff-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="723ff-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="723ff-103">Scott Allen</span></span>

<span data-ttu-id="723ff-104">Yayımlanma: Mayıs 2010</span><span class="sxs-lookup"><span data-stu-id="723ff-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="723ff-105">Giriş</span><span class="sxs-lookup"><span data-stu-id="723ff-105">Introduction</span></span>

<span data-ttu-id="723ff-106">Bu teknik incelemede, açıklamakta ve ADO.NET Entity Framework 4.0 ve Visual Studio 2010 test edilebilir kodunun nasıl yazılacağını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="723ff-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="723ff-107">Bu yazıda, test Odaklı Tasarım (TDD) ya da davranışı Odaklı Tasarım (BDD) gibi belirli bir test yöntemi odaklanmak denemez.</span><span class="sxs-lookup"><span data-stu-id="723ff-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="723ff-108">Bunun yerine bu yazıda, ADO.NET varlık çerçevesi kullanır ancak kolay yalıtmak ve otomatik bir şekilde test kalır kodunun nasıl yazılacağını üzerinde odaklanır.</span><span class="sxs-lookup"><span data-stu-id="723ff-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="723ff-109">Veri erişimi senaryoları test kolaylaştırmak ve framework kullanılırken bu desenleri uygulamak nasıl sık karşılaşılan tasarım desenleri inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="723ff-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="723ff-110">Ayrıca bu özellikleri test edilebilir kod içinde nasıl çalışabileceğini görmek için framework'ün belirli özellikleri inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="723ff-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="723ff-111">Test edilebilir kod nedir?</span><span class="sxs-lookup"><span data-stu-id="723ff-111">What Is Testable Code?</span></span>

<span data-ttu-id="723ff-112">Otomatik birim testleriyle bir yazılım parçasıdır doğrulama yeteneği arzu birçok avantaj sunar.</span><span class="sxs-lookup"><span data-stu-id="723ff-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="723ff-113">Herkesin iyi testler yazılım kusurlarını bir uygulama ve uygulamanın kalite - birim testleri sahip olmanın ancak şu ana kadar yalnızca hataları bulmaya ötesinde gider artış sayısını azaltacak bilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="723ff-114">Zamandan tasarruf edin ve oluşturdukları yazılım denetiminde kalmak bir geliştirme ekibi iyi birim test paketi sağlar.</span><span class="sxs-lookup"><span data-stu-id="723ff-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="723ff-115">Takım değişiklik yapabilmeniz için var olan kod, yeniden düzenleme, tasarlanması ve yeni gereksinimleri karşılaması ve tüm bunları yaparken test paketi bilmek bir uygulamaya yeni bir bileşen eklemek için yeniden yapılandırmaya yazılım uygulamanın davranışı doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="723ff-116">Birim testleri değişimi kolaylaştırın ve yazılım bakımı karmaşıklığı arttıkça korumak için bir hızlı geri bildirim döngüsü bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="723ff-117">Birim testi, ancak bir fiyat ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="723ff-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="723ff-118">Takım, birim testleri oluşturmak ve sürdürmek için zaman ayırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="723ff-119">Bu testleri oluşturmak için gereken çaba miktarı doğrudan ilişkili **Test Edilebilirlik** temel alınan yazılım.</span><span class="sxs-lookup"><span data-stu-id="723ff-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="723ff-120">Test etmek için yazılımın ne kadar kolay olduğunu?</span><span class="sxs-lookup"><span data-stu-id="723ff-120">How easy is the software to test?</span></span> <span data-ttu-id="723ff-121">Test Edilebilirlik aklınızda ile yazılım tasarlamanın takım geçerlilik testleri beklemediğiniz test edilebilir yazılımla birlikte çalışan ekip daha hızlı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="723ff-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="723ff-122">ADO.NET Entity Framework 4.0 (EF4) Microsoft Test Edilebilirlik düşünülerek tasarlanan.</span><span class="sxs-lookup"><span data-stu-id="723ff-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="723ff-123">Bu, geliştiricilerin framework kodunun kendisi karşı birim testleri yazma gelmez.</span><span class="sxs-lookup"><span data-stu-id="723ff-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="723ff-124">Bunun yerine, EF4 için Test Edilebilirlik hedefleri üzerinde framework derlemeleri test edilebilir kod oluşturmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="723ff-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="723ff-125">Biz belirli örneklere göz atacağız önce test edilebilir kod kalitelerini anlamak için faydalı vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="723ff-126">Test edilebilir kod kalitelerini</span><span class="sxs-lookup"><span data-stu-id="723ff-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="723ff-127">Test etmek kolayca kod her zaman en az iki nitelikler sergiler.</span><span class="sxs-lookup"><span data-stu-id="723ff-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="723ff-128">İlk test edilebilir kod kolay **gözlemleyin**.</span><span class="sxs-lookup"><span data-stu-id="723ff-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="723ff-129">Bazı girişler kümesini göz önünde bulundurulduğunda, kodun çıktısı gözlemlemek kolay olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="723ff-130">Yöntem doğrudan bir hesaplamanın sonucu döndürdüğünden Örneğin, aşağıdaki yöntemi test kolaydır.</span><span class="sxs-lookup"><span data-stu-id="723ff-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="723ff-131">Bir yöntem test yöntemi, bir ağ yuva, bir veritabanı tablosu veya dosyası aşağıdaki kod gibi hesaplanan değer yazıyorsa zordur.</span><span class="sxs-lookup"><span data-stu-id="723ff-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="723ff-132">Değerini almak için ek çalışma gerçekleştirmek test vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="723ff-133">İkincisi, test edilebilir kod kolay **yalıtmak**.</span><span class="sxs-lookup"><span data-stu-id="723ff-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="723ff-134">Bir hatalı test edilebilir kod örneği olarak aşağıdaki sözde kod kullanalım.</span><span class="sxs-lookup"><span data-stu-id="723ff-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

<span data-ttu-id="723ff-135">Gözlemlemek kolay bir yöntemdir: biz bir sigorta ilke geçirin ve dönüş değeri, beklenen sonuç eşleşen doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="723ff-136">Ancak, test yöntemi için size bir veritabanı doğru şema ile yüklü olması ve bir e-posta göndermek yöntemin çalışır durumda SMTP sunucusunu yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="723ff-137">Birim testi yalnızca yöntemi içindeki hesaplama mantığı olun ister, ancak test e-posta sunucusu çevrimdışı olduğundan veya veritabanı sunucusu taşıdığınız için başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="723ff-138">Bu hatalar her ikisi de doğrulamak için test istediği davranışı ilgili değildir.</span><span class="sxs-lookup"><span data-stu-id="723ff-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="723ff-139">Davranış izole etmek zordur.</span><span class="sxs-lookup"><span data-stu-id="723ff-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="723ff-140">Genellikle, test edilebilir kod yazmak için çaba yazılım geliştiriciler, bir görev ayrımı nettir yazmadan kodda korumak çaba harcar.</span><span class="sxs-lookup"><span data-stu-id="723ff-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="723ff-141">Yukarıdaki yöntemi, iş hesaplamaları odaklanın ve veritabanı ve e-posta uygulama ayrıntıları diğer bileşenler için temsilci.</span><span class="sxs-lookup"><span data-stu-id="723ff-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="723ff-142">Robert c Martin bu tek sorumluluk ilkesini çağırır.</span><span class="sxs-lookup"><span data-stu-id="723ff-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="723ff-143">Bir nesne bir ilke değeri hesaplama gibi tek, dar sorumluluk kapsüllemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="723ff-144">Diğer tüm veritabanını ve bildirim iş başka bir nesnenin sorumluluğunda olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="723ff-145">Bu şekilde yazılmış kodu tek bir görev üzerinde odaklanmıştır yalıtılması daha kolay olur.</span><span class="sxs-lookup"><span data-stu-id="723ff-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="723ff-146">. NET'te tek sorumluluk ilkesini izleyin ve ayırma sağlamak için ihtiyacımız soyutlama sahibiz.</span><span class="sxs-lookup"><span data-stu-id="723ff-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="723ff-147">Arabirim tanımları kullanın ve yerine somut bir türde arabirimi soyutlama kullanmak için kodu zorla.</span><span class="sxs-lookup"><span data-stu-id="723ff-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="723ff-148">Bu belgede daha sonra nasıl yukarıda verilen hatalı örneği çalışabilirsiniz gibi bir yöntem, arabirimleri görüyoruz *Ara* gibi bunlar veritabanına Bahsediyor.</span><span class="sxs-lookup"><span data-stu-id="723ff-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="723ff-149">Test zaman, ancak biz veritabanına konuşuruz değil ancak bunun yerine verileri bellek içinde tutar işlevsiz bir uygulama yerine kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="723ff-150">İşlevsiz bu uygulama, veri erişim kodu veya veritabanı yapılandırması ilgisiz sorunlar koddan yalıtmak.</span><span class="sxs-lookup"><span data-stu-id="723ff-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="723ff-151">Yalıtım için başka avantajları vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="723ff-152">Son yöntemi iş hesaplamaya yalnızca yürütmek için birkaç milisaniye sürer, ancak test çeşitli sunucular için birkaç saniye koda atlama konuşmalar ve ağ geçici olarak çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="723ff-153">Küçük değişikliklere hızlı kolaylaştırmak için birim testleri çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="723ff-154">Birim testleri, ayrıca tekrarlanabilir ve teste ilgisi olmayan bir bileşen bir sorun olduğu için başarısız değil.</span><span class="sxs-lookup"><span data-stu-id="723ff-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="723ff-155">Yazma inceleyin ve yalıtmak için kolayca kod geliştiricilerin kod için testleri yazmak daha kolay bir zaman gerekeceği anlamına gelir, yürütülecek testleri için daha az süre beklemesini ve daha da önemlisi, var olmayan hataları izleme daha az zaman harcama.</span><span class="sxs-lookup"><span data-stu-id="723ff-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="723ff-156">Umarım test avantajları için teşekkür ederiz ve test edilebilir kod sergiler kalitelerini anlayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="723ff-157">Biz hakkında EF4 gözlemlenebilir ve yalıtmak kolay kalan çalışırken verileri bir veritabanına kaydetmek için çalışan bir kodun nasıl yazılacağını ele almak için ancak ilk biz veri erişimi için test edilebilir tasarımlar tartışmak için sunduğumuz odağını daraltmak.</span><span class="sxs-lookup"><span data-stu-id="723ff-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="723ff-158">Veri kalıcılığı için Tasarım desenleri</span><span class="sxs-lookup"><span data-stu-id="723ff-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="723ff-159">Daha önce sunulan hatalı örneklerin her ikisi de çok fazla sorumlulukları vardı.</span><span class="sxs-lookup"><span data-stu-id="723ff-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="723ff-160">İlk hatalı örnek bir hesaplama gerçekleştirmek zorundaydınız *ve* bir dosyaya yazma.</span><span class="sxs-lookup"><span data-stu-id="723ff-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="723ff-161">Hatalı ikinci örnek, bir veritabanından verileri okuyamadı gerekiyordu *ve* iş hesaplamayı *ve* e-posta gönderin.</span><span class="sxs-lookup"><span data-stu-id="723ff-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="723ff-162">Konuları ayrı ve diğer bileşenler için sorumluluk daha küçük yöntemleri tasarlayarak test edilebilir kod yazmaya yönelik harika strides yapacaksınız.</span><span class="sxs-lookup"><span data-stu-id="723ff-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="723ff-163">Hedef, küçük ve odaklı soyutlama eylemlerden bir araya getirerek işlevselliği oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="723ff-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="723ff-164">Küçük veri kalıcılığı gelir ve arıyoruz için odaklanmış soyutlama kadar sık olduğunda, tasarım desenleri belgelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="723ff-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="723ff-165">Kurumsal uygulama mimarisi desenleri Martin Fowler'ın kitap yazdırma bu düzenleri tanımlamak için ilk iş oluştu.</span><span class="sxs-lookup"><span data-stu-id="723ff-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="723ff-166">Nasıl bu ADO.NET varlık çerçevesi uygular ve bu modellerle çalıştığını göstereceğiz önce aşağıdaki bölümlerde bu desenleri kısa bir açıklama sağlarız.</span><span class="sxs-lookup"><span data-stu-id="723ff-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="723ff-167">Depo düzeni</span><span class="sxs-lookup"><span data-stu-id="723ff-167">The Repository Pattern</span></span>

<span data-ttu-id="723ff-168">Fowler bir depo "etki alanı ve veri arasında eşleme katmanları etki alanı nesnelerini erişmek için bir koleksiyon benzeri arabirimi kullanarak aracılık" diyor.</span><span class="sxs-lookup"><span data-stu-id="723ff-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="723ff-169">Hedef depo düzeninin aðaçlarýn veri erişim kodu ayırmanızı ve önceki yalıtım gördüğümüz gibi test edilebilirliğe yönelik gerekli bir nitelik.</span><span class="sxs-lookup"><span data-stu-id="723ff-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="723ff-170">Yalıtım için depo nesneleri bir koleksiyon benzeri arabirimi kullanarak nasıl gösterir anahtardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="723ff-171">Depo hiçbir fikriniz nasıl sahip kullanmak için yazma mantıksal depo, rica nesnelerini gerçekleştirmek.</span><span class="sxs-lookup"><span data-stu-id="723ff-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="723ff-172">Depo bir veritabanına konuşmak veya yalnızca bir bellek içi koleksiyonundan nesneleri döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="723ff-173">Kodunuzu bilmesi gereken şey depo koleksiyonu devam ettirirler görünüyor, ve alma, ekleme ve nesneleri koleksiyonundan silme.</span><span class="sxs-lookup"><span data-stu-id="723ff-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="723ff-174">Mevcut .NET uygulamalarında somut bir depo genellikle aşağıdaki gibi bir genel arabirimi devralır:</span><span class="sxs-lookup"><span data-stu-id="723ff-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="723ff-175">Bir uygulama için EF4 sağlıyoruz, ancak temel konseptini aynı kalır, arabirim tanımı için birkaç değişiklik oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="723ff-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="723ff-176">Kod, bir koşul değerlendirmeye göre varlıklar koleksiyonu almak için birincil anahtar değerine göre varlık almak için bu arabirimi uygulayan somut bir depo kullanın veya yalnızca tüm kullanılabilir varlıkları alma.</span><span class="sxs-lookup"><span data-stu-id="723ff-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="723ff-177">Ayrıca kod ekleyebilir ve depo arabirimi aracılığıyla varlıkları kaldırın.</span><span class="sxs-lookup"><span data-stu-id="723ff-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="723ff-178">IRepository, çalışan nesneleri göz önünde bulundurulduğunda, kod aşağıdaki işlemleri gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="723ff-179">Kod bir arabirim (çalışan IRepository) kullandığından, kod ile arabirimin farklı uygulamaları sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="723ff-180">Bir uygulama, Microsoft SQL Server veritabanına EF4 ve kalıcı nesneler tarafından desteklenen bir uygulama olabilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="723ff-181">Farklı bir uygulama (bir test sırasında kullanın), bir bellek içi biri listeyi çalışan nesneler tarafından desteklenen.</span><span class="sxs-lookup"><span data-stu-id="723ff-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="723ff-182">Arabirimi, kodda ayırma sağlamak için yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="723ff-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="723ff-183">IRepository fark&lt;T&gt; arabirimi kaydetme işlemi koymuyor.</span><span class="sxs-lookup"><span data-stu-id="723ff-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="723ff-184">Biz varolan nesneleri nasıl güncelleştiririm?</span><span class="sxs-lookup"><span data-stu-id="723ff-184">How do we update existing objects?</span></span> <span data-ttu-id="723ff-185">Kaydetme işlemi içeren IRepository tanımlarında gelebilir ve bu depolar, uygulamalarını hemen veritabanına bir nesneyi kalıcı hale getirmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="723ff-186">Ancak, birçok uygulamada ayrı ayrı nesneleri kalıcı hale getirmek istiyoruz yok.</span><span class="sxs-lookup"><span data-stu-id="723ff-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="723ff-187">Bunun yerine, nesneleri hayata, belki de bu nesnelere farklı depolarından iş etkinliği bir parçası olarak değiştirin ve tek, atomik bir işlem kapsamında tüm nesneleri kalıcı hale getirmek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="723ff-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="723ff-188">Neyse ki, bu türden bir davranış izin vermek için bir desen yoktur.</span><span class="sxs-lookup"><span data-stu-id="723ff-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="723ff-189">Çalışma deseni birimi</span><span class="sxs-lookup"><span data-stu-id="723ff-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="723ff-190">Fowler iş birimi "nesnelerin bir listesini bir iş işlem tarafından etkilenen ve değişiklikleri dışında yazma ve eşzamanlılık sorunlarının çözümü koordinatları olarak tutacaktır" diyor.</span><span class="sxs-lookup"><span data-stu-id="723ff-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="723ff-191">Bir depodan hayata ve iş birimini değişiklikleri kaydetmek için biz bahsedin, biz nesnelere yaptığınız tüm değişiklikler kalıcı nesnelere değişiklikleri izlemek için iş birimini sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="723ff-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="723ff-192">Ayrıca yeni nesneler için tüm depolar ekledik ve bir veritabanı ve Yönet silme işlemi aynı zamanda nesneleri eklemek yapılacak iş birimi sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="723ff-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="723ff-193">Ardından ADO.NET veri kümeleri ile hiç olmadığı kadar herhangi bir iş yaptıysanız, zaten çalışma deseni birimle hakkında bilgi sahibi olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="723ff-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="723ff-194">ADO.NET veri kümeleri, bizim güncelleştirme, silme ve ekleme DataRow nesnelerin izlemek için konuklarımızın ve (bir TableAdapter yardımıyla) veritabanına yaptığımız değişiklikleri mutabık kılma.</span><span class="sxs-lookup"><span data-stu-id="723ff-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="723ff-195">Ancak, temel alınan veritabanı bağlantısı kesilmiş bir alt kümesi veri kümesi nesnelerini model.</span><span class="sxs-lookup"><span data-stu-id="723ff-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="723ff-196">Çalışma deseni birimidir, iş nesneleri ve veri erişim kodu yalıtılmış ve veritabanının farkında olan etki alanı nesnelerini çalışır ancak aynı davranışı sergiler.</span><span class="sxs-lookup"><span data-stu-id="723ff-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="723ff-197">.NET kodu iş birimi modellemek için bir Özet aşağıdakine benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="723ff-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="723ff-198">Tek bir iş birimi emin oluruz iş birimini depo başvurularından göstererek nesnenin tüm varlıkların gerçekleştirilmiş bir iş işlemi sırasında izleme olanağı vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="723ff-199">Commit yöntemi gerçek bir iş birimi için burada tüm veritabanı bellek içi değişikliklerle mutabık kılınacak sihrin uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="723ff-200">Kod, IUnitOfWork başvuru göz önünde bulundurulduğunda, bir veya daha fazla depolarından alınan iş nesneler değişiklik ve atomik yürütme işlemini kullanarak tüm değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="723ff-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="723ff-201">Yavaş yük düzeni</span><span class="sxs-lookup"><span data-stu-id="723ff-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="723ff-202">Fowler adı yavaş yük "tüm verileri içermeyen bir nesne gerektiği halde nasıl bilir" tanımlamak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="723ff-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="723ff-203">Saydam yavaş yükleniyor ne zaman sağlamak için önemli bir özelliği olan test edilebilir iş kod yazma ve ilişkisel bir veritabanı ile çalışmaya.</span><span class="sxs-lookup"><span data-stu-id="723ff-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="723ff-204">Örneğin, aşağıdaki kodu düşünün.</span><span class="sxs-lookup"><span data-stu-id="723ff-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="723ff-205">Zaman çizelgeleri koleksiyon nasıl doldurulur?</span><span class="sxs-lookup"><span data-stu-id="723ff-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="723ff-206">İki olası yanıtların vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-206">There are two possible answers.</span></span> <span data-ttu-id="723ff-207">Çalışan havuzu bir çalışan getirilecek sorulduğunda çalışan birlikte çalışan ilgili zaman çizelgesi bilgileri almak için bir sorgu sorunları bir cevaptır.</span><span class="sxs-lookup"><span data-stu-id="723ff-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="723ff-208">Bu genellikle bir sorgu ile bir JOIN yan tümcesi gerektiriyor ve uygulamanın daha fazla bilgi alınırken sonuçlanabilir ilişkisel veritabanları gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="723ff-209">Ne uygulamanın hiçbir zaman zaman çizelgeleri özelliği touch gerekiyor?</span><span class="sxs-lookup"><span data-stu-id="723ff-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="723ff-210">İkinci bir yanıt zaman çizelgeleri özelliği "isteğe bağlı" yüklenmesidir.</span><span class="sxs-lookup"><span data-stu-id="723ff-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="723ff-211">Kod zaman çizelgesi bilgileri almak için özel API'ler çağırma kullanılamaz çünkü örtük ve iş mantığı için saydam bu yavaş yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="723ff-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="723ff-212">Kod, zaman çizelgesi bilgileri gerektiğinde mevcut olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="723ff-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="723ff-213">Genel yöntem çağrılarını çalışma zamanı ele geçirilmesini gerektirir yavaş yükleme ile ilgili bazı Sihirli yoktur.</span><span class="sxs-lookup"><span data-stu-id="723ff-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="723ff-214">Araya giren kodu veritabanına Bahsediyor ve iş mantığı iş mantığı olmasını boş bırakarak zaman çizelgesi bilgilerini almak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="723ff-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="723ff-215">Bu geç yük Sihirli veri alma işlemleri ve daha fazla test edilebilir kod sonuçlarında kendisini yalıtmak için iş kodu sağlar.</span><span class="sxs-lookup"><span data-stu-id="723ff-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="723ff-216">Bir uygulama olan yavaş yük dezavantajı *mu* kodu bir ek sorgu yürütmek zaman çizelgesi bilgileri gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="723ff-217">Bu birçok uygulama için ancak performans duyarlı uygulamalar veya (genellikle N + 1 adlandırılan bir sorun her döngü sırasında zaman çizelgesi alınacak çalışan nesneler bir dizi döngü ve sorguyu yürüten uygulamalar için önemli değil Sorgu sorunu) olan bir sürükleme yavaş yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="723ff-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="723ff-218">Bu senaryolarda bir uygulama zaman çizelgesi bilgileri olası en verimli şekilde eagerly yüklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="723ff-219">Neyse ki, nasıl iki örtük geç yükleri EF4 destekler ve sonraki bölüme taşıyın ve bu desenleri uygulamak verimli eager yükler göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="723ff-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="723ff-220">Entity Framework ile uygulama düzenleri</span><span class="sxs-lookup"><span data-stu-id="723ff-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="723ff-221">Güzel bir haberimiz var tüm son bölümde açıkladığımız tasarım desenleri ile EF4 uygulamak basit olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="723ff-222">Göstermek için basit bir ASP.NET MVC uygulaması düzenleyin ve çalışanlar ve bunların ilişkili zaman çizelgesi bilgileri görüntülemek için kullanılacak kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="723ff-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="723ff-223">Aşağıdaki "düz eski CLR nesneler" kullanarak başlayacağız (POCOs).</span><span class="sxs-lookup"><span data-stu-id="723ff-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

<span data-ttu-id="723ff-224">Bu sınıf tanımları, biraz farklı yaklaşımlara ve EF4 özellikleri inceleyeceğiz, ancak bu sınıfların Kalıcılık ignorant (PI) mümkün olduğunca tutmak hedefi olduğu şekilde değiştirir.</span><span class="sxs-lookup"><span data-stu-id="723ff-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="723ff-225">PI nesne bilmez *nasıl*, hatta *varsa*, tuttuğu durumu bulunduğu bir veritabanı içinde.</span><span class="sxs-lookup"><span data-stu-id="723ff-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="723ff-226">PI ve POCOs test edilebilir yazılım ile bir aradadır.</span><span class="sxs-lookup"><span data-stu-id="723ff-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="723ff-227">POCO yaklaşımı kullanarak nesneleri, daha az kısıtlanmış, daha esnek ve bunlar olmadan mevcut bir veritabanını çalışır çünkü test etmek daha kolay.</span><span class="sxs-lookup"><span data-stu-id="723ff-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="723ff-228">Yerinde POCOs ile Visual Studio'da bir varlık veri modeli (EDM) oluşturabilir (bkz. Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="723ff-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="723ff-229">EDM bizim varlıklar için kod oluşturmak için kullanacağız değil.</span><span class="sxs-lookup"><span data-stu-id="723ff-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="723ff-230">Bunun yerine, biz lovingly el ile oluşturabilir varlıkları kullanmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="723ff-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="723ff-231">Yalnızca EDM bizim veritabanı şeması oluşturmak ve EF4 veritabanına nesneleri eşleştirmek için gereken meta verilerini sağlamak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="723ff-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![EF test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="723ff-233">**Şekil 1**</span><span class="sxs-lookup"><span data-stu-id="723ff-233">**Figure 1**</span></span>

<span data-ttu-id="723ff-234">Not: önce EDM modeli geliştirmek istiyorsanız, temiz, EDM POCO kod oluşturmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="723ff-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="723ff-235">Veri programlama ekibi tarafından sağlanan bir Visual Studio 2010 uzantısı ile bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="723ff-236">Uzantı indirmek için Visual Studio'da Araçlar menüsünden Uzantı Yöneticisi'ni başlatın ve "POCO" (bkz: Şekil 2)'için çevrimiçi Galerisine arayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="723ff-237">EF için kullanılabilen çeşitli POCO şablonlar vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="723ff-238">Şablon kullanma hakkında daha fazla bilgi için bkz. " [izlenecek yol: POCO varlık çerçevesi için şablon](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="723ff-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![EF test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="723ff-240">**Şekil 2**</span><span class="sxs-lookup"><span data-stu-id="723ff-240">**Figure 2**</span></span>

<span data-ttu-id="723ff-241">Başlangıç noktası bu POCO biz test edilebilir kod için iki farklı yaklaşım inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="723ff-242">İlk yaklaşım soyutlama depoları ve iş birimleri uygulamak için Entity Framework API'sinden yararlanır çünkü EF yaklaşım çağırmalıyım.</span><span class="sxs-lookup"><span data-stu-id="723ff-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="723ff-243">İkinci yaklaşımda kendi özel deponuzu soyutlama oluşturur ve sonra olumlu ve olumsuz her yaklaşımın bakın.</span><span class="sxs-lookup"><span data-stu-id="723ff-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="723ff-244">EF yaklaşım inceleyerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="723ff-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="723ff-245">EF merkezli uygulaması</span><span class="sxs-lookup"><span data-stu-id="723ff-245">An EF Centric Implementation</span></span>

<span data-ttu-id="723ff-246">ASP.NET MVC projesinde aşağıdaki denetleyici eylem göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="723ff-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="723ff-247">Eylemi bir çalışan nesnesini alır ve çalışan ayrıntılı bir görünümünü görüntülemek için bir sonuç döndürür.</span><span class="sxs-lookup"><span data-stu-id="723ff-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="723ff-248">Kod, test edilebilir nedir?</span><span class="sxs-lookup"><span data-stu-id="723ff-248">Is the code testable?</span></span> <span data-ttu-id="723ff-249">Eylemin davranışı doğrulama ihtiyacımız en az iki testleri vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="723ff-250">İlk olarak, eylemin doğru görünümü – bir kolayca test döndürür doğrulamak isteriz.</span><span class="sxs-lookup"><span data-stu-id="723ff-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="723ff-251">Biz de eylem doğru çalışan alır ve veritabanını sorgulamak için kod çalıştırmadan bunu istiyoruz doğrulamak için bir test yazmak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="723ff-252">Test edilen kod izole etmek istediğimiz unutmayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="723ff-253">Test veri erişim kodu veya veritabanı yapılandırması bir hata nedeniyle başarısız değil yalıtım sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="723ff-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="723ff-254">Test başarısız olursa, biz denetleyici mantığında ve bazı alt düzey sistemi bileşeni değil, bir hataya sahip öğrenmiş olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="723ff-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="723ff-255">Ayırma sağlamak için biz size sunulan arabirimler gibi bazı soyutlama önceki depoları ve iş birimleri için gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="723ff-256">Depo düzeni, etki alanı nesnelerini ve veri eşleme katmanı arasında aktarmak için tasarlanmıştır unutmayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="723ff-257">Bu senaryoda EF4 *olduğu* eşleme verileri katman ve zaten IObjectSet adlı bir depo benzeri soyutlama sağlar&lt;T&gt; (Başlangıç System.Data.Objects ad alanı).</span><span class="sxs-lookup"><span data-stu-id="723ff-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="723ff-258">Arabirim tanımı aşağıdaki gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="723ff-258">The interface definition looks like the following.</span></span>

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

<span data-ttu-id="723ff-259">IObjectSet&lt;T&gt; nesnelerinin bir koleksiyonunu benzer çünkü bir depo gereksinimlerini karşılayan (IEnumerable aracılığıyla&lt;T&gt;) ve ekleme ve nesneleri sanal koleksiyondan kaldırmak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="723ff-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="723ff-260">Attach ve Detach yöntemleri ek özellikler EF4 API'sinin kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="723ff-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="723ff-261">IObjectSet kullanılacak&lt;T&gt; depoları birbirine bağlamak için bir birim iş soyutlama ihtiyacımız depoları için arabirim.</span><span class="sxs-lookup"><span data-stu-id="723ff-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="723ff-262">Bu arabirimin somut bir uygulama için SQL Server Bahsediyor ve EF4 ObjectContext sınıftan kullanarak oluşturmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="723ff-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="723ff-263">Gerçek iş birimi olarak EF4 API ObjectContext sınıftır.</span><span class="sxs-lookup"><span data-stu-id="723ff-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

<span data-ttu-id="723ff-264">Bir IObjectSet getirme&lt;T&gt; yaşama ObjectContext nesnesinin CreateObjectSet yöntemini çağırma olarak oldukça kolaydır.</span><span class="sxs-lookup"><span data-stu-id="723ff-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="723ff-265">Arka planda framework meta verileri kullanır. somut ObjectSet üretmek için EDM bizim sağladığımız&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="723ff-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="723ff-266">Biz IObjectSet döndüren ile kullanacağız&lt;T&gt; istemci kodu, test edilebilirliğe korumak yardımcı olacağından arabirim.</span><span class="sxs-lookup"><span data-stu-id="723ff-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="723ff-267">Somut bu uygulama, üretim ortamında kullanışlıdır ancak nasıl bizim IUnitOfWork soyutlama test edilmesini kolaylaştırmak için kullanacağız üzerinde odaklanmak ihtiyacımız.</span><span class="sxs-lookup"><span data-stu-id="723ff-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="723ff-268">Test double</span><span class="sxs-lookup"><span data-stu-id="723ff-268">The Test Doubles</span></span>

<span data-ttu-id="723ff-269">Denetleyici eylemini yalıtmak için gerçek birimini (bir ObjectContext tarafından desteklenen) iş ve iş (bellek içi işlemleri) bir test çift veya "sahte" birimi arasında geçiş yapma özelliğini gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="723ff-270">Bu tür bir geçiş gerçekleştirmek için genel iş birimi, ancak bunun yerine denetleyici Oluşturucu parametresi olarak iş birimini geçişinde örneği MVC denetleyicisi bulunmamasını yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="723ff-271">Yukarıdaki kod, bağımlılık ekleme örneğidir.</span><span class="sxs-lookup"><span data-stu-id="723ff-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="723ff-272">Biz, bağımlılık (iş birimi) oluşturur, ancak Denetleyicisi'nde bağımlılık ekleme denetleyicisi izin vermez.</span><span class="sxs-lookup"><span data-stu-id="723ff-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="723ff-273">MVC projesinde bağımlılık ekleme otomatikleştirmek için bir denetim (IOC) kapsayıcı tersine çevirme ile birlikte bir özel denetleyici üretecini kullanımı yaygındır.</span><span class="sxs-lookup"><span data-stu-id="723ff-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="723ff-274">Bu konular için bu makalenin kapsamı dışında olsa da, bu makalenin sonunda başvuruları aşağıdaki göre daha fazla bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="723ff-275">Sahte bir birim test etme için kullanabiliriz iş uygulamasının aşağıdaki gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

<span data-ttu-id="723ff-276">Sahte iş birimi bir kabul edilen özelliği sunan dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="723ff-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="723ff-277">Bazen, test edilmesini kolaylaştırmak sahte bir sınıfa özellikler eklemek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="723ff-278">Bu durumda kod bir iş birimi işlemeler, kabul edilen özelliğini kontrol ederek gözlemlemek kolaydır.</span><span class="sxs-lookup"><span data-stu-id="723ff-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="723ff-279">Ayrıca sahte IObjectSet gerekir&lt;T&gt; çalışan ve zaman çizelgesi nesneleri bellekte tutmak için.</span><span class="sxs-lookup"><span data-stu-id="723ff-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="723ff-280">Biz genel türler kullanarak tek bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="723ff-280">We can provide a single implementation using generics.</span></span>

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

<span data-ttu-id="723ff-281">Çift bu test için bir temel alınan HashSet çalışmasının çoğunu Temsilciler&lt;T&gt; nesne.</span><span class="sxs-lookup"><span data-stu-id="723ff-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="723ff-282">Not Bu IObjectSet&lt;T&gt; T bir sınıf (bir başvuru türü) zorlamayı genel bir kısıtlama gerektirir ve ayrıca bize Iqueryable uygulamak için zorlar&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="723ff-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="723ff-283">Bir bellek içi koleksiyonu Iqueryable bilgisi görünür hale getirmek kolay&lt;T&gt; AsQueryable standart LINQ işlecini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="723ff-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="723ff-284">Testleri</span><span class="sxs-lookup"><span data-stu-id="723ff-284">The Tests</span></span>

<span data-ttu-id="723ff-285">Geleneksel birim testleri, tüm testleri tüm eylemler için tek bir MVC denetleyicisi tutmak için tek bir test sınıfı kullanır.</span><span class="sxs-lookup"><span data-stu-id="723ff-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="723ff-286">Bu testler ya da herhangi bir türde birim testi, bellek içi kullanarak fakes yazabiliriz oluşturduk.</span><span class="sxs-lookup"><span data-stu-id="723ff-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="723ff-287">Ancak bu makalede sorundan kaçınmak için tek parça test sınıfı yaklaşımı ve bunun yerine belirli bir işlevsellik odaklanmak için yaptığımız testleri gruplayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="723ff-288">Örneğin, yeni çalışan "Oluştur" tek test sınıfının yeni bir çalışan oluşturmaktan sorumlu tek denetleyici eylemi doğrulamak için kullanacağız şekilde test etmek istiyoruz işlevleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="723ff-289">Bazı ortak kurulum kodu için tüm bu ayrıntılı test sınıflarında ihtiyacımız yoktur.</span><span class="sxs-lookup"><span data-stu-id="723ff-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="723ff-290">Örneğin, size her zaman, bellek içi depo ve iş sahte birimi oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="723ff-291">Ayrıca çalışan denetleyici örneği eklenen iş sahte birimle ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="723ff-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="723ff-292">Bir temel sınıfı kullanılarak biz test sınıflarında bu ortak kurulum kodu sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="723ff-292">We’ll share this common setup code across test classes by using a base class.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

<span data-ttu-id="723ff-293">"Nesne Annenizin ve taban sınıfta kullanıyoruz" test verilerini oluşturmak için bir ortak desendir.</span><span class="sxs-lookup"><span data-stu-id="723ff-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="723ff-294">Bir nesne Annenizin test varlıkları arasında birden çok test armatürleri örneklemek için Fabrika yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="723ff-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

<span data-ttu-id="723ff-295">(Bkz. Şekil 3) test armatürleri sayısı için EmployeeControllerTestBase temel sınıf olarak kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="723ff-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="723ff-296">Her test düzeni, belirli bir denetleyici eylemi test eder.</span><span class="sxs-lookup"><span data-stu-id="723ff-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="723ff-297">Örneğin, bir test düzeni, oluşturma eylemini (bir çalışan oluşturmak için görüntülemek için) bir HTTP GET isteği sırasında kullanılan testlere olmadığını odaklanın ve farklı bir düzen, bir HTTP POST isteğinde kullanılan Oluştur eylemi odaklanacaktır (tarafından gönderilen bilgi almak için Kullanıcı) bir çalışan oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="723ff-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="723ff-298">Her bir türetilmiş sınıf, yalnızca belirli bağlamında ve sonuçlar belirli test bağlamını doğrulamak için gereken bir onayları sağlamak için gereken kurulum sorumludur.</span><span class="sxs-lookup"><span data-stu-id="723ff-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![EF test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="723ff-300">**Şekil 3**</span><span class="sxs-lookup"><span data-stu-id="723ff-300">**Figure 3**</span></span>

<span data-ttu-id="723ff-301">Burada sunulan adlandırma kuralı ve test stili test edilebilir kod için gerekli değildir-yalnızca bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="723ff-302">Şekil 4, test Çalıştırıcısı eklentisi Visual Studio 2010 için Jet Brains Resharper çalıştıran testleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="723ff-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![EF test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="723ff-304">**Şekil 4**</span><span class="sxs-lookup"><span data-stu-id="723ff-304">**Figure 4**</span></span>

<span data-ttu-id="723ff-305">Her denetleyici eylemi için birim testleri, paylaşılan Kurulum kodu işlemek için temel sınıf ile küçük ve yazmak kolay.</span><span class="sxs-lookup"><span data-stu-id="723ff-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="723ff-306">Testler hızlıca (biz bellek içi işlemleri yapıldığından) yürütülür ve ilgisiz altyapı veya çevre kaygıları nedeniyle (biz birim testten ayırdığınız nedeniyle) başarısız olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

<span data-ttu-id="723ff-307">Bu sınamalarda, temel sınıf Kurulum işin çoğunu yapar.</span><span class="sxs-lookup"><span data-stu-id="723ff-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="723ff-308">Temel sınıf oluşturucusu bellek içi depoya, sahte bir iş birimi ve EmployeeController sınıfının bir örneğini oluşturur unutmayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="723ff-309">Test sınıfı bu taban sınıftan türetilen ve oluşturma yöntemi test özellikleri odaklanır.</span><span class="sxs-lookup"><span data-stu-id="723ff-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="723ff-310">Bu durumda özellikleri yordamı sınama biriminde görürsünüz "düzenlemek, davranan ve onaylama işlemi" adımlarla boil:</span><span class="sxs-lookup"><span data-stu-id="723ff-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="723ff-311">Gelen veri benzetimini yapmak için bir Yeniçalışan nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="723ff-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="723ff-312">EmployeeController oluşturma eylemini çağırın ve Yeniçalışan içinde geçirin.</span><span class="sxs-lookup"><span data-stu-id="723ff-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="723ff-313">Oluşturma eylemini (havuzda çalışan görünür) beklenen sonuçları üretir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="723ff-314">Ne oluşturduğumuz EmployeeController eylemlerden herhangi birini test olanak sağlıyor.</span><span class="sxs-lookup"><span data-stu-id="723ff-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="723ff-315">Biz testler için çalışan denetleyici dizin eylemi yazdığınızda, biz testlerimiz için aynı temel kurulum'kurmak için test temel sınıftan devralabilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="723ff-316">Temel sınıf yeniden bellek içi depo ve iş birimini sahte EmployeeController örneği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="723ff-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="723ff-317">Dizin eylem için testler dizin eylemi çağırmaya odaklanmak yalnızca gerekli ve eylem modeli nitelikleri test döndürür.</span><span class="sxs-lookup"><span data-stu-id="723ff-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

<span data-ttu-id="723ff-318">Bellek içi fakes ile oluşturmakta testleri test doğru yerleştirilir *durumu* yazılım.</span><span class="sxs-lookup"><span data-stu-id="723ff-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="723ff-319">Örneğin, oluşturma eylemini yürütüldükten sonra – depo durumunu incelemek için istediğimiz oluşturma eylemini test ederken depoya yeni çalışan içeriyor mu?</span><span class="sxs-lookup"><span data-stu-id="723ff-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="723ff-320">Temel etkileşim test sırasında daha sonra inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="723ff-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="723ff-321">Etkileşim tabanlı test, test edilen kod uygun yöntemleri bizim nesneler üzerinde çağrılır ve doğru parametreleri geçirilen sorar.</span><span class="sxs-lookup"><span data-stu-id="723ff-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="723ff-322">Şu an için başka bir tasarım deseni – yavaş yük kapağına geçeceğiz.</span><span class="sxs-lookup"><span data-stu-id="723ff-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="723ff-323">İstekli yükleme ve yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="723ff-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="723ff-324">ASP.NET MVC Web belirli bir noktada bir çalışanın bilgilerini görüntülemek ve çalışanın eklemek için isteyebilir uygulama zaman çizelgesi ilişkili.</span><span class="sxs-lookup"><span data-stu-id="723ff-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="723ff-325">Örneğin, biz sistemde çalışan adı ve zaman çizelgesi toplam sayısını gösteren zaman çizelgesi Özet görüntü olabilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="723ff-326">Bu özelliği uygulamak için uygulayabileceğiniz çeşitli yaklaşımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="723ff-327">Projeksiyon</span><span class="sxs-lookup"><span data-stu-id="723ff-327">Projection</span></span>

<span data-ttu-id="723ff-328">Özet oluşturmak için bir kolayca görünümünde görüntülemek istediğiniz bilgileri için adanmış bir modeli oluşturmak için yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="723ff-329">Bu senaryoda modeli aşağıdaki gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="723ff-330">EmployeeSummaryViewModel bir varlık değil-diğer bir deyişle, bir veritabanında kalıcı hale getirmek istiyoruz değil unutmayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="723ff-331">Yalnızca bu sınıf türü kesin belirlenmiş bir şekilde görünüm verileri karıştırması kullanmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="723ff-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="723ff-332">Bir veri aktarımı nesnesi (DTO), hiçbir davranışları (hiçbir yöntemleri) – yalnızca özellikler içerdiği için Görünüm modeli gibidir.</span><span class="sxs-lookup"><span data-stu-id="723ff-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="723ff-333">Özellikleri taşımak için ihtiyacımız olan veriler tutar.</span><span class="sxs-lookup"><span data-stu-id="723ff-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="723ff-334">Bu görünüm modeli LINQ'ın standart projeksiyon işleci – seçme işleci kullanarak örneği oluşturmak daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="723ff-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

<span data-ttu-id="723ff-335">Yukarıdaki kodu için iki önemli özellikler mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="723ff-335">There are two notable features to the above code.</span></span> <span data-ttu-id="723ff-336">İlk – kodu inceleyin ve yalıtmak hala kolay olduğundan test etmek kolay bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="723ff-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="723ff-337">Gerçek iş birimini karşı çalıştığı gibi seçme işleci ekleyebiliyorsa, bellek içi fakes karşı çalışır.</span><span class="sxs-lookup"><span data-stu-id="723ff-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

<span data-ttu-id="723ff-338">İkinci önemli kod EF4 birlikte çalışan ve zaman çizelgesi bilgileri birleştirmek için tek ve verimli bir sorgu oluşturmak nasıl imkan özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="723ff-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="723ff-339">Çalışan bilgileri ve zaman çizelgesi bilgileri aynı nesnede herhangi bir özel API'leri kullanmadan yüklendik.</span><span class="sxs-lookup"><span data-stu-id="723ff-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="723ff-340">Kod, yalnızca uzak veri kaynaklarını yanı sıra bellek içi veri kaynaklarına karşı çalışan LINQ işleçlerini standart'ı kullanarak gerektiren bilgi ifade.</span><span class="sxs-lookup"><span data-stu-id="723ff-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="723ff-341">C ve LINQ Sorgu tarafından oluşturulan ifade ağaçları çevrilecek EF4 bağlanabildi\# derleyici içine tek ve verimli bir T-SQL sorgusu.</span><span class="sxs-lookup"><span data-stu-id="723ff-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="723ff-342">Bazen, biz bir görünüm modeli veya DTO nesne, ancak gerçek varlıklarla çalışmak istemiyor vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="723ff-343">Ne zaman biliyoruz çalışan ihtiyacımız *ve* çalışanın zaman çizelgesi, biz eagerly örtük ve verimli bir şekilde ilgili veri yükleyebilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="723ff-344">Açık istekli yükleme</span><span class="sxs-lookup"><span data-stu-id="723ff-344">Explicit Eager Loading</span></span>

<span data-ttu-id="723ff-345">İlgili varlık bilgileri ihtiyacımız bazı mekanizması iş mantığı için (veya bu senaryoda, denetleyici eylem mantıksal) eagerly yüklenmeye istediğinizde, arzu depoya ifade etmek için.</span><span class="sxs-lookup"><span data-stu-id="723ff-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="723ff-346">EF4 ObjectQuery&lt;T&gt; sınıf sorgu sırasında almak için ilgili nesneleri belirlemek için bir dahil etme yöntemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="723ff-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="723ff-347">EF4 ObjectContext varlıkları somut ObjectSet aracılığıyla kullanıma sunan unutmayın&lt;T&gt; ObjectQuery devralan sınıf&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="723ff-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="723ff-348">Biz ObjectSet kullanıyorsanız&lt;T&gt; başvuruları denetleyicisi eylemimiz biz aşağıdaki kodu yazabilirsiniz istekli bir zaman çizelgesi bilgileri, her çalışanın yüklemesini belirtin.</span><span class="sxs-lookup"><span data-stu-id="723ff-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="723ff-349">Bu yana kodumuz test edilebilir korumak çalışıyoruz ancak biz ObjectSet karşı savunmasız bırakıyorsunuz değil&lt;T&gt; gelen gerçek iş sınıfı ölçü dışında.</span><span class="sxs-lookup"><span data-stu-id="723ff-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="723ff-350">Bunun yerine, biz üzerinde IObjectSet güvenin&lt;T&gt; arabirimi sahtesini oluşturmak kolaydır ancak IObjectSet&lt;T&gt; Ekle yöntemi tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="723ff-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="723ff-351">LINQ güzelliği kendi Ekle işleci oluşturabiliriz ' dir.</span><span class="sxs-lookup"><span data-stu-id="723ff-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

<span data-ttu-id="723ff-352">Bu Ekle işleci Iqueryable için bir genişletme yöntemi tanımlanan fark&lt;T&gt; IObjectSet yerine&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="723ff-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="723ff-353">Bu yöntemi bir yelpazede Iqueryable dahil olmak üzere, olası bir türü kullanma yeteneği sağlıyor&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;ve ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="723ff-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="723ff-354">Temel alınan dizi orijinal bir EF4 ObjectQuery değil durumunda&lt;T&gt;, ardından yapılan hiçbir zarar yoktur ve bir İşlemsiz INCLUDE işlecidir.</span><span class="sxs-lookup"><span data-stu-id="723ff-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="723ff-355">Alttaki sıra, *olduğu* ObjectQuery&lt;T&gt; (veya ObjectQuery türetilmiş&lt;T&gt;), EF4 bizim ek veri gereksinimini bakın ve uygun SQL düzenleme Sorgu.</span><span class="sxs-lookup"><span data-stu-id="723ff-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="723ff-356">Bu yeni işleciyle yerinde biz açıkça bir zaman çizelgesi bilgileri istekli yükünü depodan isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="723ff-357">Gerçek bir Objectcontext'e karşı çalıştırdığınızda, aşağıdaki tek sorgu kod üretir.</span><span class="sxs-lookup"><span data-stu-id="723ff-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="723ff-358">Sorgu, veritabanından çalışan nesnelerini gerçekleştirmek ve tamamen kendi zaman çizelgeleri özelliğini doldurmak için bir seyahat yeterli bilgi toplar.</span><span class="sxs-lookup"><span data-stu-id="723ff-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="723ff-359">Eylem yöntemi içindeki kodu tam olarak test edilebilir kalır harika bir haberimiz var olur.</span><span class="sxs-lookup"><span data-stu-id="723ff-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="723ff-360">Ekle işleci desteklemek fakes bizim için herhangi bir ek özellikler sağlamak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="723ff-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="723ff-361">Ekle işleci Kalıcılık ignorant tutmak istedik kod içinde kullanılacak vardı hatalı haber olur.</span><span class="sxs-lookup"><span data-stu-id="723ff-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="723ff-362">Bu tür bir test edilebilir kod oluşturma sırasında değerlendirilecek ihtiyacınız olacak ödün prime bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="723ff-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="723ff-363">Kalıcılık kaygıları sızıntısı performans hedeflerinize ulaşmak için depo soyutlama dışında izin vermek için gerektiğinde zamanlar vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="723ff-364">İstekli yükleme için alternatiftir yavaş yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="723ff-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="723ff-365">Yaptığımız yavaş yükleniyor anlamına gelir *değil* açıkça ilişkili veri gereksinimini duyurmaktan iş kodumuz gerekir.</span><span class="sxs-lookup"><span data-stu-id="723ff-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="723ff-366">Bunun yerine, Entity Framework kullandığımız bizim varlıkları uygulamada ve ek veri olup olmadığını gereken isteğe bağlı veri yükler.</span><span class="sxs-lookup"><span data-stu-id="723ff-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="723ff-367">Yavaş yükleniyor</span><span class="sxs-lookup"><span data-stu-id="723ff-367">Lazy Loading</span></span>

<span data-ttu-id="723ff-368">Biz ne veri parçasını iş mantığı gerekir burada bilmiyorum bir senaryoyu düşünün daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="723ff-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="723ff-369">Mantıksal bir çalışan nesne olması gerekiyor, ancak biz burada bu yollar bazıları çalışana ait zaman çizelgesi bilgileri gerektirir ve bazı yapmak farklı yürütme yolları dal biliyor olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="723ff-370">Bu gibi senaryolarda veri Sihirli bir gerektiği şekilde göründüğünden örtük geç yüklemek için mükemmeldir.</span><span class="sxs-lookup"><span data-stu-id="723ff-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="723ff-371">Ertelenmiş olarak da bilinir, yavaş yükleniyor yükleniyor, bizim varlık nesneler üzerinde bazı gereksinimler yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="723ff-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="723ff-372">True Kalıcılık ignorance ile POCOs, gereksinimlere Kalıcılık katmandan yönde değil, ancak doğru Kalıcılık ignorance elde etmek çevrilmesi imkansızdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="723ff-373">Bunun yerine Kalıcılık ignorance göreli derece cinsinden ölçer.</span><span class="sxs-lookup"><span data-stu-id="723ff-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="723ff-374">Kalıcılık yönelik bir taban sınıftan devralın ya da özel bir koleksiyon POCOs yavaş yükleme elde etmek için kullanmak üzere ihtiyacımız talihsiz olurdu.</span><span class="sxs-lookup"><span data-stu-id="723ff-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="723ff-375">Neyse ki, EF4 çalışılmasını daha az sezgisel bir çözümü vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="723ff-376">Neredeyse algılanamayan</span><span class="sxs-lookup"><span data-stu-id="723ff-376">Virtually Undetectable</span></span>

<span data-ttu-id="723ff-377">POCO nesneleri kullanırken EF4 varlıklar için çalışma zamanı proxy'si dinamik olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="723ff-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="723ff-378">Bu proxy'ler görünmez gerçekleştirilmiş POCOs kaydırma ve her bir özellik kesintiye tarafından ek hizmetler elde sağlar ve ayarlama işlemi ek çalışma gerçekleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="723ff-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="723ff-379">Böyle bir hizmet arıyoruz için yavaş yükleniyor özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="723ff-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="723ff-380">Verimli bir değişiklik izleme mekanizması program bir varlığın özellik değerleri değiştiğinde kaydedebilirsiniz başka bir hizmettir.</span><span class="sxs-lookup"><span data-stu-id="723ff-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="723ff-381">Değişikliklerin listesini Objectcontext'in SaveChanges yöntemi sırasında değiştirilen güncelleştirme komutları kullanarak herhangi bir varlık kalıcı hale getirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="723ff-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="723ff-382">Çalışmak bu proxy'leri için ancak ihtiyaç duydukları get özelliği bağlama için bir yol ve ayarlama işlemleri varlık ve proxy'ler sanal üyelerini geçersiz kılarak bu amaca ulaşmak.</span><span class="sxs-lookup"><span data-stu-id="723ff-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="723ff-383">Örtük geç yükleme ve değişiklik izleme etkin olmasını istiyoruz, bu nedenle, bizim POCO sınıf tanımları için geri dönün ve Özellikler sanal olarak işaretlemek ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="723ff-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="723ff-384">Biz yine de, çoğunlukla ignorant Kalıcılık çalışan varlıktır da söyleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="723ff-385">Sanal üyeler kullanmak için tek gereksinim olmasıdır ve kodun test edilebilirliğini etkilemez.</span><span class="sxs-lookup"><span data-stu-id="723ff-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="723ff-386">Özel bir temel sınıfından türetilir veya bile Gecikmeli yükleme için ayrılmış bir özel koleksiyon gerekmez.</span><span class="sxs-lookup"><span data-stu-id="723ff-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="723ff-387">Kod kadarıyla ICollection uygulayan herhangi bir sınıfa&lt;T&gt; ilgili varlıkları tutmak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="723ff-388">Bir alt değişiklik yapmak için iş birimi içinde ihtiyacımız yoktur.</span><span class="sxs-lookup"><span data-stu-id="723ff-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="723ff-389">Gecikmeli yükleme zaman *kapalı* doğrudan bir Objectcontext'e nesne ile çalışırken, varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="723ff-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="723ff-390">Biz ContextOptions özelliği etkinleştirmek için Ertelenen yükleme ayarlayabilirsiniz bir özelliği yoktur ve her yerde yavaş yükleniyor sağlamak istiyorsanız Biz bu özellik, gerçek iş birimi içinde ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

<span data-ttu-id="723ff-391">Etkinleştirilmiş örtük geç yükleme ile ek verileri yüklemek EF için gerekli çalışmanın hiç farkında olmadan kalan çalışırken zaman çizelgesi çalışanın ilişkili ve bir çalışan uygulama kodu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="723ff-392">Yavaş yükleniyor uygulama kodu yazmak daha kolay hale getirir ve kod ile proxy Sihirli tamamen test edilebilir kalır.</span><span class="sxs-lookup"><span data-stu-id="723ff-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="723ff-393">Bellek içi fakes çalışma biriminin sadece sahte test sırasında gerektiğinde ilişkili veri varlıklarıyla önceden yükleyebilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="723ff-394">Bu noktada biz IObjectSet kullanarak depo oluşturma uygulamamızla açacağım&lt;T&gt; Kalıcılık framework'ün tüm işaretleri gizlemek için soyutlama bakın.</span><span class="sxs-lookup"><span data-stu-id="723ff-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="723ff-395">Özel depolar</span><span class="sxs-lookup"><span data-stu-id="723ff-395">Custom Repositories</span></span>

<span data-ttu-id="723ff-396">Biz çalışma tasarım deseni birimi bu makalede gösterilen zaman biz bazı örnek kodlar iş birimini görünebileceği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="723ff-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="723ff-397">Şimdi yeniden çalışan ve biz ile çalışmalar çalışan zaman çizelgesi senaryosu kullanarak bu özgün bir fikir sunar.</span><span class="sxs-lookup"><span data-stu-id="723ff-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="723ff-398">Bu iş birimi ve son bölümde oluşturduğumuz iş birimini arasındaki birincil fark nasıl bu iş birimi EF4 çerçeveden herhangi bir soyutlama kullanmaz olduğu (hiçbir IObjectSet yoktur&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="723ff-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="723ff-399">IObjectSet&lt;T&gt; depo arabirimi ancak kullanıma sunduğu API'yi olarak çalışır değil mükemmel bir şekilde hizalayın bizim uygulama gereksinimlerine göre.</span><span class="sxs-lookup"><span data-stu-id="723ff-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="723ff-400">Yaklaşan Bu yaklaşımda size özel IRepository kullanarak depoları temsil edecek&lt;T&gt; Özet.</span><span class="sxs-lookup"><span data-stu-id="723ff-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="723ff-401">Test Odaklı Tasarım, davranış Odaklı Tasarım ve etki alanı odaklı yöntemlerini tasarım izleyen birçok geliştiricinin tercih IRepository&lt;T&gt; çeşitli nedenlerle yaklaşım.</span><span class="sxs-lookup"><span data-stu-id="723ff-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="723ff-402">İlk olarak, IRepository&lt;T&gt; arabirimi, bir "bozulma önleyici" Katman'ı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="723ff-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="723ff-403">Etki alanı Odaklı Tasarım nitelemiştir Eric Evans tarafından açıklandığı gibi etki alanı kodu bir Kalıcılık API gibi altyapı API'leri, uzakta bir bozulma önleyici katman tutar.</span><span class="sxs-lookup"><span data-stu-id="723ff-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="723ff-404">İkincisi, geliştiriciler (testleri yazılırken bulunan gibi) bir uygulamanın tam gereksinimleri karşılayan depoya yöntemleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="723ff-405">Örneğin, sık FindById yöntemi depo arabirimi ekleyebilmek için bir kimlik değeri kullanarak tek bir varlık bulmak ihtiyacımız.</span><span class="sxs-lookup"><span data-stu-id="723ff-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="723ff-406">Bizim IRepository&lt;T&gt; tanımı aşağıdaki gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="723ff-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="723ff-407">Biz bırakma geri Iqueryable kullanmaya dikkat edin&lt;T&gt; arabirimi varlık koleksiyonlarını açığa çıkarır.</span><span class="sxs-lookup"><span data-stu-id="723ff-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="723ff-408">Iqueryable&lt;T&gt; LINQ ifade ağaçları EF4 sağlayıcısına akış ve sağlayıcı sorguyu bütünsel bir görünümünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="723ff-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="723ff-409">IEnumerable döndürmek için ikinci bir seçenek olacaktır&lt;T&gt;, EF4 LINQ sağlayıcısı başka bir deyişle, yalnızca depo içinde yerleşik ifadeleri görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="723ff-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="723ff-410">Tüm gruplandırma, sıralama ve havuz dışında yapılan projeksiyon performansı olumsuz etkileyebilir veritabanına gönderilen SQL komutu içine oluşacaktır değil.</span><span class="sxs-lookup"><span data-stu-id="723ff-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="723ff-411">Diğer yandan, yalnızca IEnumerable döndüren bir depo&lt;T&gt; sonuçları hiçbir zaman şaşırtan, yeni bir SQL komutu.</span><span class="sxs-lookup"><span data-stu-id="723ff-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="723ff-412">Her iki yaklaşım çalışır ve her iki yaklaşım test edilebilir kalır.</span><span class="sxs-lookup"><span data-stu-id="723ff-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="723ff-413">IRepository tek bir uygulamasını sağlamak üzere basittir&lt;T&gt; arabirimi genel türler ve EF4 ObjectContext API kullanma.</span><span class="sxs-lookup"><span data-stu-id="723ff-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

<span data-ttu-id="723ff-414">IRepository&lt;T&gt; yaklaşım sağladığı bizim sorgular üzerinde bazı ek denetim bir varlığa almak için bir yöntemi çağırmak bir istemci olduğundan.</span><span class="sxs-lookup"><span data-stu-id="723ff-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="723ff-415">Yöntem içinde size ek denetimler ve uygulama kısıtlamaları uygulamak için LINQ işleçleri sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="723ff-416">Genel tür parametresi kısıtlamaları iki arabirim olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="723ff-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="723ff-417">İlk sınırlamadır ObjectSet tarafından gerekli sınıf olumsuz taint&lt;T&gt;, bizim varlıkları IEntity – uygulama için oluşturulan bir soyutlama uygulamak için ikinci kısıtlamayı zorlar.</span><span class="sxs-lookup"><span data-stu-id="723ff-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="723ff-418">Okunabilir bir kimlik özelliği için varlıklar IEntity arabirimi zorlar ve ardından bu özellik FindById yöntemin kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="723ff-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="723ff-419">Aşağıdaki kod ile IEntity tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="723ff-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="723ff-420">Bu arabirim uygulamak için sunduğumuz varlıkları gerektiğinden IEntity Kalıcılık ignorance küçük ihlalini değerlendirilebilir.</span><span class="sxs-lookup"><span data-stu-id="723ff-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="723ff-421">Kalıcılık ignorance ödünler olan ve birçok için FindById işlevselliğini arabirimi tarafından uygulanan kısıtlama gölgede unutmayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="723ff-422">Arabirim Test Edilebilirlik üzerinde bir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="723ff-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="723ff-423">Canlı IRepository örnekleme&lt;T&gt; somut bir birimi, iş uygulaması oluşturmada yönetmesi gereken şekilde bir EF4 Objectcontext'e gerektirir.</span><span class="sxs-lookup"><span data-stu-id="723ff-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a><span data-ttu-id="723ff-424">Özel depo kullanma</span><span class="sxs-lookup"><span data-stu-id="723ff-424">Using the Custom Repository</span></span>

<span data-ttu-id="723ff-425">Özel depomuzda kullanarak IObjectSet üzerinde temel depoyu kullanarak önemli ölçüde farklı değil&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="723ff-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="723ff-426">LINQ işleçleri doğrudan bir özelliğe uygulamak yerine, önce bir Iqueryable bilgisi almak için deponun yöntemlerini çağırmak gerekir&lt;T&gt; başvuru.</span><span class="sxs-lookup"><span data-stu-id="723ff-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="723ff-427">Daha önce uygulanan özel Ekle işleci değişiklik çalışır dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="723ff-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="723ff-428">Deponun FindById yöntemi, tek bir varlık almaya çalışırken eylemlerden yinelenen mantık kaldırır.</span><span class="sxs-lookup"><span data-stu-id="723ff-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="723ff-429">Biz incelenirken iki yaklaşım de içinde önemli bir fark yoktur.</span><span class="sxs-lookup"><span data-stu-id="723ff-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="723ff-430">Biz IRepository sahte uygulamaları sağlayabilir&lt;T&gt; HashSet tarafından desteklenen somut sınıflar oluşturma tarafından&lt;çalışan&gt; - son bölümde ne yaptığımız gibi.</span><span class="sxs-lookup"><span data-stu-id="723ff-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="723ff-431">Ancak, bazı geliştiriciler, sahte nesneler kullanıp fakes derleme yerine nesne çerçeveleri sahte tercih eder.</span><span class="sxs-lookup"><span data-stu-id="723ff-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="723ff-432">Kararlılığımızın test ve fakes mocks arasındaki farklar sonraki bölümde ele mocks kullanarak inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="723ff-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="723ff-433">Mocks ile test etme</span><span class="sxs-lookup"><span data-stu-id="723ff-433">Testing with Mocks</span></span>

<span data-ttu-id="723ff-434">Bir "testi çift" hangi Martin Fowler çağrıları oluşturmak için farklı yaklaşım vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="723ff-435">Bir test (bir film stunt gibi çift) çift yapı "gerçekten bekleme için" üretim nesneleri testleri sırasında nesne değildir.</span><span class="sxs-lookup"><span data-stu-id="723ff-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="723ff-436">Oluşturduğumuz bellek içi depo için SQL Server konuşmak depoları için test double ' dir.</span><span class="sxs-lookup"><span data-stu-id="723ff-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="723ff-437">Biz kod yalıtmak ve hızlı çalışan testler tutmak için birim testleri sırasında bu test double kullanmayı gördünüz.</span><span class="sxs-lookup"><span data-stu-id="723ff-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="723ff-438">Oluşturduğumuz test double gerçek ve çalışma uygulamaları vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="723ff-439">Arka planda her biri bir somut nesnelerinin koleksiyonunu depolar ve bunlar ekleyin ve biz depo test sırasında değişiklik yaptıkça nesneleri bu koleksiyondan Kaldır.</span><span class="sxs-lookup"><span data-stu-id="723ff-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="723ff-440">Bazı geliştiriciler, kendi test double bu şekilde – gerçek kod ve çalışma uygulamaları ile oluşturmak ister.</span><span class="sxs-lookup"><span data-stu-id="723ff-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="723ff-441">Bu test double dediğimiz olan *fakes*.</span><span class="sxs-lookup"><span data-stu-id="723ff-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="723ff-442">Bunlar çalışan uygulamalara sahip, ancak bunlar üretimde kullanıma yönelik gerçek yeterli değildir.</span><span class="sxs-lookup"><span data-stu-id="723ff-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="723ff-443">Sahte depo gerçekten veritabanına yazma değil.</span><span class="sxs-lookup"><span data-stu-id="723ff-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="723ff-444">Sahte SMTP sunucusu, ağ üzerinden bir e-posta iletisi gerçekten göndermez.</span><span class="sxs-lookup"><span data-stu-id="723ff-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="723ff-445">Mocks Fakes karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="723ff-445">Mocks versus Fakes</span></span>

<span data-ttu-id="723ff-446">Başka türde bir çift olarak bilinen test yok. bir *sahte*.</span><span class="sxs-lookup"><span data-stu-id="723ff-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="723ff-447">Fakes çalışma uygulamaları olsa da, mocks hiçbir bir uygulama ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="723ff-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="723ff-448">Yardım sahte nesne framework'ün çalışma zamanında bu sahte nesneler oluşturmak ve test double kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="723ff-449">Bu bölümde framework Moq sahte işlem açık kaynak kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="723ff-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="723ff-450">Bir testi çift bir çalışan havuzu için dinamik olarak oluşturmak için Moq kullanmanın basit bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="723ff-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="723ff-451">İçin bir IRepository Moq isteriz&lt;çalışan&gt; uygulama ve yapılar bir dinamik olarak.</span><span class="sxs-lookup"><span data-stu-id="723ff-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="723ff-452">IRepository uygulayan nesne için aldığımız&lt;çalışan&gt; sahte nesne özelliğini erişerek&lt;T&gt; nesne.</span><span class="sxs-lookup"><span data-stu-id="723ff-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="723ff-453">Bizim denetleyicilerine geçirerek bu iç nesne olan ve bu çift bir test gerçek depoyu olup olmadığını bilemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="723ff-454">Biz gerçek bir uygulama bir nesne üzerinde yöntemleri çağıracaktır gibi şu nesne üzerinde yöntemleri çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="723ff-455">Biz Ekle yöntemi çağırdığınızda sahte depo yapacaksınız merak ediyor olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="723ff-456">Ekle, sahte nesnenin arkasında uygulaması olduğundan, hiçbir şey yapmaz.</span><span class="sxs-lookup"><span data-stu-id="723ff-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="723ff-457">Çalışan göz ardı edilir şekilde yazdığımız, fakes ile vardı gibi arka planda somut hiçbir koleksiyon yok.</span><span class="sxs-lookup"><span data-stu-id="723ff-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="723ff-458">FindById dönüş değeri hakkında neler diyeceksiniz?</span><span class="sxs-lookup"><span data-stu-id="723ff-458">What about the return value of FindById?</span></span> <span data-ttu-id="723ff-459">Bu durumda sahte nesnenin dönüş olan tek şey yapabilirsiniz, varsayılan bir değer yapar.</span><span class="sxs-lookup"><span data-stu-id="723ff-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="723ff-460">Biz bir başvuru türü (çalışan) döndüren olduğundan, dönüş değeri boş bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="723ff-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="723ff-461">Mocks işe yaramaz ses; Ancak, iki daha fazla hakkında konuştuk henüz mocks özellikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="723ff-462">İlk olarak, Moq framework sahte nesnesinde yapılan tüm çağrıları kaydeder.</span><span class="sxs-lookup"><span data-stu-id="723ff-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="723ff-463">Daha sonra kodda herkes Ekle yöntemi çağırdığınız ya da herkes FindById yöntemi çağırdığınız biz Moq sorabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="723ff-464">Bu "siyah kutu" kaydı özelliğini testlerinde daha sonra nasıl kullanabileceğimizi göreceğiz.</span><span class="sxs-lookup"><span data-stu-id="723ff-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="723ff-465">İkinci harika sahte bir nesneyle programlamak için Moq nasıl kullanabileceğimizi özelliğidir *beklentileri*.</span><span class="sxs-lookup"><span data-stu-id="723ff-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="723ff-466">Bir Beklenti nasıl verilen etkileşimi yanıt sahte nesne bildirir.</span><span class="sxs-lookup"><span data-stu-id="723ff-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="723ff-467">Örneğin, size sunduğumuz sahte bir Beklenti program ve birisi FindById çağırdığında çalışan nesneyi döndürmek için söyleyin.</span><span class="sxs-lookup"><span data-stu-id="723ff-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="723ff-468">Moq framework bu beklentileri programlamak için Kurulum API ve lambda ifadeleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="723ff-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

<span data-ttu-id="723ff-469">Bu örnekte bir depo dinamik olarak oluşturmak için Moq isteriz ve ardından bir Beklenti deposuyla yapacağız.</span><span class="sxs-lookup"><span data-stu-id="723ff-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="723ff-470">Beklenti birisi 5 değerini geçirme FindById yöntemi çağırdığında bir kimliği değeri 5 olan yeni bir çalışan nesne döndürmek için sahte nesne bildirir.</span><span class="sxs-lookup"><span data-stu-id="723ff-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="723ff-471">Bu test geçer ve sahte IRepository için tam bir uygulama oluşturmak gerekmedi&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="723ff-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="723ff-472">Şimdi daha önce yazdığımız testleri yeniden ziyaret ve bunları yerine fakes mocks kullanacak şekilde yeniden.</span><span class="sxs-lookup"><span data-stu-id="723ff-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="723ff-473">Tıpkı daha önce bir temel sınıf ortak parçalarını tüm denetleyicinin testleri için ihtiyacımız olan altyapı kurmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="723ff-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

<span data-ttu-id="723ff-474">Kurulum kodu çoğunlukla aynı kalır.</span><span class="sxs-lookup"><span data-stu-id="723ff-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="723ff-475">Fakes kullanmak yerine, Moq sahte nesneler oluşturmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="723ff-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="723ff-476">Temel sınıf kodu çalışanlar özelliği çağırdığında sahte bir depo döndürmek sahte iş birimini için düzenler.</span><span class="sxs-lookup"><span data-stu-id="723ff-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="723ff-477">Sahte Kurulum geri kalanını her belirli bir senaryoyu ayrılmış test armatürleri içinde gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="723ff-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="723ff-478">Örneğin, test düzeni dizin eylem için eylem sahte deponun FindAll yöntemi çağırdığında, çalışanların bir listesini döndürmek için sahte depo Kurulum.</span><span class="sxs-lookup"><span data-stu-id="723ff-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

<span data-ttu-id="723ff-479">Beklentileri dışında testlerimiz için önce vardı testleri aşağıdakine benzer.</span><span class="sxs-lookup"><span data-stu-id="723ff-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="723ff-480">Ancak, sahte bir çerçeve kaydı özelliği sayesinde biz farklı bir açıdan test yaklaşımını.</span><span class="sxs-lookup"><span data-stu-id="723ff-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="723ff-481">Sonraki bölümde bu yeni perspektif inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="723ff-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="723ff-482">Durum etkileşimini test etme karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="723ff-482">State versus Interaction Testing</span></span>

<span data-ttu-id="723ff-483">Yazılım sahte nesneler ile test etmek için kullanabileceğiniz farklı teknik vardır.</span><span class="sxs-lookup"><span data-stu-id="723ff-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="723ff-484">Durumu test ne bu belgede şu ana kadar uyguladığımız olduğu tabanlı bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="723ff-485">Yazılım durumuyla ilgili test yapar onaylar durumuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="723ff-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="723ff-486">Son test denetleyicisinde bir eylem yöntemi çağrılır ve oluşturmanız gereken onaylama modelle ilgili yapılan.</span><span class="sxs-lookup"><span data-stu-id="723ff-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="723ff-487">Test durumlarını diğer bazı örnekleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="723ff-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="723ff-488">Oluşturma yürütüldükten sonra yeni çalışan nesne deposu içerdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="723ff-489">Dizin yürütüldükten sonra model tüm çalışanların bir listesini tutar doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="723ff-490">Depoyu Sil yürütüldükten sonra belirli bir çalışan içermiyor doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="723ff-491">Başka bir yaklaşım sahte nesneler göreceksiniz olduğunu doğrulamak için *etkileşimleri*.</span><span class="sxs-lookup"><span data-stu-id="723ff-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="723ff-492">Sınama yapar onaylar nesnelerinin durumunu hakkında durumuna bağlı olsa da etkileşim nesnelerin nasıl etkileşimde bulunduğunu hakkında sınama yapar onaylar temel.</span><span class="sxs-lookup"><span data-stu-id="723ff-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="723ff-493">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="723ff-493">For example:</span></span>

-   <span data-ttu-id="723ff-494">Oluşturma yürütüldüğünde denetleyiciyi deponun Ekle yöntemi çağıran doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="723ff-495">Dizin yürütüldüğünde denetleyiciyi deponun FindAll yöntemi çağıran doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="723ff-496">Denetleyiciyi çağıran düzenleme yürütüldüğünde, değişiklikleri kaydetmek için iş yürütme yönteminin birim doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="723ff-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="723ff-497">Çünkü biz içinde koleksiyonlar duvarında açmak değildir ve sayıları doğrulanıyor etkileşim genellikle daha az test verileri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="723ff-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="723ff-498">Bir deponun FindById yöntemi doğru değerle - Ayrıntılar eylemi çağırır biliyorsanız, örneğin, ardından eylem büyük olasılıkla doğru davrandığını.</span><span class="sxs-lookup"><span data-stu-id="723ff-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="723ff-499">Tüm test verileri ayarlamadan FindById döndürmek için Biz bu davranışı doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

<span data-ttu-id="723ff-500">Yukarıdaki test düzeni gerekli tek temel sınıfı tarafından sağlanan Kurulumu kurulur.</span><span class="sxs-lookup"><span data-stu-id="723ff-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="723ff-501">Biz denetleyici eylemini çağırdığınızda Moq sahte depoyla etkileşimleri kaydeder.</span><span class="sxs-lookup"><span data-stu-id="723ff-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="723ff-502">Doğrulama API Moq kullanarak şu denetleyici doğru kimliği değeri ile FindById çağrılır, Moq kapatmamızı isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="723ff-503">Denetleyici yöntemin çağırmayan veya beklenmeyen bir parametreyi yöntemiyle çağrılır, doğrulama yöntemi bir özel durum oluşturur ve test başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="723ff-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="723ff-504">Oluşturma eylemini işleme geçerli iş biriminde çağırır doğrulamak için başka bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="723ff-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="723ff-505">Etkileşim test ile bir tehlike işlemciler üzerinde etkileşimleri belirtmek için ' dir.</span><span class="sxs-lookup"><span data-stu-id="723ff-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="723ff-506">Her etkileşim doğrulamak kayıt ve sahte nesne ile her etkileşim test gelmez doğrulamak için sahte nesnesinin özelliğini denemelisiniz.</span><span class="sxs-lookup"><span data-stu-id="723ff-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="723ff-507">Uygulama Ayrıntıları bazı etkileşimleri ve etkileşimler yalnızca doğrulamalıdır *gerekli* geçerli test karşılamak için.</span><span class="sxs-lookup"><span data-stu-id="723ff-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="723ff-508">Test ettiğiniz sistem ve kişisel mocks veya fakes arasında seçim büyük ölçüde bağlıdır (veya takım) tercihleri.</span><span class="sxs-lookup"><span data-stu-id="723ff-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="723ff-509">Sahte nesneler test double uygulamak için gereken kod miktarını önemli ölçüde azaltabilir, ancak değil herkesin beklentilerini programlama ve etkileşimleri doğrulama deneyimli olduğu.</span><span class="sxs-lookup"><span data-stu-id="723ff-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="723ff-510">Sonuçları</span><span class="sxs-lookup"><span data-stu-id="723ff-510">Conclusions</span></span>

<span data-ttu-id="723ff-511">Bu belgede size ADO.NET varlık çerçevesi veri kalıcılığını kullanırken test edilebilir kod oluşturma için çeşitli yaklaşımlar gösterilen.</span><span class="sxs-lookup"><span data-stu-id="723ff-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="723ff-512">Biz IObjectSet gibi yerleşik soyutlama yararlanabilir&lt;T&gt;, ya da kendi soyutlama IRepository gibi oluşturma&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="723ff-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="723ff-513">Her iki durumda da kalıcı olarak kalması bu soyutlama tüketicilerinin ignorant ve yüksek düzeyde sınanabilir ADO.NET Entity Framework 4.0 POCO desteği sağlar.</span><span class="sxs-lookup"><span data-stu-id="723ff-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="723ff-514">Örtük geç iş ve uygulama hizmeti yüklenmesine izin verir gibi ek EF4 özellikleri ilişkisel bir veri deposu ayrıntılarını hakkında endişelenmeden çalışmak için kodu.</span><span class="sxs-lookup"><span data-stu-id="723ff-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="723ff-515">Son olarak, sahte veya birim testleri içinde sahte oluştururuz soyutlama kolaydır ve bu yüksek oranda yalıtılmış, hızlı çalışması elde etmek için test çift ve güvenilir testler kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="723ff-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="723ff-516">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="723ff-516">Additional Resources</span></span>

-   <span data-ttu-id="723ff-517">Robert c Martin " [tek sorumluluk ilkesini](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="723ff-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="723ff-518">Martin Fowler [desen Kataloğu](http://www.martinfowler.com/eaaCatalog/index.html) gelen *Kurumsal uygulama mimari desenleri*</span><span class="sxs-lookup"><span data-stu-id="723ff-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="723ff-519">Griffin Caprio " [bağımlılık ekleme](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="723ff-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="723ff-520">Veri programlama Blogundaki " [izlenecek yol: Test odaklı geliştirme Entity Framework 4.0 ile](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="723ff-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="723ff-521">Veri programlama Blogundaki " [Entity Framework 4.0 ile kullanarak depo ve iş birimi düzenleri](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="723ff-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="723ff-522">Dave Astels " [BDD giriş](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="723ff-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="723ff-523">Aaron Jensen " [makine belirtimleri ile tanışın](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="723ff-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="723ff-524">Eric Lee " [MSTest ile BDD](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="723ff-524">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="723ff-525">Eric Evans " [etki alanı Odaklı Tasarım](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="723ff-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="723ff-526">Martin Fowler " [Mocks olmayan yer tutucular](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="723ff-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="723ff-527">Martin Fowler " [Test çift](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="723ff-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="723ff-528">Jeremy Miller, " [etkileşimini test etme ve durum](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="723ff-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   [<span data-ttu-id="723ff-529">Moq</span><span class="sxs-lookup"><span data-stu-id="723ff-529">Moq</span></span>](http://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="723ff-530">Biyografi</span><span class="sxs-lookup"><span data-stu-id="723ff-530">Biography</span></span>

<span data-ttu-id="723ff-531">Scott Allen, pluralsight teknik ekibi üyesi ve poshbeauty.com sitesinin OdeToCode.com ' dir.</span><span class="sxs-lookup"><span data-stu-id="723ff-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="723ff-532">Ticari yazılım geliştirme 15 yıl içinde Scott yüksek düzeyde ölçeklenebilir bir ASP.NET web uygulamaları 8-bit katıştırılmış cihazlara kadar her şey için çözümler üzerinde çalışmıştır.</span><span class="sxs-lookup"><span data-stu-id="723ff-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="723ff-533">Scott OdeToCode, blog veya Twitter'da ulaşabilir [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="723ff-533">You can reach Scott on his blog at OdeToCode, or on Twitter at [http://twitter.com/OdeToCode](http://twitter.com/OdeToCode).</span></span>
