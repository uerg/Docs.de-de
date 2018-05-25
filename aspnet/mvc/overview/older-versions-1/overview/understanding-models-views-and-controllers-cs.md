---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Grundlegendes zu Modellen, Ansichten und Controllern (c#) | Microsoft Docs
author: StephenWalther
description: Informationen zu Modellen, Ansichten und Controllern verwechselt? In diesem Lernprogramm führt Sie Stephen Walther auf verschiedene Teile einer ASP.NET MVC-Anwendung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c9a0cbbf6f786944d7892fbb14859939f21bdd5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="understanding-models-views-and-controllers-c"></a>Grundlegendes zu Modellen, Ansichten und Controllern (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Informationen zu Modellen, Ansichten und Controllern verwechselt? In diesem Lernprogramm führt Sie Stephen Walther auf verschiedene Teile einer ASP.NET MVC-Anwendung.


Dieses Lernprogramm enthält eine allgemeine Übersicht über ASP.NET MVC-Modelle, Ansichten und Controllern. Das heißt, es wird erläutert, das M ", V", und C "in ASP.NET MVC.

Nach dem Lesen dieses Lernprogramms, sollten Sie verstehen, wie die verschiedenen Teile einer ASP.NET MVC-Anwendung zusammenarbeiten. Sie sollten auch wissen, wie die Architektur einer ASP.NET MVC-Anwendung von einer ASP.NET Web Forms-Anwendung oder der Active Server Pages-Anwendung unterscheidet.

## <a name="the-sample-aspnet-mvc-application"></a>Das Beispiel ASP.NET MVC-Anwendung

Der standardmäßige Visual Studio-Vorlage zum Erstellen von ASP.NET MVC-Webanwendungen enthält eine sehr einfache beispielanwendung, die verwendet werden kann, um die verschiedenen Teile einer ASP.NET MVC-Anwendung zu verstehen. Wir nutzen diese einfache Anwendung in diesem Lernprogramm.

Sie erstellen eine neue ASP.NET MVC-Anwendung mit der MVC-Vorlage durch Starten von Visual Studio 2008 und dabei die Option im Menü Datei, neu (siehe Abbildung 1). Klicken Sie im Dialogfeld "Neues Projekt", wählen Sie Ihre bevorzugte Programmiersprache unter Projekttypen (Visual Basic oder c#), und wählen Sie **ASP.NET MVC-Webanwendung** unter Vorlagen. Klicken Sie auf die Schaltfläche "OK".


[![Dialogfeld "Neues Projekt"](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Abbildung 01**: Dialogfeld "Neues Projekt" ([klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-models-views-and-controllers-cs/_static/image2.png))


Bei der Erstellung einer neuen ASP.NET MVC-Anwendung, die **Komponententestprojekt erstellen** Dialogfeld wird angezeigt (siehe Abbildung 2). In diesem Dialogfeld können Sie ein separates Projekt in der Projektmappe für das Testen Ihrer ASP.NET MVC-Anwendung zu erstellen. Wählen Sie die Option **Nein, kein Komponententestprojekt erstellen** , und klicken Sie auf die **OK** Schaltfläche.


[![Unit Test-Dialogfeld "erstellen"](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Abbildung 02**: Einheit Testdialogfeld erstellen ([klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-models-views-and-controllers-cs/_static/image4.png))


Nach dem neuen ASP.NET MVC ist die Anwendung erstellt. Mehrere Ordner und Dateien im Projektmappen-Explorer-Fenster wird angezeigt. Insbesondere, sehen Sie drei Ordner Modelle, Ansichten und Controllern. Wie Sie aus dem Ordnernamen erraten können, enthalten diese Ordner der Dateien für die Implementierung der Modelle, Ansichten und Controllern aus.

Wenn Sie den Ordner Controllers erweitern, sehen Sie eine Datei namens AccountController.cs und eine Datei namens HomeController.cs. Wenn Sie den Ordner Views erweitern, sehen Sie drei Unterordner mit dem Namen Konto Home und freigegeben. Wenn Sie den Ordner Home erweitern, sehen Sie zwei zusätzliche Dateien, die mit dem Namen About.aspx und Index.aspx (siehe Abbildung 3). Diese Dateien bilden zusammen die beispielanwendung, die mit der standardmäßigen ASP.NET MVC-Vorlage enthalten.


[![Das Fenster des Projektmappen-Explorer](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Abbildung 03**: das Fenster des Projektmappen-Explorer ([klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-models-views-and-controllers-cs/_static/image6.png))


Sie können die beispielanwendung ausführen, indem Sie im Menü die Option **Debuggen, Debugging starten**. Alternativ können Sie die F5-Taste drücken.

Beim Ausführen von einer ASP.NET-Anwendung wird das Dialogfeld in Abbildung 4, die empfohlen, den Debugmodus zu aktivieren. Klicken Sie auf die Schaltfläche "OK", und die Anwendung ausgeführt wird.


[![Debuggen von Dialogfeld nicht aktiviert](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Abbildung 04**: Dialogfeld Debuggen nicht aktiviert ([klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-models-views-and-controllers-cs/_static/image8.png))


Wenn Sie eine ASP.NET MVC-Anwendung ausführen, startet Visual Studio die Anwendung in Ihrem Webbrowser an. Die beispielanwendung umfasst nur zwei Seiten: die Indexseite und der Seite "Info". Beim ersten der Anwendung starten wird die Indexseite (siehe Abbildung 5) angezeigt. Sie können auf der Seite "Info" navigieren, indem Sie auf den Link im Menü am oberen rechts von der Anwendung.


[![Die Seite "Index"](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Abbildung 05**: die Indexseite ([klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-models-views-and-controllers-cs/_static/image11.png))


Beachten Sie die URLs in der Adressleiste des Browsers. Ändert z. B. Wenn Sie den Link der Info-Menü klicken, die URL in die Adressleiste des Browsers auf **/Heim- oder bald**.

Wenn Sie das Browserfenster zu schließen, und kehren Sie zu Visual Studio zurück, wird nicht Sie einer Datei zu suchen, mit der Path-Home/ungefähr sein. Die Dateien nicht vorhanden sind. Wie dies möglich ist?

## <a name="a-url-does-not-equal-a-page"></a>Eine URL stimmt nicht mit eine Seite

Wenn Sie eine herkömmliche ASP.NET Web Forms-Anwendung oder eine Active Server Pages-Anwendung erstellen, besteht eine 1: 1-Entsprechung zwischen einer URL und eine Seite. Wenn Sie eine Seite mit dem Namen SomePage.aspx vom Server angefordert, dann wäre gibt es besser eine Seite auf dem Datenträger, die mit dem Namen SomePage.aspx. Wenn die SomePage.aspx-Datei nicht vorhanden ist, erhalten Sie eine hässlich **404 - Seite Nichtgefunden** Fehler.

Beim Erstellen einer ASP.NET MVC-Anwendung im Gegensatz dazu besteht keine Übereinstimmung zwischen der URL, die Sie in der Adressleiste des Browsers eingeben und die Dateien, die Sie in Ihrer Anwendung finden. In einer ASP.NET MVC-Anwendung entspricht eine URL eine Controlleraktion anstelle einer Seite auf dem Datenträger.

Browseranforderungen werden Seiten in einer herkömmlichen ASP.NET oder ASP-Anwendung zugeordnet. In einer ASP.NET MVC-Anwendung werden im Gegensatz dazu Browseranforderungen Controlleraktionen zugeordnet. Eine ASP.NET Web Forms-Anwendung ist. Eine ASP.NET MVC-Anwendung wird im Gegensatz dazu zentriertes Anwendungslogik.

## <a name="understanding-aspnet-routing"></a>Grundlegendes zu ASP.NET-Routing

Eine Browseranforderung zugeordnet wird, um eine Controlleraktion über eine Funktion von ASP.NET-Framework aufgerufen, *ASP.NET-Routing*. ASP.NET Routing wird von ASP.NET MVC-Framework verwendet *Route* eingehende Anforderungen an Controlleraktionen.

ASP.NET Routing verwendet eine Routentabelle zum Verarbeiten eingehender Anforderungen. Diese Routentabelle wird erstellt, beim ersten Start von Ihrer Webanwendung. Die Routingtabelle wird Setup in der Datei "Global.asax". Die standardmäßige MVC Global.asax-Datei ist im Codebeispiel 1 enthalten.

**Auflisten von 1 – "Global.asax"**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Wenn eine ASP.NET-Anwendung zuerst gestartet wird, die Anwendung\_Start()-Methode aufgerufen wird. Auflisten von 1 diese Methode ruft die RegisterRoutes()-Methode und erstellt die Tabelle mit den standardmäßigen Route die RegisterRoutes()-Methode.

Die Standard-Routingtabelle eine Route besteht aus. Dieser Standardroute unterbricht alle eingehende Anforderungen in drei Segmente (ein URL-Segment ist alles zwischen Schrägstriche). Das erste Segment Controllernamen zugeordnet ist, das zweite Segment einen Aktionsnamen zugeordnet ist und das abschließende Segment einen Parameter übergeben, um die Aktion namens-ID zugeordnet ist

Betrachten Sie beispielsweise die folgende URL:

/ Produkt/Informationen/3

Diese URL wird in drei Parameter wie folgt analysiert:

Controller = Produkt

Aktion = Details

ID = 3

In der Datei "Global.asax" definiert die Standard-Route enthält Standardwerte für alle drei Parameter. Die Standardeinstellung Controller Startseite ist der Standard-Aktion ist Index und die Standard-Id ist eine leere Zeichenfolge. Beachten Sie mit diesen Standardwerten Denken Sie daran, wie die folgende URL analysiert werden:

/ Mitarbeiter

Diese URL wird in drei Parameter wie folgt analysiert:

Controller = Mitarbeiter

Aktion = Index

ID =

Schließlich, wenn Sie eine ASP.NET MVC-Anwendung öffnen, ohne Angabe einer URLs (z. B. `http://localhost`) wird die URL wie folgt analysiert:

Controller = Home

Aktion = Index

ID =

Die Anforderung wird an die Aktion Index() HomeController-Klasse weitergeleitet.

## <a name="understanding-controllers"></a>Grundlegendes zu Controllern

Ein Domänencontroller ist zum Steuern der Möglichkeit, die Interaktion eines Benutzers mit einer MVC-Anwendung verantwortlich. Ein Controller enthält die datenflusskontrolllogik für eine ASP.NET MVC-Anwendung. Ein Controller bestimmt, welche Antwort auf die mit einem Benutzer zu senden, wenn ein Benutzer eine Browseranforderung herstellt.

Ein Controller ist nur eine Klasse (z. B. eine Visual Basic- oder C#-Klasse). Das Beispiel ASP.NET MVC-Anwendung enthält einen Controller mit dem Namen im Ordner "Controller" HomeController.cs. Der Inhalt der Datei "HomeController.cs" ist im Codebeispiel 2 reproduziert.

**Auflisten von 2 – HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Beachten Sie, dass die HomeController zwei Methoden namens Index() und About() verfügt. Diese beiden Methoden entsprechen, die zwei Aktionen, die vom Controller verfügbar gemacht. Der URL/Home/Index Ruft die HomeController.Index()-Methode und URL/Home/zu Ruft die HomeController.About()-Methode.

Jede öffentliche Methode in einem Controller wird als eine Controlleraktion verfügbar gemacht. Sie müssen dies vorsichtig sein. Dies bedeutet, dass keine öffentliche Methode, die in einem Controller enthalten von jedem Benutzer mit Zugriff auf das Internet aufgerufen werden kann, durch die richtige URL in einem Browser eingeben.

## <a name="understanding-views"></a>Grundlegendes zu Sichten

Die zwei Controlleraktionen von HomeController-Klasse, Index() und About() verfügbar gemacht werden, sowohl eine Ansicht zurückgeben. Eine Ansicht enthält den HTML-Markup und den Inhalt, die an den Browser gesendet wird. Eine Sicht ist das Äquivalent einer Seite, bei der Arbeit mit einer ASP.NET MVC-Anwendung.

Sie müssen Ihre Ansichten in den richtigen Speicherort erstellen. Die HomeController.Index()-Aktion gibt eine Ansicht finden Sie unter folgendem Pfad:

\Views\Home\Index.aspx

Die HomeController.About()-Aktion gibt eine Ansicht finden Sie unter folgendem Pfad:

\Views\Home\About.aspx

Im Allgemeinen, wenn Sie eine Ansicht für eine Controlleraktion zurückgeben möchten, müssen Sie einen Unterordner im Ordner "Ansichten" mit dem gleichen Namen wie Ihre Controller zu erstellen. In den Unterordner müssen Sie eine ASPX-Datei mit dem gleichen Namen wie der Controlleraktion erstellen.

Die Datei im Codebeispiel 3 enthält die About.aspx-Sicht.

**3 – About.aspx auflisten**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Wenn Sie die erste Zeile im Codebeispiel 3 ignorieren, den Rest der Sicht besteht aus der standard-HTML. Sie können den Inhalt der Ansicht ändern, indem Sie die HTML-Elemente, die sollen hier eingeben.

Eine Sicht ist vergleichbar mit einer Seite in Active Server Pages oder ASP.NET Web Forms. Eine Ansicht kann HTML-Inhalt und Skripts enthalten. Sie können die Skripts in Ihre bevorzugte .NET schreiben Programmiersprache (z. B. c# oder Visual Basic .NET). Verwenden Sie Skripts, um dynamische Inhalte wie Datenbankdaten anzuzeigen.

## <a name="understanding-models"></a>Grundlegendes zu Modellen

Wir haben Controller, und wir haben Ansichten diskutiert. Im letzte Thema, das wir benötigen, um zu besprechen ist Modelle. Was ist ein MVC-Modell?

Ein MVC-Modell enthält alle Ihrer Anwendungslogik, die nicht in einer Ansicht oder einen Controller enthalten ist. Das Modell sollten alle Ihre Geschäftslogik der Anwendung, die Validierungslogik und die Zugriffslogik der Datenbank enthalten. Z. B. Wenn Sie Microsoft Entity Framework Zugriff auf die Datenbank verwenden, würde dann Sie die Entity Framework-Klassen (EDMX-Datei) im Ordner "Modelle" erstellen.

Eine Sicht sollte nur im Zusammenhang mit die Benutzeroberfläche generiert Logik enthalten. Ein Controller darf nur die absolute Mindestanforderungen von der Logik zum Zurückgeben der rechten Ansicht oder leiten Sie den Benutzer an eine andere Aktion (Steuerung) enthalten. Alle anderen sollte im Modell enthalten sein.

Im Allgemeinen sollten Sie fat Modelle und skinny Controller anzustreben. Die Controllermethoden dürfen nur wenige Codezeilen. Wenn eine Controlleraktion zu fat abruft, dann sollten Sie die Logik um eine neue Klasse im Ordner "Modelle" verschieben.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm Ihnen zur Verfügung gestellt einer grobe Übersicht der verschiedenen Teile von einer ASP.NET MVC-Webanwendung. Sie haben gelernt, wie ASP.NET-Routing eingehende Browseranforderungen bestimmten Controlleraktionen zugeordnet. Sie haben gelernt, wie ein Controllern orchestrieren wie Ansichten an den Browser zurückgegeben werden. Schließlich haben Sie gelernt, wie Modelle Anwendung Business, Überprüfung und Datenbank-Zugriffslogik enthalten.
