---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Grundlegendes zu ASP.NET AJAX-Lokalisierung | Microsoft Docs
author: scottcate
description: Lokalisierung ist der Prozess beim Entwerfen und Integrieren von Unterstützung für eine bestimmte Sprache und Kultur in eine Anwendung oder eine Anwendungskomponente. Der Mic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 565b0294f57b784bc592b286b3d8b28504110415
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-localization"></a>Grundlegendes zu ASP.NET AJAX-Lokalisierung
====================
durch [Scott Kate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Lokalisierung ist der Prozess beim Entwerfen und Integrieren von Unterstützung für eine bestimmte Sprache und Kultur in eine Anwendung oder eine Anwendungskomponente. Die Microsoft ASP.NET-Plattform bietet umfangreiche Unterstützung für die Lokalisierung für standard ASP.NET-Anwendungen durch die Integration des standardmäßigen .NET Lokalisierungsmodells; Das Microsoft AJAX-Framework nutzen, die integrierte Modell zur Unterstützung der verschiedenen Szenarien, die in der Lokalisierung ausgeführt werden kann.


## <a name="introduction"></a>Einführung

Microsoft Technologie für ASP.NET bietet ein Programmiermodell objektorientierte und ereignisgesteuerte und ihn mit den Vorteilen des kompilierten Codes vereinigt. Die serverseitige Verarbeitungsmodell hat jedoch mehrere Nachteile in die Technologie, von die viele adressiert werden können, indem die neuen Features im System.Web.Extensions-Namespace, der das Microsoft AJAX-Diensten in .NET Framework kapselt inhärenten 3.5. Diese Erweiterungen ermöglichen viele rich-Client-Funktionen, zuvor als Teil von ASP.NET 2.0 AJAX-Erweiterungen, aber jetzt Teil der Framework-Basisklassenbibliothek verfügbar. Steuerelemente und Features in diesem Namespace Teilrendering von Seiten enthalten, ohne dass eine vollständige Seite-Aktualisierung, den Zugriff auf Webdienste über Clientskripts (einschließlich ASP.NET profilerstellungs-API) und eine umfangreiche clientseitige API, die auf viele spiegeln ausgelegt. die Steuerelement-Schemas in der Menge der ASP.NET serverseitiges Steuerelement angezeigt.

In diesem Whitepaper untersucht die Lokalisierungsfunktionen, die in der Microsoft AJAX-Framework und Microsoft AJAX-Skript-Bibliothek im Kontext der Anforderungen Ihres Unternehmens lokalisierungsunterstützung und Überprüfung der bereits integrierte Unterstützung für die Lokalisierung in Web vorhanden Anwendungen, die von .NET Framework bereitgestellt werden. Der Microsoft AJAX-Skript-Bibliothek nutzt das RESX-Dateiformat bereits von .NET Applications verwendet die integrierte Unterstützung von IDE und ein freigegebenes Ressourcentyp bereitstellt.

In diesem Whitepaper basiert auf der Beta 2-Version von Microsoft Visual Studio 2008. In diesem Whitepaper wird davon ausgegangen, dass Sie mit Visual Studio 2008 nicht Visual Web Developer Express arbeiten und gebe Exemplarische Vorgehensweisen gemäß der Benutzeroberfläche von Visual Studio. Einige Codebeispiele werden Projektvorlagen nutzen, die möglicherweise in Visual Web Developer Express nicht verfügbar.

## <a name="the-need-for-localization"></a>*Die Notwendigkeit für die Lokalisierung*

Insbesondere für die Anwendung Unternehmensentwickler und Komponentenentwickler geworden zunehmend erforderlichen die Möglichkeit, die Tools zu erstellen, die Beachten Sie die Unterschiede zwischen Kulturen und Sprachen verwendet werden können. Entwerfen von Komponenten die Möglichkeit bietet, passen Sie das Gebietsschema des Clients erhöht die Produktivität der Entwickler und reduziert den Aufwand für die Anpassung einer Komponente für Global funktionieren.

Lokalisierung ist der Prozess beim Entwerfen und Integrieren von Unterstützung für eine bestimmte Sprache und Kultur in eine Anwendung oder eine Anwendungskomponente. Die Microsoft ASP.NET-Plattform bietet umfangreiche Unterstützung für die Lokalisierung für standard ASP.NET-Anwendungen durch die Integration des standardmäßigen .NET Lokalisierungsmodells; Das Microsoft AJAX-Framework nutzen, die integrierte Modell zur Unterstützung der verschiedenen Szenarien, die in der Lokalisierung ausgeführt werden kann. Mit dem Microsoft AJAX-Framework können Skripts entweder in Satellitenassemblys bereitgestellt wird, oder eine statische Dateisystemstruktur geschützte lokalisiert werden.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Einbetten von Skripts mit Satellitenassemblys*

Mit den standardmäßigen .NET Framework Lokalisierungsstrategie konsistent sind, können Ressourcen in Satellitenassemblys enthalten sein. Satellitenassemblys bieten mehrere Vorteile über herkömmlichen Aufnahme in Binärdateien - kann keine bestimmten Lokalisierung ohne Aktualisierung der größeren Bilds aktualisiert werden, zusätzliche lokalisierten Versionen können einfach durch die Installation von Satellitenassemblys in bereitgestellt werden Der Projektordner, und die Satellitenassemblys können bereitgestellt werden, ohne dass ein erneutes Laden der Assembly Hauptprojekt. Insbesondere in ASP.NET-Projekten Dies ist nützlich, da die Menge an Systemressourcen geschont, die für inkrementelle Updates erheblich reduziert werden kann und minimal unterbricht die Nutzung der Produktions-Website.

Skripts werden in Assemblys eingebettet, dazu können Sie sie in verwalteten RESX-Dateien, die in die Assembly zum Zeitpunkt der Kompilierung enthalten sind (oder kompilierten Resources). Ihre Ressourcen werden dann an die skriptanwendung mithilfe von AJAX Laufzeit generierte Code über auf Assemblyebene Attribute zur Verfügung gestellt.

*Benennungskonventionen für Dateien mit eingebetteten Skripts*

Das Microsoft AJAX-Framework skriptverwaltung unterstützt eine Vielzahl von Optionen für die Verwendung in der Bereitstellung und Testen von Skripts und Richtlinien werden bereitgestellt, um diese Optionen zu ermöglichen.

*Um das Debuggen zu erleichtern:*

(Produktion) Versionsskripts sollten nicht enthalten. der `.debug` Qualifizierer im Dateinamen verwenden. Skripts für Debuggen einschließen soll `.debug` im Dateinamen verwenden.

*Um die Lokalisierung zu ermöglichen:*

Neutrale Kultur Skripts sollten keine Kulturbezeichner den Namen der Datei enthalten. Für Skripts, die lokalisierte Ressourcen enthalten, sollte der ISO-Sprachcode im Dateinamen angegeben werden. Z. B. `es-CO` für Spanisch Columbia steht.

Die folgende Tabelle fasst den Dateinamenskonventionen mit Beispielen:

| Dateiname | Bedeutung |
| --- | --- |
| Script.js | Ein Release-Version kulturneutrale-Skript. |
| Script.debug.js | Skript kulturneutrale der Debug-Version. |
| Script.en-US.js | Version Englisch, USA Skript der Version. |
| Script.debug.es-CO.js | Debug-Spanisch, Columbia Skript der Version. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Exemplarische Vorgehensweise: Erstellen eines lokalisierten, eingebetteten Skripts

*Bitte beachten Sie: in dieser exemplarischen Vorgehensweise erfordert die Verwendung von Visual Studio 2008 an, wie eine Projektvorlage für Klassenbibliothekprojekte Visual Web Developer Express nicht enthalten ist.*

1. Erstellen Sie ein neues Projekt für die Website mit ASP.NET AJAX-Erweiterungen integriert. Erstellen Sie ein anderes Projekt, ein Klassenbibliotheksprojekt, in der Projektmappe LocalizingResources aufgerufen.
2. Fügen Sie eine VerifyDeletion.js aufgerufen, um das Projekt LocalizingResources Jscript-Datei als auch von RESX-Ressourcendateien DeletionResources.resx und DeletionResources.es.resx aufgerufen. Die erste enthält kulturneutralen Ressourcen. der zweite Wert enthält spanische Ressourcen.
3. Fügen Sie den folgenden Code, um VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Für diejenigen, die mit JavaScript Regex-Syntax, Text in einzelne Schrägstriche nicht vertraut (im vorherigen Beispiel /FILENAME/ ist ein Beispiel für) steht für ein RegExp-Objekt. Der MSDN Library enthält einen umfangreichen JavaScript-Verweis, und Ressourcen in systemeigenen Objekten JavaScript online gefunden werden.

1. Fügen Sie die folgenden Ressourcenzeichenfolgen DeletionResources.resx hinzu: 

    **VerifyDelete**: sind Sie sicher, dass Sie FILENAME löschen möchten?

    **Gelöschte**: Dateiname wurde gelöscht.

1. Fügen Sie die folgenden Ressourcenzeichenfolgen DeletionResources.es.resx hinzu: 

    **VerifyDelete**: Est Seguro Que desee Quitar FILENAME?

    **Gelöschte**: FILENAME Se ha Quitado.
2. Fügen Sie der AssemblyInfo-Datei die folgenden Codezeilen hinzu:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Fügen Sie Verweise auf System.Web und System.Web.Extensions LocalizingResources-Projekt.
2. Fügen Sie einen Verweis auf das Projekt LocalizingResources aus dem Website-Projekt hinzu.
3. Aktualisieren Sie in "default.aspx" unter der Website-Projekt das ScriptManager-Steuerelement durch die folgende zusätzliche Markup:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Schließen Sie in eine beliebige Stelle auf der Seite "default.aspx" dieses Markup:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Drücken Sie F5. Wenn Sie dazu aufgefordert werden, aktivieren Sie das Debuggen. Wenn die Seite geladen ist, drücken Sie die Schaltfläche "löschen" aus. Beachten Sie, dass Sie auf Englisch aufgefordert werden (es sei denn, Computerdateien Spanisch-Sprachressourcen bevorzugt standardmäßig festgelegt ist) nach einer Bestätigung.
2. Schließen Sie das Browserfenster, und kehren Sie zu "default.aspx" zurück. In der @Page Header-Direktive, Replace Auto für Culture und UICulture "mit" es-ES. Drücken Sie erneut F5, um die Webanwendung im Browser erneut zu starten. Diesmal, beachten Sie, dass Sie aufgefordert werden, die Datei in Spanisch löschen:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-localization/_static/image6.png))


Beachten Sie, dass es mehrere Varianten für diese exemplarische Vorgehensweise gibt. Beispielsweise konnte Skripts mit dem ScriptManager-Steuerelement programmgesteuert beim Laden der Seite registriert werden.

## <a name="including-a-static-script-file-structure"></a>*Einschließlich der Dateistruktur einer statischen Skript*

Wenn Sie statische Skriptdateien für die Bereitstellung zu verwenden, verlieren Sie zu den Vorteilen der Verwendung der inhärenten .NET Lokalisierung Schemas. In erster Linie angezeigt wird, Sie den automatischen Typ verlieren von Skriptdateien für die Ressource einschließlich generiert; in der oben aufgeführten exemplarischen Vorgehensweise wurden beispielsweise Ressourcen von einer automatisch generierten Typ, der Nachricht über das ScriptManager-Steuerelement aufgerufen zur Verfügung gestellt.

Es gibt jedoch einige Vorteile der Verwendung einer statischen Skript-Datei-Struktur. Updates können ausgeführt werden, ohne erneute Kompilierung und Bereitstellung von Satellitenassemblys, und die Verwendung einer statischen Datei-Struktur kann auch erforderlich, um das Überschreiben eingebetteten Skripts, um einen geringfügigen Teil der Funktionalität, die möglicherweise nicht versendet wurden mit einer Komponente zu integrieren.

Microsoft empfiehlt, ein Problem mit Version Control mittels automatischer Generierung von Ihrem Skriptressourcen während der Projektkompilierung zu vermeiden. Wenn eine umfangreiche Skriptcode Basis verwalten, können sicherungsfestlegungen zunehmend schwieriger, stellen Sie sicher, dass Änderungen am Code in jedem lokalisierte widergespiegelt werden. Als Alternative können Sie einfach eine Logik und mehrere Lokalisierung Skripts verwalten Zusammenführen der Dateien beim Erstellen des Projekts.

Weil nicht Ressourcen deklarativ eingeschlossen sind, statische Dateien muss Skript verwiesen wird entweder durch Hinzufügen von `<asp:ScriptElement>` Elemente als untergeordnetes Element von der `<Scripts>` Tag, der das ScriptManager-Steuerelement oder programmgesteuert hinzufügen `ScriptReference` Objekte um die `Scripts` Eigenschaft von der `ScriptManager` Steuerelement auf der Seite zur Laufzeit.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*Die ScriptManager und seiner Rolle bei der Lokalisierung*

Die ScriptManager kann mehrere automatische Verhalten für lokalisierte Anwendungen:

- Er findet automatisch basierte auf Einstellungen und Benennungskonventionen Skriptdateien; z. B. Debug-fähigen Skripts im Debugmodus geladen, und lädt lokalisiert Skripts, die basierend auf den Browser Benutzerauswahl-Schnittstelle.
- Er ermöglicht die Definition der Kulturen, einschließlich Kulturen.
- Er ermöglicht die Komprimierung von Skriptdateien über HTTP.
- Es speichert Skripts, um viele Anforderungen effizient verwalten.
- Es können Skripts eine Dereferenzierungsschicht hinzugefügt, indem sie über eine verschlüsselte URL weitergeleitet.

Skriptverweise können entweder programmgesteuert oder durch ein deklaratives Markup an das ScriptManager-Steuerelement hinzugefügt werden. Deklaratives Markup ist besonders nützlich, bei der Arbeit mit Skripts in eingebetteten Assemblys als das Websiteprojekt selbst, wie der Namen des Skripts als Revisionen per Push, über übertragen werden wahrscheinlich nicht ändert.

## <a name="summary"></a>Zusammenfassung

Webanwendungen wachsen eine größere Zielgruppe erreichen, wird die Notwendigkeit, umfassendere Kulturen und Communities erreichen können Kerne und eine Geschäftsmodell; e-Commerce-Webanwendungen können für den Umgang mit foreign Währungen sein muss, Inhaltsverwaltungssystemen benötigen, können nicht nur vorhanden sein, deren Inhalt jedoch auch ihre Navigation Hinweise und Formularfelder in andere Sprachen und Unternehmen müssen wissen, dass diese Anforderung wird zugegriffen werden kann.

.NET Framework unterstützt systemintern ein Framework umfangreiche Lokalisierung dazu werden Satellitenassemblys und XML-Ressourcendateien (.resx), um eine einheitliche Methode zur Suche nach Ressourcenzeichenfolgen und Bilder zu präsentieren. ASP.NET AJAX-Erweiterungen, einschließlich der Microsoft AJAX-Framework und Microsoft AJAX-Skript-Bibliothek bieten Unterstützung für dieses Programmiermodell zu clientseitigen Code einfach Zeichenfolge Ressourcensuchen aktivieren. Satellitenassemblys unterstützen die automatische Einbindung Skriptressourcen (tatsächliche JS-Dateien) über ScriptResource.axd, solange die Dateinamen einem bestimmten Benennungsschema folgen. Dank dieser Unterstützung vereinfachen den ASP.NET AJAX-Erweiterungen, die Lokalisierung von Skripts und die Globalisierung von Anwendungen.

## <a name="bio"></a>*Bio*

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und Präsidenten des myKB.com ist ([www.myKB.com](http://www.myKB.com)), in dem er zum Schreiben von ASP.NET spezialisiert-basierten Anwendungen, die Wissensdatenbank softwarelösungen konzentriert. Scott hergestellt werden kann, per e-Mail an [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Zurück](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Weiter](understanding-asp-net-ajax-web-services.md)
