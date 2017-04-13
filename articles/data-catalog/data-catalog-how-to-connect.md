<properties
   pageTitle="Tietolähteiden yhdistämisestä | Microsoft Azure"
   description="Toimintaohjeet artikkelissa korostaminen yhdistämisestä tietolähteiden havaitsi Azure tietoluettelon avulla."
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
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>Miten tietolähteisiin yhdistäminen

## <a name="introduction"></a>Johdanto
**Microsoft Azure tietoluettelon** on täysin hallitun pilvipalvelussa, joka on järjestelmän rekisteröinnin ja järjestelmä on enterprise tietolähteiden etsiminen. Toisin sanoen **Azure tietoluettelon** 's all about toimintamallia löytää, toimintaperiaatteet ja käyttäminen tietolähteiden ja auttaa organisaatioita tulee niiden annetuista tiedoista. Tässä skenaariossa tarjoama käyttää tiedot – kun käyttäjä löytää tietolähteen ja ymmärtää tarkoitustaan, seuraava vaihe on muodostaminen tietolähteen valitseminen sen tiedot.

## <a name="data-source-locations"></a>Tietolähteen sijainnit
Tietojen lähteen rekisteröinnin yhteydessä **Azure tietoluettelon** vastaanottaa metatietojen tietoja tietolähteestä. Metatiedot sisältää tietoja tietolähteen sijainti. Sijainnin tiedot vaihtelevat tietolähteestä tietolähteeseen, mutta se sisältää aina yhteyden tarvittavat tiedot. SQL Server-taulukon sijainti esimerkiksi sisältää palvelimen nimen, tietokannan nimi, mallin nimi ja taulukkonimi, samalla, kun SQL Server Reporting Services-raportin sijainti sisältää palvelimen nimen ja polun raporttiin. Muuntyyppisten tietojen lähde on sijainnit, jotka vastaavat rakenteen ja lähdejärjestelmän ominaisuuksia.

## <a name="integrated-client-tools"></a>Integroitu asiakastyökalut
Yhteyden muodostaminen tietolähteen helpoin tapa on käyttää "avoinna..." **Azure tietoluettelon** portal-valikko. Tässä valikossa näyttää valitun kohteen muodostamisesta asetusten luettelo.
Kun käytät oletusnäkymä-ruutu, valikossa on käytettävissä kunkin-ruutu.

 ![SQL Server-taulukko avautuu Excelissä tietojen resurssi-ruutu](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Käytettäessä Luettelonäkymä-valikko on käytettävissä etsintäpalkin portaalin ikkunan yläosassa.

 ![Etsintäpalkin Report Managerin SQL Server Reporting Services-raportin avaaminen](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Tuetut asiakassovellukset
Kun käytät "avattu..." asiakastietokoneen on oltava asennettuna tietolähteet Azure tietoluettelon-portaalissa, oikea asiakassovellus valikko.

| Avaa sovelluksessa | Tiedostotunniste protokolla | Tuetut versiot |
| --- | --- | --- |
| Excel | .odc | Excel 2010 tai uudempi versio |
| Excel (ylin 1000) | .odc | Excel 2010 tai uudempi versio |
| Power Query | .xlsx | Excel 2016- tai Excel 2010- tai Excel 2013: n Power Query for Excel-apuohjelma on asennettuna
| Power BI Desktopin | .pbix | Power BI Desktopin heinäkuussa 2016 tai uudempi versio |
| SQL Server Data Tools | vsweb: / / | Visual Studio 2013 Päivitä 4 tai uudempi versio SQL Server sillä asennettuna |
| Report Managerin | http:// | Katso [selainvaatimukset SQL Server Reporting Services-palveluissa](https://technet.microsoft.com/en-us/library/ms156511.aspx) |

## <a name="your-data-your-tools"></a>Tiedot-Työkalut
Tietoja kohteiden valittuna tyypin riippuu käytettävissä-valikosta Asetukset. Kaikki mahdolliset Työkalut sisällytetään "Avaa..." valikko, mutta se on edelleen helposti muodostaa yhteyden tietolähteeseen, minkä tahansa asiakas-työkalun avulla. Kun tietojen resurssi on valittuna **Azure tietoluettelon** -portaalissa, valmis sijainti näkyy ominaisuudet-ruudussa.

 ![Yhteystietojen SQL Server-taulukko](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

Yhteyden tiedot-tiedot vaihtelevat tietolähdetyyppi tietolähteen tyyppi, mutta portaalissa tietojen avulla voit kaikki yhteyden muodostaminen tietolähteen minkä tahansa asiakas-työkalu. Käyttäjien kopioida yhteyden tietoja tietolähteistä, joita on havaitsi **Azure tietoluettelon**ne voivat käsitellä tietoja niiden valinta-työkalun avulla.

## <a name="connecting-and-data-source-permissions"></a>Yhteyden ja tietojen lähde-käyttöoikeudet
Vaikka **Azure tietoluettelon** tekee tietolähteiden havaittavissa, voi käyttää tietoja itse pysyy valvonnassa tietojen lähteen omistaja tai järjestelmänvalvoja. Tutustu **Azure tietoluettelon** tietolähde ei anna käyttäjän oikeutta käyttää tietolähdettä itse.

Helpompi käyttäjille, jotka Tutustu tietolähteen, mutta ei ole oikeutta käyttää sen tietoja, käyttäjät voivat antaa Pyydä käyttöoikeuksia-ominaisuuden, kun lisäät huomautuksia tietolähde. Tässä annettu – mukaan lukien prosessin tai toimiminen tietojen lähteen muodostamisen linkkejä – tiedot esitetään tietolähteen sijaintitietojen portaalissa: n rinnalla.

 ![Pyyntö access-ohjeita ja yhteystiedot](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>Yhteenveto
Tietolähteen rekisteröimistä **Azure tietoluettelon** mahdollistaa tietojen havaittavissa rakenteellisia ja kuvaava metatiedot kopioidaan luettelon palvelun tietolähteen. Kun tietolähde on rekisteröity ja löytää, käyttäjät voivat muodostaa yhteyden tietolähteeseen **Azure tietoluettelon** -portaalista "avattu..." " valikon tai valinta tietojen niiden työkaluilla.

## <a name="see-also"></a>Katso myös
- [Azure tietoluettelon käytön aloittaminen](data-catalog-get-started.md) -opetusohjelma vaiheittaiset lisätietoja Muodosta yhteys tietolähteisiin.
