<properties 
    pageTitle="Azure portal ottaminen käyttöön Azure resurssien avulla | Microsoft Azure" 
    description="Käyttöön Azure portal ja Azure resurssien hallinta resurssit." 
    services="azure-resource-manager,azure-portal" 
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
    ms.date="09/15/2016" 
    ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Resurssienhallinta-mallit ja Azure-portaalin resursseja käyttöönotto

> [AZURE.SELECTOR]
- [PowerShellin](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-group-template-deploy-rest.md)

Tässä ohjeaiheessa esitellään [Azure portal](https://portal.azure.com) käyttäminen [Azure Resurssienhallinta](azure-resource-manager/resource-group-overview.md) ottamaan Azure resurssit. Tietoja resurssien hallinta-kohdassa [Hallitse Azure resurssien yritysportaalin kautta](./azure-portal/resource-group-portal.md).

Tällä hetkellä ole jokaisella palvelulla tukee portal tai Resurssienhallinta. Näistä palveluista sinun on käyttää [perinteinen portal](https://manage.windowsazure.com). Artikkelissa [Azure portaalin käytettävyys kaavion](https://azure.microsoft.com/features/azure-portal/availability/)kunkin palvelun tila.

## <a name="create-resource-group"></a>Luo resurssiryhmä

1. Luo tyhjä resurssiryhmä, valitsemalla **Uusi** > **hallinnan** > **Resurssiryhmä**.

    ![Luo tyhjä resurssiryhmä](./media/resource-group-template-deploy-portal/create-empty-group.png)

2. Anna sille nimi ja sijainti ja valitse tarvittaessa tilaus. Sinun on annettava resurssiryhmän sijainti, koska tietoja resursseja resurssiryhmän metatiedot. Yhteensopivuuden vuoksi haluat ehkä määrittää, että metatietojen tallennuspaikan. Suosittelemme, Määritä sijainti, johon useimmat resurssien tallennetaan. Samaan sijaintiin avulla voidaan yksinkertaistaa mallin.

    ![ryhmän arvot](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Ottaa käyttöön resurssien Marketplacesta

Kun olet luonut resurssiryhmä, voit asentaa resurssien siihen Marketplacesta. On Marketplace tarjoaa Yleisiä tilanteita, joissa ennalta määritetyt ratkaisuja.

1. Aloita käyttöönoton valitsemalla **Uusi** ja haluat ottaa käyttöön resurssin lajin. Etsi tietty haluat ottaa käyttöön resurssi-versiota.

    ![resurssin käyttöönotto](./media/resource-group-template-deploy-portal/deploy-resource.png)

2. Jos haluat ottaa käyttöön tietyn ratkaisu ei ole näkyvissä, voit etsiä Marketplacesta.

    ![Hae Marketplacesta](./media/resource-group-template-deploy-portal/search-resource.png)

3. Valitun resurssin tyypin mukaan on kokoelma, voit määrittää ennen käyttöönottoa haluamasi ominaisuudet. Näistä asetuksista eivät näy tässä, koska ne määräytyvät resurssilaji. Kaikki tyypit on valittava kohde resurssiryhmä. Seuraava kuva esittää verkkosovellukseen luomisesta ja ota se käyttöön luomasi resurssiryhmä.

    ![Luo resurssiryhmä](./media/resource-group-template-deploy-portal/select-existing-group.png)

    Vaihtoehtoisesti voit päättää luoda Resurssiryhmän resurssien käyttöönoton yhteydessä. Valitse **Luo uusi** ja kirjoita resurssiryhmän nimi.

    ![Luo uusi resurssiryhmä](./media/resource-group-template-deploy-portal/select-new-group.png)

4. Käyttöönoton alkaa. Käyttöönotto voi kestää muutaman minuutin kuluttua. Kun asennus on valmis, näyttöön tulee ilmoitus.

    ![Näytä ilmoitus](./media/resource-group-template-deploy-portal/view-notification.png)

5. Jälkeen käyttöönotto resurssit, voit lisätä lisää resursseja resurssiryhmä resurssien ryhmä-sivu valitsemalla **Lisää** -komennolla.

    ![Lisää resurssi](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Ottaa käyttöön mukautetun mallin resurssit

Jos haluat suorittaa käyttöönoton, mutta ei käytä malleja Marketplacesta, voit luoda mukautetun mallin, joka määrittää ratkaisu infrastruktuuri. Lisätietoja mallien luomisesta on artikkelissa [Azure-Resurssienhallinta yhtä aikaa muiden kanssa-mallit](resource-group-authoring-templates.md).

1. Ottaa käyttöön mukautetun mallin portaalin kautta valitsemalla **Uusi**ja Aloita haku **Mallin käyttöönottoa** varten, ennen kuin voit valita sen asetukset.

    ![Etsi malli käyttöönotto](./media/resource-group-template-deploy-portal/search-template.png)

2. Valitse käytettävissä olevat resurssit **Mallin käyttöönottoa** .

    ![Valitse malli käyttöönotto](./media/resource-group-template-deploy-portal/select-template.png)

3. Avaa tyhjä malli, joka on käytettävissä olevista avaamista mallin käyttöönoton jälkeen.

    ![mallin luominen](./media/resource-group-template-deploy-portal/show-custom-template.png)

    Valitse editorin Lisää JSON syntaksi, joka määrittää resurssit, jonka haluat ottaa käyttöön. Valitse **Tallenna** , kun olet valmis. Ohjeita kirjoittamiseen JSON syntaksi on artikkelissa [Resurssienhallinta mallin ongelmatilanteita](resource-manager-template-walkthrough.md).

    ![mallin muokkaaminen](./media/resource-group-template-deploy-portal/edit-template.png)

4. Tai valitse olemassa mallin [Azure pikaopas malleja](https://azure.microsoft.com/documentation/templates/). Nämä mallit ovat siirtämien yhteisön. Ne kattavat useita yleisiä tilanteita, joissa ja toinen lisännyt malli, joka on samanlainen kuin mitä yrität otetaan käyttöön. Voit hakea malleja etsimääsi, joka vastaa käyttämässäsi skenaariossa.

    ![Pikaopas-malli](./media/resource-group-template-deploy-portal/select-quickstart-template.png)

    Voit tarkastella valittu malli-editorissa.

5. Kun olet antanut kaikki muut arvot, valitse **Luo** ottaa mallin käyttöön. 

    ![mallin ottaminen käyttöön](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Ottaa käyttöön resurssien tiliisi tallentaa mallin pohjalta

Portaalin avulla voit tallentaa mallin Azure-tili ja ota sen myöhemmin uudelleen. Jos haluat lisätietoja käyttäminen nämä tallentanut malleja, [Yksityinen malleja Azure-portaalin käytön aloittaminen](./marketplace-consumer/mytemplates-getstarted.md).

1. Etsi tallennettu mallit, valitse **Selaa** > **malleja**.

    ![Selaa malleja](./media/resource-group-template-deploy-portal/browse-templates.png)

2. Mallit tallennetaan tiliisi luettelosta haluamasi vaihtoehto haluat käsitellä.

    ![tallennettujen mallien](./media/resource-group-template-deploy-portal/saved-templates.png)

3. Valitse **Ota käyttöön** , ota tämä tallennettu malli uudelleen.

    ![tallennetun mallin ottaminen käyttöön](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Valvontalokien on artikkelissa [valvonta toimintojen resurssien hallinta](resource-group-audit.md).
- Käyttöönoton vianmääritys-artikkelissa [vianmääritys resurssin ryhmän ominaisuuksissa Azure-portaalissa](resource-manager-troubleshoot-deployments-portal.md).
- Hakee mallin käyttöönoton tai resurssiryhmä, katso [Vie Azure Resurssienhallinta mallin aiemmin resursseista](resource-manager-export-template.md).





