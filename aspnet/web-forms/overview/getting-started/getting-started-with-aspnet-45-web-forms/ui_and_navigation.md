---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Benutzeroberfläche und Navigation | Microsoft Docs
author: Erikre
description: Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: d2d4101455a85c53e016e567c0cf1337642f1863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="ui-and-navigation"></a>Benutzeroberfläche und Navigation
====================
by [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die diese Reihe von Lernprogrammen begleitet.


In diesem Lernprogramm ändern Sie die Benutzeroberfläche von der standardwebanwendung übernommen, die Funktionen des Wingtip Toys front Store-Anwendung zu unterstützen. Darüber hinaus fügen Sie einfach und datengebundene Navigation. Dieses Lernprogramm baut auf dem vorherigen Lernprogramm "Erstellen der Datenzugriffsebene" und ist Teil der Wingtip Toys Reihe von Lernprogrammen.

## <a name="what-youll-learn"></a>Lernen Sie:

- So ändern Sie die Benutzeroberfläche für die Funktionen des Wingtip Toys front Store-Anwendung unterstützt werden.
- So konfigurieren Sie eine HTML5-Element, um die Seitennavigation enthalten werden.
- Vorgehensweise: erstellen ein datengesteuerten Steuerelements zu bestimmten Produktdaten navigieren.
- Vorgehensweise beim Anzeigen von Daten aus einer Datenbank mithilfe von Entity Framework Code First erstellt.

ASP.NET Web Forms können Sie dynamische Inhalte für Ihre Webanwendung zu erstellen. Jede ASP.NET-Webseite ist ähnlich wie eine statische HTML-Webseite (eine Seite, die keine serverbasierte Verarbeitung umfasst) erstellt, aber ASP.NET-Webseite umfasst zusätzliche Elemente, die ASP.NET erkennt und verarbeitet werden, um HTML-Daten zu generieren, wenn die Seite ausgeführt wird.

Eine statische HTML-Seite (*htm* oder *.html* Datei), erfüllt der Server eine `Web` Anforderung durch Lesen der Datei und das Senden als-wird an den Browser. Wenn eine Person im Gegensatz dazu eine ASP.NET-Webseite anfordert (*aspx* Datei), die Seite als ein Programm auf dem Webserver ausgeführt wird. Während die Seite ausgeführt wird, können sie eine beliebige Aufgabe ausführen, die Ihre Website erforderlich sind, einschließlich berechnen von Werten, beim Lesen oder Schreiben von Datenbankinformationen oder andere Programme aufrufen. Als Ausgabe wird die Seite dynamisch erzeugt Auszeichnungen (wie z. B. in HTML-Elemente), und sendet diese dynamische Ausgabe an den Browser.

## <a name="modifying-the-ui"></a>Ändern die Benutzeroberfläche

Durch Ändern dieser Reihe von Lernprogrammen weiterhin die *"default.aspx"* Seite. Ändern Sie die Benutzeroberfläche, die von der Standardvorlage zum Erstellen der Anwendung verwendet bereits eingerichtet ist. Der Typ der Änderungen, die Sie erledigen sind typisch für die Erstellung eine Web Forms-Anwendung. Sie müssen dies durch Ändern des Titels, manche Inhalte ersetzen und Löschen nicht benötigte Standardinhalt erreichen.

1. Öffnen oder wechseln Sie zu der *"default.aspx"* Seite.
2. Wenn die Seite wird, in angezeigt **Entwurf** anzuzeigen, wechseln Sie zur **Quelle** anzeigen.
3. Am oberen Rand der Seite in der `@Page` -Direktive Änderung der `Title` -Attribut "Willkommen", wie im folgenden gelb hervorgehoben angezeigt. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Außerdem auf die *"default.aspx"* Seite, ersetzen Sie alle Standardeinstellungen Inhalt enthalten, die der `<asp:Content>` kennzeichnen, sodass das Markup als angezeigt wird unten. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Speichern Sie die *"default.aspx"* Seite durch Auswahl **speichern "default.aspx"** aus der **Datei** Menü.

   Das resultierende *"default.aspx"* Seite wird wie folgt angezeigt: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

Im Beispiel festgelegt haben die `Title` Attribut von der `@Page` Richtlinie. Wenn der HTML-Code angezeigt wird, in einem Browser, den Servercode `<%: Page.Title %>` aufgelöst wird, um den Inhalt in die `Title` Attribut.

Beispielseite "enthält die grundlegenden Elemente, die eine ASP.NET-Webseite bilden. Die Seite enthält statischen Text an, wie Sie in einer HTML-Seite, zusammen mit den Elementen, die für ASP.NET spezifisch sind. Der Inhalt der *"default.aspx"* Seite wird mit dem Inhalt der Masterseite, weiter unten in diesem Lernprogramm erläutert werden integriert werden.

### <a name="page-directive"></a>@Page Richtlinie

ASP.NET Web Forms enthalten normalerweise Direktiven, die Ihnen ermöglichen, Seiteninformationen Eigenschaften und der Konfiguration für die Seite anzugeben. Die Direktiven werden als Anweisungen zum Prozess der Seite von ASP.NET verwendet, aber sie werden nicht gerendert, im Rahmen der das Markup, das an den Browser gesendet wird.

Die am häufigsten verwendete Direktive ist die `@Page` -Direktive, die Ihnen ermöglicht, viele Konfigurationsoptionen für die Seite, einschließlich der folgenden anzugeben:

1. Der Server für den Code in die Seite, z. B. C#-Programmiersprache.
2. Gibt an, ob die Seite für eine Seite mit Servercode direkt auf der Seite ist die einer Einzeldateiseite aufgerufen wird, oder gibt an, ob es eine Seite mit Code in eine separate Klassendatei ist der Code-Behind-Seite aufgerufen wird.
3. Gibt an, ob die Seite verfügt über einen zugeordneten Masterseite und daher als Inhaltsseite behandelt werden soll.
4. Debuggen und die Ablaufverfolgungsoptionen.

Wenn Sie nicht einschließen, eine `@Page` auf der Seite Richtlinie oder wenn die Richtlinie keine bestimmte Einstellung enthält, wird eine Einstellung geerbt von der *"Web.config"* Konfigurationsdatei oder aus der *"Machine.config"* Konfigurationsdatei. Die *"Machine.config"* Datei enthält zusätzliche Konfigurationseinstellungen für alle Anwendungen, die auf einem Computer ausgeführt.

> [!NOTE] 
> 
> Die *"Machine.config"* auch enthält Details über alle möglichen Konfigurationseinstellungen.


### <a name="web-server-controls"></a>Webserversteuerelemente

In den meisten ASP.NET Web Forms-Anwendungen fügen Sie Steuerelemente, die den Benutzer interagieren mit der Seite, z. B. Schaltflächen, Textfelder, Listen und So weiter zu ermöglichen. Diese Webserversteuerelemente werden ähnlich wie HTML-Schaltflächen und Eingabeelemente. Allerdings sind sie auf dem Server, sodass Sie Servercode zu verwenden, deren Eigenschaften festlegen verarbeitet. Diese Steuerelemente auch Auslösen Ereignisse, die Sie im Server-Code behandeln können.

Serversteuerelemente verwenden eine spezielle Syntax, die ASP.NET erkennt, wenn die Seite ausgeführt wird. Der Tagname für ASP.NET-Serversteuerelemente beginnt mit einem `asp:` Präfix. Dadurch können ASP.NET erkennt und verarbeiten diese Serversteuerelemente. Das Präfix ist möglicherweise anders, wenn das Steuerelement nicht Teil von .NET Framework ist. Zusätzlich zu den `asp:` Präfix, auch ASP.NET-Serversteuerelemente enthalten die `runat="server"` Attribut und ein `ID` , Sie verwenden können, um auf das Steuerelement im Servercode zu verweisen.

Wenn die Seite ausgeführt wird, wird ASP.NET Serversteuerelemente identifiziert und führt den Code, der diese Kontrollen zugeordnet ist. Viele Steuerelemente rendern HTML oder sonstige Markups in die Seite, wenn er in einem Browser angezeigt wird.

### <a name="server-code"></a>Servercode

Die meisten ASP.NET Web Forms-Anwendungen enthalten Code, der auf dem Server ausgeführt wird, wenn die Seite verarbeitet wird. Wie bereits erwähnt, kann die Servercode zu einer Vielzahl von Aktionen, wie das Hinzufügen von Daten zu einem ListView-Steuerelement verwendet werden. ASP.NET unterstützt zahlreiche Sprachen zur Ausführung auf dem Server, einschließlich der C#-, Visual Basic, j# und andere.

ASP.NET unterstützt zwei Modelle für das Schreiben von Servercode für eine Webseite an. Der Code für die Seite befindet sich in das Modell nur einer Datei in ein Script-Element, in denen das öffnende Tag enthält, die `runat="server"` Attribut. Alternativ können Sie den Code für die Seite in eine separate Klassendatei erstellen, die als Code-Behind-Modell bezeichnet wird. In diesem Fall enthält die ASP.NET Web Forms-Seite in der Regel keine Servercode. Stattdessen die `@Page` Richtlinie enthält Informationen, verknüpft der *aspx* Seite mit der zugehörigen Code-Behind-Datei.

Die `CodeBehind` Attribut, das der `@Page` Richtlinie gibt den Namen der separate Klassendatei, und die `Inherits` -Attribut gibt den Namen der Klasse innerhalb der Code-Behind-Datei, die auf der Seite entspricht.

### <a name="updating-the-master-page"></a>Aktualisieren die Seite "Master"

In ASP.NET Web Forms können mit Masterseiten ein konsistentes Layout für die Seiten in der Anwendung zu erstellen. Eine einzelne Masterseite definiert die Aussehen und Verhalten und die gemäß dem Standardverhalten, die Sie für alle Seiten (oder eine Gruppe von Seiten) werden soll, in der Anwendung. Sie können dann einzelne Seiten erstellen, die den Inhalt, die, den Sie anzeigen möchten enthalten, wie oben erläutert. Wenn Benutzer die Inhaltsseiten anfordern, werden Sie in ASP.NET mit Ausgaben erzeugen, die das Layout der Masterseite mit dem Inhalt der Seite Inhalt kombiniert die Masterseite zusammengeführt.

Der neue Standort benötigt einen einzelnen Logo aus, auf jeder Seite angezeigt. Um dieses Logo hinzufügen, können Sie den HTML-Code auf der Seite "master" ändern.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die **Site.Master** Seite.
2. Wenn die Seite im **Entwurf** anzuzeigen, wechseln Sie zur **Quelle** anzeigen.
3. Aktualisieren die Masterseite von **ändern oder Hinzufügen von** das Markup gelb hervorgehoben: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Folgenden HTML-Code zeigt das Bild mit dem Namen *logo.jpg* aus der *Bilder* Ordner der Webanwendung, die Sie später hinzufügen. Wenn eine Seite, die Gestaltungsvorlage verwendet, die in einem Browser angezeigt wird, wird das Logo angezeigt. Wenn ein Benutzer auf das Logo klickt, wird der Benutzer zurück zum Navigieren der *"default.aspx"* Seite. Die HTML-Ankertag `<a>` dient als Wrapper für das Image-Steuerelement und das Bild, das als Teil der Verknüpfung einbezogen werden können. Die `href` -Attribut für das Anchortag Stamm gibt "`~/`" der Website als Link-Speicherort. Wird standardmäßig die *"default.aspx"* Seite wird angezeigt, wenn der Benutzer in das Stammverzeichnis der Website navigiert. Die **Image** `<asp:Image>` Webserversteuerelement enthält außerdem Eigenschaften, z. B. `BorderStyle`, bei der Anzeige in einem Browser als HTML gerendert.

### <a name="master-pages"></a>Masterseiten

Eine Masterseite ist eine ASP.NET-Datei mit der Erweiterung .master (z. B. *Site.Master*) mit einem vordefinierten Layout, statischen Text, HTML-Elemente und Steuerelemente enthalten kann. Die Masterseite wird durch eine spezielle identifiziert `@Master` -Direktive, die ersetzt die `@Page` Richtlinie für die normalen verwendeten *aspx* Seiten.

Zusätzlich zu den `@Master` -Direktive die Gestaltungsvorlage enthält auch alle Elemente der obersten Ebene HTML für eine Seite, wie z. B. `html`, `head`, und `form`. Auf der Masterseite Sie oben hinzugefügt haben, verwenden Sie z. B. ein HTML `table` für das Layout einer `img` -Element für die Firmenlogo, statischen Text und Serversteuerelemente, allgemeine Mitgliedschaft für Ihre Website zu behandeln. Sie können die HTML-Elemente und ASP.NET-Elemente als Teil der Masterseite verwenden.

Zusätzlich zu den statischen Text und Steuerelemente, die auf allen Seiten angezeigt werden, umfasst die Gestaltungsvorlage auch eine oder mehrere **ContentPlaceHolder** Steuerelemente. Diese Platzhaltersteuerelemente definieren Bereiche, wo ersetzbare Inhalt angezeigt wird. Wiederum ersetzbare Inhalt ist definiert in Inhaltsseiten, z. B. *"default.aspx"*unter Verwendung der **Inhalt** Serversteuerelement.

#### <a name="adding-image-files"></a>Hinzufügen von Bilddateien

Die Webanwendung muss das Logobild, das oben, zusammen mit allen Produktbilder verwiesen wird hinzugefügt werden, damit sie angezeigt werden können, wenn das Projekt in einem Browser angezeigt wird.

#### <a name="download-from-msdn-samples-site"></a>Herunterladen von MSDN-Codebeispiele für Standort:

[Erste Schritte mit ASP.NET 4.5 WebForms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

Der Download umfasst Ressourcen in der *WingtipToys Bestand* Ordner, die zum Erstellen der beispielanwendung verwendet werden.

1. Wenn Sie dies nicht bereits getan haben Laden Sie die komprimierte Beispieldateien, die mit den oben aufgeführten Link von der Website für MSDN-Beispiele herunter.
2. Nachdem das Download abgeschlossen ist, öffnen Sie die ZIP-Datei, und kopieren Sie den Inhalt in einen lokalen Ordner auf Ihrem Computer.
3. Suchen und öffnen Sie die *WingtipToys Bestand* Ordner.
4. Durch Ziehen und ablegen, kopieren Sie die *Katalog* Ordner aus Ihrem lokalen Ordner, in das Stammverzeichnis für das Webprojekt für die Anwendung in der **Projektmappen-Explorer** von Visual Studio. 

    ![Benutzeroberfläche und Navigation - Dateien kopieren](ui_and_navigation/_static/image1.png)
5. Als Nächstes erstellen Sie einen neuen Ordner namens *Bilder* durch Rechtsklick auf die **WingtipToys** Projekt **Projektmappen-Explorer** auswählen und **hinzufügen**  - &gt; **Neuer Ordner**.
6. Kopieren der *logo.jpg* -Datei von der *WingtipToys Bestand* Ordner in **Datei-Explorer** auf die *Bilder* Ordner der Webanwendung im Projekt **Projektmappen-Explorer** von Visual Studio.
7. Klicken Sie auf die **alle Dateien anzeigen** Option am Anfang **Projektmappen-Explorer** zum Aktualisieren der Liste der Dateien, wenn die neuen Dateien nicht angezeigt wird.  
  
    **Projektmappen-Explorer** zeigt jetzt die aktualisierte Projektdateien. 

    ![Benutzeroberfläche und Navigation - Projektmappen-Explorer](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Hinzufügen von Seiten

Vor dem Hinzufügen von Navigation auf die Webanwendung aus, fügen Sie zuerst zwei neue Seiten, denen wechseln Sie zu. Weiter unten in diesem Lernprogramm Reihe müssen Sie Produkte und Produktinformationen auf diesen neuen Seiten anzeigen.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste **WingtipToys**, klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.   
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Aktivieren Sie das Kontrollkästchen **Webformular mit Gestaltungsvorlage** aus der Mitte aus, und nennen Sie sie *"ProductList.aspx"*. 

    ![Fügen Sie Benutzeroberfläche und Navigation - Dialogfeld "Neues Element" hinzu.](ui_and_navigation/_static/image3.png)
3. Wählen Sie **Site.Master** anzufügende Masterseite auf das neu erstellte *aspx* Seite. 

    ![Wählen Sie Benutzeroberfläche und Navigation - Gestaltungsvorlage](ui_and_navigation/_static/image4.png)
4. Fügen Sie eine zusätzliche Seite mit dem Namen *ProductDetails.aspx* durch Befolgen dieselben Schritte aus.

### <a name="updating-bootstrap"></a>Bootstrap aktualisieren

Verwenden Sie die Projektvorlagen für Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), ein Layout und Designumgebung-Framework von Twitter erstellt. Bootstrap verwendet CSS3 reaktionsfähiges Design bereitstellen, was bedeutet, dass Layouts dynamisch an die anderen Browser Fenstergrößen angepasst werden können. Der Bootstrap-Designumgebung-Funktion können auch einfach eine Änderung in der Anwendung aussehen und Verhalten wirksam. Standardmäßig enthält der ASP.NET Web Application-Vorlage in Visual Studio 2013 Bootstrap als NuGet-Paket.

In diesem Lernprogramm ändern Sie Aussehen und Verhalten der Anwendung Wingtip Toys, durch den Bootstrap-CSS-Dateien ersetzen.

1. In **Projektmappen-Explorer**öffnen die *Content* Ordner.
2. Mit der rechten Maustaste die *bootstrap.css* Datei, und benennen Sie sie um *Bootstrap-original.css*.
3. Benennen Sie die *bootstrap.min.css* auf *Bootstrap-original.min.css*.
4. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Content* Ordner, und wählen **Ordner in Datei-Explorer öffnen**.  
   Die Datei-Explorer wird angezeigt. Sie werden an diesem Speicherort heruntergeladenen bootstrap CSS-Dateien speichern.
5. Wechseln Sie in Ihrem Browser zur [ http://Bootswatch.com ](http://bootswatch.com/).
6. Führen Sie einen Bildlauf im Browserfenster, bis Sie das Design Schwachstelle angezeigt. 

    ![Benutzeroberfläche und Navigation - Schwachstelle Design](ui_and_navigation/_static/image5.png)
7. Laden Sie beide die *bootstrap.css* Datei und die *bootstrap.min.css* Datei wird in der *Content* Ordner. Verwenden Sie den Pfad in den Inhaltsordner aus, die in angezeigt wird der **Datei-Explorer** Fenster, das Sie zuvor geöffnet.
8. In **Visual Studio** am oberen Rand **Projektmappen-Explorer**, wählen die **alle Dateien anzeigen** Option aus, um die neuen Dateien im Ordner "Content" angezeigt. 

    ![Benutzeroberfläche und Navigation - Projektmappen-Explorer](ui_and_navigation/_static/image6.png)

   Daraufhin werden die beiden neuen CSS-Dateien in den **Content** Ordner, aber beachten Sie, die das Symbol neben jedem Dateinamen abgeblendet ist. Dies bedeutet, dass die Datei noch nicht zum Projekt hinzugefügt wurde.
9. Mit der rechten Maustaste die *bootstrap.css* und *bootstrap.min.css* Dateien, und wählen **zu Projekt hinzufügen**.   
   Wenn Sie die Anwendung des Wingtip Toys weiter unten in diesem Lernprogramm ausführen, wird die neue Benutzeroberfläche angezeigt.

> [!NOTE] 
> 
> Der ASP.NET Web Application-Vorlage verwendet die *Bundle.config* Datei im Stammverzeichnis des Projekts zum Speichern des Pfades der Bootstrap-CSS-Dateien.


### <a name="modifying-the-default-navigation"></a>Ändern die Standardnavigation

Die Standardnavigation für jede Seite in der Anwendung kann geändert werden, durch Ändern der ungeordnete Liste Navigationselement, die in der *Site.Master* Seite.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *Site.Master* Seite.
2. Fügen Sie den zusätzlichen Navigationslink der ungeordnete Liste unten gelb hervorgehoben:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Wie im obigen HTML-Code sehen ist, werden Sie geändert jedes Zeilenelement `<li>` ein Ankertag mit `<a>` mit einem Link `href` Attribut. Jede `href` verweist auf eine Seite in der Webanwendung. Klickt ein Benutzer auf einen dieser Links im Browser (z. B. **Produkte**), navigieren sie zu der Seite, die in enthaltenen der `href` (z. B. **"ProductList.aspx"**). Die Anwendung wird am Ende dieses Lernprogramms ausgeführt werden.

> [!NOTE] 
> 
> Die Tilde (`~`) Zeichen wird verwendet, um anzugeben, dass die `href` Pfad beginnt am Stamm des Projekts.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Hinzufügen eines Datensteuerelements zum Anzeigen von Navigationsdaten

Als Nächstes fügen Sie ein Steuerelement, um alle Kategorien aus der Datenbank angezeigt. Jede Kategorie fungiert als Link zu der *"ProductList.aspx"* Seite. Klickt ein Benutzer auf den Link einer Kategorie im Browser, sie auf der Seite "Produkte" navigieren und finden Sie unter nur die Produkte, die der ausgewählten Kategorie zugeordnet ist.

Verwenden Sie eine **ListView** Steuerelement, um alle in der Datenbank enthaltenen Kategorien anzuzeigen. Hinzufügen einer **ListView** Steuerelement der Masterseite:

1. In der *Site.Master* Seite, fügen Sie die folgenden hervorgehobenen `<div>` Element **nach** der `<div>` mit der `id="TitleContent"` , die Sie zuvor hinzugefügt haben:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Mit diesem Code werden alle Kategorien aus der Datenbank angezeigt. Die **ListView** Steuerelement jeder Kategoriename als Text des links zeigt und enthält einen Link zu der *"ProductList.aspx"* Seite mit einer Abfragezeichenfolge Wert mit der `ID` der Kategorie. Durch Festlegen der `ItemType` Eigenschaft in der **ListView** zu steuern, die Datenbindungsausdruck `Item` steht innerhalb der `ItemTemplate` Knoten und das Steuerelement wird stark typisiert. Auszuwählender Details zu den `Item` -Objekt mithilfe von IntelliSense, z. B. Angeben der `CategoryName`. Dieser Code befindet sich innerhalb des Containers `<%#: %>` , die einen Datenbindungsausdruck markiert. Durch Hinzufügen der (:) bis zum Ende der `<%#` Präfix, das Ergebnis der Datenbindungsausdruck ist HTML-codiert. Wenn das Ergebnis HTML-codiert ist, wird Ihre Anwendung besser Cross-Site geschützt script-Injection (XSS) und HTML-Injection-Angriffe.

> [!NOTE] 
> 
> **Tipps**
> 
> Wenn Sie Code hinzufügen, indem Sie eingeben, während der Entwicklung, können Sie sicherstellen, dass ein gültiger Member eines Objekts gefunden wird, da stark typisiert, Datensteuerelemente Anzeigen der verfügbaren Elemente, die basierend auf IntelliSense. IntelliSense bietet kontextabhängigen Auswahlmöglichkeiten, während der Eingabe von Code, z. B. Eigenschaften, Methoden und Objekte.


Im nächsten Schritt implementieren Sie die `GetCategories` Methode, um Daten abzurufen.

### <a name="linking-the-data-control-to-the-database"></a>Verknüpfen das Steuerelement mit der Datenbank

Bevor Sie Daten in das Steuerelement anzeigen können, müssen Sie das Steuerelement mit der Datenbank zu verknüpfen. Um den Link zu machen, können Sie den Code hinter der Ändern der *Site.Master.cs* Datei.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Site.Master* Seite, und klicken Sie dann auf **Code anzeigen**. Die *Site.Master.cs* Datei wird im Editor geöffnet.
2. Am Anfang der *Site.Master.cs* Datei, die zwei zusätzliche Namespaces hinzufügen, sodass die enthaltenen Namespaces wie folgt aussehen:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Fügen Sie den hervorgehobenen `GetCategories` Methode nach der `Page_Load` -Ereignishandler wie folgt:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Der obige Code wird ausgeführt, wenn eine andere Seite, die die Gestaltungsvorlage verwendet im Browser geladen wird. Die `ListView` Steuerelement (benannte "CategoryList"), die Sie zuvor in diesem Lernprogramm hinzugefügt wurden die modellbindung zum Auswählen von Daten verwendet. Im Markup der `ListView` Steuerelement festlegen, dass des Steuerelements `SelectMethod` Eigenschaft, um die `GetCategories` oben gezeigten Methode. Die `ListView` Aufrufe steuern die `GetCategories` Methode zum entsprechenden Zeitpunkt während der Lebensdauer der Seite der Reihe nach und die zurückgegebenen Daten automatisch gebunden. Sie erfahren mehr über das Binden von Daten in den nächsten Lernprogrammen.

### <a name="running-the-application-and-creating-the-database"></a>Ausführen der Anwendung und zum Erstellen der Datenbank

Weiter oben in diesem Lernprogramm eine Initialisiererklasse (mit dem Namen "ProductDatabaseInitializer") erstellt und angegeben dieser Klasse in der *global.asax.cs* Datei. Das Entity Framework generiert die Datenbank aus, wenn die Anwendung beim ersten da ausgeführt wird die `Application_Start` -Methode aus der *global.asax.cs* Datei wird rufen die Initialisiererklasse. Verwendung der Initialisiererklasse die Modellklassen (`Category` und `Product`), die Sie weiter oben in diesem Lernprogramm zum Erstellen der Datenbank hinzugefügt.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *"default.aspx"* Seite und wählen Sie **als Startseite festlegen**.
2. In Visual Studio drücken Sie **F5**.   
 Es dauert eine gewisse Zeit alles, was bei dieser ersten Ausführung einrichten.   
    ![Benutzeroberfläche und Navigation - Browser-Fenster](ui_and_navigation/_static/image7.png)  
 Wenn Sie die Anwendung ausführen, wird die Anwendung kompiliert werden, und die Datenbank mit dem Namen *wingtiptoys.mdf* erstellt werden, der *App\_Daten* Ordner. Im Browser sehen Sie ein Navigationsmenü Kategorie. Dieses Menü wurde durch Abrufen der Kategorien aus der Datenbank generiert. Implementieren Sie die Navigation in den nächsten Lernprogrammen.
3. Schließen Sie den Browser, um die ausgeführte Anwendung zu beenden.

### <a name="reviewing-the-database"></a>Überprüfen die Datenbank

Öffnen der *"Web.config"* Datei, und sehen Sie sich den Abschnitt mit der Verbindungszeichenfolge. Sie sehen, dass die `AttachDbFilename` Wert in der Verbindungszeichenfolge verweist auf die `DataDirectory` für das Webprojekt für die Anwendung. Der Wert `|DataDirectory|` ist ein reservierter Wert darstellt, die die *App\_Daten* Ordner des Projekts. Dieser Ordner ist, in dem die Datenbank, die über die Entitätsklassen erstellt wurde, befindet.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Wenn die *App\_Daten* Ordner ist nicht sichtbar, oder wenn der Ordner leer ist, wählen Sie die **aktualisieren** Symbol und dann die **alle Dateien anzeigen** Symbol am oberen Rand der **Projektmappen-Explorer** Fenster. Erweitern die Breite der **Projektmappen-Explorer** Windows möglicherweise erforderlich, um alle verfügbaren Symbole anzuzeigen.


Nachdem Sie, die Daten prüfen können in der *wingtiptoys.mdf* Datenbankdatei mit der **Server-Explorer** Fenster.

1. Erweitern Sie die *App\_Daten* Ordner. Wenn die *App\_Daten* Ordner ist nicht sichtbar, siehe Hinweis oben.
2. Wenn die *wingtiptoys.mdf* Datenbankdatei ist nicht sichtbar ist, wählen die **aktualisieren** Symbol und dann die **alle Dateien anzeigen** Symbol am oberen Rand der **Projektmappen-Explorer**  Fenster.
3. Mit der rechten Maustaste die *wingtiptoys.mdf* Datenbankdatei, und wählen **öffnen**.  
    **Server-Explorer** wird angezeigt. 

    ![Benutzeroberfläche und Navigation - Server-Explorer](ui_and_navigation/_static/image8.png)
4. Erweitern Sie die *Tabellen* Ordner.
5. Mit der rechten Maustaste die **Produkte**Tabelle, und wählen Sie **Tabellendaten anzeigen**.  
 Die **Produkte** Tabelle wird angezeigt. 

    ![Benutzeroberfläche und Navigation - Products-Tabelle](ui_and_navigation/_static/image9.png)
6. In dieser Ansicht können Sie anzeigen und Ändern der Daten in der **Produkte** Tabelle per hand katalogisiert wird.
7. Schließen der **Produkte** Tabellenfensters.
8. In der **Server-Explorer**, mit der rechten Maustaste die **Produkte** Tabelle erneut, und wählen Sie **Tabellendefinition öffnen**.  
 Entwerfen Sie die Daten für die **Produkte** Tabelle wird angezeigt. 

    ![Benutzeroberfläche und Navigation - Produkte Entwurf](ui_and_navigation/_static/image10.png)
9. In der **T-SQL** Registerkarte sehen Sie die SQL-DDL-Anweisung, die zum Erstellen der Tabelle verwendet wurde. Sie können auch die Benutzeroberfläche in der **Entwurf** Registerkarte zum Ändern des Schemas.
10. In der **Server-Explorer**, mit der rechten Maustaste **WingtipToys** Datenbank, und wählen Sie **enge Verbindung**.   
 Durch das Trennen der Datenbank in Visual Studio, wird das Datenbankschema weiter unten in diesem Lernprogramm Reihe geändert werden können.
11. Zurück zu **Projektmappen-Explorer**durch Auswahl der **Projektmappen-Explorer** Registerkarte am unteren Rand der **Server-Explorer** Fenster.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm der Reihe haben Sie einige einfache Benutzeroberfläche, Grafiken, Seiten und Navigationsbereich hinzugefügt. Darüber hinaus haben Sie die Web-Anwendung, die die Datenbank aus den Datenklassen erstellt, die Sie im vorherigen Lernprogramm hinzugefügt ausgeführt. Sie auch den Inhalt der betrachtet die *Produkte* Tabelle der Datenbank, indem Sie die Datenbank direkt anzeigen. In den nächsten Lernprogrammen zeigen Sie Datenelemente und Details aus der Datenbank.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Einführung in ASP.NET Web Pages-Programmierung](https://msdn.microsoft.com/library/ms178125.aspx)   
[ASP.NET Web-Server steuert (Übersicht)](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[CSS-Lernprogramm](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Zurück](create_the_data_access_layer.md)
> [Weiter](display_data_items_and_details.md)
