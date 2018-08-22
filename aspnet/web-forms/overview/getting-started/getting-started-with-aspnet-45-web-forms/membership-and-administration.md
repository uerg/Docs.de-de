---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Mitgliedschaft und Verwaltung | Microsoft-Dokumentation
author: Erikre
description: Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 23d08d5a05a8321fbc794e2c9b54cc39c9b5baf6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826260"
---
<a name="membership-and-administration"></a>Mitgliedschaft und Verwaltung
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die dieser tutorialreihe begleitet.


In diesem Tutorial erfahren Sie, wie die Wingtip Toys-beispielanwendung zum Hinzufügen einer benutzerdefinierten Rolle, und Verwenden von ASP.NET Identity aktualisiert. Es zeigt auch, wie eine Verwaltungsseite implementiert, von der der Benutzer mit einer benutzerdefinierten Rolle hinzufügen und Produkte aus der Website entfernen kann.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) ist das Mitgliedschaftssystem, das zum Erstellen von ASP.NET Web-Anwendung verwendet und ist in ASP.NET 4.5 verfügbar. ASP.NET Identity wird verwendet, in der Projektvorlage für Visual Studio 2013-Web Forms als auch die Vorlagen für [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web-API](../../../../web-api/index.md), und [einseitigen ASP.NET-Anwendung](../../../../single-page-application/index.md). Sie können auch ausdrücklich Installieren der ASP.NET Identity-System mithilfe von NuGet, wenn Sie eine leere Webanwendung starten. In diesem Tutorial verwenden Sie jedoch die **Web Forms**"ProjectTemplate", die ASP.NET Identity-System enthält. ASP.NET Identity vereinfacht benutzerspezifische Profildaten in Anwendungsdaten zu integrieren. Darüber hinaus können mit ASP.NET Identity das Persistenzmodell für Benutzerprofile in Ihrer Anwendung auswählen. Sie können die Daten in einer SQL Server-Datenbank oder einem anderen Datenspeicher speichern einschließlich *NoSQL* Datenspeicher wie z. B. Windows Azure Storage-Tabellen.

Dieses Tutorial baut auf dem vorherigen Lernprogramm, das mit dem Titel "Auschecken und Zahlung mit PayPal" in der Wingtip Toys-Tutorial-Reihe.

## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Gewusst wie Code verwenden, um eine benutzerdefinierte Rolle und ein Benutzer der Anwendung hinzufügen.
- Informationen zum Zugriff auf der Seite "und" Ordner "Verwaltung" zu beschränken.
- Informationen zum Navigationsbereich für den Benutzer bereitstellen, die die benutzerdefinierte Rolle gehört.
- Gewusst wie: modellbindung zu verwenden, zum Auffüllen einer [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) -Steuerelement mit Produktkategorien.
- Gewusst wie: Hochladen einer Datei in die Web-Anwendung mithilfe der ["FileUpload"](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) Steuerelement.
- So verwenden Sie Steuerelemente zur gültigkeitsprüfung zum Implementieren der eingabeüberprüfung.
- Informationen zum Hinzufügen und Entfernen von Produkten aus der Anwendung.

## <a name="these-features-are-included-in-the-tutorial"></a>Diese Funktionen sind in diesem Tutorial enthalten:

- ASP.NET Identity
- Konfiguration und -Autorisierung
- Modellbindung
- Unaufdringliche Validierung

ASP.NET Web Forms bietet Mitgliedschaftsfunktionen. Mithilfe der Standardvorlage zu verwenden, müssen Sie die integrierte Mitgliedschaftsfunktion, die Sie sofort verwenden können, wenn die Anwendung ausgeführt wird. In diesem Tutorial erfahren Sie, wie Sie mit ASP.NET Identity eine benutzerdefinierte Rolle hinzufügen und Zuweisen eines Benutzers zu dieser Rolle. Erfahren Sie, wie den Zugriff auf den administrationsordner beschränken. Sie fügen eine Seite auf den administrationsordner, mit dem Benutzer mit einer benutzerdefinierten Rolle hinzufügen und Entfernen von Produkten und ein Produkt Vorschau anzeigen, nachdem er hinzugefügt wurde.

## <a name="adding-a-custom-role"></a>Hinzufügen einer benutzerdefinierten Rolle

Mithilfe von ASP.NET Identity, können Sie eine benutzerdefinierte Rolle hinzufügen und Zuweisen eines Benutzers zu dieser Rolle mithilfe von Code.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste auf die *Logik* Ordner, und erstellen Sie eine neue Klasse.
2. Nennen Sie die neue Klasse *RoleActions.cs*.
3. Ändern Sie den Code, sodass sie wie folgt angezeigt:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. In **Projektmappen-Explorer**öffnen die *"Global.asax.cs"* Datei.
5. Ändern der *"Global.asax.cs"* Datei durch Hinzufügen des Codes in gelb hervorgehoben werden, sodass sie wie folgt angezeigt:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Beachten Sie, dass `AddUserAndRole` rot unterstrichen ist. Doppelklicken Sie auf der AddUserAndRole-Code.  
   Die Buchstaben "A" am Anfang der markierten Methode wird unterstrichen.
7. Zeigen Sie auf den Buchstaben "A", und klicken Sie auf die Benutzeroberfläche, die Ihnen ermöglicht, einen Methodenstub für die `AddUserAndRole` Methode. 

    ![Mitgliedschaft und Advministration - Methodenstub generieren](membership-and-administration/_static/image1.png)
8. Klicken Sie auf die Option:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Öffnen der *RoleActions.cs* -Datei aus dem *Logik* Ordner.  
   Die `AddUserAndRole` Methode wurde in der Klassendatei hinzugefügt.
10. Ändern der *RoleActions.cs* Datei durch das Entfernen der `NotImplementedeException` und Hinzufügen von Code in gelb hervorgehoben, damit es wie folgt aussieht:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Der obige Code wird zuerst einen Datenbankkontext für die Mitgliedschaftsdatenbank hergestellt. Die Mitgliedschaftsdatenbank befindet sich auch als ein *mdf* Datei die *App\_Daten* Ordner. Sie werden diese Datenbank anzeigen, nachdem der erste Benutzer dieser Webanwendung angemeldet hat. 

> [!NOTE] 
> 
> Wenn Sie die Daten der Benutzergruppenmitgliedschaft zusammen mit den Produktdaten speichern möchten, können Sie erwägen, mit dem gleichen **"DbContext"** , die Sie zum Speichern der Produktdaten im obigen Code verwendet.


 Die *interne* Schlüsselwort ist ein Zugriffsmodifizierer für Typen (z. B. Klassen) und Typmember (z. B. Methoden oder Eigenschaften). Interne Typen oder Member sind nur innerhalb der Dateien, die in der gleichen Assembly enthaltenen *(.dll* Datei). Beim Erstellen Ihrer Anwendung eine Assemblydatei *(.dll*) erstellt wird, enthält den Code, der beim Ausführen der Anwendung ausgeführt wird. 

Ein `RoleStore` -Objekt, das Rollenverwaltung bereitstellt, wird basierend auf den Datenbankkontext erstellt.

> [!NOTE] 
> 
> Beachten Sie, dass bei der `RoleStore` Objekt wird erstellt. er verwendet einen generischen `IdentityRole` Typ. Dies bedeutet, dass die `RoleStore` darf nur enthalten `IdentityRole` Objekte. Außerdem werden mit Generika können Ressourcen im Speicher besser behandelt.


Als Nächstes die `RoleManager` Objekt, wird basierend auf erstellt die `RoleStore` -Objekt, das Sie gerade erstellt haben. die `RoleManager` Objekt stellt eine rollenbezogene API, die verwendet werden kann, um automatisch Speichern von Änderungen an der `RoleStore`. Die `RoleManager` darf nur enthalten `IdentityRole` Objekte, da der Code verwendet die `<IdentityRole>` generischen Typ.

Rufen Sie die `RoleExists` Methode, um zu bestimmen, ob die Rolle "CanEdit" in der Mitgliedschaftsdatenbank vorhanden ist. Wenn sie nicht der Fall ist, erstellen Sie die Rolle aus.

Erstellen der `UserManager` Objekt ist anscheinend viel komplizierter ist als die `RoleManager` zu steuern, ist es jedoch nahezu identisch. Es wird nur auf eine Zeile nicht über mehrere codiert. Hier ist der Parameter, den Sie übergeben, wie ein neues Objekt in den Klammern enthaltenen instanziieren.

Als Nächstes erstellen Sie den Benutzer "CanEditUser" durch Erstellen eines neuen `ApplicationUser` Objekt. Dann, wenn Sie den Benutzer erfolgreich erstellt haben, fügen Sie den Benutzer der neuen Rolle.

> [!NOTE] 
> 
> Die Fehlerbehandlung wird während des Lernprogramms "ASP.NET Fehlerbehandlung" weiter unten in dieser tutorialreihe aktualisiert werden.


Das nächste Mal die Anwendung gestartet wird, wird der Benutzer, die mit dem Namen "CanEditUser" wie die Rolle, die mit dem Namen "CanEdit" der Anwendung hinzugefügt werden. Weiter unten in diesem Tutorial werden Sie melden Sie sich als der Benutzer "CanEditUser", um zusätzliche Funktionen anzuzeigen, die Sie in diesem Tutorial hinzugefügt werden. Weitere Informationen zu ASP.NET Identity API finden Sie unter den [Microsoft.AspNet.Identity-Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Weitere Informationen zum Initialisieren der ASP.NET Identity-System finden Sie unter den [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Einschränken des Zugriffs auf der Seite "Verwaltung"

Die Wingtip Toys-beispielanwendung kann anonyme Benutzer und angemeldete Benutzer zum Anzeigen und den Kauf von Produkten. Der angemeldete Benutzer, der die benutzerdefinierte "CanEdit" Funktion kann jedoch eine zugriffsbeschränkte Seite zum Hinzufügen und entfernen die Produkte zugreifen.

#### <a name="add-an-administration-folder-and-page"></a>Fügen Sie einen Ordner "Verwaltung" und Seite hinzu

Als Nächstes erstellen Sie einen Ordner namens *Admin* für den "CanEditUser"-Benutzer, die die benutzerdefinierte Rolle von das Wingtip Toys-beispielanwendung.

1. Mit der rechten Maustaste in des Namens des Projekts (**Wingtip Toys**) in **Projektmappen-Explorer** , und wählen Sie **hinzufügen**  - &gt; **neuer Ordner**.
2. Nennen Sie diesen Ordner *Admin*.
3. Mit der rechten Maustaste die *Admin* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
4. Wählen Sie die <strong>Visual C#-</strong> - &gt; <strong>Web</strong> Gruppe "Vorlagen" auf der linken Seite. Wählen Sie in der mittleren Liste <strong>Webformular mit Gestaltungsvorlage</strong>, nennen Sie sie <em>AdminPage.aspx</em><strong>,</strong> und wählen Sie dann <strong>hinzufügen</strong>.
5. Wählen Sie die *Site.Master* als Masterseite Datei, und wählen Sie dann **OK**.

#### <a name="add-a-webconfig-file"></a>Fügen Sie eine Datei "Web.config"

Durch das Hinzufügen einer *"Web.config"* -Datei in die *Admin* Ordner können Sie den Zugriff auf die Seite, die in den Ordner enthaltenen.

1. Mit der rechten Maustaste die *Admin* Ordner, und wählen **hinzufügen**  - &gt; **neues Element**.  
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie aus der Liste der Visual c#-internetvorlagen, <strong>Webkonfigurationsdatei</strong>aus der mittleren Liste aus, akzeptieren Sie den Namen der <em>"Web.config"</em><strong>,</strong> und wählen Sie dann auf <strong>Hinzufügen</strong>.
3. Ersetzen Sie den vorhandenen XML-Inhalt in die *"Web.config"* Datei durch Folgendes:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Speichern Sie die *"Web.config"* Datei. Die *"Web.config"* Datei gibt an, dass nur Benutzerdaten, die zu der Rolle "CanEdit" der Anwendung gehören stehen auf die Seite innerhalb der *Admin* Ordner.

### <a name="including-custom-role-navigation"></a>Einschließlich Navigation der benutzerdefinierten Rolle

Um den Benutzer der Rolle "benutzerdefinierten"CanEdit"", navigieren Sie zum Abschnitt "Verwaltung" der Anwendung zu aktivieren, müssen Sie einen Link zum Hinzufügen der *Site.Master* Seite. Nur Benutzer, die der Rolle "CanEdit" angehören, können, finden Sie unter den **Admin** verknüpfen und den Verwaltungsabschnitt zugreifen.

1. Suchen Sie im Projektmappen-Explorer, und öffnen Sie die *Site.Master* Seite.
2. Um einen Link für den Benutzer der Rolle "CanEdit" zu erstellen, fügen Sie das Markup in Gelb, in der folgenden ungeordnete Liste hervorgehoben `<ul>` Element, damit die Liste wird angezeigt, wie folgt:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Öffnen der *Site.Master.cs* Datei. Stellen die **Admin** Link nur für den Benutzer "CanEditUser" durch Hinzufügen des Codes in Gelb zu hervorgehoben angezeigt. die `Page_Load` Handler. Die `Page_Load` Ereignishandler wie folgt angezeigt:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Wenn die Seite geladen wird, überprüft der Code an, ob der angemeldete Benutzer die Rolle "CanEdit". Wenn der Benutzer gehört, zu der Rolle "CanEdit", das Span-Element mit dem Link zu der *AdminPage.aspx* Seite (und daher den Link in der Spanne) sichtbar gemacht werden.

### <a name="enabling-product-administration"></a>Aktivieren die Produkt-Verwaltung

Bisher haben eine "CanEditUser" Benutzer, einen Ordner "Verwaltung" und eine Seite "Verwaltung" Sie die Rolle "CanEdit" erstellt und hinzugefügt. Sie Zugriffsrechte für den Ordner "Verwaltung" und die Seite festgelegt haben und die Anwendung einen Navigationslink für den Benutzer der Rolle "CanEdit" hinzugefügt haben. Als Nächstes fügen Sie Markup für die *AdminPage.aspx* Seite und code in die *AdminPage.aspx.cs* Code-Behind-Datei, mit denen den Benutzer mit der Rolle "CanEdit" hinzugefügt und entfernt Produkte.

1. In **Projektmappen-Explorer**öffnen die *AdminPage.aspx* -Datei aus dem *Admin* Ordner.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Öffnen Sie als Nächstes die *AdminPage.aspx.cs* Code-Behind-Datei mit der rechten Maustaste die *AdminPage.aspx* und auf **Anzeigecode**.
4. Ersetzen Sie den vorhandenen Code in die *AdminPage.aspx.cs* Code-Behind-Datei mit dem folgenden Code:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

In den Code, den Sie eingegeben haben die *AdminPage.aspx.cs* Code-Behind-Datei, eine Klasse namens `AddProducts` führt die eigentliche Arbeit der Produkte in der Datenbank hinzufügen. Diese Klasse ist nicht noch vorhanden, sodass Sie jetzt erstellt werden.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Logik* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Code** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie dann **Klasse**aus der Mitte aus, und nennen Sie sie *AddProducts.cs*.   
   Die neue Klassendatei wird angezeigt.
3. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

Die *AdminPage.aspx* ermöglicht es dem Benutzer, der zu der Rolle "CanEdit" gehört zum Hinzufügen und Entfernen von Produkten. Wenn ein neues Produkt hinzugefügt wird, werden die Details über das Produkt überprüft, und klicken Sie dann in die Datenbank eingegeben. Das neue Produkt ist sofort verfügbar ist, für alle Benutzer der Webanwendung.

#### <a name="unobtrusive-validation"></a>Unaufdringliche Validierung

Die Produktdetails, die der Benutzer auf die *AdminPage.aspx* Seite werden mithilfe von Steuerelementen zur gültigkeitsprüfung überprüft (`RequiredFieldValidator` und `RegularExpressionValidator`). Diese Steuerelemente verwenden automatisch die unaufdringliche Validierung. Unaufdringliche Validierung kann die Validierungssteuerelemente, die mit JavaScript für die clientseitige Validierungslogik, was bedeutet, dass die Seite eine Reise mit dem Server überprüft werden keine erforderlich ist. Standardmäßig befindet sich auf unaufdringliche Validierung der *"Web.config"* Datei entsprechend der folgenden Konfiguration:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Reguläre Ausdrücke

Der Produktpreis auf die *AdminPage.aspx* Seite überprüft mithilfe einer **RegularExpressionValidator** Steuerelement. Dieses Steuerelement wird überprüft, ob der Wert des zugeordneten Eingabesteuerelements (das Textfeld "AddProductPrice") das mit dem regulären Ausdruck angegebene Muster übereinstimmt. Ein regulärer Ausdruck ist eine Notation Mustervergleich, mit der Sie schnell zu finden und zu bestimmten Zeichenmustern übereinstimmen. Die **RegularExpressionValidator** -Steuerelement enthält eine Eigenschaft namens `ValidationExpression` , enthält den regulären Ausdruck, der zum Überprüfen der Preis-Eingabe verwendet wird, wie unten dargestellt:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>"FileUpload"-Steuerelement

Zusätzlich zu den Steuerelementen Eingabe und Überprüfung Hinzufügen der **"FileUpload"** die Steuerung an die *AdminPage.aspx* Seite. Dieses Steuerelement stellt die Funktion zum Hochladen von Dateien. In diesem Fall lassen Sie nur Bilddateien hochgeladen werden. In der CodeBehind-Datei (*AdminPage.aspx.cs*), wenn die `AddProductButton` geklickt wird, wird der Code wird überprüft die `HasFile` Eigenschaft der **"FileUpload"** Steuerelement. Wenn das Steuerelement eine Datei hat, und wenn der Dateityp (auf der Grundlage der Dateierweiterung) zulässig ist, wird das Bild, um gespeichert die *Images* Ordner und die *Bilder/Daumen* Ordner der Anwendung.

#### <a name="model-binding"></a>Modellbindung

Weiter oben in diesem Tutorial verwendet Sie modellbindung zum Auffüllen einer **ListView** -Steuerelement, ein **FormsView** -Steuerelement, ein **GridView** -Steuerelement, und ein  **DetailView** Steuerelement. In diesem Tutorial verwenden Sie modellbindung zum Auffüllen einer **DropDownList** Steuerelement mit einer Liste von Produktkategorien.

Das Markup, das Sie hinzugefügt haben die *AdminPage.aspx* -Datei enthält eine **DropDownList** Steuerelement namens `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Sie verwenden modellbindung zum Auffüllen dieser **DropDownList** durch Festlegen der `ItemType` Attribut und die `SelectMethod` Attribut. Die `ItemType` Attribut gibt an, dass Sie verwenden die `WingtipToys.Models.Category` eingeben, wenn das Steuerelement auffüllen. Sie erstellen dieses Typs zu Beginn dieser tutorialreihe definiert die `Category` Klasse (siehe unten). Die `Category` Klasse ist in der *Modelle* Ordner innerhalb der *Category.cs* Datei.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

Die `SelectMethod` Attribut der **DropDownList** Steuerelement gibt an, dass Sie verwenden die `GetCategories` -Methode (siehe unten), enthalten in der CodeBehind-Datei (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Diese Methode gibt an, dass ein `IQueryable` Schnittstelle wird verwendet, um eine Abfrage auswerten einer `Category` Typ. Der zurückgegebene Wert wird zum Auffüllen der **DropDownList** im Markup der Seite (*AdminPage.aspx*).

Der Anzeigetext für jedes Element in der Liste, durch Festlegen angegeben wird der `DataTextField` Attribut. Die `DataTextField` -Attribut verwendet die `CategoryName` von der `Category` Klasse (siehe oben), um jede Kategorie in anzuzeigen die **DropDownList** Steuerelement. Der tatsächliche Wert, der übergeben wird, wenn ein Element, in ausgewählt ist der **DropDownList** Control basiert auf der `DataValueField` Attribut. Die `DataValueField` -Attributsatz auf die `CategoryID` im Definieren der `Category` Klasse (siehe oben).

### <a name="how-the-application-will-work"></a>Wie funktioniert die Anwendung

Wenn der Benutzer, der zu der Rolle "CanEdit" gehört zu der Seite zum ersten Mal, navigiert die `DropDownAddCategory` **DropDownList** Steuerelement wird aufgefüllt, wie oben beschrieben. Die `DropDownRemoveProduct` **DropDownList** Steuerelement auch mit Produkten, die die gleiche Weise gefüllt wird. Der Benutzer, die zu der Rolle "CanEdit" gehören den Typ und fügt die Produktdetails hinzu (**Namen**, **Beschreibung**, **Preis**, und **Bilddatei**). Klickt der Benutzer, der zu der Rolle "CanEdit" gehört der **Produkt hinzufügen** Schaltfläche der `AddProductButton_Click` Ereignishandler ausgelöst wird. Die `AddProductButton_Click` Ereignishandler befinden, in der CodeBehind-Datei (*AdminPage.aspx.cs*) überprüft die Image-Datei, um sicherzustellen, dass es die zulässigen Dateitypen entspricht *(.gif*, *PNG*, *JPEG*, oder *jpg*). Anschließend wird die Image-Datei in einen Ordner für die Wingtip Toys-beispielanwendung gespeichert. Als Nächstes wird das neue Produkt mit der Datenbank hinzugefügt. Zum Hinzufügen eines neuen Produkts, eine neue Instanz der dem `AddProducts` Klasse erstellt und mit dem Namen Products. Die `AddProducts` -Klasse verfügt über eine Methode namens `AddProduct`, und die Produkte-Objekt, ruft diese Methode, um Produkte in der Datenbank hinzufügen.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Wenn der Code das neue Produkt wurde erfolgreich zur Datenbank hinzugefügt wurden, ist erneuten Laden der Seite mit den Wert der Abfragezeichenfolge `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Wenn die Seite erneut geladen wird, ist die Abfragezeichenfolge in der URL enthalten. Durch erneutes Laden der Seite ein, der Benutzer, der zu der Rolle "CanEdit" gehört kann sofort sehen die Updates in der **DropDownList** steuert, auf die *AdminPage.aspx* Seite. Außerdem kann die Seite durch, einschließlich der Abfragezeichenfolge durch die URL an, eine Erfolgsmeldung an den Benutzer, der zu der Rolle "CanEdit" gehört angezeigt.

Wenn die *AdminPage.aspx* Seite Neuladen, die `Page_Load` Ereignis aufgerufen wird.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

Die `Page_Load` -Ereignishandler den Wert der Abfragezeichenfolge überprüft und bestimmt, ob eine Erfolgsmeldung angezeigt.

## <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt, um anzuzeigen, wie Sie hinzufügen, löschen und Aktualisieren von Elementen in den Einkaufswagen ausführen. Die einkaufswagensumme spiegeln die Gesamtkosten für alle Elemente im Warenkorb.

1. Drücken Sie im Projektmappen-Explorer **F5** zum Ausführen der Wingtip Toys-beispielanwendung.  
   Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Klicken Sie auf die **melden Sie sich bei** Link am oberen Rand der Seite. 

    ![Mitgliedschaft und Verwaltung – anmelden Link](membership-and-administration/_static/image2.png)

   Die *"Login.aspx"* angezeigt wird.
3. Verwenden Sie den folgenden Benutzernamen und das Kennwort ein:  
   Benutzername: canEditUser@wingtiptoys.com  
   Kennwort: Pa$ $word1 

    ![Mitgliedschaft und Verwaltung - Anmeldeseite](membership-and-administration/_static/image3.png)
4. Klicken Sie auf die **melden Sie sich bei** Schaltfläche am unteren Rand der Seite.
5. Klicken Sie oben auf der nächsten Seite auf die **Admin** Link zum Navigieren zu den *AdminPage.aspx* Seite. 

    ![Mitgliedschaft und Verwaltung "-" Admin-Link](membership-and-administration/_static/image4.png)
6. Um die Validierung von Benutzereingaben zu testen, klicken Sie auf die **Produkt hinzufügen** Schaltfläche ohne Produktdetails. 

    ![Mitgliedschaft und Verwaltung – Seite "Administrator"](membership-and-administration/_static/image5.png)

   Beachten Sie, dass die Pflichtfeld Nachrichten angezeigt werden.
7. Fügen Sie die Details für ein neues Produkt hinzu, und klicken Sie dann auf die **Produkt hinzufügen** Schaltfläche. 

    ![Mitgliedschaft und Verwaltung – hinzufügen Produkt](membership-and-administration/_static/image6.png)
8. Wählen Sie **Produkte** aus im oberen Navigationsmenü, um das neue Produkt anzuzeigen, in dem Sie hinzugefügt haben. 

    ![Mitgliedschaft und Verwaltung – neues Produkt anzeigen](membership-and-administration/_static/image7.png)
9. Klicken Sie auf die **Admin** Link, um zur Verwaltungsseite zurückzukehren.
10. In der **Produkt entfernen** Abschnitt der Seite, wählen Sie das neue Produkt, die Sie hinzugefügt, in haben der **DropDownListBox**.
11. Klicken Sie auf die **Produkt entfernen** Schaltfläche, um das neue Produkt aus der Anwendung zu entfernen. 

    ![Mitgliedschaft und Verwaltung – Remove-Produkt](membership-and-administration/_static/image8.png)
12. Wählen Sie **Produkte** im oberen Navigationsmenü zu bestätigen, dass das Produkt entfernt wurde.
13. Klicken Sie auf **Abmelden** Verwaltungsmodus vorhanden sein.   
    Beachten Sie, die im oberen Navigationsbereich nicht mehr angezeigt wird. die **Admin** Menüelement.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial müssen Sie eine benutzerdefinierte Rolle und ein Benutzer, die die benutzerdefinierte Rolle, eingeschränkten Zugriff auf den Ordner "Verwaltung" und die Seite hinzugefügt und Navigation für den Benutzer, die die benutzerdefinierte Rolle bereitgestellt. Sie wurden die modellbindung zum Auffüllen verwendet eine **DropDownList** -Steuerelement mit Daten. Sie implementiert die **"FileUpload"** Kontrolle und Steuerelementen zur gültigkeitsprüfung. Darüber hinaus haben Sie gelernt, hinzufügen und Entfernen von Produkten aus einer Datenbank. Im nächsten Tutorial erfahren Sie, wie ASP.NET-Routing implementiert werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

["Web.config" - Authorization-Element](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Bereitstellen einer sicheren ASP.NET Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-Website](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Zurück](checkout-and-payment-with-paypal.md)
> [Weiter](url-routing.md)
