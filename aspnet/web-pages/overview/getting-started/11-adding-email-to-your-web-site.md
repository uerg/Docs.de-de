---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Senden von E-Mails von einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Kapitel wird erläutert, wie Sie eine automatisierte e-Mail-Nachricht von einer Website zu senden.
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: e94cba91da43101ef1c058b49be746821bb0fe16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841470"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Senden von E-Mails von einem Standort der ASP.NET Web Pages (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie eine e-Mail-Nachricht von einer Website gesendet, bei der Verwendung von ASP.NET Web Pages (Razor).
> 
> Sie lernen Folgendes:
> 
> - So senden Sie eine e-Mail-Nachricht auf Ihrer Website.
> - So fügen Sie eine Datei eine e-Mail-Nachricht.
> 
> Dies ist die ASP.NET-Funktion, die in diesem Artikel eingeführt:
> 
> - Die `WebMail` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Senden von e-Mail-Nachrichten von der Website

Es gibt alle möglichen Gründe, warum Sie möglicherweise e-Mails von Ihrer Website senden. Sie können die Meldungen zur benutzerbestätigung an Benutzer senden oder Sie können Benachrichtigungen an sich selbst senden, (z. B., die ein neuer Benutzer registriert hat.) Die `WebMail` Hilfsprogramm vereinfacht Sie e-Mail-Adresse senden.

Verwenden der `WebMail` Helper, müssen Sie Zugriff auf einen SMTP-Server haben. (SMTP steht für *Simple Mail Transfer Protocol*.) Ein SMTP-Server ist ein e-Mail-Server, der nur Nachrichten an den Server des Empfängers weiterleitet &#8212; der ausgehenden Seite e-Mail. Wenn Sie einen Hostinganbieter für die Website verwenden, sie wahrscheinlich richten Sie mit e-Mail-Adresse, und sie können informieren Sie Ihr SMTP-Server Name. Wenn Sie in einem Unternehmensnetzwerk arbeiten, erhalten ein Administrator oder Ihre IT-Abteilung in der Regel die Informationen über einen SMTP-Server Sie, die Sie verwenden können. Wenn Sie zu Hause arbeiten, sind Sie möglicherweise auch in der mithilfe der normalen e-Mail-Anbieter, der Sie, den Namen für die SMTP-Server erkennen kann zu testen. Sie müssen in der Regel:

- Der Name des SMTP-Servers.
- Nummer des Ports. Dies ist fast immer 25. Allerdings kann Ihr Internetdienstanbieter Verwendung von Port 587 erforderlich machen. Wenn Sie secure Sockets Layer (SSL) für die e-Mail-Adresse verwenden, sollten Sie einen anderen Port. Wenden Sie sich an Ihren e-Mail-Anbieter.
- Anmeldeinformationen (Benutzername, Kennwort).

In diesem Verfahren erstellen Sie zwei Seiten. Die erste Seite enthält ein Formular, das Benutzer, die eine Beschreibung eingeben kann, als ob sie in einem technischen Support-Formular ausfüllen wurden. Die erste Seite übermittelt Informationen zu einer zweiten Seite. In der zweiten Seite Code extrahiert die Informationen des Benutzers und sendet eine e-Mail-Nachricht. Es zeigt auch eine Meldung zur Bestätigung, dass die Problembericht empfangen wurde.

![[Image]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> In diesem Beispiel wird der Einfachheit halber der Code initialisiert die `WebMail` Helper rechts auf der Seite, wo Sie es verwenden. Für echte Websites, es ist jedoch eine bessere Vorstellung Initialisierungscode wie folgt in der globalen Datei zu platzieren, damit Sie initialisieren den `WebMail` -Hilfsprogramm für alle Dateien auf Ihrer Website. Weitere Informationen finden Sie unter [anpassen standortweite Verhalten für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).


1. Erstellen einer neuen Website.
2. Hinzufügen einer neuen Seite, die mit dem Namen *EmailRequest.cshtml* und fügen Sie das folgende Markup hinzu: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Beachten Sie, dass die `action` Attribut des Form-Elements festgelegt wurde *ProcessRequest.cshtml*. Dies bedeutet, dass das Formular auf diese Seite wieder an der aktuellen Seite gesendet werden soll.
3. Hinzufügen einer neuen Seite, die mit dem Namen *ProcessRequest.cshtml* auf der Website, und fügen Sie den folgenden Code und Markup hinzu:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    Im Code erhalten Sie die Werte der Formularfelder, die auf der Seite gesendet wurden. Rufen Sie dann die `WebMail` des Hilfsprogramms `Send` Methode zum Erstellen und Senden der e-Mail-Nachricht. In diesem Fall die zu verwendenden Werte Text bestehen, die durch die Werte zu verketten, die aus dem Formular übermittelt wurden.

    Der Code für diese Seite befindet sich in einem `try/catch` Block. Wenn Sie einen Grund, dass beim Senden einer e-Mail-Adresse nicht funktioniert (z. B. die Einstellungen nicht rechts), den Code in der `catch` Block ausgeführt wird, und legt sie fest der `errorMessage` Variable auf den aufgetretenen Fehler. (Weitere Informationen zu `try/catch` Blöcke oder `<text>` markieren, finden Sie unter [Einführung in ASP.NET Web Pages-Programmierung verwenden die Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)

    Im Text der Seite Wenn die `errorMessage` Variable ist leer (Standard), die der Benutzer sieht einer Meldung, die die e-Mail-Nachricht gesendet wurde. Wenn die `errorMessage` Variable auf "true", dem Benutzer angezeigt wird, eine Nachricht, es wurde ein Problem mit dem Senden der Nachricht festgelegt ist.

    Beachten Sie, dass im Bereich der Seite, die eine Fehlermeldung angezeigt wird, gibt es eine zusätzliche Prüfung: `if(debuggingFlag)`. Dies ist eine Variable, die Sie festlegen können, auf "true", wenn beim Senden von e-Mail Probleme auftreten. Wenn `debuggingFlag` ist "true", und liegt ein Problem beim Senden von e-Mail-Adresse, wird eine zusätzliche Fehlermeldung angezeigt, die zeigt, was auf ASP.NET gemeldet hat, bei dem Versuch, die e-Mail-Nachricht zu senden. Fairerweise sollte erwähnt Warnung muss jedoch ausgesprochen: Fehlermeldungen, die ASP.NET wird berichtet, wenn sie eine e-Mail-Nachricht senden kann nicht generisch sein können. Beispielsweise, wenn ASP.NET kann nicht wenden Sie sich an den SMTP-Server (z. B. weil Sie in den Namen des Servers einen Fehler gemacht haben), wird des Fehlers `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Wichtige** Wenn Sie eine Fehlermeldung erhalten, von einem Ausnahmeobjekt (`ex` im Code), führen Sie *nicht* dieser Nachricht über routinemäßig für Benutzer zu übergeben. Ausnahmeobjekte enthalten häufig Informationen, die Benutzern sollte nicht angezeigt und, noch ein Sicherheitsrisiko sein kann. Deshalb ist dieser Code enthält die Variable `debuggingFlag` , als einen Switch verwendet wird, um die Fehlermeldung anzuzeigen, und warum die Variable standardmäßig auf "false" festgelegt ist. Sie sollten festlegen, dass diese Variable auf "true" (und daher die Fehlermeldung angezeigt) *nur* Wenn sind Sie ein Problem mit der e-Mail-Adresse gesendet haben, und Sie zum Debuggen müssen. Nachdem Sie alle Probleme behoben haben, legen Sie `debuggingFlag` wieder auf "false".

    Ändern Sie die folgenden e-Mail-zusammengehörigen Einstellungen in den Code:

   - Legen Sie `your-SMTP-host` auf den Namen des SMTP-Servers, die Sie können zugreifen.
   - Legen Sie `your-user-name-here` , den Benutzernamen für Ihr SMTP-Server-Konto.
   - Legen Sie `your-account-password` auf das Kennwort für Ihr SMTP-Server-Konto.
   - Legen Sie `your-email-address-here` an Ihre eigene e-Mail-Adresse. Dies ist die e-Mail-Adresse, der aus die Nachricht gesendet wird. (Nicht von einigen e-Mail-Anbieter können Sie angeben, ein anderes `From` beheben, und verwenden Ihren Benutzernamen, als die `From` Adresse.)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Konfigurieren von e-Mail-Einstellungen
     > 
     > Es kann sein, dass eine Herausforderung manchmal, stellen Sie sicher, dass Sie die richtigen Einstellungen für die SMTP-Server, Portnummer und So weiter. Im Folgenden einige Tipps:
     > 
     > - Der SMTP-Servername ist häufig mit etwa `smtp.provider.com` oder `smtp.provider.net`. Wenn Sie Ihre Website mit einem Hostinganbieter veröffentlichen, der SMTP-Servernamen an diesem Punkt möglicherweise jedoch `localhost`. Das liegt, nachdem Sie veröffentlicht haben, und Ihre Website auf dem Server des Anbieters ausgeführt wird, der e-Mail-Server lokal aus der Perspektive der Anwendung sein kann. Diese Änderung in Server-Namen bedeutet möglicherweise, dass Sie so ändern Sie den SMTP-Servernamen als Teil des Veröffentlichungsprozesses verfügen.
     > - Die Nummer des Ports ist in der Regel 25. Allerdings erfordern einige Anbieter Port 587 oder einige andere Ports.
     > - Stellen Sie sicher, dass Sie die richtigen Anmeldeinformationen verwenden. Wenn Sie Ihre Website mit einem Hostinganbieter veröffentlicht haben, verwenden Sie die Anmeldeinformationen, die der Anbieter zum Abrufen von e-Mails werden insbesondere angegeben hat. Diese können sich von den Anmeldeinformationen unterscheiden, die Sie verwenden, um die Veröffentlichung.
     > - Manchmal benötigen Sie keine Anmeldeinformationen überhaupt. Versenden von e-Mails mit Ihren persönlichen Internetdienstanbieter, möglicherweise Ihre Anmeldeinformationen von Ihren e-Mail-Anbieter bereits kennen. Nachdem Sie veröffentlicht haben, müssen Sie andere Anmeldeinformationen als wenn Sie auf dem lokalen Computer testen, verwenden.
     > - Wenn Ihre e-Mail-Anbieter die Verschlüsselung verwendet, müssen Sie festlegen `WebMail.EnableSsl` zu `true`.
4. Führen Sie die *EmailRequest.cshtml* Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.)
5. Geben Sie Ihren Namen und eine Beschreibung des Problems, und klicken Sie dann auf die **senden** Schaltfläche. Sie werden umgeleitet, um die *ProcessRequest.cshtml* Seite, was bestätigt, dass Ihre Nachricht ein, und die sendet Ihnen einer e-Mail-Nachricht. 

    ![[Image]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Eine Datei per E-Mail senden

Sie können auch Dateien senden, die e-Mail-Nachrichten zugeordnet sind. In diesem Verfahren erstellen Sie eine Textdatei und zwei HTML-Seiten. Sie verwenden die Textdatei als Anlage einer e-Mail.

1. Fügen Sie auf der Website eine neue Textdatei, und nennen Sie sie *MyFile.txt*.
2. Kopieren Sie den folgenden Text ein, und fügen Sie ihn in der Datei: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Erstellen Sie eine Seite mit dem Namen *SendFile.cshtml* und fügen Sie das folgende Markup hinzu: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Erstellen Sie eine Seite mit dem Namen *ProcessFile.cshtml* und fügen Sie das folgende Markup hinzu: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Ändern Sie die folgenden e-Mail-zusammengehörigen Einstellungen in den Code aus dem Beispiel:

    - Legen Sie `your-SMTP-host` auf den Namen eines SMTP-Servers an, die Sie können zugreifen.
    - Legen Sie `your-user-name-here` , den Benutzernamen für Ihr SMTP-Server-Konto.
    - Legen Sie `your-email-address-here` an Ihre eigene e-Mail-Adresse. Dies ist die e-Mail-Adresse, der aus die Nachricht gesendet wird.
    - Legen Sie `your-account-password` auf das Kennwort für Ihr SMTP-Server-Konto.
    - Legen Sie `target-email-address-here` an Ihre eigene e-Mail-Adresse. (Wie vor, Sie normalerweise beim Senden einer e-Mail an eine andere Person, aber Sie können es für Tests an sich selbst senden.)
6. Führen Sie die *SendFile.cshtml* Seite in einem Browser.
7. Geben Sie Ihren Namen, einer Betreffzeile und den Namen der Textdatei angefügt (*MyFile.txt*).
8. Klicken Sie auf die Schaltfläche `Submit`. Wie zuvor Sie zur umgeleitet werden die *ProcessFile.cshtml* Seite, was bestätigt, dass Ihre Nachricht ein, und die sendet Ihnen einer e-Mail-Nachricht mit der angefügten Datei.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


- [Leitfaden zur Behandlung von Problemen mit ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Simple Mail Transfer-Protokoll](https://msdn.microsoft.com/library/aa480435.aspx)
- [Standortweite verhaltensanpassung für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
