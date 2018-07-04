---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Microsoft-Dokumentation
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: ''
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: fe3536f3a40f3a74df47bcc1ca0d0b8416895169
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381677"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Übersicht](#overview)
- [Installationshinweise](#installation-notes)
- [Softwareanforderungen](#software-requirements)
- [Dokumentation](#documentation)
- [Unterstützung](#support)
- [Aktualisieren eines ASP.NET MVC 2-Projekts zu ASP.NET MVC Update 3 Tools](#upgrading)
- [ASP.NET MVC 3 Toolsupdate (12. April 2011)](#tu-changes)

    - [Dialogfeld "Controller hinzufügen" kann jetzt Controller mit Ansichten und Data Access Code aufbauen.](#tu-AddControllerDialog)
    - [Verbesserungen an der "ASP.NET MVC 3 neues Projekt" im Dialogfeld](#tu-ImprovementsNewDialogBox)
    - [Neue Projektvorlage: Modernizr 1.7](#tu-Modernizr)
    - [Neue Projektvorlagen: aktualisierte Versionen von jQuery, jQuery UI und jQuery Validation](#tu-UpdatedJQuery)
    - [Neue Projektvorlage: ADO.NET Entity Framework 4.1 als vorinstallierte NuGet-Paket](#tu-EF)
    - [Neue Projektvorlagen: JavaScript-Bibliotheken als vorinstallierte NuGet-Pakete](#tu-JavaScriptLibsNuget)
    - [Bekannte Probleme](#tu-KI)
- [ASP.NET MVC 3 RTM (13 Januar 2011)](#MVC3RTM)

    - [Änderung: Die Version von jQuery-Benutzeroberfläche auf 1.8.7 aktualisiert](#RTM-1)
    - [Ändern: Die Standardeinstellungen, die ModelMetadataProvider an DataAnnotationsModelMetadataProvider geändert](#RTM-2)
    - [Behoben: Einfügen Teil eines Ausdrucks Razor, die Leerzeichen Ergebnisse darin wird umgekehrt enthält](#RTM-3)
    - [Behoben: Beim Umbenennen einer Razor-Datei, die im Editor geöffnet wird deaktiviert farbige syntaxhervorhebung und IntelliSense](#RTM-4)
    - [Bekannte Probleme](#RTM-KI)
    - [Wichtige Änderungen](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10. Dezember 2010)](#_Toc2)

    - [Projekt-Vorlagen geändert, jQuery 1.4.4, jQuery-Validierung 1.7 und jQuery UI 1.8.6y UI 1.8.6 einschließen](#_Toc2_1)
    - [Hinzugefügte "AdditionalMetadataAttribute"-Klasse](#_Toc2_2)
    - [Verbesserte Ansicht Gerüstbau](#_Toc2_3)
    - [Hinzugefügt von Html.Raw-Methode](#_Toc2_3)
    - [Umbenannte "Controller.ViewModel"-Eigenschaft und die "View"-Eigenschaft auf "ViewBag"](#_Toc2_4)
    - [Umbenannte "ControllerSessionStateAttribute" Klasse "SessionStateAttribute"](#_Toc2_5)
    - [Umbenannte RemoteAttribute "Felder"-Eigenschaft auf "AdditionalFields"](#_Toc2_6)
    - ["SkipRequestValidationAttribute", "AllowHtmlAttribute" umbenannt](#_Toc2_7)
    - [Geänderte "Html.ValidationMessage"-Methode, um die erste nützliche Fehlermeldung anzuzeigen.](#_Toc2_8)
    - [Feste @model Deklaration Leerzeichen nicht auf das Dokument hinzufügen](#_Toc2_9)
    - [Hinzugefügte "FileExtensions"-Eigenschaft, um den Ansichts-Engines zur Unterstützung der Engine-spezifische Dateinamen](#_Toc2_10)
    - [Feste "LabelFor"-Hilfsprogramm, den richtigen Wert für das Attribut "For" auszugeben.](#_Toc2_11)
    - [Feste "RenderAction"-Methode, um explizite Werte Vorrang vor, während der Modellbindung zu gewähren.](#_Toc2_12)
    - [Wichtige Änderungen](#_Toc2_BC)
    - [Bekannte Probleme](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9. November 2010)](#TOC_ASP_NET_3_RC)

    - [Neue Features in ASP.NET MVC 3 RC](#_Toc276711785)
    - [NuGet-Paket-Manager](#_Toc276711786)
    - [Verbesserte "New Project" (Dialogfeld)](#_Toc276711787)
    - [Sessionless-Controller](#_Toc276711788)
    - [Neue Validierungsattribute](#_Toc276711789)
    - [Neue Überladungen für "LabelFor" und "LabelForModel"-Methoden](#_Toc276711790)
    - [Untergeordnete Aktion Zwischenspeichern der Ausgabe](#_Toc276711791)
    - ["Add View" Dialogfeld Verbesserungen an](#_Toc276711792)
    - [Differenzierte Anforderungsvalidierung](#_Toc276711793)
    - [Wichtige Änderungen](#_Toc276711794)
    - [Bekannte Probleme](#_Toc276711795)
- [ASP. MVC 3 – Anmerkungen dieser Betaversion (6. Oktober 2010)](#TOC_ASP_NET_3_Beta)

    - [Neue Features in ASP.NET MVC 3-Beta](#0.1__Toc274034215)
    - [NuPack-Paket-Manager](#0.1__Toc274034216)
    - [Verbesserte neues Projekt (Dialogfeld)](#0.1__Toc274034217)
    - [Vereinfachte Methode, geben Sie stark typisierte Modelle in Razor-Ansichten](#0.1__Toc274034218)
    - [Unterstützung für neue ASP.NET Web Pages-Hilfsmethoden](#0.1__Toc274034219)
    - [Unterstützung der Einfügung von zusätzlichen Abhängigkeit](#0.1__Toc274034220)
    - [Neue Unterstützung für die unaufdringliche jQuery-basierten Ajax](#0.1__Toc274034221)
    - [Neue Unterstützung für die unaufdringliche jQuery-Validierung](#0.1__Toc274034222)
    - [Neue anwendungsweite-Flags für die Clientvalidierung und unaufdringliches JavaScript](#0.1__Toc274034223)
    - [Neue Unterstützung für Code, bevor die Ausführung von Ansichten ausgeführt wird.](#0.1__Toc274034224)
    - [Neue Unterstützung für die VBHTML-Razor-Syntax](#0.1__Toc274034225)
    - [Mehr Kontrolle über ValidateInputAttribute](#0.1__Toc274034226)
    - [Hilfsprogramme und konvertieren Unterstriche zu Bindestrichen für HTML-Attributnamen, die mit der anonyme Objekte angegeben](#0.1__Toc274034227)
    - [Fehlerbehebungen](#0.1__Toc274034228)
    - [Wichtige Änderungen](#0.1__Toc274034229)
    - [Bekannte Probleme](#0.1__Toc274034230)
- [Haftungsausschluss](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Übersicht

Dieses Dokument beschreibt die Version von ASP.NET MVC 3 RTM für Visual Studio 2010. ASP.NET MVC ist ein Framework zum Entwickeln von Webanwendungen, die das Muster Model-View-Controller (MVC) verwendet. Das ASP.NET MVC 3-Installationsprogramm umfasst die folgenden Komponenten:

- ASP.NET MVC 3-Laufzeitkomponenten
- ASP.NET MVC 3 Visual Studio 2010-tools
- ASP.NET Web Pages-Laufzeitkomponenten
- ASP.NET Web Pages Visual Studio 2010-tools
- Microsoft Paket-Manager für .NET (NuGet)
- Ein Update für Visual Studio 2010, die Unterstützung für Razor-Syntax ermöglicht. (Informationen finden Sie im Knowledge Base-Artikel 2483190.)

Der vollständige Satz von Anmerkungen zu dieser Version für den Vorabversionen von ASP.NET MVC 3 finden Sie auf der ASP.NET-Website unter folgender URL:

https://www.asp.net/learn/whitepapers/mvc3-release-notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Installationshinweise

Um ASP.NET MVC 3 RTM mit dem Webplattform-Installer (Web PI) installieren zu können, finden Sie auf der folgenden Seite:

[https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Alternativ können Sie das Installationsprogramm für ASP.NET MVC 3 RTM für Visual Studio 2010 von der folgenden Seite herunterladen:

https://go.microsoft.com/fwlink/?LinkID=208140

ASP.NET MVC 3 installiert werden kann, und führe die Seite-an-Seite mit ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Softwareanforderungen

Die ASP.NET MVC 3-Laufzeitkomponenten benötigen die folgende Software:

- .NET Framework, Version 4. 

    ASP.NET MVC 3 Visual Studio 2010-Tools benötigen die folgende Software:
- Visual Studio 2010 noch Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentation

Dokumentation für ASP.NET MVC ist auf der MSDN-Website unter der folgenden URL verfügbar:

[https://go.microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Lernprogramme und Weitere Informationen zu ASP.NET MVC sind auf der MVC-Seite der ASP.NET-Website unter folgender URL verfügbar:

[https://www.asp.net/mvc/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Unterstützung

Dies ist eine vollständig unterstützte Version. Informationen zur Inanspruchnahme technischer Unterstützung finden Sie unter den [Microsoft Support-Website](https://support.microsoft.com/).

Können Sie auch Fragen zu dieser Version im ASP.NET MVC-Forum, veröffentlichen, in dem Mitglieder der ASP.NET-Community informelle Unterstützung bieten. oftmals können sind:

[https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Aktualisieren eines ASP.NET MVC 2-Projekts zu ASP.NET MVC Update 3 Tools

ASP.NET MVC 3 kann auf demselben Computer ausführen, wodurch Sie die Flexibilität bei der Entscheidung, wann eine ASP.NET MVC 2-Anwendung auf ASP.NET MVC 3 aktualisieren, parallel zu ASP.NET MVC 2 installiert werden.

Um eine vorhandene ASP.NET MVC 2-Anwendung auf Version 3 manuell zu aktualisieren, führen Sie folgende Schritte aus:

1. Erstellen Sie ein neues, leeres ASP.NET MVC 3-Projekt auf Ihrem Computer. Dieses Projekt enthält einige Dateien, die für das Upgrade erforderlich sind.
2. Kopieren Sie die folgenden Dateien aus dem ASP.NET MVC 3-Projekt, in die entsprechende Position Ihres ASP.NET MVC 2-Projekts. Sie müssen alle Verweise auf die jQuery-Bibliothek für den neuen Dateinamen (jQuery-1.5.1.js) berücksichtigen zu aktualisieren: 

    - /Views/Web.config
    - /packages.config
    - /scripts/\*.js
    - /Inhalt/Designs/\*.\*
3. Kopieren der *Pakete* Ordner im Stammverzeichnis der leeren ASP.NET MVC 3-Projektmappe in das Stammverzeichnis der Projektmappe, die im Verzeichnis befindet, wo sich die SLN Datei befindet.
4. Wenn Ihr ASP.NET MVC 2-Projekt irgendwelche Bereiche enthält, kopieren Sie die Datei /Views/Web.config der *Ansichten* Ordner jedes Bereichs.
5. In beiden Web.config-Dateien in das ASP.NET MVC 2-Projekt Global suchen Sie und Ersetzen Sie die ASP.NET MVC-Version. Suchen Sie nach: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Ersetzen Sie ihn durch Folgendes:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. Löschen Sie im Projektmappen-Explorer den Verweis auf *System.Web.Mvc* (welche verweist auf die DLL von Version 2), dann fügen einen Verweis auf *System.Web.Mvc* (v3.0.0.0).
7. Fügen Sie einen Verweis auf System.Web.WebPages.dll und auf System.Web.helpers.dll hinzu. Diese Assemblys befinden sich in den folgenden Ordnern: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Namens des Projekts, und wählen Sie die Projekt entladen. Klicken Sie dann mit der rechten Maustaste erneut auf des Projektnamens, und wählen Sie bearbeiten *ProjectName*csproj.
9. Suchen Sie die *ProjectTypeGuids* Element, und Ersetzen Sie {F85E285D-A4E0-4152-9332-AB1D724D3325} mit {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Speichern Sie die Änderungen, mit der rechten Maustaste in des Projekts, und wählen Sie dann das Projekt erneut laden.
11. Fügen Sie in der Stammdatei "Web.config" der Anwendung die folgenden Einstellungen aus, um die *Assemblys* Abschnitt. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Wenn das Projekt Drittanbieter Bibliotheken, die mit ASP.NET MVC 2 kompiliert werden verweist, fügen Sie die folgende hervorgehobene *BindingRedirect* Element in der Datei Web.config im Stammverzeichnis Anwendung, unter dem  *Konfiguration* Abschnitt: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Änderungen in ASP.NET MVC 3 Tools aktualisieren.

Dieser Abschnitt beschreibt die Änderungen, die seit der ASP.NET MVC 3 RTM-Version in der ASP.NET MVC 3 Tools Update-Version.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Dialogfeld "Controller hinzufügen" kann jetzt Controller mit Ansichten und Data Access Code aufbauen.

Gerüstbau ist eine Möglichkeit, schnell einen Controller und Ansichten für Ihre Anwendung zu generieren. Nachdem der Code generiert wurde, können Sie sie entsprechend den Anforderungen Ihres Projekts bearbeiten.

Zum Starten der *Controller hinzufügen* mit der rechten Maustaste der in ASP.NET MVC 3, im Dialogfeld die *Controller* Ordner *Projektmappen-Explorer*, klicken Sie auf *hinzufügen*, und klicken Sie dann auf *Controller*. Das Dialogfeld wurde verbessert, um zusätzliche gerüstoptionen bieten.

![](mvc3-release-notes/_static/image1.png)

Standardmäßig sind drei folgenden gerüstbauvorlagen zur Verfügung.

#### <a name="empty-controller"></a>Leerer Controller

Diese Vorlage erzeugt eine leere controllerdatei. Diese Vorlage entspricht, das keine Überprüfung auf *Aktionen für Erstell-, Aktualisierungs- und Details hinzufügen, löschen Sie die Szenarien* in früheren Versionen von ASP.NET MVC. Wenn Sie diese Vorlage auswählen, sind keine weiteren Optionen verfügbar.

#### <a name="controller-with-empty-readwrite-actions"></a>Controller mit leeren Lese-/schreibaktionen

Diese Vorlage erzeugt eine controllerdatei, die alle erforderlichen Aktionsmethoden jedoch keinen Implementierungscode in den Methoden verfügt. Diese Vorlage entspricht dem überprüfen *Aktionen für Erstell-, Aktualisierungs- und Details hinzufügen, löschen Sie die Szenarien* in früheren Versionen von ASP.NET MVC. Wenn Sie diese Vorlage auswählen, sind keine weiteren Optionen verfügbar.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controller mit Lese-/schreibaktionen und Ansichten unter Verwendung von Entity Framework

Mit dieser Vorlage können Sie schnell eine funktionierende Dateneingabe-Benutzeroberfläche zu erstellen. Generiert Code, der eine Reihe von allgemeine Anforderungen und Szenarien, z. B. Folgendes behandelt:

- *Datenzugriff*. Der generierte Code liest und schreibt Entitäten in einer Datenbank. Mit dem Entity Framework Code First-Ansatz funktioniert, wenn Sie eine bestehende Datenkontextklasse auswählen oder wenn Sie zulassen, dass die Vorlage eine neue *"DbContext"* Klasse. Es funktioniert auch mit dem Entity Framework Database First oder Model First-Ansatz wählen Sie eine vorhandene *ObjectContext* Klasse.
- *Überprüfung*. Der generierte Code verwendet ASP.NET MVC-modellbindung und Metadatenfunktionen, sodass übermittelte Formulare entsprechend in Ihrer Modellklasse deklarierten Regeln überprüft werden. Dazu zählen integrierte Validierungsregeln wie das *erforderlich* und *StringLength* Attribute und benutzerdefinierte Validierungsregeln.
- *1: n Beziehungen*. Wenn Sie eine 1: n Fremdschlüssel-Beziehungen zwischen Ihren Modellklassen definieren, wird der generierte Code Dropdownlisten zum Auswählen von verknüpften Entitäten erstellt. Beispielsweise können Sie die folgenden Modellklassen Entity Framework Code First-Konventionen definieren: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Wenn dann Gerüst ein Controllers für die *Produkt* -Klasse, die Ansichten können Benutzer wählen Sie eine *Kategorie* Objekt für die einzelnen *Produkt* Instanz.

    Mit dieser Vorlage können Sie zusätzliche Optionen in der *Controller hinzufügen* Dialogfeld. Für *Modellklasse*, Sie können jede beliebige Modellklasse wählen Sie in der Projektmappe, die den Typ der Daten bestimmt, die Benutzer werden zum Erstellen oder bearbeiten können:
- Wenn Sie Entity Framework Code First verwenden möchten, können Sie jede beliebige Modellklasse auswählen.
- Wenn Sie Entity Framework Database First oder Entity Framework Model First verwenden, achten Sie darauf, dass Sie eine im konzeptionellen Modell definierte entitätenklasse zu wählen.

Für *Datenkontext Klasse*, Sie können diese Optionen machen:

- Wenn Sie möchten Code First verwenden und keine vorhandenen Datenkontext Klasse, und wählen Sie ** neuer Datenkontext **. Dann wird eine neue Datenkontextklasse für Sie generiert.
- Wenn Sie Code First verwenden und über eine bestehende Datenkontextklasse verfügen möchten, wählen Sie diese hier. Es wird aktualisiert werden, um die Modellklasse beizubehalten, die Sie ausgewählt haben.
- Wenn Sie Database First oder Model First verwenden, wählen Sie hier Ihre Objektkontextklasse aus.

Wählen Sie für Ansichten die Ansichts-Engine, die Sie verwenden möchten, oder wählen Sie None an, wenn Sie keine Ansichten erstellen möchten.

Sie können auswählen, erweiterte Optionen, um weitere Optionen für die erstellten Ansichten angeben. Beispielsweise können Sie die Layout- oder Masterseite verwenden.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Verbesserungen an der "ASP.NET MVC 3 neues Projekt" im Dialogfeld

Das Dialogfeld, die, das Sie verwenden, um die neue ASP.NET MVC 3-Projekte erstellen, enthält mehrere Verbesserungen an, wie unten aufgeführt.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Neue "Intranet"-Projektvorlage

Die Liste Projektvorlage enthält die neue Vorlage Intranetanwendung. Diese Vorlage enthält Einstellungen für das Erstellen einer Webanwendung mithilfe der Windows-Authentifizierung anstatt der Formularauthentifizierung. Da eine Intranetanwendung bestimmte IIS-Einstellungen erforderlich, die in einer Projektvorlage gekapselt werden können ist, enthält die Vorlage eine Infodatei mit Anweisungen zur Verwendung die Projektvorlage in IIS zum funktionieren zu machen. Dokumentation für die neue Vorlage Intranetanwendung ist auf der MSDN-Website unter der folgenden URL verfügbar:

[https://msdn.microsoft.com/library/gg703322(VS.98).aspx](https://msdn.microsoft.com/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Projektvorlagen unterstützen nun HTML5

Das Dialogfeld "Neues Projekt"-Feld enthält jetzt eine Option aus, um die Projektvorlagen für HTML5-spezifische Funktionen hinzuzufügen. Auswählen der Option bewirkt, dass die Ansichten generiert werden, die das neue HTML5 enthalten `<header>`, `<footer>`, und `<navigation>` Elemente.

Beachten Sie, dass ältere Browser HTML5-spezifische Tags nicht unterstützt werden. Um diese Einschränkung zu beheben, enthalten die HTML5-Projektvorlagen einen Verweis auf die Modernizr-Bibliothek. (Siehe nächsten Abschnitt.)

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Neue Projektvorlage: Modernizr 1.7

Modernizr ist eine JavaScript-Bibliothek, die Unterstützung von CSS 3 und HTML5 in Browsern ermöglicht, die noch nicht über diese Funktionen unterstützen. Diese Bibliothek ist als vorinstallierte NuGet-Paket in Vorlagen für ASP.NET MVC 3-Projekte enthalten. Weitere Information zu Modernizr finden Sie unter [ http://www.modernizr.com/ ](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Neue Projektvorlagen: aktualisierte Versionen von jQuery, jQuery UI und jQuery Validation

Die Projektvorlagen enthalten nun die folgenden Versionen der jQuery-Skripts:

- jQuery 1.5.1
- jQuery-Validierung 1.8
- jQuery UI 1.8.11

Diese Bibliotheken sind als vorinstallierte NuGet-Pakete enthalten.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Neue Projektvorlage: ADO.NET Entity Framework 4.1 als vorinstallierte NuGet-Paket

Der ADO.NET Entity Framework 4.1 enthält die Code First-Funktion. Code First ist ein neues Entwicklungsmuster für das ADO.NET Entity Framework, die eine Alternative zu bestehenden Mustern Database First und Model First bietet an.

Code First legt das Hauptaugenmerk auf die Definition von Ihr Modells mit POCO-Klassen ("plain old CLR Objects") in Visual Basic oder c# geschrieben. Diese Klassen anschließend zu einer vorhandenen Datenbank zugeordnet werden können, oder zum Generieren eines Datenbankschemas verwendet werden. Zusätzliche Konfiguration kann angegeben werden, mithilfe von *"DataAnnotations"* Attribute oder mithilfe der fluent-APIs.

Dokumentation für die Verwendung von Code Firstwith ASP.NET MVC ist auf der ASP.NET-Website unter den folgenden URLs verfügbar:

[https://www.asp.net/mvc/tutorials/getting-started-with-mvc3-part1-cs](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Neue Projektvorlagen: JavaScript-Bibliotheken als vorinstallierte NuGet-Pakete

Wenn Sie ein neues ASP.NET MVC 3-Projekt erstellen, enthält das Projekt die JavaScript-Dateien bereits zuvor (z. B. die Modernizr-Bibliothek) durch eine Installation mithilfe von NuGet nicht direkt die Skripts in den Ordner "Scripts" in der Projektvorlage Inhalt. Dadurch können Sie NuGet verwenden, um die Skripts auf die neueste Version aktualisieren, wenn neue Versionen der Skripts veröffentlicht werden.

Beispielsweise wird anhand der Häufigkeit neuer jQuery-Versionen die Version von jQuery in der Projektvorlage enthaltene irgendwann veraltet sein. Aber da jQuery als installiertes NuGet-Paket enthalten ist, werden Sie im Dialogfeld "NuGet" benachrichtigt, wenn neuere Versionen von jQuery verfügbar sind.

Da jQuery die Versionsnummer im Dateinamen enthält, die Aktualisierung von jQuery auf die neueste Version erfordert auch aktualisieren die `<script>` Tag, die jQuery-Datei verweist, um den neuen Dateinamen verwenden. Andere enthaltene Skriptbibliotheken enthalten nicht die Versionsnummer im Skriptnamen und, damit sie leichter auf die neueste Version aktualisiert werden können.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Bekannte Probleme

- In einigen Fällen kann die Installation mit "Fehler bei der Fehler Nachricht Installation mit dem Fehlercode (0 x 80070643)" fehl. Informationen dazu, wie Sie dieses Problem umgehen, finden Sie unter [Knowledge Base-Artikel 2531566](https://support.microsoft.com/kb/2531566).
- Das Gerüst für das Hinzufügen eines Controllers ist keine Entitäten erstellen, die Unterstützung in Entity Framework nutzen. Angenommen, eine Basis *Person* -Klasse, die von geerbt wird eine *für Schüler und Studenten* Klasse Gerüstbau die *für Schüler und Studenten* Klasse führt in generiertem Code, der nicht kompiliert werden kann.
- Erstellen ein neues ASP.NET MVC 3-Projekt innerhalb eines Projektmappenordners bewirkt, dass eine *"NullReferenceException"* Fehler. Die problemumgehung besteht darin erstellen Sie das ASP.NET MVC 3-Projekt im Stammverzeichnis der Projektmappe, und klicken Sie dann in den Projektmappenordner verschieben.
- IntelliSense für Razor-Syntax funktioniert nicht, wenn ReSharper installiert ist. Wenn Sie ReSharper installiert haben und die Razor IntelliSense-Unterstützung in ASP.NET MVC 3 nutzen möchten, finden Sie im Eintrag [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) hadi Blog, erläutert, wie sie noch heute zusammen verwendet.
- Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.
- Wenn Sie eine Razor-Ansicht bearbeiten (.cshtml oder. *Vbhtml* Datei), Ansichten. ASP.NET MVC 3 enthält keinerlei Ausschnitte für Razor-Ansichten keine... Aspxselecting einen Codeausschnitt für ASP.NET MVC werden Codeausschnitte für angezeigt.
- Wenn Sie ASP.NET MVC 3 für Visual Web Developer Express auf einem Computer installieren, auf dem Visual Studio nicht installiert ist, und klicken Sie dann später installieren von Visual Studio, müssen Sie ASP.NET MVC 3 erneut installieren. Visual Studio und Visual Web Developer Express haben Komponenten, die vom ASP.NET MVC 3-Installationsprogramm aktualisiert werden. Das gleiche gilt, wenn Sie ASP.NET MVC 3 für Visual Studio auf einem Computer installieren, die keine Visual Web Developer Express, und klicken Sie dann später installieren Sie Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Änderungen in ASP.NET MVC 3 RTM

Dieser Abschnitt beschreibt die Änderungen und Fehlerkorrekturen hervor, die seit dem RC2-Release in der ASP.NET MVC 3 RTM-Version vorgenommen wurden.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Änderung: Die Version von jQuery-Benutzeroberfläche auf 1.8.7 aktualisiert

Die ASP.NET MVC-Projektvorlagen für Visual Studio wurden aktualisiert, um die neueste Version von jQuery UI-Bibliothek einzuschließen. Die Vorlagen enthalten auch den minimalen Satz von Ressourcendateien, die von jQuery-Benutzeroberfläche, wie z. B. das zugeordnete CSS und die Imagedateien erforderlich sind.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Ändern: Die Standardeinstellungen, die ModelMetadataProvider an DataAnnotationsModelMetadataProvider geändert

Im RC2-Release von ASP.NET MVC 3 eingeführt wurde eine *CachedDataAnnotationsMetadataProvider* Klasse, die bereitgestellte zusätzlich zum vorhandenen zwischenspeichern *DataAnnotationsModelMetadataProvider* Klasse als ein zur Verbesserung der Leistung. Allerdings wurden einige Programmfehler mit dieser Implementierung wird gemeldet, damit die Änderung rückgängig gemacht und in der Futures von MVC-Projekt wird verschoben wurde [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Behoben: Einfügen Teil eines Ausdrucks Razor, die Leerzeichen Ergebnisse darin wird umgekehrt enthält

Wenn Sie einen Teil einer Razor-Ausdruck einfügen, die Leerzeichen in einer Razor-Datei enthält, wird der resultierende Ausdruck in Vorabversionen von ASP.NET MVC 3 umgekehrt. Betrachten Sie beispielsweise den folgenden Razor-Codeblock aus:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Wenn Sie den Text "erste Param" bei der ersten Methode auswählen, und fügen Sie ihn als Argument in der zweiten Methode, lautet das Ergebnis wie folgt:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

Das richtige Verhalten ist, dass es sich bei der Einfügevorgang in der folgenden führen soll:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Dieses Problem wurde in der RTM-Version behoben, damit der Ausdruck ordnungsgemäß während des Einfügevorgangs beibehalten wird.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Behoben: Beim Umbenennen einer Razor-Datei, die im Editor geöffnet wird deaktiviert farbige syntaxhervorhebung und IntelliSense

Umbenennen einer Razor-Datei mit Projektmappen-Explorer aus, während die Datei im Editor-Fenster geöffnet wird bewirkt, dass syntaxhervorhebung und IntelliSense nicht mehr funktioniert für diese Datei. Wurde behoben, damit Hervorhebung und IntelliSense bleiben nach der Umbenennung.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Bekannte Probleme

- Wenn Sie Visual Studio 2010 SP1 Beta schließen, während die NuGet-Paket-Manager-Konsole geöffnet ist, wird Visual Studio stürzt ab und wird neu gestartet. Dieses Problem wird behoben werden in der RTM-Version von Visual Studio 2010 SP1.
- Das ASP.NET MVC 3-Installationsprogramm kann nur eine erste Version des NuGet-Paket-Managers installieren. Nachdem Sie die erste Version installiert haben, wird von NuGet installiert werden kann und mithilfe von Visual Studio-Erweiterungs-Manager aktualisiert. Wenn Sie bereits über NuGet installiert haben, rufen Sie die Visual Studio Extension Gallery, auf die neueste Version von NuGet zu aktualisieren.
- Erstellen ein neues ASP.NET MVC 3-Projekt innerhalb eines Projektmappenordners bewirkt, dass eine *"NullReferenceException"* Fehler. Die problemumgehung besteht darin erstellen Sie das ASP.NET MVC 3-Projekt im Stammverzeichnis der Projektmappe, und klicken Sie dann in den Projektmappenordner verschieben.
- Das Installationsprogramm kann viel länger als in vorherigen Versionen von ASP.NET MVC abgeschlossen dauern. Dies ist, da er aktualisiert, dass die Komponenten von Visual Studio 2010.
- IntelliSense für Razor-Syntax funktioniert nicht, wenn ReSharper installiert ist. Wenn Sie ReSharper installiert haben und die Razor IntelliSense-Unterstützung in ASP.NET MVC 3 nutzen möchten, finden Sie im Eintrag [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) hadi Blog, erläutert, wie sie noch heute zusammen verwendet.
- CCSHTML und VBHTML-Ansichten, die mit der Betaversion der ASP.NET MVC 3 erstellt haben keine ihre Buildaktion ordnungsgemäß festgelegt, mit dem Ergebnis, das diese anzeigen Typen sind nicht angegeben, wenn das Projekt veröffentlicht wird. Der Build Action-Wert für diese Dateien sollten auf "Content" festgelegt werden. ASP.NET MVC 3 RTM behebt dieses Problem für neue Dateien, aber nicht korrigieren Sie die Einstellung für vorhandene Dateien für ein Projekt mit Vorabversionen erstellt.
- ![](mvc3-release-notes/_static/image3.png)
- Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.
- Wenn Sie eine Razor-Ansicht (cshtml-Datei) bearbeiten, das Menüelement "Gehe zu Controller" in Visual Studio nicht zur Verfügung, und es gibt keine Codeausschnitte.
- Wenn Sie ASP.NET MVC 3 für Visual Web Developer Express auf einem Computer installieren, auf dem Visual Studio nicht installiert ist, und klicken Sie dann später installieren von Visual Studio, müssen Sie ASP.NET MVC 3 erneut installieren. Visual Studio und Visual Web Developer Express haben Komponenten, die vom ASP.NET MVC 3-Installationsprogramm aktualisiert werden. Das gleiche gilt, wenn Sie ASP.NET MVC 3 für Visual Studio auf einem Computer installieren, die keine Visual Web Developer Express, und klicken Sie dann später installieren Sie Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

- In früheren Versionen von ASP.NET MVC-Aktion sind erstellen pro Anforderung außer in einigen Fällen. Dieses Verhalten wurde nie eine garantierte Verhalten jedoch lediglich ein Implementierungsdetail, und der Vertrag für Filter sie zustandslose berücksichtigt wurde. In ASP.NET MVC 3 werden die Filter aggressiver zwischengespeichert. Aus diesem Grund können benutzerdefinierten Aktionsfiltern, die von nicht ordnungsgemäß Instanzstatus gespeichert unterbrochen werden.
- Die Ausführungsreihenfolge für Ausnahmefilter wurde geändert, für Ausnahmefilter mit dem gleichen *Reihenfolge* Wert. In ASP.NET MVC 2 und früher Ausnahmefilter im Controller mit dem gleichen *Reihenfolge* -Wert, wie diese in einer Aktionsmethode vor den Ausnahmefiltern der Aktionsmethode ausgeführt werden. Dies würde i. d. r. der Fall sein, wenn Ausnahmefilter angewendet werden ohne einen angegebenen *Reihenfolge* Wert. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, damit der spezifischste Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen Wenn die *Reihenfolge* explizit angegeben wird, werden die Filter in der angegebenen Reihenfolge ausgeführt werden.
- Eine neue Eigenschaft namens *FileExtensions* wurde hinzugefügt, um die *VirtualPathProviderViewEngine* Basisklasse. Wenn ASP.NET eine Ansicht anhand des Pfads (nicht nach Namen) sucht, werden nur Ansichten, mit der Erweiterung in der Liste, die durch diese neue Eigenschaft angegebene enthaltenen berücksichtigt. Dies ist eine wichtige Änderung in Anwendungen, in denen ein benutzerdefinierte Build-Anbieter registriert ist, um eine benutzerdefinierte Dateierweiterung für Web Form-Ansichten zu aktivieren und der Anbieter über einen vollständigen Pfad und nicht über einen Namen auf diese Sichten verweisen. Die problemumgehung besteht darin, das Ändern des Werts der *FileExtensions* Eigenschaft, um die benutzerdefinierte Erweiterung einschließen.
- Benutzerdefinierte Controller Factory-Implementierungen, die direkt implementieren die *IControllerFactory* Schnittstelle muss eine Implementierung der neuen bereitstellen *GetControllerSessionBehavior* Methode, der Schnittstelle in dieser Version hinzugefügt. Im Allgemeinen wird empfohlen, dass Sie nicht diese Schnittstelle direkt implementieren und stattdessen Ableiten der Klasse aus *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Änderungen in ASP.NET MVC 3 RC2

Dieser Abschnitt beschreibt, die seit der RC-Version in der ASP.NET MVC 3 RC2-Version vorgenommenen Änderungen (neue Features und Fehlerbehebungen).

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Projekt-Vorlagen geändert, jQuery 1.4.4, jQuery-Validierung 1.7 und jQuery UI 1.8.6 einschließen

Die Projektvorlagen für ASP.NET MVC 3 enthalten jetzt die neuesten Versionen von jQuery, jQuery-Validierung und jQuery UI. jQuery UI ist eine neue Ergänzung für die Projektvorlagen und bietet nützliche Widgets für die Benutzeroberfläche. Weitere Informationen zu jQuery-Benutzeroberfläche, finden Sie auf ihrer Homepage: [ http://jqueryui.com/ ](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Hinzugefügte "AdditionalMetadataAttribute"-Klasse

Können Sie die *AdditionalMetadataAttribute* Klasse zum Auffüllen der *ModelMetadata.AdditionalValues* Wörterbuch für eine Eigenschaft des Modells.

Nehmen wir beispielsweise an, dass ein Ansichtsmodell Eigenschaften verfügt, die nur für Administratoren angezeigt werden soll. Dieses Modell kann durch das neue Attribut mit AdminOnly als "true", Schlüssel und Wert wie im folgenden Beispiel mit Anmerkungen versehen werden:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Diese Metadaten wird eine beliebige Vorlage anzeigen oder Editor zur Verfügung gestellt, wenn ein Produktmodell-Ansicht gerendert wird. Es liegt an Ihnen als Anwendungsentwickler, um die Metadateninformationen zu interpretieren.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Verbesserte Ansicht Gerüstbau

Die T4-Vorlagen, die Gerüstbau-Ansichten jetzt zum Generieren Aufrufe an Hilfsmethoden für die Vorlage wie z.B. *EditorFor* anstelle von Hilfsprogrammen wie z. B. *TextBoxFor*. Diese Änderung verbessert die Unterstützung für Metadaten für das Modell in Form von datenanmerkungsattribute, wenn das Dialogfeld "Ansicht hinzufügen" eine Ansicht generiert.

Das Gerüst für die Ansicht hinzufügen enthält auch die Verbesserte Erkennung und Nutzung von Primärschlüsselinformationen für das Modell, das entsprechend den Konventionen. Das Dialogfeld "Ansicht hinzufügen" verwendet z. B. diese Informationen, um sicherzustellen, dass es sich bei der Wert des Primärschlüssels als bearbeitbaren Formular Felder nicht Gerüst erstellt wurde.

Die standardmäßige bearbeiten und Erstellen von Vorlagen enthalten Verweise auf die jQuery-Skripts, die für die Clientvalidierung erforderlich sind.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Hinzugefügt von Html.Raw-Methode

In der Standardeinstellung der Razor anzeigen HTML-codiert-Engine-alle Werte. Beispielsweise der folgende Codeausschnitt den HTML-Code in die Variable für die Grußformel codiert, damit er in der Seite angezeigt wird `<strong>Hello World!</strong>`.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

Die neue *von Html.Raw* Methode bietet eine einfache Möglichkeit zum Anzeigen von nicht codiertes HTML, wenn der Inhalt bekannt ist, um sicherzugehen. Das folgende Beispiel zeigt die gleiche Zeichenfolge, aber die Zeichenfolge als Markup gerendert wird:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Umbenannte "Controller.ViewModel"-Eigenschaft und die "View"-Eigenschaft auf "ViewBag"

Zuvor die *"ViewModel"* Eigenschaft *Controller* , entsprach der *Ansicht* Eigenschaft einer Ansicht. Diese beiden Eigenschaften bieten die Möglichkeit, Access-Werte, der die *ViewDataDictionary* -Objekt unter Verwendung der dynamischen Eigenschaft Zugriffsmethoden-Syntax. Beide Eigenschaften wurden umbenannt, um dasselbe um Verwechslungen zu vermeiden und konsistenter sein werden.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Umbenannte "ControllerSessionStateAttribute" Klasse "SessionStateAttribute"

Die *ControllerSessionStateAttribute* Klasse in der RC-Version von ASP.NET MVC 3 eingeführt wurde. Die Eigenschaft wurde umbenannt, um eine kompaktere sein.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>Umbenannte RemoteAttribute "Felder"-Eigenschaft auf "AdditionalFields"

Die *RemoteAttribute* Klasse *Felder* Eigenschaft verursachte Verwirrung von Benutzern. Diese Eigenschaft auf Umbenennen *AdditionalFields* verdeutlicht die Zusammenhänge.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>"SkipRequestValidationAttribute", "AllowHtmlAttribute" umbenannt

Die *SkipRequestValidationAttribute* Attribut wurde umbenannt in *AllowHtmlAttribute* seine vorgesehene Verwendung besser darstellen.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Geänderte "Html.ValidationMessage"-Methode, um die erste nützliche Fehlermeldung anzuzeigen.

Die *Html.ValidationMessage* Methode wurde behoben, um die erste nützliche Fehlermeldung anstatt einfach den ersten Fehler anzuzeigen.

Während der modellbindung, die *ModelState* Wörterbuch kann aus mehreren Quellen mit Fehlermeldungen über die Eigenschaft, die aus dem Modell selbst einschließlich gefüllt werden (wenn implementiert *IValidatableObject* ), von Validierungsattributen, die auf die Eigenschaft angewendet und von Ausnahmen, die ausgelöst wird, während die Eigenschaft zugegriffen wird.

Wenn die *Html.ValidationMessage* Methode zeigt eine validierungsmeldung an, die es überspringt modellzustands-Einträge, die eine Ausnahme aus, enthalten, da diese nicht in der Regel für den Endbenutzer vorgesehen sind. Stattdessen überprüft die Methode für die erste Überprüfungsnachricht, die nicht mit einer Ausnahme zugeordnet ist, und zeigt diese an. Wenn keine solche Nachricht gefunden wird, wird standardmäßig eine allgemeine Fehlermeldung, die die erste Ausnahme zugeordnet ist.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Feste @model Deklaration Leerzeichen nicht auf das Dokument hinzufügen

In früheren Versionen der <em>@model</em> Deklaration am oberen Rand einer Ansicht der gerenderten HTML-Ausgabe eine leere Zeile hinzugefügt. Dies wurde behoben, damit die Deklaration kein Leerraum einleitet.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Hinzugefügte "FileExtensions"-Eigenschaft, um den Ansichts-Engines zur Unterstützung der Engine-spezifische Dateinamen

Eine Ansichts-Engine kann eine Sicht mit einem Pfad explizite Ansicht, wie im folgenden Beispiel zurück:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

Die erste Ansichts-Engine versucht immer zum Rendern der Ansicht. Standardmäßig ist die Web Forms-Ansichts-Engine die erste Ansichts-Engine; Da die Web Forms-Engine kann nicht zu eine Razor-Ansicht rendern, tritt ein Fehler auf. Ansichts-Engines verfügen nun über eine *FileExtensions* -Eigenschaft, die verwendet wird, an welche Dateierweiterungen, die sie unterstützen. Diese Eigenschaft aktiviert ist, wenn ASP.NET bestimmt, ob eine Datei von eine Ansichts-Engine gerendert werden kann. Dies ist eine wichtige Änderung, und weitere Details sind in enthalten die [Breaking Changes](#_Toc2_BC) Abschnitt dieses Dokuments.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Feste "LabelFor"-Hilfsprogramm, den richtigen Wert für das Attribut "For" auszugeben.

Ein Fehler wurde behoben Where der *LabelFor* Methode gerendert eine *für* -Attribut, entspricht die *Eingabe* des Elements *Namen* Attribut die ID Gemäß der W3C die *für* Attribut sollte übereinstimmen. die *Eingabe* des Element-ID

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Feste "RenderAction"-Methode, um explizite Werte Vorrang vor, während der Modellbindung zu gewähren.

In früheren Versionen, explizite Werte, die übergeben wurden, die *RenderAction* Methode wurden durch die aktuellen Werte während der modellbindung in eine untergeordnete Aktion ignoriert wird. Die Korrektur wird sichergestellt, dass explizite Werte während der modellbindung Vorrang.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

- In früheren Versionen von ASP.NET MVC wurden Aktionsfilter pro Anforderung außer in einigen Fällen erstellt. Dieses Verhalten wurde nie eine garantierte Verhalten jedoch lediglich ein Implementierungsdetail, und der Vertrag für Filter sie zustandslose berücksichtigt wurde. In ASP.NET MVC 3 werden die Filter aggressiver zwischengespeichert. Aus diesem Grund können benutzerdefinierten Aktionsfiltern, die von nicht ordnungsgemäß Instanzstatus gespeichert unterbrochen werden.
- Die Ausführungsreihenfolge für Ausnahmefilter wurde geändert, für Ausnahmefilter mit dem gleichen *Reihenfolge* Wert. In ASP.NET MVC 2 und früher Ausnahmefilter auf dem Controller an, die die gleiche *Reihenfolge* -Wert, wie diese in einer Aktionsmethode vor den Ausnahmefiltern der Aktionsmethode ausgeführt wurden. Dies würde i. d. r. der Fall sein, wenn Ausnahmefilter angewendet wurden ohne einen angegebenen *Reihenfolge* Wert. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, damit der spezifischste Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen Wenn die *Reihenfolge* explizit angegeben wird, werden die Filter in der angegebenen Reihenfolge ausgeführt werden.
- Eine neue Eigenschaft namens *FileExtensions* wurde hinzugefügt, um die *VirtualPathProviderViewEngine* Basisklasse. Wenn ASP.NET eine Ansicht anhand des Pfads (nicht nach Namen) sucht, werden nur Ansichten, mit der Erweiterung in der Liste, die durch diese neue Eigenschaft angegebene enthaltenen berücksichtigt. Dies ist eine wichtige Änderung in Anwendungen, in denen ein benutzerdefinierte Build-Anbieter registriert ist, um eine benutzerdefinierte Dateierweiterung für Web Form-Ansichten zu aktivieren und der Anbieter über einen vollständigen Pfad und nicht über einen Namen auf diese Sichten verweisen. Die problemumgehung besteht darin, das Ändern des Werts der *FileExtensions* Eigenschaft, um die benutzerdefinierte Erweiterung einschließen.
- Benutzerdefinierte Controller Factory-Implementierungen, die direkt implementieren die <em>IControllerFactory</em> Schnittstelle muss eine Implementierung der neuen bereitstellen <em>GetControllerSessionBehavior</em>  <em>Methode, die die Schnittstelle in dieser Version hinzugefügte</em>. Im Allgemeinen wird empfohlen, dass Sie nicht diese Schnittstelle direkt implementieren und stattdessen Ableiten der Klasse aus <em>DefaultControllerFactory</em>.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Bekannte Probleme

- Das ASP.NET MVC 3-Installationsprogramm kann nur eine erste Version des NuGet-Paket-Managers installieren. Nachdem Sie die erste Version installiert haben, wird von NuGet installiert werden kann und mithilfe von Visual Studio-Erweiterungs-Manager aktualisiert. Wenn Sie bereits über NuGet installiert haben, rufen Sie die Visual Studio Extension Gallery, auf die neueste Version von NuGet zu aktualisieren.
- Erstellen ein neues ASP.NET MVC 3-Projekt innerhalb eines Projektmappenordners bewirkt, dass eine *"NullReferenceException"* Fehler. Die problemumgehung besteht darin erstellen Sie das ASP.NET MVC 3-Projekt im Stammverzeichnis der Projektmappe, und klicken Sie dann in den Projektmappenordner verschieben.
- Das Installationsprogramm kann viel länger als in vorherigen Versionen von ASP.NET MVC abgeschlossen dauern. Dies ist, da er aktualisiert, dass die Komponenten von Visual Studio 2010.
- IntelliSense für Razor-Syntax funktioniert nicht, wenn ReSharper installiert ist. Wenn Sie ReSharper installiert haben und die Vorteile der Razor IntelliSense-Unterstützung in ASP.NET MVC 3 RC2 durchführen möchten, finden Sie im Eintrag [Razor Intellisense and ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) hadi Blog, erläutert, wie sie noch heute zusammen verwendet.
- CSHTML und VBHTML-Ansichten, die mit der Betaversion der ASP.NET MVC 3 erstellt haben keine ihre Buildaktion richtig festgelegt wurde, mit dem Ergebnis, das diese anzeigen Typen sind nicht angegeben, wenn das Projekt veröffentlicht wird. Die *Buildvorgang* Wert für diese Dateien auf Inhalt festgelegt werden soll ". ASP.NET MVC 3 RC2 behebt dieses Problem für neue Dateien, aber nicht korrigieren Sie die Einstellung für vorhandene Dateien für ein Projekt mit der Beta-Version erstellt wurde.![](mvc3-release-notes/_static/image4.png)
- Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.
- Wenn Sie eine Razor-Ansicht (cshtml-Datei) bearbeiten, das Menüelement "Gehe zu Controller" in Visual Studio nicht zur Verfügung, und es gibt keine Codeausschnitte.
- Wenn Sie ASP.NET MVC 3 für Visual Web Developer Express auf einem Computer installieren, auf dem Visual Studio nicht installiert ist, und klicken Sie dann später installieren von Visual Studio, müssen Sie ASP.NET MVC 3 erneut installieren. Visual Studio und Visual Web Developer Express haben Komponenten, die vom ASP.NET MVC 3-Installationsprogramm aktualisiert werden. Das gleiche gilt, wenn Sie ASP.NET MVC 3 für Visual Studio auf einem Computer installieren, die keine Visual Web Developer Express, und klicken Sie dann später installieren Sie Visual Web Developer Express.
- Installieren von ASP.NET MVC 3 RC 2 wird NuGet nicht aktualisiert, wenn Sie bereits installiert ist. Aktualisieren von NuGet, navigieren zu den Visual Studio-Erweiterungs-Manager, und er sollte als ein verfügbares Update angezeigt. Sie können NuGet auf die neueste Version von dort aus aktualisieren.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>ASP.NET MVC 3 Release Candidate

ASP.NET MVC-Release Candidate wurde am 9. November 2010 veröffentlicht.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Neue Features in ASP.NET MVC 3 RC

In diesem Abschnitt wird beschrieben, Funktionen, die eingeführt wurden, in der ASP.NET MVC 3 RC-Version, seit der Betaversion.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>NuGet-Paket-Manager

ASP.NET MVC 3 enthält den NuGet Package Manager (ehemals NuPack), die eine integrierte paketverwaltungstool für das Hinzufügen von Bibliotheken und Tools für Visual Studio-Projekte ist. Dieses Tool automatisiert die Schritte, die Entwickler heute durchführen, um eine Bibliothek in ihre Quellstruktur zu erhalten.

Sie können mit NuGet arbeiten, als ein Befehlszeilentool, als ein integriertes Konsolenfenster in Visual Studio 2010, klicken Sie im Kontextmenü der Visual Studio sowie eine Reihe von PowerShell-Cmdlets.

Weitere Informationen zu NuGet finden Sie in der [Nuget-Dokumentation](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Verbesserte "New Project" (Dialogfeld)

Wenn Sie ein neues Projekt erstellen, kann das Dialogfeld "Neues Projekt" jetzt Sie die Ansichts-Engine als auch für eine ASP.NET MVC-Projekttyp angeben.

![](mvc3-release-notes/_static/image5.png)

Unterstützung für das Ändern der Liste der Vorlagen und Module in das Dialogfeld aufgeführt ist in dieser Version enthalten.

Die Standardvorlagen sind folgende:

Leer. Enthält einen minimalen Satz von Dateien für eine ASP.NET MVC-Projekt, einschließlich der standardmäßigen Verzeichnisstruktur für ASP.NET MVC-Projekte, eine Datei "Site.CSS", das die Standard-Formatvorlagen, ASP.NET MVC und Verzeichnis "Scripts", die die Standard-JavaScript-Dateien enthält.

Internet-Anwendung. Enthält die Beispiel-Funktionalität, die zeigt, wie Sie mit der Membership-Provider mit ASP.NET MVC.

Die Liste der Projektvorlagen das Projekt, die Sie im Dialogfeld angezeigt wird, wird in der Windows-Registrierung angegeben.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Sessionless-Controller

Die neue *ControllerSessionStateAttribute* bietet Ihnen mehr Kontrolle über Sitzungszustands-Verhalten für Controller durch Angabe einer [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/library/system.web.sessionstate.sessionstatebehavior.aspx) Enumerationswert.

Im folgenden Beispiel wird das Deaktivieren des Sitzungsstatus für alle Anforderungen an einen Controller.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

Das folgende Beispiel zeigt, wie schreibgeschützte Sitzungszustand für alle Anforderungen an einen Controller festgelegt wird.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Neue Validierungsattribute

#### <a name="compareattribute"></a>CompareAttribute

Die neue *CompareAttribute* Validierungsattribut können Sie die Werte von zwei verschiedene Eigenschaften eines Modells zu vergleichen. Im folgenden Beispiel die *ComparePassword* Eigenschaft muss übereinstimmen. die *Kennwort* Feld, damit es gültig ist.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

Die neue *RemoteAttribute* Validierungsattribut nutzt die jQuery-Validierung-Plug-ins remote Validierungssteuerelement angegeben, das kann die clientseitige Validierung zum Aufrufen einer Methode auf dem Server, der die tatsächliche Überprüfungslogik ausführt.

Im folgenden Beispiel die *Benutzername* Eigenschaft verfügt über die *RemoteAttribute* angewendet. Wenn Sie diese Eigenschaft in einer Bearbeitungsansicht zu bearbeiten, ruft die Clientvalidierung eine Aktion, die mit dem Namen *UserNameAvailable* auf die *UsersController* Klasse, um dieses Feld zu überprüfen.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

Das folgende Beispiel zeigt den entsprechenden Controller an.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

Standardmäßig ist der Eigenschaftenname, der das Attribut angewendet wird als Abfragezeichenfolgen-Parameter an die Aktionsmethode gesendet.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Neue Überladungen für "LabelFor" und "LabelForModel"-Methoden

Neue Überladungen für wurde die *LabelFor* und *LabelForModel* Methoden, mit denen Sie angeben, den Text der Bezeichnung. Das folgende Beispiel zeigt, wie Sie diese Überladungen zu nutzen.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Untergeordnete Aktion Zwischenspeichern der Ausgabe

Die *OutputCacheAttribute* unterstützt die Zwischenspeicherung der Ausgabe der untergeordnete Aktionen, die aufgerufen werden, mithilfe der *Html.RenderAction* oder *Html.Action* Helper-Methoden. Das folgende Beispiel zeigt eine Ansicht, die eine andere Aktion aufruft.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

Die *GetDate* Aktion mit dem versehen ist die *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Wenn dieser Code ausgeführt wird, wird das Ergebnis des Aufrufs von Html.Action("GetDate") 100 Sekunden zwischengespeichert.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>"Add View" Dialogfeld Verbesserungen an

Wenn Sie eine stark typisierte Ansicht hinzufügen, filtert das Dialogfeld "Ansicht hinzufügen" nun weitere unzulässige Typen als in früheren Versionen, wie viele Kerne .NET Framework-Typen ein. Darüber hinaus wird jetzt die Liste sortiert, durch den Namen der Klasse und nicht durch den vollqualifizierten Typnamen, wodurch es einfacher Typen finden. Der Typname wird z. B. jetzt wie im folgenden Beispiel angezeigt:

Klassenname (Namespace)

In früheren Versionen würde dies wie folgt angezeigt wurden:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Differenzierte Anforderungsvalidierung

Die *ausschließen* Eigenschaft *ValidateInputAttribute* nicht mehr vorhanden ist. Um die Anforderungsvalidierung für bestimmte Eigenschaften eines Modells während der modellbindung übersprungen haben, verwenden Sie stattdessen die neuen *SkipRequestValidationAttribute*.

Nehmen wir beispielsweise an, dass eine Aktionsmethode verwendet wird, um einen Blogbeitrag zu bearbeiten:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

Das folgende Beispiel zeigt das Ansichtsmodell für einen Blogbeitrag.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Wenn ein Benutzer Markup für die Description-Eigenschaft übermittelt, schlägt modellbindung fehl, weil die Anforderungsvalidierung aktiviert ist. Um die Request-Überprüfung während der modellbindung für die Beschreibung im Blogbeitrag zu deaktivieren, gelten die *SkipRequpestValidationAttribute* , Eigenschaft, die im folgenden Beispiel gezeigt:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Alternativ anwenden, um die Anforderungsvalidierung für jede Eigenschaft des Modells zu deaktivieren, *ValidateInputAttribute* mit einem Wert von *"false"* an die Aktionsmethode:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

- Die Ausführungsreihenfolge für Ausnahmefilter wurde geändert, für Ausnahmefilter mit dem gleichen *Reihenfolge* Wert. In ASP.NET MVC 2 und früher Ausnahmefilter auf dem Controller an, die die gleiche *Reihenfolge* wie diese in einer Aktionsmethode vor den Ausnahmefiltern der Aktionsmethode ausgeführt wurden. Dies würde i. d. r. der Fall sein, wenn Ausnahmefilter angewendet wurden ohne einen angegebenen *Reihenfolge* Wert. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, damit der spezifischste Ausnahmehandler zuerst ausgeführt wird. Wie in früheren Versionen Wenn die *Reihenfolge* explizit angegeben wird, werden die Filter in der angegebenen Reihenfolge ausgeführt werden.
- Eine neue Eigenschaft namens hinzugefügt *FileExtensions* auf die *VirtualPathProviderViewEngine* Basisklasse. Eine Ansicht anhand des Pfads (und nicht anhand des Namens) nur für Ansichten mit einer Dateierweiterung, die in enthaltenen nachgeschlagen gilt die Liste, die durch diese neue Eigenschaft angegeben. Dies ist eine wichtige Änderung für diejenigen, die registrieren Sie eines benutzerdefinierten Anbieters um eine benutzerdefinierte Dateierweiterung für das Web Form-Ansichten zu aktivieren, und verweisen auf diese Ansichten mit einem Namen, statt einen vollständigen Pfad. Die problemumgehung besteht darin, das Ändern des Werts der *FileExtensions* Eigenschaft, um die benutzerdefinierte Erweiterung einschließen.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Bekannte Probleme

- Das Installationsprogramm möglicherweise länger dauert als in vorherigen Versionen von ASP.NET MVC abschließen, da es sich um Komponenten von Visual Studio 2010 aktualisiert.
- Das Gerüst Ansicht hinzufügen, bei der Auswahl von Astrongly typisierte Gerüste lesegeschützte Eigenschaften anzeigen. Diese sollte immer durch Gerüstbau ignoriert werden. Das Dialogfeld "Ansicht hinzufügen" erstellt schreibgeschützte Eigenschaften auch das Gerüst für die Erstellung eine Ansicht "Bearbeiten" oder "Erstellen". Schreibgeschützte Eigenschaften sollten nur für die Anzeige und die Listenansichten Gerüst erstellt werden.
- Debuggen funktioniert nicht, wenn ASP.NET MVC 3, zusammen mit dem Async CTP installiert ist. ASP.NET MVC 3 installiert Seite-an-Seite mit der Async CTP nicht möglich. Deinstallieren Sie die asynchrone CTP um Debuggen zu reparieren. Weitere Informationen finden Sie in [in diesem Blogbeitrag](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) über alle Komponenten von ASP.NET MVC 3 RC zu deinstallieren.
- Razor Intellisense funktioniert nicht, wenn Resharper installiert ist. Wenn Sie ReSharper installiert haben, und in ASP.NET MVC 3 RC, lesen Sie die Intellisense-Unterstützung für Razor nutzen möchten [in diesem Blogbeitrag](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) von JetBrains, das erläutert, wie sie noch heute gemeinsam verwendet.
- CSHTML und VBHTML-Ansichten, die mit der Betaversion der ASP.NET MVC 3 erstellt müssen ihre Buildaktion ordnungsgemäß nicht die diese Veröffentlichung nicht angibt. Die *Buildvorgang* für diese Dateien auf "Content" festgelegt werden soll. ASP.NET MVC 3 RC behebt dieses Problem für neue Dateien, aber nicht korrigieren Sie die Einstellung für vorhandene Dateien für ein Projekt mit der Betaversion erstellt wurden.
- Das Installationsprogramm möglicherweise länger dauert als in vorherigen Versionen von ASP.NET MVC abschließen, da es sich um Komponenten von Visual Studio 2010 aktualisiert.
- Das Gerüst Ansicht hinzufügen, wenn die Ansicht Gerüste wählen eine "Bearbeiten" stark typisiert werden. schreibgeschützte Eigenschaften. Lesegeschützte Eigenschaften sind ebenso für "Display" Ansichten erstellt haben.
- Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.
- Installieren der Visual Studio Async CTP verursacht einen Konflikt mit der Razor-Version, die als Teil der ASP.NET MVC 3-Tools-Installation enthalten ist. Stellen Sie sicher, dass nicht zu versuchen, die Visual Studio Async CTP und die Razor-Version auf dem gleichen Computer installieren.
- Wenn Sie eine Razor-Ansicht (cshtml-Datei) bearbeiten, das Menüelement "Gehe zu Controller" in Visual Studio nicht zur Verfügung, und es gibt keine Codeausschnitte.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>ASP.NET MVC 3 Beta

ASP.NET MVC 3 Beta wurde am 6. Oktober 2010 veröffentlicht. Die folgenden Hinweise beziehen sich auf die Betaversion, und unterliegen alle Updates oder Änderungen, die im obigen Abschnitt zu ASP.NET MVC 3 Release Candidate verwiesen wird.

## <a id="0.1__Toc274034215"></a>  Neue Featuresin ASP.NET MVC 3-Beta

<a id="0.1__Default_validation_system"></a>In diesem Abschnitt wird beschrieben, Funktionen, die eingeführt wurden, in der Betaversion von ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>  NuGet-Paket-Manager

ASP.NET MVC 3 enthält NuGet-Paket-Manager, die eine integrierte paketverwaltungstool für Hinzufügen von Bibliotheken und Tools für Visual Studio-Projekte ist. Zum größten Teil, automatisiert es die Schritte, die Entwickler heute durchführen, um eine Bibliothek in ihre Quellstruktur zu erhalten.

Sie können mit NuGet arbeiten, als ein Befehlszeilentool, als ein integriertes Konsolenfenster in Visual Studio 2010, klicken Sie im Kontextmenü der Visual Studio, und als Gruppe von PowerShell-Cmdlets.

Weitere Informationen zu NuGet finden Sie in der [NuGet-Dokumentation](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>  Verbesserte neues Projekt (Dialogfeld)

Wenn Sie ein neues Projekt erstellen, kann das Dialogfeld "Neues Projekt" jetzt Sie die Ansichts-Engine als auch für eine ASP.NET MVC-Projekttyp angeben.

![](mvc3-release-notes/_static/image6.png)

Unterstützung für das Ändern der Liste der Vorlagen und Module in das Dialogfeld aufgeführt ist nicht in dieser Version enthalten.

Die Standardvorlagen sind folgende:

Leer. Enthält einen minimalen Satz von Dateien für eine ASP.NET MVC-Projekt, einschließlich der standardmäßigen Verzeichnisstruktur für ASP.NET MVC-Projekte, eine kleine Datei "Site.CSS", das die Standard-Formatvorlagen, ASP.NET MVC und Verzeichnis "Scripts", die die Standard-JavaScript-Dateien enthält.

Internet-Anwendung. Enthält die Beispiel-Funktionalität, die zeigt, wie Sie mit der Membership-Provider in ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>  Vereinfachte Methode, geben Sie stark typisierte Modelle in Razor-Ansichten

Wurde die Möglichkeit zum Angeben des Modelltyps für stark typisierte Razor-Ansichten mithilfe des neuen vereinfacht @model Direktive für die CSHTML-Ansichten und @ModelType -Direktive für VBHTML-Ansichten. In früheren Versionen von ASP.NET MVC würden Sie angeben, dass auf diese Weise ein stark typisiertes Modell für Razor-Ansichten:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

In dieser Version können Sie die folgende Syntax:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>  Unterstützung für neue ASP.NET Web Pages-Hilfsmethoden

Die neue ASP.NET Web Pages-Technologie umfasst einen Satz von Hilfsmethoden bereit, die für das Hinzufügen von häufig verwendeten Funktionen zu Ansichten und Controller hilfreich sind. ASP.NET MVC 3 unterstützt diese Hilfsmethoden in Controllern und Ansichten (wenn geeignet). Diese Methoden sind in der Assembly System.Web.Helpers enthalten. In der folgende Tabelle sind einige der ASP.NET Web Pages-Hilfsmethoden.

| **Helper** | **Beschreibung** |
| --- | --- |
| Diagramm | Rendert ein Diagramm innerhalb einer Ansicht an. Enthält Methoden, z. B. Chart.ToWebImage Chart.Save und Chart.Write an. |
| Crypto | Verwendet, die Hashalgorithmen, um ordnungsgemäß zu erstellen, mit Salt-Wert und Kennwörter gehasht. |
| WebGrid | Wird eine Auflistung von Objekten (in der Regel Daten aus einer Datenbank) als Raster gerendert. Unterstützt die Paginierung und Sortierung. |
| WebImage | Rendert ein Bild an. |
| WebMail | Sendet eine E-Mail. |

Eine Kurzübersicht-Thema, das die Hilfsprogramme und die grundlegende Syntax aufgeführt sind, ist verfügbar als Teil der Dokumentation des ASP.NET Razor-Syntax unter folgender URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-api-reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>  Unterstützung der Einfügung von zusätzlichen Abhängigkeit

Erstellen in der ASP.NET MVC 3 Preview 1-Version, umfasst die aktuelle Version Unterstützung für zwei neue Dienste und vier vorhandene Dienste und verbesserte Unterstützung für die Auflösung von Abhängigkeiten und die Common Service Locator.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Neue IControllerActivator-Benutzeroberfläche für differenzierte Controller Instanziierung

Die neue IControllerActivator-Benutzeroberfläche bietet es sich um eine präzisere Steuerung wie Controller über Dependency Injection instanziiert werden. Das folgende Beispiel zeigt die-Schnittstelle:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Vergleichen Sie dies an, mit der Rolle der Controllerfactory. Eine Controllerfactory ist eine Implementierung der IControllerFactory-Schnittstelle, die sowohl für die Suche nach einem Typ des Domänencontrollers und Instanziieren von Instanzen dieses Controllertyps zuständig ist.

Controlleraktivatoren dienen nur Instanziieren von Instanzen ein. Sie führen keine dem Controller das Nachschlagen des Replikationstyps aus. Nach dem Suchen nach einer entsprechenden Controllertyp, sollten die Controllerfactorys behandelt die Instanziierung des Controllers zu einer Instanz von IControllerActivator delegieren.

Die Klasse DefaultControllerFactory hat es sich um einen neuen Konstruktor, der eine IControllerFactory-Instanz akzeptiert. Dadurch können Sie das Anwenden von Dependency Injection, um diesen Aspekt der controllererstellung zu verwalten, ohne dass der Typ des Domänencontrollers Lookup-Standardverhalten außer Kraft setzen.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>IServiceLocator-Schnittstelle, die durch IDependencyResolver ersetzt

Die Betaversion von ASP.NET MVC 3 wurde basierend auf Communityfeedback, die Verwendung der Schnittstelle IServiceLocator durch eine abgespeckte IDependencyResolver-Schnittstelle für die Anforderungen von ASP.NET MVC ersetzt. Das folgende Beispiel zeigt die neue Schnittstelle:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Im Rahmen dieser Änderung wurde die ServiceLocator-Klasse auch mit der DependencyResolver-Klasse ersetzt. Registrierung eines Abhängigkeitsauflösers ein ist vergleichbar mit früheren Versionen von ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Implementierungen dieser Schnittstelle sollten einfach auf den zugrunde liegenden abhängigkeitsinjektionscontainer delegieren, um den registrierten Dienst für den angeforderten Typ bereitzustellen.

Wenn keine registrierten Dienste des angeforderten Typs vorhanden sind, erwartet ASP.NET MVC Implementierungen dieser Schnittstelle von "GetService" null zurückgeben und eine leere Auflistung von GetServices zurückgibt.

Die neue DependencyResolver-Klasse können Sie die Klassen zu registrieren, die entweder die neue IDependencyResolver-Schnittstelle oder die Common Service Locator-Schnittstelle (IServiceLocator) implementieren. Weitere Informationen zum Common Service Locator finden Sie unter [CommonServiceLocator auf GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Neue IViewActivator-Benutzeroberfläche für differenzierte Ansicht Seite Instanziierung

Die neue IViewPageActivator-Benutzeroberfläche bietet es sich um eine präzisere Steuerung wie Ansichtsseiten über Dependency Injection instanziiert werden. Dies gilt für sowohl die WebFormView Instanzen als auch die RazorView-Instanzen. Das folgende Beispiel zeigt die neue Schnittstelle:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Diese Klassen akzeptieren nun ein Konstruktorargument IViewPageActivator, können Sie Abhängigkeitsinjektion verwenden, um steuern, wie die ViewPage, ViewUserControl und WebViewPage Typen instanziiert werden.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Neue Abhängigkeit Resolver-Unterstützung für vorhandene Dienste

Die neue Version enthält Unterstützung der Auflösung von Abhängigkeiten für die folgenden Dienste:

- Modell Validierungsanbieter. Klassen, die ModelValidatorProvider implementieren, die in den Abhängigkeitskonfliktlöser registriert werden können, und das System anhand dieser Client- und serverseitige Validierung unterstützt.
- Anbieter für Modellmetadaten. Eine einzelne Klasse, die ModelMetadataProvider implementiert, die in den Abhängigkeitskonfliktlöser registriert werden kann, und das System verwendet werden, um Metadaten für Vorlagen und Validierung bereitzustellen.
- Der Wertanbieter. Klassen, die ValueProviderFactory implementieren, die in den Abhängigkeitskonfliktlöser registriert werden können, und das System anhand dieser um Wertanbieter zu erstellen, die vom Controller und während der modellbindung verwendet werden.
- Modellbinder. Klassen, die IModelBinderProvider implementieren, die in den Abhängigkeitskonfliktlöser registriert werden können, und das System anhand dieser Modellbinder erstellen, die durch die das modellbindungssystem genutzt werden.

### <a id="0.1__Toc274034221"></a>  Neue Unterstützung für die unaufdringliche jQuery-basierten Ajax

ASP.NET MVC umfasst Ajax Helper-Methoden wie z. B. Folgendes an:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Diese Methoden verwenden JavaScript, um eine Aktionsmethode auf dem Server, anstatt ein vollständiges Postback aufzurufen. Diese Funktionalität wurde aktualisiert, um die Vorteile von jQuery auf unaufdringliche Weise zu nutzen. Anstatt intrusively Ausgeben von Inline-Clientskripts, trennen diese Hilfsmethoden das Verhalten aus dem Markup durch das Ausgeben von HTML5-Attribute, die unter Verwendung der *Data-Ajax-* Präfix. Verhalten wird dann an das Markup durch Verweisen auf die entsprechenden JavaScript-Dateien angewendet. Stellen Sie sicher, dass die folgenden JavaScript-Dateien verwiesen wird:

- jquery-1.4.1.js
- jquery.unobtrusive.ajax.js

Dieses Feature ist in der Datei "Web.config" in der ASP.NET MVC 3-Vorlagen für neuen Projekte standardmäßig aktiviert, aber für vorhandene Projekte standardmäßig deaktiviert. Weitere Informationen finden Sie unter [anwendungsweite-Flags für die Clientvalidierung und unaufdringliches JavaScript hinzugefügt](#0.1_AddedApplicationWideFlagsForClientValida) weiter unten in diesem Dokument.

### <a id="0.1__Toc274034222"></a>  Neue Unterstützung für die unaufdringliche jQuery-Validierung

Standardmäßig verwendet ASP.NET MVC 3 Beta jQuery-Validierung auf unaufdringliche Weise aus, um die clientseitige Validierung durchzuführen. Um unaufdringliche Clientvalidierung zu aktivieren, stellen Sie einen Aufruf wie folgt in einer Ansicht aus:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Dies erfordert, dass ViewContext.UnobtrusiveJavaScriptEnabled-Eigenschaft auf "true" festgelegt ist, die Sie tun können, indem Sie folgenden Aufruf:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Außerdem stellen Sie sicher, dass die folgenden JavaScript-Dateien verwiesen wird.

- jquery-1.4.1.js
- jquery.validate.js
- jquery.validate.unobtrusive.js

Dieses Feature ist auf in die Datei "Web.config" in der ASP.NET MVC 3-Vorlagen für neuen Projekte standardmäßig aktiviert, aber für vorhandene Projekte standardmäßig deaktiviert. Weitere Informationen finden Sie unter [neue anwendungsweite-Flags für die Clientvalidierung und unaufdringliches JavaScript](#0.1_AddedApplicationWideFlagsForClientValida) weiter unten in diesem Dokument.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>  Neue anwendungsweite-Flags für die Clientvalidierung und unaufdringliches JavaScript

Sie können aktivieren oder deaktivieren Sie die Clientvalidierung und unaufdringliches JavaScript global über statische Member der HtmlHelper-Klasse, wie im folgenden Beispiel:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Die Standardprojektvorlagen aktiviert unaufdringliches JavaScript sind standardmäßig. Sie können auch aktivieren oder deaktivieren Sie diese Funktionen in der Stammdatei "Web.config" Ihrer Anwendung, die mit den folgenden Einstellungen:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Da Sie diese Features standardmäßig aktivieren können, wurden neue Überladungen für die HtmlHelper-Klasse eingeführt, mit die Sie die Standardeinstellungen außer Kraft setzen wie in den folgenden Beispielen gezeigt können:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Um Abwärtskompatibilität zu gewährleisten werden beide Features standardmäßig deaktiviert.

### <a id="0.1__Toc274034224"></a>  Neue Unterstützung für Code, bevor die Ausführung von Ansichten ausgeführt wird.

Sie können jetzt eine Datei namens einfügen \_viewstart.cshtml (oder \_viewstart.vbhtml) im Verzeichnis "Views" und fügen Sie Code hinzu, die mehrere Ansichten in diesem Verzeichnis und seinen Unterverzeichnissen weitergegeben werden. Beispielsweise können Sie den folgenden Code in Zuordnen der \_viewstart.cshtml-Seite im Ordner "~/Views":

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Hiermit wird der Layoutseite für jede Ansicht in den Ordner "Views" und alle Unterordner rekursiv. Wenn eine Ansicht gerendert wird, den Code in die \_viewstart.cshtml-Datei ausgeführt wird, bevor der Code für die Sicht ausgeführt wird. Die \_viewstart.cshtml Code wendet auf jede Ansicht in diesem Ordner.

Standardmäßig wird der Code in die \_viewstart.cshtml Datei gilt auch für Ansichten in jedem Unterordner. Einzelne Unterordner können jedoch ihre eigene Version der haben die \_viewstart.cshtml Datei; im Fall die lokale Version Vorrang. Um Code auszuführen, die für alle Ansichten für die HomeController gemein sind, z. B. put ein \_viewstart.cshtml-Datei im Ordner "~/Views/Home".

### <a id="0.1__Toc274034225"></a>  Neue Unterstützung für die VBHTML-Razor-Syntax

Die vorherige Vorschau der ASP.NET MVC enthalten Unterstützung für Ansichten mit Razor-Syntax, die basierend auf C# -Code. Diese Ansichten verwenden die cshtml-Dateierweiterung. Im Rahmen der derzeit ausgeführte Arbeit zur Unterstützung von Razor führt das ASP.NET MVC 3-Beta-Unterstützung für die Razor-Syntax in Visual Basic, die die Erweiterung der vbhtml-Datei verwendet.

Eine Einführung in Visual Basic-Syntax in VBHTML-Seiten verwenden finden Sie im Tutorial unter folgender URL:

[https://www.asp.net/webmatrix/tutorials/asp-net-web-pages-visual-basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>  Mehr Kontrolle über ValidateInputAttribute

ASP.NET MVC hat immer die ValidateInputAttribute-Klasse, enthalten, die ASP.NET Anforderung Überprüfung Kerninfrastruktur sicherstellen, dass die eingehende Anforderung keine potenziell böswilligen Eingaben aufruft. Standardmäßig ist die Überprüfung von Eingaben aktiviert. Es ist möglich, Request-Überprüfung zu deaktivieren, indem Sie mit dem ValidateInputAttribute-Attribut, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Viele Webanwendungen haben jedoch die einzelnen Felder, die HTML-Code zu ermöglichen, während die restlichen Felder nicht sollte. Die ValidateInputAttribute-Klasse können Sie eine Liste der Felder angeben, die nicht bei der Anforderungsvalidierung enthalten sein sollten.

Wenn Sie eine Blog-Engine entwickeln, sollten Sie z. B. Markup in den Feldern Text und die Zusammenfassung zu ermöglichen. Diese Felder können durch zwei input-Element, jeweils ein Name-Attribut, das für den Eigenschaftennamen ("Text" und "Zusammenfassung") dargestellt werden. Überprüfung für diese Felder zum Deaktivieren der Anforderung nur die Namen (durch Trennzeichen getrennt) in der Exclude-Eigenschaft der ValidateInput-Klasse, wie im folgenden Beispiel angeben:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>  Hilfsprogramme und konvertieren Unterstriche zu Bindestrichen für HTML-Attributnamen, die mit der anonyme Objekte angegeben

Helper-Methoden können Sie ein anonymes Objekt, wie im folgenden Beispiel mit Attribut-Wert-Paare angeben:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Dieser Ansatz lassen nicht Sie die Bindestriche in den Attributnamen verwenden, da ein Bindestrich für einen Eigenschaftennamen in ASP.NET verwendet werden kann. Bindestriche sind jedoch wichtig, dass benutzerdefinierte HTML5-Attribute. HTML5 verwendet beispielsweise das Präfix "Data-".

Gleichzeitig die Unterstriche nicht für die Namen der Attribute im HTML-Format verwendet werden, aber in Eigenschaftennamen gültig sind. Aus diesem Grund werden bei Angabe von Attributen, die mit einem anonymen Objekt und die Attributnamen einen Unterstrich enthalten, Helper-Methoden die Unterstriche, Bindestriche konvertiert werden. Die folgende Hilfsprogramm-Syntax verwendet z. B. einen Unterstrich:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

Im vorherige Beispiel wird das folgende Markup gerendert, wenn das Hilfsprogramm ausgeführt wird:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>  Fehlerbehebungen

Die Objekt-Standardvorlage für die vorlagenhelfer EditorFor und "DisplayFor" unterstützt jetzt die Reihenfolge, in der DisplayAttribute.Order-Eigenschaft angegeben. (In früheren Versionen wurde die Einstellung nicht verwendet.)

Jetzt die Clientvalidierung unterstützt die Überprüfung von außer Kraft gesetzten Eigenschaften, die Validierungsattribute angewendet werden.

JsonValueProviderFactory ist jetzt standardmäßig registriert.

## <a id="0.1__Toc274034229"></a>  Wichtige Änderungen

Die Ausführungsreihenfolge für Ausnahmefilter wurde für Ausnahmefilter geändert, die den Wert der gleichen Reihenfolge aufweisen. In ASP.NET MVC 2 und früher filtert Ausnahme auf dem Controller, mit der gleichen Reihenfolge wie die in einer Aktionsmethode vor den Ausnahmefiltern der Aktionsmethode ausgeführt wurden. Dies würde i. d. r. der Fall sein, wenn Ausnahmefilter ohne einen Reihenfolgenwert der angegebenen angewendet wurden. In ASP.NET MVC 3 wurde diese Reihenfolge umgekehrt, damit der spezifischste Ausnahmehandler zuerst ausgeführt wird. Wenn die Order-Eigenschaft explizit angegeben wird, werden die Filter wie in früheren Versionen in der angegebenen Reihenfolge ausgeführt.

## <a id="0.1__Toc274034230"></a>  Bekannte Probleme

Während der Installation zeigt im Dialogfeld zum Akzeptieren der LIZENZBEDINGUNGEN die Lizenzbedingungen in einem Fenster, die kleiner als vorgesehen ist.

Razor-Ansichten haben keine IntelliSense-Unterstützung und syntaxhervorhebung. Es ist davon auszugehen, dass-Unterstützung für Razor-Syntax in Visual Studio als Teil einer späteren Version enthalten sein wird.

Bei der Bearbeitung einer Razor-Ansicht (CSHTML-Datei), die <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> Gehe zu Controller-Menüelement in Visual Studio nicht zur Verfügung, und es gibt keine Codeausschnitte.

Bei Verwendung der @model Syntax zum Angeben von einem strikter CSHTML anzeigen, die sprachspezifische Tastenkombinationen für die Typen werden nicht erkannt. Z. B. @model Int funktioniert nicht, aber @model Int32 funktioniert. Die problemumgehung für diesen Fehler ist auf den tatsächlichen Typnamen zu verwenden, wenn Sie den Typ des Modells angeben.

Bei Verwendung der @model Syntax zum Angeben von einer stark typisierten CSHTML-Ansicht (oder @ModelType an eine stark typisierte VBHTML-Ansicht), auf NULL festlegbare Typen und Arraydeklarationen werden nicht unterstützt. Z. B. @model Int? wird nicht unterstützt. Verwenden Sie stattdessen `@model Nullable<Int32>`. Die Syntax @model String [] wird ebenfalls nicht unterstützt; verwenden Sie stattdessen `@model IList<string>`.

Wenn Sie ein ASP.NET MVC 2-Projekt auf ASP.NET MVC 3 aktualisieren, müssen Sie Folgendes zum Abschnitt "AppSettings" der Datei "Web.config" hinzufügen:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Es ist ein bekanntes Problem, das bewirkt, die Formularauthentifizierung nicht authentifizierte Benutzer immer ~/Account /-Anmeldung dass, ignoriert die Einstellung für die Formularauthentifizierung verwendet, die in "Web.config" umgeleitet. Die problemumgehung besteht darin, die folgenden app-Einstellung hinzufügen.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>  Haftungsausschluss

© 2011 Microsoft Corporation. Alle Rechte vorbehalten. In diesem Dokument wird bereitgestellt "als-ist." Informationen und Stellungnahmen in diesem Dokument einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung ändern. Sie tragen das alleinige Verwendungsrisiko.

Dieses Dokument stellt keinerlei Rechtsansprüche auf geistiges Eigentum in Microsoft-Produkten jeglicher Art bereit. Sie dürfen Dokument für interne Informationszwecke kopieren.
