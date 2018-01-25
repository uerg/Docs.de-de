---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: "Konfigurieren einen Webserver für das Web Deploy-Veröffentlichung (Bereitstellung Offline) | Microsoft Docs"
author: jrjlee
description: "Dieses Thema beschreibt, wie einen IIS-Web-Server zur Unterstützung von offline-Webpublishing und Bereitstellung konfigurieren. Beim Arbeiten mit Internet Information Services (ich..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: dfd3ab41e44a3b000bf2c25a5a71db4344617bf2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Konfigurieren eines Webservers für Web Deploy-Veröffentlichung (Offline Bereitstellung)
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie einen IIS-Web-Server zur Unterstützung von offline-Webpublishing und Bereitstellung konfigurieren.
> 
> Wenn Sie mit Internet Information Services (IIS) Webbereitstellungstool (Web Deploy) 2.0 oder höher arbeiten, stehen drei wichtigsten Ansätze, die Sie verwenden können, um Ihre Anwendungen oder Websites auf einem Webserver abzurufen. Sie haben folgende Möglichkeiten:
> 
> - Verwenden der *Web Deploy Remote-Agent-Dienst*. Dieser Ansatz erfordert weniger Konfiguration des Webservers, jedoch müssen Sie die Anmeldeinformationen eines Administrators lokalen Server bereitstellen, um alle Elemente auf dem Server bereitstellen.
> - Verwenden der *Web Deploy-Handler*. Dieser Ansatz ist viel komplexer und erfordert mehr anfängliche Aufwand auf dem Webserver einrichten. Wenn Sie diesen Ansatz verwenden, können Sie IIS damit können Benutzer ohne Administratorrechte zum Ausführen der Bereitstellung konfigurieren. Der Web-bereitstellen-Handler ist nur in IIS, Version 7 oder höher verfügbar.
> - Verwendung *offline Bereitstellung*. Dieser Ansatz erfordert den geringsten Konfiguration des Webservers, ein Serveradministrator muss jedoch manuell kopieren Sie das Webpaket auf dem Server und über den IIS-Manager zu importieren.
> 
> Weitere Informationen zu den Hauptfunktionen, Vorteile und Nachteile dieser Ansätze, finden Sie unter [Auswählen der rechts Ansatz für die Webbereitstellung](choosing-the-right-approach-to-web-deployment.md).


Ja, wenn die Netzwerk-Infrastruktur oder Sicherheit Einschränkungen Remotebereitstellung verhindern. Dies liegt wahrscheinlich der Fall in produktionsumgebungen Internetzugriff, auf dem Webserver isoliert sind & #x 2014; entweder physisch oder durch Firewalls und Subnetze & #x 2014; vom Rest Ihrer Serverinfrastruktur.

Dieser Ansatz wird offensichtlich, weniger erwünscht sein, wenn die Webanwendungen in regelmäßigen Abständen aktualisiert werden. Wenn Ihrer Infrastruktur ermöglicht es, möglicherweise möchten Sie Remotebereitstellung, aktivieren Sie ggf. mit der Web-Handler bereitstellen oder dem Web Bereitstellen von Remote-Agent-Dienst.

## <a name="task-overview"></a>Übersicht über den Task

Um den Webserver zur Unterstützung von offlineimport und Bereitstellung von Webpaketen konfigurieren zu können, benötigen Sie:

- Installieren Sie IIS 7.5 und IIS 7 empfohlene Konfiguration.
- Installieren Sie Web Deploy 2.1 oder höher.
- Erstellen einer IIS-Website zum Hosten des bereitgestellten Inhalts.
- Deaktivieren des Webbereitstellungs-Agent-Diensts an.

Um die Projektmappe speziell zu hosten, müssen Sie außerdem auf:

- Installieren Sie .NET Framework 4.0.
- Installieren Sie ASP.NET MVC 3.

In diesem Thema erfahren Sie, wie Sie jede der folgenden Verfahren ausführen. Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie mit einem "sauberen" Server-Build mit Windows Server 2008 R2 starten. Bevor Sie fortfahren, stellen Sie sicher, die:

- Windows Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.
- Der Server ist die Domäne.
- Der Server hat eine statische IP-Adresse.

> [!NOTE]
> Weitere Informationen zum Hinzufügen von Computern zu einer Domäne, finden Sie unter [Verknüpfen von Computern zur Domäne und Anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren von statischen IP-Adressen finden Sie unter [eine statische IP-Adresse konfigurieren](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Produkte und Komponenten installieren

Dieser Abschnitt führt Sie durch die Installation der erforderlichen Produkte und Komponenten auf dem Webserver. Bevor Sie beginnen, empfiehlt sich zum Ausführen von Windows Update aus, um sicherzustellen, dass der Server vollständig auf dem neuesten Stand ist.

In diesem Fall müssen Sie Folgendes installieren:

- **Empfohlene Konfiguration für IIS 7**. Dies ermöglicht die **Webserver (IIS)** -Rolle auf Ihrem Webserver und installiert den Satz von IIS-Module und Komponenten, die Sie benötigen, um eine ASP.NET-Anwendung zu hosten.
- **.NET Framework 4.0**. Dies ist erforderlich, um Anwendungen auszuführen, die für diese Version von .NET Framework erstellt wurden.
- **Webbereitstellungstool 2.1 oder höher Web**. Hiermit werden Web Deploy (und die zugrunde liegende ausführbare Datei, MSDeploy.exe) auf dem Server installiert. Web Deploy IIS integriert und ermöglicht Ihnen die importieren und Exportieren von Webpaketen.
- **ASP.NET MVC 3**. Hiermit werden die Assemblys, die Sie zum Ausführen von MVC 3-Anwendungen müssen installiert.

> [!NOTE]
> In dieser exemplarischen Vorgehensweise beschreibt die Verwendung des Webplattform-Installer zum Installieren und Konfigurieren der verschiedenen Komponenten. Obwohl Sie nicht mit dem Webplattform-Installer, vereinfacht es während des Installationsvorgangs, indem automatisch erkennen von Abhängigkeiten, und stellen Sie sicher, dass Sie immer die aktuellsten Versionen erhalten. Weitere Informationen finden Sie unter [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/?linkid=9805118).


**Zum Installieren der erforderlichen Produkte und Komponenten**

1. Herunterladen und Installieren der [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118).
2. Wenn die Installation abgeschlossen ist, wird der Webplattform-Installer automatisch gestartet.

    > [!NOTE]
    > Sie können den Webplattform-Installer jetzt starten, zu einem beliebigen Zeitpunkt aus der **starten** Menü. Zu diesem Zweck die **starten** Menü klicken Sie auf **Programme**, und klicken Sie dann auf **Microsoft Web Platform Installer**.
3. Am oberen Rand der **Web Platform Installer 3.0** Fenster, klicken Sie auf **Produkte**.
4. Klicken Sie auf der linken Seite des Fensters klicken Sie im Navigationsbereich auf **Frameworks**.
5. In der **Microsoft .NET Framework 4** Zeile, wenn .NET Framework nicht bereits installiert ist, klicken Sie auf **hinzufügen**.

    > [!NOTE]
    > Sie können bereits .NET Framework 4.0 über Windows Update installiert haben. Wenn ein Produkt oder eine Komponente bereits installiert ist, den Webplattform-Installer wird angegeben, das durch Ersetzen der **hinzufügen** Schaltfläche mit dem Text **installiert**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. In der **ASP.NET MVC 3 (Visual Studio 2010)** auf **hinzufügen**.
7. Klicken Sie im Navigationsbereich auf **Server**.
8. In der **empfohlene Konfiguration von IIS 7** auf **hinzufügen**.
9. In der **Web Bereitstellung Tool 2.1** auf **hinzufügen**.
10. Klicken Sie auf **Installieren**. Der Webplattform-Installer erfahren Sie, eine Liste der Produkte & #x 2014; sowie alle zugehörigen Abhängigkeiten Ko & #x 2014; installiert werden und werden Sie aufgefordert, die Lizenzbedingungen zu akzeptieren.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Überprüfen Sie die Lizenzbedingungen, und wenn Sie den Bedingungen zustimmen, klicken Sie auf **ich stimme**.
12. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**, und schließen Sie dann die **Web Platform Installer 3.0** Fenster.

Wenn Sie .NET Framework 4.0 vor der Installation von IIS installiert haben, müssen Sie zum Ausführen der [ASP.NET IIS-Registrierungstool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (Aspnet\_regiis.exe) auf die neueste Version von ASP.NET bei IIS registrieren. Wenn Sie nicht dies tun, Sie werden feststellen, dass IIS statischer Inhalte (z. B. HTML-Dateien bereitstellen) ohne Probleme zurückgegeben **HTTP-Fehler 404.0 – Nichtgefunden** beim Versuch, den ASP.NET-Inhalt, durchsuchen. Das nächste Verfahren können Sie sicherstellen, dass ASP.NET 4.0 registriert ist.

**Um ASP.NET 4.0 bei IIS registrieren**

1. Klicken Sie auf **starten**, und geben Sie dann **Eingabeaufforderung**.
2. In den Suchergebnissen mit der Maustaste **Eingabeaufforderung**, und klicken Sie dann auf **als Administrator ausführen**.
3. Wechseln Sie in das Eingabeaufforderungsfenster zu dem **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** Verzeichnis.
4. Geben Sie diesen Befehl, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Wenn Sie zum Hosten von 64-Bit-Webanwendungen zu einem beliebigen Zeitpunkt planen, sollten Sie auch die 64-Bit-Version von ASP.NET bei IIS registrieren. Navigieren Sie dazu im Eingabeaufforderungsfenster zu dem **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** Verzeichnis.
6. Geben Sie diesen Befehl, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Als empfiehlt sich verwenden Sie Windows Update erneut zu diesem Zeitpunkt herunterladen und installieren alle verfügbaren Updates für die neue Produkte und Komponenten, die Sie installiert haben.

## <a name="configure-the-iis-website"></a>Konfigurieren Sie die IIS-Website

Bevor Sie von Webinhalten zu Ihrem Server bereitstellen können, müssen Sie zum Erstellen und Konfigurieren einer IIS-Website zum Hosten des Inhalts. Web Deploy kann nur Webpaketen zu einer vorhandenen IIS-Website bereitstellen. Es kann nicht die Website erstellt werden. Auf hoher Ebene müssen Sie diese Aufgaben ausführen:

- Erstellen Sie einen Ordner im Dateisystem, Ihre Inhalte hosten.
- Erstellen Sie eine IIS-Website, um den Inhalt zu fungieren, und den lokalen Ordner zuordnen.
- Erteilen von Leseberechtigungen für auf der Identität des Anwendungspools für den lokalen Ordner.

Es gibt zwar keine beenden Sie aus der Bereitstellung von Inhalt auf der Standardwebsite in IIS, wird dieser Ansatz nicht für etwas anderes als Test oder diese Demonstration Szenarien empfohlen. Um eine produktionsumgebung zu simulieren, sollten Sie eine neue IIS-Website mit Einstellungen erstellen, die den Anforderungen Ihrer Anwendung spezifisch sind.

**Zum Erstellen und Konfigurieren einer IIS-website**

1. Erstellen Sie einen Ordner zum Speichern Ihrer Inhalte im lokalen Dateisystem (z. B. **C:\DemoSite**).
2. Auf der **starten** Sie im Menü **Verwaltung**, und klicken Sie dann auf **(Internet Information Services, IIS) Manager**.
3. Im IIS-Manager in der **Verbindungen** Bereich, erweitern Sie den Serverknoten (z. B. **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Mit der rechten Maustaste die **Sites** Knoten, und klicken Sie dann auf **Website hinzufügen**.
5. In der **Standortname** geben einen Namen für die IIS-Website (z. B. **DemoSite**).
6. In der **physischen Pfad** können Sie eingeben (oder navigieren Sie zu) den Pfad zum lokalen Ordner (z. B. **C:\DemoSite**).
7. In der **Port** geben die Portnummer auf dem die Website gehostet werden soll (z. B. **85**).

    > [!NOTE]
    > Die standardmäßigen Portnummern sind 80 für HTTP und 443 für HTTPS. Wenn Sie diese Website an Port 80 hosten, müssen Sie die Standard-Website zu beenden, bevor Sie Ihre Website zugreifen können.
8. Lassen Sie die **Hostnamen** Feld leer, es sei denn, Sie verwenden möchten, konfigurieren einen Domain Name System (DNS)-Datensatz für die Website, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > In einer produktionsumgebung möchten Sie wahrscheinlich zum Hosten Ihrer Website an Port 80 und Konfigurieren eines Hostheaders, zusammen mit übereinstimmenden DNS-Datensätzen. Weitere Informationen zum Konfigurieren von Hostheadern in IIS 7 finden Sie unter [Konfigurieren eines Hostheaders für eine Website (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Weitere Informationen zu den DNS-Serverrolle in Windows Server 2008 R2, finden Sie unter [DNS-Server (Übersicht)](https://technet.microsoft.com/en-gb/library/cc770392.aspx) und [DNS-Server](https://technet.microsoft.com/windowsserver/dd448607).
9. In der **Aktionen** Bereich unter **Site bearbeiten**, klicken Sie auf **Bindungen**.
10. In der **Sitebindungen** (Dialogfeld), klicken Sie auf **hinzufügen**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. In der **Sitebindung hinzufügen** (Dialogfeld), legen die **IP-Adresse** und **Port** entsprechend die vorhandenen Website-Konfiguration.
12. In der **Hostnamen** geben den Namen des Webservers (z. B. **PROWEB1**), und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > Der erste Standort Bindung ermöglicht es Ihnen Zugriff auf die Website, die lokal mithilfe von IP-Adresse und Port oder `http://localhost:85`. Die zweite Bindungen pro Website können Sie auf die Website von anderen Computern in der Domäne, die über den Computernamen (z. B. Http://proweb1:85) zugreifen.
13. In der **Sitebindungen** (Dialogfeld), klicken Sie auf **schließen**.
14. In der **Verbindungen** Bereich, klicken Sie auf **Anwendungspools**.
15. In der **Anwendungspools** Bereich Maustaste auf den Namen des Anwendungspools, und klicken Sie dann auf **Grundeinstellungen**. Standardmäßig wird der Name der Pool der Anwendung mit dem Namen Ihrer Website übereinstimmen (z. B. **DemoSite**).
16. In der **.NET Framework-Version** Liste **.NET Framework-v4.0.30319**, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > Die beispiellösung erfordert .NET Framework 4.0. Dies ist im Allgemeinen nicht zwingend für Web Deploy.

In der Reihenfolge für Ihre Website Inhalte bereitstellen muss die Identität des Anwendungspools auf den lokalen Ordner Leseberechtigungen verfügen, in dem den Inhalt gespeichert. Führen Sie Anwendungspools in IIS 7.5 und mit einer eindeutigen Identität des Anwendungspools standardmäßig (im Gegensatz zu früheren Versionen von IIS-Anwendungspools in der Regel Ausführungsort würde mit dem NETZWERKDIENST-Konto). Die Identität des Anwendungspools ist kein Konto echte Benutzer ist und nicht auf alle Listen mit Benutzern oder Gruppen & #x 2014 angezeigt; es wird stattdessen erstellt dynamisch beim Start des Anwendungspools. Jede Identität des Anwendungspools wird hinzugefügt, der lokalen **IIS\_IUSRS** Sicherheitsgruppe "als ausgeblendetes Element.

Um Berechtigungen für eine Anwendungspoolidentität auf eine Datei oder einen Ordner zu gewähren, haben Sie zwei Optionen:

- Zuweisen von Berechtigungen auf die Identität des Anwendungspools direkt unter Verwendung des Formats **IIS AppPool\***[Anwendungspoolname] * (z. B. **IIS AppPool\DemoSite**).
- Zuweisen von Berechtigungen für die **IIS\_IUSRS** Gruppe.

Der am häufigsten verwendete Ansatz ist die Zuweisung von Berechtigungen für die lokale **IIS\_IUSRS** gruppieren, da es sich bei diesem Ansatz können Sie Anwendungspools zu ändern, ohne dass eine Neukonfiguration Dateisystemberechtigungen. Im nächste Verfahren wird dieser Ansatz Gruppenbasierte verwendet.

> [!NOTE]
> Weitere Informationen zu Anwendungspoolidentitäten in IIS 7.5, finden Sie unter [Anwendungspoolidentitäten](https://go.microsoft.com/?linkid=9805123).


**So konfigurieren Sie Berechtigungen für die IIS-website**

1. Navigieren Sie in Windows-Explorer zum Speicherort des Ordners "lokale".
2. Mit der rechten Maustaste in des Ordners, und klicken Sie dann auf **Eigenschaften**.
3. Auf der **Sicherheit** auf **bearbeiten**, und klicken Sie dann auf **hinzufügen**.
4. Klicken Sie auf **Speicherorte**. In der **Speicherorte** (Dialogfeld), wählen Sie den lokalen Server, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. In der **Benutzer oder Gruppen auswählen** Geben Sie im Dialogfeld **IIS\_IUSRS**, klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.
6. In der **Berechtigungen für *** [Ordnername]* (Dialogfeld), beachten Sie, die die neue Gruppe zugewiesen wurden die **lesen &amp; ausführen**, **Ordnerinhalt auflisten**, und **Lesen** Berechtigungen standardmäßig. Lassen Sie Sie unverändert, und klicken Sie auf **OK**.
7. Klicken Sie auf **OK** schließen die *[Ordnername] *** Eigenschaften** (Dialogfeld).

## <a name="disable-the-remote-agent-service"></a>Deaktivieren Sie den Remote-Agent-Dienst

Wenn Sie Web Deploy installieren, wird der Webbereitstellungs-Agent-Dienst installiert und automatisch gestartet. Dieser Dienst ermöglicht Ihnen das Bereitstellen und Veröffentlichen von Webpakete von einem Remotestandort aus. Wird nicht verwenden Sie die Remotebereitstellung-Funktion in diesem Szenario, daher sollten Sie beenden und deaktivieren Sie den Dienst.

> [!NOTE]
> Sie müssen nicht den remote-Agent-Dienst zum Importieren und Bereitstellen von Web-Paket manuell beenden. Allerdings ist es empfiehlt sich zu beenden und deaktivieren Sie den Dienst aus, wenn Sie nicht verwenden möchten.


Sie können beenden und deaktivieren einen Dienst auf verschiedene Weise mit verschiedenen Befehlszeilen-Hilfsprogramme oder Windows PowerShell-Cmdlets. Hier wird beschrieben, ein einfaches UI-basierte Verfahren.

**So beenden und Deaktivieren des remote-Agent-Diensts**

1. Auf der **starten** Sie im Menü **Verwaltung**, und klicken Sie dann auf **Services**.
2. Suchen Sie in der Konsole Dienste der **Webbereitstellungs-Agent-Dienst** Zeile.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Mit der rechten Maustaste **Webbereitstellungs-Agent-Dienst**, und klicken Sie dann auf **Eigenschaften**.
4. In der **Deployment Agent-Webdiensteigenschaften** (Dialogfeld), klicken Sie auf **beenden**.
5. In der **Starttyp** Liste **deaktiviert**, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Schlussbemerkung

An diesem Punkt ist Ihrem Webserver für offline Bereitstellung von Paketen bereit. Bevor Sie versuchen, Webpaketen in einer IIS-Website importieren, sollten Sie überprüfen Sie die folgenden wichtigen Punkte:

- Haben Sie ASP.NET 4.0 bei IIS registriert?
- Besitzt die Identität des Anwendungspools Lesezugriff für den Quellordner für Ihre Website?
- Haben Sie der Webbereitstellungs-Agent-Dienst angehalten?

>[!div class="step-by-step"]
[Zurück](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
[Weiter](configuring-a-database-server-for-web-deploy-publishing.md)
