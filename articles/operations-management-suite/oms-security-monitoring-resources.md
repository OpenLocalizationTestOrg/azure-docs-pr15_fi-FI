<properties
   pageTitle="Resurssien valvominen toimintojen hallinta Suite suojaus ja valvonta-ratkaisun | Microsoft Azure"
   description="Tämän asiakirjan avulla voit käyttää OMS suojaus ja valvonta-ominaisuuksia, resurssien ja tunnistaa suojaus."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Resurssien toimintojen hallinta Suite suojaus ja valvonta-ratkaisun valvominen

Tämän asiakirjan avulla voit seurata resurssien ja tunnistaa suojaus OMS suojaus ja valvonta-ominaisuuksien avulla.

## <a name="what-is-oms"></a>Mikä on OMS?

Microsoft toimintojen hallinta Suite (OMS) on Microsoft cloud perusteella IT management-ratkaisun, jonka avulla voit hallita ja suojata paikallisen oman ja cloud infrastruktuurin. Lisätietoja OMS Lue artikkeli [Toimintojen hallinta Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Resurssien valvominen

Aina, kun on mahdollista, sinun kannattaa tapahtuu ensiksi suojauksen tapaukset estää. On kuitenkin mahdotonta Estä kaikki suojauksen tapaukset. Kun jokin aktivoitu, sinun on varmistaa, että sen vaikutus on pienennetty.  On kolmen tärkeän suositukset, jonka avulla voit pienentää numero ja suojauksen tapaukset vaikutus:

- Arvioi säännöllisesti heikkouksien ympäristössä.
- Tarkista kaikki tietokonejärjestelmien ja varmistaa, että ne ovat kaikki asennettu uusimmat korjaukset verkkolaitteet säännöllisesti.
- Tarkista kaikki lokit ja kirjaaminen keinoja, kuten käyttöjärjestelmän tapahtumalokien, sovelluksen tietyn lokit ja tunkeutumisen tunnistus järjestelmälokit säännöllisesti.

OMS suojaus ja valvonta-ratkaisun avulla IT seurannassa aktiivisesti kaikille resursseille, jotka voivat auttaa pienentää suojauksen tapaukset vaikutus. OMS suojaus ja valvonta on suojaus-toimialueet, joita voi käyttää resurssien valvominen. Suojaus-toimialueiden tarjoaa nopean pääsyn asetukset-suojauksen seurannasta seuraavat toimialueet kuvattu enemmän tietoja:

- Haittaohjelmien arviointi
- Päivitä arviointi
- Tunnistetietojen ja käytön

> [AZURE.NOTE] Yleiskatsaus nämä asetukset Lue [aloittaminen toimintojen hallinta Suite suojaus ja valvonta-ratkaisun](oms-security-getting-started.md).

### <a name="monitoring-system-protection"></a>Valvonta järjestelmän suojaus

Linnaa syvyys vaiheelta jokaisen kerroksen suojaus on tärkeää oman resurssi yleinen suojaus tila. Tietokoneissa, joissa havaittiin uhkien ja tietokoneissa, joissa ei riitä suojauksen näkyvät haittaohjelman arviointi ruudun Suojaus toimialueet-kohdassa. Haittaohjelman arvioinnin tietojen avulla voit tunnistaa suunnitelma suojauksen ottaminen käyttöön palvelimiin, jotka tarvitse sitä. Jos haluat käyttää tämä asetus, noudata seuraavia ohjeita:

1. Valitse **Microsoft toimintojen hallinta Suite** tärkeimmät Raporttinäkymät-ikkunan **Suojaus ja valvonta** -ruutu.

    ![Suojaus ja valvonta](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Valitse **Suojaus ja valvonta** Raporttinäkymät-ikkunan **Antimalware arviointi** **Suojauksen toimialueet**-kohdassa. **Antimalware arviointi** Raporttinäkymät-ikkunan näkyy alla kuvatulla tavalla:

![Haittaohjelmien arviointi](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

**Haittaohjelmien arviointi** Raporttinäkymät-ikkunan avulla voit tunnistaa suojauksen seuraavat asiat:

- **Aktiivinen uhkien**: tietokoneissa, joissa on käsiin ja on aktiivinen uhkien järjestelmässä.
- **Remediated uhkien**: tietokoneissa, joissa on käsiin, mutta uhkien on remediated.
- **Allekirjoituksen ajan tasalla**: tietokoneissa, joissa on käytössä haittaohjelmilta, mutta allekirjoitus ei ole ajan tasalla.
- **Reaaliaikainen ei suojaus**: tietokoneissa, joihin ei ole asennettu antimalware.

### <a name="monitoring-updates"></a>Päivitysten seuranta 

Uusimmat päivitykset on paras käytäntö ja se olisi sisällytettävä yrityksen päivityksen hallinnan määrittäminen. Microsoft Agent seuranta-palvelun (HealthService.exe) lukee tiedot valvottu tietokoneista ja lähettää päivitetyt tiedot OMS palvelun pilveen käsittelyä varten. Microsoft Agent seuranta-palvelu on määritetty automaattinen palvelu ja se suoritetaan aina kohde tietokoneeseen.

![päivitysten seuranta](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logiikan käytetään tietojen päivitystapoja ja pilvipalvelussa tietueiden tietoja. Jos puuttuvat päivitykset on useita, ne näkyvät **päivitykset** Raporttinäkymät-ikkunan. Voit **päivittää** Raporttinäkymät-ikkunan käyttäminen puuttuvat päivitykset ja suunnitteleminen koske palvelimissa, jotka tarvitset niitä. Noudata seuraavia ohjeita voit käyttää **päivitykset** Raporttinäkymät-ikkunan:

1. Valitse **Microsoft toimintojen hallinta Suite** tärkeimmät Raporttinäkymät-ikkunan **Suojaus ja valvonta** -ruutu.
2. Valitse **Suojaus ja valvonta** Raporttinäkymät-ikkunan **Päivityksen arviointi** **Suojauksen toimialueet**-kohdassa. Päivitä Raporttinäkymät-ikkunan näkyy alla kuvatulla tavalla:

![Päivitä arviointi](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

Tämä raporttinäkymät-ikkunassa voit suorittaa päivityksen arvio ymmärtää tietokoneiden nykyisen tilan ja osoite kannalta uhkien. **Kriittinen tai päivitykset** -ruudun avulla IT-järjestelmänvalvojille voi käyttää yksityiskohtaisia tietoja päivitykset, joita ei ole alla kuvatulla tavalla:

![hakutulokset](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Tämä raportti sisältää tärkeitä tietoja, joilla voidaan tunnistaa uhkien, joka sisältää Microsoft KB päivitys- ja MS-Bulletin, joka on lisätietoja haavoittuvuuden liittyvät artikkelit ongelmalle järjestelmän tyyppi.

### <a name="monitoring-identity-and-access"></a>Tunnistetietojen ja käytön valvominen

Ja käyttäjät käyttävät missä tahansa, käyttämällä eri laitteissa ja käyttää paljon cloud ja paikallisen sovellukset on välttämätöntä, että käyttöoikeudet on suojattu. Tunnistetiedon varkaus kalastelu ovat niitä, jossa luvaton alun perin pääsee käyttämään tavallisen käyttäjän käyttöoikeudet verkoston järjestelmän. Monta kertaa alkuperäinen hyökkäys on vain tapa avata verkossa, ultimate lähinnä tuttuihin käyttöoikeuksien tilit. 

Hyökkääjät säilyvät verkkoon, käyttämällä vapaasti käytettävissä tooling tunnistetiedot noutamiseen-on muiden tilien istuntoja. Järjestelmämääritysten mukaan näitä tunnistetietoja voit purkaa hajautusarvot, liput tai jopa tekstimuotoisia salasanoja muodossa.  

> [AZURE.NOTE] koneet, joka on määritetty suoraan Internet näkee useita epäonnistui yritykset, yritä kirjautua sisään käyttämällä kaikkien tyyppisten tunnetun käyttäjänimet (kuten järjestelmänvalvoja). Useimmissa tapauksissa kannattaa OK, jos tunnetun käyttäjänimet ei käytetä ja salasana on riittävän.

On mahdollista tunnistaa nämä tunkeilijat, ennen kuin he havaittu käyttöoikeuksien tilin. Voit hyödyntää **OMS suojaus ja valvonta-ratkaisun** valvoa käyttäjätieto- ja käyttö. Noudata seuraavia ohjeita voit käyttää **tunnistetietojen ja käytön** Raporttinäkymät-ikkunan:

1. Valitse **Microsoft toimintojen hallinta Suite** tärkeimmät Raporttinäkymät-ikkunan Suojaus ja valvonta-ruutu.
2. Valitse **Suojaus ja valvonta** Raporttinäkymät-ikkunan **tunnistetietojen ja käytön** **Suojaus toimialueet**-kohdassa. **Käyttäjätieto- ja Access** -koontinäytön näkyy alla kuvatulla tavalla:

![tunnistetietojen ja käytön](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Osana yrityksen säännöllisesti seurannan määrittäminen täytyy sisältää tunnistetietojen seuranta. IT-järjestelmänvalvoja pitäisi näyttää, onko tietyn kelvollinen käyttäjänimi, jolla on monta yritykset. Tämä saattaa ilmaista joko voi saada, joka on hankittu reaali käyttäjänimi ja yritä palvelinten tai automaattinen työkalu käyttää koodattu salasanan, joka on vanhentunut.

Raporttinäkymät-ikkunan Salli se tunnistaa nopeasti liittyviä käyttäjätieto- ja yrityksen resurssien käytön mahdollisia tietoturvauhkia. Se on tietyn tärkeää tunnistaa mahdolliset trendejä myös, esimerkiksi kirjautumiset päälle aika-ruutu näkyviin ajanjakson aikana, kuinka monta kertaa epäonnistuneen suoritettiin. Tässä tapauksessa tietokoneen **tiedostopalvelin** vastaanotti 35 kirjautuminen yritykset. Voit tutkia tarkempia tietoja tämän tietokoneen napsauttamalla sitä. 

![Lisätietoja](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Tämän tietokoneen luodaan raportti näyttää tätä mallia tärkeitä tietoja. Huomannut, **tili** -sarakkeen avulla voit käyttäjätili, jolla on käytetty yrittää käyttää järjestelmää, **TIMEGENERATED** -sarakkeen avulla voit jossa yritys määritetty aikaväli ja **LOGONTYPENAME** -sarakkeen avulla voit sijainti, johon yritys määritetty. Jos nämä yritykset on tehty järjestelmän paikallisesti, ohjelma, **prosessi** -sarakkeen näyttää prosessin nimi. Mistä Kirjautumisyritys peräisin ohjelman tilanteissa sinulla on jo käytettävissä prosessin nimi ja nyt voit suorittaa edelleen tutkimuksen kohde-järjestelmässä.

## <a name="see-also"></a>Katso myös

Tässä asiakirjassa oppimiasi käyttämisestä OMS suojaus ja valvonta-ratkaisun seurannassa resurssit. Lisätietoja OMS tietoturvasta on seuraavissa artikkeleissa:

- [Toimintojen hallinta Suite (OMS) yleiskatsaus](operations-management-suite-overview.md)
- [Toimintojen hallinta Suite suojaus ja valvonta-ratkaisun käytön aloittaminen](oms-security-getting-started.md)
- [Seuranta ja niihin vastaaminen suojausvaroitusten toimintojen hallinta Suite suojaus ja valvonta ratkaisu](oms-security-responding-alerts.md)