<properties
    pageTitle="Kirjaudu Analytics usein kysytyt kysymykset | Microsoft Azure"
    description="Vastauksia usein kysyttyihin kysymyksiin Log Analytics-palvelu."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-analytics-faq"></a>Kirjaudu Analytics usein kysytyt kysymykset

Microsoft-FAQ on usein kysyttyjä kysymyksiä Log Analytics Microsoft toimintojen hallinta Suite (OMS) luettelo. Jos sinulla on lokin Analytics muita kysymyksiä, siirry [tuoteluettelon](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) ja lähettää kysymyksiä. Joku Microsoftin yhteisöltä avulla voit saada vastauksia. Jos kysymys usein kysyttyihin, emme lisää se tämän artikkelin niin, että se löytyvät nopeasti ja helposti.

## <a name="general"></a>Yleiset

**Kysymys: mitkä tarkistukset tehdään Mainos ja SQL-arviointi ratkaisuja?**

A. Seuraava kysely näyttää kaikki tarkistukset, jotka on tällä hetkellä suorittaa kuvaus:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Tulokset voidaan viedä Exceliin sitten myöhempää tarkastelua varten.

* *K: Miksi näen jotain muuta kuin *OMS* -SCOM Administration? **

A: sen mukaan, mitä Update Rollup, SCOM olet näkyviin voi tulla solmu *System Center Advisor*, *Toiminnalliset tiedot*tai *Lokin Analytics*.

Tekstin merkkijonon päivitys *OMS* sisältyy hallintapaketin, joka voi tuoda manuaalisesti. Uusimman SCOM Update Rollup Knowledge Base-artikkelin ohjeiden ja Päivitä näet uusimmat muutokset *OMS* -solmu OMS-konsolin.

* *K: onko *paikallisen* version OMS? **

V: ei. Log-analyysin käsittelee ja tallentaa hyvin suuria tietomääriä. Pilvipalveluun loki Analytics on asteikko-up tarvittaessa ja välttää minkä tahansa suorituskykyyn vaikuttavia tekijöitä ympäristön.

Myös että sinun ei tarvitse seurata Log Analytics-infrastruktuurin cloud palvelun keino ja käynnissä ja voi vastaanottaa usein käytetyt toimintojen päivitykset ja korjaukset.

## <a name="configuration"></a>Määritys
**Kysymys: Voit lukea from Azure diagnostiikka (WAD) käytettävän taulukon-Blob-objektien säilön nimen muuttaa?**  

A.  Tämä ei, ei ole tällä hetkellä mahdollista, mutta on suunniteltu uuteen versioon.

**Kysymys: mitä IP-osoitteiden tehdä OMS palvelujen käyttöä? Miten voin varmistaa, että palomuurin sallii vain liikenne OMS-palveluihin?**  

A. Lokitiedoston Analytics-palvelu rakentuu Azure ja päätepisteet vastaanottaa IP-osoitteet, jotka ovat [Microsoft Azure palvelinkeskuksen IP-alueita](http://www.microsoft.com/download/details.aspx?id=41653).

Kun palvelun käyttöönotoissa, todellinen IP-osoitteiden OMS palveluja muuttaminen. [Määritä välityspalvelin ja palomuuri loki Analytics](log-analytics-proxy-firewall.md)asetuksista on kuvattu DNS-nimien sallimaan palomuurin läpi.

**Kysymys: käytetään ExpressRoute Azure muodostamisesta. Lokitiedoston Analytics-liikenne käyttää ExpressRoute yhteys?**  

A. Erityyppiset ExpressRoute liikenne on kuvattu [ExpressRoute ohjeissa](./expressroute/expressroute-faqs.md#supported-services).

Log Analytics-tietoliikenteen käytetään julkisen peering ExpressRoute piiri.

**Kysymys: onko yksinkertainen ja helppo tapa siirtää aiemmin luodun lokin Analytics-työtilan Log Analytics-työtilan/Azure toiseen tilaukseen?**  Useita asiakkaan OMS työtilat, jotka on ollut testaus ja trialing on Microsoftin Azure tilaus- ja ne ovat siirtäminen tuotannon niin haluamme Siirry Azure/OMS oman tilauksen.  

A. `Move-AzureRmResource` Cmdlet-komento, voit siirtää Log Analytics-työtilan ja automaatio-tilin Azure tilauksesta välillä. Lisätietoja on artikkelissa [Siirrä AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Tämä muutos voi tehdä myös Azure-portaalissa.

Et voi siirtää tietoja Log Analytics-työtilasta toiseen tai muuttaa valitun alueen Log Analytics-tiedot tallennetaan.

**K: Miten voin lisätä OMS SCOM?**

A: päivittäminen uusin päivityskokoelma ja tuominen management Pack-paketit kokoustyötilaa voi muodostaa yhteyden SCOM lokin Analytics.

Huomaa, että loki Analytics SCOM yhteys on vain käytettävissä SCOM 2012 SP1 tai uudempi versio.

**K: miten voit Varmista, että agentti on pysty vaihtamaan Log Analytics kanssa?**

A:, jos haluat varmistaa, että agentti yhteydenpito OMS, siirry: Ohjauspaneeli, suojaus ja asetuksia, **Microsoft Agent seuranta**.

Etsi **Azure Log Analytics (OMS)** -välilehdessä vihreä valintamerkki. Vihreä valintamerkki vahvistaa, että agentti on pysty vaihtamaan OMS-palvelun kanssa.

Keltainen varoituskuvake tarkoittaa agentti aiheuttaa ongelmia tietoliikenteen OMS. Yksi yleinen syy on Microsoftin seuranta Agent-palvelu on pysäytetty ja on käynnistettävä.

**K: Miten voin lopettaa yhteydenpito Log Analytics-agentti?**

A: SCOM, valitse Poista tietokoneen OMS hallitun luettelosta. Pysäyttää kaikkea viestintää SCOM kautta, agentti. Varten yhdistetään OMS suoraan agenttien vuoksi, voit lopettaa ne-yhteydenpito OMS kautta: Ohjauspaneeli, suojaus ja asetuksia, **Microsoft Agent seuranta**.
**Azure Log Analytics (OMS)**, valitse Poista kaikki työtilat, jotka on lueteltu.

## <a name="agent-data"></a>Edustajan tiedot

**Kysymys: kuinka paljon tietoja voi lähettää kautta agentti Log Analytics? Onko enimmäismäärän tietoa asiakkaan kohden?**  
A. Vapaa suunnitelman asettaa päivittäinen pää 500 megatavua työtilan kohden. Vakio- ja premium-palvelupaketin oltava ei rajaa tietomäärää, joka on ladattu palvelimeen. Pilvipalveluun, kuin lokin Analytics-OMS suunniteltu automaattisesti käsittelemään äänenvoimakkuuden asteikko tulevat asiakkaan – vaikka se on päivässä teratavua.

Lokitiedoston Analytics-agentti on suunniteltu varmistaa, se on pieni vaatiman tallennustilan ja onko perustiedot hieman pakkausta. Yksi asiakkaidemme todella kirjoittamasi blogin ne suoritetaan Microsoftin agent ja siirretyt siitä, miten luot varmasti vaikutuksen testit. Tietoja äänenvoimakkuuden vaihtelee asiakkaille mahdollistaa ratkaisuja. Voit etsiä tietoja äänenvoimakkuuden yksityiskohtaiset tiedot ja nähdä tietosanomien mukaan ratkaisu **käyttö** -kohdassa kynän OMS yhteenveto-sivu.

Lisätietoja Lue [asiakkaan blogin](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) OMS edustajan pienen vaatiman tallennustilan tietoja.

**Kysymys: kuinka paljon kaistanleveyttä käytetään Microsoft Management agentti (MMA) mukaan, kun tietojen lähettäminen lokin Analytics?**

A. Kaistanleveys on lähetetyt tiedot Summa-funktiota. Tietojen pakkaus on, sillä se lähetetään verkon kautta.

**Kysymys: kuinka paljon tietoja lähetetä agentti kohden?**

A. Tämä salassa:

- olet ottanut ratkaisut
- lokit ja suorituskyvyn laskureita Kerätty määrä
- äänenvoimakkuuden tietojen lokit

Vapaa hinnoittelu taso on erinomainen tapa määrän useita palvelimia ja mittari tyypillinen data-asema. Yleinen käyttö näkyy **käyttö** -sivulla.
Tietokoneissa, joissa voivat suorittaa WireData-agentti näet, kuinka paljon tietoja lähetetään seuraava kysely:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja lokin Analytics ja ryhtyä käyttämään minuutteina [Log Analytics käytön aloittaminen](log-analytics-get-started.md) .
