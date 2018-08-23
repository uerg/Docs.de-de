---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Lebenszyklus einer ASP.NET MVC 5-Anwendung | Microsoft-Dokumentation
author: cephalin
description: Laden Sie ein PDF-Dokument, das den Lebenszyklus einer ASP.NET MVC 5-Anwendung Diagramme herunter. In diesem Dokument Lifecycle bietet einen allgemeinen Überblick über den MVC-Lebenszyklus einer...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: 5523217ae5674ac4e5898a150f9e5f0ae3a70d26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835193"
---
<a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Lebenszyklus einer ASP.NET MVC 5-Anwendung
====================
durch [Cephas Lin](https://github.com/cephalin)

[Herunterladen der PDF-Dokument](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

Hier können Sie eine PDF-Dokument herunterladen, die Diagramme, die der Lebenszyklus jeder ASP.NET MVC 5-Anwendung, empfängt die HTTP-Anforderung zum Senden der HTTP-Antwort an den Client zurück. Es dient sowohl als Bildungseinrichtungen Tool für diejenigen, die mit ASP.NET MVC vertraut sind, als auch als Referenz für diejenigen, die einen Drilldown in bestimmte Aspekte der Anwendung durchführen möchten. Das PDF-Dokument hat die folgenden Funktionen:

- Relevante [HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) Phasen, um besser zu verstehen, in denen MVC in integriert die [ASP.NET Anwendungslebenszyklus](https://msdn.microsoft.com/library/bb470252.aspx).
- Einen allgemeinen Überblick des Lebenszyklus der MVC-Anwendung, das, in dem Sie die wichtigsten Phasen verstehen können, die jeder MVC-Anwendung in der Anforderungsverarbeitungspipeline durchlaufen.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Eine Detailansicht, die einen Drilldown nach unten mit den Einzelheiten der Anforderungsverarbeitungspipeline anzeigt. Sie können vergleichen, die Zusammenfassung und Detail-Ansicht, um festzustellen, wie die Lebenszyklen Details in die verschiedenen Phasen gesammelt werden. [PDF herunterladen](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) um eine größere Ansicht anzuzeigen.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Platzierung und Zweck der allen überschreibbaren Methoden auf die [Controller](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) Objekt in der Pipeline zur anforderungsverarbeitung. Sie können nicht besitzen oder die Notwendigkeit, die alle eine Methode zu überschreiben, aber es ist wichtig, ihre Rolle im Lebenszyklus Anwendung nachvollziehen können, damit Sie in der entsprechenden Life Cycle, Lebenszyklus-Phase für den Effekt Code schreiben können, die Sie möchten.
- Einfach unglaublich von Diagrammen, die zeigt, wie die Filter-Typen (Authentifizierung, Autorisierung, Aktion und Ergebnis) aufgerufen wird.
- Verknüpfung zu einer nützlichen Artikel oder Blogs jeder Punkt in der Detailansicht von Interesse.


## <a name="next-steps"></a>Nächste Schritte

Entspricht in diesem Dokument müssen? Wir würden uns freuen, Ihr Feedback. Wenn Sie eine Frage auf der ASP.NET MVC-Lebenszyklus in Ihrer Anwendung haben [Stackoverflow](http://stackoverflow.com/help) und [ASP.NET MVC-Foren](https://forums.asp.net/1146.aspx) stellen einen hervorragenden Ort zu Fragen. Führen Sie [mich](https://twitter.com/Cephas_MSFT) auf Twitter, sodass Sie Updates auf meinem neuesten Lernprogramme zugreifen können.
