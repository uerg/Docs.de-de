---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: Einen Überblick über die Formularauthentifizierung (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden wir in reine Auseinandersetzung mit Angular Implementierung aktivieren; insbesondere betrachten wir die Formularauthentifizierung implementieren. Der Web Application-w...
ms.author: aspnetcontent
ms.date: 01/14/2008
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 757cdebc436a4cb799f92374744ee9cb69eb0e0b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809458"
---
<a name="an-overview-of-forms-authentication-vb"></a>Einen Überblick über die Formularauthentifizierung (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> In diesem Tutorial werden wir in reine Auseinandersetzung mit Angular Implementierung aktivieren; insbesondere betrachten wir die Formularauthentifizierung implementieren. Die Webanwendung, die in diesem Tutorial erstellen zunächst wird weiterhin auf, die in den nachfolgenden Tutorials erstellt werden wie einfache Formularauthentifizierung, Mitgliedschaft und Rollen wechseln.
> 
> Informieren Sie sich dieses Video für Weitere Informationen zu diesem Thema: [mithilfe von grundlegenden Formularauthentifizierung in ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](security-basics-and-asp-net-support-vb.md) erläutert verschiedene Authentifizierung, Autorisierung, und Optionen für das Konto von ASP.NET bereitgestellt wird. In diesem Tutorial werden wir in reine Auseinandersetzung mit Angular Implementierung aktivieren; insbesondere betrachten wir die Formularauthentifizierung implementieren. Die Webanwendung, die in diesem Tutorial erstellen zunächst wird weiterhin auf, die in den nachfolgenden Tutorials erstellt werden wie einfache Formularauthentifizierung, Mitgliedschaft und Rollen wechseln.

In diesem Lernprogramm beginnt mit einen detaillierten Einblick in die Formulare den Workflow der Authentifizierung, ein Thema, das wir im vorherigen Tutorial bei berührt. Danach erstellen wir eine ASP.NET-Website durch die die Konzepte der Formularauthentifizierung-demo. Als Nächstes konfigurieren wir die Website, um den Formular-Authentifizierung verwenden, erstellen eine einfache Anmeldeseite und erfahren, wie im Code, um zu bestimmen, ob ein Benutzer authentifiziert wird und wenn dies der Fall ist, den Benutzernamen sie sich angemeldet haben.

Grundlegendes zu den Formularen, Workflow der Authentifizierung, aktivieren es in einer Webanwendung, und Erstellen der Anmeldung und Abmeldung werden alle wichtigen Schritte zum Erstellen einer ASP.NET-Anwendung aus, die Benutzerkonten unterstützt und authentifiziert Benutzer über eine Webseite. Aus diesem - Grund und da in diesen Tutorials auf anderen - aufbauen ich empfehle Ihnen, vor dem Fortfahren mit dem nächsten fort, auch wenn Sie bereits Erfahrung konfigurieren Waren in früheren Projekten Formularauthentifizierung für dieses Tutorial vollständig zu arbeiten.

## <a name="understanding-the-forms-authentication-workflow"></a>Grundlegendes zu den Workflow der Forms-Authentifizierung

Wenn die ASP.NET-Laufzeitumgebung eine Anforderung für eine ASP.NET-Ressource, z. B. eine ASP.NET-Seite oder ASP.NET Web Service verarbeitet löst die Anforderung eine Anzahl von Ereignissen während des Lebenszyklus. Es sind Ereignisse, die am Ende der Anforderung, solche ausgelöst, wenn die Anforderung authentifiziert wird und autorisiert, ein Ereignis bereit, die im Falle einer nicht behandelten Ausnahme usw. sehr ab und sehr ausgelöst. Eine vollständige Liste der Ereignisse, finden Sie in der [Ereignisse des HttpApplication-Objekts](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*HTTP-Module* sind verwaltete Klassen, deren Code als Reaktion auf ein bestimmtes Ereignis im Anforderungslebenszyklus von ausgeführt wird. Im Lieferumfang von ASP.NET sind einer Reihe von HTTP-Modulen, die Durchführung wichtiger Aufgaben im Hintergrund. Zwei integrierten HTTP-Module, die für unsere Diskussion besonders relevant sind:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  -authentifiziert den Benutzer durch Überprüfen von Formularauthentifizierungstickets, das in der Regel in die Auflistung der Cookies des Benutzers enthalten ist. Wenn keine Formularauthentifizierungsticket vorhanden ist, ist der Benutzer anonym.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -bestimmt, ob der aktuelle Benutzer berechtigt ist, auf die angeforderte URL zuzugreifen. Dieses Modul ermittelt die Autorität für die von die Autorisierungsregeln, die in Konfigurationsdateien der Anwendung angegebenen consulting. ASP.NET enthält außerdem die [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) , der Autorität für die von der angeforderten Dateien ACLs consulting bestimmt.

Die FormsAuthenticationModule versucht, den Benutzer vor dem Ausführen von UrlAuthorizationModule (und FileAuthorizationModule) zu authentifizieren. Wenn der Benutzer die Anforderung nicht autorisiert ist, auf die angeforderte Ressource zuzugreifen, wird das Authentifizierungsmodul beendet die Anforderung und gibt eine [HTTP 401 Unauthorized](http://www.checkupdown.com/status/E401.html) Status. In Szenarien für Windows-Authentifizierung wird der HTTP 401-Status an den Browser zurückgegeben. Dieser Statuscode bewirkt, dass der Browser fordert den Benutzer zur Eingabe der Anmeldeinformationen über ein modales Dialogfeld. Bei der Formularauthentifizierung jedoch der HTTP 401 Unauthorized Status wird nie an den Browser gesendeten daran, dass die FormsAuthenticationModule dieser Status erkennt und wird geändert, um den Benutzer zur Anmeldeseite stattdessen eine Umleitung (über eine [HTTP 302-Umleitung](http://www.checkupdown.com/status/E302.html) Status).

Verantwortung für die Anmeldeseite ist zu ermitteln, ob die Anmeldeinformationen des Benutzers gültig sind und, wenn dies der Fall ist, erstellen ein Formularauthentifizierungsticket aus, und leiten Sie den Benutzer zurück zur Seite sie haben versucht, besuchen. Das Authentifizierungsticket ist in nachfolgenden Anforderungen an den Seiten auf der Website enthalten, die die FormsAuthenticationModule verwendet wird, um den Benutzer zu identifizieren.


[![Workflow der Forms-Authentifizierung](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Abbildung 01**: Workflow für das Forms-Authentifizierung ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Speichern Sie das Authentifizierungsticket über Seite besuchen

Nach der Anmeldung muss das Formularauthentifizierungsticket zurück an den Webserver bei jeder Anforderung gesendet werden, damit der Benutzer angemeldet bleiben, wie sie auf die Website durchsuchen. Dies erfolgt normalerweise durch das Authentifizierungsticket platzieren, in die Auflistung der Cookies des Benutzers. [Cookies](http://en.wikipedia.org/wiki/HTTP_cookie) sind kleine Textdateien, die auf dem Computer des Benutzers befinden, und bei jeder Anforderung an die Website, die das Cookie erstellt die HTTP-Header gesendet werden. Sobald das Formularauthentifizierungsticket erstellt und in der Browser die Cookies gespeichert wurde, sendet jede nachfolgende Besuch an diesem Standort aus diesem Grund das Authentifizierungsticket zusammen mit der Anforderung, und Identifizieren des Benutzers.

> [!NOTE]
> Die Demo-Webanwendung verwendet, die in jedem Lernprogramm ist als Download verfügbar. Dieses herunterladbare Anwendung, die mit Visual Web Developer 2008 als Ziel für .NET Framework, Version 3.5 erstellt wurde. Da die Anwendung für .NET 3.5 ausgerichtet ist, enthält die Datei "Web.config" zusätzliche, 3.5-spezifische Konfigurationselemente. Kurz gesagt, wenn Sie noch so installieren Sie .NET 3.5 auf Ihrem Computer klicken Sie dann die herunterladbare Webanwendung kann nicht ausgeführt, ohne zuvor die 3.5-spezifisches Markup aus "Web.config" entfernt.


Ein Aspekt von Cookies ist abgelaufen, Datum und Uhrzeit, an dem der Browser Cookies verworfen, wird. Wenn die formularauthentifizierungs-Cookie abläuft, wird der Benutzer kann, nicht mehr authentifiziert und aus diesem Grund werden anonyme. Wenn sich ein Benutzer über das öffentliche Terminal besuchen ist, ist es wahrscheinlich, dass sie ihre Authentifizierungsticket abläuft, wenn sie ihren Browser schließen möchten. Beim Besuch von zu Hause aus, der gleiche Benutzer können jedoch das Authentifizierungsticket über den Browser neu gestartet wird, damit keine Speichern jedes Mal, die sie von der Website erneut anmelden. Diese Entscheidung wird häufig durch die Benutzer in Form eines Kontrollkästchens Anmeldedaten auf der Anmeldeseite vorgenommen. In Schritt 3 wird zum Implementieren eines Kontrollkästchens Anmeldedaten auf der Anmeldungsseite untersucht. Im folgende Tutorial werden die Authentifizierung Ticket-Leerlauftimeout-Einstellungen ausführlich behandelt.

> [!NOTE]
> Es ist möglich, dass der Benutzer-Agent für die Anmeldung bei der Websites verwendet Cookies möglicherweise nicht unterstützt. In diesem Fall können ASP.NET ohne Cookies Formularauthentifizierungstickets. In diesem Modus wird das Authentifizierungsticket in der URL codiert. Betrachten wir, wenn Authentifizierung ohne Cookies Tickets verwendet werden und wie sie erstellt und im nächsten Tutorial verwaltet werden.


### <a name="the-scope-of-forms-authentication"></a>Der Bereich des Formular-Authentifizierung

Die FormsAuthenticationModule verwaltet wird code, der Teil der ASP.NET-Laufzeit. Vor Version 7 der Microsoft [(Internet Information Services, IIS)](https://www.iis.net/) Webserver, gab es eine unterschiedliche Barriere zwischen IISs-HTTP-Pipeline und der ASP.NET-Laufzeit-Pipeline. Kurz gesagt, in IIS 6 und früheren Versionen wird die FormsAuthenticationModule nur ausgeführt, wenn eine Anforderung von IIS für die ASP.NET-Laufzeitumgebung delegiert werden kann. Standardmäßig verarbeitet IIS statischen Inhalt selbst – wie HTML-Seiten und CSS-Image - und -Dateien nur übergibt die Anforderungen an die ASP.NET-Laufzeitumgebung aus, wenn eine Seite mit der Erweiterung ASPX-, ASMX- oder ashx angefordert wird.

IIS 7, ermöglicht jedoch integrierte IIS und ASP.NET Pipelines. Einige Konfigurationseinstellungen können Sie IIS 7 zum Aufrufen der FormsAuthenticationModule für einrichten *alle* Anforderungen. Darüber hinaus können Sie URL-Autorisierungsregeln für die Dateien eines beliebigen Typs mit IIS 7 definieren. Weitere Informationen finden Sie unter [Änderungen zwischen IIS 6 und IIS 7-Sicherheit](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [Sicherheit für Ihre Web-Plattform](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), und [Verständnis IIS7-URL-Autorisierung](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Lange Rede kurzer, in den Versionen vor IIS 7, nur die Formularauthentifizierung können Sie beim Schützen von Ressourcen, die von der ASP.NET-Laufzeit behandelt. Ebenso werden Regeln auf URL-Autorisierung nur auf Ressourcen, die von der ASP.NET-Laufzeit behandelt angewendet. Aber bei IIS 7 ist es möglich, die FormsAuthenticationModule und UrlAuthorizationModule in so zu erweitern und diese Funktion auf alle Anforderungen IISs-HTTP-Pipeline zu integrieren.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Schritt 1: Erstellen einer ASP.NET-Website für diese Tutorialreihe

Um die größtmögliche Zielgruppe erreichen, wir erstellen in der gesamten diese Reihe von, ASP.NET-Website mit Microsofts kostenlose Version von Visual Studio 2008 erstellt [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Wir implementieren SqlMembershipProvider Benutzerspeicher in einem [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) Datenbank. Wenn Sie Visual Studio 2005 oder eine andere Edition von Visual Studio 2008 oder SQL Server verwenden, keine Sorge – die Schritte fast identisch und alle nicht trivialen Unterschiede werden hingewiesen.

Bevor wir die Formularauthentifizierung konfigurieren können, benötigen wir zuerst eine ASP.NET-Website. Zunächst erstellen eine neue Datei systembasierte ASP.NET-Website. Um dies zu erreichen, starten Sie Visual Web Developer, wechseln Sie zu dem Menü "Datei", und klicken Sie neue Website, die das Dialogfeld Neue Website anzuzeigen. Wählen Sie die Vorlage für ASP.NET Web Site, legen Sie die Dropdownliste den Speicherort auf Dateisystem, wählen Sie einen Ordner aus, um die Website zu platzieren und legen Sie VB. Dadurch wird eine neue Website erstellt, mit einer Seite "default.aspx" ASP.NET App\_Datenordner, und eine Datei "Web.config".

> [!NOTE]
> Visual Studio unterstützt zwei Modi des Projektmanagements: Websiteprojekten und Webanwendungsprojekten. Web Site Projects fehlt eine Projektdatei, während Web Application Projects die Projektarchitektur in Visual Studio .NET 2002/2003 imitieren-sie enthalten eine Projektdatei, und Kompilieren von Quellcode des Projekts in eine einzelne Assembly, die im Ordner "/ bin" gespeichert ist. Visual Studio 2005 Projekt anfangs nur unterstützte Website, obwohl das Webanwendungsprojekt-Modell mit Service Pack 1 wieder eingeführt wurde. Visual Studio 2008 bietet beide Projektmodelle. Die Visual Web Developer 2005 und 2008-Editionen unterstützen jedoch nur von Websiteprojekten. Ich werden das Websiteprojekt-Modell verwenden. Wenn Sie eine nicht-Express-Edition verwenden und, verwenden Sie möchten die [Webanwendungsprojekt-Modell](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) stattdessen gerne tun, aber beachten Sie, dass es möglicherweise einige Diskrepanzen zwischen der Anzeige auf Ihrem Bildschirm und welche Schritte Sie müssen im Vergleich zu den dargestellten bildschirmabbildungen und Anweisungen, die in diesen Tutorials.


[![Erstellen einer neuen System-basierte-Website](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Abbildung 02**: Erstellen einer Website New File System-Based ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>Eine Masterseite hinzufügen

Als Nächstes fügen Sie eine neue Master-Seite, an die Website in das Stammverzeichnis, das mit dem Namen "Site.Master". [Masterseiten](https://msdn.microsoft.com/library/wtxbf3hh.aspx) kann ein Seitenentwickler, um eine standortweite Vorlage definieren, die auf ASP.NET-Seiten angewendet werden können. Der wichtigste Vorteil von Masterseiten ist, dass die Gesamtdarstellung der Website definiert werden kann an einem zentralen Ort, sodass es einfach zu aktualisieren oder Ändern des Standorts Layout.


[![Fügen Sie eine Master-Seite mit dem Namen "Site.Master" auf der Website](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Abbildung 03**: Fügen Sie eine Master Seite mit dem Namen "Site.Master" auf der Website ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image9.png))


Definieren Sie die standortweite Seitenlayout hier auf der Masterseite. Sie verwenden Sie die Entwurfsansicht zu sehen und beliebige Layout oder Web-Steuerelemente, die benötigten hinzufügen können, oder Sie können das Markup manuell hinzufügen, die manuell in der Datenquellensicht. Ich meine Masterseite Layout imitieren, die das Layout in strukturierten meine *[arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)* Reihe von Lernprogrammen (siehe Abbildung 4). Die Masterseite [cascading Stylesheets](http://www.w3schools.com/css/default.asp) zum Positionieren und Stile, die mit den CSS-Einstellungen, die in der Datei "Style.CSS" (die in diesem Tutorial zugehörige Download enthalten ist) definiert. Während Sie aus dem unten gezeigten Markup nicht erkennen können, werden die CSS-Regeln definiert, dass die Navigation &lt;Div&gt;Inhalt ist absolut positioniert, damit sie auf der linken Seite angezeigt wird und eine feste von 200 Pixel Breite.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Eine Masterseite definiert sowohl statische Seitenlayouts und Bereiche, die durch die ASP.NET-Seiten bearbeitet werden können, die die Masterseite verwenden. Dieser Inhalt bearbeitbare Bereiche werden durch die ContentPlaceHolder-Steuerelement, das im Inhalt angezeigt werden angegeben &lt;Div&gt;. Unsere Masterseite verfügt über ein einzelnes ContentPlaceHolder-Objekt (MainContent), aber der Masterseite möglicherweise mehrere ContentPlaceHolder-Steuerelemente.

Mit dem Markup oben eingegebenen zeigt das Umschalten zur Entwurfsansicht der Masterseite Layout aus. Alle ASP.NET-Seiten, die diese Masterseite verwenden, müssen diese einheitliche Layout, mit der Möglichkeit, geben Sie das Markup für die Region MainContent.


[![Die Masterseite, wenn Sie über die Entwurfsansicht angezeigt](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Abbildung 04**: der Masterseite, wenn angezeigt durch das Design View ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>Erstellen von Inhaltsseiten

An diesem Punkt haben wir eine Default.aspx-Seite in unsere Website, aber es verwendet keine Masterseite, die wir gerade erstellt haben. Es ist zwar möglich, deklarative Markup einer Webseite mit einer Masterseite, bearbeiten, wenn die Seite keine Inhalte enthält ist es noch einfacher, einfach löschen Sie die Seite, und fügen es erneut auf das Projekt, das Angeben der Masterseite verwenden. Daher löschen Sie "default.aspx" aus dem Projekt gestartet werden.

Klicken Sie dann mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie ein neues Webformular mit dem Namen "default.aspx" hinzufügen. Diesmal, aktivieren Sie das Kontrollkästchen für die Masterseite auswählen und wählen Sie die Masterseite Site.master aus der Liste aus.


[![Hinzufügen einer neuen Default.aspx-Seite, die auf einer Masterseite auswählen](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Abbildung 05**: Hinzufügen einer neuen "default.aspx" Seite auswählen einer Masterseite auswählen ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Verwenden Sie die Site.master-Masterseite](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Abbildung 06**: Verwenden der Site.master-Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> Bei Verwendung der Webanwendungsprojekt-Modell umfasst das Dialogfeld "Neues Element hinzufügen" keine Kontrollkästchen Masterseite auswählen. Stattdessen müssen Sie ein Element vom Typ Webinhaltsformular hinzufügen. Nach Auswahl der Option Webinhaltsformular, und klicken Sie auf Hinzufügen, zeigt Visual Studio die gleiche SELECT-Anweisung einem Masterauftrag für den in Abbildung 6 dargestellten Dialogfeld.


Deklaratives Markup für die neue Default.aspx-Seite enthält nur eine @Page Seite Richtlinie, die Angabe des Pfads zum Master der Masterseite MainContent ContentPlaceHolder-Datei und ein ContentControl-Element.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

"Default.aspx" lassen Sie vorerst leer. Wir werden später in diesem Lernprogramm zum Hinzufügen von Inhalt zu dieser zurückkehren.

> [!NOTE]
> Unsere Masterseite enthält einen Abschnitt eines Menüs oder einer anderen Navigationsschnittstelle. In einem späteren Tutorial erstellen wir eine solche Schnittstelle.


## <a name="step-2-enabling-forms-authentication"></a>Schritt 2: Aktivieren der Formularauthentifizierung

Mit der ASP.NET-Website erstellt haben besteht unsere nächste Aufgabe Formularauthentifizierung zu ermöglichen. Die Konfiguration der Anwendung-Authentifizierung angegeben ist, über die [ &lt;Authentifizierung&gt; Element](https://msdn.microsoft.com/library/532aee0e.aspx) in "Web.config". Die &lt;Authentifizierung&gt; Element enthält ein einzelnes Attribut mit dem Namen Mode, der angibt, das von der Anwendung verwendeten-Authentifizierungsmodell. Dieses Attribut kann einen der folgenden vier Werte aufweisen:

- **Windows** – wie im vorherigen Tutorial erläutert wird, wenn eine Anwendung auf Windows-Authentifizierung verwendet es obliegt dem Webserver zum Authentifizieren des Besuchers, und dies erfolgt in der Regel über Standard-, Digest- oder integrierter Windows die Authentifizierung.
- **Forms**-Benutzer werden über ein Formular auf einer Webseite authentifiziert.
- **Passport**-Benutzer werden mit der Microsoft Passport-Netzwerk authentifiziert.
- **Keine**-kein Authentifizierungsmodell verwendet; alle Besucher anonym sind.

ASP.NET-Anwendungen verwenden standardmäßig die Windows-Authentifizierung. Um den Authentifizierungstyp zur Formularauthentifizierung zu ändern, dann müssen wir ändern die &lt;Authentifizierung&gt; Mode-Attribut des Elements zu Forms.

Wenn Ihr Projekt noch nicht über eine Web.config-Datei enthält, fügen Sie eine jetzt durch mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, neues Element hinzufügen auswählen und dann eine Webkonfigurationsdatei hinzufügen.


[![Wenn das Projekt "Web.config" noch nicht enthalten ist, fügen Sie ihn jetzt](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Abbildung 07**: Wenn Ihr Projekt wird nicht noch enthalten "Web.config", fügen sie jetzt ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image21.png))


Wechseln Sie als Nächstes die &lt;Authentifizierung&gt; -Element, und aktualisieren ihn zur Verwendung der Formularauthentifizierung. Nach dieser Änderung sollte Ihre Datei "Web.config" Markup etwa wie folgt aussehen:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Da die Datei "Web.config" eine XML-Datei ist, ist die Schreibweise wichtig. Stellen Sie sicher, dass Sie Formulare, mit dem Großbuchstaben f Mode-Attribut festlegen Bei Verwendung unterschiedlicher Groß-/Kleinschreibung, z. B. Formulare, erhalten Sie beim Besuch der Website über einen Browser einen Fehler bei der Konfiguration.


Die &lt;Authentifizierung&gt; Element kann optional enthalten eine &lt;Forms&gt; untergeordneten-Element, das Forms-Authentifizierung-spezifischen Einstellungen enthält. Moment wir verwenden die standardmäßigen Einstellungen für die Formularauthentifizierung. Wir untersuchen die &lt;Forms&gt; untergeordnetes Element ausführlicher im nächsten Tutorial.

## <a name="step-3-building-the-login-page"></a>Schritt 3: Erstellen der Anmeldeseite

Um die Formularauthentifizierung benötigt unsere Website, eine Anmeldeseite. Siehe das Verständnis, die im Abschnitt der FormsAuthenticationModule Workflows für Forms-Authentifizierung automatisch umgeleitet wird autorisiert, wenn sie versuchen, eine Seite zuzugreifen, die sie nicht der Benutzer zur Anmeldeseite, anzuzeigen. Es gibt auch ASP.NET Web-Steuerelemente, die einen Link zur Anmeldeseite für anonyme Benutzer angezeigt werden. Dies führt zu folgender Frage, was die URL der Anmeldeseite ist?

Standardmäßig das Authentifizierungssystem Forms erwartet, dass die Anmeldeseite "Login.aspx" benannt werden und in das Stammverzeichnis der Webanwendung. Wenn Sie eine anderen Anmeldeseiten-URL verwenden möchten, können Sie dazu in "Web.config" angeben. Sehen wir im nachfolgenden Tutorial hierzu ein.

Die Anmeldeseite hat drei Aufgaben:

1. Geben Sie eine Schnittstelle, die den Besucher, seine Anmeldeinformationen eingeben kann.
2. Bestimmt, ob die übermittelten Anmeldeinformationen gültig sind.
3. Melden Sie sich der Benutzer, durch das Erstellen der Forms-Authentifizierungsticket.

### <a name="creating-the-login-pages-user-interface"></a>Erstellen der Benutzeroberfläche für die Anmeldeseite

Erste Schritte mit der ersten Aufgabe. Fügen Sie eine neue ASP.NET-Seite in der Website-Root-Verzeichnis mit dem Namen "Login.aspx", und ordnen sie die Masterseite Site.master.


[![Hinzufügen einer neuen ASP.NET Seite mit dem Namen "Login.aspx"](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Abbildung 08**: Hinzufügen einer neuen ASP.NET Seite mit dem Namen "Login.aspx" ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image24.png))


Die typische Login-Seite-Schnittstelle besteht aus zwei Textfelder ein – eine für den Namen des Benutzers, eine für die Kennwort- und eine Schaltfläche zum Senden des Formulars. Websites enthalten häufig ein Remember me-Kontrollkästchen, das, wenn dieses Kontrollkästchen aktiviert, das sich ergebende Authentifizierungsticket über den Browser neu gestartet wird weiterhin besteht.

Fügen Sie zwei Textfelder an "Login.aspx" aus, und legen Sie deren ID-Eigenschaften auf Benutzername und Kennwort, aus bzw. Kennwort des TextMode-Eigenschaft kann ebenfalls für das Kennwort festgelegt werden. Fügen Sie ein CheckBox-Steuerelement, und die ID-Eigenschaft auf RememberMe und seine Text-Eigenschaft, die mir unzeitgemäß Denken Sie daran festlegen Danach fügen Sie eine Schaltfläche namens LoginButton, deren Text-Eigenschaft, die Anmeldung festgelegt ist. Abschließend fügen Sie ein Label-Steuerelement hinzu und legen Sie seine ID-Eigenschaft auf InvalidCredentialsMessage, seine Texteigenschaft auf Ihr Benutzername oder Kennwort ist ungültig. Bitte versuchen Sie es., die ForeColor-Eigenschaft in Rot, und seine Visible-Eigenschaft auf "false".

An diesem Punkt Ihr Bildschirm sollte ähnlich wie im Screenshot in Abbildung 9 aussehen, und deklarative Syntax des Ihrer Seite sollte wie folgt aus:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![Die Anmeldeseite enthält, zwei Textfelder, Kontrollkästchen, eine Schaltfläche und einer Bezeichnung](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Abbildung 09**: die Anmeldung enthält zwei Textfelder der Seite, ein Kontrollkästchen, eine Schaltfläche und einer Bezeichnung ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image27.png))


Abschließend erstellen Sie einen Ereignishandler für die LoginButtons auf Ereignis. Doppelklicken Sie aus dem Designer einfach auf das Schaltflächen-Steuerelement, um Ereignishandler zu erstellen.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Ermitteln, ob die angegebenen Anmeldeinformationen gültig sind

Jetzt müssen wir Implementieren von Aufgabe 2 in der Schaltfläche auf Ereignishandler - bestimmen, ob die angegebenen Anmeldeinformationen gültig sind. Zu diesem Zweck muss es einen Benutzerspeicher, der alle von der Benutzer Anmeldeinformationen enthält, so dass wir ermitteln, ob die angegebenen Anmeldeinformationen mit bekannten Anmeldeinformationen übereinstimmen.

Vor ASP.NET 2.0 konnten Entwickler verantwortlich für die Implementierung von sowohl ihre eigenen Benutzer speichert, und den Code, um zu überprüfen, ob die angegebenen Anmeldeinformationen für den Speicher zu schreiben. Die meisten Entwickler würde den Speicher des Benutzers in einer Datenbank implementieren, erstellen eine Tabelle namens Benutzer mit Spalten, z. B. Benutzername, Kennwort, e-Mail-, LastLoginDate usw. Diese Tabelle würde dann, einen Datensatz pro Benutzerkonto verfügen. Überprüfen die Anmeldeinformationen eines Benutzers angegebenen würde betreffen, Abfragen der Datenbank zum Abgleichen des Benutzernamens ein, und klicken Sie dann sicherstellen, dass das Kennwort in der Datenbank auf das angegebene Kennwort entsprach.

Mit ASP.NET 2.0 sollten Entwickler der Mitgliedschaftsanbieter mithilfe einer um im Benutzerspeicher zu verwalten. In diesem Tutorial werden wir die SqlMembershipProvider verwenden die SQL Server-Datenbank für den Speicher des Benutzers verwendet. Bei Verwendung der SqlMembershipProvider müssen wir eine bestimmte Datenbank-Schema zu implementieren, die Tabellen, Sichten und gespeicherte Prozeduren erwartet, die vom Anbieter enthält. Untersuchen wir zum Implementieren dieses Schema in der *[erstellen das Schema für die Mitgliedschaft in SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)* Lernprogramm. Mit dem Mitgliedschaftsanbieter vorhanden, überprüfen die Anmeldeinformationen des Benutzers ist so einfach wie das Aufrufen der [Mitgliedschaftsklasse](https://msdn.microsoft.com/library/system.web.security.membership.aspx)des [ValidateUser (*Benutzername*, *Kennwort*) Methode](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), ein boolescher Wert, der zurückgegeben, ob die Gültigkeit der *Benutzername* und *Kennwort* Kombination. Wie sehen wir die SqlMembershipProviders Benutzerspeichers noch nicht implementiert haben, verwenden wir zu diesem Zeitpunkt kann nicht ValidateUser-Methode der Membership-Klasse.

Anstatt die Zeit in Anspruch nehmen erstellen eigene benutzerdefinierte Benutzer-Datenbanktabelle (der veraltet wäre, wenn wir die SqlMembershipProvider implementiert), lassen Sie uns stattdessen hartcodieren Seite die gültige Anmeldeinformationen innerhalb der Anmeldename selbst. Die LoginButtons Click-Ereignishandler, fügen Sie den folgenden Code hinzu:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Wie Sie sehen können, es werden drei gültige Benutzerkonten - Scott Jisun und Sam - und alle drei verfügen über das gleiche Kennwort (Kennwort). Der Code durchläuft die Benutzer und Kennwörter Arrays, die nach einer gültigen Benutzernamen und das Kennwort Übereinstimmung gesucht ab. Wenn sowohl der Benutzername und Kennwort gültig sind, müssen wir die Anmeldung des Benutzers, und klicken Sie dann eine Umleitung an die entsprechende Seite. Wenn die Anmeldeinformationen ungültig sind, wird die Bezeichnung InvalidCredentialsMessage angezeigt.

Wenn ein Benutzer gültige Anmeldeinformationen eingibt, erwähnt habe ich, dass sie Sie dann auf die entsprechende Seite umgeleitet werden. Was ist jedoch die entsprechende Seite? Denken Sie daran, dass wenn ein Benutzer eine Seite, die sie nicht autorisiert sind besucht, um anzuzeigen, die FormsAuthenticationModule automatisch sie zur Anmeldeseite umgeleitet. In diesem Fall enthält sie die angeforderte URL in der Abfragezeichenfolge über den Parameter "ReturnUrl" nachzuweisen. Also würde ein Benutzer versucht, ProtectedPage.aspx finden Sie auf, und sie wurden nicht dazu autorisiert, den FormsAuthenticationModule sie umleiten:

"Login.aspx"? ReturnUrl=ProtectedPage.aspx

Nach erfolgreicher Anmeldung wird sollten der Benutzer zurück zur ProtectedPage.aspx umgeleitet werden. Alternativ können Benutzer die Anmeldeseite für ihre eigenen Volition besuchen. In diesem Fall sollte nach der Anmeldung des Benutzers sie auf den Stammordner Default.aspx-Seite gesendet werden.

### <a name="logging-in-the-user"></a>Anmeldung des Benutzers

Vorausgesetzt, dass die angegebenen Anmeldeinformationen gültig sind, müssen wir eine Formularauthentifizierungsticket erstellen und den Benutzer auf der Website anmelden. Die [FormsAuthentication-Klasse](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) in die [Namespace System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) bietet verschiedene Methoden für die Protokollierung in und Benutzer über die Formulare Protokollierung-Authentifizierungssystem her. Es gibt mehrere Methoden in der FormsAuthentication-Klasse, sind die drei, die an diesem Punkt interessiert sind:

- [GetAuthCookie (*Benutzername*, *PersistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) -erstellt ein Forms-Authentifizierungsticket für die angegebene *Benutzername*. Als Nächstes wird diese Methode erstellt, und gibt ein HttpCookie-Objekt, das den Inhalt des Authentifizierungstickets enthält. Wenn *PersistCookie* "true" ist ein dauerhaftes Cookie wird erstellt.
- [SetAuthCookie (*Benutzername*, *PersistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -Ruft die GetAuthCookie (*Benutzername*, *PersistCookie*) Methode, um die formularauthentifizierungs-Cookie zu generieren. Diese Methode fügt dann das Cookie, das vom GetAuthCookie der cookieauflistung (vorausgesetzt, Cookies basierende Formularauthentifizierung verwendet werden; andernfalls wird, diese Methode ruft eine interne Klasse, die die Logik ohne Cookies Ticket behandelt).
- [RedirectFromLoginPage (*Benutzername*, *PersistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) – diese Methode ruft SetAuthCookie (*Benutzername*, *PersistCookie*), und klicken Sie dann leitet den Benutzer auf die entsprechende Seite.

GetAuthCookie ist praktisch, wenn Sie das Authentifizierungsticket zu ändern, bevor der cookieauflistung das Cookie schreiben müssen. SetAuthCookie ist nützlich, wenn Sie möchten die Formulare Authentifizierungsticket erstellen, und fügen es der cookieauflistung, aber nicht den Benutzer an die entsprechende Seite umleiten möchten. Vielleicht möchten Sie auf der Anmeldeseite aufzubewahren oder an eine alternative Seite senden.

Da wir den Benutzer anmelden und eine Umleitung an die entsprechende Seite möchten, verwenden wir RedirectFromLoginPage an. Aktualisieren Sie die LoginButtons auf Ereignishandler, und Ersetzen Sie in den zwei kommentierten TODO-Zeilen durch die folgende Codezeile:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

Authentifizierungsticket für die Erstellung der Forms verwenden wir die UserName TextBox Text-Eigenschaft für das Formularauthentifizierungsticket *Benutzername* Parameter und der Aktivierungszustand der RememberMe CheckBox für die *PersistCookie* Parameter.

Um die Anmeldeseite testen möchten, besuchen Sie es in einem Browser aus. Starten Sie durch Eingabe von ungültigen Anmeldeinformationen, z. B. dem Benutzernamen Nope und ein Kennwort falsch. Nach dem Klicken auf die Schaltfläche "Anmelden" erfolgt ein Postback aus, und die InvalidCredentialsMessage Bezeichnung angezeigt.


[![Die Bezeichnung InvalidCredentialsMessage ist ungültiger Anmeldeinformationen angezeigt, wenn eingeben](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Abbildung 10**: die InvalidCredentialsMessage-Bezeichnung ist, ungültige Anmeldeinformationen angezeigt, bei der Eingabe ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image30.png))


Als Nächstes geben Sie gültige Anmeldeinformationen ein, und klicken Sie auf die Schaltfläche "Anmelden". Dieses Mal, wenn das Postback einer Formularauthentifizierungsticket auftritt, wird erstellt, und Sie werden automatisch wieder an "default.aspx" umgeleitet. An diesem Punkt haben Sie auf der Website angemeldet allerdings stehen keine visuelle Hinweise, um anzugeben, dass Sie derzeit angemeldet sind. In Schritt 4, wie Sie programmgesteuert zu ermitteln, ob ein Benutzer sehen, wird in oder nicht sowie zur Identifizierung des Benutzers, der auf der Seite protokolliert.

Schritt 5 untersucht die Methoden für die Protokollierung von eines Benutzers der Website abgemeldet.

### <a name="securing-the-login-page"></a>Sichern die Anmeldeseite

Wenn der Benutzer ihre Anmeldeinformationen eingegeben und das Anmeldeformular für die Seite übermittelt, werden die – einschließlich Kennwort - Anmeldeinformationen über das Internet an den Webserver in übertragen *nur-Text*. Das bedeutet, dass alle Hacker man den Netzwerkverkehr des Benutzernamens und Kennworts angezeigt. Um dies zu verhindern, ist es erforderlich, den Netzwerkdatenverkehr mit verschlüsseln [Secure Socket Layer (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Dadurch wird sichergestellt, dass die Anmeldeinformationen (sowie die gesamte Seite HTML-Markup) ab dem Zeitpunkt verschlüsselt werden, die sie den Browser verlassen, bis sie vom Webserver empfangen werden.

Es sei denn, Ihre Website vertrauliche Informationen enthalten, müssen Sie nur verwenden Sie SSL auf der Anmeldeseite und auf anderen Seiten, in denen würde das Kennwort des Benutzers andernfalls über das Netzwerk als nur-Text gesendet werden. Sie müssen sich keine Gedanken um die Formulare Authentifizierungsticket sichern, da in der Standardeinstellung ist es sowohl verschlüsselt und digital signiert (um Manipulationen zu verhindern). Eine ausführlichere Erläuterung der Forms Authentication Ticket-Sicherheit wird im folgenden Tutorial angezeigt.

> [!NOTE]
> Viele Websites für Finanz- und medizinische sind so konfiguriert, dass die Verwendung von SSL auf *alle* Seiten zugegriffen werden kann, um authentifizierte Benutzer. Wenn Sie eine solche Website erstellen können Sie das Authentifizierungssystem Forms konfigurieren, damit das Formularauthentifizierungsticket nur über eine sichere Verbindung übertragen wird. Betrachten wir die verschiedenen Optionen zur Authentifizierung Formulare in den nächsten Lernprogrammen *[Konfiguration der Formularauthentifizierung und erweiterte Themen](../membership/creating-the-membership-schema-in-sql-server-vb.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Schritt 4: Erkennen von authentifizierten Besucher und ermitteln ihre Identität

An diesem Punkt wir die Formularauthentifizierung aktiviert haben, und eine einfache Anmeldeseite erstellt, aber wir haben dafür noch untersuchen, wie wir bestimmen können, ob ein Benutzer authentifiziert oder anonym ist. In bestimmten Szenarien möchten wir möglicherweise unterschiedliche Daten oder Informationen, je nachdem, ob ein Benutzer authentifizierter oder anonymer die Seite besucht angezeigt werden. Darüber hinaus müssen wir häufig die Identität des authentifizierten Benutzers kennen.

Wir erweitern die vorhandene Default.aspx-Seite, um diese Techniken zu veranschaulichen. Fügen Sie zwei Panel-Steuerelemente, die eine benannte AuthenticatedMessagePanel und eine andere benannte AnonymousMessagePanel in "default.aspx" hinzu. Fügen Sie ein Label-Steuerelement, das mit dem Namen WelcomeBackMessage im ersten Bereich hinzu. Klicken Sie im zweiten Bereich ein Linksteuerelement hinzufügen, legen Sie seine Texteigenschaft auf anmelden und die NavigateUrl-Eigenschaft auf ~ / "Login.aspx". An diesem Punkt sollte das deklarative Markup für "default.aspx" etwa wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Wie Sie mittlerweile wahrscheinlich erraten haben, wird die Idee dabei nur die AuthenticatedMessagePanel authentifizierten Besucher und nur die AnonymousMessagePanel für anonyme Besucher angezeigt. Zu diesem Zweck müssen wir diese Bereiche sichtbaren Eigenschaften je nachdem, ob der Benutzer oder nicht angemeldet ist festgelegt.

Die [Request.IsAuthenticated Eigenschaft](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) gibt einen booleschen Wert, der angibt, ob die Anforderung authentifiziert wurde. Geben Sie den folgenden Code in die Seite\_Ereignishandlercode laden:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Finden Sie mit diesem Code werden über einen Browser auf "default.aspx". Vorausgesetzt, dass Sie noch für die Anmeldung, wird ein Link zur Anmeldeseite angezeigt (siehe Abbildung 11). Klicken Sie auf diesen Link, und melden Sie sich an den Standort. Wie in Schritt 3 beschrieben, nach dem Eingeben Ihrer Anmeldeinformationen werden Sie an "default.aspx" zurückgegeben werden, aber dieses Mal die Seite zeigt die Willkommen zurück! Nachricht (siehe Abbildung 12).


[![Wenn Zugriff auf anonym, ein Protokoll In Link angezeigt wird](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Abbildung 11**: beim Zugriff auf anonym, ein Protokoll In Link angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image33.png))


[![Authentifizierte Benutzern wird gezeigt, dass die Willkommen zurück! Nachricht](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Abbildung 12**: authentifizierte Benutzern wird gezeigt, dass die Willkommen zurück! Nachricht ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image36.png))


Ermittelt, Identität des aktuell angemeldeten Benutzers, über die [HttpContext-Objekts](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)des [Benutzereigenschaft](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). Das HttpContext-Objekt stellt Informationen über die aktuelle Anforderung dar und ist für allgemeine ASP.NET-Objekte wie Antwort, der Anforderung und der Sitzungsebene, u. a. Die User-Eigenschaft stellt den Sicherheitskontext des die aktuelle HTTP-Anforderung und implementiert die [IPrincipal-Schnittstelle](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Die User-Eigenschaft wird von der FormsAuthenticationModule festgelegt. Insbesondere, wenn die FormsAuthenticationModule ein Formularauthentifizierungsticket in der eingehenden Anforderung findet, es erstellt ein neues GenericPrincipal-Objekt und die User-Eigenschaft zugewiesen.

Principal-Objekten (z. B. GenericPrincipal) enthalten Informationen zur Identität des Benutzers und die Rollen, die sie angehören. Die IPrincipal-Schnittstelle definiert zwei Elemente:

- ["IsInRole" (*RoleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) -eine Methode, die gibt einen booleschen Wert, der angibt, ob der Prinzipal, der angegebenen Rolle gehört.
- [Identität](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) -eine Eigenschaft, die ein Objekt zurückgibt, implementiert die [IIdentity-Schnittstelle](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). Die IIdentity-Schnittstelle definiert drei Eigenschaften: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), und [Namen](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Wir können den Namen des aktuellen Besuchers mithilfe des folgenden Codes ermitteln:

Dim-CurrentUsersName As String = User.Identity.Name

Beim Verwenden von Authentifizierung, forms ein [FormsIdentity Objekt](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) für GenericPrincipals-Identity-Eigenschaft erstellt wird. Die FormsIdentity-Klasse gibt immer die Zeichenfolge-Formularen für die Eigenschaft AuthenticationType und "true" zurück, für die IsAuthenticated-Eigenschaft. Die Name-Eigenschaft gibt den Benutzernamen angegeben, wenn die Forms-Authentifizierungsticket erstellen. Zusätzlich zu diesen drei Eigenschaften, FormsIdentity umfasst den Zugriff auf das zugrunde liegende Authentifizierungsticket über seine [Eigenschaft Ticket](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). Die Ticket-Eigenschaft gibt ein Objekt vom Typ [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), die über Eigenschaften, z. B. Ablauf, IsPersistent, IssueDate, Name und So weiter verfügt.

Der wichtigste Punkt vorwegzunehmen ist hier, dass die *Benutzername* in die FormsAuthentication.GetAuthCookie angegebene Parameter (*Benutzername*, *PersistCookie*), FormsAuthentication.SetAuthCookie (*Benutzername*, *PersistCookie*), und FormsAuthentication.RedirectFromLoginPage (*Benutzername*, *PersistCookie*) Methoden ist der gleiche Wert, der von User.Identity.Name zurückgegeben. Darüber hinaus ist das Authentifizierungsticket, die von diesen Methoden erstellt User.Identity einem FormsIdentity-Objekt umwandeln und anschließendes zugreifen auf die Eigenschaft Ticket verfügbar:

Dim-Ident als FormsIdentity = CType (User.Identity, FormsIdentity)

Dim-Authentifizierungsticket als FormsAuthenticationTicket Ident =. Ticket

Wir bieten eine weitere personalisierte Nachricht in "default.aspx". Aktualisieren Sie die Seite\_Ereignishandler zum Laden, damit der WelcomeBackMessage Bezeichnung Text-Eigenschaft zurück, für die Zeichenfolge Willkommen zugewiesen wird *Benutzername*!

WelcomeBackMessage.Text = "Willkommen zurück," &amp; User.Identity.Name &amp; "!"

Abbildung 13 zeigt die Auswirkung dieser Änderung (wenn es sich um eine Anmeldung als Benutzer Scott).


[![Die Willkommensnachricht enthält des derzeit angemeldeten, im Namen des Benutzers](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Abbildung 13**: die Willkommensnachricht enthält, die derzeit protokolliert im Namen des Benutzers ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>Verwenden von dem LoginView und LoginName-Steuerelement

Anzeigen von unterschiedlichen Inhalt auf authentifizierte und anonyme Benutzer besteht eine häufige Anforderung; Daher wird der Name des aktuell angemeldeten Benutzers angezeigt. Aus diesem Grund bietet ASP.NET zwei Web-Steuerelemente, die dargestellt in Abbildung 13, jedoch ohne eine einzige Codezeile schreiben zu müssen dieselbe Funktionalität bereitzustellen.

Die [LoginView-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) ein Template-basierten Websteuerelement, das es einfach macht, die unterschiedliche Daten authentifiziert und anonym Benutzern angezeigt wird. Das LoginView enthält, zwei vordefinierte Vorlagen:

- AnonymousTemplate - Markup hinzugefügt, um diese Vorlage wird nur für anonyme Besucher angezeigt.
- LoggedInTemplate - Markup mit dieser Vorlage wird nur für authentifizierte Benutzer angezeigt werden.

Fügen Sie das LoginView-Steuerelement auf unserer Website Masterseite, Site.master an. Statt nur das LoginView-Steuerelement hinzufügen allerdings fügen Sie sowohl ein neues ContentPlaceHolder-Steuerelement, und dann das LoginView-Steuerelement in diese neue ContentPlaceHolder eingefügt. Der Grund für diese Entscheidung wird gleich deutlich werden.

> [!NOTE]
> Neben den AnonymousTemplate und LoggedInTemplate kann das LoginView-Steuerelement rollenspezifische Vorlagen enthalten. Rollenspezifische Vorlagenanzeige Markup nur für diejenigen Benutzer, die zu einer bestimmten Rolle gehören. In einem späteren Tutorial untersuchen wir die Funktionen rollenbasierte, von dem LoginView-Steuerelement.


Starten, indem ein ContentPlaceHolder-Objekt mit dem Namen "LoginContent", in die Masterseite innerhalb der Navigation hinzufügen &lt;Div&gt; Element. Sie können einfach ein ContentPlaceHolder-Steuerelement ziehen, aus der Toolbox auf die Datenquellensicht, platzieren das daraus resultierende Markup direkt über das TODO: Menü wird finden Sie hier Text.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Als Nächstes fügen Sie ein LoginView-Steuerelement innerhalb der LoginContent ContentPlaceHolder hinzu. Inhalt in die Masterseite ContentPlaceHolder-Steuerelemente platziert gelten *Inhalt standardmäßig* für die ContentPlaceHolder. ASP.NET-Seiten, die diese Masterseite verwenden können, also Geben Sie ihre eigenen Inhalte für jede ContentPlaceHolder oder Standardinhalt der Masterseite verwenden.

Das LoginView und andere anmeldebezogene Steuerelemente befinden sich in der Anmeldung der Toolboxregisterkarte.


[![Das LoginView-Steuerelement in der Toolbox](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Abbildung 14**: das LoginView-Steuerelement in der Toolbox ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image42.png))


Als Nächstes fügen Sie zwei &lt;Br /&gt; Elemente unmittelbar nach dem LoginView-Steuerelement, aber immer noch innerhalb der ContentPlaceHolder. An diesem Punkt die Navigation &lt;Div&gt; der Elementknoten dem Elementmarkup sollte wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Das LoginView Vorlagen können über den Designer oder deklarativen Markup definiert werden. Erweitern Sie Visual Studio-Designer die LoginViews Smarttags, der der konfigurierten Vorlagen in einer Dropdown-Liste aufgeführt sind. Geben Sie in den Text Hello, fremden in AnonymousTemplate; als Nächstes ein Linksteuerelement hinzufügen, und legen Sie die Text- und NavigateUrl-Eigenschaften zum Anmelden und ~ / "Login.aspx", bzw.

Klicken Sie nach dem Konfigurieren der AnonymousTemplate, wechseln Sie zu der LoggedInTemplate, und geben Sie den Text, der "Willkommen zurück,". Klicken Sie dann ziehen Sie ein LoginName-Steuerelement aus der Toolbox in die LoggedInTemplate, legen Sie das Element unmittelbar nach der "Willkommen zurück," Text ein. Die [LoginName-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), wie der Name andeutet, zeigt den Namen des aktuell angemeldeten Benutzers. Intern gibt das LoginName-Steuerelement einfach die User.Identity.Name-Eigenschaft

Nach dem Herstellen dieser Ergänzungen zu den LoginView Vorlagen, sollte das Markup etwa wie folgt aussehen:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Mit folgender Ergänzung der Masterseite Site.master zeigt jede Seite in unsere Website, eine andere Meldung abhängig davon, ob der Benutzer authentifiziert wird. Abbildung 15 zeigt die Seite "default.aspx", wenn Sie über einen Browser vom Benutzer Jisun besucht. Die Willkommen zurück, Jisun Meldung zweimal wiederholt: einmal in die Masterseite-Navigationsbereich auf der linken Seite (über das LoginView-Steuerelement, das wir gerade hinzugefügt haben) und einmal in der "default.aspx" Inhaltsbereich (über Panel-Steuerelemente und programmgesteuerte Logik).


[![Das LoginView-Steuerelement zeigt Willkommen zurück, Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Abbildung 15**: das LoginView-Steuerelement zeigt Willkommen zurück, Jisun. ([Klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image45.png))


Da wir die Masterseite der LoginView hinzugefügt haben, können sie in jeder Seite auf unserer Website angezeigt. Allerdings gibt es möglicherweise Webseiten, in denen wir nicht diese Meldung angezeigt möchten. Eine solche Seite ist die Anmeldeseite, da Sie ein Link zur Anmeldeseite nicht zur es scheint. Da wir das LoginView-Steuerelement in ein ContentPlaceHolder-Objekt auf der Masterseite eingefügt haben, können wir diese Standard-Markup in unserer Seite mit überschreiben. Öffnen Sie "Login.aspx", und wechseln Sie in den Designer. Da wir nicht explizit ein ContentControl-Element definiert haben in "Login.aspx" für die LoginContent ContentPlaceHolder auf der Masterseite, wird die Anmeldeseite der Masterseite Standardmarkup für diese ContentPlaceHolder angezeigt. Sie können dies durch den Designer - finden Sie unter der LoginContent ContentPlaceHolder zeigt das Standardmarkup (das LoginView-Steuerelement).


[![Die Anmeldeseite zeigt die Standardeinstellung für die Masterseite LoginContent ContentPlaceHolder Inhalt](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Abbildung 16**: die Anmeldeseite der Standard-Editor zeigt, für the Master Page's LoginContent ContentPlaceHolder ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image48.png))


Um das Standardmarkup für das LoginContent ContentPlaceHolder zu überschreiben, mit der rechten Maustaste auf die Region, in den Designer und wählen Sie aus dem Kontextmenü die Option erstellen Sie benutzerdefinierte Inhalt. (Bei Verwendung von Visual Studio 2008 ContentPlaceHolder enthalten ein Smarttag, das bei Auswahl dieser Option bietet die Möglichkeit, demselben.) Dadurch werden ein neues ContentControl-Element, das Markup der Seite und somit auch zum Definieren von benutzerdefinierten Inhalts für diese Seite ermöglicht. Sie können hier eine benutzerdefinierte Meldung hinzufügen, z. B. Melden Sie sich, aber wir einfach dieses Feld leer lassen.

> [!NOTE]
> In Visual Studio 2005 Erstellen von benutzerdefinierten Inhalten erstellt ein leeres Content-Steuerelement auf der ASP.NET-Seite. In Visual Studio 2008 kopiert jedoch Erstellen von benutzerdefinierten Inhalt der Masterseite Standardinhalt in das neu erstellte Steuerelement. Wenn Sie Visual Studio 2008 verwenden, klicken Sie dann nach dem Erstellen des neuen Inhaltssteuerelements stellen Sie sicher, deaktivieren Sie den Inhalt der Masterseite kopiert.


Abbildung 17 zeigt die Seite "Login.aspx", wenn in einem Browser aufgerufen, nachdem diese Änderung vorgenommen. Beachten Sie, dass kein Hello, unbekannte oder Willkommen zurück, *Benutzername* Nachricht im linken Navigationsbereich &lt;Div&gt; wie beim Zugriff auf "default.aspx".


[![Die Anmeldeseite wird die Standard-LoginContent ContentPlaceHolder Markup ausgeblendet.](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Abbildung 17**: die Anmeldeseite wird ausgeblendet, die standardmäßige LoginContent ContentPlaceHolder Markup ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>Schritt 5: Abmelden

In Schritt 3 erläutert, erstellen eine Anmeldeseite, um einen Benutzer in der Website anmelden, aber wir haben dafür noch zu erfahren, wie Sie einen Benutzer abzumelden. Neben Methoden für die Protokollierung von eines Benutzers in die FormsAuthentication-Klasse auch bietet eine [SignOut-Methode](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Die SignOut-Methode löscht einfach das Formularauthentifizierungsticket, wodurch Anmelden des Benutzers von der Website.

Angebot, dass eine Link zur Abmeldung solche ein Feature ist enthält, die von ASP.NET ein Steuerelement, die speziell für einen Benutzer abzumelden. Die [LoginStatus-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) wird entweder ein LinkButton-Anmeldung oder Logout LinkButton, je nach Authentifizierungsstatus des Benutzers angezeigt. Ein LinkButton-Anmeldung ist für anonyme Benutzer dargestellt, Logout LinkButton an authentifizierte Benutzer angezeigt wird. Der Text für die Anmeldung und Logout LinkButtons kann konfiguriert werden, über den LoginStatus LoginText und LogoutText Eigenschaften.

Klicken auf LinkButton Anmeldung bewirkt, dass ein Postback handeln, von dem eine Umleitung zur Anmeldeseite ausgegeben wird. Klicken auf Logout LinkButton bewirkt, dass das LoginStatus-Steuerelement zum Aufrufen der Methode FormsAuthentication.SignOff, und klicken Sie dann die Benutzerinformationen auf einer Seite. Die Seite, die der angemeldete Benutzer wird an die LogoutAction-Eigenschaft, die auf einen der drei folgenden Werte zugewiesen werden können, hängt umgeleitet:

- Aktualisierung: die Standardeinstellung; leitet den Benutzer auf die Seite, die sie gerade besucht wurden. Wenn die Seite, die sie nur Zugriff auf wurden keine anonyme Benutzern zulässt, wird die FormsAuthenticationModule automatisch den Benutzer zur Anmeldeseite umleiten.

Möglicherweise möchten Sie gerne wissen möchten, warum eine Umleitung hier ausgeführt wird. Wenn der Benutzer möchte auf derselben Seite bleiben, warum die Notwendigkeit für die explizite Umleitung? Der Grund ist, da Wenn Logoff LinkButton geklickt wird, der Benutzer noch das Formularauthentifizierungsticket in die Auflistung der Cookies enthält. Daher ist die postbackanforderung an eine authentifizierte Anforderung an. Das LoginStatus-Steuerelement ruft die SignOut-Methode, aber dies geschieht, nachdem die FormsAuthenticationModule der Benutzer authentifiziert hat. Aus diesem Grund führt dazu, dass explizite Umleitung den Browser, um die Seite erneut anfordern. Der Zeitpunkt der Browser die Seite erneut anfordert, das Formularauthentifizierungsticket wurde entfernt, und die eingehende Anforderung ist deshalb anonym.

- Umleitungs - der Benutzer wird durch das LoginStatus LogoutPageUrl-Eigenschaft angegebenen URL umgeleitet.
- RedirectToLoginPage – der Benutzer wird an die Anmeldeseite umgeleitet.

Lassen Sie uns die Masterseite ein LoginStatus-Steuerelement hinzufügen und konfigurieren, dass der Umleitungs-Option verwenden, um den Benutzer zu einer Seite zu senden, die in einer Meldung bestätigt, dass sie wurden abgemeldet haben. Zunächst erstellen eine Seite in das Stammverzeichnis, das mit dem Namen Logout.aspx. Vergessen Sie nicht die Stammwebsite auf dieser Seite zugeordnet werden soll. Geben Sie anschließend eine Nachricht im Markup der Seite, die erläutern, die dem Benutzer, den sie sich angemeldet haben.

Als Nächstes auf die Masterseite Site.master zurückgeben Sie aus, und fügen ein LoginStatus-Steuerelement unterhalb der LoginView LoginContent ContentPlaceHolder hinzu. Legen Sie das LoginStatus-Steuerelement LogoutAction-Eigenschaft in Umleitung und die LogoutPageUrl-Eigenschaft auf ~/Logout.aspx an.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Da das LoginStatus außerhalb der LoginView-Steuerelement ist, für sowohl anonyme als auch authentifizierte Benutzer erscheint, aber das ist in Ordnung, da das LoginStatus einer Anmeldung oder Logout LinkButton ordnungsgemäß angezeigt wird. Durch das LoginStatus-Steuerelement hinzufügen die Log-In-Link in der AnonymousTemplate überflüssig ist so entfernen Sie sie.

Abbildung 18 zeigt "default.aspx" auf, wenn Jisun besucht. Beachten Sie, dass die linke Spalte die Meldung angezeigt, Willkommen Sie zurück, Jisun zusammen mit einem Link zum Abmelden. Klicken Sie auf die Abmeldung LinkButton ein Postback auslöst, Jisun aus dem System registriert und leitet sie dann auf Logout.aspx. Wie in Abbildung 19, mit der Zeit dargestellt, die Jisun Logout.aspx erreicht, er hat bereits abgemeldet und ist deshalb anonym. Daher zeigt die linke Spalte den Text Begrüßung Stranger, und es wird ein Link zur Anmeldeseite.


[!["Default.aspx" zeigt Willkommen zurück, Jisun zusammen mit LinkButton Abmelden](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Abbildung 18**: "default.aspx" zeigt Willkommen zurück, Jisun zusammen mit Logout LinkButton ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx zeigt Willkommen, unbekannte zusammen mit einem LinkButton-Anmeldung](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Abbildung 19**: Logout.aspx zeigt Willkommen, unbekannte zusammen mit einem LinkButton-Anmeldung ([klicken Sie, um das Bild in voller Größe anzeigen](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> Sollten Sie die Logout.aspx-Seite, um der Masterseite LoginContent ContentPlaceHolder ausblenden (wie für "Login.aspx" in Schritt 4) anpassen. Der Grund dafür ist, da die Anmeldung LinkButton von LoginStatus-Steuerelement gerendert (diejenige unter Hello, unbekannte) verweist den Benutzer auf die Anmeldeseite, übergeben die aktuelle URL in den Querystring-Parameter von "ReturnUrl" nachzuweisen. Kurz gesagt, wenn ein Benutzer, ist angemeldet hat diese LoginStatus Anmeldung LinkButton klickt, und klicken Sie dann anmeldet, werden sie wieder an Logout.aspx, umgeleitet werden die einfach den Benutzer verwirrend sein kann.


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben wir Schritte mit der Untersuchung von Workflow der Forms-Authentifizierung und klicken Sie dann zum Implementieren der Formularauthentifizierung in einer ASP.NET-Anwendung aktiviert. Die Formularauthentifizierung basiert auf der FormsAuthenticationModule, der zwei Aufgaben ausführt: Identifizieren von Benutzern, die basierend auf ihren Formularauthentifizierungsticket und nicht autorisierte Benutzer auf die Anmeldeseite umgeleitet.

.NET Framework-FormsAuthentication-Klasse enthält Methoden zum Erstellen, überprüfen und Entfernen von Formularauthentifizierungstickets. Die Request.IsAuthenticated-Eigenschaft und die User-Objekts unterstützen zusätzliche programmgesteuerte bestimmen, ob eine Anforderung authentifiziert wird und Informationen über die Identität des Benutzers. Es gibt auch die LoginView und LoginStatus, LoginName-Webserver-Steuerelemente, die Entwickler eine Möglichkeit für schnelle, ohne Code zu erhalten, für viele allgemeine Aufgaben anmeldebezogene. Wir werden in zukünftigen Lernprogrammen diese und andere anmeldebezogene Websteuerelemente genauer untersuchen.

In diesem Tutorial bereitgestellt, eine kurze Übersicht über die Formularauthentifizierung. Wir nicht verschiedene Konfigurationsoptionen untersuchen, sehen Sie sich wie ohne Cookies Forms Authentication Tickets arbeiten oder untersuchen, wie ASP.NET den Inhalt des Formularauthentifizierungstickets schützt. Besprechen wir diese Themen und vieles mehr in der [nächsten Tutorial](forms-authentication-configuration-and-advanced-topics-vb.md).

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Änderungen zwischen IIS 6 und IIS 7-Sicherheit](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Anmeldung von ASP.NET-Steuerelementen](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 Security, Mitgliedschafts- und Rollenverwaltung](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Die &lt;Authentifizierung&gt; Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Die &lt;Forms&gt; -Element für &lt;Authentifizierung&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>In den Themen in diesem Tutorial-Videotraining

- [Verwenden der grundlegenden Formularauthentifizierung in ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial sind Alicja Maziarz, John Suru und Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Zurück](security-basics-and-asp-net-support-vb.md)
> [Weiter](forms-authentication-configuration-and-advanced-topics-vb.md)
