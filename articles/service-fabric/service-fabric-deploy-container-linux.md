<properties
   pageTitle="Palvelun kangasta ja Linux käyttöönottaminen säilöjä | Microsoft Azure"
   description="Palvelun kangasta ja käytä Docker säilöjä microservice sovellusten. Tässä artikkelissa kuvataan ominaisuuksista, joita palvelun kangasta tarjoaa säilöjä ja käyttöönottaminen klusteriin Docker säilö-kuva"
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
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Ottaa käyttöön palvelun kangasta Docker säilö

> [AZURE.SELECTOR]
- [Windows-säilön käyttöönotto](service-fabric-deploy-container.md)
- [Ottaa käyttöön Docker säilö](service-fabric-deploy-container-linux.md)

Tässä artikkelissa käydään läpi rakentaminen tai kontteihin pakattuja Services-palvelujen Docker säilöjen Linux.

Palvelun kangasta on useita säilö-ominaisuuksia, jotka auttavat sovellusten, jotka koostuvat, jotka ovat tai kontteihin pakattuja microservices. Näitä kutsutaan tai kontteihin pakattuja palvelut.

Ominaisuuksia ovat;

- Säilön kuva käyttöönotto- ja aktivointi
- Resurssin hallinnon
- Tietovaraston todennus
- Säilön portin host portin määritys
- Säilön säilö etsiminen ja viestintä
- Mahdollisuus määrittää ja määrittää ympäristömuuttujat


## <a name="packaging-a-docker-container-with-yeoman"></a>Pakkaamista docker säilön yeoman kanssa
Kun pakkaaminen Linux säilön, voit joko käyttää yeoman mallia tai [luoda sovelluspaketin manuaalisesti](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container).

Palvelun kangasta-sovellus voi olla yksi tai useampi säilöjen, joissa tiettyä roolia välittää sovelluksen toiminnot. Linux palvelun kangasta SDK-paketissa sisältää [Yeoman](http://yeoman.io/) luontitoiminto, joka on helppo luoda sovelluksen ja lisää säilö kuva. Oletetaan, että uuden sovelluksen luominen yksittäisen Docker säilöön, jota kutsutaan *SimpleContainerApp*Yeoman avulla. Voit lisätä Lisää palveluja myöhemmin muokkaamalla muodostetut luettelon tiedostot.

## <a name="create-the-application"></a>Sovelluksen luominen

1. Kirjoita terminaalissa, **yo azuresfguest**.

2. Valitse kehys **säilö** .

3. Sovelluksen esimerkiksi SimpleContainerApp nimi

4. Säätää DockerHub repo säilö kuvan URL-osoite. Tämä tuo lomakkeen [repo] / [kuvan nimi]

![Palvelun kangasta Yeoman luontitoiminto säilöjen varten][sf-yeoman]

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

5. Poista komentosarja annettu mallin avulla voit sovelluksen poisto ja unregister sovelluksen tyyppi.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Seuraavat vaiheet

- [Yleisiä tietoja palvelun kangasta ja säilöt](service-fabric-containers-overview.md)
- [Palvelun kangasta klustereiden käyttämällä Azure-CLI käyttäminen](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

