---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Erstellen einer einfachen ASP.NET 4.5 Web Forms-Seite in Visual Studio 2013 | Microsoft-Dokumentation
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: c2051b166b8800654a107c5100ad5ed94af93407
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394452"
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Erstellen einer einfachen ASP.NET Forms 4.5-Web-Seite in Visual Studio 2013
====================
durch [Erik Reitan](https://github.com/Erikre)

Diese exemplarische Vorgehensweise bietet eine Einführung in die Web-Entwicklungsumgebung im [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) und [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Diese exemplarische Vorgehensweise führt Sie durch Erstellen einer einfachen ASP.NET Web Forms-Seite und zeigt die grundlegenden Techniken zum Erstellen einer neuen Seite, Hinzufügen von Steuerelementen und das Schreiben von Code.

In dieser exemplarischen Vorgehensweise werden u. a. folgende Aufgaben veranschaulicht:

- Erstellen eine Datei System Web Forms-Anwendungsprojekts.
- Kennenlernen von Visual Studio.
- Erstellen eine ASP.NET-Seite.
- Hinzufügen von Steuerelementen.
- Hinzufügen von Ereignishandlern.
- Ausführen, und testen eine Seite aus Visual Studio.

## <a name="prerequisites"></a>Erforderliche Komponenten


Für die Durchführung dieser exemplarischen Vorgehensweise benötigen Sie Folgendes:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework wird automatisch installiert. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 und Microsoft Visual Studio Express 2013 für Web wird häufig als Visual Studio in dieser tutorialreihe bezeichnet werden.  
    >   
    > Wenn Sie Visual Studio verwenden, in dieser exemplarischen Vorgehensweise setzt voraus, dass Sie die **Webentwicklung** Sammlung von Einstellungen der erstmaligen Start von Visual Studio. Weitere Informationen finden Sie unter [Vorgehensweise: Aktivieren Sie Umgebungseinstellungen für die Webentwicklung](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Erstellen eines Webanwendungsprojekts und einer Seite

<a id="sectionToggle0"></a>

In diesem Teil der exemplarischen Vorgehensweise werden Sie ein Webanwendungsprojekt zu erstellen, und fügen eine neue Seite hinzu. Außerdem fügen Sie HTML-Text und führen Sie die Seite in Ihrem Browser.

### <a name="to-create-a-web-application-project"></a>Erstellen ein Webanwendungsprojekts

1. Öffnen Sie Microsoft Visual Studio.
2. Wählen Sie im Menü **Datei** die Option **Neues Projekt** aus.  
    ![Menü "Datei"](creating-a-basic-web-forms-page/_static/image1.png)

    Das Dialogfeld **Neues Projekt** wird angezeigt.
3. Wählen Sie die **Vorlagen**  - &gt; **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite.
4. Wählen Sie die **ASP.NET-Webanwendung** Vorlage in der mittleren Spalte.
5. Benennen Sie Ihr Projekt ***BasicWebApp*** , und klicken Sie auf die **OK** Schaltfläche.   
![Neues Projekt (Dialogfeld)](creating-a-basic-web-forms-page/_static/image2.png)
6. Wählen Sie als Nächstes die **Web Forms** Vorlage, und klicken Sie auf die **OK** klicken, um das Projekt zu erstellen.  
![Neues ASP.NET-Projekt (Dialogfeld)](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio erstellt ein neues Projekt, das vordefinierte Funktionen, die basierend auf der Web Forms-Vorlage enthält. Es stellt nicht nur mit einem *Home.aspx* Seite eine *About.aspx* Seite eine *Contact.aspx* Seite, enthält jedoch auch Mitgliedschaftsfunktion, die Benutzer registriert, und speichert die Anmeldeinformationen, damit sie zu Ihrer Website anmelden können. Wenn eine neue Seite erstellt wird, in der Standardeinstellung zeigt Visual Studio die Seite im **Quelle** anzeigen, hier Sie die HTML-Elemente sehen. Die folgende Abbildung zeigt, was Sie sehen würden, in **Quelle** anzeigen, bei der Erstellung einer neuen Webseite mit dem Namen *BasicWebApp.aspx*.  
    ![Quellansicht](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Einen Überblick über die Visual Studio-Webentwicklungsumgebung


Bevor Sie fortfahren, indem Sie die Seite ändern, ist es hilfreich, sich mit der Visual Studio-Entwicklungsumgebung vertraut zu machen. Die folgende Abbildung zeigt Sie die Fenster und Tools, die in Visual Studio und Visual Studio Express für Web verfügbar sind.

> [!NOTE] 
> 
> Dieses Diagramm zeigt das Fenster und den Fensterpositionen. Die **Ansicht** Menü können Sie zusätzliche Fenster anzeigen und neu anordnen und Ändern der Größe Windows Ihre Einstellungen anpassen. Wenn die Anordnung der Fenster bereits Änderungen vorgenommen wurden, stimmen sehen Sie in der Abbildung nicht überein.


 Visual Studio-Umgebung

![Visual Studio-Umgebung](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Machen Sie sich mit der Web-designer

Überprüfen Sie die obige Abbildung und den Text, der in der folgenden Liste entsprechen der am häufigsten verwendeten beschrieben Fenster und Tools. (Nicht alle Fenster und Tools, die Sie sehen hier aufgeführt sind, markiert nur die in der vorherigen Abbildung.)

- Symbolleisten. Geben Sie Befehle, für das Formatieren von Text, Suchen von Text und So weiter. Einige Symbolleisten sind verfügbar, nur bei der Arbeit **Entwurf** anzeigen.
- **Projektmappen-Explorer** Fenster. Zeigt die Dateien und Ordner in Ihrer Webanwendung an.
- Dokumentfenster. Zeigt die Dokumente, die Sie gerade, im Fenster im Registerformat arbeiten. Sie können zwischen Dokumenten, indem Sie auf die Registerkarten wechseln.
- **Eigenschaften** Fenster. Können Sie Einstellungen für die Seite, HTML-Elemente, Steuerelemente und andere Objekte zu ändern.
- Anzeigen von Registerkarten. Stellen Sie mit anderen Ansichten des gleichen Dokuments. **Entwurf** Ansicht ist eine WYSIWYG Bearbeitungsoberfläche. **Quelle** ist der HTML-Editor für die Seite. **Split** zeigt sowohl den **Entwurf** anzeigen und die **Quelle** Ansicht für das Dokument. Arbeiten Sie mit der **Entwurf** und **Quelle** Ansichten weiter unten in dieser exemplarischen Vorgehensweise. Falls, zum Öffnen von Webseiten in gewünscht **Entwurf** anzuzeigen, auf die **Tools** Menü klicken Sie auf **Optionen**, wählen die **HTML-Designer** Knoten, und ändern Sie die **Seiten starten In** Option.
- **ToolBox**. Stellt Steuerelemente und HTML-Elemente, die Sie auf der Seite ziehen können. **Toolbox** Elemente sind nach Funktionen gruppiert.
- S **Erver Explorer**. Zeigt die Verbindungen mit der Datenbank. Wenn der Server-Explorer nicht angezeigt wird, klicken Sie auf das Menü "Ansicht" auf Server-Explorer.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Erstellen eine neue ASP.NET Web Forms-Seite


Beim Erstellen eine neue Web Forms-Anwendung, mit der **ASP.NET-Webanwendung** -Projektvorlage, die Visual Studio fügt einer ASP.NET-Seite (Web Forms-Seite), die mit dem Namen *"default.aspx"* sowie wie verschiedene andere Dateien und Ordner. Sie können die *"default.aspx"* Seite als Startseite für Ihre Webanwendung. Allerdings werden Sie in dieser exemplarischen Vorgehensweise erstellen und Arbeiten mit einer neuen Seite.

### <a name="to-add-a-page-to-the-web-application"></a>Hinzufügen eine Seite an die Webanwendung


1. Schließen der *"default.aspx"* Seite. Zu diesem Zweck klicken Sie auf die Registerkarte, die den Dateinamen zeigt an, und klicken Sie dann auf die Option "Schließen".
2. In **Projektmappen-Explorer**, mit der rechten Maustaste in den Namen der Web-Anwendung (in diesem Tutorial der Anwendungsname wird **BasicWebSite**), und klicken Sie dann auf **hinzufügen**  - &gt; **Neues Element**.   
Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
3. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie dann **Webformular** aus der Mitte aus, und nennen Sie sie *FirstWebPage.aspx*.   
    ![Fügen Sie im Dialogfeld Neues Element hinzu.](creating-a-basic-web-forms-page/_static/image6.png)
4. Klicken Sie auf **hinzufügen** der Webseite zu Ihrem Projekt hinzufügen.  
Visual Studio erstellt die neue Seite, und öffnet sie.


### <a name="adding-html-to-the-page"></a>Hinzufügen von HTML auf der Seite


In diesem Teil der exemplarischen Vorgehensweise fügen Sie statischen Text auf der Seite hinzu.

### <a name="to-add-text-to-the-page"></a>Hinzufügen von Text auf der Seite


1. Klicken Sie am unteren Rand des Dokumentfensters angezeigt, auf die **Entwurf** Registerkarte zu wechseln **Entwurf** anzeigen.

    Die Entwurfsansicht zeigt die aktuelle Seite in einer WYSIWYG-ähnliche Weise. An diesem Punkt müssen Sie keinen Text oder Steuerelemente auf der Seite, damit die Seite, außer eine gestrichelte Linie ist, die ein Rechteck beschreibt. Dieses Rechteck stellt eine **Div** Element auf der Seite.
2. Klicken Sie auf das Rechteck, das durch eine gestrichelte Linie beschrieben wird.
3. Typ **Willkommen bei Visual Web Developer** , und drücken Sie **EINGABETASTE** zweimal.

    Die folgende Abbildung zeigt den eingegebenen Text **Entwurf** anzeigen.

    ![Willkommen bei Text in der Entwurfsansicht](creating-a-basic-web-forms-page/_static/image7.png "Begrüßungstext in der Entwurfsansicht")
4. Wechseln Sie zur **Quelle** anzeigen.

    Sehen Sie den HTML-Code in **Quelle** anzeigen, die Sie erstellt, wenn die Eingabe im **Entwurf** anzeigen.  
    ![Webseite mit statischem Text](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Ausführen der Seite


Bevor Sie fortfahren, indem Sie das Hinzufügen von Steuerelementen auf der Seite, können Sie ihn ausführen.

### <a name="to-run-the-page"></a>Um die Seite auszuführen


1. In **Projektmappen-Explorer**, mit der rechten Maustaste *FirstWebPage.aspx* , und wählen Sie **als Startseite festlegen**.
2. Drücken Sie **STRG + F5** um die Seite auszuführen.

    Die Seite wird im Browser angezeigt. Obwohl die Seite, die Sie erstellt haben, eine Dateinamenerweiterung von hat *aspx*, er derzeit ausgeführt wird, wie jeder HTML-Seite.

    Auf eine Seite im Browser anzuzeigen. Sie können auch mit der rechten Maustaste der Seite im **Projektmappen-Explorer** , und wählen Sie **in Browser anzeigen**.
3. Schließen Sie den Browser, um die Web-Anwendung zu beenden.


## <a name="adding-and-programming-controls"></a>Hinzufügen und Programmieren von Steuerelementen


<a id="sectionToggle1"></a>

Sie werden jetzt Serversteuerelemente der Seite hinzufügen. Steuerelemente, z. B. Schaltflächen, Bezeichnungen, Textfelder und anderen Steuerelementen vertrauten bieten typische Form-Verarbeitungsfunktionen für Web Forms-Seiten. Allerdings können Sie die Steuerelemente mit Code programmieren, die auf dem Server statt auf dem Client ausgeführt wird.

Fügen Sie eine [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) -Steuerelement, ein [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) -Steuerelement, und ein [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) die Steuerung an die Seite, und Schreiben von Code zum Behandeln von der [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignis für die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Steuerelement.

### <a name="to-add-controls-to-the-page"></a>Zum Hinzufügen von Steuerelementen auf der Seite


1. Klicken Sie auf die **Entwurf** Registerkarte zu wechseln **Entwurf** anzeigen.
2. Fügen die Einfügemarke am Ende der **Willkommen bei Visual Web Developer** Text, und drücken Sie **EINGABETASTE** mindestens fünf Mal etwas Platz im vornehmen der **Div** im Element.
3. In der **Toolbox**, erweitern Sie die **Standard** gruppieren, wenn er nicht bereits erweitert ist.  
Beachten Sie, dass Sie möglicherweise zum Erweitern der **Toolbox** Fenster auf der linken Seite, um ihn anzuzeigen.
4. Ziehen Sie eine [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) steuern, auf die Seite, und legen Sie sie in der Mitte der der **Div** Element-Feld, die **Willkommen bei Visual Web Developer** in der ersten Zeile.
5. Ziehen Sie eine [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) steuern, auf die Seite, und legen Sie es rechts neben der [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) Steuerelement.
6. Ziehen Sie eine [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) steuern, auf die Seite, und legen Sie diese auf einer separaten Zeile unter der [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Steuerelement.
7. Die oben genannten Einfügemarke setzen die [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) steuern, und geben Sie **Geben Sie Ihren Namen:** .

    Diese statische HTML-Text ist die Beschriftung für die [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) Steuerelement. Sie können statische HTML- und Serversteuerelemente auf der gleichen Seite kombinieren. Die folgende Abbildung zeigt, wie die drei Steuerelemente in werden **Entwurf** anzeigen.

    ![Drei Steuerelemente in der Entwurfsansicht](creating-a-basic-web-forms-page/_static/image9.png "drei Steuerelemente in der Entwurfsansicht")


### <a name="setting-control-properties"></a>Festlegen von Steuerelementeigenschaften


Visual Studio bietet Ihnen verschiedene Möglichkeiten zum Festlegen der Eigenschaften von Steuerelementen auf der Seite. In diesem Teil der exemplarischen Vorgehensweise legen Sie Eigenschaften in beiden **Entwurf** anzeigen und **Quelle** anzeigen.

### <a name="to-set-control-properties"></a>Festlegen von Steuerelementeigenschaften


1. Zeigen Sie zunächst die **Eigenschaften** Windows durch Auswahl aus der **Ansicht** Menü -&gt; **Other Windows**  - &gt; **Fenster "Eigenschaften"**. Wählen Sie beispielsweise auch **F4** zum Anzeigen der **Eigenschaften** Fenster.
2. Wählen Sie die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) -Steuerelement, und klicken Sie dann im der **Eigenschaften** legen den Wert der **Text** zu **Anzeigenamen**. Der eingegebene Text wird auf die Schaltfläche im Designer angezeigt, wie in der folgenden Abbildung dargestellt.

    ![Festlegen des Schaltflächentexts](creating-a-basic-web-forms-page/_static/image10.png "Text der Schaltfläche festlegen")
3. Wechseln Sie zur **Quelle** anzeigen.

    **Quelle** zeigt den HTML-Code für die Seite, einschließlich der Elemente, die Visual Studio für die Steuerelemente erstellt wurden. Steuerelemente werden mit deklariert HTML-ähnliche Syntax, außer dass die Tags, das Präfix verwenden **Asp:** und das Attribut **Runat =&quot;Server&quot;**.

    Steuerelementeigenschaften werden als Attribute deklariert. Z. B. beim Festlegen der [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) -Eigenschaft für die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) steuern, die in Schritt 1, wurden Sie tatsächlich Festlegen der **Text** -Attribut in das Markup des Steuerelements.

    > [!NOTE] 
    > 
    > Alle Steuerelemente befinden sich in einem **Formular** -Element, das das Attribut verfügt auch über **Runat =&quot;Server&quot;**. Die **Runat =&quot;Server&quot;**  Attribut und die **Asp:** -Präfix für Tags für die Steuerelemente markieren, sodass sie beim Ausführen der Seite auf dem Server von ASP.NET verarbeitet werden. Code außerhalb des **&lt;form Runat =&quot;Server&quot; &gt;** und **&lt;Skript Runat =&quot;Server&quot; &gt;** Elemente erhält unverändert an den Browser, weshalb der ASP-Code innerhalb eines Elements werden muss, dessen Starttag enthält, die **Runat =&quot;Server&quot;**  Attribut.
4. Als Nächstes fügen Sie eine zusätzliche Eigenschaft der [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement. Fügen die Einfügemarke direkt nach dem **Asp: Label** in die **&lt;Asp: Label&gt;** markieren, und drücken Sie dann die **LEERTASTE**.

    Eine Dropdown-Liste angezeigt wird, die die Liste der verfügbaren Eigenschaften angezeigt, Sie, für festlegen können, eine [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement. Diese Funktion, genannt **IntelliSense**, hilft Ihnen in **Quelle** Ansicht mit der Syntax der Steuerelemente, HTML-Elementen und anderen Elementen auf der Seite. Die folgende Abbildung zeigt die **IntelliSense** Dropdown-Liste für die [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement.

    ![IntelliSense-Attribute](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense-Attribute")
5. Wählen Sie **ForeColor** und geben Sie dann mit einem Gleichheitszeichen.

    IntelliSense zeigt eine Liste der Farben an.

    > [!NOTE] 
    > 
    > Sie können anzeigen, eine **IntelliSense** Dropdown-Liste zu einem beliebigen Zeitpunkt durch Drücken von **STRG + J** beim Anzeigen von Code.
6. Wählen Sie eine Farbe für die **[Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** den Text des Steuerelements. Stellen Sie sicher, dass Sie eine Farbe auswählen, die dunkel genug sind, lesen Sie vor einem weißen Hintergrund ist.

    Die **ForeColor** Attribut mit der Farbe, die Sie ausgewählt haben, einschließlich der schließenden Klammer, die abgeschlossen wurde.


### <a name="programming-the-button-control"></a>Programmieren das Schaltflächen-Steuerelement


In dieser exemplarischen Vorgehensweise schreiben Sie Code, der den Namen gelesen werden, die der Benutzer, in das Textfeld eingibt, und klicken Sie dann zeigt den Namen in der [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement.

### <a name="add-a-default-button-event-handler"></a>Fügen Sie ein Standard-Ereignishandler hinzu.


1. Wechseln Sie zur **Entwurf** anzeigen.
2. Doppelklicken Sie auf die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Steuerelement.

    Standardmäßig Visual Studio wechselt zu einem Code-Behind-Datei und erstellt Sie eine Skelett eines ereignishandlers für das [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Standardereignis des Steuerelements, das [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) Ereignis. Die Code-Behind-Datei trennt Ihre UI-Markup (z. B. HTML) aus dem Servercode (z. B. C#)).   
   Code für diesen Ereignishandler hinzugefügt, wird der Cursor positioniert.

    > [!NOTE] 
    > 
    > Durch Doppelklicken auf ein Steuerelement im **Entwurf** Ansicht ist nur eine von mehreren Möglichkeiten können Sie Ereignishandler erstellen.
3. In der **"Button1"\_klicken Sie auf** Ereignishandler, Typ **Label1** gefolgt von einem Punkt (**.**).

    Geben Sie den Zeitraum nach der **ID** der Bezeichnung (**Label1**), Visual Studio zeigt eine Liste der verfügbaren Mitglieder für die [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) zu steuern, wie im folgenden dargestellt Abbildung. Ein Element im Allgemeinen eine Eigenschaft, Methode oder Ereignis.

    ![IntelliSense in der Codeansicht](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense in der Codeansicht")
4. Fertig stellen die **klicken Sie auf** -Ereignishandler für die Schaltfläche so, dass die It liest, wie im folgenden Codebeispiel gezeigt.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Wechseln Sie zurück zum Anzeigen der **Quelle** Überblick über Ihr HTML-Markup, mit der rechten Maustaste *FirstWebPage.aspx* in die **Projektmappen-Explorer** und auswählen **anzeigen Markup**.
6. Scrollen Sie zu der **&lt;Asp: Button&gt;** Element. Beachten Sie, dass die **&lt;Asp: Button&gt;** Element verfügt jetzt über das Attribut **Onclick =&quot;"Button1"\_klicken Sie auf&quot;**.

    Dieses Attribut gebunden wird, der Schaltfläche [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) Ereignis, um die Handlermethode, die Sie im vorherigen Schritt codiert.

    Ereignishandlermethoden können beliebig benannt werden. der Name, den Sie sehen, ist die Standardnamen von Visual Studio erstellt. Der wichtigste Punkt ist, dass der Name verwendet wird, für die **OnClick** -Attribut in der HTML-Code muss dem Namen einer Methode im Code-Behind definiert entsprechen.


### <a name="running-the-page"></a>Ausführen der Seite


Sie können nun die Steuerelemente auf der Seite testen.

### <a name="to-run-the-page"></a>Um die Seite auszuführen


1. Drücken Sie **STRG + F5** um die Seite im Browser auszuführen. Wenn ein Fehler auftritt, überprüfen Sie die obigen Schritte ausgeführt.
2. Geben Sie einen Namen in das Textfeld ein, und klicken Sie auf die **Anzeigenamen** Schaltfläche.

    Der eingegebene Name wird angezeigt, der [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement. Beachten Sie, dass wenn Sie die Schaltfläche klicken, die Seite an den Webserver gesendet wird. Klicken Sie dann ASP.NET die Seite erstellt, führt den Code (in diesem Fall die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) des Steuerelements [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignishandler ausgeführt), und sendet dann die neue Seite im Browser. Wenn Sie die Statusleiste im Browser ansehen, sehen Sie sich, dass die Seite einen Roundtrip an dem Webserver jedem stammt und Sie auf die Schaltfläche klicken.
3. Zeigen Sie im Browser die Quelle der Seite, die Sie ausgeführt werden, indem Sie mit der rechten Maustaste auf der Seite und auswählen **Quelltext anzeigen**.

    In der Quellcode der Seite sehen Sie HTML, ohne Servercode. Insbesondere nicht angezeigt. die **&lt;Asp:&gt;** Elemente, die Sie, im gearbeitet haben **Quelle** anzeigen. Wenn die Seite ausgeführt wird, wird von ASP.NET verarbeitet die Serversteuerelemente und rendert die HTML-Elemente auf der Seite, die Funktionen ausführen, die die Darstellung des Steuerelements. Z. B. die **&lt;Asp: Button&gt;** Steuerelement wird als HTML gerendert **&lt;Eingabetyp =&quot;übermitteln&quot; &gt;** Element.
4. Schließen Sie den Browser.


## <a name="working-with-additional-controls"></a>Arbeiten mit zusätzlichen Steuerelemente

<a id="sectionToggle2"></a>

In diesem Teil der exemplarischen Vorgehensweise arbeiten Sie mit der [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) -Steuerelement, das Daten pro Monat angezeigt. Die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement ist ein komplexer Steuerelement als die Schaltfläche, das Textfeld und Bezeichnung mit gearbeitet wurde, und veranschaulicht einige weitere Funktionen der Serversteuerelemente.

In diesem Abschnitt fügen Sie eine [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) die Steuerung an die Seite, und formatieren Sie ihn.

### <a name="to-add-a-calendar-control"></a>Ein Kalender-Steuerelement hinzufügen


1. Wechseln Sie in Visual Studio zum **Entwurf** anzeigen.
2. Aus der **Standard** Teil der **Toolbox**, ziehen Sie eine [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) steuern, auf die Seite, und legen Sie sie unterhalb der **Div** Element, die anderen Steuerelemente enthält.

    Der im Kalender der Smarttagbereich wird angezeigt. Im Bereich zeigt die Befehle, die Sie für die häufigsten Aufgaben für das ausgewählte Steuerelement erleichtern. Die folgende Abbildung zeigt die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) steuern in gerenderten **Entwurf** anzeigen.

    ![Monatskalender-Steuerelement in der Entwurfsansicht](creating-a-basic-web-forms-page/_static/image13.png "Monatskalender-Steuerelement in der Entwurfsansicht")
3. Wählen Sie in den Smarttagbereich **Autom. Formatierung**.

    Die **Autom. Formatierung** Dialogfeld wird angezeigt, sodass Sie ein Schema für die Formatierung für den Kalender auswählen. Die folgende Abbildung zeigt die **Autom. Formatierung** im Dialogfeld für die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement.

    ![Automatisches Formatieren (Dialogfeld) (Calendar-Steuerelement)](creating-a-basic-web-forms-page/_static/image14.png "AutoFormat (Dialogfeld) (Calendar-Steuerelement)")
4. Von der **wählen Sie ein Schema** wählen **einfache** , und klicken Sie dann auf **OK**.
5. Wechseln Sie zur **Quelle** anzeigen.

    Sie sehen die **&lt;Asp: Kalender&gt;** Element. Dieses Element ist deutlich länger als die Elemente für die einfache Steuerelemente, die Sie zuvor erstellt haben. Es enthält auch Unterelemente, wie z. B.  **&lt;WeekEndDayStyle&gt;**, die verschiedenen formatierungseinstellungen darstellen. Die folgende Abbildung zeigt die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement **Quelle** anzeigen. (Das genaue Markup, das Sie in finden Sie unter **Quelle** Ansicht möglicherweise unterscheiden sich geringfügig von der Abbildung.)

    ![Monatskalender-Steuerelement in der Quellansicht](creating-a-basic-web-forms-page/_static/image15.png "Monatskalender-Steuerelement in der Quellansicht")


### <a name="programming-the-calendar-control"></a>Programmieren das Kalender-Steuerelement


Programmieren Sie in diesem Abschnitt die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) -Steuerelement das aktuell ausgewählte Datum angezeigt.

### <a name="to-program-the-calendar-control"></a>Das Kalender-Steuerelement zu programmieren


1. In **Entwurf** anzuzeigen, doppelklicken Sie auf die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement.

    Ein neuen Ereignishandler erstellt und angezeigt, in der CodeBehind-Datei mit dem Namen *FirstWebPage.aspx.cs*.
2. Fertig stellen die [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) -Ereignishandler durch den folgenden Code.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    Der obige Code wird der Text des Label-Steuerelements auf das ausgewählte Datum des Kalender-Steuerelements.


### <a name="running-the-page"></a>Ausführen der Seite


Sie können jetzt auf den Kalender testen.

### <a name="to-run-the-page"></a>Um die Seite auszuführen


1. Drücken Sie **STRG + F5** um die Seite im Browser auszuführen.
2. Klicken Sie auf ein Datum im Kalender.

    Das Datum, die Sie geklickt wird angezeigt, der [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement.
3. Zeigen Sie den Quellcode für die Seite im Browser.

    Beachten Sie, dass die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement auf der Seite gerendert wurde eine **Tabelle**, mit jeder Tag als eine **td** Element.
4. Schließen Sie den Browser.


## <a name="next-steps"></a>Nächste Schritte


<a id="nextStepsToggle"></a>

In dieser exemplarischen Vorgehensweise wurden die grundlegenden Funktionen von der Visual Studio-Designer veranschaulicht. Nun, da Sie wissen, wie zum Erstellen und Bearbeiten von Web Forms-Seite in Visual Studio, empfiehlt es sich um andere Features auszuprobieren. Beispielsweise möchten Sie die folgenden Schritte ausführen:

- Erfahren Sie mehr über ASP.NET Web Forms anhand der schrittweisen Tutorial-Reihe [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Weitere Informationen zu Cascading Stylesheets (CSS). Weitere Informationen finden Sie unter [arbeiten mit der Übersicht über die CSS-](https://msdn.microsoft.com/library/bb398931.aspx).
