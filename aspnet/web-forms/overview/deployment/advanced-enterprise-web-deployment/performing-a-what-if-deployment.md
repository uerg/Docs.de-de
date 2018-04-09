---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Durchführen einer was, wenn Bereitstellung | Microsoft Docs
author: jrjlee
description: In diesem Thema wird beschrieben, wie "WhatIf" ausführen (oder simuliert) mit dem Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) und V Bereitstellungen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: c1a13f38c8e629bcd615190b00104109e25fb289
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="performing-a-what-if-deployment"></a>Ausführen einer "Was-wäre-wenn" Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie "Was-wäre-wenn" ausführen (oder simuliert) mit dem Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) und VSDBCMD Bereitstellungen. Dadurch können Sie die Auswirkungen Ihrer Bereitstellung Logik auf eine bestimmte zielumgebung zu bestimmen, bevor Sie die Anwendung tatsächlich bereitstellen.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf in beschriebene Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in die erstellungs-und Bereitstellung von gesteuert wird zwei Projektdateien&#x2014;eine Buildanweisungen, die für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess gelten "" enthält. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Ausführen einer "Was-wäre-wenn" Bereitstellung für Webpakete

Web Deploy umfasst Funktionen, die können Sie Bereitstellungen in "Was-wäre-wenn" ausführen (oder Testversion) Modus. Wenn Sie Elemente in "Was-wäre-wenn"-Modus bereitstellen, generiert das Web Deploy eine Protokolldatei, als ob mussten Sie die Bereitstellung ausgeführt, aber es ist nicht tatsächlich aller Elemente, die auf dem Zielserver ändern. Überprüfen die Protokolldatei kann Ihnen helfen, welche Auswirkung nachvollziehen zu können, die Bereitstellung auf dem Zielserver, insbesondere muss:

- Was wird hinzugefügt.
- Was wird aktualisiert.
- Was wird gelöscht.

Da eine "Was-wäre-wenn" Bereitstellung tatsächlich ändert nichts auf dem Zielserver, was immer nicht möglich ist, vorherzusagen, ob es sich bei eine Bereitstellung erfolgreich ist.

Wie in beschrieben [Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md), können Sie mithilfe von Web Deploy auf zwei Arten Webpaketen bereitstellen&#x2014;mithilfe des Befehlszeilendienstprogramms MSDeploy.exe direkt oder durch Ausführen der *. deploy.cmd* Datei während des Erstellungsprozesses generiert.

Wenn Sie MSDeploy.exe direkt verwenden, können Sie eine "Was-wäre-wenn" Bereitstellung ausführen, durch Hinzufügen der **"-WhatIf"** Flag auf Ihre Befehlseingaben. Beispielsweise sollte um auszuwerten, was passieren würde, wenn Sie das Paket ContactManager.Mvc.zip in einer Stagingumgebung bereitgestellt, der MSDeploy-Befehl diesem ähneln:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Wenn Sie mit den Ergebnissen der Bereitstellung "Was-wäre-wenn" zufrieden sind, können Sie entfernen die **"-WhatIf"** Flag eine aktive Bereitstellung ausführen.

> [!NOTE]
> Weitere Informationen zu Befehlszeilenoptionen für MSDeploy.exe, finden Sie unter [Web Deploy Vorgang Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Bei Verwendung der *. deploy.cmd* -Datei können Sie eine "Was-wäre-wenn" Bereitstellung ausführen, indem Sie z. B. die **/t** flag (Testmodus)-Flag anstelle von der **/y** Flag ("yes" oder Aktualisierungsmodus) in der Befehl. Beispielsweise, um auszuwerten, was passieren würde, wenn Sie durch Ausführen des Pakets ContactManager.Mvc.zip bereitgestellt der *. deploy.cmd* -Datei der Befehl sollte diesem ähneln:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Sie mit den Ergebnissen der Bereitstellungskonfiguration "Testmodus" zufrieden sind, können Sie ersetzen die **/t** flag mit einer **/y** Flag eine aktive Bereitstellung ausführen:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Weitere Informationen zu Befehlszeilenoptionen für *. deploy.cmd* finden Sie unter [Vorgehensweise: Installieren Sie eine Bereitstellung Paket mithilfe der Datei deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx). Wenn das Ausführen der *. deploy.cmd* Datei ohne Angabe von Flags, die Befehlszeile wird eine Liste der verfügbaren Flags angezeigt.


## <a name="performing-a-what-if-deployment-for-databases"></a>Ausführen einer "Was-wäre-wenn" Bereitstellung für Datenbanken

In diesem Abschnitt wird davon ausgegangen, dass Sie zum Ausführen von inkrementellen, schemabasierte datenbankbereitstellung VSDBCMD-Hilfsprogramm verwenden. Dieser Ansatz wird ausführlich in [Datenbankprojekte bereitstellen](../web-deployment-in-the-enterprise/deploying-database-projects.md). Es wird empfohlen, dass Sie sich mit den in diesem Thema vertraut, bevor Sie die hier beschriebenen Konzepte gelten auch.

Bei Verwendung von VSDBCMD in **bereitstellen** -Modus können Sie die **/dd** (oder **/DeployToDatabase**) flag steuern, ob VSDBCMD tatsächlich die Datenbank stellt oder nur generiert eine Bereitstellungsskript. Wenn Sie eine DBSCHEMA-Datei bereitstellen, ist dies das Verhalten auf:

- Bei Angabe von **/dd+** oder **/dd**, VSDBCMD ein Bereitstellungsskript generiert und die Datenbank bereitstellen.
- Bei Angabe von **/dd-** oder ohne den Schalter, VSDBCMD wird nur ein Bereitstellungsskript generiert.

> [!NOTE]
> Wenn Sie die DEPLOYMANIFEST-Datei anstelle einer DBSCHEMA-Datei, das Verhalten der Bereitstellung der **/dd** Switch ist viel komplizierter. Im Wesentlichen VSDBCMD ignoriert den Wert von der **/dd** wechseln, wenn die DEPLOYMANIFEST-Datei enthält eine **DeployToDatabase** Element mit einem Wert von **"true"**. [Bereitstellen von Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md) wird dieses Verhalten vollständig beschrieben.


Beispielsweise, um das Generieren eines Bereitstellungsskripts für die **ContactManager** Datenbank ohne eigentlichen Bereitstellung der Datenbank, die VSDBCMD-Befehl sollte diesem ähneln:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD ist ein Bereitstellungstool differenziellen Datenbank- und das Bereitstellungsskript wird als solche dynamisch generiert, um alle erforderlichen die aktuelle Datenbank zu aktualisieren, sofern, auf das angegebene Schema vorhanden SQL‑Befehle enthalten. Überprüfen das Bereitstellungsskript ist eine gute Möglichkeit, zu bestimmen, was die Bereitstellung auswirken hat in der aktuellen Datenbank und die darin enthaltenen Daten. Sie möchten z. B. zu ermitteln:

- Gibt an, ob vorhandenen Tabellen entfernt werden sollen, und gibt an, ob die, die zu Datenverlusten führen wird.
- Gibt an, ob die Reihenfolge der Vorgänge ein Risiko des Datenverlusts, z. B. führt, wenn Sie teilen oder Zusammenführen von Tabellen.

Wenn Sie mit dem Bereitstellungsskript zufrieden sind, können Sie die VSDBCMD mit Wiederholen einer **/dd+** Flag die Änderungen vornehmen. Alternativ können Sie das Bereitstellungsskript aus, um Ihre Anforderungen erfüllt, und führen Sie es manuell auf dem Datenbankserver bearbeiten.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrieren von "Was-wäre-wenn-Funktionalität in benutzerdefinierten Projektdateien

In komplexeren Bereitstellungsszenarios, Sie möchten eine benutzerdefinierte Microsoft Build Engine (MSBuild)-Projektdatei zu verwenden, um Ihre Logik Build- und Bereitstellungsprozess kapseln, wie in beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Beispielsweise ist in der [Vorgesetzten Kontakts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Beispielprojektmappe, die *Publish.proj* Datei:

- Die Projektmappe erstellt.
- Mithilfe von Web Deploy zum Packen und Bereitstellen der Anwendung ContactManager.Mvc.
- Mithilfe von Web Deploy zum Packen und Bereitstellen der Anwendung ContactManager.Service.
- Stellt die **ContactManager** Datenbank.

Wenn Sie die Bereitstellung von mehreren Webpaketen und/oder Datenbanken in einzelnen Schritten, die auf diese Weise zu integrieren, sollten Sie auch die Möglichkeit, die gesamte Bereitstellung in einer "Was-wäre-wenn"-Modus ausführen.

Die *Publish.proj* Datei veranschaulicht, wie Sie dies tun können. Zunächst müssen Sie eine Eigenschaft zum Speichern des Werts "Was-wäre-wenn" zu erstellen:


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


In diesem Fall haben Sie eine Eigenschaft mit dem Namen erstellt **"WhatIf"** hat den Standardwert des **"false"**. Benutzer können diesen Wert überschreiben, indem Sie das Festlegen von **"true"** in einen Befehlszeilenparameter wie Sie sehen in Kürze.

Die nächste Phase bezieht sich auf alle Web Deploy parametrisieren und VSDBCMD Befehle, damit die Flags entsprechen den **"WhatIf"** Eigenschaftswert. Z. B. das nächste Ziel (entnommen der *Publish.proj* Datei und vereinfachte) ausgeführt wird die *. deploy.cmd* Datei zum Bereitstellen eines Webpakets. Wird standardmäßig der Befehl umfasst einen **/y** Switch ("yes" oder Updatemodus). Wenn **"WhatIf"** festgelegt ist, um **"true"**, dies wird ersetzt durch eine **/t** Switch (Test- oder "Was-wäre-wenn"-Modus).


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Auf ähnliche Weise wird das nächste Ziel des Hilfsprogramms VSDBCMD zum Bereitstellen einer Datenbank verwendet. Wird standardmäßig ein **/dd** Switch nicht enthalten ist. Dies bedeutet, dass VSDBCMD ein Bereitstellungsskript generiert, aber die Datenbank nicht bereitgestellt werden&#x2014;heißt, "Was-wäre-wenn-Szenario. Wenn die **"WhatIf"** Eigenschaft nicht festgelegt ist, um **"true"**, **/dd** Schalter hinzugefügt wird und VSDBCMD wird die Datenbank bereitstellen.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Sie können den gleichen Ansatz verwenden, parametrisieren Sie die entsprechenden Befehle in der Projektdatei. Wenn Sie eine "Was-wäre-wenn" Bereitstellung ausführen möchten, können dann geben Sie einfach eine **"WhatIf"** Eigenschaftswert über die Befehlszeile:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


Auf diese Weise können Sie eine "Was-wäre-wenn" Bereitstellung für alle Projektkomponenten in einem einzigen Schritt ausführen.

## <a name="conclusion"></a>Schlussbemerkung

Dieses Thema beschreibt, wie Sie "Was-wäre-wenn-Bereitstellungen mithilfe von Web Deploy, VSDBCMD und MSBuild ausgeführt wird. Eine "Was-wäre-wenn" Bereitstellung können Sie das Auswerten der Auswirkungen einer vorgeschlagenen Bereitstellung aus, bevor Sie Änderungen in der zielumgebung tatsächlich vornehmen.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu Web Deploy-Befehlszeilensyntax, finden Sie unter [Web Deploy Vorgang Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Um Hilfe bei der Befehlszeilenoptionen, die bei der Verwendung der *. deploy.cmd* finden Sie unter [Vorgehensweise: Installieren Sie eine Bereitstellung Paket mithilfe der Datei deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx). Anleitungen zu VSDBCMD-Befehlszeilensyntax, finden Sie unter [Command-Line Reference for VSDBCMD. EXE-Datei (Bereitstellung und Schemaimport)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Zurück](advanced-enterprise-web-deployment.md)
> [Weiter](customizing-database-deployments-for-multiple-environments.md)
