<properties
    pageTitle="Luo SSH avaimet Linux-ja Mac | Microsoft Azure"
    description="Luo ja käyttää SSH näppäimet Linux- ja Mac Resurssienhallinta ja Azure malleissa perinteinen käyttöönotto."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>Luo SSH näppäimet Linux-ja Mac Linux VMs Azure-tietokannassa

Voit luoda Azure, oletusarvo on SSH näppäimellä todennuksessa poistamisesta kirjautumalla salasanoja tarpeen näennäiskoneiden SSH avainpari kanssa.  Salasanojen on arvata, ja avaa oman VMs puolustaja palvelinten voimassa yrittää arvata salasanaasi ylöspäin. Azure-mallien avulla luotu VMs tai `azure-cli` voi olla SSH-julkinen avain käyttöönoton kirjaa käyttöönoton määrityksen poistaminen osana.  Jos olet muodostamassa yhteyttä Linux AM Windowsista, katso [tämän asiakirjan.](virtual-machines-linux-ssh-from-windows.md)

Artikkelin edellyttää:

- Azure tili ([maksuton kokeiluversio](https://azure.microsoft.com/pricing/free-trial/)).

- [Azure CLI](../xplat-cli-install.md) kirjautunut sisään`azure login`

- Azure CLI _on oltava_ Azure resurssien hallinnan tila`azure config mode arm`

## <a name="quick-commands"></a>Pikakomennot

Korvaa seuraavat komennot ja esimerkkejä oman valinnat.

SSH avaimet ovat oletusarvon mukaan, mitä `.ssh` hakemisto.  

```bash
cd ~/.ssh/
```

Jos sinulla ei ole `~/.ssh` hakemisto `ssh-keygen` komento luo sen, joilla on tarvittavat oikeudet.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Kyselyjä tallennetun tiedoston nimi `~/.ssh/` hakemisto:

```bash
id_rsa
```

Kirjoita salasana id_rsa:

```bash
correct horse battery staple
```

On nyt `id_rsa` ja `id_rsa.pub` SSH avaimen paria `~/.ssh` hakemisto.

```bash
ls -al ~/.ssh
```

Lisää juuri luomasi avaimen `ssh-agent` Linux-ja Mac (myös lisätään OS x-Avainnippuun):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Kopioi SSH julkinen avain Linux-palvelimeen:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Testaa Kirjaudu sisään käyttämällä näppäimet salasanan sijaan:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Laskun

Helpoin tapa Linux-palvelimet kirjautuminen käyttäminen SSH julkiset ja yksityiset avaimet on. [Julkisen avaimen](https://en.wikipedia.org/wiki/Public-key_cryptography) on paljon tehokkaampi tapa kirjautuminen Linux tai BSD AM Azure-tietokannassa kuin salasanat, joka voi olla kannalta pakotettu kaukana helpommin. Kuka tahansa, voi jakaa julkinen avain mutta vain sinä (tai paikalliseen suojaus-infrastruktuurin) käytössään yksityinen avain.  SSH yksityinen avain pitäisi olla [hyvin suojattu salasanalla](https://www.xkcd.com/936/) (lähde:[xkcd.com](https://xkcd.com)) suojaamiseksi sen.  Salasana on käyttää SSH yksityinen avain ja **ei** käyttäjätilin salasanan.  Kun lisäät salasanan SSH key-tuotetunnuksen, salaa yksityinen avain niin, että yksityinen avain on hyödytön ilman salasanaa lukitus.  Jos luvaton varastaa tietoturva yksityinen avain ja avaimen ei ollut salasanan, ne olisi käyttää yksityinen avain kirjautua palvelimet, joilla on vastaava julkinen avain.  Jos yksityinen avain on suojattu salasanalla, sitä ei voi käyttää kyseisen hänen tarjoamalla suojauksen kerroksen Azure-infrastruktuurin.

Tässä artikkelissa Luo *ssh-rsa* muotoiltu avaimen tiedostot, jotka on suositeltavaa Resurssienhallinta-käyttöönotoissa.  *ssh-rsa* näppäimet on käytettävä perinteinen ja Resurssienhallinta-versioiden [portal](https://portal.azure.com) .

## <a name="create-the-ssh-keys"></a>Luo SSH avaimet

Azure edellyttää vähintään 2048-bittinen, ssh-rsa Muotoile julkiset ja yksityiset avaimet. Luo näppäimet käytön `ssh-keygen`, joka kysyy, kysymyksiä ja kirjoittaa sitten yksityinen avain ja vastaava julkinen avain. Azure-AM luomisen jälkeen julkinen avain kopioidaan `~/.ssh/authorized_keys`.  SSH näppäimet `~/.ssh/authorized_keys` käytetään hakea asiakkaan vastaamaan vastaava yksityinen avain SSH-kirjautumisen yhteydessä.

## <a name="using-ssh-keygen"></a>Ssh-keygen käyttäminen

Tämä komento luo suojattu (salattu) SSH avainpari käyttämällä 2048-bittinen RSA salasanan ja se on Kommentoitu tunnistaa sen helposti.  

Voit aloittaa muuttamalla hakemistojen selaaminen, niin, että kaikki oman ssh näppäimet luodaan kyseiseen kansioon.

```bash
cd ~/.ssh
```

Jos sinulla ei ole `~/.ssh` hakemisto `ssh-keygen` komento luo sen, joilla on tarvittavat oikeudet.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Komennon kuvaus_

`ssh-keygen`= näppäimet luomiseen käytetty ohjelma

`-t rsa`= avaimen luomiseen, joka on tyyppi [RSA muoto](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bittien avaimen

`-C "myusername@myserver"`= kommentin julkisen avaimen tiedoston tunnistamisen sen perään.  Tavallisesti sähköpostiviestin käytetään kommentin, mutta voit käyttää mitä tahansa toimii parhaiten infrastruktuurin.

### <a name="using-pem-keys"></a>PEM-näppäimellä

Jos käytät perinteinen ottaa mallin käyttöön (Azure perinteinen Portal tai Azure Service Management CLI `asm`), voit joutua muuttamaan käyttää PEM muotoiltu SSH näppäimet, jotta voit käyttää Linux VMs.  Näin PEM näppäintä luominen aiemmin luotuun SSH julkisella avaimella, ja aiemmin x509 varmenne.

Voit luoda PEM muotoiltu aiemmin SSH julkinen avain avain:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Esimerkki ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Tallennetut tiedostot:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Tässä artikkelissa avaimen pari nimi.  Jotkin työkalut voisi olettaa **id_rsa** yksityinen avain-tiedostonimi, jotta yhteen on hyvä ottaa avaimen lainausmerkeiksi nimeltä **id_rsa** on oletusarvo. Hakemiston `~/.ssh/` SSH avaimen paria ja SSH Määritystiedoston oletussijainti on.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Luettelo `~/.ssh` hakemisto. `ssh-keygen`Luo `~/.ssh` hakemiston, jos se ei ole ja määrittää myös oikean omistus- ja tiedostojen tilat.

Avaimen salasana:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`viittaa salasanan nimellä "salasana".  On *erittäin* suositeltavaa salasanan lisääminen avaimen paria. Avaimen pari suojaaminen salasanalla kaikki, joilla on yksityinen avain-tiedosto käyttää sitä kirjautua sisään palvelimeen, joka sisältää vastaavan julkinen avain. Lisäämällä salasanan tarjoaa paremman suojauksen, jos joku ei pysty käyttämään yksityisen avaimen tiedoston antanut sinulle käytetään, kun haluat muuttaa näppäimet todennetaan.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Ssh-agentti avulla voit tallentaa yksityisen avaimen salasana

Voit välttää jokaisen SSH kirjautuminen yksityinen avain-tiedosto salasanaa, voit käyttää `ssh-agent` välimuistiin yksityinen avain-tiedosto-salasanasi. Jos käytössäsi on Mac-tietokoneeseen, OS x-Avainnipun tallentaa yksityisen avaimen salasanat turvallisesti suorittaessasi `ssh-agent`.

Varmista ensin, että `ssh-agent` on käynnissä

```bash
eval "$(ssh-agent -s)"
```

Lisää nyt yksityinen avain `ssh-agent` -komennon avulla `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Yksityisen avaimen salasana on nyt tallennettu `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Luo ja määritä SSH-määritystiedosto

Luominen ja määrittäminen suositellut paras käytäntö on `~/.ssh/config` tiedoston nopeuttaa Kirjaudu laajennukset ja optimointi SSH asiakas-ongelma.

Seuraavassa esimerkissä esitetään vakio määritys.

### <a name="create-the-file"></a>Tiedoston luominen

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Muokkaa tiedostoa Lisää uudet SSH määritykset:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Esimerkki `~/.ssh/config` tiedosto:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Tämä SSH config tutustutaan osien käyttöön kunkin on oma erillinen avaimen pari kullekin palvelimelle. Oletusasetuksia (`Host *`) ovat isäntien, jotka eivät vastaa mitään sellaisten tietyn isäntien ylempänä määritystiedostossa.


### <a name="config-file-explained"></a>Kuvaus asetustiedosto

`Host`= Päätteen kutsutaan isännän nimen.  `ssh fedora22`kertoo `SSH` arvoja, käytä asetukset-alueen, jossa `Host fedora22` Huomautus: Tämä on otsikko, joka on looginen käyttö ja vastaa todellinen isäntänimi minkä tahansa palvelimen.

`Hostname 102.160.203.241`= IP-osoite tai DNS palvelimen nimi.

`User myusername`= remote käyttäjätili käyttäminen kirjauduttaessa sisään palvelimeen.

`PubKeyAuthentication yes`= kertoo SSH haluat kirjautua SSH-näppäimen avulla.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= SSH yksityinen avain ja vastaavalla julkisella avaimella todennuksessa.


## <a name="ssh-into-linux-without-a-password"></a>SSH kyselyjä Linux ilman salasanaa

Nyt kun olet luonut avaimen SSH-pari ja määritetty SSH määritystiedosto, pystyt helposti ja turvallisesti Linux AM kirjautuminen. Ensimmäisen kerran kirjaudut palvelimeen käyttämällä SSH-näppäintä komennon ohjeita voit, että tärkeimmät tiedostolle salasana.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Komennon kuvaus

Kun `ssh fedora22` suoritetaan SSH ensin paikantaa ja lataa-asetuksia `Host fedora22` lohko ja lataa kaikki jäljellä olevat asetukset viimeisen estosta `Host *`.

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraava ylös on luotava Azure Linux VMs uusi SSH julkisen avaimen avulla.  Azure VMs, jotka on luotu SSH julkisella avaimella kuin kirjautuminen ovat paremmin suojattuja kuin VMs luotu kirjautuminen oletustapa, salasanat.  Azure VMs luotu SSH-näppäimellä on oletusarvoisesti määritetty salasanojen käytöstä, välttäminen kannalta pakotettu arvata yritykset.

- [Luo suojatun Linux-AM Azure mallin avulla](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Luo suojatun Linux-AM Azure-portaalissa](virtual-machines-linux-quick-create-portal.md)
- [Luo suojatun Linux-AM Azure-CLI käyttäminen](virtual-machines-linux-quick-create-cli.md)
