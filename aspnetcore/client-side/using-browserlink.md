---
title: Browserlink in ASP.NET Core
author: ncarandini
description: Ein Visual Studio-Feature, das links von der Entwicklungsumgebung mit mindestens einem Webbrowser
keywords: ASP.NET Core, browserlink, CSS-Synchronisierung
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 211dd5d03e6b8414e0b2ed3234d8970c92f72452
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a>Einführung in ASP.NET Core Browserlink 

Durch [Nicolò Carandini](https://github.com/ncarandini), [Mike Wasson](https://github.com/MikeWasson), und [Tom Dykstra](https://github.com/tdykstra)

Browserlink ist ein Feature in Visual Studio, der einen Kommunikationskanal zwischen der Entwicklungs- und eine oder mehrere Webbrowsern erstellt. Browser-Link, um Ihre Webanwendung gleichzeitig in mehreren Browsern aktualisieren können Sie eignet sich für browserübergreifende Tests.

## <a name="browser-link-setup"></a>Setup für Browser Link

Die ASP.NET Core **Webanwendung** -Projektvorlagen in Visual Studio 2015 und höher enthalten alles, was für Browserlink benötigen.

Browser-Link zu einem Projekt hinzugefügt, die Sie erstellt mithilfe der ASP.NET Core **leere** oder **Web-API** Vorlage, gehen Sie folgendermaßen vor:

1. Hinzufügen der *Microsoft.VisualStudio.Web.BrowserLink.Loader* Paket 
2. Hinzufügen von Konfigurationscode in die *Startup.cs* Datei.

### <a name="add-the-package"></a>Fügen Sie dem Paket hinzu

Da dies eine Visual Studio-Funktion ist, ist die einfachste Möglichkeit, das Paket hinzuzufügen, öffnen Sie die **Package Manager Console** (**Ansicht > Weitere Fenster > Package Manager Console**), und führen Sie den folgenden Befehl aus:

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

Alternativ können Sie **NuGet Package Manager**.  Mit der rechten Maustaste in des Namens des Projekts **Projektmappen-Explorer**, und wählen Sie **NuGet-Pakete verwalten**. 

![Öffnen von NuGet-Paket-Manager](using-browserlink/_static/open-nuget-package-manager.png)

Klicken Sie dann suchen Sie und installieren Sie das Paket.

![Paket mit NuGet-Paket-Manager hinzufügen](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a>Fügen Sie Konfigurationscode hinzu.

Öffnen der *Startup.cs* Datei, und klicken Sie in der `Configure` Methode fügen Sie den folgenden Code hinzu:

```csharp
app.UseBrowserLink();
```

Dieser Code in der Regel befindet sich innerhalb einer `if` blockieren, die nur in der Entwicklungsumgebung Browserlink ermöglicht, wie hier gezeigt:

[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]

Weitere Informationen finden Sie unter [Working with Multiple Environments (Verwenden von mehreren Umgebungen)](../fundamentals/environments.md).

## <a name="how-to-use-browser-link"></a>Gewusst wie: Verwenden von Browserlink

Wenn Sie eine ASP.NET Core-Projekt geöffnet haben, zeigt Visual Studio das Symbolleistensteuerelement Browserlink neben der **Debugziel** Toolbar-Steuerelement:

![Browser Link Dropdown-Menü](using-browserlink/_static/browserLink-dropdown-menu.png)

Aus dem Browserlink-Toolbar-Steuerelement können Sie folgende Aktionen ausführen:

- Aktualisieren Sie die Webanwendung gleichzeitig in mehreren Browsern
- Öffnen der **Browserlink-Dashboard**
- Aktivieren oder deaktivieren Sie **Browserlink**
- Aktivieren Sie oder deaktivieren Sie die automatische CSS-Synchronisierung

> [!NOTE]
> Einige Visual Studio-Plug-ins, vor allem Kontrollvorgänge *Web Erweiterung Pack 2015* und *Web Erweiterung Pack 2017*, bieten erweiterte Funktionalität für Browserlink, aber einige zusätzlichen Funktionen, die mit ASP funktionieren nicht. NET Core-Projekte.

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Aktualisieren Sie die Webanwendung gleichzeitig in mehreren Browsern

Verwenden Sie zum Auswählen eines einzelnen Web-Browsers zum Starten, wenn das Projekt starten das Dropdown-Menü in der **Debugziel** Toolbar-Steuerelement:

![F5-Dropdownmenü](using-browserlink/_static/debug-target-dropdown-menu.png)

Zum Öffnen von mehreren Browsern gleichzeitig wählen **durchsuchen mit... ** aus der gleichen Dropdownliste aus.  Halten Sie die STRG-Taste auf die Browsern auswählen, werden soll, und klicken Sie dann auf **Durchsuchen**:

![Viele Browsern gleichzeitig öffnen](using-browserlink/_static/open-many-browsers-at-once.png)

Hier ist ein Beispiel Screenshot der Visual Studio mit die Indexansicht öffnen und zwei geöffneten Browser:

![Synchronisieren Sie mit zwei Browser-Beispiel](using-browserlink/_static/sync-with-two-browsers-example.png)

Zeigen Sie auf dem Browserlink-Symbolleisten-Steuerelement auf den Browsern, die dem Projekt verbunden sind, finden Sie unter:

![Zeigen Sie Tipp](using-browserlink/_static/hoover-tip.png)

Ändern die Indexansicht und allen verbundenen Browsern werden aktualisiert, wenn Sie auf die Aktualisierungsschaltfläche klicken Browserlink:

![Browser-Synchronisierung auf Änderungen](using-browserlink/_static/browsers-sync-to-changes.png)

Browserlink funktioniert auch mit Browsern, die Sie außerhalb von Visual Studio starten, und navigieren Sie zu der URL der Anwendung.

### <a name="the-browser-link-dashboard"></a>Die Browserlink-Dashboard

Öffnen Sie die Browserlink-Dashboard aus dem Browserlink-Dropdown-Menü zum Verwalten einer Verbindung mit geöffneten Browser:

![Open-Browserslink-dashboard](using-browserlink/_static/open-browserlink-dashboard.png)

Wenn keine Browser verbunden ist, können Sie beginnen, nicht Debuggen Sitzung auf die _in Browser anzeigen_ Link:

![Browserlink-Dashboard-ohne-Verbindungen](using-browserlink/_static/browserlink-dashboard-no-connections.png)

Andernfalls werden die verbundenen Browser, durch den Pfad zu der Seite angezeigt, die jedem Browser angezeigt wird:

![Browserlink-Dashboard-zwei-Verbindungen](using-browserlink/_static/browserlink-dashboard-two-connections.png)

Wenn Sie möchten, können Sie auf einen aufgelisteten Browsernamen dieser einzelnen Browsersitzung aktualisieren klicken.

### <a name="enable-or-disable-browser-link"></a>Aktivieren oder Deaktivieren von Browserlink

Wenn Sie Browserlink erneut aktivieren nach dem deaktivieren, müssen Sie aktualisieren die Browser, um sie erneut eine Verbindung herstellen.

### <a name="enable-or-disable-css-auto-sync"></a>Aktivieren Sie oder deaktivieren Sie die automatische CSS-Synchronisierung

Wenn die automatische CSS-Synchronisierung aktiviert ist, werden die verbundenen Browsern automatisch aktualisiert, wenn Sie für jede Änderung an der CSS-Dateien vornehmen.

## <a name="how-does-it-work"></a>Wie funktioniert das?

Browserlink verwendet SignalR, um einen Kommunikationskanal zwischen Visual Studio und den Browser zu erstellen. Wenn Browserlink aktiviert ist, verhält sich Visual Studio als SignalR-Server, dem mehrere Clients (Browser), eine Verbindung herstellen können. Browserlink registriert auch eine middlewarekomponente in der ASP.NET-Pipeline für die Anforderung. Diese Komponente fügt spezielle `<script>` Verweise in jede Seitenanforderung vom Server. Sehen Sie dazu die Skriptverweise **Quelltext anzeigen** im Browser, und scrollen bis zum Ende der `<body>` tag-Inhalte:

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

Die Quelldateien werden nicht geändert. Die Komponente fügt die Skriptverweise dynamisch. 

Da der Browser clientseitigen Code alle JavaScript ist, funktioniert es in allen Browsern, die SignalR unterstützt, ohne dass einen beliebigen Browser-Plug-in.
