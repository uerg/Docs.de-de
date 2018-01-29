---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: "Einen Überblick über die Formularauthentifizierung (VB) | Microsoft Docs"
author: rick-anderson
description: In diesem Lernprogramm wird von reinen Diskussion Implementierung Aktualisierungsfehler; insbesondere, betrachten wir Formularauthentifizierung implementieren. Die Web-Anwendung w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 90bcff91d0642e6af66f43fd807b253cc516d277
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-forms-authentication-vb"></a>Einen Überblick über die Formularauthentifizierung (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> In diesem Lernprogramm wird von reinen Diskussion Implementierung Aktualisierungsfehler; insbesondere, betrachten wir Formularauthentifizierung implementieren. Die Webanwendung, die in diesem Lernprogramm erstellen Sie zunächst wird weiterhin in den nachfolgenden Lernprogrammen basiert, werden wie wir von einfachen Formularauthentifizierung, Mitgliedschaft und Rollen verschieben.
> 
> Wenden Sie sich dieses Video für Weitere Informationen zu diesem Thema: [verwenden grundlegende der Formularauthentifizierung in ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](security-basics-and-asp-net-support-vb.md) verschiedene Authentifizierung, Autorisierung und Benutzer-Kontooptionen von ASP.NET bereitgestellte besprochen. In diesem Lernprogramm wird von reinen Diskussion Implementierung Aktualisierungsfehler; insbesondere, betrachten wir Formularauthentifizierung implementieren. Die Webanwendung, die in diesem Lernprogramm erstellen Sie zunächst wird weiterhin in den nachfolgenden Lernprogrammen basiert, werden wie wir von einfachen Formularauthentifizierung, Mitgliedschaft und Rollen verschieben.

In diesem Lernprogramm beginnt mit dem eine ausführliche Betrachtung der Forms-authentifizierungsworkflow ein Thema, das wir im vorherigen Lernprogramm nach berührt. Danach wird eine ASP.NET-Website über den die Konzepte der Formularauthentifizierung demo erstellt. Anschließend konfigurieren wir die Website, um die Formularauthentifizierung verwenden, erstellen eine einfache Anmeldeseite und erfahren, wie im Code, um zu bestimmen, ob ein Benutzer authentifiziert wird und wenn dies der Fall ist, den Benutzernamen sie sich angemeldet haben.

Grundlegendes zu den Formen authentifizierungsworkflow, es in einer Webanwendung aktivieren, und erstellen die Seiten und Abmeldeinformationen werden alle wichtigen Schritte bei der Erstellung einer ASP.NET-Anwendung, die Benutzerkonten unterstützt und Authentifizierung von Benutzern über eine Webseite. Aus diesem - Grund und da diese Lernprogramme auf anderen - aufbauen empfehle ich Sie dieses Lernprogramm vollständig zu absolvieren, bevor auf zum nächsten Endpunkt verschieben, selbst wenn Sie bereits Erfahrung konfigurieren Waren in früheren Projekten Formularauthentifizierung.

## <a name="understanding-the-forms-authentication-workflow"></a>Verstehen des Forms-Authentifizierung-Workflows

Wenn die ASP.NET-Laufzeit eine Anforderung für eine ASP.NET-Ressource, z. B. eine ASP.NET-Seite oder ASP.NET Web-Dienst verarbeitet löst die Anforderung eine Anzahl von Ereignissen während des Lebenszyklus. Ereignisse, die ausgelöst wird, am Ende sehr Anfangs- und sehr der Anforderung, diejenigen ausgelöst, wenn die Anforderung authentifiziert und autorisiert, ein Ereignis ausgelöst wird, im Falle einer nicht behandelten Ausnahme usw. vorhanden sind. Um eine vollständige Liste der Ereignisse anzuzeigen, finden Sie in der ["HttpApplication" Objektereignisse](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*HTTP-Module* verwaltete Klassen, deren Code wird als Reaktion auf ein bestimmtes Ereignis am Anforderungslebenszyklus ausgeführt, werden. Im Lieferumfang von ASP.NET sind einer Anzahl von HTTP-Module, die wichtige Aufgaben im Hintergrund ausführen. Zwei integrierte HTTP-Module, die für unsere Diskussion besonders relevant sind, sind:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -authentifiziert den Benutzer durch Überprüfen das Formularauthentifizierungsticket, das in die Auflistung der Cookies des Benutzers in der Regel enthalten ist. Wenn keine Formularauthentifizierungsticket vorhanden ist, ist der Benutzer anonym.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -legt fest, ob der aktuelle Benutzer, beim Zugriff auf die angeforderte URL berechtigt ist. Dieses Modul ermittelt die Autorität für die indem Sie die Autorisierungsregeln, die in den Anwendungskonfigurationsdateien angegeben. ASP.NET enthält außerdem die [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) , der Autorität für die von der angeforderten Dateien ACLs consulting bestimmt.

Die FormsAuthenticationModule versucht, den Benutzer vor dem Ausführen von UrlAuthorizationModule (und FileAuthorizationModule) zu authentifizieren. Wenn der anfordernde Benutzer Zugriff auf die angeforderte Ressource nicht autorisiert ist, das Autorisierungsmodul die Anforderung beendet und gibt ein [HTTP 401 Unauthorized](http://www.checkupdown.com/status/E401.html) Status. In Szenarien für Windows-Authentifizierung wird der HTTP 401-Status an den Browser zurückgegeben. Dieser Statuscode bewirkt, dass der Browser fordert den Benutzer zur Eingabe ihrer Anmeldeinformationen über ein modales Dialogfeld. Bei der Formularauthentifizierung jedoch der Status der HTTP 401 Unauthorized wird nie an den Browser gesendet die FormsAuthenticationModule erkennt diese Status und dahingehend verändert, um den Benutzer zur Anmeldeseite umzuleiten (über eine [HTTP 302-Umleitung](http://www.checkupdown.com/status/E302.html) Status).

Die Anmeldeseite Verantwortung besteht darin, zu bestimmen, ob die Anmeldeinformationen des Benutzers gültig sind, und wenn dies der Fall ist, um eine Formularauthentifizierungsticket erstellen und den Benutzer zurück zur Seite weiterleiten sie haben versucht, besuchen. Das Authentifizierungsticket ist in nachfolgenden Anforderungen an die Seiten auf der Website enthalten, die die FormsAuthenticationModule zur Identifizierung des Benutzers verwendet.


[![Der Forms-Authentifizierung-Workflow](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Abbildung 01**: der Forms-Authentifizierung-Workflow ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Das Authentifizierungsticket merken über Seite Besuche

Nach der Anmeldung muss das Formularauthentifizierungsticket zurück an den Webserver bei jeder Anforderung gesendet werden, damit der Benutzer angemeldet bleibt, während sie den Standort surfen. Dies geschieht normalerweise durch das Authentifizierungsticket in die Auflistung der Cookies des Benutzers platzieren. [Cookies](http://en.wikipedia.org/wiki/HTTP_cookie) sind kleine Textdateien, die auf dem Computer des Benutzers befinden, und werden in die HTTP-Header für jede Anforderung an die Website an, das Cookie erstellt, übertragen. Sobald das Formularauthentifizierungsticket erstellt und in der Browser Cookies gespeichert wurde, sendet jede nachfolgende Besuch für diesen Standort daher das Authentifizierungsticket zusammen mit der Anforderung, wodurch Identifizierung des Benutzers.

> [!NOTE]
> Die Demo-Webanwendung, die in jedem Lernprogramm verwendet steht als Download zur Verfügung. Diese herunterladbare Anwendung wurde in Visual Web Developer 2008 als Ziel für .NET Framework, Version 3.5 erstellt. Da die Anwendung für .NET 3.5 ausgerichtet ist, enthält die Datei "Web.config" zusätzliche, 3.5-spezifischen Konfigurationselementen. Kurz gesagt, wenn Sie noch installieren Sie .NET 3.5 auf dem Computer dann zum Herunterladen der Anwendung ist nicht möglich, ohne zuvor die 3.5-spezifischer Markups aus Datei "Web.config" entfernt.


Ein Aspekt von Cookies ist abgelaufen, also das Datum und Uhrzeit, an dem der Browser Cookies verwirft. Wenn Formularauthentifizierungscookies abläuft, wird der Benutzer kann, nicht mehr authentifiziert und aus diesem Grund werden anonyme. Wenn ein Benutzer über ein öffentliches Terminal besucht, sind wahrscheinlich, dass sie ihre Authentifizierungsticket abläuft, wenn sie ihren Browser schließen möchten. Wenn das Unternehmen besuchende von zu Hause aus demselben Benutzer können jedoch das Authentifizierungsticket hinweg Browser gespeichert werden soll, sodass keine erneut jedes Mal, die sie auf der Website anmelden. Diese Entscheidung wird vom Benutzer in Form eines Kontrollkästchens Anmeldedaten auf der Anmeldeseite häufig vorgenommen werden. In Schritt 3 werden wie auf der Anmeldungsseite ein Kontrollkästchen Anmeldedaten implementiert untersucht. Im folgende Lernprogramm Adressen am Authentifizierung Ticket-Timeoutwert im Detail.

> [!NOTE]
> Es ist möglich, dass der Benutzer-Agent, der zur Anmeldung auf der Website nicht Cookies unterstützen kann. In diesem Fall können ASP.NET cookieless Formularauthentifizierungstickets verwenden. In diesem Modus wird das Authentifizierungsticket in der URL codiert. Betrachten wir Wenn cookieless Authentifizierungstickets verwendet werden und wie sie erstellt und in den nächsten Lernprogrammen verwaltet werden.


### <a name="the-scope-of-forms-authentication"></a>Der Bereich des Formular-Authentifizierung

Die FormsAuthenticationModule verwaltet wird, Teil der ASP.NET-Laufzeit code. Vor Version 7 der Microsofts [(Internet Information Services, IIS)](https://www.iis.net/) Webserver, es wurde eine unterschiedliche Barriere zwischen IISs HTTP-Pipeline und die ASP.NET-Laufzeit Pipeline. Kurz gesagt, in IIS 6 und früher, wird die FormsAuthenticationModule nur ausgeführt, wenn eine Anforderung an die ASP.NET-Laufzeit aus IIS delegiert werden kann. Standardmäßig verarbeitet IIS statische Inhalte selbst – z. B. HTML-Seiten und CSS-- und -Imagedateien nur übergibt die Anforderungen an die ASP.NET-Laufzeit auf, wenn eine Seite mit der Erweiterung von ASPX-, ASMX- oder ashx angefordert wird.

IIS 7, ermöglicht jedoch integrierten IIS und ASP.NET Pipelines. Einige Konfigurationseinstellungen können Sie IIS 7 zum Aufrufen der FormsAuthenticationModule für einrichten *alle* Anforderungen. Darüber hinaus können Sie URL-Autorisierungsregeln für Dateien eines beliebigen Typs mit IIS 7 definieren. Weitere Informationen finden Sie unter [Änderungen zwischen IIS 6 und IIS 7-Sicherheit](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [Ihre Plattform-Websicherheit](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), und [Verständnis IIS7-URL-Autorisierung](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Lange Story kurz in Versionen vor IIS 7, nur Formularauthentifizierung können Sie beim Schützen von Ressourcen, die durch die ASP.NET-Laufzeit behandelt. Ebenso werden URL-Autorisierungsregeln nur auf Ressourcen, die durch die ASP.NET-Laufzeit behandelt angewendet. Mit IIS 7 Es ist jedoch möglich, die FormsAuthenticationModule und UrlAuthorizationModule in IISs HTTP-Pipeline, wodurch erweitern diese Funktionalität für alle Anforderungen zu integrieren.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Schritt 1: Erstellen einer ASP.NET-Website für diese Reihe von Lernprogrammen

Um die größtmögliche Zielgruppe erreichen, wir erstellen in der gesamten diese Reihe von, ASP.NET-Website mit Microsofts kostenlose Version von Visual Studio 2008 erstellt [Visual Web Developer 2008.](https://www.microsoft.com/express/vwd/). Implementieren wir im Benutzerspeicher SqlMembershipProvider in einem [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) Datenbank. Wenn Sie Visual Studio 2005 oder eine andere Edition von Visual Studio 2008 oder SQL Server verwenden, keine Sorge: die Schritte werden fast identisch, und keine bedeutenden Unterschiede werden darauf hingewiesen werden.

Bevor wir die Formularauthentifizierung konfigurieren können, benötigen wir zuerst eine ASP.NET-Website. Durch das Erstellen einer neuen systembasierten ASP.NET-Website starten. Um dies zu erreichen, starten Sie Visual Web Developer, wechseln Sie zum Menü Datei, und klicken Sie neue Website, die das Dialogfeld "neue Website" anzuzeigen. Wählen Sie die Vorlage ASP.NET-Website, legen Sie die Dropdown-Liste im Dateisystem, wählen Sie einen Ordner der Website an und legen Sie die Sprache auf VB dar. Dadurch wird eine neue Website erstellt, mit einer ASP.NET-Seite von "default.aspx", eine App\_Datenordner, und eine Datei "Web.config".

> [!NOTE]
> Visual Studio unterstützt zwei Bereitstellungsmodi Projektmanagement: Websiteprojekte und Webanwendungsprojekte. Websiteprojekte fehlen eine Projektdatei, wohingegen Webanwendungsprojekte der Projektarchitektur in Visual Studio .NET 2002/2003 imitieren – sie enthalten eine Projektdatei, und Kompilieren von Quellcode für das Projekt in einer einzelnen Assembly im Ordner "/ bin" befindet. Visual Studio 2005-Projekte anfangs nur unterstützte Website, obwohl das Webanwendungsprojekt-Modell mit Service Pack 1 wieder hinzugefügt wurde; Visual Studio 2008 bietet beider Projektmodelle. Visual Web Developer 2005 und 2008-Editionen unterstützen allerdings nur Websiteprojekte. Ich wird das Websiteprojekt-Modell verwenden. Wenn Sie eine nicht-Express Edition verwenden und, verwenden Sie möchten die [Webanwendungsprojekt Modell](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) stattdessen können Sie dies tun, aber beachten Sie, dass es möglicherweise einige Diskrepanzen zwischen Sie, auf dem Bildschirm und die Schritte, die Sie ergreifen müssen sehen, im Vergleich zu der Screenshots dargestellt und Anweisungen, die in diesen Lernprogrammen.


[![Erstellen Sie eine neue Website von Datei-basierten System](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Abbildung 02**: Erstellen einer Website New File System-Based ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>Hinzufügen einer Masterseite

Als Nächstes fügen Sie eine neue Master-Seite, auf der Website in das Stammverzeichnis, das mit dem Namen Site.master. [Masterseiten](https://msdn.microsoft.com/library/wtxbf3hh.aspx) können Seitenentwickler eine standortweite Vorlage definieren, die auf ASP.NET-Seiten angewendet werden können. Der wichtigste Vorteil von Masterseiten darin, dass die Gesamtdarstellung des Standorts kann an einem zentralen Ort definiert werden, wodurch er einfach aktualisieren oder der Website-Layout optimieren.


[![Fügen Sie eine Master-Seite mit dem Namen Site.master auf der Website](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Abbildung 03**: Fügen Sie einem Site.master mit dem Namen eines Master Seite auf der Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image9.png))


Definieren Sie die standortweite Seitenlayout hier in der Masterseite. Können Sie mithilfe die Entwurfsansicht und Hinzufügen von beliebigen Layout oder Web-Steuerelemente Sie müssen, oder Sie können das Markup manuell in der Datenquellensicht manuell hinzufügen. Ich meine Masterseite Layout imitieren, die das Layout in strukturierten meine  *[arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)*  Reihe von Lernprogrammen (siehe Abbildung 4). Die Gestaltungsvorlage verwendet [cascading Stylesheets](http://www.w3schools.com/css/default.asp) zum Positionieren und Formatvorlagen mit den CSS-Einstellungen in der Datei Style.css (das im zugehörigen Download für dieses Lernprogramm enthalten ist) definiert. Die CSS-Regeln werden definiert, während Sie nicht aus dem unten gezeigten Markup erkennen können, sodass die Navigation &lt;Div&gt;des Inhalt ist absolut positioniert, sodass er auf der linken Seite angezeigt und verfügt über eine feste Breite von 200 Pixel.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Eine Masterseite definiert das Layout der Seite "static" und die Bereiche, die durch die ASP.NET-Seiten bearbeitet werden können, die die Gestaltungsvorlage verwenden. Dieser Inhalt bearbeitbare Bereiche des Warnschwellenwerts durch ContentPlaceHolder-Steuerelements, der im Inhalt angezeigt werden kann, &lt;Div&gt;. Unsere Masterseite verfügt über eine einzelne ContentPlaceHolder (MainContent), aber der Masterseite kann mehrere ContentPlaceHolders enthalten.

Durch das Markup oben eingegebenen zeigt das Umschalten zur Entwurfsansicht der Masterseite Layout aus. Alle ASP.NET-Seiten, die diese Masterseite verwenden müssen dieses einheitliche Layout, die Möglichkeit, die das Markup für die Region MainContent angeben.


[![Die Master-Seite, wenn durch die Entwurfsansicht angezeigt](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Abbildung 04**: die Gestaltungsvorlage, wenn angezeigt über die Entwurfsansicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>Erstellen von Inhaltsseiten

An diesem Punkt haben wir eine Seite "default.aspx" auf unserer Website, aber nicht die Gestaltungsvorlage gerade erstellten verwendet. Es ist zwar möglich, deklarative Markup einer Webseite mithilfe eine Masterseite bearbeiten, wenn die Seite Inhalt enthält, ist es noch einfacher, einfach die Seite "löschen", und fügen ihn erneut für das Projekt, das Angeben der Gestaltungsvorlage verwendet. Daher Starten von "default.aspx" aus dem Projekt löschen.

Klicken Sie dann mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie ein neues Webformular mit dem Namen "default.aspx" hinzufügen. Zeit, aktivieren Sie das Kontrollkästchen Masterseite auswählen und wählen Sie die Site.master Gestaltungsvorlage aus der Liste aus.


[![Fügen Sie eine neue "default.aspx"-Seite, die auf einer Masterseite auswählen](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Abbildung 05**: Hinzufügen einer neuen "default.aspx" Seite auswählen einer Masterseite auswählen ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Verwenden Sie die Seite Site.master Master](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Abbildung 06**: Verwenden der Site.master Gestaltungsvorlage ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> Bei Verwendung des Webanwendungsmodells-Projekt umfasst das Dialogfeld "Neues Element hinzufügen" nicht das Kontrollkästchen Masterseite auswählen. Stattdessen müssen Sie ein Element vom Typ Content Webformular hinzufügen. Nachdem die Inhalte Webformular mit der Option, und klicken Sie auf Hinzufügen, zeigt Visual Studio die gleiche SELECT-Anweisung einem Masterauftrag für den in Abbildung 6 dargestellten Dialogfeld.


Die neue "default.aspx" Seite deklarativem Markup enthält nur eine @Page Richtlinie angeben des Pfads zum Master Seite Datei und ein Steuerelement für der Gestaltungsvorlage MainContent ContentPlaceHolder.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Lassen Sie vorerst "default.aspx" leer. Es wird weiter unten in diesem Lernprogramm zum Hinzufügen von Inhalt, zurückgegeben.

> [!NOTE]
> Unsere Masterseite enthält einen Abschnitt eines Menüs oder eine andere Navigationsschnittstelle. Es wird eine solche Schnittstelle in einem späteren Lernprogramm erstellt.


## <a name="step-2-enabling-forms-authentication"></a>Schritt 2: Aktivieren der Formularauthentifizierung

Mit der ASP.NET-Website erstellt wird besteht unsere nächste Aufgabe Formularauthentifizierung zu aktivieren. Authentifizierungskonfiguration für die Anwendung wird angegeben, über die [ &lt;Authentifizierung&gt; Element](https://msdn.microsoft.com/library/532aee0e.aspx) in "Web.config". Die &lt;Authentifizierung&gt; Element enthält ein einzelnes Attribut mit dem Namen Mode, der angibt, das Authentifizierungsmodell von der Anwendung verwendet. Dieses Attribut kann einen der folgenden vier Werte aufweisen:

- **Windows** – wie in dem vorherigen Lernprogramm erläutert wird, wenn eine Anwendung Windows-Authentifizierung verwendet es obliegt dem Webserver zum Authentifizieren des Besuchers, und dies erfolgt in der Regel über Standard-, Digest- oder integrierte Windows Authentifizierung.
- **Formulare**-Benutzer authentifiziert werden über ein Formular auf einer Webseite.
- **Passport**-Benutzer werden mit der Microsoft Passport-Netzwerk authentifiziert.
- **Keine**-keine Authentifizierungsmodell verwendet; alle Besucher anonym sind.

Standardmäßig werden ASP.NET-Anwendungen Windows-Authentifizierung verwenden. Um den Authentifizierungstyp für die Formularauthentifizierung zu ändern, klicken Sie dann, müssen zum Ändern der &lt;Authentifizierung&gt; Mode-Attribut des Elements auf Formulare.

Wenn das Projekt noch keine Datei "Web.config" enthält, fügen Sie eine jetzt von mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer auswählen, neues Element hinzufügen und dann eine Webkonfigurationsdatei hinzufügen.


[![Wenn das Projekt "Web.config" noch nicht enthalten ist, fügen Sie ihn jetzt](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Abbildung 07**: Wenn Ihr Projekt wird nicht noch einschließen "Web.config", jetzt hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image21.png))


Suchen Sie als Nächstes die &lt;Authentifizierung&gt; Element- und Update, um die Verwendung der Formularauthentifizierung. Nach dieser Änderung sollte die Datei "Web.config" Markup etwa wie folgt aussehen:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Da die Datei "Web.config" eine XML-Datei ist, ist die Schreibweise wichtig. Stellen Sie sicher, dass Sie Formulare, mit dem Großbuchstaben F. Mode-Attribut festlegen Bei Verwendung von abweichender Groß-/Kleinschreibung, z. B. Formulare, erhalten Sie beim Besuchen der Website über einen Browser einen Fehler bei der Konfiguration.


Die &lt;Authentifizierung&gt; Element kann optional enthalten eine &lt;Forms&gt; untergeordnetes Element, das Forms Authentifizierung spezifischen Einstellungen enthält. Vorerst wir verwenden einfach die Standardeinstellungen der Forms-Authentifizierung. Wir untersuchen die &lt;Forms&gt; untergeordnetes Element ausführlicher in den nächsten Lernprogrammen.

## <a name="step-3-building-the-login-page"></a>Schritt 3: Erstellen der Anmeldeseite

Um die Formularauthentifizierung unterstützen benötigt unserer Website eine Anmeldeseite. Wie in der Vereinbarung erläutert, die im Authentifizierungsworkflow Forms-Abschnitt der FormsAuthenticationModule keine automatische Weiterleitung wird autorisiert, wenn sie versuchen, Zugriff auf eine Seite, die sie nicht der Benutzer die Anmeldeseite, anzuzeigen. Es gibt auch ASP.NET Web-Steuerelemente, die einen Link zur Anmeldeseite für anonyme Benutzer angezeigt werden. Dies führt zu folgender Frage, wie die URL der Anmeldeseite lautet?

Wird standardmäßig der Forms-Authentifizierungssystem erwartet, dass die Anmeldeseite "Login.aspx" benannt werden und in das Stammverzeichnis der Website eingefügt. Wenn Sie eine URL für einen anderen Benutzernamen verwenden möchten, können Sie dazu dazu muss es in "Web.config". Wir sehen, dass hierzu die nachfolgende Lernprogramm.

Die Anmeldeseite hat drei Aufgaben:

1. Geben Sie eine Schnittstelle, die den Besucher zur Eingabe von Anmeldeinformationen ermöglicht.
2. Bestimmt, ob die übermittelten Anmeldeinformationen gültig sind.
3. Melden Sie sich der Benutzer die Formulare Authentifizierungsticket erstellen.

### <a name="creating-the-login-pages-user-interface"></a>Erstellen der Anmeldeseite-Benutzeroberfläche

Wir beginnen mit der ersten Aufgabe. Fügen Sie eine neue ASP.NET-Seite in der Website-Stammverzeichnis mit dem Namen "Login.aspx", und ordnen Sie es mit der Masterseite Site.master.


[![Fügen Sie eine neue ASP.NET-Seite mit dem Namen "Login.aspx"](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Abbildung 08**: Hinzufügen einer neuen ASP.NET Seite mit dem Namen "Login.aspx" ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image24.png))


Die typische Login-Seite-Schnittstelle besteht aus zwei Textfelder - eines für den Namen des Benutzers, für die Kennwort- und eine Schaltfläche zum Senden des Formulars. Websites enthalten häufig ein Anmeldedaten-Kontrollkästchen, die, wenn dieses Kontrollkästchen aktiviert, das sich ergebende Authentifizierungsticket hinweg Browser beibehält.

Fügen Sie zwei Textfelder an "Login.aspx", und ihre "ID"-Eigenschaften auf Benutzername und Kennwort festgelegt. Legen Sie auf das Kennwort des Kennworts TextMode-Eigenschaft. Fügen Sie ein CheckBox-Steuerelement, dessen ID-Eigenschaft auf RememberMe und seine Texteigenschaft auf Me vergessen Sie nicht festlegen Danach fügen Sie eine Schaltfläche mit dem Namen LoginButton, deren Texteigenschaft, die Anmeldung festgelegt ist. Schließlich fügen Sie ein Websteuerelement Bezeichnung und durch Festlegen der Eigenschaft ID InvalidCredentialsMessage, seine Texteigenschaft auf Ihr Benutzername oder Kennwort ist ungültig. Versuchen Sie es., dessen ForeColor-Eigenschaft auf Rot und die Visible-Eigenschaft auf "false".

An diesem Punkt sollte am Bildschirm in Abbildung 9 Screenshot ähnelt und deklarative Syntax für die Seite sollte wie folgt:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![Die Anmeldeseite enthält zwei Textfelder, ein Kontrollkästchen, eine Schaltfläche und einer Bezeichnung](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Abbildung 09**: der Benutzername enthält zwei Textfelder der Seite, ein Kontrollkästchen, eine Schaltfläche und eine Bezeichnung ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image27.png))


Erstellen Sie abschließend einen Ereignishandler für das LoginButton auf Ereignis. Im Designer doppelklicken Sie einfach auf das Schaltflächen-Steuerelement, um Ereignishandler zu erstellen.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Bestimmen, ob die angegebenen Anmeldeinformationen gültig sind

Jetzt müssen wir Implementieren von Aufgabe 2 in den Schaltflächen-Klickereignis Ereignishandler: bestimmen, ob die angegebenen Anmeldeinformationen gültig sind. Zu diesem Zweck muss es einen Benutzerspeicher sein, der alle von der Benutzer Anmeldeinformationen enthält, damit wir bestimmen können, ob die angegebenen Anmeldeinformationen mit bekannten Anmeldeinformationen übereinstimmen.

Vor dem ASP.NET 2.0 wurden Entwickler verantwortlich für das Implementieren von sowohl ihre eigenen Speicher des Benutzers und das Schreiben des Codes, um die angegebenen Anmeldeinformationen für den Speicher zu überprüfen. Die meisten Entwickler würde den Speicher des Benutzers in einer Datenbank implementieren, erstellen eine Tabelle namens Benutzer mit Spalten, z. B. Benutzername, Kennwort, e-Mail-, LastLoginDate usw. Diese Tabelle würde dann, einen Datensatz pro Benutzerkonto verfügen. Überprüfen die angegebenen Anmeldeinformationen des Benutzers, würde das Abfragen der Datenbank zum Abgleichen des Benutzernamens ein, und klicken Sie dann sichergestellt, dass das Kennwort in der Datenbank auf das angegebene Kennwort entsprach.

Mit ASP.NET 2.0 sollte einer der Mitgliedschaftsanbieter Entwicklerseite um Speicher des Benutzers zu verwalten. In diesem Lernprogramm Reihe werden die SqlMembershipProvider verwendet die SQL Server-Datenbank für den Speicher des Benutzers verwendet. Bei Verwendung der SqlMembershipProvider muss einem bestimmten Datenbankschema zu implementieren, die die Tabellen, Sichten und gespeicherte Prozeduren, die vom Anbieter erwartet enthält. Untersuchen wir zum Implementieren dieses Schema in der  *[erstellen das Schema für die Mitgliedschaft in SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)*  Lernprogramm. Mit der Mitgliedschaftsanbieter vorhanden, überprüfen die Anmeldeinformationen des Benutzers ist so einfach wie das Aufrufen der [Mitgliedschaftsklasse](https://msdn.microsoft.com/library/system.web.security.membership.aspx)des [ValidateUser (*Benutzername*, *Kennwort*) Methode](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), dem ein boolescher Wert, der zurückgegeben, ob die Gültigkeit der *Benutzername* und *Kennwort* Kombination. Sehen, wie wir die SqlMembershipProvider Benutzerspeicher noch nicht implementiert wurde, können wir keine der Mitgliedschaft Klassenmethode ValidateUser zu diesem Zeitpunkt.

Anstatt Zeit in Anspruch nehmen die zum Erstellen von eigenen benutzerdefinierten Benutzer Datenbanktabelle (der veraltet wäre Nachdem wir die SqlMembershipProvider implementiert), lassen Sie uns stattdessen hartcodieren Seite die gültigen Anmeldeinformationen innerhalb der Anmeldename selbst. Klicken Sie in der LoginButton Click-Ereignishandler, fügen Sie den folgenden Code hinzu:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Wie Sie sehen können, es werden drei gültige Benutzerkonten - Scott Jisun und Sam - und alle drei haben Sie das gleiche Kennwort (Kennwort). Der Code durchläuft die Benutzer und Kennwörter Arrays, die einen gültigen Benutzernamen und das Kennwort Übereinstimmung gesucht. Wenn Benutzername und Kennwort gültig sind, muss den Benutzer anmelden und diese dann an die entsprechende Seite umleiten. Wenn die Anmeldeinformationen ungültig sind, wird die Bezeichnung InvalidCredentialsMessage angezeigt.

Wenn ein Benutzer über gültige Anmeldeinformationen eingibt, bereits erwähnt, dass sie anschließend an die entsprechende Seite umgeleitet werden. Was ist jedoch die entsprechende Seite? Beachten Sie, dass wenn ein Benutzer eine Seite, die sie nicht autorisiert sind besucht, um anzuzeigen, die FormsAuthenticationModule automatisch werden zur Anmeldeseite umgeleitet. In diesem Fall enthält sie die angeforderte URL in der Abfragezeichenfolge über den ReturnUrl-Parameter. D. h. würde, wenn ein Benutzer versucht, ProtectedPage.aspx besuchen, und sie nicht dazu autorisiert wurden, die FormsAuthenticationModule damit weiterleiten:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Bei erfolgreich anmelden sollten die Benutzer an ProtectedPage.aspx umgeleitet werden. Alternativ können Benutzer die Anmeldeseite auf ihre eigenen Volition besuchen. In diesem Fall sollte nach der Anmeldung des Benutzers sie auf den Stammordner Seite "Default.aspx" gesendet werden.

### <a name="logging-in-the-user"></a>Der Benutzer anmelden

Vorausgesetzt, dass die angegebenen Anmeldeinformationen gültig sind, müssen wir ein Formularauthentifizierungsticket erstellen und dem Benutzer der Website anmelden. Die [FormsAuthentication-Klasse](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) in der [Namespace System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) bietet verschiedene Methoden für die Protokollierung in und Benutzer über die Formulare Protokollierung Authentifizierungssystem. Dafür gibt es mehrere Methoden in der FormsAuthentication-Klasse, werden die drei, denen wir in diesem Zusammenhang interessiert sind:

- [GetAuthCookie (*Benutzername*, *PersistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) -erstellt ein Formularauthentifizierungsticket für die angegebene *Benutzername*. Diese Methode wird als Nächstes erstellt und gibt ein HttpCookie-Objekt, das den Inhalt des Authentifizierungstickets enthält. Wenn *PersistCookie* ist "true", ein permanentes Cookie wird erstellt.
- [SetAuthCookie (*Benutzername*, *PersistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -Ruft die GetAuthCookie (*Benutzername*, *PersistCookie*) die Methode zum Generieren der Formularauthentifizierungscookies. Diese Methode fügt dann das Cookie vom GetAuthCookie an der Auflistung der Cookies (vorausgesetzt, Cookies basierende Formularauthentifizierung verwendet; andernfalls wird, diese Methode ruft eine interne Klasse, die die Logik cookieless Ticket behandelt) zurückgegeben.
- [RedirectFromLoginPage(*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) - this method calls SetAuthCookie(*username*, *persistCookie*), and then redirects the user to the appropriate page.

GetAuthCookie ist praktisch, wenn Sie das Authentifizierungsticket vor dem Schreiben von Cookies in der Auflistung der Cookies ändern müssen. SetAuthCookie ist nützlich, wenn Sie möchten die Formulare Authentifizierungsticket erstellen, und fügen Sie es der Auflistung der Cookies hinzu, aber nicht den Benutzer auf die entsprechende Seite umleiten möchten. Vielleicht möchten Sie auf der Anmeldeseite bleiben oder an einigen alternative Seite senden.

Da wir melden Sie sich des Benutzers, und diese an die entsprechende Seite umleiten möchten, nutzen wir RedirectFromLoginPage an. Aktualisieren Sie die LoginButton auf Ereignishandler, der zwei kommentierten TODO-Zeilen durch die folgende Codezeile ersetzen:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

Authentifizierungsticket für die Erstellung der Forms verwenden wir die UserName TextBox Text-Eigenschaft für das Formularauthentifizierungsticket *Benutzername* Parameter und der Aktivierungszustand der RememberMe CheckBox für die  *PersistCookie* Parameter.

Um die Anmeldeseite zu testen, besuchen Sie es in einem Browser aus. Ungültige Anmeldeinformationen, z. B. Nope Benutzername und Kennwort falsch eingeben starten. Wenn Sie auf die Schaltfläche "Login" ein Postback auftreten wird und die InvalidCredentialsMessage-Bezeichnung angezeigt.


[![Die Bezeichnung InvalidCredentialsMessage ist ungültige Anmeldeinformationen angezeigt, wenn eingeben](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Abbildung 10**: die InvalidCredentialsMessage Bezeichnung ist ungültige Anmeldeinformationen angezeigt, wenn eingeben ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image30.png))


Als Nächstes geben Sie gültige Anmeldeinformationen ein, und klicken Sie auf die Schaltfläche "Login". Dieses Mal bei einem der Postback ein Formularauthentifizierungsticket erstellt, und Sie werden automatisch an Default.aspx weitergeleitet. An diesem Punkt haben Sie mit der Website angemeldet allerdings stehen keine visuellen Hinweise, um anzugeben, dass Sie derzeit angemeldet sind. In Schritt 4, wie Sie programmgesteuert zu ermitteln, ob ein Benutzer sehen, wird in oder nicht sowie zur Identifizierung des Benutzers auf der Seite "protokolliert.

Schritt 5 untersucht Techniken für die Protokollierung von eines Benutzers der Website.

### <a name="securing-the-login-page"></a>Sichern von der Anmeldeseite

Wenn der Benutzer ihre Anmeldeinformationen eingibt und das Anmeldeformular Seite sendet, werden die - einschließlich Kennwort - Anmeldeinformationen über das Internet an den Webserver in übertragen *Klartext*. Das bedeutet, dass alle Hacker sniffing den Netzwerkdatenverkehr des Benutzernamens und Kennworts angezeigt werden. Um dies zu verhindern, es ist wichtig, den Netzwerkdatenverkehr mit verschlüsseln [Secure Socket Layer (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Dadurch wird sichergestellt, dass die Anmeldeinformationen (ebenso wie die gesamte Seite HTML-Markup) ab dem Zeitpunkt verschlüsselt werden, die sie den Browser lassen, bis sie vom Webserver empfangen werden.

Es sei denn, Ihre Website vertrauliche Informationen enthalten, müssen Sie nur verwenden Sie SSL auf der Anmeldeseite und auf anderen Seiten, in denen würde das Kennwort des Benutzers andernfalls unverschlüsselt als nur-Text gesendet werden. Sie müssen keine Gedanken machen, sichern die Formulare Authentifizierungsticket, da in der Standardeinstellung wird sowohl verschlüsselt und signiert (um Manipulationen zu verhindern). Eine ausführlichere Beschreibung auf Formulare Ticket Authentifizierungssicherheit wird im folgenden Lernprogramm angezeigt.

> [!NOTE]
> Viele Finanz- und medizinischen Websites sind so konfiguriert, dass die Verwendung von SSL auf *alle* Seiten zugegriffen werden kann, um authentifizierte Benutzer. Wenn Sie einer solchen Website erstellen können Sie Formulare-Authentifizierungssystem konfigurieren, damit das Formularauthentifizierungsticket nur über eine sichere Verbindung übertragen wird. Betrachten wir die verschiedenen Optionen zur Authentifizierung Formulare in den nächsten Lernprogrammen  *[Konfiguration der Formularauthentifizierung und erweiterte Themen](../membership/creating-the-membership-schema-in-sql-server-vb.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Schritt 4: Erkennen von authentifizierten Besucher und ermitteln ihre Identität

An diesem Punkt wir Formularauthentifizierung aktiviert haben, und erstellt eine rudimentäre Anmeldeseite, jedoch können wir noch untersuchen, wie wir feststellen, ob ein Benutzer authentifiziert oder anonym ist. In bestimmten Szenarien möchten wir möglicherweise unterschiedliche Daten oder Informationen, je nachdem, ob ein authentifizierte oder anonyme Benutzer die Seite besucht angezeigt. Darüber hinaus müssen häufig die Identität des authentifizierten Benutzers zu kennen.

Wir ergänzen Sie die vorhandene "default.aspx"-Seite, um diese Verfahren zu veranschaulichen. Fügen Sie zwei Panel-Steuerelemente, die einen benannten AuthenticatedMessagePanel und eine andere benannte AnonymousMessagePanel in "default.aspx" hinzu. Fügen Sie ein Bezeichnungsfeld-Steuerelement mit dem Namen WelcomeBackMessage im ersten Bereich hinzu. Im zweiten Bereich ein Linksteuerelement hinzufügen, legen Sie seine Texteigenschaft auf anmelden und seine NavigateUrl-Eigenschaft, um ~ / "Login.aspx". An diesem Punkt sollte die deklarative Markup für "default.aspx" etwa wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Wahrscheinlich inzwischen abzulegen, wird der Grundgedanke nur die AuthenticatedMessagePanel authentifizierten Besucher und nur die AnonymousMessagePanel anonyme Besucher angezeigt. Zu diesem Zweck muss diese Bereiche angezeigten Eigenschaften je nachdem, ob der Benutzer oder nicht in angemeldet ist festgelegt werden.

Die [Request.IsAuthenticated Eigenschaft](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) gibt einen booleschen Wert, der angibt, ob die Anforderung authentifiziert wurde. Geben Sie den folgenden Code in die Seite\_Ereignishandlercode laden:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Finden Sie mit diesem Code werden über einen Browser auf "default.aspx". Vorausgesetzt, dass Sie die melden Sie sich noch vorhanden, wird einen Link zur Anmeldeseite angezeigt (siehe Abbildung 11). Klicken Sie auf diesen Link, und melden Sie sich an den Standort. Wie in Schritt 3 veranschaulichte Filtermenü, nachdem Sie Ihre Anmeldeinformationen eingegeben haben Sie kehren zum "default.aspx", aber dieses Mal die Seite zeigt die Willkommen zurück! Nachrichten Sie (siehe Abbildung 12).


[![Wenn Zugriff auf anonym, ein Protokoll In Link angezeigt wird](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Abbildung 11**: beim Zugriff auf anonym, ein Protokoll In Link angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image33.png))


[![Authentifizierte Benutzern wird gezeigt, die Willkommen zurück! Nachricht](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Abbildung 12**: authentifizierten Benutzern wird gezeigt, dass Willkommen zurück! Nachricht ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image36.png))


Wir können bestimmen, der derzeit angemeldete Benutzer Identität über die [HttpContext Objekt](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)des [Benutzereigenschaft](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). Das HttpContext-Objekt stellt Informationen über die aktuelle Anforderung dar und ist für allgemeine ASP.NET-Objekte wie Antwort, der Anforderung und der Sitzungsebene, u. a. Die Benutzer-Eigenschaft stellt den Sicherheitskontext des aktuellen HTTP-Anforderung und implementiert die [IPrincipal-Schnittstelle](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Die Benutzer-Eigenschaft wird von der FormsAuthenticationModule festgelegt. Insbesondere, wenn die FormsAuthenticationModule ein Formularauthentifizierungsticket in der eingehenden Anforderung findet, es erstellt ein neues GenericPrincipal-Objekt und die Benutzer-Eigenschaft zugewiesen.

Principal-Objekte (z. B. GenericPrincipal) enthalten Informationen zur Identität des Benutzers und die Rollen, zu denen sie gehören. IPrincipal-Schnittstelle definiert zwei Elemente:

- [IsInRole (*RoleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -eine Methode, die einen booleschen Wert, der angibt, ob der Prinzipal die angegebene Rolle angehört zurückgibt.
- [Identität](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) -eine Eigenschaft, die ein Objekt zurückgibt, implementiert die ["IIdentity" wird Schnittstelle](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). "IIdentity" wird-Schnittstelle definiert drei Eigenschaften: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), und [Namen](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Wir können den Namen des aktuellen Besuchers mithilfe des folgenden Codes feststellen:

Dim currentUsersName As String = User.Identity.Name

Bei der Verwendung, Formularauthentifizierung ein [FormsIdentity Objekt](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) für die GenericPrincipal-Identity-Eigenschaft erstellt wird. Die FormsIdentity-Klasse gibt immer die String-Formularen für die Eigenschaft AuthenticationType und "true" zurück, für die zugehörige IsAuthenticated-Eigenschaft. Die Name-Eigenschaft gibt den Benutzernamen angegeben, wenn die Formulare Authentifizierungsticket erstellen. Zusätzlich zu diesen drei Eigenschaften FormsIdentity umfasst den Zugriff auf das zugrunde liegende Authentifizierungsticket über seine [Eigenschaft Ticket](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). Die Ticket-Eigenschaft gibt ein Objekt vom Typ [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), dem verfügt über Eigenschaften, z. B. Ablauf, IsPersistent IssueDate, Name und So weiter.

Der wichtige Punkt vorwegzunehmen ist hier, dass die *Benutzername* in der FormsAuthentication.GetAuthCookie angegebene Parameter (*Benutzername*, *PersistCookie*), FormsAuthentication.SetAuthCookie (*Benutzername*, *PersistCookie*), und FormsAuthentication.RedirectFromLoginPage (*Benutzername*, *PersistCookie*) Methoden stimmt mit dem Wert von User.Identity.Name zurückgegeben. Darüber hinaus ist das Authentifizierungsticket erstellt, indem diese Methoden durch Umwandlung von User.Identity mit einem FormsIdentity-Objekt, und klicken Sie dann den Zugriff auf die Ticket-Eigenschaft verfügbar:

Dim Ident als FormsIdentity = CType (User.Identity, FormsIdentity)

Dim-Authentifizierungsticket als FormsAuthenticationTicket = Ident. Ticket

Wir bieten eine personalisierte Meldung in "default.aspx" ein. Die Seite "Aktualisieren"\_Laden Sie Ereignishandler, sodass die WelcomeBackMessage Bezeichnung Texteigenschaft zurück, für die Zeichenfolge Willkommen zugewiesen ist *Benutzername*!

WelcomeBackMessage.Text = "Welcome back, " &amp; User.Identity.Name &amp; "!"

Abbildung 13 zeigt die Auswirkung dieser Änderung (bei der Anmeldung als Benutzer Scott bei).


[![Die Willkommensnachricht enthält des derzeit angemeldeten, im Namen des Benutzers](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Abbildung 13**: die Willkommensnachricht schließt die derzeit protokolliert im Namen des Benutzers ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>Mithilfe der LoginView und LoginName-Steuerelemente

Anzeigen von unterschiedlichen Inhalt für authentifizierte und anonyme Benutzer ist es allgemein erforderlich; Daher ist der Name des aktuell angemeldeten Benutzers anzeigen. Aus diesem Grund schließt ASP.NET zwei Web-Steuerelemente, die angezeigt wird, in Abbildung 13, jedoch ohne eine einzige Codezeile schreiben zu müssen dieselbe Funktionalität bereitzustellen.

Die [LoginView-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) ein vorlagenbasierter-Websteuerelement, die authentifizierten und anonymen Benutzern unterschiedliche Daten anzeigen, erleichtert wird. Die LoginView umfasst zwei vordefinierte Vorlagen:

- AnonymousTemplate - Markup hinzugefügt, um diese Vorlage wird nur für anonyme Besucher angezeigt.
- LoggedInTemplate - Markup für diese Vorlage wird nur für authentifizierte Benutzer angezeigt werden.

Fügen Sie auf unserer Website Masterseite Site.master LoginView-Steuerelement. Statt nur das LoginView-Steuerelement hinzugefügt, aber fügen Sie sowohl ein neues ContentPlaceHolder-Steuerelement, und veröffentlichen Sie das LoginView-Steuerelement in diese neue ContentPlaceHolder aus. Der Grund für diese Entscheidung wird in Kürze offensichtlich sein.

> [!NOTE]
> Zusätzlich zu den AnonymousTemplate und LoggedInTemplate kann das LoginView-Steuerelement rollenspezifische Vorlagen enthalten. Rollenspezifische Vorlagen anzeigen Markup nur für die Benutzer, die zu einer bestimmten Rolle gehören. In einem späteren Lernprogramm untersuchen wir die rollenbasierte Funktionen von LoginView-Steuerelement.


Starten, indem Sie eine mit dem Namen LoginContent in der master-Seite in der Navigationsleiste ContentPlaceHolder &lt;Div&gt; Element. Sie können einfach eine ContentPlaceHolder-Steuerelement ziehen, aus der Toolbox auf die Datenquellensicht, platzieren das resultierende Markup rechts oben das TODO: Menü wird finden Sie hier Text.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Als Nächstes fügen Sie ein LoginView-Steuerelement innerhalb der LoginContent ContentPlaceHolder hinzu. Inhalt der Masterseite ContentPlaceHolder Steuerelemente abgelegt gelten *Standardinhalt* für ContentPlaceHolder. D. h. können ASP.NET-Seiten, die verwenden diese Masterseite Geben Sie ihre eigenen Inhalte für jede ContentPlaceHolder oder Standardinhalt der Masterseite.

Die LoginView und andere Steuerelemente Anmeldung bezogene befinden sich in der Toolbox-Registerkarte "Anmeldung".


[![LoginView-Steuerelement in der Toolbox](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Abbildung 14**: das LoginView-Steuerelement in der Toolbox ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image42.png))


Als Nächstes fügen Sie zwei &lt;Br /&gt; Elemente sofort nach dem LoginView-Steuerelement, aber immer noch innerhalb der ContentPlaceHolder. An diesem Punkt die Navigation &lt;Div&gt; des Elements Markup sollte wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Die LoginView-Vorlagen können in den Designer oder deklarativem Markup definiert werden. Erweitern Sie im Visual Studio-Designer die LoginView Smarttag dem konfigurierten Vorlagen in einer Dropdownliste aufgeführt sind. Geben Sie in den Text Hello, fremden in AnonymousTemplate; als Nächstes ein Linksteuerelement hinzufügen, und legen Sie die Text- und NavigateUrl-Eigenschaften zum Anmelden und ~ / "Login.aspx", bzw.

Nach dem Konfigurieren der AnonymousTemplate, wechseln Sie zu der LoggedInTemplate aus, und geben Sie den Text, "Hallo". Ziehen Sie dann ein LoginName-Steuerelement aus der Toolbox in den LoggedInTemplate, platzieren es sofort nach der "Willkommen zurück," Text. Die [LoginName Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), wie der Name andeutet, zeigt den Namen des aktuell angemeldeten Benutzers. Intern, gibt das Steuerelement LoginName einfach die User.Identity.Name-Eigenschaft

Nach dem Herstellen dieser Ergänzungen der LoginView-Vorlagen, sollte das Markup etwa wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Durch diesen Zusatz der Masterseite Site.master zeigt jede Seite in unserer Website eine andere Meldung abhängig davon, ob der Benutzer authentifiziert wird. Abbildung 15 zeigt die Seite "default.aspx" über einen Browser vom Benutzer Jisun besucht werden. Hallo, Jisun Meldung wird zweimal wiederholt: einmal in der Masterseite-Navigationsbereich auf der linken Seite (über die soeben hinzugefügte LoginView-Steuerelement) und einmal in der "default.aspx" Inhaltsbereich (über das Panel-Steuerelemente und programmgesteuerte Logik).


[![LoginView-Steuerelement zeigt Willkommen Back Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Abbildung 15**: die LoginView-Steuerelement zeigt Willkommen zurück, Jisun. ([Klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image45.png))


Da wir die Gestaltungsvorlage der LoginView hinzugefügt haben, können sie in jeder Seite auf unserer Website angezeigt. Allerdings gibt es möglicherweise Webseiten, in denen wir diese Meldung anzeigen möchten. Eine solche Seite ist die Anmeldeseite an, da ein Link zur Anmeldeseite nicht zur es scheint. Da wir das LoginView-Steuerelement in einem ContentPlaceHolder in die Masterseite platziert werden, können wir in unserer Inhaltsseite dieser Standardmarkup überschreiben. Öffnen Sie "Login.aspx", und wechseln Sie in den Designer. Da wir ein Inhaltssteuerelement keine explizit definierten in "Login.aspx" für die LoginContent ContentPlaceHolder in die Masterseite, wird die Anmeldeseite die Gestaltungsvorlage Standardmarkup für diese ContentPlaceHolder angezeigt. Sie können dies mithilfe des Designers - sehen LoginContent ContentPlaceHolder zeigt den Standardcode (LoginView-Steuerelement).


[![Die Seite "Login" zeigt die Standardeinstellung für die Gestaltungsvorlage LoginContent ContentPlaceHolder Inhalt](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Abbildung 16**: the Master Page's LoginContent ContentPlaceHolder zeigt die Anmeldeseite Inhalt standardmäßig ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image48.png))


Um den Standardcode für LoginContent ContentPlaceHolder zu überschreiben, mit der rechten Maustaste auf den Bereich im Designer und wählen Sie die Option erstellen Sie benutzerdefinierte Inhalt aus dem Kontextmenü. (Wenn mit Visual Studio 2008 ContentPlaceHolder enthält ein Smarttag, die bei Auswahl dieser Option bietet die Möglichkeit, dieselbe.) Dadurch wird ein neues Inhaltssteuerelement, das Markup der Seite und dadurch erlaubt es uns, die benutzerdefinierte Inhalte für diese Seite zu definieren. Fügen Sie hier eine benutzerdefinierte Meldung hinzu, z. B. Melden Sie sich, aber wir nur dieses Feld leer lassen.

> [!NOTE]
> In Visual Studio 2005, Erstellen von benutzerdefinierten Inhalten erstellt ein leeres Inhaltssteuerelement in der ASP.NET-Seite. In Visual Studio 2008 kopiert jedoch Erstellen von benutzerdefinierten Inhalten Standardinhalt der Masterseite in die neu erstellte Steuerelement. Bei Verwendung von Visual Studio 2008 nach dem Erstellen der neuen Inhaltssteuerelements stellen Sie dann sicher, dass der Inhalt der Masterseite kopiert löschen.


Abbildung 17 zeigt die Seite "Login.aspx" besucht über einen Browser, nachdem Sie diese Änderung vorgenommen haben. Beachten Sie, dass kein Hello, fremden oder Willkommen zurück *Benutzername* Nachricht in der linken Navigationsleiste &lt;Div&gt; wie es beim Aufrufen von "default.aspx" ist.


[![Die Anmeldeseite Blendet die Standard-LoginContent ContentPlaceHolder Markup](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Abbildung 17**: Anmeldungsseite Blendet die standardmäßige LoginContent ContentPlaceHolders Markup ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>Schritt 5: Abmelden

In Schritt 3 erläutert Erstellen einer Anmeldeseite um einen Benutzer auf der Website anmelden, jedoch können wir noch zu erfahren, wie einen Benutzer abzumelden. Zusätzlich zu den Methoden für die Protokollierung von eines Benutzers in der FormsAuthentication-Klasse stellt außerdem eine [SignOut Methode](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Die Methode SignOut zerstört einfach das Formularauthentifizierungsticket und Anmelden des Benutzers aus der Website.

Anbietet, dass eine Abmeldung Link solche keine gebräuchliche Funktion ist enthält, dass ASP.NET die ein Steuerelement, das speziell dazu konzipiert, melden Sie sich einen Benutzer an. Die [LoginStatus-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) zeigt entweder eine Anmeldung LinkButton oder Logout LinkButton, je nach den Authentifizierungsstatus des Benutzers. Eine Anmeldung LinkButton ist für anonyme Benutzer dargestellt, Logout LinkButton authentifizierten Benutzern angezeigt wird. Der Text für den Anmeldenamen und Logout LinkButtons kann so konfiguriert werden, über die LoginStatus LoginText und LogoutText Eigenschaften.

Klicken auf die Anmeldung LinkButton bewirkt, dass ein Postback handeln, von dem eine Umleitung zu der Anmeldeseite ausgegeben wird. Auf Logout LinkButton bewirkt, dass das Steuerelement LoginStatus zum Aufrufen der Methode FormsAuthentication.SignOff, und klicken Sie dann auf eine Seite leitet den Benutzer. Die Seite, die der angemeldete Benutzer umgeleitet, hängt von der LogoutAction-Eigenschaft, die einen der drei folgenden Werte zugewiesen werden kann:

- Refresh - die Standardeinstellung; leitet den Benutzer auf die Seite, die sie soeben besucht wurden. Wenn die Seite nur besuchten nicht anonyme Benutzer zulässig ist, wird die FormsAuthenticationModule automatisch den Benutzer zur Anmeldeseite umgeleitet.

Möglicherweise möchten Sie wissen, warum eine Umleitung hier ausgeführt wird. Wenn der Benutzer auf dem Laufenden bleiben möchte warum die Notwendigkeit für die explizite Umleitung? Der Grund ist, da beim Logoff LinkButton klicken, wird der Benutzer immer noch das Formularauthentifizierungsticket in die Auflistung der Cookies verfügt. Daher ist die postback Anforderung eine authentifizierte Anforderung an. LoginStatus-Steuerelement ruft die SignOut-Methode, aber in diesem Fall nach der FormsAuthenticationModule den Benutzer authentifiziert hat. Aus diesem Grund führt dazu, dass explizite Umleitung den Browser, um die Seite erneut angefordert. Nach der Zeit, die der Browser die Seite erneut anfordert, das Formularauthentifizierungsticket entfernt wurde, und die eingehende Anforderung ist deshalb anonym.

- Umleitung – der Benutzer wird an die von der LoginStatus LogoutPageUrl-Eigenschaft angegebene URL umgeleitet.
- RedirectToLoginPage – der Benutzer wird an die Anmeldeseite umgeleitet.

Wir der Gestaltungsvorlage ein LoginStatus-Steuerelement hinzufügen und konfigurieren, dass die Umleitung-Option verwenden, um den Benutzer zu einer Seite zu senden, die zeigt eine Meldung bestätigt, dass sie abgemeldet wurden. Zunächst erstellen eine Seite in das Stammverzeichnis, das mit dem Namen Logout.aspx. Vergessen Sie nicht die Stammwebsite auf dieser Seite zugeordnet werden soll. Geben Sie anschließend eine Nachricht im Markup der Seiten, die erläutern, die dem Benutzer, die sie abgemeldet wurden.

Als Nächstes zurückgeben Sie der Masterseite Site.master aus, und fügen Sie ein Steuerelement LoginStatus unterhalb der LoginView in LoginContent ContentPlaceHolder. Legen Sie LoginStatus LogoutAction Steuerelementeigenschaft an den Umleitungs und seine LogoutPageUrl-Eigenschaft auf ~/Logout.aspx an.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Da die LoginStatus außerhalb des LoginView-Steuerelements ist, sowohl anonyme als auch authentifizierte Benutzer angezeigt wird, aber das ist in Ordnung, da die LoginStatus einer Anmeldung oder Logout LinkButton ordnungsgemäß angezeigt wird. Durch das Hinzufügen des Steuerelements LoginStatus ist das Protokoll In HyperLink im AnonymousTemplate ist überflüssig, also zu entfernen.

Abbildung 18 zeigt "default.aspx" auf, wenn Jisun besucht. Beachten Sie, dass die linke Spalte wird die Meldung angezeigt, Willkommen Sie zurück, Jisun zusammen mit einem Link zum Abmelden. Klicken auf die Abmelden LinkButton einen Postback verursacht, Jisun aus dem System signiert und leitet sie dann an Logout.aspx. Wie in Abbildung 19 gezeigt, nach der Zeit, die jisun Logout.aspx erreicht, er wurde bereits abgemeldet und ist deshalb anonym. Infolgedessen zeigt die linke Spalte den Text Willkommen, fremden und einen Link zur Anmeldeseite.


[!["Default.aspx" zeigt Willkommen zurück, zusammen mit einer Logout LinkButton Jisun](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Abbildung 18**: "default.aspx" zeigt Willkommen zurück, Jisun zusammen mit einem Logout LinkButton ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx zeigt Willkommen, fremden zusammen mit einer Anmeldung LinkButton](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Abbildung 19**: Logout.aspx zeigt Willkommen, fremden zusammen mit einer Anmeldung LinkButton ([klicken Sie hier, um das Bild in voller Größe angezeigt](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> Überzeugen Sie sich auf der Seite "Logout.aspx", um der Gestaltungsvorlage LoginContent ContentPlaceHolder auszublenden (z. B. "Login.aspx" in Schritt 4 haben) anpassen. Der Grund hierfür ist, da die Anmeldung LinkButton von LoginStatus-Steuerelement gerendert (das Element unterhalb Hello, fremden) sendet die Benutzer auf der Anmeldeseite die aktuelle URL in der ReturnUrl Querystring-Parameter übergeben. Kurz gesagt, wenn ein Benutzer abgemeldet hat diese LoginStatus Anmeldung LinkButton klickt, und klicken Sie dann anmeldet, werden sie wieder an Logout.aspx, umgeleitet werden die einfach den Benutzer verwirrend sein kann.


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm werden mit einer Prüfung des Workflows Authentifizierung Formulare gestartet und dann für die Implementierung der Formularauthentifizierung in einer ASP.NET-Anwendung aktiviert. Formularauthentifizierung beruht die FormsAuthenticationModule, der zwei Aufgaben ausführt: Identifizieren von Benutzern basierend auf deren Formularauthentifizierungsticket und nicht autorisierte Benutzer zur Anmeldeseite umgeleitet.

Das .NET Framework FormsAuthentication-Klasse enthält Methoden zum Erstellen, überprüfen und Entfernen von Formularauthentifizierungstickets. Der Request.IsAuthenticated-Eigenschaft und dem Benutzerobjekt bieten zusätzliche programmgesteuerte Unterstützung zur Bestimmung, ob eine Anforderung authentifiziert wird und Informationen über die Identität des Benutzers ein. Es gibt auch die LoginView LoginStatus und LoginName-Web-Steuerelemente, die Entwicklern eine Möglichkeit für schnelle und Freigeben von Code für viele häufige Login-bezogene Aufgaben. Wir werden diese und andere Anmeldung bezogene Websteuerelemente ausführlicher in zukünftigen Lernprogrammen untersucht.

In diesem Lernprogramm bereitgestellten eine kurze Übersicht über die Formularauthentifizierung verwenden. Wir nicht Sortiment Konfigurationsoptionen untersuchen, sehen Sie sich wie cookieless Forms Authentifizierung Tickets Arbeit oder untersuchen den Schutz von ASP.NET auf den Inhalt der das Formularauthentifizierungsticket. Besprechen wir diese Themen und mehr in der [nächsten Lernprogramm](forms-authentication-configuration-and-advanced-topics-vb.md).

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Unterschiede zwischen der IIS 6 und IIS 7-Sicherheit](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [ASP.NET Anmeldesteuerelementen](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professionelle ASP.NET 2.0 Sicherheit, Mitgliedschaft und Rollenverwaltung](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Die &lt;Authentifizierung&gt; Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Die &lt;Forms&gt; -Element für &lt;Authentifizierung&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Lehrvideos auf die Themen in diesem Lernprogramm

- [Verwenden der grundlegenden Formularauthentifizierung in ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm enthalten Alicja Maziarz, John Suru und Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Zurück](security-basics-and-asp-net-support-vb.md)
[Weiter](forms-authentication-configuration-and-advanced-topics-vb.md)
