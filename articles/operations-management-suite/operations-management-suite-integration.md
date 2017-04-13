<properties
   pageTitle="Ja toimintojen hallinta Suite (OMS) | Microsoft Azure"
   description="Lisäksi OMS perusominaisuudet, se voidaan yhdistää hallinta-sovelluksia ja antamaan management yhdistelmäympäristön, antamaan mukautetun hallinta skenaariot yksilöllinen ympäristöön tai mukautetun management services koe asiakkaillesi.  Tässä artikkelissa on yleiskatsaus eri asetusten ja OMS ja linkkejä artikkeleihin antamisen teknisiä tietoja."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Ja toimintojen hallinta Suite (OMS)

Toimintojen hallinta Suite on Microsoftin pilvipohjainen IT hallintaratkaisu, jonka avulla voit hallita ja suojata paikallisen oman ja cloud infrastruktuuria.  Lisäksi OMS perusominaisuudet, se voidaan yhdistää hallinta-sovelluksia ja antamaan management yhdistelmäympäristön, antamaan mukautetun hallinta skenaariot yksilöllinen ympäristöön tai mukautetun management services koe asiakkaillesi.  Tässä artikkelissa on yleiskatsaus eri asetusten ja OMS palvelut ja linkkejä artikkeleihin antamisen teknisiä tietoja. 



## <a name="log-analytics"></a>Lokitiedoston Analytics
Hallinnan Log Analytics keräämät tiedot tallennetaan säilöön, joka isännöi Azure-tietokannassa.  Säilön rajoitukseen sisältyy log hakuja, jotka tarjoavat pika-analyysi yli hyvin suuria tietomääriä.  Integrointi tarpeen voi olla täytä tietovarasto uusilla tiedoilla saataville analyysin tai poimimaan säilöön, anna uusi visualisointi tai integroiminen toiseen hallintatyökalun.

Säilön jokainen tieto on tallennettu tietueeksi.  Kun olet täyttänyt säilö, anna käyttäjien tietuetyyppi, joka käyttää ratkaisu ja kuvauksen ominaisuuksia.  Kun haet tietoja, sinun on nämä tiedot, joiden kanssa työskentelet tiedoista.

![Täyttää OMS-säilöön](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>Täytä Log Analytics-säilöön
On useita tapoja täyttää OMS säilö.  Menetelmä, jota voit käyttää riippuu tekijöistä, kuten lähdetietojen sisältävässä, tiedot ja jossa tarvitset tukea asiakkaille.  Kun tiedot on tallennettu säilö, se ei ole merkitystä, miten se allekirjoitukseen.

Seuraavissa osioissa on kuvattu eri asetukset täyttää OMS säilö.

#### <a name="connected-sources-and-data-sources"></a>Yhdistettyjen lähteiden ja tietolähteiden 
Jossa tietoja voi hakea OMS säilön yhdistettyjen lähteiden sijainteja.  Tietolähteiden ja ratkaisut yhdistetty lähteitä Suorita ja määritä tiedot, jotka kerätään.  Jos sovelluksen kirjoittaa tietoja jotakin näistä tietolähteistä, valitse voit kerätä se määrittämällä tietolähteen.  Esimerkiksi jos sovelluksesi Luo Syslog tapahtumat, valitse ne voidaan kerätä Syslog tietolähteen Linux-agentti.

- [Lokitiedoston Analytics tietolähteet](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Ratkaisut

Ratkaisujen laajentavat OMS ominaisuuksia.  Ratkaisun voi kerätä tietoja yhdistetyn lähteestä tai se voi suorittaa analyysi jo kerääminen säilössä tietueita.  Kunkin Microsoftin ratkaisun on yksittäisiä artikkelissa, joka sisältää tiedot, jotka se kerää tietoja.

- [Lokitiedoston Analytics ratkaisut](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>HTTP-tietojen kerääminen Ohjelmointirajapinta

Lokitiedoston Analytics HTTP tietojen kerääminen Ohjelmointirajapinnan on REST-Ohjelmointirajapinta, jonka avulla voit lisätä JSON tietoja Log Analytics-säilöön.  Kun sovellus, joka ei voi luoda tietolähteitä tai ratkaisuja tietojen avulla voidaan hyödyntää tämän API.  Se voidaan täytä asiakaskoneelta, joka voi soittaa Ohjelmointirajapinnan ja ei riippuvaisia sivustokokoelman aikatauluun tietolähteen tai ratkaisu säilö.

- [Kirjaudu Analytics HTTP tietojen kerääminen Ohjelmointirajapinta](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Tietojen noutaminen Log Analytics-säilöön

On useita tapoja tietojen noutaminen OMS säilöstä.  Voit haluat käyttäjien OMS-konsolin käyttämisestä tietojen hakemiseen ja antaa niille erilaisia visualisointeja ja analysointia varten.  Voit hakea tiedot myös ulkoinen prosessi, kuten toisen management-ratkaisun.

#### <a name="log-searches"></a>Lokitiedoston haut

Rajoitukseen OMS säilössä on saatavana log haut.  Käyttäjät voivat suorittaa oman analysoimista OMS-konsolissa tai luominen Raporttinäkymät-ikkunan visualisoinnin tietty loki haun.  Ratkaisut voivat sisältää mukautettuja näkymiä visualisointien ennalta määritettyjä haut perusteella.  Voit access-tietojen OMS säilössä Log haun Ohjelmointirajapinta ulkoisen sovelluksen tai hallinta-työkalua.  

- [Lokitiedoston Analytics log rajaamalla](../log-analytics/log-analytics-log-searches.md)
- [Lokitiedoston Analytics Kirjaudu haun REST-Ohjelmointirajapinnalla](../log-analytics/log-analytics-log-search-api.md)
- [Kirjaudu Analytics cmdlet-komennot](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Mukautetut näkymät 
Näkymän suunnittelun avulla voit luoda mukautettuja näkymiä, jotka antavat käyttäjille-ratkaisun tietojen analysointi ja visualisointi OMS konsolissa.  Kussakin näkymässä on ruutu, joka näytetään pääsivulta konsolin ja visualisoinnin osat, jotka perustuvat log haut, jotka voit määrittää haluamasi määrän.
  
- [Lokitiedoston Analytics näkymän suunnittelu](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>Power BI

Lokitiedoston Analytics automaattisesti tiedot voidaan viedä OMS säilö Power BI niin avulla voidaan hyödyntää sen visualisoinnit ja Analyysityökalut.  Se suorittaa viedä aikataulun niin tiedot on käytettävissä ajan tasalla. 

- [Power BI Log Analytics-tietojen vieminen](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Automaatio

OMS voit automatisoida prosesseja vastata kerättyjen tietojen tai muiden hallintatoiminnot suorittavan.  Se voi kerätä tietoja sovelluksesta ja lisätä sen OMS säilö tai voi automatisoida tunnettuja ongelmia säilö tiedot saatuaan korjaus. 

![Automaatio](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks

Azure automaatio Runbooks Suorita PowerShell-komentosarjojen ja työnkulut Azure pilveen.  Niiden avulla voidaan hallita Azure resursseja tai muita resursseja, joita voidaan käyttää pilvestä.  Runbooks voidaan suorittaa paikallisen palvelinkeskukseen, käyttämällä Hybrid Runbookin työntekijä.  Voit aloittaa runbookin Azure-portaalista tai ulkoisten prosessien useilla tavoilla, kuten PowerShell tai automaatio-Ohjelmointirajapinnan.

- [Tietojen runbookin Azure automaatio](../automation/automation-starting-a-runbook.md)
- [Azure automaatio cmdlet-komennot](https://msdn.microsoft.com/library/dn690262.aspx)
- [Automaatio REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/mt662285.aspx)
- [Automaatio .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Ilmoitukset

Ilmoitusten säännöt suoritetaan automaattisesti log haut aikataulun mukaan.  Jos tulokset tiettyyn ehtoja vastaavat tuloksena ilmoituksen voit aloittaa runbookin Azure automaatio- tai Soita webhook, jotka voit käynnistää ulkoinen prosessi.  Toinen näitä vastauksia voi sisältää tiedot, mukaan lukien log haku palauttaa ilmoituksen.

- [Lokitiedoston Analytics ilmoitukset](../log-analytics/log-analytics-alerts.md)
- [Lokitiedoston Analytics ilmoitusten Ohjelmointirajapinta](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Varmuuskopiointi ja palauttaminen

Yrityksen tietojen suojaaminen ja palvelimia ja sovellusten käytettävyyden varmistaminen palvelujen tarjoaminen Azure varmuuskopiointi ja palauttaminen.  Voit hyödyntää suorittamiseen tällaiset skenaariot varmuuskopion palveluja sovelluksen tai aloitetaan automaattisesti virtual koneen palveluista.

- [Azure varmuuskopiointi cmdlet-komennot](https://msdn.microsoft.com/library/mt619253.aspx)
- [Azure palauttaminen REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Azure sivuston palauttaminen cmdlet-komennot](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Mukautetut ratkaisut

Voit kapseloida integrointi logiikan mukautetun ratkaisuiksi suorittaa työtilassa tai asiakkaan työtilassa.  Ratkaisu voi olla integrointi menetelmistä tämän artikkelin lisäksi muita resursseja antamaan skenaario valmis.  Resurssit ratkaisussa on pakattu siten, että kun ratkaisu poistetaan, kaikki resurssit, se luotu poistetaan OMS työtilasta ja Azure tilauksen.

Esimerkiksi ratkaisu saattaa sisältää automaatio-runbookin kerätä ja käsitellä tietoja ja täytä sitten Log Analytics-tietovarasto HTTP tietojen kerääminen Ohjelmointirajapinnan käyttäminen.  Voit myös lisätä mukautetun näkymän, joka esittää ja analysoi kerättyjen tietojen.  

- Mukautettujen ratkaisujen (tulossa) luominen    

## <a name="next-steps"></a>Seuraavat vaiheet
- Viittaus [OMS SDK](operations-management-suite-sdk.md) teknisiä tietoja automatisointi OMS palvelut.  
