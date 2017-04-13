<properties 
    pageTitle="Apache Qpid Proton-C asentamisesta Linux AM | Microsoft Azure"
    description="Käyttämällä Azuren näennäiskoneiden CentOS Linux-AM luomisesta ja luomista ja asenna Apache Qpid Proton-C-kirjasto."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Azure Linux-AM Apache Qpid Proton-C asentaminen

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Tässä osassa esitellään luominen käyttämällä Azuren näennäiskoneiden CentOS Linux-AM ja lataaminen, luominen ja asenna sekä Python ja PHP kielen sidontojen Apache Qpid Proton-C-kirjasto. Näiden vaiheiden suorittamisen jälkeen osaat Suorita tässä oppaassa mukana Python ja PHP mallit.

Ensimmäinen vaihe suoritetaan [Azure perinteinen portal][]. Seuraavassa näyttökuvassa näkyy, miten CentOS-AM nimeltä "Juhani centos" on luotu:

![Valitse Azure Linux-AM proton][0]

Valmistelun, jälkeen portaalin tuo näkyviin seuraavat tiedot:

![Valitse Azure Linux-AM proton][1]

Jotta voit kirjautua sisään tietokoneeseen, sinun on tiedettävä SSH päätepisteportti. Voit hankkia arvoksi [Azure perinteinen portal][] valitsemalla juuri luomasi AM ja napsauttamalla **päätepisteet** -välilehdessä. Seuraavassa näyttökuvassa näkyy, että tietokoneen julkisen SSH portti on 57146.

![Valitse Azure Linux-AM proton][2]

Voit muodostaa SSH tietokoneelle. Tässä esimerkissä käytetään painovärit, muste työkalu, kuten seuraavassa näyttökuvan:

![Valitse Azure Linux-AM proton][3]

Tässä esimerkissä käytetään Python ja PHP-sovellusten Apache Proton asiakkaan kirjastoihin. Nämä kirjastot ovat ladattavissa [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Jakauman paketin Lueminut-tiedosto kerrotaan, asentaa riippuvuudet ja luoda Proton tarvittavat vaiheet. Tässä on yhteenveto vaiheita:

1.  Muokkaa yum määritystiedosto (/ etc/yum.conf) ja pois jätettyjen päivitykset ydin otsikkoihin kommentti (\# pois = ydin\*). Tämä on asennettava gcc kääntäjä.

2.  Asenna tarvittavat paketit:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  Lataa Proton-kirjastoon:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  Pura Proton koodin jakauman arkistosta:

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Muodosta ja asenna koodin otetun Lueminut-tiedosto seuraavasti:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Näiden vaiheiden suorituksen jälkeen Proton on asennettu tietokoneeseen ja valmis käytettäväksi.

## <a name="next-steps"></a>Seuraavat vaiheet

Haluatko lisätietoja? Käy napsauttamalla seuraavaa linkkiä:

- [Palvelun Bus AMQP yleiskatsaus][]

[Palvelun Bus AMQP yleiskatsaus]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure perinteinen portal]: http://manage.windowsazure.com


