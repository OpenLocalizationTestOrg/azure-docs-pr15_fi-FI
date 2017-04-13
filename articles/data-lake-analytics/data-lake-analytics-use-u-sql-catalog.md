<properties
   pageTitle="Esittele Azure tietojen järvi Analytics U-SQL-luettelon | Azure"
   description="Esittele Azure tietojen järvi Analytics U-SQL-luettelo"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="use-u-sql-catalog"></a>Käytä U-SQL-luettelo

U-SQL-luettelon käytetään rakenteen tietoja ja -koodia, jotta ne voidaan jakaa U-SQL-komentosarjat. Luettelon mahdollistaa tietojen Azure tietojen järvi mahdollisuuksiin suurin suorituskyvyn.

Azure tietojen järvi Analytics-tileille on liitetty täsmälleen yhden U-SQL-luettelon. Et voi poistaa U-SQL-luettelon. Tällä hetkellä U-SQL-luetteloita ei voi jakaa järvi tietovaraston tilien välillä.

Kunkin U-SQL-luettelo sisältää tietokanta nimeltä **perustyyli**. Perustyyli-tietokantaan ei voi poistaa.  Kunkin U-SQL-luettelon voi olla useita muut tietokannat.

U-SQL-tietokanta sisältää:

- Kokoonpanojen – jakaa .NET-koodin U-SQL-komentosarjat kesken.
- Taulukon arvojen Funktiot – Jaa U-SQL-koodin U-SQL-komentosarjat kesken.
- Taulukot – jaa tietoja U-SQL-komentosarjat kesken.
- Mallit - jakaa taulukkorakenteita U-SQL-komentosarjat kesken.

## <a name="manage-catalogs"></a>Luetteloiden hallinta
Azure tietojen järvi Analytics-tileille on liittyy oletustilin Azure Lake Tietosäilölle. Tietosäilö järvi tilin kutsutaan järvi tietovaraston oletustiliksi. U-SQL-luettelo tallennetaan järvi tietovaraston oletustilin /catalog-kansiossa. Älä poista mitään /catalog-kansiossa olevat tiedostot.

### <a name="use-azure-portal"></a>Azure-portaalin käyttäminen

[Hallitse tietojen järvi Analytics](data-lake-analytics-manage-use-portal.md#view-u-sql-catalog) ohjeaiheessa portal


### <a name="use-data-lake-tools-for-visual-studio"></a>Käytä järvi Datatyökalut Visual Studio.

Voit hallita luettelon järvi Datatyökalut Visual Studio.  Saat lisätietoja työkalujen [Käyttäminen tietojen järvi Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

**Voit hallita luettelon**

1. Avaa Visual Studio ja azure yhdistäminen. Lisätietoja on artikkelissa [etäyhteyden muodostaminen Azure](data-lake-analytics-data-lake-tools-get-started.md#connect-to-azure).
1. Avaa **Palvelimeen Explorer** painamalla **CTRL + ALT + S**.
2. **Palvelimen Explorer**Laajenna **Azure**, laajenna **Tietojen järvi Analytics**, laajenna tietojen järvi Analytics-tilisi, laajenna **Databases**ja laajenna sitten **perustyyli**.



    - Jos haluat lisätä uuden tietokannan, **tietokannan**hiiren kakkospainikkeella ja valitse sitten **Luo tietokanta**.
    - Jos haluat lisätä uuden kokoonpanon, **kokoonpanot**hiiren kakkospainikkeella ja valitse sitten **Rekisteröi kokoonpanon**.
    - Jos haluat lisätä uuden rakenteen, **rakenteita**hiiren kakkospainikkeella ja valitse sitten "Luo rakenteen **.
    - Jos haluat lisätä uuden taulukon, napsauta **taulukot**ja valitse "" Luo taulukon **.
    - Uudesta taulukkoarvoisia-funktiosta on kohdassa [kehittää U-SQL käyttäjän määrittämät tiedot järvi Analytics työt operaattorit](data-lake-analytics-u-sql-develop-user-defined-operators.md).


![Selaa U SQL Visual Studio luettelot](./media/data-lake-analytics-use-u-sql-catalog/data-lake-analytics-browse-catalogs.png)


## <a name="see-also"></a>Katso myös

- Käytön aloittaminen
    - [Aloita tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-get-started-portal.md)
    - [Aloita tietojen järvi Analytics Azure PowerShellin avulla](data-lake-analytics-get-started-powershell.md)
    - [Aloita tietojen järvi Analytics Azure .NET SDK: N avulla](data-lake-analytics-get-started-net-sdk.md)
    - [U-SQL-komentosarjojen käyttämällä järvi Datatyökalut Visual Studio kehittäminen](data-lake-analytics-data-lake-tools-get-started.md)
    - [Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md)

- U-SQL- ja kehitystyö
    - [Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md)
    - [U-SQL-ikkunassa funktioilla Azure tietojen järvi Analytics töiden](data-lake-analytics-use-window-functions.md)
    - [Kehittää tietojen järvi Analytics työt U SQL käyttäjän määrittämät operaattorit](data-lake-analytics-u-sql-develop-user-defined-operators.md)

- Hallinta
    - [Hallitse Azure tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-manage-use-portal.md)
    - [Hallitse Azure tietojen järvi Analytics Azure PowerShellin avulla](data-lake-analytics-manage-use-powershell.md)
    - [Valvo ja Azure tietojen järvi Analytics työt Azure-portaalissa vianmääritys](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

- Lopusta loppuun-opetusohjelma
    - [Käytä Azure tietojen järvi Analytics vuorovaikutteiset opetusohjelmat](data-lake-analytics-use-interactive-tutorials.md)
    - [Analysoi sivuston lokit Azure tietojen järvi analyysin avulla](data-lake-analytics-analyze-weblogs.md)
