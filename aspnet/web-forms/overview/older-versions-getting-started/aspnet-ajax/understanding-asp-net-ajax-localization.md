---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Grundlegendes zur Lokalisierung in ASP.NET AJAX | Microsoft-Dokumentation
author: scottcate
description: Lokalisierung ist der Prozess für das Entwerfen und Unterstützung für eine bestimmte Sprache und Kultur in einer Anwendung oder eine Anwendungskomponente integrieren. Der Mic...
ms.author: aspnetcontent
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: ce6404ce4faa1018a4f8118f6167a4f93956abd3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815004"
---
<a name="understanding-aspnet-ajax-localization"></a>Grundlegendes zur Lokalisierung in ASP.NET AJAX
====================
durch [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Lokalisierung ist der Prozess für das Entwerfen und Unterstützung für eine bestimmte Sprache und Kultur in einer Anwendung oder eine Anwendungskomponente integrieren. Die Plattform von Microsoft ASP.NET bietet umfangreiche Unterstützung für Lokalisierung für ASP.NET-Anwendungen standard, durch die Integration von den standardmäßigen .NET Lokalisierungsmodell; Das Microsoft AJAX-Framework nutzen, die integrierte Modell zur Unterstützung der unterschiedlichen Szenarien, in der Lokalisierung ausgeführt werden kann.


## <a name="introduction"></a>Einführung

Die Technologie von Microsoft ASP.NET bietet ein Programmiermodell, das objektorientierte und ereignisgesteuerte und vereinen sie die Vorteile des kompilierten Codes. Hat jedoch einige Nachteile, die sich von der Technologie, von die viele adressiert werden können, indem die neuen Features im Namespace "System.Web.Extensions", der die Microsoft AJAX-Dienste in .NET Framework kapselt das Modell für die serverseitige Verarbeitung 3.5. Diese Erweiterungen ermöglichen viele rich-Client-Features, zuvor als Teil von ASP.NET 2.0 AJAX Extensions, aber jetzt Teil der Framework-Basisklassenbibliothek verfügbar. Steuerelemente und Features in diesem Namespace Teilrendering von Seiten enthalten, ohne dass von einer vollständigen seitenaktualisierung, den Zugriff auf Webdienste über Clientskript (einschließlich ASP.NET profilerstellungs-API) und viele der Spiegelung für die eine umfangreiche clientseitige API die Steuerelement-Schemas in der Menge der ASP.NET serverseitiges Steuerelement angezeigt.

In diesem Whitepaper untersucht die Lokalisierungsfunktionen, die in der Microsoft AJAX-Framework und das Microsoft AJAX-Skriptbibliothek, im Kontext der geschäftsanforderungen für die Unterstützung der Lokalisierung und Überprüfen von bereits integrierten Unterstützung für die Lokalisierung im Web vorhanden Anwendungen, die von .NET Framework bereitgestellt werden. Die Microsoft AJAX-Skriptbibliothek nutzt die RESX-Dateiformat bereits von Anwendungen für .NET, verwendet die integrierte IDE-Unterstützung und einen freigegebenen Ressourcentyp bereitstellt.

In diesem Whitepaper basiert in der Beta 2-Version von Microsoft Visual Studio 2008. In diesem Whitepaper wird vorausgesetzt, dass Sie mit Visual Studio 2008 nicht Visual Web Developer Express arbeiten, und exemplarische Vorgehensweisen, gemäß der Benutzeroberfläche von Visual Studio stellt. Einige Codebeispiele werden in Projektvorlagen verwendet, die in Visual Web Developer Express nicht verfügbar sein können.

## <a name="the-need-for-localization"></a>*Die Notwendigkeit für die Lokalisierung*

Insbesondere für Darstellung des Entwicklern von unternehmensanwendungen und Komponentenentwickler ist die Fähigkeit zum Erstellen von Tools, die über die Unterschiede zwischen Kulturen und Sprachen können zunehmend erforderlich geworden. Entwerfen von Komponenten mit der Möglichkeit zur Anpassung an das Gebietsschema des Clients erhöht die Produktivität von Entwicklern und verringert die Menge der Arbeitsaufwand für die Anpassung einer Komponente Global funktionieren.

Lokalisierung ist der Prozess für das Entwerfen und Unterstützung für eine bestimmte Sprache und Kultur in einer Anwendung oder eine Anwendungskomponente integrieren. Die Plattform von Microsoft ASP.NET bietet umfangreiche Unterstützung für Lokalisierung für ASP.NET-Anwendungen standard, durch die Integration von den standardmäßigen .NET Lokalisierungsmodell; Das Microsoft AJAX-Framework nutzen, die integrierte Modell zur Unterstützung der unterschiedlichen Szenarien, in der Lokalisierung ausgeführt werden kann. Mit dem Microsoft-AJAX-Framework können Skripts entweder in Satellitenassemblys bereitgestellt wird oder durch die Verwendung einer statischen Struktur des Dateisystems lokalisiert werden.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Einbetten von Skripts mit Satellitenassemblys*

Mit der standardmäßigen .NET Framework-Lokalisierungsstrategie konsistent ist, können Ressourcen in Satellitenassemblys enthalten sein. Satellitenassemblys bieten verschiedene Vorteile über die Tatsache herkömmlichen Binärdateien - kann keine bestimmten Lokalisierung ohne Aktualisierung des größeren Bilds aktualisiert werden, können zusätzliche lokalisierungen bereitgestellt werden, einfach durch die Installation von Satellitenassemblys in Der Projektordner, und Satellitenassemblys können bereitgestellt werden, ohne dass ein erneutes Laden der Assembly Hauptprojekt. Insbesondere in ASP.NET-Projekten Dies ist nützlich, da die Menge an Systemressourcen verwendet, die für inkrementelle Updates deutlich reduziert werden kann und minimal unterbricht die Nutzung der Produktions-Website.

Skripts werden in Assemblys eingebettet, dazu können Sie sie in verwalteten RESX-Dateien, die in die Assembly zum Zeitpunkt der Kompilierung enthalten sind (oder kompilierten Resources). Ihre Ressourcen werden anschließend skriptanwendung durch AJAX zur Laufzeit generierte Code über Attribute auf Assemblyebene zur Verfügung gestellt.

*Benennungskonventionen für Dateien mit eingebetteten Skripts*

Die Microsoft AJAX-Framework-Skript-Verwaltung unterstützt eine Vielzahl von Optionen für die Verwendung in der Bereitstellung und Testen von Skripts und Richtlinien werden bereitgestellt, um diese Optionen zu ermöglichen.

*Um das Debuggen zu erleichtern:*

Versionsskripts (Produktion) darf nicht enthalten. die `.debug` Qualifizierer im Dateinamen. Skripts für das Debuggen von aufzunehmen `.debug` im Dateinamen.

*Um die Lokalisierung zu vereinfachen:*

Neutralen Kultur Skripts sollten keine Kulturbezeichner den Namen der Datei enthalten. Für Skripts, die lokalisierte Ressourcen enthalten, sollte der ISO-Sprachcode im Dateinamen angegeben werden. Z. B. `es-CO` steht für Spanisch – Kolumbien.

Die folgende Tabelle enthält den Dateinamenskonventionen anhand von Beispielen:

| Dateiname | Bedeutung |
| --- | --- |
| Script.js | Ein Release-Version kulturneutralen-Skript. |
| Script.debug.js | Ein Debug-Version kulturneutralen-Skript. |
| Script.en-US.js | Version Englisch, USA Skript der Version. |
| Script.debug.es-CO.js | Ein Spanisch Columbia-Skript für Debug-Version. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Exemplarische Vorgehensweise: Erstellen eines lokalisierten, eingebetteten Skripts

*Bitte beachten Sie: Diese exemplarische Vorgehensweise erfordert die Verwendung von Visual Studio 2008, Visual Web Developer Express eine Projektvorlage für Klassenbibliotheksprojekte keine.*

1. Erstellen Sie ein neues Projekt für die Website mit ASP.NET AJAX Extensions integriert. Erstellen Sie ein anderes Projekt, ein Klassenbibliotheksprojekt, in der Projektmappe LocalizingResources aufgerufen.
2. Fügen Sie eine Jscript-Datei VerifyDeletion.js dem LocalizingResources-Projekt namens sowie RESX-Ressourcendateien DeletionResources.resx und DeletionResources.es.resx aufgerufen. Die erste enthält kulturneutralen Ressourcen. die zweite enthält spanischen Ressourcen.
3. Fügen Sie den folgenden Code hinzu, um VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Für diejenigen, die mit JavaScript-Regex-Syntax, Text in einzelne Schrägstriche nicht vertraut sind (im vorherigen Beispiel /FILENAME/ ist ein Beispiel) gibt eine RegExp-Objekt. Der MSDN Library enthält eine umfassende JavaScript-Referenz und Ressourcen in systemeigenen JavaScript-Objekten finden Sie online.

1. Fügen Sie die folgenden Ressourcenzeichenfolgen DeletionResources.resx hinzu: 

    **VerifyDelete**: Opravdu chcete odstranit FILENAME?

    **Gelöschte**: Dateiname wurde gelöscht.

1. Fügen Sie die folgenden Ressourcenzeichenfolgen DeletionResources.es.resx hinzu: 

    **VerifyDelete**: Est Seguro Que desee Quitar FILENAME?

    **Gelöschte**: FILENAME Se ha Quitado.
2. Fügen Sie in der AssemblyInfo-Datei die folgenden Codezeilen hinzu:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Fügen Sie Verweise auf "System.Web" und "System.Web.Extensions", um das Projekt LocalizingResources.
2. Fügen Sie einen Verweis auf das Projekt LocalizingResources aus dem Website-Projekt hinzu.
3. Aktualisieren Sie in "default.aspx", unter dem Website-Projekt das ScriptManager-Steuerelement mit dem folgenden zusätzliche Markup:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Schließen Sie dieses Markup, in eine beliebige Stelle auf der Seite "default.aspx":

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Drücken Sie F5. Wenn Sie dazu aufgefordert werden, aktivieren Sie debugging. Wenn die Seite geladen wird, drücken Sie die Schaltfläche "löschen" aus. Beachten Sie, dass Sie nur auf Englisch verfügbar sind (sofern es sich bei Ihrem Computer bevorzugen spanischen Ressourcen wird standardmäßig festgelegt ist) zur Bestätigung aufgefordert.
2. Das Browserfenster schließen und zurück an "default.aspx". In der @Page Header-Direktive, ersetzen Sie dies Auto für Kultur "und" UICulture "mit" es-ES. Drücken Sie erneut F5, um die Webanwendung im Browser erneut zu starten. Beachten Sie diesmal, dass Sie aufgefordert werden, die Datei in Spanisch löschen:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-localization/_static/image6.png))


Beachten Sie, dass es mehrere Variationen für diese exemplarische Vorgehensweise gibt. Beispielsweise können Skripts beim ScriptManager-Steuerelement programmgesteuert beim Laden der Seite registriert werden.

## <a name="including-a-static-script-file-structure"></a>*Einschließlich einer statischen Datei-Struktur*

Wenn Sie statischen Skriptdateien für die Bereitstellung zu verwenden, verlieren Sie zu den Vorteilen der Verwendung der inhärenten .NET Lokalisierung Schemas. In erster Linie sichtbar ist, dass Sie die automatische, einschließlich Ressourcendateien Skript generierten Typ verlieren; in der obigen exemplarischen Vorgehensweise wurden z. B. Ressourcen durch einen automatisch generierten Typ, der Nachricht vom ScriptManager-Steuerelement namens verfügbar gemacht.

Es gibt jedoch einige Vorteile der Verwendung einer statischen Datei-Struktur. Updates ausgeführt werden können, ohne dass erneut kompiliert und Satellitenassemblys ein, und die Verwendung einer statischen Datei-Struktur kann auch ausgeführt werden, um das Überschreiben von eingebetteten Skripts, um einen kleinen Teil der Funktionalität, die möglicherweise nicht versendet wurden mit einer Komponente zu integrieren.

Microsoft empfiehlt, vermeiden ein Problem mit Steuerelement mittels automatischer Generierung von Ressourcen für Ihre Skripts während der Projektkompilierung. Wenn eine umfangreiche Skriptcode Basis verwaltet, kann es werden zunehmend schwieriger, stellen Sie sicher, dass Änderungen am Code in jedem lokalisierte Skript wiedergegeben werden. Als Alternative können Sie einfach eine Logik und Skripts für mehrere Lokalisierung, warten Zusammenführen der Dateien beim Erstellen des Projekts.

Da keine Ressourcen deklarativ einschließen, statische Skripts, die Dateien muss auf die verwiesen wird entweder durch Hinzufügen von `<asp:ScriptElement>` Elemente als untergeordnetes Element der `<Scripts>` Tag des ScriptManager-Steuerelements, oder indem Sie programmgesteuert `ScriptReference` Objekte um die `Scripts` Eigenschaft der `ScriptManager` Steuerelement auf der Seite zur Laufzeit.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager und seiner Rolle in der Lokalisierung*

ScriptManager aktiviert mehrere automatische Verhalten bei lokalisierten Anwendungen:

- Es sucht automatisch nach Skriptdateien, die basierend auf Einstellungen und Benennungskonventionen zur Verfügung; Klicken Sie z. B. Debug-aktivierte Skripts im Debugmodus geladen, und lädt lokalisiert Skripts auf Grundlage des Browsers Benutzerauswahl-Schnittstelle.
- Dadurch wird die Definition von Kulturen, einschließlich Kulturen.
- Sie können die Komprimierung von Skriptdateien über HTTP.
- Es speichert Skripts aus, um viele Anforderungen effizient zu verwalten.
- Sie können Skripts eine Schicht der Dereferenzierung hinzugefügt, durch das sie über eine verschlüsselte URL weiterleiten.

Skriptverweise können entweder programmgesteuert oder durch deklaratives Markup an das ScriptManager-Steuerelement hinzugefügt werden. Deklaratives Markup ist besonders nützlich, bei der Arbeit mit Skripts in eingebetteten Assemblys als das Websiteprojekt selbst, wie der Name des Skripts wahrscheinlich nicht ändern wird, wie Änderungen per Push, durch übertragen werden.

## <a name="summary"></a>Zusammenfassung

Webanwendungen wachsen, um eine größere Zielgruppe zu erreichen, wird muss breitere Kulturen und Communitys erreichen können Core ein Geschäftsmodell; e-Commerce-Webanwendungen für den Umgang mit Fremdschlüssel Währungen möglich sein muss, Content Management-Systemen benötigen, können nicht nur vorhanden sein, deren Inhalt aber auch ihre Navigation-Hinweise und Formularfelder in andere Sprachen und Unternehmen müssen wissen, dass diese Anforderung ist zugegriffen werden kann.

.NET Framework unterstützt systemintern ein Framework, das umfangreiche Lokalisierung mithilfe von Satellitenassemblys und XML-Ressourcendateien (.resx), um eine einheitliche Methode zur Suche nach Ressourcen-Strings und Bilder zu präsentieren. Die ASP.NET AJAX-Erweiterungen, einschließlich der Microsoft AJAX-Framework und das Microsoft AJAX-Skriptbibliothek, bieten Unterstützung für dieses Programmiermodell in der clientseitigen Code einfach Zeichenfolge Ressourcensuchen aktivieren. Satellitenassemblys unterstützt die automatische Einbindung von Skriptressourcen (tatsächliche JS-Dateien) über ScriptResource.axd, solange die Dateinamen einem bestimmten Benennungsschema folgen. Vereinfachen Sie ASP.NET AJAX Extensions mit der entsprechenden Unterstützung der Lokalisierung von Skripts und die Globalisierung von Anwendungen.

## <a name="bio"></a>*Biografie*

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und ist Vorsitzender der myKB.com ([www.myKB.com](http://www.myKB.com)), in dem er spezialisiert sich auf das Schreiben von ASP.NET basierende Anwendungen, die mit dem Schwerpunkt Knowledge Base-softwarelösungen. Scott hergestellt werden kann, per e-Mail unter [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Zurück](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Weiter](understanding-asp-net-ajax-web-services.md)
