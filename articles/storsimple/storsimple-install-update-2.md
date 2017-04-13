<properties
   pageTitle="Asenna päivitys 2 StorSimple laitteen | Microsoft Azure"
   description="Kerrotaan, miten voit asentaa StorSimple 8000 sarjan Update 2 StorSimple 8000 sarjan laitteen."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="install-update-2-on-your-storsimple-device"></a>Asenna päivitys 2 StorSimple laitteen

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten voit asentaa Update 2 StorSimple laitteeseen, jossa ohjelmistojen aiempana versiona Azure perinteinen portaalin kautta. Opetusohjelman myös kerrotaan päivitys vaaditaan, kun yhdyskäytävä on määritetty verkko-liittymän kuin StorSimple laitteen tiedot 0 ja yrität päivittää päivitystä edeltävässä 1 ohjelmiston versiosta.

Päivitys 2 sisältää laitteen ohjelmistopäivitykset, LSI päivityksistä ja levyn laitteisto päivitykset. Laiteohjelmisto ja LSI päivitykset-häiritsevä päivityksiä ja voi suojata Azure perinteinen portaalin kautta. Levyn laitteisto-päivitykset ovat häiritsevä päivitykset ja voi käyttää vain laitteen Windows PowerShell-liittymän kautta.

> [AZURE.IMPORTANT]

> -  Päivitä 2 ei välttämättä näy heti, koska olemme Tee vaiheistettu rahoittaja muutokset. Etsi uudelleen muutaman päivän päivityksiä tämän päivityksen tulee saataville pian.
> - Manuaalinen ja automaattinen vanhat tarkistukset joukko tehdään ennen asennusta kannalta laitteen tila ja verkon connectivity laite-terveydentilasta. Vanhat tarkistukset suoritetaan vain, jos käytät perinteistä Azure-portaalista päivitykset.
> - On suositeltavaa, että asennat ohjelmistojen ja ohjaimen päivitykset Azure perinteinen portaalin kautta. Windows PowerShell-liittymän (asentamaan päivitykset) laitteen pitäisi mennä vain, jos portaalissa epäonnistuu päivitystä edeltävässä yhdyskäytävä-valintaruutu. Päivitykset saattaa kestää 4 – 7 tuntia asentaminen (mukaan lukien Windows-päivitysten). Ylläpidon tila-päivitykset on oltava asennettuna laitteen Windows PowerShell-liittymän kautta. Ylläpidon tila-päivitykset ovat häiritsevä päivitykset, nämä johtaa laitteen alaspäin aika.
> - Jos käynnissä valinnainen StorSimple tilannevedoksen hallinta, varmista, että olet päivittänyt tilannevedoksen hallinta-versiosi päivitys 2 ennen päivittämistä laite.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>Asenna päivitys 2 Azure perinteinen portaalin kautta

Seuraavien toimien päivittämään laitteen [Update 2](storsimple-update2-release-notes.md).


> [AZURE.NOTE]
Päivitä 2 avulla Microsoft voi tuoda lisää vianmääritystiedot laitteesta. Tuloksena, kun toimintojen tiimimme tunnistaa laitteet, jotka on ongelmia, emme paremmin varustettu tietojen keräämiseen laitteesta ja vianmääritys. Hyväksymällä Update 2 Salli että ennakoiva tukea.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Varmista, että laite toimii **StorSimple 8000 sarjan Update 2 (6.3.9600.17673)**. **Päivitetty viimeksi päivämäärän** myös kannattaa muokata. Näet myös, että ylläpidon tila-päivitykset ovat käytettävissä (tämä viesti voi jatkaa, joka näkyy 24 tuntia päivitysten asentamisen jälkeen).

    Ylläpidon tila päivityksiä häiritsevä päivityksiä, jotka aiheuttavat laitteen käyttökatkot ja voi käyttää vain laitteesi Windows PowerShell-liittymän kautta. Joissakin tapauksissa päivityksen 1.2 suoritettaessa levyn laitteisto jo ehkä ajan tasalla, jolloin sinun ei tarvitse asentaa päivityksiä ylläpidon tila.

13. Lataa ylläpidon tila päivitykset käyttämällä [Voit ladata korjausten](#to-download-hotfixes) lueteltuja vaiheita etsimiseen ja lataamiseen KB3121899, mitkä asentaa levyn laitteisto päivitykset (muut päivitykset on oltava asennettuna nyt mukaan).

13. Noudata seuraavia [asentaminen ja tarkista ylläpidon tila korjausten](#to-install-and-verify-maintenance-mode-hotfixes) ylläpidon tila-päivitysten asentaminen.


## <a name="install-update-2-as-a-hotfix"></a>Asenna päivitys 2 korjaus

Näiden ohjeiden avulla, jos epäonnistua yhdyskäytävä-valintaruutu, kun yritän asentaa päivitykset palvelun Azure perinteinen-portaalissa. Tarkista epäonnistuu, sinulla on määritetty tietojen 0 verkon-liittymän yhdyskäytävän ja laite toimii päivitys 1 ennen ohjelmistoversio.

Ohjelmistoversiot, jotka voidaan päivittää korjaus-menetelmällä ovat päivityksen 0,1, päivitys 0,2, ja Päivitä 0,3, päivitys 1, päivitys 1.1 ja päivityksen 1.2. Korjaus-menetelmä on kolme vaihetta:

- Lataa korjausten Microsoft Update-luettelosta.
- Asenna ja varmista säännöllisesti tilassa korjaukset.
- Asenna ja varmista ylläpidon tila korjaus.

Asennettava Update 2 korjaustiedoston, lataa ja seuraavat korjausten asentaminen:

| Tilauksen  | KT        | Kuvaus                    | Päivityksen tyyppi  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Ohjelmistopäivitys         |  Normaali     |
| 2      | KB3121900 | LSI ohjain              |  Normaali     |
| 3      | KB3080728 | Storport korjaaminen </br> Windows Server 2012 R2 |  Normaali     |
| 4      | KB3090322 | Spaceport korjaaminen </br> Windows Server 2012 R2 |  Normaali     |
| 5      | KB3121899 | Levyn laitteisto           | Ylläpito  |


> [AZURE.IMPORTANT]
>
> - Jos laite toimii Release (GA)-version, ota yhteyttä [Microsoft-tuen](storsimple-contact-microsoft-support.md) auttamaan päivitys.
> - Tämä toiminto on tehtävä vain kerran sovelletaan Update 2. Voit käyttää Azure perinteinen portaalissa seuraavien päivitysten.
> - Jokaisen korjauksen asennus kestää noin 20 minuutin ajan suorittamiseen. Yhteensä Asenna aika on lähellä 2 tuntia.
> - Ennen tämän toiminnon avulla voit päivityksen, varmista, että laitteen molemmat ohjaimet ovat online-tilassa ja kaikki laitteet on kunnossa.

Seuraavien toimien tämän päivityksen kuin korjaus.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [Update 2-versiossa](storsimple-update2-release-notes.md).
