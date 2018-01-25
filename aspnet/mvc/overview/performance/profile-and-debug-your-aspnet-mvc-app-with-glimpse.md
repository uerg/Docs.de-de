---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Ein Profil erstellen und Debuggen von ASP.NET MVC-Anwendung mit Glimpse | Microsoft Docs
author: Rick-Anderson
description: "Glimpse ist verbergen und wachsenden-Familie von open Source-NuGet-Pakete, die detaillierte Leistung bietet, Debuggen und Diagnoseinformationen für ASP.NET ein..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 9cfdced21251b482ca527dda9c3a698de77cc8ca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Ein Profil erstellen und Debuggen von ASP.NET MVC-Anwendung mit Glimpse
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> Glimpse ist eine verbergen und wachsenden-Familie von open Source-NuGet-Pakete, die detaillierte Leistung bietet, debugging und Diagnoseinformationen für ASP.NET-Apps. Es ist sehr einfach zu installieren, einfache, extrem schnelle und wichtige Leistungsmetriken am unteren Rand jeder Seite angezeigt. Sie können einen Drilldown in Ihre app ausführen, wenn Sie benötigen, um herauszufinden, auf dem Server was. Glimpse bietet so viel wertvolle Informationen empfehlen wir, dass Sie es in der gesamten Entwicklungszyklus, einschließlich der Azure-testumgebung verwenden. Während [Fiddler](http://www.telerik.com/fiddler) und [F-12 Entwicklungstools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) bieten eine clientseitige Sicht Glimpse bietet eine detaillierte Übersicht über den Server. Dieses Lernprogramm konzentriert sich auf die mit der Glimpse ASP.NET MVC und EF-Paketen, aber viele andere Pakete verfügbar sind. Nach Möglichkeit wird ich eine Verknüpfung in die entsprechende [Notizenlayouts Docs](http://getglimpse.com/Docs/) die ich zu gewährleisten. Glimpse ist ein open-Source-Projekt, Sie zu können tragen zu den Quellcode und Dokumente.


- [Installieren von Glimpse](#ig)
- [Aktivieren von Glimpse für "localhost"](#eg)
- [Die Registerkarte "Zeitplan"](#Time)
- [Modellbindung](#mb)
- [Routen](#route)
- [Mithilfe von Glimpse in Azure](#da)
- [Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Installieren von Glimpse

Sie können Glimpse installieren, von der NuGet-Paket-Manager-Konsole oder von der **NuGet-Pakete verwalten** Konsole. Für diese Demo werde ich die Mvc5 und EF6 Pakete installieren:

![Installieren von Glimpse über NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Suchen Sie nach *Glimpse.EF*

![Glimpse.EF von NuGet-Installation dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Durch Auswahl **installierte Pakete**, sehen Sie die abhängigen Glimpse-Module installiert:

![Glimpse Pakete aus DLg installiert](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Die folgenden Befehle installieren Glimpse MVC5 und EF6 Module aus der Paket-Manager-Konsole:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Aktivieren von Glimpse für "localhost"

Navigieren Sie zu http://localhost:&lt;port #&gt;/glimpse.axd und klicken Sie auf die **Glimpse einschalten** Schaltfläche.

![Seite "Glimpse Axd"](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Wenn Sie die Favoritenleiste angezeigt haben, können Sie ziehen und die Glimpse Schaltflächen löschen und deren hinzufügen als Bookmarklets:

![IE mit Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Sie können jetzt Ihre app navigieren und die **Köpfe oben anzeigen** (HUD) wird am unteren Rand der Seite angezeigt.

![Seite mit HUD Kontakt-Manager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

Die [Glimpse HUD Seite](http://getglimpse.com/Docs/Heads-up-Display) die Zeitsteuerungsinformationen oben angezeigten details. Die unaufdringlichen Leistung die HUD angezeigt können ein Problem hinweisen sofort - benachrichtigt werden, bevor Sie auf den Testzyklus zugreifen können. Durch Klicken auf die &quot;g&quot; in der unteren rechten Ecke öffnet die Glimpse Bereich:

![Glimpse Bereich](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

In der Abbildung oben die [Registerkarte "Ausführung"](http://getglimpse.com/Docs/Execution-Tab) ausgewählt ist, zeigt die Details der zeitlichen Steuerung der Aktionen und Filter an, in der Pipeline. Sehen Sie meine [Überwachung beenden Filter Timer](http://www.nuget.org/packages/StopWatch/) starten in Phase 6 der Pipeline. Während Meine Lightweight-Zeitgeber bereitstellen, kann nützliche Daten Profil/Timing Cachefehler Zeit aufgewendet hat bei der Autorisierung und Rendern der Ansicht. Informieren Sie sich über Mein Zeitgeber [ein Profil erstellen und die Uhrzeit einer ASP.NET MVC-Anwendung ganz nach Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). Die [Registerkarten](http://getglimpse.com/Docs/Tabs) Seite enthält Links zu ausführlichen Informationen auf jeder Registerkarte.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Die Registerkarte "Zeitplan"

Ich Tom Dykstras verändert wurde ausstehenden [EF 6/MVC 5-Lernprogramm](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) durch den folgenden Code zu ändern, mit dem Controller Lehrkräfte:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Der obige Code ermöglicht, dass in der Abfragezeichenfolge übergeben (`eager`) Steuerelement eager oder explizite des Ladens der Daten. Explizites Laden dient in der folgenden Abbildung und die zeitlichen Steuerung Seite zeigt jede Registrierung geladen werden, der `Index` Aktionsmethode:

![Explizites Laden](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

Im folgenden Code Eager angegeben wird, und jede Registrierung wird abgerufen, nach der `Index` Ansicht aufgerufen wird:

![Eager angegeben ist](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Sie können ein Zeitsegment abzurufenden ausführliche Zeitsteuerungsinformationen zeigen:

![Wenn darauf gezeigt wird, um ausführliche Zeitsteuerungsdaten finden Sie unter](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Wurden die Modellbindung

Die [Bindung-Registerkarte "Modell"](http://getglimpse.com/Docs/Model-Binding-Tab) bietet eine Fülle von Informationen, um besser zu verstehen, wie Ihre Formularvariablen gebunden sind und warum einige nicht gebunden werden wie zu erwarten. Die folgende Abbildung zeigt die **?** Symbol ", die Sie klicken können, um die Glimpse-Hilfeseite für die jeweilige Funktion anzuzeigen.

![Glimpse modellbindung anzeigen](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Routen

 Die Registerkarte Glimpse Routen kann können Sie das Debuggen und zu verstehen, routing. In der folgenden Abbildung wird die Produkt-Route ausgewählt ist (und werden in Grün, eine Konvention Glimpse). ![Produktname ausgewählt](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route Einschränkungen, Bereiche und Daten Token werden ebenfalls angezeigt. Finden Sie unter [Glimpse Routen](http://getglimpse.com/Docs/Routes-Tab) und [Routing-Attribut in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) für Weitere Informationen. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Mithilfe von Glimpse in Azure

Die Standardsicherheitsrichtlinie Glimpse kann nur Glimpse Daten vom lokalen Host angezeigt werden. Sie können diese Sicherheitsrichtlinie ändern, damit Sie diese Daten auf einem Remoteserver (z. B. eine Web-app in Azure) anzeigen können. Für testumgebungen in Azure, fügen Sie die hervorgehobenen markieren bis zum Ende der *web.confg* Datei Glimpse zu aktivieren:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Durch diese Änderung allein kann jeder Benutzer Daten Glimpse an einem Remotestandort sehen. Betrachten Sie das Markup oben auf die Veröffentlichungsprofile hinzufügen, sodass sie nur eine angewendeten bereitgestellt hat bei der Verwendung dieses Veröffentlichungsprofil (z. B. Ihre Azure-Test-Proifle.) Um Einblick Daten zu beschränken, fügen wir der `canViewGlimpseData` Rolle und Benutzer in dieser Rolle zum Anzeigen von Glimpse Daten sind nur zulässig.

Entfernen Sie die Kommentare aus der *GlimpseSecurityPolicy.cs* Datei und ändern Sie die [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) aus aufrufen `Administrator` auf die `canViewGlimpseData` Rolle:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Sicherheit – konnte von Glimpse bereitgestellten umfangreichen Daten die Sicherheit Ihrer App verfügbar machen. Microsoft hat eine sicherheitsüberwachung von Glimpse nicht für die Verwendung auf Produktionen-apps ausgeführt.


Informationen zum Hinzufügen von Rollen finden Sie unter meinem [Secure ASP.NET MVC 5-Web-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure bereitstellen](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Lernprogramm.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Bereitstellen Sie eine sichere ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure.](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Konfiguration Notizenlayouts](http://getglimpse.com/Docs/Configuration) -Doc-Seite zum Konfigurieren von Registerkarten, Laufzeitrichtlinie, Protokollierung und vieles mehr.
