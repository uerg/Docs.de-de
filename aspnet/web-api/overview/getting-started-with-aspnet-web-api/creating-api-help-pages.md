---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "Erstellen von Hilfeseiten für ASP.NET Web-API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 18d04492529e96b6c0e14f1d7a30378b4832f4c8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Erstellen von Hilfeseiten für ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Wenn Sie eine Web-API erstellen, ist es oft hilfreich, erstellen Sie eine Hilfeseite, damit andere Entwickler wissen, wie Ihre API aufgerufen werden. Gesamte Dokumentation konnte manuell erstellen, aber es ist besser, automatisch so weit wie möglich.

Um diese Aufgabe zu vereinfachen, enthält der ASP.NET Web-API eine Bibliothek für die automatische Generierung von Hilfeseiten zur Laufzeit.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>API-Hilfeseiten erstellen

Installieren Sie [ASP.NET und Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650). Dieses Update wird Hilfeseiten in der Web-API-Projektvorlage integriert.

Als Nächstes erstellen Sie ein neues ASP.NET MVC 4-Projekt, und wählen Sie die Web-API-Projektvorlage. Die Projektvorlage erstellt einen Beispiel-API-Controller mit dem Namen `ValuesController`. Die Projektvorlage erstellt auch die API-Hilfeseiten. Alle von die Codedateien für die Hilfeseite befinden sich im Ordner "Bereiche" des Projekts.

![](creating-api-help-pages/_static/image2.png)

Wenn Sie die Anwendung ausführen, enthält der Homepage einen Link, um die API-Hilfeseite an. Über die Startseite ist der relative Pfad/Help.

![](creating-api-help-pages/_static/image3.png)

Dieser Link stellt eine Seite "Zusammenfassung API".

![](creating-api-help-pages/_static/image4.png)

Das MVC-Ansicht für diese Seite wird in Areas/HelpPage/Views/Help/Index.cshtml definiert. Sie können diese Seite zum Ändern des Layouts, Einführung, Titel, Formatvorlagen und usw. bearbeiten.

Der Hauptteil der Seite ist eine Tabelle von APIs, die vom Netzwerkcontroller gruppiert. Die Tabelleneinträge dynamisch generiert werden, mithilfe der **IApiExplorer** Schnittstelle. (Weitere Informationen über diese Schnittstelle später werde ich.) Wenn Sie einen neue API-Controller hinzufügen, wird die Tabelle automatisch zur Laufzeit aktualisiert.

Die Spalte "API" listet die HTTP-Methode und die relative URI. Die Spalte "Beschreibung" enthält die Dokumentation für jede API. Zu Beginn wird die Dokumentation nur Platzhaltertext. Im nächsten Abschnitt zeige ich Ihnen wie Dokumentation aus XML-Kommentare hinzugefügt.

Jede API verfügt über einen Link zu einer Seite mit ausführlicheren Informationen, einschließlich Beispiel Anfrage- und Antworttexte.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Hilfeseiten zu einem vorhandenen Projekt hinzufügen

Sie können Hilfeseiten zu einem vorhandenen Web-API-Projekt mithilfe von NuGet-Paket-Manager hinzufügen. Diese Option ist nützlich, die Sie von einer anderen Vorlage als der Vorlage "Web-API" beginnen.

Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. In der [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Fenster, geben Sie einen der folgenden Befehle aus:

Für eine **c#** Anwendung:`Install-Package Microsoft.AspNet.WebApi.HelpPage`

Für eine **Visual Basic** Anwendung:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Es gibt zwei Pakete, für C#- und eine für Visual Basic. Stellen Sie sicher, dass die zu verwenden, die Ihr Projekt entspricht.

Mit diesem Befehl werden die erforderlichen Assemblys und fügt die MVC-Ansichten für die Hilfeseiten (befindet sich im Ordner "Bereiche/HelpPage"). Sie müssen manuell einen Link auf der Seite "Hilfe" hinzuzufügen. Der URI ist/Help. Um einen Link in einer Razor-Ansicht zu erstellen, fügen Sie Folgendes hinzu:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Stellen Sie außerdem sicher, dass Bereiche zu registrieren. Fügen Sie in der Datei "Global.asax" den folgenden Code, der **Anwendung\_starten** -Methode, wenn sie nicht bereits angezeigt wird:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Hinzufügen von API-Dokumentation

Standardmäßig wird die Hilfe über Seiten verfügen Platzhalter-Zeichenfolgen für die Dokumentation. Sie können [XML-Dokumentationskommentare](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx) die Dokumentation zu erstellen. Um dieses Feature zu aktivieren, öffnen Sie die Datei Bereiche/HelpPage/App\_Start/HelpPageConfig.cs und die auskommentierung der folgenden Zeile:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Aktivieren Sie jetzt die XML-Dokumentation. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **Eigenschaften**. Wählen Sie die **erstellen** Seite.

![](creating-api-help-pages/_static/image6.png)

Klicken Sie unter **Ausgabe**, überprüfen Sie **XML-Dokumentationsdatei**. Geben Sie im Bearbeitungsfeld "App\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Als Nächstes öffnen Sie den Code für die `ValuesController` -API-Controller, der im /Controllers/ValuesControler.cs definiert ist. Fügen Sie einige Dokumentationskommentare, auf die Controllermethoden. Zum Beispiel:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Tipp: Wenn Sie positionieren die Einfügemarke in der Zeile über der Methode, und geben Sie drei Schrägstriche, fügt Visual Studio automatisch die XML-Elemente. Sie können Sie die Leerzeichen eingeben.


Erstellen Sie nun und die Anwendung erneut auszuführen, und navigieren Sie zu der Hilfeseiten. Die Dokumentationszeichenfolgen sollte in der API-Tabelle angezeigt werden.

![](creating-api-help-pages/_static/image8.png)

Die Hilfeseite liest die Zeichenfolgen aus der XML-Datei zur Laufzeit. (Wenn Sie die Anwendung bereitstellen, stellen Sie sicher, zum Bereitstellen der XML-Datei.)

## <a name="under-the-hood"></a>Hinter den Kulissen

Die Hilfeseiten basieren auf der Basis von der **ApiExplorer** Klasse, die Teil des Web-API-Framework ist. Die **ApiExplorer** Klasse bietet das Material für eine Hilfeseite erstellen. Für jede API **ApiExplorer** enthält ein **ApiDescription** , beschreibt die API. Zu diesem Zweck wird eine "API" als Kombination aus HTTP-Methode und der relativen URI definiert. Hier sind z. B. einige distinct-APIs:

- /Api/Products abrufen
- Abrufen Sie/API/Produkte / {Id}
- Bereitstellen Sie/api /-Produkte

Wenn eine Controlleraktion mehrere HTTP-Methoden unterstützt die **ApiExplorer** behandelt jede Methode als eine distinct-API.

Ausblenden eine API aus dem **ApiExplorer**, Hinzufügen der **ApiExplorerSettings** -Attribut auf die Aktion, und legen *IgnoreApi* auf "true".

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Sie können dieses Attribut auch auf dem Controller an, um den gesamten Controller auszuschließen hinzufügen.

Die Klasse ApiExplorer ruft Dokumentationszeichenfolgen aus dem **IDocumentationProvider** Schnittstelle. Wie weiter oben gezeigt, um die Hilfeseiten-Bibliothek stellt eine **IDocumentationProvider** Dokumentation von XML-Dokumentationszeichenfolgen abruft. Der Code befindet sich im /Areas/HelpPage/XmlDocumentationProvider.cs. Erhalten Sie Dokumentation aus einer anderen Quelle durch Schreiben von eigenen **IDocumentationProvider**. Aufrufen, um es von Netzwerkdaten, die **SetDocumentationProvider** in definierte Erweiterungsmethode **HelpPageConfigurationExtensions**

**ApiExplorer** ruft automatisch in die **IDocumentationProvider** Schnittstelle, um Dokumentationszeichenfolgen für jede API abzurufen. Er speichert sie in der **Dokumentation** Eigenschaft von der **ApiDescription** und **ApiParameterDescription** Objekte.

## <a name="next-steps"></a>Nächste Schritte

Sie sind nicht auf die hier gezeigten Hilfeseiten beschränkt. In der Tat **ApiExplorer** ist nicht auf das Erstellen von Hilfeseiten beschränkt. Yao Huang Lin wurde geschrieben, dass einige hervorragende Blogeinträge von jovan um erhalten Sie denken ausgegeben:

- [Hinzufügen von einer einfachen Testclient zu ASP.NET Web API-Hilfeseite](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Erstellen von ASP.NET Web-API Hilfeseite für selbst gehostete Dienste arbeiten](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Während der Entwurfszeit-Generierung von Hilfe zur Seite "(oder clientarbeitsauslastungen) für ASP.NET Web-API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Erweiterte Hilfeseite-Anpassungen](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
