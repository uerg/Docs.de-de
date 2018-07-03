---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: ASP.NET Web Pages 2 Developer Preview-Infodatei | Microsoft-Dokumentation
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 914e99491908294cea1e04dd23ccdb66c1dc05a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364429"
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer Preview-Infodatei
====================
durch [Microsoft](https://github.com/microsoft)

## <a name="aspnet-web-pages-2-developer-preview-readme"></a>ASP.NET Web Pages 2 Developer Preview-Infodatei

14 September 2011

### <a name="contents"></a>Inhalt

#### <a id="_Toc303701284"></a>  Installationshinweise

Um Web Pages 2 Developer Preview installieren, haben Sie folgende Möglichkeiten:

- Installieren Sie WebMatrix 2 Beta mithilfe der [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=226883). WebMatrix ist ein Satz von kostenlose Webentwicklungstools, der ASP.NET Web Pages enthält. Weitere Informationen finden Sie im Installationsabschnitt "in [die wichtigsten Funktionen in ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

- Installieren Sie die Web Pages 2 Developer Preview, die direkt mit der [Downloadlink](https://go.microsoft.com/fwlink/?LinkID=226335). Verwenden Sie diesen Ansatz, wenn die Web Pages-Anwendungen, die mit einem Text-Editor wie Editor erstellt werden soll. Um die Web Pages 2-Anwendungen ausführen zu können, müssen Sie IIS Express 7.5 verfügen. (Dies ist automatisch in WebMatrix enthalten.) Tipps zum Testen einer mit IIS Express, Web Pages-Seite finden Sie in der Randleiste "Erstellen und Testen von ASP.NET Seiten mithilfe Ihrer eigenen Text-Editor" unter [erste Schritte mit WebMatrix und ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).

ASP.NET Web Pages 2 Developer Preview installiert werden kann, und führe die Seite-an-Seite mit ASP.NET Web Pages 1. <a id="a"></a>Weitere Informationen finden Sie im Abschnitt "Ausführen von Webseiten Anwendungen Seite-an-Seite" in [die wichtigsten Funktionen in Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701285"></a>  Dokumentation

Lernprogramme und Weitere Informationen zu ASP.NET Web Pages stehen zur Verfügung, auf die Web Pages-Seite der ASP.NET-Website ([https://www.asp.net/web-pages/](../../index.md)). Weitere Informationen zu neuen Features und Verbesserungen in Web Pages 2 finden Sie unter [die wichtigsten Funktionen in Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).

#### <a id="_Toc303701286"></a>  Unterstützung

<a id="_Toc209852135"></a><a id="_Toc255833657"></a> Dies ist eine Vorschauversion und wird offiziell nicht unterstützt. Wenn Sie Fragen zum Arbeiten mit dieser Version haben, veröffentlichen Sie sie in der ASP.NET Web Pages-Forum ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ), wobei Mitglieder der ASP.NET-Community informelle Unterstützung bieten. oftmals können sind.

#### <a id="_Toc303701287"></a>  Softwareanforderungen

ASP.NET Web Pages 2 erfordert .NET Framework 4. Es funktioniert auch mit der .NET Framework 4.5 Developer Preview-Version.

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a>Fehlerbehebungen, bekannte Probleme und aktueller Änderungen

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- **Ist\* Methoden (z. B. IsDateTime) jetzt korrekte Werte zurück für alle Kulturen.** Einige Methoden wie *IsDateTime* zuvor zurückgegebenen *"false"* Wenn diese zurückgegeben haben sollten *"true"* da zuvor kulturspezifische Überprüfungen ausgeführt wurden. Diese Methoden wurden behoben, jetzt Kultur in Betracht ziehen. Dies ist eine unterbrechende Änderung. Wenn Ihre Anwendung das alte Verhalten erforderlich ist, unterbricht.
- **Das Verhalten der Href-Methode wurde geändert.** Zuvor für die Href("~/SomeFile") aufrufen würde eine URL relativ zur derzeit ausgeführten-Datei zurück. Jetzt gibt Href("~/SomeFile") immer einen absoluten Pfad vom Stamm der Anwendung zurück. In den meisten Fällen macht dies keinen Unterschied in der zurückgegebene Wert. Diese Änderung wurde vorgenommen, um bestimmte Ajax-Szenarios zu beheben. Betrachten Sie beispielsweise den folgenden aus: 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    Dieser Code würde zuvor aufgelöst Images/Logo.jpg, was für eine Ajax-Anforderung zu dieser Seite fehlerhaft wäre. Es löst jetzt in das Stammverzeichnis des der (/ MySite/Images/Logo.jpg).
- **Die HttpContext.RedirectLocal-Methode hat sich geändert**. Diese Methode akzeptiert jetzt nur URLs, die sich auf die aktuelle Anwendung beziehen. Voll qualifiziert werden URLs abgelehnt.
- **Aufrufen die Methode ModelState.IsValid jetzt erfordert, dass Sie überprüfen, rufen zuerst**. Wenn Sie Ihre Anwendung zur Verwendung der neuen Methoden für die eingabeüberprüfung konvertieren und Aufrufen der *ModelState.IsValid* -Methode, Sie müssen nun Aufrufen *Validation.Validate* im voraus. Beispielsweise müssen Sie nun diesem Muster folgen: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  Es wird allerdings empfohlen, wenn Sie die neuen Methoden für die Validierung von Benutzereingaben verwenden nicht die verwenden *ModelState.IsValid*. Strukturieren Sie stattdessen Code wie folgt: 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- **In Internet Explorer 7 und Internet Explorer 8, die clientseitige Validierung funktioniert nicht**. Die clientseitige Validierung funktioniert nicht aufgrund von Inkompatibilitäten mit jQuery 1.6.2, die in der Standardprojektvorlage enthalten ist. (Eine serverseitige Validierung funktioniert.).

#### <a id="_Toc303701289"></a>  Haftungsausschluss

© 2011 Microsoft Corporation. Alle Rechte vorbehalten. In diesem Dokument wird bereitgestellt "als-ist." Informationen und Stellungnahmen in diesem Dokument einschließlich URLs und anderer Verweise auf Internetwebsites, können ohne vorherige Ankündigung ändern. Sie tragen das alleinige Verwendungsrisiko.
