<properties
   pageTitle="Skenaariot ja esimerkkejä tilauksen hallinnon | Microsoft Azure"
   description="On esimerkkejä ottamisesta käyttöön Azure tilauksen hallinnon Yleisiä tilanteita, joissa varten."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="rdendtler"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="rodend;karlku;tomfitz"/>

# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Esimerkkejä Azure yrityksen scaffold toteuttaminen

Tämä artikkeli sisältää esimerkkejä siitä, kuinka yritys voi ottaa käyttöön [Azure yrityksen scaffold](resource-manager-subscription-governance.md)suosituksia. Tiedostossa käytetään niminen ja Contoso kuvitteelliseen yritykseen havainnollistetaan Yleisiä tilanteita, joissa parhaat käytännöt. 

## <a name="background"></a>Tausta

Contoso on maailmanlaajuinen yritys, joka tarjoaa toimituksen ketju ratkaisuja asiakkaiden kaikki pakattu malliin "ohjelmiston as a Service-mallin käyttöön paikallisen.  Ne kehittää ohjelmiston kanssa merkittäviä kehittäminen keskikohdan mukaan Intia, Yhdysvalloissa ja Kanadassa maapallo yli. 

Yrityksen ISV-osa on jaettu useita riippumaton liiketoimintayksiköitä, joilla hallitaan tuotteista merkittäviä Business-palvelussa. Yritystietojen yksittäisen on oma kehittäjät, tuotteen johtajat ja architects. 

Yrityksen tekniikkaa (ETS) business yksikkö tarjoaa keskitetyn IT-ominaisuuksien ja hallitsee useita tietojen keskukset, joiden liiketoimintayksiköitä isäntä verkkosovelluksistaan. Sekä hallinta tietojen keskikohdan mukaan-ETS organisaation tarjoaa ja hallitsee keskitetystä yhteiskäyttö (kuten sähköpostin ja sivuston) ja verkon ja palveluja. Ne myös hallinta asiakkaalle suunnatussa työmääriä pienempi liiketoimintayksiköitä, joilla ei ole toiminnassa henkilökunnan. 

Tässä ohjeaiheessa voi käyttää seuraavia mukana seuraavat henkilöt:

- ETS Azure-järjestelmänvalvoja on Dave.
- Anneli on Contoson johtaja, kehityksen toimituksen ketju business yksikköä. 

Contoso on liiketoiminta-sovellus ja asiakkaan osoittava-sovelluksen luominen. Se on päättänyt suorittaa Azure sovellukset. Dave lukee [Saat tilauksen hallinnon](resource-manager-subscription-governance.md) aihetta ja on nyt valmis toteuttamaan suositukset. 

## <a name="scenario-1-line-of-business-application"></a>Tapaus 1: liiketoiminta-sovellus

Contoso on muodostaminen tietolähteen koodin hallintajärjestelmän (BitBucket) käytettävän kehittäjät kaikkialla maailmassa.  Sovellus käyttää infrastruktuurin serviceksi (IaaS) isännöimiseen ja koostuu WWW-palvelimien ja tietokantapalvelimeen. Kehittäjät voivat käyttää palvelinten kehittäminen ympäristöissä, mutta niitä ei tarvitse Azure-palvelimet. Contoson ETS haluaa Salli sovelluksen omistaja ja ryhmän hallinta sovellus. Sovellus on käytettävissä vain aikana Contoson yrityksen verkossa. Dave on määritetty tämän sovelluksen tilaus. Tilauksen myös isännöidä developer liittyvät muiden ohjelmien tulevaisuudessa.  

### <a name="naming-standards--resource-groups"></a>Standardit ja resurssiryhmät nimeäminen

Dave luo tilauksen tukemaan Kehitystyökalut käytettävissä olevia kirjasimia kaikki liiketoimintayksiköitä yli. Hän on luotava kuvaavat nimet tilauksen ja resurssin ryhmien (sovelluksen sekä verkkoja). Hän luo tilauksen ja resurssin seuraavista ryhmistä:

| Kohteen | Nimi | Kuvaus |
| ---- | ---- | ----------- |
| Tilauksen | Contoson ETS DeveloperTools tuotannon | Tukee yleisiä Kehitystyökalut |
| Resurssiryhmä | rgBitBucket | Sisältää sovelluksen verkkopalvelin ja tietokantapalvelin |
| Resurssiryhmä | rgCoreNetworks | Sisältää virtual verkkojen ja sivusto yhdyskäytävän yhteys |


### <a name="role-based-access-control"></a>Roolipohjainen käyttöoikeuksien valvonta

Kun olet luonut hänen tilauksen, Dave haluaa varmistaa, että tarvittavat ryhmiä ja sovelluksen omistajat voivat käyttää resursseille. Dave tunnistaa kunkin ryhmän on erilaisia vaatimuksia. Hän käyttää ryhmiä, jotka on synkronoitu Contoson paikallisen Active Directory (AD)-Azure Active Directory ja tarjoaa oikean käyttöoikeustaso ryhmiin. 

Dave määrittää seuraavia rooleja tilauksen: 

| Rooli | Liitetty | Kuvaus |
| ---- | ----------- | ----------- |
| [Omistaja](./active-directory/role-based-access-built-in-roles.md#owner) | Tunnus, jota hallitaan Contoson Active | Tämä tunnus ohjataan kanssa vain kerran Tarpeeseen Accessin Contoson jäsenyyksien hallinta-työkalun ja varmistaa, että tilauksen omistaja access tarkistetaan kokonaan. |
| [Suojauksen hallinta](./active-directory/role-based-access-built-in-roles.md#security-manager) | Tietoturva ja risk management osasto | Tämä rooli avulla käyttäjät voivat Azure Tietoturvakeskus ja resurssien tila. |
| [Verkon avustaja](./active-directory/role-based-access-built-in-roles.md##network-contributor) | Verkkoryhmän jäseneltä | Tämä rooli sallii Contoson verkkoryhmän jäseneltä VPN-sivuston-sivusto ja Virtual verkostojen hallinta. |
| *Mukautettu rooli* | Sovelluksen omistaja | Dave luo rooli, voivat muokata resursseja resurssiryhmän. Lisätietoja on artikkelissa [Azure RBAC mukautettu rooleille](./active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Käytännöt

Dave on tilaus resurssien hallinta seuraavat vaatimukset:

- Kehitystyökalut tue kehittäjät kaikkialla maailmassa, hän ei haluat estää käyttäjiä luodaan resursseja minkä tahansa alueella. Kuitenkin hänen tarvitsee tietää, missä resurssit luodaan. 
- Hän on kyseisen kustannuksia. Tämän vuoksi hän haluaa estää sovelluksen omistajat luomasta tarpeettoman kallista näennäiskoneiden.  
- Koska tätä sovellusta palvelee kehittäjät-useita liiketoimintayksiköitä, hän haluaa tunnisteen kullekin resurssille liiketoiminnan yksikkö ja sovelluksen omistajan kanssa. Käyttämällä tunnisteet ETS voit laskuttaa tarvittavat ryhmiä.

Hän luo seuraavat [Resurssienhallinta käytännöt](resource-manager-policy.md): 

| Kenttä | Vaikutus | Kuvaus |
| ----- | ------ | ----------- |
| sijainti | valvonta | Audit resurssit kaikki alueen luominen |
| tyyppi | Estä | Estä G sarjan näennäiskoneiden luominen |
| tunnisteet | Estä | Edellyttää sovelluksen omistaja-tunniste |
| tunnisteet | Estä | Vaadi kustannukset center-tunniste |
| tunnisteet | liittäminen | Liittää tunnistenimi **liiketoimintayksikköön** ja tunnistearvo **ETS** kaikille resursseille |


### <a name="resource-tags"></a>Resurssin tunnisteet

Dave ymmärtää, että hän on oltava tunnistavan BitBucket käyttöönoton kustannuspaikka laskun tietoja. Lisäksi Dave haluaa tietää, joka ETS omistaa resurssit. 

Hän lisää seuraavat [tunnisteet](resource-group-using-tags.md) resurssiryhmien ja resurssien. 
 
| Tunnistenimi | Tunnistearvo |
| -------- | --------- |
| ApplicationOwner | Henkilö, joka hallinnoi tämän sovelluksen nimi. |
| CostCenter | Ryhmä, joka on maksaminen Azure kulutus kustannuspaikka. |
| Liiketoimintayksikköön | **ETS** (tilaukseen liittyvää business-yksikkö) |

### <a name="core-network"></a>Core verkossa

Contoson ETS tietoja suojaus ja risk management ryhmän tarkistaa Daven ehdotettu suunnitelma Siirry Azure sovelluksen. Käyttäjät haluavat varmistaa, että sovellus on määritetty ei Internetiin.  Dave on myös developer-sovellukset, jotka myöhemmin siirretään Azure. Nämä sovellukset edellyttävät julkisen liittymät.  Täytä näitä vaatimuksia, hän antaa sekä sisäisten ja ulkoisten virtual verkkojen ja verkon käyttöoikeusryhmän rajoittamisesta.

Hän luo on seuraavissa resursseissa:

| Resurssilaji | Nimi | Kuvaus |
| ------------- | ---- | ----------- |
| VPN | vnInternal | BitBucket-sovelluksen käyttämisestä ja ExpressRoute kautta liitetyn Contoson yrityksen verkkoon.  Aliverkon (sbBitBucket) sisältää sovelluksen toisistaan välilyönnillä tietyn IP-osoite. |
| VPN | vnExternal | Käytettävissä tulevien sovellukset edellyttävät julkiselle päätepisteet. |
| Verkko-käyttöoikeusryhmän | nsgBitBucket | Voit varmistaa tämän työtehtävistäsi toiselle työntekijälle uhanalaisten pienennetään sallimalla vain porttiin 443 aliverkon missä se sijaitsee (sbBitBucket)-yhteydet. |

### <a name="resource-locks"></a>Resurssien lukitukset 

Dave tunnistaa, että yhteys Contoson yrityksen verkosta virtual sisäisen verkon suojattu mistä tahansa wayward komentosarja tai vahingossa. 

Hän luo seuraavat [resurssin lukitus](resource-group-lock-resources.md):

| Lukitustyyppi | Resurssi | Kuvaus |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnInternal | Estää käyttäjiä poistamasta VPN- tai aliverkosta, mutta ei estä uuden aliverkosta lisäys. |

### <a name="azure-automation"></a>Azure automaatio 

Dave tyhjän automatisoida tämän sovelluksen. Vaikka hän on luonut Azure automaatio-tiliä, hän ei käytä sitä alun perin. 

### <a name="azure-security-center"></a>Azure Tietoturvakeskus 

Contoson IT hallinnan on tunnistaa nopeasti ja käsittele uhkien. He haluavat myös ymmärtää, mitä ongelmia saattaa olla.  

Täyttämään näitä vaatimuksia Dave mahdollistaa [Azure Tietoturvakeskuksessa](./security-center/security-center-intro.md)ja suojauksen hallinnan rooli käyttö. 

## <a name="scenario-2-customer-facing-app"></a>Tapaus 2: asiakkaan osoittava app

Toimituksen ketju business yksikössä business-johtajuus on tunnistanut eri mahdollisuudet niin, että välitys Contoson asiakkaiden kanssa käyttämällä kanta kortti. Anneli käyttäjän on luotava tämä sovellus ja päättää Azure kasvaa mahdollisuus liiketoiminnallista tarvetta kokous. Anneli toimii Dave-ETS määrittäminen kaksi tilausten kehittämällä ja tämän sovelluksen kanssa.

### <a name="azure-subscriptions"></a>Azure tilaukset 

Dave kirjautuu sisään Azure-yritysportaalin ja näkee toimituksen ketju osasto jo olemassa.  Tämä projekti on ensimmäisen kehittäminen projektin toimitusten ketju ryhmän Azure, Dave tunnistaa Anneli henkilön ryhmälle uuden tilin edellyttää.  Hän luo "Tutkimus ja kehitys" tilin hänen ryhmän ja määrittää access päästä. Anneli kirjautuu sisään kautta portaalin tilin ja luo kaksi tilaukset: yksi kehittäminen-palvelimia ja yksi pitoon tuotannon-palvelimiin.  Hän tapahtuu aiemmin muodostettu nimeämiskäytäntö standardeja luotaessa seuraavista tilauksista: 

| Tilauksen käyttö | Nimi |
| ---------------- | ---- |
| Kehittäminen | SupplyChain ResearchDevelopment LoyaltyCard kehittäminen |
| Tuotannon | SupplyChain toimintojen LoyaltyCard tuotannon |

### <a name="policies"></a>Käytännöt

Dave ja Anneli keskustella sovelluksen ja määritä, että tämä sovellus toimii vain Pohjois-Amerikan alueella olevat asiakkaat.  Anneli ja hänen ryhmän aiot käyttää Azure's sovelluksen Service-ympäristön ja Azure SQL sovelluksen luominen. Ne on ehkä luoda näennäiskoneiden kehityksen aikana.  Anneli haluaa varmistaa, että hänen kehittäjät on kiinni tarvittaessa tutkia ja tutki ilman tietojen ETS resurssit. 

**Kehittäminen tilauksen**he luovat seuraavaa käytäntöä:

| Kenttä | Vaikutus | Kuvaus |
| ----- | ------ | ----------- |
| sijainti | valvonta | Audit alueet resursseista luomista. |

Ne eivät rajoita tuote, käyttäjä voi luoda kehitteillä tyypin ja ne eivät edellytä resurssiryhmien ja resurssien tunnisteet.

**Tuotannon tilauksen**he luovat käytettävissään seuraavat käytännöt:

| Kenttä | Vaikutus | Kuvaus |
| ----- | ------ | ----------- |
| sijainti | Estä | Estä ulkopuolella Yhdysvaltojen tiedot keskukset resurssien luomista. |
| tunnisteet | Estä | Edellyttää sovelluksen omistaja-tunniste |
| tunnisteet | Estä | Edellytä osaston tunniste. | 
| tunnisteet | liittäminen | Liitä tunniste kunkin resurssiryhmän, joka ilmaisee tuotantoympäristössä. |

Älä rajoita tuote, käyttäjä voi luoda tuotannon tyyppi.

### <a name="resource-tags"></a>Resurssin tunnisteet 

Dave ymmärtää, että hän on oltava eivätkä omistajuus oikea liiketoiminta-ryhmän tunnistamiseen tiettyihin tietoihin. Hän määrittää resurssin tunnisteet resurssiryhmiä ja resursseja. 
 
Tunnistenimi | Tunnistearvo |
| -------- | --------- |
| ApplicationOwner | Henkilö, joka hallinnoi tämän sovelluksen nimi. |
| Osasto | Ryhmä, joka on maksaminen Azure kulutus kustannuspaikka. |
| EnvironmentType | **Tuotannon** (Vaikka tilaus sisältää **tuotannon** nimessä, tämän tunnisteen mukaan lukien mahdollistaa helppo tunnistaminen kun katsoo resurssien portaalissa tai laskussa.) |

### <a name="core-networks"></a>Core verkot

Contoson ETS tietoja suojaus ja risk management ryhmän tarkistaa Daven ehdotettu suunnitelma Siirry Azure sovelluksen. He haluavat varmistaaksesi, että kanta kortti-sovellus on oikein eristää ja suojattu DMZ verkossa.  Tämä vaatimus täyttämään Dave ja Anneli luoda ulkoisen virtual verkon ja verkon käyttöoikeusryhmän eristää Contoso yrityksen verkosta kanta kortti-sovelluksen.  

**Kehittäminen tilauksen**ne luominen:

| Resurssilaji | Nimi | Kuvaus |
| ------------- | ---- | ----------- |
| VPN | vnInternal | Toimii Contoso kanta kortin kehitysympäristö ja ExpressRoute kautta liitetyn Contoson yrityksen verkkoon. |

**Tuotannon tilauksen**ne luominen:

| Resurssilaji | Nimi | Kuvaus |
| ------------- | ---- | ----------- |
| VPN | vnExternal | Isännöi kanta kortti-sovellus ja ei ole liitetty suoraan Contoson ExpressRoute. Koodi on miten kautta niiden lähdekoodin järjestelmän suoraan PaaS-palveluiden avulla. |
| Verkko-käyttöoikeusryhmän | nsgBitBucket | Voit varmistaa tämän työtehtävistäsi toiselle työntekijälle uhanalaisten pienennetään sallimalla vain-sidottujen viestintä-TCP 443.  Contoso on myös raportoitua Web Application palomuurin käyttäminen muilla tavoilla. |  

### <a name="resource-locks"></a>Resurssien lukitukset 

Dave ja Anneli perusteella ja lisäät resurssien lukitukset johonkin tärkeiden resurssien estää vahingossa aikana automaatiokoodia koodin push-ympäristössä. 

He luovat lukituksen seuraavasti:

| Lukitustyyppi | Resurssi | Kuvaus |
| --------- | -------- | ----------- |
| **CanNotDelete** | vnExternal | Jos haluat estää käyttäjiä poistamasta VPN tai aliverkosta. Lukitse ei estä uuden aliverkosta lisäys. |

### <a name="azure-automation"></a>Azure automaatio 

Anneli ja hänen ryhmälle on laaja runbooks hallittavan tämän sovelluksen ympäristössä. Runbooks Salli lisäys/poisto solmujen ja DevOps muihin tehtäviin. 

Jos haluat käyttää näitä runbooks, niiden avulla [Automation](./automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure Tietoturvakeskus 

Contoson IT hallinnan on tunnistaa nopeasti ja käsittele uhkien. He haluavat myös ymmärtää, mitä ongelmia saattaa olla.  

Näiden tarpeiden täyttämiseksi Dave mahdollistaa Azure Tietoturvakeskuksessa. Hän varmistaa, että Azure Tietoturvakeskus valvoo resurssit ja DevOps ja suojaus-ryhmien käyttö. 

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Resurssienhallinta-mallien luomisesta on artikkelissa [Azure Resurssienhallinta-mallien luomiseen parhaita käytäntöjä](resource-manager-template-best-practices.md).

*Ohjeaihe on lähettänyt [Karl Kuhnhausen](https://github.com/karlkuhnhausen) .*
