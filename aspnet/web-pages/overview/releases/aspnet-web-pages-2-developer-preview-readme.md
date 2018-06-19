---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: ASP.NET Web Pages 2 Developer Preview Infodatei | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 1a43b2b12af9cd223d8a3622239743f7c431f617
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899089"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer Preview-Infodatei
====================
durch [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer Preview-Infodatei

14 September 2011

### <a name="contents"></a>Inhalt

#### <a id="_Toc303701284"></a>  Hinweise zur installationsnachbereitung

Um die Web Pages 2 Developer Preview installieren, müssen Sie diese Optionen:

- Installieren mithilfe von WebMatrix 2 Beta die [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix ist ein Satz von freien Webentwicklungstools, der ASP.NET Web Pages enthält. Weitere Informationen finden Sie im Abschnitt "Installation" in [der Top-Funktionen in ASP.NET Web Pages 2-Entwicklervorschau](https://go.microsoft.com/fwlink/?LinkID=227824).

- Installieren Sie Web Pages 2-Entwicklervorschau direkt mit der [download-Link](https://go.microsoft.com/fwlink/?LinkID=226335). Verwenden Sie diesen Ansatz, wenn die Web Pages-Anwendungen, die mit einem Text-Editor wie Editor erstellt werden soll. Um die Web Pages 2-Anwendungen ausführen zu können, müssen Sie IIS Express 7.5 verfügen. (Dies ist automatisch in WebMatrix enthalten.) Tipps zum Testen einer Web Pages-Seite mit IIS Express finden Sie in der Randleiste "Erstellen und Testen von ASP.NET Seiten mithilfe Ihrer eigenen Text-Editor" unter [erste Schritte mit WebMatrix und ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web Pages 2 Developer Preview installiert werden kann und Seite-an-Seite ausführen können mit ASP.NET Web Pages-1. <a id="a"></a>Weitere Informationen finden Sie im Abschnitt "Ausführen Webseiten Anwendungen Seite-an-Seite" in [der Top-Funktionen in Web Pages 2-Entwicklervorschau](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET Web Pages stehen für die Web Pages-Seite der ASP.NET-Website ([https://www.asp.net/web-pages/](../../index.md)). Informationen zu neuen Features und Verbesserungen in Web Pages 2 finden Sie unter [der Top-Funktionen in Web Pages 2-Entwicklervorschau](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Unterstützung

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Dies wird als Vorschauversion verfügbar und wird offiziell nicht unterstützt. Falls Sie Fragen zum Arbeiten mit dieser Version haben, stellen Sie diese in der ASP.NET Web Pages-Forum ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), in denen Mitglieder der ASP.NET-Community informelle Unterstützung leisten häufig können sind.

#### <a id="_Toc303701287"></a>  Softwareanforderungen

ASP.NET Web Pages 2 erfordert .NET Framework 4. Es funktioniert auch mit der .NET Framework 4.5 Developer Preview-Version.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Fehlerbehebungen, bekannte Probleme und aktueller Änderungen

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Ist\* Methoden (z. B. IsDateTime) jetzt richtig Rückgabewerte für alle Kulturen.** Einige Methoden wie *IsDateTime* zuvor zurückgegeben *"false"* Wenn sie zurückgegeben haben sollten *"true"* da zuvor kulturspezifische Überprüfungen ausgeführt wurden. Diese Methoden wurden behoben, um jetzt Kultur zu berücksichtigen. Dies ist eine unterbrechende Änderung. Wenn Ihre Anwendung das alte Verhalten erforderlich ist, wird er unterbrochen.
- **Das Verhalten der Href-Methode hat sich geändert.** Zuvor für die Href("~/SomeFile") aufrufen würde eine URL relativ zur derzeit ausgeführten Datei zurück. Jetzt gibt Href("~/SomeFile") immer einen absoluten Pfad vom Stamm der Anwendung zurück. Dieses Verhalten wird keinen Unterschied in der Rückgabewert stellen Sie bei den meisten Fällen. Diese Änderung wurde vorgenommen, um bestimmte Ajax-Szenarios zu beheben. Betrachten Sie beispielsweise den folgenden Beispielcode: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Dieser Code würde zuvor aufgelöst Images/Logo.jpg, die für eine Ajax-Anforderung zu dieser Seite unzulässig sein würde. Löst jetzt auf den Stamm der (/ MySite/Images/Logo.jpg).
- **Die HttpContext.RedirectLocal-Methode hat sich geändert**. Diese Methode akzeptiert jetzt nur mit URLs, die relativ zur aktuellen Anwendung sind. Voll qualifiziert, werden URLs abgelehnt werden.
- **Die Methode ModelState.IsValid jetzt erfordert, dass Sie rufen Sie zuerst überprüfen**. Wenn Sie die Anwendung verwendet die neuen Methoden für die Validierung von Benutzereingaben konvertieren und Aufrufen der *ModelState.IsValid* -Methode, müssen Sie jetzt Aufrufen *Validation.Validate* im voraus. Beispielsweise müssen Sie jetzt diesem Muster folgen: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Wir empfehlen jedoch, wenn Sie die neuen Methoden für die Validierung von Benutzereingaben verwenden keine verwenden *ModelState.IsValid*. Strukturieren Sie stattdessen Code wie folgt aus: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **Internet Explorer 7 und Internet Explorer 8, die clientseitige Validierung funktioniert nicht**. Die clientseitige Validierung funktioniert nicht aufgrund von Inkompatibilitäten mit jQuery 1.6.2, die mit der Standardprojektvorlage enthalten ist. (Eine serverseitige Validierung funktioniert.).

#### <a id="_Toc303701289"></a>  Haftungsausschluss

© 2011 Microsoft Corporation. Alle Rechte vorbehalten. Dieses Dokument wird bereitgestellt "als-ist." Informationen und Ansichten, ausgedrückt in diesem Dokument, einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung geändert werden. Sie tragen das alleinige Verwendungsrisiko.
