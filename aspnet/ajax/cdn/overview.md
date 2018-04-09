---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: bc5f40746ad6b1ed8a74bcb75def9ff8f08fb789
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
<a name="microsoft-ajax-content-delivery-network"></a>Microsoft Ajax Content Delivery Network
====================
> [!WARNING]
> Produktionsanwendungen geboten harte Abhängigkeiten nicht auf dem CDN-Objekte. Anwendungen sollten Testen für die CDN-Anlage, die auf die verwiesen wird, und ein fallback Medienobjekt verwenden, wenn das CDN nicht verfügbar ist. 
>
> Das Microsoft Ajax-CDN ist keine SLA-Größe mit einer Azure-CDN.
>
> Verwendung [das GitHub-Problem](https://github.com/aspnet/Docs/issues/5832) zum Melden von Problemen mit der Microsoft Ajax-CDN.

## <a name="table-of-contents"></a>Inhaltsverzeichnis

**[AJAX.Microsoft.com in ajax.aspnetcdn.com umbenannt](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**  
**[Unterstützung von Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**  
**[Mithilfe von ASP.NET Ajax vom CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**  
**[Unter Verwendung von jQuery vom CDN](#Using_jQuery_from_the_CDN_21)**  
**[Unter Verwendung von jQuery UI vom CDN](#Using_jQuery_UI_from_the_CDN_22)**  
**[Drittanbieter-Dateien auf dem CDN](#Third-Party_Files_on_the_CDN_23)**  
  
 [jQuery-Versionen, auf das CDN](#jQuery_Releases_on_the_CDN_0)  
 [Migrieren von jQuery-Versionen, auf das CDN](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [jQuery UI-Versionen, auf das CDN](#jQuery_UI_Releases_on_the_CDN_2)  
 [jQuery-Validierung Releases auf das CDN](#jQuery_Validation_Releases_on_the_CDN_3)  
 [jQuery Mobile-Versionen, auf das CDN](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [jQuery-Vorlagen-Versionen, auf das CDN](#jQuery_Templates_Releases_on_the_CDN_5)  
 [jQuery-Zyklus Releases auf das CDN](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [jQuery DataTables Releases auf das CDN](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [Modernizr Releases auf das CDN](#Modernizr_Releases_on_the_CDN_8)  
 [JSHint Releases auf das CDN](#JSHint_Releases_on_the_CDN_10)  
 [Knockout Releases auf das CDN](#Knockout_Releases_on_the_CDN_11)  
 [Bei der Globalisierung Releases auf das CDN](#Globalize_Releases_on_the_CDN_12)  
 [Reagieren Sie auf das CDN Versionen](#Respond_Releases_on_the_CDN_13)  
 [Bootstrap-Versionen auf das CDN](#Bootstrap_Releases_on_the_CDN_14)  
 [Bootstrap TouchCarousel Releases auf das CDN](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [Hammer.js Releases auf das CDN](#Hammerjs_Releases_on_the_CDN_19)  
 [ASP.NET Web Forms und Ajax-Versionen, auf das CDN](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [ASP.NET MVC frei, auf das CDN](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [ASP.NET SignalR frei, auf das CDN](#ASPNET_SignalR_Releases_on_the_CDN_17)

Der Microsoft Ajax Content Delivery Network (CDN) hostet beliebten Drittanbieter-JavaScript-Bibliotheken wie z. B. jQuery und ermöglicht es Ihnen, einfach die Webanwendungen hinzufügen. Sie können z. B. starten, unter Verwendung von jQuery der gehostet wird auf diesem CDN einfach durch Hinzufügen einer &lt;Skript&gt; -Tag, um die Seite, die auf ajax.aspnetcdn.com verweist.

Durch das CDN nutzen, können Sie die Leistung der Ajax-Anwendungen erheblich verbessern. Auf Servern, die auf der ganzen Welt werden der Inhalt des CDN zwischengespeichert. Darüber hinaus ermöglicht das CDN Browsern zum Wiederverwenden von zwischengespeicherten Drittanbieter-JavaScript-Dateien für Websites, die in unterschiedlichen Domänen befinden.

Das CDN unterstützt SSL (HTTPS) an, für den Fall, dass Sie einer Webseite mithilfe des Secure Sockets Layers bereitstellen müssen.

Das CDN hostet die folgenden Drittanbieter-Skriptbibliotheken hochgeladen worden sein, und für Sie lizenziert sind, die von den Besitzern von diesen Bibliotheken:

- jQuery (www.jquery.com)
- jQuery UI (www.jqueryui.com)
- jQuery Mobile (www.jquerymobile.com)
- jQuery-Validierung (www.jquery.com)
- jQuery-Zyklus (www.malsup.com/jquery/cycle/)
- jQuery DataTables)http://datatables.net/)

Das Microsoft Ajax-CDN umfasst außerdem die folgenden Bibliotheken, die von Microsoft hochgeladen wurden:

- ASP.NET Ajax
- ASP.NET MVC JavaScript Files
- ASP.NET SignalR JavaScript-Dateien

Microsoft erhebt keine Ansprüche für den Besitz von Drittanbieter-Bibliotheken, die auf diesem CDN gehostet. Diese Bibliotheken sind den Urheberrechtsinhabern der Bibliotheken entschieden haben. Alle Rechte, die Sie möglicherweise herunterladen und verwenden diese Bibliotheken werden ausschließlich von der jeweiligen Urheberrechtsinhabern gewährt. Da keine Microsoft-Bibliotheken sind, bietet Microsoft keine GEWÄHRLEISTUNGEN oder geistiges Eigentum Rechte Lizenzen (einschließlich keine implizite Patentrechte), für die Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.

Wenn Sie JavaScript-Bibliothek senden möchten, und die Bibliothek eine von der obersten JavaScript-Bibliotheken ist (wie aufgeführt http://trends.builtwith.com) oder Erweiterungen/Plugins an diese Bibliotheken, die (a) beliebten; oder (b) für die Verwendung in ASP.NET hilfreich sind, dann wenden Sie sich an AjaxCDNSubmission@Microsoft.com.

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a>AJAX.Microsoft.com in ajax.aspnetcdn.com umbenannt

Das CDN verwendet, um den Domänennamen "Microsoft.com" verwenden und wurde geändert, um den Domänennamen aspnetcdn.com verwenden. Diese Änderung wurde vorgenommen, um die Leistung zu erhöhen, da bei ein Browser mit die Domäne "Microsoft.com" verwiesen wird er alle Cookies aus dieser Domäne über das Netz mit jeder Anforderung sendet. Durch das Umbenennen eines Domänennamens als "Microsoft.com" kann die Leistung von möglichst alle Aufgaben auf 25 % erhöht werden. Beachten Sie ajax.microsoft.com wird zwar weiterhin ajax.aspnetcdn.com wird jedoch empfohlen.

- Altes Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js
- Neues Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a>Unterstützung von Visual Studio .vsdoc

Die .vsdoc Dateien ordnungsgemäß zu mit Visual Studio 2008 verwenden, müssen Sie sicherstellen, dass Sie Visual Studio 2008 SP1 verfügen, und der Hotfix für das Vsdoc-Dateien installiert. Sie können diese hier abrufen:

- [Herunterladen der Visual Studio 2008 SP1](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Herunterladen der Visual Studio 2008 SP1")
- [Herunterladen von .vsdoc Hotfix für Visual Studio 2008 SP1](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc Hotfix für Visual Studio 2008 SP1 herunterladen")

Visual Studio 2010 unterstützt .vsdoc Dateien ohne zusätzliche Patches.

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a>Mithilfe von ASP.NET Ajax vom CDN

Wenn ASP.NET 4 zu verwenden, können Sie alle Anforderungen für ASP.NET Frameworkskripts auf das CDN umleiten. Abrufen von Skripts aus dem CDN anstelle von Ihren lokalen Webserver kann die Leistung von öffentlichen ASP.NET-Websites erheblich verbessern.

Verwenden Sie die ScriptManager EnableCDN-Eigenschaft, um alle ASP.NET Framework Skript Anforderungen an das Microsoft Ajax-CDN umleiten:

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a>Unter Verwendung von jQuery vom CDN

Sie können die jQuery-Skripts, das durch Hinzufügen der folgenden Script-Element zu einer Seite in der Webanwendung auf CDN gehostet:

[!code-html[Main](overview/samples/sample2.html)]

Das CDN enthält auch die verkleinerte Version des jQuery-Skripts, die Sie abrufen können mithilfe des folgenden Elements:

[!code-html[Main](overview/samples/sample3.html)]

Damit wird die Seite fallback auf jQuery aus einem lokalen Pfad auf eine eigene Website laden, wenn das CDN nicht verfügbar ist, erfolgt, fügen Sie das folgende Element sofort nach dem Element, das Verweisen auf das CDN hinzu:

[!code-html[Main](overview/samples/sample4.html)]

Die folgenden Beispielseite verwendet die CDN-Version der jQuery-Bibliothek (mit Fallback auf eine lokale Kopie), zeigen Sie den Inhalt von einem Div-Element, wenn auf eine Schaltfläche geklickt wird.

[!code-html[Main](overview/samples/sample5.html)]

Sie können erfahren Sie mehr über jQuery und eine lokale Kopie des jQuery herunterzuladen, besuchen die [jQuery](http://jquery.com/) Website.

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a>Unter Verwendung von jQuery UI vom CDN

Das CDN hostet auch die jQuery UI-Bibliothek. Die jQuery UI-Bibliothek enthält einen umfangreichen Satz von Widgets und Effekte, die Sie in Ihrer ASP.NET-Anwendungen verwenden können. Die folgende Seite wird beispielsweise veranschaulicht, wie Sie dem jQuery UI Datepicker im Kontext einer ASP.NET Web Forms-Anwendung verwenden können, um ein Popupkalender angezeigt:

[!code-aspx[Main](overview/samples/sample6.aspx)]

Wenn Sie den Fokus auf das Textfeld mit der Tastatur zu verschieben, wird ein Kalender angezeigt:

![Erstellt mit Datepicker-Popupkalenders](overview/_static/image1.png)

Beachten Sie, dass im obigen Code drei Dateien aus dem CDN aufnehmen müssen:

- Die jQuery-Bibliothek &mdash; die jQuery UI-Bibliothek für die jQuery-Bibliothek abhängig ist. Sie müssen die jQuery-Bibliothek auf der Seite vor dem Hinzufügen der jQuery UI-Bibliothek hinzufügen.
- Der jQuery UI-Bibliothek &mdash; die jQuery UI-Bibliothek enthält alle Benutzeroberflächeneffekte jQuery und Widgets, z. B. das Datepicker-Widget in der oben genannten Seite verwendet.
- Ein Design der jQuery UI &mdash; jQuery UI unterstützt verschiedene Designs. Die Seite enthält einen Link in einer CSS-Datei, die das Design "Redmond" zu importieren.

Alle von der standardmäßigen jQuery UI-Designs werden auf das CDN gehostet. [Besuchen Sie diese Seite](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 auf das Microsoft Ajax-CDN") Miniaturansichten für jeden Design anzeigen.

Weitere Informationen zu der jQuery UI-Bibliothek finden Sie auf der offiziellen [jQuery UI Website](http://jQueryUI.com "jQuery UI Website").

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a>Drittanbieter-Dateien auf dem CDN

Das CDN hostet einiger der beliebtesten JavaScript-Bibliotheken von Drittanbietern. Microsoft erhebt keine Ansprüche für den Besitz von Drittanbieter-Bibliotheken, die auf diesem CDN gehostet. Diese Bibliotheken sind den Urheberrechtsinhabern der Bibliotheken entschieden haben. Alle Rechte, die Sie möglicherweise herunterladen und verwenden diese Bibliotheken werden ausschließlich von der jeweiligen Urheberrechtsinhabern gewährt. Da keine Microsoft-Bibliotheken sind, bietet Microsoft keine GEWÄHRLEISTUNGEN oder geistiges Eigentum Rechte Lizenzen (einschließlich keine implizite Patentrechte), für die Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a>jQuery-Versionen, auf das CDN

Die folgenden Versionen von jQuery, die auf das CDN gehostet werden:

#### <a name="jquery-version-331"></a>jQuery version 3.3.1
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a>jQuery version 3.2.1
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a>jQuery version 3.2.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a>jQuery-Version 3.1.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a>jQuery-Version 3.1.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a>jQuery version 3.0.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a>jQuery version 2.2.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a>jQuery-Version 2.2.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a>jQuery-Version 2.2.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a>jQuery-Version 2.2.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a>jQuery-Version 2.2.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a>jQuery-Version 2.1.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a>jQuery-Version 2.1.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a>jQuery-Version 2.1.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a>jQuery-Version 2.1.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a>jQuery-Version 2.1.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a>jQuery-Version 2.0.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a>jQuery-Version 2.0.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a>jQuery-Version 2.0.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a>jQuery-Version 2.0.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a>jQuery-Version 1.12.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a>jQuery-Version 1.12.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a>jQuery-Version 1.12.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a>jQuery-Version 1.12.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a>jQuery-Version 1.12.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a>jQuery-Version 1.11.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a>jQuery-Version 1.11.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a>jQuery-Version 1.11.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a>jQuery-Version 1.11.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a>Version der jQuery 1.10.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a>jQuery-Version 1.10.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a>jQuery-Version 1.10.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a>jQuery-Version 1.9.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a>jQuery version 1.9.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a>jQuery-Version 1.8.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a>jQuery-Version 1.8.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a>jQuery-Version 1.8.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a>jQuery-Version 1.8.0

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a>jQuery-Version 1.7.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a>jQuery-Version 1.7.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a>jQuery-Version 1.7

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a>jQuery-Version 1.6.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a>jQuery-Version 1.6.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a>jQuery-Version 1.6.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a>jQuery-Version 1.6.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a>jQuery-Version 1.6

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a>jQuery version 1.5.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a>jQuery version 1.5.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a>jQuery-Version 1.5

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a>jQuery-Version 1.4.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a>jQuery-Version 1.4.3

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a>jQuery-Version 1.4.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a>jQuery-Version 1.4.1

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a>jQuery-Version 1.4

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a>Version der jQuery 1.3.2

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a>Migrieren von jQuery-Versionen, auf das CDN

Die folgenden Versionen von jQuery migrieren, die auf das CDN gehostet werden:

#### <a name="jquery-migrate-version-300"></a>jQuery migrieren Version 3.0.0

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a>jQuery migrieren Version 1.2.1

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

jQuery migrieren Version 1.2.0

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a>jQuery migrieren Version 1.1.1

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a>jQuery migrieren Version 1.1.0

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a>jQuery migrieren Version 1.0.0

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a>jQuery UI-Versionen, auf das CDN

Die folgenden Versionen der jQuery UI-Bibliothek, die auf diesem CDN gehostet werden. Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.

- [jQuery UI 1.12.1](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.12.0](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.11.4](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.11.3](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.11.2](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.11.1](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.11.0](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.10.4](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.10.3](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.10.2](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2-Benutzeroberfläche auf das Microsoft Ajax-CDN")
- [jQuery UI 1.10.1](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.10.0](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.9.2](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.9.1](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.9.0](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.24](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.23](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.22](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.21](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.20](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.19](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.18](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.17](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.16](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.15](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.14](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.13](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.12](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.11](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.10](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.9](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.8](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.7](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.6](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 auf das Microsoft Ajax-CDN")
- [jQuery UI 1.8.5](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a>jQuery-Validierung Releases auf das CDN

Die folgenden Versionen der jQuery-Validierung-Bibliothek, die auf diesem CDN gehostet werden. Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.

- [jQuery Validate 1.17.0](jquery-validate/cdnjqueryvalidate1170.md "jQuery Validation 1.17.0")
- [jQuery Validate 1.16.0](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [jQuery Validate 1.15.1](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [jQuery Validate 1.15.0](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [jQuery Validate 1.14.0](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [jQuery Validate 1.13.1](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [jQuery Validate 1.13.0](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [jQuery Validate 1.12.0](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [jQuery Validate 1.11.1](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [jQuery Validate 1.11.0](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [jQuery Validate 1.10.0](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [jQuery Validate 1.9](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [jQuery Validate 1.8.1](jquery-validate/cdnjqueryvalidate181.md "jquery.validate version 1.8.1")
- [jQuery Validate 1.8](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [jQuery Validate 1.7](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [jQuery Validate 1.6](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [jQuery Validate 1.5.5](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a>jQuery Mobile-Versionen, auf das CDN

Die folgenden Versionen der jQuery Mobile-Bibliothek, die auf diesem CDN gehostet werden. Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.

- [jQuery Mobile 1.4.5](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.4.2](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 on the Microsoft Ajax CDN")
- [jQuery Mobile 1.4.1](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 on the Microsoft Ajax CDN")
- [jQuery Mobile 1.4.0](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 on the Microsoft Ajax CDN")
- [jQuery Mobile 1.3.2](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 on the Microsoft Ajax CDN")
- [jQuery Mobile 1.3.1](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.3.0](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.2.0](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.1.2](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.1.1](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.1.0](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.1.0 RC 2](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.0.1](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 on the Microsoft Ajax CDN")
- [jQuery Mobile 1.0](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.0 RC 2](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.0 RC 1](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 auf das Microsoft Ajax-CDN")
- [jQuery Mobile 1.0 Beta 3](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 Klicken Sie auf das Microsoft Ajax-CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a>jQuery-Vorlagen-Versionen, auf das CDN

Die folgenden Versionen der jQuery-Vorlagen-Plug-Ins, die auf diesem CDN gehostet werden. Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.

- [jQuery-Vorlagen Beta 1](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery-Vorlagen Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a>jQuery-Zyklus Releases auf das CDN

Die folgenden Versionen der jQuery-Zyklus-Plug-Ins, die auf diesem CDN gehostet werden. Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.

- [jQuery-Zyklus 2,99](jquery-cycle/cdnjquerycycle299.md "jQuery Zyklus 2,99")
- [jQuery-Zyklus 2.94](jquery-cycle/cdnjquerycycle294.md "jQuery Zyklus 2.94")
- [jQuery-Zyklus 2,88](jquery-cycle/cdnjquerycycle288.md "jQuery Zyklus 2,88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a>jQuery DataTables Releases auf das CDN

Die folgenden Versionen der jQuery DataTables-Plug-Ins, die auf diesem CDN gehostet werden. Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.

- [jQuery DataTables 1.10.5](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [jQuery DataTables 1.10.4](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [jQuery DataTables 1.9.4](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [jQuery DataTables 1.9.3](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [jQuery DataTables 1.9.2](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [jQuery DataTables 1.9.1](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [jQuery DataTables 1.9.0](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [jQuery DataTables 1.8.2](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a>Modernizr Releases auf das CDN

Die folgenden Versionen von [Modernizr](http://www.modernizr.com "Modernizr") auf das CDN gehostet werden:

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a>JSHint Releases auf das CDN

Die folgenden Versionen von [JSHint](http://www.jshint.com "JSHint") auf das CDN gehostet werden:

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a>Knockout Releases auf das CDN

Die folgenden Versionen von [Knockout](http://www.knockoutjs.com "Knockout") auf das CDN gehostet werden:

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a>Bei der Globalisierung Releases auf das CDN

Die folgenden Versionen von [Globalize](https://github.com/jquery/globalize "Globalize") auf das CDN gehostet werden:

#### <a name="globalize-version-100"></a>Globalize version 1.0.0

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a>Bei der Globalisierung Version 0.1.1

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - alle Kulturen
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - Ersetzen Sie "{Kulturcode}" durch den gewünschten Kulturcode, z. B. Microsoft globalize.culture.en GB.js== Dateien auf das CDN == diese Bibliotheken von Microsoft hochgeladen wurden.

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a>Reagieren Sie auf das CDN Versionen

Die folgenden Versionen von [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") reagieren auf das CDN gehostet werden:

#### <a name="respond-version-142"></a>Version 1.4.2 reagieren

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a>Version 1.4.1 reagieren

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a>Version 1.4.0 reagieren

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a>Version 1.3.0 reagieren

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a>Die Version 1.2.0 reagieren

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a>Bootstrap-Versionen auf das CDN

Die folgenden Versionen von [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") Bootstrap auf dem CDN gehostet werden:

#### <a name="bootstrap-version-400"></a>Bootstrap Version 4.0.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a>Bootstrap Version 3.3.7

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a>Bootstrap Version 3.3.6

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a>Bootstrap Version 3.3.5

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a>Bootstrap Version 3.3.4

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a>Bootstrap Version 3.3.2

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a>Bootstrap Version 3.3.1

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a>Bootstrap Version 3.3.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a>Bootstrap Version 3.2.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a>Bootstrap Version 3.1.1

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a>Bootstrap Version 3.1.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a>Bootstrap Version 3.0.3.

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a>Bootstrap Version 3.0.2

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a>Bootstrap Version 3.0.1

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a>Bootstrap-Version 3.0.0

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a>Bootstrap-Version 2.3.2

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a>Bootstrap Version 2.3.1

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a>Bootstrap TouchCarousel Releases auf das CDN

Die folgenden Versionen von [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") Bootstrap TouchCarousel Versionen auf das CDN gehostet werden:

#### <a name="bootstrap-touchcarousel-version-080"></a>Bootstrap TouchCarousel Version 0.8.0

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a>Hammer.js Releases auf das CDN

Die folgenden Versionen von [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js Versionen auf das CDN gehostet werden:

#### <a name="hammerjs-version-204"></a>Hammer.js Version 2.0.4

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a>ASP.NET Web Forms und Ajax-Versionen, auf das CDN

Die folgenden Versionen von ASP.NET Ajax-Bibliothek, die auf das CDN gehostet werden. Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.

- [ASP.NET Web Forms- und Ajax-Version 4.5.2](cdnajax452.md "ASP.NET Web Forms- und Ajax 4.5.2")
- [ASP.NET Web Forms- und Ajax-Version 4](cdnajax4.md "ASP.NET Web Forms- und Ajax 4")
- [ASP.NET Ajax Version 3.5](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a>ASP.NET MVC frei, auf das CDN

Die folgenden ASP.NET MVC-JavaScript-Dateien, die auf diesem CDN gehostet werden:

#### <a name="aspnet-mvc-523"></a>ASP.NET MVC 5.2.3

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a>ASP.NET MVC 5.1

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a>ASP.NET MVC 5.0

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a>ASP.NET MVC 4.0

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a>ASP.NET MVC 3.0

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a>ASP.NET MVC 2.0

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a>ASP.NET MVC 1.0

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a>ASP.NET SignalR frei, auf das CDN

Die folgenden ASP.NET SignalR JavaScript-Dateien, die auf diesem CDN gehostet werden:

#### <a name="aspnet-signalr-222"></a>ASP.NET SignalR 2.2.2

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a>ASP.NET SignalR 2.2.1

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a>ASP.NET SignalR 2.2.0

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a>ASP.NET SignalR 2.1.0

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a>ASP.NET SignalR 2.0.3

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a>ASP.NET SignalR 2.0.1

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a>ASP.NET SignalR 2.0.0

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a>ASP.NET SignalR 1.1.3

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a>ASP.NET SignalR 1.1.2

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a>ASP.NET SignalR 1.1.1

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a>ASP.NET SignalR 1.1.0

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a>ASP.NET SignalR 1.0.1

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

Weitere Informationen zu den Nutzungsbedingungen für das CDN, finden Sie unter [Microsoft Ajax-CDN Nutzungsbedingungen](https://www.asp.net/terms-of-use "Microsoft Ajax-CDN Nutzungsbedingungen").
