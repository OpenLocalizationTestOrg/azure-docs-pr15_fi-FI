<properties 
   pageTitle="Azure Vianmääritysoppaan – analysoinnin Mobile välitys" 
   description="Analyysin, seuranta, Segmentointi ja Raporttinäkymät-ikkunan vianmääritys-Azure Mobile välitys" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-analytics-monitoring-segmentation-and-dashboard-issues"></a>Oppaan Analytics, seuranta, Segmentointi ja Raporttinäkymät-ikkunan ongelmien vianmääritys

Seuraavassa on mahdolliset ongelmat ja miten Azure Mobile välitys kerää tietoja sovellusten, laitteet ja käyttäjien viestejä.

## <a name="missingdelayed-information"></a>Puuttuvat tai Delayed tiedot

### <a name="issue"></a>Ongelma
- Tiedot on viive näkyvä Analytics, Segmentointi tai Raporttinäkymät-ikkunan.
- Tietoja ei ole seuranta.
- Tietoja ei ole Analytics, Segmentointi tai Raporttinäkymät-ikkunan.
- Pallolla Segmentointi rajoitukset.

### <a name="causes"></a>Syitä

- Voit käyttää Analytics-Ohjelmointirajapinnan näytön API ja osia API tietoja sinulla ole puuttuu käyttöliittymässä on näkyvissä API kautta.
- Jos Azure Mobile välitys SDK ei ole oikein Integroitu sovellus kyselyjä sitten et voi Lisätietoja Analytics, Segmentointi, seuranta tai raporttinäkymiä.
- Osia ei voi muuttaa, kun ne on luotu, osia voi olla vain "kloonatun" (kopioidaan) tai "hävitetään" (poistettu). Osia voi sisältää vain 10 ehdot.
- Voit esikatsella seurannasta puuttuvat tiedot helpoiten testilaitteen asetukset/Poista tai asenna sovellus testi laitteeseen.
- Tiedot päivittyvät 24 tunnin välein Analytics, Segmentointi tai raporttinäkymiä.
- Tiedot uuteen osioon eivät ehkä näy 24 tuntia, kun ne on luotu, vaikka segmentin perustuu aiempia tietoja asti.
- Käyttöliittymän analytics tietojen suodattaminen näkyy kaikki tällaisia millä (versio 1 ja sovelluksen versio 2 kuten "kaatuu" suodatettu nimi näkyy) sovelluksen versiolla.
- Tunnin ajanjakso perustuu käyttäjien laiteasetusten päivämäärän, jotta käyttäjä, jonka puhelin on määritetty virheellisesti päivämäärä voi enää näy väärän ajanjakson.
- Vie ei palvelinpuolen tiedot kirjataan, kun "testata"-painikkeen avulla, tiedot kirjataan vain reaali push kampanjat.

## <a name="cant-locate-items-in-ui"></a>Löydä Käyttöliittymän osat

### <a name="issue"></a>Ongelma
- Ei voi luoda tiettyjen hallinta perusteella osia tai mukautetun sovelluksen tiedot tunnisteen ehdot.
- Ei löydy tiettyjen hallinta tai mukautetun sovelluksen tiedot tunnisteen ehtoja Analytics seuranta ja raporttinäkymiä.
- Ei voi tulkita Analytics, seuranta, Segmentointi tai koontinäytön tiedot.

### <a name="causes"></a>Syitä

- Jotkin laadittuihin kohteet ja sovelluksen tiedot tunnisteet ovat käytettävissä push ehtoina vain mutta ei välttämättä lisätty segmenttiin tai näkyvissä Analytics, seuranta tai Raporttinäkymät-ikkunan. 
- Sisäisten kohteita ja joita ei voi lisätä segmenttiin app tiedot tunnisteet tarvitset asennuksen luettelo kohdistamisen kunkin markkinointikampanjan ehtoja suorittaa saman toiminnon kuin kohdistamisen segmentin perusteella.
- Katso lisätietoja Azure Mobile välitys-käyttöliittymän Analytics, seuranta, Segmentointi ja raporttinäkymien osien pikavalikot ja niiden tiedot.

## <a name="crash-troubleshooting"></a>Kaatua vianmääritys

### <a name="issue"></a>Ongelma
- Sovellus kaatuu näkyvä Analytics seuranta ja Raporttinäkymät-ikkunan.

### <a name="causes"></a>Syitä

- Vianmääritys sovellus kaatuu nähdä Analytics, seuranta tai koontinäytön Varmista että julkaisutiedot aiempiin versioihin verrattuna SDK: n tunnetut ongelmat.
- Muita vianmääritys sovellus kaatuu suorittaa tapahtuman testi laitetta, jonka sovelluksesi asennettu ja tarkista laite-tunnuksen "– tapahtumiin"-osassa Azure Mobile välitys käyttöliittymän. Tee tapahtuman, joka aiheuttaa sovelluksesi voi kaatua ja hakea lisätietoja Azure Mobile välitys käyttöliittymän "Näytön – kaatumisen"-osassa. 

 
