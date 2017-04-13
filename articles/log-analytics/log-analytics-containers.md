<properties
    pageTitle="Lokitiedoston Analytics säilöt-ratkaisun | Microsoft Azure"
    description="Lokitiedoston Analytics säilöt-ratkaisun avulla voit tarkastella ja hallita Docker säilö-isäntien yhteen paikkaan."
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



# <a name="containers-preview-solution-log-analytics"></a>Säilöjen (esikatselu)-ratkaisun Log Analytics

Tässä artikkelissa kerrotaan, miten voit määrittää ja käyttää säilöt-ratkaisun Log Analytics, jonka avulla voit tarkastella ja hallita Docker säilö-isäntien yhteen paikkaan. Docker on säilöjen, automatisoida ohjelmiston käyttöönoton IT-infrastruktuurin luomiseen käytetyn ohjelmiston virtualization järjestelmä.

Ratkaisuun näet säilöt ovat käytössä säilö-isäntien ja mitä kuvat ovat käynnissä säilöt. Voit tarkastella yksityiskohtaisia valvontatietoja säilöjen käyttämisestä komennot. Ja voit tehdä säilöjen tarkasteleminen ja etsimällä keskitetystä lokit ilman, että voit tarkastella etäyhteyden Docker isännät. Voit etsiä säilöjen häiriöistä ja vievää ylimääräisen resurssien isännän mahdollisesti. Ja voit tarkastella keskitetystä suorittimen, muistin, varasto ja säilöjen verkon käyttö- ja suorituskyvyn tiedot.

## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu

Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

Lisää säilöt-ratkaisun OMS työtilan käyttämisestä [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet.

Asenna ja Docker käyttämisestä OMS kahdella tavalla:

- Tuetut Linux-käyttöjärjestelmissä asentaminen ja suorittaminen Docker asentaminen ja OMS agentti määrittäminen Linux
- CoreOS, valitse Asenna ja suorita Docker ja määritä sitten OMSAgent suorittamiseen säilön sisällä

Tutustu säilö isäntä- [GitHub](https://github.com/Microsoft/OMS-docker)tuetut Docker ja Linux käyttöjärjestelmäversiot.

>[AZURE.IMPORTANT] Docker on oltava käynnissä **ennen** [OMS-agentti Linux](log-analytics-linux-agents.md) asentaminen säilö isännät. Jos olet jo asentanut agentti ennen asennusta Docker, tarvitset Linux OMS-agentti uudelleen. Saat lisätietoja Docker [Docker sivuston](https://www.docker.com).

Sinun on määritetty säilö isännät ennen kuin voit valvoa säilöjen seuraavia asetuksia.

## <a name="configure-settings-for-the-linux-container-host"></a>Linux säilö host asetusten määrittäminen

Sen jälkeen, kun olet asentanut Docker, käytä seuraavia asetuksia säilö isännän määrittää käytettäviksi agentti Docker. CoreOS ei tue määritysten tätä menetelmää.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>Voit määrittää asetukset säilö host - systemd (SUSE, openSUSE, CentOS 7.x, RHEL 7.x ja Ubuntu 15.x tai uudempi versio)

1. Muokkaa docker.service Lisää seuraavasti:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Lisää $DOCKER\_VALITSEE &quot;ExecStart = / usr/bin/docker daemon&quot; docker.service-tiedostossa. Esimerkin.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Käynnistä Docker-palvelu uudelleen. Esimerkki:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>Voit määrittää asetukset säilö host - Upstart (Ubuntu 14.x)

1. Muokkaa /etc/default/docker ja lisää seuraava:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Tallenna tiedosto ja Käynnistä Docker ja OMS-palvelut.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>Säilön host - Amazon Linux asetusten määrittäminen

1. Muokkaa /etc/sysconfig/docker ja lisää seuraava:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Tallenna tiedosto ja Käynnistä Docker-palvelun.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>CoreOS säilöjen asetusten määrittäminen

Kun olet asentanut Docker, avulla Suorita Docker ja luoda säilön CoreOS seuraavia asetuksia. Voit käyttää tuettua Linux-versiota, mukaan lukien CoreOS, määritys tällä menetelmällä. Sinun on myös [OMS työtilan tunnuksen ja avaimen avulla](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>OMS käytettävät kaikki säilöt CoreOS kanssa

- Aloita OMS säilö, jota haluat seurata. Muokkaa ja käytä seuraavassa esimerkissä.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Siirtyminen on asennettu edustaja, joka toinen säilön avulla

Jos käytetään suoraan asennettu-agentti aiemmin ja haluat käyttää sen sijaan säilön käynnissä agentti, sinun on poistettava OMSAgent. Katso [ohjeiden avulla voit asentaa Linux OMS-agentti](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Säilöjen sivustokokoelman tietojen

Säilöt-ratkaisun kerää eri suorituskyvyn mittarit ja kirjaudu tietoja säilö isännät- ja säilöjen OMS tekijöiden käytön Linux, jotka on otettu käyttöön ja OMSAgent säilöjen käynnissä.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmistä ja muita tietoja siitä, miten säilöjen kerätä tietoja.

| käyttöympäristö | Linux OMS-agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Linux|![Kyllä](./media/log-analytics-containers/oms-bullet-green.png)|![Ei](./media/log-analytics-containers/oms-bullet-red.png)|![Ei](./media/log-analytics-containers/oms-bullet-red.png)|            ![Ei](./media/log-analytics-containers/oms-bullet-red.png)|![Ei](./media/log-analytics-containers/oms-bullet-red.png)| 3 minuutin välein|


Seuraavassa taulukossa esitetään säilöt-ratkaisun keräämiä tietotyyppien:

| Tietotyyppi | Kentät |
| --- | --- |
| Suorituskyvyn parantaminen isäntien ja säilöt | Tietokone, nimi, CounterName & #40; % suoritinaika levyn lukee Mt, levyn kirjoittaa Mt, muistin käyttö Mt, verkon vastaanottaa tavua, verkon lähettää tavua, suorittimen käyttö sec verkon & #41; CounterValue, TimeGenerated, CounterPath, SourceSystem |
| Varaston säilö | TimeGenerated, tietokoneen nimi, ContainerHostname, kuva, ImageTag, ContinerState, ExitCode, EnvironmentVar, -komento, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID säilö |
| Säilön kuva varaston | TimeGenerated, tietokoneeseen, kuva, ImageTag, ImageSize, VirtualSize, suorittamisen ja keskeyttää, keskeyttää, epäonnistui, SourceSystem, ImageID, TotalContainer |
| Säilön loki | TimeGenerated, tietokoneeseen, kuvan tunnus, nimi, LogEntrySource LogEntry, SourceSystem, ContainerID säilö |
| Säilön loki | TimeGenerated, tietokoneeseen, TimeOfCommand, kuva-komento, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Näytön säilöt

Kun sinulla on käytössä OMS portaalissa ratkaisu, näet **säilöt** -ruutu näkyy yhteenvetotiedot säilö-isäntien ja säilöjen isännät käynnissä.

![Säilöjen-ruutu](./media/log-analytics-containers/containers-title.png)

Ruutu näyttää kuinka monta säilöjen on yleiskatsaus-ympäristön ja onko ei ole onnistunut, käynnissä vai pysäytetty.

### <a name="using-the-containers-dashboard"></a>Säilöjen hallintanäkymässä

Napsauta **säilöt** -ruutua. Sieltä näet näkymien järjestyksessä:

- Säilön tapahtumat
- Virheet
- Säilöjen tila
- Säilön kuva varaston
- Suorittimen ja muistin suorituskyky

Kunkin ruudun koontinäytön on haun, joka suoritetaan kerättyjen tietojen visuaalisessa muodossa.

![Säilöjen Raporttinäkymät-ikkunan](./media/log-analytics-containers/containers-dash01.png)

![Säilöjen Raporttinäkymät-ikkunan](./media/log-analytics-containers/containers-dash02.png)

**Säilön tila** -sivu valitsemalla ylin alueen alla kuvatulla tavalla.

![Säilöjen tila](./media/log-analytics-containers/containers-status.png)

Log-haun avautuu ja näyttää isännät- ja säilöjen käynnissä niihin tietoja.

![Säilöjen lokitiedoston etsiminen](./media/log-analytics-containers/containers-log-search.png)

Tässä kohdassa voit muokata hakukyselyn, voit muokata sitä voit löytää tiettyjä tietoja käyttämällä. Lisätietoja lokin haut artikkelissa [Log rajaamalla Log Analytics](log-analytics-log-searches.md).

Voit esimerkiksi muuttaa hakukyselyn niin, että se näyttää pysäytetty säilöt sijaan käynnissä säilöt muuttamalla **käynnissä** **pysäytetty** hakukyselyn.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Poistamalla etsiminen epäonnistui säilö

OMS merkitsee säilön **epäonnistui** jos se on suljettu, muu kuin nolla exit-koodi. Näet, virheitä ja virheiden **Epäonnistui säilöt** -sivu ympäristössä.

### <a name="to-find-failed-containers"></a>Voit etsiä epäonnistui säilöt

1. Valitse **Säilö tapahtumat** -sivu.  
  ![säilöjen tapahtumat](./media/log-analytics-containers/containers-events.png)
2. Log-haun avautuu ja näyttää seuraavankaltaiselta säilöjen tila.  
  ![säilöjen tila](./media/log-analytics-containers/containers-container-state.png)
3. Valitse seuraavaksi epäonnistui arvon, voit tarkastella lisätietoja, kuten kuvan koko ja määrä pysäytetty ja epäonnistui. Laajenna **Näytä Lisää** kuva ID-tunnuksellasi.  
  ![säilöjen epäonnistui](./media/log-analytics-containers/containers-state-failed.png)
4. Etsi seuraavaksi säilö, joka toimii Tässä kuvassa. Kirjoita Etsi-kyselyyn.
  `Type=ContainerInventory <ImageID>`Lokit näkyviin. Voit vierittää epäonnistui säilö.  
  ![säilöjen epäonnistui](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Etsi lokit säilö tiedot

Kun olet vianmääritys tiettyä virhettä, se voi auttaa nähdäksesi, missä se on käynnissä-ympäristössä. Log seuraavanlaisia avulla voit luoda kyselyjä palauttaa tiedot haluat.

- **ContainerInventory** – Käytä tätä tyyppiä, kun haluat lisätietoja säilön sijainti, heidän nimensä on ja mitä kuvien ne käynnissä.
- **ContainerImageInventory** – Käytä tätä tyyppiä, kun yrität löytää tiedot on järjestetty kuva ja voit tarkastella kuvan tietoja, kuten kuva tunnukset tai kokoa.
- **ContainerLog** – Käytä tätä tyyppiä, kun haluat etsiä tiettyä virhettä koskevien lokitiedot ja tapahtumat.
- **ContainerServiceLog** – Käytä tätä tyyppiä, kun yrität löytää kirjausketju valvontatietoja Docker daemon, kuten alku, Lopeta, poista tai hakea komentoja.

### <a name="to-search-logs-for-container-data"></a>Voit etsiä säilö tietojen lokit

- Valitse kuva, jonka tiedät epäonnistui viimeksi ja virhelokeja etsiminen. Aloita etsimällä säilössä, jossa on käytössä kyseiseen kuvaan **ContainerInventory** haulla. Esimerkiksi hakeminen`Type=ContainerInventory ubuntu Failed`  
    ![Etsi Ubuntu säilöt](./media/log-analytics-containers/search-ubuntu.png)

  Säilön **nimen**vieressä nimi muistiin ja Hae näitä lokit. Tässä esimerkissä se on `Type=ContainerLog adoring_meitner`.

**Suorituskyvyn tietojen tarkasteleminen**

Kun olet aloittamassa luoda kyselyjä, se voi auttaa mikä on mahdollista ensin. Esimerkiksi kaikki suorituskykytietoja näkyviin yritä kirjoittamalla seuraava hakukyselyn laajan kyselyn.

```
Type=Perf
```

![säilöjen suorituskyky](./media/log-analytics-containers/containers-perf01.png)



Näet tämän Lisää graafisessa muodossa kun napsautat word **arvot** tuloksissa.

![säilöjen suorituskyky](./media/log-analytics-containers/containers-perf02.png)



Voit rajoittaa näet tietyn säilöön kirjoittamalla sen nimen oikealla puolella kyselyn suorituskykytietoja.

```
Type=Perf <containerName>
```

Suorituskyvyn mittarit, kerätään yksittäisiä säilön luettelo, joka näyttää.

![säilöjen suorituskyky](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Esimerkki log hakukyselyt

Kannattaa usein käyttää kyselyiden Esimerkki tai kahden kuluessa alkaen ja muokkaamalla ne sopivan ympäristön luomiseen. Lähtökohtana voit kokeilla voit täydentää monimutkaisemman kyselyjen **Huomattavat kyselyt** -sivu.

![Säilöjen kyselyt](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Lokitietojen tallentaminen hakukyselyt

Tallentaminen kyselyt on lokin Analytics Normaali ominaisuus. Tallentamalla ne ne, jotka olet löytänyt hyötyä on kätevä myöhempää käyttöä varten.

Kun olet luonut kyselyn, jossa voit hyödyllisiä, tallentaa sen valitsemalla **Suosikit** Log Etsi-sivun yläreunassa. Sitten voit helposti käyttää sen myöhemmin **Oma Dashboard** -sivulla.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Etsi lokit](log-analytics-log-searches.md) tarkastella yksityiskohtaisia säilö tietueita.
