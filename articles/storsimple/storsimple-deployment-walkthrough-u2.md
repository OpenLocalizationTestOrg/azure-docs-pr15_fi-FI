<properties 
   pageTitle="Ottaa käyttöön StorSimple laitteen (päivitys 2) | Microsoft Azure"
   description="Tässä artikkelissa kuvataan vaiheet ja parhaita käytäntöjä StorSimple Update 2 laitteen ja palvelun käyttöönotto."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="deploy-your-on-premises-storsimple-device-update-2"></a>Ottaa käyttöön paikallisen StorSimple laitteen (päivitys 2)

> [AZURE.SELECTOR]
- [Päivitä 2 ja uudempi versio](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Päivitys 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [GA julkaisu](../articles/storsimple/storsimple-deployment-walkthrough.md)

## <a name="overview"></a>Yleiskatsaus

Tervetuloa käyttämään Microsoft Azure StorSimple laitteen käyttöönotto. Käyttöönoton näissä Opetusohjelmissa koskevat StorSimple 8000 sarjan Update 2. Tämä videosarja, opetusohjelmat sisältää määrityksen tarkistusluettelo, määritys edellytykset ja yksityiskohtainen määritysvaiheet StorSimple laitteeseesi.

Näissä Opetusohjelmissa tiedot oletetaan, että käytössäsi tarkistanut turvallisuus, varotoimenpiteet ja avattaviksi, racked ja kytketty StorSimple laitteen. Jos haluat suorittaa sellaiset tehtävät, Aloita tarkistaminen [turvallisuutta varotoimenpiteet](storsimple-safety.md). Noudattamalla laitteen ohjeita purkaminen ja hyllykköä asennustapa kaapeli laitteen.

- [Pura, hyllykköä asennustapa ja että 8100 kaapeli](storsimple-8100-hardware-installation.md)
- [Pura, hyllykköä asennustapa ja että 8600 kaapeli](storsimple-8600-hardware-installation.md)

Sinun on järjestelmänvalvojan oikeudet Viimeistele asennus ja määritys. On suositeltavaa, että luet määrityksen tarkistusluettelo, ennen kuin aloitat. Käyttöönotto ja määritys-prosessi voi kestää jonkin aikaa suorittamiseen.

> [AZURE.NOTE] Microsoft Azure-sivustossa julkaistu StorSimple käyttöönoton tiedot koskevat vain StorSimple 8000 sarjan laitteita. Lisätietoja 7000 sarja-laitteet, siirry: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 7000 sarjan asennustiedot artikkelissa [StorSimple järjestelmän pika-aloitusopas](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Käyttöönoton vaiheet

Suorita StorSimple laitteen ja liittämällä sen StorSimple hallintapalvelu tarvittavat seuraavasti. Lisäksi tarvittavat vaiheet ovat valinnaisia ohjeita ja toimintojen, sinun on ehkä käyttöönoton aikana. Vaiheittaiset ohjeet käyttöönotto-ohjeet ilmoittaa, milloin kannattaa suorittaa kunkin valinnainen seuraavasti.


| Vaihe                                                                                   | Kuvaus                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EDELLYTYKSET**                                                                      | Nämä on suoritettava valmistelussa tulevista käyttöönottoa varten.                                                                                        |
| [Käyttöönoton määrityksen tarkistusluettelo](#deployment-configuration-checklist)                                                     | Voit käyttää tätä tarkistusluetteloa voit kerätä ja tallentaa tiedot ja sen käyttöönoton aikana.                                                                       |
| [Käyttöönoton edellytykset](#deployment-prerequisites)                                                               | Nämä vahvistaminen ympäristölle on valmis käyttöönottoa varten.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **VAIHEITTAISET OHJEET KÄYTTÖÖNOTTO**                                                                   | Tuotannon StorSimple laitteen käyttöön tarvitaan seuraavasti.                                                                                      |
| [Vaihe 1: Luo uusi palvelu](#step-1-create-a-new-service)                                                         | Määrittää cloud hallinta ja tallennustilaa StorSimple laitteeseesi. *Ohita tämä vaihe, jos sinulla on aiemmin luodun palvelun StorSimple muihin laitteisiin*.                |
| [Vaihe 2: Palvelun rekisteröinti Key-tunnuksen hankkiminen](#step-2-get-the-service-registration-key)                                               | Tämän avaimen avulla voit rekisteröidä ja yhdistää StorSimple laitteen hallinta-palveluun.                                                                         |
| [Vaihe 3: Määritä ja rekisteröi Windows PowerShellin kautta laitteen StorSimple](#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)    | Muodosta yhteys verkkoon laitteen ja rekisteröityä Azure Viimeistele määritykset management-palvelun avulla.                                            |
| [Vaihe 4: Suorita pienin laitteen määrittäminen](#step-4-complete-minimum-device-setupd)</br>[Valinnainen: Päivitä StorSimple-laitteeseen](#scan-for-and-apply-updates)      | Laitteen asennuksen ja ota se antamaan tallennustilan management-palvelun avulla.                                                                      |
| [Vaihe 5: Luo aseman säilö](#step-5-create-a-volume-container)                                                      | Voit luoda säilön säännöstä asemat. Äänenvoimakkuuden säilön on tallennustilan tilin ja kaikki sen sisältävät asemat salausasetukset kaistanleveyden.    |
| [Vaihe 6: Aseman luominen](#step-6-create-a-volume)                                                                | Valmistele tallennustilan asema(t) palvelinten StorSimple-laitteessa.                                                                                        |
| [Vaihe 7: Ota käyttöön, alusta ja muotoilla aseman](#step-7-mount-initialize-and-format-a-volume)</br>[Valinnainen: Määritä MPIO](storsimple-configure-mpio-windows-server.md)            | Yhdistä palvelinten laitteen myöntämä iSCSI-tallennustilan. Voit myös määrittää MPIO varmistamiseksi palvelinten hyväksyt linkin verkko- ja käyttöliittymän virhe.                                                                                                                                                              |
| [Vaihe 8: Varmuuskopioi](#step-8-take-a-backup)                                                                  | Tietojen suojaustapoja varmuuskopion käytäntöjen määrittäminen                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **MUITA OHJEITA**                                                                   | Joudut ehkä viitata nämä toimet, kun otat ratkaisu.                                                                                        |
| [Uusi tallennustilan tilin-palvelun määrittäminen](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [Yhteyden pitäminen laitteen serial konsoliin painovärit, muste avulla](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [Hanki Windows Server host IQN](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Luo manuaalinen varmuuskopiointi](#create-a-manual-backup)                                                                 | 


## <a name="deployment-configuration-checklist"></a>Käyttöönoton määrityksen tarkistusluettelo

Ennen kuin otat laitteessa, sinun on kerätä tietoja, voit määrittää ohjelmiston StorSimple laitteen. Jotkin näistä tiedoista etuajassa valmistellaan avulla tehostaa käyttöönotto ympäristössäsi StorSimple-laite. Lataa ja käyttää tätä tarkistusluetteloa ja Huomaa alaspäin tiedot, kun otat laitteen.

- [Lataa StorSimple käyttöönoton määrityksen tarkistusluettelo](http://www.microsoft.com/download/details.aspx?id=49159)


## <a name="deployment-prerequisites"></a>Käyttöönoton edellytykset

Seuraavissa osissa kerrotaan StorSimple hallintapalvelu ja StorSimple laitteen määritysten edellytyksistä.

### <a name="for-the-storsimple-manager-service"></a>StorSimple hallinta-palveluun

Ennen kuin aloitat, varmista, että:

- Sinulla on Microsoft-tilisi kanssa tunnistetiedoilla.

- Sinulla on Microsoft Azure tallennustilan tilisi tunnistetiedoilla.

- Microsoft Azure-tilauksesi on käytössä StorSimple hallintapalvelu. Tilauksesi on ostettuja [Enterprise Agreement-sopimus](https://azure.microsoft.com/pricing/enterprise-agreement/).

- Voit käyttää Pääte-emulointi-ohjelmistoa, kuten painovärit, muste.

### <a name="for-the-device-in-the-datacenter"></a>Laitteen joten

Ennen kuin määrität laitteen, varmista, että laitteesi on täysin avattaviksi, otettu käyttöön Teline- ja täysin kytketty power, verkon ja kuvattuja serial access:

-  [Pura ja hyllykköä asennustapa kaapeli 8100-laitteeseen](storsimple-8100-hardware-installation.md)
-  [Pura ja hyllykköä asennustapa kaapeli 8600 laitteen](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Verkon joten

Ennen kuin aloitat, varmista, että:

- Palvelinkeskuksen palomuurin porttien avataan Salli iSCSI ja cloud liikenne kuvatulla tavalla [Verkko StorSimple laitteen koskevat vaatimukset](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).


## <a name="step-by-step-deployment"></a>Vaiheittaiset ohjeet käyttöönotto

Seuraavien vaiheittaisten ohjeiden avulla voit ottaa käyttöön joten StorSimple laitteen.

## <a name="step-1-create-a-new-service"></a>Vaihe 1: Luo uusi palvelu

StorSimple hallintapalvelu voit hallita useita StorSimple laitteita. Seuraavien toimien voit luoda uuden esiintymän StorSimple hallintapalvelu.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [AZURE.IMPORTANT] Jos ei ollut käytössä tallennustilan tilin luominen automaattisesti palveluun, sinun on vähintään yksi tallennustilan tilin luominen, kun olet luonut palvelu. Tallennustilan tilin käytetään, kun luot aseman säilön. 
>
> * Jos et ole luonut automaattisesti tallennustilan tilin, siirry [Määritä uusi tallennustilan tili-palvelun](#configure-a-new-storage-account-for-the-service) saat yksityiskohtaiset ohjeet. 
> * Jos ottanut automaattisen luomisen tallennustilan tilin, siirry [Vaihe 2: hankkia palvelun rekisteröinti avain](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Vaihe 2: Palvelun rekisteröinti Key-tunnuksen hankkiminen

Kun StorSimple hallintapalvelu on hyvin alkuun, sinun on hankkia palvelun rekisteröinti avain. Voit rekisteröidä ja yhdistää StorSimple laitteen palveluun käyttää avainta.

Seuraavien toimien hallinta-portaalissa.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Vaihe 3: Määritä ja rekisteröi Windows PowerShellin kautta laitteen StorSimple

Windows PowerShell StorSimple avulla voit suorittaa StorSimple laitteen ensimmäisen määrityskerran seuraavassa esitetyllä tavalla. Sinun on pääte palvelinemulointiohjelmiston avulla voit suorittaa tämän vaiheen. Lisätietoja on artikkelissa [Käytä painovärit, muste muodostaa laitteen serial konsolin](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Vaihe 4: Suorita pienin laitteen määrittäminen

StorSimple laitteen vähimmäismäärä-kokoonpanossa olet edellyttää: 

- Määritä Toissijainen DNS-palvelin.
- Ota käyttöön iSCSI vähintään yksi verkon liittymän.
- Määrittää kiinteän IP-osoitteiden sekä ohjaimet.

Seuraavien toimien vähimmäismäärä-asennuksen viimeistelemiseen hallinta-portaalissa.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Vaihe 5: Luo aseman säilö

Äänenvoimakkuuden säilön on tallennustilan tilin ja kaikki sen sisältävät asemat salausasetukset kaistanleveyden. Haluat luoda aseman säilön, ennen kuin aloitat valmistelu tietomääristä StorSimple laitteen. 

Seuraavien toimien luominen aseman säilön hallinta-portaalissa.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Vaihe 6: Aseman luominen

Kun olet luonut aseman säilön, voit valmistella tallennustilan aseman StorSimple laitteeseen palvelinten. Seuraavien toimien aseman luominen hallinta-portaalissa.

> [AZURE.IMPORTANT] StorSimple hallinta voi luoda molemmat ohuen ja täysin valmistellun asemat. Et voi luoda kuitenkin osittain valmistellun asemat. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Vaihe 7: Ota käyttöön, alusta ja muotoilla aseman

Seuraavat vaiheet suoritetaan Windows Server host. 


> [AZURE.IMPORTANT]

> - StorSimple ratkaisu suuren käytettävyyden Suosittelemme, että määrität MPIO host palvelimiin (valinnainen) ennen iSCSI määrittäminen. MPIO määritysten host-palvelimissa varmistat palvelimet hyväksyt, linkki, verkossa tai LIP-virheen.

> - Siirry MPIO ja iSCSI asennuksesta ja määrityksestä ohjeet Windows Server isännän [Määrittäminen MPIO StorSimple laitteeseesi](storsimple-configure-mpio-windows-server.md). Näihin kuuluvat myös käyttöön, alusta ja muotoilla StorSimple tietomääristä vaiheet.

> - Ohjeet MPIO ja iSCSI asennuksesta ja määrityksestä Linux isännän Siirry [Määrittäminen MPIO StorSimple Linux isännän](storsimple-configure-mpio-on-linux.md)

Jos et halua määrittää MPIO, suorita seuraavat vaiheet ottaa käyttöön, alusta ja muotoilla StorSimple-asemista Windows Server host.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Vaihe 8: Varmuuskopioi

Varmuuskopioiden ajankohta suojausta asemista ja parantaa palauttaminen aikana pienentäminen palauttaminen kertaa. Voit tehdä varmuuskopion kahdentyyppisiä StorSimple laitteen: paikallisen tilannevedoksia ja cloud tilannevedoksia. Eri varmuuskopion voi olla **ajoitettu** tai **Manuaalinen**. 

Seuraavien toimien ajoitetun varmuuskopion luominen hallinta-portaalissa.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Voit ottaa manuaalinen varmuuskopiointi milloin tahansa. Siirry ohjeet- [Luo manuaalinen varmuuskopiointi](#create-a-manual-backup). 

## <a name="configure-a-new-storage-account-for-the-service"></a>Uusi tallennustilan tilin-palvelun määrittäminen

Tämä on valinnainen vaihe, sinun on suoritettava vain, jos ei ollut käytössä tallennustilan tilin luominen automaattisesti palveluun. Säilön StorSimple aseman luomiseen tarvitaan Microsoft Azure-tallennustilan tilin.

Jos haluat luoda Azure-tallennustilan tilin toisella alueella, saat [Tietoja Azure tallennustilan tilit](../storage/storage-create-storage-account.md) vaiheittaiset ohjeet.

Seuraavien toimien hallinta-portaalissa **StorSimple hallinta** -sivulle.

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Yhteyden pitäminen laitteen serial konsoliin painovärit, muste avulla

Jos haluat muodostaa StorSimple Windows PowerShell-käyttää Pääte-emulointi-ohjelmistoa, kuten painovärit, muste. Voit käyttää painovärit, muste, kun käytät laite suoraan serial konsolin tai avaamalla telnet istunnon etätietokoneista.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]


## <a name="scan-for-and-apply-updates"></a>Etsi ja päivitykset

Laitteen päivitys voi kestää useita tunteja. Seuraavien toimien etsiä ja asentaa päivityksiä laitteen.
<!--can take 1-4 hours--> 

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Jos haluat päivittää laitteen

1.  Valitse laitteen **Pika-aloitus** -sivulla Valitse **laitteet**. Valitse fyysinen laite, valitse **ylläpito** ja valitse sitten **Tarkista päivitykset**.  

2.  Etsi käytettävissä olevat päivitykset työn luodaan. Jos päivityksiä on saatavilla, **Asenna päivitykset**muuttuu **Tarkista päivitykset** . Valitse **Asenna päivitykset**. 

3.  Päivitä projekti luodaan. Seurata päivityksen tila siirtymällä **työt**.

    > [AZURE.NOTE] Työn päivitystä käynnistyessä, se näyttää heti tila 50 prosenttia. Tilaksi muuttuu 100 prosenttiin vasta, kun päivityksen projektin valmistumista. Ei ole päivitysprosessin tavoitettavuustietoja.

4.  Kun laite on päivitetty, käyttöön tietojen 2 ja 3 tietojen verkkoliittymät, jos ne on poistettu käytöstä.

<!-- In step 2, you may be requested to disable Data 2 and Data 3 prior to installing the updates. You must disable these network interfaces or the updates may fail.-->

## <a name="get-the-iqn-of-a-windows-server-host"></a>Hanki Windows Server host IQN

Seuraavien toimien saat iSCSI koko nimi (IQN) Windows-isännän, jossa on käytössä Windows Server® 2012.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Luo manuaalinen varmuuskopiointi

Seuraavien toimien luominen tarvittaessa yksittäisen aseman manuaalinen varmuuskopiointi StorSimple laitteen hallinta-portaalissa.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]


## <a name="next-steps"></a>Seuraavat vaiheet

- Määritä [virtual laite](storsimple-virtual-device-u2.md).

- [StorSimple hallinnan](storsimple-manager-service-administration.md) avulla voit hallita laitetta StorSimple.
 
