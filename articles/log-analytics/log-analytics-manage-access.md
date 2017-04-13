<properties
    pageTitle="Hallita niiden käyttöä Log Analytics | Microsoft Azure"
    description="Lokitiedoston Analytics erilaisilla hallintatehtäviä käyttäjille, tilit, OMS työtilojen ja Azure tilien käytön hallinta."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>Lokitiedoston Analytics käytön hallinta

Voit hallita niiden käyttöä Log Analytics-käytät hallintatehtäviä eri käyttäjille, tilit, OMS työtilojen ja Azure tilit. Jos haluat luoda uuden työtilan toimintojen hallinta Suite (OMS), valitse työtila, nimi Liitä-tilisi ja maantieteellisen sijainnin valitseminen. Työtila on käytettävä säilö, joka sisältää tilitiedot ja yksinkertainen kokoonpanotietoja tilin. Sinä tai muut organisaatiosi jäsenet voi käyttää useita OMS työtilat hallintaan eri tietojoukkojen, kerätään kaikkien tai IT-infrastruktuurin osiin.

[Lokitiedoston Analytics käytön aloittaminen](log-analytics-get-started.md) -artikkelissa kerrotaan, miten nopeasti ja käynnissä ja muiden tässä artikkelissa kuvataan tarkemmin joitain toimintoja, sinun on OMS käytön hallinta.

Vaikka voit joutua ei ensimmäinen hallinta jokaisen tehtävän suorittamiseen, käsittelemme kaikki tavallisimmat tehtävät, jotka voit käyttää seuraavissa kohdissa:

- Tarvitset työtilat määrän selvittäminen
- Tilien ja käyttäjien hallinta
- Ryhmän lisääminen aiemmin luotuun työtilaan
- Aiemmin luodun työtilan linkki Azure-tilaukseen
- Työtilan ksi maksullisia tietoliikennesopimus
- Suunnitelman tietotyypin vaihtaminen
- Azure Active Directory-organisaation lisääminen aiemmin luotuun työtilaan
- Sulje OMS-työtila

## <a name="determine-the-number-of-workspaces-you-need"></a>Tarvitset työtilat määrän selvittäminen

Työtilan on Azure resurssi ja säilön, johon kerätään, koostetaan, analysoida, ja esittää OMS-portaalissa.

Ei useita OMS Log Analytics-työtiloja kannattaa luoda ja että käyttäjät voivat käyttää yhden tai usean työtilan. Yleensä haluat pienentää työtilojen määrä, kun tämä sallii kysely ja yhdistää usean eniten tietoja. Tässä osassa kuvataan, kun se voi olla hyötyä useita työtilan luominen.

Nykyään Log Analytics-työtilassa on:

- Maantieteellisen sijainnin tietojen tallentamista varten
- Lisätietoja laskutuksesta rakeisuuden
- Tietoja eristystaso

Edellä mainittujen ominaisuuksien perusteella, haluat ehkä luoda useita työtiloja, jos:

- Olet yleinen yrityksen ja tarvitset tiettyjen alueiden tietoja paikallisen tietosuojan tai yhteensopivuuden syistä tallennettuja tietoja.
- Käytössäsi on Azure ja haluat välttää lähtevän siirron tiedonsiirtomaksuihin ottaa lokiin Analytics työtilan samalla alueella hallitsee Azure resursseina.
- Haluat kohdistaa kulut eri osastojen tai niiden käyttö business ryhmiin. Kun luot kunkin osaston tai business ryhmän työtilan, Azure laskun ja käyttö-lause esittää kunkin työtilan kulut erikseen.
- Olet hallitun palveluntarjoaja ja on lokin analytics-tiedot pysyvät kunkin asiakkaan hallitset eristetty muiden asiakastiedot.
- Useiden asiakkaiden hallinta ja kunkin asiakkaan tai osaston tai liiketoiminnan Katso omia tietoja, mutta ei tietoja muiden asiakkaiden tai osastojen tai business ryhmät.

Kun käytät tietojen keräämiseen agenttien vuoksi, voit määrittää kunkin agentti ilmoittamaan tarvittavat työtilaan.

Jos käytössäsi on System Center Operations manager, kukin Operations Manager hallinta-ryhmä voidaan yhdistää vain yhden työtila. Voit asentaa Microsoft Agent seuranta Operations Manager hallitsee tietokoneissa ja Operations Manager ja eri Log Analytics-työtilan agent-raportti.

### <a name="workspace-information"></a>Työtilan tiedot

Voit tarkastella työtilan tiedot ja valitse, haluatko tietoja Microsoftilta OMS-portaalissa.

#### <a name="view-workspace-information"></a>Työtilan tietojen tarkasteleminen

1. Valitse OMS- **asetukset** -ruutu.
2. Valitse **Asiakkaat** -välilehti.
3. Valitse **Työtilan tiedot** -välilehti.  
  ![Työtilan tiedot](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Tilien ja käyttäjien hallinta

Kunkin työtilan voi olla useita käyttäjätilejä liittyy ja jokaiselle käyttäjätilille (Microsoft-tili tai organisaation tili) voit käyttää useita OMS työtilat.

Oletusarvon mukaan Microsoft-tili tai organisaation tili, jota käytetään työtilan luominen tulee työtilan järjestelmänvalvoja. Järjestelmänvalvoja voi sitten kutsu Lisää Microsoft-sähköpostitilejä tai valitse käyttäjien Azure Active Directorysta.

Anna käyttäjien access OMS työtilaan ohjaa 2 paikassa:

- Azure-tietokannassa voit käyttää Azure tilaus ja niihin liittyvät Azure resurssit Roolipohjainen käyttöoikeuksien valvonta. Tätä käytetään myös PowerShell- ja REST API käytön.
- OMS-portaalin käytön vain OMS portal - ei ole liitetty Azure tilaus.

Jos myönnät käyttäjille oikeuksia OMS-portaaliin, mutta ei Azure tilaukseen, joka on linkitetty, sitten automaatio, varmuuskopiointi ja palauttaminen-ratkaisun ruudut eivät näy mitään tietoja käyttäjille kun ne Kirjautumisvirheitä aiheuttavien OMS-portaalissa.

Kaikki käyttäjät voivat nähdä tiedot näitä ratkaisuja, että heillä on oltava vähintään **reader** käyttää automaatio-tili, varmuuskopiointi säilö ja palauttaminen säilö, joka on linkitetty OMS työtilaan.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Lokitiedoston Analytics Azure-portaalissa käyttöoikeuksien hallinta

Jos myönnät käyttäjille oikeuksia Azure käyttöoikeuksilla Log Analytics-työtila, Azure-portaalissa esimerkiksi sitten saman käyttäjät voivat käyttää lokiin Analytics-portaalissa. Jos käyttäjät ovat Azure-portaalissa, ne Siirry OMS-portaali valitsemalla **OMS Portal** -tehtävä, kun tarkastelemalla Log Analytics työtilan.

Joitakin muistettavia seikkoja Azure portaalin seikkoja:

- Tämä ei ole *Roolipohjainen käyttöoikeuksien valvonta*. Jos sinulla on *lukija* -käyttöoikeudet Log Analytics työtilan Azure-portaalissa, voit tehdä muutoksia OMS-portaalissa. OMS-portaalissa on käsite järjestelmänvalvoja, avustaja ja vain luku-käyttäjä. Jos olet kirjautunut sisään tili on linkitetty työtilaan Azure Active Directoryn OMS-portaalissa järjestelmänvalvoja voi, muuten voi osallistujan.

- Kun kirjaudut sisään käyttämällä http://mms.microsoft.com, valitse oletusarvoisesti OMS-portaaliin näet **Valitse työtila** -luettelosta. Se sisältää vain työtilat, jotka on lisätty käyttämällä OMS-portaalissa. Voit tarkastella työtilat käytössäsi on Azure-tilausta ja valitse Määritä palvelutili osana URL-osoite. Esimerkki:

  `mms.microsoft.com/?tenant=contoso.com`Vuokraajan tunniste on usein sähköpostiosoite, johon voit kirjautua sisään viimeinen osa.

- Jos voit kirjautua sisään tilille on tilin vuokraajan Azure Active Directoryn, joka on yleensä, ellet ole kirjautua sisään kuin CSP sinun tulee OMS-portaalissa *järjestelmänvalvoja* . Jos tiliä ei ole vuokraajan Azure Active Directory, sinun on *käyttäjän* OMS-portaalissa.

- Jos haluat siirtyä suoraan portaalin, että sinulla on pääsy Azure käyttöoikeuksilla, voit joutua määrittämään resurssin osana URL-osoite. Se on mahdollista saada tätä URL-Osoitetta PowerShellin avulla.

  Esimerkiksi `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  URL-osoite näyttää:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>Käyttäjien OMS-portaalin hallinta

Voit hallita käyttäjiä ja **Käyttäjien hallinta** -välilehden asetukset-sivulta **Asiakkaat** -välilehdessä ryhmä. Ja voi suorittaa tehtäviä seuraavissa osissa.  

![käyttäjien hallinta](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Käyttäjän lisääminen aiemmin luotuun työtilaan

Seuraavien vaiheiden avulla voit lisätä käyttäjän tai ryhmän OMS työtilaan. Käyttäjä tai ryhmä saa oikeuden tarkastella ja käsitellä kaikki ilmoitukset, jotka liittyvät tämän työtilan.

>[AZURE.NOTE] Haluat käyttäjän tai ryhmän lisääminen Azure Active Directory-organisaatiotili, varmista, että olet liittänyt OMS-tilisi Active Directory-toimialueen kanssa. Katso [Lisää Azure Active Directory organisaation aiemmin luotuun työtilaan](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. Valitse OMS- **asetukset** -ruutu.
2. Valitse **Asiakkaat** -välilehti ja valitse sitten **Käyttäjien hallinta** -välilehti.
3. Valitse **Käyttäjien hallinta** -osassa voit lisätä tilin tyyppi: **Organisaatiotili**, **Microsoft-tiliä**, **Microsoft-tuen**.
    - Jos valitset Microsoft Account, kirjoita Microsoft-Account liittyvän käyttäjän sähköpostiosoite.
    - Jos valitset organisaation tiliä, voit määrittää käyttäjän tai ryhmän nimi tai sähköpostitunnus osan ja käyttäjien ja ryhmien luettelo tulee näkyviin. Käyttäjän tai ryhmän valitseminen
    - Microsoft Support avulla voit antaa Microsoft Support selvittänyt tilapäisen pääsyn työtilaa, jotka auttavat vianmäärityksessä.

    >[AZURE.NOTE] Saat parhaan suorituskyvyn, Active Directory-ryhmä, jotka on liitetty kolmeen OMS tilistä määrän rajoittaminen – yksi Järjestelmänvalvojat, yksi osallistujat ja toinen vain luku-käyttäjiä varten. Lisää ryhmien käyttäminen voi vaikuttaa myös lokin Analytics suorituskykyä.

5. Valitse haluamasi käyttäjän tai ryhmän lisääminen: **järjestelmänvalvoja**, **osallistuja**tai **Vain luku-tilassa** .  
6. Valitse **Lisää**.

  Jos olet lisäämässä Microsoft-tiliä, kutsu liittyä työtila lähetetään sovellukseen sähköpostiviesti. Sen jälkeen, kun käyttäjä seuraa ohjeita kutsussa liity OMS, käyttäjä voi tarkastella ilmoituksia ja OMS tilin tilitiedot ja osaat Näytä **asetukset** -sivun **Asiakkaat** -välilehdessä käyttäjätiedot.
  Jos olet lisäämässä organisaation tilille, käyttäjä voi käyttää lokiin Analytics heti.  
  ![Kutsu sähköposti](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Muokkaa aiemmin käyttäjätyyppi

Voit muuttaa OMS-tilisi käyttäjän tiliä-roolin. Sinulla on roolin seuraavista vaihtoehdoista:

 - *Järjestelmänvalvoja*: Voit käyttäjien hallinta, tarkastella ja käsitellä kaikki ilmoitukset ja lisätä ja poistaa palvelimet

 - *Avustaja*: Näytä ja suorittaa kaikki ilmoitukset ja lisätä ja poistaa palvelimet

 - *Vain luku-käyttäjä*: vain luku-tilassa käyttäjät eivät voi:
   1. Lisää tai poista ratkaisuja. Ratkaisuvalikoimaan piilotetaan.
   2. Lisää tai muokkaa tai poista ruutujen **Oma Raporttinäkymät-ikkunan**.
   3. Näytä **asetukset** -sivuilla. Sivut on piilotettu.
   4. Etsi näkymän, PowerBI määritykset, tallennettujen hakujen ja ilmoitusten tehtävät piilotetaan.


#### <a name="to-edit-an-account"></a>Voit muokata tiliä

1. Valitse OMS- **asetukset** -ruutu.
2. Valitse **Asiakkaat** -välilehti ja valitse sitten **Käyttäjien hallinta** -välilehti.
3. Valitse rooli, käyttäjälle, jonka haluat muuttaa.
2. Valitse vahvistusikkunassa Valitse **Kyllä**.

### <a name="remove-a-user-from-a-oms-workspace"></a>Käyttäjän poistaminen OMS-työtila

Seuraavien vaiheiden avulla voit poistaa käyttäjän OMS-työtilassa. Huomaa, että tämä Sulje käyttäjän työtila. Sen sijaan poistaa kyseisen käyttäjän ja työtilan välisen suhteen. Jos käyttäjä on liitetty useita työtilat, kyseisen käyttäjän edelleenkään käyttää OMS kirjautuminen ja nähdä muiden työtilojen.

1. Valitse OMS- **asetukset** -ruutu.
2. Valitse **Asiakkaat** -välilehti ja valitse sitten **Käyttäjien hallinta** -välilehti.
3. Valitse **Poista** , jonka haluat poistaa käyttäjänimen vieressä.
4. Valitse vahvistusikkunassa Valitse **Kyllä**.


### <a name="add-a-group-to-an-existing-workspace"></a>Ryhmän lisääminen aiemmin luotuun työtilaan

1.  Suorita vaiheet 1 -4-", käyttäjän lisääminen aiemmin luotuun työtilaan", yllä.
2.  **Valitse käyttäjän tai ryhmän**Valitse **ryhmä**.
    ![ryhmän lisääminen aiemmin luotuun työtilaan](./media/log-analytics-manage-access/add-group.png)
3.  Kirjoita näyttönimi tai sähköpostiosoite, jotka haluat lisätä ryhmän.
4.  Valitse ryhmä-luettelosta tulokset ja valitse sitten **Lisää**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Aiemmin luodun työtilan linkki Azure-tilaukseen

Ei luoda työtilan [microsoft.com/oms](https://microsoft.com/oms) -sivustosta.  Kuitenkin tiettyjä rajoituksia olemassa nämä työtilat eniten huomattavat parhaillaan 500 Megatavua/päivän tietojen lataukset rajoitukset, jos käytössäsi on ilmainen tili. Voit tehdä muutoksia tämän työtilan tarvitset *Linkki aiemmin työtilan Azure-tilaukseen*.

>[AZURE.IMPORTANT] Linkin työtilan, jotta Azure-tili on jo access haluat linkittää työtilaan.  Toisin sanoen avulla voit käyttää Azure portaalin tilin on oltava **sama** OMS työtila-tiliä. Jos tämä ei ole kohdassa [Lisää käyttäjä aiemmin luotuun työtilaan](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Voit linkittää työtilan Azure tilaukseen OMS-portaalissa

Linkin työtilan Azure tilauksen OMS-portaalissa, jotta kirjautunut sisään käyttäjällä on oltava jo maksetun Azure-tili. Työtila, jota käytät aktiivisesti saa linkitetty Azure-tili.

1. Valitse OMS- **asetukset** -ruutu.
2. Valitse **Asiakkaat** -välilehti ja valitse sitten **Azure tilaus ja tietojen suunnittelu** -välilehti.
3. Valitse tiedot-palvelupaketti, jota haluat käyttää.
4. Valitse **Tallenna**.  
  ![tilauksen ja tietoja-Palvelupaketit](./media/log-analytics-manage-access/subscription-tab.png)

Palvelupakettisi tiedot näytetään OMS portaalin valintanauhan web-sivun yläreunassa.

![OMS valintanauha](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Voit linkittää työtilan Azure tilaukseen Azure-portaalissa

1.  Kirjaudu sisään [Azure portal](http://portal.azure.com).
2.  Selaa **Log Analytics (OMS)** ja valitse se.
3.  Näet luettelon aiemmin työtilojen. Valitse **Lisää**.  
    ![työtilojen luettelo](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  Valitse **OMS työtilan** **tai linkittää olemassa**.  
    ![linkki aiemmin](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Valitse **Määritä tarvittavat asetukset**.  
    ![Määritä tarvittavat asetukset](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Näet työtiloja, joita ei ole vielä linkitetty Azure-tili. Valitse työtila.  
    ![Valitse työtilat](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Tarvittaessa voit muuttaa seuraavat arvot:
    - Tilauksen
    - Resurssiryhmä
    - Sijainti
    - Hinnat taso  
        ![arvojen muuttaminen](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Valitse **Luo**. Työtila on nyt linkitetty Azure-tili.

>[AZURE.NOTE] Jos haluat linkittää työtilassa ei ole näkyvissä, valitse Azure-tilauksesi ei ole access OMS työtilaan, jonka loit OMS-sivuston avulla.  Sinun on käyttöoikeus tähän tiliin sisällä OMS työtilan OMS-sivuston käyttäminen. Tee kohdassa [Lisää käyttäjä aiemmin luotuun työtilaan](#add-a-user-to-an-existing-workspace).



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Työtilan ksi maksullisia tietoliikennesopimus

On kolme työtilatietojen suunnitteleminen tyypit OMS: **vapaa**, **Vakio**ja **Premium**.  Jos olet *vapaa* -palvelupaketti, voit ehkä osumien tiedot-pää 500 megatavua.  Haluat päivittää työtilan ***tukiyhteyksiä suunnitelman*** lisäksi rajoitusta tietojen keräämistä varten. Milloin tahansa voit muuntaa suunnitelman tyyppi.  Saat lisätietoja OMS hinnat [Hinnat tiedot](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Työtilan suunnitelmien voi muuttaa vain, jos ne ovat *linkitetyn* Azure-tilaukseen.  Jos olet luonut työtilan Azure tai jos olet *jo* linkitetty työtilan, voit ohittaa tämän sanoman.  Jos olet luonut työtilan [OMS sivuston](http://www.microsoft.com/oms), sinun on noudattamalla [Linkki aiemmin luodun työtilan Azure-tilaukseen](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>Oikeudet-OMS-lisäosan käytön System Center

System Center OMS-lisäosa on oikeuden OMS Log Analytics-ohjeiden etsiminen [OMS hinnat](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx)Premium-palvelupaketin.

Kun ostat System Center OMS lisäosa, OMS-lisäosa on lisätty System Center sopimuksen oikeuden. Azure tilauksen, joka on luotu tässä sopimuksessa tehdä oikeus käyttää. Näin voit esimerkiksi on useita OMS-työtilat, jotka käyttävät OMS-lisäosan-oikeus.

Voit varmistaa, että OMS työtilan käyttö otetaan käyttöön oman oikeudet-OMS-lisäosa, sinun on:

1. Linkki Azure-tilaukseen, joka on osa, joka sisältää OMS apuohjelman hankkiminen ja Azure tilauksen käyttö Enterprise Agreement-sopimus OMS-työtila
2. Valitse työtilan Premium-suunnitelma

Kun olet tarkistanut käyttö Azure tai OMS-portaalissa, et näe OMS lisäosa oikeuksista. Voit kuitenkin nähdä yritysportaalin oikeudet.  

Jos haluat muuttaa OMS työtilan on linkitetty Azure tilaus, voit käyttää PowerShellin Azure [Siirrä AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) cmdlet-komento.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Enterprise Agreement-sopimus Azure säännöllistä käyttäminen

Jos haluat käyttää erillisen hinnat OMS-komponentteja, maksat OMS jokaisen osan erikseen ja Azure laskun näkyy käyttö.

Jos sinulla Azure raha Vahvista yrityksen rekisteröinti, johon Azure tilauksistasi on linkitetty, minkä tahansa Log Analytics-käyttö automaattisesti veloittaa vastaan kaikki jäljellä olevat raha Vahvista.

Jos haluat muuttaa Azure tilauksen OMS työtilan henkilöä voit käyttää PowerShellin Azure [Siirrä AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) cmdlet-komento.  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Jos haluat muuttaa työtilan maksettu tietoliikennesopimus

1.  Kirjaudu sisään [Azure portal](http://portal.azure.com).
2.  Selaa **Log Analytics (OMS)** ja valitse se.
3.  Näet luettelon aiemmin työtilojen. Valitse työtila.  
    ![työtilojen luettelo](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  Valitse **asetukset**-kohdassa **hinnoittelu taso**.  
    ![hinnat taso](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  Valitse **hinnoittelu taso**Valitse tiedot-suunnitelma ja valitse sitten **Valitse**.  
    ![Valitse suunnitelma](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Kun päivität näkymän Azure-portaalissa, näet päivittää valitun suunnitelman **hinnoittelu taso** .  
    ![Päivitä hinnoittelua taso](./media/log-analytics-manage-access/manage-access-change-plan04.png)

Nyt voit kerätä tietoja tietojen "vapaa" pää lisäksi.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Azure Active Directory-organisaation lisääminen aiemmin luotuun työtilaan

Voit yhdistää Log Analytics (OMS) työtilan Azure Active Directory-toimialueen. Näin voit lisätä käyttäjiä Active Directorysta suoraan OMS työtilan tarvitsematta eri Microsoft-tili.

Kun työtilan luominen Azure-portaalista tai työtilan linkki Azure-tilaukseen Azure Active Directory linkitetään organisaation tiliä.

Kun luot työtilan OMS-portaalista, sinua pyydetään linkki Azure tilauksen ja käytössä on organisaatiotili.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Azure Active Directory-organisaation lisääminen aiemmin luotuun työtilaan

1. Valitse asetukset-sivun OMS **tilit** ja valitse sitten **Työtilan tiedot** -välilehti.  
2. Tarkastella sen tietoja organisaation tili ja valitse sitten **Lisää organisaation**.  
    ![Lisää organisaation](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Kirjoita järjestelmänvalvojan tunnistetietoja Azure Active Directory-toimialueen. Jälkeenpäin näet kuittauksen siitä, että työtilan on liitetty Azure Active Directory-toimialueen.
    ![linkitetyn työtilan acknowledgment](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Kun tili on linkitetty organisaation tilille, linkittäminen voi poistaa tai muuttaa.

## <a name="close-your-oms-workspace"></a>Sulje OMS-työtila

Kun suljet OMS-työtila, kaikki tiedot, jotka liittyvät työtilan poistetaan OMS-palvelun sulkeminen työtilan 30 päivän kuluessa.

Jos olet järjestelmänvalvoja ja on useita käyttäjiä, jotka liittyvät työtilan, nämä käyttäjät ja työtilan välisen suhteen on katkennut. Jos käyttäjät eivät liity muiden työtilojen, valitse he voivat edelleen OMS käyttäminen muiden työtilojen. Jos niitä ei liity muiden työtilojen sitten tiedot on käytettävä OMS uuden työtilan luominen.

### <a name="to-close-an-oms-workspace"></a>Sulje OMS-työtila

1. Valitse OMS- **asetukset** -ruutu.
2. Valitse **Asiakkaat** -välilehti ja valitse sitten **Työtilan tiedot** -välilehti.
3. Valitse **Sulje työtila**.
4. Valitse jokin seuraavista työtilan sulkemisen syistä tai kirjoita eri syystä tekstiruutuun.
5. Valitse **Sulje työtila**.

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso [yhteyden Windows-tietokoneiden lokin Analytics](log-analytics-windows-agents.md) tekijöiden ja kerätä tietoja.
- [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md) Lisää toimintoja ja kerätä tietoja.
- Jos organisaatiosi käyttää palomuurin tai välityspalvelimen niin, että agenttien vuoksi viestiä Log Analytics-palvelussa [Määritä välityspalvelin ja palomuurin lokin Analytics asetukset](log-analytics-proxy-firewall.md) .
