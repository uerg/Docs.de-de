---
title: Verschlüsselung ruhender in ASP.NET Core
author: rick-anderson
description: Erfahren Sie mehr Details zur Implementierung von ASP.NET Core-Datenschutz-Verschlüsselung im Ruhezustand.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: c733540bbee2d48ab45cf2b230b7be1ee07fb146
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274699"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="285c9-103">Verschlüsselung ruhender in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="285c9-103">Key encryption at rest in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-encryption-at-rest"></a>

<span data-ttu-id="285c9-104">Wird standardmäßig die Datenschutzsystem [verwendet eine Heuristik](xref:security/data-protection/configuration/default-settings) um zu bestimmen, wie kryptografischen schlüsselmaterialien ruhende verschlüsselt werden soll.</span><span class="sxs-lookup"><span data-stu-id="285c9-104">By default, the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine how cryptographic key material should be encrypted at rest.</span></span> <span data-ttu-id="285c9-105">Der Entwickler kann außer Kraft setzen die Heuristik und manuell angeben, wie die Schlüssel im Ruhezustand verschlüsselt werden soll.</span><span class="sxs-lookup"><span data-stu-id="285c9-105">The developer can override the heuristic and manually specify how keys should be encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="285c9-106">Bei Angabe ein expliziter Schlüsselverschlüsselung zur Rest-Mechanismus wird die Datenschutzsystem den Standardmechanismus für die Speicherung Registrierung aufzuheben, den von die Heuristik zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="285c9-106">If you specify an explicit key encryption at rest mechanism, the data protection system will deregister the default key storage mechanism that the heuristic provided.</span></span> <span data-ttu-id="285c9-107">Sie müssen [Geben Sie eine explizite schlüsselspeichermechanismus](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), andernfalls das Data Protection-System nicht gestartet.</span><span class="sxs-lookup"><span data-stu-id="285c9-107">You must [specify an explicit key storage mechanism](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers), otherwise the data protection system will fail to start.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-providers"></a>

<span data-ttu-id="285c9-108">Die Datenschutzsystem wird mit drei wichtige Verschlüsselungsmechanismen mitgelieferten geliefert.</span><span class="sxs-lookup"><span data-stu-id="285c9-108">The data protection system ships with three in-box key encryption mechanisms.</span></span>

## <a name="windows-dpapi"></a><span data-ttu-id="285c9-109">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="285c9-109">Windows DPAPI</span></span>

<span data-ttu-id="285c9-110">*Dieser Mechanismus ist nur unter Windows verfügbar.*</span><span class="sxs-lookup"><span data-stu-id="285c9-110">*This mechanism is available only on Windows.*</span></span>

<span data-ttu-id="285c9-111">Wenn Windows DPAPI verwendet wird, werden Schlüsselmaterial über verschlüsselt [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) vor dem Speicher beibehalten wird.</span><span class="sxs-lookup"><span data-stu-id="285c9-111">When Windows DPAPI is used, key material will be encrypted via [CryptProtectData](https://msdn.microsoft.com/library/windows/desktop/aa380261(v=vs.85).aspx) before being persisted to storage.</span></span> <span data-ttu-id="285c9-112">DPAPI ist eine entsprechende Verschlüsselungsmechanismus für Daten, die niemals außerhalb des aktuellen Computers gelesen werden (obwohl es ist möglich, diese Schlüssel bis zu Active Directory zu sichern; Siehe [DPAPI und servergespeicherte Profile](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="285c9-112">DPAPI is an appropriate encryption mechanism for data that will never be read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="285c9-113">Beispiel: DPAPI-Schlüssel-at-Rest-Verschlüsselung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="285c9-113">For example to configure DPAPI key-at-rest encryption.</span></span>

```csharp
sc.AddDataProtection()
    // only the local user account can decrypt the keys
    .ProtectKeysWithDpapi();
```

<span data-ttu-id="285c9-114">Wenn `ProtectKeysWithDpapi` ohne Parameter aufgerufen wird, nur das aktuelle Windows-Benutzerkonto der persistente Schlüsselmaterial entschlüsseln kann.</span><span class="sxs-lookup"><span data-stu-id="285c9-114">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key material.</span></span> <span data-ttu-id="285c9-115">Optional können Sie angeben, dass jedes Benutzerkonto auf dem Computer (nicht nur das aktuelle Benutzerkonto) an das Schlüsselmaterial entschlüsseln könnte entsprechend dem folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="285c9-115">You can optionally specify that any user account on the machine (not just the current user account) should be able to decipher the key material, as shown in the below example.</span></span>

```csharp
sc.AddDataProtection()
    // all user accounts on the machine can decrypt the keys
    .ProtectKeysWithDpapi(protectToLocalMachine: true);
```

## <a name="x509-certificate"></a><span data-ttu-id="285c9-116">X. 509-Zertifikat</span><span class="sxs-lookup"><span data-stu-id="285c9-116">X.509 certificate</span></span>

<span data-ttu-id="285c9-117">*Dieser Mechanismus ist nicht verfügbar auf `.NET Core 1.0` oder `1.1`.*</span><span class="sxs-lookup"><span data-stu-id="285c9-117">*This mechanism isn't available on `.NET Core 1.0` or `1.1`.*</span></span>

<span data-ttu-id="285c9-118">Wenn Ihre Anwendung über mehrere Computer verteilt wird, kann es einfacher, eine gemeinsam genutzte x. 509-Zertifikat auf den Computern zu verteilen und Konfigurieren von Anwendungen für die Verwendung dieses Zertifikats für die Verschlüsselung der Schlüssel im Ruhezustand wird.</span><span class="sxs-lookup"><span data-stu-id="285c9-118">If your application is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and to configure applications to use this certificate for encryption of keys at rest.</span></span> <span data-ttu-id="285c9-119">Ein Beispiel finden Sie weiter unten.</span><span class="sxs-lookup"><span data-stu-id="285c9-119">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
```

<span data-ttu-id="285c9-120">Aufgrund der .NET Framework-Einschränkungen werden nur Zertifikate mit privaten Schlüsseln CAPI unterstützt.</span><span class="sxs-lookup"><span data-stu-id="285c9-120">Due to .NET Framework limitations only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="285c9-121">Finden Sie unter [zertifikatbasierte Verschlüsselung mit Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) unten mögliche problemumgehungen für diese Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="285c9-121">See [Certificate-based encryption with Windows DPAPI-NG](#data-protection-implementation-key-encryption-at-rest-dpapi-ng) below for possible workarounds to these limitations.</span></span>

<a name="data-protection-implementation-key-encryption-at-rest-dpapi-ng"></a>

## <a name="windows-dpapi-ng"></a><span data-ttu-id="285c9-122">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="285c9-122">Windows DPAPI-NG</span></span>

<span data-ttu-id="285c9-123">*Dieser Mechanismus steht nur für Windows 8 / WindowsServer 2012 und höher.*</span><span class="sxs-lookup"><span data-stu-id="285c9-123">*This mechanism is available only on Windows 8 / Windows Server 2012 and later.*</span></span>

<span data-ttu-id="285c9-124">Ab Windows 8 unterstützt das Betriebssystem DPAPI-NG (so genannte CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="285c9-124">Beginning with Windows 8, the operating system supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="285c9-125">Microsoft legt seine Verwendungsszenario wie folgt.</span><span class="sxs-lookup"><span data-stu-id="285c9-125">Microsoft lays out its usage scenario as follows.</span></span>

   <span data-ttu-id="285c9-126">Cloud computing, erfordert jedoch häufig, dass dieser Inhalt verschlüsselte auf einem Computer auf einem anderen entschlüsselt werden.</span><span class="sxs-lookup"><span data-stu-id="285c9-126">Cloud computing, however, often requires that content encrypted on one computer be decrypted on another.</span></span> <span data-ttu-id="285c9-127">Aus diesem Grund erweitert Microsoft beginnen mit dem Windows 8, des Konzepts der eine relativ einfache API Cloud-Szenarien umfassen.</span><span class="sxs-lookup"><span data-stu-id="285c9-127">Therefore, beginning with Windows 8, Microsoft extended the idea of using a relatively straightforward API to encompass cloud scenarios.</span></span> <span data-ttu-id="285c9-128">Diese neue API, DPAPI-NG namens können Sie sicher freigeben geheimen Schlüssel (Schlüssel, Kennwörter, Schlüssel) und Nachrichten von Schutz auf einen Satz von Prinzipalen, die sie nach dem richtigen Authentifizierung und Autorisierung auf verschiedenen Computern den Schutz verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="285c9-128">This new API, called DPAPI-NG, enables you to securely share secrets (keys, passwords, key material) and messages by protecting them to a set of principals that can be used to unprotect them on different computers after proper authentication and authorization.</span></span>

   <span data-ttu-id="285c9-129">Von [über DPAPI CNG](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="285c9-129">From [About CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794(v=vs.85).aspx)</span></span>

<span data-ttu-id="285c9-130">Der Prinzipal wird als ein Deskriptor Schutzregel codiert.</span><span class="sxs-lookup"><span data-stu-id="285c9-130">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="285c9-131">Betrachten Sie das folgende Beispiel, das Schlüsselmaterial verschlüsselt, so, dass nur der Domäne-Benutzer mit der angegebenen SID das Schlüsselmaterial entschlüsseln kann.</span><span class="sxs-lookup"><span data-stu-id="285c9-131">Consider the below example, which encrypts key material such that only the domain-joined user with the specified SID can decrypt the key material.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID=S-1-5-21-..."
    .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
    flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="285c9-132">Es gibt auch eine parameterlose Überladung der `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="285c9-132">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="285c9-133">Dies ist eine bequeme Methode zum Angeben der Regelnamens "SID = extrahieren", wobei extrahieren die SID des aktuellen Windows-Benutzerkonto ist.</span><span class="sxs-lookup"><span data-stu-id="285c9-133">This is a convenience method for specifying the rule "SID=mine", where mine is the SID of the current Windows user account.</span></span>

```csharp
sc.AddDataProtection()
    // uses the descriptor rule "SID={current account SID}"
    .ProtectKeysWithDpapiNG();
```

<span data-ttu-id="285c9-134">In diesem Szenario wird die AD-Domänencontroller für die Verteilung der Verschlüsselungsschlüssel, die von der DPAPI NG-Vorgängen verwendeten zuständig.</span><span class="sxs-lookup"><span data-stu-id="285c9-134">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="285c9-135">Der Zielbenutzer sind Lage die verschlüsselte Nutzlast über eine beliebige Domäne eingebundenen Computer entschlüsseln (vorausgesetzt, dass der Prozess unter ihrer Identität ausgeführt wird).</span><span class="sxs-lookup"><span data-stu-id="285c9-135">The target user will be able to decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="285c9-136">Zertifikatbasierte Verschlüsselung mit Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="285c9-136">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="285c9-137">Wenn Sie auf Windows 8.1 ausführen / Windows Server 2012 R2 oder höher, können Sie verwenden Windows DPAPI-NG zertifikatbasierte Verschlüsselung ausführen, auch wenn die Anwendung auf .NET Core ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="285c9-137">If you're running on Windows 8.1 / Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption, even if the application is running on .NET Core.</span></span> <span data-ttu-id="285c9-138">Um diesen Vorteil zu erstellen, verwenden Sie die Regel sicherheitsbeschreibungs-Zeichenfolge "Zertifikat HashId:thumbprint =", wobei Fingerabdruck den Hexadezimal codierten SHA1-Fingerabdruck des Zertifikats zu verwenden ist.</span><span class="sxs-lookup"><span data-stu-id="285c9-138">To take advantage of this, use the rule descriptor string "CERTIFICATE=HashId:thumbprint", where thumbprint is the hex-encoded SHA1 thumbprint of the certificate to use.</span></span> <span data-ttu-id="285c9-139">Ein Beispiel finden Sie weiter unten.</span><span class="sxs-lookup"><span data-stu-id="285c9-139">See below for an example.</span></span>

```csharp
sc.AddDataProtection()
    // searches the cert store for the cert with this thumbprint
    .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0",
        flags: DpapiNGProtectionDescriptorFlags.None);
```

<span data-ttu-id="285c9-140">Jede Anwendung, die an diesem Repository gezeigt wird, muss auf Windows 8.1 ausgeführt werden / Windows Server 2012 R2 oder höher in der Lage, diese Schlüssel zu entschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="285c9-140">Any application which is pointed at this repository must be running on Windows 8.1 / Windows Server 2012 R2 or later to be able to decipher this key.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="285c9-141">Benutzerdefinierte Schlüsselverschlüsselung</span><span class="sxs-lookup"><span data-stu-id="285c9-141">Custom key encryption</span></span>

<span data-ttu-id="285c9-142">Wenn die integrierten Mechanismen nicht geeignet sind, kann der Entwickler einen eigenen Mechanismus für die Schlüsselverschlüsselung angeben, durch Bereitstellen eines benutzerdefinierten `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="285c9-142">If the in-box mechanisms are not appropriate, the developer can specify their own key encryption mechanism by providing a custom `IXmlEncryptor`.</span></span>
