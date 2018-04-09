---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Erstellen eine Serverfarm mit Webfarmframework | Microsoft Docs
author: jrjlee
description: In diesem Thema wird beschrieben, wie Web Farm Framework (WFF) 2.0 zum Erstellen und Konfigurieren einer Webfarm-Server aus einer Auflistung von Servern verwendet wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 53a91660953795f2c55edcd795b053641d308dfe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Erstellen eine Serverfarm mit Webfarmframework
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Web Farm Framework (WFF) 2.0 zum Erstellen und Konfigurieren einer Webfarm-Server aus einer Auflistung von Servern verwendet wird.


Bereitzustellendes WFF können Sie Web Platform Produkte und Komponenten, Webanwendungen, Websites und Konfigurationseinstellungen auf mehrere Load Balancing Webserver synchronisieren. In Szenarien, in denen Sie mehr als ein Webserver, wie die Staging-und produktionsumgebungen kann dadurch enorm Ihrer Bereitstellung und Konfiguration vereinfacht werden. Sie können eine Webanwendung auf einem einzelnen Server bereitstellen&#x2014;der *Primärserver*&#x2014;und Replikation WFF automatisch auf alle anderen Webserver in der Serverfarm, die dieser Webanwendung.

## <a name="understanding-the-web-farm-framework"></a>Grundlegendes zu Webfarmframework

Sie können die WFF 2.0 verwenden, bereitstellen, verwalten und Bereitstellen von Inhalten für eine Gruppe von Webservern. Eine WFF-Bereitstellung besteht aus drei wichtige Serverrollen:

- Die *Controllerserver*. Sie verwenden diesen Server zum Erstellen und Konfigurieren von WFF-Serverfarmen. Der Controller-Server verwaltet die Synchronisierung von webplattformkomponenten, Konfigurationseinstellungen und Anwendungen, die Web-Server in einer Serverfarm. Sie WFF 2.0 auf dem Controllerserver installieren und der Controller-Server wird wiederum die WFF-Agent auf jedem Server in einer Serverfarm installiert. Der Controller-Server gehört nicht grundsätzlich zu keiner Serverfarm WFF und ein einzelnen Controller-Server kann mehrere Serverfarmen verwalten. In diesem Szenario verwenden Sie einen einzelnen WFF-Controller-Server zum Erstellen und Verwalten der staging-Serverfarm und die Serverfarm für die Produktion.
- Die *Primärserver*. Jede WFF-Server-Farm enthält einen einzelnen primären Server. Bei der Installation webplattformkomponenten oder Bereitstellen von Anwendungen auf dem primären Server, wird die WFF Ihre Änderungen mit allen anderen Servern in der Serverfarm synchronisiert.
- Die *Sekundärserver*. Jede WFF-Server-Farm enthält einen oder mehrere sekundäre Server. Alle vorgenommenen Änderungen an den primären Server werden auf jedem sekundären Server innerhalb der Serverfarm repliziert.

Dies zeigt, wie diese Serverrollen auf den Staging und Produktion Umgebungen von Fabrikam, Inc. beziehen:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

In diesem Szenario werden die Staging- und produktionsumgebung sowohl als WFF-Serverfarmen konfiguriert. Ein einzelner WFF-Controller-Server verwaltet sowohl Farmen. In einer Serverfarm werden alle Änderungen an den primären Server auf jedem sekundären Server repliziert.

Bevor Sie beginnen, Ihre Umgebungen Staging und Produktion konfigurieren, wird empfohlen, diesen Artikel, um sich mit den Schlüsselkonzepten von WFF 2.0 vertraut machen zu lesen:

- [Übersicht über das Webfarmframework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Einrichten einer Serverfarm mit Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805127)
- [System- und für die Plattform für das Webfarmframework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Übersicht über den Task

Um die Aufgaben und exemplarische Vorgehensweisen in diesem Thema abzuschließen, benötigen Sie mindestens drei Server&#x2014;einen WFF-Controller, einem primären Server für die Serverfarm und einen oder mehrere sekundäre Webserver für die Serverfarm. Sie können weitere sekundäre Server mit einer Serverfarm WFF zu einem beliebigen Zeitpunkt hinzufügen. Auf hoher Ebene, zum Erstellen und Konfigurieren einer WFF-Serverfarm für die Staging- oder Produktionsserver Umgebung, der Sie benötigen:

- Erstellen Sie einen Controllerserver Internet Information Services (IIS) 7.5 und WFF 2.0 installieren.
- Bereiten Sie primäre und sekundäre Server durch eine allgemeine Administratorkonto erstellen und Konfigurieren von Firewallausnahmen vor.
- Konfigurieren Sie die Serverfarm mit IIS-Manager auf dem Controllerserver.
- Konfigurieren Sie den Lastenausgleich mit IIS Application Request Routing (ARR) oder eine alternative Technologie für den Lastenausgleich.

Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie mit "sauberen" Server-Builds mit Windows Server 2008 R2 starten. Bevor Sie beginnen, können Sie für jeden Server sicher, dass:

- Windows Server 2008 R2 Service Pack 1 und alle verfügbaren Updates werden installiert.
- Der Server ist die Domäne.
- Der Server hat eine statische IP-Adresse.

> [!NOTE]
> Weitere Informationen zum Hinzufügen von Computern zu einer Domäne, finden Sie unter [Verknüpfen von Computern zur Domäne und Anmelden](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Weitere Informationen zum Konfigurieren von statischen IP-Adressen finden Sie unter [eine statische IP-Adresse konfigurieren](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Erstellen des WFF-Controller-Servers

Um einen WFF-Controller-Server erstellen, müssen Sie IIS 7 oder höher und WFF 2.0 oder höher installieren. Im Hintergrund WFF verwendet das IIS-Webbereitstellungstool (Web Deploy) 2.x auf die Server in der Farm synchronisiert. Wenn Sie den Webplattform-Installer installieren WFF verwenden, das Installationsprogramm automatisch herunterladen und installieren Sie Web Deploy für Sie.

**So erstellen den WFF-Controller-server**

1. Herunterladen und Installieren der [Webplattform-Installer](https://go.microsoft.com/?linkid=9739157).
2. Am oberen Rand der **Web Platform Installer 3.0** Fenster, klicken Sie auf **Produkte**.
3. Klicken Sie auf der linken Seite des Fensters klicken Sie im Navigationsbereich auf **Server**.
4. In der **empfohlene Konfiguration von IIS 7** auf **hinzufügen**.
5. In der <strong>Web Farm Framework 2.</strong> <em>x</em> auf <strong>hinzufügen</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Klicken Sie auf **Installieren**. Beachten Sie, dass der Webplattform-Installer das Webbereitstellungstool zusammen mit verschiedenen Abhängigkeiten der Installationsliste hinzugefügt wurde.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Überprüfen Sie die Lizenzbedingungen, und wenn Sie den Bedingungen zustimmen, klicken Sie auf **ich stimme**.
8. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**, und schließen Sie dann die **Web Platform Installer 3.0** Fenster.

## <a name="configure-the-primary-and-secondary-servers"></a>Konfigurieren Sie die primären und sekundären Server

Vor dem Erstellen einer Serverfarm WFF sollten Sie einige Vorbereitungsaufgaben zur auf den Webservern ausführen, die der Farm bilden:

- Hinzufügen von Firewallausnahmen ermöglichen die **Kernnetzwerk**, **Remoteverwaltung**, und **Datei- und Druckerfreigabe** Funktionen für die Kommunikation mit den WFF-Controller-Server .
- Erstellen Sie ein Domänenkonto (z. B. **FABRIKAM\stagingfarm**) in Active Directory und der lokalen Administratorgruppe auf jedem Server hinzuzufügen. Wenn Sie die Serverfarm erstellen, müssen Sie dieses Konto als das Administratorkonto des Server-Farm verwenden.

Weitere Informationen zum Konfigurieren von dieser Firewallausnahmen in der Windows-Firewall finden Sie unter [System- und für die Plattform für das Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805128). Andere Firewallsysteme finden Sie in der Produktdokumentation.

Das nächste Verfahren können Sie ein Domänenkonto zur lokalen Administratorengruppe in Windows Server 2008 R2 hinzufügen. Führen Sie dieses Verfahren auf jedem Server, den Sie mit der Serverfarm hinzufügen möchten&#x2014;also dasselbe Domänenkonto der lokalen Administratorengruppe auf dem primären Server und in jeder sekundären Serverinstanz hinzufügen.

**Ein Domänenkonto zur lokalen Administratorgruppe hinzufügen**

1. Auf der **starten** Sie im Menü **Verwaltung**, und klicken Sie dann auf **Server-Manager**.
2. In der **Server-Manager** Fenster, in der Strukturansicht anzuzeigen, erweitern Sie **Konfiguration**, erweitern Sie **lokale Benutzer und Gruppen**, und klicken Sie dann auf **Gruppen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. In der **Gruppen** Bereich doppelklicken Sie auf **Administratoren**.
4. In der **Eigenschaften von Administratoren** (Dialogfeld), klicken Sie auf **hinzufügen**.
5. In der **Auswahl von Benutzern, Computern, Dienstkonten oder Gruppen** (Dialogfeld), (oder suchen) auf Ihrem Domänenkonto anzumelden (z. B. **FABRIKAM\stagingfarm**), und klicken Sie dann auf **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. In der **Eigenschaften von Administratoren** (Dialogfeld), klicken Sie auf **OK**.

Die Server können jetzt mit einer Serverfarm hinzugefügt werden. Im Fall dem primären Server und konfigurieren Sie den Server aus, um den Anforderungen Ihrer Anwendung zu erfüllen, vor oder nach dem Erstellen der Serverfarm&#x2014;in beiden Fällen wird der WFF Server synchronisieren, indem Sie die gleichen Produkte, Komponenten oder eine Konfiguration bereitstellen an den sekundären Servern. Der Einfachheit halber wird in diesem Lernprogramm davon ausgegangen, dass Sie den primären Server konfigurieren müssen, wenn Sie die Serverfarm erstellt haben.

## <a name="create-the-wff-server-farm"></a>Erstellen Sie die WFF-Serverfarm

An diesem Punkt sind alle Server mit einer Serverfarm WFF hinzugefügt werden:

- Sie haben WFF auf den Controller-Server installiert.
- Sie haben die Firewallausnahmen auf primären und sekundären Webserver konfiguriert.
- Sie haben ein Domänenkonto zur lokalen Administratorgruppe auf primären und sekundären Webserver hinzugefügt.

Der nächste Schritt besteht im WFF die Serverfarm zu erstellen. Von IIS-Manager dazu auf dem Controller-Server WFF.

**So erstellen eine Serverfarm WFF**

1. Auf WFF Controller-Server auf die **starten** Sie im Menü **Verwaltung**, und klicken Sie dann auf **(Internet Information Services, IIS) Manager**.
2. In der **Verbindungen** Bereich, erweitern Sie den Knoten für den lokalen Server, mit der rechten Maustaste **Serverfarmen**, und klicken Sie dann auf **Serverfarm erstellen**.
3. In der **Serverfarm erstellen** Dialogfeld Geben Sie einen aussagekräftigen Namen für die Serverfarm (z. B. **Staging Farm**), und wählen Sie dann **Serverfarm für die Bereitstellung**.
4. Geben Sie den Benutzernamen und das Kennwort des Domänenkontos an, die Sie der lokalen Administratorengruppe auf jedem Server hinzugefügt.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Klicken Sie auf **Weiter**.
6. Auf der **Hinzufügen von Servern** Seite Geben Sie den vollqualifizierten Domänennamen (FQDN) des primären Servers wählen **Primärserver**, und klicken Sie dann auf **hinzufügen**.
7. An diesem Punkt versucht WFF wenden Sie sich an den primären Server mit den Anmeldeinformationen, die Sie bereitgestellt. Wenn die Verbindung erfolgreich ist, wird der primäre Server in der Tabelle hinzugefügt werden, auf die **Hinzufügen von Servern** Seite.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Möglicherweise haben Sie bemerkt, die **Server steht für den Lastenausgleich** ist standardmäßig aktiviert. WFF verwendet IIS ARR-Modul, um den Lastenausgleich implementieren und dadurch die Webserver in Ihrer Serverfarm verteilen Anforderungen. Sie würden nur deaktivieren, in den meisten Szenarien die **Server steht für den Netzwerklastenausgleich** option verwenden, wenn Sie eine Drittanbieter-lastenausgleichslösung stattdessen verwenden möchten.
8. Auf der **Hinzufügen von Servern** Seite Geben Sie den FQDN des sekundären Server, und klicken Sie dann auf **hinzufügen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Wiederholen Sie Schritt 7 für weitere sekundäre Server in der Farm aus, und klicken Sie dann auf **Fertig stellen**.

Die Serverfarm WFF ist jetzt einsatzbereit. Alle Web Plattformprodukte oder Komponenten, die Sie installieren, auf dem primären Server und alle Webanwendungen oder Inhalte, die Sie mit dem primären Server werden automatisch auf allen sekundären Servern bereitgestellt werden.

WFF ist eine umfassende und komplexes Thema, und Sie können weitere Informationen finden sie auf der [Microsoft Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805129) Website. Wird, sind gibt es Bereiche jedoch zwei Funktionen, denen Sie berücksichtigen müssen:

- *Anwendungsbereitstellung* ist der Prozess, der Inhalt vom primären Server, z. B. Webanwendungen und Konfigurationseinstellungen auf allen sekundären Servern in der Serverfarm repliziert. Z. B., wenn Sie die Projektmappe Contact Manager auf Ihrem primären Stagingserver bereitstellen, wird im Rahmen des Bereitstellungsprozesses für WFF Anwendung diese Lösung an den sekundären staging-Servern bereitstellen. Standardmäßig wird im Rahmen des Bereitstellungsprozesses Anwendung alle 30 Sekunden ausgeführt.
- *Die plattformbereitstellung* ist der Prozess, der Web-Plattform-Produkte und Komponenten auf dem primären Server mit allen sekundären Servern in der Serverfarm synchronisiert wird. Beispielsweise bei der Installation von ASP.NET MVC 3 auf dem primären Server für die Stagingtabelle wird im Rahmen des Bereitstellungsprozesses Plattform des Webplattform-Installers verwenden so installieren Sie ASP.NET MVC 3 auf allen sekundären Stagingserver. Standardmäßig wird im Rahmen des Bereitstellungsprozesses Plattform alle fünf Minuten ausgeführt.

Sie können auf Ihrem WFF-Controller-Server auf über IIS-Manager-basisanwendung und plattformeinstellungen Bereitstellung verwalten.

**Untersuchen der Anwendung und die Plattform bereitstellungseinstellungen**

1. Im IIS-Manager in der **Verbindungen** Bereich Ihrer Serverfarm auswählen.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. In der **Serverfarm** Bereich doppelklicken Sie auf **Anwendungsbereitstellung**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Wie Sie sehen können, ist die Serverfarm derzeit webeinstellungen Inhalt und Konfiguration zwischen dem primären Server und dem sekundären Server alle 30 Sekunden Synchronisierung konfiguriert.
4. Klicken Sie auf **wieder**, und doppelklicken Sie dann auf **Plattformbereitstellung**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Wie Sie sehen können, die Serverfarm konfiguriert, dass Web Platform Produkte und Komponenten zwischen dem primären Server und dem sekundären Server alle fünf Minuten synchronisiert.
6. Klicken Sie auf **wieder**.
7. So erzwingen die Serverfarm mit Web Plattformprodukte sofort synchronisiert in der **Aktionen** Bereich, klicken Sie auf **Plattform für die Bereitstellung**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Die plattformbereitstellung kann einige Zeit dauern. Der Prozess des Installers, die auf den sekundären Servern in Ihrer Serverfarm im Hintergrund ausgeführt werden.
8. Nachdem Sie ausreichend Zeit für die im Rahmen des Bereitstellungsprozesses abgeschlossen eingeräumt haben, können Sie überprüfen, dass die Produkte und Komponenten, die Sie mit dem primären Server hinzugefügt haben, jetzt auf den sekundären Servern repliziert wurden. Sie können z. B. Melden Sie sich bei einem sekundären Server und verwenden Sie die **Server-Manager** Fenster, um sicherzustellen, dass die Webserverrolle installiert wurde.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Sie können auch überprüfen, dass die Liste der installierten Programme um sicherzustellen, dass verschiedene webplattformkomponenten hinzugefügt wurden.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Konfigurieren des Lastenausgleichs

Bei der Erstellung einer Webfarm müssen Sie eine Form des Lastenausgleichs, die zum Verteilen von HTTP-Anforderungen zwischen dem Webserver einrichten. Dabei kann es sich um Windows Server 2008 Netzwerklastenausgleich, IIS ARR, oder eine Drittanbieter-Software oder Hardware-basierten lastenausgleichslösung handeln.

Bereitzustellendes WFF dient für die enge Integration mit IIS ARR. Um diese Integration nutzen zu können, müssen Sie die ARR-Modul auf dem WFF-Controller-Server zu installieren. Sie leiten klicken Sie dann alle Webdatenverkehr an den Controllerserver in der Regel durch die Datensätze des Domain Name System (DNS) konfigurieren. Der Controller-Server wird dann eingehende Anforderungen zwischen den Servern in Ihrer Farm basierend auf serververfügbarkeit und verschiedene andere Kriterien verteilen.

> [!NOTE]
> Sie müssen mit WFF ARR verwenden; Sie können WFF zur Bearbeitung von Drittanbieter-Lösungen für den Netzwerklastenausgleich konfigurieren. Weitere Informationen finden Sie unter [Überblick über das Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805126).


Lastenausgleich mit ARR ist ein komplexes Thema, das der Gegenstand dieses Lernprogramm ist. Allerdings können Sie das nächste Verfahren verwenden, installieren Sie ARR-Modul und erste Schritte mit Lastenausgleich.

**So richten Sie den Lastenausgleich auf den WFF-Controller-Server ein**

1. Starten Sie den Webplattform-Installer auf WFF-Controller-Server.
2. Am oberen Rand der **Web Platform Installer 3.0** Fenster, klicken Sie auf **Produkte**.
3. Klicken Sie auf der linken Seite des Fensters klicken Sie im Navigationsbereich auf **Server**.
4. In der **Application Request Routing 2.5** auf **hinzufügen**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Klicken Sie auf **installieren**, und folgen Sie den Anweisungen in der **Webplattforminstallation** Fenster.
6. Wenn die Installation abgeschlossen ist, starten Sie IIS-Manager, und klicken Sie in der **Verbindungen** Bereich, klicken Sie auf die Server-farmknoten. Beachten Sie, die mehrere neue Symbole hinzugefügt wurden die **Serverfarm** Bereich.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. In der **Serverfarm** Bereich doppelklicken Sie auf **Lastenausgleich**.
8. In der **Lastenausgleich** Bereich, wählen Sie die Last ausgeglichen Algorithmus (z. B. **mindestens die aktuelle Anforderung**).

    > [!NOTE]
    > Weitere Informationen zum Lastenausgleich bei Algorithmen und anderen Konfigurationseinstellungen finden Sie unter [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. In der **Aktionen** Bereich, klicken Sie auf **übernehmen**.

Sie haben jetzt die grundlegenden Lastenausgleich für die Server in Ihrer Serverfarm konfiguriert. Wenn Sie alle Ihre Farm Webdatenverkehr auf den Controller-Server weiterleiten, werden die Anforderungen zwischen den Servern in Ihrer Farm gemäß der Verfügbarkeit und der Lastenausgleichsalgorithmus gewählten verteilt.

Weitere Informationen zum Konfigurieren des Lastenausgleichs mit ARR finden Sie unter [Application Request Routing Module](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Überwachen Sie die Serverfarm

Sie können die Integrität Ihrer Serverfarm jederzeit über den IIS-Manager auf dem Controllerserver überwachen. In der **Verbindungen** Bereich, erweitern Sie Ihrer Serverfarm, und klicken Sie dann auf **Server**. Mittleren Bereich zeigt eine Zusammenfassung der einzelnen Server in der Farm zusammen mit einem Ablaufverfolgungsprotokoll der aktuellen Aktivität.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Schlussbemerkung

Die Serverfarm WFF sollte jetzt einsatzbereit sein. Sie können den primären Server unabhängig davon, welche Bereitstellungsansatz unterstützen gewünscht konfigurieren&#x2014;finden Sie weitere Themen im Abschnitt&#x2014;und Ihre Konfiguration wird in jeder sekundären Serverinstanz in der Farm repliziert werden.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu allen Aspekten der Konfiguration und Verwendung der WFF zu erhalten, finden Sie unter der [Microsoft Web Farm Framework 2.0 für IIS 7](https://go.microsoft.com/?linkid=9805129) Website.

> [!div class="step-by-step"]
> [Zurück](configuring-a-database-server-for-web-deploy-publishing.md)
> [Weiter](configuring-deployment-properties-for-a-target-environment.md)
