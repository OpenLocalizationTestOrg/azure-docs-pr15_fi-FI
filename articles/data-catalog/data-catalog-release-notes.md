<properties
   pageTitle="Azure tietoluettelon julkaisutiedot | Microsoft Azure"
   description="Azure tietoluettelon julkaisutiedot."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-release-notes"></a>Azure tietoluettelon julkaisutiedot

## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Muistiinpanojen 20 marraskuun 2015 julkaistut Vapauta Azure tietoluettelon.

### <a name="opening-data-sources-in-power-bi-desktop"></a>Power BI Desktopin avaaminen tietolähteet

**Azure tietoluettelon** -portaalista "Avaa-Power BI Desktopin"-asetuksen avulla käyttäjät voivat kohdata jokin kaksi ongelmista Power BI Desktopin-sovelluksessa:

- Näkyviin tulee valintaikkuna, jossa otsikko "Ei onnistu, Avaa tiedosto"
- Power BI Desktopin-sovellus avautuu, mutta tiedosto näkyy tyhjänä

Kunkin tilanteen ongelma voidaan ratkaista lataamalla ja asentamalla uusimman Power BI Desktopin [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Muistiinpanojen 13 marraskuun 2015 julkaistut Vapauta Azure tietoluettelon.

### <a name="registering-and-connecting-to-teradata"></a>Rekisteröiminen ja yhteyden muodostaminen Teradata

Jos yhteyden muodostaminen Teradata tietolähteisiin käyttäjät on oltava asennettuna oikean Teradata ODBC-ohjaimen, joka vastaa käytössä ohjelmiston bittimäärää (32-bittinen tai 64-bittinen).

Tämän ADC Julkaisupäivä saavuttaman uusimmat [windows (versio 15.10) Teradata ODBC-ohjain](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) on yhteensopiva Office 2013: n kanssa, mutta ei Office 2016 kanssa.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Muistiinpanojen 13 heinäkuussa 2015 julkaistut Vapauta Azure tietoluettelon.

### <a name="registering-and-connecting-to-oracle-database"></a>Rekisteröiminen ja yhteyden muodostaminen Oracle-tietokantaan

Jos yhteyden muodostaminen Oracle-tietokantaan tietolähteiden käyttäjät on oltava asennettuna oikeat Oracle-ohjaimet, jotka vastaavat millä ohjelmalla bittimäärää (32-bittinen tai 64-bittinen).

-   Kun rekisteröidään Oracle tietolähteiden 32-bittinen Windows-tietokoneessa, 32-bittiset Oracle-ohjaimet käytetään
-   Kun rekisteröidään Oracle tietolähteiden 64-bittinen Windows-tietokoneessa, käytetään 64-bittinen Oracle-ohjaimet
-   Muodostettaessa yhteyttä Oracle tietolähteiden Excel tietokoneessa, jossa Microsoft Officen 32-bittinen versio 64-bittisessä Windowsissa, kuten 32-bittiset Oracle-ohjaimet käytetään
-   Muodostettaessa yhteyttä Oracle tietolähteiden Excel tietokoneessa, jossa Microsoft Officen 64-bittinen versio 64-bittinen Oracle-ohjaimet käytetään

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Rekisteröiminen ja yhteyden muodostaminen SQL Server Reporting Services

SQL Server Reporting Services (SSRS) tietolähteiden tuki on tällä hetkellä rajoitettu alkuperäistilassa-palvelimiin. SharePoint-tilassa SSRS tuki lisätään myöhemmällä.

### <a name="opening-data-assets-in-excel"></a>Avaava tietojen varat Excelissä

Kun avaat Microsoft Excelin tietojen kalusto **Azure tietoluettelon** -portaalista, käyttäjät voidaan pyytää **Microsoft Excel-tietoturvailmoitus** -valintaikkuna. Tämä on vakio, toiminta ja käyttäjien valita Jatka **käyttöön** .

Lisätietoja on artikkelissa [ottaminen käyttöön tai poistaminen käytöstä linkkejä ja tiedostoja koskevien tietoturvavaroitusten epäilyttävien sivustojen](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Välityspalvelimen ja käytännön määritys ja tietojen lähde rekisteröinti

Käyttäjät voivat kohdata tilanteeseen, jossa hän kirjautua sisään Azure tietoluettelon-portaaliin, mutta yrittäessään heidän havaitsemistaan virhesanoma, joka estää niiden kirjautuminen tietojen lähteen rekisteröinti työkalun kirjautua.

Kaksi syytä mahdollisen ongelman tämän ongelman:

**Syy 1: Active Directory Federation Services määritys** Tietojen lähteen rekisteröinti työkalu käyttää todennusmenetelmien vahvistamiseen käyttäjän kirjautumiset Active Directorysta. Onnistuneen kirjautumisen todennusmenetelmien on otettava käyttöön yleisen todennus-käytännön Active Directory-järjestelmänvalvoja.

Joissakin tilanteissa virhe Tämä ongelma voi ilmetä vain, kun käyttäjä on yrityksen verkossa tai vain silloin, kun käyttäjä muodostaa yhteyden ulkopuolella yrityksen verkkoon. Yleinen todennus-käytännön avulla todennusmenetelmien käyttöön erikseen intranetissä ja ekstranet yhteydet. Kirjautuminen virheitä saattaa ilmetä, jos todennusmenetelmien ei ole käytössä, joista käyttäjä muodostaa verkon.

Lisätietoja on artikkelissa [Käyttöoikeuksien käytäntöjen määrittäminen](https://technet.microsoft.com/library/dn486781.aspx).

**Syy 2: verkon välityspalvelimen määrittäminen** Jos yrityksen verkkoon käyttää välityspalvelinta, rekisteröinti-työkalu ei ehkä yhdistäminen Azure Active Directory välityspalvelimen kautta. Käyttäjät voivat varmistaa, että rekisteröinti-työkalun muokkaamalla työkalun kokoonpanotiedosto tämän osan lisääminen tiedostoon:


      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Etsi RegistrationTool.exe.config-tiedosto, Käynnistä rekisteröinti-työkalua ja avaa sitten Tehtävienhallinta-apuohjelma. Valitse Tehtävienhallinta tiedot-välilehden RegistrationTool.exe Napsauta hiiren kakkospainikkeella ja valitse Avaa tiedostosijainti ponnahdusvalikosta.
