---
title: Sorgular nasıl çalışır-EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: ba0d68469530e6272ffbb51946d7856122a261c7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417703"
---
# <a name="how-queries-work"></a>Sorgular nasıl çalışır?

Entity Framework Core, veritabanındaki verileri sorgulamak için dil ile tümleşik sorgu (LINQ) kullanır. LINQ, türetilmiş bağlama ve C# varlık sınıflarınıza göre kesin tür belirtilmiş sorgular yazmak için (veya .net dilinizi tercih ettiğiniz) kullanmanıza olanak tanır.

## <a name="the-life-of-a-query"></a>Bir sorgunun ömrü

Aşağıda her bir sorgunun gittiği işleme ilişkin üst düzey bir genel bakış verilmiştir.

1. LINQ sorgusu, veritabanı sağlayıcısı tarafından işlenmek üzere hazırlanmaya yönelik bir temsili oluşturmak için Entity Framework Core tarafından işlenir
   1. Sonuç, bu işlemin sorgu her yürütüldüğünde yapılması gerekmemesi için önbelleğe alınır
2. Sonuç veritabanı sağlayıcısına geçirilir
   1. Veritabanı sağlayıcısı, sorgunun hangi bölümlerinin veritabanında değerlendirilebileceğini belirler
   2. Sorgunun bu bölümleri veritabanına özgü sorgu diline çevrilir (örneğin, bir ilişkisel veritabanı için SQL)
   3. Veritabanına bir veya daha fazla sorgu gönderiliyor ve döndürülen sonuç kümesi (sonuçlar, varlık örneklerinden değil, veritabanındaki değerlerdir)
3. Sonuç kümesindeki her öğe için
   1. Bu bir izleme sorgiyiyiyise EF, verilerin bağlam örneği için değişiklik izleyicide zaten bir varlığı temsil ettiğini denetler
      * Öyleyse, var olan varlık döndürülür
      * Aksi takdirde, yeni bir varlık oluşturulur, değişiklik izleme kurulum olur ve yeni varlık döndürülür
   2. Bu bir izleme sorgusu değilse EF, verilerin bu sorgu için sonuç kümesinde zaten olan bir varlığı temsil ettiğini denetler
      * Varsa, var olan varlık döndürülür <sup>(1)</sup>
      * Aksi takdirde, yeni bir varlık oluşturulur ve döndürülür

<sup>(1)</sup> hiçbir izleme sorgusu, zaten döndürülen varlıkların izlenmesini sağlamak için zayıf başvurular kullanır. Aynı kimliğe sahip önceki bir sonuç kapsam dışına gittiğinde ve çöp toplama işlemi çalıştırıyorsa, yeni bir varlık örneği alabilirsiniz.

## <a name="when-queries-are-executed"></a>Sorgular yürütüldüğünde

LINQ işleçlerini çağırdığınızda sorgunun bellek içi gösterimini oluşturursunuz. Sorgu yalnızca sonuçlar tüketiliyorsa veritabanına gönderilir.

Veritabanına gönderilen sorgunun sonucu olan en yaygın işlemler şunlardır:

* Sonuçları bir `for` döngüsünde yineleme
* `ToList`, `ToArray`, `Single`, `Count` gibi bir işleç kullanma
* Sorgunun sonuçlarını Kullanıcı arabirimine bağlama

> [!WARNING]  
> **Kullanıcı girişini her zaman doğrula:** EF Core, sorgularda parametreleri ve kaçış sabit değerlerini kullanarak SQL ekleme saldırılarına karşı koruma sağlar, girişleri doğrulamaz. Uygulamanın gereksinimlerine göre uygun doğrulama, güvenilmeyen kaynaklardan gelen değerler LINQ sorgularında kullanılmadan, varlık özelliklerine atanmadan veya diğer EF Core API 'Lerine geçirilmeden önce gerçekleştirilmelidir. Bu, sorguları dinamik olarak oluşturmak için kullanılan herhangi bir kullanıcı girişini içerir. LINQ kullanırken bile, derleme ifadelerine Kullanıcı girişini kabul ediyorsanız, yalnızca amaçlanan ifadelerin oluşturulabilse emin olmanız gerekir.
