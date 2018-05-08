---
title: Image-Taghilfsprogramm in ASP.NET Core
author: pkellner
description: Veranschaulicht die Arbeit mit dem Image-Taghilfsprogramm
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="image-tag-helper-in-aspnet-core"></a>Image-Taghilfsprogramm in ASP.NET Core

Von [Peter Kellner](http://peterkellner.net) 

Das Image-Taghilfsprogramm verbessert den Tag `img` (`<img>`). Es benötigt den Tag `src` und das `boolean`-Attribut `asp-append-version`.

Wenn die Bildquelle (`src`) eine statische Datei auf dem Hostwebserver ist, wird der Bildquelle eine eindeutige Cache-Busting-Zeichenfolge als Abfrageparameter angefügt. Dadurch wird eine eindeutige Anforderungs-URL erstellt, die den aktualisierten Anforderungsparameter enthält, wenn die Datei auf dem Hostwebserver geändert wird. Die Cache-Busting-Zeichenfolge ist ein eindeutiger Wert, der den Hash der statischen Bilddatei darstellt.

Wenn die Bildquelle (`src`) keine statische Datei ist (z.B. eine Remote-URL oder die Datei ist auf dem Server nicht vorhanden), wird das Attribut `src` des Tags `<img>` ohne einen Cache-Busting-Abfragezeichenfolgenparameter erstellt.

## <a name="image-tag-helper-attributes"></a>Attribute von Image-Taghilfsprogrammen


### <a name="asp-append-version"></a>asp-append-version

Wenn das Attribut `src` zusätzlich angegeben wird, wird das Image-Taghilfsprogramm aufgerufen.

Das folgende Beispiel zeigt ein gültiges `img`-Taghilfsprogramm:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Wenn die statische Datei im Verzeichnis *..wwwroot/images/asplogo.png* vorhanden ist, ähnelt der erstellte HTML-Code folgendem (der Hash wird abweichen):

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

Der Wert, der dem Parameter `v` zugewiesen ist, ist der Hashwert der Datei auf dem Datenträger. Wenn der Webserver den Lesezugriff auf die statische Datei, auf die verwiesen wird, nicht erhalten kann, werden keine `v`-Parameter dem Attribut `src` zugefügt.

- - -

### <a name="src"></a>src

Das src-Attribut wird auf dem Element `<img>` benötigt, um das Image-Taghilfsprogramm zu aktivieren. 

> [!NOTE]
> Das Image-Taghilfsprogramm verwendet den `Cache`-Anbieter auf dem lokalen Webserver zum Speichern der berechneten `Sha512` einer angegebenen Datei. Wenn die Datei erneut angefordert wird, muss `Sha512` nicht neu berechnet werden. Der Cache wird durch einen Datei-Watcher ungültig gemacht, der an die Datei angefügt wird, wenn `Sha512` der Datei berechnet wird.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:performance/caching/memory>
