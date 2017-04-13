<properties 
   pageTitle="Määritä MPIO StorSimple laitteeseesi | Microsoft Azure"
   description="Tässä artikkelissa käsitellään monipolku i/o (MPIO) määrittämiseksi isäntään, joka on käynnissä Windows Server 2012 R2 StorSimple laitteen."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-for-your-storsimple-device"></a>Määritä monipolku i/o StorSimple laitteeseesi

Microsoft suunniteltu tuki monipolku i/o (MPIO) toiminnon Windows Server ohjeen muodosta erittäin käytettävissä, vikasietoinen SAN määrityksiä. MPIO käyttää tarpeettomat fyysinen polku-komponentteja, ohjaimet, kaapeli ja valitsimet – voit luoda loogisen polut palvelimeen ja tallennustilaa laitteen välillä. Jos osa virheen vuoksi looginen polku epäonnistuu, Monipolkujen käyttö logiikan käyttää vaihtoehtoisen polun i/o niin, että sovellukset voivat edelleen tietoihinsa. Lisäksi määritysten mukaan MPIO voi parantaa suorituskykyä myös tasaamisen uudelleen Lataa kaikki polut yli. Lisätietoja on artikkelissa [MPIO yleiskatsaus](https://technet.microsoft.com/library/cc725907.aspx "MPIO yleiskatsaus ja ominaisuuksia").  

StorSimple ratkaisu suuri-käytettävyyden MPIO on määritettävä StorSimple laitteen. MPIO on asennettu host palvelimiin käytössä Windows Server 2012 R2-palvelimet hyväksyt sitten linkin, verkossa tai LIP-virheen. 

MPIO on valinnainen ominaisuus, Windows Server ja ei asennu oletusarvoisesti. Se on asennettava ominaisuudeksi kautta Serverin hallinta. Tässä aiheessa kerrotaan vaihe vaiheelta pitäisi seurata asentaminen ja suorittaminen Windows Server 2012 R2 isännän MPIO-toiminnolla ja yhdistetty StorSimple fyysinen laite.

>[AZURE.NOTE] **Tämä toiminto on käytettävissä vain StorSimple 8000 sarjat. MPIO ei tällä hetkellä tue StorSimple virtual laitteessa.**

Tarvitset MPIO määrittämiseksi StorSimple laitteen seuraavasti:

- Vaihe 1: Asenna Windows Server-isännän MPIO

- Vaihe 2: Määritä MPIO StorSimple levyasemien

- Vaihe 3: Ota StorSimple tietomääristä isännän

- Vaihe 4: Määritä MPIO suuren ja kuormituksen tasaaminen

Kunkin edellä kuvatut toimet on kuvattu seuraavissa osissa.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Vaihe 1: Asenna Windows Server-isännän MPIO

Tämä ominaisuus asennetaan Windows Server host, toimi seuraavasti.

#### <a name="to-install-mpio-on-the-host"></a>Asenna MPIO isännän

1. Avaa Windows Server-isännän Serverin hallinta. Oletusarvon mukaan palvelimen hallinnan käynnistyy, kun Järjestelmänvalvojat-ryhmän kirjautuu tietokoneeseen, jossa on käytössä Windows Server 2012 R2 tai Windows Server 2012 jäsen. Jos palvelimen hallinta ei ole valmiiksi avoinna, valitse **Käynnistä > palvelimen hallinnan**.
![Palvelimen hallinta](./media/storsimple-configure-mpio-windows-server/IC740997.png)
2. Valitse **Serverin hallinta > raporttinäkymät > Lisää roolit ja ominaisuudet**. **Lisää roolit ja ominaisuudet** -ohjattu toiminto käynnistyy.
![Lisää rooleista ja ohjatun ominaisuuksien 1](./media/storsimple-configure-mpio-windows-server/IC740998.png)
3. **Lisää roolit ja ominaisuudet** ohjatun toimi seuraavasti:

    - **Ennen kuin aloitat** -sivulle valitsemalla **Seuraava**.
    - **Valitse asennustyyppi** -sivulla Hyväksy **Roolipohjainen tai ominaisuuksiin perustuvaan** asennuksen oletusasetusta. Valitse **Seuraava**. ![Lisää rooleista ja ohjatun ominaisuuksien 2](./media/storsimple-configure-mpio-windows-server/IC740999.png)
    - Valitse **Valitse kohdepalvelin** -sivulla, **Valitse palvelin palvelimen käytetään varannon**. Host (isäntä)-palvelimeen olisi havaitaan automaattisesti. Valitse **Seuraava**.
    - **Valitse roolit** -sivulle valitsemalla **Seuraava**.
    - **Valitse ominaisuudet** -sivun Valitse **Monipolku i/o**ja valitse **Seuraava**. ![Lisää rooleista ja ohjatun ominaisuuksien 5](./media/storsimple-configure-mpio-windows-server/IC741000.png)
    - **Vahvista asennuksen asetukset** -sivun, valitse Vahvista valinta ja valitse sitten **Käynnistä kohdepalvelin automaattisesti tarvittaessa**uudelleen, alla kuvatulla tavalla. Valitse **Asenna**. ![Lisää rooleista ja ohjatun ominaisuuksien 8](./media/storsimple-configure-mpio-windows-server/IC741001.png)
    - Saat ilmoituksen, kun asennus on valmis. Valitsemalla **Sulje** ohjattu toiminto. ![Lisää rooleista ja ohjatun ominaisuuksien 9](./media/storsimple-configure-mpio-windows-server/IC741002.png)

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Vaihe 2: Määritä MPIO StorSimple levyasemien

MPIO varten on määritettävä tunnistavan StorSimple asemat. Voit määrittää MPIO tunnistettavan StorSimple tietomääristä toimimalla seuraavien ohjeiden mukaisesti.

#### <a name="to-configure-mpio-for-storsimple-volumes"></a>Voit määrittää MPIO StorSimple tietomääristä

1. Avaa **MPIO määritys**. Valitse **Serverin hallinta > raporttinäkymät > Työkalut > MPIO**.

2. Valitse **MPIO ominaisuudet** -valintaikkunan **Tutustu usean polut** -välilehti.

3. Valitse **Lisää tuki iSCSI laitteet**ja valitse sitten **Lisää**.  
![MPIO ominaisuudet Tutustu usean polut](./media/storsimple-configure-mpio-windows-server/IC741003.png)

4. Käynnistä palvelin pyydettäessä uudelleen.
5. Valitse **MPIO ominaisuudet** -valintaikkunan **MPIO laitteet** -välilehti. Valitse **Lisää**.
    </br>![MPIO ominaisuudet MPIO laitteet](./media/storsimple-configure-mpio-windows-server/IC741004.png)
6. Kirjoita **Lisää MPIO tuki** -valintaikkunassa **Laitteen Laitteistotunnus**-laitteen sarjanumero. Voit avata laitteen sarjanumero käyttäminen StorSimple hallintapalvelu ja Siirtyminen **laitteiden > Raporttinäkymät-ikkunan**. Laitteen sarjanumero näkyy oikeassa ruudussa **Quick Glance** laitteen Raporttinäkymät-ikkunan.
    </br>![Lisää MPIO tuki](./media/storsimple-configure-mpio-windows-server/IC741005.png)
7. Käynnistä palvelin pyydettäessä uudelleen.

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Vaihe 3: Ota StorSimple tietomääristä isännän

Kun MPIO on määritetty Windows Server, asema(t) luotu StorSimple laitteeseen voidaan asentaa ja sitten hyödyntää MPIO redundancy varten. Jos haluat aseman, suorita seuraavat vaiheet.

#### <a name="to-mount-volumes-on-the-host"></a>Ota käyttöön tietomääristä isännän

1. Avaa Windows Server-isännän **iSCSI käynnistäjä ominaisuudet** -ikkuna. Valitse **Serverin hallinta > raporttinäkymät > Työkalut > iSCSI käynnistäjä**.
2. **ISCSI käynnistäjä ominaisuudet** -valintaikkunassa etsintä-välilehti ja valitse sitten **Tutustu kohde-portaalissa**.
3. **Tutustu kohde Portal** -valintaikkunassa seuraavasti:
    
    - Kirjoita tiedot portin StorSimple laitteen IP-osoite (esimerkiksi, kirjoita tiedot 0).
    - Valitse palaa **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa **OK** .

    >[AZURE.IMPORTANT] **Jos käytät yksityisverkon iSCSI yhteyksien, kirjoita tiedot portin, joka on liitetty yksityinen verkko IP-osoite.**

4. Toista vaiheet 2 – 3 toisessa verkko-liittymän (esimerkiksi tietoja 1) laitteen. Ota huomioon, että nämä liityntäkohdat on käytössä iSCSI. Lisätietoja on artikkelissa [Muokkaa verkkoliittymät](storsimple-modify-device-config.md#modify-network-interfaces).
5. Valitse **kohteet** -välilehdessä **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa. Raportissa pitäisi näkyä StorSimple laitteen kohde IQN **Löydetyt kohteet**-kohdassa.
 ![iSCSI käynnistäjä ominaisuudet kohteet-välilehti](./media/storsimple-configure-mpio-windows-server/IC741007.png)
6. Valitse **Yhdistä** muodostaa iSCSI-istunnon StorSimple-laitteen kanssa. **Yhteyden muodostaminen kohde** -valintaikkuna tulee näkyviin.

7. Valitse **Yhdistä kohteeseen** -valintaikkunassa **Ota käyttöön usean polku** -valintaruutu. Valitse **Lisäasetukset**.

8. Valitse **Lisäasetukset** -valintaikkunassa seuraavasti:                                       
    -    Valitse pudotusvalikosta **Paikallisen sovittimen** **Microsoft iSCSI-käynnistäjä**.
    -    Valitse **Käynnistäjä IP** -pudotusvalikosta isännän IP-osoite.
    -    Valitse **Kohde-portaalin** IP avattavasta luettelosta laitteen käyttöliittymää IP.
    -    Valitse palaa **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa **OK** .

9. Valitse **Ominaisuudet**. Valitse **Ominaisuudet** -valintaikkunan **Lisää istunto**.
10. Valitse **Yhdistä kohteeseen** -valintaikkunassa **Ota käyttöön usean polku** -valintaruutu. Valitse **Lisäasetukset**.
11. **Lisäasetukset** -valintaikkunassa:                                        
    -  Valitse pudotusvalikosta **paikallisen sovittimen** Microsoft iSCSI-käynnistäjä.
    -  Valitse **Käynnistäjä IP** -pudotusvalikosta vastaavat isännän IP-osoite. Tässä tapauksessa muodostat yhden verkoston liittymään isännän kaksi verkon liittymää laitteeseen. Tämän vuoksi liittymän on sama kuin ensimmäisen istunnon olleen.
    -  Valitse **Kohde Portal IP** -pudotusvalikosta toisen laitteen käytössä tietojen-liittymän IP-osoite.
    -  Valitse palaa iSCSI käynnistäjä ominaisuudet-valintaikkunassa **OK** . Olet lisännyt toisen istunnon kohde.

12. Avaa **Tietokoneenhallinta** siirtymällä **Serverin hallinta > raporttinäkymät > Tietokoneenhallinta**. Valitse vasemmanpuoleisessa ruudussa **tallennustilan > Disk Management**. Asema(t) luotu StorSimple laitteeseen, jotka näkyvät isäntä näkyy **Levynhallinta** -kohdassa uusi levyt.

13. Alusta levy ja luo uusi asema. Muotoile aikana Valitse estä koko 64 kilotavua.
![Levynhallinta](./media/storsimple-configure-mpio-windows-server/IC741008.png)
14. **Levynhallinta**-kohdassa **levyn** hiiren kakkospainikkeella ja valitse **Ominaisuudet**.
15. StorSimple mallin ### **Usean polku levyn laitteen ominaisuudet** -valintaikkunan-ruudussa **MPIO** -välilehti.
![StorSimple 8100 usean polku levyn DeviceProp.](./media/storsimple-configure-mpio-windows-server/IC741009.png)

16. **DSM nimi** -kohtaan valitsemalla **tiedot** ja varmista, että parametrit on määritetty oletusarvoinen parametrit. Oletusparametrit ovat seuraavat:

    - Polun Tarkista kauden = 30
    - Laske uudelleen = 3
    - San poistaa kauden = 20
    - Yritä väli = 1
    - Tarkista polku käytössä = ole valittuna.


>[AZURE.NOTE] **Älä muokkaa oletusarvo-parametrit.**

## <a name="step-4-configure-mpio-for-high-availability-and-load-balancing"></a>Vaihe 4: Määritä MPIO suuren ja kuormituksen tasaaminen

N usean polku suuren käytettävyyden ja kuormituksen tasaamisen, useita istuntoja on voi lisätä manuaalisesti määritellä käytettävissä eri polkuja. Esimerkiksi jos isännän on kaksi liittymää yhteydessä SAN ja laite on yhdistetty SAN kaksi liittymää ja sinun on määritetty oikea polku permutaatioiden (vain istunnosta on suoritettava Jos kunkin tietojen käyttöliittymässä ja Host (isäntä)-liittymän on eri IP-osoitteiden ja ei ole reititettävä) neljä istunnot.

>[AZURE.IMPORTANT] **On suositeltavaa, että sinulla ole yhdistelmiä 1 GbE ja 10 GbE verkon rajapintojen. Jos käytät kahta verkkoliittymät, sekä liittymät on oltava sama tyyppi.**

Seuraavassa kerrotaan, miten voit lisätä istuntoon, kun StorSimple laitetta, jonka kaksi verkon liittymää on liitetty isäntä, jossa kaksi verkon liittymää.

### <a name="to-configure-mpio-for-high-availability-and-load-balancing"></a>Voit määrittää MPIO suuren ja kuormituksen tasaaminen

1. Suorittaa kohteen etsiminen: Valitse **iSCSI käynnistäjä ominaisuudet** -valintaikkunan **Etsintä** -välilehden **Tutustu Portal**.
2. Kirjoita **Yhdistä kohteeseen** -valintaikkunassa laitteen verkkoliittymät IP-osoite.
3. Valitse palaa **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa **OK** .

4. **ISCSI käynnistäjä ominaisuudet** -valintaikkunassa valitse **kohteet** -välilehti, korosta havaitun kohde ja valitse **Yhdistä**. **Yhteyden muodostaminen kohde** -valintaikkuna.

5. **Yhdistä kohteeseen** -valintaikkunassa:
    
    - Jätä määrittäminen **Lisää yhteyden** tuttuja kohteiden luettelo oletusarvon valittu kohde. Näin saat laitteen yrittää yhteyden uudelleen aina, kun tämä tietokone käynnistyy automaattisesti.
    - Valitse **Ota käyttöön usean polku** -valintaruutu.
    - Valitse **Lisäasetukset**.

6. **Lisäasetukset** -valintaikkunassa:
    - Valitse pudotusvalikosta **Paikallisen sovittimen** **Microsoft iSCSI-käynnistäjä**.
    - Valitse **Käynnistäjä IP** -pudotusvalikosta isännän IP-osoite.
    - Valitse **Kohde Portal IP** -pudotusvalikosta laitteeseen käytössä tietojen liittymän IP-osoite.
    - Valitse palaa iSCSI käynnistäjä ominaisuudet-valintaikkunassa **OK** .

7. Valitse **Ominaisuudet**ja valitse **Ominaisuudet** -valintaikkunan **Lisää istunto**.

8. **Yhdistä kohteeseen** -valintaikkunassa **usean polku käyttöön** -valintaruutu ja valitse sitten **Lisäasetukset**.

9. **Lisäasetukset** -valintaikkunassa:
    1. Valitse pudotusvalikosta **paikallisen sovittimen** **Microsoft iSCSI-käynnistäjä**.
    2. Valitse **Käynnistäjä IP** -pudotusvalikosta vastaavat toisen liittymään isännän IP-osoite.
    3. Valitse **Kohde Portal IP** -pudotusvalikosta toisen laitteen käytössä tietojen-liittymän IP-osoite.
    4. Valitse palaa **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa **OK** . Olet lisännyt toisen istunnon nyt kohteeseen.

10. Toista vaiheet 8 – 10 useampia istuntoja (polut) lisääminen kohde. Kaksi liittymää isännän ja kaksi laitteeseen voit lisätä korkeintaan neljä istuntoja.

11. Kun olet lisännyt haluamasi istunnot (polut) **iSCSI käynnistäjä ominaisuudet** -valintaikkunassa, valitse kohde ja valitse **Ominaisuudet**. **Ominaisuudet** -valintaikkunan istunnot-välilehdellä Huomautus tunnukset, jotka vastaavat mahdollista polku permutaatioiden neljä istunto. Jos haluat peruuttaa istunnon, valitse istunnon tunnisteen vieressä oleva valintaruutu ja valitse sitten **Katkaise yhteys**.

12. Voit tarkastella istuntojen sisältämien laitteita, valitse **laitteet** -välilehti. Voit määrittää valitun laitteen MPIO-käytäntö valitsemalla **MPIO**. **Laitteen tiedot** -valintaikkuna tulee näkyviin. **MPIO** -välilehdessä voit valita haluamasi **kuormituksen saldo** käytäntöasetukset. Voit myös tarkastella **aktiivinen** tai **valmius** -Polkutyyppi.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [muokattava StorSimple laitteen kokoonpanosi StorSimple hallinta-palvelun avulla](storsimple-modify-device-config.md).
 
