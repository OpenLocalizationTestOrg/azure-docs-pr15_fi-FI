<properties
 pageTitle="IoT keskittimeen HA ja DR | Microsoft Azure"
 description="Tässä artikkelissa kuvataan ominaisuuksia, jotta luonnissa käytettävissä IoT ratkaisujen tietojen palauttaminen ominaisuuksia."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT keskittimeen suuri käytettävyys ja tietojen palauttaminen

Azure palveluna IoT toiminnosta on suuri käytettävyys (HA) käyttämällä toistoja Azure alueen tasolla ilman mitään toimia vaatii ratkaisu. Lisäksi Azure sisältää useita ominaisuuksia, jotka auttavat ratkaisujen tietojen palauttaminen (DR) ominaisuuksia tai rajat-alueen käytettävyys luominen tarvittaessa. Täytyy suunnitella ja valmisteleminen ratkaisujen hyödyntää nämä DR ominaisuuksien, jos haluat antaa yleisen, rajat-alueen suuren käytettävyyden laitteiden tai käyttäjille. On artikkelissa [Azure liiketoiminnan jatkuvuuden tekniset ohjeet](../resiliency/resiliency-technical-guidance.md) käsitellään Liiketoiminnan jatkuvuus ja DR Azure valmiita ominaisuuksia. [Tietojen palauttaminen ja suuren käytettävyyden Azure sovellusten][] paperin sisältää Azure sovellusten HA ja DR strategioita arkkitehtuuri ohjeita.

## <a name="azure-iot-hub-dr"></a>Azure IoT keskittimeen DR
Lisäksi sisäisessä alueen HA IoT keskittimeen toteuttaa palauttaminen automaattisesti järjestelmiä, jotka edellyttävät käyttäjältä ei toimia. IoT keskittimeen DR aloitetaan itsestäsi, ja se on palautus aika-tavoite (RTO) 2 26 tuntia ja seuraavat palautus-kohdan tavoitteet (RPOs).

| Toiminto | RPO |
| ------------- | --- |
| Palvelun saatavuus rekisterin ja viestinnän toimintoja varten | CNAME-tietue katoamisen |
| Laitteen tunnistetietojen rekisterin käyttäjätiedot | 0 – 5 minuuttia tietojen menettämisen |
| Laitteen pilven viestit | Kaikki lukemattomat viestit menetetään. |
| Toiminnot, viestien seuranta | Kaikki lukemattomat viestit menetetään. |
| Cloud laitteen viestit | 0 – 5 minuuttia tietojen menettämisen |
| Cloud laitteen palautetta jonossa | Kaikki lukemattomat viestit menetetään. |

## <a name="regional-failover-with-iot-hub"></a>Alueellisten automaattisesti IoT keskittimeen kanssa

Täydellinen käsittely, IoT ratkaisujen käyttöönoton topologioissa on ulkopuolelle jääviä on tämän artikkelin, mutta suuri käytettävyys ja palauttaminen on tarkastelee *alueellisen automaattisesti* käyttöönotto-malli.

Alueellisen automaattisesti mallin ratkaisu takaisin lopussa on käynnissä ensisijaisesti yhdessä paikassa palvelinkeskukseen, mutta muita IoT-toiminnosta ja uudelleen otetaan käyttöön automaattisesti tarkoituksiin palvelinkeskuksen muualle siltä varalta, ensisijainen palvelinkeskuksen IoT-toiminnosta kärsii käyttökatkosta tai verkkoyhteyden laitteesta ensisijainen palvelinkeskukseen aiheuttaa keskeyttää. Laitteissa toissijainen Palvelupäätepisteen aina, kun ensisijainen yhdyskäytävä ei voi siirtyä. Rajat-alueen automaattisesti ominaisuus, jonka ratkaisu käytettävyyttä voi parantaa suuren käytettävyyden yhden alueen ulkopuolella.

Korkean tason toteuttamisesta alueellisen automaattisesti mallin kanssa IoT-toiminnossa tarvitset seuraavasti.

* **Toissijainen IoT keskittimeen ja laitteen reititys logiikan**: keskeytetty ensisijainen alueellasi, kyseessä laitteet on käynnistettävä muodostamisesta toissijainen alue. Valita tila-aware laatu useimmissa palveluista, on yleisiä käynnistettävän välisten alueen automaattisesti prosessin järjestelmänvalvojille ratkaisu. Paras tapa muodostaa uusi päätepiste laitteeseen, prosessin hallinta säilyttäen on niiden Tarkista säännöllisesti *concierge* -palvelun nykyisen aktiivisen päätepisteen. Concierge-palvelun voi replikoida ja säilyttää tavoitettavissa yksinkertaisen web-sovelluksen käyttäminen DNS-uudelleenohjauksen tekniikoita (esimerkiksi [Azure liikenteen hallinta][]).
* **Käyttäjätietojen rekisterin replikoinnin** - voi käyttää, jotta toissijaisen IoT toiminnosta on oltava kaikki laitteen käyttäjätietoja, jotka voit muodostaa yhteyden ratkaisu. Ratkaisu olisi Säilytä geo replikoida varmuuskopiot laitteen käyttäjätiedoista ja lataa ne toissijainen IoT keskittimeen ennen siirtymistä aktiivisen päätepisteen laitteiden. Laitteen tunnistetietojen Vie toimintoja IoT toiminnosta on erittäin hyödyllinen tässä yhteydessä. Lisätietoja [IoT keskittimeen Developer Guide - tunnistetietojen rekisterin][].
* **Logiikan yhdistäminen** – kun ensisijainen alue on käytettävissä uudelleen, kaikki tilaa ja tietoja, jotka on luotu toissijainen-sivustossa on siirrettävä takaisin ensisijainen alue. Tämä liittyy enimmäkseen laitteen käyttäjätietoja ja sovelluksen metatiedon, johon haluat liittää ensisijainen IoT-toiminnosta ja kaikki muut sovelluksen kielikohtaiset stores ensisijainen alueen. Helpottaa tämä vaihe on yleensä suositeltavaa, että käytät idempotent toimintoja. Tämä pienentää vaikutusten paitsi potentiaalisen yhdenmukaisia jakautumisen tapahtumia, mutta myös kaksoiskappaleita tai tapahtumien järjestys lähettämisen. Lisäksi sovelluksen logiikkaa olisi suunniteltava niin sallii mahdolliset epäyhtenäisyydet tai "hieman" päivämäärä tilan ulos. Tämän vuoksi muut järjestelmä "heal" kuluvaa aikaa perustuu palautus pisteen tavoitteet (RPO).

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Azure IoT keskittimeen seuraavista linkeistä:

- [IoT keskittimet (opetusohjelma) käytön aloittaminen][lnk-get-started]
- [Mikä on Azure IoT toiminnossa?][]

[Tietojen palauttaminen ja Azure sovellusten suuri käytettävyys]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure liikenteen hallinta]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT keskittimeen Developer Guide - tunnistetietojen rekisterin]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Mikä on Azure IoT toiminnossa?]: iot-hub-what-is-iot-hub.md
