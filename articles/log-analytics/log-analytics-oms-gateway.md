<properties
    pageTitle="Tietokoneiden ja laitteiden yhdistäminen käyttämällä OMS yhdyskäytävän OMS | Microsoft Azure"
    description="Yhdistä OMS hallitun laitteet ja Operations Manager seurattava tietokoneiden OMS yhdyskäytävän tietojen lähettäminen OMS-palvelun, vaikka heillä ei ole Internet-yhteyttä."
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
    ms.date="10/26/2016"
    ms.author="banders"/>

# <a name="connect-computers-and-devices-to-oms-using-the-oms-gateway"></a>Tietokoneiden ja laitteiden yhdistäminen OMS OMS yhdyskäytävän käyttäminen

Tässä asiakirjassa kerrotaan, miten OMS hallitun laitteet ja System Center Operations Manager SCOM seurattava tietokoneet voi lähettää tietoja OMS-palvelun vaikka heillä ei ole Internet-yhteyttä. OMS yhdyskäytävän voit kerätä tietoja ja lähetä se OMS-palvelun heidän puolestaan.

Yhdyskäytävä on HTTP eteenpäin välityspalvelin, joka tukee HTTP tunneling HTTP-yhteyden-komennon avulla. Yhdyskäytävän voit käsitellä jopa 2 000 OMS samanaikaisesti yhdistetty laitteiden suoritettaessa 4 core-suoritin Windows 16 Gigatavun-palvelimeen.

Luodaan esimerkiksi yrityksen tai suurissa organisaatioissa voi olla verkkoyhteyden palvelinten mutta ei ehkä ole Internet-yhteys. Toinen esimerkki voi olla useita kohdan kanssa keino seuranta suoraan myynti (POS) laitteet. Ja valitse toinen esimerkki Operations Manager voi käyttää OMS yhdyskäytävän välityspalvelinta. Näissä esimerkeissä OMS yhdyskäytävän siirtää tietoja, jotka on asennettu nämä tai kassapäätelaitteiden, OMS tekijöiden.

Sen sijaan, että kunkin yksittäisiä agentti lähettämisen tiedot suoraan OMS ja lisää suora Internet-yhteys kaikki agentti tiedot lähetetään sijaan yhteen tietokoneeseen, jossa on Internet-yhteyden kautta. Tietokone on kohtaa, johon voit asentaa ja käyttää yhdyskäytävän. Tässä skenaariossa voit asentaa agenttien vuoksi kohtaa, johon haluat kerätä tietoja tietokoneita. Yhdyskäytävän sitten siirtää tiedot niistä OMS suoraan — yhdyskäytävä ei analysoida tietoja, jotka siirretään.

OMS yhdyskäytävän ja analysoida suorituskyvyn tai tapahtumatietoja palvelin, johon se on asennettu, johon yhdyskäytävä on myös asennettu tietokoneeseen on asennettava OMS-agentti.

Yhdyskäytävä on oltava yhteydessä Internetiin tietojen lataaminen OMS. Kunkin agentti myös on oltava verkkoyhteyden sen yhdyskäytävään, jotta agenttien vuoksi siirtää automaattisesti ja sieltä pois yhdyskäytävän tiedot. Saat parhaan tuloksen Älä asenna yhdyskäytävän tietokoneeseen, jossa on myös toimialueen ohjauskoneen.

Oheinen kaavio kuvaa, Tietovuo levylle OMS suoraan agenttien vuoksi.

![Suora agent-kaavio](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Oheinen kaavio kuvaa, joka näyttää tiedonkulun Operations Manager OMS.

![Operations Manager kaavio](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="install-the-oms-gateway"></a>Asenna OMS yhdyskäytävän

Tämä Gateway asennetaan korvaa aiemmat versiot yhdyskäytävän, että tietokoneeseen on asennettu (Log Analytics Forwarder).

Edellytykset: .net Framework 4.5, Windows Server 2012 R2 SP1 ja yllä

1. Lataa OMS yhdyskäytävän uusimman version [Microsoft Download Centeristä](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi).
2. Aloita asennus kaksoisnapsauttamalla **OMS Gateway.msi**.
3. Valitse Tervetuloa-sivulla **Seuraava**.  
    ![Yhdyskäytävän ohjattuun asennukseen](./media/log-analytics-oms-gateway/gateway-wizard01.png)
4. Käyttöoikeussopimus-sivulla Valitse **hyväksyn käyttöoikeussopimuksen ehdot** Hyväksy käyttöoikeussopimus ja sitten **Seuraava**.
5. Portti- ja välityspalvelimen osoite-sivulla:
    1. Kirjoita TCP-porttinumero, jota käytetään yhdyskäytävän. Asennuksen Avaa tämä porttinumero Windowsin palomuuri. Oletusarvo on 8080.
    Porttinumero kelvollisen päivämääräalueen on 1 – 65535. Jos syöte ei kuulu tämän alueelle, näyttöön tulee virhesanoma.
    2. Voit myös silloin, kun palvelin, johon on asennettu yhdyskäytävä tarvitsee käyttämään välityspalvelinta, kirjoita välityspalvelimen osoite, johon yhdyskäytävä on yhteyden. Esimerkiksi `http://myorgname.corp.contoso.com:80` Jos kenttä on tyhjä, yhdyskäytävän yrittää muodostaa yhteys Internetiin suoraan. Muussa tapauksessa yhdyskäytävän muodostaa yhteyden välityspalvelimen. Jos välityspalvelimen-palvelin edellyttää käyttöoikeuden tarkistusta, kirjoita käyttäjänimesi ja salasanasi.
        ![Yhdyskäytävän ohjattu välityspalvelimen määritykset](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
    3. Valitse **Seuraava**
6. Jos sinulla ei ole otettu käyttöön Microsoft-Updates, Microsoft Update-sivu tulee näkyviin, josta voit valita käyttöön Microsoft Updates. Tee valinta ja valitse sitten **Seuraava**. Muussa tapauksessa siirry seuraavaan vaiheeseen.
7. Valitse kohdekansio-sivulla jättää oletusarvon kansion **%ProgramFiles%\OMS yhdyskäytävän** tai kirjoita sijainti, johon haluat asentaa yhdyskäytävän ja valitse sitten **Seuraava**.
8. Siirry Ready to install-sivulle Valitse **Asenna**. Käyttäjätilien valvonta saattaa näkyä pyytävä oikeus asentaa. Jos näin on, valitse **Kyllä**.
9. Kun asennus on valmis, valitse **Valmis**. Voit varmistaa, että palvelu on käynnissä services.msc avata ja tarkista, että palveluluetteloon **OMS yhdyskäytävän** .  
    ![Services – OMS yhdyskäytävän](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Asentaa agentti laitteisiin

Tarvittaessa nähdä tietoja suoraan asentaminen [yhteyden Windows-tietokoneiden lokin Analytics](log-analytics-windows-agents.md) yhteydessä agenttien vuoksi. Tässä artikkelissa kuvataan, miten voit asentaa agentti ohjatun asennuksen avulla tai käyttämällä komentorivillä.

## <a name="configure-oms-agents"></a>OMS käyttäjäagenttien määrittäminen

Katso lisätietoja määrittäminen käyttämään välityspalvelinta, joka on tässä tapauksessa agentti [Määritä välityspalvelin ja palomuurin asetukset Microsoft Agent seuranta](log-analytics-proxy-firewall.md) yhdyskäytävän.

Operations Manager tekijöiden lähettää joitakin tietoja, kuten Operations Manager ilmoituksia, kokoonpanon arviointi, esiintymän tilaa ja kapasiteetin tietojen hallinta-palvelimen kautta. Muut paljon tietoja, kuten IIS-lokit, suorituskyky ja suojaus on lähetetty suoraan OMS-yhdyskäytävä. Katso [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md) täydellinen luettelo tiedoista, jotka lähetetään kanavien kautta.

>[AZURE.NOTE]
Jos aiot käyttää yhdyskäytävän verkon kuormituksen tasaamisen-kohdassa [Voit myös määrittää verkon kuormituksen](#optionally-configure-network-load-balancing).

## <a name="configure-a-scom-proxy-server"></a>Määritä SCOM välityspalvelin

Voit määrittää Operations Manager Lisää yhdyskäytävän edustajana välityspalvelinta. Kun päivität välityspalvelimen määritykset, välityspalvelimen määritykset otetaan käyttöön automaattisesti kaikki agenttien vuoksi raportoinnin Operations Manager.

Jos haluat käyttää yhdyskäytävän tukea Operations Manager, on:

- Microsoft Agent seuranta (agentti versio – **8.0.10900.0** ja sitä uudemmissa versioissa) yhdyskäytävän palvelimessa asentanut ja määrittänyt OMS työtilojen, jonka kanssa haluat viestiä.
- Yhdyskäytävä on oltava Internet-yhteys tai yhdistettävä välityspalvelinta, joka toimii.

### <a name="to-configure-scom-for-the-gateway"></a>Yhdyskäytävän SCOM määrittäminen

1. Avaa Operations Manager konsoli ja **Toimintojen hallinta Suite**, valitse Valitse **yhteys** ja valitse sitten **Välityspalvelimen määrittäminen**:  
    ![Operations Manager – Määritä välityspalvelin](./media/log-analytics-oms-gateway/scom01.png)
2. Valitse **Käytä välityspalvelinta, voit käyttää toimintojen hallinta-ohjelmistopaketti** ja kirjoita sitten OMS yhdyskäytävän palvelimen IP-osoite. Varmista, että voit aloittaa kanssa `http://` etuliite:  
    ![Operations Manager – välityspalvelimen osoite](./media/log-analytics-oms-gateway/scom02.png)
3. Valitse **Valmis**. Operations Manager-palvelin on yhdistetty OMS työtilassa.

## <a name="configure-network-load-balancing"></a>Verkon kuormituksen tasaamisen määrittäminen

Voit määrittää yhdyskäytävän hyvän käytettävyyden käyttämällä verkon kuormituksen luomalla klusterin. Klusterin hallitsee liikenne edustajasi ohjaamalla tekijöiden Microsoft seuranta pyydetty yhteydet sen solmujen välillä. Jos yhden yhdyskäytäväpalvelin lakkaa-liikenne uudelleenohjataan solmujen.

1. Avaa verkon kuormituksen tasaamisen hallinta ja luoda klusterin.
2. Napsauta klusterin ennen kuin lisäät yhdyskäytävien hiiren kakkospainikkeella ja valitse **klusterin ominaisuuksien.** Määritä klusteri on IP-osoitetta:  
    ![Verkon kuormituksen tasaamisen hallinta – klusterin IP-osoitteet](./media/log-analytics-oms-gateway/nlb01.png)
3. Yhdistämään OMS yhdyskäytävän palvelimen asennettuna Microsoft seuranta Agent-klusterin IP-osoitetta hiiren kakkospainikkeella ja valitse sitten **Lisää Host klusteriin**.  
    ![Verkon ladata Vastatilin hallinta – Lisää isäntä klusteriin](./media/log-analytics-oms-gateway/nlb02.png)
4. Kirjoita IP-osoite, jonka haluat yhdistää yhdyskäytävä-palvelimeen:  
    ![Verkon kuormituksen tasaamisen Manager – Lisää isäntä klusterin: yhdistäminen](./media/log-analytics-oms-gateway/nlb03.png)
5. Tietokoneissa, joissa ei ole Internet-yhteyttä muista klusterin IP-osoitteen avulla **Microsoft Agent-ominaisuudet valvonta**:  
    ![Microsoft agenttiominaisuudet – välityspalvelimen seuranta](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Automaatio hybrid työntekijöiden määrittäminen

Jos automaatio hybrid työntekijöiden ympäristössäsi, seuraavat vaiheet kirjoittaa manuaalinen, väliaikaiset menetelmää voit määrittää yhdyskäytävän tukea ne.

Seuraavissa vaiheissa on tiedettävä Azure alueen automaatio-tilin sijainti. Etsi sijainti:

1. Kirjautuminen [Azure portal](https://portal.azure.com/).
2. Valitse Azure automaatio-palvelu.
3. Valitse haluamasi Azure automaatio-tili.
4. Näytä sen alueen, valitse **sijainti**.  
    ![Azure portal – automaatio tilin sijainti](./media/log-analytics-oms-gateway/location.png)

Seuraavissa taulukoissa avulla voit tunnistaa kunkin sijainnin URL-osoite:

**Työn runtime tietojen palvelun URL-osoitteet**

| **sijainti** | **URL-OSOITE** |
| --- | --- |
| Pohjois-keskitetyn USA | ncus jobruntimedata-tuot-su1.azure-automation.net |
| Länsi Europe | Microsoft-jobruntimedata-tuot-su1.azure-automation.net |
| Etelä keskitetyn USA | scus jobruntimedata-tuot-su1.azure-automation.net |
| Yhdysvaltojen Itä | eus jobruntimedata-tuot-su1.azure-automation.net |
| Keskitetyn Kanada | kopio jobruntimedata-tuot-su1.azure-automation.net |
| Pohjois-Eurooppa | Uusi jobruntimedata-tuot-su1.azure-automation.net |
| Etelä Itä-Aasian | meren jobruntimedata-tuot-su1.azure-automation.net |
| Keskitetyn Intia | CID jobruntimedata-tuot-su1.azure-automation.net |
| Japani | jpe jobruntimedata-tuot-su1.azure-automation.net |
| Australia | ietokannan jobruntimedata-tuot-su1.azure-automation.net |

**Agentti palvelun URL-osoitteet**

| **sijainti** | **URL-OSOITE** |
| --- | --- |
| Pohjois-keskitetyn USA | ncus agentservice-tuot-1.azure-automation.net |
| Länsi Europe | Microsoft-agentservice-tuot-1.azure-automation.net |
| Etelä keskitetyn USA | scus agentservice-tuot-1.azure-automation.net |
| Yhdysvaltojen Itä | eus2 agentservice-tuot-1.azure-automation.net |
| Keskitetyn Kanada | kopio agentservice-tuot-1.azure-automation.net |
| Pohjois-Eurooppa | Uusi agentservice-tuot-1.azure-automation.net |
| Etelä Itä-Aasian | meren agentservice-tuot-1.azure-automation.net |
| Keskitetyn Intia | CID agentservice-tuot-1.azure-automation.net |
| Japani | jpe agentservice-tuot-1.azure-automation.net |
| Australia | ietokannan agentservice-tuot-1.azure-automation.net |

Jos tietokoneeseen on rekisteröity hybrid työntekijä automaattisesti varten korjataan Update Management-ratkaisun avulla, käytä seuraavia ohjeita:

1. Lisää työn Runtime tietojen palvelun URL-osoitteet OMS yhdyskäytävän sallittu Host (isäntä)-luettelosta. Esimerkki: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Käynnistä OMS Gateway-palveluun seuraavan PowerShell cmdlet-komennon avulla:`Restart-Service OMSGatewayService`

Jos tietokoneeseen on käyttöön-nousta ja Azure automaatio hybrid työntekijä rekisteröinti cmdlet-komennolla, kirjoita seuraavasti:

1. Lisää agentti rekisteröinti URL-osoite OMS yhdyskäytävän sallittu Host (isäntä)-luettelosta. Esimerkki:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Lisää työn suorituksenaikainen tietojen palvelun URL-osoitteet OMS yhdyskäytävän sallittu Host (isäntä)-luettelosta. Esimerkki: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Käynnistä OMS Gateway-palveluun.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Hyödyllisiä PowerShellin cmdlet-komennot

Cmdlet-komentojen avulla voit suorittaa tehtäviä, joita tarvitaan päivitys OMS yhdyskäytävän asetukset. Ennen kuin voit käyttää niitä, muista:

1. Asenna OMS yhdyskäytävän (MSI).
2. Avaa PowerShell-ikkuna.
3. Tuo moduuli, kirjoita seuraava komento:`Import-Module OMSGateway`
4. Jos ei virheitä edellisessä vaiheessa-moduulin on tuotu onnistuneesti ja Cmdlet-komentoja voi käyttää. Tyyppi`Get-Module OMSGateway`
5. Kun olet tehnyt muutokset käyttämällä Cmdlet-komentoja, varmista, että käynnistät Gateway-palveluun.

Jos saat virheilmoituksen vaiheessa 3-moduulia ei tuoda. Virhe voi ilmetä, kun PowerShell ei löydä moduuli. Löydät sen yhdyskäytävän asennuspolku: C:\Program Files\Microsoft OMS Gateway\PowerShell.

| **Cmdlet-komento** | **Parametrit** | **Kuvaus** | **Esimerkkejä** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` | Key (pakollinen) <br> Arvo | Muuttaa palvelun määrittäminen | `Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` | Avain | Saa palvelun määrittäminen | `Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` | Osoite <br> Käyttäjänimi <br> Salasana | Määrittää välitys (ylöspäin) välityspalvelimen osoitteen (ja tunnistetiedon) | 1. vastaa välityspalvelimen ja tunnistetieto määrittäminen:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. Määritä vastaa-välityspalvelimen todennus silloin:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. Poista vastaa-välityspalvelimen asetukset, eli ei tarvitse vastaa välityspalvelimen:`Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` |   | Saa välitys (ylöspäin) välityspalvelimen osoite | `Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` | Host (pakollinen) | Lisää isäntä sallittujen luetteloon | `Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHos`t | Host (pakollinen) | Poistaa isäntä sallittujen luetteloon | `Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` |   | Saa tällä hetkellä sallittu host (vain paikallisesti määritetyn sallitut host, älä sisällytä automaattisesti ladatut sallittu isännät) | `Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` | Aihe (pakollinen) | Lisää Asiakasvarmenne veloittaa sallittujen luetteloon | `Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` | Aihe (pakollinen) | Poistaa asiakkaan sertifikaatin aiheen sallittujen luetteloon | `Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificat`e |   | Saa tällä hetkellä sallittu asiakkaan varmenteen aiheet (vain paikallisesti määritetyn sallittu aiheista, älä sisällytä automaattisesti ladatut sallittu aiheet) | `Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Vianmääritys

On suositeltavaa, että asennat OMS-agentti tietokoneissa, joihin on asennettu yhdyskäytävä. Voit käyttää agentti voi kerätä tapahtumat, jotka ovat kirjaamat yhdyskäytävän.

![Tapahtumienvalvonta – OMS yhdyskäytävän loki](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS yhdyskäytävän tapahtumatunnukset ja kuvaukset**

Seuraavassa taulukossa on tapahtumatunnukset ja kuvaukset OMS yhdyskäytävän tapahtumien poistaminen.

| **TUNNUS** | **Kuvaus** |
| --- | --- |
| 400 | Mikä tahansa sovelluksen virhe, joka ei ole tietyn tunnus |
| 401 | Virhe kokoonpanon. Esimerkki: listenPort = "teksti" sijaan kokonaisluku |
| 402 | Poikkeus jäsennyksen TLS handshake viestit |
| 403 | Verkko-virhe. Esimerkki: ei voi yhdistää kohde palvelimeen |
| 100 | Yleisiä tietoja |
| 101 | Palvelun käynnistäminen |
| 102 | Palvelu on lopetettu |
| 103 | HTTP-yhteyden komento vastaanotti asiakas |
| 104 | Not HTTP-yhteyden komento |
| 105 | Kohdepalvelin ei ole sallittujen luetteloon tai Kohdeportti ei ole suojattu portti (443) <br> <br> Varmista, että yhdyskäytävän palvelimeen MMA-agentti ja yhteydenpito yhdyskäytävän tekijöiden on yhdistetty samaan Log Analytics työtilaan.|
| 105 | Virhe TcpConnection – virheellinen Asiakasvarmenne: CN = yhdyskäytävän <br><br> Varmistaa, että: <br> <br> & #149; Käytät yhdyskäytävän versionumero 1.0.395.0 kanssa tai suurempi. <br> & #149; Yhteydenpito yhdyskäytävän MMA agentti yhdyskäytävän palvelimeen ja niistä on yhdistetty samaan Log Analytics työtilaan. |
| 106 | Mistä tahansa syystä TLS-istunnon on epäilyttävistä ja hylätyn |
| 107 | TLS-istunnon on vahvistettu |

**Voi kerätä suorituskykylaskureita**

Seuraavassa taulukossa käytettävissä suorituskyvyn laskureita OMS yhdyskäytävän. Voit lisätä laskureita suorituskyvyn valvonnan käyttäminen.

| **Nimi** | **Kuvaus** |
| --- | --- |
| OMS yhdyskäytävän/aktiivinen asiakasyhteys | Aktiivinen asiakas-verkkoyhteyksien (TCP) määrä |
| OMS yhdyskäytävän ja virheiden määrä | Virheiden määrä |
| OMS yhdyskäytävän/yhdistetty asiakas | Asiakkaiden määrä |
| OMS yhdyskäytävän/hylkääminen määrä | Hylättyjen vuoksi kaikki TLS kelpoisuustarkistusvirhe määrä |

![OMS yhdyskäytävän suorituskykylaskureita](./media/log-analytics-oms-gateway/counters.png)


## <a name="get-assistance"></a>Saat lisätietoja

Kun olet kirjautunut sisään Azure-portaaliin, voit luoda pyytää apua OMS yhdyskäytävän tai muissa Azure-palvelu tai palvelun ominaisuus.
Pyydä apua portaalin oikean yläkulman kysymysmerkki-merkki ja valitse sitten **Uusi tukevat pyynnön**. Suorita sitten uusi tuki tarkistuspyynnön lomake.

![Uusi tukipyyntö](./media/log-analytics-oms-gateway/support.png)

Voit myös jättää palautetta OMS tai lokin Analytics [Microsoft Azure palautetta keskustelupalstalle](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisää tietolähteitä](log-analytics-data-sources.md) , jos haluat kerätä tietoja OMS työtilassa yhdistetty lähteistä ja tallentaa sen OMS säilö.
