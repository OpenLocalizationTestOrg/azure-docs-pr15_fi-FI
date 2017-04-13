<properties
   pageTitle="Tavallisia syitä pilvipalvelussa roolit kierrättäminen | Microsoft Azure"
   description="Cloud-palvelun rooli määrää yhtäkkiä kierrättää voi aiheuttaa merkittäviä käyttökatkot. Seuraavassa on joitakin yleisiä ongelmia, jotka aiheuttavat roolit on kuluessa, joiden avulla voit pienentää käyttökatkot."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>Yleisiä ongelmia, jotka aiheuttavat roolien hallinta

Tässä artikkelissa käsitellään joitakin käyttöönoton ongelmien yleisiä aiheuttajia ja vianmääritysvihjeitä, joiden avulla voit korjata tämän ongelman. Maininta ongelma on sovellus on roolin esiintymän ei käynnisty tai se käy valmisteltaessa, varattu ja pysäyttäminen puheluikkunassa.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Puuttuvat runtime riippuvuudet

Jos sovelluksen rooli on riippuvainen jokin kokoonpano, joka ei kuulu .NET Framework tai Azure hallitut kirjastoon, kokoonpano on erikseen sisällyttäminen sovelluspaketin. Ota huomioon, että muiden Microsoft-kehysten eivät ole käytettävissä Azure oletusarvoisesti. Jos tehtäväsi on riippuvainen luoda kehys, sinun on lisättävä kyseiset kokoonpanot sovelluspaketin.

Ennen kuin luominen ja pakata sovelluksesi, tarkista seuraavat asiat:

- Jos Visual Studiossa, varmista, että kunkin Viitatun kokoonpanon projektin, joka ei kuulu Azure SDK-paketissa tai .NET Framework **Paikallinen kopio** -ominaisuudeksi on määritetty **Tosi** .

- Varmista, että seuraavan koodin korostetut viittaa kaikki käyttämättömät kokoonpanot kääntämisen-osaan.

- Jokaisen .cshtml-tiedoston **Luominen-toiminto** on määritetty **sisällön**. Tällä varmistetaan, että tiedostot näkyvät oikein paketin ja antaa muiden viitatun tiedostot näkyvät paketin.

## <a name="assembly-targets-wrong-platform"></a>Virhe kokoonpanon kohteet-ympäristö

Azure on 64-bittisessä ympäristössä. Tämän vuoksi käännetty 32-bittinen kohteen .NET-kokoonpanoja ei toimi Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Rooli ilmoittaa käsittelemättömän poikkeukset alustaminen tai pysäyttäminen

Määrittää poikkeuksia, [RoleEntryPoint] -luokka, joka sisältää [OnStart], [OnStop]ja [Suorita] menetelmistä, menetelmillä ilmenee on käsittelemättömän poikkeus. Jos käsittelemättömän poikkeuksen vuoksi jollakin seuraavista tavoista, siirrä roskakoriin rooli. Jos tehtävä on kierrättäminen toistuvasti, se saattaa palautetaan käsittelemättömän poikkeusta aina, kun se yrittää käynnistää.

## <a name="role-returns-from-run-method"></a>Suorita menetelmä palauttaa rooli

Suorita toistaiseksi eivät [Suorita] -menetelmää. Jos koodi ohittaa [Suorita] -menetelmää, se kannattaa lepotilaan jatkuvasti. Jos [Suorita] -menetelmä palauttaa rooli kierrättää.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Virheellinen DiagnosticsConnectionString-asetus

Jos sovellus käyttää Azure diagnostiikka-palvelun kokoonpanotiedosto on määritettävä `DiagnosticsConnectionString` hakumäärityksen asetus. Tämä asetus on määritettävä HTTPS-yhteyden tiliisi tallennustilan Azure-tietokannassa.

Jos haluat varmistaa, että oman `DiagnosticsConnectionString` asetus on oikein, ennen kuin otat yhteyttä sovelluspaketin Azure, tarkista seuraavat:  

- `DiagnosticsConnectionString` Pisteiden asettaminen Azure kelvollinen tallennustilan tiliin.  
  Oletusarvoisesti tämä asetus asioista emuloitu tallennustilan-tilille, jotta tämä asetus on muutettava erikseen, ennen kuin otat yhteyttä sovelluspaketin. Jos et muuta tämän asetuksen, roolin esiintymän yrittää käynnistää diagnostiikan näytön ilmenee poikkeuksen. Tämä saattaa aiheuttaa Roskakorin toistaiseksi roolin esiintymä.

- Yhteysmerkkijonon on määritetty [muoto](../storage/storage-configure-connection-string.md). (Protokolla on määritettävä as HTTPS.) Korvaa *MyAccountName* nimi tallennustilan tilin ja *MyAccountKey* access-näppäimen kanssa:    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Jos kehität sovelluksen käyttämällä Microsoft Visual Studio Azure työkaluja, voit määrittää arvon [ominaisuusikkunat](https://msdn.microsoft.com/library/ee405486) .

## <a name="exported-certificate-does-not-include-private-key"></a>Sertifikaatin ei ole yksityinen avain

Suorita web-roolin SSL-kohdassa, sinun on varmistettava viedyn hallinta-varmenteen sisältää yksityinen avain. Jos käytät *Windowsin sertifikaattien hallinta* varmenteen vieminen, muista valita **Kyllä** **Vie yksityinen avain** -vaihtoehto. Varmenne on vietävä PFX-muodossa, joka on tällä hetkellä tueta vain muoto.

## <a name="next-steps"></a>Seuraavat vaiheet

Näytä lisää [artikkeleita vianmääritys](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pilvipalveluihin.

Näytä Lisää rooli kierrättäminen skenaariot [Kevin Williamson blogin sarja](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)-palvelussa.

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[OnStart]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Suorita]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx
