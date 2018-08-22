---
uid: single-page-application/overview/templates/backbonejs-template
title: Backbone.js-Vorlage | Microsoft-Dokumentation
author: madskristensen
description: Backbone.js-SPA-Vorlage
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 325c4f5370340b2e223521fada77cf0e78a67b5b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830356"
---
<a name="backbone-template"></a>Backbone.js-Vorlage
====================
durch [Mads Kristensen](https://github.com/madskristensen)

> Der Rückgrat SPA-Vorlage wurde von Kazi Manzur Rashid geschrieben.
> 
> [Die Backbone.js SPA-Vorlage herunterladen](https://go.microsoft.com/fwlink/?LinkId=293631)


Die Backbone.js SPA-Vorlage wurde entwickelt, Sie beim schnellen Erstellen von interaktiven clientseitigen Web-apps mit Einstieg [Backbone.js.](http://backbonejs.org/)

Die Vorlage stellt einen ersten Gerüst für das Entwickeln einer Backbone.js-Anwendung in ASP.NET MVC bereit. Standardmäßig wird die grundlegende Anmeldefunktionalität ermöglichen, einschließlich Registrierung, Anmeldung, kennwortzurücksetzung durch Benutzer und Bestätigung durch den Benutzer mit basic-e-Mail-Vorlagen.

Anforderungen:

- [Update für ASP.NET und Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Erstellen Sie ein Projekt der Rückgrat-Vorlage

Herunterladen Sie und installieren Sie die Vorlage, indem Sie auf die Schaltfläche "herunterladen" oben. Die Vorlage wird als eine Datei für Visual Studio-Erweiterung (VSIX) verpackt. Sie müssen möglicherweise Visual Studio neu starten.

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

![](backbonejs-template/_static/image1.png)

In der **neues Projekt** Assistenten, wählen Backbone.js SPA-Projekt.

![](backbonejs-template/_static/image2.png)

Drücken Sie STRG + F5 zum Erstellen und Ausführen der Anwendungs ohne Debuggen, oder drücken Sie F5, um mit Debuggen auszuführen.

![](backbonejs-template/_static/image3.png)

Durch Klicken auf "Mein Konto" wird die Anmeldeseite:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Exemplarische Vorgehensweise: Client-Code

Lassen Sie uns beginnt bei der Clientseite. Die Client-Anwendung-Skripts befinden sich im Ordner "~/Scripts/application". Die Anwendung hat [TypeScript](http://www.typescriptlang.org/) (TS-Dateien) die in JavaScript (.js-Dateien) kompiliert werden.

**Anwendung**

`Application` wird in application.ts definiert. Dieses Objekt initialisiert die Anwendung und fungiert als Root-Namespace. Es verwaltet die Konfiguration und Status-Informationen, die der Anwendung gemeinsam verwendet wird, wie z. B., ob der Benutzer angemeldet ist.

Die `application.start` Methode erstellt den modalen Ansichten verwendet werden, und fügt der Ereignishandler für Ereignisse auf Anwendungsebene, wie z. B. der Benutzeranmeldung. Als Nächstes den Standardrouter erstellt und überprüft, ob alle Client-Side-URL angegeben ist. Wenn nicht, um die Standard-Url umgeleitet (#! /).

**Ereignisse**

Ereignisse sind immer wichtig, wenn Sie Komponenten entwickeln lose gekoppelt werden. Anwendungen führen häufig mehrere Vorgänge als Reaktion auf eine Benutzeraktion. Backbone bietet integrierte Ereignisse bei Komponenten wie Modell, Auflistung und anzeigen. Anstatt zu erstellen, die Abhängigkeiten zwischen diesen Komponenten, die Vorlage verwendet einen "Pub/Sub"-Modell: die `events` -Objekt, definiert in events.ts, fungiert als einen Event Hub für das Veröffentlichen und Abonnieren von Anwendungsereignissen. Die `events` ist eine Singleton-Objekt. Der folgende Code zeigt, wie Sie ein Ereignis abonniert und Auslösen des Ereignisses:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Router**

In Backbone.js bietet ein Router Methoden für die Seiten für die clientseitige routing und das Verbinden mit Aktionen und Ereignisse. Die Vorlage definiert einen einzelnen Router in router.ts. Der Router die activable Ansichten erstellt und verwaltet den Status, wenn die Ansicht gewechselt. (Activable Ansichten werden im nächsten Abschnitt beschrieben.) Das Projekt enthält zunächst zwei dummy-Ansichten, Heim- und Informationen zu. Außerdem enthält er eine NotFound-Ansicht, die angezeigt wird, wenn die Route nicht bekannt ist.

**Ansichten**

Die Ansichten werden in ~/Scripts/application/Ansichten definiert. Es gibt zwei Arten von Ansichten, activable und modales Dialogfeld Ansichten. Activable Ansichten werden vom Router aufgerufen. Wenn eine activable Ansicht angezeigt wird, werden alle anderen activable Ansichten deaktiviert. Um eine activable erstellen, erweitern Sie die Ansicht mit den `Activable` Objekt:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Erweitern von mit `Activable` fügt zwei neue Methoden in die Ansicht `activate` und `deactivate`. Der Router werden diese Methoden zum Aktivieren und die Ansicht deactive aufgerufen.

Modale Ansichten wurden implementiert, als [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modale Dialogfelder. Die `Membership` und `Profile` Ansichten sind modalen Ansichten verwendet werden. Modelle, Ansichten können alle Ereignisse der Anwendung aufgerufen werden. Z. B. in der `Navigation` anzeigen, die Sie auf den Link "Mein Konto" zeigt entweder den `Membership` anzeigen oder die `Profile` anzeigen, je nachdem, ob der Benutzer angemeldet ist. Die `Navigation` angefügt, klicken Sie auf Ereignishandler mit seinen untergeordneten Elementen, die die `data-command` Attribut. Dies ist das HTML-Markup:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Hier ist der Code in navigation.ts so verknüpfen Sie die Ereignisse ein:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelle**

Die Modelle werden in ~/Scripts/application/Modelle definiert. Alle Modelle haben Sie drei grundlegende Dinge: Standard, Attribute, Validierungsregeln und einen serverseitigen Endpunkt. Hier ist ein typisches Beispiel:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**-Plug-ins**

Der Ordner "~/Scripts/application/lib" enthält einige praktische jQuery-Plug-ins. Die form.ts-Datei definiert ein plug-in für die Arbeit mit Daten. Häufig müssen Sie zum Serialisieren oder Deserialisieren der Formulardaten und modellvalidierungsfehler anzeigen. Die form.ts-Plug-in verfügt über Methoden wie z. B. `serializeFields`, `deserializeFields`, und `showFieldErrors`. Das folgende Beispiel zeigt, wie ein Formular mit einem Modell serialisiert wird.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Die flashbar.ts-Plug-in bietet verschiedene Arten von feedbacknachrichten an dem Benutzer. Die Methoden sind `$.showSuccessbar`, `$.showErrorbar` und `$.showInfobar`. Hinter den Kulissen verwendet Twitter Bootstrap-Warnungen verwendet, um gut animierte Nachrichten angezeigt werden.

Die confirm.ts-Plug-in ersetzt des Browsers Bestätigungsdialogfeld, obwohl die API ein wenig anders ist:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Exemplarische Vorgehensweise: Server-Code

Jetzt sehen wir uns an der Serverseite.

**Controller**

In einer einseitigen Anwendung spielt der Server nur eine kleine Rolle in der Benutzeroberfläche. In der Regel der Server die erste Seite dargestellt, und klicken Sie dann sendet und empfängt JSON-Daten.

Die Vorlage verfügt über zwei MVC-Controller: `HomeController` rendert die ersten Seite und `SupportsController` wird verwendet, um neue Benutzerkonten zu bestätigen und Zurücksetzen von Kennwörtern. Alle anderen Domänencontroller in der Vorlage sind ASP.NET Web-API-Controller, der senden und Empfangen von JSON-Daten. Die Controller in der Standardeinstellung verwenden die neue `WebSecurity` Klasse, um Benutzer bezogenen Aufgaben auszuführen. Sie müssen jedoch auch optionale Konstruktoren, mit denen Sie die Delegaten für diese Aufgaben zu übergeben. Dadurch wird das Testen vereinfacht. zudem können Sie die ersetzen `WebSecurity` mit einem anderen, mit einem IoC-Container. Im Folgenden ein Beispiel:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Ansichten

Die Sichten dienen modular aufgebaut sein: jeder Abschnitt einer Seite hat eine eigene dedizierte Ansicht. In einer einseitigen Anwendung ist es üblich, die Ansichten enthalten, die keine entsprechenden Controller verfügen. Sie können eine Sicht einschließen, durch den Aufruf `@Html.Partial('myView')`, aber dies mühsam. Um dies zu vereinfachen, die Vorlage definiert eine Hilfsmethode `IncludeClientViews`, rendert alle Ansichten in einem bestimmten Ordner:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Wenn der Name des Ordners nicht angegeben wird, ist der Standardname für den Ordner "ClientViews". Wenn die Clientansicht auch Teilansichten verwendet wird, benennen Sie die partielle Ansicht mit einem Unterstrich (z. B. `_SignUp`). Die `IncludeClientViews` -Methode schließt alle Ansichten, deren Name mit einem Unterstrich beginnt. Rufen Sie zum Einschließen einer Teilansicht in Clientansicht `Html.ClientView('SignUp')` anstelle von `Html.Partial('_SignUp')`.

**Senden von E-Mails**

Zum Senden von e-Mails, die Vorlage verwendet [Postal](http://aboutcode.net/postal). Postal wird jedoch vom Rest des Codes mit abstrahiert die `IMailer` Schnittstelle, damit Sie es einfach durch eine andere Implementierung ersetzen können. Die e-Mail-Vorlagen befinden sich im Ordner "Views/E-Mails". Die e-Mail-Adresse des Absenders in der Datei "Web.config" angegeben ist, der `sender.email` -Schlüssel mit der die **"appSettings"** Abschnitt. Auch wenn `debug="true"` in "Web.config", die Anwendung erfordert keine e-Mail-Bestätigung durch den Benutzer, um die Entwicklung zu beschleunigen.

## <a name="github"></a>GitHub

Sie finden auch die Backbone.js SPA-Vorlage auf [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
