<properties
   pageTitle="Luo StorSimple tuki paketti | Microsoft Azure"
   description="Lue, miten voit luoda ja salauksen Muokkaa tuki paketin StorSimple laitteeseesi."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />


# <a name="create-and-manage-a-storsimple-support-package"></a>Voit luoda ja hallita StorSimple tuki-paketti

## <a name="overview"></a>Yleiskatsaus

StorSimple tuki paketti on helppo käyttää-järjestelmää, johon kerätään asiaa virhelokit StorSimple laitteen vianmääritys minkä tahansa Microsoft Support avustaminen. Kerättyjen lokit salataan ja pakata.

Tässä opetusohjelmassa on vaiheittaiset ohjeet, voit luoda ja hallita tuki-paketti.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Luo ja Azure perinteinen portaalissa tuki paketin lataaminen

Voit luoda ja tuki-paketin lataaminen Microsoft Support sivuston **ylläpito** -sivun Azure perinteinen portal-palvelun kautta.

> [AZURE.NOTE] Lataus edellyttää tuki salasanaa. Oman tukihenkilö annettava tämä sinulle sähköpostiviestin.

Salattujen ja pakattu tuki-paketti (.cab-tiedosto) luodaan ja tuki-sivustoon. Tukihenkilö sitten voit hakea tämän paketin tukisivustossa ongelman vianmäärityksessä.

Seuraavien toimien perinteinen portaalin tuki paketin luominen.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Tuen pakkauksen luominen Azure perinteinen-portaalissa

1. Valitse **laitteet** > **ylläpito**.

2. Valitse **tukevat paketti** -osassa **Luo ja lataa tuki-paketti**.

3. **Luo ja lataa tuki-paketti** -valintaikkunassa seuraavasti:

    ![Tuen paketin luominen](./media/storsimple-create-manage-support-package/IC740923.png)

    - Kirjoita **Tuki salasana** -ruutuun salasana. Microsoft-Tukihenkilö lähettää sinulle tätä salasanaa sähköpostitse.

    - Valitse valintaruutu, jos haluat antaa suostumus automaattinen lataaminen tuki-paketin Microsoft Support-sivustossa.

    - Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-create-manage-support-package/IC740895.png).


## <a name="manually-create-a-support-package"></a>Tuen paketin luominen manuaalisesti

Joissakin tapauksissa sinun on luoda manuaalisesti StorSimple tuki-paketin Windows PowerShellin kautta. Esimerkki:

- Jos tarvitset luottamuksellisten tietojen poistaminen log tiedostojen jakaminen Microsoft Support ennen.

- Jos sinulla on ongelmia ladataan paketin yhteysongelmien vuoksi.

Voit jakaa manuaalisesti luotu tuki-paketin Microsoft Support kanssa sähköpostin välityksellä. Seuraavien toimien tuki paketin luominen Windows PowerShellin StorSimple varten.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Windows PowerShellin tuki paketin luominen StorSimple

1. Windows PowerShell-istunnon aloittaminen etätietokoneessa, jota käytetään StorSimple laitteeseesi järjestelmänvalvojana, kirjoita seuraava komento:

    `Start PowerShell`

2. Windows PowerShell-istunnossa yhdistää laitteesi SSAdmin konsoliin:

    - Kirjoita komentoriville seuraava komento:

        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`

    1. Kirjoita näkyviin tulevan valintaikkunan laitteesi järjestelmänvalvojan salasanan. Oletus-salasana on:

        `Password1`

        ![Tunnistetietojen PowerShell-valintaikkuna](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Valitse **OK**.
    1. Kirjoita komentoriville seuraava komento:

        `Enter-PSSession $MS`

3. Istunnon aikana, joka avautuu Kirjoita haluamasi komennon.

    - Verkkoresursseja, joka on suojattu salasanalla Kirjoita:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        Sinua pyydetään salasanaa, polku verkkokansioon ja salauksen salasana, (koska tuki-paketti on salattu). Tuki-paketin sitten luodaan määritettyyn kansioon.

    - Saat osakkeet, joita ei ole suojattu salasanalla, sinun ei tarvitse `-Credential` parametria. Anna seuraavat tiedot:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        Tuki-paketin luodaan molemmat ohjaimet verkon jaettuun kansioon. Se on salattu, pakattu tiedosto, joka voidaan lähettää Microsoft Support vianmääritystä varten. Katso lisätietoja, [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md).


### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Vie HcsSupportPackage cmdlet-parametrit
Voit käyttää seuraavia parametreja Vie HcsSupportPackage cmdlet-komento.

| Parametri            | Pakollinen/Valinnainen | Kuvaus                                                                                                                                                             |
|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Path`                 | Pakollinen          | Anna sijainti, johon tuki-paketin sijoitetaan verkon jaetun kansion avulla.                                                                 |
| `-EncryptionPassphrase` | Pakollinen          | Käytä antamaan salasana salaa tuki-paketin avulla.                                                                                                        |
| `-Credential`           | Valinnainen          | Avulla verkon jaetun kansion käyttöoikeudet.                                                                                        |
| `-Force`                | Valinnainen          | Avulla voit ohittaa vaiheen salauksen salasana vahvistus.                                                                                                                |
| `-PackageTag`           | Valinnainen          | Määritä kansion *polku* , jossa tuki-paketin sijoitetaan käyttämällä. Oletusarvo on [laitteen nimi]-[nykyisen päivämäärän ja time:yyyy-MM-dd-HH-mm-ss].       |
| `-Scope`                | Valinnainen          | Määritä **klusterin** (oletus) tuki pakkauksen luominen sekä ohjaimet. Jos haluat luoda paketti vain nykyisen ohjaimen, Määritä **ohjauskoneen**. |


## <a name="edit-a-support-package"></a>Muokkaa tuki-paketti

Kun olet luonut tuki-paketti, voit joutua muuttamaan paketin luottamuksellisten tietojen muokkaaminen. Tämä voi olla aseman nimet, laite IP-osoitteet ja lokitiedostojen varmuuskopion nimet.

> [AZURE.IMPORTANT] Voit muokata vain tuki-paketti, joka on luotu StorSimple Windows PowerShellin kautta. Et voi muokata luotu StorSimple hallintapalvelu Azure perinteinen portaaliin paketin.

Voit muokata tuki-paketin ennen lataamista Microsoft Support-sivustossa, salauksen ensin tuki-paketti, muokata tiedostoja ja salaa sitä sitten uudelleen. Suorita seuraavat vaiheet.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Voit muokata Windows PowerShellin tuki paketin StorSimple varten

1. Luo tuki-paketti, kuvatun aiemmin [Windows PowerShell StorSimple-tuki pakkauksen luominen](#to-create-a-support-package-in-windows-powershell-for-storsimple).

2. [Lataa komentosarja](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) paikallisesti asiakkaalle.

3. Voit tuoda Windows PowerShell-moduulin. Määritä polku paikallisen kansioon, johon olet ladannut komentosarja. Voit tuoda moduulin kirjoittamalla:

    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. Kaikki tiedostot ovat *.aes* tiedostoja, jotka on pakattu ja salattu. Jos haluat purkaa ja salauksen tiedostot, kirjoita:

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    Huomaa, että todellinen tiedostotunnisteet näkyvät nyt kaikki tiedostot.

    ![Muokkaa tuki-paketti](./media/storsimple-create-manage-support-package/IC750706.png)

5. Kun sinua pyydetään salauksen salasana, kirjoita salasana, jota käytit tuki-paketin luonnin yhteydessä.

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:EncryptionPassphrase: ****

6. Etsi kansio, joka sisältää lokitiedostot. Lokitiedostojen on nyt puretaan ja salaus, koska ne on alkuperäinen tiedostotunnisteet. Nämä tiedostot poistetaan asiakaskohtainen tietoja, esimerkiksi volyymikäyttöoikeuksien nimet ja laitteen IP-osoitteet, muokata ja tallentaa tiedostoja.

7. Sulje tiedostoja pakata gzip ja salaa ne AES-256. Tämä on nopeuden ja tietoturvasta siirtäminen tuki-paketin verkon kautta. Pakkaa ja salata tiedostoja, anna seuraavat tiedot:

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![Muokkaa tuki-paketti](./media/storsimple-create-manage-support-package/IC750707.png)

8. Anna kehotettaessa muokattu tuki-paketin salaus-salasana.

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. Kirjoita uusi salasana muistiin niin, että voit jakaa sen Microsoft Support pyydettäessä.


### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Esimerkki: Salasanasuojattuja jaettuun tuki-paketin-tiedostojen muokkaamiseen

Seuraavassa esimerkissä esitetään, miten salaus, Muokkaa ja salata uudelleen tuki-paketti.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [tuki-paketit ja laitteen lokit vianmääritys laitteen käyttöönoton](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting)käyttäminen.

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
