<properties 
   pageTitle="StorSimple hallinta-palvelun hallinta | Microsoft Azure"
   description="Opi hallitsemaan StorSimple laitteen käyttämällä StorSimple hallintapalvelu Azure perinteinen-portaalissa."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>StorSimple hallinta-palvelun avulla voit hallita StorSimple laitteen

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan StorSimple hallinnan palvelun-liittymän, voit muodostaa yhteyden eri vaihtoehtoja ja linkkejä tiettyjä työnkulkuja, jotka voivat tehdä tämän Käyttöliittymän välityksellä, mukaan lukien. Nämä ohjeet on käytettävissä sekä; StorSimple fyysisiä ja virtual laite.

Opit luettuasi tämän artikkelin avulla:

- Yhteyden muodostaminen StorSimple hallinta
- Siirry StorSimple hallinta-Käyttöliittymä
- Hallita StorSimple laitteen StorSimple hallintapalvelu kautta


## <a name="connect-to-storsimple-manager-service"></a>Yhteyden muodostaminen StorSimple hallinta

StorSimple hallintapalvelu käytetään Microsoft Azure ja muodostaa yhteyden StorSimple useilla eri laitteilla. Keskitetyn Microsoft Azure perinteinen portaalin selaimessa avulla voit hallita seuraaviin laitteisiin. Voit muodostaa yhteyden StorSimple hallintapalveluun, toimimalla seuraavasti.

#### <a name="to-connect-to-the-service"></a>Voit muodostaa yhteyden palveluun

1. Siirry [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. Käytä Microsoft-tilin tunnistetiedot, kirjaudu sisään Microsoft Azure perinteinen-portaaliin (joka sijaitsee ylös oikealle-ruudun).

1. Vieritä alas käyttämään StorSimple hallintapalvelu vasemmanpuoleisessa siirtymisruudussa.


## <a name="navigate-storsimple-manager-service-ui"></a>Siirry StorSimple hallintapalvelu Käyttöliittymä

StorSimple hallintapalvelu siirtymis hierarkiaa Käyttöliittymän näkyy seuraavassa taulukossa.

- **StorSimple hallinta** -aloitussivu siirryt Käyttöliittymän palvelun tason sivut koskevat sisällä palvelun kaikissa laitteissa.

- **Laitteiden** sivun siirryt Laitetaso – Käyttöliittymä sivut sovellettava laitteen.

- **Äänenvoimakkuuden säilöjen** sivun siirryt äänenvoimakkuutta-sivu, jolla näkyy kaikki liittyvät laitteen asemat.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple hallinta palvelun siirtymis-hierarkia

|Aloitussivu|Palvelun tason sivut|Laitteen tason sivut|Laitteen tason sivut|
|---|---|---|---|
|StorSimple hallinta|Palvelun koontinäyttö|Laitteen Raporttinäkymät-ikkunan||
||Laitteiden →|Valvonta|
||Varmuuskopiotiedoston|Äänenvoimakkuuden containers→|Tietomääristä|
||Määritä (palvelu)|Varmuuskopioinnin määrittäminen||
||Projektit|Määritä (laite)|
||Ilmoitukset|Ylläpito|

![Video käytettävissä](./media/storsimple-manager-service-administration/Video_icon.png) **Video käytettävissä**

Katso video, jossa esitellään StorSimple hallinnan service-käyttöliittymä, napsauta [tätä](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Hallita StorSimple laitteen StorSimple hallinta

Seuraavassa taulukossa on yleisiä hallintatehtäviä ja monimutkaisia työnkulut, joita voi käyttää StorSimple hallintapalvelu UI. Näiden tehtävien järjestetään mukaan Käyttöliittymän sivuja, ne on aloitettu.

Lisätietoja jokaiselle työnkululle valitsemalla taulukon ohjeaiheen.

#### <a name="storsimple-manager-workflows"></a>StorSimple hallinnan työnkulut

|Jos haluat tehdä tämä...|Siirry Käyttöliittymän tältä sivulta...|Näiden ohjeiden avulla.|
|---|---|---|
|Luo palvelu</br>Palvelun poistaminen</br>Hae palvelun rekisteröinti avain</br>Palvelun rekisteröinti avain uudelleen|StorSimple hallinta|[Ottaa käyttöön StorSimple hallintapalvelu](storsimple-manage-service.md)
|Palvelun tiedot salausavaimen muuttaminen</br>Näytä toiminto-lokit|StorSimple hallinta palvelun → Raporttinäkymät-ikkunan|[Käytä StorSimple hallinta-palvelun koontinäyttö](storsimple-service-dashboard.md)|
|Laitteen aktivoinnin poistaminen</br>Poista laitteen|StorSimple hallinta palvelun → laitteet|[Poista laitteen tai aktivoinnin poistaminen](storsimple-deactivate-and-delete-device.md)|
|Lisätietoja tietojen palauttaminen ja laitteen automaattisesti</br>Laitteen fyysinen automaattisesti</br>Virtuaalinen laitteen automaattisesti</br>Liiketoiminnan jatkuvuus palauttaminen (BCDR)|StorSimple hallinta palvelun → laitteet|[Laitteen StorSimple automaattisesti ja tietojen palauttaminen](storsimple-device-failover-disaster-recovery.md)|
|Luettelon varmuuskopioiden aseman</br>Valitse varmuuskopioinnin määrittäminen</br>Poista varmuuskopioinnin määrittäminen|StorSimple hallinta palvelun → varmuuskopioinnin luettelo|[Varmuuskopioiden hallinta](storsimple-manage-backup-catalog.md)|
|Kloonaa aseman|StorSimple hallinta palvelun → varmuuskopioinnin luettelo|[Kloonaa aseman](storsimple-clone-volume.md)|
|Palauttaa varmuuskopiointi|StorSimple hallinta palvelun → varmuuskopioinnin luettelo|[Palauttaa varmuuskopiointi](storsimple-restore-from-backup-set.md)|
|Tietoja tallennustilan</br>Tallennustilan tilin lisääminen</br>Tallennustilan tilin muokkaaminen</br>Tallennustilan tilin poistaminen</br>Tallennustilan tilien avaimen kierto|StorSimple hallinta palvelun → määrittäminen|[Tallennustilan tilien hallinta](storsimple-manage-storage-accounts.md)|
|Tietoja kaistanleveyden malleista</br>Lisää kaistanleveyden malli</br>Muokkaa kaistanleveyden mallia</br>Kaistanleveyden mallin poistaminen</br>Käyttää oletusmallia, kaistanleveyden</br>Koko päivän, joka käynnistää määritettynä ajankohtana kaistanleveyden-mallin luominen|StorSimple hallinta palvelun → määrittäminen|[Kaistanleveyden Jakeluryhmien hallinta](storsimple-manage-bandwidth-templates.md)|
|Access-ohjausobjektin tietueista</br>Access-ohjausobjektin tietueen luominen</br>Access-ohjausobjektin tietueen muokkaaminen</br>Access-ohjausobjektin tietueen poistaminen|StorSimple hallinta palvelun → määrittäminen|[Accessin ohjausobjektin tietueiden hallinta](storsimple-manage-acrs.md)|
|Työn tarkasteleminen</br>Työn peruuttaminen|StorSimple hallinta palvelun → työt|[Töiden hallinta](storsimple-manage-jobs.md)
|Ilmoitusten vastaanottaminen</br>Ilmoitusten hallinta</br>Tarkista ilmoitukset|StorSimple hallinta palvelun → ilmoitukset|[Tarkastella ja hallita StorSimple ilmoitukset](storsimple-manage-alerts.md)
|Tarkastele yhdistetyn laitteistokäynnistäjät</br>Etsi laitteen sarjanumero</br>Etsi kohde IQN|StorSimple hallinta palvelun → laitteiden → Raporttinäkymät-ikkunan|[Käytä StorSimple laitteen Raporttinäkymät-ikkunan](storsimple-device-dashboard.md)|
|Seurannan kaavioiden luominen|StorSimple hallinta palvelun → laitteiden → valvonta|[Valvoa StorSimple laitteen](storsimple-monitor-device.md)|
|Lisää äänenvoimakkuutta-säilö</br>Muokata asemaa säilö</br>Äänenvoimakkuuden säilön poistaminen|StorSimple hallinta palvelun → laitteiden → äänenvoimakkuuden säilöt|[Äänenvoimakkuuden säilöjen hallinta](storsimple-manage-volume-containers.md)|
|Lisää aseman</br>Muokata asemaa</br>Tilavuus offline-tilaan</br>Aseman poistaminen</br>Näytön aseman|StorSimple hallinta palvelun → laitteiden → äänenvoimakkuuden säilöjen → tietomääristä|[Hallitse tietomääristä](storsimple-manage-volumes.md)|
|Laitteen asetusten muokkaaminen</br>Aika-asetusten muokkaaminen</br>DNS.md asetusten muokkaaminen</br>Määritä verkon liityntäkohdat|StorSimple hallinta palvelun → laitteiden → määrittäminen|[Laitteen määrittäminen laitteeseesi StorSimple muokkaaminen](storsimple-modify-device-config.md)|
|Näytä web-välityspalvelimen|StorSimple hallinta palvelun → laitteiden → määrittäminen|[Web-välityspalvelimen laitteen määrittäminen](storsimple-configure-web-proxy.md)|
|Muokkaa laitteen järjestelmänvalvojan salasana</br>Muokkaa StorSimple tilannevedoksen hallinta salasanan|StorSimple hallinta palvelun → laitteiden → määrittäminen|[Muuta StorSimple salasanat](storsimple-change-passwords.md)|
|Määritä etähallinta|StorSimple hallinta palvelun → laitteiden → määrittäminen|[Etäkäyttö StorSimple-laitteeseen](storsimple-remote-connect.md)|
|Ilmoitusasetusten määrittäminen|StorSimple hallinta palvelun → laitteiden → määrittäminen|[Tarkastella ja hallita StorSimple ilmoitukset](storsimple-manage-alerts.md)|
|Määritä CHAP StorSimple laitteeseesi|StorSimple hallinta palvelun → laitteiden → määrittäminen|[Määritä CHAP StorSimple laitteeseesi](storsimple-configure-chap.md)|
|Lisää varmuuskopion käytäntö</br>Lisää tai Muokkaa aikataulua</br>Poista varmuuskopioinnin käytäntö</br>Ota manuaalinen varmuuskopiointi</br>Mukautetun varmuuskopio-käytännön luominen useita asemat ja aikatauluja|StorSimple hallinta palvelun → laitteiden → varmuuskopiointi käytännöt|[Varmuuskopion käytäntöjen hallinta](storsimple-manage-backup-policies.md)|
|Lopeta laitteen ohjaimet</br>Käynnistä laite ohjaimet</br>Sulje laitteen ohjaimet</br>Palauttaa laitteen tehdasasetukset</br>(Edellä ovat vain paikallisen laitteen)|StorSimple hallinta palvelun → laitteiden → ylläpito|[Hallitse StorSimple laitteen ohjain](storsimple-manage-device-controller.md)|
|Lisätietoja StorSimple laitteet</br>Näytön laitteiston tila</br>(Edellä ovat vain paikallisen laitteen)|StorSimple hallinta palvelun → laitteiden → ylläpito|[Valvonta-laitteet](storsimple-monitor-hardware-status.md)|
|Tuen paketin luominen|StorSimple hallinta palvelun → laitteiden → ylläpito|[Voit luoda ja hallita tuki-paketti](storsimple-create-manage-support-package.md)|
|Asenna saatavilla olevat ohjelmistopäivitykset|StorSimple hallinta palvelun → laitteiden → ylläpito|[Päivittää laitteen](storsimple-update-device.md)|


##<a name="next-steps"></a>Seuraavat vaiheet
Jos kohtaat ongelmia StorSimple laitteen päivittäisestä tai sen laitteisto-osia, lue lisätietoja:

- [Toiminnallisia laitteen vianmääritys](storsimple-troubleshoot-operational-device.md)
- [Käytä StorSimple ilmaisin merkkivalot seuranta](storsimple-monitoring-indicators.md)

Jos et pysty ratkaisemaan ongelmat eikä sinun tarvitse Luo palvelupyyntö, [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md)viitata.
