---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Schützen von Verbindungszeichenfolgen und anderen Konfigurationsinformationen (VB) | Microsoft Docs
author: rick-anderson
description: Eine ASP.NET-Anwendung speichert die Konfigurationsinformationen in der Regel in einer Datei "Web.config". Einige dieser Informationen ist vertraulich und Schutz gewährleistet. Indem Sie mit DEF...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 3372416dd9143afbfd442eaffb39cd807fae0de6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Schützen von Verbindungszeichenfolgen und anderen Konfigurationsinformationen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) oder [PDF herunterladen](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Eine ASP.NET-Anwendung speichert die Konfigurationsinformationen in der Regel in einer Datei "Web.config". Einige dieser Informationen ist vertraulich und Schutz gewährleistet. Standardmäßig wird diese Datei kein Besucher der Website bedient werden, aber ein Administrator oder ein Hacker Zugriff auf das Dateisystem des Webservers und zeigen Sie den Inhalt der Datei kann. In diesem Lernprogramm erfahren Sie, dass ASP.NET 2.0 uns ermöglicht, die vertraulichen Informationen zu schützen, indem verschlüsselt Abschnitte der Datei "Web.config".


## <a name="introduction"></a>Einführung

Konfigurationsinformationen für ASP.NET-Anwendungen werden häufig in einer XML-Datei mit dem Namen gespeichert `Web.config`. Im Verlauf der Ausführung dieser Lernprogramme wurden aktualisiert die `Web.config` eine Handvoll Zeiten. Beim Erstellen der `Northwind` typisierte DataSet in die [ersten Lernprogramm](../introduction/creating-a-data-access-layer-vb.md), z. B. Verbindungszeichenfolgen-Informationen automatisch hinzugefügt wurde `Web.config` in die `<connectionStrings>` Abschnitt. Weiter unten in der [Masterseiten und Websitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Lernprogramm wurde manuell aktualisiert `Web.config`, Hinzufügen von einer `<pages>` Element gibt an, dass alle Seiten ASP.NET im Projekt verwendet werden soll die `DataWebControls` Design.

Seit `Web.config` möglicherweise sensible Daten wie Verbindungszeichenfolgen, es ist wichtig, den Inhalt des `Web.config` sicher und nicht autorisierte Viewer verborgen bleiben. Standardmäßig alle HTTP-Anforderung, in eine Datei mit der `.config` Erweiterung wird vom ASP.NET-Modul die zurückgibt, verarbeitet die *dieser Seitentyp wird nicht verarbeitet* Nachricht, die in Abbildung 1 dargestellt. Dies bedeutet, dass Besucher nicht einsehen dürfen Ihre `Web.config` s Dateiinhalt einfach durch Eingabe der http://www.YourServer.com/Web.config in die Adressleiste des Browsers s.


[![Besuchen die Datei "Web.config" über ein Browser gibt einen Typ der Seite wird Nachricht nicht verarbeitet](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Abbildung 1**: Besuchen `Web.config` über ein Browser gibt einen Typ der Seite wird die Nachricht nicht verarbeitet ([klicken Sie hier, um das Bild in voller Größe angezeigt](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))


Aber was geschieht, wenn ein Angreifer kann einige andere Exploit finden, die sie anzeigen können Ihre `Web.config` s Dateiinhalt? Was könnte ein Angreifer bewirken, mit diesen Informationen und welche Schritte kann getroffen werden, weiter die vertrauliche Informationen innerhalb `Web.config`? Glücklicherweise den meisten Abschnitten in `Web.config` enthalten keine vertraulichen Informationen. Welche Schäden kann ein Angreifer auch, wenn sie wissen, dass der Name der Standardinstallation Design von ASP.NET-Seiten verwendet?

Bestimmte `Web.config` Abschnitte, allerdings enthalten vertraulichen Informationen, die Verbindungszeichenfolgen, Benutzernamen, Kennwörter, Servernamen, Verschlüsselungsschlüssel usw. enthalten können. Diese Informationen finden Sie in der Regel in der folgenden `Web.config` Abschnitte:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

In diesem Lernprogramm werden wir die Techniken zum Schützen von solche vertraulichen Konfigurationsinformationen betrachten. Da wir angezeigt wird, enthält .NET Framework, Version 2.0 einer geschützten Konfigurationen-System, das programmgesteuert verschlüsseln und Entschlüsseln der ausgewählten Konfigurationsabschnitte ein Kinderspiel gestaltet.

> [!NOTE]
> In diesem Lernprogramm endet mit einem Blick auf Microsoft-s-Empfehlungen zum Herstellen einer Verbindung mit einer Datenbank von einer ASP.NET-Anwendung. Zusätzlich zum Verschlüsseln der Verbindungszeichenfolgen, hilft Ihnen, Ihr System Leuchten Härten, den Sie mit der Datenbank auf sichere Weise verbinden.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Schritt 1: Untersuchen von ASP.NET 2.0 s geschützt, Konfigurationsoptionen

ASP.NET 2.0 umfasst eine geschützte Konfigurationssystem zum Verschlüsseln und Entschlüsseln von Konfigurationsinformationen. Dazu gehören Methoden in .NET Framework, die programmgesteuert ver-oder Entschlüsseln von Konfigurationsinformationen verwendet werden kann. Das System für die geschützte Konfiguration verwendet die [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), womit Entwickler aus, welche kryptografische Implementierung verwendet wird.

Im Lieferumfang von .NET Framework sind zwei geschützte Konfigurationsanbieter:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) – verwendet den asymmetrischen [RSA-Algorithmus](http://en.wikipedia.org/wiki/Rsa) für die Ver- und Entschlüsselung.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -verwendet die Windows [Data Protection API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) für die Ver- und Entschlüsselung.

Da das System die geschützte Konfiguration des Entwurfsmusters Anbieter implementiert, ist es möglich, einen eigenen Anbieter für die geschützte Konfiguration erstellen und binden diese in Ihrer Anwendung. Finden Sie unter [Implementieren eines Anbieters für die geschützte Konfiguration](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) für Weitere Informationen zu diesem Vorgang.

Die RSA und DPAPI-Anbieter verwenden die Schlüssel für ihren Routinen Ver- und Entschlüsselung, und dieser Schlüssel können auf dem Computer oder Benutzerebene gespeichert werden. Computerebene Schlüssel sich ideal für Szenarien sind, in denen die Webanwendung auf einem eigenen dedizierten Server ausgeführt wird, oder wenn es mehrere Anwendungen auf einem Server, die gemeinsam nutzen müssen verschlüsselte Informationen. Benutzerebene Schlüssel werden eine sicherere Option in der gemeinsam genutzten Hostingumgebungen, in dem andere Anwendungen auf dem gleichen Server nicht entschlüsseln Ihrer Anwendung s geschützt Konfigurationsabschnitte werden soll.

In diesem Lernprogramm werden unsere Beispiele der DPAPI-Anbieter und der Computerebene Schlüssel verwenden. Genauer gesagt betrachten wir Verschlüsseln der `<connectionStrings>` im Abschnitt `Web.config`, obwohl das System die geschützte Konfiguration verwendet werden kann, um die meisten alle verschlüsseln `Web.config` Abschnitt. Informationen auf Benutzerebene-Tasten verwenden oder mithilfe des RSA-Anbieters finden Sie in den Ressourcen im Abschnitt Weitere Messwerte am Ende dieses Lernprogramms.

> [!NOTE]
> Die `RSAProtectedConfigurationProvider` und `DPAPIProtectedConfigurationProvider` Anbieter registriert sind, der `machine.config` -Datei mit dem Anbieternamen `RsaProtectedConfigurationProvider` und `DataProtectionConfigurationProvider`bzw. Beim Verschlüsseln oder Entschlüsseln von Konfigurationsinformationen, die wir benötigen, geben Sie den Namen der entsprechenden Anbieter (`RsaProtectedConfigurationProvider` oder `DataProtectionConfigurationProvider`) und nicht den tatsächlichen Typnamen (`RSAProtectedConfigurationProvider` und `DPAPIProtectedConfigurationProvider`). Sie finden die `machine.config` in der Datei die `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` Ordner.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Schritt 2: Programmgesteuert zu verschlüsseln und Entschlüsseln von Konfigurationsabschnitte

Mit wenigen Codezeilen können wir verschlüsseln oder entschlüsseln einen bestimmten Konfigurationsabschnitt, der mithilfe eines angegebenen Anbieters. Muss der Code, siehe wir kurz, wird einfach den entsprechende Konfigurationsabschnitt programmgesteuert verweisen Aufrufen seiner `ProtectSection` oder `UnprotectSection` -Methode, und rufen Sie dann die `Save` Methode, um die Änderungen beibehalten werden. Darüber hinaus enthält das .NET Framework hilfreich Befehlszeilen-Hilfsprogramm, das Verschlüsseln und Entschlüsseln von Konfigurationsinformationen. Wir untersuchen diese Befehlszeilen-Hilfsprogramm in Schritt 3.

Um programmgesteuert schützen Konfigurationsinformationen zu veranschaulichen, Let s erstellen eine ASP.NET-Seite, die enthält Schaltflächen zum Verschlüsseln und Entschlüsseln der `<connectionStrings>` im Abschnitt `Web.config`.

Öffnen Sie zunächst die `EncryptingConfigSections.aspx` auf der Seite der `AdvancedDAL` Ordner. Ziehen Sie ein TextBox-Steuerelement aus der Toolbox in den Designer, Festlegen seiner `ID` Eigenschaft, um `WebConfigContents`, dessen `TextMode` Eigenschaft, um `MultiLine`, und die zugehörige `Width` und `Rows` zu 95 % und 15, Eigenschaften bzw. Dieses Textfeld-Steuerelement zeigt den Inhalt der `Web.config` ermöglicht uns schnell erkennen, wenn der Inhalt oder nicht verschlüsselt sind. Natürlich in einer realen Anwendung würde keine sollen zur Anzeige der Inhalte des `Web.config`.

Fügen Sie zwei Button-Steuerelementen, die mit dem Namen hinzu, unter dem Textfeld `EncryptConnStrings` und `DecryptConnStrings`. Legen Sie deren Eigenschaften Text auf Verbindungszeichenfolgen zu verschlüsseln und Entschlüsseln der Verbindungszeichenfolgen.

An diesem Punkt sollte Ihr Bildschirm ähnlich wie in Abbildung 2 aussehen.


[![Hinzufügen eines Textfelds und zwei Button-Web-Steuerelementen auf der Seite "](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Abbildung 2**: ein Textfeld und zwei Button-Web-Steuerelementen auf der Seite hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))


Wir als Nächstes Code zu schreiben, lädt und zeigt den Inhalt des `Web.config` in die `WebConfigContents` Textfeld aus, wenn die Seite erste wird geladen. Fügen Sie den folgenden Code auf der Seite s Code-Behind-Klasse. Dieser Code Fügt eine Methode namens `DisplayWebConfig` und ruft ihn von der `Page_Load` Ereignishandler bei `Page.IsPostBack` ist `False`:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

Die `DisplayWebConfig` -Methode verwendet die [ `File` Klasse](https://msdn.microsoft.com/library/system.io.file.aspx) , öffnen Sie die Anwendung s `Web.config` Datei, die [ `StreamReader` Klasse](https://msdn.microsoft.com/library/system.io.streamreader.aspx) zum Lesen von Inhalt in eine Zeichenfolge ein, und die [ `Path` Klasse](https://msdn.microsoft.com/library/system.io.path.aspx) auf den physischen Pfad zum Generieren der `Web.config` Datei. Diese drei Klassen alle befinden sich unter dem [ `System.IO` Namespace](https://msdn.microsoft.com/library/system.io.aspx). Sie müssen daher Hinzufügen einer `Imports``System.IO` Anweisung am Anfang der Code-Behind-Klasse oder alternativ diese Klassennamen mit Präfix `System.IO.`

Wir als Nächstes fügen Ereignishandler für die zwei Schaltflächen-Steuerelemente `Click` Ereignisse und fügen Sie den erforderlichen Code zum Verschlüsseln und Entschlüsseln der `<connectionStrings>` Abschnitt mithilfe einer Computerebene Schlüssels mit der DPAPI-Anbieter. Doppelklicken Sie im Designer auf die Schaltflächen Hinzufügen einer `Click` -Ereignishandler in der CodeBehind-Klasse, und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Der Code verwendet die zwei Ereignishandler ist nahezu identisch. Beide starten, indem das Abrufen von Informationen über die aktuelle Anwendung s `Web.config` Datei über die [ `WebConfigurationManager` Klasse](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` Methode](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Diese Methode gibt die Webkonfigurationsdatei für den angegebenen virtuellen Pfad zurück. Als Nächstes wird die `Web.config` Datei "s" `<connectionStrings>` Abschnitt erfolgt über die [ `Configuration` Klasse](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` Methode](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), welche gibt eine [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) Objekt.

Die `ConfigurationSection` Objekt enthält eine [ `SectionInformation` Eigenschaft](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) , zusätzliche Informationen und Funktionen zu den Konfigurationsabschnitt bereitstellt. Wie der Code oben zeigt, können wir feststellen, ob Konfigurationsabschnitts, überprüft verschlüsselt ist der `SectionInformation` Eigenschaft s `IsProtected` Eigenschaft. Darüber hinaus kann im Abschnitt verschlüsselt oder entschlüsselt, die über die `SectionInformation` Eigenschaft s `ProtectSection(provider)` und `UnprotectSection` Methoden.

Die `ProtectSection(provider)` Methode akzeptiert als Eingabe eine Zeichenfolge, die den Namen des Anbieters geschützte Konfiguration beim Verschlüsseln zu verwendende angibt. In der `EncryptConnString` Schaltfläche s-Ereignishandler DataProtectionConfigurationProvider in übergeben die `ProtectSection(provider)` Methode, damit der DPAPI-Anbieter verwendet wird. Die `UnprotectSection` -Methode kann den Anbieter, der wurde verwendet, um den Konfigurationsabschnitt zu verschlüsseln und daher nicht erfordert einen Eingabeparameter ermitteln.

Nach dem Aufruf der `ProtectSection(provider)` oder `UnprotectSection` -Methode, die Sie aufrufen müssen die `Configuration` s-Objekt [ `Save` Methode](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) damit die Änderungen beibehalten. Sobald die Konfigurationsinformationen verschlüsselt oder entschlüsselt wurde und die Änderungen gespeichert, wir nennen `DisplayWebConfig` beim Laden der aktualisierten `Web.config` Inhalt in das Textfeld-Steuerelement.

Wenn Sie den oben aufgeführten Code eingegeben haben, testen Sie sie durch Aufrufen der `EncryptingConfigSections.aspx` Seite über einen Browser. Daraufhin sollte zunächst eine Seite, die den Inhalt der listet `Web.config` mit der `<connectionStrings>` Abschnitt im nur-Text angezeigt (siehe Abbildung 3).


[![Hinzufügen eines Textfelds und zwei Button-Web-Steuerelementen auf der Seite "](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Abbildung 3**: ein Textfeld und zwei Button-Web-Steuerelementen auf der Seite hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))


Klicken Sie nun auf die Schaltfläche "Verschlüsseln von Verbindungszeichenfolgen". Wenn die Anforderungsvalidierung aktiviert ist, veröffentlicht das Markup wieder aus der `WebConfigContents` TextBox erzeugt ein `HttpRequestValidationException`, woraufhin die Nachricht, die eine potenziell gefährliche `Request.Form` Wert wurde vom Client erkannt. Request-Validierung in ASP.NET 2.0 standardmäßig aktiviert ist, verhindert die Postbacks, die nicht codiertes HTML enthalten und erleichtert die Skript-Injection-Angriffe zu verhindern. Diese Überprüfung kann auf der Seite oder Anwendungsebene deaktiviert werden. Für diese Seite deaktivieren, legen Sie die `ValidateRequest` auf `False` in der `@Page` Richtlinie. Die `@Page` -Direktive am oberen Rand der Seite "s" deklarativen Markup gefunden wird.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Weitere Informationen zur Anforderungsvalidierung von, der Aufgaben, wie auf der Seite "- und auf Anwendungsebene, wie sowie dazu, wie HTML-Markup Codierung zu deaktivieren, finden Sie unter [Anforderungsvalidierung - Skript-Angriffe verhindert](../../../../whitepapers/request-validation.md).

Nach dem Deaktivieren der Anforderungsvalidierung für die Seite, versuchen Sie erneut auf die Schaltfläche "Verschlüsseln von Verbindungszeichenfolgen" klicken. Beim Postback die Konfigurationsdatei zugegriffen wird und die zugehörige `<connectionStrings>` Abschnitt mit den DPAPI-Anbieter verschlüsselt. Das Textfeld wird dann aktualisiert, um die neue anzuzeigen `Web.config` Inhalt. Wie in Abbildung 4 gezeigt, die `<connectionStrings>` Informationen werden jetzt verschlüsselt.


[![Klicken Sie auf die verschlüsseln Verbindung Zeichenfolgen Schaltfläche verschlüsselt die &lt;"ConnectionString"&gt; Abschnitt](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Abbildung 4**: Klicken Sie auf die verschlüsseln Verbindung Zeichenfolgen Schaltfläche verschlüsselt die `<connectionString>` Abschnitt ([klicken Sie hier, um das Bild in voller Größe angezeigt](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))


Die verschlüsselte `<connectionStrings>` folgt, obwohl einige des Inhalts im Abschnitt auf meinem Computer generiert die `<CipherData>` Element wurde aus Gründen der Übersichtlichkeit entfernt:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> Die `<connectionStrings>` Element gibt den Anbieter verwendet, um die Verschlüsselung auszuführen (`DataProtectionConfigurationProvider`). Diese Informationen werden verwendet, indem Sie die `UnprotectSection` Methode, wenn die Verbindungszeichenfolgen entschlüsselt Schaltfläche geklickt wird.


Wenn die Verbindungszeichenfolgeninformationen zugegriffen wird `Web.config` - entweder indem wir schreiben, von einem SqlDataSource-Steuerelement oder die automatisch generierten Code von den TableAdaptern getrennt in unserer typisierter DataSets - wird automatisch entschlüsselt. Kurz gesagt, es müssen nicht zum Hinzufügen von zusätzlichen Code und keine zusätzliche Logik, um die verschlüsselte entschlüsseln `<connectionString>` Abschnitt. Um dies zu demonstrieren, finden Sie auf den früheren Tutorials zu diesem Zeitpunkt an, z. B. das einfache Anzeige Lernprogramm im Abschnitt "grundlegende Reporting" (`~/BasicReporting/SimpleDisplay.aspx`). Wie in Abbildung 5 gezeigt wird, funktioniert das Lernprogramm genau wie wir erwarten, gibt an, dass der verschlüsselte Verbindungszeichenfolgen-Informationen mit der ASP.NET-Seite automatisch entschlüsselt wird.


[![Die Datenzugriffsebene entschlüsselt automatisch die Verbindungszeichenfolgeninformationen.](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Abbildung 5**: die Datenzugriffsebene automatisch entschlüsselt, die Verbindungszeichenfolgeninformationen ([klicken Sie hier, um das Bild in voller Größe angezeigt](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))


Zurücksetzen der `<connectionStrings>` Abschnitt zurück in die nur-Text-Darstellung, klicken Sie auf die Schaltfläche mit den Verbindungszeichenfolgen entschlüsselt. Beim Postback sehen Sie die Verbindungszeichenfolgen im `Web.config` im nur-Text. An diesem Punkt sollte Ihren Bildschirm aussehen, wie es angezeigt, wenn zunächst diese Seite (siehe Abbildung 3) besuchen.

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Schritt 3: Verschlüsseln von Konfigurationsabschnitten mit`aspnet_regiis.exe`

.NET Framework enthält eine Reihe von Befehlszeilentools in den `$WINDOWS$\Microsoft.NET\Framework\version\` Ordner. In der [mithilfe von SQL-Cache-Abhängigkeiten](../caching-data/using-sql-cache-dependencies-vb.md) Lernprogramm, z. B. erläutert, mit der `aspnet_regsql.exe` -Befehlszeilentool, um die Infrastruktur für die SQL-Cache-Abhängigkeiten hinzuzufügen. Ein anderes nützlich Befehlszeilentool in diesem Ordner wird die [ASP.NET IIS Registration-Tool (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Wie der Name schon sagt, wird das ASP.NET IIS-Registrierungstool wird hauptsächlich verwendet, um eine ASP.NET 2.0-Anwendung mit Microsoft s professionelle Webserver IIS zu registrieren. Zusätzlich zu seiner IIS-bezogene Features, das ASP.NET IIS-Registrierungstool kann eingesetzt werden zum Verschlüsseln oder entschlüsseln angegebenen Konfigurationsabschnitten in `Web.config`.

Die folgende Anweisung zeigt die allgemeine Syntax zum Verschlüsseln von eines Konfigurationsabschnitts, mit der `aspnet_regiis.exe` -Befehlszeilentool:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*Abschnitt* Konfigurationsabschnitts verschlüsseln (z. B. ConnectionStrings), wird die *physischen\_Directory* ist der vollständige physische Pfad in das Stammverzeichnis der Anwendung s Web und *Anbieter*  ist der Name des Anbieters geschützte Konfiguration (z. B. DataProtectionConfigurationProvider) verwenden. Wenn die Webanwendung im IIS registriert ist können Sie den virtuellen Pfad statt den physischen Pfad mit der folgenden Syntax eingeben:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Die folgenden `aspnet_regiis.exe` Beispiel verschlüsselt der `<connectionStrings>` im Abschnitt mit den DPAPI-Anbieter mit einem Schlüssel auf Computerebene:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Auf ähnliche Weise die `aspnet_regiis.exe` -Befehlszeilentool kann verwendet werden, um Konfigurationsabschnitte zu entschlüsseln. Anstatt die `-pef` -Schalters `-pdf` (oder instead of `-pe`, verwenden Sie `-pd`). Beachten Sie, dass der Name des Anbieters nicht erforderlich, beim Entschlüsseln ist.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Da wir den DPAPI-Anbieter verwenden, der Schlüssel, die speziell für den Computer verwendet, müssen Sie ausführen `aspnet_regiis.exe` aus dem gleichen Computer, die aus dem Web Pages bedient werden. Z. B. Wenn Sie über die Befehlszeilenprogramm aus dem lokalen Entwicklungscomputer ausführen und anschließend die verschlüsselte Datei "Web.config" auf dem Produktionsserver hochladen, wird der Produktionsserver nicht die Verbindungszeichenfolgeninformationen zu entschlüsseln, da er verschlüsselt wurde Verwenden von Schlüsseln, die spezifisch für Ihren Entwicklungscomputer. RSA-Anbieters muss diese Einschränkung nicht, wie es möglich ist, die RSA-Schlüssel in einem anderen Computer exportieren.


## <a name="understanding-database-authentication-options"></a>Grundlegendes zu Datenbank-Authentifizierungsoptionen

Bevor eine Anwendung ausgeben kann `SELECT`, `INSERT`, `UPDATE`, oder `DELETE` Abfragen mit einer Microsoft SQL Server-Datenbank, die Datenbank müssen zuerst den anforderer identifizieren. Dieser Prozess wird als bezeichnet *Authentifizierung* und SQL Server bietet zwei Methoden der Authentifizierung:

- **Windows-Authentifizierung** – der Prozess, unter dem die Anwendung ausgeführt wird, für die Kommunikation mit der Datenbank verwendet. Wenn eine ASP.NET-Anwendung über Visual Studio 2005 s ASP.NET Development Server ausgeführt wird, wird die ASP.NET-Anwendung die Identität des aktuell angemeldeten Benutzers angenommen. Für ASP.NET-Anwendungen auf Microsoft Internet Information Server (IIS), die Identität des in der Regel von ASP.NET-Anwendungen angenommen `domainName``\MachineName` oder `domainName``\NETWORK SERVICE`, obwohl dies angepasst werden kann.
- **SQL-Authentifizierung** -eine Benutzer-ID und Kennwort-Werte werden als Anmeldeinformationen für die Authentifizierung bereitgestellt. Mit der SQL-Authentifizierung werden die Benutzer-ID und Kennwort in der Verbindungszeichenfolge bereitgestellt.

Windows-Authentifizierung wird über die SQL-Authentifizierung bevorzugt, da sie sicherer ist. Die Verbindungszeichenfolge ist mit Windows-Authentifizierung von Benutzername und Kennwort und den Webserver und Datenbankserver auf zwei unterschiedlichen Computern befinden, werden die Anmeldeinformationen nicht im nur-Text über das Netzwerk gesendet. Mit der SQL-Authentifizierung jedoch Anmeldeinformationen für die Authentifizierung in der Verbindungszeichenfolge hartcodiert sind, und werden vom Webserver an den Datenbankserver im nur-Text übertragen.

Diese Lernprogramme sind Windows-Authentifizierung verwendet. Sie können feststellen, welcher Authentifizierungsmodus durch Überprüfen der Verbindungszeichenfolge verwendet wird. Die Verbindungszeichenfolge in `Web.config` für unseren Tutorials wurde:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Die integrierte Sicherheit = "true" und des Benutzernamens und Kennworts anzugeben, dass die Windows-Authentifizierung verwendet wird. In einigen Verbindung Zeichenfolgen, die den Begriff vertrauenswürdige Verbindung = Yes "oder" integrierte Sicherheit = SSPI verwendet, anstatt Sie zu integrierte Sicherheit = "true", aber alle drei an, die Verwendung der Windows-Authentifizierung.

Das folgende Beispiel zeigt eine Verbindungszeichenfolge, die SQL-Authentifizierung verwendet. Beachten Sie die Anmeldeinformationen in der Verbindungszeichenfolge eingebettet:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Stellen Sie sich vor, dass ein Angreifer Ihre Anwendung s anzeigen kann `Web.config` Datei. Wenn Sie SQL-Authentifizierung verwenden, um mit einer Datenbank herzustellen, die über das Internet zugänglich ist, kann der Angreifer diese Verbindungszeichenfolge verwenden, um mit Ihrer Datenbank über SQL Management Studio oder von ASP.NET-Seiten auf eigene Website herstellen. Um diese Bedrohung zu verringern, verschlüsseln Sie die Verbindungszeichenfolgeninformationen in `Web.config` mit der geschützten Konfiguration.

> [!NOTE]
> Weitere Informationen über die verschiedenen Arten der Authentifizierung in SQL Server verfügbaren finden Sie unter [Building Secure ASP.NET Applications: Authentifizierung, Autorisierung und sichere Kommunikation](https://msdn.microsoft.com/library/aa302392.aspx). Weitere Beispiele für Verbindungszeichenfolgen zur Veranschaulichung der Unterschiede zwischen Windows- und SQL-Authentifizierung-Syntax, finden Sie unter [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Zusammenfassung

Standardmäßig Dateien mit einer `.config` Erweiterung in einer ASP.NET-Anwendung kann nicht über einen Browser zugegriffen werden. Diese Dateitypen werden nicht zurückgegeben werden, da sie möglicherweise vertraulichen Informationen wie Datenbankverbindungszeichenfolgen, Benutzernamen und Kennwörter, usw. enthalten. Das System die geschützte Konfiguration in .NET 2.0 schützt Weitere vertraulichen Informationen angegebenen Konfigurationsabschnitte verschlüsselt werden können. Es gibt zwei integrierte geschützte Konfigurationsanbieter: eines, das den RSA-Algorithmus und die andere die Windows-Datenschutz-API (DPAPI verwendet) verwendet.

In diesem Lernprogramm erläutert wir, wie zum Verschlüsseln und Entschlüsseln von Konfigurationseinstellungen mithilfe des DPAPI-Anbieters. Dies geschieht sowohl programmgesteuert ausführen, wie in Schritt 2 wurde erläutert, sowie über die `aspnet_regiis.exe` -Befehlszeilentool, das in Schritt 3 behandelt wurde. Weitere Informationen auf Benutzerebene-Tasten verwenden, oder verwenden Sie stattdessen die RSA-Anbieters finden Sie in den Ressourcen im Abschnitt Weitere Themen.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Erstellen von sicheren ASP.NET-Anwendung: Authentifizierung, Autorisierung und sichere Kommunikation](https://msdn.microsoft.com/library/aa302392.aspx)
- [Verschlüsseln von Konfigurationsinformationen in ASP.NET 2.0 Anwendungen](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Verschlüsseln von `Web.config` Werte in ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Gewusst wie: Verschlüsseln von Konfigurationsabschnitten in ASP.NET 2.0 mit DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Gewusst wie: Verschlüsseln von Konfigurationsabschnitten in ASP.NET 2.0 mit RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [Der Konfigurations-API in .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Windows Data Protection](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Teresa Murphy und Randy Schmidt. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [Weiter](debugging-stored-procedures-vb.md)
