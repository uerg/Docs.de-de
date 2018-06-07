---
title: Hinzufügen, herunterladen und Löschen von benutzerdefinierten Benutzerdaten Identität in einem Projekt auf ASP.NET Core
author: rick-anderson
description: Weitere Informationen Sie zum Hinzufügen von benutzerdefinierten Daten zu Identität in einem Projekt auf ASP.NET Core. Löschen von Daten pro GDPR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: cc7b29499e9db702cab70be7c15eac53373d450d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819070"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Hinzufügen, herunterladen und Löschen von benutzerdefinierten Benutzerdaten Identität in einem Projekt auf ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieser Artikel zeigt, wie Sie:

* Fügen Sie benutzerdefinierte Benutzerdaten an eine ASP.NET Core-Web-app.
* Ergänzen Sie die benutzerdefinierten Datenmodell mit der [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) Attribut, damit er automatisch für herunterladen und Löschen verfügbar ist. Machen die Daten heruntergeladen und gelöscht werden kann hilft bei der Erfüllung [GDPR](xref:security/gdpr) Anforderungen.

Project-Beispiels aus einer Razor-Seiten-Web-app erstellt, aber die Anweisungen ähneln sich für eine ASP.NET Core MVC-Web-app.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Erstellen einer Razor-Web-App

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**. Nennen Sie das Projekt **WebApp1** Wenn Sie möchten einen übereinstimmenden Namespace der [Beispiel herunterladen](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) Code.
* Wählen Sie **Webanwendung ASP.NET Core** > **OK**
* Wählen Sie **ASP.NET Core 2.1** in der Dropdownliste
* Wählen Sie **-Webanwendung**  > **OK**
* Erstellen Sie das Projekt, und führen Sie es aus.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

------

## <a name="run-the-identity-scaffolder"></a>Führen Sie die Identität scaffolder

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Von **Projektmappen-Explorer**, mit der rechten Maustaste auf das Projekt > **hinzufügen** > **neues Gerüstelement**.
* Im linken Bereich des der **Gerüst hinzufügen** wählen Sie im Dialogfeld **Identität** > **hinzufügen**.
* In der **ADD Identität** Dialog, der die folgenden Optionen:
  * Wählen Sie Ihre vorhandenen Layoutdatei *~/Pages/Shared/_Layout.cshtml*
  * Wählen Sie die folgenden Dateien überschreiben:
    * **Konto/registrieren**
    * **Konto / / Index verwalten**
  * Wählen Sie die **+** Schaltfläche zum Erstellen eines neuen **Datenkontextklasse**. Akzeptieren Sie den Typ (**WebApp1.Models.WebApp1Context** , wenn Sie das Projekt mit dem Namen **WebApp1**).
  * Wählen Sie die **+** Schaltfläche zum Erstellen eines neuen **Benutzerklasse**. Akzeptieren Sie den Typ (**WebApp1User** , wenn Sie das Projekt mit dem Namen **WebApp1**) > **hinzufügen**.
* Wählen Sie **hinzufügen**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

Wenn Sie die ASP.NET Scaffolder noch nicht installiert haben, installieren Sie es jetzt ein:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Hinzufügen eines Verweises Paket auf [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) der Projektdatei (csproj). Führen Sie den folgenden Befehl in das Projektverzeichnis:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Führen Sie den folgenden Befehl zum Auflisten von Optionen Scaffolder Identität:

```cli
dotnet aspnet-codegenerator identity -h
```

Führen Sie in den Projektordner der Identität Scaffolder aus:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

Die Anweisungen im [Migrationen, UseAuthentication und Layout](xref:security/authentication/scaffold-identity#efm) die folgenden Schritte ausführen:

* Erstellen Sie eine Migration aus, und Aktualisieren der Datenbank.
* Fügen Sie `UseAuthentication` zu `Startup.Configure` hinzu.
* Hinzufügen `<partial name="_LoginPartial" />` der Layout-Datei.
* Testen der App:
  * Registrieren eines Benutzers
  * Wählen Sie den neuen Benutzernamen ein (neben der **Logout** Link). Sie müssen möglicherweise erweitern das Fenster, oder wählen das Navigationssymbol Balken, um den Benutzernamen und anderen Links anzuzeigen.
  * Wählen Sie die **persönliche Daten** Registerkarte.
  * Wählen Sie die **herunterladen** Schaltfläche und untersucht die *PersonalData.json* Datei.
  * Testen der **löschen** Schaltfläche, die der angemeldete Benutzer löscht.

## <a name="add-custom-user-data-to-the-identity-db"></a>Hinzufügen von benutzerdefinierten Benutzerdaten mit der Identity-Datenbank

Update der `IdentityUser` abgeleitete Klasse mit benutzerdefinierten Eigenschaften. Wenn Sie Ihr Projekt WebApp1 genannt, kann die Datei heißt *Areas/Identity/Data/WebApp1User.cs*. Aktualisieren Sie die Datei mit den folgenden Code ein:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Eigenschaften mit ergänzt die [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) Attribut sind:

* Gelöscht, sobald die *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor-Seite ruft `UserManager.Delete`.
* Enthalten in der heruntergeladenen Daten durch die *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor-Seite.

### <a name="update-the-accountmanageindexcshtml-page"></a>Die Seite Account/Manage/Index.cshtml "Aktualisieren"

Update der `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* mit den folgenden hervorgehobenen Code:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95)]

Update der *Areas/Identity/Pages/Account/Manage/Index.cshtml* mit dem folgenden hervorgehobenen Markup:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Die Seite Account/Register.cshtml "Aktualisieren"

Update der `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* mit den folgenden hervorgehobenen Code:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Update der *Areas/Identity/Pages/Account/Register.cshtml* mit dem folgenden hervorgehobenen Markup:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Erstellen Sie das Projekt.

### <a name="add-a-migration-for-the-custom-user-data"></a>Fügen Sie eine Migration für die benutzerdefinierte Daten hinzu

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

In Visual Studio **Package Manager Console**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>Test erstellen, anzeigen, herunterladen, Löschen von benutzerdefinierten Daten

Testen der App:

* Registrieren Sie einen neuen Benutzer.
* Zeigen Sie die benutzerdefinierten Benutzerdaten auf der `/Identity/Account/Manage` Seite.
* Herunterladen und zeigen Sie die Benutzer persönliche Daten aus der `/Identity/Account/Manage/PersonalData` Seite.
