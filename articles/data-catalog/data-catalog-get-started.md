<properties
    pageTitle="Tietojen käytön aloittaminen | Microsoft Azure"
    description="Lopusta loppuun-opetusohjelma esittäminen skenaariot ja Azure tietoluettelon ominaisuuksista."
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>Azure tietoluettelon käytön aloittaminen
Azure tietoluettelon on täysin hallitun pilvipalvelussa, joka on järjestelmän rekisteröinnin ja yrityksen tietojen varoja etsiminen järjestelmän. Lisätietoja [on](data-catalog-what-is-data-catalog.md)artikkelissa Azure tietoluettelon.

Tässä opetusohjelmassa avulla pääset alkuun Azure tietoluettelon. Tässä opetusohjelmassa tekemällä seuraavat toimet:

| Toimintosarja | Kuvaus |
| :--- | :---------- |
| [Säännöstä tietoluettelo](#provision-data-catalog) | Tässä toimintosarjassa valmistelu tai Azure tietoluettelon määritetään. Voit tehdä tässä vaiheessa vain, jos luettelo ei ole määritetty ennen. Käytössä voi olla vain yksi tietoluettelon kohti organisaation (Microsoft Azure Active Directory-toimialueen), vaikka on useita tilauksia Azure tilisi. |
| [Tietoja resurssien rekisteröiminen](#register-data-assets) | Tässä toimintosarjassa rekisteröidä tietojen omaisuuden AdventureWorks2014 mallitietokannan tehostaminen tietoluettelon. Rekisteröinnissä annetaan avaimen rakenteellisia metatietoja, kuten nimiä, tyyppi ja sijainnit purkaminen tietolähteen ja kopioimalla kyseisen metatiedot-luettelossa. Tietolähteen ja tietojen varat säilyvät kohtaa, johon ne ovat, mutta metatiedot käytetään luettelon, jotta ne on helpompi löydettävissä ja ymmärrettäviä. |
| [Tutustu tietojen resurssit](#discover-data-assets) | Prosessin myöhemmässä vaiheessa Käytä Azure tietoluettelon portaalin tuttuihin tietojen omaisuus, joka on rekisteröity edellisessä vaiheessa. Kun tietolähde on rekisteröity Azure tietoluettelon, sen metatietoja on indeksoitu-palvelun niin, että käyttäjät voivat helposti etsiä tarvitsemansa tiedot. |
| [Tietoja resurssien huomautuksia](#annotate-data-assets) | Tässä toimintosarjassa huomautukset (tietoja, kuten kuvaukset, tunnisteita, asiakirjat tai asiantuntijoiden) säädettävä tietojen kalusto. Nämä tiedot täydentää uuttaa tietolähteen ja halutaan tietolähteen Lisää ymmärrettäviä osallistujia metatiedot. |
| [Tietoja resurssien yhdistäminen](#connect-to-data-assets) | Tässä toimintosarjassa Avaa tietojen varat integroitu asiakastyökaluissa (kuten Excel- ja SQL Server Data Tools) ja -integroitu työkalu (SQL Server Management Studiossa). |
| [Tietoja resurssien hallinta](#manage-data-assets) | Prosessin myöhemmässä vaiheessa voit määrittää suojauksen tietojen resurssien osalta. Tietoluettelo ei antaa käyttäjille itse tietoihin. Tietolähteen omistaja voi hallita tietojen käyttöä. <br/><br/> Tietoluettelon avulla voit tutustua tietolähteet ja Näytä **metatietojen** liittyvät rekisteröity luettelon lähteet. Voi olla tilanteessa, jossa tietolähteiden pitäisi olla näkyvissä vain tietyille käyttäjille tai tietyn ryhmissä. Näissä tilanteessa voit käyttää tietojen omistajuutta rekisteröidyt tiedot luettelon sisällä ja hallita omistat varat näkyvyyden. |
| [Tietoja resurssien poistaminen](#remove-data-assets) | Tässä kerrotaan tietojen varat poistamisesta tietoluettelon. |  

## <a name="tutorial-prerequisites"></a>Opetusohjelman edellytykset

### <a name="azure-subscription"></a>Azure-tilaus
Määrittämään Azure tietoluettelon on oltava omistaja tai rinnakkaisomistajan Azure-tilaukseen.

Azure tilaukset avulla voit järjestää cloud palvelun resurssien kuten Azure tietoluettelon käyttöoikeudet. Ne myös Ohje, voit määrittää, miten raportoidun resurssien käyttö, laskuttaa ja maksettu. Jokaisen tilauksen voi olla eri laskutus- ja asetukset, joten voi olla eri tilaukset ja erilaiset osaston, project, alueellisen office ja niin edelleen. Jokaisen pilvipalvelussa kuuluu tilauksen ja tarvitset tilauksesi, ennen kuin määrität Azure tietoluettelon. Lisätietoja on artikkelissa [Hallitse asiakkaat, tilaukset- ja järjestelmänvalvojan roolit](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Jos sinulla ei ole tilaus, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. [Maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/) lisätietoja.

### <a name="azure-active-directory"></a>Azure Active Directory
Määrittääksesi Azure tietoluettelon, sinun on kirjauduttava sisään Azure Active Directory (Azure AD)-käyttäjätilin. Sinun on oltava omistaja tai rinnakkaisomistajan Azure-tilaukseen.  

Azure AD on yrityksesi hallittavan tunnistetietojen ja käytön, sekä cloud ja paikallisen helposti. Voit käyttää yhden työpaikan tai oppilaitoksen tilille kirjaudutaan sisään cloud tai paikalliseen web-sovelluksen. Azure tietoluettelon käyttää Azure AD-kirjautuminen tarkistamiseen. Lisätietoja on artikkelissa [Azure Active Directory ominaisuudet](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory-käytännön määritys

T tilanteen, jossa voit kirjautuminen Azure tietoluettelon-portaaliin, mutta kun yrität kirjautua tietojen lähteen rekisteröinti-työkalu, kohtaat virhesanoma, joka estää kirjautuminen. Tämä virhe voi ilmetä, kun olet yrityksen verkossa tai jos olet muodostamassa ulkopuolella yrityksen verkkoon.

Rekisteröinti työkalu käyttää *todennusmenetelmien* vahvistamiseen Kirjaudu apuohjelmien käyttäjän Azure Active Directorysta. Onnistuneiden kirjautumista varten Azure Active Directory-järjestelmänvalvojan on otettava käyttöön todennusmenetelmien *Yleinen todennus käytännön*.

Yleinen todennus-käytäntö otetaan käyttöön todennus erikseen intranet- ja ekstranet-yhteydet seuraavassa kuvassa esitetyllä tavalla. Kirjautumisvirheitä saattaa ilmetä, jos todennusmenetelmien ei ole otettu käyttöön, joista olet muodostamassa verkossa.

 ![Azure Active Directory-yleinen todentaminen käytäntö](./media/data-catalog-prerequisites/global-auth-policy.png)

Lisätietoja on artikkelissa [määrittäminen todennus käytännöt](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Säännöstä tietoluettelo
Voit valmistella vain yksi tietoluettelon kohti organisaation (Azure Active Directory-toimialueen). Sen vuoksi, jos omistaja tai Azure-tilauksen rinnakkaisomistajan joka kuuluu Azure Active Directory-toimialueen tiedoissa on luotu luetteloon, ei osaat luoda luetteloon uudelleen, vaikka sinulla on useita tilauksia Azure. Testaa, onko tietoluettelon on luotu Azure Active Directory-toimialueen käyttäjä, siirry [Azure tietoluettelon kotisivun](http://azuredatacatalog.com) ja tarkistaa, onko näet luettelon. Jos luetteloon on jo luotu puolestasi, Ohita seuraavalla tavalla ja siirry seuraavaan osaan.    

1. Siirry [tietoluettelon palvelun-sivulla](https://azure.microsoft.com/services/data-catalog) ja valitse **Aloita**.

    ![Azure tietoluettelon--markkinoinnin aloitussivu](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Kirjaudu sisään käyttäjätili, jolla on omistaja tai rinnakkaisomistajan Azure-tilaukseen. Kun olet kirjautunut näkyvissä seuraava sivu.

    ![Azure tietoluettelon--säännöstä tietoluettelo](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Määritä **nimi** tietoluettelon ja **tilauksen** , jota haluat käyttää luettelon **sijainti** .
4. Laajenna **hinnoittelu** ja valitse Azure tietoluettelon **edition** (vapaa tai vakio).
    ![Azure tietoluettelon – Valitse edition](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Laajenna **Luettelon käyttäjät** ja valitse **Lisää** tietoluettelon käyttäjien lisäämisestä. Sinut lisätään automaattisesti tässä ryhmässä.
    ![Azure tietoluettelon--käyttäjät](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Laajenna **Luettelon järjestelmänvalvojat** ja valitsemalla **Lisää** voit lisätä muita järjestelmänvalvojien tietoluettelon. Sinut lisätään automaattisesti tässä ryhmässä.
    ![Azure tietoluettelon--Järjestelmänvalvojat](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Valitse **Luo luettelo** organisaation tietoluettelon luomiseen. Näet tietoluettelon kotisivulle sen luomisen jälkeen.
    ![Azure tietoluettelon--luotu](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Etsi tietoluettelon Azure-portaalissa
1. Erilliseen välilehteen selaimessa tai erillisessä selainikkunassa Siirry [Azure portal](https://portal.azure.com) ja kirjaudu sisään sama tili, jota käytit luodessasi tietoluettelon edellisessä vaiheessa.
2. Valitse **Selaa** ja valitse sitten **Tietoluettelon**.

    ![Azure tietoluettelon--Siirry Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) näet luomasi tietoluettelon.

    ![Azure tietoluettelon--näkymä-luettelon luettelossa](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Valitse luettelo, jonka loit. Näet portaalissa **Tietoluettelon** -sivu.

    ![Azure tietoluettelon--sivu-portaalissa ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. Voit tarkastella tietoluettelon ominaisuuksia ja päivittää ne. Esimerkiksi Valitse **hinnoittelu taso** ja muuta versio.

    ![Azure tietoluettelon--hinnoittelua taso](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works-mallitietokanta
Tässä opetusohjelmassa olet rekisteröinyt tietojen kalusto (taulukot) AdventureWorks2014 otoksen tietokannasta SQL Server-tietokantamoduulin, mutta voit käyttää mitä tahansa tuetut tietolähteet, jos haluat käsitellä tietoja, joka on tuttu ja rooli. Tuettujen tietolähteiden luettelo on ohjeaiheessa [Tuetut tietolähteet](data-catalog-dsr.md).

### <a name="install-the-adventure-works-2014-oltp-database"></a>Asenna Adventure Works 2014 OLTP-tietokanta
Adventure Works-tietokannan tukee standard online tapahtumien käsittely-skenaariot kuvitteellinen polkupyörän valmistajan (Adventure Works-jaksot), joka sisältää tuotteiden myynnin ja ostamisen. Tässä opetusohjelmassa rekisteröidä tuotetietoja Azure tietoluettelon kyselyjä.

Voit asentaa Adventure Works-mallitietokantaa seuraavasti:

1. Lataa codeplexissä [Adventure Works 2014 koko tietokannan Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) .
2. Palauttaa tietokannan käyttämääsi laitteeseen, noudata ohjeita [tietokannan varmuuskopio käyttämällä SQL Server Management Studiossa](http://msdn.microsoft.com/library/ms177429.aspx)tai toimimalla seuraavasti:
    1. Avaa SQL Server Management Studiossa ja muodosta yhteys SQL Server-tietokantamoduuli.
    2. **Tietokantojen** hiiren kakkospainikkeella ja valitse **Palauta tietokanta**.
    3. Valitse **Palauta tietokanta** **lähteen** **laite** -vaihtoehto ja valitse **Selaa**.
    4. Valitse **Valitse varmuuskopioinnin laitteet**-kohdassa **Lisää**.
    5. Siirry kansioon, jossa **AdventureWorks2014.bak** -tiedosto, valitse tiedosto ja valitse **OK** ja sulje **Varmuuskopiotiedosto Etsi** -valintaikkuna.
    6. Valitse **OK** ja sulje **Valitse varmuuskopioinnin laitteet** -valintaikkunassa.    
    7. Valitse Sulje **Palauta tietokanta** -valintaikkunassa **OK** .

Voit nyt rekisteröidä tietoja Adventure Works-mallitietokantaa omaisuuden Azure tietoluettelon avulla.

## <a name="register-data-assets"></a>Tietoja resurssien rekisteröiminen

Tässä voit rekisteröinti-työkalun rekisteröidä luettelon tietojen varat Adventure Works-tietokannasta. Rekisteröinnissä annetaan avaimen rakenteellisia metatietoja, kuten nimiä, tyyppi ja sijainnit purkaminen tietolähteen ja se sisältää varat ja kopioiminen kyseisen metatiedot-luettelossa. Tietolähteen ja tietojen varat säilyvät kohtaa, johon ne ovat, mutta metatiedot käytetään luettelon, jotta ne on helpompi löydettävissä ja ymmärrettäviä.

### <a name="register-a-data-source"></a>Rekisteröi tietolähde

1.  Siirry [Azure tietoluettelon aloitus-sivulla](https://azuredatacatalog.com) ja valitse **Julkaise tiedot**.

    ![Azure tietojen luettelon--tietojen julkaiseminen painike](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  Valitse Lataa ja asenna rekisteröinti-työkalun suorittaminen tietokoneessa **Käynnistä sovellus** .

    ![Azure tietoluettelon--käynnistyspainike](media/data-catalog-get-started/data-catalog-launch-application.png)

3. Valitse **Tervetuloa** -sivulla **Kirjaudu sisään** ja anna tunnistetiedot.    

    ![Azure tietoluettelon--aloitussivu](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. **Microsoft Azure tietoluettelon** sivulla Valitse **SQL Server** - ja **Seuraava**.

    ![Azure tietoluettelon--tietolähteet](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  Kirjoita SQL Server-yhteyden ominaisuudet **AdventureWorks2014** (katso seuraava esimerkki) ja valitse **Yhdistä**.

    ![Azure tietoluettelon--SQL Server-yhteyden asetukset](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  Rekisteröi tietojen resurssi metatiedot. Tässä esimerkissä rekisteröiminen **Tuotannon/tuotteen** objektit AdventureWorks tuotannon nimitila:

    1. Laajenna **AdventureWorks2014** **Palvelimen hierarkia** -kohtaa ja valitse **tuotannon**.
    2. Valitse **tuote**, **ProductCategory**, **ProductDescription**ja **ProductPhoto** käyttämällä Ctrl + napsautus.
    3. **Siirtää valitun nuolta** (**>**). Tämä toiminto siirtää kaikki valitut objektit **voi rekisteröidä objektit** -luettelosta.

        ![Azure tietoluettelon opetusohjelma – Etsi ja objektien valitseminen](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. Valitse **Sisällytä esikatselussa** sisällytettävät tiedot tilannevedoksen esikatselu. Tilannevedoksen sisältää enintään 20 kunkin taulukon tietueita ja se kopioidaan luettelon.
    5. Valitse **Sisällytä tietoja profiilin** sisältämään tilannevedoksen objektin tilastotiedot profiilin tiedot (esimerkiksi: vähintään, enintään ja keskimääräinen arvot sarakkeen rivien määrä).
    6. Kirjoita **Lisää tunnisteet** -kenttään **adventure works-jaksot**. Tämä toimenpide lisää nämä tiedot varat Etsi tunnisteet. Tunnisteet ovat erinomainen tapa auta käyttäjiä etsimään rekisteröity tietolähde.
    7. Määritä nimi **asiantuntija** tiedoista (valinnainen).

        ![Azure tietoluettelon opetusohjelma – voi rekisteröidä objektit](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. Valitse **Rekisteröi**. Azure tietoluettelon Rekisteröi valittuja objekteja. Tässä on rekisteröity Adventure Works-valittuja objekteja. Rekisteröinti-työkalun metatietojen poimii tietoja resurssi ja kopioi tiedot Azure tietoluettelon-palveluun. Tiedot pysyvät, johon tällä hetkellä se sijaitsee, ja se pysyy järjestelmänvalvojat-valvonnassa ja nykyisen järjestelmän käytännöt.

        ![Azure tietoluettelon--rekisteröity objektit](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. Rekisteröityjen tietolähdeobjektit näkyviin valitsemalla **Näytä Portal**. Vahvista Azure tietoluettelon-portaalissa, näet kaikki neljä taulukot ja tietokannan ruudukko-näkymässä.

        ![Objektien Azure tietoluettelon-portaalissa ](media/data-catalog-get-started/data-catalog-view-portal.png)


Tässä rekisteröity Adventure Works-mallitietokantaa objektit siten, että ne voi helposti löytää käyttäjien koko organisaatiossa. Seuraava Harjoitus opit miten saat tietää, rekisteröidyt tiedot resurssit.

## <a name="discover-data-assets"></a>Tutustu tietojen resurssit
Azure tietoluettelon etsiminen käyttää kaksi ensisijainen järjestelmiä: hakemisesta ja suodattamisesta.

Haku on suunniteltu intuitiivinen ja tehokas. Oletusarvon mukaan hakusanat, jotka vastaavat vastaan minkä tahansa luettelon, kuten käyttäjän antamaa huomautukset-ominaisuus.

Suodatus on suunniteltu täydentämään haku. Voit valita tiettyjä ominaisuuksia ovat esimerkiksi asiantuntijoiden, tietolähteen tyyppi, Objektilaji ja tunnisteiden avulla voit tarkastella vastaavat tiedot varat ja rajoittaa haun tulokset vastaavat varat.

Hakemisesta ja suodattamisesta yhdistelmän avulla voit siirtyä nopeasti tietolähteitä, joka on rekisteröity Azure tietoluettelon saat tietää, tarvitset tietoja varat.

Tässä käytetään Azure tietoluettelon portaalin tuttuihin tietojen varat edellisen Harjoitus on rekisteröity. Katso lisätietoja haun syntaksi [Tietoluettelohaku syntaksi](https://msdn.microsoft.com/library/azure/mt267594.aspx) .

Seuraavassa on muutamia esimerkkejä etsiminen tietojen luettelon kohteita.  

### <a name="discover-data-assets-with-basic-search"></a>Tutustu tietojen resurssien perushaun avulla
Perushaku auttaa etsiminen luettelon yhden tai useamman hakusanojen. Tulokset ovat kalusteet, joiden vastaa ominaisuuksista yhden tai useamman: n ehtojen kanssa.

1. Valitse **Aloitus** , jos Azure tietoluettelon-portaalissa. Jos olet sulkenut web-selaimessa, siirry [Azure tietoluettelon aloitussivulle](https://www.azuredatacatalog.com).
2. Kirjoita hakuruutuun `cycles` ja paina **ENTER**-näppäintä.

    ![Azure tietoluettelon--Perusteksti haku](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Varmista, että näet kaikki neljä taulukot ja tulokset tietokannan (AdventureWorks2014). Voit vaihtaa **ruudukkonäkymässä** ja **luettelo-näkymä** napsauttamalla työkalurivin painikkeet seuraavassa kuvassa esitetyllä tavalla. Huomaa, että hakutuloksissa näkyy korostettuna haku hakusanalla koska ** **Korosta** -vaihtoehto on**. Voit myös määrittää **tulosten sivulla** määrä hakutuloksissa.

    ![Azure tietoluettelon--Perusteksti etsinnän tulokset](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    **Haku** -paneeli on vasemmalla ja **Ominaisuudet** -ruutu on oikealla. Valitse **haku** -ruudun voit muuttaa hakuehtoja ja suodattaa tulokset. **Ominaisuudet** -ruutu näyttää valitun objektin ominaisuudet ruudukkoon tai luetteloon.

4. Valitse **tuote** hakutuloksista. Valitse **Esikatselu**, **sarakkeet**, **Profiilin tiedot**ja **asiakirjat** välilehdet tai laajenna alaruudun nuolta.  

    ![Azure tietoluettelon--alaruudun](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    **Esikatselu** -välilehdessä näet esikatselun tiedoista **Product** -taulukkoon.  
5. Valitse Etsi tietoja sarakkeiden (kuten **nimi** ja **tietotyyppi**) tietojen kohteen **sarakkeet** -välilehti.
6. Voit tarkastella tietoja profilointi **Profiilin tiedot** -välilehti (esimerkiksi: määrä rivejä, koon tai sarakkeen pienin arvo)-tietojen kohteen.
7. Voit suodattaa tulokset käyttämällä **suodattimia** vasemmalla. Valitse esimerkiksi **taulukon** **Objektilaji**ja näet vain neljä taulukoiden, ei tietokantaa.

    ![Azure tietoluettelon--Suodata hakutulokset](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Tutustu tietojen resurssien ominaisuuden vaikutusalueen määrittäminen
Ominaisuuden vaikutusalueen määrittäminen auttaa löytämään tietoja varat johon hakusanoja verrataan määritetty ominaisuus.

1. Valitse **Objektityyppi** **suodattimissa**suodattimen **taulukon** .  
2. Kirjoita hakuruutuun `tags:cycles` ja paina **ENTER**-näppäintä. Katso [Tietoluettelohaku syntaksi](https://msdn.microsoft.com/library/azure/mt267594.aspx) kaikki ominaisuudet, joita voit käyttää tietojen etsimistä varten.
3. Varmista, että näet kaikki neljä taulukot ja tulokset tietokannan (AdventureWorks2014).  

    ![Tietoluettelo--ominaisuuden rajauksen hakutulokset](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Haun tallentaminen
1. **Nykyinen haku** -osassa **haut** -ruudussa Etsi nimi ja valitse **Tallenna**.

    ![Azure tietoluettelon--Tallenna haku](media/data-catalog-get-started/data-catalog-save-search.png)
2. Varmista kohdassa **Tallennettujen hakujen**näkyy tallennetun haun.

    ![Azure tietoluettelon--tallennetut haut](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Valitse jokin tallennetun haun (**nimeäminen uudelleen**, **Poista**, **Tallenna oletukseksi** haku)-toimia.

    ![Azure tietoluettelon--tallennettu hakuasetukset](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Totuusarvo-operaattorit
Voit laajentaa tai tarkentaa hakua Totuusarvo-operaattorit kanssa.

1. Kirjoita hakuruutuun `tags:cycles AND objectType:table`, ja paina **ENTER**-näppäintä.
2. Varmista, että näet vain taulukoista (ei tietokanta) tuloksissa.  

    ![Azure tietoluettelon--hakutoiminnossa totuusarvo-operaattori](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Sulkeissa ryhmittely
Ryhmittelemällä sulkeissa voit ryhmitellä kyselyn saavuttamiseksi looginen eristystaso, erityisesti Totuusarvo-operaattorit ja osat.

1. Kirjoita hakuruutuun `name:product AND (tags:cycles AND objectType:table)` ja paina **ENTER**-näppäintä.
2. Varmista, että näet vain hakutulosten **Product** -taulukkoon.

    ![Azure tietoluettelon--ryhmittelyn haku](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Vertailuoperaattorit
Vertailuoperaattorit, jossa voit käyttää vertailuja kuin tasa ominaisuuksia, joilla on numeerista tai päivämäärä-tietotyypit.

1. Kirjoita hakuruutuun `lastRegisteredTime:>"06/09/2016"`.
2. Valitse **Objektityyppi**- **taulukon** suodattimen.
3. Paina **ENTER**-näppäintä.
4. Varmista, että näet **tuotteen**, **ProductCategory**, **ProductDescription**ja **ProductPhoto** taulukot ja hakutuloksissa rekisteröity AdventureWorks2014 tietokannan.

    ![Azure tietoluettelon--vertailu etsinnän tulokset](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Katso, [miten saat tietää, tietojen varat](data-catalog-how-to-discover.md) yksityiskohtaisia tietoja tietojen varat ja [Tietoluettelohaku syntaksi](https://msdn.microsoft.com/library/azure/mt267594.aspx) etsiminen haun syntaksin.

## <a name="annotate-data-assets"></a>Tietoja resurssien huomautuksia
Tämä Harjoitus Azure tietoluettelon portaalin avulla lisätä huomautuksia (Lisää tietoja, kuten kuvaukset, tunnisteet tai asiantuntijoiden) tietojen varat olet aikaisemmin rekisteröinyt luettelossa. Huomautukset täydentää ja parantaa poimittujen tietolähteen rekisteröinnin yhteydessä rakenteellisia metatiedot ja tietojen resurssien on helpompi löytää ja ymmärtää.

Tässä voit lisätä huomautuksia yhtä resurssi (ProductPhoto). Nimen ja kuvauksen lisääminen ProductPhoto tietojen kohteen.  

1.  Siirry [Azure tietoluettelon aloitussivu](https://www.azuredatacatalog.com) ja etsiä `tags:cycles` voit etsiä tietoja varat olet rekisteröinyt.  
2. Valitse **ProductPhoto** hakutuloksissa.  
3. Kirjoita **Kutsumanimi** ja **tuotteen valokuvia markkinoinnin materiaalien** **tuotteen kuvia** **kuvaus**.

    ![Azure tietoluettelon--ProductPhoto kuvaus](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    **Kuvaus** auttaa muiden löydät ja miksi ymmärtää ja Opi käyttämään valitun kohteen. Voit myös lisätä tunnisteita ja tarkastella. Nyt voit kokeilla hakemisesta ja suodattamisesta tuttuihin tietojen resurssien luetteloon lisäämäsi kuvaava metatietojen avulla.

Voit tehdä myös tällä sivulla seuraavasti:

- Lisää tiedot kohteen asiantuntijoiden. Valitse **Lisää** **asiantuntijoiden** -alueella.
- Tunnisteiden lisääminen tietojoukko tasolla. Valitse **Lisää** **tunnisteet** -alueella. Tunnisteen voi olla käyttäjän tunnisteen tai sanasto-tunniste. Standard Edition, tietoluettelon sisältää liiketoiminta-sanasto, joka auttaa luettelon järjestelmänvalvojien määrittää keskitetyn business luokituksen. Luettelon käyttäjät voi lisätä huomautuksia sanaston termeistä resurssien tiedot. Katso lisätietoja, [Voit määrittää määräytyvät tunnisteissa Business-sanasto](data-catalog-how-to-business-glossary.md)
- Tunnisteiden lisääminen sarakkeen tasolla. Valitse **tunnisteet** -kohdasta **Lisää** huomautuksia käytettävää saraketta varten.
- Lisää kuvaus sarakkeen tasolla. Kirjoita sarakkeen **kuvaus** . Voit myös tarkastella poimittujen tietolähteen kuvaus metatiedot.
- Lisää näyttää käyttäjille, miten tiedot kohteen käyttöoikeuden pyytäminen **käyttöoikeuksien pyytäminen** tiedot.

    ![Azure tietoluettelon--tunnisteiden, kuvausten lisääminen](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- Valitse **asiakirjat** -välilehti ja anna tiedot kohteen ohjeissa. Azure tietoluettelon asiakirjojen voit tietojen luettelon sisällön säilöön kuin valmis henkilöstöpäällikön tietojen kohteiden luominen.

    ![Azure tietoluettelon--asiakirjat-välilehti](media/data-catalog-get-started/data-catalog-documentation.png)


Voit lisätä huomautuksen myös useita tietojen resurssit. Esimerkiksi voit valita kaikki rekisteröidyt tietojen varat ja määritä niiden asiantuntija.

![Azure tietoluettelon--huomautuksia useita tietojen resurssit](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure tietoluettelon tukee joukosta hankinta hallintatavan huomautukset. Tietoluettelo kuka tahansa käyttäjä, voit lisätä tunnisteet (käyttäjä tai sanasto), kuvaukset ja muita metatietoja, niin, että kaikki käyttäjät, joilla tietojen resurssi ja sen käyttö perspektiivin voi olla, perspektiivi siepatun ja muiden käyttäjien käytettävissä.

Katso, [miten huomautuksia tietojen varat](data-catalog-how-to-annotate.md) yksityiskohtaisia tietoja tietojen varat lisätä huomautuksia.

## <a name="connect-to-data-assets"></a>Tietoja resurssien yhdistäminen
Tässä avata integroitu-asiakasohjelma (Excel) ja -integroitu työkalu (SQL Server Management Studiossa) tietojen kohteita käyttämällä yhteystiedot.

> [AZURE.NOTE] On tärkeää muistaa, että Azure tietoluettelon ei avulla voit käyttää todellinen tietolähteen – se vain on helppo voit tutkia ja sen ymmärtämistä. Kun muodostat yhteyden tietolähteeseen, asiakassovellus, voit valita käytössä Windows-tunnistetiedot tai pyytää tunnistetietoja tarpeen mukaan. Jos sinulla ei ole aiemmin myönnetty access tietolähteeseen, sinun on myönnettävä käyttöoikeudet, ennen kuin voit muodostaa yhteyden.

### <a name="connect-to-a-data-asset-from-excel"></a>Yhteyden muodostaminen tietojen resurssi Excelistä

1. Valitse **tuote** hakutuloksista. Napsauta työkalurivin **Avaa-** ja sitten **Excel**.

    ![Azure tietoluettelon--resurssi tietojen yhdistäminen](media/data-catalog-get-started/data-catalog-connect1.png)
2. Valitse **Avaa** Lataa ponnahdusikkunan. Tämä todentamistyypistä riippuen selaimessa.

    ![Azure tietoluettelon--ladatut Excelin yhteystiedoston](media/data-catalog-get-started/data-catalog-download-open.png)
3. **Microsoft Excel-tietoturvailmoitus** -ikkunassa Valitse **Ota käyttöön**.

    ![Azure tietoluettelon--Excel Suojaus-valikko](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Oletusarvoiset koota **Tuontitiedot** -valintaikkunassa ja valitse **OK**.

    ![Azure tietoluettelon – Excel-tietojen tuominen](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Tarkastele tietoja Excelissä.

    ![Azure tietoluettelon--product-taulukkoon Excelissä](media/data-catalog-get-started/data-catalog-connect2.png)

Tässä yhteyden tietoja resurssien havaitsi Azure tietoluettelon avulla. Azure tietoluettelon-portaalissa voit muodostaa suoraan käyttämällä **Avaa-** valikossa integroitu asiakassovelluksiin. Voit myös muodostaa on kaikissa sovelluksissa, valitse resurssi-metatiedot sisältyvät yhteystiedot sijainti käyttämällä. Voit esimerkiksi rekisteröity tässä opetusohjelmassa tietojen varoihin tietojen AdventureWorks2014-tietokantayhteyden muodostamisessa SQL Server Management Studiossa.

1. Avaa **SQL Server Management Studiossa**.
2. Kirjoita **Muodosta yhteys palvelimeen** -valintaikkunassa Azure tietoluettelon portaalin **Ominaisuudet** -ruutuun palvelimen nimi.
3. Käytä asianmukaiset todennusta ja tietojen kohteen käyttöoikeudet. Jos sinulla ei ole access-käyttää tietoja hankkiminen **Pyydä käyttöoikeuksia** -kenttään.

    ![Azure tietoluettelon--käyttöoikeuksien pyytäminen](media/data-catalog-get-started/data-catalog-request-access.png)

Valitse **Näytä yhteyden merkkijonojen** tarkasteleminen ja kopioi Leikepöydälle sovelluksen käytettäviksi ADF.NET, ODBC ja OLEDB yhteyden merkkijonoja.

## <a name="manage-data-assets"></a>Tietoja resurssien hallinta
Tässä vaiheessa näet, miten tiedot resurssien suojauksen määrittäminen. Tietoluettelo ei antaa käyttäjille itse tietoihin. Tietolähteen omistaja voi hallita tietojen käyttöä.

Voit käyttää tietojen löydät tietolähteitä sekä tarkastella luettelon rekisteröity käyttömahdollisuus liittyvät metatiedot. Voi olla tilanteessa, jossa tietolähteisiin olisi on näkyvissä vain tietyille käyttäjille tai tietyn ryhmissä. Näissä tilanteissa tietoluettelon avulla voit omistajaksi kuluessa luettelon resurssien rekisteröidyt tiedot ja sitten ohjausobjektiin varat näkyvyyden omistat.

> [AZURE.NOTE] Kuvattu tämän Harjoitus hallintatoiminnot ovat käytettävissä vain Standard Edition, Azure tietoluettelon, ei vapaa Edition.
Azure-tietoluetteloon omistajaksi tietojen kalusto, muiden omistajien lisääminen tietojen varat ja tietojen kalusto-näkyvyyden asetus.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Ottaa tietojen omistajuutta ja rajoittaa näkyvyyttä

1. Siirry [Azure tietoluettelon aloitussivulle](https://www.azuredatacatalog.com). Kirjoita **Etsi** -tekstiruutuun `tags:cycles` ja paina **ENTER**-näppäintä.
2. Tulosluettelossa kohde ja valitse **Ota omistajuus** työkalurivillä.
3. Valitse **Ominaisuudet** -ruutu **hallinta** -osassa **Ota omistajuus**.

    ![Azure tietoluettelon – Ota omistajuus](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Rajoittaa näkyvyyttä, valitse **näkyvyys** -kohdassa **omistajat ja nämä käyttäjät** ja sitten **Lisää**. Kirjoita käyttäjien sähköpostiosoitteet tekstiruutuun ja paina **ENTER**-näppäintä.

    ![Azure tietoluettelon--käytön rajoittaminen](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Tietoja resurssien poistaminen

Tässä käyttää Azure tietoluettelon portaalin tietojen esikatselu poistaminen rekisteröidyt tiedot varat ja poistaa tietoja resurssien luettelosta.

Azure-tietoluetteloon voit poistaa yksittäisten kohteiden tai poistaa useita kohteita.

1. Siirry [Azure tietoluettelon aloitussivulle](https://www.azuredatacatalog.com).
2. Kirjoita **Etsi** -tekstiruutuun `tags:cycles` ja sitten **ENTER-näppäintä**.
3. Valitse tulosluettelossa kohde ja valitse **Poista työkalurivin seuraavassa kuvassa esitetyllä tavalla:**

    ![Azure tietoluettelon--Poista ruudukon kohde](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    Jos käytössäsi on Luettelonäkymä-valintaruutu on vasemmalla puolella olevan kohteen seuraavassa kuvassa esitetyllä tavalla:

    ![Azure tietoluettelon--Poista luettelokohde](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    Voit myös valita useita tietojen kalusto ja poista ne seuraavassa kuvassa esitetyllä tavalla:

    ![Azure tietoluettelon--useita tietoja resurssien poistaminen](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Luettelon oletusasetuksista on että kuka tahansa käyttäjä, minkä tahansa tietolähteen rekisteröimiseen ja että kuka tahansa käyttäjä, voit poistaa kaikki tiedot resurssi, joka on rekisteröity. Sisältyvät Standard Edition, Azure tietoluettelon hallintatoiminnot tarjoavat lisävaihtoehtoja omistajuuden varat rajoittaminen kuka löytävät varat ja rajoittaminen, joka poistaa kohteita.


## <a name="summary"></a>Yhteenveto

Tässä opetusohjelmassa tarkasteltavia Azure tietoluettelon, mukaan lukien rekisteröiminen, lisäät huomautuksia, tutustu ja yrityksen tietojen resurssien hallinta olennaiset ominaisuuksia. Nyt kun olet suorittanut opetusohjelman, on aika avulla pääset alkuun. Voit aloittaa tänään ryhmäsi riippuvaisia tietolähteiden rekisteröintiä ja kutsu työtovereita käyttämään luettelon.

## <a name="references"></a>Viittaukset

- [Tietoja resurssien rekisteröimään](data-catalog-how-to-register.md)
- [Miten saat tietää, tietojen resurssit](data-catalog-how-to-discover.md)
- [Voit katsella tietojen kalusto](data-catalog-how-to-annotate.md)
- [Miten asiakirjan tiedot-resurssit](data-catalog-how-to-documentation.md)
- [Tietoja resurssien yhdistämisestä](data-catalog-how-to-connect.md)
- [Tietoja resurssien hallinta](data-catalog-how-to-manage.md)
