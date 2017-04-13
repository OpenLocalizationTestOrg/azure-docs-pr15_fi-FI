<properties
   pageTitle="Sovelluksen riippuvuus näyttö (ADM) määrittäminen toimintojen hallinta-ohjelmistopaketin (OMS) | Microsoft Azure"
   description="Sovelluksen riippuvuus näyttö (ADM) on toimintojen hallinta Suite (OMS)-ratkaisu, joka löytää sovelluksen osia Windows- ja Linux järjestelmiin ja yhdistää välisen palvelut automaattisesti.  Tässä artikkelissa on tietoja käyttöönotosta ADM ympäristön ja käyttää sitä sekä skenaariot."
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

# <a name="configuring-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Sovelluksen riippuvuus näyttö-ratkaisun määrittäminen toimintojen hallinta Suite (OMS)
![Ilmoitusten hallinta-kuvake](media/operations-management-suite-application-dependency-monitor/icon.png) Sovelluksen riippuvuus näyttö (ADM) löytää sovelluksen osia Windows- ja Linux järjestelmiin ja yhdistää välisen palvelut automaattisesti. Sen avulla voit tarkastella palvelinten, voit ajatella hänet yhdistetty järjestelmät, jotka toimittavat tärkeiden palveluiden.  Sovelluksen riippuvuuden näyttötavan-palvelimet-prosessien väliset yhteydet ja portit yli minkä tahansa TCP-yhteys-arkkitehtuuri ja määrityksiä vaaditaan vain asennettuun agentti.

Tässä artikkelissa on tietoja määrittäminen sovelluksen riippuvuus näyttö ja onboarding agenttien vuoksi.  Lisätietoja ADM käyttämisestä on artikkelissa [käyttämällä sovelluksen riippuvuuden näyttö-ratkaisun toimintojen hallinta Suite (OMS)](operations-management-suite-application-dependency-monitor.md)

>[AZURE.NOTE]Sovelluksen riippuvuus näyttö on tällä hetkellä yksityinen esikatselu.  Voit pyytää ADM yksityinen esikatselu [https://aka.ms/getadm](https://aka.ms/getadm)on pääsy.
>
>Yksityinen esikatselun aikana kaikki OMS-tileillä on oikeus ADM.  ADM solmut ovat maksuttomia, mutta lokin Analytics-tiedot AdmComputer_CL ja AdmProcess_CL mitataan kuten muiden ratkaisu.
>
>Kun ADM siirtyy public preview-version, se on käytettävissä vain maksuttomia ja maksullisia asiakkaiden tietoja ja Analytics-OMS hinnat suunnitteleminen.  Vapaa taso tilit rajoittuvat 5 ADM solmujen.  Jos on meneillään yksityinen esikatselu ja ovat ei ole rekisteröity OMS hinnat suunnitteleminen ADM tullessa public preview-version, ADM eivät ole käytettävissä tällä kertaa. 



## <a name="connected-sources"></a>Yhdistettyjen lähteiden
Seuraavassa taulukossa on kuvattu yhdistettyjen lähteiden on ADM-ratkaisun tukemat.

| Yhdistettyjen lähde | Tuettu | Kuvaus |
|:--|:--|:--|
| [Windowsin agenttien vuoksi](../log-analytics/log-analytics-windows-agents.md) | Kyllä | ADM analysoi ja kerää tietoja Windows agentti tietokoneista.  <br><br>Huomaa, että lisäksi OMS-agentti Windows tekijöiden edellyttää Microsoft Agent riippuvuuden.  Katso luettelo kaikista käyttöjärjestelmien [Tuetut käyttöjärjestelmät](#supported-operating-systems) . |
| [Linux agenttien vuoksi](../log-analytics/log-analytics-linux-agents.md) | Kyllä | ADM analysoi ja kerää tietoja Linux agentti tietokoneista.  <br><br>Huomaa, että lisäksi OMS-agentti Linux tekijöiden edellyttää Microsoft Agent riippuvuuden.  Katso luettelo kaikista käyttöjärjestelmien [Tuetut käyttöjärjestelmät](#supported-operating-systems) . |
| [SCOM hallinta-ryhmä](../log-analytics/log-analytics-om-agents.md) | Kyllä | ADM analysoi ja kerää tietoja Windowsin ja Linux agenttien vuoksi yhdistetyn SCOM hallinta-ryhmässä. <br><br>Suoran yhteyden SCOM agentti tietokoneesta OMS tarvitaan. Tiedot lähetetään suoraan välittää hallinta-ryhmästä OMS säilöön.|
| [Azure-tallennustilan tilin](../log-analytics/log-analytics-azure-storage.md) | Ei | ADM kerää tietoja tietokoneiden, jotta siinä ei ole tietoja siitä, johon kerätään Azure-tallennustilan. |

Huomaa, että ADM tukee vain 64-bittisten käyttöjärjestelmien työpöytäsovelluksiin.

Windows, Microsoft seuranta agentti (MMA) käytetään SCOM ja OMS kerää ja lähetä-tietojen valvominen.  (Tämä agentti kutsutaan SCOM Agent, OMS agentti, MMA tai suoraan agentti kontekstin mukaan.)  SCOM ja OMS sisältävät eri ulos MMA ruutuun versioissa, mutta näitä versioita voi jokaisen raportin SCOM, OMS tai molempia.  Valitse Linux-Linux kerää ja lähettää OMS-tietojen valvominen OMS-agentti.  Voit käyttää ADM palvelimissa OMS suoraan tekijöiden kanssa tai palvelimissa, jotka on liitetty OMS SCOM hallinta ryhmät kautta.  Näitä ohjeita varten on viittaavat kaikki nämä tekijöiden – Linux- tai Windows-onko yhteydessä SCOM MG tai suoraan OMS – "OMS agentti", ellei edustajan käyttöönotto-nimi tarvita kontekstissa.

ADM-agentti ei siirrä itse tietoja, ja se ei edellytä palomuurit tai portit muutosten.  OMS-edustaja, OMS, joka lähetetään ADM käyttäjän tiedot aina, joko suoraan tai OMS yhdyskäytävän kautta.

![ADM agenttien vuoksi](media/operations-management-suite-application-dependency-monitor/agents.png)

Jos olet SCOM asiakkaan yhteydessä OMS hallinta ryhmän kanssa:

- Jos SCOM edustajasi voit muodostaa yhteyden OMS Internetiä, tarvitaan lisämäärityksiä.  
- Jos SCOM edustajasi voi käyttää OMS Internetin kautta, sinun on määrittäminen toimimaan SCOM OMS-yhdyskäytävä.
  
Jos käytössäsi on suora OMS-agentti, haluat määrittää OMS agentti itse muodostaa OMS tai OMS-yhdyskäytävä.  OMS yhdyskäytävän voi ladata [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666)


### <a name="avoiding-duplicate-data"></a>Tietojen kaksoiskappaleiden välttäminen

Jos olet SCOM asiakas, kannattaa määrittää SCOM edustajasi toimittaa suoraan OMS ei tai tietojen raportoidaan kahdesti.  ADM tuloksena kahdesti Machine-luettelossa näkyvä tietokoneissa.

OMS määrittäminen tapahtuu vain yhteen seuraavista sijainneista:

- Hallitun tietokoneissa SCOM konsolin toimintojen hallinta Suite-ruutu
- Azure toiminnallisia havainnollistamisen määritysten MMA ominaisuuksiin

Molemmat käyttömahdollisuudet käyttäminen samassa työtilan kaikkien aiheuttaa tietojen kopioinnista. Käyttämällä sekä eri työtilat voi aiheuttaa ristiriitaisia määrityksistä (yksi ADM ratkaisu käytössä ja toisen ilman), jotka voivat estää tietojen juoksutus ADM kokonaan.  

Huomaa, että vaikka koneen itse ei ole määritetty SCOM-konsolin OMS kokoonpanossa, jos esiintymä-ryhmän, esimerkiksi "Windows Server esiintymät Group" on aktiivinen, se voi silti johtaa vastaanottamisessa OMS kokoonpanossa SCOM kautta.



## <a name="management-packs"></a>Management Pack-paketit
Kun ADM aktivoidaan OMS-työtilassa, 300 kt Management Pack lähetetään kaikki Microsoft seuranta agenttien vuoksi työtilan.  Jos käytät SCOM tekijöiden [yhdistetty hallinta-ryhmä](../log-analytics/log-analytics-om-agents.md), otetaan käyttöön ADM Management Pack-SCOM.  Jos niistä on suoraan yhteydessä, MP toimittaa OMS.

MP nimetään Microsoft.IntelligencePacks.ApplicationDependencyMonitor*.  Se on kirjoitettu *%Programfiles%\Microsoft seuranta Agent\Agent\Health State\Management palvelupakettien\*.  Management pack käyttämän tietolähteen on *% ohjelma files%\Microsoft seuranta Agent\Agent\Health palvelun State\Resources\<AutoGeneratedID > \Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Määritys
Lisäksi Windows ja Linux tietokoneissa on asennettuna ja liitettynä OMS agentti, riippuvuuden agentti asennusohjelman on ladattava ADM-ratkaisusta ja asennettu pääkansion tai järjestelmänvalvojan kussakin hallitun palvelimessa.  Kun ADM-agentti on asennettu raportoinnin OMS palvelimessa, ADM riippuvuuden kartat näkyvät 10 minuutin kuluessa.  Jos sinulla on ongelmia, Tarkista sähköposti [oms-adm-support@microsoft.com](mailto:oms-adm-support@microsoft.com).


### <a name="migrating-from-bluestripe-factfinder"></a>Siirtyminen BlueStripe FactFinder:
Sovelluksen riippuvuuden näytön toimittaa BlueStripe tekniikka kyselyjä OMS vaiheissa. FactFinder asiakkaille tuetaan, mutta ei ole enää ostettavissa yksittäisiä.  Riippuvuus-agentti esikatselu versiossa voit vain yhteyttä OMS.  Jos olet nykyisen FactFinder-asiakas, tunnistaa testi palvelinten joukko ADM, joita ei ole hallitaan FactFinder varten. 

### <a name="download-the-dependency-agent"></a>Lataa riippuvuus-agentti
Microsoft Management agentti (MMA) ja jotka tarjoavat tietokoneen ja OMS välinen yhteys OMS Linux agentti lisäksi analysoida sovelluksen riippuvuus tarkkailu kaikissa tietokoneissa on oltava asennettuna riippuvuuden agentti.  Linux-Linux OMS-agentti on oltava asennettuna ennen riippuvuus-agentti. 

![Sovelluksen riippuvuus näyttölaitteen kuvaketta](media/operations-management-suite-application-dependency-monitor/tile.png)

Jotta voit ladata riippuvuus-agentti, valitse **Määrittäminen ratkaisu** **Sovelluksen riippuvuus näyttö** -ruudun Avaa **Riippuvuuden Agent** -sivu.  Riippuvuus Agent-sivu on linkkejä Windowsin ja Linux agenttien vuoksi. Valitse tarvittavat kunkin agentti latauslinkkiä. Katso lisätietoja seuraavissa osissa agentti asentamista toinen järjestelmiin.

### <a name="install-the-dependency-agent"></a>Asenna riippuvuus-agentti

#### <a name="microsoft-windows"></a>Microsoft Windows
Voit asentaa ja poistaa agentti tarvitaan järjestelmänvalvojan oikeudet.

Riippuvuus-agentti on asennettu Windows-tietokoneissa, joissa on ADM-agentti-Windows.exe. Jos suoritat näin suoritettava ilman mitään asetuksia, se käynnistyy ohjattu vaiheittaisten ohjeiden avulla voit suorittaa asennusta vuorovaikutteisesti.  

Seuraavien vaiheiden avulla voit riippuvuus-agentti asentaminen Windows-tietokoneissa.

1.  Varmista, että OMS-agentti on asennettu Yhdistä tietokoneiden siirtyä OMS ohjeita.
2.  Lataa Windows-agentti ja suorita seuraavalla komennolla.<br>*ADM-agentti-Windows.exe*
3.  Noudata ohjatun toiminnon asentaa agentti.
4.  Jos riippuvuus-agentti ei käynnisty, tarkistaa kirjaa virheen yksityiskohtaiset tiedot. Log-kansio on *C:\Program Files\BlueStripe\Collector\logs*Windows agenttien vuoksi. 

Windowsin riippuvuuden agentti voidaan poistaa järjestelmänvalvojan Ohjauspaneelin kautta.


#### <a name="linux"></a>Linux
Pääkansio access tarvitaan asentaminen ja määrittäminen agentti.

Riippuvuus-agentti on asennettu Linux tietokoneissa, joissa on ADM-agentti-Linux64.bin-liittymän komentosarja itse talteen binaaritiedosto kanssa. Voit suorittaa tiedoston nä tai lisätä suorittaa itse tiedoston käyttöoikeudet.
 
Seuraavien vaiheiden avulla voit asentaa riippuvuus-agentti Linux kussakin tietokoneessa.

1.  Varmista, että OMS-agentti on asennettu ohjeiden etsiminen [kerää ja tietojen hallinta Linux tietokoneista.  Tämä on asennettava ennen Linux riippuvuuden agentti](https://technet.microsoft.com/library/mt622052.aspx).
2.  Asentaa Linux riippuvuus-agentti pääkansion seuraavan komennon avulla.<br>*nä ADM-agentti-Linux64.bin*.
3.  Jos riippuvuus-agentti ei käynnisty, tarkistaa kirjaa virheen yksityiskohtaiset tiedot. Log-kansio on */var/opt/microsoft/dependency-agent/log*Linux agenttien vuoksi.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>Riippuvuus-agentti Linux asennuksen poistaminen
Poista kokonaan riippuvuus-agentti Linux, sinun on poistettava agentti itse ja johon on asennettu automaattisesti agentti välityspalvelimen.  Voit poistaa molemmat yhteen seuraavalla komennolla.

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Asentaminen komentoriviltä
Edellisen osan ohjeita asentamisesta riippuvuuden näyttö-agentti oletusasetuksilla.  Seuraavissa osissa esitellään asentamiseen agentti komentoriviltä käyttämällä mukautettuja asetuksia.

#### <a name="windows"></a>Windows
Alla olevasta taulukosta asetusten avulla voit suorittaa asennusta komentoriviltä. Luettelo asennuksen liput suorittamalla asennusohjelma kanssa /? Merkitse seuraavasti.

    ADM-Agent-Windows.exe /?

| Merkintä | Kuvaus |
|:--|:--|
| /S | Suorittaa automaattista asennusta ilman käyttäjän vahvistusta. |

Windows-riippuvuus agentti tiedostot sijoitetaan *C:\Program Files\BlueStripe\Collector* oletusarvoisesti.


#### <a name="linux"></a>Linux
Alla olevasta taulukosta asetusten avulla voit suorittaa asennusta. Saat näkyviin luettelon merkinnät Suorita asennuksen asennuksen ohjelman ohje - merkinnän seuraavasti.

    ADM-Agent-Linux64.bin -help

| Merkinnän kuvaus
|:--|:--|
| -s | Suorittaa automaattista asennusta ilman käyttäjän vahvistusta. |
| --tarkistaminen | Tarkistaa käyttöoikeudet ja käyttöjärjestelmän, mutta eivät asennu agentti. |

Riippuvuus-agentti tiedostot ovat käytettävissä seuraaviin kansioihin.

| Tiedostot | Sijainti |
|:--|:--|
| Core tiedostot | /USR/lib/bluestripe-collector |
| Lokitiedostojen | /var/Opt/Microsoft/dependency-Agent/log |
| Config tiedostot | /ETC/Opt/Microsoft/dependency-Agent/Config |
| Palvelun suoritettavat | /sbin/bluestripe-collector<br>/sbin/bluestripe-collector-Manager |
| Tiedostojen binaarinen tallentaminen | /var/Opt/Microsoft/dependency-Agent/Storage |



## <a name="troubleshooting"></a>Vianmääritys
Jos kohtaat ongelmia sovelluksen riippuvuus näyttöä, voit kerätä vianmääritystietoja seuraavat tiedot käyttämällä useita komponenteilta.

### <a name="windows-agents"></a>Windowsin agenttien vuoksi

#### <a name="microsoft-dependency-agent"></a>Riippuvuus Microsoft Agent
Luo vianmääritystiedot riippuvuus-agentti, Avaa komentorivi järjestelmänvalvojana ja suorittamalla seuraavan komennon avulla CollectBluestripeData.vbs komentosarjan.  Voit lisätä--ohjeen merkintä ja näyttää Lisäasetukset.

    cd C:\Program Files\Bluestripe\Collector\scripts
    cscript CollectDependencyData.vbs

Tuki tietojen pakkaus on tallennettu nykyisen käyttäjän % USERPROFILE %-kansioon.  Voit käyttää--tiedoston <filename> vaihtoehto, jos haluat tallentaa sen eri sijaintiin.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Riippuvuus Microsoft Agent Management Pack for MMA
Riippuvuus-agentti hallintapaketin suorittaa Microsoft Management Agent sisällä.  Se hakee tietoja riippuvuus-agentti ja välittää sen ADM pilvipalveluun.
  
Varmista, että management pack ladataan suorittamalla seuraavat vaiheet.

1.  Etsi tiedosto nimeltä Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp C:\Program Files\Microsoft seuranta Agent\Agent\Health State\Management palvelupakettien.  
2.  Jos tiedosto ei ole ja agentti on yhdistetty SCOM hallinta-ryhmä, varmista, että se on tuotu SCOM valitsemalla Toiminnot-konsolin Management Pack hallinta-työtilassa.

ADM MP kirjoittaa tapahtumien Operations Manager Windowsin tapahtumalokiin.  Kirjaudu voi olla järjestelmän log-ratkaisun kautta [etsitään OMS](../log-analytics/log-analytics-log-searches.md) jossa voit määrittää minkä lokitiedostojen lataaminen.  Jos virheenkorjaus tapahtumat ovat käytössä, ne on kirjoitettu tapahtumaloki tapahtuman tietolähteeseen *AdmProxy*.

#### <a name="microsoft-monitoring-agent"></a>Microsoft Agent seuranta
Kerää diagnostiikan jäljittää, Avaa komentorivi järjestelmänvalvojana ja suorittamalla seuraavat komennot: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Tapahtumat kirjoitetaan c:\Windows\Logs\OpsMgrTrace.  Voit lopettaa jäljityksen StopTracing.cmd kanssa.


### <a name="linux-agents"></a>Linux agenttien vuoksi

#### <a name="microsoft-dependency-agent"></a>Riippuvuus Microsoft Agent
Voit luoda vianmääritystiedot riippuvuuden agentti, kirjaudu sisään tilille, jolla on oikeudet sudo tai pääkansion ja suorittamalla seuraavan komennon.  Voit lisätä--ohjeen merkintä ja näyttää Lisäasetukset.

    /usr/lib/bluestripe-collector/scripts/collect-dependency-agent-data.sh

Tuki tietojen pakkaus on tallennettu /var/opt/microsoft/dependency-agent/log (Jos pääkansion) agentti asennuksen hakemiston tai pääkansion käyttäjän komentosarja (Jos muu-pääkansio).  Voit käyttää--tiedoston <filename> vaihtoehto, jos haluat tallentaa sen eri sijaintiin.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Riippuvuus Microsoft Agent Fluentd Linux laajennus
Riippuvuus agentti Fluentd laajennuksen suorittaa OMS Linux-agentti sisällä.  Se hakee tietoja riippuvuus-agentti ja välittää sen ADM pilvipalveluun.  

Lokit kirjoitetaan seuraavat kaksi tiedostoa.

- /var/Opt/Microsoft/omsagent/log/omsagent.log
- /var/log/messages

#### <a name="oms-agent-for-linux"></a>Linux OMS-agentti
Täältä löytyvät vianmäärityksen resurssin Linux-palvelimet muodostamisesta OMS: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

Lokien Linux OMS-agentti sijaitsevat */var tai valita/microsoft/omsagent/lokin/*.  

Lokien omsconfig (agentti määritys) sijaitsevat */var tai valita/microsoft/omsconfig/lokin/*.
 
Lokissa OMI ja SCX osat, joiden suorituskyvyn mittarit tiedot sijaitsevat */var tai valita/omi/lokin/* ja */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Tietojen kerääminen
Voit odottaa kunkin agentti siirtämiseen suurin piirtein 25 Mt päivässä sen mukaan, miten monimutkainen järjestelmän riippuvuudet ovat.  ADM riippuvuuden tiedot lähetetään mukaan kunkin agentti 15 sekunnin välein.  

ADM-agentti siinä käytetään yleensä 0,1 muistin ja 0,1 prosentin järjestelmän suorittimen.


## <a name="supported-operating-systems"></a>Tuetut käyttöjärjestelmät
Seuraavissa osioissa on lueteltu tuetut käyttöjärjestelmät riippuvuus-agentti.   32-bittisiä ei tue sellaisille käyttöjärjestelmille.

### <a name="windows-server"></a>Windows Server
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows-työpöytä
- Huomautus: Windows 10 ei vielä tueta
- Windows 8.1
- Windows 8: ssa
- Windows 7: ssä

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Punainen on Enterprise Linux, CentOS Linux ja Oracle Linux (ja RHEL ydin)
- Oletusarvoiset ja SMP Linux ydin versioiden tuetaan.
- Normaalista ydin versiot, kuten PAE ja Xen, ei tueta minkä tahansa Linux-jakauman. Esimerkiksi "2.6.16.21-0.8-xen" release-merkkijonolla järjestelmän ei tueta.
- Mukautetun ytimet, mukaan lukien vakio ytimet uudelleenkääntämisiä ei tueta
- Centos Plus ydin ei tueta.
- Oracle Unbreakable ydin (UEK) käsitellään eri alla olevasta osiosta.


#### <a name="red-hat-linux-7"></a>Punainen on Linux 7
| Käyttöjärjestelmäversio | Ydinversio |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |

#### <a name="red-hat-linux-6"></a>Punainen on Linux 6
| Käyttöjärjestelmäversio | Ydinversio |
|:--|:--|
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6,7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Punainen on Linux 5
| Käyttöjärjestelmäversio | Ydinversio |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5,9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Oracle Enterprise Linux ja Unbreakable ydin (UEK)

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Käyttöjärjestelmäversio | Ydinversio
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Käyttöjärjestelmäversio | Ydinversio
|:--|:--|
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5,9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Käyttöjärjestelmäversio | Ydinversio
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Käyttöjärjestelmäversio | Ydinversio
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Diagnostiikka-ja käyttötiedot
Microsoft kerää automaattisesti sovelluksen riippuvuuden näyttö-palvelun käyttöä tietojen käyttöä ja suorituskykyä. Microsoft käyttää näitä tietoja ja laatua, suojaus ja eheys sovelluksen riippuvuuden näyttö-palvelun. Tiedot on tietoja siitä, että käyttöjärjestelmä ja versio ohjelmistojen määritykset ja myös jotta ovat yhdenmukaisia ja tehokkaan vianmäärityksen ominaisuuksia IP-osoite, DNS-nimen ja työaseman nimi. Emme kerää nimiä, osoitteita tai muita yhteystietoja.

Katso lisätietoja tietojen kerääminen ja käyttö [Microsoft Online Services-tietosuojatiedot](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Seuraavat vaiheet
- Katso, miten voit [käyttää sovelluksen riippuvuuden näyttöä](operations-management-suite-application-dependency-monitor.md) kerran se on otettu käyttöön ja määritetty.
