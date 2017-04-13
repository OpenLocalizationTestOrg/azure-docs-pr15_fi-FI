<properties
   pageTitle="Lataa tiedot SQL Server Azure SQL tietojen varasto (SSIS) kyselyjä | Microsoft Azure"
   description="Näytetään, miten voit luoda SQL Server Integration Services (SSIS)-paketti, jos haluat siirtää tietoja useista tietolähteistä SQL-tietovarasto."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;sonyama;barbkess"/>

# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Lataa tiedot SQL Server Azure SQL tietojen varasto (SSIS) tuominen

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Luo SQL Server Integration Services (SSIS)-paketti, voit ladata tietoja SQL Server Azure SQL-tietovarasto. Voit halutessasi uudelleenjärjestely, muunna ja puhdistaa tiedot, kun se kulkee SSIS tiedonkulun.

Tässä opetusohjelmassa menettelet seuraavasti:

- Luo uusi integrointipalveluiden projekti Visual Studiossa.
- Muodosta yhteys tietolähteisiin, kuten SQL Server (lähteenä) ja SQL-tietovarasto (kohteeksi).
- Suunnittele SSIS-paketin, lataa tiedot lähteestä kohteeseen.
- Suorita lataamaan tietoja SSIS-paketin.

Tässä opetusohjelmassa käyttää SQL Server tietolähteenä. SQL Server voidaan käyttää paikallisen tai Azure virtuaalikoneen.

## <a name="basic-concepts"></a>Peruskäsitteet

Paketin on SSIS työmäärän yksikkö. Aiheeseen liittyvät paketit on ryhmitelty projekteissa. Voit luoda SQL Server Data Tools Visual Studiossa projektien ja rakenne-paketit. Suunnitteluprosessin on visual prosessi, jossa vedä ja pudota komponentit työkaluryhmästä suunnitteluosaan, yhdistää, ja niiden ominaisuuksien määrittäminen. Kun paketti on valmis, voit halutessasi käyttöönottoa SQL Serverin täydellinen hallinta, seurantaa ja suojaus.

## <a name="options-for-loading-data-with-ssis"></a>SSIS-tietojasi lataamisen asetukset

SQL Server Integration Services (SSIS) on joustavia joukko työkaluja, joka tarjoaa useita vaihtoehtoja yhteyden ja tietojen lataaminen käännetään, SQL-tietovarasto.

1. ADO-verkon kohde avulla voit muodostaa yhteyden SQL-tietovarasto. Tässä opetusohjelmassa käyttää ADO-verkon kohde, koska se sisältää vähiten kokoonpanoasetusten määrittäminen.
2. OLE DB kohde avulla voit muodostaa yhteyden SQL-tietovarasto. Tämä vaihtoehto voi antaa hieman parempi suorituskyky kuin ADO verkon kohde.
3. Vaiheen tiedot Azure-Blob-objektien tallennustilaan Azure Blob Lataa tehtävän avulla. Valitse Käynnistä Polybase komentosarjan, joka lataa tiedot SQL-tietovarasto SSIS suorittaa SQL-tehtävän avulla. Tämä vaihtoehto sisältää parhaan suorituskyvyn kolmesta vaihtoehdosta tässä luettelossa. Azure-Blob-objektien Lataa tehtävän saat lataamalla [Microsoft SQL Server 2016 Integration Services Feature Packin Azure varten][]. Saat lisätietoja Polybase [PolyBase][]oppaassa.

## <a name="before-you-start"></a>Ennen aloittamista

Jos haluat askel Tässä opetusohjelmassa on:

1. **SQL Server Integration Services (SSIS)**. SSIS on SQL Serverin osan, ja vaatii kokeiluversio tai käyttöoikeus SQL Server-version. SQL Server 2016 Preview'n kokeiluversio asentamisesta [SQL Server arvioinnit][].
2. **Visual Studio**. Ilmainen Visual Studio 2015 yhteisön Editionin asentamisesta [Visual Studio yhteisön][].
3. **SQL Server Data Tools for Visual Studio (SSDT)**. Saat SQL Server Data Tools for Visual Studio 2015-kohdassa [Lataa SQL Server Data Tools (SSDT)][].
4. **Mallitiedot**. Tässä opetusohjelmassa käyttää tallennettu SQL Server-AdventureWorks mallitietokannan tietolähteenä mallitiedot on ladattava SQL-tietovarasto kyselyjä. AdventureWorks mallitietokannan asentamisesta [AdventureWorks 2014 otoksen tietokannat][].
5. **A SQL-tietovarasto tietokannan ja käyttöoikeudet**. Tässä opetusohjelmassa muodostaa yhteyden SQL-tietovarasto esiintymän ja lataa tiedot siihen. Sinulla on oikeudet taulukon luominen ja lataa tiedot.
6. **Palomuurisäännön**. Sinun on luotava palomuurisäännön SQL-tietovarasto Valitse paikallinen IP-osoitteeseen ennen kuin voit ladata tietoja SQL-tietovarasto.

## <a name="step-1-create-a-new-integration-services-project"></a>Vaihe 1: Luo uusi integrointipalveluiden projekti

1. Käynnistä Visual Studio 2015.
2. Valitse **Tiedosto** -valikossa **Uusi | Projektin**.
3. Siirry **asennettu | Mallien | Liiketoimintatiedot | Integration Services** projektityypit.
4. Valitse **Integration Services Project**. Anna arvot **nimi** ja **sijainti**ja valitse sitten **OK**.

Visual Studio avautuu, ja luo uuden projektin Integration Services (SSIS). Valitse Visual Studio avautuu projektin suunnittelu yhden uuden SSIS-paketin (Package.dtsx). Näet näytön seuraavilla alueilla:

- Valitse vasemmalla olevassa **työkaluryhmän** SSIS-osia.
- Keskellä suunnitteluosaan, jossa on useita välilehtiä. Käytetään yleensä ainakin **Hallintavuo** ja **Tiedonkulku** -välilehdet.
- Oikealle, **Ratkaisunhallinnassa** ja **Ominaisuudet** -ruudut.

    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Vaihe 2: Luo perustiedot-työnkulku

1. Vedä Data Flow tehtävän työkaluryhmästä suunnitteluosaan keskikohtaan ( **Hallintavuo** -välilehti).

    ![][02]

2. Data Flow tehtävä kaksoisnapsauttamalla tiedonkulku-välilehteen.
3. Vedä ADO.NET-tietolähteen suunnitteluosaan työkaluryhmän muista lähteistä-luettelosta. Lähde-sovittimen, ovat valittuina, ja muuta kansion nimeä **SQL Server-tietolähteen** **Ominaisuudet** -ruudussa.
4. Vedä työkalurivi muut kohteet-luettelosta ADO.NET-kohteen suunnitteluosaan ADO.NET-lähde-kohdassa. Kohde-sovittimen, ovat valittuina, ja muuta kansion nimeä **SQL DW** kohteeseen **Ominaisuudet** -ruudussa.

    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Vaihe 3: Määritä lähde-näyttösovittimen

1. Kaksoisnapsauta lähde-sovittimen, voit avata **ADO.NET-lähde-editori**.

    ![][03]

2. Valitse **ADO.NET-lähde-editori** **Yhteyksienhallinnan** -välilehdessä **Uusi** -painike avaa **ADO.NET-Yhteyksienhallinnan määrittäminen** -valintaikkuna ja luo SQL Server-tietokantaan, josta tässä opetusohjelmassa lataa tiedot yhteysasetukset **ADO.NET-Yhteyksienhallinnan** -luettelon vieressä.

    ![][04]

3. Valitse **Uusi** -painike avaa **Yhteyksienhallinnan** -valintaikkuna ja luoda uuden tietoyhteyden **ADO.NET-Yhteyksienhallinnan määrittäminen** -valintaikkuna.

    ![][05]

4. Tee seuraavat kohdat **Yhteyksienhallinnan** -valintaikkunassa.

    1. **Toimittaja**-SqlClient-tietopalvelu valitseminen
    2. Kirjoita **palvelimen nimi**, SQL Server-nimi.
    3. **Kirjaudu sisään palvelimeen** -kohdassa Valitse tai kirjoita todennustiedot.
    4. Valitse **Muodosta yhteys tietokantaan** -osiossa AdventureWorks mallitietokannassa.
    5. Valitse **Testiyhteys**.
    
        ![][06]
    
    6. Valitse avautuvassa valintaikkunassa raportoi mukaan Yhteystesti tulokset, voit palauttaa **Yhteyden hallinta** -valintaikkunassa **OK** .
    7. Valitse **Yhteyksienhallinnan** -valintaikkunan palaa **Määrittäminen ADO.NET Yhteyksienhallinnan** -valintaikkunassa **OK** .
 
5. Valitse **Määritä ADO.NET-Yhteyksienhallinnan** -valintaikkunassa **OK** , jos haluat palata **ADO.NET-lähde-editori**.
6. **ADO.NET-lähde-editori**, **taulukon tai näkymän nimi** -luettelossa valitse **Sales.SalesOrderDetail** taulukko.

    ![][07]

7. Valitsemalla **Esikatselu** voit tarkistaa lähdetaulukon **Esikatselu kyselytulokset** -valintaikkunassa ensin 200 tietorivit.

    ![][08]

8. **Esikatselun kyselytulokset** -valintaikkuna valitsemalla **Sulje** palaa **ADO.NET-lähde-editori**.
9. **ADO.NET-lähde-editori**valitsemalla **OK** tietolähteen määrittämisen.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Vaihe 4: Yhdistäminen lähde-sovittimen kohde-näyttösovittimen

1. Valitse lähde-sovittimen suunnittelualueella.
2. Valitse sinisen nuolen, joka ulottuu lähde-sovittimen ja vedä se kohde-editorin, kunnes se kohdistuu haluamaasi kohtaan.

    ![][10]

    Tyypillinen SSIS-paketin Käytä useita muita osia välissä lähde ja kohde SSIS-työkaluryhmästä uudelleenjärjestely, muuntaminen ja puhdistaa tiedot, kun se kulkee SSIS tiedonkulun. Jos haluat säilyttää tässä esimerkissä mahdollisimman yksinkertaisena, emme muodostamassa lähteen suoraan kohteeseen.

## <a name="step-5-configure-the-destination-adapter"></a>Vaihe 5: Määritä kohde-näyttösovittimen

1. Kaksoisnapsauta kohde-sovittimen, **ADO.NET kohde-editorin**avaaminen.

    ![][11]

2. Valitse **ADO.NET kohde editorin** **Yhteyksienhallinnan** -välilehdessä **Uusi** -painike avaa **ADO.NET-Yhteyksienhallinnan määrittäminen** -valintaikkuna ja luo Azure SQL-tietovarasto tietokanta, johon tässä opetusohjelmassa lataa tiedot yhteysasetukset **Yhteyksienhallinnan** -luettelon vieressä.
3. Valitse **Uusi** -painike avaa **Yhteyksienhallinnan** -valintaikkuna ja luoda uuden tietoyhteyden **ADO.NET-Yhteyksienhallinnan määrittäminen** -valintaikkuna.
4. Tee seuraavat kohdat **Yhteyksienhallinnan** -valintaikkunassa.
    1. **Toimittaja**-SqlClient-tietopalvelu valitseminen
    2. Kirjoita **palvelimen nimi**-SQL-tietovarasto nimi.
    3. **Kirjaudu sisään palvelimeen** -osassa valitsemalla **Käytä SQL Server-todennusta** ja kirjoita todennustiedot.
    4. Valitse **Muodosta yhteys tietokantaan** -osiossa aiemmin luotuun tietovarasto SQL-tietokantaan.
    5. Valitse **Testiyhteys**.
    6. Valitse avautuvassa valintaikkunassa raportoi mukaan Yhteystesti tulokset, voit palauttaa **Yhteyden hallinta** -valintaikkunassa **OK** .
    7. Valitse **Yhteyksienhallinnan** -valintaikkunan palaa **Määrittäminen ADO.NET Yhteyksienhallinnan** -valintaikkunassa **OK** .
5. Valitse **OK** , jos haluat palata **ADO.NET kohde editorin** **ADO.NET-Yhteyksienhallinnan määrittäminen** -valintaikkuna.
6. **ADO.NET kohde-editori**valitsemalla **Uusi** Avaa **Luo taulukko** -valintaikkuna, Luo uusi kohdetaulukko, joka vastaa lähdetaulukon sarakeluettelo **käyttäminen taulukon tai näkymän** -luettelon vieressä.

    ![][12a]

7. Tee seuraavat kohdat **Luo taulukko** -valintaikkunassa.

    1. Siirry **SalesOrderDetail**kohdetaulukko taulukon nimi.
    2. Poista **rowguid** -sarake. SQL-tietovarasto ei tueta **uniqueidentifier** -tietotyyppi.
    3. Siirry **rahaa** **LineTotal** -sarakkeen tietotyypin. **Decimal** -tietotyyppi ei tueta SQL-tietovarasto. Lisätietoja tuetut tietotyypit on artikkelissa [CREATE TABLE (Azure SQL-tietovarasto, Parallel Data Warehouse)][].
    
        ![][12b]
    
    4. Valitse **OK** taulukon luominen ja palaa **ADO.NET kohde-editorin**.

8. Valitse **ADO.NET kohde editorin**nähdäksesi, kuinka lähdetaulukon sarakkeet yhdistetään sarakkeiden kohde **yhdistämismääritykset** -välilehti.

    ![][13]

9. Valitse tietolähteen määrittämisen **OK** .

## <a name="step-6-run-the-package-to-load-the-data"></a>Vaihe 6: Suorita lataamaan tiedot-paketti

Suorita-paketti, napsauttamalla työkalurivin **Käynnistä** -painiketta tai valitsemalla yhden **Virheenkorjaus** -valikossa **Suorita** -asetukset.

Paketin käynnistyy, näet keltainen pyörivä vierityspainikkeet osoittamaan tehtävän sekä käsitellä mennessä rivien määrä.

![][14]

Kun paketti on suoritettu, näet vihreitä valintamerkkejä osoittamaan success sekä ladata lähteestä kohteeseen tietoriviä kokonaismäärän.

![][15]

Onnittelen! SQL Server-Integrointipalveluilla onnistuneesti käyttänyt voit ladata tietoja SQL Azure-tietovarasto.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja SSIS tiedonkulun. Aloita tästä: [Tiedonkulku][].
- Opettele virheenkorjaus ja vianmääritys pakettien oikealla rakenne-ympäristössä. Aloita tästä: [Paketin kehittäminen vianmääritys työkaluja][].
- Opi ottamaan SSIS projekteja ja pakettien Integration Services-palvelimen tai johonkin muuhun tallennussijaintiin. Aloita tästä: [projektien käyttöönotto ja paketit][].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase opas]: https://msdn.microsoft.com/library/mt143171.aspx
[Lataa SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[Luo taulukko (Azure SQL-tietovarasto, Parallel-tietovarasto)]: https://msdn.microsoft.com/library/mt203953.aspx
[Tiedonkulun]: https://msdn.microsoft.com/library/ms140080.aspx
[Vianmääritystyökalut paketin kehittäminen]: https://msdn.microsoft.com/library/ms137625.aspx
[Projektien ja pakettien käyttöönotto]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack-Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server arvioinnit]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio yhteisön]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 otoksen tietokannat]: https://msftdbprodsamples.codeplex.com/releases/view/125550
