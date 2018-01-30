---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Konfigurations- und Instrumentation | Microsoft Docs
author: microsoft
description: "Es gibt wichtige Änderungen an der Konfiguration und Instrumentation in ASP.NET 2.0. Die neue API zum ASP.NET Konfiguration ermöglicht Änderungen an der Konfiguration erfolgen Pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: 16dfe3c899dfa028d8a52b4b5f9c2868887e8fa9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
<a name="configuration-and-instrumentation"></a>Konfiguration und -Instrumentation
====================
durch [Microsoft](https://github.com/microsoft)

> Es gibt wichtige Änderungen an der Konfiguration und Instrumentation in ASP.NET 2.0. Die neue ASP.NET Konfigurations-API ermöglicht konfigurationsänderungen programmgesteuert erfolgen. Darüber hinaus zahlreiche neue Konfigurationseinstellungen vorhanden für neue Konfigurationen und -Instrumentation zulassen.


Es gibt wichtige Änderungen an der Konfiguration und Instrumentation in ASP.NET 2.0. Die neue ASP.NET Konfigurations-API ermöglicht konfigurationsänderungen programmgesteuert erfolgen. Darüber hinaus zahlreiche neue Konfigurationseinstellungen vorhanden für neue Konfigurationen und -Instrumentation zulassen.

In diesem Modul besprechen wir ASP.NET der Konfigurations-API, wie er bezieht sich auf von lesen und Schreiben in ASP.NET-Konfigurationsdateien und ASP.NET Instrumentation werden ebenfalls behandelt. Außerdem werden die neuen Funktionen von in der ASP.NET-Ablaufverfolgung behandelt.

## <a name="aspnet-configuration-api"></a>ASP.NET Konfigurations-API

Die Konfigurations-API von ASP.NET können Sie entwickeln, bereitstellen und Verwalten von Anwendungsdaten für die Konfiguration mit einem einzelnen Programmierschnittstelle. Sie können die Konfigurations-API entwickeln und vollständige ASP.NET-Konfigurationen programmgesteuert ändern, ohne die XML-Code in den Konfigurationsdateien direkt bearbeiten. Darüber hinaus können Sie den Konfigurations-API in konsolenanwendungen und Skripts, die Sie entwickeln, Web-basierte Verwaltungstools und Microsoft Management Console (MMC)-Snap-ins verwenden.

Die folgenden beiden Configuration Management-Tools verwenden den Konfigurations-API und sind im Lieferumfang von .NET Framework, Version 2.0:

- Die ASP.NET MMC-Snap-in, der die Konfigurations-API zur Vereinfachung von Verwaltungsaufgaben verwendet, die Bereitstellung einer integrierte Ansicht der lokalen Konfigurationsdaten aus allen Ebenen der Konfigurationshierarchie aus.
- Die Websiteverwaltungs-Tool, in dem Sie Konfigurationseinstellungen für lokale und remote-Anwendungen verwalten können, einschließlich der gehosteten Websites.

Die Konfigurations-API von ASP.NET umfasst einen Satz von ASP.NET-Verwaltungsobjekte, die Sie verwenden können, Websites und Anwendungen programmgesteuert zu konfigurieren. Verwaltungsobjekte werden als eine .NET Framework-Klassenbibliothek implementiert. Der Konfigurations-API-Programmiermodell trägt zur Sicherstellung von Konsistenz von Code und Zuverlässigkeit, durch das Erzwingen von Datentypen zum Zeitpunkt der Kompilierung. Die Konfigurations-API zum Verwalten von Anwendungskonfigurationen zu vereinfachen, können Sie Daten anzeigen, die aus der alle Punkte in der Hierarchie, die als einzelne Auflistung, statt die Daten als getrennte Auflistungen aus verschiedenen zusammengeführt werden Konfigurationsdateien. Darüber hinaus können mit die Konfigurations-API vollständige Anwendungskonfigurationen ändern, ohne die XML-Code in den Konfigurationsdateien direkt bearbeiten. Schließlich vereinfacht die API Konfigurationsaufgaben durch die Unterstützung von Verwaltungstools wie die Websiteverwaltungs-Tool. Die Konfigurations-API vereinfacht die Bereitstellung von unterstützen die Erstellung von Konfigurationsdateien auf einem Computer und Konfigurationsskripts auf mehreren Computern ausgeführt.

> [!NOTE]
> Die Konfigurations-API unterstützt nicht die Erstellung von IIS-Anwendungen.


## <a name="working-with-local-and-remote-configuration-settings"></a>Arbeiten mit lokalen und Remote-Konfigurationseinstellungen

Ein Konfigurationsobjekt darstellt, die Ansicht der zusammengeführte Konfigurationseinstellungen, die für eine bestimmte physische Einheit, z. B. ein Computer oder für eine logische Entität, wie eine Anwendung oder eine Website gelten. Die angegebene logische Entität kann auf dem lokalen Computer oder auf einem Remoteserver vorhanden sein. Wenn keine Konfigurationsdatei für eine angegebene Entität vorhanden ist, stellt das Konfigurationsobjekt gemäß der Definition von der Datei "Machine.config" die Standardeinstellungen für die Konfiguration dar.

Sie können ein Konfigurationsobjekt abrufen, mithilfe einer der Methoden Öffnen einer Konfiguration von den folgenden Klassen:

1. Die ConfigurationManager-Klasse, wenn die Entität eine Clientanwendung ist.
2. Die WebConfigurationManager-Klasse, wenn die Entität eine Webanwendung ist.

Diese Methoden geben ein Konfigurationsobjekt zurück, das wiederum die erforderlichen Methoden und Eigenschaften zum Behandeln der zugrunde liegenden Konfigurationsdateien bereitstellt. Sie können diese Dateien zum Lesen oder Schreiben zugreifen.

### <a name="reading"></a>Lesen

Sie verwenden die GetSection oder GetSectionGroup-Methode, um Konfigurationsinformationen zu lesen. Der Benutzer oder Prozess, der liest muss auf allen Konfigurationsdateien in der Hierarchie Leseberechtigungen verfügen.

> [!NOTE]
> Wenn Sie eine statische GetSection-Methode, die einen Path-Parameter akzeptiert verwenden, muss die Path-Parameter an die Anwendung finden Sie in der der Code ausgeführt wird. Andernfalls der Parameter wird ignoriert, und die Konfigurationsinformationen für den aktuell ausgeführten Anwendung wird zurückgegeben.


### <a name="writing"></a>Schreiben

Sie verwenden eine der Methoden speichern Konfigurationsinformationen zu schreiben. Der Benutzer oder Prozess, die Schreibvorgänge benötigen, Schreibberechtigung für die Konfigurationsdatei und das Verzeichnis auf der aktuellen Konfiguration Hierarchieebene sowie Leseberechtigungen für alle Konfigurationsdateien in der Hierarchie.

Um eine Konfigurationsdatei zu generieren, die die geerbten Konfigurationseinstellungen für eine angegebene Entität darstellt, verwenden Sie eine der folgenden Methoden speichern-Konfiguration:

1. Die Save-Methode, um eine neue Konfigurationsdatei zu erstellen.
2. Die SaveAs-Methode, um eine neue Konfigurationsdatei an einem anderen Speicherort zu generieren.

## <a name="configuration-classes-and-namespaces"></a>Configuration-Klassen und Namespaces

Viele Configuration-Klassen und Methoden sind einander ähnlich. Die folgende Tabelle beschreibt die am häufigsten verwendeten Configuration-Klassen und Namespaces.

| **Konfigurationsklasse oder namespace** | **Beschreibung** |
| --- | --- |
| ["System.Configuration"](https://msdn.microsoft.com/library/system.configuration.aspx) Namespace | Enthält die wichtigsten Konfigurationsklassen für alle .NET Framework-Clientanwendungen. Abschnitt Handler Klassen werden verwendet, um Konfigurationsdaten für einen Abschnitt aus Methoden wie GetSection und GetSectionGroup abzurufen. Diese zwei Methoden sind nicht statisch. |
| "System.Configuration.Configuration"-Klasse | Stellt einen Satz von Konfigurationsdaten für einen Computer, Anwendung, Webverzeichnis oder einer sonstigen Ressource dar. Diese Klasse enthält nützliche Methoden, z. B. GetSection und GetSectionGroup, zum Aktualisieren von Konfigurationseinstellungen und Verweise auf Abschnitte und Abschnittsgruppen abrufen. Diese Klasse wird als Rückgabetyp für Methoden verwendet, die zur Entwurfszeit Konfigurationsdaten, wie die Methoden der Klassen WebConfigurationManager und ConfigurationManager abrufen. |
| System.Web.Configuration-namespace | Der Abschnitt Handler-Klassen enthält, für die Konfigurationsabschnitte ASP.NET auf definiert [ASP.NET-Konfigurationseinstellungen](https://msdn.microsoft.com/library/b5ysx397.aspx). Abschnitt Handler Klassen werden verwendet, um Konfigurationsdaten für einen Abschnitt aus Methoden wie GetSection und GetSectionGroup abzurufen. |
| System.Web.Configuration.WebConfigurationManager class | Stellt nützliche Methoden zum Abrufen von Verweisen auf Konfigurationseinstellungen, die zur Laufzeit und Entwurfszeit bereit. Diese Methoden verwenden die Klasse "System.Configuration.Configuration" als Rückgabetyp an. Sie können die statische GetSection Methode dieser Klasse oder die nicht statische GetSection-Methode der Klasse System.Configuration.ConfigurationManager Synonym verwenden. Für Web-Anwendungskonfigurationen wird die System.Web.Configuration.WebConfigurationManager-Klasse anstelle der Klasse System.Configuration.ConfigurationManager empfohlen. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) Namespace | Bietet eine Möglichkeit zum Anpassen und erweitern den Konfigurationsanbieter. Dies ist die Basisklasse für alle Anbieterklassen im Konfigurationssystem. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) Namespace | Enthält Klassen und Schnittstellen für die Verwaltung und Überwachung der Integrität von Webanwendungen. Streng genommen ist dieser Namespace nicht als Teil der Konfigurations-API betrachtet. Z. B. ist Ablaufverfolgung und Auslösen des Ereignisses durch die Klassen in diesem Namespace erreicht. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) Namespace | Stellt die Klassen, die für die Instrumentierung von Anwendungen, deren Verwaltungsinformationen und-Ereignisse über die Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) potenziellen Consumern verfügbar zu machen. ASP.NET-Systemüberwachung verwendet WMI, um Ereignisse zu übermitteln. Streng genommen ist dieser Namespace nicht als Teil der Konfigurations-API betrachtet. |

## <a name="reading-from-aspnet-configuration-files"></a>Das Lesen aus ASP.NET-Konfigurationsdateien

Die WebConfigurationManager-Klasse ist die Hauptklasse für das Lesen aus ASP.NET-Konfigurationsdateien. Es gibt im Wesentlichen drei Schritten ASP.NET-Konfigurationsdateien lesen:

1. Abrufen eines Konfigurationsobjekts, die mithilfe der OpenWebConfiguration-Methode.
2. Rufen Sie einen Verweis auf den gewünschten Abschnitt in der Konfigurationsdatei.
3. Lesen Sie die gewünschte Informationen aus der Konfigurationsdatei.

Die Konfiguration wie das Objekt darstellt, stellt keine bestimmten Konfigurationsdatei dar. Stattdessen stellt es sich um eine zusammengeführte Ansicht der Konfiguration von einem Computer, Anwendung oder Website. Im folgenden Codebeispiel wird ein Konfigurationsobjekt, das die Konfiguration einer Webanwendung namens darstellt instanziiert *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Beachten Sie, dass im oben stehenden Code ist der /ProductInfo Pfad nicht vorhanden, die Standardkonfiguration entsprechend den Angaben in der Datei "Machine.config" zurückgegeben wird.


Nachdem Sie das Konfigurationsobjekt haben, klicken Sie dann können die GetSection oder GetSectionGroup-Methode Sie die Konfigurationseinstellungen nichtkompatibilitätsinformationen. Im folgenden Beispiel wird einen Verweis auf die identitätswechseleinstellungen für die oben genannten ProductInfo-Anwendung:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Das Schreiben in ASP.NET-Konfigurationsdateien

Wie in den Konfigurationsdateien lesen ist die Klasse WebConfigurationManager das Kernstück zum Schreiben in ASP.NET-Konfigurationsdateien. Es gibt auch drei Schritte zum Schreiben in ASP.NET-Konfigurationsdateien.

1. Abrufen eines Konfigurationsobjekts, die mithilfe der OpenWebConfiguration-Methode.
2. Rufen Sie einen Verweis auf den gewünschten Abschnitt in der Konfigurationsdatei.
3. Schreiben Sie die gewünschte Informationen aus der Konfigurationsdatei mit dem Speichern "oder" SaveAs Methode.

Der folgende code ändert die **Debuggen** Attribut des der &lt;Kompilierung&gt; Element auf "false":

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Wenn dieser Code ausgeführt wird, die **Debuggen** Attribut von der &lt;Kompilierung&gt; Element wird auf "false" festgelegt werden die *WebApp* Datei web.config der Anwendung.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management-Namespace

Der System.Web.Management-Namespace stellt Klassen und Schnittstellen für die Verwaltung und Überwachung der Integrität von ASP.NET-Anwendungen.

Protokollierung erfolgt durch Definieren einer Regel, die ein Anbieter Ereignisse zuordnet. Die Regel definiert die Art der Ereignisse, die an den Anbieter gesendet werden. Die folgenden grundlegenden Ereignisse stehen für die Anmeldung zur Verfügung:

| **WebBaseEvent** | Die für alle Ereignisse Basisereignis-Klasse. Enthält die erforderlichen Eigenschaften für alle Ereignisse, beispielsweise Ereigniscode, Detail Ereigniscode, Datum und Uhrzeit, die das Ereignis ausgelöst wurde, Anzahl, die in der ereignismeldung wird und die Ereignisdetails zu sequenzieren. |
| --- | --- |
| **WebManagementEvent** | Die Basis-Ereignisklasse für Verwaltungsereignisse, z. B. die Anwendungslebensdauer anfordern, Fehler und Ereignisse überwachen. |
| **WebHeartbeatEvent** | Das Ereignis generiert, die von der Anwendung in regelmäßigen Abständen nützlich Zustand Laufzeitinformationen zu erfassen. |
| **WebAuditEvent** | Die Basisklasse für Sicherheitsüberwachungsereignisse, die verwendet werden, um Bedingungen wie z. B. Autorisierungsfehler, Entschlüsselungsfehler, markieren *usw..* |
| **WebRequestEvent** | Die Basisklasse für alle Anforderungsereignisse nur zu Informationszwecken. |
| **WebBaseErrorEvent** | Die Basisklasse für alle Ereignisse angezeigt, die möglichen Fehlerzustände. |

Die Typen der verfügbaren Anbieter können Sie Ereignis-Ausgabe an Ereignisanzeige, SQL Server, Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) und e-Mails senden. Die vorkonfiguriert, dass Anbieter und die Ereignis-Zuordnungen verringern die notwendigen Ereignis Ausgabe protokolliert zu arbeiten.

ASP.NET 2.0 verwendet das Ereignisprotokoll Anbieter Out-of-the-Box, um Ereignisse basierend auf Anwendungsdomänen starten und beenden, sowie-Protokollierung nicht behandelten Ausnahmen zu protokollieren. Dadurch wird bei einigen der grundlegenden Szenarien abzudecken. Nehmen wir beispielsweise an, dass die Anwendung löst eine Ausnahme aus, aber der Benutzer den Fehler nicht speichern, und Sie können es nicht reproduzierbar. Mit der Standardregel Ereignisprotokoll würden Sie möglicherweise benötigen Sie die Ausnahme und zum Stack Informationen zum Abrufen von besseren verstehen, welche Art von Fehler aufgetreten ist. Ein weiteres Beispiel gilt, wenn Ihre Anwendung Sitzungszustand verlieren. In diesem Fall können Sie in das Ereignisprotokoll, um zu bestimmen, ob die Anwendungsdomäne wiederverwendet wird, und warum die Anwendungsdomäne ursprünglich beendet sehen.

Das System für die Systemüberwachung ist außerdem erweiterbar. Sie können z. B. benutzerdefinierte Webereignisse definieren, innerhalb der Anwendung ausgelöst werden und definieren Sie eine Regel klicken, um die Ereignisinformationen zu einem Anbieter z. B. Ihre e-Mail-Adresse senden. Dadurch können Sie einfach die Instrumentation der Anbieter für die Systemüberwachung gebunden. Ein weiteres Beispiel konnte Sie ein Ereignis bei jedem Auslösen eine Bestellung verarbeitet und richten Sie eine Regel, die jedes Ereignis mit der SQL Server-Datenbank sendet. Sie können auch auslösen, ein Ereignis, wenn ein Benutzer ein Fehler auftritt, melden Sie sich auf mehrere Male in einer Zeile, und richten Sie das Ereignis an die e-Mail-basierte Anbieter verwenden.

Die Konfiguration für die standardmäßige Anbieter und der Ereignisse wird in der globalen Datei "Web.config" gespeichert. Die globale Datei "Web.config" speichert alle Web-based Einstellungen, die in der Datei "Machine.config" in ASP.NET 1 gespeichert waren X. Die globale Datei "Web.config" befindet sich im folgenden Verzeichnis:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

Die &lt;HealthMonitoring&gt; Teil der globalen Datei "Web.config" stellt Konfigurationseinstellungen. Sie können diese Einstellung zu überschreiben oder konfigurieren Sie Ihren eigenen Einstellungen durch die Implementierung der &lt;HealthMonitoring&gt; Abschnitt in der Datei "Web.config" für Ihre Anwendung.

Die &lt;HealthMonitoring&gt; Teil der globalen Datei "Web.config" enthält die folgenden Elemente:

| **providers** | Enthält die Anbieter für die Ereignisanzeige, WMI und SQL Server eingerichtet. |
| --- | --- |
| **eventMappings** | Enthält Zuordnungen für die verschiedenen WebBase-Klassen. Sie können diese Liste erweitern, wenn Sie eine eigene Ereignisklasse generieren. Generieren eine eigene Klasse des Ereignisses erhalten Sie eine geringere Granularität auf die Anbieter, die, denen Sie Informationen zu senden. Beispielsweise können Sie nicht behandelte Ausnahmen an SQL-Server gesendet werden, während des Sendens von eigenen benutzerdefinierten Ereignisse auf e-Mail konfigurieren. |
| **rules** | Links EventMappings an den Anbieter. |
| **buffering** | Mit SQL Server und e-Mail-Anbieter verwendet, um zu bestimmen, wie oft die Ereignisse an den Anbieter zu leeren. |

Im folgenden finden Sie ein Codebeispiel aus der globalen Datei "Web.config".

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Gewusst wie: Speichern von Ereignissen in der Ereignisanzeige

Wie oben erwähnt: den Anbieter zum Protokollieren von Ereignissen ist im Event Viewer für Sie in der globalen Datei "Web.config" konfiguriert. Standardmäßig alle Ereignisse basierend auf **WebBaseErrorEvent** und **WebFailureAuditEvent** werden protokolliert. Sie können zusätzliche Regeln, um zusätzliche Informationen im Ereignisprotokoll protokollieren hinzufügen. Angenommen, Sie möchten alle Ereignisse zu protokollieren (*also*, jedes Ereignis basierend auf **WebBaseEvent**), konnten Sie die Datei "Web.config" die folgende Regel hinzugefügt:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Diese Regel würde Verknüpfen der **alle Ereignisse** ereigniszuordnung an den Anbieter Ereignisprotokoll. "EventMapping" und der Anbieter sind in der globalen Datei "Web.config" enthalten.

## <a name="how-to-store-events-to-sql-server"></a>Gewusst wie: Speichern von Ereignissen mit SQL Server

Diese Methode verwendet die **ASPNETDB** Datenbank, die von der Aspnet generiert wird\_regsql.exe-Tool. Der Standardanbieter verwendet die "LocalSqlServer"-Verbindungszeichenfolge, die entweder eine dateibasierte Datenbank in der App verwendet\_Datenordner oder der lokalen SQLExpress-Instanz von SQL Server. Die Verbindungszeichenfolge "LocalSqlServer" und der SqlProvider werden in der globalen Datei "Web.config" konfiguriert.

Die Verbindungszeichenfolge "LocalSqlServer" in der globalen Datei "Web.config" sieht wie folgt:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Wenn Sie eine andere SQL Server-Instanz verwenden möchten, müssen Sie das Aspnet verwenden\_regsql.exe-Tool, das sich in der % windir%\Microsoft.Net\Framework\v2.0.\* Ordner. Verwenden Sie das Aspnet\_regsql.exe-Tool zum Generieren einer benutzerdefiniertes **ASPNETDB** Datenbank auf SQL Server-Instanz, und fügen Sie die Verbindungszeichenfolge zu Ihrer Konfigurationsdatei Anwendungen fügen Sie einen Anbieter mithilfe des neuen hinzu Verbindungszeichenfolge. Nachdem Sie haben die **ASPNETDB** Datenbank erstellt, Sie müssen einen Regelsatz ein "EventMapping" mit der SqlProvider zu verknüpfen.

Verwenden Sie den Standardnamen SqlProvider oder einen eigenen Anbieter konfigurieren, müssen Sie eine Regel, verknüpfen den Anbieter mit einem ereigniszuordnung hinzuzufügen. Die folgende Regel verknüpft den neuen Anbieter, die Sie soeben erstellt, um haben die **alle Ereignisse** ereigniszuordnung. Mit dieser Regel werden alle Ereignisse, die basierend auf protokolliert **WebBaseEvent** und senden sie an der MySqlWebEventProvider, die die MYASPNETDB-Verbindungszeichenfolge verwenden. Der folgende Code Fügt eine Regel klicken, um den Anbieter mit einer Karte Ereignis zu verknüpfen:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Wenn Sie nur Fehler mit SQL Server senden möchten, können Sie die folgende Regel hinzufügen:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Weiterleiten von Ereignissen mit WMI

Sie können auch die Ereignisse für WMI weiterleiten. Der WMI-Anbieter ist standardmäßig für Sie in der globalen Datei "Web.config" konfiguriert.

Im folgenden Codebeispiel fügt eine Regel zum Weiterleiten der Ereignisse an WMI hinzu:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Sie müssen eine Regel klicken, um eine "EventMapping" mit dem Anbieter und auch eine WMI-Listener-Anwendung, die für die Ereignisse überwachen zuordnen hinzufügen. Im folgenden Codebeispiel fügt eine Regel klicken, um die WMI-Anbieter zum Verknüpfen der **alle Ereignisse** ereigniszuordnung:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Weiterleiten von Ereignissen auf e-Mails

Sie können auch Ereignisse auf e-Mail weiterleiten. Seien Sie vorsichtig, über welche Ereignisregeln, die Sie an Ihre e-Mail-Anbieter ordnen, wie Sie unbeabsichtigt selbst einer Vielzahl von Informationen, die senden können möglicherweise besser geeignet für SQL Server oder im Ereignisprotokoll. Es gibt zwei e-Mail-Anbieter. SimpleMailWebEventProvider und TemplatedMailWebEventProvider. Jede verfügt über die gleiche Konfigurationsattribute mit Ausnahme von den Attributen "Template" und "DetailedTemplateErrors", die beide nur auf die TemplatedMailWebEventProvider verfügbar sind.

> [!NOTE]
> Keines dieser e-Mail-Anbieter ist für Sie konfiguriert. Sie müssen sie die Datei "Web.config" hinzugefügt.


Der Hauptunterschied zwischen diesen zwei e-Mail-Anbieter ist, dass SimpleMailWebEventProvider-e-Mails in eine generische Vorlage sendet, die nicht geändert werden kann. Die Beispieldatei für die Datei "Web.config" hinzugefügt der Liste der konfigurierten Anbieter die folgende Regel mit dieser e-Mail-Anbieter:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Die folgende Regel wird ebenfalls hinzugefügt, um den e-Mail-Anbieter zum Binden der **alle Ereignisse** ereigniszuordnung:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 Tracing

Es gibt drei wichtige Verbesserungen an der Ablaufverfolgung in ASP.NET 2.0.

1. Integrierte ereignisablaufverfolgungs-Funktionalität
2. Programmgesteuerter Zugriff auf Ablaufverfolgungsmeldungen
3. Verbesserte Protokollierung auf Anwendungsebene

## <a name="integrated-tracing-functionality"></a>Integrierte Funktionen Tracing

Sie können jetzt Weiterleiten von Nachrichten, die ausgegeben wird, von der Klasse System.Diagnostics.Trace Ablaufverfolgungsausgabe ASP.NET und Weiterleiten von Nachrichten von ASP.NET-Ablaufverfolgung auf System.Diagnostics.Trace ausgegeben. Sie können auch ASP.NET Instrumentation System.Diagnostics.Trace Weiterleiten von Ereignissen an. Diese Funktionalität wird bereitgestellt, durch die neue **WriteToDiagnosticsTrace** Attribut von der &lt;Ablaufverfolgung&gt; Element. Wenn dieser boolesche Wert true ist, werden Ablaufverfolgungsmeldungen von ASP.NET die System.Diagnostics-Ablaufverfolgungsinfrastruktur für die Verwendung durch alle Listener weitergeleitet, die registriert werden, um Ablaufverfolgungsmeldungen anzuzeigen.

## <a name="programmatic-access-to-trace-messages"></a>Programmgesteuerter Zugriff auf Nachrichten

ASP.NET 2.0 ermöglicht den programmgesteuerten Zugriff auf alle Ablaufverfolgungsmeldungen über die **TraceContextRecord** Klasse und die **TraceRecords** Auflistung. Die effizienteste Methode des Zugriffs auf Ablaufverfolgungsmeldungen wird zum Registrieren einer **TraceContextEventHandler** Delegaten (ebenfalls neu in ASP.NET 2.0) zum Behandeln des neuen **TraceFinished** Ereignis. Sie können dann die Ablaufverfolgungsmeldungen durchlaufen, beliebig.

Im folgenden Codebeispiel wird veranschaulicht dies:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Im obigen Beispiel ich die TraceRecords-Auflistung durchlaufen und dann jede Nachricht an den Antwortstream schreiben.

## <a name="improved-application-level-tracing"></a>Verbesserte Protokollierung auf Anwendungsebene

Ablaufverfolgung auf Anwendungsebene wird verbessert, über die Einführung der neuen **MostRecent** Attribut des der &lt;Ablaufverfolgung&gt; Element. Dieses Attribut gibt an, ob die letzte auf Anwendungsebene Ablaufverfolgungsausgabe angezeigt wird und ältere Daten jenseits der Grenzen, die durch die RequestLimit angegeben sind, werden verworfen. Wenn "false" werden Ablaufverfolgungsdaten für Anforderungen angezeigt, bis das Attribut RequestLimit erreicht ist.

## <a name="aspnet-command-line-tools"></a>Befehlszeilenprogramme für ASP.NET

Es gibt mehrere Befehlszeilentools, um Unterstützung bei der Konfiguration von ASP.NET. ASP.NET-Entwickler sollten mit der Aspnet vertraut sein\_regiis.exe-Tool. ASP.NET 2.0 bietet drei andere Befehlszeilentools, um die Konfiguration zu erleichtern.

Die folgenden Befehlszeilentools sind verfügbar:

| **Tool** | **Verwendung** |
| --- | --- |
| **aspnet\_regiis.exe** | Ermöglicht die Registrierung von ASP.NET in IIS. Es gibt zwei Versionen dieses Tools, die ASP.NET 2.0 im Lieferumfang enthalten, von denen eine 32-Bit-Systeme (im Ordner Framework) und eine für 64-Bit-Systeme (im Ordner Framework64.) Die 64-Bit-Version wird nicht auf einem 32-Bit-Betriebssystem installiert werden. |
| **aspnet\_regsql.exe** | Die ASP.NET SQL Server-Registrierungstool dient zum Erstellen einer Microsoft SQL Server-Datenbank für die Verwendung durch SQL Server-Anbieter in ASP.NET oder zum Hinzufügen oder Entfernen von Optionen aus einer vorhandenen Datenbank. Das Aspnet\_regsql.exe-Datei befindet sich im Ordner "[drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber" auf dem Webserver. |
| **aspnet\_regbrowsers.exe** | Die ASP.NET Browser Registration-Tool analysiert und alle systemweite Browserdefinitionen in eine Assembly kompiliert und installiert die Assembly im globalen Assemblycache. Das Tool verwendet die Browser-Definitionsdateien (. Browserdateien) aus dem .NET Framework-Browser-Unterverzeichnis. Das Tool kann im Verzeichnis %SystemRoot%\Microsoft.NET\Framework\version\ gefunden werden. |
| **aspnet\_compiler.exe** | Die ASP.NET-Kompilierung-Tool können Sie eine ASP.NET-Webanwendung, entweder direkt oder für die Bereitstellung in einem Zielspeicherort wie z. B. einem Produktionsserver zu kompilieren. Direkter Kompilierung unterstützt die Anwendungsleistung, da Endbenutzer eine Verzögerung bei der ersten Anforderung an die Anwendung nicht treffen, während die Anwendung kompiliert wird. |

Da das Aspnet\_regiis.exe Tool ist nicht in ASP.NET 2.0 neu, nicht besprechen wir sie hier.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Registrierungstool für ASP.NET SQL Server - Aspnet\_regsql.exe

Sie können mehrere Typen von Optionen, die mit dem Tool für die ASP.NET SQL Server-Registrierung festlegen. Sie können Geben Sie eine SQL-Verbindung angeben, welche ASP.NET-Anwendungsdiensten SQL Server zum Verwalten von Informationen, die angeben, welche Datenbank oder Tabelle für SQL-Cacheabhängigkeit verwendet wird und hinzufügen oder Entfernen der Unterstützung für die Verwendung von SQL Server zum Speichern von Prozeduren und Status der Sitzung.

Mehrere ASP.NET-Anwendungsdiensten basieren auf einem Anbieter zu verwalten, speichern und Abrufen von Daten aus einer Datenquelle. Jeder Anbieter bezieht sich auf die Datenquelle. ASP.NET umfasst einen SQL Server-Anbieter für die folgenden ASP.NET-Funktionen:

- Mitgliedschaft (die [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) Klasse).
- Rollenverwaltung (die [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) Klasse).
- Profil (die [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) Klasse).
- Webparts-Personalisierung (die [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) Klasse).
- Web-Ereignisse (die [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) Klasse).

Bei der Installation von ASP.NET enthält die Datei "Machine.config" für den Server Konfigurationselemente, die SQL Server-Anbietern für jede der ASP.NET-Funktionen angeben, die für einen Anbieter abhängig sind. Diese Anbieter sind standardmäßig für die Verbindung mit einer lokalen Benutzerinstanz von SQL Server Express 2005 konfiguriert. Wenn Sie die Standard-Verbindungszeichenfolge, die von den Anbietern ändern, müssen klicken Sie dann vor der Verwendung einer der in der Computerkonfiguration konfigurierten ASP.NET-Funktionen können Sie SQL Server-Datenbank und die Datenbankelemente für die ausgewählte Funktion mit AspnetInstallieren\_regsql.exe. Wenn die Datenbank, die Sie, mit dem SQL-Registrierungstool angeben nicht bereits vorhanden ist (Aspnetdb ist die Standarddatenbank, wenn in der Befehlszeile nicht angegeben wird), muss der aktuelle Benutzer Berechtigungen zum Erstellen von Datenbanken in SQL Server sowie zum Erstellen von Schemas e verfügen Lements innerhalb einer Datenbank.

### <a name="sql-cache-dependency"></a>SQL-Cacheabhängigkeit

Ein erweitertes Feature ASP.NET Ausgabecache handelt es sich um SQL-Cacheabhängigkeit. SQL-Cacheabhängigkeit unterstützt zwei verschiedene Modi des Vorgangs: die andere verwendet eine ASP.NET-Implementierung der Tabelle abrufen und einen zweiten-Modus, der die Benachrichtigung Abfragefunktionen von SQL Server 2005 verwendet. So konfigurieren Sie den Tabellenabruf-Betriebsmodus, kann SQL Registration-Tool verwendet werden.

### <a name="session-state"></a>Sitzungszustand

Standardmäßig werden Sitzungszustandswerte und Informationen im Arbeitsspeicher innerhalb des Prozesses ASP.NET gespeichert. Alternativ können Sie Sitzungsdaten in einer SQL Server-Datenbank speichern, in dem er sich auf mehrere Webserver gemeinsam genutzt werden kann. Wenn die Datenbank, die Sie, für den Sitzungszustand mit dem SQL-Registrierungstool angeben nicht bereits vorhanden ist, muss der aktuelle Benutzer Berechtigungen zum Erstellen von Datenbanken in SQL Server sowie zum Erstellen von Schemaelementen in einer Datenbank haben. Wenn die Datenbank vorhanden ist, muss der aktuelle Benutzer Rechte zum Erstellen von Schemaelementen in der vorhandenen Datenbank haben.

Um die Sitzungszustands-Datenbank auf SQL Server zu installieren, führen Sie das Aspnet\_regsql.exe-Tool, und übergeben Sie die folgenden Informationen ein, mit dem Befehl:

- Der Name der SQL Server-Instanz, mit der **-S** Option.
- Die Anmeldeinformationen für ein Konto mit Berechtigung zum Erstellen einer Datenbank auf einem Computer mit SQL Server. Verwenden Sie die **-E** Option aus, um den aktuell angemeldeten Benutzer verwenden, oder verwenden Sie die **- U** Option zur Angabe einer Benutzer-ID zusammen mit der **-P** Option aus, um ein Kennwort angeben.
- Die **- Ssadd** Befehlszeilenoption Sitzungszustands-Datenbank hinzufügen.

Standardmäßig können Sie keine der Aspnet\_regsql.exe Tool Sitzungszustands-Datenbank auf einem Computer mit SQL Server 2005 Express Edition installiert.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>Registrierungstool für ASP.NET-Browser - Aspnet\_regbrowsers.exe

ASP.NET, Version 1.1, enthalten die Datei "Machine.config" Abschnitt &lt;BrowserCaps&gt;. In diesem Abschnitt enthalten eine Reihe von XML-Einträge, die die Konfigurationen für verschiedene Browser anhand eines regulären Ausdrucks definiert. Für ASP.NET, Version 2.0 ein neues. Browserdatei definiert die Parameter eines bestimmten Browsers unter Verwendung von XML-Einträgen. Sie können Informationen auf ein neues Browserfenster hinzufügen, indem er ein neues. Browserdatei in den Ordner am %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers auf Ihrem System.

Da eine Anwendung jedes Mal, wenn dafür Browserinformationen keine .config-Datei liest, können Sie ein neues erstellen. Browserdatei, und führen Aspnet\_regbrowsers.exe So fügen Sie die erforderlichen Änderungen auf die Assembly hinzu. Dadurch wird den Server auf den neuen Browserinformationen sofort zugreifen, müssen Sie nicht herunterfahren eine Ihrer Anwendungen aus, um die Informationen zu übernehmen. Eine Anwendung kann über die Browser-Eigenschaft des aktuellen HttpRequest Browserfunktionen zugreifen.

Die folgenden Optionen sind verfügbar, wenn Aspnet ausgeführt\_regbrowser.exe:

| **Option** | **Beschreibung** |
| --- | --- |
| **-?** | Zeigt das Aspnet\_regbbrowsers.exe Hilfetext in das Befehlsfenster. |
| **-i** | Die Assembly für die Laufzeit des Browsers erstellt und installiert es im globalen Assemblycache. |
| **-u** | Deinstalliert die Assembly für die Laufzeit des Browsers aus dem globalen Assemblycache. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>Das ASP.NET Kompilierungstool - Aspnet\_compiler.exe

Das Tool für die ASP.NET-Kompilierung kann in zwei allgemeine Arten verwendet werden: für die direkte Kompilierung und Kompilierung für die Bereitstellung, wobei ein Zielverzeichnis für die Ausgabe angegeben wird.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilieren einer Anwendung vorhanden](https://msdn.microsoft.com/library/ms229863.aspx)

Die ASP.NET können Kompilieren einer Anwendung vorhanden, d. h. es das Verhalten, die mehrere Anforderungen an die Anwendung, wodurch die normalen Kompilierung imitiert. Benutzer einer vorkompilierte Website treten keine Verzögerung, die durch Kompilieren der Seite auf die erste Anforderung verursacht.

Wenn Sie eine Site direkt vorkompilieren, gelten die folgenden Elemente:

- Die Site behält ihre Dateien und die Verzeichnisstruktur.
- Sie müssen den Compiler für alle Programmiersprachen, die vom Standort auf dem Server verfügen.
- Jede Datei Kompilierung ein Fehler auftritt, schlägt der gesamte Standort Kompilierung fehl.

Sie können auch der erneuten Kompilierung einer Anwendung vorhanden, nachdem neue Quelldateien hinzugefügt. Kompiliert das Tool nur die neuen oder geänderten Dateien aus, es sei denn, Sie enthalten die **- C** Option.

> [!NOTE]
> Kompilierung einer Anwendung, die eine geschachtelte Anwendung enthält kompiliert die geschachtelte Anwendung nicht. Die geschachtelte Anwendung muss separat kompiliert werden.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilieren einer Anwendung für die Bereitstellung](https://msdn.microsoft.com/library/ms229863.aspx)

Durch Festlegen des Parameters TargetDir kompilieren Sie eine Anwendung für die Bereitstellung (Kompilierung zu einer Zielposition). TargetDir kann der endgültige Position für die Webanwendung, oder die kompilierte Anwendung kann weiter bereitgestellt werden. Mithilfe der **-u** Option wird die Anwendung so, dass Sie Änderungen an bestimmte Dateien in der kompilierten Anwendung vornehmen können, ohne Neukompilieren kompiliert. ASPNET\_compiler.exe wird unterschieden zwischen statischen und dynamischen Dateitypen und ihnen anders verarbeitet, wenn die resultierenden Anwendung zu erstellen.

- Statische Dateien sind solche Pakete, die keinen Compiler zugeordneten haben oder erstellen die Anbieter, wie z. B. Dateien, deren benannte Erweiterungen, z. B. CSS-, GIF, htm, HTML, JPG, js usw. haben. Diese Dateien werden an den Zielspeicherort an, und ihre relativen Positionen in der Verzeichnisstruktur beibehalten einfach kopiert.
- Dynamische Dateitypen sind diejenigen, die einen Compiler zugeordneten oder Anbieter, einschließlich Dateien mit ASP.NET-spezifische Dateinamenerweiterungen, wie z. B. .asax, .ascx, .ashx, aspx, Browser, .master usw. zu erstellen. Das Tool für die ASP.NET-Kompilierung generiert Assemblys aus diesen Dateien. Wenn die **-u** Option ausgelassen wird, erstellt das Tool auch Dateien mit der Dateinamenerweiterung. COMPILED, die die ursprünglichen Quelldateien ihre Assembly zugeordnet. Um sicherzustellen, dass die gleiche Verzeichnisstruktur aufweisen wie die Anwendungsquelle beibehalten wird, generiert das Tool Platzhalterdateien in den entsprechenden Positionen in der Zielanwendung an.

Verwenden Sie die **-u** Option, um anzugeben, dass der Inhalt der kompilierten Anwendung geändert werden kann. Andernfalls werden die nachfolgenden Änderungen werden ignoriert oder zur Laufzeit Fehler verursachen.

In der folgenden Tabelle wird beschrieben, wie die ASP.NET-Kompilierung verschiedene Dateitypen behandelt, wenn die **-u** Option enthalten ist.

| **Dateityp** | **Compilerfehler-Aktion** |
| --- | --- |
| .ascx, .aspx, .master | Diese Dateien werden in Markup und Quellcode, einschließlich der Code-Behind-Dateien und jeglicher Code, der in eingeschlossen ist geteilt &lt;Skript Runat = "Server"&gt; Elemente. Quellcode wird in Assemblys kompiliert, deren Namen, die von einem Hashalgorithmus abgeleitet sind, und die Assemblys befinden sich im Verzeichnis "bin". Inlinecode, also, Code eingeschlossen, zwischen den  **&lt; %**  und  **% &gt;**  Klammern, ist im Lieferumfang von Markup und wird nicht kompiliert. Neue Dateien mit dem gleichen Namen wie die Quelldateien werden erstellt, um das Markup enthalten und in die entsprechenden Ausgabeverzeichnisse eingefügt. |
| .ashx, .asmx | Diese Dateien werden nicht kompiliert und in die Ausgabeverzeichnisse ist und nicht kompiliert verschoben werden. Wenn Sie den Ereignishandler Code kompiliert haben möchten, fügen Sie den Code in Quellcodedateien in der App\_Code (Verzeichnis). |
| cs,. vb, .jsl, cpp (mit Code-Behind-Dateien für die zuvor aufgelisteten Dateitypen) | Diese Dateien werden kompiliert und als Ressource in Assemblys, die auf die Handles verweisen enthalten. Quelldateien werden nicht in das Ausgabeverzeichnis kopiert. Wenn eine Codedatei nicht verwiesen wird, wird er nicht kompiliert. |
| Benutzerdefinierte Dateitypen | Diese Dateien werden nicht kompiliert. Diese Dateien werden in die entsprechenden Ausgabeverzeichnisse kopiert. |
| Quellcodedateien in der App\_Codeunterverzeichnis | Diese Dateien werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. |
| RESX- und RESOURCES-Dateien in der App\_GlobalResources Unterverzeichnis | Diese Dateien werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. Keine App\_GlobalResources-Unterverzeichnis unter dem Verzeichnis Hauptausgabe erstellt wird und keine RESX- oder RESOURCES-Dateien, die im Quellverzeichnis gefunden in die Ausgabeverzeichnisse kopiert. |
| RESX- und RESOURCES-Dateien in der App\_Unterverzeichnis "LocalResources" | Diese Dateien werden nicht kompiliert und in die entsprechenden Ausgabeverzeichnisse kopiert werden. |
| Skin-Dateien in der App\_Designs Unterverzeichnis | Die Skin-Dateien und statische Designdateien werden nicht kompiliert und in die entsprechenden Ausgabeverzeichnisse kopiert werden. |
| Browser-Datei "Web.config" statischen Typen Assemblys im Bin-Verzeichnis bereits vorhanden. | Diese Dateien werden kopiert, unverändert an die Ausgabeverzeichnisse. |

In der folgenden Tabelle wird beschrieben, wie die ASP.NET-Kompilierung verschiedene Dateitypen behandelt, wenn die **-u** Option ausgelassen wird.

| **Dateityp** | **Compilerfehler-Aktion** |
| --- | --- |
| aspx, ASMX, .ashx,. Master | Diese Dateien werden in Markup und Quellcode, einschließlich der Code-Behind-Dateien und jeglicher Code, der in eingeschlossen ist geteilt &lt;Skript Runat = "Server"&gt; Elemente. Quellcode wird in Assemblys, deren Namen kompiliert, die von einem Hashalgorithmus abgeleitet sind. Der resultierenden Assemblys befinden sich im Verzeichnis "bin". Inlinecode, also, Code eingeschlossen, zwischen den  **&lt; %**  und  **% &gt;**  Klammern, ist im Lieferumfang von Markup und wird nicht kompiliert. Der Compiler erstellt neue Dateien, um das Markup mit dem gleichen Namen wie die Quelldateien enthalten. Diese resultierenden Dateien befinden sich im Verzeichnis "bin". Der Compiler erstellt auch Dateien mit dem gleichen Namen wie die Quelldateien, aber mit der Erweiterung. COMPILED, die Zuordnungsinformationen zu enthalten. Die. KOMPILIERTE Dateien werden in die entsprechende am ursprünglichen Speicherort der Quelldateien Ausgabeverzeichnisse platziert. |
| .ascx | Diese Dateien werden in Markup und Quellcode geteilt. Quellcode wird in Assemblys kompiliert und platziert in das Verzeichnis "bin" mit Namen, die von einem Hashalgorithmus abgeleitet sind. Es werden keine Markupdateien generiert. |
| cs,. vb, .jsl, cpp (mit Code-Behind-Dateien für die zuvor aufgelisteten Dateitypen) | Quellcode, die von .ascx, .ashx oder ASPX-Dateien generierten Assemblys verwiesen wird ist in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. Es werden keine Quelldateien kopiert. |
| Benutzerdefinierte Dateitypen | Diese Dateien werden wie dynamische Dateien kompiliert. Je nach Typ der Datei, auf denen sie basieren, kann der Compiler Zuordnungsdateien in die Ausgabeverzeichnisse platzieren. |
| Dateien in der App\_Codeunterverzeichnis | Quellcodedateien in dieses Unterverzeichnis werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. |
| Dateien in der App\_GlobalResources Unterverzeichnis | Diese Dateien werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. Keine App\_GlobalResources-Unterverzeichnis unter dem Verzeichnis Hauptausgabe erstellt wird. Wenn die Konfigurationsdatei AppliesTo gibt = "All" RESX- und RESOURCES-Dateien werden in die Ausgabeverzeichnisse kopiert. Sie werden nicht kopiert, wenn sie von referenziert werden eine [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| RESX- und RESOURCES-Dateien in der App\_Unterverzeichnis "LocalResources" | Diese Dateien werden in Assemblys mit eindeutigen Namen kompiliert und in das Verzeichnis "bin" abgelegt. Keine RESX- oder RESOURCES-Dateien in die Ausgabeverzeichnisse kopiert. |
| Skin-Dateien in der App\_Designs Unterverzeichnis | Designs werden in Assemblys kompiliert und in das Verzeichnis "bin" eingefügt. Stub-Dateien werden für Skin-Dateien erstellt und im entsprechenden Ausgabeverzeichnis eingefügt. Statische Dateien (z. B. CSS) werden in die Ausgabeverzeichnisse kopiert. |
| Browser-Datei "Web.config" statischen Typen Assemblys im Bin-Verzeichnis bereits vorhanden. | Diese Dateien werden kopiert, in das Ausgabeverzeichnis ist. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Feste Assemblynamen](https://msdn.microsoft.com/library/ms229863.aspx##)

Einige Szenarien, z. B. Bereitstellen einer Webanwendung, die mit dem Windows Installer MSI-Datei erfordern die Verwendung von konsistent Dateinamen und Inhalt sowie konsistente Verzeichnisstrukturen um Assemblys oder von Konfigurationseinstellungen für Updates zu identifizieren. In diesen Fällen können Sie die **- Fixednames** Option aus, um anzugeben, dass die ASP.NET eine Assembly kompilieren soll für jede Quelldatei anstelle von Where mit mehreren Seiten in Assemblys kompiliert werden. Dies kann an eine große Anzahl von Assemblys, führen, wenn Sie mit Skalierbarkeit befürchten sollten Sie diese Option mit Vorsicht verwenden.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilierung mit starkem Namen](https://msdn.microsoft.com/library/ms229863.aspx##)

Die **Aptca -**, **- Delaysign**, **- Keycontainer** und **- Keyfile** Optionen stehen zur Verfügung, sodass Sie Aspnet verwenden können\_ Compiler.exe stark erstellen mit dem Namen Assemblys ohne Verwendung der [Strong Name-Tool (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) getrennt. Diese Optionen entsprechen jeweils in **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**, und  **AssemblyKeyFileAttribute**.

Erläuterung dieser Attribute befindet sich außerhalb des Bereichs von diesem Kurs.

## <a name="labs"></a>Labs

Jede der folgenden Übungseinheiten baut auf den vorherigen Übungseinheiten. Sie müssen sie in der Reihenfolge ausgeführt werden.

## <a name="lab-1-using-the-configuration-api"></a>Übung 1: Verwenden der Konfigurations-API

1. Erstellen eine neue Website aufgerufen *mod9lab*.
2. Fügen Sie eine neue Web-Konfigurationsdatei an den Standort aus.
3. Fügen Sie in der Datei "Web.config" Folgendes ein:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Dadurch wird sichergestellt, dass Sie die Berechtigung, um Änderungen an der Datei "Web.config" speichern.

1. "Default.aspx" ein neues Label-Steuerelement hinzu, und ändern Sie die ID in **LblDebugStatus**.
2. Fügen Sie ein neue Schaltflächen-Steuerelement hinzu "default.aspx" ein.
3. Ändern Sie das Schaltflächen-Steuerelement-ID in **BtnToggleDebug** und der Text, der **ein-/ausschalten Debuggen Status**.
4. Öffnen Sie die Codeansicht für das Code-Behind-Datei von "default.aspx" und fügen Sie eine **mit** -Anweisung für **System.Web.Configuration** wie folgt:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Fügen Sie zwei private Variablen auf die Klasse und einer Seite\_Init-Methode, wie unten dargestellt:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Fügen Sie den folgenden Code zur Seite\_laden:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Speichern und Durchsuchen von "default.aspx". Beachten Sie, dass das Label-Steuerelement auf der aktuellen Debugstatus wird angezeigt.
2. Doppelklicken Sie auf das Schaltflächen-Steuerelement im Designer, und fügen Sie den folgenden Code zum Click-Ereignis für das Schaltflächen-Steuerelement:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Speichern und Durchsuchen von "default.aspx", und klicken Sie auf die Schaltfläche "".
2. Die Datei "Web.config" zu öffnen, nachdem jede Schaltfläche, klicken Sie auf, und beobachten Sie die **Debuggen** Attribut in der &lt;Kompilierung&gt; Abschnitt.

## <a name="lab-2-logging-application-restarts"></a>Übung 2: Protokollierung Anwendungsneustarts

In dieser Übung erstellen Sie Code, mit dem Sie die Protokollierung von Anwendung herunterfahren, Neustarten und neukompilierungen in der Ereignisanzeige aktivieren bzw. deaktivieren können.

1. "Default.aspx" DropDownList hinzu, und ändern Sie die ID in DdlLogAppEvents.
2. Legen Sie die **AutoPostBack** -Eigenschaft für der DropDownList zum **"true"**.
3. Der Items-Auflistung für DropDownList drei Elemente hinzugefügt. Stellen Sie die **Text** für das erste Element *Select Value* und der Wert-1 zurück. Stellen Sie die **Text** und **Wert** des zweiten Elements **"true"** und **Text** und **Wert** des dritten Elements **"False"**.
4. Fügen Sie eine neue Bezeichnung hinzu "default.aspx" ein. Ändern Sie die ID in **LblLogAppEvents**.
5. Öffnen Sie die Code-Behind-Ansicht für "default.aspx", und fügen Sie eine neue Deklaration für eine Variable vom Typ HealthMonitoringSection hinzu, wie unten dargestellt:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Fügen Sie den folgenden Code am vorhandenen Code auf der Seite\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Doppelklicken Sie auf der DropDownList, und fügen Sie den folgenden Code dem SelectedIndexChanged-Ereignis:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Durchsuchen von "default.aspx".
2. Legen Sie auf die Dropdownliste **"false"**.
3. Deaktivieren Sie in der Ereignisanzeige das Anwendungsprotokoll.
4. Klicken Sie auf die Schaltfläche, um das Debug-Attribut für die Anwendung zu ändern.
5. Aktualisieren Sie das Anwendungsprotokoll in der Ereignisanzeige. 

    1. Wurden keine Ereignisse werden protokolliert?
    2. Warum oder warum nicht?
6. Legen Sie auf die Dropdownliste **"true".**
7. Klicken Sie auf die Schaltfläche, um das Debug-Attribut für die Anwendung zu wechseln.
8. Der Anmeldename der Anwendung der Ereignisanzeige zu aktualisieren. 

    1. Wurden keine Ereignisse werden protokolliert?
    2. Der Grund für das Herunterfahren der app war?
9. Experimentieren Sie mit der aktiviert oder deaktiviert die Protokollierung, und sehen Sie sich die Änderungen an der Datei "Web.config".

## <a name="more-information"></a>Weitere Informationen:

ASP.NET 2.0 Anbietermodell können Sie Ihre eigenen Anbieter für nicht nur Anwendungsinstrumentation, aber für viele weitere Verwendungsmöglichkeiten sowie z. B. Mitgliedschaft, Profile usw. zu erstellen. Ausführliche Informationen zu einen benutzerdefinierten Anbieter zum Protokollieren von Ereignissen in eine Textdatei schreiben, finden Sie auf [diesen Link](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
