<properties 
   pageTitle="Lue tietoja järvi analyysin ja U-SQL Azure Portal vuorovaikutteiset opetusohjelmat avulla | Azure" 
   description="Pika-aloitusopas ja liittyviä tietoja järvi Analytics- ja U-SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Käytä Azure tietojen järvi Analytics vuorovaikutteiset opetusohjelmat

Azure-portaalissa on vuorovaikutteinen opetusohjelma puolestasi tietojen järvi Analytics käytön aloittaminen. Tässä artikkelissa kerrotaan, miten käsitellään opetusohjelman analysointiin sivuston lokitiedot.


>[AZURE.NOTE]Jos haluat käydä läpi saman opetusohjelman Visual Studiossa, katso [Analysoi sivuston lokit käyttäminen tietojen järvi Analytics](data-lake-analytics-analyze-weblogs.md).
>Lisää vuorovaikutteiset opetusohjelmat portaalin lisättäväksi.


Muut opetusohjelmat seuraavissa artikkeleissa:

- [Aloita tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-get-started-portal.md)
- [Aloita tietojen järvi Analytics Azure PowerShellin avulla](data-lake-analytics-get-started-powershell.md)
- [Käyttämällä .NET SDK tietojen järvi Analytics käytön aloittaminen](data-lake-analytics-get-started-net-sdk.md)
- [U-SQL-komentosarjojen käyttämällä järvi Datatyökalut Visual Studio kehittäminen](data-lake-analytics-data-lake-tools-get-started.md) 

**Edellytykset**

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **A tietojen järvi Analytics-tili**.  Katso [Azure tietojen järvi Analytics Azure-portaalissa käytön aloittaminen](data-lake-analytics-get-started-portal.md).

##<a name="create-data-lake-analytics-account"></a>Tietoja järvi Analytics-tilin luominen 

Sinulla on oltava tietojen järvi Analytics-tili, ennen kuin voit suorittaa töitä.

Tietoja järvi Analytics-tileille on [Azure järvi tietosäilö](../data-lake-store/data-lake-store-overview.md) -tilin riippuvuuden.  Tämän tilin kutsutaan järvi tietovaraston oletustiliksi.  Voit luoda järvi tietovaraston tilin etukäteen, tai kun luot tietojen järvi Analytics-tilillesi. Tässä opetusohjelmassa luodaan järvi tietovaraston tilin Analytics-tilin kanssa

**Tietoja järvi Analytics-tilin luominen**

1. Kirjaudu [Azure Portal](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute).
2. Napsauta **Microsoft Azure** StartBoard Avaa vasemmassa yläkulmassa.
3. Napsauta **Marketplace** -ruutua.  
3. Kirjoita **Azure tietojen järvi Analytics** hakuruutuun **kaikki sisältö** -sivu ja paina **ENTER**-näppäintä. On artikkelissa **Azure tietojen järvi Analytics** -luettelossa.
4. Valitse **Azure tietojen järvi Analytics** -luettelosta.
5. Valitse **Luo** siitä sivu.
6. Kirjoita tai valitse seuraavasti:

    ![Portaalin Azure tietojen järvi Analytics-sivu](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Nimi**: Analytics-tilin nimi.
    - **Tietosäilö Lake**: kunkin tietojen järvi Analytics tilillä on riippuvainen järvi tietosäilö-tilin. Tietoja järvi Analytics-tili ja riippuvainen järvi tietosäilö on sijaittava saman Azure tietokeskuksen. Uuden järvi tietovaraston asiakkaan noudattamalla tai valitse olemassa.
    - **Tilaus**: Valitse Analytics-tilissä Azure tilaus.
    - **Resurssiryhmä**. Valitse Azure-resurssiryhmä tai luoda uuden. Sovellusten yleensä tehty useita osia, kuten verkkosovellukseen, tietokannan, tietokantapalvelimeen, tallennustilan ja 3 osapuolen palvelujen. Azure resurssien hallinta (ARM) mahdollistaa ryhmänä Azure-resurssiryhmä nimitystä sovelluksen resurssien käsitteleminen. Voit käyttöön, Päivitä, valvoa tai poistaa kaikkien resurssien sovelluksen yhteen, koordinoidun toiminnossa. Mallin käyttäminen käyttöönottoa varten ja mallin toimii eri ympäristöissä, kuten testauksen testaus- ja. Voit selventää Laskutus organisaation tarkastelemalla koko ryhmän kootut kustannuksiin. Lisätietoja on artikkelissa [Azure Resurssienhallinta yleiskatsaus](azure-resource-manager/resource-group-overview.md). 
    - **Sijainti**. Valitse Azure tietokeskuksen tietojen järvi Analytics-tilille. 
7. Valitse **Kiinnitä Startboard**. Tämä on pakollinen tämä opetusohjelma.
8. Valitse **Luo**. Se avaa portaalin StartBoard. Uusi ruutu lisätään aloitus-sivulla näkyy "Käyttöönotto Azure tietojen järvi Analytics" otsikko. Kestää jonkin aikaa tietojen järvi Analytics-tilin luominen. Kun tili on luotu, portaalin avautuu uusi sivu-tilin.

    ![Portaalin Azure tietojen järvi Analytics-sivu](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>Suorita sivuston Log analyysi vuorovaikutteinen opetusohjelma

**Avaa sivuston Log Analytics vuorovaikutteinen opetusohjelma**

1. Napsauta **Microsoft Azure** -portaalissa vasemmasta valikosta Avaa StartBoard.
2. Napsauta ruutua, joka on linkitetty tietojen järvi Analytics-tiliisi.
3. Valitse **Selaa vuorovaikutteiset opetusohjelmat** **Essentials** -palkista.

    ![Tietoja järvi Analytics vuorovaikutteiset opetusohjelmat](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Jos näet oranssi varoitus-ilmoitus "esimerkit ei ole määritetty, valitse...", valitse **Kopioi mallitiedot** kopioi esimerkkitiedot järvi tietovaraston oletustilin. Vuorovaikutteinen opetusohjelma tarvitsee suorittaa tietoja.
5. Napsauta **Sivuston Log Analytics** **Vuorovaikutteiset opetusohjelmat** -sivu. Portaalin avautuu opetusohjelman uuden portaalin sivu.
5. Valitse **1 johdanto** ja noudata näyttöön tulevia ohjeita

##<a name="see-also"></a>Katso myös

- [Microsoft Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md)
- [Aloita tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-get-started-portal.md)
- [Aloita tietojen järvi Analytics Azure PowerShellin avulla](data-lake-analytics-get-started-powershell.md)
- [U-SQL-komentosarjojen käyttämällä järvi Datatyökalut Visual Studio kehittäminen](data-lake-analytics-data-lake-tools-get-started.md)
- [Analysoi sivuston lokit Azure tietojen järvi analyysin avulla](data-lake-analytics-analyze-weblogs.md)
