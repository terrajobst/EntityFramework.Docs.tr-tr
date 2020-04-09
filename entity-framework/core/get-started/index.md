---
title: Başlarken - EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416885"
---
# <a name="getting-started-with-ef-core"></a>EF Core ile Başlarken

Bu eğitimde, Entity Framework Core'u kullanarak bir SQLite veritabanına karşı veri erişimi gerçekleştiren bir .NET Core konsol uygulaması oluşturursunuz.

Öğreticiyi Windows'da Visual Studio'yu kullanarak veya Windows, macOS veya Linux'ta .NET Core CLI'yi kullanarak takip edebilirsiniz.

[Bu makalenin örneğini GitHub'da görüntüleyin.](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted)

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki yazılımları yükleyin:

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [.NET Çekirdek SDK](https://www.microsoft.com/net/download/core).

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2019 sürüm 16.3 veya daha sonra](https://www.visualstudio.com/downloads/) bu iş yükü ile:
  * **.NET Çekirdek çapraz platform geliştirme** **(Diğer Araç Setleri**altında)

---

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio’yu açın
* **Yeni proje Oluştur'u** tıklatın
* **C#** etiketiyle **Konsol Uygulaması'nı (.NET Core)** seçin ve **İleri'yi** tıklatın
* Ad için **EFGetStarted'u** girin ve **Oluştur'u** tıklatın

---

## <a name="install-entity-framework-core"></a>Varlık Çerçeve Çekirdeğini Yükle

EF Core'u yüklemek için, hedeflemek istediğiniz EF Core veritabanı sağlayıcısı(lar) için paketi yüklersiniz. Bu öğretici, .NET Core'un desteklediği tüm platformlarda çalıştığından SQLite kullanır. Kullanılabilir sağlayıcıların listesi için [Veritabanı Sağlayıcıları'na](../providers/index.md)bakın.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu > Araçlar**
* Aşağıdaki komutları çalıştırın:

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

İpucu: Projeye sağ tıklayıp **NuGet Paketlerini Yönet'i** seçerek paketleri de yükleyebilirsiniz

---

## <a name="create-the-model"></a>Modeli oluşturma

Modeli oluşturan bağlam sınıfı ve varlık sınıfları tanımlayın.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Proje dizininde, aşağıdaki kodla **Model.cs**

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Projeye sağ tıklayın ve **> Sınıfı Ekle'yi** seçin
* Ad olarak **Model.cs** girin ve **Ekle'yi** tıklatın
* Dosyanın içeriğini aşağıdaki kodla değiştirme

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

EF Core, varolan bir veritabanından bir modeli [tersine çevirebilir.](../managing-schemas/scaffolding.md)

İpucu: Gerçek bir uygulamada, her sınıfı ayrı bir dosyaya koyar ve [bağlantı dizesini](../miscellaneous/connection-strings.md) bir yapılandırma dosyasına veya ortam değişkenine koyarsınız. Öğreticiyi basit tutmak için her şey tek bir dosyada bulunur.

## <a name="create-the-database"></a>Veritabanını oluşturma

Aşağıdaki adımlar, bir veritabanı oluşturmak için [geçişleri](xref:core/managing-schemas/migrations/index) kullanır.

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* Aşağıdaki komutları çalıştırın:

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  Bu, [dotnet ef'i](../miscellaneous/cli/dotnet.md) ve projedeki komutu çalıştırmak için gereken tasarım paketini yükler. Komut, `migrations` model için ilk tablo kümesini oluşturmak için bir geçiş iskelesi oluşturur. Komut `database update` veritabanını oluşturur ve yeni geçişi uygular.

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Paket Yöneticisi Konsolunda** aşağıdaki komutları çalıştırın

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  Bu, [EF Core için PMC araçlarını](../miscellaneous/cli/powershell.md)yükler. Komut, `Add-Migration` model için ilk tablo kümesini oluşturmak için bir geçiş iskelesi oluşturur. Komut `Update-Database` veritabanını oluşturur ve yeni geçişi uygular.

---

## <a name="create-read-update--delete"></a>Oluşturma, okuma, güncelleme & silme

* *Program.cs* açın ve içindekileri aşağıdaki kodla değiştirin:

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a>Uygulamayı çalıştırma

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio,.NET Core konsol uygulamalarını çalıştırırken tutarsız bir çalışma dizini kullanır. (bkz: [dotnet/proje-sistem#3619](https://github.com/dotnet/project-system/issues/3619)) Bu bir özel durum atılıyor sonuçlanır: *böyle bir tablo: Bloglar*. Çalışma dizini güncelleştirmek için:

* Projeye sağ tıklayın ve **Proje Dosyasını Edit'i** seçin
* *TargetFramework* özelliğinin hemen altında aşağıdakileri ekleyin:

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* Dosyayı kaydetme

Artık uygulamayı çalıştırabilirsiniz:

* **Hata Ayıklama > Hata Ayıklama olmadan Başlat**

---

## <a name="next-steps"></a>Sonraki adımlar

* Bir web uygulamasında EF Core'u kullanmak için [ASP.NET Core Tutorial'ı](/aspnet/core/data/ef-rp/intro) izleyin
* [LINQ sorgu ifadeleri](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations) hakkında daha fazla bilgi edinin
* Gerekli [ve](xref:core/modeling/entity-properties#required-and-optional-properties) [maksimum uzunluk](xref:core/modeling/entity-properties#maximum-length) gibi şeyleri belirtecek [şekilde modelinizi yapılandırın](xref:core/modeling/index)
* Modelinizi değiştirdikten sonra veritabanı şemasını güncelleştirmek için [Geçişler'i](xref:core/managing-schemas/migrations/index) kullanma
