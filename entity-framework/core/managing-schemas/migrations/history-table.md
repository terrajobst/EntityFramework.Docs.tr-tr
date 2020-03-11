---
title: Özel geçişler geçmiş tablosu-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416845"
---
# <a name="custom-migrations-history-table"></a>Özel geçişler geçmiş tablosu

Varsayılan olarak EF Core, veritabanını `__EFMigrationsHistory`adlı bir tabloya kaydederek hangi geçişlerin uygulanmış olduğunu izler. Çeşitli nedenlerle bu tabloyu gereksinimlerinize daha uygun olacak şekilde özelleştirmek isteyebilirsiniz.

> [!IMPORTANT]
> Geçişleri uyguladıktan *sonra* geçişleri geçmiş tablosunu özelleştirirseniz, veritabanında var olan tabloyu güncelleştirmekten siz sorumlusunuz.

## <a name="schema-and-table-name"></a>Şema ve tablo adı

Şema ve tablo adını, `OnConfiguring()` (veya ASP.NET Core üzerinde `ConfigureServices()`) `MigrationsHistoryTable()` yöntemini kullanarak değiştirebilirsiniz. SQL Server EF Core sağlayıcısını kullanan bir örnek aşağıda verilmiştir.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Diğer değişiklikler

Tablonun ek yönlerini yapılandırmak için sağlayıcıya özgü `IHistoryRepository` hizmetini geçersiz kılın ve değiştirin. SQL Server MigrationID sütununun adını *kimlik* olarak değiştirme örneği aşağıda verilmiştir.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` iç ad alanı içindedir ve gelecek sürümlerde değiştirilebilir.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
