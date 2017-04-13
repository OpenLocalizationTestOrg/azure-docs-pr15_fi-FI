<properties
   pageTitle="Toimintojen hallinta Suite suojaus ja valvonta-ratkaisun perusaikataulun | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten voit käyttää OMS suojaus ja valvonta-ratkaisun suorittamiseen kaikki valvottu tietokoneet perusaikataulun arvioinnin vaatimustenmukaisuus ja suojaus tarkoitusta varten."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>

# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Toimintojen hallinta Suite suojaus ja valvonta-ratkaisun perusaikataulun arviointi

Tämän asiakirjan avulla voit käyttää [Toimintojen hallinta Suite (OMS) suojaus ja valvonta-ratkaisun](operations-management-suite-overview.md) perusaikataulun arviointi ominaisuuksia valvottu resurssien suojattu tila.

## <a name="what-is-baseline-assessment"></a>Mikä on perusaikataulun arviointi?

Microsoft-teollisuuden ja government organisaatiot maailmanlaajuisesti, ja määrittää Windowsiin, joka edustaa suojatuissa palvelimen ympäristöissä. Tässä määrityksessä on rekisteriavaimia, valvonta-käytäntöasetukset ja käytännön suojausasetukset sekä Microsoftin suositeltuja arvoja näiden asetusten. Tämä sääntöryhmän kutsutaan suojauksen perusaikataulun. OMS suojaus ja valvonta perusaikataulun arviointi ominaisuuksien saumattomasti skannata yhteensopivuuden kaikkiin tietokoneisiin. 

On kolmenlaisia säännöt:

- **Rekisterin sääntöjä**: Tarkista, että rekisteriavaimia on määritetty oikein.
- **Audit sääntöjä**: Valvo käytäntöjen säännöt.
- **Käytännön suojaussäännöt**: käyttäjien käyttöoikeuksien koneessa koskevia sääntöjä.

> [AZURE.NOTE] Tässä artikkelissa [Käytä OMS suojauksen arvioida suojauksen määritys perusaikataulun](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) lyhyesti tätä ominaisuutta.

## <a name="security-baseline-assessment"></a>Perusaikataulun arviointi

Voit tarkastella omaa nykyisen perusaikataulun arvioinnin kaikissa tietokoneissa, joissa on seurattava OMS suojaus ja valvonta hallintanäkymässä.  Suorita suojaus perusaikataulun arviointi Raporttinäkymät-ikkunan tekemällä seuraavat toimet:

1. Valitse **Microsoft toimintojen hallinta Suite** tärkeimmät koontinäyttö, **Suojaus ja valvonta** -ruutu.
2. Valitse **Suojaus ja valvonta** Raporttinäkymät-ikkunan **Perusaikataulun arviointi** **Suojauksen toimialueet**-kohdassa. **Perusaikataulun arvioinnin** Raporttinäkymät-ikkunan näkyy seuraavassa kuvassa esitetyllä tavalla:
    
    ![OMS suojaus ja valvonta perusaikataulun arviointi](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Tämä koontinäyttö on jaettu kolme pääaluetta:

- **Tietokoneiden verrattuna perusaikataulun**: Tässä osassa kerrotaan tietokoneissa, joissa on käytetty määrä ja tietokoneissa, joissa välitetty arvioinnin prosentteina. Voit myös aloittaa Ylin 10 tietokoneiden ja prosentti tulos arviointia varten.
- **Pakollinen säännöt tila**: Tässä osassa on tuomaan tilatieto epäonnistui sääntöjen mukaan vakavuus käyttötarkoitukseen ja epäonnistui säännöt tyypin mukaan. Haluatko ensimmäisen kaavio voivat tunnistaa nopeasti Jos useimmat epäonnistui säännöt ovat tärkeitä, vai ei. Voit myös aloittaa luettelon 10 epäonnistui säännöt ja niiden vakavuus. Toisessa kaaviossa näkyy sääntö, joka epäonnistui arvioinnin aikana tyyppi. 
- **Tietokoneiden puuttuu perusaikataulun arviointi**: Tässä osassa luettelon tietokoneissa, joihin ei ole käytetty vuoksi käyttöjärjestelmän eivät ole yhteensopivia tai epäonnistuu. 

### <a name="accessing-computers-compared-to-baseline"></a>Perusaikataulun verrattuna tietokoneita

Kaikki tietokoneet ovat Ihannetapauksessa ole yhteensopiva suojauksen perusaikataulun Assessment. Kuitenkin olettaa, että jotkin olosuhteissa tämä ei tapahdu. Osana suojauksen hallinta-prosessi on tärkeää sisällyttää tarkistaminen tietokoneissa, joihin ei läpäissyt kaikki suojauksen arviointi testeissä. Nopea tapa visualisointi, joka on **tietokoneiden käyttää** **tietokoneiden verrattuna perusaikataulun** -osan valitseminen. Lokitiedoston hakutuloksen tietokoneissa, kuten seuraavassa ruudussa näkyy luettelo, jossa näkyvät pitäisi näkyä seuraavat:

![Tietokone käyttää tulokset](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Hakutuloksen näkyy taulukkomuotoon, jossa ensimmäisessä sarakkeessa on tietokonenimi ja toisen värin on säännöt, jotka epäonnistui määrä. Koskeva sääntö, joka epäonnistui tyypin tietojen hakemiseen, napsauta epäonnistui sääntöjen tietokoneen nimen määrä. Pitäisi näkyä tulos, joka on samanlainen kuin seuraavan kuvan mukaisesti:

![Tietokone käyttää tulokset tiedot](./media/oms-security-baseline/oms-security-baseline-fig3.png)

Tämän hakutulos on käyttää sääntöjä, tärkeät säännöt, jotka epäonnistui määrä, Varoitussäännöt ja tiedot epäonnistui sääntöjä yhteensä.

### <a name="accessing-required-rules-status"></a>Käyttämiseen tarvittavat säännöt tila

Saatuaan tietokoneissa, joissa välitetty arvioinnin määrä prosentteina koskevat tiedot voit saada lisätietoja siitä, mitä säännöt epäonnistuvat kriittisyyden mukaan. Tämä visualisoinnin avulla voit priorisoida tietokoneet olisi käsiteltävä ensin jotta ne ovat yhteensopivia seuraava arvioinnissa. Kaavio sijaitsee **epäonnistui mukaan vakavuus säännöt** -ruudun kohdassa **pakollinen säännöt tila** tärkeä osa päälle ja napsauta sitä. Tulos on samankaltainen kuin seuraavassa näytössä pitäisi näkyä seuraavat:

![Epäonnistui säännöt vakavuus tiedot](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

Lokitiedoston tuloksen näet perusaikataulun sääntö, joka epäonnistui tämän säännön kuvaus sekä yleisten määritysten luettelointi (CCE) tunnus suojauksen tämän säännön tyyppi. Seuraavanlaisia määritteitä olisi riittävästi suorittamiseen korjausta voit ratkaista ongelman kohde tietokoneeseen.

> [AZURE.NOTE] Lisätietoja CCE käyttää [Kansallisia haavoittuvuus tietokannan](https://nvd.nist.gov/cce/index.cfm).

### <a name="accessing-computers-missing-baseline-assessment"></a>Tietokoneita puuttuu perusaikataulun arviointi

OMS tukee toimialueen jäsenen perusaikataulun profiili Windows Server 2008 R2 Windows Server 2012 R2 ylöspäin. Windows Server 2016 perusaikataulun ei vielä lopullinen ja lisätään, kun se on julkaistu. Muut käyttöjärjestelmät skannattu kautta OMS suojaus ja valvonta perusaikataulun arviointi näkyy **tietokoneiden puuttuu perusaikataulun arviointi** -osassa.

## <a name="see-also"></a>Katso myös

Tässä asiakirjassa sai perusaikataulun arvioida OMS suojaus ja valvonta. Lisätietoja OMS tietoturvasta on seuraavissa artikkeleissa:

- [Toimintojen hallinta Suite (OMS) yleiskatsaus](operations-management-suite-overview.md)
- [Seuranta ja niihin vastaaminen suojausvaroitusten toimintojen hallinta Suite suojaus ja valvonta ratkaisu](oms-security-responding-alerts.md)
- [Resurssien valvominen toimintojen hallinta Suite suojaus ja valvonta ratkaisu](oms-security-monitoring-resources.md)

