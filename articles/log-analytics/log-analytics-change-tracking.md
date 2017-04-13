<properties
    pageTitle="Muutosten jäljityksen loki Analytics-ratkaisun | Microsoft Azure"
    description="Voit tehdä määritysten muutosten jäljityksen-ratkaisun Log Analytics avulla voit helposti tunnistaa ohjelmiston ja Windows Services muutokset, jotka esiintyvät ympäristön – tunnistaminen nämä määritysmuutoksia auttaa sinua pinpoint toiminnallisia ongelmia."
    services="operations-management-suite"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operations-management-suite"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="change-tracking-solution-in-log-analytics"></a>Muuta seuranta-ratkaisun Log Analytics


Voit tehdä määritysten muutosten jäljityksen-ratkaisun Log Analytics avulla voit helposti tunnistaa ohjelmiston ja Windows-palvelujen ja Linux daemon muutokset, jotka esiintyvät ympäristön – tunnistaminen nämä määritysmuutoksia auttaa sinua pinpoint toiminnallisia ongelmia.

Asennat ratkaisun Päivitä agentti on asennettuna. Muutokset asentanut ohjelmiston ja Windows-palveluiden valvottu palvelimissa luetaan ja sitten tiedot lähetetään Log Analytics-palvelu pilveen käsittelyä varten. Logiikan käytetään vastaanotetut tiedot ja pilvipalvelussa tietueiden tietoja. Kun muutokset on useita, palvelinten muutokset näkyvät muutosten jäljityksen Raporttinäkymät-ikkunan. Muutosten jäljityksen Raporttinäkymät-ikkunan tietojen avulla näet helposti server-julkaisuinfrastruktuuri tehdyt muutokset.

## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu
Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

- Muutosten jäljityksen-ratkaisun tarvitaan Operations Manager.
- Sinulla on Windows- tai Operations Manager agentti käyttöön kussakin tietokoneessa, johon haluat seurata muutoksia.
- Muutosten jäljityksen-ratkaisun lisääminen OMS työtilan käyttämisestä [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet.  Ei ole tarvitaan lisämäärityksiä.


## <a name="change-tracking-data-collection-details"></a>Seuranta-sivustokokoelman tietojen muuttaminen

Muutosten jäljityksen kerää ohjelmiston varaston ja Windows-palvelun metatietojen avulla agenttien vuoksi, jotka on otettu käyttöön.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmät ja muita tietoja siitä, miten muutosten jäljityksen kerättyjä tietoja.

| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Windows|![Kyllä](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Kyllä](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Ei](./media/log-analytics-change-tracking/oms-bullet-red.png)|            ![Ei](./media/log-analytics-change-tracking/oms-bullet-red.png)|![Kyllä](./media/log-analytics-change-tracking/oms-bullet-green.png)| HOURLY|

## <a name="use-change-tracking"></a>Muutoslokin käyttäminen

Kun ratkaisu on asennettu, voit tarkastella muutoksia yhteenveto valvottu palvelinten **Muutosten jäljityksen** -ruutua käyttämällä OMS **Yleiskatsaus** -sivulla.

![Muutosten jäljityksen ruudun kuva](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

Voit tarkastella muutoksia infrastruktuuri ja sitten Poraudu kyselyjä tiedot seuraavat luokat:

- Kirjoita muutokset määrityksestä ohjelmistojen ja palveluiden Windows
- Ohjelmiston muutoksista sovelluksia ja päivittää yksittäisten palvelinten
- Ohjelmiston muutokset kunkin sovelluksen kokonaismäärä
- Windows palvelumuutoksista yksittäisiä palvelimia

![Muutosten jäljityksen Raporttinäkymät-ikkunan kuva](./media/log-analytics-change-tracking/oms-changetracking01.png)

![Muutosten jäljityksen Raporttinäkymät-ikkunan kuva](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>Voit tarkastella muutoksia mille tahansa muuttaa tyyppi

1. Valitse **sivulla** Napsauta **Muutosten jäljityksen** -ruutua.
2. **Muuta seuranta** -koontinäytössä Tarkista yhteenvetotiedot jossakin näiden Muuta tyyppi ja valitse sitten jokin voit tarkastella yksityiskohtaisia tietoja **log haun** sivulle.
3. Mihin tahansa log etsintäsivuja näet tulokset aika, yksityiskohtaiset tulokset ja log hakujen. Voit myös suodattaa muita tarkentamalla hakutuloksia mukaan.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Log rajaamalla Log Analytics](log-analytics-log-searches.md) avulla voit tarkastella yksityiskohtaisia muutosten jäljityksen tiedot.
