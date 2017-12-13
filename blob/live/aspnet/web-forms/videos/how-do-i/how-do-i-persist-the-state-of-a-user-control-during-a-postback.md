---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: "[Wie kann ich]: den Zustand eines Benutzersteuerelements beibehalten, während ein Postback | Microsoft Docs"
author: rick-anderson
description: In diesem video Chris Pels wird gezeigt, wie der Status eines oder mehrere Objekte in einem Benutzersteuerelement beibehalten werden. Zuerst wird ein Benutzersteuerelement erstellt, das die Abilit darstellt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a><span data-ttu-id="de83b-104">[Wie kann ich]: den Zustand eines Benutzersteuerelements beibehalten, während ein Postback</span><span class="sxs-lookup"><span data-stu-id="de83b-104">[How Do I]: Persist the State of a User Control During a Postback</span></span>
====================
<span data-ttu-id="de83b-105">durch [Chris PEL-Spareinlagen](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="de83b-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="de83b-106">In diesem video Chris Pels wird gezeigt, wie der Status eines oder mehrere Objekte in einem Benutzersteuerelement beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="de83b-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="de83b-107">Zuerst wird ein Benutzersteuerelement erstellt, die die Möglichkeit für einen Benutzer zur Angabe von Filterkriterien für die Suche darstellt.</span><span class="sxs-lookup"><span data-stu-id="de83b-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="de83b-108">Darüber hinaus wird eine begleitende Filterklasse erstellt, um die Filterinformationen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="de83b-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="de83b-109">Einige Elemente der Benutzeroberfläche werden die Filtersteuerelement sowie einige Methoden und Eigenschaften zum Speichern der aktuellen Filterinformationen in die Klasseninstanz Filter hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="de83b-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="de83b-110">Als Nächstes wird die Benutzer Control Persistenz implementiert mithilfe der RegisterRequiresControlState-Methode und zugeordneten speichern/wiederherstellen-Methoden.</span><span class="sxs-lookup"><span data-stu-id="de83b-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="de83b-111">Diese Methoden speichert die Instanz des Filter-Klasse und ihre Daten während der Seitenpostbacks.</span><span class="sxs-lookup"><span data-stu-id="de83b-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="de83b-112">Es wird eine Erläuterung dazu, wie mehrere Objekte in der Implementierung des Steuerelements Zustand zu speichern.</span><span class="sxs-lookup"><span data-stu-id="de83b-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="de83b-113">&#9654; Sehen Sie sich an (23 Minuten)</span><span class="sxs-lookup"><span data-stu-id="de83b-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
