<properties
   pageTitle="Azure Tietoturvakeskus pikaopas | Microsoft Azure"
   description="Tämän artikkelin avulla voit aloittaa nopeasti Azure Tietoturvakeskuksessa voit ohjata – suojaus seuranta- ja käytännön management-osat ja linkittäminen vaiheisiin."
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
   ms.date="10/28/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-quick-start-guide"></a>Azure Tietoturvakeskus pika-aloitusopas

Tämän artikkelin avulla voit nopeasti alkuun Azure Tietoturvakeskus luotsaa suojauksen valvonnan ja käytännön hallinta osat Tietoturvakeskus kautta.

> [AZURE.NOTE] Tässä artikkelissa esitellään palvelun Esimerkki käyttöönoton avulla. Tässä artikkelissa ei ole vaiheittaiset ohjeet.

## <a name="prerequisites"></a>Edellytykset

Aloita Tietoturvakeskus, jos sinulla on Microsoft Azure-tilausta. Jos sinulla ei ole tilaus, voit rekisteröityä [ilmaisen tilin](https://azure.microsoft.com/pricing/free-trial/).

Tietoturvakeskus vapaa taso on automaattisesti käytössä tilaus ja antaa näkyvyyden Azure resurssien suojaus-tilaan. Tarjoaa suojauksen hallinta, suojausta koskevia suosituksia ja integrointi suojaus-tuotteiden ja palvelujen Azure yhteistyökumppanien.

Voit käyttää Tietoturvakeskus [Azure portal](https://azure.microsoft.com/features/azure-portal/). Saat lisätietoja Azure portaalissa on [portaalin ohjeissa](https://azure.microsoft.com/documentation/services/azure-portal/).

## <a name="data-collection"></a>Tietojen kerääminen

Tietoturvakeskus kerää tietoja arvioida niiden suojaus-tilaan, anna suojausta koskevia suosituksia ja ilmoittaa uhkien oman näennäiskoneiden (VMs). Kun käytät Tietoturvakeskus, tietojen kerääminen on otettu käyttöön kaikissa VMs-tilaukseesi. Tietojen kerääminen suositellaan, mutta voit jättäytyä poistamalla käytöstä tietojen kerääminen Tietoturvakeskus käytännön.

Seuraavassa kuvataan, miten käyttää Tietoturvakeskus osat. Nämä vaiheet on esitellään, miten käytöstä tietojen kerääminen, jos haluat jättäytyä.

## <a name="access-security-center"></a>Access-Tietoturvakeskus

Portaalissa voit käyttää Tietoturvakeskus seuraavasti:

1. Valitse **Microsoft Azure** -valikossa **Tietoturvakeskuksessa**.
![Azure-valikko][1]

2. Jos käytät käyttöoikeuspalvelinta Tietoturvakeskus ensimmäistä kertaa, **Tervetuloa** -sivu avautuu. Valitse **Kyllä! Haluan julkaisun Azure Tietoturvakeskus** Avaa **Tietoturvakeskus** -sivu ja ota käyttöön tietojen kerääminen.
![Aloitusnäyttö][10]

3. Kun Käynnistä Tietoturvakeskus Tervetuloa-sivu ja valitse Tietoturvakeskus Microsoft Azure-valikosta, **Tietoturvakeskus** -sivu avautuu. **Tietoturvakeskus** sivu helposti myöhemmin, valitse **PIN-tunnuksen sivu raporttinäkymät-** vaihtoehto (oikeassa yläkulmassa).
![PIN-tunnuksen sivu dashboard-vaihtoehto][2]

## <a name="use-security-center"></a>Käytä Tietoturvakeskus

Voit määrittää suojauskäytäntöjä Azure tilaukset ja resurssiryhmät. Määrittää japanin suojauskäytäntö tilauksen:

1. Valitse **Tietoturvakeskus** -sivu **käytäntö** -ruutu.
![Suojauskäytäntö][3]

2. Valitse tilauksen **suojauskäytäntö – Määritä käytäntö kohti tilaus tai resurssiryhmä** -sivu.
3. **Tietojen kerääminen** on käytössä **suojauskäytäntö** , sivu voi kerätä automaattisesti lokit. Valitse kaikki nykyisen ja uuden tilauksen VMs on valmisteltu seurantaa tunniste. (Voit valita tietojen kerääminen ulos, **tietojen kerääminen** määrittämällä asetukseksi **ei käytössä**, mutta tämä estää Tietoturvakeskus suojausvalintaikkuna suojausvaroitusten ja suositukset.)
4. Valitse **Valitse tallennustilan-tilin alueittain** **suojauskäytäntö** -sivu. Kunkin alueen, jonka sisältö on virtuaalilaitteiksi Valitse kyseiset VMs kerättyjen tietojen tallennussijainti tallennustilan tilin. Jos et valitse tallennustilan-tili kullekin alueelle, se luodaan puolestasi. Tiedot, jotka kerätään eristetään loogisesti muiden asiakkaiden tietojen tietoturvasyistä.

     > [AZURE.NOTE] Suosittelemme, että Ota käyttöön tietojen kerääminen ja valitsemalla tallennustilan tilin, tilauksen tasolla ensin. Suojauskäytäntöjä voidaan määrittää resurssin ryhmittelytaso ja Azure tilauksen tasolla, mutta tietojen kerääminen ja tallennustilaa tilin tapahtuu vain tilauksen tasolla.

5. Valitse **estävään käytäntöön** **suojauskäytäntö** -sivu. **Estävään käytäntöön** -sivu avautuu.
![Estävään käytäntöön][4]

6. Käyttöön **estävään käytäntöön** -sivu, jonka haluat nähdä suojauskäytäntö osana suosituksia. Esimerkkejä:

 - **Järjestelmäpäivitykset** asettaminen **Valitse** tarkistaa kaikki tuetut näennäiskoneiden puuttuvat OS päivitykset.
 - Kaikki tuetut näennäiskoneiden tunnistaa kaikki OS määritykset, joka voi olla virtuaalikoneen alttiimpi **OS heikkouksien** asettaminen **Valitse** tarkistaa.

### <a name="view-recommendations"></a>Näytä suositukset

1. Palaa **Tietoturvakeskus** -sivu ja **suositukset** -ruutu. Tietoturvakeskus analysoi säännöllisesti Azure resurssien suojaus-tilaan. Kun Tietoturvakeskus havaitsee mahdollisen tietoturva-aukkoja, se näkyy suosituksia **suositukset** -sivu.
![Suosituksia Azure Tietoturvakeskuksessa][5]

2.  Valitse suositus **suositukset** -sivu, voit tarkastella tarkempia tietoja ja/tai toimenpiteistä, voit ratkaista ongelman.

### <a name="view-the-health-and-security-state-of-your-resources"></a>Resurssien terveys- ja tilan tarkasteleminen

1.  Palaa **Tietoturvakeskus** -sivu. **Resurssien suojaus kunto** -ruutu sisältää ilmaisimia suojauksen valtion näennäiskoneiden, verkko, tietoja ja sovelluksia.
2.  Valitse **näennäiskoneiden** voivat tarkastella tarkempia tietoja. **Näennäiskoneiden** -sivu avautuu ja näyttää yhteenvedon antimalware ohjelmat, järjestelmäpäivityksiä, käynnistyy ja OS heikkouksien, että VMs tila.
![Azure Tietoturvakeskuksessa resurssien kunto-ruutu][6]

3.  Valitse kohdassa **VIRTUAALIKONEEN SUOSITUKSIA** lisätietojen tarkasteleminen ja/tai toiminnon suositus määrittämiseen tarvittavat ohjausobjektit.
4.  Valitse kohdassa voit tarkastella lisätietoja **näennäiskoneiden** AM.

### <a name="view-security-alerts"></a>Näytä suojausvaroitusten

1.  Palaa **Tietoturvakeskus** -sivu ja valitse **suojausvaroitusten** -ruutu. **Suojausvaroitusten** -sivu avautuu ja näyttää luettelon ilmoitukset. Tietoturva ja verkon toiminnan Tietoturvakeskus analyysi Luo äänimerkkien. Integroitu kumppaniratkaisuihin ilmoitukset otetaan mukaan.
![Suojausvaroitusten Azure Tietoturvakeskuksessa][7]

    > [AZURE.NOTE] Suojausvaroitusten ovat käytettävissä vain, jos Tietoturvakeskus vakio taso on käytössä. 90 päivän maksuton kokeiluversio vakio taso on käytettävissä. Lisätietoja siitä, miten voit saada vakio taso on [seuraavat vaiheet](#next-steps) .

2.  Valitse ilmoituksen, voit tarkastella lisätietoja. Tässä esimerkissä Valitse **muokattu järjestelmän binaaritiedosto havaitsi**. Tämä avaa terät, joka on lisätietoja ilmoituksen.
![Ilmoitusten suojaustietoja Azure Tietoturvakeskuksessa][8]

### <a name="view-the-health-of-your-partner-solutions"></a>Kumppaniratkaisuihin kunto tarkasteleminen

1. Palaa **Tietoturvakeskus** -sivu. **Kumppaniratkaisuihin** -ruudun avulla voit valvoa yhdellä silmäyksellä, integroitu Azure tilauksesi kumppanin ratkaisujen kunto-tilaa.
2. Valitse **kumppaniratkaisuihin** -ruutu. Näyttöön avautuu ja näyttää luettelon kumppanin ratkaisujen Tietoturvakeskus yhteydessä.
![Kumppaniratkaisuihin][9]

3. Valitse kumppanin ratkaisu. Tässä esimerkissä Valitse **F5 WAF** -ratkaisun.  Näyttöön avautuu ja näyttää kumppanin ratkaisun ja niihin liittyvät resurssit ratkaisun tilan. Valitse Avaa kumppanin hallintatoimintojen tämän ratkaisun **ratkaisu konsolin** .

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä artikkelissa käyttöön suojauksen valvonnan ja käytännön hallinta osat Tietoturvakeskuksessa. Nyt kun olet tutustunut Tietoturvakeskus, kokeile seuraavaa:

- Määritä suojauskäytäntö Azure tilauksen. Lisätietoja on artikkelissa [Azure Tietoturvakeskuksessa asetus suojauskäytäntöjä](security-center-policies.md).
- Voit suojata Azure resurssien Tietoturvakeskus suositukset avulla. Lisätietoja on artikkelissa [Azure Tietoturvakeskus hallinta suojauksen suositukset](security-center-recommendations.md).
- Tarkastella ja hallita nykyisen suojausvaroitusten. Lisätietoja on artikkelissa [hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md).
- Lisätietoja [uhkien tunnistus lisäominaisuudet](security-center-detection-capabilities.md) , jotka sisältyvät Tietoturvakeskus [Vakio taso](security-center-pricing.md) . 90 päivän maksuton kokeiluversio vakio taso on käytettävissä.
- Jos sinulla on kysyttävää suojauksen suunnittelu-artikkelissa [Azure Security Center usein kysytyt kysymykset](security-center-faq.md).

<!--Image references-->
[1]: ./media/security-center-get-started/azure-menu.png
[2]: ./media/security-center-get-started/security-center-pin.png
[3]: ./media/security-center-get-started/security-policy.png
[4]: ./media/security-center-get-started/prevention-policy.png
[5]: ./media/security-center-get-started/recommendations.png
[6]: ./media/security-center-get-started/resources-health.png
[7]: ./media/security-center-get-started/security-alert.png
[8]: ./media/security-center-get-started/security-alert-detail.png
[9]: ./media/security-center-get-started/partner-solutions.png
[10]: ./media/security-center-get-started/welcome.png
