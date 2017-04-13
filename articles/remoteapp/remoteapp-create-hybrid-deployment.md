<properties
    pageTitle="Hybrid sivustokokoelman luominen Azure RemoteApp | Microsoft Azure"
    description="Opettele luomaan RemoteApp, joka yhdistää sisäisen verkon käyttöönoton."
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

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Azure RemoteApp hybrid sivustokokoelman luominen

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp kokoelmien kahdenlaisia:

- Cloud: sijaitsee kokonaan Azure. Voit tallentaa kaikki tiedot pilvipalveluun (jotta vain pilvipalveluita sivustokokoelman) tai voivat yhdistää kokoelmaa VNET ja tallentaa tiedot siellä.   
- Hybrid: sisältää virtual verkon paikallisen access - Tämä edellyttää Azure AD käyttö ja paikallisen Active Directory-ympäristössä.

Tiedä, mikä on? Tutustu [sivustokokoelman millainen on hyvä Azure RemoteApp varten](remoteapp-collections.md).

Tässä opetusohjelmassa esitellään hybrid sivustokokoelman luominen. On kahdeksan vaihetta:

1.  Päättää, mitä [Kuva](remoteapp-imageoptions.md) käytettävä kokoelmasta. Voit luoda mukautetun kuvan tai jollakin tilaukseen sisältyy Microsoft-kuvat.
2. Virtuaalinen-verkoston määrittäminen. Tutustu [VNET suunnittelu](remoteapp-planvnet.md) ja [koon](remoteapp-vnetsizing.md) tiedot.
2.  Luo kokoelma.
2.  Liity kokoelmaa paikallisen toimialueen.
3.  Lisää suunnittelumallin kuva kokoelmasta.
4.  Määritä hakemistosynkronoinnin. Azure RemoteApp edellyttää integroida Azure Active Directory joko 1) määrittäminen Azure Active Directory-synkronoinnin salasanan synkronointi-vaihtoehto tai 2) määrittäminen Azure Active Directory-synkronoinnin ilman salasanan synkronointi-vaihtoehto, mutta käyttää toimialuetta, joka on liitetty AD FS. Tutustu [Active Directory RemoteApp kokoonpanotietoja](remoteapp-ad.md).
5.  Julkaise RemoteApp-sovellukset.
6.  Määritä käyttöoikeudet.

**Ennen aloittamista**

Sinun on suoritettava seuraavat toimet ennen sivustokokoelman luomista:

- [Rekisteröidy](https://azure.microsoft.com/services/remoteapp/) Azure RemoteApp.
- Käyttäjätilin luominen Azure RemoteApp-palvelutili käyttämään Active Directoryssa. Rajoita tämän tilin käyttöoikeuksia niin, että se vain liittyä koneet toimialueeseen.
- Kerää tietoja paikallisen verkon: IP-osoite ja VPN laitteen tiedot.
- [PowerShellin Azure](../powershell-install-configure.md) -moduulin asentaminen.
- Kerää tietoja käyttäjät, jotka haluat antaa oikeudet. Tarvitset Azure Active Directory-käyttäjän pääasiallista nimi (esimerkiksi name@contoso.com) kullekin käyttäjälle. Varmista, että UPN vastaa Azure AD välillä ja Active Directorysta.
- Valitse malli-kuva. Azure RemoteApp-suunnittelumallin kuva sisältää sovellukset ja ohjelmat, jotka haluat julkaista käyttäjiä. Lisätietoja on kohdassa [Azure RemoteApp kuva-asetukset](remoteapp-imageoptions.md) .
- Haluatko käyttää Office 365 ProPlus kuvan? Tutustu tiedot [tähän](remoteapp-officesubscription.md).
- [Määrittää Active Directory RemoteApp varten](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Vaihe 1: Virtual-verkoston määrittäminen
Voit ottaa käyttöön hybrid kokoelma, joka käyttää aiemmin luodussa Azure virtual verkossa tai voit luoda uuden virtual verkon. Virtuaalinen verkon avulla käyttäjät access-tietojen paikallisen verkon kautta RemoteApp remote resurssit. Sivustokokoelman suoraan verkkokäyttö Azure virtual verkon käyttäminen antaa muiden Azure palveluihin ja näennäiskoneiden virtual verkon käyttöön.

Varmista, että voit tarkistaa [VNET suunnittelu](remoteapp-planvnet.md) ja [VNET koko](remoteapp-vnetsizing.md) -tiedot, ennen kuin voit luoda oman VNET.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Azure-VNET luominen ja liittäminen Active Directory-käyttöönotto

Aloita luomalla [VPN](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Azure-portaalissa **Verkko** -välilehti on valmis. Sinun täytyy virtual verkon yhdistäminen, jotka synkronoidaan Azure Active Directory-asiakasympäristöön Active Directory-käyttöönotto.

Lisätietoja on kohdassa [Luo virtuaalisia verkko Azure-portaalissa](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Varmista, että virtual verkko on valmiina Azure RemoteApp
Ennen kuin luot kokoelmaa, varmista seuraavaksi, uusi virtual verkkoyhteys on valmis. Voit tarkistaa seuraavasti:

1. Luo Azure virtuaalikoneen, aliverkon luomaasi varten RemoteApp virtual verkoston sisällä.
2. Etätyöpöytä avulla voit muodostaa yhteyden virtuaalikoneen. (Valitse **Muodosta yhteys**).
3. Liity, jota haluat käyttää RemoteApp saman Active Directory-käyttöönottoon.

, Joka toimii? VPN ja aliverkon ovat valmiita Azure RemoteApp!

Löydät lisätietoja luominen Azure-virtuaalikoneissa ja muodostaa ne Remote työpöydän [tähän](https://msdn.microsoft.com/library/azure/jj156003.aspx).

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Vaihe 2: Luo Azure RemoteApp-kokoelma ##



1. Siirry [Azure portal](http://manage.windowsazure.com)Azure RemoteApp-sivulle.
2. Valitse **Uusi > Luo VNET**.
3. Kirjoita nimi kokoelmasta.
4. Valitse palvelupaketti, jos haluat käyttää - standardin tai basic.
5. Valitse oman VNET avattavasta luettelosta ja valitse sitten käytettävissä myös aliverkon ulkopuolella.
6. Valitse liittäminen toimialueen.
5. Valitse **Luo RemoteApp sivustokokoelman**.

Kun Azure RemoteApp-sivustokokoelman on luotu, kaksoisnapsauta kokoelman nimi. Joka tuo mukanaan **Pika-aloitus** -sivun asetukset - kohtaa, johon olet määrittäminen kokoelma on.

Jotain ongelman tyyppi? Tutustu [hybrid sivustokokoelman vianmääritykseen liittyviä tietoja](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Vaihe 3: Linkittäminen kokoelmaa paikallisen toimialueen ##


1. Napsauta **Pika-aloitus** -sivulla **Liity paikallisen toimialueen**.
2. Lisää Azure RemoteApp palvelutilin paikallisen Active Directory-toimialueen. Sinun on toimialuenimi, organisaatioyksikkö, palvelun tilin käyttäjänimi ja salasana.

    Tämä on keräsit, jos olet toiminut edellä kuvatulla tavalla [määrittäminen Active](remoteapp-ad.md)Directory Azure RemoteApp tiedot.


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Vaihe 4: Linkki Azure RemoteApp-kuva ##

Azure RemoteApp-suunnittelumallin kuva sisältää ohjelmia, jonka haluat jakaa käyttäjien kanssa. Voit luoda uuden [mallin kuva](remoteapp-imageoptions.md) tai linkittäminen aiemmin luodun kuvan (yksi jo tuotu tai ladataan Azure RemoteApp). Voit myös linkittää johonkin Azure RemoteApp [sivustomallien kuvia](remoteapp-images.md) , jotka sisältävät Office 365: n tai Office 2013 (kokeiluversion käytettäväksi)-ohjelmissa.

Jos ladattava uusi kuva, joudut nimi ja sitten kuvan sijainti. Ohjatun toiminnon seuraavalla sivulla artikkelissa PowerShell-cmdlet - kopio ja Suorita järjestelmänvalvojana Windows PowerShell-kehotteessa, määritetyn kuvan lataaminen näiden cmdlet-komennot.

Jos olet muodostamassa yhteyttä aiemmin mallin kuvaa, määrittämällä kuvan nimi, sijainti ja liittyvän Azure tilauksen.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Vaihe 5: Määrittää Active Directory-hakemistosynkronointi ##

Azure RemoteApp edellyttää integroida Azure Active Directory joko 1) määrittäminen Azure Active Directory-synkronoinnin salasanan synkronointi-vaihtoehto tai 2) määrittäminen Azure Active Directory-synkronoinnin ilman salasanan synkronointi-vaihtoehto, mutta käyttää toimialuetta, joka on liitetty AD FS.

Tutustu [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – tämän artikkelin avulla voit määrittää hakemiston integroinnin 4 vaiheessa.

Katso [hakemistosynkronoinnin ohjeet](http://msdn.microsoft.com//library/azure/hh967642.aspx) suunnittelun tiedot ja toimimalla seuraavasti.

## <a name="step-6-publish-apps"></a>Vaihe 6: Julkaise sovellukset ##

Azure RemoteApp-sovellus on sen sovelluksen tai ohjelma, jonka voit tarjota käyttäjille. Se sijaitsee ladattujen kokoelman mallin kuvaa. Kun käyttäjä käyttää sovelluksen, se näkyy niiden paikallisessa ympäristössä, mutta se on todella käynnissä Azure-tietokannassa.

Ennen kuin voi käyttää sovelluksia, sinun pitää julkaista ne – näillä oikeuksilla käyttäjät käyttää sovellukset Etätyöpöytä-asiakasohjelman kautta.

Voit julkaista useita sovelluksia kokoelmaan. Valitse **Julkaise** , jos haluat lisätä sovelluksen julkaiseminen-sivulla. Voit julkaista joko **Käynnistä** -valikosta suunnittelumallin kuva tai määrittämällä polku mallikuva sovelluksen. Jos haluat lisätä **Käynnistä** -valikko, valitse ohjelma lisää. Jos valitset polun-sovellukseen, anna sille nimi sovellus ja missä se on asennettu suunnittelumallin kuva polku.

## <a name="step-7-configure-user-access"></a>Vaihe 7: Käytettävyystietojen määrittäminen ##

Nyt kun olet luonut kokoelmaa, sinun on lisättävä käyttäjät, joita haluat käyttää remote resurssit. Käyttäjien on pääsy on olemassa tilaukseen liittyvää Active Directory-vuokraajan käytit Azure RemoteApp-sivustokokoelman luomiseen.

1.  Napsauta Pika-aloitus-sivun **Määritä käyttöoikeudet**.
2.  Syötä työpaikan (Active Directorysta) tai Microsoft-tili, jonka haluat myöntää käyttöoikeuden.

    **Huomautus:**

    Varmista, että käytät “user@domain.com” muodossa.

    Jos käytössäsi on Office 365 ProPlus-kokoelmaa, sinun on käytettävä Active Directory-käyttäjätietojen käyttäjiä. Näin voit tarkistaa käyttöoikeudet.


3.  Kun käyttäjät on vahvistettu, valitse **Tallenna**.


## <a name="next-steps"></a>Seuraavat vaiheet ##
Siinä – olet luonut ja Azure RemoteApp-hybrid sivustokokoelman käyttöön. Seuraavaksi käyttäjät voivat ladata ja asentaa etätyöpöydän kautta. Voit etsiä URL-Osoitteen asiakkaan Azure RemoteApp Pika-aloitus-sivulla. Tämän jälkeen on asiakkaan käyttäjien on kirjauduttava ja access-sovelluksia, voit julkaista.



### <a name="help-us-help-you"></a>Auta meitä auttamaan
Tiesitkö, että lisäksi tässä artikkelissa arviointi ja tehdä kommentit alaspäin alla, voit tehdä muutoksia itse artikkelin? Järjestelmässä ei löydy? Ongelmia? Kirjoittaa jotakin, mitä on vain hankalaa? Vierittää ylöspäin ja valitse muokkaamiseen **GitHub muokkaaminen** - ne toimitetaan US tarkastelua varten ja sitten, kun olemme kirjautua niitä, näet muutokset ja parannukset täältä.
