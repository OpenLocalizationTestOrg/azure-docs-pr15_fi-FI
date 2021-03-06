<properties
   pageTitle="Asenna päivitys 2.2 StorSimple-laitteessa | Microsoft Azure"
   description="Kerrotaan, miten voit asentaa StorSimple 8000 sarjan päivitys 2.2 StorSimple 8000 sarjan laitteeseen."
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
   ms.date="08/02/2016"
   ms.author="alkohli" />

# <a name="install-update-22-on-your-storsimple-device"></a>Asenna päivitys 2.2 StorSimple laitteen

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten voit asentaa päivityksen 2.2 StorSimple laitteeseen käytössä ohjelmistoversiota kautta Azure perinteinen portaalin ja korjaus-menetelmällä. Korjaus-menetelmää käytetään, kun yhdyskäytävä on määritetty verkko-liittymän kuin StorSimple laitteen tiedot 0 ja yrität päivittää päivitystä edeltävässä 1 ohjelmiston versiosta.

Päivitys 2.2 sisältää Laiteohjelmisto, WMI ja iSCSI päivitykset. Jos päivitys-versio 2.1, vain laitteen ohjelmistopäivitys on käytettäväksi. Jos päivität päivitystä edeltävässä 2 versiosta, voit myös edellyttää LSI ohjaimen, Spaceport, Storport ja levyn laitteisto päivitykset. Esimerkiksi Laiteohjelmisto WMI, iSCSI, LSI ohjaimen, Spaceport ja Storport korjaukset-häiritsevä päivityksiä ja voi suojata Azure perinteinen portaalin kautta. Levyn laitteisto-päivitykset ovat häiritsevä päivitykset ja voi käyttää vain laitteen Windows PowerShell-liittymän kautta. 

> [AZURE.IMPORTANT]

> - Manuaalinen ja automaattinen vanhat tarkistukset joukko tehdään ennen asennusta kannalta laitteen tila ja verkon connectivity laite-terveydentilasta. Vanhat tarkistukset suoritetaan vain, jos käytät perinteistä Azure-portaalista päivitykset.
> - On suositeltavaa, että asennat ohjelmistojen ja ohjaimen päivitykset Azure perinteinen portaalin kautta. Windows PowerShell-liittymän (asentamaan päivitykset) laitteen pitäisi mennä vain, jos portaalissa epäonnistuu päivitystä edeltävässä yhdyskäytävä-valintaruutu. Olet päivittämässä versio riippuen päivitykset saattaa kestää 1,5 2,5 tunteja asentamiseksi. Ylläpidon tila-päivitykset on oltava asennettuna laitteen Windows PowerShell-liittymän kautta. Ylläpidon tila-päivitykset ovat häiritsevä päivitykset, nämä johtaa laitteen alaspäin aika.
> - Jos käynnissä valinnainen StorSimple tilannevedoksen hallinta, varmista, että olet päivittänyt tilannevedoksen hallinta-versiosi päivityksen 2.2 ennen päivittämistä laite.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>Asenna päivitys 2.2 Azure perinteinen portaalin kautta

Seuraavien toimien päivittämään laitteen [Päivitä 2.2](storsimple-update21-release-notes.md).


> [AZURE.NOTE]
Käytettäessä Update 2 tai uudempi (kuten päivityksen 2.1), Microsoft voi tuoda lisää vianmääritystiedot laitteesta. Tuloksena, kun toimintojen tiimimme tunnistaa laitteet, jotka on ongelmia, emme paremmin varustettu tietojen keräämiseen laitteesta ja vianmääritys. Päivityksen 2 tai uudempi hyväksymällä Salli että ennakoiva tukevat.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Varmista, että laite toimii **StorSimple 8000 sarjan päivitys 2.2 (6.3.9600.17708)**. **Päivitetty viimeksi päivämäärän** myös kannattaa muokata. 

    Jos päivität Update 2: a vanhemmalla versiolla, näet myös, että saatavilla olevat päivitykset ylläpito-tilassa (tämä viesti voi jatkaa, joka näkyy 24 tuntia päivitysten asentamisen jälkeen).

    Ylläpidon tila päivityksiä häiritsevä päivityksiä, jotka aiheuttavat laitteen käyttökatkot ja voi käyttää vain laitteesi Windows PowerShell-liittymän kautta. Joissakin tapauksissa päivityksen 1.2 suoritettaessa levyn laitteisto jo ehkä ajan tasalla, jolloin sinun ei tarvitse asentaa päivityksiä ylläpidon tila.

    Jos päivität päivityksen 2-laitteen pitäisi nyt olla ajan tasalla. Voit ohittaa annettuja ohjeita.

13. Lataa ylläpidon tila päivitykset käyttämällä [Voit ladata korjausten](#to-download-hotfixes) lueteltuja vaiheita etsimiseen ja lataamiseen KB3121899, mitkä asentaa levyn laitteisto päivitykset (muut päivitykset on oltava asennettuna nyt mukaan).

13. Noudata seuraavia [asentaminen ja tarkista ylläpidon tila korjausten](#to-install-and-verify-maintenance-mode-hotfixes) ylläpidon tila-päivitysten asentaminen. 

  

## <a name="install-update-22-as-a-hotfix"></a>Asenna päivitys 2.2 korjaus

Näiden ohjeiden avulla, jos epäonnistua yhdyskäytävä-valintaruutu, kun yritän asentaa päivitykset palvelun Azure perinteinen-portaalissa. Tarkista epäonnistuu, sinulla on määritetty tietojen 0 verkon-liittymän yhdyskäytävän ja laite toimii päivitys 1 ennen ohjelmistoversio.

Ohjelmistoversiot, jotka voidaan päivittää korjaus-menetelmällä ovat seuraavat:

- Päivitä 0,1, 0,2, 0,3
- Päivitä 1, 1.1, 1.2
- Päivitä 2 2.1 

> [AZURE.IMPORTANT]
>
> - Jos laite toimii Release (GA)-version, ota yhteyttä [Microsoft-tuen](storsimple-contact-microsoft-support.md) auttamaan päivitys.

Korjaus-menetelmä on kolme vaihetta:

- Lataa korjausten Microsoft Update-luettelosta.
- Asenna ja varmista säännöllisesti tilassa korjaukset.
- Asenna ja varmista ylläpidon tila korjaus (vain, kun päivitetään päivitystä edeltävässä 2-ohjelmiston).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Lataa laitteessa päivityksen 2.1-ohjelmiston päivitykset

**Jos laite toimii Päivitä 2.1**, sinun on ladattava vain laitteen ohjelmistopäivityksen KB3179904. Asenna vain etuliitteenä on 'kaikki hcsmdssoftwareudpate' binaaritiedosto. Älä asenna Cis ja MDS agentti päivitys etuliitteenä on `all-cismdsagentupdatebundle`. Voit tehdä tämän virheen aiheuttaa virheen. Tämä on-häiritsevä päivitys, IO ei katketa ja laite ei ole mitään käyttökatkot.


#### <a name="download-updates-for-a-device-running-update-2-software"></a>Lataa laitteessa päivityksen 2-ohjelmiston päivitykset

**Jos laite toimii Update 2**, lataa ja asenna seuraavat korjausten järjestetty määritetyssä järjestyksessä:

| Tilauksen  | KT        | Kuvaus                    | Päivityksen tyyppi  | Asenna aika |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3179904 | Ohjelmistopäivitys & #42;  |  Normaali <br></br>Muut häiritsevä     | noin 45 minuuttia |
| 2.      | KB3146621 | iSCSI-paketti | Normaali <br></br>Muut häiritsevä  | noin 20 minuutin |
| 3.      | KB3103616 | WMI-paketti |  Normaali <br></br>Muut häiritsevä      | noin 12 minuuttia |


 & #42;  Huomautus-ohjelmiston *päivitys koostuu kaksi binaaritiedostoja: laitteen ohjelmistopäivitys etuliitteenä on `all-hcsmdssoftwareupdate` ja Cis ja Mds-agentti etuliitteenä on `all-cismdsagentupdatebundle`. Laitteen ohjelmistopäivitys on oltava asennettuna ennen Cis ja Mds-agentti. On käynnistettävä aktiivinen ohjauskoneen kautta `Restart-HcsController` cmdlet-komento, kun olet asentanut Cis ja MDS-agentti päivittäminen (ja ennen käyttöä jäljellä olevat päivitykset).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Lataa laitteessa päivitystä edeltävässä 2 ohjelmiston päivitykset

**Jos laite toimii versioita 0,2, 0,3, 1.0 ja 1.1**, sinun on ladattava ja asenna LSI ohjaimen ja laiteohjelmiston Päivitä lisäksi ohjelmiston, iSCSI ja WMI päivitykset. Tämä päivitys on jo asennettu, jos käytössäsi on päivitettävä 1.2 tai 2. 
 
| Tilauksen  | KT        | Kuvaus                    | Päivityksen tyyppi  | Asenna aika |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3121900 | LSI ohjain ja laiteohjelmiston             |  Normaali <br></br>Muut häiritsevä      | noin 20 minuutin |


<br></br>
**Jos laite toimii versioita 0,2, 0,3, 1.0, 1.1 ja 1.2**, täytyy ladata ja asentaa Spaceport ja Storport korjaus. Nämä on jo asennettu, jos käytössäsi on Update 2.

| Tilauksen  | KT        | Kuvaus                    | Päivityksen tyyppi  | Asenna aika |
|--------|-----------|-------------------------|------------- |-------------|
| 5.      | KB3090322 | Spaceport korjaaminen </br> Windows Server 2012 R2 |  Normaali <br></br>Muut häiritsevä      | noin 20 minuutin |
| 6.      | KB3080728 | Storport korjaaminen </br> Windows Server 2012 R2 |  Normaali <br></br>Muut häiritsevä      | noin 20 minuutin |



<br></br>
Haluat ehkä myös levyn laitteisto-päivitysten asentaminen. Voit tarkistaa, onko sinun levyn laitteisto-päivitykset suorittamalla `Get-HcsFirmwareVersion` cmdlet-komento. Jos käytössäsi on laiteohjelmiston seuraavia versioita: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, valitse sinun ei tarvitse asentaa päivitykset.


| Tilauksen  | KT        | Kuvaus                    | Päivityksen tyyppi  | Asenna aika |
|--------|-----------|-------------------------|------------- |-------------|
| 7.      | KB3121899 | Levyn laitteisto              |  Ylläpito <br></br>Häiritsevä      | noin 30 minuutin |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Tämä toiminto on tehtävä vain kerran sovelletaan päivityksen 2.2. Voit käyttää Azure perinteinen portaalissa seuraavien päivitysten.
> - Jos päivität päivityksen 2, yhteensä Asenna aika on lähellä 1,5 tuntia.
> - Ennen tämän toiminnon avulla voit päivityksen, varmista, että sekä laitteen ohjaimet ovat online-tilassa, ja kaikki laitteet on kunnossa.

Seuraavien toimien ladattava ja asennettava korjaukset.

[AZURE.INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [Update 2.1-versiossa](storsimple-update21-release-notes.md).
