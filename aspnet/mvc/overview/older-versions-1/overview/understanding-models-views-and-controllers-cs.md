---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Grundlegendes zu Modellen, Ansichten und Controllern (c#) | Microsoft-Dokumentation
author: StephenWalther
description: Fragen Sie sich zu Modellen, Ansichten und Controllern? In diesem Tutorial erläutert Stephen Walther die verschiedenen Teile einer ASP.NET MVC-Anwendung.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: c4c5247ac4b880c1be60f0419ebc9fc9b790c639
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823393"
---
<a name="understanding-models-views-and-controllers-c"></a>Grundlegendes zu Modellen, Ansichten und Controllern (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Fragen Sie sich zu Modellen, Ansichten und Controllern? In diesem Tutorial erläutert Stephen Walther die verschiedenen Teile einer ASP.NET MVC-Anwendung.


Dieses Tutorial bietet eine allgemeine Übersicht über ASP.NET MVC-Modelle, Ansichten und Controllern. Das heißt, es wird erläutert, das M ", V", und C "in ASP.NET MVC.

Nach der Lektüre dieses Tutorial, sollten Sie verstehen, wie die verschiedenen Teile einer ASP.NET MVC-Anwendung zusammenarbeiten. Sie sollten auch wissen, wie die Architektur einer ASP.NET MVC-Anwendung von einer ASP.NET Web Forms-Anwendung oder eine Active Server Pages-Anwendung unterscheidet.

## <a name="the-sample-aspnet-mvc-application"></a>Die ASP.NET MVC-Beispielanwendung

Die Standard-Visual Studio-Vorlage zum Erstellen von ASP.NET MVC-Webanwendungen enthält eine sehr einfache beispielanwendung, die verwendet werden kann, um die verschiedenen Teile einer ASP.NET MVC-Anwendung zu verstehen. Wir nutzen diese einfache Anwendung in diesem Tutorial.

Erstellen Sie eine neue ASP.NET MVC-Anwendung mit der MVC-Vorlage durch Starten von Visual Studio 2008 und Auswahl der Menüoption im Datei neu Projekt (siehe Abbildung 1). Klicken Sie im Dialogfeld "Neues Projekt" Wählen Sie Ihre bevorzugte Programmiersprache unter Projekttypen (Visual Basic oder c#), und wählen Sie **ASP.NET MVC-Webanwendung** unter Vorlagen. Klicken Sie auf die Schaltfläche "OK".


[![Dialogfeld "Neues Projekt"](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Abbildung 01**: Dialogfeld "Neues Projekt" ([klicken Sie, um das Bild in voller Größe anzeigen](understanding-models-views-and-controllers-cs/_static/image2.png))


Bei der Erstellung einer ASP.NET MVC-Anwendung, die **Komponententestprojekt erstellen** angezeigt (siehe Abbildung 2). In diesem Dialogfeld können Sie ein separates Projekt in der Projektmappe für das Testen Ihrer ASP.NET MVC-Anwendung zu erstellen. Wählen Sie die Option **Nein, kein Komponententestprojekt erstellen** , und klicken Sie auf die **OK** Schaltfläche.


[![Unit Test-Dialogfeld "erstellen"](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Abbildung 02**: Einheit Testdialogfeld erstellen ([klicken Sie, um das Bild in voller Größe anzeigen](understanding-models-views-and-controllers-cs/_static/image4.png))


Nach der neuen ASP.NET MVC wird die Anwendung erstellt. Mehrere Ordner und Dateien im Projektmappen-Explorer-Fenster wird angezeigt. Insbesondere sehen Sie drei Ordner namens Modelle, Ansichten und Controllern. Wie Sie aus dem Ordnernamen sich denken können, enthalten diese Ordner die Dateien für die Implementierung von Modellen, Ansichten und Controllern aus.

Wenn Sie den Ordner "Controllers" erweitern, sehen Sie eine Datei namens AccountController.cs und eine Datei namens "HomeController.cs". Wenn Sie den Ordner "Views" erweitern, sehen Sie drei Unterordner namens "Konto", "Home" und "freigegeben. Wenn Sie den Ordner "Home" erweitern, sehen Sie zwei zusätzliche Dateien, die mit dem Namen About.aspx und Index.aspx (siehe Abbildung 3). Diese Dateien bilden zusammen die beispielanwendung, die mit der standardmäßigen ASP.NET MVC-Vorlage enthalten.


[![Das Fenster des Projektmappen-Explorer](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Abbildung 03**: das Fenster des Projektmappen-Explorer ([klicken Sie, um das Bild in voller Größe anzeigen](understanding-models-views-and-controllers-cs/_static/image6.png))


Sie können die beispielanwendung ausführen, indem Sie durch Auswählen der Menüoption **Debuggen, Debugging starten**. Alternativ können Sie die F5-Taste drücken.

Beim ersten eine ASP.NET-Anwendung ausführen wird das Dialogfeld in Abbildung 4, die empfohlen, dass Sie den Debugmodus aktivieren angezeigt. Klicken Sie auf die Schaltfläche "OK", und die Anwendung ausgeführt wird.


[![Debuggen von Dialogfeld nicht aktiviert](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Abbildung 04**: Dialogfeld Debuggen nicht aktiviert ([klicken Sie, um das Bild in voller Größe anzeigen](understanding-models-views-and-controllers-cs/_static/image8.png))


Wenn Sie eine ASP.NET MVC-Anwendung ausführen, startet Visual Studio die Anwendung in Ihrem Webbrowser. Die beispielanwendung umfasst nur zwei Seiten: die Seite "Index" und die Seite "Info". Beim ersten Start die Anwendung wird auf der Indexseite angezeigt, die (siehe Abbildung 5). Sie können auf der Seite "Info" navigieren, auf den Link im Menü oben rechts auf der die Anwendung.


[![Die Indexseite](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Abbildung 05**: die Indexseite ([klicken Sie, um das Bild in voller Größe anzeigen](understanding-models-views-and-controllers-cs/_static/image11.png))


Beachten Sie die URLs in die Adressleiste Ihres Browsers ein. Wenn Sie den Info-Menü-Link klicken, die URL in die Adressleiste des Browsers ändert sich beispielsweise um **/Home/About**.

Wenn Sie das Browserfenster schließen und zurück zu Visual Studio, nicht Sie können eine Datei zu suchen, wobei der Pfad-Start- > über. Die Dateien nicht vorhanden sind. Wie ist dies möglich?

## <a name="a-url-does-not-equal-a-page"></a>Eine URL entspricht keine Seite

Wenn Sie eine herkömmliche ASP.NET Web Forms-Anwendung oder eine Active Server Pages-Anwendung erstellen, besteht eine 1: 1-Entsprechung zwischen eine URL und eine Seite. Wenn Sie eine Seite namens SomePage.aspx vom Server angefordert, dann wäre gibt es besser eine Seite auf dem Datenträger, die mit dem Namen SomePage.aspx. Wenn die SomePage.aspx-Datei nicht vorhanden ist, erhalten Sie eine unschöne **404 - Seite wurde nicht gefunden.** Fehler.

Beim Erstellen einer ASP.NET MVC-Anwendung im Gegensatz dazu besteht keine Übereinstimmung zwischen der URL, die Sie in der Adressleiste des Browsers eingeben und die Dateien, die Sie in Ihrer Anwendung finden. In einer ASP.NET MVC-Anwendung entspricht eine Controlleraktion anstelle einer Seite, auf dem Datenträger eine URL.

In einer herkömmlichen ASP.NET oder ASP-Anwendung werden die Browseranforderungen für Seiten zugeordnet. In einer ASP.NET MVC-Anwendung werden im Gegensatz dazu Browseranforderungen zu Controlleraktionen zugeordnet. Eine ASP.NET Web Forms-Anwendung ist Content-orientierte. Eine ASP.NET MVC-Anwendung ist im Gegensatz dazu zentrierte Anwendungslogik.

## <a name="understanding-aspnet-routing"></a>Grundlegendes zu ASP.NET-Routing

Eine Browseranforderung zugeordnet wird, um eine Controlleraktion über ein Feature von ASP.NET-Framework namens *ASP.NET-Routing*. ASP.NET-Routing wird von der ASP.NET MVC-Framework verwendet *Route* eingehende Anforderungen zu Controlleraktionen.

ASP.NET-Routing verwendet eine Routentabelle zum Verarbeiten eingehender Anforderungen. Diese Routentabelle wird erstellt, beim ersten Starten von Ihrer Webanwendung. Die Routentabelle ist die Einrichtung in der Datei "Global.asax". Die standardmäßige MVC Global.asax-Datei, die in Codebeispiel 1 enthalten ist.

**Codebeispiel 1 – "Global.asax"**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Wenn eine ASP.NET-Anwendung zuerst gestartet wird, die Anwendung\_Start()-Methode wird aufgerufen. Klicken Sie in Codebeispiel 1 diese Methode ruft die RegisterRoutes()-Methode, und die RegisterRoutes()-Methode erstellt die Standardroutingtabelle.

Die Standardroutingtabelle besteht eine Route aus. Diese Standardroute unterbricht alle eingehende Anforderungen in drei Segmente (ein URL-Segment ist alles zwischen Schrägstriche). Das erste Segment eines Controllernamens zugeordnet ist, das zweite Segment einen Aktionsnamen zugeordnet ist und das letzte Segment einen an die Aktion, die mit dem Namen Id. übergebenen Parameter zugeordnet ist

Nehmen wir beispielsweise die folgende URL:

/ Produkt/Details/3

Diese URL wird in drei Parameter wie folgt analysiert:

Controller = Produkt

Aktion = Details

ID = 3

Die Standardroute, die in der Datei "Global.asax" definiert enthält Standardwerte für alle drei Parameter. Der Standardwert Controller Startseite ist, der Standard-Aktion ist Index und der Standard-Id ist eine leere Zeichenfolge. Beachten Sie mit diesen Standardwerten Denken Sie daran, wie die folgende URL analysiert wird:

/ Mitarbeiter

Diese URL wird in drei Parameter wie folgt analysiert:

Controller = Mitarbeiter

Aktion = Index

ID =

Schließlich, wenn Sie eine ASP.NET MVC-Anwendung öffnen, ohne Angabe einer URLs (z. B. `http://localhost`) wird die URL wie folgt analysiert:

Controller = Home

Aktion = Index

ID =

Die Anforderung wird an die Index()-Aktion für die HomeController-Klasse weitergeleitet.

## <a name="understanding-controllers"></a>Grundlegendes zu Controllern

Ein Controller ist verantwortlich für die Möglichkeit, die Interaktion eines Benutzers mit einer MVC-Anwendung steuern. Ein Controller enthält die datenflusskontrolllogik für eine ASP.NET MVC-Anwendung. Ein Controller bestimmt, welche Antwort, die mit einem Benutzer zu senden, wenn ein Benutzer einen Browser anfordert.

Ein Controller ist nur eine Klasse (z. B. eine Visual Basic oder c#-Klasse). Die ASP.NET MVC-beispielanwendung enthält einen Controller namens "HomeController.cs" im Ordner "Controller". Der Inhalt der Datei "HomeController.cs" wird im Codebeispiel 2 reproduziert.

**Codebeispiel 2 - Datei "HomeController.cs"**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Beachten Sie, dass die HomeController zwei Methoden namens Index() und About(). Diese beiden Methoden entsprechen die beiden Aktionen, die von der Controller verfügbar gemacht werden. Die URL/Home/Index Ruft die HomeController.Index()-Methode auf und ruft die URL/Home/About die HomeController.About()-Methode.

Jede öffentliche Methode in einem Controller wird als eine Controlleraktion verfügbar gemacht. Sie müssen dies vorsichtig sein. Dies bedeutet, dass jede öffentliche Methode, die in einem Controller enthalten durch Eingeben der richtigen URL in einem Browser von jedem Benutzer mit Zugriff auf das Internet aufgerufen werden kann.

## <a name="understanding-views"></a>Grundlegendes zu Sichten

Die beiden Controller-Aktionen, die verfügbar gemacht werden, indem Sie die HomeController-Klasse, Index() und About(), sowohl eine Ansicht zurückgeben. Eine Ansicht enthält die HTML-Markup und den Inhalt, der an den Browser gesendet wird. Eine Sicht ist die Entsprechung einer Seite an, bei der Arbeit mit einer ASP.NET MVC-Anwendung.

Sie müssen Ihre Ansichten am richtigen Speicherort erstellen. Die HomeController.Index()-Aktion gibt eine Ansicht befindet sich unter folgendem Pfad:

\Views\Home\Index.aspx

Die HomeController.About()-Aktion gibt eine Ansicht befindet sich unter folgendem Pfad:

\Views\Home\About.aspx

Im Allgemeinen, wenn Sie eine Ansicht für eine Controlleraktion zurückgeben möchten, müssen Sie einen Unterordner im Ordner "Views" mit dem gleichen Namen wie Ihrem Controller zu erstellen. In den Unterordner müssen Sie eine ASPX-Datei mit dem gleichen Namen wie die Controlleraktion erstellen.

Die Datei in Programmausdruck 3 enthält die About.aspx-Ansicht.

**Codebeispiel 3 - About.aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Wenn Sie die erste Zeile in Programmausdruck 3 ignorieren, die meisten den Rest der Ansicht besteht aus standard-HTML. Sie können den Inhalt der Ansicht ändern, indem Sie die HTML-Code, die sollen hier eingeben.

Eine Ansicht ist sehr ähnlich ist, auf eine Seite in Active Server Pages "oder" ASP.NET Web Forms. Eine Sicht kann es sich um HTML-Inhalte und Skripts enthalten. Sie können die Skripts in Ihre bevorzugte .NET schreiben Programmiersprache (z. B. c# oder Visual Basic .NET). Sie können Skripts verwenden, um dynamischen Inhalt wie z. B. Daten anzuzeigen.

## <a name="understanding-models"></a>Grundlegendes zu Modellen

Wir haben Controller und haben wir Ansichten diskutiert. Das letzte Thema, das wir besprechen müssen, ist Modelle. Was ist eine MVC-Modell?

Ein MVC-Modell enthält alle Ihre Anwendungslogik, die nicht in einer Ansicht oder einen Controller enthalten ist. Das Modell sollten alle Ihre Geschäftslogik der Anwendung, Validierungslogik und Datenbankzugriffslogik enthalten. Z. B. Wenn Sie Ihre Datenbank den Zugriff auf das Microsoft Entity Framework verwenden, würde dann Sie die Entity Framework-Klassen (EDMX-Datei) im Ordner "Models" erstellen.

Eine Sicht sollte nur Logik, die für das Generieren der Benutzeroberfläche enthalten. Ein Controller darf nur das absolute Minimum Logik erforderlich, um die richtige Ansicht zurückzugeben, oder leiten Sie den Benutzer zu einer anderen Aktion (Steuerung) enthalten. Alles sollte im Modell enthalten sein.

Im Allgemeinen sollten Sie sich für fat-Modelle und skinny Controller bemühen. Die Controllermethoden sollten nur ein paar Zeilen Code enthalten. Wenn eine Controlleraktion zu fat abruft, dann sollten Sie die Logik um eine neue Klasse im Ordner "Models" zu verschieben.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial Ihnen zur Verfügung gestellt eines allgemeinen Überblick über die verschiedenen Teile einer ASP.NET MVC-Webanwendung. Sie erfahren, wie ASP.NET-Routing eingehenden Browseranforderungen bestimmten Controlleraktionen zugeordnet. Sie erfahren, wie der Controller orchestriert wie Ansichten an den Browser zurückgegeben werden. Schließlich haben Sie gelernt, wie Modelle Anwendung Business, Validierung und Datenbankzugriffslogik enthalten.
