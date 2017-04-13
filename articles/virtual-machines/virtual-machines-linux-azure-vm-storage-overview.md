<properties
  pageTitle="Azure ja tallennustilaa Linux AM | Microsoft Azure"
  description="Kuvataan Azure vakio- ja Premium tallennustilan Linux näennäiskoneiden kanssa."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines-linux"
  authors="vlivech"
  manager="timlt"
  editor=""/>

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="NA"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure"
  ms.date="10/04/2016"
  ms.author="v-livech"/>

# <a name="azure-and-linux-vm-storage"></a>Azure ja Linux AM-tallennustilan

Azure-tallennustila on cloud tallennustilan ratkaisu Moderni sovelluksia, jotka käyttävät kestävyyttä, käytettävyys ja skaalattavuus asiakkaidensa tarpeisiin.  Lisäksi tekeminen mahdollista kehittäjät voivat luoda uusia skenaarioita tukemaan suurissa sovelluksia Azuren tallennustilaan sekä tallennustilan foundation Azuren näennäiskoneiden.

## <a name="azure-storage-standard-and-premium"></a>Azuren tallennustila: Vakio- ja Premium

Azure AM on suunniteltu vakio tallennustilan levyjen tai premium tallennustilan levyjä.  Kun portaalin avulla voit valita, että AM on Vaihda avattava perusteet näytössä tarkastelemiseen vakio- ja premium levyjä.  Seuraavassa näyttökuvassa korostaa Näytä tai piilota valikon.  Kun napsautettu Suoritettaessa, vain premium tallennustilan näytetään käytössä VMs, kaikki palautettua mukaan Suoritettaessa asemat.  Kun napsautettu kiintolevylle, vakio tallennustilan käyttöön VMs varmuuskopioidut pyörivä levyasemien näytetään sekä premium tallennustilan VMs palautettua Suoritettaessa mukaan.

  ![screen1](../virtual-machines/media/virtual-machines-linux-azure-vm-storage-overview/screen1.png)

AM-luotaessa `azure-cli` voit valita vakio- ja premium valittaessa AM koon kautta `-z` tai `--vm-size` cli merkinnän.

### <a name="create-a-vm-with-standard-storage-vm-on-the-cli"></a>Luo AM vakio tallennustilan AM cli käyttöön

Cli merkinnän `-z` valitsee Standard_A1 A1 kanssa on vakio tallennustilan perusteella Linux AM.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-on-the-cli"></a>Luo AM premium cli-tallennustilan

Cli merkinnän `-z` valitsee Standard_DS1 kanssa DS1 parhaillaan premium-tallennustilan perusteella Linux AM.

```bash
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Vakio-tallennustilan

Azure vakio-tallennustila on oletustietotyyppi lisätallennustilaa.  Vakio tallennus on edullinen on edelleen performant.  

## <a name="premium-storage"></a>Premium-tallennustilan

Azure Premium tallennustilan toimittaa tehokas, pieni viive levyn tuki näennäiskoneiden voin/O-paljon toiminnoista. Virtual machine (AM)-levyjä, jotka käyttävät Premium tallennustilan tallentaa tietoja tasainen tilan asemista (SSDs). Voit siirtää sovelluksen AM levyjen Azure Premium tallennustilan tulevat hyödyntää nopeuden ja näiden levyjen suorituskykyä.

Tallennustilan erikoisominaisuuksia:

- Premium tallennustilan levyjen: Azure Premium tallennustilan tukee AM-levyjä, jotka voidaan liittää DS, DSv2 tai GS sarjan Azure VMs.

- Premium Blob-objektien: Premium tallennustilan tukee sivun Azure BLOB, joita käytetään pidä pysyvä levyjä, Azuren näennäiskoneiden (VMs).

- Premium paikallisesti tarpeettomat tallennustila: Premium-tallennustilan tilin vain tukee paikallisesti tarpeettomat Storage (LRS) replikoinnin-asetus ja säilyttää tiedot kolme kopiota yhden alueen.

- [Premium-tallennustilan](../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Premium tallennustilan tuettu VMs

Premium tallennustilan tukee DS-sarjan, DSv2 sarjan GS sarjan ja Fs sarjan Azuren näennäiskoneiden (VMs). Voit vakio- ja Premium tallennustilan levyjen Premium tallennustilan VMs on tuettu. Mutta et voi käyttää tallennustilan Premium levyjen AM sarjan, jotka eivät ole yhteensopivia Premium-tallennustilan.

Seuraavassa on Linux jaot, joka on vahvistettu Premium tallennustilan kanssa.

| Jakauman. | Versio                 | Tuetut ydin    |
|--------------|-------------------------|---------------------|
| Ubuntu       | 12.04                   | 3.2.0-75.110+       |
| Ubuntu       | 14.04                   | 3.13.0-44.73+       |
| Debian       | 7.x, 8.x                | 3.16.7-ckt4-1+      |
| SLES         | SLES 12                 | 3.12.36-38.1+       |
| SLES         | SLES 11 SP4             | 3.0.101-0.63.1+     |
| CoreOS       | 584.0.0+                | 3.18.4+             |
| Centos       | 6.5 6.6 6,7, 7.0, 7.1 | 3.10.0-229.1.2.el7+ |
| RHEL         | 6.8 +, 7.2 +              |                     |


## <a name="file-storage"></a>Tiedostojen tallennus

Azure tiedostojen tallentamisesta on tiedostoresurssit pilveen vakio SMB-protokollan avulla. Azure-tiedostoja voit siirtää enterprise-sovelluksia, jotka käyttävät Azure tiedostopalvelinten. Azure-sovellukset voi helposti ottaa tiedostoresurssit Azuren näennäiskoneiden Linux. Ja tiedostojen tallentamisesta uusimman version, jossa voit liittää jaetun tiedoston paikallisen-sovelluksesta, joka tukee SMB 3.0.  Koska tiedostoresurssit SMB osakkeet, voit käyttää niitä vakio tiedostojärjestelmän ohjelmointirajapinnan kautta.

Tiedostojen tallentamisesta on muodostettu samaan tekniikkaan kuin Blob, taulukon ja jonon tallennustilaa, niin tallennus on käytettävyyden, kestävyyttä, skaalattavuus ja geo-redundancy, joka on rakennettu Azure tallennustilan-ympäristössä. Lisätietoja tiedostojen tallennustilan suorituskyvyn kohteet ja rajoitukset on artikkelissa Azure-tallennustilan skaalattavuus ja suorituskyvyn kohteet.

- [Voit käyttää Azure tiedostosäilön Linux](../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Kuuma tallennustila

Azure kuuma tallennustilan taso on tarkoitettu, jota käytetään usein tietojen tallentaminen.  Kuuma tallennus on blob stores storage oletuslajin.

## <a name="cool-storage"></a>Hieno tallennustila

Azure hienoja tallennustilan taso on tarkoitettu tiedot, jotka ovat harvoin käytetyt ja pitkäikäiset tallentamiseen. Esimerkki Käytä laatikkomäärät hienoja tallennustilan ovat varmuuskopiot, mediasisältöä, tieteellisen tiedon, vaatimustenmukaisuus ja arkistointia tietojen. Yleensä tiedot, joita käytetään harvoin on hienoja tallennustilan sopivaa perusavaimella.

|                             | Kuuma tallennustilan taso      | Hieno tallennustilan taso     |
|:----------------------------|:---------------------:|:---------------------:|
| Käytettävyys                | 99,9 %                 | 99 %                   |
| Käytettävyys (RA GRS lukee) | 99,99 %                | 99,9 %                 |
| Käyttökustannukset               | Tallennustilan kustannuksia  | Alempi tallennustilan kustannukset   |
|                             | Pienet käyttö          | Korkeampi käyttö         |
|                             | ja kustannukset | ja kustannukset |


## <a name="redundancy"></a>Redundancy

Microsoft Azure-tallennustilan tilin tiedot aina replikoida kestävyyttä ja suuren käytettävyyden, kokouksen Azure-tallennustilan SLA jopa menettämisen tapahtuessa lyhytkestoisia laitteisto varmistamiseksi.

Kun luot tallennustilan tilin, on valittava jokin seuraavista Replikointiasetukset:

- Paikallisesti tarpeettomat storage (LRS)
- Vyöhykkeen tarpeettomat storage (ZRS)
- GEO tarpeettomat storage (GRS)
- Lukuoikeudet geo tarpeettomat storage (RA GRS)

### <a name="locally-redundant-storage"></a>Paikallisesti ylimääräinen

Paikallisesti tarpeettomat storage (LRS) kopioi alueen, jonka loit tallennustilan tilin tiedot. Voit suurentaa kestävyyttä jokaisen pyynnön vastaan tallennustilan tilisi tiedot on replikoida kolme kertaa. Nämä kolme replikoiden kunkin sijaitsevat eri vika toimialueet ja Päivitä toimialueet.  Pyyntö palauttaa onnistuu vain, kun se on kirjoitettu kolme replikoita.

### <a name="zone-redundant-storage"></a>Vyöhykkeen ylimääräinen

Vyöhykkeen tarpeettomat storage (ZRS) kopioi tiedot yli kaksi tai kolme tarjoavia, vain yhden alueen sisällä tai eri kummallakin alueella suurempi kuin LRS kestävyyttä. Tallennustilan-tililläsi on käytössä ZRS, jos tiedot ovat kestävät jopa kyseessä jokin tilat virheen.

### <a name="geo-redundant-storage"></a>GEO ylimääräinen

GEO tarpeettomat storage (GRS) kopioi tietojen toissijainen alue, joka on satoja Mailia ensisijainen alueen ulkopuolella. Tallennustilan-tililläsi on käytössä GRS, jos tiedot ovat kestävät jopa kyseessä valmis alueellisen käyttökatkosta tai tietojen, jossa ensisijaisen alue ei voi palauttaa.

### <a name="read-access-geo-redundant-storage"></a>Lukuoikeudet geo tarpeettomat tallennustila

Lukuoikeudet geo tarpeettomat storage (RA GRS) suurentaa käytettävyys tallennustilan tilissäsi antamalla toissijaiseen sijaintiin lisäksi yli kummallakin alueella GRS myöntämä replikoinnin tietojen vain luku-tilassa. Silloin, kun tietoja ei ole käytettävissä ensisijainen alueen, sovellus voi lukea toissijainen alueen olevia tietoja.

Saat perinpohjaisesti käsittelevään artikkeliin Azure tallennustilan redundancy Katso:

- [Azure tallennustilan sallittuja](../storage/storage-redundancy.md)

## <a name="scalability"></a>Skaalattavuus

Azuren tallennustila on erittäin skaalattava, joten voit tallentaa ja prosessi satoja, teratavua tietojen tukemaan big datasta käyttötavoista vaatii tiede-, analyysi- ja media-sovellukset. Tai voit tallentaa small business-sivuston tarvittavat tiedot jonkin verran. Sitä tarpeisiisi kuuluvat maksat vain tallennettavien tietojen. Azure-tallennustilan tällä hetkellä tallentaa trillions yksilöllinen asiakkaan objektien kymmenlukuun, ja käsittelee pyynnöt sekunnissa miljoonia keskimäärin.

Vakio tallennustilan tilien: Vakio tallennustilan-tilillä on 20 000 IOPS yhteensä pyynnön korkokannan. Yhteensä IOPS kaikissa virtuaalikoneen levyjen vakio tallennustilan tilillä korkeintaan rajoitusta.

Premium-tallennustilan tilin: premium-tallennustilan tilin on suurin yhteensä siirtonopeuden, 50 Gbps. Yhteensä siirtonopeuden kaikissa AM levyjen korkeintaan rajoitusta.

## <a name="availability"></a>Käytettävyys

Olemme takaa, että vähintään 99,99 % (99,9 % hienoja Access-tason) ajan, emme onnistuneesti käsittelee pyynnöt tietojen lukuoikeus Geo tarpeettomat Storage (RA GRS) tileiltä lukemiseen, epäonnistunut yrittää lukea ensisijainen alueen yritetään uudelleen toissijainen alueen.

Olemme takaa, että vähintään 99,9 % (99 % hienoja Access-tason) ajan, emme onnistuneesti käsittelee pyynnöt voi lukea tietoja paikallisesti (LRS tarpeettomat tallennustilan), vyöhykkeen tarpeettomat Storage (ZRS) ja Geo tarpeettomat Storage (GRS)-tilit.

Olemme takaa, että vähintään 99,9 % (99 % hienoja Access-tason) ajan, emme onnistuneesti käsittelee tietojen kirjoittaminen paikallisesti (LRS tarpeettomat tallennustilan), vyöhykkeen tarpeettomat Storage (ZRS), ja Geo tarpeettomat Storage (GRS)- ja lukuoikeudet Geo tarpeettomat Storage (RA GRS) tilien pyynnöt.

- [Azure SLA tallennuksessa](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)


## <a name="regions"></a>Alueiden

Azure on yleisesti saatavilla eri puolilla maailmaa 30 alueilla ja on ilmoitettiin suunnitelmien 4 muita alueiden. Maantieteelliset laajentamista on Azure prioriteetin, koska se mahdollistaa sen asiakkaiden saavuttamiseksi paremman suorituskyvyn ja tukevat niiden vaatimukset ja tietojen sijainti koskevat asetukset.  Azures käynnistää uusimman alue on saksa.

Microsoft Cloud saksa on Microsoft Cloud services-jo käytettävissä eriteltyjen mahdollisuus kaikkialla Euroopassa, parantavat mahdollisuuksia innovaatiot ja taloudellisen kasvun erittäin säännellyillä kumppaneiden ja asiakkaiden luominen saksa, Euroopan unionin (EU) ja Euroopan liitos (EFTA).

Uusi-palvelinkeskusten Magdeburg ja Frankfurt, asiakkaan tietoja hallitaan tietojen luottamussuhde, T-Systemsin kansainvälisen, riippumaton Saksan yrityksen sekä Saksan Telekom tytäryhtiöiden hallinnassa. Microsoftin kaupallista pilvipalveluihin nämä palvelinkeskusten noudattaa Saksan tietojen käsittely asetusten ja antaa asiakkaiden lisävaihtoehtoja, miten ja missä tiedot käsitellään.


- [Azure alueiden kartta](https://azure.microsoft.com/regions/)

## <a name="security"></a>Tietoturva

Azuren tallennustila on kattavaa tietoturvaominaisuudet, jotka mahdollistavat yhdessä kehittäjät voivat luoda suojatun sovelluksia. Tallennustilan tilin itse voidaan suojata Roolipohjainen käyttöoikeuksien valvonta ja Azure Active Directoryn. Tietoja voi suojata siirron sovelluksen ja Azure välillä käyttämällä asiakaspuolen salausta, HTTPS tai SMB 3.0. Tietoja voidaan määrittää salata automaattisesti, kun kirjoitettu Azuren tallennustilaan tallennustilan palvelun salaus (SSE) avulla. OS ja tietojen levyjen näennäiskoneiden käyttämä voi määrittää salata Azure salauksen. Tieto-objektit Azuren tallennustilaan valtuutetun pääsy voi myöntää jaettu Access allekirjoitusten käyttäminen.

### <a name="management-plane-security"></a>Hallinnan tasossa suojaus

Hallinnan tasossa koostuu resursseja tallennustilan tilin hallinta. Tässä osassa käsittelemme Azure resurssien hallinnan käyttöönottomalli ja Roolipohjainen käyttöoikeuksien valvonta (RBAC) avulla voit hallita tallennustilan tilit. Käsittelemme myös tallennustilan tilin avaimien ja miten ne uudelleen.

### <a name="data-plane-security"></a>Tasossa tietoturva

Tässä osiossa tarkastellaan salliminen todellisten tietojen-objektien tallennustilaan tiliisi, kuten BLOB-objektit, tiedostot tai olevien taulukoiden jaettu Access allekirjoitukset ja tallennettu käytäntöjen avulla. Käsiteltävät aiheet palvelun tason Suojaussidosten ja tilin tason Suojaussidokset. Olemme näkyy myös poistamisesta rajoittaa IP-osoite (tai IP-osoitteiden alue), rajoittamisesta HTTPS protokolla ja kumota jaettu Access allekirjoituksen odottamatta se päättyy.

## <a name="encryption-in-transit"></a>Siirron salaus

Tässä osassa käsitellään tietojen suojaamisesta, kun siirrät siihen sisään tai ulos Azure-tallennustilan. Käsittelemme HTTPS ja käyttää SMB 3.0 Azure tiedostoresurssit salaus suositellut käytön. On myös siirtää katsaus asiakaspuolen salausta, joiden avulla voit salata tiedot, ennen kuin se on siirretty asiakassovellus varastoon ja salauksen tiedot, kun se siirretään loppunut.

## <a name="encryption-at-rest"></a>Salauksen Rest-palvelussa

Käsittelemme tallennustilan palvelun salaus (SSE) ja miten voit ottaa sen käyttöön tallennustilan tilin tuloksena on estä BLOB-objektit, sivu-BLOB- ja liitä BLOB parhaillaan salattuja automaattisesti, kun kirjoitettu Azure-tallennustilan. On myös näyttää, miten voit käyttää Azure salauksen ja tutustu basic erot ja SSE ja asiakkaan salaus ja salauksen tapauksissa. Olemme näyttää lyhyesti osoitteessa FIPS yhteensopivuuden US Government-tietokoneissa.

- [Azure tallennustilan opas](../storage/storage-security-guide.md)

## <a name="cost-savings"></a>Kustannusten säästöjen

- [Tallennustilan kustannukset](https://azure.microsoft.com/pricing/details/storage/)

- [Tallennustilan kustannusten laskuri](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Tallennustilan rajoitusten

- [Palvelun tallennustilarajojen](../azure-subscription-service-limits.md#storage-limits)
