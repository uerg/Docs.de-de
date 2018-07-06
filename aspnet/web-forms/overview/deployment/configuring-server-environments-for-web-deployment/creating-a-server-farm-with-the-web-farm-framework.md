---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Erstellen eine Serverfarm mit Webfarmframework | Microsoft-Dokumentation
author: jrjlee
description: Dieses Thema beschreibt, wie Sie mit der Web Farm Framework (WFF) 2.0 zum Erstellen und Konfigurieren einer Webfarm-Server aus einer Auflistung von Servern.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 5fdd1088f1f4e21f5e79b8c8379b7865f825ad53
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808939"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Erstellen eine Serverfarm mit Webfarmframework
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Sie mit der Web Farm Framework (WFF) 2.0 zum Erstellen und Konfigurieren einer Webfarm-Server aus einer Auflistung von Servern.


WFF können Sie Web Platform-Produkte und Komponenten, Webanwendungen, Websites und Konfigurationseinstellungen auf mehrere Lastenausgleich-Webserver zu synchronisieren. In Szenarien, in denen man mehr als eine Webserver, wie z.B. Staging-und produktionsumgebungen, kann dies den Prozess für Bereitstellung und Konfiguration-GUIs erheblich vereinfachen. Sie können eine Webanwendung auf einem einzelnen Server bereitstellen&#x2014;der *Primärserver*&#x2014;und WFF werden automatisch repliziert, die dieser Webanwendung auf alle anderen Webservern in der Serverfarm.

## <a name="understanding-the-web-farm-framework"></a>Grundlegendes zu Webfarmframework

Sie können die WFF 2.0 verwenden, bereitstellen, verwalten und Bereitstellen von Inhalten für eine Gruppe von Webservern. Eine WFF-Bereitstellung besteht aus drei der wichtigsten Serverrollen:

- Die *Controllerserver*. Sie verwenden diesen Server zum Erstellen und Konfigurieren von WFF-Serverfarmen. Der Controller-Server verwaltet die Synchronisierung von webplattformkomponenten, Konfigurationseinstellungen und Anwendungen zwischen den Webservern in einer Serverfarm. Sie WFF-2.0 installieren, auf dem Controllerserver, und der Controller-Server wird wiederum der WFF-Agent auf allen Servern in einer Serverfarm installiert. Der Controller-Server gehört nicht grundsätzlich zu keiner WFF-Serverfarm und ein einzelnen Controller-Server kann mehrere Serverfarmen verwalten. In diesem Szenario verwenden Sie eine einzelne WFF-Controller-Server zu erstellen und Verwalten der staging-Serverfarm und die Serverfarm für die Produktion.
- Die *Primärserver*. Jede WFF-Server-Farm enthält einen einzelnen primären Server. Wenn Sie Webkomponenten für die Plattform zu installieren oder Bereitstellen von Anwendungen auf dem primären Server, synchronisiert der WFF Ihre Änderungen an allen anderen Servern in der Serverfarm.
- Die *Sekundärserver*. Jede WFF-Server-Farm enthält mindestens einen sekundären Server. Mit dem primären Server vorgenommene Änderungen werden auf jedem sekundären Server innerhalb der Serverfarm repliziert.

Dies zeigt, wie diese Serverrollen für die Staging- und produktionsumgebungen Umgebungen von Fabrikam, Inc. beziehen:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

In diesem Szenario werden die Staging- und produktionsumgebung sowohl als WFF-Serverfarmen konfiguriert. Einem einzelnen WFF-Controller-Server verwaltet die beiden Farmen. In der Serverfarm werden alle Änderungen an den primären Server auf jedem sekundären Server repliziert.

Bevor Sie beginnen, konfigurieren Sie Ihre Umgebungen Staging und Produktion, empfehlen wir, lesen Sie diesen Artikel mit den grundlegenden Konzepten von WFF 2.0 vertraut:

- [Übersicht über das Webfarmframework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Das Einrichten einer Serverfarm mit Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805127)
- [System- und Plattformanforderungen zum Webfarmframework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Übersicht über den Task

Um die Aufgaben und exemplarische Vorgehensweisen in diesem Thema abzuschließen, benötigen Sie mindestens drei Server&#x2014;einen WFF-Controller, eine primäre Web-Server für die Serverfarm und einen oder mehrere sekundäre Webserver für die Serverfarm. Sie können weitere sekundäre Server mit einer Serverfarm WFF zu einem beliebigen Zeitpunkt hinzufügen. Auf einer hohen Ebene zum Erstellen und Konfigurieren einer WFF-Serverfarm für die Staging- oder produktionsumgebung Umgebung benötigen Sie zu:

- Erstellen Sie einen Controllerserver, durch die Installation von Internet Information Services (IIS) 7.5 und WFF-2.0.
- Bereiten Sie primäre und sekundäre Server, indem Sie eine allgemeine Administratorkonto erstellen und Konfigurieren von Firewallausnahmen.
- Konfigurieren Sie die Serverfarm mit IIS-Manager auf dem Controllerserver.
- Konfigurieren des Lastenausgleichs mit IIS Application Request Routing (ARR) oder eine alternative Technologie für den Lastenausgleich.

Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie mit "sauberen" Server-Builds, die unter Windows Server 2008 R2 beginnen. Bevor Sie beginnen, können Sie für jeden Server, stellen Sie sicher, dass:

- Windows Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.
- Der Server ist die Domäne eingebunden.
- Der Server hat eine statische IP-Adresse.

> [!NOTE]
> Weitere Informationen zum Einbinden von Computern in einer Domäne, finden Sie unter [Hinzufügen von Computern zur Domäne und Anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren von statischen IP-Adressen finden Sie unter [Konfigurieren einer statischen IP-Adresse](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Erstellen Sie den WFF-Controller-Server

Um einen WFF-Controller-Server zu erstellen, müssen Sie IIS 7 oder höher und WFF 2.0 oder höher installieren. Im Hintergrund verwendet WFF das IIS-Webbereitstellungstool (Web Deploy) 2.x auf die Server in der Farm zu synchronisieren. Wenn Sie den Webplattform-Installer verwenden, um WFF zu installieren, wird automatisch das Installationsprogramm herunterladen und installieren Sie Web Deploy für Sie.

**Um den WFF-Controller-Server zu erstellen.**

1. Herunterladen und Installieren der [Webplattform-Installer](https://go.microsoft.com/?linkid=9739157).
2. Am oberen Rand der **Web Platform Installer 3.0** Fenster, klicken Sie auf **Produkte**.
3. Klicken Sie auf der linken Seite des Fensters, klicken Sie im Navigationsbereich auf **Server**.
4. In der **empfohlenen IIS 7-Konfiguration** auf **hinzufügen**.
5. In der <strong>Web Farm Framework 2.</strong> <em>x</em> auf <strong>hinzufügen</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Klicken Sie auf **Installieren**. Beachten Sie, dass der Webplattform-Installer die Installationsliste das Tool zur Webbereitstellung, zusammen mit verschiedenen anderen Abhängigkeiten hinzugefügt hat.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Lesen Sie die Lizenzbedingungen, und wenn Sie den Bedingungen zustimmen, klicken Sie auf **akzeptieren**.
8. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**, und schließen Sie dann die **Web Platform Installer 3.0** Fenster.

## <a name="configure-the-primary-and-secondary-servers"></a>Konfigurieren Sie die primären und sekundären Server

Vor der Erstellung einer WFF-Serverfarm sollten Sie einige Vorbereitungsaufgaben zur auf den Servern ausführen, die der Farm bilden:

- Hinzufügen von Firewallausnahmen, ermöglichen die **Kernnetzwerk**, **Remoteverwaltung**, und **Datei- und Druckerfreigabe** Funktionen für die Kommunikation mit dem WFF-Controller-Server .
- Erstellen Sie ein Domänenkonto (z. B. **FABRIKAM\stagingfarm**) in Active Directory und fügen sie der lokalen Administratorgruppe auf jedem Server hinzu. Sie verwenden dieses Konto als das Server-Farm-Administratorkonto beim Erstellen der Serverfarm.

Weitere Informationen zum Konfigurieren von dieser Firewallausnahmen in der Windows-Firewall finden Sie unter [System- und Plattformanforderungen für mit der Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805128). Andere Firewallsysteme finden Sie in der Produktdokumentation.

Sie können im nächste Verfahren verwenden, ein Domänenkonto der lokalen Administratorengruppe in Windows Server 2008 R2 hinzu. Führen Sie dieses Verfahren auf jedem Server, die Sie mit der Serverfarm hinzufügen möchten&#x2014;in anderen Worten, das gleiche Domänenkonto auf der lokalen Administratorgruppe auf dem primären Server und auf jedem sekundären Server hinzufügen.

**Ein Domänenkonto der lokalen Administratorgruppe hinzu**

1. Auf der **starten** Startmenü **Verwaltung**, und klicken Sie dann auf **Server-Manager**.
2. In der **Server-Manager** Fenster im Strukturansichtsbereich, erweitern Sie **Konfiguration**, erweitern Sie **lokale Benutzer und Gruppen**, und klicken Sie dann auf **Gruppen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. In der **Gruppen** Bereich doppelklicken Sie auf **Administratoren**.
4. In der **Administratoreigenschaften** Dialogfeld klicken Sie auf **hinzufügen**.
5. In der **Auswahl von Benutzern, Computern, Dienstkonten oder Gruppen** (Dialogfeld), Typ (oder Durchsuchen) auf Ihrem Domänenkonto anzumelden (z. B. **FABRIKAM\stagingfarm**), und klicken Sie dann auf **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. In der **Administratoreigenschaften** Dialogfeld klicken Sie auf **OK**.

Ihre Server sind jetzt mit einer Serverfarm hinzugefügt werden. Wenn der primäre Server, können Sie den Server, um die Anforderungen Ihrer Anwendung vor oder nach dem Erstellen der Serverfarm konfigurieren&#x2014;in beiden Fällen werden die WFF Server synchronisiert, durch die Bereitstellung der gleichen Produkte, Komponenten oder der Konfiguration mit Ihren sekundären Servern. Aus Gründen der Einfachheit halber wird in in diesem Tutorial davon ausgegangen, dass Sie den primären Server konfigurieren müssen, wenn Sie die Serverfarm erstellt haben.

## <a name="create-the-wff-server-farm"></a>Erstellen Sie die WFF-Serverfarm

An diesem Punkt sind alle Ihre Server mit einer Serverfarm WFF hinzugefügt werden:

- Sie haben die WFF auf Controller-Server installiert.
- Sie haben die Firewallausnahmen auf Ihre primären und sekundären Webserver konfiguriert.
- Sie haben ein Domänenkonto zur lokalen Administratorgruppe auf Ihren Webservern mit primären und sekundären hinzugefügt.

Der nächste Schritt ist die Erstellung die Serverfarm in WFF. Sie können in IIS-Manager auf dem WFF-Controller-Server dazu.

**Um eine WFF-Serverfarm zu erstellen.**

1. Auf dem WFF-Controller-Server auf die **starten** , zeigen Sie auf **Verwaltung**, und klicken Sie dann auf **(Internet Information Services, IIS) Manager**.
2. In der **Verbindungen** Bereich, erweitern Sie den lokalen Server-Knoten, mit der rechten Maustaste **Serverfarmen**, und klicken Sie dann auf **Serverfarm erstellen**.
3. In der **Serverfarm erstellen** Dialogfeld geben einen aussagekräftigen Namen für die Serverfarm (z. B. **Staging Farm**), und wählen Sie dann **Serverfarm bereitstellen**.
4. Geben Sie den Benutzernamen und das Kennwort des Domänenkontos ein, dem Sie der lokalen Administratorengruppe auf jedem Server hinzugefügt haben.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Klicken Sie auf **Weiter**.
6. Auf der **Hinzufügen von Servern** Seite Geben Sie den vollqualifizierten Domänennamen (FQDN) des primären Servers, auf **Primärserver**, und klicken Sie dann auf **hinzufügen**.
7. An diesem Punkt versucht WFF wenden Sie sich an den primären Server mit den Anmeldeinformationen, die Sie angegeben haben. Wenn die Verbindung erfolgreich ist, wird der primäre Server in der Tabelle hinzugefügt werden, auf die **Hinzufügen von Servern** Seite.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Sie haben vielleicht bemerkt, die **Server steht für den Lastenausgleich** ist standardmäßig aktiviert. WFF verwendet das IIS-ARR-Modul zum Lastenausgleich zu implementieren und Anforderungen auf die Webserver in Ihrer Serverfarm zu verteilen. Sie würden nur deaktivieren, in den meisten Szenarien die **Server ist für Lastenausgleich verfügbar** option, wenn Sie eine Drittanbieter-lastenausgleichslösung stattdessen verwenden möchten.
8. Auf der **Hinzufügen von Servern** Seite Geben Sie den FQDN Ihres ersten sekundären Servers, und klicken Sie dann auf **hinzufügen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Wiederholen Sie Schritt 7 für alle weiteren sekundären Server in der Farm aus, und klicken Sie dann auf **Fertig stellen**.

Die WFF-Server-Farm wird nun ausgeführt. Alle Web-Plattform-Produkten oder -Komponenten, die Sie installieren, auf dem primären Server und -Webanwendungen oder Inhalte, die Sie auf dem primären Server werden automatisch auf allen sekundären Servern bereitgestellt werden.

WFF ist eine umfassende und komplexes Thema, und Sie können weitere Informationen auf der [Microsoft Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805129) Website. Vorerst, gibt es jedoch zwei Features stehen zur Verfügung, denen Sie berücksichtigen müssen:

- *Bereitstellung von Anwendungen auf* ist der Prozess, der Inhalt vom primären Server, z. B. Webanwendungen und Konfigurationseinstellungen auf allen sekundären Servern in der Farm repliziert. Z. B. Wenn Sie die beispiellösung Contact Manager an Ihrem primären Stagingserver bereitstellen, wird der WFF-Bereitstellungsprozess der Anwendung dieser Lösung bereitstellen für alle sekundären staging-Server. Standardmäßig wird der Bereitstellungsprozess für die Anwendung alle 30 Sekunden ausgeführt.
- *Plattformbereitstellung* ist der Prozess, der Web-Plattform-Produkte und Komponenten auf dem primären Server an alle sekundären Server in der Serverfarm synchronisiert wird. Z. B. bei der Installation von ASP.NET MVC 3 auf Ihrem primären Stagingserver verwendet im Rahmen des Bereitstellungsprozesses für die Plattform des Webplattform-Installers ASP.NET MVC 3 auf allen sekundären staging Servern installieren. Im Rahmen des Bereitstellungsprozesses für die Plattform wird standardmäßig alle fünf Minuten ausgeführt.

Sie können auf Ihre WFF-Controller-Server auf dem IIS-Manager grundlegenden Anwendungs- und Einstellungen für die Bereitstellung der Plattform verwalten.

**Untersuchen der Anwendungs- und Plattformebene, die Einstellungen für die Bereitstellung**

1. Im IIS-Manager in der **Verbindungen** Bereich Ihrer Serverfarm auswählen.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. In der **Serverfarm** Bereich doppelklicken Sie auf **Anwendungsbereitstellung**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Wie Sie sehen können, ist derzeit die Serverfarm konfiguriert, um webeinstellungen Inhalte und Konfigurationen zwischen dem primären Server und die sekundären Server alle 30 Sekunden zu synchronisieren.
4. Klicken Sie auf **wieder**, und doppelklicken Sie dann auf **Plattformbereitstellung**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Wie Sie sehen können, ist derzeit die Serverfarm konfiguriert, um alle fünf Minuten Web-Plattform-Produkte und Komponenten, die zwischen dem primären Server und den sekundären Servern zu synchronisieren.
6. Klicken Sie auf **wieder**.
7. Erzwingen Sie die Serverfarm mit Web-Plattformprodukte sofort synchronisiert in die **Aktionen** Bereich, klicken Sie auf **Plattform bereitstellen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Plattformbereitstellung kann einige Zeit dauern. Der Installer-Prozess, die im Hintergrund auf den sekundären Servern in einer Serverfarm ausgeführt wird.
8. Nachdem Sie ausreichend Zeit für die auf den Abschluss des Bereitstellungsvorgangs eingeräumt haben, können Sie überprüfen, dass die Produkte und Komponenten, die Sie an den primären Server hinzugefügt haben, nun auf den sekundären Servern repliziert wurden. Sie können z. B. an einem sekundären Server anmelden und Verwenden der **Server-Manager** Fenster, um sicherzustellen, dass die Webserverrolle installiert wurde.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Sie können auch überprüfen, dass die Liste der installierten Programme um sicherzustellen, dass verschiedene webplattformkomponenten hinzugefügt wurden.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Konfigurieren des Lastenausgleichs

Bei der Erstellung einer Webfarm müssen Sie eine Form des Lastenausgleichs zum Verteilen von HTTP-Anforderungen zwischen Ihre Webserver einrichten. Dies kann es sich um Windows Server 2008 Netzwerklastenausgleich, IIS-ARR, oder ein Drittanbieter-Software oder Hardware-basierten Lösung mit Lastenausgleich sein.

WFF dient Sie eng mit IIS ARR. integrieren Um diese Integration nutzen zu können, müssen Sie das ARR-Modul auf dem WFF-Controller-Server installieren. Sie anweisen, klicken Sie dann Ihren Web-Datenverkehr an den Controllerserver, in der Regel durch die Datensätze des Domain Name System (DNS) konfigurieren. Der Controller-Server wird dann eingehende Anforderungen zwischen den Servern in der Farm, die basierend auf der serververfügbarkeit und verschiedene andere Kriterien verteilen.

> [!NOTE]
> Sie müssen keine ARR mit WFF verwenden; Sie können WFF zum Arbeiten mit Drittanbieter-Lösungen für den Netzwerklastenausgleich konfigurieren. Weitere Informationen finden Sie unter [Überblick über das Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805126).


Lastenausgleich mit ARR ist ein komplexes Thema, das meisten der würde den Rahmen dieses Lernprogramms ist. Allerdings können Sie das nächste Verfahren zum Installieren des ARR-Moduls und erste Schritte mit Lastenausgleich.

**Zum Einrichten des Lastenausgleichs auf WFF Controller-server**

1. Starten Sie den Webplattform-Installer, auf den WFF-Controller-Server.
2. Am oberen Rand der **Web Platform Installer 3.0** Fenster, klicken Sie auf **Produkte**.
3. Klicken Sie auf der linken Seite des Fensters, klicken Sie im Navigationsbereich auf **Server**.
4. In der **Application Request Routing 2.5** auf **hinzufügen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Klicken Sie auf **installieren**, und befolgen Sie dann die Anweisungen in der **Webplattforminstallation** Fenster.
6. Starten Sie IIS-Manager, wenn die Installation abgeschlossen ist, und klicken Sie in der **Verbindungen** Bereich, klicken Sie auf Ihrem Server-farmknoten. Beachten Sie, die mehrere neue Symbole hinzugefügt wurden die **Serverfarm** Bereich.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. In der **Serverfarm** Bereich doppelklicken Sie auf **Lastenausgleich**.
8. In der **Load Balance** Bereich, wählen Sie eine Auslastung ausgleichen Algorithmus (z. B. **mindestens die aktuelle Anforderung**).

    > [!NOTE]
    > Weitere Informationen zu den Lastenausgleich, Algorithmen und andere Konfigurationseinstellungen, finden Sie unter [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. In der **Aktionen** Bereich, klicken Sie auf **übernehmen**.

Sie haben jetzt die grundlegenden Lastenausgleich für die Server in Ihrer Serverfarm konfiguriert. Wenn Sie alle Ihre Farm Webdatenverkehr an den Controller-Server weiterleiten, werden die Anforderungen zwischen den Servern in der Farm gemäß der Verfügbarkeit und den Lastenausgleichsalgorithmus gewählten verteilt.

Weitere Informationen zum Konfigurieren des Lastenausgleichs mit ARR finden Sie unter [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Überwachen Sie die Serverfarm

Sie können die Integrität Ihrer Serverfarm zu einem beliebigen Zeitpunkt über IIS-Manager auf dem Controllerserver überwachen. In der **Verbindungen** Bereich, erweitern Sie Ihrer Serverfarm, und klicken Sie dann auf **Server**. Der mittlere Bereich zeigt eine Zusammenfassung der einzelnen Server in der Farm zusammen mit einem Ablaufverfolgungsprotokoll für die letzten Aktivitäten.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Schlussbemerkung

Die WFF-Server-Farm sollte jetzt betriebsbereit sein. Sie können den primären Server zur Bereitstellungsansatz Unterstützung gewünscht konfigurieren&#x2014;finden Sie im Abschnitt "Weiterführende Literatur" Ausführliche&#x2014;und Ihre Konfiguration auf jedem sekundären Server in der Farm repliziert werden.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu allen Aspekten der Konfiguration und Verwendung der WFF finden Sie unter den [Microsoft Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805129) Website.

> [!div class="step-by-step"]
> [Zurück](configuring-a-database-server-for-web-deploy-publishing.md)
> [Weiter](configuring-deployment-properties-for-a-target-environment.md)
