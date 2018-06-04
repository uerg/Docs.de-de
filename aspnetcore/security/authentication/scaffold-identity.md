---
title: Gerüst Identität in ASP.NET Core-Projekten
author: rick-anderson
description: Erfahren Sie, wie das Gerüst für Identität in einem Projekt auf ASP.NET Core erstellen.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: a43b7bbaf1f90d3373b3846bc3f4f32be6b80bd4
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729609"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Gerüst Identität in ASP.NET Core-Projekten

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 und höher bietet [ASP.NET Core Identity](xref:security/authentication/identity) als eine [Razor-Klassenbibliothek](xref:mvc/razor-pages/ui-class). Anwendungen, die Identität können der Scaffolder um selektiv hinzufügen, den Quellcode in der Identität Razor Klasse Bibliothek (RCL) enthalten angewendet werden. Möglicherweise möchten Generieren von Quellcode, dadurch können Sie den Code zu ändern und das Verhalten zu ändern. Sie können z. B. die Scaffolder zum Generieren des Codes in der Registrierung verwendete anweisen. Generierter Code hat Vorrang vor den gleichen Code in der Identität RCL.

Anwendungen, die **nicht** enthalten Authentifizierung kann die Scaffolder zum Hinzufügen des Pakets RCL Identität anwenden. Sie haben die Möglichkeit, auswählen von ID-Code generiert werden soll.

Obwohl die Scaffolder Großteil der erforderliche Code generiert, müssen Sie Ihrem Projekt zum Abschließen des Vorgangs aktualisieren. Dieses Dokument erläutert die Schritte zum Abschließen eines Identität Gerüstbau Updates erforderlich sind.

Wenn die Identität Scaffolder ausgeführt wird, eine *ScaffoldingReadme.txt* Datei ist im Projektverzeichnis erstellt. Die *ScaffoldingReadme.txt* -Datei enthält allgemeine Anweisungen auf, was erforderlich ist, um die Identität des Gerüstbaus Aktualisierung abgeschlossen ist. Dieses Dokument enthält ausführlichere Anweisungen als das Lesen der *ScaffoldingReadme.txt* Datei.

Es wird empfohlen, mit einem Quellcodeverwaltungssystem, die die Unterschiede zwischen und bietet die Möglichkeit, außerhalb des gültigen Änderungen sichern. Überprüfen Sie die Änderungen nach dem Ausführen der Identität Scaffolder.

## <a name="scaffold-identity-into-an-empty-project"></a>Gerüst Identität in ein leeres Projekt

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Fügen Sie die folgenden hervorgehobenen Aufrufe an die `Startup` Klasse:

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Gerüst Identität in einer Razor-Projekt, ohne vorhandene Autorisierung

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Identität konfiguriert ist, *Areas/Identity/IdentityHostingStartup.cs*. Weitere Informationen finden Sie unter [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

In der `Configure` Methode der `Startup` -Klasse, rufen Sie [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) nach `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Änderungen am Layout

Optional: Fügen Sie der partiellen Anmeldung hinzu (`_LoginPartial`) in der Layoutdatei:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Gerüst Identität in einer Razor-Projekt mit Autorisierung

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Einige identitätsoptionen im konfiguriert sind *Areas/Identity/IdentityHostingStartup.cs*. Weitere Informationen finden Sie unter [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Gerüst Identität in einer MVC-Projekt, ohne vorhandene Autorisierung

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Optional: Fügen Sie der partiellen Anmeldung hinzu (`_LoginPartial`), die *Views/Shared/_Layout.cshtml* Datei:

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Verschieben der *Pages/Shared/_LoginPartial.cshtml* Datei *Views/Shared/_LoginPartial.cshtml*

Identität konfiguriert ist, *Areas/Identity/IdentityHostingStartup.cs*. Weitere Informationen finden Sie unter IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Rufen Sie [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) nach `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Gerüst Identität in einer MVC-Projekt mit Autorisierung

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Löschen der *Seiten/freigegebene* Ordner und Dateien in diesem Ordner.
