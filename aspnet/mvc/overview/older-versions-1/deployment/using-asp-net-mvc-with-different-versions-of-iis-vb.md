---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: Mithilfe von ASP.NET MVC mit verschiedenen Versionen von IIS (VB) | Microsoft-Dokumentation
author: microsoft
description: In diesem Tutorial erfahren Sie, wie Sie ASP.NET MVC und URL-Routing, mit verschiedenen Versionen von Internet Information Services verwenden. Erfahren Sie, verschiedene Strategien...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e997f3c7db124869a731b346a4fb3b04072fb0b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370044"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>Mithilfe von ASP.NET MVC mit verschiedenen Versionen von IIS (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> In diesem Tutorial erfahren Sie, wie Sie ASP.NET MVC und URL-Routing, mit verschiedenen Versionen von Internet Information Services verwenden. Erfahren Sie, verschiedene Strategien für die Verwendung von ASP.NET MVC mit IIS 7.0 (klassischer Modus), IIS 6.0 und früheren Versionen von IIS.


ASP.NET MVC-Framework hängt von ASP.NET-Routing zum Weiterleiten von Anforderungen zu Controlleraktionen Browser. Um die Vorteile von ASP.NET-Routing nutzen zu können, müssen Sie möglicherweise zusätzliche Konfigurationsschritte ausführen, auf dem Webserver. Dies ist die Version von Internet Information Services (IIS) und der anforderungsverarbeitungs-Modus für Ihre Anwendung abhängig.

Hier ist eine Zusammenfassung der verschiedenen Versionen von IIS:

- IIS 7.0 (integrierter Modus) – keine spezielle Konfiguration erforderlich, die ASP.NET-Routing verwenden.
- IIS 7.0 (klassischer Modus) – müssen Sie spezielle Konfiguration zur Verwendung von ASP.NET-Routing ausführen.
- IIS 6.0 oder im folgenden - müssen Sie spezielle Konfiguration zur Verwendung von ASP.NET-Routing ausführen.

Die neueste Version von IIS ist Version 7.5 (unter Win7). IIS 7 von IIS ist in mit Windows Server 2008 und VISTA SP1 und höher. Sie können auch IIS 7.0 unter jeder Version von der Vista-Betriebssystem, mit Ausnahme von Home Basic installieren (siehe [ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

IIS 7.0 unterstützt zwei Modi für die Verarbeitung von Anforderungen an. Sie können die integrierten Modus oder im klassischen Modus verwenden. Sie müssen keine speziellen Konfigurationsschritte ausführen, wenn es sich bei IIS 7.0 im integrierten Modus zu verwenden. Sie müssen jedoch zusätzliche Konfigurationsschritte ausführen, wenn Sie IIS 7.0 im klassischen Modus zu verwenden.

Microsoft Windows Server 2003 umfasst IIS 6.0. Sie können nicht IIS 6.0 zu IIS 7.0 aktualisieren, wenn das Betriebssystem Windows Server 2003 verwenden. Sie müssen zusätzliche Konfigurationsschritte ausführen, wenn Sie IIS 6.0 verwenden.

Microsoft Windows XP Professional enthält IIS 5.1. Wenn Sie IIS 5.1 verwenden, müssen Sie zusätzliche Konfigurationsschritte ausführen.

Schließlich enthält Microsoft Windows 2000 und Microsoft Windows 2000 Professional IIS 5.0. Sie müssen zusätzliche Konfigurationsschritte ausführen, wenn Sie IIS 5.0 verwenden.

## <a name="integrated-versus-classic-mode"></a>Im Vergleich zur klassischen Modus integriert

IIS 7.0 können Verarbeitung von Anforderungen, die mithilfe von zwei verschiedenen Anforderung Verarbeitungsmodi: integrierter und klassischer. Im integrierter Modus bietet eine bessere Leistung und mehr Funktionen. Im klassischen Modus ist für die Abwärtskompatibilität enthalten Kompatibilität mit früheren Versionen von IIS.

Der Verarbeitungsmodus für die Anforderung richtet sich nach den Anwendungspool. Sie können bestimmen, welchen Verarbeitungsmodus durch den Anwendungspool der Anwendung zugeordneten von einer bestimmten Webanwendung verwendet wird. Führen Sie folgende Schritte aus:

1. Starten Sie den Internetinformationsdienste-Manager
2. Wählen Sie im Fenster "Verbindungen" eine Anwendung
3. Klicken Sie im Fenster Aktionen auf der **Grundeinstellungen** Link, um das Dialogfeld "Anwendung bearbeiten" zu öffnen (siehe Abbildung 1)
4. Notieren Sie sich den Anwendungspool ausgewählt.

Standardmäßig IIS ist so konfiguriert, dass um zwei Anwendungspools zu unterstützen: **DefaultAppPool** und **Classic .NET AppPool**. Wenn DefaultAppPool ausgewählt ist, wird Ihre Anwendung im integrierten anforderungsverarbeitung-Modus ausgeführt. Wenn Classic .NET AppPool ausgewählt ist, wird Ihre Anwendung im klassischen anforderungsverarbeitung-Modus ausgeführt.


[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Abbildung 1**: ermitteln den Verarbeitungsmodus für die Anforderung ([klicken Sie, um das Bild in voller Größe anzeigen](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))


Beachten Sie, dass Sie den Modus "anforderungsverarbeitung" innerhalb der Anwendung bearbeiten-Dialogfelds ändern können. Klicken Sie auf die auswählen-Schaltfläche, und ändern Sie den Anwendungspool der Anwendung zugeordnet. Beachten Sie, dass es Kompatibilitätsprobleme, liegen Wenn Sie eine ASP.NET-Anwendung aus dem klassischen Bereitstellungsmodell auf den integrierten Modus zu ändern. Weitere Informationen finden Sie in den folgenden Artikeln:

- Aktualisieren von ASP.NET 1.1 in IIS 7.0 auf Windows Vista und WindowsServer 2008: [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- ASP.NET-Integration in IIS 7.0- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)


Wenn eine ASP.NET-Anwendung DefaultAppPool verwendet wird, dann müssen Sie zusätzliche Schritte zum Abrufen von ASP.NET-Routing (und daher ASP.NET MVC) arbeiten ausführen. Wenn die ASP.NET-Anwendung zum Verwenden von Classic .NET AppPool, und klicken Sie dann lesen Sie weiter konfiguriert ist, müssen Sie jedoch mehr Arbeit erforderlich ist.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Mithilfe von ASP.NET MVC mit älteren Versionen von IIS

Wenn Sie ASP.NET MVC mit einer älteren Version von IIS als IIS 7.0 verwenden möchten oder Sie IIS 7.0 im klassischen Modus zu verwenden müssen, müssen Sie zwei Optionen zur Verfügung. Erstens können Sie die Routingtabelle aus, um das Erweiterungen verwenden ändern. Anstelle der Anforderung einer URL wie /Store/Details würden Sie z. B. eine URL wie /Store.aspx/Details anfordern.

Die zweite Option ist das so genannte erstellen eine *Skriptzuordnung mit Platzhalter*. Eine Skriptzuordnung mit Platzhalter können Sie jede Anforderung dem ASP.NET-Framework zuzuordnen.

Wenn Sie keinen Zugriff auf Ihren Webserver (z. B. Ihre ASP.NET MVC Anwendung von einem Internetdienstanbieter gehostet wird), müssen Sie die erste Option zu verwenden. Wenn nicht die Darstellung Ihrer URLs ändern möchten, und Sie Zugriff auf Ihren Webserver haben, können Sie die zweite Option verwenden.

Jede Option in den folgenden Abschnitten ausführlich erläutert.

## <a name="adding-extensions-to-the-route-table"></a>Hinzufügen von Erweiterungen der Routentabelle

Die einfachste Möglichkeit zum Abrufen von ASP.NET-Routing zum Arbeiten mit älteren Versionen von IIS ist so ändern Sie Ihre Routentabelle, in der Datei "Global.asax". Die Standardeinstellung und unveränderten Global.asax-Datei in Codebeispiel 1 wird eine Route mit dem Namen der Standardroute konfiguriert.

**Codebeispiel 1 – "Global.asax" (unverändert)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

Die Standardroute konfiguriert, die in Codebeispiel 1 können Sie auf der Route-URLs, die wie folgt aussehen:

/ Home/Index

/ Produkt/Details/3

/ Product

Leider wird nicht in ältere Versionen von IIS diese Anforderungen an das ASP.NET-Framework übergeben werden. Aus diesem Grund, diese Anforderungen wird nicht auf einen Controller weitergeleitet werden. Z. B. Wenn Sie eine Browseranforderung für den URL/Home/Index erhalten klicken Sie dann die Seite "Fehler" in Abbildung 2 Sie.


[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Abbildung 2**: Empfangen von Fehler 404 nicht gefunden ([klicken Sie, um das Bild in voller Größe anzeigen](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))


Ältere Versionen von IIS werden nur bestimmte Anforderungen auf dem ASP.NET-Framework zuzuordnen. Die Anforderung muss eine URL mit der richtigen Dateinamenerweiterung. ASP.NET-Framework ruft z. B. eine Anforderung für /SomePage.aspx zugeordnet. Dies gelingt jedoch nicht für eine Anforderung für /SomePage.htm.

Um ASP.NET-Routing Arbeit zu erhalten, müssen wir daher die Standardroute ändern, damit sie eine Dateierweiterung enthält, die das ASP.NET-Framework zugeordnet ist.

Dies erfolgt mithilfe eines Skripts, die mit dem Namen `registermvc.wsf`. Es wurde in der ASP.NET MVC-1-Version in `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, aber seit ASP.NET 2 dieses Skript wurde in der ASP.NET Futures, verfügbar unter [ http://aspnet.codeplex.com/releases/view/39978 ](http://aspnet.codeplex.com/releases/view/39978).

Dieses Skript ausführt, registriert eine neue MVC-Erweiterung mit IIS. Nachdem Sie die MVC-Erweiterung registriert haben, können Sie die Routen in der Datei "Global.asax" ändern, damit die Routen die MVC-Erweiterung verwenden.

Die geänderte Datei "Global.asax" in Liste 2 funktioniert mit älteren Versionen von IIS.

**Codebeispiel 2 – "Global.asax" (mit Erweiterungen geändert)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]


Wichtig: Denken Sie daran, Ihre ASP.NET MVC-Anwendung zu erstellen, später die Datei "Global.asax" ändern.


Es gibt zwei wichtige Änderungen an der Datei "Global.asax" in Liste 2 ein. Es gibt jetzt zwei Routen, die in "Global.asax" definiert. Das URL-Muster für die Standardroute, die erste Route, sieht nun folgendermaßen aus:

{controller}.mvc/{action}/{id}

Das Hinzufügen der MVC-Erweiterung ändert sich den Typ der Dateien, die das Modul ASP.NET-Routing abfängt. Durch diese Änderung leitet die ASP.NET MVC-Anwendung jetzt Anforderungen wie folgt:

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

Die zweite Route, der stammroute ist neu. Diese URL-Muster für die Stamm-Route ist eine leere Zeichenfolge. Diese Route ist für den Abgleich von Anforderungen für das Stammverzeichnis der Anwendung erforderlich. Beispielsweise entspricht der stammroute eine Anforderung, die folgendermaßen aussieht:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Nach dem vornehmen dieser Änderungen an der Routingtabelle, müssen Sie sicherstellen, dass alle Verknüpfungen in Ihrer Anwendung mit dieser neuen URL-Muster kompatibel sind. Das heißt, stellen Sie sicher, dass alle Ihre Links die MVC-Erweiterung enthalten. Wenn Sie die Hilfsmethode Html.ActionLink() zum Generieren von Ihrer Links verwenden, sollten Sie keine müssen keine Änderungen vornehmen.


Anstatt das registermvc.wcf-Skript zu verwenden, können Sie eine neue Erweiterung zu IIS hinzufügen, die das ASP.NET-Framework, manuell zugeordnet ist. Wenn Sie eine neue Erweiterung selbst hinzufügen, stellen Sie sicher, dass das Kontrollkästchen mit der Bezeichnung **überprüfen, ob die Datei vorhanden ist** nicht aktiviert ist.


## <a name="hosted-server"></a>Gehosteten Server

Sie haben immer keinen Zugriff auf Ihren Webserver. Z. B. Wenn Ihrer ASP.NET MVC-Anwendung, die über einen Internet-Hosting-Anbieter gehostet werden, klicken Sie dann Sie unbedingt den Zugriff auf IIS keine.

In diesem Fall sollten Sie eine der vorhandenen Erweiterungen verwenden, die das ASP.NET-Framework zugeordnet sind. Beispiele für Dateierweiterungen zugeordnet zu ASP.NET sind die aspx-.axd und ASHX-Erweiterungen.

Die geänderte Datei "Global.asax" in Programmausdruck 3 verwendet beispielsweise die ASPX-Erweiterung anstelle der MVC-Erweiterung.

**Codebeispiel 3 – "Global.asax" (mit aspx-Erweiterungen geändert)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

Die Datei "Global.asax" in Programmausdruck 3 entspricht genau der vorherigen Datei "Global.asax", mit Ausnahme der Tatsache, dass es die ASPX-Erweiterung anstelle der MVC-Erweiterung verwendet. Sie haben keine Setup auf dem remote-Web-Server der ASPX-Erweiterung durchführen.

## <a name="creating-a-wildcard-script-map"></a>Erstellen eine Skriptzuordnung mit Platzhalter

Wenn Sie nicht, ändern Sie die URLs für Ihre ASP.NET MVC-Anwendung möchten und Zugriff auf Ihren Webserver haben, müssen Sie eine weitere Option. Sie können eine Skriptzuordnung mit Platzhalter erstellen, die alle Anforderungen an den Webserver für ASP.NET-Framework zugeordnet werden. Auf diese Weise können Sie die standardmäßige ASP.NET MVC-Route-Tabelle mit IIS 7.0 (im klassischen Modus) oder IIS 6.0 verwenden.

Denken Sie daran, dass diese Option bewirkt, IIS dass abgefangen wird jede Anforderung für den Webserver. Dies schließt Anforderungen für Bilder, klassischen ASP-Seiten und HTML-Seiten. Aus diesem Grund hat aktivieren einen Platzhalter Skriptzuordnung auf ASP.NET Auswirkungen auf die Leistung.

Hier ist, wie Sie eine Skriptzuordnung mit Platzhalter für IIS 7.0 aktivieren:

1. Wählen Sie Ihre Anwendung im Fenster "Verbindungen"
2. Stellen Sie sicher, dass die **Features** Ansicht aktiviert ist
3. Doppelklicken Sie auf die **Handlerzuordnungen** Schaltfläche
4. Klicken Sie auf die **Skriptzuordnung mit Platzhalter hinzufügen** verknüpfen (siehe Abbildung 3)
5. Geben Sie den Pfad der Aspnet\_isapi.dll-Datei (Sie können diesen Pfad über die PageHandlerFactory-Skriptzuordnung kopieren)
6. Geben Sie den Namen MVC
7. Klicken Sie auf die **OK** Schaltfläche


[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Abbildung 3**: erstellen eine Skriptzuordnung mit Platzhalter mit IIS 7.0 ([klicken Sie, um das Bild in voller Größe anzeigen](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))


Um eine Skriptzuordnung mit Platzhalter mit IIS 6.0 zu erstellen, gehen Sie wie folgt vor:

1. Mit der rechten Maustaste einer Websites, und wählen Sie Eigenschaften
2. Wählen Sie die **Basisverzeichnis** Registerkarte
3. Klicken Sie auf die **Konfiguration** Schaltfläche
4. Wählen Sie die **Zuordnungen** Registerkarte
5. Klicken Sie auf die **einfügen** Schaltfläche (siehe Abbildung 4)
6. Fügen Sie den Pfad zu der Aspnet\_isapi.dll in das ausführbare Feld (Sie können diesen Pfad von der Skriptzuordnung für ASPX-Dateien kopieren)
7. Deaktivieren Sie das Kontrollkästchen mit der Bezeichnung **überprüfen, ob die Datei vorhanden ist**
8. Klicken Sie auf die **OK** Schaltfläche


[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Abbildung 4**: erstellen eine Skriptzuordnung mit Platzhalter mit IIS 6.0 ([klicken Sie, um das Bild in voller Größe anzeigen](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))


Nach der Aktivierung von Platzhalter-Skriptzuordnungen müssen Sie die Routingtabelle, in der Datei "Global.asax" zu ändern, sodass es sich um eine stammroute enthält. Andernfalls erhalten Sie die Fehlerseite in Abbildung 5, wenn Sie eine Anforderung für die Stammseite der Anwendung vornehmen. Sie können die geänderte Datei "Global.asax" in Listing 4 verwenden.


[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Abbildung 5**: fehlende Route stammfehler ([klicken Sie, um das Bild in voller Größe anzeigen](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))


**Programmausdruck 4 – "Global.asax" (mit stammroute geändert)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

Nachdem Sie eine Skriptzuordnung mit Platzhalter für IIS 7.0 oder IIS 6.0 aktiviert haben, können Sie Anforderungen, die Arbeit mit die Standardroutingtabelle vornehmen, die wie folgt aussehen:

/

/ Home/Index

/ Produkt/Details/3

/ Product

## <a name="summary"></a>Zusammenfassung

Das Ziel in diesem Tutorial wurde erläutert, wie Sie ASP.NET MVC verwenden können, wenn Sie eine ältere Version von IIS (oder IIS 7.0 im klassischen Modus) verwenden. Zwei Methoden zum Abrufen von ASP.NET-Routing zum Arbeiten mit älteren Versionen von IIS werden erörtert: Ändern Sie die Standardroutingtabelle oder erstellen Sie eine Skriptzuordnung mit Platzhalter.

Die erste Option müssen Sie die URLs in Ihrer ASP.NET MVC-Anwendung zu ändern. Eine sehr bedeutende Vorteile dieser ersten Option ist, dass Sie keinen Zugriff auf einem Webserver benötigen um die Routingtabelle zu ändern. Das bedeutet, dass Sie die erste Option verwenden können, selbst wenn Hosten Ihrer ASP.NET MVC-Anwendung mit einem Internet-Hostingunternehmen.

Die zweite Option ist die Erstellung eine Skriptzuordnung mit Platzhalter. Der Vorteil dieser zweite Option ist, dass Sie nicht benötigen, um die URLs zu ändern. Der Nachteil dieser zweite Option ist, dass sie die Leistung Ihrer ASP.NET MVC-Anwendung auswirken kann.

> [!div class="step-by-step"]
> [Vorherige](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
