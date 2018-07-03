---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: Verhindern von Angriffen der offene Umleitungen (c#) | Microsoft-Dokumentation
author: jongalloway
description: In diesem Tutorial wird erläutert, wie Sie offene umleitungen-Angriffen in ASP.NET MVC-Anwendungen verhindern können. In diesem Tutorial wird erläutert, die Änderungen, die vorgenommen wurden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 27921e23d38d34332b81fb85dcc795c8f9ff0352
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375469"
---
<a name="preventing-open-redirection-attacks-c"></a>Verhindern von Angriffen der offene Umleitungen (c#)
====================
durch [Jon Galloway](https://github.com/jongalloway)

> In diesem Tutorial wird erläutert, wie Sie offene umleitungen-Angriffen in ASP.NET MVC-Anwendungen verhindern können. Dieses Tutorial erläutert die Änderungen an, die in der AccountController-Komponente in ASP.NET MVC 3 vorgenommen wurden, und es wird veranschaulicht, wie Sie diese Änderungen in Ihrer vorhandenen ASP.NET MVC 1.0 und 2-Anwendungen anwenden können.


## <a name="what-is-an-open-redirection-attack"></a>Was ist ein Open Redirect-Angriff?

Jede Webanwendung, die an eine URL, die über die Anforderung angegeben wird umleitet, wie z. B. die Abfragezeichenfolge oder Form-Daten möglicherweise manipuliert werden können Benutzer auf eine externe, böswillige URL umgeleitet wird. Diese Manipulation wird einen offene umleitungen-Angriff bezeichnet.

Wenn Ihre Anwendungslogik an eine angegebene URL umgeleitet wird, müssen Sie sicherstellen, dass die umleitungs-URL nicht manipuliert wurde. Der Standardwert "AccountController" für ASP.NET MVC 1.0 und ASP.NET MVC 2 verwendete Anmeldename ist anfällig für Umleitung Angriffe zu öffnen. Glücklicherweise ist es einfach, Ihre vorhandenen Anwendungen zu verwenden, die Korrekturen von ASP.NET MVC 3 Preview zu aktualisieren.

Um das Sicherheitsrisiko zu verstehen, sehen wir uns die Funktionsweise der der umleitungs für die Anmeldung in einer ASP.NET MVC 2-Webanwendung Standardprojekt. In dieser Anwendung leitet bei dem Versuch, eine Controlleraktion, die die [Authorize]-Attribut aufweist, finden Sie auf nicht autorisierte Benutzer auf die Ansicht /Account/LogOn. Diese Umleitung zu /Account/LogOn enthalten die Querystring-Parameter "ReturnUrl" nachzuweisen, damit der Benutzer zur ursprünglich angeforderten URL zurückgegeben werden kann, nachdem sie sich erfolgreich angemeldet haben.

Im folgenden Screenshot sehen wir, dass die /Account/ChangePassword-Ansicht, wenn Sie nicht angemeldet sind zugreifen auf eine Umleitung zum /Account/LogOn führt? ReturnUrl = % 2fAccount % 2fChangePassword % 2f.

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

**Abbildung 01**: Anmeldeseite durch eine offene umleitungen

Da der Querystring-Parameter von "ReturnUrl" nicht überprüft wird, kann ein Angreifer, um eine beliebige URL-Adresse in der Parameter für das Durchführen eines Angriffs offene umleitungen einfügen ändern. Um dies zu demonstrieren, ändern wir den Parameter "ReturnUrl", um [ http://bing.com ](http://bing.com), damit die resultierende Anmelde-URL/Account/LogOn werden? ReturnUrl =<http://www.bing.com/>. Nach erfolgreicher Anmeldung auf der Website, wir werden zur [ http://bing.com ](http://bing.com). Da diese Umleitung nicht überprüft wird, kann es stattdessen auf eine schädliche Website verweisen, der versucht, den Benutzer dazu zu bringen.

### <a name="a-more-complex-open-redirection-attack"></a>Ein komplexer Open Umleitung-Angriff

Offene umleitungen Angriffe sind besonders gefährlich, da ein Angreifer weiß, dass wir versuchen, sich bei einer bestimmten Website, was uns anfällig macht eine [Phishing-Angriff](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx). Beispielsweise könnte ein Angreifer böswillige-e-Mails für Benutzer der Website Versuch, ihre Kennwörter zu erfassen senden. Sehen wir uns, wie dies auf der Website für die NerdDinner funktionieren würde. (Beachten Sie, dass der NerdDinner-Livewebsite zum Schutz vor Angriffen von offene umleitungen aktualisiert wurde.)

Zuerst sendet ein Angreifer uns einen Link zur Anmeldeseite auf NerdDinner, die eine Umleitung zur Seite mit gefälschten enthält:

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

Beachten Sie, dass die Rückgabe-URL verweist auf nerddiner.com, der eine "n" fehlt, von der Dinner Word. In diesem Beispiel ist dies eine Domäne, die der Angreifer gesteuert. Wenn wir den oben aufgeführten Link zugreifen, sind wir auf die legitime von NerdDinner.com-Anmeldeseite weitergeleitet.

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

**Abbildung 02**: NerdDinner-Anmeldeseite durch eine offene umleitungen

Wenn es richtig angemeldet haben, leitet LogOn-Aktion der ASP.NET MVC AccountController Komponente uns auf die URL in den Querystring-Parameter von "ReturnUrl" nachzuweisen. In diesem Fall ist es die URL, die der Angreifer eingegeben hat, handelt es sich [ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn). Es sei denn, wir sehr wachsames sind, es ist sehr wahrscheinlich, dass wir dies nicht bemerken, vor allem, da der Angreifer sorgfältig wurde, um sicherzustellen, dass sieht die Seite mit ihren gefälschte genau wie die legitime Anmeldeseite ein. Diese Anmeldeseite enthält einen Fehler mit dem anfordern, dass wir erneut anmelden. Umständlich us, haben wir müssen das Kennwort falsch eingegeben.

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

**Abbildung 03**: gefälschte NerdDinner-Anmeldebildschirm

Wenn wir unsere Benutzernamen und das Kennwort erneut ein, wird die gefälschten Anmeldungsseite speichert die Informationen und sendet uns an die legitime von NerdDinner.com-Website. An diesem Punkt war die Website von NerdDinner.com bereits uns also direkt zu dieser Seite die gefälschten Anmeldeseite umgeleitet werden kann. Das Endergebnis ist, dass der Angreifer unser Benutzername und Kennwort hat, und wir sind nicht bekannt, dass wir es für sie bereitgestellt haben.

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a>Betrachten den anfälligen Codes in der Aktion der AccountController-Anmeldung

Der Code für die Anmeldung Aktion in einer ASP.NET MVC 2-Anwendung wird unten dargestellt. Beachten Sie, dass der Controller nach einer erfolgreichen Anmeldung eine Umleitung an die ReturnUrl zurückgibt. Sie können sehen, dass keine Validierung, für den Parameter "ReturnUrl" nachzuweisen durchgeführt wird.

**Codebeispiel 1: ASP.NET MVC 2-Anmeldung in Aktion `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

Jetzt sehen wir uns die Änderungen an der Anmeldung für ASP.NET MVC 3-Aktion. Dieser Code wurde geändert, um den Parameter "ReturnUrl" zu überprüfen, indem Sie eine neue Methode in der System.Web.Mvc.Url-Hilfsklasse, die mit dem Namen aufrufen `IsLocalUrl()`.

**Codebeispiel 2: ASP.NET MVC 3-Anmeldung in Aktion `AccountController.cs`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

Dies wurde geändert, um die URL-Rückgabeparameter zu überprüfen, indem Sie eine neue Methode in der Hilfsklasse System.Web.Mvc.Url Aufrufen `IsLocalUrl()`.

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a>Schützen Ihre ASP.NET MVC 1.0 und MVC 2 Anwendungen

Wir können die ASP.NET MVC 3-Änderungen in unseren vorhandenen ASP.NET MVC 1.0 und 2-Anwendungen nutzen, indem die Hilfsmethode IsLocalUrl() hinzufügen und aktualisieren die Aktion anmelden, um zu überprüfen, ob den Parameter "ReturnUrl" nachzuweisen.

Die UrlHelper IsLocalUrl()-Methode, die tatsächlich nur Aufrufen einer Methode in System.Web.WebPages, als diese Überprüfung wird auch von ASP.NET Web Pages-Anwendungen verwendet werden.

**Codebeispiel 3 – die IsLocalUrl()-Methode aus der ASP.NET MVC 3-UrlHelper `class`**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

Die IsUrlLocalToHost-Methode enthält die tatsächliche Überprüfungslogik, wie in Codebeispiel 4 dargestellt.

**Programmausdruck 4 – IsUrlLocalToHost() Methode System.Web.WebPages zur RequestExtensions-Klasse**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

Klicken Sie in unserer ASP.NET MVC 1.0 oder 2-Anwendung eine Methode IsLocalUrl() AccountController-Komponente hinzugefügt, aber Sie können es nach Möglichkeit auf einer separaten Hilfsklasse hinzufügen. Wir werden zwei kleine Änderungen auf die ASP.NET MVC 3-Version von IsLocalUrl() stellen, damit er innerhalb der AccountController-Komponente verwendet werden. Zuerst ändern wir es von einer öffentlichen Methode um eine private Methode, da öffentliche Methoden in Controllern als Controlleraktionen zugegriffen werden können. Zweitens ändern wir den Aufruf, der die URL-Host für den Anwendungshost überprüft. Dass der Aufruf verwendet eine lokale RequestContext Feld die UrlHelper-Klasse. Anstelle von. RequestContext.HttpContext.Request.Url.Host, wird dies verwendet. Request.Url.Host. Der folgende Code zeigt die geänderte IsLocalUrl()-Methode für die Verwendung mit einer Controllerklasse in ASP.NET MVC 1.0 und 2-Anwendungen.

**Programmausdruck 5 – IsLocalUrl()-Methode, die für die Verwendung mit einer MVC-Controller-Klasse geändert wird**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

Nun, da die IsLocalUrl()-Methode vorhanden ist, können wir es aus unserer Anmeldung Aktion zur Überprüfung des Parameters "ReturnUrl" aufrufen, wie im folgenden Code gezeigt.

**Auflisten von 6 – aktualisierte Anmeldemethode, die den Parameter "ReturnUrl" validiert.**

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

Jetzt können wir einen Angriff offene umleitungen testen, indem versucht wird, melden Sie sich mit einem externen Rückgabe-URL. Wir verwenden /Account/LogOn? ReturnUrl =<http://www.bing.com/> erneut aus.

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

**Abbildung 04**: Testen der aktualisierten LogOn-Aktion

Nach erfolgreicher Anmeldung, werden wir die Home/Index-Controlleraktion, anstatt die externe URL umgeleitet.

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

**Abbildung 05**: zuzusehen offene Umleitungen-Angriff

## <a name="summary"></a>Zusammenfassung

Offene umleitungen Angriffen können auftreten, wenn Redirection URLs als Parameter in der URL für eine Anwendung übergeben werden. Die ASP.NET MVC 3-Vorlage zum Schutz vor enthält öffnen Umleitung-Angriffe. Sie können diesen Code mit einigen Änderungen an ASP.NET MVC 1.0 und 2-Anwendungen hinzufügen. Hinzufügen einer IsLocalUrl()-Methode, und überprüfen Sie den Parameter "ReturnUrl" in der Aktion für die Anmeldung, um offene umleitungen Schutz vor Angriffen bei der Anmeldung von ASP.NET 1.0 und 2-Anwendungen.
