<properties
   pageTitle="Hallitse useita ympäristöissä-palvelun kangasta | Microsoft Azure"
   description="Palvelun kangasta sovellukset voidaan suorittaa klustereiden, yksi tietokoneesta kooksi koneet tuhansia alue. Joissakin tapauksissa haluat määrittää kyseiset eri ympäristöissä eri tavalla, sovelluksen. Tässä artikkelissa kerrotaan, miten voit määrittää eri sovelluksessa parametria yhdessä ympäristössä."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Hallitse useita ympäristössä sovelluksen parametrit

Voit luoda Azure palvelun kangasta klustereiden käyttämällä jotakin yhdestä koneet useita tuhansia. Kun sovelluksen binaaritiedostot avata sellaisenaan tämän leveä eri ympäristöjen välillä, usein haluat määrittää sovelluksen eri tavoin sen mukaan, kuinka moneen olet ottaminen käyttöön.

Yksinkertainen esimerkkinä, harkitse `InstanceCount` tilattomien palvelun. Kun käytät sovellukset Azure-tietokannassa, yleensä haluat tämän parametrin arvoksi "1" määräten arvo. Näin varmistat, että palvelu on käynnissä kunkin solmun klusterin. Tämä määritys ei ole kuitenkin sopii yksittäisen tietokoneen klusterin, koska ei voi olla useita prosesseja listening saman päätepisteen yhteen tietokoneeseen. Sen sijaan, voit tavallisesti määrittää `InstanceCount` "1".

## <a name="specifying-environment-specific-parameters"></a>Ympäristön kielikohtaiset parametrien määrittäminen

Määritysten ongelman ratkaisu on joukko Parametroitu oletusarvon services ja sovelluksen parametritiedostot, jotka täyttävät parametriarvot ympäristössä. Oletuspalveluiden ja sovelluksen parametrit on määritetty sovellus- ja -luettelot. ServiceManifest.xml ja ApplicationManifest.xml tiedostojen rakenteen määritys on asennettu palvelun kangasta SDK ja *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*-työkaluja.

### <a name="default-services"></a>Oletus-palvelut

Palvelun kangasta sovellusten koostuvat kokoelma palvelun esiintymät. Vaikka voit luoda tyhjän sovelluksen ja luo sitten kaikki palveluesiintymiä dynaamisesti, useimpien sovellusten on core Services, aina luodaan, kun sovellus esiintymää. Nämä kutsutaan "oletus-palvelut". Ne on määritetty sovellusluettelo ympäristön-määritys sisältyy hakasulkeisiin paikkamerkkejä kanssa:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Kunkin nimetyt parametrit on määritettävä sovelluksen luettelo parametrit-elementissä:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Oletusarvo-määrite määrittää arvon, jota käytetään Lisää kielikohtaiset parametrin puuttuessa ympäristössä.

>[AZURE.NOTE] Kaikki palvelun esiintymän parametrien soveltuvat ympäristön-määritys. Yllä olevassa esimerkissä palvelun osiointimalli LowKey ja HighKey arvot erikseen määritetään palvelun kaikkien esiintymien koska osio on funktiota tietojen muokattavan toimialueen ei ympäristössä.


### <a name="per-environment-service-configuration-settings"></a>Ympäristön-Palveluasetukset

[Palvelun kangasta sovelluksen malli](service-fabric-application-model.md) mahdollistaa palveluiden sisältämään määritys-paketteja, jotka sisältävät mukautetut avain-arvo-pareina, joita voi lukea suorituksen aikana. Näistä asetuksista arvot myös voidaan eritellä mukaan ympäristön määrittämällä `ConfigOverride` sovelluksen luettelossa.

Oletetaan, että seuraava asetus on Config\Settings.xml-tiedostossa `Stateful1` palvelu:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Voit ohittaa tämän arvon tietyn sovelluksen/ympäristön pari luominen `ConfigOverride` tuotaessa sovelluksen luettelo service-luettelo.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Tämä parametri sitten voidaan määrittää ympäristön yllä esitetyllä tavalla. Voit tehdä tämän määritteleminen sovelluksen luettelo parametrit-osassa ja määrittämällä ympäristön kielikohtaiset arvot sovelluksen parametri-tiedostoissa.

>[AZURE.NOTE] Palveluasetukset, kyseessä on kolmessa paikassa, jossa voidaan määrittää avaimen arvo: määritys palvelupakettiin, sovelluksen luettelo ja sovelluksen parametri-tiedosto. Palvelun kangasta valittavana aina sovelluksen parametri-tiedoston ensimmäisen (Jos määritetty), valitse sovellusluettelo ja lopuksi määritys-paketti.


### <a name="application-parameter-files"></a>Sovelluksen parametritiedostot

Palvelun kangasta sovelluksen project voi sisältää vähintään yhden sovelluksen tiedostoista. Jokainen niistä määrittää arvot, jotka on määritetty sovellusluettelo parametrien:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Oletusarvon mukaan uuden sovelluksen sisältää kaksi sovelluksen parametrin järjestysluvut, Local.xml ja Cloud.xml:

![Sovelluksen parametrin tiedostojen ratkaisunhallinnassa][app-parameters-solution-explorer]

Voit luoda uuden parametri-tiedoston yksinkertaisesti kopioida ja liittää aiemmin luodun ja nimetä sen uudelleen.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Paikallisen ympäristön kielikohtaiset parametrit käyttöönoton aikana

Käyttöönoton aikana sinun täytyy lisätä sovelluksen tarvittavat parametrit-tiedosto. Voit tehdä tämän Visual Studiossa Julkaise-valintaikkunan kautta tai PowerShellin kautta.

### <a name="deploy-from-visual-studio"></a>Visual Studio käyttöönotto

Voit valita käytettävissä parametrin tiedostojen luettelosta, kun julkaiset sovelluksen Visual Studiossa.

![Valitse Julkaise-valintaikkunasta parametri-tiedosto][publishdialog]

### <a name="deploy-from-powershell"></a>PowerShell-käyttöönotto

`Deploy-FabricApplication.ps1` PowerShell-komentosarjaa sovelluksen projektimalli sisältyvät hyväksyy Julkaise profiilin parametrina ja PublishProfile sisältää viittauksen sovelluksen parametrit-tiedostoon.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja joitakin core-käsitteestä, jotka tässä artikkelissa käsitellään artikkelissa [palvelun kangasta teknisiä tietoja](service-fabric-technical-overview.md). Tietoja muiden app-ominaisuuksia, jotka ovat käytettävissä Visual Studiossa artikkelissa [Visual Studiossa palvelun kangasta sovellusten hallinta](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
