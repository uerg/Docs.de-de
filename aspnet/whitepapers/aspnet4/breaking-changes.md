---
uid: whitepapers/aspnet4/breaking-changes
title: 'ASP.NET 4: bedeutende Änderungen | Microsoft-Dokumentation'
author: rick-anderson
description: Dieses Dokument beschreibt die Änderungen, die für .NET Framework, Version 4 Version vorgenommen wurden, die möglicherweise Anwendungen auswirken können, die erstellt wurden, mit...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: e6d7972c333e302bb8b6b2d23ea7123b8757b2f4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842512"
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4: bedeutende Änderungen
====================
> Dieses Dokument beschreibt die Änderungen, die für .NET Framework, Version 4 Version vorgenommen wurden, die möglicherweise Anwendungen auswirken können, die mit früheren Versionen, einschließlich der ASP.NET 4 Beta 1 und Beta 2-Versionen erstellt wurden.
> 
> [In diesem Whitepaper herunterladen](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Inhalt

[ControlRenderingCompatibilityVersion-Einstellung in der Datei "Web.config"](#0.1__Toc256770141 "_Toc256770141")  
[Änderungen für "ClientIDMode"](#0.1__Toc256770142 "_Toc256770142")  
["HTMLEncode" und "UrlEncode" nun codieren Sie einfache Anführungszeichen](#0.1__Toc256770143 "_Toc256770143")  
[ASP.NET-Seite (.aspx)-Parser ist Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Browserdefinitionsdateien aktualisiert](#0.1__Toc256770145 "_Toc256770145")  
["System.Web.Mobile.dll" entfernt, vom Stamm der Webkonfigurationsdatei](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET Anforderungsvalidierung](#0.1__Toc256770147 "_Toc256770147")  
[Standardhashalgorithmus ist jetzt HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Fehler im Zusammenhang mit der neuen ASP.NET 4-Stammkonfiguration](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 untergeordnete Anwendungen werden nicht gestartet unter ASP.NET 2.0 oder ASP.NET 3.5 Applications](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 Web Sites nicht auf Computern gestartet werden, auf dem SharePoint installiert ist](#0.1__Toc256770151 "_Toc256770151")  
[Die Eigenschaft HttpRequest.FilePath enthält nicht mehr PathInfo-Werte](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 Applications möglicherweise HttpException-Fehler, die auf eurl.axd verweisen generiert](#0.1__Toc256770153 "_Toc256770153")  
[Ereignishandler möglicherweise nicht nicht ausgelöst werden in einem Standarddokument in IIS 7 oder IIS 7.5 integrierten Modus](#0.1__Toc256770154 "_Toc256770154")  
[Änderungen an der Codezugriffssicherheit (CAS)-Implementierung von ASP.NET-Code Zugriff](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser und anderen Typen in der System.Web.Security-Namespace verschoben wurden](#0.1__Toc256770156 "_Toc256770156")  
[Ausgabe-Caching Änderungen variieren \* HTTP-Header](#0.1__Toc256770157 "_Toc256770157")  
[System.Web.Security-Typen für Passport sind, Obsolete](#0.1__Toc256770158 "_Toc256770158")  
[Die Eigenschaft MenuItem.PopOutImageUrl Rendern ein Bild in ASP.NET 4 ist ein Fehler](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl "und" Menu.DynamicPopOutImageUrl Fail, Bilder zu rendern, wenn Pfade umgekehrten Schrägstriche enthalten](#0.1__Toc256770160 "_Toc256770160")  
[Haftungsausschluss](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>ControlRenderingCompatibilityVersion-Einstellung in der Datei "Web.config"

ASP.NET-Steuerelemente wurden geändert, in .NET Framework, Version 4 damit können Sie angeben, genauer gesagt, wie sie Markup gerendert. In früheren Versionen von .NET Framework ausgegeben, einige Steuerelemente Markup, das Sie keine Möglichkeit haben, zu deaktivieren. Standardmäßig ist ASP.NET 4 diese Art von Markup nicht mehr generiert.

Wenn Sie Visual Studio 2010 verwenden, um die Anwendung von ASP.NET 2.0 oder ASP.NET 3.5 zu aktualisieren, fügt das Tool automatisch eine Einstellung, um die `Web.config` Datei das Legacyrendering beibehält. Wenn Sie eine Anwendung jedoch upgraden, indem Sie den Anwendungspool in IIS so ändern, dass er sich an .NET Framework 4 richtet, verwendet ASP.NET standardmäßig den neuen Renderingmodus. Um den neuen Renderingmodus zu deaktivieren, fügen Sie die folgende Einstellung in der `Web.config` Datei:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Die wichtigsten Renderingänderungen, die das neue Verhalten bietet lauten wie folgt aus:

- Die **Image** und **ImageButton** Rendern von Steuerelementen nicht mehr eine `border="0"` Attribut.
- Die **BaseValidator** -Klasse und die Validierungssteuerelemente, die von ihr abgeleitet werden nicht mehr Rendern roten Text standardmäßig.
- Die **HtmlForm** Steuerelement rendert keine **Namen** Attribut.
- Die **Tabelle** -Steuerelement nicht mehr rendert eine `border="0"` Attribut.
- Steuerelemente, die nicht für Benutzereingaben entworfen wurden (z. B. die **Bezeichnung** Steuerelement), rendern nicht mehr die `disabled="disabled"` Attribut, wenn ihre **aktiviert** -Eigenschaftensatz auf **"false"**(oder wenn sie diese Einstellung von einem Containersteuerelement erben).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>Änderungen für "ClientIDMode"

Die **"ClientIDMode"** -Einstellung in ASP.NET 4 können Sie angeben, wie ASP.NET generiert die **Id** -Attribut für HTML-Elemente. In früheren Versionen von ASP.NET entsprach das Standardverhalten, das **"AutoID"** -Einstellung **"ClientIDMode"**. Die Standardeinstellung ist jetzt jedoch **vorhersagbar**.

Wenn Sie Visual Studio 2010 verwenden, um die Anwendung von ASP.NET 2.0 oder ASP.NET 3.5 zu aktualisieren, fügt das Tool automatisch eine Einstellung, um die `Web.config` -Datei, die das Verhalten früherer Versionen von .NET Framework beibehält. Wenn Sie eine Anwendung jedoch upgraden, indem Sie den Anwendungspool in IIS so ändern, dass er sich an .NET Framework 4 richtet, verwendet ASP.NET standardmäßig den neuen Modus. Um den neuen Client-ID-Modus zu deaktivieren, fügen Sie die folgende Einstellung in der `Web.config` Datei:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>"HTMLEncode" und "UrlEncode" nun codieren einfache Anführungszeichen

In ASP.NET 4 die **HtmlEncode** und **UrlEncode** Methoden der **HttpServerUtility** und **das HttpServerUtility** Klassen aktualisiert wurden, um Codieren Sie das einfache Anführungszeichen (') wie folgt:

- Die **HtmlEncode** Methode codiert Instanzen von einfachem Anführungszeichen wie.
- Die **UrlEncode** Methode codiert Instanzen von einfachem Anführungszeichen als % 27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>ASP.NET-Seite (.aspx)-Parser ist Stricter

Der Seitenparser für ASP.NET-Seiten (`.aspx` Dateien) und Benutzersteuerelemente (`.ascx` Dateien) ist in ASP.NET 4 strenger und weitere Instanzen Ungültiges Markup abgelehnt. Z. B. die folgenden zwei Codeausschnitten werden erfolgreich in früheren Versionen von ASP.NET analysiert, aber löst jetzt Parserfehler in ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Beachten Sie, dass das ungültige Semikolon am Ende der **HiddenField** Tag.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Beachten Sie, dass die nicht geschlossene **Stil** -Attribut, das Auftreten der **CssClass** Attribut.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Browserdefinitionsdateien aktualisiert

Die Browserdefinitionsdateien wurden aktualisiert und enthalten jetzt Informationen zu neuen und aktualisierten Browsern und Geräten. Ältere Browser und Geräte wie Netscape Navigator wurden entfernt und neuere Browser und Geräte wie Google Chrome und Apple iPhone hinzugefügt.

Wenn Ihre Anwendung benutzerdefinierte Browserdefinitionen enthält, die von einer der entfernten Browserdefinitionen erben, wird ein Fehler angezeigt. Z. B. wenn die `App_Browsers` Ordner enthält eine Browserdefinition, die von der IE2 Browserdefinition erbt, erhalten Sie die folgende Konfiguration Fehlermeldung angezeigt:

- Der Browser oder das Gateway-Element mit der ID 'IE2' kann nicht gefunden werden.

> [!NOTE]
> Die **HttpBrowserCapabilities** Objekt (das wird verfügbar gemacht, von der Seite **Request.Browser** Eigenschaft) wird durch die Browser-Definitionen-Dateien gesteuert. Aus diesem Grund kann Zugriff auf eine Eigenschaft dieses Objekts in ASP.NET 4 zurückgegebene Informationen in einer früheren Version von ASP.NET zurückgegebenen Informationen abweichen.


Sie können die alten Browserdefinitionsdateien wiederherstellen, durch die Browserdefinitionsdateien aus dem folgenden Ordner kopieren:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Kopieren Sie die Dateien in den entsprechenden `\CONFIG\Browsers` Ordner für ASP.NET 4. Nachdem Sie die Dateien kopiert haben, führen Sie das Aspnet\_regbrowsers.exe-Befehlszeilentool.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>"System.Web.Mobile.dll" aus der Webkonfigurationsdatei Stamm entfernt

In früheren Versionen von ASP.NET ein Verweis auf die Assembly "System.Web.Mobile.dll" enthalten war, im Stammverzeichnis `Web.config` Datei die **Assemblys** unter Abschnitt. Um die Leistung zu verbessern, wurde der Verweis auf diese Assembly entfernt.

Die Assembly "System.Web.Mobile.dll" ist in ASP.NET 4 enthalten, aber es ist als veraltet markiert. Wenn Sie Typen aus der Assembly "System.Web.Mobile.dll" verwenden möchten, fügen Sie einen Verweis auf diese Assembly entweder dem Stamm `Web.config` Datei oder eine Anwendung `Web.config` Datei. Z. B. Wenn Sie keines (veraltet) ASP.NET mobile-Steuerelemente verwenden möchten, müssen Sie hinzufügen einen Verweis auf die Assembly "System.Web.Mobile.dll", um die `Web.config` Datei.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>ASP.NET-Request-Überprüfung

Das Feature zur Anforderung Validierung in ASP.NET bietet ein gewisses Maß an standardmäßigen Schutz vor Angriffen mit siteübergreifenden Skripterstellung (XSS). In früheren Versionen von ASP.NET wurde die Request-Überprüfung standardmäßig aktiviert. Allerdings es nur für ASP.NET-Seiten angewendet (`.aspx` Dateien und ihre Klassendateien) und, wenn die Seiten ausgeführt wurden.

In ASP.NET 4 standardmäßig Anforderungsvalidierung ist aktiviert, für alle Anforderungen, da vor der Aktivierung der **BeginRequest** Phase einer HTTP-Anforderung. Daher gilt Anforderungsvalidierung auf Anforderungen für alle ASP.NET-Ressourcen nicht nur die ASPX-Seiten-Anforderungen ein. Dies schließt Anforderungen z. B. Aufrufe des Webdiensts und benutzerdefinierte HTTP-Handler. Anforderungsvalidierung ist auch aktiv, wenn benutzerdefinierte HTTP-Module den Inhalt einer HTTP-Anforderung gelesen werden.

Fehler bei Überprüfung der Anforderungen können daher jetzt für Anforderungen auftreten, die zuvor kein Fehler ausgelöst. Um das Verhalten der das Feature zur Validierung von ASP.NET 2.0-Anforderung wiederherzustellen, fügen Sie die folgende Einstellung in der `Web.config` Datei:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Wir empfehlen jedoch, Sie analysieren, um zu bestimmen, ob die vorhandenen Handler, Module oder anderem benutzerdefinierten Code greift auf potenziell unsichere HTTP-Eingaben, die möglicherweise XSS Angriffsvektor Anforderungsvalidierungsfehler.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Standardhashalgorithmus ist jetzt HMACSHA256

ASP.NET verwendet sowohl Verschlüsselung als auch Hashalgorithmen, um Daten wie Formularauthentifizierungscookies und den Ansichtsstatus zu sichern. Standardmäßig verwendet ASP.NET 4 jetzt den HMACSHA256-Algorithmus bei Hashvorgängen für Cookies und den Ansichtszustand. Frühere Versionen von ASP.NET verwendet, den älteren HMACSHA1-Algorithmus.

Ihre Anwendungen möglicherweise beeinflusst werden, wenn das Ausführen von gemischten ASP.NET 2.0/ASP.NET 4 Umgebungen, in denen Daten wie Formularauthentifizierungscookies across.NET Framework-Versionen funktionieren müssen. Um eine ASP.NET 4 Web-Anwendung mit der ältere Algorithmus für die HMACSHA1 konfigurieren zu können, fügen Sie die folgende Einstellung in der `Web.config` Datei:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Fehler im Zusammenhang mit der neuen ASP.NET 4-Stammkonfiguration

Die Dateien der Stamm-Konfiguration (die `machine.config` -Datei und den Stamm `Web.config` Datei) für die .NET Framework 4 (und daher ASP.NET 4) aktualisiert haben die meisten Standardkonfigurationsinformationen, die in ASP.NET 3.5 in gefunden wurde die Anwendung `Web.config` Dateien. Aufgrund der Komplexität der verwalteten IIS 7 und IIS 7.5-Konfiguration-Systeme kann ASP.NET 3.5-Anwendungen unter ASP.NET 4 und IIS 7 und IIS 7.5 ausgeführt in ASP.NET oder IIS Konfigurationsfehler führen.

Es wird empfohlen, dass Sie ASP.NET 3.5-Anwendungen zu ASP.NET 4 aktualisieren, indem mithilfe der Projekt-Upgrade-Tools in Visual Studio 2010 sollte möglichst. Visual Studio 2010 ändert automatisch den ASP.NET 3.5-Anwendung `Web.config` Datei, die die entsprechenden Einstellungen für ASP.NET 4 enthalten.

Es ist jedoch ein unterstütztes Szenario zum Ausführen von ASP.NET 3.5-Anwendungen mit .NET Framework 4 ohne erneute Kompilierung. In diesem Fall möglicherweise manuell ändern, der Anwendung `Web.config` Datei vor dem Ausführen der Anwendung unter .NET Framework 4 und IIS 7 oder IIS 7.5.

In den nächsten beiden Abschnitten beschreiben die Änderungen, die Sie möglicherweise für verschiedene Kombinationen von Software treffen müssen.

**Windows Vista SP1 oder WindowsServer 2008 SP1, auf dem weder Hotfix KB958854 noch SP2 installiert werden.** In dieser Konfiguration führt das Konfigurationssystem von IIS 7 nicht ordnungsgemäß verwalteten Konfiguration einer Anwendung durch einen Vergleich der Anwendungsebene `Web.config` Datei, die ASP.NET 2.0 `machine.config` Dateien. Aufgrund dieser, Anwendungsebene `Web.config` Dateien von .NET Framework 3.5 oder höher benötigen eine **"System.Web.Extensions"** Abschnitt Konfigurationsdefinition (das Element) in der Reihenfolge, dazu führen, dass ein Validierungsfehler für IIS 7 nicht.

Jedoch manuell auf Anwendungsebene geändert `Web.config` -Dateieinträge, die nicht genau der ursprünglichen Codebausteine Abschnitt Konfigurationsdefinitionen entsprechen, die mit Visual Studio 2008 eingeführten führt dazu, dass ASP.NET-Konfigurationsfehler. (Die Standardeinträge für die Konfiguration von den Visual Studio 2008 erstellten ordnungsgemäß funktionieren.) Ein häufiges Problem ist, die manuell geändert `Web.config` Dateien auslassen der **AllowDefinition** und **RequirePermission** Konfigurationsattribute, die auf verschiedenen Konfigurationsabschnitt gefunden werden Definitionen. Dies bewirkt, dass einen Konflikt zwischen den abgekürzten Konfigurationsabschnitt in auf Anwendungsebene `Web.config` Dateien und die vollständige Definition in ASP.NET 4 `machine.config` Datei. Daher löst das ASP.NET 4-Konfigurationssystem zur Laufzeit ein Fehler bei der Konfiguration aus.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2, and also Windows Vista SP1 and Windows Server 2008 SP1 where hotfix KB958854 is installed.**

In diesem Szenario IIS 7 und IIS 7.5 native Konfiguration gibt das System zurück ein Konfigurationsfehler aufgetreten, da er auf einen Textvergleich führt die **Typ** -Attribut, das für einen verwalteten Konfigurationsabschnittshandler definiert ist. Da alle `Web.config` Dateien, die von Visual Studio 2008 und Visual Studio 2008 SP1 generiert werden, haben "3.5", in die Typzeichenfolge für die **"System.Web.Extensions"** (und Verwandte) konfigurationsabschnitthandler, da ASP.NET 4 `machine.config` Datei ist "4.0" in der **Typ** -Attribut für die gleiche konfigurationsabschnitthandler, Anwendungen, die in Visual Studio 2008 oder Visual Studio 2008 SP1, immer generiert werden Konfiguration in IIS 7 Validierungsfehler und IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Zum Beheben dieser Probleme

Die problemumgehung für das erste Szenario ist, aktualisieren Sie die Anwendungsebene `Web.config` Datei einschließlich der Standardtext für die Konfiguration von einem `Web.config` -Datei, die automatisch von Visual Studio 2008 generiert wurde.

Eine alternative problemumgehung für das erste Szenario ist mit Service Pack 2 für Vista oder Windows Server 2008 auf Ihrem Computer zu installieren oder installieren Sie Hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) beheben Sie das falsche Konfiguration-Merge-Verhalten von der IIS-Konfigurationssystem. Jedoch nach dem Sie eine der folgenden Aktionen ausführen, wird Ihre Anwendung wahrscheinlich einen Fehler bei der Konfiguration für das zweite Szenario beschriebenen Gründen auftreten.

Die problemumgehung für das zweite Szenario besteht darin, zu löschen, oder kommentieren Sie all die **"System.Web.Extensions"** Abschnitt Konfigurationsdefinitionen und Konfigurationsabschnitt gruppieren Sie die Definitionen von der Anwendungsebene `Web.config` Datei. Diese Definitionen sind in der Regel am oberen Rand der Anwendungsebene `Web.config` Datei, und identifiziert werden können, indem die **ConfigSections** -Element und seine untergeordneten Elemente.

Bei beiden Szenarien wird empfohlen, dass Sie auch manuell löschen, die **system.codedom** Abschnitt, obwohl dies nicht erforderlich ist.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 untergeordnete Anwendungen werden nicht gestartet unter ASP.NET 2.0 oder ASP.NET 3.5-Anwendungen

ASP.NET 4-Anwendungen, die als untergeordnete Anwendungen konfiguriert wurden, die frühere Versionen von ASP.NET ausführen, können möglicherweise aufgrund von Konfigurations- oder Kompilierungsfehlern nicht starten. Das folgende Beispiel zeigt eine Verzeichnisstruktur für eine Anwendung.

`/parentwebapp` (Verwendung von ASP.NET 2.0 oder ASP.NET 3.5 konfiguriert)  
`/childwebapp` (zur Verwendung von ASP.NET 4 konfiguriert)

Die Anwendung in der `childwebapp` Ordner fehl mit IIS 7 oder IIS 7.5 zu beginnen und gibt einen Fehler bei der Konfiguration. Der Fehlertext enthält eine Meldung ähnlich der folgenden:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

Auf die Anwendung in IIS 6 die `childwebapp` Ordner auch nicht gestartet, aber es wird einen anderen Fehler gemeldet. Der Fehlertext kann z. B. Folgendes angeben:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Diese Szenarien auftreten, da die Konfigurationsinformationen aus der übergeordneten Anwendung in der `parentwebapp` Ordner ist Teil der Hierarchie von Konfigurationsinformationen, die die Einstellungen für die endgültige zusammengeführten Konfiguration bestimmt, die von der untergeordneten Web verwendet werden Anwendung in der `childwebapp` Ordner. Abhängig davon, ob die Webanwendung für ASP.NET 4 für IIS 7 (oder IIS 7.5) oder auf IIS 6 ausgeführt wird wird entweder das IIS-Konfigurationssystem oder das Kompilierungssystem von ASP.NET 4 einen Fehler zurück.

Die Schritte, die Sie befolgen müssen, um dieses Problem zu beheben und die untergeordnete ASP.NET 4-Anwendung zur Zusammenarbeit abzurufen hängen davon ab, ob der ASP.NET 4-Anwendung unter IIS 6 oder IIS 7 (oder IIS 7.5) ausgeführt wird.

### <a name="step-1-iis-7-or-iis-75-only"></a>Schritt 1 (IIS 7 oder IIS 7.5 nur)

Dieser Schritt ist nur für Betriebssysteme, die Ausführung von IIS 7 oder IIS 7.5, einschließlich Windows Vista, Windows Server 2008, Windows 7 und Windows Server 2008 R2 erforderlich.

Verschieben der **ConfigSections** -Definition in der `Web.config` -Datei der übergeordneten Anwendung (die Anwendung, die ASP.NET 2.0 oder ASP.NET 3.5 ausgeführt wird) in das Stammverzeichnis `Web.config` -Datei für.NET Framework 2.0. IIS 7 und IIS 7.5 native Konfigurationssystem durchsucht die **ConfigSections** -Element, wenn sie die Hierarchie von Konfigurationsdateien zusammengeführt. Verschieben der **ConfigSections** Definition aus der übergeordneten Webanwendung `Web.config` Datei im Stammverzeichnis `Web.config` Datei effektiv Blendet das Element aus dem Konfigurationsvorgang für das Zusammenführen, die für die untergeordnete ASP.NET 4 auftritt. die Anwendung.

Auf einem 32-Bit-Betriebssystem oder bei 32-Bit-Anwendungspools Stamm `Web.config` -Datei für ASP.NET 2.0 und ASP.NET 3.5 befindet sich normalerweise im folgenden Ordner:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

Auf einem 64-Bit-Betriebssystem oder für 64-Bit-Anwendungspools, die den Stamm `Web.config` -Datei für ASP.NET 2.0 und ASP.NET 3.5 befindet sich normalerweise im folgenden Ordner:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Wenn Sie sowohl die 32-Bit-als auch die 64-Bit-Webanwendungen auf einem 64-Bit-Computer ausführen, müssen Sie verschieben die **ConfigSections** Element nach oben in Stamm `Web.config` -Dateien für sowohl die 32-Bit-als auch die 64-Bit-Systeme.

Wenn Sie Einfügen der **ConfigSections** Element im Stammverzeichnis `Web.config` Datei, fügen Sie im Abschnitt unmittelbar nach der **Konfiguration** Element. Das folgende Beispiel zeigt, welche den oberen Bereich des Stamms `Web.config` Datei sollte aussehen wie, wann Sie abgeschlossen haben, verschieben die Elemente.

> [!NOTE]
> Im folgenden Beispiel haben Zeilen wurden zur besseren Lesbarkeit umgebrochen.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Schritt 2 (alle Versionen von IIS)

Dieser Schritt ist erforderlich, ob das untergeordnete ASP.NET 4-Webanwendung unter IIS 6 oder IIS 7 (oder IIS 7.5) ausgeführt wird.

In der `Web.config` des übergeordneten Web-Anwendung, die mit ASP.NET 2 oder ASP.NET 3.5-Datei Hinzufügen einer **Speicherort** Tag, das explizit (für IIS und ASP.NET Configuration-Systeme) gibt an, die nur die Konfigurationseinträge gelten Sie für die übergeordnete Webanwendung. Das folgende Beispiel zeigt die Syntax der **Speicherort** Element hinzugefügt werden:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

Das folgende Beispiel zeigt wie die **Speicherort** Tag wird verwendet, um alle Konfigurationsabschnitte, die ab dem **"appSettings"** Abschnitt und endend mit **"System.Webserver"** Abschnitt.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Wenn Sie die Schritte 1 und 2 abgeschlossen haben, werden untergeordnete ASP.NET 4-Webanwendungen ohne Fehler gestartet werden.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 nicht Websites auf Computern gestartet werden, auf dem SharePoint installiert ist

Webserver, auf denen SharePoint ausgeführt haben eine `Web.config` -Datei, die im Stammverzeichnis einer SharePoint-Website bereitgestellt wird (z. B. `c:\inetpub\wwwroot\web.config` für Default Web Site). In diesem `Web.config` SharePoint-Datei eine benutzerdefinierte, partielle Vertrauensebene legt diese Stufe fest mit dem Namen WSS\_Minimal.

Wenn Sie versuchen, eine ASP.NET 4-Website, die bereitgestellt wird als ein untergeordnetes Element dieser Art von SharePoint-Website ausführen, sehen Sie den folgenden Fehler:

`Could not find permission set named 'ASP.NET'.`

Dieser Fehler tritt auf, da Codezugriffssicherheit (CAS) der Infrastruktur für den ASP.NET 4-Code für einen Berechtigungssatz, der mit dem Namen ASP.NET sucht. Die partielle Vertrauenswürdigkeit jedoch Konfigurationsdatei, die von WSS verwiesen wird\_minimale enthält keine Berechtigungssätze mit diesem Namen.

Derzeit ist keine Version von SharePoint zur Verfügung, die mit ASP.NET kompatibel ist. Daher sollten Sie nicht versuchen, eine ASP.NET 4-Website als untergeordneter Standort unter SharePoint-Web Sites ausgeführt.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>Die HttpRequest.FilePath-Eigenschaft enthalten nicht mehr PathInfo-Werte.

Früheren Versionen von ASP.NET enthalten einen **PathInfo** Wert in den Rückgabewert aus verschiedenen pfadbezogener Dateieigenschaften, einschließlich **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, und **HttpRequest.CurrentExecutionFilePath**. ASP.NET 4 nicht mehr umfasst die **PathInfo** Wert in der Rückgabe von Werten aus dieser Eigenschaften. Stattdessen die **PathInfo** Informationen finden Sie in **HttpRequest.PathInfo**. Nehmen wir z.B. das folgenden URL-Fragment:

`/testapp/Action.mvc/SomeAction`

In früheren Versionen von ASP.NET **HttpRequest** Eigenschaften die folgenden Werte aufweisen:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (leer)

In ASP.NET 4 **HttpRequest** Eigenschaften stattdessen die folgenden Werte aufweisen:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 Applications möglicherweise HttpException-Fehler, die auf eurl.axd verweisen generieren

Nachdem ASP.NET 4 für IIS 6 aktiviert wurde, können ASP.NET 2.0-Anwendungen, die auf IIS 6 (in Windows Server 2003 oder Windows Server 2003 R2) ausgeführt, Fehler wie den folgenden generiert:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Dieser Fehler tritt auf, da bei ASP.NET erkennt, dass es sich bei eine Website für die Verwendung von ASP.NET 4 konfiguriert ist, eine native Komponente von ASP.NET 4 eine URL ohne Erweiterung an den verwalteten Teil von ASP.NET zur weiteren Verarbeitung übergibt. Jedoch, wenn die virtuellen Verzeichnisse, die nachstehend sehen Sie eine ASP.NET 4-Website für die Verwendung von ASP.NET 2.0 konfiguriert sind, verarbeitet wird die URL, die in dieser Weise führt eine geänderte URL ohne Erweiterung, die Zeichenfolge "eurl.axd" enthält. Diese geänderte URL wird dann an die ASP.NET 2.0-Anwendung gesendet. ASP.NET 2.0 erkannt nicht das Format "eurl.axd". Aus diesem Grund versucht ASP.NET 2.0 finden Sie eine Datei namens `eurl.axd` und führen Sie es. Da keine solche Datei vorhanden ist, missling die Anforderung mit einem **HttpException** Ausnahme.

Sie können dieses Problem mithilfe einer der folgenden Optionen umgehen.

### <a name="option-1"></a>Option 1

Wenn ASP.NET 4 nicht erforderlich, um die Website ist, ordnen Sie die Website aus, um stattdessen ASP.NET 2.0 zu verwenden.

### <a name="option-2"></a>Option 2

Wenn ASP.NET 4 für das Ausführen der Website erforderlich ist, verschieben Sie alle untergeordneten virtuellen ASP.NET 2.0-Verzeichnisse, auf eine andere Website, die ASP.NET 2.0 zugeordnet ist.

### <a name="option-3"></a>Option 3

Wenn es nicht möglich, auf der Website für ASP.NET 2.0 neu zuordnen oder zum Ändern des Speicherorts eines virtuellen Verzeichnisses ist, sollten Sie die deaktivieren Sie URL ohne Erweiterung, die Verarbeitung in ASP.NET 4 explizit. Verwenden Sie das folgende Verfahren:

1. Öffnen Sie in der Windows-Registrierung den folgenden Knoten:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Erstellen Sie ein neues **DWORD** Wert mit dem Namen **EnableExtensionlessUrls**.
2. Legen Sie **EnableExtensionlessUrls** auf 0. Dadurch wird ohne Erweiterung URL-Verhalten deaktiviert.
3. Speichern Sie den Registrierungswert, und schließen Sie den Registrierungs-Editor.
4. Führen Sie die **"iisreset"** Befehlszeilentool, wodurch IIS für den neuen Registrierungswert zu lesen.

> [!NOTE]
> Festlegen von **EnableExtensionlessUrls** 1 können ohne Erweiterung URL-Verhalten. Dies ist die Standardeinstellung, wenn kein Wert angegeben wird.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Ereignishandler möglicherweise nicht nicht ausgelöst werden in einem Standarddokument in IIS 7 oder IIS 7.5 integrierten-Modus

ASP.NET 4 umfasst Änderungen, die ändern, wie die **Aktion** -Attribut des HTML- **Formular** Element gerendert wird, wenn eine URL ohne Erweiterung in ein Standarddokument aufgelöst wird. Ein Beispiel für eine URL ohne Erweiterung zu ein Standarddokument [ http://contoso.com/ ](http://contoso.com/), wodurch eine Anforderung zum [ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx).

ASP.NET 4 rendert nun den HTML-Code **Formular** des Elements **Aktion** -Attributwert wird als leere Zeichenfolge festgelegt, wenn eine Anforderung an eine URL ohne Erweiterung erfolgt, die ein Standarddokument zugeordnet wurde. In früheren Versionen von ASP.NET eine Anforderung zum beispielsweise [ http://contoso.com ](http://contoso.com) würde eine Anforderung zum `Default.aspx`. In diesem Dokument wurde das öffnende **Formular** Tag wie im folgenden Beispiel gerendert:

`<form action="Default.aspx" />`

In ASP.NET 4 wird eine Anforderung zum [ http://contoso.com ](http://contoso.com) führt auch eine Anforderung zum `Default.aspx`. Allerdings rendert ASP.NET jetzt das öffnende HTML- **Formular** Tag wie im folgenden Beispiel gezeigt:

`<form action="" />`

Dieser Unterschied wie die **Aktion** Attribut wird gerendert, kann dazu führen, dass geringfügige Änderungen in der wie ein Formular-Post von IIS und ASP.NET verarbeitet wird. Wenn die **Aktion** -Attribut ist eine leere Zeichenfolge, die IIS **DefaultDocumentModule** Objekt erstellt eine untergeordnete Anforderung an `Default.aspx`. In den meisten Situationen ist diese untergeordnete Anforderung für Anwendungscode transparent, und die `Default.aspx` Seite wird normal ausgeführt.

Allerdings kann eine mögliche Interaktion zwischen verwaltetem Code und IIS 7 oder IIS 7.5 im integrierten Modus dazu führen, dass verwaltete ASPX-Seiten während der untergeordnete Anforderung nicht mehr ordnungsgemäß ausgeführt werden. Wenn die folgenden Bedingungen auftreten, die untergeordnete Anforderung an eine `Default.aspx` Dokument führt zu einem Fehler oder unerwartetes Verhalten:

1. Eine ASPX-Seite wird gesendet, an den Browser mit der **Formular** des Elements **Aktion** -Attributsatz auf "".
2. Das Formular wird an ASP.NET zurückgesendet.
3. Ein verwaltetes HTTP-Modul liest einen Teil des einheitstextkörpers. Angenommen, ein Modul liest **Request.Form** oder **Request.Params**. Aus diesem Grund wird der Einheitstextkörper der POST-Anforderung im verwalteten Arbeitsspeicher gelesen. Der Einheitstextkörper ist daher nicht mehr für native Codemodule verfügbar, die in IIS 7 oder IIS 7.5 im integrierten Modus ausgeführt werden.
4. Die IIS **DefaultDocumentModule** Objekt schließlich ausgeführt und erstellt eine untergeordnete Anforderung an die `Default.aspx` Dokument. Da der Einheitstextkörper bereits von verwaltetem Code gelesen wurde, kann kein Einheitstextkörper an die untergeordnete Anforderung gesendet werden.
5. Wenn die HTTP-Pipeline ausgeführt wird, für die untergeordnete Anforderung, den Handler für `.aspx` Dateien, die während der Handlerausführungsphase ausgeführt wird.
6. Da es kein einheitstextkörper ist, es gibt keine Formularvariablen und keinen Ansichtszustand und aus diesem Grund wird keine Informationen verfügbar, für den Handler ASPX-Seite, um zu bestimmen, welches Ereignis (sofern vorhanden) ausgelöst werden soll. Daher werden keine Postback-Ereignishandler für die betroffene ASPX-Seite ausgeführt.

Sie können dieses Verhalten umgehen, es gibt folgende Möglichkeiten:

- Identifizieren Sie das HTTP-Modul, das den Text der Anforderung Entität während der Standard-dokumentanforderungen zugreift, und zu bestimmen Sie, ob diese konfiguriert werden kann, nur für verwaltete Anforderungen ausgeführt. Im integrierten Modus für IIS 7 und IIS 7.5 HTTP-Module gekennzeichnet werden können, um nur für verwaltete Anforderungen ausführen, indem Sie das folgende Attribut des Moduls hinzufügen **System.Webserver/Modules** Eintrag:

- `precondition="managedHandler"`

- Diese Einstellung deaktiviert, die das Modul für die fordert, dass IIS 7 und IIS 7.5 bestimmt werden, als nicht verwaltet Anforderungen. Ist für Standard-Dokument-Anforderungen die erste Anforderung an eine URL ohne Erweiterung. Aus diesem Grund wird IIS nicht ausgeführt verwaltete Module, die markiert sind mit eine Vorbedingung für die verwalteten Handler während der Verarbeitung der ursprünglichen Anforderung. Daher verwaltete Module nicht versehentlich Entitätstext gelesen, und daher der Entitätstext ist weiterhin verfügbar und wird übergeben, um die untergeordnete Anforderung und das Standard-Dokument.

- Wenn die problematische HTTP-Module für alle Anforderungen ausführen (für statische Dateien, für URLs ohne Erweiterung, die zum Auflösen der **DefaultDocumentModule** -Objekt, um verwaltete Anforderungen etc.), ändern Sie die betroffene ASPX-Seiten, indem explizit Festlegen der **Aktion** -Eigenschaft der Seite des **System.Web.UI.HtmlControls.HtmlForm** Steuerelement auf eine nicht leere Zeichenfolge. Wenn das Standarddokument ist z. B. `Default.aspx`, ändern Sie den Code der Seite explizit festlegen der **HtmlForm** des Steuerelements **Aktion** Eigenschaft auf "Default.aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Änderungen an der ASP-Code Access Security (CAS)-Implementierung

ASP.NET 2.0, und durch Erweiterung verwenden ASP.NET-Features, die in 3.5 hinzugefügt wurden, die .NET Framework 1.1 und 2.0-Code-Zugriffsmodell für Codezugriffssicherheit (CAS). Die Implementierung von CAS wurde in ASP.NET 4 aber erheblich überholt. Daher können teilweise vertrauenswürdigen ASP.NET-Anwendungen, die abhängig von vertrauenswürdigem Code ausgeführt wird, im globalen Assemblycache (GAC) mit verschiedenen Sicherheitsausnahmen fehlschlagen. Teilweise vertrauenswürdige Anwendungen, die umfangreiche Änderungen an den CAS-Richtlinien des Computers abhängen können auch mit Ausnahmen zur Codezugriffssicherheit fehlschlagen.

Können Sie teilweise vertrauenswürdigen ASP.NET 4-Anwendungen das Verhalten von ASP.NET 1.1 und 2.0, die mit dem neuen Wiederherstellen **LegacyCasModel** -Attribut in der **Vertrauensstellung** Konfigurationselement, wie im folgenden Beispiel dargestellt :

`<trust level= "Medium" legacyCasModel="true" />`

Wenn Sie die legacy-CAS-Modell wiederherstellen, werden die folgenden alten CAS-Verhalten aktiviert:

- Computer-CAS-Richtlinie wird berücksichtigt.
- Es sind mehrere unterschiedliche Berechtigungssätze in einer einzelnen Anwendungsdomäne zulässig.
- Explizite Berechtigung Assertionen sind nicht erforderlich, für Assemblys im GAC, die aufgerufen werden, wenn nur ASP.NET oder andere .NET Framework-Code auf dem Stapel befindet.

Ein Szenario kann nicht wiederhergestellt werden, in .NET Framework 4: nicht-teilweise vertrauenswürdige Anwendungen können nicht mehr bestimmte APIs in "System.Web.dll" und System.Web.Extensions.dll aufrufen. In früheren Versionen von .NET Framework war es möglich, für nicht-Anwendungen mit eingeschränkter Vertrauenswürdigkeit explizit gewährt werden <strong>überschneidet AspNetHostingPermission</strong> Berechtigungen. Diese Anwendungen können dann <strong>System.Web.HttpUtility</strong>, Typen in der <strong>System.Web.ClientServices.\< / strong > *-Namespaces und Typen im Zusammenhang mit Mitgliedschaft, Rollen und Profile. Diese Typen von Anwendungen mit teilweiser Vertrauenswürdigkeit nicht-aufrufen ist in .NET Framework 4 nicht mehr unterstützt.

> [!NOTE]
> Die **HtmlEncode** und **HtmlDecode** Funktionalität der **System.Web.HttpUtility** Klasse wurde verschoben, um die neuen .NET Framework 4  **System.Net.WebUtility** Klasse. War, die die einzige ASP.NET-Funktionalität, die verwendet wurde, ändern Sie den Code der Anwendung zur Verwendung der neuen **WebUtility** stattdessen.


Im folgenden finden eine Zusammenfassung der Änderungen an der Standard-CAS-Implementierung in ASP.NET 4:

- ASP.NET-Anwendungsdomänen sind jetzt Homogene Anwendungsdomänen. Nur teilweise Vertrauenswürdigkeit und voller Vertrauenswürdigkeit gewähren Mengen sind in einer Anwendungsdomäne verfügbar.
- ASP.NET sind teilweise Vertrauenswürdigkeit gewähren unabhängig von jedem CAS-Richtlinie auf Benutzerebene, Computerebene oder auf Unternehmensebene.
- ASP.NET-Assemblys, die in 3.5 und 3.5 SP1 geliefert haben konvertiert wurde, um das Transparenzmodell der .NET Framework 4 zu verwenden.
- Verwendung von ASP.NET **überschneidet AspNetHostingPermission** Attribut wurde erheblich reduziert. Die meisten Instanzen von diesem Attribut wurden aus der öffentlichen ASP.NET APIs entfernt.
- Dynamisch kompilierte Assemblys, die vom Buildanbieter für ASP.NET erstellt werden wurden aktualisiert, um explizit Assemblys als transparent kennzeichnen.
- Alle ASP.NET-Assemblys werden nun so markiert, die das APTCA-Attribut nur in Webhostingumgebungen berücksichtigt wird. Teilweise vertrauenswürdige nicht-Hostumgebungen wie ClickOnce werden kann nicht in ASP.NET-Assemblys aufgerufen.

Weitere Informationen zu den neuen ASP.NET 4 Codezugriff-Sicherheitsmodell, finden Sie unter [Using Code Access Security in ASP.NET-Anwendungen](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) auf der MSDN-Website.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser und anderen Typen in der System.Web.Security-Namespace wurden verschoben

Einige Typen, die in der ASP.NET-Mitgliedschaft verwendet werden von verschoben `System.Web.dll` auf die neue "System.Web.ApplicationServices.dll"-Assembly. So sollten architektonische Schichtabhängigkeiten zwischen Typen im Client und erweiterten .NET Framework-SKUs gelöst werden.

Websiteprojekte müssen Probleme durch Verschieben dieser Typen keine, da "System.Web.ApplicationServices.dll" hinzugefügt wurde, um die Liste der referenzierten Assemblys, die standardmäßig verwendet wird durch das Kompilierungssystem von ASP.NET. Wenn Sie eine Website-Projekt, die mit einer früheren Version von ASP.NET zu ASP.NET 4 aktualisieren durch Öffnen in Visual Studio 2010 erstellt wurde, wird das Projekt ohne Fehler kompiliert.

Auf ähnliche Weise, wenn Sie ein Webanwendungsprojekt, die in einer früheren Version von ASP.NET zu ASP.NET 4 erstellt wurde aktualisieren, indem Sie sie in Visual Studio 2010 öffnen, die Verfahren beim Upgrade einen Verweis auf "System.Web.ApplicationServices.dll" dem Projekt hinzugefügt. Aus diesem Grund aktualisiert Web-Anwendungsprojekte auch ohne Fehler kompiliert werden.

Kompilierte (binäre) Dateien, die mit früheren Versionen von ASP.NET erstellt wurden auch führt ohne Fehler in ASP.NET 4, obwohl die Mitgliedschaftstypen in einer anderen Assembly verschoben wurden. Typweiterleitung Informationen wurde auf die ASP.NET 4-Version von `System.Web.dll` Runtime-Verweise für diese Typen, die automatisch an den neuen Speicherort für die Typen weiterleitet.

Allerdings können Klassenbibliotheken, die bestimmte Mitgliedschaftstypen verwenden, und, die von früheren Versionen von ASP.NET aktualisiert wurden, nicht kompilieren, wenn in einem ASP.NET 4-Projekt verwendet. Beispielsweise kann ein Klassenbibliotheksprojekt passieren kompilieren und meldet einen Fehler wie den folgenden:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Sie können dieses Problem umgehen, indem Sie einen Verweis in Ihr Klassenbibliotheksprojekt "System.Web.ApplicationServices.dll" hinzufügen.

Die folgende Liste zeigt die *System.Web.Security* Typen, die vom verschoben wurden `System.Web.dll` System.Web.ApplicationServices.dll:

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

In ASP.NET 1.0 zwischengespeicherte aufgrund ein Fehlers Seiten, die angegeben `Location="ServerAndClient"` als Ausgabecache-Einstellung zum Ausgeben einer `Vary:*` HTTP-Header in der Antwort. So wurde Clientbrowsern mitgeteilt, dass Seiten nie lokal zwischengespeichert werden sollen.

In ASP.NET 1.1 die **System.Web.HttpCachePolicy.SetOmitVaryStar** Methode wurde hinzugefügt, das können Sie aufrufen, um das Unterdrücken der `Vary:*` Header. Diese Methode wurde gewählt, da ändern den ausgegebenen HTTP-Header wurde eine potenziell wichtige Änderung zum Zeitpunkt. Jedoch vom Verhalten in ASP.NET haben Entwickler verwirrt, und legen Problemberichte nahe, dass Entwickler keine Kenntnis von dem **SetOmitVaryStar** Verhalten.

In ASP.NET 4 wurde die Entscheidung getroffen, um die Stamm-Problem zu beheben. Die `Vary:*` HTTP-Header wird nicht mehr in Antworten, die angeben, die folgende Anweisung ausgegeben:

`<%@OutputCache Location="ServerAndClient" %>`

Daher **SetOmitVaryStar** wird nicht mehr benötigt, um das Unterdrücken der `Vary:*` Header.

In Anwendungen, die angeben, `Location="ServerAndClient"` in die **@ OutputCache** Direktive auf einer Seite, sehen Sie nun das Verhalten, die durch den Namen des impliziert die **Speicherort** Wert des Attributs, Seiten werden im Browser ohne, die Sie aufrufen zwischengespeichert der **SetOmitVaryStar** Methode.

Wenn Seiten in Ihrer Anwendung ausgeben müssen `Vary:*`, rufen Sie die **AppendHeader** -Methode, wie im folgenden Beispiel gezeigt:

`HttpResponse.AppendHeader("Vary","*");`

Alternativ können Sie ändern den Wert der Ausgabecache **Speicherort** -Attribut auf "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>System.Web.Security sind für Passport veraltet

In ASP.NET 2.0 integrierte Unterstützung für Passport seit einigen Jahren aufgrund von Änderungen in Passport (jetzt Live ID) veraltet und wird nicht unterstützt. Daher die fünf Typen in Zusammenhang mit Passport in **System.Web.Security** sind jetzt mit markiert die **ObsoleteAttribute** Attribut.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>Die Eigenschaft MenuItem.PopOutImageUrl Rendern ein Bild in ASP.NET 4 ist ein Fehler

In ASP.NET 3.5 die *MenuItem.PopOutImageUrl* -Eigenschaft können Sie die URL für ein Bild angeben, die angezeigt wird, in einem Menüelement, um anzugeben, dass das Menüelement über ein dynamisches Untermenü verfügt. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft in Markup in ASP.NET 3.5 angeben.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

Als Ergebnis einer entwurfsänderung in ASP.NET 4, wird keine Ausgabe für gerendert der *PopOutImageUrl* , wenn die Eigenschaft, für festgelegt ist die *"MenuItem"* Klasse. Stattdessen müssen Sie angeben, einen Bild-URL direkt in die *Menü* steuern, verwenden entweder die *StaticPopOutImageUrl* Eigenschaft oder das *DynamicPopOutImageUrl* Eigenschaft. Bei der Arbeit mit einem statischen Menü, das *Menu.StaticPopOutImageUrl* -Eigenschaft gibt die URL für ein Bild, das angezeigt wird, um anzugeben, dass das statische Menüelement über ein Untermenü verfügt, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Wenn Sie mit einem dynamischen Menü arbeiten, verwenden Sie die *Menu.DynamicPopOutImageUrl* Eigenschaft, um die URL für ein Bild anzugeben, der angibt, dass ein dynamisches Menüelement über ein Untermenü verfügt. Im folgende Beispiel ähnelt dem vorherigen Beispiel, aber es zeigt, wie die *DynamicPopOutImageUrl* -Eigenschaft für ein dynamisches Menü.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Wenn die *Menu.DynamicPopOutImageUrl* Eigenschaft nicht festgelegt ist und die *Menu.DynamicEnableDefaultPopOutImage* -Eigenschaftensatz auf *"false"*, kein Bild angezeigt wird. Auf ähnliche Weise, wenn die *StaticPopOutImageUrl* Eigenschaft nicht festgelegt ist und die *StaticEnableDefaultPopOutImage* -Eigenschaftensatz auf *"false"*, kein Bild angezeigt.

Wenn Sie die Pfade für diese Eigenschaften festlegen, verwenden Sie einen Schrägstrich (/) anstelle von einem umgekehrten Schrägstrich (\). Weitere Informationen finden Sie unter [Menu.StaticPopOutImageUrl und Rendern von Images beim Pfade enthalten umgekehrte Schrägstriche nicht Menu.DynamicPopOutImageUrl](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") an anderer Stelle in diesem Dokument.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu.StaticPopOutImageUrl und Menu.DynamicPopOutImageUrl keine Bilder darstellen, wenn die Pfade umgekehrten Schrägstriche enthalten.

In ASP.NET 4 die Images, die Sie angeben, mit der *Menu.StaticPopOutImageUrl* und *Menu.DynamicPopOutImageUrl* Eigenschaften werden nicht gerendert, wenn der Pfad Backlashes enthält (\). Dies ist eine Änderung gegenüber früheren Versionen von ASP.NET.

Im folgenden Beispiel *Menü* -Steuerelement Markup zeigt die *StaticPopOutImageUrl* -Eigenschaft festgelegt wird, verwenden einen Pfad, einen umgekehrten Schrägstrich enthält. In ASP.NET 4 wird das Bild in der Eigenschaft angegebenen nicht gerendert.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Um dieses Problem zu beheben, ändern Sie die Pfadwerte angeben, die im angegebenen die *StaticPopOutImageUrl* und *DynamicPopOutImageUrl* Eigenschaften, die Schrägstriche (/) verwendet werden. Das folgende Beispiel zeigt diese Änderung:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Beachten Sie, dass Anwendungen, die von früheren Versionen von ASP.NET zu ASP.NET 4 migriert wurden, auch kann beeinflusst werden, da die *MenuItem.PopOutImageUrl* -Eigenschaft geändert wurde. Weitere Informationen finden Sie unter [der MenuItem.PopOutImageUrl-Eigenschaft kein Bild in ASP.NET 4 gerendert](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") an anderer Stelle in diesem Dokument.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor dem kommerziellen Release der beschriebenen Software ggf. erheblich geändert wird.

Die Informationen in diesem Dokument repräsentieren den aktuellen Standpunkt der Microsoft Corporation zu den erörterten Problemen zum Veröffentlichungstermin. Da Microsoft auf das Ändern von Marktlagen reagieren muss, ist das Dokument keinesfalls als Verpflichtung von Microsoft zu interpretieren, und Microsoft kann die Genauigkeit der Informationen nicht über den Zeitpunkt der Veröffentlichung hinaus garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation kein Teil dieses Dokuments für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, die Firmen, Organisationen, Produkte, Domänennamen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse in diesen Beispielen sind frei erfunden, und mit jeder Firmen, Unternehmen, Produkt, Domänennamen, e-Mail-Adresse Adresse ","-Logo "," Person "," Ort "oder" Ereignis richtet sich an, oder rein zufällig.

© 2010 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
