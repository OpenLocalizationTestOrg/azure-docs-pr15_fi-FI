<properties 
    pageTitle="Seurata ja hallita Azure Data Factory putkistot" 
    description="Lue, miten voit seurata ja hallita Azure tietojen tehtaan ja putkistot seuranta ja hallinta-sovelluksen avulla." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>Seurata ja hallita Azure Data Factory putkistot uusi seuranta ja hallinta-sovelluksen avulla
> [AZURE.SELECTOR]
- [Azure Portal/Azure PowerShellin avulla](data-factory-monitor-manage-pipelines.md)
- [Käyttämällä seuranta ja hallinta-sovellus](data-factory-monitor-manage-app.md)

Tässä artikkelissa käsitellään valvoa, hallita ja virheenkorjaus oman putkistot ja luo ilmoituksia haluat saada ilmoituksen **Seuranta ja hallinta-sovelluksen**virheet. Voit myös katsoa seuraavassa videossa seuranta ja hallinta-sovelluksen käyttämisestä.
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>Avaamista seuranta ja hallinta-sovelluksen
Käynnistää valvonnan ja hallinnan sovellus, valitsemalla **Seuranta ja hallinta** **Tietojen FACTORY** -sivu, tietojen factory ruutu.

![Data Factory kotisivu-ruutu seuranta](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

Raportissa pitäisi näkyä seuranta ja hallinta-sovelluksen käynnistää erillisessä välilehti/ikkunassa.  

![Valvonnan ja hallinnan sovellus](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] Jos huomaat, että selain pysähtyy "Sallimisesta...", **Estä kolmannen osapuolen evästeet ja sivuston tiedot** -asetus käytöstä ja poista valinta (tai) säilyttää sen käytössä ja luoda poikkeuksen **login.microsoftonline.com** ja yritä sitten sovellus käynnistetään uudelleen.


Jos tehtävän windows alareunassa luettelossa ei ole näkyvissä, valitse Päivitä luettelo valitsemalla työkaluriviltä **Päivitä** -painiketta. Määritä lisäksi **alkamisaika** ja **Päättymisaika** -suodattimet oikealle.  


## <a name="understanding-the-monitoring-and-management-app"></a>Tietoja seuranta ja hallinta-sovellus
Vasemmalla on kolme välilehteä (**Resurssin Explorer**, **Seuranta näkymien**ja **ilmoitusten**) ja ensimmäisen (resurssin Explorer)-välilehti on valittuna oletusarvoisesti. 

### <a name="resource-explorer"></a>Resurssin Explorer
On seuraavissa artikkeleissa: 

- Resurssin Explorer **puunäkymän** vasemmanpuoleisessa ruudussa.
- **Verkkokaavio-näkymän** yläreunassa.
- Valitse keskimmäisen ruudun alareunassa **Toiminta Windows** luettelo.
- **Ominaisuuksien**/**Tehtävän ikkunan Explorerin** välilehdet oikeanpuoleisen ruudun. 

Resurssin Resurssienhallinnassa näet kaikki resurssit (putkistot, tietojoukkoja ja linkitetyt services) tietojen factory puunäkymässä. Kun valitset objektin resurssin Resurssienhallinnassa, huomaat seuraavasti: 

- liittyvien tietojen Factory kohde näkyy korostettuna kaavionäkymään.
- liittyvän tehtävän windows (napsauttamalla [tätä](data-factory-scheduling-and-execution.md) Saat lisätietoja toiminta windows) näkyvät korostettuina toiminta Windows-luettelon alaosassa.  
- Valitse oikeanpuoleisessa ruudussa ominaisuudet-ikkunassa valitun objektin ominaisuudet. 
- Valitun objektin tarvittaessa JSON määritys. Esimerkki: linkitetyn palvelun tai tietojoukko tai putkijohto. 

![Resurssin Explorer](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Katso [ajoitus ja suorittamisen](data-factory-scheduling-and-execution.md) artikkelissa yksityiskohtaisia käsitteellisiä tietoja toiminta-ikkuna. 

### <a name="diagram-view"></a>Kaavionäkymä
Tietoja factory kaavionäkymään tarjoaa yhden laidassa, kun haluat seurata ja hallita tietojen factory ja sen varat. Kun valitset Data Factory-kohteen (tietojoukko/myyntijakso) kaavionäkymään, huomaat seuraavasti:
 
- tietoja factory kohde valitaan puunäkymässä
- liitetyn tehtävän windows näkyvät korostettuina toiminta Windows-luettelosta.
- Ominaisuudet-ikkunassa valitun objektin ominaisuudet

Kun putkijohto on käytössä (eivät sisälly keskeytystilassa), se näkyy vihreällä viivalla. 

![Myyntijakso käynnissä](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Huomaa, että on kaavionäkymään myyntijakso kolme komentopainiketta. Voit pysäyttää putkijohto toista-painiketta. Keskeyttäminen ei lopeta parhaillaan käynnissä olevat toimet ja ilmoita jatkaa työn kesto päivinä. Kolmas painike keskeyttää putkijohto ja lopettaa sen nykyiset toiminnot suoritetaan. Ensimmäinen painike säilyy putkijohto. Kun oman myyntijakso on keskeytetty, huomaat putkijohto värien muuttamista vierekkäin seuraavasti.

![Pysäytä/jatka-ruutu](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

Voit Monivalinta kahden tai useamman putkistot (joko CTRL) ja käyttää komentopalkin painikkeita keskeyttäminen ja jatkaminen useita putkistot kerrallaan.

![Pysäytä/jatka-komentopalkki](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

Näet kaikki putkijohto, aktiviteetit myyntijakso-ruutua hiiren kakkospainikkeella ja valitsemalla **Avaa myyntijakso**.

![Myyntijakso-valikon avaaminen](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

Avattu myyntijakso-näkymässä näet putkijohto kaikki toiminnot. Tässä esimerkissä on vain yksi tehtävä: kopioi tehtävän. Palaa edelliseen näkymään napsauttamalla yläreunassa linkkipolun-valikosta tietoja factory nimeä.

![Avattu myyntijakso](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

Kun napsautat tulostus-tietojoukko tai kun siirrät hiiren päälle tulostus-tietojoukko myyntijakso-näkymässä on, että tietojoukko toiminta Windows-ponnahdusikkuna.

![Tehtävän Windows-valikko](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Voit valita tehtävä-ikkunassa näkevän tietoja se oikeanpuoleisen ruudun **Ominaisuudet** -ikkuna. 

![Tehtävän ikkunan ominaisuudet](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Siirry oikeanpuoleisen ruudun **Tehtävän ikkunan hallinta** -välilehti, niin saat näkyviin tarkempia.

![Tehtävän ikkunan Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

Näet myös **ratkennut muuttujat** kunkin tehtävän suorittaminen yrittää **yritykset** -osassa. 

![Ratkaistu muuttujat](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Siirtää valitun objektin JSON komentosarja-sanaa saat **komentosarja** -välilehteen.   

![Komentosarja-välilehti](./media/data-factory-monitor-manage-app/ScriptTab.png)

Näet toiminta windows kolmessa paikassa:

- Tehtävän Windows ponnahdusikkuna kaavionäkymässä (keskimmäisessä ruudussa).
- Tehtävän ikkunan Explorer oikeanpuoleisen ruudun.
- Tehtävän alaruudun Windows-luettelosta.

Tehtävän Windows ponnahdusikkunoiden ja tehtävän ikkunan Explorerissa voit vierittää edellisellä viikolla ja ensi viikolla, käyttämällä vasen ja oikea nuoli.

![Tehtävän ikkunan Explorer vasemmalle/oikealle nuolet](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Verkkokaavio-näkymän alaosassa näet Lähennä, Loitonna, Zoomaus Sovita Zoomaus 100 %, Lukitse asettelu-painikkeita. Lukitse asettelu-painike estää siirtäminen vahingossa kaavionäkymään taulukoita ja putkistot ja oletusarvo on käytössä. Voit poistaa sen käytöstä ja kohteiden siirtyminen kaavioon. Kun poistat sen käytöstä, voit sijoittaa automaattisesti taulukoiden ja putkistot viimeistä painiketta. Voit myös lähentää / Loitonna käyttäen kiekkopainiketta.

![Kaavio-näkymän Zoomaus-komennot](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>Tehtävän Windows-luettelo
Keskimmäisen ruudun alaosassa tehtävän windows-luettelo sisältää resurssin Resurssienhallinnassa tai kaavionäkymä valitun tietojoukon tehtävän ikkunoiden. Oletusarvon mukaan luettelo on laskevaan järjestykseen, mikä tarkoittaa, että näet uusimmat tehtävän ikkunan yläosassa. 

![Tehtävän Windows-luettelo](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Luetteloa ei päivitä automaattisesti, käytä niin Päivitä-painike työkalurivin voit päivittää sen manuaalisesti.  


Tehtävän windows voi olla jokin seuraavista tiloista:

<table>
<tr>
    <th align="left">Tila</th><th align="left">Alitila</th><th align="left">Kuvaus</th>
</tr>
<tr>
    <td rowspan="8">Odottaa</td><td>ScheduleTime</td><td>Suorita tehtävä-ikkunassa on tule aika.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Seuraavat riippuvuudet eivät ole valmis.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Laske-resurssit eivät ole käytettävissä.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Tehtävän esiintymät on varattu tehtävä muita käyttöjärjestelmässä.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Tehtävä on keskeytetty ja voi suorittaa tehtävän windows, kunnes se on sen käyttöä jatketaan.</td>
</tr>
<tr>
<td>Yritä uudelleen</td><td>Tehtävän suorittaminen yritetään.</td>
</tr>
<tr>
<td>Kelpoisuustarkistus</td><td>Kelpoisuustarkistus ei ole vielä aloitettu.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Odotetaan kelpoisuuden yritettävä.</td>
</tr>
<tr>
<tr
<td rowspan="2">Aktiivisuustila</td><td>Vahvistaminen</td><td>Vahvistus on käynnissä.</td>
</tr>
<td></td>
<td>Tehtävä-ikkunassa käsitellään.</td>
</tr>
<tr>
<td rowspan="4">Epäonnistui</td><td>Aikakatkaisu</td><td>Suorittamisen kulunut kauemmin kuin, joka on tehtävä.</td>
</tr>
<tr>
<td>Peruuttaa</td><td>Peruuttaa käyttäjältä-toiminto.</td>
</tr>
<tr>
<td>Kelpoisuustarkistus</td><td>Vahvistus epäonnistuu.</td>
</tr>
<tr>
<td></td><td>Luo ja/tai Vahvista toiminta-ikkuna epäonnistui.</td>
</tr>
<td>Valmis</td><td></td><td>Toiminta-ikkuna on valmis käytettäväksi.</td>
</tr>
<tr>
<td>Ohitetaan</td><td></td><td>Toiminta-ikkuna ei käsitellä.</td>
</tr>
<tr>
<td>Ei mitään</td><td></td><td>Toiminta-ikkuna, jossa käytetään eri tila liitettyinä, mutta on vaihdettu.</td>
</tr>
</table>


Kun napsautat tehtävä-ikkunan luettelossa, näet tietoja sen **Tehtävän Resurssienhallinnassa** tai **Ominaisuudet** -ikkunassa oikealla.

![Tehtävän ikkunan Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Tehtävän Windowsin päivittäminen  
Tietoja ei automaattisesti päivitetään, jotta voit käyttää **Päivitä** -painiketta (Toista-painike) komentopalkin päivityspainike toiminta windows-luettelosta.  
 

### <a name="properties-window"></a>Ominaisuudet-ikkuna
Ominaisuudet-ikkuna on seuranta ja hallinta-sovelluksen oikeanpuoleiset-ruudussa. 

![Ominaisuudet-ikkuna](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Se näyttää resurssin explorer (puunäkymä) (tai) kaavio (tai näkymässä) toiminta windows-luettelossa valitun kohteen ominaisuudet. 

### <a name="activity-window-explorer"></a>Tehtävän ikkunan Explorer

**Tehtävän ikkunan** ikkunan on seuranta ja hallinta-sovelluksen oikeanpuoleiset-ruudussa. Se näyttää tietoja toiminta Windows ponnahdusikkunoiden tai tehtävän Windows-luettelosta valittuna toiminta-ikkuna. 

![Tehtävän ikkunan Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

Voit liikkua toiseen toiminta-ikkuna napsauttamalla sitä kalenterinäkymän yläreunassa. Voit myös käyttää **vasenta**/**oikeaa** nuolipainiketta toiminta windows-Edellinen/seuraava viikko Nähdäksesi yläosassa.

Voit käyttää **uudelleen** toiminta-ikkuna tai **Päivitä** ala-ruudun painikkeet tiedot-ruudussa. 

### <a name="script"></a>Komentosarja 
**Komentosarja** -välilehden avulla voit tarkastella tietoja Factory valitun kohteen (linkitetyistä, tietojoukko ja myyntijakso) JSON-määritys. 

![Komentosarja-välilehti](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>Järjestelmänäkymien käyttäminen
Seuranta ja hallinta-sovellus on valmiita järjestelmänäkymiä (**Viimeisimmät toiminta windows**, **epäonnistui toiminta windows** **Edistymisen toiminta windows**), jonka avulla voit tarkastella tietoja factory windows viimeisimmät/epäonnistui/keskeneräisen tehtävän. 

Siirry vasemmalla **Seuranta näkymät** -välilehti napsauttamalla sitä. 

![Seurannan näkymät-välilehti](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Tällä hetkellä kolme järjestelmänäkymiä ei tueta. Valitse vaihtoehto, kun haluat tarkastella viimeisimmät toiminta windows (tai) epäonnistui toiminta windows () keskeneräisen tehtävän windows toiminta Windows-luettelossa (keskimmäisen ruudun alareunassa). 

Kun valitset **Viimeisimmät toiminta windows** -vaihtoehdon, näet kaikki viimeisimmät tehtävän-ikkunat laskevaan järjestykseen, **Yritä viimeksi aika**. 

Voit **epäonnistui toiminta windows** -näkymää näet kaikki epäonnistui toiminta windows-luettelossa. Valitse luettelon ja tarkastella tietoja **Ominaisuudet** (tai) **Tehtävän ikkunan Explorer**epäonnistui toiminta-ikkuna. Voit ladata kaikki lokit epäonnistui toiminta-ikkuna. 


## <a name="sorting-and-filtering-activity-windows"></a>Lajittelu ja suodatus toiminta windows
Komentopalkin **alkamisaika** ja **Päättymisaika** -asetusten muuttaminen suodattimen toimintaa Windowsiin. Kun olet muuttanut alkamisaika ja päättymisaika, valitse päättymisaika-päivittämiseen toiminta Windows-luettelon vieressä olevaa-painiketta.

![Aloitus- ja päättymisajat](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] Tällä hetkellä kaikki ajat ovat seuranta ja hallinta-sovelluksen UTC-muodossa. 

Valitse **tehtävän Windows-luettelosta**sarakkeen nimi (esimerkiksi: tila). 

![Tehtävän Windowsin luettelon sarake-valikko](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Toimi seuraavasti:

- Lajittele nousevaan järjestykseen.
- Lajittele laskevaan järjestykseen.
- Suodattaminen yhteen tai useaan arvoon (valmis, odottaa jne.)

Jos määrität suodattimen sarakkeen, näet-sarakkeen arvojen kuuluvien suodatetut arvot sarakkeen käytössä Suodatin-painiketta. 

![Tehtävän Windowsin luettelon sarakkeen suodattaminen](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Voit poistaa suodattimet saman ponnahdusikkuna. Kaikkien suodattimien toiminta windows-luettelossa, napsauta komentopalkin Tyhjennä suodatin-painiketta. 

![Kaikkien suodattimien toiminta Windows-luettelosta](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>Erän toimintojen tekemistä

### <a name="rerun-selected-activity-windows"></a>Suorita valittu tehtävä windows
Valitse tehtävä-ikkunassa, valitse ensimmäinen palkin komentopainikkeen alaspäin osoittavaa nuolta ja valitse **Suorita** / **kanssa suorittamalla edeltävät myyntijakso**. Kun olet valinnut **Suorita kanssa edeltävät myyntijakso-** vaihtoehdon, myös kaikki seuraavat toiminta windows suorittaa uudelleen. 
    ![Suorita tehtävä-ikkunassa](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Voit valita useita toiminta windows-luettelossa myös ja Suorita uudelleen samaan aikaan. Voit suodattaa toiminta windows tilan perusteella (esimerkiksi: **epäonnistui**) ja suorita epäonnistui toiminta windows ongelman, joka aiheuttaa toiminta windows epäonnistuu korjaamisen jälkeen. Artikkelissa on seuraavassa osassa suodattamisesta toiminta windows-luettelossa.  

### <a name="pauseresume-multiple-pipelines"></a>Pysäytä/jatka useita putkistot
Voit Monivalinta kahden tai useamman putkistot (joko CTRL) ja käyttää komentopalkin painikkeita (korostettuna punaiseen suorakulmioon seuraavan kuvan mukaisesti) keskeyttäminen ja jatkaminen ne kerrallaan.

![Keskeyttää/ansioluettelo-komentopalkki](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>Ilmoitusten luominen 
Ilmoitukset-sivulla voit luoda ilmoituksen aiemmin ilmoitusten näyttäminen, muokkaaminen ja poistaminen. Voit myös poistaa käytöstä tai ottaa sen käyttöön ilmoituksen. Jos haluat nähdä ilmoitukset-sivulle, valitse Ilmoitukset-välilehti.

![Ilmoitukset-välilehti](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Ilmoituksen luominen

1. Valitse **Lisää ilmoitus** Lisää ilmoituksen. Näet tiedot-sivu. 

    ![Luo ilmoituksia - tiedot-sivu](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. Määritä **nimi** ja **kuvaus** ilmoituksen, ja valitse **Seuraava**. Raportissa pitäisi näkyä **suodattimet** -sivulla.

    ![Luo ilmoituksia - suodattimet-sivulla](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. Valitse **tapahtuma**, **tila**ja **alitila** (valinnainen), jossa haluat varoituksen Data Factory-palvelu ja valitse **Seuraava**. Näyttöön tulee **vastaanottajat** -sivu.

    ![Luo ilmoituksia - vastaanottajat-sivu](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. **Sähköpostin tilauksen valvojat** -vaihtoehdon ja/tai kirjoita **muita järjestelmänvalvojan sähköposti**ja valitse **Valmis**. Raportissa pitäisi näkyä luettelossa ilmoituksen. 
    
    ![Ilmoitukset-luettelo](./media/data-factory-monitor-manage-app/AlertsList.png)

Ilmoitukset-luettelossa Muokkaa ja poista ja poista käytöstä/Enable ilmoituksen ilmoitukseen liittyvä painikkeilla. 

### <a name="eventstatussubstatus"></a>Tapahtuman/tila/alitila
Seuraavassa taulukossa on luettelo käytettävissä olevista tapahtumista ja tilat (ja substatuses).

Tapahtuman nimi | Tila | Sub-tila
-------------- | ------ | ----------
Tehtävän suorittaminen aloittaminen | Käytön aloittaminen | Aloittaminen
Tehtävän suorittaminen valmiiksi | Onnistui | Onnistui 
Tehtävän suorittaminen valmiiksi | Epäonnistui| Epäonnistuneiden resurssivaraus<br/><br/>Suorittaminen epäonnistui<br/><br/>Aikakatkaisu<br/><br/>Epäonnistuneiden vahvistus<br/><br/>Ilman ylläpitäjää
Tarvittaessa HDI klusterin luominen aloittaminen | Käytön aloittaminen | &nbsp; |
Tarvittaessa HDI klusterin luominen onnistui | Onnistui | &nbsp; |
Poistaa tarvittaessa HDI klusteri | Onnistui | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>Muokkaa tai poista ja poista ilmoituksen avulla


![Ilmoitukset-painikkeet](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


