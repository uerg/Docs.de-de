---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 benutzerdefinierte Aktionsfilter | Microsoft Docs
author: rick-anderson
description: "ASP.NET MVC bietet Aktionsfilter für das Ausführen von Filterlogik entweder vor oder nach eine Aktionsmethode aufgerufen wird. Aktionsfilter sind benutzerdefinierte Attribute Tha..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 639815cc92b7cb5f3dfb4e1a198f6b4c2476dc90
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>ASP.NET MVC 4 benutzerdefinierten Aktionsfiltern

Durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC bietet Aktionsfilter für das Ausführen von Filterlogik entweder vor oder nach eine Aktionsmethode aufgerufen wird. Aktionsfilter sind benutzerdefinierte Attribute, die bereitstellen, deklaratives Mittel für den Controller Aktionsmethoden einfügen vor und nach Abschluss der Aktion Verhalten hinzu.

In dieser praktischen Übungseinheit erstellen Sie eine benutzerdefinierte Aktionsfilterattribut in MvcMusicStore Lösung zum Abfangen von Anforderungen des Controllers und melden Sie sich die Aktivität von einem Standort in einer Datenbanktabelle. Sie werden können den Protokollierungsfilter von Injection Controller bzw. die Aktionsmethode hinzufügen. Schließlich wird die Protokollansicht angezeigt, die die Liste der Besucher anzeigt.

Diese praktische Übungseinheit wird vorausgesetzt, dass grundlegende Kenntnisse im **ASP.NET-MVC**. Wenn Sie nicht verwendet haben **ASP.NET-MVC** vorher, empfehlen wir Ihnen, durchlaufen **ASP.NET MVC 4-Grundlagen** praktische Übungseinheit.

> [!NOTE]
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit zur Nächten enthalten [Versionen von Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für diese Übung finden Sie unter [benutzerdefinierte Aktionsfilter von ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Erstellen Sie eine benutzerdefinierte Aktionsfilterattribut zum Erweitern von Filterfunktionen
- Wenden Sie ein benutzerdefiniertes Filterattribut an, indem Sie Injection auf einen bestimmten Grad an
- Registrieren Sie einen benutzerdefinierten Aktionsfiltern Global

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:

- [Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

**Installieren von Codeausschnitten**

Der Einfachheit halber gilt Großteil des Codes, den Sie entlang dieser Übung verwalten als Visual Studio-Codeausschnitte verfügbar. So installieren Sie die Codeausschnitte ausführen **.\Source\Setup\CodeSnippets.vsi** Datei.

Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang "c:" mithilfe von Code Snippets](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit wird durch den folgenden Übungen umfasst:

1. [Übung 1: Protokollieren von Aktionen](#Exercise1)
2. [Übung 2: Verwalten mehrerer Aktionsfilter](#Exercise2)

Geschätzte Zeit zum Abschließen dieser testumgebung: **30 Minuten**.

> [!NOTE]
> Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Übung 1: Protokollieren von Aktionen

In dieser Übung erfahren Sie, wie eine benutzerdefinierte Aktion Protokollfilter erstellt mithilfe von ASP.NET MVC 4-Filteranbieter. Zu diesem Zweck werden Sie einen Protokollierungsfilter für die MusicStore Site gelten, die alle Aktivitäten in der ausgewählten Controller aufgezeichnet werden.

Erweitern Sie der Filter **ActionFilterAttributeClass** und überschreiben **OnActionExecuting** Methode, um jede Anforderung abfangen und führen Sie dann das Protokollieren von Aktionen. Die Kontextinformationen über HTTP-Anforderungen, Ausführen von Methoden, Ergebnisse und Parameter bereitgestellt werden von ASP.NET MVC **ActionExecutingContext** Klasse **.**

> [!NOTE]
> ASP.NET MVC 4 hat auch die Filter-Standardanbieter, die Sie verwenden können, ohne einen benutzerdefinierten Filter erstellen. ASP.NET MVC 4 stellt die folgenden Arten von Filtern:
> 
> - **Autorisierung** zu filtern, wodurch sicherheitsrelevanten Aspekten, ob eine Aktionsmethode, z. B. zur Authentifizierung oder zum Überprüfen der Eigenschaften der Anforderung ausgeführt.
> - **Aktion** Filter, der die aktionsmethodenausführung umschließt. Dieser Filter kann zusätzliche Verarbeitungsschritte, z. B. Bereitstellen zusätzlicher Daten für die Aktionsmethode, das Überprüfen des Rückgabewerts oder das Abbrechen der Ausführung der Aktionsmethode ausgeführt.
> - **Ergebnis** Filter, der Ausführung der ActionResult-Objekt umschließt. Dieser Filter kann zusätzliche Verarbeitung des Ergebnisses, z. B. Ändern der HTTP-Antwort ausführen.
> - **Ausnahme** Filter, der ausgeführt wird, wenn es eine nicht behandelte Ausnahme, die an einer beliebigen Stelle in der Aktionsmethode, die mit den Autorisierungsfilter beginnt und endet mit der Ausführung des Ergebnisses ausgelöst werden. Ausnahmefilter können für Aufgaben wie die Protokollierung oder Anzeigen einer Fehlerseite verwendet werden.
> 
> Weitere Informationen zu Filter-Anbietern finden Sie auf diesen Link MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Informationen zu MVC Music Store-Anwendung Protokollierung (Feature)

Diese Lösung Music Store hat eine neue Datentabelle Modell für die Website Protokollierung **ActionLog**, mit den folgenden Feldern: Name des Controllers, die eine Anforderung, die aufgerufene Aktion, die Client-IP und die Zeitstempel erhalten haben.

![Entity Data Model. ActionLog-Tabelle. ] (aspnet-mvc-4-custom-action-filters/_static/image1.png "Datenmodell. ActionLog-Tabelle.")

*Datenmodell - ActionLog-Tabelle*

Die Lösung bietet eine ASP.NET MVC-Ansicht für das Aktionsprotokoll, die am befinden **MvcMusicStores/Ansichten/ActionLog**:

![Aktion protokollsicht](aspnet-mvc-4-custom-action-filters/_static/image2.png "Aktionsprotokoll anzeigen")

*Aktion Protokollansicht*

Mit dieser Struktur erhält wird die gesamte Arbeit konzentriert sich werden, auf Anforderung des Controllers unterbrechen und die Protokollierung mit benutzerdefinierten Filtern ausführen.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Aufgabe 1: Erstellen eines benutzerdefinierten Filters zum Abfangen von einem Domänencontroller-Anforderung

In dieser Aufgabe erstellen Sie eine benutzerdefinierten Filter Attributklasse, die die protokollierungslogik enthalten soll. Erweitern Sie zu diesem Zweck ASP.NET-MVC **ActionFilterAttribute** Klasse und Implementieren der Schnittstelle **IActionFilter**.

> [!NOTE]
> Die **ActionFilterAttribute** ist die Basisklasse für die Attribut-Filter. Es bietet die folgenden Methoden, um eine bestimmte Logik nach und vor der Ausführung des Controlleraktion auszuführen:
> 
> - **OnActionExecuting**(ActionExecutingContext FilterContext): nur vor der Aktion wird aufgerufen.
> - **OnActionExecuted**(ActionExecutedContext FilterContext): Nachdem die Aktionsmethode aufgerufen wurde und bevor das Ergebnis (vor dem Rendern der Ansicht) ausgeführt wird.
> - **OnResultExecuting**(ResultExecutingContext FilterContext): unmittelbar vor dem Ausführen des Ergebnisses (vor dem Rendern der Ansicht).
> - **OnResultExecuted**(ResultExecutedContext FilterContext): nach dem Ausführen des Ergebnisses (nach dem Rendern der Ansicht).
> 
> Durch das Überschreiben einer dieser Methoden in einer abgeleiteten Klasse, können Sie Ihren eigenen Filter Code ausführen.


1. Öffnen der **beginnen** Lösung finden Sie unter **\Source\Ex01-LoggingActions\Begin** Ordner.

    1. Sie müssen einige fehlende NuGet-Pakete herunterladen, bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
    > 
    > Weitere Informationen finden Sie im Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Fügen Sie eine neue C#-Klasse in der **Filter** Ordner und nennen Sie sie *CustomActionFilter.cs*. In diesem Ordner werden die benutzerdefinierten Filter gespeichert.
3. Open **CustomActionFilter.cs** und Hinzufügen eines Verweises auf **System.Web.Mvc** und **MvcMusicStore.Models** Namespaces:

    (Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Erben der **CustomActionFilter** -Klasse aus **ActionFilterAttribute** und stellen Sie dann **CustomActionFilter** Klasse implementieren **IActionFilter** Schnittstelle.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Stellen Sie **CustomActionFilter** Klasse überschreiben Sie die Methode **OnActionExecuting** und fügen Sie die erforderliche Logik zum Protokollieren der Ausführung der Filter hinzu. Zu diesem Zweck fügen Sie den folgenden hervorgehobenen Code innerhalb **CustomActionFilter** Klasse.

    (Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** Methode ist die Verwendung **Entity Framework** zum Hinzufügen eines neuen ActionLog-Registers. Erstellt und füllt Sie mit die Kontextinformationen über eine neue Entitätsinstanz **FilterContext**.
    > 
    > Sie können erfahren Sie mehr über **ControllerContext** am-Klasse [Msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Aufgabe 2: Räumen einen Code-Interceptor in die Speicher-Controller-Klasse

In dieser Aufgabe fügen Sie den benutzerdefinierten Filter von Räumen es auf alle Controllerklassen und Controlleraktionen, die protokolliert werden. Im Rahmen dieser Übung wird die Store-Controllerklasse ein Protokoll haben.

Die Methode **OnActionExecuting** aus **ActionLogFilterAttribute** benutzerdefinierter Filter ausgeführt wird, wenn ein eingefügten Element aufgerufen wird.

Es ist auch möglich, eine bestimmte Controllermethode abzufangen.

1. Öffnen der **StoreController** am **MvcMusicStore\Controllers** und fügen einen Verweis auf die **Filter** Namespace:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Einfügen von benutzerdefinierten Filter **CustomActionFilter** in **StoreController** Klasse durch Hinzufügen von **[CustomActionFilter]** Attribut vor der Klassendeklaration.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > Wenn Sie ein Filter in einer Controllerklasse eingefügt wird, werden auch alle zugehörigen Aktionen eingefügt. Wenn Sie den Filter nur für eine Reihe von Aktionen anwenden möchten, müssten Sie einfügen **[CustomActionFilter]** jeweils davon:
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Aufgabe 3: Ausführen der Anwendung

In dieser Aufgabe testen Sie, dass die Protokollierungsfilter arbeitet. Sie startet die Anwendung und den Store besuchen, und klicken Sie dann prüft protokollierte Aktivitäten.

1. Drücken Sie **F5**, um die Anwendung auszuführen.
2. Navigieren Sie zu **/ActionLog** zum ersten Protokoll Ansichtszustand finden Sie unter:

    ![Protokollstatus Tracker vor der Aktivität "Seite"](aspnet-mvc-4-custom-action-filters/_static/image3.png "Tracker Protokollstatus vor der Aktivität "Seite"")

    *Protokoll Tracker Status vor der Aktivität "Seite"*

    > [!NOTE]
    > Standardmäßig wird immer ein Element angezeigt, die generiert wird, wenn Sie die vorhandenen Genres für das Menü abrufen.
    > 
    > Aus Gründen der Einfachheit halber haben wir Sie Bereinigen der **ActionLog** Tabelle jedes Mal die Anwendung ausgeführt wird, damit sie nur die Protokolle der Überprüfung für jede bestimmte Aufgabe angezeigt werden.
    > 
    > Sie müssen unter Umständen entfernen den folgenden Code aus der **Sitzung\_starten** Methode (in der **"Global.asax"** Klasse), um ein Verlaufsprotokoll für alle Aktionen, die ausgeführt werden, in den Speicher zu speichern Controller.
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.
4. Navigieren Sie zu **/ActionLog** und wenn das Protokoll leer drücken ist **F5** auf der Seite zu aktualisieren. Überprüfen Sie, dass Ihre Besuche nachverfolgt wurden:

    ![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image4.png "Aktionsprotokoll mit Aktivitäten protokolliert")

    *Aktionsprotokoll mit Aktivitäten protokolliert*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Übung 2: Verwalten mehrerer Aktionsfilter

In dieser Übung werden fügen Sie eine zweite Custom Action-Filter auf die Klasse StoreController und definieren die Reihenfolge, in der beide Filter ausgeführt werden. Klicken Sie dann aktualisieren Sie den Code, um den Filter global zu registrieren.

Sie haben verschiedene Optionen, die beim Definieren der Filterliste Ausführungsreihenfolge berücksichtigen. Z. B. die Order-Eigenschaft und die Filter Bereich:

Sie können definieren, eine **Bereich** für jeden dieser Filter, z. B. Sie konnte den Bereich der Aktionsfilter für die Ausführung innerhalb der **Controller Bereich**, und alle Autorisierungsfilter für die Ausführung im **globalen Gültigkeitsbereich** . Die Bereiche haben eine definierte Ausführungsreihenfolge.

Jeder Aktionsfilter verfügt darüber hinaus eine Order-Eigenschaft, die verwendet wird, um zu bestimmen, die Ausführungsreihenfolge im Bereich des Filters.

Weitere Informationen zu Ausführungsreihenfolge benutzerdefinierte Aktionsfilter verwendet werden, finden Sie auf diese MSDN-Artikel: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Aufgabe 1: Erstellen eines neuen benutzerdefinierten Aktionsfilters

In dieser Aufgabe erstellen Sie eine neue benutzerdefinierte Aktionsfilter in der Klasse StoreController einfügen lernen, wie die Ausführungsreihenfolge der Filter zu verwalten.

1. Öffnen der **beginnen** Lösung finden Sie unter **\Source\Ex02-ManagingMultipleActionFilters\Begin** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

    1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

        > [!NOTE]
        > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
        > 
        > Weitere Informationen finden Sie im Artikel: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Fügen Sie eine neue C#-Klasse in der **Filter** Ordner und nennen Sie sie *MyNewCustomActionFilter.cs*
3. Open **MyNewCustomActionFilter.cs** und Hinzufügen eines Verweises auf **System.Web.Mvc** und **MvcMusicStore.Models** Namespace:

    (Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Ersetzen Sie die Standard-Klassendeklaration durch den folgenden Code.

    (Codeausschnitt - *benutzerdefinierte Aktionsfilter ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Diese benutzerdefinierte Aktionsfilter entspricht fast dem als die Version, die Sie in der vorherigen Übung erstellt haben. Der Hauptunterschied besteht darin, dass sie hat die  *&quot;protokolliert von&quot;*  Attribut aktualisiert, die durch diese neue Art Namen zum Identifizieren des Filters eingetragen werden sollen, registriert das Protokoll.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Aufgabe 2: Räumen einen neuen Code Interceptor in die StoreController-Klasse

In dieser Aufgabe erstellen Sie fügen einen neuen benutzerdefinierten Filter in die Klasse StoreController und führen Sie die Projektmappe, um zu überprüfen, wie beide Filter zusammenarbeiten.

1. Öffnen der **StoreController** Klasse finden Sie unter **MvcMusicStore\Controllers** und Einfügen der neuen benutzerdefinierten Filter **MyNewCustomActionFilter** in  **StoreController** -Klasse wie im folgenden Code gezeigt wird.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Führen Sie nun die Anwendung aus, um zu sehen, wie diese zwei benutzerdefinierte Aktionsfilter funktionieren. Drücken Sie die zu diesem Zweck **F5** und warten Sie, bis die Anwendung gestartet wird.
3. Navigieren Sie zu **/ActionLog** zum ersten Protokoll Ansichtszustand finden Sie unter.

    ![Protokollstatus Tracker vor der Aktivität "Seite"](aspnet-mvc-4-custom-action-filters/_static/image5.png "Tracker Protokollstatus vor der Aktivität "Seite"")

    *Protokoll Tracker Status vor der Aktivität "Seite"*
4. Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.
5. Überprüfen Sie, ob dieses Mal; Ihre Besuche zweimal nachverfolgt wurden: für jede der benutzerdefinierten Aktionsfilter im hinzugefügten der **StorageController** Klasse.

    ![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image6.png "Aktionsprotokoll mit Aktivitäten protokolliert")

    *Aktionsprotokoll mit Aktivitäten protokolliert*
6. Schließen Sie den Browser.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Aufgabe 3: Verwalten von Filterreihenfolge

In dieser Aufgabe erfahren Sie, wie die Filter Ausführungsreihenfolge zu verwalten, indem Sie die Order-Eigenschaft.

1. Öffnen der **StoreController** Klasse finden Sie unter **MvcMusicStore\Controllers** , und geben Sie die **Reihenfolge** Eigenschaft in beide Filter wie unten dargestellt.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Überprüfen Sie nun, wie die Filter ausgeführt werden, je nach Wert für die Order-Eigenschaft. Sie finden, die den Filter mit dem kleinsten Wert der Bestellung (**CustomActionFilter**) ist das erste Schema an, die ausgeführt wird. Drücken Sie **F5** und warten Sie, bis die Anwendung gestartet wird.
3. Navigieren Sie zu **/ActionLog** zum ersten Protokoll Ansichtszustand finden Sie unter.

    ![Protokollstatus Tracker vor der Aktivität "Seite"](aspnet-mvc-4-custom-action-filters/_static/image7.png "Tracker Protokollstatus vor der Aktivität "Seite"")

    *Protokoll Tracker Status vor der Aktivität "Seite"*
4. Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.
5. Überprüfen Sie, dass dieses Mal Ihre Besuche nachverfolgt wurden nach der Filterliste Reihenfolgenwert sortiert: **CustomActionFilter** Protokolle erste.

    ![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image8.png "Aktionsprotokoll mit Aktivitäten protokolliert")

    *Aktionsprotokoll mit Aktivitäten protokolliert*
6. Jetzt, Sie Reihenfolgenwert der Filterliste aktualisieren und überprüfen Sie, wie sich die Reihenfolge der Protokollierung ändert. In der **StoreController** Klasse, zum Aktualisieren der Filterliste Reihenfolgenwert wie unten gezeigt.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Führen Sie die Anwendung erneut durch Drücken von **F5**.
8. Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.
9. Überprüfen Sie, dass dieses Mal erstellt die Protokolle mit **MyNewCustomActionFilter** Filter wird zuerst angezeigt.

    ![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image9.png "Aktionsprotokoll mit Aktivitäten protokolliert")

    *Aktionsprotokoll mit Aktivitäten protokolliert*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Aufgabe 4: Registrieren von Global filtert

In dieser Aufgabe aktualisieren Sie die Lösung, um den neuen Filter zu registrieren (**MyNewCustomActionFilter**) als globaler Filter. Dadurch wird es durch alle die Aktionen ausgeführt, in der Anwendung und nicht nur auf die StoreController diejenigen wie in der vorherigen Aufgabe ausgelöst werden.

1. In **StoreController** -Klasse, entfernen Sie **[MyNewCustomActionFilter]** Attribut und der Order-Eigenschaft von **[CustomActionFilter]**. Es sollte wie folgt aussehen:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Open **"Global.asax"** Datei, und suchen Sie die **Anwendung\_starten** Methode. Beachten Sie, dass bei jedem der Anwendung es Start durch Aufrufen der globalen Filter registrieren **RegisterGlobalFilters** Methode innerhalb **FilterConfig** Klasse.

    ![Globale Filter in Global.asax registriert](aspnet-mvc-4-custom-action-filters/_static/image10.png "globale Filter in Global.asax registriert")

    *Globale Filter registriert in Global.asax*
3. Open **FilterConfig.cs** innerhalb **App\_starten** Ordner.
4. Fügen Sie einen Verweis auf System.Web.Mvc verwenden; Verwenden MvcMusicStore.Filters; Namespace.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Update **RegisterGlobalFilters** Methode des benutzerdefinierten Filters hinzufügen. Zu diesem Zweck fügen Sie den hervorgehobenen Code hinzu:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Führen Sie die Anwendung durch Drücken von **F5**.
7. Klicken Sie auf eines der **Genres** aus dem Menü und einige Aktionen vorhanden ist, wie z. B. Browsen verfügbaren Album ausführen.
8. Überprüfen Sie, die nun **[MyNewCustomActionFilter]** wird in HomeController und ActionLogController zu eingefügt wird.

    ![Aktionsprotokoll mit Aktivitäten protokolliert](aspnet-mvc-4-custom-action-filters/_static/image11.png "Aktionsprotokoll mit Aktivitäten protokolliert")

    *Aktionsprotokoll mit globalen Aktivitäten protokolliert*

> [!NOTE]
> Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch diese praktische Übungseinheit haben Sie gelernt erweitert einen Aktionsfilter, um benutzerdefinierte Aktionen auszuführen. Sie haben auch gelernt, wie ein beliebiger anzuwendender Filter auf Ihren Domänencontrollern Seite einfügen. Es wurden die folgenden Konzepte verwendet:

- Vorgehensweise: Erstellen von benutzerdefinierten Aktionsfiltern mit der ASP.NET MVC ActionFilterAttribute-Klasse
- Gewusst wie: Einfügen von Filtern in ASP.NET MVC-Controller.
- Verwalten von Filtern Sortierung mit der Order-Eigenschaft
- Vorgehensweise beim Registrieren von Global filtern

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [Https://go.microsoft.com/? Linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; *Visual Studio Express 2012 für das Web mit Windows Azure SDK*&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](aspnet-mvc-4-custom-action-filters/_static/image12.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

In diesem Anhang wird gezeigt, wie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und veröffentlichen Sie die Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Windows Azure bereitgestellt werden.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal

1. Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Sie können mit Windows Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** in der Befehlszeile.

    ![Erstellen einer neuen Website](aspnet-mvc-4-custom-action-filters/_static/image18.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **berechnen** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können. Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen. Es umfasst nicht die Schritte zum Einrichten einer Datenbank.

    ![Erstellen eine neue Website, die mit der Schnellerfassung](aspnet-mvc-4-custom-action-filters/_static/image19.png "erstellen eine neue Website, die mit der Schnellerfassung")

    *Erstellen eine neue Website, die mit der Schnellerfassung*
4. Warten Sie, bis die neue **Website** wird erstellt.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website funktioniert.

    ![Um die neue Website durchsuchen](aspnet-mvc-4-custom-action-filters/_static/image20.png "durchsuchen, um die neue Website")

    *Um die neue Website durchsuchen*

    ![Website](aspnet-mvc-4-custom-action-filters/_static/image21.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](aspnet-mvc-4-custom-action-filters/_static/image22.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](aspnet-mvc-4-custom-action-filters/_static/image23.png "der Website herunterladen eines Veröffentlichungsprofils")

    *Die Website herunterladen eines Veröffentlichungsprofils*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein. In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](aspnet-mvc-4-custom-action-filters/_static/image24.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: konfigurieren den Datenbankserver

Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen. Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.

    ![SQL-Datenbank-Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL-Datenbank-Dashboard")

    *SQL-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**. Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    Client-IP-Adresse hinzufügen
3. Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    Bestätigen von Änderungen

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

1. Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](aspnet-mvc-4-custom-action-filters/_static/image29.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren des Veröffentlichungsprofils](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.

    ![Überprüfen der Verbindung](aspnet-mvc-4-custom-action-filters/_static/image31.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

    - In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.
    - In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.
    - In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.
    - Geben Sie einen neuen Datenbanknamen ein.

    ![Konfigurieren von Zielverbindungszeichenfolge](aspnet-mvc-4-custom-action-filters/_static/image33.png "Zielverbindungszeichenfolge konfigurieren")

    *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.

    ![Erstellen der Datenbank](aspnet-mvc-4-custom-action-filters/_static/image34.png "erstellen die Datenbank-Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungszeichenfolge zur SQL-Datenbank zeigt](aspnet-mvc-4-custom-action-filters/_static/image35.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")

    *Verbindungszeichenfolge, die auf der SQL-Datenbank*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](aspnet-mvc-4-custom-action-filters/_static/image36.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Anhang C: Verwendung von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen. Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.

![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](aspnet-mvc-4-custom-action-filters/_static/image37.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")

*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*

***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.
2. Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.
4. Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.

![Geben Sie den Namen des Ausschnitts](aspnet-mvc-4-custom-action-filters/_static/image38.png "starten Sie den Namen des Ausschnitts eingeben")

*Starten Sie den Namen des Ausschnitts eingeben*

![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](aspnet-mvc-4-custom-action-filters/_static/image39.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")

*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*

![Drücken Sie erneut die Tab und den Ausschnitt erweitert](aspnet-mvc-4-custom-action-filters/_static/image40.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")

*Drücken Sie erneut die Tab und den Ausschnitt erweitert*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)*** 1. Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.

1. Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.
2. Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](aspnet-mvc-4-custom-action-filters/_static/image41.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](aspnet-mvc-4-custom-action-filters/_static/image42.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*
