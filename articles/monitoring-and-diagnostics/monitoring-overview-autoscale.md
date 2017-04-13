<properties
    pageTitle="Yleistä Microsoft Azuren näennäiskoneiden pilvipalveluihin ja verkkosovelluksissa Automaattinen skaalaus | Microsoft Azure"
    description="Microsoft Azure Automaattinen skaalaus yleiskatsaus. Koskee näennäiskoneiden, pilvipalveluihin ja verkkosovelluksissa."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="robb"/>

# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Automaattinen skaalaus Microsoft Azuren näennäiskoneiden, pilvipalveluihin ja Web Apps-sovellusten yleiskatsaus

Tässä artikkelissa kuvataan, mitä Microsoft Azure Automaattinen skaalaus on, sen edut ja voit aloittaa sen käyttämisen.  

Azure näytön Automaattinen skaalaus koskee vain [Virtuaalikoneen asteikko joukot](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Pilvipalveluihin](https://azure.microsoft.com/services/cloud-services/)ja [Sovelluksen palvelu - verkkosovelluksissa](https://azure.microsoft.com/services/app-service/web/).

>[AZURE.NOTE] Azure on kaksi Automaattinen skaalaus-menetelmää. Automaattinen skaalaus vanhempi versio koskee näennäiskoneiden (käytettävyys sääntöjoukot). Tämä toiminto on rajoitettu tuki ja suosittelemme siirtämisestä AM asteikko joukot nopeammin ja luotettava Automaattinen skaalaus-tukea. Tässä artikkelissa sisältyy linkin käyttämisestä vanha tekniikka.  


## <a name="what-is-autoscale"></a>Automaattinen skaalaus-ominaisuudet

Automaattinen skaalaus voit on käynnissä, jotta sovelluksesi kuormituksen käsittelyssä resursseja oikea määrä. Voit lisätä resursseja käsitellä kuormituksen lisäykset ja Tallenna myös poistamalla resurssit, jotka ovat Istuva rahaa käyttämättömänä. Voit määrittää vähimmäis- ja useita kertoja, suorittaa ja Lisää tai poista VMs sääntöjoukon automaattisesti perusteella. Pysty vähintään tekee, varmista, että sovelluksesi on aina käynnissä jopa ei kuormitus. Ottaa enintään rajoittaa oman kokonaiskustannukset mahdollista kerran tunnissa. Voit automaattisesti skaalaamaan välillä nämä kaksi suojaavat, voit luoda sääntöjen avulla.

 ![Automaattinen skaalaus kuvaus. Voit lisätä ja poistaa VMs](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Kun säännön ehdot täyttyvät, vähintään yksi Automaattinen skaalaus-toiminto käynnistyy. Voit lisätä ja poistaa VMs tai tehdä muita toimenpiteitä. Seuraavat Käsitekaavio näyttää tätä prosessia.  

 ![Käsitteellinen Automaattinen skaalaus Tietovuokaavion Esimerkki](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)


## <a name="autoscale-process-explained"></a>Automaattinen skaalaus-prosessin kuvaus
Seuraavat selitys koskevat edeltävän kaavion osiin.   

### <a name="resource-metrics"></a>Resurssin arvot
Resurssien lähettää arvot, jotka säännöt käsitellään myöhemmin. Arvot ovat peräisin kautta kolmella eri tavalla.
AM asteikko joukot käyttää telemetriatietojen tietoja Azure diagnostiikka tekijöiden olisi telemetriatietojen Web Apps-sovellusten ja pilvipalveluihin tulee suoraan Azure-infrastruktuuria. Jotkin tavallisimmat tilastotiedot sisältävät suorittimen käyttö, muistinkäytön, viestiketjun määrät, jonon pituuden ja levytilan. Voit käyttää telemetriatietojen tietoja luettelo-kohdassa [Automaattinen skaalaus yleiset arvot](insights-autoscale-common-metrics.md).

### <a name="time"></a>Aika
Aikataulun sääntöjen perustuvat UTC-aika. Sinun on määritettävä aikavyöhykkeen oikein, kun sääntöjen määrittämisestä.  

### <a name="rules"></a>Säännöt
Kaaviossa näkyy vain yksi Automaattinen skaalaus sääntö, mutta sinulla on useita niistä. Voit luoda monimutkaisia päällekkäisiä sääntöjä tilanteeseesi tarvittaessa.  Sisällytä sääntötyypeistä  

 - **Arvo-pohjainen** - esimerkiksi suorittaa tämän toiminnon, kun suorittimen käyttö on yli 50 %.
 - **Aikapohjaisten** – esimerkiksi käynnistimen webhook jokaisen 8 am annetun aikavyöhykkeen lauantaihin.

Metrijärjestelmän sääntöjen mittaa sovelluksen lataaminen ja Lisää tai poista VMs kyseisen kuormituksen perusteella. Aikataulun sääntöjen avulla voit skaalata, kun aika kuviot kohdassa että latautuvat ja skaalattava ennen mahdollista kuormituksen Suurenna tai Pienennä.  


### <a name="actions-and-automation"></a>Toiminnot- ja automaatiofunktiot

Sääntöjen voit käynnistää yhden tai usean tyyppisiä toimia.

- **Skaalaa** - asteikon VMs sisään tai ulos
- **Sähköposti** - tilauksen Järjestelmänvalvojat, apuyhteyshenkilöiden ja/tai muita määritettyyn sähköpostiosoitteeseen sähköpostiviestin lähettäminen
- **Automatisoiminen kautta webhooks** - kutsu webhooks, jotka voivat aiheuttaa useita monimutkaisia toimintoja kuuluvan tai sen ulkopuolisen Azure. Sisällä Azure voit aloittaa Azure automaatio runbookin, Azure-funktiota tai Azure logiikan sovelluksen. Esimerkki 3 osapuolen URL-osoite ulkopuolella Azure sisällä palvelujen, kuten liukuma- ja Twilio.


## <a name="autoscale-settings"></a>Automaattinen skaalaus-asetukset
Seuraavat termit ja rakenteen avulla Automaattinen skaalaus.

- **Automaattinen skaalaus-asetus** on on lukea Automaattinen skaalaus-ohjelma sallitaanko skaalata ylös tai alas. Siinä on yksi tai useampi profiilit, tietoja kohde resurssi ja ilmoitusasetusten määrittäminen.
    - Aiemmin **Automaattinen skaalaus-profiili** on yhdistelmä kapasiteetin määrittäminen sääntöjoukon käynnistimien ja mittakaava toiminnot profiilin ja toistuva tapahtuma. Voit määrittää useita profiileja, jonka voit huolehtia erilaisia päällekkäisiä vaatimuksia.
        - **Kapasiteetin asetus** osoittaa, vähintään, enintään ja kerrat oletusarvot. [1 Viikunapuun käyttämään haluamaasi paikkaan]
        - **Säännön** sisältää käynnistimen – metrisillä käynnistimen tai aika-käynnistimen – ja mittakaava toiminto, joka ilmaisee, onko Automaattinen skaalaus skaalata, ylös tai alas kun sääntöön täyttyy.
        - **Toistuminen** osoittaa, kun Automaattinen skaalaus Sijoita voimaan profiilia. Käytössä voi olla eri Automaattinen skaalaus-profiileista eri silloin päivä tai viikonpäivien, esimerkiksi.
- **Ilmoitusten asetus** määrittää, mitä ilmoitukset suoritetaan, kun Automaattinen skaalaus-tapahtuman ilmetessä Automaattinen skaalaus-asetus-profiileista vaatimukset täyttävän perusteella. Automaattinen skaalaus voit ilmoittaa vähintään yksi sähköpostiosoitteet tai soittaa vähintään yksi webhooks.

![Azure Automaattinen skaalaus-asetus, profiilin ja säännön rakenne](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

Täydellinen luettelo määritettäviä kenttiä ja kuvauksia ei käytettävissä [Automaattinen skaalaus REST API](https://msdn.microsoft.com/library/dn931928.aspx).

Katso koodiesimerkkejä

* [Resurssienhallinta-mallien avulla AM asteikko joukkojen Automaattinen skaalaus-määritysten Lisäasetukset](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Automaattinen skaalaus REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/dn931953.aspx)



## <a name="horizontal-vs-vertical-scaling"></a>Vaaka-ja pystysuunnassa skaalaus

Automaattinen skaalaus kasvaa vain Skaalaa resurssien vaakasuunnassa, joka kasvaa (",") tai pienentää AM esiintymien määrän ("tomaatti").  Vaakasuuntainen skaalauksen, joka on joustavampaa cloud tilanteessa, kun sen avulla voit suorittaa mahdollisesti VMs käsittelemään kuormituksen tuhansia. Pystysuuntainen skaalaus on erilainen. Säilyttää VMs sama määrä, mutta tekee AM ("asetukset") enemmän tai vähemmän ("alaspäin") tehokkaita. Power mitataan muistin, suorittimen nopeus, levytilaa.  Pystysuuntainen skaalaus on useita rajoituksia. Se on riippuvainen suurempi laitteita, jotka voivat vaihdella alueittain ja käynnit nopeasti ja yläraja saatavuus. Skaalauksen pystysuunnassa tarvitaan myös yleensä AM Lopeta ja Käynnistä. Lisätietoja on artikkelissa [skaalata pystysuunnassa Azure virtual tietokoneeseen, joka sisältää Azure automaatio](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md).


## <a name="methods-of-access"></a>Access-menetelmiä
Voit määrittää Automaattinen skaalaus kautta

- [Azure portal](insights-how-to-scale.md)
- [PowerShellin](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Office kaikissa ympäristöissä rivin komentoliittymä (CLI)](insights-cli-samples.md#autoscale )
- [Azure näytön REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dn931953.aspx )

## <a name="supported-services-for-autoscale"></a>Tuetut palvelut Automaattinen skaalaus


| Palvelun                              | Rakenne ja asiakirjoja                                       |
|--------------------------------------|-----------------------------------------------------|
| Web Apps-sovelluksista                             | [Skaalaus Web Apps-sovelluksista](insights-how-to-scale.md)              |
| Pilvipalveluihin                       | [Automaattinen skaalaus pilvipalveluun](../cloud-services/cloud-services-how-to-scale.md) |
| Näennäiskoneiden: perinteinen           | [Perinteinen virtuaalikoneen käytettävyys joukot skaalaus](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Näennäiskoneiden: Windows asteikko joukot| [Skaalaus AM asteikko määrittää Windowsissa](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)  |
| Näennäiskoneiden: Linux asteikko määrittää  | [Skaalaus AM asteikko määrittää Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Näennäiskoneiden: Windows-Esimerkki   | [Resurssienhallinta-mallien avulla AM asteikko joukkojen Automaattinen skaalaus-määritysten Lisäasetukset](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Automaattinen skaalaus, käytä Automaattinen skaalaus-vaihe vaiheelta luettelossa aiemmin tai seuraavissa resursseissa:

- [Azure näytön Automaattinen skaalaus yleiset arvot](insights-autoscale-common-metrics.md)
- [Azure näytön Automaattinen skaalaus parhaat käytännöt](insights-autoscale-best-practices.md)
- [Automaattinen skaalaus-toimintojen avulla voit lähettää sähköpostiviestejä ja webhook ilmoitukset](insights-autoscale-to-webhook-email.md)
- [Automaattinen skaalaus REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/dn931953.aspx)
- [Vianmäärityksen virtuaalikoneen asteikko joukot Automaattinen skaalaus](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
