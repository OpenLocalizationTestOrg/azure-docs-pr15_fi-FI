<properties
    pageTitle="Mallin luominen Recommnendations käyttöliittymän | Microsoft Azure"
    description="Azure koneen Learning suositukset - rakentamiseen niin, että suositukset-Käyttöliittymä"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Suositukset-käyttöliittymän mallin luominen

Tämä asiakirja on vaiheittaiset ohjeet. Microsoftin tavoitteena on opastusta vaiheista kouluttaminen mallia, [Suositukset-Käyttöliittymän](https://recommendations-portal.azurewebsites.net/)avulla.
Käyttöoikeuden loppuun voit ymmärtää prosessin etsimisen mallin ennen kuin voit tehdä ohjelmallisesti. Se myös familiarizes voit niin, että Käyttöliittymä, joka on hyödyllinen, kun aloitat-palvelun avulla.

Tämä Harjoitus kestää noin 30 minuuttia.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Vaihe 1 - merkki, suositukset Käyttöliittymä ##

1. Jos et ole vielä niin, sinun on [kirjautumisen](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) uusi [Suosituksia API](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) -tilaukseen. Voit rekisteröityä **Azure** -palvelun [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) ja kirjaudu sisään Azure-tili. Rekisteröitymisen tarkat ohjeet toimitetaan [Aloitusoppaan](cognitive-services-recommendations-quick-start.md) *tehtävän 1* 

1. Kun olet saanut **avaimen** suosituksista API-tilauksen, valitse [Suositukset-Käyttöliittymä](https://recommendations-portal.azurewebsites.net/). 

1. Kirjaudu sisään-portaaliin syöttämällä key-tuotetunnuksen **Tiliavain Tiliavain** -kenttään ja valitse sitten **Kirjaudu sisään** -painike.

    ![Suosituksia Käyttöliittymän: Kirjaudu sisään-valintaikkuna.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Vaihe 2: kerätä japanin koulutustietoja ##

Voit luoda muodosta-moduulin on kaksi kappaletta tietojen: luettelotiedoston ja tiedostojoukon käyttö. 

Luettelotiedosto sisältää tietoja tarjoat asiakkaalle. Käyttö-tiedosto sisältää tietoja siitä, miten kohteiden käytetään ja yrityksesi tapahtumat.

Yleensä kyselyn kaupan tietokannan nämä tiedot kappaletta. Nykyään on annettu mallitietoja, niin, että Lue, miten suositukset-Ohjelmointirajapinnan käyttäminen.

Voit ladata tiedot [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Kopioi ja purkaa tiedoston **Books.Zip** paikallisessa tietokoneessa kansioon. Esimerkiksi **c:\data**.

Yksityiskohtaiset tiedot rakenteen luettelon ja käyttö tiedostot löytyvät [kouluttaminen mallin tietojen kerääminen](cognitive-services-recommendations-collecting-data.md) on artikkelissa.
 
Saat tämän Harjoitus tarkastellaan niin, että ei ole liian pitkä koulutus odotettava erittäin pienen tiedoston käyttöä varten. Voit halutessasi kokeilla realistisesta tiedosto on myös sijoittanut **MsStoreData.zip** , joka sisältää otoksen Microsoft Storesta [samaan sijaintiin](http://aka.ms/RecoSampleData).

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Vaihe 3 – projektin luominen ja lataa luettelon ja käyttötiedot ##

Yhteydessä kirjautumisesta [Suositukset-Käyttöliittymä](https://recommendations-portal.azurewebsites.net/), näet Projektit-sivu. Jos olet aiemmin luonut projekteja, niiden pitäisi näkyä tähän.
Projekti (tunnetaan myös nimellä *mallin* API viittauksen) on säilön soveltuu tietojesi luettelon ja käyttö. Voit luoda useita *muodostaa* Projectissa. Olemme edetään ohjatusti seuraavia vaiheita prosessi.

1. Jos haluat luoda uuden projektin luominen, kirjoittamalla nimen tekstiruudun (johonkin esimerkiksi "MyFirstModel" toimivat) ja **Lisää projekti**.
 
    ![Suosituksia Käyttöliittymän: Projektit-sivu.][reco_projects]

1. Kun projekti luodaan, napsauta **Lisää luettelotiedoston** osassa **Selaa tiedoston** -painiketta. Olet saanut vaiheessa 2 luettelon lataaminen Jos olet tallentanut sen osoitteessa *c:\data*, haluat siirtyä haluamaasi kansioon.

    ![Suosituksia Käyttöliittymän: Tietojen lisääminen projektin.][reco_firstmodel]

1. Kun luettelo on ladattu, napsauta **Lisää käyttö tiedostot** -osassa **Selaa tiedoston** -painiketta. Lisää usage_large.txt-tiedosto.

> **Entäpä jos käytössä on suuren tiedoston käyttötietojen?**
>
> Vain pienempi kuin 200 Megatavun käyttö-tiedostoja voi ladata. Said järjestelmän mahtuu 2 gt eurolla tapahtumatiedot, jotta voit ladata useita tiedostoja tarvittaessa.
> Et ehkä tarvitse, että tietojen hyvä mallin luominen, se ei ole vain tiedot, jotka vaikuttaa suorituskykyyn, mutta tietojen laadun kokoa. On yhteinen käyttötiedot jossa tapahtumat on vain muutama Suosituimmat kohteet ja on "vähän signaalin" muut kohteet.

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Vaihe 4 – Tee oletetaan, että jotkin koulutus! ##

Nyt kun olet ladannut luettelon ja käyttötietoja, emme valmis kouluttaminen moduulin niin, että se lukea kuviot Microsoftin tiedoista.

1.  Valitse **Uusi muodosta** -painiketta.

1.  Tyypiksi **suosituksia** muodosta. Huomaa, että sovellus tukee luokitus luominen ja muuttaa FBT-Maksuerien (usein ostanut yhdessä) luominen sekä tyypit.

    ![Suosituksia Käyttöliittymän: Muodosta valintaikkuna.][reco_build_dialog.png]


    Muuttaa FBT-Maksuerien muodosta voit tunnistaa tuotteita, jotka ovat yleensä ostettu/kulutettu saman tapahtuman kuviot.
    Luokittelun muodosta käytetään halutut toiminnot. 
    Ei siirryttävä hyvin kattavaa muuttaa FBT-Maksuerien tai luokittelu muodostaa tämän Workshop, mutta jos olet kiinnostunut kannattaa kuitata ulos [Muodosta tyypit ja mallin laatu asiakirjat-sivu](cognitive-services-recommendations-buildtypes.md).

1. Napsauta **Muodosta** -painiketta. Muodosta prosessi kestää noin viisi minuuttia, jos käytössäsi on kirjojen tiedot. Kestää kauemmin Valitse suurempi tietojoukkoja.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Vaihe 5 – selvittää oletetaan, että koneen asiat! ##

Kun oman muodosta on valmis, huomaat versiot-luettelossa uusi versio. Tämän koontiversion voidaan suorittaa kysely kohteen ja käyttäjän suositukset.

1. Kun oman muodosta on valmis, valitse **tulos**. Näin voit toistaa, joka sisältää mallin ja nähdä kohteet on suositeltavaa.

    ![Suositukset Käyttöliittymän: Pisteet-painike][reco_score_button]

1. Valitse kohde nähdäksesi, mitkä kohteet palautetaan suosituksia sen päälle. Huomaa, että jos tietokoneella ei ole riittävästi tapahtumia ennustaa suositus jotain tiettyä kohdetta, järjestelmä ei palauta kyseisen kohteen suositusten.  Jos jostain syystä on kohteessa, joka palauttaa ei suositukset, poista näkyvissä pistemäärä muut kohteet.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Vaihe 6 - seuraavat vaiheet ##
Onnittelen! Sinulla on koulutus mallin ja suositukset saatua sitä sitten.  Suositukset-Käyttöliittymän on hyötyä työkalua, joka näyttää projektien tila ja muodostaa. 

Nyt kun olet luonut mallia, voit halutessasi lisätietoja kaikki edellä mainitut toimet ohjelmallisesti. Lisätietoja Soita Ohjelmointirajapinnan ohjelmallisesti, jotta Microsoft kutsu voit tarkistaa [Suosituksia API-viittaus](http://go.microsoft.com/fwlink/?LinkId=759348) ja lataa [Sovelluksen suositukset-malli](http://go.microsoft.com/fwlink/?LinkID=759344).


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
