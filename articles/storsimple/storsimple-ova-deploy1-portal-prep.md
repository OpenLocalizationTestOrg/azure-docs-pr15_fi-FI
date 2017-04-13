<properties
   pageTitle="Ottaa käyttöön StorSimple Virtual taulukko 1 - portaalin valmistelu"
   description="Ensimmäinen opetusohjelma ottamaan StorSimple virtual matriisin liittyy portaalin valmisteleminen"
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
   ms.date="05/24/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---prepare-the-portal"></a>Ottaa käyttöön StorSimple Virtual matriisi - portaalin valmisteleminen

![](./media/storsimple-ova-deploy1-portal-prep/getstarted4.png)

## <a name="overview"></a>Yleiskatsaus

Tämä artikkeli koskee Microsoft Azure StorSimple Virtual matriisi (tunnetaan myös nimellä StorSimple paikallisen virtual laitteen tai StorSimple virtual laite) käynnissä maaliskuussa 2016 yleiseen käyttöön (GA)-versioon. Tämä on ensimmäinen artikkeli käyttöönoton opetusohjelmat käyttöönoton kokonaan tiedostopalvelimeen tai iSCSI palvelimen virtual matriisin edellyttämät sarjan. Tässä artikkelissa kuvataan valmistelu tarvitse luominen ja määrittäminen ennen valmistelu virtual matriisin StorSimple hallinta-palvelussa. Tässä artikkelissa on myös linkki ulos käyttöönoton määrityksen tarkistusluettelo sekä määritysten edellytykset.

Sinun on järjestelmänvalvojan oikeudet Viimeistele asennus ja määritys. On suositeltavaa, että luet käyttöönoton määrityksen tarkistusluettelo, ennen kuin aloitat. Portaalin valmistelu kestää alle 10 minuuttia.

Julkaistu tämän artikkelin tiedot koskevat Azure perinteinen portal sekä Microsoft Azure Government Cloud StorSimple Virtual matriisien käyttöönotto.

### <a name="get-started"></a>Käytön aloittaminen

Käyttöönotto-työnkulun koostuu valmistellaan portaalin ja valmistelu virtualisoidussa ympäristössä virtual matriisi asetusten. Aloita StorSimple Virtual matriisin käyttöönoton tiedostopalvelimeen tai iSCSI-palvelimeen, tarvitset taulukoituja seuraavissa resursseissa (artikkelit ja videot).

#### <a name="deployment-articles"></a>Käyttöönoton artikkelit

Lisätietoja on seuraavissa artikkeleissa ottamaan StorSimple Virtual matriisin järjestetty määritetyssä järjestyksessä.

| **#** | **Tässä vaiheessa**                          | **Voit tehdä tämän...**                                                         | **Käytä näitä asiakirjoja.**|
|------|-------------------------------------------|--------------------------------------------------------------------------------|------------------------|
|1.   | **Azure perinteinen portaalin määrittäminen**       | Luo ja määritä ennen valmistelu StorSimple virtual laitteen StorSimple hallinta-palvelussa.  |[Portaalin valmisteleminen](storsimple-ova-deploy1-portal-prep.md)|
|2.   | **Valmistele Virtual matriisi**           | Hyper-V valmistelu ja muodostaa yhteyden StorSimple virtual laitteeseen Hyper-V käytössä Windows Server 2012 R2, Windows Server 2012: ssa tai Windows Server 2008 R2 host järjestelmään. <br></br> <br></br> Valmistele VMware, ja muodostaa yhteyden StorSimple paikallisen virtual laitteeseen host järjestelmässä käytössä VMware ESXi 5.5 ja yläpuolella.<br></br>| [Valmistele virtual matriisin Hyper-v](storsimple-ova-deploy2-provision-hyperv.md) <br></br> <br></br> [Valmistele-VMware virtual matriisi](storsimple-ova-deploy2-provision-vmware.md)|
|3.    | **Virtuaalinen matriisin määrittäminen**              | Tiedosto-palvelin-suorittaa ensimmäisen määrityskerran rekisteröidä StorSimple tiedostopalvelimeen ja laitteen asennuksen. Voit valmistella sitten SMB osakkeet. <br></br> <br></br> ISCSI-palvelin-suorittaa ensimmäisen määrityskerran ja laitteen asennuksen rekisteröidä StorSimple iSCSI palvelimellesi. Voit valmistella sitten iSCSI asemat.| [Määritetty tiedostopalvelimeen virtual matriisi](storsimple-ova-deploy3-fs-setup.md)<br></br> <br></br>[Virtuaalinen matriisin iSCSI palvelimeksi määrittäminen](storsimple-ova-deploy3-iscsi-setup.md)|

#### <a name="deployment-videos"></a>Käyttöönotto-videot

| **Tee tämä vaihe...** |  **Katso tämä video.**|
|----------------|-------------|
| Vaiheittaiset ohjeet StorSimple Virtual matriisin käytön aloittaminen. | [StorSimple Virtual matriisin käytön aloittaminen](https://azure.microsoft.com/documentation/videos/get-started-with-the-storsimple-virtual-array/)|
| Vaiheittaiset ohjeet valmistelu Hyper-v StorSimple Virtual matriisi.|[Luo StorSimple Virtual matriisi](https://azure.microsoft.com/documentation/videos/create-a-storsimple-virtual-array/) |
|Vaiheittaiset ohjeet ja rekisteröi StorSimple Virtual matriisi|[Määritä StorSimple Virtual matriisi](https://azure.microsoft.com/documentation/videos/configure-a-storsimple-virtual-array/)|
|Vaiheittaiset ohjeet osakkeet, Luo osakkeet varmuuskopioiminen ja palauttaminen StorSimple Virtual matriisi, määritetty tiedostopalvelimeen tiedot|[Käytä StorSimple Virtual matriisi](https://azure.microsoft.com/documentation/videos/use-the-storsimple-virtual-array/)|
|Vaiheittaiset ohjeet StorSimple Virtual matriisin automaattisesti ja tietojen palauttaminen|[StorSimple Virtual matriisin palauttaminen](https://azure.microsoft.com/documentation/videos/storsimple-virtual-array-disaster-recovery/)

Voit nyt aloittaa Azure perinteinen portaalin määrittäminen.

## <a name="configuration-checklist"></a>Määrityksen tarkistusluettelo

Määrityksen tarkistusluettelo kuvataan tietoja, jotka haluat kerätä ennen kuin määrität ohjelmiston StorSimple laitteen. Valmistellaan etuajassa tiedot auttavat tehostaa käyttöönotto ympäristössäsi StorSimple-laite. Sen mukaan, onko StorSimple virtual laitteen otetaan käyttöön tiedostopalvelimeen tai iSCSI-palvelimeen, sinun on jokin seuraavista tarkistusluetteloiden.

-   Lataa [StorSimple Virtual matriisi tiedoston Server Configuration tarkistusluettelon](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayFileServerConfigurationChecklist.pdf).

-   Lataa [StorSimple Virtual matriisin iSCSI Server Configuration tarkistusluettelon](http://download.microsoft.com/download/E/E/6/EE690BB0-B442-4B84-8165-4731EE727ACF/MicrosoftAzureStorSimpleVirtualArrayiSCSIServerConfigurationChecklist.pdf).

## <a name="prerequisites"></a>Edellytykset

Tätä löydät StorSimple hallintapalveluun, StorSimple virtual laitteen ja palvelinkeskuksen verkon määritysten edellytyksistä.

### <a name="for-the-storsimple-manager-service"></a>StorSimple hallinta-palveluun

Ennen kuin aloitat, varmista, että:

-   Sinulla on Microsoft-tilisi kanssa tunnistetiedoilla.

-   Sinulla on Microsoft Azure tallennustilan tilisi tunnistetiedoilla.

-   Microsoft Azure-tilauksen pitäisi olla käytössä StorSimple hallinta.

### <a name="for-the-storsimple-virtual-device"></a>StorSimple virtual laitteen

Ennen kuin otat virtual laitteen, varmista, että:

-   Sinulla on oikeudet host järjestelmän toimintaa Hyper-V Windows Server 2008 R2 tai uudempia tai VMware (ESXi 5,5 tai uudempi versio), joka voidaan käyttää varaus laite.

-   Host ()-isäntäjärjestelmä pystyy tälle valmistelu virtual laite on seuraavissa resursseissa:

    -   Vähintään 4 sydämiä.

    -   Vähintään 8 Gigatavun RAM-muistia.

    -   Yhden verkoston käyttöliittymä.

    -   500 gt virtual levyn Järjestelmätiedot.

### <a name="for-the-datacenter-network"></a>Palvelinkeskuksen verkossa

Ennen kuin aloitat, varmista, että:

-   Oman joten verkko on määritetty verkko StorSimple laitteeseesi mukaisesti. Lisätietoja on artikkelissa [StorSimple Virtual matriisin järjestelmävaatimukset](storsimple-ova-system-requirements.md).

-   StorSimple virtual laitteessasi on erillinen 5 Mbps Internet-kaistanleveyden (tai useamman) käytettävissä koko ajan. Tämä kaistanleveyden ei voi jakaa muiden sovellusten kanssa.

## <a name="step-by-step-preparation"></a>Vaiheittaiset ohjeet valmistelu

Seuraavien vaiheittaisten ohjeiden avulla voit laatia portaalin StorSimple hallinta-palveluun.

## <a name="step-1-create-a-new-service"></a>Vaihe 1: Luo uusi palvelu

Yksittäisen esiintymän StorSimple hallintapalvelu voit hallita useita StorSimple 1200 laitteita. Seuraavien toimien voit luoda uuden esiintymän StorSimple hallintapalvelu. Jos sinulla on aiemmin luodun StorSimple hallinta-palvelun 1200 laitteiden hallinta, Ohita tämä vaihe ja siirry [vaiheen2 ohjeiden mukaisesti: hankkia palvelun rekisteröinti avain](#step-2-get-the-service-registration-key).

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

> [AZURE.IMPORTANT]
>
> Jos ei ollut käytössä tallennustilan tilin luominen automaattisesti palveluun, sinun on vähintään yksi tallennustilan tilin luominen, kun olet luonut palvelu.
>

> - Jos et ole luonut automaattisesti tallennustilan tilin, siirry [Määritä uusi tallennustilan tili-palvelun](#optional-step-configure-a-new-storage-account-for-the-service) saat yksityiskohtaiset ohjeet.
>

> - Jos ottanut automaattisen luomisen tallennustilan tilin, siirry [Vaihe 2: hankkia palvelun rekisteröinti avain](#step-2-get-the-service-registration-key).


## <a name="step-2-get-the-service-registration-key"></a>Vaihe 2: Palvelun rekisteröinti Key-tunnuksen hankkiminen


Kun StorSimple hallintapalvelu on hyvin alkuun, sinun on hankkia palvelun rekisteröinti avain. Voit rekisteröidä ja yhdistää StorSimple laitteen palveluun käyttää avainta.

Seuraavien toimien [Azure perinteinen portal](https://manage.windowsazure.com/).


[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

> [AZURE.NOTE]
>
> Palvelun rekisteröinti avainta käytetään Rekisteröi kaikki StorSimple hallinta-laitteet, joissa on rekisteröitävä StorSimple hallinta-palvelun kanssa.

## <a name="step-3-download-the-virtual-device-image"></a>Vaihe 3: Lataa virtual laitteen kuva

Kun olet palvelun rekisteröinti avainta, sinun on lataamaan virtual laitteen kuvan valmisteleminen virtual laitteen Host (isäntä)-järjestelmään. Virtuaalinen laitteen kuvat ovat tietyn käyttöjärjestelmän ja voi ladata Pika-aloitus-sivulla Azure perinteinen portaalissa.

> [AZURE.IMPORTANT] StorSimple Virtual matriisin käytössä ohjelmiston voi käyttää vain Storsimple hallinta-palvelun kanssa.


Seuraavien toimien [Azure perinteinen portal](https://manage.windowsazure.com/).

#### <a name="to-get-the-virtual-device-image"></a>Virtuaalinen laitteen kuva

1.  Valitse palvelu, jonka loit **StorSimple hallinta** -sivulla. Tällöin näyttöön **Pika-aloitus** -sivulle. (Voit valita pika-aloitusopas-kuvake ![](./media/storsimple-ova-deploy1-portal-prep/image8.png) **Pika-aloitus** -sivun käyttämiseen milloin tahansa.)

1.  Valitse kuva, jonka haluat ladata Microsoft Download Centeristä vastaavat linkki. Kuvatiedostot ovat 4,8 Gigatavua.

    -   Hyper-V Windows Server 2012: ssa ja uudemmassa versiossa VHDX

    -   Hyper-V Windows Server 2008 R2 ja uudemmissa versioissa Näennäiskiintolevyn

    -   VMDK VMWare ESXi 5.5 tai uudempaa versiota

2.  Lataamalla ja purkamalla tiedoston tiedoston paikalliseen levyasemaan, että Huomautus, jossa wsp tiedosto sijaitsee.

![videokuvake](./media/storsimple-ova-deploy1-portal-prep/video_icon.png) **Video käytettävissä**

Katso vaiheittaiset ohjeet StorSimple Virtual matriisin Aloita video.

> [AZURE.VIDEO get-started-with-the-storsimple-virtual-array]



## <a name="optional-step-configure-a-new-storage-account-for-the-service"></a>Valinnainen vaihe: uusi tallennustilan tilin-palvelun määrittäminen

Tämä on valinnainen vaihe on voidaan suorittaa vain, jos ei ollut käytössä tallennustilan tilin luominen automaattisesti palveluun.

Jos haluat luoda Azure-tallennustilan tilin toisella alueella, katso, [miten voit luoda tallennustilan tilin](storage-create-storage-account.md#create-a-storage-account) vaiheittaiset ohjeet.

Seuraavien toimien [Azure perinteinen portal](https://manage.windowsazure.com/) StorSimple hallinta-sivulle, voit lisätä käytössä olevan Microsoft Azure-tallennustilan tilin.

#### <a name="to-add-a-storage-account"></a>Tallennustilan tilin lisääminen

1.  Siirtymissivu StorSimple hallinta-palveluun Valitse palvelu ja kaksoisnapsauta sitä. Tällöin näyttöön **Pika-aloitus** -sivulle. Valitse **Määritä** sivun.

2.  Valitse **Lisää/Muokkaa tallennustilan tili**. **Lisää/Muokkaa tallennustilan tili** -valintaikkunassa seuraavasti:

    1.  Valitse **Lisää uusi**.

    1.  Anna tallennustilan tilin nimi.

    1.  Anna Microsoft Azure-tallennustilan tilin ensisijainen **Pikanäppäin** .

    1.  Valitse käyttämäsi laite ja pilveen väliseen viestintään verkon suojatun kanavan luominen **käyttöön SSL-tilassa** . Poista **Ota käyttöön SSL-tilassa** -valintaruudun valinta, vain, jos käyttämälläsi sisällä yksityinen pilvestä.

    1.  Valitse tarkistuksen kuvake ![](./media/storsimple-ova-deploy1-portal-prep/image7.png). Saat ilmoituksen, kun tallennustilan-tili on luotu.

        ![](./media/storsimple-ova-deploy1-portal-prep/image11.png)

1.  Juuri luomasi tallennustilan tilin näkyvät **tallennustilan tilien** **määrittäminen** -sivulla. Valitse Tallenna juuri luomasi tallennustilan tilin **Tallenna** . Valitse **OK** , kun ohjelma pyytää vahvistusta.


## <a name="next-step"></a>Seuraava vaihe

Seuraavaksi valmistelu virtual kone StorSimple virtual laitteeseesi. Katso yksityiskohtaisia ohjeita host käyttämäsi käyttöjärjestelmän mukaan:

-   [Valmistele StorSimple Virtual matriisin Hyper-v](storsimple-ova-deploy2-provision-hyperv.md)

-   [Valmistele-VMware StorSimple Virtual matriisi](storsimple-ova-deploy2-provision-vmware.md)
