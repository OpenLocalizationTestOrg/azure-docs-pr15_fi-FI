<properties
   pageTitle="Johdanto Azure Tietoturvakeskus | Microsoft Azure"
   description="Lisätietoja Azure Tietoturvakeskus, sen tärkeimpiin ominaisuuksiin ja miten se toimii."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Johdanto Azure Tietoturvakeskus

Lisätietoja Azure Tietoturvakeskus, sen tärkeimpiin ominaisuuksiin ja miten se toimii.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.

## <a name="what-is-azure-security-center"></a>Mikä on Azure Tietoturvakeskus?
 Tietoturvakeskus auttaa estämään, haku- ja vastata uhkien näkyvyyden kyselyjä ja hallita Azure resurssien suojaus. Siinä on integroitu suojaus seuranta ja käytäntöjen hallinta Azure-tilauksissa, auttaa tunnistamaan uhkien, jotka muuten huomaamatta ja laaja valikoima security-ratkaisujen mediatyökaluja toimii.

##  <a name="key-capabilities"></a>Tärkeimmät ominaisuudet
 Tietoturvakeskus toimittaa helposti käytettävällä ja tehokkaan uhkien estäminen tunnistaminen ja vastaus ominaisuuksia, jotka ovat sisäisiä Azure. Tärkeimmät ominaisuudet ovat:

| | |
|----- |-----|
| Estä | Valvoo Azure resurssien suojaus-tila |
| | Määrittää jäsenyydelle ryhmissä perusteella yrityksen suojausvaatimukset ja Azure tilaukset, jota käytät sovellukset ja tietojen suojaustasoa tyypit |
| | Käytännön perustuva käyttää suojausta koskevia suosituksia apuviivaan palvelun omistajat käyttöönoton vaiheet tarvittavat ohjausobjektit |
| | Ottaa nopeasti käyttöön suojaustoimintoja ja laitteiden Microsoftin ja kumppanien |
| Tunnista |Kerää ja analysoi suojaustietoja Azure resursseja, verkon ja kumppaniratkaisuihin, kuten antimalware ohjelmat ja palomuurit automaattisesti |
| | Yleinen leverages uhkien liiketoimintatietojen Microsoftin tuotteista ja palveluista, Microsoft digitaalisen rikokset yksikkö (DCU), Microsoftin Security vastauksen Center (MSRC) ja ulkoiset syötteet |
| | Koskee kehittyneen analyysin, mukaan lukien koneen oppiminen ja käytöspiirteen analyysi |
| Vastaa | Sisältää Priorisoidut tapaukset/suojausvaroitusten |
| | Tarjoaa tietoja kyselyjä hyökkäyksen tai sellaisiin resurssit |
| | Lopeta nykyinen hyökkäyksen ja estää tulevien kalastelu ehdottaa |

## <a name="introductory-walkthrough"></a>Johdanto vaiheittainen kuvaus
 Voit käyttää Tietoturvakeskus [Azure portal](https://azure.microsoft.com/features/azure-portal/). [Kirjaudu sisään-portaaliin](https://portal.azure.com), valitse **Selaa**, ja selaamalla **Tietoturvakeskus** -vaihtoehto tai **Tietoturvakeskus** -ruutu, joka kiinnitetyt portaalin koontinäyttö.

![Azure-portaalissa suojaus-ruutu][1]

Suojauksen Centeristä suojauskäytäntöjen määrittäminen, valvoa suojausmäärityksiä ja tarkastella suojausvaroitusten.

### <a name="security-policies"></a>Suojauskäytäntöjä

Voit määrittää jäsenyydelle Azure tilaukset ja resurssiryhmien mukaan yrityksen suojausvaatimukset. Voit myös mukauttaa ne sovellukset, käytössäsi on erilaisia tai tiedot suojaustasoa jokaisen tilauksen. Esimerkiksi kehittäminen tai testaaminen käytettäviä resursseja voi olla eri kuin käytetyt tuotannon sovellusten suojausvaatimukset. Sovellusten vastaavasti säännellyillä tietoja, kuten henkilökohtaisia tietoja voi vaatia paremman suojauksen.

> [AZURE.NOTE] Voit muokata suojauskäytäntö tilauksen tasolla tai resurssin ryhmittelytason, on oltava tilauksen omistaja tai osallistujan siihen.

Valitse tilaukset ja resurssiryhmien luettelo **käytäntö** -ruutu **Tietoturvakeskus** -sivu.   

![Tietoturvakeskus-sivu][2]

Valitse tilaus, voit tarkastella käytännön **suojauskäytäntö** -sivu.

![Suojauksen käytännön sivu-tilaus][3]

**Tietojen kerääminen** (katso yllä) mahdollistaa tietojen kerääminen suojauskäytäntö varten. Ottaminen käyttöön on:

- Suojauksen seuranta ja suosituksia päivittäinen scanning kaikkien tukevat virtual koneet.
- Sivustokokoelman suojauksen tapahtumien analysointia ja uhkien tunnistamista varten.

**Valitse tallennustilan-tilin alueittain.** (katso yllä) voit valita kunkin alueen jonka sisältö on näennäiskoneiden-tallennustilan tilin näiden näennäiskoneiden kerättyjen tietojen tallennussijainti. Jos et valitse tallennustilan-tili kullekin alueelle, se luodaan puolestasi. Tiedot, jotka kerätään eristetään loogisesti muiden asiakkaiden tietojen tietoturvasyistä.

> [AZURE.NOTE] Tietojen kerääminen ja tallennustilaa-tilin alueittain valitsemalla on määritetty tilauksen tasolla.

Avaa **estävään käytäntöön** -sivu valitsemalla **estävään käytäntöön** (katso yllä). **Näytä suositukset,** voit valita, jota haluat seurata ja suosittelee turvaominaisuudet perusteella tilauksen piiriin kuuluvien resurssien suojauksen tarpeisiin.

Valitse seuraavaksi resurssiryhmä, voit tarkastella tietoja.

![Käytännön sivu resurssin käyttöoikeusryhmän][4]

**Periytyminen** (katso yllä) avulla voit määrittää resurssiryhmän nimellä:

- Peritään (oletus) eli kaikki suojauskäytäntöjä resurssin ryhmän periytyvät tilauksen taso.
- Yksilöivä eli resurssiryhmän on mukautettu suojauskäytäntö. Haluat tehdä muutoksia **Näytä suositukset**-kohdassa.

> [AZURE.NOTE] Jos näkyvissä on ristiriidassa tilauksen käytännöt ja resurssin tason Ryhmäkäytäntö, resurssi tason ryhmäkäytännön ohittaa.

### <a name="security-recommendations"></a>Suojausta koskevia suosituksia

 Tietoturvakeskus analysoi Azure resurssien tunnistaa mahdolliset tietoturva-aukkoja suojaus-tilaan. Suosituksia luettelo opastaa prosessin määrittämiseen tarvittavat ohjausobjektit. Esimerkkejä:

- Antimalware tunnistamista ja Poista haittaohjelmat valmistelu
- Verkon käyttöoikeusryhmät ja näennäiskoneiden liikenteen hallinta sääntöjen määrittäminen
- Web-sovellusten web application palomuurit suojata kalastelu vastaan, joiden kohteena valmistelu
- Käyttöönotto puuttuvat Järjestelmäpäivitykset
- Osoitteiden OS määritykset, jotka eivät vastaa suositeltu perusviivat

Valitse suosituksia luettelo **suositukset** -ruutu. Valitse kunkin suositus, voit tarkastella lisätietoja tai toimenpiteistä, voit ratkaista ongelman.

![Suojausta koskevia suosituksia Azure Tietoturvakeskuksessa][5]

### <a name="resource-health"></a>Resurssin kunto

**Resurssin suojaus kunto** -ruutu näyttää Yleinen suojaus reagoimisessa, ympäristön resurssin lajin, esimerkiksi näennäiskoneiden verkkosovellusten ja muita resursseja.   

Valitse resurssilaji **resurssin suojauksen kunto** -ruutu, voit tarkastella lisätietoja, kuten kaikki mahdolliset tietoturva-aukkoja, jotka on luettelo. (**Näennäiskoneiden** on valittuna alla olevasta esimerkistä.)

![Resurssien kunto-ruutu][6]

### <a name="security-alerts"></a>Suojausvaroitusten

 Tietoturvakeskus automaattisesti kerää, analysoi ja integroi Azure resursseja, verkon ja kumppaniratkaisuihin, kuten antimalware ohjelmat ja palomuurit lokitiedot. Kun uhkien havaitaan, suojausvaroituksen luodaan. Esimerkkejä tunnistaminen:

- Vaarantuneen näennäiskoneiden yhteydenpito tunnetut haittaohjelmien IP-osoitteet
- Lisäasetukset haittaohjelman tunnistaa käyttämällä Windowsin virheraportointi
- Palvelinten kalastelu näennäiskoneiden vastaan
- Suojausvaroitusten integroitu antimalware ohjelmista ja palomuurit

**Suojausvaroitusten** -ruutu valitsemalla näyttää luettelon Priorisoidut ilmoituksia.

![Suojausvaroitusten][7]

Lisätietoja hyökkäyksen ja miten se riskien parantaminen ehdotuksia valitsemalla ilmoituksen näyttää.

![Ilmoitusten suojaustietoja][8]

### <a name="partner-solutions"></a>Kumppaniratkaisuihin

**Kumppaniratkaisuihin** -ruudun avulla voit näytön yhdellä silmäyksellä kumppaniratkaisuihin kunnon tila integroitu Azure tilauksen. Tietoturvakeskus näkyvät ratkaisuja tulevat ilmoitukset.

Valitse **kumppaniratkaisuihin** -ruutu. Sivu avautuu ja siinä näkyvät kaikki yhdistetyn kumppaniratkaisuihin luettelo.

![Kumppaniratkaisuihin][9]

## <a name="get-started"></a>Käytön aloittaminen
Aloita Tietoturvakeskus, tarvitset Microsoft Azure-tilausta. Tietoturvakeskuksessa on käytössä Azure-tilaus. Jos sinulla ei ole tilaus, voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).

 Voit käyttää Tietoturvakeskus [Azure portal](https://azure.microsoft.com/features/azure-portal/). On lisätietoja [portaalin ohjeissa](https://azure.microsoft.com/documentation/services/azure-portal/) .

[Käytön aloittaminen Azure Tietoturvakeskus](security-center-get-started.md) ohjaa nopeasti läpi suojaus-seuranta ja hallinta osien Tietoturvakeskuksessa.

## <a name="see-also"></a>Katso myös
Tässä asiakirjassa on tuotu Tietoturvakeskus, sen tärkeimpiin ominaisuuksiin ja aloittamisesta. Lisätietoja on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Suojausta koskevia suosituksia Azure Tietoturvakeskuksessa hallinta](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon seuranta Azure Tietoturvakeskus](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) – Katso, miten voit hallita ja vastata suojausvaroitusten.
- [Kumppaniratkaisuihin ja Azure Tietoturvakeskus seuranta](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – uusimmista Azure suojaus ja tiedot.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
