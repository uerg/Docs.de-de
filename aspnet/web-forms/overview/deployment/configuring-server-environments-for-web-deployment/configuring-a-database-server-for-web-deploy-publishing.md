---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Konfigurieren einen Datenbankserver für Web Deploy-Veröffentlichung | Microsoft Docs
author: jrjlee
description: In diesem Thema beschreibt, wie einen SQL Server 2008 R2-Datenbankserver zur Unterstützung von Web Deploy und die Veröffentlichung zu konfigurieren. In diesem Thema beschriebenen Aufgaben sind co...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: a2340c0d561ed274e281b5f6d942af0a2027315a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885571"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Konfigurieren einen Datenbankserver für Web Deploy-Veröffentlichung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema beschreibt, wie einen SQL Server 2008 R2-Datenbankserver zur Unterstützung von Web Deploy und die Veröffentlichung zu konfigurieren.
> 
> Die in diesem Thema beschriebenen Tasks beziehen sich auf alle Bereitstellungsszenarien&#x2014;es spielt keine Rolle, ob Ihre Webserver konfiguriert sind, verwenden Sie die Remote-Agent-Dienst von IIS-Webbereitstellungstool (Web Deploy), das Bereitstellen von Web-Handler oder offline Bereitstellung oder die Anwendung wird auf einem einzelnen Webserver oder in einer Serverfarm ausgeführt. Die Bereitstellung der Datenbank kann entsprechend den sicherheitsanforderungen und andere Faktoren ändern. Beispielsweise können Sie die Datenbank mit oder ohne Beispieldaten, und Sie benutzerzuordnungen für die Rolle bereitstellen oder manuell konfigurieren, nach der Bereitstellung möglicherweise. Die Möglichkeit, die Sie konfigurieren, dass den Datenbankserver bleibt jedoch gleich.


Sie müssen zusätzliche Produkte oder -Tools installieren, konfigurieren einen Datenbankserver zur Unterstützung einer webbereitstellung. Vorausgesetzt, dass Ihr Datenbankserver und den Webserver auf unterschiedlichen Computern laufen, müssen Sie einfach:

- Lassen Sie SQL Server für die Kommunikation über TCP/IP.
- SQL Server-Datenverkehr über eventuell vorhandene Firewalls zugelassen.
- Geben Sie dem Computerkonto des Web-Server eine SQL Server-Anmeldung.
- Alle erforderlichen Datenbankrollen die Anmeldung beim Computer-Konto zuordnen.
- Gewähren Sie dem Konto, das der Bereitstellung einer SQL Server-Anmeldung und einen Datenbankbenutzer Ersteller Berechtigungen ausgeführt wird.
- Um Bereitstellungen zu unterstützen, ordnen Sie die Bereitstellung kontoanmeldung an die **Db\_Besitzer** -Datenbankrolle.

In diesem Thema erfahren Sie, wie Sie jede der folgenden Verfahren ausführen. Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie starten mit einer Standardinstanz von SQL Server 2008 R2 auf Windows Server 2008 R2 ausgeführt wird. Bevor Sie fortfahren, stellen Sie sicher, die:

- Windows Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.
- Der Server ist die Domäne.
- Der Server hat eine statische IP-Adresse.
- SQL Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.

SQL Server-Instanz muss nur enthalten die **Database Engine Services** Rolle, die automatisch in jeder SQL Server-Installation enthalten ist. Jedoch zur Vereinfachung der Konfiguration und Wartung, empfehlen wir, dass Sie enthalten die **Verwaltungstools – einfach** und **Verwaltungstools – vollständig** Serverrollen.

> [!NOTE]
> Weitere Informationen zum Hinzufügen von Computern zu einer Domäne, finden Sie unter [Verknüpfen von Computern zur Domäne und Anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren von statischen IP-Adressen finden Sie unter [eine statische IP-Adresse konfigurieren](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Weitere Informationen zum Installieren von SQL Server finden Sie unter [Installieren von SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Aktivieren des Remotezugriffs mit SQLServer

SQL Server verwendet die TCP/IP für die Kommunikation mit Remotecomputern. Wenn Ihr Datenbankserver und den Webserver auf unterschiedlichen Computern befinden, müssen Sie:

- Konfigurieren Sie Netzwerke SQL Server-Einstellungen zum Zulassen der Kommunikation über TCP/IP.
- Konfigurieren Sie alle Hardware oder Software-Firewalls so, dass TCP-Datenverkehr zugelassen (und in einigen Fällen (UDP = User Datagram Protocol)-Datenverkehr), für die Ports, die SQL Server-Instanz verwendet.

Um SQL Server die Kommunikation über TCP/IP zu aktivieren, verwenden Sie SQL Server-Konfigurations-Manager so ändern Sie die Netzwerkkonfiguration für die SQL Server-Instanz.

**So aktivieren Sie SQL Server für die Kommunikation über TCP/IP**

1. Auf der **starten** Sie im Menü **Programme**, klicken Sie auf **Microsoft SQL Server 2008 R2**, klicken Sie auf **Konfigurationstools**, und klicken Sie dann auf **SQL Server-Konfigurations-Manager**.
2. Erweitern Sie im Strukturansichtsbereich, **SQL Server-Netzwerkkonfiguration**, und klicken Sie dann auf **Protokolle für MSSQLSERVER**.

   > [!NOTE]
   > Wenn Sie mehrere Instanzen von SQL Server installiert haben, sehen Sie eine <strong>Protokolle für</strong><em>[Instanzname]</em> Element für jede Instanz. Sie müssen zum Konfigurieren von Netzwerkeinstellungen auf eine vollverteilte Instanz nach Instanz.
3. Klicken Sie im Detailbereich mit der Maustaste die **TCP/IP** Zeile, und klicken Sie dann auf **aktivieren**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. In der **Warnung** (Dialogfeld), klicken Sie auf **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Sie müssen den MSSQLSERVER-Dienst neu starten, bevor die Konfiguration des neuen Netzwerks wirksam werden. Sie können an einer Eingabeaufforderung über die Konsole Dienste oder von SQL Server Management Studio nachholen. In diesem Verfahren verwenden Sie SQL Server Management Studio.
6. SQL Server-Konfigurations-Manager zu schließen.
7. Auf der **starten** Sie im Menü **Programme**, klicken Sie auf **Microsoft SQL Server 2008 R2**, und klicken Sie dann auf **SQL Server Management Studio**.
8. In der **Verbindung mit Server herstellen** Dialogfeld die **Servernamen** Feld Geben Sie den Namen des Datenbankservers ein, und klicken Sie dann auf **verbinden**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. In der **Objekt-Explorer** Bereich mit der rechten Maustaste des Knotens der übergeordnete Server (z. B. **TESTDB1**), und klicken Sie dann auf **Neustart**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. In der **Microsoft SQL Server Management Studio** (Dialogfeld), klicken Sie auf **Ja**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Wenn der Dienst neu gestartet wurde, schließen Sie SQL Server Management Studio.

Um SQL Server-Datenverkehr über eine Firewall zuzulassen, müssen Sie zuerst wissen, welche Ports Ihrer SQL Server-Instanz verwendet wird. Dies hängt wie SQL Server-Instanz erstellt und konfiguriert wurde:

- Ein *Standardinstanz* überwacht SQL Server (und reagiert auf) die Anforderungen an TCP-Port 1433.
- Ein *benannte Instanz* überwacht SQL Server (und reagiert auf) Anforderungen auf einem dynamisch zugewiesenen TCP-Port.
- Wenn der SQL Server-Browser-Dienst aktiviert ist, können Clients den Dienst an UDP-Port 1434, um herauszufinden, welche TCP-Port für eine bestimmte SQL Server-Instanz verwenden Abfragen. Dieser Dienst ist jedoch häufig aus Sicherheitsgründen deaktiviert.

Vorausgesetzt, dass Sie eine Standardinstanz von SQL Server verwenden, müssen Sie zum Konfigurieren der Firewall zum Zulassen von Datenverkehr.

| Richtung | Von Port | So portieren Sie | Porttyp |
| --- | --- | --- | --- |
| Eingehende | Beliebig | 1433 | TCP |
| Ausgehende | 1433 | Beliebig | TCP |
  

> [!NOTE]
> Technisch gesehen ist ein Clientcomputer verwendet einen zufällig zugewiesenen TCP-Port zwischen 1024 und 5000 für die Kommunikation mit SQL Server, und Sie können einschränken, die Firewall-Regeln entsprechend. Weitere Informationen zu SQL Server-Ports und Firewalls, finden Sie unter [TCP/IP-Portnummern erforderlich, um Sie zu SQL über eine Firewall kommunizieren](https://go.microsoft.com/?linkid=9805125) und [Vorgehensweise: konfigurieren ein Servers zur Überwachung eines bestimmten TCP-Ports (SQL Server-Konfiguration -Manager)](https://msdn.microsoft.com/library/ms177440.aspx).


In den meisten Windows Server-Umgebungen müssen Sie wahrscheinlich die Windows-Firewall auf dem Datenbankserver zu konfigurieren. Standardmäßig ermöglicht Windows-Firewall gesamte ausgehende Verkehr, es sei denn, eine Regel ausdrücklich untersagt. Um Ihrem Webserver zum Erreichen Ihrer Datenbank zu aktivieren, müssen Sie eine eingehende Regel zu konfigurieren, die TCP-Datenverkehr über die Portnummer ermöglicht wird, die SQL Server-Instanz verwendet. Wenn Sie eine Standardinstanz von SQL Server verwenden, können Sie dem nächsten Verfahren fort, um diese Regel zu konfigurieren.

**So konfigurieren Sie Windows-Firewall zum Zulassen der Kommunikation mit einer Standardinstanz von SQL Server**

1. Auf dem Datenbankserver auf die **starten** Sie im Menü **Verwaltung**, und klicken Sie dann auf **Windows-Firewall mit erweiterter Sicherheit**.
2. Klicken Sie im Strukturansichtsbereich, auf **eingehende Regeln**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. In der **Aktionen** Bereich unter **eingehende Regeln**, klicken Sie auf **neue Regel**.
4. In der neue eingehende Regel-Assistenten auf der **Regeltyp** Seite **Port**, und klicken Sie dann auf **Weiter**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Auf der **Protokoll und Ports** Seite Sie sicher, dass **TCP** ausgewählt ist, und klicken Sie in der **bestimmte lokale Ports** geben **1433**, und klicken Sie dann auf **Weiter**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Auf der **Aktion** Seite, lassen Sie **Verbindung zulassen** ausgewählt, und klicken Sie auf **Weiter**.
7. Auf der **Profil** Seite, lassen Sie **Domäne** ausgewählt ist, deaktivieren Sie die **Private** und **öffentlichen** Kontrollkästchen, und klicken Sie dann auf  **Nächste**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Auf der **Namen** Seite, geben Sie der Regel einen geeigneter aussagekräftigen Namen (z. B. **SQL Server-Standardinstanz – Netzwerkzugriff**), und klicken Sie dann auf **Fertig stellen**.

Weitere Informationen zum Konfigurieren von Windows-Firewall für SQL Server, insbesondere, wenn Sie nicht standardmäßige oder dynamische Ports mit SQL Server zu kommunizieren, finden Sie unter [Vorgehensweise: Konfigurieren einer Windows-Firewall für Datenbankmodulzugriff](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Konfigurieren von Anmeldungen und Berechtigungen

Wenn Sie eine Webanwendung zu IIS (Internetinformationsdienste) bereitstellen, führt die Anwendung über die Identität des Anwendungspools. Verwenden Sie in einer domänenumgebung Anwendungspoolidentitäten das Computerkonto des Servers, auf denen sie den Zugriff auf Netzwerkressourcen ausgeführt. Computerkonten haben die Form <em>[Domänenname]</em><strong>\</ strong ><em>[Computername]</em><strong>$</strong>&#x2014;beispielsweise <strong>FABRIKAM\TESTWEB1$</strong>. Damit Ihre Webanwendung auf eine Datenbank über das Netzwerk zugreifen können, müssen Sie:

- SQL Server-Instanz eine Anmeldung für das Computerkonto des Web-Server hinzugefügt.
- Alle erforderlichen Datenbankrollen die Anmeldung beim Computer-Konto zuordnen (i. d. r. **Db\_Datareader** und **Db\_Datawriter**).

Wenn Ihre Webanwendung auf einer Serverfarm, anstatt von einem einzelnen Server ausgeführt wird, müssen Sie diese Verfahren für alle Webserver in der Serverfarm zu wiederholen.

> [!NOTE]
> Weitere Informationen zu Anwendungspoolidentitäten und den Zugriff auf Netzwerkressourcen, finden Sie unter [Anwendungspoolidentitäten](https://go.microsoft.com/?linkid=9805123).


Sie können diese Aufgaben auf verschiedene Weise angehen. Um die Anmeldung zu erstellen, können Sie entweder:

- Erstellen Sie den Anmeldenamen manuell auf dem Datenbankserver mit Transact-SQL oder SQL Server Management Studio.
- Verwenden eines SQL Server 2008-Server-Projekts in Visual Studio erstellen und Bereitstellen der Anmeldung.

SQL Server-Anmeldung ist ein Objekt auf Datenbankebene, statt eines Objekts auf Serverebene, daher ist es nicht der Datenbank abhängt, die Sie bereitstellen möchten. Daher können Sie die Anmeldung an einem beliebigen Punkt erstellen, und der einfachste Lösungsansatz ist jedoch häufig, erstellen Sie den Anmeldenamen manuell auf dem Datenbankserver Sie vor der Bereitstellung von Datenbanken. Das nächste Verfahren können Sie um eine Anmeldung in SQL Server Management Studio zu erstellen.

**So erstellen eine SQL Server-Anmeldung für das Computerkonto des Web-server**

1. Auf dem Datenbankserver auf die **starten** Sie im Menü **Programme**, klicken Sie auf **Microsoft SQL Server 2008 R2**, und klicken Sie dann auf **SQL Server Management Studio** .
2. In der **Verbindung mit Server herstellen** Dialogfeld die **Servernamen** Feld Geben Sie den Namen des Datenbankservers ein, und klicken Sie dann auf **verbinden**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. In der **Objektexplorer** Bereich mit der rechten Maustaste **Sicherheit**, zeigen Sie auf **neu**, und klicken Sie dann auf **Anmeldung**.
4. In der **Anmeldung – neu** Dialogfeld die **Anmeldename** geben den Namen des Computerkontos Ihrer Web-Server (z. B. **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Klicken Sie auf **OK**.

An diesem Punkt ist Ihrem Datenbankserver für die Web Deploy-Veröffentlichung bereit. Allerdings funktioniert keine Projektmappen, die Sie bereitstellen, bis Sie die erforderlichen Datenbankrollen die Anmeldung beim Computer-Konto zuordnen. Zuordnen der Anmeldung zu Datenbankrollen erfordert viel mehr betrachtet werden, als Sie können nicht Zuordnung Rollen erst nach dem haben bereitgestellt der Datenbank. Um die Anmeldung beim Computer Konto die erforderlichen Datenbankrollen zuzuordnen, ist Folgendes möglich:

- Weisen Sie die Datenbankrollen für die Anmeldung manuell, nachdem Sie die Datenbank zum ersten Mal bereitgestellt haben.
- Verwenden Sie ein Skript nach der Bereitstellung, um die Datenbankrollen für die Anmeldung zuzuweisen.

Weitere Informationen über das Automatisieren der Erstellung von Anmeldenamen und Datenbank-rollenzuordnungen, finden Sie unter [Bereitstellen von Datenbank-Rollenmitgliedschaften auf Testumgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Das nächste Verfahren können Sie alternativ die Anmeldung beim Computer Konto die erforderlichen Datenbankrollen manuell zuordnen. Denken Sie daran, dass Sie dieses Verfahren erst ausführen, können *nach* haben Sie die Datenbank bereitgestellt.

**Zuordnen von Datenbankrollen für die Web-Machine-Konto-Anmeldung**

1. Öffnen Sie SQL Server Management Studio wie bisher.
2. In der **Objekt-Explorer** Bereich erweitern Sie die **Sicherheit** Knoten, erweitern Sie die **Anmeldungen** Knoten, und doppelklicken Sie dann auf die kontoanmeldung für Computer (z. B.  **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. In der **Anmeldungseigenschaften** (Dialogfeld), klicken Sie auf **Benutzerzuordnung**.
4. In der **Benutzer, die dieser Anmeldung zugeordnet** table, wählen Sie den Namen der Datenbank (z. B. **ContactManager**).
5. In der **Mitgliedschaft in Datenbankrolle für:** *[Datenbankname]* wählen Sie die erforderlichen Berechtigungen. Wählen Sie im Fall der Beispielprojektmappe Manager wenden Sie sich an den **Db\_Datareader** und **Db\_Datawriter** Rollen.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Klicken Sie auf **OK**.

Obwohl manuell zuordnen von Datenbankrollen oft mehr als ausreichend für testumgebungen, ist es weniger wünschenswert für automatisierte oder einmalklick-Bereitstellungen auf Staging-oder produktionsumgebung. Weitere Informationen zum Automatisieren dieser Art der Aufgabe, die mithilfe von Skripts nach der Bereitstellung in finden Sie [Bereitstellen von Datenbank-Rollenmitgliedschaften auf Testumgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Weitere Informationen zu Server und Datenbankprojekte, finden Sie unter [Visual Studio 2010 SQL Server-Datenbankprojekte](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Konfigurieren von Berechtigungen für das Bereitstellungskonto

Wenn das Konto, mit dem Sie die Bereitstellung ausgeführt, die nicht SQL Server-Administrator ist, müssen Sie auch eine Anmeldung für dieses Konto zu erstellen. Um die Datenbank zu erstellen, muss das Konto ein Mitglied der **Dbcreator** Serverrolle oder über vergleichbare Berechtigungen verfügen.

> [!NOTE]
> Wenn Sie Web Deploy oder VSDBCMD zum Bereitstellen einer Datenbank verwenden, können Sie Windows-Anmeldeinformationen oder Anmeldeinformationen für SQL Server verwenden, (falls es sich um eine Instanz von SQL Server für die Unterstützung der Authentifizierung im gemischten Modus konfiguriert ist). Im nächste Verfahren wird davon ausgegangen, dass Sie Windows-Anmeldeinformationen verwenden möchten, aber es nichts hindert, dass Sie einen SQL Server-Benutzernamen und Kennwort in der Verbindungszeichenfolge angeben ist, wenn Sie die Bereitstellung konfigurieren.


**Zum Einrichten von Berechtigungen für das bereitstellungskonto**

1. Öffnen Sie SQL Server Management Studio wie bisher.
2. In der **Objektexplorer** Bereich mit der rechten Maustaste **Sicherheit**, zeigen Sie auf **neu**, und klicken Sie dann auf **Anmeldung**.
3. In der **Anmeldung – neu** Dialogfeld die **Anmeldename** geben den Namen Ihres Kontos für die Bereitstellung (z. B. **FABRIKAM\matt**).
4. In der **Seite auswählen** Bereich, klicken Sie auf **Serverrollen**.
5. Wählen Sie **Dbcreator**, und klicken Sie dann auf **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Um spätere Bereitstellungen zu unterstützen, Sie müssen auch das Bereitstellen von Konto fügen die **Db\_Besitzer** Rolle in der Datenbank nach der ersten Bereitstellung. Dies liegt daran für spätere Bereitstellungen Sie sind Ändern des Schemas einer vorhandenen Datenbank, anstatt eine neue Datenbank erstellen. Wie im vorherigen Abschnitt beschrieben wird, können nicht Sie einen Benutzer einer Datenbankrolle hinzufügen, bis Sie die Datenbank naheliegenden Gründen erstellt haben.

**Um die Bereitstellung-Konto-Anmeldung mit der Datenbank zuzuordnen\_Besitzer der Datenbank**

1. Öffnen Sie SQL Server Management Studio wie bisher.
2. In der **Objekt-Explorer** Fenster, erweitern Sie die **Sicherheit** Knoten, erweitern Sie die **Anmeldungen** Knoten, und doppelklicken Sie dann auf die kontoanmeldung für Computer (z. B.  **FABRIKAM\matt**).
3. In der **Anmeldungseigenschaften** (Dialogfeld), klicken Sie auf **Benutzerzuordnung**.
4. In der **Benutzer, die dieser Anmeldung zugeordnet** table, wählen Sie den Namen der Datenbank (z. B. **ContactManager**).
5. In der **Mitgliedschaft in Datenbankrolle für:** *[Datenbankname]* Liste der **Db\_Besitzer** Rolle.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Klicken Sie auf **OK**.

## <a name="conclusion"></a>Schlussbemerkung

Ihr Datenbankserver sollte jetzt bereit, um Remotedatenbank Bereitstellungen zu akzeptieren und remote IIS-Webserver für den Datenbankzugriff zu ermöglichen. Bevor Sie versuchen, bereitstellen und Verwenden von Datenbanken, sollten Sie überprüfen Sie die folgenden wichtigen Punkte:

- Haben Sie die SQL-Server zum Annehmen von TCP/IP-Remoteverbindungen konfiguriert?
- Haben Sie eventuell vorhandene Firewalls, damit SQL Server-Verkehr zugelassen konfiguriert?
- Haben Sie eine Computer-kontoanmeldung für jede Web-Server erstellt, die auf SQL Server zugreifen?
- Schließt die Bereitstellung der Datenbank ein Skript zum Erstellen von benutzerzuordnungen für die Rolle, oder müssen Sie diese manuell erstellen, nachdem Sie die Datenbank zum ersten Mal bereitstellen?
- Haben Sie eine Anmeldung für das bereitstellungskonto erstellt und hinzugefügt, damit die **Dbcreator** Serverrolle?

## <a name="further-reading"></a>Weiterführende Themen

Anleitung zum Bereitstellen von Datenbankprojekten, finden Sie unter [Datenbankprojekte bereitstellen](../web-deployment-in-the-enterprise/deploying-database-projects.md). Anleitungen zum Erstellen von Datenbank-Rollenmitgliedschaften durch Ausführen eines Skripts nach der Bereitstellung finden Sie unter [Bereitstellen von Datenbank-Rollenmitgliedschaften auf Testumgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Anleitung zur Verwendung der individuelle Bereitstellung Auflagen erfüllt werden können, die Mitgliedschaft Datenbanken darstellen, finden Sie unter [Mitgliedschaft-Datenbanken bereitstellen, um Unternehmensumgebungen](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Zurück](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Weiter](creating-a-server-farm-with-the-web-farm-framework.md)
