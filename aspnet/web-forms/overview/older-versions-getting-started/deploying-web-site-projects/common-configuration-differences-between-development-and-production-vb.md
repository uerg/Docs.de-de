---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: Allgemeine Konfigurationsunterschiede zwischen Entwicklungs- und Produktionsumgebungen (VB) | Microsoft Docs
author: rick-anderson
description: "In früheren Lernprogrammen haben wir unsere Website durch Kopieren alle relevanten Dateien aus der Entwicklungsumgebung in der produktionsumgebung bereitgestellt. Aber ich..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: 8de1acada8713abf5f92c1f13fa82a5d4ccc18be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="common-configuration-differences-between-development-and-production-vb"></a>Allgemeine Konfigurationsunterschiede zwischen Entwicklungs- und Produktionsumgebungen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> In früheren Lernprogrammen haben wir unsere Website durch Kopieren alle relevanten Dateien aus der Entwicklungsumgebung in der produktionsumgebung bereitgestellt. Allerdings ist es nicht ungewöhnlich, dass dort Konfigurationsunterschiede zwischen Umgebungen, die aktualisiert werden, dass jede Umgebung ist eine eindeutige Datei "Web.config"-Datei sein. Dieses Lernprogramm Standardkonfiguration Unterschiede untersucht und Strategien für die Verwaltung von separaten Konfigurationsinformationen untersucht.


## <a name="introduction"></a>Einführung


Die letzten beiden Lernprogramme durch Bereitstellen einer einfachen Webanwendung mit Firmenbranding geleitet. Die [ *Bereitstellung Ihrer Website mithilfe eines FTP-Clients* ](deploying-your-site-using-an-ftp-client-vb.md) Lernprogramm wurde gezeigt, wie einen eigenständige FTP-Client verwenden, um die erforderlichen Dateien in der Entwicklungsumgebung bis zur Produktion zu kopieren. Dem vorherigen Lernprogramm [ *Bereitstellen Ihrer Website mit Visual Studio*](deploying-your-site-using-visual-studio-vb.md), suchenden bei der Bereitstellung mit Visual Studio Tools zum Kopieren von Websites und Option "Veröffentlichen". In beiden Lernprogramme wurde jeder Datei in der produktionsumgebung eine Kopie einer Datei in der Entwicklungsumgebung. Allerdings ist es nicht ungewöhnlich, dass Konfigurationsdateien in der produktionsumgebung zu unterscheiden sich von denjenigen in der Entwicklungsumgebung. Eine Webanwendung Konfiguration wird gespeichert, der `Web.config` Datei und enthält in der Regel Informationen zu externen Ressourcen, z. B. die Datenbank, Web- und e-Mail-Servern. Es werden auch das Verhalten der Anwendung in bestimmten Situationen, z. B. welche Aktion soll, wenn eine nicht behandelte Ausnahme auftritt.

Beim Bereitstellen einer Webanwendung ist es wichtig, dass die entsprechenden Konfigurationsinformationen in der produktionsumgebung annehmen. In den meisten Fällen die `Web.config` Datei in der Entwicklungsumgebung kann nicht kopiert werden, als in der produktionsumgebung-ist. Stattdessen eine angepasste Version von `Web.config` bis hin zur Produktion hochgeladen werden muss. In diesem Lernprogramm überprüft kurz einige der häufigeren Konfigurationsunterschiede; Es werden auch einige Techniken für die Aufrechterhaltung von anderen Konfigurationsinformationen zwischen diesen Umgebungen zusammengefasst.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Typische Konfigurationsunterschiede zwischen der Entwicklungs- und Produktionsumgebungen

Die `Web.config` -Datei enthält eine Sammlung von Konfigurationsinformationen für eine ASP.NET-Anwendung. Einige dieser Konfigurationsinformationen ist unabhängig von der Umgebung. Für die Instanz, die Authentifizierungseinstellungen und URL-Autorisierungsregeln wie folgt buchstabiert der `Web.config` Datei `<authentication>` und `<authorization>` Elemente sind in der Regel unabhängig von der Umgebung. Andere Konfigurationsinformationen – z. B. Informationen zu externen Ressourcen –, die in der Regel je nach Umgebung unterscheidet sich jedoch.

Datenbank-Verbindungszeichenfolgen sind ein gutes Beispiel von Konfigurationsinformationen, die unterscheidet sich in der Umgebung-basierten. Wenn eine Webanwendung mit kommuniziert einen Datenbankserver müssen sie zuerst eine Verbindung herstellen und, erfolgt über eine [Verbindungszeichenfolge](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Es ist, zwar möglich, hartcodieren der Datenbank-Verbindungszeichenfolge direkt in den Webseiten oder den Code, der mit der Datenbank verbunden es wird empfohlen, sie platzieren `Web.config`des [ `<connectionStrings>` Element](https://msdn.microsoft.com/library/bf7sd233.aspx) so, dass die Verbindungszeichenfolge Informationen finden Sie in einem zentralen Speicherort. Häufig wird eine andere Datenbank während der Entwicklung verwendet werden, als in der Produktion verwendet wird. Folglich muss die Verbindungszeichenfolgeninformationen für jede Umgebung eindeutig sein.

> [!NOTE]
> Zukünftige Lernprogramme untersuchen Bereitstellen von datengesteuerten Anwendungen an diesem, die Punkt wir uns beschäftigen müssen zu den Besonderheiten wie Datenbank-Verbindungszeichenfolgen in der Konfigurationsdatei gespeichert werden.


Das beabsichtigte Verhalten von Entwicklungs- und produktionsumgebungen Umgebungen unterscheidet erheblich. Eine Webanwendung in der Entwicklungsumgebung ist, getestet und debuggt durch eine kleine Gruppe von Entwicklern erstellt wird. In der produktionsumgebung, dass dieselbe Anwendung von vielen anderen gleichzeitigen Benutzern besucht wird. ASP.NET umfasst eine Reihe von Funktionen, mit denen Entwickler beim Testen und Debuggen einer Anwendung, aber diese Funktionen müssen für Leistung und Sicherheit in der produktionsumgebung deaktiviert werden. Sehen wir uns einige-Konfigurationseinstellungen an.

### <a name="configuration-settings-that-impact-performance"></a>Konfigurationseinstellungen, die Leistung auswirken

Bei eine ASP.NET-Seite besucht wird für erstmalig (oder der ersten Mal nach der Änderung wurde), muss seine deklarative Markup in einer Klasse konvertiert werden, und diese Klasse kompiliert werden muss. Wenn die Webanwendung automatische Kompilierung verwendet, muss der Seite Code-Behind-Klasse zu kompiliert werden. Sie können konfigurieren, dass eine Sammlung von Kompilierungsoptionen über die `Web.config` Datei [ `<compilation>` Element](https://msdn.microsoft.com/library/s10awwz0.aspx).

Das Debugattribut ist eines der wichtigsten Attribute in der `<compilation>` Element. Wenn die `debug` -Attribut auf "true", und klicken Sie dann den kompilierten Assemblys Debugsymbole, die erforderlich sind enthalten, wenn eine Anwendung in Visual Studio Debuggen festgelegt ist. Aber Debugsymbole vergrößern die Assembly und erzwingen Sie zusätzlichen arbeitsspeicheranforderungen aus, wenn der Code ausgeführt wird. Darüber hinaus, wenn die `debug` -Attribut auf "alle Inhalte, die zurückgegeben werden, indem Sie"Wahr"festgelegt ist `WebResource.axd` nicht zwischengespeichert werden, was bedeutet, dass jedes Mal ein Benutzer eine Seite besucht, sie die statische Inhalte zurückgegebenes erneut herunterladen müssen `WebResource.axd`.

> [!NOTE]
> `WebResource.axd`eine integrierte HTTP-Handler ist in ASP.NET 2.0 eingeführt, mit denen Serversteuerelemente eingebettete Ressourcen, z. B. Skriptdateien, Bilder, CSS-Dateien und andere Inhalte abrufen. Weitere Informationen zum `WebResource.axd` funktioniert und wie Sie Zugriff auf eingebettete Ressourcen von Ihrer benutzerdefinierten Serversteuerelemente verwenden finden Sie unter [Zugriff auf eingebettete Ressourcen über eine URL verwendet `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


Die `<compilation>` des Elements `debug` Attribut wird festgelegt, dass Sie "true" in der Entwicklungsumgebung. Tatsächlich, dass auf dieses Attribut muss auf "true", um eine Webanwendung Debuggen festgelegt werden kann Wenn Sie versuchen, eine ASP.NET-Anwendung in Visual Studio zu debuggen und die `debug` -Attribut auf "False" festgelegt ist, Visual Studio zeigt eine Meldung erläutert wird, dass die Anwendung kann, bis gedebuggt werden die `debug` -Attribut auf "true" und wird festgelegt ist Angebot, für Sie diese Änderung vorzunehmen.

Sie sollten **nie** haben die `debug` -Attribut festgelegt auf "True" in einer produktionsumgebung aufgrund dessen Auswirkungen auf Leistung. Eine ausführlichere Erläuterung zu diesem Thema, finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogbeitrag [Don't ausführen Produktion ASP.NET Applications mit `debug="true"` aktiviert](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Benutzerdefinierte Fehler und Protokollierung

Tritt eine nicht behandelte Ausnahme in einer ASP.NET-Anwendung ausgelöst es bis zu die Common Language Runtime an diesem, die Punkt eines von drei Dingen geschieht:

- Eine generische Common Language Runtime-Fehlermeldung wird angezeigt. Auf dieser Seite wird der Benutzer, der es wurde ein Laufzeitfehler, enthält jedoch keine Informationen über den Fehler informiert.
- Meldung Details zu einer Ausnahme wird angezeigt, die Informationen auf der gerade ausgelöste Ausnahme enthält.
- Eine benutzerdefinierte Fehlerseite wird angezeigt, also in einer ASP.NET-Seite, die Sie erstellen, in dem alle Nachrichten angezeigt, die Sie wünschen.

Was geschieht, bei dem eine nicht behandelte Ausnahme richtet sich nach der `Web.config` Datei [ `<customErrors>` Abschnitt](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Beim Entwickeln und Testen einer Anwendung hilft, jede Ausnahme im Browser angezeigt. Mit Ausnahmedetails in einer Anwendung in Produktion ist jedoch ein potenzielles Sicherheitsrisiko dar. Darüber hinaus ist unflattering und macht Ihre Website unprofessionell. Im Falle einer nicht behandelten Ausnahme wird eine Webanwendung in der Entwicklungsumgebung idealerweise die Ausnahme Details anzeigen, während die gleiche Anwendung in Produktion eine benutzerdefinierte Fehlerseite angezeigt werden.

> [!NOTE]
> Die Standardeinstellung `<customErrors>` Einstellung zeigt die Details der Ausnahme Nachricht nur, wenn die Seite "durch" localhost "aufgerufen wird und die Laufzeit generische Fehlerseite andernfalls zeigt. Dies ist nicht ideal, aber es um zu wissen, dass das Standardverhalten Ausnahmedetails nicht lokaler Besucher Offenlegen nicht gewährleistet. Ein Lernprogramm zukünftige untersucht die `<customErrors>` Abschnitt ausführlicher und gezeigt, wie Sie eine benutzerdefinierte Fehlerseite angezeigt, wenn ein Fehler in der Produktion auftritt.


Ist eine weitere ASP.NET-Funktion, die während der Entwicklung nützlich ist die Ablaufverfolgung. Ablaufverfolgung, wenn aktiviert, zeichnet Informationen über jede eingehende Anforderung und bietet eine spezielle Webseite `Trace.axd`, für die Anzeige von Details der letzten Anforderung. Sie können aktivieren und Konfigurieren der Ablaufverfolgung über die [ `<trace>` Element](https://msdn.microsoft.com/library/6915t83k.aspx) in `Web.config`.

Wenn Sie Ablaufverfolgung stellen Sie sicher, dass, die sie in der produktionsumgebung deaktiviert ist aktivieren. Da die Ablaufverfolgungsinformationen Cookies, Sitzungsdaten und andere potenziell vertrauliche Informationen enthält, ist es wichtig, zum Deaktivieren der Ablaufverfolgung in der Produktion. Die gute Nachricht ist, dass standardmäßig die Ablaufverfolgung deaktiviert ist und die `Trace.axd` Datei kann nur durch "localhost" zugegriffen werden. Wenn Sie diese Standardeinstellungen bei der Entwicklung ändern, stellen Sie sicher, dass sie wieder in der produktionsumgebung deaktiviert werden.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Techniken zum Verwalten von separaten Konfigurationsinformationen

Mit anderen Konfigurationseinstellungen in die Entwicklungs- und produktionsumgebungen wird den Bereitstellungsprozess komplizierter. In den vorherigen zwei Lernprogrammen Beteiligten des Bereitstellungsprozesses kopieren alle erforderlichen Dateien von der Entwicklung bis hin zur Produktion, aber dieser Ansatz nur, funktioniert Wenn die Konfigurationsinformationen in beiden Umgebungen identisch ist. Es gibt eine Vielzahl von Verfahren zum Bereitstellen einer Anwendung mit unterschiedlichen Konfigurationsinformationen. Lassen Sie uns einige dieser Optionen für gehosteten Webanwendungen des Katalogs.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Manuelles Bereitstellen der Konfigurationsdatei der Produktions-Umgebung

Die einfachste Vorgehensweise ist zum Verwalten von zwei Versionen der `Web.config` Datei: eine für die Entwicklungsumgebung und eine für die produktionsumgebung. Bereitstellen von einem Standort bis hin zur Produktion umfasst das Kopieren aller Dateien auf dem Produktionsserver in der Entwicklungsumgebung *außer* für die `Web.config` Datei. Stattdessen die Produktion umgebungsspezifische `Web.config` Datei kopiert werden, bis hin zur Produktion.

Dieser Ansatz ist nicht sehr anspruchsvolle, aber es ist einfach zu implementieren, da die Konfigurationsinformationen sich selten ändern. Dies funktioniert am besten bei Anwendungen mit einem kleinen Entwicklungsteam, die auf einem einzelnen Webserver gehostet werden und deren Konfigurationsinformationen ist nur selten geändert. Es ist am häufigsten Hauptfeuerlöschpumpe, wenn Sie die Dateien der Anwendung mithilfe eines eigenständigen FTP-Clients manuell bereitstellen. Bei Verwendung von Tools zum Kopieren von Websites oder die Option "Veröffentlichen" Visual Studio müssen Sie zuerst auszutauschen, bevor die bereitstellungsspezifischen `Web.config` vor der Bereitstellung mit dem Produktions-spezifische Datei, und tauschen Sie sie nach der Bereitstellung abgeschlossen ist.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Ändern Sie die Konfiguration während der Erstellung oder der Bereitstellungsprozess

Die Diskussionen haben bisher davon ausgegangen, dass einen Ad-hoc-Build- und Bereitstellungsprozess-Prozess. Viele größere Softwareprojekten haben mehr Prozesse formalisiert, die Verwendung der Open-Source-adressenverwaltung, oder Drittanbietertools zu machen. Für solche Projekte können wahrscheinlich anpassen den erstellungs- oder bereitstellungs-Prozess, um die Konfigurationsinformationen entsprechend zu ändern, bevor es in die Produktion übertragen wird. Wenn die Anwendung mit der Erstellung [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), oder ein anderes Tool erstellen, fügen Sie wahrscheinlich einen Buildschritt so ändern Sie die `Web.config` Datei um die Produktions-spezifische Einstellungen zu übernehmen. Der Bereitstellungsworkflow könnte programmgesteuert Herstellen einer Verbindung mit dem Quellcode-Verwaltungsserver und Abrufen der entsprechenden `Web.config` Datei.

Die aktuelle Methode zum Abrufen von des entsprechenden Konfigurationsinformationen zu bis hin zur Produktion hängt häufig die Tools und Workflows. Daher wird es nicht in diesem Thema ausführlicher behandelt. Bei Verwendung ein beliebten Build-Tools wie MSBuild oder NAnt können Sie Artikel zur Bereitstellung und Lernprogramme auf diese Tools über eine Internetsuche nach bestimmten finden.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Verwalten von Konfigurationsunterschiede über die zur Bereitstellung von Add-In-Projekt

Im Jahr 2006 hat Microsoft die Web Development Project-Add-In für Visual Studio 2005 veröffentlicht. Ein Add-In für Visual Studio 2008 wurde im 2008 veröffentlicht. Dieses Add-Ins kann für ASP.NET-Entwickler, eine separate Webbereitstellungsprojekt zusammen mit ihren Webanwendungsprojekt erstellen, die bei der Erstellung, explizit kompiliert die Webanwendung und kopiert die Dateien in einen lokalen Ausgabeverzeichnis bereitstellen. Das Webanwendungsprojekt verwendet MSBuild im Hintergrund.

Wird standardmäßig der Entwicklungsumgebung des `Web.config` Datei in das Ausgabeverzeichnis kopiert, aber Sie können einrichten, dass das Webprojekt für die Bereitstellung zum Anpassen der

die Konfigurationsinformationen, die in dieses Verzeichnis Ruft ab, auf folgende Weise kopiert:

- Über `Web.config` Datei Abschnitt Ersatz, in dem Sie im Abschnitt zum Ersetzen angeben und eine XML-Datei, die den Ersetzungstext enthält.
- Durch die Bereitstellung eines Pfads zu einer externen Quelle-Konfigurationsdatei an. Diese Option ausgewählt ist, kopiert das Webprojekt für die Bereitstellung eine bestimmten `Web.config` Datei in das Ausgabeverzeichnis (statt über das `Web.config` Datei, die in der Entwicklungsumgebung verwendet).
- Durch Hinzufügen von benutzerdefinierten Regeln für die MSBuild-Datei, die durch das Webprojekt für die Bereitstellung verwendet.

In der Web deploy Anwendung das Webprojekt für die Bereitstellung zu erstellen, und kopieren Sie die Dateien aus Ausgabeordner des Projekts in der produktionsumgebung.

Erfahren Sie mehr über das Webprojekt für die Bereitstellung Auschecken [in diesem Artikel Webprojekte für die Bereitstellung](https://msdn.microsoft.com/magazine/cc163448.aspx) aus der Ausgabe des April 2007 [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), oder wenden Sie sich an den Links im Abschnitt Weitere Themen am der Ende dieses Lernprogramms.

> [!NOTE]
> Sie können das Webprojekt für die Bereitstellung nicht mit Visual Web Developer verwenden, da das Webprojekt für die Bereitstellung als ein Visual Studio-Add-In implementiert wird und Visual Studio Express-Editionen (einschließlich Visual Web Developer)-Add-Ins nicht unterstützt.


## <a name="summary"></a>Zusammenfassung

Die externen Ressourcen und das Verhalten einer Webanwendung in der Entwicklung unterscheiden sich i. d. r. als wenn die gleiche Anwendung in Produktion ist. Unterscheiden sich z. B. Datenbankverbindungszeichenfolgen, Kompilierungsoptionen und das Verhalten, wenn eine nicht behandelte Ausnahme, häufig auftritt zwischen Umgebungen. Der Bereitstellungsprozess muss diese Unterschiede berücksichtigen. Wie in diesem Lernprogramm erläutert, ist der einfachste Ansatz eine alternative Konfiguration-Datei manuell in die produktionsumgebung kopieren. Elegantere Lösungen sind möglich, wenn das Web Deployment Project-Add-In verwenden, oder bei einem mehr formalisierter erstellungs- oder bereitstellungs-Prozess, der solche Anpassungen aufnehmen kann.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Erläutert Verbindungszeichenfolgen](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [@ ConnectionStrings.com für Datenbank-Verbindungszeichenfolgen](http://www.connectionstrings.com/)
- [Führen Sie nicht ASP.NET Produktionsanwendungen mit `debug="true"` aktiviert](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Ordnungsgemäß reagieren auf nicht behandelte Ausnahmen - benutzerfreundliche Fehlerseiten anzeigen](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Gewusst wie: Verwenden Sie ein Visual Studio 2008 Web Bereitstellungsprojekt?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Wichtige Konfigurationseinstellungen, die bei der Bereitstellung einer Datenbank](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008-Bereitstellung Projekte Webdownload](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 Webdownload Bereitstellung Projekte bereit](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Visual Studio 2008 Web-Bereitstellungsprojekte](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [Bereitstellung Projekt Unterstützung für Visual Studio 2008 Web freigegeben](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Webbereitstellungsprojekte](https://msdn.microsoft.com/magazine/cc163448.aspx)

>[!div class="step-by-step"]
[Zurück](deploying-your-site-using-visual-studio-vb.md)
[Weiter](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
