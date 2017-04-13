<properties
   pageTitle="Tietoja palvelun kangasta sovelluksen ja palvelun suojauskäytäntöjä | Microsoft Azure"
   description="Opit järjestelmä ja paikallisen suojauksen käyttäjätilejä, kuten SetupEntry-kohta, jossa sovellus on suoritettava sellaisten jonkin toiminnon, ennen kuin se käynnistyy palvelun kangasta sovelluksen suoritetaan"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Sovelluksen suojauskäytäntöjen määrittäminen
Azure palvelun kangasta mahdollistaa suojatun klusterin eri käyttäjätileille Valitse sovellukset. Palvelun kangasta suojaa myös käyttämät sovellusten käyttöönoton käyttäjätiliä, kuten tiedostot, kansiot ja varmenteet milloin resurssit. Tämä on käynnissä olevat ohjelmat, myös jaetussa isännöityä ympäristössä, suojattu toisistaan. 

Oletusarvon mukaan palvelun kangasta sovellukset suoritetaan, joka toimii Fabric.exe prosessi-tilillä. Palvelun kangasta mahdollistaa myös sovelluksia paikallisen käyttäjätilin tai paikallisen järjestelmätilin määritettyjen sovellusluettelo. Tuetut paikallinen järjestelmä tilityyppejä ovat **LocalUser**, **NetworkService**tai **LocalService** **LocalSystem**.

 Suoritettaessa palvelun kangasta Windows Server-että tietokeskuksen erillisen-asennuksen avulla voit käyttää Active Directory (AD) toimialuetilit.

Käyttäjäryhmien voidaan määrittää ja luo siten, että yhden tai usean käyttäjän voidaan lisätä kunkin ryhmän tuleeko yhdessä. Tästä on hyötyä, kun on useita käyttäjiä eri pikakuvakkeet ja tarvittavista on tiettyjä yleiset käyttöoikeudet, jotka ovat käytettävissä ryhmän tasolla.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Palvelun SetupEntryPoint käytännön määrittäminen

Kuvatulla tavalla [sovelluksen mallin](service-fabric-application-model.md) **SetupEntryPoint** on sellaisten aloituskohtaa, joka suoritetaan muita aloituskohtaa ennen kuin palvelun kangasta (yleensä *NetworkService* -tili) samoilla tunnuksilla. **Aloituskohta** määrittämää suoritettavat on yleensä pitkään suoritettavien service-isännässä, joten on erillinen asennuksen aloituskohta vältetään tarvitse suorittaa suoritettavan palvelun host hyvin oikeuksilla pitkän ajan. **Aloituskohta** määrittämää suoritettavat suoritetaan, kun **SetupEntryPoint** poistuu onnistuneesti. Tuloksena oleva prosessi on seurattava ja käynnistetään uudelleen, alkavat uudelleen **SetupEntryPoint**, jos koskaan lopettaa tai kaatuu.

Seuraavassa on yksinkertainen palvelun luettelon esimerkki, jossa näkyy SetupEntryPoint ja tärkeimmät aloituskohta palvelulle.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Käytännön avulla paikallisen tilin määrittäminen

Kun olet määrittänyt asetukset aloituskohta on palvelu, voit muuttaa käyttöoikeuksia, se suoritetaan sovelluksen luettelossa. Followin-esimerkissä suoritetaan käyttäjän järjestelmänvalvojaksi palvelun määrittämisestä.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Luo **ansaitun** osan käyttäjän nimi, kuten SetupAdminUser. Tämä ilmaisee, että käyttäjällä on järjestelmän Järjestelmänvalvojat-ryhmän jäsen.

Seuraavaksi **ServiceManifestImport** -kohdassa Määritä käytäntöä sovelletaan tämän lyhennys **SetupEntryPoint**. Tämä kertoo palvelun kangasta, että **MySetup.bat** tiedoston suoritettaessa sen pitäisi olla RunAs järjestelmänvalvojan oikeuksilla. Koska, että sinulla *ei* käytetä käytännön Päähakusana-kohdan, **MyServiceHost.exe** koodia suoritetaan järjestelmän **NetworkService** -tiliä. Tämä on oletustilin, joka suoritetaan kaikki palvelun pikakuvakkeet.

Katsotaan nyt voit lisätä tiedoston MySetup.bat Visual Studio projektiin Testaa järjestelmänvalvojan oikeudet. Visual Studiossa Napsauta palvelun projektin ja Lisää uusi tiedosto nimeltä MySetup.bat.

Varmista seuraavaksi, että palvelupakettiin sisältyy MySetup.bat-tiedosto. Oletusarvon mukaan ei ole. Valitse tiedosto, saat pikavalikon hiiren kakkospainikkeella ja valitse **Ominaisuudet**. Valitse ominaisuudet-valintaikkunassa varmistamaan, että **Kopioi tulosteen-kansioon** on **kopioiminen Jos uudempaan**. Näkyviin tulee seuraava näyttö koodin.

![Visual Studio-CopyToOutput SetupEntryPoint komentojonotiedosto][image1]

Nyt MySetup.bat-tiedoston avaaminen ja lisää seuraavista komennoista:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Luoda ja ottaa ratkaisun paikallista kehittämistä klusteriin.  Kun palvelu on alkanut, kuten kangasta Explorerissa, näet, että MySetup.bat onnistui kahdella tavalla. Avaa PowerShell-komentokehote ja kirjoita:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Tarkista, palvelun on otettu käyttöön ja käynnistää palvelun kangasta Resurssienhallinnassa esimerkiksi solmu 2 solmun nimi. Siirry seuraavaksi out.txt-tiedosto, joka näyttää arvon **TestVariable**sovelluksen esiintymää työ-kansioon. Jos tämä palvelu on otettu käyttöön solmu 2 ja valitse esimerkiksi voit Valitse polkuun **MyApplicationType**:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Paikallinen järjestelmä tilien käytännön määrittäminen
Usein on suositeltavaa käyttää paikallisen järjestelmätilin järjestelmänvalvojat-tiliin, kuten edeltävän sijaan käynnistyskomentosarjan suorittaminen. Käytössä RunAs käytännön järjestelmänvalvojia yleensä ei toimi hyvin, koska tietokoneissa on käyttäjän Access valvonnan oletusarvon mukaan käytössä. Tässä tapauksessa **Suosituksena on Suorita SetupEntryPoint järjestelmänä sen sijaan, että paikallinen käyttäjä järjestelmänvalvojien ryhmään**. Seuraavassa esimerkissä määrittäminen toimimaan järjestelmänä SetupEntryPoint.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>Käynnistä PowerShell-komennoista SetupEntryPoint
Suorittamaan PowerShell **SetupEntryPoint** -kohdassa voit suorittaa **PowerShell.exe** komentojonotiedosto, joka viittaa PowerShell-tiedostoon. Lisää ensin palvelun projektiin, kuten **MySetup.ps1**PowerShell-tiedosto. Muista *kopioida, jos uudempaan* -ominaisuuden määrittäminen niin, että palvelupakettiin sisältyy myös tiedoston. Seuraavassa esimerkissä esitetään käynnistää PowerShell-tiedosto nimeltä MySetup.ps1, joka määrittää järjestelmän ympäristömuuttuja, jota kutsutaan **TestVariable**erä mallitiedosto.


MySetup.bat käynnistää PowerShell-tiedosto.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

Lisää PowerShell-tiedosto, voit määrittää järjestelmän ympäristömuuttuja seuraavasti:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Huomautus:** Jos komentojonotiedosto suoritetaan se näyttää oletusarvoisesti kutsutaan tiedostojen **toimi** sovelluksen-kansioon. Tässä tapauksessa MySetup.bat suoritettaessa haluamme tämän, jotta löydät MySetup.ps1 samaan kansioon, joka on sovelluksen **koodin paketti** -kansiossa. Jos haluat muuttaa tähän kansioon, Määritä työkansion seuraavassa esitetyllä tavalla.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Paikallinen virheenkorjaus konsolin uudelleenohjauksen käyttäminen
Joskus kannattaa käyttää nähdä konsolin tulosteen suorittamasta komentosarjaa vianmääritystä varten. Näin voit määrittää konsolin uudelleenohjauskäytännön, joka kirjoittaa tulosteen tiedostoon. Tiedoston tulosteen kirjoitetaan sovelluksen-kansio nimeltä **log** solmun, jossa sovellus otetaan käyttöön ja suorita (Katso löydät tämän esimerkin).

**Huomautus: koskaan** konsolin uudelleenohjauskäytännön käyttäminen sovelluksen käyttöön tuotannon, koska tämä vaikuttaa sovelluksen vikasietotilaa. **Vain** tällä paikallista kehittämistä ja muistin varten.  

Seuraavassa esimerkissä FileRetentionCount arvolla console-uudelleenohjauksen määrittäminen.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Jos muutat nyt kirjoittaa **Päivitä** -komento MySetup.ps1-tiedoston, tämä kirjoittaa kohdetiedosto vianmääritystä varten.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Kun olet aloittaessasi komentosarja, heti Poista tämä konsoli uudelleenohjaus käytäntö**

## <a name="configure-policy-for-service-code-packages"></a>Palvelun koodi pakettien käytännön määrittäminen 
Edellä kuvatut toimet näyttöön tuli ottamisesta käyttöön RunAs käytännön SetupEntryPoint. Seuraavassa esitetty hieman tarkempaa luominen eri ansaitun, joka voi käyttää palvelua käytännöt kyselyjä.

### <a name="create-local-user-groups"></a>Paikallinen käyttäjäryhmien luominen
Käyttäjäryhmien voidaan määrittää ja luoda, joka mahdollistaa yhden tai usean käyttäjän lisääminen ryhmään. Tämä on erityisen hyödyllinen, jos on useita käyttäjiä eri pikakuvakkeet ja tarvittavista on tiettyjä yleiset käyttöoikeudet, jotka ovat käytettävissä ryhmän tasolla. Seuraavassa esimerkissä esitetään paikallisen ryhmän nimeltä **LocalAdminGroup** järjestelmänvalvojan oikeuksilla. Kaksi käyttäjää, Customer1 ja Customer2, tehdään paikallisen tämän ryhmän jäsenet.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Luo Paikalliset käyttäjät
Voit luoda paikallinen käyttäjä, jonka avulla voidaan suojata palvelu-sovelluksessa. Kun **LocalUser** tilityyppi on määritetty sovellusluettelo ansaitun-osassa, palvelun kangasta Luo paikallisten käyttäjätilien tietokoneissa, joissa sovellus otetaan käyttöön. Oletusarvon mukaan näissä tileissä ei ole on sama nimi kuin määritettyjä sovellusluettelo (esimerkiksi Customer3 seuraavassa esimerkissä). Sen sijaan ne luodaan dynaamisesti ja on satunnainen salasanoja.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Määritä käytännöt service koodi-paketit
**ServiceManifestImport** **RunAsPolicy** -osassa määrittää tilin ansaitun osasta, joka olisi käytettävä koodi-paketin suorittaminen. Se myös liittää koodin palvelu luettelo palvelupaketteja käyttäjätilit ansaitun-osassa. Voit määrittää asetukset tai Päähakusana pisteitä, tai voit määrittää `All` voit käyttää molempia. Seuraavassa esimerkissä esitetään eri käytännöt otetaan käyttöön:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Jos **EntryPointType** ei ole määritetty, oletusarvo on määritetty EntryPointType = "Tärkeimmät". Määrittämällä **SetupEntryPoint** on hyötyä erityisesti silloin, kun haluat suorittaa tiettyjä suuren käyttöoikeuksien asetukset toiminnon järjestelmä-tilissä. Todellinen palvelukoodi suorittamisen alemman käyttöoikeuksien-tilissä.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Määrittää oletusarvon käytännön kaikki service koodi-paketit
Määritä kaikki koodi-paketteja, jotka eivät sisällä tiettyä **RunAsPolicy** , määritetty käyttäjälle tili käytetään **DefaultRunAsPolicy** -osassa. Jos useimmat määritetyn palvelun luettelo sovellus käyttää sitä koodi-paketit on sama käyttäjä suoritetaan, sovellus määrittää vain oletusarvon RunAs käytännön käyttäjän kyseisellä tilillä. Seuraavassa esimerkissä määrittää, että jos koodi pakettia ei ole **RunAsPolicy** määritetty-koodi-paketin suoritetaan **MyDefaultAccount** ansaitun-osassa.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Active Directory-toimialueryhmän tai käyttäjän avulla
Palvelun kangasta on asennettu Windows Server erillisen-asennuksen kanssa, voit suorittaa Active Directory (AD) käyttäjän tai ryhmätilin tunnistetiedot-kohdassa palvelu. Huomautus: Tämä on Active Directory toimialueellasi sijaitseville paikalliset ja ei ole ja Azure Active Directory (AAD). Käyttämällä toimialuekäyttäjän tai ryhmän, voit käyttää sitten muut resurssit (esimerkiksi tiedostoresurssit) toimialueen, sinulla on tarvittavat käyttöoikeudet.

Seuraavassa esimerkissä esitetään, AD-käyttäjä nimeltä *TestUser* salattu sertifikaatilla, jota kutsutaan *MyCert*salasanan. Voit käyttää `Invoke-ServiceFabricEncryptText` Powershell-komentoa, voit luoda salainen salaus tekstin. Katso tämän artikkelin [tietoja palvelun kangasta sovellusten hallinta](service-fabric-application-secret-management.md) . Salasanan salauksen sertifikaatin yksityinen avain on otettava käyttöön paikallinen kone-nauha-menetelmän (Valitse Azure tämä on Resurssienhallinta kautta). Sitten, kun palvelun kangasta käyttää tietokoneen palvelupakettiin, on salauksen toiminta ja todentamismenetelmä käyttäjänimi, sekä AD suoritetaan tunnistetiedot.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>Määritä HTTP- ja HTTPS päätepisteet SecurityAccessPolicy
Jos RunAs käytäntöä sovelletaan palvelu ja palvelun luettelo ilmoittaa päätepisteen resursseja HTTP-protokolla, sinun on määritettävä **SecurityAccessPolicy** varmistaa, että nämä päätepisteet kohdistettu portit ovat oikein käyttöoikeuksien hallinta luettelossa RunAs-käyttäjätili, jolla palvelua suoritetaan. Muussa tapauksessa **http** ei ole käyttämään palvelua ja saat virheet soitettaessa asiakaskoneesta. Followin esimerkki koskee Customer3 tilin päätepisteen kutsutaan **ServiceEndpointName**, jossa se on täydet oikeudet.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

HTTPS-päätepisteen käytössä on myös osoittamaan varmenteen palaa asiakkaan nimi. Voit tehdä tämän käyttämällä **EndpointBindingPolicy**sertifikaatin määritetty sovelluksen luettelo varmenteet-kohdassa.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Valmis sovelluksen tiedostojen Esimerkki
Seuraavat sovellusluettelo on monia erilaisia vaihtoehtoja:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

* [Tietoja sovelluksen malli](service-fabric-application-model.md)
* [Määritä resurssien service-luettelo](service-fabric-service-manifest-resources.md)
* [Sovelluksen ottaminen käyttöön](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
