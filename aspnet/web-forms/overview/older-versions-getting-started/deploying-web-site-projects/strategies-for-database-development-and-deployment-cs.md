---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: Strategien zur Datenbankentwicklung und-Bereitstellung (c#) | Microsoft Docs
author: rick-anderson
description: "Beim Bereitstellen einer datengesteuerte Anwendung zum ersten Mal können Sie die Datenbank Blind in der Entwicklungsumgebung in die produktionsumgebung kopieren. B..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 21d63b175eb52838ac9a12e33efc59fded4ed87d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="strategies-for-database-development-and-deployment-c"></a>Strategien zur Datenbankentwicklung und-Bereitstellung (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> Beim Bereitstellen einer datengesteuerte Anwendung zum ersten Mal können Sie die Datenbank Blind in der Entwicklungsumgebung in die produktionsumgebung kopieren. Aber Ausführen einer Blind Kopie in nachfolgenden Bereitstellungen überschreibt keine Daten eingegeben werden, in der Produktionsdatenbank. Stattdessen angewandt Bereitstellen einer Datenbank die Änderungen in der Entwicklungsdatenbank seit der letzten Bereitstellung auf der Produktionsdatenbank. In diesem Lernprogramm überprüft diese Herausforderungen bereit und bietet verschiedene Strategien zur Unterstützung beim chronicling und Anwenden der Änderungen an der Datenbank seit der letzten Bereitstellung vorgenommen wird.


## <a name="introduction"></a>Einführung

Kopieren den relevanten Inhalt aus der Entwicklungsumgebung, in der produktionsumgebung als in vorherigen Lernprogrammen erläutert wird, umfasst Bereitstellung einer ASP.NET-Anwendung. Bereitstellung ist keine einmalige, aber stattdessen etwas, das erfolgt jedes Mal, wenn eine neue Version der Software veröffentlicht wird, oder wenn Fehler oder die Sicherheitsrisiken identifiziert und behoben wurden. Beim Kopieren von ASP.NET-Seiten wurden Bilder, JavaScript-Dateien und andere solche Dateien in der produktionsumgebung, die Sie nicht benötigen, um sich mit der Verwendung dieser Datei betreffen die seit der letzten Bereitstellung geändert. Sie können die Datei Blind bis hin zur Produktion, kopieren, den vorhandenen Inhalt überschrieben. Leider erstreckt diese Einfachheit sich nicht zum Bereitstellen der Datenbank.

Beim Bereitstellen einer datengesteuerte Anwendung zum ersten Mal können Sie die Datenbank Blind in der Entwicklungsumgebung in die produktionsumgebung kopieren. Aber Ausführen einer Blind Kopie in nachfolgenden Bereitstellungen überschreibt keine Daten eingegeben werden, in der Produktionsdatenbank. Bereitstellen einer Datenbank stattdessen angewandt der *Änderungen* in der Entwicklungsdatenbank seit der letzten Bereitstellung auf der Produktionsdatenbank vorgenommen. In diesem Lernprogramm überprüft diese Herausforderungen bereit und bietet verschiedene Strategien zur Unterstützung beim chronicling und Anwenden der Änderungen an der Datenbank seit der letzten Bereitstellung vorgenommen wird.

## <a name="the-challenges-of-deploying-a-database"></a>Die Herausforderungen der Bereitstellung einer Datenbank

Bevor eine datengesteuerte Anwendung zum ersten Mal bereitgestellt wurde, nur eine Datenbank vorhanden ist, d. h. die Datenbank in der Entwicklungsumgebung, weshalb beim Bereitstellen einer datengesteuerte Anwendung zum ersten Mal Sie blind können kopieren Sie die Datenbank in der Entwicklungsumgebung in der produktionsumgebung. Nachdem die Anwendung bereitgestellt wurde, stehen zwei Kopien der Datenbank aber: eine Entwicklungs-und eine in der Produktion.

Zwischen Bereitstellungen können die Entwicklungs- und produktionsumgebungen Datenbanken nicht synchronisiert werden. Während der Produktion s Datenbankschema unverändert bleibt, kann die Development-s-Datenbankschema ändern, wenn neue Funktionen hinzugefügt werden. Sie können hinzufügen oder Entfernen von Spalten, Tabellen, Sichten oder gespeicherte Prozeduren. Möglicherweise ist auch nicht wichtige Daten, die in der Entwicklungsdatenbank hinzugefügt werden. Viele datengesteuerte Anwendungen enthalten Nachschlagetabellen mit hartcodierten, anwendungsspezifische Daten, die nicht Benutzer bearbeitet werden aufgefüllt. Z. B. möglicherweise eine Auktion Website eine Dropdown-Liste mit Auswahlmöglichkeiten, die die Bedingung des Elements wird versteigert beschreiben: neu, wie neue funktionierende und Fair. Anstatt eine feste Programmierung dieser Optionen direkt in der Dropdown-Liste ist es normalerweise besser, diese in einer Datenbanktabelle zu platzieren. Wenn während der Entwicklung eine neue Bedingung mit dem Namen schlecht die Tabelle dann beim Bereitstellen der Anwendung hinzugefügt wird muss denselben Datensatz zur Nachschlagetabelle in der Produktionsdatenbank hinzugefügt werden.

Im Idealfall würde Bereitstellen der Datenbank das Kopieren der Datenbank von der Entwicklung bis hin zur Produktion. Jedoch sollten Sie bedenken, dass nach der Anwendung bereitgestellt und Entwicklung fortgesetzt, die Produktionsdatenbank mit um echte Daten von wirklichen Benutzer aufgefüllt wird. Aus diesem Grund würden Sie die Datenbank einfach von der Entwicklung bis hin zur Produktion, bei der nächsten Bereitstellung zu kopieren Sie die Produktionsdatenbank zu überschreiben und die vorhandenen Daten verloren gehen. Das Ergebnis ist, dass zum Anwenden der Änderungen in der Entwicklungsdatenbank seit der letzten Bereitstellung Kristallisieren Bereitstellen der Datenbank.

Da Bereitstellen einer Datenbank die Änderungen in das Schema und möglicherweise die Daten seit der letzten Bereitstellung anwenden umfasst, der Änderungsverlauf muss beibehalten (oder zum Zeitpunkt der Bereitstellung bestimmt), damit diese Änderungen auf die Produktion angewendet werden können. Es gibt eine Vielzahl von Techniken für die Verwaltung und Änderungen in das Datenmodell anwenden.

### <a name="defining-the-baseline"></a>Definieren der Basislinie

Um die Änderungen an Ihrer Anwendung-s-Datenbank zu gewährleisten, müssen Sie einige Anfangszustand einer Baseline verfügen, für die Änderungen übernommen werden. Einerseits konnte der Startzustand eine leere Datenbank mit keine Tabellen, Sichten oder gespeicherte Prozeduren sein. Solche eine Baseline führt in einem LargeChange-Protokoll, da die Erstellung aller s Datenbanktabellen, Ansichten und gespeicherten Prozeduren sowie alle Änderungen, die nach der ersten Bereitstellung berücksichtigt werden müssen. Am anderen Ende des Spektrums können Sie die Baseline als die Version der Datenbank festlegen, die anfänglich in der produktionsumgebung bereitgestellt wird. Diese Auswahl führt zu eine wesentlich kleinere Änderungsprotokoll, da sie nur die Änderungen an der Datenbank nach der ersten Bereitstellung enthalten. Dies ist der Ansatz, den ich möchte. Und natürlich Sie können eine weitere Mitte des Ansatzes Road definieren die Baseline als bestimmten Zeitpunkt zwischen der ursprünglichen Erstellung der Datenbank und wenn die Datenbank zuerst bereitgestellt wird.

Nachdem Sie haben berücksichtigen ausgewählte Basislinie Generieren eines SQL-Skripts, das ausgeführt werden kann, um die Baseline-Version neu zu erstellen. Solches Skript ermöglicht die Baseline-Version der Datenbank schnell neu zu erstellen. Diese Funktionalität ist vor allem hilfreich bei größeren Projekten, wobei es möglicherweise mehrere Entwickler arbeiten auf das Projekt oder die zusätzliche Umgebungen, wie z. B. Test- oder Staging, benötigen, die jeweils ihre eigenen Kopie der Datenbank.

Es gibt eine Vielzahl von Tools zur Verfügung, die zum Generieren eines SQL-Skripts der baselineversion. Von SQL Server Management Studio (SSMS) auf die Datenbank Rechtsklick, wechseln Sie zu dem Untermenü Tasks, und wählen Sie die Option Skripts generieren. Dies startet des Skript-Assistenten, in dem mitgeteilt wird, um eine Datei zu generieren, die SQL-Befehle zum Erstellen Ihrer Datenbank s Objekte enthält. Eine andere Möglichkeit ist die Datenbankveröffentlichungs-Assistenten, die die SQL-Befehle nicht nur das Schema der Datenbank, sondern auch die Daten in den Datenbanktabellen erstellen generieren können. Der Datenbankveröffentlichungs-Assistent wurde ausführlich untersucht zurück in die *Bereitstellen einer Datenbank* Lernprogramm. Unabhängig davon, welches Tool Sie verwenden, müssen Sie, am Ende die Notwendigkeit eine Skriptdatei, die Sie verwenden können, die Baseline-Version Ihrer Datenbank neu erstellen sollte auftreten.

## <a name="documenting-the-database-changes-in-prose"></a>Dokumentieren von Datenbankänderungen in Text

Die einfachste Möglichkeit, die während der Entwicklungsphase ein Protokolls der Änderungen in das Datenmodell verwalten werden die Änderungen im Programmierkontext aufzeichnen. Wenn während der Entwicklung einer bereits bereitgestellte Anwendung Sie eine neue Spalte hinzufügen, z. B. die `Employees` Tabelle und eine Spalte aus der `Orders` Tabelle, und fügen Sie eine neue Tabelle (`ProductCategories`), verwalten Sie würde eine Textdatei oder Microsoft Word-Dokument mit dem folgenden Verlauf:

<a id="0.4_table01"></a>


| **Änderungsdatum** | **Ändern von Details** |
| --- | --- |
| 2009-02-03: | Hinzugefügte Spalte `DepartmentID` (`int`, NOT NULL), die `Employees` Tabelle. Eine foreign Key-Einschränkung von hinzugefügt `Departments.DepartmentID` auf `Employees.DepartmentID`. |
| 2009-02-05: | Entfernte Spalte `TotalWeight` aus der `Orders` Tabelle. Zugeordnete Daten, die bereits erfasst `OrderDetails` Datensätze. |
| 2009-02-12: | Erstellt die `ProductCategories` Tabelle. Es gibt drei Spalten: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), und `Active` (`bit`, `NOT NULL`). Um eine primary Key-Einschränkung hinzugefügt `ProductCategoryID`, und ein Standardwert von 1 bis `Active`. |


Es gibt diverse Nachteil dieses Ansatzes liegt. Zunächst einmal besteht keine Möglichkeit für die Automatisierung. Jederzeit müssen diese Änderungen in einer Datenbank - angewendet werden, z. B. wenn die Anwendung bereitgestellt wird – ein Entwickler muss manuell implementieren zum Ändern der einzelnen einzeln nacheinander. Darüber hinaus, wenn Sie eine bestimmte Version der Datenbank, aus der Basislinie mithilfe der Änderungsprotokoll rekonstruieren müssen, wird dadurch so mehr Zeit in Anspruch nehmen als das Protokoll wächst. Ein weiterer Nachteil dieser Methode ist, dass die Klarheit und die Detailebene der Protokolleintrag für die Änderung an die Person, die Aufzeichnung der Änderung bleibt. In einem Team mehrere Entwickler möglicherweise einige ausführlichere, besser lesbar und eine genauere Einträge als andere Stellen. Darüber hinaus sind Tippfehler und andere Human bezogene Dateneingabefehler möglich.

Der Hauptvorteil der dokumentieren die datenbankänderungen in Text ist die Einfachheit. Sie Verschlüsselungskennwort t mit der SQL-Syntax für das Erstellen und Ändern von Datenbankobjekten müssen vertraut sind. Stattdessen können die Änderungen in Programmierkontext aufzuzeichnen, und über SQL Server Management Studio s grafische Benutzeroberfläche zu implementieren.

Verwalten Ihrer Änderungsprotokoll in Programmierkontext, zugegeben, nicht sehr anspruchsvolle und erzielter t Arbeit auch mit bestimmten Projekten, wie diejenigen, die im Gültigkeitsbereich befindet, groß sind haben Sie häufige Änderungen in das Datenmodell oder umfassen Sie mehrere Entwickler. Aber ich haben gesehen, dass dieser Ansatz funktioniert sehr gut in kleinen, one-man-Projekten, die nur gelegentliche Änderungen in das Datenmodell besitzen und, in dem die Steigung Developer verfügt nicht über Kenntnisse in der SQL-Syntax für das Erstellen und Ändern von Datenbankobjekten.

> [!NOTE]
> Während die Informationen in das Änderungsprotokoll nur benötigt, bis eine bereitstellen Zeit wird, technisch gesehen, empfehlen wir der Änderungsverlauf beibehalten. Aber anstatt zu warten ein einzelnes, ständig Protokolldatei ändern, verwenden Sie eine andere Änderung Protokolldatei für jede Datenbankversion. In der Regel sollten auf Version die Datenbank jedes Mal Sie sie bereitgestellt wird. Indem Sie ein Protokoll Änderungsprotokolle verwalten können, beginnend mit der Basislinie ruht, neu erstellen einer beliebigen Datenbankversion durch Ausführen der Protokoll-Änderungsskripts, die beginnend mit Version 1 und fortsetzen, bis Sie erreichen, dass die Version müssen Sie neu erstellen.


## <a name="recording-the-sql-change-statements"></a>Aufzeichnen der SQL-Anweisungen ändern

Der primäre Nachteil des Verwalten des Änderungsprotokolls in Text ist das Fehlen der Automatisierung. Im Idealfall, implementieren die Änderungen an Datenbanken in der Produktionsdatenbank zum Zeitpunkt der Bereitstellung so einfach wie das Klicken auf eine Schaltfläche zum Ausführen eines Skripts, anstatt manuell eine Liste von Anweisungen ausführen wäre. Solche Automatisierung ist möglich durch Verwalten einer Änderungsprotokoll, die diese SQL-Befehle, die auch verwendet, um das Datenmodell enthält.

Die SQL-Syntax umfasst eine Reihe von Anweisungen zum Erstellen und Ändern von verschiedenen Datenbankobjekten. Z. B. die [ *CREATE TABLE-Anweisung*](https://msdn.microsoft.com/en-us/library/ms174979.aspx), bei der Ausführung erstellt eine neue Tabelle mit den angegebenen Spalten und Einschränkungen. Die [ *ALTER TABLE-Anweisung* ](https://msdn.microsoft.com/en-us/library/ms190273.aspx) ändert eine vorhandene Tabelle hinzufügen, entfernen oder ändern die Spalten bzw. Einschränkungen. Es sind auch Anweisungen zum Erstellen, ändern und Löschen von Indizes, Sichten, benutzerdefinierte Funktionen, gespeicherte Prozeduren, Trigger und andere Datenbankobjekte.

Zur im Beispiel oben zurückkehren, die während der Entwicklung einer bereits bereitgestellte Anwendung, die Sie eine neue Spalte hinzufügen image der `Employees` Tabelle und eine Spalte aus der `Orders` Tabelle, und fügen Sie eine neue Tabelle (`ProductCategories`). Solche Aktionen führt zu einer Änderung Protokolldatei mit den folgenden SQL-Befehlen:

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Übertragen diese Änderungen in der Produktionsdatenbank zum Zeitpunkt der Bereitstellung ist ein einmalklick-Vorgang: Öffnen Sie SQL Server Management Studio, Herstellen einer Verbindung mit der Produktionsdatenbank, ein neues Abfragefenster öffnen, fügen Sie den Inhalt des Änderungsprotokolls, und klicken Sie auf Ausführen, um das Skript auszuführen.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Mithilfe eines Vergleichs-Tools zum Synchronisieren der Datenmodelle

Dokumentieren von datenbankänderungen in Text ist einfach, aber implementieren die Änderungen erfordert jede Änderung in der Produktionsdatenbank eine zu einem Zeitpunkt stellen Entwickler; Dokumentieren die SQL-Änderungsbefehle macht, implementieren diese Änderungen in der Produktionsdatenbank so einfach und schnell, wie das Klicken auf eine Schaltfläche, erfordert jedoch erlernen und mastering der SQL-Anweisungen und die Syntax für das Erstellen und Ändern von Datenbankobjekten. Vergleich Datenbanktools nehmen Sie das beste aus beiden Ansätzen und die schlechteste verwerfen.

Eine Datenbank Vergleichstool vergleicht das Schema oder Daten von zwei Datenbanken und zeigt einen zusammenfassenden Bericht anzeigt, wie die Datenbanken unterscheiden. Anschließend können Sie mit einem Klick auf eine Schaltfläche, die SQL-Befehle für die Synchronisierung eine oder mehrere Datenbankobjekte erstellen. Kurz gesagt, können Sie eine Datenbank Vergleichstool vergleichen Sie die Entwicklung und Produktionsdatenbanken zum Zeitpunkt der bereitstellen, generieren eine Datei mit der SQL-Befehle, wenn ausgeführt, gelten die Änderungen für das Produktions-Datenbankschema s so, dass die It Spiegelt das Entwicklung-s-Datenbankschema.

Es gibt eine Vielzahl von Drittanbieter-Database Vergleichstools, die von vielen verschiedenen Anbietern angeboten. Ein Beispiel ist [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), von [ *Red Gate Software*](http://www.red-gate.com/). Lassen Sie s, die durch die Verwendung von SQL Compare zu vergleichen und Synchronisieren der Schemas für Entwicklungs- und produktionsumgebungen Datenbanken zu durchlaufen.

> [!NOTE]
> Zum Zeitpunkt der Erstellung dieses Dokuments wurde die aktuelle Version von SQL Compare-Version 7.1, mit der Standard Edition Kostenberechnungen 395. Sie können durch Herunterladen der 14-Tage-Testversion nachvollziehen.


Beim Starten von SQL Compare Öffnet das Dialogfeld Vergleich Projekte, die mit der gespeicherten SQL Compare-Projekte. Erstellen Sie ein neues Projekt. Dies startet den Projekt-Assistenten, der für Informationen zu den Datenbanken werden Sie aufgefordert, verglichen werden soll (siehe Abbildung 1). Geben Sie die Informationen für die Entwicklung und Produktion Umgebung Datenbanken aus.


[![Vergleichen Sie die Entwicklung und Produktionsdatenbanken](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Abbildung 1**: Vergleichen der Entwicklungs- und Produktionsdatenbanken ([klicken Sie hier, um das Bild in voller Größe angezeigt](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))


> [!NOTE]
> Wenn Ihre Umgebung Entwicklungsdatenbank eine SQL Express Edition-Datenbankdatei in ist die `App_Data` Ordner der Website Sie die Datenbank auf dem SQL Server Express-Datenbankserver zu registrieren, um ihn über das Dialogfeld, das in Abbildung 1 veranschaulichte auszuwählen müssen. Die einfachste Möglichkeit, dies zu erreichen, werden von SQL Server Management Studio (SSMS) öffnen, eine Verbindung mit SQL Server Express-Datenbankserver und fügen Sie die Datenbank ab. Wenn Sie nicht SSMS, die auf Ihrem Computer installiert haben können Sie herunterladen und installieren Sie die kostenlose [ *Version von SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Zusätzlich zur Auswahl der Datenbanken, verglichen werden soll, können Sie auch eine Reihe von vergleichseinstellungen aus der Registerkarte "Optionen" angeben. Eine Option, die Sie aktivieren möchten, möglicherweise ist die "Ignore Einschränkung und Index Namen." Beachten Sie, dass in dem vorherigen Lernprogramm aus, den wir, dass die Anwendungsdienste Datenbankobjekte, die die Entwicklung und Produktion Datenbanken hinzugefügt. Bei Verwendung der `aspnet_regsql.exe` Tool zum Erstellen dieser Objekte in der Produktionsdatenbank, und Sie sehen, dass die primary key- und unique-Einschränkungsnamen zwischen den Datenbanken Entwicklungs- und produktionsumgebungen unterscheiden. Folglich wird SQL Compare alle Tabellen der Anwendung Dienste als unterschiedliche kennzeichnen. Lassen Sie entweder die "Ignore Einschränkung und Index Names" deaktiviert und synchronisieren Sie die Namen der Einschränkungen oder anweisen, SQL vergleichen, um diese Unterschiede zu ignorieren.

Nach der Auswahl der Datenbanken auf ' vergleichen ' (und überprüfen die Vergleichsoptionen), klicken Sie auf die Schaltfläche "jetzt vergleichen", um den Vergleich zu starten. Über den nächsten Sekunden SQL Compare untersucht die Schemas der beiden Datenbanken und generiert einen Bericht, wie sie sich unterscheiden. Ich Ve vorgenommen absichtlich einige Änderungen in der Entwicklungsdatenbank aus, um anzuzeigen, wie solche abweichungen in der SQL Compare-Schnittstelle hingewiesen werden. Wie in Abbildung 2 gezeigt, ich hinzugefügt haben eine `BirthDate` Spalte die `Authors` Tabelle entfernt die `ISBN` Spalte aus der `Books` Tabelle, und eine neue Tabelle hinzugefügt `Ratings`, die Benutzer besuchen der Website-Rate überprüft Bücher können sollen.

> [!NOTE]
> Datenmodelländerungen, die in diesem Lernprogramm vorgenommen wurden ausgeführt, um mit einem Vergleichstool Datenbank zu veranschaulichen. Sie werden diese Änderungen nicht in der Datenbank in zukünftigen Lernprogrammen feststellen.


[![SQL Compare sind die Unterschiede zwischen der Entwicklungs- und Produktionsdatenbanken aufgeführt.](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Abbildung 2**: SQL Compare aufgeführt, die Unterschiede zwischen der Entwicklungs- und Produktionsdatenbanken ([klicken Sie hier, um das Bild in voller Größe angezeigt](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))


SQL Compare gliedert die Datenbankobjekte in Gruppen, welche Objekte schnell angezeigt in beiden Datenbanken vorhanden, aber unterschiedlich sind, die Objekte in einer Datenbank, aber nicht in der anderen vorhanden, und welche Objekte identisch sind. Wie Sie sehen können, sind zwei Objekte, die in beiden Datenbanken vorhanden sind, jedoch unterscheiden: das `Authors` Tabelle, die eine Spalte hinzugefügt haben, und die `Books` Tabelle, die eine entfernt wurde. Es ist ein Objekt, das nur in der Entwicklungsdatenbank, nämlich das neu erstellte zurückgeht `Ratings` Tabelle. Und stehen 117 Objekte, die in beiden Datenbanken identisch sind.

Ein Datenbankobjekt auswählen, zeigt das Fenster SQL Unterschiede, das zeigt, wie diese Objekte unterscheiden. Hervorgehoben Fenster SQL Unterschiede unten in Abbildung 2 angezeigt wird, die `Authors` Tabelle in der Entwicklungsdatenbank verfügt über die `BirthDate` Spalte, die nicht im ist die `Authors` Tabelle in der Produktionsdatenbank.

Als Nächstes werden nach dem Überprüfen der Unterschiede und Auswählen der Objekte, die Sie synchronisieren möchten, die SQL-Befehle erforderlich, um die Produktion s Datenbankschema aktualisieren, entsprechend die Entwicklungsdatenbank erstellen. Dies geschieht mithilfe des Assistenten für die Synchronisierung. Die Synchronisierung-Assistent bestätigt, welche Objekte zum Synchronisieren, und die Aktionen zusammengefasst (siehe Abbildung 3) planen. Sie können die Datenbanken sofort zu synchronisieren oder Generieren eines Skripts mit den SQL-Befehlen, die jederzeit ausgeführt werden können.


[![Mithilfe des Assistenten für die Synchronisierung Ihrer Schemas Datenbanken synchronisieren](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Abbildung 3**: Verwenden Sie den Synchronisierung-Assistenten zum Synchronisieren der Datenbanken Schemas ([klicken Sie hier, um das Bild in voller Größe angezeigt](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))


Vergleich von Datenbanktools wie Red Gate Software SQL Compare s stellen Anwenden der Änderungen am Datenbankschema "Entwicklung" in der Produktionsdatenbank so einfach wie zeigen, und klicken Sie auf.

> [!NOTE]
> Vergleicht und zwei Datenbanken synchronisiert SQL Compare *Schemas*. Leider nicht verglichen werden soll, und die Daten in zwei Datenbanken Tabellen zu synchronisieren. Red Gate Software bietet ein Produkt mit dem Namen [ *SQL Datenvergleich* ](http://www.red-gate.com/products/SQL_Data_Compare/) , vergleicht und synchronisiert die Daten zwischen zwei Datenbanken, aber es ist ein separates Produkt aus SQL vergleichen und die Kosten liegen bei einem anderen 395.


## <a name="taking-the-application-offline-during-deployment"></a>Erstellen die Anwendung Offline, während der Bereitstellung

Als wir in diesen Lernprogrammen sehen Ve Bereitstellung ist ein Prozess, die mehrere Schritte umfasst: kopieren die ASP.NET-Seiten, Masterseiten, CSS-Dateien, JavaScript-Dateien, Bilder und anderen erforderlichen Inhalte aus der Entwicklungsumgebung für die Produktion Umgebung; Kopieren Sie die Konfigurationsinformationen der Produktion umgebungsspezifische bei Bedarf; und Anwenden der Änderungen in das Datenmodell seit der letzten Bereitstellung. Abhängig von der Anzahl der Dateien und die Komplexität der Änderungen an der Datenbank können folgendermaßen von ein paar Sekunden mehrere Minuten dauern. Während dieses Fenster wird die Webanwendung endgültig festgelegt ist, und Benutzern der Website möglicherweise Fehler oder unerwartetes Verhalten auftreten.

Beim Bereitstellen einer Websites empfiehlt es sich um die Webanwendung "offline" dauern, bis zum Abschluss der Bereitstellung. Offlineschalten der Anwendung (und Sichern Sie zu schalten, nachdem der Bereitstellungsvorgang abgeschlossen ist) ist genauso einfach wie das Hochladen einer Datei, und klicken Sie dann löschen. Beginnend mit ASP.NET 2.0, die Anwesenheit einer Datei mit dem Namen `app_offline.htm` in der Anwendung s schaltet Stammverzeichnis die gesamte Website "offline". Jede Anforderung zu einer ASP.NET-Seite an diesem Standort automatisch mit dem Inhalt des beantwortet die `app_offline.htm` Datei. Sobald die Datei entfernt wurde, die Anwendung wieder online geschaltet wird.

Erstellen von einer Anwendung offline, während der Bereitstellung, dann ist so einfach wie das Hochladen einer `app_offline.htm` Datei in der produktionsumgebung s Stammverzeichnis vor der Bereitstellung beginnen und dann löschen (oder etwas anderes umbenennen) nach Bereitstellung ist abgeschlossen. Weitere Informationen zu dieser Technik finden Sie unter John Peterson s-Artikel dauert ein [ *ASP.NET Anwendung Offline*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Zusammenfassung

Die größte Herausforderung beim Bereitstellen einer Anwendung eines datengesteuerten dreht sich alles um Bereitstellen der Datenbank. Da es zwei Versionen der Datenbank - einen in der Entwicklungsumgebung und einen in der produktionsumgebung gibt können diese Schemas der beiden Datenbanken synchronisiert sind, wie die neue Funktionen in der Entwicklung hinzugefügt werden. Welche mehr, da der Produktionsdatenbank als mit um echte Daten von wirklichen Benutzer aufgefüllt wird, Sie nicht die Produktionsdatenbank mit der geänderten Entwicklungsdatenbank überschreiben können wie beim Bereitstellen von Dateien, aus denen die Anwendung (ASP.NET-Seiten, s Bilddateien usw.). Bereitstellen einer Datenbank umfasst stattdessen den genauen Satz von Änderungen in der Entwicklungsdatenbank in der Produktionsdatenbank seit der letzten Bereitstellung implementieren.

In diesem Lernprogramm erläutert drei Verfahren zum Verwalten und zum Anwenden eines Protokolls der Änderungen an der Datenbank. Der einfachste Ansatz ist die Änderungen im Programmierkontext aufzeichnen. Trotz aller bemühungen dies zu Kompilierungsfehlern implementieren diese Änderungen auf die Produktionsdatenbank ein manueller Prozess, ist es nicht Kenntnisse über die SQL-Befehle zum Erstellen und Ändern von Datenbankobjekten erforderlich. Eine anspruchsvollere Vorgehensweise, und einem gefällt in größeren oder Projekte mit mehreren Entwicklern wird zum Aufzeichnen von Änderungen als eine Reihe von SQL-Befehle. Dies beschleunigt erheblich, diese Änderungen in die Zieldatenbank eingeführt. Das beste aus beiden Ansätzen können mit einem Vergleichstool Datenbank z. B. Red Gate Software s SQL Compare erreicht werden.

Dieses Lernprogramm geht unser Fokus zum Bereitstellen einer Anwendung eines datengesteuerten verwendet. Der nächste Satz von Lernprogramme prüft reagieren auf Fehler in der produktionsumgebung. Sehen wir uns eine benutzerfreundlichen Fehlerseite stattdessen anstelle der gelben Bildschirm Tod anzeigen. Und wir sehen, wie die s-Fehlerdetails protokollieren und um Sie zu warnen, wenn solche Fehler auftreten.

Viel Spaß beim Programmieren!

>[!div class="step-by-step"]
[Zurück](configuring-a-website-that-uses-application-services-cs.md)
[Weiter](displaying-a-custom-error-page-cs.md)
