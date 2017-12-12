---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.NET Fehlerbehandlung | Microsoft Docs
author: Erikre
description: "Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d5d89a6a82c91b915d61ddc3c350ea0935511c07
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-error-handling"></a>ASP.NET-Fehlerbehandlung
====================
Durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die diese Reihe von Lernprogrammen begleitet.


In diesem Lernprogramm ändern Sie die beispielanwendung Wingtip Toys Fehlerbehandlung und Protokollierung von Anzeigefehlern aufgenommen werden sollen. Fehlerbehandlung können die Anwendung ordnungsgemäß Behandeln von Fehlern und Fehlermeldungen entsprechend angezeigt. Protokollierung von Anzeigefehlern können Sie zum Suchen und Beheben von Fehlern, die aufgetreten sind. Dieses Lernprogramm baut auf dem vorherigen Lernprogramm "URL-Routing" und ist Teil der Wingtip Toys Reihe von Lernprogrammen.

## <a name="what-youll-learn"></a>Lernen Sie:

- So fügen Sie auf die Konfiguration der Anwendung zur globalen Fehlerbehandlung.
- So fügen Sie der Anwendung, die Seite oder den Codeebenen für die Fehlerbehandlung.
- Informationen zum Fehler für eine spätere Prüfung protokolliert.
- Vorgehensweise beim Anzeigen von Fehlermeldungen, die nicht über die Sicherheit beeinträchtigen.
- Wie Sie Protokollierung von Anzeigefehlern Error Logging Modules und Ereignishandler (ELMAH) zu implementieren.

## <a name="overview"></a>Übersicht

ASP.NET-Anwendungen müssen Lage, Fehler zu behandeln, die während der Ausführung konsistent auftreten. ASP.NET verwendet die common Language Runtime (CLR), die bietet eine Möglichkeit, fehlerbenachrichtigung für Anwendungen auf einheitliche Weise. Wenn ein Fehler auftritt, wird eine Ausnahme ausgelöst. Eine Ausnahme ist, alle Fehler, eine Bedingung oder ein unerwartetes Verhalten, das eine Anwendung auftritt.

In .NET Framework stellt eine Ausnahme ein Objekt dar, das von der `System.Exception`-Klasse erbt. Eine Ausnahme wird in einem Codebereich ausgelöst, in dem ein Fehler aufgetreten ist. Die Ausnahme wird oben in der Aufrufliste an eine Stelle übergeben, bei denen die Anwendung Code zur Behandlung von Ausnahmen bereitstellt. Wenn die Anwendung die Ausnahme nicht behandelt, wird der Browser gezwungen, um die Fehlerdetails anzuzeigen.

Als bewährte Methode, Behandeln von Fehlern in Ebene der Code in `Try` / `Catch` / `Finally` Blöcke innerhalb des Codes. Versuchen Sie, diese Blöcke zu platzieren, sodass der Benutzer Probleme im Zusammenhang mit korrigieren kann, in dem sie auftreten. Wenn die Fehlerbehandlung-Blöcke werden zu weit aus, in dem der Fehler aufgetreten ist, wird es schwieriger zu Benutzern bereitstellen, die Informationen, die sie benötigen, um das Problem zu beheben.

### <a name="exception-class"></a>Exception-Klasse

Exception-Klasse ist die Basisklasse, von der Ausnahmen erben. Die meisten Ausnahmeobjekte sind Instanzen einer abgeleiteten Klasse der Exception-Klasse, z. B. die `SystemException` -Klasse, die `IndexOutOfRangeException` -Klasse, oder die `ArgumentNullException` Klasse. Exception-Klasse verfügt über Eigenschaften, z. B. die `StackTrace` -Eigenschaft, die `InnerException` -Eigenschaft, und die `Message` Eigenschaft, die bestimmte Informationen zu diesem Fehler bereitstellen, der aufgetreten ist.

### <a name="exception-inheritance-hierarchy"></a>Ausnahme Vererbungshierarchie

Die Runtime hat einen Basissatz von Ausnahmen, die von der `SystemException` Klasse, die die Common Language Runtime auslöst, wenn eine Ausnahme aufgetreten ist. Die meisten Klassen, die von der Ausnahmeklasse, wie z. B. erben die `IndexOutOfRangeException` Klasse und die `ArgumentNullException` Klasse, die zusätzliche Member nicht implementiert. Aus diesem Grund kann die wichtigste Informationen für eine Ausnahme in der Hierarchie von Ausnahmen, die den Namen der Ausnahme und die Informationen in der Ausnahme gefunden werden.

### <a name="exception-handling-hierarchy"></a>Behandlung von Ausnahmenhierarchie

In einer ASP.NET Web Forms-Anwendung können basierend auf einer bestimmten Behandlung Hierarchie Ausnahmen behandelt werden. Eine Ausnahme kann auf folgenden Ebenen behandelt werden:

- Anwendungsebene
- Seitenebene
- Codeebene

Wenn eine Anwendung Ausnahmen behandelt, kann, die dem Benutzer zusätzliche Informationen über die Ausnahme, die von der Ausnahmeklasse geerbt wird häufig abgerufen und angezeigt werden. Zusätzlich zur Anwendung, die Seite und die Ebene können auch Ausnahmen, die auf der Ebene des HTTP-Modul und mithilfe eines benutzerdefinierten Handlers von IIS behandelt werden.

### <a name="application-level-error-handling"></a>-Anwendung Servicelevel-Fehlerbehandlung

Sie können standardmäßig Fehler auf Anwendungsebene behandeln, entweder durch Ändern der Konfiguration Ihrer Anwendung oder durch Hinzufügen einer `Application_Error` Ereignishandler in der *"Global.asax"* der Anwendung.

Behandeln von Standard-Fehlern und HTTP-Fehler durch Hinzufügen einer `customErrors` im Abschnitt zu den *"Web.config"* Datei. Die `customErrors` Abschnitt können Sie eine Standardseite angeben, die Benutzer umgeleitet werden, wenn ein Fehler auftritt. Zudem können Sie einzelne Seiten für Fehler in bestimmten Status Code angeben.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Leider, wenn Sie die Konfiguration zum Umleiten des Benutzers zu einer anderen Seite verwenden, müssen Sie nicht die Details des Fehlers, die aufgetreten ist.

Sie an einer beliebigen Stelle auftretende Fehler in der Anwendung abfangen, Hinzufügen von Code, der `Application_Error` Ereignishandler in der *"Global.asax"* Datei.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Fehler-Ereignisbehandlung Seitenebene

Ein Handler auf Seitenebene gibt den Benutzer zur Seite zurück, wobei der Fehler aufgetreten ist, aber da Instanzen von Steuerelementen nicht beibehalten werden, kommt es nicht mehr alle Elemente auf der Seite. Die Fehlerdetails, um dem Benutzer der Anwendung bereitzustellen, müssen Sie die Fehlerdetails speziell auf die Seite schreiben.

In der Regel würden Sie einen Fehlerhandler auf Seitenebene verwenden, um nicht behandelte Fehler zu protokollieren oder den Benutzer auf eine Seite zu nutzen, die hilfreiche Informationen angezeigt werden können.

Dieses Codebeispiel zeigt einen Handler für das Error-Ereignis auf einer ASP.NET-Webseite an. Dieser Handler fängt alle Ausnahmen, die nicht bereits in behandelt `try` / `catch` auf der Seite blockiert.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Nachdem Sie einen Fehler zu behandeln, müssen Sie es löschen, durch Aufrufen der `ClearError` Methode des Serverobjekts (`HttpServerUtility` Klasse), andernfalls wird einen zuvor aufgetretenen Fehler angezeigt.

### <a name="code-level-error-handling"></a>Code Ebene Fehlerbehandlung

Die Try-Catch-Anweisung besteht aus einem Try-Block gefolgt von einem oder mehreren catch-Klauseln, die Handler für verschiedene Ausnahmen angeben. Wenn eine Ausnahme ausgelöst wird, sucht die common Language Runtime (CLR) die Catch-Anweisung, die diese Ausnahme behandelt. Wenn die derzeit ausgeführte Methode keinen Catch-Block enthält, sucht die CLR an die Methode, die aktuelle Methode usw., der Aufrufliste aufgerufen. Wenn keine Catch-Block gefunden wird, klicken Sie dann die CLR zeigt eine nicht behandelte Ausnahme-Nachricht an den Benutzer, und beendet die Ausführung des Programms.

Das folgende Codebeispiel zeigt eine allgemeine Möglichkeit der Verwendung von `try` / `catch` / `finally` Fehler behandelt werden.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

Im obigen Code enthält die Try-Block den Code, der gegen eine mögliche Ausnahme geschützt werden muss. Der Block wird ausgeführt, bis entweder eine Ausnahme ausgelöst oder der Block wird erfolgreich abgeschlossen. Wenn entweder ein `FileNotFoundException` Ausnahme oder ein `IOException` Ausnahme auftritt, wird die Ausführung auf eine andere Seite übertragen. Klicken Sie dann den Code in den die schließlich Block ausgeführt wird, ob ein Fehler aufgetreten, oder nicht ist.

## <a name="adding-error-logging-support"></a>Hinzufügen von Unterstützung für Fehler protokollieren

Vor dem Hinzufügen von mit der Wingtip Toys-beispielanwendung für die Fehlerbehandlung, werden Sie Protokollierung von Anzeigefehlern-Unterstützung hinzufügen, indem ein `ExceptionUtility` Klasse, um die *Logik* Ordner. Durch diese Vorgehensweise wird jedes Mal die Anwendung einen Fehler behandelt, werden die Fehlerdetails in die Fehlerprotokolldatei hinzugefügt werden.

1. Mit der rechten Maustaste die *Logik* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Code** Gruppe "Vorlagen" auf der linken Seite. Aktivieren Sie das Kontrollkästchen **Klasse**aus der Mitte aus, und nennen Sie sie **ExceptionUtility.cs**.
3. Wählen Sie **Hinzufügen** aus. Die neue Klassendatei wird angezeigt.
4. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Wenn eine Ausnahme auftritt, kann die Ausnahme in einer Protokolldatei für die Ausnahme geschrieben werden, durch Aufrufen der `LogException` Methode. Diese Methode nimmt zwei Parameter, die das Ausnahmeobjekt und eine Zeichenfolge, die Details der Quelle der Ausnahme enthält. Das Ausnahmeprotokoll bezieht sich auf die *ErrorLog.txt* in der Datei die *App\_Daten* Ordner.

### <a name="adding-an-error-page"></a>Eine Fehlerseite hinzufügen

In der beispielanwendung Wingtip Toys wird eine Seite verwendet werden, um Fehler anzuzeigen. Die Fehlerseite dient zum Anzeigen einer sicheren Fehlermeldung an der Benutzern der Website. Allerdings ist der Benutzer ein Entwickler eine HTTP-Anforderung, die lokal auf dem Computer bereitgestellt werden, wo befindet sich der Code, zusätzliche Fehlerinformationen auf der Seite "Fehler" angezeigt.

1. Mit der rechten Maustaste des Projektnamens (**Wingtip Toys**) in **Projektmappen-Explorer** , und wählen Sie **hinzufügen**  - &gt; **neues Element**.   
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie aus der Liste mittleren **Webformular mit Gestaltungsvorlage**, und nennen Sie sie **ErrorPage.aspx**.
3. Klicken Sie auf **Hinzufügen**.
4. Wählen Sie die *Site.Master* als Masterseite, und wählen Sie dann **OK**.
5. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Ersetzen Sie den vorhandenen Code für das Code-Behind-Modell (*ErrorPage.aspx.cs*), damit er wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Wenn die Seite "Fehler" angezeigt wird, die `Page_Load` -Ereignishandler ausgeführt wird. In der `Page_Load` Handler, richtet sich der Speicherort der, in dem der Fehler zuerst behandelt wurde. Klicken Sie dann der letzten aufgetretenen Fehler wird bestimmt durch Aufruf der `GetLastError` Methode des Serverobjekts. Wenn die Ausnahme nicht mehr vorhanden ist, wird eine generische Ausnahme erstellt. Klicken Sie dann, wenn die HTTP-Anforderung lokal durchgeführt wurde, werden alle Fehlerdetails angezeigt. In diesem Fall wird nur der lokale Computer, die Ausführung der Webanwendung diese Fehlerdetails angezeigt. Nachdem die Fehlerinformationen angezeigt wurde, wird der Fehler in der Protokolldatei hinzugefügt, und der Fehler wird vom Server gelöscht.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Anzeigen von nicht behandelten Fehlermeldungen für die Anwendung

Durch Hinzufügen einer `customErrors` Abschnitt der *"Web.config"* -Datei können Sie schnell einfache in der gesamten Anwendung auftretende Fehler behandeln. Sie geben Sie die Fehlerbehandlung basierend auf deren Status-Codewert z. B. 404 - Datei wurde nicht gefunden.

#### <a name="update-the-configuration"></a>Aktualisieren Sie die Konfiguration

Aktualisieren Sie die Konfiguration durch Hinzufügen einer `customErrors` im Abschnitt zu den *"Web.config"* Datei.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *"Web.config"* Datei im Stammverzeichnis des Wingtip Toys-beispielanwendung.
2. Hinzufügen der `customErrors` im Abschnitt zu den *"Web.config"* Datei innerhalb der `<system.web>` Knoten wie folgt:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Speichern Sie die *"Web.config"* Datei.

Die `customErrors` Abschnitt gibt den Modus an, die auf "On" festgelegt ist. Es gibt auch an die `defaultRedirect`, weist der Anwendung die Seite zu navigieren, wenn ein Fehler auftritt. Darüber hinaus haben Sie eine bestimmte Error-Element hinzugefügt, der angibt, wie Fehler 404 behandelt, wenn eine Seite nicht gefunden wird. Weiter unten in diesem Lernprogramm fügen Sie, dass zusätzliche Fehlerbehandlung, die die Details eines Fehlers auf Anwendungsebene aufzeichnen kann.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung nun auf die aktualisierte Routen finden Sie unter ausführen.

1. Drücken Sie **F5** zum Ausführen der Anwendung des Wingtip Toys-Beispiel.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Geben Sie die folgende URL im Browser (Achten Sie darauf, dass Sie verwenden **Ihrer** Portnummer):  
    `https://localhost:44300/NoPage.aspx`
3. Überprüfen Sie die *ErrorPage.aspx* im Browser angezeigt. 

    ![ASP.NET Fehlerbehandlung - Seite nicht gefunden.](aspnet-error-handling/_static/image1.png)

Wenn Sie anfordern der *NoPage.aspx* Seite, die nicht vorhanden ist, die Fehlerseite werden angezeigt, die einfache Fehlermeldung und die detaillierten Fehlerinformationen, wenn Detailinformationen zur Verfügung stehen. Wenn der Benutzer von einem Remotestandort aus eine nicht existierende Seite angefordert, würden jedoch die Fehlermeldung von der Seite "Fehler" nur in Rot angezeigt.

### <a name="including-an-exception-for-testing-purposes"></a>Z. B. eine Ausnahme zu testen

Um zu überprüfen, wie Ihre Anwendung funktioniert, wenn ein Fehler auftritt, können Sie absichtlich fehlerbedingungen in ASP.NET erstellen. In der beispielanwendung des Wingtip Toys löst Sie eine Test-Ausnahme beim Laden der Standardseite, um festzustellen, was geschieht.

1. Öffnen Sie den Code-Behind von der *"default.aspx"* Seite in Visual Studio.   
 Die *"default.aspx.cs"* Code-Behind-Seite wird angezeigt.
2. In der `Page_Load` Handler, fügen Sie Code hinzu, sodass die Ereignishandler wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Es ist möglich, verschiedene Arten von Ausnahmen erstellen. Im obigen Code, erstellen Sie eine `InvalidOperationException` bei der *"default.aspx"* Seite wird geladen.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung aus, um festzustellen, wie die Anwendung die Ausnahme behandelt ausführen.

1. Drücken Sie **STRG + F5** zum Ausführen der Anwendung des Wingtip Toys-Beispiel.  
 Die Anwendung löst das InvalidOperationException aus. 

    > [!NOTE] 
    > 
    > Drücken Sie **STRG + F5** um die Seite anzuzeigen, ohne in den Code, um die Quelle des Fehlers in Visual Studio anzuzeigen.
2. Überprüfen Sie die *ErrorPage.aspx* im Browser angezeigt. 

    ![ASP.NET Fehlerbehandlung - Fehler (Seite)](aspnet-error-handling/_static/image2.png)

Wie Sie in den Fehlerdetails sehen können, wurde die Ausnahme abgefangen, durch die `customError` im Abschnitt der *"Web.config"* Datei.

### <a name="adding-application-level-error-handling"></a>Hinzufügen von Fehlerbehandlung auf Anwendungsebene

Anstatt die Ausnahme mit Trap der `customErrors` im Abschnitt der *"Web.config"* Datei, wo Sie wenig Informationen über die Ausnahme erhalten, können Sie das Abfangen des Fehlers auf Anwendungsebene und Abrufen von Details zum Systemfehler.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *Global.asax.cs* Datei.
2. Hinzufügen einer **Anwendung\_Fehler** Ereignishandler, sodass er wie folgt angezeigt wird:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Tritt ein Fehler in der Anwendung die `Application_Error` Handler aufgerufen. In dieser Handler ist die letzte Ausnahme abgerufen und überprüft. Wenn die Ausnahme nicht behandelt wurde, und die Ausnahme enthält die Details der inneren Ausnahme (d. h. `InnerException` ist ungleich null), die Anwendung übergibt die Ausführung an die Fehlerseite, auf dem die Details der Ausnahme angezeigt werden.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung für den zusätzlichen Fehlerinformationen, die von der Behandlung der Ausnahme auf Anwendungsebene bereitgestellt finden Sie unter ausführen.

1. Drücken Sie **STRG + F5** zum Ausführen der Anwendung des Wingtip Toys-Beispiel.  
 Die Anwendung löst das `InvalidOperationException` .
2. Überprüfen Sie die *ErrorPage.aspx* im Browser angezeigt. 

    ![ASP.NET Fehlerbehandlung - Ebene Anwendungsfehler](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Hinzufügen von Seitenebene-Fehlerbehandlung

Sie können auf eine Seite auf Seitenebene Fehlerbehandlung hinzufügen, entweder mithilfe von Hinzufügen einer `ErrorPage` -Attribut auf die `@Page` der Seite, oder indem Sie die Richtlinie ein `Page_Error` -Ereignishandler, um den Code-Behind einer Seite. In diesem Abschnitt fügen Sie eine `Page_Error` -Ereignishandler, die Ausführung zu übertragen, wird die *ErrorPage.aspx* Seite.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *"default.aspx.cs"* Datei.
2. Hinzufügen einer `Page_Error` Handler folgt, damit an, dass der Code-Behind als angezeigt wird:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Tritt ein Fehler auf der Seite der `Page_Error` Ereignishandler wird aufgerufen. In dieser Handler ist die letzte Ausnahme abgerufen und überprüft. Wenn ein `InvalidOperationException` tritt auf, die `Page_Error` Ereignishandler übergibt die Ausführung an die Fehlerseite, auf dem die Details der Ausnahme angezeigt werden.

#### <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung nun auf die aktualisierte Routen finden Sie unter ausführen.

1. Drücken Sie **STRG + F5** zum Ausführen der Anwendung des Wingtip Toys-Beispiel.  
 Die Anwendung löst das `InvalidOperationException` .
2. Überprüfen Sie die *ErrorPage.aspx* im Browser angezeigt. 

    ![ASP.NET Fehlerbehandlung - Fehler auf der Seite "](aspnet-error-handling/_static/image4.png)
3. Schließen Sie das Browserfenster.

### <a name="removing-the-exception-used-for-testing"></a>Entfernen die Ausnahme, die zum Testen verwendet

Damit wird die Wingtip Toys-beispielanwendung funktionieren ohne Auslösen der Ausnahme, die Sie zuvor in diesem Lernprogramm hinzugefügt haben, entfernen Sie die Ausnahme.

1. Öffnen Sie den Code-Behind von der *"default.aspx"* Seite.
2. In der `Page_Load` Handler, entfernen Sie den Code, der die Ausnahme auslöst, damit der Ereignishandler wie folgt aussieht:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Hinzufügen von Code auf Dokumentebene Fehlerprotokollierung

Wie weiter oben in diesem Lernprogramm erwähnt, können Sie Try/Catch-Anweisungen aus, um einen Codeabschnitt ausführen und behandeln den ersten Fehler, der auftritt, versuchen hinzufügen. In diesem Beispiel nur schreiben die Fehlerdetails in die Fehlerprotokolldatei Sie so, dass der Fehler später überprüft werden kann.

1. In **Projektmappen-Explorer**in der *Logik* Ordner suchen und Öffnen der *PayPalFunctions.cs* Datei.
2. Update der `HttpCall` -Methode folgt, sodass der Code als angezeigt wird:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

Der obige Code Ruft die `LogException` -Methode, die in enthalten ist die `ExceptionUtility` Klasse. Hinzugefügte der *ExceptionUtility.cs* Klassendatei, die *Logik* Ordner weiter oben in diesem Lernprogramm. Die `LogException`-Methode nimmt zwei Parameter an. Der erste Parameter ist das Ausnahmeobjekt. Der zweite Parameter ist eine Zeichenfolge, mit die Quelle des Fehlers zu erkennen.

### <a name="inspecting-the-error-logging-information"></a>Überprüfen die Fehlerinformationen für die Protokollierung

Wie bereits erwähnt, können Sie das Fehlerprotokoll, um zu bestimmen, welche Fehler in der Anwendung zuerst behoben werden müssen. Natürlich werden nur Fehler, die abgefangen und in das Fehlerprotokoll geschrieben wurden aufgezeichnet.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *ErrorLog.txt* in der Datei die *App\_Daten* Ordner.   
 Sie müssen auswählen der "**alle Dateien anzeigen**" Option oder die "**aktualisieren**" Option vom oberen Rand **Projektmappen-Explorer** , finden Sie unter der *ErrorLog.txt*Datei.
2. Überprüfen Sie das Fehlerprotokoll, die in Visual Studio angezeigt: 

    ![ASP.NET Fehlerbehandlung - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Sichere Fehlermeldungen

Es ist **beachten** , wenn Fehlermeldungen von Ihrer Anwendung angezeigt wird, es keine Informationen enthalten sollen, die ein böswilliger Benutzer in einen Angriff auf die Anwendung hilfreich sein kann. Z. B. wenn die Anwendung in eine Datenbank geschrieben, erfolglos versucht, sollten keine Fehlermeldung anzuzeigen, die den Benutzernamen enthält, die verwendet wird. Aus diesem Grund ist eine allgemeine Fehlermeldung in Rot, die dem Benutzer angezeigt. Alle zusätzlichen Fehlerinformationen werden nur für den Entwickler auf dem lokalen Computer angezeigt.

## <a name="using-elmah"></a>ELMAH verwenden

ELMAH (Error Logging Modules and Handlers) ist eine Fehlerprotokollierungsfunktion, die Sie in der ASP.NET-Anwendung als NuGet-Paket eingebunden. ELMAH bietet die folgenden Funktionen:

- Die Protokollierung von nicht behandelten Ausnahmen.
- Eine Webseite auf das gesamte Protokoll umcodierten nicht behandelte Ausnahmen anzuzeigen.
- So zeigen Sie die vollständigen Details zu jeder an eine Webseite protokolliert Ausnahme.
- Eine e-Mail-Benachrichtigung der einzelnen Fehler zum Zeitpunkt der Ausführung.
- Ein RSS-Feed von der letzten 15 Fehlern aus dem Protokoll.

Bevor Sie mit der ELMAH arbeiten können, müssen Sie es installieren. Dies ist einfach, mithilfe der *NuGet* Installer-Paket. Wie weiter oben in diesem Lernprogramm erwähnt, ist NuGet eine Visual Studio-Erweiterung, die zum Installieren und Aktualisieren von open Source-Bibliotheken und Tools in Visual Studio vereinfacht.

1. In Visual Studio aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**  - &gt; **NuGet-Pakete für Projektmappe verwalten**. 

    ![ASP.NET Fehlerbehandlung - NuGet-Pakete für Projektmappe verwalten](aspnet-error-handling/_static/image6.png)
2. Die **NuGet-Pakete verwalten** wird das Dialogfeld in Visual Studio angezeigt.
3. In der **NuGet-Pakete verwalten** Dialogfeld erweitern Sie **Online** auf der linken Seite und wählen Sie dann **nuget.org**. Klicken Sie dann, suchen und Installieren der **ELMAH** Paket aus der Liste der online verfügbaren Pakete. 

    ![ASP.NET Fehlerbehandlung - ELMA NuGet-Paket](aspnet-error-handling/_static/image7.png)
4. Sie benötigen eine Internetverbindung, um das Paket herunterzuladen.
5. In der **Projekte auswählen** Dialogfeld stellen Sie sicher, dass die **WingtipToys** Auswahl ausgewählt ist, und klicken Sie dann auf **OK**. 

    ![ASP.NET Fehlerbehandlung - Projekte Dialogfeld zum Auswählen](aspnet-error-handling/_static/image8.png)
6. Klicken Sie auf **schließen** in **NuGet-Pakete verwalten** (Dialogfeld), wenn erforderlich.
7. Wenn Visual Studio fordert, dass Sie alle geöffneten Dateien erneut zu laden, wählen Sie "**Ja für alle**".
8. Das ELMAH-Paket fügt Einträge für sich selbst in die *"Web.config"* Datei im Stammverzeichnis des Projekts. Wenn Visual Studio Sie werden gefragt, ob Sie die geänderte erneut laden möchten *"Web.config"* Datei, klicken Sie auf **Ja**.

ELMAH kann jetzt nicht behandelte Fehler zu speichern, die auftreten.

### <a name="viewing-the-elmah-log"></a>Anzeigen des Anwendungsprotokolls ELMAH

Anzeigen des Anwendungsprotokolls ELMAH ist einfach, aber zuerst erstellen Sie eine nicht behandelte Ausnahme, die das ELMAH-Protokoll aufgezeichnet werden.

1. Drücken Sie **STRG + F5** zum Ausführen der Anwendung des Wingtip Toys-Beispiel.
2. Um eine nicht behandelte Ausnahme in das ELMAH-Protokoll zu schreiben, navigieren Sie in Ihrem Browser auf die folgende URL (mit der Portnummer):  
    `https://localhost:44300/NoPage.aspx`Die Seite "Fehler" wird angezeigt.
3. Navigieren Sie zum Anzeigen des Protokolls ELMAH in Ihrem Browser auf die folgende URL (mit der Portnummer):  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET Fehlerbehandlung - ELMAH-Fehlerprotokoll](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gelernt, zur Behandlung von Fehlern auf Anwendungsebene, die Ebene der Seite und den der Codeebene. Sie haben auch gelernt, wie behandelte als auch nicht behandelte Fehler für eine spätere Prüfung protokolliert werden. Sie haben das ELMAH-Hilfsprogramm zum Bereitstellen der Ausnahme protokollieren und Ihre Anwendung mithilfe von NuGet hinzugefügt. Darüber hinaus haben Sie über die Bedeutung der sicheren Fehlermeldungen gelernt.

## <a name="tutorial-series-conclusion"></a>Zusammenfassung der Reihe von Lernprogrammen

*Vielen Dank, dass folgende zusammen. Ich hoffe, diese Reihe von Lernprogrammen Sie ASP.NET Web Forms kennen gelernt haben. Wenn Sie weitere Informationen zu Web Forms-Funktionen in ASP.NET 4.5 und Visual Studio 2013 verfügbar möchten, finden Sie unter* [ *ASP.NET und Webtools für Visual Studio 2013-Versionshinweise* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Achten Sie auch auf einen Blick auf das Lernprogramm erwähnt, der*   ***Arbeitsschritte *** Abschnitt und Defintely Probieren Sie Sie aus der* [ *kostenlose Azure-Testversion* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Vielen Dank, dass - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Bereitstellen der Webanwendung in Microsoft Azure, finden Sie unter [Bereitstellen einer Secure ASP.NET Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-Website](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Kostenlose Testversion

[Microsoft Azure - Testversion](https://azure.microsoft.com/pricing/free-trial/)  
 Veröffentlichen Ihre Website in Microsoft Azure werden Sie Zeit, Wartung und Ausgaben speichern. Es ist ein schneller Vorgang, um Ihre Web-app in Azure bereitstellen. Wenn Sie zum Verwalten und überwachen Ihre Web-app benötigen, bietet Azure eine Vielzahl von Tools und Diensten. Verwalten von Daten, Datenverkehr, Identität, Sicherungen, messaging, Medien und Leistung in Azure. Und all dies erfolgt in einem sehr kostengünstig Ansatz.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.NET-Systemüberwachung protokollieren Fehlerdetails](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Bestätigungen

Ich möchte die vielen Dank, dass die folgenden Personen, die bedeutende Beiträge auf den Inhalt dieser Reihe von Lernprogrammen vorgenommen:

- [Alberto Poblacion, MVP &amp; MCT, Spanien](https://mvp.microsoft.com/en-us/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen, Niederlande](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, USA](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Slowenien](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brasilien](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos Dos Santos, Brasilien](http://www.carloscds.net/)
- [Dave Campbell, USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael Doppelkreuze, USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike PPPoE
- [Mitchel Verkäufer, USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [TIM Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Beiträge aus der Community

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
 Visual Studio 2012 im Zusammenhang Codebeispiel auf MSDN: [Navigation Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
 Visual Studio 2012 im Zusammenhang Codebeispiel auf MSDN: [ASP.NET 4.5 Web Forms Lernprogramm Reihe in Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - Microsoft technischen Zielgruppe Contributor (twitter: @driazevedo)  
 Visual Studio 2012-Übersetzung: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1 - Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

>[!div class="step-by-step"]
[Zurück](url-routing.md)
