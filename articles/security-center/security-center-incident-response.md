<properties
   pageTitle="Azure Tietoturvakeskus käytön tapauksen vastauksen | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten voit käyttää Azure Tietoturvakeskus vastauksen tapaus-skenaario."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>

# <a name="using-azure-security-center-for-an-incident-response"></a>Azure Tietoturvakeskus käytön tapauksen vastaus
Monet organisaatiot Opi vastata suojauksen tapaukset vasta kärsivät hyökkäys. Voit pienentää kustannuksia ja vahinko, on tärkeää on tapauksen vastauksen suunnittelemisesta on lisätty ennen hyökkäys tapahtuu. Voit käyttää Azure Tietoturvakeskus tapauksen vastauksen eri vaiheita.

## <a name="incident-response-planning"></a>Tapauksen vastauksen suunnittelu

Tehokas suunnitelman määräytyy kolme core ominaisuudet: ei voi suojata, haku- ja uhkien vastata. Suojaus on tietoja estämisestä tapaukset, tunnistus käsittelee uhkien tunnistaminen aikaisessa vaiheessa ja vastaus on evicting hän ja pienentämään vaikutukset, jonka järjestelmien palauttaminen.

Tässä artikkelissa käyttävät suojauksen tapauksen vastauksen vaiheet [Microsoft Azure suojauksen vastaus pilvessä](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) -artikkelista, kuten seuraavassa kaaviossa on esitetty:

![Tapauksen vastauksen elinkaari](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Voit käyttää Tietoturvakeskus vaiheissa tunnista Assess ja vianmääritys. Seuraavassa on esimerkkejä siitä, miten Tietoturvakeskus voi olla hyötyä kolme tapauksen vastauksesta vaiheissa:

- **Tunnista**: Tarkista tapahtuman tutkimuksen ensimmäinen merkintä.
    - Esimerkki: Tarkista, että tärkeäksi suojausvaroituksessa on korotettuna Tietoturvakeskus koontinäytön alkuperäinen vahvistus.
- **Assess**: alkuperäinen arvioiminen saadaksesi lisätietoja epäilyttävien tehtävän.
    - Esimerkki: Hanki lisätietoja suojausvaroitus.
- **Vianmääritys**: tekninen tutkimuksen järjestäminen ja sisältymisiä rajoituksen ja vaihtoehtoinen menetelmä selvittäminen.
    - Esimerkki: noudattamalla korjaus kuvaaman Tietoturvakeskus, erityisesti suojausvaroituksessa.

Tilanne, jossa seuraa avulla voit hyödyntää Tietoturvakeskus tunnista Assess ja vianmääritys ja vastata vaiheissa suojauksen tapahtumasta. Tietoturvakeskus [turvallisuutta](security-center-incident.md) on kooste resurssin kaikki ilmoitukset, joka [lopettaa ketju](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) kuvioilla tasata. Tapahtumat näkyvät [suojausvaroitusten](security-center-managing-and-responding-alerts.md) -ruutu ja sivu. Tapahtuma näyttää tietoruudussa liittyvät ilmoitukset-luettelo, jonka avulla voit saada lisätietoja jokaiselle esiintymälle. Tietoturvakeskus näkyy myös erillinen suojausvaroitusten, joita voi käyttää myös epäilyttävistä toimintojen avulla.

## <a name="scenario"></a>Skenaario

Contoson siirretty joitakin paikallisen resursseille viimeksi Azure, mukaan lukien kaikki virtuaalikoneen perustuva liiketoiminta-toiminnoista ja SQL-tietokannat. Contoson Core tietokoneen suojauksen tapahtumaa vastauksen ryhmän (CSIRT) on tällä hetkellä tutkiminen suojausongelmien vuoksi suojauksen liiketoimintatietojen ole integroitu niiden nykyinen tapauksen vastauksen työkaluilla ongelma. Tämä integrointi puuttuminen esitellään ongelma tunnista vaiheessa (liian monta tunnistettujen)- ja vaiheissa Assess ja vianmääritys. Osana tämän siirron ne päättänyt osallistua Tietoturvakeskus käyttäjille tämän ongelman.

Tämän siirron ensimmäisen vaiheen Valmis jälkeen ne onboarded kaikki resurssit ja kaikki Tietoturvakeskus suojaussuositukset. Contoson CSIRT on tietokeskuksen tietokoneen suojauksen tapaukset osalta. Ryhmän koostuu henkilöryhmän kanssa vastuu käsittely-ja minkä tahansa suojaus-tapahtumaa. Ryhmän jäsenten on selkeästi määritellyt tehtävät ei-vastauksen alue jää peittämättömästä.

Tässä skenaariossa varten tutustutaan kohdistus seuraavat mukana seuraavat henkilöt, jotka ovat osa Contoso CSIRT rooleille:

![Tapauksen vastauksen elinkaari](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Judy on suojaustoimintojen. Hänen Yleiset tehtävät ovat seuraavat:

- Seuranta- ja vastaa tietoturvauhkia ympäri.
- Escalating cloud kuormituksen omistaja tai suojauksen henkilön tarpeen mukaan.

SAM on suojaus-henkilön ja hänen tehtäviin kuulua muun muassa:

- Raportoitua kalastelu.
- Idfixin ilmoitukset.
- Kuormituksen omistajat voivat määrittää ja käyttää ongelman pienentämistavat käsitteleminen.

Kuten näet, Judy ja Sam on eri vastuut ja ne täytyy toimivat yhdessä Tietoturvakeskus tietojen jakamista varten.

## <a name="recommended-solution"></a>Suositeltu ratkaisu

Judy ja Sam on eri rooleja, koska ne voit käyttää eri alueiden Tietoturvakeskus hankkiminen päivittäinen toimintaa tärkeät tiedot. Judy käyttää **suojausvaroitusten** osana hänen päivittäinen seuranta.

![Suojausvaroitusten](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Judy käyttää suojausvaroitusten Jäljitä ja Assess vaiheissa. Judy päätyttyä alustava arviointi hän voi muuntaa Sam ongelman, jos muut tutkimuksen tarvitaan. Tässä vaiheessa Sam käyttää tietoja antamien mukaan Tietoturvakeskus, joskus muista lähteistä, siirry diagnosointi vaiheen yhdessä.


## <a name="how-to-implement-this-solution"></a>Tämä ratkaisu ottamisesta käyttöön

Nähdäksesi, kuinka vakiomittoja Azure Tietoturvakeskus tapauksen vastauksen tilanteessa, syy Judy's ohjeiden Jäljitä ja Assess vaiheissa ja katso, mitä Sam ongelman vianmääritystä.

### <a name="detect-and-assess-incident-response-stages"></a>Haku- ja arvioida tapauksen vastauksen vaiheet

Judy kirjautunut sisään Azure-portaaliin, ja se toimii Tietoturvakeskus konsolissa. Osana hänen päivittäisen toiminnan valvominen hän alkuun tarkistamista tärkeäksi suojausvaroitusten seuraavasti:

1. **Suojausvaroitusten** -ruutua ja käyttää **suojausvaroitusten** -sivu.
    ![Ilmoitusten Suojaus-sivu](./media/security-center-incident-response/security-center-incident-response-fig4.png)

    > [AZURE.NOTE] Tässä skenaariossa varten Judy suorittaminen suorittaa tehtävän haittaohjelmien SQL-ilmoituksen arvio tarkastelu edeltävässä kuvassa.
2. Valitse **haittaohjelmien SQL tehtävän** ilmoituksen ja tarkista hyökkäyksen kohteeksi joutuneen resursseista **tehtävän haittaohjelmien SQL** -sivu:  ![tapahtumaa tiedot](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    Tämä sivu Judy voi kestää huomautukset hyökkäyksen kohteeksi joutuneen resurssit, kuinka monta kertaa hyökkäys on tapahtunut, ja kun se on havaittu.
3. Valitse **hyökkäyksen kohteeksi resurssin** saadaksesi lisätietoja hyökkäys.

Kun olet lukenut kuvauksen, Judy on ovat vakuuttuneita siitä, että tämä ei ole väärä hälytys ja että hän on muuntaa tässä tapauksessa SAM.

### <a name="diagnose-incident-response-stage"></a>Tapauksen vastauksen vaiheen vianmääritys

SAM vastaanottaa kirjainkoon Judy ja käynnistää tarkistaminen korjaus vaiheet, jotka Tietoturvakeskus ehdotetut.

![Tapauksen vastauksen elinkaari](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Lisäresursseja

Tapauksen vastauksen ryhmän voit myös hyödyntää erilaisia raportteja Nähdäksesi [Security Center Power BI](security-center-powerbi.md) -ominaisuus. Näiden raporttien avulla ne edelleen tutkimuksen visualisointi, analysoida ja suodattaa suosituksia ja suojausvaroitukset aikana. Tutkimuksen aikana niiden suojaustietojen ja tapahtuman hallintaratkaisu (SIEM) käyttävät yritykset he voivat myös [integroida Tietoturvakeskus niiden-ratkaisuun](security-center-integrating-alerts-with-log-integration.md). Voit integroida Azure valvontalokien ja virtuaalikoneen (AM) suojaustapahtumia [Azure log integrointi-työkalun](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/)avulla. Tutkia hyökkäys, voit käyttää näitä tietoja tietoja, jotka Tietoturvakeskus yhdessä.


## <a name="conclusion"></a>Tekemistä

Kokoaminen työryhmän, ennen kuin tapahtuma ilmenee, on erittäin tärkeää organisaation ja vaikuttavat positiivisesti tapaukset käsittelytavan. Oikeiden työkalujen seurannassa resurssien ottaa auttaa tämän ryhmän tarkkoja korjausohjeet valitsemalla jokin riskien parantaminen. Tietoturvakeskus [tunnistaa](security-center-detection-capabilities.md) auttaa IT nopeasti suojausongelmia ja riskien parantaminen suojauksen ongelmat.
