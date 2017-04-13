<properties
    pageTitle="SSH salasanat Linux AM käytöstä määrittämällä SSHD | Microsoft Azure"
    description="Suojatun Linux-AM Azure-poistamalla salasana kirjautumiset SSH varten."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Poista käytöstä SSH salasanat Linux AM määrittämällä SSHD

Tässä artikkelissa kerrotaan siitä, miten voit lukita Linux AM kirjautuminen suojaus.  Kun SSH portti 22 avataan yrität kirjautua sisään arvata salasanoja maailman bots alkuun.  Mitä on hyötyä, tässä artikkelissa on käytöstä salasanan kirjautumiset SSH päälle.  Poistamalla kokonaan voi käyttää salasanoja Microsoft suojaa Linux AM tässä muodossa palvelinten.  Lisätty bonuksen on olemme määritetään Linux SSHD, jos haluat, että vain kirjautumiset kautta julkiset ja yksityiset avaimet SSH mennessä Linux kirjautuessasi turvallisin tapa.  Mahdolliset yhdistelmät se edellyttäisi arvata yksityinen avain on erittäin ja yhdentyneille vuoksi bots-jopa bothering yrittää palvelinten SSH avaimet.


## <a name="goals"></a>Tavoitteet

- Määritä SSHD disallow:
  - Salasanan kirjautumiset
  - Pääkansio kirjautuminen
  - Todennus-vastaus-todennus
- Määritä SSHD, jos haluat, että:
  - vain SSH avaimen kirjautumiset
- Käynnistä SSHD edelleen kirjautuneena
- Uusi SSHD määritysten testaaminen

## <a name="introduction"></a>Johdanto

[Määritetty SSH](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD on SSH palvelin, joka suoritetaan Linux AM.  SSH on asiakaskone, joka suorittaa on MacBook tai Linuxissa työasemalle.  SSH on myös protokolla, jolla voidaan suojata ja salata työaseman ja Linux AM välisen.

Tähän artikkeliin on erittäin tärkeää pitää yhden kirjautuessasi Linux AM Avaa koko tutustua.  Tästä syystä olemme avautuu kaksi päätteitä ja SSH Linux AM, sekä ne.  Käytämme ensimmäisen pääte SSHD-palvelun käynnistäminen uudelleen ja tee haluamasi muutokset SSHDs määritystiedostoa.  Käytämme toisen pääte Testaa muutokset, kun palvelu on käynnistettävä uudelleen.  Koska olemme käytöstä SSH salasanat ja käyttäisit ehdottoman SSH näppäimiä, jos SSH näppäimet eivät ole oikein ja AM yhteys Sulje, AM päivitetään pysyvästi estetty eikä kukaan saa oikeuden kirjautuminen siihen edellyttävän viesti poistetaan ja luoda uudelleen.

## <a name="prerequisites"></a>Edellytykset

- [Luo SSH näppäimet Linux-ja Mac Linux VMs Azure-tietokannassa](virtual-machines-linux-mac-create-ssh-keys.md)
- Azure-tili
  - [ilmainen kokeiluversio liittyminen](https://azure.microsoft.com/pricing/free-trial/)
  - [Azure portal](http://portal.azure.com)
- Linux AM azure-käyttöjärjestelmässä
- SSH julkisen ja yksityisen avaimen paria`~/.ssh/`
- SSH julkinen avain `~/.ssh/authorized_keys` -Linux AM
- Sudo käyttöoikeudet AM
- Avaa 22 portti

## <a name="quick-commands"></a>Pikakomennot

_Kokenut Linux järjestelmänvalvojille, jotka haluat TLDR-versio, Aloita tästä.  Kaikilta muilta haluaa tarkan ja tutustua ohittaa tämän osion._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Yksityiskohtainen tutustua

Kirjaudu Linux AM pääte 1 (T1).  Kirjaudu Linux AM pääte 2 (T2).

Valitse T2 tarkastellaan Muokkaa SSHD määritystiedosto.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

Tässä on Muokkaa vain salasanojen käytöstä ja käyttöön SSH avaimen kirjautumiset asetuksia.  On useita asetuksia, tämä tiedosto, joka olisi tutkiminen ja muuta tekemään Linux & SSH yhtä turvallinen kuin tarvitset.

#### <a name="disable-password-logins"></a>Salasanan kirjautumiset poistaminen käytöstä

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Julkisen avaimen todennuksen käyttöön ottaminen

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Pääkansio kirjautuminen poistaminen käytöstä

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Poista todennus-vastaus-todennus

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Käynnistä SSHD

T1 shell-Varmista, että olet vielä kirjautunut sisään.  Tämä on tärkeitä, joten sinun ei Hae lukita oman AM ulos SSH näppäimet eivät ole oikein, koska salasanat ovat nyt poissa käytöstä.  Jos mikä tahansa asetus ovat virheellisiä, että Linux AM T1 avulla voit korjata sshd_config, voit silti kirjautunut sisään ja SSH valitseminen pitää yhteyttä toiminnassa aikana SSHD-palvelu uudelleen.

Valitse Suorita T2:

##### <a name="on-the-debian-family"></a>Valitse Debian tuoteperhe

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>Valitse RedHat tuoteperhe

```
username@macbook$ sudo service sshd restart
```

Oman AM suojaaminen sen palvelinten salasana kirjautuminen yritykset nyt käytössä salasanat.  Vain SSH nuolinäppäimillä sallittu voi kirjautua sisään nopeammin ja paljon muuta turvallisesti.
