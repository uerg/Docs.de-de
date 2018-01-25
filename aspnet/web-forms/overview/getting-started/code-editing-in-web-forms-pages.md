---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Bearbeiten von Code in Visual Studio 2013 ASP.NET Web Forms | Microsoft Docs
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 8714f673cb0434189ca23d2dda14035d8652a051
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Code bearbeiten ASP.NET-Web Forms in Visual Studio 2013
====================
by [Erik Reitan](https://github.com/Erikre)

In vielen ASP.NET Web Forms-Seiten schreiben Sie Code in Visual Basic, c# oder einer anderen Sprache. Code-Editor in Visual Studio können Sie Code schnell zu schreiben, während gleichzeitig Fehler zu vermeiden. Darüber hinaus bietet der Editor zum Erstellen von wiederverwendbaren Codes Möglichkeiten, um den Arbeitsaufwand zu reduzieren, die Sie ausführen müssen.

Diese exemplarische Vorgehensweise veranschaulicht die verschiedenen Funktionen von Visual Studio Code-Editor.

Bei dieser exemplarischen Vorgehensweise lernen Sie Folgendes:

- Inline-Codierungsfehler zu korrigieren.
- Umgestalten und Umbenennen von Code.
- Umbenennen von Variablen und Objekte.
- Fügen Sie Codeausschnitte.

## <a name="prerequisites"></a>Erforderliche Komponenten


Für die Durchführung dieser exemplarischen Vorgehensweise benötigen Sie Folgendes:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework wird automatisch installiert. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 und Microsoft Visual Studio Express 2013 für das Web wird häufig als Visual Studio in der gesamten dieser Reihe von Lernprogrammen bezeichnet werden.  
    >   
    > Wenn Sie Visual Studio verwenden, wird in dieser exemplarischen Vorgehensweise davon ausgegangen, dass Sie ausgewählt haben die **Webentwicklung** Auflistung von Einstellungen zum ersten Mal starten von Visual Studio. Weitere Informationen finden Sie unter [wie: Wählen Sie Web Development Umgebungseinstellungen](https://msdn.microsoft.com/library/ff521558.aspx).

 Eine Einführung zu Visual Studio und ASP.NET finden Sie unter [erstellen eine grundlegenden ASP.NET 4.5 Web Forms-Seite in Visual Studio 2013](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Erstellen ein Webanwendungsprojekt und eine Seite

<a id="sectionToggle0"></a>

In diesem Teil der exemplarischen Vorgehensweise erstellen Sie ein Webanwendungsprojekt zu erstellen und ihm eine neue Seite hinzugefügt.

### <a name="to-create-a-web-application-project"></a>Um ein Webanwendungsprojekt zu erstellen.

1. Öffnen Sie Microsoft Visual Studio.
2. Wählen Sie im Menü **Datei** die Option **Neues Projekt** aus.  
    ![Menü "Datei"](code-editing-in-web-forms-pages/_static/image1.png)

    Das Dialogfeld **Neues Projekt** wird angezeigt.
3. Wählen Sie die **Vorlagen**  - &gt; **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite.
4. Wählen Sie die **ASP.NET-Webanwendung** Vorlage in der mittleren Spalte.
5. Namen für das Projekt ***BasicWebApp*** , und klicken Sie auf die **OK** Schaltfläche.   
![Neues Projekt (Dialogfeld)](code-editing-in-web-forms-pages/_static/image2.png)
6. Wählen Sie als Nächstes die **Web Forms** Vorlage, und klicken Sie auf die **OK** Schaltfläche, um das Projekt zu erstellen.  
![Neues ASP.NET-Projekt (Dialogfeld)](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio erstellt ein neues Projekt, das vordefinierte Funktionen, die basierend auf der Web Forms-Vorlage enthält.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Erstellen einer neuen ASP.NET Web Forms-Seite


Beim Erstellen einer neuen Web Forms-Anwendung, mithilfe der **ASP.NET-Webanwendung** Projektvorlage für Startseiten, fügt Visual Studio eine ASP.NET-Seite (Web Forms-Seite), die mit dem Namen *"default.aspx"*sowie mehrere andere Dateien und Ordner ab. Sie können die *"default.aspx"* Seite als Startseite für Ihre Web-Anwendung. Allerdings werden Sie in dieser exemplarischen Vorgehensweise erstellen und Arbeiten mit einer neuen Seite.

### <a name="to-add-a-page-to-the-web-application"></a>Die Webanwendung eine Seite hinzu


1. In **Projektmappen-Explorer**, mit der rechten Maustaste in den Namen der Webanwendung (in diesem Lernprogramm der Anwendungsname wird **BasicWebSite**), und klicken Sie dann auf **hinzufügen**  - &gt; **Neues Element**.   
Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Aktivieren Sie das Kontrollkästchen **Webformular** aus der Mitte aus, und nennen Sie sie *FirstWebPage.aspx*.   
    ![Fügen Sie im Dialogfeld Neues Element hinzu.](code-editing-in-web-forms-pages/_static/image4.png)
3. Klicken Sie auf **hinzufügen** Web Forms-Seite zu Ihrem Projekt hinzufügen.  
 Visual Studio die Seite neue erstellt und öffnet sie.
4. Legen Sie anschließend diese neue Seite als der Standard-Startseite an. In **Projektmappen-Explorer**, mit der rechten Maustaste in der neuen Seite mit dem Namen *FirstWebPage.aspx* , und wählen Sie **als Startseite festlegen**. Das nächste Mal ausführen dieser Anwendung so testen Sie unseren Fortschritt wird automatisch diese neue Seite im Browser angezeigt.


## <a name="correcting-inline-coding-errors"></a>Korrigieren von Inline-Codierungsfehler


Der Code-Editor in Visual Studio können Sie Fehler vermeiden, wie Sie Code schreiben, und wenn Sie einen Fehler vorgenommen haben, im Code-Editor Sie den Fehler zu beheben können. In diesem Teil der exemplarischen Vorgehensweise schreiben Sie eine Codezeile, die die Fehlerkorrektur im Editor zu veranschaulichen.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>So beheben Sie einfache Codierungsfehler in Visual Studio


1. In **Entwurf** anzuzeigen, doppelklicken Sie auf die leere Seite, um einen Handler für erstellen die **laden** Ereignis für die Seite.   
Verwenden Sie den Ereignishandler nur als Ort, um Code zu schreiben.
2. Geben Sie die folgende Zeile, die ein Fehler, und drücken Sie enthält die Handler **EINGABETASTE**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

 Beim Drücken **EINGABETASTE**, Code-Editor platziert, grüne und rote unterstrichen (häufig aufrufen &quot;Wellenlinie&quot; Zeilen) unter Bereiche des Codes, die Probleme haben. Eine grüne Unterstreichung wird eine Warnung generiert. Ein roter Unterstrich signalisiert einen Fehler, den Sie beheben müssen. 

    Halten Sie den Mauszeiger über `myStr` um eine QuickInfo anzuzeigen, die Aufschluss über die Warnung. Darüber hinaus halten Sie den Mauszeiger über die rote Unterstreichung, um die Fehlermeldung anzuzeigen.

    Die folgende Abbildung zeigt den Code mit der unterstrichen angezeigt.

    ![Willkommen bei Text in der Entwurfsansicht](code-editing-in-web-forms-pages/_static/image5.png "Willkommen Text in der Entwurfsansicht")  
 Der Fehler muss behoben werden, durch ein Semikolon hinzufügen `;` bis zum Ende der Linie. Die Warnung einfach benachrichtigt Sie, dass Sie verwendet haben die `myStr` noch Variable.  

    > [!NOTE] 
    > 
    > Der aktuelle Code Formatierung Einstellungen in Visual Studio durch Auswahl anzeigen **Tools**  - &gt; **Optionen**  - &gt; **Schriftarten und Farben**.


## <a name="refactoring-and-renaming"></a>Umgestaltung und Umbenennen

Umgestalten ist eine Software, bei der Umstrukturierung Ihrer Codes, damit es einfacher zu verstehen und zu verwalten und dabei seine Funktionalität beizubehalten. Ein einfaches Beispiel möglicherweise, dass Sie Code schreiben, die sich in einem Ereignishandler, um Daten aus einer Datenbank abzurufen. Bei der Entwicklung Ihrer Seite können Sie ermitteln, müssen Sie die Daten aus mehreren verschiedenen Handler zuzugreifen. Aus diesem Grund gestalten Sie den Code der Seite durch eine Datenzugriffs-Methode auf der Seite erstellen und Aufrufe der Methode in die Handler einfügen.

Der Code-Editor enthält Tools, mit denen Sie verschiedene Umgestaltung Aufgaben ausführen. In dieser exemplarischen Vorgehensweise arbeiten Sie mit zwei Umgestaltung Techniken: Umbenennen von Variablen und Methoden zu extrahieren. Andere Umgestaltungsoptionen Kapseln von Feldern, lokale Variablen Methodenparametern heraufstufen und Verwalten von Methodenparameter. Die Verfügbarkeit dieser Umgestaltungsoptionen hängt von der Position im Code ab.

### <a name="refactoring-code"></a>Umgestalten von Code

Ein häufiges Szenario für die Umgestaltung zu erstellen (Extrahieren) ist eine Methode von Code, der innerhalb einer anderen Member, z. B. eine Methode ist. Dies reduziert die Größe des ursprünglichen Elements und macht den extrahierten Code wiederverwendet werden.

In diesem Teil der exemplarischen Vorgehensweise müssen Sie einfachen Code schreiben, und extrahieren Sie eine Methode aus. Die Umgestaltung ist für c# unterstützt, daher erstellen Sie eine Seite, die c# als Programmiersprache verwendet.

### <a name="to-extract-a-method-in-a-c-page"></a>Um eine Methode in einer C#-Seite zu extrahieren.

1. Wechseln Sie zur **Entwurf** anzeigen.
2. In der **Toolbox**, aus der **Standard** Registerkarte, ziehen Sie eine [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) -Steuerelement auf die Seite.
3. Doppelklicken Sie auf die **Schaltfläche** Steuerelement erstellt, einen Handler für dessen [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) Ereignis, und fügen Sie den folgenden hervorgehobenen Code hinzu:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

 Der Code erstellt ein **ArrayList** Objekt, mithilfe einer Schleife mit Werten gefüllt, und verwendet dann eine andere Schleife zur Anzeige der Inhalte von den **ArrayList** Objekt.
4. Drücken Sie **STRG + F5** führen Sie die Seite, und klicken Sie dann auf die **Schaltfläche** um sicherzustellen, dass Sie die folgende Ausgabe erhalten:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Code-Editor zurückzukehren Sie, und wählen Sie dann die folgenden Zeilen im Ereignishandler.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Mit der rechten Maustaste in der Auswahl, klicken Sie auf **Umgestalten**, und wählen Sie dann **Methode extrahieren**. 

    Die **Methode extrahieren** Dialogfeld wird angezeigt.
7. In der **neue Methodennamen** geben **DisplayArray**, und klicken Sie dann auf **OK**. 

    Code-Editor erstellt eine neue Methode mit dem Namen `DisplayArray`, und fügt einen Aufruf an die neue Methode in der **klicken Sie auf** Handler, bei denen die Schleife ursprünglich war.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Drücken Sie **STRG + F5** führen Sie die Seite erneut, und klicken Sie auf die **Schaltfläche**.

    Die Seite funktioniert genauso wie zuvor. Die `DisplayArray` Methode kann jetzt Aufruf von jedem beliebigen Standort in der Page-Klasse.

## <a name="renaming-variables"></a>Umbenennen von Variablen

Bei der Arbeit mit Variablen sowie Objekte empfiehlt es sich, sie umbenennen, nachdem sie bereits in Ihrem Code referenziert werden. Allerdings kann Umbenennen von Variablen und Objekte, dass der Code unterbrochen, wenn Sie einen der Verweise umbenennen auslassen. Aus diesem Grund können Sie die Umgestaltung führen die Umbenennung.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Um die Umgestaltung verwenden, um eine variable


1. In der **klicken Sie auf** Ereignishandler, suchen Sie die folgende Zeile:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Mit der rechten Maustaste des Variablennamen `alist`, wählen Sie **Umgestalten**, und wählen Sie dann **umbenennen**.

    Die **umbenennen** Dialogfeld wird angezeigt.
3. In der **neuen Namen** geben **ArrayList1** , und stellen Sie sicher, dass die **Vorschau der Änderungen Verweis** Kontrollkästchen ausgewählt wurde. Klicken Sie dann auf **OK**.

    Die **Vorschau der Änderungen** Dialogfeld wird angezeigt, und zeigt eine Struktur, die alle Verweise auf die Variable enthält, die Sie umbenennen möchten.
4. Klicken Sie auf **übernehmen** schließen die **Vorschau der Änderungen** (Dialogfeld).

    Die Variablen, die speziell für die Instanz zu verweisen, die Sie ausgewählt haben, werden umbenannt. Beachten Sie jedoch, die die Variable `alist` in der folgenden Zeile nicht umbenannt.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Die Variable `alist` in dieser Zeile ist nicht umbenannt, da er nicht den gleichen Wert wie die Variable darstellt `alist` , die Sie umbenannt. Die Variable `alist` in die `DisplayArray` Deklaration ist eine lokale Variable für diese Methode. Dies veranschaulicht, dass mit der Umgestaltung Umbenennen von Variablen einfach ein suchen und Ersetzen im Editor Aktion unterscheidet; Umgestaltung mit Umbenennen von Variablen mit Kenntnis der Semantik der Variablen, mit denen er arbeitet.


## <a name="inserting-snippets"></a>Einfügen von Ausschnitten

Kommen viele Programmieraufgaben, die Web Forms-Entwickler häufig ausführen müssen, stellt der Code-Editor eine Bibliothek von Codeausschnitten oder vordefinierte Codeblöcke bereit. Sie können diese Codeausschnitte in Ihre Seite einfügen.

Jede Sprache, die Sie in Visual Studio verwenden, hat geringfügige Unterschiede bei der Datenerfassung Einfügen von Codeausschnitten. Informationen zum Einfügen von Ausschnitten finden Sie unter [Visual Basic IntelliSense-Codeausschnitte](https://msdn.microsoft.com/library/18yz4be4.aspx). Informationen zum Einfügen von Ausschnitten in Visual c# finden Sie unter [Visual C#-Codeausschnitte](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Nächste Schritte

In dieser exemplarischen Vorgehensweise wurden die grundlegenden Funktionen im Code-Editor von Visual Studio 2010 für Korrigieren von Fehlern in Ihren Code Umgestalten von Code, Umbenennen von Variablen und Einfügen von Codeausschnitten in Ihrem Code veranschaulicht. Zusätzliche Features im Editor können Anwendungsentwicklung schneller und einfacher machen. Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:

- Weitere Informationen über die Funktionen von IntelliSense, z. B. IntelliSense-Optionen ändern und Verwalten von Codeausschnitten für Codeausschnitte online suchen. Weitere Informationen finden Sie unter [Verwenden von IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Erfahren Sie, wie Sie eigene Codeausschnitte erstellen. Weitere Informationen finden Sie unter [erstellen und Verwenden von IntelliSense-Codeausschnitte](https://msdn.microsoft.com/library/ms165392.aspx)
- Erfahren Sie mehr über die Visual Basic-spezifisches Features von IntelliSense-Codeausschnitte, z. B. Anpassen der Ausschnitte und Problembehandlung. Weitere Informationen finden Sie unter [Visual Basic IntelliSense-Codeausschnitte](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Erfahren Sie mehr über die c#-spezifische Funktionen von IntelliSense, z. B. Umgestaltung und Codeausschnitte. Weitere Informationen finden Sie unter [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
