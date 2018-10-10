---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Erstellen von Hilfeseiten für ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: c081064a32151a71fc4f3ea407e0c48a1539432a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913124"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Erstellen von Hilfeseiten für ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Bei der Erstellung einer Webs-API ist es oft sinnvoll, eine Seite zu erstellen, sodass andere Entwickler zum Aufrufen Ihrer API veranschaulicht werden. Konnten Sie gesamte Dokumentation manuell erstellen, aber es ist besser, automatisch so weit wie möglich.

Um diese Aufgabe erleichtern, bietet ASP.NET Web-API für die automatische Generierung von Hilfeseiten eine Bibliothek zur Laufzeit an.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Erstellen von API-Hilfeseiten

Installieren Sie [ASP.NET und Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650). Dieses Update ist Hilfeseiten in der Web-API-Projektvorlage integriert.

Als Nächstes erstellen Sie ein neues ASP.NET MVC 4-Projekt, und wählen Sie die Web-API-Projektvorlage aus. Die Projektvorlage erstellt einen Beispiel-API-Controller mit dem Namen `ValuesController`. Die Vorlage erstellt auch die API-Hilfeseiten. Alle Codedateien für die Hilfeseite befinden sich im Ordner "Areas" des Projekts.

![](creating-api-help-pages/_static/image2.png)

Wenn Sie die Anwendung ausführen, enthält der Startseite eine Verknüpfung mit der API-Hilfeseite an. Auf der Startseite ist der relative Pfad ' / help '.

![](creating-api-help-pages/_static/image3.png)

Diesen Link gelangen Sie zu einer Seite "API-Zusammenfassung".

![](creating-api-help-pages/_static/image4.png)

Die MVC-Ansicht für diese Seite wird im Areas/HelpPage/Views/Help/Index.cshtml definiert. Sie können diese Seite, um das Layout, Einführung, Titel, Stile usw. ändern bearbeiten.

Der Hauptteil der Seite ist eine Tabelle mit APIs, die vom Netzwerkcontroller gruppiert. Die Tabelleneinträge dynamisch generiert werden, mithilfe der **IApiExplorer** Schnittstelle. (Werde ich mehr über diese Schnittstelle später eingehen.) Wenn Sie einen neuen API-Controller hinzufügen, wird die Tabelle automatisch zur Laufzeit aktualisiert werden.

Die "API"-Spalte enthält den HTTP-Methode und des relativen URI. Die Spalte "Beschreibung" enthält die Dokumentation für jede API. Die Dokumentation ist zunächst nur Platzhaltertext. Im nächsten Abschnitt zeige ich Ihnen wie Dokumentation aus XML-Kommentare hinzugefügt.

Jede API verfügt über einen Link zu einer Seite mit ausführlicheren Informationen, einschließlich Beispiel Anforderungs- und Antworttext.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Hilfeseiten zu einem vorhandenen Projekt hinzufügen

Sie können Hilfeseiten zu einem vorhandenen Web-API-Projekt mithilfe von NuGet-Paket-Manager hinzufügen. Diese Option ist nützlich, die Sie von einer anderen Vorlage als die Vorlage "Web-API" beginnen.

Aus der **Tools** die Option **NuGet Paket-Manager**, und wählen Sie dann **Paket-Manager Konsole**. In der [-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Fenster, geben Sie einen der folgenden Befehle aus:

Für eine **c#** Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Für eine **Visual Basic** Anwendung: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Es gibt zwei Pakete, die für C#- und 1 für Visual Basic. Stellen Sie sicher, dass das Konto verwendet, das Ihr Projekt entspricht.

Dieser Befehl installiert die erforderlichen Assemblys und fügt die MVC-Ansichten für die Hilfeseiten (befindet sich im Ordner "Bereiche/HelpPage"). Sie müssen manuell einen Link zur Hilfeseite für die hinzufügen. Der URI ist ' / help '. Um einen Link in einer Razor-Ansicht zu erstellen, fügen Sie Folgendes hinzu:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Stellen Sie außerdem sicher, um Bereiche zu registrieren. Fügen Sie in der Datei "Global.asax" den folgenden Code, der **Anwendung\_starten** -Methode, sofern sie nicht bereits angezeigt wird:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Hinzufügen der API-Dokumentation

Standardmäßig werden in der Hilfe Seiten sind Platzhalter-Zeichenfolgen für die Dokumentation. Sie können [XML-Dokumentationskommentare](https://msdn.microsoft.com/library/b2s063f7.aspx) der Dokumentation zu erstellen. Um dieses Feature zu aktivieren, öffnen Sie die Bereiche/HelpPage/App-Datei\_Start/HelpPageConfig.cs und kommentieren Sie die folgende Zeile:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Jetzt aktivieren Sie XML-Dokumentation. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **Eigenschaften**. Wählen Sie die **erstellen** Seite.

![](creating-api-help-pages/_static/image6.png)

Klicken Sie unter **Ausgabe**, überprüfen Sie **XML-Dokumentationsdatei**. Geben Sie in das Bearbeitungsfeld "App\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Öffnen Sie als Nächstes den Code für die `ValuesController` -API-Controller, der im /Controllers/ValuesControler.cs definiert ist. Die Controllermethoden einige Dokumentationskommentare hinzugefügt. Zum Beispiel:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Tipp: Wenn Sie positionieren den Textcursor in der Zeile über der Methode, und geben Sie drei Schrägstriche, fügt Visual Studio automatisch die XML-Elemente. Sie können dann in die leeren Felder ausfüllen.


Jetzt erstellen Sie und führen Sie die Anwendung erneut aus, und navigieren Sie zu den Hilfeseiten. Die Dokumentationszeichenfolgen sollte in der API-Tabelle angezeigt werden.

![](creating-api-help-pages/_static/image8.png)

Die Hilfeseite liest die Zeichenfolgen aus der XML-Datei zur Laufzeit. (Wenn Sie die Anwendung bereitstellen, stellen Sie sicher, dass die XML-Datei bereitstellen.)

## <a name="under-the-hood"></a>Im Hintergrund

Hilfeseiten basieren auf der die **ApiExplorer** Klasse, die Bestandteil des Web-API-Framework ist. Die **ApiExplorer** Klasse stellt das Material für das Erstellen einer Hilfeseite ein. Für jede API **ApiExplorer** enthält ein **ApiDescription** , die die API beschreibt. Zu diesem Zweck wird eine "API" als die Kombination von HTTP-Methode und des relativen URI definiert. Hier sind z. B. einige unterschiedliche APIs:

- /Api/Products abrufen
- Abrufen Sie/API/Produkte / {Id}
- Veröffentlichen von Produkten/api /

Wenn eine Controlleraktion mehrere HTTP-Methoden, unterstützt die **ApiExplorer** behandelt jede Methode als distinct-API.

Ausblenden eine API aus dem **ApiExplorer**, Hinzufügen der **ApiExplorerSettings** -Attribut auf die Aktion und den Satz *IgnoreApi* auf "true".

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Sie können dieses Attribut auch auf den Controller, für den gesamten Controller ausschließen hinzufügen.

Die ApiExplorer Klasse ruft Dokumentationszeichenfolgen aus der **IDocumentationProvider** Schnittstelle. Wie Sie gesehen haben, um die Hilfeseiten-Bibliothek bietet eine **IDocumentationProvider** Dokumentation von XML-Dokumentationszeichenfolgen abruft. Der Code befindet sich im /Areas/HelpPage/XmlDocumentationProvider.cs. Erhalten Sie Dokumentation aus einer anderen Quelle durch Schreiben eigener **IDocumentationProvider**. Aufrufen, um diese verknüpfen, die **SetDocumentationProvider** Erweiterungsmethode, die in definierten **HelpPageConfigurationExtensions**

**ApiExplorer** ruft automatisch in die **IDocumentationProvider** Schnittstelle zum Abrufen von Dokumentationszeichenfolgen für jede API. Er speichert sie in der **Dokumentation** Eigenschaft der **ApiDescription** und **ApiParameterDescription** Objekte.

## <a name="next-steps"></a>Nächste Schritte

Sie sind nicht auf den hier gezeigten Hilfeseiten beschränkt. In der Tat **ApiExplorer** ist nicht zum Erstellen von Hilfeseiten beschränkt. Yao Huang Lin verfügt über einige großartige Blogbeiträge, rufen Sie vielleicht zum Nachdenken standardmäßig geschrieben:

- [ASP.NET Web API-Hilfeseite hinzugefügt einen einfachen Test-Client](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Vornehmen von ASP.NET Web API-Hilfeseite für selbst gehostete Dienste arbeiten](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Generieren zur Entwurfszeit von Hilfe Seite (oder clientarbeitsauslastungen) für ASP.NET Web-API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Erweiterte Hilfeseite-Anpassungen](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
