---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: JSON- und XML-Serialisierung in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2012
ms.topic: article
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: e7fbcd41d64651255763c7629f0232788dcb3d30
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400920"
---
<a name="json-and-xml-serialization-in-aspnet-web-api"></a>JSON- und XML-Serialisierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Dieser Artikel beschreibt die JSON- und XML-Formatierer in ASP.NET Web-API.

In ASP.NET Web-API eine *medientypformatierer* ist ein Objekt, das können:

- Read-CLR-Objekte aus einer HTTP-Nachrichtentext
- Schreiben von CLR-Objekten in einen HTTP-Nachrichtentext

Web-API bietet die medientypformatierer für JSON- und XML. Das Framework fügt dieser Formatierer in die Pipeline in der Standardeinstellung. Clients können XML oder JSON im Accept-Header der HTTP-Anforderung anfordern.

## <a name="contents"></a>Inhalt

- [JSON-Medientypformatierer](#json_media_type_formatter)

    - [Schreibgeschützte Eigenschaften](#json_readonly)
    - [Datumsangaben](#json_dates)
    - [Einzug](#json_indenting)
    - [Kamel-Schreibweise](#json_camelcasing)
    - [Anonyme und schwach typisierte Objekte](#json_anon)
- [XML-Medientypformatierer](#xml_media_type_formatter)

    - [Schreibgeschützte Eigenschaften](#xml_readonly)
    - [Datumsangaben](#xml_dates)
    - [Einzug](#xml_indenting)
    - [Festlegen von pro-Type-XML-Serialisierungsprogrammen](#xml_pertype)
- [Entfernen die JSON- oder XML-Formatierungsprogramm](#removing_the_json_or_xml_formatter)
- [Behandeln von zyklischen Objektverweisen](#handling_circular_object_references)
- [Testen die Serialisierung von Objekten](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>JSON-Medientypformatierer

JSON-Formatierung erfolgt über die **JsonMediaTypeFormatter** Klasse. In der Standardeinstellung **JsonMediaTypeFormatter** verwendet die [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Bibliothek zur Serialisierung. Json.NET wird eine Drittanbieter-open-Source-Projekt.

Wenn Sie es vorziehen, können Sie konfigurieren die **JsonMediaTypeFormatter** zu verwendende Klasse an die **DataContractJsonSerializer** anstelle von Json.NET. Zu diesem Zweck legen Sie die **UseDataContractJsonSerializer** Eigenschaft **"true"**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>JSON-Serialisierung

Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von JSON-Formatierungsprogramm, über das standardmäßige [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Serialisierungsprogramm. Dies soll keine umfassende Dokumentation der JSON.NET-Bibliothek werden; Weitere Informationen finden Sie unter den [JSON.NET-Dokumentation](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>Was serialisiert werden?

Standardmäßig sind alle öffentlichen Eigenschaften und Felder in den serialisierten JSON-Code enthalten. Um eine Eigenschaft oder ein Feld zu unterdrücken, versehen sie mit der **JsonIgnore** Attribut.

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Falls gewünscht ein &quot;teilnehmen&quot; Ansatz, ergänzen die Klasse der **DataContract** Attribut. Wenn dieses Attribut vorhanden ist, werden Elemente ignoriert, es sei denn, sie haben die **DataMember**. Sie können auch **DataMember** Private Member zu serialisieren.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Schreibgeschützte Eigenschaften

Standardmäßig werden die schreibgeschützten Eigenschaften serialisiert.

<a id="json_dates"></a>
### <a name="dates"></a>Datumsangaben

Standardmäßig schreibt Json.NET Datumsangaben [ISO 8601](http://www.w3.org/TR/NOTE-datetime) Format. Datumsangaben in UTC (Coordinated Universal Time) werden mit dem Suffix "Z" geschrieben. Datumsangaben in Ortszeit enthalten einen Zeitzonenoffset. Zum Beispiel:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

Standardmäßig behält Json.NET die Zeitzone an. Sie können dies durch Festlegen der DateTimeZoneHandling-Eigenschaft außer Kraft setzen:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Wenn Sie lieber mit [Microsoft JSON-Datumsformat](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) anstelle von ISO 8601, legen Sie die **DateFormatHandling** Eigenschaft für die serialisierereinstellungen:

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
### <a name="anonymous-and-weakly-typed-objects"></a>Anonyme und schwach typisierte Objekte

Eine Aktionsmethode kann ein anonymes Objekt zurückgeben und deren Serialisierung in JSON. Zum Beispiel:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

Text der Antwortnachricht enthält die folgenden JSON-Code:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Wenn Ihre Web-API lose empfängt JSON-Objekte von den Clients strukturiert sind, können Sie den Hauptteil der Anforderung zum Deserialisieren einer **Newtonsoft.Json.Linq.JObject** Typ.

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Allerdings ist es normalerweise besser, stark typisierte Datenobjekte zu verwenden. Klicken Sie dann Sie müssen nicht die Daten zu analysieren, und erhalten Sie die Vorteile der modellvalidierung.

Das XML-Serialisierungsprogramm unterstützt keine anonyme Typen oder **"jobject"** Instanzen. Wenn Sie diese Funktionen für die JSON-Daten verwenden, sollten Sie das XML-Formatierungsprogramm aus der Pipeline entfernen, wie weiter unten in diesem Artikel beschrieben.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>XML-Medientypformatierer

XML-Formatierung erfolgt über die **XmlMediaTypeFormatter** Klasse. In der Standardeinstellung **XmlMediaTypeFormatter** verwendet die **DataContractSerializer** Klasse zur Serialisierung.

Falls gewünscht, können Sie konfigurieren die **XmlMediaTypeFormatter** verwenden die **XmlSerializer** anstelle von der **DataContractSerializer**. Zu diesem Zweck legen Sie die **UseXmlSerializer** Eigenschaft **"true"**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

Die **XmlSerializer** Klasse unterstützt eine eingeschränktere Gruppe von Typen als **DataContractSerializer**, jedoch bietet mehr Kontrolle über den sich ergebenden XML-Code. Erwägen Sie die Verwendung **XmlSerializer** Wenn Sie ein vorhandenes XML-Schema entsprechen müssen.

### <a name="xml-serialization"></a>XML-Serialisierung

Dieser Abschnitt beschreibt einige bestimmten Verhaltensweisen von der XML-Formatierer, der über das standardmäßige **DataContractSerializer**.

Standardmäßig verhält sich das DataContractSerializer-Element wie folgt:

- Es werden alle öffentlichen Lese-/Schreibeigenschaften und Felder serialisiert. Um eine Eigenschaft oder ein Feld zu unterdrücken, versehen sie mit der **IgnoreDataMember** Attribut.
- Private und geschützte Member werden nicht serialisiert.
- Schreibgeschützte Eigenschaften werden nicht serialisiert. (Allerdings werden die Inhalte einer nur-Lese Auflistungseigenschaft serialisiert.)
- Klassen- und Membernamen werden in der XML-Code geschrieben werden, genau wie in der Klassendeklaration.
- Es wird ein XML-Standardnamespace verwendet.

Wenn Sie mehr Kontrolle über die Serialisierung benötigen, können Sie ergänzen die Klasse der **DataContract** Attribut. Wenn dieses Attribut vorhanden ist, wird die Klasse wie folgt serialisiert:

- &quot;Abonnieren von&quot; Ansatz: Eigenschaften und Felder werden standardmäßig nicht serialisiert. Um eine Eigenschaft oder ein Feld serialisieren, versehen sie mit der **DataMember** Attribut.
- Um ein privates oder geschütztes Member zu serialisieren, versehen sie mit der **DataMember** Attribut.
- Schreibgeschützte Eigenschaften werden nicht serialisiert.
- Um ändern, wie der Klassenname in der XML-Code angezeigt wird, legen die *Namen* Parameter in der **DataContract** Attribut.
- Um ändern, wie ein Elementnamen in der XML-Code angezeigt wird, legen die *Namen* Parameter in der **DataMember** Attribut.
- Um den XML-Namespace zu ändern, legen die *Namespace* Parameter in der **DataContract** Klasse.

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Schreibgeschützte Eigenschaften

Schreibgeschützte Eigenschaften werden nicht serialisiert. Verfügt eine nur-Lese Eigenschaft auf ein privates Unterstützungsfeld, können Sie mit das private Feld markieren die **DataMember** Attribut. Dieser Ansatz erfordert den **DataContract** -Attribut in der Klasse.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Datumsangaben

Datumsangaben werden im ISO 8601-Format geschrieben. Z. B. &quot;2012-05-23T20:21:37.9116538Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Einzug

Um eingezogene XML zu schreiben, legen die **Einzug** Eigenschaft **"true"**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Festlegen von pro-Type-XML-Serialisierungsprogrammen

Sie können verschiedene XML-Serialisierer für andere CLR-Typen festlegen. Angenommen, Sie müssen möglicherweise ein bestimmtes Objekt, das erfordert **XmlSerializer** um Abwärtskompatibilität zu gewährleisten. Sie können **XmlSerializer** für dieses Objekt aus, und verwenden weiterhin **DataContractSerializer** für andere Typen.

Rufen Sie zum Festlegen einer XML-Serialisierungsprogramm für einen bestimmten Typ **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Sie können angeben, ein **XmlSerializer** oder ein beliebiges Objekt, das von abgeleitet ist **XmlObjectSerializer-Elements**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Entfernen die JSON- oder XML-Formatierungsprogramm

Sie können das JSON-Formatierungsprogramm oder das XML-Formatierungsprogramm aus der Liste der Formatierer, entfernen, wenn Sie nicht, deren Verwendung möchten. Zu diesem Zweck die wichtigsten Gründe sind:

- Um Ihre Web-API-Antworten auf einen bestimmten Medientyp zu beschränken. Sie könnten z. B. nur JSON-Antworten zu unterstützen, und entfernen das XML-Formatierungsprogramm.
- Um das Standardformatierungsprogramm durch ein benutzerdefiniertes Formatierungsprogramm zu ersetzen. Beispielsweise können Sie das JSON-Formatierungsprogramm durch Ihre eigene benutzerdefinierte Implementierung einer JSON-Formatierung ersetzen.

Der folgende Code zeigt, wie Sie die Standard-Formatierer zu entfernen. Rufen Sie diese aus Ihrem **Anwendung\_starten** Methode, die in "Global.asax" definiert.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Behandeln von zyklischen Objektverweisen

Standardmäßig schreiben die JSON- und XML-Formatierer alle Objekte als Werte. Wenn zwei Eigenschaften, die auf dasselbe Objekt verweisen oder wenn das gleiche Objekt zweimal in einer Auflistung angezeigt wird, das Formatierungsprogramm das Objekt zweimal serialisiert. Dies ist ein bestimmtes Problem enthält Ihre Objektdiagramm Zyklen, da das Serialisierungsprogramm eine Ausnahme auslöst, wenn im Diagramm eine Schleife erkannt.

Erwägen Sie die folgenden Objektmodellen und den Controller.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Aufrufen von dieser Aktion wird den Formatierer, dazu führen, dass eine Ausnahme, die in einem Status Code 500 (Interner Serverfehler) Antwort an den Client übersetzt.

Um Objektverweise im JSON-Format zu erhalten, fügen Sie den folgenden Code **Anwendung\_starten** -Methode in der Datei "Global.asax":

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Jetzt wird die Controlleraktion JSON zurück, die wie folgt aussieht:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Beachten Sie, die das Serialisierungsprogramm fügt eine &quot;$id&quot; Eigenschaft, um beide Objekte. Darüber hinaus erkannt wird, dass die Employee.Department-Eigenschaft eine Schleife erstellt, sodass den Wert durch einen Objektverweis ersetzt: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Objektverweise sind nicht in JSON-standard. Beachten Sie, ob die Clients die Ergebnisse analysieren können, werden, bevor Sie mit dieser Funktion können. Es möglicherweise besser einfach, Zyklen aus dem Diagramm zu entfernen. In diesem Beispiel ist die Verknüpfung aus der Mitarbeiter zur Abteilung z. B. wirklich nicht erforderlich.


Um Objektverweise in XML zu beizubehalten, müssen Sie zwei Optionen zur Verfügung. Die einfachere Option besteht darin hinzufügen `[DataContract(IsReference=true)]` der Model-Klasse. Die *IsReference* Parameter kann Objektverweise. Beachten Sie, dass **DataContract** Serialisierung teilnehmen, macht, sodass Sie auch hinzufügen müssen **DataMember** -Attribute auf die Eigenschaften:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Jetzt XML-Code ähnelt das Formatierungsprogramm erzeugt, die folgenden:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Wenn Sie Attribute in Ihrer Modellklasse vermeiden möchten, steht eine andere Option: Erstellen Sie eine neue typspezifische **DataContractSerializer** -Instanz, und legen Sie *PreserveObjectReferences* zu **"true"**  im Konstruktor. Legen Sie dann diese Instanz als Serialisierungsprogramm pro Typ, für die XML-medientypformatierer. Der folgende Code zeigt, wie Sie dies durchführen:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Testen die Serialisierung von Objekten

Beim Entwerfen Ihrer Web-API ist es hilfreich, testen, wie Ihre Datenobjekte serialisiert werden. Dies ist möglich, ohne einen Controller oder eine Controlleraktion aufgerufen.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
