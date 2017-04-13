<properties 
   pageTitle="Muokkaa StorSimple laitteen kokoonpanotietoja | Microsoft Azure" 
   description="Tässä artikkelissa käsitellään toimintatavan StorSimple laitteella, joka on jo otettu StorSimple hallinta-palvelun avulla." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="09/29/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-modify-your-storsimple-device-configuration"></a>StorSimple hallinta-palvelun avulla voit muokata StorSimple-laitteen määrittäminen

## <a name="overview"></a>Yleiskatsaus 

Azure perinteinen portaalin **Määritä** sivun sisältää kaikki laitteen parametrit, voit määrittää uudelleen StorSimple laitteessa, joka hallitsee StorSimple hallintapalvelu. Tässä opetusohjelmassa kerrotaan, miten voit käyttää **määrittäminen** -sivulla seuraavat laitteen tason tehtävät:

- Laitteen asetusten muokkaaminen 
- Aika-asetusten muokkaaminen 
- DNS-asetusten muokkaaminen 
- Muokkaa verkkoliittymät
- Vaihda tai delegoida IP-osoitteet

## <a name="modify-device-settings"></a>Laitteen asetusten muokkaaminen

Laitteen asetukset ovat kutsumanimi laitteen ja laitteen kuvaus.

StorSimple laitteella, joka on yhteydessä StorSimple hallintapalvelu määritetään oletusnimi. Oletusnimi vastaa yleensä laitteen järjestysluvun. Laitteen oletusnimi on 15 merkkiä pitkä, kuten 8600-SHX0991003G44HT ilmaisee, esimerkiksi seuraavasti:

- **8600** – ilmaisee laitteen mallista.
- **SHX** – ilmaisee on tarkoitus-sivustossa.
- **0991003** - osoittaa tietyn tuotteen.
- **G44HT**- 5 viimeistä ovat kasvaa Ainutkertaiset sarjanumerot luomiseen. Ei ehkä ole järjestyksessä.

Voit muuttaa laitteen nimi ja määritä se yksilöllinen kutsumanimi valittua Azure perinteinen portaalin. Kutsumanimi voi olla mikä tahansa merkkiä, ja se voi olla enintään 64 merkin pituinen.

Voit myös määrittää laitteen kuvaus. Laitteen kuvaus auttaa yleensä tunnistaa omistaja ja laitteen fyysinen sijainti. Kuvaus-kenttään voi olla enintään 256 merkkiä pitkä.
 
## <a name="modify-time-settings"></a>Aika-asetusten muokkaaminen

Laitteen on aikaa jotta todennetaan cloud tallennustilan-palveluntarjoajalta. Valitse aikavyöhyke avattavasta luettelosta ja määrittää kaksi verkon aika Protocol (NTP). Ensisijainen NTP-palvelin tarvitaan ja määritetään, kun käytät Windows PowerShell StorSimple määrittäminen laitteeseen. Voit määrittää oletusarvon Windows Server **time.windows.com** NTP-palvelin. Voit tarkastella ensisijainen NTP palvelinkokoonpanon palvelun Azure perinteinen-portaalissa, mutta sinun on käytettävä Windows PowerShell-liittymän sen muuttamisesta.

Toissijainen NTP server-määritys on valinnainen. Voit määrittää toissijaisen NTP-palvelin perinteinen portaalin. 

NTP-palvelimen määritettäessä Varmista verkon sallii NTP-liikenne paikalliseen välittää Internet-yhteyttä palvelinkeskuksen. Varmista määritettäessä julkisen NTP-palvelin, verkon palomuurien ja suojauksen muissa laitteissa on määritetty sallimaan NTP liikenne kulkemaan ja sieltä ulkopuolelta verkon. Jos kaksisuuntainen NTP liikenne ei ole sallittua, sinun on käytettävä sisäiselle palvelimelle NTP (Windows-toimialueen ohjauskoneen tarjoaa toiminto). Jos laite ei pysty synkronoimaan aikaa, se välttämättä pysty vaihtamaan cloud tallennustilan palvelussa.

Voit tarkastella NTP palvelinten luettelo valitsemalla [NTP palvelinten Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-the-device-is-deployed-in-a-different-time-zone"></a>Mitä tapahtuu, jos laite on otettu käyttöön toisen aikavyöhykkeen?

Jos laite on otettu käyttöön toisen aikavyöhykkeen, muuttaa laitteen aikavyöhyke. Koska, että kaikki varmuuskopion käytännöt käyttää laitteen aikavyöhykkeen, varmuuskopion käytännöt säätää automaattisesti uuden aikavyöhykkeen mukaisesti. Käyttäjän toimia ei tarvita.

## <a name="modify-dns-settings"></a>DNS-asetusten muokkaaminen

DNS-palvelin käytetään, kun laite yrittää muodostaa yhteyden cloud tallennustilan-palveluntarjoajalta. Hyvän käytettävyyden tarvitaan määrittäminen ensisijainen ja Toissijainen DNS-palvelimet alkuperäisen laitteen käyttöönoton aikana. Määrittämään ensisijainen DNS-palvelin on käyttää Windows PowerShell-liittymän StorSimple-laitteessasi.

Voit muokata Toissijainen DNS-palvelin, voit käyttää Azure perinteinen portaalin.



## <a name="modify-network-interfaces"></a>Muokkaa verkkoliittymät

Laitteessasi on kuusi laitteen verkkoliittymät, joista neljä 1 GbE ja kaksi, jotka ovat 10 GbE. Nämä liityntäkohdat on merkintä tietojen 0 – tietojen 5. TIETOJA 0, tietojen 1, tietojen 4 ja 5 tiedot ovat 1 GbE tietojen 2 ja 3 tiedot ovat 10 GbE Verkkoliittymät.

**Verkon käyttöliittymän asetusten** määrittäminen kunkin käytettävän liittymät. Jotta suuren käytettävyyden, on suositeltavaa, että sinulla on vähintään kaksi iSCSI liittymät ja kaksi cloud käyttävä liittymää laitteen. Microsoft suosittelee, mutta eivät edellytä, että käyttämättömät liittymät poistetaan käytöstä.

Kun määrität jokin verkkoliittymät, sinun on määritettävä virtual IP (VIP).

TIETOJA 0 on cloud käytössä oletusarvoisesti. TIETOJA 0 määritettäessä, myös tarvitse kaksi kiinteä IP-osoitteiden määrittäminen, yhteen kunkin ohjauskoneen. Nämä kiinteä IP-osoitteet voidaan käyttää laitteen ohjaimet suoraan ja on hyötyä, kun asennat päivitykset laitteeseen tai kun käytät ohjaimet vianmääritystä varten.

StorSimple 8000 sarjan päivitys 1 tietojen 0 reititys arvo on määritetty pienimpään; Jos laite toimii StorSimple 8000 sarjan päivitys 1, pilven liikenne reititetään vuoksi tietojen 0 –. Pane merkille tämä, jos sinulla on useampi kuin yksi cloud käyttävä verkkoliittymän StorSimple laitteen.

>[AZURE.NOTE] Kiinteä IP-osoitteiden ohjaimen käytetään ylläpidon laitteeseen päivitykset. Tämän vuoksi kiinteä IP-osoitteet on oltava reitittää ja voitava muodostaa yhteys Internetiin.

Kunkin verkko-liittymän seuraavat parametrit ovat näkyvissä:

- **Nopeuden** – käyttäjän määrittämät parametria. TIETOJA 0, tietojen 1, tietojen 4 ja 5 tiedot ovat aina 1 GbE tietojen 2 ja 3 tiedot ovat 10 GbE liittymät.

     >[AZURE.NOTE] Nopeuden ja kaksipuolinen tulostus aina automaattinen-neuvotellaan. Jumbo-kehykset eivät ole tuettuja.
 
- **Käyttöliittymän tilaan** – käyttöliittymään voi ottaa käyttöön tai poistaa käytöstä. Jos käytössä, laite yrittää käyttää käyttöliittymässä. On suositeltavaa, että vain liittymät, jotka ovat yhteydessä verkkoon ja käyttää on käytössä. Poista käytöstä kaikki liittymistä, joita et käytä.

- **Liittymän tyyppi** – tämä parametri voit erottaa iSCSI tietoliikenteen cloud tallennustilan liikenne. Tämä parametri voi olla jokin seuraavista:

    - **Cloud käytössä** – käytössä laitteen käyttämällä liittymän pilveen yhteydessä.
    - **käytössä iSCSI** – käytössä, kun laite käytetään liittymän iSCSI isännällä viestiä.

    On suositeltavaa eristää iSCSI tietoliikenteen cloud tallennustilan liikenne. Huomaa myös, jos isäntä on sama kuin laitteen aliverkon sisällä, sinun ei tarvitse yhdyskäytävän; määrittäminen kuitenkin jos isäntä on eri aliverkon kuin laitteessa, sinun on yhdyskäytävän määrittäminen.

- **IP-osoite** – IPv4 tai IPv6: ta tai molempia. IPv4- ja IPv6 address perheille tuetaan laitteen Verkkoliittymät. Kun käytät IPv4, määrittää 32-bittinen IP-osoite (*xxx.xxx.xxx.xxx*) piste kymmenjärjestelmän. IPv6 käytettäessä toimittaa yksinkertaisesti 4-numeroinen etuliite ja 128 bittistä osoite luodaan automaattisesti laitteen verkko-käyttöliittymän, että etuliite perusteella.

- **Aliverkon** – Tämä viittaa aliverkkopeite ja on määritetty Windows PowerShell-liittymän kautta.

- **Yhdyskäytävän** – tämä on oletusarvo yhdyskäytävään, joka on käytettävä tämän liittymän, kun se yrittää yhteydenpito solmut, jotka eivät ole sama IP-osoitetilaa (aliverkon). Oletusarvo-yhdyskäytävä on oltava saman osoitetilan (aliverkon) kuin liittymän IP-osoite, aliverkkopeite mukaisesti.

- **Kiinteä IP-osoite** – tämä kenttä on käytettävissä vain silloin, kun määrität tiedot 0 käyttöliittymän. Toimintoja, kuten päivitykset tai laitteen vianmääritys joudut ehkä laitteen ohjaimen yhdistäminen. Kiinteä IP-osoite voidaan käyttää aktiivinen ja passiivinen ohjauskoneen laitteen.

Voit määrittää Controller 0 ja 1 ohjauskoneen uudelleen palvelun Azure perinteinen-portaalissa.

>[AZURE.NOTE] 
>
>- Asianmukaisen toiminnan varmistamiseksi Varmista käyttöliittymän nopeuden ja kaksipuolinen tulostus-valitsin, joka on liitetty kunkin laitteen käyttöliittymää. Vaihda liittymät olisi joko neuvottelemalla kanssa tai määritetty Gigabit Ethernet (1000 Mbps) ja kaksisuuntainen. Käyttöjärjestelmä nopeuksia tai yksisuuntainen liittymät aiheuttaa suorituskykyongelmia.
>
>- Pienennä keskeytyksiä ja käyttökatkot, on suositeltavaa portfast kunkin valitsin-portit, jotka laitteesi iSCSI verkon-liittymän muodostamisesta ottaminen käyttöön. Näin varmistat, että verkkoyhteys voidaan muodostaa nopeasti virhetilanteessa.
 
## <a name="swap-or-reassign-ips"></a>Vaihda tai delegoida IP-osoitteet

Tällä hetkellä, jos mikä tahansa verkkoliittymän ohjaimen määritetään VIP, joka on käytössä (tekijä: samaan laitteeseen tai jotakin muuta laitetta verkostossa), valitse ohjaimen epäonnistuu päälle. Tämän vuoksi on noudattamalla ERISNIMI Jos ovat vaihtaminen VIPs laitteen verkko-liittymän, koska luodaan kaksoiskappale IP-tilanteeseen.

Seuraavien toimien Vaihda tai määrittää minkään verkkoliittymät VIPs:

#### <a name="to-reassign-ips"></a>Voit määrittää IP-osoitteet

1. Poista sekä liittymät IP-osoite.

2. Kun IP-osoitteet ole valittuina, määrittää uuden IP-osoitteiden vastaaviin liittymät.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja määrittäminen [MPIO StorSimple laitteeseesi](storsimple-configure-mpio-windows-server.md).

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
     
