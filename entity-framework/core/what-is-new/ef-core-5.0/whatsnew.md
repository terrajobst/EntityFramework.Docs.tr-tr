---
title: EF Core 5,0 ' deki yenilikler
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 65d7bd43e8a00c77fd6091a74c677635710d03e3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417968"
---
# <a name="whats-new-in-ef-core-50"></a>EF Core 5,0 ' deki yenilikler

EF Core 5,0 şu anda geliştirme aşamasındadır.
Bu sayfa, her önizlemede sunulan ilginç değişikliklere genel bir bakış içerir.
EF Core 5,0 ' nin ilk önizlemesi, içinde, 2020 ' in ilk çeyreğinde kesin olarak beklenmez.

Bu sayfa [EF Core 5,0 planını](plan.md)yinelemez.
Plan, son yayını teslim etmeden önce dahil ettiğimiz her şey dahil olmak üzere EF Core 5,0 ' a yönelik genel temaları açıklar.

Yayımlanmakta olan resmi belgelere buradan bağlantılar ekleyeceğiz.

## <a name="preview-1-not-yet-shipped"></a>Önizleme 1 (henüz sevk edilmemiş)

### <a name="simple-logging"></a>Basit günlüğe kaydetme

Bu özellik EF6 içinde `Database.Log` benzer işlevler ekler.
Yani, her türlü harici günlük çerçevesini yapılandırmaya gerek kalmadan EF Core günlükleri almanın basit bir yolunu sunar.

İlk belgeler, [5 aralık 2019 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.

Ek belgeler [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085)soruna göre izlenir.

### <a name="simple-way-to-get-generated-sql"></a>Oluşturulan SQL almanın basit yolu

EF Core 5,0, bir LINQ sorgusu yürütürken EF Core üretebileceği SQL döndürecek `ToQueryString` genişletme yöntemini tanıtır.

Ön belgeler, [9 ocak 2020 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)dahildir.

Ek belgeler [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331)soruna göre izlenir.

### <a name="enhanced-debug-views"></a>Gelişmiş hata ayıklama görünümleri

Hata ayıklama görünümlerinde EF Core iç yapıları göz atmak kolay bir yoludur.
Modelin bir hata ayıklama görünümü bir süre önce uygulandı.
EF Core 5,0 için model görünümü ' ne daha kolay okunabilir ve durum yöneticisinde izlenen varlıklar için yeni bir hata ayıklama görünümü ekledik.

Ön belgeler, [12 aralık 2019 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206)dahildir.

Ek belgeler [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086)soruna göre izlenir.

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Bağlantı veya bağlantı dizesi, başlatılmış DbContext üzerinde değiştirilebilir

Artık herhangi bir bağlantı veya bağlantı dizesi olmadan bir DbContext örneği oluşturulması daha kolay.
Ayrıca, bağlantı veya bağlantı dizesi artık bağlam örneği üzerinde değiştirilebilir.
Bu, aynı bağlam örneğinin farklı veritabanlarına dinamik olarak bağlanmasını sağlar.

Belgeler [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075)soruna göre izlenir.

### <a name="change-tracking-proxies"></a>Değişiklik izleme proxy 'leri

EF Core, artık otomatik olarak [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) ve [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1)uygulayan çalışma zamanı proxy 'leri oluşturabilir.
Böylece, varlık özelliklerindeki değer değişiklikleri doğrudan EF Core olarak raporlanarak değişiklik taraması ihtiyacını önler.
Ancak, proxy 'ler kendi kısıtlama kümesiyle gelir, bu nedenle herkes için değildir.

Belgeler [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076)soruna göre izlenir.

### <a name="improved-handling-of-database-null-semantics"></a>Veritabanı null semantiğini gelişmiş işleme

İlişkisel veritabanları genellikle NULL değerini bilinmeyen bir değer olarak değerlendirir ve bu nedenle başka bir NULL değere eşit değildir.
C#, diğer taraftan, null değeri, diğer herhangi bir null ile eşit olan tanımlı bir değer olarak davranır.
EF Core, varsayılan olarak, null semantiğini kullanacak C# şekilde sorguları çevirir.
EF Core 5,0, bu çevirilerin verimliliğini önemli ölçüde geliştirir.

Belgeler [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612)soruna göre izlenir.

### <a name="indexer-properties"></a>Dizin Oluşturucu özellikleri

EF Core 5,0, C# Dizin Oluşturucu özelliklerinin eşlenmesini destekler.
Bu, varlıkların, sütunların pakette adlandırılmış özelliklerle eşlendikleri özellik paketleri olarak davranmasını sağlar.

Belgeler [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018)soruna göre izlenir.

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Enum eşlemeleri için denetim kısıtlamaları oluşturma

EF Core 5,0 geçişleri, artık enum özelliği eşlemeleri için DENETIM kısıtlamaları oluşturabilir.
Örnek:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

Belgeler [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082)soruna göre izlenir.

### <a name="query-translations-for-more-datetime-constructs"></a>Daha fazla TarihSaat yapıları için sorgu çevirileri

Yeni veri süresi oluşturma içeren sorgular artık çevrilir.
Ayrıca, DateDiffWeek SQL Server işlevi artık eşleştirilir.

Belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.

### <a name="query-translations-for-more-byte-array-constructs"></a>Daha fazla bayt dizisi yapıları için sorgu çevirileri

Byte [] özellikleri, Contains, length, Sequenceeşittir, vb. kullanan sorgular artık SQL 'e çevrilir.

İlk belgeler, [5 aralık 2019 Için EF haftalık durumuna](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863)dahildir.

Ek belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.

### <a name="query-translation-for-reverse"></a>Ters için sorgu çevirisi

`Reverse` kullanan sorgular artık çevrilir.
Örnek:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.

### <a name="query-translation-for-bitwise-operators"></a>Bit düzeyinde işleçler için sorgu çevirisi

Bit düzeyinde işleçler kullanan sorgular artık daha fazla durumda çevrilir:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.

### <a name="query-translation-for-strings-on-cosmos"></a>Cosmos üzerinde dizeler için sorgu çevirisi

Dize yöntemlerini kullanan sorgular şunlardır, StartsWith ve EndsWith artık Azure Cosmos DB sağlayıcısı kullanılırken çevrilir.

Belgeler [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079)soruna göre izlenir.
