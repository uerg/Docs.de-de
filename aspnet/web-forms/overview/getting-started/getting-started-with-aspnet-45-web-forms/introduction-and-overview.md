---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 | Microsoft-Dokumentation
author: Erikre
description: Diese detaillierte lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung, die mit ASP.NET 4.5 und Microsoft Visual Studio Express Edition-Datenbank...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: b9ed6ce4ac13f047f53c56e183433cbd038ea15c
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450683"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

Diese detaillierte lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. 

## <a name="introduction"></a>Einführung

In dieser tutorialreihe führt Sie durch die erforderlichen Schritte zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von Visual Studio Express 2013 für Web und ASP.NET 4.5.

Die Anwendung, Sie erstellen, den Namen **Wingtip Toys**. Es ist ein vereinfachtes Beispiel einer Speicher-Front-Website, das Elemente online verkauft. Dieser tutorialreihe werden die neuen Features in ASP.NET 4.5 hervorgehoben.

Kommentare sind Willkommen, und wir erstellen die höchste anstrengungen, um dieser tutorialreihe basierend auf Ihren Vorschlägen zu aktualisieren.

### <a name="download-completed-project"></a>Heruntergeladenen Projekt

Sie können ein C#-Projekt herunterladen, die das vollständige Lernprogramm enthält.

- [Erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Überprüfen Sie den Inhalt, indem die zugehörigen ASP.NET Web Forms-Quiz

Nachdem Sie dieses Tutorial abgeschlossen haben, testen Sie Ihr Wissen und wichtige Konzepte zu verstärken, Dank der [ASP.NET Web Forms-Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Dieses Quiz wurde speziell von Inhalt in dieser Reihe von Tutorials entwickelt. Jede Frage, im Quiz erläutert sowie Links zu weiteren Anleitungen.

- [ASP.NET Web Forms-Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Zielgruppe

Die Zielgruppe für diese tutorialreihe ist erfahrene Entwickler zur Verfügung, die mit ASP.NET Web Forms vertraut sind. Ein Entwickler, die in dieser tutorialreihe möchten, müssen die folgenden Fähigkeiten:

- Vertraute mit einem Objekt dienstorientierten Programmiersprache (OOP)
- Mit den Web Development-Konzepten (HTML, CSS, JavaScript)
- Mit relationalen Datenbanken vertraut sind.
- Mit den Konzepten der n-schichtige Architektur

Wenn Sie möchten, die oben aufgeführten Bereiche sind, berücksichtigen Sie Folgendes überprüfen:

- [Erste Schritte mit Visual c#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Webentwicklung](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP und JQuery](http://w3schools.com/)
- [Relationale Datenbank](http://en.wikipedia.org/wiki/Relational_database)
- [Multi-Tier-Architektur](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Anwendungsfeatures

Die ASP.NET Web Form-Features, die in dieser Serie vorgestellten umfassen:

- Das Webanwendungsprojekt (keine Website-Projekt)
- Web Forms
- Masterseiten, Konfiguration
- Bootstrap-Stil
- Entitätsframework Code First, LocalDB
- Request-Überprüfung
- Modellbindung, Datenanmerkungen, stark typisierte Datensteuerelemente, und der Wert von Anbietern
- SSL- und OAuth
- ASP.NET Identity, Konfiguration und -Autorisierung
- Unaufdringliche Validierung
- Routing
- ASP.NET – Fehlerbehandlung

### <a name="application-scenarios-and-tasks"></a>Anwendungsszenarien und Aufgaben

Aufgaben, die in dieser Reihe veranschaulicht:

- Erstellen, überprüfen und Ausführen des neuen Projekts
- Erstellen die Struktur der Datenbank
- Initialisieren und das seeding der Datenbank
- Anpassen der Benutzeroberfläche mit Stilen, Grafiken und eine Masterseite
- Hinzufügen von Seiten und navigation
- Anzeigen von Menü-Details und Produktdaten
- Erstellen einen Einkaufswagen gelegt hat
- Hinzufügen von SSL und OAuth-Unterstützung
- Eine Zahlungsmethode hinzufügen
- Einschließlich der Rolle Administrator angehören und ein Benutzer der Anwendung
- Einschränken des Zugriffs auf bestimmte Seiten und Ordner
- Hochladen einer Datei in der Webanwendung
- Implementieren der eingabeüberprüfung
- Registrieren von Routen für die Webanwendung
- Implementieren der Fehlerbehandlung und Protokollierung von Anzeigefehlern

## <a name="overview"></a>Übersicht

Wenn Sie mit ASP.NET Web Forms vertraut sind, weisen aber Kenntnisse im Umgang mit Programmierkonzepten, Sie haben das richtige Tutorial. Wenn Sie bereits mit ASP.NET Web Forms vertraut sind, können Sie aus dieser tutorialreihe durch die neuen Features in ASP.NET 4.5 profitieren. Wenn Sie sich mit Konzepten und ASP.NET Web Forms-Programmierung auskennen, finden Sie unter der Weitere Lernprogramme zur Verfügung gestellt, in den Webformularen [Einstieg](../../../index.md) Abschnitt auf der ASP.NET-Website.

Die spezifischen **neueste** ASP.NET 4.5 Funktionen bereitgestellte in dieser Web Forms tutorialreihe Folgendes einschließen:

- Eine einfache Benutzeroberfläche zum Erstellen von Projekten dieses Angebot [Unterstützung für mehrere ASP.NET Frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC und Web-API).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), ein Layout und Design-Framework, reaktionsschnelle Entwurfs- und Designfunktionen Funktionen bietet.
- [ASP.NET Identity](../../../../identity/index.md), ein neues ASP.NET-Mitgliedschaftssystem, die in allen ASP.NET-Framework-Versionen und funktioniert mit Web-Software als IIS-hosting funktioniert.
- [Entitätsframework 6](https://msdn.microsoft.com/data/ef.aspx), ein Update für das Entity Framework, das ermöglicht das Abrufen und Bearbeiten von Daten als stark typisierte Objekte, den Zugriff auf Daten asynchron Behandeln von vorübergehenden Verbindungsfehlern, und melden Sie sich SQL-Anweisungen.

Eine vollständige Liste der ASP.NET 4.5-Funktionen, finden Sie unter [ASP.NET and Web Tools für Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Die Wingtip Toys-Beispielanwendung

Die folgenden Screenshots bieten einen schnellen Überblick über die ASP.NET Web Forms-Anwendung, die Sie in diesem Tutorial erstellen. Wenn Sie die Anwendung aus Visual Studio Express 2013 für Web ausführen, sehen Sie die folgenden Web-Homepage.

![Wingtip Toys - Standardseite](introduction-and-overview/_static/image1.png)

Sie können als neuer Benutzer registrieren, oder melden Sie sich als einen vorhandenen Benutzer. Navigation wird am oberen für jede Produktkategorie bereitgestellt, indem Sie die verfügbaren Produkte aus der Datenbank abrufen.

Wenn Sie den Link Produkte auswählen, werden Sie sehen eine Liste aller verfügbaren Produkte.

![Wingtip Toys - Produkte](introduction-and-overview/_static/image2.png)

Einzelne Produktdetails sehen auch dazu einen der aufgeführten Produkte.

![Wingtip Toys - Produktinformationen](introduction-and-overview/_static/image3.png)

Sie können als Benutzer registrieren und melden Sie sich mit die Standardfunktionen der Web Forms-Vorlage. In diesem Tutorial wird erklärt, wie die Anmeldung mit einem vorhandenen Gmail-Konto. Darüber hinaus können sich anmelden, als Administrator hinzufügen und Entfernen von Produkten aus der Datenbank.

![Wingtip Toys - Anmeldung](introduction-and-overview/_static/image4.png)

Nachdem Sie sich als Benutzer angemeldet haben, können Sie Produkte auf den Einkaufswagen und Diskussion zum Bezahlvorgang mit PayPal hinzufügen. Beachten Sie, dass diese Anwendung konzipiert ist, mit PayPals-Entwickler-Sandbox. Keine Transaktion tatsächlich Zahlung erfolgt.

![Wingtip Toys - Warenkorb](introduction-and-overview/_static/image5.png)

PayPal wird Ihr Konto, Reihenfolge und Zahlungsinformationen zu bestätigen.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Sie können nach der Rückkehr aus PayPal, überprüfen und Abschließen der Bestellung.

![Wingtip Toys - Order-Überprüfung](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Vorraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass Sie die folgende Software auf Ihrem Computer installiert haben:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework wird automatisch installiert.

Dieser tutorialreihe verwendet Microsoft Visual Studio Express 2013 für Web. Sie können entweder von Microsoft Visual Studio Express 2013 für Web oder Microsoft Visual Studio 2013 verwenden, zum Abschließen dieser tutorialreihe.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 und Microsoft Visual Studio Express 2013 für Web wird häufig als Visual Studio in dieser tutorialreihe bezeichnet werden.


Wenn Sie bereits über eine Visual Studio-Version installiert haben, wird während des Installationsvorgangs neben der vorhandenen Version Visual Studio 2013 oder Microsoft Visual Studio Express 2013 für Web installieren. In früheren Versionen erstellten Standorte können in Visual Studio 2013 geöffnet werden und weiterhin in früheren Versionen öffnen.

> [!NOTE] 
> 
> In dieser exemplarischen Vorgehensweise wird davon ausgegangen, dass die Auswahl der *Webentwicklung* Sammlung von Einstellungen der erstmaligen Start von Visual Studio. Weitere Informationen finden Sie unter [Vorgehensweise: Aktivieren Sie Umgebungseinstellungen für die Webentwicklung](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Herunterladen der Beispielanwendung

Nach der Installation der erforderlichen Komponenten können Sie mit dem Erstellen von neuen Webprojekts, das angezeigt werden in diesem Tutorial beginnen. Wenn Sie möchten **optional** ausführen die beispielanwendung, die dieser tutorialreihe erstellt, Sie können es von der Website für MSDN-Beispiele. Dieser Download enthält Folgendes:

- Die beispielanwendung in der *WingtipToys* Ordner.
- Die Ressourcen, die zum Erstellen der beispielanwendung in der *WingtipToys-Assets* Ordner in der *WingtipToys* Ordner.

#### <a name="download-the-file-from-msdn-samples-site"></a>Die Datei von MSDN-Beispiele-Website herunterladen:

[Erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#) 

Der Download ist eine <em>ZIP</em> Datei. Um anzuzeigen, suchen und Auswählen des abgeschlossenen Projekts, das dieser tutorialreihe erstellt die <em>c#</em>Ordner in der <em>ZIP</em> Datei. Speichern Sie die <em>c#</em> den Ordner aus dem Ordner, die Sie verwenden, um mit Visual Studio 2013-Projekten zu arbeiten. Standardmäßig lautet der Ordner von Visual Studio 2013-Projekte wie folgt:

<strong>C:\Users&#92;</strong><strong><em>&lt;Benutzername&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong>

Benennen Sie die ***c#*** Ordner ***WingtipToys***.

> [!NOTE]
> Wenn Sie bereits einen Ordner namens haben *WingtipToys* im Projektordner, bevor Sie Sie umbenennen, vorhandenen Ordner vorübergehend Umbenennen der *c#* Ordner *WingtipToys*.


Öffnen Sie zum Ausführen des abgeschlossenen Projekts der *WingtipToys* Ordner und doppelklicken Sie auf die *WingtipToys.sln* Datei. Visual Studio 2013 wird das Projekt zu öffnen. Als Nächstes mit der rechten Maustaste die *"default.aspx"* im Projektmappen-Explorer die Datei, und klicken Sie mit der rechten Maustaste auf In Browser anzeigen.

### <a name="tutorial-support-and-comments"></a>Tutorial Support und Kommentare

Verwenden Sie den f und A-Abschnitt, der in enthalten die [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)-Beispiel für Fragen und Kommentare.

Kommentare zu dieser tutorialreihe sind Willkommen, und bei der Aktualisierung dieser tutorialreihe wird bemüht versucht, berücksichtigt der Konto-Korrekturen oder Vorschläge für Verbesserungen, die in den Kommentaren zum Tutorial bereitgestellt werden.

Wenn ein Fehler tritt auf, während der Entwicklung oder die Website nicht ordnungsgemäß ausgeführt wird, wird die Fehlermeldungen können komplexe Hinweise geben, an die Quelle des Problems oder können es nicht erläutert, wie Sie diesen Fehler beheben. Sie können auch verwenden, um Sie mit einigen gängigen Problemszenarien zu unterstützen, die [ASP.NET-Foren](https://forums.asp.net/) oder im Lieferumfang von f und A-Abschnitt der [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) Beispiel. Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wie Sie die Tutorials durchgehen, achten Sie darauf, dass Sie die oben genannten Speicherorten zu suchen.

> [!div class="step-by-step"]
> [Nächste](create-the-project.md)
