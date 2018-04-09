---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Anpassen von standortweite Verhalten für ASP.NET-Webseiten (Razor)-Websites | Microsoft Docs
author: tfitzmac
description: In diesem Kapitel erläutert, wie auf die gesamte Website oder einen ganzen Ordner anstatt auf eine Seite Einstellungen vornehmen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 4457318bcf1d2886eb8ed68fdd795eea7905368b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Anpassen von standortweite Verhalten für ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie Sie Standort-Seite Einstellungen für Seiten auf einer Website für ASP.NET Web Pages (Razor).
> 
> Lernen Sie:
> 
> - Wie der auszuführende Code, Sie mit der Werte festlegen (globale Werte oder Helper-Einstellungen) für alle Seiten auf einer Website an.
> - Wie Sie Code ausführen, in dem Sie Werte für alle Seiten in einem Ordner festlegen können.
> - Zum Ausführen von Code vor und nach einer Seite lädt.
> - Informationen zum Fehler auf einer zentralen Fehlerseite zu senden.
> - Das Hinzufügen von Authentifizierung für alle Seiten in einem Ordner.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library (NuGet-Paket)
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 3 und kann keine Visual Studio 2013 (oder ein Visual Studio Express 2013 für Web), außer dass Sie die ASP.NET Web Helpers Library.


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Hinzufügen von Website-Startcode für ASP.NET Web Pages

Für einen Großteil des Codes, den Sie in ASP.NET Web Pages schreiben, kann eine einzelne Seite der gesamte Code enthalten, der für diese Seite erforderlich ist. Wenn eine Seite eine e-Mail-Nachricht sendet, ist es beispielsweise möglich, alle den Code für diesen Vorgang in einer einzelnen Seite einfügen. Dies kann den Code, um die Einstellungen für das Senden von e-Mail zu initialisieren enthalten (d. h. für den SMTP-Server) und zum Senden der e-Mail-Nachricht.

In einigen Situationen möchten jedoch möglicherweise einige Code ausführen, bevor jede Seite auf der Website ausgeführt wird. Dies ist nützlich für das Festlegen von Werten, die an einer beliebigen Stelle in der Website verwendet werden können (so genannte *globale Werte*.) Beispielsweise erfordern einige Hilfsprogramme Sie Werte wie e-Mail-Einstellungen oder der Account-Schlüssel angeben. Sie können globale Werte zu diesen Einstellungen hilfreich sein.

Hierzu erstellen eine Seite mit dem Namen  *\_AppStart.cshtml* im Stammverzeichnis der Website. Wenn auf dieser Seite vorhanden ist, wird eine andere Seite in der Website angefordert wird zum erste Mal ausgeführt. Daher ist es eine gute zum Ausführen von Code, um globale Werte festzulegen. (Da  *\_AppStart.cshtml* wurde von einem Unterstrich als Präfix, ASP.NET wird nicht die Seite in einem Browser senden, auch wenn es von Benutzern direkt angefordert.)

Im folgenden Diagramm wird gezeigt, wie die  *\_AppStart.cshtml* Seite funktioniert. Wenn eine Anforderung für eine Seite eingeht, und ist dies die erste Anforderung für eine beliebige Seite in der Website, die ASP.NET prüft zunächst, ob eine  *\_AppStart.cshtml* Seite vorhanden ist. Wenn dies der Fall ist, wird der code in der  *\_AppStart.cshtml* Seite ausgeführt wird, und klicken Sie dann auf die angeforderte Seite ausgeführt wird.

![[Image]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Festlegen von globalen Werten für Ihre Website

1. Erstellen Sie im Stammordner einer WebMatrix-Website, eine Datei namens  *\_AppStart.cshtml*. Die Datei muss sich im Stammverzeichnis der Website.
2. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Dieser Code speichert einen Wert in der `AppState` Wörterbuch, das automatisch für alle Seiten der Website verfügbar ist. Beachten Sie, dass die  *\_AppStart.cshtml* Datei weist kein Markup darin. Die Seite wird führen Sie den Code, und klicken Sie dann auf der Seite, die ursprünglich angefordert wurde geleitet.

    > [!NOTE]
    > Seien Sie vorsichtig, wenn Sie Code, in Einfügen der  *\_AppStart.cshtml* Datei. Wenn beim Auftreten von Fehlern im Code in der  *\_AppStart.cshtml* Datei, die Website wird nicht gestartet.
3. Erstellen Sie eine neue Seite mit dem Namen in den Stammordner *AppName.cshtml*.
4. Ersetzen Sie den Standard-Markup und den Code durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Dieser Code extrahiert den Wert aus der `AppState` -Objekt, das Sie, in Festlegen der  *\_AppStart.cshtml* Seite.
5. Führen Sie die *AppName.cshtml* Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Die Seite zeigt den globalen Wert. 

    ![[Image]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Festlegen von Werten für Hilfsprogramme

Eine gute Verwendung für die  *\_AppStart.cshtml* Datei wird zum Festlegen von Werten für Hilfsprogramme, die Sie verwenden, an Ihrem Standort und initialisiert werden. Typische Beispiele dafür sind e-Mail-Einstellungen für die `WebMail` -Hilfsobjekt und die privaten und öffentlichen Schlüssel für die `ReCaptcha` Helper. In diesen Fällen können Sie festlegen, nach die Werten in der  *\_AppStart.cshtml* und legen Sie dann sie sind bereits für alle Seiten auf Ihrer Website.

Dieses Verfahren wird gezeigt, wie Sie festlegen `WebMail` Einstellungen Global. (Weitere Informationen zum Verwenden der `WebMail` Helper, finden Sie unter [E-Mail hinzufügen, um eine ASP.NET Web Pages-Website](../getting-started/11-adding-email-to-your-web-site.md).)

1. Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), wenn Sie bereits hinzugefügt haben.
2. Wenn Sie bereits besitzen eine  *\_AppStart.cshtml* Datei, die im Stammordner einer Website erstellen Sie eine Datei namens  *\_AppStart.cshtml*.
3. Fügen Sie die folgenden `WebMail` Einstellungen für die  *\_AppStart.cshtml* Datei: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Ändern Sie die folgenden e-Mail-verwandte Einstellungen im Code:

   - Legen Sie `your-SMTP-host` auf den Namen des SMTP-Servers, die Sie können zugreifen.
   - Legen Sie `your-user-name-here` mit dem Benutzernamen für Ihr SMTP-Server-Konto.
   - Legen Sie `your-account-password` auf das Kennwort für Ihr SMTP-Server-Konto.
   - Legen Sie `your-email-address-here` an Ihre eigene e-Mail-Adresse. Dies ist die e-Mail-Adresse, der aus die Nachricht gesendet wird. (Einige e-Mail-Anbieter nicht können Sie angeben, ein anderes `From` behandeln und Ihren Benutzernamen als verwendet die `From` Adresse.)

     Weitere Informationen zu SMTP-Einstellungen finden Sie unter [Konfigurieren von e-Mail-Einstellungen](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) im Artikel [Senden von E-Mails aus einer Website für ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) und [Probleme beim Senden von e-Mail-](https://go.microsoft.com/fwlink/?LinkId=253001#email)in der [ASP.NET Web Pages (Razor) Handbuch zur Problembehandlung](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Speichern Sie die  *\_AppStart.cshtml* Datei, und schließen Sie es.
5. Erstellen Sie im Stammordner einer Website, die neue Seite mit dem Namen *TestEmail.cshtml*.
6. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Führen Sie die *TestEmail.cshtml* Seite in einem Browser.
8. Füllen Sie die Felder an, selbst eine e-Mail-Nachricht zu senden, und klicken Sie dann auf **senden**.
9. Überprüfen Sie Ihre e-Mail, um sicherzustellen, dass Sie die Meldung erhalten haben.

Wichtiger Teil dieses Beispiels ist, die die Einstellungen, die Sie in der Regel nicht ändern – z. B. den Namen des SMTP-Server und Ihre e-Mail-Anmeldeinformationen – festgelegt sind, der  *\_AppStart.cshtml* Datei. Auf diese Weise müssen Sie richten sie erneut auf den einzelnen Seiten, in denen Sie e-Mail senden. (Wenn Sie diese Einstellungen nicht ändern müssen, Sie sie einzeln auf einer Seite können zwar festlegen.) Auf der Seite legen Sie nur die Werte, die in der Regel jedes Mal, z. B. dem Empfänger, und der Text der e-Mail-Nachricht ändern.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Ausführen von Code vor und nach Dateien in einem Ordner

Wie Sie verwenden können,  *\_AppStart.cshtml* zum Schreiben von Code vor dem Ausführen von Seiten auf der Website können Code schreiben, der vor (und nach) ausgeführt wird eine andere Seite in einem bestimmten Ordner ausführen. Dies ist nützlich, z. B. Layoutseite für alle Seiten in einem Ordner oder zur Überprüfung, die ein Benutzer angemeldet ist, bevor Sie mit einer Seite in den Ordner festlegen.

Für Seiten in bestimmten Ordnern können Sie erstellen Code in einer Datei namens  *\_PageStart.cshtml*. Im folgenden Diagramm wird gezeigt, wie die  *\_PageStart.cshtml* Seite funktioniert. Wenn eine Anforderung für eine Seite eingehen, ASP.NET zuerst überprüft, ob eine  *\_AppStart.cshtml* Seite und, ausgeführt wird. ASP.NET überprüft, ob es ist ein  *\_PageStart.cshtml* Seite, und wenn dies der Fall ist, ausgeführt wird. Anschließend wird die angeforderte Seite ausgeführt.

Innerhalb der  *\_PageStart.cshtml* Seite, Sie können angeben, wo während der Verarbeitung, Sie die angeforderte Seite einschließlich der Ausführung möchten, einer `RunPage` Methode. Auf diese Weise können Sie Code ausführen, bevor die angeforderte Seite ausgeführt wird und dann danach erneut aus. Wenn Sie nicht einschließen `RunPage`, den Code in  *\_PageStart.cshtml* ausgeführt wird, und klicken Sie dann auf die angeforderte Seite automatisch ausgeführt wird.

![[Image]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET ermöglicht die Erstellung eine Hierarchie von  *\_PageStart.cshtml* Dateien. Sie setzen eine  *\_PageStart.cshtml* -Datei im Stammverzeichnis der Website und in jedem Unterordner. Wenn eine Seite angefordert wird, die  *\_PageStart.cshtml* Datei an die oberste Ebene (am nächsten ist das Stammverzeichnis der Site) ausgeführt wird, gefolgt von der  *\_PageStart.cshtml* Datei in den nächsten Unterordner, und so weiter unten die Unterordnerstruktur, bis die Anforderung den Ordner erreicht, der die angeforderte Seite enthält. Nach dem entsprechenden  *\_PageStart.cshtml* Dateien ausgeführt wurden, die angeforderte Seite ausgeführt wird.

Angenommen, Sie müssen möglicherweise die folgende Kombination von  *\_PageStart.cshtml* Dateien und *Default.cshtml* Datei:

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Bei der Ausführung */myfolder/default.cshtml*, sehen Sie Folgendes:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Initialisierungscode für alle Seiten in einem Ordner ausführen

Eine gute Verwendung für  *\_PageStart.cshtml* initialisiert werden, die gleichen Layoutseite für alle Dateien in einem einzigen Ordner für Dateien verwendet wird.

1. In den Stammordner, erstellen Sie einen neuen Ordner namens *InitPages*.
2. In der *InitPages* Ordner der Website, erstellen Sie eine Datei namens  *\_PageStart.cshtml* und Ersetzen Sie den Standardcode und den Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Erstellen Sie im Stammverzeichnis der Website, die einen Ordner namens *Shared*.
4. In der *Shared* Ordner, erstellen Sie eine Datei mit dem Namen  *\_Layout1.cshtml* und Ersetzen Sie den Standardcode und den Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. In der *InitPages* Ordner, erstellen Sie eine Datei mit dem Namen *Content1.cshtml* und Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. In der *InitPages* Ordner, erstellen Sie eine andere Datei mit dem Namen *Content2.cshtml* und Ersetzen Sie den Standardcode durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Führen Sie *Content1.cshtml* in einem Browser. 

    ![[Image]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Wenn die *Content1.cshtml* Seite ausgeführt wird, die  *\_PageStart.cshtml* Dateisätzen `Layout` und setzt auch `PageData["MyBackground"]` eine Farbe. In *Content1.cshtml*, das Layout und die Farbe angewendet werden.
8. Anzeige *Content2.cshtml* in einem Browser. 

    Das Layout ist identisch, da beide Seiten den gleichen Layoutseite und die Farbe verwenden, wie in initialisiert  *\_PageStart.cshtml*.

## <a name="using-pagestartcshtml-to-handle-errors"></a>Mit \_PageStart.cshtml zur Fehlerbehandlung

Verwenden Sie einen anderen gut für die  *\_PageStart.cshtml* Datei ist, erstellen Sie eine Möglichkeit zum Verarbeiten von Programmierfehlern (Ausnahmen), die in einem auftreten *cshtml* Seite in einem Ordner. In diesem Beispiel wird gezeigt, eine Möglichkeit hierfür.

1. Erstellen Sie im Stammordner, einen Ordner namens *InitCatch*.
2. In der *InitCatch* Ordner der Website, erstellen Sie eine Datei namens  *\_PageStart.cshtml* und Ersetzen Sie das vorhandene Markup und Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    In diesem Code, versuchen Sie es unter der angeforderten Seite explizit durch Aufrufen der `RunPage` Methode innerhalb einer `try` Block. Wenn beim Auftreten von Fehlern Programmierung in der angeforderten Seite, darin den Code der `catch` -Block ausgeführt. In diesem Fall leitet der Code auf eine Seite (*Error.cshtml*) und übergibt den Namen der Datei, die als Teil der URL der Fehler aufgetreten ist. (Sie müssen die Seite in Kürze erstellen.)
3. In der *InitCatch* Ordner der Website, erstellen Sie eine Datei namens *Exception.cshtml* und Ersetzen Sie das vorhandene Markup und Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Für die Zwecke dieses Beispiels wird auf dieser Seite tun absichtlich erstellen einen Fehler beim Versuch, eine Datenbankdatei zu öffnen, die nicht vorhanden.
4. Erstellen Sie im Stammordner, eine Datei namens *Error.cshtml* und Ersetzen Sie das vorhandene Markup und Code durch Folgendes: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Auf dieser Seite des Ausdrucks `@Request["source"]` Ruft den Wert aus der URL ab und zeigt ihn.
5. Klicken Sie auf der Symbolleiste auf **speichern**.
6. Führen Sie *Exception.cshtml* in einem Browser. 

    ![[Image]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Da ein Fehler, in auftritt *Exception.cshtml*, die  *\_PageStart.cshtml* Seite leitet an die *Error.cshtml* -Datei, die die Meldung angezeigt.

    Weitere Informationen zu Ausnahmen finden Sie unter [Einführung in ASP.NET Web Pages Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>Mit \_PageStart.cshtml zum Einschränken des Zugriffs auf Ordner

Sie können auch die  *\_PageStart.cshtml* Datei, um den Zugriff auf alle Dateien in einem Ordner zu beschränken.

1. In WebMatrix Erstellen einer neuen Website mithilfe der **Websitevorlage aus** Option.
2. Wählen Sie aus den verfügbaren Vorlagen **Starter Site**.
3. Erstellen Sie im Stammordner, einen Ordner namens *AuthenticatedContent*.
4. In der *AuthenticatedContent* Ordner, erstellen Sie eine Datei mit dem Namen  *\_PageStart.cshtml* und Ersetzen Sie das vorhandene Markup und Code durch Folgendes: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Der Code beginnt mit der verhindert, dass alle Dateien im Ordner "" zwischengespeichert werden. (Dies ist für Szenarien wie öffentliche Computer, die ein Benutzer zwischengespeicherte Seiten für den nächsten Benutzer verfügbar sein soll nicht erforderlich.) Als Nächstes bestimmt den Code an, ob der Benutzer auf die Website angemeldet hat, bevor sie eine der Seiten in den Ordner anzeigen können. Wenn der Benutzer nicht angemeldet ist, leitet der Code der Anmeldeseite. Die Anmeldeseite kann den Benutzer auf der Seite, die ursprünglich angefordert wurde, wenn Sie einen Abfrage-Zeichenfolgenwert namens einschließen zurückgeben `ReturnUrl`.
5. Erstellen Sie eine neue Seite in der *AuthenticatedContent* Ordner mit dem Namen *Page.cshtml*.
6. Ersetzen Sie den Standardcode durch Folgendes:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Führen Sie *Page.cshtml* in einem Browser. Der Code, den Sie zu einer Anmeldeseite weitergeleitet. Sie müssen vor der Anmeldung registrieren. Nachdem Sie registriert und angemeldet haben, können zur Seite navigieren und dessen Inhalt anzeigen.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Einführung in ASP.NET Web Pages Programmierung mithilfe von Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587)
