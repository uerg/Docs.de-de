---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Mitgliedschaft und Administration | Microsoft Docs
author: Erikre
description: "Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: a10dbfe1ca49baee1604aac8dd9a1f93ccfcb7f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="membership-and-administration"></a>Mitgliedschaft und Verwaltung
====================
Durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die diese Reihe von Lernprogrammen begleitet.


In diesem Lernprogramm wird gezeigt, wie beim Aktualisieren der Wingtip Toys-beispielanwendung für das Hinzufügen einer benutzerdefinierten Rolle und ASP.NET Identity verwenden. Es zeigt auch, wie eine Seite "Verwaltung" implementiert, von dem der Benutzer mit einer benutzerdefinierten Rolle hinzufügen und Produkte aus der Website entfernen kann.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) ist das Mitgliedschaftssystem, die zum Erstellen von ASP.NET Web-Anwendung verwendet und steht in ASP.NET 4.5. ASP.NET Identity wird verwendet, in der Visual Studio 2013 Web Forms-Projektvorlage als auch die Vorlagen für [ASP.NET-MVC](../../../../mvc/index.md), [ASP.NET-Web-API](../../../../web-api/index.md), und [ASP.NET einseitigen Anwendung](../../../../single-page-application/index.md). Sie können auch ausdrücklich installieren die ASP.NET Identity-System mithilfe von NuGet, wenn Sie mit einer leeren Webanwendung starten. In diesem Tutorial Reihen verwenden Sie jedoch die **Web Forms**Projecttemplate, die das ASP.NET Identity-System enthält. ASP.NET Identity erleichtert die Anwendungsdaten benutzerspezifische Profildaten integriert. Darüber hinaus können mit ASP.NET Identity der Persistenzmodell für Benutzerprofile in Ihrer Anwendung auswählen. Sie können die Daten in einer SQL Server-Datenbank oder einen anderen Datenspeicher speichern einschließlich *NoSQL* Datenspeicher wie z. B. Windows Azure-Speichertabellen.

Dieses Lernprogramm baut auf dem vorherigen Lernprogramm, mit dem Titel "Auschecken und Zahlung mit PayPal" in der Reihe von Lernprogrammen Wingtip Toys.

## <a name="what-youll-learn"></a>Lernen Sie:

- Wie Sie Code verwenden, um eine benutzerdefinierte Sicherheitsrolle und ein Benutzer der Anwendung hinzufügen.
- Wie Sie den Zugriff auf den Ordner Verwaltung und die Seite einschränken.
- Wie Sie die Navigation für den Benutzer zu ermöglichen, zu der die benutzerdefinierte Rolle gehört.
- Vorgehensweise wurden die modellbindung verwenden Sie zum Auffüllen einer [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) -Steuerelement mit Produktkategorien.
- Gewusst wie: Hochladen einer Datei in der Anwendung über die [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) Steuerelement.
- Wie Sie Validierungssteuerelemente, um die Validierung von Benutzereingaben zu implementieren.
- Gewusst wie: Hinzufügen und Entfernen von Produkten aus der Anwendung.

## <a name="these-features-are-included-in-the-tutorial"></a>Diese Funktionen sind im Lernprogramm enthalten:

- ASP.NET Identity
- Konfiguration und Autorisierung
- Wurden die Modellbindung
- Unaufdringlichen Überprüfung

ASP.NET Web Forms stellt Mitgliedschaft. Mithilfe der Standardvorlage müssen Sie die der integrierten Mitgliedschaftsfunktionen, die Sie sofort verwenden können, wenn die Anwendung ausgeführt wird. In diesem Lernprogramm wird gezeigt, wie mithilfe von ASP.NET Identity zum Hinzufügen einer benutzerdefinierten Rolle und weisen Sie einen Benutzer, die dieser Rolle. Sie erfahren, wie den Zugriff auf den Ordner Verwaltung einschränken. Sie fügen eine Seite in den Ordner Verwaltung, die ermöglicht es einem Benutzer mit einer benutzerdefinierten Rolle hinzufügen und Entfernen von Produkten und ein Produkt Vorschau anzuzeigen, nachdem er hinzugefügt wurde.

## <a name="adding-a-custom-role"></a>Hinzufügen einer benutzerdefinierten Rolle

Mithilfe von ASP.NET Identity, können Sie eine benutzerdefinierte Sicherheitsrolle hinzufügen und Zuweisen eines Benutzers zu dieser Rolle mithilfe von Code.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste auf die *Logik* Ordner, und erstellen Sie eine neue Klasse.
2. Benennen Sie die neue Klasse *RoleActions.cs*.
3. Ändern Sie den Code, sodass er wie folgt aussieht:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. In **Projektmappen-Explorer**öffnen die *Global.asax.cs* Datei.
5. Ändern der *Global.asax.cs* Datei durch Hinzufügen des Codes in gelb hervorgehoben werden, sodass er wie folgt aussieht:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Beachten Sie, dass `AddUserAndRole` rot unterstrichen ist. Doppelklicken Sie auf den AddUserAndRole-Code.  
 Der Buchstabe "A" am Anfang der markierten Methode wird unterstrichen.
7. Zeigen Sie auf den Buchstaben "A", und klicken Sie auf der Benutzeroberfläche, die Sie zum Generieren eines Methodenstubs für ermöglicht die `AddUserAndRole` Methode. 

    ![Mitgliedschaft und Advministration - Methodenstub generieren](membership-and-administration/_static/image1.png)
8. Klicken Sie auf die Option mit dem Titel:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Öffnen der *RoleActions.cs* -Datei von der *Logik* Ordner.  
 Die `AddUserAndRole` Methode in der Klassendatei hinzugefügt wurde.
10. Ändern der *RoleActions.cs* Datei durch das Entfernen der `NotImplementedeException` und Hinzufügen von Code in gelb hervorgehoben, damit er wie folgt aussieht:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Der obige Code wird zuerst ein Datenbankkontext für die Mitgliedschaftsdatenbank. Die Mitgliedschaftsdatenbank wird auch als gespeichert ein *mdf* in der Datei die *App\_Daten* Ordner. Sie werden können, um diese Datenbank anzuzeigen, sobald der erste Benutzer dieser Webanwendung angemeldet hat. 

> [!NOTE] 
> 
> Wenn Sie die Daten der Benutzergruppenmitgliedschaft zusammen mit der Produktdaten speichern möchten, können Sie erwägen, mit dem gleichen **DbContext** , dass Sie zum Speichern von der Produktdaten im obigen Code verwendet.


 Die *interne* -Schlüsselwort ist einen Zugriffsmodifizierer für Typen (z. B. Klassen) und Typmembern (z. B. Methoden oder Eigenschaften). Interne Typen oder Member nur innerhalb der Dateien in der gleichen Assembly zugegriffen werden *(.dll* Datei). Bei der Erstellung Ihrer Anwendung eine Assemblydatei *(DLL*) erstellt wird, enthält den Code, der ausgeführt wird, wenn Sie die Anwendung ausführen. 

Ein `RoleStore` -Objekt, das Rollenverwaltung bereitstellt, wird basierend auf den Kontext der Datenbank erstellt.

> [!NOTE] 
> 
> Beachten Sie, dass bei der `RoleStore` -Objekt wird erstellt, verwendet eine generische `IdentityRole` Typ. Dies bedeutet, dass die `RoleStore` darf nur enthalten `IdentityRole` Objekte. Außerdem werden durch Verwenden von Generika, Ressourcen im Arbeitsspeicher besser verarbeitet.


Als Nächstes wird die `RoleManager` Objekt, das erstellt basierend auf der `RoleStore` -Objekt, das Sie gerade erstellt haben. die `RoleManager` Objekt stellt eine rollenbezogene API, die verwendet werden kann, automatisch zum Speichern der `RoleStore`. Die `RoleManager` darf nur enthalten `IdentityRole` Objekte, da der Code verwendet die `<IdentityRole>` generischen Typ.

Rufen Sie die `RoleExists` Methode, um zu bestimmen, ob die Rolle "CanEdit" in der Mitgliedschaftsdatenbank vorhanden ist. Wenn sie nicht der Fall ist, erstellen Sie die Rolle an.

Erstellen der `UserManager` Objekt anscheinend komplizierter als die `RoleManager` zu steuern, ist es jedoch nahezu identisch. Es wird nur auf einer Zeile anstatt mehrere codiert. Der Parameter, den Sie übergeben wird hier als ein neues Objekt, das in den Klammern enthaltenen instanziieren.

Als Nächstes erstellen Sie den Benutzer "CanEditUser" durch Erstellen eines neuen `ApplicationUser` Objekt. Klicken Sie dann, wenn Sie den Benutzer erstellen, fügen Sie den Benutzer der neuen Rolle.

> [!NOTE] 
> 
> Die Fehlerbehandlung wird während des Lernprogramms "ASP.NET Fehlerbehandlung" weiter unten in diesem Lernprogramm Reihe aktualisiert.


Das nächste Mal die Anwendung gestartet wird, wird der Benutzer mit dem Namen "CanEditUser" wie die Rolle mit dem Namen "CanEdit" der Anwendung hinzugefügt werden. Weiter unten in diesem Lernprogramm werden Sie sich anmelden, als der Benutzer "CanEditUser", um zusätzliche Funktionen anzuzeigen, die Sie im Verlauf dieses Lernprogramms hinzugefügt werden. API-Informationen zu ASP.NET Identity finden Sie unter der [Microsoft.AspNet.Identity-Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Weitere Informationen zum Initialisieren des Systems ASP.NET Identity finden Sie unter der [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Einschränken des Zugriffs auf die Seite "Verwaltung"

Die beispielanwendung des Wingtip Toys kann anonyme Benutzer und angemeldete Benutzer zum Anzeigen und Produkte kaufen. Der angemeldete Benutzer mit der benutzerdefinierten "CanEdit" Rolle kann jedoch eine zugriffsbeschränkte Seite zum Hinzufügen und Entfernen von Produkten zugreifen.

#### <a name="add-an-administration-folder-and-page"></a>Fügen Sie einen Ordner Verwaltung und Seite

Als Nächstes erstellen Sie einen Ordner namens *Admin* beispielanwendung für den "CanEditUser" Benutzer zur Rolle benutzerdefinierten der Wingtip Toys gehören.

1. Mit der rechten Maustaste des Projektnamens (**Wingtip Toys**) in **Projektmappen-Explorer** , und wählen Sie **hinzufügen**  - &gt; **neuer Ordner**.
2. Nennen Sie diesen Ordner *Admin*.
3. Mit der rechten Maustaste die *Admin* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
4. Wählen Sie die **Visual C#-** - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie aus der Liste mittleren **Webformular mit Gestaltungsvorlage**, nennen Sie sie *AdminPage.aspx***,** und wählen Sie dann **hinzufügen**.
5. Wählen Sie die *Site.Master* als Masterseite, und wählen Sie dann **OK**.

#### <a name="add-a-webconfig-file"></a>Fügen Sie eine Datei "Web.config" hinzu.

Durch Hinzufügen einer *"Web.config"* Datei wird in der *Admin* Ordner, Sie können den Zugriff auf die Seite, die im Ordner enthaltenen.

1. Mit der rechten Maustaste die *Admin* Ordner, und wählen **hinzufügen**  - &gt; **neues Element**.  
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie in der Liste der Visual c#-internetvorlagen, **Webkonfigurationsdatei**aus mittleren, akzeptieren Sie den Standardnamen der *"Web.config"***,** und wählen Sie dann auf **Hinzufügen**.
3. Ersetzen Sie das vorhandene XML-Inhalt in die *"Web.config"* Datei durch Folgendes:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Speichern Sie die *"Web.config"* Datei. Die *"Web.config"* Datei gibt an, dass nur Benutzerdaten, die zur Rolle "CanEdit" der Anwendung gehören kann greifen Sie auf der Seite, die in enthaltenen der *Admin* Ordner.

### <a name="including-custom-role-navigation"></a>Einschließlich benutzerdefinierte Rolle, die Navigation

Um dem Benutzer die Rolle "benutzerdefinierten"CanEdit"" Navigieren Sie zum Bereich "Verwaltung" der Anwendung zu aktivieren, müssen Sie einen Link zum Hinzufügen der *Site.Master* Seite. Nur Benutzer, die der Rolle "CanEdit" gehören, sehen die **Admin** verknüpfen und Zugriff auf den Bereich "Verwaltung".

1. Suchen Sie im Projektmappen-Explorer, und öffnen Sie die *Site.Master* Seite.
2. Um einen Link für den Benutzer der Rolle "CanEdit" zu erstellen, fügen Sie das Markup, das auf die folgenden ungeordnete gelb hervorgehoben `<ul>` Element, damit die Liste wird angezeigt, wie folgt:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Öffnen der *Site.Master.cs* Datei. Stellen Sie die **Admin** Link ist nur für den Benutzer "CanEditUser" durch Hinzufügen des Codes in gelb hervorgehoben angezeigt der `Page_Load` Handler. Die `Page_Load` Handler wird wie folgt aussehen:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Beim Laden der Seite überprüft der Code an, ob der angemeldete Benutzer die Rolle des "CanEdit". Wenn der Benutzer gehört, für die Rolle "CanEdit" das Span-Element mit dem Link zu der *AdminPage.aspx* Seite (und daher den Link in der Folge) sichtbar gemacht.

### <a name="enabling-product-administration"></a>Aktivieren der Product-Verwaltung

Bisher haben Sie erstellt die Rolle "CanEdit" und ein "CanEditUser" Benutzer, einen Ordner für die Verwaltung und eine Seite "Verwaltung" hinzugefügt. Sie Zugriffsrechte für den Ordner Verwaltung und die Seite "festgelegt haben und die Anwendung einen Navigationslink für die Benutzer der Rolle"CanEdit"hinzugefügt haben. Als Nächstes fügen Sie Markup für die *AdminPage.aspx* Seite und code in der *AdminPage.aspx.cs* Code-Behind-Datei, die dem Benutzer mit der Rolle "CanEdit" hinzufügen und Entfernen von Produkten ermöglichen.

1. In **Projektmappen-Explorer**öffnen die *AdminPage.aspx* -Datei von der *Admin* Ordner.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Öffnen Sie als Nächstes die *AdminPage.aspx.cs* Code-Behind-Datei mit der rechten Maustaste die *AdminPage.aspx* und auf **Code anzeigen**.
4. Ersetzen Sie den vorhandenen Code in der *AdminPage.aspx.cs* Code-Behind-Datei mit den folgenden Code:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

In den Code, der eingegebene für die *AdminPage.aspx.cs* Code-Behind-Datei, eine Klasse mit dem Namen `AddProducts` ist die eigentliche Arbeit der Produkte in der Datenbank hinzufügen. Diese Klasse ist nicht noch vorhanden, damit Sie ihn jetzt erstellen werden.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Logik* Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
 Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Code** Gruppe "Vorlagen" auf der linken Seite. Aktivieren Sie das Kontrollkästchen **Klasse**aus der Mitte aus, und nennen Sie sie *AddProducts.cs*.   
 Die neue Klassendatei wird angezeigt.
3. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

Die *AdminPage.aspx* ermöglicht es dem Benutzer zur Rolle "CanEdit" gehören zum Hinzufügen und Entfernen von Produkten. Wenn ein neues Produkt hinzugefügt wird, werden die Details zu dem Produkt überprüft, und klicken Sie dann in die Datenbank eingegeben. Das neue Produkt ist für alle Benutzer der Webanwendung sofort verfügbar.

#### <a name="unobtrusive-validation"></a>Unaufdringlichen Überprüfung

Die Produktdetails, die der Benutzer eingibt, auf die *AdminPage.aspx* Seite werden überprüft, über Validierungssteuerelemente (`RequiredFieldValidator` und `RegularExpressionValidator`). Diese Steuerelemente verwenden automatisch unaufdringlichen Überprüfung. Unaufdringlichen Überprüfung ermöglicht die Validierungssteuerelemente mit JavaScript für die clientseitige Validierungslogik, dies, dass die Seite nicht mit einem Vorgang mit dem Server bedeutet überprüft werden erfordert. Standardmäßig unaufdringlichen Überprüfung enthalten ist, der *"Web.config"* dateibasierten auf die folgende Konfigurationseinstellung:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Reguläre Ausdrücke

Der Produktpreis auf die *AdminPage.aspx* Seite überprüft wird, mit einem **RegularExpressionValidator** Steuerelement. Dieses Steuerelement überprüft, ob der Wert des zugeordneten Eingabesteuerelements (das Textfeld "AddProductPrice") das mit dem regulären Ausdruck angegebene Muster übereinstimmt. Ein regulärer Ausdruck ist eine Mustervergleich-Notation, die ermöglicht es Ihnen, schnell zu finden und bestimmte Muster übereinstimmen. Die **RegularExpressionValidator** Steuerelement enthält eine Eigenschaft namens `ValidationExpression` , enthält den regulären Ausdruck, der zum Preis-Eingabe überprüfen verwendet werden, wie unten dargestellt:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>FileUpload-Steuerelement

Zusätzlich zu den Steuerelementen Eingabe und Überprüfung Hinzufügen der **FileUpload** die Steuerung an die *AdminPage.aspx* Seite. Dieses Steuerelement bietet die Möglichkeit zum Hochladen von Dateien. In diesem Fall sind Sie nur die Bilddateien hochgeladen werden erlauben. In der CodeBehind-Datei (*AdminPage.aspx.cs*), wenn die `AddProductButton` geklickt wird, wird der Code überprüft die `HasFile` Eigenschaft der **FileUpload** Steuerelement. Wenn das Steuerelement verfügt über eine Datei, und wenn der Dateityp (auf der Grundlage der Dateierweiterung) zulässig ist, wird das Bild, um gespeichert die *Bilder* Ordner und die *Bilder/Daumen* Ordner der Anwendung.

#### <a name="model-binding"></a>Wurden die Modellbindung

Weiter oben in diesem Lernprogramm verwendet Sie wurden die modellbindung zum Auffüllen einer **ListView** -Steuerelement, ein **FormsView** -Steuerelement, ein **GridView** -Steuerelement, und ein  **DetailView** Steuerelement. In diesem Lernprogramm verwenden Sie wurden die modellbindung zum Auffüllen einer **DropDownList** -Steuerelement mit einer Liste von Produktkategorien.

Das Markup, das Sie hinzugefügt haben die *AdminPage.aspx* -Datei enthält eine **DropDownList** -Steuerelement namens `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Sie wurden die modellbindung verwenden, um dies zu füllen **DropDownList** durch Festlegen der `ItemType` Attribut und der `SelectMethod` Attribut. Die `ItemType` Attribut gibt an, dass Sie verwenden die `WingtipToys.Models.Category` eingeben, wenn das Steuerelement auffüllen. Sie erstellen dieses Typs zu Beginn dieser Reihe von Lernprogrammen definiert die `Category` Klasse (siehe unten). Die `Category` Klasse befindet sich in der *Modelle* Ordner innerhalb der *Category.cs* Datei.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

Die `SelectMethod` Attribut von der **DropDownList** Steuerelement gibt an, dass Sie die `GetCategories` Methode (siehe unten), in der Code-Behind-Datei enthalten (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Diese Methode gibt an, dass ein `IQueryable` Schnittstelle wird verwendet, um eine Abfrage auswerten einer `Category` Typ. Der zurückgegebene Wert dient zum Auffüllen der **DropDownList** im Markup der Seite (*AdminPage.aspx*).

Der Text angezeigt, für jedes Element in der Liste, durch Festlegen angegeben wird der `DataTextField` Attribut. Die `DataTextField` -Attribut verwendet die `CategoryName` von der `Category` Klasse (siehe oben) zum Anzeigen der einzelnen Kategorien in der **DropDownList** Steuerelement. Der tatsächliche Wert, der übergeben wird, wenn ein Element, in ausgewählt ist der **DropDownList** Steuerelement basiert auf der `DataValueField` Attribut. Die `DataValueField` -Attributsatz zur der `CategoryID` in der Definition der `Category` Klasse (siehe oben).

### <a name="how-the-application-will-work"></a>Wie funktioniert die Anwendung

Wenn der Benutzer zur Rolle "CanEdit" gehören zum ersten Mal zur Seite navigiert der `DropDownAddCategory` **DropDownList** Steuerelement wird aufgefüllt, wie oben beschrieben. Die `DropDownRemoveProduct` **DropDownList** Steuerelement auch mit Produkten die gleiche Weise gefüllt wird. Der Benutzer zur Rolle "CanEdit" gehören wählt den Typ der Kategorie und fügt die Produktdetails hinzu (**Namen**, **Beschreibung**, **Preis**, und **Bilddatei**). Klickt der Benutzer zur Rolle "CanEdit" gehören die **Produkt hinzufügen** Schaltfläche, die `AddProductButton_Click` -Ereignishandler ausgelöst wird. Die `AddProductButton_Click` -Ereignishandler im Code-Behind-Datei befindet (*AdminPage.aspx.cs*) überprüft die Image-Datei, um sicherzustellen, dass es die zulässigen Dateitypen entspricht *(GIF*, *PNG*, *JPEG*, oder *jpg*). Anschließend wird die Abbilddatei in der Ordnerstruktur des Wingtip Toys-beispielanwendung gespeichert. Als Nächstes wird das neue Produkt mit der Datenbank hinzugefügt. Zum Hinzufügen eines neuen Produkts, eine neue Instanz der Ausführen der `AddProducts` Klasse erstellt und mit dem Namen Produkte. Die `AddProducts` -Klasse verfügt über eine Methode namens `AddProduct`, und die Produkte Objekt ruft diese Methode, um die Produkte in der Datenbank hinzufügen.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Wenn der Code erfolgreich das neue Produkt mit der Datenbank hinzugefügt, die Seite wird erneut geladen, mit dem Wert der Abfragezeichenfolge `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Wenn die Seite erneut geladen wird, ist die Zeichenfolge der Abfrage in der URL enthalten. Durch das erneute Laden der Seite ein, der Benutzer zur Rolle "CanEdit" gehören kann sofort sehen die Updates in der **DropDownList** auf steuert die *AdminPage.aspx* Seite. Zudem kann dazu die Abfragezeichenfolge durch die URL die Seite eine Erfolgsmeldung an den Benutzer zur Rolle "CanEdit" gehören angezeigt werden.

Wenn die *AdminPage.aspx* Seite Neuladen, die `Page_Load` Ereignis aufgerufen wird.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

Die `Page_Load` -Ereignishandler den Wert der Abfragezeichenfolge überprüft und bestimmt, ob eine Erfolgsmeldung angezeigt.

## <a name="running-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt sehen, wie Sie hinzufügen können, löschen und Aktualisieren von Elementen in den Warenkorb ausführen. Die shopping Cart insgesamt wider, die Gesamtkosten für alle Elemente in den Einkaufswagen Sinn macht.

1. Drücken Sie im Projektmappen-Explorer **F5** zum Ausführen der Anwendung des Wingtip Toys-Beispiel.  
 Der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
2. Klicken Sie auf die **melden Sie sich** Link am oberen Rand der Seite. 

    ![Mitgliedschaft und Administration - anmelden Link](membership-and-administration/_static/image2.png)

 Die *"Login.aspx"* angezeigt wird.
3. Verwenden Sie den folgenden Benutzernamen und Kennwort:  
 Benutzername:canEditUser@wingtiptoys.com  
 Kennwort: Pa$ $word1 

    ![Mitgliedschaft und Administration - Anmeldeseite](membership-and-administration/_static/image3.png)
4. Klicken Sie auf die **melden Sie sich** Schaltfläche am unteren Rand der Seite.
5. Wählen Sie am Anfang der nächsten Seite die **Admin** Link zu navigieren der *AdminPage.aspx* Seite. 

    ![Mitgliedschaft und Administration - Admin-Link](membership-and-administration/_static/image4.png)
6. Um die Validierung von Benutzereingaben zu testen, klicken Sie auf die **Produkt hinzufügen** Schaltfläche ohne Produktdetails. 

    ![Mitgliedschaft und Verwaltung – Seite "Administrator"](membership-and-administration/_static/image5.png)

 Beachten Sie, dass das Pflichtfeld Nachrichten angezeigt werden.
7. Fügen Sie die Details für ein neues Produkt hinzu, und klicken Sie dann auf die **Produkt hinzufügen** Schaltfläche. 

    ![Fügen Sie Mitgliedschaft und Administration - Produkt hinzu.](membership-and-administration/_static/image6.png)
8. Wählen Sie **Produkte** im oberen Navigationsmenü auf das neue Produkt anzuzeigen, die Sie hinzugefügt. 

    ![Mitgliedschaft und Administration - neues Produkt anzeigen](membership-and-administration/_static/image7.png)
9. Klicken Sie auf die **Admin** Link auf der Seite "Verwaltung" zurückgegeben.
10. In der **Produkt entfernen** Abschnitt der Seite Wählen Sie das neue Produkt, die Sie hinzugefügt, in haben der **DropDownListBox**.
11. Klicken Sie auf die **Produkt entfernen** Schaltfläche, um das neue Produkt aus der Anwendung zu entfernen. 

    ![Mitgliedschaft und Administration - Produkt entfernen](membership-and-administration/_static/image8.png)
12. Wählen Sie **Produkte** aus dem oberen Navigationsmenü, um zu bestätigen, dass das Produkt entfernt wurden.
13. Klicken Sie auf **Abmelden** Verwaltungsmodus vorhanden sein.   
 Beachten Sie, die im oberen Navigationsbereich nicht mehr angezeigt wird der **Admin** Menüelement.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm hinzugefügt, eine benutzerdefinierte Sicherheitsrolle und ein Benutzer, der für die benutzerdefinierte Rolle eingeschränkten Zugriff auf den Ordner Verwaltung und die Seite gehört und Navigation für den Benutzer zur Rolle benutzerdefinierten gehören bereitgestellt. Sie wurden die modellbindung zum Auffüllen verwendet eine **DropDownList** -Steuerelement mit Daten. Sie implementiert die **FileUpload** Steuerelement und Validierungssteuerelemente. Darüber hinaus haben Sie gelernt, hinzufügen und Entfernen von Produkten aus einer Datenbank. In den nächsten Lernprogrammen erfahren Sie, wie ASP.NET-Routing implementiert werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

["Web.config" - Authorization-Element](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Bereitstellen Sie eine sichere ASP.NET Web Forms-App Mitgliedschaft, OAuth und SQL-Datenbank auf einer Azure-Website](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - Testversion](https://azure.microsoft.com/pricing/free-trial/)

>[!div class="step-by-step"]
[Zurück](checkout-and-payment-with-paypal.md)
[Weiter](url-routing.md)
