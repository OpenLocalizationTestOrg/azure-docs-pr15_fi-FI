<properties
   pageTitle="Azure virtuaalikoneen DotNet Core opetusohjelman 1 | Microsoft Azure"
   description="Azure virtuaalikoneen DotNet Core-opetusohjelma"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="automating-application-deployments-to-azure-virtual-machines"></a>Sovelluksen käyttöönotoissa, Azuren näennäiskoneiden automatisointi

Tämän nelivaiheisen sarjan tiedot ottaminen käyttöön ja määrittäminen Azure resurssi- ja sovellusten mallien Azure resurssien hallinta. Sarjassa malli-malli on otettu käyttöön ja käyttöönotto-mallin tarkastellaan. Sarjassa on kerro Azure resurssien välisen suhteen ja antaa kädet käyttöönottoa täysin integroitu Azure Resurssienhallinta-mallit. Tämän asiakirjan olettaa perustason tietämyksen Azure resurssien hallinta, ennen kuin aloitat Tässä opetusohjelmassa valintanauhaan tutustuminen peruskäsitteet Azure Resurssienhallinta.

## <a name="music-store-application"></a>Musiikin store-sovellus

Malli käyttää sarjassa on .net simuloimalla ostokset kokemus musiikkia säilön Core-sovellus. Tämä sovellus otetaan käyttöön virtual Linux- tai Windows-järjestelmään, otoksen ominaisuuksissa on luotu molemmat. Sovellus on web-sovelluksen ja SQL-tietokantaan. Ennen lukemalla artikkeleita samasta, ottaa käyttöön sovelluksen käyttöönotto-painike tämän sivun. Kun täysin käyttöön, sovelluksen / Azure-arkkitehtuuri näyttää seuraavassa kaaviossa. 

Musiikin kaupan Resurssienhallinta-mallin tässä luettelossa, [Musiikkia kaupan Linux malli]( https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db)

![Musiikin Store-sovellus](./media/virtual-machines-linux-dotnet-core/music-store.png)

Nämä osat associate mallin JSON tutkia neljä seuraavissa artikkeleissa.

- [**Sovelluksen arkkitehtuuri**](./virtual-machines-linux-dotnet-core-2-architecture.md) – sovelluksen osia, kuten sivustoista ja tietokantoja on sijaittava Azure tietokoneelta, kuten näennäiskoneiden ja Azure SQL-tietokannat. Tämän asiakirjan käydään läpi yhdistämisen suorittaminen tarve Azure resursseja ja otat käyttöön näiden resurssien Azure Resurssienhallinta-malli. 

- [**Käytön ja turvallisuuden**](./virtual-machines-linux-dotnet-core-3-access-security.md) – kun isännöinnin Azure-sovelluksia, on tarpeen ottaa huomioon, miten sovelluksen käytetään, ja miten eri sovellusten osien käytön toisiinsa. Tämän asiakirjan tiedot tarjoamalla ja suojaaminen internet-käyttöoikeudet-sovellukseen ja sovelluksen osien välillä.

- [**Käytettävyys ja skaalausta**](./virtual-machines-linux-dotnet-core-4-availability-scale.md) – käytettävyys ja skaalausta viittaavat sovellusten mahdollisuus pysy käytön aikana infrastruktuurin käyttökatkot ja voi skaalata Laske resurssien täyttävän sovelluksen tarvittaessa. Tämän tiedoston tietojen ottamaan kuormitus, tasataan tarvittavat komponentit ja hyvin käytettävissä oleva.

- [**Sovelluksen käyttöönotto**](./virtual-machines-linux-dotnet-core-5-app-deployment.md) - otettaessa sovelluksia Azuren näennäiskoneiden sivulle, jolla on asennettu sovelluksen binaaritiedostot virtuaalikoneen menetelmä on otettava huomioon. Tämän asiakirjan tiedot automatisointi sovelluksen asennuksen Azure virtuaalikoneen mukautettu komentosarja tunnisteet.

Kun kehittäminen Azure Resurssienhallinta malleja tavoitteena on automatisoida Azure infrastruktuuri ja asennuksesta ja määrityksestä minkä tahansa parhaillaan isännöimät tämän Azure infrastruktuurin sovellusten käyttöönoton. Nämä artikkelit – työskentelyä on esimerkki Tämä ongelma.

## <a name="deploy-the-music-store-application"></a>Musiikin store-sovellus

Musiikin Store-sovellus otetaan käyttöön tällä painikkeella.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-linux%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Azure Resurssienhallinta-malli edellyttää seuraavat parametriarvot.

|Parametrin nimi |Kuvaus   |
|---|---|
|SSHKEYDATA   | SSH suojataan virtuaalikoneen pääsy tärkeimmät tiedot. Lisätietoja avaimen SSH-pari luomisesta on artikkelissa [luominen SSH näppäimet Linux VMs Azure-tietokannassa](virtual-machines-linux-mac-create-ssh-keys.md).  |
|ADMINUSERNAME   | Järjestelmänvalvojan käyttäjänimeä, jota käytetään virtuaalikoneen ja Azure SQL-tietokanta.  |
|SQLADMINPASSWORD | Salasana, jota käytetään Azure SQL-tietokanta.  |
|NUMBEROFINSTANCES | Luoda näennäiskoneiden määrä. Kaikkien näiden näennäiskoneiden isännöidä musiikkia kaupan web-sovelluksen ja kaikki liikenne on kuormituksen saapuva ne. |
|PUBLICIPADDRESSDNSNAME | GUID julkiseen IP-osoite liittyvän DNS-nimen. |

Kun mallin käyttöönottoa on valmis, siirry julkiseen IP-osoite, minkä tahansa internet-selaimessa. .Net Core musiikkia sivuston esitetään.

## <a name="next-steps"></a>Seuraavat vaiheet

<hr>

[Vaihe 1: Application arkkitehtuuri Azure Resurssienhallinta malleja](./virtual-machines-linux-dotnet-core-2-architecture.md)

[Vaihe 2 – käytön ja turvallisuuden Azure Resurssienhallinta-mallit](./virtual-machines-linux-dotnet-core-3-access-security.md)

[Vaihe 3: saatavuudesta ja Azure Resurssienhallinta-mallit-asteikkoa](./virtual-machines-linux-dotnet-core-4-availability-scale.md)

[Vaihe 4 - sovellusten käyttöön Azure Resurssienhallinta-mallit](./virtual-machines-linux-dotnet-core-5-app-deployment.md)


