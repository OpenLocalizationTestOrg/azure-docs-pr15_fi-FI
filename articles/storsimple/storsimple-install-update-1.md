<properties
   pageTitle="Asenna päivitys 1.2 StorSimple laitteen | Microsoft Azure"
   description="Kerrotaan, miten voit asentaa StorSimple 8000 sarjan päivitys 1.2 StorSimple 8000 sarjan laitteeseen."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Asenna päivitys 1.2 StorSimple laitteen

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten voit asentaa päivityksen 1.2 StorSimple-laitteeseen, jossa on käytössä ohjelmiston-version ennen päivitystä 1. Opetusohjelman kattaa myös pakollinen päivityksen, kun yhdyskäytävä on määritetty verkko-liittymän kuin tietojen 0 StorSimple laitteen lisätoimia.

Päivitys 1.2 sisältää laitteen ohjelmistopäivitykset, LSI päivityksistä ja levyn laitteisto päivitykset. Ohjelmiston ja LSI päivitysten-häiritsevä päivityksiä ja voi suojata Azure perinteinen portaalin kautta. Levyn laitteisto-päivitykset ovat häiritsevä päivitykset ja voi käyttää vain laitteen Windows PowerShell-liittymän kautta.

Sen mukaan, mitä versiota laite on käynnissä, voit määrittää, jos päivitys 1.2 käytetään. Voit tarkistaa laitteen ohjelmiston version siirtymällä kohtaan **quick glance** laitteesi **raporttinäkymät-ikkunan**.

</br>

| Jos ohjelmiston versiota...   | Mitä tapahtuu portaalin?                              |
|---------------------------------|--------------------------------------------------------------|
| Vapauta - GA                    | Jos käytössäsi on julkaisuversio (GA), eivät vaikuta päivitys. Varmista, että voit päivittää laitteen [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) .|
| Päivitä 0,1                      | Portaalin koskee päivityksen 1.2.                                |
| Päivitä 0,2                      | Portaalin koskee päivityksen 1.2.                                |
| Päivitä 0,3                      | Portaalin koskee päivityksen 1.2.                                |
| Päivitys 1                        | Tämä päivitys ei ole käytettävissä.                           |
| Päivitä 1.1                      | Tämä päivitys ei ole käytettävissä.                           |

</br>

> [AZURE.IMPORTANT]

> -  Päivitä 1.2 ei välttämättä näy heti, koska olemme Tee vaiheistettu rahoittaja muutokset. Etsi uudelleen muutaman päivän päivityksiä tämän päivityksen tulee saataville pian.
> - Tämä päivitys sisältää manuaalinen ja automaattinen vanhat tarkistukset laite-terveydentilasta kannalta laitteiston tila ja verkko-yhteys. Vanhat tarkistukset suoritetaan vain, jos käytät perinteistä Azure-portaalista päivitykset.
> - On suositeltavaa, että asennat ohjelmistojen ja ohjaimen päivitykset Azure perinteinen portaalin kautta. Windows PowerShell-liittymän (asentamaan päivitykset) laitteen pitäisi mennä vain, jos portaalissa epäonnistuu päivitystä edeltävässä yhdyskäytävä-valintaruutu. Päivitykset saattaa kestää 5 – 10 tuntia asentaminen (mukaan lukien Windows-päivitysten). Ylläpidon tila-päivitykset on oltava asennettuna laitteen Windows PowerShell-liittymän kautta. Ylläpidon tila-päivitykset ovat häiritsevä päivitykset, nämä johtaa laitteen alaspäin aika.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Asenna päivitys 1.2 Azure perinteinen portaalin kautta

Seuraavien toimien päivittämään laitteen [Päivitä 1.2](storsimple-update1-release-notes.md). Tämän toimintosarjan avulla vain, jos sinulla on määritetty tietojen 0 verkon liittymän laitteen yhdyskäytävä.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Varmista, että laite toimii **StorSimple 8000 sarjan päivitys 1.2 (6.3.9600.17584)**. **Päivitetty viimeksi päivämäärän** myös kannattaa muokata. Näet myös, että ylläpidon tila-päivitykset ovat käytettävissä (tämä viesti voi jatkaa, joka näkyy 24 tuntia päivitysten asentamisen jälkeen).

    Ylläpidon tila päivityksiä häiritsevä päivityksiä, jotka aiheuttavat laitteen käyttökatkot ja voi käyttää vain laitteesi Windows PowerShell-liittymän kautta.

    ![Ylläpitosivun] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Ylläpitosivun")

13. Lataa ylläpidon tila päivitykset käyttämällä [Voit ladata korjausten]( #to-download-hotfixes) lueteltuja vaiheita etsimiseen ja lataamiseen KB3063416, mitkä asentaa levyn laitteisto päivitykset (muut päivitykset on oltava asennettuna nyt mukaan).

13. Noudata seuraavia [asentaminen ja tarkista ylläpidon tila korjausten](#to-install-and-verify-maintenance-mode-hotfixes) ylläpidon tila-päivitysten asentaminen.

14. Azure perinteinen portaalissa siirtymistä **ylläpito** -sivulle ja sivun alalaidassa, valitsemalla minkä tahansa Windows-päivitysten tarkistaminen ja valitse sitten **Asenna päivitykset** **Tarkista päivitykset** . Olet valmis, kun kaikki päivitykset on asennettu.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Asenna päivitys 1.2 laitteissa, joissa on määritetty tietojen 0 verkko-liittymän yhdyskäytävän

Tämä toiminto on käytettävä vain, jos epäonnistua yhdyskäytävä-valintaruutu, kun yritän asentaa päivitykset palvelun Azure perinteinen-portaalissa. Tarkista epäonnistuu, sinulla on määritetty tietojen 0 verkon-liittymän yhdyskäytävän ja laite toimii päivitys 1 ennen ohjelmistoversio. Jos laite ei ole yhdyskäytävän tiedot 0 verkko-liittymän, voit päivittää laitteesi suoraan Azure perinteinen portaalin. Kohdassa [Install Päivitä 1.2 Azure perinteinen portaalin kautta](#install-update-1.2-via-the-azure-classic-portal).

Ohjelmistoversiot, jotka voidaan päivittää tällä menetelmällä ovat 0,1 päivityksen, Päivitä 0,2 ja Päivitä 0,3.


> [AZURE.IMPORTANT]
>
> - Jos laite toimii Release (GA)-version, ota yhteyttä [Microsoft-tuen](storsimple-contact-microsoft-support.md) auttamaan päivitys.
> - Tämä toiminto on tehtävä vain kerran sovelletaan päivityksen 1.2. Voit käyttää Azure perinteinen portaalissa seuraavien päivitysten.

Jos laite toimii päivitystä edeltävässä 1 ohjelmiston ja siinä on yhdyskäytävän määrittäminen verkko-liittymän kuin tietojen 0, voit käyttää päivityksen 1.2 kaksi seuraavasti:

- **Vaihtoehto 1**: lataa päivitys ja lisätä ne käyttämällä `Start-HcsHotfix` laitteen käyttöliittymästä Windows PowerShellin cmdlet-komento. Tämä on suositeltava. **Älä käytä tätä menetelmää voit käyttää päivityksen 1.2, jos laitteesi käyttöjärjestelmänä on päivitys 1.0 tai Päivitä 1.1.**

- **Vaihtoehto 2**: Poista yhdyskäytävä-määritys ja asenna päivitys suoraan Azure perinteinen portaalin.


Lisätietoja kaikkien näiden on käytettävissä seuraavissa osissa.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Vaihtoehto 1: Käytä Windows PowerShell StorSimple sovelletaan päivityksen 1.2 kuin korjaus

Tämä toiminto on käytettävä vain, jos käytössäsi on päivitys 0,1-0,2, 0,3 ja jos yhdyskäytävä-valintaruudun epäonnistui, kun yritän asentaa päivitykset perinteinen Azure-portaalista. Jos käytössäsi on Release (GA) ohjelmiston, ota [Microsoft-tuen](storsimple-contact-microsoft-support.md) ja päivittää laitteen.

Asennettava päivitys 1.2 korjaustiedoston, lataa ja seuraavat korjausten asentaminen:

| Tilauksen  | KT        | Kuvaus             | Päivityksen tyyppi  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Ohjelmistopäivitys         |  Normaali     |
| 2      | KB3043005 | LSI SAS ohjauskoneen päivitys |  Normaali     |
| 3      | KB3063416 | Levyn laitteisto           | Ylläpito  |

Ennen tämän toiminnon avulla voit päivityksen, varmista seuraavat seikat:

- Laitteen molemmat ohjaimet ovat online-tilassa.

Seuraavien toimien sovelletaan päivityksen 1.2. **Päivitykset voi kestää noin 2 tuntia suorittamiseen (noin 30 minuuttia-ohjelmiston 30 minuuttia ohjaimen levyn laitteisto 45 minuuttia).**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Vaihtoehto 2: Käytä Azure perinteinen portaalin avulla päivityksen 1.2 poistettuasi yhdyskäytävän määritykset

Tämä koskee vain StorSimple-laitteet, joka on käytössä ohjelmiston-version ennen päivitystä 1 ja määrittää verkko-liittymän kuin tietojen 0 yhdyskäytävä on. Sinun on ennen päivittämistä yhdyskäytävä-asetuksen valinta.

Päivitys voi kestää muutaman tunnin suorittamiseen. Jos yhteyttä isännät ovat eri aliverkosta, iSCSI liittymät yhdyskäytävä-määritysten poistaminen saattaa johtaa käyttökatkot. Suosittelemme, että määrität voit pienentää käyttökatkot 0 iSCSI tietoliikenteen tietojen.

Seuraavien toimien käytöstä Gatewayn Verkkokäyttöliittymän ja sitten päivityksen.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [Update 1.2-versiossa](storsimple-update1-release-notes.md).
