<properties
   pageTitle="Azure tietoluettelon tuetut tietolähteet | Microsoft Azure"
   description="Tuetut tietolähteet määritys."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-supported-data-sources"></a>Azure tietoluettelon tuetut tietolähteet

Azure-tietoluettelon, käyttäjät voivat julkaista julkisen napsauttamalla Ohjelmointirajapinnan käyttäminen metatiedot-kerran rekisteröinti korjaustyökalulla tai kirjoittamalla tiedot manuaalisesti siirtyä tietoluettelon web-portaalissa. Seuraavassa ruudukossa on yhteenveto tukemat luettelo hetkellä ja julkaisun ominaisuudet kunkin kaikki lähteet.  Myös lueteltu on ulkoiset tiedot-työkaluja, joilla kunkin tietolähteen voit käynnistää Microsoftin portal-Avaa-kohdassa"-toiminto. Toisessa ruudukossa on artikkelissa on lisätietoja määrittely kunkin tietojen lähteitä yhteyden ominaisuudet.


## <a name="list-of-supported-data-sources"></a>Tuettujen tietolähteiden luettelo

<table>

    <tr>
       <td><b>Tietolähdeobjekti</b></td>
       <td><b>OHJELMOINTIRAJAPINTA</b></td>
       <td><b>Manuaalinen merkintä</b></td>
       <td><b>Rekisteröity-työkalu</b></td>
       <td><b>Avaa Työkalut</b></td>
       <td><b>Huomautuksia</b></td>
    </tr>

    <tr>
      <td>Azure järvi tietovaraston hakemisto</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure järvi tietovaraston tiedosto</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tallennustilan Azure-Blob</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure-tallennustilan hakemisto</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure-tallennustilan taulukko</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>HDFS-kansio</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HDFS-tiedosto</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Taulukko rakenne</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Rakenne-näkymä</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-taulukossa</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-näkymä</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle-tietokantataulukko</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle tietokantanäkymä</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Muut (yleinen resurssi)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Varaston SQL arvotaulukko</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Varaston SQL tietonäkymä</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-dimensio</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services mitta</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-taulukko</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Reporting Services-raportti</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Selain</font></td>
      <td><font size=2>Vain alkuperäistilassa-palvelimiin. SharePoint-tilassa ei tueta.</font></td>
    </tr>

    <tr>
      <td>SQL Server-taulukko</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server-näkymä</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel-PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata-taulukossa</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata-näkymä</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP: in Hana näkymä</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>Laskenta ja analyyttisten näkymiä. Määritteen näkymiä ei tueta.</font></td>
    </tr>

    <tr>
      <td>Db2-taulukkoon</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Db2-näkymä</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tiedostojärjestelmän tiedostoon</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-hakemisto</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-tiedosto</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-raportti</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP loppupisteen merkki</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-tiedosto</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-kohteen määrittäminen</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-funktio</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Postgresql-taulukossa</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Postgresql-näkymä</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP: in Hana näkymä</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Salesforce-objekti</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SharePoint-luetteloon </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Jos tarvitset tukea muita lähteitä, Lähetä ominaisuuspyyntö käyttämällä [Azure tietoluettelon keskustelupalstalle](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Tietojen lähteen viittaus määritys
> [AZURE.NOTE] "DSL rakenteen-sarakkeen seuraavassa taulukossa vain luettelot yhteyden ominaisuuksia"address"ominaisuuden kantolaukku, jotka käyttävät Azure tietoluettelon (eli"address"ominaisuuden kantolaukku voi olla muiden yhteyden ominaisuudet, jotka Azure tietoluettelon toistuu, mutta ei käytä tietolähteen.)
<table>
    <tr>
       <td><b>Lähdetyyppi</b></td>
       <td><b>Kohteen tyyppi</b></td>
       <td><b>Objektin tyypit</b></td>
       <td><b>DSL rakenne<b></td>
    </tr>
    <tr>
      <td>Azure järvi tietosäilö</td>
      <td>Säilö</td>
      <td>Tietoja järvi</td>
      <td>
        <font size=2>protokolla: webhdfs <br>todentaminen: {basic, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Azure järvi tietosäilö</td>
      <td>Taulukko</td>
      <td>Hakemisto-tiedosto</td>
      <td>
        <font size=2>protokolla: webhdfs <br>todentaminen: {basic, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Azure-tallennustilan</td>
      <td>Säilö</td>
      <td>Säilö</td>
      <td>
        <font size=2>protokolla: azure-BLOB-objektit <br>todentaminen: {azure-pikanäppäin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;toimialueen <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tilin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;säilö</font>
      </td>
    </tr>
    <tr>
      <td>Azure-tallennustilan</td>
      <td>Taulukko</td>
      <td>BLOB-kansio</td>
      <td>
        <font size=2>protokolla: azure-BLOB-objektit <br>todentaminen: {azure-pikanäppäin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;toimialueen <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tilin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;säilö <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nimi</font>
      </td>
    </tr>
    <tr>
      <td>Azure-tallennustilan</td>
      <td>Säilö</td>
      <td>Säilö</td>
      <td>
        <font size=2>protokolla: azure-taulukot <br>todentaminen: {azure-pikanäppäin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;toimialueen <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tilin</font>
      </td>
    </tr>
    <tr>
      <td>Azure-tallennustilan</td>
      <td>Taulukko</td>
      <td>Taulukko</td>
      <td>
        <font size=2>protokolla: azure-taulukot <br>todentaminen: {azure-pikanäppäin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;toimialueen <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tilin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nimi</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Säilö</td>
      <td>Virtuaalinen klusterin</td>
      <td>
        <font size=2>protokolla: cosmos <br>todentaminen: {basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Taulukko</td>
      <td>Virta-joukkoon virtaa näkymä</td>
      <td>
        <font size=2>protokolla: cosmos <br>todentaminen: {basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Säilö</td>
      <td>Sivuston</td>
      <td>
        <font size=2>protokolla: http <br>todentaminen: {ei mitään, basic, windows, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Raportti</td>
      <td>Raportti-koontinäyttö</td>
      <td>
        <font size=2>protokolla: http <br>todentaminen: {ei mitään, basic, windows, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Säilö</td>
      <td>Tietokannan</td>
      <td>
        <font size=2>protokolla: db2 <br>todentaminen: {basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Taulukko</td>
      <td>Taulukon, näkymän</td>
      <td>
        <font size=2>protokolla: db2 <br>todentaminen: {basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne</font>
      </td>
    </tr>
    <tr>
      <td>Tiedostojärjestelmässä</td>
      <td>Taulukko</td>
      <td>Tiedoston</td>
      <td>
        <font size=2>protokolla: tiedosto <br>todentaminen: {ei mitään, basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;polku</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Taulukko</td>
      <td>Hakemisto-tiedosto</td>
      <td>
        <font size=2>protokolla: ftp <br>todentaminen: {ei mitään, basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Säilö</td>
      <td>Klusterin</td>
      <td>
        <font size=2>protokolla: webhdfs <br>todentaminen: {basic, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Taulukko</td>
      <td>Hakemisto-tiedosto</td>
      <td>
        <font size=2>protokolla: webhdfs <br>todentaminen: {basic, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Rakenne</td>
      <td>Säilö</td>
      <td>Tietokannan</td>
      <td>
        <font size=2>protokolla: rakenne <br>todentaminen: {hdinsight, perus-käyttäjänimi ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Rakenne</td>
      <td>Taulukko</td>
      <td>Taulukon, näkymän</td>
      <td>
        <font size=2>protokolla: rakenne <br>todentaminen: {hdinsight, perus-käyttäjänimi ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Säilö</td>
      <td>Sivuston</td>
      <td>
        <font size=2>protokolla: http <br>todentaminen: {ei mitään, basic, windows, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Raportti</td>
      <td>Raportti-koontinäyttö</td>
      <td>
        <font size=2>protokolla: http <br>todentaminen: {ei mitään, basic, windows, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Taulukko</td>
      <td>Loppupisteen merkki-tiedosto</td>
      <td>
        <font size=2>protokolla: http <br>todentaminen: {ei mitään, basic, windows, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Säilö</td>
      <td>Tietokannan</td>
      <td>
        <font size=2>protokolla: mysql <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Taulukko</td>
      <td>Taulukon, näkymän</td>
      <td>
        <font size=2>protokolla: mysql <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Säilö</td>
      <td>Kohteen säilö</td>
      <td>
        <font size=2>protokolla: odata <br>todentaminen: {ei mitään, basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Taulukko</td>
      <td>Kohteen määrittäminen-funktio</td>
      <td>
        <font size=2>protokolla: odata <br>todentaminen: {ei mitään, basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;resurssi</font>
      </td>
    </tr>
    <tr>
      <td>Oracle-tietokantaan</td>
      <td>Säilö</td>
      <td>Tietokannan</td>
      <td>
        <font size=2>protokolla: oracle <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan</font>
      </td>
    </tr>
    <tr>
      <td>Oracle-tietokantaan</td>
      <td>Taulukko</td>
      <td>Taulukon, näkymän</td>
      <td>
        <font size=2>protokolla: oracle <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Säilö</td>
      <td>Tietokannan</td>
      <td>
        <font size=2>protokolla: postgresql <br>todentaminen: {basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Taulukko</td>
      <td>Taulukon, näkymän</td>
      <td>
        <font size=2>protokolla: postgresql <br>todentaminen: {basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Säilö</td>
      <td>Sivuston</td>
      <td>
        <font size=2>protokolla: http <br>todentaminen: {ei mitään, basic, windows, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Raportti</td>
      <td>Raportti-koontinäyttö</td>
      <td>
        <font size=2>protokolla: http <br>todentaminen: {ei mitään, basic, windows, oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Taulukko</td>
      <td>Tietoja koosteen</td>
      <td>
        <font size=2>protokolla: power Queryssä <br>todentaminen: {oauth} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Taulukko</td>
      <td>Objektin</td>
      <td>
        <font size=2>protokolla: salesforce-com <br>todentaminen: {basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;luokan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemName</font>
      </td>
    </tr>
    <tr>
      <td>SAP-Hana</td>
      <td>Säilö</td>
      <td>Palvelin</td>
      <td>
        <font size=2>protokolla: sap-hana-sql <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin</font>
      </td>
    </tr>
    <tr>
      <td>SAP-Hana</td>
      <td>Taulukko</td>
      <td>Näytä</td>
      <td>
        <font size=2>protokolla: sap-hana-sql <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Taulukko</td>
      <td>Luettelo</td>
      <td>
        <font size=2>protokolla: sharepoint-luettelo <br>todentaminen: {basic, windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite</font>
      </td>
    </tr>
    <tr>
      <td>SQL-tietovarasto</td>
      <td>Komento</td>
      <td>Tallennettu toimintosarja</td>
      <td>
        <font size=2>protokolla: tkj <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>SQL-tietovarasto</td>
      <td>TableValuedFunction</td>
      <td>Taulukkoarvoisen funktion</td>
      <td>
        <font size=2>protokolla: tkj <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>SQL-tietovarasto</td>
      <td>Säilö</td>
      <td>Tietokannan</td>
      <td>
        <font size=2>protokolla: tkj <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan</font>
      </td>
    </tr>
    <tr>
      <td>SQL-tietovarasto</td>
      <td>Taulukko</td>
      <td>Taulukon, näkymän</td>
      <td>
        <font size=2>protokolla: tkj <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Komento</td>
      <td>Tallennettu toimintosarja</td>
      <td>
        <font size=2>protokolla: tkj <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Taulukkoarvoisen funktion</td>
      <td>
        <font size=2>protokolla: tkj <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Säilö</td>
      <td>Tietokannan</td>
      <td>
        <font size=2>protokolla: tkj <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Taulukko</td>
      <td>Taulukon, näkymän</td>
      <td>
        <font size=2>protokolla: tkj <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rakenne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Servicesin monidimensionaalisen</td>
      <td>Säilö</td>
      <td>Malli</td>
      <td>
        <font size=2>protokolla: analysis Servicesistä <br>todentaminen: {windows, basic, anonyymi, ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Servicesin monidimensionaalisen</td>
      <td>KPI:</td>
      <td>KPI:</td>
      <td>
        <font size=2>protokolla: analysis Servicesistä <br>todentaminen: {windows, basic, anonyymi, ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektilaji: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Servicesin monidimensionaalisen</td>
      <td>Toimenpide</td>
      <td>Toimenpide</td>
      <td>
        <font size=2>protokolla: analysis Servicesistä <br>todentaminen: {windows, basic, anonyymi, ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektilaji: {mittayksikön}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Servicesin monidimensionaalisen</td>
      <td>Taulukko</td>
      <td>Dimensio</td>
      <td>
        <font size=2>protokolla: analysis Servicesistä <br>todentaminen: {windows, basic, anonyymi, ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektilaji: {Dimension}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Servicesin taulukkomuotoisen</td>
      <td>Säilö</td>
      <td>Malli</td>
      <td>
        <font size=2>protokolla: analysis Servicesistä <br>todentaminen: {windows, basic, anonyymi, ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Servicesin taulukkomuotoisen</td>
      <td>KPI:</td>
      <td>KPI:</td>
      <td>
        <font size=2>protokolla: analysis Servicesistä <br>todentaminen: {windows, basic, anonyymi, ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektilaji: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Servicesin taulukkomuotoisen</td>
      <td>Toimenpide</td>
      <td>Toimenpide</td>
      <td>
        <font size=2>protokolla: analysis Servicesistä <br>todentaminen: {windows, basic, anonyymi, ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektilaji: {mittayksikön}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Servicesin taulukkomuotoisen</td>
      <td>Taulukko</td>
      <td>Taulukko</td>
      <td>
        <font size=2>protokolla: analysis Servicesistä <br>todentaminen: {windows, basic, anonyymi, ei mitään} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektilaji: {taulukon}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Servicesin</td>
      <td>Säilö</td>
      <td>Palvelin</td>
      <td>
        <font size=2>protokolla: reporting Servicesin <br>todentaminen: {windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versio: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Servicesin</td>
      <td>Raportti</td>
      <td>Raportti</td>
      <td>
        <font size=2>protokolla: reporting Servicesin <br>todentaminen: {windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;polku <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versio: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Säilö</td>
      <td>Tietokannan</td>
      <td>
        <font size=2>protokolla: teradata <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Taulukko</td>
      <td>Taulukon, näkymän</td>
      <td>
        <font size=2>protokolla: teradata <br>todentaminen: {protokolla, Windowsin} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;palvelin <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tietokannan <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objektin</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server päällikön tietopalvelut</td>
      <td>Säilö</td>
      <td>Malli</td>
      <td>
        <font size="2">protokolla: mssql mds <br>todentaminen: {windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versio</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server päällikön tietopalvelut</td>
      <td>Taulukko</td>
      <td>Kohteen</td>
      <td>
        <font size="2">protokolla: mssql mds <br>todentaminen: {windows} <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL-osoite <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;malli <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versio <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kohteen</font>
      </td>
    </tr>
    <tr>
      <td>Muut (ei jokin edellä mainituista)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>protokolla: yleinen resurssi <br>osoite: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Aihetunnus</font>
      </td>
    </tr>
</table>
