<properties
    pageTitle="Voit määrittää Business-sanasto, määräytyvät tunnisteita | Microsoft Azure"
    description="Toimintaohjeet artikkelissa korostaminen Azure tietoluettelon business-sanasto määrittäminen ja käyttäminen yleisiä liiketoiminta-sanasto lisätä tunnisteen rekisteröity tietojen resurssit."
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

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>Voit määrittää määräytyvät tunnisteissa Business-sanasto

## <a name="introduction"></a>Johdanto

Azure tietoluettelon sisältää ominaisuuksia, joilla tietojen lähteen etsiminen, käyttäjät voivat helposti löydät ja ymmärtää analysoinnissa ja päätösten tarvitsemiaan tietolähteitä. Etsinnän näiden ominaisuuksien tehdä suurimmista vaikutus, kun käyttäjät voivat löytää ja ymmärtää laaja solualue, käytettävissä olevat tietolähteet.

Yksi tietoluettelon-ominaisuus, joka sisältää enemmän tietoja resurssien tietoja on tunnisteita. Tunnisteiden määrityksessä avulla käyttäjät voivat yhdistää avainsanat sijoituksen tai sarake, joka puolestaan on helppo Tutustu kohteiden etsiminen tai selaaminen, ja käyttäjät voivat ymmärtää helpommin kontekstin ja palveluita kohteen.

Kuitenkin tunnisteita voi joskus aiheuttaa ongelmia omaa. Esimerkkejä ongelmia, jotka voidaan ottaa käyttöön merkitsemällä ovat seuraavat:

1.  Käyttäjien lyhenteet käyttäminen joitakin varat ja laajennettu teksti muille aikana tunnisteita. Tämä ristiriidassa haitallinen vaikutus kohteiden etsiminen, vaikka käyttötarkoitukseen on tunnisteen varat sama tunniste.
2.  Tunnisteita, jotka tarkoittaa eri asiaa eri tilanteissa. Esimerkiksi kutsua "Tuotto" asiakkaan tietojoukon tunnisteen saattaa tarkoittaa tuotto asiakkaan mukaan, mutta samaa tunnistetta vuosineljänneksen myynti dataset-saattaa tarkoittaa vuosineljännesten tuoton yritykselle.  

Jotta nämä ja muut vastaavat haasteisiin, tietoluettelon sisältää liiketoiminta-sanasto.

Data Catalog Business sanasto avulla organisaatiot voivat asiakirjan tärkeisiin ehdot ja määritelmät yleisiä liiketoiminta-sanaston luomiseen. Tämä hallinnointitapa mahdollistaa tietoliikenteestä yhdenmukaisuuden koko organisaation. Kun ehdot on määritetty business sanastosta, ne voidaan varata tiedot ja luettelon kohteita käyttämällä samoin kuin tunnisteiden, siten ottaminen käyttöön _sovelletaan tunnisteita_.

> [AZURE.NOTE] Tässä artikkelissa kuvatut toiminnot ovat käytettävissä vain tietoluettelossa Standard Edition, Azure. Vapaa Edition ei tarjoa säännelty tunnisteita tai business sanasto ominaisuuksia.

## <a name="glossary-availability-and-privileges"></a>Sanasto saatavuudesta ja käyttöoikeudet

*Liiketoiminta-sanasto on käytettävissä Standard Edition, Azure tietoluettelon. Vapaa Edition, tietoluettelon ei sisällä sanasto.*

Liiketoiminta-sanasto käyttää kautta tietoluettelon portal siirtymisvalikkoa "Sanastosta"-vaihtoehto.  

![Käyttäminen business-sanasto](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Data Catalog Järjestelmänvalvojat ja sanasto järjestelmänvalvojat-roolin jäsenet voi luoda, muokata ja poistaa sanaston termeistä business sanastosta. Kaikki tietoluettelon käyttäjät voivat tarkastella termin määritelmiä ja merkitä sanaston termeistä resurssien.

![Uuden termin sanaston lisääminen](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>Sanaston termeistä luominen

Tietoluettelon ja sanasto järjestelmänvalvojat voit luoda uuden sanaston termeistä valitsemalla uuden termin ' sanaston termeistä luontipainiketta, kun seuraavat kentät:

* Termin business määritelmä
* Kuvaus, joka kuvaa käyttötarkoituksen tai business säännöt resurssi tai sarake
* Luettelo sidosryhmän, joka eniten lisätietoja termi
* Päätermi, joka määrittää, missä termi on järjestetty hierarkia


## <a name="glossary-term-hierarchies"></a>Sanastossa termi hierarkiat

Tietoluettelo business sanasto mahdollistaa kuvaamaan business sanaston kuin hierarkian ehdot. Näin organisaatiot voivat luoda luokitteluun ehdot, joka edustaa paremmin niiden business luokituksen.

Termin nimen on oltava yksilöllinen tietyllä hierarkiatasolla olevan hierarkia - samannimiset, ei sallita. Ei ole rajoitettu hierarkian tasojen määrän, mutta hierarkian on usein helpompi ymmärtää kun on kolme tasoa tai vähemmän.

Hierarkioiden business sanastosta käyttäminen on valinnaista. Jätä ylemmän tason termin kenttä tyhjäksi sanaston termeistä luominen ehdot (ei-hierarkkisia) flat luettelo sanastosta.  

## <a name="tagging-assets-with-glossary-terms"></a>Tunnisteiden resurssien sanasto ehdot

Kun sanasto ehdot on määritetty luettelon sisällä, kokemukset tunnisteita varat on optimoitu etsiä sanastossa käyttäjän kirjoittaessa niiden tunniste. Tietoluettelo portaalin näyttää luettelon vastaavat valittavana käyttäjän sanaston termeistä. Jos käyttäjä valitsee sanasto termin tiedot lisätään kohteen tunnisteen nimellä (tietovälinettä luettelosta sanasto-tunniste). Käyttäjä voi halutessaan Luo uusi termi, joka ei ole sanastossa (tietovälinettä kirjoittamalla käyttäjän tunniste).

![Tietoja resurssi merkitty yhden käyttäjän tunniste- ja kaksi sanasto-tunnisteet](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Käyttäjän tunnisteet ovat tunniste, jota tuetaan tietoluettelossa vapaa Edition on ainoa.

### <a name="hover-behavior-on-tags"></a>Pidä hiiren osoitinta tunnisteiden toiminta
Tietoluettelo portaalissa tunnisteet kahdentyyppisiä ovat visuaalisesti eri eri hover toiminnan kanssa. Kun käyttäjä päällä käyttäjän tunnistetta he voivat nähdä tunnisteteksti ja käyttäjän tai käyttäjät, jotka on lisätty tunnisteen. Kun käyttäjä päällä sanasto-tunniste, ne näkevät määritelmä sanastossa termi ja linkki avaa business sanastossa termi koko määritelmän tarkastelemiseen.

### <a name="search-filters-for-tags"></a>Tunnisteiden hakusuodattimet
Sanasto tunnisteet ja käyttäjän tunnisteita voi ja voidaan käyttää suodattimina löytyy hakutoiminnolla.

## <a name="summary"></a>Yhteenveto
Liiketoiminta-sanasto Azure tietoluettelon ja sen avulla säännelty tunnisteita Salli tietojen varat voidaan tunnistaa, hallita ja yhdenmukaisesti havaitsi. Liiketoiminta-sanasto voivat esitellä organisaation käyttäjien keskuudessa business-sanaston oppiminen ja tukee kuvaava metatiedon varaamisen, että kohteiden etsiminen ja postitusosoitteet tietoja.

## <a name="see-also"></a>Katso myös

- [REST API ohjeissa liiketoimintaa sanasto](https://msdn.microsoft.com/library/mt708855.aspx)
