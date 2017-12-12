---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Erste Schritte mit ASP.NET 4.5 WebForms und Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Diese schrittweise Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ee3e244c4ed29384d11c7acc1440692d3f9b23e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Erste Schritte mit ASP.NET 4.5 WebForms und Visual Studio 2013
====================
Durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese schrittweise Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für das Web. [ASP.NET Web Forms-Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Testen Sie Ihr Wissen und zu verstärken Sie Schlüsselkonzepte ergreifen Sie hierzu das Quiz zu ASP.NET Web Forms. Dieses Quiz wurde speziell von Inhalt in diesem Lernprogramm Reihe. Jede Frage im Quiz erläutert sowie Links zu weiteren Anleitungen.


## <a name="introduction"></a>Einführung

Diese Reihe von Lernprogrammen führt Sie durch die Schritte zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von Visual Studio Express 2013 für Web und ASP.NET 4.5.

Die Anwendung, die Sie erstellen, wird mit dem Namen **Wingtip Toys**. Es ist ein vereinfachtes Beispiel einer Front-Store-Website, die Elemente online verkauft. Diese Reihe von Lernprogrammen werden die neue Funktionen von ASP.NET 4.5 hervorgehoben.

Kommentare sind Willkommen, und stellen wir bestrebt, die diese Reihe von Lernprogrammen basierend auf Ihren Vorschlägen zu aktualisieren.

### <a name="download-completed-project"></a>Download abgeschlossen-Projekt

Sie können ein C#-Projekt herunterladen, die das vollständige Lernprogramm enthält.

- [Erste Schritte mit ASP.NET 4.5 WebForms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Überprüfen Sie den Inhalt, indem die zugehörige ASP.NET Web Forms-Quiz

Nachdem Sie dieses Lernprogramm abgeschlossen haben, testen Sie Ihre Kenntnisse und Schlüsselkonzepte vertiefen, ergreifen Sie hierzu die [ASP.NET Web Forms-Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Dieses Quiz wurde speziell von Inhalt in diesem Lernprogramm Reihe. Jede Frage im Quiz erläutert sowie Links zu weiteren Anleitungen.

- [ASP.NET Web Forms-Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Zielgruppe

Die Zielgruppe dieses Reihe von Lernprogrammen wird erfahrenen Entwickler, die ASP.NET Web Forms nicht vertraut sind. Ein Entwickler, die diese Reihe von Lernprogrammen interessiert, sollten die folgenden Kenntnisse verfügen:

- Vertraute mit einem Objekt dienstorientierten Programmiersprache (OOP)
- Mit Web Development-Konzepten (HTML, CSS, JavaScript)
- Mit relationalen Datenbanken vertraut sind.
- Mit den Konzepten der n-Tier-Architektur

Wenn Sie interessiert, überprüfen die oben aufgeführten Bereiche sind, erwägen Sie, überprüfen den folgenden Inhalt:

- [Erste Schritte mit Visual c#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Webentwicklung](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP und JQuery](http://w3schools.com/)
- [Relationale Datenbank](http://en.wikipedia.org/wiki/Relational_database)
- [Multi-Tier-Architektur](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Anwendungsfunktionen

Der ASP.NET Web Form-Funktionen, die in diese Reihe dargestellt:

- Das Webanwendungsprojekt (keine Website-Projekt)
- Web Forms
- Masterseiten, Konfiguration
- Bootstrap
- Entity Framework Code zuerst LocalDB
- Anforderungsüberprüfung
- Datensteuerelementen, stark typisierten Modell binden, Datenanmerkungen, und der Wert von Anbietern
- SSL- und OAuth
- ASP.NET Identity, Konfiguration und Autorisierung
- Unaufdringlichen Überprüfung
- Routing
- ASP.NET-Fehlerbehandlung

### <a name="application-scenarios-and-tasks"></a>Anwendungsszenarien und Aufgaben

U. a. folgende Aufgaben in dieser Reihe veranschaulicht:

- Erstellen, überprüfen und Ausführen des neuen Projekts
- Erstellen die Struktur der Datenbank
- Initialisieren und das seeding der Datenbank
- Anpassen der Benutzeroberfläche, die mit Formatvorlagen, Grafiken und einer Masterseite
- Hinzufügen von Seiten und navigation
- Zum Anzeigen von Details im Menü und Produktdaten
- Erstellen den Einkaufswagen legen
- Hinzufügen von SSL und OAuth-Unterstützung
- Eine Zahlungsmethode hinzufügen
- Z. B. eine Rolle "Administrator" und ein Benutzer auf die Anwendung
- Einschränken des Zugriffs auf bestimmte Seiten und Ordner
- Hochladen einer Datei an die Web-Anwendung
- Implementieren die Validierung von Benutzereingaben
- Registrieren von Routen für die Webanwendung
- Implementieren von Fehlerbehandlungs- und Protokollierung von Anzeigefehlern

## <a name="overview"></a>Übersicht

Wenn Sie mit ASP.NET Web Forms vertraut sind, weisen aber Kenntnisse Programmierungskonzepte, müssen Sie das richtige Lernprogramm. Wenn Sie bereits mit ASP.NET Web Forms vertraut sind, profitieren Sie von dieser Reihe von Lernprogrammen durch die neuen Funktionen in ASP.NET 4.5 verfügbar. Wenn Sie mit Konzepten und ASP.NET Web Forms-Programmierung noch nicht vertraut sind, finden Sie unter der Weitere Lernprogramme zur Verfügung gestellt, in der Web Forms [Einstieg](../../../index.md) Abschnitt auf der ASP.NET-Website.

Die spezifische **neueste** ASP.NET 4.5-Funktionen bereitgestellt in dieser Web Forms Reihe von Lernprogrammen Folgendes:

- Eine einfache Benutzeroberfläche zum Erstellen von Projekten des betreffenden Angebots [für mehrere Frameworks, die ASP.NET unterstützen](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC und Web-API).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), ein Layout und Designumgebung Framework, das reaktionsfähige Design und Designumgebung Funktionen bereitstellt.
- [ASP.NET Identity](../../../../identity/index.md), eine neue ASP.NET-Mitgliedschaftssystem, die in allen ASP.NET Frameworks und funktioniert mit Webhosting-Software als IIS arbeitet.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), ein Update für das Entity Framework dadurch abrufen und Bearbeiten von Daten als stark typisierte Objekte, den Zugriff auf Daten asynchron verarbeiten vorübergehende Verbindungsfehler auslösen, und SQL-Anweisungen zu protokollieren.

Eine vollständige Liste der Features von ASP.NET 4.5 finden Sie unter [ASP.NET und Webtools für Visual Studio 2013-Versionshinweise](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Die Beispielanwendung des Wingtip Toys

Die folgenden Screenshots bieten einen schnellen Überblick über die ASP.NET Web Forms-Anwendung, die Sie in diesem Lernprogramm Reihe erstellen. Wenn Sie die Anwendung aus Visual Studio Express 2013 für Web ausführen, sehen Sie die folgenden Web-Startseite.

![Wingtip Toys - Standardseite](introduction-and-overview/_static/image1.png)

Sie können als neuer Benutzer registrieren, oder melden Sie sich als einen vorhandenen Benutzer. Navigation wird am oberen für jede Produktkategorie durch Abrufen der verfügbaren Produkte aus der Datenbank bereitgestellt.

Wenn Sie den Link Produkte auswählen, werden Sie eine Liste aller verfügbaren Produkte angezeigt.

![Wingtip Toys - Produkte](introduction-and-overview/_static/image2.png)

Einzelne Produktdetails sehen dazu einen der aufgeführten Produkte.

![Wingtip Toys - Produktdetails](introduction-and-overview/_static/image3.png)

Als ein Benutzer können Sie registrieren und melden Sie sich mit den Standardfunktionen der Web Forms-Vorlage. In diesem Lernprogramm wird beschrieben, wie mit einem vorhandenen Gmail-Konto anmelden. Darüber hinaus können Sie sich als Administrator hinzufügen und Entfernen von Produkten aus der Datenbank anmelden.

![Wingtip Toys - Anmeldung](introduction-and-overview/_static/image4.png)

Nachdem Sie sich als Benutzer angemeldet haben, können Sie mit dem Einkaufswagen und Auschecken von PayPal Produkte hinzufügen. Beachten Sie, dass diese beispielanwendung mit PayPal Entwickler Sandkasten geeignet ist. Keine tatsächlichen Money-Transaktion wird ausgeführt.

![Wingtip Toys - Einkaufswagen](introduction-and-overview/_static/image5.png)

PayPal wird Ihr Konto verhindern, Reihenfolge und Zahlungsinformationen bestätigt.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Nach der Rückkehr von PayPal, können Sie überprüfen und Ihre Bestellung abzuschließen.

![Wingtip Toys - Reihenfolge überprüfen](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass Sie die folgende Software auf Ihrem Computer installiert haben:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web). .NET Framework wird automatisch installiert.

Diese Reihe von Lernprogrammen verwendet Microsoft Visual Studio Express 2013 für Web an. Sie können entweder Microsoft Visual Studio Express 2013 für Web oder Microsoft Visual Studio 2013 verwenden, zum Abschließen dieser Reihe von Lernprogrammen.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 und Microsoft Visual Studio Express 2013 für das Web wird häufig als Visual Studio in der gesamten dieser Reihe von Lernprogrammen bezeichnet werden.


Wenn Sie bereits eine Version von Visual Studio installiert haben, wird während des Installationsvorgangs neben der vorhandenen Version Visual Studio 2013 oder Microsoft Visual Studio Express 2013 für Web installieren. In früheren Versionen erstellten Standorte können in Visual Studio 2013 geöffnet werden und weiterhin in früheren Versionen öffnen.

> [!NOTE] 
> 
> In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass die Auswahl der *Webentwicklung* Auflistung von Einstellungen zum ersten Mal starten von Visual Studio. Weitere Informationen finden Sie unter [wie: Wählen Sie Web Development Umgebungseinstellungen](https://msdn.microsoft.com/en-us/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Laden Sie die Beispielanwendung

Nach der Installation der erforderlichen Komponenten können Sie beginnen, erstellen das neue Web-Projekt, das bereitgestellt wird, in diesem Lernprogramm Reihe. Wenn Sie eine möchten **optional** Ausführen der beispielanwendung, die diese Reihe von Lernprogrammen erstellt, Sie können es herunterladen von der Website für MSDN-Beispiele. Dieser Download enthält Folgendes:

- Die beispielanwendung in der *WingtipToys* Ordner.
- Ressourcen zum Erstellen der beispielanwendung in der *WingtipToys Bestand* Ordner in der *WingtipToys* Ordner.

#### <a name="download-the-file-from-msdn-samples-site"></a>Die Datei von MSDN-Beispiele-Website herunterladen:

[Erste Schritte mit ASP.NET 4.5 WebForms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) 

Der Download ist eine *ZIP* Datei. Anzeigen des abgeschlossenen Projekts, die diese Reihe von Lernprogrammen erstellt, suchen und wählen Sie die *c#*Ordner in der *ZIP* Datei. Speichern Sie die *c#* den Ordner aus dem Ordner, die Sie zum Arbeiten mit Visual Studio 2013-Projekte verwenden. Standardmäßig lautet der Ordner von Visual Studio 2013-Projekte wie folgt:

**C:\Users\*****&lt;Benutzername&gt;*** \Documents\Visual Studio 2013\Projects**

Benennen Sie die ***c#*** Ordner ***WingtipToys***.

> [!NOTE]
> Wenn bereits einen Ordner namens *WingtipToys* im Projektordner, benennen Sie vorübergehend vorhandenen Ordner vor dem Umbenennen der *c#* Ordner *WingtipToys*.


Öffnen Sie zum Ausführen des abgeschlossenen Projekts, das *WingtipToys* Ordner und doppelklicken Sie auf die *WingtipToys.sln* Datei. Visual Studio 2013 wird das Projekt zu öffnen. Als Nächstes mit der rechten Maustaste die *"default.aspx"* im Fenster Projektmappen-Explorer die Datei, und klicken Sie mit der rechten Maustaste In Browser anzeigen.

### <a name="tutorial-support-and-comments"></a>Lernprogramm Support und Kommentare

Verwenden Sie den f und A-Abschnitt enthalten die [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)-Beispiel für Fragen und Kommentare.

Kommentare für diese Reihe von Lernprogrammen Willkommen sind, und bei der Aktualisierung dieser Reihe von Lernprogrammen bemüht vorgenommen werden berücksichtigt Konto Korrekturen oder Vorschläge für Verbesserungen, die in den Kommentaren Tutorial bereitgestellt werden.

Wenn ein Fehler tritt auf, während der Entwicklung, oder die Website nicht ordnungsgemäß ausgeführt werden kann, wird die Fehlermeldungen können komplexe Informationen zu den Ursachen geben, an die Quelle des Problems oder möglicherweise nicht erläutert, wie sie diesen Fehler beheben. Sie können auch verwenden, um Sie für einige allgemeine Problemszenarien zu finden, die [ASP.NET Foren](https://forums.asp.net/) oder im Lieferumfang von f und A-Abschnitt der [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ( C#) Beispiel. Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie die Lernprogramme absolvieren, achten Sie darauf, dass Sie die oben genannten Speicherorte zu überprüfen.

>[!div class="step-by-step"]
[Nächste](create-the-project.md)
