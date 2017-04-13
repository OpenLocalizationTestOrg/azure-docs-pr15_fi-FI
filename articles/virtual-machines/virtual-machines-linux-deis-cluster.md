<properties
   pageTitle="Ottaa käyttöön 3-solmu Deis klusterin | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten Luo 3-solmu Deis klusterin-Azure Azure Resurssienhallinta-mallin avulla"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>Ottaa käyttöön 3-solmu Deis klusteri

Tässä artikkelissa käydään läpi valmistelu [Deis](http://deis.io/) Azure-klusterin. Se kattaa kaikki tarvittavat varmenteet ja skaalausta otoksen **Siirry** sovelluksen juuri valmistellun klusterin luomasta vaiheet.

Seuraavassa kaaviossa on otettu käyttöön järjestelmän arkkitehtuuri. Järjestelmänvalvoja hallitsee klusterin käyttämällä Deis työkaluilla, kuten **deis** ja **deisctl**. Yhteydet on luotu Azure kuormituksen, joka välittää yhteydet johonkin jäsenen solmujen klusterin kautta. Asiakkaat-käytön käyttöön kuormituksen sekä sovelluksia. Tässä tapauksessa kuormituksen välittää tietoliikennettä Deis reitittimen verkko, jossa routs edelleen liikenne vastaavan Docker säiliöiden isännöimät klusterin.

  ![Arkkitehtuuri kaavion otettu käyttöön Desis klusterin](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Jotta voit suorittaa seuraavat vaiheet, sinun on:

 * Aktiivinen Azure-tilaus. Jos sinulla ei ole, saat ilmaisen [azure.com](https://azure.microsoft.com/).
 * Työpaikan tai oppilaitoksen tunnusten käyttöasetusten Azure resurssiryhmät. Jos sinulla on henkilökohtainen tili ja kirjaudu sisään Microsoft-tunnuksen, haluat [luoda oman väristä työn tunnuksen](virtual-machines-windows-create-aad-work-id.md).
 * Joko--asiakkaan käyttöjärjestelmästä riippuen-- [PowerShellin Azure](../powershell-install-configure.md) - tai [Mac, Linux-ja Windows Azure CLI](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). Luo tarvittavat varmenteet käytetään OpenSSL.
 * Git-asiakas, kuten [Git Bash](https://git-scm.com/).
 * Voit testata sovelluksen malli, sinun on myös DNS-palvelimeen. Voit käyttää mitä tahansa DNS-palvelimia tai palvelut, jotka tukevat yleismerkkien A tietueita.
 * Tietokoneen suorittamaan Deis asiakastyökalut. Voit käyttää joko paikallisessa tietokoneessa tai virtual machine. Voit suorittaa melkein mitä tahansa Linux-jakauman työkaluja, mutta seuraavien ohjeiden avulla Ubuntu.

## <a name="provision-the-cluster"></a>Klusterin valmistelu

Tässä osassa käytät [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) -malli Avaa lähde säilöön [azure-pikaopas-mallit](https://github.com/Azure/azure-quickstart-templates). Ensimmäisen kerran voit kopioida mallin alaspäin. Valitse sitten Luo uusi SSH avaimen lainausmerkeiksi todennusta varten. Ja valitse sitten määrittäminen voit klusterin uusi tunnus. Ja lopuksi tulee käyttää klusteri valmisteleminen liittymän komentosarja tai PowerShell-komentosarjaa.

1. Kloonaa säilö: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Siirry mallit-kansioon:

        cd azure-quickstart-templates\deis-cluster-coreos

3. Luo uusi SSH avaimen pari ssh-keygen avulla:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. Luo varmennetta käyttämällä yllä Yksityinen avain:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Siirry [https://discovery.etcd.io/new](https://discovery.etcd.io/new) voit luoda uuden klusterin-tunnuksen, mitkä näyttää tähän tapaan:

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Kunkin CoreOS klusterin on oltava yksilöllinen tunnus tämän vapaa-palvelusta. Katso lisätietoja [CoreOS ohjeissa](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) .

6. Korvaa aiemmin luotu **etsiminen** tunnuksen uuden tunnuksen **cloud config.yaml** -tiedoston muokkaaminen:

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Muokkaa tiedostoa **azuredeploy parameters.json** : Avaa tekstieditorissa vaiheessa 4 luomasi sertifikaatti. Kopioi kaikki tekstiä `----BEGIN CERTIFICATE-----` ja `-----END CERTIFICATE-----` yhdeksi **sshKeyData** parametrin (sinun on poistettava kaikki rivinvaihtomerkit).

8. Muokkaa **newStorageAccountName** -parametria. Tämä on AM OS levyjen tallennustilan-tili. Tämän tilin nimen on oltava yksilöivä.

9. Muokkaa **publicDomainName** -parametria. Tämä muuttuu kuormituksen tasauspalvelun julkiseen IP liittyvän DNS-nimeen. Lopullinen FQDN on _parametrin arvon_muoto. _[alue]_. cloudapp.azure.com. Esimerkiksi jos määrität nimi deishbai32 ja resurssiryhmän otetaan käyttöön Länsi US-alue, valitse lopullinen FQDN, että kuormituksen on deishbai32.westus.cloudapp.azure.com.

10. Tallenna tiedosto parametria. Ja sitten voit valmistella klusterin Azure PowerShellin avulla:

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  tai Azure CLI:

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Kun resurssiryhmän on valmisteltu, näet kaikki resurssit ryhmästä Azure perinteinen portaalissa. Seuraavassa näyttökuvassa esitetyllä Resurssiryhmä sisältää virtual verkossa, jossa on kolme VMs, jotka ovat liittyneet käytettävyys samat. Ryhmän sisältää myös kuormituksen, johon on liitetty julkiseen IP.

  ![Valmistellun resurssiryhmä Azure perinteinen-portaalissa](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Asiakasohjelman asentaminen

Tarvitset **deisctl** ohjausobjektiin yhteyttä klusterin Deis. Mutta deisctl asennetaan automaattisesti kaikki klusterisolmut, se on hyvä käyttää deisctl erillinen järjestelmänvalvojan tietokoneeseen. Lisäksi, koska kaikki solmut määritetään vain yksityinen IP-osoitteet, tarvitset käyttämään SSH kuormituksen, jonka on julkiseen IP-solmu koneet muodostaa kautta. Seuraavassa on koskevan deisctl käyttöön erillinen Ubuntu, fyysinen tai virtuaalikoneen määrittämisestä.

1. Asenna deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. Lisää yksityinen avain ssh agentti:

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Määritä deisctl:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

Mallin määrittää saapuvan NAT-säännöt, jotka vastaavat 2223 esiintymän 1, 2224 esiintymän 2 ja 3-esiintymässä 2225. Tämä on redundancy deisctl-työkalun avulla. Voit nyt sääntöjen Azure perinteinen portaalissa:

![Kuormituksen NAT säännöt](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] Malli tukee tällä hetkellä vain 3-solmu klustereiden. Tämä on Azure Resurssienhallinta-mallissa NAT säännön määritys, joka ei tue silmukan syntaksi rajoitukset.

## <a name="install-and-start-the-deis-platform"></a>Asenna ja Käynnistä Deis ympäristö

Nyt voit käyttää deisctl ja asentaa Deis ympäristö:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Aloitetaan platform kestää jonkin aikaa (jopa 10 minuuttia). Erityisesti muodostin-palvelun käynnistäminen voi kestää kauan. Ja joskus rekisteröijän paria yritystä onnistuu: Jos ja ripustaa toiminnon, yritä kirjoittaa `ctrl+c` voit poistaa komennon suorittamisen ja yritä uudelleen.

Voit käyttää `deisctl list` vahvistamiseksi, jos kaikki palvelut ovat käynnissä:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Onnittelen! Nyt koneessasi on käynnissä Deis clsuter Azure! Seuraavaksi käyttöön japanin otoksen Siirry sovelluksen Nähdäksesi klusterin käytössä.

## <a name="deploy-and-scale-a-hello-world-application"></a>Ottaa käyttöön ja Skaalaa Hei maailma-sovellus

Seuraavissa vaiheissa kuvataan ottamisesta käyttöön "Hei maailma" Siirry klusterin sovelluksen. Ohjeita perustuvat [Deis ohjeissa](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Saat reititys verkon toimii oikein tarvitset toimialueen osoittamista kuormituksen julkiseen IP yleismerkkien A-tietueita. Seuraavassa näyttökuvassa näkyy esimerkki toimialueen rekisteröinti A-tietuetta GoDaddy:

    ![Godaddy-A-tietue](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Asenna deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Luo uuden SSH tunnuksen ja lisää sitten julkinen avain GitHub (, voit myös käyttää aiemmin luotuja näppäimet). Voit luoda uuden SSH avaimen pari käyttämällä:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Lisää GitHub id_rsa.pub tai valintasi julkinen avain. Voit tehdä tämän käyttämällä Lisää SSH avaimen painike näytön määritysten SSH näppäimet:

  ![Github avain](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Rekisteröi uusi käyttäjä:

        deis register http://deis.[your domain]
<p />
6. Lisää SSH-näppäintä:

        deis keys:add [path to your SSH public key]
  <p />      
7. Luo sovelluksen.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.Git push käynnistää Docker kuvia sisäisten ja otettu käyttöön, joka kestää muutaman minuutin kuluttua. Käyttökokemus, valitse toisinaan vaihe 10 (Pushing kuva yksityinen säilöön) voivat katkaista. Tällöin voit Lopeta prosessi, poista sovellus käyttää ' deis sovellukset: tuhoa – <application name> ` to remove the application and try again. You can use `deis apps:list "Saat lisätietoja sovelluksen nimeä. Jos kaikki toimii, esimerkiksi seuraava komento tulostaa lopussa pitäisi näkyä seuraavat:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Tarkista, toimiiko sovellus:

        curl -S http://[your application name].[your domain]
  Näkyviin tulee:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. Skaalaa 3 esiintymät sovelluksen:

        deis scale cmd=3
<p />
11. Vaihtoehtoisesti voit tutkia tietoja sovelluksen tiedot deis. Seuraavat tulostaa ovat sovelluksen-asennus:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa walked valmistelu uuden vaiheiden läpi Deis Azure Resurssienhallinta-mallin avulla Azure-klusterin. Malli tukee redundancy tooling yhteydet sekä kuormituksen käyttöön sovellusten hallinta. Malliin voidaan myös käyttämällä julkiseen IP-osoitteet jäsenen solmuissa, joka tallentaa arvokkaita julkiseen IP-resurssit ja tarjoaa enemmän suojatun ympäristön Host (isäntä)-sovelluksia. Lisätietoja on seuraavissa artikkeleissa:

[Azure resurssin hallinnassa: yleiskatsaus] [resource-group-overview]  
[Azure-CLI käyttäminen] [azure-command-line-tools]  
[Azure PowerShell kanssa Azure resurssien hallinnan avulla] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md
