<properties
   pageTitle="Palvelun kangasta klustereiden käyttämällä CLI käyttäminen | Microsoft Azure"
   description="Miten käsitellä palvelun kangasta klusterin Azure CLI avulla"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Azure-CLI avulla voit käsitellä palvelun kangasta klusterin

Voit käsitellä palvelun kangasta klusterin Azure-CLI käyttäminen Linux Linux-tietokoneissa.

Ensimmäiseksi on get-git puhelukeskuksessa CLI uusimman version ja määrittää sen polusta käyttämällä seuraavia komentoja:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

Jokaisen komennon se tukee, voit kirjoittaa saaminen komennolle komennon nimi. Automaattinen täydentäminen tue komentoja. Esimerkiksi seuraava komento Avaa ohjeen sovelluksen komennot. 

```sh
 azure servicefabric application 
```

Voit suodattaa ohjetta tietyn komennon, kuten seuraavassa esimerkissä:

```sh
 azure servicefabric application  create
```

Voit ottaa käyttöön automaattisen-valmistumaan CLI, suorita seuraavat komennot:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Seuraavat komennot muodostaa yhteyttä klusterin ja Näytä klusterin solmut:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Nimetyt parametrit ja etsi ne oikein, voit kirjoittaa--Ohje-komennon jälkeen. Esimerkki:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Kun muodostat usean kuormitusryhmälle klusterin machine **ei kuulu klusterin**, kirjoita seuraava komento:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Korvaa PublicIPorFQDN tunnisteen reaali IP- tai FQDN tarpeen mukaan. Kun muodostat usean kuormitusryhmälle klusterin tietokoneen **, joka on osa klusterin**, kirjoita seuraava komento:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Voit käyttää PowerShell tai Linux Service-kangasta-klusterin vuorovaikutuksessa CLI luotu palvelun Azure-portaalissa. 

**Varoitus:** Näiden klustereiden ei ole suojattu, siis voit saattaa voidaan avaaminen yksi-ruutu lisätään julkiseen IP-osoitteeseen klusterin luettelo.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Yhteyden muodostaminen palvelun kangasta klusterin Azure-CLI avulla

Azure CLI seuraavista komennoista kerrotaan, kuinka voit muodostaa yhteyden suojatun klusterin. Varmenteen tiedot on oltava samat klusterin solmuissa varmenne.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Sertifikaatin on varmenteiden myöntäjistä (CA), jos haluat lisätä seuraavan esimerkin--ca varmenne-path-parametrin: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Jos sinulla on useita CAs, käytä pilkkua erottimena.
 
Jos sertifikaatin yleisiä nimesi vastaa yhteyden endpoint, voit käyttää parametrin `--strict-ssl` ohittamaan vahvistus, kuten seuraava komento: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Jos haluat ohittaa CA vahvistus, voit lisätä--hylkää-ei valtuuksia parametrin kuten seuraava komento: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Kun olet yhdistänyt, sinun pitäisi suorittaa ottaa yhteyttä klusterin CLI muut komennot. 

## <a name="deploying-your-service-fabric-application"></a>Palvelun kangasta sovelluksen käyttöönotto

Suorita seuraavat komennot kopioiminen, Rekisteröi ja Käynnistä kangasta palvelusovelluksen:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Sovelluksen päivittäminen

Prosessi on samanlainen kuin [Windowsin prosessi](service-fabric-application-upgrade-tutorial-powershell.md)).

Muodosta, kopioida, Rekisteröi ja luoda sovelluksen projektin pääkansiosta. Jos sovellusesiintymän nimi on kangasta: / MySFApp ja laji on MySFApp-komennot olisi seuraavasti:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Muutoksen sovelluksen ja muodostaa muokattu palvelu.  Päivitä muokattu palvelun luettelon tiedosto (ServiceManifest.xml) päivitettyjen versioiden Service (ja koodin tai Config tai tietoja tarpeen mukaan). Päivitetyn version numeron sovelluksen ja muokattu palvelun myös muokata sovelluksen luettelo (ApplicationManifest.xml).  Nyt kopioi ja rekisteröi päivitetty sovellus käyttämällä seuraavia komentoja:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Voit nyt aloittaa sovelluksen päivitys seuraavalla komennolla:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Nyt voit valvoa käyttämällä SFX sovelluksen päivitys. Muutaman minuutin kuluttua-sovelluksen on päivitetty.  Voit kokeilla päivitetyn-sovelluksen ja järjestelmä antaa virheilmoituksen ja tarkista palvelun kangasta automaattisen palautuksen toimintoja.

## <a name="troubleshooting"></a>Vianmääritys

### <a name="copying-of-the-application-package-does-not-succeed"></a>Kopiointi sovelluspaketin ei onnistu

Tarkista, onko `openssh` on asennettu. Oletusarvon mukaan Ubuntu työpöytää ei ole se. Asenna se käyttämällä seuraava komento:

```
 sudo apt-get install openssh-server openssh-client**
```

Jos ongelma jatkuu, kokeile käytöstä PAM varten ssh muuttamalla **sshd_config** tiedoston käyttämällä jotakin seuraavista komennoista:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Jos ongelma ei edelleenkään poistu, kokeile lisätä määrän ssh istuntojen suorittamalla seuraavat komennot:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Käytön näppäimet ssh todennus (eikä salasanat) ei vielä tueta (koska ympäristö ssh kopioi pakettien), käytä niin salasanan todennusta sijaan.


## <a name="next-steps"></a>Seuraavat vaiheet

Kehittäminen ympäristön määrittäminen ja ota käyttöön palvelun kangasta-sovelluksen Linux-klusteriin.
