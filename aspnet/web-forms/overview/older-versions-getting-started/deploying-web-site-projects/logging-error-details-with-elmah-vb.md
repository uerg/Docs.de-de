---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Protokollieren von Fehlerdetails mit ELMAH (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Fehler beim Protokollieren Module und Handler (ELMAH) bietet ein weiteres Verfahren zum Protokollieren von Laufzeitfehlern in einer produktionsumgebung. ELMAH ist ein kostenfreies open Source-Fehler...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: ed59c6099925a2046d201e0eab0a9afdd620de28
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389201"
---
<a name="logging-error-details-with-elmah-vb"></a>Protokollieren von Fehlerdetails mit ELMAH (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Fehler beim Protokollieren Module und Handler (ELMAH) bietet ein weiteres Verfahren zum Protokollieren von Laufzeitfehlern in einer produktionsumgebung. ELMAH ist eine kostenlose open Source Fehler protokollierungsbibliothek, die Features wie fehlerfilterung und die Möglichkeit, die an das Fehlerprotokoll von einer Webseite als RSS-feed oder als eine durch Trennzeichen getrennte Datei zum Herunterladen enthält. In diesem Tutorial führt Sie durch das Herunterladen und Konfigurieren von ELMAH.


## <a name="introduction"></a>Einführung

Die [vorherigen Lernprogramm](logging-error-details-with-asp-net-health-monitoring-vb.md) ASP überprüft. NET Systemüberwachung System, das nicht der Box-Bibliothek für die Aufzeichnung von einer Vielzahl von Webereignissen bietet. Viele Entwickler verwenden, um sich anzumelden und die Details der nicht behandelten Ausnahmen e-Mail für die Systemüberwachung. Es gibt jedoch einige Problempunkte, die mit diesem System. Zuallererst ist das fehlen jede Art von Benutzeroberfläche zum Anzeigen von Informationen zu den protokollierten Ereignissen. Wenn Sie möchten eine Zusammenfassung der letzten 10 Fehler, oder zeigen Sie die Details eines Fehlers, die letzte Woche aufgetreten sind, müssen Sie entweder schaffen, in der Datenbank, suchen Sie in Ihrem e-Mail-Posteingang oder erstellen Sie eine Webseite, die Informationen aus anzeigt der `aspnet_WebEvent_Events` Tabelle.

Eine andere Regelungen basiert auf Überwachung der Integrität der Komplexität. Da Überwachung der Integrität verwendet werden kann eine Vielzahl von verschiedenen Ereignissen aufgezeichnet, und es gibt eine Vielzahl von Optionen angewiesen wird, wie und wann Ereignisse protokolliert werden, kann die ordnungsgemäße Konfiguration für die Systemüberwachung System eine lästige Aufgabe sein. Schließlich wurden Probleme mit der Anwendungskompatibilität. Da Systemüberwachung zuerst .NET Framework Version 2.0 hinzugefügt wurde, ist es nicht verfügbar für ältere Webanwendungen mit ASP.NET-Version 1.x. Darüber hinaus die `SqlWebEventProvider` -Klasse, die wir im vorherigen Tutorial Fehlerdetails für Protokolle mit einer Datenbank verwendet, funktioniert nur mit Microsoft SQL Server-Datenbanken. Sie müssen eine benutzerdefiniertes Protokoll-Provider-Klasse erstellen, müssen Sie sollten das Protokollieren von Fehlern in einen alternativen Datenspeicher, z. B. eine XML-Datei oder einer Oracle-Datenbank.

Eine Alternative zum System für die Systemüberwachung ist Fehler protokollieren Module und Handler (ELMAH), eine kostenlose, quelloffene fehlerprotokollierung-System erstellt [Atif Aziz](http://www.raboof.com/). Der wichtigste Unterschied zwischen den beiden Systemen ist ELAMHs Möglichkeit, um eine Liste von Fehlern und die Details eines bestimmten Fehlers von einer Webseite und als RSS-feed anzuzeigen. ELMAH ist einfacher zu konfigurieren, die als Überwachung der Integrität, da es nur Fehler protokolliert. ELMAH enthält darüber hinaus Unterstützung für ASP.NET 1.x, ASP.NET 2.0 und ASP.NET 3.5-Anwendungen und umfasst eine Vielzahl von Protokollanbietern für die Quelle.

In diesem Tutorial erläutert die Schritte zum Hinzufügen von ELMAH an eine ASP.NET-Anwendung ab. Fangen wir an!

> [!NOTE]
> Die Systemüberwachung für den System- und ELMAH haben beide ihre eigenen Gruppen von vor- und Nachteile. Sollten Sie versuchen beide Systeme, und entscheiden, ein am besten erfüllt Ihre Anforderungen.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Hinzufügen von ELMAH an eine ASP.NET-Webanwendung

Integrieren von ELMAH in einer neuen oder vorhandenen ASP.NET-Anwendung ist ein einfach und unkompliziert-Prozess, der weniger als fünf Minuten dauert. Kurz gesagt, umfasst diese vier einfache Schritten:

1. Herunterladen von ELMAH und Hinzufügen der `Elmah.dll` Assembly zu Ihrer Webanwendung
2. Registrieren von HTTP-Module und Handler in der ELMAH `Web.config`,
3. Geben Sie ELMAHs-Konfigurationsoptionen, und
4. Erstellen Sie der Fehler-Log-Source-Infrastruktur, bei Bedarf.

Betrachten wir nun jede der folgenden vier Schritte aus, einzeln nacheinander.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Schritt 1: Herunterladen der Dateien des ELMAH-Projekt und Hinzufügen von`Elmah.dll`zu Ihrer Webanwendung

ELMAH-1.0 BETA 3 (10617 erstellen), die neueste Version zum Zeitpunkt der geschrieben wird, ist im Download zur Verfügung, in diesem Tutorial enthalten. Alternativ finden Sie auf die [ELMAH Website](https://code.google.com/p/elmah/) auf die neueste Version zu erhalten oder um den Quellcode herunterladen. Extrahieren Sie den ELMAH-Download in einen Ordner auf Ihrem Desktop, und suchen Sie die Assemblydatei ELMAH (`Elmah.dll`).

> [!NOTE]
> Die `Elmah.dll` Datei befindet sich im Download des `Bin` Ordner, in dem Unterordner für die verschiedenen Versionen von .NET Framework und Release- und Debugkonfigurationen Builds besitzt. Verwenden Sie die endgültige Produktversion, für die entsprechende Framework-Version. Kopieren Sie z. B. Wenn Sie eine ASP.NET 3.5-Webanwendung erstellen, die `Elmah.dll` -Datei aus der `Bin\net-3.5\Release` Ordner.


Als Nächstes öffnen Sie Visual Studio, und fügen Sie die Assembly dem Projekt, mit der rechten Maustaste auf den Namen der Website im Projektmappen-Explorer und auswählen Verweis aus dem Kontextmenü hinzufügen. Dadurch wird das Dialogfeld "Verweis hinzufügen". Navigieren Sie zur Registerkarte "Durchsuchen", und wählen Sie die `Elmah.dll` Datei. Diese Aktion fügt der `Elmah.dll` Datei an der Webanwendung `Bin` Ordner.

> [!NOTE]
> Der Typ der Web Application Project (WAP) wird nicht angezeigt. die `Bin` Ordner im Projektmappen-Explorer. Stattdessen werden diese Elemente unter dem Ordner "Verweise" aufgeführt.


Die `Elmah.dll` Assembly enthält die Klassen, die durch das ELMAH-System verwendet. Diese Klassen werden in drei Kategorien unterteilt:

- **HTTP-Module** -HTTP-Modul ist eine Klasse, die Ereignishandler für definiert `HttpApplication` Ereignisse, z. B. die `Error` Ereignis. ELMAH umfasst mehrere HTTP-Module, die drei am häufigsten von Belang diejenigen wird: 

    - `ErrorLogModule` -nicht behandelte Ausnahmen in einem Protokollquelle protokolliert.
    - `ErrorMailModule` -die Details einer nicht behandelten Ausnahme in einer e-Mail-Nachricht sendet.
    - `ErrorFilterModule` – bezieht sich Entwickler angegebene Filter, um zu bestimmen, welche Ausnahmen protokolliert werden und was diejenigen werden ignoriert.
- **HTTP-Handler** -HTTP-Handler ist eine Klasse, die zum Generieren von Markup für eine bestimmte Art von Anforderung zuständig ist. ELMAH enthält HTTP-Handler, die Fehlerdetails als Webseite verweist, als RSS-feed oder als eine durch Trennzeichen getrennte Datei (CSV) zu rendern.
- **Fehler-Protokollquellen** – standardmäßig ELMAH Fehler in den Speicher, in einer Microsoft SQL Server-Datenbank mit einer Microsoft Access-Datenbank mit einer Oracle-Datenbank, um anmelden kann eine XML-Datei, die in einer SQLite-Datenbank oder einer Vista-DB-Datenbank. Wie das Überwachungssystem wurde ELMAH Architektur mit das Providermodell, was bedeutet, dass Sie erstellen und integrieren eigene benutzerdefinierte Protokollanbieter von Quelle, bei Bedarf erstellt.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Schritt 2: Registrieren des ELMAH HTTP-Modul und Handler

Während der `Elmah.dll` -Datei enthält die HTTP-Module und Handler erforderlich sind, automatisch nicht behandelte Ausnahmen zu protokollieren und Anzeigen von Fehlerdetails, die von einer Webseite, diese müssen explizit in der Konfiguration von der Webanwendung registriert werden. Die `ErrorLogModule` HTTP-Modul nach der Registrierung abonniert die `HttpApplication`des `Error` Ereignis. Dieses Ereignis wird ausgelöst, wenn die `ErrorLogModule` protokollieren Sie die Details der Ausnahme zu einer angegebenen Quelle. Erfahren Sie, wie zum Definieren des Protokollanbieters für die Datenquelle im nächsten Abschnitt "Konfigurieren von ELMAH". Die `ErrorLogPageFactory` HTTP-Handler-Factory ist verantwortlich für das Markup generiert, wenn Sie das Fehlerprotokoll auf einer Webseite anzeigen.

Die spezielle Syntax zum Registrieren von HTTP-Module und Handler richtet sich nach der Webserver, der die Website verbessern der Leistung ist. Bei der ASP.NET Development Server und Microsoft IIS 6.0 und früheren Versionen, HTTP-Module und Handler im registriert werden die `<httpModules>` und `<httpHandlers>` Abschnitte, die in angezeigt werden. die `<system.web>` Element. Wenn Sie IIS 7.0 verwenden, und klicken Sie dann sie die Eintragung in müssen die `<system.webServer>` des Elements `<modules>` und `<handlers>` Abschnitte. Glücklicherweise können Sie definieren, die HTTP-Module und Handler in *sowohl* platziert werden, unabhängig von der Webserver verwendet wird. Diese Option ist das Ergebnis am besten portierbar die gleiche Konfiguration in den Umgebungen für Entwicklungs- und produktionsumgebungen unabhängig von der Webserver verwendet wird, verwendet werden können.

Registrieren Sie zunächst die `ErrorLogModule` HTTP-Modul und die `ErrorLogPageFactory` HTTP-Handler aus der `<httpModules>` und `<httpHandlers>` im Abschnitt `<system.web>`. Wenn Ihre Konfiguration bereits diese beiden Elemente einfach dann definiert enthalten die `<add>` für HTTP-Modul und der Handler des ELMAH-Element.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

Registrieren Sie als Nächstes ELMAHs HTTP-Modul und -Handler in der `<system.webServer>` Element. Wie zuvor, wenn dieses Element nicht bereits in Ihrer Konfiguration vorhanden ist fügen Sie es.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

Standardmäßig IIS 7 meldet, wenn HTTP-Module und Handler, in registriert sind der `<system.web>` Abschnitt. Die `validateIntegratedModeConfiguration` -Attribut in der `<validation>` Element weist IIS 7 diese Fehlermeldungen unterdrückt werden sollen.

Beachten Sie, dass die Syntax für die Registrierung der `ErrorLogPageFactory` HTTP-Handler enthält einen `path` -Attribut, die auf `elmah.axd`. Dieses Attribut informiert der Webanwendung, auch wenn eine Anforderung eingeht, für eine Seite namens `elmah.axd` und klicken Sie dann die Anforderung sollen, indem verarbeitet werden die `ErrorLogPageFactory` HTTP-Handler. Wir sehen uns die `ErrorLogPageFactory` HTTP-Handler in der Aktion weiter unten in diesem Tutorial.

### <a name="step-3-configuring-elmah"></a>Schritt 3: Konfigurieren von ELMAH

ELMAH sucht in der Website auf die Konfigurationsoptionen `Web.config` Datei in einem benutzerdefinierten Konfigurationsabschnitt, der mit dem Namen `<elmah>`. Zum Verwenden eines benutzerdefinierten Abschnitts im `Web.config` muss zuerst definiert werden die `<configSections>` Element. Öffnen der `Web.config` Datei, und fügen Sie das folgende Markup auf der `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Wenn Sie ELMAH für eine ASP.NET 1.x-Anwendung konfigurieren und entfernen Sie dann die `requirePermission="false"` -Attribut aus dem `<section>` obigen Elemente.


Die oben aufgeführten Syntax registriert die benutzerdefinierte `<elmah>` Abschnitt und die zugehörigen Unterabschnitte: `<security>`, `<errorLog>`, `<errorMail>`, und `<errorFilter>`.

Fügen Sie als Nächstes die `<elmah>` Abschnitt `Web.config`. Dieser Abschnitt sollte angezeigt werden, auf der gleichen Ebene wie die `<system.web>` Element. In der `<elmah>` Abschnitt hinzufügen, die `<security>` und `<errorLog>` Abschnitte wie folgt:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

Die `<security>` des Abschnitts `allowRemoteAccess` Attribut gibt an, ob der Remotezugriff zulässig ist. Wenn dieser Wert auf 0 festgelegt ist, können dann die Fehler-Log-Webseite nur lokal angezeigt werden. Wenn dieses Attribut auf 1 festgelegt ist, ist der Fehler Log-Webseite für lokalen und remote-Besucher aktiviert. Deaktivieren Sie vorerst wir die Fehler-Log-Webseite remote Besucher. Remotezugriff ermöglichen später, nachdem wir die Möglichkeit, erläutern die Sicherheitsaspekte, der auf diese Weise möchten.

Die `<errorLog>` Abschnitt definiert die Protokollquelle Fehler, der gibt an, in denen die Fehlerdetails werden aufgezeichnet, ähnelt, die `<providers>` Abschnitt in das System für die Systemüberwachung. Gibt an, die oben aufgeführten Syntax der `SqlErrorLog` Klasse als Quelle Protokoll Fehler, die protokolliert wird der Fehler mit einer Microsoft SQL Server-Datenbank, die gemäß der `connectionStringName` Attributwert.

> [!NOTE]
> ELMAH im Lieferumfang von zusätzlichen Fehlerinformationen von Protokollanbietern, die zum Protokollieren von Fehlern, die eine XML-Datei, eine Microsoft Access-Datenbank, eine Oracle-Datenbank und anderen Datenspeichern verwendet werden können. Im Beispiel `Web.config` -Datei, die mit dem ELMAH-Download für Informationen zur Verwendung dieser Alternativen Fehler Protokollanbieter enthalten ist.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Schritt 4: Erstellen der Fehler Log-Source-Infrastruktur

Die ELMAH `SqlErrorLog` -Anbieter protokolliert Fehlerdetails in eine angegebene Microsoft SQL Server-Datenbank. Die `SqlErrorLog` Anbieter erwartet, dass diese Datenbank in eine Tabelle mit dem Namen `ELMAH_Error` und drei gespeicherte Prozeduren: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, und `ELMAH_LogError`. Der ELMAH-Download umfasst eine Datei namens `SQLServer.sql` in die `db` Ordner mit der T-SQL zum Erstellen dieser Tabelle und die folgenden gespeicherten Prozeduren. Sie müssen diese Anweisungen führen Sie für Ihre Datenbank mit der `SqlErrorLog` Anbieter.

**Abbildungen 1** und **2** im Datenbank-Explorer in Visual Studio nach der erforderlichen Datenbankobjekte Anzeigen der `SqlErrorLog` Anbieter hinzugefügt wurden.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Abbildung 1**: die `SqlErrorLog` -Anbieter protokolliert Fehler, um die `ELMAH_Error` Tabelle

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Abbildung 2**: die `SqlErrorLog` -Anbieter verwendet drei gespeicherte Prozeduren

## <a name="elmah-in-action"></a>ELMAH In Aktion

Wir haben an diesem Punkt ELMAH hinzugefügt, auf die Webanwendung, die registriert der `ErrorLogModule` HTTP-Modul und die `ErrorLogPageFactory` HTTP-Handler angegeben ELMAHs-Konfigurationsoptionen in `Web.config`, und die erforderlichen Datenbankobjekte für die `SqlErrorLog` Fehler-Protokollanbieter. Wir können nun ELMAH in Aktion zu sehen! Besuchen Sie die Website Book Reviews und finden Sie auf eine Seite, die einen Laufzeitfehler, wie z. B. generiert `Genre.aspx?ID=foo`, oder eine nicht existierende-Seite, wie z. B. `NoSuchPage.aspx`. Was Sie sehen, wenn diese Seiten besuchen hängt von der `<customErrors>` Konfigurations- und gibt an, ob Sie lokal oder Remote besuchen. (Siehe die [ *Anzeigen einer benutzerdefinierten Fehlerseite* Tutorial](displaying-a-custom-error-page-vb.md) auffrischen möchten zu diesem Thema.)

ELMAH hat keine Auswirkungen auf Inhalte, die dem Benutzer angezeigt wird, wenn eine nicht behandelte Ausnahme auftritt; nur protokolliert die Details. Dieses Fehlerprotokoll kann zugegriffen werden, von der Webseite `elmah.axd` aus dem Stammverzeichnis der Website, z. B. `http://localhost/BookReviews/elmah.axd`. (Diese Datei ist nicht physisch vorhanden, in Ihrem Projekt, aber wenn eine Anforderung für eingeht `elmah.axd` die Laufzeit sendet er die `ErrorLogPageFactory` HTTP-Handler, die das Markup zurück an den Browser gesendeten generiert.)

> [!NOTE]
> Sie können auch die `elmah.axd` Seite anweisen ELMAH, um einen Testfehler zu generieren. Zugriff auf `elmah.axd/test` (als `http://localhost/BookReviews/elmah.axd/test`) bewirkt, dass ELMAH auslösen eine Ausnahme vom Typ `Elmah.TestException`, die die Fehlermeldung enthält: "Dies ist ein testausnahme, die ignoriert werden kann."


**Abbildung 3** im Fehlerprotokoll zeigt, wenn das Unternehmen besuchen `elmah.axd` aus der Entwicklungsumgebung.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Abbildung 3**: `Elmah.axd` zeigt das Fehlerprotokoll auf einer Webseite  
([Klicken Sie, um das Bild in voller Größe anzeigen](logging-error-details-with-elmah-vb/_static/image7.png))

Das Fehlerprotokoll **Abbildung 3** enthält sechs Fehlereinträge. Jeder Eintrag enthält den HTTP-Statuscode: (404 oder 500 für diese Fehler), der Typ, der Beschreibung, den Namen des angemeldeten Benutzers bei der der Fehler aufgetreten ist, und die Datums- /. Klicken Sie auf den Link Details Zeigt eine Seite, die die gleiche Fehlermeldung angezeigt, in der Fehlermeldung Details gelben Bildschirm of Death enthält (finden Sie unter **Abbildung 4**) zusammen mit den Werten der Servervariablen zum Zeitpunkt des Fehlers (finden Sie unter  **Abbildung 5**). Sie können auch die unformatierten XML-Daten in dem die Fehlerdetails gespeichert werden, anzeigen, die zusätzlichen Informationen, z. B. die Werte in den HTTP-POST-Header enthält.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Abbildung 4**: Zeigen Sie die Fehlerdetails YSOD  
([Klicken Sie, um das Bild in voller Größe anzeigen](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Abbildung 5**: Untersuchen Sie die Werte der Server Variables-Auflistung zum Zeitpunkt des Fehlers  
([Klicken Sie, um das Bild in voller Größe anzeigen](logging-error-details-with-elmah-vb/_static/image13.png))

Bereitstellen von ELMAH auf der Produktionswebsite umfasst:

- Kopieren der `Elmah.dll` -Datei in die `Bin` Ordner in der Produktion
- Kopieren die ELMAH-spezifische Konfigurationseinstellungen für die `Web.config` Datei, die in der Produktion verwendet und
- Hinzufügen von der protokollinfrastruktur Quelle Fehler in der Produktionsdatenbank.

Wir haben Methoden zum Kopieren von Dateien von der Entwicklung zur Produktion in vorherigen Tutorials behandelt. Vielleicht die einfachste Möglichkeit zum Abrufen von der protokollinfrastruktur Quelle Fehler in der Produktionsdatenbank ist, verwenden Sie SQL Server Management Studio eine Verbindung mit der Produktionsdatenbank, und führen Sie dann die `SqlServer.sql` -Skriptdatei, die die benötigte Tabelle erstellt und gespeichert Verfahren.

### <a name="viewing-the-error-details-page-on-production"></a>Anzeigen der Detailseite der Fehler für Produktionssysteme

Klicken Sie nach Ihrer Website in einer produktionsumgebung bereitstellen, finden Sie auf der Produktionswebsite, und generieren Sie eine nicht behandelte Ausnahme. Wie in der Entwicklungsumgebung hat ELMAH keine Auswirkungen auf die Fehlerseite angezeigt, wenn eine nicht behandelte Ausnahme auftritt; Stattdessen wird lediglich den Fehler protokolliert. Wenn Sie versuchen, auf der Seite "Fehler" finden Sie unter (`elmah.axd`) aus der produktionsumgebung werden Sie mit der Seite unzulässig in begrüßt werden **Abbildung 6**.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Abbildung 6**: Standardmäßig können nicht Remote Besucher der Fehler Log-Webseite anzeigen  
([Klicken Sie, um das Bild in voller Größe anzeigen](logging-error-details-with-elmah-vb/_static/image16.png))

Bedenken Sie, dass in der Konfigurations des ELMAH `<security>` Abschnitt wir legen die `allowRemoteAccess` -Attribut auf 0 (null) und verhindert, dass Remotebenutzer Anzeigen des Fehlerprotokolls. Es ist wichtig, anonyme Besucher aus Anzeigen des Fehlerprotokolls, wie Sie die Fehlerdetails zu Sicherheitsrisiken oder andere vertrauliche Informationen offenlegen können verhindert werden soll. Wenn Sie dieses Attribut auf 1 festgelegt, und Aktivieren des Remotezugriffs im Fehlerprotokoll möchten es ist wichtig, die Sperren der `elmah.axd` Pfad, sodass nur Besucher autorisierte darauf zugreifen kann. Dies kann erreicht werden, durch das Hinzufügen einer `<location>` Element, das `Web.config` Datei.

Die folgende Konfiguration ermöglicht nur Benutzern die Administratorrolle aus, um die Fehler-Log-Webseite zugreifen:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> Die Rolle "Administrator" und die drei Benutzer im System - Scott Jisun und Alice - wurden hinzugefügt, der [ *konfigurieren eine Website, dass verwendet Anwendungsdienste* Tutorial](configuring-a-website-that-uses-application-services-vb.md). Benutzer Scott und Jisun sind Mitglieder der Rolle "Admin". Weitere Informationen zu Authentifizierung und Autorisierung, finden Sie in meinem [Website-Lernprogramme zur ASP.NET-Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Das Fehlerprotokoll in der produktionsumgebung kann jetzt von remote-Benutzern angezeigt werden; zurückgreifen **Abbildungen 3**, **4**, und **5** Screenshots der der Fehler Log-Webseite. Allerdings, wenn ein anonymer oder nicht-Administrator-Benutzer versucht, auf der Seite "Fehler" anzeigen sie werden automatisch zur Anmeldeseite (`Login.aspx`), als **abbildung7** zeigt.

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Abbildung 7**: nicht autorisierte Benutzer werden automatisch zur Anmeldeseite umgeleitet.  
([Klicken Sie, um das Bild in voller Größe anzeigen](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Programmgesteuertes Fehlerprotokollierung

Die ELMAH `ErrorLogModule` HTTP-Modul meldet nicht behandelte Ausnahmen automatisch an die angegebene Quelle. Alternativ können Sie einen Fehler protokollieren, ohne Auslösen eine nicht behandelte Ausnahme mithilfe der `ErrorSignal` Klasse und die zugehörige `Raise` Methode. Die `Raise` -Methode übergeben eine `Exception` Objekt und wird protokolliert, als ob diese Ausnahme ausgelöst wurde und hatte die ASP.NET-Laufzeit ohne behandelt werden. Der Unterschied ist jedoch, dass die Anforderung fortgesetzt werden normalerweise nach dem Ausführen der `Raise` Methode aufgerufen wurde, während eine ausgelöst wird, nicht behandelte Ausnahme unterbricht der normalen Ausführung der Anforderung und führt dazu, dass die ASP.NET-Laufzeit die konfigurierte angezeigt Fehler (Seite).

Die `ErrorSignal` -Klasse ist hilfreich in Situationen, in dem eine Aktion, die möglicherweise nicht vorhanden ist, der Fehler ist jedoch nicht katastrophale Folgen für den gesamten Vorgang ausgeführt wird. Eine Website kann beispielsweise ein Formular, das der Benutzereingaben akzeptiert, speichert es in einer Datenbank und sendet dann eine e-Mail informiert dem Benutzer enthalten, dass sie Informationen zu verarbeitet wurde. Was soll passieren, wenn die Informationen werden in der Datenbank wurde erfolgreich gespeichert, aber es ein Fehler beim Senden der e-Mail-Nachricht ist? Eine Möglichkeit wäre, eine Ausnahme auslösen und den Benutzer senden, auf die Fehlerseite. Jedoch kann dies den Benutzer verleitet verwechselt werden, die die eingegebenen Informationen nicht gespeichert wurde. Ein anderer Ansatz wäre, den e-Mail-bezogenen Fehler protokollieren, aber nicht die benutzererfahrung in keiner Weise geändert werden. Hier kommt die `ErrorSignal` -Klasse eignet.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>Fehlerbenachrichtigung per E-Mail

Zusammen mit der Protokollierung von Fehlern in einer Datenbank kann auch ELMAH konfiguriert werden, um die Fehlerdetails per e-Mail an einen angegebenen Empfänger senden. Diese Funktionalität wird bereitgestellt, durch die `ErrorMailModule` HTTP-Modul; aus diesem Grund müssen Sie diese HTTP-Modul in registrieren `Web.config` muss, um die Fehlerdetails per e-Mail zu senden.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

Geben Sie als Nächstes die Informationen über die Fehler-e-Mail in der `<elmah>` des Elements `<errorMail>` Abschnitt, der angibt, die e-Mail Absender und Empfänger, Betreff, und gibt an, ob die e-Mail-Adresse asynchron gesendet wird.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Mit den obigen Einstellungen an Stelle jedes Mal, wenn ein Laufzeitfehler tritt auf, ELMAH sendet eine e-Mail an support@example.com mit den Fehlerdetails. ELMAH des Fehler-e-Mail enthält die gleiche Informationen wie der Fehler Web Detailseite, d. h. die Fehlermeldung, die stapelüberwachung und die Servervariablen (siehe **Abbildungen 4** und **5**). Der Fehler-e-Mail enthält auch den Inhalt der Ausnahme Details gelben Bildschirm of Death als Anlage (`YSOD.html`).

**Abbildung 8** zeigt ELMAHs Fehler-e-Mail finden Sie unter generiert `Genre.aspx?ID=foo`. Während **Abbildung 8** zeigt nur die Fehler und die stapelüberwachung Ablaufverfolgung, die Servervariablen befinden sich weiter nach unten in der e-Mail Text.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Abbildung 8**: Sie können konfigurieren, dass ELMAH, um die Fehlerdetails per E-Mail senden  
([Klicken Sie, um das Bild in voller Größe anzeigen](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Nur Protokollierung von Fehlern, die von Interesse sind

Standardmäßig protokolliert ELMAH die Details jedes nicht behandelte Ausnahme, einschließlich 404 und andere HTTP-Fehler. Sie können anweisen, ELMAH diese oder andere Arten von Fehlern, die mithilfe von Filtern Fehler ignoriert werden sollen. Die Filterlogik wird ausgeführt, indem die ELMAH `ErrorFilterModule` HTTP-Modul, das müssen Sie in der Registrierung `Web.config` um die Filterlogik verwenden. Die Regeln für die Filterung werden angegeben, der `<errorFilter>` Abschnitt.

Das folgende Markup weist ELMAH 404-Fehlern nicht anmelden.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> Vergessen Sie nicht, um Fehler Filtern müssen Sie registrieren die `ErrorFilterModule` HTTP-Modul.


Die `<equal>` Element innerhalb der `<test>` Abschnitt wird als Assertion bezeichnet. Wenn die Assertion auf "true" ausgewertet wird. wird der Fehler aus ELMAH Protokoll gefiltert. Es gibt andere Assertionen zur Verfügung, darunter: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`und so weiter. Sie können auch mithilfe von Assertionen kombinieren die `<and>` und `<or>` booleschen Operatoren. Sie können auch einen einfachen JavaScript-Ausdruck als Assertion enthalten oder Ihre eigenen Assertionen in c# oder Visual Basic schreiben.

Weitere Informationen zu ELMAHs Fehler Filterfunktionen finden Sie in der [Fehlerfilterung Abschnitt](https://code.google.com/p/elmah/wiki/ErrorFiltering) in der [ELMAH Wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Zusammenfassung

ELMAH bietet es sich um einen einfachen, aber leistungsfähiges Mechanismus zum Protokollieren von Fehlern in einer ASP.NET-Webanwendung. Wie Microsoft Health-Überwachungssystem ELMAH kann Fehler in einer Datenbank protokollieren und die Fehlerdetails an ein Entwickler per e-Mail senden kann. Im Gegensatz zu das Überwachungssystem ELMAH, die standardmäßig Unterstützung für eine größere Anzahl der Fehler Log-Datenspeichern, einschließlich umfasst: Microsoft SQL Server, Microsoft Access, Oracle, XML-Dateien und andere. Darüber hinaus ELMAH bietet einen integrierten Mechanismus für die Anzeige im Fehlerprotokoll und die Details zu einem bestimmten Fehler von einer Webseite `elmah.axd`. Die `elmah.axd` Rendern Seite kann auch Fehlerinformationen als RSS-feed oder als durch Trennzeichen getrennte Datei (CSV), die Sie mit Microsoft Excel lesen können. Sie können auch ELMAH Filtern von Fehlern aus dem Protokoll, das mithilfe von deklarativer oder eine programmgesteuerter Assertions anweisen. Und ELMAH mit ASP.NET Version 1.x-Anwendungen verwendet werden kann.

Jeder bereitgestellte Anwendung sollte einen Mechanismus für die automatische Anmeldung nicht behandelte Ausnahmen und Benachrichtigung an das Entwicklungsteam gesendet haben. Gibt an, ob dies erreicht wird, mit der Überwachung der Integrität oder ELMAH ist. Das heißt, ist es unerheblich viel gibt an, ob Sie Überwachung der Integrität oder ELMAH verwenden; Evaluieren Sie beider Systeme ein, und wählen Sie dann, die Ihren Anforderungen optimal entspricht. Im Grunde wichtig ist, ein Mechanismus zum Protokollieren von nicht behandelter Ausnahmen in der produktionsumgebung eingesetzt werden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [ELMAH - Error Logging Modules and Handlers](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [ELMAH-Projektseite](https://code.google.com/p/elmah/) (source Code, Beispiele, Wiki)
- [Anschließen von ELMAH in einer Webanwendung auf nicht behandelte Ausnahmen abfangen](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (video)
- [Fehler beim Sperren von Sicherheitsseiten](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Verwenden von HTTP-Module und Handler zum Erstellen von ASP.NET-Plug-in-Komponenten](https://msdn.microsoft.com/library/aa479332.aspx)
- [Website-Lernprogramme zur ASP.NET-Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Zurück](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [Weiter](precompiling-your-website-vb.md)
