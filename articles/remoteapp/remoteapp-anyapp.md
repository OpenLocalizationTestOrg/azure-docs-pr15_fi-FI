<properties
   pageTitle="Suorittaa minkä tahansa Windows-sovelluksen millä tahansa laitteella Azure RemoteApp | Microsoft Azure"
   description="Lue, miten voit jakaa minkä tahansa Windows-sovelluksen käyttäjien kanssa käyttämällä Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Suorittaa minkä tahansa Windows-sovelluksen millä tahansa laitteella Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Voit suorittaa missä tahansa Windows-sovelluksen millä tahansa laitteella tällä hetkellä vakavasti - Azure RemoteApp käyttämällä. Olipa kirjoitettu 10 vuotta sitten mukautetun sovelluksen tai Office-sovelluksen käyttäjien tarvitse enää voi liittää tietyn käyttöjärjestelmän (kuten Windows XP) muutamia näiden sovellusten.

Azure RemoteApp käyttäjien voit myös käyttää omia Android- tai Apple-laite ja saada saman kokemus ne käytössä Windows (tai Windows-puhelimet). Tämä on kirjautumalla kokoelma näennäiskoneiden Windows Azure-sovelluksen Windows - käyttäjät voivat käyttää niitä missä tahansa heillä on yhteydessä Internetiin. 

Tutustu jakamistapoihin Esimerkki tarkasti, kuinka voit tehdä tämän.

Tässä artikkelissa tutustutaan Access jakaminen kaikkien käyttäjien kanssa. Voit kuitenkin käyttää minkä tahansa sovelluksen. Kun voit asentaa sovelluksen Windows Server 2012 R2-tietokoneeseen, voit jakaa sen seuraavien ohjeiden mukaisesti. Voit tarkastella [sovelluksen vaatimukset](remoteapp-appreqs.md) varmistaaksesi, että sovelluksesi toimivat oikein.

Huomaa, että koska Access-tietokanta on, ja haluamme tietokannan voi olla hyödyllistä, emme voi tekevät muutamia ylimääräisiä toimia, jos haluat, että käyttäjät voivat käyttää Access-tietojen jakaminen. Jos sovellus ei ole tietokannan tai et tarvitse käyttäjät voivat käyttää jaettua tiedostoresurssia, voit ohittaa nämä vaiheet Tässä opetusohjelmassa

> [AZURE.NOTE] <a name="note"></a>Sinun on suoritettava tässä opetusohjelmassa Azure tilin:
> - Voit [avata Azure-tili maksutta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): Saat hyvitykset avulla voit kokeilla maksettu Azure services ja senkin jälkeen, kun niitä käytetään enintään voit pitää tilin ja käyttää vapaa Azure-palvelut, kuten sivustot. Luottokorttisi koskaan veloitetaan, ellet nimenomaisesti muuttaminen ja pyydä veloitetaan.
> - Voit [aktivoida MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Your MSDN-tilauksen käyttöösi hyvitykset joka kuukausi, jonka maksettu Azure-palveluiden avulla.


## <a name="create-a-collection-in-remoteapp"></a>Luo kokoelma RemoteApp

Aloita luomalla kokoelma. Kokoelma on säilön sovellukset ja käyttäjille. Valikoimien perustuu kuva – voit luoda omia tai jollakin tilauksen mukana. Tässä opetusohjelmassa esimerkissä on käytössä Office 2013: n kokeiluversion kuva - se sisältää sovelluksen, että haluat jakaa.

1. Azure-portaaliin Selaa vasemmalla olevasta siirtymisruudussa puun kunnes näet RemoteApp. Avaa sivu.
2. Valitse **Luo RemoteApp-sivustokokoelman**.
3. Valitse **nopea luominen** ja kokoelmaa nimi.
4. Valitse haluamasi alue että sivustokokoelman luomiseen. Saat parhaan kokemuksen Valitse alue, joka on lähimpänä maantieteellisesti haluamaasi kohtaan, johon käyttäjien käyttää sovellusta. Esimerkiksi Tässä opetusohjelmassa käyttäjät sijaitsee Redmond, Washington. Lähin Azure alue on **Länsi US**.
5. Valitse Laskutus suunnitelma, jota haluat käyttää. Perustietoja Laskutus palvelupaketin Lisää 16 käyttäjät suuri Azure-AM samalla, kun vakio Laskutus palvelupaketin on 10 käyttäjää, suuri Azure-AM. Yleiset esimerkkinä basic suunnitelman sopii hyvin merkinnän tyyppi työnkululle. Tuottavuutta-sovelluksen, kuten Office-standard järjestelyn haluat.
6. Valitse lopuksi Office 2013 Professional-kuva. Tämä kuva on Office 2013-sovellukset. Muistutus - Tämä kuva on vain kokeiluversion sivustokokoelmat ja POCs. Voit "ei voi käyttää kuvassa tuotannon kokoelman.
7. Valitse **Luo RemoteApp sivustokokoelman**nyt.

![Cloud sivustokokoelman luominen RemoteApp](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Kokoelma luominen käynnistyy, mutta se voi viedä tunneiksi.

Nyt olet valmis lisäämään käyttäjille.

## <a name="share-the-app-with-users"></a>Sovelluksen jakaminen käyttäjien kanssa

Kun kokoelmaa on luotu, on aika julkaista Access käyttäjille ja Lisää käyttäjät, jotka kannattaa käyttää niitä.

Jos olet siirtynyt poispäin Azure RemoteApp-solmu samalla, kun kokoelman luotiin, Aloita tekemällä etsimäsi takaisin Azure-kotisivulla.

2. Valitse luomasi aiemmissa Lisäasetukset ja määritä kokoelman sivustokokoelman.
![Uuden RemoteApp cloud kokoelman](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. Valitse **Julkaise** -välilehdestä **Julkaise** näytön alareunassa ja valitse sitten **Julkaise Käynnistä-valikon ohjelmat**.
![Julkaise RemoteApp-ohjelmasta](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Valitse julkaistavat luettelosta sovellukset. Tutustu tarkoitukseen Microsoft valitsi Access. Valitse **Valmis**. Odota, Lopeta julkaiseminen sovellukset.
![Julkaiseminen Access RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Kun sovellus on valmis jaettu julkaisuoikeus, pää päälle **Käyttöoikeudet** -välilehdessä Lisää kaikki käyttäjät, jotka tarvitsevat sovelluksia. Kirjoita käyttäjien käyttäjänimiä (sähköpostiosoite) ja valitse sitten **Tallenna**.

![Käyttäjien lisääminen RemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Nyt on aika kertoa käyttäjille uusi nämä sovellukset ja ohjeet niiden käyttöön. Voit tehdä tämän lähettämällä käyttäjille osoittamalla etätyöpöydän asiakkaan lataamisen URL-Osoitteen sähköposti.
![Asiakkaan Lataa RemoteApp URL-osoite](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Accessin käytön määrittäminen

Jotkin sovellukset on ylimääräisiä määritykset, kun otat ne RemoteApp kautta. Erityisesti Access-tutustutaan luominen Azure, kuka tahansa voi käyttää jaettua tiedostoresurssia. (Jos et halua tehdä, voit luoda [hybrid sivustokokoelman](remoteapp-create-hybrid-deployment.md) [sijaan Microsoftin cloud sivustokokoelman], jonka avulla käyttäjät tiedostot ja tiedot kuin paikalliseen verkkoon kirjauduttaessa.) Tämän jälkeen on on kertominen Microsoftin käyttäjille, voit yhdistää kyseisen henkilön tietokoneessa paikalliseen asemaan Azure tiedostojärjestelmässä.

Järjestelmänvalvojana voit tehdä ensimmäinen osa. Tämän jälkeen on muutama käyttäjiä.

1. Aloita julkaisemalla rivin komentoliittymä (cmd.exe). Valitse **Julkaise** -välilehdestä, **cmd**ja valitse sitten **Julkaise > Julkaise ohjelmaa polku**.
2. Kirjoita nimi sovelluksen ja polun. Tutustu tarkoitukseen Määritä "Resurssienhallinnan" nimi ja "% SYSTEMDRIVE%\windows\explorer.exe" polku.
![Julkaise cmd.exe-tiedosto.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Kun haluat luoda Azure- [tallennustilan tilin](../storage/storage-create-storage-account.md). Olemme nimeltä stävällisin "accessstorage", valitse niin, joka on merkityksellinen nimi. (Voit misquote Highlander voi olla vain yksi "accessstorage.") ![Our Azure-tallennustilan tilin](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Nyt palaa raporttinäkymän niin polku voi käyttää tallennustilan (päätepisteen sijainti). Käyttäminen vähän, joten varmista, kopioi se toiseen sijaintiin.
![Tallennustilan tilin polku](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Kun tallennustilan-tili on luotu, sinun on seuraavaksi ensisijainen pikanäppäin. Valitse **Hallitse pikanäppäimet**ja kopioi ensisijainen pikanäppäin.
6. Nyt Aseta konteksti-tallennustilan tilin ja luo jaetun tiedoston käytön. Suorita järjestelmänvalvojana Windows PowerShell-ikkunassa seuraavat cmdlet-komennot:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    Tutustu jaetun nämä, joiden on suorittaa cmdlet-komennot:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Nyt on käyttäjän ottaminen käyttöön. Pyydä [RemoteApp-asiakasohjelman](remoteapp-clients.md)asentaminen käyttäjille. Seuraavaksi käyttäjät on Yhdistä verkkoasemaan niiden tililtä Azure luomasi resurssin ja lisätä Access-tiedostonsa. Miten ne tehdä seuraavat asiat:

1. RemoteApp-asiakasohjelmassa access julkaistun sovelluksia. Aloita cmd.exe-ohjelman.
2. Suorita seuraava komento Yhdistä verkkoasemaan tietokoneesta jaettuun resurssiin:

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Jos määrität **/ pysyvä** parametrin arvoksi Kyllä, levyasemasta säily istunto.
1. Käynnistä nyt RemoteApp Resurssienhallinta-sovellus. Kopioi tiedostoja Access, jota haluat käyttää jaettua tiedostoresurssia jaetun sovelluksen.
![Tiedostojen sijoittaminen Azure Jaa](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Lopuksi Avaa Access ja avaa sitten juuri jakamasi tietokanta. Accessin pilvestä tietojen pitäisi näkyä.
![Käynnissä pilvestä reaali Access-tietokantaan](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Nyt voit käyttää Access mihin tahansa laitteelta - vain Varmista, että RemoteApp-asiakasohjelman asentaminen.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet selvittänyt luominen kokoelma, yritä luoda [sivustokokoelman, joka käyttää Office 365: ssä](remoteapp-tutorial-o365anywhere.md). Voit myös luoda [hybrid sivustokokoelman ](remoteapp-create-hybrid-deployment.md), voit käyttää kuin paikalliseen verkkoon kirjauduttaessa.

<!--Image references-->
 
