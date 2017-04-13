<properties
   pageTitle="Aloita Microsoft Power BI upotetun"
   description="Power BI upotetun, Lisää vuorovaikutteisia Power BI-raportteja business intelligence-sovellukseen"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Aloita Microsoft Power BI upotetun

**Power BI upotetun** on Azure-palvelu, jonka avulla sovelluskehittäjät voivat lisätä omat sovellukset vuorovaikutteisia Power BI-raportteja. Aiemmin luotujen sovellusten **Power BI upotetun** työskentelee tarvitsematta uudelleensuunnittelua tai kuinka käyttäjät muuttaminen kirjautunut sisään.

**Microsoft Power BI upotetun** resurssit ovat valmisteltu [Azure ARM-ohjelmointirajapinnan](https://msdn.microsoft.com/library/mt712306.aspx)kautta. Tässä tapauksessa voit valmistella resurssi on **Power BI työtilan sivustokokoelman**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>Työtilan sivustokokoelman luominen
**Työtilan sivustokokoelman** on ylimmän tason Azure resurssi ja sisältöön, joka upotetaan sovelluksen säilö. **Työtilan sivustokokoelman** voidaan luoda kahdella tavalla:

   -    Manuaalisesti käyttämällä Azure-portaalissa
   -    Azure resurssin Manager(ARM) ohjelmointirajapinnan käyttäminen ohjelmallisesti

Oletetaan, että käy läpi vaiheet, voit luoda **Työtilan sivustokokoelman** Azure-portaalissa.

   1.   Avaa ja kirjautua **Azure-portaaliin**: [http://portal.azure.com](http://portal.azure.com).

   2.   Valitse **+ Uusi** on yläreunan paneeli.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   **Tietoja ja Analytics** kohdassa **Power BI upotetun**.
   4.   Valitse **Luomisen sivu**Anna vaaditut tiedot. Katso **hinnoittelu** [Power BI upotetun hinnat](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Valitse **Luo**.

**Työtilan sivustokokoelman** kestää jonkin aikaa valmistelu. Kun olet valmis, sinun otettava **Työtilan sivustokokoelman sivu**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

**Luonti-sivu** sisältää Soita API, työtiloja kannattaa luoda ja ottaa käyttöön sisällön niihin tietoja.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Näytä Power BI API-pikanäppäimet

Yksi Soita Power BI REST API tarvittavat tiedot tärkeimmät osat ovat **Pikanäppäimet**. Näitä käytetään Luo **sovelluksen tunnuksia** , joita käytetään tarkistamiseen API-pyynnöt. Voit tarkastella **Pikanäppäimet**, valitsemalla **Asetukset-sivu** **Pikanäppäimet** . Lisätietoja **sovelluksen tunnusten**on [Authenticating ja sallimisesta Power BI upotetun kanssa](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Saat ' ilmoitus, että sinulla on kaksi avaimet.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Kopioi painettavat näppäimet ja tallentaa ne suojatusti sovelluksen. On tärkeää Käsittele painettavat näppäimet tapaan salasanaa, koska niissä kaikki **Työtilan sivustokokoelman**sisällön käyttöä.

Vaikka kahta näppäintä on lueteltu, vain yksi näppäin tarvitaan tiettynä ajankohtana. Toisen avaimen annetaan, jotta voit säännöllisesti uudelleen näppäimet keskeyttämättä käyttämään palvelua.

Nyt kun olet luonut sovelluksen ja **Pikanäppäinten**Power BI-esiintymän, voit tuoda Omat sovelluksen raportin. Ennen opit tuominen raportin, seuraavassa osassa kuvataan luominen Power BI-tietojoukkoja ja raportteja voi upottaa sovellukseen.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Power BI-tietojoukkoja ja upota sovellukseen raporttien luominen

Nyt kun olet luonut Power BI-esiintymän sovelluksen ja on **Pikanäppäimet**, sinun on Power BI-tietojoukkoja ja raporttien, jonka haluat upottaa. **Power BI Desktopin**avulla voidaan luoda tietojoukkoja ja raportteja. Voit ladata [Power BI Desktopin, Vapauta](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). Vaihtoehtoisesti voit nopeasti alkuun, voit ladata [jälleenmyynti analyysin otoksen PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547). Lisätietoja **Power BI Desktopin**käyttämisestä on artikkelissa [Power BI Desktopin käytön aloittaminen](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

**Power BI Desktopin**voit muodostaa yhteyden tietolähteeseen kopion tiedot tuodaan **Power BI Desktopin** tai muodostaminen tietolähteen **DirectQuery**.

Seuraavassa on erot käytettäessä **tuonti** ja **DirectQuery**.

|Tuo | DirectQuery
|---|---
|Taulukot, sarakkeet *ja tiedot* tuodaan tai **Power BI Desktopin**kopioidaan. Kun käsittelet visualisointeja, **Power BI Desktopin** kyselyn kopion tiedot. Jos haluat nähdä kaikki muutokset, jotka on tapahtunut pohjana olevat tiedot, Päivitä tai tuo, valmis, nykyisen tietojoukko uudelleen.|Vain *taulukoiden ja sarakkeiden* tuotu tai kopioitu **Power BI Desktopin**. Kun käsittelet visualisointeja, **Power BI Desktopin** kyselyn pohjana olevassa tietolähteessä, mikä tarkoittaa, että tarkastelet aina ajan tasalla olevat tiedot.

Lisätietoja yhteyden muodostamisesta tietolähde on artikkelissa [etäyhteyden muodostaminen tietolähteen](power-bi-embedded-connect-datasource.md).

Kun olet tallentanut työsi **Power BI Desktopin**, PBIX-tiedosto luodaan. Tiedoston nimi sisältää raporttiin. Lisäksi jos tuot tietoja PBIX on valmis tietojoukko tai käytettäessä **DirectQuery**PBIX sisältää vain tietojoukko rakenteen. Käyttöönottoa PBIX ohjelmallisesti käyttämällä [Power BI tuo API](https://msdn.microsoft.com/library/mt711504.aspx)työtilaan.

> [AZURE.NOTE] **Power BI upotetun** on muita ohjelmointirajapinnan palvelin ja tietokanta, osoittamalla oman tietojoukko ja määrittää service-tilin tunnistetiedot, jotka dataset käytetään muodostaa yhteys tietokantaan. Katso [Kirjaa SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) ja [korjaustiedoston yhdyskäytävän tietolähde](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet
Edellisessä vaiheessa luomasi työtilan sivustokokoelman ja ensimmäinen raportti ja tietojoukko. Nyt on aika Opettele kirjoittamaan for **Power BI upotettu**koodi. Avulla pääset alkuun luomaasi otoksen verkkosovellukseen: [otosten käytön aloittaminen](power-bi-embedded-get-started-sample.md). Otosten kerrotaan, miten voit:

  - Sisällön valmisteleminen
      - Työtilan luominen
      - PBIX-tiedoston tuominen
      - Päivitä yhteysmerkkijonot ja määritä tunnistetiedot oman tietojoukkoja.

  - Upottaa suojatusti raportin

## <a name="see-also"></a>Katso myös
- [Esimerkki käytön aloittaminen](power-bi-embedded-get-started-sample.md)
- [Käyttöoikeudet ja myöntää Power BI upotetun kanssa](power-bi-embedded-app-token-flow.md)
- [Power BI Desktopin](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
