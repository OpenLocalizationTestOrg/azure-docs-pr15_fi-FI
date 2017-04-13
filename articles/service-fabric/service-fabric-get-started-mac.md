<properties
   pageTitle="Mac OS x: ssä kehittäminen-ympäristön määritys | Microsoft Azure"
   description="Asenna suorituksenaikainen, SDK ja työkalut ja luoda paikallisen kehittäminen klusterin. Kun olet suorittanut tämän asetuksen, sinun on valmis-sovelluksia Mac OS x: ssä."
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
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Mac OS x: ssä kehittäminen-ympäristön määritys

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OS X](service-fabric-get-started-mac.md)

Voit luoda palvelun kangasta sovellukset toimimaan Linux klustereiden käyttämällä Mac OS X. Tässä artikkelissa kerrotaan, miten määrittämään kehittämiseen Mac-tietokoneeseen.

## <a name="prerequisites"></a>Edellytykset

Palvelun kangasta ei suoriteta grafiikkatiedostomuotoja OS x: ssä. Suorita paikallisen palvelun kangasta-klusterin annamme esimääritettyjä Ubuntu virtual kone Vagrant ja VirtualBox avulla. Ennen kuin aloitat, sinun on:

- [Vagrant (v1.8.4 tai uudempi versio)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>Luo paikalliseen AM

Luoda paikallisen AM sisältävä 5 solmun palvelun kangasta-klusterin, toimi seuraavasti:

1. Kloonaa Vagrantfile repo

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Siirry repo paikallisen Kloonaa

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Valinnainen) Muokkaa AM oletusasetukset

    Oletusarvon mukaan paikallisen AM määritetään seuraavasti:

    - 3 Gigatavua varattu
    - Yksityinen host verkon määritetty IP 192.168.50.50 läpivienti liikenteen Mac-isännästä ottaminen käyttöön

    Voit muuttaa näitä asetuksia tai lisätä muiden asetusten-Vagrantfile AM. On täysi luettelo asetukset [Vagrant ohjeissa](http://www.vagrantup.com/docs) .

4. Luo AM

    ```bash
    vagrant up
    ```

    Tässä vaiheessa lataukset ennalta määritettyjä AM kuva käynnistyksen se paikallisesti ja määritä paikallisen palvelun kangasta klusterin ei. Odotat olisi sen kestää muutaman minuutin. Jos asennus on valmis, näyttöön tulee sanoma, joka ilmaisee, että klusterin käynnistyy tulosteessa.

    ![Klusterin asennuksen käynnistäminen seuraavan AM valmistelu][cluster-setup-script]

5. Testaa, klusterin on määritetty oikein siirtymällä palvelun kangasta Explorer http://192.168.50.50:19080/Explorer (olettaen että säilyttää oletusarvoisesti yksityisverkon IP)-palvelussa.

    ![Palvelun kangasta Explorer tarkastella Mac-isännästä][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Asenna laajennus palvelun kangasta Pimennys Neon (valinnainen)

Palvelun kangasta on laajennuksen Pimennys Neon IDE, joka voidaan yksinkertaistaa ja käyttöönottoon Java-palvelut.

1. Pimennys Varmista että sinulla on Buildship 1.0.17 versio tai uudempi versio. Voit tarkistaa asennetut osat versioiden valitsemalla **Ohje > asennustiedot**. Voit päivittää annettujen ohjeiden mukaisesti Buildship [tähän][buildship-update].

2. Asenna Service kangasta laajennus valitsemalla **Ohje > Asenna uusi ohjelmisto...**

3. Kirjoita "Käyttäminen"-tekstiruutuun: http://dl.windowsazure.com/eclipse/servicefabric.

4. Valitse Lisää.

    ![Pimennys Neon ‑laajennuksen palvelun kangasta][sf-eclipse-plugin-install]

5. Valitse palvelun kangasta laajennus ja valitse sitten Seuraava.

6. Käy läpi asennus ja hyväksy käyttöoikeussopimus.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Luo ensimmäinen palvelun kangasta sovelluksen Linux varten](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Palvelun kangasta-klusterin luominen Azure-portaalissa](service-fabric-cluster-creation-via-portal.md)
- [Luo-palvelun kangasta klusteri Azure resurssien hallinnan avulla](service-fabric-cluster-creation-via-arm.md)
- [Tutustu palvelun kangasta-sovelluksen malli](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
