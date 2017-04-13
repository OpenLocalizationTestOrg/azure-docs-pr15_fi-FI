
<properties
    pageTitle="Azure tarkistuksessa Android | Microsoft Azure"
    description="Microsoft Azure todentaja app voidaan sisään käyttämään työresurssit. Azure todentaja sovellus ilmoittaa, odotetaan kaksivaiheinen vahvistus pyyntö näyttämällä ilmoituksen mobiililaitteeseen."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="azure-authenticator-for-android"></a>Android Azure tarkistuksessa

IT-järjestelmänvalvoja voi suositella käytettäväksi Microsoft Azure käyttöoikeudet sisään käyttämään työmäärä. Tämän sovelluksen sisältää seuraavat kaksi kirjautumisen vaihtoehdot:

* Monimenetelmäisen todentamisen avulla voit suojata kaksivaiheinen vahvistus on työpaikan tai oppilaitoksen tunnuksellasi. Kirjaudut sisään käyttämällä jotakin tiedät (esimerkiksi salasana) ja suojata tilin täsmentää jollakin sinulla (suojauksen näppäintä tästä sovelluksesta). Azure todentaja sovellus ilmoittaa, odotetaan kaksivaiheinen vahvistus pyyntö näyttämällä ilmoituksen mobiililaitteeseen. Sinun täytyy yksinkertaisesti tarkasteleminen sovelluksessa ja napauta pyynnön Tarkista tai Peruuta. Vaihtoehtoisesti voit pyydetään tunnuskoodin näyttää sovelluksessa.

* Työpaikan voit Android-puhelimeen tai tablettiin muuttuvat luotettava laite ja antaa yrityksen sovellusten Single Sign-On (SSO). IT-järjestelmänvalvoja saattaa edellyttää, että voit lisätä työmäärän tili yrityksen resurssien käyttö. SSO voit kirjautua sisään kerran ja soveltaa automaattisesti kaikki yrityksesi on määrittänyt sinulle sovellusten kirjautumisessa.

## <a name="installing-the-azure-authenticator-app"></a>Azure todentaja-sovelluksen

Voit asentaa Azure todentaja sovellus Google Play-kaupasta.
Jos haluat lisätä työpaikan Samsung Android-laite-ja muu kuin Samsung Android-laitteessa ohjeet ovat hieman eri tavalla. Sekä ohjeet näkyvät jäljempänä.

<a name="adding-the-work-account-from-samsung-android-device"></a>Työpaikan tilin lisääminen Samsung Android-laitteissa
----------------------------------------------------------------------------------------------------------------
###<a name="adding-the-work-account-through-the-app-home-screen"></a>Sovelluksen aloitusnäytön – työ-tilin lisääminen

Seuraavat ohjeet sovelletaan Samsung GS3 ja puhelimet eli Huomautus 2 ja tablettien yläpuolelle.

1. Valitse sovelluksen aloitusnäytössä Hyväksy käyttöoikeussopimus (käyttöoikeussopimus).
2. Valitse Aktivoi tili-näytön oikeanpuoleiseen pikavalikkoa ja valitse **työpaikan**.
3. Valitse Lisää tili-näytön** Työpaikan**.
4. Aktivoi laitteen järjestelmänvalvoja näytössä Valitse **Aktivoi**.
5. Tietosuojakäytännön näytössä Valitse valintaruutu ja valitse **Vahvista**.
6. Työpaikan liittyä näytössä Kirjoita organisaatiosi tarjoama käyttäjätunnus ja valitse **Liity**.
7. Kirjaudu sisään Azure todentaja-sovelluksen kirjoittamalla oman organisaation *** siakkaan ja salasana ja valitse **Kirjaudu sisään**.
8. Seuraavassa näytössä, jossa näkyy tietoja multi-factor todentaminen (MFA) on lisätty, suojaus ja on valinnainen. Tämä näyttö tulee näkyviin, jos työpaikan tai oppilaitoksen edellyttää todennusta toisen kerroin työpaikan luomiseen. Vahvista tilisi edelleen sen ohjeita.
9. Työpaikan Join-näytössä näkyvät viestin "**liittyminen työpaikalle**". Azure todentaja sovellus yrittää liittyä laitteen työpaikalle.
10. Valitse seuraavassa näytössä pitäisi näkyä työpaikkasi liittyneet viestin.

>[AZURE.NOTE]
> Yksittäisen työpaikan laitteen sallitaan.

### <a name="adding-the-work-account-from-the-settings-menu"></a>Työpaikan tilin lisääminen asetukset-valikossa
Kun olet asentanut Azure todentaja sovellus, voit myös luoda työpaikan Android Tilienhallinta.

1. Asetukset-valikossa **tilit** Siirry ja **Lisää**tili.
2. Vaiheet 3 – 10 menettelyä, työ-tilin kautta sovelluksen aloitusnäytössä työ-tilin lisääminen.

<a name="adding-the-work-account-from-a-non-samsung-android-device"></a>Työpaikan tilin lisääminen Samsung Android-laitteissa
------------------------------------------------------------------------------------------------------------------
### <a name="adding-the-work-account-through-the-app-home-screen"></a>Sovelluksen aloitusnäytön – työ-tilin lisääminen

1. Valitse sovelluksen aloitusnäytössä Hyväksy käyttöoikeussopimus (käyttöoikeussopimus).
2. Valitse Aktivoi tili-näytön oikeanpuoleiseen pikavalikkoa ja valitse **työpaikan**.
3. Valitse tilit-näytössä **Lisää tili**.
4. Jos näet tilit-näytössä, valitse **Lisää tili**. Jos työ-tili on jo luotu aiemmin, näet synkronoinnin näyttöön näyttää nykyisen työtilin. Voit säilyttää työpaikan napauttamalla yksinkertaisesti taaksepäin osoittavaa nuolta ja valitsemalla aloitusnäytössä. Vaihtoehtoisesti voit valita käytettävän tilin poistaminen ja luo uusi työ tilin työpaikkasi liittyä näyttöön, kirjoita organisaatiosi tarjoama käyttäjätunnus ja valitse liity.
5. Kirjautuaksesi sisään Azure todentaja sovelluksen Kirjoita organisaation tili ja salasana ja valitse **Kirjaudu sisään**.
7. Seuraavassa näytössä, jossa näkyy tietoja multi-factor authentication (MFA) on lisätty, suojaus ja on valinnainen. Tämä näyttö tulee näkyviin, jos työpaikan tai oppilaitoksen edellyttää todennusta toisen tekijä työpaikan luomiseen. Vahvista tilisi edelleen sen ohjeita.
8. Valitse seuraavassa näytössä valitsemalla **OK** . Varmenteen nimi eivät muutu.
viestin "Liittyminen työpaikalle". Azure todentaja sovellus yrittää liittyä laitteen työpaikalle.
Valitse seuraavassa näytössä pitäisi näkyä työpaikkasi liittyneet viestin.

>[AZURE.NOTE]
> Yksittäisen työpaikan laitteen sallitaan.

Kun olet asentanut Azure todentaja sovellus, voit myös luoda työpaikan Android Tilienhallinta.

1. **Asetukset** -valikossa tilit Siirry ja **Lisää**tili.
2. Noudata vaiheet 2 – 7 kuvattuja –-sovelluksen aloitus näytössä **, työ-tilin lisääminen työ-tilin lisääminen.

### <a name="how-to-find-out-which-version-is-installed"></a>Voit selvittää, mitä on asennettuna

1. Voit tarkistaa, mikä sovellusversio Azure todentaja ja liitetyn palvelun versio asennetaan laitteen.
2. Valitse ponnahdusvalikosta, **tietoja**.
3. Tietoja näyttöön avautuu sovelluksen ja laitteeseesi asennettu versiot palvelut.
 
### <a name="sending-log-files-to-report-issues"></a>Lokitiedostojen lähettäminen raporttien ongelmat

1. Ohjeita noudattamalla Microsoft Support raportin tapahtuma Azure todentaja sovelluksen, hanki tapauksen numero ja Lähetä lokitiedostot vastaan tapauksen numeroa seuraavasti:
2. Valitse ponnahdusvalikosta, **lokiin kirjaaminen**.
3. Jos sinulla on Microsoft Support ja Avaa tapahtuma, muistiin tapauksen numero (tarvitset niitä myöhemmin varten). Jos ole vielä luonut tuki tapahtumaa haluaisit apua, noudata toisen Avaa uusia tapaukseen [Microsoft-tuen](https://support.microsoft.com/en-us/contactus) ohjeet.
4. Valitse kirjaaminen-näytössä **Lähetä nyt**.
5. Valitse sähköpostipalvelua, jota haluat käyttää.
7. Jos sinulla on jo Avaa tapahtuma Microsoft Support, ota ongelmaasi myönnetyt Lue, miten voit lähettää lokitiedot ja sen oman tapaukseen liittyvät tukihenkilö. Tukihenkilö antaa sinulle sähköpostin osoite ja aihe-rivin tiedoilla. Jos sinulla ei ole tuki-tapahtumaa, noudata ohjeita Microsoft Support Avaa uusia tapaukseen osoitteessa.
9. Muokkaa **rivi** - ja **Aiherivi Microsoft Support saamasi tiedot** .
10. Azure todentaja sovelluksen liittää lokitiedosto haluat lähettää sähköpostia. Kuvaa on ilmennyt ongelma, Päivitä vastaanottajaluettelon (valinnainen) ja Lähetä sähköpostiviesti.

### <a name="deleting-the-work-account-and-leaving-your-workplace"></a>Työpaikan tilin poistaminen ja jätä työpaikalle

Voit poistaa työpaikan, luoda milloin tahansa seuraavasti:

**Voit poistaa työn tilin asetukset-valikossa**

1. Valitse Tilienhallinta- **työpaikan**.
2. Työpaikan näytössä, **Yleisasetukset**, valitse **tilin asetukset – jätä työpaikkasi verkkoon**.
3. Valitse **Jätä** **Työpaikkasi Join** -näytössä.
4. Valitse **OK** , kun näyttöön tulee sanoma "Ovat varmasti, josta haluat poistua työpaikkasi".
5. Näin varmistat, että olet poistanut tililtä työpaikasta riippumattomaksi.

>[AZURE.NOTE]
>On suositeltavaa, että et käytä Poista tili-vaihtoehto Poista työ-tilin, kun tämä asetus ei ehkä toimi joissakin Android aiemmissa versioissa.

##<a name="uninstalling-the-app"></a>Sovelluksen asennuksen poistaminen

Samsung Android-laitteessa laitteen järjestelmänvalvojan oikeudet on poistettava seuraavasti ennen asennuksen poistaminen 
1. Valitse **asetukset**, valitse **Järjestelmä**- **Suojaus**.
2. Olet D**evice hallinta**Napsauta **laitteen järjestelmänvalvojat**. Varmista, että **Azure todentaja** vieressä oleva valintaruutu ole valittu.

##<a name="troubleshooting"></a>Vianmääritys

Jos näet **Keystore virhe**, tämä voi johtua siitä ei ole Lukitse näyttö set up PIN-koodilla. Voit kiertää tämän ongelman Azure todentaja-sovelluksen asennuksen poistaminen, Lukitse näytön PIN-tunnuksen määrittäminen ja asenna sovellus.
