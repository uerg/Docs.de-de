---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON und XML-Serialisierung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: b1fcaf70cc38d73da0a454764520197b97f34b26
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28038100"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON und XML-Serialisierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Dieser Artikel beschreibt die JSON und XML-Formatierer in ASP.NET Web-API.

In ASP.NET Web-API eine *medientypformatierer* ist ein Objekt, das haben kann:

- Read-CLR-Objekte aus einem HTTP-Nachrichtentext
- Schreiben von CLR-Objekte in einer HTTP-Nachrichtentexts

Web-API bietet die medientypformatierer für JSON und XML. Das Framework fügt diese Formatierer in die Pipeline in der Standardeinstellung. Clients können JSON oder XML im Accept-Header der HTTP-Anforderung anfordern.

## <a name="contents"></a>Inhalt

- [JSON-Medientypformatierer](#json_media_type_formatter)

    - [Schreibschutzeigenschaften](#json_readonly)
    - [Datumsangaben](#json_dates)
    - [Festlegen von Einzügen](#json_indenting)
    - [Kamel-Schreibweise](#json_camelcasing)
    - [Anonyme als auch schwach typisierte Objekte](#json_anon)
- [XML-Medientypformatierer](#xml_media_type_formatter)

    - [Schreibschutzeigenschaften](#xml_readonly)
    - [Datumsangaben](#xml_dates)
    - [Festlegen von Einzügen](#xml_indenting)
    - [Festlegen von pro-Type-XML-Serialisierungsprogrammen](#xml_pertype)
- [Entfernen des JSON oder XML-Formatierer](#removing_the_json_or_xml_formatter)
- [Behandlung von zirkuläre Objektverweise](#handling_circular_object_references)
- [Testen die Serialisierung von Objekten](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>JSON-Medientypformatierer

JSON-Formatierung bereitgestellt wird, durch die **JsonMediaTypeFormatter** Klasse. Standardmäßig **JsonMediaTypeFormatter** verwendet die [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Bibliothek, die Serialisierungen durchführen. Json.NET ist ein Drittanbieter-open-Source-Projekt.

Wenn Sie dies bevorzugen, können Sie konfigurieren die **JsonMediaTypeFormatter** -Klasse die **DataContractJsonSerializer** anstelle von Json.NET. Legen Sie hierzu die **UseDataContractJsonSerializer** Eigenschaft **"true"**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON-Serialisierung

Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von der JSON-Formatierer, die unter Verwendung des standardmäßigen [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Serialisierungsprogramm. Dies ist nicht vorgesehen, eine umfassende Dokumentation der Bibliothek Json.NET sein; Weitere Informationen finden Sie unter der [Json.NET Dokumentation](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Was serialisiert werden?

Standardmäßig sind alle öffentlichen Eigenschaften und Felder in der JSON-serialisierten enthalten. Um eine Eigenschaft oder ein Feld zu unterdrücken, ergänzen sie die **JsonIgnore** Attribut.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Falls gewünscht ein &quot;teilnehmen&quot; Ansatz, ergänzen Sie die Klasse mit dem **DataContract** Attribut. Wenn dieses Attribut vorhanden ist, werden Elemente ignoriert, es sei denn, sie haben die **DataMember**. Sie können auch **DataMember** Private Member zu serialisieren.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Schreibschutzeigenschaften

Schreibgeschützte Eigenschaften werden standardmäßig serialisiert.

<a id="json_dates"></a>
### <a name="dates"></a>Datumsangaben

Standardmäßig schreibt Json.NET Datumsangaben in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) Format. Datumsangaben, UTC (Coordinated Universal Time) werden mit dem Suffix "Z" geschrieben. Datumsangaben in Ortszeit enthalten einen Zeitzonenoffset. Zum Beispiel:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Standardmäßig behält Json.NET der Zeitzone. Dies kann durch Festlegen der Eigenschaft DateTimeZoneHandling überschrieben werden:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Wenn Sie es vorziehen, verwenden [Microsoft JSON-Datumsformat](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) statt ISO 8601, setzen die **DateFormatHandling** Eigenschaft auf die serialisierereinstellungen:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Einzug

Um eingezogen JSON zu schreiben, legen die **Formatierung** auf **Formatting.Indented**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Kamel-Schreibweise

Um JSON-Eigenschaftennamen mit Kamel-Schreibweise, schreiben, ohne Ihr Datenmodell ändern, legen die **CamelCasePropertyNamesContractResolver** auf das Serialisierungsprogramm:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Anonyme als auch schwach typisierte Objekte

Eine Aktionsmethode kann ein anonymes Objekt zurückgeben und deren Serialisierung in JSON. Zum Beispiel:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Der Nachrichtentext der Antwort enthält die folgenden JSON-Code:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Wenn Ihre Web-API-lose empfängt JSON-Objekten von den Clients strukturiert sind, können Sie der Anforderungstext zum Deserialisieren einer **Newtonsoft.Json.Linq.JObject** Typ.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Es ist jedoch in der Regel besser, stark typisierte Datenobjekte verwenden. Klicken Sie dann brauchen Sie die Daten zu analysieren, und die Vorteile der modellvalidierung erhalten.

Das XML-Serialisierungsprogramm unterstützt keine anonyme Typen oder **JObject** Instanzen. Wenn Sie diese Funktionen für die JSON-Daten verwenden, sollten Sie die XML-Formatierer aus der Pipeline entfernen, wie weiter unten in diesem Artikel beschrieben.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML-Medientypformatierer

XML-Formatierung wird bereitgestellt, indem Sie die **XmlMediaTypeFormatter** Klasse. Standardmäßig **XmlMediaTypeFormatter** verwendet die **"DataContractSerializer"** Klasse, die Serialisierungen durchführen.

Falls gewünscht, können Sie konfigurieren die **XmlMediaTypeFormatter** verwenden die **XmlSerializer** anstelle von der **"DataContractSerializer"**. Legen Sie hierzu die **UseXmlSerializer** Eigenschaft **"true"**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

Die **XmlSerializer** Klasse unterstützt einen engeren Satz von Typen als **"DataContractSerializer"**, bietet jedoch mehr Kontrolle über die resultierenden XML-Daten. Erwägen Sie **XmlSerializer** Wenn Sie ein vorhandenes XML-Schema entsprechen müssen.

### <a name="xml-serialization"></a>XML-Serialisierung

Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von der XML-Formatierer, der unter Verwendung des standardmäßigen **"DataContractSerializer"**.

Standardmäßig verhält sich "DataContractSerializer" wie folgt:

- Alle öffentlichen Lese-/Schreibeigenschaften und Felder serialisiert werden. Um eine Eigenschaft oder ein Feld zu unterdrücken, ergänzen sie die **IgnoreDataMember** Attribut.
- Private und geschützte Member werden nicht serialisiert.
- Schreibgeschützte Eigenschaften werden nicht serialisiert. (Allerdings werden der Inhalt einer schreibgeschützten Auflistung Eigenschaft serialisiert.)
- Klassen- und Membernamen werden in der XML-Code geschrieben werden, genau so, wie sie in der Klassendeklaration angezeigt werden.
- Ein XML-Standardnamespace wird verwendet.

Wenn Sie mehr Kontrolle über die Serialisierung benötigen, können Sie die Klasse mit ergänzen die **DataContract** Attribut. Wenn dieses Attribut vorhanden ist, wird die Klasse wie folgt serialisiert:

- &quot;Abonnieren von&quot; Ansatz: Eigenschaften und Felder werden standardmäßig nicht serialisiert. Um eine Eigenschaft oder ein Feld zu serialisieren, ergänzen sie die **DataMember** Attribut.
- Um eine private oder geschützte Member zu serialisieren, ergänzen sie die **DataMember** Attribut.
- Schreibgeschützte Eigenschaften werden nicht serialisiert.
- Um ändern, wie der Name der Klasse in der XML-Code angezeigt wird, legen Sie die *Namen* Parameter in der **DataContract** Attribut.
- Legen Sie zum Ändern der Darstellung der eines Elementnamen in der XML-Code die *Namen* Parameter in der **DataMember** Attribut.
- Um den XML-Namespace zu ändern, legen die *Namespace* Parameter in der **DataContract** Klasse.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Schreibschutzeigenschaften

Schreibgeschützte Eigenschaften werden nicht serialisiert. Wenn eine schreibgeschützte Eigenschaft ein private dahinter liegende Feld verfügt, können, markieren Sie das private Feld mit der **DataMember** Attribut. Dieser Ansatz erfordert das **DataContract** -Attribut für die Klasse.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Datumsangaben

Datumsangaben werden im ISO 8601-Format geschrieben. Beispielsweise &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Einzug

Um eingezogene XML zu schreiben, legen die **Einzug** Eigenschaft **"true"**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Festlegen von pro-Type-XML-Serialisierungsprogrammen

Sie können verschiedene XML-Serialisierer für andere CLR-Typen festlegen. Angenommen, Sie müssen möglicherweise ein bestimmtes Datenobjekt, das erfordert **XmlSerializer** Gründen der Abwärtskompatibilität. Sie können **XmlSerializer** für dieses Objekt aus, und verwenden weiterhin **"DataContractSerializer"** für andere Typen.

Rufen Sie zum Festlegen einer XML-Serialisierungsprogramm für einen bestimmten Typ **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Sie können angeben, ein **XmlSerializer** oder jedes Objekt, das von abgeleitet ist **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Entfernen des JSON oder XML-Formatierer

Sie können die JSON-Formatierungsprogramm oder der XML-Formatierer aus der Liste der Formatierer, entfernen, wenn Sie nicht verwenden möchten. Die Gründe hierfür sind:

- Beschränken Sie Ihre Web-API-Antworten auf einen bestimmten Medientyp. Sie könnten z. B. nur JSON-Antworten zu unterstützen, und entfernen die XML-Formatierer.
- Der Standardformatierer durch ein benutzerdefiniertes Formatierungsprogramm ersetzen. Beispielsweise konnte durch eine eigene benutzerdefinierte Implementierung einer JSON-Formatierung den JSON-Formatierer ersetzt werden.

Der folgende Code zeigt, wie Sie die Standard-Formatierer entfernen. Rufen Sie diese von Ihrem **Anwendung\_starten** Methode, die in "Global.asax" definiert.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Behandlung von zirkuläre Objektverweise

Die JSON- und XML-Formatierer Schreiben aller Objekte als Werte. Wenn zwei Eigenschaften auf dasselbe Objekt verweisen oder das gleiche Objekt zweimal in einer Auflistung angezeigt wird, wird das Formatierungsprogramm Serialisieren des Objekts zweimal. Dies ist ein bestimmtes Problem enthält Ihre Objektdiagramm Zyklen, da das Serialisierungsprogramm eine Ausnahme auslöst, wenn eine Schleife im Diagramm erkannt wird.

Betrachten Sie den folgenden Objektmodellen und Controller.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Diese Aktion wird das Formatierungsprogramm löst das Aufrufen eine Ausnahme aus, das in einem Status Code 500 (Interner Serverfehler) Antwort an den Client übersetzt ausgelöst.

Um Objektverweise im JSON-Format zu erhalten, fügen Sie den folgenden Code zum **Anwendung\_starten** Methode in der Datei "Global.asax":

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Jetzt wird der Controlleraktion JSON zurück, die wie folgt aussieht:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Beachten Sie, die das Serialisierungsprogramm fügt eine &quot;$id&quot; Eigenschaft, um beide Objekte. Erkennt außerdem, dass die Employee.Department-Eigenschaft eine Schleife erstellt, sodass es den Wert durch einen Objektverweis ersetzt: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Objektverweise sind nicht in JSON standard. Beachten Sie, ob die Clients die Ergebnisse analysieren können, bevor Sie mit dieser Funktion. Es möglicherweise besser einfach, Zyklen aus dem Diagramm zu entfernen. In diesem Beispiel wird die Verknüpfung zwischen Mitarbeiter an Abteilung z. B. wirklich nicht erforderlich.


Um Objektverweise im XML-Format zu erhalten, müssen Sie zwei Optionen zur Verfügung. Die einfachere Möglichkeit besteht darin hinzufügen `[DataContract(IsReference=true)]` der Modell-Klasse. Die *IsReference* Parameter ermöglicht Objektverweise. Beachten Sie, dass **DataContract** Serialisierung teilnehmen können, stellt, daher Sie auch hinzufügen müssen **DataMember** -Attribute verwenden, um die Eigenschaften:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Nun das Formatierungsprogramm XML-Code ähnelt generiert, die folgenden:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Wenn Sie Attribute in Ihrer Modellklasse vermeiden möchten, ist es eine weitere Option: Erstellen einer neuen typspezifische **"DataContractSerializer"** -Instanz, und legen Sie *PreserveObjectReferences* zu **"true"**  im Konstruktor. Legen Sie dann diese Instanz als ein Serialisierungsprogramm pro Typ, auf die XML-medientypformatierer. Der folgende Code zeigt, wie dies:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testen die Serialisierung von Objekten

Wie Sie Ihre Web-API entworfen haben, ist es hilfreich, testen, wie Ihre Datenobjekte serialisiert werden. Dies ist möglich, ohne Erstellen eines Domänencontrollers oder eine Controlleraktion aufgerufen.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
