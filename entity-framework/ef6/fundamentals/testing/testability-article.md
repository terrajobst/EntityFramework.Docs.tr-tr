---
title: Test edilebilirlik ve Entity Framework 4,0-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 28ec5446ce9faf98fb8fff141832236d70b29daf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181585"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="90709-102">Test edilebilirlik ve Entity Framework 4,0</span><span class="sxs-lookup"><span data-stu-id="90709-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="90709-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="90709-103">Scott Allen</span></span>

<span data-ttu-id="90709-104">Yayımlandı: Mayıs 2010</span><span class="sxs-lookup"><span data-stu-id="90709-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="90709-105">Giriş</span><span class="sxs-lookup"><span data-stu-id="90709-105">Introduction</span></span>

<span data-ttu-id="90709-106">Bu Teknik İnceleme, ADO.NET Entity Framework 4,0 ve Visual Studio 2010 ile nasıl bir test edilebilir kod yazılacağını açıklar.</span><span class="sxs-lookup"><span data-stu-id="90709-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="90709-107">Bu kağıt, test odaklı tasarım (TDD) veya davranış odaklı tasarım (BDD) gibi belirli bir test metodolojisine odaklanmayı denemez.</span><span class="sxs-lookup"><span data-stu-id="90709-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="90709-108">Bunun yerine, bu kağıt, otomatik bir biçimde yalıtmak ve test etmek için, ADO.NET Entity Framework kullanan kodu yazma konusunda odaklanacaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="90709-109">Veri erişim senaryolarında sınamayı kolaylaştıran ortak tasarım düzenlerine bakacağız ve Framework kullanırken bu desenleri nasıl uygulayacağınızı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="90709-110">Ayrıca, bu özelliklerin test edilebilir kodda nasıl çalıştığını görmek için Framework 'ün belirli özelliklerine bakacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="90709-111">Testable kod nedir?</span><span class="sxs-lookup"><span data-stu-id="90709-111">What Is Testable Code?</span></span>

<span data-ttu-id="90709-112">Otomatikleştirilmiş birim testlerini kullanarak bir yazılım parçasını doğrulama özelliği, birçok tercih eden avantaj sunar.</span><span class="sxs-lookup"><span data-stu-id="90709-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="90709-113">Herkes iyi testlerin bir uygulamadaki yazılım kusurlarının sayısını azalttığını ve uygulamanın kalitesini artırdığını bilir, ancak yerinde birim testlerine sahip olmak, yalnızca hata bulmaktan daha fazla ilerler.</span><span class="sxs-lookup"><span data-stu-id="90709-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="90709-114">İyi bir birim test paketi, geliştirme ekibinin zaman kaydetmesine ve oluşturdukları yazılımın denetiminde kalmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="90709-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="90709-115">Bir ekip, yeni gereksinimleri karşılamak için mevcut kodda değişiklik yapabilir, yeniden düzenleme, yeniden düzenleme ve yazılımı yeniden yapılandırabilir ve test paketinin uygulamanın davranışını doğrulayabilirken bir uygulamaya yeni bileşenler ekleyebilirler.</span><span class="sxs-lookup"><span data-stu-id="90709-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="90709-116">Birim testleri, değişikliği kolaylaştırmak ve yazılım bakım artışı arttıkça yazılımın bakım artışına karşı korumak için hızlı geri bildirim döngüsünün bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="90709-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="90709-117">Ancak birim testi bir fiyatla birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="90709-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="90709-118">Bir ekibin birim testlerini oluşturmak ve sürdürmek için zaman yatırmaları gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="90709-119">Bu testleri oluşturmak için gereken çaba miktarı, temel yazılımın **Test edilebilirlik** ile doğrudan ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="90709-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="90709-120">Yazılım ne kadar kolay test edilecek?</span><span class="sxs-lookup"><span data-stu-id="90709-120">How easy is the software to test?</span></span> <span data-ttu-id="90709-121">Göz önünde bulundurularak yazılım tasarlayan bir ekip, takımdan çalışmayan yazılımlarla çalışan ekipten daha hızlı etkili sınamalar oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="90709-122">Microsoft, ADO.NET Entity Framework 4,0 (EF4) 'i test edilebilirlik ile tasarlamıştır.</span><span class="sxs-lookup"><span data-stu-id="90709-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="90709-123">Bu, geliştiricilerin çerçeve kodu için birim testleri yazmaları anlamına gelmez.</span><span class="sxs-lookup"><span data-stu-id="90709-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="90709-124">Bunun yerine, EF4 için test edici amaçları, Framework üzerinde yapı oluşturan test edilebilir kod oluşturmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="90709-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="90709-125">Belirli örneklere bakmadan önce, test edilebilir kodun kalitelerini anlamak oldukça önemlidir.</span><span class="sxs-lookup"><span data-stu-id="90709-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="90709-126">Testable kodunun nitelikleri</span><span class="sxs-lookup"><span data-stu-id="90709-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="90709-127">Test için kolay olan kod her zaman en az iki nitelikler sergiler.</span><span class="sxs-lookup"><span data-stu-id="90709-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="90709-128">İlk olarak, test edilebilir kodun **gözlemek**kolaydır.</span><span class="sxs-lookup"><span data-stu-id="90709-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="90709-129">Bazı girişler kümesi verildiğinde kodun çıkışını gözlemlemek kolay olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="90709-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="90709-130">Örneğin, yöntemi, bir hesaplamanın sonucunu doğrudan döndürdüğünden, aşağıdaki yöntemi kolayca test edin.</span><span class="sxs-lookup"><span data-stu-id="90709-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="90709-131">Yöntem, hesaplanan değeri bir ağ yuvasına, bir veritabanı tablosuna veya aşağıdaki kodla benzer bir dosyaya yazdığında bir yöntemi test etmek zordur.</span><span class="sxs-lookup"><span data-stu-id="90709-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="90709-132">Test, değeri almak için ek iş gerçekleştirmelidir.</span><span class="sxs-lookup"><span data-stu-id="90709-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="90709-133">İkinci olarak, test edilebilir kodun kolayca **yalıtılacağı**.</span><span class="sxs-lookup"><span data-stu-id="90709-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="90709-134">Aşağıdaki sözde kodu yanlış bir test kodu örneği olarak kullanalım.</span><span class="sxs-lookup"><span data-stu-id="90709-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="90709-135">Yöntemi, daha kolay bir şekilde bir sigorta ilkesi geçirebilir ve dönüş değerinin beklenen bir sonuçla eşleşip eşleşmediğini doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="90709-136">Ancak, yöntemi test etmek için doğru şemaya sahip bir veritabanının yüklü olması ve yöntemin bir e-posta göndermeyi denediği durumlarda SMTP sunucusunu yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="90709-137">Birim testi yalnızca yöntemi içindeki hesaplama mantığını doğrulamak istiyor, ancak e-posta sunucusu çevrimdışı olduğu veya veritabanı sunucusu taşındığı için test başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="90709-138">Bu hatalardan her ikisi de testin doğrulamak istediği davranışta ilgisiz değildir.</span><span class="sxs-lookup"><span data-stu-id="90709-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="90709-139">Davranışı yalıtmak zordur.</span><span class="sxs-lookup"><span data-stu-id="90709-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="90709-140">Test edilebilir kod yazmak için gereken yazılım geliştiricileri, genellikle yazdıkları koddaki kaygıları ayırmayı sürdürmek için çaba harcar.</span><span class="sxs-lookup"><span data-stu-id="90709-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="90709-141">Yukarıdaki yöntem iş hesaplamalarına odaklanmalı ve diğer bileşenlere veritabanı ve e-posta uygulama ayrıntılarını devredebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="90709-142">Robert C. MARTIN bu tek sorumluluk Ilkesini çağırır.</span><span class="sxs-lookup"><span data-stu-id="90709-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="90709-143">Bir nesne, bir ilkenin değerini hesaplamak gibi tek ve dar bir sorumluluğu kapsüllemelidir.</span><span class="sxs-lookup"><span data-stu-id="90709-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="90709-144">Diğer tüm veritabanı ve bildirim çalışmaları, başka bir nesnenin sorumluluğu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="90709-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="90709-145">Bu biçimde yazılan kodun tek bir göreve odaklandığı için yalıtmak daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="90709-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="90709-146">.NET ' te, tek sorumluluk Ilkesini izlemek ve yalıtıma ulaşmak için gereken soyutlamalar var.</span><span class="sxs-lookup"><span data-stu-id="90709-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="90709-147">Arabirim tanımlarını kullanabilir ve kodu somut bir tür yerine arabirim soyutlamasını kullanmaya zorlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="90709-148">Bu yazının ilerleyen kısımlarında, yukarıdaki hatalı *örnek gibi bir* yöntemin veritabanıyla iletişim kurabiledikleri gibi arabirimler ile nasıl çalışabilecekleri hakkında bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="90709-149">Bununla birlikte, test sırasında veritabanıyla iletişim kurmayacak, ancak bellekte verileri tutan bir kukla uygulamayı yerine getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="90709-150">Bu kukla uygulama, veri erişim kodundaki veya veritabanı yapılandırmasındaki ilişkisiz sorunlardan kodu yalıtır.</span><span class="sxs-lookup"><span data-stu-id="90709-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="90709-151">Yalıtımın ek avantajları vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="90709-152">Son yöntemdeki iş hesaplamasının yürütülmesi yalnızca birkaç milisaniye sürer, ancak test kendisi ağ etrafında kod attığından ve çeşitli sunuculara ulaşacak kadar birkaç saniye çalışabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="90709-153">Küçük değişiklikleri kolaylaştırmak için birim testlerin hızlı çalışması gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="90709-154">Test ile ilişkili olmayan bir bileşen bir sorun içerdiğinden, birim testleri de yinelenebilir olmalıdır ve başarısız olmaz.</span><span class="sxs-lookup"><span data-stu-id="90709-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="90709-155">Gözlemek ve yalıtmak için çok daha kolay kod yazmak, geliştiricilerin kod için testleri yazma daha kolay zaman harcayacağı, testlerin yürütülmesi için beklediği daha az zaman harcaması ve daha önemlisi, mevcut olmayan hataları daha az zaman harcamaya daha fazla bilgi sahibi olacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="90709-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="90709-156">Test almanın avantajlarından faydalanmanız ve test edilen kodun getirdiği kaliteleri anlamanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="90709-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="90709-157">EF4 ile birlikte çalışarak, verileri bir veritabanına kaydetmek ve daha kolay bir şekilde yalıtmak için bir kod yazmak istiyoruz, ancak ilk olarak veri erişimi için test edilebilir tasarımları tartışmak üzere odaklanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="90709-158">Veri kalıcılığı için tasarım desenleri</span><span class="sxs-lookup"><span data-stu-id="90709-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="90709-159">Daha önce sunulan kötü örneklerden her ikisi de çok fazla sorumluluklara sahipti.</span><span class="sxs-lookup"><span data-stu-id="90709-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="90709-160">İlk hatalı örnek bir hesaplama gerçekleştirmesi *ve* bir dosyaya yazılması gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="90709-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="90709-161">İkinci hatalı örnek *bir veritabanından veri* okuyup bir iş hesaplaması *ve* e-posta gönderecek.</span><span class="sxs-lookup"><span data-stu-id="90709-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="90709-162">Diğer bileşenlere sorunları ve sorumluluğun yetkisini ayıran daha küçük Yöntemler tasarlayarak, test edilebilir kod yazmak için harika bir yöntem elde edersiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="90709-163">Amaç, küçük ve odaklanmış soyutlamalar arasından eylemler oluşturarak işlevselliği dermaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="90709-164">Veri kalıcılığına geldiğinde, bakdığımız küçük ve odaklanmış soyutlamalar, tasarım desenleri olarak belgelendikleri yaygın olarak verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="90709-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="90709-165">Marwler 'in kurumsal uygulama mimarisine ait kitap desenleri, bu desenleri yazdırma sırasında açıklamaya yönelik ilk çalışmamıştı.</span><span class="sxs-lookup"><span data-stu-id="90709-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="90709-166">Bu ADO.NET Entity Framework nasıl uyguladığımızda ve bu desenlerle nasıl çalıştığını göstermemiz için aşağıdaki bölümlerde bu desenlerin kısa bir açıklamasını sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="90709-167">Depo deseninin</span><span class="sxs-lookup"><span data-stu-id="90709-167">The Repository Pattern</span></span>

<span data-ttu-id="90709-168">Fowler, "etki alanı nesnelerine erişmek için koleksiyon benzeri bir arabirim kullanarak etki alanı ve veri eşleme katmanları arasında bir depo" belirtir.</span><span class="sxs-lookup"><span data-stu-id="90709-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="90709-169">Depo deseninin hedefi, verileri veri erişiminin dakikalarından yalıtmak ve daha önceki yalıtımın, test edilebilirlik için gerekli bir nitelik.</span><span class="sxs-lookup"><span data-stu-id="90709-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="90709-170">Yalıtım için anahtar, deponun koleksiyon benzeri bir arabirim kullanarak nesneleri nasıl kullanıma sunduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="90709-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="90709-171">Depoyu kullanmak için yazdığınız mantık, deponun isteğiniz nesneleri nasıl sunacağı konusunda fikir sahibi değildir.</span><span class="sxs-lookup"><span data-stu-id="90709-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="90709-172">Depo bir veritabanıyla konuşabilir veya yalnızca bellek içi bir koleksiyondaki nesneleri döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="90709-173">Tüm kodunuzun bilmeniz gerekir, deponun koleksiyonu sürdürmek için görünmesi ve koleksiyondan nesne alabilir, ekleyebilir ve silebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="90709-174">Mevcut .NET uygulamalarında somut bir depo genellikle aşağıdaki gibi genel bir arabirimden devralınır:</span><span class="sxs-lookup"><span data-stu-id="90709-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="90709-175">EF4 için bir uygulama sağladığımızda arabirim tanımında birkaç değişiklik yapacağız, ancak temel kavram aynı kalır.</span><span class="sxs-lookup"><span data-stu-id="90709-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="90709-176">Kod, bir koşulun değerlendirilmesine göre bir varlık koleksiyonu almak için bu arabirimi uygulayan somut bir depoyu kullanabilir, bir koşul değerlendirmesini temel alan varlıkların koleksiyonunu alabilir veya yalnızca tüm kullanılabilir varlıkları alırlar.</span><span class="sxs-lookup"><span data-stu-id="90709-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="90709-177">Kod ayrıca depo arabirimi aracılığıyla varlık ekleyebilir ve kaldırabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="90709-178">Çalışan nesnelerinin bir ırepository 'si verildiğinde, kod aşağıdaki işlemleri gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="90709-179">Kod bir arabirim (bir çalışan ırepository) kullandığından, kodu farklı arabirim uygulamalarıyla sağlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="90709-180">Bir uygulama, EF4 ve kalıcı nesneleri Microsoft SQL Server bir veritabanında desteklenen bir uygulama olabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="90709-181">Farklı bir uygulama (test sırasında kullandığımız bir adet), çalışan nesnelerinin bellek içi bir listesi tarafından yönetilebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="90709-182">Arabirim, kodda yalıtım elde etmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="90709-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="90709-183">Irepository&lt;T&gt; arabiriminin bir kaydetme işlemi sergilemediğine dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="90709-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="90709-184">Mevcut nesneleri nasıl güncelleştiririz?</span><span class="sxs-lookup"><span data-stu-id="90709-184">How do we update existing objects?</span></span> <span data-ttu-id="90709-185">Kaydet işlemini içeren ırepository tanımlarında gelebilir ve bu depoların uygulamalarının bir nesneyi veritabanına hemen kalıcı hale getirmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="90709-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="90709-186">Ancak birçok uygulamada nesneleri ayrı ayrı kalıcı hale getirmek istemedik.</span><span class="sxs-lookup"><span data-stu-id="90709-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="90709-187">Bunun yerine, farklı depolardan belki de nesneleri hayata getirmek istiyoruz, bu nesneleri iş etkinliklerinin bir parçası olarak değiştirebilir ve sonra tüm nesneleri tek bir atomik işlemin parçası olarak kalıcı hale getiririz.</span><span class="sxs-lookup"><span data-stu-id="90709-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="90709-188">Neyse ki, bu tür davranışa izin veren bir model vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="90709-189">Çalışma birimi deseninin</span><span class="sxs-lookup"><span data-stu-id="90709-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="90709-190">Fowler, bir iş birimini "bir iş işleminden etkilenen nesnelerin listesini koruuyor ve yazma işlemlerini ve eşzamanlılık sorunlarının çözümlendiğini" belirtir.</span><span class="sxs-lookup"><span data-stu-id="90709-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="90709-191">Bu, bir depodan hayata getirdiğimiz nesnelerdeki değişiklikleri izlemek ve iş birimine değişiklikleri kaydetmeye söylediğimiz zaman nesneler üzerinde yaptığımız değişiklikleri sürdürmek için iş biriminin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="90709-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="90709-192">Ayrıca, tüm depolara eklediğimiz yeni nesneleri almak ve nesneleri bir veritabanına eklemek ve ayrıca silmeyi de zorunlu hale gelmesi için iş biriminin sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="90709-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="90709-193">ADO.NET veri kümeleri ile herhangi bir çalışmayı yapmadıysanız, iş deseninin birimini zaten öğrenirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="90709-194">ADO.NET veri kümeleri, güncelleştirmelerimizi, Silinenleri ve DataRow nesneleri ekleme olanımızı takip etmiş ve (TableAdapter 'ın yardımıyla) bir veritabanında yapılan tüm değişiklikleri mutabık hale getirmenize olanak içeriyordu.</span><span class="sxs-lookup"><span data-stu-id="90709-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="90709-195">Ancak, veri kümesi nesneleri temeldeki veritabanının bağlantısı kesik bir alt kümesini modelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="90709-196">İş deseninin birimi aynı davranışı sergiler, ancak veri erişim kodundan yalıtılmış ve veritabanının farkında olmadan iş nesneleri ve etki alanı nesneleriyle çalışır.</span><span class="sxs-lookup"><span data-stu-id="90709-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="90709-197">.NET kodundaki iş birimini modelleyebilir bir soyutlama aşağıdakine benzeyebilir:</span><span class="sxs-lookup"><span data-stu-id="90709-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="90709-198">İş biriminden depo başvurularını açığa çıkarmaktan, tek bir iş nesnesinin bir iş işlemi sırasında gerçekleştirilmiş tüm varlıkları izleyebilmesini sağlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="90709-199">Gerçek bir çalışma birimi için COMMIT yönteminin uygulanması, tüm Magic 'in, bellek içi değişiklikleri veritabanıyla bağdaştırma gerçekleştiği yerdir.</span><span class="sxs-lookup"><span data-stu-id="90709-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="90709-200">Bir IUnitOfWork başvurusu verildiğinde, kod, bir veya daha fazla depodan alınan iş nesnelerinde değişiklik yapabilir ve atomik kaydetme işlemini kullanarak tüm değişiklikleri kaydedebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="90709-201">Geç yük kalıbı</span><span class="sxs-lookup"><span data-stu-id="90709-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="90709-202">Fowler, "ihtiyacınız olan tüm verileri içermeyen, ancak nasıl alınacağını bilen" bir nesneyi belirtmek için Lazy Load adını kullanır.</span><span class="sxs-lookup"><span data-stu-id="90709-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="90709-203">Saydam yavaş yükleme, test kodu yazarken ve ilişkisel veritabanıyla çalışırken önemli bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="90709-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="90709-204">Örnek olarak, aşağıdaki kodu göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="90709-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="90709-205">Zaman kartı koleksiyonu nasıl doldurulur?</span><span class="sxs-lookup"><span data-stu-id="90709-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="90709-206">Olası iki yanıt vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-206">There are two possible answers.</span></span> <span data-ttu-id="90709-207">Bir yanıt, çalışan deposunun bir çalışanı getirmesi istendiğinde, çalışanın ilişkili zaman kartı bilgileriyle birlikte hem çalışanı hem de alacak bir sorgu yayınlar.</span><span class="sxs-lookup"><span data-stu-id="90709-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="90709-208">İlişkisel veritabanlarında, bu genellikle JOIN yan tümcesi içeren bir sorgu gerektirir ve uygulama gereksiniminden daha fazla bilgi alınmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="90709-209">Uygulamanın zaman çizelgeleri özelliğine dokunması gerekmediği ne olursa?</span><span class="sxs-lookup"><span data-stu-id="90709-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="90709-210">İkinci bir yanıt, "isteğe bağlı" zaman kartı özelliğini yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="90709-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="90709-211">Kod, zaman kartı bilgilerini almak için özel API 'Leri çağırmadığından, bu yavaş yükleme örtük ve iş mantığına saydamdır.</span><span class="sxs-lookup"><span data-stu-id="90709-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="90709-212">Kod, gerektiğinde kart bilgilerinin mevcut olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="90709-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="90709-213">Genellikle Yöntem etkinleştirmeleri için çalışma zamanı yakalanmasını içeren, geç yükleme ile ilgili bazı Magic vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="90709-214">Kesintiye uğratan kod, iş mantığını ücretsiz olarak bırakarak, veritabanıyla iletişim sağlanmasından ve zaman kartı bilgilerinin alınmasında sorumludur.</span><span class="sxs-lookup"><span data-stu-id="90709-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="90709-215">Bu yavaş yük Magic, iş kodunun kendisini veri alma işlemlerinden yalıtmasına ve daha kararlı bir koda neden olmasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="90709-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="90709-216">Yavaş yükün dezavantajı, bir uygulamanın kodun ek bir sorgu *yürütebilmesi için* zaman kartı bilgisine ihtiyaç duyacaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="90709-217">Bu pek çok uygulama için bir sorun değildir; ancak, performans duyarlı uygulamalar veya uygulamalar için bir dizi çalışan nesnesinden döngü gerçekleştirin ve döngünün her yinelemesi sırasında zaman kartlarını almak üzere bir sorgu yürütüyordur (genellikle N + 1 olarak ifade edilir) sorgu sorunu), yavaş yükleme bir sürükle.</span><span class="sxs-lookup"><span data-stu-id="90709-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="90709-218">Bu senaryolarda bir uygulama, zaman kartı bilgilerini mümkün olduğunca en verimli şekilde yüklemek isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="90709-219">Neyse ki, bir sonraki bölüme geçiş yaptığımız ve bu desenleri uygulayan, EF4 'in hem örtük yavaş yükleri hem de verimli Eager yüklerini nasıl desteklediğini görüyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="90709-220">Entity Framework desenler uygulama</span><span class="sxs-lookup"><span data-stu-id="90709-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="90709-221">En iyi haberler, son bölümde açıklandığımız tüm Tasarım desenlerinin EF4 ile uygulanması için basittir.</span><span class="sxs-lookup"><span data-stu-id="90709-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="90709-222">Bir basit ASP.NET MVC uygulaması kullanarak çalışanları ve ilgili zaman kartı bilgilerini düzenleyebilir ve görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="90709-223">Aşağıdaki "düz eski CLR nesneleri" (POCOs) kullanılarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="90709-224">Bu sınıf tanımları, EF4 'in farklı yaklaşımlarını ve özelliklerini araştırtığımız kadar biraz değişecektir, ancak amaç bu sınıfları mümkün olduğunca Kalıcılık Ignorant (PI) olarak tutacaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="90709-225">Bir PI nesnesi, sahip olduğu durumun bir veritabanı içinde *nasıl*olduğunu *veya hatta olduğunu*bilmez.</span><span class="sxs-lookup"><span data-stu-id="90709-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="90709-226">PI ve Pocos, test edilebilir yazılımlarla birlikte çalışmaya devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="90709-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="90709-227">Bir POCO yaklaşımı kullanan nesneler, bir veritabanı mevcut olmadan çalışabildiklerinden daha az kısıtlamalı, daha esnektir ve test edilebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="90709-228">POCOs 'un yerinde, Visual Studio 'da bir Varlık Veri Modeli (EDM) oluşturabilir (bkz. Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="90709-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="90709-229">Varlıklarımıza yönelik kod oluşturmak için EDM 'yi kullanmıyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="90709-230">Bunun yerine, sevdiğimiz varlıkları kullanmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="90709-231">Veritabanı şemanızı oluşturmak için yalnızca EDM kullanacağız ve nesneleri veritabanına eşlemek için meta veri EF4 ihtiyaçlarını sunacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![EF test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="90709-233">**Şekil 1**</span><span class="sxs-lookup"><span data-stu-id="90709-233">**Figure 1**</span></span>

<span data-ttu-id="90709-234">Note: önce EDM modelini geliştirmek isterseniz, EDM 'den temiz, POCO kodu oluşturmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="90709-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="90709-235">Bunu, veri programlama ekibi tarafından sunulan bir Visual Studio 2010 uzantısıyla yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="90709-236">Uzantıyı indirmek için, Visual Studio 'daki Araçlar menüsünden Uzantı Yöneticisi ' ni başlatın ve "POCO" şablonlarının çevrimiçi galerisinde arama yapın (bkz. Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="90709-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="90709-237">EF için birkaç POCO şablonu mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="90709-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="90709-238">Şablonu kullanma hakkında daha fazla bilgi için, bkz. " [Izlenecek yol: POCO şablonu Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="90709-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![EF test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="90709-240">**Şekil 2**</span><span class="sxs-lookup"><span data-stu-id="90709-240">**Figure 2**</span></span>

<span data-ttu-id="90709-241">Bu POCO başlangıç noktasından, test edilebilir kodda iki farklı yaklaşım keşfedecektir.</span><span class="sxs-lookup"><span data-stu-id="90709-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="90709-242">İlk yaklaşım, iş ve depo birimleri uygulamak için Entity Framework API 'den soyutlamalar kullandığından EF yaklaşımını çağırdım.</span><span class="sxs-lookup"><span data-stu-id="90709-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="90709-243">İkinci yaklaşımda kendi özel depo soyutlamalarını oluşturacağız ve ardından her yaklaşımın olumlu ve olumsuz yönlerini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="90709-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="90709-244">EF yaklaşımını inceleyerek başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="90709-245">EF merkezli bir uygulama</span><span class="sxs-lookup"><span data-stu-id="90709-245">An EF Centric Implementation</span></span>

<span data-ttu-id="90709-246">Bir ASP.NET MVC projesinden aşağıdaki denetleyici eylemini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="90709-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="90709-247">Eylem bir çalışan nesnesi alır ve çalışanın ayrıntılı görünümünü görüntülemek için bir sonuç döndürür.</span><span class="sxs-lookup"><span data-stu-id="90709-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="90709-248">Kod test edilebilir mi?</span><span class="sxs-lookup"><span data-stu-id="90709-248">Is the code testable?</span></span> <span data-ttu-id="90709-249">Eylemin davranışının doğrulanması için gereken en az iki test var.</span><span class="sxs-lookup"><span data-stu-id="90709-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="90709-250">İlk olarak, eylemin doğru görünümü döndürdüğünü ve kolay bir test olduğunu doğrulamak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="90709-251">Ayrıca, eylemin doğru çalışanı aldığını doğrulamak için de bir test yazmak istiyoruz ve veritabanını sorgulamak için kodu yürütmeden bunu yapmak istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="90709-252">Test altındaki kodu yalıtmak istediğinizi unutmayın.</span><span class="sxs-lookup"><span data-stu-id="90709-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="90709-253">Yalıtım, veri erişim kodundaki veya veritabanı yapılandırmasındaki bir hata nedeniyle testin başarısız olmamasını güvence altına almayacaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="90709-254">Test başarısız olursa, denetleyici mantığındaki bir hata olduğunu ve bazı alt düzey sistem bileşenlerinden olmadığını biliyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="90709-255">Yalıtım sağlamak için, daha önce depolar ve iş birimleri için sunduğumuz arabirimler gibi bazı soyutlamalar olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="90709-256">Depo deseninin, etki alanı nesneleri ve veri eşleme katmanı arasında aracılık için tasarlandığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="90709-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="90709-257">Bu *SENARYODA EF4 veri* eşleme katmanıdır ve zaten ıobjectset&lt;t&gt; (System. Data. Objects ad alanından) adlı depo benzeri bir soyutlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="90709-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="90709-258">Arabirim tanımı aşağıdaki gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="90709-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="90709-259">IObjectSet&lt;T&gt;, bir depo gereksinimlerini karşılar çünkü bir nesne koleksiyonuna (IEnumerable&lt;T&gt;aracılığıyla) benzer ve sanal koleksiyona nesne ekleme ve kaldırma yöntemleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="90709-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="90709-260">Attach ve Detach yöntemleri, EF4 API 'sinin ek özelliklerini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="90709-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="90709-261">Depolamakümesi&lt;T&gt; depolar için arabirim olarak kullanmak için, depoları birbirine bağlamak üzere bir iş soyutlama birimi gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="90709-262">Bu arabirimin somut bir uygulanması SQL Server konuşacak ve EF4 adresinden ObjectContext sınıfını kullanarak oluşturmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="90709-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="90709-263">ObjectContext sınıfı, EF4 API 'sindeki gerçek iş birimidir.</span><span class="sxs-lookup"><span data-stu-id="90709-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="90709-264">Bir IObjectReference&lt;T&gt; hayata getirme, ObjectContext nesnesinin CreateObjectSet metodunu çağırmak kadar kolaydır.</span><span class="sxs-lookup"><span data-stu-id="90709-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="90709-265">Arka planda çerçeve, somut bir ObjectSet&lt;T&gt;oluşturmak için EDM 'da sağladığımız meta verileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="90709-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="90709-266">İstemci kodunda test kararlılığını korumaya yardımcı olacağı için IObjectSet&lt;T&gt; arabirimini döndürmeyle başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="90709-267">Bu somut uygulama üretimde faydalıdır, ancak sınamayı kolaylaştırmak için IUnitOfWork soyutımızı nasıl kullanacağımız üzerine odaklanmamız gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="90709-268">Test Double değerleri</span><span class="sxs-lookup"><span data-stu-id="90709-268">The Test Doubles</span></span>

<span data-ttu-id="90709-269">Denetleyici eylemini yalıtmak için gerçek iş birimi (bir ObjectContext tarafından desteklenir) ve bir test Double veya "sahte" iş birimi arasında geçiş yapma yeteneğinin olması gerekir (bellek içi işlemler gerçekleştiriliyor).</span><span class="sxs-lookup"><span data-stu-id="90709-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="90709-270">Bu tür bir geçiş yapmak için yaygın yaklaşım, MVC denetleyicisinin bir iş birimi oluşturmasını ve bunun yerine çalışma birimini denetleyiciye bir oluşturucu parametresi olarak iletmektir.</span><span class="sxs-lookup"><span data-stu-id="90709-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="90709-271">Yukarıdaki kod, bağımlılık ekleme için bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="90709-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="90709-272">Denetleyicinin bağımlılığı (iş birimi) oluşturmasına izin vermez, ancak bağımlılığı denetleyiciye ekler.</span><span class="sxs-lookup"><span data-stu-id="90709-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="90709-273">Bir MVC projesinde, bağımlılık ekleme işlemini otomatikleştirmek için bir Control (IOC) kapsayıcısının Inversion ile birlikte özel bir denetleyici fabrikası kullanılması yaygındır.</span><span class="sxs-lookup"><span data-stu-id="90709-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="90709-274">Bu konular bu makalenin kapsamı dışındadır, ancak bu makalenin sonundaki başvuruları izleyerek daha fazla bilgi edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="90709-275">Test için kullandığımız sahte bir iş birimi, aşağıdaki gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="90709-276">Sahte iş biriminin bir iletişim olarak kabul edilen bir özelliği kullanıma sunduğunu fark edin.</span><span class="sxs-lookup"><span data-stu-id="90709-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="90709-277">Bazen, testi kolaylaştıran sahte bir sınıfa özellikler eklemek yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="90709-278">Bu durumda, işlenen özelliği denetleyerek kodun bir iş birimi işlediğini gözlemlemek kolaydır.</span><span class="sxs-lookup"><span data-stu-id="90709-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="90709-279">Ayrıca, bellekte çalışan ve zaman kartı nesneleri tutmak için sahte bir IObjectSet&lt;T&gt; gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="90709-280">Genel türler kullanarak tek bir uygulama sağlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="90709-281">Bu test, işinin çoğunu temel bir diyez kümesi&lt;T&gt; nesnesine devreder.</span><span class="sxs-lookup"><span data-stu-id="90709-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="90709-282">IObjectSet&lt;T&gt; 'in bir sınıf (başvuru türü) olarak T uygulayan genel bir kısıtlama gerektirdiğini ve ayrıca IQueryable&lt;T&gt;uygulamamızı zorleyebileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="90709-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="90709-283">Bir bellek içi toplamanın standart LINQ işleci olan bir IQueryable&lt;T&gt; olarak görünmesini kolay hale getirmek kolaydır.</span><span class="sxs-lookup"><span data-stu-id="90709-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="90709-284">Testler</span><span class="sxs-lookup"><span data-stu-id="90709-284">The Tests</span></span>

<span data-ttu-id="90709-285">Geleneksel birim testleri tek bir MVC denetleyicisindeki tüm eylemlerin tüm testlerini tutacak tek bir test sınıfı kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="90709-286">Bu testleri veya herhangi bir birim testi türünü derlediğiniz bellek içi Fakes 'i kullanarak yazalım.</span><span class="sxs-lookup"><span data-stu-id="90709-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="90709-287">Ancak, bu makalede tek parçalı test sınıfı yaklaşımının önüne geçeceğiz ve testlerimizi belirli bir işlevsellik parçasına odaklanmak üzere gruplandıracağız.</span><span class="sxs-lookup"><span data-stu-id="90709-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span><span data-ttu-id="90709-288">  Örneğin, "yeni çalışan oluştur" test etmek istediğimiz işlevsellik olabilir, bu nedenle yeni bir çalışan oluşturmaktan sorumlu tek denetleyicideki bir eylemi doğrulamak için tek bir test sınıfı kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-288">  For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="90709-289">Tüm bu ayrıntılı test sınıfları için ihtiyacımız olan bazı yaygın kurulum kodları vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="90709-290">Örneğin, her zaman bellek içi depolarımızın ve sahte iş birimimizin oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="90709-291">Ayrıca, bir çalışan denetleyicinin bir örneğine, sahte iş birimi eklenmiş bir örnek de ihtiyacımız var.</span><span class="sxs-lookup"><span data-stu-id="90709-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="90709-292">Bu ortak kurulum kodunu, bir temel sınıf kullanarak test sınıfları genelinde paylaşacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="90709-293">Temel sınıfta kullandığımız "nesne anne", test verileri oluşturmaya yönelik bir ortak modeldir.</span><span class="sxs-lookup"><span data-stu-id="90709-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="90709-294">Bir nesne Anne, test varlıklarını birden çok test armatürleri genelinde kullanılmak üzere örneklemek için Fabrika yöntemleri içerir.</span><span class="sxs-lookup"><span data-stu-id="90709-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="90709-295">EmployeeControllerTestBase öğesini bir dizi test armakodu için temel sınıf olarak kullanabiliriz (bkz. Şekil 3).</span><span class="sxs-lookup"><span data-stu-id="90709-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="90709-296">Her test armatürü, belirli bir denetleyici eylemini test eder.</span><span class="sxs-lookup"><span data-stu-id="90709-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="90709-297">Örneğin, bir test armatürü bir HTTP GET isteği sırasında kullanılan oluşturma eyleminin test edilmesine odaklanacaktır (bir çalışan oluşturmak için görünümü görüntülemek için) ve farklı bir armatürü, bir HTTP POST isteğinde kullanılan oluşturma eylemine odaklanacaktır ( bir çalışan oluşturmak için Kullanıcı).</span><span class="sxs-lookup"><span data-stu-id="90709-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="90709-298">Her türetilmiş sınıf, yalnızca belirli bağlamında gerekli olan kurulumdan sorumludur ve belirli test bağlamı için sonuçları doğrulamak üzere gereken onayları sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![EF test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="90709-300">**Şekil 3**</span><span class="sxs-lookup"><span data-stu-id="90709-300">**Figure 3**</span></span>

<span data-ttu-id="90709-301">Burada sunulan adlandırma kuralı ve test stili, test edilebilir kodu için gerekli değildir; tek bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="90709-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="90709-302">Şekil 4 ' te, Visual Studio 2010 için Jet Brains ReSharper Test Çalıştırıcısı eklentisinde çalışan testler gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="90709-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![EF test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="90709-304">**Şekil 4**</span><span class="sxs-lookup"><span data-stu-id="90709-304">**Figure 4**</span></span>

<span data-ttu-id="90709-305">Paylaşılan kurulum kodunu işlemek için bir temel sınıf ile, her bir denetleyici eylemi için birim testleri küçük ve yazma kolaydır.</span><span class="sxs-lookup"><span data-stu-id="90709-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="90709-306">Testler hızla yürütülecektir (bellek içi işlemleri gerçekleştirdiğimiz için) ve ilişkisiz altyapı veya çevresel sorunlar nedeniyle başarısız olmamalıdır (test edilen birimi yalıtdığımız için).</span><span class="sxs-lookup"><span data-stu-id="90709-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="90709-307">Bu testlerde, temel sınıf kurulum işinin çoğunu yapar.</span><span class="sxs-lookup"><span data-stu-id="90709-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="90709-308">Temel sınıf oluşturucunun bellek içi depoyu, sahte bir iş birimini ve EmployeeController sınıfının bir örneğini oluşturduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="90709-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="90709-309">Test sınıfı bu temel sınıftan türetilir ve Create metodunu test etme özelliklerine odaklanır.</span><span class="sxs-lookup"><span data-stu-id="90709-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="90709-310">Bu durumda, Ayrıntılar herhangi bir birim testi yordamında göreceğiniz "düzenleme, işlem ve onaylama" adımlarına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="90709-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="90709-311">Gelen verilerin benzetimini yapmak için bir Yeniçalışan nesnesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="90709-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="90709-312">EmployeeController 'ın oluşturma eylemini çağırın ve Yeniçalışan içinde geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="90709-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="90709-313">Oluşturma eyleminin beklenen sonuçları (çalışanın depoda göründüğünü) ürettiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="90709-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="90709-314">Oluşturduğumuz, EmployeeController eylemlerini test etmemizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="90709-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="90709-315">Örneğin, testler için aynı temel kurulumu oluşturmak üzere test temel sınıfından kalıtımla aldığımız çalışan denetleyicinin Dizin eylemi için testler yazdığımızda.</span><span class="sxs-lookup"><span data-stu-id="90709-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="90709-316">Temel sınıf, bellek içi depoyu, sahte iş birimini ve EmployeeController örneğini oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="90709-317">Dizin eyleminin testleri yalnızca dizin eylemini çağırmaya ve eylemin döndürdüğü modelin kalitelerini test etmeye odaklanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="90709-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="90709-318">Bellek içi Fakes ile oluşturmakta olduğumuz testler, yazılımın *durumunun* test edilmesine yönelik olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="90709-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="90709-319">Örneğin, oluşturma eylemini sınarken, oluşturma eylemi yürütüldükten sonra deponun durumunu incelemek istiyoruz. depo yeni çalışanı mi tutar?</span><span class="sxs-lookup"><span data-stu-id="90709-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="90709-320">Daha sonra etkileşim tabanlı teste bakacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="90709-321">Etkileşim tabanlı test, test altındaki kodun nesnelerimiz üzerinde doğru yöntemleri çağırırmasını ve doğru parametreleri geçtiğini ister.</span><span class="sxs-lookup"><span data-stu-id="90709-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="90709-322">Şimdilik, başka bir tasarım düzenine (yavaş yük) geçeceğiz.</span><span class="sxs-lookup"><span data-stu-id="90709-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="90709-323">Eager yükleme ve geç yükleme</span><span class="sxs-lookup"><span data-stu-id="90709-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="90709-324">ASP.NET MVC web uygulamasında bir noktada, bir çalışanın bilgilerini göstermek ve çalışanın ilişkili zaman kartlarını eklemek isteyebilirler.</span><span class="sxs-lookup"><span data-stu-id="90709-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="90709-325">Örneğin, çalışanın adını ve sistemdeki toplam zaman kartı sayısını gösteren bir zaman kartı Özet görüntüsü olabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="90709-326">Bu özelliği uygulamak için birkaç yaklaşım olabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="90709-327">Projeksiyon</span><span class="sxs-lookup"><span data-stu-id="90709-327">Projection</span></span>

<span data-ttu-id="90709-328">Özet oluşturmaya yönelik kolay bir yaklaşım, görünümde görüntülenmesini istediğimiz bilgilere adanmış bir model oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="90709-329">Bu senaryoda, model aşağıdaki gibi görünebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="90709-330">EmployeeSummaryViewModel bir varlık değil, başka bir deyişle veritabanında kalıcı hale getirmek istediğimiz bir şey değildir.</span><span class="sxs-lookup"><span data-stu-id="90709-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="90709-331">Bu sınıfı yalnızca, verileri kesin belirlenmiş bir şekilde görünüme karıştırmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="90709-332">Görünüm modeli bir veri aktarımı nesnesi (DTO) gibi bir davranış (yöntem yok) içerdiğinden, yalnızca özellikler içeriyor.</span><span class="sxs-lookup"><span data-stu-id="90709-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="90709-333">Özellikler, taşınması gereken verileri tutacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="90709-334">LINQ 'ın standart projeksiyon işlecini (Select işleci) kullanarak bu görünüm modelini oluşturmak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="90709-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="90709-335">Yukarıdaki koda yönelik iki önemli özellik vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-335">There are two notable features to the above code.</span></span> <span data-ttu-id="90709-336">İlk: daha kolay bir şekilde izlemek ve yalıtmak için kodun test edilmesi kolaydır.</span><span class="sxs-lookup"><span data-stu-id="90709-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="90709-337">Select işleci, gerçek iş biriminde olduğu gibi bellek içi faflarımızla aynı zamanda çalışır.</span><span class="sxs-lookup"><span data-stu-id="90709-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="90709-338">İkinci önemli özelliği, kodun çalışanların ve zaman kartı bilgilerini bir araya getirmek için tek ve verimli bir sorgu oluşturmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="90709-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="90709-339">Çalışan bilgilerini ve zaman kartı bilgilerini özel API 'Ler kullanmadan aynı nesneye yükledik.</span><span class="sxs-lookup"><span data-stu-id="90709-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="90709-340">Kod, yalnızca, bellek içi veri kaynaklarına ve uzak veri kaynaklarına karşı çalışan standart LINQ işleçleri kullanmak için gereken bilgileri ifade ediyor.</span><span class="sxs-lookup"><span data-stu-id="90709-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="90709-341">EF4, LINQ sorgusu ve C\# derleyicisi tarafından oluşturulan ifade ağaçlarını tek ve verimli bir T-SQL sorgusuna çevirebildi.</span><span class="sxs-lookup"><span data-stu-id="90709-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="90709-342">Bir görünüm modeliyle veya DTO nesnesiyle çalışmak istemediğimiz zaman, ancak gerçek varlıklarla, başka zamanlar da vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="90709-343">Bir çalışana *ve* çalışanların zaman kartlarına ihtiyacım olduğunu öğrendiğimiz zaman, ilgili verileri engellemeyen ve verimli bir şekilde yükleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="90709-344">Açık Eager yüklemesi</span><span class="sxs-lookup"><span data-stu-id="90709-344">Explicit Eager Loading</span></span>

<span data-ttu-id="90709-345">İlgili varlık bilgilerini daha fazla yüklemek istediğimiz için, iş mantığı için bazı mekanizmaları (veya bu senaryoda, denetleyici eylem mantığı), isteğini depoya ifade etmek istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="90709-346">EF4 ObjectQuery&lt;T&gt; sınıfı, bir sorgu sırasında alınacak ilgili nesneleri belirtmek için bir Include yöntemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="90709-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="90709-347">EF4 ObjectContext 'in, ObjectQuery&lt;T&gt;devralan somut ObjectSet&lt;T&gt; sınıfı aracılığıyla varlıkları açığa çıkardığı unutulmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="90709-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span><span data-ttu-id="90709-348">  Denetleyici eylemimizde ObjectSet&lt;T&gt; başvurularını kullandığımızda, her çalışana yönelik bir zaman kartı bilgileri yüklemesi belirtmek için aşağıdaki kodu yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-348">  If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="90709-349">Ancak, kod test etmemiz yaptığımız için, gerçek iş sınıfının dışından ObjectSet&lt;T&gt; kullanıma sunmuyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="90709-350">Bunun yerine, sahte kümesi&lt;T&gt; arabirimine güveniyoruz, ancak IObjectSet&lt;T&gt; bir Içerme yöntemi tanımlamaz.</span><span class="sxs-lookup"><span data-stu-id="90709-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="90709-351">LINQ 'ın, kendi ekleme işleçlerimizi oluşturduğumuz.</span><span class="sxs-lookup"><span data-stu-id="90709-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="90709-352">Bu Include işlecinin, IObjectSet&lt;T&gt;yerine IQueryable&lt;T&gt; için bir genişletme yöntemi olarak tanımlandığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="90709-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="90709-353">Bu, yöntemi IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;ve ObjectSet&lt;T&gt;dahil olmak üzere daha geniş bir dizi olası tür ile kullanma olanağı sunar.</span><span class="sxs-lookup"><span data-stu-id="90709-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="90709-354">Temeldeki sıra, gerçek bir EF4 ObjectQuery&lt;T&gt;olmadığından, bir sorun yoktur ve Içerme işleci hiçbir şey değildir.</span><span class="sxs-lookup"><span data-stu-id="90709-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="90709-355">Temeldeki sıra bir ObjectQuery&lt;T&gt; (ya da ObjectQuery&lt;T&gt;) ise, EF4 ek veriler için gereksinimimizi görür ve uygun SQL sorgusunu *formüllendiriyor* .</span><span class="sxs-lookup"><span data-stu-id="90709-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="90709-356">Bu yeni işleçle birlikte, depodan bir zaman kartı bilgileri yüklemesi isteğinde bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="90709-357">Gerçek bir ObjectContext 'e karşı çalıştırıldığında, kod aşağıdaki tek sorguyu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="90709-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="90709-358">Sorgu, çalışan nesnelerini bir yolculuğa alacak ve zaman kartları özelliğini tamamen dolduracak şekilde veritabanından yeterli bilgi toplar.</span><span class="sxs-lookup"><span data-stu-id="90709-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="90709-359">Harika haberler, eylem yöntemi içindeki koddur tamamen test edilebilir olarak kalır.</span><span class="sxs-lookup"><span data-stu-id="90709-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="90709-360">Ekleme işlecini desteklemek için Fakes 'e yönelik ek özellikler sağlamamız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="90709-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="90709-361">Kötü haberler, kalıcılık Ignorant 'i tutmak istediğimiz kodun içinde Include işlecini kullandık.</span><span class="sxs-lookup"><span data-stu-id="90709-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="90709-362">Bu, test edilebilir kod oluştururken değerlendirmeniz gereken dengetür bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="90709-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="90709-363">Kalıcı kaygıların, performans hedeflerini karşılamak için depo soyutlaması dışında sızmasına izin vermek için gereken durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="90709-364">Eager yükleme alternatifi geç yükleme ' dir.</span><span class="sxs-lookup"><span data-stu-id="90709-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="90709-365">Yavaş yükleme, iş kodumuza ilişkili verilerin gereksinimini açıkça duyurmak için ihtiyaç *duymadığımız* anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="90709-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="90709-366">Bunun yerine, uygulama içinde varlıklarımızı kullanıyoruz ve ek veriler gerekliyse Entity Framework verileri isteğe bağlı olarak yükler.</span><span class="sxs-lookup"><span data-stu-id="90709-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="90709-367">Geç yükleme</span><span class="sxs-lookup"><span data-stu-id="90709-367">Lazy Loading</span></span>

<span data-ttu-id="90709-368">Bir iş mantığı parçasının hangi verileri suneceğimizi bilmeyen bir senaryoya çok daha kolay bir şekilde düşünün.</span><span class="sxs-lookup"><span data-stu-id="90709-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="90709-369">Mantığın bir çalışan nesnesine ihtiyacı olduğunu biliyoruz, ancak bu yollardan bazılarının çalışanla ilgili zaman kartı bilgileri gerektirmesi ve bazıları olmaması durumunda farklı yürütme yollarına dallanbiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="90709-370">Bu gibi senaryolar, verilerin genel olarak gerekli bir şekilde göründüğünden, örtük yavaş yükleme için mükemmeldir.</span><span class="sxs-lookup"><span data-stu-id="90709-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="90709-371">Ertelenmiş yükleme olarak da bilinen yavaş yükleme, bazı gereksinimleri varlık nesnelerimize yerleştirir.</span><span class="sxs-lookup"><span data-stu-id="90709-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="90709-372">Doğru Kalıcılık kalıcılığı olan POCOs, kalıcılık katmanından herhangi bir gereksinim sağlamaz, ancak gerçek Kalıcılık Ignorance 'in elde etmek neredeyse olanaksız hale geliyordu.</span><span class="sxs-lookup"><span data-stu-id="90709-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span><span data-ttu-id="90709-373">  Bunun yerine, kalıcılık Ignorance ' i göreli derece ölçyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-373">  Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="90709-374">Bir kalıcılık odaklı taban sınıftan devralması gerekiyorsa veya POCOs 'ta geç yükleme elde etmek için özel bir koleksiyon kullanmanız talihsiz.</span><span class="sxs-lookup"><span data-stu-id="90709-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="90709-375">Neyse ki, EF4 daha az zorlayıcıdır bir çözüme sahiptir.</span><span class="sxs-lookup"><span data-stu-id="90709-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="90709-376">Neredeyse algılanamaz</span><span class="sxs-lookup"><span data-stu-id="90709-376">Virtually Undetectable</span></span>

<span data-ttu-id="90709-377">POCO nesneleri kullanılırken EF4, varlıklar için çalışma zamanı proxy 'leri dinamik olarak oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="90709-378">Bu proxy 'ler, gerçekleştirilmiş POCOs ' i daha fazla sarın ve her bir özellik al ve ayarla işlemini ek iş gerçekleştirecek şekilde kesintiye getirerek ek hizmetler sağlar.</span><span class="sxs-lookup"><span data-stu-id="90709-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="90709-379">Bu tür bir hizmet, ardığımız yavaş yükleme özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="90709-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="90709-380">Başka bir hizmet, program bir varlığın özellik değerlerini değiştirdiğinde kaydedebilen etkili bir değişiklik izleme mekanizmasıdır.</span><span class="sxs-lookup"><span data-stu-id="90709-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="90709-381">Değişiklik listesi, GÜNCELLEŞTIRME komutlarını kullanarak değiştirilen varlıkların kalıcı hale getirilmesi için SaveChanges yöntemi sırasında ObjectContext tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="90709-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="90709-382">Bununla birlikte, bu proxy 'lerin çalışması için bir varlık üzerinde özellik al ve ayarla işlemlerine bağlamak için bir yol gerekir ve proxy 'ler sanal üyeleri geçersiz kılarak bu amaca ulaşırlar.</span><span class="sxs-lookup"><span data-stu-id="90709-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="90709-383">Bu nedenle, örtük yavaş yükleme ve etkili değişiklik izleme sağlamak istiyorsam, POCO sınıfı tanımlarımıza dönmemiz ve özellikleri sanal olarak işaretlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="90709-384">Yine de çalışan varlığının, genellikle Kalıcılık Ignorant olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="90709-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="90709-385">Tek gereksinim, sanal üyelerin kullanılması ve bu kodun test edilebilirlik etkilemez.</span><span class="sxs-lookup"><span data-stu-id="90709-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="90709-386">Herhangi bir özel taban sınıftan türetmemiz gerekmez, hatta yavaş yüklemeye adanmış özel bir koleksiyon kullanın.</span><span class="sxs-lookup"><span data-stu-id="90709-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="90709-387">Kodun gösterdiği gibi, ICollection&lt;T&gt; uygulayan tüm sınıflar ilgili varlıkları tutmak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="90709-388">İş birimimizin içinde yapabilmemiz gereken bir küçük değişiklik de vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="90709-389">Doğrudan bir ObjectContext nesnesiyle çalışırken geç yükleme varsayılan olarak *kapalıdır* .</span><span class="sxs-lookup"><span data-stu-id="90709-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="90709-390">Bu özelliği, ertelenmiş yüklemeyi etkinleştirmek için ContextOptions özelliğinde ayarlayabiliriz ve her yerde yavaş yüklemeyi etkinleştirmek istiyorsam gerçek iş birimimizin içinde ayarlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="90709-391">Örtük yavaş yükleme etkin olduğunda, uygulama kodu bir çalışanı ve çalışanın ilişkili zaman kartlarını kullanarak ek verileri yüklemek için gerekli olan işin kalan çalışmalarından haberdar olabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="90709-392">Yavaş yükleme, uygulama kodunu yazmayı daha kolay hale getirir ve proxy Magic ile kod tamamen test edilebilir kalır.</span><span class="sxs-lookup"><span data-stu-id="90709-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="90709-393">Çalışma birimi için bellek içi Fakes, bir test sırasında gerektiğinde ilgili verileri içeren sahte varlıkları önyükleyebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="90709-394">Bu noktada, IObjectSet&lt;T&gt; kullanarak depo oluşturma konusunda ilgilenmeniz ve kalıcılık çerçevesinin tüm işaretlerini gizlemek için soyutlamaları göz atacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="90709-395">Özel depolar</span><span class="sxs-lookup"><span data-stu-id="90709-395">Custom Repositories</span></span>

<span data-ttu-id="90709-396">Bu makaledeki iş birimi tasarım deseninin ilk olarak sunulduğunu, iş biriminin nasıl görünebileceğini görmek için bazı örnek kodlar sağladık.</span><span class="sxs-lookup"><span data-stu-id="90709-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="90709-397">Bu özgün fikri, birlikte çalıştığımız çalışan ve çalışan zaman kartı senaryosunu kullanarak yeniden sunalım.</span><span class="sxs-lookup"><span data-stu-id="90709-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="90709-398">Bu iş birimi ile son bölümde oluşturduğumuz iş birimi arasındaki birincil fark, bu iş biriminin EF4 çerçevesinden herhangi bir soyutlama kullanmadığında (bir IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="90709-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="90709-399">IObjectSet&lt;T&gt; bir depo arabirimi olarak iyi çalışmaktadır, ancak açığa çıkardığı API, uygulama gereksinimlerimize uygun şekilde hizalanmayabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="90709-400">Bu yaklaşan yaklaşımda, özel bir ırepository&lt;T&gt; soyutlama kullanan depoları temsil edeceğiz.</span><span class="sxs-lookup"><span data-stu-id="90709-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="90709-401">Test odaklı tasarım, davranış odaklı tasarım ve etki alanı odaklı Yöntemler tasarımını izleyen birçok geliştirici, bazı nedenlerle ırepository&lt;T&gt; yaklaşımını tercih eder.</span><span class="sxs-lookup"><span data-stu-id="90709-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="90709-402">İlk olarak, ırepository&lt;T&gt; arabirimi bir "bozulma önleme" katmanını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="90709-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="90709-403">Etki alanı odaklı tasarım defterindeki Eric Evans 'Lar tarafından açıklandığı gibi, bir kalıcılık API 'SI gibi, etki alanı kodunuzu altyapı API 'Lerinden uzakta tutar.</span><span class="sxs-lookup"><span data-stu-id="90709-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="90709-404">İkincisi, geliştiriciler, bir uygulamanın tam ihtiyaçlarını karşılayan (testleri yazarken bulunur), depoya Yöntemler oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="90709-405">Örneğin, genellikle bir KIMLIK değeri kullanarak tek bir varlığı bulduğumuz için, depo arabirimine bir Findbyıd yöntemi ekleyebiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span><span data-ttu-id="90709-406">  Irepository&lt;T&gt; tanımımız aşağıdaki gibi görünür.</span><span class="sxs-lookup"><span data-stu-id="90709-406">  Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="90709-407">Varlık koleksiyonlarını göstermek için IQueryable&lt;T&gt; arabirimini kullanmaya geri başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="90709-408">IQueryable&lt;T&gt;, LINQ Expression ağaçlarının EF4 sağlayıcısına akmasını ve sağlayıcıya sorgunun bütünsel görünümünü vermesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="90709-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="90709-409">İkinci bir seçenek IEnumerable&lt;T&gt;döndürmek, bu da EF4 LINQ sağlayıcının yalnızca deponun içinde yerleşik olan ifadeleri göremeyeceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="90709-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="90709-410">Deponun dışında yapılan herhangi bir gruplandırma, sıralama ve projeksiyon, veritabanına gönderilen SQL komutuna uygulanmaz ve bu da performansa zarar verebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="90709-411">Diğer taraftan, yalnızca IEnumerable&lt;T&gt; sonuçları döndüren bir depo, yeni bir SQL komutu ile sizi hiçbir şekilde hiçbir şekilde hiçbir şekilde sürmez.</span><span class="sxs-lookup"><span data-stu-id="90709-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="90709-412">Her iki yaklaşım da çalışacaktır ve her iki yaklaşım da testable olarak kalır.</span><span class="sxs-lookup"><span data-stu-id="90709-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="90709-413">Genel türler ve EF4 ObjectContext API 'SI kullanılarak ırepository&lt;T&gt; arabiriminin tek bir uygulamasını sağlamak basittir.</span><span class="sxs-lookup"><span data-stu-id="90709-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="90709-414">Irepository&lt;T&gt; yaklaşımı, bir istemcinin bir varlığa ulaşmak için bir yöntem çağırması gerektiğinden sorgularımızda bazı ek denetimler elde etmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="90709-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="90709-415">Yöntemi içinde, uygulama kısıtlamalarını zorlamak için ek denetimler ve LINQ işleçleri sağlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="90709-416">Arabirimin genel tür parametresinde iki kısıtlama olduğunu fark edin.</span><span class="sxs-lookup"><span data-stu-id="90709-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="90709-417">İlk kısıtlama, ObjectSet&lt;T&gt;için gerekli olan bir sınıftır ve ikinci kısıtlama varlıklarımızı uygulama için oluşturulan bir soyutlama olan IEntity 'ı uygulayacak şekilde zorlar.</span><span class="sxs-lookup"><span data-stu-id="90709-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="90709-418">IEntity arabirimi, varlıkların okunabilir bir ID özelliğine sahip olmasını zorlar ve daha sonra bu özelliği Findbyıd yönteminde kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="90709-419">IEntity aşağıdaki kodla tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="90709-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="90709-420">Varlıklarımızın bu arabirimi uygulaması gerektiğinden, IEntity küçük bir kalıcılığı ihlal edilebilir olarak düşünülebilir.</span><span class="sxs-lookup"><span data-stu-id="90709-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="90709-421">Kalıcılık Ignorance 'in, bir denge hakkında olduğunu ve birçok Findbyıd işlevinin, arabirim tarafından uygulanan kısıtlamayı aşacak şekilde olacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="90709-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="90709-422">Arabirimin test edilebilirlik üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="90709-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="90709-423">Canlı bir ırepository 'nin örneklenmesi&lt;T&gt; bir EF4 ObjectContext gerektirir, bu nedenle somut bir iş uygulaması biriminin örnek oluşturmayı yönetmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="90709-424">Özel depoyu kullanma</span><span class="sxs-lookup"><span data-stu-id="90709-424">Using the Custom Repository</span></span>

<span data-ttu-id="90709-425">Özel havuzumuzu kullanmak, IObjectSet&lt;T&gt;tabanlı depoyu kullanmaktan önemli ölçüde farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="90709-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="90709-426">LINQ işleçlerini doğrudan bir özelliğine uygulamak yerine, önce bir IQueryable&lt;T&gt; başvurusunu almak için bir deponun yöntemlerini çağırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="90709-427">Daha önce uyguladığımız özel Içerme işlecinin değişiklik olmadan çalışabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="90709-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="90709-428">Deponun Findbyıd yöntemi, tek bir varlığı almaya çalışan eylemlerden yinelenen mantığı kaldırır.</span><span class="sxs-lookup"><span data-stu-id="90709-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="90709-429">İncelediğimiz iki yaklaşımın test edilebilirlik açısından önemli bir fark yoktur.</span><span class="sxs-lookup"><span data-stu-id="90709-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="90709-430">HashSet&lt;çalışan&gt; tarafından desteklenen somut sınıflar oluşturarak (son bölümde yaptığımız gibi) ırepository&lt;T&gt; sahte uygulamalar sağlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="90709-431">Ancak bazı geliştiriciler, Fakes oluşturmak yerine, sahte nesneler ve sahte nesne çerçeveleri kullanmayı tercih eder.</span><span class="sxs-lookup"><span data-stu-id="90709-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="90709-432">Uygulamamızı test etmek ve sonraki bölümde yer aldığı ve bu farklılıkları ve Fakes arasındaki farkları tartışmak için de moyaları kullanma bölümüne bakacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="90709-433">Moklarla test etme</span><span class="sxs-lookup"><span data-stu-id="90709-433">Testing with Mocks</span></span>

<span data-ttu-id="90709-434">Tek Marwler 'in bir "test Double" araması için farklı yaklaşımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="90709-435">Bir test Double (bir film takip eden çift gibi), testler sırasında gerçek, üretim nesneleri için "Stand" olarak oluşturduğunuz bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="90709-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="90709-436">Oluşturduğumuz bellek içi depolar, SQL Server ile iletişim kuran depolar için Double değerleri test ediyor.</span><span class="sxs-lookup"><span data-stu-id="90709-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="90709-437">Kodu yalıtmak ve testleri hızlı bir şekilde çalıştırmak için birim testlerinde bu testlerin nasıl çift olarak kullanılacağını gördük.</span><span class="sxs-lookup"><span data-stu-id="90709-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="90709-438">Test, geliştirdiğimiz gerçek, çalışma uygulamalarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="90709-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="90709-439">Arka planda her biri, bir dizi somut nesneleri depolar ve bir test sırasında depoyu işlememiz halinde bu koleksiyondan nesne ekler ve kaldırır.</span><span class="sxs-lookup"><span data-stu-id="90709-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="90709-440">Bu şekilde test oluşturmak gibi bazı geliştiriciler, gerçek kod ve çalışma uygulamalarıyla birlikte bu şekilde iki katına çıkarır.</span><span class="sxs-lookup"><span data-stu-id="90709-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span><span data-ttu-id="90709-441">  Bu test, *Fakes*'i çağırdığımız şeydir.</span><span class="sxs-lookup"><span data-stu-id="90709-441">  These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="90709-442">Bunlarla çalışan uygulamalar vardır, ancak üretim kullanımı için yeterince gerçek değildir.</span><span class="sxs-lookup"><span data-stu-id="90709-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="90709-443">Sahte depo aslında veritabanına yazmıyor.</span><span class="sxs-lookup"><span data-stu-id="90709-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="90709-444">Sahte SMTP sunucusu aslında ağ üzerinden e-posta iletisi göndermez.</span><span class="sxs-lookup"><span data-stu-id="90709-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="90709-445">Moları ve Fakes karşılaştırması</span><span class="sxs-lookup"><span data-stu-id="90709-445">Mocks versus Fakes</span></span>

<span data-ttu-id="90709-446">*Sahte*olarak bilinen başka bir test türü vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="90709-447">Fakes çalışma uygulamalarına sahip olsa da, moları hiçbir uygulamayla birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="90709-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="90709-448">Bir sahte nesne çerçevesinin yardımıyla, bu sahte nesneleri çalışma zamanında oluşturur ve bunları test Double olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="90709-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="90709-449">Bu bölümde, açık kaynak sahte işlem Framework moq ' ını kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="90709-450">Bir çalışan deposu için dinamik olarak bir test Double oluşturmak üzere moq kullanmanın basit bir örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="90709-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="90709-451">Bir ırepository&lt;çalışan&gt; uygulaması için moq soruyoruz ve dinamik olarak bir tane oluşturur.</span><span class="sxs-lookup"><span data-stu-id="90709-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="90709-452">Irepository&lt;çalışan nesneye,&gt;, sahte&lt;T&gt; nesnesinin nesne özelliğine erişerek ulaşacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="90709-453">Denetleyicilerimize geçebilmemiz için bu iç nesne budur ve bu, bir test Double veya gerçek depo olduğunu bilmez.</span><span class="sxs-lookup"><span data-stu-id="90709-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="90709-454">Nesneleri, gerçek bir uygulamayla bir nesne üzerinde çağırırız gibi, nesne üzerinde Yöntemler çağırabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="90709-455">Add metodunu çağırdığımızda, sahte deponun ne yapacağına merak etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="90709-456">Sahte nesnenin arkasında hiçbir uygulama olmadığından, ekleme işlemi hiçbir şey yapmaz.</span><span class="sxs-lookup"><span data-stu-id="90709-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="90709-457">Yazdığımız Fakes gibi sahnelerin arkasında somut bir koleksiyon yoktur, bu nedenle çalışan atılır.</span><span class="sxs-lookup"><span data-stu-id="90709-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="90709-458">Findbyıd 'nin dönüş değeri ne?</span><span class="sxs-lookup"><span data-stu-id="90709-458">What about the return value of FindById?</span></span> <span data-ttu-id="90709-459">Bu durumda, sahte nesne yalnızca yapabileceği şeyi yapar; bu, varsayılan bir değer döndürür.</span><span class="sxs-lookup"><span data-stu-id="90709-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="90709-460">Bir başvuru türü (bir çalışan) döndürtiğimiz için, dönüş değeri null bir değerdir.</span><span class="sxs-lookup"><span data-stu-id="90709-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="90709-461">Kneztal, daha az ses alabilir; Bununla birlikte, bu konuda daha fazla bilgi edindiğimiz iki ek özellik vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="90709-462">İlk olarak, moq çerçevesi, sahte nesne üzerinde yapılan tüm çağrıları kaydeder.</span><span class="sxs-lookup"><span data-stu-id="90709-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="90709-463">Kodun ilerleyen kısımlarında, Add yöntemini veya herkes Findbyıd yöntemini çağırırsam moq 'a sorun.</span><span class="sxs-lookup"><span data-stu-id="90709-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="90709-464">Daha sonra, bu "kara kutu" kayıt özelliğini testlerde nasıl kullanacağımız hakkında daha fazla bilgi edineceksiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="90709-465">İkinci harika özellik, bir sahte nesneyi *beklentilerle*programlamak Için moq 'yi nasıl kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="90709-466">Bir beklenti, sahte nesnesine verilen etkileşime nasıl yanıt verileceğini söyler.</span><span class="sxs-lookup"><span data-stu-id="90709-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="90709-467">Örneğin, bir beklentisini bir beklenemizi programlarız ve birisi Findbyıd 'yi çağırdığında bir çalışan nesnesi döndürmesini söyleriz.</span><span class="sxs-lookup"><span data-stu-id="90709-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="90709-468">Moq çerçevesi, bu beklentileri programlamak için bir kurulum API 'SI ve lambda ifadeleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="90709-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="90709-469">Bu örnekte, moq 'ın bir depoyu dinamik olarak oluşturmasını ve sonra depoyu bir beklentisiyle programlıyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="90709-470">Bu beklentiler, bir Kullanıcı Findbyıd metodunu 5 değerini geçirerek bir kimlik değeri 5 olan yeni bir çalışan nesnesi döndürmesini söyler.</span><span class="sxs-lookup"><span data-stu-id="90709-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="90709-471">Bu test geçirilir ve sahte ırepository&lt;T&gt;için tam bir uygulama oluşturmamız gerekmiyor.</span><span class="sxs-lookup"><span data-stu-id="90709-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="90709-472">Daha önce yazdığımız testleri tekrar ziyaret edelim ve bunları Fakes yerine kları kullanacak şekilde yeniden ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="90709-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="90709-473">Daha önce olduğu gibi, denetleyicinin tüm testleri için ihtiyacımız olan yaygın altyapı parçalarını ayarlamak için bir temel sınıf kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="90709-474">Kurulum kodu çoğunlukla aynı kalır.</span><span class="sxs-lookup"><span data-stu-id="90709-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="90709-475">Fakes 'i kullanmak yerine, model nesneleri oluşturmak için moq kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="90709-476">Temel sınıf, kod çalışanlar özelliğini çağırdığında bir sahte depo döndürecek şekilde, sahte iş birimi için düzenler.</span><span class="sxs-lookup"><span data-stu-id="90709-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="90709-477">Sahte kurulumun geri kalanı, her bir senaryoya ayrılan test armatürleri içinde gerçekleşmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="90709-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="90709-478">Örneğin, Dizin eyleminin test armatürü, bir işlem, sahte deponun FindAll yöntemini çağırdığında çalışanların bir listesini döndürecek şekilde ayarlar.</span><span class="sxs-lookup"><span data-stu-id="90709-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="90709-479">Beklentiler dışında, sınamalarımız, daha önce yaptığımız testlere benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="90709-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="90709-480">Bununla birlikte, bir sahte çerçevenin kayıt yeteneği sayesinde, farklı bir açıdan teste yaklaşımda olabilir.</span><span class="sxs-lookup"><span data-stu-id="90709-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="90709-481">Sonraki bölümde bu yeni perspektife bakacağız.</span><span class="sxs-lookup"><span data-stu-id="90709-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="90709-482">Eyalet ve etkileşim testi</span><span class="sxs-lookup"><span data-stu-id="90709-482">State versus Interaction Testing</span></span>

<span data-ttu-id="90709-483">Sahte nesnelerle yazılım test etmek için kullanabileceğiniz farklı teknikler vardır.</span><span class="sxs-lookup"><span data-stu-id="90709-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="90709-484">Bir yaklaşım, bu kağıda şimdiye kadar yaptığımız durum tabanlı test kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="90709-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="90709-485">Durum tabanlı test, yazılımın durumu hakkında Onaylamalar yapar.</span><span class="sxs-lookup"><span data-stu-id="90709-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="90709-486">Son testte, denetleyicide bir eylem yöntemi çağırdı ve oluşturması gereken model hakkında bir onay yaptı.</span><span class="sxs-lookup"><span data-stu-id="90709-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="90709-487">Test durumunun diğer örnekleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="90709-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="90709-488">Oluşturma yürütüldükten sonra deponun yeni çalışan nesnesini içerdiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="90709-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="90709-489">Modelin Dizin yürütüldükten sonra tüm çalışanların bir listesini tuttuğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="90709-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="90709-490">Deponun, silme yürütüldükten sonra belirli bir çalışan içermediğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="90709-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="90709-491">Diğer bir yaklaşım de, *etkileşimlerin*doğrulanmak üzere, sahte nesneler ile göreceğiniz bir yaklaşım.</span><span class="sxs-lookup"><span data-stu-id="90709-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="90709-492">Durum tabanlı test, nesnelerin durumu hakkında onaylamaları yaptığında, etkileşim tabanlı test, nesnelerin nasıl etkileşime gireceğini onaylar.</span><span class="sxs-lookup"><span data-stu-id="90709-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="90709-493">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="90709-493">For example:</span></span>

-   <span data-ttu-id="90709-494">Oluşturma yürütüldüğünde, denetleyicinin deponun Add metodunu çağırdığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="90709-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="90709-495">Dizin yürütüldüğünde denetleyicinin deponun FindAll yöntemini çağırdığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="90709-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="90709-496">Denetleyicinin, düzenleme yürütüldüğünde değişiklikleri kaydetmek için iş birimi yürütme yöntemini çağırdığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="90709-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="90709-497">Etkileşim testi genellikle daha az test verisi gerektirir, çünkü koleksiyonlar içinde hiç bir göz aşmadığımyız ve sayıları doğrulıyoruz.</span><span class="sxs-lookup"><span data-stu-id="90709-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="90709-498">Örneğin, Ayrıntılar eyleminin, doğru değere sahip bir deponun Findbyıd metodunu çağırdığından emin olduysa, eylem muhtemelen doğru şekilde davranmaktadır.</span><span class="sxs-lookup"><span data-stu-id="90709-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="90709-499">Bu davranışı, Findbyıd 'den döndürülecek hiçbir test verisi ayarlamadan doğrulayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="90709-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="90709-500">Yukarıdaki test armaöğesinde gereken tek kurulum, temel sınıf tarafından sunulan kurulumdır.</span><span class="sxs-lookup"><span data-stu-id="90709-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="90709-501">Denetleyici eylemini çağırdığınızda MOQ, etkileşimleri sahte depoyla kaydeder.</span><span class="sxs-lookup"><span data-stu-id="90709-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="90709-502">Moq doğrulama API 'sini kullanarak, denetleyici Findbyıd 'yi uygun KIMLIK değeri ile çağırırdı.</span><span class="sxs-lookup"><span data-stu-id="90709-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="90709-503">Denetleyici yöntemi çağırmadı veya yöntemi beklenmeyen bir parametre değeriyle çağırırdı, Verify yöntemi bir özel durum oluşturur ve test başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="90709-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="90709-504">İşte, geçerli iş birimi üzerinde yürütmeyi çağıran oluşturma eylemini doğrulamaya yönelik başka bir örnek.</span><span class="sxs-lookup"><span data-stu-id="90709-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="90709-505">Etkileşim testi olan bir olma tehlikesi, etkileşimlerin belirtilmesinden çok daha fazla.</span><span class="sxs-lookup"><span data-stu-id="90709-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="90709-506">Sahte nesne ile her etkileşimi kaydetme ve doğrulama özelliği, testin her etkileşimi doğrulamaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="90709-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="90709-507">Bazı etkileşimler uygulama ayrıntılarıdır ve yalnızca geçerli testi karşılamak için *gerekli* olan etkileşimleri doğrulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="90709-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="90709-508">Körler veya Fakes arasındaki seçim büyük ölçüde test ettiğiniz sisteme ve kişisel (veya ekip) tercihlerinize bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="90709-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="90709-509">Sahte nesneler, test Double değerlerini uygulamak için gereken kod miktarını büyük ölçüde azaltabilir, ancak herkes, beklentileri ve etkileşimleri doğrulamakta değildir.</span><span class="sxs-lookup"><span data-stu-id="90709-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="90709-510">Sonuçlar</span><span class="sxs-lookup"><span data-stu-id="90709-510">Conclusions</span></span>

<span data-ttu-id="90709-511">Bu yazıda, veri kalıcılığı için ADO.NET Entity Framework kullanırken, test edilebilir kod oluşturmak için çeşitli yaklaşımlar yaptık.</span><span class="sxs-lookup"><span data-stu-id="90709-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="90709-512">IObjectSet&lt;T&gt;gibi yerleşik soyutlamalar kullanabilir veya ırepository&lt;T&gt;gibi kendi soyutlamalarını oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span><span data-ttu-id="90709-513">  Her iki durumda da, ADO.NET Entity Framework 4,0 ' deki POCO desteği, bu soyutlamalar tüketicilerinin kalıcı olarak Ignorant ve yüksek oranda bir şekilde kalmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="90709-513">  In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="90709-514">Örtük yavaş yükleme gibi ek EF4 özellikleri, iş ve uygulama hizmeti kodunun, ilişkisel bir veri deposunun ayrıntıları konusunda endişelenmeden çalışmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="90709-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="90709-515">Son olarak, oluşturduğumuz soyutlamalar birim testlerinin içinde anlamlı veya taklit edilebilir. bu test Double değerlerini kullanarak hızlı çalışan, yüksek oranda yalıtılmış ve güvenilir testler elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="90709-516">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="90709-516">Additional Resources</span></span>

-   <span data-ttu-id="90709-517">Robert C. MARI, " [tek sorumluluk ilkesi](https://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="90709-517">Robert C. Martin, “ [The Single Responsibility Principle](https://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="90709-518">Marwler, *Kurumsal uygulama mimarisi desenlerinden* [desenler kataloğu](https://www.martinfowler.com/eaaCatalog/index.html)</span><span class="sxs-lookup"><span data-stu-id="90709-518">Martin Fowler, [Catalog of Patterns](https://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="90709-519">Griffin Caprio, " [bağımlılık ekleme](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="90709-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="90709-520">Veri programlama blogu, " [Izlenecek yol: Entity Framework 4,0 Ile test odaklı geliştirme](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="90709-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="90709-521">Veri programlama blogu, " [Entity Framework 4,0 Ile depo ve Iş birimi desenleri kullanma](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="90709-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="90709-522">Aaron Jensen, " [makine belirtimlerini tanıtma](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="90709-522">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="90709-523">Eric eser, " [MSTest Ile BDD](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="90709-523">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="90709-524">Eric Evans, " [etki alanı odaklı tasarım](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="90709-524">Eric Evans, “ [Domain Driven Design](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="90709-525">Marwler, "her bir yer [tutucular](https://martinfowler.com/articles/mocksArentStubs.html)yok"</span><span class="sxs-lookup"><span data-stu-id="90709-525">Martin Fowler, “ [Mocks Aren’t Stubs](https://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="90709-526">Marwler, " [Test Double](https://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="90709-526">Martin Fowler, “ [Test Double](https://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   [<span data-ttu-id="90709-527">Moq dili</span><span class="sxs-lookup"><span data-stu-id="90709-527">Moq</span></span>](https://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="90709-528">Biyografi</span><span class="sxs-lookup"><span data-stu-id="90709-528">Biography</span></span>

<span data-ttu-id="90709-529">Scott Allen, Plurali ve OdeToCode.com 'in en altında bulunan teknik personelin bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="90709-529">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="90709-530">15 yıllık ticari yazılım geliştirme sürecinde Scott, 8 bit ekli cihazlardan her şeyin çözüm üzerinde, yüksek düzeyde ölçeklenebilir ASP.NET Web uygulamalarına kadar bir süredir çalıştık.</span><span class="sxs-lookup"><span data-stu-id="90709-530">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="90709-531">Scott 'a OdeToCode konumundaki blogda veya [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode)adresinden Twitter üzerinden ulaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90709-531">You can reach Scott on his blog at OdeToCode, or on Twitter at [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span></span>
