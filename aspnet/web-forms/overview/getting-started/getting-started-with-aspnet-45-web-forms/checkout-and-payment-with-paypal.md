---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Auschecken "und" Payment PayPal | Microsoft Docs
author: Erikre
description: Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 0dba613594686a28b82bc6d7701cda6e24b82e2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="checkout-and-payment-with-paypal"></a>Auschecken "und" Payment mit PayPal
====================
by [Erik Reitan](https://github.com/Erikre)

[Herunterladen des Wingtip Toys-Beispielprojekt (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [(PDF) E-Book herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese Reihe von Lernprogrammen erfahren Sie die Grundlagen der Erstellung einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die diese Reihe von Lernprogrammen begleitet.


In diesem Lernprogramm wird beschrieben, wie so ändern Sie die beispielanwendung Wingtip Toys benutzerautorisierung, Registrierung und Zahlung per PayPal eingeschlossen wird. Nur Benutzer, die angemeldet sind, müssen die Autorisierung Produkte zu kaufen. Die ASP.NET 4.5 Web Forms-Projektvorlage integrierte Registrierung Benutzerfunktionalität enthält bereits Großteil, was Sie benötigen. Sie fügen PayPal Express Checkout-Funktionen. In diesem Lernprogramm verwenden Sie das PayPal-Entwickler-Umgebung zu testen, so dass keine tatsächlichen Betrag übertragen werden sollen. Am Ende des Lernprogramms testen Sie die Anwendung durch Auswählen von Produkten zum Einkaufswagen, auf die Schaltfläche, und Übertragen von Daten mit der Website der PayPal-Tests hinzufügen. Auf der Website der PayPal-Test Sie bestätigen Sie die Informationen zum Protokollversand "und" Payment und dann mit der lokalen Wingtip Toys-beispielanwendung, um zu bestätigen, und führen Sie den Einkauf zurück.

Es gibt mehrere spezialisieren erfahrenen Drittanbieter-Payment-Prozessoren Onlineshopping dieser Adresse Skalierbarkeit und Sicherheit. ASP.NET-Entwickler sollten die Vorteile der Lösung eines Drittanbieters Zahlung vor dem Implementieren einer Warenkorb und Erwerb von Projektmappen nutzen.

> [!NOTE] 
> 
> Die beispielanwendung des Wingtip Toys wurde mit folgenden Zielen konzipiert bestimmte ASP.NET Konzepte und Funktionen verfügbar sind für ASP.NET Web-Entwickler angezeigt. Diese beispielanwendung wurde nicht für alle möglichen Situationen hinsichtlich der Skalierbarkeit und Sicherheit optimiert.


## <a name="what-youll-learn"></a>Lernen Sie:

- Wie Sie den Zugriff auf bestimmte Seiten in einem Ordner zu beschränken.
- Vorgehensweise: erstellen ein bekanntes Einkaufswagen aus einem anonymen Einkaufswagen.
- Zum Aktivieren von SSL für das Projekt.
- Wie Sie einen OAuth-Anbieter zum Projekt hinzufügen möchten.
- Wie PayPal mithilfe der PayPal-testumgebung Produkte zu kaufen.
- Vorgehensweise beim Anzeigen von Details von PayPal in einem **DetailsView** Steuerelement.
- Aktualisieren die Datenbank des Wingtip Toys-Anwendung mit Details von PayPal abgerufen.

## <a name="adding-order-tracking"></a>Hinzufügen von Order-Überwachung

In diesem Lernprogramm erstellen Sie zwei neue Klassen zum Nachverfolgen von Daten von der Reihenfolge, die ein Benutzer erstellt wurde. Die Klassen werden Daten im Zusammenhang mit Versandinformationen Kauf insgesamt und zahlungsbestätigung verfolgen.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Fügen Sie die Reihenfolge und OrderDetail Modellklassen

Weiter oben in diesem Lernprogramm Reihe, die Sie definiert des Schemas für die Kategorien, Produkte und Warenkorb Elemente durch das Erstellen der `Category`, `Product`, und `CartItem` Klassen in der *Modelle* Ordner. Fügen Sie nun zwei neue Klassen, um das Schema für die Reihenfolge, Produkt und die Details des Auftrags zu definieren.

1. In der **Modelle** Ordner, fügen Sie eine neue Klasse mit dem Namen *Order.cs*.   
   Die neue Klassendatei wird im Editor angezeigt.
2. Ersetzen Sie den Standardcode durch Folgendes:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Hinzufügen einer *OrderDetail.cs* Klasse, um die *Modelle* Ordner.
4. Ersetzen Sie den Standardcode durch folgenden Code:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Die `Order` und `OrderDetail` Klassen enthalten, das Schema, um die Bestellinformationen zum Kauf und Versand zu definieren.

Darüber hinaus müssen Sie zum Aktualisieren der Datenbank Context-Klasse, die die Entitätsklassen verwaltet und den Datenzugriff auf die Datenbank bereitstellt. Zu diesem Zweck fügen Sie den neu erstellten Auftrag und `OrderDetail` Modellklassen zu `ProductContext` Klasse.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *ProductContext.cs* Datei.
2. Den hervorgehobenen Code zum Hinzufügen der *ProductContext.cs* Datei wie unten dargestellt:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Wie bereits erwähnt in diesem Lernprogramm Reihe, die den Code in der *ProductContext.cs* Datei fügt die `System.Data.Entity` Namespace, damit Sie Zugriff auf die Kernfunktionen von Entity Framework haben. Diese Funktionalität umfasst die Möglichkeit, Abfragen, einfügen, aktualisieren und Löschen von Daten mit stark typisierten Objekten arbeiten. Der obige Code in der `ProductContext` Klasse fügt Entity Framework Zugriff auf das neu hinzugefügte `Order` und `OrderDetail` Klassen.

## <a name="adding-checkout-access"></a>Auschecken Zugriff hinzufügen

Die beispielanwendung des Wingtip Toys kann anonyme Benutzer überprüfen und einem Einkaufswagen Produkte hinzuzufügen. Jedoch wenn anonyme Benutzer wählen, die Produkte zu erwerben, die sie dem Warenkorb hinzugefügt, müssen sie auf der Website anmelden. Nachdem sie sich angemeldet haben, können sie die eingeschränkten Seiten der Webanwendung zugreifen, die behandeln das Auschecken und Prozess zu erwerben. Diese eingeschränkten Seiten befinden sich der *Auschecken* Ordner der Anwendung.

### <a name="add-a-checkout-folder-and-pages"></a>Fügen Sie einen Ordner Auschecken und Seiten

Erstellen Sie jetzt die *Auschecken* Ordner und die Seiten, die der Kunde während des Auscheckvorgangs angezeigt werden. Diese Seiten wird weiter unten in diesem Lernprogramm aktualisiert werden.

1. Mit der rechten Maustaste des Projektnamens (**Wingtip Toys**) in **Projektmappen-Explorer** , und wählen Sie **fügen Sie einen neuen Ordner**. 

    ![Auschecken "und" Payment PayPal - neuen Ordner](checkout-and-payment-with-paypal/_static/image1.png)
2. Nennen Sie diesen Ordner *Auschecken*.
3. Mit der rechten Maustaste die *Auschecken* Ordner, und wählen Sie dann **hinzufügen**-&gt;**neues Element**. 

    ![Auschecken "und" Payment PayPal - neues Element](checkout-and-payment-with-paypal/_static/image2.png)
4. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
5. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie dann im mittleren Bereich **Webformular mit Gestaltungsvorlage**und nennen Sie sie *CheckoutStart.aspx*. 

    ![Fügen Sie Auschecken "und" Payment PayPal - Dialogfeld "Neues Element" hinzu.](checkout-and-payment-with-paypal/_static/image3.png)
6. Wählen Sie wie zuvor den *Site.Master* Datei als die Gestaltungsvorlage.
7. Die folgenden zusätzlichen Seiten beim Hinzufügen der *Auschecken* Ordner mithilfe der oben genannten Schritte:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Fügen Sie eine Datei "Web.config" hinzu.

Durch Hinzufügen eines neuen *"Web.config"* Datei wird in der *Auschecken* Ordner zeilenfilterausdruck Zugriff auf alle im Ordner enthaltenen Seiten zu beschränken.

1. Mit der rechten Maustaste die *Auschecken* Ordner, und wählen **hinzufügen**  - &gt; **neues Element**.  
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie dann im mittleren Bereich **Webkonfigurationsdatei**, übernehmen Sie den Standardnamen *"Web.config"*, und wählen Sie dann **hinzufügen**.
3. Ersetzen Sie das vorhandene XML-Inhalt in die *"Web.config"* Datei durch Folgendes:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Speichern Sie die *"Web.config"* Datei.

Die *"Web.config"* Datei gibt an, dass alle unbekannten Benutzer der Webanwendung den Zugriff auf den Seiten, die in enthaltenen verweigert werden, müssen die *Auschecken* Ordner. Wenn jedoch der Benutzer verfügt über ein Konto registriert und angemeldet ist, sie wird ein bekannter Benutzer sein und haben Zugriff auf die Seiten in der *Auschecken* Ordner.

Es ist wichtig zu beachten, dass die ASP.NET-Konfiguration eine Hierarchie folgt, in dem jede *"Web.config"* Datei Konfigurationseinstellungen angewendet, auf den Ordner, die darauf und auf alle untergeordneten Verzeichnisse darunter.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Aktivieren von SSL für das Projekt

 Secure Sockets Layer (SSL) ist ein Protokoll, das definiert, dass Webservern und Webclients höherer Sicherheit durch Verschlüsselung Kommunikation zulassen. Wenn SSL nicht verwendet wird, ist zwischen Client und Server gesendete Daten zu Paket-sniffing, indem jede Person mit physischem Zugriff auf das Netzwerk geöffnet. Darüber hinaus sind einige allgemeine Authentifizierungsschemas nicht über einfaches HTTP sicher. Insbesondere senden Standardauthentifizierung und Formularauthentifizierung unverschlüsselte Anmeldeinformationen. Um sicher zu sein, müssen dieses Authentifizierungsschema SSL verwendet. 

1. In **Projektmappen-Explorer**, klicken Sie auf die **WingtipToys** Projekt, und drücken Sie dann **F4** zum Anzeigen der **Eigenschaften** Fenster.
2. Änderung **SSL aktiviert** auf `true`.
3. Kopieren der **SSL-URL** , damit Sie später verwenden können.   
 Die SSL-URL `https://localhost:44300/` , wenn Sie SSL-Websites zuvor erstellt haben (wie unten gezeigt).   
    ![Projekteigenschaften](checkout-and-payment-with-paypal/_static/image4.png)
4. In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die **WingtipToys** Projekt, und klicken Sie auf **Eigenschaften**.
5. Klicken Sie in der linken Registerkarte auf **Web**.
6. Ändern der **Projekt-Url** verwenden die **SSL-URL** , die Sie zuvor gespeichert haben.   
    ![Webeigenschaften für workflowdienstprojekt](checkout-and-payment-with-paypal/_static/image5.png)
7. Speichern Sie die Seite durch Drücken von **STRG + S**.
8. Drücken Sie **STRG+F5**, um die Anwendung auszuführen. Visual Studio zeigt eine Option aus, um die Ihnen ermöglichen, die SSL-Warnungen zu vermeiden.
9. Klicken Sie auf **Ja** IIS Express-SSL-Zertifikat vertrauen, und den Vorgang fortsetzen.   
    ![Details für IIS Express-SSL-Zertifikats](checkout-and-payment-with-paypal/_static/image6.png)  
 Es wird eine Sicherheitswarnung angezeigt.
10. Klicken Sie auf **Ja** zum Installieren des Zertifikats auf Ihrem "localhost".   
    ![Sicherheit Warnung (Dialogfeld)](checkout-and-payment-with-paypal/_static/image7.png)  
 Das Browserfenster wird angezeigt.

Sie können nun einfach Ihre Web-Anwendung, die lokal mithilfe von SSL testen.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Fügen Sie einen OAuth 2.0-Anbieter

ASP.NET Web Forms bietet verbesserte Optionen für die Mitgliedschaft und Authentifizierung. Dazu zählen OAuth. OAuth ist ein offenes Protokoll, das in einer einfachen und standard-Methode aus, mobilen und desktop-Webanwendungen sichere Autorisierung ermöglicht. Der ASP.NET Web Forms-Vorlage wird mit OAuth um als Authentifizierungsanbieter Facebook, Twitter, Google und Microsoft verfügbar zu machen. Obwohl dieses Lernprogramm nur Google als Authentifizierungsanbieter verwendet wird, können Sie einfach den Code zur Verwendung eines der Anbieter ändern. Die Schritte zur Implementierung von anderen Anbietern sind sehr ähnlich, mit den Schritten, die Sie in diesem Lernprogramm sehen werden.

Zusätzlich zur Authentifizierung wird das Lernprogramm auch Rollen verwenden, um die Autorisierung zu implementieren. Nur die Benutzer, die Sie, um hinzufügen die `canEdit` Rolle wird in der Lage, Daten zu ändern (erstellen, bearbeiten oder Löschen von Kontakten).

> [!NOTE] 
> 
> Windows Live-Anwendungen akzeptieren nur eine live-URL für eine Website verwenden, damit Sie eine lokale Website-URL für das Testen von Benutzernamen verwenden können.


Die folgenden Schritte können Sie einen Google-Authentifizierungsanbieter hinzufügen.

1. Öffnen der *App\_Start\Startup.Auth.cs* Datei.
2. Entfernen Sie die Kommentarzeichen aus der `app.UseGoogleAuthentication()` Methode, damit die Methode angezeigt wird, wie folgt: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Navigieren Sie zu der [Google-Entwicklerkonsole](https://console.developers.google.com/). Sie müssen auch zur Anmeldung mit Ihrem Google Developer-e-Mail-Konto (gmail.com). Wenn Sie nicht mit einem Google-Konto verfügen, wählen Sie die **erstellen Sie ein Konto** Link.   
   Als Nächstes sehen Sie die **Google-Entwicklerkonsole**.   
    ![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)
4. Klicken Sie auf die **Projekt erstellen** und geben Sie einen Projektnamen und eine ID (Sie können die Standardwerte verwenden). Klicken Sie auf die **Vereinbarung Kontrollkästchen** und **erstellen** Schaltfläche.  

    ![Google - neues Projekt](checkout-and-payment-with-paypal/_static/image9.png)

   In wenigen Sekunden wird das neue Projekt erstellt werden, und Ihr Browser wird die Seite "neue Projekte" angezeigt.
5. Klicken Sie in der linken Registerkarte auf **APIs &amp; Auth**, und klicken Sie dann auf **Anmeldeinformationen**.
6. Klicken Sie auf die **neue Client-ID erstellen** unter **OAuth**.   
   Die **Client-ID erstellen** Dialogfeld wird angezeigt.   
    ![Google - Client-ID erstellen](checkout-and-payment-with-paypal/_static/image10.png)
7. In der **Client-ID erstellen** Dialogfeld, behalten Sie den Standardwert **-Webanwendung** für den Anwendungstyp.
8. Legen Sie die **autorisierte JavaScript-Ursprünge** , die Sie zuvor in diesem Lernprogramm verwendete SSL-URL (`https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben).   
   Diese URL ist der Ursprung für Ihre Anwendung. Für dieses Beispiel werden Sie nur die "localhost" Test-URL eingeben. Allerdings können Sie mehrere URLs um "localhost" und Produktion berücksichtigen eingeben.
9. Legen Sie die **Umleitungs-URI autorisiert** , der dem folgenden: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Dieser Wert ist der URI, ASP.NET OAuth Benutzer mit der Google OAuth-Server kommunizieren. Beachten Sie die oben verwendeten SSL-URL ( `https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben).
10. Klicken Sie auf die **Client-ID erstellen** Schaltfläche.
11. Klicken Sie im linken Menü des Google-Entwicklerkonsole, auf die **Zustimmung Bildschirm** Menüelement, legen Sie dann die e-Mail-Adresse und Product Name. Wenn Sie das Formular ausgefüllt haben, klicken Sie auf **speichern**.
12. Klicken Sie auf die **APIs** Menüelement, einen Bildlauf nach unten, und klicken Sie auf die **deaktiviert** neben **Google + API**.   
    Akzeptieren diese Option wird der Google +-API ermöglichen.
13. Sie müssen außerdem ein update der **"Microsoft.owin"** NuGet-Paket auf Version 3.0.0.   
    Aus der **Tools** klicken Sie im Menü **NuGet Package Manager** und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten**.  
    Aus der **NuGet-Pakete verwalten** Fenster Suchen und Aktualisieren der **"Microsoft.owin"** Paket auf Version 3.0.0.
14. In Visual Studio aktualisiert die `UseGoogleAuthentication` Methode der *Startup.Auth.cs* Seite durch Kopieren und Einfügen der **Client-ID** und **geheimen Clientschlüssel** an die Methode. Die **Clientkennung** und **Clientschlüssel** unten aufgeführten Werte sind Beispiele und funktionieren nicht. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Drücken Sie **STRG + F5** erstellen und Ausführen der Anwendung. Klicken Sie auf die **melden Sie sich** Link.
16. Klicken Sie unter **einen anderen Dienst zum Anmelden verwenden**, klicken Sie auf **Google**.  
    ![Anmelden](checkout-and-payment-with-paypal/_static/image11.png)
17. Wenn Sie Ihre Anmeldeinformationen eingeben müssen, werden Sie auf der Google-Website weitergeleitet, wo Sie Ihre Anmeldeinformationen eingeben werden.  
    ![Google - Anmeldung](checkout-and-payment-with-paypal/_static/image12.png)
18. Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, werden Sie aufgefordert, Berechtigungen für die Webanwendung zu geben, die Sie gerade erstellt haben.  
    ![Projekt Standarddienstkonto](checkout-and-payment-with-paypal/_static/image13.png)
19. Klicken Sie auf **akzeptieren**. Nun werden Sie umgeleitet an die **registrieren** auf der Seite der **WingtipToys** Anwendung, wo Sie Ihre Google-Konto registrieren.  
    ![Mit Ihrem Google-Konto registrieren](checkout-and-payment-with-paypal/_static/image14.png)
20. Sie haben die Möglichkeit, das Ändern des lokalen e-Mail-Registrierung für Ihr Konto Gmail verwendet, aber im Allgemeinen das standardmäßige e-Mail-Alias (d. h., die Sie für die Authentifizierung verwendet eine) bleiben sollen. Klicken Sie auf **melden Sie sich** wie oben gezeigt.

### <a name="modifying-login-functionality"></a>Ändern die Anmeldefunktionalität

Wie in diesem Lernprogramm Reihe zuvor erwähnt ist ein Großteil der Funktionalität für die Registrierung in der Vorlage für ASP.NET Web Forms standardmäßig enthalten. Nachdem Sie die Standardeinstellung ändern, werden *"Login.aspx"* und *Register.aspx* Seiten zum Aufrufen der `MigrateCart` Methode. Die `MigrateCart` Methode ordnet einen neu angemeldeten Benutzer eine anonyme Einkaufswagen legen. Durch den Benutzer zuordnen und Einkaufswagen, wird die beispielanwendung des Wingtip Toys den Einkaufswagen des Benutzers zwischen Besuche verwalten können.

1. In **Projektmappen-Explorer**, suchen und öffnen Sie die *Konto* Ordner.
2. Ändern Sie die Code-Behind-Seite mit dem Namen *Login.aspx.cs* auf den Code in gelb hervorgehoben enthalten, damit er wie folgt aussieht:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Speichern Sie die *Login.aspx.cs* Datei.

Jetzt können Sie ignorieren der Warnung, die es keine Definition für ist die `MigrateCart` Methode. Sie werden weiter unten in diesem Lernprogramm hinzugefügt werden.

Die *Login.aspx.cs* Code-Behind-Datei unterstützt eine LogIn-Methode. Durch Untersuchen der Seite "Login.aspx" werden Sie feststellen, dass diese Seite eine Schaltfläche "Anmelden" enthält, die bei Klicken Sie auf die Trigger der `LogIn` angegebenen Ereignishandler für das Code-Behind-Modell.

Wenn die `Login` Methode für die *Login.aspx.cs* aufgerufen wird, wird eine neue Instanz der mit dem Namen Einkaufswagen `usersShoppingCart` wird erstellt. Die ID des Warenkorbs (eine GUID) abgerufen und zum Festlegen der `cartId` Variable. Anschließend wird die `MigrateCart` -Methode aufgerufen wird, übergibt sowohl die `cartId` und den Namen des angemeldeten Benutzers für diese Methode. Wenn der Warenkorb migriert wird, wird die GUID, die zur Identifizierung des anonyme Einkaufswagen mit dem Benutzernamen ersetzt.

Zusätzlich zum Ändern der *Login.aspx.cs* Code-Behind-Datei auf den Einkaufswagen migriert werden, wenn der Benutzer anmeldet, müssen Sie auch ändern der *Register.aspx.cs Code-Behind-Datei* Einkaufswagen migrieren Wenn der Benutzer ein neues Konto erstellt und anmeldet.

1. In der *Konto* Ordner, öffnen die Code-Behind-Datei mit dem Namen *Register.aspx.cs*.
2. Ändern Sie den Code-Behind-Datei durch den Code in Gelb, einschließlich, sodass er wie folgt aussieht:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Speichern Sie die *Register.aspx.cs* Datei. Ignorieren Sie die Warnung noch einmal: Informationen zu den `MigrateCart` Methode.

Beachten Sie, dass der Code in verwendet das `CreateUser_Click` -Ereignishandler ist sehr ähnlich dem Code verwendet werden, in der `LogIn` Methode. Wenn der Benutzer registriert oder meldet sich der Standort, einem Aufruf der `MigrateCart` Methode erfolgen.

## <a name="migrating-the-shopping-cart"></a>Migrieren den Einkaufswagen

Nun, da Sie die Anmeldung und Registrierung Prozess aktualisiert haben, können Sie den Code zum Migrieren der shopping Cart mit Hinzufügen der `MigrateCart` Methode.

1. In **Projektmappen-Explorer**, suchen die *Logik* Ordner, und öffnen Sie die *ShoppingCartActions.cs* Klassendatei.
2. Fügen Sie den Code, um den vorhandenen Code in gelb hervorgehoben der *ShoppingCartActions.cs* -Datei, damit der Code in der *ShoppingCartActions.cs* Datei sieht wie folgt aus:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

Die `MigrateCart` Methode verwendet den vorhandenen CartId den Einkaufswagen des Benutzers gefunden. Als Nächstes der Code durchläuft die shopping Cart-Elemente und ersetzt die `CartId` Eigenschaft (gemäß der `CartItem` Schema) mit dem Namen des angemeldeten Benutzers.

### <a name="updating-the-database-connection"></a>Aktualisieren die Verbindung mit der Datenbank

Wenn Sie dieses Lernprogramm mithilfe der **vorgefertigten** Wingtip Toys-beispielanwendung müssen Sie die Standarddatenbank für die Mitgliedschaft neu erstellen. Durch Ändern der Standard-Verbindungszeichenfolge, wird die Mitgliedschaftsdatenbank das nächste Mal erstellt die Anwendung ausgeführt wird.

1. Öffnen der *"Web.config"* Datei im Stammverzeichnis des Projekts.
2. Aktualisieren Sie die Standard-Verbindungszeichenfolge, sodass er wie folgt aussieht:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integration von PayPal

PayPal ist eine webbasierte Abrechnung-Plattform, die von online-Händlern Zahlungen akzeptiert. In diesem Lernprogramm wird als Nächstes erläutert, wie PayPal Express Checkout-Funktionen in Ihre Anwendung integrieren. Express Auschecken kann Ihre Kunden PayPal bezahlen für die Elemente in seinen Einkaufswagen legen hinzugefügte verwenden.

### <a name="create-paylpal-test-accounts"></a>Erstellen von PaylPal-Testkonten

Um die PayPal-testumgebung zu verwenden, müssen Sie erstellen und überprüfen Sie ein Entwicklerkonto für den Test. Sie werden vom Test-Entwicklerkonto verwenden, um ein Käufer Testkonto und einem Verkäufer-Testkonto erstellen. Die Anmeldeinformationen des Testkontos Developer ermöglichen außerdem die Wingtip Toys-beispielanwendung auf die Tests PayPal-Umgebung zuzugreifen.

1. Navigieren Sie in einem Browser zu der Website testen PayPal-Entwickler:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Wenn Sie eine PayPal-Entwicklerkonto haben, erstellen Sie ein neues Konto, indem Sie auf **registrieren**und die Registrierung über die Schritte befolgen. Wenn Sie eine vorhandene PayPal-Entwicklerkonto haben, melden Sie sich, indem Sie auf **anmelden**. Sie benötigen Ihre PayPal-Entwicklerkonto So testen Sie die beispielanwendung des Wingtip Toys weiter unten in diesem Lernprogramm.
3. Wenn Sie nur für Ihre PayPal-Entwickler-Konto registriert haben, müssen Sie möglicherweise Ihre PayPal-Entwickler-Konto mit PayPal bestätigen. Sie können Ihr Konto überprüfen, indem Sie die Schritte, die PayPal an Ihr e-Mail-Konto gesendet. Nachdem Sie Ihre PayPal-Entwicklerkonto überprüft haben, melden Sie sich wieder der PayPal-Entwickler, die Website testen.
4. Nachdem Sie, PayPal Developer-Website mit Ihrem PayPal-Entwicklerkonto an, dass Sie müssen ein PayPal Käufer-Testkonto erstellen angemeldet sind, wenn Sie noch nicht vorhanden. So erstellen ein Testkonto Käufer PayPal-Website auf die **Anwendungen** Registerkarte, und klicken Sie dann auf **Sandkasten Konten**.   
 Die **Sandkasten Testkonten** Seite wird angezeigt.   

    > [!NOTE] 
    > 
    > PayPal-Entwicklerwebsite enthält bereits ein Händler Testkonto.

    ![Auschecken "und" Payment PayPal - Sandkasten-Testkonten](checkout-and-payment-with-paypal/_static/image15.png)
5. Klicken Sie auf der Seite Sandkasten Test Konten auf **Konto erstellen**.
6. Auf der **Testkonto erstellen** auszuwählen, die ein Käufer Testmail-Konto und das Kennwort Ihrer Wahl.   

    > [!NOTE] 
    > 
    > Sie benötigen den Käufer e-Mail-Adressen und das Kennwort zum Testen der Anwendung des Wingtip Toys-Beispiel am Ende dieses Lernprogramms.

    ![Auschecken "und" Payment PayPal - Sandkasten-Testkonten](checkout-and-payment-with-paypal/_static/image16.png)
7. Der Käufer-Testkonto erstellen, indem Sie auf die **Konto erstellen** Schaltfläche.  
 Die **Sandkasten Testkonten** angezeigt wird. 

    ![Auschecken "und" Payment PayPal - Konten PaylPal](checkout-and-payment-with-paypal/_static/image17.png)
8. Auf der **Sandkasten Testkonten** auf die **Moderator** e-Mail-Konto.  
    **Profil** und **Benachrichtigung** Optionen angezeigt.
9. Wählen Sie die **Profil** option aus, und klicken Sie auf **API Anmeldeinformationen** Ihre API-Anmeldeinformationen für das Geschäftskonto Testkonto anzeigen.
10. Kopieren Sie die TEST-API-Anmeldeinformationen in den Editor ein.

Sie benötigen Ihre angezeigten klassische TEST-API-Anmeldeinformationen (Benutzername, Kennwort und Signatur) API-Aufrufe aus der Anwendung des Wingtip Toys-Beispiel ausführen, die testumgebung, PayPal. Sie werden im nächsten Schritt fügen Sie die Anmeldeinformationen hinzu.

### <a name="add-paypal-class-and-api-credentials"></a>Hinzufügen von PayPal-Klasse und API-Anmeldeinformationen

Setzen Sie den Großteil der PayPal-Code in eine einzelne Klasse. Diese Klasse enthält die Methoden zur Kommunikation mit PayPal verwendet. Darüber hinaus fügen Sie diese Klasse Ihre PayPal-Anmeldeinformationen.

1. Die beispielanwendung des Wingtip Toys innerhalb von Visual Studio, mit der Maustaste die **Logik** Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Klicken Sie unter **Visual C#-** aus der **installiert** Bereich auf der linken Seite, wählen **Code**.
3. Wählen Sie in der Mitte **Klasse**. Nennen Sie diese neue Klasse **PayPalFunctions.cs**.
4. Klicken Sie auf **Hinzufügen**.  
   Die neue Klassendatei wird im Editor angezeigt.
5. Ersetzen Sie den Standardcode durch folgenden Code:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Fügen Sie die Anbieter-API-Anmeldeinformationen (Benutzername, Kennwort und Signatur), die Sie zuvor in diesem Lernprogramm angezeigt, sodass Sie die Funktionsaufrufe in der testumgebung PayPal vornehmen können.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> In dieser beispielanwendung werden die Anmeldeinformationen in einer C#-Datei (cs) einfach hinzugefügt werden. Allerdings in einer Lösung implementierten sollten Sie Ihre Anmeldeinformationen in einer Konfigurationsdatei verschlüsseln.


Die NVPAPICaller-Klasse enthält den Großteil der PayPal-Funktionalität. Der Code in der Klasse enthält die Methoden, die erforderlich sind, um einen Test aus der testumgebung PayPal erwerben. Die folgenden drei PayPal-Funktionen werden verwendet, um Einkäufe zu tätigen:

- `SetExpressCheckout` Funktion
- `GetExpressCheckoutDetails` Funktion
- `DoExpressCheckoutPayment` Funktion

Die `ShortcutExpressCheckout` Methode sammelt die Test-Kaufdetails an Informationen und Produkt aus den Einkaufswagen und ruft die `SetExpressCheckout` PayPal-Funktion. Die `GetCheckoutDetails` Methode bestätigt Kaufdetails und ruft die `GetExpressCheckoutDetails` PayPal-Funktion, bevor Sie den Test Kauf vornehmen. Die `DoCheckoutPayment` Methode schließt den Kauf Test aus der testumgebung durch Aufrufen der `DoExpressCheckoutPayment` PayPal-Funktion. Im restlichen Code unterstützt die PayPal-Methoden und der Prozess, z. B. Zeichenfolgen Codierung, Decodierung von Zeichenfolgen, Arrays verarbeiten und bestimmen die Anmeldeinformationen an.

> [!NOTE] 
> 
> PayPal ermöglicht Ihnen das Einfügen von optionalen Kaufdetails basierend auf [PayPal API-Spezifikation](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Erweitern den Code in der beispielanwendung des Wingtip Toys, können Sie Details Lokalisierung, produktbeschreibungen, Steuer, eine Kundennummer für den Dienst sowie viele andere optionale Felder enthalten.


Beachten Sie, dass die Rückgabe- und "Abbrechen" URLs, die im angegebenen der **ShortcutExpressCheckout** Methode verwenden, eine Portnummer an.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Wenn Visual Web Developer ein Webprojekt über SSL ausgeführt wird, wird der Port 44300 häufig für den Webserver verwendet werden. Wie oben gezeigt, ist die Nummer des Ports 44300. Wenn Sie die Anwendung ausführen, können Sie eine andere Portnummer anzeigen. Der Port Number muss ordnungsgemäß im Code festgelegt werden, damit können Sie erfolgreich führen Sie die beispielanwendung des Wingtip Toys am Ende dieses Lernprogramms. Im nächsten Abschnitt dieses Lernprogramm wird erläutert, wie die Portnummer für den lokalen Host abgerufen und aktualisiert die PayPal-Klasse.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aktualisieren Sie die Portnummer auf "localhost" in der PayPal-Klasse

Die beispielanwendung des Wingtip Toys erwirbt Produkte durch Navigieren zum Testen PayPal-Website und die Rückgabe an Ihre lokale Instanz des Wingtip Toys-beispielanwendung. PayPal mit der richtigen URL zurückgegeben werden sollen, müssen Sie die Portnummer der lokal ausgeführten angeben beispielanwendung in der oben genannten PayPal-Code.

1. Mit der rechten Maustaste des Projektnamens (**WingtipToys**) in **Projektmappen-Explorer** , und wählen Sie **Eigenschaften**.
2. Wählen Sie in der linken Spalte der **Web** Registerkarte.
3. Rufen Sie die Portnummer der **Projekt-Url** Feld.
4. Aktualisieren Sie ggf. die `returnURL` und `cancelURL` in der PayPal-Klasse (`NVPAPICaller`) in der *PayPalFunctions.cs* die Portnummer Ihrer Webanwendung zu verwendende Datei:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Der Code, den Sie hinzugefügt wird jetzt den erwarteten Port für die lokale Web-Anwendung überein. PayPal werden können mit der richtigen URL auf dem lokalen Computer zurückgegeben.

### <a name="add-the-paypal-checkout-button"></a>Der Checkout PayPal-Schaltfläche "hinzufügen"

Nun, dass die beispielanwendung die Hauptfunktionen von PayPal hinzugefügt haben, können Sie beginnen, Hinzufügen von Markup und Code erforderlich, um diese Funktionen aufrufen. Zunächst müssen Sie die Schaltfläche "Auschecken" hinzufügen, die dem Benutzer auf die Warenkorb-Seite angezeigt wird.

1. Öffnen der *ShoppingCart.aspx* Datei.
2. Bildlauf zum Ende der Datei, und suchen Sie die `<!--Checkout Placeholder -->` Kommentar.
3. Ersetzen Sie den Kommentar mit einer `ImageButton` steuern, sodass die Markierung von, das wie folgt ersetzt wird:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. In der *ShoppingCart.aspx.cs* Datei, nachdem die `UpdateBtn_Click` Ereignishandler gegen Ende der Datei hinzufügen der `CheckOutBtn_Click` Ereignishandler:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Auch in der *ShoppingCart.aspx.cs* Datei, fügen einen Verweis auf die `CheckoutBtn`, sodass die neue Schaltfläche "Bild" wie folgt verwiesen wird:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Speichern Sie die Änderungen an der *ShoppingCart.aspx* Datei und die *ShoppingCart.aspx.cs* Datei.
7. Wählen Sie in der Menüleiste **Debuggen**-&gt;**WingtipToys erstellen**.  
   Das Projekt wird neu erstellt werden mit der neu hinzugefügten **ImageButton** Steuerelement.

### <a name="send-purchase-details-to-paypal"></a>Kaufdetails an PayPal senden

Wenn der Benutzer klickt auf die **Auschecken** auf die Warenkorb-Seite auf die Schaltfläche (*ShoppingCart.aspx*), beginnen sie den Kaufvorgang fort. Der folgende Code Ruft die erste PayPal-Funktion, die erforderlich sind, Produkte zu kaufen.

1. Aus der *Auschecken* Ordner, öffnen die Code-Behind-Datei mit dem Namen *CheckoutStart.aspx.cs*.   
   Achten Sie darauf, dass die Code-Behind-Datei zu öffnen.
2. Ersetzen Sie den vorhandenen Code durch folgenden Code:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Klickt der Benutzer der Anwendung die **Auschecken** auf die Warenkorb-Seite, der Browser auf die Schaltfläche wird, wechseln Sie zu der *CheckoutStart.aspx* Seite. Wenn die *CheckoutStart.aspx* Seite geladen wird, die `ShortcutExpressCheckout` -Methode aufgerufen wird. An diesem Punkt wird der Benutzer auf der Website der PayPal-Tests übertragen. Auf der Website PayPal der Benutzer gibt seine Anmeldeinformationen PayPal Kaufdetails, akzeptiert die PayPal-Vereinbarung und überprüft an die Anwendung des Wingtip Toys Beispiel zurückgegeben, in denen die `ShortcutExpressCheckout` Methode abgeschlossen wird. Wenn die `ShortcutExpressCheckout` Methode abgeschlossen ist, wird es den Benutzer umleiten der *CheckoutReview.aspx* im angegebene Seite die `ShortcutExpressCheckout` Methode. Dies ermöglicht es dem Benutzer, den Bestellungsdetails aus innerhalb der Anwendung des Wingtip Toys Beispiel zu überprüfen.

### <a name="review-order-details"></a>Überprüfen Sie Auftragsdetails

Nach der Rückkehr von PayPal, die *CheckoutReview.aspx* Seite des Wingtip Toys-beispielanwendung zeigt die Auftragsdetails enthält. Auf dieser Seite können den Benutzer zu den Details der Bestellung zu überprüfen, bevor die Produkte kaufen. Die *CheckoutReview.aspx* Seite muss wie folgt erstellt werden:

1. In der *Auschecken* Ordner öffnen die Seite mit dem Namen *CheckoutReview.aspx*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Öffnen Sie die CodeBehind-Seite mit dem Namen *CheckoutReview.aspx.cs* und Ersetzen Sie den vorhandenen Code durch Folgendes:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Die **DetailsView** Steuerelement wird verwendet, um die Auftragsdetails anzeigen, die von PayPal zurückgegeben wurden. Der obige Code speichert auch, den Bestellungsdetails der Wingtip Toys-Datenbank als eine `OrderDetail` Objekt. Wenn der Benutzer klickt auf die **vollständigen Order** Schaltfläche, die er an umgeleitet werden die *CheckoutComplete.aspx* Seite.

> [!NOTE] 
> 
> **Tipps**
> 
> Im Markup der *CheckoutReview.aspx* Seite, beachten Sie, dass die `<ItemStyle>` Tag wird verwendet, um das Format der Elemente im Ändern der **DetailsView** Steuerelement am unteren Rand der Seite. Durch Anzeigen der Seite in **Entwurfsansicht** (durch Auswahl **Entwurf** in der unteren linken Ecke von Visual Studio), wählen Sie dann die **DetailsView** steuern und die auswählen** Smarttag** (das Pfeilsymbol am oberen Rand des Steuerelements nach rechts), werden können, finden Sie unter der **DetailsView-Aufgaben**.
> 
> ![Auschecken "und" Payment PayPal - Bearbeiten Felder](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Durch Auswahl **Felder bearbeiten**, **Felder** Dialogfeld wird angezeigt. In diesem Dialogfeld Sie können problemlos steuern die visuellen Eigenschaften, z. B. **ItemStyle**, der die **DetailsView** Steuerelement.
> 
> ![Auschecken "und" Payment PayPal - Felder-Dialogfeld](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Führen Sie Einkauf

*CheckoutComplete.aspx* auf der Seite können Sie den Kauf von PayPal. Wie bereits erwähnt, muss der Benutzer klicken, auf die **vollständigen Order** Schaltfläche vor die Anwendung zu navigieren, wird die *CheckoutComplete.aspx* Seite.

1. In der *Auschecken* Ordner öffnen die Seite mit dem Namen *CheckoutComplete.aspx*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Öffnen Sie die CodeBehind-Seite mit dem Namen *CheckoutComplete.aspx.cs* und Ersetzen Sie den vorhandenen Code durch Folgendes:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Wenn die *CheckoutComplete.aspx* Seite geladen ist, die `DoCheckoutPayment` -Methode aufgerufen wird. Wie oben erwähnt: die `DoCheckoutPayment` Methode schließt den Kauf von der PayPal-testumgebung. Wenn PayPal den Kauf des Auftrags abgeschlossen ist die *CheckoutComplete.aspx* Seite zeigt eine Zahlungstransaktion `ID` an den Käufer.

### <a name="handle-cancel-purchase"></a>Handle "Abbrechen" Kauf

Wenn der Benutzer entscheidet, den Kaufvorgang zu stornieren, sie geleitet werden, die *CheckoutCancel.aspx* Seite, in dem sie sehen, dass ihre Reihenfolge abgebrochen wurde.

1. Öffnen Sie die Seite mit dem Namen *CheckoutCancel.aspx* in der *Auschecken* Ordner.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Kauf Fehler behandeln

Fehler während des Kaufvorgangs werden vom behandelt die *CheckoutError.aspx* Seite. Der Code-Behind von der *CheckoutStart.aspx* Seite der *CheckoutReview.aspx* Seite und die *CheckoutComplete.aspx* Seite jedes weitergeleitet wird, die * CheckoutError.aspx* Seite, wenn ein Fehler auftritt.

1. Öffnen Sie die Seite mit dem Namen *CheckoutError.aspx* in der *Auschecken* Ordner.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

Die *CheckoutError.aspx* Seite wird mit den Details der Fehlermeldung angezeigt, wenn während des Auscheckvorgangs ein Fehler auftritt.

## <a name="running-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung zu erfahren, wie Produkte zu kaufen. Beachten Sie, dass Sie in der PayPal ausführen, testen die Umgebung. Keine tatsächliche Money ist ausgetauscht wird.

1. Stellen Sie sicher, dass alle Ihre Dateien in Visual Studio gespeichert sind.
2. Öffnen Sie einen Webbrowser, und navigieren Sie zu [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Melden Sie sich mit Ihrem PayPal-Entwicklerkonto an, die Sie zuvor in diesem Lernprogramm erstellt haben.  
   Für den Sandkasten für PayPal Entwickler, müssen Sie am angemeldet sein [ https://developer.paypal.com ](https://developer.paypal.com/) express Auschecken zu testen. Dies gilt nur für PayPal Sandkasten, nicht für PayPal live-Umgebung testen.
4. Drücken Sie in Visual Studio **F5** zum Ausführen der Anwendung des Wingtip Toys-Beispiel.  
   Nachdem die Datenbank neu erstellt werden, der Browser öffnen und Anzeigen der *"default.aspx"* Seite.
5. Hinzufügen von drei verschiedenen Produkten zum Einkaufswagen von Product Category, z. B. "Cars" auswählen und dann auf **Add to Cart** neben jedes Produkt.  
   Der Einkaufswagen wird das Produkt angezeigt, das Sie ausgewählt haben.
6. Klicken Sie auf die **PayPal** Schaltfläche zur Kasse gehen. 

    ![Auschecken "und" Payment PayPal - Einkaufswagen](checkout-and-payment-with-paypal/_static/image20.png)

   Auschecken erfordern, dass Sie ein Benutzerkonto für die beispielanwendung des Wingtip Toys verfügen.
7. Klicken Sie auf die **Google** Link am rechten Rand der Seite mit einem vorhandenen gmail.com-e-Mail-Konto anmelden.  
   Wenn Sie nicht über ein gmail.com-Konto verfügen, können Sie eine für Testzwecke am erstellen [www.gmail.com](https://www.gmail.com/). Sie können auch ein standard lokales Konto verwenden, durch Klicken auf "Registrieren". 

    ![Auschecken "und" Payment PayPal - Anmeldung](checkout-and-payment-with-paypal/_static/image21.png)
8. Melden Sie sich mit Ihren Gmail-Konto und Ihr Kennwort. 

    ![Auschecken "und" Payment PayPal - Gmail anmelden](checkout-and-payment-with-paypal/_static/image22.png)
9. Klicken Sie auf die **melden Sie sich** Schaltfläche, um Ihr Konto Gmail durch Ihren Benutzernamen des Wingtip Toys Beispiel Anwendung registrieren. 

    ![Auschecken "und" Payment PayPal - Konto registrieren](checkout-and-payment-with-paypal/_static/image23.png)
10. PayPal-Website hinzufügen die **Käufer** e-Mail-Adresse und Kennwort, das Sie zuvor in diesem Lernprogramm erstellt haben, und klicken Sie auf die **anmelden** Schaltfläche. 

    ![Auschecken "und" Payment PayPal - PayPal-Anmeldung](checkout-and-payment-with-paypal/_static/image24.png)
11. Akzeptieren Sie die PayPal-Richtlinie, und klicken Sie auf die **zustimmen und Fortfahren** Schaltfläche.  
    Beachten Sie, dass diese Seite nur angezeigt, der ersten Verwendung dieses PayPal-Konto. Erneut Beachten Sie, dass dies ein Testkonto ist keine echte Money ausgetauscht wird. 

    ![Auschecken "und" Payment PayPal - PayPal-Richtlinie](checkout-and-payment-with-paypal/_static/image25.png)
12. Überprüfen Sie auf die PayPal testen Umgebung Seite "Überprüfen", und klicken Sie auf die Bestellinformationen **Fortfahren**. 

    ![Auschecken "und" Payment PayPal - Informationen überprüfen](checkout-and-payment-with-paypal/_static/image26.png)
13. Auf der *CheckoutReview.aspx* Seite, vergewissern Sie sich der Bestellwert und zeigen Sie die generierte Lieferadresse an. Klicken Sie auf die **vollständigen Order** Schaltfläche. 

    ![Auschecken "und" Payment PayPal - Reihenfolge überprüfen](checkout-and-payment-with-paypal/_static/image27.png)
14. Die **CheckoutComplete.aspx** Seite wird angezeigt, mit der eine Zahlung Transaktions-ID. 

    ![Auschecken "und" Payment PayPal - Auschecken abgeschlossen](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Überprüfen die Datenbank

Überprüfen Sie die aktualisierten Daten in der Anwendung des Wingtip Toys-Beispieldatenbank, nach dem Ausführen der Anwendung, sehen Sie sich, dass die Anwendung erfolgreich den Kauf der Produkte aufgezeichnet.

Überprüfen Sie die Daten in der *Wingtiptoys.mdf* Datenbankdatei mit der **Datenbank-Explorer** Fenster (**Server-Explorer** Fenster in Visual Studio) wie weiter oben in diesem Lernprogramm Reihe.

1. Schließen Sie das Browserfenster, wenn es noch geöffnet ist.
2. Wählen Sie in Visual Studio die **alle Dateien anzeigen** Symbol am oberen Rand **Projektmappen-Explorer** , die Ihnen ermöglichen, erweitern die **App\_Daten** Ordner.
3. Erweitern Sie die **App\_Daten** Ordner.  
 Sie müssen auswählen der **alle Dateien anzeigen** Symbol für den Ordner.
4. Mit der rechten Maustaste die *Wingtiptoys.mdf* Datenbankdatei, und wählen **öffnen**.  
    **Server-Explorer** wird angezeigt.
5. Erweitern Sie die **Tabellen** Ordner.
6. Mit der rechten Maustaste die **Aufträge**Tabelle, und wählen Sie **Tabellendaten anzeigen**.  
 Die **Aufträge** Tabelle wird angezeigt.
7. Überprüfen Sie die **PaymentTransactionID** Spalte erfolgreiche Transaktionen zu bestätigen. 

    ![Auschecken "und" Payment PayPal - Datenbank überprüfen](checkout-and-payment-with-paypal/_static/image29.png)
8. Schließen der **Aufträge** Tabellenfensters.
9. Im Server-Explorer mit der Maustaste die **OrderDetails** Tabelle, und wählen Sie **Tabellendaten anzeigen**.
10. Überprüfen Sie die `OrderId` und `Username` Werte in der **OrderDetails** Tabelle. Beachten Sie, die diese Werte entsprechen den `OrderId` und `Username` in enthaltenen Werte der **Aufträge** Tabelle.
11. Schließen der **OrderDetails** Tabellenfensters.
12. Mit der rechten Maustaste der Wingtip Toys-Datenbankdatei (*Wingtiptoys.mdf*), und wählen Sie **enge Verbindung**.
13. Wenn Sie nicht angezeigt werden die **Projektmappen-Explorer** Fenster, klicken Sie auf **Projektmappen-Explorer** am unteren Rand der **Server-Explorer** Fenster zum Anzeigen der **Projektmappen-Explorer ** erneut aus.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie Reihenfolge und Order Detail-Schemas, um den Kauf von Produkten nachverfolgen hinzugefügt. Sie werden auch PayPal-Funktionalität in der beispielanwendung des Wingtip Toys integriert.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Übersicht über die Konfiguration von ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Bereitstellen von Mitgliedschaft, OAuth und SQL-Datenbank eine sichere ASP.NET Web Forms-App in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - Testversion](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Haftungsausschluss

Dieses Lernprogramm enthält Beispielcode. Dieser Beispielcode wird "wie besehen" bereitgestellt, ohne jegliche Garantie bereitgestellt. Microsoft wird entsprechend der Genauigkeit, Integrität und Qualität des Beispielcodes nicht sichergestellt. Stimmen Sie den Beispielcode auf eigenes Risiko verwenden. Unter keinen Umständen werden Microsoft haftet Ihnen in keiner Weise alle Beispielcode, der Inhalt, einschließlich, aber nicht beschränkt auf, bei Fehlern oder auslassungen in den Beispielcode, Inhalt, oder Verlust oder Beschädigung jeglicher Art, die durch die Verwendung von Beispielcode entstehen. Hiermit werden benachrichtigt, und lehnen schliessen freistellen, speichern Sie, und halten Sie Microsoft gegen alle verloren gehen, die Ansprüche von Verlusten, verletzungs- oder Schäden jeder Art einschließlich ohne Einschränkung, die trägt oder solchen, die aus Material, das Sie bereitstellen, übertragen, verwenden, oder abhängig ist, einschließlich, aber nicht beschränkt auf die Ansichten, die darin enthaltenen ausgedrückt.

> [!div class="step-by-step"]
> [Zurück](shopping-cart.md)
> [Weiter](membership-and-administration.md)
