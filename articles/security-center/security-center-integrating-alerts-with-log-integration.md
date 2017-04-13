<properties
   pageTitle="Azure Tietoturvakeskus ilmoitukset-integraation Azure log integrointi (ennakkoversio) | Microsoft Azure"
   description="Tämän artikkelin avulla voit aloittaa integroinnissa Tietoturvakeskus ilmoitusten Azure log integrointi."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Integrointi Tietoturvakeskus ilmoitusten Azure log-integrointi (ennakkoversio)

Useimmat suojaustoimintojen ja tapauksen vastauksen ryhmiä luottavat suojaustietojen ja tapahtuman Management (SIEM)-ratkaisun alkupisteeksi triaging ja tutkiminen suojausvaroitusten. Asiakkaiden synkronoida Azure log-integroinnin Azure Tietoturvakeskus ilmoitusten sekä virtuaalikoneen suojauksen tapahtumien keräämiä Azure diagnostiikka- ja Azure valvontalokien log analytics-tai SIEM ratkaisun lähes reaaliajassa.

Azure log integrointi toimii HP ArcSight, Splunk tai IBM QRadar.

## <a name="what-logs-can-i-integrate"></a>Mitä lokeja voi integroida?

Azure tuottaa monipuolisia kirjaaminen jokaisella palvelulla. Lokitiedostot luokitellaan seuraavasti:

- **Ohjausobjektin/hallinnan lokit** joka tuo näkyvyyttä Azure Resurssienhallinta luominen, Päivitä ja poista-toimintoja.
- **Tietoja tasossa lokit** joka antaa näkyvyys käynnistyy, kun käyttämällä Azure resurssin tapahtumat. Esimerkki on Windowsin tapahtumalokiin kirjaaminen - suojauksen ja sovelluksen kirjaa virtual machine.

Azure log integrointi tukee tällä hetkellä integrointi:

- Azure AM lokit
- Azure valvontalokien
- Azure Tietoturvakeskus ilmoitukset

## <a name="install-azure-log-integration"></a>Asenna Azure log-integrointi

Lataa [Azure Kirjaudu integrointi](https://www.microsoft.com/download/details.aspx?id=53324).

Azure log integration-palvelu kerää telemetriatietoja tietokoneesta, johon se on asennettu.  Telemetriatietojen kerättyjä tietoja on:

- Poikkeukset Azure suorituksen aikana ilmenevät Kirjaudu integrointi
- Arvot tietoja kyselyjen ja käsitellä tapahtumien määrä
- Mitä Azlog.exe tietoja komentorivivalitsimet käytetään tilastotiedot

> [AZURE.NOTE] Voit poistaa käytöstä telemetriatietoja kokoelma poistamalla asetus.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integroi Azure valvontalokien ja Tietoturvakeskus ilmoitukset

1. Avaa komentokehote ja **CD-levylle** **c:\Program Files\Microsoft Azure Log integrointi**kyselyjä.

2. Suorita **azlog createazureid** -komento [Azure Active Directory Service lyhennys](../active-directory/active-directory-application-objects.md) luominen Azure Active Directory (AD) alihallinnat, jotka isännöivät Azure tilaukset.

    Kysyy Azure kirjautumisen.

    > [AZURE.NOTE] Sinun on oltava tilauksen omistaja tai muiden tilauksen järjestelmänvalvoja.

    Azure-todennus on valmis Azure AD kautta.  Service-lyhennys Azure log integroinnin luominen luo Azure AD-tunnistetiedot, joka annetaan access lukea Azure tilaukset.

3. Suorita **azlog sallivat <SubscriptionID> ** komento lukuoikeudet tilauksen määrittämiseen palvelun lyhennys loit vaiheessa 2. Jos et määritä **SubscriptionID**, valitse palvelu lyhennys liitetään lukija kaikki tilaukset, joihin sinulla on käyttöoikeus.

    > [AZURE.NOTE] Voit nähdä varoitukset, jos **Hyväksy** -komennon suorittaminen heti **createazureid** -komennon jälkeen koska on joitakin viive, kun tili on käytettävissä – kun Azure AD-tili on luotu. Jos Odota noin 10 sekunnin jälkeen **createazureid** -komennon avulla **Hyväksy** -komennon suorittaminen, valitse ei olisi näy nämä varoitukset.

4. Tarkista Vahvista valvonta lokitiedostojen JSON onko seuraavat kansiot:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Tarkista, varmista, että Tietoturvakeskus ilmoitukset niitä seuraavat kansiot:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Osoita haluamaasi kansioon, voit tulostaa tiedot SIEM esiintymään vakio SIEM tiedoston forwarder yhdistin. Tutustu [SIEM käyttömahdollisuudet](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) SIEM kokoonpanosi.

Jos sinulla on kysyttävää Azure Log integrointi, Lähetä sähköpostia [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure valvontalokien ja ominaisuuden määrityksiä on seuraavissa artikkeleissa:

- [Valvonta-toimintojen resurssien hallinta](../resource-group-audit.md)
- [Luettelo tilauksen hallinta-tapahtumat](https://msdn.microsoft.com/library/azure/dn931934.aspx) - hakemiseen valvontalokin tapahtumien poistaminen.

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) , lue, miten voit hallita ja vastata suojausvaroitusten.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – uusimmista Azure suojaus ja tiedot.
