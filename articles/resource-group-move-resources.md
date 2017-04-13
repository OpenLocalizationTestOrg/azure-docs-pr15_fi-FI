<properties 
    pageTitle="Siirrä uusi resurssiryhmä resurssien | Microsoft Azure" 
    description="Azure resurssien hallinnan avulla voit siirtää resurssien uusi resurssiryhmä tai tilaus." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/21/2016" 
    ms.author="tomfitz"/>

# <a name="move-resources-to-new-resource-group-or-subscription"></a>Siirrä uusi resurssiryhmä ja tilauksen resurssit

Tässä ohjeaiheessa esitellään, miten uusi tilaus tai valitse saman tilauksen uusi resurssiryhmä resurssien siirtyminen. Siirry resurssin käyttämällä portaalin, PowerShell, Azure CLI tai REST-Ohjelmointirajapinnalla. Tässä ohjeaiheessa Siirrä-toiminnot ovat käytettävissä sinulle ilman mitään apua Azure tuesta.

Yleensä resurssien siirtäminen, kun olet päättänyt:

- Laskutuksen tarkoituksiin resurssi on eri tilauksen tallennetaan.
- Resurssin enää jakaa saman elinkaari aiemmin ryhmiteltynä resursseina. Haluat siirtää uusi resurssiryhmä, jotta voit hallita resurssin erikseen muita resursseja.

Kun siirrät resursseja, sekä lähde-ryhmän ja kohderyhmän lukitaan toiminnon aikana. Kirjoittaminen ja poista toimintojen torjuttavien ryhmät, ennen kuin siirtäminen on valmis.

Et voi muuttaa resurssia sijainti. Resurssin siirtäminen vain siirtää sen uusi resurssiryhmä. Uusi resurssiryhmä voi olla eri paikkaan, mutta, jotka eivät muutu resurssin sijainti.

> [AZURE.NOTE] Tässä artikkelissa käsitellään siirtää aiemmin Azure resursseja tilin työkalua tarjota. Jos haluat muuttaa Azure tilisi ojentamassa (kuten päivittäminen ryhmävakuutussopimukset valmiiksi maksaa) samalla, kun aiemmin resurssien käsitteleminen-kohdassa [vaihtaminen Azure tilauksesi toiseen tarjous](billing-how-to-switch-azure-offer.md). 

## <a name="checklist-before-moving-resources"></a>Tarkistusluettelon ennen siirtämistä resurssit

On tärkeää muutama suorittamiseen ennen siirtämistä resurssin. Voit välttää virheet vahvistamalla näitä ehtoja.

1. Palvelu on otettava voi siirtää resursseja. Katso lisätietoja siitä, mitkä [palvelut käyttöön siirtäminen resurssien](#services-that-enable-move)alla olevassa luettelossa.
1. Lähde- ja tilaukset on oltava samassa [Active Directory-vuokraajan](./active-directory/active-directory-howto-tenant.md)kuluessa. Jos haluat siirtää uuden vuokraajan, ota yhteyttä.
2. Kohde-tilaus on rekisteröitävä resurssin palveluntarjoajasi resurssin siirretään. Jos et, näyttöön tulee virhesanoma, jossa ilmoitetaan, **tilausta ei ole rekisteröity resurssilaji**. Saattaa ilmetä ongelman liikkuva resurssin uusi tilaus, mutta, että tilaus on aina käytetty kyseisen resurssin laji. Lisätietoja rekisteröinti-tilan tarkistaminen ja rekisteröi resurssin tarjoajien on artikkelissa [resurssin palveluntarjoajat ja tyypit](../resource-manager-supported-services.md#resource-providers-and-types).
4. Jos siirrät sovelluksen palvelun sovelluksen, olet tarkistanut [App palvelurajoitukset](#app-service-limitations).
4. Jos siirrät resurssien palautuksen palveluihin liittyviä, olet tarkistanut [palautus-palveluiden rajoituksista](#recovery-services-limitations)
5. Jos siirrät – perinteinen mallin resursseja, olet tarkistanut [perinteinen käyttöönoton rajoitukset](#classic-deployment-limitations).

## <a name="when-to-call-support"></a>Milloin kannattaa Soita tukeen

Voit siirtää useimmat resursseja Omatoiminen-toiminnot näkyvät tässä ohjeaiheessa kautta. Käytä omatoimista toiminnot, jotka:

- Siirrä Resurssienhallinta resurssit.
- Siirrä perinteinen resurssien mukaan [perinteinen käyttöönoton rajoitukset](#classic-deployment-limitations). 

Soita tukeen, kun haluat:

- Siirrä resurssien uusi Azure-tili (ja Active Directory-vuokraajan).
- Siirtää perinteinen resursseja, mutta rajoitukset kanssa on ongelmia.

## <a name="services-that-enable-move"></a>Palvelut, jotka mahdollistavat siirtäminen

Palvelut, jotka mahdollistavat siirtäminen uusi resurssiryhmä- ja tilausasioiden ovat nyt:

- API hallinta
- Sovelluksen palvelun apps (online) - kohdassa [sovelluksen palvelurajoitukset](#app-service-limitations)
- Automaatio
- Erän
- CDN
- Cloud Services - kohdassa [perinteinen käyttöönoton rajoitukset](#classic-deployment-limitations)
- Tietoja Factory
- DNS
- DocumentDB
- HDInsight klustereiden
- IoT keskittimet
- Avaimen säilö 
- Media-palveluita
- Mobiili välitys
- Ilmoitus keskittimet
- Toiminnalliset tiedot
- Redis välimuisti
- Ajoitus
- Haku
- Palvelun Bus
- Tallennustilan
- Tallennustilan (perinteinen) - kohdassa [perinteinen käyttöönoton rajoitukset](#classic-deployment-limitations)
- SQL-tietokantapalvelin, johon - palvelin ja tietokanta on sijaittava saman resurssiryhmä. Kun siirrät SQL server, myös kaikki sen tietokantojen siirretään.
- Näennäiskoneiden - kuitenkin käytettäessä tue Siirrä uusi tilaus, kun sen varmenteet on tallennettu avain säilöön
- Näennäiskoneiden (perinteinen) - kohdassa [perinteinen käyttöönoton rajoitukset](#classic-deployment-limitations)
- Virtuaalinen verkot

## <a name="services-that-do-not-enable-move"></a>Palvelut, jotka eivät salli siirtäminen

Palvelut, jotka tällä hetkellä Älä ota käyttöön siirtämällä resurssin ovat seuraavat:

- Sovelluksen yhdyskäytävän
- Hakemuksen tiedot
- Express reitin
- Palautus Services säilöön - myös ei siirrä suorittaminen, verkon ja tallennustilaa palautus Services säilö liittyvät resurssit, katso [palautus-palveluiden rajoituksista](#recovery-services-limitations).
- Näennäiskoneiden varmenteen avaimen säilöön tallennettuja kanssa
- Näennäiskoneiden asteikko joukot
- Virtuaalinen verkkojen (perinteinen) - kohdassa [perinteinen käyttöönoton rajoitukset](#classic-deployment-limitations)
- VPN-yhdyskäytävän

## <a name="app-service-limitations"></a>Sovelluksen palvelurajoitukset

Kun käsittelet App palvelun sovellukset, ei voi siirtää vain sovelluksen palvelun suunnitelma. Siirry sovelluksen palvelun sovellukset vaihtoehdot ovat:

- Siirtää sovelluksen palvelusopimus ja kaikki muut sovelluksen palvelun resurssit resurssin kyseisen ryhmän uusi resurssiryhmä, joka ei ole vielä App Service resurssit. Tämä vaatimus tarkoittaa sitä, sinun on siirrettävä jopa, joka ei ole liitetty App palvelusopimus App palvelun resurssit. 
- Siirrä sovellukset eri resurssiryhmä, mutta säilyttää alkuperäisen resurssiryhmä kaikki sovelluksen palvelusopimusten vaihtoehdot.

Jos alkuperäisen resurssiryhmä sisältyy myös sovelluksen tiedot-resurssi, ei voi siirtää resurssia, koska sovelluksen tietoja ei tällä hetkellä Ota siirtoa. Jos lisäät sovelluksen tiedot resurssin liikkuva App palvelun sovellukset, koko Siirrä epäonnistuu. Kuitenkin hakemuksen tiedot ja sovelluksen palvelusopimus ei tarvitse asuvat samassa resurssiryhmä kuin app-sovelluksen toiminta.

Esimerkki: Resurssiryhmä sisältää:

- **Web-a** joka liittyy **suunnitelman a** ja **sovelluksen-tiedot-a**
- **web-b** , joka liittyy **suunnitelman b** ja **sovelluksen-tiedot-b**

Vaihtoehdot ovat:

- Siirrä **web a**, **suunnitelman a**- **web-b**ja **suunnitelman b**
- Siirrä **web a** ja **web-b**
- Siirrä **web-a**
- Siirrä **web-b**

Kaikki muut yhdistelmät ratkaisemiseen siirtäminen Resurssityyppi, jota ei voi siirtää (hakemuksen tiedot) tai jätä takana Resurssityyppi, joka ei voi jättää takana, kun siirrät sovelluksen palvelusopimus (tyyppi App palvelun resurssin).

Jos eri resurssiryhmä kuin sovelluksen palvelusopimus sijaitsee web-sovelluksen, mutta haluat siirtää molemmat uusi resurssiryhmä, sinun täytyy suorittaa siirron seuraavalla tavalla. Esimerkki:

- **Web-a** sijaitsee **web-ryhmä**
- **suunnitelman joka** sijaitsee **suunnitelma-ryhmä**
- Haluat **web-a** ja **palvelupaketti on** **yhdistetty** ryhmässä sijaitsevan

Suoritettava tämä siirtää suorittaa kaksi erillistä Siirrä seuraavassa järjestyksessä:

1. Siirrä **web-a** **suunnitelman** ryhmään
2. Siirrä **web a** ja **palvelupaketti on** **yhdistetty**ryhmään.

Voit siirtää sovelluksen-palvelun varmenteen uusi resurssiryhmä tai-Tilaustani ongelmitta. Kuitenkin jos koodiin sisältyy SSL-varmenne, jota ulkoisesti ostettu ja ladattu-sovellukseen, sinun on poistettava varmenteen ennen siirtämistä web Appissa. Voit esimerkiksi suorittaa seuraavia ohjeita:

1. Poistaa ladatut sertifikaatin web Appista
2. Siirrä web Appissa
3. Lataa sertifikaatti web App-sovellukseen

## <a name="recovery-services-limitations"></a>Palautus Services rajoitukset

Siirrä ei ole otettu käyttöön tallennus-verkko- tai resurssit, palauttaminen ja Azure palauttaminen määritit Laske. 

Oletetaan, että määrittänyt replikoinnin paikallisen-laitteiden tallennustilan-tilille (Storage1) ja suojattu tietokone tulee automaattisesti jälkeen voit Azure virtual machine (VM1), joka on liitetty virtual verkon (Network1) kuin haluat. Nämä Azure resurssit - Storage1, VM1 ja Network1 - ei voi siirtää, saman tilauksen piiriin kuuluvien resurssien ryhmien tai -tilauksissa.

## <a name="classic-deployment-limitations"></a>Perinteinen käyttöönoton rajoitukset

Siirtymisen – perinteinen mallin resursseja asetukset vaihtelevat mukaan, onko siirrät resurssit tilauksen tai uuteen tilaukseen. 

### <a name="same-subscription"></a>Saman tilauksen

Kun siirrät resurssit ryhmästä yksi resurssi toiseen saman tilauksen piiriin kuuluvien resurssiryhmä, koskevat seuraavat rajoitukset:

- Virtuaalinen verkkojen (perinteinen) ei voi siirtää.
- Näennäiskoneiden (perinteinen) on siirrettävä pilvipalvelussa. 
- Pilvipalvelussa voi siirtää vain, kun siirto on sen näennäiskoneiden.
- Vain yksi pilvipalvelussa voidaan siirtää kerrallaan.
- Vain yksi tallennustilan-tili (perinteinen), voidaan siirtää kerrallaan.
- Tallennustilan tilin (perinteinen) ei voi siirtää virtual machine tai pilvipalveluun sama toiminto.

Siirry saman tilauksen piiriin kuuluvien uusi resurssiryhmä perinteinen resurssien [portal](#use-portal), [PowerShellin Azure](#use-powershell), [Azure CLI](#use-azure-cli)tai [REST API](#use-rest-api)vakio Siirrä-toimintojen käyttäminen Voit käyttää samat toiminnot, kun käytät siirtymisen Resurssienhallinta resurssit.

### <a name="new-subscription"></a>Uusi tilaus

Kun siirrät resurssien uusi tilaus, koskevat seuraavat rajoitukset:

- Kaikki perinteinen resurssit-tilaus on siirrettävä samalla kertaa.
- Kohde-tilaus ei saa olla perinteinen resurssit.
- Siirrä voidaan pyytää vain perinteinen siirtää erillisessä REST API-Liittymän kautta. Vakio resurssien hallinnan siirtäminen-komennot eivät toimi, kun perinteinen resurssien siirtäminen uuteen tilaukseen.

Siirrä perinteinen resurssien uusi tilaus, sinun on käytettävä REST-toiminnoista, joita ovat perinteinen resurssit. Seuraavien toimien perinteinen resurssien Siirry uuteen tilaukseen.

1. Tarkista, jos lähde-tilauksen voi osallistua rajat-tilauksen Siirrä. Seuraavan toiminnon avulla:

         POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
     Pyyntö tekstiosaan ovat seuraavat:

         {
           "role": "source"
         }

     Vahvistus-toiminnon vastaus on seuraavanlainen:

         {
           "status": "{status}",
           "reasons": [
             "reason1",
             "reason2"
           ]
         }

2. Tarkista, jos kohdetilauksen voi osallistua rajat-tilauksen Siirrä. Seuraavan toiminnon avulla:

         POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01

     Pyyntö tekstiosaan ovat seuraavat:

         {
           "role": "target"
         }

     Vastaus on samassa muodossa kuin lähteen tilauksen vahvistus.

3. Sekä tilaukset välittää vahvistus, jos Siirrä kaikki perinteinen resurssit yksi tilaus seuraavan toiminnon toinen tilaus:

         POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

    Pyyntö tekstiosaan ovat seuraavat:

         {
           "target": "/subscriptions/{target-subscription-id}"
         }

Toiminto voi kestää useita minuutteja. 

## <a name="use-portal"></a>Portaalin käyttäminen

Siirrä uusi resurssiryhmä **saman tilauksen**resursseja, valitse sisältävä resursseja resurssiryhmän ja valitse sitten **Siirrä** .

![Resurssien siirtäminen](./media/resource-group-move-resources/edit-rg-icon.png)

Vaihtoehtoisesti voit resurssien siirtäminen **uuteen tilaukseen**, jotka sisältävät kyseiset resursseja resurssiryhmän Valitse ja valitse sitten tilauksen muokkauskuvaketta.

![Resurssien siirtäminen](./media/resource-group-move-resources/change-subscription.png)

Valitse Siirrä resurssit ja kohde-resurssiryhmä. Kuittaa täytyy päivittää komentosarjojen näiden resurssien ja valitse sitten **OK**. Jos valitsit tilauksen muokkauskuvaketta edellisessä vaiheessa, on valittava kohde-tilaus.

![kohteen valitseminen](./media/resource-group-move-resources/select-destination.png)

**Ilmoitukset**näet, että siirtotoiminto on käynnissä.

![Näytä siirron tila](./media/resource-group-move-resources/show-status.png)

Kun se on valmis, saat ilmoituksen tuloksen.

![Näytä siirtää tulos](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>PowerShellin käyttäminen

Jos haluat siirtää aiemmin resurssien toiseen resurssiryhmä tai tilauksen, **Siirrä AzureRmResource** -komennolla.

Ensimmäinen esimerkissä, voit siirtää yhden resurssin uusi resurssiryhmä.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

Toinen esimerkki näyttää, miten voit siirtyä useiden resurssien uusi resurssiryhmä.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Siirry Uusi tilaus sisältää **DestinationSubscriptionId** -parametrin arvo.

Sinua pyydetään vahvistamaan, että haluat siirtää määritettyjä resursseja.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## <a name="use-azure-cli"></a>Käytä Azure CLI

Jos haluat siirtää aiemmin resurssien toiseen resurssiryhmä tai tilauksen, komennolla **Siirry azure resurssi** . Haluat tarjota resursseista siirtää resurssitunnukset. Voit hankkia resurssitunnukset seuraavalla komennolla:

    azure resource list -g sourceGroup --json

Joka palauttaa seuraavanlainen:

    [
      {
        "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
        "name": "storagedemo",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "southcentralus",
        "tags": {},
        "kind": "Storage",
        "sku": {
          "name": "Standard_RAGRS",
          "tier": "Standard"
        }
      }
    ]

Seuraavassa esimerkissä, voit siirtää tallennustilan tilin uusi resurssiryhmä. Ole **-i** -parametrin resurssitunnus CSV-luettelo on siirrettävä.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
    
Sinua pyydetään vahvistamaan, että haluat siirtää määritettyä resurssia.

## <a name="use-rest-api"></a>Käytä REST-Ohjelmointirajapinnalla

Jos haluat siirtää aiemmin resurssien toiseen resurssiryhmä tai tilauksen, suorita:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

Pyyntö tekstiosaan Määritä kohde resurssiryhmä ja resursseja. Katso lisätietoja siirron REST-toiminto [siirtää resursseja](https://msdn.microsoft.com/library/azure/mt218710.aspx).


## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja PowerShell cmdlet-komennot hallinta-tilauksesi on artikkelissa [Azure PowerShellin resurssien hallinta](powershell-azure-resource-manager.md).
- Lisätietoja hallinnan tilauksen Azure CLI komennoista on artikkelissa [Azure CLI resurssien hallinta](xplat-cli-azure-resource-manager.md).
- Lisätietoja tilauksen hallintaan portaalin ominaisuuksista on artikkelissa [Azure-portaalissa voit hallita resursseja](./azure-portal/resource-group-portal.md).
- Tietoja looginen organisaation soveltaminen resurssit-kohdassa [käyttäminen tunnisteen, kun haluat järjestää resurssit](resource-group-using-tags.md).
