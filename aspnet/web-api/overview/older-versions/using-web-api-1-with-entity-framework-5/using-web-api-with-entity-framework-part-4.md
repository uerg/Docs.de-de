---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Teil 4: Hinzufügen einer-Administratoransicht | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 599f684ba200821d7fcd33819937c5a5b5a29ce8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371050"
---
<a name="part-4-adding-an-admin-view"></a>Teil 4: Hinzufügen einer-Administratoransicht
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Hinzufügen einer-Administratoransicht

Nun wir auf der Clientseite zu aktivieren, und fügen Sie eine Seite, die Daten über den Admin-Controller verwenden. Die Seite können Benutzer zu erstellen, bearbeiten oder Löschen von Produkten, per AJAX-Anforderungen an den Controller.

Klicken Sie im Projektmappen-Explorer erweitern Sie den Ordner "Controllers", und öffnen Sie die Datei mit dem Namen "HomeController.cs". Diese Datei enthält einen MVC-Controller. Fügen Sie eine Methode mit dem Namen `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

Die **HttpRouteUrl** Methode erstellt den URI der Web-API, und wir dies in den ansichtsbehälter zur späteren Verwendung speichern.

Als Nächstes, positionieren Sie den Textcursor in den `Admin` Aktionsmethode, und klicken Sie dann mit der rechten Maustaste und wählen **Ansicht hinzufügen**. Hierdurch wird die **Ansicht hinzufügen** Dialogfeld.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

In der **Ansicht hinzufügen** Dialogfeld Namen der Ansicht "Admin". Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**. Klicken Sie unter **Modellklasse**, wählen Sie "Product (ProductStore.Models)". Behalten Sie alle anderen Optionen die Standardwerte.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Auf **hinzufügen** Fügt eine Datei mit dem Namen Admin.cshtml unter Views/Home. Öffnen Sie diese Datei, und fügen Sie folgenden HTML-Code hinzu. Dieser HTML-Code definiert die Struktur der Seite, aber keine Funktionen läuft noch.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Erstellen Sie einen Link auf der Seite "Administrator"

Klicken Sie im Projektmappen-Explorer erweitern Sie den Ordner "Views", und erweitern Sie dann den Ordner Shared. Öffnen Sie die Datei mit dem Namen \_Layout.cshtml. Suchen Sie die **Ul** Element mit der Id = "Menu" und einen Aktionslink für die Admin-Ansicht:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> Im Beispielprojekt habe ich ein paar andere kosmetischen Änderungen ermöglicht, z. B. ersetzt die Zeichenfolge "Ihr Logo hier einfügen" vorgenommen. Dies betrifft nicht die Funktionalität der Anwendung verwenden. Sie können das Projekt herunterladen und vergleichen Sie die Dateien.


Führen Sie die Anwendung, und klicken Sie auf den Link "Admin", der am oberen Rand der Startseite angezeigt wird. Die Seite "Administrator" sollte wie folgt aussehen:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Rechts jetzt die Seite nicht. Im nächsten Abschnitt verwenden wir "Knockout.js" zum Erstellen einer dynamischen Benutzeroberflächenautomatisierungs.

## <a name="add-authorization"></a>Autorisierung hinzufügen

Die Seite "Administrator" ist derzeit für alle Benutzer Zugriff auf die Website zugegriffen werden kann. Wir ändern Sie diese Option, um die Berechtigungen für Administratoren einschränken.

Durch Hinzufügen einer Rolle "Administrator" und einen Benutzer mit Administratorrechten starten. Klicken Sie im Projektmappen-Explorer erweitern Sie den Filter-Ordner, und öffnen Sie die Datei mit dem Namen InitializeSimpleMembershipAttribute.cs. Suchen Sie die `SimpleMembershipInitializer` Konstruktor. Nach dem Aufruf von **WebSecurity.InitializeDatabaseConnection**, fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Dies ist eine schnelle Möglichkeit zum Hinzufügen der Rolle "Administrator", und erstellen Sie einen Benutzer für die Rolle.

Klicken Sie im Projektmappen-Explorer erweitern Sie den Ordner "Controllers", und öffnen Sie die Datei "HomeController.cs". Hinzufügen der **autorisieren** -Attribut auf die `Admin` Methode.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Öffnen Sie die Datei AdminController.cs und Hinzufügen der **autorisieren** -Attribut auf die gesamte `AdminController` Klasse.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC und Web-API, die beide definieren **autorisieren** Attribute, die in verschiedenen Namespaces. MVC verwendet **System.Web.Mvc.AuthorizeAttribute**, während die Web-API verwendet **"System.Web.http.AuthorizeAttribute"**.


Nur Administratoren können jetzt die Seite "Administrator" anzeigen. Wenn Sie mit dem Admin-Controller eine HTTP-Anforderung senden, muss die Anforderung außerdem ein Authentifizierungscookie enthalten. Wenn dies nicht der Fall ist, wird der Server sendet eine HTTP 401 (nicht autorisiert)-Antwort. Dies sehen Sie in Fiddler durch Senden einer GET-Anforderung zu `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-3.md)
> [Weiter](using-web-api-with-entity-framework-part-5.md)
