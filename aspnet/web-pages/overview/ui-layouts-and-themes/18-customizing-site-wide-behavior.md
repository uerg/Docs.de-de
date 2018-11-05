---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Für ASP.NET-Webseiten (Razor) Sites standortweite verhaltensanpassung | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Kapitel wird erläutert, wie Sie Einstellungen auf der gesamten Website oder einen gesamten Ordner statt nur für eine Seite.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: ca7c241d6e4d1e4baa581faf2bda4ed275b4e785
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021468"
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Anpassen von standortweite Verhalten für ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie Website-Seite Einstellungen für Seiten auf einer Website für ASP.NET Web Pages (Razor).
> 
> Sie lernen Folgendes:
> 
> - Wie Sie Code ausführen, Sie mit der Werte Set (globale Werte oder Helper-Einstellungen) für alle Seiten in einem Standort an.
> - Informationen zum Ausführen von Code, in dem Sie die Werte für alle Seiten in einem Ordner festlegen kann.
> - Ausführen von Code vor und nach einer Seite lädt.
> - Informationen zum Senden von Fehlern an einer zentralen Fehlerseite.
> - Das Hinzufügen von Authentifizierung für alle Seiten in einem Ordner.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library (NuGet-Paket)
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages-3, und kann keine Visual Studio 2013 (oder ein Visual Studio Express 2013 für Web), außer dass Sie die ASP.NET Web Helpers Library.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Hinzufügen von Startcode für die Website für ASP.NET Web Pages

Für einen Großteil des Codes, den Sie in ASP.NET Web Pages schreiben, kann eine einzelne Seite der gesamte Code enthalten, der für diese Seite erforderlich ist. Wenn eine Seite eine e-Mail-Nachricht sendet, ist es beispielsweise möglich, den gesamten Code für diesen Vorgang in einer einzelnen Seite eingefügt. Dies kann den Code, um die Einstellungen zu initialisieren, für das Senden von e-Mail-Adresse einschließen (d. h. für den SMTP-Server) und zum Senden von e-Mail-Nachricht.

In einigen Fällen möchten jedoch möglicherweise Code ausführen, bevor jede Seite auf der Website ausgeführt wird. Dies ist nützlich für das Festlegen von Werten, die an einer beliebigen Stelle auf der Website verwendet werden können (so genannte *globale Werte*.) Beispielsweise erfordern einige Hilfsmethoden Sie Werte wie e-Mail-Einstellungen oder den Schlüssel angeben. Es kann praktisch, um diese Einstellungen in den globalen Werten beibehalten sein.

Hierzu erstellen eine Seite namens  *\_AppStart.cshtml* im Stammverzeichnis der Website. Wenn auf dieser Seite vorhanden ist, wird beim ersten einer beliebigen Seite am Standort angefordert wird ausgeführt. Aus diesem Grund ist es eine gute Möglichkeit zum Ausführen von Code, um globale Werte festzulegen. (Da  *\_AppStart.cshtml* ein Unterstrich Präfix, ASP.NET wird nicht die Seite für einen Browser senden, auch wenn der Benutzer direkt anfordern.)

Im folgenden Diagramm wird wie die  *\_AppStart.cshtml* Seite funktioniert. Wenn eine Anforderung für eine Seite eingeht, und ist dies die erste Anforderung für eine beliebige auf der Seite der Website, die ASP.NET prüft zunächst, ob eine  *\_AppStart.cshtml* Seite vorhanden ist. Falls Ja, jeder beliebige code im der  *\_AppStart.cshtml* Seite ausgeführt wird, und klicken Sie dann die angeforderte Seite ausgeführt wird.

![[Image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Festlegen von globalen Werten für Ihre Website

1. Erstellen Sie im Stammordner einer WebMatrix-Website, eine Datei namens  *\_AppStart.cshtml*. Die Datei muss im Stammverzeichnis der Website sein.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Dieser Code wird ein Wert in der `AppState` Wörterbuch, das automatisch für alle Seiten der Website verfügbar ist. Beachten Sie, dass die  *\_AppStart.cshtml* Datei keine Markup darin. Die Seite wird führen Sie den Code und leiten Sie dann auf der Seite, die ursprünglich angefordert wurde.

    > [!NOTE]
    > Seien Sie vorsichtig, wenn Sie Code, in Einfügen der  *\_AppStart.cshtml* Datei. Auftreten von Fehlern im Code in die  *\_AppStart.cshtml* -Datei, die Website nicht gestartet.
3. In den Stammordner, erstellen Sie eine neue Seite mit dem Namen *AppName.cshtml*.
4. Ersetzen Sie die Standard-Markup und Code durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Dieser Code extrahiert den Wert aus der `AppState` -Objekt, das Sie, in Festlegen der  *\_AppStart.cshtml* Seite.
5. Führen Sie die *AppName.cshtml* Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Die Seite zeigt den globalen Wert. 

    ![[Image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Festlegen von Werten für Hilfsprogramme

Eine gute Verwendung für die  *\_AppStart.cshtml* Datei wird zum Festlegen von Werten für Hilfsprogramme, die Sie an Ihrem Standort verwenden, die initialisiert werden müssen. Typische Beispiele sind e-Mail-Einstellungen für die `WebMail` -Hilfsobjekt und die privaten und öffentlichen Schlüssel für die `ReCaptcha` Helper. In solchen Fällen können Sie festlegen, nach die Werten in der  *\_AppStart.cshtml* dann sind bereits festgelegt, für alle Seiten auf Ihrer Website.

Dieses Verfahren zeigt, wie festzulegende `WebMail` Einstellungen Global. (Weitere Informationen zur Verwendung der `WebMail` Hilfsprogramms finden Sie unter [Hinzufügen von e-Mail-Adresse zu einer ASP.NET Web Pages-Website](../getting-started/11-adding-email-to-your-web-site.md).)

1. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), falls Sie bereits hinzugefügt haben.
2. Wenn Sie noch keinem  *\_AppStart.cshtml* Datei, erstellen Sie eine Datei namens im Stammordner einer Website  *\_AppStart.cshtml*.
3. Fügen Sie die folgenden `WebMail` Einstellungen, die die  *\_AppStart.cshtml* Datei: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Ändern Sie die folgenden e-Mail-zusammengehörigen Einstellungen in den Code:

   - Legen Sie `your-SMTP-host` auf den Namen des SMTP-Servers, die Sie können zugreifen.
   - Legen Sie `your-user-name-here` , den Benutzernamen für Ihr SMTP-Server-Konto.
   - Legen Sie `your-account-password` auf das Kennwort für Ihr SMTP-Server-Konto.
   - Legen Sie `your-email-address-here` an Ihre eigene e-Mail-Adresse. Dies ist die e-Mail-Adresse, der aus die Nachricht gesendet wird. (Nicht von einigen e-Mail-Anbieter können Sie angeben, ein anderes `From` beheben, und verwenden Ihren Benutzernamen, als die `From` Adresse.)

     Weitere Informationen zu SMTP-Einstellungen finden Sie unter [Konfigurieren von e-Mail-Einstellungen](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) in diesem Artikel [Senden von e-Mail-Adresse von einer Website für ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) und [Probleme beim Senden von e-Mail-](https://go.microsoft.com/fwlink/?LinkId=253001#email)in die [ASP.NET Web Pages (Razor) – Handbuch zur Problembehandlung](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Speichern Sie die  *\_AppStart.cshtml* Datei, und schließen Sie es.
5. Erstellen Sie im Stammordner einer Website, die neue Seite, die mit dem Namen *TestEmail.cshtml*.
6. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Führen Sie die *TestEmail.cshtml* Seite in einem Browser.
8. Geben Sie die Felder aus, selbst eine e-Mail-Nachricht zu senden, und klicken Sie dann auf **senden**.
9. Überprüfen Sie Ihre e-Mail-Adresse, um sicherzustellen, dass Sie die Nachricht erhalten haben.

Der wichtige Teil dieses Beispiels ist, die die Einstellungen, die Sie in der Regel nicht ändern – z. B. den Namen Ihres SMTP-Servers und Ihrer e-Mail-Anmeldeinformationen – in festgelegt sind das  *\_AppStart.cshtml* Datei. Auf diese Weise müssen Sie sie erneut festlegen auf den einzelnen Seiten, in denen Sie e-Mail. (Obwohl es sich um eine Falls aus irgendeinem Grund Sie diese Einstellungen ändern möchten, Sie diese einzeln auf einer Seite festlegen können.) Auf der Seite legen Sie nur die Werte, die in der Regel jedes Mal, wie Sie den Empfänger und der Text der e-Mail-Nachricht ändern.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Ausführen von Code vor und nach Dateien in einem Ordner

Wie Sie verwenden können,  *\_AppStart.cshtml* zum Schreiben von Code vor dem Ausführen von Seiten der Website, Sie können Code schreiben, bevor Sie (und höher) ausgeführt, einer beliebigen Seite in einem bestimmten Ordner ausführen. Dies ist nützlich für Aufgaben wie das Festlegen der gleichen Layoutseite für alle Seiten in einem Ordner oder für die Überprüfung, die ein Benutzer angemeldet ist, bevor Sie mit einer Seite in den Ordner.

Für Seiten insbesondere Ordnern können Sie erstellen Code in einer Datei namens  *\_PageStart.cshtml*. Im folgenden Diagramm wird wie die  *\_PageStart.cshtml* Seite funktioniert. Wenn eine Anforderung für eine Seite eingeht, überprüft ASP.NET zuerst für eine  *\_AppStart.cshtml* Seite und ausgeführt wird. Anschließend ASP.NET überprüft, ob es eine  *\_PageStart.cshtml* Seite, und wenn dies der Fall ist, ausgeführt wird. Anschließend wird die angeforderte Seite ausgeführt.

In der  *\_PageStart.cshtml* Seite Sie können angeben, wo bei der Verarbeitung, die Sie die angeforderte Seite einschließlich der Ausführung möchten einer `RunPage` Methode. Dadurch können Sie Code ausführen, bevor die angeforderte Seite ausgeführt wird und dann erneut danach. Wenn Sie nicht einschließen `RunPage`, der gesamte Code in  *\_PageStart.cshtml* ausgeführt wird, und klicken Sie dann auf die angeforderte Seite automatisch ausgeführt.

![[Image]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET ermöglicht das Erstellen eine Hierarchie von  *\_PageStart.cshtml* Dateien. Sie können Einfügen einer  *\_PageStart.cshtml* -Datei im Stammverzeichnis der Website und in jedem Unterordner. Wenn eine Seite angefordert wird, die  *\_PageStart.cshtml* Datei an die oberste Ebene (am ehesten entsprechen dem Stammverzeichnis der Website) ausgeführt wird, gefolgt von der  *\_PageStart.cshtml* Datei in den nächsten Unterordner usw. weiter unten in der Unterordnerstruktur, bis die Anforderung auf den Ordner erreicht, der die angeforderte Seite enthält. Nach dem entsprechenden  *\_PageStart.cshtml* Dateien ausführen, die angeforderte Seite ausgeführt wird.

Angenommen, Sie müssen möglicherweise die folgende Kombination von  *\_PageStart.cshtml* Dateien und *Default.cshtml* Datei:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Beim Ausführen von */myfolder/default.cshtml*, sehen Sie Folgendes:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Ausführen von Code zum Initialisieren der für alle Seiten in einem Ordner

Eine gute Verwendung für  *\_PageStart.cshtml* Dateien ist die Initialisierung von der gleichen Layoutseite für alle Dateien in einem einzelnen Ordner.

1. In den Stammordner, erstellen Sie einen neuen Ordner namens *InitPages*.
2. In der *InitPages* Ordner der Website, erstellen Sie eine Datei, die mit dem Namen  *\_PageStart.cshtml* und Ersetzen Sie die Standard-Markup und Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Erstellen Sie im Stammverzeichnis der Website, einen Ordner namens *Shared*.
4. In der *Shared* Ordner, erstellen Sie eine Datei mit dem Namen  *\_Layout1.cshtml* und Ersetzen Sie die Standard-Markup und Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. In der *InitPages* Ordner eine Datei namens *Content1.cshtml* und Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. In der *InitPages* Ordner, erstellen Sie eine andere Datei, die mit dem Namen *Content2.cshtml* und Ersetzen Sie die Standard-Markup durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Führen Sie *Content1.cshtml* in einem Browser. 

    ![[Image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Wenn die *Content1.cshtml* Seite ausgeführt wird, die  *\_PageStart.cshtml* legt `Layout` und legt `PageData["MyBackground"]` in eine Farbe. In *Content1.cshtml*, das Layout und die Farbe angewendet werden.
8. Anzeige *Content2.cshtml* in einem Browser. 

    Das Layout ist identisch, da beide Seiten den gleichen auf der Layoutseite und die Farbe verwenden, wie in initialisiert  *\_PageStart.cshtml*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Mithilfe von \_PageStart.cshtml Behandeln von Fehlern

Verwenden Sie einen anderen gut für die  *\_PageStart.cshtml* Datei ist, erstellen Sie eine Möglichkeit zum Verarbeiten von Programmierfehlern (Ausnahmen), die in einem auftreten *.cshtml* Seite in einem Ordner. Dieses Beispiel zeigt, die eine Möglichkeit dazu.

1. Erstellen Sie in den Stammordner einen Ordner namens *InitCatch*.
2. In der *InitCatch* Ordner der Website, erstellen Sie eine Datei, die mit dem Namen  *\_PageStart.cshtml* und Ersetzen Sie das vorhandene Markup und Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    In diesem Code, versuchen Sie es unter der angeforderten Seite explizit durch Aufrufen der `RunPage` Methode innerhalb einer `try` Block. Wenn Programmierung Fehler auftreten, in die angeforderte Seite den Code in die `catch` -Block ausgeführt. In diesem Fall der Code auf einer Seite umgeleitet (*Error.cshtml*) und übergibt den Namen der Datei, die als Teil der URL der Fehler aufgetreten ist. (Sie müssen die Seite in Kürze erstellen.)
3. In der *InitCatch* Ordner der Website, erstellen Sie eine Datei, die mit dem Namen *Exception.cshtml* und Ersetzen Sie das vorhandene Markup und Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Für die Zwecke dieses Beispiels wird was Sie auf dieser Seite tun absichtlich erstellt einen Fehler mit dem Versuch, eine Datei zu öffnen, die nicht vorhanden.
4. Erstellen Sie eine Datei namens im Stammordner, *Error.cshtml* und Ersetzen Sie das vorhandene Markup und Code durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Auf dieser Seite des Ausdrucks `@Request["source"]` Ruft den Wert nicht in der URL ab und zeigt ihn.
5. Klicken Sie auf der Symbolleiste auf **speichern**.
6. Führen Sie *Exception.cshtml* in einem Browser. 

    ![[Image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Da ein Fehler auftritt *Exception.cshtml*,  *\_PageStart.cshtml* Seite leitet an die *Error.cshtml* -Datei, die die Meldung angezeigt wird.

    Weitere Informationen zu Ausnahmen finden Sie unter [Einführung in ASP.NET Web Pages-Programmierung verwenden die Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Mithilfe von \_PageStart.cshtml zum Einschränken des Ordnerzugriffs

Sie können auch die  *\_PageStart.cshtml* Datei, um den Zugriff auf alle Dateien in einem Ordner zu beschränken.

1. In WebMatrix erstellen eine neue Website mit dem **Websitevorlage aus** Option.
2. Wählen Sie aus den verfügbaren Vorlagen, **Starter Site**.
3. Erstellen Sie in den Stammordner einen Ordner namens *AuthenticatedContent*.
4. In der *AuthenticatedContent* Ordner eine Datei namens  *\_PageStart.cshtml* und Ersetzen Sie das vorhandene Markup und Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Der Code beginnt, indem verhindert, dass alle Dateien im Ordner zwischengespeichert wird. (Dies ist für Szenarien, z. B. öffentliche Computer eines Benutzers zwischengespeicherte Seiten für den nächsten Benutzer verfügbar sein sollen nicht erforderlich.) Als Nächstes ermittelt der Code an, ob der Benutzer bei der Website angemeldet hat, bevor sie eine der Seiten im Ordner anzeigen können. Wenn der Benutzer nicht angemeldet ist, leitet zur Anmeldeseite des Codes. Die Anmeldeseite kann den Benutzer auf der Seite, die ursprünglich angefordert wurde, wenn Sie einen Abfragezeichenfolgenwert mit dem Namen einschließen zurückgeben `ReturnUrl`.
5. Erstellen eine neue Seite in der *AuthenticatedContent* Ordner mit dem Namen *Page.cshtml*.
6. Ersetzen Sie das Standardmarkup durch Folgendes:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Führen Sie *Page.cshtml* in einem Browser. Der Code, die Sie an eine Anmeldeseite weitergeleitet. Sie müssen vor der Anmeldung registrieren. Nach dem Sie angemeldet und registriert haben, können Sie zur Seite navigieren, und dessen Inhalt anzeigen.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Einführung zu ASP.NET Web Pages-Programmierung mit Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587)
