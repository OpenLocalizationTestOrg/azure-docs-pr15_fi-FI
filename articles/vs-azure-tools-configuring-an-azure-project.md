<properties
   pageTitle="Määritä Azure Cloud palvelun projekti Visual Studiossa | Microsoft Azure"
   description="Opettele määrittämään Azure cloud-palvelun projektin Visual Studion projektin tarpeen mukaan."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-an-azure-cloud-service-project-with-visual-studio"></a>Määritä Azure Cloud palvelun projekti Visual Studiossa

Voit määrittää Azure cloud service-projektin projektin tarpeen mukaan. Voit määrittää seuraavat luokat-projektin ominaisuudet:

- **Julkaiseminen Azure pilvipalveluun**

  Voit määrittää, varmista, että aiemmin pilvipalvelussa, ottaa käyttöön Azure ei poisteta vahingossa ominaisuus.

- **Suorita tai paikallisessa tietokoneessa Pilvipalvelun korjaaminen**

  Voit valita palvelun määritykset, ja haluat aloittaa Azure tallennustilan emulaattori.

- **Vahvista cloud-palvelupakettiin, kun se on luotu**

  Voit päättää, Käsittele kaikki varoitukset virheinä, jotta voit varmistaa, että cloud-palvelupakettiin otetaan käyttöön ongelmitta. Tämä pienentää oman odotusaika, jos käyttöönotto ja tutustu tapahtui virhe.

Seuraavassa kuvassa näkyy valitseminen määritys, jota käytetään, kun suoritat tai virheenkorjaus pilvipalvelussa paikallisesti. Voit määrittää projektin ominaisuuksia, jotka edellyttävät tässä ikkunassa kuvassa esitetyllä tavalla.

![Määritä Microsoft Azure-projekti](./media/vs-azure-tools-configuring-an-azure-project/IC713462.png)

## <a name="to-configure-an-azure-cloud-service-project"></a>Azure cloud-palvelun projektin määrittäminen

1. Cloud palvelun projektin **Ratkaisunhallinnassa**, Avaa pikavalikko cloud palvelun projektin ja valitse sitten **Ominaisuudet**.

  Cloud palvelun projektin nimen sisältävä sivu tulee näkyviin Visual Studio-editorissa.

1. Valitse **kehitys** -välilehti.

1. Varmista, ettet vahingossa poista olemassa olevaan käyttöympäristöön Azure-tietokannassa, ennen kuin poistat aiemmin luodun käyttöönoton luettelon kehotteessa valitsemalla **Tosi**.

1. Valitse palvelun määritykset, jota haluat käyttää, kun suoritat tai virheenkorjaus pilvipalvelussa paikallisesti **Service configuration** -luettelosta valitsemalla palvelun määrityksiä.

  >[AZURE.NOTE] Jos haluat luoda palvelumäärityksen artikkelista miten: hallinta Service-määrityksiä ja profiilit. Jos haluat muokata kohteen rooli, katso, [miten voit määrittää Visual Studiossa Azure pilvipalvelussa roolit](vs-azure-tools-configure-roles-for-cloud-service.md).

1. Jos haluat aloittaa Azure-tallennustilan emulaattorin, kun suoritat tai virheenkorjaus pilvipalvelussa paikallisesti, **Käynnistä Azure-tallennustilan emulaattorin**Valitse **Tosi**.

1. Voit varmistaa, että et voi julkaista Jos **Käsittele varoitukset virheinä**paketin tarkistusvirheitä, valitse **Tosi**.

1. Varmista, että web-roolin käyttää samaa porttia aina, kun se käynnistyy paikallisesti IIS Expressin **käyttäminen web projektin portit**, valitse **Tosi**. Jos haluat käyttää tiettyä porttia tietyssä WWW-projektin, avaa web-projekti pikavalikko, **Ominaisuudet** -välilehti, **Web** -välilehti ja muuta porttinumero **IIS Express** -osassa **Project URL-osoite** -asetusta. Kirjoita esimerkiksi `http://localhost:14020` projektin URL-osoitteena.

1. Tallenna muutokset, jotka olet tehnyt cloud service-projektin ominaisuudet valitsemalla työkalurivillä **Tallenna** -painiketta.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure cloud palvelun projektien määrittämisestä Visual Studiossa, katso [määrittäminen Your Azure projektin käyttämällä useita palvelun määrityksiä](vs-azure-tools-multiple-services-project-configurations.md).
