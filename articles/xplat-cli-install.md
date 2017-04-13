<properties
    pageTitle="Asenna Azure käyttöliittymä | Microsoft Azure"
    description="Asenna Azure käyttöliittymä (CLI) Mac, Linux ja Windows Azure palveluiden käytön aloittaminen"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Asenna Azure CLI

> [AZURE.SELECTOR]
- [PowerShellin](powershell-install-configure.md)
- [Azure CLI](xplat-cli-install.md)

Asenna nopeasti Azure käyttöliittymä (Azure CLI) käyttämään Avaa lähde shell-pohjainen komentojen luomisen ja ylläpidon Microsoft Azure resurssit. Sinulla on useita vaihtoehtoja asentaminen tietokoneeseen Office kaikissa ympäristöissä työkaluja: 

* **npm paketti** - asenna uusin Azure CLI paketin Linux jakauman tai OS suorittamalla npm (JavaScript-, paketti-hallinta). Tarvitaan node.js ja npm tietokoneeseen.
* **Installer** - lataus on helppo asennuksen Mac-tai Windows installer.
* **Docker säilö** - uusimman CLI valmis Suorita Docker säilössä käytön aloittaminen. Edellyttää Docker host tietokoneeseen.
    
Asetukset ja tausta on artikkelissa project-tietovarasto [GitHub](https://github.com/azure/azure-xplat-cli). 

Kun Azure-CLI on asennettu, [Yhdistä se Azure-tilaus](xplat-cli-connect.md) ja **azure** -komentojen pitäminen komentorivivalitsimet käyttöliittymän (Bash, terminaalissa, komentokehote ja niin edelleen) Azure resurssien käyttöä varten.



## <a name="option-1-install-an-npm-package"></a>Vaihtoehto 1: Asenna npm-paketti

Asenna CLI npm-paketti, varmista, että olet ladannut ja asentanut [uusimman Node.js ja npm](https://nodejs.org/en/download/package-manager/). Suorita sitten Asenna azure cli paketti **npm asentaminen** : 

    npm install -g azure-cli

Valitse Linux jaot voit joutua muuttamaan **sudo** avulla voit suorittaa __npm__ -komento, seuraavasti:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Jos haluat asentaa tai päivittää Node.js ja npm Linux jakauman tai OS, suosittelemme, että asennat uusimman Node.js l. version (4.x). Jos käytössäsi on vanhempi versio, voit saada asennusvirheet. 

Voit halutessasi Lataa uusin Linux [tar tiedoston] [ linux-installer] npm paketin paikallisesti. Asenna ladatut npm-paketti (Valitse Linux jaot, haluat ehkä käyttää **sudo**) seuraavasti:

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Vaihtoehto 2: Käytä asennusohjelmaa

Jos käytät Mac- tai Windows-tietokoneessa, seuraavat CLI asennusohjelmia ovat ladattavissa:

* [Mac OS X installer][mac-installer]

* [Windowsin MSI][windows-installer] 

>[AZURE.TIP]Windows-voit ladata, asentaa CLI [WWW-ympäristö asennusohjelma](https://go.microsoft.com/?linkid=9828653) . Tämä asennusohjelma tutustutaan voi asentaa muita Azure SDK ja komentorivin Työkalut CLI asentamisen jälkeen. 


## <a name="option-3-use-a-docker-container"></a>Vaihtoehto 3: Käytä Docker säilö

Jos olet määrittänyt [Docker](https://docs.docker.com/engine/understanding-docker/) host tietokoneeseen, voit suorittaa uusimman Azure CLI Docker säilö. (Valitse Linux jaot, haluat ehkä käyttää **sudo**) varten suorittamalla seuraavan komennon:

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Suorita Azure CLI komennot
Kun Azure-CLI on asennettu, suorita **azure** -komento komentorivin käyttöliittymän (Bash, terminaalissa, komentokehote ja niin edelleen). Esimerkiksi voidaan suorittaa ohjekomento, kirjoita seuraava komento:

```
azure help
```
> [AZURE.NOTE]Jotkin Linux jaot, näyttöön saattaa tulla virhesanoma `/usr/bin/env: ‘node’: No such file or directory`. Tämä virhe tulee Node.js /usr/bin/nodejs on asennettu uusimmat asennuksia. Voit korjata tilanteen, Luo /usr/bin/node symbolinen linkki suorittamalla tämän komennon:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Saat Azure-CLI on asennettu versio, kirjoita seuraava komento:

```
azure --version
```

Kun olet valmis! Voit käyttää kaikkia CLI toimimaan oman resurssit, [Muodosta yhteys Azure-CLI Azure tilauksen](xplat-cli-connect.md)komentoja.

>[AZURE.NOTE] Kun käytät Azure CLI ensimmäistä kertaa, näyttöön tulee sanoma, jossa kysytään, jos haluat sallia Microsoft kerää käyttötiedot. Osallistuminen on vapaaehtoista. Jos haluat osallistua, voit estää milloin tahansa suorittamalla `azure telemetry --disable`. Voit milloin tahansa osallistumisen käyttöön suorittaa `azure telemetry --enable`.


## <a name="update-the-cli"></a>Päivitä CLI

Microsoft julkaisee usein päivitetyt Azure-CLI versiot. Asenna käyttämällä asennusohjelma käyttöjärjestelmäkohtaisia CLI tai suorittaa uusimman Docker säilö. Tai jos sinulla on uusimmat Node.js ja npm asennettuna, Päivitä kirjoittamalla seuraava (Valitse Linux jaot, haluat ehkä käyttää **sudo**).

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Ota käyttöön välilehden täyttäminen

Mac- ja Linux tuetaan CLI komentojen suorittamiseen välilehti.

Voit ottaa sen käyttöön zsh, suorittamalla:

```
echo '. <(azure --completion)' >> .zshrc
```

Ottamaan se käyttöön bash suorittaa:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Seuraavat vaiheet 

* [Yhdistä-CLI Azure-tilaukseen](xplat-cli-connect.md) voi luoda ja hallita Azure resurssit.

* Lisätietoja Azure-CLI Lataa lähdekoodi, raportoida ongelmista tai edistää projektin, siirry [Azure-CLI GitHub säilö](https://github.com/azure/azure-xplat-cli).

* Jos sinulla on kysyttävää Azure CLI tai Azure avulla, tutustu [Azure keskustelupalstoilla](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Jos haluat, voit kokeilla Python perustuva- [Azure CLI 2.0 esikatselu](https://github.com/azure/azure-cli).

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
