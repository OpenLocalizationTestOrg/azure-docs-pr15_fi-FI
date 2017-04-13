<properties
    pageTitle="Lokitiedoston Analytics VMware seuranta-ratkaisun | Microsoft Azure"
    description="Tietoja siitä, kuinka VMware seuranta-ratkaisun avulla hallita lokit ja valvoa ESXi isännät."
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
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Log Analytics-ratkaisun VMware seuranta (ennakkoversio)

Lokitiedoston Analytics VMware seuranta-ratkaisu on ratkaisun, joka auttaa luomaan keskitetty kirjaaminen ja seurantaa lähestymistapaan suuri VMware lokitiedot. Tässä artikkelissa kuvataan, kuinka voit liittyviä ongelmia, siepata ja hallita ESXi isännät-ratkaisun käyttäminen yhdessä paikassa. Ratkaisuun näet yksityiskohtaista tietoa kaikkien oman ESXi isäntien yhteen paikkaan. Näet tapahtuman ylin arvo, tila ja trendien AM ja ESXi isäntien ESXi host lokit kautta. Voit tehdä tarkasteleminen ja etsimällä keskitetystä ESXi host lokitiedot. Ja voit luoda ilmoituksia log hakukyselyt perusteella.

Ratkaisu käyttää alkuperäisen syslog toimintoja ESXi isännän kohde AM, jonka on OMS agentti push-tietoihin. Kuitenkin ratkaisun ei kirjoita tiedostojen syslog kohde AM kuluessa. OMS-agentti portin 1514 avautuu, ja seuraa tätä. Kun se saa tiedot, OMS-agentti Vie tiedot OMS kyselyjä.

## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu

Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

- Lisää VMware seuranta-ratkaisun OMS työtilan käyttämisestä [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet.

#### <a name="supported-vmware-esxi-hosts"></a>Tuetut VMware ESXi isännät
vSphere ESXi Host 5.5 ja 6.0

#### <a name="prepare-a-linux-server"></a>Valmistele Linux-palvelin
Luo Linux-käyttöjärjestelmän kaikki syslog tietojen vastaanottamiseksi ESXi isäntien AM. [OMS Linux agentti](log-analytics-linux-agents.md) on sivustokokoelman kaikkien ESXi host syslog tietojen. Voit käyttää useita ESXi isännät voit lähettää virhelokit yhteen Linux-palvelimeen, kuten seuraavassa esimerkissä.  

   ![syslog työnkulku](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Määritä syslog sivustokokoelman

1. Määritä VSphere syslog edelleenlähetys. Lisätietoja avulla syslog edelleenlähetyksen määrittäminen on [määrittäminen syslog ESXi 5.x ja 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Siirry **ESXi isännän kokoonpano** > **ohjelmiston** > **Lisäasetukset** > **Syslog**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. Lisää Linux palvelimen ja portin numeroa *1514* *Syslog.global.logHost* -kenttään. Esimerkiksi `tcp://hostname:1514` tai`tcp://123.456.789.101:1514`

3. Avaa syslog ESXi-isännän palomuuri. **ESXi isännän kokoonpano** > **ohjelmiston** > **Suojausprofiilin** > **palomuurin** ja Avaa **Ominaisuudet**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Tarkista vSphere konsolin voit varmistaa, että kyseinen syslog on määritetty oikein. Vahvista, että port ESXI isännän **1514** on määritetty.

5. Testaa Linux-palvelin ja ESXi isännän väliset yhteydet käyttämällä `nc` ESXi isännän-komennon. Esimerkki:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Lataa ja asenna OMS-agentti Linux Linux-palvelimeen. Katso lisätietoja [OMS-agentti Linux ohjeissa](https://github.com/Microsoft/OMS-Agent-for-Linux).

7. Kun Linux OMS-agentti on asennettu, siirry kansioon, /etc/opt/microsoft/omsagent/sysconf/omsagent.d ja kopioi /etc/opt/microsoft/omsagent/conf/omsagent.d-hakemistosta ja muuta vmware_esxi.conf tiedoston omistaja-ryhmän ja käyttöoikeudet-tiedoston. Esimerkki:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  Käynnistä uudelleen suorittamalla Linux OMS-agentti `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. Tee OMS-portaalissa log haku `Type=VMware_CL`. Kun OMS kerää syslog tietoja, sekä syslog-muodossa. Valitse-portaalissa tietyn kenttiä välitetyt, kuten *Hostname* ja *prosessinimi*.  

    ![tyyppi](./media/log-analytics-vmware/type.png)  

    Jos Näytä loki hakutuloksia muistuttavat yllä olevan kuvan, on määritetty käyttämään OMS VMware seuranta Raporttinäkymät-ikkunan ratkaisu.  

## <a name="vmware-data-collection-details"></a>VMware sivustokokoelman tietojen

VMware seuranta-ratkaisun kerää eri suorituskyvyn mittarit ja kirjaudu tietojen ESXi isännät OMS tekijöiden käytön Linux, jotka on otettu käyttöön.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmistä ja muita tietoja siitä, miten kerätä tietoja.

| käyttöympäristö | Linux OMS-agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Linux|![Kyllä](./media/log-analytics-vmware/oms-bullet-green.png)|![Ei](./media/log-analytics-vmware/oms-bullet-red.png)|![Ei](./media/log-analytics-vmware/oms-bullet-red.png)|            ![Ei](./media/log-analytics-containers/oms-bullet-red.png)|![Ei](./media/log-analytics-vmware/oms-bullet-red.png)| 3 minuutin välein|


Seuraavassa taulukossa Näytä VMware seuranta-ratkaisun keräämiä tietokentät Esimerkkejä:

| kenttänimi | kuvaus |
| --- | --- |
| Device_s| VMware tallennuslaitteet |
| ESXIFailure_s | Virhe tyypit |
| EventTime_t | Kun tapahtuman aika |
| HostName_s | ESXi isäntänimi |
| Operation_s | Luo AM tai poista AM |
| ProcessName_s | tapahtuman nimi |
| ResourceId_s | VMware isännän nimeä |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | Hyper-V |
| SCSIStatus_s | VMware SCSI tila |
| SyslogMessage_s | Syslog tiedot |
| UserName_s | käyttäjä, jolla luodaan tai poistetaan AM |
| VMName_s | AM nimi |
| Tietokoneen | isäntätietokoneen |
| TimeGenerated | tiedot on luotu aika |
| DataCenter_s | VMware palvelinkeskukseen |
| StorageLatency_s | tallennustilan viive (ms) |

## <a name="vmware-monitoring-solution-overview"></a>VMware valvonnan ratkaisuun yleiskatsaus

VMware-ruutu näkyy OMS-portaalissa. Se on ylätason näkymän kaikki virheet. Kun valitset ruudun, voit siirtyä dashboard-näkymään.

![ruutu](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Etsi dashboard-näkymä

**VMware** dashboard-näkymän lavat järjestetään mukaan:

- Virhe tilan määrä
- Laskee yläreunan Host tapahtuman mukaan
- Tapahtuman ylin arvo
- Virtuaalikoneen toiminnot
- ESXi Host levyn tapahtumat


![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Napsauta mitä tahansa sivu Avaa loki Analytics hakuruutu, jossa on yksityiskohtaiset tiedot tietyn sivu.

Tässä kohdassa voit muokata muokata jotain tiettyä hakukyselyn. Opetusohjelmaan OMS haku valitsemalla Perustiedot, Kuittaa ulos [OMS log haun opetusohjelma.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Etsi ESXi host tapahtumat

Yksi ESXi isäntä Luo useita lokeja, niiden prosessien perusteella. VMware seuranta-ratkaisu keskittää ne ja tapahtuma-arvo on yhteenveto. Tämä näkymä on keskitetty auttaa sinua ymmärtämään, mitä ESXi host on tapahtumien määrää ja mitä tapahtumien useimmin ympäristössäsi.

![tapahtuman](./media/log-analytics-vmware/events.png)

Voit siirtyä edelleen valitsemalla ESXi-isäntä tai tapahtumatyyppi.

Kun napsautat ESXi-isäntänimi, voit tarkastella ESXi isännöivien tiedot. Jos haluat rajata tuloksia tapahtumatyyppi, Lisää `“ProcessName_s=EVENT TYPE”` hakukysely. Voit valita **prosessinimi** etsintäsuodatinta. Joka supistaa tiedot puolestasi.

![Siirtyminen](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Etsi hyvin AM toiminnot

Virtual machine voi luoda ja poistaa kaikki ESXi isännän. Kannattaa järjestelmänvalvoja voi määrittää kuinka monta VMs ESXi host Luo. Kyseisen--vuorostaan auttaa ymmärtämään suorituskykyyn ja kapasiteettiin suunnittelu. Pitäminen ja AM tehtävän tapahtumat on tärkeä, kun ympäristön hallinta.

![Siirtyminen](./media/log-analytics-vmware/vmactivities1.png)

Jos haluat nähdä muita ESXi host AM luominen tietojen, valitse ESXi-isäntänimi.

![Siirtyminen](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Yleisiä hakukyselyt

Ratkaisu on muita hyödyllisiä kyselyt, joiden avulla voit hallita ESXi-isäntien, kuten suuri tallennustilaa, tallennustilan viive ja polku-virhe.

![Kyselyt](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Tallenna kyselyt

Tallentaminen hakukyselyt on vakio ominaisuus OMS ja avulla voit pitää kyselyitä, jotka olet löytänyt hyötyä. Kun olet luonut kyselyn, jossa voit hyödyllisiä, tallentaa sen valitsemalla **Suosikit**. Tallennetun kyselyn avulla voit käyttää sitä helposti myöhemmin [Oma raporttinäkymä](log-analytics-dashboards.md) -sivulle, jossa voit luoda oman mukautetun raporttinäkymiä.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Luo ilmoituksia kyselyistä

Kun olet luonut kyselyissä, haluat ehkä ilmoitus, kun tietyn tapahtumien kyselyjen avulla. Saat tietoja siitä, miten voit luoda ilmoituksia [Log Analytics ilmoitukset](log-analytics-alerts.md) . Esimerkkejä kyselyjen ja kysely muita esimerkkejä ilmoitat on blogimerkinnässä [näytön VMware käyttämällä OMS Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) .

## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Mitä tarvitsen isännöidä asetus käyttöön ESXi? Mitä vaikutuksia se on nykyisen-ympäristöön?
Ratkaisu käyttää ESXi isännän Syslog alkuperäinen versio välittäminen järjestelmä. Sinun ei tarvitse mitään muita Microsoft-ohjelmiston ESXi isännän kannattaa tallentaa lokit. Se on pieni vaikutus aiemmin-ympäristöön. Voit kuitenkin on määritettävä syslog välitys, joka on ESXI toimintoja.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Pitääkö ESXi-host uudelleen?
Ei. Tämä toimenpide ei edellytä uudelleen. Joskus vSphere ei oikein Päivitä syslog. Siinä tapauksessa Kirjaudu sisään ESXi isäntään ja lataa syslog. Uudelleen sinun ei tarvitse, isäntä uudelleen, jotta tätä prosessia ei ole ympäristössäsi häiritä.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Voit suurentaa tai pienentää äänenvoimakkuuden OMS lähetettyjä tietoja?
Kyllä, voit tehdä. Voit käyttää ESXi isännän tason asetukset vSphere. Log-sivustokokoelman perustuu *tiedot* taso. Siis valvottavat AM luominen tai poistaminen, jos haluat säilyttää Hostd *tiedot* -tason. Lisätietoja on artikkelissa [VMware Knowledge Base-tietokannan](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Miksi Hostd ei tarjoa tietojen OMS avulla? Log-asetus on määritetty tiedot.
Oli ESXi Host (isäntä)-ohjelmavirhe syslog aikaleima varten. Lisätietoja on artikkelissa [VMware Knowledge Base-tietokannan](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Kun olet asentanut menetelmä, Hostd pitäisi toimia normaalisti.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Voi olla useita ESXi isännät välittäminen syslog tiedot yhteen AM omsagent kanssa?
Kyllä. Voit määrittää useita ESXi isännät välittäminen yksittäisen AM omsagent kanssa.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Miksi en näe tietojen tuominen OMS juoksutus?

Voi olla useita syitä:

- ESXi isäntä on ei ole oikein valitseminen tiedot, jossa omsagent AM. Testaa, toimi seuraavasti:
    1. Vahvista, kirjaudu sisään käyttämällä ssh ESXi isäntään ja suorittamalla seuraavan komennon:`nc -z ipaddressofVM 1514`

        Jos tämä ei ole onnistunut, vSphere asetukset Lisäasetukset todennäköisesti korjaa. Lisätietoja ESXi isännän syslog edelleenlähetyksen määrittämisestä on kohdassa [Määritä syslog sivustokokoelman](#configure-syslog-collection) .

    2. Jos syslog portin yhteys onnistuu, mutta ei ole vielä näkyvissä mitään tietoja, lataa sitten ESXi isännän syslog käyttämällä ssh suorittamalla seuraavan komennon:` esxcli system syslog reload`

- AM kanssa OMS-agentti ei ole määritetty oikein. Voit testata tämän toimimalla seuraavasti:
    1. OMS seuraa porttiin 1514 ja vie tietoja OMS kyselyjä. Jos haluat varmistaa, että se on avoinna, suorita seuraava komento:`netstat -a | grep 1514`
    2. Näkyviin tulee portin `1514/tcp` Avaa. Jos et, tarkista, omsagent on asennettu oikein. Jos porttitietoja ei ole näkyvissä, syslog portti ei ole avoinna AM.
        1. Varmista, että OMS-agentti käynnissä käyttämällä `ps -ef | grep oms`. Jos se ei ole käynnissä, Käynnistä korjaus-komennon suorittaminen` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Avaa `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` tiedosto.

            Varmista, että oikea käyttäjän ja ryhmän-asetus on voimassa, samalla:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Jos tiedosto ei ole olemassa tai käyttäjän ja ryhmän-asetus on väärä, kestää korjausta mukaan [Valmistellaan Linux-palvelimeen](#prepare-a-linux-server).

## <a name="next-steps"></a>Seuraavat vaiheet

- Log Analytics [Log hakujen](log-analytics-log-searches.md) avulla voit tarkastella yksityiskohtaisia VMware host tietoja.
- [Luo omia raporttinäkymiä](log-analytics-dashboards.md) VMware host tiedot näkyvät.
- [Luo ilmoituksia](log-analytics-alerts.md) tietyn VMware host tapahtumien yhteydessä.
