---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: Allgemeine Konfigurationsunterschiede zwischen Entwicklungs- und Produktionsumgebungen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir unsere Website durch Kopieren aller relevanten Dateien in der Entwicklungsumgebung in der produktionsumgebung bereitgestellt. Aber ich...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: ec1d575fac4527db8f5bcab5f0d84e1f17eac46b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826248"
---
<a name="common-configuration-differences-between-development-and-production-vb"></a>Allgemeine Konfigurationsunterschiede zwischen Entwicklungs- und Produktionsumgebungen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> In den vorherigen Tutorials haben wir unsere Website durch Kopieren aller relevanten Dateien in der Entwicklungsumgebung in der produktionsumgebung bereitgestellt. Allerdings ist es nicht ungewöhnlich, dass es gibt Konfigurationsunterschiede zwischen Umgebungen, das erfordert, dass jede Umgebung über eine eindeutige Datei "Web.config"-Datei verfügen. In diesem Tutorial werden die typische Konfigurationsunterschiede untersucht und untersucht die Strategien für die Verwaltung von separate Konfiguration.


## <a name="introduction"></a>Einführung


Die letzten beiden Tutorials wurde erläutert, wie eine einfache Webanwendung bereitstellen. Die [ *Bereitstellen einer Website mithilfe eines FTP-Clients* ](deploying-your-site-using-an-ftp-client-vb.md) Tutorial wurde gezeigt, wie Sie einen eigenständigen FTP-Client verwenden, um die erforderlichen Dateien aus der Entwicklungsumgebung bis zur Produktion zu kopieren. Das vorherige Tutorial [ *Bereitstellen Ihrer Website mit Visual Studio*](deploying-your-site-using-visual-studio-vb.md), bei der Bereitstellung mithilfe von Visual Studio Tools zum Kopieren von Websites und Option "Veröffentlichen" suchenden. In beiden Lernprogramme war jede Datei in der produktionsumgebung eine Kopie einer Datei in der Entwicklungsumgebung. Allerdings ist es nicht ungewöhnlich, dass Konfigurationsdateien in der produktionsumgebung, in der Entwicklungsumgebung unterscheiden. Eine Webanwendung-Konfiguration befindet sich in der `Web.config` Datei, und in der Regel enthält Informationen zu externen Ressourcen, z. B. Datenbank, Web- und e-Mail-Server. Es beschreibt auch das Verhalten der Anwendung in bestimmten Situationen, z. B. welche Aktion soll, wenn eine nicht behandelte Ausnahme auftritt.

Beim Bereitstellen einer Webanwendung ist es wichtig, dass die entsprechenden Konfigurationsinformationen in der produktionsumgebung erhalten. In den meisten Fällen die `Web.config` Datei in der Entwicklungsumgebung kann nicht kopiert werden, in der produktionsumgebung als-ist. Stattdessen eine benutzerdefinierte Version der `Web.config` muss in die produktionsumgebung hochgeladen werden. In diesem Tutorial werden kurz einige der häufigeren Konfigurationsunterschiede; Es werden auch einige Techniken zum Verwalten von Informationen der anderen Konfiguration zwischen den Umgebungen zusammengefasst.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Typische Konfigurationsunterschiede zwischen der Entwicklungs- und Produktionsumgebungen

Die `Web.config` -Datei enthält eine Sammlung von Konfigurationsinformationen für eine ASP.NET-Anwendung. Einige dieser Konfigurationsinformationen ist unabhängig von der Umgebung identisch. Z. B. die Authentifizierungseinstellungen und die URL-Autorisierungsregeln ausgeschrieben die `Web.config` dateimodell `<authentication>` und `<authorization>` Elemente sind in der Regel unabhängig von der Umgebung. Andere Konfigurationsinformationen – z. B. Informationen zu externen Ressourcen –, die in der Regel abhängig von der Umgebung unterscheidet sich jedoch.

Datenbank-Verbindungszeichenfolgen sind ein gutes Beispiel Konfigurationsinformationen, die unterscheidet sich abhängig von der Umgebung. Wenn eine Webanwendung mit kommuniziert einen Datenbank-Server müssen sie zunächst eine Verbindung herstellen und, erfolgt über eine [Verbindungszeichenfolge](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Es ist, zwar möglich, vordefinierten Code die Verbindungszeichenfolge der Datenbank direkt in den Webseiten oder der Code, der mit der Datenbank verbindet es empfiehlt sich, sie platzieren `Web.config`des [ `<connectionStrings>` Element](https://msdn.microsoft.com/library/bf7sd233.aspx) , damit der Verbindungszeichenfolge Informationen finden Sie in einem zentralen Standort. Wenn wird eine andere Datenbank während der Entwicklung verwendet als in der Produktion verwendet wird. Daher muss die Verbindungszeichenfolgeninformationen für jede Umgebung einzigartig sein.

> [!NOTE]
> Zukünftige Tutorials werden Bereitstellen von datengesteuerten Anwendungen, an welchem, die Punkt wir uns befassen müssen im einzelnen erläutert, wie Datenbank-Verbindungszeichenfolgen in der Konfigurationsdatei gespeichert werden.


Das beabsichtigte Verhalten von Entwicklungs- und produktionsumgebungen Umgebungen unterscheidet sich grundlegend. Eine Webanwendung in der Entwicklungsumgebung wird, getestet und gedebuggt durch eine kleine Gruppe von Entwicklern erstellt. In der produktionsumgebung wird die gleiche Anwendung von vielen anderen gleichzeitigen Benutzern zugegriffen wird. ASP.NET umfasst eine Reihe von Features, mit denen Entwickler beim Testen und Debuggen einer Anwendung, aber diese Funktionen sollten für Leistungs- und Sicherheitsgründen in der produktionsumgebung deaktiviert werden. Wir sehen uns ein Paar diese Konfigurationseinstellungen.

### <a name="configuration-settings-that-impact-performance"></a>Konfigurationseinstellungen, die auf die Leistung auswirken

Bei eine ASP.NET-Seite besucht wird beim ersten (oder der ersten Mal, nachdem es geändert wurde), muss der deklarative Markup in einer Klasse konvertiert werden, und diese Klasse muss kompiliert werden. Wenn die Webanwendung die automatische Kompilierung verwendet dann die CodeBehind-Klasse der Seite, zu kompiliert werden muss. Sie können konfigurieren, dass eine Sammlung von Kompilierungsoptionen über die `Web.config` dateimodell [ `<compilation>` Element](https://msdn.microsoft.com/library/s10awwz0.aspx).

Das Debug-Attribut ist eine der wichtigsten Attribute in der `<compilation>` Element. Wenn die `debug` -Attributsatz auf "true", und klicken Sie dann die kompilierten Assemblys Debugsymbole enthalten, die beim Debuggen einer Anwendung in Visual Studio erforderlich sind. Aber Debugsymbole erhöhen Sie die Größe der Assembly und zusätzliche arbeitsspeicheranforderungen zu erzwingen, wenn der Code ausgeführt. Darüber hinaus, wenn die `debug` -Attribut auf "alle Inhalte, die zurückgegeben werden, indem Sie"true"festgelegt ist `WebResource.axd` nicht zwischengespeichert werden, was bedeutet, dass jedes Mal ein Benutzer eine Seite besucht, sie den statischen Inhalt vom erneut herunterladen müssen `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` eine integrierte HTTP-Handler wird in ASP.NET 2.0 eingeführt, die Steuerelemente zu verwenden, um eingebettete Ressourcen, z. B. Skriptdateien, Bilder, CSS-Dateien und andere Inhalte abzurufen. Weitere Informationen dazu, wie `WebResource.axd` funktioniert und wie Sie es auf eingebettete Ressourcen aus Ihrem benutzerdefinierten Steuerelementen, verwenden können, finden Sie unter [zugreifen auf eingebettete Ressourcen über eine URL verwenden `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


Die `<compilation>` des Elements `debug` -Attributsatz ist in der Regel auf "true" in der Entwicklungsumgebung. In der Tat, dass auf dieses Attribut muss auf "true", um eine Webanwendung; Debuggen festgelegt werden kann Wenn Sie versuchen, eine ASP.NET-Anwendung in Visual Studio debuggen und die `debug` -Attribut auf "False" festgelegt ist, zeigt Visual Studio wird eine Meldung angezeigt, dass die Anwendung kann, bis gedebuggt werden die `debug` -Attribut ist auf "true" und wird festgelegt bieten Sie diese Änderung vornehmen.

Sie sollten **nie** haben die `debug` -Attribut festgelegt ist, auf "True" in einer produktionsumgebung aufgrund der Auswirkungen auf die Leistung. Eine ausführlichere Erläuterung zu diesem Thema, finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)Blogbeitrag [Don't ausführen Produktion ASP.NET Anwendungen mit `debug="true"` aktiviert](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Benutzerdefinierte Fehler und Ablaufverfolgung

In einer ASP.NET-Anwendung tritt eine nicht behandelte Ausnahme ausgelöst es die Laufzeit, die an diesem Punkt eines von drei Dingen geschieht:

- Eine generische Laufzeitfehlermeldung wird angezeigt. Auf dieser Seite wird der Benutzer, der es wurde ein Laufzeitfehler, sondern bietet keine Details über den Fehler informiert.
- Meldung Details zu einer Ausnahme wird angezeigt, die Informationen zu der soeben ausgelöste Ausnahme enthält.
- Eine benutzerdefinierten Fehlerseite wird angezeigt, d.h. eine ASP.NET-Seite, die Sie erstellen, in dem alle Nachrichten angezeigt, die Sie wünschen.

Was geschieht, bei einer nicht behandelten Ausnahme hängt von der `Web.config` dateimodell [ `<customErrors>` Abschnitt](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Beim Entwickeln und Testen einer Anwendung, die ist es hilfreich, jede Ausnahme im Browser angezeigt. Zeigt Details der Ausnahme in einer Anwendung in Produktion ist jedoch ein potenzielles Sicherheitsrisiko dar. Darüber hinaus ist unflattering und macht Ihrem unprofessionell Website. Im Falle einer nicht behandelten Ausnahme werden eine Webanwendung in der Entwicklungsumgebung im Idealfall der Ausnahme-Details angezeigt, während die gleiche Anwendung in der Produktion mit eine benutzerdefinierten Fehlerseite angezeigt werden.

> [!NOTE]
> Der Standardwert `<customErrors>` Abschnitt-Einstellung zeigt die Details der Ausnahme Nachricht nur, wenn die Seite "durch" localhost "aufgerufen wird und der generischen Fehlermeldung Laufzeitseite andernfalls zeigt. Diese Vorgehensweise nicht optimal, aber es erkennt, dass das Standardverhalten Ausnahmedetails für nicht-lokalen Besucher dadurch nicht gewährleistet. Eine zukünftige Lernprogramm wird untersucht, die `<customErrors>` Abschnitt noch ausführlicher und zeigt, wie Sie eine benutzerdefinierten Fehlerseite angezeigt, wenn ein Fehler in der Produktion auftritt.


Ist ein weiteres ASP.NET-Feature, das während der Entwicklung nützlich ist die Ablaufverfolgung. Ablaufverfolgung, wenn aktiviert, zeichnet folgende Informationen für jede eingehende Anforderung und stellt eine spezielle Webseite `Trace.axd`, für die Anzeige von Details zur aktuellen Anforderung. Sie können aktivieren und Konfigurieren der Ablaufverfolgung über die [ `<trace>` Element](https://msdn.microsoft.com/library/6915t83k.aspx) in `Web.config`.

Wenn Sie Ablaufverfolgung stellen Sie sicher, dass, die sie in der produktionsumgebung deaktiviert ist aktivieren. Da die Ablaufverfolgungsinformationen Cookies, Sitzungsdaten und andere potenziell vertrauliche Informationen enthält, ist es wichtig, zum Deaktivieren der Ablaufverfolgung in der Produktion. Die gute Nachricht ist, wird standardmäßig die Ablaufverfolgung deaktiviert ist und die `Trace.axd` Datei kann nur über einen Localhost zugegriffen werden. Wenn Sie diese Standardeinstellungen in der Entwicklung ändern stellen Sie sicher, dass sie wieder in der produktionsumgebung deaktiviert sind.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Techniken für die Verwaltung von Separate Konfiguration

Mit anderen Konfigurationseinstellungen in der Entwicklungs- und produktionsumgebungen erschwert während des Bereitstellungsvorgangs. In den vorherigen beiden Tutorials beteiligt während des Bereitstellungsvorgangs kopieren alle erforderlichen Dateien von der Entwicklung zur Produktion, aber diese Ansatz nur, funktioniert Wenn die Konfigurationsinformationen in beiden Umgebungen übereinstimmt. Es gibt eine Vielzahl von Techniken zur Bereitstellung einer Anwendung mit unterschiedlicher Konfigurationsinformationen. Lassen Sie uns einige dieser Optionen für gehostete Webanwendungen des Katalogs.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Manuelles Bereitstellen der Konfigurationsdatei der Produktions-Umgebung

Der einfachste Ansatz ist, zwei Versionen von verwalten die `Web.config` Datei: eine für die Entwicklungsumgebung und eine für die produktionsumgebung. Bereitstellung eines Standorts für die Produktion umfasst das Kopieren aller Dateien auf dem Produktionsserver in der Entwicklungsumgebung *außer* für die `Web.config` Datei. Stattdessen die Produktion bestimmte `Web.config` Datei für die Produktion kopiert werden.

Dieser Ansatz ist nicht sehr hoch entwickeltes, aber es ist einfach zu implementieren, da die Konfigurationsinformationen sich selten ändern. Dies funktioniert am besten bei Anwendungen mit einem kleinen Entwicklungsteam die auf einem einzelnen Webserver gehostet werden und deren Konfigurationsinformationen ist nur selten geändert werden. Es ist am häufigsten Hauptfeuerlöschpumpe, wenn Sie die Anwendungsdateien mit einem eigenständigen FTP-Client manuell bereitstellen. Wenn Visual Studio Tools zum Kopieren von Websites oder Option "Veröffentlichen" verwenden, müssen Sie zuerst Auslagern der bereitstellungsspezifischen `Web.config` vor der Bereitstellung mit der Produktions-spezifische Datei, und klicken Sie dann wechseln sie zurück nach Abschluss der Bereitstellung.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Ändern Sie die Konfiguration während des Builds oder der Bereitstellungsprozess

Die Diskussionen haben bisher davon ausgegangen, dass einen Ad-hoc-Build & Deployment-Prozess. Viele umfangreicheren Softwareprojekten haben mehr Prozesse formalisiert, die Verwendung von Open-Source-adressenverwaltung, oder Drittanbietertools zu machen. Für diese Projekte können Sie wahrscheinlich den erstellungs- oder bereitstellungs-Vorgang, um die Informationen entsprechend zu ändern, bevor es in die Produktion überführt wird anpassen. Wenn Sie Ihre Webanwendung mit erstellen [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), einem anderen Buildtool können Sie wahrscheinlich einen Buildschritt hinzufügen, ändern, Hinzufügen der `Web.config` hinzu, um die Produktions-spezifischen Einstellungen einzubeziehen. Oder Ihren Bereitstellungsworkflow kann programmgesteuert eine Verbindung mit dem Quellcode-Verwaltungsserver herstellen und Abrufen der entsprechenden `Web.config` Datei.

Der tatsächliche Ansatz zum Abrufen der Konfigurationsinformationen für die Produktion variiert basierend auf Ihren Tools und Workflows. Daher behandeln wir nicht in diesem Thema weiter. Bei Verwendung ein gängigen Build-Tools wie MSBuild oder NAnt finden Artikel zur Bereitstellung und Tutorials für diese Tools über eine Internetsuche Sie.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Verwalten von Konfigurationsunterschiede über die Webbereitstellung-Projekt-Add-In

Im Jahr 2006 hat Microsoft das Project-Add-In Web Development für Visual Studio 2005 veröffentlicht. Ein Add-In für Visual Studio 2008 wurde 2008 veröffentlicht. Dieses Add-In ermöglicht Entwicklern das ASP.NET einen separaten Web-Bereitstellungsprojekt zusammen mit ihrer Web-Application-Projekt erstellen, die bei der Erstellung explizit kompiliert die Webanwendung und kopiert die Dateien in einem lokalen Ausgabeverzeichnis bereitstellen. Das Webanwendungsprojekt verwendet MSBuild, hinter den Kulissen.

In der Standardeinstellung der Entwicklungsumgebung des `Web.config` -Datei in das Ausgabeverzeichnis kopiert wird, aber Sie können einrichten, dass die Web-Bereitstellungsprojekt zum Anpassen der

die Konfigurationsinformationen, die in dieses Verzeichnis Ruft ab, es gibt folgende Möglichkeiten kopiert:

- Über `Web.config` Datei Abschnitt Ersatz, in dem Sie im Abschnitt zum Ersetzen angeben und eine XML-Datei, die den Ersetzungstext enthält.
- Durch die Bereitstellung eines Pfads zu einer externen Quelle-Konfigurationsdatei an. Diese Option ausgewählt ist, kopiert das Webprojekt für die Bereitstellung eine bestimmten `Web.config` Datei in das Ausgabeverzeichnis (anstelle der `Web.config` Datei, die in der Entwicklungsumgebung verwendet).
- Durch das Hinzufügen von benutzerdefinierter Regeln auf die MSBuild-Datei, die von der Web-Bereitstellungsprojekt verwendet.

In der Web deploy Anwendung erstellen die Web-Bereitstellungsprojekt und kopieren Sie die Dateien aus dem Ordner "Output" des Projekts in der produktionsumgebung bereit.

Finden Sie weitere Informationen zur Verwendung der Web-Bereitstellungsprojekt sehen Sie sich [in diesem Artikel für die Web-Bereitstellungsprojekte](https://msdn.microsoft.com/magazine/cc163448.aspx) aus der Ausgabe vom April 2007 des [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), oder wenden Sie sich an den Links im Abschnitt "Weiterführende Literatur" am die Ende dieses Tutorials.

> [!NOTE]
> Sie können nicht der Web-Bereitstellungsprojekt mit Visual Web Developer verwenden, da es sich bei der Web-Bereitstellungsprojekt ist als ein Visual Studio-Add-In implementiert und die Visual Studio Express Edition (einschließlich Visual Web Developer)-Add-Ins nicht unterstützt.


## <a name="summary"></a>Zusammenfassung

Die externen Ressourcen und das Verhalten einer Webanwendung in der Entwicklung unterscheiden sich in der Regel als wenn dieselbe Anwendung in der Produktion ist. Beispielsweise unterscheiden sich Datenbank-Verbindungszeichenfolgen, Kompilierungsoptionen und das Verhalten, wenn eine unbehandelte Ausnahme, häufig auftritt zwischen Umgebungen ein. Während des Bereitstellungsvorgangs muss diese Unterschiede berücksichtigen. Wie in diesem Tutorial erläutert, ist der einfachste Ansatz, eine alternative Konfiguration-Datei manuell in die produktionsumgebung zu kopieren. Elegantere Lösungen sind möglich, wenn Sie das Web Deployment Project-Add-In verwenden, oder mit einem mehr formalisierte erstellungs- oder bereitstellungs-Prozess, die solche Anpassungen aufnehmen kann.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Verbindungszeichenfolgen erläutert](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [@ ConnectionStrings.com für Datenbank-Verbindungszeichenfolgen](http://www.connectionstrings.com/)
- [Führen Sie nicht ASP.NET Produktionsanwendungen mit `debug="true"` aktiviert](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Ordnungsgemäß reagieren auf Unbehandelte Ausnahmen – anzeigen von benutzerfreundlichen Fehlerseiten](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Gewusst wie: Verwenden Sie eine Visual Studio 2008-Webbereitstellungsprojekts?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Wichtige Konfigurationseinstellungen, die beim Bereitstellen einer Datenbank](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008-Bereitstellung Projekte Webdownload](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Download Visual Studio 2005 Web Deployment Projects](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Visual Studio 2008-Web-Bereitstellungsprojekte](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [Visual Studio 2008-Projekt Webbereitstellungssupport veröffentlicht](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Webbereitstellungsprojekte](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Zurück](deploying-your-site-using-visual-studio-vb.md)
> [Weiter](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
