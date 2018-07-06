---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: Strategien zur Datenbankentwicklung und-Bereitstellung (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Beim Bereitstellen einer datengesteuerten Anwendung zum ersten Mal können Sie die Datenbank Blind in der Entwicklungsumgebung in der produktionsumgebung kopieren. B...
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 1964a8c482fba39aeb64158c2cb4624980bcff0d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838711"
---
<a name="strategies-for-database-development-and-deployment-vb"></a>Strategien zur Datenbankentwicklung und-Bereitstellung (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> Beim Bereitstellen einer datengesteuerten Anwendung zum ersten Mal können Sie die Datenbank Blind in der Entwicklungsumgebung in der produktionsumgebung kopieren. Aber eine Blind ausführen Kopie in nachfolgende Bereitstellungen überschreibt alle Daten, die in der Produktionsdatenbank eingegeben wurden. Stattdessen umfasst das Bereitstellen einer Datenbank anwenden der Änderungen, die in der Entwicklungsdatenbank seit der letzten Bereitstellung auf die Produktionsdatenbank. In diesem Tutorial werden diese Herausforderungen und bietet verschiedene Strategien zur Unterstützung der chronicling und Anwenden der Änderungen an der Datenbank seit der letzten Bereitstellung vorgenommen werden.


## <a name="introduction"></a>Einführung

Wie in vorherigen Tutorials erwähnt, umfasst Bereitstellung einer ASP.NET-Anwendung und kopieren den relevanten Inhalt aus der Entwicklungsumgebung, in der produktionsumgebung bereit. Bereitstellung ist es sich nicht um eine einmalige Aufgabe, aber eher etwas, das der Fall, jedes Mal, wenn eine neue Version der Software veröffentlicht wird oder wenn Fehler oder die Sicherheitsrisiken identifiziert und behoben wurden. Beim Kopieren von ASP.NET-Seiten wurden Bilder, JavaScript-Dateien und andere solche Dateien in der produktionsumgebung bereit, die Sie nicht benötigen, um sich darum kümmern, wie diese Datei seit der letzten Bereitstellung geändert. Sie können die Datei Blind in die produktionsumgebung kopieren, die vorhandene Inhalte überschrieben. Leider wird diese einfache Vorgehensweise zum Bereitstellen der Datenbank nicht erweitert.

Beim Bereitstellen einer datengesteuerten Anwendung zum ersten Mal können Sie die Datenbank Blind in der Entwicklungsumgebung in der produktionsumgebung kopieren. Aber eine Blind ausführen Kopie in nachfolgende Bereitstellungen überschreibt alle Daten, die in der Produktionsdatenbank eingegeben wurden. Stattdessen Bereitstellen einer Datenbank wird angewendet, wobei die *Änderungen* in der Entwicklungsdatenbank seit der letzten Bereitstellung auf die Produktionsdatenbank vorgenommen. In diesem Tutorial werden diese Herausforderungen und bietet verschiedene Strategien zur Unterstützung der chronicling und Anwenden der Änderungen an der Datenbank seit der letzten Bereitstellung vorgenommen werden.

## <a name="the-challenges-of-deploying-a-database"></a>Die Herausforderungen beim Bereitstellen einer Datenbank

Bevor Sie eine datengesteuerte Anwendung zum ersten Mal bereitgestellt wurde, nur eine Datenbank vorhanden ist, d. h. die Datenbank in der Entwicklungsumgebung, weshalb beim Bereitstellen einer datengesteuerten Anwendung zum ersten Mal Sie blind können kopieren Sie die Datenbank in der der Entwicklungsumgebung in der produktionsumgebung bereit. Aber nach der Bereitstellung der Anwendungs zwei Kopien der Datenbank vorhanden sind: eine, bei der Entwicklung und eine in der Produktion.

Zwischen den Bereitstellungen können die Datenbanken für Entwicklungs- und produktionsumgebungen nicht synchronisiert werden. Während der Produktion s Datenbankschema unverändert bleibt, kann das Datenbankschema des s Entwicklung ändern, wie neue Funktionen hinzugefügt werden. Sie können hinzufügen oder Entfernen von Spalten, Tabellen, Sichten oder gespeicherte Prozeduren. Möglicherweise ist auch nicht wichtige, Daten, die in der Entwicklungsdatenbank hinzugefügt werden. Viele datengesteuerte Anwendungen umfassen Nachschlagetabellen mit hartcodierten, anwendungsspezifische Daten, die nicht Benutzer bearbeitbare aufgefüllt. Z. B. möglicherweise eine Auktion-Website eine Dropdownliste mit Optionen, die die Bedingung des Elements wird versteigert beschreiben: New, z. B. neue, gut und Fair. Anstatt einer hartcodierung diese Optionen direkt in der Dropdown-Liste ist es normalerweise besser, die sie in einer Datenbanktabelle zu platzieren. Wenn Sie während der Entwicklung eine neue Bedingung mit dem Namen schlecht auf die Tabelle dann hinzugefügt wird beim Bereitstellen der Anwendung muss denselben Datensatz, der Nachschlagetabelle, in der Produktionsdatenbank hinzugefügt werden.

Im Idealfall würde die Bereitstellung der Datenbank beinhalten Kopieren der Datenbank von der Entwicklung zur Produktion. Aber denken Sie daran, dass nach dem Sie die Anwendung bereitgestellt und Entwicklung fortgesetzt, die Produktionsdatenbank mit echten Daten von realen Benutzern aufgefüllt wird. Aus diesem Grund würden Sie die Datenbank einfach von der Entwicklung zur Produktion auf die nächste Bereitstellung zu kopieren Sie die Produktionsdatenbank zu überschreiben und die vorhandenen Daten verloren gehen. Das Ergebnis ist, dass das Bereitstellen der Datenbank zum Anwenden der Änderungen, die in der Entwicklungsdatenbank seit der letzten Bereitstellung läuft.

Da Bereitstellen einer Datenbank umfasst das Anwenden von Änderungen in das Schema und möglicherweise die Daten seit der letzten Bereitstellung, ein Verlauf der Änderungen muss verwaltet (oder zum Zeitpunkt der Bereitstellung bestimmt), damit diese Änderungen in der Produktion angewendet werden können. Es gibt eine Vielzahl von Techniken zum Verwalten und Änderungen in das Datenmodell anwenden.

### <a name="defining-the-baseline"></a>Definieren der Baseline

Um die Änderungen an Ihrer Anwendung-s-Datenbank zu gewährleisten müssen Sie einige Anfangszustand, eine Baseline auf den die Änderungen angewendet werden. Ein Extremfall ist möglicherweise der Anfangszustand, eine leere Datenbank mit keine Tabellen, Sichten oder gespeicherte Prozeduren. Solche eine Baseline führt in einem Protokoll und umfangreiche Änderung, da die Erstellung aller die s-Datenbanktabellen, Sichten und gespeicherten Prozeduren zusammen mit der alle Änderungen, die nach der ersten Bereitstellung berücksichtigt werden müssen. Am anderen Ende des Spektrums können Sie die Baseline als die Version der Datenbank festlegen, die anfänglich in der produktionsumgebung bereitgestellt wird. Diese Auswahl führt zu eine viel kleinere Änderungsprotokoll, da sie nur die Änderungen an der Datenbank nach der ersten Bereitstellung enthalten. Dies ist der von mir bevorzugte Ansatz. Und natürlich können Sie eine weitere mittleren Road-Ansatz definieren die Baseline als irgendwann zwischen der ersten Erstellung der Datenbank, und wenn die Datenbank zuerst bereitgestellt wird.

Sobald man berücksichtigen die ausgewählte Baseline Generieren eines SQL-Skripts, das ausgeführt werden kann, um die Baseline-Version neu zu erstellen. Einem solchen Skript ermöglicht es, schnell die Baseline-Version der Datenbank neu erstellen. Diese Funktion ist insbesondere für größere Projekte, wobei es möglicherweise mehrere Entwickler arbeiten auf das Projekt oder eine weitere Umgebungen, z. B. Tests oder Staging und benötigen, die jeweils eigene Kopie der Datenbank.

Es gibt eine Vielzahl von Tools zur Verfügung, um ein SQL­Skript für die Baseline-Version zu generieren. Von SQL Server Management Studio (SSMS) für die Datenbank mit der rechten Maustaste wird, können, wechseln Sie zu dem Untermenü Tasks, und wählen Sie die Option Skripts generieren. Dadurch wird der Skript-Assistent, der Sie anweisen können, um eine Datei zu generieren, die SQL-Befehle zum Erstellen von der Datenbank-s-Objekten enthält. Eine weitere Option ist der Datenbankveröffentlichungs-Assistenten, die die SQL-Befehle, um nicht nur das Datenbankschema, sondern auch die Daten in den Tabellen zu erstellen, generieren kann. Der Datenbankveröffentlichungs-Assistent im Detail untersucht wurde in der *Bereitstellen einer Datenbank* Tutorial. Unabhängig davon, welches Tool Sie verwenden, müssen Sie, letztlich müssen eine Skriptdatei, die Sie verwenden können, um die Baseline-Version Ihrer Datenbank neu zu erstellen sollte auftreten.

## <a name="documenting-the-database-changes-in-prose"></a>Dokumentieren die Datenbankänderungen im Text

Die einfachste Möglichkeit, ein Protokoll der Änderungen in das Datenmodell während der Entwicklungsphase aufzeichnen werden die Änderungen im Text zu zeichnen. Wenn während der Entwicklung eine bereits bereitgestellte Anwendung Sie eine neue Spalte hinzufügen, z. B. die `Employees` Tabelle und eine Spalte aus der `Orders` Tabelle, und fügen Sie eine neue Tabelle (`ProductCategories`), würden Sie verwalten, einer Textdatei oder Microsoft Word-Dokument mit den folgenden Verlauf:

<a id="0.8_table01"></a>


| **Datum der Änderung** | **Änderungsdetails** |
| --- | --- |
| 2009-02-03: | Hinzugefügte Spalte `DepartmentID` (`int`, NOT NULL), die `Employees` Tabelle. Eine foreign Key-Einschränkung von hinzugefügt `Departments.DepartmentID` zu `Employees.DepartmentID`. |
| 2009-02-05: | Entfernte Spalte `TotalWeight` aus der `Orders` Tabelle. Zugeordnete Daten, die bereits erfasst `OrderDetails` Datensätze. |
| 2009-02-12: | Erstellt die `ProductCategories` Tabelle. Es gibt drei Spalten: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), und `Active` (`bit`, `NOT NULL`). Um eine primary Key-Einschränkung hinzugefügt `ProductCategoryID`, und ein Standardwert von 1 bis `Active`. |


Es gibt eine Reihe von Nachteilen dieses Ansatzes. Es ist beispielsweise keine Hoffnung für die Automatisierung. Müssen diese Änderungen jederzeit auf eine Datenbank - angewendet werden, wie z. B. wenn die Anwendung bereitgestellt wird – ein Entwickler muss manuell implementieren. zum Ändern der einzelnen einzeln nacheinander. Darüber hinaus, wenn Sie eine bestimmte Version der Datenbank von der Baseline, die mit dem Änderungsprotokoll wiederherstellen müssen, wird dies so mehr Zeit in Anspruch nehmen wächst die Größe des Protokolls. Ein weiterer Nachteil dieser Methode ist, dass die Übersichtlichkeit und die Details der einzelnen Protokolleinträge für die Änderung bleibt der Person, die Aufzeichnung der Änderung. In einem Team mit mehreren Entwicklern möglicherweise einige ausführlichere, besser lesbar und eine genauere Einträge als andere Stellen. Darüber hinaus sind Rechtschreibfehler korrigiert und andere Menschen bezogene Dateneingabefehler möglich.

Der Hauptvorteil die datenbankänderungen im Text zu dokumentieren, ist Einfachheit. T muss Vertrautheit mit der SQL-Syntax für das Erstellen und Ändern von Datenbankobjekten Einbau zusätzlichen. Sie können stattdessen Notieren Sie die Änderungen im Text und über die grafische Benutzeroberfläche für SQL Server Management Studio s implementieren.

Verwalten Ihrer Änderungsprotokoll im Text, zugegebenermaßen nicht sehr anspruchsvolle und gewonnenem t arbeiten gut mit bestimmte Projekte, z. B. ist, die im Bereich groß sind haben Sie häufige Änderungen des Datenmodells oder umfassen Sie mehrere Entwickler. Aber ich habe diesen Ansatz funktioniert sehr gut in kleinen, one-man-Projekten, die nur gelegentliche Änderungen des Datenmodells aufweisen und, in dem der Mann verfügt nicht über eine umfassende Erfahrung in der SQL-Syntax zum Erstellen und Ändern von Datenbankobjekten, gesehen.

> [!NOTE]
> Während die Informationen im Änderungsprotokoll, technisch gesehen nur erforderlich, bis zum Zeitpunkt der Bereitstellung ist, empfehle ich einen Verlauf der Änderungen beibehalten. Doch anstatt zu warten, eine einzelne, ständig Log-Änderungsdatei, erwägen Sie, dass eine andere Änderung Protokolldatei für jede Datenbankversion. In der Regel empfiehlt auf Version die Datenbank jedes Mal, die sie bereitgestellt wird. Indem Sie ein Protokoll der Änderungsprotokolle verwalten kann, ausgehend von der Baseline neu erstellt jede Datenbankversion durch Ausführen der Log-Änderungsskripts ab Version 1 und fortsetzen, bis Sie erreichen, dass die Version muss wiederhergestellt werden.


## <a name="recording-the-sql-change-statements"></a>Aufzeichnen der SQL-Anweisungen ändern

Der Hauptnachteil verwalten das Änderungsprotokoll im Text ist das Fehlen der Automatisierung. Implementieren die Änderungen in der Datenbank in der Produktionsdatenbank zum Zeitpunkt der Bereitstellung würde idealerweise so einfach wie das Klicken auf eine Schaltfläche zum Ausführen eines Skripts, anstatt manuell ausführen, eine Liste von Anweisungen sein. Diese Art der Automatisierung ist möglich, indem ein, die die SQL-Befehle verwendet, um das Datenmodell enthält Änderungsprotokoll beibehalten.

Die SQL-Syntax enthält eine Anzahl von Anweisungen zum Erstellen und ändern die verschiedenen Datenbankobjekte. Z. B. die [ *CREATE TABLE-Anweisung*](https://msdn.microsoft.com/library/ms174979.aspx), bei der Ausführung erstellt eine neue Tabelle mit den angegebenen Spalten und Einschränkungen. Die [ *ALTER TABLE-Anweisung* ](https://msdn.microsoft.com/library/ms190273.aspx) eine vorhandene Tabelle hinzufügen, entfernen oder ändern die Spalten bzw. Einschränkungen ändert. Es gibt auch Anweisungen zum Erstellen, ändern und Löschen von Indizes, Sichten, benutzerdefinierte Funktionen, gespeicherte Prozeduren, Trigger und andere Datenbankobjekte.

An die im Beispiel oben zurückgegeben werden, die während der Entwicklung eine bereits bereitgestellte Anwendung, die Sie eine neue Spalte hinzufügen Bild der `Employees` Tabelle und eine Spalte aus der `Orders` Tabelle, und fügen Sie eine neue Tabelle (`ProductCategories`). Aktionen dieser Art würde sich in einer Protokolldatei ändern, mit den folgenden SQL-Befehlen:

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

Übertragen diese Änderungen in der Produktionsdatenbank zum Zeitpunkt der Bereitstellung ist ein Vorgang mit nur einem Klick: Öffnen Sie SQL Server Management Studio, eine Verbindung mit der Produktionsdatenbank herstellen, ein neues Abfragefenster öffnen, fügen Sie den Inhalt des Änderungsprotokolls und klicken Sie auf Ausführen, um das Skript auszuführen.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Synchronisieren die Datenmodelle mithilfe eines Vergleichstools

Dokumentieren von datenbankänderungen in Prose ist ganz einfach implementieren die Änderungen muss allerdings Entwickler jedes auf eine Produktionsdatenbank zu einem Zeitpunkt ändern; Dokumentieren die SQL-Befehle geändert wird, implementieren diese Änderungen auf die Datenbank der Produktion so einfach und schnell, wie das Klicken auf eine Schaltfläche, erfordert jedoch erlernen und verwenden die SQL-Anweisungen und die Syntax zum Erstellen und Ändern von Datenbankobjekten. Datenbank-Vergleichstools nehmen das beste aus beiden Ansätzen und verwerfen die schlechteste.

Ein Datenbank-Vergleichstools vergleicht das Schema oder Daten von zwei Datenbanken und zeigt einen zusammenfassenden Bericht zeigt Ihnen, wie sich die Datenbanken unterscheiden. Anschließend können Sie mit der auf eine Schaltfläche, die SQL-Befehle für die Synchronisierung von ein oder mehrere Datenbankobjekte generieren. Kurz gesagt, können Sie eine Datenbank-Vergleichstools vergleichen Sie die Entwicklung und Produktionsdatenbanken zum Zeitpunkt der Bereitstellung, generieren eine Datei mit der SQL-Befehle, bei der Ausführung, gelten die Änderungen für das Schema der Produktions-s-Datenbank so, dass die It Spiegelt das Development-s-Datenbankschema.

Es gibt eine Vielzahl von Drittanbieter-Datenbank-Vergleichstools von vielen unterschiedlichen Herstellern angeboten werden. Ein Beispiel ist [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), [ *Red Gate Software*](http://www.red-gate.com/). Lassen Sie s, die durch die Verwendung von SQL Compare zum Vergleichen und synchronisieren die Schemas für Entwicklungs- und produktionsumgebungen Datenbanken geleitet.

> [!NOTE]
> Zum Zeitpunkt der Erstellung dieses Dokuments wurde die aktuelle Version von SQL Compare-Version 7.1, mit der Standard Edition 395 $ Kosten. Nachfolgend eine 14-Tage-Testversion herunterladen.


Beim Starten von SQL Compare Öffnet das Dialogfeld Vergleich-Projekte, die mit der gespeicherten SQL Compare-Projekte. Erstellen Sie ein neues Projekt. Dadurch wird es sich um den Projekt-Assistenten, der Informationen zu den Datenbanken dazu aufgefordert werden, verglichen werden soll (siehe Abbildung 1). Geben Sie die Informationen für die Datenbanken für Entwicklungs- und produktionsumgebungen Umgebung aus.


[![Vergleichen Sie die Entwicklung und Produktion von Datenbanken](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**Abbildung 1**: Vergleichen Sie die Entwicklung und Produktion von Datenbanken ([klicken Sie, um das Bild in voller Größe anzeigen](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))


> [!NOTE]
> Der Entwicklungsdatenbank für die Umgebung ist eine SQL Express Edition-Datenbankdatei in den `App_Data` Ordner der Website Sie die Datenbank auf dem SQL Server Express-Datenbank-Server zu registrieren, um es aus dem in Abbildung 1 dargestellten Dialogfeld auswählen müssen. Die einfachste Möglichkeit hierzu ist, öffnen Sie SQL Server Management Studio (SSMS), mit dem SQL Server Express-Datenbank-Server verbinden, und fügen Sie die Datenbank. Wenn Sie nicht SSMS-Installation auf Ihrem Computer verfügen können Sie herunterladen und installieren Sie die kostenlose [ *Version von SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Zusätzlich zur Auswahl der Datenbanken, verglichen werden soll, können Sie auch eine Vielzahl von Einstellungen für Schwellenwertvergleich aus der Registerkarte "Optionen" angeben. Eine Möglichkeit, die Sie aktivieren möchten, möglicherweise ist die "Ignore-Einschränkung und Index Namen." Denken Sie daran, dass in den vorherigen Tutorials aus, den wir, dass die Anwendung auf Datenbankobjekte, die die Datenbanken für Entwicklungs- und produktionsumgebungen services hinzugefügt. Bei Verwendung der `aspnet_regsql.exe` Tool, um diese Objekte auf die Produktionsdatenbank zu erstellen, und Sie feststellen, dass die Datenbanken für Entwicklungs- und produktionsumgebungen die primary key- und unique-Einschränkungsnamen unterscheiden. Daher wird SQL Compare aller Application Services Tabellen als unterschiedliche flag. Lassen Sie entweder die "ignorieren-Einschränkung und Index-Namen" deaktiviert und synchronisieren Sie die Namen der Einschränkungen oder anweisen, SQL vergleichen, um diese Unterschiede ignoriert werden sollen.

Nach der Auswahl der Datenbanken vergleichen (und überprüfen die Vergleichsoptionen), klicken Sie auf die Schaltfläche "jetzt vergleichen", um den Vergleich zu beginnen. Über den nächsten Sekunden die Schemas der beiden Datenbanken untersucht und generiert einen Bericht über deren Unterschiede SQL Compare. Ich Ve vorgenommen absichtlich einige Änderungen in der Entwicklungsdatenbank aus, um anzuzeigen, wie solche abweichungen in der SQL Compare-Schnittstelle angegeben werden. Wie in Abbildung 2 gezeigt, ich hinzugefügt haben eine `BirthDate` Spalte die `Authors` Tabelle entfernt die `ISBN` Spalte aus der `Books` Tabelle, und eine neue Tabelle hinzugefügt `Ratings`, die Benutzer besuchen der Website-Rate überprüft Bücher können sollen.

> [!NOTE]
> Die Änderungen des Datenmodells, die in diesem Tutorial vorgenommen wurden vorgenommen, um mit einem Datenbank-Vergleichs-Tool zu veranschaulichen. Sie werden diese Änderungen nicht in zukünftigen Lernprogrammen in der Datenbank gefunden.


[![SQL-Vergleich werden die Unterschiede zwischen der Entwicklung und Produktion von Datenbanken](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**Abbildung 2**: SQL Compare aufgeführt, die Unterschiede zwischen der Entwicklung und Produktion von Datenbanken ([klicken Sie, um das Bild in voller Größe anzeigen](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))


Die Datenbankobjekte in Gruppen unterteilt werden SQL Compare schnell Sie welche Objekte mit in beiden Datenbanken vorhanden sind, aber unterschiedlich sind, die Objekte in einer Datenbank, aber nicht in der anderen vorhanden und welche Objekte identisch sind. Wie Sie sehen können, sind zwei Objekte, die in beiden Datenbanken vorhanden sind, jedoch unterscheiden: das `Authors` -Tabelle, die eine Spalte hinzugefügt haben, und die `Books` -Tabelle, die eine entfernt. Es gibt ein Objekt, das nur in der Entwicklungsdatenbank, d. h. die neu erstellte vorhanden `Ratings` Tabelle. Und es gibt 117 Objekte, die in beiden Datenbanken identisch sind.

Ein Datenbankobjekt auswählen, zeigt die SQL-Unterschiede-Fenster, in dem zeigt, wie diese Objekte unterscheiden. Im SQL-Unterschiede Fenster unten in Abbildung 2 angezeigt werden, die die `Authors` Tabelle in der Entwicklungsdatenbank verfügt über die `BirthDate` Spalte, die in nicht gefunden wurde die `Authors` Tabelle auf die Produktionsdatenbank.

Im nächste Schritt werden nach dem die Unterschiede überprüfen und Auswählen der Objekte, die Sie synchronisieren möchten, generieren die SQL-Befehle erforderlich, um die Produktion s Datenbankschema zu aktualisieren, mit die Entwicklungsdatenbank übereinstimmen. Dies erfolgt mithilfe des Assistenten für die Synchronisierung. Der Assistent für die Synchronisierung bestätigt werden soll, welche Objekte, die zum Synchronisieren, und fasst die Aktion planen (siehe Abbildung 3). Sie können die Datenbanken sofort zu synchronisieren oder generieren ein Skript mit der SQL-Befehle, die in aller Ruhe ausgeführt werden können.


[![Mithilfe des Assistenten für die Synchronisierung Ihrer Datenbankschemas synchronisieren](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**Abbildung 3**: Verwenden Sie den Assistenten "Synchronisierung" zu synchronisieren der Datenbankschemas ([klicken Sie, um das Bild in voller Größe anzeigen](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))


Datenbank-Vergleichstools wie Red Gate Software SQL Compare s stellen Anwenden der Änderungen am Datenbankschema "Entwicklung" in der Produktionsdatenbank, die so einfach wie zeigen und klicken Sie auf.

> [!NOTE]
> Vergleicht und zwei Datenbanken synchronisiert SQL Compare *Schemas*. Leider es nicht verglichen werden soll, und die Daten in zwei Datenbanken, Tabellen zu synchronisieren. Red Gate Software bietet ein Produkt, mit dem Namen [ *SQL Datenvergleich* ](http://www.red-gate.com/products/SQL_Data_Compare/) , vergleicht und synchronisiert die Daten zwischen zwei Datenbanken, aber ist ein separates Produkt SQL Compare und Kosten für eine andere 395 $.


## <a name="taking-the-application-offline-during-deployment"></a>Nehmen die Anwendung offline schalten, während der Bereitstellung

Als wir haben gesehen, die in diesen Lernprogrammen Bereitstellung ist ein Prozess, die mehrere Schritte umfasst: kopieren die ASP.NET-Seiten, Masterseiten, CSS-Dateien, JavaScript-Dateien, Bilder und anderen benötigten Inhalte aus der Entwicklungsumgebung für die Produktion Umgebung; Kopieren die Produktion umgebungsspezifischer Konfiguration-Informationen bei Bedarf; und Anwenden der Änderungen des Datenmodells seit der letzten Bereitstellung. Abhängig von der Anzahl der Dateien und die Komplexität Ihrer Datenbank geändert können diese Schritte an einer beliebigen Stelle in wenigen Sekunden mehrere Minuten dauern. In diesem Zeitfenster wird die Webanwendung Flux, und Benutzer, die Website für den Fehler oder unerwartetes Verhalten auftreten.

Beim Bereitstellen einer Websites empfiehlt es sich um die Webanwendung "offline" dauern, bis die Bereitstellung abgeschlossen ist. Die Anwendung offline zu schalten (und alles aus einer Sicherung nach Abschluss des Bereitstellungsprozesses) ist so einfach wie das Hochladen einer Datei und anschließend gelöscht wird. Ab ASP.NET 2.0, die Anwesenheit einer Datei namens `app_offline.htm` in der Anwendung s dauert Stammverzeichnis der gesamte Website "offline". Jede Anforderung für eine ASP.NET-Seite an diesem Standort automatisch mit dem Inhalt der beantwortet die `app_offline.htm` Datei. Nachdem die Datei entfernt wurde, wird die Anwendung wieder online geschaltet.

Erstellen eine Anwendung offline schalten, während der Bereitstellung, dann ist so einfach wie das Hochladen einer `app_offline.htm` Datei in der produktionsumgebung s Stammverzeichnis vor der Bereitstellung beginnen und anschließend gelöscht wird (oder etwas anderes umbenennen) nach Bereitstellung ist abgeschlossen. Weitere Informationen zu dieser Technik finden Sie in Artikel John Peterson, dass ein [ *ASP.NET Anwendung Offline*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Zusammenfassung

Bereitstellen der Datenbank ist die größte Herausforderung beim Bereitstellen einer datengesteuerten Anwendung decken. Da es zwei Versionen der Datenbank – einen in der Entwicklungsumgebung und einen in der produktionsumgebung gibt können diese Schemas von zwei Datenbanken nicht synchronisiert werden, da neue Features in der Entwicklung hinzugefügt werden. Welche mehr, da die Produktionsdatenbank als mit echten Daten von realen Benutzern aufgefüllt wird, Sie nicht die Produktionsdatenbank mit der geänderten Entwicklungsdatenbank überschreiben können wie beim Bereitstellen der Dateien, aus denen die Anwendung (die ASP.NET-Seiten, s Bilddateien usw.). Dagegen umfasst das Bereitstellen einer Datenbank den genauen Satz von Änderungen in der Entwicklungsdatenbank aus, für die Produktionsdatenbank seit der letzten Bereitstellung implementieren.

In diesem Tutorial haben drei Verfahren zum Verwalten, und ein Protokoll der Änderungen in der Datenbank anwenden. Der einfachste Ansatz werden die Änderungen im Text zu zeichnen. Während dieses Vorgehen implementieren diese Änderungen auf die Datenbank der Produktion ein manueller Prozess stellt, ist es nicht Kenntnisse über die SQL-Befehle zum Erstellen und Ändern von Datenbankobjekten erforderlich. Ein anspruchsvolleres Verfahren und eine weitaus größere Projekte oder Projekte für Entwickler, die mehrere angenehm ist zum Aufzeichnen von Änderungen als eine Reihe von SQL-Befehle. Dies wird deutlich beschleunigt, Einführung dieser Änderungen in die Zieldatenbank. Das beste aus beiden Ansätzen können mithilfe eines Tools der Datenbank-Vergleich, wie z. B. Red Gate Software s SQL Compare erreicht werden.

Dieses Lernprogramm schließt unsere Konzentration auf die Bereitstellung einer datengesteuerten Anwendung. Der nächste Satz von Tutorials untersucht die zum Reagieren auf Fehler in der produktionsumgebung. Wir werden die Vorgehensweise beim Anzeigen einer Fehlermeldung Seite stattdessen anstelle der gelben Bildschirm of Death betrachten. Und wir sehen, wie Sie die s-Fehlerdetails protokollieren und um Sie zu warnen, wenn solche Fehler auftreten.

Viel Spaß beim Programmieren!

> [!div class="step-by-step"]
> [Zurück](configuring-a-website-that-uses-application-services-vb.md)
> [Weiter](displaying-a-custom-error-page-vb.md)
