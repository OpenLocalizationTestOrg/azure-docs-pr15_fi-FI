<properties
   pageTitle="Azure-tallennustilan suojauksen yleiskatsaus | Microsoft Azure"
   description=" Azure-tallennustila on cloud tallennustilan ratkaisu Moderni sovelluksia, jotka käyttävät kestävyyttä, käytettävyys ja skaalattavuus asiakkaidensa tarpeisiin. Tässä artikkelissa on yleiskatsaus tärkeä Azure suojausominaisuuksia, jotka voidaan käyttää Azure-tallennustilan. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Azure-tallennustilan suojauksen yleiskatsaus

Azure-tallennustila on cloud tallennustilan ratkaisu Moderni sovelluksia, jotka käyttävät kestävyyttä, käytettävyys ja skaalattavuus asiakkaidensa tarpeisiin. Azuren tallennustila on tietoturvaominaisuudet tarkoituksiin:

- Tallennustilan tilin voi suojata Roolipohjainen käyttöoikeuksien valvonta ja Azure Active Directoryn.
- Tietoja voi suojata siirron sovelluksen ja Azure välillä käyttämällä asiakaspuolen salausta, HTTPS tai SMB 3.0.
- Tietoja voidaan määrittää salata automaattisesti, kun kirjoitettu Azuren tallennustilaan käyttää tallennustilan Service-salausta.
- OS ja tietojen levyjen näennäiskoneiden käyttämä voi määrittää salata Azure salauksen.
- Tieto-objektit Azuren tallennustilaan valtuutetun pääsy voi myöntää jaettu Access allekirjoitusten käyttäminen.
- Käyttää henkilö, kun he käyttävät tallennustilan todentamismenetelmän voi seurata tallennustilan analytics.

Yksityiskohtaisempia tarkastella Azuren tallennustilaan suojaus on [Azuren tallennustilaan opas](../storage/storage-security-guide.md). Tässä oppaassa perinpohjaisesti käsittelevään artikkeliin Azuren tallennustilaan suojausominaisuudet tallennustilan tilin näppäimet, kuten salauksen siirron ja muille käyttäjille ja tallennustilaa analytics.

Tässä artikkelissa on yleiskatsaus Azure suojausominaisuuksia, jotka voidaan käyttää Azure-tallennustilan. Linkit ovat edellyttäen artikkeleihin, joissa antaa lisätietoja kullekin ominaisuudelle, niin voit lukea lisää.

Seuraavassa on tämän artikkelin kattama core-ominaisuuksia:

- Roolipohjainen käyttöoikeuksien valvonta
- Valtuutetun tallennustilan objektien käyttöä
- Siirron salaus
- Salauksen osoitteessa muille käyttäjille ja tallennustilaa palvelun salaus
- Azure salauksen
- Azure avaimen säilö

## <a name="role-based-access-control-rbac"></a>Roolipohjainen käyttöoikeuksien valvonta (RBAC)

Voit suojata tilisi tallennustilan Roolipohjainen käyttöoikeuksien valvonta (RBAC). [Hyvä tietää](https://en.wikipedia.org/wiki/Need_to_know) ja [käyttöoikeuksien myöntäminen](https://en.wikipedia.org/wiki/Principle_of_least_privilege) suojauksen periaatteiden perusteella käytön rajoittaminen on organisaatioissa, joissa haluat käyttää tietojen käytön suojauskäytäntöjä. Nämä käyttöoikeudet myönnetään kohdistamalla RBAC asianmukainen rooli ryhmiä ja sovellusten tietyn alueen. [Valmiin RBAC roolit](../active-directory/role-based-access-built-in-roles.md)-tallennustilan tilin osallistuja, kuten avulla voit määrittää käyttäjille oikeuksia.

Opi lisää:

- [Azure Active Directory Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Valtuutetun tallennustilan objektien käyttöä

Jaettu käyttö allekirjoituksen (SAS) tarjoaa valtuutetun resurssien tallennustilan tilissäsi. Suojaussidokset tarkoittaa, että voit myöntää asiakkaan rajoitetut käyttöoikeudet objektien tallennustila-tilisi vuosipoiston annettuna kautena ajan ja määritetyn käyttöoikeudet. Voit antaa nämä rajoitetut käyttöoikeudet eikä sinun tarvitse jakaa tilin pikanäppäimet. Suojaussidokset on URI, joka kattaa sen kyselyparametrit-kaikki todennetut käytön tallennustilan resurssille tarvittavat tiedot. Tallennustilan resursseja Suojaussidokset käyttämään asiakkaan tarvitsee vain siirtää Suojaussidokset oikeaa konstruktoria tai menetelmää.

Opi lisää:

- [Tietoja SAS-malli](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Luominen ja käyttäminen SAS Blob-objektien tallennustilaan](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Siirron salaus
Siirron salaus on puitteissa tietojen suojaaminen, kun se siirretään verkkojen. Azure-tallennustilan kanssa suojattu tietojen käyttäminen:

- [Transport-tason salausta](../storage/storage-security-guide.md#encryption-in-transit), kuten HTTPS siirrettäessä tietoja sisään tai ulos Azure-tallennustilan.
- [Wire salauksen](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), kuten Azure tiedostoresurssit SMB 3.0 salausta.
- [Asiakaspuolen salausta](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage), ennen kuin se siirretään varastoon tiedot salataan ja purkaa tiedot, kun se siirretään loppunut.

Lisätietoja asiakkaan salaus:

- [Asiakkaan salaus Microsoft Azuren tallennustilaan](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud security ohjaa sarja: salaaminen tietojen siirrettäessä](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Salauksen rest-palvelussa

Monissa organisaatioissa [salauksen rest-palvelussa](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) on pakollinen askel tietojen tietosuoja ja vaatimustenmukaisuus tietoja paikallisen tietosuojan. On kolme Azure ominaisuuksia, jotka sisältävät tiedot, jotka ovat "at"muut salaamista:

- [Tallennustilan palvelun salaus](../storage/storage-security-guide.md#encryption-at-rest) voit pyytää, että tallennuspalvelu salaa tiedot automaattisesti, kun kirjoitat sen Azure-tallennustilan.
- [Asiakkaan salaus](../storage/storage-security-guide.md#client-side-encryption) sisältää myös salauksen osoitteessa rest-toiminnon.
- Voit salata OS levyille ja tietojen levyjen käyttämä IaaS-virtuaalikoneen [Azure salauksen](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) .

Lisätietoja tallennustilan palvelun salaus:

- [Azure-tallennustilan palvelun salausta](https://azure.microsoft.com/services/storage/) ei [Azure-Blob-objektien tallennustilaan](https://azure.microsoft.com/services/storage/blobs/). Lisätietoja Azure muihin sijainteihin, on artikkelissa [tiedoston](https://azure.microsoft.com/services/storage/files/), [levyn (Premium tallennus)](https://azure.microsoft.com/services/storage/premium-storage/)tai [taulukon](https://azure.microsoft.com/services/storage/tables/) [jonossa](https://azure.microsoft.com/services/storage/queues/).
- [Tietoja muiden Azure tallennustilan palvelun salaus](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure salauksen

Azuren näennäiskoneiden (VMs) salauksen auttaa osoite organisaation suojaus ja yhteensopivuusvaatimukset salaamalla AM levyjen (mukaan lukien käynnistyksen ja tietojen levyjen) Key-tuotetunnusten ja [Azure avain](https://azure.microsoft.com/services/key-vault/)säilöön käyttöoikeuksia voidaan hallita käytännöt.

Levynsalaus VMs toimii Linux- ja Windows-käyttöjärjestelmien. Se myös käyttää avaimen säilö avulla voit suojata ja hallita levyn salausavaimet käytön valvonta. AM levyjen kaikki tiedot salataan käyttämällä yleisesti salaustekniikkaa Azure-tallennustilan tilin rest-palvelussa. Windowsin salauksen ratkaisu perustuu [Microsoft BitLocker-salaus](https://technet.microsoft.com/library/cc732774.aspx)ja Linux-ratkaisuun perustuu [dm crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Opi lisää:

- [Windows-ja Linux IaaS näennäiskoneiden Azure salauksen](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure avaimen säilö

Azure salauksen käyttää [Azure avaimen säilö](https://azure.microsoft.com/services/key-vault/) avulla voit määrittää ja hallita levyn salausavaimet ja avaimen säilöön-tilaukseen ja varmistaa, että kaikki tiedot virtuaalikoneen kohtauksen salataan Azuren tallennustilaan muille käyttäjille, että tietoja. Voit valvoa näppäimet ja käytännön käyttö käytettävä avaimen säilö.

Opi lisää:

- [Mikä on Azure avaimen säilö?](../key-vault/key-vault-whatis.md)
- [Azure avaimen säilö käytön aloittaminen](../key-vault/key-vault-get-started.md)
