<properties 
    pageTitle="Logiikan sovellusten Visual Studiossa luominen | Microsoft Azure" 
    description="Luo projekti Visual Studiossa, voit luoda ja logiikka sovelluksen käyttöönotto." 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Muodosta ja ota käyttöön Visual Studiossa sovellusten logiikka

Vaikka [Azure-portaalissa](https://portal.azure.com/) avulla voit suunnitella ja hallita logiikan sovelluksia, voit halutessasi myös suunnittelu ja käyttöönotto Visual Studio logiikan sovelluksen sijaan.  Logiikan sovellukset tulee määrittää monipuolisia Visual Studio toolset jonka avulla voit kehittää-ikkunan suunnittelusovelluksen logiikan sovelluksen, käyttöönotto- ja automaatiofunktiot mallia ja ota käyttöön kaikki-ympäristöön.  

## <a name="installation-steps"></a>Asennusohjeita

Alla on ohjeet asennetaan ja määritetään Visual Studio-työkalut funktioiden sovellusten.

### <a name="prerequisites"></a>Edellytykset

- [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Uusimman Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 tai uudempi)
- [Azure PowerShell] (https://github.com/Azure/azure-powershell#installation)
- Access Web-sivusto, kun upotetun Designerilla

### <a name="install-visual-studio-tools-for-logic-apps"></a>Visual Studio tools for logiikan-sovellusten asentaminen

Kun olet asentanut, edellytykset 

1. Avaa Visual Studio 2015 **Työkalut** ja valitse **tunnisteet ja päivitykset**
1. Etsi Internetistä **Online** -luokka
1. Etsi **Logiikan sovellukset** näyttämään **Azure logiikan sovellusten Tools for Visual Studio**
1. Valitse Lataa ja asenna laajennus **Lataa** -painike
1. Käynnistä Visual Studio uudelleen asennuksen jälkeen

> [AZURE.NOTE] Voit ladata tunniste [linkistä suoraan](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9)

Kerran asennettu osaat logiikan App Designerin Azure resurssiryhmä-Projectin avulla.

## <a name="create-a-project"></a>Projektin luominen

1. Valitse **Tiedosto** ja valitse **Uusi** >  **projektin** (tai voivat **Lisää** ja valitse sitten **Uusi projekti** lisääminen aiemmin luotuun ratkaisu):  ![tiedosto-valikko](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. -Valintaikkunassa Etsi **Cloud**ja valitse sitten **Azure resurssiryhmä**. Kirjoita **nimi** ja valitse sitten **OK**.
    ![Lisää uusi projekti](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. Valitse **logiikan sovelluksen** -malli. Tämä vaihtoehto Luo aluksi tyhjä logiikan sovelluksen käyttöönotto-malli.
    ![Azure-malli](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Kun olet valinnut **mallin**, valitse **OK**.

    Nyt logiikan app projektin lisätään ratkaisu. Pitäisi näkyä Solution Explorerissa käyttöönotto-tiedosto:  

    ![Käyttöönotto](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>Logiikan App Designerilla

Kun Azure resurssiryhmä-projekti, joka sisältää logiikan-sovelluksen, voit avata Visual Studion, voit helpottaa luominen työnkulun suunnittelussa.  Suunnittelija edellyttää internet-yhteyden kyselyn yhdistimet käytettävissä olevat ominaisuudet ja tiedot (esimerkiksi jos Dynamics CRM Online Connectorin avulla suunnittelija kyselyn CRM-esiintymässä käytettävissä Mukautettu ja oletusominaisuudet luettelon).

1. Napsauta hiiren kakkospainikkeella `<template>.json` tiedosto ja valitse **Avaa logiikan App Designerin** (tai `Ctrl+L`)
1. Tilauksen, resurssiryhmä ja käyttöönoton mallin sijainnin valitseminen
    - On tärkeää muistaa, että suunnittelet logiikan app Luo **API yhteyden** resurssien kyselyn ominaisuudet suunnittelun aikana.  Resurssiryhmä valittu on käytettävä yhteyksien luominen suunnittelun aikana resurssiryhmä.  Voit tarkasteleminen tai muokkaaminen API mahdolliset yhteydet Azure-portaalissa siirtymällä ja selaamisen **API yhteydet**.
    ![Tilauksen valitsin](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. Suunnittelija on hahmonnetaan määritelmän perusteella `<template>.json` tiedosto.
1. Voit nyt luoda ja suunnitella logiikan sovelluksen ja muutokset päivittyvät käyttöönotto-mallissa.
    ![Visual Studio suunnittelu](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

Näet myös `Microsoft.Web/connections` mahdolliset yhteydet, jotka tarvitaan funktion logiikan sovelluksen resurssi-tiedostosi lisätään resursseja.  Nämä ominaisuudet voit määrittää, kun otat käyttöön ja hallitaan jälkeen Azure-portaalissa **API yhteydet** otetaan käyttöön.

### <a name="switching-to-the-json-code-view"></a>JSON-koodinäkymän vaihtaminen

Voit valita suunnittelun, siirry logiikan sovelluksen JSON esittäminen alareunassa **Koodinäkymän** -välilehti.  Voit siirtyä takaisin koko resurssin JSON, napsauta hiiren kakkospainikkeella `<template>.json` tiedosto ja valitse **Avaa**.

### <a name="saving-the-logic-app"></a>Logiikan sovelluksen tallentaminen

Voit tallentaa logiikan sovelluksen **Tallenna** -painiketta milloin tahansa kautta tai `Ctrl+S`.  Jos tallennat aikaa on virheitä logiikan-sovelluksessa, ne näy Visual Studio **tulostus** -ikkunaan.

## <a name="deploying-your-logic-app"></a>Logiikan-sovelluksen ottaminen käyttöön

Lopuksi sen jälkeen, kun olet määrittänyt sovelluksen, voit ottaa suoraan Visual Studio vain muutama vaiheet. 

1. Projektin Solution Explorerissa hiiren kakkospainikkeella ja valitse **Ota käyttöön** > **Uudet käyttöönoton...** 
     ![Uusi käyttöönotto](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Sinua kehotetaan kirjautumaan Azure tilauksistasi. 

3. Nyt tarvitset Valitse resurssiryhmä, johon haluat ottaa käyttöön sovelluksen logiikan tiedot. 
    ![Ottaa käyttöön resurssiryhmä](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Muista valita oikean mallin ja parametrit tiedostot resurssiryhmä (esimerkiksi jos otat tuotantoympäristössä kannattaa valita tuotannon Parametrit-tiedosto). 
4. Valitse Ota käyttöön-painike
 
    
6. Käyttöönoton tilan tulee **kohde** -ikkunassa (voit joutua valitsemaan **Azure valmistelu**. 
    ![Tulos](./media/app-service-logic-deploy-from-vs/output.png)

Myöhemmin voit tarkistaa logiikan tietolähteen ohjausobjektin sovelluksen ja käyttöön Visual Studiossa uusia versioita. 

> [AZURE.NOTE] Jos muokkaat Azure-portaalissa määritelmän suoraan, kun seuraavan kerran, voit ottaa Visual Studio muutokset korvataan.

## <a name="next-steps"></a>Seuraavat vaiheet

- Aloita logiikan sovellukset, katso [logiikan sovelluksen luominen](app-service-logic-create-a-logic-app.md) -opetusohjelma.  
- [Näytä yleisiä esimerkkejä ja skenaariot](app-service-logic-examples-and-scenarios.md)
- [Voit automatisoida liiketoimintaprosesseja logiikan sovelluksilla](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Opettele järjestelmien integroida logiikan sovellukset](http://channel9.msdn.com/Events/Build/2016/P462)