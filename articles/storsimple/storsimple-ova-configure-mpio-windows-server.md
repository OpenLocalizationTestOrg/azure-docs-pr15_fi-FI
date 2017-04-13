<properties 
   pageTitle="Määritä MPIO StorSimple virtual matriisi-isännän | Microsoft Azure"
   description="Tässä artikkelissa käsitellään monipolku i/o (MPIO) määrittäminen oman StorSimple virtual matriisin yhteydessä käytössä Windows Server 2012 R2 isäntä."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Monipolku i/o määrittämistä Windows Server host StorSimple Virtual taulukolle

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan, miten voit asentaa Windows Server-isännän monipolku i/o-ominaisuuden (MPIO), käytä vain StorSimple tietomääristä tietyt asetukset ja tarkista levyasemien StorSimple MPIO. Oletetaan, että StorSimple 1200 Virtual matriisin kaksi verkon liittymät muodostanut yhteyden Windows Server-isäntä, jossa kaksi verkon liittymää. Tässä artikkelissa tietoja koskee vain virtual matriisi. Lisätietoja StorSimple 8000 sarjan laitteissa Siirry [Määrittäminen MPIO StorSimple isännän](storsimple-configure-mpio-windows-server.md). 

Windows Server auttaa muodosta erittäin käytettävissä, vikasietoinen tallennustilan käyttömahdollisuudet MPIO-toiminnon. MPIO käyttää tarpeettomat fyysinen polku-komponentteja, ohjaimet, kaapeli ja valitsimet – voit luoda loogisen polut palvelimeen ja tallennustilaa laitteen välillä. Jos osa virheen vuoksi looginen polku epäonnistuu, Monipolkujen käyttö logiikan käyttää vaihtoehtoisen polun i/o niin, että sovellukset voivat edelleen tietoihinsa. Lisäksi määritysten mukaan MPIO voi parantaa suorituskykyä myös tasaamisen uudelleen Lataa kaikki polut yli. Lisätietoja on artikkelissa [MPIO yleiskatsaus](https://technet.microsoft.com/library/cc725907.aspx "MPIO yleiskatsaus ja ominaisuuksia").  

StorSimple ratkaisu suuri-käytettävyyden voit määrittää MPIO Windows Server-isäntien yhteydessä StorSimple 1200 Virtual matriisin (tunnetaan myös nimellä paikallisen virtual laite). Host-palvelimissa hyväksyt sitten linkin, verkossa tai LIP-virheen. 

Tarvitset voit määrittää MPIO seuraavasti: 

- Määritysten edellytykset

- Vaihe 1: Asenna Windows Server-isännän MPIO

- Vaihe 2: Määritä MPIO StorSimple levyasemien

- Vaihe 3: Ota StorSimple tietomääristä isännän

Kunkin edellä kuvatut toimet on kuvattu seuraavissa osissa.


## <a name="prerequisites"></a>Edellytykset

Tässä osassa kerrotaan Windows Server host ja virtual matriisin määritysten edellytyksistä.

### <a name="on-windows-server-host"></a>Windows Server isännän

-  Varmista, että Windows Server host on 2 verkkoliittymät, jotka on otettu käyttöön.


### <a name="on-storsimple-virtual-array"></a>Valitse StorSimple virtual matriisi

- Virtuaalinen matriisissa on määritettävä iSCSI palvelimeksi. Lisätietoja on artikkelissa [virtual matriisin iSCSI-palvelimen määrittäminen](storsimple-ova-deploy3-iscsi-setup.md). Yhden tai useamman verkkoliittymät tulisi olla käytössä olevan matriisi.   

- Virtuaalinen matriisin verkkoliittymät pitäisi olla käytettävissä Windows Server-isännästä.

- Yhden tai useamman tietomääristä luodaan StorSimple Virtual matriisin. Lisätietoja on artikkelissa [Lisää aseman](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) StorSimple 1200 virtual matriisin. Prosessin myöhemmässä vaiheessa luomaamme virtual matriisi-3 asemat (2 paikallisesti kiinnitetty ja 1 Porrastettu aseman kuten alla).
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Laitteistomääritykset StorSimple virtual matriisin

Alla olevassa kuvassa näkyy suuren laitekokoonpanon ja kuormituksen tasaamisen Monipolkujen käyttö Windows Server host ja StorSimple virtual matriisi, jota käytetään prosessin myöhemmässä vaiheessa.  

![MPIO laitteiston määritykset](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

Kuten edellä olevassa kuvassa:

- Valmistella Hyper-V StorSimple virtual matriisin on määritetty iSCSI-palvelimeen yhden solmun aktiivinen laite.

- Kaksi VPN-liittymää on otettu käyttöön matriisin. Paikallisen sivuston 1200 virtual matriisin Käyttöliittymän Varmista, että kaksi verkon liittymää ovat käytössä siirtymällä **Verkkoasetukset** alla kuvatulla tavalla:

    ![Verkkoliittymät 1200 käytössä](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Huomaa, että käytössä verkkoliittymät (Ethernet-oletusarvoisesti Ethernet-2) IPv4-osoitteet ja Tallenna isännän myöhempää käyttöä varten.

- Kaksi verkon liittymää on käytössä Windows Server host. Jos yhdistetyt liittymät host ja matriisi on saman aliverkon, on 4 polut käytettävissä. Tämä on sen jälkeen tapausta prosessin myöhemmässä vaiheessa. Kuitenkin Jos matriisissa ja Host (isäntä)-liittymän kunkin verkkoliittymän eri IP-aliverkon (ja eikä reitittää), valitse vain 2 polut ovat käytettävissä.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Vaihe 1: Asenna Windows Server-isännän MPIO

MPIO on valinnainen ominaisuus, Windows Server ja ei asennu oletusarvoisesti. Se on asennettava ominaisuudeksi kautta Serverin hallinta. Tämä ominaisuus asennetaan Windows Server host, toimi seuraavasti.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Vaihe 2: Määritä MPIO StorSimple levyasemien

MPIO varten on määritettävä tunnistavan StorSimple asemat. Voit määrittää MPIO tunnistettavan StorSimple tietomääristä toimimalla seuraavien ohjeiden mukaisesti.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Vaihe 3: Ota StorSimple tietomääristä isännän

Kun MPIO on määritetty Windows Server, asema(t) luotu StorSimple matriisi voidaan asentaa ja sitten hyödyntää MPIO redundancy varten. Jos haluat aseman, suorita seuraavat vaiheet.

#### <a name="to-mount-volumes-on-the-host"></a>Ota käyttöön tietomääristä isännän

1. Avaa Windows Server-isännän **iSCSI käynnistäjä ominaisuudet** -ikkuna. Valitse **Serverin hallinta > raporttinäkymät > Työkalut > iSCSI käynnistäjä**.
2. **ISCSI käynnistäjä ominaisuudet** -valintaikkunassa etsintä-välilehti ja valitse sitten **Tutustu kohde-portaalissa**.
3. **Tutustu kohde Portal** -valintaikkunassa seuraavasti:
    
    - Anna StorSimple virtual matriisin ensimmäisen käytössä verkkoliittymän IP-osoite. Oletusarvoisesti tämä olisi **Ethernet**. 
    - Valitse palaa **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa **OK** .

    >[AZURE.IMPORTANT] **Jos käytät yksityisverkon iSCSI yhteyksien, kirjoita tiedot portin, joka on liitetty yksityinen verkko IP-osoite.**

4. Toista vaiheet 2 – 3 toisessa verkko-liittymän (esimerkiksi Ethernet-2)-matriisin. 

5. Valitse **kohteet** -välilehdessä **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa. Oman virtual matriisin näkyy kunkin aseman pinta kohteeksi **Löydetyt kohteet**-kohdassa. Tässä tapauksessa havaitaan kolme kohteet (koskien kolme asemaa).

    ![mpio1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. Valitse **Yhdistä** muodostaa iSCSI-istunnon StorSimple matriisin kanssa. **Yhteyden muodostaminen kohde** -valintaikkuna tulee näkyviin. Valitse **Ota käyttöön usean polku** -valintaruutu. Valitse **Lisäasetukset**.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. Valitse **Lisäasetukset** -valintaikkunassa seuraavasti:                                       
    -    Valitse pudotusvalikosta **Paikallisen sovittimen** **Microsoft iSCSI-käynnistäjä**.
    -    Valitse **Käynnistäjä IP** -pudotusvalikosta isännän IP-osoite.
    -    Valitse **Kohde-portaalin** IP avattavasta luettelosta taulukko käyttöliittymän IP.
    -    Valitse palaa **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa **OK** .

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Valitse **Ominaisuudet**. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. Valitse **Ominaisuudet** -valintaikkunan **Lisää istunto**.

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. Valitse **Yhdistä kohteeseen** -valintaikkunassa **Ota käyttöön usean polku** -valintaruutu. Valitse **Lisäasetukset**.
11. **Lisäasetukset** -valintaikkunassa:                                        
    -  Valitse pudotusvalikosta **paikallisen sovittimen** Microsoft iSCSI-käynnistäjä.
    -  Valitse **Käynnistäjä IP** -pudotusvalikosta vastaavat isännän IP-osoite. Tässä tapauksessa muodostat yhden verkoston liittymään isännän kaksi verkon liittymää matriisin. Tämän vuoksi liittymän on sama kuin ensimmäisen istunnon olleen.
    -  Valitse **Kohde Portal IP** -pudotusvalikosta toisen taulukon käytössä tietojen-liittymän IP-osoite.
    -  Valitse palaa iSCSI käynnistäjä ominaisuudet-valintaikkunassa **OK** . Olet lisännyt toisen istunnon kohde.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Kun olet lisännyt haluamasi istunnot (polut) **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa, valitse kohde ja valitse **Ominaisuudet**. **Ominaisuudet** -valintaikkunan istunnot-välilehdellä Huomautus tunnukset, jotka vastaavat mahdollista polku permutaatioiden neljä istunto. Jos haluat peruuttaa istunnon, valitse istunnon tunnisteen vieressä oleva valintaruutu ja valitse sitten **Katkaise yhteys**.
 
    - Voit tarkastella istuntojen sisältämien laitteita, valitse **laitteet** -välilehti. Voit määrittää valitun laitteen MPIO-käytäntö valitsemalla **MPIO**. **
    -  Tiedot** valintaikkuna tulee näkyviin. Valitse **MPIO** -välilehdessä voit valita haluamasi **kuormituksen saldo käytännön** asetukset. Voit myös tarkastella **aktiivisen** tai **valmius ** polku.

10. Toista vaiheet 8-11 useampia istuntoja (polut) lisääminen kohde. Kaksi liittymää isännän ja kaksi virtual matriisin käyttöön voit lisätä korkeintaan neljä istuntojen kunkin kohteen. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. Tarvitset, toista nämä vaiheet jokaisen aseman (Näyttömalli käyttää kohteeksi).

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. Avaa **Tietokoneenhallinta** siirtymällä **Serverin hallinta > raporttinäkymät > Tietokoneenhallinta**. Valitse vasemmanpuoleisessa ruudussa **tallennustilan > Disk Management**. Asema(t) StorSimple virtual matriisi on luotu, jotka näkyvät isäntä näkyy **Levynhallinta** -kohdassa uusi levyt.

13. Alusta levy ja luo uusi asema. Muotoile aikana Valitse kohdistus yksikön koko (Australia) 64 kilotavua. Toista tämä kaikki käytettävissä olevat asemat.

    ![Levynhallinta](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. **Levynhallinta**-kohdassa **levyn** hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

15. Valitse **Usean polku levyn laitteen ominaisuudet** -valintaikkunan **MPIO** -välilehti.

    ![Levyn ominaisuudet](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. **DSM nimi** -kohtaan valitsemalla **tiedot** ja varmista, että parametrit on määritetty oletusarvoinen parametrit. Oletusparametrit ovat seuraavat:

    - Polun Tarkista kauden = 30
    - Laske uudelleen = 3
    - San poistaa kauden = 20
    - Yritä väli = 1
    - Tarkista polku käytössä = ole valittuna.

    >[AZURE.NOTE] **Älä muokkaa oletusarvo-parametrit.**


## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [ammattimainen StorSimple Virtual matriisin StorSimple hallinta-palvelun avulla](storsimple-ova-manager-service-administration.md).
 
