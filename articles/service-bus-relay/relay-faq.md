<properties 
    pageTitle="Usein kysyttyjä Kysymyksiä välittää | Microsoft Azure"
    description="Vastataan joihinkin Azure välitys vastauksia usein kysyttyihin kysymyksiin."
    services="service-bus"
    documentationCenter="na"
    authors="jtaubensee"
    manager=""
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub" />

# <a name="relay-faq"></a>Välitys usein kysytyt kysymykset

Tässä artikkelissa vastataan joihinkin Microsoft Azure välitys vastauksia usein kysyttyihin kysymyksiin. Voit myös käydä Azure hinnoittelua ja tukea yleistiedot [Azure tukevat usein kysytyt kysymykset](http://go.microsoft.com/fwlink/?LinkID=185083) . Sisältyvät on seuraavissa artikkeleissa:

- [Yleisiä kysymyksiä Azure välitys](#general-questions)
- [Hinnat](#pricing)
- [Kiintiön](#quotas)
- [Tilauksen ja nimitilan hallinta](#subscription-and-namespace-management)
- [Vianmääritys](#troubleshooting)

## <a name="general-questions"></a>Yleisiä kysymyksiä

### <a name="what-is-azure-relay"></a>Mikä on Azure välitys?

[Välityspalvelu](relay-what-is-it.md) mahdollistaa läpinäkyvä isännöidä ja käyttää WCF-palveluiden missä tahansa. Toisin sanoen näin hybrid-sovellukset, jotka suoritetaan Azure palvelinkeskuksen ja paikallisen yritysympäristössä.

### <a name="what-is-a-relay-namespace"></a>Mikä on välitys nimitilan?

[Nimitilan](Relay-create-namespace-portal.md) tarjoaa säilöön osoitteiden välitys resurssien sovelluksessa. Luominen on käytettävä välitys ja päivitetään jokin ensimmäiset vaiheet käytön aloittaminen.

## <a name="pricing"></a>Hinnat

Tässä osassa on vastauksia joihinkin hinnat rakenteen välitys vastauksia usein kysyttyihin kysymyksiin. Voit myös vierailla Microsoft Azure hinnoittelu yleistiedot [Azure tuki usein kysytyt kysymykset](http://go.microsoft.com/fwlink/?LinkID=185083) . Välitys hinnoittelua valmis tietoja on artikkelissa [palvelun Bus hinnoittelua tiedot](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="how-do-you-charge-for-relay"></a>Miten voit veloittaa välitys?

Lisätietoja välitys hinnoittelua, katso [palvelun Bus hinnoittelua tiedot][Pricing overview]. Lisäksi merkille hinnat perittävän juniin ulkopuolella tietokeskuksen, jossa sovellus on valmisteltu siirrot liittyvät tiedot.

### <a name="what-usage-of-relay-is-subject-to-data-transfer"></a>Mitä käyttö välitys peritään tiedonsiirto?

Välitys sisältää 5 gt tietojen tunkeutumisen tilauskohtaisten kuukaudessa. Ei muita Azure tunkeutumisen/juniin maksutonta välitys käyttämät tiedot.

Välitys tietojen maksu koskee vain lähettäjien tunkeutumisen, kuin välitys kuuntelijoita ei maksamaan tietojen kulun. Esimerkiksi jos lähetät 1 gt, vain laskutetaan 1 gt, vaikka kuuntelija on myös vastaanotettu 1 gt, ja se voi olla Azure's palvelinkeskusten ulkopuolella.

### <a name="how-are-relay-hours-calculated"></a>Miten välitys tuntia lasketaan?

Välitys tuntia ovat laskuttaa kumulatiivinen ajanjakson aikana kunkin välitys on "avoinna" laskutuksen tiettynä aikana. Välitys on implisiittisesti esiintymää ja avataan annetun nimitilan kun välitys kuuntelija muodostaa ensin osoite. Välityksen on suljettu, vain, kun viimeinen listener katkaisee osoite. Vuoksi laskutuksen tarkoituksiin välitys pidetään "Avaa" aika ensimmäisen välitys kuuntelua yhdistää ja viimeisen välitys kuuntelua katkaisee nimitilan ajaksi. Toisin sanoen välitys pidetään Avaa aina, kun yksi tai Lisää välitys kuuntelijoita muodostanut yhteyden palvelun Bus osoite.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-given-relay"></a>Entäpä jos käytössä on useampi kuin yksi yhdistetty annetun välitys listener?

Joissakin tapauksissa yksittäisen välitys voi olla useita yhdistetyn kuuntelijoita. Välitys pidetään "Avaa", kun se on yhdistetty vähintään yksi välitys listener. Lisää kuuntelijoita lisääminen Avaa välitys johtaa Lisää välitys tuntia. Välitys lähettäjien (asiakkaiden, joka käynnistää tai lähettää viestejä releitä) määrän yhteydessä välitys myös ei ole vaikutusta välitys tunnin laskeminen.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Miten viestit mittarin lasketaan WCF releitä?

**Tämä koskee vain WCF releitä ja ei ole Hybrid yhteyksien kustannus**

Yleensä laskutettavan viestien lasketaan releitä menetelmällä se kohteiden (olevien, aiheet ja tilaukset) yllä olevien ohjeiden mukaisesti. On kuitenkin useita keskeisiä eroja:

Lähettää viestin Service-Bus välitys on tekstiarvoina, joka vastaanottaa Lähetä palvelun Bus välitys sijaan viestin välitys kuuntelutoiminto "koko kautta" Lähetä perään toimittamista välitys kuuntelua. Vuoksi pyynnön vastaus tyylin palvelun käynnistäminen (enintään 64 Kilotavun) vastaan välitys kuuntelija johtaa kaksi laskutettavan viestien: yksi laskutettavan viestin pyynnön ja vastauksen yhden laskutettavan viestin (olettaen vastaus on myös \<= 64 Kilotavua). Tämä eroaa mediate asiakkaan ja palvelun välillä jonon avulla. Jälkimmäisessä-pyynnön vastaus samoissa edellyttäisi jonossa ja sen jälkeen dequeue/toimituksen jonossa palveluun perään vastauksen lähettämisen ja toinen jono ja dequeue/toimituksen kyseisen jonon asiakkaalle pyyntö Lähetä. Samalla (\<= 64 Kilotavua) kokoa koko perustiedot, sovitetuista jonon kuvion näin johtaa neljä laskutettavan viestien kahdesti laskuttaa toteuttamisesta käyttämällä välitys samoissa numero. Liittyy etuja saavuttamiseksi tätä mallia, kuten kestävyyttä ja kuormituksen tasaus olevien avulla. Seuraavat edut voivat oikeuttaa muita kulut.

Releitä, joka avataan netTCPRelay WCF sidonta Käsittele viestejä ei yksittäisten viestien mutta kuin stream tietojen juoksutus järjestelmän kautta. Toisin sanoen vain lähettäjä ja listener on yksittäiset kehysten näkyvyys viestit lähetetään/vastaanotettu tämän sidonnan avulla. Näin ollen releitä käyttämällä netTCPRelay sidonta, saat kaikki tiedot tulkitaan stream laskettaessa laskutettavan viestejä. Tässä tapauksessa palvelun Bus laskee kokonaismäärä tiedot lähetetään tai vastaanotetuista tekstiviesteistä kunkin yksittäisiä välitys 5 minuutin välein ja, yhteensä jaetaan 64 Kilotavua jotta määrittää kyseisen ajanjaksolla poistettavaan välitys laskutettavan viestien määrä.

## <a name="quotas"></a>Kiintiön

|Kiintiön nimi|Laajuus|Tyyppi|Ylitti toiminta|Arvo|
|---|---|---|---|---|
|Samanaikainen kuuntelijoita-välitys|Kohteen|Staattinen|Lisää yhteyksien seuraavien pyyntöjen hylätään ja poikkeuksen vastaanotetaan puheluja koodilla.|25|
|Samanaikainen välitys kuuntelijoita|Järjestelmää|Staattinen|Lisää yhteyksien seuraavien pyyntöjen hylätään ja poikkeuksen vastaanotetaan puheluja koodilla.|2 000|
|Samanaikainen välitys yhteyksien kaikki välitys päätepisteet palvelun nimitila|Järjestelmää|Staattinen|-|5 000|
|Välitys päätepisteet palvelun nimitila kohden|Järjestelmää|Staattinen|-|10 000|
|Välittää viestin koko [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) ja [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx)|Järjestelmää|Staattinen|Saapuviin viesteihin, jotka ylittävät näiden kiintiöiden hylätään ja poikkeuksen vastaanotetaan puheluja koodilla.|64 KILOTAVUA
|Välittää viestin koko [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) ja [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx)|Järjestelmää|Staattinen|-|Rajoittamaton tallennus|

### <a name="does-relay-have-any-usage-quotas"></a>Onko välitys minkä tahansa käyttökiintiön?

Oletusarvon mukaan mitään cloud palvelun Microsoft määrittää kooste kuukausittain käyttökiintiö, joka lasketaan kaikissa asiakkaan tilaukset. Koska Ymmärrämme, voit joutua yli nämä raja-arvot, ota yhteyttä asiakaspalvelun milloin tahansa niin, että voimme ymmärtävät tarpeesi ja säädä nämä raja-arvot asianmukaisesti. Palvelun Bus koostetietoja kiintiöt ovat seuraavat:

- 5 miljardia viestit
- 2 miljoonaa välitys tunnit

Kun pidättää oikeuden käytöstä, joka on ylittänyt sen käyttökiintiön tietyn kuukauden asiakastilin, syy antaa sähköposti-ilmoituksen ja tehdä useita muodostaa yhteyden asiakkaan ennen toimenpiteiden. Suurempi kuin näiden kiintiöiden asiakkaiden on vastuussa, jotka ylittävät kiintiöiden kuluja.

#### <a name="naming-restrictions"></a>Nimeämiskäytännön

Välitys nimitilan nimi voi olla vain välillä 6 – 50 merkkiä.

## <a name="subscription-and-namespace-management"></a>Tilauksen ja nimitilan hallinta

### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Miten nimitilan siirtäminen toiseen Azure-tilaukseen?

Voit käyttää PowerShell-komennoilla (artikkelissa [tähän](../service-bus-messaging/service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription)), siirry Azure yksi tilaus nimitilan toiseen. Jotta voit suorittaa toiminnon, nimitila on jo oltava aktiivisena. Myös komentojen käyttäjän on oltava järjestelmänvalvoja, sekä lähde- ja tilaukset.

## <a name="troubleshooting"></a>Vianmääritys

### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-their-suggested-actions"></a>Mitä Azure välitys ohjelmointirajapinnan ja niiden ehdotetut toimenpiteet poikkeukset?

[Välitys poikkeukset] [ Relay exceptions] artikkelissa poikkeuksia ehdotetut toimenpiteet kanssa.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Mikä on jaettu Access-allekirjoitus ja mitkä kielet tukee allekirjoituksen luodaan?

Jaetun Access-allekirjoitukset ovat SHA – 256 suojatun hajautusarvot tai URI perusteella todennus-järjestelmä. Lisätietoja siitä, miten voit luoda oman allekirjoitukset solmu ja PHP, Java C\#, on artikkelissa [Jaettujen Access allekirjoitukset][] .

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Jaettu käyttö allekirjoitukset]: service-bus-sas-overview.md

## <a name="next-steps"></a>Seuraavat vaiheet:

- [Luo nimitila](relay-create-namespace-portal.md)
- [.NET käytön aloittaminen](relay-hybrid-connections-dotnet-get-started.md)
- [Solmun käytön aloittaminen](relay-hybrid-connections-node-get-started.md)