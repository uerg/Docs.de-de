---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Benutzeroberfläche und Navigation | Microsoft-Dokumentation
author: Erikre
description: Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: 55c659cbaf48dbb02dc34e013242443d4fbd8845
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826334"
---
<a name="ui-and-navigation"></a>Benutzeroberfläche und Navigation
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die dieser tutorialreihe begleitet.


In diesem Tutorial ändern Sie die Benutzeroberfläche von der standardwebanwendung, Funktionen der Wingtip Toys-Speicher-Front-Anwendung zu unterstützen. Außerdem fügen Sie einfach und datengebundene Navigation. Dieses Tutorial baut auf dem vorherigen Lernprogramm "Erstellen der Data Access Layer" und ist Teil der tutorialreihe Wingtip Toys.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- So ändern Sie die Benutzeroberfläche, um die Funktionen der Wingtip Toys-Speicher-Front-Anwendung zu unterstützen.
- Vorgehensweise: Konfigurieren Sie ein HTML5-Element, um die Seitennavigation enthalten.
- So erstellen Sie ein Steuerelement eines datengesteuerten, um zu bestimmten Produktdaten zu navigieren.
- Die Vorgehensweise beim Anzeigen von Daten aus einer Datenbank mit Entity Framework Code First erstellt.

ASP.NET Web Forms können Sie dynamische Inhalte für Ihre Webanwendung zu erstellen. Jeder ASP.NET Webseite ähnlich zu einer statischen HTML-Webseite (eine Seite, die nicht die serverbasierte Verarbeitung enthalten ist) erstellt, aber ASP.NET-Webseite umfasst zusätzliche Elemente, die ASP.NET erkennt und verarbeitet werden, um HTML zu generieren, wenn die Seite ausgeführt wird.

Mit einer statischen HTML-Seite (*.htm* oder *.html* Datei), erfüllt der Server eine `Web` Anforderung durch Lesen der Datei, und senden ihn als-ist an den Browser. Wenn ein Benutzer im Gegensatz dazu eine ASP.NET-Webseite anfordert (*aspx* Datei), die Seite als ein Programm auf dem Webserver ausgeführt wird. Während die Seite ausgeführt wird, können sie sämtliche Aufgaben ausführen, die Ihre Website erforderlich sind, einschließlich berechnen von Werten, lesen oder Schreiben von Datenbankinformationen oder das Aufrufen anderer Programme. Als Ausgabe wird die Seite dynamisch erzeugt Markup (z. B. die Elemente im HTML-Format), und sendet diese dynamischen Ausgabe an den Browser.

## <a name="modifying-the-ui"></a>Ändern die Benutzeroberfläche

Sie müssen dieser tutorialreihe fortfahren, indem Sie ändern die *"default.aspx"* Seite. Ändern Sie die Benutzeroberfläche, die durch die Standardvorlage, die zum Erstellen der Anwendung verwendet bereits eingerichtet ist. Der Typ der Änderungen, die Sie erledigen sind typisch, wenn jeder Web Forms-Anwendung zu erstellen. Dazu müssen Sie den Titel ändern, ersetzen einige Inhalte und Entfernen nicht benötigter Standardinhalt.

1. Öffnen Sie ein, oder wechseln Sie zu der *"default.aspx"* Seite.
2. Wenn die Seite wird, in angezeigt **Entwurf** anzuzeigen, wechseln Sie zur **Quelle** anzeigen.
3. Am oberen Rand der Seite in der `@Page` Direktive, Änderung der `Title` -Attribut "Willkommen", wie im unten gelb hervorgehoben angezeigt. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. Auch auf die *"default.aspx"* Seite, ersetzen Sie alle Standardeinstellungen Inhalt innerhalb der `<asp:Content>` kennzeichnen, sodass das Markup wird, wie angezeigt unten. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Speichern Sie die *"default.aspx"* Seite **speichern "default.aspx"** aus der **Datei** im Menü.

   Die resultierende *"default.aspx"* Seite wie folgt angezeigt: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

Im Beispiel festgelegt haben die `Title` Attribut der `@Page` Richtlinie. Wenn der HTML-Code angezeigt wird, in einem Browser, den Servercode `<%: Page.Title %>` aufgelöst wird, um den Inhalt in die `Title` Attribut.

Die Beispielseite enthält die grundlegenden Elemente, die eine ASP.NET-Webseite bilden. Die Seite enthält statischen Text an, wie Sie in eine HTML-Seite mit Elementen, die spezifisch für ASP.NET. sind möglicherweise. Der Inhalt die *"default.aspx"* Seite integriert wird Inhalt der Masterseite, die später in diesem Tutorial erläutert wird.

### <a name="page-directive"></a>@Page Richtlinie

ASP.NET Web Forms enthalten in der Regel Direktiven, die Ihnen ermöglichen, Eigenschaften und der Konfiguration Seiteninformationen für die Seite anzugeben. Die Anweisungen werden von ASP.NET als Anweisungen verwendet, Informationen zum Prozess der Seite, aber sie werden nicht gerendert, im Rahmen von Markup, das an den Browser gesendet wird.

Die am häufigsten verwendete-Direktive ist die `@Page` -Direktive, die Ihnen ermöglicht, viele Konfigurationsoptionen für die Seite, einschließlich der folgenden anzugeben:

1. Der Server, die Programmiersprache für Code auf der Seite, wie z. B. c#.
2. Gibt an, ob die Seite für eine Seite mit Server-Code direkt auf der Seite ist die Einzeldatei Seite aufgerufen wird, oder gibt an, ob es eine Seite mit Code in eine separate Klassendatei, ist der Code-Behind-Seite aufgerufen wird.
3. Gibt an, ob die Seite verfügt über eine Masterseite zugeordnet ist und daher als Inhaltsseite behandelt werden soll.
4. Das Debuggen und Ablaufverfolgungsoptionen.

Wenn Sie nicht einschließen, ein `@Page` auf der Seite die Richtlinie oder wenn die Richtlinie nicht mit eine bestimmte Einstellung enthalten ist, wird eine Einstellung von geerbt werden die *"Web.config"* Konfigurationsdatei oder die *"Machine.config"* Konfigurationsdatei. Die *"Machine.config"* Datei bietet zusätzliche Konfigurationseinstellungen für alle Anwendungen, die auf einem Computer ausgeführt.

> [!NOTE] 
> 
> Die *"Machine.config"* enthält auch entsprechende Details zu allen möglichen Konfigurationseinstellungen.


### <a name="web-server-controls"></a>Webserver-Steuerelemente

In den meisten ASP.NET Web Forms-Anwendungen fügen Sie Steuerelemente, mit die den Benutzer die Seite, z. B. Schaltflächen, Textfelder, Listen usw. interagieren können. Diese Webserversteuerelemente sind ähnlich wie HTML-Schaltflächen und input-Elemente. Allerdings werden sie auf dem Server, und Sie können Servercode verwenden zum Einstellen derer Eigenschaften verarbeitet. Diese Steuerelemente lösen auch Ereignisse, die Sie in Server-Code behandeln können.

Webserversteuerelemente verwenden eine spezielle Syntax, die ASP.NET erkennt, wenn die Seite ausgeführt wird. Der Tagname für ASP.NET-Serversteuerelemente beginnt mit einem `asp:` Präfix. Dadurch können ASP.NET erkennt und verarbeiten diese Serversteuerelemente. Das Präfix ist möglicherweise anders, wenn das Steuerelement nicht Teil von .NET Framework ist. Zusätzlich zu den `asp:` Präfix auch ASP.NET-Serversteuerelemente enthalten die `runat="server"` Attribut und ein `ID` , Sie verwenden können, um auf das Steuerelement im Servercode zu verweisen.

Wenn die Seite ausgeführt wird, wird von ASP.NET die Serversteuerelemente identifiziert und führt den Code, der mit den Steuerelementen verknüpft ist. Viele Steuerelemente rendern HTML-Code oder anderes Markup auf der Seite, wenn er in einem Browser angezeigt wird.

### <a name="server-code"></a>Servercode

Die meisten ASP.NET Web Forms-Anwendungen enthalten Code, auf dem Server ausgeführt wird, wenn die Seite verarbeitet wird. Wie bereits erwähnt, kann die Server-Code auf einer Vielzahl von Möglichkeiten, z. B. das Hinzufügen von Daten an ein ListView-Steuerelement verwendet werden. ASP.NET unterstützt viele Sprachen auf dem Server, einschließlich c#, Visual Basic, j# usw. ausgeführt.

ASP.NET unterstützt zwei Modelle für das Schreiben von Servercode für eine Webseite an. Der Code für die Seite befindet sich im Modell Einzeldatei-in ein Script-Element, in dem das öffnende Tag enthält, den `runat="server"` Attribut. Alternativ können Sie den Code für die Seite in einer separaten Klasse-Datei erstellen, die als Code-Behind-Modell bezeichnet wird. In diesem Fall enthält die ASP.NET Web Forms-Seite in der Regel keinen Servercode. Stattdessen die `@Page` Richtlinie enthält Informationen, verknüpft die *aspx* Seite mit der zugeordneten Code-Behind-Datei.

Die `CodeBehind` Attribut, das der `@Page` Richtlinie gibt den Namen der separate Klassendatei, und die `Inherits` -Attribut gibt den Namen der Klasse innerhalb der CodeBehind-Datei, die der Seite entspricht.

### <a name="updating-the-master-page"></a>Aktualisieren der Masterseite

In ASP.NET Web Forms ermöglichen Masterseiten Ihnen, ein konsistentes Layout für die Seiten in Ihrer Anwendung zu erstellen. Eine einzige masterdseite definiert das Aussehen und Verhalten und die gemäß dem Standardverhalten, das Sie für alle Seiten (oder eine Gruppe von Seiten) verwenden möchten, in Ihrer Anwendung. Sie können dann einzelne Seiten erstellen, die den Inhalt, die, den Sie anzeigen möchten enthalten, wie oben beschrieben. Wenn Benutzer Inhaltsseiten anfordern, führt ASP.NET diese mit der Masterseite Ausgabe erzeugt, die das Layout der Masterseite mit dem Inhalt der Inhaltsseite kombiniert.

Die neue Website benötigt ein einzelnes Logo, die auf jeder Seite angezeigt. Wenn dieses Logo hinzufügen möchten, können Sie den HTML-Code auf der Masterseite ändern.

1. In **Projektmappen-Explorer**suchen oder öffnen Sie die **Site.Master** Seite.
2. Wenn die Seite im **Entwurf** anzuzeigen, wechseln Sie zur **Quelle** anzeigen.
3. Aktualisieren Sie die Masterseite durch **ändern oder Hinzufügen von** das Markup in gelb hervorgehoben: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Dieser HTML-Code zeigt das Bild, das mit dem Namen *logo.jpg* aus der *Images* Ordner der Webanwendung, die Sie später hinzufügen. Wenn eine Seite, die die Masterseite in einem Browser angezeigt wird, wird das Logo angezeigt. Wenn ein Benutzer auf das Logo klickt, wird der Benutzer zurück zum Navigieren der *"default.aspx"* Seite. Das HTML-Anchor-Tag `<a>` dient als Wrapper für das Image-Steuerelement und das Bild, das als Teil des Links eingefügt werden können. Die `href` -Attribut für das Anchor-Tag gibt an, das Stammverzeichnis "`~/`" der Website als Speicherort für Verknüpfung. In der Standardeinstellung die *"default.aspx"* Seite wird angezeigt, wenn der Benutzer, in das Stammverzeichnis der Website navigiert. Die **Image** `<asp:Image>` enthält außerdem Eigenschaften, z. B. `BorderStyle`, Rendern im HTML-Format, wenn in einem Browser angezeigt.

### <a name="master-pages"></a>Masterseiten

Eine Masterseite ist eine ASP.NET-Datei mit der Erweiterung .master (z. B. *Site.Master*) mit einem vordefinierten Layout, die statischen Text, HTML-Elemente und Steuerelemente enthalten kann. Die Masterseite wird durch eine spezielle identifiziert `@Master` Richtlinie, die ersetzt die `@Page` Richtlinie für die normale verwendeten *aspx* Seiten.

Zusätzlich zu den `@Master` -Direktive die Masterseite enthält auch alle Elemente der obersten Ebene HTML für eine Seite, wie z. B. `html`, `head`, und `form`. Auf der Masterseite, die Sie weiter oben hinzugefügt haben, verwenden Sie z. B. eine HTML `table` für das Layout für ein `img` -Element für das Logo des Unternehmens, statischen Text und Serversteuerelemente, gemeinsame Mitgliedschaft für Ihre Website zu behandeln. Sie können die HTML-Elemente und alle ASP.NET-Elemente als Teil Ihrer Masterseite verwenden.

Zusätzlich zum statischen Text und Steuerelemente, die auf allen Seiten angezeigt werden, enthält die Masterseite auch eine oder mehrere **ContentPlaceHolder** Steuerelemente. Diese Platzhaltersteuerelemente definiert werden können, wo die ersetzbaren Inhalt angezeigt wird. Wiederum der Inhalt austauschbare ist definiert auf Inhaltsseiten, z. B. *"default.aspx"* unter Verwendung der **Inhalt** -Steuerelement.

#### <a name="adding-image-files"></a>Hinzufügen von Bilddateien

Das Logobild, das oben, sowie alle der Produktbilder verwiesen wird muss an die Webanwendung hinzugefügt werden, damit sie angezeigt werden können, wenn das Projekt in einem Browser angezeigt wird.

#### <a name="download-from-msdn-samples-site"></a>Von MSDN-Beispiele-Website herunterladen:

[Erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013 – Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (c#)

Der Download enthält die Ressourcen in der *WingtipToys-Assets* Ordner, die verwendet werden, um die beispielanwendung zu erstellen.

1. Wenn Sie nicht bereits geschehen, laden Sie die komprimierte Beispieldateien, die mit den oben aufgeführten Link auf der MSDN Samples-Website herunter.
2. Nach dem Herunterladen, öffnen Sie die ZIP-Datei, und kopieren Sie den Inhalt in einen lokalen Ordner auf Ihrem Computer.
3. Suchen und öffnen Sie die *WingtipToys-Assets* Ordner.
4. Durch Ziehen und ablegen, kopieren Sie die *Katalog* Ordner aus Ihrem lokalen Ordner, in das Stammverzeichnis des das Webanwendungsprojekt in der **Projektmappen-Explorer** von Visual Studio. 

    ![Benutzeroberfläche und Navigation - Dateien kopieren](ui_and_navigation/_static/image1.png)
5. Als Nächstes erstellen Sie einen neuen Ordner namens *Images* mit der rechten Maustaste die **WingtipToys** Projekt **Projektmappen-Explorer** und **hinzufügen**  - &gt; **Neuer Ordner**.
6. Kopieren der *logo.jpg* -Datei aus der *WingtipToys-Objekte* Ordner **Datei-Explorer** auf der *Images* Ordner der Webanwendung Projekt **Projektmappen-Explorer** von Visual Studio.
7. Klicken Sie auf die **alle Dateien anzeigen** am oberen Rand die Option **Projektmappen-Explorer** die Liste der Dateien aktualisiert, wenn die neuen Dateien nicht angezeigt wird.  
  
    **Projektmappen-Explorer** zeigt jetzt die aktualisierte Projektdateien. 

    ![Benutzeroberfläche und Navigation - Projektmappen-Explorer](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Hinzufügen von Seiten

Vor dem Hinzufügen von Navigation an die Webanwendung aus, fügen Sie zunächst zwei neue Seiten, zu der Sie navigieren müssen. Weiter unten in dieser Reihe von Tutorials zeigen Sie auf diese neuen Seiten Produkte "und" Produktdetails.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste **WingtipToys**, klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.   
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie dann **Webformular mit Gestaltungsvorlage** aus der Mitte aus, und nennen Sie sie *"ProductList.aspx"*. 

    ![Fügen Sie Benutzeroberfläche und Navigation - Dialogfeld "Neues Element" hinzu.](ui_and_navigation/_static/image3.png)
3. Wählen Sie **Site.Master** anzufügende Masterseite auf das neu erstellte *aspx* Seite. 

    ![Wählen Benutzeroberfläche und Navigation - Masterseite](ui_and_navigation/_static/image4.png)
4. Fügen Sie eine zusätzliche Seite, die mit dem Namen *ProductDetails.aspx* anhand dieser Schritte.

### <a name="updating-bootstrap"></a>Aktualisieren von Bootstrap

Verwenden Sie die Projektvorlagen für Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), ein Layout und Design-Framework erstellt die Lösung durch Twitter. Bootstrap verwendet CSS3 reaktionsfähiges Design bereitstellen, was bedeutet, dass Layouts an anderen Browser Fenstergrößen dynamisch anpassen können. Sie können auch die Bootstrap Design-Funktion verwenden, auf einfache Weise eine Änderung in der Anwendung aussehen und Verhalten wirksam. Standardmäßig enthält die ASP.NET Web Application-Vorlage in Visual Studio 2013 Bootstrap als NuGet-Paket.

In diesem Tutorial werden Sie Aussehen und Verhalten der Anwendung Wingtip Toys ändern, indem Sie die Bootstrap-CSS-Dateien ersetzen.

1. In **Projektmappen-Explorer**öffnen die *Content* Ordner.
2. Mit der rechten Maustaste die *Datei "Bootstrap.CSS"* Datei, und benennen Sie sie in *Bootstrap-original.css*.
3. Benennen Sie die *bootstrap.min.css* zu *Bootstrap-original.min.css*.
4. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Content* Ordner, und wählen **Ordner in Datei-Explorer öffnen**.  
   Datei-Explorer wird angezeigt. Sie speichert heruntergeladene bootstrap CSS-Dateien an diesen Speicherort.
5. Wechseln Sie in Ihrem Browser zu [ https://bootswatch.com/3/ ](https://bootswatch.com/3/).
6. Scrollen Sie im Browserfenster, bis Sie sehen, dass das Schwachstelle Design. 

    ![Benutzeroberfläche und Navigation - Schwachstelle Design](ui_and_navigation/_static/image5.png)
7. Laden der *Datei "Bootstrap.CSS"* Datei und die *bootstrap.min.css* -Datei in die *Content* Ordner. Verwenden Sie den Pfad zu dem Inhaltsordner ein, die in angezeigt wird der **Datei-Explorer** Fenster, das Sie zuvor geöffnet.
8. In **Visual Studio** am oberen Rand **Projektmappen-Explorer**, wählen die **alle Dateien anzeigen** Option, um die neuen Dateien im Ordner "Content" anzuzeigen. 

    ![Benutzeroberfläche und Navigation - Projektmappen-Explorer](ui_and_navigation/_static/image6.png)

   Sie sehen die beiden neuen CSS-Dateien in die **Content** Ordner, aber beachten Sie, die das Symbol neben jedem Dateinamen ausgegraut ist. Dies bedeutet, dass die Datei noch nicht zum Projekt hinzugefügt wurde.
9. Mit der rechten Maustaste die *Datei "Bootstrap.CSS"* und *bootstrap.min.css* Dateien, und wählen **zu Projekt**.   
   Wenn Sie die Anwendung Wingtip Toys weiter unten in diesem Tutorial ausführen, wird die neue Benutzeroberfläche angezeigt.

> [!NOTE] 
> 
> Die ASP.NET Web Application-Vorlage verwendet die *Bundle.config* Datei im Stammverzeichnis des Projekts, das den Pfad der Bootstrap-CSS-Dateien zu speichern.


### <a name="modifying-the-default-navigation"></a>Ändern die Standardnavigation

Die Standardnavigation für jede Seite in der Anwendung kann geändert werden, durch Ändern der unsortierten Liste Navigationselement, die in der *Site.Master* Seite.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *Site.Master* Seite.
2. Fügen Sie den zusätzlichen Navigationslink in Gelb zu der unsortierten Liste unten hervorgehoben:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Wie Sie in den oben genannten HTML-Code sehen können, Sie jedes Zeilenelement geändert `<li>` ein Anchor-Tag mit `<a>` mit einem Link `href` Attribut. Jede `href` verweist auf eine Seite in der Webanwendung. Im Browser, wenn ein Benutzer auf einen dieser Links klickt (z. B. **Produkte**), sie werden zur Seite navigieren, innerhalb der `href` (z. B. **"ProductList.aspx"**). Sie werden die Anwendung am Ende dieses Tutorials ausführen.

> [!NOTE] 
> 
> Die Tilde (`~`) Zeichen wird verwendet, um anzugeben, dass die `href` Pfad beginnt im Stammverzeichnis des Projekts.


### <a name="adding-a-data-control-to-display-navigation-data"></a>Ein Steuerelement zum Anzeigen von Navigationsdaten hinzufügen

Als Nächstes fügen Sie ein Steuerelement zum Anzeigen aller Kategorien aus der Datenbank. Jede Kategorie fungiert als Link zu der *"ProductList.aspx"* Seite. Klickt ein Benutzer auf einen Kategorielink in den Browser, werden sie navigieren Sie zu der Seite "Produkte" und sehen nur die Produkte, die der ausgewählten Kategorie zugeordnet.

Verwenden Sie eine **ListView** Steuerelement, um alle in der Datenbank enthaltenen Kategorien anzuzeigen. Hinzufügen einer **ListView** Steuerelement auf die Masterseite:

1. In der *Site.Master* Seite, fügen Sie die folgende hervorgehobene `<div>` Element **nach** der `<div>` mit der `id="TitleContent"` , die Sie zuvor hinzugefügt haben:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Dieser Code zeigt alle Kategorien aus der Datenbank. Die **ListView** Steuerelement jeder Kategoriename als Linktext angezeigt und enthält einen Link zu der *"ProductList.aspx"* Seite einen Abfragezeichenfolgen-Wert enthält die `ID` der Kategorie. Durch Festlegen der `ItemType` -Eigenschaft in der **ListView** zu steuern, die XPath-Datenbindungsausdruck `Item` finden Sie in der `ItemTemplate` Knoten und das Steuerelement wird stark typisiert. Können Sie die Details der Auswählen der `Item` Objekt, um mithilfe von IntelliSense, z. B. Angeben der `CategoryName`. Dieser Code befindet sich innerhalb des Containers `<%#: %>` , die einen XPath-Datenbindungsausdruck markiert. Durch Hinzufügen der (:) bis zum Ende der `<%#` Präfix, das Ergebnis der XPath-Datenbindungsausdruck ist HTML-codiert. Wenn das Ergebnis HTML-codiert ist, ist Ihre Anwendung besser vor Cross-Site-geschützt script Injection (XSS) und HTML-Injection-Angriffe.

> [!NOTE] 
> 
> **Tipp**
> 
> Wenn Sie Code hinzufügen, indem Sie eingeben, während der Entwicklung, können Sie sicher sein, dass ein gültiger Member eines Objekts gefunden wird, dass Data-Steuerelemente, die verfügbaren Mitglieder basierend auf IntelliSense angezeigt, da stark typisiert. IntelliSense bietet Kontext entsprechenden Auswahlmöglichkeiten, während der Eingabe von Code, z. B. Eigenschaften, Methoden und Objekte.


Im nächsten Schritt implementieren Sie die `GetCategories` Methode zum Abrufen von Daten.

### <a name="linking-the-data-control-to-the-database"></a>Verknüpfen das Steuerelement mit der Datenbank

Bevor Sie Daten im Steuerelement anzeigen können, müssen Sie das Steuerelement mit der Datenbank zu verknüpfen. Um den Link zu machen, können Sie den Code hinter der Ändern der *Site.Master.cs* Datei.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Site.Master* Seite, und klicken Sie dann auf **Ansichtscode**. Die *Site.Master.cs* Datei wird im Editor geöffnet.
2. Am Anfang des der *Site.Master.cs* Datei, fügen Sie zwei zusätzliche Namespaces hinzu, sodass alle enthaltenen Namespaces wie folgt aussehen:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Fügen Sie die hervorgehobene `GetCategories` Methode nach der `Page_Load` Ereignishandler wie folgt:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

Der obige Code wird ausgeführt, wenn jede Seite, die die Masterseite im Browser geladen wird. Die `ListView` Control (benannte "CategoryList"), die Sie zuvor in diesem Lernprogramm hinzugefügt verwendet die modellbindung, Daten auszuwählen. Im Markup der `ListView` Steuerelement des Steuerelements festlegen `SelectMethod` Eigenschaft, um die `GetCategories` oben gezeigten Methode. Die `ListView` kontrollaufrufe der `GetCategories` Methode zum geeigneten Zeitpunkt im Lebenszyklus Seite blättern und bindet automatisch die zurückgegebenen Daten. Erfahren Sie mehr über das Binden von Daten im nächsten Tutorial.

### <a name="running-the-application-and-creating-the-database"></a>Ausführen der Anwendung und zum Erstellen der Datenbank

Weiter oben in diesem Tutorial können Sie auch eine Initialisiererklasse (mit dem Namen "ProductDatabaseInitializer") erstellt und dieser Klasse angegeben die *"Global.asax.cs"* Datei. Das Entity Framework generiert die Datenbank, wenn die Anwendung beim ersten da ausgeführt wird die `Application_Start` -Methode aus der *"Global.asax.cs"* Datei wird die Initialisiererklasse aufrufen. Die Initialisiererklasse verwendet die Modellklassen (`Category` und `Product`), die Sie zuvor in dieser tutorialreihe zum Erstellen der Datenbank hinzugefügt.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *"default.aspx"* Seite und wählen Sie **als Startseite festlegen**.
2. In Visual Studio drücken Sie **F5**.   
 Es dauert einen Moment, alles, was während dieser ersten Ausführung einrichten.   
    ![Benutzeroberfläche und Navigation - Browser-Windows](ui_and_navigation/_static/image7.png)  
 Wenn Sie die Anwendung ausführen, wird die Anwendung kompiliert werden und die Datenbank mit dem Namen *wingtiptoys.mdf* erstellt werden, der *App\_Daten* Ordner. Im Browser sehen Sie ein Navigationsmenü Kategorie. Dieses Menü wurde generiert, indem Sie die Kategorien aus der Datenbank abrufen. Im nächsten Tutorial implementieren Sie die Navigation.
3. Schließen Sie den Browser, um die ausgeführte Anwendung zu beenden.

### <a name="reviewing-the-database"></a>Überprüfen die Datenbank

Öffnen der *"Web.config"* Datei, und sehen Sie sich den Abschnitt mit der Verbindungszeichenfolge. Sie sehen, dass die `AttachDbFilename` Wert in der Verbindungszeichenfolge verweist auf die `DataDirectory` für das Webprojekt für die Anwendung. Der Wert `|DataDirectory|` ist ein reservierter Wert darstellt, die die *App\_Daten* Ordner im Projekt. Dieser Ordner ist, wo sich die Datenbank, die aus Ihrer Entitätsklassen erstellt wurde, befindet.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Wenn die *App\_Daten* Ordner ist nicht sichtbar, oder wenn der Ordner leer ist, wählen Sie die **aktualisieren** Symbol und klicken Sie dann die **alle Dateien anzeigen** Symbol am oberen Rand der **Projektmappen-Explorer** Fenster. Erweitern die Breite der **Projektmappen-Explorer** Windows möglicherweise erforderlich, um alle verfügbaren Symbole anzuzeigen.


Nachdem Sie die in enthaltenen Daten überprüfen können die *wingtiptoys.mdf* Datenbankdatei mit der **Server-Explorer** Fenster.

1. Erweitern Sie die *App\_Daten* Ordner. Wenn die *App\_Daten* Ordner nicht angezeigt wird, finden Sie im Hinweis oben.
2. Wenn die *wingtiptoys.mdf* Datenbankdatei ist nicht sichtbar ist, wählen die **aktualisieren** Symbol und klicken Sie dann die **alle Dateien anzeigen** Symbol am oberen Rand der **Projektmappen-Explorer**  Fenster.
3. Mit der rechten Maustaste die *wingtiptoys.mdf* Datenbankdatei, und wählen **öffnen**.  
    **Server-Explorer** wird angezeigt. 

    ![Benutzeroberfläche und Navigation - Server-Explorer](ui_and_navigation/_static/image8.png)
4. Erweitern Sie die *Tabellen* Ordner.
5. Mit der rechten Maustaste die **Produkte**Tabelle, und wählen Sie **Tabellendaten anzeigen**.  
 Die **Produkte** Tabelle wird angezeigt. 

    ![Benutzeroberfläche und Navigation - Products-Tabelle](ui_and_navigation/_static/image9.png)
6. In dieser Ansicht können Sie anzeigen und Bearbeiten von Daten in die **Produkte** Tabelle manuell.
7. Schließen der **Produkte** Tabellenfenster.
8. In der **Server-Explorer**, mit der rechten Maustaste die **Produkte** Tabelle erneut aus, und wählen Sie **Tabellendefinition öffnen**.  
 Entwerfen Sie die Daten für die **Produkte** Tabelle wird angezeigt. 

    ![Benutzeroberfläche und Navigation - Produkte-Entwurf](ui_and_navigation/_static/image10.png)
9. In der **T-SQL** Registerkarte sehen Sie die SQL-DDL-Anweisung, die zum Erstellen der Tabelle verwendet wurde. Sie können auch die Benutzeroberfläche in der **Entwurf** Registerkarte zum Ändern des Schemas.
10. In der **Server-Explorer**, mit der rechten Maustaste **WingtipToys** Datenbank, und wählen Sie **enge Verbindung**.   
 Durch das Trennen der Datenbank in Visual Studio, wird das Datenbankschema weiter unten in dieser tutorialreihe geändert werden können.
11. Wechseln Sie zurück zur **Projektmappen-Explorer**durch Auswählen der **Projektmappen-Explorer** Registerkarte am unteren Rand der **Server-Explorer** Fenster.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial der Reihe haben Sie einige einfache Benutzeroberfläche, Grafiken, Seiten und Navigation hinzugefügt. Darüber hinaus haben Sie die Webanwendung, die die Datenbank aus den Datenklassen erstellt, die Sie im vorherigen Tutorial hinzugefügt ausgeführt. Sie auch angezeigt, den Inhalt der *Produkte* Tabelle der Datenbank, indem Sie die Datenbank direkt anzeigen. Im nächsten Tutorial werden Sie von Datenelementen und Details aus der Datenbank anzeigen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Einführung in die Programmierung von ASP.NET-Webseiten](https://msdn.microsoft.com/library/ms178125.aspx)   
[Übersicht über die für ASP.NET-Webserversteuerelemente](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[CSS-Tutorial](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Zurück](create_the_data_access_layer.md)
> [Weiter](display_data_items_and_details.md)
