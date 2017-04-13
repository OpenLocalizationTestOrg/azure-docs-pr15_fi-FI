<properties
   pageTitle="Palvelun kangasta klusterin Visual Studiossa määrittäminen | Microsoft Azure"
   description="Tässä artikkelissa käsitellään määrittämään palvelun kangasta klusterin Azure resurssiryhmä-Project Visual Studiossa luodut Azure Resurssienhallinta-mallin avulla"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Palvelun kangasta klusterin Visual Studiossa määrittäminen
Tässä artikkelissa käsitellään muodostaa Azure palvelun kangasta-klusterin Visual Studio ja Azure Resurssienhallinta-mallin avulla. Käytämme resurssin yhteisessä projektissa ja Visual Studio Azure mallin luomiseen. Kun malli on luotu, se voidaan ottaa käyttöön suoraan Azure Visual Studio. Se voidaan myös komentosarjan tai jatkuva integrointi (CI) tilojen osana.

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Palvelun kangasta klusterin mallin luominen Azure resurssi-ryhmän Projectin avulla
Aloita Avaa Visual Studio ja luoda Azure resurssin ryhmän projektin (se on käytettävissä **Cloud** -kansiossa):

![Uusi projekti-valintaikkunasta, jossa valitun projektin Azure resurssiryhmä][1]

Voit luoda uuden Visual Studio-ratkaisun projektin tai lisätä sen aiemmin ratkaisu.

>[AZURE.NOTE] Jos ei ole näkyvissä Cloud solmun Azure resurssien ryhmä-projektiin, SDK Azure-asennus ei ole. Käynnistä WWW-ympäristö asennusohjelma ([asentaa sen nyt,](http://www.microsoft.com/web/downloads/platform.aspx) Jos sinulla ei vielä ole), ja Hae "Azure SDK, .NET- ja asenna Visual Studio-version kanssa yhteensopivalla versiolla.

Kun valitset OK-painiketta, Visual Studio pyytää valitsemaan luotavan Resurssienhallinta-malliin:

![Valitse Azure-malli-valintaikkuna, jossa on Service kangasta klusterin malli valittuna][2]

Valitse palvelun kangasta klusterin-malli ja valitse sitten OK-painiketta uudelleen. Projektin ja Resurssienhallinta-malli on luotu.

## <a name="prepare-the-template-for-deployment"></a>Mallin valmisteleminen käyttöönottoa varten
Ennen kuin malli otetaan käyttöön klusterin luominen, sinun on annettava tarvittavat malliin parametrien arvot. Seuraavat parametriarvot luetaan `ServiceFabricCluster.parameters.json` tiedosto, joka on `Templates` resurssien ryhmä project-kansioon. Avaa tiedosto ja seuraavat arvot:

|Parametrin nimi           |Kuvaus|
|-----------------------  |--------------------------|
|adminUserName            |Palvelun kangasta koneet (solmujen) järjestelmänvalvojatilin nimi.|
|certificateThumbprint    |Varmenne, jota suojaa klusterin allekirjoitus.|
|sourceVaultResourceId    |*Resurssitunnus* varmenne, jota suojaa klusterin tallennuspaikan avaimen säilö.|
|certificateUrlValue      |Klusterin suojausvarmenne URL-osoite.|

Visual Studio palvelun kangasta Resurssienhallinta-malli luo suojatun klusteriin, joka on suojattu varmenne. Tämän todistuksen tunnistetaan kolmea viimeistä Malliparametrit (`certificateThumbprint`, `sourceVaultValue`, ja `certificateUrlValue`), ja se on oltava **Azure avaimen säilö**. Lisätietoja siitä, miten voit luoda klusterin suojausvarmenne, katso [palvelun kangasta klusterin suojaustapoja](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) artikkelissa.

## <a name="optional-change-the-cluster-name"></a>Valinnainen: klusterin nimen muuttaminen
Jokaisen palvelun kangasta klusterin on nimi. Kun kangasta klusterin luodaan Azure-klusterinimi määrittää (ja Azure alue) klusterin toimialueen nimi järjestelmän (DNS)-nimi. Jos nimeät yhteyttä klusterin esimerkiksi `myBigCluster`, ja sijainti (Azure alue), joka isännöi uuden klusterin resurssiryhmä on Yhdysvaltojen Itä, klusterin DNS-nimi on `myBigCluster.eastus.cloudapp.azure.com`.

Oletusarvoisesti klusterinimeä luodaan automaattisesti ja yksilöivä tekemät satunnaisia loppuliite liittäminen "klusterin" etuliite. Tämä on hyvin helppo käyttää mallia **Jatkuva integrointi** (CI) järjestelmän osana. Jos haluat käyttää tiettyä yhteyttä klusterin nimi, sähköpostiosoitteen loppuosa on havainnollinen, määritä arvo `clusterName` muuttujan Resurssienhallinta-mallitiedoston (`ServiceFabricCluster.json`) valitun nimi. Se on määritetty, että tiedostossa ensimmäisen muuttuja.

## <a name="optional-add-public-application-ports"></a>Valinnainen: Lisää julkisen sovelluksen portit
Voit halutessasi myös muuttamaan julkisen sovelluksen portit klusterin ennen niiden käyttöönottoa. Malli avaa oletusarvoisesti kahdella julkisen TCP-portit (80 ja 8081) määrittäminen. Jos tarvitset lisää sovellusten, Muokkaa Azure kuormituksen määritystä mallissa. Määritys on tallennettu tärkeimmät mallitiedoston (`ServiceFabricCluster.json`). Avaa tiedosto ja Etsi `loadBalancedAppPort`. Kunkin portin on liitetty kolmeen palvelutiedot:

1. Malli-muuttuja, joka määrittää portin TCP Port (portti)-arvo:

    ```json
    "loadBalancedAppPort1": "80"
    ```

2. *Näytteenottimen* , joka määrittää, kuinka usein ja kuinka kauan Azure kuormituksen tasauspalvelun yrittää käyttää tietyn palvelun kangasta solmun ennen epäonnistumista kautta toiseen. Kuormituksen resurssi keräysputkien kuuluvat. Näin ensimmäisen sovelluksen oletusportti näytteenottimen määritelmä:

    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```

3. *Kuormituksen tasaamisen sääntö* , joka sitoo yhdessä portin ja keräysputken, joka mahdollistaa kuormituksen usean palvelun kangasta klusterisolmujen joukko:

    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
Jos sovellukset, jotka haluat ottaa käyttöön klusterin on enemmän portit, voit lisätä ne luotaessa uusia näytteenotin ja kuormituksen tasaamisen sääntöjen määritykset. Lisätietoja Azure ladata tasaustoiminto Resurssienhallinta mallien käsittelemisestä on artikkelissa [luomisesta mallin avulla sisäinen kuormituksen](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>Mallin käyttöön Visual Studiossa
Kun olet tallentanut kaikki tarvittavat parametriarvot`ServiceFabricCluster.param.dev.json` tiedosto on valmis malli ja palvelun kangasta-klusterin luominen. Visual Studio ratkaisunhallinnassa resurssien ryhmä-projektiin hiiren kakkospainikkeella ja valitse **käyttöönotto | Uudet käyttöönoton...**. Tarvittaessa Visual Studio näytetään **käyttöönotto resurssiryhmä** -valintaikkunasta sinulta kysytään, haluatko todentaa Azure:

![Ottaa käyttöön resurssiryhmä-valintaikkuna][3]

Valintaikkunassa voit valita olemassa olevan Resurssienhallinta resurssiryhmän klusterin ja antaa asetus, jos haluat luoda uuden. Tavallisesti kannattaa käyttää erillisen resurssiryhmä palvelun kangasta klusterin.

Kun valitset Ota käyttöön-painiketta, Visual Studio kehottaa sinua vahvistamaan mallin parametriarvot. Osumien **Tallenna** -painiketta. Yksi parametri ei ole pysyviä arvoa: klusterin järjestelmänvalvojatilin salasanan. Sinun on annettava salasana arvo, kun Visual Studio kehottaa seuraavasti.

>[AZURE.NOTE] Azure SDK 2.9 alkaen Visual Studio tukee lukeminen salasanat **Azure avain** säilöstä käyttöönoton aikana. Mallin parametrit-valintaikkunassa Huomaa, `adminPassword` parametrin tekstiruudussa on pieni "avain"-kuvakkeen oikealla. Tämän kuvakkeen avulla voit valita olemassa olevan avaimen säilöön-salaisuus klusterin järjestelmänvalvojan salasanan. Vain Varmista, että ensimmäinen Enable Azure Resurssienhallinta access mallin käyttöönottoa avaimen säilö käytäntöjen Access Lisäasetukset. 

Voit valvoa edistymistä käyttöönottoprosessin Visual Studio kohde-ikkunassa. Kun mallin käyttöönottoa on valmis, uusi klusterin on valmis käyttämään!

>[AZURE.NOTE] Jos PowerShell on käyttänyt hallinnoimaan Azure tietokoneesta, jota käytät nyt, sinun on suoritettava hieman ylläpitotoimintoja.
>1. Ota käyttöön PowerShell-komentosarjoja suorittamalla [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) komento. Kehittäminen koneet "rajoittamaton" käytäntö on yleensä hyväksyttävä.
>2. Päätä, haluatko sallia diagnostiikan tietojen kerääminen PowerShellin Azure komentoja ja suorita [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) tai [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) tarpeen mukaan. Tarpeettomien kehotteita välttämiseksi mallin käyttöönoton aikana.

Jos luettelossa on virheitä, siirry [Azure portal](https://portal.azure.com/) ja Avaa resurssiryhmä, voit ottaa käyttöön. Valitse **kaikki asetukset**ja valitse sitten **ominaisuuksissa** -asetukset-sivu. Epäonnistui resurssiryhmä käyttöönoton jättää yksityiskohtaiset vianmääritystiedot siellä.

>[AZURE.NOTE] Palvelun kangasta klustereiden edellyttävät tietyn määrän solmujen antaa ylläpitää käytettävyys ja säilyttää tila - kutsutaan "ylläpito quorum." Tämän vuoksi ei ole Turvalliset Sammuta kaikki klusterin koneet paitsi, jos olet jo suorittanut [Osavaltio varmuuskopiointi](service-fabric-reliable-services-backup-restore.md).

## <a name="next-steps"></a>Seuraavat vaiheet
- [Lisätietoja palvelun kangasta klusterin Azure-portaalissa](service-fabric-cluster-creation-via-portal.md)
- [Lue, miten voit hallita ja palvelun kangasta sovellusten Visual Studiossa](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
