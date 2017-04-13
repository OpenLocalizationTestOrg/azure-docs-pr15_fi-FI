<properties
   pageTitle="Käyttöoikeudet ja myöntää Power BI upotetun kanssa"
   description="Käyttöoikeudet ja myöntää Power BI upotetun kanssa"
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

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Käyttöoikeudet ja myöntää Power BI upotetun kanssa

Power BI upotetun palvelun käyttää **näppäimet** ja **Sovelluksen tunnusten** todennus- ja -sijaan eksplisiittinen käyttäjän todennusta. Tämän mallin sovelluksen hallitsee todennus- ja että loppukäyttäjille. Tarvittaessa sovelluksen Luo ja lähettää App tunnuksia, jossa kerrotaan palvelun hahmontamaan pyydetty raportti. Tämän rakenteen ei edellytä käyttämään Azure Active Directory käyttöoikeuksien ja todennus-sovelluksen, mutta voit silti.

## <a name="two-ways-to-authenticate"></a>Kaksi tapaa todennusta

**Avain** - voi käyttää näppäimet kaikki Power BI upotetun REST API-puhelut. Näppäimet löytyy **Azure portal** valitsemalla **kaikki asetukset** ja sitten **pikanäppäimet**. Käsittele aina key-tuotetunnuksen, jos se salasana. Painettavat näppäimet on oikeus tehdä kaikki tietyn työtilan sivustokokoelman soittaminen REST-Ohjelmointirajapinnalla.

Haluat käyttää avainta muiden puhelua, Lisää authorization-otsikko:            

    Authorization: AppKey {your key}

**Sovelluksen tunnus** - sovelluksen tunnusten käytetään kaikkien upottaminen pyynnöt. Ne on suunniteltu suorittaa asiakaspuolen, jotta ne on rajoitettu yksi raportti ja vanheneminen ajan määrittämisen parhaana käytäntönä on.

Sovelluksen tunnukset ovat JWT (JSON Web suojaustunnuksen), joka on allekirjoitettu joltakin avaimien.

Sovelluksen tunnus voi olla seuraavat saatavat:

| Varaa      | Kuvaus        |
|--------------|------------|
| **ver**      | Sovelluksen tunnuksen versio. 0.2.0 on nykyinen versio.       |
| **aud**      | Tunnuksen halutulle vastaanottajalle. Power BI upotetun käytettäväksi: "https://analysis.windows.net/powerbi/api".  |
| **ISS**      |  Merkkijono, joka ilmoittaa, joka on antanut tunnuksen sovelluksen.    |
| **tyyppi**     | Sovelluksen tunnus, joka luodaan tyyppi. Nykyinen ainoa tuettu tyyppi on **upottaa**.   |
| **WCN**      | Työtilan sivustokokoelman nimi tunnuksen myöntää varten.  |
| **WID**      | Työtilan ID tunnus annetaan varten.  |
| **poistaminen**      | Raportin ID tunnus annetaan varten.     |
| **käyttäjänimi** (valinnainen) |  Käytetään RLS, tämä on merkkijono, joka auttaa tunnistamaan käyttäjän käytettäessä RLS säännöt. |
| **roolit** (valinnainen)   |   Valitse rivin tasolle suojaussäännöt kohdistettaessa roolit sisältävä merkkijono. Jos useampi kuin yksi rooli, on siirrettävä merkkijono matriisina.    |
| **eksponentti** (valinnainen)    |   Ilmaisee ajan, jossa tunnuksen päättyy. Nämä olisi voi välittää Unix aikaleimat.   |
| **NBF** (valinnainen)    |   Osoittaa aika, jonka tunnus aloittaa oikeellisuuden. Nämä olisi voi välittää Unix aikaleimat.   |

Esimerkki sovelluksen tunnus näyttää tältä:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Kun purkaa, se näyttää tältä:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Näin kulun toiminta

1. Kopioi sovelluksen API-näppäimiä. Saat näppäimet **Azure**-portaalissa.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Tunnuksen vahvistukset vaatimus, ja se on vanheneminen-aika.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Tunnuksen saa allekirjoitettu API-näppäimiä.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Jos haluat tarkastella raportin pyynnöt.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Tunnuksen tarkistetaan API-pikanäppäinten avulla.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  Power BI upotetun lähettää raportin käyttäjälle.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Kun **Power BI upotetun** lähettää raportin käyttäjän, käyttäjä voi tarkastella raportin mukautetun sovelluksen. Esimerkiksi jos olet tuonut [analysoiminen myynnin tiedot PBIX otoksen](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), otoksen web Appin näyttää tältä:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Katso myös
- [Microsoft Power BI upotetun otoksen käytön aloittaminen](power-bi-embedded-get-started-sample.md)
- [Microsoft Power BI upotetun Yleisiä tilanteita, joissa](power-bi-embedded-scenarios.md)
- [Aloita Microsoft Power BI upotetun](power-bi-embedded-get-started.md)
