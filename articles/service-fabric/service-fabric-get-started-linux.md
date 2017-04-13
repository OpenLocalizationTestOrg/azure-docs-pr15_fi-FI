<properties
   pageTitle="Linux kehittäminen-ympäristön määritys | Microsoft Azure"
   description="Asenna suorituksenaikainen ja SDK ja luoda paikallisen kehittäminen klusterin Linux. Kun olet suorittanut tämän asetuksen, sinun on valmis-sovelluksia."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Linux kehittäminen lisätietoja ympäristön valmistelemisesta


> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OS X](service-fabric-get-started-mac.md)

 Ottaa käyttöön ja suorittaa [Azure palvelun kangasta sovelluksia](service-fabric-application-model.md) Linux kehittäminen tietokoneeseen, asenna runtime ja yleisiä SDK. Voit myös asentaa valinnainen SDK: T Java ja .NET Core.

## <a name="prerequisites"></a>Edellytykset
### <a name="supported-operating-system-versions"></a>Tuetut käyttöjärjestelmien versiot
Seuraavien käyttöjärjestelmien tuetaan kehittämiseen:

- Ubuntu 16.04 (Xenial Xerus)

## <a name="update-your-apt-sources"></a>Päivitä piharakennus lähteistä

Jos haluat asentaa SDK ja kautta piharakennus get liittyvät runtime-paketti, sinun on päivitettävä piharakennus lähteistä.

1. Avaa päätteeseen.
2. Lisää palvelu kangasta repo lähteet-luetteloon.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Lisää uusi GPG avain piharakennus keyring.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Lisätyn säilöjen tietoihin perustuvan paketin luetteloiden päivittäminen

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Asenna ja määritä SDK

Kun omista lähteistä päivitetään, voit asentaa SDK.

1. Asenna Service kangasta SDK-paketti. Ohjelma pyytää sinua vahvistamaan asennuksen ja hyväksy käyttöoikeussopimus.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. SDK-asennuksen komentosarjan suorittaminen

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>Azure Office kaikissa ympäristöissä CLI määrittäminen

[Azure Office kaikissa ympäristöissä CLI] [ azure-xplat-cli-github] sisältää komennot palvelun kangasta kohteita, kuten klustereiden ja sovellusten käyttäminen. Se perustuu Node.js niin, [Varmista, että olet asentanut solmu] [ install-node] ennen jatkamista alla olevia ohjeita.

1. Kloonaa github repo kehittäminen tietokoneeseesi.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Vaihda kloonatun repo kyselyjä ja asenna CLI riippuvuudet solmu paketin hallinnan (npm) avulla.

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Luo kloonatun repo bin/azure-kansio symlink /usr/bin/azure niin, että se on lisätty path ja komennot ovat käytettävissä minkä tahansa hakemistosta.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Lopuksi Ota käyttöön automaattinen täydentäminen palvelun kangasta komentoja.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Paikallisen klusterin määrittäminen

Jos kaikki on asennettu onnistuneesti, sinun pitäisi Käynnistä paikallisen klusterin.

1. Klusterin asennuksen komentosarjan suorittaminen

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Avaa selain ja siirry http://localhost:19080/Explorer. Jos klusterin on alkanut, pitäisi näkyä palvelun kangasta Explorer Raporttinäkymät-ikkunan.

    ![Palvelun kangasta Explorer Linux][sfx-linux]

Tässä vaiheessa olet valmiita palvelun kangasta sovelluksen pakettien tai uusia Vieras säilöt-tai Vieras suoritettavat käyttöönotto. Jos haluat luoda uusia palveluja Java-tai .NET Core SDK: T, valinnaiset asetukset seuraavia ohjeita noudattamalla.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>(Valinnainen) Java SDK ja Pimennys Neon laajennuksen asentaminen

Java-SDK sisältää kirjastoja ja palvelun kangasta-palveluita käyttämällä Java luonnissa tarvittavat malleja.

1. Asenna Java SDK-paketti.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. SDK-asennuksen komentosarjan suorittaminen

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Voit asentaa Pimennys laajennus palvelun kangasta Pimennys Neon IDE kuluessa.

1. Pimennys Varmista että sinulla on Buildship 1.0.17 versio tai uudempi versio. Voit tarkistaa asennetut osat versioiden valitsemalla **Ohje > asennustiedot**. Voit päivittää annettujen ohjeiden mukaisesti Buildship [tähän][buildship-update].

2. Asenna Service kangasta laajennus valitsemalla **Ohje > Asenna uusi ohjelmisto...**

3. Kirjoita "Käyttäminen"-tekstiruutuun: http://dl.windowsazure.com/eclipse/servicefabric

4. Valitse Lisää.

    ![Pimennys-lisäosa][sf-eclipse-plugin]

5. Valitse palvelun kangasta laajennus ja valitse sitten Seuraava.

6. Käy läpi asennus ja hyväksy käyttöoikeussopimus.

## <a name="install-the-net-core-sdk-optional"></a>Asenna .NET Core SDK (valinnainen)

.NET Core SDK sisältää kirjastoja ja palvelun kangasta-palveluita käyttämällä Office kaikissa ympäristöissä .NET Core luonnissa tarvittavat malleja.

1. Asenna .NET Core SDK-paketti.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. SDK-asennuksen komentosarjan suorittaminen

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Seuraavat vaiheet

- [Linux ensimmäisen Java-sovelluksen luominen](service-fabric-create-your-first-linux-application-with-java.md)

- [OS x-lisätietoja kehitysympäristön valmistelemisesta](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
