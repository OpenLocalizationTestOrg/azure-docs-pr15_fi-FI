<properties
    pageTitle="Log Analytics kapasiteetin hallintaratkaisu | Microsoft Azure"
    description="Voit käyttää kapasiteetin suunnittelu-ratkaisun Log Analytics voi auttaa sinua ymmärtämään kapasiteetti Hyper-V-palvelimiin hallitun System Center virtuaalikoneen hallinta"
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

# <a name="capacity-management-solution-in-log-analytics"></a>Lokitiedoston Analytics kapasiteetin hallintaratkaisu


Voit Log Analytics kapasiteetin suunnittelu-ratkaisun voi auttaa sinua ymmärtämään kapasiteetti hallitun System Center virtuaalikoneen hallinta Hyper-V-palvelimiin. Tämä ratkaisu edellyttää System Center Operations Manager ja System Center virtuaalikoneen hallinta. Kapasiteetin suunnittelu ei ole käytettävissä, jos käytössäsi on vain suoraan yhdistettyjä agenttien vuoksi. Asennat päivittämään Operations Manager-agentti ratkaisu. Ratkaisu lukee suorituskyvyn laskureita valvottu palvelimessa ja lähettää käyttötietoja OMS-palvelun Pilvipalvelun käsittely. Logiikan käytetään käyttötietojen ja pilvipalvelussa tietueiden tietoja. Ajan myötä käyttötavat tunnistetaan ja kapasiteetti on suunniteltu, nykyisen kulutus perusteella.

Ennuste voi esimerkiksi määritetään, kun muita suoritin sydämiä tai lisää muistia tarvitaan yksittäisiä palvelimen. Tässä esimerkissä ennuste saattaa tarkoittaa, että 30 päivän aikana palvelin on lisää muistia. Tämän avulla voit suunnitella muistin päivityksen palvelimen Seuraava ylläpito-ikkunassa, joka saattaa ilmetä kerran kahden viikon aikana.

>[AZURE.NOTE] Kapasiteetin Management-ratkaisun ei ole käytettävissä, jotka on lisättävä. Asiakkaat, joilla on asennettu kapasiteetin management-ratkaisun edelleen käyttää ratkaisu.  

Suunnitteluratkaisun kapasiteetti on parhaillaan on päivitetty käyttämällä seuraavia asiakkaan ilmoitettava haasteet:

- Käytössä on virtuaalikoneen hallinta ja Operations Manager
- Ryhmät perustuvat ei voi mukauttaa tai suodatus
- Tunnin välein tietojen koostaminen säännöllisempää ei ole tarpeeksi
- Ei ole AM tason tiedot
- Tietojen luotettavuus

Uuden kapasiteetin-ratkaisun edut:

- Hajautetun tietojen kerääminen tukevan käyttövarmuuden ja tarkkuus
- Tarvitsematta VMM Hyper-V-tuki
- Visualisoinnin PowerBI arvot
- Valitse AM tason käyttö tiedot


## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu
Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

- Operations Manager vaaditaan kapasiteetin Management-ratkaisun.
- Virtuaalikoneen hallinta vaaditaan kapasiteetin Management-ratkaisun.
- Operations Manager connectivity virtuaalikoneen Manager (VMM) on pakollinen. Saat lisätietoja yhteyden järjestelmien [yhteyden VMM Operations Manager](http://technet.microsoft.com/library/hh882396.aspx).
- Lokitiedoston Analytics on oltava yhdistettynä Operations Manager.
- Lisää kapasiteetin Management-ratkaisun OMS työtilan käyttämisestä [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet.  Ei ole tarvitaan lisämäärityksiä.


## <a name="capacity-management-data-collection-details"></a>Kapasiteetin hallinta tietojen keräämisen tiedot

Kapasiteetin hallinta kerää suorituskykytietoja metatietojen ja tila-tiedot, jotka on otettu käyttöön tekijöiden avulla.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmät ja muita tietoja siitä, miten tiedot kerätään kapasiteetin hallintaa varten.

| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Windows|![Ei](./media/log-analytics-capacity/oms-bullet-red.png)|![Kyllä](./media/log-analytics-capacity/oms-bullet-green.png)|![Ei](./media/log-analytics-capacity/oms-bullet-red.png)|            ![Kyllä](./media/log-analytics-capacity/oms-bullet-green.png)|![Kyllä](./media/log-analytics-capacity/oms-bullet-green.png)| HOURLY|

Seuraavassa taulukossa on esimerkkejä kapasiteetin hallinta keräämiä tietotyypit:

|**Tietotyyppi**|**Kentät**|
|---|---|
|Metatietojen|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IP-osoite, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-osoite, NetbiosDomainName, LogicalProcessors, DNSName, näyttönimi, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Suorituskyky|Objektin nimi, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded|
|Tila|StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekstissa, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="capacity-management-page"></a>Kapasiteetin hallintasivu


 Kapasiteetin suunnittelu-ratkaisun asennuksen jälkeen voit tarkastella valvottu palvelinten kapasiteetin **Kapasiteetin suunnittelu** -ruutua käyttämällä OMS **Yleiskatsaus** -sivulla.

![Kuva kapasiteetin suunnittelu-ruutu](./media/log-analytics-capacity/oms-capacity01.png)

Ruutu avautuu, missä voit katsella palvelimen kapasiteetin yhteenveto **Kapasiteetin hallinta** Raporttinäkymät-ikkunan. Sivulla näkyy, voit valita seuraavat ruudut:

- *Virtuaalikoneen määrä*: Näyttää näennäiskoneiden kapasiteetin jäljellä olevien päivien määrän.
- *Laske*: Näyttää suorittimen Sydämiä ja käytettävissä oleva muisti
- *Tallennustilan*: Näyttää levytilaa ja keskimääräinen levyn viive
- *Etsi*: tietoresurssien, voit käyttää mitä tahansa tietojen hakeminen OMS-järjestelmässä

![Kapasiteetin hallinta Raporttinäkymät-ikkunan kuva](./media/log-analytics-capacity/oms-capacity02.png)


### <a name="to-view-a-capacity-page"></a>Voit tarkastella kapasiteetti-sivu

- Valitse **Yleiskatsaus** -sivulla **Kapasiteetin**hallinta ja valitse sitten **Laske** - ja **tallennustilaa**.

## <a name="compute-page"></a>Laske sivu

Voit **laskea** Raporttinäkymät-ikkunan Microsoft Azure OMS kapasiteetin käyttöä, suunnitellut kapasiteetista, päivän ja tehokkuutta infrastruktuurin liittyviä tietoja. **Käyttö** -alueen avulla voit tarkastella suorittimen core ja muistin käyttö virtuaalikoneen isännät. Ennuste-työkalulla voit arvioida, kuinka paljon kapasiteetti on tarkoitus olla käytettävissä päivämäärävälillä. **Tehokkuuden** -alueen avulla voit tarkistaa, kuinka tehokas virtuaalikoneen isännät ovat. Voit tarkastella tietoja linkitetyt kohteet napsauttamalla niitä.

Voit luoda Excel-työkirjan seuraavat luokat:

- Ylimmät isännät kanssa suurin core käyttö
- Ylimmät isännät kanssa suurin muistin käyttö
- Ylimmät isännät tehotonta näennäiskoneiden kanssa
- Ylimmät isännät käytön mukaan
- Ala-isäntien käytön mukaan

![Kuva sivun kapasiteetin hallinta laskemiseen](./media/log-analytics-capacity/oms-capacity03.png)


**Laske** -koontinäytössä näkyvät seuraavilla alueilla:

**Käyttö**: näkymän suorittimen core ja muistin käyttö virtuaalikoneen isännät.

- *Käyttää sydämiä*: summa kaikki isäntien (prosenttia käyttää suorittimen fyysinen sydämiä isännän määrä kerrottuna).
- *Vapaa sydämiä*: fyysinen sydämiä miinus käytetyt sydämiä yhteensä.
- *Prosentti sydämiä käytettävissä*: vapaa fyysinen sydämiä fyysinen sydämiä määrä jaettuna.
- *Virtuaalinen sydämiä AM kohden*: yhteensä virtual sydämiä näennäiskoneiden määrä jaettuna järjestelmän järjestelmässä.
- *Virtuaalinen sydämiä fyysinen sydämiä suhde*: yhteensä fyysinen sydämiä fyysinen sydämiä, jotka käyttävät näennäiskoneiden järjestelmään, välisen suhteen.
- *Määrä virtual sydämiä käytettävissä*: Virtual core ja fyysinen sydämiä suhde kerrottuna käytettävissä fyysinen sydämiä.
- *Käyttää muisti*: summa, joka on käyttämä kaikki isännät.
- *Muisti*: yhteensä miinus käytetyn muistin muistin.
- *Prosentti muistia käytettävissä*: vapaa muistin yhteensä muistin jaettuna.
- *Näennäismuistin AM kohden*: yhteensä näennäismuistin järjestelmän jaettuna näennäiskoneiden kokonaismäärän järjestelmässä.
- *Näennäismuistin fyysinen muistin suhde*: yhteensä näennäismuistin järjestelmän jaettuna yhteensä muistin järjestelmässä.
- *Näennäismuistin käytettävissä*: ja muistin suhde näennäismuistin kerrottuna käytettävissä muistin.

**Ennuste-työkalu**

Ennuste-työkalun avulla voit tarkastella historiallisten trendejä resurssien käyttö. Tämä sisältää käyttö kehityksen näennäiskoneiden, muistin, core ja tallennustilaa. Ennuste-ominaisuus käyttää ennuste-algoritmin avulla, kun käytössäsi on jokaisen resurssin ulos. Näin voit laskea ERISNIMI kapasiteetin suunnittelu niin, että tiedät, kun haluat ostaa lisää kapasiteetin (kuten muistin Sydämiä ja tallennustilaa).

**Tehokkuutta**

- *Vapaa AM*: alle 10 prosenttia suorittimen ja 10 % muistin käytön ajan.
- *Overutilized AM*: yli 90 prosenttia suorittimen ja 90 prosentin muistin käytön ajan.
- *Vapaa Host (isäntä)*: alle 10 prosenttia suorittimen ja 10 % muistin käytön ajan.
- *Overutilized Host (isäntä)*: yli 90 prosenttia suorittimen ja 90 prosentin muistin käytön ajan.

### <a name="to-work-with-items-on-the-compute-page"></a>Voit käsitellä kohteita Laske-sivulla

1. Näytä kapasiteetin tietoja sydämiä suorittimen ja muistin käytön **Laske** Raporttinäkymät-ikkunan **käyttö** -alueella.
2. Napsauta kohdetta, voit avata **Etsi** -sivulla ja tarkastella yksityiskohtaisia tietoja.
3. **Ennuste** -työkalun Näytä ennuste kapasiteetista, jota käytetään päivänä, voit valita siirtämällä päivämäärä.
4. **Tehokkuuden** -alueella tarkastella tehokkuuden kapasiteetin näennäiskoneiden ja virtuaalikoneen isännöi tietoja.

## <a name="direct-attached-storage-page"></a>Suora liitetty tallennustila

Voit **Suoraan liitetty tallennustilan** Raporttinäkymät-ikkunan OMS kapasiteetin liittyviä tietoja tallennustilan käyttö ja suorituskyvyn parantaminen levyn kapasiteetti arvioidut päivän. **Käyttö** -alueen avulla voit tarkastella levytilan virtuaalikoneen isännät. **Suorituskyvyn** -alueen avulla voit tarkastella levyn siirtonopeuden ja viive virtuaalikoneen isännät. Ennuste-työkalun avulla voit myös arvioida, kuinka paljon kapasiteetti on tarkoitus olla käytettävissä päivämäärävälillä. Voit tarkastella tietoja linkitetyt kohteet napsauttamalla niitä.

Voit luoda Excel-työkirjan kapasiteetin tiedot seuraavat luokat:

- Ylimmät levytilan isäntä
- Ylimmät keskimääräinen odotusaika isäntä

**Tallennustilan** -sivulla näkyvät seuraavilla alueilla:

- *Käyttö*: levytilan tarkasteleminen virtuaalikoneen isännät.
- *Yhteensä levytilaa*: summa (looginen levytilaa) kaikki isäntien
- *Käyttää levytilaa*: summa (käytetyn looginen levytilan) kaikki isäntien
- *Käytettävissä olevat levytilaa*: levytilaa miinus käytetyn levytilan yhteensä
- *Levyn käytössä prosentteina*: käyttää levytilaa jaettuna levytilaa yhteensä
- *Prosentti levyn käytettävissä*: levytilaa yhteensä jaettuna vapaata kiintolevytilaa

![Kuva kapasiteetin hallinta suoraan liitetty tallennustilan sivun](./media/log-analytics-capacity/oms-capacity04.png)

**Suorituskyvyn parantaminen**

OMS avulla voit tarkastella levytilan historiallisten käyttö trendin. Ennuste-ominaisuus käyttää algoritmin projektin tulevien käyttö. Tilaa käytön erityisesti ennuste-ominaisuuksien avulla voit projekti, kun suoritat ehkä levytila. Tämä auttaa sinua ERISNIMI tallennustilan suunnitteleminen ja tiedät, kun haluat ostaa lisää tallennustilaa.

**Ennuste-työkalu**

Ennuste-työkalun avulla voit tarkastella historiatietojen ja suuntausten levyn tilan-käyttö. Ennuste-ominaisuuksien avulla voit myös projekti, kun käytössäsi on liian vähän levytilaa. Tämän avulla voit suunnitella ERISNIMI kapasiteetin ja tiedät, kun haluat ostaa lisää tallennustilaa.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Voit käsitellä kohteita suoraan liitetty tallennustilan-sivulla

1. **Suora liitetty tallennustilan** Raporttinäkymät-ikkunan **käyttö** -alueessa voit tarkastella levyn käyttö tiedot.
2. Valitse linkitetty kohde avataan **haun** sivulle ja tarkastella yksityiskohtaisia tietoja.
3. **Levyn suorituskyky** -kohdassa voit tarkastella levyn siirtonopeuden ja viive tiedot.
4. **Ennuste-työkalun**Näytä ennuste kapasiteetista, jota käytetään päivänä, voit valita siirtämällä päivämäärä.


## <a name="next-steps"></a>Seuraavat vaiheet

- Käytä [Log rajaamalla Log Analytics](log-analytics-log-searches.md) yksityiskohtainen kapasiteetin hallintatietojen.
