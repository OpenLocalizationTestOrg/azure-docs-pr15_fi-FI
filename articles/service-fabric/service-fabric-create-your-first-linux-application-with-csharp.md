<properties
   pageTitle="Luo ensimmäisen palvelun kangasta-sovelluksen käyttäminen C# Linux | Microsoft Azure"
   description="Luoda ja ottaa käyttöön palvelun kangasta-sovellus, joka käyttää C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Ensimmäinen Azure palvelun kangasta-sovelluksen luominen

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Palvelun kangasta on SDK: T etsimisen services Linux .NET Core ja Java. Tässä opetusohjelmassa Odotamme, kuinka voit luoda sovelluksen Linux ja luoda palvelun C# (.NET Core).

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat, varmista, että [Linux kehitysympäristö määrittäminen](service-fabric-get-started-linux.md). Jos käytössäsi on Mac OS X, voit [määrittää Linux-yhden-ympäristön avulla Vagrant virtual kone](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Sovelluksen luominen

Palvelun kangasta-sovellus voi olla vähintään yksi palveluja, joissa tiettyä roolia välittää sovelluksen toiminnot. Linux palvelun kangasta SDK-paketissa sisältää [Yeoman](http://yeoman.io/) luontitoiminto, joka on helppo, voit luoda ensimmäisen palvelun ja lisätä myöhemmin. Sovelluksen luominen yksittäisen palvelussa käyttää Yeoman.

1. Kirjoita päätteeseen, voit aloittaa rakennustelineet muodostamisen seuraava komento:`yo azuresfcsharp`

2. Sovelluksen nimeä.

3. Valitse ensimmäinen palvelun tyypin ja anna sille nimi. Tässä opetusohjelmassa tarkoitetaan on kohdassa luotettava toimija-palvelua.

  ![Palvelun kangasta Yeoman luontitoiminnon c#][sf-yeoman]

>[AZURE.NOTE] Lisätietoja asetuksista on artikkelissa [palvelun kangasta ohjelmoinnin yleiskatsaus](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>Sovelluksen luominen

Palvelun kangasta Yeoman mallit sisältävät muodosta komentosarjan, joiden avulla voit luoda pääte sovellus (sen jälkeen, kun sovellus-kansioon siirtyminen).

  ```bash
 cd myapp 
 ./build.sh 
  ```

## <a name="deploy-the-application"></a>Sovelluksen käyttöönotto

Kun sovellus on luotu, voit ottaa paikallisen klusterin Azure-CLI avulla.

1. Muodosta yhteys palvelun kangasta paikallisen klusterin.

    ```bash
    azure servicefabric cluster connect
    ```

2. Asenna komentosarja annettu mallin avulla voit kopioida sovelluspaketin klusterin kuva säilöön, Rekisteröi sovelluksen tyyppi ja luoda sovelluksen esiintymää.

    ```bash
    ./install.sh
    ```

3. Avaa selain ja siirry kohtaan palvelun kangasta Explorer http://localhost:19080/Explorer (korvaa localhost ja jos Mac OS X Vagrant AM yksityinen IP)-palvelussa.

4. Laajenna sovellukset-solmu ja Huomaa, että on nyt merkintä sovelluksen tyyppi ja toinen tyypin ensimmäisen esiintymän.

## <a name="start-the-test-client-and-perform-a-failover"></a>Aloita testi-asiakasohjelman ja suorittaa automaattisesti

Toimija projektien tehdä mitään yhteiskäyttöä. Ne edellyttävät toisen palvelun tai asiakas voi lähettää viestejä. Toimija-malli sisältää yksinkertainen koe komentosarjan, joka voit käsitellä toimija-palvelun avulla.

1. Seuranta-apuohjelman avulla voit nähdä tulosteen toimija-palvelun komentosarjan suorittaminen

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Etsi solmu isännöinnin ensisijainen se toimija-palvelun kangasta Resurssienhallinnassa. Alla olevassa näyttökuvassa on solmu 3.

    ![Ensisijainen se etsiminen kangasta Explorerissa][sfx-primary]

3. Valitse edellisessä vaiheessa löytämäsi solmu ja valitse sitten Toiminnot-valikosta **Poista aktivointi (Käynnistä)** . Tämä toiminto käynnistyy yhtä viidestä solmujen oman paikallisen klusterin pakottaminen automaattisesti käytössä toisen solmun toissijainen replikan. Voit suorittaa tämän toiminnon, kiinnitä huomiota tulosteen testi asiakkaan ja Huomaa, että laskuri säilyy kasvattaa vikasietotilaa huolimatta.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisätietoja luotettava toimijat](service-fabric-reliable-actors-introduction.md)
- [Palvelun kangasta klustereiden käyttämällä Azure-CLI käyttäminen](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
