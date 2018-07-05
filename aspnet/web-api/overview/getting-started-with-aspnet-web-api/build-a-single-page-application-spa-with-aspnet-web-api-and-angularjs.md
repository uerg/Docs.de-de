---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Praxisnahe Übung: Erstellen eine Einzelseitenanwendung (SPA) mit ASP.NET-Web-API und Angular.js | Microsoft-Dokumentation'
author: rick-anderson
description: In herkömmlichen Webanwendungen initiiert der Client (Browser) die Kommunikation mit dem Server durch Anfordern einer Seite an. Der Server, klicken Sie dann die Anforderung verarbeitet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 174084d6923cf1fa445485b7c0dc639a240720a5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370862"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Praxisnahe Übung: Erstellen einer Einzelseitenanwendung (SPA) mit ASP.NET-Web-API und Angular.js
====================
durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](http://aka.ms/webcamps-training-kit)

> In herkömmlichen Webanwendungen initiiert der Client (Browser) die Kommunikation mit dem Server durch Anfordern einer Seite an. Der Server verarbeitet die Anforderung dann und sendet den HTML-Code der Seite an den Client. In nachfolgenden Interaktionen mit der Seite – z. B. der Benutzer navigiert zu einem Link oder ein Formular mit Daten übermittelt werden – eine neue Anforderung an den Server gesendet wird, und der Flow wird neu gestartet: der Server verarbeitet die Anforderung und sendet Sie eine neue Seite, an den Browser als Reaktion auf die neue aktionsanforderung ED vom Client.
> 
> In Single-Page-Webanwendungen (SPAs) die gesamte Seite im Browser nach der ersten Anforderung geladen wird, aber die nachfolgende Interaktionen aufrechterhalten erfolgen über Ajax-Anforderungen. Dies bedeutet, dass der Browser nur den Teil der Seite zu aktualisieren, die geändert wurden; besteht keine Notwendigkeit, die gesamte Seite neu zu laden. Der SPA-Ansatz reduziert die Zeit, die von der Anwendung auf Benutzeraktionen, wodurch eine flexiblere Benutzeroberfläche reagiert.
> 
> Die Architektur einer SPA umfasst bestimmte Probleme bestehen, die nicht in herkömmliche Webanwendungen vorhanden sind. Allerdings Erweitern von Technologien wie ASP.NET Web-API, JavaScript-Frameworks wie AngularJS, und neue Stile Funktionen von CSS3 realisieren einer wirklich einfachen entwerfen und Erstellen von SPAs.
> 
> In dieser Übungseinheit Hand-on-werden Sie diese Technologien implementieren Meister Quiz, eine Trivia-Website auf der Grundlage der SPA-Konzepts nutzen. Implementieren Sie zuerst die Dienstschicht mit ASP.NET Web-API verfügbar machen die erforderlichen Endpunkte für die Quizfragen abzurufen und die Antworten zu speichern. Anschließend erstellen Sie eine umfassende und reaktionsfähige Benutzeroberfläche, die mithilfe von AngularJS- und CSS3-Transformation-Effekten.
> 
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen eines ASP.NET Web-API-Diensts zum Senden und Empfangen von JSON-Daten
- Erstellen einer reaktionsfähigen Benutzeroberflächenautomatisierungs mithilfe von AngularJS
- Verbessern Sie die Benutzeroberfläche mit CSS3-Transformationen

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit auszuführen:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher

<a id="Setup"></a>
### <a name="setup"></a>Setup

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.

1. Öffnen von Windows-Explorer und navigieren Sie zu des Labs die **Quelle** Ordner.
2. Mit der rechten Maustaste auf **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** zum Starten des Setup-Prozesses, die die Umgebung konfigurieren, und installieren die Visual Studio-Codeausschnitte für diese Übung.
3. Wenn das Dialogfeld "Benutzerkontensteuerung" angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.

> [!NOTE]
> Stellen Sie sicher, dass Sie alle Abhängigkeiten für diese laborumgebung aktiviert haben, bevor Sie das Setup ausführen.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden von Codeausschnitten

In diesem Dokument Lab werden Sie aufgefordert, zum Einfügen von Codeblöcken. Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitten, die Sie in Visual Studio 2013, um zu vermeiden, müssen sie manuell hinzufügen aus zugreifen können.

> [!NOTE]
> Jede Übung umfasst eine ab Lösung befindet sich in der **beginnen** Ordner der Übung, mit dem Sie jede Übung unabhängig von den anderen verfolgen kann. Bedenken Sie bitte, dass die Codeausschnitte, die während der Übung hinzugefügt werden fehlen aus diesen Lösungen ab und funktioniert möglicherweise nicht, bis Sie in dieser Übung abgeschlossen haben. In den Quellcode für eine Übung, finden Sie auch eine **End** Ordner, der Visual Studio-Projektmappe mit dem Code, die aus der Schritte in der entsprechenden Übung enthält. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Dieser praktischen Übungseinheit enthält die folgenden Übungen:

1. [Erstellen einer Webs-API](#Exercise1)
2. [Erstellen einer SPA-Schnittstelle](#Exercise2)

Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen. Jede vordefinierte Sammlung dient einem bestimmten Entwicklungsstil und bestimmt, Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Dialogfeld "Optionen". In dieser Übung wird beschrieben, die erforderlichen Aktionen zum Ausführen der jeweiligen Aufgabe in Visual Studio bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung. Wenn Sie eine Sammlung mit anderen Einstellungen für Ihre Entwicklungsumgebung auswählen, unter Umständen gibt es bestehen Unterschiede in den Schritten, die Sie berücksichtigen sollten.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Übung 1: Erstellen einer Webs-API

Einer der wichtigsten Bestandteile einer SPA ist die Dienstschicht. Er ist verantwortlich für die Verarbeitung von Ajax-Aufrufe, die von der Benutzeroberfläche und die Rückgabe von Daten als Antwort auf diesen Aufruf gesendet werden. In einem maschinenlesbaren Format um analysiert und vom Client verwendet werden, sollte die abgerufenen Daten angezeigt werden.

Die Web-API-Framework ist Teil der ASP.NET-Stapel und dient zum Implementieren von HTTP-Diensten, die in der Regel senden und Empfangen von JSON oder XML-formatierte Daten über eine RESTful-API erleichtern. In dieser Übung erstellen Sie die Website, um die Meister Quiz-Anwendung hostet, und klicken Sie dann die Back-End-Dienst verfügbar zu machen, und speichern die Test-Daten, die mithilfe von ASP.NET Web-API zu implementieren.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Aufgabe 1 – Erstellen des anfänglichen Projekts für die Geek-Quiz

In dieser Aufgabe starten Sie erstellen ein neues ASP.NET MVC-Projekt mit Unterstützung für ASP.NET Web-API auf der Grundlage der **One ASP.NET** Projekttyp, der mit Visual Studio enthalten ist. **Eine ASP.NET** vereinigt alle ASP.NET-Technologien und bietet Ihnen die Möglichkeit zum kombinieren, und passen sie nach Bedarf. Sie werden Entity Framework ViewModel-Klassen und die Datenbank Initializator einzufügende die Quizfragen hinzugefügt.

1. Open **Visual Studio Express 2013 für Web** , und wählen Sie **Datei | Neues Projekt...**  um eine neue Projektmappe zu starten.

    ![Erstellen eines neuen Projekts](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "Erstellen eines neuen Projekts")

    *Erstellen eines neuen Projekts*
2. In der **neues Projekt** wählen Sie im Dialogfeld **ASP.NET-Webanwendung** unter der **Visual c# | Web** Registerkarte. Stellen Sie sicher, dass **.NET Framework 4.5** wird ausgewählt, nennen Sie sie *GeekQuiz*, wählen Sie eine **Speicherort** , und klicken Sie auf **OK**.

    ![Erstellen eines neuen Projekts für die ASP.NET-Webanwendung](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "Erstellen eines neuen Projekts für die ASP.NET-Webanwendung")

    *Erstellen eines neuen Projekts für die ASP.NET-Webanwendung*
3. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **MVC** Vorlage, und wählen die **Web-API-** Option. Stellen Sie außerdem sicher, dass die **Authentifizierung** Option wird festgelegt, um **einzelne Benutzerkonten**. Klicken Sie auf **OK** um den Vorgang fortzusetzen.

    ![Erstellen eines neuen Projekts mit der MVC-Vorlage, einschließlich Web-API-Komponenten](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Erstellen eines neuen Projekts mit der MVC-Vorlage, einschließlich Web-API-Komponenten*
4. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Modelle** Ordner die **GeekQuiz** Projekt, und wählen **hinzufügen | Vorhandenes Element...** .

    ![Hinzufügen eines vorhandenen Elements](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "ein vorhandenes Element hinzufügen")

    *Hinzufügen eines vorhandenen Elements*
5. In der **vorhandenes Element hinzufügen** Dialogfeld Feld, navigieren Sie zu der **Quelle/Assets/Modelle** Ordner, und wählen Sie alle Dateien. Klicken Sie auf **Hinzufügen**.

    ![Hinzufügen der Model-Objekte](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "die Model-Objekte hinzufügen")

    *Die Model-Objekte hinzufügen*

    > [!NOTE]
    > Indem Sie diese Dateien hinzufügen, fügen Sie das Datenmodell, Entity Framework Datenbankkontext und der Datenbankinitialisierer für die Anwendung Meister Quiz hinzu.
    > 
    > **Entity Framework (EF)** ist ein Objektrelationaler Mapper (ORM), die Ihnen die Erstellung von Anwendungen für den Datenzugriff durch das Programmieren mit einem konzeptionellen Anwendungsmodell anstelle des Programmierens direkt mit einem Speicherschema ermöglicht. Weitere Informationen finden Sie Informationen zu Entity Framework [hier](../../../entity-framework.md).
    > 
    > Im folgenden finden eine Beschreibung der Klassen, die Sie gerade hinzugefügt haben:
    > 
    > - **TriviaOption:** stellt eine einzelne Option, eine Frage Quiz zugeordnet
    > - **TriviaQuestion:** stellt eine Frage Quiz, und macht die zugehörigen Optionen über die **Optionen** Eigenschaft
    > - **TriviaAnswer:** stellt die Option ausgewählt, die vom Benutzer als Reaktion auf eine Frage des Quiz
    > - **TriviaContext:** Entity Framework Datenbankkontext der Anwendung Meister Quiz darstellt. Diese Klasse wird von **DContext** und macht **"DbSet"** Eigenschaften, die Auflistungen der oben beschriebenen Entitäten darstellen.
    > - **TriviaDatabaseInitializer:** die Implementierung von den Entity Framework-Initialisierer für die **TriviaContext** -Klasse die erbt **"createDatabaseIfNotExists"**. Das Standardverhalten dieser Klasse zum Erstellen der Datenbank nur dann, wenn er nicht vorhanden ist, Einfügen von Entitäten angegeben der **Ausgangswert** Methode.
6. Öffnen der **"Global.asax.cs"** Datei, und fügen Sie die folgenden using-Anweisung.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Fügen Sie den folgenden Code am Anfang der **Anwendung\_starten** Methode zum Festlegen der **TriviaDatabaseInitializer** als den Datenbankinitialisierer.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Ändern der **Startseite** Controller zum Einschränken des Zugriffs auf authentifizierte Benutzer. Zu diesem Zweck öffnen Sie die **"HomeController.cs"** Datei innerhalb der **Controller** Ordner und Hinzufügen der **autorisieren** -Attribut auf die **HomeController**Definition der Klasse.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > Die **autorisieren** Filtern überprüft, ob der Benutzer authentifiziert ist. Wenn der Benutzer nicht authentifiziert ist, gibt es HTTP-Statuscode 401 (nicht autorisiert) ohne Aufrufen der Aktion zurück. Sie können den Filter Global auf Controllerebene und auf der Ebene der einzelnen Aktionen anwenden.
9. Sie werden nun das Layout von Webseiten und das branding anpassen. Zu diesem Zweck öffnen Sie die  **\_Layout.cshtml** Datei innerhalb der **Ansichten | Freigegebene** Ordner, und aktualisieren Sie den Inhalt des der **&lt;Titel&gt;** Element durch Ersetzen *My ASP.NET Application* mit *Meister Quiz* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Aktualisieren Sie in der gleichen Datei, durch das Entfernen der Navigationsleiste auf die *zu* und *wenden Sie sich an* Links und das Umbenennen der *Startseite* Verknüpfen mit *spielen*. Benennen Sie die *Anwendungsname* Verknüpfen mit *Meister Quiz*. Der HTML-Code für die Navigationsleiste sollte wie im folgenden Code aussehen.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Die Fußzeile der Layoutseite aktualisieren, indem Sie ersetzen *My ASP.NET Application* mit *Meister Quiz*. Ersetzen Sie den Inhalt zu diesem Zweck die **&lt;Fußzeile&gt;** -Element mit den folgenden hervorgehobenen Code.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Aufgabe 2: Erstellen der TriviaController-Web-API

In der vorherigen Aufgabe haben Sie die Anfangsstruktur der Meister Quiz-Webanwendung erstellt. Erstellen Sie jetzt einen einfache Web-API-Dienst, der mit dem Datenmodell Quiz interagiert und macht die folgenden Aktionen:

- **GET/api/Trivia**: Ruft die nächste Frage aus der Liste Quiz nach authentifiziertem Benutzer beantwortet werden.
- **POST/api/Trivia**: speichert die Quiz-Antwort, die durch den authentifizierten Benutzer angegeben.

Sie werden von Visual Studio bereitgestellten ASP.NET-Gerüstbau Tools verwenden, um die Baseline für die Web-API-Controller-Klasse zu erstellen.

1. Öffnen der **WebApiConfig.cs** Datei innerhalb der **App\_starten** Ordner. Diese Datei definiert die Konfiguration der Web-API-Dienst, z.B. wie Web-API-Controlleraktionen Routen zugeordnet werden.
2. Fügen Sie die folgenden using-Anweisung am Anfang der Datei.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Fügen Sie den folgenden hervorgehobenen Code in die **registrieren** Methode, um den Formatierer für die JSON-Daten abgerufen, indem die Web-API-Aktionsmethoden global zu konfigurieren.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > Die **CamelCasePropertyNamesContractResolver** konvertiert automatisch Eigenschaftennamen *Camel* Fall, die die allgemeine Konvention für Eigenschaftsnamen in JavaScript ist.
4. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Controller** Ordner die **GeekQuiz** Projekt, und wählen **hinzufügen | Neues Gerüstelement...** .

    ![Erstellen ein neues gerüstelement](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "erstellen ein neues gerüstelement")

    *Erstellen ein neues gerüstelement*
5. In der **Gerüst hinzufügen** Dialogfeld Feld, stellen Sie sicher, dass die **allgemeine** Knoten im linken Bereich ausgewählt ist. Wählen Sie dann die **Web API 2-Controller – leer** Vorlagen im mittleren Bereich, und auf **hinzufügen**.

    ![Auswählen der Web-API 2 Controller-leer-Vorlage](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Auswählen der Web-API 2 Controller-leer-Vorlage")

    *Auswählen der Web-API 2 Controller-leer-Vorlage*

    > [!NOTE]
    > **ASP.NET-Gerüstbau** ist ein Code-Generierung-Framework für Webanwendungen mit ASP.NET. Visual Studio 2013 umfasst die vorinstallierte Code-Generatoren für MVC und Web-API-Projekte. Sie sollten in Ihrem Projekt Gerüstbau verwenden, wenn Sie Code, der mit Datenmodellen interagiert, um die Zeitspanne zu reduzieren, die zum Entwickeln von standard-Datenvorgänge erforderlichen schnell hinzufügen, möchten.
    > 
    > Der Gerüstbau-Prozess sichergestellt, dass alle erforderlichen Abhängigkeiten im Projekt installiert werden. Z. B. Wenn Sie mit einem leeren ASP.NET-Projekt beginnen und dann mithilfe von Gerüstbau einen Web-API-Controller hinzufügen, werden die erforderlichen Web-API-NuGet-Pakete und Verweise zu Ihrem Projekt automatisch hinzugefügt.
6. In der **Controller hinzufügen** (Dialogfeld), Typ *TriviaController* in die **Controllername** Textfeld ein und klicken Sie auf **hinzufügen**.

    ![Hinzufügen des Controllers Trivia](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Trivia Controller hinzufügen")

    *Hinzufügen des Controllers Trivia*
7. Die **TriviaController.cs** Datei wird dann hinzugefügt, um die **Controller** Ordner die **GeekQuiz** Projekt, das Sie enthält einer leeres **TriviaController** Klasse. Fügen Sie die folgenden using-Anweisungen am Anfang der Datei.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Fügen Sie den folgenden Code am Anfang der **TriviaController** Klasse definieren, zu initialisieren und Freigeben der **TriviaContext** -Instanz in den Controller.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > Die **Dispose** -Methode der **TriviaController** Ruft die **Dispose** -Methode der der **TriviaContext** Instanz stellt sicher, dass alle Das Context-Objekt verwendeten Ressourcen werden freigegeben, wenn die **TriviaContext** -Instanz verworfen oder Garbage collection. Dies umfasst, schließen alle Datenbankverbindungen, die von Entity Framework geöffnet.
9. Fügen Sie die folgende Hilfsmethode am Ende der **TriviaController** Klasse. Diese Methode ruft folgender Quiz Frage ab, aus der Datenbank, die vom angegebenen Benutzer beantwortet werden.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Fügen Sie die folgenden **erhalten** Aktionsmethode die **TriviaController** Klasse. Diese Aktionsmethode ruft die **NextQuestionAsync** Hilfsmethode, die im vorherigen Schritt zum Abrufen der nächsten Frage für den authentifizierten Benutzer definiert.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Fügen Sie die folgende Hilfsmethode am Ende der **TriviaController** Klasse. Diese Methode speichert die angegebene Antwort in der Datenbank und gibt ein boolescher Wert, der zurück, ob die Antwort richtig ist.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Fügen Sie die folgenden **Post** Aktionsmethode die **TriviaController** Klasse. Diese Aktionsmethode ordnet die Antwort auf die Benutzerauthentifizierung und ruft die **StoreAsync** Hilfsmethode. Klicken Sie dann sendet es eine Antwort mit dem booleschen Wert, der mit der Hilfsmethode zurückgegeben.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Ändern Sie den Web-API-Controller, um den Zugriff auf authentifizierte Benutzer einzuschränken, durch das Hinzufügen der **autorisieren** -Attribut auf die **TriviaController** Definition der Klasse.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Aufgabe 3: Ausführen der Lösung

In dieser Aufgabe überprüfen Sie, dass die Web-API-Dienst in der vorherigen Aufgabe erstellten wie erwartet funktioniert. Verwenden Sie den Internet Explorer **F12-Entwicklertools** den Netzwerkdatenverkehr zu erfassen, und überprüfen die vollständige Antwort aus dem Web-API-Dienst.

> [!NOTE]
> Stellen Sie sicher, dass **Internet Explorer** ausgewählt ist, der **starten** Schaltfläche befindet sich auf der Symbolleiste von Visual Studio.
> 
> ![Internet Explorer-option](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Drücken Sie **F5** um die Projektmappe auszuführen. Die **melden Sie sich bei** Seite sollte im Browser angezeigt werden.

    > [!NOTE]
    > Beim Starten der Anwendung die Standard-MVC-Route ausgelöst wird, die in der Standardeinstellung wird zugeordnet, die **Index** Aktion die **HomeController** Klasse. Da **HomeController** auf authentifizierte Benutzer beschränkt ist (Denken Sie daran, dass Sie diese Klasse mit ergänzt die **autorisieren** -Attribut in Übung 1) und es gibt keine Benutzerauthentifizierung noch, die Anwendung leitet die ursprüngliche Anforderung an die Anmeldeseite geöffnet.

    ![Ausführen der Projektmappe](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "Ausführen der Lösung")

    *Ausführen der Lösung*
2. Klicken Sie auf **registrieren** zum Erstellen eines neuen Benutzers.

    ![Registrieren eines neuen Benutzers](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "Registrieren eines neuen Benutzers")

    *Registrieren eines neuen Benutzers*
3. In der **registrieren** geben eine **Benutzernamen** und **Kennwort**, und klicken Sie dann auf **registrieren**.

    ![Seite "registrieren"](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "Seite Register")

    *Seite "registrieren"*
4. Die Anwendung registriert, das neue Konto und der Benutzer authentifiziert und zurück zur Startseite umgeleitet wird.

    ![Benutzer wird authentifiziert](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "authentifizierte Benutzer")

    *Benutzer wird authentifiziert*
5. Drücken Sie im Browser **F12** zum Öffnen der **Entwicklertools** Bereich. Drücken Sie **STRG + 4** oder klicken Sie auf die **Netzwerk** Symbol, und klicken Sie dann auf der grüne Pfeil-Schaltfläche, beginnt die Erfassung von Netzwerkdatenverkehr.

    ![Initiieren die netzwerkerfassung für Web-API-](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "netzwerkerfassung initiieren, Web-API")

    *Initiieren die netzwerkerfassung für Web-API*
6. Fügen Sie **-api/Trivia** an die URL in die Adressleiste des Browsers. Sie werden jetzt die Details der Antwort überprüfen die **erhalten** Aktionsmethode im **TriviaController**.

    ![Abrufen der Daten über Web-API der nächsten Frage](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "zum Abrufen der nächsten Frage Daten über Web-API")

    *Abrufen der nächsten Frage Daten über Web-API*

    > [!NOTE]
    > Sobald der Download abgeschlossen ist, werden Sie aufgefordert, eine Aktion mit der heruntergeladenen Datei herzustellen. Lassen Sie das Dialogfeld geöffnet, um den Inhalt der Antwort über das Entwickler-Fenster überwachen können.
7. Sie werden jetzt der Text der Antwort überprüfen. Zu diesem Zweck klicken Sie auf die **Details** Registerkarte, und klicken Sie dann auf **Antworttext**. Sehen Sie sich, dass die heruntergeladenen Daten ein Objekt mit den Eigenschaften **Optionen** (Dies ist eine Liste der **TriviaOption** Objekte), **Id** und **Titel** , entsprechen die **TriviaQuestion** Klasse.

    ![Anzeigen des Web-API-Antworttexts](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "den Web-API-Antworttext anzeigen")

    *Anzeigen von Web-API-Antworttext*
8. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Übung 2: Erstellen der SPA-Schnittstelle

In dieser Übung zuerst erstellen Sie die Web-Front-End-Teil von Meister Quiz, konzentrieren sich die Interaktion mit Single-Page Application **AngularJS**. Sie werden dann verbessern die benutzererfahrung mit CSS3 durchführen umfassende Animationen und eines visuellen Effekts der Kontextwechsel, die bei der Umstellung auf die nächste Frage.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Aufgabe 1 – Erstellen der SPA-Schnittstelle, die mithilfe von AngularJS

In dieser Aufgabe verwenden Sie **AngularJS** auf die Clientseite der Anwendung Meister Quiz zu implementieren. **AngularJS** ist ein Open-Source-JavaScript-Framework, das Standardfehlerprotokoll erweitert, browserbasierte Anwendungen mit *Model-View-Controller* (MVC)-Funktion, die beide Entwicklung zu vereinfachen und zu testen.

Sie werden gestartet, indem Sie AngularJS von Visual Studio Paket-Manager-Konsole installieren. Anschließend erstellen Sie den Controller, um das Verhalten der app Meister Quiz und zu den Quizfragen und Antworten, die mithilfe der AngularJS-Vorlagen-Engine zu rendernden Ansicht bereitzustellen.

> [!NOTE]
> Weitere Informationen zu AngularJS, finden Sie unter [ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/).


1. Öffnen Sie **Visual Studio Express 2013 für Web** , und öffnen Sie die **GeekQuiz.sln** Lösung befindet sich in der **Quelle/Ex2-CreatingASPAInterface/Anfang** Ordner. Alternativ können Sie die Lösung weiter nutzen, den Sie in der vorherigen Übung haben.
2. Öffnen der **-Paket-Manager-Konsole** aus **Tools** | **Bibliothekspaket-Manager**. Geben Sie den folgenden Befehl zum Installieren der **AngularJS.Core** NuGet-Paket.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Skripts** Ordner die **GeekQuiz** Projekt, und wählen **hinzufügen | Neuer Ordner**. Nennen Sie den Ordner **app** , und drücken Sie **EINGABETASTE**.
4. Mit der rechten Maustaste die **app** Ordner, die Sie gerade erstellt, und wählen Sie **hinzufügen | JavaScript-Datei**.

    ![Erstellen einer neuen JavaScript-Datei](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Erstellen einer neuen JavaScript-Datei*
5. In der **Namen für Artikel angeben** (Dialogfeld), Typ *Quiz-Controller* in die **Elementname** Textfeld ein und klicken Sie auf **OK**.

    ![Benennen Sie die neue JavaScript-Datei](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Benennen Sie die neue JavaScript-Datei*
6. In der **Quiz-controller.js** Datei, fügen Sie den folgenden Code zum Deklarieren und initialisieren die AngularJS **QuizCtrl** Controller.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Die Konstruktorfunktion die **QuizCtrl** Controller erwartet, dass einen um injizierbare Parameter mit dem Namen **$scope**. Der anfängliche Zustand des Bereichs sollten eingerichtet werden in der Konstruktorfunktion von Eigenschaften zum Anfügen der **$scope** Objekt. Die Eigenschaften enthalten die **Ansichtsmodell**, und werden bei der Controller registriert ist auf die Vorlage zugreifen kann.
    > 
    > Die **QuizCtrl** Controller wird definiert, in ein Modul namens **QuizApp**. Module sind, dass die Arbeitseinheiten, mit denen Sie die Anwendung in separate Komponenten unterteilt. Die wichtigsten Vorteile der Verwendung von Modulen ist, dass der Code ist einfacher zu verstehen und erleichtert die Komponententests, wiederverwendbarkeit und verwaltbarkeit.
7. Fügen Sie jetzt Verhalten für den Bereich zum Reagieren auf Ereignisse, die ausgelöst wird, aus der Sicht. Fügen Sie den folgenden Code am Ende der **QuizCtrl** Controller zum Definieren der **NextQuestion** Funktion in der **$scope** Objekt.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Diese Funktion ruft die nächste Frage aus der **Trivia** Web-API in der vorherigen Übung erstellt haben, und fügt die Daten der Frage, die **$scope** Objekt.
8. Fügen Sie den folgenden Code am Ende der **QuizCtrl** Controller zum Definieren der **SendAnswer** Funktion in der **$scope** Objekt.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Diese Funktion sendet die Antwort, die vom Benutzer ausgewählten der **Trivia** Web-API und speichert das Ergebnis – d. h., wenn die Antwort richtig ist – in der **$scope** Objekt.
    > 
    > Die **NextQuestion** und **SendAnswer** Funktionen von oben verwenden die AngularJS **$http** Objekt, das die Kommunikation mit der Web-API über XMLHttpRequest abstrahieren JavaScript-Objekt aus dem Browser. AngularJS unterstützt einen anderen Dienst, der eine höhere Abstraktionsebene von CRUD-Vorgängen für einer Ressource über RESTful-APIs bietet. Die AngularJS **$resource** Objekt hat die Aktionsmethoden, die allgemeine Verhalten, ohne dass die Notwendigkeit für die Interaktion mit bieten die **$http** Objekt. Erwägen Sie die Verwendung der **$resource** Objekt in Szenarien, die den CRUD-Modell benötigt (fore Informationen finden Sie unter den [$resource Dokumentation](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Der nächste Schritt besteht darin die AngularJS-Vorlage zu erstellen, die die Ansicht für das Quiz definiert. Zu diesem Zweck öffnen Sie die **"Index.cshtml"** Datei innerhalb der **Ansichten | Startseite** Ordner, und Ersetzen Sie den Inhalt durch den folgenden Code.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Die AngularJS-Vorlage ist eine deklarative Spezifikation, die Informationen aus dem Modell und den Controller zum Transformieren von statischen Markups in die dynamische Ansicht, die der Benutzer im Browser angezeigt wird. Es folgen Beispiele für die AngularJS-Elemente und Attribute des Elements, die in einer Vorlage verwendet werden kann:
    > 
    > - Die **ng-app** -Direktive weist AngularJS das DOM-Element, das das Stammelement der Anwendung darstellt.
    > - Die **ng-Controller** Richtlinie fügt einen Controller, auf das DOM, an dem Punkt, in dem die Anweisung deklariert wird.
    > - Die Notation mit geschweiften Klammern **{{}}** steht für Bindungen an den Bereichseigenschaften, die im Controller definiert.
    > - Die **ng Klick** Direktive wird verwendet, um die in den Bereich als Reaktion auf Mausklicks definierten Funktionen aufrufen.
10. Öffnen der **"Site.CSS"** Datei innerhalb der **Content** Ordner, und fügen Sie am Ende der Datei, die ein Aussehen und Verhalten für die Ansicht Quiz bieten die folgenden hervorgehobenen Stile hinzu.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Aufgabe 2: Ausführen der Lösung

In dieser Aufgabe, die Sie ausführen, werden die Lösung, mit der neuen Schnittstelle Sie erstellt und AngularJS, um die Quiz Fragen zu beantworten.

1. Drücken Sie **F5** um die Projektmappe auszuführen.
2. Registrieren eines neuen Benutzerkontos. Führen Sie dazu die Schritte zur Registrierung in Übung 1, Aufgabe 3 beschrieben aus.

    > [!NOTE]
    > Wenn Sie die Projektmappe aus der vorherigen Übung verwenden, können Sie sich mit dem Benutzerkonto anmelden, die, denen Sie vor erstellt haben.
3. Die **Startseite** Seite sollte angezeigt werden, die erste Frage des Quiz angezeigt. Beantworten Sie die Frage, indem Sie eine der Optionen auf. Dies löst die **SendAnswer** zuvor definierte Funktion der sendet die ausgewählte Option in der **Trivia** Web-API.

    ![Die Beantwortung einer Frage](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "die Beantwortung einer Frage")

    *Die Beantwortung einer Frage*
4. Nach dem Klicken auf eine der Schaltflächen, sollte die Antwort angezeigt werden. Klicken Sie auf **nächste Frage** die folgende Frage angezeigt. Dies löst die **NextQuestion** Funktion, die im Controller definiert.

    ![Die nächste Frage anfordern](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "die nächste Frage anfordern")

    *Die nächste Frage anfordern*
5. Die nächste Frage sollte angezeigt werden. Weiterhin so oft wie gewünscht der Beantwortung von Fragen. Nach Abschluss der alle Fragen sollten Sie auf die erste Frage zurückgeben.

    ![Eine weitere Frage](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "eine weitere Frage")

    *Nächste Frage*
6. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Aufgabe 3: erstellen eine Flip-Animation mithilfe von CSS3

In dieser Aufgabe verwenden Sie CSS3-Eigenschaften zum Ausführen von umfangreichen Animationen durch einen Flip Effekt hinzufügen, wenn eine Frage beantwortet wird und die nächste Frage abgerufen wird.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Content** Ordner die **GeekQuiz** Projekt, und wählen **hinzufügen | Vorhandenes Element...** .

    ![Hinzufügen eines vorhandenen Elements in den Inhaltsordner](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Hinzufügen eines vorhandenen Elements in den Inhaltsordner")

    *Hinzufügen eines vorhandenen Elements in den Inhaltsordner*
2. In der **vorhandenes Element hinzufügen** Dialogfeld Feld, navigieren Sie zu der **Quelle bzw. Objekte** Ordner, und wählen **Flip.css**. Klicken Sie auf **Hinzufügen**.

    ![Hinzufügen der Flip.css-Datei aus Medienobjekten](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Hinzufügen der Flip.css-Datei aus Medienobjekten")

    *Hinzufügen der Flip.css-Datei aus Medienobjekten*
3. Öffnen der **Flip.css** Datei, die Sie gerade hinzugefügt haben, und überprüfen Sie den Inhalt.
4. Suchen Sie die **kippen Transformation** Kommentar. Die Formate unter diesem Kommentar verwenden das CSS **Perspektive** und **RotateY** Transformationen zum Generieren einer &quot;Karte kippen&quot; Auswirkung.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Suchen Sie die **Rückseite Bereich auszublenden, während der kippen** Kommentar. Das Format unter diesem Kommentar Blendet die zurück-Seite der Flächen aus, wenn sie vom Betrachter weglenken, durch Festlegen konfrontiert sind der **Backface Sichtbarkeit** CSS-Eigenschaft, um *ausgeblendeten*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Öffnen der **BundleConfig.cs** -Datei in die **App\_starten** Ordner, und fügen Sie den Verweis auf die **Flip.css** Datei die **&quot;~/Content/css&quot;** Style Bundle

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Drücken Sie **F5** zum Ausführen von Projektmappen und melden Sie sich mit Ihren Anmeldeinformationen.
8. Beantworten Sie eine Frage, indem Sie eine der Optionen auf. Beachten Sie, dass die Flip Auswirkungen beim Übergang zwischen den Ansichten.

    ![Die Beantwortung einer Frage mit dem Flip-Effekt](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "die Beantwortung einer Frage mit dem Flip-Effekt")

    *Die Beantwortung einer Frage mit dem Flip-Effekt*
9. Klicken Sie auf **nächste Frage** die folgende Frage abrufen. Die Flip Auswirkungen sollten erneut angezeigt werden.

    ![Die folgende Frage mit dem Flip-Effekt abrufen](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "die folgende Frage mit dem Flip-Effekt abrufen")

    *Die folgende Frage mit dem Flip-Effekt abrufen*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Nach Abschluss dieser praktischen Übungseinheit Sie haben gelernt, wie Sie:

- Erstellen eines ASP.NET Web-API-Controllers mithilfe von Gerüstbau für ASP.NET
- Implementieren Sie eine Aktion Abrufen von Web-API zum Abrufen der nächsten Frage des quiz
- Implementieren einer Web-API-Post-Aktion, um die quizantworten speichern
- Installieren Sie AngularJS über die Visual Studio-Paket-Manager-Konsole
- AngularJS-Vorlagen implementieren und Controllern
- Verwenden Sie zum Ausführen von Animationseffekten CSS3-Übergängen
