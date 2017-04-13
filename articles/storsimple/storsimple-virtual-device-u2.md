<properties 
   pageTitle="StorSimple virtual laite päivityksen 2 | Microsoft Azure"
   description="Lue, miten voit luoda ja ottaa käyttöön StorSimple virtual laitteen virtual Microsoft Azure-verkoston hallinta. (Koskee StorSimple päivitys 2)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Käyttöönotto ja Azure StorSimple virtual laitteen hallinta


##<a name="overview"></a>Yleiskatsaus
StorSimple 8000 sarjan virtual laite on Lisää ominaisuus, joka sisältää Microsoft Azure StorSimple ratkaisu. StorSimple virtual laitteen suorittaa virtual tietokoneeseen Microsoft Azure virtual verkossa, ja voit käyttää sitä ja Kloonaa tietoja oman isäntien. Tässä opetusohjelmassa kerrotaan, miten käyttöön ja hallita Azure virtual laitteen ja koskevat virtual laitteiden ohjelmiston versiota Update 2 ja pienempi.


#### <a name="virtual-device-model-comparison"></a>Virtuaalinen laitteen tietomallien vertailu

StorSimple virtual laite on käytettävissä kaksi mallia, vakio 8010, (aiemmalta nimeltään 1100) ja premium 8020 (otettu käyttöön päivitys 2). Alla on taulukkomuotoinen vertailu kaksi mallia.


| Laitteen mallista          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Suurin mahdollinen**      | 30 TT                                                                     | 64 TT                                                                                                                                |
| **Azure AM**              | Standard_A3 (4 sydämiä, 7 Gigatavun muistia)                                                                      | Standard_DS3 (4 sydämiä, 14 Gigatavun muistia)                                                                                                                          |
| **Yhteensopivuus** | Käynnissä versioiden päivittäminen valmiiksi 2 tai uudempi versio                                             | Käynnissä päivitys, 2 tai uudempi versio                                                                                                  |
| **Alueen käytettävyys**   | Kaikki Azure alueet                                                         | Azure alueet, jotka tukevat Premium-tallennustilan<br></br>Alueiden luettelo on artikkelissa [Tuetut 8020 alueet](#supported-regions-for-8020) |
| **Tallennustyyppi**          | Käyttää Azure vakio tallennuspaikkaa paikallisen levyjen<br></br> Lisätietoja [tallennustilan vakio-tilin]() luominen | Käyttää Azure Premium tallennuspaikkaa paikallisen levyjä<sup>2</sup> <br></br>Lue, miten [Premium-tallennustilan tilin](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk) luominen                                                               |
| **Kuormituksen ohjeet**     | Kohteen tiedostot varmuuskopioista tason noutaminen                                              | Cloud keskihajonta ja testaa skenaariot, pieni viive, suurempi suorituskyvyn toiminnoista <br></br>Toissijainen laitteen tietojen palauttaminen                                                                                            |
 
<sup>1</sup> *1100 ennen nimeltään*.

<sup>2</sup> 8010 ja 8020 *käyttää Azure vakio tallennustilan cloud taso. Ero on vain paikallinen ylätasolla sisällä laitteen*.

#### <a name="supported-regions-for-8020"></a>Tuettujen alueiden 8020 varten

Alla on taulukkomuotoinen Premium tallennustilan alueiden tietueet, jotka ovat tällä hetkellä tueta 8020. Tämän luettelon päivitetään jatkuvasti, kuten Premium tallennustila on käytettävissä Lisää alueilla. 

| S-KIRJAINTA. Ei.                                                  | Tällä hetkellä tueta alueilla |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | Keskitetyn USA                     |
| 2                                                       |  Yhdysvaltojen Itä                       |
| 3                                                       |  Yhdysvaltojen Itä 2                     |
| 4                                                       | Länsi USA                        |
| 5                                                       | Pohjois-Eurooppa                   |
| 6                                                       | Länsi Europe                    |
| 7                                                       | Kaakkoisaasialaiset Aasian                 |
| 8                                                       | Japani Itä                     |
| 9                                                       | Japani Länsi                     |
| 10                                                      | Australia Itä                 |
| 11                                                      | Australia varaaja *           |
| 12                                                      | Itä-Aasian *                     |
| 13                                                      | Etelä keskitetyn US *              |

* Premium tallennustila on käynnistetty äskettäin nämä geos.

Tässä artikkelissa kuvataan vaiheittaisiin ohjeisiin siirtymistä otat käyttöön StorSimple virtual laitteen Azure-tietokannassa. Luettuasi tämän artikkelin menettelet seuraavasti:

- Tietoja siitä, miten virtual laitteen eroaa fyysinen laite.

- Voi luominen ja määrittäminen virtual laite.

- Muodosta yhteys virtual laite.

- Opettele käsittelemään virtual laitteen kanssa.

Tässä opetusohjelmassa koskee kaikkia StorSimple virtual laitteita käynnissä päivitys, 2 tai uudempi versio. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Miten virtual laitteen eroaa fyysinen laite

StorSimple virtual laite on StorSimple, joka suoritetaan, valitse yksittäinen solmu: Microsoft Azure Virtual kone vain ohjelmiston versiota. Virtuaalinen laite tukee tietojen palauttaminen käyttötavoista jossa fyysistä laitetta ei ole käytettävissä, ja vastaava käytettäviksi Kohdetason hakeminen varmuuskopiot, paikallisen palauttaminen ja cloud keskihajonta ja testaa skenaarioita.

#### <a name="differences-from-the-physical-device"></a>Fyysinen laite erot

Seuraavassa taulukossa on joitakin keskeisiä eroja StorSimple virtual laite ja StorSimple fyysinen laitteen välillä.

|                             | Laitteen fyysinen                                          | Virtuaalinen laite                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Sijainti**                    | Sijaitsee joten.                               | Suorittaa Azure-tietokannassa.                                                                            |
| **Verkon liityntäkohdat**          | On kuusi verkkoliittymät: tietojen 0 – tietojen 5.                  | On vain yksi verkkoliittymän: tietojen 0.                                        |
| **Rekisteröinti**                | Rekisteröity määritys vaiheessa.                | Rekisteröinti on erillinen tehtävä.                                                          |
| **Palvelun tiedot salausavain** | Luo fyysinen laitteessa ja päivittää virtual laitteen uusi avain.           | Ei voi luoda virtuaalisia laitteesta uudelleen. |


## <a name="prerequisites-for-the-virtual-device"></a>Virtuaalinen laitteen edellytykset

Seuraavissa osissa kerrotaan StorSimple virtual laitteen määritysten edellytyksistä. Ennen käyttöönotto virtual laite, tarkista [virtual laitteella suojaustietoja](storsimple-security.md#storsimple-virtual-device-security).

#### <a name="azure-requirements"></a>Azure vaatimukset

Ennen kuin voit valmistella virtual laitteen, sinun on suorittaa seuraavat valmistelut Azure-ympäristössä:

- Virtuaalinen laitteen, [Määritä Azure virtual verkossa](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Jos Premium tallennustilaa, sinun on luotava virtual verkon Azure alueessa, joka tukee Premium-tallennustilan. Lisätietoja [alueet, jotka tällä hetkellä tueta 8020](#supported-regions-for-8020).
- On suositeltavaa käyttää myöntämä Azure sijaan omia DNS-palvelimen nimi, joka määrittää DNS oletuspalvelimeen. Jos DNS-palvelimen nimi ei ole kelvollinen tai DNS-palvelin ei pysty ratkaisemaan IP-osoitteet oikein, virtual laitteen luominen epäonnistuu.
- Pisteen sivuston ja sivuston sivuston on valinnainen, mutta ei tarvita. Jos haluat, että, näiden asetusten määrittäminen monimutkaisemman skenaarioita. 
- Voit luoda virtuaalisen verkkoon, voit käyttää virtual laitteen näyttämiä asemat [Azuren näennäiskoneiden](../virtual-machines/virtual-machines-linux-about.md) (host-palvelimissa). Seuraavissa palvelimissa on täytettävä seuraavat vaatimukset:                            
    - Windows- tai Linux VMs liittyä iSCSI käynnistäjä ohjelmistot.
    - Suoritettava virtual laitteen virtual samassa verkostossa.
    - Yhteyksiä voi muodostaa virtual laitteen sisäisen IP-osoitteen virtual laitteesi iSCSI kohteeseen.

- Varmista, että olet määrittänyt iSCSI ja pilvi liikenne tuki virtual samassa verkossa.


#### <a name="storsimple-requirements"></a>StorSimple vaatimukset

Varmista seuraavat päivitykset Azure StorSimple-palveluun, ennen kuin voit luoda virtuaalisen laitteen:


- Lisää [access ohjausobjektin tietueet](storsimple-manage-acrs.md) , jotka ovat käyttäjästä tulee host palvelinten virtual laitteeseesi VMs.

- Käytä [tallennustilan tilin](storsimple-manage-storage-accounts.md#add-a-storage-account) samalla alueella on virtual laite. Heikko suorituskyky voi johtaa tallennustilan tilit eri alueilla. Voit käyttää vakio- tai Premium-tallennustilan tilin virtual laitteen kanssa. Lisätietoja siitä, miten voit luoda [Vakio-tallennustilan tilin]() tai [Premium-tallennustilan tilin](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)

- Käyttää eri tallennustilan tilin virtual laitteen luomiseen käytetty tiedoillesi. Heikko suorituskyky voi johtaa saman tallennustilan tilin avulla.

Varmista, että sinulla on seuraavat tiedot ennen aloittamista:

- Azure perinteinen portaalin tilisi tunnistetiedoilla.

- Laitteen fyysinen palvelun tiedot salausavaimen kopio.


## <a name="create-and-configure-the-virtual-device"></a>Luo ja määritä virtual laite

Ennen kuin suoritat nämä toimet, varmista, että [virtual laitteen edellytykset](#prerequisites-for-the-virtual-device)täyttyvät. 

Sen jälkeen, kun olet luonut virtual verkkoon, määritetty StorSimple hallintapalvelu ja rekisteröidyn laitteen fyysinen StorSimple-palvelussa, voit seuraavia ohjeita voit luoda ja määrittää StorSimple virtual laite. 

### <a name="step-1-create-a-virtual-device"></a>Vaihe 1: Virtual laitteen luominen

Seuraavien toimien StorSimple virtual laitteen luomiseen.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Jos virtual laitteen luominen epäonnistuu tämä vaihe, voit ehkä ole Internet-yhteys. Jos haluat lisätietoja, siirry [Internet-yhteys virheiden](#troubleshoot-internet-connectivity-errors) vianmääritys kun creatig virtual laite.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Vaihe 2: Määritä ja rekisteröi virtual laite

Ennen kuin aloitat nämä toimet, varmista, että palvelun tiedot salausavaimen kopio. Palvelun tiedot salausavaimen luotiin, kun olet määrittänyt ensimmäisen StorSimple laitteen ja tallenna se turvalliseen paikkaan pyydettiin. Jos palvelun tiedot salausavaimen kopio ei ole, ota yhteyttä Microsoft Support järjestelmänvalvojalta.

Seuraavien toimien ja rekisteröi StorSimple virtual laitteen.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Vaihe 3: (Valinnainen) Muokkaa laitteen asetukset

Seuraavassa kerrotaan laitteen asetukset tarvitaan StorSimple virtual laite, jos haluat käyttää CHAP StorSimple tilannevedoksen hallinta tai laitteen järjestelmänvalvojan salasanan vaihtaminen.

#### <a name="configure-the-chap-initiator"></a>Määritä CHAP lähettäjä

Tämä parametri sisältää tunnistetiedot, jotka virtual laitteen (kohde) odottaa-laitteistokäynnistäjät (palvelimet), jotka yrittävät käyttää asemat. Laitteistokäynnistäjät tarjoaa CHAP käyttäjänimen ja CHAP salasanan tunnistamiseen laitteeseen todennus aikana. Yksityiskohtaisia ohjeita Siirry [Määrittäminen CHAP laitteeseesi](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Määritä CHAP kohde

Tämä parametri sisältää tunnistetiedot, jotka virtual laitteen käyttää, kun CHAP käyttävä käynnistäjä pyytää keskinäistä tai kaksisuuntaisen todennusta. Virtuaalinen laitteen käyttää käänteinen CHAP käyttäjänimi ja peruuttaa CHAP salasana tunnistavan itse käynnistäjä todennus vaihdon. Huomaa, että CHAP kohde asetuksia Yleiset asetukset. Kun ne on otettu käyttöön, kaikki liitetty tallennustilan virtual laitteeseen asemat käyttävät todennusta CHAP. Yksityiskohtaisia ohjeita Siirry [Määrittäminen CHAP laitteeseesi](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Määritä salasana StorSimple tilannevedoksen hallinta

StorSimple tilannevedoksen hallinta ohjelmiston sijaitsee Windows-isännän ja avulla järjestelmänvalvojat voivat hallita varmuuskopioiden StorSimple laitteen paikallinen lomakkeessa ja cloud tilannevedoksia.

>[AZURE.NOTE] Windows-host on virtual laitteen Azure virtuaalikoneen.

Kun määrität laitteen StorSimple tilannevedoksen hallinta, voit pyydetään StorSimple laitteen IP-osoite ja salasana tallennustilan laitteen tarkistamiseen. Yksityiskohtaisia ohjeita Siirry [Määritä StorSimple tilannevedoksen Managerin salasana](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Laitteen järjestelmänvalvojan salasanan vaihtaminen 

Windows PowerShell-liittymän avulla voit käyttää virtual laite, kun olet tarvitaan laitteen järjestelmänvalvoja-salasana. Tietojen suojauksen sinun on tilapäinen salasana, ennen kuin virtual laitteen voi käyttää. Yksityiskohtaisia ohjeita Siirry [Määritä laitteen järjestelmänvalvojan salasanan](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-virtual-device"></a>Etäkäyttö virtual laitteeseen
Etäkäyttöpalvelimen Windows PowerShell-liittymän kautta virtual laitteeseesi ei ole käytössä oletusarvoisesti. Haluat ottaa etähallinnan virtual laitteeseen ja käyttöön asiakas, jota käytetään käyttämään virtual laitteen.

Etäkäyttö kaksivaiheinen prosessi on kuvattu alla.

### <a name="step-1-configure-remote-management"></a>Vaihe 1: Määritä etähallinta

Seuraavien toimien määrittäminen etähallinnan StorSimple virtual laitteeseesi.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Vaihe 2: Etäyhteyden virtual laite

Kun olet ottanut etähallinnan StorSimple laitteen määritys-sivulla, voit Windows PowerShellin Etäpalvelujen muodostaa virtual laitteeseen toisista virtual sisällä samassa virtual verkossa; Voit esimerkiksi muodostaa isännältä AM, joka on määritetty ja yhdistää iSCSI. Useimmat käyttöönotoissa olet jo avannut julkisen päätepisteen käyttämään isäntä AM, joita voit käyttää virtual laitetta.

>[AZURE.WARNING] **Tietoturvasyistä Suosittelemme, että käytät HTTPS päätepisteet yhdistettäessä ja poista päätepisteet, kun olet suorittanut PowerShell-etäistunnon.**

Noudata seuraavia ohjeita: [yhteyden muodostaminen etäyhteyden StorSimple laitteen](storsimple-remote-connect.md) Etäpalvelujen virtual laitteen määrittäminen.

## <a name="connect-directly-to-the-virtual-device"></a>Virtuaalinen laitteen yhdistäminen

Voit myös muodostaa yhteyden suoraan virtual laite. Jos haluat muodostaa yhteyden suoraan virtual laitteen toisesta tietokoneesta virtual verkon ulkopuolella tai Microsoft Azure-ympäristön ulkopuolella, haluat luoda uusia päätepisteet seuraavassa kuvatulla tavalla. 

Seuraavien toimien Luo julkinen päätepiste virtual laitteeseen.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

On suositeltavaa liittyä toisista virtual sisällä samassa virtual verkossa, koska käytäntö pienentää julkisen päätepisteet virtual verkossa määrä. Tätä tapaa käytettäessä yksinkertaisesti yhdistäminen virtuaalikoneen istunnon etätyöpöydän kautta ja määrittänyt, että virtual machine käytettäväksi tapaan kaikki muut paikalliseen verkkoon kirjauduttaessa Windows-asiakas. Sinun ei tarvitse liittää julkisen porttinumero, koska portti jo tunnetuksi.

## <a name="work-with-the-storsimple-virtual-device"></a>Toimi StorSimple virtual laitteen kanssa

Nyt kun olet luonut ja määrittänyt StorSimple virtual laitteen, olet valmis aloittamaan käyttämiseen. Voit käsitellä aseman säilöjen asemat ja varmuuskopion käytännöt virtual laitteessa samalla tavalla kuin laitteessa fyysisesti StorSimple. Ainoa ero on, että haluat Varmista, että valitset virtual laitteen laitteen luettelosta. Lisätietoja on vaiheittaiset ohjeet eri hallintatehtäviä virtual laitteen [hallittavan virtual laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md) .

Seuraavissa osissa käsitellään joitakin eroja kohtaat virtual laitetta käytettäessä.

### <a name="maintain-a-storsimple-virtual-device"></a>Ylläpidä StorSimple virtual laite

Koska vain ohjelmiston laitteen, ylläpito virtual laitteen on pienin ylläpito fyysinen laitteen verrattuna. Sinulla on seuraavat vaihtoehdot:

- **Saatavilla olevat ohjelmistopäivitykset** – voit näyttää päivämäärän, ohjelmisto on päivitetty, mitään ja Päivitä tilasanomat. Sivun alareunassa **Tarkista päivitykset** -painikkeen avulla voit suorittaa tarkistuksen manuaalisesti, jos haluat tarkistaa uudet päivitykset. Jos päivityksiä on saatavilla, valitse **Asenna päivitykset** . Koska virtual laitteessa on vain yksi käyttöliittymä, tämä tarkoittaa, että hakutuloksissa näkyy hieman palvelukatkos kun päivitykset on käytössä. Virtuaalinen laite Sammuta ja Käynnistä uudelleen (tarvittaessa) mahdollisesti päivitysten, jotka on julkaistu. Vaiheittaiset ohjeet Siirry [päivittää laitteen](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- **Tue paketin** – voit luoda ja ladata tuki-paketti, jotta Microsoft Support vianmääritys virtual laitteen kanssa. Siirry vaiheittaiset ohjeet, [Voit luoda ja hallita tuki-paketti](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Tallennustilan tilit virtual laitteen

Tallennustilan tilit luodaan käytettäväksi StorSimple hallintapalveluun, virtual laite ja fyysinen laite. Luodessasi tallennustilan tilit, suosittelemme, että käytät kutsumanimi aluetunnus varmistaa, että alueella ovat yhdenmukaisia koko kaikki järjestelmäosat. Virtual laitteen on tärkeää, että kaikkien osien on saman alue, jos haluat estää suorituskykyongelmia.

Vaiheittaiset ohjeet Siirry [tallennustilan tilin lisääminen](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>StorSimple virtual laitteen aktivoinnin poistaminen

Virtual laitteen aktivoinnin poistaminen poistaa AM ja luodaan, kun se on valmisteltu resurssit. Virtuaalinen laite on poistettu käytöstä, kun sitä ei voi palauttaa edelliseen tilaan. Ennen kuin olet poistanut virtual laite, varmista, että voit lopettaa tai poistaa asiakkaat ja isännöi, riippuvaisia.

Hallita virtual laitteen tuottaa seuraavat toimet:

- Virtuaalinen laite on poistettu.

- OS levyn ja luoda virtuaalisen laitteen tiedot levyjen poistetaan.

- Isännöityjen palvelun ja luotu valmistelun aikana virtual verkon säilyvät. Jos et käytä niitä, poista ne manuaalisesti.

- Cloud tilannevedoksia luotu virtual laitteen säilyvät.

Vaiheittaiset ohjeet, siirry [aktivointi ja poista StorSimple laitteen](storsimple-deactivate-and-delete-device.md).

Kun virtual laitteen näkyy poistettu StorSimple hallinta-sivulle, voit poistaa virtual laitteen laitteen luettelosta **laitteet** -sivulla.


### <a name="start-stop-and-restart-a-virtual-device"></a>Aloita, Lopeta ja Käynnistä uudelleen virtual laite
Toisin kuin StorSimple fyysinen laite ei ole power käytössä tai power käytöstä StorSimple virtual laitteessa push-painiketta. Kuitenkin ehkä tapahtumia tarvittaessa virtual laitteen käynnistettävä uudelleen. Jotkin päivitykset voi esimerkiksi edellyttää AM käynnistettävä Viimeistele Päivitysprosessista. Voit aloittaa helpoin tapa Lopeta ja Käynnistä virtual laite on käytettävä näennäiskoneiden Management Console.

Kun tarkastelet Management Console-virtual laitteen tila on **käynnissä** , koska se on käynnistetty oletusarvoisesti sen luomisen jälkeen. Voit aloittaa, lopettaa ja virtual machine uudelleen milloin tahansa.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Palauta tehdasasetukset

Jos päätät, että haluat aloittaa alusta virtual laitteen kanssa, aktivoinnin ja poista se ja luo sitten uusi. Kun fyysinen laite on palautettu, kuten uusi virtual laitteesi ei ole kaikki päivitykset on asennettu. sen vuoksi Varmista, että voit tarkistaa päivitykset, ennen kuin käytät sitä.


## <a name="fail-over-to-the-virtual-device"></a>Ei onnistu virtual laitteen kautta

Tietojen palauttaminen (DR) on keskeiset skenaariot StorSimple virtual laite on suunniteltu varten. Tässä skenaariossa fyysistä StorSimple laitetta tai koko palvelinkeskuksen ehkä ole käytettävissä. Lähettäjä voit palauttaa vaihtoehtoisen sijainnin toimintojen virtual laite. Aikana DR-asema säilöt lähde-laitteesta muuttaa omistajuutta ja siirretään virtual laitteeseen. DR edellytyksistä ovat että virtual laite on luotu ja määritetty, kaikki aseman säilöön asemat on määritetty offline ja aseman säilö on liitetty cloud tilannevedoksen.

>[AZURE.NOTE] 
>
> - Kun käytät virtual laite on toissijainen laite DR, ota huomioon, että 8010 on 30 TT: n vakio tallennustilan ja 8020 on 64 TT: n Premium-tallennustilan. Suurempi kapasiteetti 8020 virtual laite saattaa olla lisää kyseiseen DR tilanteeseen.
> - Ei tue tai Kloonaa käynnissä Update 2 päivitystä edeltävässä 1 käyttö laitteeseen laitteesta. Voit kuitenkin epäonnistua päälle laitteeseen, jossa Update 2 päivitys 1 (1.1 tai 1.2)-laitteeseen

Vaiheittaiset ohjeet Siirry [virtual laitteen automaattisesti](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Sammuta tai poista virtuaalikoneen laite

Jos aiemmin määritetty ja käyttää StorSimple, virtual laitteen haluat mutta nyt lopettaa kerry Laske ovat sen käyttöä varten, voit Sammuta virtual laite. Suljetaan virtual laite ei poista sen käyttöjärjestelmän tai tietojen levyjen tallennustilaan. Se estää kulujen kerry tilauksen, mutta tallennustilan kulujen OS ja tietoja-levyjen säilyvät.

Jos poistat tai Sammuta virtual laite, se näkyy **offline-tilana** StorSimple hallintapalvelu laitteet-sivulla. Voit poistaa käytöstä tai poista laitteen, jos haluat poistaa virtual laitteen luoma varmuuskopioita myös. Lisätietoja on artikkelissa [aktivointi ja poista StorSimple laite](storsimple-deactivate-and-delete-device.md).

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Internet-yhteyden vianmääritys 

Virtuaalinen laitteen luonnin aikana silloin ei ole Internet-yhteys luomisen vaihe epäonnistuu. Vianmääritys, jos virheen vuoksi Internet-yhteys, seuraavien toimien Azure perinteinen portaalissa:

1. Luo Azure Windows server 2012 virtual machine. Virtual tämän tietokoneen käytettävä samaa tallennustilan tiliä, VNet ja aliverkon, jolla virtual laitteen. Jos sinulla on jo olemassa Windows Server-isännän Azure saman tallennustilan tilin, Vnet ja aliverkon, voit käyttää sitä myös Internet-yhteyksien vianmääritys.
2. Lokitietojen edellisessä vaiheessa luotu virtuaalikoneen kyselyjä. 
3. Avaa komentorivi-ikkuna virtuaalikoneen sisällä (Win + R ja kirjoita `cmd`).
4. Suorita seuraavat cmd tulee näkyviin.

    `nslookup windows.net`

5. Jos `nslookup` epäonnistuu, Internet-yhteys epäonnistui estää virtual laitteen rekisteröinti StorSimple hallintapalveluun. 
6. Tee tarvittavat muutokset virtual verkosto ja varmista, että virtual laite on käyttää Azure sivustoissa, kuten "windows.net".

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue ohjeet [hallittavan virtual laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
 
- Tietoja palauttaminen [StorSimple aseman varmuuskopion joukosta](storsimple-restore-from-backup-set.md). 

