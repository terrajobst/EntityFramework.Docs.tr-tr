---
title: Başlarken-EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416885"
---
# <a name="getting-started-with-ef-core"></a>EF Core kullanmaya başlama

Bu öğreticide, Entity Framework Core kullanarak bir SQLite veritabanına yönelik veri erişimi gerçekleştiren bir .NET Core konsol uygulaması oluşturacaksınız.

Windows üzerinde Visual Studio 'yu kullanarak veya Windows, macOS veya Linux üzerinde .NET Core CLI kullanarak öğreticiyi izleyebilirsiniz.

[Bu makalenin örneğini GitHub 'Da görüntüleyin](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yazılımı yükler:

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [.NET Core SDK](https://www.microsoft.com/net/download/core).

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2019 sürüm 16,3 veya üzeri](https://www.visualstudio.com/downloads/) bu iş yükü:
  * **.NET Core platformlar arası geliştirme** ( **diğer araç kümeleri**altında)

---

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio’yu açın
* **Yeni proje oluştur ' a** tıklayın
* Etiketi ile **konsol uygulaması (.NET Core)** seçeneğini belirleyin ve ileri ' ye tıklayın. **C#**
* Ad için **Efgetstarted** girin ve **Oluştur** ' a tıklayın.

---

## <a name="install-entity-framework-core"></a>Entity Framework Core yüklensin

EF Core yüklemek için, hedeflemek istediğiniz EF Core veritabanı sağlayıcılarının paketini yüklersiniz. Bu öğretici, .NET Core 'un desteklediği tüm platformlarda çalıştığı için SQLite kullanır. Kullanılabilir sağlayıcıların bir listesi için bkz. [veritabanı sağlayıcıları](../providers/index.md).

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi konsolu**
* Aşağıdaki komutları çalıştırın:

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

İpucu: Ayrıca, projeye sağ tıklayıp **NuGet Paketlerini Yönet** ' i seçerek paketleri yükleyebilirsiniz.

---

## <a name="create-the-model"></a>Modeli oluşturma

Modeli oluşturan bir bağlam sınıfı ve varlık sınıfları tanımlayın.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Proje dizininde aşağıdaki kodla **model.cs** oluşturun

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Projeye sağ tıklayın ve **> sınıfı Ekle** ' yi seçin.
* Ad olarak **model.cs** girin ve **Ekle** ' ye tıklayın.
* Dosyanın içeriğini aşağıdaki kodla değiştirin

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

EF Core Ayrıca, varolan bir veritabanından bir modele [ters mühendislik](../managing-schemas/scaffolding.md) uygulanabilir.

İpucu: gerçek bir uygulamada, her bir sınıfı ayrı bir dosyaya yerleştirip [bağlantı dizesini](../miscellaneous/connection-strings.md) bir yapılandırma dosyasına veya ortam değişkenine yerleştirebilirsiniz. Öğreticiyi bir şekilde korumak için her şey tek bir dosyada yer alır.

## <a name="create-the-database"></a>Veritabanını oluşturma

Aşağıdaki adımlar bir veritabanı oluşturmak için [geçişleri](xref:core/managing-schemas/migrations/index) kullanır.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Aşağıdaki komutları çalıştırın:

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  Bu, bir projede komutu çalıştırmak için gerekli olan [DotNet EF](../miscellaneous/cli/dotnet.md) ve tasarım paketini de yüklüyor. `migrations` komutu, modelin ilk tablo kümesini oluşturmak için bir geçişi oluşturur. `database update` komutu veritabanını oluşturur ve yeni geçişi uygular.

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Paket Yöneticisi konsolunda** aşağıdaki komutları çalıştırın

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  Bu, [EF Core Için PMC araçlarını](../miscellaneous/cli/powershell.md)kurar. `Add-Migration` komutu, modelin ilk tablo kümesini oluşturmak için bir geçişi oluşturur. `Update-Database` komutu veritabanını oluşturur ve yeni geçişi uygular.

---

## <a name="create-read-update--delete"></a>Oluşturma, okuma, güncelleştirme & silme

* *Program.cs* açın ve içeriğini şu kodla değiştirin:

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio, .NET Core konsol uygulamaları çalıştırırken tutarsız bir çalışma dizini kullanır. (bkz. [DotNet/Project-System # 3619](https://github.com/dotnet/project-system/issues/3619)) Bu durum, oluşan bir özel durumla sonuçlanır: *böyle bir tablo: blogları*. Çalışma dizinini güncelleştirmek için:

* Projeye sağ tıklayın ve **Proje dosyasını Düzenle** ' yi seçin.
* *TargetFramework* özelliğinin hemen altında aşağıdakileri ekleyin:

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* Dosyayı kaydetme

Artık uygulamayı çalıştırabilirsiniz:

* **Hata ayıklama > hata ayıklama olmadan Başlat**

---

## <a name="next-steps"></a>Sonraki adımlar

* Bir Web uygulamasında EF Core kullanmak için [ASP.NET Core öğreticisini](/aspnet/core/data/ef-rp/intro) izleyin
* [LINQ sorgu ifadeleri](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations) hakkında daha fazla bilgi edinin
* [Gerekli](xref:core/modeling/entity-properties#required-and-optional-properties) ve [en fazla uzunluk](xref:core/modeling/entity-properties#maximum-length) gibi Işlemleri belirtmek için [modelinizi yapılandırın](xref:core/modeling/index)
* Modelinizi değiştirdikten sonra veritabanı şemasını güncelleştirmek için [geçişleri](xref:core/managing-schemas/migrations/index) kullanma
