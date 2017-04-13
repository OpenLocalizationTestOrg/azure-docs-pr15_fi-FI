<properties
   pageTitle="Yleisiä FabricClient poikkeukset | Microsoft Azure"
   description="Tässä artikkelissa kuvataan yleisiä poikkeukset ja virheitä, jotka FabricClient-ohjelmointirajapinnan on ilmenee suoritettaessa sovelluksen ja klusterin hallintatoiminnot."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Yleisiä poikkeukset ja virheitä, kun FabricClient-ohjelmointirajapinnan käyttäminen
[FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -ohjelmointirajapinnan käyttöön klusterin ja sovelluksen hallintatehtäviin palvelun kangasta-sovelluksen, palvelun tai klusterin järjestelmänvalvojat. Esimerkiksi sovelluksen käyttöönottoa, päivitys ja poisto-klusterin kuntotietojen tarkistuksen tai testausta palvelu. Sovellusten kehittäjille ja klusterin järjestelmänvalvojat voivat käyttää FabricClient-ohjelmointirajapinnan kehittää hallintatyökalut palvelun kangasta klusterin ja sovellukset.

On erilaisia toimintoja, jotka voidaan suorittaa FabricClient.  Kummassakin menetelmässä voivat palauttaa poikkeukset virheiden vuoksi virheellinen syöte, runtime virheitä tai lyhytkestoisia infrastruktuurin ongelmat.  Katso API-oppaat löydät, mitkä poikkeukset ovat ilmenee tietyn menetelmällä. Joitakin poikkeuksia, mutta joka on antanut useita eri [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) API mukaan. Seuraavassa taulukossa on ollut käytettävissä olevia kirjasimia FabricClient-ohjelmointirajapinnan kautta.

|Poikkeus| Virhe ilmenee, kun|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|[FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -objekti on suljettu-tilaan. Luovuttaa [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) objektin käytät ja vahvistaa uusi [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -objekti. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Toiminnon aikakatkaisu. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) palautetaan, jos toimintoa kestää yli MaxOperationTimeout suorittamiseen.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|Access-tarkistus toiminnon epäonnistui. E_ACCESSDENIED palautetaan.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Suorituksenaikainen virhe toimintoa suoritettaessa. FabricClient tavoilla voivat mahdollisesti palauttaa [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx), [virhekoodi](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) -ominaisuus ilmaisee syyn poikkeuksen. Virhekoodit on määritetty [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) -luettelossa.|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|Toiminto epäonnistui lyhytkestoisia virhetilassa, jokin lisätoimi vuoksi. Esimerkiksi toiminto saattaa epäonnistua, koska replikoiden äänistä tilapäisesti ei ole tavoitettavissa. Lyhytkestoisia poikkeukset vastaavat epäonnistui toiminnoista, joita voi yrittää uudelleen.|

Yleisiä [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) virheitä voi palauttaa [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx):

|Virhe| Ehto|
|---------|:-----------|
|CommunicationError|Viestintä-virheen seurauksena toiminto epäonnistuu, yritä uudelleen.|
|InvalidCredentialType|Tunnistetiedon tyyppi on virheellinen.|
|InvalidX509FindType|X509FindType on virheellinen.|
|InvalidX509StoreLocation|X509 tallennuspaikan on virheellinen.|
|InvalidX509StoreName|X509 liikkeen nimi on virheellinen.|
|InvalidX509Thumbprint|X509 varmenteen allekirjoitus-merkkijono on virheellinen.|
|InvalidProtectionLevel|Suojaustaso on virheellinen.|
|InvalidX509Store|Sertifikaatin ei voi avata X509.|
|InvalidSubjectName|Aihe on virheellinen.|
|InvalidAllowedCommonNameList|Yleisiä nimi-luettelosta merkkijonon muoto on virheellinen. Pilkuilla erotettu luettelo on oltava.|
