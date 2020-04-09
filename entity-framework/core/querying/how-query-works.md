---
title: Sorgular Nasıl Çalışır - EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136230"
---
# <a name="how-queries-work"></a>Sorgular Nasıl Çalışır?

Entity Framework Core, veritabanındaki verileri sorgulamak için Dil Tümleşik Sorgusu 'nu (LINQ) kullanır. LINQ, türetilmiş bağlam ve varlık sınıflarınıza göre güçlü bir şekilde yazılan sorguları yazmak için C# (veya seçtiğiniz .NET dilini) kullanmanıza olanak tanır.

## <a name="the-life-of-a-query"></a>Sorgunun ömrü

Aşağıda, her sorgunun geçtiği işlemin üst düzey bir özeti veremiştir.

1. LINQ sorgusu, veritabanı sağlayıcısı tarafından işlenmeye hazır bir gösterim oluşturmak için Entity Framework Core tarafından işlenir
   1. Sonuç önbelleğe alının, böylece sorgu her yürütüldolduğunda bu işlemin yapılması gerekmez
2. Sonuç veritabanı sağlayıcısına aktarılır
   1. Veritabanı sağlayıcısı, sorgunun hangi bölümlerinin veritabanında değerlendirilebileceğini tanımlar
   2. Sorgunun bu bölümleri veritabanına özgü sorgu diline (örneğin, ilişkisel bir veritabanı için SQL) çevrilir.
   3. Bir sorgu veritabanına gönderilir ve sonuç kümesi döndürülür (sonuçlar varlık örnekleri değil veritabanından gelen değerlerdir)
3. Sonuç kümesindeki her öğe için
   1. Bu bir izleme sorgusuysa, EF, verilerin bağlam örneği için değişiklik izleyicisinde zaten bulunan bir varlığı temsil edip edemediğini denetler
      * Bu nedenle, varolan varlık döndürülür
      * Değilse, yeni bir varlık oluşturulur, değişiklik izleme kurulumu ve yeni varlık döndürülür
   2. Bu bir izleme sorgusu değilse, her zaman yeni bir varlık oluşturulur ve döndürülür

## <a name="when-queries-are-executed"></a>Sorgular yürütüldüğünde

LINQ işleçlerini aradiğinizde, yalnızca sorgunun bellek içi bir temsilini oluşturuyorsunuz. Sorgu yalnızca sonuçlar tüketildiğinde veritabanına gönderilir.

Sorgunun veritabanına gönderilmesiyle sonuçlanan en yaygın işlemler şunlardır:

* Sonuçları bir `for` döngüde yinele
* , `ToList`, `ToArray` `Single` `Count` , veya eşdeğer async aşırı yükleri gibi bir işleç kullanma

> [!WARNING]  
> **Kullanıcı girişini her zaman doğrulayın:** EF Core parametreleri kullanarak ve sorgularda literals kaçarak SQL enjeksiyon saldırılarına karşı korur iken, girişleri doğrulamaz. Uygun doğrulama, uygulamanın gereksinimlerine göre, INLAQ sorgularında güvenilmeyen kaynaklardan gelen değerler kullanılmadan, varlık özelliklerine atanmadan veya diğer EF Core API'lerine geçirilmeden önce gerçekleştirilmelidir. Bu, sorguları dinamik olarak oluşturmak için kullanılan kullanıcı girişlerini içerir. LINQ kullanırken bile, ifadeleri oluşturmak için kullanıcı girişini kabul ediyorsanız, yalnızca amaçlanan ifadelerin oluşturulabileceğinden emin olmanız gerekir.
