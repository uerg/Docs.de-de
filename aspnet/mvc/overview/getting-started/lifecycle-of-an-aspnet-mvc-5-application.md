---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Lebenszyklus einer ASP.NET MVC 5-Anwendung | Microsoft Docs
author: cephalin
description: "Laden Sie ein PDF-Dokument, das den Lebenszyklus einer ASP.NET MVC 5-Anwendung Diagramme. Dieses Lebenszyklus-Dokument enthält eine allgemeine Ansicht des MVC-Lebenszyklus einer..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/28/2014
ms.topic: article
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5692c43168eb261c91f40e2046897a1e5d31a028
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Lebenszyklus einer ASP.NET MVC 5-Anwendung
====================
durch [Cephas Lin](https://github.com/cephalin)

[Herunterladen von PDF-Dokument](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Hier können Sie ein PDF-Dokument herunterladen, die Diagramme, die der Lebenszyklus von jeder ASP.NET MVC 5-Anwendung empfängt die HTTP-Anforderung an die HTTP-Antwort senden an den Client zurück. Es dient sowohl als Bildungseinrichtung Tool für diejenigen, die in ASP.NET MVC vertraut sind, als auch als Referenz für die Benutzer einen Drilldown in bestimmte Aspekte der Anwendung ausführen müssen. Das PDF-Dokument hat die folgenden Funktionen:

- Relevante ["HttpApplication"](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx) Stufen, um besser zu verstehen, in denen mit MVC integriert ist die [ASP.NET Anwendungslebenszyklus](https://msdn.microsoft.com/en-us/library/bb470252.aspx).
- Einen allgemeinen Überblick des Lebenszyklus der MVC-Anwendung, das, in dem Sie die wichtigen Phasen verstehen können, die jeder MVC-Anwendung in der Anforderung Verarbeitungspipeline durchlaufen.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Eine Detailansicht, die einen Drilldown nach unten in die Details der Anforderung Verarbeitungspipeline anzeigt. Sie können allgemeine Ansicht und der Detailansicht, um festzustellen, wie die Lebenszyklen Details gesammelt werden, in den verschiedenen Phasen vergleichen. [PDF herunterladen](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) um eine größere Ansicht anzuzeigen.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Platzierung und Zweck der allen überschreibbaren Methoden auf die [Controller](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.aspx) Objekt in die Pipeline zur anforderungsverarbeitung. Kann oder möglicherweise nicht erforderlich, alle eine Methode zu überschreiben, aber es ist wichtig, dass Sie ihre Rolle im Lebenszyklus Anwendung verstehen, damit Sie in der entsprechenden Lebenszyklusphase Effekt Code schreiben können, die Sie möchten.
- Vergrößert-nach-oben-Diagramme anzeigen, wie alle Filtertypen (Authentifizierung, Autorisierung, Aktion und Ergebnis) aufgerufen wird.
- Link zu einer nützlichen Artikel oder Blog von jeder Punkt in der Detailansicht von Interesse.


## <a name="next-steps"></a>Nächste Schritte

Kennengelernt dieses Dokument Ihren Bedürfnissen? Wir würden Ihr Feedback freuen. Wenn Sie Frage des ASP.NET MVC-Lebenszyklus in Ihrer Anwendung haben [Stackoverflow](http://stackoverflow.com/help) und [ASP.NET MVC-Foren](https://forums.asp.net/1146.aspx) eignen sich hervorragend, um Unterstützung bitten. Führen Sie die [me](https://twitter.com/Cephas_MSFT) auf Twitter, sodass Sie Updates auf meine aktuelle Lernprogramme abrufen können.
