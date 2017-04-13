<properties 
    pageTitle="Azure Media-sovelluksen Mobile välitys käyttöönotto"
    description="Media-sovelluksen skenaarion toteuttamisesta Azure Mobile välitys" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
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

#<a name="implement-mobile-engagement-with-media-app"></a>Ota käyttöön Mobile välitys Media-sovelluksessa

## <a name="overview"></a>Yleiskatsaus

Teemu on mobile projektipäällikkö suuri media yritys. Hän viimeksi käynnistää uusi sovellus, jossa on erittäin suuri ladattava määrä. Hän on osumien hänen tavoitteet ladattavaksi, mutta edelleen hänen palauttaa-Investment(ROI) käyttäjäkohtainen ei vastaa hänen vaatimuksia. 

Teemu on jo määritetty, miksi hänen ROI on liian alhainen. Käyttäjien Pysäytä usein hänen Appilla kahden viikon kuluttua ja yleensä ne koskaan toinen käyttäjä palaa lomaltaan. Hän haluaa niin, että hänen app säilyttäminen.

Jotkin alkuperäinen testaus, kun hän on opitut asiat, kun hän tekee hänen käyttäjät, joilla on push-ilmoitukset ne yleensä jatkaa hänen-sovelluksella. Myös käyttäjille, jotka olivat passiivinen usein palauttaa sen mukaan, hän lähettää ilmoitukset-sovellukseen. Jukka päättää sijoittamaan jokin lisätoimi välitys-ohjelman hänen sovellus, joka käyttää Lisäasetukset kohdistamisen push-ilmoitukset.

Teemu on viimeksi Lue [Azure Mobile välitys - Aloitusoppaan ja parhaita käytäntöjä](mobile-engagement-getting-started-best-practices.md) ja päättänyt toteuttamisesta apuviivan suositukset.

##<a name="objectives-and-kpis"></a>Tavoitteet ja KPI: T

Keskeiset sidosryhmät John's sovelluksen tapaa. Liiketoiminnan johdetaan mainosten käyttäjien tarjoaman hänen media. Suurentamalla sisällön kulutettu käyttäjäkohtainen John kasvaa hänen tuloista. Kaikki sopivat yksi ensisijainen tavoite: niin, että myynti Active Directory-25 %: lla. He luovat Business suorituskykyilmaisimista (KPI) ja mittayksikön asema tämän tavoitteen

* Mainokset napsautettu käyttäjäkohtainen määrä
* Kuinka monta artikkelisivuja avoimelle (/ käyttäjä / istunnossa / viikossa / kk...)
* Mitkä ovat tuttuja luokkia

John's kokouksen avaimen sidosryhmien kanssa perusteella hän on määritetty hänen Business KPI: T. Hän seuraava osa 1 [Azure Mobile välitys - Aloitusoppaan ja parhaita käytäntöjä](mobile-engagement-getting-started-best-practices.md). 

Seuraavaksi hän luo seuraavat välitys KPI, varmista, että tavoitteet on saavutettu:

* Valvoa säilytys yli seuraavassa väleihin: päivittäin, viikoittain bi viikoittain tai kuukausittain.
* Aktiiviset käyttäjät-määrät
* Tallentaa sovelluksen luokitus-sovelluksessa

IT-työryhmän suositukset-perusteella seuraavat tekniset suorituskykyilmaisimet lisättiin seuraaviin kysymyksiin:

* Mikä on käyttäjän-polku (sivu on avattu, kuinka monta aika käyttäjät käyttävät sitä)
* Määrä kaatuu tai virheet istunnossa tapahtui?
* Mitä käyttöjärjestelmä Omat käyttäjät on käytössä?
* Mikä on koko näytön käyttäjille keskimäärin?
* Millaisia internet-yhteydet Omat täytyykö käyttäjän?

Kunkin KPI: n hän luokittelee tarvittavat tiedot ja hän tallentaa sen hänen playbook oikeaan paikkaan.

## <a name="engagement-program-and-integration"></a>Välitys ohjelma ja integrointi

Nyt, John on lopettanut määrittämisen hänen KPI: T, hän aloittaa hänen välitys strategia vaiheen 4 välitys ohjelmista ja niiden tavoitteet määrittämällä:    ![][1]

Valitse John siirtyy tarkempaa mukaan näkyy yksityiskohtaisesti jokaisen ohjelman push-ilmoitukset. Push-ilmoituksen määrittämät viisi osat:

1. Tavoite: mikä on ilmoitus tavoitteena
2. Miten tavoite saavutettu
3. Kohde: kuka saa ilmoituksen?
4. Sisältö: Mikä on sanamuotojen ja muodon koskeva ilmoitus (App/Out, sovellusten)
5. Milloin: mikä on paras hetken Lähetä tämä push-ilmoitus

    ![][2]

Lisätietoja viitata [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks).

Osan mukaan laadinta John 2 käyttää kohde-osassa voit määrittää hänellä on voi kerätä tietoja ja kirjoittaa hänen tunnisteen suunnitteleminen yhdessä IT-työryhmän kanssa toteuttamisesta ratkaisu. Viikon käyttöönotto ja käyttäjän hyväksymisen testaamiseen kuluttua John lopuksi käynnistää hänen ohjelmat.

##<a name="program-results"></a>Ohjelman tulokset

4 kuukautta myöhemmin, John tarkistaa ohjelmien toiminnan. Tervetuloa ohjelma ja viikoittain ohjelman kokouksen hänen tavoitteet. Käyttäjä, jolla on vain yksi istunto määrä vähenee, sovelluksen muut ominaisuudet ovat käytössä ja viikossa yhteyksien määrä on kaksinkertainen.

**Passiiviset ohjelma** auttaa ymmärtämään käyttäjän pieni Teemu. Se näkyy 15 % Passiiviset käyttäjät palata-sovellukseen. Mutta yleensä ne ei pidä active yli 1 kuukausi. Teemu häiriötilanne mahdolliset optimointi ja muita ilmoituksia ja laajentaminen hänen sisällön vaihtoehtojen tässä järjestyksessä.

**Tutustu ohjelma** ei toimi asianmukaisesti. Se kasvaa cross myyminen mutta ei riitä hänen tavoitteiden saavuttamiseksi. Teemu ilmaisee, että hän ei ole tarpeeksi tietojen asiaa kohdistamisen ja Ehdota sisällön. Hän lopettaa ohjelma ja ohjeessa on lähettäminen "toimituksellinen push-ilmoitukset" Azure Mobile välitys kanssa. Hänen lehdet on jo CMS-ratkaisun, push-ilmoitukset lähetetään, ja ne halua muuttaa.

Jukka päättää saavuttaa Ohjelmointirajapinnan on HTTP REST API, joka sallii hallinta Reach Kampanjat käyttämättä AZME Web-liittymän avulla. Tämän menetelmän kanssa John voi kerätä tietoja, hän tarvitsee ja Salli hänen kirjoittajien jatkaa CMS-ratkaisun.

Voit varmistaa, että ominaisuus toimii oikein, John pyytää IT-työryhmän on valppaina seuraavat seikat:

1. **Toiminnon järjestelmien** : ne kaikki on omien sääntöjen hallita push-ilmoitukset, jotta John luettelon kaikissa tapauksissa päättää ja tarkistaa, jos API käsittele sitä.
Esimerkiksi: Android push-järjestelmä sallii laajemmin, joka ei ole iOS tapauksessa.

2. **Ajanjakson**: John haluaa API, joka määrittää aikavälin ja määritä loppu kampanjat. Hän haluaa säilyttää kaikki häiritsevä ilmoitus bombing käyttäjiltä.

3. **Luokat**: Markkinointi-ryhmän valmistellaan erityyppisiin ilmoitat mallia. Teemu pyytää IT-ryhmän, voit määrittää luokkia Ohjelmointirajapinnan sisällä.

Kun olet kaikkia testejä Teemu on tyytyväinen. Tämä API alkuun lehdet silti lähettää push-ilmoitukset ja niiden CMS ja Azure Mobile välitys kerää kaikki käytöspiirteen tietoja niiden

Nämä 4 jälkeen ensin kuukausia tulokset vastaavat hyvä yleinen suorituskyky ja antaa varmuudella Teemu ja hänen taulun ROI kohti käyttäjän kasvaa 15 % kohden ja mobile myynti muodostaa 17.5 % kokonaismyynnistä, vain neljä kuukautta kasvu 7,5 %.

<!--Image references-->
[1]: ./media/mobile-engagement-media-scenario/engagement-strategy.png
[2]: ./media/mobile-engagement-media-scenario/push-scenarios.png

<!--Link references-->
[Media Playbook link]: https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks
