<properties
    pageTitle="Azure multi-factor Authentication - seuraava"
    description="Tämä on Azure multi-factor authentication sivu, jossa kuvataan, mitä tehdään MFA Seuraava.  Tämä sisältää raportit, petoksilta ilmoitus, erikseen ohituksen, mukautetut ääniviestejä välimuistiin luotettujen IP-osoitteet ja sovelluksen salasanat."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="kgremban"/>

# <a name="configuring-azure-multi-factor-authentication"></a>Azure Monimenetelmäisen todentamisen määrittäminen

Tämän artikkelin avulla voit hallita Azure Monimenetelmäisen todentamisen ovat hyvin alkuun.  Se peittää eri aiheista, jotka auttavat hyödyntäminen tehokkaasti Azure Monimenetelmäisen todentamisen.  Kaikki nämä ominaisuudet ovat käytettävissä jokaisella Azure multi-factor Authentication-versiossa.

Joitakin ominaisuuksia alla määrityskohde löytyy Azure multi-factor Authentication hallinta-portaalissa. Kahdella eri, jota voit käyttää MFA hallinta-portaalin, jotka tehdään Azure portaalin kautta. Ensimmäinen on hallinta multi-factor Auth palveluntarjoaja, jos kulutus perustuva MFA. Toinen on MFA-Palveluasetukset. Toinen vaihtoehto edellyttää multi-factor Auth palveluntarjoaja tai Azure MFA, Azure AD Premium tai yrityksen Mobility ohjelmiston käyttöoikeuden.

MFA hallinta-portaalin kautta tarjoajan Azure multi-factor-todennus käyttämään kirjautua Azure-portaaliin järjestelmänvalvojana ja valitse Active Directory-vaihtoehto. **Multi-Factor Auth palvelut** -välilehti ja sitten Valitse hakemistossa ja valitse **hallinta** -painike alaosassa.

MFA-Palveluasetukset-sivulla MFA hallinta-portaalin käyttämään kirjautua Azure-portaaliin järjestelmänvalvojana ja valitse Active Directory-vaihtoehto. Valitse hakemisto ja valitse sitten **Määritä** -välilehti. Valitse multi-factor authentication-osassa **Hallitse Palveluasetukset**. MFA-Palveluasetukset-sivun alareunassa napsauttamalla **portaalista** -linkkiä.


Toiminto| Kuvaus| Mitä käsitellään
:------------- | :------------- | :------------- |
[Ilmoituksen ilmoitus](#fraud-alert)|Ilmoituksen ilmoitus on määritetty ja määrittäminen niin, että käyttäjien raportoida vilpilliseen yrittää käyttää niiden resursseja.|Määritä, määrittäminen ja raportin petoksen
[Ohita kerran](#one-time-bypass) |Erikseen ohituksen avulla käyttäjä voi todentaa "ohittaminen" monimenetelmäisen todentamisen yhdellä kertaa.|Miten erikseen ohituksen määrittäminen
[Mukautetun ääniviestejä](#custom-voice-messages) |Mukautetun ääniviestejä avulla voit käyttää omia tallenteet tai tervehdyksiä monimenetelmäisen todentamisen. |Miten määrittäminen mukautetut tervehdykset ja viestit
[Välimuistiin tallentaminen](#caching-in-azure-multi-factor-authentication)|Välimuistin avulla voit määrittää tietyn ajan kuluessa kauden niin, että seuraavien todennus yritykset onnistuu automaattisesti. |Miten määrittäminen todennus välimuistiin.
[Luotettu IP-osoitteet](#trusted-ips)|Luotetut IP-osoitteet on monimenetelmäisen todentamisen, jonka avulla järjestelmänvalvojat hallitun tai liitetyt vuokraajan voi ohittaa monimenetelmäisen todentamisen käyttäjille, jotka ovat kirjautua sisään yrityksen Paikallinen intranet-ominaisuus.|Määrittää, ja IP-osoitteet, jotka ovat vapautettujen monimenetelmäisen todentamisen määrittäminen
[Sovelluksen salasanat](#app-passwords)|Sovelluksen salasanan avulla sovellus, joka ei ole MFA-aware voit ohittaa monimenetelmäisen todentamisen ja jatka työskentelyä.|Sovelluksen salasanat tietoja.
[Muista Monimenetelmäisen todentamisen tallennettujen laitteet ja selaimet](#remember-multi-factor-authentication-for-devices-users-trust)|Voit muista laitteiden päivää, kun käyttäjä on kirjautunut sisään käyttämällä MFA tietyn ajan.|Tietoja toiminnon ottaminen käyttöön ja määrittäminen päivien määrän.
[Voidaan valita vahvistus menetelmät](#selectable-verification-methods)|Voit valita, joita käyttäjät voivat käyttää todennustavat.|Tietoja käyttöönoton tai käytöstäpoiston tietyn todennusmenetelmät, kuten puhelun tai tekstiviestillä.



## <a name="fraud-alert"></a>Ilmoituksen ilmoitus
Ilmoituksen ilmoitus on määritetty ja määrittäminen niin, että käyttäjien raportoida vilpilliseen yrittää käyttää niiden resursseja.  Käyttäjät voivat raportoida ilmoituksen tai Mobiilisovelluksella puhelimeensa kautta.

### <a name="to-set-up-and-configure-fraud-alert"></a>Jos haluat ilmoituksen ilmoituksen määrittäminen

1.  Kirjaudu sisään http://azure.microsoft.com
2.  Siirry tämän sivun yläosassa ohjeiden MFA hallinta-portaalin.
3.  Valitse Azure multi-factor Authentication hallinta-portaalin asetusten määrittäminen-osassa.
4.  Ilmoituksen ilmoitusten asetukset-sivun-osassa Tarkista Salli-käyttäjät voivat lähettää ilmoituksen ilmoitukset-valintaruutu.
5.  Jos haluat, että käyttäjät voivat olla estetty, kun petoksen ilmoitetaan, Lisää rasti estä käyttäjää, kun petoksen ilmoitetaan.
6.  Kirjoita numero tunnus, jolla voidaan puhelun vahvistus **Koodi, raportin petoksen aikana alkuperäisen tervehdys** -tekstiruutuun. Kun käyttäjän kirjoittamat tämän koodin plus # sen sijaan, että vain #-merkki, petoksilta ilmoitus raportoidaan.
7.  Ja valitse Tallenna.

>[AZURE.NOTE]
>Microsoftin oletusarvon puhepostin tervehdysten Kehota käyttäjiä painamaan 0# lähettää ilmoituksen ilmoitus. Jos haluat käyttää koodia kuin 0, kannattaa tallentaa ja lataa omia mukautettuja voice tervehdykset tarvittavat ohjeet.


![Cloud](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="to-report-fraud-alert"></a>Raportin petoksen hälytys
Ilmoituksen ilmoitus voit raportoida kahdella tavalla.  Joko mobiilisovelluksessa tai puhelimen kautta.  

### <a name="to-report-fraud-alert-with-the-mobile-app"></a>Raportin petoksen hälytys Mobiilisovelluksella



1. Kun vahvistusta lähetetään puhelimeesi, valitse Käynnistä Microsoft Authenticator-sovellus.
2. Raportin ilmoituksen, valitse Peruuta ja raportin ilmoituksen. Tämä näyttää ruutu, jossa lukee organisaation IT-tukihenkilöstö henkilökunnan ilmoitetaan.
3. Valitse raportti-ilmoituksen.
4. Valitse sovellus Valitse Sulje.

![Cloud](./media/multi-factor-authentication-whats-next/report1.png)


![Cloud](./media/multi-factor-authentication-whats-next/fraud2.png)

### <a name="to-report-fraud-alert-with-the-phone"></a>Raportin petoksen hälytys puhelimen kanssa

1. Työpuhelimessa vahvistus puhelimeesi, vastata siihen.  
2. Voit ilmoittaa petoksen koodi, joka on määritetty vastaamaan raportoinnin petoksen kautta puhelimen ja #-merkki. Saat ilmoituksen petoksen ilmoitus on lähetetty.
3. Lopeta puhelu.

### <a name="to-view-the-fraud-report"></a>Ilmoituksen raportin tarkasteleminen

1. Kirjaudu [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Valitse vasemmassa reunassa Active Directorysta.
3. Valitse multi-factor Auth tarjoajat yläreunassa. Tämä näyttää luettelon multi-factor Auth-palveluista.
4. Jos sinulla on useampi kuin yksi multi-factor Auth tarjoajan, valitse haluat ilmoituksen ilmoitusten raportin tarkasteleminen ja hallinta sivun alareunassa. Jos käytössäsi on vain yksi, valitse hallinta. Tämä avaa Azure multi-factor Authentication hallinta-portaalin.
5. Azure multi-factor Authentication hallinta-portaalissa, vasemmalla, valitse Näytä A raportti valitsemalla petoksen ilmoitus.
6. Määritä päivämääräalue, jotka haluat näyttää raportissa. Voit myös määrittää mitään tiettyä käyttäjätunnukset ja puhelinnumerot käyttäjän tila.
7. Valitse Suorita. Tämä näyttää raportissa alla. Voit myös napsauttaa CSV Vie Jos haluat viedä raportin.

## <a name="one-time-bypass"></a>Ohita kerran

Erikseen ohituksen avulla käyttäjä voi todentaa "ohittaminen" monimenetelmäisen todentamisen yhdellä kertaa. Ohituksen ovat väliaikaisia ja päättyy määritetyn ajan kuluttua.  Jotta tilanteissa, jossa mobiilisovelluksen tai puhelinnumero on saa ilmoituksen tai puhelu, voit ottaa erikseen ohituksen käyttäjä voi käyttää haluamasi resurssi.

### <a name="to-create-a-one-time-bypass"></a>Voit luoda erikseen ohitus

1.  Kirjaudu sisään http://azure.microsoft.com
2.  Siirry tämän sivun yläosassa ohjeiden MFA hallinta-portaalin.
3.  Azure multi-factor Authentication hallinta-portaalissa, jos näet vuokraajan tai Azure MFA-palvelun kanssa vasemmalla vieressä- painiketta + eri MFA palvelimen replikoinnin ryhmiä ja Azure oletusarvo-ryhmä. Valitse haluamasi ryhmä.
4.  Käyttäjien hallinta-kohdasta **One-Time Ohita**.
![Cloud](./media/multi-factor-authentication-whats-next/create1.png)
5.  Valitse **Uusi One-Time ohituksen**One-Time ohitus-sivulla.
6.  Kirjoita käyttäjän käyttäjänimi, sekunteina, ohituksen ovat olemassa, ohituksen syy ja valitse **Ohita**.
![Cloud](./media/multi-factor-authentication-whats-next/create2.png)
7.  Tässä vaiheessa käyttäjän täytyy kirjautua sisään, ennen kuin erikseen ohituksen päättyy.



### <a name="to-view-the-one-time-bypass-report"></a>Erikseen ohitus-raportin tarkasteleminen

1. Kirjaudu [http://azure.microsoft.com](https://azure.microsoft.com/)
2. Valitse vasemmassa reunassa Active Directorysta.
3. Valitse multi-factor Auth tarjoajat yläreunassa. Tämä näyttää luettelon multi-factor Auth-palveluista.
4. Jos sinulla on useampi kuin yksi multi-factor Auth tarjoajan, valitse haluat ilmoituksen ilmoitusten raportin tarkasteleminen ja hallinta sivun alareunassa. Jos käytössäsi on vain yksi, valitse hallinta. Tämä avaa Azure multi-factor Authentication hallinta-portaalin.
5. Azure multi-factor Authentication hallinta-portaalissa, vasemmalla, valitse Näytä A raportti valitsemalla One-Time Ohita.
6. Määritä päivämääräalue, jotka haluat näyttää raportissa. Voit myös määrittää mitään tiettyä käyttäjätunnukset ja puhelinnumerot käyttäjän tila.
7. Valitse Suorita. Tämä näyttää raportissa alla. Voit myös napsauttaa CSV Vie Jos haluat viedä raportin.

<center>![Cloud](./media/multi-factor-authentication-whats-next/report.png)</center>

## <a name="custom-voice-messages"></a>Mukautetun ääniviestejä

Mukautetun ääniviestejä avulla voit käyttää omia tallenteet tai tervehdyksiä monimenetelmäisen todentamisen.  Nämä voidaan lisäksi tai korvaa Microsoft tietueita.

Ennen kuin aloitat huomioon seuraavasti:

- Nykyisen tuetut tiedostomuodot ovat .wav ja .mp3.
- Tiedoston enimmäiskoko on 5 Megatavua.
- On suositeltavaa, että varten todennus viestit, se ei ole ei saa olla yli 20 sekuntia. Mitään suurempi kuin tämä saattaa aiheuttaa vahvistus epäonnistuu, koska käyttäjä ei välttämättä vastaa, ennen kuin viesti on valmis ja vahvistus aikakatkaistaan.



### <a name="to-set-up-custom-voice-messages-in-azure-multi-factor-authentication"></a>Mukautetun vastaajan viestien Azure Monimenetelmäisen todentamisen määrittäminen
1.  Luo mukautettu ääniviesti, käytä jotakin seuraavista tuetut tiedostomuodot.
2.  Kirjaudu sisään http://azure.microsoft.com
3.  Siirry tämän sivun yläosassa ohjeiden MFA hallinta-portaalin.
4.  Valitse ääniviestejä Azure multi-factor Authentication hallinta-portaalin määrittäminen-osassa.
5.  Vastaajan viestit-kohdasta **Uusi kuva**.
![Cloud](./media/multi-factor-authentication-whats-next/custom1.png)
6.  Valitse Määritä: vastaajan viestien **Äänitiedostot hallinta**-sivulla.
![Cloud](./media/multi-factor-authentication-whats-next/custom2.png)
7.  Valitse Määritä: äänen tiedostot-sivulla, valitse **Lataa äänitiedosto**.
![Cloud](./media/multi-factor-authentication-whats-next/custom3.png)
8.  Valitse Määritä: lataa äänitiedosto, valitse **Selaa** ja siirry voice viestin, valitse **Avaa**.
![Cloud](./media/multi-factor-authentication-whats-next/custom4.png)
9.  Lisää halutessasi kuvaus ja valitse Lataa.
10. Kun toiminto on suoritettu, viestin vahvistaa on onnistuneesti ladattu tiedosto.
11. Valitse vasemmassa reunassa ääniviestejä.
12. Vastaajan viestit-kohdasta uusi kuva.
13. Kieli avattavasta luettelosta Valitse kieli.
14. Jos tämä viesti on tietylle sovellukselle, Määritä sovellus-ruutuun.
15. Valitse haluamasi viesti viestin tyyppi voi ohittaa tämän uuden mukautetun viestin.
16. Äänitiedoston avattavasta Valitse haluamasi äänitiedosto.
17. Valitse **Luo**. Sanoma vahvistaa, että olet luonut ääniviestiä.
![Cloud](./media/multi-factor-authentication-whats-next/custom5.png)</center>



## <a name="caching-in-azure-multi-factor-authentication"></a>Välimuistiin tallentamisen Azure Monimenetelmäisen todentamisen

Välimuistin avulla voit määrittää tietyn ajan kuluessa kauden niin, että seuraavien todennus yritykset onnistuu automaattisesti. Tätä käytetään pääasiassa, kun paikallisen järjestelmien, kuten VPN lähettää useita vahvistus-pyyntöjä ensimmäisen pyynnön ollessa käynnissä. Tämä asetus mahdollistaa seuraavat pyynnöt onnistuu automaattisesti, kun käyttäjä on luotu vahvistus on meneillään. Huomaa, että välimuistiin ei ole tarkoitettu, jota käytetään Azure AD Kirjaudu apuohjelmia.


### <a name="to-set-up-caching-in-azure-multi-factor-authentication"></a>Välimuistiin tallentamisen Azure Monimenetelmäisen todentamisen määrittäminen

1.  Kirjaudu sisään http://azure.microsoft.com
2.  Siirry tämän sivun yläosassa ohjeiden MFA hallinta-portaalin.
3.  Valitse välimuisti Azure multi-factor Authentication hallinta-portaalin määrittäminen-osassa.
4.  Valitse Määritä välimuistiin sivun uusi välimuisti
5.  Valitse välimuistin tyypin ja välimuistin sekuntia. Valitse Luo.

<center>![Cloud](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Luotettu IP-osoitteet

Luotetut IP-osoitteet on monimenetelmäisen todentamisen, jonka avulla järjestelmänvalvojat hallitun tai liitetyt vuokraajan voi ohittaa monimenetelmäisen todentamisen käyttäjille, jotka ovat kirjautua sisään yrityksen Paikallinen intranet-ominaisuus. Ominaisuudet ovat käytettävissä Azure AD-alihallinnat, jotka on Azure AD Premium, yrityksen Mobility Suite tai Azure Monimenetelmäisen todentamisen käyttöoikeuksia.


Azure AD-vuokraajan tyyppi| Luotetut IP-vaihtoehdot
:------------- | :------------- |
Hallittu|Tietyn IP-osoitealueet – järjestelmänvalvojat voivat määrittää alueen, IP-osoitteet, jotka voivat ohittaa monimenetelmäisen todentamisen käyttäjille, jotka ovat sovellukseen on kirjauduttu sisään yrityksen intranet.
Organisaation ulkopuolinen|<li>Kaikkien organisaation ulkopuolisten käyttäjien - monimenetelmäisen todentamisen käyttämällä AD FS myöntämä vaatimus ohittavat kaikki liitetyt käyttäjät, jotka ovat kirjautua sisään-organisaation sisällä.</li><li>Tietyn IP-osoitealueet – järjestelmänvalvojat voivat määrittää alueen, IP-osoitteet, jotka voivat ohittaa monimenetelmäisen todentamisen käyttäjille, jotka ovat sovellukseen on kirjauduttu sisään yrityksen intranet.

Tämä ohittaa toimii vain-ja yrityksen intranet sisällä. Siten esimerkiksi, jos olet valinnut kaikki liitetyt käyttäjät ja käyttäjä kirjautuu-ulkopuolella yrityksen intranet-käyttäjä on tarkistamiseen monimenetelmäisen todentamisen avulla, vaikka käyttäjä esittää AD FS-ryhmän. Seuraavassa taulukossa on kuvattu, kun multi-factor authentication ja sovelluksen salasanat ovat pakollisia, että corpnet ja että corpnet sivuihin, kun Luotetut IP-osoitteet on otettu käyttöön.


|Luotetut käytössä IP-osoitteet| Luotetut käytöstä IP-osoitteet
:------------- | :------------- | :------------- |
Keltaiset corpnet|Selaimen kulkee monimenetelmäisen todentamisen ei tarvita.|Selaimen kulkee monimenetelmäisen todentamisen pakollinen
|Rich client-sovellusten säännöllisesti salasanat toimivat, jos käyttäjä ei luotu minkä tahansa sovelluksen salasanat. Kun ohjelman salasana on luotu, sovelluksen salasanat tarvitaan.|Rich client-sovellusten ohjelmasalasanat pakollinen
Ulkopuolinen corpnet|Selaimen kulkee monimenetelmäisen todentamisen pakollinen.|Selaimen kulkee monimenetelmäisen todentamisen pakollinen.
|Rich client-sovellusten edellyttää sovelluksen salasanat.|Rich client-sovellusten edellyttää sovelluksen salasanat.

### <a name="to-enable-trusted-ips"></a>Jos haluat ottaa käyttöön luotettujen IP-osoitteet

1. Kirjaudu sisään Azure perinteinen-portaaliin.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Valitse-kohdasta kansio haluat Luotetut IPsing Määritä hakemistoon.
4. Valitse kansio on valittuna, Määritä.
5. Valitse multi-factor authentication-osassa Hallitse Palveluasetukset.
6. Valitse Palveluasetukset-sivulla Luotetut IP-osoitteet Valitse jompikumpi seuraavista:

    - Varten pyynnöt organisaation ulkopuolinen käyttäjien Omat intranet peräisin – liitetty kaikki käyttäjät, jotka ovat kirjautua sisään yrityksen verkosta ohittavat monimenetelmäisen todentamisen AD FS myöntämä vaatimus avulla.
    - Kirjoita IP-osoitteiden pyyntöjä julkinen IP-osoitteet – tietyn alueen ruutuihin käyttäen CIDR-merkintätapaa. Esimerkki: xxx.xxx.xxx.0/24 IP-osoitteiden alueen – xxx.xxx.xxx.1 xxx.xxx.xxx.254 tai xxx.xxx.xxx.xxx/32 yksi IP-osoite. Voit kirjoittaa enintään 50 IP-osoitealueet.

7. Valitse Tallenna.
8. Kun päivitykset on otettu käyttöön, valitse Sulje.



![Luotettu IP-osoitteet](./media/multi-factor-authentication-whats-next/trustedips3.png)




## <a name="app-passwords"></a>Sovelluksen salasanat

Jotkin sovellukset, kuten Office 2010: ssä tai vanhemmissa versioissa ja Apple Mail ei voi käyttää monimenetelmäisen todentamisen.  Käyttää nämä sovellukset, tarvitset käyttämään "sovelluksen salasanat-tekstin näyttäminen perinteinen salasanasi.  Ohjelman salasana sallii sovelluksen ohittaa monimenetelmäisen todentamisen ja jatka työskentelyä.

>[AZURE.NOTE] Nykyaikainen todentaminen Office 2013-asiakasohjelmat
>
> Office 2013: n asiakkaiden (mukaan lukien Outlook) tue uusi todennusprotokollia ja on määritetty tukemaan Monimenetelmäisen todentamisen nyt.  Tämä tarkoittaa, että kun otettu käyttöön, sovelluksen salasanat eivät ole pakollisia käytettäväksi Office 2013-asiakkaiden kanssa.  Lisätietoja on artikkelissa [Office 2013: n Nykyaikainen todentaminen julkisen esikatselu ilmoitettiin](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).



### <a name="important-things-to-know-about-app-passwords"></a>Lisätietoja sovelluksen salasanat tärkeitä tietoja

Seuraavassa on tärkeitä asioita, jotka kannattaa tietää sovelluksen salasanat-luettelo.

- Käyttäjillä voi olla useita ohjelmasalasanat, mikä suurentaa varkaus pinta. Koska sovelluksen salasanat ovat voi unohtua helposti, se saattaa rohkaista, kirjoita se muistiin. Tämä ei ole suositeltavaa ja suositella koska vain yksi tekijä on edellyttää kirjautumista sovelluksen salasana.
- Sovellukset, jotka välimuistiin salasanat ja käyttää sitä paikallisen skenaariot aloittaa puuttuessa, koska ohjelman salasana ei ole tiedossa organisaation tunnus ulkopuolella. Esimerkki on Exchange-sähköpostia, jotka ovat paikallisen mutta arkistoidut mail on pilvipalvelussa. Sama salasana ei toimi.
- Todellinen salasana luodaan automaattisesti ja käyttäjä ei ole toimitettu. Tämä johtuu siitä automaattisesti luotu salasana on näyttävyyttä hyödyntämällä arvata ja on entistä turvallisempi.
- Tällä hetkellä on 40 salasanojen käyttäjäkohtainen rajoitukset. Voit pyydetään poistettava jonkin olemassa olevan ohjelmasalasanat jotta voit luoda uuden.
- Kun käyttäjän tilin monimenetelmäisen todentamisen on otettu käyttöön, sovelluksen salasanat voidaan käyttää useimmissa esimerkiksi Outlookista tai Lync selaimen asiakkaiden kanssa, mutta hallintatehtäviin ei voi suorittaa käyttämällä sovelluksen salasanat selaimen sovelluksista, kuten Windows PowerShellin avulla, vaikka, että käyttäjällä on jo järjestelmänvalvojan tili.  Varmista luominen palvelutili vahva salasana, jos haluat suorittaa PowerShell-komentosarjojen ja ota käyttöön monimenetelmäisen todentamisen syyhyn.

>[AZURE.WARNING]  Sovelluksen salasanat eivät toimi yhdistelmäympäristöstä, jossa on sekä paikallista yhteydenpito asiakkaat ja cloud autodiscover päätepisteet. Tämä johtuu siitä toimialueen salasanat tarvitaan paikallisen tarkistamiseen ja sovelluksen salasanat edellyttää todennusta pilven kautta.


### <a name="naming-guidance-for-app-passwords"></a>Sovelluksen salasanat nimeämiskäytäntö ohjeet
On suositeltavaa, että sovelluksen salasanan nimet on vastattava laitteen, jossa niitä käytetään. Esimerkiksi jos käytössäsi on kannettava tietokone, jossa on selain sovellukset, kuten Outlook, Word ja Excel-haluat vain yhden ohjelman salasana nimeltä kannettavan tietokoneen luoda ja käyttää kyseisen ohjelman salasana kaikki sovellukset. Voit luoda erilliset salasanat kaikki sovellukset, mutta se ei ole suositeltavaa. Tapa on yksi laite sovelluksen salasanaa suositellaan.


<center>![Cloud](./media/multi-factor-authentication-whats-next/naming.png)</center>


### <a name="federated-sso-app-passwords"></a>Liitetyt (SSO)-sovelluksen salasanat
Azure AD tukee ‑pikaviestintäpalveluntarjoajan kanssa paikallisen Windows Server Active Directory ‑toimialueen palveluihin (AD DS). Jos organisaatiosi on federated(SSO) Azure AD kanssa ja aiot käyttää Azure Monimenetelmäisen todentamisen ja valitse seuraavassa on tärkeitä tietoja, sinun olisi otettava huomioon, kun käytät sovelluksen salasanat. Tämä koskee vain federated(SSO) asiakkaille.

- Ohjelman salasana on tarkistanut Azure AD ja näin ollen ohittaa federation. Liitetty viestintä käytetään vain aktiivisesti ohjelman salasana määritettäessä.
- Federated(SSO) käyttäjien on aina Siirry toisin kuin passiivinen kulun tunnistetietojen toimittaja (IdP). Organisaation tunnus on tallennettu salasanat. Jos käyttäjä jättää yrityksen, näitä tietoja on flow organisaation tunnus reaaliajassa Dirsync-työkalun avulla. Tilin poistaminen käytöstä tai poistaminen saattaa kestää jopa kolme tuntia synkronointia varten on viivästyy Azure AD-sovelluksen salasanan poistaminen käytöstä tai poistaminen.
- Paikallisen asiakkaan käyttöoikeuksien valvonta-asetukset eivät ole voimassa App salasanalla
- Paikallisen tarkistusta kirjaaminen / valvonta-ominaisuus on käytettävissä sovelluksen salasana
- Lisää käyttäjän education tarvitaan Microsoft Lync 2013-asiakasohjelmassa. Tutustu tarvittavat vaiheet sähköposti-ilmoituksen salasanan vaihtaminen sovelluksen salasana.
- Tietyt kehittyneet suunnitelmat voi vaatia organisaation käyttäjänimi ja salasana ja sovelluksen salasanat yhdistelmän avulla käytettäessä monimenetelmäisen todentamisen sen mukaan, missä ne todentavat asiakkaiden kanssa. Asiakkaat, jotka todennetaan paikallinen infrastruktuuri käyttäisi organisaation käyttäjänimi ja salasana. Asiakkaille, jotka todennetaan Azure AD-käyttäisi sovelluksen salasana.

Oletetaan, että sinulla on arkkitehtuuri, joka koostuu seuraavista:

- Paikallinen-esiintymässä Active Directory ja Azure AD ovat sisällytetyistä
- Käytät Exchange online-tilassa
- Käytössäsi on Lync, jotka on nimenomaisesti paikallinen
- Käytössäsi on Azure Monimenetelmäisen todentamisen


![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

 Tällöin toimi seuraavasti:

- Kun kirjautua sisään Lynciin, käytä että organisaatiot käyttäjänimi ja salasana.
- Kun yrität käyttää osoitteiston Outlook-asiakas, joka yhdistää Exchange Onlinen kautta, käytä sovelluksen salasanan.

### <a name="allowing-app-password-creation"></a>Sovelluksen salasanan luomisen salliminen
Oletusarvon mukaan käyttäjät eivät voi luoda sovelluksen salasanat.  Tämä ominaisuus on otettava käyttöön.  Jotta käyttäjät voivat luoda sovelluksen salasanat, toimimalla seuraavasti.

#### <a name="to-enable-users-to-create-app-passwords"></a>Voit antaa käyttäjien luoda sovelluksen salasanat



1. Kirjaudu Azure perinteinen-portaaliin.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Valitse-kansion kohdasta haluat ottaa käyttöön käyttäjän kansio.
4. Valitse käyttäjät yläreunassa.
5. Valitse sivun alareunassa multi-factor todennussertifikaatin hallinta  
6. Valitse Palveluasetukset monimenetelmäisen todentamisen sivun yläreunassa.
7. Varmista, että käyttäjät voivat luoda sovelluksen salasanat kirjautumalla selaimen sovellukset-kohdan vieressä oleva valintanappi on valittuna.


![Luo sovelluksen salasanat](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="creating-app-passwords"></a>Sovelluksen salasanojen luominen
Käyttäjät voivat luoda sovelluksen salasanat niiden ensimmäisen rekisteröinnin yhteydessä.  Käyttäjät saavat vaihtoehto, jonka avulla voi luoda niitä rekisteröintiprosessi loppuun.

Lisäksi käyttäjät voivat luoda myös sovelluksen salasanat myöhemmin Azure-portaalissa, Office 365-portaalin asetusten muuttamisesta tai mukaan

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Sovelluksen salasanat luominen Office 365-portaalissa
--------------------------------------------------------------------------------


1. Kirjaudu Office 365-portaaliin
2. Valitse oikeassa yläkulmassa asetukset-pienoissovelluksella
3. Valitse vasemmassa reunassa muita suojauksen todentaminen
4. Valitse oikeanpuoleisesta **päivittää Omat puhelinnumerot tilin suojaukseen**
5. Proofup-sivun yläreunassa, valitse sovelluksen salasanat
6. Valitse **Luo**
7. Kirjoita nimi sovelluksen salasana ja valitse **Seuraava**
8. Ohjelman salasana Kopioi Leikepöydälle ja liitä sovelluksen.

<center>![Cloud](./media/multi-factor-authentication-whats-next/security.png)</center>


### <a name="to-create-app-passwords-in-the-azure-portal"></a>Luo sovelluksen salasanat Azure-portaalissa
--------------------------------------------------------------------------------
1. Kirjaudu Azure perinteinen-portaaliin.
3. Ylhäällä Napsauta käyttäjänimeä hiiren kakkospainikkeella ja valitse Lisää suojauksen vahvistus.
5. Proofup-sivun yläreunassa, valitse sovelluksen salasanat
6. Valitse **Luo**
7. Kirjoita nimi sovelluksen salasana ja valitse **Seuraava**
8. Ohjelman salasana Kopioi Leikepöydälle ja liitä sovelluksen.


![Sovelluksen salasanat](./media/multi-factor-authentication-whats-next/app2.png)

### <a name="to-create-app-passwords-if-you-do-not-have-an-office-365-or-azure-subscription"></a>Luo sovelluksen salasanat, jos sinulla ei ole Office 365- tai Azure-tilaus
--------------------------------------------------------------------------------
1. [Https://myapps.microsoft.com](https://myapps.microsoft.com) kirjautuminen
2. Ylhäällä Valitse profiili.
3. Valitse käyttäjänimi ja valitse Lisää suojauksen vahvistus.
5. Proofup-sivun yläreunassa, valitse sovelluksen salasanat
6. Valitse **Luo**
7. Kirjoita nimi sovelluksen salasana ja valitse **Seuraava**
8. Ohjelman salasana Kopioi Leikepöydälle ja liitä sovelluksen.

![Sovelluksen salasanat](./media/multi-factor-authentication-whats-next/myapp.png)

## <a name="remember-multi-factor-authentication-for-devices-users-trust"></a>Muista Monimenetelmäisen todentamisen laitteiden käyttäjien luota

Monimenetelmäisen todentamisen muistaminen laitteet ja selaimet, että käyttäjät luota vapaa toimintoa MFA kaikille käyttäjille.  Sen avulla voit antaa käyttäjien määrittäminen päivien onnistunut suorituksen jälkeen ohituksen MFA mahdollisuus Kirjaudu sisään käyttämällä MFA. Näin voit parantaa käyttäjien käytettävyyttä.

Käyttäjät voivat muista MFA luotettujen laitteiden, koska tämä ominaisuus voi vähentää tilin suojausta. Tilin suojausta pitäisi palauttaa Monimenetelmäisen todentamisen niiden laitteiden jommankumman seuraavissa tilanteissa:

- Jos niiden Yritystili on tullut käytetä luvattomasti
- Jos tallennettujen laitteen kadonneita tai varastamiselta

> [AZURE.NOTE] Tämä ominaisuus on toteutettu selaimen eväste välimuistin. Se ei toimi, jos selaimen evästeet eivät ole käytössä.

### <a name="how-to-enabledisable-remember-multi-factor-authentication"></a>Voit ottaa käyttöön/poistaa käytöstä muista monimenetelmäisen todentamisen

1. Kirjaudu Azure perinteinen-portaaliin.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Kohdassa Active Directory-hakemiston haluat laitteiden muista Monimenetelmäisen todentamisen määrittäminen.
4. Valitse kansio on valittuna, Määritä.
5. Valitse multi-factor authentication-osassa Hallitse Palveluasetukset.
6. Valitse Palveluasetukset-sivulla käyttäjän laitteen asetusten hallinta-Valitse **Salli käyttäjien muista monimenetelmäisen todentamisen luottaa laitteilla**tai poista sen valinta.
![Muista laitteet](./media/multi-factor-authentication-whats-next/remember.png)
8. Määritä, jonka haluat sallia keskeyttäminen päivien määrän. Oletusarvo on 14 päivän ajan.
9. Valitse Tallenna.
10. Valitse Sulje.


## <a name="selectable-verification-methods"></a>Voidaan valita vahvistus menetelmät
Cloud ja paikallisen versioita voit määrittää mitä vahvistus menetelmiä ovat käytettävissä käyttäjien. Seuraavassa taulukossa esitellään lyhyesti kukin menetelmää.

Kun käyttäjien rekisteröinti MFA niiden tilien ne Valitse niiden ensisijaiseksi tarkistustavaksi ulos otit haluamasi asetukset. Ohjeet niiden rekisteröinti prosessin käsitellään [määrittäminen tilini kaksivaiheista vahvistusta varten](multi-factor-authentication-end-user-first-time.md)

Menetelmä|Kuvaus
:------------- | :------------- |
Soita puhelimeen |  Sijoittaa automaattinen äänipuhelu todennus-puhelimeen. Käyttäjä vastaa puheluun ja painaa # puhelimen näppäimistön tarkistamiseen. Tämä puhelinnumero ei ole synkronoitu paikallisen Active Directory.
Tekstiviestin puhelimeen | Lähettää tekstiviestin tarkistuskoodi käyttäjälle, joka sisältää. Käyttäjää pyydetään joko tekstiviestin vastauksen tarkistuskoodi tai kirjoita tarkistuskoodi Kirjaudu sisään-käyttöliittymän.
Ilmoitus mobiilisovelluksen kautta | Tässä tilassa Microsoft Authenticator sovellus estää näin luvattoman pääsyn tilit ja lopettaa vilpilliseen tapahtumat. Tämä on valmis käyttämällä push-ilmoituksen puhelimella tai rekisteröidyn laitteen. Tarkastele vain ilmoituksen ja jos se on oikeutettu valitsemalla Tarkista. Voit muussa Valitse Estä tai estä ja raportin vilpilliseen ilmoituksen. Artikkelissa käyttämisestä estä ja raportin petoksen toiminto Monimenetelmäisen todentamisen tietojen raportoinnin vilpilliseen ilmoitukset.</br></br>Microsoft Authenticator-sovellus on käytettävissä [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)-, [Android](http://go.microsoft.com/fwlink/?Linkid=825072)- ja [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).|
Tarkistuskoodi mobiilisovelluksessa | Tässä tilassa Microsoft Authenticator-sovellus voidaan kuin ohjelmiston tunnuksen sijasta-tarkistuskoodi luomiseen. Tämä tarkistuskoodi lisättävissä sekä käyttäjänimi ja salasana, antaa käyttöoikeuden toisessa lomakkeessa.</li><br><p> Microsoft Authenticator-sovellus on käytettävissä [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071)-, [Android](http://go.microsoft.com/fwlink/?Linkid=825072)- ja [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

### <a name="how-to-enabledisable-authentication-methods"></a>Voit ottaa käyttöön/poistaa käytöstä todennustavat

1. Kirjaudu Azure perinteinen-portaaliin.
2. Valitse vasemmassa reunassa Active Directorysta.
3. Kohdassa Active Directory-hakemiston haluat ottaa käyttöön tai poistaminen käytöstä todennustavat.
4. Valitse kansio on valittuna, Määritä.
5. Valitse multi-factor authentication-osassa Hallitse Palveluasetukset.
6. Valitse Palveluasetukset-sivun vahvistus-asetukset-kohdassa Valitse tai poista sen valinta asetukset, jota haluat käyttää.</br></br>
![Tarkistus-asetukset](./media/multi-factor-authentication-whats-next/authmethods.png)
9. Valitse Tallenna.
10. Valitse Sulje.
