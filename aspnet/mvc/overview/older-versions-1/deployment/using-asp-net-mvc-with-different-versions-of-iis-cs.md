---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Mithilfe von ASP.NET MVC mit verschiedenen Versionen von IIS (c#) | Microsoft Docs
author: microsoft
description: In diesem Lernprogramm erfahren Sie, wie Sie ASP.NET MVC und die URL-Routing, mit verschiedenen Versionen von Internetinformationsdienste (IIS) verwenden. Erfahren Sie, verschiedene Strategien...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: fdd024aba399f26e9ef7d01a00078cd3d5750d94
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Mithilfe von ASP.NET MVC mit verschiedenen Versionen von IIS (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> In diesem Lernprogramm erfahren Sie, wie Sie ASP.NET MVC und die URL-Routing, mit verschiedenen Versionen von Internetinformationsdienste (IIS) verwenden. Erfahren Sie, verschiedene Strategien für die Verwendung von ASP.NET MVC mit IIS 7.0 (klassischen Modus), IIS 6.0 und früheren Versionen von IIS.


ASP.NET MVC-Framework hängt von ASP.NET-Routing zum Routen von Anforderungen auf Controlleraktionen Browser. Um der ASP.NET-Routing nutzen können, müssen Sie möglicherweise zusätzliche Konfigurationsschritte ausführen, auf dem Webserver. Es hängt alles die Version von Internet Information Services (IIS) und der Modus für Ihre Anwendung bei der anforderungsverarbeitung.

Hier wird ein Überblick über die verschiedenen Versionen von IIS:

- IIS 7.0 (integrierter Modus) – keine besondere Konfiguration notwendig, ASP.NET-Routing zu verwenden.
- IIS 7.0 (klassischen Modus) – müssen Sie besondere Konfiguration Verwendung ASP.NET-Routing ausführen.
- IIS 6.0 oder niedriger - müssen Sie besondere Konfiguration Verwendung ASP.NET-Routing ausführen.

Die neueste Version von IIS ist Version 7.5 (Win7). IIS 7 von IIS ist in mit Windows Server 2008 und VISTA/SP1 und höher. Sie können auch auf eine beliebige Version des Betriebssystems Vista mit Ausnahme von Home Basic installieren IIS 7.0 (finden Sie unter [https://technet.microsoft.com/en-us/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/en-us/library/cc731179%28WS.10%29.aspx)).

IIS 7.0 unterstützt zwei Modi für die Verarbeitung von Anforderungen. Sie können die integrierten Modus oder im klassischen Modus verwenden. Sie müssen keine speziellen Konfigurationsschritte ausführen, wenn es sich bei IIS 7.0 im integrierten Modus zu verwenden. Allerdings müssen Sie zusätzliche Konfigurationsschritte vornehmen, wenn IIS 7.0 im klassischen Modus verwendet.

Microsoft Windows Server 2003 enthält IIS 6.0. Sie können nicht IIS 6.0 zu IIS 7,0 aktualisiert, wenn das Betriebssystem Windows Server 2003 verwenden. Wenn IIS 6.0 verwenden, müssen Sie zusätzliche Konfigurationsschritte ausführen.

Microsoft Windows XP Professional schließt IIS 5.1. Wenn Sie IIS 5.1 verwenden, müssen Sie zusätzliche Konfigurationsschritte ausführen.

Schließlich enthält Microsoft Windows 2000 und Microsoft Windows 2000 Professional IIS 5.0. Bei IIS 5.0 verwenden, müssen Sie zusätzliche Konfigurationsschritte ausführen.

## <a name="integrated-versus-classic-mode"></a>Im Vergleich zur klassischen Modus integriert

IIS 7.0 können verarbeitet werden, die mit zwei verschiedenen Anforderung Verarbeitungsmodi: integrierte und klassische. Im integrierter Modus bietet eine bessere Leistung und weitere Funktionen. Im klassischer Modus wird aus Gründen der Abwärtskompatibilität mit früheren Versionen von IIS.

Der Verarbeitungsmodus für die Anforderung wird durch den Anwendungspool bestimmt. Sie können bestimmen, welche Verarbeitungsmodus von einem bestimmten Web-Anwendung von der Anwendung zugeordneten den Anwendungspool verwendet wird. Führen Sie folgende Schritte aus:

1. Starten Sie den Internetinformationsdienste-Manager
2. Wählen Sie im Fenster Verbindungen eine Anwendung
3. Klicken Sie im Fenster Aktionen auf der **Grundeinstellungen** klicken, um das Dialogfeld "Anwendung bearbeiten" zu öffnen (siehe Abbildung 1)
4. Notieren Sie den Anwendungspool ausgewählt.

Standardmäßig ist IIS zur Unterstützung von zwei Anwendungspools konfiguriert: **DefaultAppPool** und **Classic .NET AppPool**. Wenn DefaultAppPool ausgewählt ist, wird die Anwendung im integrierten Anforderung Verarbeitungsmodus ausgeführt. Wenn Classic .NET AppPool ausgewählt ist, wird die Anwendung im klassischen anforderungsverarbeitung-Modus ausgeführt.

[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Abbildung 1**: Erkennen des Anforderung erforderliche Modus ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Beachten Sie, dass Sie den Verarbeitungsmodus aus Anforderung in das Dialogfeld Anwendung bearbeiten ändern können. Klicken Sie auf die Schaltfläche auswählen, und ändern Sie des Anwendungspools, die der Anwendung zugeordnet. Beachten Sie, dass beim Ändern von einer ASP.NET-Anwendung aus klassischen auf den integrierten Modus Kompatibilitätsprobleme vorliegen. Weitere Informationen finden Sie in den folgenden Artikeln:

- Upgrade von ASP.NET 1.1 auf IIS 7.0 unter Windows Vista und WindowsServer 2008 – [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Integration von ASP.NET in IIS 7.0 - [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Wenn eine ASP.NET-Anwendung DefaultAppPool verwendet wird, müssen Sie zusätzliche Schritte zum Abrufen von ASP.NET-Routing (und daher ASP.NET-MVC) arbeiten ausführen. Wenn die ASP.NET-Anwendung konfiguriert ist, um die Classic .NET AppPool verwenden, und klicken Sie dann lesen, müssen Sie jedoch weitere Arbeit an.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Mithilfe von ASP.NET MVC mit älteren Versionen von IIS

Wenn Sie ASP.NET MVC mit einer älteren Version von IIS vor IIS 7.0 verwenden müssen, oder Sie IIS 7.0 im klassischen Modus verwenden müssen, müssen Sie zwei Optionen zur Verfügung. Erstens können Sie die Routentabelle Dateierweiterungen Verwendung ändern. Statt eine URL wie /Store/Details anfordern, würden Sie z. B. eine URL wie /Store.aspx/Details anfordern.

Die zweite Möglichkeit besteht, so genannte erstellen eine *Skriptzuordnung mit Platzhalter*. Eine Zuordnung mit Platzhalterzeichen Skript können Sie jede Anforderung in dem ASP.NET-Framework zuordnen.

Wenn Sie keinen Zugriff auf Ihren Webserver (z. B. Ihre ASP.NET MVC Anwendung von einem Internetdienstanbieter gehostet wird), müssen Sie die erste Option zu verwenden. Wenn Sie nicht, ändern Sie die Darstellung Ihrer URLs möchten und Sie Zugriff auf Ihren Webserver haben, können Sie die zweite Option verwenden.

Jede Option in den folgenden Abschnitten ausführlich untersucht.

## <a name="adding-extensions-to-the-route-table"></a>Hinzufügen von Erweiterungen an der Routingtabelle

Die einfachste Möglichkeit zum Abrufen von ASP.NET-Routing zum Arbeiten mit älteren Versionen von IIS ist so ändern Sie die Routingtabelle in der Datei "Global.asax". Die Standardeinstellung und nicht geänderten Datei "Global.asax" im Codebeispiel 1 wird eine Route mit dem Namen der Standardroute konfiguriert.

**Auflisten von 1 – "Global.asax" (unverändert)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

Die Standardroute im Codebeispiel 1 konfigurierten ermöglicht Route-URLs, die wie folgt aussehen:

/ Home/Index

/ Produkt/Informationen/3

/ Produkt

Leider wird nicht in ältere Versionen von IIS diese Anforderungen auf dem ASP.NET-Framework übergeben werden. Aus diesem Grund wird nicht diese Anforderungen auf einen Domänencontroller weitergeleitet werden. Z. B. Wenn Sie eine Browseranforderung für den/URL-Home/Index vornehmen erhalten klicken Sie dann die Seite "Fehler" in Abbildung 2 Sie.

[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Abbildung 2**: Empfangen von Fehler 404 Nichtgefunden ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Ältere Versionen von IIS wird lediglich bestimmte Anforderungen auf dem ASP.NET-Framework zugeordnet. Die Anforderung muss für einen URL an, mit der rechten Dateierweiterung. Eine Anforderung für /SomePage.aspx ruft z. B. dem ASP.NET-Framework zugeordnet. Wird jedoch eine Anforderung für /SomePage.htm nicht.

Um ASP.NET-Routing zu erhalten, müssen wir daher die Standardroute ändern, damit sie eine Dateierweiterung enthält, die das dem ASP.NET-Framework zugeordnet ist.

Dies erfolgt mithilfe eines Skripts, mit dem Namen `registermvc.wsf`. Es wurde der ASP.NET MVC-1-Version im Lieferumfang `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, aber ab ASP.NET 2 dieses Skript wurde in der ASP.NET Futures, verfügbar unter [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).

Dieses Skript ausführen, wird eine neue MVC-Erweiterung mit IIS registriert. Nachdem Sie die MVC-Erweiterung registriert haben, können Sie Ihre Routen in der Datei "Global.asax" ändern, so, dass die Routen, die MVC-Erweiterung verwenden.

Die geänderte Datei "Global.asax" in der Liste 2 funktioniert mit älteren Versionen von IIS.

**Auflisten von 2 – "Global.asax" (mit Erweiterungen geändert)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Wichtige**: Denken Sie daran, Ihre ASP.NET MVC-Anwendung erstellen, nach Ändern der Datei "Global.asax" erneut.

Es gibt zwei wichtige Änderungen an der Datei "Global.asax" 2 aufgelistet. Es sind nun zwei Routen, die in der Datei Global.asax definiert. Das URL-Muster für die Standardroute die erste Route sieht jetzt wie folgt:

{controller}.mvc/{action}/{id}

Das Hinzufügen der MVC-Erweiterung ändert den Typ der Dateien, die das Modul ASP.NET-Routing abfängt. Durch diese Änderung leitet die ASP.NET MVC-Anwendung jetzt Anforderungen wie folgt:

/Home.Mvc/Index/

/Product.Mvc/Details/3

/Product.Mvc/

Die zweite Route, die Route Stamm ist neu. Diese URL-Muster für die Route Stamm ist eine leere Zeichenfolge. Diese Route ist notwendig, damit das Stammverzeichnis der Anwendung gerichtete Anforderungen entsprechen. Beispielsweise entspricht die Stamm-Route eine Anforderung, die wie folgt aussieht:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Nachdem Sie diese Änderungen an der Routingtabelle haben, müssen Sie sicherstellen, dass alle Verknüpfungen in der Anwendung mit dieser neuen URL-Muster kompatibel sind. Das heißt, stellen Sie sicher, dass alle Ihre Links die Erweiterung "MVC" enthalten. Wenn Sie die Hilfsmethode Html.ActionLink() zum Generieren von Links verwenden, müssen Sie keine Änderungen vornehmen.

Anstatt das Skript registermvc.wcf zu verwenden, können Sie eine neue Erweiterung zu IIS hinzufügen, die von hand auf dem ASP.NET-Framework zugeordnet ist. Wenn Sie eine neue Erweiterung selbst hinzufügen, stellen Sie sicher, dass das Kontrollkästchen mit der Bezeichnung **Vergewissern Sie sich diese Datei vorhanden ist** nicht aktiviert ist.

## <a name="hosted-server"></a>Gehosteten Server

Sie haben immer keinen Zugriff auf Ihren Webserver. Z. B. Wenn Sie Ihre ASP.NET MVC-Anwendung mit einem Internet-Hostinganbieter hosten, wird nicht dann Sie unbedingt IIS zugreifen können.

In diesem Fall sollten Sie eine der vorhandenen Dateierweiterungen verwenden, die auf dem ASP.NET-Framework zugeordnet sind. Beispiele für Erweiterungen, die ASP.NET zugeordnet sind die aspx-.axd und ASHX-Erweiterungen.

Die geänderte Datei "Global.asax" 3 auflisten verwendet beispielsweise die ASPX-Erweiterung anstelle der MVC-Erweiterung.

**Auflisten von 3 – "Global.asax" (mit aspx-Erweiterungen geändert)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Die Datei "Global.asax" auflisten 3 entspricht genau die vorherige Datei "Global.asax" mit Ausnahme der Tatsache, dass sie die ASPX-Erweiterung anstelle von der MVC-Erweiterung verwendet. Sie müssen Setup auf dem remote-Webserver für die Verwendung der ASPX-Erweiterung ausführen.

## <a name="creating-a-wildcard-script-map"></a>Erstellen eine Skriptzuordnung mit Platzhalter

Wenn Sie nicht die URLs für ASP.NET MVC-Anwendung ändern möchten, und Sie Zugriff auf Ihren Webserver haben, müssen Sie eine zusätzliche Option. Sie können eine Zuordnung mit Platzhalterzeichen Skript erstellen, die alle Anforderungen an den Webserver auf dem ASP.NET-Framework zuordnet. Auf diese Weise können Sie die Standardeinstellung ASP.NET MVC-Routingtabelle mit IIS 7.0 (im klassischen Modus) oder IIS 6.0 verwenden.

Denken Sie daran, dass diese Option bewirkt, dass IIS den Webserver gesendete Anforderungen abfangen. Dies schließt Anforderungen für Bilder, klassische ASP-Seiten und HTML-Seiten. Aus diesem Grund besitzt Aktivieren eines Platzhalters Skriptzuordnung auf ASP.NET Leistungseinbußen.

Hier ist, wie Sie eine Zuordnung mit Platzhalterzeichen Skript für IIS 7.0 aktiviert:

1. Wählen Sie Ihre Anwendung im Fenster Verbindungen
2. Stellen Sie sicher, dass die **Funktionen** Ansicht ausgewählt ist
3. Doppelklicken Sie auf die **Handlerzuordnungen** Schaltfläche
4. Klicken Sie auf die **Skriptzuordnung mit Platzhalter hinzufügen** verknüpfen (siehe Abbildung 3)
5. Geben Sie den Pfad der Aspnet\_isapi.dll-Datei (Sie können diesen Pfad aus der Skriptzuordnung PageHandlerFactory kopieren)
6. Geben Sie den Namen MVC
7. Klicken Sie auf die **OK** Schaltfläche

[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Abbildung 3**: erstellen eine Zuordnung mit Platzhalterzeichen Skript mit IIS 7.0 ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Führen Sie diese Schritte, um eine Zuordnung mit Platzhalterzeichen Skript mit IIS 6.0 zu erstellen:

1. Mit der rechten Maustaste einer Websites, und wählen Sie Eigenschaften
2. Wählen Sie die **Basisverzeichnis** Registerkarte
3. Klicken Sie auf die **Konfiguration** Schaltfläche
4. Wählen Sie die **Zuordnungen** Registerkarte
5. Klicken Sie auf die **einfügen** Schaltfläche (siehe Abbildung 4)
6. Fügen Sie den Pfad zu der Aspnet\_isapi.dll in das Feld "ausführbare" (Sie können diesen Pfad aus der Skriptzuordnung für ASPX-Dateien kopieren)
7. Deaktivieren Sie das Kontrollkästchen mit der Bezeichnung **Vergewissern Sie sich diese Datei vorhanden ist**
8. Klicken Sie auf die **OK** Schaltfläche

[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Abbildung 4**: erstellen eine Zuordnung mit Platzhalterzeichen Skript mit IIS 6.0 ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Nach der Aktivierung von Platzhalter-Skript-Zuordnungen müssen Sie die Routingtabelle in der Datei "Global.asax" so ändern, dass es sich um eine Stamm-Route enthält. Andernfalls erhalten Sie die Seite "Fehler" in Abbildung 5 Wenn Sie eine Anforderung für die Seite "Root" Ihrer Anwendung vornehmen. Sie können der geänderten Datei "Global.asax" in der Liste 4 verwenden.

[![Das Dialogfeld "Neues Projekt"](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Abbildung 5**: fehlende Route stammfehler ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Auflisten von 4 – "Global.asax" (mit Stamm Route geändert)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Nachdem Sie eine Zuordnung mit Platzhalterzeichen Skript für IIS 7.0 oder IIS 6.0 aktiviert haben, können Sie Anforderungen, die mit der Tabelle mit den standardmäßigen Route arbeiten vornehmen, die wie folgt aussehen:

/

/ Home/Index

/ Produkt/Informationen/3

/ Produkt

## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde zur Erläuterung, wie Sie ASP.NET MVC verwenden können, wenn eine frühere Version von IIS (oder IIS 7.0 im klassischen Modus) verwenden. Wir haben auch über zwei Methoden zum Abrufen von ASP.NET-Routing zum Arbeiten mit älteren Versionen von IIS: Ändern Sie die Standard-Route-Tabelle, oder erstellen Sie eine Zuordnung mit Platzhalterzeichen Skript.

Die erste Option erfordert, dass Sie so ändern Sie die URLs in ASP.NET MVC-Anwendung verwendet. Eine sehr bedeutende Vorteile dieser ersten Option ist, dass Sie keinen Zugriff auf einem Webserver benötigen um die Routentabelle zu ändern. Das bedeutet, dass Sie die erste Option verwenden können, selbst wenn Ihre ASP.NET MVC-Anwendung mit einem Internet gehostet Unternehmen hosten.

Die zweite Option besteht darin eine Zuordnung mit Platzhalterzeichen Skript zu erstellen. Der Vorteil, dass diese zweite Möglichkeit ist, dass Sie nicht benötigen, um die URLs zu ändern. Der Nachteil, dass diese zweite Möglichkeit ist, dass es die Leistung Ihrer ASP.NET MVC-Anwendung auswirken kann.

>[!div class="step-by-step"]
[Nächste](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
