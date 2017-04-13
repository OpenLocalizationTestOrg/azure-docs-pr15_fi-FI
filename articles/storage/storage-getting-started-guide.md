<properties
    pageTitle="Aloita Azuren tallennustilaan päättymässä | Microsoft Azure"
    description="Nopeasti päästä Microsoft Azure-BLOB, taulukon ja Azure tallennustilan nopeasti käynnistyy, Visual Studio ja Azure tallennustilan emulaattori olevien. Suorita ensimmäisen Azuren tallennustilaan-sovelluksen viisi minuuttia."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Aloita päättymässä Azuren tallennustilaan

## <a name="overview"></a>Yleiskatsaus

On helppoa Aloita kehittämisen Azure-tallennustilan. Tässä opetusohjelmassa näytetään, miten Azuren tallennustilaan-sovelluksen määrittäminen ja käytössä nopeasti. Käytät mukana Azure SDK-paketissa, .NET Pika-aloitus-mallia. Nämä nopeasti käynnistyy sisältää valmis Suorita tunnus, jolla esitellään joitakin basic ohjelmoinnin skenaarioita Azuren tallennustilaan kanssa.

Saat lisätietoja Azure-tallennustilan ennen Sukeltava koodiksi, katso [Osio seuraavat vaiheet](#next-steps).

## <a name="prerequisites"></a>Edellytykset

Tarvitset seuraavat vaatimukset, ennen kuin aloitat:

1. Voit kääntää ja luoda sovelluksen, sinun on [Visual Studio](https://www.visualstudio.com/) asennettu versio.

2. Asenna uusin versio [Azure SDK.NET](https://azure.microsoft.com/downloads/). SDK sisältää Azure pikaopas otoksen projekteja, Azure tallennustilan emulaattori ja [Azure tallennustilan asiakkaan kirjasto .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Varmista, että sinulla on asennettu tietokoneeseen, [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) edellyttää sitä, että voit käyttää tässä opetusohjelmassa Azure-pikaopas otoksen projektit.

    Jos et ole varma, mitä .NET Framework-versio on asennettu tietokoneeseen, katso [Toimintaohje: määrittää, jonka .NET Framework asennetaan](https://msdn.microsoft.com/vstudio/hh925568.aspx). Tai paina **Käynnistä** -painiketta tai Windows-näppäintä, kirjoita **Ohjauspaneeli**. Valitse **Ohjelmat** > **Ohjelmat ja toiminnot**, ja selvitä, onko .NET Framework 4.5 löytämäsi asennettujen ohjelmien kesken.

4. Sinun on Azure tilauksen ja Azure-tallennustilan tilin.

    - Pääset Azure tilauksen saat [Maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/)hankkiminen ja [Asetuksia](https://azure.microsoft.com/pricing/purchase-options/) [Jäsenen tarjoaa](https://azure.microsoft.com/pricing/member-offers/) (MSDN-, Microsoft Partner Network- ja BizSpark, ja muihin Microsoft-ohjelmiin jäsenet).
    - Azure-tallennustilan tilin luominen, katso, [miten voit luoda tallennustilan tilin](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Ensimmäinen Azuren tallennustilaan-sovelluksen käyttämiseen vastaan Azuren tallennustilaan pilveen

Kun olet määrittänyt tilin, voit luoda yksinkertaisen Azuren tallennustilaan-sovelluksesta valitsemalla yhden projektin otoksen Azure nopeasti käynnistää Visual Studiossa. Tässä opetusohjelmassa ohjeessa otoksen projektien Azuren tallennustilaan: **Azuren tallennustilaan: BLOB**, **Azuren tallennustilaan: tiedostojen**, **Azuren tallennustilaan: olevien**, ja **Azuren tallennustilaan: taulukoiden**:

1. Käynnistä Visual Studio.
2. Valitse **Tiedosto** -valikossa **Uusi projekti**.
3. Valitse **Uusi projekti** -valintaikkunan **asennetut** > **mallien** > **Visual C#** > **Cloud** > **QuickStarts** > **Tietopalvelut**.
    a. Valitse jokin seuraavista malleista: **Azuren tallennustilaan: BLOB**, **Azuren tallennustilaan: tiedostojen**, **Azuren tallennustilaan: olevien**, tai **Azuren tallennustilaan: taulukoiden**.
    b. Varmista, että **.NET Framework 4.5** on valittu kohde framework.
    - 3.c. Projektin nimi ja luoda uuden Visual Studio-ratkaisuun:

    ![Azure pika-aloituksen][Image1]

Voit tarkastella lähdekoodin ennen sovelluksen suorittamisen. Valitse tarkistettava koodin **Ratkaisunhallinnassa** Visual Studiossa **Näytä** -valikon. Kaksoisnapsauta sitten Program.cs-tiedosto.

Seuraava esimerkkisovelluksen käyttämiseen:

1.  Visual Studiossa Valitse **Näytä** -valikon **Ratkaisunhallinnassa** . Avaa Azure tallennustilan emulaattori App.config-tiedostoa ja ulos yhteysmerkkijonon kommentti:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Kommentointi yhteysmerkkijonoa Azure-tallennustilan palvelun ja anna tallennustilan tilin nimi ja käyttää avainta App.config-tiedostoa:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    Noutaa tallennustilan tilin käyttö key-tuotetunnuksen, on artikkelissa [Hallitse tallennustilan pikanäppäimet](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  Kun annat tallennustilan tilin nimi ja pikanäppäin App.config-tiedostoa, valitse **Tiedosto** -valikosta Tallenna valitsemalla **Tallenna kaikki** kaikki projektitiedostot.
4.  Valitse **Muodosta** **Luominen ratkaisu**.
5.  Valitse **Virheenkorjaus** -valikossa painamalla **F11** suorittamaan ratkaisu vaiheittainen tai painamalla **F5-näppäintä** voit suorittaa ratkaisu.


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Ensimmäinen Azuren tallennustilaan-sovelluksen käyttämiseen paikallisesti Azure-tallennustilan emulaattorin vastaan

[Azure-tallennustilan emulaattorin](storage-use-emulator.md) on paikallisessa ympäristössä, joka emuloi tarkoitettu Azure-Blob, jonossa ja taulukon palvelut. Voit käyttää tallennustilan emulaattori Testaa tallennustilan sovelluksen paikallisesti ilman Azure tilaus tai tallennustilan tilin luomista ja niin, ettei kustannukset.

Voit kokeilla sitä luodaan yksinkertainen Azuren tallennustilaan-sovelluksesta valitsemalla yhden projektin otoksen Azure nopeasti käynnistää Visual Studiossa. Tässä opetusohjelmassa ohjeessa on **Azure-Blob-säiliö**ja **Azure-taulukkotallennus** **Azure jonon tallennustilan** otoksen projekteja:

1. Käynnistä Visual Studio.
2. Valitse **Tiedosto** -valikossa **Uusi projekti**.
3. Valitse **Uusi projekti** -valintaikkunan **asennetut** > **mallien** > **Visual C#** > **Cloud** > **QuickStarts** > **Tietopalvelut**.
    a. Valitse jokin seuraavista malleista: **Azuren tallennustilaan: BLOB**, **Azuren tallennustilaan: tiedostojen**, **Azuren tallennustilaan: olevien**, tai **Azuren tallennustilaan: taulukoiden**.
    b. Varmista, että **.NET Framework 4.5** on valittu kohde framework.
    c-näppäinyhdistelmää. Projektin nimi ja luoda uuden Visual Studio-ratkaisuun:

    ![Azure pika-aloituksen][Image1]

4.  Visual Studiossa Valitse **Näytä** -valikon **Ratkaisunhallinnassa** . Avaa App.config-tiedostoa ja kommentin yhteysmerkkijonoa Azure-tallennustilan tilin, jos olet jo lisännyt jokin. Kommentointi sitten yhteysmerkkijonoa Azure tallennustilan emulaattori varten:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Voit tarkastella lähdekoodin ennen sovelluksen suorittamisen. Valitse tarkistettava koodin **Ratkaisunhallinnassa** Visual Studiossa **Näytä** -valikon. Kaksoisnapsauta sitten Program.cs-tiedosto.

Suorita seuraava-sovelluksen malli Azure-tallennustilan emulaattorin:

1.  Paina **Käynnistä** -painiketta tai Windows-näppäintä, Etsi *Microsoft Azuren tallennustilaan emulaattorin*ja Käynnistä sovellus. Emulaattori käynnistyessä, näet kuvake ja ilmoituksen Windows tehtävänäkymä-alueella.
2.  Visual Studiossa **Muodosta** -valikosta **Luo ratkaisu** .
3.  Valitse **Virheenkorjaus** -valikossa painamalla **F11** Suorita ratkaisu vaiheittainen tai painamalla **F5-näppäintä** voit suorittaa ratkaisun alusta loppuun.

## <a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja Azure-tallennustilan nämä resurssit:

* [Johdanto Microsoft Azuren tallennustilaan](storage-introduction.md)
* [Azure-tallennustilan Explorer käytön aloittaminen](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Pääset alkuun käyttämällä .NET Azure-Blob-säiliö](storage-dotnet-how-to-use-blobs.md)
* [Pääset alkuun käyttämällä .NET Azure-taulukkotallennus](storage-dotnet-how-to-use-tables.md)
* [Käyttämällä .NET Azure jonon tallennustilan käytön aloittaminen](storage-dotnet-how-to-use-queues.md)
* [Windows Azure-tiedostosäilön käytön aloittaminen](storage-dotnet-how-to-use-files.md)
* [Siirrä tiedot ja AzCopy komentorivivalitsimet-apuohjelma](storage-use-azcopy.md)
* [Azure-tallennustilan dokumentaatio](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azuren tallennustila asiakkaan kirjasto .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
