<properties
   pageTitle="Sovelluksen riippuvuuden näyttö (ADM) toimintojen hallinta-ohjelmistopaketin (OMS) | Microsoft Azure"
   description="Sovelluksen riippuvuus näyttö (ADM) on toimintojen hallinta Suite (OMS)-ratkaisu, joka löytää sovelluksen osia Windows- ja Linux järjestelmiin ja yhdistää välisen palvelut automaattisesti.  Tässä artikkelissa on tietoja käyttöönotto ADM ympäristön ja käyttää sitä skenaarioita."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Käyttämällä sovelluksen riippuvuus näytön ratkaisu toimintojen hallinta Suite (OMS)
![Ilmoitusten hallinta-kuvake](media/operations-management-suite-application-dependency-monitor/icon.png) Sovelluksen riippuvuus näyttö (ADM) löytää sovelluksen osia Windows- ja Linux järjestelmiin ja yhdistää välisen palvelut automaattisesti. Sen avulla voit tarkastella palvelinten, voit ajatella hänet yhdistetty järjestelmät, jotka toimittavat tärkeiden palveluiden.  Sovelluksen riippuvuus näyttötavan-palvelimet-prosessien väliset yhteydet ja portit yli minkä tahansa TCP-yhteys-arkkitehtuuri ja määrityksiä vaaditaan vain asennettuun agentti.

Tässä artikkelissa on tietoja sovelluksen riippuvuus näytöstä.  Lisätietoja ADM ja onboarding määrittämisestä on artikkelissa [määrittäminen sovelluksen riippuvuuden näyttö-ratkaisun toimintojen hallinta Suite (OMS)](operations-management-suite-application-dependency-monitor-configure.md)

>[AZURE.NOTE]Sovelluksen riippuvuus näyttö on tällä hetkellä yksityinen esikatselu.  Voit pyytää ADM yksityinen esikatselu [https://aka.ms/getadm](https://aka.ms/getadm)on pääsy.
>
>Yksityinen esikatselun aikana kaikki OMS-tileillä on oikeus ADM.  ADM solmut ovat maksuttomia, mutta lokin Analytics-tiedot AdmComputer_CL ja AdmProcess_CL mitataan kuten muiden ratkaisu.
>
>Kun ADM siirtyy public preview-version, se on käytettävissä vain maksuttomia ja maksullisia asiakkaiden tietoja ja Analytics-OMS hinnat suunnitteleminen.  Vapaa taso tilit rajoittuvat 5 ADM solmujen.  Jos on meneillään yksityinen esikatselu ja ovat ei ole rekisteröity OMS hinnat suunnitteleminen ADM tullessa public preview-version, ADM eivät ole käytettävissä tällä kertaa. 


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Käytä tapauksissa: Tee IT käsittelee huomioon riippuvuus

### <a name="discovery"></a>Etsiminen
ADM muodostaa automaattisesti yleisiä viittaus kartan riippuvuuksista palvelimissa, prosessit ja 3 osapuolen palvelujen.  Se löytää ja yhdistää kaikki TCP-riippuvuudet, tunnistaminen muutokset yllätä yhteydet, etä-3 voit määräytyvät osapuolen-järjestelmiä ja perinteinen Tumma alueita verkossa, kuten DNS ja AD riippuvuudet.  ADM löytää epäonnistui verkkoyhteyksien hallittujen järjestelmien yrität muodostaa, jotka auttavat tunnistaa mahdolliset palvelimen virheellisen määrityksen, palvelukatkoksia ja verkko-ongelmista.

### <a name="incident-management"></a>Tapauksen hallintaan
ADM voit vähentää ongelman eristyksen vähentää arvailua näytetään, miten järjestelmien muodostanut ja vaikuttavat toisiinsa.  Lisäksi epäonnistui yhteydet tietoja yhteydessä asiakkaista auttaa tunnistamisessa väärä kuormituksen tasoitusmääritykset, surprising tai liiallinen kuormitus tärkeiden palveluiden ja päivittämättömien asiakkaiden, kuten kehittäjä koneet puhuminen tuotannon järjestelmiin.  Integroitu työnkulut OMS muuta seuranta avulla näet, onko muuta tapahtumasta taustatietokantaan tietokoneeseen myös tai palvelun kerrotaan tapahtuma pääkansion syy.

### <a name="migration-assurance"></a>Siirron ylläpito
ADM avulla voit tehokkaasti suunnittelu nopeuttamiseksi ja vahvista Azure siirrot varmistetaan, että ei jää mitään ja ei ole muutokset yllätä katkokset.  Voit tutustua toisiinsa järjestelmien, joka on siirtää yhdessä, arvioi Järjestelmäkokoonpano ja kapasiteettiin ja tunnistaa käynnissä järjestelmään on edelleen tarjota käyttäjille tai candidate on sen sijaan, että siirron käytöstä.  Kun siirto on valmis, voit tarkistaa asiakkaan kuormituksen ja jäsenyyden vahvistamiseksi testi järjestelmien ja asiakkaiden yrität muodostaa yhteyden.  Jos aliverkon suunnittelu- ja palomuuriasetukset määritelmät ongelmia, epäonnistunut yhteydet ADM karttoja osoita voit järjestelmiin, jotka tarvitsevat yhteys.

### <a name="business-continuity"></a>Liiketoiminnan jatkuvuus
Jos käytössäsi on Azure palauttaminen ja on apua sovelluksen ympäristön ADM palautus järjestyksen määrittäminen voit automaattisesti kerrotaan, miten järjestelmien luottavat toisiinsa varmistaa, että palautus ei luotettavia.  Kriittinen palvelimen valitseminen ja tuomalla sen asiakkaiden, saat selville edusta järjestelmien, joka palauttaa vain, kun kriittinen on palautettu ja käytettävissä.  Vastaavasti katsomalla kriittinen palvelimen taustatietokantaan riippuvuudet voit tunnistaa järjestelmät, joka on palautettu, ennen kuin kohdistus järjestelmän palauttaa.

### <a name="patch-management"></a>Korjaustiedoston hallinta
ADM parantaa OMS järjestelmässä päivitys arviointi käytön mukaan näytetään jotka muita ryhmiä ja palvelinten riippuvat palvelustasi, niin voit ilmoittaa ne ennen julkaisemista järjestelmien, korjataan etukäteen.  ADM parantaa korjaustiedoston hallinnan OMS myös mukaan, joka osoittaa, ovatko palvelujen käytettävissä ja että se on liitetty sen jälkeen, kun ne on asennettu ja käynnistetään uudelleen. 


## <a name="mapping-overview"></a>Yhdistämismäärityksen yleiskatsaus
ADM tekijöiden kerää tietoja TCP-yhteys prosessien palvelimessa, johon ne on asennettu sekä kunkin prosessin saapuvat ja lähtevät yhteydet tietoja.  Machine-luettelon avulla ADM-ratkaisun vasemmassa reunassa varustetut ADM tekijöiden voidaan valita voi esittää niiden välisten riippuvuuksien valittua aikaa alueen yli.  Tietokone riippuvuus keskittyä tiettyyn tietokoneen määritykset ja näyttää kaikissa tietokoneissa on suora TCP-asiakkaiden tai palvelimissa, että koneen.

![ADM yleiskatsaus](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Koneet voidaan laajentaa määrityksen näyttämään aktiivinen verkkoyhteyksien prosessit valitun ajankohtana.  Kun kanssa ADM agentti etätietokone on laajennettu ja näyttää tiedot, vain kohdistus-koneen kanssa prosessit ovat näkyvissä.  Agentitonta edusta koneet yhdistäminen yhdeksi kohdistus-koneen määrä näkyy vasemmassa reunassa he muodostaa prosesseista.  Jos kohdistus-koneen tekee yhteyden taustatietokantaan tietokoneeseen ilman sitä, että taustatietokantaa esitetään, jos solmun määrityksen, joka näyttää sen IPv4-osoite ja solmu on laajennettu ja näyttää yksittäiset portit ja palveluita kohdistus tietokone on yhteydessä.

Oletusarvon mukaan ADM kartoissa näyttää riippuvuustiedot viimeisten 10 minuutin.  Aika-asetusten avulla vasemmassa yläkulmassa, karttoja voi tehdä kyselyn historiallisen ajan alueita, enintään, tunnin leveä, jos haluat näyttää, miten riippuvuudet etsitään menneisyydessä, kuten tapahtuma aikana tai ennen on muuttunut.    ADM tiedot tallennetaan 30 päivän maksettu työtilojen ja vapaa-työtilojen seitsemän päivää.

![Koneen kartan, jossa on valittu tietokoneen ominaisuudet](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Epäonnistuneiden yhteydet
Epäonnistuneiden yhteydet näytetään ADM karttojen prosesseja ja tietokoneissa, punaisella katkoviivalla näyttäminen Jos asiakkaan järjestelmän epäonnistui tuntemattomasta prosessin tai portin saavuttamiseksi.  Epäonnistui yhteydet raportoidaan käyttöön ADM-agentti minkä tahansa järjestelmästä, jos järjestelmässä on yritetään yhteys epäonnistui.  ADM mittaa tämä tarkkailemalla TCP sockets, jotka eivät läpäise muodostamaan yhteyttä.  Tämä voi johtua palomuurin virheellisen-määrityksen, asiakkaan tai palvelimen tai remote palvelu ei ole ollut käytettävissä. 

![Epäonnistuneiden yhteydet](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Tietoja epäonnistui yhteydet auttaa siirron kelpoisuuden, suojauksen analysointi ja yleistä arkkitehtuuri tietoja vianmäärityksessä.  Joskus epäonnistui yhteydet vaarattomuuden, mutta ne usein viittaavat suoraan ongelman automaattisesti-ympäristössä, jossa on tulossa yhtäkkiä saavuttamattomissa, kuten.. ei ehkä cloud siirron jälkeen painotalon tai kahden sovelluksen tasoa.  Yllä olevan kuvan IIS ja WebSphere ovat molemmat käynnissä, mutta niitä ei saa yhteyttä. 

## <a name="computer-and-process-properties"></a>Tietokoneen ja prosessin ominaisuudet
Siirryttäessä ADM-kartan, voit valita koneet ja prosessien ja myönnä lisäkontekstia niiden ominaisuuksista.  Tietokoneissa on tietoja DNS-nimen, IPv4-osoitteet, suorittimen ja muistin kapasiteetin, AM tyyppi, käyttöjärjestelmäversio, viimeisen uudelleen aika ja niiden OMS ja ADM edustajien tunnukset.

Prosessitiedot kerätään käyttöjärjestelmän metatietosäilöstä prosessit, kuten prosessin nimi, prosessikuvaus, käyttäjänimi ja -toimialueella (Windows), yrityksen nimen, tuotteen nimen, tuoteversio, työkansion, komentoriviltä ja prosessin alkamisaika suorittamisesta.

![Prosessin ominaisuudet](media/operations-management-suite-application-dependency-monitor/process-properties.png)

Prosessin yhteenveto-ruudussa on lisätietoja tämän prosessin yhdistämispalvelu, mukaan lukien sen sidotun portit, saapuvat ja lähtevät yhteydet ja epäonnistui yhteydet. 

![Prosessin yhteenveto](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>OMS muutosten jäljityksen integrointi
ADM's integrointi muutosten jäljityksen on automaattista, kun molemmat vaihtoehdot on otettu käyttöön ja määrittänyt OMS työtilassa.

Koneen yhteenveto-ruudussa ilmaisee, onko muutosten jäljityksen tapahtumien valitun tietokoneeseen tapahtui valittua aikajaksoa.

![Koneen yhteenveto-ruudussa](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

Koneen muuta seuranta-ruutu näyttää kaikki muutokset, viimeisimmän ensimmäisen, sekä siirtyä lokitiedoston etsiminen lisätiedot linkki luettelo.
![Koneen muuta seuranta-ruutu](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Seuraavassa on valittuasi **lokin Analytics näyttäminen**alirakenteen määritysten muutos tapahtuman näkymää.
![Määritysten Change-tapahtuma](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Kirjaudu Analytics-tietueet
ADM käyttäjän tietokoneen ja prosessin Varastotiedot ovat käytettävissä [haun](../log-analytics/log-analytics-log-searches.md) Log Analytics.  Tämä voidaan ottaa käyttöön skenaarioiden esimerkiksi siirron suunnittelu kapasiteetin, etsiminen ja ad hoc suorituskyvyn vianmääritys. 

Yksi tietue luodaan tunnissa kunkin yksilöllinen tietokoneen ja prosessin lisäksi tietueet luodaan kun, että prosessin tai tietokoneen alkaa tai valitse-nousta ADM.  Nämä tietueet on seuraavissa taulukoissa ominaisuudet. 

Sisäisesti ominaisuuksien avulla voit tunnistaa yksilöllinen prosessit ja tietokoneissa on:

- Prosessin määritys yksilöllisesti määrittämiä PersistentKey_s, kuten komentorivin ja käyttäjätunnus.  Se on yksilöllinen tietokoneessa, mutta voit toistaa tietokoneiden välillä.
- ProcessId_s ja ComputerId_s ovat GUID ADM-mallissa.



### <a name="admcomputercl-records"></a>AdmComputer_CL tietueet
Tietueet, joiden **AdmComputer_CL** tyyppi on varastotiedot palvelinten ADM tekijöiden kanssa.  Nämä tietueet on määrittämäsi ominaisuudet seuraavan taulukon.  

| Ominaisuus | Kuvaus |
|:--|:--|
| Tyyppi | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Windows- tai Linux tietokonenimi |
| CPUSpeed_d | Suoritin (MHz) nopeus |
| DnsNames_s | Kaikki tämän tietokoneen DNS nimien luettelossa |
| IPv4s_s | Luettelon kaikki tämän tietokoneen IPv4-osoitteet |
| IPv6s_s | Kaikki tämän tietokoneen IPv6-osoitteet luettelo.  (ADM tunnistaa IPv6-osoitteet, mutta ei löydä IPv6 riippuvuudet.) |
| Is64Bit_b | TOSI tai EPÄTOSI OS lajin perusteella |
| MachineId_s | Sisäinen GUID-yksilöllinen yli OMS-työtila  |
| OperatingSystemFamily_s | Windows- tai Linux |
| OperatingSystemVersion_s | Pitkä käyttöjärjestelmän versiomerkkijono |
| TimeGenerated | Päivämäärä ja kellonaika, jolloin tietue on luotu. |
| TotalCPUs_d | Suorittimen sydämiä määrä |
| TotalPhysicalMemory_d | Muistin koko megatavuina |
| VirtualMachine_b | TOSI tai EPÄTOSI, onko OS AM Vieras perusteella |
| VirtualMachineID_g | Hyper-V AM tunnus |
| VirtualMachineName_g | Hyper-V AM nimi |
| VirtualMachineType_s | Hyperv, Vmware, Xen, Kvm, Ldom, Lpar, Virtualpc |


### <a name="admprocesscl-type-records"></a>AdmProcess_CL tietueet 
Tietueet, joiden **AdmProcess_CL** tyyppi on varastotiedot TCP-yhteys kannalta ADM tekijöiden-palvelimiin.  Nämä tietueet on määrittämäsi ominaisuudet seuraavan taulukon.

| Ominaisuus | Kuvaus |
|:--|:--|
| Tyyppi | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | Komentorivin koko prosessin |
| CompanyName_s | Yrityksen nimi (Windows PE tai Linux JÄLLEENMYYNTIHINNAN) |
| Description_s | Monivaiheinen prosessikuvaus (Windows PE: stä tai Linux JÄLLEENMYYNTIHINNAN) |
| FileVersion_s | Suoritettava tiedostoversion (Windows PE, vain Windows) |
| FirstPid_d | OS tunnus |
| InternalName_s | Suoritettava tiedosto sisäinen nimeä (Windows PE, vain Windows) |
| MachineId_s | Sisäinen GUID yksilöllinen yli OMS-työtila  |
| Name_s | Prosessin suoritettavan tiedoston nimi |
| Path_s | Prosessin suoritettavan tiedoston tiedostopolun |
| PersistentKey_s | Yksilöivä tämän tietokoneen sisäinen GUID-tunnus |
| PoolId_d | Sisäinen tunnus yhdistäminen prosessit samalla komento rivien perusteella. |
| ProcessId_s | Sisäinen GUID yksilöllinen yli OMS-työtila  |
| ProductName_s | Tuotteen nimi-merkkijono (Windows PE: stä tai Linux JÄLLEENMYYNTIHINNAN) |
| ProductVersion_s | Protokollan versio (Windows PE: stä tai Linux JÄLLEENMYYNTIHINNAN) |
| StartTime_t | Prosessin alkamisaika paikallisen tietokoneen kellon |
| TimeGenerated | Päivämäärä ja kellonaika, jolloin tietue on luotu. |
| UserDomain_s | Toimialueen prosessin omistajan (vain Windows) |
| UserName_s | Prosessin omistajan (vain Windowsissa) nimi |
| WorkingDirectory_s | Prosessin työkansion |


## <a name="sample-log-searches"></a>Esimerkki log haut

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Luettele kaikki hallitut tietokoneet muistikapasiteetti. 
Tyyppi = AdmComputer_CL | Valitse TotalPhysicalMemory_d, ComputerName_s | Dedup ComputerName_s

![ADM kyselyn Esimerkki](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name-dns-ip-and-os-version"></a>Tietokonenimi, DNS-, IP- ja käyttöjärjestelmän versio.
Tyyppi = AdmComputer_CL | Valitse ComputerName_s, OperatingSystemVersion_s, DnsNames_s IPv4s_s | dedup ComputerName_s

![ADM kyselyn Esimerkki](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Etsi "SQL-prosessien komentorivillä
Tyyppi = AdmProcess_CL CommandLine_s = \*sql\* | dedup ProcessId_s

![ADM kyselyn Esimerkki](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process-use-its-machine-id-to-retrieve-the-computers-name"></a>Tapahtuman tietojen tarkasteleminen annetulle jälkeen käsitellä, sen koneen tunnuksen avulla voit noutaa sen tietokoneen nimi
Tyyppi = "m!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" AdmComputer_CL | Eri ComputerName_s

![ADM kyselyn Esimerkki](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>Luettele kaikki SQL-tietokoneissa
Tyyppi = AdmComputer_CL MachineId_s IN {tyyppi = AdmProcess_CL \*sql\* | Eri MachineId_s} | Eri ComputerName_s

![ADM kyselyn Esimerkki](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Kaikkien yksilöllisen product kääntö Omat joten-versioiden luettelo
Tyyppi = AdmProcess_CL Name_s = kääntö | Eri ProductVersion_s

![ADM kyselyn Esimerkki](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Kaikkien tietokoneissa CentOS tietokoneen ryhmän luominen

![ADM kyselyn Esimerkki](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Diagnostiikka-ja käyttötiedot
Microsoft kerää automaattisesti sovelluksen riippuvuus näyttö-palvelun käyttöä tietojen käyttöä ja suorituskykyä. Microsoft käyttää näitä tietoja ja laatua, suojaus ja eheys sovelluksen riippuvuus näyttö-palvelun. Tiedot on tietoja siitä, että käyttöjärjestelmä ja versio ohjelmistojen määritykset ja myös jotta ovat yhdenmukaisia ja tehokkaan vianmäärityksen ominaisuuksia IP-osoite, DNS-nimen ja työaseman nimi. Emme kerää nimiä, osoitteita tai muita yhteystietoja.

Katso lisätietoja tietojen kerääminen ja käyttö [Microsoft Online Services-tietosuojatiedot](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja [Kirjaudu haut](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.)
