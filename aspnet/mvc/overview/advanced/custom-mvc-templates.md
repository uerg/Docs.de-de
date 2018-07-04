---
uid: mvc/overview/advanced/custom-mvc-templates
title: Benutzerdefinierte MVC-Vorlage | Microsoft-Dokumentation
author: joeloff
description: Erstellen Sie eine Vorlage als VSIX-Erweiterung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 715990e2d7151ad1ce8288169ef4bdd5c4243369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367243"
---
<a name="custom-mvc-template"></a>Benutzerdefinierte MVC-Vorlage
====================
durch [; Jacques Eloff](https://github.com/joeloff)

Die Version von MVC 3 Tools Update für Visual Studio 2010 eingeführt, ein separates Projekt-Assistent für MVC-Projekte. Die Änderung wurde durch zwei Faktoren gesteuert. Die Einführung der neuen Vorlagen in MVC 3 und Unterstützung für zusätzliche Ansichts-Engines wie z. B. Razor zu overcrowding das Dialogfeld "Neues Projekt" in Visual Studio zunächst führen. Zweitens Kunden für Erweiterungspunkte gebeten hatten, und der Assistent für neue MVC würde es sich leisten, uns die Möglichkeit, die auf diese Anforderungen zu reagieren.

Hinzufügen von benutzerdefinierten Vorlagen wurde ein damit den mühseligen Prozess, der benötigt werden, mit der Registrierung auf um neue Vorlagen für die MVC-Projekt-Assistenten sichtbar zu machen. Wrappen Sie es in eine MSI-Datei, um sicherzustellen, dass bei der Installation der erforderlichen Registrierungseinträge erstellt werden würde, ist der Autor einer neuen Vorlage musste. Die Alternative war, stellen eine ZIP-Datei, die mit der Vorlage zur Verfügung, und den Endbenutzer die erforderlichen Registrierungseinträge manuell erstellen müssen.

Keiner der oben genannten Ansätze eignet sich ideal, damit wir beschlossen, einige der vorhandenen Internetinfrastruktur nutzen [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) -Erweiterungen für den Autor, erleichtern verteilen, und installieren Sie benutzerdefinierte MVC-Vorlagen, die beginnend mit MVC 4 für Visual Studio 2012. Einige der Vorteile, die diesen Ansatz sind:

- Eine VSIX-Erweiterung kann mehrere Vorlagen, die verschiedene Sprachen (C#- und Visual Basic) zu unterstützen und mehrere Ansichts-Engines ("ASPX" und "Razor") enthalten.
- Eine VSIX-Erweiterung kann mehrere SKUs von Visual Studio, einschließlich Express-SKUs abzielen.
- Die [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) erleichtert die Verteilung der Erweiterungs für eine große Zielgruppe.
- VSIX-Erweiterungen können aktualisiert werden, erleichtert Ihnen die Korrekturen und Updates für benutzerdefinierten Vorlagen erstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Benutzer müssen mit der Erstellung von Projektvorlagen, einschließlich der Vstemplate-Dateien, z.B. für das erforderliche Markup vertraut sein.
- Benutzer müssen Visual Studio Professional und höher installiert ist. Express-SKUs unterstützen keine VSIX-Projekte erstellen.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installiert.

## <a name="example"></a>Beispiel

Der erste Schritt ist die Erstellung ein neues VSIX-Projekts mit c# oder Visual Basic. Wählen Sie **Datei > Neues Projekt**, klicken Sie dann auf **Erweiterbarkeit** im linken Bereich, und wählen Sie die **VSIX-Projekt**.

![Neues Projekt](custom-mvc-templates/_static/image1.jpg)

Nachdem das Projekt erstellt wurde, wird der VSIX-Designer geöffnet.

![Projekt-Designer-Metadaten](custom-mvc-templates/_static/image2.jpg)

Der Designer kann verwendet werden, um einige der allgemeinen Eigenschaften der Erweiterung zu bearbeiten, die Benutzern angezeigt wird, wenn sie die Erweiterung installieren, oder durchsuchen die installierten Erweiterungen in Visual Studio (**Tools > Erweiterungen und Updates**). Sie haben die allgemeine Informationen klicken Sie auf die **Registerkarte "Ziele installieren"**.

![Projekt-Designer installieren](custom-mvc-templates/_static/image3.jpg)

Diese Registerkarte wird verwendet, um anzugeben, die SKUs und Versionen von Visual Studio, die durch die Erweiterung unterstützt werden. Aktivieren Sie das Kontrollkästchen **diese VSIX ist installiert für alle Benutzer** pro Computer installiert, der die VSIX-Datei zu aktivieren. Klicken Sie auf die **neu** Schaltfläche auf der rechten Seite, um zusätzliche SKUs wie z. B. Web Developer Express (VWD) hinzuzufügen.

![Neue Installationsziel hinzufügen](custom-mvc-templates/_static/image4.jpg)

Wenn Sie beabsichtigen, unterstützen alle Professional und höheren SKUs (Professional, Premium und Ultimate) müssen nur die Mindest-SKU wählen Sie in der Familie, **"Microsoft.VisualStudio.pro"**. Denken Sie daran, alle Ihre Änderungen zu speichern, nachdem Sie die Ziele installieren haben.

![Projekt-Designer installieren](custom-mvc-templates/_static/image5.jpg)

Die **Assets** Registerkarte wird verwendet, um alle Inhaltsdateien VSIX-Projekt hinzufügen. Da MVC benutzerdefinierten Metadaten erforderlich ist. Sie bearbeiten das unformatierte XML der VSIX-manifest-Datei anstelle der **Assets** Tab, um Inhalt hinzuzufügen. Starten Sie, indem Sie das VSIX-Projekt den Inhalt der Vorlage hinzugefügt. Es ist wichtig, dass die Struktur des Ordners und der Inhalt das Layout des Projekts widerspiegeln. Das folgende Beispiel enthält vier Projektvorlagen, die von der einfachen MVC-Projektvorlage abgeleitet wurden. Stellen Sie sicher, dass alle Dateien, die Ihre Projektvorlage (alles unterhalb des Ordners ProjectTemplates) umfassen hinzugefügt werden die **Content** Itemgroup in VSIX project-Datei und jedes Element enthält die  **CopyToOutputDirectory** und **IncludeInVsix** -Metadaten festzulegen, wie im folgenden Beispiel gezeigt.

&lt;Den Inhalt einschließen =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;"true"&lt;/IncludeInVSIX&gt;

&lt;/ Inhalte&gt;

Wenn dies nicht der Fall ist, wird die IDE versucht, kompilieren den Inhalt der Vorlage aus, wenn Sie die VSIX-Datei erstellen, und Sie wahrscheinlich eine Fehlermeldung erhalten. Codedateien in Vorlagen enthalten häufig spezielle [Vorlagenparameter](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) von Visual Studio verwendet werden, wenn die Projektvorlage instanziiert wird und aus diesem Grund nicht werden, in der IDE kompiliert kann.

![Projektmappen-Explorer](custom-mvc-templates/_static/image6.jpg)

Schließen Sie die VSIX-Designer, und klicken Sie mit der rechten Maustaste auf die **"Source.Extension.vsixmanifest"** Datei **Projektmappen-Explorer** , und wählen Sie **Öffnen mit** , und wählen Sie die **XML ( Text) Editor** Option.

![Mit dem Dialogfeld Öffnen](custom-mvc-templates/_static/image7.jpg)

Erstellen einer **&lt;Assets&gt;** Element und Hinzufügen einer **&lt;Asset&gt;** -Element für jede Datei, die in VSIX-Pakete enthalten sein muss. Die **Typ** -Attribut der einzelnen **&lt;Asset&gt;** Element muss festgelegt werden, um **Microsoft.VisualStudio.Mvc.Template**. Dies ist ein benutzerdefinierter Namespace, den nur in der MVC-Projekt-Assistent versteht. Lesen Sie die VSIX-Schema "2.0" Dokumentation für Weitere Informationen zu die Struktur und das Layout der Manifestdatei.

Die Dateien nur die VSIX-Datei hinzufügen reicht nicht aus, um die Vorlagen mit dem MVC-Assistenten registrieren. Sie müssen Informationen wie z. B. Namen, Beschreibung, unterstützten Ansichts-Engines und Programmiersprache zum MVC-Assistenten angeben. Diese Informationen in benutzerdefinierten Attributen, die zugeordneten erfolgt die **&lt;Asset&gt;** -Element für jede **Vstemplate** Datei.

&lt;Asset-D:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

D:Source =&quot;Datei&quot;

Path =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language =&quot;c#&quot;

Viewengine mit =&quot;Aspx&quot;

TemplateId =&quot;MyMvcApplication&quot;

Title =&quot;benutzerdefinierten einfachen Webanwendung&quot;

Beschreibung =&quot;eine benutzerdefinierte Vorlage abgeleitet wird, aus einer einfachen MVC-Webanwendung (Razor)&quot;

Version=&quot;4.0&quot;/&gt;

Im folgenden finden Sie eine Erläuterung der benutzerdefinierten Attribute, die vorhanden sein müssen:

- **ProjectType** muss auf MVC festgelegt werden.
- **Sprache** kennzeichnet die Entwicklungssprache unterstützt, die von der Vorlage. Gültige Werte sind entweder c# oder VB.
- **Viewengine mit** kennzeichnet die Ansichts-Engine, die von der Vorlage wie z. B. aspx- oder Razor unterstützt. Sie können einen benutzerdefinierten Wert für dieses Feld angeben.
- **TemplateId** dient zum Gruppieren von Vorlagen. Wenn der Wert entspricht einer vorhandenen Vorlagen-ID, die Sie überschreiben, Vorlagen, die mit dem MVC-Assistenten bereits registriert.
- **Titel** kennzeichnet die kurze Beschreibung, die im MVC-Assistenten unter jede Projektvorlage angezeigt.
- **Beschreibung** kennzeichnet eine ausführlichere Beschreibung der Vorlage.

Nachdem Sie alle Dateien, der dem Manifest hinzugefügt haben, und sie gespeichert, Sie, dass feststellen werden die **Assets** -Registerkarte im Designer werden alle Dateien angezeigt, aber nicht die benutzerdefinierte Attribute Sie hinzugefügt haben, um die **&lt;Asset&gt;** Elemente für die **Vstemplate** Dateien.

![Projekt-Designer-Ressourcen](custom-mvc-templates/_static/image8.jpg)

Alle diese bleibt jetzt ist das VSIX-Projekt zu kompilieren und zu installieren.

Stellen Sie sicher, dass alle Instanzen von Visual Studio geschlossen sind, auf dem Computer, in dem Sie die VSIX-Erweiterung zu testen möchten. Visual Studio sucht nach neuen Erweiterungen während des Starts, wenn es sich bei die IDE geöffnet ist, während der Installation von einer VSIX-Datei müssen Sie Visual Studio neu starten. Im Explorer einen Doppelklick auf die VSIX-Datei zum Starten der **VSIX-Installationsprogramm**, klicken Sie auf **installieren** , und starten Sie Visual Studio.

![VSIX-Installationsprogramm](custom-mvc-templates/_static/image9.jpg)

Wählen Sie im Menü **Tools > Erweiterungen und Updates** zu bestätigen, dass die Erweiterung installiert wurde. Wenn das VSIX-Installationsprogramm Fehler während der Installation der Erweiterung gemeldet, können Sie das VSIX-Installer-Protokoll für Weitere Informationen anzeigen. Das Protokoll wird in der Regel erstellt, der **%TEMP%** Ordner des Benutzers, der die Erweiterung, z. B. installiert **C:\Users\Bob\AppData\Local\Temp**.

![Erweiterungen und Updates](custom-mvc-templates/_static/image10.jpg)

Nach dem Schließen des Fensters können Sie erstellen eine MVC 4-Projekt, um festzustellen, ob die neuen Vorlagen im MVC-Assistenten angezeigt werden.

![Neues ASP.NET MVC 4-Projekt](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Einschränkungen

1. Der MVC-Assistent unterstützt keine lokalisierte benutzerdefinierte Vorlagen.
2. Der Assistent wird keine Fehler gemeldet, wenn mit benutzerdefinierte Vorlagen zu suchen. Wenn die erforderlichen benutzerdefinierten Attribute vorhanden sind, würde die Vorlage einfach über den Assistenten ausgeschlossen werden.
