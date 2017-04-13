<properties 
   pageTitle="Aktivointi ja poista StorSimple-laite | Microsoft Azure"
   description="Tässä artikkelissa käsitellään StorSimple laitteen poistaminen palvelun käytöstä ensin se ja poistamalla sitten se."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Aktivointi ja poista StorSimple-laite

## <a name="overview"></a>Yleiskatsaus

Halutessasi voit ottaa StorSimple laitteen ulos palvelu (esimerkiksi, jos korvaaminen tai laitteen päivittäminen tai jos käytössäsi on enää StorSimple). Jos näin on, sinun on poistanut laite, ennen kuin voit poistaa sen. Hallita katkaisee laite ja vastaavan StorSimple hallintapalvelu välinen yhteys. Tässä opetusohjelmassa kerrotaan, miten voit poistaa StorSimple laitteen palvelun ensimmäisen käytöstä sen ja poistamalla sitten se. 

Kun olet poistanut laitteen, tietoihin, jotka on tallennettu paikallisesti laitteessa enää ole käytettävissä. Vain laitteella, joka on tallennettu pilvipalveluun liittyviä tietoja voi palauttaa.  

>[AZURE.WARNING] Käytöstä poistamisen on pysyvä toiminto, eikä niitä voi perua. Aktivointi on poistettu laitteesta ei voi rekisteröidä StorSimple hallinta-palvelussa, ellei se ensin palauttaa tehdas. 
>
>Tehdas Palauta prosessin poistaa kaikki tiedot, jotka on tallennettu paikallisesti laitteessasi. Tämän vuoksi on tärkeää ottaa kaikki tiedot cloud tilannevedoksen, ennen kuin olet poistanut laitteen. Näin voit palauttaa kaikki tiedot myöhemmässä vaiheessa.

Tässä opetusohjelmassa kerrotaan, miten voit:

- Laitteen aktivointi ja poista tiedot
- Poista aktivointi laite ja säilyttää tiedot

Se myös kerrotaan, miten käytöstä poistamisen ja StorSimple virtual laite toimii poisto.

>[AZURE.NOTE] Ennen kuin olet poistanut StorSimple fyysinen tai näennäinen laitteen, varmista, että voit lopettaa tai poistaa asiakkaat ja isännöi, jotka riippuvat laitteen.

## <a name="deactivate-and-delete-data"></a>Aktivoinnin ja tietojen poistaminen

Jos olet kiinnostunut laitteen poistaminen kokonaan ja et halua säilyttää tietoja laitteeseen, toimimalla seuraavasti.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Laitteen käytöstä ja poistaa tiedot  

1. Ennen hallita laitetta, sinun on poistettava kaikki äänenvoimakkuuden säilöjen (ja asemat) laitteeseen. Voit poistaa aseman säilöjen vain, kun olet poistanut liittyvät varmuuskopiot.

2. Poista laitteen käytöstä seuraavasti:

    1. StorSimple hallinta palvelun **laitteet** -sivulla Valitse laite, jotka haluat poistaa käytöstä, ja valitse sivun alareunassa **Poista aktivointi**.

    2. Näyttöön vahvistussanoma. Jatka valitsemalla **Kyllä** . Poista käytöstä-prosessi käynnistyy ja kestää muutaman minuuttia.

3. Kun käytöstä poistamisen, voit poistaa laitteen kokonaan. Laitteen poistaminen poistaa sen palvelun liitettyjen laitteiden luettelo. Palvelun sitten enää hallita poistettu laitteesta. Seuraavien vaiheiden avulla voit poistaa laitteen:

    1. Valitse StorSimple hallinta-palvelu‑sivulla **laitteiden** käytöstä laite, jonka haluat poistaa.

    2. Valitse sivun alareunassa Valitse **Poista**.

    3. Voit pyydetään vahvistamaan. Jatka valitsemalla **Kyllä** .

    Voi kestää muutaman minuutin kuluttua laitteen poistetaan.

## <a name="deactivate-and-retain-data"></a>Poista aktivointi ja säilyttää tiedot

Jos ovat kiinnostuneita poistaminen laitteen, mutta haluat säilyttää tiedot, toimimalla seuraavasti.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Laitteen käytöstä ja säilyttää tiedot 

1. Poista käytöstä laite. Äänenvoimakkuuden säilöjä ja laitteen tilannevedosten säilyvät.

    1. StorSimple hallinta palvelun **laitteet** -sivulla Valitse laite, jotka haluat poistaa käytöstä, ja valitse sivun alareunassa **Poista aktivointi**.

    2. Näyttöön vahvistussanoma. Jatka valitsemalla **Kyllä** . Poista käytöstä-prosessi käynnistyy ja kestää muutaman minuuttia.

2. Voit nyt onnistu aseman säilöjä ja liittyvän tilannevedoksia. Siirry ohjeet- [automaattisesti ja tietojen palauttaminen StorSimple laitteeseesi](storsimple-device-failover-disaster-recovery.md).

3. Kun käytöstä poistamisen ja automaattisesti, voit poistaa laitteen kokonaan. Laitteen poistaminen poistaa sen palvelun liitettyjen laitteiden luettelo. Palvelun sitten enää hallita poistettu laitteesta. Seuraavien vaiheiden laitteen poistaminen:
 
    1. Valitse StorSimple hallinta-palvelu‑sivulla **laitteiden** käytöstä laite, jonka haluat poistaa.

    2. Valitse sivun alareunassa Valitse **Poista**.

    3. Voit pyydetään vahvistamaan. Jatka valitsemalla **Kyllä** .

    Voi kestää muutaman minuutin kuluttua laitteen poistetaan.

## <a name="deactivate-and-delete-a-virtual-device"></a>Aktivointi ja poista virtuaalikoneen laite

StorSimple: laitteen virtual käytöstä poistamisen vapauttaa virtuaalikoneen. Voit poistaa virtuaalikoneen ja luoda, jos se on valmisteltu resurssit. Virtuaalinen laite on poistettu käytöstä, kun sitä ei voi palauttaa edelliseen tilaan. 

Käytöstä poistamisen tuloksena seuraavat toimet:

- StorSimple virtual laite on poistettu.

- OSDisk ja tietoja-levyjä, jotka on luotu StorSimple virtual laitteen poistetaan.

- Isännöidään palvelun ja Virtual verkkoon, joka on luotu valmistelun aikana säilyvät. Jos et käytä nämä objektit, poista ne manuaalisesti.

- Cloud tilannevedoksia StorSimple virtual laitteen luoma säilyvät.

## <a name="next-steps"></a>Seuraavat vaiheet
- Palauttaa aktivointi laitteeseen tehdasasetukset, siirry [Palauta oletusarvoiksi laitteen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- Saat teknistä tukea, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md).

- Lisätietoja käyttämisestä StorSimple hallintapalveluun, siirry [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten. 
