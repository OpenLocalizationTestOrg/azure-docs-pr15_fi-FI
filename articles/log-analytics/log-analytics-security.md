<properties
    pageTitle="Kirjaudu Analytics-tietojen suojauksen | Microsoft Azure"
    description="Tietoja siitä, miten Log Analytics suojaa yksityisyyttäsi ja suojaa tiedot."
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
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Kirjaudu Analytics-tietojen suojaaminen

Microsoft on sitoutunut suojaamaan yksityisyyttäsi ja tietojen suojaaminen pitäminen ohjelmistoja ja palveluja, avulla voit hallita organisaation IT-infrastruktuurin. Olemme huomioi, että kun tietojen muille tehtäväksi, että Salli edellyttää tiukka suojausta. Microsoft noudattaa tarkka yhteensopivuus ja suojauksen ohjeita –-koodaus, toimivat palvelu.

Suojaaminen ja tietojen suojaaminen on Microsoftin yläreunan prioriteetti. Ota yhteyttä kysymyksiä, ehdotuksia tai jokin seuraavat tiedot, mukaan lukien Microsoftin suojauskäytäntöjä osoitteessa [Azure tukivaihtoehdot](http://azure.microsoft.com/support/options/)ongelmista.

Tässä artikkelissa kerrotaan, miten tietojen kerääminen, käsitteleminen ja suojattu Log Analytics toimintojen hallinta Suite (OMS). Tekijöiden avulla voit muodostaa web-palvelu, kun käytät System Center Operations Manager toiminnallisia tietojen keräämiseen tai tietojen noutaminen Azure diagnostiikka Log Analytics käytettäväksi. Kerättyjen tietojen lähetetään Internetin välityksellä käyttämällä lokiin Analytics-palvelu, mikä Microsoft Azure nykyisessä varmenteen ja SSL 3. Tietojen pakkaus on mukaan agentti ennen sen lähettämistä.

Lokitiedoston Analytics-palvelu hallitsee pilvipohjainen tiedot suojatusti käyttämällä seuraavista tavoista:

- tietoja eriytymistä
- tietojen säilytys
- Fyysinen suojaus
- tapauksen hallintaan
- yhteensopivuus
- suojauksen standardit varmenteet


## <a name="data-segregation"></a>Tietoja eriytymistä

Asiakastietojen pidetään jokaisen osan koko OMS-palvelun loogisesti erillään. Kaikki tiedot on merkitty organisaation kohden. Tämä tunnisteita jatkuu koko tietojen elinkaari ja kunkin tasolla palvelun pakotettu. Kukin asiakas on erillinen Azure-Blob-objektien joka sisältää pitkään tiedot

## <a name="data-retention"></a>Tietojen säilytys

Indeksoidut haun lokitiedot tallennetaan ja säilyttää hinnoittelu palvelupaketin mukaan. Lisätietoja on artikkelissa [Log Analytics hinnat](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft poistaa asiakastietoja 30 päivän OMS työtila on suljettu. Microsoft myös poistaa Azure-tallennustilan tilin jossa tiedot sijaitsevat. Kun asiakkaan tiedot poistetaan, ei ole fyysiset asemat häviävät.

Seuraavassa taulukossa on lueteltu joitakin OMS ja esimerkkejä tietojen lajit ne käytettävissä ratkaisuja.

| **Ratkaisu** | **Tietotyypit** |
| --- | --- |
| Kokoonpanon arviointi | Kokoonpanotietoja, metatietojen ja tila-tiedot |
| Kapasiteetin suunnittelu | Suorituskyvyn tiedot ja metatiedot |
| Antimalware | Määritysten tiedot ja metatiedot |
| Järjestelmässä päivitys arviointi | Metatieto-ja tila |
| Lokitiedoston hallinta | Käyttäjän määrittämät tapahtumalokit, Windowsin tapahtumalokien ja/tai IIS-lokit |
| Muutosten jäljityksen | Ohjelmiston varaston ja Windows-palvelun metatiedot |
| SQL- ja Active Directory-arviointi | WMI-tietoja, rekisteritietoja, suorituskykytietoja ja SQL Server dynaaminen hallinta Tarkastele tuloksia |

Seuraavassa taulukossa on esimerkkejä tietotyypit:

| **Tietotyyppi** | **Kentät** |
| --- | --- |
| Ilmoitus | Ilmoita nimi, ilmoituksen kuvaus, BaseManagedEntityId, ongelman tunnuksen, IsMonitorAlert, RuleId, ResolutionState, prioriteetti, vakavuus, luokka, omistaja, ResolvedBy, TimeRaised, TimeAdded, LastModified, viimeksi muokannut, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Määritys | Asiakastunnus, AgentID, tunnus, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Tapahtuman | EventId, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, numero, luokka, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Huomautus:** Kun kirjaudut tapahtumien mukautetut kentät Windowsin tapahtumalokiin, OMS kerää ne. |
| Metatietojen | BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IP-osoite, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-osoite, NetbiosDomainName, LogicalProcessors, DNSName, näyttönimi, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Suorituskyky | Objektin nimi, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Tila | StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekstissa, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fyysinen suojaus

Log-Analytics OMS-palvelun miehistönä on Microsoftin henkilöstön ja kaikki tehtävät kirjautunut ja voi seurata. Palvelu suoritetaan kokonaan Azure ja Azure yleisiä suunnitteluryhmät vaatimusten mukainen. Voit tarkastella tietoja Azure varat fyysinen turvallisuus [Microsoft Azure suojauksen yleiskatsaus](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf)-sivulla 18. Fyysinen käyttöoikeudet suojaamiseen alueet muutetaan yhden päivän kaikki, joilla on enää vastuu OMS-palvelua, mukaan lukien siirto ja lopettamista varten. Voit lukea tietoja yleisen fyysinen infrastruktuurin Käytämme [Microsoft palvelinkeskusten](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx)-palvelussa.

## <a name="incident-management"></a>Tapauksen hallintaan

OMS on tapauksen hallintaprosessi, joka noudattaa kaikki Microsoftin palvelut. Yhteenveto on:

- Käyttää jaettua vastuu mallin suojaus vastuu osa sijaintipaikka Microsoft samalla, kun osa kuuluu asiakkaalle
- Azure suojauksen tapaukset hallinta
  - Tunnista tapahtuma on ensimmäinen merkintä, Aloita tutkimuksen
  - Arvioi, vaikutus ja vakavuus tapahtuma-puhelun tapauksen vastaus-ryhmän jäsen. Perusteella näyttöä arvioinnin voi tai ei voi seurata edelleen Vie vastauksen suojaus ryhmälle.
  - Vianmääritys tapahtuma suojauksen vastauksen asiantuntijoiden Järjestä teknisiä tai lainmukaisten tutkimuksen, sisältyvyys, rajoituksen ja vaihtoehtoinen menetelmä selvittäminen. Jos suojaus-ryhmän uskoo, että asiakastiedot ehkä on muuttuvat tarjoamia lainvastaiseen tai luvattoman yksittäiselle, rinnakkain suorittamisen asiakkaan tapahtumaa ilmoituksen yhteydessä alkaa rinnakkain.  
  - Yrittää vahvistaa ja palauttaminen ajankohta. Tapauksen vastaus-ryhmästä Luo palautus suunnitelma pienentämään ongelman. Varalta sisältymisiä vaiheet, kuten ehkäisytoimenpiteet sellaisiin järjestelmien voi ilmetä välittömästi ja vianmäärityksen kanssa samanaikaisesti. Pidempi termin ongelman pienentämistavat saattaa olla suunniteltu jotka tapahtuvat heti riski kuluttua.  
  - Sulje ajankohta ja Järjestä postmortem. Tapauksen vastaus-ryhmästä Luo jälkiselvittely, joka määrittelee tapahtumasta kanssa aikoo muutetaan toimintaperiaatteiden, menettelytapojen ja prosessit voivat estää reoccurrence tapahtuman tiedot.
- Ilmoita asiakkaille suojauksen tapaukset
  - Laajuuden sellaisiin asiakkaiden ja antamaan kuka tahansa käyttäjä, jotka vaikuttaa tarkasti mahdollisimman ilmoitus
  - Luo ilmoitus tarjoaminen asiakkaille, joilla on tarkat tarpeeksi tietoa, niin, että he voivat suorittaa tutkimuksen niiden lopussa ja täyttävät kaikki sitoumukset ne niiden käyttäjille tehnyt viivästyy ei perusteettomasti ilmoituksen prosessin aikana.
  - Vahvista ja määritellä tapahtumaa tarpeen mukaan.
  - Ilmoita asiakkaille, joilla on tapauksen ilmoituksen tekemäänsä viipymättä ja kaikki oikeudelliset tai sopimukseen sitoumusta. Suojauksen tapaukset-ilmoitus lähetetään vähintään yksi asiakkaan järjestelmänvalvojat millä tahansa Microsoft valitsee, mukaan lukien sähköpostitse.
- Järjestä ryhmän valmiuksien ja koulutus
  - Microsoftin henkilöstön on suojaus ja tilatieto koulutus, jonka avulla he voivat tunnistaa ja raportoida ongelmista mahdolliset suojaus.  
  - Työskentelevän Microsoft Azure-palvelusta on lisäksi koulutus velvoitteiden ympärillä pääsyä luottamuksellisia järjestelmien isännöinnin asiakastiedot.
  - Microsoftin security vastauksen henkilöstön kouluttamisesta erityinen niiden roolien


Jos kaikki asiakkaan tietojen menettämistä, Ilmoita kunkin asiakkaan yhden päivän kuluessa. Asiakkaan tietojen menettämisen on kuitenkin koskaan ilmennyt OMS. Lisäksi on säilyttää tiedot, jotka on luotu kopioita ja maantieteellisesti jaetaan.

Saat lisätietoja siitä, miten Microsoft reagoi suojauksen tapahtumiin [Microsoft Azure suojauksen vastaus pilvessä](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Yhteensopivuus

OMS software development ja palvelun ryhmän tiedot suojaus- ja seurantatyökaluilla ohjelma tukee business vaatimukset, ja noudattaa lainsäädännön [Microsoft Azure-Valvontakeskus](https://azure.microsoft.com/support/trust-center/) ja [Microsoft luota Center yhteensopivuuden](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx)ohjeiden mukaisesti. Miten OMS määritetään suojausvaatimukset, tunnistaa turvaominaisuudet, hallinnoi ja valvoo riskien esitetään myös siellä. Vuosittain on Järjestä tarkastelun käytännöt standardeja, menettelytapojen ja ohjeita.

Kunkin OMS kehittäminen työryhmän jäsen vastaanottaa virallista sovelluksen suojauksen koulutusta. Sisäisesti Käytämme versio ohjausobjektin järjestelmän ohjelmisto kehittämiseen. Ohjelmiston kunkin projektin on suojattu versio.

Microsoft on tietosuojaan ja vaatimustenmukaisuuteen ryhmän, jonka hän ja kaikki palvelut Microsoft sopimuksen merkityksellisiin. Tietojen suojauksen hyväksyjien muodostavat ryhmän, ja ne ei ole liitetty suunnitteluryhmät Osastot, joiden kehittää OMS. Suojauksen hyväksyjien on omia hallinta ketju ja Järjestä riippumaton arvioita tuotteista ja palveluista, jotta tietosuojaan ja vaatimustenmukaisuuteen.

Microsoftin johtokunta ilmoittaa ja vuosikertomus tietoja kaikista tietosuojaa ohjelmia Microsoftin.

Microsoftin Legal and yhteensopivuus-ryhmiä ja muiden alan kumppanit voivat hankkia kaltaisilta erilaisia aktiivisesti käyttäminen OMS ohjelmiston kehittäminen ja palvelun ryhmän.

## <a name="security-standards-certifications"></a>Suojauksen standardit varmenteet

Kirjaudu Analytics OMS tällä hetkellä tavata suojauksen seuraavat vaatimukset:

- [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) ja [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498) yhteensopiva
- Maksun kortin Yleinen (PCI yhteensopiva) tietojen suojauksen standardi (PCI DSS) PCI turvallisuusneuvosto mukaisia.
- [Palvelun organisaation ohjausobjektit (SOC) 1 laji on 1 ja SOC 2 tyyppi 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) yhteensopiva
- Windowsin yhteiset suunnittelun perusteet
- Microsoftin luotettava tietojenkäsittely varmenteiden
- Azure palveluna osat, jotka OMS käyttää noudattaa Azure koskevien velvoitteiden täyttämisessä. Voit lukea lisää [Microsoft luota Center-yhteensopivuus](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx)-palvelussa.


## <a name="cloud-computing-security-data-flow"></a>Cloud security tiedonkulun tietojenkäsittely
Seuraavassa kaaviossa näkyy cloud suojausarkkitehtuuri tietoja yrityksestäsi ja miten se on suojattu sellaisenaan siirtää Log Analytics-palvelu, kädessä tarkastella voit OMS-portaalissa. Lisätietoja kunkin vaiheen seuraa kaavioon.

![Kuva OMS tietojen kerääminen ja suojaus](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. rekisteröityminen Log analyysin ja tietojen kerääminen

Tietojen lähettäminen lokin Analytics organisaation määrittäminen Windows tekijöiden agenttien vuoksi käytössä Linux-Azuren näennäiskoneiden tai OMS agenttien vuoksi. Jos käytät Operations Manager agenttien vuoksi, valitse käytetään ohjattu määritystoiminto toimintojen konsolissa määrittää niitä. Käyttäjien (joka voi olla voit, muiden yksittäisten käyttäjien tai käyttäjäryhmä) luo vähintään yksi OMS tilit (OMS työtilat) ja rekisteröi agenttien vuoksi käyttämällä jotakin seuraavista tileistä:


- [Organisaation tunnus](../active-directory/sign-up-organization.md)

- [Microsoft-tili - Outlookin Office Live MSN](http://www.microsoft.com/account/default.aspx)

OMS-työtila on, johon kerätään, koostetaan, analysoida, ja esittää. Työtilan käytetään pääasiassa maksuvälineenä osion tietoihin ja kunkin työtila on yksilöllinen. Haluat esimerkiksi on yksi OMS Workspacen hallitun tuotannon tietojen ja hallitaan sen mukaan, työtiloissa testitiedot. Työtilojen avulla järjestelmänvalvoja-ohjausobjektin käytettävyystietojen tietoihin. Kunkin työtilan voi olla useita käyttäjätilejä liittyy ja jokaisen käyttäjätilin voi käyttää useita OMS työtila. Voit luoda työtiloja palvelinkeskuksen alueen perusteella. Kunkin työtila on replikoida muihin palvelinkeskusten-alue, ensisijaisesti OMS palvelun saatavuus.

Kun ohjattu määritystoiminto on valmis, Operations Manager for Operations Manager hallinta kunkin ryhmän muodostaa yhteyden Log Analytics-palvelussa. Valitse käyttämällä ohjattua tietokoneiden lisääminen, mitä tietokoneita hallinta-ryhmässä on oikeus lähettää tiedot-palveluun. Agentti muiden apuohjelmatyyppien kunkin yhdistää suojatusti OMS-palvelun.

Kaikkien yhdistettyjen järjestelmien ja Log Analytics-palvelu välinen tietoliikenne on salattu.  TLS (HTTPS)-protokollaa käytetään salausta.  Sen jälkeen Microsoft SDL-prosessin varmistamiseksi Log Analytics on salattu protokollia viimeisin kehitys uusimmat.

Kullakin virhelajilla on agentti kerää tietoja lokin analysoinnissa. Tietotyyppi, jota kerätään on riippuvainen tiedostotyypit, käyttää ratkaisuja. Näet [Lisää Log Analytics](log-analytics-add-solutions.md)Solutions-sivustossa ratkaisuvalikoimasta liittyvä tietojen kerääminen yhteenveto. Lisäksi yksityiskohtaisempia sivustokokoelman tiedot ovat käytettävissä useimmissa ratkaisuja. Ratkaisu on paljon valmiiksi määritettyjä näkymiä, loki hakukyselyt, tietojen keräämisen säännöt ja käsittely logiikan. Vain järjestelmänvalvojat voivat käyttää lokiin Analytics ratkaisun tuominen. Kun ratkaisu on tuotu, se siirretään Operations Manager hallinta-palvelimiin (Jos käytössä) ja sitten tekijöiden, jotka olet valinnut. Jälkeenpäin niistä kerätä tietoja.

## <a name="2-send-data-from-agents"></a>2. tietojen lähettäminen agenttien vuoksi

Voit rekisteröidä agentti kaikenlaisten rekisteröinti-näppäintä ja suojattu yhteys on muodostettu agentti ja varmenteen ja SSL käyttäminen porttiin 443 Log Analytics-palvelun välillä. OMS käyttää salainen säilön voi luoda ja ylläpitää avaimet. Yksityiset avaimet ovat kierretään 90 päivän välein ja Azure tallennetaan ja hallitaan Azure toiminnot, jotka tarkka säädösten ja vaatimustenmukaisuus käytäntöjä.

Operations Manager työtilan rekisteröityä Log Analytics-palvelu ja suojatun HTTPS-yhteys on muodostettu Operations Manager management Serverin välillä.

Windowsin agenttien vuoksi Azuren näennäiskoneiden käytössä vain luku-tallennustilan näppäintä käytetään lukemaan Azure taulukoiden diagnostiikan tapahtumat.

Jos kaikki agentti ei voi muodostaa jostakin syystä-palveluun, kerätään tiedot tallennetaan paikallisesti tilapäinen välimuisti ja asiakirjanhallinnan palvelimeen yrittää lähettää tiedot minuutin välein 8 2 tuntia. Käyttöjärjestelmän tunnistetietosäilöä on suojattu agentti välimuistiin tallennetut tiedot. Jos palvelun ei voi käsitellä tietoja 2 tunnin jälkeen, niistä jono tiedot. Jos jonossa muuttuu koko, OMS käynnistyy pudottaminen tietotyyppejä, suorituskykytietoja alkaen. Agentti jonon rajana on rekisteriavaimen sen muokkaamista tarvittaessa. Kerättyjen tietojen on pakattu ja lähetetään palveluun ohittaminen paikallisen tietokannan, jotta ei lisätä minkä tahansa kuormituksen tietyille käyttäjille. Kun kerättyjen tietojen on lähetetty, se poistetaan välimuistista.

Yllä olevien ohjeiden mukaisesti edustajasi tiedot lähetetään Microsoft Azure palvelinkeskusten SSL-yhteyden kautta. Vaihtoehtoisesti voit säätää lisäsuojauksen tiedot ExpressRoute. ExpressRoute on tapa suoraan yhteyden Azure olemassa olevat WAN verkosta, kuten usean protokolla tarra vaihtaminen (MPLS) VPN-verkon palveluntarjoajan myöntämä. Lisätietoja on artikkelissa [ExpressRoute](https://azure.microsoft.com/services/expressroute/).


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Log Analytics-palvelu vastaanottaa ja käsittelee tietoja

Lokitiedoston Analytics-palvelu varmistaa, että saapuvat tiedot vahvistamalla varmenteet ja tietojen eheys Azure-todennuksen on peräisin luotettavasta lähteestä. Käsittelemättömän raaka tiedot tallennetaan [Microsoft Azuren](../storage/storage-introduction.md) tallennustilaan Blob-objektien ja salausta. Kunkin tallennustilan Azure-blob on kuitenkin ainutlaatuisia näppäimet on käytettävissä vain kyseisen käyttäjän joukko. Tietoja, jotka on tallennettu tyyppi on riippuvainen ratkaisuja, jotka on tuotu ja käyttää tietojen keräämiseen tyypit. Lokitiedoston Analytics-palvelu käsittelee Azure tallennustilan blogin raaka tiedot.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Käytä Log Analytics tietojen

Voit voit kirjautua Log Analytics OMS portaalissa organisaation tiliä tai Microsoft-tili, voit määrittää aiemmin avulla. Kaikki liikenne välillä OMS portaalin ja Log Analytics-OMS lähetetään suojatun HTTPS-kanavan kautta. Käytettäessä OMS portaalin istunnon tunnus luodaan käyttäjän asiakkaan (selain) ja tiedot tallennetaan paikallisen välimuistin, kunnes istunto katkaistaan. Kun lopetettu, välimuistin poistetaan. Asiakaspuolen evästeet, jotka eivät sisällä-henkilökohtaisia tietoja, ei poisteta automaattisesti. Istunnon evästeet merkitään HTTPOnly ja ovat paikallaan. Ennalta määritetty käyttämättömänä ajan kuluttua OMS portaalin istunto katkaistaan.

Käytä OMS-portaalissa, voit viedä tiedot CSV-tiedostoon ja voit käyttää tietoja Search-ohjelmointirajapinnan käyttäminen. CSV-vienti on rajoitettu 50 000 riviä / vienti ja API tiedot on rajoitettu 5 000 riviä kohden haku.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja lokin Analytics ja ryhtyä käyttämään minuutteina [Log Analytics käytön aloittaminen](log-analytics-get-started.md) .
