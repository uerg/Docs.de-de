---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Bezahlvorgang und Zahlung mit PayPal | Microsoft-Dokumentation
author: Erikre
description: Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für wir...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 3299da33a68f02ac1b3ffe7c037d06d8ece9455e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835630"
---
<a name="checkout-and-payment-with-paypal"></a>Bezahlvorgang und Zahlung mit PayPal
====================
durch [Erik Reitan](https://github.com/Erikre)

[Herunterladen der Wingtip Toys-Beispielprojekts (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) oder [E-Book (PDF) herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Diese lernprogrammreihe vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von ASP.NET 4.5 und Microsoft Visual Studio Express 2013 für Web. Eine Visual Studio 2013 [-Projekts mit C#-Quellcode](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ist verfügbar, die dieser tutorialreihe begleitet.


In diesem Tutorial wird beschrieben, wie so ändern Sie die Wingtip Toys-beispielanwendung für die Benutzerauthentifizierung, Registrierung und Zahlung mit PayPal enthalten wird. Nur Benutzer, die angemeldet sind, müssen Autorisierung Produkte zu kaufen. Die ASP.NET 4.5 Web Forms-Projektvorlage integriertes Benutzer-registrierungsfunktionalität enthält bereits viel, was Sie benötigen. Fügen Sie PayPal-Express-Checkout-Funktionalität. In diesem Tutorial werden Verwendung der PayPal-Entwickler, die testumgebung, damit keine tatsächlichen Betrag übertragen werden. Am Ende des Tutorials testen Sie die Anwendung durch Auswählen der Produkte aus, um den Warenkorb, klicken Sie auf die Schaltfläche zum Auschecken und Übertragen von Daten in der PayPal-testen-Website hinzufügen. Auf der Website der PayPal-Test Sie bestätigen Sie Ihre Zahlung und Versand Daten und dann zurück, mit der lokalen Wingtip Toys-beispielanwendung, um zu bestätigen und den Kauf abzuschließen.

Es gibt mehrere erfahrene Drittanbieter-Payment-Prozessoren, die spezialisiert im online-shopping diese Adresse Skalierbarkeit und Sicherheit. Entwickler von ASP.NET-ANWENDUNGEN sollten die Vorteile der Lösung eines Drittanbieters Zahlung vor dem Implementieren Warenkorb und Erwerb von Projektmappen nutzen.

> [!NOTE] 
> 
> Die Wingtip Toys-beispielanwendung entworfen bestimmte ASP.NET-Konzepte und Features verfügbar, ASP.NET Web-Entwickler angezeigt wurde. Diese Anwendung wurde allen möglichen Situationen, in Bezug auf Skalierbarkeit und Sicherheit nicht optimiert.


## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- Informationen zum Zugriff auf bestimmte Seiten in einem Ordner zu beschränken.
- Vorgehensweise: Erstellen von einem bekannten Einkaufswagen aus einer anonymen Warenkorb.
- Wie Sie SSL für das Projekt zu aktivieren.
- Wie Sie einen OAuth-Anbieter zum Projekt hinzufügen.
- Wie Sie mit PayPal, zum Erwerb von Produkten mithilfe der PayPal-testumgebung.
- Vorgehensweise beim Anzeigen von Details aus PayPal in einem **DetailsView** Steuerelement.
- So aktualisieren Sie die Datenbank der Anwendung Wingtip Toys mit Details, die vom PayPal abgerufen werden.

## <a name="adding-order-tracking"></a>Hinzufügen von Order-nachverfolgung

In diesem Tutorial erstellen Sie zwei neue Klassen zum Nachverfolgen von Daten von der Reihenfolge, die ein Benutzer erstellt hat. Die Klassen werden Daten im Zusammenhang mit Versandinformationen Kauf gesamt und zahlungsbestätigung nachverfolgt.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Fügen Sie die Reihenfolge und OrderDetail-ViewModel-Klassen

Weiter oben in dieser tutorialreihe, die Sie definiert des Schemas für Kategorien, Produkte und Warenkorb Elemente durch das Erstellen der `Category`, `Product`, und `CartItem` Klassen in der *Modelle* Ordner. Fügen Sie nun zwei neue Klassen zum Definieren des Schemas für die Product-Reihenfolge und die Details der Bestellung.

1. In der **Modelle** Ordner, fügen Sie eine neue Klasse mit dem Namen *Order.cs*.   
   Die neue Klassendatei wird im Editor angezeigt.
2. Ersetzen Sie den Standardcode durch Folgendes:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Hinzufügen einer *OrderDetail.cs* -Klasse auf die *Modelle* Ordner.
4. Ersetzen Sie den Standardcode durch folgenden Code:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Die `Order` und `OrderDetail` Klassen enthalten, das Schema zum definieren, der Bestellinformationen zum Kauf und versenden.

Darüber hinaus müssen Sie den Datenbankkontext-Klasse zu aktualisieren, verwaltet die Entitätsklassen und den Datenzugriff auf die Datenbank bereitstellt. Zu diesem Zweck fügen Sie den neu erstellten Auftrag und `OrderDetail` Modellklassen zu `ProductContext` Klasse.

1. In **Projektmappen-Explorer**suchen oder öffnen Sie die *ProductContext.cs* Datei.
2. Fügen Sie den hervorgehobenen Code die *ProductContext.cs* Datei wie unten dargestellt:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Wie bereits erwähnt in dieser tutorialreihe, den Code in die *ProductContext.cs* Datei fügt die `System.Data.Entity` Namespace, damit Sie Zugriff auf die Kernfunktionen des Entity Framework haben. Diese Funktionen umfassen die Möglichkeit, Abfragen, einfügen, aktualisieren und Löschen von Daten durch Verwendung stark typisierter Objekte. Der obige Code in die `ProductContext` Klasse fügt Entity Framework Zugriff auf die neu hinzugefügte `Order` und `OrderDetail` Klassen.

## <a name="adding-checkout-access"></a>Hinzufügen von Checkout-Zugriff

Die Wingtip Toys-beispielanwendung kann anonyme Benutzer überprüfen und Produkten zum Einkaufswagen hinzufügen. Jedoch, wenn anonyme Benutzer entscheiden, die die Produkte zu kaufen, die sie dem Einkaufswagen hinzugefügt, müssen sie auf der Website anmelden. Sobald sie angemeldet haben, können sie die eingeschränkten Seiten der Webanwendung zugreifen, die das Auschecken behandeln und Prozess erwerben. Diese eingeschränkten Seiten befinden sich die *Auschecken* Ordner der Anwendung.

### <a name="add-a-checkout-folder-and-pages"></a>Fügen Sie einem Auscheckordner und Seiten hinzu

Erstellen Sie jetzt die *Auschecken* Ordner und die Seiten, die der Kunde während des Auscheckvorgangs angezeigt wird. Weiter unten in diesem Tutorial aktualisieren Sie diese Seiten.

1. Mit der rechten Maustaste in des Namens des Projekts (**Wingtip Toys**) in **Projektmappen-Explorer** , und wählen Sie **Hinzufügen eines neuen Ordners**. 

    ![Bezahlvorgang und Zahlung mit PayPal - neuen Ordner](checkout-and-payment-with-paypal/_static/image1.png)
2. Nennen Sie diesen Ordner *Auschecken*.
3. Mit der rechten Maustaste die *Auschecken* Ordner, und wählen Sie dann **hinzufügen**-&gt;**neues Element**. 

    ![Bezahlvorgang und Zahlung mit PayPal - neues Element](checkout-and-payment-with-paypal/_static/image2.png)
4. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
5. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie dann im mittleren Bereich **Webformular mit Gestaltungsvorlage**und nennen Sie sie *CheckoutStart.aspx*. 

    ![Fügen Sie Bezahlvorgang und Zahlung mit PayPal - Dialogfeld "Neues Element" hinzu.](checkout-and-payment-with-paypal/_static/image3.png)
6. Wählen Sie wie zuvor die *Site.Master* Datei als Masterseite.
7. Fügen Sie die folgenden zusätzlichen Seiten, um die *Auschecken* Ordner mithilfe der oben genannten Schritte:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Fügen Sie eine Datei "Web.config"

Durch Hinzufügen eines neuen *"Web.config"* -Datei in die *Auschecken* Ordner, Sie werden den Zugriff auf alle Seiten im Ordner enthaltenen einschränken.

1. Mit der rechten Maustaste die *Auschecken* Ordner, und wählen **hinzufügen**  - &gt; **neues Element**.  
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Wählen Sie die **Visual C#-**  - &gt; **Web** Gruppe "Vorlagen" auf der linken Seite. Wählen Sie dann im mittleren Bereich **Webkonfigurationsdatei**, akzeptieren Sie den Namen der *"Web.config"*, und wählen Sie dann **hinzufügen**.
3. Ersetzen Sie den vorhandenen XML-Inhalt in die *"Web.config"* Datei durch Folgendes:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Speichern Sie die *"Web.config"* Datei.

Die *"Web.config"* legt fest, dass es sich bei allen unbekannte Benutzern der Webanwendung den Zugriff auf den Seiten, die in enthaltenen verweigert werden, müssen die *Auschecken* Ordner. Allerdings, wenn der Benutzer verfügt über ein Konto registriert und angemeldet ist, sie werden einen bekannten Benutzer und haben Zugriff auf die Seiten in der *Auschecken* Ordner.

Es ist wichtig zu beachten, dass ASP.NET-Konfiguration eine Hierarchie folgt, in dem jede *"Web.config"* Konfigurationseinstellungen Datei angewendet werden, zu dem Ordner, die in und alle untergeordneten Verzeichnisse darunter.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Aktivieren von SSL für das Projekt

 Secure Sockets Layer (SSL) ist ein Protokoll, um Webservern und Webclients, sicherer Kommunikation durch Verschlüsselung zu ermöglichen. Wenn SSL nicht verwendet wird, ist zwischen Client und Server gesendete Daten Paket-sniffing von jedem mit physischem Zugriff auf das Netzwerk geöffnet. Darüber hinaus sind verschiedene häufig verwendete Authentifizierungsschemas nicht über einfaches HTTP sicher. Insbesondere senden die einfache Authentifizierung und Formularauthentifizierung unverschlüsselte Anmeldeinformationen. Um sicher zu sein, müssen diese Authentifizierungsschemas SSL verwenden. 

1. In **Projektmappen-Explorer**, klicken Sie auf die **WingtipToys** Projekt, und drücken Sie **F4** zum Anzeigen der **Eigenschaften** Fenster.
2. Änderung **SSL-fähigen** zu `true`.
3. Kopieren der **SSL-URL** , damit Sie sie später verwenden können.   
 Die SSL-URL `https://localhost:44300/` , sofern Sie zuvor SSL-Websites erstellt haben (wie unten gezeigt).   
    ![Projekteigenschaften](checkout-and-payment-with-paypal/_static/image4.png)
4. In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die **WingtipToys** Projekt, und klicken Sie auf **Eigenschaften**.
5. Klicken Sie auf der linken Registerkarte auf **Web**.
6. Ändern der **Projekt-Url** verwenden die **SSL-URL** , die Sie zuvor gespeichert haben.   
    ![Projekt-Webeigenschaften](checkout-and-payment-with-paypal/_static/image5.png)
7. Speichern Sie die Seite durch Drücken von **STRG + S**.
8. Drücken Sie **STRG+F5**, um die Anwendung auszuführen. Visual Studio zeigt eine Option aus, um die Ihnen ermöglichen, die SSL-Warnungen zu vermeiden.
9. Klicken Sie auf **Ja** auf dem IIS Express SSL-Zertifikat vertrauen und fortzufahren.   
    ![IIS Express SSL-Zertifikatdetails](checkout-and-payment-with-paypal/_static/image6.png)  
 Es wird eine Sicherheitswarnung angezeigt.
10. Klicken Sie auf **Ja** , das Zertifikat zu Ihrem Localhost zu installieren.   
    ![Sicherheit Warnung (Dialogfeld)](checkout-and-payment-with-paypal/_static/image7.png)  
 Das Browserfenster wird angezeigt.

Sie können jetzt ganz einfach auf Ihre Webanwendung lokal mit SSL testen.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Hinzufügen eines OAuth 2.0-Anbieters

ASP.NET Web Forms bietet erweiterte Optionen für Mitgliedschaft und Authentifizierung. Diese Erweiterungen umfassen OAuth. OAuth ist ein offenes Protokoll, das sichere Autorisierung in einer einfachen und Standardmethode von Web-, Desktop- und mobile Anwendungen ermöglicht. Die ASP.NET Web Forms-Vorlage verwendet OAuth, Facebook, Twitter, Google und Microsoft als Authentifizierungsanbieter verfügbar machen. Obwohl in diesem Lernprogramm nur Google als Authentifizierungsanbieter eingesetzt wird, können Sie einfach den Code, um einen der Anbieter verwenden, ändern. Die Schritte zur Implementierung anderer Anbieter sind sehr ähnlich, mit den Schritten, die Sie in diesem Lernprogramm sehen werden.

Zusätzlich zur Authentifizierung wird das Lernprogramm auch Rollen verwendet, um die Autorisierung zu implementieren. Nur die Benutzer, die Sie hinzufügen, die `canEdit` Rolle wird in der Lage, Daten zu ändern (erstellen, bearbeiten oder Löschen von Kontakten).

> [!NOTE] 
> 
> Windows Live-Anwendungen akzeptieren nur eine live-URL für eine Arbeitswebsite, damit Sie eine lokale Website-URL für das Testen von Anmeldungen nicht verwenden können.


Die folgenden Schritte können Sie einen Google-Authentifizierungsanbieter hinzufügen.

1. Öffnen der *App\_Start\Startup.Auth.cs* Datei.
2. Entfernen Sie die Kommentarzeichen aus der `app.UseGoogleAuthentication()` Methode, damit die Methode angezeigt wird, wie folgt: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Navigieren Sie zu der [Google-Entwicklerkonsole](https://console.developers.google.com/). Sie müssen auch mit Ihrem Google Entwickler-e-Mail-Konto (gmail.com) anmelden. Wenn Sie nicht über ein Google-Konto verfügen, wählen Sie die **erstellen Sie ein Konto** Link.   
   Als Nächstes sehen Sie die **Google Developers Console**.   
    ![Google-Entwicklerkonsole](checkout-and-payment-with-paypal/_static/image8.png)
4. Klicken Sie auf die **Erstellen eines Projekts** und geben Sie einen Projektnamen und -ID (Sie können die Standardwerte verwenden). Klicken Sie auf die **Vereinbarung Kontrollkästchen** und **erstellen** Schaltfläche.  

    ![Google - neues Projekt](checkout-and-payment-with-paypal/_static/image9.png)

   In wenigen Sekunden wird das neue Projekt erstellt werden, und Ihr Browser zeigt die Seite für neue Projekte.
5. Klicken Sie auf der linken Registerkarte auf **APIs &amp; Auth**, und klicken Sie dann auf **Anmeldeinformationen**.
6. Klicken Sie auf die **erstellen Sie neue Client-ID** unter **OAuth**.   
   Die **Client-ID erstellen** Dialogfeld wird angezeigt.   
    ![Google - Client-ID erstellen](checkout-and-payment-with-paypal/_static/image10.png)
7. In der **Client-ID erstellen** Dialogfeld behalten Sie den Standardwert **Webanwendung** für den Anwendungstyp.
8. Legen Sie die **autorisierte JavaScript-Ursprünge** , die Sie zuvor in diesem Tutorial verwendete SSL-URL (`https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben).   
   Diese URL ist der Ursprung für Ihre Anwendung. In diesem Beispiel werden Sie nur die "localhost" Test-URL eingeben. Allerdings können Sie mehrere URLs um "localhost" und die Produktion berücksichtigen eingeben.
9. Legen Sie die **Authorized Redirect URI** folgt: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Dieser Wert ist der URI, den ASP.NET OAuth-Benutzer mit dem Google-OAuth-Server kommunizieren. Beachten Sie die oben verwendeten SSL-URL ( `https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben).
10. Klicken Sie auf die **Client-ID erstellen** Schaltfläche.
11. Klicken Sie auf der Google-Entwicklerkonsole im linken Menü auf der **zustimmungsbildschirm** Menüelement, legen Sie dann auf der e-Mail-Adresse und Product Name. Wenn Sie das Formular ausgefüllt haben, klicken Sie auf **speichern**.
12. Klicken Sie auf die **APIs** Menüelement, führen Sie einen Bildlauf nach unten, und klicken Sie auf die **aus** neben **Google + API**.   
    Akzeptieren diese Option ermöglicht das Google +-API.
13. Sie müssen auch aktualisieren die **Microsoft.Owin** NuGet-Paket in Version 3.0.0.   
    Von der **Tools** , wählen Sie im Menü **NuGet Package Manager** und wählen Sie dann **NuGet-Pakete für Projektmappe verwalten**.  
    Von der **NuGet-Pakete verwalten** Fenster Suchen und Aktualisieren der **Microsoft.Owin** -Paket zu Version 3.0.0.
14. Aktualisieren Sie in Visual Studio die `UseGoogleAuthentication` -Methode der der *Startup.Auth.cs* Seite durch Kopieren und Einfügen der **Client-ID** und **Clientgeheimnis** an die Methode. Die **Client-ID** und **Clientgeheimnis** unten aufgeführten Werte sind Beispiele und funktionieren nicht. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Drücken Sie **STRG + F5** erstellen und Ausführen der Anwendungs. Klicken Sie auf die **melden Sie sich bei** Link.
16. Klicken Sie unter **verwenden Sie einen anderen Dienst anmelden**, klicken Sie auf **Google**.  
    ![Anmelden](checkout-and-payment-with-paypal/_static/image11.png)
17. Wenn Sie Ihre Anmeldeinformationen eingeben müssen, werden Sie zur Google-Website weitergeleitet, in dem Sie Ihre Anmeldeinformationen eingeben.  
    ![Google - Anmeldung](checkout-and-payment-with-paypal/_static/image12.png)
18. Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, werden Sie aufgefordert werden, so erteilen Berechtigungen für die Webanwendung, die Sie gerade erstellt haben.  
    ![Standarddienstkonto, das Projekt](checkout-and-payment-with-paypal/_static/image13.png)
19. Klicken Sie auf **akzeptieren**. Jetzt werden Sie umgeleitet an die **registrieren** auf der Seite die **WingtipToys** Anwendung, wo Sie Ihr Google-Konto registrieren.  
    ![Mit Ihrem Google-Konto registrieren](checkout-and-payment-with-paypal/_static/image14.png)
20. Sie haben die Möglichkeit, den Namen lokaler e-Mail-Registrierung für Ihr Gmail-Konto zu ändern, aber Sie möchten in der Regel behalten Sie das Standard-e-Mail-Alias (d. h. diejenige, die Sie für die Authentifizierung verwendet). Klicken Sie auf **melden Sie sich bei** wie oben gezeigt.

### <a name="modifying-login-functionality"></a>Ändern die Anmeldefunktionalität ermöglichen

Wie bereits in dieser tutorialreihe erwähnt ist ein Großteil der Funktionalität für die Registrierung in der ASP.NET Web Forms-Vorlage standardmäßig enthalten. Nun ändern Sie die Standardeinstellung *"Login.aspx"* und *Register.aspx* Seiten zum Aufrufen der `MigrateCart` Methode. Die `MigrateCart` Methode ordnet einen neu angemeldeten Benutzer ein anonymer Warenkorb. Durch den Benutzer zuordnen und Warenkorb, wird die Wingtip Toys-beispielanwendung den Einkaufswagen des Benutzers für Abonnenten‟ verwalten können.

1. In **Projektmappen-Explorer**suchen oder öffnen Sie die *Konto* Ordner.
2. Ändern Sie die CodeBehind-Seite, die mit dem Namen *Login.aspx.cs* gelb hervorgehobenen Code einschließen, damit es wie folgt aussieht:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Speichern Sie die *Login.aspx.cs* Datei.

Jetzt können Sie ignorieren Sie die Warnung, die es keine Definition für ist die `MigrateCart` Methode. Sie werden ihn später in diesem Tutorial hinzugefügt werden.

Die *Login.aspx.cs* Code-Behind-Datei unterstützt eine LogIn-Methode. Durch Überprüfen der Seite "Login.aspx" an, wie Sie sehen, diese Seite eine Schaltfläche "Anmelden" enthält, die bei Klicken Sie auf die Trigger die `LogIn` angegebenen Ereignishandler für das Code-Behind.

Wenn die `Login` Methode für die *Login.aspx.cs* aufgerufen wird, wird eine neue Instanz der mit dem Namen Einkaufswagen `usersShoppingCart` wird erstellt. Die ID des Warenkorbs (eine GUID) wird abgerufen, und legen Sie auf die `cartId` Variable. Anschließend wird die `MigrateCart` -Methode aufgerufen wird, übergibt sowohl die `cartId` und den Namen des angemeldeten Benutzers an diese Methode. Wenn der Einkaufswagen migriert wird, wird die GUID, die zum Identifizieren des anonymen Einkaufswagens mit dem Benutzernamen ersetzt.

Zusätzlich zum Ändern der *Login.aspx.cs* Code-Behind-Datei zum Einkaufswagen ordnungsgemäß migriert werden, wenn der Benutzer anmeldet, müssen Sie auch ändern der *Register.aspx.cs Code-Behind-Datei* zum Migrieren des Einkaufswagens Wenn der Benutzer erstellt ein neues Konto und Anmeldung.

1. In der *Konto* Ordner Code-Behind-Datei mit dem Namen *Register.aspx.cs*.
2. Ändern Sie den Code-Behind-Datei durch den Code in Gelb, einschließlich, damit es wie folgt aussieht:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Speichern Sie die *Register.aspx.cs* Datei. Ignorieren Sie die Warnung auch hier über die `MigrateCart` Methode.

Beachten Sie, dass Sie den Code in verwendet die `CreateUser_Click` -Ereignishandler ist sehr ähnlich wie der Code, der Sie verwendet haben, in der `LogIn` Methode. Wenn der Benutzer registriert oder die Anmeldung bei der Website, die einen Aufruf der `MigrateCart` Methode erfolgen.

## <a name="migrating-the-shopping-cart"></a>Migrieren des Einkaufswagens

Nun, da Sie die Anmeldung und Registrierung Prozess aktualisiert haben, können Sie den Code, um die shopping Cart mit migrieren Hinzufügen der `MigrateCart` Methode.

1. In **Projektmappen-Explorer**, finden Sie die *Logik* Ordner, und Öffnen der *ShoppingCartActions.cs* Klassendatei.
2. Fügen Sie den Code in Gelb zu den vorhandenen Code in markiert die *ShoppingCartActions.cs* -Datei, damit der Code in die *ShoppingCartActions.cs* Datei sieht wie folgt:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

Die `MigrateCart` Methode verwendet die vorhandene CartId, um den Einkaufswagen des Benutzers zu suchen. Als Nächstes der Code durchläuft alle dem Artikel im Warenkorb und ersetzt die `CartId` Eigenschaft (gemäß der `CartItem` Schema) mit dem Namen des angemeldeten Benutzers.

### <a name="updating-the-database-connection"></a>Aktualisieren die Datenbankverbindung

Wenn Sie dieses Tutorial auch mit der **vorgefertigte** Wingtip Toys-beispielanwendung, müssen Sie die Standarddatenbank für die Mitgliedschaft neu erstellen. Ändern Sie die Standardverbindungszeichenfolge, wird die Mitgliedschaftsdatenbank das nächste Mal erstellt die Anwendung ausgeführt wird.

1. Öffnen der *"Web.config"* Datei im Stammverzeichnis des Projekts.
2. Aktualisieren Sie die Standardverbindungszeichenfolge, sodass sie wie folgt aussieht:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Die Integration von PayPal

PayPal ist eine webbasierte Abrechnung-Plattform, die Zahlungen von online-Händlern akzeptiert. In diesem Tutorial erläutert als Nächstes das PayPal Express-Checkout-Funktionalität in Ihre Anwendung integrieren. Express Auschecken kann Ihre Kunden PayPal zu verwenden, um die Elemente bezahlen, die sie zu ihrem Einkaufswagen hinzugefügt haben.

### <a name="create-paylpal-test-accounts"></a>Erstellen von PaylPal Testkonten

Um das PayPal-testumgebung zu verwenden, müssen Sie erstellen und überprüfen Sie ein Entwicklerkonto für den Test. Sie werden die Developer-Testkonto verwenden, um ein Käufer Testen der Konten- und ein Verkäufer-Testkonto zu erstellen. Die Anmeldeinformationen des Testkontos Entwickler ermöglichen außerdem die Wingtip Toys-beispielanwendung auf die PayPal-Test-Umgebung zuzugreifen.

1. Navigieren Sie in einem Browser zu der PayPal-Entwickler, die Website testen:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Wenn Sie nicht über ein PayPal-Developer-Konto verfügen, erstellen Sie ein neues Konto, indem Sie auf **registrieren**und registrieren Sie sich Schritte befolgen. Wenn Sie ein vorhandenes PayPal-Developer-Konto verfügen, melden Sie sich, indem Sie auf **anmelden**. Sie benötigen Ihre PayPal-Developer-Konto auf die Wingtip Toys-beispielanwendung später in diesem Tutorial zu testen.
3. Wenn Sie nur für Ihre PayPal-Developer-Konto registriert haben, müssen Sie möglicherweise Ihrer PayPal-Developer-Konto mit PayPal zu überprüfen. Sie können Ihr Konto überprüfen, indem Sie die Schritte, die PayPal an Ihr e-Mail-Konto gesendet. Nachdem Sie Ihre PayPal-Developer-Konto überprüft haben, melden Sie sich wieder bei der PayPal-Entwickler, die Website testen.
4. Nachdem Sie angemeldet sind der PayPal-Entwicklerwebsite mit Ihrer PayPal-Developer-Konto benötigen Sie ein PayPal Käufer-Testkonto zu erstellen, wenn Sie noch keine besitzen eines. Erstellen Sie ein Testkonto für Käufer, klicken Sie auf der PayPal-Website auf der **Anwendungen** Registerkarte, und klicken Sie dann auf **sandboxkonten**.   
 Die **Test sandboxkonten** Seite wird angezeigt.   

    > [!NOTE] 
    > 
    > Der PayPal-Entwicklerwebsite enthält bereits ein Händler Testkonto.

    ![Bezahlvorgang und Zahlung mit PayPal - Sandbox-Testkonten](checkout-and-payment-with-paypal/_static/image15.png)
5. Klicken Sie auf die Sandbox-Test-Kontoseite auf **Kontoerstellung**.
6. Auf der **Testkonto erstellen** auszuwählen, die ein Käufer Test e-Mail-Adresse und ein Kennwort Ihrer Wahl.   

    > [!NOTE] 
    > 
    > Sie benötigen den Käufer e-Mail-Adressen und das Kennwort am Ende dieses Tutorials die Wingtip Toys-beispielanwendung zu testen.

    ![Bezahlvorgang und Zahlung mit PayPal - Sandbox-Testkonten](checkout-and-payment-with-paypal/_static/image16.png)
7. Käufer Testkonto erstellen, indem Sie auf die **Kontoerstellung** Schaltfläche.  
 Die **Sandbox Testkonten** angezeigt wird. 

    ![Bezahlvorgang und Zahlung mit PayPal - PaylPal Konten](checkout-and-payment-with-paypal/_static/image17.png)
8. Auf der **Test sandboxkonten** klicken Sie auf die **vermittlermitglied** e-Mail-Konto.  
    **Profil** und **Benachrichtigung** Optionen angezeigt.
9. Wählen Sie die **Profil** option, und klicken Sie dann **-API-Anmeldeinformationen** Ihre API-Anmeldeinformationen für die Händler Testkonto an.
10. Kopieren Sie die WEBLEISTUNGSTEST-API-Anmeldeinformationen in den Editor ein.

Sie benötigen Ihre angezeigten klassischen WEBLEISTUNGSTEST-API-Anmeldeinformationen (Benutzername, Kennwort und Signatur), API-Aufrufe aus der Wingtip Toys-beispielanwendung an das PayPal-testumgebung. Im nächsten Schritt fügen Sie die Anmeldeinformationen.

### <a name="add-paypal-class-and-api-credentials"></a>Hinzufügen von PayPal-Klasse und API-Anmeldeinformationen

Setzen Sie den Großteil der PayPal-Code in einer einzelnen Klasse. Diese Klasse enthält die Methoden, die zur Kommunikation mit PayPal verwendet. Darüber hinaus fügen Sie Ihrer PayPal-Anmeldeinformationen für diese Klasse hinzu.

1. In der Wingtip Toys-beispielanwendung in Visual Studio mit der Maustaste der **Logik** Ordner, und wählen Sie dann **hinzufügen**  - &gt; **neues Element**.   
   Das Dialogfeld **Neues Element hinzufügen** wird angezeigt.
2. Klicken Sie unter **Visual C#-** aus der **installiert** Bereich auf der linken Seite auf **Code**.
3. Wählen Sie im mittleren Bereich **Klasse**. Nennen Sie diese neue Klasse **PayPalFunctions.cs**.
4. Klicken Sie auf **Hinzufügen**.  
   Die neue Klassendatei wird im Editor angezeigt.
5. Ersetzen Sie den Standardcode durch folgenden Code:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Fügen Sie die Händler-API-Anmeldeinformationen (Benutzername, Kennwort und Signatur), die Sie zuvor in diesem Tutorial angezeigt, sodass Sie Funktionsaufrufe von der PayPal-testumgebung vornehmen können.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> In dieser beispielanwendung werden Sie einfach die Anmeldeinformationen in einer C#-Datei (cs) hinzugefügt. Allerdings in einer Lösung implementierten sollten Sie Ihre Anmeldeinformationen in einer Konfigurationsdatei verschlüsseln.


Die NVPAPICaller-Klasse enthält den Großteil der PayPal-Funktionen. Der Code in der Klasse enthält die Methoden zum Erstellen ein Tests, die von der PayPal-testumgebung erwerben. Die folgenden drei PayPal-Funktionen werden verwendet, um Einkäufe zu tätigen:

- `SetExpressCheckout` Funktion
- `GetExpressCheckoutDetails` Funktion
- `DoExpressCheckoutPayment` Funktion

Die `ShortcutExpressCheckout` Methode erfasst die Informationen und Product Details der Test-Bestellung aus dem Einkaufswagen und Aufrufe der `SetExpressCheckout` PayPal-Funktion. Die `GetCheckoutDetails` Methode bestätigt Kaufdetails und ruft die `GetExpressCheckoutDetails` PayPal-Funktion vor dem Erwerb von Tests. Die `DoCheckoutPayment` Methode den Kauf Test aus der testumgebung führt Sie durch Aufrufen der `DoExpressCheckoutPayment` PayPal-Funktion. Der restliche Code unterstützt die PayPal-Methoden und der Prozess, z. B. die Codierung von Zeichenfolgen, Decodieren von Zeichenfolgen, Arrays zu verarbeiten und ermitteln die Anmeldeinformationen an.

> [!NOTE] 
> 
> PayPal können Sie optional Kaufdetails basierend auf enthalten [PayPal API-Spezifikation](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Erweitern den Code in der Wingtip Toys-beispielanwendung, können Sie Details der Lokalisierung, produktbeschreibungen, steuern, eine Kundennummer des Diensts sowie viele andere optionale Felder hinzufügen.


Beachten Sie, dass die Rückgabe- und "Abbrechen" URLs, die im angegebenen die **ShortcutExpressCheckout** Methode verwenden, eine Portnummer an.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Wenn Visual Web Developer ein Webprojekt über SSL ausgeführt wird, wird der Port 44300 häufig für den Webserver verwendet werden. Wie oben gezeigt, ist die Nummer des Ports 44300 an. Wenn Sie die Anwendung ausführen, können Sie eine andere Portnummer finden Sie unter. Ihrer Port Number muss ordnungsgemäß im Code festgelegt werden, damit können Sie erfolgreich ausführen der Wingtip Toys-beispielanwendung am Ende dieses Tutorials. Im nächsten Abschnitt in diesem Tutorial wird erläutert, wie Sie die Portnummer für den lokalen Host abrufen und aktualisieren Sie die PayPal-Klasse.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aktualisieren Sie die Portnummer für "localhost" in der PayPal-Klasse

Die Wingtip Toys-beispielanwendung Käufe Produkte testen PayPal-Website navigieren und an Ihre lokale Instanz von das Wingtip Toys-beispielanwendung zurückgegeben wird. Um zurück auf die richtige URL PayPal haben, müssen Sie angeben der Portnummer der lokalen Ausführung-beispielanwendung in der oben genannten PayPal-Code.

1. Mit der rechten Maustaste in des Namens des Projekts (**WingtipToys**) in **Projektmappen-Explorer** , und wählen Sie **Eigenschaften**.
2. Wählen Sie in der linken Spalte der **Web** Registerkarte.
3. Rufen Sie die Portnummer der **Projekt-Url** Feld.
4. Aktualisieren Sie ggf. die `returnURL` und `cancelURL` in der PayPal-Klasse (`NVPAPICaller`) in der *PayPalFunctions.cs* Datei, die die Portnummer Ihrer Webanwendung verwenden:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Jetzt wird der Code, den Sie hinzugefügt haben, dem erwarteten Port für Ihre lokale Web-Anwendung entsprechen. PayPal werden kann auf die richtige URL auf dem lokalen Computer zurückgegeben.

### <a name="add-the-paypal-checkout-button"></a>Die PayPal-Checkout-Schaltfläche "hinzufügen"

Nun, da die beispielanwendung die Hauptfunktionen von PayPal hinzugefügt wurden, können Sie beginnen, Hinzufügen von Markup und Code erforderlich, um diese Funktionen aufrufen. Zunächst müssen Sie die Schaltfläche zum Auschecken hinzufügen, die der Benutzer auf die Warenkorb-Seite angezeigt wird.

1. Öffnen der *ShoppingCart.aspx* Datei.
2. Führen Sie einen Bildlauf zum unteren Rand der Datei, und suchen die `<!--Checkout Placeholder -->` Kommentar.
3. Ersetzen Sie den Kommentar mit einer `ImageButton` steuern, damit das Markup wie folgt ersetzt wird:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. In der *ShoppingCart.aspx.cs* -Datei, nachdem die `UpdateBtn_Click` Ereignishandler am Ende der Datei hinzufügen die `CheckOutBtn_Click` -Ereignishandler:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Auch in der *ShoppingCart.aspx.cs* Datei, fügen einen Verweis auf die `CheckoutBtn`, sodass die neue Schaltfläche "Bild" wie folgt auf die verwiesen wird:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Speichern Sie die Änderungen sowohl die *ShoppingCart.aspx* Datei und die *ShoppingCart.aspx.cs* Datei.
7. Wählen Sie im Menü **Debuggen**-&gt;**erstellen WingtipToys**.  
   Das Projekt wird neu erstellt werden mit der neu hinzugefügten **ImageButton** Steuerelement.

### <a name="send-purchase-details-to-paypal"></a>Kaufdetails an PayPal senden

Klickt der Benutzer die **Auschecken** auf die Warenkorb-Seite auf die Schaltfläche (*ShoppingCart.aspx*), sie beginnen den Kaufvorgang fort. Der folgende Code Ruft die erste PayPal-Funktion erforderlich, um Produkte kaufen.

1. Von der *Auschecken* Ordner Code-Behind-Datei mit dem Namen *CheckoutStart.aspx.cs*.   
   Achten Sie darauf, dass Sie die CodeBehind-Datei zu öffnen.
2. Ersetzen Sie den vorhandenen Code durch folgenden Code:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Klickt der Benutzer der Anwendung die **Auschecken** auf die Warenkorb-Seite, den Browser auf die Schaltfläche wird, navigieren Sie zu der *CheckoutStart.aspx* Seite. Wenn die *CheckoutStart.aspx* Seite geladen wird, die `ShortcutExpressCheckout` Methode wird aufgerufen. An diesem Punkt wird der Benutzer auf der Website der PayPal-Tests übertragen. Auf der PayPal-Website der Benutzer gibt seine PayPal-Anmeldeinformationen, die Details der Bestellung überprüft, akzeptiert die PayPal-Vereinbarung und gibt Sie zurück zur Wingtip Toys-beispielanwendung, in denen die `ShortcutExpressCheckout` Methode abgeschlossen wird. Wenn die `ShortcutExpressCheckout` Methode abgeschlossen ist, wird er leitet den Benutzer auf die *CheckoutReview.aspx* im angegebene Seite die `ShortcutExpressCheckout` Methode. Dadurch kann der Benutzer, überprüfen die Auftragsdetails in der Wingtip Toys-beispielanwendung aus.

### <a name="review-order-details"></a>Überprüfen Sie Auftragsdetails

Nach Rückgabe von PayPal, die *CheckoutReview.aspx* auf der Seite die Wingtip Toys-beispielanwendung Bestelldetails werden angezeigt. Auf dieser Seite können den Benutzer, den Details der Bestellung zu überprüfen, bevor Sie die Produkte erwerben. Die *CheckoutReview.aspx* Seite muss wie folgt erstellt werden:

1. In der *Auschecken* Ordner die Seite mit dem Namen *CheckoutReview.aspx*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Öffnen Sie die CodeBehind-Seite, die mit dem Namen *CheckoutReview.aspx.cs* und Ersetzen Sie den vorhandenen Code durch Folgendes:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Die **DetailsView** Steuerelement wird verwendet, um die Auftragsdetails anzuzeigen, die vom PayPal zurückgegeben wurden. Der obige Code speichert außerdem die Auftragsdetails die Wingtip Toys-Datenbank als eine `OrderDetail` Objekt. Wenn der Benutzer klickt auf die **vollständigen Order** Schaltfläche, sie werden zur der *CheckoutComplete.aspx* Seite.

> [!NOTE] 
> 
> **Tipp**
> 
> Im Markup der *CheckoutReview.aspx* Seite, beachten Sie, dass die `<ItemStyle>` Tag wird verwendet, um das Format der Elemente im Ändern der **DetailsView** Steuerelement am unteren Rand der Seite. Anzeigen der Seite in **Entwurfsansicht** (dazu **Entwurf** in der unteren linken Ecke von Visual Studio), wählen Sie dann die **DetailsView** steuern, und wählen Sie die  **Smarttag** (das Symbol "Pfeil" oben rechts auf der das Steuerelement), können, finden Sie unter der **DetailsView-Aufgaben**.
> 
> ![Bezahlvorgang und Zahlung mit PayPal - Bearbeiten Felder](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Durch Auswahl **Felder bearbeiten**, **Felder** Dialogfeld wird angezeigt. In diesem Dialogfeld können Sie leicht steuern die visuellen Eigenschaften, z. B. **ItemStyle**, der die **DetailsView** Steuerelement.
> 
> ![Bezahlvorgang und Zahlung mit PayPal - Dialogfeld](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>Kauf abgeschlossen

*CheckoutComplete.aspx* auf der Seite können Sie den Kauf von PayPal. Wie bereits erwähnt, muss der Benutzer klicken, auf die **vollständigen Order** Schaltfläche, bevor die Anwendung zu navigieren, wird die *CheckoutComplete.aspx* Seite.

1. In der *Auschecken* Ordner die Seite mit dem Namen *CheckoutComplete.aspx*.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Öffnen Sie die CodeBehind-Seite, die mit dem Namen *CheckoutComplete.aspx.cs* und Ersetzen Sie den vorhandenen Code durch Folgendes:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Wenn die *CheckoutComplete.aspx* Seite geladen wurde, die `DoCheckoutPayment` Methode wird aufgerufen. Wie bereits erwähnt die `DoCheckoutPayment` Methode abgeschlossen wird, den Kauf von der PayPal-testumgebung. Wenn PayPal den Kauf von der Reihenfolge abgeschlossen ist die *CheckoutComplete.aspx* Seite zeigt eine Zahlungstransaktion `ID` dem Käufer.

### <a name="handle-cancel-purchase"></a>Cancel Purchase behandeln

Wenn der Benutzer entscheidet, die den Kaufvorgang zu stornieren, sie geleitet werden, die *CheckoutCancel.aspx* Seite, in denen sie sehen, dass die Reihenfolge abgebrochen wurde.

1. Öffnen Sie die Seite mit dem Namen *CheckoutCancel.aspx* in die *Auschecken* Ordner.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Behandeln von Fehlern erwerben

Fehler bei der Kaufabwicklung werden vom behandelt die *CheckoutError.aspx* Seite. Der Code-Behind der der *CheckoutStart.aspx* Seite die *CheckoutReview.aspx* Seite und die *CheckoutComplete.aspx* Seite leitet jede an die  *CheckoutError.aspx* Seite, wenn ein Fehler auftritt.

1. Öffnen Sie die Seite mit dem Namen *CheckoutError.aspx* in die *Auschecken* Ordner.
2. Ersetzen Sie das vorhandene Markup durch Folgendes:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

Die *CheckoutError.aspx* Seite mit den Fehlerdetails angezeigt wird, tritt ein Fehler während des Auscheckvorgangs.

## <a name="running-the-application"></a>Ausführen der Anwendung

Führen Sie die Anwendung zu erfahren, wie Sie Produkte kaufen. Beachten Sie, dass Sie das PayPal ausführen, testen die Umgebung. Keine tatsächliche Geld ist ausgetauscht wird.

1. Stellen Sie sicher, dass alle Ihre Dateien in Visual Studio gespeichert werden.
2. Öffnen Sie einen Webbrowser, und navigieren Sie zu [ https://developer.paypal.com ](https://developer.paypal.com/).
3. Melden Sie sich mit Ihrer PayPal-Developer-Konto, das Sie zuvor in diesem Tutorial erstellt haben.  
   Für PayPal Entwickler Sandkasten, müssen Sie am angemeldet sein, [ https://developer.paypal.com ](https://developer.paypal.com/) express Auschecken zu testen. Dies gilt nur PayPals-Sandbox, die nicht zu PayPals-liveumgebung.
4. Drücken Sie in Visual Studio **F5** zum Ausführen der Wingtip Toys-beispielanwendung.  
   Nachdem die Datenbank neu erstellt, der Browser wird geöffnet und zeigt die *"default.aspx"* Seite.
5. Drei verschiedene Produkte zum Einkaufswagen hinzufügen, indem Sie die Product Category, z. B. "Cars" auswählen, und klicken Sie dann auf **zum Warenkorb hinzufügen** neben jedem Produkt.  
   Der Einkaufswagen wird das Produkt angezeigt, die, das Sie ausgewählt haben.
6. Klicken Sie auf die **PayPal** Schaltfläche zum Auschecken. 

    ![Bezahlvorgang und Zahlung mit PayPal - Warenkorb](checkout-and-payment-with-paypal/_static/image20.png)

   Auschecken müssen Sie ein Benutzerkonto für die Wingtip Toys-beispielanwendung verfügen.
7. Klicken Sie auf die **Google** Link auf der rechten Seite der Seite mit einem vorhandenen gmail.com-e-Mail-Konto anmelden.  
   Wenn Sie nicht über ein gmail.com-Konto verfügen, können Sie erstellen, eine für Testzwecke an [www.gmail.com](https://www.gmail.com/). Sie können auch ein lokales standard-Konto verwenden, indem Sie auf "Registrieren". 

    ![Bezahlvorgang und Zahlung mit PayPal - Anmeldung](checkout-and-payment-with-paypal/_static/image21.png)
8. Melden Sie sich mit Ihren Gmail-Konto und Ihr Kennwort. 

    ![Bezahlvorgang und Zahlung mit PayPal - Gmail-Anmeldung](checkout-and-payment-with-paypal/_static/image22.png)
9. Klicken Sie auf die **melden Sie sich bei** Schaltfläche, um Ihr Gmail-Konto mit Ihrem Benutzernamen von Wingtip Toys-Beispiel-Anwendung zu registrieren. 

    ![Bezahlvorgang und Zahlung mit PayPal - Register-Konto](checkout-and-payment-with-paypal/_static/image23.png)
10. Fügen Sie auf der Website für den PayPal-Test, Ihre **Käufer** e-Mail-Adresse und Kennwort, das Sie zuvor in diesem Tutorial erstellt haben, und klicken Sie dann die **anmelden** Schaltfläche. 

    ![Bezahlvorgang und Zahlung mit PayPal - PayPal-Anmeldung](checkout-and-payment-with-paypal/_static/image24.png)
11. Akzeptieren Sie die PayPal-Richtlinie, und klicken Sie auf die **zustimmen und Fortfahren** Schaltfläche.  
    Beachten Sie, dass diese Seite nur angezeigt, bei der ersten Verwendung dieses PayPal-Konto. Beachten Sie erneut, dass dies ist ein Testkonto keine Menge Geld gespart werden ausgetauscht. 

    ![Bezahlvorgang und Zahlung mit PayPal - PayPal-Richtlinie](checkout-and-payment-with-paypal/_static/image25.png)
12. Überprüfen Sie die Order-Informationen für den PayPal testen Umgebung Seite "Überprüfen", und klicken Sie auf **Weiter**. 

    ![Bezahlvorgang und Zahlung mit PayPal - Informationen überprüfen](checkout-and-payment-with-paypal/_static/image26.png)
13. Auf der *CheckoutReview.aspx* Seite, überprüfen Sie die Bestellmenge aus, und zeigen Sie die generierte Lieferadresse. Klicken Sie auf die **vollständigen Order** Schaltfläche. 

    ![Bezahlvorgang und Zahlung mit PayPal - Order-Überprüfung](checkout-and-payment-with-paypal/_static/image27.png)
14. Die **CheckoutComplete.aspx** Seite wird angezeigt, mit einer Zahlung Transaktions-ID. 

    ![Bezahlvorgang und Zahlung mit PayPal - Checkout abgeschlossen](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Überprüfen die Datenbank

Überprüfen Sie die aktualisierten Daten in der Anwendung Wingtip Toys-Beispieldatenbank, nach dem die Anwendung ausführen, sehen Sie sich, dass die Anwendung erfolgreich den Erwerb der Produkte aufgezeichnet.

Sie können überprüfen, dass die Daten in die *Wingtiptoys.mdf* Datenbankdatei mit der **Datenbank-Explorer** Fenster (**Server-Explorer** Fenster in Visual Studio) wie weiter oben in dieser tutorialreihe.

1. Schließen Sie das Browserfenster, wenn es noch geöffnet ist.
2. Wählen Sie in Visual Studio die **alle Dateien anzeigen** Symbol am oberen Rand **Projektmappen-Explorer** , die Ihnen ermöglichen, erweitern die **App\_Daten** Ordner.
3. Erweitern Sie die **App\_Daten** Ordner.  
 Sie müssen möglicherweise die **alle Dateien anzeigen** Symbol für den Ordner.
4. Mit der rechten Maustaste die *Wingtiptoys.mdf* Datenbankdatei, und wählen **öffnen**.  
    **Server-Explorer** wird angezeigt.
5. Erweitern Sie die **Tabellen** Ordner.
6. Mit der rechten Maustaste die **Bestellungen**Tabelle, und wählen Sie **Tabellendaten anzeigen**.  
 Die **Bestellungen** Tabelle wird angezeigt.
7. Überprüfen Sie die **PaymentTransactionID** Spalte, um erfolgreiche Transaktionen zu bestätigen. 

    ![Bezahlvorgang und Zahlung mit PayPal - Review-Datenbank](checkout-and-payment-with-paypal/_static/image29.png)
8. Schließen der **Bestellungen** Tabellenfenster.
9. Klicken Sie im Server-Explorer mit der Maustaste der **OrderDetails** Tabelle, und wählen Sie **Tabellendaten anzeigen**.
10. Überprüfen Sie die `OrderId` und `Username` Werte in der **OrderDetails** Tabelle. Beachten Sie, die diese Werte entsprechen den `OrderId` und `Username` in enthaltenen Werte der **Bestellungen** Tabelle.
11. Schließen der **OrderDetails** Tabellenfenster.
12. Mit der rechten Maustaste in der Wingtip Toys-Datenbankdatei (*Wingtiptoys.mdf*), und wählen Sie **enge Verbindung**.
13. Wenn Sie nicht sehen die **Projektmappen-Explorer** Fenster klicken Sie auf **Projektmappen-Explorer** am unteren Rand der **Server-Explorer** Fenster angezeigt wird der **Projektmappen-Explorer**  erneut aus.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie Reihenfolge und Order Detail-Schemas zum Nachverfolgen des Kauf von Produkten hinzugefügt. Sie werden auch PayPal-Funktionalität in der Wingtip Toys-beispielanwendung integriert.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Übersicht über die Konfiguration von ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Bereitstellen einer sicheren ASP.NET Web Forms-App mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Haftungsausschluss

In diesem Tutorial enthält Beispielcode. Solche Beispielcode wird "wie besehen" bereitgestellt, ohne jegliche Garantie bereitgestellt. Microsoft wird entsprechend der Genauigkeit, die Integrität und die Qualität des Beispielcodes nicht garantiert. Stimmen Sie den Beispielcode auf eigenes Risiko verwenden. Unter keinen Umständen werden Microsoft Verantwortung in keiner Weise für Beispielcode, Inhalt, einschließlich, aber nicht beschränkt auf, bei Fehlern oder auslassungen in Beispielcode, Inhalt oder Verlust oder Schäden jeglicher Art, die durch die Verwendung von Beispielcode entstehen. Sie werden hiermit benachrichtigt, und stimmen Sie hiermit zu stehen, speichern Sie Microsoft und gegen alle Verlust, die Ansprüche von Verlust, Verletzungen oder Beschädigung aller Art einschließlich und ohne Einschränkung, die trägt oder Material, das Sie bereitstellen, entstehen, übertragen, verwenden oder einschließlich, aber nicht beschränkt auf, die darin Stellungnahmen abhängig.

> [!div class="step-by-step"]
> [Zurück](shopping-cart.md)
> [Weiter](membership-and-administration.md)
