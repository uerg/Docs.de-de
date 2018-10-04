---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilerstellung und Debuggen der ASP.NET MVC-app mit Glimpse | Microsoft-Dokumentation
author: Rick-Anderson
description: Glimpse ist erfolgreich sein und wachsende Familie von open-Source-NuGet-Pakete, die ausführliche Leistung bietet, Debuggen und Diagnoseinformationen für ASP.NET ein...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 94a72f22cbcd7fa84528dde502cceaa1e26dcaa1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577287"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profilerstellung und Debuggen der ASP.NET MVC-app mit Glimpse
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Glimpse ist erfolgreich sein und wachsenden Familie von open-Source-NuGet-Pakete, die ausführliche Leistung bietet, Debuggen und Diagnoseinformationen für ASP.NET-Apps. Dabei handelt es sich sehr einfach zu installieren, einfache, extrem schnellen, wichtige Leistungsmetriken am unteren Rand jeder Seite angezeigt. Sie können einen Drilldown in Ihre app ausführen beim müssen Sie herausfinden, was auf dem Server vor sich geht. Glimpse bietet so viel wertvolle Informationen, die es wird, dass Sie es während des gesamten Entwicklungszyklus empfohlen, einschließlich Ihrer Azure-Test-Umgebung verwenden. Während [Fiddler](http://www.telerik.com/fiddler) und [F-12 Entwicklungstools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) eine clientseitige Sicht Glimpse bietet eine detaillierte Übersicht vom Server. Dieses Tutorial konzentriert sich auf die mit dem Blick auf ASP.NET MVC und EF-Pakete, aber viele andere Pakete verfügbar sind. Nach Möglichkeit werden ich auf die entsprechenden verknüpfen [Glimpse-Docs](http://getglimpse.com/Docs/) die ich dabei helfen, verwalten. Glimpse ist ein open-Source-Projekt, Sie zu seinen möglichen Beitrag zu den Quellcode und Dokumentation.


- [Installieren von Glimpse](#ig)
- [Aktivieren von Glimpse für "localhost"](#eg)
- [Die Registerkarte "Zeitplan"](#Time)
- [Modellbindung](#mb)
- [Routen](#route)
- [Verwenden von Glimpse in Azure](#da)
- [Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Installieren von Glimpse

Sie können die Glimpse installieren, von der NuGet-Paket-Manager-Konsole oder von der **NuGet-Pakete verwalten** Konsole. Für diese Demo werde ich die Mvc5- und EF6-Pakete zu installieren:

![Installieren von Glimpse über NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Suchen Sie nach *Glimpse.EF*

![Glimpse.EF von NuGet installieren dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Durch Auswahl **installierte Pakete**, sehen Sie die abhängigen Glimpse-Module installiert:

![Glimpse-Pakete aus DLg installiert](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Die folgenden Befehle installieren Glimpse MVC5- und EF6-Module aus der Paket-Manager-Konsole:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Aktivieren von Glimpse für "localhost"

Navigieren Sie zu http://localhost:&lt; port #&gt;/glimpse.axd, und klicken Sie auf die <strong>Glimpse einschalten</strong> Schaltfläche.

![Glimpse Axd Seite](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Wenn Sie Ihre Favoritenleiste angezeigt haben, können Sie ziehen und legen die Schaltflächen Einblick und deren hinzufügen als Bookmarklets:

![IE mit Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Sie können jetzt Ihre app navigieren und die **Mal "Kopf" nach oben anzeigen** (HUD) wird am unteren Rand der Seite angezeigt.

![Kontakt-Manager-Seite mit HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

Die [Glimpse HUD-Seite](http://getglimpse.com/Docs/Heads-up-Display) details der oben gezeigte Informationen zur zeitlichen Steuerung. Die unaufdringliche Leistung dem HUD angezeigt können Sie für ein Problem sofort - benachrichtigen, bevor Sie den Testszyklus abrufen müssen. Durch Klicken auf die &quot;g&quot; in der unteren rechten Ecke der Glimpse-Bereich wird:

![Glimpse-Bereich](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

In der Abbildung oben die [Registerkarte "Ausführung"](http://getglimpse.com/Docs/Execution-Tab) ausgewählt ist, zeigt die Details der zeitlichen Steuerung der Aktionen und Filter in der Pipeline. Sehen Sie meine [Überwachung beenden Filter Timer](http://www.nuget.org/packages/StopWatch/) in Phase 6 von der Pipeline beginnen. Während meiner Lightweight-Timer bieten, kann nützliche Profil/Timing Data Erkennung fehlerhaft sein sollte die Zeit aufgewendet wird, bei der Autorisierung und zum Rendern der Ansicht. Informieren Sie sich über meine Timer auf [Profil und die Uhrzeit Ihrer ASP.NET MVC-app in Azure ganz](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). Die [Registerkarten](http://getglimpse.com/Docs/Tabs) Seite enthält Links zu detaillierten Informationen auf jeder Registerkarte.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Die Registerkarte "Zeitplan"

Ich habe Tom Dykstras geändert ausstehenden [EF 6/MVC 5-Tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) , des Controllers des Dozenten mit dem folgenden Code zu ändern:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Der obige Code kann ich in einer Abfragezeichenfolge übergeben (`eager`) zum Steuern eager oder explizite Laden von Daten. Explizites Laden werden in der folgenden Abbildung und die zeitlichen Steuerung Seite zeigt jede Registrierung geladen werden, der `Index` Aktionsmethode:

![Explizites Laden](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

In den folgenden Code, mittels Eager Load angegeben ist, und jede Registrierung wird abgerufen, nachdem die `Index` Ansicht aufgerufen wird:

![mittels Eager Load ist angegeben.](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Sie können auf ein Zeitsegment zum Abrufen von ausführliche Zeitsteuerungsinformationen zeigen:

![Wenn darauf gezeigt wird, um ausführliche Zeitsteuerungsdaten finden Sie unter](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Modellbindung

Die [Modell Registerkarte ' Bindung '](http://getglimpse.com/Docs/Model-Binding-Tab) bietet eine Fülle von Informationen, die Ihnen zu verstehen, wie Ihre Formularvariablen gebunden sind und warum einige nicht gebunden werden wie zu erwarten ist. Die folgende Abbildung zeigt die **?** Symbol, den Sie klicken können, um die Glimpse-Hilfeseite für diese Funktion aufzurufen.

![Glimpse-modellbindung anzeigen](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Routen

 Die Registerkarte Glimpse Routen können können Sie das Debuggen und zu verstehen, routing. In der folgenden Abbildung die Produkt-Route ausgewählt ist (und werden in Grün und eine Konvention Glimpse). ![Produktname ausgewählt](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route, Bereiche und Datentoken werden ebenfalls angezeigt. Finden Sie unter [Glimpse Routen](http://getglimpse.com/Docs/Routes-Tab) und [Attribut-Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) für Weitere Informationen. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Verwenden von Glimpse in Azure

Die Standardsicherheitsrichtlinie für Glimpse lässt nur Einblick in Daten vom lokalen Host angezeigt werden. Sie können diese Sicherheitsrichtlinie ändern, damit Sie diese Daten auf einem Remoteserver (z. B. eine Web-app in Azure) anzeigen können. Für testumgebungen in Azure, fügen Sie die hervorgehobene Markierung bis zum Ende der *web.confg* Datei um Einblick zu aktivieren:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Durch diese Änderung nur sehen alle Benutzer Ihre Glimpse-Daten an einem Remotestandort. Betrachten Sie das Markup oben zu einem Veröffentlichungsprofil hinzufügen, sodass sie nur eine angewendeten bereitgestellt hat bei der Verwendung dieses Veröffentlichungsprofils (beispielsweise Ihr Azure-Test-Proifle.) Um Einblick in die Datenübertragungszeiten einzuschränken, fügen wir die `canViewGlimpseData` Rolle und nur Benutzer in dieser Rolle aus, um Einblick in Daten anzuzeigen.

Entfernen Sie die Kommentare aus der *GlimpseSecurityPolicy.cs* und Ändern der ["IsInRole"](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) aus aufrufen `Administrator` auf die `canViewGlimpseData` Rolle:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Sicherheit – konnte die umfangreichen Daten Einblick in die Sicherheit Ihrer App verfügbar machen. Microsoft hat eine sicherheitsüberwachung von Glimpse nicht für die Verwendung auf Produktionen apps ausgeführt.


Informationen zum Hinzufügen von Rollen finden Sie in meinem [bereitstellen eine sicheren ASP.NET MVC 5-Web-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Tutorial.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Bereitstellen einer sicheren ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Glimpse-Konfiguration](http://getglimpse.com/Docs/Configuration) -Doc-Seite zum Konfigurieren von Registerkarten, Laufzeitrichtlinie, Protokollierung und vieles mehr.
