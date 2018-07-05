---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Teil 8: Letzte Seiten, Ausnahmebehandlung und Zusammenfassung | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 8 wird eine Kontakte-Seite zu Seite und die Ausnahme hinzugefügt...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 4fb0147a5cd8621e5341e4995960adf2f00fffc0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804716"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Teil 8: Letzte Seiten, Ausnahmebehandlung und Zusammenfassung
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 8 wird eine wenden Sie sich an-Seite zu Seite und die Ausnahmebehandlung hinzugefügt. Dies ist das Ende der Reihe.


## <a id="_Toc260221680"></a>  Wenden Sie sich an die Seite (Senden von e-Mail von ASP.NET)

Erstellen Sie eine neue Seite mit dem Namen ContactUs.aspx

Mithilfe des Designers, erstellen Sie das folgende Format, die spezielle notieren, bestehend aus dem ToolkitScriptManager und der Editor-Steuerelement aus der AjaxdControlToolkit. sein.

![](tailspin-spyworks-part-8/_static/image1.jpg)

Doppelklicken Sie auf die Schaltfläche "Absenden", um einen Click-Ereignishandler in der CodeBehind-Datei generieren, und implementieren Sie eine Methode, um die Kontaktinformationen als eine e-Mail zu senden.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Dieser Code erfordert, dass die Datei "Web.config" einen Eintrag im Abschnitt "Konfiguration" enthalten, der angibt, den SMTP-Server zum Senden von e-Mail verwendet.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  Zu den Seiten

Erstellen Sie eine Seite mit dem Namen AboutUs.aspx, und fügen Sie den gewünschten Inhalt hinzu.

## <a id="_Toc260221682"></a>  Globalen Ausnahmehandlers

Und schließlich in der gesamten Anwendung haben wir ausgelöste Ausnahmen aus, und es gibt unvorhergesehene Umstände, kalte auch Ursache, die nicht behandelte Ausnahmen in unserer Webanwendung.

Wir möchten nicht, eine nicht behandelte Ausnahme, die ein Besucher der Website angezeigt werden.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Nicht behandelte Ausnahmen können auch ein Sicherheitsproblem sein, abgesehen davon, dass eine schlechte benutzererfahrung.

Wir werden ein globalen ausnahmehandlers implementieren, um dieses Problem zu beheben.

Zu diesem Zweck öffnen Sie die Datei "Global.asax", und beachten Sie den folgenden vorab generierten Ereignishandler.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Hinzufügen von Code zum Implementieren der Anwendung\_Fehlerhandler wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Klicken Sie dann eine Seite namens Error.aspx der Projektmappe, und fügen Sie diesem Markup-Codeausschnitt hinzu.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Jetzt auf der Seite\_Event Handler extrahieren die Fehlermeldungen aus dem Request-Objekt zu laden.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Schlussbemerkung

Wir haben gesehen, dass ASP.NET WebForms erleichtert usw. mit Zugriff auf die Datenbank, Mitgliedschaft, AJAX, eine komplexe Website zu erstellen. sehr schnell.

Hoffentlich hat in diesem Tutorial Ihnen die Tools gegeben, die Sie Ihren eigenen ASP.NET WebForms-Anwendungen zu entwickeln müssen!

> [!div class="step-by-step"]
> [Vorherige](tailspin-spyworks-part-7.md)
