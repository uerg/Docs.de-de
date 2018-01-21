---
title: "Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 861ac619c7f5fb19a56c59536e20724d96bbddca
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)

In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer WebApp mit Benutzerdaten durch Autorisierung geschützt wird. Es zeigt eine Liste von Kontakten, die authentifizierte Benutzer (registrierten) erstellt haben. Es gibt drei Sicherheitsgruppen:

* Registrierte Benutzer können alle genehmigten Kontaktdaten anzeigen.
* Registrierte Benutzer können bearbeiten/ihre eigenen Daten löschen. 
* Manager können genehmigen oder ablehnen Kontaktdaten. Nur genehmigte Kontakte werden für Benutzer angezeigt.
* Administratoren können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.

In der folgenden Abbildung, Benutzer Rick (`rick@example.com`) angemeldet ist. Rick kann nur Benutzeransicht Kontakte genehmigt und seine Kontakte bearbeiten/löschen. Nur der letzte Datensatz erstellt, indem Rick zeigt bearbeiten und Löschen von Verknüpfungen

![Bild oben beschriebenen](secure-data/_static/rick.png)

In der folgenden Abbildung `manager@contoso.com` in und in der Rolle "Manager" signiert ist. 

![Bild oben beschriebenen](secure-data/_static/manager1.png)

Die folgende Abbildung zeigt den Managern Detailansicht eines Kontakts an.

![Bild oben beschriebenen](secure-data/_static/manager.png)

Nur Manager und Administratoren haben die genehmigen und Ablehnen von Schaltflächen.

In der folgenden Abbildung `admin@contoso.com` signiert ist, in und in der Administratorrolle sein. 

![Bild oben beschriebenen](secure-data/_static/admin.png)

Der Administrator verfügt über alle Berechtigungen. Sie können eine beliebige kontaktieren, Lese-/bearbeiten/löschen und ändern Sie den Status von Kontakten.

Die app wurde erstellt, indem [Gerüstbau](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) Folgendes `Contact` Modell:

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

Ein `ContactIsOwnerAuthorizationHandler` Autorisierung-Handler wird sichergestellt, dass Benutzer nur ihre Daten bearbeiten kann. Ein `ContactManagerAuthorizationHandler` Autorisierung Handler ermöglicht Manager zum Genehmigen oder ablehnen von Kontakten.  Ein `ContactAdministratorsAuthorizationHandler` Autorisierung Handler ermöglicht Administratoren zum Genehmigen oder ablehnen von Kontakten und Bearbeiten/Löschen von Kontakten. 

## <a name="prerequisites"></a>Erforderliche Komponenten

Dies ist kein Lernprogramm ab. Sie sollten mit vertraut sein:

* [ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Die Starter und abgeschlossene Anwendung

[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [abgeschlossen](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app. [Test](#test-the-completed-app) abgeschlossene Anwendung, damit Sie mit der Sicherheitsfunktionen vertraut machen. 

### <a name="the-starter-app"></a>Die Starter-app

Es ist hilfreich, den Code mit dem vollständigen Beispiel verglichen werden soll.

[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [Starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app. 

Finden Sie unter [Starter-app erstellen](#create-the-starter-app) , wenn Sie von Grund auf neu erstellen möchten.

Die Datenbank zu aktualisieren:

```none
   dotnet ef database update
```

Führen Sie die app, tippen Sie auf die **ContactManager** verknüpfen, und überprüfen Sie Sie erstellen, bearbeiten und Löschen eines Kontakts.

Dieses Lernprogramm sind die wichtigsten Schritte zum Erstellen der sicheren Benutzer Daten app. Sie können es zum Verweisen auf das abgeschlossene Projekt hilfreich sein.

## <a name="modify-the-app-to-secure-user-data"></a>Ändern Sie die app aus, um Benutzerdaten zu sichern.

In den folgenden Abschnitten sind die wichtigsten Schritte zum Erstellen der sicheren Benutzer Daten app. Sie können es zum Verweisen auf das abgeschlossene Projekt hilfreich sein.

### <a name="tie-the-contact-data-to-the-user"></a>Binden Sie die Kontaktdaten für den Benutzer

Verwenden Sie die ASP.NET [Identität](xref:security/authentication/identity) Benutzer-ID, um sicherzustellen, dass Benutzer ihre Daten, aber nicht für andere Benutzerdaten bearbeiten kann. Hinzufügen `OwnerID` auf die `Contact` Modell:

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`ist die ID des Benutzers aus der `AspNetUser` -Tabelle in der [Identität](xref:security/authentication/identity) Datenbank. Die `Status` Feld bestimmt, ob ein Kontakt allgemeine Benutzer sichtbar ist. 

Das Gerüst für eine neue Migration erstellen und Aktualisieren der Datenbank:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>Erfordern von SSL und authentifizierte Benutzer

In der `ConfigureServices` Methode der *Startup.cs* hinzufügen. die [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) Autorisierungsfilter:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

Zum Umleiten von HTTP-Anforderungen zu HTTPS finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting). Wenn Sie mithilfe von Visual Studio-Code oder auf lokale Plattform, die ein Testzertifikat für SSL keine testen:

- Legen Sie `"LocalTest:skipSSL": true` in der *appsettings.json* Datei.

### <a name="require-authenticated-users"></a>Erfordern Sie authentifizierte Benutzern

Legen Sie die Standardrichtlinie für die Authentifizierung der Benutzer authentifiziert werden müssen. Sie können ablehnen der Authentifizierung auf dem Controller bzw. die Aktionsmethode-Methode, mit der `[AllowAnonymous]` Attribut. Bei diesem Ansatz erfordern neue Domänencontroller hinzugefügt automatisch Authentifizierung, die sicherer ist als der vertrauenden Seite auf neue Domänencontroller enthalten die `[Authorize]` Attribut. Fügen Sie Folgendes an der `ConfigureServices` Methode der *Startup.cs* Datei:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

Hinzufügen `[AllowAnonymous]` zum home-Controller, sodass anonyme Benutzer Informationen über die Website abrufen können, bevor sie registriert werden.

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>Konfigurieren Sie das Testkonto an

Die `SeedData` Klasse zwei Konten "," Administrator "und"-Manager erstellt. Verwenden der [Secret-Manager-Tool](xref:security/app-secrets) zum Festlegen eines Kennworts für diese Konten. Führen Sie diese aus dem Projektverzeichnis (das Verzeichnis mit *"Program.cs"*).

```console
dotnet user-secrets set SeedUserPW <PW>
```

Update `Configure` Testkennwort verwenden:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

Fügen Sie die Administrator-Benutzer-ID und `Status = ContactStatus.Approved` für Kontakte. Fügen Sie nur einen Kontakt angezeigt wird, die Benutzer-ID an alle Kontakte hinzu:

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Erstellen Sie, Besitzer, Manager und Administrator Autorisierung Handler

Erstellen einer `ContactIsOwnerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner. Die `ContactIsOwnerAuthorizationHandler` werden überprüfen Sie, ob der Benutzer auf die Ressource dient die Ressource gehört.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Die `ContactIsOwnerAuthorizationHandler` Aufrufe `context.Succeed` ist der aktuelle authentifizierte Benutzer der Besitzer des Kontakts. Autorisierung Handler brauchbaren `context.Succeed` Wenn die Anforderungen erfüllt sind. Diese zurückgeben `Task.FromResult(0)` Wenn Anforderungen nicht erfüllt sind. `Task.FromResult(0)`weder Erfolg oder Fehler auftritt, wird dadurch andere Autorisierung Handler ausgeführt. Wenn Sie explizit fehlschlägt müssen, zurückgeben `context.Fail()`.

Wir können Kontakt Besitzer bearbeiten/ihre eigenen Daten löschen, damit wir nicht brauchen, überprüfen Sie den Vorgang im Parameters Anforderung übergeben.

### <a name="create-a-manager-authorization-handler"></a>Erstellen Sie einen Ereignishandler der Manager-Autorisierung

Erstellen einer `ContactManagerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner. Die `ContactManagerAuthorizationHandler` der Benutzer, die auf die Ressource ist ein Manager überprüft. Nur Manager können genehmigen oder Ablehnen der Änderungen am Inhalt (neue oder geänderte).

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Erstellen Sie einen Administrator-Autorisierung-Ereignishandler

Erstellen einer `ContactAdministratorsAuthorizationHandler` -Klasse in der *Autorisierung* Ordner. Die `ContactAdministratorsAuthorizationHandler` der Benutzer, die auf die Ressource ist ein Administrator überprüft. Administratoren kann alle Vorgänge tun.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrieren Sie die Authorization-Handler

Verwendung von Entity Framework Core Services müssen registriert werden, für die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) mit [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). Die `ContactIsOwnerAuthorizationHandler` verwendet ASP.NET Core [Identität](xref:security/authentication/identity), die basiert auf Entity Framework Core. Registrieren Sie die Handler mit die Auflistung, damit sie verfügbar sind, werden die `ContactsController` über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection). Fügen Sie den folgenden Code am Ende der `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`und `ContactManagerAuthorizationHandler` als Singletons hinzugefügt werden sollen. Sie Singletons sind, da sie nicht EF verwenden und alle erforderlichen Informationen in den `Context` Parameter von der `HandleRequirementAsync` Methode.

Die vollständige `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>Aktualisieren Sie den Code, um die Autorisierung zu unterstützen.

In diesem Abschnitt Aktualisieren Sie den Controller und Ansichten und fügen Sie eine Operations Anforderungen-Klasse.

### <a name="update-the-contacts-controller"></a>Aktualisieren Sie den Controller Kontakte

Update der `ContactsController` Konstruktor:

* Hinzufügen der `IAuthorizationService` Dienst den Zugriff auf die Autorisierung Handler. 
* Hinzufügen der `Identity` `UserManager` Dienst:

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>Fügen Sie eine wenden Vorgänge Anforderungen-Klasse

Hinzufügen der `ContactOperations` Klasse, um die *Autorisierung* Ordner. Diese Klasse enthalten, die Anforderungen unserer app unterstützt:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>Update zu erstellen.

Update der `HTTP POST Create` aufzurufende Methode:

* Fügen Sie die Benutzer-ID an den `Contact` Modell.
* Rufen Sie den Handler Autorisierung, um sicherzustellen, dass der Benutzer im Besitz des Kontakts ist.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>Update bearbeiten

Aktualisieren Sie sowohl `Edit` Methoden, um den Handler für die Autorisierung verwenden, um zu überprüfen, ob den Benutzer der Besitzer des Kontakts ist. Da wir Autorisierung Ressource ausführen kann nicht verwendet die `[Authorize]` Attribut. Wir haben keinen Zugriff auf die Ressource, wenn Attribute ausgewertet werden. Ressourcenbasierter Autorisierung muss unbedingt erforderlich. Überprüfungen müssen ausgeführt werden, sobald wir Zugriff auf die Ressource haben durch Laden in unserer Controller oder durch Laden in den Handler selbst. Häufig werden Sie durch die Übergabe der Ressourcenschlüssel die Ressource zugreifen.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>Aktualisieren Sie die Delete-Methode

Aktualisieren Sie sowohl `Delete` Methoden, um den Handler für die Autorisierung verwenden, um zu überprüfen, ob den Benutzer der Besitzer des Kontakts ist.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>Fügen Sie dem Prüfdienst in den Ansichten

Derzeit wird die Benutzeroberfläche zeigt bearbeiten und Löschen von Verknüpfungen für Daten, die der Benutzer ändern kann. Wir beheben, die durch Anwenden des Autorisierung Handlers mit der Ansichten.

Fügen Sie dem Prüfdienst in der *Views/_ViewImports.cshtml* Datei, sodass es auf alle Sichten verfügbar sind:

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

Update der *Views/Contacts/Index.cshtml* Razor-Ansicht nur anzeigen, die bearbeiten und Löschen von Verknüpfungen für Benutzer, die den Kontakt bearbeiten/löschen können.

Hinzufügen`@using ContactManager.Authorization;`

Update der `Edit` und `Delete` verknüpft, sodass sie nur für Benutzer mit Berechtigung zum Bearbeiten und löschen den Kontakt gerendert werden.

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

Warnung: Ausblenden von Links von Benutzern, die keine Berechtigung zum Bearbeiten oder löschen Sie Daten jedoch die app nicht gesichert. Durch das Ausblenden Links wird der app mehrere Benutzer angezeigte durch Anzeigen der einzige gültige Links. Benutzer können die generierten URLs zum Aufrufen bearbeiten und Löschen von Operationen mit Daten, die sie besitzen keine hack.  Der Controller muss wiederholt werden, dass die zugriffsüberprüfungen schützen.

### <a name="update-the-details-view"></a>Die Detailansicht aktualisieren

Aktualisieren Sie die Detailansicht, damit Manager genehmigen oder ablehnen von Kontakten können:

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>Abgeschlossene Anwendung testen

Wenn Sie mithilfe von Visual Studio-Code oder auf lokale Plattform, die ein Testzertifikat für SSL keine testen:

- Legen Sie `"LocalTest:skipSSL": true` in der *appsettings.json* Datei.

Wenn Sie die app ausgeführt haben und Kontakte, löschen Sie alle Datensätze in der `Contact` Tabelle, und starten Sie die app Ausgangswert für die Datenbank neu. Wenn Sie Visual Studio verwenden, müssen Sie zum Beenden und starten IIS Express Ausgangswert der Datenbank.

Registrieren Sie einen Benutzer, um die Kontakte zu durchsuchen.

Eine einfache Möglichkeit zum Testen Sie die abgeschlossenen app wird auf drei verschiedenen Browsern (oder inkognito/InPrivate-Versionen) zu starten. In einem Browser eingeben, z. B. einen neuen Benutzer registrieren `test@example.com`. Melden Sie sich mit einem anderen Benutzer jedem Browser an. Überprüfen Sie Folgendes:

* Registrierte Benutzer können alle genehmigten Kontaktdaten anzeigen.
* Registrierte Benutzer können bearbeiten/ihre eigenen Daten löschen. 
* Manager können genehmigen oder ablehnen Kontaktdaten. Die `Details` Ansicht zeigt **genehmigen** und **ablehnen** Schaltflächen. 
* Administratoren können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.

| Benutzer| Optionen |
| ------------ | ---------|
| test@example.com | Können bearbeiten/eigene Daten löschen |
| manager@contoso.com | Können ablehnen/genehmigen und bearbeiten/löschen, die Daten besitzen  |
| admin@contoso.com | Können bearbeiten/löschen und alle Daten genehmigen/ablehnen|

Erstellen Sie einen Kontakt im Browser Administratoren. Kopieren Sie die URL zum Löschen und Bearbeiten von den Administrator wenden Sie sich an. Fügen Sie diese Links in den Browser des Testbenutzers zu überprüfen, ob der Testbenutzer diese Vorgänge ausführen kann nicht aus.

## <a name="create-the-starter-app"></a>Erstellen der Starter-app

Befolgen Sie diese Anweisungen zum Erstellen der Starter-app aus.

* Erstellen einer **ASP.NET-Webanwendung für Core** mit [Visual Studio 2017](https://www.visualstudio.com/) mit dem Namen "ContactManager"

  * Erstellen Sie die app mit **einzelne Benutzerkonten**.
  * Nennen Sie sie "ContactManager", damit Ihr Namespace den Namespace verwenden, in dem Beispiel entspricht.

* Fügen Sie die folgenden `Contact` Modell:

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* Gerüst der `Contact` -Modells mithilfe von Entity Framework Core und die `ApplicationDbContext` -Datenkontext. Übernehmen Sie alle Standardeinstellungen der Gerüstbau. Mit `ApplicationDbContext` Klasse fügt für den Datenkontext der Contact-Tabelle in der [Identität](xref:security/authentication/identity) Datenbank. Finden Sie unter [Hinzufügen eines Modells](xref:tutorials/first-mvc-app/adding-model) für Weitere Informationen.

* Update der **ContactManager** Verankern der *Views/Shared/_Layout.cshtml* aus der Datei `asp-controller="Home"` auf `asp-controller="Contacts"` tippen also die **ContactManager** Link Kontakte-Controllers wird aufgerufen werden. Das ursprüngliche Markup:

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

Der aktualisierte Markupcode aus:

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* Erstellen der anfänglichen Migrations und Aktualisierung der Datenbank

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* Testen der app durch erstellen, bearbeiten und Löschen eines Kontakts

### <a name="seed-the-database"></a>Ausführen eines Seedings für die Datenbank

Hinzufügen der `SeedData` Klasse, um die *Daten* Ordner. Wenn Sie das Beispiel heruntergeladen haben, können Sie kopieren die *SeedData.cs* Datei wird in der *Daten* Ordner des Startprojekts.

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

Fügen Sie den hervorgehobenen Code am Ende der `Configure` Methode in der *Startup.cs* Datei:

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

Testen Sie, ob die Anwendung die Datenbank mit Anfangsdaten gefüllt. Die Seed-Methode wird nicht ausgeführt, wenn alle Zeilen in den Kontakt DB vorhanden sind.

### <a name="create-a-class-used-in-the-tutorial"></a>Erstellen Sie eine Klasse, die im Lernprogramm verwendet

* Erstellen Sie einen Ordner mit dem Namen *Autorisierung*.
* Kopieren der *Authorization\ContactOperations.cs* aus dem Download des abgeschlossenen Projekts-Datei ein, oder kopieren Sie den folgenden Code:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Zusätzliche Ressourcen

* [ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop). Diese Übung wird ausführlicher auf den Sicherheitsfeatures, die in diesem Lernprogramm eingeführt.
* [Autorisierung in ASP.NET Core: einfach, anspruchsbasierte und benutzerdefinierten Rolle](index.md)
* [Benutzerdefinierte, richtlinienbasierte Autorisierung](policies.md)
