---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Konfigurieren eines Datenbankservers für Web Deploy-Veröffentlichung | Microsoft-Dokumentation
author: jrjlee
description: Dieses Thema beschreibt, wie Sie einen SQL Server 2008 R2-Datenbank-Server zur Unterstützung von Web-Bereitstellung und Veröffentlichung zu konfigurieren. In diesem Thema beschriebenen Aufgaben sind co...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 5563b737e3fd200070951f8d281ed8015137ae35
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384458"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Konfigurieren eines Datenbankservers aus, für Web Deploy-Veröffentlichung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Sie einen SQL Server 2008 R2-Datenbank-Server zur Unterstützung von Web-Bereitstellung und Veröffentlichung zu konfigurieren.
> 
> In diesem Thema beschriebenen Aufgaben gelten für jedes Bereitstellungsszenario&#x2014;es spielt keine Rolle, ob Ihre Webserver konfiguriert sind, auf der IIS-Webbereitstellungstool (Web Deploy) Remote-Agent-Dienst, das Bereitstellen von Web-Handler oder offline-Bereitstellung verwenden oder die Anwendung wird auf einem einzelnen Web-Server oder einer Serverfarm ausgeführt werden. Die Möglichkeit, die Sie die Datenbank bereitstellen kann gemäß den sicherheitsanforderungen und andere Überlegungen zu ändern. Beispielsweise können Sie die Datenbank, die mit oder ohne Beispieldaten bereitstellen, und Sie benutzerzuordnungen für die Rolle bereitstellen oder manuell nach der Bereitstellung zu konfigurieren. Die Möglichkeit, die Sie konfigurieren, dass den Datenbankserver bleibt jedoch identisch.


Sie haben keine zusätzliche Produkte oder Tools zu installieren, konfigurieren Sie einen Datenbankserver zur Unterstützung von Web-Bereitstellung. Vorausgesetzt, dass Ihr Datenbankserver und den Webserver auf unterschiedlichen Computern ausführen, müssen Sie einfach:

- Lassen Sie SQL Server für die Kommunikation über TCP/IP.
- SQL Server-Datenverkehr über eventuell vorhandene Firewalls zugelassen.
- Geben Sie dem Computerkonto des Web-Server eine SQL Server-Anmeldung.
- Ordnen Sie der kontoanmeldung für Computer, um alle erforderlichen Datenbankrollen.
- Gewähren Sie dem Konto, das der Bereitstellung eine SQL Server-Anmeldung und einen Datenbankbenutzer Ersteller Berechtigungen ausgeführt wird.
- Um mehrere Bereitstellungen zu unterstützen, ordnen Sie der Bereitstellung-kontoanmeldung an die **Db\_Besitzer** -Datenbankrolle.

In diesem Thema werden Sie zum Durchführen dieser Verfahren erläutert. Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie starten mit einer Standardinstanz von SQL Server 2008 R2 auf Windows Server 2008 R2 ausgeführt wird. Bevor Sie fortfahren, stellen Sie sicher, dass:

- Windows Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.
- Der Server ist die Domäne eingebunden.
- Der Server hat eine statische IP-Adresse.
- SQL Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.

SQL Server-Instanz muss nur enthalten die **Database Engine Services** Rolle, die automatisch in jeder SQL Server-Installation enthalten ist. Jedoch zur Vereinfachung der Konfiguration und Wartung, es wird empfohlen, dass Sie enthalten die **Verwaltungstools – einfach** und **Verwaltungstools – vollständig** Serverrollen.

> [!NOTE]
> Weitere Informationen zum Einbinden von Computern in einer Domäne, finden Sie unter [Hinzufügen von Computern zur Domäne und Anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren von statischen IP-Adressen finden Sie unter [Konfigurieren einer statischen IP-Adresse](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Weitere Informationen zum Installieren von SQL Server finden Sie unter [Installieren von SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Aktivieren des Remotezugriffs auf SQLServer

SQL Server verwendet die TCP/IP für die Kommunikation mit Remotecomputern. Wenn Sie Ihren Datenbankserver und den Webserver auf unterschiedlichen Computern befinden, müssen Sie:

- Konfigurieren Sie SQL Server-Netzwerkeinstellungen, um die Kommunikation über TCP/IP zu ermöglichen.
- Konfigurieren Sie alle Hardware oder Software-Firewalls zum Zulassen von TCP-Datenverkehr (und in einigen Fällen User Datagram Protocol (UDP) für Datenverkehr), auf die Ports, die die SQL Server-Instanz verwendet.

Um SQL Server für die Kommunikation über TCP/IP zu aktivieren, verwenden Sie SQL Server-Konfigurations-Manager, um die Netzwerkkonfiguration für Ihre SQL Server-Instanz zu ändern.

**So aktivieren Sie SQL Server für die Kommunikation über TCP/IP**

1. Auf der **starten** Startmenü **Programme**, klicken Sie auf **Microsoft SQL Server 2008 R2**, klicken Sie auf **Konfigurationstools**, und klicken Sie dann auf **SQL Server-Konfigurations-Manager**.
2. Erweitern Sie im Strukturansichtsbereich, **SQL Server-Netzwerkkonfiguration**, und klicken Sie dann auf **Protokolle für MSSQLSERVER**.

   > [!NOTE]
   > Wenn Sie mehrere Instanzen von SQL Server installiert haben, sehen Sie eine <strong>Protokolle für</strong><em>[Instanzname]</em> Element für jede Instanz. Sie müssen zum Konfigurieren von Netzwerkeinstellungen auf einer Instanz von Instanz Basis.
3. Klicken Sie im Detailbereich mit der Maustaste der **TCP/IP** Zeile, und klicken Sie dann auf **aktivieren**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. In der **Warnung** Dialogfeld klicken Sie auf **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Sie müssen den MSSQLSERVER-Dienst neu starten, damit die neue Netzwerkkonfiguration wirksam wird. Die an einer Eingabeaufforderung aus der Konsole Dienste oder aus SQL Server Management Studio möglich. In diesem Verfahren verwenden Sie SQL Server Management Studio.
6. SQL Server-Konfigurations-Manager zu schließen.
7. Auf der **starten** Startmenü **Programme**, klicken Sie auf **Microsoft SQL Server 2008 R2**, und klicken Sie dann auf **SQL Server Management Studio**.
8. In der **Herstellen einer Verbindung mit Server** Dialogfeld die **Servernamen** , geben Sie den Namen des Datenbankservers ein, und klicken Sie dann auf **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. In der **Objekt-Explorer** Bereich mit der rechten Maustaste in des übergeordneten Server-Knotens (z. B. **Datenbank "testdb1"**), und klicken Sie dann auf **neu starten**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. In der **Microsoft SQL Server Management Studio** Dialogfeld klicken Sie auf **Ja**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Wenn der Dienst neu gestartet wurde, schließen Sie SQL Server Management Studio.

Um SQL Server-Datenverkehr über eine Firewall zu ermöglichen, müssen Sie wissen, welche Ports Ihrer SQL Server-Instanz verwendet wird. Dies hängt wie SQL Server-Instanz erstellt und konfiguriert wurde:

- Ein *Standardinstanz* von SQL Server überwacht (und reagiert auf) Anforderungen an TCP-Port 1433.
- Ein *benannte Instanz* von SQL Server überwacht (und reagiert auf) die Anforderungen für einen dynamisch zugewiesenen TCP-Port.
- Wenn der SQL Server-Browser-Dienst aktiviert ist, können Clients den Dienst auf UDP-Port 1434, um herauszufinden, welche TCP-Port für eine bestimmte SQL Server-Instanz verwenden Abfragen. Dieser Dienst ist jedoch häufig aus Sicherheitsgründen deaktiviert.

Vorausgesetzt, dass Sie eine Standardinstanz von SQL Server verwenden, müssen Sie Ihre Firewall zum Zulassen von Datenverkehr zu konfigurieren.

| Richtung | Über Port | So portieren Sie | Porttyp |
| --- | --- | --- | --- |
| Eingehende | Beliebig | 1433 | TCP |
| Ausgehende | 1433 | Beliebig | TCP |
  

> [!NOTE]
> Technisch gesehen einen zufällig zugewiesenen TCP-Port zwischen 1024 und 5000 ein Clientcomputer für die Kommunikation mit SQL Server verwenden, und Sie können die Firewall-Regeln entsprechend einschränken. Weitere Informationen zu SQL Server-Ports und Firewalls, finden Sie unter [TCP/IP-Portnummern für die Kommunikation mit SQL über eine Firewall erforderlich](https://go.microsoft.com/?linkid=9805125) und [Vorgehensweise: konfigurieren ein Servers zur Überwachung eines bestimmten TCP-Ports (SQL Server-Konfiguration -Manager)](https://msdn.microsoft.com/library/ms177440.aspx).


In den meisten Windows Server-Umgebungen müssen Sie wahrscheinlich zum Konfigurieren von Windows-Firewall auf dem Datenbankserver. Standardmäßig lässt Windows-Firewall sämtlicher ausgehenden Datenverkehr, es sei denn, eine Regel ausdrücklich untersagt. Um Ihrem Webserver das Erreichen Ihrer Datenbank zu aktivieren, müssen Sie eine Eingangsregel zu konfigurieren, die TCP-Datenverkehr auf die Portnummer zugelassen wird, die SQL Server-Instanz verwendet. Wenn Sie eine Standardinstanz von SQL Server verwenden, können Sie das nächste Verfahren, mit dieser Regel zu konfigurieren.

**So konfigurieren Sie Windows-Firewall, damit die Kommunikation mit einer Standardinstanz von SQL Server**

1. Auf dem Datenbankserver auf die **starten** Startmenü **Verwaltung**, und klicken Sie dann auf **Windows-Firewall mit erweiterter Sicherheit**.
2. Klicken Sie im Strukturansichtsbereich, auf **Eingangsregeln**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. In der **Aktionen** Bereich unter **Eingangsregeln**, klicken Sie auf **neue Regel**.
4. In den neuen Assistenten für eingehende Regeln auf die **Regeltyp** Seite **Port**, und klicken Sie dann auf **Weiter**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Auf der **Protokoll und Ports** Seite **TCP** ausgewählt ist, und klicken Sie in der **bestimmte lokale Ports** geben **1433**, und klicken Sie dann auf **Weiter**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Auf der **Aktion** Seite, lassen Sie **Verbindung zulassen,** ausgewählt, und klicken Sie auf **Weiter**.
7. Auf der **Profil** Seite, lassen Sie **Domäne** ausgewählt haben, deaktivieren Sie die **Private** und **öffentliche** Kontrollkästchen, und klicken Sie dann auf  **Nächste**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Auf der **Namen** Seite, geben Sie der Regel einen entsprechend aussagekräftigen Namen (z. B. **SQL Server-Standardinstanz – Netzwerkzugriff**), und klicken Sie dann auf **Fertig stellen**.

Weitere Informationen zum Konfigurieren von Windows-Firewall für SQL Server, insbesondere, wenn Sie müssen mit SQL Server nicht standardmäßiger oder dynamische Ports zu kommunizieren, finden Sie unter [Vorgehensweise: Konfigurieren einer Windows-Firewall für Datenbank-Engine-Zugriff](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Konfigurieren von Anmeldungen und Berechtigungen

Wenn Sie eine Webanwendung zu IIS (Internetinformationsdienste) bereitstellen, führt die Anwendung mit der Identität des Anwendungspools. Verwenden Sie in einer domänenumgebung Anwendungspoolidentitäten das Computerkonto des Servers, auf denen sie Zugriff auf Netzwerkressourcen ausgeführt. Computerkonten werden in der Form <em>[Domänenname]</em><strong>\</ strong ><em>[Computername]</em><strong>$</strong>&#x2014;z. B. <strong>FABRIKAM\TESTWEB1$</strong>. Damit Ihre Webanwendung auf eine Datenbank über das Netzwerk zugreifen kann, müssen Sie:

- Fügen Sie eine Anmeldung für das Computerkonto des Web, mit der SQL Server-Instanz.
- Der Computer-kontoanmeldung alle erforderlichen Datenbankrollen zugeordnet (i. d. r. **Db\_Datareader** und **Db\_Datawriter**).

Wenn Ihre Webanwendung auf einer Serverfarm, statt auf einem einzelnen Server ausgeführt wird, müssen Sie diese Verfahren für jeden Webserver in der Serverfarm zu wiederholen.

> [!NOTE]
> Weitere Informationen zu den Identitäten des dienstanwendungspools und den Zugriff auf Netzwerkressourcen, finden Sie unter [Anwendungspoolidentitäten](https://go.microsoft.com/?linkid=9805123).


Sie können diese Aufgaben auf verschiedene Weise angehen. Um die Anmeldung zu erstellen, ist Folgendes möglich:

- Erstellen Sie den Anmeldenamen manuell auf dem Datenbankserver, die mithilfe von Transact-SQL oder SQL Server Management Studio.
- Verwenden Sie ein SQL Server 2008-Server-Projekt in Visual Studio zum Erstellen und Bereitstellen der Anmeldung.

SQL Server-Anmeldename ist ein Objekt auf Serverebene, anstatt ein Objekt auf Datenbankebene, daher ist es nicht abhängig von der Datenbank, die Sie bereitstellen möchten. Daher können Sie die Anmeldung zu jedem Zeitpunkt erstellen, und es ist häufig der einfachste Ansatz, erstellen Sie den Anmeldenamen manuell auf dem Datenbankserver vor der Bereitstellung von Datenbanken. Sie können im nächste Verfahren verwenden, um eine Anmeldung in SQL Server Management Studio zu erstellen.

**Zum Erstellen einer SQL Server-Anmeldung für das Computerkonto des Web-server**

1. Auf dem Datenbankserver auf die **starten** , zeigen Sie auf **Programme**, klicken Sie auf **Microsoft SQL Server 2008 R2**, und klicken Sie dann auf **SQL Server Management Studio** .
2. In der **Herstellen einer Verbindung mit Server** Dialogfeld die **Servernamen** , geben Sie den Namen des Datenbankservers ein, und klicken Sie dann auf **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. In der **Objekt-Explorer** Bereich mit der rechten Maustaste **Sicherheit**, zeigen Sie auf **neu**, und klicken Sie dann auf **Anmeldung**.
4. In der **Anmeldung – neu** Dialogfeld die **Anmeldename** geben den Namen des Computerkontos Ihrer Web-Server (z. B. **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Klicken Sie auf **OK**.

An diesem Punkt ist Ihrem Datenbankserver für die Web Deploy-Veröffentlichung bereit. Alle Lösungen, die Sie bereitstellen funktioniert jedoch nicht, bis Sie die erforderlichen Datenbankrollen der kontoanmeldung für Computer zuordnen. Zuordnen der Anmeldung zu Datenbankrollen erfordert viel mehr vorstellen, wie Sie können nicht Zuordnung Rollen erst nach dem haben bereitgestellt der Datenbank. Um die Anmeldung beim Computer Konto die erforderlichen Datenbankrollen zuzuordnen, ist Folgendes möglich:

- Weisen Sie die Datenbankrollen für die Anmeldung manuell, nachdem Sie die Datenbank zum ersten Mal bereitgestellt haben.
- Verwenden Sie ein Skript nach der Bereitstellung der Anmeldung die Datenbankrollen zuweisen.

Weitere Informationen zum Automatisieren der Erstellung von Anmeldungen und die rollenzuordnungen für die Datenbank, finden Sie unter [Bereitstellen von Datenbankrollenmitgliedschaften in Test-Umgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Im nächste Verfahren können Sie alternativ die kontoanmeldung für Computer manuell die erforderlichen Datenbankrollen zuzuordnen. Beachten Sie, dass Sie dieses Verfahren erst ausführen, können keine *nach* haben Sie die Datenbank bereitgestellt.

**Zuordnen von Datenbankrollen zu der Web-Server-Computer-kontoanmeldung**

1. Öffnen Sie SQL Server Management Studio wie bisher.
2. In der **Objekt-Explorer** Bereich erweitern Sie die **Sicherheit** Knoten erweitern Sie die **Anmeldungen** Knoten, und doppelklicken Sie dann auf der kontoanmeldung für Computer (z. B.  **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. In der **Anmeldungseigenschaften** Dialogfeld klicken Sie auf **Benutzerzuordnung**.
4. In der **Benutzer zugeordnet, die dieser Anmeldung** Tabelle, wählen Sie den Namen der Datenbank (z. B. **ContactManager**).
5. In der **Mitgliedschaft in Datenbankrolle für:** *[Datenbankname]* wählen Sie die erforderlichen Berechtigungen. Wählen Sie im Fall der Contact Manager-beispiellösung die **Db\_Datareader** und **Db\_Datawriter** Rollen.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Klicken Sie auf **OK**.

Während der manuellen Zuordnung Datenbankrollen oft mehr als ausreichend für testumgebungen ist, ist es nicht das bevorzugte Tag für automatisierte oder einmalklick-Bereitstellungen in Staging-oder produktionsumgebungen. Sie finden weitere Informationen zum Automatisieren von dieser Art von Aufgabe mithilfe von Skripts nach der Bereitstellung in [Bereitstellen von Datenbankrollenmitgliedschaften in Test-Umgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Weitere Informationen zu Server-Projekte und Datenbankprojekte, finden Sie unter [Visual Studio 2010 SQL Server-Datenbankprojekte](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Konfigurieren von Berechtigungen für das Bereitstellungskonto

Wenn das Konto, das Sie verwenden, um die Bereitstellung ausgeführt, die nicht SQL Server-Administrator ist, müssen Sie auch eine Anmeldung für dieses Konto zu erstellen. Um die Datenbank zu erstellen, muss das Konto ein Mitglied der **Dbcreator** Serverrolle oder über vergleichbare Berechtigungen verfügen.

> [!NOTE]
> Wenn Sie Web Deploy oder VSDBCMD verwenden, um eine Datenbank bereitzustellen, können Sie Windows oder SQL Server-Anmeldeinformationen, (wenn es sich um eine Instanz von SQL Server für die Unterstützung der Authentifizierung im gemischten Modus konfiguriert ist). Im nächste Verfahren wird davon ausgegangen, dass Sie Windows-Anmeldeinformationen verwenden möchten, aber nichts hindert, dass Sie einen SQL Server-Benutzernamen und Kennwort in der Verbindungszeichenfolge angeben, wenn Sie die Bereitstellung konfigurieren.


**Einrichten von Berechtigungen für das bereitstellungskonto**

1. Öffnen Sie SQL Server Management Studio wie bisher.
2. In der **Objekt-Explorer** Bereich mit der rechten Maustaste **Sicherheit**, zeigen Sie auf **neu**, und klicken Sie dann auf **Anmeldung**.
3. In der **Anmeldung – neu** Dialogfeld die **Anmeldename** geben den Namen Ihres Kontos für die Bereitstellung (z. B. **FABRIKAM\matt**).
4. In der **wählen Sie eine Seite** Bereich, klicken Sie auf **Serverrollen**.
5. Wählen Sie **Dbcreator**, und klicken Sie dann auf **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Um nachfolgende Bereitstellungen zu unterstützen, Sie müssen auch das Bereitstellen von Konto fügen die **Db\_Besitzer** Rolle in der Datenbank nach der ersten Bereitstellung. Dies ist, da bei nachfolgenden Bereitstellungen Sie können das Ändern des Schemas einer vorhandenen Datenbank, anstatt eine neue Datenbank erstellen. Wie im vorherigen Abschnitt beschrieben wird, können nicht Sie einen Benutzer zu einer Datenbankrolle hinzufügen, bis Sie die Datenbank aus offensichtlichen Gründen erstellt haben.

**Um die Bereitstellung-Konto-Anmeldung in der Datenbank zuzuordnen\_-Datenbank-Rolle "Besitzer"**

1. Öffnen Sie SQL Server Management Studio wie bisher.
2. In der **Objekt-Explorer** Fenster erweitern Sie die **Sicherheit** Knoten erweitern Sie die **Anmeldungen** Knoten, und doppelklicken Sie dann auf der kontoanmeldung für Computer (z. B.  **FABRIKAM\matt**).
3. In der **Anmeldungseigenschaften** Dialogfeld klicken Sie auf **Benutzerzuordnung**.
4. In der **Benutzer zugeordnet, die dieser Anmeldung** Tabelle, wählen Sie den Namen der Datenbank (z. B. **ContactManager**).
5. In der **Mitgliedschaft in Datenbankrolle für:** *[Datenbankname]* Liste der **Db\_Besitzer** Rolle.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Klicken Sie auf **OK**.

## <a name="conclusion"></a>Schlussbemerkung

Ihren Datenbankserver sollte jetzt bereit, akzeptieren Sie die Remotedatenbank-Bereitstellungen und remote IIS-Webserver auf Ihre Datenbanken zugreifen können. Bevor Sie versuchen, bereitstellen und Verwenden von Datenbanken, möchten Sie möglicherweise diese wichtigen Punkte überprüfen:

- Haben Sie die SQL Server zum Zulassen von TCP/IP-Remoteverbindungen konfiguriert?
- Haben Sie eventuell vorhandene Firewalls zum Zulassen von SQL Server-Datenverkehr konfiguriert?
- Haben Sie eine Computer-kontoanmeldung für jeden Webserver erstellt, die den Zugriff auf SQL Server wird?
- Schließt die Bereitstellung der Datenbank ein Skript zum Erstellen von benutzerzuordnungen für die Rolle aus, oder möchten Sie diese manuell erstellen, nachdem Sie die Datenbank zum ersten Mal bereitstellen?
- Haben Sie eine Anmeldung für das bereitstellungskonto erstellt und hinzugefügt, damit die **Dbcreator** Serverrolle?

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Bereitstellen von Datenbankprojekten, finden Sie unter [Bereitstellen von Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md). Anleitungen zum Erstellen von Datenbank-Rollenmitgliedschaften durch Ausführen eines Skripts nach der Bereitstellung finden Sie unter [Bereitstellen von Datenbankrollenmitgliedschaften in Test-Umgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Anleitungen dazu, wie Sie die individuelle Bereitstellung-Herausforderungen zu meistern, die Mitgliedschaft Datenbanken darstellen, finden Sie unter [Bereitstellen von Datenbankrollenmitgliedschaften in Enterprise-Umgebungen](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Zurück](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Weiter](creating-a-server-farm-with-the-web-farm-framework.md)
