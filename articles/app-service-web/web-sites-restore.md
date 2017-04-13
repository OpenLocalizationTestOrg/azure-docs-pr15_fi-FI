<properties 
    pageTitle="Palauttaa Azure-sovelluksen" 
    description="Opettele sovelluksen palauttaminen varmuuskopiosta." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Palauttaa Azure-sovelluksen

Tässä artikkelissa näytetään, miten voit palauttaa [Azure App palvelun](../app-service/app-service-value-prop-what-is.md) , joka on varmuuskopioidut (katso [Azure sovelluksen varmuuskopioiminen](web-sites-backup.md))-sovelluksen. Voit palauttaa sen linkitetyn tietokantoja (SQL-tietokanta tai MySQL) tarvittaessa ja sovelluksen edelliseen tilaan tai jokin alkuperäisen sovelluksen varmuuskopiointi perusteella uuden sovelluksen luominen. Uuden sovelluksen, joka suoritetaan rinnakkain uusimpaan versioon luominen voi olla hyötyä A / B testaamiseen.

Varmuuskopioista palauttaminen on käytettävissä käynnissä **Vakio** - ja **Premium** taso-sovelluksiin. Katso tietoja skaalausta, sovelluksen [skaalata Azure-sovelluksen määrittäminen](web-sites-scale.md). **Premium** tason avulla on suurempi määrä päivittäin varmuuskopioita muun kuin **Vakio** taso.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Sovelluksen palauttaminen varmuuskopio

1. Valitse Azure-portaalissa sovelluksen **asetukset** -sivu, **varmuuskopioiden** näyttämään **varmuuskopiot** -sivu. Valitse **Palauta nyt** painikkeita. 
    
    ![Valitse Palauta nyt][ChooseRestoreNow]

3. Valitse Varmuuskopioi lähde ensin **palauttaminen** -sivu. 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    **Sovelluksen varmuuskopio** -asetus näyttää kaikki olemassa olevat varmuuskopiot nykyisen sovelluksen ja voit helposti valita niistä. 
    **Tallennustilan** -vaihtoehdon voit valita minkä tahansa varmuuskopion ZIP-tiedoston aiemmin Azure-tallennustilan tilin ja tilauksen säilö. 
    Jos yrität palauttaminen varmuuskopiosta toisesta sovelluksesta, käytä **tallennustilan** -vaihtoehtoa.

4. Määritä Palauta sovellus kohde Palauta **kohde**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] Jos valitset **Korvaa**, kaikki olemassa olevat tiedot nykyinen sovellus poistetaan. Ennen kuin valitset **OK**, varmista, että se on täsmälleen mitä haluat tehdä.
    
    Voit valita Palauta varmuuskopio app saman resoure ryhmän toisessa sovelluksessa **Luodun sovelluksen** . Ennen kuin voit käyttää tätä asetusta, olisi olet jo luonut toisessa sovelluksessa resurssi-ryhmän kanssa peilaukseen tietokannan kokoonpano määritetty sovelluksen varmuuskopion. 
    
5. Valitse **OK**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Ladata tai poistaa varmuuskopion tallennustilan tililtä
    
1. Pääikkunassa **Selaa** sivu Azure-portaalin Valitse **tallennustilan tilit**.
    
    Olemassa olevan tallennustilan asiakkaiden luettelo näytetään. 
    
2. Valitse tallennustilan tili, joka sisältää varmuuskopion, jonka haluat ladata tai poistaa.
    
    Tallennustilan tilin sivu tulee näkyviin.

3. Valitse tallennustilan accountn-sivu haluat säilö
    
    ![Näytä säilöt][ViewContainers]

4. Valitse varmuuskopiotiedosto, jonka haluat ladata tai poistaa.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Valitse **Lataa** tai **poistaa** sen mukaan, mitä haluat tehdä.  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>Näytön palautustyön
    
1. Jos haluat lisätietoja onnistumisesta tai epäonnistumisesta sovelluksen palauttaminen toiminnon, siirry Azure-portaalissa **Valvontaloki** -sivu. 
    
    **Valvontalokien** -sivu näyttää kaikki toimintoja, sekä taso, tila ja resurssin tiedot.
    
2. Vieritä löydät haluamasi palautustoiminto ja valitse se napsauttamalla.

Tiedot-sivu näyttää käytettävissä palautustoiminto liittyviä tietoja.
    
## <a name="next-steps"></a>Seuraavat vaiheet

Voit myös varmuuskopioida ja palauttaa sovelluksen Service-sovelluksista käsin REST API (katso [Käytä LOPUT varmuuskopioiminen ja palauttaminen App palvelun sovellukset](websites-csm-backup.md)).

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
