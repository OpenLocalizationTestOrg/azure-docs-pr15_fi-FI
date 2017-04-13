<properties 
    pageTitle="Luominen Azure RemoteApp cloud kokoelma | Microsoft Azure" 
    description="Opettele luomaan Azure RemoteApp, joka tallentaa tiedot Azure pilveen käyttöönoton." 
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

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Azure RemoteApp cloud kokoelma luominen

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

[Azure RemoteApp](remoteapp-collections.md)kokoelmien kahdenlaisia: 

- Cloud: sijaitsee kokonaan Azure. Voit tallentaa kaikki tiedot pilvipalveluun (jotta vain pilvipalveluita sivustokokoelman) tai voivat yhdistää kokoelmaa VNET ja tallentaa tiedot siellä.   
- Hybrid: sisältää virtual verkon paikallisen access - Tämä edellyttää Azure AD käyttö ja paikallisen Active Directory-ympäristössä.

Tässä opetusohjelmassa esitellään cloud sivustokokoelman luominen. On neljä vaihetta: 

1.  Azure RemoteApp-sivustokokoelman luominen
2.  Voit myös määrittää hakemistosynkronoinnin. Jos käytössäsi on Azure AD + Active Directoryssa, sinun on synkronoida käyttäjät, yhteystiedot ja salasanojen paikallisen Active Directorysta Azure AD-vuokraajan.
5.  Julkaise-sovelluksia.
6.  Määritä käyttöoikeudet.


**Ennen aloittamista**

Sinun on suoritettava seuraavat toimet ennen sivustokokoelman luomista:

- [Rekisteröidy](https://azure.microsoft.com/services/remoteapp/) Azure RemoteApp. 
- Kerää tietoja käyttäjät, jotka haluat antaa oikeudet. Tämä voi olla Microsoft-tilin tiedot tai Active Directory-työ tilitiedot käyttäjille.
- Tässä toimintosarjassa oletetaan, että sinulla on joko siirtyminen käyttämään tilauksen osana mallia kuvat tai sinulla on jo ladattu suunnittelumallin kuva, jota haluat käyttää. Jos sinun on ladattava eri suunnittelumallin kuva-, jotka voit tehdä sivustomallien kuvia-sivulta. Napsauta **mallin kuvan lataaminen** ja noudata ohjatun toiminnon ohjeita. 
- Haluatko käyttää Office 365 ProPlus kuvan? Tutustu tiedot [tähän](remoteapp-officesubscription.md).
- Haluat tarjota mukautettujen sovellusten tai LOB ohjelmat? Luo uusi [Kuva](remoteapp-imageoptions.md) ja käytä sitä cloud-sivustokokoelman.
- Onko sinun VNET yhdistäminen voi selvittää. Jos haluat muodostaa yhteyden VNET, varmista, että se täyttää [koon muuttaminen ohjeita](remoteapp-vnetsizing.md) ja että [Voit muodostaa yhteyden RemoteApp](remoteapp-vnet.md). Katso lisätietoja [VNET suunnitteluoppaan artikkelissa ](remoteapp-planvnet.md).
- Jos käytössäsi VNET, päätä, haluatko liittäminen paikallisen Active Directory-toimialueen.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Vaihe 1: Luo cloud - kokoelman kanssa tai ilman niitä VNET##


Käyttää **vain pilvipalveluita sivustokokoelman**luomiseen seuraavasti:

1. Siirry hallinta-portaalin RemoteApp-sivulle.
2. Valitse **Uusi > nopea luominen**.
3. Kokoelma nimi ja valitse alue.
4. Valitse palvelupaketti, jos haluat käyttää - standardin tai basic.
5. Valitse tämän sivustokokoelman käytettävän mallin. 

    **Vihje:** RemoteApp-tilauksesi sisältää [sivustomallien kuvia](remoteapp-images.md) , jotka sisältävät Office 365: n tai Office 2013: n (kokeiluversion käytettäväksi) ohjelmat, jotkin julkaistu (esimerkiksi Word) ja muiden valmis julkaisemaan. Voit myös luoda uusi [Kuva](remoteapp-imageoptions.md) ja käyttää sitä cloud kokoelmasta.


1. Valitse **Luo RemoteApp sivustokokoelman**.
    
    **Tärkeä:** Voi kestää enimmillään 30 minuuttia valmistelu kokoelmasta.

Kun Azure RemoteApp-sivustokokoelman on luotu, kaksoisnapsauta kokoelman nimi. Joka tuo mukanaan **Pika-aloitus** -sivun asetukset - kohtaa, johon olet määrittäminen kokoelma on.

Luo **cloud + VNET sivustokokoelman**noudattamalla seuraavia ohjeita:

1. Valitse hallinta-portaalissa Siirry Azure RemoteApp-sivulle.
2. Valitse **Uusi** > **VNET luominen**.
3. Kirjoita nimi kokoelmasta.
4. Valitse palvelupaketti, jos haluat käyttää - standardin tai basic.
5. Valitse aiemmin luomasi VNET. Tiedä voit tehdä? Vaiheet ovat nyt [hybrid](remoteapp-create-hybrid-deployment.md) -artikkelissa.
6. Päätä, haluatko liittyä kokoelmaa toimialueen. Kyllä, jos ne on AD Connect avulla voit integroida Azure AD ja Active Directory-ympäristössä. Joka on kuvattu alla **Vaiheessa**2.
6. Valitse **Luo RemoteApp sivustokokoelman**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Vaihe 2: Määritä Active Directory-hakemistosynkronoinnin (valinnainen) ##

Jos haluat käyttää Active Directory-Azure RemoteApp vaatii Azure Active Directory ja paikallisen Active Directory synkronoimaan käyttäjät, yhteystiedot ja Azure Active Directory-vuokraajan salasanojen Directoryn synkronointia. Katso lisätietoja [Määrittäminen Active Directory Azure RemoteApp](remoteapp-ad.md) . Voit myös siirtyä suoraan kohtaan [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tietoja.

## <a name="step-3-publish-apps"></a>Vaihe 3: Julkaise sovellukset ##

Azure RemoteApp-sovellus on sen sovelluksen tai ohjelma, jonka voit tarjota käyttäjille. Se sijaitsee ladattujen kokoelman mallin kuvaa. Kun käyttäjä käyttää sovelluksen-sovellus näkyy niiden paikallisessa ympäristössä, mutta se on todella käynnissä virtual kone Azure-tietokannassa. 

Ennen sovellusten voi käyttää, sinun pitää julkaista ne – julkaiseminen sovellusten avulla käyttäjät käyttää sovellukset Etätyöpöytä-asiakasohjelman kautta.
 
Voit julkaista useita sovelluksia Azure RemoteApp-kokoelmaan. Valitse **Julkaise** , jos haluat lisätä ohjelman julkaiseminen-sivulla. Voit julkaista joko **Käynnistä** -valikosta suunnittelumallin kuva tai määrittämällä polku mallikuva sovelluksen. Jos haluat lisätä **Käynnistä** -valikko, valitse poistettava sovellus julkaisemaan. Jos valitset polun-sovellukseen, anna sille nimi sovellus ja missä se on asennettu suunnittelumallin kuva polku.

## <a name="step-4-configure-user-access"></a>Vaihe 4: Käytettävyystietojen määrittäminen ##

Nyt kun olet luonut kokoelmaa, sinun on lisättävä käyttäjät, joita haluat käyttää remote resurssit. Jos käytössäsi on Active Directory-käyttäjät on pääsy on olemassa tilaukseen liittyvää Active Directory-vuokraajan käytit tämän sivustokokoelman luomiseen.

1.  Napsauta Pika-aloitus-sivun **Määritä käyttöoikeudet**. 
2.  Syötä työpaikan (Active Directorysta) tai Microsoft-tili, jonka haluat myöntää käyttöoikeuden.

    **Huomautus:** 

    Varmista, että käytät “user@domain.com” muodossa.

    Jos käytössäsi on Office 365 ProPlus-kokoelmaa, sinun on käytettävä Active Directory-käyttäjätietojen käyttäjiä. Näin voit tarkistaa käyttöoikeudet. 

3.  Kun käyttäjät on vahvistettu, valitse **Tallenna**.


## <a name="next-steps"></a>Seuraavat vaiheet ##

Siinä – olet luonut ja ottaa käyttöön Azure RemoteApp cloud-sivustokokoelman. Seuraavaksi käyttäjät voivat ladata ja asentaa etätyöpöydän kautta. Voit etsiä URL-Osoitteen asiakkaan Azure RemoteApp Pika-aloitus-sivulla. Tämän jälkeen on asiakkaan käyttäjien on kirjauduttava ja access-sovelluksia, voit julkaista.

### <a name="help-us-help-you"></a>Auta meitä auttamaan 
Tiesitkö, että lisäksi tässä artikkelissa arviointi ja tehdä kommentit alaspäin alla, voit tehdä muutoksia itse artikkelin? Järjestelmässä ei löydy? Ongelmia? Kirjoittaa jotakin, mitä on vain hankalaa? Vierittää ylöspäin ja valitse muokkaamiseen **GitHub muokkaaminen** - ne toimitetaan US tarkastelua varten ja sitten, kun olemme kirjautua niitä, näet muutokset ja parannukset täältä.