---
title: Özel geçişler geçmiş tablosu-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655712"
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
