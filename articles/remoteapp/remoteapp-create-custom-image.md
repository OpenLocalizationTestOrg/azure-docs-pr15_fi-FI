<properties
    pageTitle="Mukautetun mallin kuva luominen Azure RemoteApp | Microsoft Azure"
    description="Lue, miten voit luoda mukautetun mallin kuva Azure RemoteApp. Voit käyttää tätä mallia hybrid tai pilvestä sivustokokoelman."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>Mukautetun mallin kuva luominen Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp käyttää Windows Server 2012 R2-suunnittelumallin kuva isännöimiseen kaikki ohjelmat, jotka haluat jakaa käyttäjien kanssa. Jos haluat luoda mukautetun RemoteApp-suunnittelumallin kuva, voit aloittaa olemassa olevan kuvan tai luoda uuden. 


> [AZURE.TIP] Tiesitkö, että voit luoda kuvan Azure-AM? Tarinan TOSI, ja niiden Leikkaa ajan, se on tuotava kuva. Lue ohjeet [tähän](remoteapp-image-on-azurevm.md).

Kuva, joka voi ladata ja käyttää siinä Azure RemoteApp vaatimukset ovat seuraavat:


- Kuvan koko on oltava MBs kerrannaiseen. Jos yrität ladata kuvan, joka ei ole on jaollinen, lataus epäonnistuu.
- Kuvan koko on oltava 127 gt tai Pienennä.
- Sen on oltava .vhd-tiedosto (VHDX tiedostoja [Hyper-V virtual kiintolevyillä] ei tällä hetkellä tueta).
- Näennäiskiintolevyn ei saa olla luonti 2 virtual koneen.
- Näennäiskiintolevyn voi olla kiinteä tai dynaamisesti laajennettavassa. Dynaamisesti laajennettavassa Näennäiskiintolevyn on suositeltavaa, sillä kuluu vähemmän aikaa, voit ladata Azure kuin kiinteä Näennäiskiintolevytiedosto.
- Levy on valmistellaan käyttämällä perustyyli käynnistyksen tietueen (Pääkäynnistystietue) jakaminen tyyli. GUID-tunnus osion (GPT) osion Taulukkotyyli ei tueta.
- Näennäiskiintolevyn on oltava yksittäinen Windows Server 2012 R2-asennus. Se voi olla useita asemat, mutta vain yksi, joka sisältää Windowsin asennuksen.
- Remote Desktop istunnon Host (RDSH)-roolin ja Desktop Experience-toiminto on oltava asennettuna.
- Remote Desktop yhteyksien välittäjä rooli on *ei* voitu asentaa.
- Salaaminen File System (EFS) on poistettava käytöstä.
- Kuvan täytyy olla käyttäen parametreja Sysprep **/oobe / generalize/shutdown** (Älä käytä **/mode:vm** -parametri).
- Ladata tilannevedoksen yhdistettyjen oman Näennäiskiintolevyn ei tueta.


**Ennen aloittamista**

Sinun on suoritettava seuraavat toimet ennen palvelun luomista:

- [Rekisteröidy](https://azure.microsoft.com/services/remoteapp/) RemoteApp.
- Käyttäjätilin luominen Active Directory RemoteApp palvelutilin käyttää. Rajoita tämän tilin käyttöoikeuksia niin, että se vain liittyä koneet toimialueeseen. Lisätietoja on kohdassa [Määritä Azure Active Directory RemoteApp varten](remoteapp-ad.md) .
- Kerää tietoja paikallisen verkon: IP-osoite ja VPN laitteen tiedot.
- [PowerShellin Azure](../powershell-install-configure.md) -moduulin asentaminen.
- Kerää tietoja käyttäjät, jotka haluat antaa oikeudet. Tämä voi olla Microsoft-tilin tiedot tai Active Directory-työ tilitiedot käyttäjille.



## <a name="create-a-template-image"></a>Luo malli-kuva ##

Voit luoda uuden mallin kuvan alusta korkean tason vaiheet ovat seuraavat:

1.  Etsi Windows Server 2012 R2: n päivitys DVD-levyllä tai ISO-kuva.
2.  Luo Näennäiskiintolevytiedosto.
4.  Asenna Windows Server 2012 R2.
5.  Asenna Remote Desktop istunnon Host (RDSH)-roolin ja Desktop Experience-toiminto.
6.  Asenna vaatii sovellusten lisäominaisuuksia.
7.  Asenna ja määritä sovellukset. Voit helpottaa jakamisen sovellusten Lisää kaikki sovellukset tai ohjelmat, jotka haluat jakaa kuvan, erityisesti **%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs **Käynnistä** -valikkoon.
8.  Suorittaa muita Windows-määritykset, vaatii sovellukset.
9.  Poista käytöstä salauksen tiedostojärjestelmässä (EFS).
10. **Vaaditaan:** Siirry Windows Update ja asenna kaikki tärkeät päivitykset.
9.  SYSPREP kuva.

Yksityiskohtaisia ohjeita luodaan uusi kuva ovat seuraavat:

1.  Etsi Windows Server 2012 R2: n päivitys DVD-levyllä tai ISO-kuva.
2.  Luo Näennäiskiintolevytiedosto Levynhallinnan avulla.
    1.  Käynnistä Disk Management (diskmgmt.msc).
    2.  Vähintään 40 Gigatavun kokoisia dynaamisesti laajennettavassa Näennäiskiintolevyn luominen. (Windows-sovellusten ja mukautukset tarvittavat jäävän arvio. RDSH rooli ja Desktop Experience-toiminto on asennettu Windows Server edellyttää 10 gt kiintolevytilaa).
        1.  Valitse **toiminto > Luo Näennäiskiintolevyn**.
        2.  Määritä sijainti, koko ja Näennäiskiintolevyn muoto. Valitse **dynaamisesti laajentaminen**ja valitse sitten **OK**.

            Tämä suorittaa muutaman sekunnin ajan. Kun Näennäiskiintolevyn luominen on valmis, näkyy uuden levyn ilman mitään asemakirjaimen ja levyn hallintakonsoli "Alustamaton"-tilaan.

        - Napsauta hiiren kakkospainikkeella levyä (ei levytyypistä) ja valitse sitten **Alusta levy**. Valitse **MBR** (päätietueen käynnistyksen) osion tyyli ja valitse sitten **OK**.
        - Luo uusi asema: varaamatonta tilaa hiiren kakkospainikkeella ja valitse sitten **Uusi tavallinen asema**. Hyväksy oletusarvot ohjatun toiminnon, mutta varmista, että määrität kirjaimen ongelmien välttämiseksi, kun olet ladannut mallin kuva.
        - Levyn hiiren kakkospainikkeella ja valitse sitten **Irrota VHD**.





1. Asenna Windows Server 2012 R2:
    1. Luo uuden virtual koneen. Käytä ohjattua virtuaalikoneen Hyper-V Manager tai asiakkaan Hyper-V.
        1. Valitse Määritä luonti-sivulla **luonti 1**.
        2. Yhdistä Virtual kiintolevyn sivulla Valitse **Käytä aiemmin virtual kiintolevylle**ja Etsi edellisessä vaiheessa luomasi Näennäiskiintolevyn.
        2. Valitse asennuksen asetukset-sivulla Valitse **asentaa käynnistyksen CD-levylle tai DVD_ROM-käyttöjärjestelmän**ja valitse sitten Windows Server 2012 R2-asennustietoväline sijainti.
        3. Valitse ohjatun toiminnon edellyttämät Asenna Windowsin ja sovellusten muut asetukset. Ohjatun toiminnon.
    2.  Kun ohjattu toiminto päättyy, virtuaalikoneen asetusten muokkaaminen ja tee muut muutokset tarpeen Asenna Windowsin ja ohjelmien, kuten virtual suorittimien määrän ja valitse sitten **OK**.
    4.  Yhteyden muodostaminen virtuaalikoneen ja asenna Windows Server 2012 R2.
1. Asenna Remote Desktop istunnon Host (RDSH)-roolin ja Desktop Experience-toiminto:
    1. Käynnistä Serverin hallinta.
    2. Valitse **Lisää rooleja ja toiminnot** aloitusnäyttöön tai **hallinta** -valikon.
    3. Ennen aloittamista-sivulle valitsemalla **Seuraava** .
    4. Valitse **Roolipohjainen tai ominaisuuksiin perustuvaan asennus**ja valitse sitten **Seuraava**.
    5. Valitse Paikallinen kone-luettelosta ja valitse sitten **Seuraava**.
    6. Valitse **Etätyöpöytäpalvelut**ja valitse sitten **Seuraava**.
    7. Laajenna **käyttöliittymien ja infrastruktuuri** ja valitse **Desktop Experience**.
    8. Valitse **Lisää ominaisuuksia**ja valitse sitten **Seuraava**.
    9. Valitse Etätyöpöytäpalvelut-sivulla **Seuraava**.
    10. Valitse **työpöydän etäistunnon isäntä**.
    11. Valitse **Lisää ominaisuuksia**ja valitse sitten **Seuraava**.
    12. Valitse Vahvista asennuksen asetukset-sivun **Käynnistä kohdepalvelin automaattisesti tarvittaessa**ja valitse sitten Käynnistä varoitus **Kyllä** .
    13. Valitse **Asenna**. Tietokone käynnistetään uudelleen.
1.  Asentaa sovellukset, kuten .NET Framework 3.5 vaatii lisäominaisuuksia. Voit asentaa ominaisuudet, suorittamalla Lisää rooleja ja ominaisuudet ohjatun toiminnon.
7.  Asenna ja määritä ohjelmat ja sovelluksia, jotka haluat julkaista RemoteApp kautta.

>[AZURE.IMPORTANT]
>
>Asenna RDSH rooli varmistaa, että sovellusten yhteensopivuuden ongelmia löytyy, ennen kuin kuva on ladattu RemoteApp sovellusten asentamista.
>
>Varmista, että pikakuvakkeen (**.lnk** -tiedosto)-sovellukseen näkyvät kaikille käyttäjille (%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs) **Käynnistä** -valikko. Varmista myös näkyviin **Käynnistä** -valikon kuvaketta on, mitä haluat käyttäjien näkevän. Jos et muuta sitä. (Sinulla ei *ole* voit lisätä sovelluksen alkuun valikon, mutta se on helpompi RemoteApp sovelluksen julkaisemaan. Muussa tapauksessa on annettava asennuspolku sovelluksen sovelluksen julkaisukerralla.)


8.  Suorittaa muita Windows-määritykset, vaatii sovellukset.
9.  Poista käytöstä salauksen tiedostojärjestelmässä (EFS). Oikeuksin suoritettava komentorivi-ikkuna, suorita seuraava komento:

        Fsutil behavior set disableencryption 1

    Vaihtoehtoisesti voit määrittää tai lisätä rekisterin DWORD-arvo:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  Jos kokoat kuvan Azure virtuaalikoneen sisällä, nimetä uudelleen ** \%windir%\Panther\Unattend.xml** tiedoston, koska se estää Lataa komentosarja käyttää myöhemmin ei toimi. Muuttaa tämän tiedoston nimi Unattend.old niin, että on edelleen tiedoston siltä varalta, kun haluat palata käyttöönoton.
10. Siirry Windows Update ja asenna kaikki tärkeät päivitykset. Voit joutua muuttamaan useita kertoja saat kaikki päivitykset Windows Updaten suorittaminen. (Joskus päivitys asentaa ja kyseinen päivitys itse vaatii.)
10. SYSPREP kuva. Järjestelmänvalvojan oikeuksin suoritettava komentokehote, suorita seuraava komento:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe / Shutdown**

    **Huomautus:** Älä käytä **/mode:vm** Vaihda SYSPREP-komennon, vaikka tämä on.


## <a name="next-steps"></a>Seuraavat vaiheet ##
Nyt kun olet luonut mukautetun mallin kuvaa, sinun on ladattava kyseiseen kuvaan RemoteApp-kokoelmaan. Seuraavissa artikkeleissa tietojen avulla voit luoda kokoelmaa:


- [Hybrid kokoelma RemoteApp luominen](remoteapp-create-hybrid-deployment.md)
- [Cloud kokoelma RemoteApp luominen](remoteapp-create-cloud-deployment.md)
 