<properties
   pageTitle="StorSimple Virtual matriisin tietojen palauttaminen ja laitteen automaattisesti"
   description="Lue lisää siitä, kuinka voit siirtää StorSimple Virtual matriisin."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>StorSimple Virtual matriisin tietojen palauttaminen ja laitteen automaattisesti


## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan palauttaminen Microsoft Azure StorSimple Virtual matriisin (tunnetaan myös nimellä StorSimple paikallisen virtual laite), mukaan lukien pakollinen epäonnistuu päälle virtual toiseen laitteeseen, jos huono seuraavasti. Voit siirtää tietoja *lähde* -laitteesta joten *kohde* laitetta, joka sijaitsee samassa tai eri maantieteellisen sijainnin automaattisesti. Laitteen vikasietotilaa on koko laite. Aikana automaattisesti lähde-laitteen cloud-tiedot muuttuvat omistajuus, Kohdelaite.

Laitteen automaattisesti on koordinoitu tietojen palautustoiminto (DR) kautta ja **laitteet** -sivu on käynnistetty. Tällä sivulla tabulates kaikki yhdistetty StorSimple hallintapalveluun StorSimple laitteet. Pienoisohjelman näkyvät kutsumanimi, tila, valmisteltu ja suurin, tyyppi ja malli.

![](./media/storsimple-ova-failover-dr/image15.png)

Tässä artikkelissa on StorSimple Virtual matriisin koskevat vain. Siirry epäonnistumisen päälle 8000 sarjan-laitteella [automaattisesti ja palauttaminen StorSimple laitteen](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Mikä on palauttaminen?

Tietojen palauttaminen (DR)-skenaario ensisijainen laite enää toimi. Tässä tapauksessa voit siirtää toiseen laitteeseen epäonnistui laitteen liittyviä käyttämällä ensisijainen laite *lähde* ja määrittämällä toisen laitteen kuin *kohde*cloud tietoja. Tätä prosessia kutsutaan *automaattisesti*. Aikana automaattisesti kaikki asemat tai lähde-laitteesta osakkeet muuttaa omistajuutta ja kohdelaitteen siirretään. Ei suodatusta tiedot on sallittu.

DR on mallintaa koko laitteen palauttaminen käyttämällä lämpökartan karttapohjaisten – tiering ja seuranta. Lämpökartan on määritetty määrittämällä lämpökartan arvon tietoja edelleen lukeminen ja kirjoittaminen kuviot. Tämä lämpökartan Yhdistä sitten tasoa alin lämpökartan joukkojen pilveen ensin ja säilyttää high (käytetyt) lämpökartan joukkojen paikallinen ylätasolla. Aikana DR lämpökartta käytetään tiedostoa ja lisätä vettä ennen nauttimista pilveen tiedot. Laitteen hakee kaikki asemat/osakkeet edellisen viimeisimmät varmuuskopioinnin (kuten määräytyy sisäisesti) ja suorittaa palautuksen kyseisen varmuuskopiosta. Koko DR prosessin on koordinoitu laite.


## <a name="prerequisites-for-device-failover"></a>Laitteen automaattisesti edellytyksistä


### <a name="prerequisites"></a>Edellytykset

Laitteen kaikki automaattisesti, on täytettävä seuraavat vaatimukset:

- Lähdelaite on oltava **aktivointi poistetaan** -tilaan.

- Kohdelaite on Näytä **aktiiviseksi** Azure perinteinen portaalin. Sinun on valmistelu kohde virtual laitteen kapasiteetista saman tai uudempi versio. Paikallisen sivuston Käyttöliittymä sitten käytettävä ja rekisteröi onnistuneesti virtual laite.

    > [AZURE.IMPORTANT] Ei yrittää määrittää palvelun kautta virtual rekisteröidyn laitteen valitsemalla **laitteen täydellinen asennus**. Ei ole laitemääritys on tehtävä-palvelun kautta.

- Lähde- ja laite on oltava samaa tyyppiä. Vain voi epäonnistua määritetty tiedostopalvelimessa toiseen tiedostopalvelimeen virtual laitteen päälle. Sama koskee iSCSI-palvelimeen.

- Tiedostopalvelimeen DR Suosittelemme Liitä kohdelaitteen samaa toimialuetta, lähteen niin, että Jaa käyttöoikeudet ovat automaattisesti ratkennut. Tässä versiossa tuetaan vain keskeneräiset samaa toimialuetta kohde laitteeseen.

### <a name="other-considerations"></a>Muita huomioon otettavia seikkoja

- On suositeltavaa, että tallentumaan asemat tai osakkeet lähde laitteeseen offline-tilassa.

- Jos se on suunniteltu automaattisesti, suosittelemme, että laitteen Varmuuskopioi ja jatka sitten vikasietotilaa Pienennä tietojen menettämisen. Suunnittelematon automaattisesti on uusin varmuuskopio käytetään palauttaa laitteeseen.

- DR käytettävissä kohde-laitteita ovat laitteet, joissa on sama tai suurempi kapasiteetti lähde-laitteen verrattuna. Laitteet, jotka on yhdistetty palveluun, mutta eivät täytä ehtoja tarpeeksi tilaa eivät ole käytettävissä kohde laitteet.

### <a name="dr-prechecks"></a>DR prechecks

Ennen kuin DR alkaa prechecks suoritetaan laitteeseen. Tarkistukset auttaa varmistamaan, että virheitä ilmenee, kun DR aloittamista. Prechecks ovat seuraavat:

- Tallennustilan tilin vahvistaminen

- Tarkistuksen Azure cloud yhteys

- Kohde-laitteessa käytettävissä olevan tilan tarkistaminen

- Tarkistaminen Jos iSCSI palvelimen lähde-laite on kelvollinen ACR nimet, IQN (enintään 220 merkkiä) ja salasana CHAP (12 – 16 merkkiä) liittyvät asemat

Jos jokin edellä prechecks epäonnistuu, ei voi jatkaa DR. Haluat ratkaista kyseiset ongelmat ja yritä DR.

Kun DR on suoritettu onnistuneesti, cloud tietoja lähde-laitteen omistajuus siirretään Kohdelaite. Valitse Lähdelaite ei ole enää käytettävissä portaalissa. Kaikki asemat/jaettuja lähde-laitteessa on estetty ja kohdelaitteen aktivoituu.

> [AZURE.IMPORTANT]
> 
> Laite ei ole enää käytettävissä, mutta virtuaalikoneen, valmisteltu Host (isäntä)-järjestelmässä on edelleen käyttö resurssit. Kun DR on suoritettu onnistuneesti, voit poistaa virtual tämän tietokoneen host-järjestelmästä.

## <a name="fail-over-to-a-virtual-array"></a>Ei onnistu virtual matriisin päälle

On suositeltavaa, että sinulla on toisen StorSimple Virtual taulukon valmistelun yhteydessä, määritetty paikallisen sivuston Käyttöliittymä ja StorSimple hallintapalvelu ennen tämän toimintosarjan rekisteröityjä.


> [AZURE.IMPORTANT]
> 
> - Et voi epäonnistua päälle 1200 virtual laitteen StorSimple 8000 sarjan laitteesta.
> - Voi epäonnistua päälle FIPS Federal Information Processing Standard () käyttöön virtual laitteesta, Government-portaalissa virtual laitteeseen Azure perinteinen portaalissa käyttöön. Vastaavasti on myös tosi.

Seuraavien toimien laitteen palautettava kohde StorSimple virtual laite.

1. Ota isännän asemat/osakkeet offline-tilassa. Tutustu käyttöjärjestelmän – koskevien ohjeiden isännän asemat/osakkeet offline-tilaan. Jos jo offline sinun on otettava kaikki asemat/osakkeet offline-tilassa laitteeseen valitsemalla **laitteiden > osakkeet** (tai **laitteen > tietomääristä**). Valitse Jaa/asema ja valitse sivun alareunan **offline-tilaan** . Kun ohjelma pyytää vahvistusta, valitse **Kyllä**. Tämä toimenpide on toistettava kaikki osakkeet/asemat laitteeseen.

2. Valitse **laitteet** -sivulla Valitse lähde laitteen automaattisesti ja valitse **Poista aktivointi**. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. Voit pyydetään vahvistamaan. Laitteen käytöstä poistamisen poistetaan pysyvästi tavalla, joita ei voi kumota. Näyttöön tulee virheilmoitus myös tulevat isännän jälkeen osakkeet/asemat offline-tilassa.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Vahvistus-yhteydessä käytöstä poistamisen alkaa. Poista toiminnot käytöstä on suoritettu onnistuneesti, kun saat tiedon.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Valitse **laitteet** -sivulla laitteen tila muuttuu nyt **aktivointi poistetaan**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Valitse käytöstä laite ja valitse sivun alareunassa **automaattisesti**.

6. Vahvista automaattisesti ohjatun, joka avaa toimi seuraavasti:

    1. Valitse avattavasta luettelosta laitteista, **kohdelaitteen.** Laitteet, joissa on riittävän näkyvät avattavasta luettelosta.

    2. Tarkista tiedot, kuten laitteen nimi, kokonaiskapasiteetti ja jaettuja resursseja, jotka epäonnistuu päälle nimet lähde-laitteeseen.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. Tarkista **voin hyväksyy automaattisesti on pysyvä toiminto, ja kun vikasietotilaa on suoritettu onnistuneesti, Lähdelaite poistetaan**.

8. Valitse tarkistuksen kuvake ![](./media/storsimple-ova-failover-dr/image1.png).


9. Aloitetaan automaattisesti työn ja saat tiedon. Valitse **Näytä työn** seurannassa vikasietotilaa.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. **Työt** -sivu tulee näkyviin lähde-laitteen ohjelma luo automaattisesti projektin. Työn suorittaa DR prechecks.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Kun DR prechecks onnistuvat, automaattisesti työn spawn palauttaminen työt jokaisen Jaa/aseman, joka on lähde-laitteessasi.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. Kun vikasietotilaa on valmis, siirry **laitteet** -sivulle.

    a. Valitse StorSimple virtual laite, jota käytettiin automaattisesti prosessin kohde-laite.

    b. Siirry **osakkeet** -sivulle (tai **asemat** Jos iSCSI server). Kaikki vanhat laitteesta osakkeet (määrät) nyt pitäisi luetteloitu.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**Video käytettävissä**

Tässä videossa esitellään, miten voi epäonnistua StorSimple laitteen paikallisen virtual toiseen laitteeseen virtual päälle.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Liiketoiminnan jatkuvuus palauttaminen (BCDR)

Liiketoiminnan jatkuvuus tietojen palauttaminen (BCDR) tilanne ilmenee, kun koko Azure palvelinkeskuksen lakkaa toimimasta. Tämä voi vaikuttaa StorSimple hallintapalvelu ja liittyvät StorSimple laitteet.

Jos määritettynä on StorSimple-laitteet, joka on rekisteröity, ennen kuin huono tapahtui, StorSimple seuraaviin laitteisiin ehkä voi poistaa. Kun tietojen, Luo ja määrittää kyseiset laitteet.

## <a name="errors-during-dr"></a>Virheet DR aikana

**Cloud connectivity käyttökatkosta DR aikana**

Jos cloud yhteydet keskeytyy, kun DR on jo alkanut ja ennen laitteen palautus on valmis, DR epäonnistuu, ja saat tiedon. Kohdelaite, jota käytettiin DR sitten merkitty *käyttökelvoton.* Sama Kohdelaite ei voi käyttää sitten tulevien DRs.

**Ei ole yhteensopiva kohde-laitteet**

Jos käytettävissä kohde-laitteiden ei ole tarpeeksi tilaa, näet virheen tehosteen, ettei tietoalueessa ei ole yhteensopiva kohde-laitteita.

**Precheck virheet**

Jos jokin prechecks ei täyty, sinun tulee näkyviin precheck virheet.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja ammattimainen [Käyttäminen paikallisen sivuston Käyttöliittymä StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).
