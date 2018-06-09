---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Praktische Übungseinheiten: Erstellen eine einzelnen Seite Anwendung (SPA) mit ASP.NET Web-API und Angular.js | Microsoft Docs'
author: rick-anderson
description: In herkömmlichen Webanwendungen initiiert der Client (Browser) die Kommunikation mit dem Server durch die Anforderung einer Seite. Der Server verarbeitet dann die Anforderung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "26507259"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Praktische Übungseinheiten: Erstellen einer einzelnen Seite Anwendung (SPA) mit ASP.NET Web-API und Angular.js
====================
Durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](http://aka.ms/webcamps-training-kit)

> In herkömmlichen Webanwendungen initiiert der Client (Browser) die Kommunikation mit dem Server durch die Anforderung einer Seite. Der Server verarbeitet die Anforderung und HTML-Code von der Seite an den Client sendet. Bei nachfolgenden Interaktionen mit der Seite "– z. B. der Benutzer navigiert zu einem Link oder ein Formulars mit Daten übermittelt werden – eine neue Anforderung an den Server gesendet wird, und der Ablauf wird neu gestartet: der Server verarbeitet die Anforderung und sendet Sie eine neue Seite im Browser als Reaktion auf die neue aktionsanforderung Torsten vom Client.
> 
> In den Single Page Applications (SPAs) die gesamte Seite nach der ersten Anforderung im Browser geladen, aber nachfolgende Interaktionen stattfinden über Ajax-Anforderungen. Dies bedeutet, dass der Browser muss nur der Bereich auf der Seite zu aktualisieren, die geändert wurden; Es ist nicht erforderlich, die gesamte Seite erneut zu laden. Der SPA-Ansatz reduziert die Zeit, die von der Anwendung für die Reaktion auf Benutzeraktionen, was eine mehr flüssigen Erfahrung.
> 
> Die Architektur einer SPA umfasst einigen Herausforderungen, die nicht in herkömmlichen Webanwendungen vorhanden sind. Allerdings begegnet Technologien wie ASP.NET Web-API, JavaScript-Frameworks wie AngularJS und neue Formatvorlage Features von CSS3 können sie entwerfen und Aufbauen von SPAs wirklich einfach.
> 
> In dieser Übung Hand-on-gelangen Sie, diese Technologien Meister Quiz, eine faszinierende-Website, die basierend auf dem Konzept SPA implementieren. Implementieren Sie zuerst die Dienstschicht mit ASP.NET Web-API, um die erforderlichen Endpunkte zum Abrufen der Quizfragen und speichern die Antworten. Anschließend erstellen Sie eine umfangreiche und reaktionsschnelle-Benutzeroberfläche mithilfe von AngularJS- und CSS3-Transformation Effekten.
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen von ASP.NET Web API-Dienst zum Senden und Empfangen von JSON-Daten
- Erstellen einer dynamischen Benutzeroberflächenautomatisierungs mithilfe von AngularJS
- Verbessern der Arbeitsmöglichkeiten UI mit CSS3-Transformationen

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit:

- [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/) oder höher

<a id="Setup"></a>
### <a name="setup"></a>Setup

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.

1. Öffnen Sie Windows-Explorer und Navigieren in des Labors **Quelle** Ordner.
2. Mit der rechten Maustaste auf **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** um den Setupvorgang zu starten, die Ihrer Umgebung konfigurieren und installieren die Visual Studio-Codeausschnitte für diese Übungseinheit.
3. Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.

> [!NOTE]
> Stellen Sie sicher, dass Sie alle Abhängigkeiten für diese Umgebung aktiviert haben, bevor Sie das Setup ausführen.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden die Codeausschnitte

In der Lab-Dokument werden Sie aufgefordert, Codeblöcke einfügen. Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitte, die Sie in Visual Studio 2013 zu vermeiden, dass sie manuell hinzufügen aus zugreifen können.

> [!NOTE]
> Jede Übung wird eine beginnend Projektmappe befindet sich im beiliegen der **beginnen** Ordner der Übung, die Ihnen ermöglicht, jede Übung unabhängig von den anderen folgen. Denken Sie daran, dass die Codeausschnitte, die während einer Übung hinzugefügt werden in diese Lösungen starten fehlen, werden und funktionieren möglicherweise nicht, bis die Übung abgeschlossen ist. In den Quellcode für eine Übung, finden Sie auch eine **End** Ordner, der durch den Code, der aus den Schritten in der entsprechenden Übung führt Visual Studio-Projektmappe enthält. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie weitere Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit enthält die folgenden Übungen durcharbeiten:

1. [Erstellen eine Web-API](#Exercise1)
2. [Erstellen einer SPA-Schnittstelle](#Exercise2)

Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen. Jede vordefinierte Sammlung dient zur einer bestimmten Entwicklungsstil und Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Optionen des Dialogfelds bestimmt. In dieser Übung wird beschrieben, die Aktionen erforderlich, um eine bestimmte Aufgabe in Visual Studio auszuführen, bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung. Wählen Sie eine Auflistung von unterschiedlichen Einstellungen für Ihre Entwicklungsumgebung möglicherweise Unterschiede in den Schritten, die Sie berücksichtigen sollten.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Übung 1: Erstellen einer Webs-API

Einer der wichtigsten Bestandteile einer SPA ist die Dienstebene. Er ist verantwortlich für die Verarbeitung von Ajax-Aufrufe, die von der Benutzeroberfläche und die Rückgabe von Daten als Antwort auf diesen Aufruf gesendet werden. Die abgerufenen Daten sollte angezeigt werden, in einem maschinenlesbaren Format um analysiert und vom Client genutzt wird.

Die Web-API-Framework ist Teil des Stapels ASP.NET und dient zum Implementieren von HTTP-Dienste in der Regel senden und Empfangen von JSON oder XML-formatierte Daten über eine REST-API erleichtern. In dieser Übung erstellen Sie die Website zum Hosten der Anwendung Meister Quiz und anschließenden Implementieren der Back-End-Dienst verfügbar zu machen und das Quiz Daten mithilfe der ASP.NET Web API beibehält.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Aufgabe 1 – erstellen das ursprüngliche Projekt Meister Quiz

In dieser Aufgabe starten Sie erstellen ein neues ASP.NET MVC-Projekt mit Unterstützung für ASP.NET Web-API auf der Grundlage der **One ASP.NET"** Projekttyp, der mit Visual Studio enthalten ist. **Eine ASP.NET** vereinheitlicht alle ASP.NET-Technologien und bietet Ihnen die Möglichkeit, mischen und zuordnen diese wie gewünscht. Sie werden dann Modellklassen für das Entity Framework und die Datenbank Initializator einzufügende Quizfragen hinzufügen.

1. Open **Visual Studio Express 2013 für Web** , und wählen Sie **Datei | Neues Projekt...**  um eine neue Projektmappe zu starten.

    ![Erstellen eines neuen Projekts](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "Erstellen eines neuen Projekts")

    *Erstellen eines neuen Projekts*
2. In der **neues Projekt** wählen Sie im Dialogfeld **ASP.NET-Webanwendung** unter der **Visual C# | Web** Registerkarte. Stellen Sie sicher, dass **.NET Framework 4.5** ist ausgewählt, nennen Sie sie *GeekQuiz*, wählen Sie eine **Speicherort** , und klicken Sie auf **OK**.

    ![Erstellen eines neuen Projekts für die ASP.NET-Webanwendung](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "Erstellen eines neuen Projekts für die ASP.NET-Webanwendung")

    *Erstellen eines neuen Projekts für die ASP.NET-Webanwendung*
3. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **MVC** Vorlage, und wählen die **Web-API** Option. Stellen Sie außerdem sicher, dass die **Authentifizierung** Option wird festgelegt, um **einzelne Benutzerkonten**. Klicken Sie auf **OK** um den Vorgang fortzusetzen.

    ![Erstellen eines neuen Projekts die MVC-Vorlage, einschließlich Web-API-Komponenten](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Erstellen eines neuen Projekts die MVC-Vorlage, einschließlich Web-API-Komponenten*
4. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Modelle** Ordner, der die **GeekQuiz** Projekt, und wählen Sie **hinzufügen | Vorhandenes Element...** .

    ![Hinzufügen eines vorhandenen Elements](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "ein vorhandenes Element hinzufügen")

    *Ein vorhandenes Element hinzufügen*
5. In der **vorhandenes Element hinzufügen** Dialogfeld Feld, navigieren Sie zu der **Quell-/Bestand/Modelle** Ordner, und wählen Sie alle Dateien. Klicken Sie auf **Hinzufügen**.

    ![Hinzufügen der Model-Objekte](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Hinzufügen der Model-Objekte")

    *Hinzufügen der Model-Objekte*

    > [!NOTE]
    > Indem Sie diese Dateien hinzufügen, fügen Sie das Datenmodell, Datenbankkontext für das Entity Framework und den Datenbankinitialisierer für die Anwendung Meister Quiz.
    > 
    > **Entity Framework (EF)** ist eine objektrelationale Mapper (ORM), die Ihnen zum Erstellen von datenzugriffsanwendungen mittels Programmierung mit einem konzeptionellen Anwendungsmodell anstelle des Programmierens direkt mit einem Speicherschema ermöglicht. Weitere Informationen finden Sie Informationen zu Entity Framework [hier](../../../entity-framework.md).
    > 
    > Im folgenden finden eine Beschreibung der Klassen, die Sie gerade hinzugefügt haben:
    > 
    > - **TriviaOption:** stellt eine einzelne Option, eine Frage Quiz zugeordnet
    > - **TriviaQuestion:** stellt eine Frage Quiz dar und macht die zugehörigen Optionen durch die **Optionen** Eigenschaft
    > - **TriviaAnswer:** stellt die Option ausgewählt, die vom Benutzer als Antwort auf eine Frage Quiz
    > - **TriviaContext:** der Entity Framework-Datenbankkontext der Anwendung Meister Quiz darstellt. Diese Klasse leitet sich von **DContext** und macht **DbSet** Eigenschaften, die Auflistungen der oben beschriebenen Entitäten darstellen.
    > - **TriviaDatabaseInitializer:** die Implementierung von Entity Framework Initialisierer für die **TriviaContext** die erbt **CreateDatabaseIfNotExists**. Das Standardverhalten dieser Klasse wird zum Erstellen der Datenbank nur dann, wenn er nicht vorhanden ist, Einfügen von Entitäten gemäß dem **Ausgangswert** Methode.
6. Öffnen der **Global.asax.cs** Datei, und fügen Sie die folgende Anweisung.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Fügen Sie den folgenden Code am Anfang der **Anwendung\_starten** -Methode zum Festlegen der **TriviaDatabaseInitializer** als Datenbankinitialisierer.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Ändern der **Home** Controller aus, um Zugriff auf authentifizierte Benutzer. Öffnen Sie hierzu die **HomeController.cs** innerhalb der Datei die **Controller** Ordner und Hinzufügen der **autorisieren** -Attribut auf die **HomeController**Klassendefinition.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > Die **autorisieren** Filtern prüft, ob der Benutzer authentifiziert ist. Wenn der Benutzer nicht authentifiziert ist, gibt die HTTP-Statuscode 401 (nicht autorisiert) ohne Aufrufen der Aktion zurück. Sie können den Filter, die Global auf Controllerebene, oder klicken Sie auf der Ebene der einzelnen Aktionen anwenden.
9. Sie werden nun das Layout von Webseiten und das branding anpassen. Öffnen Sie hierzu die  **\_Layout.cshtml** Datei innerhalb der **Ansichten | Freigegebene** Ordner und aktualisieren Sie den Inhalt der **&lt;Titel&gt;** Element durch Ersetzen *meine ASP.NET-Anwendung* mit *Meister Quiz* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. Aktualisieren Sie die Navigationsleiste in der gleichen Datei durch das Entfernen der *zu* und *Kontakt* Links und Umbenennen von der *Home* Verknüpfen mit *wiedergeben*. Benennen Sie die *Anwendungsname* Verknüpfen mit *Meister Quiz*. Der HTML-Code für die Navigationsleiste sollte den folgenden Code ähneln.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Die Fußzeile der Seite "Layout" aktualisieren, indem Sie ersetzen *meine ASP.NET-Anwendung* mit *Meister Quiz*. Ersetzen Sie dazu den Inhalt der **&lt;Fußzeile&gt;** Element mit den folgenden hervorgehobenen Code.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Aufgabe 2 – erstellen die TriviaController-Web-API

In der vorherigen Aufgabe haben Sie die anfängliche Struktur der Webanwendung Meister Quiz erstellt. Erstellen Sie jetzt einen einfachen Web-API-Dienst, der mit dem Datenmodell Quiz interagiert und macht die folgenden Aktionen:

- **GET/api/faszinierende**: Ruft die nächste Frage aus der Liste Quiz des authentifizierten Benutzers beantwortet werden.
- **POST/api/faszinierende**: speichert die Quiz-Antwort vom authentifizierten Benutzer angegeben wird.

Um die Basislinie für die Web-API-Controllerklasse zu erstellen, verwenden Sie die ASP.NET Gerüstbau-Tools von Visual Studio bereitgestellt werden.

1. Öffnen der **WebApiConfig.cs** Datei innerhalb der **App\_starten** Ordner. Diese Datei definiert die Konfiguration des Web-API-Diensts, z. B. wie Web-API-Controlleraktionen Routen zugeordnet sind.
2. Fügen Sie die folgenden using-Anweisung am Anfang der Datei.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Fügen Sie den folgenden hervorgehobenen Code in die **registrieren** Methode, um den Formatierer für die JSON-Daten abgerufen, indem die Web-API-Aktionsmethoden global zu konfigurieren.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > Die **CamelCasePropertyNamesContractResolver** Eigenschaftennamen, die automatisch konvertiert *in Kamel* Fall, die die allgemeine Konvention für Eigenschaftsnamen in JavaScript ist.
4. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Controller** Ordner, der die **GeekQuiz** Projekt, und wählen Sie **hinzufügen | Neues Gerüst...** .

    ![Erstellen eines neuen gerüstelements](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "Erstellen eines neuen gerüstelements")

    *Erstellen eines neuen gerüstelements*
5. In der **Gerüst hinzufügen** Dialogfeld Feld, stellen Sie sicher, dass die **allgemeine** Knoten im linken Bereich ausgewählt ist. Wählen Sie dann die **Web API 2-Controller - leere** Vorlage in der Mitte und auf **hinzufügen**.

    ![Wählen Sie die Web-API 2-Controller leere Vorlage](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "die Web-API 2-Controller leere Vorlage auswählen")

    *Die Web-API 2-Controller leere Vorlage auswählen*

    > [!NOTE]
    > **ASP.NET Gerüstbau** ist ein Code-Generierung-Framework für ASP.NET-Webanwendungen. Visual Studio 2013 umfasst die vorinstallierte Code-Generatoren für MVC und Web-API-Projekte. Verwenden Gerüstbau in Ihrem Projekt auf, wenn Sie zum schnellen Hinzufügen von Code die Interaktion mit Datenmodellen zu erstellen, um die Zeitspanne zu reduzieren erforderlich, um die standard-Datenvorgänge entwickeln möchten.
    > 
    > Gerüstbau wird sichergestellt, auch, dass die erforderlichen Abhängigkeiten im Projekt installiert sind. Z. B. Wenn Sie mit einem leeren ASP.NET-Projekt beginnen und dann mithilfe von Gerüstbau einen Web-API-Controller hinzufügen, werden die erforderlichen Web-API-NuGet-Pakete und Verweise dem Projekt automatisch hinzugefügt.
6. In der **Controller hinzufügen** Geben Sie im Dialogfeld *TriviaController* in der **Controllernamen** Textfeld ein und klicken Sie auf **hinzufügen**.

    ![Hinzufügen des Controllers faszinierende](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "faszinierende Controller hinzufügen")

    *Hinzufügen der faszinierende-Controllers*
7. Die **TriviaController.cs** Datei wird dann hinzugefügt, um die **Controller** Ordner von der **GeekQuiz** Projekt, das eine leere enthält **TriviaController** Klasse. Fügen Sie die folgenden using-Anweisungen am Anfang der Datei.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Fügen Sie den folgenden Code am Anfang der **TriviaController** Klasse definieren, initialisieren und verwerfen die **TriviaContext** Instanz im Controller.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > Die **Dispose** Methode **TriviaController** Ruft die **Dispose** Methode der **TriviaContext** -Instanz, die wird, dass sichergestellt alle die von der Context-Objekt verwendeten Ressourcen werden freigegeben, wenn die **TriviaContext** verworfen oder Garbage collection-Instanz. Dies schließt schließen alle Datenbankverbindungen, die von Entity Framework geöffnet.
9. Fügen Sie die folgende Hilfsmethode am Ende der **TriviaController** Klasse. Diese Methode ruft die folgenden Quiz Frage aus der Datenbank für den angegebenen Benutzer beantwortet werden.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Fügen Sie die folgenden **abrufen** -Aktionsmethode, die **TriviaController** Klasse. Diese Aktionsmethode ruft die **NextQuestionAsync** Hilfsmethode, die im vorherigen Schritt zum Abrufen der nächsten Frage für den authentifizierten Benutzer definiert.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Fügen Sie die folgende Hilfsmethode am Ende der **TriviaController** Klasse. Diese Methode speichert die angegebene Antwortdatei in der Datenbank und gibt einen booleschen Wert, der angibt, unabhängig davon, ob die Antwort richtig ist.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Fügen Sie die folgenden **Post** -Aktionsmethode, die **TriviaController** Klasse. Dieser Aktionsmethode ordnet der Antwort für den authentifizierten Benutzer und die Aufrufe der **StoreAsync** Hilfsmethode. Anschließend sendet er eine Antwort mit der boolesche Wert, der zurückgegeben wird, indem die Hilfsmethode.

    (Codeausschnitt - *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Ändern den Web-API-Controller zum Einschränken des Zugriffs auf authentifizierten Benutzern durch Hinzufügen der **autorisieren** -Attribut auf die **TriviaController** Klassendefinition.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Aufgabe 3: Ausführen der Projektmappe

In dieser Aufgabe überprüfen Sie, dass der Web-API-Dienst in der vorhergehenden Aufgabe erstellte wie erwartet funktioniert. Verwenden Sie Internet Explorer **F12 Entwicklertools** den Netzwerkdatenverkehr zu erfassen und überprüfen die vollständige Antwort aus dem Web-API-Dienst.

> [!NOTE]
> Stellen Sie sicher, dass **Internet Explorer** ausgewählt ist, der **starten** Schaltfläche befindet sich auf der Symbolleiste von Visual Studio.
> 
> ![Internet Explorer-option](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Drücken Sie **F5** um die Projektmappe auszuführen. Die **melden Sie sich** Seite sollte im Browser angezeigt werden.

    > [!NOTE]
    > Beim Starten der Anwendung die MVC-Standardroute wird ausgelöst, die standardmäßig zugeordnet, die **Index** Aktion von der **HomeController** Klasse. Da **HomeController** auf authentifizierte Benutzer beschränkt ist (Denken Sie daran, dass Sie diese Klasse mit ergänzt die **autorisieren** Attribut in Übung 1) und es ist kein Benutzer authentifiziert noch, die Anwendung leitet die ursprüngliche Anforderung auf der Anmeldeseite an.

    ![Ausführen der Projektmappe](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "Ausführen der Projektmappe")

    *Ausführen der Projektmappe*
2. Klicken Sie auf **registrieren** zum Erstellen eines neuen Benutzers.

    ![Registrieren eines neuen Benutzers](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "einen neuen Benutzer registrieren")

    *Registrieren eines neuen Benutzers*
3. In der **registrieren** geben eine **Benutzername** und **Kennwort**, und klicken Sie dann auf **registrieren**.

    ![Seite "registrieren"](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "Seite Register")

    *Seite "registrieren"*
4. Die Anwendung registriert das neue Konto und der Benutzer authentifiziert und zurück zur Startseite umgeleitet wird.

    ![Benutzer ist authentifiziert](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "authentifizierte Benutzer")

    *Benutzer wird authentifiziert.*
5. Drücken Sie im Browser **F12** So öffnen die **Entwicklertools** Bereich. Drücken Sie **STRG + 4** oder klicken Sie auf die **Netzwerk** Symbol, und klicken Sie dann auf der grüne Pfeil-Schaltfläche, um das Erfassen des Netzwerkverkehrs zu beginnen.

    ![Initiieren von Web-API-netzwerkerfassung](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "netzwerkerfassung initiieren, Web-API")

    *Web-API-netzwerkerfassung initiieren*
6. Append **-api/faszinierende** an die URL in die Adressleiste des Browsers angezeigt. Sie werden jetzt die Details der Antwort vom Überprüfen der **abrufen** Aktionsmethode in **TriviaController**.

    ![Abrufen der nächsten Frage Daten über Web-API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Abrufen der nächsten Frage Daten über Web-API")

    *Abrufen der nächsten Frage Daten über Web-API*

    > [!NOTE]
    > Sobald der Download abgeschlossen ist, werden Sie aufgefordert, eine Aktion mit der heruntergeladenen Datei herzustellen. Lassen Sie das Dialogfeld geöffnet, um des Inhalts der Antwort durch den Entwickler Toolfenster beobachten können.
7. Sie werden nun den Text der Antwort überprüfen. Klicken Sie hierzu auf die **Details** Registerkarte, und klicken Sie dann auf **Antworttext**. Sehen Sie sich, dass die heruntergeladenen Daten ein Objekt mit den Eigenschaften **Optionen** (Dies ist eine Liste der **TriviaOption** Objekte), **Id** und **Titel** , entsprechen die **TriviaQuestion** Klasse.

    ![Anzeigen des Web-API-Antworttexts](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "den Web-API-Antworttext anzeigen")

    *Anzeigen von Web-API-Antworttext*
8. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Übung 2: Erstellen der SPA-Schnittstelle

In dieser Übung zuerst erstellen Sie die Web-Front-End Teil Meister Quiz, wobei schwerpunktmäßig auf die Anwendung Einzelseiten-Interaktion mit **AngularJS**. Sie werden dann die vom Benutzer wahrnehmbare CSS3, umfangreiche Animationen ausführen und bereitstellen, der bei der Umstellung eine Frage zur nächsten zum Wechseln des Kontexts eines visuellen Effekts erhöhen.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Aufgabe 1 – zum Erstellen der SPA-Schnittstelle, die mithilfe von AngularJS

In dieser Aufgabe verwenden Sie **AngularJS** auf die Clientseite der Meister Quiz Anwendung implementieren. **AngularJS** ist eine Open-Source-JavaScript-Frameworks, die browserbasierten Anwendungen mit ergänzt *Model-View-Controller* (MVC)-Funktion, Erleichterung beide Entwicklung und Tests.

Sie werden gestartet, indem Sie AngularJS von Visual Studio-Paket-Manager-Konsole installieren. Anschließend erstellen Sie den Controller, um das Verhalten der app Meister Quiz sowie die zu den Quizfragen und Antworten, die mithilfe der AngularJS-Vorlagenmodul zu rendernde Ansicht bereitzustellen.

> [!NOTE]
> Weitere Informationen zu AngularJS finden Sie unter [ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/).


1. Öffnen Sie **Visual Studio Express 2013 für Web** , und öffnen Sie die **GeekQuiz.sln** Projektmappe befindet sich in der **Quell-/Ex2-CreatingASPAInterface/Anfang** Ordner. Alternativ können Sie die Projektmappe fortsetzen, dass Sie im vorherigen Schritt abgerufen haben.
2. Öffnen der **Package Manager Console** aus **Tools** | **Bibliothekspaket-Manager**. Geben Sie den folgenden Befehl zum Installieren der **AngularJS.Core** NuGet-Paket.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Skripts** Ordner, der die **GeekQuiz** Projekt, und wählen Sie **hinzufügen | Neuer Ordner**. Benennen Sie den Ordner **app** , und drücken Sie **EINGABETASTE**.
4. Mit der rechten Maustaste die **app** Ordner, die Sie soeben erstellt haben, und wählen **hinzufügen | JavaScript-Datei**.

    ![Erstellen einer neuen JavaScript-Datei](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Erstellen einer neuen JavaScript-Datei*
5. In der **Namen für Artikel angeben** Geben Sie im Dialogfeld *Quiz-Controller* in der **Elementname** Textfeld ein und klicken Sie auf **OK**.

    ![Benennen die neue JavaScript-Datei](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Benennen die neue JavaScript-Datei*
6. In der **Quiz controller.js** Datei, fügen Sie den folgenden Code zum Deklarieren und initialisieren die AngularJS **QuizCtrl** Controller.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Die Konstruktorfunktion die **QuizCtrl** Controller erwartet einen um injizierbare Parameter mit dem Namen **$scope**. Der anfängliche Zustand des Bereichs sollten eingerichtet werden in der Konstruktorfunktion von Eigenschaften zum Anfügen der **$scope** Objekt. Die Eigenschaften enthalten die **Ansichtsmodell**, und kann an der Vorlage zugegriffen werden, wenn der Controller registriert wird.
    > 
    > Die **QuizCtrl** Controller wird innerhalb eines Moduls mit dem Namen definiert **QuizApp**. Module sind Arbeitseinheiten, mit denen Sie verschiedene Komponenten die Anwendung unterbrechen. Die Hauptvorteile der Verwendung von Modulen ist, dass der Code ist leichter zu verstehen und erleichtert die Komponententests bereit, die wiederverwendbarkeit und verwaltbarkeit.
7. Fügen Sie jetzt Verhalten für den Bereich um das Reagieren auf Ereignisse, die ausgelöst wird, aus der Sicht. Fügen Sie den folgenden Code am Ende der **QuizCtrl** Controller zum Definieren der **NextQuestion** -Funktion in der **$scope** Objekt.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Diese Funktion ruft die nächste Frage aus der **faszinierende** Web-API im vorherigen Schritt erstellte und fügt die Frage Daten an die **$scope** Objekt.
8. Fügen Sie den folgenden Code am Ende der **QuizCtrl** Controller zum Definieren der **SendAnswer** -Funktion in der **$scope** Objekt.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Diese Funktion sendet die Antwort, die vom Benutzer ausgewählten der **faszinierende** Web-API und speichert das Ergebnis – d. h., wenn die Antwort richtig ist – in der **$scope** Objekt.
    > 
    > Die **NextQuestion** und **SendAnswer** Funktionen von oben verwenden die AngularJS **$http** Objekt, das die Kommunikation mit der Web-API über die XMLHttpRequest abstrakten JavaScript-Objekt über den Browser. AngularJS unterstützt einen anderen Dienst, der ein höheres Maß an Abstraktion zum Ausführen von CRUD-Vorgänge für eine Ressource über Rest-APIs bereitstellt. Die AngularJS **$resource** Objekt hat Aktionsmethoden, die allgemeine Verhaltensweisen ohne die Notwendigkeit für die Interaktion mit Bereitstellen der **$http** Objekt. Erwägen Sie die **$resource** Objekt in Szenarien, die die CRUD-Modell erfordert (fore Informationen finden Sie unter der [$resource Dokumentation](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Der nächste Schritt besteht darin die AngularJS-Vorlage erstellen, die die Sicht für das Quiz definiert. Öffnen Sie hierzu die **Index.cshtml** Datei innerhalb der **Ansichten | Startseite** Ordner und Ersetzen Sie den Inhalt durch folgenden Code.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Die AngularJS-Vorlage ist eine deklarative-Spezifikation, die Informationen aus dem Modell und dem Controller verwendet, um statische Markup in die dynamische Ansicht zu transformieren, die dem Benutzer im Browser angezeigt wird. Es folgen Beispiele für die AngularJS-Elemente und Attribute des Elements, die in einer Vorlage verwendet werden kann:
    > 
    > - Die **ng app** -Direktive AngularJS weist das DOM-Element, das das Stammelement der Anwendung darstellt.
    > - Die **ng-Controller** Direktive fügt einen Controller mit dem DOM, an dem Punkt, in dem die Anweisung deklariert wird.
    > - Die Notation mit geschweiften Klammern **{{}}** kennzeichnet Bindungen an den Bereichseigenschaften im Controller definiert.
    > - Die **ng Klick** Richtlinie wird verwendet, um die im Bereich als Antwort auf die benutzeraufrufen definierte Funktionen aufzurufen.
10. Öffnen der **"Site.CSS" ändern** Datei innerhalb der **Content** Ordner und fügen Sie die folgenden hervorgehobenen Stile am Ende der Datei um ein Aussehen und Verhalten für die Ansicht Quiz bereitzustellen.

    (Codeausschnitt - *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Aufgabe 2: Ausführen der Projektmappe

In dieser Aufgabe, die Sie ausführen, werden die Projektmappe mit der neuen Schnittstelle integriert AngularJS das Quiz Fragen zu beantworten.

1. Drücken Sie **F5** um die Projektmappe auszuführen.
2. Registrieren eines neuen Benutzerkontos. Zu diesem Zweck die Schritte Registrierung in Übung 1, Aufgabe 3 beschrieben.

    > [!NOTE]
    > Wenn Sie die Projektmappe aus der vorherigen Übung verwenden, können Sie sich mit dem Benutzerkonto anmelden vor der Erstellung.
3. Die **Home** Seite sollte angezeigt werden, die erste Frage des Quiz angezeigt. Durch Klicken auf eine der Optionen die Frage beantwortet wird. Daraufhin die **SendAnswer** zuvor definierte Funktion sendet die ausgewählte Option in der **faszinierende** Web-API.

    ![Um eine Frage zu beantworten](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "um eine Frage beantworten.")

    *Um eine Frage beantworten.*
4. Nach dem Klicken auf eine der Schaltflächen klicken, sollte der Antwort angezeigt werden. Klicken Sie auf **nächste Frage** folgende Frage angezeigt. Daraufhin die **NextQuestion** Funktion, die im Controller definiert.

    ![Die nächste Frage anfordern](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "die nächste Frage anfordern")

    *Die nächste Frage anfordern*
5. Die nächste Frage sollte angezeigt werden. Weiterhin Beantwortung von Fragen so oft wie gewünscht an. Nach Abschluss aller Fragen sollte die erste Frage wiederhergestellt werden.

    ![Eine andere Frage](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "gleichzusetzen")

    *Nächste Frage*
6. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Aufgabe 3: Erstellen einer Flip Animation mithilfe von CSS3

In dieser Aufgabe verwenden Sie CSS3-Eigenschaften zum Ausführen von umfangreicher Animationen durch Hinzufügen einer Flip wirksam, wenn eine Frage beantwortet wird, und wenn die nächste Frage abgerufen wird.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die **Content** Ordner, der die **GeekQuiz** Projekt, und wählen Sie **hinzufügen | Vorhandenes Element...** .

    ![Hinzufügen eines vorhandenen Elements in den Inhaltsordner](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Hinzufügen eines vorhandenen Elements in den Inhaltsordner")

    *Hinzufügen eines vorhandenen Elements in den Inhaltsordner*
2. In der **vorhandenes Element hinzufügen** Dialogfeld Feld, navigieren Sie zu der **Quell-/Bestand** Ordner, und wählen **Flip.css**. Klicken Sie auf **Hinzufügen**.

    ![Hinzufügen der Flip.css-Datei aus Objekten](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Hinzufügen der Flip.css-Datei aus Objekten")

    *Hinzufügen der Flip.css-Datei aus Objekten*
3. Öffnen der **Flip.css** Datei, die Sie gerade hinzugefügt haben, und überprüfen Sie den Inhalt.
4. Suchen Sie die **kippen Transformation** Kommentar. Die Formate unter diesem Kommentar verwenden Sie den CSS-Code **Perspektive** und **RotateY** Transformationen zum Generieren einer &quot;Karte die Flip&quot; wirksam.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Suchen Sie die **Rückseite Bereich auszublenden, während die Flip** Kommentar. Das Format unter diesem Kommentar Blendet clientseitige zurück auf den Flächen aus, wenn sie durch Festlegen von Viewer auseinandersetzen müssen die **rückwärtige Sichtbarkeit** CSS-Eigenschaft, um *ausgeblendete*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Öffnen der **BundleConfig.cs** Datei innerhalb der **App\_starten** Ordner und fügen Sie den Verweis auf die **Flip.css** in der Datei die **&quot;~/Content/css&quot;** Style-Paket

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Drücken Sie **F5** zum Ausführen von Projektmappen und melden Sie sich mit Ihren Anmeldeinformationen.
8. Durch Klicken auf eine der Optionen, um eine Frage zu beantworten. Beachten Sie die Flip Auswirkungen beim Übergang zwischen Ansichten.

    ![Die Beantwortung einer Frage mit die Flip Effekt](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "die Beantwortung einer Frage mit die Flip Auswirkung")

    *Die Beantwortung einer Frage mit die Flip Auswirkung*
9. Klicken Sie auf **nächste Frage** folgende Frage abgerufen. Die Flip Auswirkungen sollte erneut angezeigt werden.

    ![Abrufen der folgenden Frage mit die Flip Effekt](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "folgende Frage mit die Flip Auswirkungen abrufen")

    *Die folgende Frage mit die Flip Auswirkungen abrufen*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Dieser praktischen Übungseinheit abschließen, indem Sie haben gelernt, wie Sie:

- Erstellen eines ASP.NET Web-API-Controllers mithilfe von ASP.NET Gerüstbau
- Implementieren einer Aktion Web-API aufzurufen, um die nächste Quiz Frage abrufen
- Implementieren einer Web-API-Post-Aktion, um die quizantworten speichern
- Installieren Sie AngularJS über die Visual Studio-Paket-Manager-Konsole
- AngularJS-Vorlagen implementieren und Controllern
- Verwenden von CSS3-Übergänge Animationseffekte ausführen
