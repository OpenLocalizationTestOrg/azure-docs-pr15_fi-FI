<properties 
   pageTitle="Azure Vianmääritysoppaan - palvelun Mobiilisovellusten välitys" 
   description="Azure Mobile välitys apuviivat vianmääritys" 
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

# <a name="troubleshooting-guide-for-service-issues"></a>Oppaan palvelun ongelmien vianmääritys

Seuraavassa on mahdollista liittyvistä ongelmista ja Azure Mobile välitys-ohjelman suorittamistavan.

## <a name="service-outages"></a>Palvelukatkoksia

### <a name="issue"></a>Ongelma
- Ongelmat, jotka näyttävät johtua Azure Mobile välitys palvelukatkoksia.

### <a name="causes"></a>Syitä
- Ongelmat, jotka näyttävät johtua Azure Mobile välitys palvelukatkoksia voivat johtua useista eri ongelmat:
    - Eristettyjen ongelmat, jotka näkyvät alun perin systeeminen kaikissa Azure Mobile välitys
    - Tunnetut ongelmat aiheutuvat palvelimen katkokset (ei aina palvelintila näyttää):
    - Ajoittaminen viiveitä, Targeting virheitä, merkin update-ongelmien tilastotiedot Lopeta kerääminen Push lakkaa toimimasta, API Lopeta toimimasta, uudessa sovelluksessa tai käyttäjien ei voi luoda, DNS-virheet ja aikakatkaisun virheet Käyttöliittymän, API tai sovellusten laitteessa.
    - Cloud riippuvuuden katkokset [Azure palvelutila](http://status.azure.com/)
    - Push-ilmoituksen Services (PNS) riippuvuuden katkokset
    - App Store katkokset

1) Testaa, onko ongelma systeeminen voit testata kohteesta toiseen saman toiminnon
   
   - Azure Mobile välitys Integroitu sovellus
   - Testi-laite
   - Testaa laitteen käyttöjärjestelmän versio
   - Markkinointikampanja
   - Järjestelmänvalvojan
   - Selaimen (IE, Firefox, Chrome jne.)
   - Tietokoneen

2) Jos ongelma koskee vain Käyttöliittymän tai Ohjelmointirajapinnan testaaminen:

   - Testaa Azure Mobile välitys Käyttöliittymä ja Azure Mobile välitys API samaan toimintoon.

3) Jos ongelma liittyy matkapuhelimen verkon testaaminen:

   - Testaa yhdistettynä Internetin kautta WIFI ja yhdistettynä 3G matkapuhelimen verkon kautta.
   - Varmista, että palomuurin ei estä jokin Azure Mobile välitys IP-osoitteista ja porteista.

4) Testaa Jos ongelma ei-laitteen kanssa:

   - Testaa Jos laite on pystyttävä muodostamaan yhteys Azure Mobile välitys toiseen Azure Mobile välitys Integroitu sovellus kanssa.
   - Testaa, joita voit luoda tapahtumien, työt ja kaatuu puhelimesta, näkevät Azure Mobile välitys käyttöliittymässä. 
   - Testaa Jos voi lähettää push-ilmoitukset Azure Mobile välitys Käyttöliittymän laitteeseesi sen laitteen tunnuksen perusteella 

5) Jos ongelma liittyy sovelluksen testaaminen:

   - Asentaa ja testata sovelluksesi kohteesta emulaattorin sijaan fyysinen laitteesta:
   
6) Jos ongelma liittyy OS päivitykset käyttäjälle laitteet, joissa edellytetään SDK: ksi ratkaista testaaminen:

   - Testaa sovelluksesi eri laitteissa käyttöjärjestelmän eri versioiden kanssa.
   - Varmista, että käytät uusinta versiota SDK.
 
## <a name="connectivity-and-incorrect-information-issues"></a>Yhteys- ja virheellisten tietojen ongelmat

### <a name="issue"></a>Ongelma
- Lokiin kirjaaminen Azure Mobile välitys käyttöliittymän ongelmat.
- Yhteyden Azure Mobile välitys Ohjelmointirajapinnan kanssa virheet.
- Sovelluksen tiedot-tunnisteet kautta laitteen Ohjelmointirajapinnan ladattaessa ilmenevien ongelmien.
- Ongelmat lataamalla Azure Mobile välitys lokit tai viedyille tiedoille.
- Virheelliset tiedot Azure Mobile välitys-Käyttöliittymä.
- Virheelliset tiedot Azure Mobile välitys lokitiedot.

### <a name="causes"></a>Syitä
* Varmista, että käyttäjätililläsi on riittävät oikeudet tehtävän suorittamiseen.
* Varmista, että ongelma ei ole eristetty toisesta tietokoneesta tai paikalliseen verkkoon.
* Varmista, että Azure Mobile välitys palvelulla ei raportoitu katkokset.
* Varmista, että sovelluksen tiedot tunnisteen tiedostojen Noudata kaikkia sääntöjen:
    - Käytä vain UTF8 merkistö (ANSI-merkistön ei voi käyttää).
    - Käytä pilkkua "," erotinmerkin (Voit avata palvelun pyynnön avulla voidaan pyytää voit muuttaa .csv-erotinmerkki pilkku "," toisen merkin puolipisteellä esimerkiksi ";").
    - Kaikki pieniksi kirjaimiksi käytetään totuusarvoja, "true" ja "false".
    - Käytä tiedoston, joka on pienempi kuin suurin sallittu tiedostokoko 35 megatavua.
 
