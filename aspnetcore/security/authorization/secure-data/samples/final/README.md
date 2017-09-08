# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="ad1d8-101">Build/sichere Benutzer Datenbeispiel Ausführung</span><span class="sxs-lookup"><span data-stu-id="ad1d8-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="ad1d8-102">Legen Sie das Kennwort, mit dem geheimen Schlüssel-Manager-Tool:</span><span class="sxs-lookup"><span data-stu-id="ad1d8-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="ad1d8-103">Die Datenbank zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="ad1d8-103">Update the database:</span></span>

    `dotnet ef database update`
