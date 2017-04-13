<properties
   pageTitle="Azure virtuaalikoneen-ympäristö, jonka kokki | Microsoft Azure"
   description="Lue, miten voit käyttää kokki automaattinen virtuaalikoneen käyttöönotto- ja Microsoft Azure-määritys"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Azure virtuaalikoneen-ympäristö, jonka kokki automatisointi

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Kokki on erinomainen työkalu välittää automaatio ja haluttu tilan määrityksiä.

Tutustu cloud api uusimmassa versiossa kokki mahdollistaa saumattomasti Azure-suojausvalintaikkuna valmistelu ja otetaan käyttöön määritysten hyötyä yhden komennon kautta.

Tässä artikkelissa voin esitellään kokki ympäristön voit valmistella Azuren näennäiskoneiden ja käydä läpi luomalla käytännön tai "Keittokirja" ja otat sitten käyttöön Azure virtuaalikoneen tämän keittokirja määrittämisestä.

Aloitetaan!

## <a name="chef-basics"></a>Kokki perusteet

Ennen kuin aloitat, voin ehdottaa tarkistat kokki peruskäsitteet. On hyvä materiaalin <a href="http://www.chef.io/chef" target="_blank">tähän</a> ja voin suosittelee sinulla on nopea luku, ennen kuin yrität tämän vaiheittaisen kuvauksen suorittamiseksi. Voin kuitenkin recap perusteet, ennen kuin olemme aloittaminen.

Seuraavassa kaaviossa on esitetty ylätason kokki arkkitehtuuri.

![][2]

Kokki on kolme pääosaa arkkitehtuuri: kokki Server, kokki asiakkaan (solmu) ja kokki työasema.

Kokki palvelin on Microsoftin hallintapisteen ja kokki palvelimen kahdella eri tavalla: isännöityä ratkaisu tai paikallisen ratkaisun. Esimerkissä käytössä isännöityä ratkaisu.

Kokki asiakkaan (solmu) on agentti, joka on avattujen hallinnoit palvelimiin.

Kokki työasema on Microsoftin järjestelmänvalvojan työaseman kohtaa, johon on Microsoftin käytännöt luominen ja hallinta Microsoftin komennot. Olemme Suorita kokki työaseman hallittavan Microsoftin infrastruktuurin **veitsi** -komento.

On myös käsite "Keittokirjat" ja "Reseptit". Nämä ovat tehokas käytännöt on määrittäminen ja käyttäminen Microsoftin palvelimiin.

## <a name="preparing-the-workstation"></a>Työaseman määrittäminen

Avulla ensin valmistelu työaseman. Käytän vakio Windows-työasemaan. Luo kansio tallentamiseen sekä config-tiedostoihin ja keittokirjat annettava.

Luo kansio nimeltä C:\chef.

Luo toinen kansio nimeltä c:\chef\cookbooks.

Nyt annettava Lataa Microsoftin Azure asetustiedosto, jotta kokki voivat viestiä ja tutustu Azure-tilaus.

Lataa oman Julkaisuasetukset [täältä](https://manage.windowsazure.com/publishsettings/).

Julkaise asetukset-tiedosto tallennetaan C:\chef.

##<a name="creating-a-managed-chef-account"></a>Hallitut kokki tilin luominen

Isännöityjen kokki-tilin rekisteröiminen [tähän](https://manage.chef.io/signup).

Kirjautuminen aikana sinua pyydetään luomaan uuden organisaation.

![][3]

Kun organisaatiossa on luotu, lataa starter kit.

![][4]

> [AZURE.NOTE] Jos näyttöön tulee kehote varoitus avaimien palautetaan, on jatketaan ei ole vielä määritetty olemassa olevan infrastruktuurin on ok.

Tämä starter kit zip-tiedosto sisältää organisaation config-tiedostoihin ja avaimet.

##<a name="configuring-the-chef-workstation"></a>Kokki työaseman määrittäminen

Pura kokki starter.zip, C:\chef sisällöstä.

Kopioi kaikki tiedostot-kohdassa kokki starter\chef-repo\.kokki c:\chef-kansioon.

Hakemiston pitäisi nyt näyttää suunnilleen seuraavassa esimerkissä.

![][5]

Sinulla on nyt neljä tiedostojen, kuten Azure julkaistava tiedosto c:\chef pääkansiossa.

PEM tiedostoissa on organisaation ja järjestelmänvalvojan yksityiset avaimet viestintään samalla, kun knife.rb-tiedosto sisältää veitsi-määritys. Annettava knife.rb-tiedoston muokkaaminen.

Avaa tiedosto-muokkausohjelmassa valinta ja Muokkaa "cookbook_path" poistamalla /... / polusta, jotta se tulee näkyviin, kuten seuraava.

    cookbook_path  ["#{current_dir}/cookbooks"]

Lisää myös seuraava rivi, mieti, että Azure nimi julkaista asetustiedosto.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Knife.rb tiedoston pitäisi nyt muistuttaa seuraavan esimerkin.

![][6]

Nämä rivit varmistaa veitsi viittaa c:\chef\cookbooks keittokirjat hakemiston ja käyttää myös Azure toimintojen aikana Microsoftin Azure Julkaisuasetukset-tiedostoa.

## <a name="installing-the-chef-development-kit"></a>Kokki Development Kit asentaminen

Seuraava [lataaminen ja asentaminen](http://downloads.getchef.com/chef-dk/windows) kokki työaseman määrittäminen ChefDK (kokki Development Kit).

![][7]

Asenna c:\opscode oletussijainti. Asennus kestää noin 10 minuuttia.

Vahvista LIIKERATA muuttuja sisältää merkinnät C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Jos ne eivät ole, varmista, että lisäät näitä polkuja.

*HUOMAUTUS JÄRJESTYKSEN POLKU ON TÄRKEÄÄ!* Jos opscode polut eivät ole oikeassa järjestyksessä on ongelmia.

Käynnistä työaseman uudelleen, ennen kuin jatkat.

Seuraavaksi on asennetaan veitsi Azure-tunniste. Tämä on veitsi "Azure laajennus" kanssa.

Suorita seuraava komento.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] – Pre-argumentin varmistaa vastaanotat RC uusimpaan versioon, josta pääsee ohjelmointirajapinnan uusimman joukko veitsi Azure-laajennus.

On todennäköistä, että useita riippuvuuksia myös asennetaan samanaikaisesti.

![][8]


Jotta kaikki tiedot on määritetty oikein, suorittamalla seuraavan komennon.

    knife azure image list

Jos kaikki tiedot on määritetty oikein, näet käytettävissä olevat Azure kuvat vierittämällä luettelo.

Onnittelen. Työaseman määrittäminen.

##<a name="creating-a-cookbook"></a>Keittokirja luominen

Keittokirja käytetään kokki määrittää komennot, jotka haluat suorittaa hallitun asiakkaalle. Keittokirja luominen on helppoa, ja olemme Luo meidän keittokirja malli **kokki Luo keittokirja** -komennon avulla. Voin oltava kutsumista keittokirja web-palvelimeen yhteenkuuluvien voin käytännön, joka ottaa automaattisesti käyttöön IIS.

Valitse C:\Chef hakemistossa varten suorittamalla seuraavan komennon.

    chef generate cookbook webserver

Tämä luo tiedostojoukon hakemiston C:\Chef\cookbooks\webserver. Nyt annettava määrittää haluamme kokki asiakasta suorittamaan Microsoftin hallitun virtuaalikoneen komentoja.

Komentojen on tallennettu tiedosto default.rb. Tässä tiedostossa voin määrittäminen joukon komentoja, jotka IIS-asennukset, IIS käynnistyy ja kopioi mallitiedosto wwwroot-kansiossa.

Muokkaa C:\chef\cookbooks\webserver\recipes\default.rb-tiedosto ja lisää seuraavat rivit.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Kun olet valmis, Tallenna tiedosto.

## <a name="creating-a-template"></a>Mallin luominen

Kuten on edellä mainittiin, annettava Luo mallitiedosto, jossa käytetään Microsoftin default.html sivuna.

Suorita seuraava komento luo malli.

    chef generate template webserver Default.htm

Siirry nyt C:\chef\cookbooks\webserver\templates\default\Default.htm.erb-tiedosto. Muokkaa tiedostoa lisäämällä joitakin yksinkertaisia "Hei maailma" HTML-koodin ja tallenna sitten tiedosto.



## <a name="upload-the-cookbook-to-the-chef-server"></a>Lataa opas kokki palvelimeen

Tässä vaiheessa syy on, että olet luonut Microsoftin paikallisessa tietokoneessa keittokirja kopiota ja lataaminen kokki isännöidyn palvelimeen. Kun ladattu palvelimeen, opas näkyvät **käytäntö** -välilehti.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Ottaa käyttöön virtual tietokoneessa, jossa veitsi Azure

Nyt on otetaan käyttöön Azure virtuaalikoneen ja käytä "Webserver"-opas, joka asentaa Microsoftin IIS-palvelun ja oletusarvon web-sivun.

Jotta voit tehdä tämän komennolla **veitsi azure palvelimen luominen** .

Olen Esimerkki komento näkyy seuraava.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Parametrit ovat selkeitä. Korvaa tietyn muuttujat ja suorita.

> [AZURE.NOTE] Kautta komentorivi-I 'M myös automatisointi päätepisteen verkon suodattimen säännöt – tcp päätepisteet parametrilla. Avatun portit 80 ja 3389 pääsy RDP-istunnon omien web-sivun ylöspäin.

Kun suoritat komennon, siirry Azure-portaaliin ja näet käyttämääsi laitteeseen, Aloita valmistelu.

![][13]

Komentorivi-ikkuna tulee näkyviin seuraava.

![][10]

Kun asennus on valmis, emme pitäisi yhdistäminen WWW-palvelun portin 80 kautta on ollut avata portti, kun on valmisteltu virtuaalikoneen veitsi Azure-komennolla. Virtual tämä tietokone on vain virtuaalikoneen Omat pilvipalvelussa, minulla yhteydessä cloud palvelun URL-osoite.

![][11]

Kuten näet, sain creative HTML-koodia sisältävien.

Älä unohda emme voi yhdistää RDP-istunnon kautta portti 3389 Azure perinteinen-portaalista kautta.

Voin Toivottavasti tästä on hyötyä. Siirry ja alkaa infrastruktuurin koodin matkan kanssa Azure tänään!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
