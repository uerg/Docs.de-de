---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Neues in ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Dieses Dokument beschreibt die neuen Features und Verbesserungen in ASP.NET MVC 2 eingeführt. Dieses Dokument ist auch zum Download zur Verfügung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 808f51b48b31e21848d76e7ded436ca1b17901d2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885231"
---
<a name="whats-new-in-aspnet-mvc-2"></a>Was ist neu in ASP.NET MVC 2
====================
> Dieses Dokument beschreibt die neuen Features und Verbesserungen in ASP.NET MVC 2 eingeführt. Dieses Dokument ist auch zur [herunterladen](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Einführung](#_TOC1)   
[Aktualisieren eines ASP.NET MVC 1,0-Projekts zu ASP.NET MVC 2](#_TOC2)   
[Neue Funktionen](#_TOC3)   
[Mit Hilfsvorlagen](#_TOC3_1)   
[Bereiche](#_TOC3_2)   
[Unterstützung für asynchrone Controller](#_TOC3_3)   
[Unterstützung für DefaultValueAttribute in Aktionsmethodenparameter](#_TOC3_4)   
[Unterstützung für das Binden von Binärdaten mit Modellbinder](#_TOC3_5)   
[ModelMetadata und ModelMetadataProvider-Klassen](#_TOC3_6)   
[Unterstützung für DataAnnotations-Attributen](#_TOC3_7)   
[Modellvalidierungssteuerelement-Anbieter](#_TOC3_8)   
[Die clientseitige Validierung](#_TOC3_9)   
[Neue Codeausschnitte für Visual Studio 2010](#_TOC3_10)   
[Neue RequireHttpsAttribute Action-Filter](#_TOC3_11)   
[Überschreiben die Methode für HTTP-Verb](#_TOC3_12)   
[Für Hilfsvorlagen neue HiddenInputAttribute-Klasse](#_TOC3_13)   
[Hilfsmethode Html.ValidationSummary kann Fehler auf Modellebene angezeigt.](#_TOC3_14)   
[T4-Vorlagen in Visual Studio Code generieren, die spezifisch auf die Zielversion von .NET Framework](#_TOC3_15)[API-Optimierungen](#_TOC4)  
[Wichtige Änderungen](#_TOC5)  
[Haftungsausschluss](#_TOC6)  

## <a id="_TOC1"></a>Einführung

ASP.NET MVC 2 baut auf ASP.NET MVC 1.0 und führt einen großen Satz von Verbesserungen und Features mit dem Schwerpunkt Produktivität zu steigern. Diese Version ist kompatibel mit ASP.NET MVC 1,0, damit alle Knowledge, wissen, Code und -Erweiterungen für ASP.NET MVC 1,0 gelten weiterhin.

Weitere Informationen zu ASP.NET MVC finden Sie auf den folgenden Ressourcen:

- [ASP.NET MVC-Dokumentation auf MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Der ASP.NET MVC-Website](https://asp.net/mvc/)
- [Die ASP.NET MVC-Foren](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>Aktualisieren eines ASP.NET MVC 1,0-Projekts zu ASP.NET MVC 2

ASP.NET MVC 2 kann parallel zu ASP.NET MVC 1.0 auf dem gleichen Server installiert werden die Anwendung Entwickler Flexibilität bei der Wahl der Zeitpunkt des Upgrades von einer ASP.NET MVC 1.0-Anwendung in ASP.NET MVC 2 gewährt. Informationen zum upgrade finden Sie in das Dokument [Aktualisieren einer ASP.NET MVC-Anwendung 1.0 in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>Neue Funktionen

Dieser Abschnitt beschreibt die Funktionen, die eingeführt wurden in der MVC 2-Version.

### <a id="_TOC3_1"></a>Mit Hilfsvorlagen

Mit Hilfsvorlagen können Sie automatisch zuordnen HTML-Elementen zur Bearbeitung und mit Datentypen angezeigt. Z. B. wenn Daten vom Typ System.DateTime in einer Ansicht angezeigt werden, kann ein Datumsauswahl Benutzeroberflächenelement automatisch gerendert werden. Dies ähnelt der Funktionsweise von Feldvorlagen in ASP.NET Dynamic Data. Weitere Informationen finden Sie unter [Hilfsvorlagen zum Anzeigen von Daten mithilfe von](https://go.microsoft.com/fwlink/?LinkId=159062) auf der MSDN-Website.

### <a id="_TOC3_2"></a>Bereiche

Bereiche können Sie ein umfangreiches Projekt in mehrere kleinere Abschnitte zu organisieren, um die Komplexität einer großen Web-Anwendung zu verwalten. Jeder Abschnitt ("Bereich") in der Regel stellt einen separaten Abschnitt eine große Website dar und dient zum Gruppieren von verwandten Sätze von Controllern und Ansichten. Weitere Informationen finden Sie unter [Exemplarische Vorgehensweise: Organisieren einer ASP.NET MVC-Anwendung durch Bereiche](https://go.microsoft.com/fwlink/?LinkId=158978) auf der MSDN-Website.

Zum Erstellen eines neuen Bereichs im Projektmappen-Explorer mit der rechten Maustaste des Projekts, klicken Sie auf Hinzufügen, und klicken Sie dann auf Bereich. Dies zeigt ein Dialogfeld an, die Sie für den Bereichsnamen aufgefordert werden. Nachdem Sie den Bereichsnamen eingeben, wird dem Projekt von Visual Studio einen neuen Bereich hinzugefügt.

Die folgende Abbildung zeigt ein Beispiellayout für ein Projekt mit zwei Bereichen, Admin und Blogs.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Wenn Sie einen Bereich erstellen, fügt Visual Studio eine Klasse, die auf den jeweiligen Bereich AreaRegistration abgeleitet. Diese Klasse ist erforderlich, um den Bereich und seine Routen zu registrieren, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Die Standard-Projektvorlage für ASP.NET MVC 2 enthält einen Aufruf an die RegisterAllAreas-Methode im Code für die Datei "Global.asax". Diese Methode registriert jeden Bereich im Projekt durch suchen alle Typen, die von der AreaRegistration-Klasse abgeleitet werden, Instanziieren einer Instanz des Typs, und klicken Sie dann die RegisterArea-Methode in der Instanz aufgerufen. Das folgende Beispiel zeigt, wie dies funktioniert.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Wenn Sie nicht den Namespace in der Methode RegisterArea angeben durch Aufrufen des Kontexts. Namespaces.Add-Methode, den Namespace der Registrierungsklasse wird standardmäßig verwendet.

### <a id="_TOC3_3"></a>Unterstützung für asynchrone Controller

ASP.NET MVC 2 können jetzt Anforderungen asynchron zu verarbeiten. Dies kann zu einer Verbesserung der Leistung führen, können Sie die Server, die häufig blockierende Vorgänge (z. B. netzwerkanforderungen) aufrufen, um die nicht blockierend Entsprechungen rufen stattdessen. Weitere Informationen finden Sie unter der [Verwenden eines asynchronen Controllers in ASP.NET-MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) Thema auf MSDN.

### <a id="_TOC3_4"></a>Unterstützung für DefaultValueAttribute in Aktionsmethodenparameter

Die Klasse System.ComponentModel.DefaultValueAttribute an ermöglicht es sich um einen Standardwert für den Argumentparameter an eine Aktionsmethode bereitgestellt werden. Nehmen wir beispielsweise an, dass die folgenden Standardroute definiert ist:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Darüber hinaus vorausgesetzt, dass die folgende Methode der Controller und Aktion definiert ist:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Keines der folgenden Anforderung werden URLs die anzeigen-Action-Methode aufrufen, die im vorherigen Beispiel definiert ist.

- /Article/View/123
- / Artikel/Sicht/123? Seite = 1 (effektiv identisch mit der vorhergehenden Anforderung)
- /Article/View/123?page=2

Die erste URL aus der vorangehenden Liste würde ohne DefaultValueAttribute-Attribut nicht funktionieren, da das Seitenargument ein NULL-Werttyp ist, dessen Wert nicht angegeben wurde.

Wenn Ihr Code in Visual Basic 2010 oder Visual c# 2010 geschrieben wird, können Sie anstelle der DefaultValueAttribute-Attributs, optionale Parameter verwenden, wie im folgenden Beispiel gezeigt:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>Unterstützung für das Binden von Binärdaten mit Modellbinder

Es gibt zwei neue Überladungen der Html.Hidden-Hilfsprogramm, die Binärwerte als Base-64-codierte Zeichenfolgen codiert werden:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Eine typische Verwendung ist so betten Sie einen Zeitstempel für ein Objekt in der Ansicht ein. Beispielsweise könnte die Anwendung die folgenden Produktobjekt enthalten:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Ein Bearbeitungsformular, kann die TimeStamp-Eigenschaft in der Form, wie im folgenden Beispiel gezeigt Rendern:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Dieses Markup rendert ein verborgenes Eingabeelement mit dem Timestamp-Wert als Base-64-codierte Zeichenfolge, die das folgende Beispiel ähnelt:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Dieses Formular kann zu einer Aktionsmethode, die ein Argument des Typs Produkt bereitgestellt werden, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

In der Aktionsmethode wird die TimeStamp-Eigenschaft ordnungsgemäß aufgefüllt, da die bereitgestellte, Base-64-codierte Zeichenfolge in ein Bytearray konvertiert wird.

### <a id="_TOC3_6"></a>ModelMetadata und ModelMetadataProvider-Klassen

Die ModelMetadataProvider-Klasse stellt eine Abstraktion zum Abrufen von Metadaten für das Modell in einer Ansicht bereit. MVC 2 enthält einen Standardanbieter, der die Metadaten zur Verfügung, die durch die Attribute im System.ComponentModel.DataAnnotations-Namespace verfügbar gemacht wird. Es ist möglich, Modellmetadaten-Anbieter erstellen, die Metadaten aus anderen Datenspeichern, wie Datenbanken oder XML-Dateien bereitstellen.

Die ViewDataDictionary-Klasse macht ein ModelMetadata-Objekt, das Metadaten enthält, das von der Klasse ModelMetadataProvider aus dem Modell extrahiert wird. Dies ermöglicht die Hilfsvorlagen nutzen diese Metadaten und ihre Ausgabe entsprechend anpassen.

Weitere Informationen finden Sie in der Dokumentation für die [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) und [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) Klassen.

### <a id="_TOC3_7"></a>Unterstützung für DataAnnotations-Attributen

ASP.NET MVC 2 unterstützt die Verwendung der Validierungsattribute RangeAttribute, RequiredAttribute ' StringLengthAttribute ' und RegexAttribute (definiert im System.ComponentModel.DataAnnotations-Namespace) beim Binden an ein Modell um die Bereitstellung von Eingaben Überprüfung.

Weitere Informationen finden Sie unter [Vorgehensweise: Validieren Modell Daten mithilfe von DataAnnotations-Attributen](https://go.microsoft.com/fwlink/?LinkId=159063) auf der MSDN-Website. Ein Beispielprojekt, in dem veranschaulicht die Verwendung dieser Attribute ist zum Download verfügbar unter [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>Modellvalidierungssteuerelement-Anbieter

Die modellvalidierung Provider-Klasse stellt eine Abstraktion, die Validierungslogik für das Modell bereitstellt. ASP.NET MVC umfasst einen Standardanbieter basierend auf Validierungsattribute, die im System.ComponentModel.DataAnnotations-Namespace enthalten sind. Sie können auch eigene Validierungsanbietern erstellen, die benutzerdefinierte Validierungsregeln und benutzerdefinierten Zuordnungen von Validierungsregeln für das Modell zu definieren. Weitere Informationen finden Sie in der Dokumentation für die [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) Klasse.

### <a id="_TOC3_9"></a>Die clientseitige Validierung

Die Anbieterklasse Modellvalidierungssteuerelement-macht Validierungsmetadaten an den Browser in Form von JSON-serialisierten Daten, die von einer Bibliothek für die clientseitige Validierung genutzt werden können. ASP.NET MVC 2 umfasst eine Clientbibliothek für die Validierung und die Adapter, die gemäß den zuvor notierten DataAnnotations-Namespace überprüfungsattribute unterstützt. Die Anbieterklasse ermöglicht Ihnen die Verwendung von anderen Clientvalidierung Bibliotheken durch das Schreiben eines Adapters, das die JSON-Daten und Aufrufe in die alternative Bibliothek verarbeitet.

### <a id="_TOC3_10"></a>Neue Codeausschnitte für Visual Studio 2010

Ein Satz von HTML-Codeausschnitte für ASP.NET MVC 2 wird mit Visual Studio 2010 installiert. Wählen Sie zum Anzeigen einer Liste von diesen coeausschnitten im Menü "Extras" Codeausschnitt-Manager ein. Klicken Sie für die Sprache wählen Sie HTML aus, und wählen Sie für Location: ASP.NET MVC 2. Weitere Informationen zum Verwenden von Codeausschnitten finden Sie unter der Visual Studio-Dokumentation.

### <a id="_TOC3_11"></a>Neue RequireHttpsAttribute Action-Filter

ASP.NET MVC 2 umfasst eine neue RequireHttpsAttribute-Klasse, die an Aktionsmethoden und Controller angewendet werden kann. Standardmäßig leitet der Filter eine nicht - SSL-HTTP-Anforderung in die entsprechende SSL-fähigen (HTTPS) an.

### <a id="_TOC3_12"></a>Überschreiben die Methode für HTTP-Verb

Wenn Sie eine Website mithilfe von architektonische REST-Stil erstellen, werden HTTP-Verben verwendet, um zu bestimmen, welche Aktion Sie für eine Ressource ausführen. REST ist erforderlich, diese Anwendungen unterstützen das gesamte Spektrum der allgemeine HTTP-Verben, z. B. GET, PUT, POST und löschen.

ASP.NET MVC 2 enthält neue Attribute, die Sie dieses Feature kompakte Syntax zu Aktionsmethoden anwenden können. Diese Attribute ermöglichen, ASP.NET MVC auswählen eine Aktionsmethode, die basierend auf dem HTTP-Verb. Im folgenden Beispiel eine POST-Anforderung wird die erste Aktionsmethode aufrufen, und eine PUT-Anforderung die zweite Aktionsmethode.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

In früheren Versionen von ASP.NET MVC erforderlich diese Aktionsmethoden ausführlicheren Syntax, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Da Browser nur Get- und POST HTTP-Verben unterstützen, ist es nicht möglich, einer Aktion bereitstellen, die einen anderen Verb erfordert. Daher ist es nicht möglich, alle RESTful Anforderungen systemeigen unterstützen.

Zur Unterstützung von RESTful Anforderungen während des POST-Operationen mit ASP.NET-MVC 2 stellt jedoch eine neue HttpMethodOverride HTML-Hilfsmethode. Diese Methode rendert ein verborgenes Eingabeelement, das bewirkt, das Formular dass, um alle HTTP-Methode effektiv zu emulieren. Haben Sie beispielsweise mithilfe der HttpMethodOverride HTML-Hilfsmethode eine Formularübergabe angezeigt werden eine Put- oder DELETE-Anforderung. Das Verhalten des HttpMethodOverride wirkt sich auf die folgenden Attribute:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Die ausgeblendete input-Element verfügt über seinen Namen X-HTTP-Method-Override und dem Wert der HTTP-Verb zu emulieren. Der Überschreibungswert kann auch in einen HTTP-Header oder in einem Abfrage-Zeichenfolgenwert werden als ein Name/Wert-Paar angegeben.

Die Außerkraftsetzung kann nur verwendet werden, wenn die tatsächliche Anforderung eine POST-Anforderung ist. Der Überschreibungswert wird für Anforderungen ignoriert, die alle anderen HTTP-Verb zu verwenden.

### <a id="_TOC3_13"></a>Für Hilfsvorlagen neue HiddenInputAttribute-Klasse

Sie können das neue HiddenInputAttribute Attribut anwenden, eine Modelleigenschaft, um anzugeben, ob ein verborgenes Eingabeelement gerendert werden soll, wenn das Modell in eine Editor-Vorlage angezeigt. (Das Attribut legt impliziten UIHint Wert HiddenInput). DisplayValue-Eigenschaft des Attributs können Sie angeben, ob der Wert im Editor angezeigt wird und Anzeigemodi. Wenn DisplayValue auf "false" festgelegt ist, wird nichts angezeigt, auch nicht auf das HTML-Markup, das normalerweise auf ein Feld umgibt. Der Standardwert für DisplayValue ist "true".

Sie können HiddenInputAttribute-Attribut in den folgenden Szenarien verwenden:

- Wenn eine Sicht ermöglicht Benutzern, die die ID eines Objekts bearbeiten und muss ebenfalls den Wert angezeigt wird, um ein verborgenes Eingabeelement bereitzustellen, das die alte Kennung enthält, sodass sie wieder an den Controller übergeben werden können.
- Wenn eine Sicht kann Bearbeiten von Benutzern eine binäre Eigenschaft, die nie, z. B. eine Timestamp-Eigenschaft angezeigt werden sollen. In diesem Fall werden der Wert und den umgebenden HTML-Markup (z. B. die Beschriftung als auch Wert) nicht angezeigt.

Im folgende Beispiel wird gezeigt, wie die HiddenInputAttribute-Klasse verwendet wird.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Wenn das Attribut festgelegt ist auf "true" (oder kein Parameter angegeben wird), geschieht Folgendes:

- Klicken Sie im Anzeigevorlagen eine Bezeichnung wird gerendert, und der Wert für den Benutzer angezeigt wird.
- Klicken Sie im Editor-Vorlagen eine Bezeichnung wird gerendert, und der Wert wird in einem ausgeblendeten input-Element gerendert.

Wenn das Attribut auf "false" festgelegt ist, geschieht Folgendes:

- In Anzeigevorlagen wird keine Aktion für das Feld gerendert.
- Klicken Sie im Editorvorlagen keine Bezeichnung wird gerendert, und der Wert wird in einem ausgeblendeten input-Element gerendert.

### <a id="_TOC3_14"></a>Hilfsmethode Html.ValidationSummary kann Fehler auf Modellebene angezeigt.

Statt immer alle Validierungsfehler, hat die Hilfsmethode Html.ValidationSummary eine neue Option nur auf Modellebene angezeigt werden sollen. Dadurch wird auf Modellebene in der validierungszusammenfassung anzuzeigende und feldspezifischen Fehlern, die neben jedem Feld angezeigt werden.

### <a id="_TOC3_15"></a>T4-Vorlagen in Visual Studio Code generieren, die spezifisch auf die Zielversion von .NET Framework

Eine neue Eigenschaft ist verfügbar in T4-Dateien aus dem ASP.NET MVC T4-Host, der die Version von .NET Framework gibt an, die von der Anwendung verwendet wird. Dies ermöglicht eine T4-Vorlagen zum Generieren von Code und Markup, das speziell für eine Version von .NET Framework ist. In Visual Studio 2008 lautet der Wert immer .NET 3.5. In Visual Studio 2010 ist der Wert auf, .NET 3.5 oder .NET 4.

## <a id="_TOC4"></a>API-Optimierungen

Dieser Abschnitt beschreibt die Änderungen an vorhandenen ASP.NET MVC-Typen und Member.

- Eine geschützte virtuelle CreateActionInvoker-Methode hinzugefügt in der Controllerklasse. Diese Methode wird aufgerufen, indem die ActionInvoker-Eigenschaft des Controllers und für die verzögerte Instanziierung der aufrufenden Instanz ermöglicht wird, wenn keine aufrufende Instanz bereits festgelegt ist.
- Eine geschützte virtuelle HandleUnauthorizedRequest-Methode hinzugefügt in den AuthorizeAttribute-Klasse. Dadurch können Filter, mit die Ableiten von AuthorizeAttribute zum Steuern des Verhaltens, wenn ein Autorisierungsfehler auftritt.
- Eine Methode hinzufügen (Zeichenfolge, Objekt Schlüsselwert) hinzugefügt in die ValueProviderDictionary-Klasse. Dadurch können Sie die Syntax des Objektinitialisierers Wörterbuch für ValueProviderDictionary, wie im folgenden Beispiel gezeigt verwenden:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Einen Get hinzugefügt\_Objekt-Methode in der Sys.Mvc.AjaxContext-Klasse. Dies ist eine JavaScript-Methode, die die Get ähnelt\_get Data-Methode, aber wenn Sie der Inhaltstyp der Antwort Application/Json,\_Objekt zurückgibt, das JSON-Objekt.
- Eine ActionDescriptor-Eigenschaft hinzugefügt in der Klasse AuthorizationContext.
- Hinzugefügt ein UrlParameter.Optional-Token, das verwendet werden kann, um Probleme umgehen können beim Binden an ein Modell, das eine ID-Eigenschaft enthält, wenn die Eigenschaft nicht vorhanden ist in einem Formular-Post. Weitere Details finden Sie im Blogeintrag [ASP.NET MVC 2 optionale URL Parameter](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) Haacks Blog.

## <a id="_TOC5"></a>Wichtige Änderungen

Die folgenden Änderungen in vorhandene 1.0 für ASP.NET MVC-Anwendungen möglicherweise Fehler.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Ändern Sie die Eigenschaft Überprüfungsverhalten für Klassen, die IDataErrorInfo implementieren

Für Model-Objekte, mit denen IDataErrorInfo Überprüfung ausführen, wird jede Eigenschaft überprüft, unabhängig davon, ob ein neuer Wert festgelegt wurde. In ASP.NET MVC 1.0 wurden nur die Eigenschaften, auf die neuen Werte festgelegt wurden überprüft. In ASP.NET MVC 2 ist die Error-Eigenschaft des IDataErrorInfo nur aufgerufen, wenn alle Validierungssteuerelemente Eigenschaft erfolgreich waren.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Skript für IIS-Skript-Zuordnung ist nicht mehr verfügbar ist, im Installationsprogramm

Das IIS-Skriptzuordnung Skript ist ein Befehlszeilenskript, mit dem Skript-Zuordnungen im klassischen Modus für IIS 6 und IIS 7 zu konfigurieren. Das Skript Skriptzuordnung ist nicht erforderlich, wenn Sie Visual Studio Development Server verwenden oder wenn Sie IIS 7 im integrierten Modus verwenden. Die Skripts als separater Download auf nicht unterstützte stehen die [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>Die Hilfsmethode Html.Substitute in MVC Futures ist nicht mehr verfügbar

Aufgrund von Änderungen in das Renderingverhalten der Ansichtsmodule für MVC die Hilfsmethode Html.Substitute funktioniert nicht und entfernt wurde.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>Die Schnittstelle IValueProvider ersetzt alle Vorkommen von IDictionary

Jede Eigenschaft oder Methode-Argument, das jetzt IDictionary in MVC 1.0 akzeptiert akzeptiert IValueProvider. Diese Änderung betrifft nur Anwendungen, die Anbietern eines benutzerdefinierten Werts oder benutzerdefinierten Modellbinder enthalten. Die Beispiele von Eigenschaften und Methoden, die von dieser Änderung betroffen sind:

- Die Eigenschaft ValueProvider ControllerBase und ModelBindingContext Ereignisklassen.
- Der Controllerklasse TryUpdateModel-Methoden.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Neue CSS-Klassen wurden in der Datei "Site.CSS" ändern hinzugefügt.

Die Datei "Site.CSS" ändern, in der ASP.NET MVC-Projektvorlagen wurde aktualisiert, um neue Stile verwendet werden, indem Sie die Funktionen zur zertifikatsvalidierung und die Hilfsvorlagen einzuschließen.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Hilfsprogramme jetzt zurück MvcHtmlString Objekt.

Um die neue Ausdruckssyntax von HTML-Codierung in ASP.NET 4 nutzen zu können, ist der Rückgabetyp für HTML-Hilfsmethoden jetzt MvcHtmlString anstelle einer Zeichenfolge. Wenn Sie ASP.NET MVC 2 und die neue Hilfsprogramme in ASP.NET 3.5 verwenden, werden Sie nicht nutzen, die Syntax der HTML-Codierung werden; die neue Syntax steht nur, wenn Sie ASP.NET MVC 2 auf ASP.NET 4 ausführen.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult jetzt reagiert nur für HTTP POST-Anforderungen.

Um das Minderung JSON hijacking-Angriffe, die potenzielle Offenlegung von Informationen, in der Standardeinstellung haben reagiert die JsonResult-Klasse jetzt nur für HTTP POST-Anforderungen. AJAX abrufen Aufrufe zu Aktionsmethoden, die ein JsonResult-Objekt zurückgeben sollte geändert werden, um POST stattdessen zu verwenden. Bei Bedarf können Sie dieses Verhalten überschreiben, durch die neue JsonRequestBehavior-Eigenschaft des JsonResult festlegen. Weitere Informationen zu den potenziellen Angriff, finden Sie im Blogbeitrag [JSON Hijacking](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) Haacks Blog.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Modell und ModelType Eigenschaftensetter auf ModelBindingContext sind veraltet.

Die Klasse ModelBindingContext wurde eine neue festlegbare ModelMetadata-Eigenschaft hinzugefügt. Die neue Eigenschaft kapselt das Modell und die ModelType-Eigenschaften. Obwohl die Modell- und ModelType Eigenschaften veraltet sind, funktionieren für Abwärtskompatibilität Eigenschaftengetter weiterhin; Sie Delegieren der ModelMetadata-Eigenschaft zum Abrufen des Werts.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Änderungen an der Klasse DefaultControllerFactory unterbrechen benutzerdefinierte Controllerfactorys, die von ihr abgeleitet werden

Die Klasse DefaultControllerFactory wurde behoben, durch das Entfernen der RequestContext-Eigenschaft. Anstelle von dieser Eigenschaft wird die Anforderung Kontext-Instanz an den geschützten virtuellen GetControllerInstance und GetControllerType-Methode übergeben. Diese Änderung wirkt sich auf benutzerdefinierte Controllerfactorys, die von DefaultControllerFactory abgeleitet werden.

Benutzerdefinierte Controllerfactorys werden häufig verwendet, um Abhängigkeitsinjektion für ASP.NET MVC-Anwendung bereitstellen. Um die benutzerdefinierte Controllerfactorys zur Unterstützung von ASP.NET MVC 2 zu aktualisieren, ändern Sie die Signatur der Methode oder Signaturen, um die neue Signaturen übereinstimmen, und verwenden Sie die Anforderung Kontextparameter anstelle der Eigenschaft.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Bereich" erfolgt nun einen reservierten Route-Wert-Schlüssel

Der Zeichenfolge "Bereich" in Routenwerte verfügt jetzt über eine besondere Bedeutung in ASP.NET MVC in die gleiche Weise wie "Controller" und "Aktion". Eine Folge ist, dass wenn eine Route-Wert-Wörterbuch mit "Bereich" HTML-Hilfsmethoden bereitgestellt werden, die Hilfsprogramme nicht mehr "Bereich" in der Abfragezeichenfolge angefügt werden.

Wenn Sie die Bereiche-Funktion verwenden, stellen Sie sicher, {Bereich} nicht im Rahmen der Routen-URL zu verwenden.


## <a id="_TOC6"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor dem kommerziellen Release der beschriebenen Software ggf. erheblich geändert wird.

Die Informationen in diesem Dokument repräsentieren den aktuellen Standpunkt der Microsoft Corporation zu den erörterten Problemen zum Veröffentlichungstermin. Da Microsoft auf das Ändern von Marktlagen reagieren muss, ist das Dokument keinesfalls als Verpflichtung von Microsoft zu interpretieren, und Microsoft kann die Genauigkeit der Informationen nicht über den Zeitpunkt der Veröffentlichung hinaus garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation kein Teil dieses Dokuments für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, einer der Firmen, Organisationen, Produkte, Domänennamen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse in diesem Dokument dargestellten sind frei erfunden und keine Zuordnung zu echten Unternehmen, Organisation, Produkt, Domänennamen, e-Mail Adresse Logo "," Person "," Ort "oder" Ereignis dient, noch ableitbar.

© 2010 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
