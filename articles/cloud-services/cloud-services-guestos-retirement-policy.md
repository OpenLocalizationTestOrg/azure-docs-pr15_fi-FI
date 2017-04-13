<properties 
   pageTitle="Azure Vieras OS supportability ja käytöstä poistaminen käytännön opas | Microsoft Azure" 
   description="Tietoja siitä, mitä Microsoft tukee seikkojen Azure Vieras OS-käyttöjärjestelmän käyttämä pilvipalveluihin." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure Vieras OS supportability ja käytöstä poistaminen käytäntö
Tällä sivulla tiedot liittyvät pilvipalveluihin työntekijä ja web roolit (PaaS) Azure vieras-käyttöjärjestelmän ([Vieras OS](cloud-services-guestos-update-matrix.md)). Ei koske näennäiskoneiden (IaaS). 

Microsoft on julkaistu [tukevat Vieras OS käytännön](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Luet sivulla kerrotaan nyt käytännön toteuttamistapaa.

Käytäntö on 

1. Microsoft tue **vähintään OS Vieras uusimman kaksi perheille**. Kun perhe on poistettu käytöstä, asiakkaiden on virallinen päättymispäivämäärän päivittämään uudempaan tuetut Vieras OS tuoteperheeseen 12 kuukauden.
2. Microsoft tue **vähintään tuetut Vieras OS perheille kaksi uusinta versiota**. 
3. Microsoft tue **vähintään uusinta kaksi versiota Azure-SDK**-palvelussa. Kun SDK versio on poistettu käytöstä, asiakkaiden on virallinen päättymispäivämäärän päivittämään uudempaan 12 kuukauden. 

Joskus enemmän kuin kaksi perheille tai versioiden voidaan tukea. Virallinen Vieras OS: n tukitiedot näkyvät [Azure Vieras OS versioista ja SDK yhteensopivuuden matriisissa](cloud-services-guestos-update-matrix.md).


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Kun Vieras OS perhe tai versio on poistettu käytöstä 


Uuden asiakkaan Käyttöjärjestelmä **perheen** on otettu käyttöön viime uusi virallinen Windows Server-käyttöjärjestelmän versio julkaisemisen jälkeen. Aina, kun uusi Vieras OS perhe on otettu käyttöön, Microsoft Keskeytä vanhimmasta Vieras OS perhe. 

Uusi Vieras OS **versiot** tuodaan tietoja kuukausittain toteuttamaan MSRC uusimmat päivitykset. Normaali kuukausittain päivitykset vuoksi Vieras käyttöjärjestelmäversio on tavallisesti käytöstä 60 päivää sen julkaisemisen jälkeen. Tämä pitää vähintään kaksi Vieras-käyttöjärjestelmä kunkin perheen käytettäväksi. 

### <a name="process-during-a-guest-os-family-retirement"></a>Prosessin aikana Vieras OS perheen käytöstä poistaminen 


Kun käytöstä poistaminen ilmoitettiin, asiakkaiden on 12 kuukauden "siirtymä" ajan ennen vanhempia perheen poistetaan virallisesti-palvelusta. Siirtymän tällä hetkellä ulottaa harkintansa mukaan Microsoft. Päivitykset kirjataan [Azure Vieras OS versioista ja SDK yhteensopivuuden matriisissa](cloud-services-guestos-update-matrix.md).

Asteittain käytöstä poistaminen prosessin alkaa 6 kuukautta siirtymän ajan. Kyseisenä ajanjaksona:

1. Microsoft ilmoittaa asiakkaille käytöstä poistaminen. 
2. Azure-SDK uudempi versio ei tue käytöstä poistettuja Vieras OS perhe.
3. Uudet käyttöönoton ja pilvipalveluihin redeployments ei sallita käytöstä poistettuja perheen käyttöön

Microsoft säilyvät esitellä uuden Vieras käyttöjärjestelmäversio saamiesi uusimmat MSRC päivitykset, kunnes siirtymän ajan nimeltään "päättymispäivän" viimeinen päivä. Samaan aikaan mitään käynnissä pilvipalveluihin joita Azure SLA-kohdassa. Microsoft on voivat pakottaa päivitys, poistaminen tai lopettaa näistä palveluista tämän päivän jälkeen.



### <a name="process-during-a-guest-os-version-retirement"></a>Prosessin aikana Vieras käyttöjärjestelmäversio käytöstä poistaminen 
Jos asiakkaiden niiden Vieras OS päivitetään automaattisesti, heillä on aina huolehtia käsittely Vieras OS-versioiden kanssa. Ne aina käyttää Vieras OS uusimpaan versioon.

Vieras käyttöjärjestelmä julkaistaan joka kuukausi. Vuoksi säännöllisesti versioiden määrä jokaisessa versiossa on kiinteä käyttöikä.

60 päivän kyselyjä käyttöikä versio on "*käytöstä*". "Käytöstä" tarkoittaa, että versio poistetaan perinteinen Azure-portaalista. Se myös enää voidaan määrittää CSCFG määritys-tiedostosta. Aiemmin ominaisuuksissa jäävät käytössä, mutta uusi ominaisuuksissa ja koodi- ja olemassa olevissa käyttöympäristöissä päivityksiä ei sallita. 

Myöhemmin, Vieras-käyttöjärjestelmän versio "*vanhenee*" ja minkä tahansa asennusten käynnissä versiossa on päivitetty voimassa ja arvoksi päivittää automaattisesti Vieras OS tulevaisuudessa. Vanhenemisen tehdään erissä niin ajan, valitse se vanheneminen voivat vaihdella. 

Nämä jaksot voidaan tehdä pidempi Microsoftin harkinnan asiakkaan siirtymät helpottamiseksi. Muutokset ilmoitettava [Azure Vieras OS versioista ja SDK yhteensopivuuden matriisissa](cloud-services-guestos-update-matrix.md).



### <a name="notifications-during-retirement"></a>Ilmoitukset käytöstä poistaminen aikana 

* **Perheen käytöstä poistaminen** <br>Microsoft käyttää blogimerkintöjen ja Azure perinteinen portaalin ilmoitus. Asiakkaat, jotka yhä käyttävät käytöstä poistettuja Vieras OS perhe ilmoitetaan tehtävään järjestelmänvalvojille suora viestintä (sähköpostin, portaalin viestit, puhelu) kautta. Kaikki muutokset kirjataan tälle sivulle ja RSS-syöte on lueteltu tämän sivun alussa. 


* **Version käytöstä poistaminen** <br>Kaikki muutokset kirjataan tälle sivulle ja RSS-syötteen luettelossa tätä sivua, mukaan lukien release käytöstä ja vanhentumispäivämäärien alussa. Palvelujen järjestelmänvalvojat saavat sähköpostit, jos heillä on käytössä käytöstä Vieras käyttöjärjestelmäversio ja sukulaisten tarkasteltavaksi ominaisuuksissa. Nämä sähköpostit ajoituksen voi vaihdella. Yleensä ne ovat vähintään kuukausi, ennen kuin se, vaikka tämä ajoitus ei ole virallinen SLA. 


## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

**Miten siirron vaikutukset pienentämään?**

Käytettävä uusimman Vieras OS perheen suunnittelun Cloud Services-palvelut. 

1. Aloita siirto uudempaan perhe alkuvaiheessa suunnittelu. 
2. Määritä tilapäinen testi ominaisuuksissa Testaa uuden perhe-että pilvipalvelu. 
3. Aseta Vieras käyttöjärjestelmäversio **Automaattinen** (osVersion = * [.cscfg](cloud-services-model-and-package.md#cscfg) -tiedostoon) niin uusi Vieras käyttöjärjestelmä siirrosta tapahtuu automaattisesti.

**Entä jos web-sovelluksen omat vaatii käyttöjärjestelmän tarkempaa integrointi?**

Jos web-sovelluksen arkkitehtuuri vaatii tarkempaa riippuvuuden pohjana-käyttöjärjestelmässä, käytä ympäristö tukee ominaisuudet, kuten [Käynnistys tehtävät](cloud-services-startup-tasks.md) tai muiden laajennettavuus menetelmien, joka voi olla tulevaisuudessa. Vaihtoehtoisesti voit myös [Azuren näennäiskoneiden](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – infrastruktuurin palvelu)-kohtaa, johon olet vastuussa pohjana käyttöjärjestelmä, jota säilytetään.
 
## <a name="next-steps"></a>Seuraavat vaiheet
Tarkista uusimmat [Vieras OS vapauttaa](cloud-services-guestos-update-matrix.md).
