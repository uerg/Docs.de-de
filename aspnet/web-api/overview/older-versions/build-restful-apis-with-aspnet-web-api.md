---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Erstellen von RESTful-APIs mit ASP.NET Web-API | Microsoft-Dokumentation
author: rick-anderson
description: In den letzten Jahren hat er deutlich werden, dass HTTP nicht ist nur für HTML-Seiten bereitstellt. Es ist auch eine leistungsstarke Plattform zum Erstellen von Web-APIs, die über eine Handvoll o...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9866b5f75771c633165587ba04e694f72a1e626c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835598"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Erstellen von RESTful-APIs mit ASP.NET Web-API
====================
durch [Web Camps Team](https://twitter.com/webcamps)

> In den letzten Jahren hat er deutlich werden, dass HTTP nicht ist nur für HTML-Seiten bereitstellt. Es ist auch eine leistungsstarke Plattform zum Erstellen von Web-APIs, mit ein paar Verben (GET, POST usw.) sowie einige einfache Konzepte wie *URIs* und *Header*. ASP.NET Web-API ist eine Reihe von Komponenten, die HTTP-Programmiermodell zu vereinfachen. Da es auf die ASP.NET MVC-Laufzeit aufgebaut ist, verarbeitet Web-API automatisch die Low-Level Transportdetails von HTTP. Zur gleichen Zeit stellt Web-API auf natürliche Weise das HTTP-Programmiermodell. Ein Ziel der Web-API in der Tat ist es, *nicht* abstrahieren der Verwendung von HTTP. Daher ist Web-API, flexibel und einfach zu erweitern. In dieser praktischen Übungseinheit verwenden Sie die Web-API, um eine einfache REST-API für eine Kontakt-Manager-Anwendung zu erstellen. Erstellen Sie auch einen Client für die API verwenden. Architektonische REST-Stil erwiesenermaßen eine effektive Methode zur HTTP - Nutzen sein, obwohl es sicherlich nicht der einzige gültige Ansatz für HTTP ist. Der Kontakte-Manager wird die RESTful zum Auflisten, hinzufügen und Entfernen von Kontakten, unter anderem verfügbar machen. Dieses erfordert ein grundlegendes Verständnis von HTTP, REST, und setzt voraus, dass Sie über grundlegende Kenntnisse von HTML, JavaScript und jQuery verfügen.
> 
> > [!NOTE]
> > Die ASP.NET-Website verfügt über einen speziellen Bereich für die ASP.NET Web-API-Framework unter [ https://asp.net/web-api ](https://asp.net/web-api). Dieser Standort wird weiterhin bieten aktuelle Informationen, Beispiele und Nachrichten, die im Zusammenhang mit der Web-API so überprüfen Sie sie häufig, wenn Sie tiefer in die Kunst gerne der benutzerdefinierten Web-APIs auf praktisch jedem Gerät oder Entwicklung Framework erstellen.
> > 
> > ASP.NET Web-API, wie ASP.NET MVC 4 verfügt über mehr Flexibilität im Hinblick auf die Trennung von der Dienstebene aus den Controllern, und Sie können mehrere Frameworks verfügbar Dependency Injection recht einfach zu verwenden. Ist es ein gutes Beispiel in MSDN, die zeigt, wie Ninject für Dependency Injection in ASP.NET Web-API-Projekt, das Sie es aus herunterladen [hier](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Implementieren einer RESTful-Web-API
- Aufrufen der API aus einem HTML-client

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit auszuführen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang B](#AppendixB) Anleitungen zur Installation).

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang A: Verwenden von Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Dieser praktischen Übungseinheit enthält in der folgenden Übung werden:

1. [Übung 1: Erstellen einer nur-Lese Webs-API](#Exercise1)
2. [Übung 2: Erstellen einer Lese-/Schreibzugriff-Webs-API](#Exercise2)
3. [Übung 3: Nutzen Sie die Web-API aus einem HTML-Client](#Exercise3)

> [!NOTE]
> Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll. Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.


Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Übung 1: Erstellen einer nur-Lese Webs-API

In dieser Übung implementieren Sie die ReadOnly-GET-Methoden für den Kontakt-Manager.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Aufgabe 1: Erstellen das API-Projekt

In dieser Aufgabe verwenden Sie die neuen ASP.NET Web-Projektvorlagen zum Erstellen einer Web-API-Web-Anwendung.

1. Führen Sie **Visual Studio-2012 Express für Web**, Sie gehen Sie dazu zur **starten** , und geben **Visual Studio Express für Web** drücken Sie dann die **EINGABETASTE**.
2. Von der **Datei** , wählen Sie im Menü **neues Projekt**. Wählen Sie die **Visual c# | Web** Projekttyp aus der Strukturansicht des Projekt-Typ aus, und wählen Sie dann die **ASP.NET MVC 4-Webanwendung** Projekttyp. Legen Sie die **Namen** zu *ContactManager* und **Projektmappenname** zu *beginnen*, klicken Sie dann auf **OK**.

    ![Erstellen einer neuen ASP.NET MVC 4.0-Webanwendungsprojekt](build-restful-apis-with-aspnet-web-api/_static/image1.png "erstellen eine neue ASP.NET MVC 4.0-Webanwendungsprojekt")

    *Erstellen einer neuen ASP.NET MVC 4.0-Webanwendungsprojekt*
3. Wählen Sie im Dialogfeld für den Typ ASP.NET MVC 4-Projekt die **Web-API-** Projekttyp. Klicken Sie auf **OK**.

    ![Gibt den Typ der Web-API-Projekt](build-restful-apis-with-aspnet-web-api/_static/image2.png ", die den Typ der Web-API-Projekt")

    *Gibt den Typ der Web-API-Projekt*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Aufgabe 2: Erstellen der Kontakt-Manager-API-Controller

In dieser Aufgabe erstellen Sie die Controllerklassen in denen API-Methoden befinden soll.

1. Löschen Sie die Datei mit dem Namen **ValuesController.cs** in **Controller** Ordner aus dem Projekt.
2. Mit der rechten Maustaste die **Controller** Ordner im Projekt, und wählen Sie **hinzufügen | Controller** aus dem Kontextmenü.

    ![Hinzufügen eines neuen Controllers zum Projekt](build-restful-apis-with-aspnet-web-api/_static/image3.png "Hinzufügen eines neuen Controllers zum Projekt")

    *Hinzufügen eines neuen Controllers zum Projekt*
3. In der **Controller hinzufügen** Dialogfeld, das angezeigt wird, wählen Sie **leerer API-Controller** Menü der Vorlage. Nennen Sie die Controllerklasse **ContactController**. Klicken Sie auf **hinzufügen.**

    ![Verwenden das Dialogfeld "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers](build-restful-apis-with-aspnet-web-api/_static/image4.png "verwenden das Dialogfeld \"Controller hinzufügen\" zum Erstellen eines neuen Web-API-Controllers")

    *Verwenden das Dialogfeld "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers*
4. Fügen Sie den folgenden Code der **ContactController**.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Get-API-Methode*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Drücken Sie **F5** zum Debuggen der Anwendung. Die Standardstartseite für eine Web-API-Projekt sollte angezeigt werden.

    ![Standard-Startseite einer ASP.NET Web-API-Anwendung](build-restful-apis-with-aspnet-web-api/_static/image5.png "Standardstartseite einer ASP.NET Web-API-Anwendung")

    *Standard-Startseite einer ASP.NET Web-API-Anwendung*
6. Drücken Sie in Internet Explorer-Fenster, das **F12** Schlüssel zum Öffnen der **Entwicklertools** Fenster. Klicken Sie auf die **Netzwerk** Registerkarte, und klicken Sie dann auf die **erfassen starten** Schaltfläche beginnt die Erfassung von Netzwerkdatenverkehr in das Fenster.

    ![Öffnen die Registerkarte "Netzwerk" und das Initiieren der netzwerkerfassung](build-restful-apis-with-aspnet-web-api/_static/image6.png "öffnen die Registerkarte \"Netzwerk\" und das Initiieren der netzwerkerfassung")

    *Öffnen die Registerkarte "Netzwerk" und dem Initiieren der netzwerkerfassung*
7. Fügen Sie die URL in die Sie in der Adressleiste des Browsers mit **/api/Contact** , und drücken Sie die EINGABETASTE. Die Details für die Übertragung werden im Netzwerk erfassen-Fenster angezeigt. Beachten Sie, dass die Antwort des MIME-Typ **Application/Json**. Dieses Beispiel veranschaulicht, wie das Standardausgabeformat JSON ist.

    ![Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerkansicht](build-restful-apis-with-aspnet-web-api/_static/image7.png "Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerk-Ansicht")

    *Die Ausgabe der Web-API-Anforderung anzeigen in der Netzwerk-Ansicht*

    > [!NOTE]
    > Internet Explorer 10-Standardverhalten werden nun gefragt, ob die Benutzer sollen, speichern oder Öffnen den Stream, aus der Web-API-Aufruf. Die Ausgabe wird eine Textdatei mit dem JSON-Ergebnis des Aufrufs Web-API-URL sein. Brechen Sie nicht das Dialogfeld, um Inhalte von der Antwort durch Entwickler Toolfenster ansehen zu können.
8. Klicken Sie auf die **wechseln Sie zur detaillierten Ansicht** Schaltfläche, um weitere Details zu dieser API-Aufruf die Antwort anzuzeigen.

    ![Wechseln Sie zur detaillierten Ansicht](build-restful-apis-with-aspnet-web-api/_static/image8.png "wechseln Sie zur Detailansicht")

    *Wechseln Sie zur detaillierten Ansicht*
9. Klicken Sie auf die **Antworttext** Registerkarte, um den tatsächlichen Text der JSON-Antwort anzuzeigen.

    ![Anzeigen des JSON-Codes in der Microsoft-Netzwerkmonitor Ausgabetext](build-restful-apis-with-aspnet-web-api/_static/image9.png "Ausgabetext Anzeigen des JSON-Codes in der Microsoft-Netzwerkmonitor")

    *Anzeigen von JSON-Ausgabetext in den Netzwerkmonitor*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Aufgabe 3: beim Erstellen der Modelle wenden Sie sich an, und Erweitern der Contact-Controller

In dieser Aufgabe erstellen Sie die Controllerklassen in denen API-Methoden befinden soll.

1. Mit der rechten Maustaste die **Modelle** Ordner, und wählen **hinzufügen | Klasse...**  aus dem Kontextmenü.

    ![Hinzufügen eines neuen Modells an die Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image10.png "Hinzufügen eines neuen Modells an die Webanwendung")

    *Hinzufügen eines neuen Modells an die Webanwendung*
2. In der **neues Element hinzufügen** Dialogfeld benennen Sie die neue Datei **Contact.cs** , und klicken Sie auf **hinzufügen.**

    ![Erstellen der neuen Kontakt-Klassendatei](build-restful-apis-with-aspnet-web-api/_static/image11.png "-Datei der neue Kontakt erstellen")

    *Erstellen die neue Klassendatei in Kontakt*
3. Fügen Sie den folgenden hervorgehobenen Code in die **wenden Sie sich an** Klasse.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Kontakt Klasse*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. In der **ContactController** Klasse, markieren Sie das Wort **Zeichenfolge** in der Definition der Methode die **erhalten** -Methode, und geben Sie das Wort *wenden Sie sich an*. Sobald Sie in das Wort eingeben, erscheint ein Indikator am Anfang des Worts **wenden Sie sich an**. Entweder halten Sie die **STRG** Schlüssel, und drücken Sie auf den Punkt (.), oder klicken Sie auf das Symbol mit der Maus, um das Dialogfeld "Hilfe" im Code-Editor automatisch ausfüllen zu öffnen der **mit** Direktive für die Modelle Namespace.

    ![Verwenden Intellisense-Unterstützung für Namespacedeklarationen](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Verwenden Intellisense-Unterstützung für Namespacedeklarationen*
5. Ändern Sie den Code für die **erhalten** Methode so, dass die It ein Array von Instanzen der Kontakt-Modell zurückgegeben.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Rückgabe einer Liste von Kontakten*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Drücken Sie **F5** zum Debuggen der Webanwendung im Browser. Um die an die antwortausgabe der API vorgenommenen Änderungen anzuzeigen, führen Sie die folgenden Schritte aus.

   1. Wenn der Browser geöffnet wird, drücken Sie die **F12** , wenn die Developer Tools noch nicht geöffnet sind.
   2. Klicken Sie auf die **Netzwerk** Registerkarte.
   3. Drücken Sie die **erfassen starten** Schaltfläche.
   4. Fügen Sie das URL-Suffix **/api/Contact** an die URL in die Adressleiste ein und drücken Sie die **EINGABETASTE** Schlüssel.
   5. Drücken Sie die **wechseln Sie zur detaillierten Ansicht** Schaltfläche.
   6. Wählen Sie die **Antworttext** Registerkarte. Daraufhin sollte eine JSON-Zeichenfolge, die die serialisierte Form eines Arrays von wenden Sie sich an Instanzen darstellt.

      ![JSON-serialisierte Ausgabe von komplexer Aufruf einer Web-API-Methode](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialisierte Ausgabe von komplexer Aufruf einer Web-API-Methode")

      *JSON-serialisierte Ausgabe von komplexer Aufruf einer Web-API-Methode*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Aufgabe 4: Extrahieren von Funktionen in einer Dienstebene

Diese Aufgabe zeigen, wie Funktionen in einer Dienstschicht erleichtert Entwicklern das Trennen ihrer Dienstfunktionalität von der Controller-Ebene, wodurch die wiederverwendbarkeit der Dienste, die eigentliche Arbeit ausführen zu extrahieren.

1. Erstellen Sie einen neuen Ordner in den Projektmappenstamm, und nennen Sie sie **Services**. Dazu, mit der Maustaste **ContactManager** -Projekt, wählen **hinzufügen** | **neuer Ordner**, nennen Sie sie *Services*.

    ![Erstellen der Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image14.png "diensteordner erstellen")

    *Erstellen Ordner "Dienste"*
2. Mit der rechten Maustaste die **Services** Ordner, und wählen **hinzufügen | Klasse...**  aus dem Kontextmenü.

    ![Hinzufügen einer neuen Klasse zum Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image15.png "eine neue Klasse hinzufügen, um den Ordner \"Dienste\"")

    *Hinzufügen einer neuen Klasse zum Ordner "Dienste"*
3. Wenn die **neues Element hinzufügen** Dialogfeld angezeigt wird, benennen Sie die neue Klasse **ContactRepository** , und klicken Sie auf **hinzufügen**.

    ![Erstellen eine Klassendatei, um den Code für die Dienstebene der Kontakt-Repository enthält](build-restful-apis-with-aspnet-web-api/_static/image16.png "erstellen eine Klassendatei, um den Code für die Dienstebene der Kontakt-Repository enthält")

    *Erstellen eine Klassendatei, um den Code für die Dienstebene der Kontakt-Repository enthält*
4. Hinzufügen einer using-Direktive hinzu der **ContactRepository.cs** Datei, die den Modellen-Namespace enthalten.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Fügen Sie den folgenden hervorgehobenen Code in die **ContactRepository.cs** Datei GetAllContacts-Methode implementieren.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Kontaktrepository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Öffnen Sie die **ContactController.cs** Datei, wenn es nicht bereits geöffnet ist.
7. Fügen Sie die folgenden using-Anweisung mit dem Namespace-Deklaration-Abschnitt der Datei.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Fügen Sie den folgenden hervorgehobenen Code in die **ContactController.cs** Klasse, um ein privates Feld für die Darstellung der Instanz des Repositorys, hinzufügen, damit der Rest der Klasse, die Mitglieder ausführen können der dienstimplementierung.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Contact-Controller*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Ändern der **erhalten** Methode, sodass die It stellt das Kontaktrepository-Dienst verwenden.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Zurückgeben einer Liste von Kontakten über das Repository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Fügen Sie einen Haltepunkt auf der **ContactController**des **erhalten** Methodendefinition.

   ![Haltepunkte hinzufügen, um die Contact-Controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Haltepunkte auf der Contact-Controller hinzufügen")

   *Haltepunkte auf der Contact-Controller hinzufügen*
11. Drücken Sie **F5**, um die Anwendung auszuführen.
12. Wenn der Browser geöffnet wird, drücken Sie die **F12** zu den Entwicklertools zu öffnen.
13. Klicken Sie auf die **Netzwerk** Registerkarte.
14. Klicken Sie auf die **erfassen starten** Schaltfläche.
15. Fügen Sie die URL in die Adressleiste ein, mit dem Suffix **/api/Contact** , und drücken Sie **EINGABETASTE** API-Controller zu laden.
16. Visual Studio 2012 sollte ein Halt **erhalten** Methode mit der Ausführung beginnt.

   ![In der Get-Methode wichtige](build-restful-apis-with-aspnet-web-api/_static/image18.png "wichtige in der Get-Methode")

   *Wichtige in der Get-Methode*
17. Drücken Sie **F5**, um fortzufahren.
18. Wechseln Sie zurück zu Internet Explorer, wenn sie noch nicht im Fokus ist. Beachten Sie das Netzwerk erfassen-Fenster.

    ![Netzwerk-Ansicht in Internet Explorer zeigt die Ergebnisse des Web-API-Aufrufs](build-restful-apis-with-aspnet-web-api/_static/image19.png "Netzwerk anzeigen in Internet Explorer, die Ergebnisse des Web-API-Aufrufs angezeigt.")

    *Netzwerkansicht in Internet Explorer, die Ergebnisse des Web-API-Aufrufs angezeigt.*
19. Klicken Sie auf die **wechseln Sie zur detaillierten Ansicht** Schaltfläche.
20. Klicken Sie auf die **Antworttext** Registerkarte. Beachten Sie die JSON-Ausgabe von den API-Aufruf, und wie sie die zwei Kontakte abgerufen, indem die Dienstschicht darstellt.

    ![Anzeigen von der JSON-Ausgabe aus der Web-API im Fenster mit Entwicklertools](build-restful-apis-with-aspnet-web-api/_static/image20.png "Anzeigen der JSON-Ausgabe aus der Web-API im Fenster mit Entwicklertools")

    *Anzeigen von der JSON-Ausgabe aus der Web-API im Fenster mit Entwicklertools*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Übung 2: Erstellen einer Lese-/Schreibzugriff-Webs-API

In dieser Übung implementieren Sie die POST und PUT-Methoden für die der Kontakt-Manager, um sie mit Features Bearbeiten von Daten zu aktivieren.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Aufgabe 1: Öffnen die Web-API-Projekt

In dieser Aufgabe bereiten Sie zur Verbesserung der Web-API-Projekts in Übung 1 erstellt werden, sodass sie Benutzereingaben akzeptieren kann.

1. Führen Sie **Visual Studio-2012 Express für Web**, Sie gehen Sie dazu zur **starten** , und geben **Visual Studio Express für Web** drücken Sie dann die **EINGABETASTE**.
2. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex02-ReadWriteWebAPI/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
3. Öffnen der **Services/ContactRepository.cs** Datei.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Aufgabe 2: Hinzufügen von Datenpersistenz Features für die Implementierung Kontaktrepository

In dieser Aufgabe werden Sie die ContactRepository-Klasse des Web-API-Projekts, damit es beibehalten und akzeptieren von Benutzereingaben und neuer Kontakt-Instanzen kann die in Übung 1 erstellten erweitern.

1. Fügen Sie die folgende Konstante, die die **ContactRepository** Klasse, um den Namen des Cache Element Schlüssel den Namen des Webservers später in dieser Übung darzustellen.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Fügen Sie einen Konstruktor, der **ContactRepository** mit dem folgenden Code.

    (Codeausschnitt - *Web-API-Lab - Ex02 - Kontaktrepository Konstruktor*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Ändern Sie den Code für die **GetAllContacts** Methode wie unten dargestellt.

    (Codeausschnitt - *Web-API-Lab - Ex02 - Get All Contacts*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > In diesem Beispiel dient zu Demonstrationszwecken und verwendet die Webserver Cache als einem Speichermedium, damit die Werte werden verfügbar sein, um mehrere Clients gleichzeitig, anstatt einen Speichermechanismus für die Sitzung oder eine Anforderung Storage Lebensdauer verwenden. Eine kann Entity Framework, XML-Speicherung oder jeder anderen Sorte anstelle der Web-Server-Cache verwenden.
4. Implementieren Sie eine neue Methode namens **SaveContact** auf die **ContactRepository** Klasse, um die Arbeit ein Kontakts zu speichern. Die **SaveContact** Methode dauert ein einzelnes **wenden Sie sich an** Parameter und Rückgabetypen ein boolescher Wert, der angibt, Erfolg oder Fehler.

    (Codeausschnitt - *Web-API-Lab - Ex02 - Implementierung der Methode SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Übung 3: Nutzen Sie die Web-API aus einem HTML-Client

In dieser Übung erstellen Sie einen HTML-Client zum Aufrufen der Web-API. Dieser Client wird zu den Datenaustausch mit der Web-API mit JavaScript vereinfachen und die Ergebnisse in einem Webbrowser mit HTML-Markup angezeigt.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Aufgabe 1: Ändern der Ansicht "Index", um eine grafische Benutzeroberfläche zu bieten, für die Anzeige von Kontakten

In dieser Aufgabe ändern Sie die Standardansicht der Index der Webanwendung zur Unterstützung der Anforderung die Liste der vorhandenen Kontakte in einem HTML-Browser anzuzeigen.

1. Öffnen Sie **Visual Studio-2012 Express für Web** , wenn es nicht bereits geöffnet ist.
2. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex03-ConsumingWebAPI/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
3. Öffnen der **"Index.cshtml"** Datei **Views/Home** Ordner.
4. Ersetzen Sie den HTML-Code in das Div-Element mit der Id **Text** , damit sie sich wie der folgende Code aussieht.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Fügen Sie den folgenden Javascript-Code am Ende der Datei zum Ausführen der HTTP-Anforderung an die Web-API.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Öffnen Sie die **ContactController.cs** Datei, wenn es nicht bereits geöffnet ist.
7. Fügen Sie einen Haltepunkt auf der **erhalten** Methode der **ContactController** Klasse.

    ![Platzieren einen Haltepunkt für die Get-Methode der API-Controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "einen Haltepunkt für die Get-Methode der API-Controller einfügen")

    *Platzieren einen Haltepunkt für die Get-Methode der API-controller*
8. Drücken Sie **F5** um das Projekt auszuführen. Der Browser lädt das HTML-Dokument.

    > [!NOTE]
    > Stellen Sie sicher, dass Sie auf die Stamm-URL Ihrer Anwendung navigieren.
9. Sobald die Seite geladen wird, und der JavaScript-Code ausgeführt wird, wird der Haltepunkt erreicht, und die Ausführung von Code im Controller hält.

    ![Debuggen in der Web-API-Aufrufe, die mithilfe von Visual Studio Express für Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debuggen in der Web-API-Aufrufe, die mithilfe von Visual Studio Express für Web")

    *Debuggen in der Web-API-Aufruf mithilfe von Visual Studio 2012 Express für Web*
10. Entfernen Sie den Haltepunkt, und drücken Sie **F5** oder der debugging-Symbolleiste **Weiter** Schaltfläche, um den Vorgang fortzusetzen, laden die Ansicht im Browser. Nach Abschluss der Web-API-Aufruf sollte im Browser dargestellt als von Listenelementen rufen Sie aus der Web-API zurückgegebenen Kontakte angezeigt werden.

    ![Ergebnisse von den API-Aufruf im Browser angezeigt wird, als Listenelemente](build-restful-apis-with-aspnet-web-api/_static/image23.png "Ergebnisse von den API-Aufruf im Browser angezeigt wird, als Listenelemente")

    *Ergebnisse von den API-Aufruf im Browser angezeigt wird, als Listenelemente*
11. Beenden des Debuggens.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Aufgabe 2: Ändern der Ansicht "Index", um eine grafische Benutzeroberfläche für die Erstellung von Contacts bereitzustellen

In dieser Aufgabe werden Sie weiterhin so ändern Sie die Ansicht "Index", von der MVC-Anwendung. Die HTML-Seite, die Erfassen von Benutzereingaben und senden Sie sie an die Web-API zum Erstellen eines neuen Kontakts wird ein Formular hinzugefügt werden, und eine neue Web-API-Controller-Methode erstellt wird, um das Datum über die grafische Benutzeroberfläche zu sammeln.

1. Öffnen der **ContactController.cs** Datei.
2. Fügen Sie eine neue Methode für die Controllerklasse, die mit dem Namen **Post** wie im folgenden Code gezeigt.

    (Codeausschnitt - *Web-API-Lab - Ex03 - Post-Methode*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Öffnen Sie die **"Index.cshtml"** Datei in Visual Studio, wenn es nicht bereits geöffnet ist.
4. Fügen Sie den folgenden HTML-Code in die Datei direkt nach der unsortierten Liste, die Sie in der vorherigen Aufgabe hinzugefügt.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. Fügen Sie in der Script-Element, am unteren Rand des Dokuments werden soll folgenden hervorgehobenen Code zum Behandeln von Schaltfläche – Click-Ereignisse, die die Daten an die Web-API sendet, über eine HTTP POST-Aufruf hinzu.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. In **ContactController.cs**, platzieren Sie einen Haltepunkt auf der **Post** Methode.
7. Drücken Sie **F5** zum Ausführen der Anwendung im Browser.
8. Sobald die Seite im Browser geladen wird, geben Sie einen neuen Kontaktnamen und -Id und klicken Sie auf die **speichern** Schaltfläche.

    ![Das Client-HTML-Dokument geladen wird, im Browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "das Client-HTML-Dokument geladen wird, im Browser")

    *Das Client-HTML-Dokument im Browser geladen.*
9. Wenn das Debuggerfenster Zeilenumbrüche, in der **Post** -Methode, verschaffen Sie sich einen Blick auf die Eigenschaften des der **wenden Sie sich an** Parameter. Die Werte sollten mit den Daten übereinstimmen, die Sie in das Formular eingegeben haben.

    ![Der Contact-Objekt, das an die Web-API vom Client gesendeten](build-restful-apis-with-aspnet-web-api/_static/image25.png "der Contact-Objekt, an die Web-API vom Client gesendeten")

    *Der Contact-Objekt, das an die Web-API vom Client gesendeten*
10. Durchlaufen Sie die Methode im Debugger bis der **Antwort** Variable erstellt wurde. Bei näherer Untersuchung in der **"lokal"** Fenster im Debugger, werden Sie feststellen, dass alle Eigenschaften festgelegt wurden.

   ![Die Antwort nach der Erstellung im Debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "die Antwort nach der Erstellung im Debugger")

   *Die Antwort nach der Erstellung im debugger*
11. Wenn Sie drücken **F5** oder klicken Sie auf **Weiter** im Debugger die Anforderung abgeschlossen wird. Nachdem Sie sich an den Browser wechseln, der neue Kontakt hinzugefügt wurde die Liste der Kontakte, die von gespeicherten der **ContactRepository** Implementierung.

   ![Der Browser gibt erfolgreichen Erstellung der neuen Instanz wenden Sie sich an](build-restful-apis-with-aspnet-web-api/_static/image27.png "Browser gibt erfolgreichen Erstellung der neuen Instanz wenden Sie sich an")

   *Der Browser gibt erfolgreichen Erstellung der neuen Instanz wenden Sie sich an*

> [!NOTE]
> Darüber hinaus können Sie diese Anwendung in Azure Folgendes bereitstellen [Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Dieses Lab wurde das neue ASP.NET Web-API-Framework und die Implementierung von RESTful Web-APIs, die mit dem Framework eingeführt. Von hier aus können Sie ein neues Repository, das Dauerhaftigkeit von Daten mit einer Reihe verschiedener Mechanismen erleichtert erstellen und socialsharing.Share dieses Diensts anstelle der einfachen in dieser Übungseinheit als Beispiel bereitgestellt. Web-API unterstützt zahlreiche zusätzliche Funktionen, z. B. das Aktivieren der Kommunikation mit nicht-HTML-Clients, die in jeder beliebigen Sprache, die von HTTP und JSON oder XML unterstützt. Die Möglichkeit zum Hosten einer Web-API außerhalb einer typischen Webanwendung ist auch möglich, als auch ist die Möglichkeit, Ihre eigenen Serialisierungsformate zu erstellen.

Die ASP.NET-Website verfügt über einen speziellen Bereich für die ASP.NET Web-API-Framework unter [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Dieser Standort wird weiterhin bieten aktuelle Informationen, Beispiele und Nachrichten, die im Zusammenhang mit der Web-API so überprüfen Sie sie häufig, wenn Sie tiefer in die Kunst gerne der benutzerdefinierten Web-APIs auf praktisch jedem Gerät oder Entwicklung Framework erstellen.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Anhang A: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code zur Hand benötigten. Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](build-restful-apis-with-aspnet-web-api/_static/image28.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")

*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)

1. Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.
2. Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.
4. Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.

    ![Geben Sie den Namen des Ausschnitts](build-restful-apis-with-aspnet-web-api/_static/image29.png "Geben Sie den Namen des Ausschnitts")

    *Geben Sie den Namen des Ausschnitts*

    ![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](build-restful-apis-with-aspnet-web-api/_static/image30.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")

    *Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*

    ![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](build-restful-apis-with-aspnet-web-api/_static/image31.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")

    *Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)

1. Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.
2. Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.
3. Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.

    ![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](build-restful-apis-with-aspnet-web-api/_static/image32.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

    *Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

    ![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](build-restful-apis-with-aspnet-web-api/_static/image33.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")

    *Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Anhang B: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

In diesem Anhang erfahren Sie, wie eine neue Website aus dem Azure-Portal erstellen und Veröffentlichen der Anwendung, die Sie erhalten haben, indem der testumgebung können die Nutzung der Web Deploy-publishing-Funktion von Azure bereitgestellten.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website über das Azure-Portal

1. Wechseln Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Mit Azure können Sie 10 ASP.NET-Websites kostenlos hosten und dann zu skalieren, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Verwaltungsportal*
2. Klicken Sie auf **neu** auf der Befehlsleiste.

    ![Erstellen einer neuen Website](build-restful-apis-with-aspnet-web-api/_static/image40.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Azure ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können. Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung in Azure von außerhalb des Portals bereitstellen. Er umfasst keine Informationen zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der Schnellerfassung](build-restful-apis-with-aspnet-web-api/_static/image41.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")

    *Erstellen einer neuen Website mithilfe der Schnellerfassung*
4. Warten Sie, bis die neue **Website** erstellt wird.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website ausgeführt wird.

    ![Navigieren zu der neuen Website](build-restful-apis-with-aspnet-web-api/_static/image42.png "auf die neue Website durchsuchen")

    *Um die neue Website durchsuchen*

    ![Website](build-restful-apis-with-aspnet-web-api/_static/image43.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](build-restful-apis-with-aspnet-web-api/_static/image44.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung auf Azure für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Azure.

    ![Herunterladen der Website-Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image45.png "herunterladen den Website-Veröffentlichungsprofil")

    *Herunterladen der Website-Veröffentlichungsprofil*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter. In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung in Azure aus Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image46.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbank-Servers

Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen. Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank. Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement im Azure-Verwaltungsportal auf **Sql-Datenbanken** | **Server** | **Dashboard des Servers**. Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.

    ![SQL Server-Datenbank-Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "Dashboard der SQL-Datenbank-Server")

    *SQL Server-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**. Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Bestätigen von Änderungen*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

1. Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](build-restful-apis-with-aspnet-web-api/_static/image51.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren das Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.

    ![Überprüfen der Verbindung](build-restful-apis-with-aspnet-web-api/_static/image53.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.
   - In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.
   - In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.
   - Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.

     ![Konfigurieren von Ziel-Verbindungszeichenfolge](build-restful-apis-with-aspnet-web-api/_static/image55.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.

    ![Erstellen der Datenbank](build-restful-apis-with-aspnet-web-api/_static/image56.png "erstellen die datenbankzeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Windows Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Auf der SQL-Datenbank-Verbindungszeichenfolge](build-restful-apis-with-aspnet-web-api/_static/image57.png "auf SQL-Datenbank-Verbindungszeichenfolge")

    *Auf der SQL-Datenbank-Verbindungszeichenfolge*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image58.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.

    ![Anwendung auf Windows Azure veröffentlicht](build-restful-apis-with-aspnet-web-api/_static/image59.png "Anwendung auf Windows Azure veröffentlicht")

    *Anwendung in Azure veröffentlicht*
