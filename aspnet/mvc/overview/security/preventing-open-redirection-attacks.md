---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Verhindern von Angriffen von Open Umleitung (c#) | Microsoft Docs
author: jongalloway
description: "In diesem Lernprogramm wird erläutert, wie Sie in Ihrer ASP.NET MVC-Anwendung öffnen Umleitung Angriffe verhindern können. In diesem Lernprogramm wird erläutert, die Änderungen, die vorgenommen wurden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 97e0aacbf21914bf95f01019cf4dcc9e7ca1c4be
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="preventing-open-redirection-attacks-c"></a>Verhindern von Angriffen von Open Umleitung (c#)
====================
durch [Jon Galloway](https://github.com/jongalloway)

> In diesem Lernprogramm wird erläutert, wie Sie in Ihrer ASP.NET MVC-Anwendung öffnen Umleitung Angriffe verhindern können. In diesem Lernprogramm wird erläutert, die Änderungen, die in der AccountController in ASP.NET MVC 3 vorgenommen wurden und veranschaulicht, wie Sie diese Änderungen in Ihrer vorhandenen ASP.NET MVC 1,0 und 2 Anwendungen anwenden können.


## <a name="what-is-an-open-redirection-attack"></a>Was ist Angriff Umleitung öffnen?

Alle Webanwendung, die an eine URL, die über die Anforderung angegeben wird umleitet, wie z. B. die Abfragezeichenfolge oder Formular Daten möglicherweise manipuliert werden können Benutzer an eine externe, böswillige URL umleiten. Diese Manipulation wird einen öffnen-Redirect-Angriff bezeichnet.

Wenn Sie die Anwendungslogik zu einer angegebenen URL umleitet, müssen Sie sicherstellen, dass die umleitungs-URL nicht manipuliert wurde. Der in der standardmäßigen AccountController für ASP.NET MVC 1,0 und ASP.NET MVC 2 verwendete Anmeldename ist anfällig für Angriffe Umleitung zu öffnen. Glücklicherweise ist es einfach, aktualisieren Sie Ihre vorhandenen Anwendungen, um die Korrekturen von ASP.NET MVC 3 Preview zu verwenden.

Um die Sicherheitslücke zu verstehen, sehen wir uns die Funktionsweise der umleitungs für die Anmeldung in einer ASP.NET MVC 2-Webanwendung Standardprojekt. In dieser Anwendung wird bei dem Versuch, eine Controlleraktion besuchen das Attribut [Authorize] ist nicht autorisierte Benutzer anzuzeigende /Account/LogOn umleiten. Diese Umleitung zu /Account/LogOn, schließt einen ReturnUrl Querystring-Parameter, damit der Benutzer an die ursprünglich angeforderte URL zurückgegeben werden kann, nachdem sie erfolgreich angemeldet haben.

Im folgenden Screenshot sehen wir, dass eine Umleitung Versuch, den Zugriff auf /Account/ChangePassword-Sicht, wenn Sie nicht angemeldet sind, /Account/LogOn führt? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Abbildung 01**: Anmeldeseite mit einem geöffneten Umleitung

Da ReturnUrl Querystring-Parameter nicht überprüft wird, kann ein Angreifer, um eine beliebige URL-Adresse in der Parameter für die Durchführung ein Angriffs öffnen Umleitung einfügen ändern. Um dies zu demonstrieren, können wir den ReturnUrl-Parameter, um ändern [http://bing.com](http://bing.com), sodass die resultierenden Anmelde-URL/Account/LogOn? ReturnUrl = http://www.bing.com/. Nach der Anmeldung wurde erfolgreich an den Standort, werden wir umgeleitet [http://bing.com](http://bing.com). Da diese Umleitung nicht überprüft wird, konnte er stattdessen auf eine bösartige Website verweisen, die versucht, den Benutzer zu bringen.

### <a name="a-more-complex-open-redirection-attack"></a>Eine komplexere öffnen Umleitung-Angriff

Open Umleitung Angriffe sind besonders riskant, daran, dass ein Angreifer bekannt, dass wir versuchen, sich in einer bestimmten Website, was uns anfällig macht eine [Phishing-Attacke](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Beispielsweise könnte ein Angreifer bei einem Versuch, ihre Kennwörter erfassen böswillige-e-Mails Benutzern der Website senden. Sehen wir uns, wie dies auf der Website NerdDinner erfolgen kann. (Beachten Sie, dass zum Schutz vor Angriffen durch Öffnen Umleitung live NerdDinner-Standort aktualisiert wurde.)

Zuerst sendet ein Angreifer uns einen Link zur Anmeldeseite auf NerdDinner, die eine Umleitung an ihrer gefälschten Seite enthält:

[http://NerdDinner.com/Account/Logon?returnUrl=http://nerddiner.com/Account/Logon](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Beachten Sie, dass die Rückgabe-URL verweist auf nerddiner.com, der eine "n" fehlt in der Word-Dinner. In diesem Beispiel ist dies eine Domäne, die der Angreifer steuert. Wenn wir den oben aufgeführten Link zugreifen, haben wir der legitime NerdDinner.com Anmeldeseite übernommen.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Abbildung 02**: NerdDinner-Anmeldeseite mit einem geöffneten Umleitung

Wenn es sich ordnungsgemäß anmelden, leitet der ASP.NET MVC-AccountController Anmeldung Aktion uns an die URL, die in der ReturnUrl Querystring-Parameter angegeben. In diesem Fall entspricht Sie der URL, mit denen der Angreifer hat, also [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn). Wenn wir äußerst weiter sind, es ist sehr wahrscheinlich, dass wir hierzu bemerken, vor allem, da Sie vorsichtig, um sicherzustellen, dass der Angreifer wurde sieht ihre gefälschte Seite genau der legitime Anmeldeseite. Diese Anmeldeseite enthält Fehler anfordern, dass wir erneut anmelden. Grammatik einer us, haben wir müssen das Kennwort falsch eingegeben.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Abbildung 03**: gefälschtes NerdDinner-Anmeldebildschirm

Wenn wir unsere Benutzernamen und das Kennwort erneut ein, wird die gefälschten Anmeldeseite speichert die Informationen und sendet uns zurück an der legitime NerdDinner.com-Website. An diesem Punkt hat die NerdDinner.com-Website bereits us, authentifiziert damit gefälschten Anmeldungsseite direkt zu dieser Seite umgeleitet werden kann. Das Endergebnis ist, dass der Angreifer verfügt über unsere Benutzername und Kennwort, und wir sind nicht bekannt, dass wir es für sie bereitgestellt haben.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Die anfällig für Code in der AccountController Anmeldung Aktion ansehen

Der Code für die Anmeldung Aktion in einer ASP.NET MVC 2-Anwendung wird unten gezeigt. Beachten Sie, dass nach einer erfolgreichen Anmeldung der Controller eine Umleitung an den ReturnUrl aus zurückgibt. Sie können sehen, dass keine Validierung wird anhand der ReturnUrl Parameter ausgeführt wird.

**Auflisten von 1 – ASP.NET MVC 2-Anmeldung in Aktion`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Jetzt sehen wir uns die Änderungen an der Anmeldung für ASP.NET MVC 3-Aktion. Dieser Code wurde geändert, um der ReturnUrl-Parameter zu überprüfen, indem Sie eine neue Methode aufrufen, in der System.Web.Mvc.Url Hilfsklasse, die mit dem Namen `IsLocalUrl()`.

**Auflisten von 2 – ASP.NET MVC 3-Anmeldung in Aktion`AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Dies wurde geändert, um den URL-Rückgabeparameter zu überprüfen, indem Sie eine neue Methode aufrufen, in der Hilfsklasse System.Web.Mvc.Url `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Schutz der ASP.NET MVC 1,0 und MVC 2 Anwendungen

Wir können die ASP.NET MVC 3-Änderungen in unseren vorhandenen ASP.NET MVC 1,0 und 2 Anwendungen nutzen durch Hinzufügen der IsLocalUrl()-Hilfsmethode und aktualisieren die Aktion der Anmeldung die ReturnUrl Parameter zu überprüfen.

Die UrlHelper IsLocalUrl()-Methode, die tatsächlich nur Aufrufe in einer Methode in System.Web.WebPages, als diese Überprüfung wird auch von ASP.NET Web Pages-Anwendungen verwendet werden.

**3 – der IsLocalUrl()-Methode aus der ASP.NET MVC 3-UrlHelper auflisten`class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Die IsUrlLocalToHost-Methode enthält die tatsächliche Überprüfungslogik wie im Codebeispiel 4 dargestellt.

**Auflisten von 4 – IsUrlLocalToHost() Methode der System.Web.WebPages RequestExtensions-Klasse**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

In unserem ASP.NET MVC 1.0 oder 2-Anwendung fügen wir eine IsLocalUrl()-Methode auf die AccountController, Sie sind jedoch nach Möglichkeit auf einer separaten Hilfsklasse hinzufügen. Wir werden zwei kleinere Änderungen auf die ASP.NET MVC 3-Version von IsLocalUrl() stellen, damit er innerhalb der AccountController funktioniert. Zunächst müssen wir ändern Sie ihn von einer öffentlichen Methode zu einer privaten Methode seit öffentliche Methoden in Controllern als Controlleraktionen zugegriffen werden können. Zweitens müssen wir den Aufruf ändern, der den URL-Host für den Anwendungshost überprüft. Dass der Aufruf wird von einem lokalen RequestContext verwenden Feld die UrlHelper-Klasse. Anstatt diese. RequestContext.HttpContext.Request.Url.Host, wird diese verwendet. Request.Url.Host. Der folgende Code zeigt die geänderte IsLocalUrl()-Methode für die Verwendung mit einer Controllerklasse in ASP.NET MVC 1,0 und 2.

**Auflisten von 5 – IsLocalUrl()-Methode, die für die Verwendung mit einer MVC-Controller-Klasse geändert wird**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Nun, dass die Methode IsLocalUrl() vorhanden ist, können wir über unser Anmeldung Aktion zum Überprüfen des Parameters ReturnUrl aufrufen wie im folgenden Code gezeigt.

**Auflisten von 6 – aktualisierte Anmeldemethode die ReturnUrl-Parameter überprüft.**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Wir können jetzt Angriff öffnen Umleitung testen, indem versucht wird, melden Sie sich mit einem externen Rückgabe-URL. Ermöglicht die Verwendung/Account/anmelden? ReturnUrl = Sie http://www.bing.com/ erneut aus.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Abbildung 04**: Testen der aktualisierten LogOn-Aktion

Nach der erfolgreichen Anmeldung im, werden wir Home/Index-Controlleraktion, anstatt die externe URL umgeleitet.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Abbildung 05**: Öffnen Umleitung Angriff entgegengewirkt

## <a name="summary"></a>Zusammenfassung

Open-Redirect-Angriffen können auftreten, wenn Redirection URLs als Parameter in der URL für eine Anwendung übergeben werden. Die ASP.NET MVC 3, Code zum Schutz vor umfasst die Vorlage, öffnen Umleitung Angriffe. Sie können diesen Code mit einigen Änderungen in ASP.NET MVC 1.0 und 2 Anwendungen hinzufügen. Um beim Anmelden in ASP.NET 1.0 und 2 Anwendungen öffnen Umleitung vor Angriffen zu schützen, fügen Sie eine IsLocalUrl()-Methode, und Überprüfen des ReturnUrl-Parameters in der Aktion für die Anmeldung.
