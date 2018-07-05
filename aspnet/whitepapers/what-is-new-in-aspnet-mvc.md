---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Was ist neu in ASP.NET MVC 2 | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Dokument beschreibt die neuen Features und Verbesserungen in ASP.NET MVC 2 eingeführt wurden. Dieses Dokument ist auch zum Download zur Verfügung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: ''
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 51349c9967890675b4e13a6710572c1cea7d9337
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400430"
---
<a name="whats-new-in-aspnet-mvc-2"></a>Neuerungen in ASP.NET MVC 2
====================
> Dieses Dokument beschreibt die neuen Features und Verbesserungen in ASP.NET MVC 2 eingeführt wurden. In diesem Dokument finden Sie auch für [herunterladen](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Einführung in](#_TOC1)   
[Aktualisieren eines ASP.NET MVC 1.0-Projekts zu ASP.NET MVC 2](#_TOC2)   
[Neue Features](#_TOC3)   
[Vorlagenhelfer](#_TOC3_1)   
[Bereiche](#_TOC3_2)   
[Unterstützung für asynchrone Controller](#_TOC3_3)   
[Unterstützung für DefaultValueAttribute in Aktionsmethodenparameter](#_TOC3_4)   
[Unterstützung für das Binden von Binärdaten mit Modellbindungen](#_TOC3_5)   
[ModelMetadata und ModelMetadataProvider-Klassen](#_TOC3_6)   
[Unterstützung für DataAnnotations-Attributen](#_TOC3_7)   
[Modellvalidierungssteuerelement-Anbieter](#_TOC3_8)   
[Die clientseitige Validierung](#_TOC3_9)   
[Neue Codeausschnitte für Visual Studio 2010](#_TOC3_10)   
[Neue RequireHttpsAttribute Action-Filter](#_TOC3_11)   
[Überschreiben das Verb HTTP-Methode](#_TOC3_12)   
[Neue HiddenInputAttribute-Klasse für Vorlagenhelfer](#_TOC3_13)   
[Html.ValidationSummary Hilfsmethode kann Fehler auf Modellebene angezeigt.](#_TOC3_14)   
[T4-Vorlagen in Visual Studio Code generieren, die spezifisch auf die Zielversion von .NET Framework](#_TOC3_15)[API-Verbesserungen](#_TOC4)  
[Wichtige Änderungen](#_TOC5)  
[Haftungsausschluss](#_TOC6)  

## <a id="_TOC1"></a>  Einführung in

ASP.NET MVC 2 baut auf ASP.NET MVC 1.0 und führt zahlreiche Verbesserungen und Funktionen, die auf die Produktivität gesteigert. Diese Version ist kompatibel mit ASP.NET MVC 1.0, damit alle Ihre Kenntnisse, Fähigkeiten, Code und Erweiterungen für ASP.NET MVC 1.0 weiterhin angewendet.

Weitere Informationen zu ASP.NET MVC finden Sie auf den folgenden Ressourcen:

- [ASP.NET MVC-Dokumentation auf MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Die ASP.NET MVC-Website](https://asp.net/mvc/)
- [Die ASP.NET MVC-Foren](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>  Aktualisieren eines ASP.NET MVC 1.0-Projekts zu ASP.NET MVC 2

ASP.NET MVC 2 kann auf dem gleichen Server, wodurch Anwendung Entwickler Flexibilität bei der Entscheidung, wann eine ASP.NET MVC 1.0-Anwendung auf ASP.NET MVC 2 aktualisieren, parallel zu ASP.NET MVC 1.0 installiert werden. Informationen zum upgrade finden Sie im Dokument [aktualisieren eine ASP.NET MVC 1.0-Anwendung zu ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>  Neue Features

In diesem Abschnitt wird beschrieben, Funktionen, die eingeführt wurden, in der MVC 2-Version.

### <a id="_TOC3_1"></a>  Vorlagenhelfer

Hilfsvorlagen können Sie automatisch zuordnen HTML-Elemente für die Bearbeitung und mit Datentypen angezeigt. Beispielsweise wenn Daten vom Typ System.DateTime in einer Ansicht angezeigt werden, kann ein Datumsauswahl-Benutzeroberflächenelement automatisch gerendert werden. Dies ähnelt der Funktionsweise von Feldvorlagen in ASP.NET Dynamic Data. Weitere Informationen finden Sie unter [Hilfsvorlagen zum Anzeigen von Daten mithilfe von](https://go.microsoft.com/fwlink/?LinkId=159062) auf der MSDN-Website.

### <a id="_TOC3_2"></a>  Bereiche

Bereiche können Sie ein großes Projekt in mehrere kleinere Abschnitte zu organisieren, um die Komplexität einer großen Webanwendung zu verwalten. Jeder Abschnitt ("Bereich") in der Regel stellt einen separaten Abschnitt eine große Website dar und wird verwendet, um verwandte Controller und Ansichten zu gruppieren. Weitere Informationen finden Sie unter [Exemplarische Vorgehensweise: Organisieren von einer ASP.NET MVC-Anwendung durch Bereiche](https://go.microsoft.com/fwlink/?LinkId=158978) auf der MSDN-Website.

Zum Erstellen eines neuen Bereichs im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, klicken Sie auf Hinzufügen, und klicken Sie dann auf Bereich. Dies zeigt ein Dialogfeld, das Sie auffordert, den Bereichsnamen einzugeben. Nachdem Sie den Bereichsnamen eingeben, fügt Visual Studio einen neuen Bereich auf das Projekt an.

Die folgende Abbildung zeigt ein Beispiellayout für ein Projekt mit zwei Bereichen, Administrator und Blogs.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Wenn Sie einen Bereich erstellen, fügt Visual Studio eine Klasse, die zu den verschiedenen Bereichen AreaRegistration abgeleitet. Diese Klasse ist erforderlich, um den Bereich und die Routen, registrieren, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Die Standard-Projektvorlage für ASP.NET MVC 2 enthält einen Aufruf an die RegisterAllAreas-Methode im Code für die Datei "Global.asax". Diese Methode registriert jeden Bereich im Projekt durch sucht nach allen, die von der AreaRegistration-Klasse abgeleitet werden, eine Instanz des Typs instanziieren und rufen Sie dann die RegisterArea-Methode, auf die Instanz. Das folgende Beispiel zeigt, wie dies vonstatten geht.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Wenn Sie nicht den Namespace in die RegisterArea-Methode angeben durch Aufrufen des Kontexts. Namespaces.Add-Methode, den Namespace der Registrierungsklasse wird standardmäßig verwendet.

### <a id="_TOC3_3"></a>  Unterstützung für asynchrone Controller

ASP.NET MVC 2 können jetzt sowohl Anforderungen asynchron zu verarbeiten. Dies kann zu leistungsverbesserungen führen, von Servern, die häufig blockierende Vorgänge (z. B. netzwerkanforderungen) aufrufen, um eine nicht blockierende Entsprechungen rufen stattdessen ermöglicht. Weitere Informationen finden Sie unter den [Verwenden eines asynchronen Controllers in ASP.NET-MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) Thema auf MSDN.

### <a id="_TOC3_4"></a>  Unterstützung für DefaultValueAttribute in Aktionsmethodenparameter

Die System.ComponentModel.DefaultValueAttribute-Klasse ermöglicht es sich um einen Standardwert für den Argumentparameter zu einer Aktionsmethode angegeben werden. Nehmen wir beispielsweise an, dass die folgende Standardroute definiert ist:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Darüber hinaus vorausgesetzt, dass die folgende Methode der Controller und eine Aktion definiert ist:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Keines der folgenden Anforderung werden URLs die Aktionsmethode für die Ansicht aufgerufen, die im vorherigen Beispiel definiert ist.

- / Artikel/Ansicht/123
- / Artikel/Ansicht/123? Seite = 1 (identisch mit der vorherigen Anforderung)
- /Article/View/123?page=2

Ohne das Attribut DefaultValueAttribute würde die erste URL aus der obigen Liste nicht funktionieren, da das Seitenargument ein NULL-Werttyp ist, dessen Wert nicht bereitgestellt wurde.

Wenn Ihr Code in Visual Basic 2010 oder Visual c# 2010 geschrieben wird, können optionale Parameter anstatt des DefaultValueAttribute-Attributs Sie wie im folgenden Beispiel gezeigt:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>  Unterstützung für das Binden von Binärdaten mit Modellbindungen

Es gibt zwei neue Überladungen der Html.Hidden-Hilfe, die Binärwerte als Base-64-codierte Zeichenfolgen zu codieren:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Eine typische Verwendung ist einen Zeitstempel für ein Objekt in der Ansicht einbetten. Beispielsweise kann Ihre Anwendung die folgende Product-Objekt enthalten:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Ein Formular zum Bearbeiten kann es sich um die TimeStamp-Eigenschaft in der Form, wie im folgenden Beispiel gezeigt Rendern:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Dieses Markup rendert einen ausgeblendeten input-Element mit dem Timestamp-Wert als Base-64-codierte Zeichenfolge, die im folgende Beispiel ähnelt:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Dieses Formular möglicherweise zu einer Aktionsmethode, die ein Argument des Typs Produkt bereitgestellt werden, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

In der Aktionsmethode wird die TimeStamp-Eigenschaft ordnungsgemäß aufgefüllt, da die bereitgestellte, Base-64-codierte Zeichenfolge in ein Bytearray konvertiert wird.

### <a id="_TOC3_6"></a>  ModelMetadata und ModelMetadataProvider-Klassen

Die ModelMetadataProvider-Klasse stellt eine Abstraktion zum Abrufen von Metadaten für das Modell in einer Ansicht bereit. MVC 2 enthält einen Standardanbieter an, der die Metadaten zur Verfügung, die durch die Attribute in der System.ComponentModel.DataAnnotations-Namespace verfügbar gemacht wird. Es ist möglich, um Modellmetadaten-Anbieter zu erstellen, die Metadaten aus anderen Datenspeichern, z. B. Datenbanken oder XML-Dateien bereitstellen.

Die ViewDataDictionary-Klasse macht ein ModelMetadata-Objekt, das die Metadaten enthält, die von der Klasse ModelMetadataProvider aus dem Modell extrahiert werden. Dies ermöglicht die Hilfsvorlagen nutzen diese Metadaten und die Ausgabe entsprechend anpassen.

Weitere Informationen finden Sie in der Dokumentation für die [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) und [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) Klassen.

### <a id="_TOC3_7"></a>  Unterstützung für DataAnnotations-Attributen

ASP.NET MVC 2 unterstützt die Verwendung der Validierungsattribute RangeAttribute, RequiredAttribute, StringLengthAttribute und RegexAttribute (definiert in der System.ComponentModel.DataAnnotations-Namespace) nach der Bindung an ein Modell um die Bereitstellung von Eingaben die Überprüfung.

Weitere Informationen finden Sie unter [Vorgehensweise: Überprüfen Sie Daten mithilfe von "DataAnnotations" Modellattribute](https://go.microsoft.com/fwlink/?LinkId=159063) auf der MSDN-Website. Ein Beispielprojekt, das die Verwendung dieser Attribute veranschaulicht steht als Download im [ https://go.microsoft.com/fwlink/?LinkId=157753 ](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>  Modellvalidierungssteuerelement-Anbieter

Die modellvalidierung Provider-Klasse stellt eine Abstraktion, die Validierungslogik für das Modell bereitstellt. ASP.NET MVC umfasst einen Standardanbieter basierend auf den Validierungsattributen, die in der System.ComponentModel.DataAnnotations-Namespace enthalten sind. Sie können auch eigene Überprüfung-Anbieter erstellen, die benutzerdefinierte Validierungsregeln und benutzerdefinierten Zuordnungen von Validierungsregeln zum Modell zu definieren. Weitere Informationen finden Sie in der Dokumentation für die [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) Klasse.

### <a id="_TOC3_9"></a>  Die clientseitige Validierung

Die Modellvalidierungssteuerelement-Provider-Klasse verfügbar macht, Validierungsmetadaten an den Browser in Form von JSON-serialisierte Daten, die von einer Bibliothek für die clientseitige Validierung genutzt werden können. ASP.NET MVC 2 umfasst eine Clientbibliothek für die Validierung und die Adapter, die zuvor notierten DataAnnotations-Namespace Validierungsattribute unterstützt. Die Provider-Klasse können Sie mit anderen Bibliotheken Validierung durch Schreiben eines Adapters, das die JSON-Daten und die Aufrufe in die alternative Bibliothek verarbeitet auch.

### <a id="_TOC3_10"></a>  Neue Codeausschnitte für Visual Studio 2010

Ein Satz von HTML-Codeausschnitten für ASP.NET MVC 2 ist in Visual Studio 2010 installiert. Wählen Sie zum Anzeigen einer Liste der Codeausschnitte, in das Menü "Extras" Codeausschnitt-Manager ein. Klicken Sie für die Sprache wählen Sie HTML, und wählen Sie für Location: ASP.NET MVC 2. Weitere Informationen zum Verwenden von Codeausschnitten finden Sie unter der Visual Studio-Dokumentation.

### <a id="_TOC3_11"></a>  Neue RequireHttpsAttribute Action-Filter

ASP.NET MVC 2 umfasst eine neue RequireHttpsAttribute-Klasse, die auf Aktionsmethoden und Controller angewendet werden kann. Standardmäßig leitet der Filter eine nicht - SSL-HTTP-Anforderung in die entsprechende SSL-fähigen (HTTPS).

### <a id="_TOC3_12"></a>  Überschreiben das Verb HTTP-Methode

Wenn Sie eine Website mithilfe der architektonischen REST-Stil erstellen, werden HTTP-Verben verwendet, um zu bestimmen, welche Aktion für eine Ressource aus. REST ist erforderlich, diese Anwendungen unterstützen die vollständige Palette der häufig von HTTP-Verben GET, PUT, POST und DELETE.

ASP.NET MVC 2 enthält neue Attribute, die Sie dieses Feature kompakte Syntax zu Aktionsmethoden anwenden können. Diese Attribute ermöglichen, ASP.NET MVC zu eine Aktionsmethode, die basierend auf dem HTTP-Verb auswählen. Im folgenden Beispiel eine POST-Anforderung wird die erste Aktionsmethode aufrufen, und eine PUT-Anforderung wird die zweite Aktionsmethode aufrufen.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

In früheren Versionen von ASP.NET MVC erforderlich diese Aktionsmethoden ausführlicheren Syntax, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Da Browser nur die Get- und POST-HTTP-Verben unterstützt, ist es nicht möglich, die an eine Aktion gesendet werden soll, die eine andere Verb erforderlich sind. Daher ist es nicht möglich, alle RESTful-Anforderungen nativ unterstützen.

Zur Unterstützung von RESTful-basierten Anforderungen bei POST-Vorgänge, ASP.NET-MVC 2 führt jedoch eine neue HttpMethodOverride HTML-Hilfsmethode. Diese Methode rendert ein verborgenes Eingabeelement, das bewirkt, das Formular dass, um effektiv eine beliebige HTTP-Methode zu emulieren. Beispielsweise verwenden Sie die HttpMethodOverride HTML-Hilfsmethode, dass eine Übermittlung des Formulars angezeigt werden werden eine Put- oder DELETE-Anforderung. Das Verhalten der HttpMethodOverride wirkt sich auf die folgenden Attribute:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Die ausgeblendete input-Element hat seinen Namen X-HTTP-Method-Override und dessen Wert auf das HTTP-Verb zu emulieren. Der Wert kann auch als Name/Wert-Paar in einen HTTP-Header oder im Wert einer Abfragezeichenfolge angegeben, werden.

Die Außerkraftsetzung kann nur verwendet werden, wenn die echte Anforderung eine POST-Anforderung ist. Der Wert wird ignoriert für Anforderungen, die alle anderen HTTP-Verb zu verwenden.

### <a id="_TOC3_13"></a>  Neue HiddenInputAttribute-Klasse für Vorlagenhelfer

Sie können das neue HiddenInputAttribute Attribut anwenden, um eine Modelleigenschaft, um anzugeben, ob ein verborgenes Eingabeelement gerendert werden soll, wenn das Modell in einer Editor-Vorlage angezeigt. (Das-Attribut legt die impliziten Wert "UIHint" HiddenInput). Die Eigenschaft des Attributs DisplayValue können Sie angeben, ob der Wert im Editor angezeigt wird, und die Anzeigemodi. Wenn DisplayValue auf "false" festgelegt ist, wird nichts angezeigt, auch nicht auf das HTML-Markup, das normalerweise ein Feld umgeben ist. Der Standardwert für DisplayValue ist "true".

Sie können HiddenInputAttribute-Attribut in den folgenden Szenarien verwenden:

- Wenn eine Ansicht ermöglicht Benutzern, die die ID eines Objekts zu bearbeiten, und es ist erforderlich, die ebenfalls den Wert anzuzeigen, geben Sie ein verborgenes Eingabeelement, das die alte ID enthält, damit sie wieder an den Controller übergeben werden können.
- Wenn eine Sicht kann Bearbeiten von Benutzern eine binäre Eigenschaft, die nie, wie z. B. eine Timestamp-Eigenschaft angezeigt werden soll. In diesem Fall werden dem Wert und die umgebenden HTML-Markup (z. B. die Bezeichnung und Wert) nicht angezeigt.

Das folgende Beispiel zeigt, wie Sie mit der HiddenInputAttribute-Klasse.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Wenn das Attribut wurde festgelegt auf "true" (oder kein Parameter angegeben wird), geschieht Folgendes:

- Klicken Sie in den Anzeigevorlagen eine Bezeichnung wird gerendert, und der Wert wird dem Benutzer angezeigt.
- Klicken Sie im Editor-Vorlagen eine Bezeichnung wird gerendert, und der Wert wird in einem ausgeblendeten input-Element gerendert.

Wenn das Attribut auf "false" festgelegt ist, geschieht Folgendes:

- In den Anzeigevorlagen wird nichts für dieses Feld gerendert.
- Klicken Sie im Editor-Vorlagen keine Bezeichnung wird gerendert, und der Wert wird in einem ausgeblendeten input-Element gerendert.

### <a id="_TOC3_14"></a>  Html.ValidationSummary Hilfsmethode kann Fehler auf Modellebene angezeigt.

Anstatt immer alle Validierungsfehler, hat die Html.ValidationSummary-Hilfsmethode eine neue Option, um nur auf Modellebene Fehler anzuzeigen. Dadurch wird auf Modellebene in der validierungszusammenfassung anzuzeigende und feldspezifischen Fehlern, die neben jedem Feld angezeigt werden.

### <a id="_TOC3_15"></a>  T4-Vorlagen in Visual Studio Code generieren, die spezifisch auf die Zielversion von .NET Framework

Eine neue Eigenschaft ist verfügbar in T4-Dateien aus dem ASP.NET MVC T4-Host, der die Version von .NET Framework gibt an, die von der Anwendung verwendet wird. Dies ermöglicht die T4-Vorlagen zum Generieren von Code und Markup, das auf eine Version von .NET Framework spezifisch ist. In Visual Studio 2008 ist der Wert immer .NET 3.5. In Visual Studio 2010 ist der Wert auf, entweder .NET 3.5 oder .NET 4.

## <a id="_TOC4"></a>  API-Verbesserungen

Dieser Abschnitt beschreibt die Änderungen an vorhandenen ASP.NET MVC-Typen und Member.

- Eine geschützte virtuelle CreateActionInvoker-Methode hinzugefügt in der Controllerklasse. Diese Methode wird aufgerufen, indem die Eigenschaft "ActionInvoker" des Controllers und für die verzögerte Instanziierung von der aufrufenden Instanz ermöglicht wird, wenn keine aufrufenden Instanz bereits festgelegt ist.
- Eine geschützte virtuelle HandleUnauthorizedRequest-Methode hinzugefügt in der AuthorizeAttribute-Klasse. Dadurch können Filter, die sich von AuthorizeAttribute zur Steuerung des Verhaltens ableiten, wenn ein Autorisierungsfehler auftritt.
- Eine Methode hinzufügen (Zeichenfolge, Objekt Schlüsselwert) hinzugefügt in der Klasse ValueProviderDictionary. Dadurch können Sie die Syntax des Initialisierers Wörterbuch für ValueProviderDictionary, wie im folgenden Beispiel zu verwenden:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Hinzugefügt von Get\_Objekt-Methode in der Sys.Mvc.AjaxContext-Klasse. Dies ist eine JavaScript-Methode, die GET-Anforderung ähnelt\_get Data-Methode, jedoch ist, wenn der Inhaltstyp der Antwort Application/Json,\_-Objekt zurückgibt, das JSON-Objekt.
- Eine ActionDescriptor-Eigenschaft hinzugefügt, in der Klasse "AuthorizationContext" erwartet.
- Hinzugefügt von ein UrlParameter.Optional-Token, das verwendet werden kann, Sie Probleme umgehen, wenn auf ein Modell gebunden, die eine ID-Eigenschaft enthält, wenn die Eigenschaft nicht vorhanden ist in einer formularbereitstellung. Weitere Informationen finden Sie im Eintrag [ASP.NET MVC 2 optionale URL Parameter](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) Haacks Blog.

## <a id="_TOC5"></a>  Wichtige Änderungen

Die folgenden Änderungen in vorhandenen ASP.NET MVC 1.0-Anwendungen möglicherweise Fehler.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Ändern Sie in der Eigenschaft das Validierungsverhalten für Klassen, die IDataErrorInfo implementieren

Für Model-Objekte, die IDataErrorInfo für die Validierung verwenden, wird jede Eigenschaft überprüft, unabhängig davon, ob ein neuer Wert festgelegt wurde. In ASP.NET MVC 1.0 wurden nur die Eigenschaften, die neuen Werte, die festlegen mussten überprüft. In ASP.NET MVC 2 ist die Error-Eigenschaft von IDataErrorInfo nur aufgerufen, wenn alle Validierungssteuerelemente für die Eigenschaft erfolgreich waren.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Skript für das IIS-Skript-Zuordnung ist nicht mehr im Installer verfügbar

Das IIS-Skript-Zuordnung-Skript ist ein Befehlszeilenskript, mit der Skriptzuordnungen für IIS 6 und IIS 7 im klassischen Modus zu konfigurieren. Das Skript für die Skript-Zuordnung ist nicht erforderlich, wenn Sie Visual Studio Development Server verwenden oder bei Verwendung von IIS 7 im integrierten Modus. Die Skripts stehen als separater Download für nicht unterstützte die [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>Die Html.Substitute-Hilfsmethode in MVC Futures ist nicht mehr verfügbar

Aufgrund von Änderungen in das Renderingverhalten von MVC-Ansichts-Engines die Hilfsmethode Html.Substitute funktioniert nicht und entfernt wurde.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>Die Schnittstelle für IValueProvider ersetzt alle Vorkommen von IDictionary

Jede Eigenschaft oder Methode-Argument, das IDictionary in MVC 1.0 jetzt akzeptiert akzeptiert IValueProvider. Diese Änderung betrifft nur Anwendungen, die benutzerdefinierte Wertanbieter oder benutzerdefinierten modellbindern enthalten. Die folgenden: Beispiele von Eigenschaften und Methoden, die von dieser Änderung betroffen sind

- Die Eigenschaft ValueProvider von ControllerBase und ModelBindingContext-Klassen.
- Die TryUpdateModel Methoden der Controller-Klasse.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Es wurden neue CSS-Klassen in der Datei "Site.CSS" hinzugefügt.

Die Datei "Site.CSS" in den ASP.NET MVC-Projektvorlagen wurde aktualisiert, um neue Stile verwendet werden, indem Sie die Funktionen zur zertifikatsvalidierung und die Hilfsvorlagen einzuschließen.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Hilfsprogramme jetzt zurück ein MvcHtmlString-Objekt.

Um die neue Ausdruckssyntax von HTML-Codierung in ASP.NET 4 nutzen zu können, ist der Rückgabetyp für HTML-Hilfsprogramme jetzt MvcHtmlString anstelle einer Zeichenfolge. Wenn Sie ASP.NET MVC 2 sowie die neuen Hilfsprogramme in ASP.NET 3.5 verwenden, werden Sie nicht die Syntax der HTML-Codierung nutzen können; die neue Syntax ist verfügbar, nur, wenn Sie ASP.NET MVC 2 auf ASP.NET 4 ausführen.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult jetzt reagiert nur für HTTP POST-Anforderungen.

Damit verringern die JSON-hijacking-Angriffe, bei denen für die Offenlegung von Informationen, in der Standardeinstellung reagiert die JsonResult-Klasse jetzt nur für HTTP POST-Anforderungen. AJAX erhalten Aufrufe zu Aktionsmethoden, die ein JsonResult-Objekt zurückgeben sollte geändert werden, um POST stattdessen zu verwenden. Bei Bedarf können Sie dieses Verhalten überschreiben, durch die neue JsonRequestBehavior-Eigenschaft des JsonResult festlegen. Weitere Informationen zu den potenziellen Angriff, finden Sie im Blogbeitrag [JSON-Hijacking](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) Haacks Blog.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Modell und die Eigenschaften-Settern auf ModelBindingContext ModelType sind veraltet

Eine neue festlegbare ModelMetadata-Eigenschaft wurde auf die ModelBindingContext-Klasse hinzugefügt. Die neue Eigenschaft kapselt das Modell und die ModelType-Eigenschaften. Obwohl die Modell- und ModelType Eigenschaften veraltet sind, funktionieren für die Abwärtskompatibilität der Eigenschaftengetter weiterhin; delegiert an die ModelMetadata-Eigenschaft zum Abrufen des Werts.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Änderungen der DefaultControllerFactory-Klasse zur funktionsunfähigkeit von benutzerdefinierten Controllerfactorys, die von ihr abgeleitet werden

Die Klasse DefaultControllerFactory wurde behoben, durch das Entfernen der RequestContext-Eigenschaft. Anstelle dieser Eigenschaft wird die Anforderung Context-Instanz an die geschützten virtuellen GetControllerInstance und GetControllerType Methoden übergeben. Diese Änderung wirkt sich auf benutzerdefinierten Controllerfactorys, die von DefaultControllerFactory abgeleitet werden.

Benutzerdefinierte Controllerfactorys werden häufig verwendet, um den Dependency Injection für ASP.NET MVC-Anwendungen stellen. Um die benutzerdefinierten Controllerfactorys zur Unterstützung von ASP.NET MVC 2 zu aktualisieren, ändern Sie die Signatur der Methode oder Signaturen, um die neuen Signaturen übereinstimmen, und verwenden Sie anstelle der Eigenschaft des Kontextparameters für die Anforderung.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Area" ist nun ein reservierter routenwert-Schlüssel

Die Zeichenfolge "Bereich" in Routenwerten verfügt jetzt über eine besondere Bedeutung in ASP.NET MVC, auf die gleiche Weise, die "Controller" und "Action". Eine Folge ist, dass wenn HTML-Hilfsprogramme, die mit einer Route-Wert-Wörterbuch, das mit "Bereich" angegeben werden, die Hilfsprogramme nicht mehr "Bereich" in der Abfragezeichenfolge angefügt werden.

Wenn Sie das Feature für die Bereiche verwenden, sicher {"Bereich"} "nicht als Teil Ihrer Routen-URL zu verwenden.


## <a id="_TOC6"></a>  Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor dem kommerziellen Release der beschriebenen Software ggf. erheblich geändert wird.

Die Informationen in diesem Dokument repräsentieren den aktuellen Standpunkt der Microsoft Corporation zu den erörterten Problemen zum Veröffentlichungstermin. Da Microsoft auf das Ändern von Marktlagen reagieren muss, ist das Dokument keinesfalls als Verpflichtung von Microsoft zu interpretieren, und Microsoft kann die Genauigkeit der Informationen nicht über den Zeitpunkt der Veröffentlichung hinaus garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation kein Teil dieses Dokuments für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, die Firmen, Organisationen, Produkte, Domänennamen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse in diesen Beispielen sind frei erfunden, und mit jeder Firmen, Unternehmen, Produkt, Domänennamen, e-Mail-Adresse Adresse ","-Logo "," Person "," Ort "oder" Ereignis richtet sich an, oder rein zufällig.

© 2010 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
