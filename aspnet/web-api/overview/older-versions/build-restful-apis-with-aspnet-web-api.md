---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Erstellen von RESTful-APIs mit ASP.NET Web-API | Microsoft Docs
author: rick-anderson
description: In den vergangenen Jahren hat es klar sein, dass HTTP nicht ist nur für HTML-Seiten bedient. Es ist auch eine leistungsstarke Plattform zum Erstellen von Web-APIs, die mit einer Handvoll o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: cb02288e93be801a1e55852741ed1443d8d3617d
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306883"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Erstellen von RESTful-APIs mit ASP.NET Web-API
====================
Durch [Web Lager Team](https://twitter.com/webcamps)

> In den vergangenen Jahren hat es klar sein, dass HTTP nicht ist nur für HTML-Seiten bedient. Es ist auch eine leistungsstarke Plattform zum Erstellen von Web-APIs, mit ein paar Verben (GET, POST usw.) sowie einige einfache Konzepte wie *URIs* und *Header*. ASP.NET-Web-API ist eine Reihe von Komponenten, die HTTP-Programmiermodell zu vereinfachen. Da es auf Grundlage der ASP.NET MVC-Laufzeit erstellt wurde, verarbeitet Web-API automatisch die Transportdetails auf niedriger Ebene HTTP. Zur gleichen Zeit macht das HTTP-Programmiermodell natürlich von Web-API verfügbar. Tatsächlich ist eines der Ziele von Web-API zum *nicht* Abstraktion für die Verwendung von HTTP. Deshalb ist Web-API flexibel und einfach zu erweitern. In dieser praktischen Übungseinheit verwenden Sie die Web-API, um eine einfache REST-API für einen Kontakt-Manager-Anwendung zu erstellen. Erstellen Sie auch einen Client für die API nutzen. Architektonische REST-Stil hat eine effektive Möglichkeit für die HTTP - genutzt werden zwar sicherlich nicht der einzige gültige Ansatz in HTTP-erwiesen. Der Kontakte-Manager wird die RESTful zum Auflisten, hinzufügen und Entfernen von Kontakten, u. a. verfügbar machen. In dieser Umgebung erfordert ein grundlegendes Verständnis von HTTP, REST, und setzt voraus, dass Sie über grundlegende Kenntnisse von HTML, JavaScript und jQuery haben.
> 
> > [!NOTE]
> > Die ASP.NET-Website verfügt über einen speziellen Bereich für die ASP.NET Web API-Framework unter [ https://asp.net/web-api ](https://asp.net/web-api). Dieser Standort wird weiterhin bereitstellen, hochaktuelle Informationen, Beispiele und Nachrichten, die im Zusammenhang mit der Web-API so checken Sie es häufig, wenn Sie tiefer in die Kunst zum Erstellen von benutzerdefinierten Web-APIs auf praktisch jedem Gerät oder einer Entwicklungsumgebung Framework vertiefen möchten.
> > 
> > ASP.NET Web-API, ähnlich wie in ASP.NET MVC 4 hat hohe Flexibilität im Hinblick auf die Dienstebene trennen, durch den Controller, und Sie können einige der verfügbaren Abhängigkeitsinjektion Frameworks relativ einfach zu verwenden. Es ist ein gutes Beispiel in MSDN, die zeigt, wie Ninject für Abhängigkeitsinjektion in ASP.NET Web-API-Projekt, das Sie ihn von herunterladen können [hier](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Implementieren Sie eine RESTful-Web-API
- Aufrufen der API aus einem HTML-client

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit:

- [Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang B](#AppendixB) Anleitungen zur Installation).

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang A: Verwenden von Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit enthält in der folgenden Übung:

1. [Übung 1: Erstellen einer nur-Lese Web-API](#Exercise1)
2. [Übung 2: Erstellen einer Lese-/Schreibzugriff-Webs-API](#Exercise2)
3. [Übung 3: Verwenden der Webs-API aus einem HTML-Client](#Exercise3)

> [!NOTE]
> Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.


Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Übung 1: Erstellen einer nur-Lese Web-API

In dieser Übung wird die nur-Lese GET-Methoden für den Kontakt-Manager implementiert werden.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Aufgabe 1: Erstellen des Projekts-API

In dieser Aufgabe verwenden Sie die neue ASP.NET Web-Projektvorlagen zum Erstellen einer Web-API-Webanwendung.

1. Führen Sie **zuerst Visual Studio 2012 Express für Web**, wechseln Sie zu diesem möchten **starten** und Typ **Visual Studio Express für Web** drücken Sie dann die **EINGABETASTE**.
2. Aus der **Datei** klicken Sie im Menü **neues Projekt**. Wählen Sie die **Visual C# | Web** Projekttyp aus der Strukturansicht der Projekt-Typ aus, und wählen Sie dann die **ASP.NET MVC 4-Webanwendung** Projekttyp. Legen Sie das Projekt **Namen** auf *ContactManager* und **Projektmappenname** auf *beginnen*, klicken Sie dann auf **OK**.

    ![Erstellen eine neue ASP.NET MVC 4.0 Webanwendungsprojekt](build-restful-apis-with-aspnet-web-api/_static/image1.png "Erstellen eines neuen ASP.NET MVC 4.0 Web-Anwendungsprojekts")

    *Erstellen eines neuen ASP.NET MVC 4.0 Web-Anwendungsprojekts*
3. Wählen Sie im Dialogfeld für den Typ ASP.NET MVC 4-Projekt die **Web-API** Projekttyp. Klicken Sie auf **OK**.

    ![Angeben den Web-API-Projekttyp](build-restful-apis-with-aspnet-web-api/_static/image2.png "Angabe des Typs der Web-API-Projekt")

    *Angeben den Web-API-Projekttyp*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Aufgabe 2: erstellen die Kontakt-Manager-API-Controller

In dieser Aufgabe erstellen Sie die Controllerklassen in denen API-Methoden gespeichert werden soll.

1. Löschen Sie die Datei mit dem Namen **ValuesController.cs** in **Controller** Ordner aus dem Projekt.
2. Mit der rechten Maustaste die **Controller** Ordner im Projekt, und wählen Sie **hinzufügen | Controller** aus dem Kontextmenü.

    ![Hinzufügen eines neuen Controllers zum Projekt](build-restful-apis-with-aspnet-web-api/_static/image3.png "Hinzufügen eines neuen Controllers zum Projekt")

    *Hinzufügen eines neuen Controllers zum Projekt*
3. In der **Controller hinzufügen** Dialogfeld, das angezeigt wird, wählen Sie **leerer API-Controller** aus dem Menü Vorlage. Benennen Sie die Controllerklasse **ContactController**. Klicken Sie auf **hinzufügen.**

    ![Verwenden das Dialogfeld "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers](build-restful-apis-with-aspnet-web-api/_static/image4.png "verwenden das Dialogfeld \"Controller hinzufügen\" zum Erstellen eines neuen Web-API-Controllers")

    *Verwenden das Dialogfeld "Controller hinzufügen" zum Erstellen eines neuen Web-API-Controllers*
4. Fügen Sie folgenden Code, der **ContactController**.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Get-API-Methode*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Drücken Sie **F5** zum Debuggen der Anwendung. Die Standardstartseite für eine Web-API-Projekt sollte angezeigt werden.

    ![Die Standardstartseite einer ASP.NET Web-API-Anwendung](build-restful-apis-with-aspnet-web-api/_static/image5.png "die Standardstartseite einer ASP.NET Web-API-Anwendung")

    *Die Standardstartseite einer ASP.NET Web-API-Anwendung*
6. Drücken Sie in Internet Explorer-Fenster die **F12** Schlüssel zum Öffnen der **Entwicklertools** Fenster. Klicken Sie auf die **Netzwerk** Registerkarte, und klicken Sie dann auf die **starten erfassen** zu beginnen, Erfassen von Netzwerkdatenverkehr in das Fenster.

    ![Öffnen die Registerkarte "Netzwerk" und das Initiieren von netzwerkerfassung](build-restful-apis-with-aspnet-web-api/_static/image6.png "öffnen die Registerkarte \"Netzwerk\" und das Initiieren von netzwerkerfassung")

    *Öffnen die Registerkarte "Netzwerk" und das Initiieren von netzwerkerfassung*
7. Fügen Sie die URL in die Adressleiste des Browsers, mit **/api/Kontakte** und drücken Sie die EINGABETASTE. Die Übertragung wird im Fenster erfassen Netzwerks angezeigt werden. Beachten Sie, dass die Antwort MIME-Typ **Application/Json**. Dies zeigt, wie das Standardausgabeformat JSON ist.

    ![Anzeigen der Ausgabe der Web-API-Anforderung in der Netzwerkansicht](build-restful-apis-with-aspnet-web-api/_static/image7.png "die Ausgabe der Web-API-Anforderung in der Ansicht anzeigen")

    *Die Ausgabe der Web-API-Anforderung anzeigen in der Ansicht*

    > [!NOTE]
    > Internet Explorer 10-Standardverhalten werden an diesem Punkt gefragt, ob der Benutzer, speichern oder den Stream, aus dem Web-API-Aufruf resultierende öffnen möchten. Die Ausgabe wird eine Textdatei mit dem JSON-Ergebnis des Aufrufs Web-API-URL sein. Brechen Sie das Dialogfeld "nicht um Antwortinhalt durch Entwickler Toolfenster beobachten können.
8. Klicken Sie auf die **wechseln Sie zur detaillierten Ansicht** Schaltfläche, um weitere Details über die API-Aufrufs Antwort anzuzeigen.

    ![Wechseln Sie zur detaillierten Ansicht](build-restful-apis-with-aspnet-web-api/_static/image8.png "wechseln Sie in der Detailansicht")

    *Wechseln Sie in der Detailansicht*
9. Klicken Sie auf die **Antworttext** Registerkarte, um den tatsächlichen Text der JSON-Antwort anzuzeigen.

    ![Anzeigen von JSON output Text in den Netzwerkmonitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Text in den Netzwerkmonitor Ausgabe anzeigen von JSON")

    *Den Text der JSON-Ausgabe anzeigen in die Netzwerkmonitor*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Aufgabe 3 - Kontakt Modelle erstellen, und erweitern Sie den Kontakten Controller

In dieser Aufgabe erstellen Sie die Controllerklassen in denen API-Methoden gespeichert werden soll.

1. Mit der rechten Maustaste die **Modelle** Ordner, und wählen **hinzufügen | Klasse...**  aus dem Kontextmenü.

    ![Hinzufügen eines neuen Modells an die Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image10.png "der Webanwendung ein neues Modell hinzufügen")

    *Hinzufügen eines neuen Modells an die Web-Anwendung*
2. In der **neues Element hinzufügen** Dialogfeld, benennen Sie die neue Datei **Contact.cs** , und klicken Sie auf **hinzufügen.**

    ![Erstellen die neue Klassendatei Kontakt](build-restful-apis-with-aspnet-web-api/_static/image11.png "die neue Klassendatei Kontakt erstellen")

    *Die neue Klassendatei Kontakt erstellen*
3. Fügen Sie den folgenden hervorgehobenen Code in die **Kontakt** Klasse.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Kontakt Klasse*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. In der **ContactController** Klasse, wählen Sie das Wort **Zeichenfolge** in der Definition des der **abrufen** -Methode, und geben Sie das Wort *Kontakt*. Sobald das Wort im eingegeben wurde, erscheint ein Indikator am Anfang des Worts **Kontakt**. Entweder halten Sie die **STRG** -Taste und drücken Sie die Taste Punkt (.) oder klicken Sie auf das Symbol mit der Maus, um das Dialogfeld "Hilfe" im Code-Editor automatisch ausfüllen zu öffnen die **mit** Richtlinie für die Modelle Namespace.

    ![Verwenden von Intellisense-Unterstützung für Namespacedeklarationen](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Verwenden von Intellisense-Unterstützung für Namespacedeklarationen*
5. Ändern Sie den Code für die **abrufen** Methode, sodass diese ein Array von Instanzen der Kontakt-Modell zurückgegeben.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Zurückgeben einer Liste von Kontakten*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Drücken Sie **F5** zum Debuggen der Webanwendung im Browser. Um die an die antwortausgabe der API vorgenommenen Änderungen anzuzeigen, führen Sie die folgenden Schritte aus.

   1. Nachdem der Browser geöffnet wird, drücken Sie **F12** , wenn die Entwicklertools noch nicht geöffnet sind.
   2. Klicken Sie auf die **Netzwerk** Registerkarte.
   3. Drücken Sie die **starten erfassen** Schaltfläche.
   4. Fügen Sie die URL-Suffix **/api/Kontakt** an die URL in die Adressleiste ein und drücken Sie die **EINGABETASTE** Schlüssel.
   5. Drücken Sie die **wechseln Sie zur detaillierten Ansicht** Schaltfläche.
   6. Wählen Sie die **Antworttext** Registerkarte. Daraufhin sollte eine JSON-Zeichenfolge, die die serialisierte Form eines Arrays von Kontakt Instanzen darstellt.

      ![JSON-serialisierten Ausgabe eines komplexen Web-API-Methodenaufrufs](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialisierten Ausgabe des einen komplexen Web-API-Methodenaufruf")

      *JSON-serialisierten Ausgabe des einen komplexen Web-API-Methodenaufruf*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Aufgabe 4 - extrahieren Funktionalität in einer Dienstebene

Diese Aufgabe zeigen, wie Funktionen in einer Dienstebene zu erleichtern Entwicklern, trennen ihre Service-Funktionalität von der Controller-Ebene, wodurch die Wiederverwendung der Dienste, die die Verarbeitung durch erfolgt extrahieren.

1. Erstellen Sie einen neuen Ordner im Projektmappen-Stammverzeichnis und nennen sie **Services**. Zu diesem Zweck Maustaste **ContactManager** -Projekt, wählen **hinzufügen** | **neuer Ordner**, nennen Sie sie *Services*.

    ![Erstellen der Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image14.png "Ordner \"Dienste erstellen\"")

    *Erstellen Ordner "Dienste"*
2. Mit der rechten Maustaste die **Services** Ordner, und wählen **hinzufügen | Klasse...**  aus dem Kontextmenü.

    ![Eine neue Klasse hinzufügen, um den Ordner "Dienste"](build-restful-apis-with-aspnet-web-api/_static/image15.png "eine neue Klasse hinzufügen, um den Ordner \"Dienste\"")

    *Eine neue Klasse hinzufügen, um den Ordner "Dienste"*
3. Wenn die **neues Element hinzufügen** Dialogfeld angezeigt wird, benennen Sie die neue Klasse **ContactRepository** , und klicken Sie auf **hinzufügen**.

    ![Erstellen eine Klassendatei, um den Code für die Dienstebene Kontakt Repository enthalten](build-restful-apis-with-aspnet-web-api/_static/image16.png "erstellen eine Klassendatei, um den Code für die Dienstebene Kontakt Repository enthalten")

    *Erstellen eine Klassendatei, um den Code für die Dienstebene Kontakt Repository enthalten*
4. Fügen Sie eine using-Direktive hinzu der **ContactRepository.cs** Datei, die den Modellen-Namespace enthalten.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Fügen Sie den folgenden hervorgehobenen Code in die **ContactRepository.cs** Datei GetAllContacts Methode implementiert.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Kontaktrepository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Öffnen Sie die **ContactController.cs** Datei, sofern dieser nicht bereits geöffnet ist.
7. Fügen Sie die folgende Anweisung verwenden, mit dem Namespace-Deklaration-Abschnitt der Datei.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Fügen Sie den folgenden hervorgehobenen Code in die **ContactController.cs** Klasse, um ein privates Feld für die Darstellung der Instanz des Repositorys, hinzuzufügen, sodass der Rest der Klasse, die Mitglieder ausführen können mithilfe der dienstimplementierung.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Kontakt Controller*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Ändern der **abrufen** Methode, sodass die It stellt den Kontaktrepository-Dienst verwenden.

    (Codeausschnitt - *Web-API-Lab - Ex01 - Zurückgeben einer Liste von Kontakten über das Repository*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Fügen Sie einen Haltepunkt in der **ContactController**des **abrufen** Methodendefinition.

   ![Haltepunkte hinzufügen, mit dem Kontakt Controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Haltepunkte mit dem Kontakt Controller hinzufügen")

   *Haltepunkte mit dem Kontakt Controller hinzufügen*
11. Drücken Sie **F5**, um die Anwendung auszuführen.
12. Wenn der Browser geöffnet wird, drücken Sie die **F12** zu den Entwicklertools zu öffnen.
13. Klicken Sie auf die **Netzwerk** Registerkarte.
14. Klicken Sie auf die **starten erfassen** Schaltfläche.
15. Fügen Sie die URL in die Adressleiste ein, mit dem Suffix **/api/Kontakt** , und drücken Sie **EINGABETASTE** beim Laden der API-Controller.
16. Visual Studio 2012 sollte einmal unterbrechen **abrufen** Methode startet die Ausführung.

   ![Innerhalb der Methode Get wichtige](build-restful-apis-with-aspnet-web-api/_static/image18.png "wichtige innerhalb der Get-Methode")

   *Wichtige in der Get-Methode*
17. Drücken Sie **F5**, um fortzufahren.
18. Wechseln Sie zurück in Internet Explorer, wenn es nicht bereits im Fokus ist. Beachten Sie das Fenster "Netzwerk Capture" aus.

    ![Netzwerk-Ansicht in Internet Explorer, dabei werden die Ergebnisse des Web-API-Aufrufs](build-restful-apis-with-aspnet-web-api/_static/image19.png "Netzwerk in Internet Explorer, dabei werden die Ergebnisse des Web-API-Aufrufs anzeigen")

    *Ansicht in Internet Explorer, dabei werden die Ergebnisse der Web-API-Aufruf*
19. Klicken Sie auf die **wechseln Sie zur detaillierten Ansicht** Schaltfläche.
20. Klicken Sie auf die **Antworttext** Registerkarte. Beachten Sie die JSON-Ausgabe der API-Aufruf, und wie die beiden Kontakte abgerufen, indem die Dienstebene dar.

    ![Anzeigen von JSON-Ausgabe aus der Web-API in das Fenster "Developer"](build-restful-apis-with-aspnet-web-api/_static/image20.png "die JSON-Ausgabe aus der Web-API in das Fenster \"Developer\" anzeigen")

    *Die JSON-Ausgabe aus der Web-API anzeigen in das Fenster "Developer"*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Übung 2: Erstellen einer Lese-/Schreibzugriff-Webs-API

In dieser Übung implementieren Sie POST und PUT-Methoden für den Kontakt-Manager zum Bearbeiten von Daten von Funktionen zu aktivieren.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Aufgabe 1: Öffnen das Web-API-Projekt

In dieser Aufgabe bereiten Sie verbessern die Web-API-Projekt in Übung 1 erstellt werden, sodass sie Benutzereingaben akzeptieren kann.

1. Führen Sie **zuerst Visual Studio 2012 Express für Web**, wechseln Sie zu diesem möchten **starten** und Typ **Visual Studio Express für Web** drücken Sie dann die **EINGABETASTE**.
2. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex02-ReadWriteWebAPI/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
3. Öffnen der **Services/ContactRepository.cs** Datei.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Aufgabe 2: Hinzufügen von Datenpersistenz Funktionen für die Implementierung Kontaktrepository

In dieser Aufgabe wird die ContactRepository-Klasse, der das Web-API-Projekt, die in Übung 1 erstellt haben, damit es beibehalten und von Benutzereingaben und neuer Kontakt-Instanzen annehmen kann erweitert werden.

1. Die folgende Konstante zum Hinzufügen der **ContactRepository** Klasse, um den Namen des Cache Element Schlüssel den Namen des Webservers weiter unten in dieser Übung darzustellen.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Fügen Sie einen Konstruktor, um die **ContactRepository** mit dem folgenden Code.

    (Codeausschnitt - *Web-API-Lab - Ex02 - Kontaktrepository Konstruktor*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Ändern Sie den Code für die **GetAllContacts** Methode wie nachstehend gezeigt.

    (Codeausschnitt - *Web-API-Lab - Ex02 - Abrufen aller Kontakte*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > In diesem Beispiel wird zu Demonstrationszwecken und verwendet die Webserver Cache als einem Speichermedium, damit die Werte werden gleichzeitig an mehrere Clients verfügbar sein, anstatt eine Sitzung Speichermechanismus oder eine Anforderung Speicher Lebensdauer verwenden. Eine konnte Entity Framework, XML-Speicherung oder anderen verschiedener anstelle der Cache des Webservers verwenden.
4. Implementieren Sie eine neue Methode mit dem Namen **SaveContact** auf die **ContactRepository** Klasse zum Durchführen der Aufgaben ein Kontakts zu speichern. Die **SaveContact** -Methode sollte ein einzelnes annehmen **Kontakt** Parameter- und Rückgabetypen ein boolescher Wert, Erfolg oder Fehler angibt.

    (Codeausschnitt - *Web-API-Lab - Ex02 - Implementierung der SaveContact-Methode*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Übung 3: Verwenden der Webs-API aus einem HTML-Client

In dieser Übung erstellen Sie einen HTML-Client, um die Web-API aufzurufen. Dieser Client wird Datenaustausch mit der Web-API, die mit JavaScript vereinfachen und zeigt die Ergebnisse in einem Webbrowser mit HTML-Markup.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Aufgabe 1 – ändern die Indexansicht, um eine GUI bereitstellen, für die Anzeige von Kontakten

In dieser Aufgabe ändern Sie die Standardansicht des Index der Webanwendung, die die Anforderung zum Anzeigen von der Liste der existierenden Kontakte in einem HTML-Browser zu unterstützen.

1. Öffnen Sie **zuerst Visual Studio 2012 Express für Web** , wenn er nicht bereits geöffnet ist.
2. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex03-ConsumingWebAPI/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
3. Öffnen der **Index.cshtml** Datei **Ansichten/Start** Ordner.
4. Ersetzen Sie den HTML-Code innerhalb des Div-Elements mit der Id **Text** , damit sie den folgenden Code ähnelt.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Fügen Sie den folgenden Javascript-Code am Ende der Datei zum Ausführen der HTTP-Anforderung an die Web-API.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Öffnen Sie die **ContactController.cs** Datei, sofern dieser nicht bereits geöffnet ist.
7. Fügen Sie einen Haltepunkt auf der **abrufen** Methode der **ContactController** Klasse.

    ![Platzieren einen Haltepunkt für die Get-Methode von der API-Controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "einen Haltepunkt einfügen, auf die Get-Methode von der API-Controller")

    *Platzieren einen Haltepunkt für die Get-Methode von der API-controller*
8. Drücken Sie **F5** um das Projekt auszuführen. Das HTML-Dokument wird im Browser geladen werden.

    > [!NOTE]
    > Stellen Sie sicher, dass Sie an die Stamm-URL Ihrer Anwendung durchsuchen möchten.
9. Nach dem Laden der Seite, und der JavaScript-Code ausführt, wird der Haltepunkt erreicht, und hält die Ausführung des Codes im Controller.

    ![Debuggen in der Web-API-Aufrufe, die mit Visual Studio Express für Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debuggen in der Web-API-Aufrufe, die mit Visual Studio Express für Web")

    *Debuggen in der Web-API-Aufruf mithilfe von Visual Studio 2012 Express für Web*
10. Entfernen Sie den Haltepunkt, und drücken Sie **F5** oder der Symbolleiste Debuggen **Fortfahren** um den Vorgang fortzusetzen, laden die Ansicht im Browser. Nach Abschluss der Web-API-Aufruf sollte aus der Web-API zurückgegebenen Kontakte aufrufen, dargestellt als Listenelemente im Browser angezeigt werden.

    ![Ergebnisse von der API-Aufruf im Browser angezeigt wird, als Listenelemente](build-restful-apis-with-aspnet-web-api/_static/image23.png "Ergebnisse dem API-Aufruf im Browser angezeigt wird, als Listenelemente")

    *Ergebnisse von der API-Aufruf im Browser angezeigt wird, als Listenelemente*
11. Beenden des Debuggens.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Aufgabe 2: ändern die Indexansicht, um eine GUI zu bieten, zum Erstellen von Kontakten

In dieser Aufgabe werden Sie weiterhin die Indexansicht MVC-Anwendung zu ändern. Die HTML-Seite, die Benutzereingaben erfassen und senden sie an die Web-API zum Erstellen eines neuen Kontakts wird ein Formular hinzugefügt werden, und eine neue Web-API-Controller-Methode wird erstellt, um das Datum aus der GUI zu sammeln.

1. Öffnen der **ContactController.cs** Datei.
2. Fügen Sie eine neue Methode für die Controllerklasse, die mit dem Namen **Post** wie im folgenden Code gezeigt.

    (Codeausschnitt - *Web-API-Lab - Ex03 - Post-Methode*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Öffnen Sie die **Index.cshtml** Datei in Visual Studio, wenn er nicht bereits geöffnet ist.
4. Fügen Sie den folgenden HTML-Code in die Datei direkt hinter dem ungeordnete Liste, die Sie in der vorherigen Aufgabe hinzugefügt.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. Fügen Sie in das Script-Element am unteren Rand des Dokuments um Schaltfläche – Click-Ereignisse zu behandeln, die die Daten an die Web-API bereitstellen werden, über eine HTTP POST-Aufruf die folgenden hervorgehobenen Code hinzu.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. In **ContactController.cs**, fügen Sie einen Haltepunkt auf der **Post** Methode.
7. Drücken Sie **F5** zum Ausführen der Anwendung im Browser.
8. Sobald die Seite im Browser geladen wird, geben Sie in einem neuen Kontaktnamen und -Id und klicken Sie auf die **speichern** Schaltfläche.

    ![Das Client-HTML-Dokument im Browser geladen](build-restful-apis-with-aspnet-web-api/_static/image24.png "das Client-HTML-Dokument im Browser geladen.")

    *Die Client-HTML-Dokument im Browser geladen.*
9. Wenn im Debuggerfenster Zeilenumbrüche, in der **Post** -Methode, erstellen Sie eine Blick auf die Eigenschaften des der **wenden Sie sich an** Parameter. Die Werte übereinstimmen, die Daten, die Sie in das Formular eingegeben haben.

    ![Die Kontakt-Objekt, das an die Web-API vom Client gesendeten](build-restful-apis-with-aspnet-web-api/_static/image25.png "die Kontakt-Objekt, die an die Web-API vom Client gesendet werden")

    *Die Kontakt-Objekt, das an die Web-API vom Client gesendeten*
10. Durchlaufen Sie die Methode im Debugger bis der **Antwort** Variable erstellt wurde. Bei der Überprüfung in der **"lokal"** Fenster im Debugger, werden Sie feststellen, dass alle Eigenschaften festgelegt wurden.

   ![Die Antwort nach der Erstellung im Debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "die Antwort nach der Erstellung im Debugger")

   *Die Antwort nach der Erstellung im debugger*
11. Wenn Sie drücken **F5** oder klicken Sie auf **Fortfahren** im Debugger die Anforderung abgeschlossen wird. Sobald Sie wieder an den Browser wechseln, der neue Kontakt hinzugefügt wurde der Liste der Kontakte, die vom gespeicherten der **ContactRepository** Implementierung.

   ![Der Browser gibt bei erfolgreicher Erstellung des neuen Kontakts Instanz wieder](build-restful-apis-with-aspnet-web-api/_static/image27.png "der Browser gibt bei erfolgreicher Erstellung des neuen Kontakts Instanz wieder")

   *Der Browser gibt bei erfolgreicher Erstellung des neuen Kontakts Instanz wieder*

> [!NOTE]
> Darüber hinaus können Sie die Bereitstellung dieser Anwendung auf Azure Folgendes [Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser Umgebung wurde das neue ASP.NET Web API-Framework und die Implementierung von RESTful Web-APIs, die mithilfe des Frameworks eingeführt. Von hier aus können Sie ein neues Repository, das die Dauerhaftigkeit von Daten, die eine beliebige Anzahl von Mechanismen vereinfacht erstellen und verknüpfen diesen Dienst statt der einfachen eine in dieser Übungseinheit als Beispiel bereitgestellt. Web-API unterstützt eine Reihe zusätzlicher Funktionen, z. B. das Aktivieren der Kommunikation von nicht-HTML-Clients, die in einer beliebigen Sprache, die von HTTP und JSON oder XML unterstützt geschrieben wurden. Die Möglichkeit, eine Web-API außerhalb einer typischen Webanwendung gehostet ist auch möglich, als auch ist die Möglichkeit, eigene Serialisierungsformate erstellen.

Die ASP.NET-Website verfügt über einen speziellen Bereich für die ASP.NET Web API-Framework unter [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Dieser Standort wird weiterhin bereitstellen, hochaktuelle Informationen, Beispiele und Nachrichten, die im Zusammenhang mit der Web-API so checken Sie es häufig, wenn Sie tiefer in die Kunst zum Erstellen von benutzerdefinierten Web-APIs auf praktisch jedem Gerät oder einer Entwicklungsumgebung Framework vertiefen möchten.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Anhang A: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen. Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.

![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](build-restful-apis-with-aspnet-web-api/_static/image28.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")

*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)

1. Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.
2. Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.
4. Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.

    ![Geben Sie den Namen des Ausschnitts](build-restful-apis-with-aspnet-web-api/_static/image29.png "starten Sie den Namen des Ausschnitts eingeben")

    *Starten Sie den Namen des Ausschnitts eingeben*

    ![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](build-restful-apis-with-aspnet-web-api/_static/image30.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")

    *Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*

    ![Drücken Sie erneut die Tab und den Ausschnitt erweitert](build-restful-apis-with-aspnet-web-api/_static/image31.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")

    *Drücken Sie erneut die Tab und den Ausschnitt erweitert*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)

1. Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.
2. Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.
3. Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.

    ![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](build-restful-apis-with-aspnet-web-api/_static/image32.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

    *Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

    ![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](build-restful-apis-with-aspnet-web-api/_static/image33.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")

    *Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Anhang B: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](build-restful-apis-with-aspnet-web-api/_static/image34.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang C: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

In diesem Anhang wird gezeigt, wie eine neue Website aus dem Azure-Portal erstellen und Veröffentlichen der Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Azure bereitgestellt werden.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Azure-Portal

1. Wechseln Sie zu der [Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Sie können mit Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich beim Portal*
2. Klicken Sie auf **neu** in der Befehlszeile.

    ![Erstellen einer neuen Website](build-restful-apis-with-aspnet-web-api/_static/image40.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **berechnen** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Azure ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können. Die Option Schnellerfassung können Sie eine vollständige Webanwendung in Azure von außerhalb des Portals bereitstellen. Es umfasst nicht die Schritte zum Einrichten einer Datenbank.

    ![Erstellen eine neue Website, die mit der Schnellerfassung](build-restful-apis-with-aspnet-web-api/_static/image41.png "erstellen eine neue Website, die mit der Schnellerfassung")

    *Erstellen eine neue Website, die mit der Schnellerfassung*
4. Warten Sie, bis die neue **Website** wird erstellt.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website funktioniert.

    ![Um die neue Website durchsuchen](build-restful-apis-with-aspnet-web-api/_static/image42.png "durchsuchen, um die neue Website")

    *Um die neue Website durchsuchen*

    ![Website](build-restful-apis-with-aspnet-web-api/_static/image43.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](build-restful-apis-with-aspnet-web-api/_static/image44.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einem von Azure für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung beim Lesen von veröffentlichungsprofilen zum Automatisieren der Konfiguration dieser Programme für Veröffentlichung von Webanwendungen in Azure.

    ![Herunterladen der Website-Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image45.png "der Website herunterladen eines Veröffentlichungsprofils")

    *Die Website herunterladen eines Veröffentlichungsprofils*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein. In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung in Azure aus Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](build-restful-apis-with-aspnet-web-api/_static/image46.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: konfigurieren den Datenbankserver

Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen. Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sehen Sie die SQL-Datenbankservern über Ihr Abonnement im Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **Dashboard des Servers**. Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.

    ![SQL-Datenbank-Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL-Datenbank-Dashboard")

    *SQL-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**. Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Bestätigen von Änderungen*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

1. Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](build-restful-apis-with-aspnet-web-api/_static/image51.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren des Veröffentlichungsprofils](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.

    ![Überprüfen der Verbindung](build-restful-apis-with-aspnet-web-api/_static/image53.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.
   - In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.
   - In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.
   - Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.

     ![Konfigurieren von Zielverbindungszeichenfolge](build-restful-apis-with-aspnet-web-api/_static/image55.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.

    ![Erstellen der Datenbank](build-restful-apis-with-aspnet-web-api/_static/image56.png "erstellen die Datenbank-Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungszeichenfolge zur SQL-Datenbank zeigt](build-restful-apis-with-aspnet-web-api/_static/image57.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")

    *Verbindungszeichenfolge, die auf der SQL-Datenbank*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](build-restful-apis-with-aspnet-web-api/_static/image58.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.

    ![Anwendung in Windows Azure veröffentlicht](build-restful-apis-with-aspnet-web-api/_static/image59.png "Veröffentlichung der Anwendung in Windows Azure")

    *Anwendung in Azure veröffentlicht*
