<properties 
   pageTitle="Ottaa käyttöön StorSimple laitteen (päivitys 1) | Microsoft Azure"
   description="Tässä artikkelissa kuvataan vaiheet ja parhaita käytäntöjä StorSimple päivitys 1 laitteen ja palvelun käyttöönotto."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="deploy-your-on-premises-storsimple-device-update-1"></a>Ottaa käyttöön paikallisen StorSimple laitteen (päivitys 1)

> [AZURE.SELECTOR]
- [Päivitä 2](../articles/storsimple/storsimple-deployment-walkthrough-u2.md)
- [Päivitys 1](../articles/storsimple/storsimple-deployment-walkthrough-u1.md)
- [GA julkaisu](../articles/storsimple/storsimple-deployment-walkthrough.md)

## <a name="overview"></a>Yleiskatsaus

Tervetuloa käyttämään Microsoft Azure StorSimple laitteen käyttöönotto. Käyttöönoton näissä Opetusohjelmissa koskevat StorSimple 8000 sarjan päivitys 1.0. Tämä videosarja, opetusohjelmat kuvataan StorSimple laitteen määrittäminen ja määrityksen tarkistusluettelo, määritys edellytykset ja määrityksistä vaiheet.

Näissä Opetusohjelmissa tiedot oletetaan, että käytössäsi tarkistanut turvallisuus, varotoimenpiteet ja avattaviksi, racked ja kytketty StorSimple laitteen. Jos haluat suorittaa sellaiset tehtävät, Aloita tarkistaminen [turvallisuutta varotoimenpiteet](storsimple-safety.md). Laitteen mallista riippuen sinun voit sitten Pura, Teline asennustapa ja kaapeli noudattamalla seuraavia ohjeita:

- [Pura, hyllykköä asennustapa ja että 8100 kaapeli](storsimple-8100-hardware-installation.md)
- [Pura, hyllykköä asennustapa ja että 8600 kaapeli](storsimple-8600-hardware-installation.md)

Sinun on järjestelmänvalvojan oikeudet Viimeistele asennus ja määritys. On suositeltavaa, että luet määrityksen tarkistusluettelo, ennen kuin aloitat. Käyttöönotto ja määritys-prosessi voi kestää jonkin aikaa suorittamiseen.

> [AZURE.NOTE] Microsoft Azure-sivustossa julkaistu StorSimple käyttöönoton tiedot koskevat vain StorSimple 8000 sarjan laitteita. Lisätietoja 5000 ja 7000 sarja-laitteet, siirry: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). 5000 ja 7000 sarjan käyttöönoton lisätietoja [StorSimple järjestelmän pika-aloitusopas](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Käyttöönoton vaiheet

Suorita StorSimple laitteen ja liittämällä sen StorSimple hallintapalvelu tarvittavat seuraavasti. Lisäksi tarvittavat vaiheet ovat valinnaisia ohjeita ja toimintojen, sinun on ehkä käyttöönoton aikana. Vaiheittaiset ohjeet käyttöönotto-ohjeet ilmoittaa, milloin kannattaa suorittaa kunkin valinnainen seuraavasti.


| Vaihe                                                                                   | Kuvaus                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EDELLYTYKSET**                                                                      | Nämä on suoritettava valmistelussa tulevista käyttöönottoa varten.                                                                                        |
| Käyttöönoton määrityksen tarkistusluettelo.                                                     | Voit käyttää tätä tarkistusluetteloa voit kerätä ja tallentaa tiedot ja sen käyttöönoton aikana.                                                                       |
| Käyttöönoton edellytykset.                                                               | Nämä vahvistaminen ympäristölle on valmis käyttöönottoa varten.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **VAIHEITTAISET OHJEET KÄYTTÖÖNOTTO**                                                                   | Tuotannon StorSimple laitteen käyttöön tarvitaan seuraavasti.                                                                                      |
| Vaihe 1: Luo uusi palvelu.                                                         | Määrittää cloud hallinta ja tallennustilaa StorSimple laitteeseesi. Ohita tämä vaihe, jos sinulla on aiemmin luodun palvelun StorSimple muihin laitteisiin.                |
| Vaihe 2: Pyydä palvelun rekisteröinti-näppäintä.                                               | Tämän avaimen avulla voit rekisteröidä ja yhdistää StorSimple laitteen hallinta-palveluun.                                                                         |
| Vaihe 3: Määritä ja Rekisteröidy StorSimple laitteen Windows PowerShellin kautta.    | Muodosta yhteys verkkoon laitteen ja rekisteröityä Azure Viimeistele määritykset management-palvelun avulla.                                            |
| Vaihe 4: Suorita pienin laitteen määrittäminen</br>Valinnainen: Päivitä StorSimple laitteen.      | Laitteen asennuksen ja ota se antamaan tallennustilan management-palvelun avulla.                                                                      |
| Vaihe 5: Luo aseman säilön.                                                      | Voit luoda säilön säännöstä asemat. Äänenvoimakkuuden säilön on tallennustilan tilin ja kaikki sen sisältävät asemat salausasetukset kaistanleveyden.    |
| Vaihe 6: Luo asema.                                                                | Valmistele tallennustilan asema(t) palvelinten StorSimple-laitteessa.                                                                                        |
| Vaihe 7: Ota käyttöön, alusta ja muotoilla aseman.</br>Valinnainen: Määritä MPIO.            | Yhdistä palvelinten laitteen myöntämä iSCSI-tallennustilan. Voit myös määrittää MPIO varmistaa, että palvelinten hyväksyt verkossa-linkkiä ja käyttöliittymän virhe.                                                                                                                                                              |
| Vaihe 8: Varmuuskopioi.                                                                  | Tietojen suojaustapoja varmuuskopion käytäntöjen määrittäminen                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **MUITA OHJEITA**                                                                   | Joudut ehkä viitata nämä toimet, kun otat ratkaisu.                                                                                        |
| Määritä uusi tallennustilan tili-palvelun.                                      |                                                                                                                                                               |
| Yhteyden pitäminen laitteen serial konsoliin painovärit, muste avulla.                                    |                                                                                                                                                               |
| Hanki Windows Server host IQN.                                                   |                                                                                                                                                               |
| Manuaalinen varmuuskopiointi luominen                                                                 | 


## <a name="deployment-configuration-checklist"></a>Käyttöönoton määrityksen tarkistusluettelo

Käyttöönoton määrityksen-tarkistusluettelo kuvataan tietoja, jotka haluat kerätä ja ennen kuin määrität ohjelmiston StorSimple laitteen. Jotkin näistä tiedoista etuajassa valmistellaan avulla tehostaa käyttöönotto ympäristössäsi StorSimple-laite. Voit käyttää tätä tarkistusluetteloa ja Huomaa alaspäin tiedot, kun otat laitteen.

| Vaihe                                  | Parametri                                         | Tiedot                                                                                                                                                                | Arvot |
|----------------------------------------|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| **Laitteen kaapeli**                      | Sarja käyttö                                     | Alkuperäinen laitteen määrittäminen                                                                  | Kyllä/ei |
|   |   |  |  |
| **Määritä ja rekisteröi laite**          | Tietoja 0 verkkoasetukset                           | Tietoja 0 IP-osoite:</br>Aliverkkopeite:</br>Yhdyskäytävän:</br>Ensisijainen DNS-palvelin:</br>Ensisijainen NTP-palvelimeen:</br>Web-välityspalvelimen IP/FQDN (valinnainen):</br>Web-välityspalvelimen portti:|        |
|                                        | Laitteen järjestelmänvalvojan salasana                     | Salasanassa on oltava 8-15 merkkiä kirjaimiksi, isot, numeeriset ja määräten merkkiä. |        |
|                                        | Salasanan StorSimple tilannevedoksen hallinta              | Salasanassa on oltava kirjaimiksi, isot, numeeriset ja määräten merkkejä sisältäviä 14 tai 15 merkkiä.|        |
|                                        | Palvelun rekisteröinti avain                          | Tämä avain luodaan perinteinen Azure-portaalista.    |        |
|                                        | Palvelun tiedot salausavain                       | Tämä avain luodaan, kun laite on rekisteröity StorSimple hallinta-palveluun Windows PowerShellin kautta. Kopioi tämä avain ja tallenna se turvalliseen paikkaan.|  |
|   |   |  |  |
| **Valmis pienin laitteen määrittäminen**          | Laitteen kutsumanimi                     | Tämä on laitteen kuvaava nimi. |        |
|                                        | Aikavyöhyke                                          | Laitteen käyttää tätä aikavyöhykkeen ajoitetun kaikki toiminnot.  |        |
|                                        | Toissijainen DNS-palvelin                              | Tämä on pakollinen asetus.                                  |        |
|                                        | Verkkoliittymän: tiedoista 0 kiinteä IP-osoitteet                                     | Nämä IP pitäisi olla reititettävä Internetiin.</br>Controller 0 kiinteä IP address:</br>Kiinteä ohjauskoneen 1 IP address:|
|   |   |  |  |
| **Lisää verkko-käyttöliittymän asetukset**  | Verkkoliittymän: tietojen 1</br>Jos iSCSI käytössä, ei määritetä yhdyskäytävän.      | Aihe: Cloud/iSCSI/ei käytössä</br>IP-osoite:</br>Aliverkkopeite:</br>Yhdyskäytävän:|
|                                        | Verkkoliittymän: tietojen 2</br>Jos iSCSI käytössä, ei määritetä yhdyskäytävän.      | Aihe: Cloud/iSCSI/ei käytössä</br>IP-osoite:</br>Aliverkkopeite:</br>Yhdyskäytävän:|
|                                        | Verkkoliittymän: tietojen 3</br>Jos iSCSI käytössä, ei määritetä yhdyskäytävän.      | Aihe: Cloud/iSCSI/ei käytössä</br>IP-osoite:</br>Aliverkkopeite:</br>Yhdyskäytävän:|
|                                        | Verkkoliittymän: tietojen 4</br>Jos iSCSI käytössä, ei määritetä yhdyskäytävän.      | Aihe: Cloud/iSCSI/ei käytössä</br>IP-osoite:</br>Aliverkkopeite:</br>Yhdyskäytävän:|
|                                        | Verkkoliittymän: tietojen 5</br>Jos iSCSI käytössä, ei määritetä yhdyskäytävän.      | Aihe: Cloud/iSCSI/ei käytössä</br>IP-osoite:</br>Aliverkkopeite:</br>Yhdyskäytävän:|
|   |   |  |  |
| **Luo aseman säilö**                      | Avauksen ja vaihdon säilön nimi:                            | Säilön nimi                                                                                                                                                 |        |
|                                        | Azure-tallennustilan tilin:                            | Tallennustilan tilin nimi ja access-näppäintä liittäminen äänenvoimakkuutta-säilö                                                                                              |        |
|                                        | Cloud tallennustilan salausavaimen:                     | Tallennustilan kunkin säilön salausavain                                                                                                                           |        |
|   |   |  |  |
| **Aseman luominen**                        | Jokaisen aseman tiedot                           | Levyn nimi:                                                                                                                                                           |        |
|                                        |                                                   | Enimmäiskoko:                                                                                                                                                                  |        |
|                                        |                                                   | Käyttötyyppi:                                                                                                                                                            |        |
|                                        |                                                   | ACR nimi:                                                                                                                                                              |        |
|                                        |                                                   | Varmuuskopion oletuskäytäntö:                                                                                                                                                 |        |
|   |   |  |  |
| **Ottaa käyttöön, alusta ja muotoilla aseman** | Kunkin yhteyden tallennustilan Host (isäntä)-palvelimen tiedot | Windows-palvelimen nimi:                                                                                                                                                   |        |
|                                        |                                                   | Windows Server IQN:                                                                                                                                                    |        |
|                                        |                                                   | Windows Server-levyn nimi:                                                                                                                                                   |        |
|                                        |                                                   | NTFS käyttöön kohta/kirjain:                                                                                                                                      |        |

## <a name="deployment-prerequisites"></a>Käyttöönoton edellytykset

Seuraavissa osissa kerrotaan StorSimple hallintapalvelu ja StorSimple laitteen määritysten edellytyksistä.

### <a name="for-the-storsimple-manager-service"></a>StorSimple hallinta-palveluun

Ennen kuin aloitat, varmista, että:

- Sinulla on Microsoft-tilisi kanssa tunnistetiedoilla.

- Sinulla on Microsoft Azure tallennustilan tilisi tunnistetiedoilla.

- Microsoft Azure-tilauksesi on käytössä StorSimple hallintapalvelu. Tilauksesi on ostettuja [Enterprise Agreement-sopimus](https://azure.microsoft.com/pricing/enterprise-agreement/).

- Voit käyttää Pääte-emulointi-ohjelmistoa, kuten painovärit, muste.

### <a name="for-the-device-in-the-datacenter"></a>Laitteen joten

Ennen kuin määrität laitteen, varmista seuraavat seikat:

- Laite on täysin avattaviksi otettu käyttöön Teline- ja täysin kytketty power, verkon ja serial access kuvatulla tavalla:

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

Seuraavien toimien Azure perinteinen-portaalissa.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Vaihe 3: Määritä ja rekisteröi Windows PowerShellin kautta laitteen StorSimple

Windows PowerShell StorSimple avulla voit suorittaa StorSimple laitteen ensimmäisen määrityskerran seuraavassa esitetyllä tavalla. Sinun on pääte palvelinemulointiohjelmiston avulla voit suorittaa tämän vaiheen. Lisätietoja on artikkelissa [Käytä painovärit, muste muodostaa laitteen serial konsolin](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-u1](../../includes/storsimple-configure-and-register-device-u1.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Vaihe 4: Suorita pienin laitteen määrittäminen

StorSimple laitteen vähimmäismäärä-kokoonpanossa olet edellyttää: 

- Määritä Toissijainen DNS-palvelin.
- Ota käyttöön iSCSI vähintään yksi verkon liittymän.
- Määrittää kiinteän IP-osoitteiden sekä ohjaimet.

Seuraavien toimien vähimmäismäärä-asennuksen viimeistelemiseen Azure perinteinen-portaalissa.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Vaihe 5: Luo aseman säilö

Äänenvoimakkuuden säilön on tallennustilan tilin ja kaikki sen sisältävät asemat salausasetukset kaistanleveyden. Haluat luoda aseman säilön, ennen kuin aloitat valmistelu tietomääristä StorSimple laitteen. 

Seuraavien toimien luominen aseman säilön Azure perinteinen-portaalissa.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Vaihe 6: Aseman luominen

Kun olet luonut aseman säilön, voit valmistella tallennustilan aseman StorSimple laitteeseen palvelinten. Seuraavien toimien aseman luominen Azure perinteinen-portaalissa.

> [AZURE.IMPORTANT] StorSimple hallinta voi luoda vain levinneet valmistellun asemat. Et voi luoda valmistellun kokonaan tai osittain valmistellun asemat. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

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

Seuraavien toimien ajoitetun varmuuskopion luominen Azure perinteinen-portaalissa.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Voit ottaa manuaalinen varmuuskopiointi milloin tahansa. Siirry ohjeet- [Luo manuaalinen varmuuskopiointi](#create-a-manual-backup). 

## <a name="configure-a-new-storage-account-for-the-service"></a>Uusi tallennustilan tilin-palvelun määrittäminen

Tämä on valinnainen vaihe, sinun on suoritettava vain, jos ei ollut käytössä tallennustilan tilin luominen automaattisesti palveluun. Säilön StorSimple aseman luomiseen tarvitaan Microsoft Azure-tallennustilan tilin.

Jos haluat luoda Azure-tallennustilan tilin toisella alueella, saat [Tietoja Azure tallennustilan tilit](../storage/storage-create-storage-account.md) vaiheittaiset ohjeet.

Seuraavien toimien Azure perinteinen portaalissa **StorSimple hallinta** -sivulle.

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

Seuraavien toimien luominen tarvittaessa yksittäisen aseman manuaalinen varmuuskopiointi StorSimple laitteen Azure perinteinen-portaalissa.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="configure-mpio"></a>Määritä MPIO

Monipolkuisten i/o (MPIO) on valinnainen ominaisuus, ja se ei ole asennettu Windows Server oletusarvoisesti. Se on asennettava ominaisuudeksi kautta Serverin hallinta. Siirry MPIO asennusohjeet [Määrittäminen MPIO StorSimple laitteeseesi](storsimple-configure-mpio-windows-server.md).

Siirry MPIO asennusohjeet Linux isäntään StorSimple laitteen [Määrittäminen MPIO Linux isännän](storsimple-configure-mpio-on-linux.md).


> [AZURE.NOTE] MPIO ei tue StorSimple virtual laitteessa. 



## <a name="next-steps"></a>Seuraavat vaiheet

- Määritä [virtual laite](storsimple-virtual-device-u2.md).

- [StorSimple hallinnan](storsimple-manager-service-administration.md) avulla voit hallita laitetta StorSimple.
 
