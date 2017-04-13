<properties
   pageTitle="Hallitse StorSimple laitteen ohjaimet | Microsoft Azure"
   description="Opettele lopettaminen, Käynnistä uudelleen, Sammuta tai palauta StorSimple laite-ohjaimet."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="manage-your-storsimple-device-controllers"></a>Hallitse StorSimple laite-ohjaimet

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kuvataan eri toiminnoista, joita voidaan suorittaa StorSimple laite-ohjaimet. Laitteen StorSimple ohjaimet ovat tarpeettomia (vertaisjärjestelmä) ohjaimet Aktiivinen-passiivinen-kokoonpanossa. Tietyn kerrallaan vain yhden ohjauskoneen on aktiivinen ja käsittelee kaikki levy ja verkko-toiminnot. Passiivinen tila on muita ohjauskoneen. Jos aktiivinen ohjauskoneen epäonnistuu, passiivinen ohjauskoneen aktivoituu automaattisesti.

Tässä opetusohjelmassa on vaiheittaiset ohjeet toteutetun laitteen ohjaimet:

- StorSimple hallintapalvelu **ylläpito** -sivun **ohjaimet** -osa
- Windows PowerShell StorSimple.

On suositeltavaa hallinta StorSimple hallintapalvelu kautta laitteen-ohjaimet. Jos toiminto voidaan suorittaa vain Windows PowerShellin avulla StorSimple-opetusohjelman tekee se muistiin.

Kun olet lukenut Tässä opetusohjelmassa, saa oikeuden:

- Käynnistä tai Sammuta StorSimple laite-ohjain
- Sulje StorSimple-laite
- Palauta StorSimple laitteen tehdasasetukset


## <a name="restart-or-shut-down-a-single-controller"></a>Käynnistä tai yksittäisen ohjauskoneen Sammuta

Ohjaimen uudelleenkäynnistämisen tai sammuttamisen ei tarvita osana Normaali järjestelmän toimintaa. Yksittäisen laitteen ohjaimen Sammuta toiminnot ovat yleisiä ainoastaan tapauksissa, jossa epäonnistui laitteen laitteisto-osan vaatii korvaavan. Ohjaimen uudelleen saatetaan pyytää tilanteessa, jossa suorituskykyyn vaikuttaa liiallinen muistinkäyttö- tai valvonta on toimintahäiriö. Voit joutua myös käynnistämään valvonta on onnistunut ohjauskoneen, korvaa jälkeen, jos haluat ottaa käyttöön ja testata korvattu seuraavasti.

Uudelleenkäynnistys laite ei ole häiritä yhdistetyn laitteistokäynnistäjät, jos passiivinen ohjain on käytettävissä. Jos passiivinen ohjain ei ole käytettävissä tai poistettu käytöstä ja sitten uudelleen aktiivisen ohjauskoneen saattaa johtaa käyttökatkot ja häiriöt.

> [AZURE.IMPORTANT]

> - **Valvonta on käytössä olisi koskaan fyysisesti poistetaan, kun tämä johtaa tappio arvojen ja parantavat riskiä käyttökatkot.**

> - Seuraavat toimintaohjeet koskevat vain StorSimple fyysinen laite. Saat tietoja siitä, miten voit aloittaa tai lopettaa virtual laitteen uudelleen, [virtual laitteen käsitteleminen](storsimple-virtual-device-u2.md#work-with-the-storsimple-virtual-device).

Voit käynnistää uudelleen tai Sammuta yhteen laitteeseen ohjauskoneen käyttämällä StorSimple hallintapalvelu-tai Windows PowerShellin Azure perinteinen portaalin StorSimple

Seuraavien toimien hallittavan laitteen ohjaimet perinteinen Azure-portaalista.

#### <a name="to-restart-or-shut-down-a-controller-in-classic-portal"></a>Käynnistä tai Sammuta ohjauskoneen perinteinen-portaalissa

1. Siirry **laitteiden > ylläpito**.

1. Siirry **Laitteiston tila** ja varmista, että molemmat ohjaimet laitteen tila on **kunnossa**.

    ![Tarkista kunnossa StorSimple laitteen ohjaimet](./media/storsimple-manage-device-controller/IC766017.png)

1. Napsauta **Hallitse ohjaimet** **ylläpito** -sivun alareunaan.

    ![Hallitse StorSimple laitteen ohjaimet](./media/storsimple-manage-device-controller/IC766018.png)</br>

    >[AZURE.NOTE] Jos et näe **Hallinta ohjaimet**, sinun on päivityksiä asennettaessa. Lisätietoja on artikkelissa [päivittää StorSimple laitteen](storsimple-update-device.md).

1. **Valvonta-asetusten muuttaminen** -valintaikkunassa seuraavasti:
    1. Valitse avattavasta **Select Controller** ohjaimen, jonka haluat hallita. Vaihtoehdot ovat Controller 0 ja 1 ohjauskoneen. Nämä ohjaimet tunnistetaan myös aktiivisen tai passiivisen.

        >[AZURE.NOTE] Ohjain ei voi hallita, jos se ei ole käytettävissä tai poistettu käytöstä, ja se näkyy ei avattavasta luettelosta.

    2. **Toiminnon valitseminen** avattavasta luettelosta Valitse **Käynnistä ohjaimen** tai **ohjauskoneen Sammuta**.

        ![Käynnistä StorSimple laitteen passiivinen-ohjain](./media/storsimple-manage-device-controller/IC766020.png)
    3. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-manage-device-controller/IC740895.png).

Tämä käynnistää uudelleen tai ohjaimen Sammuta. Seuraavassa taulukossa on yhteenveto tietoja siitä, mitä tapahtuu **Valvonta-asetusten muuttaminen** -valintaikkunassa tekemiesi valintojen mukaan.  


|Valinta #|Jos...|Tämä tapahtuu.|
|---|---|---|
|1.|Passiivinen ohjauskoneen uudelleen.|Työn luodaan käynnistämään ohjaimen ja saat ilmoituksen, kun työ on luotu. Tämä aloittaa ohjauskoneen uudelleen. Voit valvoa uudelleenkäynnistyksen prosessin käyttäminen **palvelun > raporttinäkymät > tarkastella toimintojen lokit** ja sitten suodattaminen tietyn palvelun parametrit.|
|2.|Käynnistä uudelleen aktiivisen ohjauskoneen.|Näyttöön tulee seuraava varoitus: "Jos käynnistät aktiivinen ohjauskoneen, laite epäonnistuu päälle passiivinen valvojalle. Haluat jatkaa?" </br>Jos haluat jatkaa tätä toimintoa, seuraavat vaiheet ovat samat käytettyjä käynnistämään passiivinen ohjauskoneen (Katso valintaa 1).|
|3.|Passiivinen ohjauskoneen Sammuta.|Näyttöön tulee seuraava sanoma: "Sammuta päätyttyä, tarvitset push power-painike opaspainiketta, voit ottaa sen käyttöön. Oletko varma, että haluat sulkea tämän ohjauskoneen?" </br>Jos haluat jatkaa tätä toimintoa, seuraavat vaiheet ovat samat käytettyjä käynnistämään passiivinen ohjauskoneen (Katso valintaa 1).|
|4.|Sulje aktiivinen ohjauskoneen.|Näyttöön tulee seuraava sanoma: "Sammuta päätyttyä, tarvitset push power-painike opaspainiketta, voit ottaa sen käyttöön. Oletko varma, että haluat sulkea tämän ohjauskoneen?" </br>Jos haluat jatkaa tätä toimintoa, seuraavat vaiheet ovat samat käytettyjä käynnistämään passiivinen ohjauskoneen (Katso valintaa 1).|


#### <a name="to-restart-or-shut-down-a-controller-in-windows-powershell-for-storsimple"></a>Käynnistä tai suljetaan StorSimple Windows PowerShell-ohjain
Seuraavien toimien Sammuta tai yksittäisen ohjauskoneen perinteinen Azure-portaalista StorSimple laitteen uudelleen.


1. Käyttää laitteen serial konsolin tai etätietokoneista telnet-istunnossa. [Käytä painovärit, muste muodostaa laitteen serial konsolin](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)noudattamalla yhdistäminen Controller 0 tai ohjauskoneen 1.

1. Valitse sarja konsoli-valikon vaihtoehto 1, **Kirjaudu sisään täydet oikeudet**.

1. Ilmoituspalkin viesti, merkitse muistiin ohjauskoneen, yhteys on muodostettu (Controller 0 tai ohjauskoneen, 1) ja onko aktiivisen tai passiivisen (valmiustilassa)-ohjain.
    - Sulkeutumaan yhden ohjauskoneen, tulee näkyviin, kirjoita:

        `Stop-HcsController`

        Tämä suljetaan ohjauskoneen, yhteys on muodostettu. Jos lopetat aktiivinen ohjauskoneen, valitse se epäonnistuu päälle passiivinen valvojalle ennen kuin se suljetaan.
    - Käynnistämään ohjauskoneen, tulee näkyviin, kirjoita:

        `Restart-HcsController`

        Tämä käynnistää ohjauskoneen, yhteys on muodostettu. Jos käynnistät aktiivinen ohjauskoneen, se epäonnistuu passiivinen valvojalle ennen uudelleenkäynnistyksen päälle.


## <a name="shut-down-a-storsimple-device"></a>Sammuta StorSimple-laite

Tässä osassa kerrotaan, miten sulkeutumaan käynnissä tai etätietokoneista epäonnistui StorSimple laite. Laite on poistettu käytöstä laite-ohjaimet sammuttamisen jälkeen. Laitteen sulkeminen on valmis, kun laite fyysisesti siirretään tai palvelun otetaan.

> [AZURE.IMPORTANT] Ennen kuin suljet laitteen, laitteen osat kuntotietojen tarkistus. Siirry **laitteiden > ylläpito > laitteiston tila** ja varmista, että kaikki komponentit LED tilan vihreä. Kunnossa laite on vihreä tila. Jos laite on parhaillaan Sammuta toimintahäiriö osan, näet epäonnistui (punainen) tai vastaaviin osat eivät toimi oikein (keltainen) tila.

#### <a name="to-shut-down-a-storsimple-device"></a>Sammuta StorSimple-laite

1. [Käynnistä tai valvonta on Sammuta](#restart-or-shut-down-a-single-controller) -toiminnon avulla voit tunnistaa ja laitteen passiivinen ohjauskoneen Sammuta. Voit suorittaa tämän toiminnon Azure perinteinen-portaalista tai Windows PowerShellin StorSimple varten.
2. Toista edellinen vaihe aktiivinen ohjauskoneen sulkeutumaan.
3. Sinun on nyt tarkastella laitteen takaisin suuntaisesti. Kun kahden ohjauskoneen ovat täysin Sammuta, molemmat ohjaimet tilan merkkivalot olisi vilkkuvaa punainen. Jos haluat poistaa käytöstä laitteen kokonaan tällä hetkellä, käännä power valitsimet Power ja jäähdytys moduulit (PCMs) asentoon ei käytössä. Tämä kannattaa poistaa käytöstä laite.


<!--#### To shut down a StorSimple device in Windows PowerShell for StorSimple

1. Connect to the serial console of the StorSimple device by following the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-serial-console).

1. In the serial console menu, verify from the banner message that the controller you are connected to is the passive controller. If you are connected to the active controller, disconnect from this controller and connect to the other controller.

1. In the serial console menu, choose option 1, **log in with full access**.

1. At the prompt, type:

    `Stop-HCSController`

    This should shut down the current controller. To verify whether the shutdown has finished, check the back of the device. The controller status LED should be solid red.

1. Repeat steps 1 through 4 to connect to the active controller and then shut it down.

1. After both the controllers are completely shut down, the status LEDs on both should be blinking red. If you need to turn off the device completely at this time, flip the power switches on both Power and Cooling Modules (PCMs) to the OFF position.-->

## <a name="reset-the-device-to-factory-default-settings"></a>Laitteen factory oletusasetusten palauttaminen

> [AZURE.IMPORTANT] Jos haluat palauttaa laitteen oletusarvoiksi, ota yhteyttä Microsoft Support. Alla kuvattua käytetään vain yhdessä Microsoft Support.

Tässä kuvataan palauttaminen Microsoft Azure StorSimple laitteesi Windows PowerShellin avulla StorSimple asetuksia.
Laitteen nollaaminen poistaa koko klusterin oletusarvon mukaan kaikki tiedot ja asetukset.

Seuraavien toimien vaihtamaan Microsoft Azure StorSimple laitteen oletusarvoiksi:

### <a name="to-reset-the-device-to-default-settings-in-windows-powershell-for-storsimple"></a>Voit palauttaa laitteen oletusasetusten Windows PowerShellin StorSimple

1. Käyttää laitteen sen serial konsolissa. Tarkista varmistaa, että olet muodostanut yhteyden aktiivisen ohjauskoneen nauha-viesti.

1. Valitse sarja konsoli-valikon vaihtoehto 1, **Kirjaudu sisään täydet oikeudet**.

1. Tulee näkyviin Kirjoita seuraava komento vaihtamaan koko klusterin, poistetaan kaikki tiedot ja metatiedot ohjauskoneen asetukset:

    `Reset-HcsFactoryDefault`

    Voit tehdä sen sijaan palauttaa yksittäisen ohjauskoneen, [Palauta HcsFactoryDefault](http://technet.microsoft.com/library/dn688132.aspx) cmdlet-komento, jossa `-scope` parametrin.)

    Järjestelmä käynnistetään useita kertoja. Saat ilmoituksen, kun palauttaminen onnistui. Järjestelmän mallista riippuen se voi viedä 45 – 60 minuutin 8100 laitteen ja viimeistele Tämä toimenpide 8600 60 90 minuuttia.

    > [AZURE.TIP]

    > - Jos käytät päivityksen 1.2 tai käyttää sitä vanhemmissa versioissa `–SkipFirmwareVersionCheck` parametri ohittaa laitteisto-version tarkistusta (muussa näet laitteisto ristiriita-virheen: Factory Palauta ei voi jatkaa vuoksi ristiriita laitteisto-versioissa. ).

    > - Factory Palauta menettelyä voi epäonnistua StorSimple laitteille, jotka käyttävät päivitys 1 tai 1.1 Government-portaalissa ja olet suorittanut onnistuneen yhden tai kahden ohjauskoneen korvaa (ja korvaava-ohjaimet päivitystä edeltävässä 1 ohjelmiston mukana toimitettua). Tämä tapahtuu, kun tehdas Palauta kuva tarkistetaan päivitystä edeltävässä 1 ohjelmiston ohjaimen, jota ei ole tiedoston SHA1 esiintymistä. Jos näet tämän factory palauttaa virheen, ota yhteyttä Microsoftin Support auttamaan seuraaviin vaiheisiin. Tämä ongelma ei nähdä korvaava-ohjaimet, jotka toimitettiin-factory päivitys 1 tai uudempi ohjelmiston kanssa.


## <a name="questions-and-answers-about-managing-device-controllers"></a>Kysymysten ja vastausten hallinta laitteen ohjaimet

Tässä osassa on tehdä yhteenvedon joihinkin usein kysyttyihin kysymyksiin, jotka koskevat hallinta StorSimple laitteen ohjaimet.

**Q.** Mitä tapahtuu, jos molemmat ohjaimet laitteessa on kunnossa ja otettu käyttöön ja voin uudelleen tai Sulje aktiivinen ohjauskoneen?

**A.** Ovatko molemmat ohjaimet laitteen kunnossa ja otettu käyttöön, sinua pyydetään vahvistamaan. Voit halutessasi:

- **Käynnistä active ohjauskoneen** – saat ilmoituksen, että aktiivinen ohjauskoneen uudelleenkäynnistyksen aiheuttaa laite, jota ei onnistu passiivinen ohjauskoneen päälle. Ohjaimen käynnistetään uudelleen.

- **Sulje aktiivinen ohjauskoneen** – saat ilmoituksen, että aktiivinen ohjauskoneen suljetaan johtaa käyttökatkot. Sinun on myös painaa laitteeseen ohjaimen ottamaan power-painiketta.

**Q.** Mitä tapahtuu, jos laitteessa passiivinen ohjain ei ole käytettävissä tai poistettu käytöstä ja voin uudelleen tai Sammuta aktiivinen ohjauskoneen?

**A.** Passiivinen ohjauskoneen laitteen ollessa poissa käytöstä tai poistettu käytöstä ja päätät:

- **Käynnistä active ohjauskoneen** – saat ilmoituksen, että jatkat toiminnon aiheuttaa palvelun tilapäinen häiriöitä ja sinua pyydetään vahvistamaan.

- **Sulje aktiivinen ohjauskoneen** – saat ilmoituksen, että toiminto jatkuvaa johtaa käyttökatkot ja on push power-painike toinen tai molemmat ohjauskoneen laitteen käyttöön. Voit pyydetään vahvistamaan.

**Q.** Kun ei ohjauskoneen uudelleenkäynnistämisen tai sammuttamisen edistymisen ei onnistu?

**A.** Uudelleenkäynnistys tai ohjaimen suljetaan saattaa epäonnistua, jos:

- Laitteen päivitys on käynnissä.

- Ohjaimen uudelleen on jo käynnissä.

- Ohjauskoneen sammuttamisen on jo käynnissä.

**Q.** Kuinka voit selvittää, jos valvonta on käynnistettävä uudelleen tai Sammuta?

**A.** Voit tarkistaa ohjauskoneen ylläpito-sivun tila. Ohjaimen tila ilmoittaa, onko valvonta on käynnistettävä uudelleen tai sulje. Lisäksi ilmoitukset-sivulle tulee tiedoksi ilmoituksen, jos on käynnistettävä uudelleen tai Sammuta. Ohjaimen uudelleen ja Sammuta toimintojen tallennetaan myös toimintojen lokit. Lisätietoja toimintojen lokit Siirry [Näytä toiminnon lokit](storsimple-service-dashboard.md#view-the-operations-logs).

**Q.** Onko käytettävissä minkä tahansa vaikuttavia tekijöitä i tuloksena ohjauskoneen automaattisesti?

**A.** Laitteistokäynnistäjät ja aktiivinen ohjauskoneen välillä TCP-yhteydet palautetaan tuloksena ohjauskoneen automaattisesti, mutta voi muodostaa uudelleen kun passiivinen ohjauskoneen oletetaan, että toiminto. Tämän toiminnon aikana saattaa olla tilapäinen (alle 30 sekuntia) keskeyttää i/o-toiminnossa laitteistokäynnistäjät ja laitteen välillä.

**Q.** Miten oma ohjauskoneen palvelun sen jälkeen, kun se on suljettava ja poistaa palauttaa?

**A.** Voit palata valvonta on palvelu, se on lisääminen alustan kuvatulla tavalla [Korvaa StorSimple laitteen ohjain moduuli](storsimple-controller-replacement.md).

## <a name="next-steps"></a>Seuraavat vaiheet

- Jos kohtaat ongelmia ja että StorSimple laitteen ohjaimet, jotka et voi ratkaista luetellut Tässä opetusohjelmassa, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md)ohjeiden mukaisesti.

- Lisätietoja käyttämällä StorSimple hallinta-palvelua, tutustu [StorSimple hallintapalvelu ammattimainen StorSimple laitteen](storsimple-manager-service-administration.md).
