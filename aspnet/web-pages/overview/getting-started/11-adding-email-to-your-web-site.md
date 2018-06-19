---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Senden von E-Mail von einem ASP.NET Web Pages (Razor) Standort | Microsoft Docs
author: tfitzmac
description: In diesem Kapitel wird erläutert, wie eine automatisierte e-Mail-Nachricht von einer Website gesendet werden.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 9be242d238c627a9557fe7ff7e596974e5b7d1c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896515"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Senden von E-Mail von einem Standort der ASP.NET Web Pages (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie eine e-Mail-Nachricht von einer Website gesendet, bei der Verwendung von ASP.NET Web Pages (Razor).
> 
> Lernen Sie:
> 
> - So senden Sie eine e-Mail-Nachricht aus der Website.
> - Wie eine Datei an eine e-Mail-Nachricht angefügt.
> 
> Dies ist die ASP.NET-Funktion im Artikel eingeführt:
> 
> - Die `WebMail` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Senden von e-Mail-Nachrichten von der Website

Es gibt viele Gründe, warum Sie möglicherweise das Senden von e-Mails aus Ihrer Website. Können Sie Bestätigungsnachrichten an Benutzer senden, oder Sie können Benachrichtigungen an sich selbst senden, (z. B., die ein neuer Benutzer registriert wurden.) Die `WebMail` Helper erleichtert Ihnen das Senden von e-Mails.

Verwenden der `WebMail` Helper, müssen Sie Zugriff auf einen SMTP-Server haben. (SMTP steht für *Simple Mail Transfer Protocol*.) Ein SMTP-Server ist ein e-Mail-Server, der nur Nachrichten an den Server des Empfängers weiterleitet &#8212; der ausgehenden e-Mail-Seite ist. Wenn Sie einen Hostinganbieter für Ihre Website verwenden, sie wahrscheinlich richten Sie mit e-Mail-Adresse, und sie ableiten können Sie was Ihr SMTP-Servername ist. Wenn Sie in einem Unternehmensnetzwerk arbeiten, erhalten ein Administrator oder Ihre IT-Abteilung in der Regel die Informationen über einen SMTP-Server Sie, die Sie verwenden können. Wenn Sie zu Hause arbeiten, sind Sie möglicherweise auch in der Ihre normale e-Mail-Anbieter, die Sie, den Namen des SMTP-Servers erkennen können zu testen. Sie müssen in der Regel:

- Der Name des SMTP-Servers.
- Die Portnummer. Dies ist fast immer 25. Allerdings ISP benötigen Sie möglicherweise Port 587 verwenden. Bei Verwendung von secure Sockets Layer (SSL) zum Abrufen von e-Mails ggf. Sie einen anderen Port. Wenden Sie sich an Ihrem e-Mail-Anbieter.
- Anmeldeinformationen (Benutzername, Kennwort).

In diesem Verfahren erstellen Sie zwei Seiten. Die erste Seite verfügt über ein Formular, das Benutzer, die eine Beschreibung eingeben kann, als ob sie in einer Form des technischen Supports gefüllt wurden. Die erste Seite sendet die Informationen zu einer zweiten Seite. Auf der zweiten Seite Code des Benutzers Informationen extrahiert und sendet eine e-Mail-Nachricht. Außerdem wird bestätigt, dass der Problembericht empfangen wurde in eine Meldung angezeigt.

![[Image]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Um dieses Beispiel einfach zu halten, im Code initialisiert das `WebMail` Helper rechts auf der Seite, wo Sie es verwenden. Allerdings für echte Websites ist es besser, Initialisierungscode wie folgt in einer globalen Datei abgelegt, damit Sie initialisieren die `WebMail` -Hilfsprogramm für alle Dateien in Ihrer Website. Weitere Informationen finden Sie unter [anpassen standortweite Verhalten für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Erstellen einer neuen Website.
2. Fügen Sie eine neue Seite mit dem Namen *EmailRequest.cshtml* und fügen Sie das folgende Markup hinzu: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Beachten Sie, dass die `action` Attribut des Form-Elements vorsieht *ProcessRequest.cshtml*. Dies bedeutet, dass das Formular zu dieser Seite wieder an der aktuellen Seite übermittelt werden.
3. Fügen Sie eine neue Seite mit dem Namen *ProcessRequest.cshtml* auf der Website, und fügen Sie folgenden Code und Markup hinzu:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    In den Code erhalten Sie die Werte der Felder des Formulars, die auf der Seite gesendet wurden. Rufen Sie dann die `WebMail` der Hilfsfunktion `Send` Methode zum Erstellen und Senden der e-Mail-Nachricht. In diesem Fall die zu verwendenden Werte des Texts bestehen, die mit den Werten zu verketten, die aus dem Formular gesendet wurden.

    Der Code für diese Seite befindet sich innerhalb einer `try/catch` Block. Wenn alle Grund, dass beim Senden eine e-Mail nicht funktioniert (z. B. die Einstellungen nicht rechts), den Code in der `catch` Block ausgeführt wird, und legt sie fest der `errorMessage` Variable mit dem Fehler, der aufgetreten ist. (Weitere Informationen zu `try/catch` Blöcke oder `<text>` zu kennzeichnen, finden Sie unter [Einführung in ASP.NET Web Pages Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    Im Hauptteil der Seite "Wenn die `errorMessage` Variable ist leer (Standard), der Benutzer wird eine Meldung, die die e-Mail-Nachricht gesendet wurde. Wenn die `errorMessage` Variable wird auf "true", dem Benutzer angezeigt wird, eine Meldung, dass ein Problem mit dem Senden der Nachricht wurde festgelegt.

    Beachten Sie, dass im Bereich der Seite, die eine Fehlermeldung angezeigt wird, es eine zusätzliche Prüfung ist: `if(debuggingFlag)`. Dies ist eine Variable, die Sie festlegen können, auf "true", wenn beim Senden von e-Mail Probleme auftreten. Wenn `debuggingFlag` ist "true", und ist ein Problem beim Senden von e-Mail, wird eine zusätzliche Fehlermeldung angezeigt, die zeigt, was ASP.NET gemeldet hat, bei dem Versuch, die e-Mail-Nachricht zu senden. Durchschnittlich "Warnung" und obwohl: Fehlermeldungen, die ASP.NET wird berichtet, wenn sie eine e-Mail-Nachricht senden kann nicht generisch sein können. Beispielsweise, wenn ASP.NET kann nicht wenden Sie sich an den SMTP-Server (z. B. weil Sie Fehler in den Namen des Servers vorgenommen), Fehler: `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Wichtige** Wenn Sie eine Fehlermeldung erhalten, von einem Ausnahmeobjekt (`ex` im Code), führen Sie *nicht* dieser Nachricht über routinemäßig für Benutzer zu übergeben. Ausnahmeobjekte gehören häufig Informationen, die Benutzern sollte nicht angezeigt und können, die gerade ein Sicherheitsrisiko sein. Deshalb ist dieser Code enthält die Variable `debuggingFlag` , als Schalter verwendet wird, um die Fehlermeldung anzuzeigen, und warum die Variable standardmäßig auf "false" festgelegt ist. Sie sollten diese Variable auf "true" (und daher die Fehlermeldung angezeigt) festlegen *nur* sind, haben Sie ein Problem mit dem Senden von e-Mails und zum Debuggen. Nachdem Sie alle Probleme behoben haben, festlegen `debuggingFlag` wieder auf "false".

    Ändern Sie die folgenden e-Mail-verwandte Einstellungen im Code:

   - Legen Sie `your-SMTP-host` auf den Namen des SMTP-Servers, die Sie können zugreifen.
   - Legen Sie `your-user-name-here` mit dem Benutzernamen für Ihr SMTP-Server-Konto.
   - Legen Sie `your-account-password` auf das Kennwort für Ihr SMTP-Server-Konto.
   - Legen Sie `your-email-address-here` an Ihre eigene e-Mail-Adresse. Dies ist die e-Mail-Adresse, der aus die Nachricht gesendet wird. (Einige e-Mail-Anbieter nicht können Sie angeben, ein anderes `From` behandeln und Ihren Benutzernamen als verwendet die `From` Adresse.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Konfigurieren von e-Mail-Einstellungen
     > 
     > Es kann eine Herausforderung in einigen Fällen stellen Sie sicher, dass Sie die richtigen Einstellungen für den SMTP-Server, Portnummer und usw. sein. Im Folgenden einige Tipps:
     > 
     > - Der SMTP-Servername ist häufig etwa `smtp.provider.com` oder `smtp.provider.net`. Jedoch wenn Sie Ihre Website mit einem Hostinganbieter veröffentlichen, der SMTP-Servernamen zu diesem Zeitpunkt möglicherweise `localhost`. Dies ist möglicherweise, nachdem Sie veröffentlicht haben, und Ihre Website auf dem Server des Anbieters ausgeführt wird, der e-Mail-Server aus der Perspektive der Anwendung lokalen. Diese Änderung in Server-Namen kann dies bedeuten, dass Sie den SMTP-Servernamen als Teil der Veröffentlichungsprozess ändern müssen.
     > - Die Nummer des Ports ist in der Regel 25. Allerdings erfordern einige Anbieter Port 587 oder ein anderer Port.
     > - Stellen Sie sicher, dass Sie die richtigen Anmeldeinformationen verwenden. Wenn Sie Ihre Website mit einem Hostinganbieter veröffentlicht haben, verwenden Sie die Anmeldeinformationen, die der Anbieter zum Abrufen von e-Mails sind speziell angegeben hat. Hierbei können sich von den Anmeldeinformationen handeln, die Sie zum Veröffentlichen verwenden.
     > - In einigen Fällen benötigen nicht Sie die Anmeldeinformationen an. Wenn Sie e-Mail-Nachricht mit Ihren persönlichen Internetdienstanbieter gesendet, möglicherweise Ihre Anmeldeinformationen von Ihrem e-Mail-Anbieter bereits kennen. Nachdem Sie veröffentlicht haben, müssen Sie andere Anmeldeinformationen als beim Testen auf dem lokalen Computer verwenden.
     > - Wenn die Verschlüsselung von Ihrem e-Mail-Anbieter verwendet wird, müssen Sie festlegen `WebMail.EnableSsl` auf `true`.
4. Führen Sie die *EmailRequest.cshtml* Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.)
5. Geben Sie Ihren Namen und eine Beschreibung des Problems, und klicken Sie dann auf die **Absenden** Schaltfläche. Sie sind umgeleitet, um die *ProcessRequest.cshtml* Seite, die die Nachricht bestätigt und dem sendet Ihnen einer e-Mail-Nachricht. 

    ![[Image]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Eine Datei per E-Mail senden

Sie können auch Dateien senden, die e-Mail-Nachrichten angefügt sind. In diesem Verfahren erstellen Sie eine Textdatei und zwei HTML-Seiten. Verwenden Sie die Textdatei als e-Mail-Anlage.

1. Fügen Sie auf der Website eine neue Textdatei hinzu, und nennen Sie sie *MyFile.txt*.
2. Kopieren Sie den folgenden Text ein, und fügen Sie ihn in der Datei: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Erstellen Sie eine Seite mit dem Namen *SendFile.cshtml* und fügen Sie das folgende Markup hinzu: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Erstellen Sie eine Seite mit dem Namen *ProcessFile.cshtml* und fügen Sie das folgende Markup hinzu: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Ändern Sie die folgenden e-Mail-verwandte Einstellungen in den Code aus dem Beispiel:

    - Legen Sie `your-SMTP-host` auf den Namen eines SMTP-Servers, die Sie können zugreifen.
    - Legen Sie `your-user-name-here` mit dem Benutzernamen für Ihr SMTP-Server-Konto.
    - Legen Sie `your-email-address-here` an Ihre eigene e-Mail-Adresse. Dies ist die e-Mail-Adresse, der aus die Nachricht gesendet wird.
    - Legen Sie `your-account-password` auf das Kennwort für Ihr SMTP-Server-Konto.
    - Legen Sie `target-email-address-here` an Ihre eigene e-Mail-Adresse. (Wie vor, würden Sie normalerweise eine e-Mail an eine andere Person senden, aber Sie können es zum Testen und sich selbst senden.)
6. Führen Sie die *SendFile.cshtml* Seite in einem Browser.
7. Geben Sie Ihren Namen, eine Betreffzeile und den Namen der Textdatei angefügt (*MyFile.txt*).
8. Klicken Sie auf die Schaltfläche `Submit`. Wie bereits zuvor können Sie auf umgeleitet sind die *ProcessFile.cshtml* Seite, die bestätigt, dass Ihre Nachricht und die sendet Ihnen einer e-Mail-Nachricht mit der angefügten Datei.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


- [Leitfaden zur Behandlung von Problemen mit ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Simple Mail Transfer-Protokoll](https://msdn.microsoft.com/library/aa480435.aspx)
- [Anpassen des standortweite Verhaltens für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
