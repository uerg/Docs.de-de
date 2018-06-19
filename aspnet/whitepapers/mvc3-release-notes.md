---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: 0bfe9cdc215226457ccfafff2b85ace87325b91b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
ms.locfileid: "30899102"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Übersicht](#overview)
- [Hinweise zur installationsnachbereitung](#installation-notes)
- [Softwareanforderungen](#software-requirements)
- [Dokumentation](#documentation)
- [Support](#support)
- [Aktualisieren eines ASP.NET MVC 2-Projekts zu ASP.NET MVC Update 3 Tools](#upgrading)
- [ASP.NET MVC 3 Tools Update (12. April 2011)](#tu-changes)

    - [Dialogfeld "Controller hinzufügen" kann jetzt Gerüst Controller mit Ansichten und Zugangscode erstellen.](#tu-AddControllerDialog)
    - [Verbesserungen an der "ASP.NET MVC 3 neues Projekt" (Dialogfeld)](#tu-ImprovementsNewDialogBox)
    - [Neue Projektvorlage: Modernizr 1.7](#tu-Modernizr)
    - [Neue Projektvorlagen: aktualisierte Versionen der jQuery, jQuery UI und jQuery Validation](#tu-UpdatedJQuery)
    - [Neue Projektvorlage: ADO.NET Entity Framework 4.1 als vorinstallierte NuGet-Paket](#tu-EF)
    - [Neue Projektvorlagen: JavaScript-Bibliotheken als vorinstallierte NuGet-Pakete](#tu-JavaScriptLibsNuget)
    - [Bekannte Probleme](#tu-KI)
- [ASP.NET MVC 3 RTM (13 Januar 2011)](#MVC3RTM)

    - [Änderung: Aktualisiert die Version von jQuery UI auf 1.8.7](#RTM-1)
    - [Ändern: Die Standardeinstellungen, die ModelMetadataProvider DataAnnotationsModelMetadataProvider wieder geändert](#RTM-2)
    - [Behoben: Einfügen Teil eines Ausdrucks Razor, die Leerzeichen Ergebnisse darin wird umgekehrt enthält](#RTM-3)
    - [Behoben: Umbenennen einer Razor-Datei, die im Editor geöffnet ist deaktiviert, syntaxhervorhebung und IntelliSense](#RTM-4)
    - [Bekannte Probleme](#RTM-KI)
    - [Wichtige Änderungen](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10. Dezember 2010)](#_Toc2)

    - [Projekt-Vorlagen geändert haben, z. B. jQuery 1.4.4, jQuery-Validierung 1.7 und jQuery UI 1.8.6y UI 1.8.6](#_Toc2_1)
    - [Hinzugefügte "AdditionalMetadataAttribute"-Klasse](#_Toc2_2)
    - [Verbesserte Ansicht Gerüstbau](#_Toc2_3)
    - [Hinzugefügte Html.Raw-Methode](#_Toc2_3)
    - [Umbenannte "Controller.ViewModel"-Eigenschaft und die "View"-Eigenschaft auf "ViewBag"](#_Toc2_4)
    - [Umbenannte "ControllerSessionStateAttribute" Klasse "SessionStateAttribute"](#_Toc2_5)
    - [Umbenannte RemoteAttribute "Felder"-Eigenschaft auf "AdditionalFields"](#_Toc2_6)
    - ["SkipRequestValidationAttribute", "AllowHtmlAttribute" umbenannt](#_Toc2_7)
    - [Geänderte "Html.ValidationMessage"-Methode, um die erste nützlich Fehlermeldung anzuzeigen.](#_Toc2_8)
    - [Feste @model Deklaration, um das Dokument keine Leerzeichen hinzugefügt](#_Toc2_9)
    - [Hinzugefügte "FileExtensions"-Eigenschaft, um die Ansichtsmodule zur Unterstützung der Datenbankmodul-spezifischen Dateinamen](#_Toc2_10)
    - [Feste "labelFor"zurück-Hilfsmethode zum Ausgeben des richtigen Wert für das Attribut "Für"](#_Toc2_11)
    - [Feste "RenderAction"-Methode, um explizite Werte Vorrang, während der Modellbindung](#_Toc2_12)
    - [Wichtige Änderungen](#_Toc2_BC)
    - [Bekannte Probleme](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9. November 2010)](#TOC_ASP_NET_3_RC)

    - [Neue Funktionen in ASP.NET MVC 3 RC](#_Toc276711785)
    - [NuGet-Paket-Manager](#_Toc276711786)
    - ["Neues Projekt" verbessert (Dialogfeld)](#_Toc276711787)
    - [Sessionless-Controller](#_Toc276711788)
    - [Neue Validierungsattribute](#_Toc276711789)
    - [Neue Überladungen für "labelFor zurück" und "LabelForModel"-Methoden](#_Toc276711790)
    - [Untergeordnete Aktion Zwischenspeichern der Ausgabe](#_Toc276711791)
    - [Dialogfeld Feld-Verbesserungen "Ansicht hinzufügen"](#_Toc276711792)
    - [Differenzierte Anforderungsvalidierung](#_Toc276711793)
    - [Wichtige Änderungen](#_Toc276711794)
    - [Bekannte Probleme](#_Toc276711795)
- [ASP IST. MVC 3 notiert Beta (6 Oktober 2010)](#TOC_ASP_NET_3_Beta)

    - [Neue Funktionen in ASP.NET MVC 3-Beta](#0.1__Toc274034215)
    - [NuPack-Paket-Manager](#0.1__Toc274034216)
    - [Verbesserte neues Projekt (Dialogfeld)](#0.1__Toc274034217)
    - [Vereinfachte Möglichkeit stark angeben typisierte Modelle in Razor-Ansichten](#0.1__Toc274034218)
    - [Unterstützung für neue ASP.NET Web Pages-Hilfsmethoden](#0.1__Toc274034219)
    - [Unterstützung für zusätzliche Abhängigkeiten-Injection](#0.1__Toc274034220)
    - [Neue Unterstützung für unaufdringliche jQuery-basierte Ajax](#0.1__Toc274034221)
    - [Neue Unterstützung für unaufdringliche jQuery-Validierung](#0.1__Toc274034222)
    - [Neue anwendungsweite-Flags für die Clientvalidierung und unaufdringliches JavaScript](#0.1__Toc274034223)
    - [Neue Unterstützung für Code, der vor der Ausführung von Ansichten ausgeführt wird.](#0.1__Toc274034224)
    - [Neue Unterstützung für die VBHTML-Razor-Syntax](#0.1__Toc274034225)
    - [Präzisere Kontrolle über "validateinputattribute"](#0.1__Toc274034226)
    - [Hilfsprogramme konvertieren Unterstriche, Bindestriche für die Namen der HTML-Attribute mit anonymer Objekte angegeben](#0.1__Toc274034227)
    - [Fehlerkorrekturen](#0.1__Toc274034228)
    - [Wichtige Änderungen](#0.1__Toc274034229)
    - [Bekannte Probleme](#0.1__Toc274034230)
- [Haftungsausschluss](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Übersicht

Dieses Dokument beschreibt die Version von ASP.NET MVC 3 RTM für Visual Studio 2010. ASP.NET MVC ist ein Framework zum Entwickeln von Webanwendungen, die die Model-View-Controller (MVC)-Muster verwendet wird. Der ASP.NET MVC 3-Installer umfasst die folgenden Komponenten:

- ASP.NET MVC 3-Laufzeitkomponenten
- ASP.NET MVC 3 Visual Studio 2010-tools
- ASP.NET Web Pages-Laufzeitkomponenten
- ASP.NET Web Pages Visual Studio 2010-tools
- Microsoft Paket-Manager für .NET (NuGet)
- Ein Update für Visual Studio 2010, die Unterstützung für Razor-Syntax ermöglicht. (Details finden Sie im Knowledge Base-Artikel 2483190.)

Der vollständige Satz von Anmerkungen zu dieser Version für jede Vorabversion von ASP.NET MVC 3 finden Sie auf der ASP.NET-Website unter folgender URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Hinweise zur installationsnachbereitung

Um ASP.NET MVC 3 RTM mit dem Webplattform-Installer (Web PI) installieren, finden Sie auf der folgenden Seite:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternativ können Sie das Installationsprogramm für ASP.NET MVC 3 RTM für Visual Studio 2010 auf der folgenden Seite herunterladen:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 installiert werden kann und Seite-an-Seite ausführen können mit ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

ASP.NET MVC 3-Laufzeitkomponenten benötigen die folgende Software:

- .NET Framework, Version 4. 

    ASP.NET MVC 3 Visual Studio 2010-Tools benötigen die folgende Software:
- Visual Studio 2010 noch Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Dokumentation für ASP.NET MVC ist auf der MSDN-Website unter folgender URL verfügbar:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Lernprogramme und Weitere Informationen zu ASP.NET MVC sind auf die MVC-Seite der ASP.NET-Website unter folgender URL verfügbar:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Unterstützung

Dies ist eine vollständige Unterstützung geboten. Informationen zur Inanspruchnahme technischer Unterstützung finden Sie unter der [Microsoft Support-Website](https://support.microsoft.com/).

Darüber hinaus können Sie Fragen zu dieser Version auf der ASP.NET MVC-Forum bereitgestellt, in denen die ASP.NET-Community informelle Unterstützung leisten häufig können gehören:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Aktualisieren eines ASP.NET MVC 2-Projekts zu ASP.NET MVC Update 3 Tools

ASP.NET MVC 3 kann parallel zu ASP.NET MVC 2 auf demselben Computer installiert werden die Flexibilität bei der Auswahl der Zeitpunkt des Upgrades von einer ASP.NET MVC 2-Anwendung in ASP.NET MVC 3 enthält.

Um eine vorhandene ASP.NET MVC 2-Anwendung auf Version 3 manuell zu aktualisieren, führen Sie folgende Schritte aus:

1. Erstellen Sie ein neues, leeres ASP.NET MVC 3-Projekt auf Ihrem Computer. Dieses Projekt enthält einige Dateien, die für das Upgrade erforderlich sind.
2. Kopieren Sie die folgenden Dateien aus dem ASP.NET MVC 3-Projekt, in die entsprechende Position Ihres ASP.NET MVC 2-Projekts. Sie müssen alle Verweise auf die jQuery-Bibliothek für den neuen Dateinamen (jQuery-1.5.1.js) tragen zu aktualisieren: 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /Content/themes/\*.\*
3. Kopieren der *Pakete* Ordner im Stammverzeichnis der leeren ASP.NET MVC 3-Projektmappe in das Stammverzeichnis der Projektmappe im Verzeichnis befindet sich die SLN Datei befindet.
4. Wenn Ihr ASP.NET MVC 2-Projekt irgendwelche Bereiche enthält, kopieren Sie die Datei /Views/Web.config die *Ansichten* Ordner jedes Bereichs.
5. In beiden "Web.config"-Dateien in das ASP.NET MVC 2-Projekt Global suchen Sie und Ersetzen Sie die ASP.NET MVC-Version. Suchen Sie nach: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Ersetzen Sie es durch Folgendes:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. Löschen Sie im Projektmappen-Explorer den Verweis auf *System.Web.Mvc* (welche verweist auf die DLL von Version 2), fügen Sie dann einen Verweis auf *System.Web.Mvc* (v3.0.0.0).
7. Fügen Sie einen Verweis auf System.Web.WebPages.dll und auf System.Web.helpers.dll hinzu. Diese Assemblys befinden sich in den folgenden Ordnern: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projektnamens, und wählen Sie Projekt entladen. Klicken Sie dann mit der rechten Maustaste erneut auf des Projektnamens, und wählen Sie bearbeiten *Projektname*csproj.
9. Suchen Sie die *ProjectTypeGuids* Element, und Ersetzen Sie {F85E285D-A4E0-4152-9332-AB1D724D3325} durch {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Speichern Sie die Änderungen rechten Maustaste auf das Projekt, und wählen Sie dann Projekt erneut laden.
11. Fügen Sie in der Stammdatei "Web.config" der Anwendung die folgenden Einstellungen, um die *Assemblys* Abschnitt. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Wenn das Projekt alle Bibliotheken von Drittanbietern, die mithilfe von ASP.NET MVC 2 kompiliert wurden verweist, fügen Sie Folgendes hervorgehoben *BindingRedirect* Element in der Datei Web.config im Anwendungsstamm die  *Konfiguration* Abschnitt: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Änderungen in ASP.NET MVC 3 Tools Update

Dieser Abschnitt beschreibt die Änderungen, die seit der ASP.NET MVC 3 RTM-Version in der ASP.NET MVC 3 Tools Update-Version.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Dialogfeld "Controller hinzufügen" kann jetzt Gerüst Controller mit Ansichten und Zugangscode erstellen.

Gerüstbau stellt eine Möglichkeit, schnell einen Controller und Ansichten für Ihre Anwendung zu generieren. Nach Erstellung des Codes können Sie sie entsprechend den Anforderungen Ihres Projekts bearbeiten.

Zum Starten der *Controller hinzufügen* Maustaste in ASP.NET MVC 3, im Dialogfeld die *Controller* Ordner in *Projektmappen-Explorer*, klicken Sie auf *hinzufügen*, und klicken Sie dann auf *Controller*. Das Dialogfeld wurde verbessert, um zusätzliche gerüstoptionen bieten.

![](mvc3-release-notes/_static/image1.png)

Standardmäßig sind drei folgenden gerüstbauvorlagen zur Verfügung.

#### <a name="empty-controller"></a>Leerer Controller

Diese Vorlage erzeugt eine leere controllerdatei. Diese Vorlage entspricht der Deaktivierung des Kontrollkästchens *Aktionen, für Erstell-, Aktualisierungs- und Details hinzufügen und Löschen von Szenarien* in früheren Versionen von ASP.NET MVC. Wenn Sie diese Vorlage auswählen, sind keine weiteren Optionen verfügbar.

#### <a name="controller-with-empty-readwrite-actions"></a>Controller mit leeren Lese-/schreibaktionen

Diese Vorlage erzeugt eine controllerdatei, die alle erforderlichen Aktionsmethoden jedoch keinen Implementierungscode in den Methoden aufweist. Diese Vorlage entspricht dem überprüfen *Aktionen, für Erstell-, Aktualisierungs- und Details hinzufügen und Löschen von Szenarien* in früheren Versionen von ASP.NET MVC. Wenn Sie diese Vorlage auswählen, sind keine weiteren Optionen verfügbar.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controller mit Lese-/schreibaktionen und Ansichten unter Verwendung von Entity Framework

Mit dieser Vorlage können Sie schnell eine funktionierende Dateneingabe Benutzeroberfläche zu erstellen. Generiert Code, der eine Reihe von allgemeinen Anforderungen und Szenarien, z. B. die folgenden behandelt:

- *Datenzugriff*. Der generierte Code liest und schreibt Entitäten in einer Datenbank. Mit dem Entity Framework Code First-Ansatz funktioniert, wenn Sie eine bestehende Datenkontextklasse auswählen oder wenn Sie die Vorlage ein neues generieren lassen *DbContext* Klasse. Es funktioniert auch mit dem Entity Framework Database First oder Model First-Ansatz bei Auswahl einer vorhandenen *ObjectContext* Klasse.
- *Überprüfung*. Der generierte Code verwendet ASP.NET MVC-modellbindung und Metadatenfunktionen, sodass Formularübermittlung entsprechend in Ihrer Modellklasse deklarierten Regeln überprüft werden. Dazu zählen integrierte Validierungsregeln, wie die *erforderlich* und *StringLength* Attribute und benutzerdefinierte Validierungsregeln.
- *1: n-Beziehungen*. Wenn Sie 1: n-Fremdschlüssel-Beziehungen zwischen Ihren Modellklassen definieren, wird der generierte Code Dropdownlisten für die Auswahl von verknüpften Entitäten erzeugen. Beispielsweise können Sie die folgenden Modellklassen gemäß Entity Framework Code First-Konventionen definieren: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Wenn Sie dann das Gerüst Erstellen eines Controllers für die *Produkt* -Klasse, die Sichten ermöglichen es Benutzern, wählen eine *Kategorie* -Objekt für jedes *Produkt* Instanz.

    Mit dieser Vorlage können Sie zusätzliche Optionen in der *Controller hinzufügen* (Dialogfeld). Für *Modellschemas*, Sie können jede beliebige Modellklasse wählen Sie in der Projektmappe, die den Typ der Daten bestimmt, die Benutzer werden zum Erstellen oder bearbeiten können:
- Wenn Sie Entity Framework Code First verwenden möchten, können Sie jede beliebige Modellklasse wählen.
- Wenn Sie Entity Framework Database First oder Entity Framework Model First verwenden, achten Sie darauf, dass Sie eine im konzeptionellen Modell definierte entitätenklasse zu wählen.

Für *Datenkontext Klasse*, nehmen Sie diese Auswahlen:

- Wenn Sie möchten Code First verwenden und haben keine vorhandenen Datenkontext Klasse, wählen Sie ** neuen Datenkontext **. Dann wird eine neue Datenkontextklasse für Sie generiert.
- Wenn Sie Code First verwenden und über eine bestehende Datenkontextklasse verfügen möchten, wählen Sie diese hier an. Es werden aktualisiert werden, um die Modellklasse beizubehalten, die Sie ausgewählt haben.
- Wenn Sie Database First oder Model First verwenden, wählen Sie hier Ihre Objektkontextklasse aus.

Wählen Sie für Sichten das Ansichtsmodul aus, die, das Sie verwenden möchten, oder wählen Sie keines, wenn Sie nicht das Gerüst für alle Ansichten erstellen möchten.

Sie können auswählen, Advanced Options Weitere Optionen für die erstellten Ansichten angeben. Beispielsweise können Sie die Layout- oder Masterseite verwenden auswählen.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Verbesserungen an der "ASP.NET MVC 3 neues Projekt" (Dialogfeld)

Das Dialogfeld, das Sie verwenden, um neue ASP.NET MVC 3-Projekte erstellen umfasst mehrere Verbesserungen wie nachstehend aufgelistet.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Neue "Intranet" Projektvorlage

Die Liste Projektvorlage enthält eine neue Vorlage Intranetanwendung. Diese Vorlage enthält Einstellungen für das Erstellen einer Webanwendung, die mithilfe von Windows-Authentifizierung anstatt der Formularauthentifizierung. Da eine Intranetanwendung bestimmte IIS-Einstellungen erforderlich, die in einer Projektvorlage gekapselt werden können ist, enthält die Vorlage eine Infodatei durch Anweisungen dazu, wie Sie die Projektvorlage in IIS zum funktionieren. Dokumentation für die eine neue Vorlage Intranetanwendung ist auf der MSDN-Website unter folgender URL verfügbar:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Projektvorlagen sind nun HTML5 aktiviert

Des Dialogfelds "Neues Projekt" "enthält jetzt eine Option aus, um die Projektvorlagen von HTML5-spezifische Funktionen hinzuzufügen. Wählen Sie die Option bewirkt, dass Sichten generiert werden, die das neue HTML5 enthalten `<header>`, `<footer>`, und `<navigation>` Elemente.

Beachten Sie, dass frühere Versionen der Browser keine HTML5-spezifische Tags unterstützen. Zur Überwindung dieser Einschränkung enthalten die HTML5-Projektvorlagen einen Verweis auf die Modernizr-Bibliothek. (Siehe nächsten Abschnitt.)

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Neue Projektvorlage: Modernizr 1.7

Modernizr ist eine JavaScript-Bibliothek, die Unterstützung von CSS 3 und HTML5 in Browsern ermöglicht, die noch nicht über diese Funktionen unterstützen. Diese Bibliothek ist als vorinstallierte NuGet-Paket in den Vorlagen für ASP.NET MVC 3-Projekte enthalten. Weitere Information zu Modernizr finden Sie unter [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Neue Projektvorlagen: aktualisierte Versionen der jQuery, jQuery UI und jQuery Validation

Die Projektvorlagen enthalten nun die folgenden Versionen von jQuery-Skripts:

- jQuery 1.5.1
- jQuery-Validierung 1.8
- jQuery UI 1.8.11

Diese Bibliotheken sind als vorinstallierte NuGet-Pakete enthalten.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Neue Projektvorlage: ADO.NET Entity Framework 4.1 als vorinstallierte NuGet-Paket

Das ADO.NET Entity Framework 4.1 enthält die Code First-Funktion. Code First ist ein neues Entwicklungsmuster für das ADO.NET Entity Framework, die eine Alternative zur vorhandenen Muster Database First und Model First bietet.

Code First legt das Hauptaugenmerk auf die das Modell unter Verwendung von POCO-Klassen ("plain old CLR Objects") in Visual Basic oder c# geschriebene Definition. Diese Klassen anschließend zu einer vorhandenen Datenbank zugeordnet werden können oder verwendet werden, um die Erstellung eines Datenbankschemas. Zusätzliche Konfiguration angegeben werden, dass mit *DataAnnotations* Attribute oder mithilfe der fluent-APIs.

Dokumentation für die Verwendung von Code Firstwith ASP.NET MVC ist auf der ASP.NET-Website unter den folgenden URLs verfügbar:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Neue Projektvorlagen: JavaScript-Bibliotheken als vorinstallierte NuGet-Pakete

Wenn Sie ein neues ASP.NET MVC 3-Projekt erstellen, enthält das Projekt die JavaScript-Dateien bereits erwähnten (z. B. die Modernizr-Bibliothek) durch eine Installation mithilfe von NuGet anstatt direkt die Skripts in den Ordner Scripts in der Projektvorlage Inhalt. Dadurch können Sie NuGet verwenden, um die Skripts auf die neueste Version zu aktualisieren, wenn neue Versionen der Skripts freigegeben werden.

Beispielsweise wird bei die Häufigkeit neuer jQuery-Versionen, die Version von jQuery in der Projektvorlage enthalten irgendwann veraltet sein. Allerdings da jQuery als ein installiertes NuGet-Paket enthalten ist, werden Sie im Dialogfeld NuGet benachrichtigt, wenn neuere Versionen von jQuery verfügbar sind.

Da jQuery die Versionsnummer im Dateinamen enthält, Aktualisierung von jQuery auf die neueste Version auch erfordert Aktualisieren der `<script>` Tag, die jQuery-Datei verweist, um den neuen Dateinamen verwenden. Andere enthaltene Skriptbibliotheken enthalten nicht die Versionsnummer im Skriptnamen, damit sie leichter auf die jeweils neuesten Version aktualisiert werden können.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Bekannte Probleme

- In einigen Fällen kann die Installation mit der Fehlermeldung "Installationsfehler Nachricht Installation mit dem Fehlercode (0 x 80070643)" fehl. Informationen zum Umgehen dieses Problems finden Sie unter [Knowledge Base-Artikel 2531566](https://support.microsoft.com/kb/2531566).
- Das Gerüst für das Hinzufügen eines Controllers wird nicht zu Entitäten erstellen, die Unterstützung in Entity Framework nutzen. Angenommen, eine Basis *Person* -Klasse, die von geerbt wird eine *Student* -Klasse unter Gerüstbaus die *Student* Klasse führt in generiertem Code, der nicht kompiliert wird.
- Erstellen ein neues ASP.NET MVC 3-Projekt innerhalb eines Projektmappenordners Ursachen eine *NullReferenceException* Fehler. Die problemumgehung besteht darin, das ASP.NET MVC 3-Projekt in das Stammverzeichnis der Projektmappe zu erstellen, und klicken Sie dann in den Projektmappenordner verschieben.
- IntelliSense für Razor-Syntax funktioniert nicht, wenn ReSharper installiert ist. Wenn Sie ReSharper installiert haben und die Razor IntelliSense-Unterstützung in ASP.NET MVC 3 nutzen möchten, finden Sie im Eintrag [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) hadis-Blog erläutert die Möglichkeiten, diese heute zusammen verwenden.
- Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.
- Wenn Sie eine Razor-Ansicht bearbeiten (cshtml oder. *Vbhtml* Datei), Ansichten. ASP.NET MVC 3 enthält keinerlei Ausschnitte für Razor-Ansichten keine... Aspxselecting einen Codeausschnitt für ASP.NET MVC werden Ausschnitte für angezeigt.
- Wenn Sie ASP.NET MVC 3 für Visual Web Developer Express auf einem Computer installieren, auf dem Visual Studio nicht installiert ist, und klicken Sie dann später Visual Studio installieren, müssen Sie ASP.NET MVC 3 erneut installieren. Visual Studio und Visual Web Developer Express haben die Komponenten, die von ASP.NET MVC 3-Installer aktualisiert werden. Das gleiche Problem gilt, wenn Sie ASP.NET MVC 3 für Visual Studio auf einem Computer installieren, die keine Visual Web Developer Express, und klicken Sie dann später installieren Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Änderungen in ASP.NET MVC 3 RTM

Dieser Abschnitt beschreibt die Änderungen und Fehlerkorrekturen in der ASP.NET MVC 3 RTM-Version, die seit der RC2-Version vorgenommen wurden.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Änderung: Aktualisiert die Version von jQuery UI auf 1.8.7

Die ASP.NET MVC-Projektvorlagen für Visual Studio wurden aktualisiert, um die neueste Version von jQuery UI-Bibliothek einzuschließen. Die Vorlagen umfassen auch den minimalen Satz von Ressourcendateien, jQuery UI, z. B. die zugeordnete CSS und Bilddateien erforderlich.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Ändern: Die Standardeinstellungen, die ModelMetadataProvider DataAnnotationsModelMetadataProvider wieder geändert

Die RC2-Version von ASP.NET MVC 3 eingeführt wurde eine *CachedDataAnnotationsMetadataProvider* -Klasse derjenigen zusätzlich zum vorhandenen zwischenspeichern *DataAnnotationsModelMetadataProvider* der Klasse eine zur Verbesserung der Leistung. Allerdings wurden einige Programmfehler in dieser Implementierung gemeldet, damit die Änderung wurde zurückgesetzt und in der Futures MVC-Projekt zur verschoben [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Behoben: Einfügen Teil eines Ausdrucks Razor, die Leerzeichen Ergebnisse darin wird umgekehrt enthält

Beim Einfügen eines Teils eines Razor-Ausdrucks, der Leerzeichen in einer Razor-Datei enthält, wird der Ergebnisausdruck in Vorabversionen von ASP.NET MVC 3 umgekehrt. Betrachten Sie beispielsweise den folgenden Block der Razor-Code:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Wenn Sie den Text "erste Param" bei der ersten Methode auswählen, fügen Sie ihn als Argument an die zweite Methode ist das Ergebnis wie folgt:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Das richtige Verhalten ist, dass es sich bei der Einfügevorgang in der folgenden führen soll:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Dieses Problem wurde in der RTM-Version behoben, damit der Ausdruck ordnungsgemäß während des Einfügevorgangs beibehalten wird.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Behoben: Umbenennen einer Razor-Datei, die im Editor geöffnet ist deaktiviert, syntaxhervorhebung und IntelliSense

Umbenennen einer Razor-Datei mit Projektmappen-Explorer aus, während die Datei im Editor-Fenster geöffnet wird bewirkt, dass syntaxhervorhebung und IntelliSense zum Programmende für diese Datei. Dies wurde korrigiert, damit hervorgehoben, und IntelliSense nach einer Umbenennung beibehalten werden.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Bekannte Probleme

- Wenn Sie Visual Studio 2010 SP1 Beta schließen, während die NuGet-Paket-Manager-Konsole geöffnet ist, wird von Visual Studio stürzt ab und wird neu gestartet. Dieses Problem wird behoben werden in der RTM-Version von Visual Studio 2010 SP1.
- ASP.NET MVC 3-Installationsprogramm kann nur eine erste Version des NuGet-Paket-Managers installieren. Nach der Installation der Ursprungsversion NuGet installiert werden kann, und mithilfe von Visual Studio-Erweiterungs-Manager aktualisiert. Wenn Sie bereits über NuGet installiert haben, wechseln Sie in der Visual Studio-Erweiterung Gallery Update auf die neueste Version von NuGet.
- Erstellen ein neues ASP.NET MVC 3-Projekt innerhalb eines Projektmappenordners Ursachen eine *NullReferenceException* Fehler. Die problemumgehung besteht darin, das ASP.NET MVC 3-Projekt in das Stammverzeichnis der Projektmappe zu erstellen, und klicken Sie dann in den Projektmappenordner verschieben.
- Das Installationsprogramm dauert wesentlich länger als in vorherigen Versionen von ASP.NET MVC abgeschlossen. Dies ist, da es Komponenten von Visual Studio 2010 aktualisiert werden.
- IntelliSense für Razor-Syntax funktioniert nicht, wenn ReSharper installiert ist. Wenn Sie ReSharper installiert haben und die Razor IntelliSense-Unterstützung in ASP.NET MVC 3 nutzen möchten, finden Sie im Eintrag [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) hadis-Blog erläutert die Möglichkeiten, diese heute zusammen verwenden.
- CCSHTML und VBHTML-Ansichten, die mit der Betaversion von ASP.NET MVC 3 erstellt werden, nicht ihre Buildvorgang ordnungsgemäß festgelegt, Typen werden mit dem Ergebnis, das diese anzuzeigen weggelassen, wenn das Projekt veröffentlicht wird. Der Build Action-Wert für diese Dateien sollten auf "Inhalt" festgelegt werden. ASP.NET MVC 3 RTM behebt dieses Problem für neue Dateien, aber nicht beheben Sie die Einstellung für vorhandene Dateien für ein Projekt mit Vorabversionen erstellt.
- ![](mvc3-release-notes/_static/image3.png)
- Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.
- Bei der Bearbeitung einer Razor-Ansicht (cshtml-Datei), das Menüelement zu Controller wechseln Sie in Visual Studio ist nicht verfügbar, und es sind keine Codeausschnitte.
- Wenn Sie ASP.NET MVC 3 für Visual Web Developer Express auf einem Computer installieren, auf dem Visual Studio nicht installiert ist, und klicken Sie dann später Visual Studio installieren, müssen Sie ASP.NET MVC 3 erneut installieren. Visual Studio und Visual Web Developer Express haben die Komponenten, die von ASP.NET MVC 3-Installer aktualisiert werden. Das gleiche Problem gilt, wenn Sie ASP.NET MVC 3 für Visual Studio auf einem Computer installieren, die keine Visual Web Developer Express, und klicken Sie dann später installieren Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

- In früheren Versionen von ASP.NET MVC Aktion, die Filter erstellen pro Anforderung außer in einigen Fällen. Dieses Verhalten wurde nie ein Verhalten garantiert jedoch lediglich ein Implementierungsdetail, und der Vertrag für Filter sie zustandslose berücksichtigt wurde. Filter werden in ASP.NET MVC 3 genauer zwischengespeichert. Aus diesem Grund können benutzerdefinierten Aktionsfiltern die Instanzstatus nicht ordnungsgemäß speichern unterbrochen werden.
- Die Ausführungsreihenfolge für Ausnahmefilter wurde geändert, für die Ausnahmefilter, die dieselbe *Reihenfolge* Wert. In ASP.NET MVC 2 und früheren Ausnahmefilter auf dem Controller an, die dieselbe *Reihenfolge* Wert wie denen auf eine Aktionsmethode vor die Ausnahmefilter auf die Aktionsmethode ausgeführt werden. Dies würde i. d. r. der Fall sein, wenn Ausnahmefilter angewendet werden ohne angegebenen *Reihenfolge* Wert. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, damit der spezifischste Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen Wenn die *Reihenfolge* explizit angegeben wird, werden die Filter in der angegebenen Reihenfolge ausgeführt werden.
- Eine neue Eigenschaft mit dem Namen *FileExtensions* wurde hinzugefügt, um die *VirtualPathProviderViewEngine* Basisklasse. Wenn ASP.NET eine Ansicht von Pfad (nicht durch den Namen) sucht, werden nur Ansichten mit der Erweiterung in der Liste, die durch diese neue Eigenschaft angegebene enthaltenen berücksichtigt. Dies ist eine wichtige Änderung in Anwendungen, in denen ein benutzerdefinierte Buildanbieter registriert ist, um eine benutzerdefinierte Erweiterung für Web Form-Ansichten zu aktivieren und der Anbieter diese Sichten mit einem Namen, statt einen vollständigen Pfad verweist. Die problemumgehung besteht darin, Ändern des Werts der *FileExtensions* Eigenschaft, um die benutzerdefinierte Erweiterung einschließen.
- Implementierungen von benutzerdefinierten Controller-Factory, die direkt implementieren die *IControllerFactory* Schnittstelle muss eine Implementierung der neuen bereitstellen *GetControllerSessionBehavior* -Methode, die wurde die Schnittstelle in dieser Version hinzugefügt. Im Allgemeinen wird empfohlen, dass Sie nicht direkt diese Schnittstelle implementieren und stattdessen eine Klasse von leiten *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Änderungen in ASP.NET MVC 3-RC2

Dieser Abschnitt beschreibt die Änderungen (neue Features und Fehlerkorrekturen) in der ASP.NET MVC 3 RC2-Version, die seit der RC-Version vorgenommen wurden.

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Projekt-Vorlagen geändert haben, z. B. jQuery 1.4.4, jQuery-Validierung 1.7 und jQuery UI 1.8.6

Die Projektvorlagen für ASP.NET MVC 3 enthalten jetzt die neuesten Versionen der jQuery, jQuery-Validierung und jQuery UI. jQuery UI ist eine neue Funktion mit die Projektvorlagen und bietet nützliche Benutzer Schnittstelle Widgets. Weitere Informationen zu jQuery UI, finden Sie auf ihrer Startseite: [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Hinzugefügte "AdditionalMetadataAttribute"-Klasse

Sie können der *AdditionalMetadataAttribute* Klasse zum Auffüllen der *ModelMetadata.AdditionalValues* Wörterbuch für eine Modelleigenschaft.

Angenommen Sie, dass einem Ansichtsmodell Eigenschaften verfügt, die nur an einen Administrator angezeigt werden sollen. Dieses Modell kann das neue Attribut mit AdminOnly als Schlüssel und "true" als Wert an, wie im folgenden Beispiel mit Anmerkungen versehen:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Diese Metadaten ist Anzeige oder Editor-Vorlage zur Verfügung gestellt, wenn ein Produktmodell für die Ansicht gerendert wird. Es liegt bei Ihnen als Entwickler die Metadateninformationen zu interpretieren.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Verbesserte Ansicht Gerüstbau

T4-Vorlagen, Ansichten Gerüstbau jetzt zum Generieren Aufrufe von Hilfsmethoden für die Vorlage wie z. B. *EditorFor* anstelle von Hilfsprogrammen wie z. B. *TextBoxFor*. Diese Änderung verbessert die Unterstützung für Metadaten für das Modell in Form von Datenattributen für die Anmerkung, wenn das Dialogfeld "Ansicht hinzufügen" eine Ansicht generiert.

Das Gerüst für die Ansicht hinzufügen hinaus Verbesserte Erkennung und die Nutzung von Primärschlüsselinformationen für das Modell anhand der Konvention. Das Dialogfeld "Ansicht hinzufügen" verwendet z. B. diese Informationen, um sicherzustellen, dass der Primärschlüsselwert nicht als ein bearbeitbares Formularfeld Gerüstbau.

Der Standard bearbeiten und Erstellen von Vorlagen enthalten Verweise auf die jQuery-Skripts, die für die Clientvalidierung erforderlich.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Hinzugefügte Html.Raw-Methode

Standardmäßig der Razor anzeigen Engine HTML-Codierung aller Werte. Beispielsweise der folgende Codeausschnitt den HTML-Code in die Variable für die Grußformel codiert, damit er auf der Seite als angezeigt wird `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Die neue *Html.Raw* Methode bietet eine einfache Möglichkeit zum Anzeigen von nicht codierte HTML, wenn der Inhalt bekannt ist, um sicher zu werden. Das folgende Beispiel zeigt die gleiche Zeichenfolge jedoch die Zeichenfolge wird als Markup gerendert:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Umbenannte "Controller.ViewModel"-Eigenschaft und die "View"-Eigenschaft auf "ViewBag"

Zuvor die *ViewModel* Eigenschaft *Controller* zugestimmt haben, um die *Ansicht* Eigenschaft einer Sicht. Diese beiden Eigenschaften bieten die Möglichkeit, Zugriffswerte, der die *ViewDataDictionary* -Objekt unter Verwendung der dynamischen Eigenschaftenaccessor-Syntax. Beide Eigenschaften wurden umbenannt, um identisch damit eine Verwechslung vermieden und mehr konsistent sein.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Umbenannte "ControllerSessionStateAttribute" Klasse "SessionStateAttribute"

Die *ControllerSessionStateAttribute* Klasse in der RC-Version von ASP.NET MVC 3 eingeführt wurde. Die Eigenschaft wurde umbenannt, um eine kompaktere sein.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Umbenannte RemoteAttribute "Felder"-Eigenschaft auf "AdditionalFields"

Die *RemoteAttribute* Klasse *Felder* Eigenschaft aufgrund eine gewissen Verwirrung von Benutzern. Diese Eigenschaft auf Umbenennen *AdditionalFields* ihre Absicht verdeutlicht.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>"SkipRequestValidationAttribute", "AllowHtmlAttribute" umbenannt

Die *SkipRequestValidationAttribute* Attribut wurde umbenannt in *AllowHtmlAttribute* , besser seiner vorgesehene Verwendung entsprechen.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Geänderte "Html.ValidationMessage"-Methode, um die erste nützlich Fehlermeldung anzuzeigen.

Die *Html.ValidationMessage* Methode behoben wurde, um die erste nützlich Fehlermeldung anstatt Sie einfach den ersten Fehler anzuzeigen.

Während der modellbindung, die *ModelState* Wörterbuch aus mehreren Quellen mit Fehlermeldungen zu dieser Eigenschaft, die aus dem Modell selbst einschließlich ausgefüllt werden (wenn implementiert *IValidatableObject* ), von Validierungsattributen für die Eigenschaft angewendet und von Ausnahmen, die ausgelöst wird, während die Eigenschaft zugegriffen wird.

Wenn die *Html.ValidationMessage* Methode zeigt eine validierungsmeldung an, das modellzustands-Einträge, die eine Ausnahme enthalten übersprungen, da diese in der Regel nicht für den Endbenutzer vorgesehen sind. Der erste validierungsmeldung, die nicht mit einer Ausnahme zugeordnet ist, und diese Nachricht zeigt stattdessen sucht die Methode. Wenn keine solche Nachricht gefunden wird, wird standardmäßig eine allgemeine Fehlermeldung, die die erste Ausnahme zugeordnet ist.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Feste @model Deklaration, um das Dokument keine Leerzeichen hinzugefügt

In früheren Versionen der <em>@model</em> Deklaration am oberen Rand einer Ansicht der gerenderten HTML-Ausgabe eine leere Zeile hinzugefügt. Dies wurde korrigiert, damit die Deklaration keine Leerzeichen entstehen.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Hinzugefügte "FileExtensions"-Eigenschaft, um die Ansichtsmodule zur Unterstützung der Datenbankmodul-spezifischen Dateinamen

Eine Sicht mit einem expliziten Ansichtspfad wie im folgenden Beispiel kann ein Ansichtsmodul zurückgegeben werden:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Das erste Anzeigemodul versucht immer zum Rendern der Ansicht. Standardmäßig ist das WebForms-Anzeigemodul das erste Ansichtsmodul; Da das Web Forms-Modul eine Razor-Ansicht rendern kann, tritt ein Fehler auf. Ansichtsmodule verfügen jetzt über eine *FileExtensions* -Eigenschaft, die verwendet wird, welche Dateierweiterungen angeben, die sie unterstützen. Diese Eigenschaft ist aktiviert, wenn ASP.NET bestimmt, ob eine Datei ein Ansichtsmodul gerendert werden kann. Dies ist eine wichtige Änderung und weitere Details sind in enthalten die [Breaking Changes](#_Toc2_BC) Abschnitt dieses Dokuments.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Feste "labelFor"zurück-Hilfsmethode zum Ausgeben des richtigen Wert für das Attribut "Für"

Ein Fehler behoben wurde Where der *LabelFor* Methode gerendert eine *für* -Attribut, entspricht die *input* des Elements *Namen* Attribut seine-ID. Gemäß der W3C die *für* Attribut übereinstimmen, die *input* der Element-ID

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Feste "RenderAction"-Methode, um explizite Werte Vorrang, während der Modellbindung

In früheren Versionen, explizite Werte, die übergeben wurden, die *RenderAction* Methode wurden zugunsten der aktuellen Formularwerte während der modellbindung in eine untergeordnete Aktion ignoriert wird. Die Lösung stellt sicher, dass explizite Werte während der modellbindung Vorrang.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

- In früheren Versionen von ASP.NET MVC erstellt wurden die Aktionsfilter pro Anforderung außer in einigen Fällen. Dieses Verhalten wurde nie ein Verhalten garantiert jedoch lediglich ein Implementierungsdetail, und der Vertrag für Filter sie zustandslose berücksichtigt wurde. Filter werden in ASP.NET MVC 3 genauer zwischengespeichert. Aus diesem Grund können benutzerdefinierten Aktionsfiltern die Instanzstatus nicht ordnungsgemäß speichern unterbrochen werden.
- Die Ausführungsreihenfolge für Ausnahmefilter wurde geändert, für die Ausnahmefilter, die dieselbe *Reihenfolge* Wert. In ASP.NET MVC 2 und früheren Ausnahmefilter auf dem Controller an, die die gleiche mussten *Reihenfolge* Wert wie denen auf eine Aktionsmethode vor die Ausnahmefilter auf die Aktionsmethode ausgeführt wurden. Dies würde i. d. r. der Fall sein, wenn Ausnahmefilter angewendet wurden ohne angegebenen *Reihenfolge* Wert. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, damit der spezifischste Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen Wenn die *Reihenfolge* explizit angegeben wird, werden die Filter in der angegebenen Reihenfolge ausgeführt werden.
- Eine neue Eigenschaft mit dem Namen *FileExtensions* wurde hinzugefügt, um die *VirtualPathProviderViewEngine* Basisklasse. Wenn ASP.NET eine Ansicht von Pfad (nicht durch den Namen) sucht, werden nur Ansichten mit der Erweiterung in der Liste, die durch diese neue Eigenschaft angegebene enthaltenen berücksichtigt. Dies ist eine wichtige Änderung in Anwendungen, in denen ein benutzerdefinierte Buildanbieter registriert ist, um eine benutzerdefinierte Erweiterung für Web Form-Ansichten zu aktivieren und der Anbieter diese Sichten mit einem Namen, statt einen vollständigen Pfad verweist. Die problemumgehung besteht darin, Ändern des Werts der *FileExtensions* Eigenschaft, um die benutzerdefinierte Erweiterung einschließen.
- Implementierungen von benutzerdefinierten Controller-Factory, die direkt implementieren die <em>IControllerFactory</em> Schnittstelle muss eine Implementierung der neuen bereitstellen <em>GetControllerSessionBehavior</em>  <em>Methode, die die Schnittstelle in dieser Version hinzugefügte</em>. Im Allgemeinen wird empfohlen, dass Sie nicht direkt diese Schnittstelle implementieren und stattdessen eine Klasse von leiten <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Bekannte Probleme

- ASP.NET MVC 3-Installationsprogramm kann nur eine erste Version des NuGet-Paket-Managers installieren. Nach der Installation der Ursprungsversion NuGet installiert werden kann, und mithilfe von Visual Studio-Erweiterungs-Manager aktualisiert. Wenn Sie bereits über NuGet installiert haben, wechseln Sie in der Visual Studio-Erweiterung Gallery Update auf die neueste Version von NuGet.
- Erstellen ein neues ASP.NET MVC 3-Projekt innerhalb eines Projektmappenordners Ursachen eine *NullReferenceException* Fehler. Die problemumgehung besteht darin, das ASP.NET MVC 3-Projekt in das Stammverzeichnis der Projektmappe zu erstellen, und klicken Sie dann in den Projektmappenordner verschieben.
- Das Installationsprogramm dauert wesentlich länger als in vorherigen Versionen von ASP.NET MVC abgeschlossen. Dies ist, da es Komponenten von Visual Studio 2010 aktualisiert werden.
- IntelliSense für Razor-Syntax funktioniert nicht, wenn ReSharper installiert ist. Wenn Sie ReSharper installiert haben und die Razor IntelliSense-Unterstützung in ASP.NET MVC 3 RC2 nutzen möchten, finden Sie im Eintrag [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) hadis-Blog erläutert die Möglichkeiten, diese heute zusammen verwenden.
- CSHTML und VBHTML-Ansichten, die mit der Betaversion von ASP.NET MVC 3 erstellt werden, nicht ihre Buildvorgang ordnungsgemäß festgelegt, mit dem Ergebnis, das Anzeigen dieser Typen sind nicht angegeben, wenn der Veröffentlichung des Projekts. Die *Buildvorgang* Wert für diese Dateien auf Inhalt festgelegt werden soll ". ASP.NET MVC 3 RC2 behebt dieses Problem für neue Dateien, aber nicht beheben Sie die Einstellung für vorhandene Dateien für ein Projekt mit der Betaversion erstellt wurden.![](mvc3-release-notes/_static/image4.png)
- Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.
- Bei der Bearbeitung einer Razor-Ansicht (cshtml-Datei), das Menüelement zu Controller wechseln Sie in Visual Studio ist nicht verfügbar, und es sind keine Codeausschnitte.
- Wenn Sie ASP.NET MVC 3 für Visual Web Developer Express auf einem Computer installieren, auf dem Visual Studio nicht installiert ist, und klicken Sie dann später Visual Studio installieren, müssen Sie ASP.NET MVC 3 erneut installieren. Visual Studio und Visual Web Developer Express haben die Komponenten, die von ASP.NET MVC 3-Installer aktualisiert werden. Das gleiche Problem gilt, wenn Sie ASP.NET MVC 3 für Visual Studio auf einem Computer installieren, die keine Visual Web Developer Express, und klicken Sie dann später installieren Visual Web Developer Express.
- Installieren ASP.NET MVC 3 RC 2 wird NuGet nicht aktualisiert, wenn Sie bereits installiert ist. Um ein upgrade von NuGet, fahren Sie mit der Erweiterung für Visual Studio-Manager, und es sollte als verfügbares Update angezeigt. Sie können NuGet auf die neueste Version von dort aus aktualisieren.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Release Candidate

ASP.NET MVC Release Candidate wurde am 9. November 2010 veröffentlicht.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Neue Funktionen in ASP.NET MVC 3 RC

Dieser Abschnitt beschreibt die Funktionen, die eingeführt wurden in der ASP.NET MVC 3 RC-Version, seit die Betaversion.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet-Paket-Manager

ASP.NET MVC 3 enthält die NuGet-Paket-Manager (ehemals NuPack), also ein Verwaltungstool integrierten Paket zum Hinzufügen von Bibliotheken und Tools für Visual Studio-Projekte. Dieses Tool automatisiert die Schritte, die Entwickler noch heute zu ergreifen, um eine Bibliothek in ihrer ursprünglichen Struktur abrufen.

Sie können mit NuGet, wie ein Befehlszeilentool, wie einer integrierten Konsolenfenster in Visual Studio 2010, klicken Sie im Kontextmenü der Visual Studio und als einen Satz von PowerShell-Cmdlets arbeiten.

Weitere Informationen zu NuGet finden Sie unter der [NuGet-Dokumentation](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>"Neues Projekt" verbessert (Dialogfeld)

Wenn Sie ein neues Projekt erstellen, kann das Dialogfeld "Neues Projekt" jetzt Sie das Ansichtsmodul als auch für eine ASP.NET MVC-Projekttyp angeben.

![](mvc3-release-notes/_static/image5.png)

Unterstützung zum Ändern der Liste der Vorlagen und Module im Dialogfeld aufgeführt ist in dieser Version enthalten.

Die Standardvorlagen sind folgende:

Leer. Enthält einen minimalen Satz von Dateien für eine ASP.NET MVC-Projekt, z. B. die standardmäßige Verzeichnisstruktur für ASP.NET MVC-Projekte, eine "Site.CSS" ändern-Datei, enthält die Standard-Formatvorlagen, ASP.NET MVC und Skripts Verzeichnisse, die die Standard-JavaScript-Dateien enthält.

Internetanwendung. Enthält Beispiel-Funktionen, die den Mitgliedschaftsanbieter mit ASP.NET MVC veranschaulicht.

Die Liste der Projektvorlagen, die Sie im Dialogfeld angezeigt wird, wird in der Windows-Registrierung angegeben.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Sessionless-Controller

Die neue *ControllerSessionStateAttribute* bietet Ihnen mehr Kontrolle über das Verhalten des Sitzungszustands für Controller durch Angabe einer [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) -Enumerationswert.

Das folgende Beispiel veranschaulicht das Deaktivieren des Sitzungsstatus für alle Anforderungen an einen Controller.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Im folgende Beispiel wird gezeigt, wie nur-Lese Sitzungsstatus für alle Anforderungen für einen Controller eingerichtet.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Neue Validierungsattribute

#### <a name="compareattribute"></a>CompareAttribute

Die neue *CompareAttribute* Validierungsattribut können Sie die Werte von zwei verschiedene Eigenschaften eines Modells zu vergleichen. Im folgenden Beispiel die *ComparePassword* Eigenschaft muss übereinstimmen. die *Kennwort* Feld, damit es gültig ist.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Die neue *RemoteAttribute* Validierungsattribut nutzt die jQuery Validation-Plug-ins remotebestätigung dadurch die clientseitige Validierung zum Aufrufen einer Methode auf dem Server, der die tatsächliche Überprüfungslogik ausführt.

Im folgenden Beispiel die *Benutzername* Eigenschaft verfügt über die *RemoteAttribute* angewendet. Wenn Sie diese Eigenschaft in einer Bearbeitungsansicht bearbeiten, ruft die Clientvalidierung eine Aktion, die mit dem Namen *UserNameAvailable* auf die *UsersController* Klasse, um dieses Feld zu überprüfen.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Das folgende Beispiel zeigt den entsprechenden Controller an.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Standardmäßig ist der Eigenschaftenname, der das Attribut angewendet wird als Abfragezeichenfolgen-Parameter an die Aktionsmethode gesendet.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Neue Überladungen für "labelFor zurück" und "LabelForModel"-Methoden

Neue Überladungen wurden hinzugefügt, für die *LabelFor* und *LabelForModel* Methoden, mit denen Sie angeben, den Bezeichnungstext. Das folgende Beispiel zeigt, wie diese Überladungen zu verwenden.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Untergeordnete Aktion Zwischenspeichern der Ausgabe

Die *OutputCacheAttribute* unterstützt Zwischenspeichern der Ausgabe des untergeordneten Aktionen, die aufgerufen werden, mithilfe der *Html.RenderAction* oder *Html.Action* Hilfsmethoden. Das folgende Beispiel zeigt eine Ansicht, die eine andere Aktion aufruft.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

Die *GetDate* Aktion mit versehen ist die *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Wenn dieser Code ausgeführt wird, wird das Ergebnis des Aufrufs von Html.Action("GetDate") 100 Sekunden lang zwischengespeichert.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Dialogfeld Feld-Verbesserungen "Ansicht hinzufügen"

Wenn Sie eine stark typisierte Ansicht hinzufügen, filtert das Dialogfeld "Ansicht hinzufügen" jetzt weitere nicht anwendbar Typen als in früheren Versionen, z. B. viele Core .NET Framework-Typen. Darüber hinaus wird die Liste jetzt sortiert, durch den Klassennamen und nicht durch den vollqualifizierten Typnamen, die leichter Typen gefunden werden kann. Der Typname wird z. B. jetzt wie im folgenden Beispiel angezeigt:

Klassenname (Namespace)

In früheren Versionen würde dies wie folgt angezeigt:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Differenzierte Anforderungsvalidierung

Die *ausschließen* Eigenschaft *"validateinputattribute"* nicht mehr vorhanden ist. Um die Anforderungsvalidierung für bestimmte Eigenschaften eines Modells während der modellbindung übersprungen haben, verwenden Sie stattdessen die neuen *SkipRequestValidationAttribute*.

Nehmen wir beispielsweise an, dass eine Aktionsmethode so bearbeiten Sie einen Blogbeitrag verwendet wird:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Das folgende Beispiel zeigt das Ansichtsmodell für einen Blogbeitrag.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Wenn ein Benutzer einige Markup für die Description-Eigenschaft übermittelt, schlägt wurden die modellbindung fehl, weil die Anforderungsvalidierung aktiviert ist. Um Anforderungsvalidierung während der modellbindung für den Blogbeitrag Beschreibung zu deaktivieren, gelten die *SkipRequpestValidationAttribute* der Eigenschaft wie im folgenden Beispiel gezeigt:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Alternativ zum Deaktivieren der anforderungsüberprüfung für jede Eigenschaft des Modells anwenden *"validateinputattribute"* mit einem Wert von *"false"* an die Aktionsmethode:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

- Die Ausführungsreihenfolge für Ausnahmefilter wurde geändert, für die Ausnahmefilter, die dieselbe *Reihenfolge* Wert. In ASP.NET MVC 2 und früheren Ausnahmefilter auf dem Controller an, die die gleiche mussten *Reihenfolge* wie denen auf eine Aktionsmethode vor die Ausnahmefilter auf die Aktionsmethode ausgeführt wurden. Dies würde i. d. r. der Fall sein, wenn Ausnahmefilter angewendet wurden ohne angegebenen *Reihenfolge* Wert. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, damit der spezifischste Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen Wenn die *Reihenfolge* explizit angegeben wird, werden die Filter in der angegebenen Reihenfolge ausgeführt werden.
- Eine neue Eigenschaft mit dem Namen hinzugefügt *FileExtensions* auf die *VirtualPathProviderViewEngine* Basisklasse. Wenn eine Ansicht anhand des Pfads (und nicht anhand des Namens) nur Ansichten, mit der Erweiterung in enthaltenen Nachschlagen wird durch diese neue Eigenschaft angegebene Liste betrachtet. Dies ist eine wichtige Änderung für diejenigen, registrieren einen benutzerdefinierte Buildanbieter um eine benutzerdefinierte Datei-Erweiterung für Formular Webansichten aktivieren und auf diese Ansichten mit einem Namen, statt einen vollständigen Pfad verweisen. Die problemumgehung besteht darin, Ändern des Werts der *FileExtensions* Eigenschaft, um die benutzerdefinierte Erweiterung einschließen.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Bekannte Probleme

- Das Installationsprogramm möglicherweise länger dauert als in vorherigen Versionen von ASP.NET MVC abschließen, da es Komponenten von Visual Studio 2010 aktualisiert werden.
- Das Gerüst Ansicht hinzufügen, bei der Auswahl von Astrongly typisierte Gerüste WriteOnly-Eigenschaften anzeigen. Diese sollen immer durch Gerüstbau ignoriert werden. Das Dialogfeld "Ansicht hinzufügen" Gerüste auch schreibgeschützte Eigenschaften zur Erstellung eine Sicht "Bearbeiten" oder "Erstellen". Schreibgeschützte Eigenschaften sollten nur für die Anzeige und die Listenansichten Gerüstbau sein.
- Debuggen funktioniert nicht bei der Installation von ASP.NET MVC 3 zusammen mit der Async CTP. ASP.NET MVC 3 nebeneinander installiert, mit dem Async-CTP nicht möglich. Deinstallieren Sie die Async-CTP um Debuggen zu reparieren. Weitere Informationen finden Sie unter [diesem Blogbeitrag](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) zur Deinstallation von alle Teile von ASP.NET MVC 3 RC.
- Razor Intellisense funktioniert nicht, wenn Resharper installiert ist. Wenn Sie ReSharper installiert haben und in ASP.NET MVC 3 RC, lesen Sie die Intellisense-Unterstützung für Razor nutzen möchten [diesem Blogbeitrag](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) aus JetBrains die erläutert, weisen diese heute zusammen verwenden.
- CSHTML und VBHTML-Ansichten, die mit der Betaversion von ASP.NET MVC 3 erstellt die Buildaktion ordnungsgemäß keine, die ihnen bei der Veröffentlichung weglässt. Die *Buildvorgang* für diese Dateien auf "Inhalt" festgelegt werden soll. ASP.NET MVC 3 RC behebt dieses Problem für neue Dateien, aber nicht beheben Sie die Einstellung für vorhandene Dateien für ein Projekt mit der Betaversion erstellt wurden.
- Das Installationsprogramm möglicherweise länger dauert als in vorherigen Versionen von ASP.NET MVC abschließen, da es Komponenten von Visual Studio 2010 aktualisiert werden.
- Das Gerüst Ansicht hinzufügen, wenn Ansicht Gerüste wählen eine "Bearbeitung" stark typisiert werden. schreibgeschützte Eigenschaften. Ebenso werden nur-schreiben Eigenschaften für "Anzeige" Ansichten Gerüstbau.
- Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.
- Installieren der Visual Studio Async CTP verursacht einen Konflikt mit der Razor-Version, die als Teil der ASP.NET MVC 3-Installation Tools enthalten ist. Stellen Sie sicher, dass Sie nicht versucht, die Visual Studio-Async-CTP und der Razor-Version auf demselben Computer installieren.
- Bei der Bearbeitung einer Razor-Ansicht (cshtml-Datei), das Menüelement zu Controller wechseln Sie in Visual Studio ist nicht verfügbar, und es sind keine Codeausschnitte.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 Beta

ASP.NET MVC 3 Beta wurde 6 Oktober 2010 veröffentlicht. Die folgenden Hinweise gelten für die Betaversion und unterliegen alle Updates oder Änderungen, die im obigen Abschnitt zu ASP.NET MVC 3 Release Candidate verwiesen.

## <a id="0.1__Toc274034215"></a>  Neue Featuresin ASP.NET MVC 3-Beta

<a id="0.1__Default_validation_system"></a>Dieser Abschnitt beschreibt die Funktionen, die eingeführt wurden in der Betaversion von ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>  NuGet-Paket-Manager

ASP.NET MVC 3 enthält NuGet Package Manager, die ein integriertes Paket Verwaltungstool für Hinzufügen von Bibliotheken und Tools für Visual Studio-Projekte ist. Meistens, automatisiert es die Schritte, die Entwickler noch heute zu ergreifen, um eine Bibliothek in ihrer ursprünglichen Struktur abrufen.

Sie können mit NuGet, als ein Befehlszeilentool, wie einer integrierten Konsolenfenster in Visual Studio 2010, klicken Sie im Kontextmenü der Visual Studio und als Satz von PowerShell-Cmdlets arbeiten.

Weitere Informationen zu NuGet finden Sie unter der [NuGet-Dokumentation](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Verbesserte neues Projekt (Dialogfeld)

Wenn Sie ein neues Projekt erstellen, kann das Dialogfeld "Neues Projekt" jetzt Sie das Ansichtsmodul als auch für eine ASP.NET MVC-Projekttyp angeben.

![](mvc3-release-notes/_static/image6.png)

Unterstützung zum Ändern der Liste der Vorlagen und Module im Dialogfeld aufgeführt ist nicht in dieser Version enthalten.

Die Standardvorlagen sind folgende:

Leer. Enthält einen minimalen Satz von Dateien für eine ASP.NET MVC-Projekt, z. B. die standardmäßige Verzeichnisstruktur für ASP.NET MVC-Projekte, eine kleine "Site.CSS" ändern-Datei, enthält die Standard-Formatvorlagen, ASP.NET MVC und Skripts Verzeichnisse, die die Standard-JavaScript-Dateien enthält.

Internetanwendung. Enthält Beispiel-Funktionen, die den Mitgliedschaftsanbieter in ASP.NET MVC veranschaulicht.

### <a id="0.1__Toc274034218"></a>  Vereinfachte Möglichkeit stark angeben typisierte Modelle in Razor-Ansichten

Wurde die Möglichkeit, geben Sie den Typ des Modells für stark typisierte Razor-Ansichten mithilfe des neuen vereinfacht @model -Direktive für CSHTML-Ansichten und @ModelType -Direktive für VBHTML-Ansichten. In früheren Versionen von ASP.NET MVC würden Sie angeben, dass ein stark typisiertes Modell für die Razor-Ansichten auf diese Weise:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

In dieser Version können Sie die folgende Syntax verwenden:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Unterstützung für neue ASP.NET Web Pages-Hilfsmethoden

Die neue ASP.NET Web Pages-Technologie umfasst einen Satz von Hilfsmethoden, die für das Hinzufügen von häufig verwendeten Funktionen, Ansichten und Controllern nützlich sind. ASP.NET MVC 3 unterstützt diese Hilfsmethoden innerhalb der Controller und Ansichten (falls zutreffend). Diese Methoden sind in der Assembly System.Web.Helpers enthalten. Die folgende Tabelle enthält nur einige der ASP.NET Web Pages-Hilfsmethoden.

| **Helper** | **Beschreibung** |
| --- | --- |
| Diagramm | Rendert ein Diagramm innerhalb einer Ansicht an. Enthält Methoden, wie z. B. Chart.ToWebImage Chart.Save und Chart.Write. |
| Crypto | Verwendet Hashalgorithmen ordnungsgemäß erstellen Salt-Wert und ein Hashwert erstellt Kennwörter. |
| WebGrid | Wird eine Auflistung von Objekten (in der Regel Daten aus einer Datenbank) als Raster gerendert. Unterstützt das paging und sortieren. |
| WebImage | Rendert ein Bild an. |
| WebMail | Sendet eine E-Mail. |

Eine Kurzübersicht Thema mit den Hilfsprogrammen und grundlegende Syntax steht als Teil der Dokumentation des ASP.NET Razor-Syntax unter folgender URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Unterstützung für zusätzliche Abhängigkeiten-Injection

Baut auf der Version von ASP.NET MVC 3 Preview 1 und enthält die aktuelle Version Unterstützung für zwei neue Dienste und vier vorhandenen Dienste und verbesserte Unterstützung für Abhängigkeit Auflösung und die Common Service Locator.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Neue IControllerActivator Benutzeroberfläche für differenzierte-Controller Instanziierung

Die neue IControllerActivator-Schnittstelle bietet eine präzisere Steuerung wie Controller über Abhängigkeitsinjektion instanziiert werden. Das folgende Beispiel zeigt die Schnittstelle:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Vergleichen Sie dies in die Rolle der Controllerfactory. Eine Controllerfactory ist eine Implementierung der IControllerFactory-Schnittstelle, die sowohl für die Suche nach einem Controllertyp und zum Instanziieren einer Instanz dieses Controllertyps verwendet wird.

Controlleraktivatoren sind nur für eine Instanz eines Controllertyps instanziieren verantwortlich. Sie führen nicht die Suche nach Controller-Typ. Nach der Lokalisierung einer richtigen Controllertyp an, sollten Controllerfactorys mit einer Instanz von IControllerActivator delegieren, um die tatsächliche Instanziierung des Controllers zu behandeln.

Die DefaultControllerFactory-Klasse verfügt über einen neuen Konstruktor, der eine Instanz IControllerFactory akzeptiert. Dadurch können Sie die Abhängigkeitsinjektion um dieser Aspekt des Controller-Erstellung zu verwalten, ohne das Standardverhalten für Controllertyp Suche überschreiben anwenden.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>IServiceLocator-Schnittstelle, die durch IDependencyResolver ersetzt

Die Betaversion von ASP.NET MVC 3 wurde basierend auf Communityfeedback, die Verwendung der IServiceLocator-Schnittstelle mit einer bestimmten auf die Bedürfnisse von ASP.NET MVC abgespeckte IDependencyResolver Schnittstelle ersetzt. Das folgende Beispiel zeigt die neue Benutzeroberfläche:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Im Rahmen dieser Änderung wurde die ServiceLocator-Klasse auch mit der Klasse DependencyResolver ersetzt. Ein Abhängigkeitskonfliktlöser Registrierung ist vergleichbar mit früheren Versionen von ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementierungen dieser Schnittstelle sollten einfach an die zugrunde liegenden abhängigkeitsinjektionscontainer delegieren, um den registrierten Dienst für den angeforderten Typ bereitzustellen.

Wenn keine registrierten Dienste des angeforderten Typs vorhanden sind, erwartet ASP.NET-MVC Implementierungen dieser Schnittstelle aus GetService null zurückgegeben und von "GetServices" auf eine leere Auflistung zurückgegeben.

Die neue DependencyResolver-Klasse können Sie die Klassen zu registrieren, die der neuen IDependencyResolver-Schnittstelle oder die Common Service Locator-Schnittstelle (IServiceLocator) zu implementieren. Weitere Informationen zum Common Service Locator finden Sie unter [CommonServiceLocator auf GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Neue IViewActivator Benutzeroberfläche für differenzierte Ansicht Seite Instanziierung

Die neue IViewPageActivator-Schnittstelle bietet eine präzisere Steuerung wie Ansichtsseiten über Abhängigkeitsinjektion instanziiert werden. Dies gilt für Instanzen von WebFormView und RazorView-Instanzen. Das folgende Beispiel zeigt die neue Benutzeroberfläche:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Diese Klassen akzeptieren jetzt IViewPageActivator Konstruktorargument, und Sie können Sie die Abhängigkeitsinjektion verwenden steuern, wie die ViewPage, ViewUserControl und WebViewPage-Typen instanziiert werden.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Neue Abhängigkeit Resolver-Unterstützung für vorhandene Dienste

Die neue Version umfasst Unterstützung für vollbildauflösung Abhängigkeit für die folgenden Dienste:

- Modell Validierungsanbieter. Klassen, die ModelValidatorProvider implementieren, die in den Abhängigkeitskonfliktlöser registriert werden können, und das System verwendet diese zur Client- und serverseitige Validierung unterstützen.
- Metadatenanbieter für Modelle. Eine einzelne Klasse, die ModelMetadataProvider implementiert, die in den Abhängigkeitskonfliktlöser registriert werden kann, und das System wird verwendet, um Metadaten für die Systeme Datenvorlagen und Validierung bereitzustellen.
- Der Wertanbieter. Klassen, die ValueProviderFactory implementieren, die in den Abhängigkeitskonfliktlöser registriert werden können, und das System verwendet diese zur Wertanbieter zu erstellen, die vom Controller und während der modellbindung genutzt werden.
- Modellbinder. Klassen, die IModelBinderProvider implementieren, die in den Abhängigkeitskonfliktlöser registriert werden können, und das System verwendet diese zur Modellbinder erstellen, die vom Modell Bindungssystem genutzt werden.

### <a id="0.1__Toc274034221"></a>  Neue Unterstützung für unaufdringliche jQuery-basierte Ajax

ASP.NET MVC umfasst Ajax-Hilfsmethoden, z. B. Folgendes an:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Diese Methoden verwenden JavaScript zum Aufrufen einer Aktionsmethode auf dem Server statt verwenden ein komplettes Postback. Diese Funktionalität wurde aktualisiert, um die jQuery in einer unaufdringlichen Weise nutzen. Anstatt intrusively ausgeben Client Inlineskripts, trennen mit diesen Hilfsmethoden das Verhalten von der Markuperweiterung durch das Ausgeben von HTML5-Attribute, die mit der *Data-Ajax* Präfix. Verhalten wird dann dem Markup durch Verweisen auf die entsprechenden JavaScript-Dateien angewendet. Stellen Sie sicher, dass die folgenden JavaScript-Dateien verwiesen werden:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Diese Funktion ist standardmäßig in der Datei "Web.config" in der ASP.NET MVC 3-Vorlagen für neuen Projekte aktiviert, aber es ist für vorhandene Projekte standardmäßig deaktiviert. Weitere Informationen finden Sie unter [hinzugefügt anwendungsweite Flags für die Clientvalidierung und unaufdringliches JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) weiter unten in diesem Dokument.

### <a id="0.1__Toc274034222"></a>  Neue Unterstützung für unaufdringliche jQuery-Validierung

Standardmäßig verwendet ASP.NET MVC 3 Beta jQuery-Validierung automatisierbar unaufdringlichen aus, um die clientseitige Überprüfung auszuführen. Stellen Sie einen Aufruf wie den folgenden innerhalb einer Ansicht, um unaufdringliche Clientvalidierung zu aktivieren:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Dies erfordert, dass ViewContext.UnobtrusiveJavaScriptEnabled-Eigenschaft auf "true" festgelegt ist, wozu Sie verwenden können, die den folgenden Aufruf:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Stellen Sie außerdem sicher, dass die folgenden JavaScript-Dateien verwiesen werden.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Diese Funktion ist standardmäßig in der Datei "Web.config" in Vorlagen für neue ASP.NET MVC 3-Projekte auf aktiviert, aber es ist für vorhandene Projekte standardmäßig deaktiviert. Weitere Informationen finden Sie unter [neue anwendungsweite-Flags für die Clientvalidierung und unaufdringliches JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) weiter unten in diesem Dokument.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  Neue anwendungsweite-Flags für die Clientvalidierung und unaufdringliches JavaScript

Sie können aktivieren oder deaktivieren die Clientvalidierung und unaufdringliches JavaScript global über statische Member der HtmlHelper-Klasse, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Die Standard-Projektvorlagen aktiviert unaufdringliches JavaScript standardmäßig. Sie können auch aktivieren oder deaktivieren Sie diese Funktionen sollten in der Stammdatei "Web.config" Ihrer Anwendung mithilfe der folgenden Einstellungen:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Da Sie diese Funktionen standardmäßig aktivieren können, wurden neue Überladungen für die Klasse HtmlHelper eingeführt, mit die Sie die Standardeinstellungen außer Kraft setzen, wie in den folgenden Beispielen gezeigt können:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Um Abwärtskompatibilität zu gewährleisten werden beide Funktionen standardmäßig deaktiviert.

### <a id="0.1__Toc274034224"></a>  Neue Unterstützung für Code, der vor der Ausführung von Ansichten ausgeführt wird.

Sie können jetzt speichern Sie eine Datei mit dem Namen \_viewstart.cshtml (oder \_viewstart.vbhtml) im Verzeichnis "Views" und fügen Sie Code hinzu, die für mehrere Ansichten in diesem Verzeichnis und seinen Unterverzeichnissen freigegeben wird. Sie können z. B. Legen Sie den folgenden Code in die \_viewstart.cshtml Seite im Ordner "~/Views":

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Hiermit wird die Seite "Layout" für jede Ansicht in den Ordner Views und alle Unterordner rekursiv. Wenn eine Ansicht gerendert wird, den Code in der \_viewstart.cshtml-Datei ausgeführt wird, bevor der Code für die Sicht ausgeführt wird. Die \_viewstart.cshtml Code gilt für jede Ansicht in diesem Ordner.

Standardmäßig wird der Code in der \_viewstart.cshtml Datei gilt auch für Sichten in jedem Unterordner. Einzelne Unterordner können jedoch ihre eigene Version der haben die \_viewstart.cshtml Datei; in dieser Fall ist die lokale Version Vorrang. Um Code auszuführen, die auf alle Ansichten für die HomeController gemein sind, z. B. legen eine \_viewstart.cshtml-Datei im Ordner "~/Views/Home".

### <a id="0.1__Toc274034225"></a>  Neue Unterstützung für die VBHTML-Razor-Syntax

Die vorherigen ASP.NET MVC-Vorschau enthalten Unterstützung für Ansichten mit Razor-Syntax, die basierend auf c#. Diese Sichten verwenden die CSHTML-Erweiterung. Im Rahmen der derzeit ausgeführte Arbeit Razor unterstützen führt die Betaversion von ASP.NET MVC 3-Unterstützung für die Razor-Syntax in Visual Basic, die Erweiterung der vbhtml-Datei wird verwendet.

Eine Einführung zur Verwendung von Visual Basic-Syntax in VBHTML-Seiten finden Sie im Lernprogramm unter folgender URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  Präzisere Kontrolle über "validateinputattribute"

ASP.NET MVC hat immer die Klasse "validateinputattribute" enthalten, die der ASP.NET Anforderung Überprüfung Kerninfrastruktur um sicherzustellen, dass die eingehende Anforderung keine potenziell schädlicher Eingaben enthält aufruft. Validierung von Benutzereingaben ist standardmäßig aktiviert. Es ist möglich, die Anforderungsvalidierung zu deaktivieren, indem Sie mit dem "validateinputattribute"-Attribut, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Viele Webanwendungen müssen jedoch einzelne Formularfelder, die HTML-Code zu ermöglichen, während die übrigen Felder nicht enthalten sein sollen. Die Klasse "validateinputattribute" können jetzt eine Liste der Felder angeben, die bei der Anforderungsvalidierung nicht enthalten sein sollten.

Wenn Sie eine Blog-Engine entwickeln, sollten Sie z. B. Markup in den Feldern Text und Zusammenfassung zu ermöglichen. Diese Felder können durch zwei input-Element, jeweils ein Name-Attribut entspricht der Eigenschaftenname ("Text" und "Zusammenfassung") dargestellt werden. Um die Anforderung zu deaktivieren Überprüfung für diese Felder nur die Namen angeben (durch Trennzeichen getrennt) in der Exclude-Eigenschaft der ValidateInput-Klasse, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Hilfsprogramme konvertieren Unterstriche, Bindestriche für die Namen der HTML-Attribute mit anonymer Objekte angegeben

Hilfsmethoden können Sie ein anonymes Objekt, wie im folgenden Beispiel mit Attribut-Wert-Paare angeben:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Dieser Ansatz lassen nicht Sie die Bindestriche in den Attributnamen verwenden, da ein Bindestrich für einen Eigenschaftsnamen in ASP.NET verwendet werden kann. Bindestriche sind jedoch wichtig für benutzerdefinierte HTML5-Attribute. HTML5 verwendet beispielsweise das Präfix "Daten-".

Zur gleichen Zeit Unterstriche können nicht für die Namen der Attribute im HTML-Format verwendet werden, aber in Eigenschaftsnamen gültig sind. Aus diesem Grund werden bei Angabe von Attributen, die mit einem anonymen Objekt und die Attributnamen Unterstrich einschließen, Hilfsmethoden die Unterstriche, Bindestriche konvertieren. Die folgende Syntax der Hilfsprogramm verwendet z. B. einen Unterstrich:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Im vorherige Beispiel wird das folgende Markup gerendert, wenn das Hilfsprogramm ausgeführt wird:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Fehlerkorrekturen

Objekt Standardvorlage für Hilfsprogramme Vorlage EditorFor und DisplayFor unterstützt jetzt die Reihenfolge in der DisplayAttribute.Order-Eigenschaft angegeben. (In früheren Versionen war die Einstellung für die Reihenfolge nicht verwendet.)

Jetzt Clientvalidierung unterstützt die Überprüfung von außer Kraft gesetzten Eigenschaften, die Validierungsattribute angewendet haben.

JsonValueProviderFactory ist jetzt standardmäßig registriert.

## <a id="0.1__Toc274034229"></a>  Wichtige Änderungen

Die Ausführungsreihenfolge für Ausnahmefilter wurde für Ausnahmefilter geändert, die über denselben Reihenfolgenwert verfügen. In ASP.NET MVC 2 und früheren filtert Ausnahme auf dem Controller mit der gleichen Reihenfolge wie denen auf eine Aktionsmethode vor die Ausnahmefilter auf die Aktionsmethode ausgeführt wurden. Dies würde i. d. r. der Fall sein, wenn Ausnahmefilter ohne einen Reihenfolgenwert der angegebenen angewendet wurden. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, damit der spezifischste Ausnahmehandler zuerst ausgeführt wird. Wenn die Order-Eigenschaft explizit angegeben wird, werden die Filter wie in früheren Versionen in der angegebenen Reihenfolge ausgeführt.

## <a id="0.1__Toc274034230"></a>  Bekannte Probleme

Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.

Razor-Ansichten haben keine IntelliSense-Unterstützung noch syntaxhervorhebung. Es wird erwartet, dass Unterstützung für Razor-Syntax in Visual Studio als Teil einer späteren Version enthalten kann.

Bei der Bearbeitung einer Razor-Ansicht (CSHTML-Datei), die <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> Menüelement in Visual Studio wechseln Sie zu Controller ist nicht verfügbar, und es sind keine Codeausschnitte.

Bei Verwendung der @model Syntax zum Angeben von einer stark typisierten CSHTML anzeigen, die sprachspezifische Tastenkombinationen für Typen werden nicht erkannt. Beispielsweise @model Int funktioniert nicht, aber @model Int32 funktioniert. Die problemumgehung für diesen Fehler ist die Verwendung von den tatsächlichen Typnamen, bei der Angabe von Typ des Modells.

Bei Verwendung der @model Syntax zum Angeben von einer stark typisierten CSHTML-Ansicht (oder @ModelType an einer stark typisierten Ansicht der VBHTML), auf NULL festlegbare Typen und Arraydeklarationen werden nicht unterstützt. Beispielsweise @model Int? wird nicht unterstützt. Verwenden Sie stattdessen `@model Nullable<Int32>`. Die Syntax @model String [] ist ebenfalls nicht unterstützt; verwenden Sie stattdessen `@model IList<string>`.

Beim upgrade eines ASP.NET MVC 2-Projekts auf ASP.NET MVC 3 Stellen Sie sicher, dass im folgenden Abschnitt "appSettings" der Datei "Web.config" hinzugefügt:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Es ist ein bekanntes Problem, das führt dazu, dass bei der Formularauthentifizierung für nicht authentifizierte Benutzer immer ~/Account/Anmeldung ignorieren die Einstellung für die Formularauthentifizierung verwendet, die in "Web.config" umleiten. Die problemumgehung besteht darin, die folgenden app-Einstellung hinzufügen.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Haftungsausschluss

© 2011 Microsoft Corporation. Alle Rechte vorbehalten. Dieses Dokument wird bereitgestellt "als-ist." Informationen und Ansichten, ausgedrückt in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Sie tragen das alleinige Verwendungsrisiko.

Dieses Dokument stellt keinerlei Rechtsansprüche auf geistiges Eigentum in Microsoft-Produkten jeglicher Art bereit. Sie dürfen Dokument für interne Informationszwecke kopieren.
