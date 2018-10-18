---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Bearbeiten von Code in Visual Studio 2013 ASP.NET Web Forms | Microsoft-Dokumentation
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 670f81ca1ef9923575cb2fee1747f06f426963d8
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391218"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Codebearbeitung in ASP.NET Web Forms in Visual Studio 2013
====================
durch [Erik Reitan](https://github.com/Erikre)

Viele ASP.NET Web Form-Seiten schreiben Sie Code in Visual Basic, c# oder einer anderen Sprache. Code-Editor in Visual Studio können Sie den Code in schnell zu schreiben, und gleichzeitig dazu beitragen, Fehler zu vermeiden. Darüber hinaus bietet der Editor Ihnen Möglichkeiten zum Erstellen von wiederverwendbaren Codes, um den Arbeitsaufwand zu reduzieren, die Sie tun müssen.

Diese exemplarische Vorgehensweise veranschaulicht verschiedene Features von Visual Studio Code-Editor.

Bei dieser exemplarischen Vorgehensweise lernen Sie Folgendes:

- Inline-Codierungsfehler zu beheben.
- Gestalten und Umbenennen von Code.
- Umbenennen von Variablen und Objekte.
- Einfügen von Codeausschnitten.

## <a name="prerequisites"></a>Vorraussetzungen


Für die Durchführung dieser exemplarischen Vorgehensweise benötigen Sie Folgendes:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework wird automatisch installiert. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 und Microsoft Visual Studio Express 2013 für Web wird häufig als Visual Studio in dieser tutorialreihe bezeichnet werden.  
    >   
    > Wenn Sie Visual Studio verwenden, in dieser exemplarischen Vorgehensweise setzt voraus, dass Sie die **Webentwicklung** Sammlung von Einstellungen der erstmaligen Start von Visual Studio. Weitere Informationen finden Sie unter [Vorgehensweise: Aktivieren Sie Umgebungseinstellungen für die Webentwicklung](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Erstellen eines Webanwendungsprojekts und einer Seite

<a id="sectionToggle0"></a>

In diesem Teil der exemplarischen Vorgehensweise werden Sie ein Webanwendungsprojekt zu erstellen, und fügen eine neue Seite hinzu.

### <a name="to-create-a-web-application-project"></a>Erstellen ein Webanwendungsprojekts

1. Öffnen Sie Microsoft Visual Studio.
2. Wählen Sie im Menü **Datei** die Option **Neues Projekt** aus.  
    ![Menü "Datei"](code-editing-in-web-forms-pages/_static/image1.png)

    Das Dialogfeld **Neues Projekt** wird angezeigt.
3. Wählen Sie die **Vorlagen**  - &gt; **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite.
4. Wählen Sie die **ASP.NET-Webanwendung** Vorlage in der mittleren Spalte.
5. Benennen Sie Ihr Projekt ***BasicWebApp*** , und klicken Sie auf die **OK** Schaltfläche.   
![Neues Projekt (Dialogfeld)](code-editing-in-web-forms-pages/_static/image2.png)
6. Wählen Sie als Nächstes die **Web Forms** Vorlage, und klicken Sie auf die **OK** klicken, um das Projekt zu erstellen.  
![Neues ASP.NET-Projekt (Dialogfeld)](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio erstellt ein neues Projekt, das vordefinierte Funktionen, die basierend auf der Web Forms-Vorlage enthält.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Erstellen eine neue ASP.NET Web Forms-Seite


Beim Erstellen eine neue Web Forms-Anwendung, mit der **ASP.NET-Webanwendung** -Projektvorlage, die Visual Studio fügt einer ASP.NET-Seite (Web Forms-Seite), die mit dem Namen *"default.aspx"* sowie wie verschiedene andere Dateien und Ordner. Sie können die *"default.aspx"* Seite als Startseite für Ihre Webanwendung. Allerdings werden Sie in dieser exemplarischen Vorgehensweise erstellen und Arbeiten mit einer neuen Seite.

### <a name="to-add-a-page-to-the-web-application"></a>Hinzufügen eine Seite an die Webanwendung


1. In **Projektmappen-Explorer**, mit der rechten Maustaste in den Namen der Web-Anwendung (in diesem Tutorial der Anwendungsname wird **BasicWebSite**), und klicken Sie dann auf **hinzufügen**  - &gt; **Neues Element**.   
Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie dann **Webformular** aus der Mitte aus, und nennen Sie sie *FirstWebPage.aspx*.   
    ![Fügen Sie im Dialogfeld Neues Element hinzu.](code-editing-in-web-forms-pages/_static/image4.png)
3. Klicken Sie auf **hinzufügen** Web Forms-Seite zu Ihrem Projekt hinzufügen.  
 Visual Studio erstellt die neue Seite, und öffnet sie.
4. Anschließend legen Sie diese neue Seite, wie die Standardseite für den Start. In **Projektmappen-Explorer**, mit der rechten Maustaste in der neuen Seite mit dem Namen *FirstWebPage.aspx* , und wählen Sie **als Startseite festlegen**. Das nächste Mal ausführen dieser Anwendung so testen Sie unsere ausgeführt wird, werden Sie automatisch diese neue Seite im Browser angezeigt.


## <a name="correcting-inline-coding-errors"></a>Beheben Sie Codefehlern Inline


Der Code-Editor in Visual Studio können Sie Fehler zu vermeiden, wie Sie Code schreiben, und wenn Sie einen Fehler gemacht haben, wird der Code-Editor Sie den Fehler zu beheben können. In diesem Teil der exemplarischen Vorgehensweise schreiben Sie eine Codezeile, die die Fehlerkorrektur im Editor zu veranschaulichen.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Um einfache Codefehler in Visual Studio zu beheben.


1. In **Entwurf** anzuzeigen, doppelklicken Sie auf die leere Seite zum Erstellen eines Handlers für die **Load** -Ereignis für die Seite.   
   Verwenden Sie den Ereignishandler nur als Ort, um Code zu schreiben.
2. Geben Sie die folgende Zeile mit einem Fehler, und drücken Sie die Handler **EINGABETASTE**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Beim Drücken **EINGABETASTE**, Code-Editor platziert, grüne und rote unterstrichen (häufig aufrufen &quot;Wellenlinien&quot; Zeilen) in die Bereiche des Codes, die Probleme haben. Eine grüne Unterstreichung wird eine Warnung generiert. Eine rote Unterstreichung gibt einen Fehler, den Sie beheben müssen. 

    Halten Sie den Mauszeiger über `myStr` um eine QuickInfo anzuzeigen, der anzeigt, zu der Warnung. Darüber hinaus halten Sie den Mauszeiger über die rote Unterstreichung, die folgende Fehlermeldung angezeigt.

    Die folgende Abbildung zeigt den Code durch die unterstreichungen.

    ![Willkommen bei Text in der Entwurfsansicht](code-editing-in-web-forms-pages/_static/image5.png "Begrüßungstext in der Entwurfsansicht")  
   Der Fehler muss behoben werden, indem Sie ein Semikolon hinzufügen `;` an das Ende der Zeile. Die Warnung einfach benachrichtigt Sie, dass Sie nicht verwendet haben die `myStr` noch Variable.  

    > [!NOTE] 
    > 
    > Sie zeigen den aktuellen Code-Formatierung von Einstellungen in Visual Studio durch Auswahl **Tools**  - &gt; **Optionen**  - &gt; **Schriftarten und Farben**.


## <a name="refactoring-and-renaming"></a>Refactoring und Umbenennen

Umgestalten ist eine Software, bei der Umstrukturierung von Ihren Codes, um zu verstehen und zu verwalten und gleichzeitig ihre Funktionalität erleichtern. Ein einfaches Beispiel möglicherweise, dass Sie Code schreiben, die sich in einem Ereignishandler, um Daten aus einer Datenbank abzurufen. Bei der Entwicklung Ihrer Seite können Sie ermitteln, müssen Sie Zugriff auf die Daten aus verschiedenen Handlern. Aus diesem Grund gestalten Sie den Code der Seite durch eine Datenzugriffs-Methode auf der Seite erstellen und Einfügen von Aufrufen an die Methode in den Handlern.

Der Code-Editor enthält Tools, mit denen Sie verschiedene Umgestaltung Aufgaben ausführen. In dieser exemplarischen Vorgehensweise arbeiten Sie mit zwei Umgestaltung Techniken: Umbenennen von Variablen, und Extrahieren von Methoden. Andere Umgestaltungsoptionen Felder kapseln, lokale Variablen, Methodenparameter und Verwalten der Methodenparameter. Die Verfügbarkeit dieser Umgestaltungsoptionen hängt von der Position im Code ab.

### <a name="refactoring-code"></a>Umgestalten von Code

Ein häufiges Szenario für die umgestaltungs ist die Erstellung von (Extract) eine Methode von Code, der in einem anderen Member, z. B. eine Methode ist. Dadurch reduziert die Größe des ursprünglichen Elements und den extrahierten Code wiederverwendet werden.

In diesem Teil der exemplarischen Vorgehensweise Sie einfachen Code schreiben, und klicken Sie dann Extrahieren einer Methode aus. Umgestaltung wird für c# unterstützt, damit Sie eine Seite erstellen, die c# als Programmiersprache verwendet.

### <a name="to-extract-a-method-in-a-c-page"></a>Extrahieren eine Methode in einer C#-Seite

1. Wechseln Sie zur **Entwurf** anzeigen.
2. In der **Toolbox**, aus der **Standard** Registerkarte, ziehen Sie eine [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) -Steuerelement auf der Seite.
3. Doppelklicken Sie auf die **Schaltfläche** Steuerelement so erstellen Sie einen Ereignishandler für die [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignis, und fügen Sie folgenden hervorgehobenen Code hinzu:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Der Code erstellt ein **ArrayList** Objekt mithilfe einer Schleife mit Werten gefüllt und verwendet dann eine andere Schleife zum Anzeigen der Inhalte der **ArrayList** Objekt.
4. Drücken Sie **STRG + F5** führen Sie die Seite, und klicken Sie dann auf die **Schaltfläche** um sicherzustellen, dass Sie die folgende Ausgabe angezeigt:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Zurück zum Code-Editor, und wählen Sie dann die folgenden Zeilen im Ereignishandler.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Die Auswahl, klicken Sie auf **Umgestalten**, und wählen Sie dann **Methode extrahieren**. 

    Die **Methode extrahieren** Dialogfeld wird angezeigt.
7. In der **Neuer Methodenname** geben **DisplayArray**, und klicken Sie dann auf **OK**. 

    Der Codeeditor erstellt eine neue Methode namens `DisplayArray`, und stellt einen Aufruf an die neue Methode in der **klicken Sie auf** Handler, in dem die Schleife ursprünglich war.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Drücken Sie **STRG + F5** führen Sie die Seite erneut, und klicken Sie auf die **Schaltfläche**.

    Die Seite funktioniert genauso wie vorher. Die `DisplayArray` Methode aufrufen, die von jedem beliebigen Standort nun auch in der Page-Klasse.

## <a name="renaming-variables"></a>Umbenennen von Variablen

Bei der Arbeit mit Variablen als auch Objekte, möchten Sie diese umbenennen, nachdem sie bereits in Ihrem Code referenziert werden. Allerdings kann Umbenennen von Variablen und Objekte, dass der Code nicht funktioniert, wenn Sie eine der Referenzen umbenennen verpassen. Aus diesem Grund können Sie die Umgestaltung führen Sie die Umbenennung.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Zum Verwenden der Umgestaltung, eine Variable umzubenennen


1. In der **klicken Sie auf** -Ereignishandler die folgende Zeile:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Mit der rechten Maustaste in den Namens des Variablen `alist`, wählen Sie **Umgestalten**, und wählen Sie dann **umbenennen**.

    Die **umbenennen** Dialogfeld wird angezeigt.
3. In der **neuen Namen** geben **ArrayList1** , und stellen Sie sicher, dass die **Vorschau der verweisänderungen** Kontrollkästchen ausgewählt wurde. Klicken Sie dann auf **OK**.

    Die **Vorschau der Änderungen** Dialogfeld wird angezeigt, und zeigt eine Struktur, die alle Verweise auf die Variable enthält, die umbenannt werden.
4. Klicken Sie auf **übernehmen** schließen die **Vorschau der Änderungen** Dialogfeld.

    Die Variablen, die speziell für die Instanz zu verweisen, die Sie ausgewählt werden umbenannt. Beachten Sie jedoch, die die Variable `alist` in der folgenden Zeile wird nicht umbenannt.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    Die Variable `alist` in dieser Zeile ist nicht umbenannt, da sie nicht den gleichen Wert wie die Variable darstellt `alist` , die Sie umbenannt. Die Variable `alist` in die `DisplayArray` Deklaration eine lokale Variable für diese Methode ist. Dies zeigt, dass mit der Umgestaltung Umbenennen von Variablen wie das Ausführen einer Aktion suchen und Ersetzen im Editor unterscheidet; Umgestaltung mit Umbenennen von Variablen mit Kenntnissen der Semantik der Variablen, mit denen er arbeitet.


## <a name="inserting-snippets"></a>Einfügen von Ausschnitten

Da sind zahlreiche Codierungsaufgaben, die Web Forms-Entwickler häufig ausführen müssen, bietet der Code-Editor eine Bibliothek mit Codeausschnitten oder vordefinierte Codeblöcke. Sie können diese Ausschnitte in Ihre Seite einfügen.

Jede Sprache, die Sie in Visual Studio verwenden, hat es sich um geringfügige Unterschiede in der Möglichkeit, das Einfügen von Codeausschnitten. Informationen zum Einfügen von Ausschnitten, finden Sie unter [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx). Informationen zum Einfügen von Ausschnitten in Visual C#-, finden Sie unter [Visual C#-Codeausschnitte](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Nächste Schritte

In dieser exemplarischen Vorgehensweise wurden die grundlegenden Funktionen im Code-Editor von Visual Studio 2010 veranschaulicht, für das Beheben von Fehlern in Ihren Code Umgestalten von Code, Umbenennen von Variablen und Einfügen von Codeausschnitten in Ihrem Code. Zusätzliche Features im Editor können die Anwendungsentwicklung schneller und einfacher machen. Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:

- Erfahren Sie mehr über die Features von IntelliSense, z. B. IntelliSense-Optionen ändern und Verwalten von Codeausschnitten suchen online nach Codeausschnitten. Weitere Informationen finden Sie unter [Verwenden von IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Erfahren Sie, wie Sie Ihre eigenen Codeausschnitte erstellen. Weitere Informationen finden Sie unter [erstellen und Verwenden von IntelliSense-Codeausschnitte](https://msdn.microsoft.com/library/ms165392.aspx)
- Erfahren Sie mehr zu den Visual Basic-spezifisches IntelliSense-Codeausschnitte, z. B. das Anpassen der Ausschnitte und Problembehandlung. Weitere Informationen finden Sie unter [Visual Basic IntelliSense-Codeausschnitte](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Erfahren Sie mehr über die c#-spezifische Funktionen von IntelliSense, wie die Umgestaltung und Codeausschnitte. Weitere Informationen finden Sie unter [Visual C#-IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
