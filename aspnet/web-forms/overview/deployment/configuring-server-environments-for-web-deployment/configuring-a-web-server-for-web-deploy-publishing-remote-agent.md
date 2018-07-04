---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Konfigurieren eines Webservers für das Web Deploy-Veröffentlichung (Remote-Agent) | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein Webserver (Internet Information Services, IIS), zur Unterstützung von Webpublishing und die Bereitstellung mithilfe der IIS-Web-Bereitstellung konfigurieren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: cb3191a260eb10a47f1aaf818052fcae023ff74a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392763"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Konfigurieren eines Webservers aus, für Web Deploy-Veröffentlichung (Remote-Agent)
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Sie einen Internet Information Services (IIS)-Web-Server zur Unterstützung von Webpublishing und die Bereitstellung mithilfe der IIS-Webbereitstellungstool (Web Deploy) Remote-Agent-Dienst zu konfigurieren.
> 
> Wenn Sie mit Web Deploy 2.0 oder höher arbeiten, stehen drei wichtigsten Ansätze, die Sie verwenden können, um Ihre Anwendungen oder Websites auf einem Webserver zu erhalten. Sie haben folgende Möglichkeiten:
> 
> - Verwenden der *Web Deploy Remote-Agent-Dienst*. Dieser Ansatz erfordert weniger Konfiguration des Webservers, jedoch müssen Sie die Anmeldeinformationen eines lokalen Server-Administrators angeben, um alles auf dem Server bereitstellen.
> - Verwenden der *Web Deploy-Handler*. Dieser Ansatz ist sehr viel komplizierter und erfordert mehr anfänglichen Aufwand auf dem Webserver einrichten. Wenn Sie diesen Ansatz verwenden, können Sie IIS, damit Benutzer ohne Administratorrechte, zur Durchführung der Bereitstellung konfigurieren. Der Handler für die Web-Bereitstellung ist nur in IIS, Version 7 oder höher verfügbar.
> - Verwendung *offlinebereitstellung*. Dieser Ansatz erfordert nur geringfügige Konfigurationen des Webservers, aber ein Server-Administrator muss manuell kopieren des Pakets auf dem Server und importieren Sie es über IIS-Manager.
> 
> Weitere Informationen zu den Hauptfunktionen, Vorteile und Nachteile dieser Ansätze zu erhalten, finden Sie unter [Entscheidung zur Webbereitstellung rechts](choosing-the-right-approach-to-web-deployment.md).


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>Gibt an, das Web Deploy Remote-Agent der richtige Ansatz für Sie?

Ja, wenn der Benutzer, der den Inhalt bereitstellen, werden die Anmeldeinformationen eines Administrators auf dem Zielserver bereitstellen kann. Dieser Ansatz ist häufig wünschenswert, in diesen Szenarien:

- Entwicklungs- und testumgebungen, in denen der Entwickler vollständige Kontrolle über den Ziel-Webserver und Datenbankserver hat.
- Kleinere Organisationen, in denen ein einzelner Benutzer oder eine kleine Gruppe von Benutzern Kontrolle über den gesamten Lebenszyklus der Anwendung hat.

In viel größerer Organisationen und insbesondere für die Staging-oder produktionsumgebungen ist es oft nicht realistisch, Benutzer über Administratorrechte auf Webservern gewähren. Bei gehosteten Webservern ist dies besonders unwahrscheinlich, dass der Fall. Darüber hinaus, wenn Sie beabsichtigen, zum Automatisieren der Bereitstellung von einem Buildserver aus, können Sie nicht Administratoranmeldeinformationen für den Bereitstellungsprozess verwenden möchten. Konfigurieren Sie in diesen Szenarien wird Ihre Webserver zur Unterstützung der Bereitstellung mithilfe der [WebHandler bereitstellen](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) möglicherweise interessant Auswahl bereit.

## <a name="task-overview"></a>Übersicht über den Task

Dieses Thema beschreibt, wie Sie einen Internet Information Services (IIS) 7.5 Webserver zur akzeptieren und Bereitstellen von Webpaketen von einem Remotecomputer mit dem Ansatz der Web Deploy Remote-Agents konfigurieren. Sie müssen Aktionen ausführen:

- Installieren Sie IIS 7.5 und die empfohlene Konfiguration von IIS 7.
- Installieren Sie Web Deploy 2.1 oder höher.
- Erstellen einer IIS-Website zum Hosten des bereitgestellten Inhalts.
- Stellen Sie sicher, dass der Webbereitstellungs-Agent-Dienst ausgeführt wird.

Um die Projektmappe explizit zu hosten, müssen Sie auch auf:

- Installieren von .NET Framework 4.0.
- Installieren Sie ASP.NET MVC 3.

In diesem Thema werden Sie zum Durchführen dieser Verfahren erläutert. Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie mit einem "sauberen" Server-Build mit Windows Server 2008 R2 beginnen. Bevor Sie fortfahren, stellen Sie sicher, dass:

- Windows Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.
- Der Server ist die Domäne eingebunden.
- Der Server hat eine statische IP-Adresse.

> [!NOTE]
> Weitere Informationen zum Einbinden von Computern in einer Domäne, finden Sie unter [Hinzufügen von Computern zur Domäne und Anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren von statischen IP-Adressen finden Sie unter [Konfigurieren einer statischen IP-Adresse](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Der Remote-Agent-Dienst wird von IIS 6 oder höher unterstützt und muss nicht mit einer Domäne verknüpft werden. Allerdings die Schritte in diesem Tutorial wurden entwickelt und getestet mit IIS 7.5 und Prozeduren für andere Versionen können variieren.


## <a name="install-products-and-components"></a>Installieren der Produkte und Komponenten

Dieser Abschnitt führt Sie durch die Installation der erforderlichen Produkte und Komponenten auf dem Webserver. Bevor Sie beginnen, hat sich bewährt, zum Ausführen von Windows Update aus, um sicherzustellen, dass der Server vollständig auf dem neuesten Stand ist.

In diesem Fall müssen Sie Folgendes installieren:

- **Empfohlene Konfiguration von IIS 7**. Dies ermöglicht die **Webserver (IIS)** -Rolle auf dem Webserver und installiert den Satz von IIS-Module und Komponenten, die Sie benötigen, um eine ASP.NET-Anwendung zu hosten.
- **.NET Framework 4.0**. Dies ist erforderlich, um Anwendungen auszuführen, die in dieser Version von .NET Framework erstellt wurden.
- **Webbereitstellungstool 2.1 oder höher**. Dadurch wird Web Deploy (und die zugrunde liegende ausführbare Datei, die MSDeploy.exe) auf dem Server installiert. Im Rahmen dieser Prozess wird es installiert und startet die Web-Agent-Dienst. Dieser Dienst können Sie die Webpakete von einem Remotecomputer bereitstellen.
- **ASP.NET MVC 3**. Dadurch werden die Assemblys, die Sie zum Ausführen von MVC 3-Anwendungen müssen installiert.

> [!NOTE]
> In dieser exemplarischen Vorgehensweise wird die Verwendung des Webplattform-Installers zum Installieren und konfigurieren die erforderlichen Komponenten beschrieben. Zwar Sie nicht mit dem Webplattform-Installer müssen, vereinfacht den Installationsvorgang durch automatisches Erkennen von Abhängigkeiten und sicherstellen, dass Sie immer die aktuellsten Versionen erhalten. Weitere Informationen finden Sie unter [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).


**Installieren der erforderlichen Produkte und Komponenten**

1. Herunterladen und Installieren der [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118).
2. Wenn die Installation abgeschlossen ist, wird der Webplattform-Installer automatisch gestartet.

    > [!NOTE]
    > Sie können jetzt den Webplattform-Installer starten, zu einem beliebigen Zeitpunkt aus der **starten** Menü. Zu diesem Zweck die **starten** Menü klicken Sie auf **Programme**, und klicken Sie dann auf **Microsoft Web Platform Installer**.
3. Am oberen Rand der **Web Platform Installer 3.0** Fenster, klicken Sie auf **Produkte**.
4. Klicken Sie auf der linken Seite des Fensters, klicken Sie im Navigationsbereich auf **Frameworks**.
5. In der **Microsoft .NET Framework 4** Zeile, wenn .NET Framework nicht bereits installiert ist, klicken Sie auf **hinzufügen**.

    > [!NOTE]
    > Möglicherweise haben Sie bereits über Windows Update .NET Framework 4.0 installiert. Wenn ein Produkt oder eine Komponente bereits installiert ist, den Webplattform-Installer wird angegeben, dies durch Ersetzen der **hinzufügen** Schaltfläche mit dem Text **installiert**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. In der **ASP.NET MVC 3 (Visual Studio 2010)** auf **hinzufügen**.
7. Klicken Sie im Navigationsbereich auf **Server**.
8. In der **empfohlenen IIS 7-Konfiguration** auf **hinzufügen**.
9. In der **Web Deployment Tool 2.1** auf **hinzufügen**.
10. Klicken Sie auf **Installieren**. Der Webplattform-Installer zeigt Ihnen eine Liste der Produkte&#x2014;zusammen mit verknüpften Abhängigkeiten&#x2014;installiert werden und werden Sie aufgefordert, den Lizenzbedingungen zustimmen.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. Lesen Sie die Lizenzbedingungen, und wenn Sie den Bedingungen zustimmen, klicken Sie auf **akzeptieren**.
12. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**, und schließen Sie dann die **Web Platform Installer 3.0** Fenster.

Wenn Sie .NET Framework 4.0 vor der Installation von IIS installiert, müssen Sie zum Ausführen der [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (Aspnet\_regiis.exe) auf die neueste Version von ASP.NET bei IIS registrieren. Wenn Sie dies nicht beachten, Sie feststellen werden, dass IIS statischer Inhalte (z. B. HTML-Dateien bereitstellen) ohne Probleme, aber es gibt **HTTP-Fehler 404.0 – nicht gefunden.** beim Versuch, um ASP.NET-Inhalt zu suchen. Sie können dieses Verfahren verwenden, um sicherzustellen, dass ASP.NET 4.0 registriert ist.

**Um ASP.NET 4.0 bei IIS registrieren.**

1. Klicken Sie auf **starten**, und geben Sie dann **Eingabeaufforderung**.
2. In den Suchergebnissen mit der Maustaste **Eingabeaufforderung**, und klicken Sie dann auf **als Administrator ausführen**.
3. Wechseln Sie in das Eingabeaufforderungsfenster zu dem **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** Verzeichnis.
4. Geben Sie folgenden Befehl aus, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. Wenn Sie zum Hosten von 64-Bit-Webanwendungen zu einem beliebigen Zeitpunkt planen, sollten Sie auch die 64-Bit-Version von ASP.NET bei IIS registrieren. Navigieren Sie im Eingabeaufforderungsfenster Befehl dazu, zu der **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** Verzeichnis.
6. Geben Sie folgenden Befehl aus, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

Als sich bewährt verwenden Sie Windows Update erneut an diesem Punkt zum Herunterladen und installieren alle verfügbaren Updates für die neue Produkte und Komponenten, die Sie installiert haben.

## <a name="configure-the-iis-website"></a>Konfigurieren Sie die IIS-Website

Bevor Sie die Webinhalte auf Ihrem Server bereitstellen können, müssen Sie zum Erstellen und Konfigurieren einer IIS-Website, um den Inhalt zu hosten. Web Deploy kann Webpakete nur auf einer vorhandenen IIS-Website bereitgestellt werden; die Website kann nicht für Sie erstellt. Im Allgemeinen müssen Sie diese Aufgaben ausführen:

- Erstellen Sie einen Ordner im Dateisystem, um Ihren Inhalt zu hosten.
- Erstellen einer IIS-Website, um den Inhalt zu verarbeiten, und ordnen sie den lokalen Ordner.
- Erteilen von Leseberechtigungen auf die Identität des Anwendungspools für den lokalen Ordner.

Es ist, zwar nichts hindert, dass Sie zum Bereitstellen von Inhalten an die Standardwebsite in IIS wird dieser Ansatz nicht für etwas anderes als Test oder Demo-Szenarien empfohlen. Um eine produktionsumgebung zu simulieren, sollten Sie eine neue IIS-Website mit Einstellungen erstellen, die den Anforderungen Ihrer Anwendung spezifisch sind.

**Zum Erstellen und Konfigurieren einer IIS-website**

1. Auf dem lokalen Dateisystem, erstellen Sie einen Ordner zum Speichern Ihrer Inhalte (z. B. **C:\DemoSite**).
2. Auf der **starten** Startmenü **Verwaltung**, und klicken Sie dann auf **(Internet Information Services, IIS) Manager**.
3. Im IIS-Manager in der **Verbindungen** Bereich, erweitern Sie den Serverknoten (z. B. **TESTWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. Mit der rechten Maustaste die **Websites** Knoten, und klicken Sie dann auf **Website hinzufügen**.
5. In der **Standortname** geben einen Namen für die IIS-Website (z. B. **DemoSite**).
6. In der **physischer Pfad** Feld aus, geben Sie (oder navigieren Sie zu) den Pfad zum lokalen Ordner (z. B. **C:\DemoSite**).
7. In der **Port** geben die Portnummer, die Sie auf die Website zu hosten (z. B. **85**).

    > [!NOTE]
    > Die standardmäßigen Portnummern sind 80 für HTTP und 443 für HTTPS. Wenn Sie diese Website an Port 80 hosten, müssen Sie jedoch die Standard-Website zu beenden, bevor Sie Ihre Website zugreifen können.
8. Lassen Sie die **Hostnamen** Feld leer, es sei denn, Sie verwenden möchten, konfigurieren Sie einen Domain Name System (DNS)-Eintrag für die Website, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > In einer produktionsumgebung sollten Sie wahrscheinlich zum Hosten Ihrer Website an Port 80 und Konfigurieren eines Hostheaders, zusammen mit DNS-Datensätze werden abgeglichen. Weitere Informationen zum Konfigurieren von Host-Header in IIS 7 finden Sie unter [Konfigurieren eines Hostheaders für eine Website (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Weitere Informationen zu den DNS-Serverrolle in Windows Server 2008 R2, finden Sie unter [DNS-Serverübersicht](https://technet.microsoft.com/en-gb/library/cc770392.aspx) und [DNS-Server](https://technet.microsoft.com/windowsserver/dd448607).
9. In der **Aktionen** Bereich unter **Site bearbeiten**, klicken Sie auf **Bindungen**.
10. In der **Sitebindungen** Dialogfeld klicken Sie auf **hinzufügen**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. In der **Sitebindung hinzufügen** (Dialogfeld), legen die **IP-Adresse** und **Port** entsprechend Ihrer vorhandenen Standortkonfiguration.
12. In der **Hostnamen** geben den Namen des Webservers (z. B. **TESTWEB1**), und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > Die erste sitebindung ermöglicht Ihnen Zugriff auf die Website, die lokal mithilfe der IP-Adresse und Port oder `http://localhost:85`. Die zweite sitebindung ermöglicht Ihnen Zugriff auf die Website von anderen Computern in der Domäne, die mit dem Computernamen (z. B. http://testweb1:85).
13. In der **Sitebindungen** Dialogfeld klicken Sie auf **schließen**.
14. In der **Verbindungen** Bereich, klicken Sie auf **Anwendungspools**.
15. In der **Anwendungspools** Bereich mit der rechten Maustaste in des Namens des Anwendungspools, und klicken Sie dann auf **Grundeinstellungen**. Standardmäßig wird der Name des Ihr Anwendungspool mit dem Namen Ihrer Website übereinstimmen (z. B. **DemoSite**).
16. In der **.NET Framework-Version** Liste **.NET Framework-v4.0.30319**, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > Die beispiellösung ist .NET Framework 4.0 erforderlich. Dies ist im Allgemeinen nicht erforderlich für Web Deploy.

In der Reihenfolge für Ihre Website Inhalte bereitstellen muss die Identität des Anwendungspools schreibgeschützten Zugriff auf den lokalen Ordner verfügen, in dem den Inhalt gespeichert. Führen in IIS 7.5 und Anwendungspools mit der eine eindeutige Identität des Anwendungspools standardmäßig (im Gegensatz zu früheren Versionen von IIS-Anwendungspools in der Regel Ausführungsort würde mit dem NETZWERKDIENST-Konto). Die Identität des Anwendungspools ist kein echtes Benutzerkonto und wird nicht für alle Listen von Benutzern oder Gruppen ausgewiesen&#x2014;stattdessen, er wird dynamisch erstellt, wenn der Anwendungspool gestartet wird. Jede Identität des Anwendungspools wird hinzugefügt, für die lokale **IIS\_IUSRS** Sicherheitsgruppe als ausgeblendetes Element.

Um Berechtigungen für eine Anwendungspoolidentität in einer Datei oder Ordner zu gewähren, haben Sie zwei Möglichkeiten:

- Zuweisen von Berechtigungen, die Identität des Anwendungspools direkt mit dem Format <strong>IIS AppPool\</ strong ><em>[Name des Anwendungspools]</em>(z. B. <strong>IIS AppPool\DemoSite</strong>).
- Zuweisen von Berechtigungen für die **IIS\_IUSRS** Gruppe.

Der gängigste Ansatz ist das Zuweisen von Berechtigungen für die lokale **IIS\_IUSRS** denn diesen Ansatz Sie die Anwendungspools zu ändern, ohne dass eine Neukonfiguration von Dateisystemberechtigungen können zu gruppieren. Das nächste Verfahren verwendet diese Gruppe basierenden Ansatz.

> [!NOTE]
> Weitere Informationen zu den Identitäten der Anwendungspools in IIS 7.5, finden Sie unter [Anwendungspoolidentitäten](https://go.microsoft.com/?linkid=9805123).


**So konfigurieren Sie Berechtigungen für eine IIS-website**

1. Navigieren Sie in Windows Explorer auf den Speicherort Ihres lokalen Ordners.
2. Mit der rechten Maustaste in des Ordners, und klicken Sie dann auf **Eigenschaften**.
3. Auf der **Sicherheit** auf **bearbeiten**, und klicken Sie dann auf **hinzufügen**.
4. Klicken Sie auf **Speicherorte**. In der **Speicherorte** Dialogfeld Wählen Sie den lokalen Server, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. In der **Benutzer oder Gruppen auswählen** (Dialogfeld), Typ **IIS\_IUSRS**, klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.
6. In der <strong>Berechtigungen für</strong><em>[Ordnername]</em>(Dialogfeld), beachten Sie, die die neue Gruppe zugewiesen wurde die <strong>lesen &amp; ausführen</strong>, <strong>Ordner auflisten Inhalt</strong>, und <strong>lesen</strong> standardmäßig Berechtigungen. Lassen Sie diese unverändert, und klicken Sie auf <strong>OK</strong>.
7. Klicken Sie auf <strong>OK</strong> schließen die <em>[Ordnername]</em><strong>Eigenschaften</strong> Dialogfeld.

Als letzte Aufgabe bevor Sie versuchen, alle Webpakete mit Ihrem Server bereitstellen sollten Sie sicherstellen, dass der Webbereitstellungs-Agent-Dienst ausgeführt wird. Wenn Sie ein Paket von einem Remotecomputer bereitstellen, ist der Webbereitstellungs-Agent-Dienst zum Extrahieren und installieren den Inhalt des Pakets verantwortlich. Der Dienst wird standardmäßig gestartet, wenn Sie das Web Deployment Tool installieren und unter der Identität Network Service ausgeführt wird.

Sie können überprüfen, ob ein Dienst ausgeführt wird, mehrere Möglichkeiten, mit verschiedenen Befehlszeilen-Hilfsprogramme oder Windows PowerShell-Cmdlets. Dieses Verfahren beschreibt ein einfaches Verfahren zur UI-basierten.

**Überprüfen Sie, dass der Webbereitstellungs-Agent-Dienst ausgeführt wird**

1. Auf der **starten** Startmenü **Verwaltung**, und klicken Sie dann auf **Services**.
2. Suchen Sie die **Webbereitstellungs-Agent-Dienst** Zeile aus, und überprüfen Sie, ob die **Status** nastaven NA hodnotu **gestartet**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. Wenn der Dienst noch nicht gestartet wird, klicken Sie auf **starten**.

## <a name="configure-firewall-exceptions"></a>Konfigurieren von Firewallausnahmen

Standardmäßig wird TCP-Port 80 unter dieser URL der Remote-Agent-Dienst lauscht:

<http://servername.com/MSDEPLOYAGENTSERVICE>

In den meisten Fällen müssen Sie wird keine zusätzliche Firewall-Regeln für den Remote-Agent-Dienst zu konfigurieren, da der Webserver in der Regel für HTTP-Anforderungen an Port 80 lauscht. Wenn Sie die Installation das Lauschen über einen nicht standardmäßigen Port angepasst haben, müssen Sie Firewallausnahmen nach Bedarf konfigurieren.

## <a name="conclusion"></a>Schlussbemerkung

An diesem Punkt kann der Webserver akzeptieren und Web-Pakete von einem Remotecomputer installieren. Bevor Sie versuchen, eine Webanwendung mit dem Server bereitstellen, sollten Sie diese wichtigen Punkte überprüfen:

- Haben Sie ASP.NET 4.0 bei IIS registriert?
- Besitzt die Identität des Anwendungspools Lesezugriff auf den Quellordner für Ihre Website?
- Werden der Webbereitstellungs-Agent-Dienst wird ausgeführt?

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Konfigurieren von benutzerdefinierter Microsoft Build Engine (MSBuild)-Projektdateien zum Bereitstellen von Web-Pakete in der Remote-Agent-Dienst finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Zurück](scenario-configuring-a-production-environment-for-web-deployment.md)
> [Weiter](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
