<properties
   pageTitle="Voit rekisteröidä tietolähteiden | Microsoft Azure"
   description="Toimintaohjeet artikkelissa korostaminen tietolähteiden rekisteröitymisestä Azure tietoluettelon, mukaan lukien metatietokentät poimittujen rekisteröinnin yhteydessä."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>


# <a name="how-to-register-data-sources"></a>Voit rekisteröidä tietolähteet

## <a name="introduction"></a>Johdanto
**Microsoft Azure tietoluettelon** on täysin hallitun pilvipalvelussa, joka on järjestelmän rekisteröinnin ja järjestelmä on enterprise tietolähteiden etsiminen. Toisin sanoen **Azure tietoluettelon** 's all about toimintamallia löytää, toimintaperiaatteet ja käyttäminen tietolähteiden ja auttaa organisaatioita tulee niiden annetuista tiedoista. Ja tekemään tietolähteen havaittavissa kautta **Azure tietoluettelon** ensimmäiseksi on kyseisen tietolähteen rekisteröimiseen.
## <a name="registering-data-sources"></a>Tietolähteiden rekisteröiminen
Rekisteröinti on tietolähteen metatiedot purkaminen ja **Azure tietoluettelon** palvelun tiedot kopioidaan. Tiedot pysyvät, johon se tällä hetkellä sijaitsee, ja pysyy järjestelmänvalvojat-valvonnassa ja nykyisen järjestelmän käytännöt.

Rekisteröi tietolähteen, Käynnistä **Azure tietoluettelon** tietojen lähteen rekisteröinti työkalun **Azure tietoluettelon** -portaalista. Kirjaudu sisään työpaikan tai oppilaitoksen tilille (sama Azure Active Directory tunnistetietoja kirjautuneena portaaliin) ja valitse haluat rekisteröidä tietolähteeseen.
Katso vaiheittaiset lisätietoja [Azure tietoluettelon käytön aloittaminen](data-catalog-get-started.md) -opetusohjelma.

Kun tietolähteen rekisteröity luettelon seuraa paikkaan ja indeksoi sen metatietoja, niin, että käyttäjät voivat hakea, Selaa, ja tutustu tietolähteen ja muodostaa sen sovelluksen tai työkalun haluamaansa paikkaan avulla.

## <a name="sources-supported"></a>Tuetut tietolähteet
Tutustu [Data Catalog DSR](data-catalog-dsr.md) tällä hetkellä tuettujen tietolähteiden luettelo.
<br/>


## <a name="structural-metadata"></a>Rakenteelliset metatiedot
Kun olet rekisteröidään tietolähde, rekisteröinti-työkalu purkaa valitset objektien rakenteen tietoja – tämä on tarkoitettu niin rakenteellisia metatiedot.

Kaikkien objektien rakenteellisia metatiedot sisällytetään objektin sijainti, niin, että käyttäjät, jotka löytämään tietoja voi käyttää näitä tietoja asiakastyökaluissa haluamaansa objektin yhdistäminen. Muuta rakenteellisia metatietoa sisältää objektinimi ja tyyppi ja määritteen/sarakkeen nimi ja kirjoita.

## <a name="descriptive-metadata"></a>Kuvaava metatiedot
Lisäksi core rakenteellisia metatietojen poimittujen tietolähteen tietojen lähteen rekisteröinti-työkalun myös purkaa kuvaava metatiedot. SQL Server Analysis Services ja SQL Server Reporting Services tämä on otettu näyttämiä näiden palvelujen kuvaus-ominaisuudet. SQL Server-arvojen avulla laajennettuna ominaisuutena ms_description puretaan. Oracle-tietokantaan tietojen lähteen rekisteröinti-työkalun Pura Kommentit-sarakkeessa ALL_TAB_COMMENTS-näkymä.

Lisäksi tietolähteeseen poimittujen kuvaava metatietoja käyttäjät voivat kirjoittaa kuvaava metatietojen tietojen lähteen rekisteröinti-työkalun avulla. Käyttäjien lisätä tunnisteet ja tunnistaa asiantuntijoiden objekteille on rekisteröity. Kaikki kuvaava metatiedot kopioidaan sekä rakenteellisia metatietojen **Azure tietoluettelon** -palveluun.

## <a name="including-previews"></a>Kuten esikatselu

Oletusarvon mukaan vain metatietojen poimittujen tietolähteet ja kopioida **Azure tietoluettelon** -palveluun, mutta tietoja tietolähteen usein tehdään helpottaa tietojen otoksen näe siellä.

**Azure tietoluettelon** tietojen lähteen rekisteröinti-työkalun avulla käyttäjät sisällytettävien tietojen tilannevedoksen esikatselun kunkin taulukon ja näkymä, joka on rekisteröity. Jos käyttäjä valitsee sisältämään esikatselut rekisteröinnin yhteydessä, rekisteröinti-työkalun sisällytetään enintään 20 tietueet kunkin taulukon ja Näytä. Tämä tilannevedos kopioituu sitten luettelon sekä rakenteellisia ja kuvaava metatiedot.


> [AZURE.NOTE]  Useita taulukoita, joissa on paljon sarakkeita voi olla enintään 20 tietueita niiden esikatselu.


## <a name="including-data-profiles"></a>Kuten käyttäjäprofiileissa

Samalla tavalla kuin, mukaan lukien esikatselu voit antaa arvokkaita kontekstin käyttäjien etsiminen **Azure tietoluettelon**tietolähteiden, mukaan lukien profiilin tiedot myös helpottavat selvittääksesi havaittuja tietolähteet.

**Azure tietoluettelon** tietojen lähteen rekisteröinti-työkalun avulla käyttäjät voivat sisältää tietoja profiilin kunkin taulukon ja näkymä, joka on rekisteröity. Jos käyttäjä valitsee sisällytettävien tietojen profiilin rekisteröinnin yhteydessä, rekisteröinti-työkalu sisältää tiedot tilastoja kunkin taulukon ja näkymässä, mukaan lukien:

* Monta riviä ja tietojen-objektin kokoa
* Tietojen ja rakenteen objektin viimeisimmän päivityksen päivämäärä
* Null tietueita ja sarakkeiden eri arvojen määrä
* Sarakkeiden vähintään, enintään, keskiarvo ja keskihajonta-arvot

Nämä tilastoja kopioidaan sitten luettelon sekä rakenteellisia ja kuvaava metatiedot.

> [AZURE.NOTE]  Teksti ja päivämäärä-sarakkeet eivät sisällä keskiarvon tai keskihajonnan tilastoja tietojen profiiliinsa.

## <a name="updating-registrations"></a>Merkintöjen päivittäminen

Rekisteröiminen tietolähde helpompaa havaittavissa **Azure tietoluettelon** avulla metatiedot- ja valinnaiset esikatselu uuttaa rekisteröinnin yhteydessä. Jos tietolähde on päivitettävä luettelon (esimerkiksi, jos objektin rakenne on muuttunut, taulukoiden alun perin pois sisällytetään tai käyttäjä haluaa esikatselut sisältyvien tietojen päivittäminen) tietojen lähteen rekisteröinti työkalu voidaan suorittaa uudelleen.

Rekisteröidään uudelleen jo rekisteröity tietolähteen suorittaa yhdistämisen "upsert"-toimintoa: ennestään olevat objektit päivitetään, kun uudet objektit luodaan. Käyttäjien palvelun **Tietoluettelon Azure** -portaalissa metatietoihin säilyvät.

## <a name="summary"></a>Yhteenveto
Kyseisen tietolähteen tietolähteen rekisteröimistä **Azure tietoluettelon** on helpompi löytää ja ymmärtää kopioimalla rakenteellisia ja kuvaava metatietojen tietolähteen luettelo-palvelun. Tietolähde on rekisteröity, kun se sitten voidaan lisätä huomautuksia, hallituista ja havaitun **Azure tietoluettelon** -portaalissa.

## <a name="see-also"></a>Katso myös
- [Azure tietoluettelon käytön aloittaminen](data-catalog-get-started.md) -opetusohjelma vaiheittaiset lisätietoja rekisteröi tietolähteet.
