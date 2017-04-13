<properties
   pageTitle="Usein kysyttyjä kysymyksiä Azure tietoluettelon | Microsoft Azure"
   description="Usein kysyttyjä kysymyksiä Azure tietoluettelon, mukaan lukien ominaisuuksia tietojen lähteen etsiminen, huomautus ja hallinta."
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

# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure tietoluettelon usein kysytyt kysymykset

Tässä artikkelissa on vastauksia usein kysyttyihin kysymyksiin, jotka liittyvät Microsoft **Azure tietoluettelon** -palvelua varten.

## <a name="q-what-is-azure-data-catalog"></a>K: mikä on Azure tietoluettelon?

V: Microsoft Azure tietoluettelon on täysin hallitun palvelu isännöidään Microsoft Azure pilveen, joka on järjestelmän rekisteröinnin ja järjestelmä on enterprise tietolähteiden etsiminen. Azure tietoluettelon sisältää ominaisuuksia, jotka mahdollistavat käyttäjiä – analyytikot tietojen tutkijoiden kehittäjille – voit rekisteröityä, tutustu, ymmärtää ja käyttää tietolähteet.

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>K: mitä asiakkaan haastaa onko Azure tietoluettelon ratkaista?

Azure tietoluettelon sähköpostiviesteissä sallimalla käyttäjien osat yrityksen tietolähteet ja tutustu haasteisiin tietojen lähteen etsimiseen ja "Tumma-tiedot".

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>K: kuka on Kohdekäyttäjäryhmät Azure tietoluettelon varten?

Azure tietoluettelon sisältää tekniset ja tehtävistä selainpohjaisista käyttäjille, mukaan lukien:

- Tietoja sovelluskehittäjille, BI ja Analytics-asiantuntijoille: ketkä ovat vastuussa tuottaa tiedot ja analysoinnin sisältöä muiden tarjoaman
-   Tietojen Sisältöjärjestäjät: Käyttäjät, jotka tietävän tiedot, mitä se tarkoittaa ja miten se on tarkoitus käyttää ja mitä tarkoitusta varten
- Tietoja kuluttajille:, Käyttäjille, joiden helposti löydät, ymmärtää ja yhteyden muodostaminen tietoihin pystyttävä tarvitsee työn suorittamiseen niiden valinta-työkalulla
- Keskitetyn IT: kirjastonäkymiä tietolähteiden satoja for business-käyttäjät tarvitsevat, ja kuka on säilytettävä valvonta, kuinka tietoja käytetään päälle ja kuka

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>K: mikä on Azure tietoluettelon alueen käytettävyys?

Azure tietoluettelon-palvelut ovat tällä hetkellä käytettävissä seuraavat tiedot keskikohdan mukaan:

- Länsi USA
- Yhdysvaltojen Itä
- Länsi Europe
- Pohjois-Eurooppa
- Australia Itä
- Kaakkoisaasialaiset Aasian

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>K: mitkä ovat Azure tietoluettelon kohteita tietojen määrän rajoitukset?

Vapaa Edition ja Azure tietoluettelon on rajoitettu 5 000 rekisteröidyt tiedot resurssit.

Standard Edition, Azure tietoluettelon tukee enintään 100 000 rekisteröidyt tiedot resurssit.

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>K: mitkä ovat tuetut lähde- ja resurssi tietotyyppien?

Tutustu [Data Catalog DSR](data-catalog-dsr.md) tällä hetkellä tuettujen tietolähteiden luettelo.

## <a name="q-how-do-i-request-support-for-another-data-source"></a>K: miten pyydän tuki toisen tietolähteen?

Ehdottaa ominaisuuksia ja muita palaute voidaan lähettää [Azure tietoluettelon keskustelupalstalle](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>K: miten aloitetaan Azure tietoluettelon kanssa?

Aloita hyvä on noudattamalla seuraavia ohjeita [Tietojen käytön aloittaminen](data-catalog-get-started.md). Tämä artikkeli on yleiskatsaus lopusta loppuun-palvelun ominaisuuksia.

## <a name="q-how-do-i-register-my-data"></a>K: miten tiedot rekisteröidä?

Voit rekisteröidä tietojen Azure tietoluettelon, Käynnistä Azure tietoluettelon portaalin "Julkaise"-kohdassa Azure tietoluettelon rekisteröinti-työkalu. Azure tietoluettelon julkaisun-sovelluksessa Kirjaudu sisään samalla tunnistetiedoilla portaalin Azure tietoluettelon avulla ja valitse sitten tietolähteen ja haluat rekisteröidä tietyn resurssit.

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>K: mitä ominaisuuksia purkamisen, tietojen omaisuus, joka on rekisteröity?

Tietyt ominaisuudet eroavat tietolähteen tietolähteeseen, mutta yleensä Azure tietoluettelon julkaisun palvelun Pura seuraavat tiedot:

- Kohteiden nimi
- Kohteen tyyppi
- Kohteiden kuvaus
- Määritteen/sarakkeiden nimet
- Määritteen tai sarake-tietotyypit
- Määrite ja sarakkeen kuvaus

> [AZURE.IMPORTANT] Tietoja resurssien rekisteröimistä Azure tietoluettelon ei siirrä tai kopioi tiedot pilvipalveluun. Tietolähteen saatuja rekisteröiminen kopioidaan ne kalusteet metatietojen Azure, mutta tiedot säilyvät olemassa olevan tietolähteen sijainti. Ainoa poikkeus tähän sääntöön on, jos käyttäjä valitsee ladata tietueiden esikatselu tai profiilin tiedot, kun rekisteröidään kalusto. Kun kuten esikatselu, enintään 20 tietuetta kopioidaan kunkin resurssi ja Azure tietoluettelon tilannevedoksena tallennetaan. Kun myös tietojen profiili, kooste tiedot (esimerkiksi taulukoiden ja sarakkeiden pienin-, vähimmäis- ja keskiarvo-arvot saraketta kohti prosentti null-arvoja koon) lasketaan ja sisältyvät tallennetut luettelon metatiedot.

<br/>

> [AZURE.NOTE] Sovelluksen julkaiseminen Azure tietoluettelon purkaa tietolähteitä, kuten SQL Server Analysis Services, joiden pääosin **kuvaus** -ominaisuuden arvoa. SQL Server relaatiotietokantojen, joka ei ole pääosin **kuvaus** -ominaisuuden, Azure tietoluettelon julkaisun sovelluksen Pura arvon laajennettu objektit ja sarakkeet-ominaisuuden ms_description. Lisätietoja on TechNet- [Laajennettu ominaisuudet: tietokantaobjektit](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>K: miten kauan olisi kestää juuri rekisteröity varat näkyvän Azure tietoluettelon?

Kun olet rekisteröinyt varat Azure tietoluettelo 5 – 10 sekuntia, ennen kuin ne näkyvät Azure tietoluettelon portaalin ajan voi olla kanssa.

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>K: miten huomautuksia ja täydentää metatiedot Omat rekisteröidyt tiedot myös tulostaa?

Helpoin tapa antaa metatietojen rekisteröity varat on kohteen Azure tietoluettelon portaalissa ja kirjoita sitten valitun objektin metatietojen arvot ominaisuudet-ruudussa tai rakenne-ruudussa.

Voit myös lisätä joitakin metatietoja, kuten asiantuntijoiden ja tunnisteita, rekisteröinnin aikana. Azure tietoluettelon julkaisun palvelussa annettujen arvojen koskevat kaikki resurssit on rekisteröity aikalohkon. Voit tarkastella viimeksi rekisteröity objektit portaalissa lisää huomautuksen valitsemalla Azure tietoluettelon julkaisun sovelluksen viimeisessä näytössä **Näytä Portal** -painiketta.

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>K: Miten voin poistaa Omat rekisteröidyt tieto-objektit?

Voit poistaa objektin Azure tietoluettelon valitsemalla objektin portaalin ja valitsemalla sitten **Poista** -painiketta. Tämä poistaa objektin metatiedot Azure tietoluettelon, mutta ei vaikuta todellinen pohjana olevassa tietolähteessä.

## <a name="q-what-is-an-expert"></a>K: mikä on asiantuntija?

Asiantuntija on henkilö, joka on ajan tasalla perspektiivin, tietoja tieto-objekti. Objektin voi olla useita asiantuntijoilta. Asiantuntija ei tarvitse olla objektin; "omistaja" avustaja on yksinkertaisesti henkilölle, joka tietää, miten tiedot voi ja tulisi käyttää.

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>K: Miten voin jakaa tietoja Azure tietoluettelon ryhmän Jos voin ongelmia?

Käytä Azure tietoluettelon-keskustelupalstalla, voit raportoida ongelmista, jakaa tietoja ja esittää kysymyksiä. Neuvoa tukikäytännöistä http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>K: Azure tietoluettelon kanssa voidaan käyttää tätä muiden tietolähdettä käyttämällä olen?
Pyrimme parhaillaan aktiivisesti useampiin tietolähteisiin lisäämisestä Azure tietoluettelon. Jos tietolähde, jota haluat artikkelissa tuetut, ota ehdottaa sitä (tai äänen yhteyttä tukeen, jos ehdotetut sitä) [Azure tietoluettelon keskustelupalstalle](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>K: miten Azure tietoluettelon liittyy tietoluettelon Power BI Office 365: n?

Voit ajatella Azure tietoluettelo, tietoluettelon kehitys. Azure tietoluettelon toimittaa samanlaisia ominaisuuksia tietojen lähteen julkaiseminen ja etsiminen, mutta se on aktiivinen laajempi skenaariot ja Office 365: ssä ei ole riippuvainen. Pian, kun Azure tietoluettelon on yleensä käytettävissä kaksi luetteloiden yhdistäminen yksittäistä.

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>K: mitä oikeuksia käyttäjä on kalusto rekisteröitymään Azure tietoluettelon?

Azure tietoluettelon rekisteröinti-työkalun käyttäjälle on tietolähde, jonka avulla voi lukea metatiedot oikeudet. Jos käyttäjä valitsee myös esikatselun, valitse käyttäjällä on myös oltava käyttöoikeudet, jotka sallivat hänelle rekisteröintiä objektien tietojen lukeminen.

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>K: Azure tietoluettelon ne ovat käytettävissä myös paikalliseen ympäristöön?

Azure tietoluettelon on pilvipalvelussa, jota voidaan käyttää cloud ja paikallisten tietolähteiden välittää hybrid tietojen lähteen etsiminen ratkaista. Ei tällä hetkellä ei suunnitelmia Azure tietoluettelon-palvelun, joka suoritetaan paikallisen version.

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>K: olemme purkaa enemmän / tehokkaan metatietojen olemme rekisteröidä tietolähteistä?

Pyrimme parhaillaan aktiivisesti Laajenna Azure tietoluettelon ominaisuuksista. Jos näkyvissä on metatietoja, jotka haluat nähdä poimitun tietolähteen rekisteröinnin yhteydessä, ota Ehdota sen (tai äänestä sitä ehdotetut sen) [Azure tietoluettelon keskustelupalstalle](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Olemme tulevaisuudessa sallii kolmansien osapuolten Lisää uusi lähde tietotyypit laajennettavuus API kautta.

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>K: miten rekisteröidyt tiedot varat näkyvyyden rajoittaa niin, että vain tietyt henkilöt voivat löytää ne?

Vastaus: Valitse tietojen kalusto Azure tietoluettelossa ja "Kestää omistajuus"-painiketta. Tietoja resurssien Azure-tietoluetteloon omistajat voivat muuttaa näkyvyyden-asetuksia sallimaan joko kaikki luettelon käyttäjät Tutustu omistama kalusto tai rajoittaa näkyvyyttä tietyille käyttäjille.

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>K: miten rekisteröinnin, tiedot annetaan, jos haluat päivittää, joka muuttuu tietolähteen näkyvät luettelossa?

A:, jos haluat päivittää tiedot, jotka on jo rekisteröity luettelon resurssien metatiedot, riittää, että uudelleen tietolähteen rekisteröimiseen, joka sisältää ne kalusteet. Muutokset tietolähteeseen, kuten sarakkeiden lisäämisen tai poistamisen taulukoiden tai näkymien päivitetään luettelon, mutta huomautuksia käyttäjien säilyvät.

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>Kysymys et kysymykseesi tähän – mitä minun pitäisi tehdä?

Pää-päälle [Azure tietoluettelon keskustelupalstalle](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Kysytyt kysymykset etsii niiden tavalla.
