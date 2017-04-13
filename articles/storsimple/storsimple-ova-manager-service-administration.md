<properties 
   pageTitle="StorSimple hallinta Virtual matriisin hallinnan | Microsoft Azure"
   description="Opi hallitsemaan StorSimple paikallisen Virtual matriisin käyttämällä StorSimple hallintapalvelu Azure perinteinen-portaalissa."
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

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>StorSimple hallinta-palvelun avulla voit hallita StorSimple Virtual matriisin

![asennuksen prosessinkulku](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan StorSimple hallinta service-käyttöliittymä, mukaan lukien sen ja eri vaihtoehdoista, yhdistämisestä ja linkkejä tiettyjä työnkulkuja, jotka voivat tehdä tämän Käyttöliittymän välityksellä. 

Luettuasi tämän artikkelin saat tietää, miten voit:

- Yhteyden muodostaminen StorSimple hallintapalvelu
- Siirry StorSimple hallinta-Käyttöliittymä
- Hallita StorSimple Virtual matriisin StorSimple hallintapalvelu kautta

> [AZURE.NOTE] Voit tarkastella käytettävissä StorSimple 8000 sarjan laitteen hallinta-asetusten, tutustu [StorSimple hallintapalvelu ammattimainen StorSimple laitteen](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>Yhteyden muodostaminen StorSimple hallintapalvelu

StorSimple hallintapalvelu käytetään Microsoft Azure ja muodostaa yhteyden useisiin StorSimple Virtual matriiseja. Keskitetyn Microsoft Azure perinteinen portaalin selaimessa avulla voit hallita seuraaviin laitteisiin. Voit muodostaa yhteyden StorSimple hallintapalveluun, toimimalla seuraavasti.

#### <a name="to-connect-to-the-service"></a>Voit muodostaa yhteyden palveluun

1. Siirry [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. Käytä Microsoft-tilin tunnistetiedot, kirjaudu sisään Microsoft Azure perinteinen-portaaliin (joka sijaitsee ylös oikealle-ruudun).

3. Vieritä alas käyttämään StorSimple hallintapalvelu vasemmanpuoleisessa siirtymisruudussa.

    ![Siirry palvelun](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>Siirry StorSimple hallintapalvelu Käyttöliittymä

StorSimple hallintapalvelu siirtymis hierarkiaa Käyttöliittymän näkyy seuraavassa taulukossa.

- **StorSimple hallinta** -aloitussivu siirryt Käyttöliittymän palvelun tason sivut koskevat kaikkien Virtual matriisien palvelu kuluessa.

- **Laitteet** -sivun siirryt Laitetaso – Käyttöliittymä sivut koskevat tietyt Virtual matriisi.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple hallinta palvelun siirtymis-hierarkia

|Aloitussivu|Palvelun tason sivut|Laitteen tason sivut|
|---|---|---|
|StorSimple hallinta|Raporttinäkymät-ikkunan (palvelu)|Raporttinäkymät-ikkunan (laite)|
||Laitteiden →|Valvonta|
||Varmuuskopiotiedoston|Osakkeet (tiedostopalvelimeen) tai </br>Asemat (iSCSI-palvelin)|
||Määritä (palvelu)|Määritä (laite)|
||Projektit|Ylläpito|
||Ilmoitukset|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>Suorittaa hallintatehtäviä StorSimple hallinta-palvelun avulla

Seuraavassa taulukossa on yleisiä hallintatehtäviä ja monimutkaisia työnkulut, joita voi käyttää StorSimple hallintapalvelu UI. Näiden tehtävien järjestetään mukaan Käyttöliittymän sivuja, ne on aloitettu.

Lisätietoja jokaiselle työnkululle valitsemalla taulukon ohjeaiheen.

#### <a name="storsimple-manager-workflows"></a>StorSimple hallinnan työnkulut

|Jos haluat tehdä tämä...|Siirry Käyttöliittymän tältä sivulta...|Tällä menetelmällä|
|---|---|---|
|Luo palvelu</br>Palvelun poistaminen</br>Palvelun rekisteröinti Key-tunnuksen hankkiminen</br>Palvelun rekisteröinti avain uudelleen|StorSimple hallinta|[Ottaa käyttöön StorSimple hallintapalvelu](storsimple-ova-manage-service.md)|
|Palvelun tiedot salausavaimen muuttaminen</br>Näytä toiminnot-lokit|StorSimple hallinta palvelun → Raporttinäkymät-ikkunan|[Käytä StorSimple palvelun koontinäyttö](storsimple-ova-service-dashboard.md)|
|Aktivoinnin Virtual matriisi</br>Poista virtuaalikoneen matriisi|StorSimple hallinta palvelun → laitteet|[Poista virtuaalikoneen matriisi tai aktivoinnin poistaminen](storsimple-ova-deactivate-and-delete-device.md)|
|Tietojen palauttaminen ja laitteen automaattisesti</br>Automaattisesti edellytykset</br>Virtuaalinen laitteen automaattisesti</br>Liiketoiminnan jatkuvuus palauttaminen (BCDR)</br>Virheet aikana palauttaminen|StorSimple hallinta palvelun → laitteet|[StorSimple Virtual matriisin tietojen palauttaminen ja laitteen automaattisesti](storsimple-ova-failover-dr.md)|
|Osakkeet ja asemat varmuuskopiointi</br>Ota manuaalinen varmuuskopiointi</br>Varmuuskopion aikataulun muuttaminen</br>Tarkastele varmuuskopioon|StorSimple hallinta palvelun → varmuuskopioinnin luettelo|[StorSimple Virtual matriisin varmuuskopiointi](storsimple-ova-backup.md)|
|Palauttaa osakkeet varmuuskopioinnin määrittäminen</br>Palauttaa tietomääristä varmuuskopioinnin määrittäminen</br>Kohdetason palauttaminen (vain tiedoston server)|StorSimple hallinta palvelun → varmuuskopioinnin luettelo|[Palauttaminen varmuuskopiosta StorSimple Virtual matriisin](storsimple-ova-restore.md)|
|Tietoja tallennustilan</br>Tallennustilan tilin lisääminen</br>Tallennustilan tilin muokkaaminen</br>Tallennustilan tilin poistaminen|StorSimple hallinta palvelun → määrittäminen|[Tallennustilan tilien hallinta StorSimple Virtual taulukolle](storsimple-ova-manage-storage-accounts.md)|
|Access-ohjausobjektin tietueista</br>Lisääminen tai muokkaaminen access-ohjausobjektin tietue </br>Access-ohjausobjektin tietueen poistaminen|StorSimple hallinta palvelun → määrittäminen|[Access-ohjausobjektin tietueiden hallintaa StorSimple Virtual matriisi](storsimple-ova-manage-acrs.md)|
|Työn tarkasteleminen|StorSimple hallinta palvelun → työt| [StorSimple Virtual matriisin töiden hallinta](storsimple-ova-manage-jobs.md)|
|Ilmoitusasetusten määrittäminen</br>Ilmoitusten vastaanottaminen</br>Ilmoitusten hallinta</br>Tarkista ilmoitukset|StorSimple hallinta palvelun → ilmoitukset|[Tarkastele ja hallitse ilmoituksia StorSimple Virtual taulukolle](storsimple-ova-manage-alerts.md)|
|Muokkaa laitteen järjestelmänvalvojan salasana|StorSimple hallinta palvelun → laitteiden → määrittäminen|[StorSimple Virtual matriisin laitteen järjestelmänvalvojan salasanan vaihtaminen](storsimple-ova-change-device-admin-password.md)|
|Asenna saatavilla olevat ohjelmistopäivitykset|StorSimple hallinta palvelun → laitteiden → ylläpito|[Päivitä Virtual matriisi](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] Tarvitset [paikallisen sivuston Käyttöliittymä](storsimple-ova-web-ui-admin.md) seuraavat toimet:
>
>- [Palvelun tiedot Salausavaimen noutaminen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Tuen paketin luominen](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Lopeta ja Käynnistä uudelleen Virtual matriisi](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja sivuston Käyttöliittymä ja miten sitä käytetään Siirry [käyttämään StorSimple web Käyttöliittymän ammattimainen StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).
