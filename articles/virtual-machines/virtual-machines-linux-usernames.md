<properties 
    pageTitle="Valitsemalla Linux käyttäjänimet | Microsoft Azure" 
    description="Lue, miten voit valita Linux virtual koneen käyttäjänimet Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



#<a name="selecting-user-names-for-linux-on-azure"></a>Valitse Azure Linux käyttäjänimet valitseminen#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Kun Linux virtual tietokoneen Azure valmistella pääkansio käyttäjän, joiden avulla voit myöhemmin AM Kirjaudu nimi on määritettävä. Voit valita uuden käyttäjän nimi tai jos valmistelu Azure perinteinen portaalin kautta voit hyväksyä oletusarvon nimi "azureuser".

Useimmissa tapauksissa käyttäjä ei ole kuvaa ja luodaan valmistelun yhteydessä. Jos käyttäjä on kantaluku AM kuva, valitse Azure Linux-agentti yksinkertaisesti määrittää salasanan ja/tai kyseisen käyttäjän AM luotaessa määritetty tietojen SSH-näppäintä.

**Kuitenkin**Linux määrittää käyttäjien nimet, joita ei voi käyttää. Valmistelu prosessi on **epäonnistuu** , jos yrität valmistella Linux-AM käyttämällä olemassa olevan Järjestelmäkäyttäjän, joka on määritetty käyttäjänä, jolla UID 0 – 99. Tyypillinen esimerkki on `root` käyttäjä, jolla on UID 0.

 - Katso myös: [Linux Standard Base - Käyttäjätunnus alueet](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Seuraavassa on yleisiä CentOS ja Ubuntu, sinun kannattaa välttää käyttämistä Linux virtual tietokoneen Azure valmisteltaessa sisäinen käyttäjien luettelo. Tässä luettelossa on vain yksi esimerkki, jaettavaksi varmistaa, että valitset käyttäjänimi ei ole ristiriidassa nykyisen Järjestelmäkäyttäjän ohjeissa.


## <a name="centos"></a>CentOS
- abrt
- adm
- ääni
- roskakorit
- CD
- cgred
- Daemon
- dbus
- soittotoiminnolle
- DIP
- levy
- levyke
- FTP
- FTP
- visualisointi n
- Gopher
- haldaemon
- Pysäytä
- kmem
- Lukitse
- Kielipaketin
- sähköposti
- Mies
- muistin
- nfsnobody
- ei kukaan
- NTP
- operaattori
- oprofile
- postdrop
- postfix
- qpidd
- pääkansio
- RPC
- rpcuser
- saslauth
- sulkeminen
- slocate
- sshd
- stapdev
- stapusr
- synkronointi
- sys
- Nauha
- Testaa
- tcpdump
- tekstipuhelintila (TTY)
- käyttäjät
- utempter
- utmp
- uucp
- vcsa
- Video
- vierityspainike


## <a name="ubuntu"></a>Ubuntu
- adm
- Järjestelmänvalvoja
- ääni
- Varmuuskopiointi
- roskakorit
- CD
- crontab
- Daemon
- soittotoiminnolle
- DIP
- levy
- faksin
- levyke
- sulake
- visualisointi n
- gnats
- IRC
- kmem
- vaakasuunta
- libuuid
- luettelo
- Kielipaketin
- sähköposti
- Mies
- MessageBus
- mlocate
- netdev
- Uutiset
- ei kukaan
- nogroup
- operaattori
- plugdev
- välityspalvelin
- pääkansio
- SASL
- varjostus
- src
- ssh
- sshd
- henkilökunnan
- sudo
- synkronointi
- sys
- syslog
- Nauha
- tekstipuhelintila (TTY)
- käyttäjät
- utmp
- uucp
- Video
- vastaajan
- whoopsie
- WWW-tiedot

 