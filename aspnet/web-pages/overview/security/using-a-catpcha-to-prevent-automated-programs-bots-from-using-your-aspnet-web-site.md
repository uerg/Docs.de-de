---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Verwenden ein CAPTCHA, um zu verhindern, dass Bots mithilfe der ASP.NET Web-Razor) Website | Microsoft Docs
author: microsoft
description: "In diesem Artikel wird erläutert, wie ReCaptcha (eine Sicherheitsmaßnahme) verwenden, um zu verhindern, dass automatisierte Programme (Bots) Ausführen von Aufgaben in einer ASP.NET Web Pages (Razor) wir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Verwenden ein CAPTCHA, um zu verhindern, dass Bots mithilfe der ASP.NET Web-Razor) Standort
====================
durch [Microsoft](https://github.com/microsoft)

> In diesem Artikel erläutert, wie ReCaptcha (eine Sicherheitsmaßnahme) verwenden, um zu verhindern, dass automatisierte Programme (Bots) Ausführen von Aufgaben in einer ASP.NET Web Pages (Razor)-Website.
> 
> **Lernen Sie:** 
> 
> - Wie ein CAPTCHA-Prüfung auf Ihrer Website hinzugefügt.
> 
> Dies sind die Funktionen von ASP.NET im Artikel:
> 
> - Die `ReCaptcha` Helper.
> 
> > [!NOTE]
> > Die Informationen in diesem Artikel gelten für ASP.NET Web Pages 1.0 und Web Pages 2.


## <a name="about-captchas"></a>Informationen zu CAPTCHAs

Jedes Mal, die Sie an Ihrem Standort oder auch nur Personen registrieren können Geben Sie einen Namen und eine URL (wie bei einem Blogkommentar), erhalten Sie möglicherweise eine Flut von falschen Namen. Diese werden häufig durch automatisierte Programme (Bots) beibehalten, die versuchen, die URLs auf jeder Website verlassen, die sie finden können. (Eine allgemeine Motivation ist, werden die URLs der Produkte für den Verkauf bereitgestellt.)

Sie können sicherstellen, dass ein Benutzer mit realen Person und nicht in einem Computerprogramm ist ein *CAPTCHA* zum valideren von Benutzern beim Registrieren oder andernfalls geben Sie ihren Namen und den Standort. CAPTCHA steht für Öffentliche Turing vollständig automatisierte Test anzuweisen, Computer und Menschen auseinander. Ein CAPTCHA ist ein *Abfrage / Rückmeldung-* Test in der der Benutzer aufgefordert, eine Aktion, die für eine Person, führen Sie einfach, aber nur schwer automatisiertes Programm zu tun ist. Der am häufigsten verwendete Typ des CAPTCHA ist, in dem Sie einige verzerrten Buchstaben und werden aufgefordert, sie geben. (Die Verzerrung sollte um für den Bots, die Buchstaben zu entschlüsseln zu vereinfachen.)

## <a name="adding-a-recaptcha-test"></a>Hinzufügen eines ReCaptcha-Tests

In ASP.NET-Seiten können Sie die `ReCaptcha` Hilfsmethode zum ein CAPTCHA-Prüfung zu rendern, die basierend auf den ReCaptcha-Dienst ([http://recaptcha.net](http://recaptcha.net)). Die `ReCaptcha` Hilfsprogramm zeigt ein Bild von zwei verzerrt Wörter, die Benutzer müssen ordnungsgemäß eingeben, bevor die Seite überprüft wird. Die Antwort des Benutzers wird vom Dienst ReCaptcha.Net überprüft.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrieren Sie Ihre Website unter ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Wenn Sie die Registrierung abgeschlossen haben, erhalten Sie einen öffentlichen Schlüssel und einem privaten Schlüssel.
2. Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
3. Wenn Sie bereits besitzen eine  *\_AppStart.cshtml* Datei, die im Stammordner einer Website erstellen Sie eine Datei namens  *\_AppStart.cshtml*.
4. Fügen Sie die folgenden `Recaptcha` Helper-Einstellungen in der  *\_AppStart.cshtml* Datei: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Legen Sie die `PublicKey` und `PrivateKey` Eigenschaften mit Ihren eigenen öffentlichen und privaten Schlüssel.
6. Speichern Sie die  *\_AppStart.cshtml* Datei, und schließen Sie es.
7. Erstellen Sie im Stammordner einer Website, die neue Seite mit dem Namen *Recaptcha.cshtml*.
8. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Führen Sie die *Recaptcha.cshtml* Seite in einem Browser. Wenn die `PrivateKey` Wert gültig ist, die Seite zeigt die ReCaptcha-Steuerelement und einer Schaltfläche. Wenn Sie die Schlüssel nicht global in festlegen  *\_AppStart.html*, die Seite würde ein Fehler angezeigt. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Geben Sie die Wörter für den Test aus. Wenn Sie den Test ReCaptcha übergeben, wird eine entsprechende Nachricht angezeigt. Andernfalls eine Fehlermeldung angezeigt, und die ReCaptcha-Steuerelement wird erneut angezeigt.

> [!NOTE]
> Wenn der Computer in einer Domäne, die Proxyserver verwendet befindet, müssen Sie möglicherweise Konfigurieren der `defaultproxy` Element von der *"Web.config"* Datei. Das folgende Beispiel zeigt eine *"Web.config"* -Datei mit den `defaultproxy` Element so konfiguriert, dass den ReCaptcha-Dienst funktioniert.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


- [Anpassen von standortweite Verhalten für ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha-Website](https://www.google.com/recaptcha)
