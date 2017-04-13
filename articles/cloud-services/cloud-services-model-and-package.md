<properties
    pageTitle="Mikä pilvipalvelussa mallin ja paketin | Microsoft Azure"
    description="Tässä artikkelissa kuvataan cloud palvelumallin (.csdef, .cscfg) ja Azure-paketti (.cspkg)"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Mikä on pilvipalvelussa mallin ja miten se pakata?
Pilvipalveluun luodaan kolme osaa, palvelun määritys _(.csdef)_, palvelun config _(.cscfg)_ja palvelun pakkauksen _(.cspkg)_. **ServiceDefinition.csdef** ja **ServiceConfig.cscfg** tiedostot on XML-pohjainen ja kuvaavat rakenteen käytössäsi ovat sekä siitä, miten se on määritetty. muunnokset mallin nimi. **ServicePackage.cspkg** on zip-tiedosto, joka sisältää kaikki tarvittavat ‑binaaripohjainen riippuvuudet, luo **ServiceDefinition.csdef** ja muun muassa. Azure Luo pilvipalveluun **ServicePackage.cspkg** ja **ServiceConfig.cscfg**.

Kun Azure-tietokannassa on käynnissä pilvipalvelussa, määritä se **ServiceConfig.cscfg** -tiedoston avulla, mutta ei voi muuttaa määritystä.

## <a name="what-would-you-like-to-know-more-about"></a>Mitä haluat tietää enemmän?

* Haluan tietää lisää [ServiceDefinition.csdef](#csdef) ja [ServiceConfig.cscfg](#cscfg) tiedostot.
* Tiedän jo tietoja, Anna minulle-voin määrittää [joitakin esimerkkejä](#next-steps) .
* Haluan luoda [ServicePackage.cspkg](#cspkg).
* Käytän Visual Studio ja haluan nyt...
    * [Luo uusi pilvipalvelussa][vs_create]
    * [Määritä aiemmin pilvipalvelussa][vs_reconfigure]
    * [Ota käyttöön pilvipalvelussa-projekti][vs_deploy]
    * [Etätyöpöytä cloud palvelun esiintymän tuominen][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
**ServiceDefinition.csdef** tiedosto määrittää asetuksia, jotka käyttävät Azure määrittäminen pilvipalveluun. [Azure määritelmä malli (.csdef tiedoston)](https://msdn.microsoft.com/library/azure/ee758711.aspx) on sallittu muoto palvelun määritystiedosto. Seuraavassa esimerkissä verkko- ja työntekijä roolien voidaan määrittää haluamasi asetukset:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Voit katsoa saat paremman käsityksen käyttää XML-rakenteen [määritelmä malli]-[], mutta seuraavassa on lyhyt kuvaus joitakin osia:

**Sivustot**  
Sisältää sivustot tai web-sovellukset, joita isännöidään IIS7 määritykset.

**InputEndpoints**  
Sisältää päätepisteet, joita käytetään yhteystiedot pilvipalvelussa.

**InternalEndpoints**  
Sisältää päätepisteet, joita käytetään mukaan rooli esiintymät keskenään.

**ConfigurationSettings**  
Sisältää asetus määritelmät tarkoitettuja tietyn roolin.

**Varmenteet**  
On sertifikaatteja, joita tarvitaan roolin määritykset. Edellinen koodi-esimerkissä varmenne, jota käytetään Azure yhteyden määrityksiä.

**LocalResources**  
On paikallinen tallennus resurssien määritykset. Paikallinen tallennus resurssi on varattu kansio, johon rooli esiintymää on käynnissä virtuaalikoneen tiedostojärjestelmässä.

**Tuonti**  
On tuotu moduulit määritykset. Koodin edellisessä esimerkissä näkyy moduulit Etätyöpöytäyhteys ja Azure Yhdistä.

**Käynnistys**  
Sisältää rooli käynnistyessä suoritettavat tehtävät. Tehtävät määritetään cmd. tai suoritettava tiedosto.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Cloud-palvelun asetusten määrittäminen määräytyy **ServiceConfiguration.cscfg** tiedoston arvojen mukaan. Voit määrittää minuuttimäärän, jonka esiintymät, jonka haluat ottaa käyttöön jokaisen roolin tässä tiedostossa. Asetukset, jotka on määritetty palvelun määritystiedostossa arvot lisätään palvelun määritystiedostoa. Minkä tahansa hallinta varmenteiden, jotka liittyvät pilvipalvelussa sijaitsevaan thumbprints lisätään myös tiedoston. [Azure määritysten malli (.cscfg tiedoston)](https://msdn.microsoft.com/library/azure/ee758710.aspx) on sallitun muoto palvelun määritystiedostoa.

Palvelun kokoonpanotiedosto ei ole pakattu sovelluksessa, mutta tuodaan Azure erillisenä tiedostona ja määritetään pilvipalvelussa. Voit ladata uusi palvelu kokoonpanotiedosto ilman mallirakenteeseen pilvipalvelussa sijaitsevaan. Määritysten arvot pilvipalvelussa voi muuttaa pilvipalvelussa on käynnissä. Seuraavassa esimerkissä esitetään, joka voidaan määrittää verkko- ja työntekijä roolien asetukset:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Voit katsoa paremmin tietoja käyttää XML-rakenteen [Määritys malli](https://msdn.microsoft.com/library/azure/ee758710.aspx) -, seuraavassa on lyhyt kuvaus osat:

**Esiintymiä**  
Määrittää roolin kopioiden määrä. Jos haluat estää pilvipalvelussa sijaitsevaan jatkossa mahdollisesti aikana päivityksiä ei ole käytettävissä, on suositellaan web osoittava roolit useamman kuin yhden esiintymän otetaan käyttöön. Tällöin on siitä [Azure Laske palvelussa palvelutasosopimusta (SLA)](http://azure.microsoft.com/support/legal/sla/), joka takaa 99.95 % ulkoinen yhteys Internetiin yhteydessä oleva roolien, kun vähintään kaksi roolin esiintymät on otettu käyttöön palvelun ohjeita.

**ConfigurationSettings**  
Määrittää roolin käynnissä olevat esiintymät asetuksia. Nimi `<Setting>` osat on vastattava palvelun määritystiedostossa asetus-määrityksiä.

**Varmenteet**  
Määrittää palvelun käytettävät varmenteet. Edellinen koodi-esimerkissä määrittäminen RemoteAccess moduulin varmennetta. *Allekirjoitus* -määritteen arvon on määritettävä käyttämään varmenteen allekirjoitus.

<p/>

 >[AZURE.NOTE] Varmenteen allekirjoitus voidaan lisätä määritystiedoston tekstieditorissa tai arvon voidaan lisätä Visual Studiossa roolin **Ominaisuudet** -sivun **Varmenteet** -välilehti.



## <a name="defining-ports-for-role-instances"></a>Portit roolin esiintymien määrittäminen
Azure avulla kaksi aloituskohtaa web-roolin. Tämä tarkoittaa, että kaikki liikenne tapahtuu – yksi IP-osoite. Voit määrittää, että sivustojen jakaminen portin määrittäminen voidaan ohjata pyynnön oikeaan paikkaan toimialuenimi. Voit myös määrittää sovellustesi kuunnella tunnetun portit IP-osoite.

Seuraavassa esimerkissä on esitetty kokoonpano web-roolin verkkosivusto- ja web-sovelluksen kanssa. Sivusto on määritetty tapahtuma oletussijainti porttiin 80 ja web-sovellukset on määritetty vastaanottamaan pyyntöjä vaihtoehtoinen tietojen toimialuenimi, jota kutsutaan "mail.mysite.cloudapp.net".

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Roolin määrittäminen muuttaminen
Voit päivittää oman pilvipalvelussa määritysten sen ollessa käynnissä Azure-tekemättä palvelun offline-tilassa. Voit muuttaa kokoonpanotietoja, joko Lataa uusi kokoonpanotiedosto tai Muokkaa kokoonpanotiedosto paikassa ja käyttää käynnissä-palveluun. Palvelun työnkulkuun voit tehdä seuraavat muutokset:

- **Arvojen Hakumääritysten asetusten muuttaminen**  
Kun määritys, asetuksiin tehtävät muutokset-roolin esiintymän voi valita ja lisätä muuta esiintymää on online-tilassa tai Roskakorin esiintymän tilanteen ja ota aikana esiintymän muutos on offline-tilassa.

- **Palvelun topologian roolin esiintymien muuttaminen**  
Topologian muutokset eivät vaikuta käynnissä olevat esiintymät, paitsi jos erillisen poistetaan. Kaikki jäljellä olevat esiintymät yleensä ei tarvitse olla kuluessa; Voit kuitenkin Roskakorin roolin esiintymät vastauksena Topologiamuutos.

- **Varmenteen allekirjoituksen muuttaminen**  
Varmenteen voi päivittää vain, kun rooli esiintymää on offline-tilassa. Jos sertifikaatin lisätään, poistaa tai muuttaa, kun rooli esiintymää on online-Azure tilanteen Vie esiintymän offline-tilassa varmenne ja noutaa sen online-tilaan, kun muutos on valmis.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Käsittely määritysmuutoksia ja palvelun suorituksenaikainen tapahtumat
[Azure suorituksenaikainen kirjasto](https://msdn.microsoft.com/library/azure/mt419365.aspx) sisältää [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) nimitilan, joka sisältää luokkia liittyvät rooli-esiintymän koodin Azure ympäristön käyttäminen. [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) -luokka määrittää seuraavia tapahtumia, jotka ovat korotettuna ennen ja jälkeen määritysten muuttaminen:

- **Tapahtuman [muuttaminen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx)**  
Tämä tapahtuu ennen määritysten muutokset tulevat voimaan antamalla voit toimia roolin esiintymät alaspäin, jos pakollinen rooli määritetyn esiintymään.
- **[Muutetut](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) tapahtuma**  
Tapahtuu, kun määritysten muutokset tulevat voimaan rooli määritetyn esiintymään.

> [AZURE.NOTE] Koska varmenteen muutokset aina offline-tilaan rooli esiintymät, ne eivät korottaa RoleEnvironment.Changing tai RoleEnvironment.Changed tapahtumat.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Ottaa käyttöön sovelluksen pilvipalvelussa Azure-tietokannassa on ensin Pakkaa sovelluksen oikeassa muodossa. (Asennetulla [Azure SDK](https://azure.microsoft.com/downloads/)) **CSPack** komentorivin työkalun avulla voit luoda pakettitiedoston Visual Studio vaihtoehtoisena.

**CSPack** käyttää palvelun määritystiedosto ja palvelun kokoonpanotiedosto sisällön paketin sisällön määrittämisessä. **CSPack** Luo sovelluksen pakettitiedoston (.cspkg), voit ladata Azure [Azure portal](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Oletusarvon mukaan paketin nimi `[ServiceDefinitionFileName].cspkg`, mutta avulla voit määrittää eri nimellä `/out` **CSPack**-vaihtoehtoa.

**CSPack** on yleensä osoitteessa  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
(Windows): n CSPack.exe on käytettävissä **Microsoft Azure komentokehote** -pikakuvakkeen, joka on asennettu SDK suorittamalla.  
>  
>Suorita CSPack.exe ohjelma itse ohjeista kaikki mahdolliset valitsimet ja komennot.

<p />

>[AZURE.TIP]
Suorita pilvipalvelussa sijaitsevaan paikallisesti **Microsoft Azure Laske emulaattorin**, tämä asetus, kopioi hakemisto-asettelu, josta ne voi suorittaa Laske emulaattori sovelluksen binaaritiedostoja **/copyonly** -vaihtoehto.

### <a name="example-command-to-package-a-cloud-service"></a>Esimerkkikomento pakkaaminen pilvipalveluun
Seuraavassa esimerkissä luodaan sovelluksen-paketti, joka sisältää web-roolia koskevia tietoja. Komento määrittää palvelun määritystiedoston käyttämään, josta löytyy binaaritiedostoja-kansio ja Pakettitiedoston nimi.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Jos sovellus on web-rooli ja työntekijän rooli, käytössä on seuraava komento:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Jos muuttujat määritellään seuraavasti:

| Muuttujan                  | Arvo |
| ------------------------- | ----- |
| \[Toiminta\]         | Projektin pääkansion Azure projektin .csdef tiedoston sisältävä alikansion.|
| \[ServiceDefinition\]     | Palvelun määritystiedoston nimi. Oletusarvoisesti tämä tiedosto nimetään ServiceDefinition.csdef.  |
| \[Tulostetiedoston_nimi\]        | Luotu Pakettitiedoston nimi. Tämä on yleensä määritetty sovelluksen nimi. Jos tiedostonimeä ei määritetä, sovelluspaketin luodaan \[ApplicationName\].cspkg.|
| \[RoleName\]              | Roolin määritysten palvelun määritystiedostossa nimi.|
| \[RoleBinariesDirectory] | Binaaritiedostot roolin sijainti.|
| \[VirtualPath\]           | Jokaisen Palvelumääritelmän sivustot-osassa määritetyn virtual polun fyysisiä kansioita.|
| \[PhysicalPath\]          | Jokainen määritetty palvelun määrityksen sivuston-solmu virtual polku sisältö fyysisiä kansioita.|
| \[RoleAssemblyName\]      | Binaaritiedosto roolin nimi.| 


## <a name="next-steps"></a>Seuraavat vaiheet

Cloud palvelun pakkauksen 'M luominen ja haluan nyt...

* [Cloud palvelun esiintymän Etätyöpöytä asetukset][remotedesktop]
* [Ota käyttöön pilvipalvelussa-projekti][deploy]

Käytän Visual Studio ja haluan nyt...

* [Luo uusi pilvipalvelussa][vs_create]
* [Määritä aiemmin pilvipalvelussa][vs_reconfigure]
* [Ota käyttöön pilvipalvelussa-projekti][vs_deploy]
* [Cloud palvelun esiintymän Etätyöpöytä asetukset][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
