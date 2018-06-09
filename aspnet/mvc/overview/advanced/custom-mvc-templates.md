---
uid: mvc/overview/advanced/custom-mvc-templates
title: Benutzerdefinierte MVC-Vorlage | Microsoft Docs
author: joeloff
description: Erstellen Sie eine Vorlage als VSIX-Erweiterung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034606"
---
<a name="custom-mvc-template"></a>Benutzerdefinierte MVC-Vorlage
====================
durch [; Jacques Eloff](https://github.com/joeloff)

Die Version von MVC 3 Tools Update für Visual Studio 2010 eingeführt, ein separates Projekt-Assistent für MVC-Projekte. Die Änderung wurde durch zwei Faktoren betrieben. Die Einführung von neuen Vorlagen in MVC 3 und Unterstützung für zusätzliche Ansichtsmodule z. B. Razor zu overcrowding Dialogfeld Neues Projekt in Visual Studio zunächst führen. Zweitens Kunden hatte gebeten Erweiterungspunkte, und der Assistent für neue MVC würden uns die Möglichkeit, auf diese Anforderungen reagieren leisten.

Hinzufügen von benutzerdefinierten Vorlagen, wurde ein damit den mühseligen Prozess, der basieren auf den Einsatz der Registrierung zum neue Vorlagen für die MVC-Projekt-Assistent sichtbar zu machen. Der Autor einer neuen Vorlage musste binden Sie diese in eine MSI-Datei, um sicherzustellen, dass die erforderlichen Registrierungseinträge während der Installation erstellt werden würde. Die Alternative wurde eine ZIP-Datei mit der Vorlage zur Verfügung stellen und des Endbenutzers die erforderliche Registrierungseinträge manuell erstellen.

Keiner der oben genannten Ansätze ist ideal, damit wir uns entschlossen haben Sie einige der vorhandenen Internetinfrastruktur können [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) Erweiterungen zum Autor, erleichtern verteilen und Installieren des benutzerdefinierte beginnend mit MVC 4 MVC-Vorlagen für Visual Studio 2012. Einige der Vorteile dieses Ansatzes sind:

- Eine VSIX-Erweiterung kann mehrere Vorlagen, die mehrere Sprachen (C#- und Visual Basic) zu unterstützen und mehrere Ansichtsmodule (ASPX und Razor) enthalten.
- Eine VSIX-Erweiterung kann mehrere SKUs von Visual Studio Express-SKUs einschließlich abzielen.
- Die [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) vereinfacht die Erweiterung für eine Breite Zielgruppe verteilen.
- VSIX-Erweiterungen können aktualisiert werden, erleichtert es, Korrekturen und Updates für Ihre benutzerdefinierte Vorlagen erstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Benutzer müssen beim Erstellen von Projektvorlagen, einschließlich der erforderlichen Markup für Vstemplate-Dateien usw. vertraut sein.
- Benutzer benötigen Visual Studio Professional und höher installiert ist. Express-SKUs unterstützen keine VSIX-Projekte erstellen.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installiert.

## <a name="example"></a>Beispiel

Der erste Schritt besteht darin ein neues VSIX-Projekt mit c# oder Visual Basic erstellen. Wählen Sie **Datei > Neues Projekt**, klicken Sie dann auf **Erweiterbarkeit** im linken Bereich, und wählen Sie die **VSIX-Projekt**.

![Neues Projekt](custom-mvc-templates/_static/image1.jpg)

Nachdem das Projekt erstellt wurde, wird das VSIX-Designer geöffnet.

![Projekt-Designer-Metadaten](custom-mvc-templates/_static/image2.jpg)

Der Designer kann verwendet werden, um einige der allgemeinen Eigenschaften der Erweiterung zu bearbeiten, die Benutzern angezeigt wird, wenn sie die Erweiterung installieren, oder durchsuchen die installierten Erweiterungen in Visual Studio (**Tools > Erweiterungen und Updates**). Nach Abschluss der klicken Sie die allgemeine Informationen auf der **Ziele installieren Registerkarte**.

![Projekt-Designer-Installation auf](custom-mvc-templates/_static/image3.jpg)

Diese Registerkarte dient zum Angeben der SKUs und Versionen von Visual Studio, die von der Erweiterung unterstützt werden. Aktivieren Sie das Kontrollkästchen **diese VSIX ist installiert für alle Benutzer** pro Computer installiert des VSIX-Projekt zu aktivieren. Klicken Sie auf die **neu** Schaltfläche auf der rechten Seite, um zusätzliche SKUs z. B. Web Developer Express (VWD) hinzuzufügen.

![Hinzufügen von neuen Installationsziel](custom-mvc-templates/_static/image4.jpg)

Wenn Sie beabsichtigen, unterstützen alle Professional und höhere SKUs (Professional, Premium und Ultimate) müssen nur die minimale SKU wählen Sie in der Familie **Microsoft.VisualStudio.Pro**. Denken Sie daran, alle Ihre Änderungen zu speichern, nachdem Sie die Ziele für die Installation abgeschlossen haben.

![Projekt-Designer-Installation auf](custom-mvc-templates/_static/image5.jpg)

Die **Bestand** Registerkarte wird verwendet, um alle Inhaltsdateien VSIX-Projekt hinzufügen. Da MVC benutzerdefinierten Metadaten erfordert Sie bearbeiten die unformatierten XML-Codes der VSIX-manifest-Datei anstelle der **Bestand** Tab, um Inhalt hinzuzufügen. Starten Sie, indem Sie das VSIX-Projekt den Vorlageninhalt hinzugefügt. Es ist wichtig, dass die Struktur des Ordners und der Inhalt des Layouts des Projekts widerspiegeln. Das folgende Beispiel enthält vier Projektvorlagen, die die grundlegende MVC-Projektvorlage abgeleitet wurden. Stellen Sie sicher, dass alle Dateien, die die Projektvorlage (alles unterhalb des Ordners ProjectTemplates) bilden hinzugefügt werden die **Content** Itemgroup in VSIX project-Datei und jedes Element enthält die  **CopyToOutputDirectory** und **IncludeInVsix** Metadaten festgelegt werden, wie im folgenden Beispiel gezeigt.

&lt;Inhalt enthalten =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;"true"&lt;/IncludeInVSIX&gt;

&lt;/ Content&gt;

Wenn dies nicht der Fall ist, wird die IDE versucht, kompilieren den Inhalt der Vorlage aus, wenn Sie die VSIX-Pakete erstellen und ein Fehler wird wahrscheinlich angezeigt. Codedateien in Vorlagen enthalten häufig spezielle [Vorlagenparameter](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) von Visual Studio verwendet werden, wenn die Projektvorlage instanziiert wird und kann daher nicht in der IDE kompiliert werden.

![Projektmappen-Explorer](custom-mvc-templates/_static/image6.jpg)

Schließen Sie die VSIX-Designer, und klicken Sie mit der rechten Maustaste auf die **"Source.Extension.vsixmanifest"** Datei **Projektmappen-Explorer** , und wählen Sie **Öffnen mit** , und wählen Sie die **XML ( Texteditor)** Option.

![Mit dem Dialogfeld Öffnen](custom-mvc-templates/_static/image7.jpg)

Erstellen einer **&lt;Bestand&gt;** Element und Hinzufügen einer **&lt;Asset&gt;** -Element für jede Datei, die in VSIX-Projekt enthalten sein muss. Die **Typ** -Attribut der einzelnen **&lt;Asset&gt;** Element muss festgelegt werden, um **Microsoft.VisualStudio.Mvc.Template**. Dies ist ein benutzerdefinierter Namespace, den nur in der MVC-Projekt-Assistent versteht. Finden Sie in der VSIX-Schema für 2.0-Dokumentation für Weitere Informationen über die Struktur und das Layout der Manifestdatei.

Nur die Dateien der VSIX hinzufügen reicht nicht aus, um die Vorlagen mit dem Assistenten für MVC zu registrieren. Sie müssen Informationen wie Namen, Beschreibung, unterstützten Ansichtsmodule und Programmiersprache für die MVC-Assistenten bereitstellen. Diese Informationen in benutzerdefinierten Attributen zugeordneten durchgeführt wird die **&lt;Asset&gt;** -Element für jede **Vstemplate** Datei.

&lt;Asset-D:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

D:Source =&quot;Datei&quot;

Path =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language =&quot;c#&quot;

Viewengine mit =&quot;Aspx&quot;

TemplateId =&quot;MyMvcApplication&quot;

Titel =&quot;benutzerdefinierte grundlegende Webanwendung&quot;

Description =&quot;eine benutzerdefinierte Vorlage abgeleitet wurde eine grundlegende MVC-Webanwendung (Razor)&quot;

Version=&quot;4.0&quot;/&gt;

Es folgt eine Erklärung der benutzerdefinierten Attribute, die vorhanden sein müssen:

- **ProjectType** muss auf MVC festgelegt werden.
- **Sprache** kennzeichnet die Entwicklungssprache von Vorlage unterstützt werden. Gültige Werte sind entweder c# oder VB dar.
- **Viewengine mit** kennzeichnet das Ansichtsmodul, die von der Vorlage wie z. B. aspx- oder Razor unterstützt. Sie können einen benutzerdefinierten Wert für dieses Feld angeben.
- **TemplateId** dient zum Gruppieren von Vorlagen. Wenn der Wert entspricht einem vorhandenen Vorlagen-ID, die er überschreiben, Vorlagen, die zuvor mit dem Assistenten für MVC registriert.
- **Titel** kennzeichnet die kurze Beschreibung im MVC-Assistenten unter jede Projektvorlage angezeigt.
- **Beschreibung** kennzeichnet eine detailliertere Beschreibung der Vorlage.

Nachdem Sie alle Dateien, der dem Manifest hinzugefügt haben und die Datei gespeichert ist, Sie, dass feststellen werden die **Bestand** -Registerkarte im Designer werden alle Dateien angezeigt, jedoch nicht den benutzerdefinierten Attributen Sie hinzugefügt, die **&lt;Asset&gt;** Elemente für die **Vstemplate** Dateien.

![Projekt-Designer-Objekte](custom-mvc-templates/_static/image8.jpg)

Alle jetzt bleibt das VSIX-Projekt zu kompilieren und installieren es.

Stellen Sie sicher, dass alle Instanzen von Visual Studio geschlossen sind, auf dem Computer, auf dem Sie die VSIX-Erweiterung zu testen möchten. Visual Studio sucht nach neuen Erweiterungen während des Starts, daher, wenn die IDE geöffnet ist, während der Installation von einer VSIX Sie Visual Studio neu starten müssen. Im Explorer, klicken Sie mit der Doppelklicken auf die VSIX-Datei zum Starten der **VSIX-Installationsprogramm**, klicken Sie auf **installieren** , und starten Sie Visual Studio.

![VSIX-Installationsprogramm](custom-mvc-templates/_static/image9.jpg)

Wählen Sie in der Menüleiste **Tools > Erweiterungen und Updates** zu bestätigen, dass die Erweiterung installiert wurde. Das VSIX-Installationsprogramm Fehler während der Installation der Erweiterung gemeldet können Sie das VSIX-Installationsprogramm-Protokoll für Weitere Informationen anzeigen. Das Protokoll wird in der Regel erstellt, der **"% Temp%"** Ordner des Benutzers, der die Erweiterung, z. B. installiert **C:\Users\Bob\AppData\Local\Temp**.

![Erweiterungen und Updates](custom-mvc-templates/_static/image10.jpg)

Nach dem Schließen des Fensters können Sie erstellen eine MVC 4-Projekt, um festzustellen, ob die neuen Vorlagen im MVC-Assistenten angezeigt werden.

![Neues ASP.NET MVC 4-Projekt](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Einschränkungen

1. Das MVC-Assistent unterstützt keine lokalisierte benutzerdefinierte Vorlagen.
2. Der Assistent wird keine Fehler gemeldet, wenn es keine benutzerdefinierte Vorlagen findet. Wenn die erforderlichen benutzerdefinierten Attribute vorhanden sind, würde die Vorlage einfach aus dem Assistenten ausgeschlossen werden.
