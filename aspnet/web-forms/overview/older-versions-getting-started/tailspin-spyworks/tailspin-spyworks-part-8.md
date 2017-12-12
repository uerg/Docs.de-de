---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "Teil 8: Endgültige Seiten, Ausnahmebehandlung und Abschluss | Microsoft Docs"
author: JoeStagner
description: "Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 8 Fügt eine wenden Sie sich an-Seite zu Seite und die Ausnahme..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 0dd1717ff1051f18a78fe77402c7603008b9b486
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Teil 8: Endgültige Seiten, Ausnahmebehandlung und Abschluss
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 8 Fügt eine wenden Sie sich an-Seite zu Seite und die Ausnahmebehandlung. Dies ist das Ende der Reihe.


## <a id="_Toc260221680"></a>Wenden Sie sich an die Seite (Senden von e-Mails von ASP.NET)

Erstellen Sie eine neue Seite mit dem Namen ContactUs.aspx

Mithilfe des Designers, erstellen Sie das folgende Format, die spezielle notieren, die ToolkitScriptManager und die Editor-Steuerelement aus der AjaxdControlToolkit aufgenommen werden sollen. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Doppelklicken Sie auf die Schaltfläche "Submit", um einen Click-Ereignishandler in der CodeBehind-Datei generieren und implementieren Sie eine Methode, um die Kontaktinformationen als eine e-Mail zu senden.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Dieser Code erfordert, dass die Datei "Web.config" einen Eintrag im Abschnitt "Konfiguration" enthalten, der angibt, den SMTP-Server zum Senden von e-Mail verwendet werden soll.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>Zu den Seiten

Erstellen Sie eine Seite mit dem Namen AboutUs.aspx und fügen Sie beliebige Inhalte, die Ihnen gefällt.

## <a id="_Toc260221682"></a>Globalen Ausnahmehandlers

Schließlich in der gesamten Anwendung haben wir Ausnahmen ausgelöst, und es sind unvorhergesehenen Umstände, kalte auch Ursache, die nicht behandelte Ausnahmen in der vorliegenden Webanwendung.

Wir möchten nie eine nicht behandelte Ausnahme, die ein Besucher der Website angezeigt werden.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Nicht behandelte Ausnahmen können auch ein Sicherheitsproblem sein, abgesehen von einer schrecklichen Benutzeroberfläche wird.

Um dieses Problem zu lösen implementieren wir ein globalen ausnahmehandlers.

Zu diesem Zweck öffnen Sie die Datei "Global.asax" aus, und notieren Sie sich den folgenden vorgenerierten Ereignishandler.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Fügen Sie Code zum Implementieren der Anwendung\_Fehlerhandler wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Anschließend fügen Sie eine Seite mit dem Namen Error.aspx der Projektmappe, und fügen Sie diesen markupcodeausschnitt hinzu.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Jetzt auf der Seite\_Ereignishandler extrahieren die Fehlermeldungen aus dem Request-Objekt zu laden.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Schlussfolgerung

Wir haben gesehen, dass diese ASP.NET Web Forms erleichtert usw. eine anspruchsvolle Website mit Datenbankzugriff, AJAX-Mitgliedschaft zu erstellen. ziemlich schnell.

Hoffentlich hat in diesem Lernprogramm Ihnen die Tools gegeben, die Sie beginnen damit, eigene ASP.NET WebForms-Anwendungen erstellen müssen!

>[!div class="step-by-step"]
[Zurück](tailspin-spyworks-part-7.md)
