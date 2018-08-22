---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Verwenden ein CAPTCHA, um zu verhindern, dass Bots mit Ihrer ASP.NET-Webanwendung Razor) Standort | Microsoft-Dokumentation
author: microsoft
description: In diesem Artikel wird erläutert, wie ReCaptcha (Dies ist eine Sicherheitsmaßnahme) verwenden, um zu verhindern, dass die automatisierten Programmen (Bots) Ausführen von Aufgaben in einer ASP.NET Web Pages (Razor) wir...
ms.author: riande
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: dc014f42490327743764787d58c613b7caa89f1f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827200"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Verwenden ein CAPTCHA, um zu verhindern, dass Bots mit Ihrer ASP.NET-Webanwendung Razor) Standort
====================
durch [Microsoft](https://github.com/microsoft)

> In diesem Artikel wird erläutert, wie ReCaptcha (Dies ist eine Sicherheitsmaßnahme) verwenden, um zu verhindern, dass die automatisierten Programmen (Bots) Ausführen von Aufgaben in einer ASP.NET Web Pages (Razor)-Website wird.
> 
> **Sie lernen Folgendes:** 
> 
> - Wie Ihre Website einen CAPTCHA-Test hinzugefügt.
> 
> Dies sind die Funktionen von ASP.NET in diesem Artikel:
> 
> - Die `ReCaptcha` Helper.
> 
> > [!NOTE]
> > Die Informationen in diesem Artikel gelten für ASP.NET Web Pages-1.0- und Web Pages 2.


## <a name="about-captchas"></a>Informationen zu CAPTCHAs

Geben Sie jedes Mal, die Sie an Ihrem Standort oder sogar nur Personen registrieren können, einen Namen und eine URL (z.B. für einen Blogkommentar), erhalten Sie möglicherweise eine Flut von gefälschten Namen. Diese werden häufig von automatisierten Programmen (Bots) beibehalten, die versuchen, die URLs auf jeder Website verlassen, die sie finden können. (Eine allgemeine Motivation ist die URLs der Produkte für den Verkauf veröffentlichen.)

Sie können sicherstellen, dass ein Benutzer mit realen Person "und" nicht in einem Computerprogramm ist eine *CAPTCHA* zum valideren von Benutzern beim Registrieren oder andernfalls geben Sie ihren Namen und den Standort. CAPTCHA steht für Öffentliche Turing vollständig automatisierten Test anzuweisen, Computer und Menschen auseinander liegen. Ein CAPTCHA ist eine *Abfrage / Rückmeldung-* Test in der der Benutzer aufgefordert, etwas zu tun, die einfach für eine Person Sie jedoch für eine automatisierte Programm schwierig ist. Die am häufigsten verwendete Typ der CAPTCHA ist, in dem Sie finden Sie einige verzerrten Buchstaben und werden zur Eingabe aufgefordert. (Die Verzerrung sollte um für Bots, die Buchstaben entschlüsseln schwer zu erleichtern.)

## <a name="adding-a-recaptcha-test"></a>Hinzufügen eines ReCaptcha-Tests

In ASP.NET-Seiten können Sie mithilfe der `ReCaptcha` Hilfsmethode zum einen CAPTCHA-Test zu rendern, die basierend auf den ReCaptcha-Dienst ([http://recaptcha.net](http://recaptcha.net)). Die `ReCaptcha` Hilfsprogramm zeigt ein Bild von zwei verzerrt Wörtern, die Benutzer sich nicht ordnungsgemäß eingeben, bevor die Seite überprüft wird. Die Antwort des Benutzers wird durch den Dienst ReCaptcha.Net überprüft.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Registrieren Sie Ihre Website in ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Wenn Sie die Registrierung abgeschlossen haben, erhalten Sie einen öffentlichen Schlüssel und einen privaten Schlüssel.
2. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
3. Wenn Sie noch keinem  *\_AppStart.cshtml* Datei, erstellen Sie eine Datei namens im Stammordner einer Website  *\_AppStart.cshtml*.
4. Fügen Sie die folgenden `Recaptcha` Helper-Einstellungen in der  *\_AppStart.cshtml* Datei: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Legen Sie die `PublicKey` und `PrivateKey` Eigenschaften mit Ihren eigenen öffentlichen und privaten Schlüsseln.
6. Speichern Sie die  *\_AppStart.cshtml* Datei, und schließen Sie es.
7. Erstellen Sie im Stammordner einer Website, die neue Seite, die mit dem Namen *Recaptcha.cshtml*.
8. Ersetzen Sie den vorhandenen Inhalt durch Folgendes: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Führen Sie die *Recaptcha.cshtml* Seite in einem Browser. Wenn die `PrivateKey` Wert gültig ist, die Seite zeigt das ReCaptcha-Steuerelement und eine Schaltfläche. Wenn Sie die Schlüssel nicht global in festgelegt hatten  *\_AppStart.html*, die Seite wird eine Fehlermeldung angezeigt. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Geben Sie die Wörter für den Test ein. Wenn Sie den Test ReCaptcha übergeben, sehen Sie eine entsprechende Meldung. Andernfalls eine Fehlermeldung angezeigt, und die ReCaptcha-Steuerelement wird erneut angezeigt.

> [!NOTE]
> Wenn der Computer in einer Domäne, die Proxyserver verwendet befindet, müssen Sie möglicherweise konfigurieren die `defaultproxy` Element der *"Web.config"* Datei. Das folgende Beispiel zeigt eine *"Web.config"* -Datei mit den `defaultproxy` Element so konfiguriert, dass den ReCaptcha-Dienst funktioniert.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


- [Anpassen von standortweite Verhalten für ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha-Website](https://www.google.com/recaptcha)
