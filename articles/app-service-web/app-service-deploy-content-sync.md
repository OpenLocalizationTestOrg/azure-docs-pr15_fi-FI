<properties
    pageTitle="Synkronoi sisällön cloud-kansiosta sovelluksen Azure-palvelu"
    description="Opettele käyttöönottaminen sovelluksen Azure App palvelun kautta sisällön synkronointi pilven-kansiosta."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Synkronoi sisällön cloud-kansiosta sovelluksen Azure-palvelu

Tässä opetusohjelmassa näytetään, miten ottamaan [Azure](http://go.microsoft.com/fwlink/?LinkId=529714) -sovelluksen palveluun synkronoinnin Suositut cloud tallennustilan palvelujen, kuten Dropbox ja OneDrive-sisältöä. 

## <a name="overview"></a>Synkronoi sisällön käyttöönoton yleiskatsaus

Integroitu sovellus palvelun [Kudu käyttöönoton ohjelma](https://github.com/projectkudu/kudu/wiki) on tarjoaa tarvittaessa sisällön synkronoinnin käyttöönotto. [Azure-portaalissa](https://portal.azure.com)voit määrittää kansion cloud tallennustilaan, sovelluksen koodi ja-kansion sisältö ja synkronoida sovelluksen palveluun napsauttamalla hiiren napsautuksella. Sisällön synkronointi käyttämällä Kudu prosessin muodosta ja käyttöönottoa varten. 
    
## <a name="contentsync"></a>Miten voit ottaa käyttöön sisällön synkronoinnin käyttöönotto
Voit ottaa käyttöön sisällön synkronointi [Azure-portaalissa](https://portal.azure.com), toimimalla seuraavasti:

1. Valitse sinua sovelluksen sivu Azure-portaalissa, **asetukset** > **Käyttöönoton lähde**. **Valitse**lähde ja valitse sitten **OneDrive** tai **Dropbox** lähteenä käyttöönottoa varten. 

    ![Sisällön synkronointi](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] API pohjana eroavaisuudet vuoksi **OneDrive for Business** ei voi tällä hetkellä. 

2. Suorita luvan työnkulun käyttöön App palvelun käyttöoikeuksien tiettyyn ennalta määritetyt nimettyjen polkuun Onedriveen tai Dropboxiin, jossa kaikki sovelluksen palvelun sisältö tallennetaan.  
    Luvan App palvelun jälkeen ympäristön avulla voit Luo nimetty sisällön polussa sisällön kansio tai valitse tässä nimettyjen sisällön polussa sisällön kansio. Nimetty sisällön polkujen käyttää sovelluksen palvelun synkronointi pilven tallennustilan tilit-kohdassa ovat seuraavat:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Dropbox**:`Dropbox\Apps\Azure`

3. Sisällön alkusynkronoinnin jälkeen sisällön synkronointi voidaan aloittaa pyydettäessä Azure-portaalista. Käyttöönoton historia on käytettävissä **käyttöönottoa** sivu.

    ![Käyttöönoton historia](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
[Ota käyttöön dropboxissa](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx)on lisätietoja Dropbox käyttöönottoa varten. 


