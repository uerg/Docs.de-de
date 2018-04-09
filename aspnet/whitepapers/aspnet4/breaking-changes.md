---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 Lauffähigkeit | Microsoft Docs
author: rick-anderson
description: Dieses Dokument beschreibt die Änderungen, die für die Version von .NET Framework 4-Version vorgenommen wurden, die Anwendungen beeinträchtigen können, die mit erstellt wurden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 7eea51add6b05684357314e3d6aa5087383c6408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 wichtige Änderungen
====================
> Dieses Dokument beschreibt die Änderungen, die für die Version von .NET Framework 4-Version vorgenommen wurden, die Anwendungen beeinträchtigen können, die mit früheren Versionen, einschließlich der ASP.NET 4 Beta 1 und Beta 2-Versionen erstellt wurden.
> 
> [In diesem Whitepaper herunterladen](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Inhalt

[ControlRenderingCompatibilityVersion-Einstellung in der Datei "Web.config"](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode Changes](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode und UrlEncode jetzt codieren Sie einfache Anführungszeichen](#0.1__Toc256770143 "_Toc256770143")  
[ASP.NET-Seite (.aspx) Parser ist Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Browser-Definitionsdateien aktualisiert](#0.1__Toc256770145 "_Toc256770145")  
["System.Web.Mobile.dll" daraus Stamm Webkonfigurationsdatei](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET Anforderungsvalidierung](#0.1__Toc256770147 "_Toc256770147")  
[Hashalgorithmus ist standardmäßig jetzt HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Fehler beim Konfigurieren des im Zusammenhang mit der neuen ASP.NET 4-Stammkonfiguration](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET-Anwendungen für 4 untergeordnete Fehler beim Start unter ASP.NET 2.0 oder ASP.NET 3.5-Anwendungen](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 Web Sites nicht auf Computern gestartet, auf dem SharePoint installiert ist](#0.1__Toc256770151 "_Toc256770151")  
[Die HttpRequest.FilePath-Eigenschaft umfasst nicht mehr PathInfo Werte](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 Anwendungen möglicherweise generieren HttpException-Fehler, die eurl.axd verweisen](#0.1__Toc256770153 "_Toc256770153")  
[Ereignishandler können in ein Standarddokument in IIS 7 oder IIS 7.5 nicht nicht ausgelöst integrierten Modus](#0.1__Toc256770154 "_Toc256770154")  
[Änderungen an der Codezugriffssicherheit (CAS)-Implementierung von ASP.NET-Code Zugriff](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser und anderen Typen in den System.Web.Security-Namespace verschoben wurden](#0.1__Toc256770156 "_Toc256770156")  
[Ausgabe-Caching Änderungen variieren \* HTTP-Header](#0.1__Toc256770157 "_Toc256770157")  
[System.Web.Security-Typen für Passport sind veraltet](#0.1__Toc256770158 "_Toc256770158")  
[Ein Bild in ASP.NET 4 beim Rendern der MenuItem.PopOutImageUrl-Eigenschaft ein Fehler](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl und Menu.DynamicPopOutImageUrl erzeugt einen Fehler, Bilder zu rendern, wenn Pfade umgekehrten Schrägstriche enthalten](#0.1__Toc256770160 "_Toc256770160")  
[Disclaimer](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>ControlRenderingCompatibilityVersion-Einstellung in der Datei "Web.config"

ASP.NET-Steuerelemente haben in .NET Framework, Version 4 geändert wurde, damit können Sie angeben, genauer gesagt, wie sie Markup gerendert. In früheren Versionen von .NET Framework ausgegeben, einige Steuerelemente Markup, das Sie keine Möglichkeit hatte, zu deaktivieren. Standardmäßig wird ASP.NET 4 diese Art von Markup nicht mehr generiert.

Wenn Sie Visual Studio 2010 verwenden, um Ihre Anwendung von ASP.NET 2.0 oder ASP.NET 3.5 zu aktualisieren, fügt das Tool automatisch eine Einstellung, um die `Web.config` Datei, die legacy-Rendering beibehält. Wenn Sie eine Anwendung jedoch upgraden, indem Sie den Anwendungspool in IIS so ändern, dass er sich an .NET Framework 4 richtet, verwendet ASP.NET standardmäßig den neuen Renderingmodus. Um den neuen Renderingmodus deaktivieren möchten, fügen Sie die folgende Einstellung in der `Web.config` Datei:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Die wichtige Renderingänderungen, die das neue Verhalten schaltet sind wie folgt:

- Die **Image** und **ImageButton** Rendern von Steuerelementen nicht mehr eine `border="0"` Attribut.
- Die **BaseValidator** Klasse und Validierung Steuerelemente, die sich davon Herleiten Rendern rot formatierter Text nicht mehr standardmäßig.
- Die **HtmlForm** Rendern Steuerelements keine **Namen** Attribut.
- Die **Tabelle** -Steuerelement mehr rendert eine `border="0"` Attribut.
- Steuerelemente, die keine Benutzereingaben (z. B. die **Bezeichnung** Steuerelement) nicht mehr Rendern der `disabled="disabled"` Attribut, wenn ihre **aktiviert** -Eigenschaftensatz auf **"false"**(oder wenn sie diese Einstellung von einem Containersteuerelement erben).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode Änderungen

Die **ClientIDMode** Einstellung in ASP.NET 4 können Sie angeben, wie ASP.NET generiert die **Id** Attribut für HTML-Elemente. In früheren Versionen von ASP.NET wurde das Standardverhalten entspricht der **AutoID** Einstellung des **ClientIDMode**. Die Standardeinstellung ist jetzt jedoch **vorhersagbar**.

Wenn Sie Visual Studio 2010 verwenden, um Ihre Anwendung von ASP.NET 2.0 oder ASP.NET 3.5 zu aktualisieren, fügt das Tool automatisch eine Einstellung, um die `Web.config` -Datei, die das Verhalten von früheren Versionen von .NET Framework behält. Wenn Sie eine Anwendung jedoch upgraden, indem Sie den Anwendungspool in IIS so ändern, dass er sich an .NET Framework 4 richtet, verwendet ASP.NET standardmäßig den neuen Modus. Um den neuen Client-ID-Modus zu deaktivieren, fügen Sie die folgende Einstellung in der `Web.config` Datei:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode und UrlEncode jetzt codieren einfache Anführungszeichen

In ASP.NET 4 hat das **HtmlEncode** und **UrlEncode** Methoden die **HTTP Utility** und **HttpServerUtility** Klassen aktualisiert wurden, um Codieren Sie das einfache Anführungszeichen (') wie folgt:

- Die **HtmlEncode** Methode codiert Instanzen von einfachem Anführungszeichen wie.
- Die **UrlEncode** Methode Instanzen von einfachem Anführungszeichen als % 27 codiert.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>ASP.NET-Seite (.aspx) Parser ist Stricter

Der Parser für die ASP.NET-Seiten (`.aspx` Dateien) und Benutzersteuerelemente (`.ascx` Dateien) ist in ASP.NET 4 strenger und weitere Instanzen von Ungültiges Markup abgelehnt. Z. B. werden die folgenden zwei Ausschnitte würde in früheren Versionen von ASP.NET erfolgreich analysiert jedoch jetzt ausgelöst Parserfehler in ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Beachten Sie das ungültige Semikolon am Ende der **HiddenField** Tag.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Beachten Sie die nicht geschlossen **Stil** -Attribut, das in der **CssClass** Attribut.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Browser-Definitionsdateien aktualisiert

Die Browserdefinitionsdateien wurden aktualisiert und enthalten jetzt Informationen zu neuen und aktualisierten Browsern und Geräten. Ältere Browser und Geräte wie Netscape Navigator wurden entfernt und neuere Browser und Geräte wie Google Chrome und Apple iPhone hinzugefügt.

Wenn Ihre Anwendung benutzerdefinierte Browserdefinitionen enthält, die von einer der entfernten Browserdefinitionen erben, wird ein Fehler angezeigt. Beispielsweise, wenn die `App_Browsers` Ordner enthält eine Browserdefinition, die von der Definition der IE2 Browser erbt, Sie erhalten die Fehlermeldung der folgenden Konfiguration:

- Der Browser oder das Gateway-Element mit ID "IE2" kann nicht gefunden werden.

> [!NOTE]
> Die **HttpBrowserCapabilities** Objekt (die verfügbar gemacht wird von der Seite **Request.Browser** Eigenschaft) wird gesteuert durch die Definitionen Browserdateien. Aus diesem Grund kann die durch den Zugriff auf eine Eigenschaft dieses Objekts in ASP.NET 4 zurückgegebene Informationen in einer früheren Version von ASP.NET zurückgegebenen Informationen abweichen.


Sie können die alten Browserdefinitionsdateien zurückkehren durch Kopieren von Dateien mit der Browser aus dem folgenden Ordner:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Kopieren Sie die Dateien in die entsprechende `\CONFIG\Browsers` Ordner für ASP.NET 4. Nachdem Sie die Dateien kopieren, führen Sie das Aspnet\_regbrowsers.exe-Befehlszeilentool.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>"System.Web.Mobile.dll" aus der Webkonfigurationsdatei Stamm entfernt

In früheren Versionen von ASP.NET wurde ein Verweis auf die Assembly "System.Web.Mobile.dll" im Stammverzeichnis enthaltenen `Web.config` in der Datei die **Assemblys** Handlerbereich unter. Um die Leistung zu verbessern, wurde der Verweis auf diese Assembly entfernt.

Die Assembly "System.Web.Mobile.dll" ist in ASP.NET 4 enthalten, aber es ist veraltet. Wenn Sie die Typen aus der Assembly "System.Web.Mobile.dll" verwenden möchten, fügen Sie einen Verweis auf diese Assembly entweder dem Stamm `Web.config` Datei oder eine Anwendung `Web.config` Datei. Z. B., wenn Sie eine (veraltet) ASP.NET mobile-Steuerelemente verwenden möchten, müssen Sie einen Verweis hinzufügen auf die Assembly "System.Web.Mobile.dll" die `Web.config` Datei.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Anforderungsüberprüfung für ASP.NET

Die Anforderung Überprüfungsfunktion in ASP.NET bietet ein gewisses Maß an Standardeinstellung Schutz vor Angriffen siteübergreifendem Skripting (XSS). In früheren Versionen von ASP.NET wurde Anforderungsvalidierung standardmäßig aktiviert. Allerdings es nur auf ASP.NET-Seiten angewendet (`.aspx` und ihre Klassendateien) und nur, wenn die Seiten ausgeführt wurden.

In ASP.NET 4, anforderungsüberprüfung ist standardmäßig aktiviert für alle Anforderungen, da vor der Aktivierung der **BeginRequest** Phase einer HTTP-Anforderung. Daher gilt Anforderungsvalidierung auf Anforderungen für alle ASP.NET-Ressourcen, nicht nur die ASPX-Seiten-Anforderungen. Dies schließt Anforderungen, z. B. Aufrufe des Webdiensts und benutzerdefinierte HTTP-Handler. Anforderungsüberprüfung ist auch aktiv, wenn benutzerdefinierte HTTP-Module auf den Inhalt einer HTTP-Anforderung gelesen werden.

Folglich auftreten Anfrage Überprüfungsfehler jetzt für Anforderungen, die zuvor keine Fehler ausgelöst. Um das Verhalten der Funktion zur Validierung von ASP.NET 2.0-Anforderung wiederherzustellen, fügen Sie die folgende Einstellung in der `Web.config` Datei:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Allerdings wird empfohlen, dass Sie analysieren, um zu bestimmen, ob vorhandene Handler, Module und anderen benutzerdefinierten Code greift auf potenziell unsichere HTTP-Eingaben, die XSS handeln Vektoren Angriffe Anfrage Überprüfungsfehler auftreten.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Hashalgorithmus ist standardmäßig jetzt HMACSHA256

ASP.NET verwendet sowohl Verschlüsselung als auch Hashalgorithmen, um Daten wie Formularauthentifizierungscookies und den Ansichtsstatus zu sichern. Standardmäßig verwendet ASP.NET 4 jetzt die HMACSHA256-Algorithmus für Hashvorgänge für Cookies und Ansichtszustand. Die älteren HMACSHA1-Algorithmus wird von frühere Versionen von ASP.NET verwendet.

Ihre Anwendungen beeinträchtigt werden könnte, wenn das Ausführen von gemischten ASP.NET 2.0/ASP.NET 4 Umgebungen, in denen Daten, z. B. Formularauthentifizierungscookies across.NET Framework-Versionen arbeiten müssen. Um eine ASP.NET 4 Web-Anwendung mithilfe des älteren HMACSHA1 Algorithmus zu konfigurieren, fügen Sie die folgende Einstellung in der `Web.config` Datei:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Konfigurationsfehler, die im Zusammenhang mit der neuen ASP.NET 4-Stammkonfiguration

Die Stamm-Konfigurationsdateien (die `machine.config` Datei- und im Stammverzeichnis `Web.config` Datei) für die .NET Framework 4 (und daher ASP.NET 4) aktualisiert wurden, enthalten i. Standardkonfigurationsinformationen, die in ASP.NET 3.5 in gefunden wurde die Anwendung `Web.config` Dateien. Wegen der Komplexität von den verwalteten Systemen für IIS 7 und IIS 7.5-Konfiguration kann ASP.NET 3.5-Anwendungen unter ASP.NET 4 "und" IIS 7 und IIS 7.5 ausgeführt ASP.NET- oder IIS Konfigurationsfehler führen.

Wir empfehlen, Sie ASP.NET 3.5-Anwendungen auf ASP.NET 4 aktualisieren, indem mit den Projekt-Upgrade-Tools in Visual Studio 2010, wenn praktische. Visual Studio 2010 ändert automatisch der ASP.NET 3.5-Anwendungsverzeichnis `Web.config` Datei, die die entsprechenden Einstellungen für ASP.NET 4 enthalten.

Es ist jedoch ein unterstütztes Szenario zum Ausführen von ASP.NET 3.5-Anwendungen mit .NET Framework 4 ohne erneute Kompilierung aus. In diesem Fall möglicherweise so ändern Sie der Anwendungsverzeichnis manuell `Web.config` Datei vor dem Ausführen der Anwendung, die unter .NET Framework 4 "und" IIS 7 oder IIS 7.5.

In den nächsten beiden Abschnitten werden die Änderungen, die Sie möglicherweise für verschiedene Kombinationen der Software stellen beschrieben.

**Windows Vista SP1 oder WindowsServer 2008 SP1, auf dem weder Hotfix KB958854 noch SP2 installiert werden.** In dieser Konfiguration führt das Konfigurationssystem IIS 7 eine verwaltete Anwendungskonfiguration nicht ordnungsgemäß durch Vergleichen der Anwendungsebene `Web.config` Datei in die ASP.NET 2.0 `machine.config` Dateien. Aufgrund dieser, Anwendungsebene `Web.config` Dateien, die von .NET Framework 3.5 oder höher benötigen eine **system.web.extensions** Abschnitt Konfigurationsdefinition (Element) in der Reihenfolge nicht dazu führen, dass ein IIS 7-Überprüfungsfehler.

Jedoch manuell auf Anwendungsebene geändert `Web.config` Einträge, die nicht genau den ursprünglichen Textbaustein Abschnitt Konfigurationsdefinitionen entsprechen, die mit Visual Studio 2008 eingeführten werden dazu führen, dass ASP.NET Konfigurationsfehler. (Die Standardeinträge für die Konfiguration von den Visual Studio 2008 erstellten ordnungsgemäß funktioniert.) Ein häufig auftretendes Problem ist, die manuell geändert `Web.config` Dateien lassen die **AllowDefinition** und **RequirePermission** Konfigurationsattribute, die auf verschiedenen Konfigurationsabschnitt gefunden werden Definitionen. Dies bewirkt, dass einen Konflikt zwischen den abgekürzten Konfigurationsabschnitt in auf Anwendungsebene `Web.config` Dateien und die vollständige Definition in ASP.NET 4 `machine.config` Datei. Daher löst der ASP.NET 4-Konfigurationssystem zur Laufzeit ein Fehler bei der Konfiguration aus.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2, and also Windows Vista SP1 and Windows Server 2008 SP1 where hotfix KB958854 is installed.**

In diesem Szenario gibt der systemeigenen Konfigurationssystem IIS 7 und IIS 7.5 einen Fehler bei der Konfiguration, da er auf ein Textvergleich ausgeführt der **Typ** -Attribut, das für einen verwalteten Konfigurationsabschnittshandler definiert ist. Da alle `Web.config` Dateien, die von Visual Studio 2008 und Visual Studio 2008 SP1 generiert werden, haben "3.5" in die Typzeichenfolge für die **system.web.extensions** (und zugehörige) Konfigurationsabschnittshandler, und da ASP.NET 4 `machine.config` Datei ist "4.0" in der **Typ** -Attribut für die gleiche Konfigurationsabschnittshandler, Anwendungen, die in Visual Studio 2008 oder Visual Studio 2008 SP1, immer generiert werden die Überprüfung der Konfiguration in IIS 7 fehl und IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Zum Beheben dieser Probleme

Die problemumgehung für das erste Szenario ist beim Aktualisieren der Anwendungsebene `Web.config` Datei dazu den Standardtext für die Konfiguration von einem `Web.config` Datei, die automatisch von Visual Studio 2008 generiert wurde.

Eine alternative problemumgehung für das erste Szenario ist mit Service Pack 2 für Vista oder Windows Server 2008 auf Ihrem Computer zu installieren oder installieren Sie Hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) zu beheben, die falsche Konfiguration der Zusammenführung von der IIS-Konfigurationssystems. Jedoch, nachdem Sie eine der folgenden Aktionen ausführen, wird Ihre Anwendung wahrscheinlich einen Fehler bei der Konfiguration für das zweite Szenario beschriebenen Gründen auftreten.

Die problemumgehung für das zweite Szenario zu löschen, oder kommentieren Sie Sie aus allen ist der **system.web.extensions** Abschnitt Konfigurationsdefinitionen und Konfigurationsabschnitt Gruppe Definitionen über die Anwendungsebene `Web.config` Datei. Diese Definitionen sind in der Regel am oberen Rand der Anwendungsebene `Web.config` Datei und kann festgestellt werden, indem die **"configSections"** Element und seine untergeordneten Elemente.

Für beide Szenarien wird empfohlen, dass Sie auch manuell löschen, die **system.codedom** Abschnitt, obwohl dies nicht erforderlich ist.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET-Anwendungen für 4 untergeordnete Fehler beim Start unter ASP.NET 2.0 oder ASP.NET 3.5-Anwendungen

ASP.NET 4-Anwendungen, die als untergeordnete Anwendungen konfiguriert wurden, die frühere Versionen von ASP.NET ausführen, können möglicherweise aufgrund von Konfigurations- oder Kompilierungsfehlern nicht starten. Das folgende Beispiel zeigt eine Verzeichnisstruktur für eine Anwendung betroffenen.

`/parentwebapp` (für die Verwendung von ASP.NET 2.0 oder ASP.NET 3.5 konfiguriert)  
`/childwebapp` (für die Verwendung von ASP.NET 4 konfiguriert wird)

Die Anwendung in der `childwebapp` Ordner fehl, starten Sie auf IIS 7 oder IIS 7.5 und meldet einen Fehler bei der Konfiguration. Der Fehlertext wird eine Meldung ähnlich der folgenden enthalten:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

Auf die Anwendung in IIS 6 die `childwebapp` Ordner auch nicht gestartet, aber einen anderen Fehler gemeldet. Der Fehlertext möglicherweise z. B. die folgenden Status:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Diese Szenarien auftreten, da die Konfigurationsinformationen aus der übergeordneten Anwendung in der `parentwebapp` Ordner ist Teil der Hierarchie der Konfigurationsinformationen, die die endgültige zusammengeführte Konfigurationseinstellungen bestimmt, die vom untergeordneten-Webdienst verwendet werden Anwendung in der `childwebapp` Ordner. Abhängig davon, ob die ASP.NET 4 Web-Anwendung auf IIS 7 (oder IIS 7.5) oder IIS 6 ausgeführt wird wird dem Konfigurationssystem von IIS oder ASP.NET 4-Kompilierung-System einen Fehler zurück.

Die Schritte, die Sie befolgen müssen, um dieses Problem zu beheben und die abzurufenden untergeordneten ASP.NET 4-Anwendung arbeiten, hängen davon ab, gibt an, ob der ASP.NET 4-Anwendung auf IIS 6 oder IIS 7 (oder IIS 7.5) ausgeführt wird.

### <a name="step-1-iis-7-or-iis-75-only"></a>Schritt 1 (IIS 7 oder IIS 7.5 nur)

Dieser Schritt ist nur für Betriebssysteme, auf denen IIS 7 ausgeführt oder IIS 7.5, darunter Windows Vista, Windows Server 2008, Windows 7 und Windows Server 2008 R2 erforderlich.

Verschieben der **"configSections"** -Definition in der `Web.config` Datei, der die übergeordnete Anwendung (die Anwendung, die ASP.NET 2.0 oder ASP.NET 3.5 ausgeführt wird) in den Stamm `Web.config` -Datei für.NET Framework 2.0. Durchsucht das System für IIS 7 und IIS 7.5 native Konfiguration der **"configSections"** -Element, wenn die Hierarchie von Konfigurationsdateien zusammengeführt. Verschieben der **"configSections"** Definition aus dem übergeordneten Webanwendung `Web.config` Datei im Stammverzeichnis `Web.config` Datei effektiv Blendet das Element aus dem Konfigurationsvorgang für das Zusammenführen, die für das untergeordnete Element ASP.NET 4 auftritt. die Anwendung.

Auf einem 32-Bit-Betriebssystem oder für 32-Bit-Anwendungspools, der Stamm `Web.config` für ASP.NET 2.0 und ASP.NET 3.5 Datei befindet sich normalerweise im folgenden Ordner:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

Auf einem 64-Bit-Betriebssystem oder für 64-Bit-Anwendungspools, der Stamm `Web.config` für ASP.NET 2.0 und ASP.NET 3.5 Datei befindet sich normalerweise im folgenden Ordner:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Wenn Sie 32-Bit und 64-Bit-Webanwendungen auf einem 64-Bit-Computer ausführen, müssen Sie verschieben den **"configSections"** Element oben in Stamm `Web.config` Dateien für die 32-Bit und 64-Bit-Systeme.

Wenn Sie Ablegen der **"configSections"** Element im Stammverzeichnis `Web.config` Datei, fügen Sie im Abschnitt unmittelbar nach der **Konfiguration** Element. Das folgende Beispiel zeigt, welche im oberen Bereich des Stamms `Web.config` Datei sollte wie folgt, wenn Sie abgeschlossen haben, verschieben die Elemente aussehen.

> [!NOTE]
> Im folgenden Beispiel haben Zeilen zur besseren Lesbarkeit umbrochen.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Schritt 2 (alle Versionen von IIS)

Dieser Schritt ist erforderlich, ob das untergeordnete ASP.NET 4 Web-Anwendung auf IIS 6 oder IIS 7 (oder IIS 7.5) ausgeführt wird.

In der `Web.config` des übergeordneten Webanwendung 2 von ASP.NET oder ASP.NET 3.5 ausgeführt wird-Datei hinzufügen eine **Speicherort** Tag, die explizit (für sowohl IIS als auch ASP.NET Configuration-Systeme angibt), nur die Konfigurationseinträge gelten Sie für das übergeordnete Objekt Webanwendung. Das folgende Beispiel zeigt die Syntax der **Speicherort** hinzuzufügenden Elements:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

Das folgende Beispiel zeigt wie die **Speicherort** Tag wird verwendet, um alle Konfigurationsabschnitte beginnend mit umschließen die **"appSettings"** Abschnitt und endend mit **"System.Webserver"**Abschnitt.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Wenn Sie die Schritte 1 und 2 abgeschlossen haben, werden untergeordnete ASP.NET 4 Web Applications ohne Fehler gestartet.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 nicht Websites auf Computern gestartet, auf dem SharePoint installiert ist

Webserver, auf denen SharePoint ausgeführt haben eine `Web.config` -Datei, die im Stammverzeichnis einer SharePoint-Website bereitgestellt wird (z. B. `c:\inetpub\wwwroot\web.config` für Default Web Site). In diesem `Web.config` SharePoint-Datei eine benutzerdefinierte teilweise vertrauenswürdigen legt diese Stufe fest mit dem Namen WSS\_Minimal.

Wenn Sie versuchen, eine ASP.NET 4-Website, die bereitgestellt wird als ein untergeordnetes Element dieses Typs der SharePoint-Website ausführen, wird die folgende Fehlermeldung angezeigt:

`Could not find permission set named 'ASP.NET'.`

Dieser Fehler tritt auf, da die Codezugriffssicherheit (CAS)-Infrastruktur von ASP.NET 4 für einen Berechtigungssatz, der mit dem Namen ASP.NET sucht. Jedoch der partiellen Konfigurationsdatei, die von WSS verwiesen wird, vertrauen\_minimale enthält keine Berechtigungssätze mit diesem Namen.

Derzeit ist keine Version von SharePoint, die mit ASP.NET kompatibel ist. Daher sollten Sie nicht versuchen, eine ASP.NET 4-Website als untergeordneter Standort unter SharePoint-Web Sites ausführen.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>Die HttpRequest.FilePath-Eigenschaft enthält nicht mehr PathInfo-Werte.

Frühere Versionen von ASP.NET enthalten eine **PathInfo** Wert in den Rückgabewert aus verschiedenen pfadbezogene Dateieigenschaften, einschließlich **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, und **HttpRequest.CurrentExecutionFilePath**. ASP.NET 4 umfasst nicht mehr die **PathInfo** Wert in die Rückgabewerte von dieser Eigenschaften. Stattdessen die **PathInfo** Informationen finden Sie in **HttpRequest.PathInfo**. Nehmen wir z.B. das folgenden URL-Fragment:

`/testapp/Action.mvc/SomeAction`

In früheren Versionen von ASP.NET **HttpRequest** Eigenschaften die folgenden Werte aufweisen:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (empty)

In ASP.NET 4 **HttpRequest** Eigenschaften stattdessen die folgenden Werte aufweisen:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 Anwendungen möglicherweise generieren HttpException-Fehler, die eurl.axd verweisen

Nach ASP.NET 4 auf IIS 6 aktiviert wurde, möglicherweise ASP.NET 2.0-Anwendungen, die auf IIS 6 (in Windows Server 2003 oder Windows Server 2003 R2) ausgeführt wie der folgende Fehler generiert:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Dieser Fehler tritt auf, da ASP.NET erkennt, dass eine Website für die Verwendung von ASP.NET 4 konfiguriert ist, eine systemeigene Komponente von ASP.NET 4 eine URL ohne Erweiterung an den verwalteten Teil von ASP.NET zur weiteren Verarbeitung übergibt. Jedoch wenn virtuelle Verzeichnisse, die unterhalb einer ASP.NET 4-Website für die Verwendung von ASP.NET 2.0 konfiguriert sind, verarbeitet die URL, die in dieser Weise Ergebnisse in einer geänderten URL ohne Erweiterung, die die Zeichenfolge "eurl.axd" enthält. Diese geänderten URL wird dann an die ASP.NET 2.0-Anwendung gesendet. ASP.NET 2.0 erkannt nicht das Format "eurl.axd". ASP.NET 2.0 aus diesem Grund versucht, eine Datei mit dem Namen `eurl.axd` und führen Sie sie aus. Da keine derartige Datei vorhanden ist, missling die Anforderung mit einem **HttpException** Ausnahme.

Sie können dieses Problem mithilfe einer der folgenden Optionen umgehen.

### <a name="option-1"></a>Option 1

Wenn ASP.NET 4 nicht erforderlich, um die Website ausgeführt wird, ordnen Sie den Standort für die Verwendung von ASP.NET 2.0 stattdessen an.

### <a name="option-2"></a>Option 2

Wenn ASP.NET 4 erforderlich ist, um die Website ausgeführt wird, verschieben Sie alle untergeordneten virtuellen ASP.NET 2.0-Verzeichnisse auf eine andere Website, die ASP.NET 2.0 zugeordnet ist.

### <a name="option-3"></a>Option 3

Wenn es nicht möglich, auf der Website für ASP.NET 2.0 erneut zuordnen oder zum Ändern des Speicherorts eines virtuellen Verzeichnisses ist, sollten Sie die deaktivieren Sie URL ohne Erweiterung, die Verarbeitung in ASP.NET 4 explizit. Verwenden Sie das folgende Verfahren:

1. Öffnen Sie in der Windows-Registrierung die folgenden Knoten:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Erstellen Sie ein neues **DWORD** -Wert namens **EnableExtensionlessUrls**.
2. Legen Sie **EnableExtensionlessUrls** auf 0. Dadurch werden ohne Erweiterung URL-Verhalten deaktiviert.
3. Speichern Sie den Registrierungswert "", und schließen Sie den Registrierungs-Editor.
4. Führen Sie die **Iisreset** Befehlszeilentool, wodurch IIS die neue Registrierungswert gelesen.

> [!NOTE]
> Festlegen von **EnableExtensionlessUrls** 1 ermöglicht Standarddokument URL-Verhalten. Dies ist die Standardeinstellung, wenn kein Wert angegeben wird.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Ereignishandler können in ein Standarddokument in IIS 7 oder IIS 7.5 nicht nicht ausgelöst integrierten-Modus

ASP.NET 4 umfasst Änderungen, die ändern, wie die **Aktion** -Attribut von HTML **Formular** Element gerendert wird, wenn eine URL ohne Erweiterung in ein Standarddokument aufgelöst wird. Ein Beispiel für eine URL ohne Erweiterung zu Standarddokument wäre [ http://contoso.com/ ](http://contoso.com/), wodurch eine Anforderung zum [ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx).

ASP.NET 4 stellt nun das HTML- **Formular** des Elements **Aktion** -Attributwert als eine leere Zeichenfolge, wenn eine Anforderung an eine URL ohne Erweiterung erfolgt, die ein Standarddokument zugeordnet wurde. Beispielsweise ist in früheren Versionen von ASP.NET verwenden, eine Anforderung zum [ http://contoso.com ](http://contoso.com) würde eine Anforderung zum `Default.aspx`. In diesem Dokument, das öffnende **Formular** Tag würde gerendert werden, wie im folgenden Beispiel gezeigt:

`<form action="Default.aspx" />`

In ASP.NET 4 hat eine Anforderung zum [ http://contoso.com ](http://contoso.com) führt auch eine Anforderung zum `Default.aspx`. Allerdings rendert ASP.NET jetzt das öffnende HTML **Formular** Tag, wie im folgenden Beispiel gezeigt:

`<form action="" />`

Dieser Unterschied wie die **Aktion** Attribut wird gerendert, kann dazu führen, dass geringfügige Änderungen im wie eine Formular-Post von IIS und ASP.NET verarbeitet wird. Wenn die **Aktion** Attribut ist eine leere Zeichenfolge, die IIS **DefaultDocumentModule** objekterstellung eine untergeordnete Anforderung an `Default.aspx`. Die meisten ausgelastet dieser untergeordnete Anforderung für Anwendungscode transparent ist und die `Default.aspx` Seite normal ausgeführt wird.

Allerdings kann eine mögliche Interaktion zwischen verwaltetem Code und IIS 7 oder IIS 7.5 im integrierten Modus dazu führen, dass verwaltete ASPX-Seiten während der untergeordnete Anforderung nicht mehr ordnungsgemäß ausgeführt werden. Wenn die folgenden Bedingungen auftreten, die untergeordnete Anforderung an eine `Default.aspx` Dokument führt zu einem Fehler oder unerwartetes Verhalten:

1. Eine ASPX-Seite wird gesendet, um den Browser mit der **Formular** des Elements **Aktion** -Attributsatz zur "".
2. Das Formular wird an ASP.NET zurückgesendet.
3. Ein verwaltetes HTTP-Modul liest einen Teil des einheitstextkörpers. Angenommen, ein Modul liest **Request.Form** oder **Request.Params**. Aus diesem Grund wird der Einheitstextkörper der POST-Anforderung im verwalteten Arbeitsspeicher gelesen. Der Einheitstextkörper ist daher nicht mehr für native Codemodule verfügbar, die in IIS 7 oder IIS 7.5 im integrierten Modus ausgeführt werden.
4. Die IIS **DefaultDocumentModule** Objekt schließlich ausgeführt wird, und erstellt eine untergeordnete Anforderung an die `Default.aspx` Dokument. Da der Einheitstextkörper bereits von verwaltetem Code gelesen wurde, kann kein Einheitstextkörper an die untergeordnete Anforderung gesendet werden.
5. Wenn die HTTP-Pipeline ausgeführt wird, für die untergeordnete Anforderung, den Handler für `.aspx` Dateien, die während der Handlerausführungsphase ausgeführt wird.
6. Da es kein-einheitstextkörper ist, stehen keine Formularvariablen und kein Ansichtszustand und aus diesem Grund sind keine Informationen verfügbar, für den Handler ASPX-Seite, um zu bestimmen, welches Ereignis (sofern vorhanden), die ausgelöst werden soll. Daher werden keine Postback-Ereignishandler für die betroffene ASPX-Seite ausgeführt.

Sie können dieses Verhalten auf folgende Weise umgehen:

- Identifizieren Sie das HTTP-Modul, das die Anforderungs-einheitstextkörper während der Standard-dokumentanforderungen zugreift, und bestimmen Sie, ob diese konfiguriert werden kann, nur für verwaltete Anforderungen ausgeführt. Im integrierten Modus für IIS 7 und IIS 7.5 HTTP-Module gekennzeichnet werden können, um nur für verwaltete Anforderungen ausgeführt werden, indem Sie das folgende Attribut an des Moduls **System.Webserver/Modules** Eintrag:

- `precondition="managedHandler"`

- Diese Einstellung deaktiviert, die das Modul für IIS 7 und IIS 7.5 als nicht bestimmen fordert verwaltet Anforderungen. Die erste Anforderung werden für Standard-dokumentanforderungen aus einer URL ohne Erweiterung. Aus diesem Grund wird IIS nicht ausgeführt verwaltete Module, die markiert sind mit einer Vorbedingung verwaltete Handler während der Verarbeitung der ursprünglichen Anforderung. Folglich verwaltete Module nicht versehentlich den entitätsnachrichtentext liest, weshalb die Entitätstext ist weiterhin verfügbar und entlang übergeben wird, um die untergeordnete Anforderung und das Standarddokument.

- Wenn die problematischen HTTP-Module für alle Anforderungen ausführen (für statische Dateien für URLs ohne Erweiterung, die zum Auflösen der **DefaultDocumentModule** -Objekt, um verwaltete Anforderungen usw.), ändern Sie die betroffenen ASPX-Seiten, indem Sie explizit Festlegen der **Aktion** Eigenschaft von der Seite **System.Web.UI.HtmlControls.HtmlForm** Steuerelement in eine nicht leere Zeichenfolge. Z. B., wenn das Standarddokument ist `Default.aspx`, ändern Sie den Anzeigezustand der Seite Code aus, um explizit die **HtmlForm** des Steuerelements **Aktion** -Eigenschaft auf "Default.aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Änderungen an der Implementierung von ASP.NET-Code Zugriff Codezugriffssicherheit (CAS)

ASP.NET 2.0, und die Erweiterung ASP.NET-Funktionen, die in der 3.5 verwendet wird, hinzugefügt wurden, mithilfe der .NET Framework 1.1 und 2.0 (Security, CAS) Codezugriffssicherheitsmodell. Die Implementierung von CAS wurde in ASP.NET 4 aber erheblich überholt. Folglich können teilweise vertrauenswürdigen ASP.NET-Anwendungen, die abhängig von vertrauenswürdigem Code ausgeführt wird, im globalen Assemblycache (GAC) mit verschiedenen Sicherheitsausnahmen fehlschlagen. Teilweise vertrauenswürdige Anwendungen, die auf eine umfangreiche Änderungen auf die CAS-Richtlinie Computer basieren können auch mit Sicherheitsausnahmen fehlschlagen.

Können Sie teilweise vertrauenswürdigem ASP.NET 4-Anwendungen mit dem Verhalten von ASP.NET 1.1 und 2.0, die mit dem neuen Wiederherstellen **LegacyCasModel** Attribut in der **Vertrauensstellung** Configuration-Element, wie im folgenden Beispiel gezeigt. :

`<trust level= "Medium" legacyCasModel="true" />`

Wenn Sie die legacy-CAS-Modell zurückkehren, werden die folgenden alten CAS-Verhalten aktiviert:

- Computer-CAS-Richtlinie wird berücksichtigt.
- Es sind mehrere unterschiedliche Berechtigungssätze in einer einzelnen Anwendungsdomäne zulässig.
- Explizite Berechtigung Assertionen sind nicht für Assemblys im GAC, die aufgerufen werden, wenn nur ASP.NET oder andere .NET Framework-Code auf dem Stapel ist erforderlich.

Ein Szenario kann nicht wiederhergestellt werden, in der .NET Framework 4: nicht-teilweise vertrauenswürdige Anwendungen können nicht mehr bestimmte APIs in "System.Web.dll" und System.Web.Extensions.dll aufrufen. In früheren Versionen von .NET Framework war es möglich für nicht-teilweise vertrauenswürdige Webanwendungen explizit erteilt werden <strong>überschneidet AspNetHostingPermission</strong> Berechtigungen. Anschließend können diese Anwendungen verwenden, <strong>System.Web.HttpUtility</strong>, Typen in der <strong>System.Web.ClientServices.\< / strong > *-Namespaces und Typen im Zusammenhang mit Mitgliedschaft, Rollen und Profile. Diese Typen von nicht-teilweise vertrauenswürdige Anwendungen aufrufen wird in .NET Framework 4 nicht mehr unterstützt.

> [!NOTE]
> Die **HtmlEncode** und **HtmlDecode** Funktionalität von der **System.Web.HttpUtility** Klasse wurde verschoben, um die neuen .NET Framework 4  **System.Net.WebUtility** Klasse. War die einzige ASP.NET-Funktionalität, die verwendet wurde, ändern Sie die Anwendung Code entsprechend der neuen **WebUtility** stattdessen.


Im folgenden finden eine Zusammenfassung der Änderungen an der Standard-CAS-Implementierung in ASP.NET 4:

- ASP.NET-Anwendungsdomänen werden jetzt Homogene Anwendungsdomänen. Nur teilweise vertrauenswürdigem und volle Vertrauenswürdigkeit gewähren Mengen sind in einer Anwendungsdomäne verfügbar.
- ASP.NET teilweise Vertrauenswürdigkeit gewähren Sätze sind unabhängig von der alle auf Unternehmensebene, Computerebene oder Benutzerebene CAS-Richtlinie.
- ASP.NET-Assemblys, die in der 3.5 und 3.5 SP1 geliefert wurden konvertiert, um das Transparenzmodell der .NET Framework 4 verwenden.
- Verwendung von ASP.NET **überschneidet AspNetHostingPermission** Attribut wurde erheblich reduziert. Die meisten Instanzen dieses Attributs wurden aus der öffentlichen ASP.NET APIs entfernt.
- Dynamisch kompilierte Assemblys durch die ASP.NET-Anbieter für Build erstellte wurden aktualisiert, um explizit Assemblys als transparent kennzeichnen.
- Alle Assemblys auf ASP.NET werden jetzt so markiert, die das APTCA-Attribut nur im Webhosting-Umgebungen berücksichtigt wird. Teilweise vertrauenswürdige nicht-Hostingumgebungen wie ClickOnce kann nicht in ASP.NET-Assemblys aufgerufen werden.

Weitere Informationen zu den neuen ASP.NET 4-Codezugriffssicherheitsmodell, finden Sie unter [mithilfe der Codezugriffssicherheit in ASP.NET-Anwendungen](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) auf der MSDN-Website.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser und anderen Typen in den System.Web.Security-Namespace wurden verschoben

Einige Typen, die in die ASP.NET-Mitgliedschaft verwendeten wurden aus verschoben `System.Web.dll` der neuen System.Web.ApplicationServices.dll-Assembly. So sollten architektonische Schichtabhängigkeiten zwischen Typen im Client und erweiterten .NET Framework-SKUs gelöst werden.

Websiteprojekte weisen keine Probleme aufgrund dieser Typen veranlassen, verschieben, weil System.Web.ApplicationServices.dll hinzugefügt wurde, um die Liste der referenzierten Assemblys, die standardmäßig verwendet wird vom ASP.NET Kompilierungssystem. Wenn Sie eine Website-Projekt, die mit einer früheren Version von ASP.NET auf ASP.NET 4 aktualisieren, indem Sie sie in Visual Studio 2010 öffnen erstellt wurde, wird das Projekt ohne Fehler kompiliert.

Auf ähnliche Weise, wenn Sie ein Webprojekt für die Anwendung, die in einer früheren Version von ASP.NET auf ASP.NET 4 erstellt wurde aktualisieren, indem Sie sie in Visual Studio 2010 öffnen, im Rahmen des Upgradeprozesses einen Verweis auf System.Web.ApplicationServices.dll dem Projekt hinzugefügt. Aus diesem Grund aktualisiert Web-Anwendungsprojekten auch ohne Fehler kompiliert werden.

Kompilierte Dateien (binäre), die mit früheren Versionen von ASP.NET erstellten auch führen ohne Fehler ASP.NET 4 auf, obwohl die Typen der Mitgliedschaft in einer anderen Assembly verschoben wurden. Informationen zur typweiterleitung hinzugefügt wurde, auf ASP.NET 4-Version von `System.Web.dll` , die zur Laufzeit Verweise für diese Typen automatisch auf den neuen Speicherort für die Typen weiterleitet.

Klassenbibliotheken, die bestimmte Mitgliedschaft Typen verwenden und aus früheren Versionen von ASP.NET aktualisiert wurden, treten jedoch bei der Verwendung in einem ASP.NET 4-Projekt zu kompilieren. Angenommen, ein Klassenbibliotheksprojekt möglicherweise nicht kompiliert, und melden einen Fehler wie den folgenden:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Sie können dieses Problem umgehen, indem Sie in Ihr Klassenbibliotheksprojekt einen Verweis hinzufügen, um System.Web.ApplicationServices.dll.

Die folgende Liste zeigt die *System.Web.Security* Typen, die von verschoben wurden `System.Web.dll` auf System.Web.ApplicationServices.dll:

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *System.Web.Security.MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Ausgabe-Caching Änderungen variieren \* HTTP-Header

In ASP.NET 1.0, ein Fehler verursacht zwischengespeicherte Seiten, die angegeben `Location="ServerAndClient"` als Ausgabecache – Einstellung zum Ausgeben einer `Vary:*` HTTP-Header in der Antwort. So wurde Clientbrowsern mitgeteilt, dass Seiten nie lokal zwischengespeichert werden sollen.

In ASP.NET Version 1.1 die **System.Web.HttpCachePolicy.SetOmitVaryStar** -Methode wurde hinzugefügt, die konnte heraus aufgerufen werden unterdrückt die `Vary:*` Header. Diese Methode wurde gewählt, weil ändern die angegebene HTTP-Header betrachtet wurde eine potenziell wichtige Änderung zum Zeitpunkt. Jedoch vom Verhalten in ASP.NET haben Entwickler verwirrt und Problemberichte vorgeschlagen, dass Entwickler keine Kenntnis von den vorhandenen sind **SetOmitVaryStar** Verhalten.

In ASP.NET 4 wurde die Entscheidung getroffen, beheben Sie das Hauptproblem liegt. Die `Vary:*` HTTP-Header wird nicht mehr in Antworten, die angeben, die folgende Anweisung ausgegeben:

`<%@OutputCache Location="ServerAndClient" %>`

Folglich **SetOmitVaryStar** wird nicht mehr benötigt werden, um Unterdrücken der `Vary:*` Header.

In Anwendungen, die angeben, `Location="ServerAndClient"` in der **@ OutputCache** Richtlinie auf einer Seite, sehen Sie jetzt das Verhalten durch den Namen des impliziert die **Speicherort** Attributwert – also, Seiten werden Übertragen von zwischenspeicherbaren im Browser ohne, die Sie Aufrufen der **SetOmitVaryStar** Methode.

Wenn Seiten in der Anwendung ausgeben müssen `Vary:*`, rufen Sie die **AppendHeader** Methode, wie im folgenden Beispiel gezeigt:

`HttpResponse.AppendHeader("Vary","*");`

Alternativ können Sie den Wert der Ausgabecache ändern **Speicherort** -Attribut auf "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>System.Web.Security-Typen für Passport sind veraltet

Die Passport-Unterstützung in ASP.NET 2.0 integriert seit Jahren aufgrund von Änderungen in der Passport (jetzt LiveID) veraltet und wird nicht unterstützt. Daher die fünf Typen verknüpft Passport in **System.Web.Security** sind jetzt mit markiert die **ObsoleteAttribute** Attribut.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>Die MenuItem.PopOutImageUrl-Eigenschaft ein Fehler auftritt, um ein Bild in ASP.NET 4 zu rendern

In ASP.NET 3.5 die *MenuItem.PopOutImageUrl* -Eigenschaft können Sie die URL für ein Bild angeben, das angezeigt wird, in ein Menüelement, um anzugeben, dass das Menüelement über ein dynamisches Untermenü verfügt. Im folgende Beispiel wird gezeigt, wie diese Eigenschaft in Markup in ASP.NET 3.5 angegeben.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

Als Ergebnis einer entwurfsänderung in ASP.NET 4, wird keine Ausgabe für gerendert der *PopOutImageUrl* , wenn die Eigenschaft, für festgelegt ist die *"MenuItem"* Klasse. Stattdessen müssen Sie angeben, einen Bild-URL direkt in die *Menü* steuern, verwenden entweder die *StaticPopOutImageUrl* Eigenschaft oder die *DynamicPopOutImageUrl* Eigenschaft. Bei der Arbeit mit einem statischen Menü der *Menu.StaticPopOutImageUrl* Eigenschaft gibt die URL für ein Bild, das angezeigt wird, um anzugeben, dass das statische Menüelement über ein Untermenü verfügt, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Wenn Sie mit einem dynamischen Menü arbeiten, verwenden Sie die *Menu.DynamicPopOutImageUrl* Eigenschaft, um die URL für ein Bild anzugeben, der angibt, dass ein dynamisches Menüelement über ein Untermenü verfügt. Das folgende Beispiel ähnelt dem vorherigen, aber veranschaulicht das Festlegen der *DynamicPopOutImageUrl* -Eigenschaft für ein dynamisches Menü.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Wenn die *Menu.DynamicPopOutImageUrl* Eigenschaft nicht festgelegt ist und die *Menu.DynamicEnableDefaultPopOutImage* -Eigenschaftensatz auf *"false"*, kein Bild angezeigt wird. Auf ähnliche Weise, wenn die *StaticPopOutImageUrl* Eigenschaft nicht festgelegt ist und die *StaticEnableDefaultPopOutImage* -Eigenschaftensatz auf *"false"*, kein Bild angezeigt wird.

Wenn Sie die Pfade für diese Eigenschaften festlegen, verwenden Sie einen Schrägstrich (/) anstelle von einem umgekehrten Schrägstrich (\). Weitere Informationen finden Sie unter [Menu.StaticPopOutImageUrl und Menu.DynamicPopOutImageUrl erzeugt einen Fehler zum Rendern von Images beim Pfade enthalten umgekehrte Schrägstriche](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") an anderer Stelle in diesem Dokument.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl und Menu.DynamicPopOutImageUrl keine Bilder darstellen, wenn die Pfade umgekehrten Schrägstriche enthalten.

In ASP.NET 4, die Bilder, die Sie angeben, mit der *Menu.StaticPopOutImageUrl* und *Menu.DynamicPopOutImageUrl* Eigenschaften werden nicht gerendert, wenn der Pfad Backlashes enthält (\). Dies ist eine Änderung gegenüber früheren Versionen von ASP.NET.

Im folgenden Beispiel *Menü* Markup zeigt steuern die *StaticPopOutImageUrl* Eigenschaftensatz unter Verwendung eines Pfads, der einen umgekehrten Schrägstrich enthält. In ASP.NET 4 wird das Bild in der Eigenschaft angegebenen nicht gerendert.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Um dieses Problem zu beheben, ändern Sie Pfadwerte, die im angegebenen der *StaticPopOutImageUrl* und *DynamicPopOutImageUrl* Eigenschaften Schrägstriche (/) verwendet werden. Das folgende Beispiel zeigt diese Änderung:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Beachten Sie, dass Anwendungen, die von früheren Versionen von ASP.NET auf ASP.NET 4 migriert wurden, auch kann beeinflusst werden, da die *MenuItem.PopOutImageUrl* -Eigenschaft geändert wurde. Weitere Informationen finden Sie unter [der MenuItem.PopOutImageUrl Eigenschaft kann eine ASP.NET 4-Bild rendert](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") an anderer Stelle in diesem Dokument.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor dem kommerziellen Release der beschriebenen Software ggf. erheblich geändert wird.

Die Informationen in diesem Dokument repräsentieren den aktuellen Standpunkt der Microsoft Corporation zu den erörterten Problemen zum Veröffentlichungstermin. Da Microsoft auf das Ändern von Marktlagen reagieren muss, ist das Dokument keinesfalls als Verpflichtung von Microsoft zu interpretieren, und Microsoft kann die Genauigkeit der Informationen nicht über den Zeitpunkt der Veröffentlichung hinaus garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation kein Teil dieses Dokuments für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, einer der Firmen, Organisationen, Produkte, Domänennamen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse in diesem Dokument dargestellten sind frei erfunden und keine Zuordnung zu echten Unternehmen, Organisation, Produkt, Domänennamen, e-Mail Adresse Logo "," Person "," Ort "oder" Ereignis dient, noch ableitbar.

© 2010 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
