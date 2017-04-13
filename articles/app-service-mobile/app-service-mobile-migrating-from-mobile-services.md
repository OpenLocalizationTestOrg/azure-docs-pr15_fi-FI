<properties
    pageTitle="Siirtää Mobile-palvelujen App palvelun mobiilisovelluksessa"
    description="Opi siirtämään helposti Mobile-palvelut-sovellus App palvelun Mobile-sovellukseen"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>Siirtää aiemmin Azure mobiilipalvelun App Azure-palvelu

[Azure App palvelun yleiseen käyttöön]Azure Mobile-palveluiden sivustojen helposti siirtäminen käytönaikainen hyödyntää kaikkia Azure App palvelun toimintoja.  Tässä asiakirjassa kerrotaan toiminta, kun sivuston siirtämisestä Azure App palvelun Azure Mobile-palvelut.

## <a name="what-does-migration-do"></a>Mitä siirron tehdä sivustoon

Azure-mobiilipalvelun siirron muuttuu mobiilipalvelun ominaisuudet [Azure App palvelun] sovelluksen vaikuttamatta koodi.  Oman ilmoituksen keskittimet, SQL tietoyhteyden, todennusasetukset, ajoitetuissa ja toimialuenimen pysyvät muuttumattomina.  Azure-mobiilipalvelun käyttämisen mobiilisovellukset jatkaa normaalisti.  Siirron käynnistyy palvelussa, kun se siirretään Azure-sovelluksen palveluun.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Miksi kannattaa siirtää sivustoon

Microsoft on suositellaan siirtää Azure Mobile-palvelun hyödyntää Azure sovelluksen-palvelua, mukaan lukien toiminnot:

  *  Uusia Host (isäntä)-ominaisuuksia, kuten [WebJobs] ja [Mukautetut toimialueet].
  *  Paikallinen resurssien käyttäminen [VNet] lisäksi [Hybrid yhteyksiä]yhteydet.
  *  Seuranta ja uuden Relic tai [Hakemuksen tiedot]vianmääritys.
  *  Valmiin DevOps sillä, kuten [väliaikainen paikkaa], kuvat – Edellinen- ja -tuotannon testaamiseen.
  *  [Automaattinen skaalaus]kuormituksen tasaamisen ja [suorituskyvyn seurantaa].

Lisätietoja etuja Azure App palvelun on ohjeaiheessa [Services ja sovelluksen mobiilipalvelun] .

## <a name="before-you-begin"></a>Ennen aloittamista

Ennen aloittamista sivustosi pää työ, kannattaa [varmuuskopioida mobiilipalvelun ominaisuudet] komentosarjojen ja SQL-tietokantaan.

## <a name="migrating-site"></a>Siirtyminen sivustot

Siirtoprosessia siirtää kaikkien sivustojen yksittäisen Azure-alueella.

Sivuston siirtäminen:

  1.  Kirjaudu sisään [Azure perinteinen Portal].
  2.  Valitse alue, jonka haluat siirtää mobiilipalvelun ominaisuudet.
  3.  Valitse **Siirrä App palveluun** -painike.

    ![Siirrä-painike][0]

  4.  Lue artikkelista App Service-valintaikkuna.
  5.  Kirjoita Mobile-palvelun nimi ruutuun.  Esimerkiksi jos toimialuenimi on contoso.azure mobile.net, kirjoita _contoso_ tarkoitettuun ruutuun.
  6.  Napsauta jakoviivojen-painiketta.

Valvonta järjestelmän valvonnasta siirron tila. Sivuston lueteltujen *Siirtyminen* perinteinen Azure-portaalissa.

  ![Siirron järjestelmän valvonta][1]

Kunkin siirron niihin missä tahansa 3 15 minuuttia jätettävien mobiilipalvelu kohden.  Sivuston säilyvät siirron aikana.
Sivuston käynnistetään siirtoprosessia lopussa.  Sivusto ei ole käytettävissä uudelleenkäynnistyksen aikana, jotka voivat viimeksi muutaman sekunnin.

## <a name="finalizing-migration"></a>Siirron viimeisteleminen

Testaa sivustosi mobiilisovelluksen aina, kun siirtoprosessin suunnitelma.  Varmista, että voit suorittaa kaikki yleiset asiakas-toiminnot mobiilisovelluksen muuttamatta.  

### <a name="update-app-service-tier"></a>Valitse haluamasi sovellus-palvelun hinnat taso

Sinun on joustavammin hinnat Azure-sovelluksen palveluun siirron jälkeen.

  1.  Kirjaudu sisään [Azure portal].
  2.  Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla siirretyt mobiilipalvelun nimi.
  3.  Asetukset-sivu avautuu oletusarvoisesti.
  4.  Valitse Asetukset-valikossa **App palvelun suunnitteleminen** .
  5.  Napsauta **Hinnat taso** -ruutua.
  6.  Valitse tarpeisiisi sopiva ruutu ja valitse sitten **Valitse**.  Sinun täytyy ehkä valitsemalla **Näytä kaikki** käytettävissä olevat hinnoittelua tasoa.

Lähtökohtana Suosittelemme seuraavia tasoa:

| Mobiilipalvelu hinnoittelua taso | Sovelluksen palvelun hinnat taso |
| :-------------------------- | :----------------------- |
| Vapaa                        | F1 vapaa                  |
| Perustoiminnot                       | B1 Basic                 |
| Vakio                    | S1 vakio              |

Hinnat taso sovelluksen oikealle valitsemalla on haluamaksesi.  Lisätietoja [Sovelluksen palvelun hinnat] n tarkat tiedot, valitse uusi sovellus-palvelun hinnat.

> [AZURE.TIP]Sovelluksen palvelun vakio taso sisältää access useita toimintoja, joita haluat käyttää, mukaan lukien [väliaikaisen paikkaa], automaattisen varmuuskopioinnin ja Automaattinen skaalaus.  Katso ollessasi on uusia ominaisuuksia.

### <a name="review-migration-scheduler-jobs"></a>Tarkista siirretyt ajoituksen työt

Ajoituksen työt eivät ole näkyvissä ennen noin 30 minuuttia siirron jälkeen.  Ajoitetuissa edelleen suorittaa taustalla.
Voit tarkastella ajoitetut työt, kun ne ovat näkyvissä uudelleen seuraavasti:

  1.  Kirjaudu sisään [Azure portal].
  2.  Valitse **Selaa >**, kirjoita **aikataulun** _Suodatin_ -kenttään ja valitse sitten **Ajoituksen sivustokokoelmat**.

On ilmainen ajoituksen työt käytettävissä jälkeinen siirron rajoitettu määrä.  Tarkista käyttö ja [Azure ajoituksen suunnitelmien].

### <a name="configure-cors"></a>Määritä CORS tarvittaessa

Usean origin resurssien jakaminen on tekniikka Salli Web-sivuston käyttäminen WWW-Ohjelmointirajapinta, valitse toinen toimialue.  Jos käytössäsi on Mobile Azure-palveluihin liittyvä sivusto, voit joutua määrittämään CORS siirron osana.  Jos käytät käyttöoikeuspalvelinta Azure Mobile-palveluihin yksinomaan mobiililaitteista, valitse CORS ei tarvitse määritettävä lukuun ottamatta vain harvoin.

Siirretyt CORS-asetukset ovat käytettävissä **MS_CrossDomainWhitelist** sovelluksen-asetukseksi.  Voit siirtää sivuston App palvelun CORS-toimintoon:

  1.  Kirjaudu sisään [Azure portal].
  2.  Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla siirretyt mobiilipalvelun nimi.
  3.  Asetukset-sivu avautuu oletusarvoisesti.
  4.  Valitse **CORS** API-valikossa.
  5.  Kirjoita kaikki sallittu alkuperän tarkoitettuun-ruutuun painamalla Enter-näppäintä jokaisen komennon jälkeen.
  6.  Kun sallittu alkuperistä luettelo on oikein, valitse Tallenna-painiketta.

> [AZURE.TIP]Azure-sovelluksen palveluun eduista on, että voit suorittaa sivuston ja mobiilipalvelu samassa sivustossa.  Lisätietoja on kohdassa [vaiheisiin](#next-steps) .

### <a name="download-publish-profile"></a>Lataa uusi Julkaisuprofiili

Sivuston julkaisun profiilin muutetaan siirrettäessä Azure-sovelluksen palveluun.  Jos haluat julkaista sivuston Visual Studion, sinun on julkaistava uusi profiili.  Voit ladata uuden julkaisun profiilin seuraavasti:

  1.  Kirjaudu sisään [Azure portal].
  2.  Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla siirretyt mobiilipalvelun nimi.
  3.  Valitse **Hae Julkaise profiili**.

PublishSettings tiedosto ladataan tietokoneeseen.  Tätä kutsutaan tavallisesti _nimi_. PublishSettings.  Julkaise-asetusten tuominen olemassa olevan projektin:

  1.  Avaa Visual Studio ja Azure mobiilipalvelun projektin.
  2.  Napsauta **Ratkaisunhallinnassa** projektin hiiren kakkospainikkeella ja valitse **Julkaise...**
  3.  Valitse **Tuo**
  4.  Valitse **Selaa** ja valitse kohdassa ladatut julkaista asetustiedosto.  Valitse **OK**
  5.  Valitse **Vahvista yhteyden** varmistamiseksi Julkaise asetukset toimivat.
  6.  Valitse **Julkaise** -sivuston.


## <a name="working-with-your-site"></a>Sivustossa siirtymisen jälkeisten käsitteleminen

Aloita uusi sovellus palvelu [Azure portal] jälkeinen siirtämistä käsitteleminen.  Seuraavassa on joitakin tiettyjä toiminnoista, joita olet käyttänyt [Azure perinteinen Portal]-sovellus palvelun vastaavia ja muistiinpanot.

### <a name="publishing-your-site"></a>Lataamalla ja siirretyt Julkaisusivusto

Sivustosi on käytettävissä git tai ftp ja julkaista eri eri keinoja, kuten WebDeploy, TFS, Mercurial, GitHub ja FTP kanssa.  Käyttöönotto-tunnistetiedot siirretään sivuston muiden kanssa.  Jos et ole määrittänyt käyttöönoton tunnistetiedot tai et muista niitä, voit palauttaa ne:

  1. Kirjaudu sisään [Azure portal].
  2. Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla siirretyt mobiilipalvelun nimi.
  3. Asetukset-sivu avautuu oletusarvoisesti.
  4. Valitse **käyttöönoton tunnistetiedot** julkaisu-valikosta.
  5. Kirjoita uuden käyttöönoton käyttäjätiedot-ruutuihin ja valitse sitten Tallenna-painiketta.

Voit käyttää näitä tunnistetietoja Kloonaa git sivustoon tai määrittää automaattisia ominaisuuksissa GitHub, TFS tai Mercurial.  Katso lisätietoja [Azure App palvelun käyttöönoton ohjeet].

### <a name="appsettings"></a>Sovelluksen asetukset

Useimmat siirretyt mobiilipalvelu asetukset ovat käytettävissä App-asetukset.  Saat [Azure portal]-sovelluksen asetusten luettelo.
Voit tarkastella tai muuttaa app-asetukset:

  1. Kirjaudu sisään [Azure portal].
  2. Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla siirretyt mobiilipalvelun nimi.
  3. Asetukset-sivu avautuu oletusarvoisesti.
  4. Valitse Yleiset-valikossa **asetukset** .
  5. Vieritä sovelluksen asetukset-kohta ja Etsi sovellus-asetus.
  6. Valitse Muokkaa arvoa sovelluksen-asetuksen arvoa.  Valitse Tallenna arvo **Tallenna** .

Voit päivittää useita sovelluksen asetusten samanaikaisesti.

> [AZURE.TIP]On kaksi sovellusasetuksia, joilla on sama arvo.  Esimerkiksi näkyviin voi tulla _ApplicationKey_ ja _MS\_ApplicationKey_.  Päivitä sekä sovellusasetukset samanaikaisesti.

### <a name="authentication"></a>Todennus

Kaikki todennusasetukset ovat käytettävissä sovelluksen siirretyt sivuston asetukset.  Päivitä todennuksen asetukset, voit muuttaa haluamasi app-asetukset.  Seuraavassa taulukossa on todennus-palveluntarjoajan tarvittavat app-asetukset:

| Toimittaja          | Asiakastunnus                 | Asiakkaan salaisuus                | Muut asetukset             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Microsoft-tili | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebook          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter-           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure AD          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Huomautus: **MS\_AadTenants** on tallennettu CSV-vuokraajan toimialueiden ("Sallittu Alihallinnat" kentät Mobile-palvelut-portaalissa) luetteloa.

> [AZURE.WARNING] **Älä käytä todennus-järjestelmiä asetukset-valikko**
>
> Azure sovelluksen-palvelu sisältää erillisen "koodittomia" todennus- ja järjestelmä-kohdassa _todennus / luvan_ asetukset-valikko ja (vanhentunut) _Mobile todennusta_ -asetus asetukset-valikossa.  Nämä vaihtoehdot eivät ole yhteensopivia siirretyt Azure mobiilipalvelun.  Voit hyödyntää Azure App palvelun todennus [päivittää sivustoon](app-service-mobile-net-upgrading-from-mobile-services.md) .

### <a name="easytables"></a>Tietoja

_Tiedot_ -välilehden Mobile-palvelut on korvattu _Helposti taulukoiden_ Azure portaalissa.  Voit käyttää helposti taulukot seuraavasti:

  1. Kirjaudu sisään [Azure portal].
  2. Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla siirretyt mobiilipalvelun nimi.
  3. Asetukset-sivu avautuu oletusarvoisesti.
  4. Valitse NUOLET-valikon **helposti taulukot** .

Voit lisätä taulukkoon valitsemalla **Lisää** -painiketta tai käyttää aiemmin luotuja taulukoita valitsemalla taulukkonimi.  On eri toiminnoista, jotka voit tehdä tämän sivu, mukaan lukien:

* Taulukon käyttöoikeuksien muuttaminen
* Toiminnallisia komentosarjojen muokkaaminen
* Taulukkorakenteen hallinta
* Taulukon poistaminen
* Taulukon sisällön tyhjentäminen
* Taulukon rivien poistaminen

### <a name="easyapis"></a>OHJELMOINTIRAJAPINTA

_API_ -välilehden Mobile-palvelut on korvattu _Helposti ohjelmointirajapinnan_ Azure portaalissa.  Voit käyttää helposti ohjelmointirajapinnan seuraavasti:

  1. Kirjaudu sisään [Azure portal].
  2. Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla siirretyt mobiilipalvelun nimi.
  3. Asetukset-sivu avautuu oletusarvoisesti.
  4. Valitse **Helposti ohjelmointirajapinnan** MOBIILI-valikossa.

Siirretyt ohjelmointirajapinnan on jo lueteltu sivu.  Voit myös lisätä Ohjelmointirajapinnan tämä sivu.  Voit hallita tietyn API valitsemalla Ohjelmointirajapinnan.
Uusi sivu-voit muuttaa käyttöoikeuksia ja muokata komentosarjoja Ohjelmointirajapinnan.

### <a name="on-demand-jobs"></a>Ajoituksen työt

Kaikki ajoituksen työt ovat saatavilla ajoituksen työn sivustokokoelmat-osassa.  Voit käyttää ajoituksen työt seuraavasti:

  1. Kirjaudu sisään [Azure portal].
  2. Valitse **Selaa >**, kirjoita **aikataulun** _Suodatin_ -kenttään ja valitse sitten **Ajoituksen sivustokokoelmat**.
  3. Valitse sivuston työ-sivustokokoelman.  Sen nimi on _sivuston nimi_-työt.
  4. Valitse **asetukset**.
  5. Valitse **ajoitus töiden** hallinta-kohdasta.

Ajoitetuissa näkyvät usein määritit ennen siirtoa.  Tarvittaessa työt eivät ole käytettävissä.  Ohjattu projektin suorittaminen

  1. Valitse työ, jonka haluat suorittaa.
  2. Valitse tarvittaessa käyttöön työn **ottaminen käyttöön** .
  3. Valitse **asetukset**ja sitten **aikataulu**.
  4. Valitse toistuminen, **kerran**ja valitse sitten **Tallenna**

Tarvittaessa työt sijaitsevat `App_Data/config/scripts/scheduler post-migration`.  On suositeltavaa muuntaa kaikki tarvittaessa työt [WebJobs] tai - [funktioita].  Kirjoita uuden ajoituksen työt [WebJobs] tai [funktioita].

### <a name="notification-hubs"></a>Ilmoitus keskittimet

Mobile-palvelut käyttää ilmoituksen keskittimet push-ilmoitukset.  Ilmoitus-toiminnossa linkittäminen mobiilipalvelun ominaisuudet siirron jälkeen käytetään App-asetukset:

| Sovelluksen käyttäjänimiasetusta                    | Kuvaus                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | Ilmoitus-toiminnossa Namespace           |
| **MS\_NotificationHubName**             | Ilmoitus-toiminnossa nimi                |
| **MS\_NotificationHubConnectionString** | Ilmoitus-toiminnossa yhteysmerkkijonon   |
| **MS\_NamespaceName**                   | Aliaksen MS_PushEntityNamespace      |

Ilmoitus-toiminnossa hallitaan [Azure portal]avulla.  Huomautus ilmoituksen keskittimeen nimi (löydät tämän sovelluksen asetusten):

  1. Kirjaudu sisään [Azure portal].
  2. Valitse **Selaa**>, valitse **Ilmoituksen keskittimet**
  3. Valitse liittyvä mobiilipalvelun ilmoitus-toiminnossa nimi.

> [AZURE.NOTE]Ilmoitus-toiminnossa on "Sekoitus", jos se ei ole näkyvissä.  "Eri" Kirjoita ilmoituksen keskittimet käyttämiseen ilmoituksen keskittimet ja aiempien versioiden palvelun Bus ominaisuuksia.  [Muuntaa erilaiset nimitilan] ennen jatkamista.  Kun muunnos on valmis, ilmoitus-toiminnossa näkyy [Azure portal].

Saat lisätietoja Tarkista [Ilmoituksen keskittimet] ohjeissa.

> [AZURE.TIP]Ilmoitus keskittimet hallintaominaisuudet [Azure portaalissa] on yhä esikatselu.  [Azure perinteinen portaalissa] on edelleen käytettävissä kaikissa ilmoituksen keskittimissä hallintaan.

### <a name="legacy-push"></a>Aiempien versioiden Push-asetukset

Jos olet määrittänyt Push mobiili-palvelusta ennen käyttöönottoa-ilmoituksen keskittimet, käytössäsi on _Vanha push_.  Jos käytössäsi on Push ja ei ole näkyvissä luettelossa kokoonpanoa ilmoituksen keskittimeen, on todennäköisesti käytössäsi on _Vanha push_.  Tämä toiminto on siirretty kaikkien muiden toimintojen kanssa.  Suosittelemme kuitenkin, että päivität ilmoituksen porttiin heti, kun siirto on valmis.

Välin kaikki vanhat push-asetukset (kanssa APN-sertifikaatti huomattavat poikkeus) ovat käytettävissä App-asetukset.  Päivitä APN-sertifikaatti korvaamalla tiedostojärjestelmän tiedostosta.

### <a name="app-settings"></a>Sovelluksen muut asetukset

Lisää sovellus seuraavat asetukset ovat siirretyt Mobile-palvelusta ja *asetukset*-kohdassa käytettävissä > *App-asetukset*:

| Sovelluksen käyttäjänimiasetusta              | Kuvaus                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | Sovelluksen nimi                    |
| **MS\_MobileServiceDomainSuffix** | Toimialueen etuliite. ts Azure-mobile.net |
| **MS\_ApplicationKey**            | Sovellus-näppäintä                    |
| **MS\_MasterKey**                 | Sovelluksen perustyyli key-tuotetunnuksen                     |

Sovellusnäppäintä ja avaimen ovat samat sovelluksen näppäimiä alkuperäisen Mobile-palvelusta.  Erityisesti sovellusnäppäintä lähetetään mukaan mobiilisovellukset vahvistamiseen mobile Ohjelmointirajapinnan käyttäminen.

### <a name="cliequivalents"></a>Komentorivin vastineet

Voit hallita Azure Mobile Services-sivuston enää _azure mobiili_ -komennon avulla.  Sen sijaan _azure site_ -komento on korvattu monia funktioita.  Seuraavan taulukon avulla voit saada vastineet yleiset komennot:

| _Azure Mobile_ Komento                     | Vastaavat _Azure Site_ -komento            |
| :----------------------------------------- | :----------------------------------------- |
| Mobiili sijainnit                           | sivuston sijaintiluettelosta                         |
| Mobiili-luettelo                                | sivustoluettelon                                  |
| Mobiili Näytä _nimi_                         | Näytä _nimi_                           |
| Mobiili uudelleenkäynnistyksen _nimi_                      | sivuston uudelleenkäynnistyksen _nimi_                        |
| Mobiili laukaisun _nimi_                     | sivuston käyttöönoton laukaisun _commitId_ _nimi_ |
| Mobiili avain määritetty _nimi_ _tyyppi_ - _arvo_       | sivuston appsetting Poista _avaimen_ _nimi_ <br/> _sivuston appsetting plusnäppäin_=_arvon_ _nimi_ |
| Mobiili-config luettelon _nimi_                  | sivuston appsetting luettelon _nimi_                |
| Mobiili-config Hae _nimen_ _avain_             | sivuston appsetting Näytä _avaimen_ _nimi_          |
| Mobiili-config määrittää _nimen_ _avain_             | sivuston appsetting Poista _avaimen_ _nimi_ <br/> _sivuston appsetting plusnäppäin_=_arvon_ _nimi_ |
| Mobiili toimialueen _nimi_                  | sivuston toimialueen _nimi_                    |
| Mobiili toimialueen _nimi_ - _toimialueen_ lisääminen          | sivuston toimialueen lisääminen _toimialueen_ _nimi_            |
| Mobiili toimialueen poistaminen _nimi_                | toimialueen poistaminen _toimialueen_ _nimi_         |
| Mobiili asteikko Näytä _nimi_                   | Näytä _nimi_                           |
| Mobiili asteikko _nimen_ muuttaminen                 | sivuston asteikko tilassa _tila_ _nimi_ <br /> sivuston asteikko esiintymät _esiintymät_ _nimi_ |
| Mobiili appsetting luettelon _nimi_              | sivuston appsetting luettelon _nimi_                |
| Mobiili appsetting Lisää _ _avaimen_ _arvo_ _ | _sivuston appsetting plusnäppäin_=_arvon_ _nimi_   |
| Mobiili appsetting Poista _nimen_ _avain_      | sivuston appsetting Poista _avaimen_ _nimi_        |
| Mobiili appsetting Näytä _nimen_ _avain_        | sivuston appsetting Poista _avaimen_ _nimi_        |

Päivitä-todennuksen tai push ilmoitusasetusten määrittäminen päivittämällä sovelluksen haluamasi asetus.
Tiedostojen muokkaaminen ja julkaista sivuston ftp- tai git kautta.

### <a name="diagnostics"></a>Diagnostiikka ja kirjaaminen

Diagnostiikan kirjaus on yleensä poissa käytöstä Azure-sovelluksen-palvelussa.  Ota Diagnostiikan kirjaus käyttöön seuraavasti:

  1. Kirjaudu sisään [Azure portal].
  2. Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla siirretyt mobiilipalvelun nimi.
  3. Asetukset-sivu avautuu oletusarvoisesti.
  4. Valitse Toiminnot-valikon **Vianmäärityslokit** .
  5. **Seuraavat lokit napsauttamalla** : **Sovelluksen Logging (tiedostojärjestelmän)**, **yksityiskohtaiset virhesanomat**ja **epäonnistui pyynnön jäljittäminen**
  6. Valitse **Tiedostojärjestelmän** Web palvelimen kirjaaminen
  7. Valitse **Tallenna**

Voit tarkastella lokit seuraavasti:

  1. Kirjaudu sisään [Azure portal].
  2. Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla siirretyt mobiilipalvelun nimi.
  3. Valitse **Työkalut** -painike
  4. Valitse **Log Stream** KÄYTTÄJÄTIETUEISSA-valikossa.

Lokit näkyvät ikkunan, ne on luotu.  Voit ladata lokitiedot tunnistetiedoillasi käyttöönoton myöhempää käyttöä varten. Lisätietoja on ohjeissa [lokiin kirjaaminen] .

## <a name="known-issues"></a>Tunnetut ongelmat

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Siirtää Mobile sovelluksen-Kloonaa poistaminen saa sivuston käyttökatkosta

Jos Kloonaa oman siirretyt mobiilipalvelu Azure PowerShellin avulla ja valitse Poista Kloonaa tuotannon-palvelun DNS-merkintä poistetaan.  Sivustosi on enää käyttää Internetistä.  

Ratkaisu: Jos haluat kopioida sivustoon, tee niin portaalin kautta.

### <a name="changing-webconfig-does-not-work"></a>Vaihtaminen Web.config ei onnistu

Jos sinulla on ASP.NET-sivuston, `Web.config` tiedoston Hae ei käytetä.  Azure-sovelluksen palvelun muodostaa sopivat `Web.config` tiedostoa käynnistyksen tukemaan Mobile-palveluihin suorituksen aikana.  Voit ohittaa tietyt asetukset (esimerkiksi mukautetut otsikot) XML-muunnos-tiedoston avulla.  Tiedoston luominen kohdassa kutsutaan `applicationHost.xdt` -tiedosto on end `D:\home\site` hakemiston Azure-palvelusta.  Lataa `applicationHost.xdt` arkistoidaan mukautettu käyttöönotto-komentosarjan kautta tai käyttämällä suoraan Kudu.  Seuraavassa kuvassa näkyy asiakirjaa:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Lisätietoja on ohjeissa [XDT Transform näytteiden] GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Siirretyt Mobile-palveluihin ei voi lisätä liikenteen hallinta

Kun luot liikenteen hallinta profiilin, et voi valita suoraan siirretyt mobiilipalvelu profiiliin.  Käytä "ulkoisen päätepisteen."  Ulkoinen päätepiste voidaan lisätä vain PowerShellin kautta.  Lisätietoja on artikkelissa [opetusohjelma liikenteen hallinta](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun sovellus on siirretty sovellus-palveluun, on entistä enemmän toimintoja, voit käyttää:

  * Käyttöönoton [väliaikaisen paikkojen] avulla voit tehdä muutoksia sivustoon vaiheen ja suorittaa A / B testaamiseen.
  * [WebJobs] on tarvittaessa ajoitetut työt korvaa.
  * Voit [ottaa jatkuvasti] sivuston linkittämällä sivuston GitHub, TFS tai Mercurial.
  * [Hakemuksen tiedot] avulla voit seurata sivustoa.
  * Yhteyshenkilönä sivusto- ja Mobile-Ohjelmointirajapinnan sama koodi.

### <a name="upgrading-your-site"></a>Mobile Services-sivuston päivittämisestä Azure Mobile-sovellusten SDK

  * Projektien Node.js-palvelimeen uuden [Mobile-sovellusten Node.js SDK] on useita uusia ominaisuuksia. Esimerkiksi voit nyt tehdä paikallista kehittämistä ja virheiden, käytä edellä 0,10 Node.js mikä tahansa versio ja mukauttaa minkä tahansa Express.js middleware.

  * . VERKON-pohjaisen palvelimen projektien uudet [Mobile Apps SDK NuGet paketit](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) on joustavammin NuGet riippuvuuksista.  Nämä paketit uuden sovelluksen Service-todennusta ja kirjoita kaikki ASP.NET-projekti. Lisätietoja päivittäminen on artikkelissa [päivittää aiemmin .NET-mobiilipalvelun sovelluksen-palveluun](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[Sovelluksen palvelun hinnat]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Hakemuksen tiedot]: ../application-insights/app-insights-overview.md
[Automaattinen skaalaus]: ../app-service-web/web-sites-scale.md
[Azure sovelluksen-palvelu]: ../app-service/app-service-value-prop-what-is.md
[Azure App palvelun käyttöönoton ohjeet]: ../app-service-web/web-sites-deploy.md
[Azure perinteinen Portal]: https://manage.windowsazure.com
[Azure portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure ajoitus-Palvelupaketit]: ../scheduler/scheduler-plans-billing.md
[jatkuvasti käyttöönotto]: ../app-service-web/app-service-continuous-deployment.md
[Muuntaa erilaiset nimitilan]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[mukautettuja toimialuenimiä]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[Azure-sovelluksen palvelun yleiseen käyttöön]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybrid yhteydet]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Lokiin kirjaaminen]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobiilisovellusten Node.js SDK-paketissa]: https://github.com/azure/azure-mobile-apps-node
[Palvelut ja sovelluksen mobiilipalvelu]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Ilmoitus keskittimet]: ../notification-hubs/notification-hubs-push-notification-overview.md
[suorituskyvyn seurantaa]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Varmuuskopioi mobiilipalvelun ominaisuudet]: ../mobile-services/mobile-services-disaster-recovery.md
[väliaikaisen paikkaa]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT muunto-objektit]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Funktiot]: ../azure-functions/functions-overview.md
