---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 benutzerdefinierte Aktionsfilter | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET MVC bietet Aktionsfilter für die Ausführung der Filterlogik entweder vor oder nach eine Aktionsmethode aufgerufen wird. Aktionsfilter sind benutzerdefinierte Attribute Tha...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 0170fda6849c1dfb53b44908ea55ba2cad0dd067
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827390"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 – benutzerdefinierte Aktionsfilter

durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC bietet Aktionsfilter für die Ausführung der Filterlogik entweder vor oder nach eine Aktionsmethode aufgerufen wird. Aktionsfilter sind benutzerdefinierte Attribute, die deklarative systemverarbeitungsaufwand Aktionsmethoden des Controllers einfügen vor und nach Abschluss der Aktion-Verhalten hinzu.

In dieser praktischen Übungseinheit erstellen Sie eine benutzerdefinierte Aktionsfilterattribut in MvcMusicStore Lösung zum Abfangen von Anforderungen des Controllers, und melden Sie sich die Aktivität von einem Standort in eine Datenbanktabelle. Sie werden Ihre Protokollierungsfilter durch Injection auf alle Controller oder eine Aktion hinzufügen. Zum Schluss sehen Sie die Protokollansicht mit der Liste der Besucher.

Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse der **ASP.NET MVC**. Wenn Sie nicht verwendet haben **ASP.NET MVC** vor, wir empfehlen Ihnen, sich mit Windows Live ID **Grundlagen von ASP.NET MVC 4** Hands-On Lab.

> [!NOTE]
> Alle Beispielcode und Ausschnitte sind im Web Camps Trainingskit, erhältlich über enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für dieses Lab finden Sie unter [benutzerdefinierte Aktionsfilter für ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen einer benutzerdefinierten Aktionsfilterattribut zum Erweitern von Filterfunktionen
- Wenden Sie ein benutzerdefiniertes Filterattribut an, indem Sie Injection auf eine bestimmte Ebene
- Registrieren Sie eine benutzerdefinierte Aktionsfilter Global

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes, um diese testumgebung abzuschließen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber ist Großteil des Codes, die entlang dieser Übungseinheit verwaltet werden soll als Codeausschnitte für Visual Studio verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang C: Verwenden von Code Snippets](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit besteht aus durch die folgenden Übungen:

1. [Übung 1: Protokollieren von Aktionen](#Exercise1)
2. [Übung 2: Verwalten von sind mehrere Aktionsfilter](#Exercise2)

Geschätzte Zeit für diese testumgebung abzuschließen: **30 Minuten**.

> [!NOTE]
> Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll. Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Übung 1: Protokollieren von Aktionen

In dieser Übung lernen Sie, wie ein benutzerdefinierten Aktionsfilters Protokoll erstellt wird, mithilfe von ASP.NET MVC 4-Filteranbieter. Zu diesem Zweck werden Sie einen Protokollierungsfilter auf der Music Store-Website anwenden, die alle Aktivitäten in den ausgewählten Controllern aufgezeichnet werden.

Der Filter wird erweitert **ActionFilterAttributeClass** und überschreiben **OnActionExecuting** Methode zum Abfangen jeder Anforderung ein, und führen Sie dann die Protokollierungsaktionen. Die Kontextinformationen über HTTP-Anforderungen, Ausführen von Methoden, Ergebnisse und Parameter erfolgt durch ASP.NET MVC **ActionExecutingContext** Klasse **.**

> [!NOTE]
> ASP.NET MVC 4 verfügt auch über Standard-Filter-Anbieter, die Sie verwenden können, ohne einen benutzerdefinierten Filter erstellen. ASP.NET MVC 4 bietet die folgenden Arten von Filtern:
> 
> - **Autorisierung** zu filtern, wodurch sicherheitsrelevanten Aspekten, ob eine Aktionsmethode – z. B. zur Authentifizierung oder zum Überprüfen der Eigenschaften der Anforderung ausgeführt.
> - **Aktion** Filter, der die aktionsmethodenausführung umschließt. Dieser Filter kann zusätzliche Verarbeitungsschritte, z. B. Bereitstellen zusätzlicher Daten an die Aktionsmethode, das Überprüfen des Rückgabewerts oder das Abbrechen der Ausführung der Aktionsmethode ausführen.
> - **Ergebnis** Filter, der Ausführung von der ActionResult-Objekt umschließt. Dieser Filter kann zusätzliche Verarbeitung des Ergebnisses, z. B. Ändern der HTTP-Antwort ausführen.
> - **Ausnahme** Filter, der ausgeführt wird, wenn es eine nicht behandelte Ausnahme, die an einer beliebigen Stelle in der Aktionsmethode, die mit der Autorisierungsfilter beginnt und endet mit der Ausführung des Ergebnisses ausgelöst werden. Ausnahmefilter können für Aufgaben wie die Protokollierung oder Anzeigen einer Fehlerseite verwendet werden.
> 
> Weitere Informationen zu Anbietern für Filter finden Sie unter diesem MSDN-Link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Informationen zu MVC Music Store-Anwendung Protokollierung (Feature)

Diese Lösung für die Music Store verfügt über eine neue Datentabelle Modell für die Website-Protokollierung **ActionLog**, mit den folgenden Feldern: Name des Controllers, die eine Anforderung, die aufgerufene Aktion, die Client-IP und die Zeitstempel zu empfangen.

![Entity Data Model. ActionLog-Tabelle. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Datenmodell. ActionLog-Tabelle.")

*Datenmodell - ActionLog-Tabelle*

Die Lösung bietet eine ASP.NET MVC-Ansicht für das Aktionsprotokoll, die finden Sie unter **MvcMusicStores/Views/ActionLog**:

![Aktion-Protokollansicht](aspnet-mvc-4-custom-action-filters/_static/image2.png "Aktionsprotokoll anzeigen")

*Aktion-Log-Ansicht*

Mit dieser Struktur angegeben werden alle Aufgaben des Controllers Anforderung unterbrechen und Durchführen der protokollierungs mit benutzerdefinierten Filtern ausgerichtet sein.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Aufgabe 1: Erstellen eines benutzerdefinierten Filters zum Abfangen eines Controllers-Anforderung

In dieser Aufgabe erstellen Sie eine benutzerdefinierten Filter Attributklasse, die die protokollierungslogik enthalten soll. Erweitern Sie für diesen Zweck ASP.NET MVC **ActionFilterAttribute** -Klasse und Implementieren der Schnittstelle **IActionFilter**.

> [!NOTE]
> Die **ActionFilterAttribute** ist die Basisklasse für alle Attributfilter. Es bietet die folgenden Methoden, um eine bestimmte Logik nach und vor der Ausführung der der Controlleraktion auszuführen:
> 
> - **OnActionExecuting**(ActionExecutingContext FilterContext): nur vor der Aktion wird aufgerufen.
> - **OnActionExecuted**(ActionExecutedContext FilterContext): Nachdem die Aktionsmethode aufgerufen wurde und bevor das Ergebnis (vor dem Rendern der Ansicht) ausgeführt wird.
> - **OnResultExecuting**(ResultExecutingContext FilterContext): vor dem Ausführen des Ergebnisses (vor dem Rendern der Ansicht).
> - **OnResultExecuted**(ResultExecutedContext FilterContext): nach dem Ausführen des Ergebnisses (nach dem Rendern der Ansicht).
> 
> Indem Sie Sie einer dieser Methoden in einer abgeleiteten Klasse überschreiben, können Sie Ihren eigenen Filter Code ausführen.


1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **\Source\Ex01-LoggingActions\Begin** Ordner.

   1. Sie müssen einige fehlende NuGet-Pakete herunterladen, bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
      > 
      > Weitere Informationen finden Sie im Artikel: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Fügen Sie eine neue C#-Klasse in der **Filter** Ordner und nennen Sie sie *CustomActionFilter.cs*. In diesem Ordner werden alle benutzerdefinierten Filter gespeichert.
3. Open **CustomActionFilter.cs** und Hinzufügen eines Verweises auf **System.Web.Mvc** und **MvcMusicStore.Models** Namespaces:

    (Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex1-CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Erben der **"customactionfilter"** -Klasse aus **ActionFilterAttribute** und stellen Sie dann **"customactionfilter"** implementieren **IActionFilter** Schnittstelle.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Stellen **"customactionfilter"** Klasse überschreiben Sie die Methode **OnActionExecuting** und fügen Sie die erforderliche Logik, um den Filter für die Ausführung zu protokollieren. Zu diesem Zweck fügen Sie folgenden hervorgehobenen Code in **"customactionfilter"** Klasse.

    (Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex1-LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** Methode ist die Verwendung **Entity Framework** um ein neues ActionLog Journal hinzuzufügen. Erstellt und füllt Sie mit den Kontextinformationen über eine neue Entitätsinstanz **FilterContext**.
    > 
    > Erfahren Sie mehr über **ControllerContext** -Klasse am [Msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Aufgabe 2: Einfügen eines Code-Interceptors in der Store-Controller-Klasse

In dieser Aufgabe fügen Sie den benutzerdefinierten Filter durch Einspeisen von es für alle Controllerklassen und Controlleraktionen, die protokolliert werden. Im Rahmen dieser Übung müssen die Store-Controller-Klasse, ein Protokoll.

Die Methode **OnActionExecuting** aus **ActionLogFilterAttribute** benutzerdefinierter Filter ausgeführt wird, wenn ein eingefügten Element aufgerufen wird.

Es ist auch möglich, eine bestimmte Controllermethode abzufangen.

1. Öffnen der **StoreController** am **MvcMusicStore\Controllers** und Hinzufügen eines Verweises auf die **Filter** Namespace:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Den benutzerdefinierten Filter einfügen **"customactionfilter"** in **StoreController** -Klasse durch das Hinzufügen **["customactionfilter"]** Attribut vor der Klassendeklaration.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Wenn Sie ein Filter in einer Controllerklasse eingefügt wird, werden auch alle zugehörigen Aktionen eingefügt. Wenn Sie den Filter nur für eine Reihe von Aktionen anwenden möchten, müssten Sie injizieren **["customactionfilter"]** jeweils davon:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, dass der Protokollierungsfilter ausgeführt wird. Sie starten die Anwendung und besuchen Sie den Store, und anschließend werden Sie protokollierte Aktivitäten überprüfen.

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Navigieren Sie zu **/ActionLog** auf den Ausgangszustand des Protokoll-Ansicht finden Sie unter:

    ![Protokollstatus Tracker vor der Aktivität "Seite"](aspnet-mvc-4-custom-action-filters/_static/image3.png "Tracker-Status vor der Aktivität \"Seite\" Protokollieren")

    *Log-Tracker-Status vor der Aktivität "Seite"*

   > [!NOTE]
   > Standardmäßig wird immer ein Element angezeigt, die generiert wird, wenn Sie die vorhandenen Genres für das Menü abrufen.
   > 
   > Aus Gründen der Einfachheit haben wir Bereinigen von der **ActionLog** Tabelle jedes Mal die Anwendung ausgeführt wird, sodass sie nur die Protokolle der Überprüfung für jede bestimmte Aufgabe angezeigt wird.
   > 
   > Müssen Sie möglicherweise entfernen den folgenden Code aus der **Sitzung\_starten** Methode (in der **"Global.asax"** Klasse), um ein historisches Protokoll für alle Aktionen, die ausgeführt werden, in den Store zu speichern. Controller.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Klicken Sie auf eines der **Genres** aus dem Menü und führen Sie einige Aktionen, wie das Durchsuchen eines Albums verfügbaren.
4. Navigieren Sie zu **/ActionLog** und wenn das Protokoll, drücken Sie die leere ist **F5** die Seite aktualisieren. Überprüfen Sie, dass Ihre Besuche nachverfolgt wurden:

    ![Aktionsprotokoll mit Aktivität protokolliert](aspnet-mvc-4-custom-action-filters/_static/image4.png "Aktionsprotokoll mit Aktivität protokolliert")

    *Aktionsprotokoll mit Aktivität protokolliert*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Übung 2: Verwalten von sind mehrere Aktionsfilter

In dieser Übung werden Sie eine zweite benutzerdefinierte Aktionsfilter der StoreController-Klasse hinzufügen und Definieren dieser spezifische Reihenfolge, in der beide Filter ausgeführt werden. Klicken Sie dann aktualisieren Sie den Code, um den Filter global zu registrieren.

Es gibt verschiedene Optionen in Betracht ziehen, bei der Definition der Ausführungsreihenfolge der Filter ein. Z. B. die Order-Eigenschaft und die Filter Bereich:

Sie können definieren, eine **Bereich** für jeden der Filter, z. B. können Sie den Bereich der Aktionsfilter für die Ausführung innerhalb der **Bereich von Controller**, und alle Autorisierungsfilter für die Ausführung im **globalen Gültigkeitsbereich** . Die Bereiche verfügen über einen definierten Ausführungsreihenfolge.

Darüber hinaus verfügt jeder Aktionsfilter eine Order-Eigenschaft, die verwendet wird, um zu bestimmen, die Ausführungsreihenfolge im Bereich des Filters.

Weitere Informationen zu Ausführungsreihenfolge benutzerdefinierte Aktionsfilter verwendet werden, finden Sie unter diesem MSDN-Artikel: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Aufgabe 1: Erstellen eines neuen benutzerdefinierten Aktionsfilters

In dieser Aufgabe erstellen Sie eine neue benutzerdefinierte Aktionsfilter beim Einfügen in die StoreController-Klasse, lernen, wie Sie die Ausführungsreihenfolge der Filter zu verwalten.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **\Source\Ex02-ManagingMultipleActionFilters\Begin** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

        > [!NOTE]
        > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
        > 
        > Weitere Informationen finden Sie im Artikel: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Fügen Sie eine neue C#-Klasse in der **Filter** Ordner und nennen Sie sie *MyNewCustomActionFilter.cs*
3. Open **MyNewCustomActionFilter.cs** und Hinzufügen eines Verweises auf **System.Web.Mvc** und **MvcMusicStore.Models** Namespace:

    (Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex2-MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Ersetzen Sie die Standard-Klassendeklaration durch den folgenden Code ein.

    (Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex2-MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Diese benutzerdefinierte Aktionsfilter ist fast identisch als das, was Sie in der vorherigen Übung erstellt haben. Der Hauptunterschied besteht darin, dass sie hat die *&quot;protokolliert von&quot;* Attribut, die mit dieser neuen Klasse-Namen zur Identifizierung von eingetragen werden sollen Filter aktualisiert registriert das Protokoll.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Aufgabe 2: Einfügen eines neuen Code-Interceptors in der StoreController-Klasse

In dieser Aufgabe Sie einen neuen benutzerdefinierten Filter hinzufügen, in die StoreController-Klasse und führen Sie die Projektmappe, um zu überprüfen, wie beide Filter zusammenarbeiten.

1. Öffnen der **StoreController** Klasse befindet sich am **MvcMusicStore\Controllers** , und fügen Sie den neuen benutzerdefinierten Filter **MyNewCustomActionFilter** in  **StoreController** Klasse wie im folgenden Code gezeigt wird.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Führen Sie nun die Anwendung aus, um zu sehen, wie diese zwei benutzerdefinierte Aktionsfilter. Drücken Sie die zu diesem Zweck **F5** und warten, bis die Anwendung gestartet wird.
3. Navigieren Sie zu **/ActionLog** Anfangszustand des Protokoll-Ansicht angezeigt.

    ![Protokollstatus Tracker vor der Aktivität "Seite"](aspnet-mvc-4-custom-action-filters/_static/image5.png "Tracker-Status vor der Aktivität \"Seite\" Protokollieren")

    *Log-Tracker-Status vor der Aktivität "Seite"*
4. Klicken Sie auf eines der **Genres** aus dem Menü und führen Sie einige Aktionen, wie das Durchsuchen eines Albums verfügbaren.
5. Überprüfen Sie, ob dieses Mal; Ihre Besuche zweimal nachverfolgt wurden: Nachdem für jede der benutzerdefinierten Aktionsfilter, die Sie in hinzugefügt haben die **"storagecontroller"** Klasse.

    ![Aktionsprotokoll mit Aktivität protokolliert](aspnet-mvc-4-custom-action-filters/_static/image6.png "Aktionsprotokoll mit Aktivität protokolliert")

    *Aktionsprotokoll mit Aktivität protokolliert*
6. Schließen Sie den Browser.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Aufgabe 3: Verwalten von Filterreihenfolge

In dieser Aufgabe lernen Sie, wie Sie die Ausführungsreihenfolge der Filter zu verwalten, indem Sie mit der Reihenfolge aus.

1. Öffnen der **StoreController** Klasse befindet sich am **MvcMusicStore\Controllers** , und geben Sie die **Reihenfolge** -Eigenschaft in beide Filter wie wie unten gezeigt.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Überprüfen Sie, wie die Filter ausgeführt werden, abhängig von der Order-Eigenschaft-Wert. Sie finden, das den Filter mit dem kleinsten Wert der Bestellung (**"customactionfilter"**) ist der erste, der ausgeführt wird. Drücken Sie **F5** und warten, bis die Anwendung gestartet wird.
3. Navigieren Sie zu **/ActionLog** Anfangszustand des Protokoll-Ansicht angezeigt.

    ![Protokollstatus Tracker vor der Aktivität "Seite"](aspnet-mvc-4-custom-action-filters/_static/image7.png "Tracker-Status vor der Aktivität \"Seite\" Protokollieren")

    *Log-Tracker-Status vor der Aktivität "Seite"*
4. Klicken Sie auf eines der **Genres** aus dem Menü und führen Sie einige Aktionen, wie das Durchsuchen eines Albums verfügbaren.
5. Überprüfen Sie, dass dieses Mal Ihre Besuche nachverfolgt wurden durch die Filter den Wert sortiert: **"customactionfilter"** Protokolle erste.

    ![Aktionsprotokoll mit Aktivität protokolliert](aspnet-mvc-4-custom-action-filters/_static/image8.png "Aktionsprotokoll mit Aktivität protokolliert")

    *Aktionsprotokoll mit Aktivität protokolliert*
6. Jetzt Sie die Filter den Wert zu aktualisieren und überprüfen Sie, wie die Protokollierung Reihenfolge ändert. In der **StoreController** Klasse, zum Aktualisieren der Filterliste Reihenfolgenwert wie unten gezeigt.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Die Anwendung erneut ausführen, durch Drücken von **F5**.
8. Klicken Sie auf eines der **Genres** aus dem Menü und führen Sie einige Aktionen, wie das Durchsuchen eines Albums verfügbaren.
9. Überprüfen Sie, dass dieses Mal die Protokolle, indem erstellt **MyNewCustomActionFilter** Filter wird zuerst angezeigt.

    ![Aktionsprotokoll mit Aktivität protokolliert](aspnet-mvc-4-custom-action-filters/_static/image9.png "Aktionsprotokoll mit Aktivität protokolliert")

    *Aktionsprotokoll mit Aktivität protokolliert*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Aufgabe 4: Registrieren von Global filtert

In dieser Aufgabe aktualisieren Sie die Projektmappe, um den neuen Filter zu registrieren (**MyNewCustomActionFilter**) als globalen Filter. Auf diese Weise wird es über alle Aktionen-ausgeführt in der Anwendung und nicht nur in der StoreController werden wie in der vorherigen Aufgabe ausgelöst werden.

1. In **StoreController** -Klasse, entfernen Sie **[MyNewCustomActionFilter]** Attribut und der Order-Eigenschaft von **["customactionfilter"]**. Es sollte wie folgt aussehen:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Open **"Global.asax"** Datei, und suchen Sie die **Anwendung\_starten** Methode. Beachten Sie, dass es sich bei jedem Start der Anwendung es wird der globalen Filter registrieren, durch den Aufruf **RegisterGlobalFilters** -Methode im **FilterConfig** Klasse.

    ![Registrieren globale Filter in "Global.asax"](aspnet-mvc-4-custom-action-filters/_static/image10.png "registrieren globaler Filter in \"Global.asax\"")

    *Registrieren globale Filter in "Global.asax"*
3. Open **"FilterConfig.cs"** in Datei **App\_starten** Ordner.
4. Fügen Sie einen Verweis auf System.Web.Mvc verwenden; Verwenden von MvcMusicStore.Filters; Namespace.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Update **RegisterGlobalFilters** Methode, die Ihren benutzerdefinierten Filter hinzufügen. Zu diesem Zweck fügen Sie den hervorgehobenen Code hinzu:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Führen Sie die Anwendung durch Drücken von **F5**.
7. Klicken Sie auf eines der **Genres** aus dem Menü und führen Sie einige Aktionen, wie das Durchsuchen eines Albums verfügbaren.
8. Überprüfen Sie, die jetzt **[MyNewCustomActionFilter]** wird auch im HomeController und ActionLogController eingefügt wird.

    ![Aktionsprotokoll mit Aktivität protokolliert](aspnet-mvc-4-custom-action-filters/_static/image11.png "Aktionsprotokoll mit Aktivität protokolliert")

    *Aktionsprotokoll mit globale Aktivität protokolliert*

> [!NOTE]
> Darüber hinaus können Sie diese Anwendung für Windows Azure-Websites Folgendes bereitstellen [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Von dieser praktischen Übungseinheit haben Sie gelernt einen Aktionsfilter zum Ausführen von benutzerdefinierter Aktionen zu erweitern. Sie haben auch gelernt, Filter an, die Ihre Seite Controller einzufügen. Es wurden die folgenden Begriffe verwendet:

- Vorgehensweise: Erstellen von benutzerdefinierten Aktionsfiltern mit der ASP.NET MVC ActionFilterAttribute-Klasse
- Gewusst wie: Filtern in ASP.NET MVC-Controller einfügen
- So verwalten Sie Filter zu sortieren, mit der Order-Eigenschaft
- Vorgehensweise: Filter Global registrieren

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

In diesem Anhang wird veranschaulicht, wie erstellen eine neue Website aus dem Windows Azure-Verwaltungsportal, und Veröffentlichen der Anwendung, die Sie erhalten haben, indem der testumgebung können die Nutzung der Web Deploy-publishing-Funktion von Windows Azure bereitgestellte.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal

1. Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Mit Windows Azure können Sie 10 ASP.NET-Websites kostenlos hosten und dann zu skalieren, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** auf der Befehlsleiste.

    ![Erstellen einer neuen Website](aspnet-mvc-4-custom-action-filters/_static/image18.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Ein Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können. Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung auf dem Windows Azure-Website von außerhalb des Portals bereitstellen. Er umfasst keine Informationen zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der Schnellerfassung](aspnet-mvc-4-custom-action-filters/_static/image19.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")

    *Erstellen einer neuen Website mithilfe der Schnellerfassung*
4. Warten Sie, bis die neue **Website** erstellt wird.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website ausgeführt wird.

    ![Navigieren zu der neuen Website](aspnet-mvc-4-custom-action-filters/_static/image20.png "auf die neue Website durchsuchen")

    *Um die neue Website durchsuchen*

    ![Website](aspnet-mvc-4-custom-action-filters/_static/image21.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-custom-action-filters/_static/image22.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Windows Azure-Websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-custom-action-filters/_static/image23.png "herunterladen den Website-Veröffentlichungsprofil")

    *Herunterladen der Website-Veröffentlichungsprofil*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter. In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-custom-action-filters/_static/image24.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbank-Servers

Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen. Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank. Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement in das Windows Azure-Verwaltungsportal unter **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.

    ![SQL Server-Datenbank-Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "Dashboard der SQL-Datenbank-Server")

    *SQL Server-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**. Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Bestätigen von Änderungen*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

1. Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](aspnet-mvc-4-custom-action-filters/_static/image29.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren das Veröffentlichungsprofil](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.

    ![Überprüfen der Verbindung](aspnet-mvc-4-custom-action-filters/_static/image31.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.
   - In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.
   - In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.
   - Geben Sie einen neuen Datenbanknamen ein.

     ![Konfigurieren von Ziel-Verbindungszeichenfolge](aspnet-mvc-4-custom-action-filters/_static/image33.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.

    ![Erstellen der Datenbank](aspnet-mvc-4-custom-action-filters/_static/image34.png "erstellen die datenbankzeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Windows Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Auf der SQL-Datenbank-Verbindungszeichenfolge](aspnet-mvc-4-custom-action-filters/_static/image35.png "auf SQL-Datenbank-Verbindungszeichenfolge")

    *Auf der SQL-Datenbank-Verbindungszeichenfolge*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](aspnet-mvc-4-custom-action-filters/_static/image36.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code zur Hand benötigten. Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](aspnet-mvc-4-custom-action-filters/_static/image37.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")

*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*

***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.
2. Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.
4. Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-custom-action-filters/_static/image38.png "Geben Sie den Namen des Ausschnitts")

*Geben Sie den Namen des Ausschnitts*

![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](aspnet-mvc-4-custom-action-filters/_static/image39.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")

*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*

![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](aspnet-mvc-4-custom-action-filters/_static/image40.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")

*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.
2. Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-custom-action-filters/_static/image41.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](aspnet-mvc-4-custom-action-filters/_static/image42.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*
