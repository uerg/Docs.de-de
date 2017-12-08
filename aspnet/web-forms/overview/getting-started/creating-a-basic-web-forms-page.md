---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Erstellen einen grundlegenden ASP.NET 4.5 Web Forms-Seite in Visual Studio 2013 | Microsoft Docs
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 20e920ff63444c0d69cecb972619b07fe6d23097
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>Erstellen einer einfachen ASP.NET Forms 4.5 Web Seite in Visual Studio 2013
====================
Durch [Erik Reitan](https://github.com/Erikre)

In dieser exemplarischen Vorgehensweise bietet Ihnen eine Einführung in die Web-Entwicklungsumgebung in [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) und [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web). Diese exemplarische Vorgehensweise führt Sie durch Erstellen einer einfachen ASP.NET Web Forms-Seite und veranschaulicht die grundlegenden Techniken zum Erstellen einer neuen Seite, Hinzufügen von Steuerelementen und Code schreiben.

In dieser exemplarischen Vorgehensweise werden u. a. folgende Aufgaben veranschaulicht:

- Erstellen eine Datei System Web Forms-Anwendungsprojekt.
- Kennenlernen von Visual Studio.
- Erstellen einer ASP.NET-Seite.
- Hinzufügen von Steuerelementen.
- Hinzufügen von Ereignishandlern.
- Ausführen und Testen einer Seite in Visual Studio.

## <a name="prerequisites"></a>Erforderliche Komponenten


Für die Durchführung dieser exemplarischen Vorgehensweise benötigen Sie Folgendes:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/en-us/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web). .NET Framework wird automatisch installiert. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 und Microsoft Visual Studio Express 2013 für das Web wird häufig als Visual Studio in der gesamten dieser Reihe von Lernprogrammen bezeichnet werden.  
    >   
    > Wenn Sie Visual Studio verwenden, wird in dieser exemplarischen Vorgehensweise davon ausgegangen, dass Sie ausgewählt haben die **Webentwicklung** Auflistung von Einstellungen zum ersten Mal starten von Visual Studio. Weitere Informationen finden Sie unter [wie: Wählen Sie Web Development Umgebungseinstellungen](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="creating-a-web-application-project-and-a-page"></a>Erstellen ein Webanwendungsprojekt und eine Seite

<a id="sectionToggle0"></a>

In diesem Teil der exemplarischen Vorgehensweise erstellen Sie ein Webanwendungsprojekt zu erstellen und ihm eine neue Seite hinzugefügt. Außerdem fügen Sie HTML-Text und führen Sie die Seite in Ihrem Browser.

### <a name="to-create-a-web-application-project"></a>Um ein Webanwendungsprojekt zu erstellen.

1. Öffnen Sie Microsoft Visual Studio.
2. Wählen Sie im Menü **Datei** die Option **Neues Projekt** aus.  
    ![Menü "Datei"](creating-a-basic-web-forms-page/_static/image1.png)

    Das Dialogfeld **Neues Projekt** wird angezeigt.
3. Wählen Sie die **Vorlagen**  - &gt; **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite.
4. Wählen Sie die **ASP.NET-Webanwendung** Vorlage in der mittleren Spalte.
5. Namen für das Projekt ***BasicWebApp*** , und klicken Sie auf die **OK** Schaltfläche.   
![Neues Projekt (Dialogfeld)](creating-a-basic-web-forms-page/_static/image2.png)
6. Wählen Sie als Nächstes die **Web Forms** Vorlage, und klicken Sie auf die **OK** Schaltfläche, um das Projekt zu erstellen.  
![Neues ASP.NET-Projekt (Dialogfeld)](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio erstellt ein neues Projekt, das vordefinierte Funktionen, die basierend auf der Web Forms-Vorlage enthält. Er bietet nicht nur mit einem *Home.aspx* Seite ein *About.aspx* Seite eine *Contact.aspx* Seite, sondern umfasst auch Mitgliedschaftsfunktionen, die Benutzer registriert und speichert Ihre Anmeldeinformationen, damit Ihre Website anmelden können. Wenn eine neue Seite erstellt wird, in der Standardeinstellung zeigt Visual Studio die Seite in **Quelle** Ansicht, in dem HTML-Elemente der Seite angezeigt. Die folgende Abbildung zeigt, was Sie in sehen, **Quelle** anzeigen, wenn Sie eine neue Webseite mit dem Namen erstellt *BasicWebApp.aspx*.  
    ![Quellansicht](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Einen Überblick über die Entwicklungsumgebung von Visual Studio Web


Bevor Sie fortfahren, indem Sie die Seite ändern, ist es hilfreich, sich mit der Visual Studio-Entwicklungsumgebung vertraut zu machen. Die folgende Abbildung zeigt Sie die Fenster und Tools, die in Visual Studio und Visual Studio Express für Web zur Verfügung stehen.

> [!NOTE] 
> 
> Dieses Diagramm zeigt Standardfenster und Fensterpositionen. Die **Ansicht** Menü können Sie zusätzliche Fenster anzeigen und neu anordnen und ändern Sie Größe Ihren Bedürfnissen entsprechend. Wenn das Fenster Anordnung bereits Änderungen vorgenommen wurden, stimmen sehen Sie die Abbildung nicht überein.


 Visual Studio-Umgebung

![Visual Studio-Umgebung](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Kennenlernen der Web-designer

Überprüfen die obigen Abbildung und vergleichen Sie den Text in der folgenden Liste der am häufigsten verwendeten beschrieben Fenster und Tools. (Nicht alle Fenster und Tools, die Sie sehen hier aufgeführt sind, markiert nur die in der vorherigen Abbildung.)

- Symbolleisten. Geben Sie Befehle, für das Formatieren von Text, Suchen von Text und So weiter. Einige Symbolleisten stehen nur bei der Arbeit **Entwurf** anzeigen.
- **Projektmappen-Explorer** Fenster. Zeigt die Dateien und Ordner in der Webanwendung an.
- Dokumentfenster angezeigt. Zeigt die Dokumente, die Sie im Fenster im Registerformat arbeiten. Sie können zwischen Dokumenten, indem Sie auf die Registerkarten wechseln.
- **Eigenschaften** Fenster. Können Sie Einstellungen für die Seite, HTML-Elemente, Steuerelemente und andere Objekte zu ändern.
- Anzeigen von Registerkarten. Stellen Sie mit anderen Ansichten des gleichen Dokuments. **Entwurf** Ansicht ist eine WYSIWYG Bearbeitungsoberfläche. **Quelle** ist der HTML-Editor für die Seite. **Split** zeigt sowohl den **Entwurf** anzeigen und die **Quelle** Ansicht für das Dokument. Arbeiten Sie mit der **Entwurf** und **Quelle** Ansichten weiter unten in dieser exemplarischen Vorgehensweise. Wenn Sie es vorziehen, Öffnen von Webseiten in **Entwurf** anzuzeigen, die **Tools** im Menü klicken Sie auf **Optionen**, wählen die **HTML-Designer** Knoten, und Ändern der **Seiten starten In** Option.
- **ToolBox**. Stellt Steuerelemente und HTML-Elemente, die Sie auf der Seite ziehen können. **Toolbox** durch common-Funktion die Elemente gruppiert sind.
- S **Erver Explorer**. Zeigt die Verbindungen mit der Datenbank. Wenn Server-Explorer nicht sichtbar ist, klicken Sie im Menü Ansicht auf Server-Explorer.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Erstellen einer neuen ASP.NET Web Forms-Seite


Beim Erstellen einer neuen Web Forms-Anwendung, mithilfe der **ASP.NET-Webanwendung** Projektvorlage für Startseiten, fügt Visual Studio eine ASP.NET-Seite (Web Forms-Seite), die mit dem Namen *"default.aspx"*sowie mehrere andere Dateien und Ordner ab. Sie können die *"default.aspx"* Seite als Startseite für Ihre Web-Anwendung. Allerdings werden Sie in dieser exemplarischen Vorgehensweise erstellen und Arbeiten mit einer neuen Seite.

### <a name="to-add-a-page-to-the-web-application"></a>Die Webanwendung eine Seite hinzu


1. Schließen der *"default.aspx"* Seite. Zu diesem Zweck klicken Sie auf der Registerkarte ", die den Dateinamen anzeigt, und klicken Sie dann auf die Option schließen.
2. In **Projektmappen-Explorer**, mit der rechten Maustaste in den Namen der Webanwendung (in diesem Lernprogramm der Anwendungsname wird **BasicWebSite**), und klicken Sie dann auf **hinzufügen**  - &gt; **Neues Element**.   
Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
3. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Aktivieren Sie das Kontrollkästchen **Webformular** aus der Mitte aus, und nennen Sie sie *FirstWebPage.aspx*.   
    ![Fügen Sie im Dialogfeld Neues Element hinzu.](creating-a-basic-web-forms-page/_static/image6.png)
4. Klicken Sie auf **hinzufügen** auf der Webseite zu Ihrem Projekt hinzufügen.  
Visual Studio die Seite neue erstellt und öffnet sie.


### <a name="adding-html-to-the-page"></a>Hinzufügen von HTML auf der Seite "


In diesem Teil der exemplarischen Vorgehensweise fügen Sie statischen Text auf der Seite.

### <a name="to-add-text-to-the-page"></a>Hinzufügen von Text auf der Seite "


1. Klicken Sie unten im Dokumentfenster klicken Sie auf die **Entwurf** Registerkarte zu wechseln **Entwurf** anzeigen.

    Die Entwurfsansicht zeigt die aktuelle Seite in einer WYSIWYG-ähnliche Weise. An diesem Punkt müssen Sie keinen Text oder Steuerelemente auf der Seite, damit die Seite außer eine gestrichelte Linie ist, die ein Rechteck erläutert. Dieses Rechteck stellt eine **Div** Element auf der Seite.
2. Klicken Sie in das Rechteck, das durch eine gestrichelte Linie beschrieben wird.
3. Typ **Welcome to Visual Web Developer** , und drücken Sie **EINGABETASTE** zweimal.

    Die folgende Abbildung zeigt den Text, die Sie eingegeben, im haben **Entwurf** anzeigen.

    ![Willkommen bei Text in der Entwurfsansicht](creating-a-basic-web-forms-page/_static/image7.png "Willkommen Text in der Entwurfsansicht")
4. Wechseln Sie zur **Quelle** anzeigen.

    Sehen Sie den HTML-Code in **Quelle** anzeigen, die Sie erstellt haben, wenn Sie in den Typ **Entwurf** anzeigen.  
    ![Webseite mit statischem Text](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>Ausführen der Seite


Bevor Sie fortfahren, durch Hinzufügen von Steuerelementen auf der Seite ", können Sie ihn ausführen.

### <a name="to-run-the-page"></a>Um die Seite auszuführen


1. In **Projektmappen-Explorer**, mit der rechten Maustaste *FirstWebPage.aspx* , und wählen Sie **als Startseite festlegen**.
2. Drücken Sie **STRG + F5** um die Seite auszuführen.

    Die Seite wird im Browser angezeigt. Obwohl die Seite, die Sie erstellt eine Dateinamenerweiterung von hat *aspx*, sie derzeit ausgeführt wird, wie HTML-Seite.

    Auf eine Seite im Webbrowser anzuzeigen. Sie können auch mit der rechten Maustaste der Seite in **Projektmappen-Explorer** , und wählen Sie **in Browser anzeigen**.
3. Schließen Sie den Browser, um die Web-Anwendung zu beenden.


## <a name="adding-and-programming-controls"></a>Hinzufügen und Programmieren von Steuerelementen


<a id="sectionToggle1"></a>

Steuerelemente werden jetzt auf der Seite hinzugefügt werden. Serversteuerelemente, z. B. Schaltflächen, Bezeichnungen, Textfelder und andere vertraute Steuerelemente bieten die typische Form-Verarbeitungsfunktionen für Web Forms-Seiten. Allerdings können Sie die Steuerelemente mit Code programmieren, die auf dem Server statt der Client ausgeführt wird.

Fügen Sie eine [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) -Steuerelement, ein [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) -Steuerelement, und ein [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) die Steuerung an die Seite, und Schreiben von Code für die Behandlung der [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignis für die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Steuerelement.

### <a name="to-add-controls-to-the-page"></a>Zum Hinzufügen von Steuerelementen auf der Seite "


1. Klicken Sie auf die **Entwurf** Registerkarte zu wechseln **Entwurf** anzeigen.
2. Platzieren die Einfügemarke am Ende der **Welcome to Visual Web Developer** Text, und drücken Sie **EINGABETASTE** mindestens fünf Mal zu Platz in der **Div** im Element.
3. In der **Toolbox**, erweitern Sie die **Standard** gruppieren, wenn er nicht bereits erweitert ist.  
Beachten Sie, das Sie erweitern ggf. die **Toolbox** Fenster auf der linken Seite, um ihn anzuzeigen.
4. Ziehen Sie eine [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) steuern, auf die Seite, und legen Sie sie in der Mitte von der **Div** im Element, das verfügt **Welcome to Visual Web Developer** in der ersten Zeile.
5. Ziehen Sie eine [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) steuern, auf die Seite, und legen Sie es rechts neben der [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) Steuerelement.
6. Ziehen Sie eine [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) steuern, auf die Seite, und legen Sie ihn auf einer separaten Zeile unter der [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Steuerelement.
7. Platzieren die Einfügemarke oberhalb der [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) steuern, und geben Sie **Geben Sie Ihren Namen:** .

    Diese statische HTML-Text ist die Beschriftung für die [Textfeld](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) Steuerelement. Sie können statische HTML und Serversteuerelemente auf derselben Seite kombinieren. Die folgende Abbildung zeigt, wie die drei Steuerelemente in angezeigt werden **Entwurf** anzeigen.

    ![Drei Steuerelemente in der Entwurfsansicht](creating-a-basic-web-forms-page/_static/image9.png "drei Steuerelemente in der Entwurfsansicht")


### <a name="setting-control-properties"></a>Festlegen von Steuerelementeigenschaften


Visual Studio bietet verschiedene Möglichkeiten zum Festlegen der Eigenschaften der Steuerelemente auf der Seite. In diesem Teil der exemplarischen Vorgehensweise legen Sie Eigenschaften in beiden **Entwurf** anzeigen und **Quelle** anzeigen.

### <a name="to-set-control-properties"></a>Festlegen von Steuerelementeigenschaften


1. Zeigen Sie zuerst die **Eigenschaften** Windows durch Auswahl aus der **Ansicht** Menü -&gt; **Weitere Fenster**  - &gt; **Eigenschaften-Fenster**. Sie können alternativ auswählen **F4** zum Anzeigen der **Eigenschaften** Fenster.
2. Wählen Sie die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) -Steuerelement, und klicken Sie dann im die **Eigenschaften** Fenster, legen Sie den Wert der **Text** auf **Anzeigenamen**. Der von Ihnen eingegebene Text wird auf die Schaltfläche im Designer angezeigt, wie in der folgenden Abbildung dargestellt.

    ![Button-Text festlegen](creating-a-basic-web-forms-page/_static/image10.png "Schaltflächentext festlegen")
3. Wechseln Sie zur **Quelle** anzeigen.

    **Quelle** zeigt den HTML-Code für die Seite, einschließlich der Elemente, die Visual Studio für die Serversteuerelemente erstellt hat. Steuerelemente mit HTML-ähnliche Syntax, außer dass die Tags, das Präfix verwenden werden deklarierten **Asp:** und das Attribut enthalten **Runat =&quot;Server&quot;**.

    Eigenschaften werden als Attribute deklariert. Z. B. beim Festlegen der [Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) -Eigenschaft für die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) steuern, die in Schritt 1, wurden Sie tatsächlich Festlegen der **Text** -Attribut in das Markup des Steuerelements.

    > [!NOTE] 
    > 
    > Alle Steuerelemente befinden sich in einem **Formular** -Element, das das Attribut verfügt auch über **Runat =&quot;Server&quot;**. Die **Runat =&quot;Server&quot;**  Attribut und der **Asp:** -Präfix für Steuerelementtags die Steuerelemente markieren, sodass sie beim Ausführen der Seite auf dem Server von ASP.NET verarbeitet werden. Code außerhalb des  **&lt;bilden Runat =&quot;Server&quot; &gt;**  und  **&lt;Skript Runat =&quot;Server&quot; &gt;**  Elemente wird unverändert an den Browser, weshalb der ASP-Code innerhalb eines Elements werden muss, dessen Starttag enthält, gesendet, die **Runat =&quot;Server&quot;**  Attribut.
4. Anschließend fügen Sie eine zusätzliche Eigenschaft der [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement. Platzieren die Einfügemarke direkt nach dem **Asp: Label** in der  **&lt;Asp: Label&gt;**  markieren, und drücken Sie dann die **LEERTASTE**.

    Eine Dropdownliste wird angezeigt, in dem die Liste der verfügbaren Eigenschaften angezeigt, Sie, für festlegen können, eine [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement. Dieses Feature genannt **IntelliSense**, hilft Ihnen in **Quelle** Ansicht mit der Syntax von Serversteuerelementen, HTML-Elemente und andere Elemente auf der Seite. Die folgende Abbildung zeigt die **IntelliSense** Dropdown-Liste für die [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement.

    ![IntelliSense-Attribute](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense-Attribute")
5. Wählen Sie **ForeColor** , und geben Sie ein Gleichheitszeichen.

    IntelliSense zeigt eine Liste von Farben an.

    > [!NOTE] 
    > 
    > Sie können anzeigen, eine **IntelliSense** Dropdown-Liste jederzeit durch Drücken von **STRG + J** beim Anzeigen von Code.
6. Wählen Sie eine Farbe für die  **[Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)**  Text des Steuerelements. Stellen Sie sicher, dass Sie eine Farbe auswählen, die dunkel gegen einen weißen Hintergrund gelesen wird.

    Die **ForeColor** Attribut mit der Farbe, die Sie ausgewählt haben, z. B. das schließende Anführungszeichen, abgeschlossen ist.


### <a name="programming-the-button-control"></a>Programmieren das Button-Steuerelement


In dieser exemplarischen Vorgehensweise schreiben Sie Code, der den Namen gelesen werden, die der Benutzer gibt es in das Textfeld, und zeigt dann den Namen in der [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement.

### <a name="add-a-default-button-event-handler"></a>Fügen Sie ein Standard-Ereignishandler hinzu.


1. Wechseln Sie zur **Entwurf** anzeigen.
2. Doppelklicken Sie auf die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Steuerelement.

    Standardmäßig Visual Studio wechselt zu einer Code-Behind-Datei und erstellt einen Skeleton-Ereignishandler für das [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) Standardereignis des Steuerelements, das [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) Ereignis. Die Code-Behind-Datei trennt UI Markup (z. B. HTML) aus dem Servercode (z. B. c#).   
Code für diesen Ereignishandler hinzugefügt, wird der Cursor positioniert.

    > [!NOTE] 
    > 
    > Durch Doppelklicken auf ein Steuerelement in **Entwurf** Sicht ist nur eine von mehreren Möglichkeiten können Sie Ereignishandler erstellen.
3. Innerhalb der **Button1\_klicken Sie auf** -Ereignishandler, Typ **Label1** gefolgt von einem Punkt (**.**).

    Bei den Zeitraum nach der Eingabe der **ID** der Bezeichnung (**Label1**), Visual Studio zeigt eine Liste der verfügbaren Elemente für die [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) zu steuern, wie das folgende Beispiel zeigt Abbildung. Ein Element im Allgemeinen eine Eigenschaft, eine Methode oder ein Ereignis.

    ![IntelliSense in der Codeansicht](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense in der Codeansicht")
4. Fertig stellen die **klicken Sie auf** -Ereignishandler für die Schaltfläche, damit die It aussieht, wie im folgenden Codebeispiel gezeigt.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Wechseln Sie zurück zum Anzeigen der **Quelle** Überblick über Ihre HTML-Markup, indem Sie mit der rechten Maustaste *FirstWebPage.aspx* in der **Projektmappen-Explorer** auswählen und **anzeigen Markup**.
6. Führen Sie einen Bildlauf zu der  **&lt;Asp: Schaltfläche&gt;**  Element. Beachten Sie, dass die  **&lt;Asp: Schaltfläche&gt;**  Element verfügt jetzt über das Attribut **Onclick =&quot;Button1\_klicken Sie auf&quot;**.

    Dieses Attribut bindet der Schaltfläche [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) Ereignis, um die Ereignishandlermethode, die Sie im vorherigen Schritt codiert.

    Ereignishandlermethoden können beliebig benannt werden. der Name, die angezeigt wird von Visual Studio erstellte Standardname. Der wichtige Punkt ist, dass der Name verwendet die **OnClick** Attribut im HTML-Code muss dem Namen einer Methode im Code-Behind definierten entsprechen.


### <a name="running-the-page"></a>Ausführen der Seite


Sie können jetzt die Steuerelemente auf der Seite testen.

### <a name="to-run-the-page"></a>Um die Seite auszuführen


1. Drücken Sie **STRG + F5** auf die Seite im Browser ausgeführt. Wenn ein Fehler auftritt, überprüfen Sie die oben beschriebenen Schritte aus.
2. Geben Sie einen Namen in das Textfeld ein, und klicken Sie auf die **Anzeigenamen** Schaltfläche.

    Der eingegebene Name wird angezeigt, der [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement. Beachten Sie, dass wenn Sie auf die Schaltfläche klicken, die Seite an den Webserver zurückgesendet wird. ASP.NET klicken Sie dann die Seite neu, führt den Code (in diesem Fall die [Schaltfläche](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) des Steuerelements [klicken Sie auf](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) -Ereignishandler ausgeführt), und klicken Sie dann die neue Seite an den Browser sendet. Wenn Sie die Statusleiste im Browser zu beobachten, sehen Sie sich, dass die Seite einen Roundtrip an dem Webserver jedes Mal macht Sie auf die Schaltfläche klicken.
3. Zeigen Sie im Browser die Quelle der Seite, die Sie ausführen, indem Sie auf der Seite mit der rechten Maustaste und auswählen **Quelltext anzeigen**.

    In den Quellcode der Seite sehen Sie HTML, ohne Servercode. Insbesondere nicht angezeigt. die  **&lt;Asp:&gt;**  Elemente, die Sie mit arbeiteten **Quelle** anzeigen. Wenn die Seite ausgeführt wird, wird von ASP.NET Serversteuerelemente verarbeitet und rendert die HTML-Elemente auf der Seite ", mit denen die Funktionen, die die Darstellung des Steuerelements. Z. B. die  **&lt;Asp: Schaltfläche&gt;**  Steuerelement wird als HTML gerendert  **&lt;Eingabetyp =&quot;übermitteln&quot; &gt;**  Element.
4. Schließen Sie den Browser.


## <a name="working-with-additional-controls"></a>Arbeiten mit zusätzlichen Steuerelemente

<a id="sectionToggle2"></a>

In diesem Teil der exemplarischen Vorgehensweise arbeiten Sie mit der [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement, das zu einem Zeitpunkt Datumsangaben pro Monat anzeigt. Die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement ist ein komplexer Steuerelement als die Schaltfläche Textfeld und Bezeichnung gearbeitet haben. mit und veranschaulicht einige weitere Funktionen enthaltener Serversteuerelemente.

In diesem Abschnitt fügen Sie eine [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) die Steuerung an die Seite, und formatieren Sie ihn.

### <a name="to-add-a-calendar-control"></a>So fügen Sie einem Monatskalender-Steuerelement hinzu


1. Wechseln Sie in Visual Studio zum **Entwurf** anzeigen.
2. Aus der **Standard** Teil der **Toolbox**, ziehen Sie eine [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) steuern, auf die Seite, und legen Sie sie unterhalb der **Div** Element, die anderen Steuerelemente enthält.

    Der Kalender Smarttagbereich wird angezeigt. Der Bereich zeigt die Befehle, die Sie für die häufigsten Aufgaben für das ausgewählte Steuerelement zu erleichtern. Die folgende Abbildung zeigt die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) steuern in gerenderten **Entwurf** anzeigen.

    ![Monatskalender-Steuerelement in der Entwurfsansicht](creating-a-basic-web-forms-page/_static/image13.png "Monatskalender-Steuerelement in der Entwurfsansicht")
3. Wählen Sie im Smarttagbereich, **AutoFormat**.

    Die **AutoFormat** Dialogfeld wird angezeigt, können Sie ein Schema für die Formatierung für den Kalender auswählen. Die folgende Abbildung zeigt die **AutoFormat** im Dialogfeld für die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement.

    ![Auto-Format (Dialogfeld) (Calendar-Steuerelement)](creating-a-basic-web-forms-page/_static/image14.png "AutoFormat (Dialogfeld) (Calendar-Steuerelement)")
4. Aus der **wählen Sie ein Schema** wählen **einfache** , und klicken Sie dann auf **OK**.
5. Wechseln Sie zur **Quelle** anzeigen.

    Sehen Sie die  **&lt;Asp: Kalender&gt;**  Element. Dieses Element ist wesentlich länger als die Elemente für einfache Steuerelemente, die Sie zuvor erstellt haben. Es enthält auch Unterelemente, z. B.  **&lt;WeekEndDayStyle&gt;**, die verschiedenen formatierungseinstellungen darstellen. Die folgende Abbildung zeigt die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) steuern **Quelle** anzeigen. (Das genaue Markup, das Sie in finden Sie unter **Quelle** Ansicht möglicherweise unterscheiden sich geringfügig von der Abbildung.)

    ![Monatskalender-Steuerelement in der Quellansicht](creating-a-basic-web-forms-page/_static/image15.png "Monatskalender-Steuerelement in der Quellansicht")


### <a name="programming-the-calendar-control"></a>Programmieren das Kalender-Steuerelement


In diesem Abschnitt programmieren Sie die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement, das derzeit ausgewählte Datum anzuzeigen.

### <a name="to-program-the-calendar-control"></a>So programmieren Sie das Kalender-Steuerelement


1. In **Entwurf** anzuzeigen, doppelklicken Sie auf die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement.

    Ein neuen Ereignishandler erstellt und angezeigt, in der Code-Behind-Datei mit dem Namen *FirstWebPage.aspx.cs*.
2. Fertig stellen die [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) -Ereignishandler durch den folgenden Code.


    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

 Der obige Code wird der Text des Label-Steuerelements auf das ausgewählte Datum Rand des Kalendersteuerelements an.


### <a name="running-the-page"></a>Ausführen der Seite


Sie können nun den Kalender testen.

### <a name="to-run-the-page"></a>Um die Seite auszuführen


1. Drücken Sie **STRG + F5** auf die Seite im Browser ausgeführt.
2. Klicken Sie auf ein Datum im Kalender.

    Das Datum, die Sie geklickt wird angezeigt, der [Bezeichnung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) Steuerelement.
3. Zeigen Sie den Quellcode für die Seite im Browser an.

    Beachten Sie, dass die [Kalender](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) Steuerelement gerendert wurden auf die Seite als ein **Tabelle**, mit jeder Tag als eine **td** Element.
4. Schließen Sie den Browser.


## <a name="next-steps"></a>Nächste Schritte


<a id="nextStepsToggle"></a>

In dieser exemplarischen Vorgehensweise wurden die grundlegenden Funktionen von Visual Studio-Seite-Designer veranschaulicht. Nun, dass Sie wissen, wie zum Erstellen und bearbeiten eine Web Forms-Seite in Visual Studio, empfiehlt es sich um andere Funktionen zu untersuchen. Sie möchten z. B. folgende Aktionen ausführen:

- Weitere Informationen zu ASP.NET Web Forms anhand der schrittweise Tutorial Reihe [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Weitere Informationen zu Cascading Stylesheets (CSS). Weitere Informationen finden Sie unter [arbeiten mit CSS Overview](https://msdn.microsoft.com/library/bb398931.aspx).
