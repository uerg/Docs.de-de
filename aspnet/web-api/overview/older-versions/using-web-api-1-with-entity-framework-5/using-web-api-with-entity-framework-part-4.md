---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Teil 4: Hinzufügen der Ansicht für ein Admin | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879529"
---
<a name="part-4-adding-an-admin-view"></a>Teil 4: Hinzufügen einer-Administratoransicht
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Fügen Sie eine Administrator-Ansicht

Nun wir aktivieren Sie auf der Clientseite und fügen Sie eine Seite, die Daten aus den Admin-Controller verwenden kann. Die Seite ermöglichen es Benutzern zu erstellen, bearbeiten oder Löschen von Produkten, per AJAX-Anforderungen an den Controller.

Im Projektmappen-Explorer erweitern Sie den Ordner Controller, und öffnen Sie die Datei mit dem Namen HomeController.cs. Diese Datei enthält einen MVC-Controller. Fügen Sie eine Methode mit dem Namen `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

Die **HttpRouteUrl** Methode erstellt den URI der Web-API, und wir dies in den ansichtsbehälter für die spätere speichern.

Als Nächstes positionieren Sie den Textcursor innerhalb der `Admin` Aktionsmethode, und klicken Sie dann mit der rechten Maustaste und wählen Sie **Ansicht hinzufügen**. Hierdurch erscheint der **Ansicht hinzufügen** Dialogfeld.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

In der **Ansicht hinzufügen** Dialogfeld Name der Ansicht "Admin". Aktivieren Sie das Kontrollkästchen mit der Bezeichnung **eine stark typisierte Ansicht erstellen**. Klicken Sie unter **Modellklasse**, wählen Sie "Product (ProductStore.Models)". Behalten Sie alle anderen Optionen die Standardwerte bei.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Auf **hinzufügen** Fügt eine Datei mit dem Namen Admin.cshtml unter Ansichten/Start hinzu. Öffnen Sie diese Datei, und fügen Sie folgenden HTML-Code hinzu. Folgenden HTML-Code definiert die Struktur der Seite, jedoch keine Funktionalität läuft noch.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Erstellen Sie eine Verknüpfung zur Seite "Administrator"

Klicken Sie im Projektmappen-Explorer erweitern Sie den Ordner Views, und erweitern Sie dann den Ordner Shared. Öffnen Sie die Datei mit dem Namen \_Layout.cshtml. Suchen Sie die **Ul** Element mit Id = "im Menü" und einen Aktionslink für die Admin-Ansicht:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> In das Beispielprojekt vorgenommen ich einigen andere kosmetischen Änderungen, z. B. die Zeichenfolge "Ihr Logo hier einfügen" ersetzt. Diese nicht auf die Funktionalität der Anwendung auswirken. Sie können das Projekt herunterladen und vergleichen Sie die Dateien.


Führen Sie die Anwendung, und klicken Sie auf den Link "Admin", der am oberen Rand auf der Startseite angezeigt wird. Die Seite "Administrator" sollte wie folgt aussehen:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Rechts jetzt die Seite keine Auswirkung. Im nächsten Abschnitt werden wir Sie Knockout.js verwenden, um einer dynamischen Benutzeroberfläche zu erstellen.

## <a name="add-authorization"></a>Hinzufügen von Autorisierung

Die Seite "Administrator" wird aktuell für Personen, die auf der Website zugegriffen werden kann. Ändern Sie diese Option, um die Berechtigungen für Administratoren einschränken.

Durch Hinzufügen einer Rolle "Administrator" und einen Benutzer mit Administratorrechten starten. Im Projektmappen-Explorer erweitern Sie den Filter-Ordner, und öffnen Sie die Datei mit dem Namen InitializeSimpleMembershipAttribute.cs. Suchen Sie die `SimpleMembershipInitializer` Konstruktor. Nach dem Aufruf von **WebSecurity.InitializeDatabaseConnection**, fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Dies ist eine schnelle Möglichkeit zum Hinzufügen der Rolle "Administrator", und erstellen Sie einen Benutzer für die Rolle.

Im Projektmappen-Explorer erweitern Sie den Ordner Controller, und öffnen Sie die Datei "HomeController.cs". Hinzufügen der **autorisieren** -Attribut auf die `Admin` Methode.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Öffnen Sie die Datei AdminController.cs und Hinzufügen der **autorisieren** -Attribut auf die gesamte `AdminController` Klasse.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC und Web-API, die beide definieren **autorisieren** Attribute, die in verschiedenen Namespaces. MVC verwendet **System.Web.Mvc.AuthorizeAttribute**, während der Web-API verwendet **System.Web.Http.AuthorizeAttribute**.


Nur Administratoren können jetzt die Seite "Administrator" anzeigen. Wenn Sie eine HTTP-Anforderung an den Controller Admin senden, muss die Anforderung außerdem ein Authentifizierungscookie enthalten. Wenn dies nicht der Fall ist, wird der Server sendet eine HTTP 401 (nicht autorisiert)-Antwort. Sehen Sie dies in Fiddler durch Senden einer GET-Anforderung an `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-3.md)
> [Weiter](using-web-api-with-entity-framework-part-5.md)
