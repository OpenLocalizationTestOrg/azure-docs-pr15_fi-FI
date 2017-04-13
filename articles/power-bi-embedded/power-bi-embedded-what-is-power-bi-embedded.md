<properties
   pageTitle="Mikä on Microsoft Power BI upotetun?"
   description="Power BI upotetun avulla voit integroida Power BI-raportit web tai mobile-sovellukset, joten sinun ei tarvitse mukautettujen ratkaisujen käyttäjien tietojen visualisointi"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="what-is-microsoft-power-bi-embedded"></a>Mikä on Microsoft Power BI upotetun?

**Power BI upotetun**, jossa voit integroida Power BI-raportit oikealle verkossa tai mobile-sovellukset.

![](media\powerbi-embedded-whats-is\what-is.png)

Power BI upotetun on **Azure-palvelu** , joka mahdollistaa valmistajille ja sovellusten kehittäjille pinta Power BI-tietojen ohjelmissa niiden-sovelluksissa. Kehittäjän olet aiemmin luonut sovellukset ja sovellukset on omia käyttäjät ja eri käytettävissä olevia ominaisuuksia. Kyseisissä sovelluksissa myös voi tapahtua, jos haluat, että jotkin Excelin valmiita tietojen osia, kuten kaavioita ja raportteja, voit nyt olla tarjoaa Microsoft Power BI upotetun. Käyttäjien ei tarvitse Power BI-tili, jota käytetään sovelluksen. He voivat edelleen samalla tavalla kuin ennenkin-sovellukseen sisäänkirjautuminen ja tarkastella ja käsitellä Power BI raporttien käytettävyyttä niin, ettei mitään muita käyttöoikeudet.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Microsoft Power BI-käyttöoikeuksien myöntämisestä upotettu

**Microsoft Power BI upotetun** käyttö mallissa Power BI-käyttöoikeuksien myöntämisestä ei ole vastuussa käyttäjä.  Sen sijaan **hahmontaa** sovellusta, joka on muissa kuvat kehittäjä ostettu ja peritään tilaukseen, joka omistaa resurssit.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI upotettu käsitteellinen malli

![](media\powerbi-embedded-whats-is\model.png)

Muuhun palveluun Azure-tietokannassa, kuten resursseja voi käyttää Power BI upotetun valmisteltu [Azure ARM-ohjelmointirajapinnan](https://msdn.microsoft.com/library/mt712306.aspx)kautta. Tässä tapauksessa voit valmistella käyttämäsi resurssi on **Power BI työtilan sivustokokoelman**.

## <a name="workspace-collection"></a>Työtilan sivustokokoelman

**Työtilan sivustokokoelman** on ylimmän tason Azure säilö resursseja, joka sisältää 0 tai Lisää **työtilat**.  **Työtilan** **sivustokokoelman** on kaikki Azure vakio-ominaisuudet sekä seuraavasti:

-   **Pikanäppäinten** – avaimet soitettaessa suojatusti Power BI-ohjelmointirajapinnan (myöhemmin osiossa esitettyjä).
-   **Käyttäjien** – Azure Active Directory (AAD)-käyttäjät, joilla voit hallita Power BI työtilan sivustokokoelman palvelun Azure-portaalissa tai HAARAN API järjestelmänvalvojan oikeuksia.
-   **Alueen** – osana valmistelu **Työtilan sivustokokoelman**, voit valita alueen valmisteltava. Lisätietoja on artikkelissa [Azure-alueita](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Työtilan

**Työtila** on Power BI-sisältöä, jotka voivat sisältää tietojoukkoja, raporttien ja raporttinäkymien säilö. **Työtila** on tyhjä, kun ensimmäisen kerran. Aikana esikatselu luominen Power BI Desktopin käyttäminen kaikki sisältö ja voit ladata sen johonkin työtilojen käyttäminen [Power BI REST API](http://docs.powerbi.apiary.io/reference).

## <a name="using-workspace-collections-and-workspaces"></a>Työtilan sivustokokoelmat ja työtilojen käyttäminen
**Työtilan sivustokokoelmat** ja **työtilojen** ovat säilöjä sisällön käytettävät ja järjestetty kumpi paras tapa sopii luotavaa sovelluksen rakenne. Ole monella eri tavalla, voit järjestää ne sisältöön. Voit halutessasi kaiken sisällön yhden työtilassa ja myöhemmin käyttämällä sovelluksen tunnusten edelleen jakaa sisällön toiseen asiakkaillesi. Voit halutessasi myös laittaa kaikki asiakkaille erillisessä työtilat niin, että jotkin väliä. Vaihtoehtoisesti voit järjestää käyttäjät alueittain eikä asiakkaan perusteella. Tämän joustavan rakenteen avulla voit valita parhaiten sisällön järjestämistä varten.

## <a name="cached-datasets"></a>Välimuistiin tallennetut tietojoukkoja

Välimuistiin tallennetut tietojoukkoja voidaan esikatselussa.  Voi kuitenkin päivittää välimuistiin tallennetut tiedot, kun se on ladattu **Microsoft Power BI upotetun**.

## <a name="authentication-and-authorization-with-app-tokens"></a>Todennus- ja sovelluksen tunnusten kanssa

**Microsoft Power BI upotetun** lykkää sovelluksen suorittamiseen tarvittavat käyttöoikeuksien ja luvan. Ei ole eksplisiittinen vaatimus, että käyttäjiesi on asiakkaiden Azure Active Directory (Azure AD).  Sen sijaan sovelluksen ilmaisee **Microsoft Power BI upotetun** luvan hahmontamaan Power BI-taulukkoraportin **Sovelluksen käyttöoikeuksien tunnusten (sovelluksen tunnuksiksi)**avulla.  Näiden **Tunnusten App** luodaan haluamallasi tavalla, kun sovellus haluaa hahmontamiseen raportin.  Katso [sovelluksen tunnukset](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

**Sovelluksen käyttöoikeuksien tunnusten (sovelluksen tunnuksiksi)** käytetään **Microsoft Power BI upotetun**todennetaan.  Liittyy **Sovelluksen tunnusten**kolmenlaisia:

1.  Tunnusten - käytetään, kun valmistelu uuden **työtilan** **Työtilan sivustokokoelman** valmistelu
2.  Kehittäminen tunnusten - käytetään, kun tehdään puhelut suoraan **Power BI REST API**
3.  Tunnusten - käytetään, kun puheluiden soittamiseen hahmontamiseen upotetun iframe-raportin upottaminen

Näiden tunnusten käytetään tekemiesi aktiviteettien **Microsoft Power BI upotetun**eri vaiheita.  Tunnukset on suunniteltu siten, että voit delegoida käyttöoikeuksia sovellus Power BI. Lisätietoja on artikkelissa [Sovelluksen suojaustunnuksen Flow](power-bi-embedded-app-token-flow.md).

## <a name="see-also"></a>Katso myös
- [Microsoft Power BI upotetun Yleisiä tilanteita, joissa](power-bi-embedded-scenarios.md)
- [Aloita Microsoft Power BI upotetun](power-bi-embedded-get-started.md)
