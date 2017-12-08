---
uid: single-page-application/overview/templates/backbonejs-template
title: Backbone-Vorlage | Microsoft Docs
author: madskristensen
description: Backbone.js SPA-Vorlage
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="backbone-template"></a>Backbone-Vorlage
====================
durch [Mads Kristensen](https://github.com/madskristensen)

> Backbone-SPA-Vorlage wurde vom Kazi Manzur Rashid geschrieben.
> 
> [Herunterladen der Backbone.js SPA-Vorlage](https://go.microsoft.com/fwlink/?LinkId=293631)


Die Vorlage Backbone.js SPA dient zum schnellen Erstellen von interaktiven clientseitigen webapps, die mit Ihnen den Einstieg [Backbone.js.](http://backbonejs.org/)

Die Vorlage bietet eine anfängliche Skelett für die Entwicklung einer Anwendung Backbone.js in ASP.NET MVC. Direktes es Standardbenutzer Anmeldung stellt Funktionalität bereit, einschließlich registrieren, anmelden, das Zurücksetzen von Benutzerkennwörtern und Bestätigung durch den Benutzer mit grundlegenden e-Mail-Vorlagen.

Anforderungen:

- [Update für ASP.NET und Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Erstellen Sie ein Vorlagenprojekt Backbone

Herunterladen Sie und installieren Sie die Vorlage, indem Sie auf die Schaltfläche "herunterladen" oben. Die Vorlage wird als Visual Studio-Erweiterung (VSIX)-Datei verpackt. Sie müssen möglicherweise Visual Studio neu starten.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-**Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt, und klicken Sie auf **OK**.

![](backbonejs-template/_static/image1.png)

In der **neues Projekt** Assistenten, wählen Backbone.js SPA-Projekt.

![](backbonejs-template/_static/image2.png)

Drücken Sie STRG + F5, um zu erstellen, und führen Sie die Anwendung ohne Debuggen aus, oder drücken Sie F5, um mit Debuggen auszuführen.

![](backbonejs-template/_static/image3.png)

Klicken auf "Mein Konto" wird die Anmeldeseite geöffnet:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Exemplarische Vorgehensweise: Clientcode für

Wir beginnt mit der Clientseite. Die Client-Anwendungskripts befinden sich im Ordner "~/Scripts/application". Die Anwendung wird im geschrieben [TypeScript](http://www.typescriptlang.org/) (TS-Dateien) werden die in JavaScript (JS-Dateien) kompiliert.

**Anwendung**

`Application`wird in application.ts definiert. Dieses Objekt die Anwendung initialisiert und fungiert als Stamm-Namespace. Es enthält Informationen zur Konfiguration und Status, die für die Anwendung freigegeben ist, z. B., ob der Benutzer angemeldet ist.

Die `application.start` Methode erstellt den modalen Ansichten verwendet werden, und fügt Sie Ereignishandler für Ereignisse auf Anwendungsebene, wie z. B. Benutzer anmelden. Als Nächstes den Standard-Router erstellt und überprüft, ob alle Client-Side-URL angegeben wurde. Wenn nicht, um die Standard-Url leitet (#! /).

**Ereignisse**

Ereignisse sind immer wichtig, wenn Komponenten entwickeln lose gekoppelt werden. Anwendungen werden häufig mehrere Vorgänge ausführen, als Antwort auf eine Benutzeraktion. Backbone bietet integrierte Ereignisse bei Komponenten wie das Modell, Sammlung und anzeigen. Statt den Abhängigkeiten zwischen diesen Komponenten, die Vorlage verwendet ein Modell "Pub/Sub": die `events` -Objekt, definiert in events.ts, fungiert als einen Event Hub für das Veröffentlichen und Abonnieren von Ereignissen. Die `events` ist eine Singleton-Objekt. Der folgende Code zeigt das Abonnieren eines Ereignisses und zum Auslösen des Ereignisses:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Router**

In Backbone.js bietet ein Router Methoden für die clientseitige Seiten routing und das Verbinden mit Aktionen und Ereignisse. Die Vorlage definiert einen einzelnen Router in router.ts. Der Router activable Ansichten erstellt und verwaltet den Status, wenn die Ansicht gewechselt. (Activable Ansichten werden im nächsten Abschnitt beschrieben.) Zunächst das Projekt verfügt über zwei dummy-Ansichten, Heim und Informationen zu. Darüber hinaus weist er eine Sicht nicht gefunden wurde, der angezeigt wird, wenn die Route nicht bekannt ist.

**Ansichten**

Die Ansichten werden in ~/Scripts/application/Ansichten definiert. Es gibt zwei Arten von Ansichten, activable Ansichten und Ansichten modales Dialogfeld. Activable Ansichten werden vom Router aufgerufen. Wenn eine activable Ansicht angezeigt wird, werden alle anderen activable Ansichten deaktiviert. Um eine activable Ansicht zu erstellen, erweitern Sie die Ansicht mit der `Activable` Objekt:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Erweitern von mit `Activable` fügt zwei neue Methoden in der Ansicht zu `activate` und `deactivate`. Der Router ruft diese Methoden zum Aktivieren und die Ansicht deactive.

Modale Ansichten wurden implementiert, als [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modale Dialogfelder. Die `Membership` und `Profile` Ansichten sind modalen Ansichten verwendet werden. Modellansichten können durch alle Anwendungsereignisse aufgerufen werden. Z. B. in der `Navigation` anzeigen, die Sie auf den Link "Mein Konto" zeigt entweder den `Membership` anzeigen oder die `Profile` anzeigen, je nachdem, ob der Benutzer angemeldet ist. Die `Navigation` fügt klicken Sie auf die Ereignishandler für alle untergeordneten Elemente, die über die `data-command` Attribut. Hier ist das HTML-Markup:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Hier ist der Code in navigation.ts, um die Ereignisse zu verknüpfen:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelle**

Die Modelle werden in ~/Scripts/application-Modelle definiert. Die Modelle verfügen über drei grundlegende Dinge: Standard, Attribute, Validierungsregeln und einer serverseitigen Endpunkt. Hier ist ein typisches Beispiel:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**-Plug-ins**

Der ~/Scripts/application/lib-Ordner enthält einige praktische jQuery-Plug-ins. Die Datei form.ts definiert ein plug-in für die Arbeit mit Daten. Häufig müssen Sie zum Serialisieren oder Deserialisieren der Formulardaten und modellvalidierungsfehler anzeigen. Die form.ts-Plug-in verfügt über Methoden wie z. B. `serializeFields`, `deserializeFields`, und `showFieldErrors`. Im folgende Beispiel wird gezeigt, wie ein Formular mit einem Modell serialisiert wird.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Die flashbar.ts-Plug-in bietet verschiedene Arten von Feedback Nachrichten an dem Benutzer. Die Methoden sind `$.showSuccessbar`, `$.showErrorbar` und `$.showInfobar`. Im Hintergrund verwendet es Twitter Bootstrap-Warnungen, um ordentlich animierte Nachrichten anzuzeigen.

Die confirm.ts-Plug-in ersetzt des Browsers bestätigen (Dialogfeld), aus, obwohl die API etwas anders ist:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Exemplarische Vorgehensweise: Servercode

Jetzt sehen wir uns die Serverseite.

**Controller**

In einer einzelnen Seite Anwendung spielt der Server nur eine kleine Rolle in der Benutzeroberfläche. In der Regel der Server die erste Seite gerendert und dann sendet und empfängt JSON-Daten.

Die Vorlage hat zwei MVC-Controller: `HomeController` rendert die erste Seite und `SupportsController` wird verwendet, um zu bestätigen neue Benutzerkonten und Kennwörter zurücksetzen. Alle anderen Domänencontroller in der Vorlage sind die ASP.NET Web API-Controller an, die senden und Empfangen von JSON-Daten. Die Controller verwenden standardmäßig die neue `WebSecurity` Klasse, um Benutzer bezogenen Aufgaben auszuführen. Sie haben allerdings auch optionale Konstruktoren, mit denen Sie die Delegaten für diese Aufgaben zu übergeben. Dadurch wird der testen vereinfachen und ermöglicht es Ihnen ersetzen `WebSecurity` mit etwas anderem, mithilfe einer IoC-Container. Im Folgenden ein Beispiel:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Ansichten

Die Ansichten dienen als modulare: jeder Abschnitt einer Seite verfügt über einen eigenen dedizierten anzeigen. In einer einzelnen Seite Anwendung ist es üblich, Ansichten enthalten, die keine entsprechenden Controller verfügen. Sie können eine Sicht einschließen, durch den Aufruf `@Html.Partial('myView')`, aber dies ruft mühsam. Um dies zu vereinfachen, die Vorlage definiert eine Hilfsmethode `IncludeClientViews`, rendert alle Ansichten in einem bestimmten Ordner:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Wenn der Name des Ordners nicht angegeben wird, wird der Standardname des Ordners "ClientViews". Wenn Ihre Clientansicht auch Teilansichten verwendet wird, benennen Sie die partielle Ansicht mit einem Unterstrich (z. B. `_SignUp`). Die `IncludeClientViews` Methode schließt alle Sichten, deren Name mit einem Unterstrich beginnt. Rufen Sie zum Einschließen einer Teilansicht in der Clientansicht `Html.ClientView('SignUp')` anstelle von `Html.Partial('_SignUp')`.

**Senden von E-Mail**

Um e-Mail zu senden, die Vorlage verwendet [Postal](http://aboutcode.net/postal). Postal wird jedoch vom Rest des Codes mit abstrahiert die `IMailer` Schnittstelle, damit Sie problemlos mit einer anderen Implementierung ersetzen können. Die e-Mail-Vorlagen befinden sich im Ordner "Ansichten/e-Mails". Die e-Mail-Adresse des Absenders in der Datei "Web.config" angegeben ist, der `sender.email` der Schlüssel der **"appSettings"** Abschnitt. Auch wenn `debug="true"` in "Web.config", die Anwendung erfordert keine Benutzer e-Mail-Bestätigung, um die Entwicklung zu beschleunigen.

## <a name="github"></a>GitHub

Sie erhalten auch die Backbone.js SPA-Vorlage auf [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
