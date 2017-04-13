<properties
    pageTitle="Toimintojen lisääminen ensimmäisen onlineiin"
    description="Lisäominaisuuksien lisääminen ensimmäinen online muutaman minuutin kuluttua."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>Toimintojen lisääminen ensimmäisen onlineiin

[Ensimmäinen koodiin Azure päättymässä, ota](app-service-web-get-started.md)käyttöön otoksen verkkosovellukseen [Azure](../app-service/app-service-value-prop-what-is.md)-sovelluksen palveluun. Tässä artikkelissa lisätä hyvien joitakin toimintoja nopeasti käyttöön web App-sovellukseen. Muutaman minuutin kuluttua menettelet seuraavasti:

- pakottaa todentamisen käyttäjiä varten
- Skaalaa sovelluksen automaattisesti
- sovelluksen suorituskykyyn ilmoituksia

Riippumatta siitä, mihin otoksen sovellukseen Edellinen artikkeli käyttöön sinun on helppo seurata opetusohjelman.

Tässä opetusohjelmassa kolme toiminnot ovat vain muutamia esimerkkejä useita hyödyllisiä ominaisuuksia, jotka saat, kun lisäät web app-sovelluksen palvelun. Monet ominaisuudet ovat käytettävissä **vapaa** -tason (joka on mitä ensimmäisen web-sovellus on käynnissä), ja voit käyttää kokeiluversion hyvitykset kokeilemaan ominaisuuksia, jotka vaativat hinnat suurempi tasoa. Asennusprosessi, että koodiin pysyy **vapaa** taso, ellei erikseen, että muuttaa sen, eri hinnoittelu taso.

>[AZURE.NOTE] Luodessasi Azure CLI online suorittaa **vapaa** taso, jonka avulla vain yhden palvelinresurssien kiintiön jaetun AM-esiintymää. Lisätietoja Hae **vapaa** taso kanssa on artikkelissa [sovelluksen palvelun rajoitukset](../azure-subscription-service-limits.md#app-service-limits).

## <a name="authenticate-your-users"></a>Käyttäjien todentamiseen

Nyt seuraavaksi, miten helppoa on lisätä todennus (myös osoitteessa [App palvelun todennus/luvan](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)) sovelluksen.

1. -Sovelluksen portaalin sivu, jonka juuri avattu- **asetukset** > **todennusta / luvan**.  
    ![Todennus - asetukset-sivu](./media/app-service-web-get-started/aad-login-settings.png)

2. Valitse **Valitse** todennus käyttöön.  

4. Valitse **Käyttöoikeustarkistuspalvelun** **Azure Active Directory**.  
    ![Todennus - Valitse Azure AD](./media/app-service-web-get-started/aad-login-config.png)

5. Valitse **Azure Active Directory-asetukset** -sivu valitsemalla **Pika-asennus**ja valitse sitten **OK**. Oletusasetuksia Luo uusi Azure AD-sovelluksen oletusarvon hakemistossa.  
 ![Todennus - express määritys](./media/app-service-web-get-started/aad-login-express.png)

6. Valitse **Tallenna**.  
    ![Todennus - Tallenna määritys](./media/app-service-web-get-started/aad-login-save.png)

    Kun muutos on tarkistettu, näyttöön tulee ilmoitus äänimerkki vihreitä sekä helpossa muodossa olevan viestin.

7. Edellinen-sovelluksen portaalin sivu valitsemalla **URL-Osoitteen** linkin (tai **Selaa** valikkorivillä). Linkki on HTTP-osoite.  
    ![Todennus - Siirry URL-osoite](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Mutta kun se avautuu sovelluksen uudessa välilehdessä, URL-osoite ruutuun Uudelleenohjaa useita kertoja ja päättyy sovelluksen HTTPS-osoite. Mitä näet on, että olet jo kirjautunut Azure-tilauksesi ja olet automaattisesti todennus-sovelluksessa.  
    ![Todennus - kirjautunut sisään](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Jotta Jos avaat nyt Todentamattomille istunnon eri selainta, näet ja kirjaudu sisään-näyttöön, kun siirryt URL-Osoitetta.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   Jos olet koskaan valmis mitään Azure Active Directory-hakemistosta, oletusarvo-kansio ei ehkä ole Azure AD käyttäjiä. Siinä tapauksessa on ainoa tili ei todennäköisesti Azure-tilaus on Microsoft-tili. Minkä vuoksi on kirjautunut sisään automaattisesti aiemmin samassa selaimessa-sovellukseen.
   Voit kirjautua sisään tällä Kirjaudu sisään-sivulla samalla Microsoft-tiliin.

Onnittelut, ovat todennustapa kaikki liikenne web App-sovellukseen.

Olet ehkä jo huomannut **todennus / luvan** sivu, jotka voit tehdä paljon muuta, kuten:

- Ota käyttöön sosiaalisen kirjautuminen
- Ottaa käyttöön useita Kirjautumisasetukset
- Muuttaa oletustoiminnan, kun käyttäjät siirtyä ensin sovelluksen

Sovelluksen-palvelu sisältää Ota avain ratkaista joitakin yleisiä todennus on niin, että sinun ei tarvitse antaa käyttöoikeuden logiikan.
Lisätietoja on artikkelissa [Sovelluksen palvelun todennus/luvan](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Skaalaa sovelluksen automaattisesti tarvittaessa perusteella

Seuraavaksi oletetaan, että Automaattinen skaalaus sovelluksen niin, että se automaattisesti säätää kapasiteetti vastata käyttäjän vaatimus (myös [asteikko Azure-sovelluksen määrittäminen](web-sites-scale.md) ja [skaalata esiintymän Laske manuaalisesti tai automaattisesti](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Lyhyesti-koodiin skaalata kahdella tavalla:

- [Laajentaa](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Lisää suorittimen, muistin, levytilaa ja lisäominaisuuksia, kuten Oma VMs, mukautetut toimialueet ja todistuksista, väliaikaisen paikkojen ja automaattisen skaalauksen poistaminen. Voit skaalata muuttamalla sovelluksen kuuluu App palvelusopimus hinnoittelu taso.
- [Mittakaava,](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): AM määrän, jotka suoritetaan sovelluksen esiintymät.
Voit skaalata jopa 50 esiintymissä hinnoittelu tason mukaan.

Ado, ilman määritetään automaattisen skaalauksen poistaminen.

1. Ensin japanin skaalata ottaa käyttöön automaattisen skaalauksen poistaminen. Valitse sovelluksen portaalin sivu **asetukset** > **Asteikko ylöspäin (sovelluksen palvelun palvelupaketti)**.  
    ![Skaalaa ylös - asetukset-sivu](./media/app-service-web-get-started/scale-up-settings.png)

2. Vieritä ja valitse **S1 vakio** taso, joka tukee automaattisen skaalauksen poistaminen (ympyröity näyttökuvassa) ja valitse sitten **Valitse**pienin taso.  
    ![Skaalaa - tason valitseminen](./media/app-service-web-get-started/scale-up-select.png)

    Olet valmis skaalauksen ylöspäin.

    >[AZURE.IMPORTANT] Tämä taso expends oman maksuttoman kokeiluversion hyvitykset. Jos sinulla on maksamisen käyttö-tilin, se veloitetaan tiliisi.

3. Seuraavaksi määrittäminen oletetaan, että automaattisen skaalauksen poistaminen. Valitse sovelluksen portaalin sivu **asetukset** > **Mittakaava, (sovelluksen palvelun palvelupaketti)**.  
    ![Skaalaa ulos - asetukset-sivu](./media/app-service-web-get-started/scale-out-settings.png)

4. Muuta **asteikon mukaan** **Suorittimen prosentti**. Alla olevaa avattavaa valikkoa liukusäätimiä Päivitä vastaavasti. Määritä **esiintymät** -alueen välille **1** ja **2** **kohdealue** **80**– **40** . Viestin käsitteleminen kirjoittamalla ruutuihin tai liukusäätimillä.  
 ![Skaalaa ulos - Määritä automaattisen skaalauksen poistaminen](./media/app-service-web-get-started/scale-out-configure.png)

    Määritysten mukaan sovelluksen Skaalaa automaattisesti kun suorittimen käyttö on yli 80 % ja Skaalaa suorittimen käyttö on alle 40 %.

5. Valitse valikkoriviltä valitsemalla **Tallenna** .

Onnittelut-sovelluksen automaattisen skaalauksen poistaminen.

Olet ehkä jo huomannut, voit tehdä paljon muuta, kuten **Mittakaava-asetuksia** -sivu:

- Skaalaa tietystä numerosta esiintymien manuaalisesti
- Suorituskyvyn muita tietoja, kuten muistin prosenttiosuuden tai levy jonon skaalata
- Mukauta skaalauksen toimintaa, kun suorituskyvyn säännön käynnistäminen
- Aikataulun Automaattinen skaalaus
- Tulevien tapahtuman automaattisen skaalauksen poistaminen toiminnan määrittäminen

Saat lisätietoja sovelluksen ylöspäin skaalaus- [skaalata Azure-sovelluksen määrittäminen](../app-service-web/web-sites-scale.md). Saat lisätietoja laajentaminen [skaalata esiintymän Laske manuaalisesti tai automaattisesti](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Ilmoituksia, kun sovellus

Nyt kun sovellus on automaattisen skaalauksen poistaminen, mitä tapahtuu, kun se saavuttaa suurimman esiintymän määrä (2) ja Suoritin on suurempi kuin haluamasi käyttö (80 %)?
Voit määrittää (myös osoitteessa [Vastaanota ilmoitukset](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)) ilmoituksen asiasta tässä tilanteessa, jotta voit edelleen skaalata ylös/ulos sovellus, esimerkiksi. Oletetaan, että nopeasti määrittäminen ilmoituksen Tämä skenaario.

1. Valitse sovelluksen portaalin sivu **Työkalut** > **ilmoitukset**.  
    ![Ilmoitukset - asetukset-sivu](./media/app-service-web-get-started/alert-settings.png)

2. Valitse **Lisää-ilmoitus**. Valitse **resurssi** -ruutuun resurssi, joka päättyy **(serverfarms)**. Sovelluksen palvelusopimus on.  
    ![Ilmoitukset - Lisää ilmoittaa, kun sovellus-palvelusopimus](./media/app-service-web-get-started/alert-add.png)

3. Määritä **nimi** kuin `CPU Maxed`, **metrijärjestelmä** **Suorittimen prosentti**ja kuin **kynnysarvo** `90`, valitse **sähköpostin omistajat, osallistujat, ja lukijat**ja valitse sitten **OK**.   
 ![Määritä hälytykset - ilmoitus](./media/app-service-web-get-started/alert-configure.png)

    Azure on luonut ilmoituksen, kun se näkyvät **ilmoitukset** -sivu.  
    ![Ilmoitukset - valmis-näkymä](./media/app-service-web-get-started/alert-done.png)

Onnittelut, saat nyt ilmoitukset.

Ilmoitusten asetus tarkistaa suorittimen käyttö viiden minuutin välein. Jos luku yli 90 %, saat sähköposti-ilmoituksen, ja kuka tahansa, jolla on sallittua. Jos haluat nähdä kaikki henkilöt, joilla on oikeus ilmoitukset, palaa sovelluksen portaalin sivu ja **Access** -painiketta.  
![Katso saajan ilmoitukset](./media/app-service-web-get-started/alert-rbac.png)

Raportissa pitäisi näkyä **tilauksen valvojat** ovat jo sovelluksen **omistaja** . Tämän ryhmän lisätä tilin järjestelmänvalvojille Azure tilauksen (kuten kokeilutilauksen). Saat lisätietoja Azure Roolipohjainen käyttöoikeuksien valvonta [Azure Role-Based käyttöoikeuksien hallinta](../active-directory/role-based-access-control-configure.md).

> [AZURE.NOTE] Ilmoitusten sääntöjen on Azure ominaisuus. Lisätietoja on artikkelissa [Vastaanota ilmoitukset](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Ilmoituksen määrittäminen matkalla olet ehkä jo huomannut on monenlaisia tools- **Työkalut** -sivu. Tässä kohdassa voit tehdä ongelmista on suorituskyvyn seuranta testata heikkouksien, resurssien, käsitellä AM-konsolin ja lisää hyödyllisiä tunnisteet. Olemme kutsu napsauttamalla niitä yksitellen työkaluista saat tietää, yhdellä sormella vihjeitä yksinkertaisen mutta tehokkaan työkaluja.

Lue, miten entistä monipuolisemmin käyttöön-sovelluksessa. Ohessa on vain osittaisen luettelo:

- [Osta ja mukautetun toimialuenimen määrittäminen](custom-dns-web-site-buydomains-web-app.md) - Osta houkuttelevien toimialue, web App-sovelluksen sijaan *. azurewebsites.net toimialueen. Tai käyttää toimialuetta jo käytössäsi olevan.
- [Väliaikaisen ympäristöissä määrittäminen](web-sites-staged-publishing.md) - sovelluksen käyttöön ennen käyttöönottoa tuotannon väliaikaisen URL-osoite. Päivitä LUOTTAMUSVÄLI live web Appissa. Määritä monimutkaisia DevOps-ratkaisussa, jossa on useita käyttöönoton paikkaa.
- [Jatkuva käyttöönoton asetusten määrittäminen](app-service-continuous-deployment.md) - sovellusten käyttöönoton integroida järjestelmän Ohjausobjektin lähde. Ota käyttöön Azure jokaisen Vahvista kanssa.
- [Access - ympäristöön resurssit](web-sites-hybrid-connection-get-started.md) - aiemmin luodun Access paikallisen tietokannan tai CRM-järjestelmään.
- [Sovelluksen varmuuskopiointi](web-sites-backup.md) - määrittäminen takaisin ylös- ja palautustyön web App-sovelluksen. Valmistaudu odottamattomia virheitä ja palauttaa ne.
- IIS-lokeja [käyttöön vianmäärityslokit](web-sites-enable-diagnostic-log.md) - lukea Azure- tai jälkitiedot. Lukea niitä stream, lataa ne tai portti ne yhdeksi [Hakemuksen tiedot](../application-insights/app-insights-overview.md) Ota avain analyysia varten.
- [Etsi sovellus heikkouksien](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
skannata web-sovellusta Moderni uhkien myöntämä [Tinapaperi suojaus](https://www.tinfoilsecurity.com/)-palveluun.
- [Suorita taustan työt](../azure-functions/functions-overview.md) - töissä käsittelyn, raportointi, jne.
- [Lisätietoja sovelluksen palvelun toiminta](../app-service/app-service-how-works-readme.md)
