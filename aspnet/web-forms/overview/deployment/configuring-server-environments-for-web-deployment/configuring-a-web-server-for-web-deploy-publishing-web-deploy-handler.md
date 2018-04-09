---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Konfigurieren einen Webserver für das Web Deploy-Veröffentlichung (Web Deploy-Handler) | Microsoft Docs
author: jrjlee
description: Dieses Thema beschreibt, wie ein Webserver (Internet Information Services, IIS) zur Unterstützung von Webpublishing und Bereitstellung mit der IIS Web bereitstellen Han konfigurieren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: d98be2859181e014ad332298ee3a572ad4235649
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Konfigurieren einen Webserver für das Web Deploy-Veröffentlichung (Web Deploy-Handler)
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie ein Webserver (Internet Information Services, IIS) zur Unterstützung von Webpublishing und Bereitstellung mit der IIS-Web-bereitstellen-Handler zu konfigurieren.
> 
> Wenn Sie mit Web Deploy 2.0 oder höher arbeiten, stehen drei wichtigsten Ansätze, die Sie verwenden können, um Ihre Anwendungen oder Websites auf einem Webserver abzurufen. Sie haben folgende Möglichkeiten:
> 
> - Verwenden der *Web Deploy Remote-Agent-Dienst*. Dieser Ansatz erfordert weniger Konfiguration des Webservers, jedoch müssen Sie die Anmeldeinformationen eines Administrators lokalen Server bereitstellen, um alle Elemente auf dem Server bereitstellen.
> - Verwenden der *Web Deploy-Handler*. Dieser Ansatz ist viel komplexer und erfordert mehr anfängliche Aufwand auf dem Webserver einrichten. Wenn Sie diesen Ansatz verwenden, können Sie IIS damit können Benutzer ohne Administratorrechte zum Ausführen der Bereitstellung konfigurieren. Der Web-bereitstellen-Handler ist nur in IIS, Version 7 oder höher verfügbar.
> - Verwendung *offline Bereitstellung*. Dieser Ansatz erfordert den geringsten Konfiguration des Webservers, ein Serveradministrator muss jedoch manuell kopieren Sie das Webpaket auf dem Server und über den IIS-Manager zu importieren.
> 
> Weitere Informationen zu den Hauptfunktionen, Vorteile und Nachteile dieser Ansätze, finden Sie unter [Auswählen der rechts Ansatz für die Webbereitstellung](choosing-the-right-approach-to-web-deployment.md).


Ja, wenn Sie nicht-Administratorbenutzer zum Bereitstellen von Inhalt auf bestimmte IIS-Websites zulassen möchten. Dieser Ansatz ist häufig wünschenswert in Szenarien:

- Staging- oder Produktionsserver Umgebungen, in dem die Person oder ein Dienst-Konto, das die Remotebereitstellung löst wahrscheinlich haben Zugriff auf die Anmeldeinformationen eines Serveradministrators wird.
- Gehostete Umgebungen, in dem Sie Remotebenutzer Zugriff auf ihren Websites zu aktualisieren, ohne Ihnen Vollzugriff auf den Webservern (oder den Zugriff auf Websites von einer anderen Person) zu gewähren möchten.

In Szenarien für Entwicklungs- oder testumgebung oder in kleineren Organisationen, Bereitstellen von Inhalt mithilfe von Server-Administratoranmeldeinformationen ist oft strittigen. In diesen Szenarien konfigurieren Sie Ihre Webserver zur Unterstützung der Bereitstellung mithilfe der [Web Bereitstellen von Remote-Agent-Dienst](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) bietet einen einfachen Ansatz.

## <a name="task-overview"></a>Übersicht über den Task

Zum Konfigurieren des Servers zum annehmen und Bereitstellen von Webpaketen von einem Remotecomputer mit dem Web bereitstellen Handler Ansatz müssen Sie Folgendes ausführen:

- Erstellen Sie, oder wählen Sie ein Domänenbenutzerkonto an (die "Benutzer ohne Administratorrechte"), dessen Anmeldeinformationen, die Sie zum Ausführen von Bereitstellungen verwenden.
- Installieren Sie IIS 7.5, einschließlich der Webverwaltungsdienst und das Modul die Standardauthentifizierung.
- Installieren Sie Web Deploy 2.1 oder höher.
- Konfigurieren Sie der Webverwaltungsdienst um Remoteverbindungen zuzulassen, und starten Sie den Dienst.
- Erstellen einer IIS-Website zum Hosten des bereitgestellten Inhalts.
- Gewähren Sie Ihre Benutzer ohne Administratorrechte auf Ihrer Website im IIS-Manager.
- Sicherstellen Sie, dass der Webverwaltungsdienst Delegierungsregeln ermöglichen den Dienst hinzufügen und Ändern von Website-Inhalte, die mit Ihrem Benutzerkonto ohne Administratorrechte.
- Konfigurieren von Firewalls für eingehende Verbindungen an Port 8172.

Um die beispiellösung ContactManager speziell zu hosten, müssen Sie außerdem auf:

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
- **IIS: Verwaltungsdienst**. Hiermit werden der Webverwaltungsdienst (WMSvc) in IIS installiert. Dieser Dienst ermöglicht die Remoteverwaltung von IIS-Websites und macht den Endpunkt Web bereitstellen Handler für Clients verfügbar.
- **IIS: Standardauthentifizierung**. Hiermit werden die IIS-Standardauthentifizierung-Modul installiert. Dadurch können die Web-Verwaltungsdienst (WMSvc) authentifizieren, die Anmeldeinformationen, die Sie bereitstellen.
- **Webbereitstellungstool 2.1 oder höher Web**. Hiermit werden Web Deploy (und die zugrunde liegende ausführbare Datei, MSDeploy.exe) auf dem Server installiert. Im Rahmen dieses Prozesses installiert der Web-Handler bereitstellen und der Webverwaltungsdienst integriert.
- **.NET Framework 4.0**. Dies ist erforderlich, um Anwendungen auszuführen, die für diese Version von .NET Framework erstellt wurden.
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

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. In der **ASP.NET MVC 3 (Visual Studio 2010)** auf **hinzufügen**.
7. Klicken Sie im Navigationsbereich auf **Server**.
8. In der **empfohlene Konfiguration von IIS 7** auf **hinzufügen**.
9. In der **Web Bereitstellung Tool 2.1** auf **hinzufügen**.
10. In der **IIS: Standardauthentifizierung** auf **hinzufügen**.
11. In der **IIS: Verwaltungsdienst** auf **hinzufügen**.
12. Klicken Sie auf **Installieren**. Zeigen Sie den Webplattform-Installer eine Liste mit Produkten&#x2014;zusammen mit verknüpften Abhängigkeiten&#x2014;installiert werden und werden Sie aufgefordert, die Lizenzbedingungen zu akzeptieren.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Überprüfen Sie die Lizenzbedingungen, und wenn Sie den Bedingungen zustimmen, klicken Sie auf **ich stimme**.
14. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**, und schließen Sie dann die **Web Platform Installer 3.0** Fenster.

Wenn Sie .NET Framework 4.0 vor der Installation von IIS installiert haben, müssen Sie zum Ausführen der [ASP.NET IIS-Registrierungstool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (Aspnet\_regiis.exe) auf die neueste Version von ASP.NET bei IIS registrieren. Wenn Sie nicht dies tun, Sie werden feststellen, dass IIS statischer Inhalte (z. B. HTML-Dateien bereitstellen) ohne Probleme zurückgegeben **HTTP-Fehler 404.0 – Nichtgefunden** beim Versuch, den ASP.NET-Inhalt, durchsuchen. Das nächste Verfahren können Sie sicherstellen, dass ASP.NET 4.0 registriert ist.

**Um ASP.NET 4.0 bei IIS registrieren**

1. Klicken Sie auf **starten**, und geben Sie dann **Eingabeaufforderung**.
2. In den Suchergebnissen mit der Maustaste **Eingabeaufforderung**, und klicken Sie dann auf **als Administrator ausführen**.
3. Wechseln Sie in das Eingabeaufforderungsfenster zu dem **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** Verzeichnis.
4. Geben Sie diesen Befehl, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Wenn Sie zum Hosten von 64-Bit-Webanwendungen zu einem beliebigen Zeitpunkt planen, sollten Sie auch die 64-Bit-Version von ASP.NET bei IIS registrieren. Navigieren Sie dazu im Eingabeaufforderungsfenster zu dem **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** Verzeichnis.
6. Geben Sie diesen Befehl, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Als empfiehlt sich verwenden Sie Windows Update erneut zu diesem Zeitpunkt herunterladen und installieren alle verfügbaren Updates für die neue Produkte und Komponenten, die Sie installiert haben.

## <a name="configure-the-web-management-service"></a>Den Webverwaltungsdienst konfigurieren

Nun, da Sie alles, was installiert haben, die Sie benötigen, besteht der nächste Schritt zum Konfigurieren der Webverwaltungsdienst in IIS. Auf hoher Ebene müssen Sie diese Aufgaben ausführen:

- Aktivieren der Standardauthentifizierung auf Serverebene.
- Der Webverwaltungsdienst für Remoteverbindungen zu konfigurieren.
- Den Webverwaltungsdienst wird gestartet.
- Überprüfen Sie, dass die erforderlichen Webverwaltungsdienst Delegierungsregeln vorhanden sind.

**Der Webverwaltungsdienst konfigurieren**

1. Auf der **starten** Sie im Menü **Verwaltung**, und klicken Sie dann auf **(Internet Information Services, IIS) Manager**.
2. Im IIS-Manager in der **Verbindungen** Bereich, klicken Sie auf den Serverknoten (z. B. **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. Im mittleren Bereich unter **IIS**, doppelklicken Sie auf **Authentifizierung**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. Mit der rechten Maustaste **Standardauthentifizierung**, und klicken Sie dann auf **aktivieren**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. In der **Verbindungen** Bereich, klicken Sie auf den Knoten für den Server erneut aus, um an den Einstellungen der obersten Ebene zurück.
6. Im mittleren Bereich unter **Management**, doppelklicken Sie auf **Verwaltungsdienst**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. Wählen Sie im mittleren Bereich **Remoteverbindungen**.

    > [!NOTE]
    > Wenn der Webverwaltungsdienst bereits ausgeführt wird, müssen Sie zuerst zu beenden.
8. In der **Aktionen** Bereich, klicken Sie auf **starten** zum Starten des Web-Management-Diensts.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Wenn Sie aufgefordert werden, die Einstellungen zu speichern, klicken Sie auf **Ja**.

    > [!NOTE]
    > Möglicherweise möchten Sie auch den Dienst für den automatischen start konfigurieren. Zu diesem Zweck öffnen Sie die Dienstekonsole der rechten Maustaste auf **Webverwaltungsdienst**, und klicken Sie dann auf **Eigenschaften**. In der **Starttyp** Dropdownliste **automatische**, und klicken Sie dann auf **OK**.
10. In der **Verbindungen** Bereich, klicken Sie auf den Knoten für den Server erneut aus, um an den Einstellungen der obersten Ebene zurück.
11. Im mittleren Bereich unter **Management**, doppelklicken Sie auf **Management Service Delegierung**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Stellen Sie sicher, dass der Mitte einen Satz von Regeln enthält.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Diese Regeln können autorisierte Benutzer Webverwaltungsdienst verschiedene Web Deploy-Anbieter verwenden. Z. B. zum Bereitstellen von Webanwendungen und Inhalt mit IIS über die Web-Handler bereitstellen, es muss eine Delegierung zulässt, dass alle authentifizierten Benutzer Webverwaltungsdienst verwenden die **ContentPath** und **IisApp**  Anbieter (die letzte Regel, die im Screenshot angezeigt werden können).

    Wenn Sie in der Reihenfolge, in diesem Thema beschriebenen Produkte und Komponenten installiert haben, sollten die neueste Version von Web Deploy der Webverwaltungsdienst automatisch alle erforderlichen Delegierungsregeln hinzufügen. Wenn alle Regeln, die von der Seite "Management Service Delegierung" nicht angezeigt werden, müssen Sie sie selbst erstellen. Informationen hierzu finden Sie unter [konfigurieren Sie das Web Deployment Handler](https://go.microsoft.com/?linkid=9805124).
13. In der **Verbindungen** Bereich, klicken Sie auf den Knoten für den Server erneut aus, um an den Einstellungen der obersten Ebene zurück.

## <a name="create-and-configure-an-iis-website"></a>Erstellen und Konfigurieren einer IIS-Website

Bevor Sie von Webinhalten zu Ihrem Server bereitstellen können, müssen Sie zum Erstellen und Konfigurieren einer IIS-Website zum Hosten des Inhalts. Web Deploy kann nur Webpaketen zu einer vorhandenen IIS-Website bereitstellen. Es kann nicht die Website erstellt werden. Sie müssen auch Vorgehensweise eine wenig zusätzliche Konfiguration, um Ihr Konto ohne Administratorrechte zum Bereitstellen von Inhalt Remote zu ermöglichen. Auf hoher Ebene müssen Sie diese Aufgaben ausführen:

- Erstellen Sie einen Ordner im Dateisystem, Ihre Inhalte hosten.
- Erstellen Sie eine IIS-Website, um den Inhalt zu fungieren, und den lokalen Ordner zuordnen.
- Erteilen von Leseberechtigungen für auf der Identität des Anwendungspools für den lokalen Ordner.
- Erteilen Sie die erforderlichen IIS-Berechtigungen für das Domänenkonto ein, das Ihre Webanwendung bereitgestellt wird.

Es gibt zwar keine beenden Sie aus der Bereitstellung von Inhalt auf der Standardwebsite in IIS, wird dieser Ansatz nicht für etwas anderes als Test oder diese Demonstration Szenarien empfohlen. Um eine produktionsumgebung zu simulieren, sollten Sie eine neue IIS-Website mit Einstellungen erstellen, die den Anforderungen Ihrer Anwendung spezifisch sind.

**So erstellen eine IIS-website**

1. Erstellen Sie einen Ordner zum Speichern Ihrer Inhalte im lokalen Dateisystem (z. B. **C:\DemoSite**).
2. Auf der **starten** Sie im Menü **Verwaltung**, und klicken Sie dann auf **(Internet Information Services, IIS) Manager**.
3. Im IIS-Manager in der **Verbindungen** Bereich, erweitern Sie den Serverknoten (z. B. **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Mit der rechten Maustaste die **Sites** Knoten, und klicken Sie dann auf **Website hinzufügen**.
5. In der **Standortname** geben einen Namen für die IIS-Website (z. B. **DemoSite**).
6. In der **physischen Pfad** können Sie eingeben (oder navigieren Sie zu) den Pfad zum lokalen Ordner (z. B. **C:\DemoSite**).
7. In der **Port** geben die Portnummer auf dem die Website gehostet werden soll (z. B. **85**).

    > [!NOTE]
    > Die standardmäßigen Portnummern sind 80 für HTTP und 443 für HTTPS. Wenn Sie diese Website an Port 80 hosten, müssen Sie die Standard-Website zu beenden, bevor Sie Ihre Website zugreifen können.
8. Lassen Sie die **Hostnamen** Feld leer, es sei denn, Sie verwenden möchten, konfigurieren einen Domain Name System (DNS)-Datensatz für die Website, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > In einer produktionsumgebung möchten Sie wahrscheinlich zum Hosten Ihrer Website an Port 80 und Konfigurieren eines Hostheaders, zusammen mit übereinstimmenden DNS-Datensätzen. Weitere Informationen zum Konfigurieren von Hostheadern in IIS 7 finden Sie unter [Konfigurieren eines Hostheaders für eine Website (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Weitere Informationen zu den DNS-Serverrolle in Windows Server 2008 R2, finden Sie unter [DNS-Server (Übersicht)](https://technet.microsoft.com/en-gb/library/cc770392.aspx) und [DNS-Server](https://technet.microsoft.com/windowsserver/dd448607).
9. In der **Aktionen** Bereich unter **Site bearbeiten**, klicken Sie auf **Bindungen**.
10. In der **Sitebindungen** (Dialogfeld), klicken Sie auf **hinzufügen**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. In der **Sitebindung hinzufügen** (Dialogfeld), legen die **IP-Adresse** und **Port** entsprechend die vorhandenen Website-Konfiguration.
12. In der **Hostnamen** geben den Namen des Webservers (z. B. **STAGEWEB1**), und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > Der erste Standort Bindung ermöglicht es Ihnen Zugriff auf die Website, die lokal mithilfe von IP-Adresse und Port oder `http://localhost:85`. Die zweite Bindungen pro Website können Sie Zugriff auf die Website von anderen Computern in der Domäne, die über den Computernamen (z. B. http://stageweb1:85).
13. In der **Sitebindungen** (Dialogfeld), klicken Sie auf **schließen**.
14. In der **Verbindungen** Bereich, klicken Sie auf **Anwendungspools**.
15. In der **Anwendungspools** Bereich Maustaste auf den Namen des Anwendungspools, und klicken Sie dann auf **Grundeinstellungen**. Standardmäßig wird der Name der Pool der Anwendung mit dem Namen Ihrer Website übereinstimmen (z. B. **DemoSite**).
16. In der **.NET Framework-Version** Liste **.NET Framework-v4.0.30319**, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

    > [!NOTE]
    > Die beispiellösung erfordert .NET Framework 4.0. Dies ist im Allgemeinen nicht zwingend für Web Deploy.

In der Reihenfolge für Ihre Website Inhalte bereitstellen muss die Identität des Anwendungspools auf den lokalen Ordner Leseberechtigungen verfügen, in dem den Inhalt gespeichert. Führen Sie Anwendungspools in IIS 7.5 und mit einer eindeutigen Identität des Anwendungspools standardmäßig (im Gegensatz zu früheren Versionen von IIS-Anwendungspools in der Regel Ausführungsort würde mit dem NETZWERKDIENST-Konto). Die Identität des Anwendungspools ist kein Konto echte Benutzer ist und nicht auf alle Listen von Benutzern oder Gruppen angezeigt&#x2014;, es wird stattdessen erstellt dynamisch beim Start des Anwendungspools. Jede Identität des Anwendungspools wird hinzugefügt, der lokalen **IIS\_IUSRS** Sicherheitsgruppe "als ausgeblendetes Element.

Um Berechtigungen für eine Anwendungspoolidentität auf eine Datei oder einen Ordner zu gewähren, haben Sie zwei Optionen:

- Zuweisen von Berechtigungen auf die Identität des Anwendungspools direkt unter Verwendung des Formats <strong>IIS AppPool\</ strong ><em>[Anwendungspoolname]</em>(z. B. <strong>IIS AppPool\DemoSite</strong>).
- Zuweisen von Berechtigungen für die **IIS\_IUSRS** Gruppe.

Der am häufigsten verwendete Ansatz ist die Zuweisung von Berechtigungen für die lokale **IIS\_IUSRS** gruppieren, da es sich bei diesem Ansatz können Sie Anwendungspools zu ändern, ohne dass eine Neukonfiguration Dateisystemberechtigungen. Im nächste Verfahren wird dieser Ansatz Gruppenbasierte verwendet.

> [!NOTE]
> Weitere Informationen zu Anwendungspoolidentitäten in IIS 7.5, finden Sie unter [Anwendungspoolidentitäten](https://go.microsoft.com/?linkid=9805123).


**So konfigurieren Sie Berechtigungen für die IIS-website**

1. Navigieren Sie in Windows-Explorer zum Speicherort des Ordners "lokale".
2. Mit der rechten Maustaste in des Ordners, und klicken Sie dann auf **Eigenschaften**.
3. Auf der **Sicherheit** auf **bearbeiten**, und klicken Sie dann auf **hinzufügen**.
4. Klicken Sie auf **Speicherorte**. In der **Speicherorte** (Dialogfeld), wählen Sie den lokalen Server, und klicken Sie dann auf **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. In der **Benutzer oder Gruppen auswählen** Geben Sie im Dialogfeld **IIS\_IUSRS**, klicken Sie auf **Namen überprüfen**, und klicken Sie dann auf **OK**.
6. In der <strong>Berechtigungen für</strong><em>[Ordnername]</em> (Dialogfeld), beachten Sie, die die neue Gruppe zugewiesen wurden die <strong>lesen &amp; ausführen</strong>, <strong>Ordner auflisten Inhalt</strong>, und <strong>lesen</strong> Berechtigungen standardmäßig. Lassen Sie Sie unverändert, und klicken Sie auf <strong>OK</strong>.
7. Klicken Sie auf <strong>OK</strong> schließen die <em>[Ordnername]</em><strong>Eigenschaften</strong> (Dialogfeld).

Als letzte Aufgabe müssen Sie die entsprechenden Berechtigungen für dem Benutzer ohne Administratorrechte gewähren, dessen Anmeldeinformationen, die Sie zum Bereitstellen von Inhalt verwenden. Dieser Benutzer benötigt die Berechtigungen zum Bereitstellen von Inhalt Remote auf Ihre Website.

**So konfigurieren Sie IIS-Website-Berechtigungen für ein Domänenbenutzerkonto ohne Administratorrechte**

1. Im IIS-Manager in der **Verbindungen** Bereich mit der rechten Maustaste des Knotens für die Website (z. B. **DemoSite**), zeigen Sie auf **bereitstellen**, und klicken Sie dann auf **Web konfigurieren Deploy-Veröffentlichung**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. In der **konfigurieren bereitstellen Webveröffentlichung** (Dialogfeld), rechts neben der **wählen Sie einen Benutzer auf die publishing-Berechtigungen erteilen,** Liste, klicken Sie auf die Schaltfläche mit den Auslassungspunkten.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. In der **Benutzer** Dialogfeld Geben Sie die Domäne und den Namen des Kontos, das Sie verwenden, um Inhalte bereitzustellen, und klicken Sie dann auf möchten **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. In der **konfigurieren bereitstellen Webveröffentlichung** (Dialogfeld), klicken Sie auf **Setup**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Dieser Vorgang führt zwei wichtige Funktionen in einem Schritt. Zunächst gewährt die Benutzer die Berechtigung zum Ändern der Websites Remote über die Webverwaltungsdienst gemäß den Delegierungsregeln, die Sie im vorherigen Abschnitt untersucht. Zweitens wird der Benutzer Vollzugriff auf den Quellordner für die Website ein, dem den Benutzer hinzufügen, ändern und Festlegen von Berechtigungen für die Website-Inhalte gewährt.
5. In der **konfigurieren bereitstellen Webveröffentlichung** (Dialogfeld), klicken Sie auf **schließen**.

## <a name="configure-firewall-exceptions"></a>Konfigurieren der Firewallausnahmen verfügbar

Standardmäßig lauscht der IIS-Webverwaltungsdienst an TCP-Port 8172. Wenn Windows-Firewall auf dem Webserver aktiviert ist, müssen Sie eine neue eingehende Regel zum Zulassen von TCP-Datenverkehr über Port 8172, der (alle ausgehender Datenverkehr wird standardmäßig in Windows-Firewall zulässig) erstellen. Wenn Sie eine Drittanbieter-Firewall verwenden, müssen Sie Regeln zum Zulassen des Datenverkehrs zu erstellen.

| Richtung | Von Port | So portieren Sie | Porttyp |
| --- | --- | --- | --- |
| Eingehende | Beliebig | 8172 | TCP |
| Ausgehende | 8172 | Beliebig | TCP |
  

Weitere Informationen zum Konfigurieren von Regeln in der Windows-Firewall finden Sie unter [Konfigurieren von Firewallregeln](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Firewalls von Drittanbietern finden Sie in der Produktdokumentation.

## <a name="conclusion"></a>Schlussbemerkung

Ihr Webserver sollte jetzt bereit zur Annahme von Remotebereitstellungen auf den Webdienst bereitstellen Handler über der Webverwaltungsdienst sein. Bevor Sie versuchen, eine Webanwendung mit dem Server bereitstellen, sollten Sie überprüfen Sie die folgenden wichtigen Punkte:

- Haben Sie die Standardauthentifizierung auf Serverebene in IIS aktiviert?
- Haben Sie Remoteverbindungen auf der Webverwaltungsdienst aktiviert?
- Haben Sie die Web-Management-Dienst gestartet?
- Gibt es Management Service-Delegierungsregeln vorhanden?
- Besitzt die Identität des Anwendungspools Lesezugriff für den Quellordner für Ihre Website?
- Verfügt das Benutzerkonto ohne Administratorrechte auf Standortebene Berechtigungen in IIS?
- Lässt die Firewall eingehende Verbindungen mit dem Server an TCP-Port 8172?

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Konfigurieren von benutzerdefinierter Microsoft Build Engine (MSBuild)-Projektdateien zum Bereitstellen von Webpaketen an den Handler für die Web-Bereitstellung finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Zurück](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [Weiter](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
