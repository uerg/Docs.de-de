---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: Schützen von Verbindungszeichenfolgen und anderen Konfigurationsinformationen (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Eine ASP.NET-Anwendung speichert die Konfigurationsinformationen in der Regel in einer Datei "Web.config". Einige dieser Informationen ist vertraulich und Schutz erfordert. Von "def"...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 61ac52ffe28762ce0cf8892621343f71e73a9ca7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836195"
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>Schützen von Verbindungszeichenfolgen und anderen Konfigurationsinformationen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) oder [PDF-Datei herunterladen](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Eine ASP.NET-Anwendung speichert die Konfigurationsinformationen in der Regel in einer Datei "Web.config". Einige dieser Informationen ist vertraulich und Schutz erfordert. Standardmäßig wird diese Datei kein Besucher der Website bedient werden, aber ein Administrator oder ein Hacker kann, erhalten Sie Zugang zum Dateisystem des Webservers, und zeigen Sie den Inhalt der Datei. In diesem Tutorial erfahren Sie, dass ASP.NET 2.0 zum Schutz sensibler Informationen verschlüsselt Abschnitte der Web.config-Datei ermöglicht.


## <a name="introduction"></a>Einführung

Konfigurationsinformationen für ASP.NET-Anwendungen befindet sich im Allgemeinen in eine XML-Datei mit dem Namen `Web.config`. Wir haben im Verlauf des in diesen Tutorials aktualisiert die `Web.config` wenige Male. Beim Erstellen der `Northwind` typisierte DataSet in den [ersten Tutorial](../introduction/creating-a-data-access-layer-cs.md), z. B. Informationen zur Verbindungszeichenfolge automatisch hinzugefügt wurde `Web.config` in die `<connectionStrings>` Abschnitt. Weiter unten in der [Masterseiten und Sitenavigation](../introduction/master-pages-and-site-navigation-cs.md) Tutorial wir manuell aktualisiert `Web.config`, Hinzufügen einer `<pages>` Element gibt an, dass alle die ASP.NET-Seiten in unserem Projekt verwenden, sollten die `DataWebControls` Design.

Da `Web.config` kann vertrauliche Daten wie Verbindungszeichenfolgen enthalten es ist wichtig, die den Inhalt der `Web.config` gehalten werden, sicher und nicht autorisierte Viewer ausgeblendet. In der Standardeinstellung alle HTTP-Anforderung an eine Datei mit der `.config` Erweiterung erfolgt durch die Engine für ASP.NET, wodurch die *dieser Seite wird nicht verarbeitet* Nachricht, die in Abbildung 1 dargestellt. Dies bedeutet, dass Besucher nicht einsehen dürfen Ihre `Web.config` s Dateiinhalt einfach durch Eingabe der http://www.YourServer.com/Web.config in die Adressleiste des Browsers s.


[![Besuchen die Datei "Web.config" durch ein Browser gibt einen Typ der Seite wird keine Nachricht bereitgestellt](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Abbildung 1**: Zugriff auf `Web.config` über ein Browser gibt einen Typ der Seite wird die Nachricht nicht verarbeitet ([klicken Sie, um das Bild in voller Größe anzeigen](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


Was geschieht, wenn ein Angreifer kann jedoch einige andere Exploit zu suchen, die sie anzeigen können Ihre `Web.config` s Dateiinhalte? Was könnte ein Angreifer tun, mit diesen Informationen, und welche Schritte kann erstellt werden, die vertrauliche Informationen in weiter zu schützen `Web.config`? Glücklicherweise die meisten Abschnitte `Web.config` enthalten keine vertraulichen Informationen. Welchen Schaden kann ein Angreifer begehen, wenn sie wissen, dass der Name der Standard-Design von ASP.NET-Seiten verwendet?

Bestimmte `Web.config` Abschnitte, allerdings enthalten vertrauliche Informationen, die Verbindungszeichenfolgen, Benutzernamen, Kennwörter, Servernamen, Verschlüsselungsschlüssel, usw. enthalten können. Diese Informationen finden Sie in der Regel in der folgenden `Web.config` Abschnitte:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

In diesem Tutorial werden wir die Verfahren zum Schützen von solche vertraulichen Konfigurationsinformationen betrachten. Wie wir sehen, enthält .NET Framework, Version 2.0 ein geschützte Konfigurationen-System, das programmgesteuerte verschlüsseln und Entschlüsseln der ausgewählten Konfigurationsabschnitte zu einem Kinderspiel macht.

> [!NOTE]
> Dieses Lernprogramm schließt sich mit einem Blick auf Microsoft-s-Empfehlungen für die Verbindung mit einer Datenbank aus einer ASP.NET-Anwendung. Zusätzlich zur Verschlüsselung von Verbindungszeichenfolgen, kann Ihnen helfen, Ihr System Leuchten absichern, das Sie mit der Datenbank auf sichere Weise verbunden werden.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Schritt 1: Untersuchen von ASP.NET 2.0 s geschützt, Konfigurationsoptionen

ASP.NET 2.0 umfasst eine geschützte Konfigurationssystem zum Verschlüsseln und Entschlüsseln von Konfigurationsinformationen. Dazu gehören Methoden in .NET Framework, die zum programmgesteuerten verschlüsseln oder entschlüsseln Konfigurationsinformationen verwendet werden kann. Das System für die geschützte Konfiguration verwendet die [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), wodurch Entwicklern die Auswahl, welche kryptografische Implementierung, die verwendet wird.

Im Lieferumfang von .NET Framework sind zwei geschützte Konfigurationsanbieter:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -wird verwendet, den asymmetrischen [RSA-Algorithmus](http://en.wikipedia.org/wiki/Rsa) für die Ver- und Entschlüsselung.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) – verwendet die Windows [Data Protection API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) für die Ver- und Entschlüsselung.

Da das System für die geschützte Konfiguration der Provider-Entwurfsmuster implementiert, ist es möglich, Ihren eigenen Anbieter für die geschützte Konfiguration erstellen und ihn in Ihrer Anwendung. Finden Sie unter [Implementieren eines geschützten Konfigurationsanbieters](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) für Weitere Informationen zu diesem Prozess.

Diese Schlüssel können auf den Computer oder Benutzer-Ebenen gespeichert werden, und verwenden die RSA und DPAPI-Anbieter für die Ver- und Entschlüsselung Routinen. Computerebene Schlüssel sich ideal für Szenarien, in dem die Webanwendung auf einem eigenen dedizierten Server ausgeführt wird, oder wenn es mehrere Anwendungen auf einem Server, die gemeinsam nutzen müssen verschlüsselte Informationen. Auf Benutzerebene Schlüssel sind eine sicherere Option in gemeinsamen hosting-Umgebungen, in dem andere Anwendungen auf dem gleichen Server nicht entschlüsseln die Konfigurationsabschnitte Ihrer Anwendung s geschützt werden soll.

In diesem Tutorial werden unsere Beispiele der DPAPI-Anbieter und der Computerebene Schlüssel verwenden. Genauer gesagt betrachten wir verschlüsseln die `<connectionStrings>` im Abschnitt `Web.config`, obwohl das System für die geschützte Konfiguration verwendet werden kann, um die meisten alle verschlüsseln `Web.config` Abschnitt. Informationen auf Benutzerebene-Schlüssel oder mithilfe von RSA-Anbieters finden Sie in den Ressourcen im Abschnitt Weitere Messwerte am Ende dieses Tutorials.

> [!NOTE]
> Die `RSAProtectedConfigurationProvider` und `DPAPIProtectedConfigurationProvider` Anbieter werden registriert, der `machine.config` -Datei mit dem Anbieternamen `RsaProtectedConfigurationProvider` und `DataProtectionConfigurationProvider`bzw. Beim Verschlüsseln oder Entschlüsseln von Konfigurationsinformationen, die wir benötigen den Namen des jeweiligen Anbieters angeben (`RsaProtectedConfigurationProvider` oder `DataProtectionConfigurationProvider`) anstelle der tatsächlichen Typnamen (`RSAProtectedConfigurationProvider` und `DPAPIProtectedConfigurationProvider`). Sie finden die `machine.config` Datei der `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` Ordner.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Schritt 2: Programmgesteuert zu ver- und Entschlüsselung Konfigurationsabschnitte

Mit wenigen Codezeilen können wir verschlüsseln oder entschlüsseln einen bestimmtes Konfigurationsabschnitt, der mithilfe eines angegebenen Anbieters. Der Code, wie wir gleich sehen werden einfach muss der entsprechende Konfigurationsabschnitt, programmgesteuert auf Aufrufen der `ProtectSection` oder `UnprotectSection` -Methode, und rufen Sie dann die `Save` Methode, um die Änderungen beibehalten werden. Darüber hinaus enthält das .NET Framework eine hilfreiche Befehlszeilen-Hilfsprogramm, das Verschlüsseln und Entschlüsseln von Konfigurationsinformationen zu kann. Wir untersuchen diese Befehlszeilen-Hilfsprogramm in Schritt 3.

Um programmgesteuert den Schutz von Konfigurationsinformationen zu veranschaulichen, erstellen können s eine ASP.NET-Seite, die enthält Schaltflächen zum Verschlüsseln und Entschlüsseln der `<connectionStrings>` im Abschnitt `Web.config`.

Öffnen Sie zunächst die `EncryptingConfigSections.aspx` auf der Seite die `AdvancedDAL` Ordner. Ziehen Sie ein TextBox-Steuerelement aus der Toolbox auf den Designer, Festlegen der `ID` Eigenschaft `WebConfigContents`, dessen `TextMode` Eigenschaft, um `MultiLine`, und die zugehörige `Width` und `Rows` Eigenschaften zu 95 % und 15 bzw. Dieses Textfeld-Steuerelement zeigt den Inhalt der `Web.config` es uns ermöglicht, schnell erkennen, wenn der Inhalt oder nicht verschlüsselt werden. Natürlich in einer echten Anwendung nie möchten den Inhalt des anzeigen `Web.config`.

Fügen Sie unterhalb im Textfeld für die zwei Schaltflächen-Steuerelemente, die mit dem Namen `EncryptConnStrings` und `DecryptConnStrings`. Legen Sie deren Eigenschaften Text, auf die Verbindungszeichenfolgen zu verschlüsseln und Entschlüsseln der Verbindungszeichenfolgen.

An diesem Punkt werden Ihr Bildschirm sollte ähnlich wie in Abbildung 2 aussehen.


[![Fügen Sie einem Textfeld und zwei Schaltflächen-Web-Steuerelemente auf der Seite](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Abbildung 2**: Fügen Sie ein Textfeld und zwei Schaltflächen-Web-Steuerelemente auf der Seite ([klicken Sie, um das Bild in voller Größe anzeigen](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


Als Nächstes müssen wir Code schreiben, lädt und zeigt den Inhalt der `Web.config` in die `WebConfigContents` Textfeld aus, wenn die Seite erstmalig geladen. Fügen Sie den folgenden Code, auf der Seite s Code-Behind-Klasse. Dieser Code Fügt eine Methode namens `DisplayWebConfig` und ruft aus der `Page_Load` -Ereignishandler bei `Page.IsPostBack` ist `false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

Die `DisplayWebConfig` -Methode verwendet die [ `File` Klasse](https://msdn.microsoft.com/library/system.io.file.aspx) , öffnen Sie die Anwendung s `Web.config` -Datei, die [ `StreamReader` Klasse](https://msdn.microsoft.com/library/system.io.streamreader.aspx) , lesen seinen Inhalt in eine Zeichenfolge ein, und die [ `Path` Klasse](https://msdn.microsoft.com/library/system.io.path.aspx) zum Generieren des physischen Pfads der `Web.config` Datei. Diese drei Klassen sind alle finden Sie der [ `System.IO` Namespace](https://msdn.microsoft.com/library/system.io.aspx). Sie müssen daher Hinzufügen einer `using` `System.IO` Anweisung am Anfang der CodeBehind-Klasse oder alternativ diese Klasse Namen mit Präfix `System.IO.` .

Als Nächstes müssen wir Ereignishandler für die zwei Schaltflächen-Steuerelemente hinzufügen `Click` Ereignisse und fügen Sie den erforderlichen Code zum Verschlüsseln und Entschlüsseln der `<connectionStrings>` im Abschnitt mit einem auf Computerebene-Schlüssel mit der DPAPI-Anbieter. Doppelklicken Sie im Designer auf die Schaltflächen zum Hinzufügen einer `Click` -Ereignishandler in der CodeBehind-Klasse, und fügen Sie den folgenden Code:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

Der Code in die beiden Ereignishandler verwendet, ist nahezu identisch. Beide Abrufen von Informationen über die aktuelle Anwendung s zunächst `Web.config` Datei über die [ `WebConfigurationManager` Klasse](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` Methode](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Diese Methode gibt die Webkonfigurationsdatei für den angegebenen virtuellen Pfad zurück. Als Nächstes die `Web.config` Datei s `<connectionStrings>` Abschnitt erfolgt über die [ `Configuration` Klasse](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` Methode](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), gibt eine [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) Objekt.

Die `ConfigurationSection` Objekt enthält eine [ `SectionInformation` Eigenschaft](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) , der zusätzliche Informationen und Funktionen in Bezug auf den Konfigurationsabschnitt bereitstellt. Wie der Code oben zeigt, können wir bestimmen, ob der Konfigurationsabschnitt, anhand verschlüsselt ist der `SectionInformation` Eigenschaft s `IsProtected` Eigenschaft. Darüber hinaus im Abschnitt verschlüsselt oder über entschlüsselt werden kann die `SectionInformation` Eigenschaft s `ProtectSection(provider)` und `UnprotectSection` Methoden.

Die `ProtectSection(provider)` Methode akzeptiert als Eingabe eine Zeichenfolge, die den Namen der der geschützte Konfigurationsanbieter zu verwenden, bei der Verschlüsselung angibt. In der `EncryptConnString` Schaltfläche "s"-Ereignishandler wir DataProtectionConfigurationProvider in übergeben die `ProtectSection(provider)` Methode, damit der DPAPI-Anbieter verwendet wird. Die `UnprotectSection` -Methode kann den Anbieter, der wurde verwendet, um den Konfigurationsabschnitt zu verschlüsseln und aus diesem Grund erfordert keine alle benötigten Eingabeparameter ermitteln.

Nach dem Aufruf der `ProtectSection(provider)` oder `UnprotectSection` -Methode, die Sie aufrufen müssen die `Configuration` s-Objekt [ `Save` Methode](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) , die Änderungen beizubehalten. Sobald die Konfigurationsinformationen verschlüsselt oder entschlüsselt wurde und die Änderungen gespeichert, wir rufen `DisplayWebConfig` Laden Sie die aktualisierte `Web.config` Inhalt in TextBox-Steuerelement.

Wenn Sie den obigen Code eingegeben haben, testen, indem Sie auf die `EncryptingConfigSections.aspx` Seite über einen Browser. Zunächst sollte eine Seite, die der Inhalt des aufgelistet `Web.config` mit der `<connectionStrings>` Abschnitt im nur-Text angezeigt (siehe Abbildung 3).


[![Fügen Sie einem Textfeld und zwei Schaltflächen-Web-Steuerelemente auf der Seite](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Abbildung 3**: Fügen Sie ein Textfeld und zwei Schaltflächen-Web-Steuerelemente auf der Seite ([klicken Sie, um das Bild in voller Größe anzeigen](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


Klicken Sie nun auf die Schaltfläche "Verschlüsseln von Verbindungszeichenfolgen". Wenn die Anforderungsvalidierung aktiviert ist, wird das Markup bereitgestellt, von der `WebConfigContents` Textfeld erzeugt eine `HttpRequestValidationException`, woraufhin die Nachricht, die eine potenziell gefährliche `Request.Form` Wert wurde vom Client erkannt. Request-Überprüfung, die in ASP.NET 2.0 standardmäßig aktiviert ist, verhindert, dass Postbacks, die nicht codierten HTML enthalten und soll helfen, Script-Injection Angriffe zu verhindern. Diese Überprüfung kann auf der Seite oder Anwendungsebene deaktiviert werden. Für diese Seite deaktivieren, legen Sie die `ValidateRequest` auf `false` in die `@Page` Richtlinie. Die `@Page` Richtlinie befindet sich oben auf der Seite s deklarativen Markup.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

Weitere Informationen zur Anforderungsvalidierung, den Zweck, zum Deaktivieren auf der Seite - und auf Anwendungsebene, wie und wie HTML-Markup Codierung finden Sie unter [Anforderungsvalidierung - Skript-Angriffe verhindern](../../../../whitepapers/request-validation.md).

Versuchen Sie nach der Deaktivierung Anforderungsvalidierung für die Seite erneut auf die Schaltfläche "Verschlüsseln von Verbindungszeichenfolgen" klicken. Beim Postback die Konfigurationsdatei zugegriffen wird und die zugehörige `<connectionStrings>` Abschnitt mit der DPAPI-Anbieter verschlüsselt. Das Textfeld wird dann aktualisiert, um die neue anzuzeigen `Web.config` Inhalt. Wie in Abbildung 4 gezeigt, die `<connectionStrings>` Informationen werden jetzt verschlüsselt.


[![Klicken Sie auf die verschlüsseln Verbindung Zeichenfolgen Schaltfläche verschlüsselt die &lt;"ConnectionString"&gt; Abschnitt](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Abbildung 4**: Klicken Sie auf die verschlüsseln Verbindung Zeichenfolgen Schaltfläche verschlüsselt die `<connectionString>` Abschnitt ([klicken Sie, um das Bild in voller Größe anzeigen](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


Die verschlüsselten `<connectionStrings>` auf meinem Computer generiert Abschnitt folgt, obwohl einige Inhalte in der `<CipherData>` Element wurde aus Gründen der Übersichtlichkeit entfernt:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> Die `<connectionStrings>` Element gibt an, der zum Durchführen der Verschlüsselung verwendete Anbieter (`DataProtectionConfigurationProvider`). Diese Informationen werden verwendet, durch die `UnprotectSection` Methode, wenn die Verbindungszeichenfolgen entschlüsselt-Schaltfläche geklickt wird.


Wenn die Informationen zur Verbindungszeichenfolge aus erfolgt `Web.config` – entweder durch Code, die wir schreiben, von einem SqlDataSource-Steuerelement, oder die automatisch generierten Code von den TableAdaptern getrennt in unserer typisierte DataSets - werden automatisch entschlüsselt. Kurz gesagt: Wir müssen keine zusätzlichen Code oder Logik, um die verschlüsselten entschlüsseln hinzufügen `<connectionString>` Abschnitt. Um dies zu veranschaulichen, finden Sie auf den früheren Tutorials zu diesem Zeitpunkt an, wie z. B. das einfache Tutorial das Kapitel zur grundlegenden Berichterstellung (`~/BasicReporting/SimpleDisplay.aspx`). Wie in Abbildung 5 gezeigt, funktioniert das Lernprogramm, wie wir erwarten, gibt an, dass die verschlüsselten Verbindungsinformationen für die Zeichenfolge mit der ASP.NET-Seite automatisch entschlüsselt wird.


[![Die Datenzugriffsebene entschlüsselt automatisch Informationen zur Verbindungszeichenfolge](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Abbildung 5**: die Datenzugriffsebene automatisch entschlüsselt, die Informationen zur Verbindungszeichenfolge ([klicken Sie, um das Bild in voller Größe anzeigen](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


Wiederherstellen der `<connectionStrings>` Abschnitt zurück in die nur-Text-Darstellung, klicken Sie auf die Schaltfläche mit den Verbindungszeichenfolgen entschlüsselt. Beim Postback sollte die Verbindungszeichenfolgen im `Web.config` im nur-Text. An diesem Punkt sollte Ihr Bildschirm sieht wie beim ersten Zugriff auf dieser Seite (siehe Abbildung 3) auf.

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Schritt 3: Verschlüsseln von Konfigurationsabschnitten mit`aspnet_regiis.exe`

.NET Framework enthält eine Reihe von Befehlszeilentools in die `$WINDOWS$\Microsoft.NET\Framework\version\` Ordner. In der [mithilfe von SQL-Cacheabhängigkeiten](../caching-data/using-sql-cache-dependencies-cs.md) Tutorial, z. B. erläutert, mit der `aspnet_regsql.exe` -Befehlszeilentool, um die Infrastruktur für die SQL-cacheabhängigkeiten hinzuzufügen. Eine andere nützliche-Befehlszeilentools in diesem Ordner wird die [ASP.NET IIS Registration-Tool (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Wie der Name schon sagt, wird in erster Linie das ASP.NET IIS-Registrierungstool verwendet, um eine ASP.NET 2.0-Anwendung mit Microsoft s professionelle Web-Server, IIS zu registrieren. Zusätzlich zu der IIS-bezogene Features, das ASP.NET IIS-Registrierungstool kann auch der informationsfindung zum Verschlüsseln oder Entschlüsseln der angegebenen Konfigurationsabschnitten in `Web.config`.

Die folgende Anweisung zeigt die allgemeine Syntax verwendet, um einen Konfigurationsabschnitt mit verschlüsseln die `aspnet_regiis.exe` -Befehlszeilentools:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*Abschnitt* ist der Konfigurationsabschnitt "zum Verschlüsseln (z. B. ConnectionStrings), die *physischen\_Directory* ist der vollständige physische Pfad in das Stammverzeichnis der Web-Anwendung s und *Anbieter*  ist der Name der der geschützte Konfigurationsanbieter (z. B. DataProtectionConfigurationProvider). Wenn die Webanwendung in IIS registriert wird können Sie alternativ den virtuellen Pfad anstelle des physischen Pfads mit der folgenden Syntax eingeben:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

Die folgenden `aspnet_regiis.exe` Beispiel verschlüsselt der `<connectionStrings>` im Abschnitt mit der DPAPI-Anbieter mit einem auf Computerebene-Schlüssel:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

Auf ähnliche Weise die `aspnet_regiis.exe` Befehlszeilentool kann verwendet werden, um die Konfigurationsabschnitte zu entschlüsseln. Anstatt die `-pef` wechseln, verwenden Sie `-pdf` (anstelle von oder `-pe`, verwenden Sie `-pd`). Beachten Sie, dass der Name des Anbieters nicht erforderlich, beim Entschlüsseln ist.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Da wir den DPAPI-Anbieter verwenden, die Schlüssel, die speziell für den Computer verwendet, müssen Sie ausführen `aspnet_regiis.exe` aus dem gleichen Computer aus dem Web Pages verarbeitet werden. Z. B. Wenn Sie dieses Programm über die Befehlszeile aus Ihrem lokalen Entwicklungscomputer ausführen, und klicken Sie dann die verschlüsselte Datei "Web.config" auf dem Produktionsserver hochladen, wird der Produktionsserver nicht die Verbindungszeichenfolgeninformationen zu entschlüsseln, da er verschlüsselt wurde verwenden die Schlüssel, die spezifisch für Ihren Entwicklungscomputer. RSA-Anbieters muss diese Einschränkung nicht, wie es möglich ist, die RSA-Schlüssel in einem anderen Computer zu exportieren.


## <a name="understanding-database-authentication-options"></a>Grundlegendes zu Datenbank-Authentifizierungsoptionen

Bevor eine Anwendung ausgeben kann `SELECT`, `INSERT`, `UPDATE`, oder `DELETE` Abfragen zu einer Microsoft SQL Server-Datenbank, die Datenbank müssen zuerst den anforderer identifizieren. Dieser Prozess wird als bezeichnet *Authentifizierung* und SQL Server bietet zwei Methoden der Authentifizierung:

- **Windows-Authentifizierung** – der Prozess, unter dem die Anwendung ausgeführt wird, für die Kommunikation mit der Datenbank verwendet. Wenn eine ASP.NET-Anwendung über Visual Studio 2005-s ASP.NET Development Server ausgeführt wird, nimmt die ASP.NET-Anwendung die Identität des aktuell angemeldeten Benutzers an. Für ASP.NET-Anwendungen auf Microsoft Internet Information Server (IIS), die Identität des in der Regel von ASP.NET-Anwendungen angenommen `domainName``\MachineName` oder `domainName``\NETWORK SERVICE`, obwohl dies angepasst werden kann.
- **SQL-Authentifizierung** -eine Benutzer-ID und Kennwort angegeben werden als Anmeldeinformationen für die Authentifizierung. Mit SQL-Authentifizierung werden Benutzer-ID und Kennwort in der Verbindungszeichenfolge bereitgestellt.

Windows-Authentifizierung wird über SQL-Authentifizierung bevorzugt, da sie sicherer ist. Mit der Windows-Authentifizierung kann die Verbindungszeichenfolge aus Benutzername und Kennwort, und wenn sich der Webserver und Datenbankserver auf zwei unterschiedlichen Computern befinden, werden die Anmeldeinformationen nicht über das Netzwerk im nur-Text gesendet. Mit der SQL-Authentifizierung jedoch Anmeldeinformationen für die Authentifizierung in der Verbindungszeichenfolge hartcodiert sind, und werden vom Webserver an den Datenbankserver im nur-Text übertragen.

In diesen Tutorials haben Windows-Authentifizierung verwendet. Sie können feststellen, welcher Authentifizierungsmodus verwendet wird, indem Sie die Verbindungszeichenfolge zu überprüfen. Die Verbindungszeichenfolge in `Web.config` für die Lernprogramme wurde:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Die integrierte Sicherheit = "true" und der Mangel an einen Benutzernamen und ein Kennwort anzugeben, dass es sich bei Windows-Authentifizierung verwendet wird. In einigen Verbindung Zeichenfolgen den Begriff vertrauenswürdige Verbindung = Yes "oder" integrierte Sicherheit = SSPI verwendet, anstatt die integrierte Sicherheit = "true", aber alle drei an, die Verwendung der Windows-Authentifizierung.

Das folgende Beispiel zeigt eine Verbindungszeichenfolge, die SQL-Authentifizierung verwendet. Beachten Sie die Anmeldeinformationen in der Verbindungszeichenfolge eingebettet:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Stellen Sie sich vor, dass ein Angreifer Ihre Anwendung s anzeigen kann `Web.config` Datei. Wenn Sie SQL-Authentifizierung verwenden, die Verbindung mit einer Datenbank, die über das Internet zugänglich ist, kann der Angreifer diese Verbindungszeichenfolge verwenden, für die Verbindung mit Ihrer Datenbank über SQL Management Studio oder von ASP.NET-Seiten auf ihrer eigenen Website. Um diese Bedrohung zu verringern, verschlüsselt die Verbindungszeichenfolgeninformationen in `Web.config` mit der geschützten Konfiguration.

> [!NOTE]
> Weitere Informationen zu den verschiedenen Arten der Authentifizierung in SQL Server verfügbar sind, finden Sie unter [Building Secure ASP.NET Applications: Authentifizierung, Autorisierung und sichere Kommunikation](https://msdn.microsoft.com/library/aa302392.aspx). Weitere Beispiele für Verbindungszeichenfolgen zur Veranschaulichung der Unterschiede zwischen Windows und SQL-Authentifizierung-Syntax, finden Sie unter [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Zusammenfassung

Standardmäßig sind Dateien mit einem `.config` Erweiterung in einer ASP.NET-Anwendung kann nicht über einen Browser zugegriffen werden. Diese Dateitypen werden nicht zurückgegeben werden, da sie möglicherweise vertrauliche Informationen wie Datenbankverbindungszeichenfolgen, Benutzernamen und Kennwörtern, usw. enthalten. Das System die geschützte Konfiguration in .NET 2.0 kann sensible Informationen weiter zu schützen, durch den angegebenen Konfigurationsabschnitten verschlüsselt werden können. Es gibt zwei integrierte geschützte Konfigurationsanbieter: verwendet den RSA-Algorithmus und die Windows Data Protection API (DPAPI) verwendet.

In diesem Tutorial erläutert, wie Sie zum Verschlüsseln und Entschlüsseln von Konfigurationseinstellungen mithilfe des DPAPI-Anbieters. Dies kann erreicht werden sowohl programmgesteuert, wie in Schritt 2 beschrieben, sowie über die `aspnet_regiis.exe` -Befehlszeilentool, das in Schritt 3 erläutert wurde. Weitere Informationen auf Benutzerebene-Schlüssel, oder verwenden Sie stattdessen die RSA-Anbieters finden Sie unter den Ressourcen im Abschnitt Weitere nützliche Informationen.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Erstellen von sicheren ASP.NET-Anwendung: Authentifizierung, Autorisierung und sichere Kommunikation](https://msdn.microsoft.com/library/aa302392.aspx)
- [Verschlüsseln von Konfigurationsinformationen in ASP.NET 2.0 Applications](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Verschlüsseln von `Web.config` Werte in ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Gewusst wie: Verschlüsseln von Konfigurationsabschnitten in ASP.NET 2.0 mithilfe von DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Gewusst wie: Verschlüsseln von Konfigurationsabschnitten in ASP.NET 2.0 mithilfe von RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [Der Konfigurations-API in .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Windows-Datenschutz](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Teresa Murphy und Randy Schmidt. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
> [Weiter](debugging-stored-procedures-cs.md)
