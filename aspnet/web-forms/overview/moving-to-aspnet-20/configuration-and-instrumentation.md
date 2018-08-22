---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Konfiguration und Instrumentierung | Microsoft-Dokumentation
author: microsoft
description: Es gibt wesentliche Änderungen in der Konfiguration und Instrumentierung in ASP.NET 2.0. Die neue ASP.NET-Konfigurations-API ermöglicht Änderungen an der Konfiguration Pull Request vorgenommen werden...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: ba116140faa0667d504e0ff101c274db9f46079e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827331"
---
<a name="configuration-and-instrumentation"></a>Konfiguration und Instrumentierung
====================
durch [Microsoft](https://github.com/microsoft)

> Es gibt wesentliche Änderungen in der Konfiguration und Instrumentierung in ASP.NET 2.0. Die neue ASP.NET-Konfigurations-API ermöglicht Änderungen an der Konfiguration programmgesteuert erfolgen. Darüber hinaus viele neue Konfigurationseinstellungen vorhanden ermöglichen neue Konfigurationen und Instrumentation.


Es gibt wesentliche Änderungen in der Konfiguration und Instrumentierung in ASP.NET 2.0. Die neue ASP.NET-Konfigurations-API ermöglicht Änderungen an der Konfiguration programmgesteuert erfolgen. Darüber hinaus viele neue Konfigurationseinstellungen vorhanden ermöglichen neue Konfigurationen und Instrumentation.

In diesem Modul werden während dieser bezieht sich auf das Lesen und Schreiben in Konfigurationsdateien für ASP.NET und ASP.NET Instrumentation werden ebenfalls behandelt ASP.NET der Konfigurations-API beschrieben. Die neuen Funktionen von in der ASP.NET-Ablaufverfolgung werden ebenfalls behandelt.

## <a name="aspnet-configuration-api"></a>ASP.NET-Konfigurations-API

Der Konfigurations-API von ASP.NET können Sie entwickeln, bereitstellen und Verwalten von Konfigurationsdaten für die Anwendung mit einer einzelnen Programmierschnittstelle. Sie können die Konfigurations-API verwenden, zum Entwickeln und vollständige ASP.NET-Konfigurationen programmgesteuert ändern, ohne den XML-Code in den Konfigurationsdateien direkt zu bearbeiten. Darüber hinaus können Sie den Konfigurations-API in konsolenanwendungen und Skripts, die Sie entwickeln, in der Web-basierte Verwaltungstools und in Microsoft Management Console (MMC)-Snap-ins verwenden.

Die folgenden zwei Tools für konfigurationsverwaltung Konfigurations-API verwenden und sind im Lieferumfang von .NET Framework, Version 2.0:

- Bereitstellen von der ASP.NET MMC-Snap-in der die Konfigurations-API verwendet, um Verwaltungsaufgaben zu vereinfachen, eine integrierte Ansicht der lokal gespeicherten Konfigurationsdaten aus allen Ebenen der Konfigurationshierarchie.
- Die Websiteverwaltungs-Tool, das Sie zum Verwalten der Konfigurationseinstellungen für lokale und remote-Anwendungen ermöglicht, einschließlich der gehosteten Websites.

Der Konfigurations-API von ASP.NET umfasst einen Satz von ASP.NET-Verwaltungsobjekte, die Sie verwenden können, Websites und Anwendungen programmgesteuert konfigurieren. Management-Objekte werden als eine .NET Framework-Klassenbibliothek implementiert. Das Programmiermodell des Konfigurations-API trägt Codekonsistenz und Zuverlässigkeit durch das Erzwingen von Datentypen zum Zeitpunkt der Kompilierung. Die Konfigurations-API zum Verwalten von Anwendungskonfigurationen zu erleichtern, können Sie Daten anzeigen, die aus der alle Punkte in der Hierarchie, die als eine einzelne Ressourcensammlung, anstatt die Daten als separate Sammlungen von verschiedenen zusammengeführt werden Konfigurationsdateien. Darüber hinaus können mit die Konfigurations-API Sie vollständige Anwendungskonfigurationen zu ändern, ohne den XML-Code in den Konfigurationsdateien direkt zu bearbeiten. Schließlich vereinfacht die API Konfigurationsaufgaben durch die Unterstützung von Verwaltungstools wie das Websiteverwaltungs-Tool. Die Konfigurations-API vereinfacht die Bereitstellung von unterstützt die Erstellung der Konfigurationsdateien auf einem Computer und Konfigurationsskripts auf mehreren Computern ausgeführt.

> [!NOTE]
> Die Konfigurations-API unterstützt die Erstellung von IIS-Anwendungen nicht.


## <a name="working-with-local-and-remote-configuration-settings"></a>Arbeiten mit lokalen und Remote-Konfigurationseinstellungen

Ein Konfigurationsobjekt darstellt, die Ansicht der Konfigurationseinstellungen, die für eine bestimmte physische Einheit, z. B. ein Computer, oder für eine logische Entität, z. B. eine Anwendung oder eine Website gelten. Die angegebene logische Einheit kann auf dem lokalen Computer oder auf einem Remoteserver vorhanden sein. Wenn keine Konfigurationsdatei für eine angegebene Entität vorhanden ist, stellt das Konfigurationsobjekt die Standardeinstellungen für die Konfiguration dar, wie in der Datei Machine.config definiert.

Sie erhalten ein Konfigurationsobjekt, mit einer der von den folgenden Klassen Methoden Konfiguration öffnen:

1. Die ConfigurationManager-Klasse, wenn die Entität eine Client-Anwendung ist.
2. Die WebConfigurationManager-Klasse, wenn die Entität über eine Webanwendung ist.

Diese Methoden werden ein Konfigurationsobjekt zurück, das wiederum die erforderlichen Methoden und Eigenschaften zum Behandeln der zugrunde liegenden Konfigurationsdateien bereitstellt. Sie können diese Dateien zum Lesen oder Schreiben zugreifen.

### <a name="reading"></a>Lesen

Verwenden Sie die GetSection "und" GetSectionGroup-Methode, um Konfigurationsinformationen zu lesen. Der Benutzer oder Prozess, der liest muss auf allen die Konfigurationsdateien in der Hierarchie über Leseberechtigungen verfügen.

> [!NOTE]
> Wenn Sie eine statische GetSection-Methode, die Path-Parameter akzeptiert verwenden, muss der Path-Parameter an die Anwendung finden Sie in der der Code ausgeführt wird. Andernfalls der Parameter wird ignoriert, und die Konfigurationsinformationen für den aktuell ausgeführten Anwendung wird zurückgegeben.


### <a name="writing"></a>Schreiben

Sie verwenden eine der Methoden speichern, um Konfigurationsinformationen zu schreiben. Der Benutzer oder Prozess an, die Schreibvorgänge enthalten, muss Schreibberechtigungen für die Konfigurationsdatei und das Verzeichnis unter der aktuellen Ebene der Hierarchie sowie Leseberechtigungen für alle Konfigurationsdateien in der Hierarchie.

Um eine Konfigurationsdatei zu generieren, die die geerbten Konfigurationseinstellungen für eine angegebene Entität darstellt, verwenden Sie eine der folgenden Methoden speichern-Konfiguration:

1. Die Save-Methode, um eine neue Konfigurationsdatei zu erstellen.
2. Die SaveAs-Methode, um eine neue Konfigurationsdatei an einem anderen Speicherort zu generieren.

## <a name="configuration-classes-and-namespaces"></a>Configuration-Klassen und Namespaces

Viele Configuration-Klassen und Methoden sind einander ähnlich. Die folgende Tabelle beschreibt die am häufigsten verwendete Konfigurationsklassen und Namespaces.

| **Configuration-Klasse oder namespace** | **Beschreibung** |
| --- | --- |
| ["System.Configuration"](https://msdn.microsoft.com/library/system.configuration.aspx) Namespace | Enthält die Hauptkonfiguration-Klassen für alle .NET Framework-Anwendungen. Abschnitt handlerklassen werden zum Abrufen von Konfigurationsdaten für einen Bereich von Methoden, z. B. GetSection "und" GetSectionGroup verwendet. Diese beiden Methoden sind nicht statisch. |
| "System.Configuration.Configuration"-Klasse | Stellt einen Satz von Konfigurationsdaten für einen Computer, Anwendung, Webverzeichnis oder einer sonstigen Ressource. Diese Klasse enthält nützliche Methoden, wie z. B. GetSection "und" GetSectionGroup, zum Aktualisieren von Konfigurationseinstellungen und Verweise auf Abschnitte und Abschnittsgruppen. Diese Klasse dient als Rückgabetyp für Methoden, die während der Entwurfszeit Konfigurationsdaten, wie die Methoden der Klassen WebConfigurationManager und ConfigurationManager abrufen. |
| System.Web.Configuration-namespace | Enthält die Abschnitt-Handler-Klassen für die Konfigurationsabschnitte von ASP.NET auf definiert [ASP.NET-Konfigurationseinstellungen](https://msdn.microsoft.com/library/b5ysx397.aspx). Abschnitt handlerklassen werden zum Abrufen von Konfigurationsdaten für einen Bereich von Methoden, z. B. GetSection "und" GetSectionGroup verwendet. |
| System.Web.Configuration.WebConfigurationManager class | Bietet nützliche Methoden zum Abrufen von Verweisen auf die Laufzeit und Entwurfszeit-Konfigurationseinstellungen an. Diese Methoden verwenden die Klasse "System.Configuration.Configuration" als Rückgabetyp an. Sie können die statische GetSection-Methode dieser Klasse oder die nicht statische GetSection-Methode der-Klasse System.Configuration.ConfigurationManager austauschbar verwenden. -Konfigurationen empfiehlt sich die System.Web.Configuration.WebConfigurationManager-Klasse anstelle der System.Configuration.ConfigurationManager-Klasse. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) Namespace | Bietet eine Möglichkeit zum Anpassen und erweitern den Konfigurationsanbieter an. Dies ist die Basisklasse für alle Anbieterklassen im Konfigurationssystem. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) namespace | Enthält Klassen und Schnittstellen für die Verwaltung und Überwachung der Integrität von Webanwendungen. Genau genommen ist dieser Namespace nicht als Teil der Konfigurations-API betrachtet. Z. B. Ablaufverfolgung und Auslösen des Ereignisses erfolgt durch die Klassen in diesem Namespace. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) Namespace | Stellt die Klassen, die für die Instrumentierung von Anwendungen für ihre Verwaltungsinformationen und Ereignisse über die Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) für potenzielle Kunden verfügbar gemacht. ASP.NET-Systemüberwachung verwendet WMI, um Ereignisse zu übermitteln. Genau genommen ist dieser Namespace nicht als Teil der Konfigurations-API betrachtet. |

## <a name="reading-from-aspnet-configuration-files"></a>Lesen aus der ASP.NET-Konfigurationsdateien

Die WebConfigurationManager-Klasse ist die Hauptklasse für das Lesen von ASP.NET-Konfigurationsdateien. Es gibt im Wesentlichen aus drei Schritten ASP.NET-Konfigurationsdateien lesen:

1. Erhalten Sie ein Konfigurationsobjekt, mit der OpenWebConfiguration-Methode.
2. Rufen Sie einen Verweis auf den gewünschten Abschnitt in der Konfigurationsdatei.
3. Lesen Sie die gewünschte Informationen aus der Konfigurationsdatei.

Die Konfiguration, die Objekt darstellt, stellt keine bestimmten Konfigurationsdatei dar. Stattdessen stellt es sich um eine zusammengeführte Ansicht der Konfiguration von einem Computer, Anwendung oder Website. Im folgenden Codebeispiel wird ein Konfigurationsobjekt, das die Konfiguration einer Webanwendung namens darstellt instanziiert *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Beachten Sie, dass der obige Code ist der /ProductInfo Pfad nicht vorhanden, die Standardkonfiguration, wie angegeben in der Datei "Machine.config" zurückgeben wird.


Nachdem Sie das Konfigurationsobjekt haben, klicken Sie dann können die GetSection "und" GetSectionGroup-Methode Sie die Konfigurationseinstellungen anzuzeigen. Im folgenden Beispiel wird einen Verweis auf die identitätswechseleinstellungen für die oben genannten ProductInfo-Anwendung:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Das Schreiben in ASP.NET-Konfigurationsdateien

Wie in den Konfigurationsdateien lesen ist die WebConfigurationManager-Klasse den Kern für das Schreiben in Asp.NET-Konfigurationsdateien. Des weiteren gibt es drei Schritte zum Schreiben in ASP.NET-Konfigurationsdateien.

1. Erhalten Sie ein Konfigurationsobjekt, mit der OpenWebConfiguration-Methode.
2. Rufen Sie einen Verweis auf den gewünschten Abschnitt in der Konfigurationsdatei.
3. Schreiben Sie die gewünschte Informationen aus der Konfigurationsdatei, die über das Speichern oder SaveAs Methode.

Der folgende code ändert die **Debuggen** Attribut der &lt;Kompilierung&gt; Element auf "false":

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Wenn dieser Code ausgeführt wird, die **Debuggen** Attribut der &lt;Kompilierung&gt; -Element auf "false" festgelegt die *-Web-App* Datei web.config der Anwendung.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management-Namespace

Die System.Web.Management-Namespace enthält Klassen und Schnittstellen für die Verwaltung und Überwachung der Integrität von ASP.NET-Anwendungen.

Protokollierung erfolgt durch Definieren einer Regel, die ein Anbieter Ereignisse zuordnet. Die Regel definiert die Art der Ereignisse, die an den Anbieter gesendet werden. Die folgenden Basis-Ereignisse sind für die Anmeldung zur Verfügung:

| **WebBaseEvent** | Die Klasse für alle Ereignisse. Enthält die erforderlichen Eigenschaften für alle Ereignisse, z. B. Ereigniscode, Ereignisdetailcode, Datum und Uhrzeit, die das Ereignis ausgelöst wurde, Anzahl, die ereignismeldung und Ereignisdetails zu sequenzieren. |
| --- | --- |
| **WebManagementEvent** | Die Klasse für Management-Ereignisse, wie die Lebensdauer der Anwendung, Anforderung, Fehler und Überwachungsereignisse. |
| **WebHeartbeatEvent** | Das Ereignis generiert, die von der Anwendung in regelmäßigen Abständen nützliche Common Language Runtime Informationen erfassen. |
| **WebAuditEvent** | Die Basisklasse für Sicherheitsüberwachungsereignisse, die verwendet werden, markieren Sie Bedingungen wie z. B. Autorisierungsfehler, Fehler beim Entschlüsseln, *usw..* |
| **WebRequestEvent** | Die Basisklasse für alle Information-Request-Ereignisse. |
| **WebBaseErrorEvent** | Die Basisklasse für alle Ereignisse angezeigt, fehlerbedingungen. |

Die Typen der verfügbaren Anbieter können Sie ereignisausgabe-Ereignisanzeige, SQL Server, Windows-Verwaltungsinstrumentation (Windows Management Instrumentation, WMI) und e-Mail zu senden. Das vorkonfiguriert, dass Anbieter und die Ereignis-Zuordnungen reduzieren den Umfang der Arbeit, die zum Ereignis Ausgabe protokolliert.

ASP.NET 2.0 verwendet das Ereignisprotokoll Anbieter Out-von-the-Box Protokollieren von Ereignissen, die basierend auf Anwendungsdomänen starten und beenden, als auch nicht behandelten Ausnahmen zu protokollieren. Dadurch werden einige der grundlegenden Szenarien abzudecken. Nehmen wir beispielsweise an, dass Ihre Anwendung eine Ausnahme auslöst, aber der Benutzer den Fehler nicht speichern, und Sie nicht werden reproduziert können. Mit der Standardregel Ereignisprotokoll würden Sie möglicherweise zum Sammeln von der Ausnahme und Stack Informationen, um einen besseren Überblick darüber, welche Art von Fehler ist aufgetreten. Ein weiteres Beispiel gilt, wenn es sich bei Ihrer Anwendung den Sitzungszustand entfernt wird. In diesem Fall sehen Sie in das Ereignisprotokoll, um zu bestimmen, ob die Anwendungsdomäne wird neu gestartet, und warum die Anwendungsdomäne im vornherein angehalten.

Das System für die Systemüberwachung ist außerdem erweiterbar. Sie können z. B. benutzerdefinierte Web-Ereignisse zu definieren, sie innerhalb der Anwendung ausgelöst und definieren Sie dann auf eine Regel, um die Ereignisinformationen zu einem Anbieter wie z. B. Ihre e-Mail-Adresse senden. Dadurch können Sie ganz einfach zum Binden von der Instrumentierung der Systemüberwachung-Anbieter verwenden. Ein weiteres Beispiel können Sie ein Ereignis jedes Mal ausgelöst eine Bestellung verarbeitet und richten Sie eine Regel, die jedes Ereignis an SQL Server-Datenbank sendet. Sie können auch ein Ereignis, wenn ein Benutzer mehrmals hintereinander anmelden und so verwenden Sie die e-Mail-basierte Anbieter eingerichtet, dass das Ereignis nicht auslösen.

Die Konfiguration für den Standardanbieter und den Ereignissen wird in der globalen Datei "Web.config" gespeichert. Der globale Datei "Web.config" speichert alle Web-basierte Einstellungen, die in der Datei Machine.config in ASP.NET 1. gespeichert wurden X. Der globale Datei "Web.config" befindet sich im folgenden Verzeichnis:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

Die &lt;HealthMonitoring&gt; Teil der globalen Datei "Web.config" stellt Konfigurationseinstellungen. Sie können diese Einstellung zu überschreiben oder konfigurieren Sie Ihre eigenen Einstellungen durch die Implementierung der &lt;HealthMonitoring&gt; Abschnitt in der Datei "Web.config" für Ihre Anwendung.

Die &lt;HealthMonitoring&gt; Teil der globalen Datei "Web.config" enthält die folgenden Elemente:

| **Anbieter** | Enthält Anbieter für die Ereignisanzeige, WMI und SQL Server einrichten. |
| --- | --- |
| **eventMappings** | Enthält die Zuordnungen für die verschiedenen WebBase-Klassen. Sie können diese Liste erweitern, wenn Sie Ihre eigenen Event-Klasse generieren. Generieren Ihren eigenen Event-Klasse bietet Ihnen Feinerer Granularität über die Anbieter, die, denen Sie Informationen zu senden. Sie können z. B. nicht behandelte Ausnahmen für SQL Server gesendet werden, und senden Ihre eigenen benutzerdefinierten Ereignisse an e-Mail-Adresse konfigurieren. |
| **Regeln** | EventMappings verknüpft mit dem Anbieter. |
| **buffering** | Mit SQL Server- und e-Mail-Anbieter verwendet, um zu bestimmen, wie oft die Ereignisse an den Anbieter zu leeren. |

Im folgenden finden Sie ein Codebeispiel, aus der globalen Datei "Web.config".

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Gewusst wie: Speichern von Ereignissen in der Ereignisanzeige

Wie oben erwähnt: den Anbieter zum Protokollieren von Ereignissen wird im Viewer für Sie in der globalen Datei "Web.config" konfiguriert. Standardmäßig alle Ereignisse anhand **WebBaseErrorEvent** und **WebFailureAuditEvent** werden protokolliert. Sie können zusätzliche Informationen im Ereignisprotokoll protokollieren zusätzliche Regeln hinzufügen. Wenn Sie alle Ereignisse zu protokollieren möchten, z. B. (*z. B.*, jedes Ereignis auf der Grundlage **WebBaseEvent**), können Sie die Datei "Web.config" die folgende Regel hinzufügen:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Mit dieser Regel verknüpft die **alle Ereignisse** Event-Zuordnung mit dem Ereignisprotokoll-Anbieter. Sowohl "EventMapping" und "die Anbieter sind in der globalen Datei" Web.config "enthalten.

## <a name="how-to-store-events-to-sql-server"></a>Gewusst wie: Speichern von Ereignissen in SQL Server

Diese Methode verwendet die **ASPNETDB** -Datenbank, die von der Aspnet generierten\_regsql.exe-Tool. Der Standardanbieter verwendet die Verbindungszeichenfolge LocalSqlServer, die entweder eine dateibasierte Datenbank in der App verwendet\_Datenordner des oder der lokalen SQLExpress-Instanz von SQL Server. Sowohl die Verbindungszeichenfolge LocalSqlServer als auch die SqlProvider werden in der globalen Datei "Web.config" konfiguriert.

Die Verbindungszeichenfolge LocalSqlServer in der globalen Datei "Web.config"-Datei sieht folgendermaßen aus:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Wenn Sie eine andere SQL Server-Instanz verwenden möchten, müssen Sie das Aspnet verwenden\_regsql.exe-Tool, das in der % windir%\Microsoft.Net\Framework\v2.0 befinden\* Ordner. Verwenden Sie das Aspnet\_regsql.exe-Tool zum Generieren eines benutzerdefiniertes **ASPNETDB** Datenbank auf SQL Server-Instanz, und klicken Sie dann fügen Sie die Verbindungszeichenfolge zu Ihrer Konfigurationsdatei Anwendungen, und fügen Sie dann einen Anbieter mithilfe des neuen hinzu die Verbindungszeichenfolge. Nachdem Sie haben die **ASPNETDB** Datenbank erstellt wurde, müssen Sie einen Regelsatz ein EventMapping mit der SqlProvider zu verknüpfen.

Ob Sie den Standardwert SqlProvider oder konfigurieren Sie Ihren eigenen Anbieter, müssen Sie eine Regel, verknüpfen den Anbieter mit einem Event-Zuordnung hinzuzufügen. Die folgende Regel verknüpft den neuen Anbieter, die Sie soeben erstellt, um haben die **alle Ereignisse** Event-Zuordnung. Diese Regel protokolliert alle Ereignisse, die basierend auf **WebBaseEvent** und senden sie an der MySqlWebEventProvider, der die Verbindungszeichenfolge MYASPNETDB verwendet wird. Der folgende Code Fügt eine Regel, um den Anbieter mit einem Event-Zuordnung zu verknüpfen:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Falls gewünscht, können Fehler nur auf SQL Server zu senden, können Sie die folgende Regel hinzufügen:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Weiterleiten von Ereignissen mit WMI

Sie können auch die Ereignisse für WMI weiterleiten. Der WMI-Anbieter ist standardmäßig für Sie in der globalen Datei "Web.config" konfiguriert.

Im folgenden Codebeispiel wird eine Regel zum Weiterleiten der Ereignisse für WMI hinzugefügt:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Sie benötigen, um eine Regel zum Zuordnen einer EventMapping an den Anbieter, und darüber hinaus eine WMI-Listener-Anwendung zum Lauschen auf Ereignisse hinzuzufügen. Das folgende Codebeispiel fügt eine Regel, um die WMI-Anbieter zum Verknüpfen der **alle Ereignisse** ereigniszuordnung:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Weiterleiten von Ereignissen an e-Mail-Adresse

Sie können auch Ereignisse an e-Mail-Adresse weiterleiten. Seien Sie vorsichtig, welche Ereignisregeln, die Sie an Ihre e-Mail-Anbieter, zuordnen, wie Sie versehentlich sich einer Vielzahl von Informationen, die senden können möglicherweise besser geeignet für SQL Server oder im Ereignisprotokoll. Es gibt zwei e-Mail-Anbieter. SimpleMailWebEventProvider und TemplatedMailWebEventProvider. Jede verfügt über die gleichen Configuration-Attribute, mit Ausnahme von den Attributen "Template" und "DetailedTemplateErrors", die beide nur auf die TemplatedMailWebEventProvider verfügbar sind.

> [!NOTE]
> Keines dieser e-Mail-Anbieter ist für Sie konfiguriert. Sie müssen sie die Datei "Web.config" hinzufügen.


Der Hauptunterschied zwischen diesen zwei e-Mail-Anbieter ist, dass SimpleMailWebEventProvider-e-Mails in einer generischen Vorlage sendet, die nicht geändert werden kann. Die Beispieldatei "Web.config" hinzugefügt der Liste der konfigurierten Anbieter mithilfe der folgenden Regel dieser e-Mail-Anbieter:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

Die folgende Regel wird auch hinzugefügt, um den e-Mail-Anbieter zum Verknüpfen der **alle Ereignisse** ereigniszuordnung:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 Ablaufverfolgung

Es gibt drei wichtige Verbesserungen bei der Ablaufverfolgung in ASP.NET 2.0.

1. Integrierten Ablaufverfolgungsfunktionen
2. Den programmgesteuerten Zugriff auf Meldungen zur Ablaufverfolgung
3. Verbesserte auf Anwendungsebene-Ablaufverfolgung

## <a name="integrated-tracing-functionality"></a>Integrierte Funktionen Ablaufverfolgung

Sie können jetzt Weiterleiten von Nachrichten, die von der System.Diagnostics.Trace-Klasse für ASP.NET die Ausgabe der Ablaufverfolgung ausgegeben und Weiterleiten von Nachrichten, die von ASP.NET-Ablaufverfolgung "System.Diagnostics.Trace" ausgegeben. Sie können auch ASP.NET-Instrumentationsereignisse "System.Diagnostics.Trace" weiterleiten. Diese Funktionalität wird bereitgestellt, durch die neue **WriteToDiagnosticsTrace** Attribut der &lt;Ablaufverfolgung&gt; Element. Wenn dieser boolesche Wert WAHR ist, werden Meldungen zur Ablaufverfolgung von ASP.NET für die Infrastruktur des System.Diagnostics-Ablaufverfolgung für die Verwendung durch alle Listener weitergeleitet, die registriert werden, um Meldungen zur Ablaufverfolgung anzuzeigen.

## <a name="programmatic-access-to-trace-messages"></a>Den programmgesteuerten Zugriff auf Nachrichten verfolgen

ASP.NET 2.0 ermöglicht den programmgesteuerten Zugriff auf alle Meldungen über die **TraceContextRecord** Klasse und die **TraceRecords** Auflistung. Die effizienteste Methode für den Zugriff auf Meldungen zur Ablaufverfolgung wird zum Registrieren einer **TraceContextEventHandler** Delegat (ebenfalls neu in ASP.NET 2.0) zum Behandeln des neuen **TraceFinished** Ereignis. Sie können dann die Ablaufverfolgungsmeldungen durchlaufen, wie gewünscht.

Das folgende Codebeispiel veranschaulicht dies:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

Im obigen Beispiel, wenn ich die TraceRecords Auflistung durchlaufen und und schreiben dann jede Nachricht in den Antwortstream.

## <a name="improved-application-level-tracing"></a>Verbesserte auf Anwendungsebene-Ablaufverfolgung

Anwendungsebene-Ablaufverfolgung wurde verbessert, über die Einführung der neuen **MostRecent** Attribut der &lt;Ablaufverfolgung&gt; Element. Dieses Attribut gibt an, ob die aktuelle auf Anwendungsebene Ablaufverfolgungsausgabe angezeigt wird, und ältere Daten jenseits der Grenzen, die von der RequestLimit angegeben sind, werden verworfen. False gibt an, werden für die Ablaufverfolgung für Anforderungen angezeigt, bis das Attribut RequestLimit erreicht ist.

## <a name="aspnet-command-line-tools"></a>ASP.NET-Befehlszeilentools

Es gibt mehrere Befehlszeilentools zur Unterstützung bei der Konfiguration von ASP.NET. Entwickler sollten mit der Aspnet vertraut sein\_regiis.exe-Tool. ASP.NET 2.0 bietet drei anderen Befehlszeilentools zur Unterstützung bei der Konfiguration.

Die folgenden Befehlszeilentools sind verfügbar:

| **Tool** | **Verwendung** |
| --- | --- |
| **aspnet\_regiis.exe** | Ermöglicht die Registrierung von ASP.NET mit IIS. Es gibt zwei Versionen dieses Tools, die ASP.NET 2.0 im Lieferumfang enthalten, von denen eine für 32-Bit-Systeme (im Ordner "Framework") und eine für 64-Bit-Systeme (im Ordner Framework64.) Die 64-Bit-Version wird nicht auf einem 32-Bit-Betriebssystem installiert werden. |
| **aspnet\_regsql.exe** | Das Registrierungstool für ASP.NET SQL Server wird verwendet, um eine Microsoft SQL Server-Datenbank für die Verwendung durch SQL Server-Anbieter in ASP.NET zu erstellen oder zum Hinzufügen oder Entfernen von Optionen aus einer vorhandenen Datenbank. Das Aspnet\_regsql.exe-Datei befindet sich in der [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber Ordner auf Ihrem Webserver. |
| **aspnet\_regbrowsers.exe** | Das Registrierungstool für ASP.NET Browser analysiert und alle systemweiten Browserdefinitionen in eine Assembly kompiliert und installiert die Assembly im globalen Assemblycache. Das Tool verwendet die Browserdefinitionsdateien (. Browserdateien) aus dem .NET Framework-Browser-Unterverzeichnis. Das Tool finden Sie im Verzeichnis %SystemRoot%\Microsoft.NET\Framework\version\. |
| **aspnet\_compiler.exe** | Die ASP.NET-Kompilierung-Tool können Sie zum Kompilieren einer ASP.NET Web-Anwendung, entweder direkt oder für die Bereitstellung an einem Zielspeicherort wie z. B. einem Produktionsserver. Direkter Kompilierung kann die Anwendungsleistung, da Endbenutzer keine Verzögerung bei der ersten Anforderung an die Anwendung auftreten während der Kompilierung der Anwendung. |

Da das Aspnet\_regiis.exe Tool nicht in ASP.NET Version 2.0 neu ist, nicht besprechen wir es hier.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Registrierungstool für ASP.NET SQL Server - Aspnet\_regsql.exe

Sie können mehrere Typen von Optionen, die mit dem Registrierungstool für ASP.NET SQL Server festlegen. Sie können eine SQL-Verbindung angeben, welche ASP.NET-Anwendungsdienste SQL Server zum Verwalten von Informationen, die angeben, welche Datenbank oder Tabelle für die SQL-Cacheabhängigkeit verwendet wird hinzufügen und Entfernen der Unterstützung für die Verwendung von SQL Server zum Speichern von Prozeduren und Sitzungszustand.

Mehrere ASP.NET-Anwendungsdienste abhängig von einem Anbieter zu verwalten, speichern und Abrufen von Daten aus einer Datenquelle ab. Jeder Anbieter ist spezifisch für die Datenquelle. ASP.NET umfasst einen SQL Server-Anbieter für die folgenden ASP.NET-Funktionen:

- Mitgliedschaft (die [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) Klasse).
- Rollenverwaltung (die [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) Klasse).
- Profil (die [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) Klasse).
- Webparts-Personalisierung (die [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) Klasse).
- Web-Ereignisse (die [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) Klasse).

Bei der Installation von ASP.NET enthält die Datei "Machine.config" für Ihren Server Konfigurationselemente, die SQL Server-Anbieter für jede der ASP.NET-Features angeben, die für einen Anbieter zu verwenden. Diese Anbieter sind standardmäßig, um eine Verbindung mit einer lokalen Instanz von SQL Server Express 2005 konfiguriert. Wenn Sie die Standardverbindungszeichenfolge, die der Anbieter ändern, müssen klicken Sie dann vor ASP.NET Funktionen konfiguriert werden, in der Konfiguration des Computers, der Verwendung Sie SQL Server-Datenbank und die Datenbankelemente für die ausgewählte Funktion mit AspnetInstallieren\_regsql.exe. Wenn die Datenbank, die Sie, mit dem SQL-Registrierungstool angeben nicht bereits vorhanden ist (Aspnetdb ist die Standarddatenbank, wenn eine in der Befehlszeile nicht angegeben ist), der aktuelle Benutzer über die Rechte zum Erstellen von Datenbanken in SQL Server sowie zum Erstellen von Schemas e verfügen muss Lements innerhalb einer Datenbank.

### <a name="sql-cache-dependency"></a>SQL-Cacheabhängigkeit

Ein erweitertes Feature der ausgabezwischenspeicherung in ASP.NET handelt es sich um SQL-Cacheabhängigkeit. SQL-Cacheabhängigkeit unterstützt zwei verschiedene Modi des Vorgangs: verwendet eine ASP.NET-Implementierung Tabelle abrufen und einen zweiten Modus, der die Abfragebenachrichtigungsfeatures von SQL Server 2005 verwendet. Tools für die SQL-Registrierung kann so konfigurieren Sie den Tabellenabruf-Betriebsmodus verwendet werden.

### <a name="session-state"></a>Sitzungszustand

Standardmäßig werden Sitzungszustandswerte und Informationen im Arbeitsspeicher innerhalb des Prozesses ASP.NET gespeichert. Alternativ können Sie Daten in einer SQL Server-Datenbank speichern, wo er von mehreren Webservern gemeinsam genutzt werden kann. Wenn die Datenbank, die Sie, für den Sitzungszustand mithilfe der SQL-Registrierungs-Tools angeben nicht bereits vorhanden ist, klicken Sie dann benötigen der aktuelle Benutzer Berechtigungen zum Erstellen von Datenbanken in SQL Server sowie zum Erstellen von Schemaelementen in einer Datenbank. Wenn die Datenbank vorhanden ist, klicken Sie dann benötigen der aktuelle Benutzer Rechte zum Erstellen von Schemaelementen in der vorhandenen Datenbank.

Um die Sitzungszustands-Datenbank auf SQL Server zu installieren, führen Sie das Aspnet\_regsql.exe-Tool, und geben Sie die folgenden Informationen, mit dem Befehl:

- Der Name der SQL Server-Instanz, mit der **' -s'** Option.
- Die Anmeldeinformationen für ein Konto mit Berechtigung zum Erstellen einer Datenbank auf einem Computer mit SQL Server. Verwenden Sie die **-E** Option aus, um den aktuell angemeldeten Benutzer verwenden, oder verwenden Sie die **- U** Option zum Angeben einer Benutzer-ID zusammen mit der **-P** Option aus, um ein Kennwort angeben.
- Die **- Ssadd** Befehlszeilenoption Sitzungsstatusdatenbank hinzufügen.

Standardmäßig können Sie keine der Aspnet\_regsql.exe-Tool, um die Sitzungszustands-Datenbank auf einem Computer mit SQL Server 2005 Express Edition zu installieren.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>Registrierungstool für ASP.NET-Browser - Aspnet\_regbrowsers.exe

In ASP.NET, Version 1.1, enthalten die Datei "Machine.config" einen Abschnitt namens &lt;BrowserCaps&gt;. In diesem Abschnitt enthalten eine Reihe von XML-Einträge, die die Konfigurationen für verschiedene Browser basierend auf einem regulären Ausdruck definiert. Für ASP.NET Version 2.0 ein neues. Browserdatei definiert die Parameter von einem bestimmten Browser mithilfe von XML-Einträgen. Sie können Informationen auf einer neuen Browserregisterkarte hinzufügen, indem er ein neues. Browserdatei in den Ordner am %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers auf Ihrem System.

Da eine Anwendung jedes Mal, wenn sie Browserinformationen erfordert keine .config-Datei liest, können Sie ein neues erstellen. Browserdatei, und führen Aspnet\_regbrowsers.exe die erforderlichen Änderungen auf die Assembly hinzufügen. Dadurch wird den Server auf die neuen Browserinformationen sofort zugreifen, sodass Sie nicht zum Herunterfahren einer Ihrer Anwendungen, um die Informationen zu übernehmen. Eine Anwendung kann über die Browser-Eigenschaft des die aktuelle HTTP-Anforderung auf Browserfunktionen zugreifen.

Die folgenden Optionen sind verfügbar, bei der Ausführung von Aspnet\_regbrowser.exe:

| **Option** | **Beschreibung** |
| --- | --- |
| **-?** | Zeigt das Aspnet\_regbbrowsers.exe Hilfetext im Befehlsfenster angezeigt. |
| **-i** | Die Assembly für die Laufzeit des Browsers erstellt und installiert es im globalen Assemblycache. |
| **-u** | Deinstalliert die Assembly für die Laufzeit des Browsers aus dem globalen Assemblycache. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>Die ASP.NET-Kompilierungstool - Aspnet\_compiler.exe

Die ASP.NET-Kompilierung-Tool kann auf zwei allgemeine Arten verwendet werden: für die Kompilierung des direktes und Kompilierung für die Bereitstellung, in denen ein Zielverzeichnis für die Ausgabe angegeben ist.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilieren einer Anwendung vorhanden](https://msdn.microsoft.com/library/ms229863.aspx)

Die können Kompilieren einer Anwendung vorhanden, d. h. er das Verhalten von mehreren Anfragen an die Anwendung, wodurch die Kompilierung von regulären imitiert. Benutzer einer vorkompilierten Website treten nicht auf eine Verzögerung, die durch das Kompilieren der Seite bei der ersten Anforderung verursacht werden.

Beim Vorkompilieren einer Website eingerichtet, gelten die folgenden Elemente:

- Die Site behält seine Dateien und die Verzeichnisstruktur.
- Sie müssen die Compiler für alle Programmiersprachen, die vom Standort auf dem Server verfügen.
- Wenn eine beliebige Datei Kompilierung fehlschlägt, schlägt die Kompilierung der gesamte Standort fehl.

Sie können auch eine Anwendung direkt nach dem Hinzufügen von neuen Quelldateien, neu kompilieren. Kompiliert das Tool nur die neuen oder geänderten Dateien, es sei denn, Sie enthalten die **- C** Option.

> [!NOTE]
> Kompilierung einer Anwendung, die eine geschachtelte Anwendung enthält wird die geschachtelte Anwendung nicht kompiliert werden. Die geschachtelte Anwendung muss separat kompiliert werden.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilieren einer Anwendung für die Bereitstellung](https://msdn.microsoft.com/library/ms229863.aspx)

Sie kompilieren eine Anwendung für die Bereitstellung (Kompilierung zu einer Zielposition) durch Festlegen des TargetDir-Parameters. TargetDir kann den endgültigen Speicherort für die Webanwendung, oder die kompilierte Anwendung kann weiter bereitgestellt werden. Mithilfe der **-u** Option kompiliert die Anwendung so, dass Sie auf bestimmte Dateien in der kompilierten Anwendung Änderungen vornehmen können, ohne Neukompilieren. ASPNET\_compiler.exe wird unterschieden zwischen statischen und dynamischen Dateitypen, und sie unterschiedlich behandelt, wenn Sie die Anwendung zu erstellen.

- Statischen Dateitypen sind diejenigen, die einen Compiler zugeordnet bzw. keine Buildanbieter, z. B. Dateien, deren benannte haben Sie Erweiterungen wie CSS, GIF, htm, HTML, JPG, js-und so weiter. Diese Dateien werden einfach an den Zielspeicherort an, und ihre relativen Positionen in der Verzeichnisstruktur beibehalten kopiert.
- Dynamische Dateitypen sind diejenigen, die einen Compiler zugeordnet oder Anbieter, einschließlich Dateien mit ASP.NET-spezifische Dateierweiterungen wie asax, ASCX, .ashx, aspx, .browser, Master- und usw. zu erstellen. Das Tool für die ASP.NET-Kompilierung generiert Assemblys aus diesen Dateien. Wenn die **-u** Option ausgelassen wird, wird das Tool erstellt auch Dateien mit der Dateinamenerweiterung. COMPILED, die die ursprünglichen Quelldateien ihre Assembly zuordnen. Um sicherzustellen, dass die Verzeichnisstruktur des Quellcodes der Anwendung beibehalten wird, generiert das Tool die Platzhalterdateien in den entsprechenden Speicherorten in der Zielanwendung.

Verwenden Sie die **-u** Option, um anzugeben, dass der Inhalt der kompilierten Anwendung geändert werden kann. Andernfalls nachfolgende Änderungen werden ignoriert, oder zur Laufzeit Fehler verursachen.

In der folgende Tabelle wird beschrieben, wie die ASP.NET-Kompilierung verschiedene Dateitypen behandelt, wenn die **-u** Option enthalten ist.

| **Dateityp** | **Compiler-Aktion** |
| --- | --- |
| ASCX, aspx, Master | Diese Dateien werden in Markup und Quellcode, darunter sowohl Code-Behind-Dateien und jeglicher Code, der in eingeschlossen ist geteilt &lt;Skript Runat = "Server"&gt; Elemente. Quellcode ist in Assemblys kompiliert, mit dem Namen, die einen Hashalgorithmus abgeleitet sind, und die Assemblys im Bin-Verzeichnis platziert werden. Inlinecode, d.h., Code eingeschlossen, zwischen den **&lt; %** und **% &gt;** Klammern, ist im Markup enthalten und wird nicht kompiliert. Neue Dateien mit dem gleichen Namen wie die Quelldateien werden erstellt, um das Markup enthalten und in den entsprechenden Ausgabeverzeichnisse platziert. |
| ASHX, ASMX | Diese Dateien werden nicht kompiliert und in den Ausgabeverzeichnisse als und unkompiliert verschoben werden. Wenn Sie vom Handlercode kompiliert haben möchten, fügen Sie den Code in Quellcodedateien in der App\_Verzeichnis "Code". |
| cs, vb, jsl, cpp (mit Code-Behind-Dateien für die Dateitypen, die zuvor aufgeführten) | Diese Dateien werden kompiliert und als Ressource in Assemblys, die darauf verweisen enthalten. Quelldateien werden nicht in das Ausgabeverzeichnis kopiert werden. Wenn eine Codedatei nicht verwiesen wird, ist es nicht kompiliert. |
| Benutzerdefinierte Dateitypen | Diese Dateien werden nicht kompiliert. Diese Dateien werden in die entsprechenden Ausgabeverzeichnisse kopiert. |
| Quellcodedateien in der App\_Codeunterverzeichnis | Diese Dateien werden in Assemblys kompiliert und im Verzeichnis "bin" gespeichert. |
| RESX- und RESOURCES-Dateien in der App\_GlobalResources Unterverzeichnis | Diese Dateien werden in Assemblys kompiliert und im Verzeichnis "bin" gespeichert. Keine App\_GlobalResources-Unterverzeichnis im Verzeichnis Hauptausgabe erstellt wird, und keine RESX- oder RESOURCES-Dateien im Quellverzeichnis werden in die Ausgabeverzeichnisse kopiert. |
| RESX- und RESOURCES-Dateien in der App\_"LocalResources" Unterverzeichnis | Diese Dateien werden nicht kompiliert und in die entsprechende Ausgabeverzeichnisse kopiert werden. |
| Skin-Dateien in der App\_Designs Unterverzeichnis | Die Skin-Dateien und statische Dateien werden nicht kompiliert, und in die entsprechende Ausgabeverzeichnisse kopiert werden. |
| .Browser-Datei "Web.config" statischen Typen von Assemblys im Bin-Verzeichnis bereits vorhanden. | Diese Dateien werden kopiert, in den Ausgabeverzeichnisse. |

In der folgende Tabelle wird beschrieben, wie die ASP.NET-Kompilierung verschiedene Dateitypen behandelt, wenn die **-u** Option ausgelassen wird.

| **Dateityp** | **Compiler-Aktion** |
| --- | --- |
| .aspx, .asmx, .ashx, Master | Diese Dateien werden in Markup und Quellcode, darunter sowohl Code-Behind-Dateien und jeglicher Code, der in eingeschlossen ist geteilt &lt;Skript Runat = "Server"&gt; Elemente. Quellcode wird in Assemblys, deren Namen kompiliert, die von einem Hashalgorithmus abgeleitet sind. Die erstellten Assemblys werden in das Bin-Verzeichnis platziert. Inlinecode, d.h., Code eingeschlossen, zwischen den **&lt; %** und **% &gt;** Klammern, ist im Markup enthalten und wird nicht kompiliert. Der Compiler erstellt neue Dateien, um das Markup mit dem gleichen Namen wie die Quelldateien enthalten. Diese resultierenden Dateien werden in das Bin-Verzeichnis platziert. Der Compiler erstellt auch Dateien mit dem gleichen Namen wie die Quelldateien, aber mit der Erweiterung. COMPILED, die Informationen über die Zuordnung enthalten. Die. KOMPILIERTE Dateien werden in den Ausgabeverzeichnisse, die für am ursprünglichen Speicherort der Quelldateien platziert. |
| ASCX | Diese Dateien werden in Markup und Quellcode aufgeteilt. Quellcode ist in Assemblys kompiliert und platziert in das Bin-Verzeichnis mit Namen, die von einem Hashalgorithmus abgeleitet sind. Kein Markup-Dateien werden generiert. |
| cs, vb, jsl, cpp (mit Code-Behind-Dateien für die Dateitypen, die zuvor aufgeführten) | Quellcode, die ASCX, .ashx oder ASPX-Dateien generierten Assemblys verwiesen wird, wird in Assemblys kompiliert und im Verzeichnis "bin" gespeichert. Es wurden keine Quelldateien werden kopiert. |
| Benutzerdefinierte Dateitypen | Diese Dateien werden wie dynamische-Dateien kompiliert. Je nach Art der Datei, auf denen sie basieren, kann der Compiler die Mapping-Dateien in den Ausgabeverzeichnisse platzieren. |
| Dateien in der App\_Codeunterverzeichnis | Quellcodedateien in dieses Unterverzeichnis werden in Assemblys kompiliert und im Verzeichnis "bin" gespeichert. |
| Dateien in der App\_GlobalResources Unterverzeichnis | Diese Dateien werden in Assemblys kompiliert und im Verzeichnis "bin" gespeichert. Keine App\_GlobalResources-Unterverzeichnis im Verzeichnis Hauptausgabe erstellt wird. Wenn die Konfigurationsdatei AppliesTo gibt = "All" RESX- und RESOURCES-Dateien werden in den Ausgabeverzeichnisse kopiert. Sie werden nicht kopiert, wenn sie von referenziert werden eine [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| RESX- und RESOURCES-Dateien in der App\_"LocalResources" Unterverzeichnis | Diese Dateien werden in Assemblys mit eindeutigen Namen kompiliert und im Verzeichnis "bin" gespeichert. Keine RESX- oder RESOURCES-Dateien werden in den Ausgabeverzeichnisse kopiert. |
| Skin-Dateien in der App\_Designs Unterverzeichnis | Designs sind in Assemblys kompiliert und im Verzeichnis "bin" gespeichert. Stub-Dateien für die Skin-Dateien erstellt und in das entsprechende Verzeichnis platziert. Statische Dateien (z. B. CSS) werden in den Ausgabeverzeichnisse kopiert. |
| .Browser-Datei "Web.config" statischen Typen von Assemblys im Bin-Verzeichnis bereits vorhanden. | Diese Dateien werden kopiert, da in das Ausgabeverzeichnis ist. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Festgelegten Assemblynamen](https://msdn.microsoft.com/library/ms229863.aspx##)

Einige Szenarien, z. B. das Bereitstellen einer Webanwendung mithilfe der Windows Installer MSI-Datei erfordern die Verwendung von konsistenten Dateinamen und Inhalte sowie konsistente Verzeichnisstrukturen zum Identifizieren von Assemblys oder von Konfigurationseinstellungen für Updates. In diesen Fällen können Sie die **- Fixednames** Option aus, um anzugeben, dass die ASP.NET eine Assembly kompilieren soll für jede Quelldatei anstelle von Where mit mehreren Seiten in Assemblys kompiliert werden. Dies kann für eine große Anzahl von Assemblys führen, also wenn Sie sich mit der Skalierbarkeit sollten Sie diese Option mit Vorsicht verwenden.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Kompilierung mit starkem Namen](https://msdn.microsoft.com/library/ms229863.aspx##)

Die **- Aptca**, **- Delaysign**, **- Keycontainer** und **- Keyfile** Optionen werden bereitgestellt, sodass Sie, Aspnet verwenden können\_ Compiler.exe stark erstellen mit dem Namen Assemblys ohne Verwendung der [Strong Name-Tool (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) getrennt. Diese Optionen entsprechen jeweils zu **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**, und  **AssemblyKeyFileAttribute**.

Die Beschreibung dieser Attribute ist außerhalb des Bereichs dieses Kurses.

## <a name="labs"></a>Labs

Jede die folgenden Übungen baut auf den vorherigen Übungen. Sie müssen sie in der Reihenfolge ausgeführt werden.

## <a name="lab-1-using-the-configuration-api"></a>Übung 1: Verwendung der Konfigurations-API

1. Erstellen Sie eine neue Website namens *mod9lab*.
2. Fügen Sie eine neue Web-Konfigurationsdatei mit dem Standort hinzu.
3. Fügen Sie der Datei "Web.config" Folgendes ein:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Dadurch wird sichergestellt, dass Sie berechtigt, die Datei "Web.config" zu speichern.

1. Fügen Sie ein neues Label-Steuerelement an "default.aspx" und ändern Sie die ID, die **LblDebugStatus**.
2. Fügen Sie eine neue Schaltflächen-Steuerelement an "default.aspx" hinzu.
3. Ändern Sie das Schaltflächen-Steuerelement-ID, **BtnToggleDebug** und der Text, der **Umschaltstatus Debuggen**.
4. Öffnen Sie die Codeansicht für die Code-Behind-Datei "default.aspx", und fügen eine **mit** -Anweisung für **System.Web.Configuration** wie folgt:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Fügen Sie zwei private Variablen auf die Klasse und eine Seite\_Init-Methode, wie unten dargestellt:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Fügen Sie den folgenden Code zur Seite\_laden:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Speichern Sie und suchen Sie "default.aspx". Beachten Sie, dass das Label-Steuerelement den aktuellen Debugstatus angezeigt.
2. Doppelklicken Sie auf das Schaltflächen-Steuerelement im Designer, und fügen Sie den folgenden Code zum Click-Ereignis für das Schaltflächen-Steuerelement:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Speichern Sie und Durchsuchen Sie "default.aspx", und klicken Sie auf die Schaltfläche.
2. Öffnen Sie die Datei "Web.config" aus, nachdem jede Schaltfläche auf, und sehen die **Debuggen** -Attribut in der &lt;Kompilierung&gt; Abschnitt.

## <a name="lab-2-logging-application-restarts"></a>Übung 2: Protokollierung Neustarts der Anwendung

In dieser Übungseinheit erstellen Sie Code, der Sie die Protokollierung von Anwendung herunterfahren, Startups und neukompilierungen in der Ereignisanzeige umschalten können.

1. Fügen Sie einem DropDownList-Steuerelement an "default.aspx" ein, und ändern Sie die ID in DdlLogAppEvents.
2. Legen Sie die **AutoPostBack** -Eigenschaft für das DropDownList, **"true"**.
3. Die Items-Auflistung, für die DropDownList drei Elemente hinzufügen. Stellen die **Text** für das erste Element *Select Value* und den Wert-1. Stellen Sie die **Text** und **Wert** des zweiten Elements **"true"** und **Text** und **Wert** des dritten Elements **"False"**.
4. Fügen Sie eine neue Bezeichnung an "default.aspx" hinzu. Ändern Sie die ID, **LblLogAppEvents**.
5. Öffnen Sie die CodeBehind-Ansicht für "default.aspx", und fügen Sie eine neue Deklaration für eine Variable vom Typ HealthMonitoringSection, wie unten dargestellt:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Fügen Sie den folgenden Code am vorhandenen Code auf\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Doppelklicken Sie auf der Dropdownliste aus, und fügen Sie den folgenden Code, um das SelectedIndexChanged-Ereignis:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Durchsuchen Sie "default.aspx".
2. Legen Sie auf die Dropdownliste **"false"**.
3. Deaktivieren Sie das Anwendungsprotokoll in der Ereignisanzeige.
4. Klicken Sie auf die Schaltfläche, um das Debug-Attribut für die Anwendung zu ändern.
5. Aktualisieren Sie das Anwendungsprotokoll in der Ereignisanzeige. 

    1. Wurden keine Ereignisse werden protokolliert?
    2. Warum oder warum nicht?
6. Legen Sie auf die Dropdownliste **"true".**
7. Klicken Sie auf die Schaltfläche, um das Debug-Attribut für die Anwendung zu wechseln.
8. Aktualisieren Sie die Anmeldung der Anwendung der Ereignisanzeige. 

    1. Wurden keine Ereignisse werden protokolliert?
    2. Was war der Grund für das Herunterfahren der app?
9. Experimentieren Sie mit der aktiviert oder deaktiviert die Protokollierung, und sehen Sie sich die Änderungen an der Datei "Web.config".

## <a name="more-information"></a>Weitere Informationen:

ASP.NET 2.0-Anbieter-Modells können Sie Ihre eigenen Anbieter für nicht nur für anwendungsinstrumentierung, aber für viele weitere Verwendungsmöglichkeiten sowie z. B. Mitgliedschaft, Profile usw. zu erstellen. Ausführliche Informationen zum Schreiben eines benutzerdefinierten Anbieters zum Protokollieren von Anwendungsereignissen in eine Textdatei, finden Sie unter [diesen Link](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
