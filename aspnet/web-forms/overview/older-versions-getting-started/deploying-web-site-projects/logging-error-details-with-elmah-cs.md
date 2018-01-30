---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: ELMAH (c#) Fehlerdetails protokollieren | Microsoft Docs
author: rick-anderson
description: "Fehler beim Protokollieren Module und Handler (ELMAH) bietet einen anderen Ansatz für die Protokollierung von Common Language Runtime Fehlern in einer produktiven Umgebung. ELMAH ist ein kostenfreies open Source-Fehler..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: 26d40d17447b3b03d17265f291b8ac246a449966
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
<a name="logging-error-details-with-elmah-c"></a>ELMAH (c#) protokollieren Fehlerdetails
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> Fehler beim Protokollieren Module und Handler (ELMAH) bietet einen anderen Ansatz für die Protokollierung von Common Language Runtime Fehlern in einer produktiven Umgebung. ELMAH ist ein kostenfreies open Source Fehler Protokollierung der Bibliothek, die Funktionen wie Filtern Fehler sowie die Möglichkeit, das Fehlerprotokoll von einer Webseite als RSS-feed anzeigen oder um sie als eine durch Trennzeichen getrennte Datei herunterzuladen. Dieses Lernprogramm führt Sie durch Herunterladen und Konfigurieren von ELMAH.


## <a name="introduction"></a>Einführung

Die [vorherigen Lernprogramm](logging-error-details-with-asp-net-health-monitoring-cs.md) ASP untersucht. NET Überwachungssystem, die eine Out-of der Feld-Bibliothek zum Aufzeichnen von vielen verschiedenen Webereignisse bietet. Viele Entwickler verwenden zum Protokollieren und per e-Mail die Details der nicht behandelten Ausnahmen für die Systemüberwachung. Es gibt jedoch einige Problembereiche mit diesem System. Zunächst ist das fehlen jegliche Art von Benutzeroberfläche zum Anzeigen von Informationen zu den protokollierten Ereignissen. Wenn Sie möchten finden eine Zusammenfassung der letzten 10 Fehler, oder zeigen Sie die Details eines Fehlers, die letzte Woche aufgetreten sind, müssen Sie entweder Poke über die Datenbank, Durchsuchen Sie Ihre e-Mail-Posteingang oder erstellen Sie eine Webseite, die Anzeige von Informationen aus der `aspnet_WebEvent_Events` Tabelle.

Eine andere Schwachstelle dreht sich alles um Komplexität Health überwacht. Da Systemüberwachung verwendet werden kann eine Vielzahl von verschiedenen Ereignissen aufgezeichnet, und es gibt eine Vielzahl von Optionen angewiesen wird, wie und wann Ereignisse protokolliert werden, kann die ordnungsgemäße Konfiguration System für die Systemüberwachung einen Task sehr aufwändig sein. Schließlich sind Kompatibilitätsprobleme. Da Systemüberwachung zuerst .NET Framework Version 2.0 hinzugefügt wurde, ist es nicht verfügbar für ältere Webanwendungen mit ASP.NET-Version erstellte 1.x. Darüber hinaus die `SqlWebEventProvider` Klasse, die wir im vorherigen Lernprogramm Protokolle Fehlerdetails in einer Datenbank verwendet, funktioniert nur mit Microsoft SQL Server-Datenbanken. Sie benötigen, um ein benutzerdefiniertes Protokoll Anbieterklasse zu erstellen, sollten Sie zum Protokollieren von Fehlern an einen alternativen Datenspeicher, z. B. eine XML-Datei oder einer Oracle-Datenbank durchführen müssen.

Eine Alternative zum System für die Systemüberwachung ist Fehler protokollieren Module und Handler (ELMAH), eine kostenlose, Open Source-Fehler Protokollierungssystem erstellt, indem [Atif Aziz](http://www.raboof.com/). Der wichtigste Unterschied zwischen den beiden Systemen ist ELAMHs Möglichkeit der Anzeige einer Liste von Fehlern und die Details eines bestimmten Fehlers von einer Webseite und als RSS-feed. ELMAH ist einfacher zu konfigurieren als Health überwacht werden, da es nur Fehler protokolliert. ELMAH enthält darüber hinaus Unterstützung für ASP.NET 1.x, ASP.NET 2.0 und ASP.NET 3.5-Anwendungen und umfasst eine Vielzahl von Protokollanbietern für die Quelle.

Dieses Lernprogramm führt durch die Schritte in einer ASP.NET-Anwendung ELMAH hinzugefügt. Fangen wir an!

> [!NOTE]
> Die Systemüberwachung "System" und ELMAH sowohl die eigene Sätze von vor- und Nachteile haben. Ich empfehlen Ihnen, wiederholen Sie den beiden Systemen und entscheiden, welche eine optimale Suits Ihren Anforderungen.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Hinzufügen von ELMAH an eine ASP.NET-Webanwendung

Integrieren von ELMAH in einer neuen oder vorhandenen ASP.NET-Anwendung ist ein einfach und unkompliziert Prozess, der unter fünf Minuten in Anspruch nimmt. Kurz gesagt umfasst es vier einfache Schritten:

1. ELMAH herunterladen und Hinzufügen der `Elmah.dll` Assembly zu einer Webanwendung
2. Registrieren des ELMAH-HTTP-Module und Ereignishandler in `Web.config`,
3. Angeben ELMAHs-Konfigurationsoptionen, und
4. Erstellen Sie die Fehler-Protokoll-quellinfrastruktur, sofern erforderlich.

Wir führen Sie durch jede der folgenden vier Schritte aus, einzeln an.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Schritt 1: Herunterladen von ELMAH-Projektdateien und Hinzufügen von`Elmah.dll`für Ihre Webanwendung

ELMAH 1.0 BETA 3 (10617 erstellen), die neueste Version zum Zeitpunkt der Verfassung, ist im Download zur Verfügung, mit diesem Lernprogramm enthalten. Alternativ können Sie besuchen die [ELMAH Website](https://code.google.com/p/elmah/) auf die neueste Version zu erhalten oder um den Quellcode herunterladen. Extrahieren Sie den Download ELMAH in einen Ordner auf dem Desktop, und suchen Sie die Assemblydatei ELMAH (`Elmah.dll`).

> [!NOTE]
> Die `Elmah.dll` befindet sich in des Downloads `Bin` Ordner, Unterordner für unterschiedliche .NET Framework-Versionen und für die Release- und Debugkonfigurationen Builds. Verwenden Sie den Releasebuild für die entsprechenden Frameworkversion. Kopieren Sie z. B. Wenn Sie eine ASP.NET 3.5-Web-Anwendung erstellen, die `Elmah.dll` -Datei von der `Bin\net-3.5\Release` Ordner.


Als Nächstes öffnen Sie Visual Studio, und fügen Sie die Assembly zum Projekt mit der rechten Maustaste auf den Namen der Website im Projektmappen-Explorer und wählen Sie im Kontextmenü den Befehl Verweis hinzufügen. Daraufhin wird das Dialogfeld "Verweis hinzufügen". Navigieren Sie zur Registerkarte "Durchsuchen", und wählen Sie die `Elmah.dll` Datei. Mit dieser Aktion wird die `Elmah.dll` Datei an der Webanwendung `Bin` Ordner.

> [!NOTE]
> Der Typ Web Application Project (WAP) wird nicht angezeigt. die `Bin` Ordner im Projektmappen-Explorer. Stattdessen werden diese Elemente unter dem Ordner "Verweise" aufgeführt.


Die `Elmah.dll` -Assembly enthält, die vom System ELMAH verwendeten Klassen. Diese Klassen werden in drei Kategorien unterteilt:

- **HTTP-Module** -ein HTTP-Modul ist eine Klasse, die Ereignishandler für definiert `HttpApplication` Ereignisse, z. B. die `Error` Ereignis. ELMAH enthält mehrere HTTP-Module, die am häufigsten von Belang drei Konfigurationen wird: 

    - `ErrorLogModule`-eine Protokollquelle nicht behandelte Ausnahmen protokolliert.
    - `ErrorMailModule`-die Details einer nicht behandelten Ausnahme in einer e-Mail-Nachricht sendet.
    - `ErrorFilterModule`-Entwickler angegebene Filter, um zu bestimmen, welche Ausnahmen protokolliert werden und welche gilt Einsen werden ignoriert.
- **HTTP-Handler** -HTTP-Handler ist eine Klasse, die zum Generieren von Markup für eine bestimmte Art von Anforderung zuständig ist. ELMAH enthält die HTTP-Handler, die Fehlerdetails als eine Webseite, als RSS-feed oder als eine durch Trennzeichen getrennte Datei (CSV) rendern.
- **Fehler-Protokollquellen** - gebrauchsfertigen ELMAH Fehler in den Arbeitsspeicher, um eine Microsoft SQL Server-Datenbank, um eine Microsoft Access-Datenbank mit einer Oracle-Datenbank, um anmelden kann eine XML-Datei in eine SQLite-Datenbank oder in einer Datenbank von Vista-DB. Wie das Überwachungssystem wurde ELMAHs-Architektur mit Anbietermodell, was bedeutet, dass Sie erstellen und eigene benutzerdefinierte Quelle Protokollanbieter nahtlos integrieren, bei Bedarf erstellt.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Schritt 2: Registrieren des ELMAH-HTTP-Modul und Ereignishandler

Während der `Elmah.dll` -Datei enthält den HTTP-Modulen und Handler erforderlich ist, automatisch nicht behandelte Ausnahmen zu protokollieren und um Fehlerdetails von einer Webseite anzuzeigen, diese müssen explizit in der Konfigurationsdatei der Webanwendung registriert werden. Die `ErrorLogModule` HTTP-Modul, nach der Registrierung abonniert die `HttpApplication`des `Error` Ereignis. Dieses Ereignis wird ausgelöst, wenn die `ErrorLogModule` die Details der Ausnahme mit einer Datenquelle angegebenen Protokoll protokolliert. Wir sehen zum Definieren des Protokollanbieters für die Datenquelle im nächsten Abschnitt "Konfigurieren von ELMAH". Die `ErrorLogPageFactory` HTTP-Handler-Factory ist verantwortlich für das Markup generiert, wenn Sie das Fehlerprotokoll von einer Webseite anzeigen.

Die spezifische Syntax zum Registrieren von HTTP-Module und Handler richtet sich nach der Webserver, der den Standort eingeschaltet ist. Für den ASP.NET Development Server und Microsofts IIS Version 6.0 und früher, HTTP-Module und Handler im registriert sind die `<httpModules>` und `<httpHandlers>` Abschnitte, die innerhalb der `<system.web>` Element. Wenn Sie IIS 7.0 verwenden, müssen sie in registriert werden die `<system.webServer>` des Elements `<modules>` und `<handlers>` Abschnitte. Glücklicherweise können Sie definieren, die HTTP-Module und Handler in *beide* platziert werden, unabhängig von der Webserver verwendet wird. Diese Option wird die meisten tragbaren wie die gleiche Konfiguration in den Entwicklungs- und produktionsumgebungen Umgebungen unabhängig von der Webserver verwendeten verwendet werden können.

Starten, indem Sie beim Registrieren der `ErrorLogModule` HTTP-Modul und die `ErrorLogPageFactory` HTTP-Handler in der `<httpModules>` und `<httpHandlers>` im Abschnitt `<system.web>`. Wenn Ihre Konfiguration bereits dieser beiden Elemente klicken Sie dann einfach definiert enthalten die `<add>` für HTTP-Modul und die Handler des ELMAH-Element.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

Als Nächstes Registrieren ELMAHs-HTTP-Modul und Ereignishandler in der `<system.webServer>` Element. Wenn dieses Element nicht bereits in Ihrer Konfiguration vorhanden ist dann fügen Sie ihn.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

Standardmäßig meldet IIS 7 in den HTTP-Module und Handler registriert werden die `<system.web>` Abschnitt. Die `validateIntegratedModeConfiguration` Attribut in der `<validation>` Element weist IIS 7 solche Fehlermeldungen unterdrückt.

Beachten Sie, dass die Syntax für die Registrierung der `ErrorLogPageFactory` HTTP-Handler enthält eine `path` -Attribut, das festgelegt wird, um `elmah.axd`. Dieses Attribut wird der Webanwendung, auch wenn eine Anforderung eingeht, für eine Seite mit dem Namen `elmah.axd` und dann die Anforderung sollen, durch verarbeitet werden die `ErrorLogPageFactory` HTTP-Handler. Wir sehen den `ErrorLogPageFactory` HTTP-Handler in der Aktion weiter unten in diesem Lernprogramm.

### <a name="step-3-configuring-elmah"></a>Schritt 3: Konfigurieren von ELMAH

ELMAH sucht die Konfigurationsoptionen auf der Website `Web.config` Datei in einem benutzerdefinierten Konfigurationsabschnitt, der mit dem Namen `<elmah>`. Zum Verwenden eines benutzerdefinierten Abschnitts in `Web.config` muss zuerst definiert werden die `<configSections>` Element. Öffnen der `Web.config` Datei, und fügen Sie das folgende Markup zum Rendern der `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> Wenn Sie für eine ASP.NET-Anwendung für die 1.x ELMAH konfigurieren und entfernen Sie dann die `requirePermission="false"` -Attribut aus der `<section>` oben aufgeführten Elemente.


Die oben aufgeführten Syntax registriert die benutzerdefinierten `<elmah>` Abschnitt und die Unterabschnitte: `<security>`, `<errorLog>`, `<errorMail>`, und `<errorFilter>`.

Als Nächstes fügen Sie der `<elmah>` Abschnitt `Web.config`. In diesem Abschnitt sollte angezeigt werden, auf der gleichen Ebene wie die `<system.web>` Element. Innerhalb der `<elmah>` Abschnitt Hinzufügen der `<security>` und `<errorLog>` Abschnitte wie folgt:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

Die `<security>` Bereich "Benutzerportal" `allowRemoteAccess` Attribut gibt an, ob der Remotezugriff zulässig ist. Wenn dieser Wert auf 0 festgelegt ist, kann der Fehler Protokoll Webseite nur lokal angezeigt. Wenn dieses Attribut auf 1 festgelegt ist ist der Fehler Protokoll-Webseite für Remote- und lokale Besucher aktiviert. Jetzt sehen wir Deaktivieren der Fehler Protokoll Web für remote Besucher. Remotezugriff zulässig später, nachdem es die Möglichkeit, die Sicherheitsaspekte, dies hat erörtern möchten.

Die `<errorLog>` Abschnitt definiert die Protokollquelle Fehler, welche gibt an, in denen die Fehlerdetails aufgezeichnet werden, da ähnelt, der `<providers>` Abschnitt im System für die Systemüberwachung. Gibt an, die oben aufgeführten Syntax der `SqlErrorLog` Klasse als Protokoll Fehlerquelle, welche die Fehler mit einer Microsoft SQL Server-Datenbank, die gemäß Protokolle der `connectionStringName` Attributwert.

> [!NOTE]
> ELMAH geliefert wird mit zusätzlichen Fehlerinformationen von Protokollanbietern, die zum Protokollieren von Fehlern zu einer XML-Datei, eine Microsoft Access-Datenbank, einer Oracle-Datenbank und anderen Datenspeichern verwendet werden können. Im Beispiel `Web.config` -Datei, die mit dem ELMAH Download Informationen zur Verwendung dieser Protokollanbieter alternativen Fehler enthalten ist.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Schritt 4: Erstellen der Protokoll-Quelle Fehlerinfrastruktur

ELMAH des `SqlErrorLog` -Anbieter protokolliert Fehlerinformationen in eine angegebene Microsoft SQL Server-Datenbank. Die `SqlErrorLog` Anbieter erwartet, dass diese Datenbank eine Tabelle mit dem Namen `ELMAH_Error` und drei gespeicherte Prozeduren: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, und `ELMAH_LogError`. Das ELMAH-Download umfasst eine Datei namens `SQLServer.sql` in den `db` Ordner mit der T-SQL zum Erstellen dieser Tabelle und die folgenden gespeicherten Prozeduren. Sie müssen diese Anweisungen in Ihrer Datenbank zu verwenden, führen die `SqlErrorLog` Anbieter.

**Abbildung 1** und **2** Datenbank-Explorer in Visual Studio nach der erforderlichen Datenbankobjekte Anzeigen der `SqlErrorLog` Anbieter hinzugefügt wurden.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Abbildung 1**: die `SqlErrorLog` -Anbieter protokolliert Fehler, um die `ELMAH_Error` Tabelle

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Abbildung 2**: die `SqlErrorLog` -Anbieter verwendet drei gespeicherte Prozeduren

## <a name="elmah-in-action"></a>ELMAH In Aktion

Zu diesem Zeitpunkt haben wir ELMAH hinzugefügt, auf die Webanwendung registriert die `ErrorLogModule` HTTP-Modul und die `ErrorLogPageFactory` HTTP-Handler angegeben ELMAHs-Konfigurationsoptionen in `Web.config`, und die erforderlichen Datenbankobjekte für hinzugefügt der `SqlErrorLog` Fehler-Protokollanbieter. Wir können jetzt ELMAH in Aktion zu sehen. Besuchen Sie die Website Buch Kritiken und finden Sie auf eine Seite, die einen Laufzeitfehler, wie z. B. generiert `Genre.aspx?ID=foo`, oder eine nicht existierende-Seite, wie z. B. `NoSuchPage.aspx`. Was Sie sehen, wenn diese Seiten besucht richtet sich nach der `<customErrors>` Konfigurations- und gibt an, ob Sie lokal oder Remote aufrufen. (Verweisen zurück auf den [ *eine benutzerdefinierte Fehlerseite anzeigen* Lernprogramm](displaying-a-custom-error-page-cs.md) aufzufrischen zu diesem Thema.)

ELMAH keinen Einfluss auf welche Inhalte an den Benutzer angezeigt wird, wenn eine nicht behandelte Ausnahme auftritt; nur protokolliert die Details. Diesem Fehlerprotokoll kann zugegriffen werden, auf der Webseite `elmah.axd` aus dem Stammverzeichnis der Website, z. B. `http://localhost/BookReviews/elmah.axd`. (Diese Datei ist physisch nicht vorhanden, in Ihrem Projekt, aber wenn eine Anforderung für eingehen `elmah.axd` die Laufzeit sendet er den `ErrorLogPageFactory` HTTP-Handler, die das Markup zurück an den Browser gesendet generiert.)

> [!NOTE]
> Sie können auch die `elmah.axd` Seite ELMAH zum Generieren von einem Testfehler angewiesen. Zugriff auf `elmah.axd/test` (wie in `http://localhost/BookReviews/elmah.axd/test`) bewirkt, dass eine Ausnahme vom Typ ELMAH `Elmah.TestException`, enthält die Fehlermeldung angezeigt: "This is ein testausnahmefehler, die ignoriert werden kann."


**Abbildung 3** zeigt das Fehlerprotokoll an, beim Zugriff auf `elmah.axd` aus der Entwicklungsumgebung.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Abbildung 3**: `Elmah.axd` zeigt das Fehlerprotokoll von einer Webseite  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](logging-error-details-with-elmah-cs/_static/image7.png))

Das Fehlerprotokoll **Abbildung 3** enthält sechs Fehlereinträge. Jeder Eintrag enthält den HTTP-Statuscode ("404" oder "500, für diese Fehler"), den Typ, die Beschreibung, den Namen des angemeldeten Benutzers bei der der Fehler aufgetreten ist, sowie Datum und Uhrzeit. Auf den Link Details Zeigt eine Seite, die die gleiche Fehlermeldung angezeigt, in dem Fehler Details gelben Bildschirm des Tod enthält (siehe **Abbildung 4**) zusammen mit den Werten der Servervariablen zum Zeitpunkt des Fehlers (finden Sie unter  **Abbildung 5**). Sie können auch das unformatierte XML, in dem die Fehlerdetails gespeichert sind, anzeigen, die zusätzlichen Informationen, z. B. die Werte in den HTTP-POST-Header enthält.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Abbildung 4**: Zeigen Sie die Fehlerdetails YSOD an.  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Abbildung 5**: Untersuchen Sie die Werte der Server Variables-Auflistung zum Zeitpunkt des Fehlers  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](logging-error-details-with-elmah-cs/_static/image13.png))

Bereitstellen von ELMAH und der Produktionswebsite umfasst:

- Kopieren der `Elmah.dll` Datei wird in der `Bin` Ordner für die Produktion
- Kopieren die ELMAH-spezifische Konfigurationseinstellungen für die `Web.config` Datei, die für die Produktion verwendet und
- Hinzufügen der Protokoll-Quelle Fehlerinfrastruktur in der Produktionsdatenbank.

Wir haben die Techniken zum Kopieren von Dateien von der Entwicklung bis hin zur Produktion in vorherigen Lernprogrammen untersucht. Möglicherweise ist die einfachste Möglichkeit, die Quelle der Fehlerinfrastruktur-Protokoll in der Produktionsdatenbank zu erhalten, verwenden SQL Server Management Studio eine Verbindung mit der Produktionsdatenbank, und führen Sie dann die `SqlServer.sql` Skriptdatei, die die benötigte Tabelle erstellt und gespeichert Prozeduren.

### <a name="viewing-the-error-details-page-on-production"></a>Anzeigen der Details auf in Produktion

Besuchen Sie nach der Bereitstellung Ihrer Website bis hin zur Produktion die Produktionswebsite und eine nicht behandelte Ausnahme generiert. Wie in der Entwicklungsumgebung hat ELMAH keine Auswirkung auf die Fehlerseite angezeigt, wenn eine nicht behandelte Ausnahme auftritt; Stattdessen protokolliert er lediglich der Fehler. Wenn Sie versuchen, besuchen die Fehlerseite-Protokoll (`elmah.axd`) aus der produktionsumgebung, werden Sie der Seite "verboten" gezeigt begrüßt werden **Abbildung 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Abbildung 6**: Standardmäßig können nicht Remote Besucher der Webseite des Fehler-Protokoll anzeigen  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](logging-error-details-with-elmah-cs/_static/image16.png))

Bedenken Sie, dass in der ELMAH Konfigurations `<security>` Abschnitt wir legen den `allowRemoteAccess` -Attribut auf 0 (null) und verhindert, dass Remotebenutzer Anzeigen des Fehlerprotokolls. Es ist wichtig, anonyme Besucher von Anzeigen des Fehlerprotokolls wie die Fehlerdetails Sicherheitsrisiken oder andere vertrauliche Informationen offenlegen möglicherweise verhindert werden soll. Wenn Sie dieses Attribut auf 1 festgelegt, und Aktivieren des Remotezugriffs auf das Fehlerprotokoll möchten und es wichtig, Sperren ist der `elmah.axd` Pfad, sodass Besucher nur von autorisierten darauf zugreifen kann. Dies kann durch Hinzufügen einer `<location>` Element an der `Web.config` Datei.

Die folgende Konfiguration erlaubt nur Benutzer in der Administratorrolle für die Fehler Protokoll-Webseite zugreifen:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> Die Rolle "Admin" und die drei Benutzer im System - Scott Jisun und Alice - wurden hinzugefügt, der [ *Konfigurieren einer Website, dass verwendet Anwendungsdienste* Lernprogramm](configuring-a-website-that-uses-application-services-cs.md). Benutzer Scott und Jisun sind Mitglieder der Rolle "Admin". Weitere Informationen zur Authentifizierung und Autorisierung finden Sie in meinem [Website-Lernprogramme](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Das Fehlerprotokoll in der produktionsumgebung kann jetzt von Remotebenutzern angezeigt werden; Verweisen auf **Zahlen 3**, **4**, und **5** für Screenshots der Webseite Fehler Protokoll. Jedoch, wenn ein anonymer oder non-Admin Benutzer versucht, die Protokoll-Fehlerseite anzeigen er werden automatisch umgeleitet zur Anmeldeseite (`Login.aspx`), als **Abbildung 7** zeigt.

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Abbildung 7**: nicht autorisierte Benutzer werden automatisch an die Anmeldeseite umgeleitet.  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Programmgesteuertes Protokollierung von Fehlern

ELMAH des `ErrorLogModule` HTTP-Modul protokolliert automatisch nicht behandelte Ausnahmen in der Quelle angegebenen Protokoll. Alternativ können Sie einen Fehler protokollieren, ohne eine nicht behandelte Ausnahme mithilfe Auslösen der `ErrorSignal` Klasse und ihre `Raise` Methode. Die `Raise` -Methode übergeben ein `Exception` Objekt, und es protokolliert, als ob diese Ausnahme ausgelöst wurde, und hat die ASP.NET-Laufzeit erreicht, ohne verarbeitet wird. Der Unterschied ist jedoch, dass die Anforderung fortgesetzt, normalerweise nach dem Ausführen der `Raise` -Methode aufgerufen wurde, während eine ausgelöst wird, eine nicht behandelte Ausnahme unterbricht der normalen Ausführung der Anforderung und führt dazu, dass die ASP.NET-Laufzeit die konfigurierte angezeigt Fehler (Seite).

Die `ErrorSignal` Klasse ist hilfreich in Situationen, in denen eine Aktion, die möglicherweise nicht vorhanden ist, aber der Fehler ist nicht schwerwiegenden auf den gesamten Vorgang ausgeführt wird. Beispielsweise darf eine Website ein Formular, das die Eingabe des Benutzers verwendet, speichert es in einer Datenbank und sendet dann die Benutzer per e-Mail informiert wird, dass sie Informationen verarbeitet wurde. Was soll passieren, wenn die Informationen in der Datenbank erfolgreich gespeichert ist, aber ein Fehler aufgetreten ist, wenn die e-Mail-Nachricht zu senden? Eine Möglichkeit wäre, lösen eine Ausnahme aus, und den Benutzer auf die Fehlerseite senden. Jedoch kann dies die Benutzer verleitet verwechselt werden, die die eingegebenen Informationen nicht gespeichert wurde. Ein anderer Ansatz wäre, den e-Mail-bezogenen Fehler protokollieren, aber nicht die benutzererfahrung in keiner Weise geändert. Dies ist, wenn die `ErrorSignal` Klasse ist hilfreich.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>Fehlerbenachrichtigung per E-Mail

Zusammen mit der Protokollierung von Fehlern in einer Datenbank kann auch ELMAH konfiguriert werden, um die Fehlerdetails per e-Mail an einen angegebenen Empfänger senden. Diese Funktionalität wird bereitgestellt, indem Sie die `ErrorMailModule` HTTP-Modul; aus diesem Grund müssen Sie diese HTTP-Modul in registrieren `Web.config` um Fehlerdetails per e-Mail zu senden.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

Als Nächstes geben Sie Informationen zu dem Fehler e-Mails in der `<elmah>` des Elements `<errorMail>` Abschnitt, der angibt, die e-Mail Absender und Empfänger, den Betreff und gibt an, ob die e-Mail-Adresse asynchron gesendet wird.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Mit den oben genannten Einstellungen vorhanden, wenn ein Laufzeitfehler auftritt ELMAH sendet eine e-Mail an support@example.com mit den Fehlerdetails. ELMAH des Fehler-e-Mail enthält die gleiche Informationen wie der Fehler Details Webseite, nämlich die Fehlermeldung, die stapelüberwachung und die Servervariablen (verweisen zurück auf **Abbildungen 4** und **5**). Der Fehler-e-Mail enthält auch den Inhalt der Ausnahme Details gelben Bildschirm der Tod als Anlage (`YSOD.html`).

**Abbildung 8** zeigt ELMAHs Fehler-e-Mail von Besuchen generierten `Genre.aspx?ID=foo`. Während **Abbildung 8** zeigt nur der Fehler und die stapelüberwachung Ablaufverfolgung, die Servervariablen sind weitere enthalten nach unten in der e-Mail-Text.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Abbildung 8**: Sie können konfigurieren, dass ELMAH zum Senden von Fehlerdetails per E-Mail  
([Klicken Sie hier, um das Bild in voller Größe angezeigt](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Nur Protokollierung von Fehlern, die von Interesse sind

Standardmäßig protokolliert ELMAH die Details zu allen unbehandelten Ausnahmen, einschließlich 404 und andere HTTP-Fehler. Sie können anweisen, ELMAH, diesen oder anderen Arten von Fehlern, die mithilfe von Filtern Fehler ignoriert werden sollen. Die Filterlogik erfolgt durch die ELMAH `ErrorFilterModule` HTTP-Modul, das müssen Sie zum Registrieren im `Web.config` um die Filterlogik zu verwenden. Die Regeln für das Filtern werden angegeben, der `<errorFilter>` Abschnitt.

Das folgende Markup weist ELMAH nicht 404-Fehler zu protokollieren.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> Vergessen Sie nicht, um Fehler zu filtern, Sie registrieren müssen, verwenden die `ErrorFilterModule` HTTP-Modul.


Die `<equal>` Element innerhalb der `<test>` Abschnitt wird als Assertion bezeichnet. Wenn die Assertion auf "true" ergibt wird der Fehler aus dem Protokoll des ELMAH gefiltert. Andere Assertionen verfügbar sind, einschließlich: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`und so weiter. Sie können auch mit Assertionen Kombinieren der `<and>` und `<or>` boolesche Operatoren. Sie können auch einen einfachen Ausdruck für JavaScript als Assertion enthalten oder eigene Assertionen in c# oder Visual Basic schreiben.

Weitere Informationen zu Filterfunktionen ELMAHs-Fehler finden Sie in der [Abschnitt Fehler filtern](https://code.google.com/p/elmah/wiki/ErrorFiltering) in der [ELMAH Wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Zusammenfassung

ELMAH bietet einen einfache, aber leistungsstarken Mechanismus zum Protokollieren von Fehlern in einer ASP.NET Web-Anwendung. Wie Microsoft Health-Überwachungssystem ELMAH kann Fehler in einer Datenbank protokollieren und den Fehlerdetails an ein Entwickler per e-Mail senden. Im Gegensatz zu das Überwachungssystem ELMAH enthält, aus der Box-Unterstützung für eine größere Anzahl von Fehler Protokoll Datenspeichern, einschließlich: Microsoft SQL Server, Microsoft Access, Oracle, XML-Dateien und einigen weiteren Bildschirmen. Darüber hinaus bietet ELMAH einen eingebauten Mechanismus für die Anzeige im Fehlerprotokoll und die Details zu einem bestimmten Fehler von einer Webseite `elmah.axd`. Die `elmah.axd` Seite kann auch Rendern Fehlerinformationen als RSS-feed oder als eine durch Trennzeichen getrennte Datei (CSV), die mit Microsoft Excel gelesen werden können. Sie können auch ELMAH filtern Sie Fehler anweisen, aus dem Protokoll, verwenden deklarative oder programmgesteuerte Assertionen. Und ELMAH mit Version 1.x von ASP.NET-Anwendungen verwendet werden kann.

Jede bereitgestellte Anwendung sollte einen Mechanismus für die automatische Anmeldung nicht behandelte Ausnahmen und Senden der Benachrichtigung an das Entwicklungsteam verfügen. Gibt an, ob dies erreicht wird, mithilfe von Systemüberwachung oder ELMAH ist sekundären. In anderen Worten, ist es unerheblich, viel gibt an, ob Sie Systemüberwachung oder ELMAH verwenden; Auswerten von beiden Systemen, und wählen Sie dann das Projekt, das Ihren Anforderungen am besten entspricht. Im Grunde wichtig ist, dass ein Mechanismus bereit, um zu melden Sie sich nicht behandelte Ausnahmen in der produktionsumgebung eingefügt werden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ELMAH - Error Logging Modules and Handlers](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [ELMAH-Projektseite](https://code.google.com/p/elmah/) (source Code, Beispiele, Wiki)
- [Unter Verwendung der ELMAH in einer Webanwendung auf nicht behandelte Ausnahmen abfangen](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (video)
- [Fehler beim Sperren von Sicherheitsseiten](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Mithilfe von HTTP-Module und Handler austauschbare ASP.NET Komponenten erstellen](https://msdn.microsoft.com/library/aa479332.aspx)
- [Lernprogramme für Website-Sicherheit](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

>[!div class="step-by-step"]
[Zurück](logging-error-details-with-asp-net-health-monitoring-cs.md)
[Weiter](precompiling-your-website-cs.md)
