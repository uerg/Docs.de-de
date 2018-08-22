---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.NET – Fehlerbehandlung | Microsoft-Dokumentation
author: Erikre
description: Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: ed5d7b9b4e61b0289734f4cdef1039b31ddda7a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827083"
---
<a name="aspnet-error-handling"></a>ASP.NET – Fehlerbehandlung
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die dieser tutorialreihe begleitet.


In diesem Tutorial ändern Sie die Wingtip Toys-beispielanwendung für die Fehlerbehandlung und Protokollierung von Anzeigefehlern enthalten. Fehlerbehandlung ermöglicht, dass die Anwendung ordnungsgemäß Behandeln von Fehlern und Meldungen entsprechend anzuzeigen. Fehlerprotokollierung können Sie zum Suchen und Beheben von Fehlern, die aufgetreten sind. Dieses Tutorial baut auf dem vorherigen Lernprogramm "URL-Routing" und ist Teil der tutorialreihe Wingtip Toys.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Informationen zum Hinzufügen von globalen Fehlerbehandlung, die in der Konfigurationsdatei der Anwendung.
- So fügen Sie auf der Anwendung, einen Codeebene und eine Seite für die Fehlerbehandlung.
- Informationen zum Fehler für eine spätere Prüfung protokolliert.
- Informationen zum Anzeigen von Fehlermeldungen, die nicht über die Sicherheit beeinträchtigen.
- Informationen zum Implementieren der Protokollierung von Anzeigefehlern Error Logging Modules und Handler (ELMAH)

## <a name="overview"></a>Übersicht

ASP.NET-Anwendungen müssen in der Lage, Fehler zu behandeln, die während der Ausführung konsistent auftreten. ASP.NET verwendet die common Language Runtime (CLR), ist die fehlerbenachrichtigung für Anwendungen auf einheitliche Weise eine Möglichkeit bietet. Wenn ein Fehler auftritt, wird eine Ausnahme ausgelöst. Eine Ausnahme ist, alle Fehler, Bedingung oder unerwartetem Verhalten, das eine Anwendung auftritt.

In .NET Framework stellt eine Ausnahme ein Objekt dar, das von der `System.Exception`-Klasse erbt. Eine Ausnahme wird in einem Codebereich ausgelöst, in dem ein Fehler aufgetreten ist. Die Ausnahme wird an einem Ort auf der Aufrufliste nach oben übergeben, in denen die Anwendung Code zur Behandlung von Ausnahmen bereitstellt. Wenn die Anwendung die Ausnahme nicht behandelt, wird der Browser erzwungen, um die Fehlerdetails anzuzeigen.

Als bewährte Methode, Behandeln von Fehlern in der Code-Ebene in `Try` / `Catch` / `Finally` Blöcke innerhalb des Codes. Versuchen Sie es, um diese Blöcke zu platzieren, damit der Benutzer Probleme im Zusammenhang mit korrigieren kann, in denen sie auftreten. Wenn die Fehlerbehandlung Blöcke sind zu weit weg aus, in dem der Fehler aufgetreten ist, wird es zunehmend schwieriger, Benutzern bereitstellen, mit den Informationen, die sie zum Beheben des Problems benötigen.

### <a name="exception-class"></a>Exception-Klasse

Die Exception-Klasse ist die Basisklasse, die von der Ausnahmen erben. Die meisten Exception-Objekte sind Instanzen einer abgeleiteten Klasse der Exception-Klasse, wie z. B. die `SystemException` -Klasse, die `IndexOutOfRangeException` -Klasse, oder die `ArgumentNullException` Klasse. Die Exception-Klasse verfügt über Eigenschaften, z. B. die `StackTrace` -Eigenschaft, die `InnerException` -Eigenschaft, und die `Message` Eigenschaft, die bestimmte Informationen zu diesem Fehler bereitstellen, die aufgetreten ist.

### <a name="exception-inheritance-hierarchy"></a>Ausnahme-Vererbungshierarchie

Die Runtime hat einen Basissatz von Ausnahmen, die von der `SystemException` -Klasse, die die Runtime löst, wenn eine Ausnahme aufgetreten ist. Die meisten Klassen, die von der Ausnahmeklasse, z. B. erben die `IndexOutOfRangeException` Klasse und die `ArgumentNullException` Klasse, implementieren keine zusätzlichen Member. Aus diesem Grund kann die wichtigste Informationen für eine Ausnahme in der Hierarchie von Ausnahmen, dem Namen der Ausnahme und die Informationen in die Ausnahme gefunden werden.

### <a name="exception-handling-hierarchy"></a>Behandlung von Ausnahmenhierarchie

In einer ASP.NET Web Forms-Anwendung können Ausnahmen behandelt werden basierend auf einer Hierarchie für die einzelnen zu behandeln. Eine Ausnahme kann auf folgenden Ebenen behandelt werden:

- Anwendungsebene
- Auf Seitenebene
- Codeebene

Wenn eine Anwendung Ausnahmen behandelt, kann, die dem Benutzer zusätzliche Informationen über die Ausnahme, die von der Ausnahmeklasse geerbt wird häufig abgerufen und angezeigt werden. Zusätzlich zur Anwendung, Seiten- und Codeebene können Sie auch Ausnahmen auf der Ebene der HTTP-Modul und mithilfe einer benutzerdefinierten IIS-Handlers behandeln.

### <a name="application-level-error-handling"></a>Anwendung auf Fehlerbehandlung

Sie Standard-Fehler auf Anwendungsebene behandeln, entweder durch Ändern der Konfiguration Ihrer Anwendung oder durch Hinzufügen einer `Application_Error` Ereignishandler in der *"Global.asax"* Datei der Anwendung.

Sie können die Standard-Fehler und HTTP-Fehler behandeln, durch das Hinzufügen einer `customErrors` Abschnitt der *"Web.config"* Datei. Die `customErrors` Abschnitt können Sie eine Standardseite angeben, die Benutzer umgeleitet werden, wenn ein Fehler auftritt. Sie können auch einzelne Seiten für Fehler in bestimmten Status Code angeben.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Wenn Sie die Konfiguration verwenden, um den Benutzer zu einer anderen Seite umzuleiten, müssen Sie nicht die Details des Fehlers, die aufgetreten ist.

Sie können jedoch Fehler, die an einer beliebigen Stelle auftreten in Ihrer Anwendung abfangen, indem Sie Code zum Hinzufügen der `Application_Error` Ereignishandler in der *"Global.asax"* Datei.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Ereignis-Fehlerbehandlung auf Seitenebene

Ein Handler auf Seitenebene gibt den Benutzer zur Seite zurück, wo der Fehler aufgetreten ist, aber da keine Instanzen von Steuerelementen verwaltet werden, es werden nicht mehr alles auf der Seite. Um die Fehlerdetails für dem Benutzer der Anwendung bereitzustellen, müssen Sie die Fehlerdetails speziell auf die Seite schreiben.

Sie würden einen Fehler auf Seitenebene-Handler in der Regel verwenden, um nicht behandelte Fehler zu protokollieren oder den Benutzer auf einer Seite zu leiten, die nützliche Informationen anzeigen können.

Dieses Codebeispiel zeigt einen Handler für die Error-Ereignis in einer ASP.NET-Webseite an. Dieser Handler fängt alle Ausnahmen, die nicht bereits in behandelt werden `try` / `catch` auf der Seite blockiert.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Nachdem Sie einen Fehler zu behandeln, müssen Sie es löschen, durch den Aufruf der `ClearError` Methode des Serverobjekts (`HttpServerUtility` Klasse), andernfalls sehen Sie einen zuvor aufgetretenen Fehler.

### <a name="code-level-error-handling"></a>Fehlerbehandlung auf Code

Die Try-Catch-Anweisung besteht aus einem Try-Block gefolgt von einem oder mehr catch-Klauseln, die Handler für verschiedene Ausnahmen angeben. Wenn eine Ausnahme ausgelöst wird, sucht die common Language Runtime (CLR) die Catch-Anweisung, die diese Ausnahme behandelt. Wenn die derzeit ausgeführte Methode nicht mit einen Catch-Block enthält, sucht die CLR, bei der Methode, die die aktuelle Methode und So weiter, der Aufrufliste aufgerufen. Wenn kein Catch-Block gefunden wird, klicken Sie dann die CLR dem Benutzer eine Nachricht nicht behandelte Ausnahme angezeigt, und beendet die Ausführung des Programms.

Das folgende Codebeispiel zeigt eine gängige Methode der Verwendung von `try` / `catch` / `finally` um Fehler zu behandeln.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Im obigen Code enthält der Try-Block den Code, der für eine mögliche Ausnahme geschützt werden muss. Der Block wird ausgeführt, bis eine Ausnahme ausgelöst oder der Block erfolgreich abgeschlossen wurde. Wenn entweder eine `FileNotFoundException` Ausnahme oder ein `IOException` Ausnahme auftritt, wird die Ausführung zu einer anderen Seite übertragen. Klicken Sie dann den Code in den die finally-Block ausgeführt, ob ein Fehler aufgetreten, oder nicht ist.

## <a name="adding-error-logging-support"></a>Hinzufügen von Unterstützung für Fehler protokollieren

Vor dem Hinzufügen der Wingtip Toys-beispielanwendung für die Fehlerbehandlung, fügen Sie fehlerprotokollierung Unterstützung durch das Hinzufügen einer `ExceptionUtility` -Klasse auf die *Logik* Ordner. Auf diese Weise, jedes Mal, die die Anwendung einen Fehler behandelt werden die Protokolldatei die Fehlerdetails hinzugefügt werden.

1. Mit der rechten Maustaste die *Logik* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Code** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie dann **Klasse**aus der Mitte aus, und nennen Sie sie **ExceptionUtility.cs**.
3. Wählen Sie **Hinzufügen** aus. Die neue Klassendatei wird angezeigt.
4. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Wenn eine Ausnahme auftritt, kann die Ausnahme in einer Protokolldatei für die Ausnahme geschrieben werden, durch Aufrufen der `LogException` Methode. Diese Methode akzeptiert zwei Parameter, der Exception-Objekt und eine Zeichenfolge, die Details der Quelle der Ausnahme enthält. Das Ausnahmeprotokoll wird geschrieben, um die *ErrorLog.txt* Datei die *App\_Daten* Ordner.

### <a name="adding-an-error-page"></a>Eine Fehlerseite hinzufügen

In der Wingtip Toys-beispielanwendung wird eine Seite verwendet werden, um Fehler anzuzeigen. Die Fehlerseite soll Benutzern der Website eine sichere Fehlermeldung angezeigt. Allerdings werden zusätzliche Fehlerdetails, wenn der Benutzer ein Entwickler ist, erstellen eine HTTP-Anforderung, die lokal auf dem Computer bereitgestellt werden, wo befindet sich der Code, auf der Fehlerseite angezeigt.

1. Mit der rechten Maustaste in des Namens des Projekts (**Wingtip Toys**) in **Projektmappen-Explorer** , und wählen Sie **hinzufügen**  - &gt; **neues Element**.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie in der mittleren Liste **Webformular mit Gestaltungsvorlage**, und nennen Sie sie **ErrorPage.aspx**.
3. Klicken Sie auf **Hinzufügen**.
4. Wählen Sie die *Site.Master* als Masterseite Datei, und wählen Sie dann **OK**.
5. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Ersetzen Sie den Code des Code-Behind (*ErrorPage.aspx.cs*), damit es wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Wenn die Seite "Fehler" angezeigt wird, die `Page_Load` -Ereignishandler ausgeführt wird. In der `Page_Load` Handler auf, richtet sich der Speicherort der, in dem der Fehler zuerst behandelt wurde. Klicken Sie dann die zuletzt aufgetretenen wird bestimmt durch Aufruf der `GetLastError` Methode des Serverobjekts. Wenn die Ausnahme nicht mehr vorhanden ist, wird eine generische Ausnahme erstellt. Klicken Sie dann, wenn die HTTP-Anforderung lokal erstellt wurde, werden alle Fehlerdetails angezeigt. In diesem Fall wird nur der lokale Computer, denen die Webanwendung diese Fehlerdetails angezeigt. Nach der die Fehlerinformationen angezeigt wurde, wird der Fehler wird in die Protokolldatei hinzugefügt, und der Fehler wird vom Server gelöscht.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Anzeigen von nicht behandelten Fehlermeldungen für die Anwendung

Durch das Hinzufügen einer `customErrors` Abschnitt der *"Web.config"* -Datei kann schnell einfache Fehler behandelt, die in der gesamten Anwendung auftreten. Sie können auch Gewusst wie: Behandeln von Fehlern, die basierend auf ihren Code-Statuswerte, wie z. B. 404 - Datei wurde nicht gefunden angeben.

#### <a name="update-the-configuration"></a>Aktualisieren Sie die Konfiguration

Aktualisieren Sie die Konfiguration durch Hinzufügen einer `customErrors` im Abschnitt zu den *"Web.config"* Datei.

1. In **Projektmappen-Explorer**suchen oder öffnen Sie die *"Web.config"* Datei im Stammverzeichnis der Wingtip Toys-beispielanwendung.
2. Hinzufügen der `customErrors` im Abschnitt zu den *"Web.config"* Datei innerhalb der `<system.web>` Knoten wie folgt:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Speichern Sie die *"Web.config"* Datei.

Die `customErrors` Abschnitt gibt den Modus an, die auf "On" festgelegt ist. Es gibt auch an die `defaultRedirect`, die weist die Anwendung auf der Seite zu navigieren, wenn ein Fehler auftritt. Darüber hinaus haben Sie eine bestimmte Error-Element hinzugefügt, der angibt, wie Sie einen 404-Fehler zu behandeln, wenn eine Seite nicht gefunden wird. Weiter unten in diesem Tutorial fügen Sie, dass zusätzliche Fehlerbehandlung, die die Details eines Fehlers auf der Anwendungsebene erfasst.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung nun auf die aktualisierte Routen finden Sie unter ausführen.

1. Drücken Sie **F5** zum Ausführen der Wingtip Toys-beispielanwendung.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Geben Sie die folgende URL in den Browser (verwenden Sie **Ihre** Portnummer):  
    `https://localhost:44300/NoPage.aspx`
3. Überprüfen Sie die *ErrorPage.aspx* im Browser angezeigt. 

    ![ASP.NET – Fehlerbehandlung - Seite nicht gefunden.](aspnet-error-handling/_static/image1.png)

Wenn Sie anfordern, die *NoPage.aspx* Seite, die nicht vorhanden ist, die Fehlerseite zeigt die einfache Fehlermeldung und die detaillierten Fehlerinformationen, wenn Detailinformationen zur Verfügung stehen. Wenn der Benutzer eine nicht existierende-Seite von einem Remotestandort aus angefordert, würde jedoch die Fehlermeldung von die Fehlerseite nur in Rot angezeigt.

### <a name="including-an-exception-for-testing-purposes"></a>Auch für eine Ausnahme für Tests verwenden

Um zu überprüfen, wie die Anwendung funktioniert, wenn ein Fehler auftritt, können Sie absichtlich fehlerbedingungen in ASP.NET erstellen. In der Wingtip Toys-beispielanwendung löst Sie eine testausnahme beim Laden der Standardseite, um festzustellen, was passiert.

1. Öffnen Sie den Code-Behind von der *"default.aspx"* Seite in Visual Studio.   
   Die *"default.aspx.cs"* Code-Behind-Seite wird angezeigt.
2. In der `Page_Load` Handler auf, fügen Sie Code hinzu, damit der Ereignishandler wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Es ist möglich, die verschiedenen Typen von Ausnahmen zu erstellen. Im obigen Code erstellen Sie eine `InvalidOperationException` bei der *"default.aspx"* Seite wird geladen.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können angeben, Ausführen der Anwendung aus, um festzustellen, wie die Anwendung für die Ausnahme behandelt.

1. Drücken Sie **STRG + F5** zum Ausführen der Wingtip Toys-beispielanwendung.  
 Die Anwendung löst eine "InvalidOperationException" aus. 

    > [!NOTE] 
    > 
    > Drücken Sie **STRG + F5** um die Seite anzuzeigen, ohne in den Code, um die Quelle des Fehlers in Visual Studio anzuzeigen.
2. Überprüfen Sie die *ErrorPage.aspx* im Browser angezeigt. 

    ![ASP.NET – Fehlerbehandlung - Fehler (Seite)](aspnet-error-handling/_static/image2.png)

Wie Sie in den Fehlerdetails sehen können, wurde die Ausnahme aufgefangen, durch die `customError` im Abschnitt der *"Web.config"* Datei.

### <a name="adding-application-level-error-handling"></a>Hinzufügen der Fehlerbehandlung auf Anwendungsebene

Anstatt Trap, die die Ausnahme mit der `customErrors` im Abschnitt der *"Web.config"* Datei, wenn Sie wenig Informationen über die Ausnahme erhalten, können Sie das Abfangen des Fehlers auf der Anwendungsebene und Abrufen von Fehlerdetails.

1. In **Projektmappen-Explorer**suchen oder öffnen Sie die *"Global.asax.cs"* Datei.
2. Hinzufügen einer **Anwendung\_Fehler** Ereignishandler, sodass er wie folgt angezeigt wird:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Tritt ein Fehler in der Anwendung die `Application_Error` Handler wird aufgerufen. In diesem Handler ist die letzte Ausnahme abgerufen und überprüft. Wenn die Ausnahme nicht behandelt wurde und die Ausnahme enthält die Details der inneren Ausnahme (d. h. `InnerException` ist nicht null), die Anwendung überträgt die Ausführung auf die Fehlerseite, in dem die Details der Ausnahme angezeigt werden.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung den zusätzlichen Fehlerinformationen, die von der Behandlung der Ausnahme auf Anwendungsebene bereitgestellt finden Sie unter ausführen.

1. Drücken Sie **STRG + F5** zum Ausführen der Wingtip Toys-beispielanwendung.  
 Die Anwendung löst das `InvalidOperationException` .
2. Überprüfen Sie die *ErrorPage.aspx* im Browser angezeigt. 

    ![ASP.NET – Fehlerbehandlung - Anwendungsfehler-Ebene](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Fehlerbehandlung auf Seitenebene hinzufügen

Sie können auf eine Seite Fehlerbehandlung auf Seitenebene hinzufügen, entweder mithilfe von Hinzufügen einer `ErrorPage` -Attribut auf die `@Page` -Direktive der Seite, oder indem Sie eine `Page_Error` -Ereignishandler zum Code-Behind einer Seite. In diesem Abschnitt fügen Sie eine `Page_Error` -Ereignishandler, die der Ausführung übertragen, wird die *ErrorPage.aspx* Seite.

1. In **Projektmappen-Explorer**suchen oder öffnen Sie die *"default.aspx.cs"* Datei.
2. Hinzufügen einer `Page_Error` Handler folgt, sodass das Code-Behind als angezeigt wird:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Tritt ein Fehler auf der Seite die `Page_Error` Ereignishandler wird aufgerufen. In diesem Handler ist die letzte Ausnahme abgerufen und überprüft. Wenn ein `InvalidOperationException` auftritt, die `Page_Error` Ereignishandler überträgt die Ausführung auf die Fehlerseite, in dem die Details der Ausnahme angezeigt werden.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung nun auf die aktualisierte Routen finden Sie unter ausführen.

1. Drücken Sie **STRG + F5** zum Ausführen der Wingtip Toys-beispielanwendung.  
 Die Anwendung löst das `InvalidOperationException` .
2. Überprüfen Sie die *ErrorPage.aspx* im Browser angezeigt. 

    ![ASP.NET – Fehlerbehandlung - Fehler auf der Seite](aspnet-error-handling/_static/image4.png)
3. Schließen Sie Ihr Browserfenster.

### <a name="removing-the-exception-used-for-testing"></a>Entfernen die Ausnahme, die zum Testen verwendet

Damit wird die Wingtip Toys-beispielanwendung für die Funktion ohne das Auslösen der Ausnahme, die Sie zuvor in diesem Lernprogramm hinzugefügt haben, entfernen Sie die Ausnahme.

1. Öffnen Sie den Code-Behind von der *"default.aspx"* Seite.
2. In der `Page_Load` Handler auf, entfernen Sie den Code, der die Ausnahme auslöst, damit der Ereignishandler wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Hinzufügen der Fehlerprotokollierung von Code auf

Wie weiter oben in diesem Tutorial erwähnt, können Sie Try/Catch-Anweisungen aus, um einen Codeabschnitt ausführen und den ersten Fehler, der auftritt, versuchen hinzufügen. In diesem Beispiel nur schreiben die Fehlerdetails in die Fehlerprotokolldatei Sie damit, dass der Fehler später überprüft werden kann.

1. In **Projektmappen-Explorer**in die *Logik* Ordner suchen und öffnen Sie die *PayPalFunctions.cs* Datei.
2. Update der `HttpCall` Methode, damit sieht der Code wie folgt:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Im obigen Code wird die `LogException` -Methode, die in enthalten ist das `ExceptionUtility` Klasse. Sie hinzugefügt haben die *ExceptionUtility.cs* Klassendatei mit dem *Logik* Ordner weiter oben in diesem Tutorial. Die `LogException`-Methode nimmt zwei Parameter an. Der erste Parameter ist das Ausnahmeobjekt. Der zweite Parameter ist eine Zeichenfolge verwendet, um die Ursache des Fehlers erkennen.

### <a name="inspecting-the-error-logging-information"></a>Überprüfen die Fehlerinformationen für die Protokollierung

Wie bereits erwähnt, können Sie das Fehlerprotokoll, um zu bestimmen, welche Fehler in Ihrer Anwendung zuerst behoben werden sollen. Natürlich werden nur die Fehler, die erfasst und in das Fehlerprotokoll geschrieben wurden aufgezeichnet.

1. In **Projektmappen-Explorer**suchen oder öffnen Sie die *ErrorLog.txt* Datei die *App\_Daten* Ordner.   
 Müssen Sie möglicherweise wählen Sie die "**alle Dateien anzeigen**" Option oder die "**aktualisieren**" Option am oberen Rand **Projektmappen-Explorer** , finden Sie unter den *ErrorLog.txt*Datei.
2. Überprüfen Sie das Fehlerprotokoll in Visual Studio angezeigt: 

    ![ASP.NET – Fehlerbehandlung - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Sicherer Fehlermeldungen

Es ist **beachten** , dass Fehlermeldungen angezeigt, es keine Informationen enthalten sollen, die ein böswilliger Benutzer in Ihrer Anwendung angreift hilfreich sein kann. Beispielsweise wenn Ihre Anwendung in einer Datenbank zu schreiben, im erfolglos versucht, sollen sie keine Fehlermeldung angezeigt werden, die den Benutzernamen enthält, die, den es verwendet wird. Aus diesem Grund wird eine generische Fehlermeldung in Rot, die dem Benutzer angezeigt. Alle zusätzliche Fehlerdetails werden nur für den Entwickler auf dem lokalen Computer angezeigt.

## <a name="using-elmah"></a>Verwenden von ELMAH

ELMAH (Error Logging Modules and Handlers) ist ein Fehler die Protokollierungsfunktion, die Sie Ihrer ASP.NET-Anwendung als NuGet-Paket nutzen. ELMAH bietet die folgenden Funktionen:

- Die Protokollierung von nicht behandelten Ausnahmen.
- Eine Webseite an das gesamte Protokoll der umcodierten nicht behandelten Ausnahmen.
- Eine Webseite zum Anzeigen der vollständigen Details der einzelnen protokolliert die Ausnahme.
- Eine e-Mail-Benachrichtigung der einzelnen Fehler zu dem Zeitpunkt, der es auftritt.
- Ein RSS-feed aus dem Protokoll der letzten 15 Fehler.

Bevor Sie das ELMAH arbeiten können, müssen Sie es installieren. Dies lässt sich leicht mithilfe der *NuGet* Installer-Paket. Wie weiter oben in dieser tutorialreihe erwähnt, ist NuGet Visual Studio-Erweiterung, die zum Installieren und Aktualisieren von open-Source-Bibliotheken und Tools in Visual Studio erleichtert.

1. In Visual Studio aus der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**  - &gt; **NuGet-Pakete für Projektmappe verwalten**. 

    ![ASP.NET – Fehlerbehandlung - NuGet-Pakete für Projektmappe verwalten](aspnet-error-handling/_static/image6.png)
2. Die **NuGet-Pakete verwalten** wird das Dialogfeld in Visual Studio angezeigt.
3. In der **NuGet-Pakete verwalten** Dialogfeld erweitern Sie **Online** auf der linken Seite, und wählen Sie dann **nuget.org**. Klicken Sie dann, suchen und Installieren der **ELMAH** Paket aus der Liste der online verfügbaren Pakete. 

    ![ASP.NET – Fehlerbehandlung - ELMA NuGet-Paket](aspnet-error-handling/_static/image7.png)
4. Sie benötigen eine Internetverbindung zum Herunterladen des Pakets.
5. In der **Projekte auswählen** Dialogfeld stellen Sie sicher, dass die **WingtipToys** Auswahl ausgewählt ist, und klicken Sie dann auf **OK**. 

    ![ASP.NET – Fehlerbehandlung - Projekte, Dialogfeld "auswählen"](aspnet-error-handling/_static/image8.png)
6. Klicken Sie auf **schließen** in **der NuGet-Pakete verwalten** Dialogfeld bei Bedarf.
7. Wenn Visual Studio fordert, dass Sie alle geöffneten Dateien erneut laden, wählen Sie "**Ja, alle**".
8. Das ELMAH-Paket fügt Einträge für sich selbst in die *"Web.config"* Datei im Stammverzeichnis des Projekts. Wenn Visual Studio Sie werden gefragt, ob Sie die geänderte erneut laden möchten *"Web.config"* , klicken Sie auf **Ja**.

ELMAH ist nun bereit, um nicht behandelte Fehler zu speichern, die auftreten.

### <a name="viewing-the-elmah-log"></a>Anzeigen des Anwendungsprotokolls ELMAH

Anzeigen des Anwendungsprotokolls ELMAH ist einfach, aber zunächst erstellen Sie eine nicht behandelte Ausnahme, die in das ELMAH-Protokoll aufgezeichnet werden.

1. Drücken Sie **STRG + F5** zum Ausführen der Wingtip Toys-beispielanwendung.
2. Um eine nicht behandelte Ausnahme in das ELMAH-Protokoll zu schreiben, navigieren Sie in Ihrem Browser zu folgender URL (unter Verwendung Ihrer Portnummer):  
    `https://localhost:44300/NoPage.aspx` Die Fehlerseite wird angezeigt.
3. Um das ELMAH-Protokoll anzuzeigen, navigieren Sie in Ihrem Browser zu folgender URL (unter Verwendung Ihrer Portnummer):  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET – Fehlerbehandlung - ELMAH-Fehlerprotokoll](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie erfahren, zur Behandlung von Fehlern auf die Ebene, Seitenebene und Codeebene. Sie haben auch gelernt, wie behandelten und nicht behandelten Fehler für eine spätere Prüfung protokolliert werden. Sie haben das ELMAH-Hilfsprogramm, um ausnahmeprotokollierung und Ihre Anwendung mithilfe von NuGet bieten hinzugefügt. Darüber hinaus haben Sie über die Bedeutung von sicheren Fehlermeldungen erhalten.

## <a name="tutorial-series-conclusion"></a>Zusammenfassung der Tutorialreihe

*Nochmals vielen Dank für die folgenden zusammen. Ich hoffe, diese Reihe von Lernprogrammen Sie mit ASP.NET Web Forms vertraut machen konnten. Weitere Informationen zu Web Forms-Funktionen in ASP.NET 4.5 und Visual Studio 2013 verfügbar sind, finden Sie unter* [ *ASP.NET and Web Tools für Visual Studio 2013 Release Notes* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Darüber hinaus müssen Sie unbedingt einen Blick auf das Tutorial erwähnt der* ***Nächstes *** Abschnitt und Defintely Probieren Sie die* [ *kostenlose Azure-Testversion* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Vielen Dank - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Bereitstellen der Webanwendung in Microsoft Azure, finden Sie unter [Bereitstellen einer sicheren ASP.NET Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-Website](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Kostenlose Testversion

[Microsoft Azure – kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)  
 Veröffentlichen Ihre Website in Microsoft Azure werden Sie Zeit, Wartung und Kosten speichern. Es ist ein schneller Vorgang, um Ihre Web-app in Azure bereitstellen. Wenn Sie zu verwalten und Überwachen Ihrer Web-app müssen, bietet Azure eine Vielzahl von Tools und Diensten. Verwalten von Daten, Datenverkehr, Identität, Sicherungen, messaging, Medien und Leistung in Azure. Und all dies wird in einem sehr kostengünstig Ansatz bereitgestellt.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Protokollieren von Fehlerdetails mit ASP.NET-Systemüberwachung](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Bestätigungen

Ich möchte die vielen Dank, dass die folgenden Personen, die wichtige Beiträge zu den Inhalt dieser tutorialreihe vorgenommen:

- [Alberto Poblacion, MVP &amp; MCT, Spanien](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Niederlande](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slowenien](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brasilien](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos Dos Santos, Brasilien](http://www.carloscds.net/)
- [Dave Campbell, USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael Doppelkreuze, USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Priester
- [Mitchel Verkäufer "," USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [TIM Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Community-Beiträge

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Visual Studio 2012 im Zusammenhang-Codebeispiel auf MSDN: [Navigation Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Visual Studio 2012 im Zusammenhang-Codebeispiel auf MSDN: [ASP.NET 4.5 Web Forms-Tutorial-Reihe in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo – Microsoft Technical Audience "Mitwirkender" (twitter: @driazevedo)  
  Visual Studio 2012-Übersetzung: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Vorherige](url-routing.md)
