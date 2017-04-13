<properties
   pageTitle="Voit lisätä huomautuksia tietolähteet | Microsoft Azure"
   description="Toimintaohjeet artikkelissa miten huomautuksia Azure tietoluettelon, kuten nimen, tunnisteet, kuvaukset ja asiantuntijoiden kohteita tietojen korostaminen."
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
   ms.date="09/21/2016"
   ms.author="maroche"/>


# <a name="how-to-annotate-data-sources"></a>Voit lisätä huomautuksia tietolähteet

## <a name="introduction"></a>Johdanto
**Microsoft Azure tietoluettelon** on täysin hallitun pilvipalvelussa, joka on järjestelmän rekisteröinnin ja järjestelmä on enterprise tietolähteiden etsiminen. Toisin sanoen tietoluettelon 's all about toimintamallia löytää, toimintaperiaatteet ja käyttäminen tietolähteiden ja auttaa organisaatioita tulee niiden annetuista tiedoista. Kun tietolähde on rekisteröitynyt tietoluettelon, sen metatietoja kopioidaan ja indeksoitu-palvelu, mutta Tarinan ei lopeta siellä. Tietoluettelon avulla käyttäjät voivat antaa omia kuvaava metatietojen – kuten kuvaukset ja tunnisteita – täydentää poimittujen tietolähteen metatiedot ja tehdä tietolähteen ymmärrettäviä Lisää osallistujia.

## <a name="annotation-and-crowdsourcing"></a>Huomautukset- ja crowdsourcing
Kaikilla on mielestä. Ja tämä on hyvä asian.
Tietoluettelo tunnistaa, että eri käyttäjien on eri perspektiivit yrityksen tietolähteiden ja, että jokainen perspektiivien voi olla arvokkaita. Ota huomioon seuraavat skenaariota:

* Järjestelmänvalvojan tietää palvelimien tai palvelujen tason palvelusopimus isännöivät tietolähteeseen.
* Tietokannan järjestelmänvalvoja näkee tietokantojen ja käsittelyn sallitun ETL windows varmuuskopioinnin aikataulu.
* Järjestelmän omistaja tietää prosessi, jossa käyttäjä pyytää tietolähteen käyttöoikeutta.
* Data steward tietää, miten varat ja määritteet tietolähteeseen yhdistäminen yrityksen tietomalliin.
* Määrityksen tietää, miten tietoja käytetään hän tukee liiketoimintaprosessien kontekstissa.

Kunkin perspektiivien tuoko ja tietoluettelon käyttää crowdsourcing-vaihtoehto, joka sallii kutakin niistä tallennetaan ja käyttää kuvan rekisteröityjä tietolähteiden metatiedoista. Käytä tietoluettelon-portaalissa, kunkin käyttäjän lisätä ja muokata omaan huomautukset aikana johtaa siihen, voit tarkastella muiden käyttäjien huomautukset.

## <a name="different-types-of-annotations"></a>Erityyppiset huomautukset
Tietoluettelo tukee huomautukset seuraavanlaisia:

| Huomautus     | Huomautuksia                                                                                                                                                                                                                                                                                                                                                           |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kutsumanimi  | Tietokoneen nimet voivat toimittaa resurssi tasolla, varmistaaksesi, että tietojen varat ymmärtää helpommin. Tietokoneen nimet ovat hyödyllisimpiä, kun pohjana olevan objektinimi on suojaus, lyhennetty tai muussa kuvaava käyttäjille.                                                                                                                            |
| Kuvaus    | Kuvaukset voivat toimittaa tiedot resurssi ja määrite / sarakkeen tasot. Kuvaukset ovat vapaamuotoiset lyhyt teksti huomautukset, jotka kuvaavat käyttäjän perspektiivi tietojen resurssi tai sen käyttöä.                                                                                                                                                              |
| Tunnisteet (käyttäjän tunnisteita)          | Tunnisteita voi antaa tietoja resurssi ja määrite / sarakkeen tasot. Käyttäjän tunnisteet ovat käyttäjän määrittämä otsikoita, jonka avulla voit luokitella tietoja resurssien tai määritteet.                                                                                                                                                                                                    |
| Tunnisteet (sanasto tunnisteita)          | Tunnisteita voi antaa tietoja resurssi ja määritteen / sarakkeen tasot. Sanasto tunnisteet ovat keskitetysti määrittämät sanaston termeistä, jonka avulla voit luokitella tietoja resurssien tai käyttämällä yleisiä liiketoiminta-luokituksen määritteet. Katso lisätietoja [määrittämisestä määräytyvät tunnisteissa Business-sanasto](data-catalog-how-to-business-glossary.md)                                                                                                                                                                                                    |
| Asiantuntijoiden        | Asiantuntijoiden voit toimittaa resurssi tasolla. Asiantuntijoiden tunnistamaan käyttäjien tai ryhmien tietoja asiantuntija perspektiivien käyttäminen voit yhteyshenkilönä kohtiin yhteyshenkilön käyttäjille, joilla löydät rekisteröity tietolähteitä ja on kysymykset, joihin olemassa olevia huomautuksia ei vastaa.  |
| Käyttöoikeuksien pyytäminen | Accessin tietojen voit toimittaa resurssi tasolla. Nämä tiedot ovat käyttäjille, jotka Tutustu tietolähde, jota hän ei vielä ole käyttöoikeuksia. Käyttäjien kirjoittaa sähköpostiosoite käyttäjän tai ryhmän, joka antaa käyttöoikeudet, prosessin tai työkalu, jonka käyttäjät tarvitsevat saat käyttöösi, URL-osoite tai kirjoittaa prosessin itse tekstinä. |
| Ohjeet | Ohjeissa on annettu resurssi tasolla. Resurssi on RTF-tiedot, jotka sisältävät linkit ja kuvat, ja jonka voi tarjota tieto, joka ei ole välitetään läpi, kuvaukset ja tunnisteet. |


## <a name="annotating-multiple-assets"></a>Lisätä huomautuksia useita kohteita
Valittaessa useita tietojen varat tietoluettelon portaalissa käyttäjät voi lisätä huomautuksia kaikki valitun varat yhdellä kertaa. Valittujen kohteiden, henkilöistä ja valitse ja anna yhdenmukaisia kuvaus ja määrittää tunnisteet ja asiantuntijoiden liittyvien tietojen kalusto koskevat huomautukset.

> [AZURE.NOTE] Tunnisteet ja asiantuntijoiden myös voit antaa, kun rekisteröinyt tietojen varat tietoluettelon tietojen lähde rekisteröinti-työkalu.

Kun valitaan useita taulukot ja näkymät, vain sarakkeiden kaikki valitut tiedot varat yhteiset näkyvät tietoluettelon-portaalissa. Tämän avulla käyttäjät voivat antaa tunnisteet ja kuvaukset kaikkien sarakkeiden kaikki valitun varat samaa nimeä.

## <a name="annotations-and-discovery"></a>Huomautukset- ja etsiminen
Samalla tavalla kuin poimittujen tietolähteen rekisteröinnin yhteydessä metatietojen on lisätty tietoluettelon hakuindeksin, käyttäjän metatiedot indeksoidaan. Tämä tarkoittaa, että paitsi huomautukset Varmista helpompi ymmärtää ne löytämään tietoja, huomautusten myös helpotat käyttäjät voivat löytää huomautettuja tietojen varat haun, joka katsoo sen perustelluksi niihin ehtojen perusteella.

## <a name="summary"></a>Yhteenveto
Tietolähteen rekisteröimistä tietoluettelon mahdollistaa tietojen havaittavissa rakenteellisia ja kuvaava metatiedot kopioidaan luettelon palvelun tietolähteen. Kun tietolähde on rekisteröity, käyttäjät voivat antaa helpompi osat tietoluettelon portaalin ja tutustu huomautukset.

## <a name="see-also"></a>Katso myös
- [Azure tietoluettelon käytön aloittaminen](data-catalog-get-started.md) -opetusohjelma vaiheittaiset lisätietoja huomautuksia tietolähteet.
