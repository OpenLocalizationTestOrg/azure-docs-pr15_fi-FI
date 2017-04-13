<properties
   pageTitle="Aloittaminen Azure log integrointi | Microsoft Azure"
   description="Opettele Azure log integrointi-palvelun asentaminen ja integroida lokit Azure-tallennustilan, Azure valvontalokien ja Azure Tietoturvakeskus ilmoitukset."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="get-started-with-azure-log-integration-preview"></a>Aloita Azure log-integrointi (ennakkoversio)

Azure log-integroinnin avulla voit integroida raaka lokit Azure resurssien paikallisen suojaustietojen ja tapahtuman Management (SIEM) järjestelmien. Integrointi on yhdistetty Raporttinäkymät-ikkunan kaikki oman varat paikallisen tai pilvipalvelussa, niin, että voit koostaa, yhdistää, analysoida ja ilmoita suojauksen tapahtumien liittyvät sovellukset.

Tässä opetusohjelmassa esitellään asentaa Azure log integrointi ja integroida lokit Azure-tallennustilan, Azure valvontalokien ja Azure Tietoturvakeskus ilmoitukset. Tässä opetusohjelmassa arvioitu kesto on tunnin.

## <a name="prerequisites"></a>Edellytykset

Jotta voit suorittaa tässä opetusohjelmassa, sinulla on oltava seuraavasti:

- Koneen (paikallisen tai pilvipalvelussa) Azure log integrointi-palvelun asentaminen. Tässä tietokoneessa on oltava käytössä 64-bittinen Windows-Käyttöjärjestelmää .net asennettuna 4.5.1 kanssa. Tämän tietokoneen kutsutaan **Azlog integraattori**.
- Azure tilaus. Jos ei ole, voit rekisteröityä [ilmaisen tilin](https://azure.microsoft.com/free/).
- Azure diagnostiikka käytössä Azuren näennäiskoneiden (VMs). Diagnostiikan käyttöön pilvipalveluihin, on artikkelissa [Azure diagnostiikka ottaminen käyttöön Azure Cloud Services-palveluissa](../cloud-services/cloud-services-dotnet-diagnostics.md). Ottaa käyttöön käynnissä Windows Azure-AM diagnostiikka-kohdassa [PowerShellin virtuaalikoneen käynnissä Windows Azure diagnostiikka käyttöön](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Yhteys-Azlog integraattori Azure säilöön ja todennusta ja Määritä Azure-tilaukseen.
- Azure AM lokitiedot SIEM-agentti (esimerkiksi Splunk yleinen Forwarder, HP ArcSight Windowsin tapahtumien keräystoiminto agentti tai IBM QRadar WinCollect) on oltava asennettuna Azlog integraattori.

## <a name="deployment-considerations"></a>Käyttöönottoon liittyviä huomioita

Voit suorittaa useita kertoja Azlog integraattori, jos tapahtuma on paljon. Kuormituksen tasaamisen *(WAD)* Windows Azure diagnostiikka tallennustilan tilit ja tilaukset esiintymät tarjota lukumäärän tulee perustua oman kapasiteetti.

8-suoritin (core)-koneessa yksittäisen esiintymän Azlog integraattori voit käsitellä tietoja 24 miljoonaa tapahtumien päivässä (~1M/hour).

4-suoritin (core), tietokoneeseen Azlog integraattori yksittäisen esiintymän voit käsitellä tietoja 1,5 miljoonaa tapahtumien päivässä (~62.5K/hour).

## <a name="install-azure-log-integration"></a>Asenna Azure log-integrointi

Lataa [Azure Kirjaudu integrointi](https://www.microsoft.com/download/details.aspx?id=53324).

Azure log integration-palvelu kerää telemetriatietoja tietokoneesta, johon se on asennettu.  Telemetriatietojen kerättyjä tietoja on:

- Poikkeukset Azure suorituksen aikana ilmenevät Kirjaudu integrointi
- Arvot tietoja kyselyjen ja käsitellä tapahtumien määrä
- Mitä Azlog.exe tietoja komentorivivalitsimet käytetään tilastotiedot

> [AZURE.NOTE] Voit poistaa käytöstä telemetriatietoja kokoelma poistamalla asetus.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Integroi Azure AM lokit Azure diagnostiikka tallennustilan tileistä

1. Tarkista varmistaa, että tilisi WAD tallennustila on kerääminen lokit ennen jatkamista Azure log-integrointi yllä edellytykset. Suorita seuraavat vaiheet, jos tilisi WAD tallennustila on ei kerääminen lokit.

2. Avaa komentokehote ja **CD-levylle** **c:\Program Files\Microsoft Azure Log integrointi**kyselyjä.

3. Suorita-komento

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      Korvaa StorageAccountName määritetty vastaanottamaan diagnostiikkatapahtumien lisääminen AM Azure-tallennustilan tilin nimi.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Jos haluat näkyvän tapahtuman XML Tilaustunnus-liittäminen tilauksen tunnus kutsumanimi:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Odota 30 – 60 minuuttia (voi kestää niin kauan kuin tunnin) ja valitse Näytä tapahtumat, jotka ovat vedetään tallennustilan tilin. Voit tarkastella avaamalla **Tapahtumienvalvonta > Windows-lokit > välittää tapahtumien** Azlog integraattori käyttöön.

5. Varmista, että vakio yhdistimen SIEM asennettu tietokoneeseen on määritetty poiminta tapahtumien **Välittää tapahtumat** -kansiosta ja pipe ne SIEM esiintymään. Tarkista SIEM kokoonpano ja katso integrointi lokitiedot.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Entä jos tietojen ei näy välittää tapahtumat-kansioon?

Jos tunnin jälkeen tietoja ei näy **Välittää tapahtumat** -kansioon, valitse:

1. Tarkista tietokoneen ja varmista, että se käyttää Azure. Testaa yhteys, yritä avata [Azure portal](http://portal.azure.com) selaimesta.

2. Varmista, että käyttäjän tilin **azlog** on kirjoitusoikeus-kansion **users\azlog**.

3. Varmista, että lisätty komento **azlog lähteen lisääminen** tallennustilan-tili näkyy, kun suoritat komennon **azlog lähde-luettelosta**.
4. Siirry **Tapahtumienvalvonta > Windows-lokit > sovelluksen** raportoitu Azure log-integrointi ole virheitä.

Jos et edelleenkään näe tapahtumat, valitse:

1. Lataa [Microsoft Azure-tallennustilan Explorer](http://storageexplorer.com/).

2. Yhdistä lisätty komento **azlog lähde Lisää**tallennustilaa-tiliin.

3. Siirry Microsoft Azure tallennustilan Resurssienhallinnassa taulukon **WADWindowsEventLogsTable** onko mitään tietoja. Jos et valitse AM diagnostiikka ei ole määritetty oikein.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integroi Azure valvontalokien ja Tietoturvakeskus ilmoitukset

1. Avaa komentokehote ja **CD-levylle** **c:\Program Files\Microsoft Azure Log integrointi**kyselyjä.

2. Suorita-komento

        azlog createazureid

      Tämä komento pyytää Azure kirjautumisnimi. Komento luo [Azure Active Directory Service lyhennys](../active-directory/active-directory-application-objects.md) Azure AD-Alihallinnat, jotka isännöivät Azure tilaukset, jotka kirjautuneena-käyttäjä on järjestelmänvalvoja, muiden järjestelmänvalvojan tai omistajan. Komento epäonnistuu, jos kirjautuneena-käyttäjä on vain Azure AD-vuokraajan Vieras-käyttäjän. Azure-todennus on tehty kautta Azure Active Directory (AD).  Azure AD tunnistetiedot, jotka saavat lukea Azure tilaukset access Luo luominen service-lyhennys Azlog integrointia varten.

3. Suorita-komento

        azlog authorize <SubscriptionID>

      Tämä määrittää lukuoikeudet tilauksen principal-palveluun, loit vaiheessa 2. Jos et määritä SubscriptionID, se yrittää pääasiallista reader roolia, joka on kaikki tilaukset, joihin sinulla on kaikki käyttöoikeus.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Voit nähdä varoitukset, jos **Hyväksy** -komennon suorittaminen heti **createazureid** -komennon jälkeen. On joitakin viive, kun tili on käytettävissä – kun Azure AD-tili on luotu. Jos Odota noin 10 sekunnin jälkeen **createazureid** -komennon avulla **Hyväksy** -komennon suorittaminen, valitse ei olisi näy nämä varoitukset.

4. Tarkista Vahvista valvonta lokitiedostojen JSON onko seuraavat kansiot:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Tarkista, varmista, että Tietoturvakeskus ilmoitukset niitä seuraavat kansiot:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Osoita haluamaasi kansioon, voit tulostaa tiedot SIEM esiintymään vakio SIEM tiedoston forwarder yhdistin. Voit joutua joitakin kentän yhdistämismäärityksiä, käytössäsi on SIEM tuotteen perusteella.

Jos sinulla on kysyttävää Azure Log integrointi, Lähetä sähköpostia [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit asentaa Azure log integrointi ja integroida lokit Azure säilöstä. Lisätietoja on seuraavissa artikkeleissa:

- [Microsoft Azure Log integrointi Azure lokit (ennakkoversio)](https://www.microsoft.com/download/details.aspx?id=53324) – tiedot, järjestelmävaatimukset ja asennusohjeet Azure log integroinnista Download Centeristä.
- [Johdanto Azure log integrointi](security-azure-log-integration-overview.md) – tässä asiakirjassa esitellään Azure log integrointi, sen tärkeimpiin ominaisuuksiin ja miten se toimii.
- [Kumppanin määritysvaiheet](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – on blogikirjoituksessa avulla voit määrittää Azure log integrointi kumppaniratkaisuihin Splunk, HP ArcSight ja IBM QRadar käyttöä varten.
- [Azure log integrointi usein kysytyt kysymykset](security-azure-log-integration-faq.md) – tämä usein kysytyt kysymykset vastauksia kysymyksiin Azure log integrointi.
- [Integrointi Tietoturvakeskus varoittaa Azure Kirjaudu integrointi](../security-center/security-center-integrating-alerts-with-log-integration.md) – tässä asiakirjassa kerrotaan, miten synkronoimaan Tietoturvakeskus ilmoitusten sekä virtuaalikoneen suojauksen tapahtumien keräämiä Azure diagnostiikka- ja Azure valvontalokien log analytics tai SIEM ratkaisu.
- [Azure diagnostiikka- ja Azure valvontalokien uudet ominaisuudet](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – tässä blogimerkinnässä kerrotaan Azure valvontalokien ja muita ominaisuuksia, jotta voit käyttää tietoja Azure resurssien toimintoja.
