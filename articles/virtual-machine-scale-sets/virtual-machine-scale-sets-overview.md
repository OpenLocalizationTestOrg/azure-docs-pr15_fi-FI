<properties
    pageTitle="Virtuaalikoneen asteikko määrittää yleiskatsaus | Microsoft Azure"
    description="Lisätietoja virtuaalikoneen asteikko joukot"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

# <a name="virtual-machine-scale-sets-overview"></a>Virtuaalikoneen asteikko joukot yleiskatsaus

Virtuaalikoneen asteikko ovat Azure Laske resurssin, voit ottaa käyttöön ja hallita samanlaiset VMs joukko. Kaikki VMs määritetty samoja, AM asteikko joukot on suunniteltu tukemaan tosi Automaattinen skaalaus – ei ole valmiiksi valmistelu VMs tarvitaan – ja siten helpottaa luominen suurissa services kohdistamisen suuri Laske big datasta ja tai kontteihin pakattuja toiminnoista.

Sovellukset, jotka on skaalata vika ja Päivitä toimialueilla Laske resurssien ulos ja -toimintojen implisiittisesti saapuva asteikko. Johdanto AM näytöstä joukot viitata [Azure blogi-ilmoitus](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Tutustu lisätietoja AM asteikko määrittää nämä videot:

 - [Merkitse Russinovich puheen Azure asteikko joukot](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtuaalikoneen asteikko määrittää Guy Bowerman kanssa](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Määrittää luomisen ja ylläpidon AM asteikko

Voit luoda AM asteikko määrittäminen [Azure portal](https://portal.azure.com) valitsemalla _Uusi_ ja kirjoittamalla "mittakaava" etsintäpalkin. Näkyviin tulee "virtuaalikoneen asteikko määrittäminen" tuloksissa. Sieltä voit täyttää mukauttaminen ja otetaan käyttöön asteikko määrittäminen tarvittavat kentät. 

AM asteikko määrittää myös voidaan määrittää ja ottaa käyttöön käyttämällä JSON-mallit ja [REST API](https://msdn.microsoft.com/library/mt589023.aspx) juuri, kuten yksittäisiä Azure Resurssienhallinta VMs. Tämän vuoksi kaikki tavallisia Azure Resurssienhallinta käyttöönotto-menetelmiä voidaan. Saat lisätietoja malleista [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md).

Esimerkki malleja AM asteikko joukkojen joukko löytyy Azure pikaopas malleja GitHub säilössä [tähän.](https://github.com/Azure/azure-quickstart-templates) (Etsi mallien _vmss_ otsikko)

Näiden mallien lisätietosivut näet painike, joka linkittyy yritysportaalin käyttöönotto-ominaisuus. Ottamaan AM asteikko-joukko napsauttamalla-painiketta ja täytä parametreja, joita tarvitaan portaalissa. Jos et ole varma, onko resurssi tukee kirjaimiksi on turvallisempi Käytä aina pieniksi kirjaimiksi parametriarvot ylemmässä tai molempia sisältäväksi. On myös kätevä videon kuivumisen AM asteikko määrittäminen mallin tähän:

[AM asteikko määrittäminen mallin kuivumisen](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Määrittää skaalaus AM mittakaava, ja

Voit suurentaa tai pienentää näennäiskoneiden AM asteikko joukon määrä, riittää, että _kapasiteetti_ -ominaisuuden muuttaminen ja ota uudelleen malli. Tämä yksinkertaisuuden on helppo kirjoittaa omia mukautettuja skaalauksen kerroksen, jos haluat määrittää Mukautettu mittakaava tapahtumia, jotka eivät tue Azure Automaattinen skaalaus.

Jos ovat mallirakenteeseen muuttaa kapasiteetin mallia, voit määrittää paljon pienempi mallin joka sisältää vain olevat versiot ja päivitetyt kapasiteetti. Tässä on esimerkki [tähän.](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)

Käy läpi vaiheet, joka luo mittakaava, joka on skaalattu automaattisesti-kohdassa [Automaattisesti asteikko koneet virtuaalikoneen-asteikko-joukko](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>Määritä valvonta AM asteikko

[Azure portal](https://portal.azure.com) luetteloiden asteikko joukot ja näyttää perusominaisuudet sekä luettelo VMs, valitse Määritä. Lisätietoja tarkastella AM asteikko joukot [Azure resurssin Explorerin](https://resources.azure.com) avulla. AM asteikko joukot ovat Microsoft.Compute, valitse resurssi, joten tästä sivustosta voit tarkastella niitä laajentamalla on seuraavissa linkeissä:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>AM asteikko määrittäminen skenaariot

Tässä osassa on lueteltu joitakin tyypillinen AM asteikko määrittäminen skenaarioita. Jotkin suurempi tason Azure palveluja (kuten erän, palvelun kangasta ja Azure säilö Service) käyttää tilanteista.

 - **RDP / SSH AM näytöstä määrittäminen esiintymät** - A AM asteikko joukko luodaan sisällä VNET ja yksittäisiä VMs asteikko-määrittäminen ei varata julkiseen IP-osoitteet. Tämä on hyvä asian, koska yleensä piilotettavien jakamiseksi erillisessä julkisten IP-osoitteiden Laske ruudukon tilattomien resurssien kustannukset ja hallinta-katseltavan, ja voit helposti muodostaa yhteyden nämä VMs muista resursseista oman VNET myös julkiseen IP-osoitteet, kuten kuormituksen tasoitusmääritykset tai erillisen näennäiskoneiden on.

 - **Yhteyden muodostaminen NAT sääntöjen avulla VMs** - voit luoda julkisen IP-osoite, liittäminen kuormituksen ja määrittää saapuvan NAT säännöt, jotka IP-osoite porttiin yhdistäminen porttiin AM AM asteikko määrittäminen. Esimerkki:
 
    Lähde | Lähdeportti | Kohde | Kohdeportti
    --- | --- | --- | ---
    Julkiseen IP | Portti 50000 | vmss\_0 | Portti 22
    Julkiseen IP | Portti 50001 | vmss\_1 | Portti 22
    Julkiseen IP | Portti 50002 | vmss\_2 | Portti 22

    Esimerkki, joka käyttää NAT sääntöjä, joka määrittää yksittäisen julkiseen IP käyttämällä mittakaava AM SSH yhteyden ottaminen käyttöön AM asteikko-joukon luominen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Tässä on esimerkki suorittaa saman RDP-ja Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Yhteyden muodostaminen käyttämällä "jumpbox" VMs** - Jos luot AM asteikolla määrittäminen ja yksittäisen AM saman VNET, erillinen AM ja AM asteikko joukon VMs muodostaa keskenään käyttämällä sisäinen IP-osoitteet VNET/aliverkon määrittämä. Jos Luo julkinen IP-osoite ja määritä se erilliseksi AM voit RDP tai SSH erilliseksi AM ja liitä sitten AM asteikko määrittäminen kopioita tietokoneesta. Saatat huomata tässä vaiheessa joukko yksinkertainen AM-asteikko on salliminen turvallisempi kuin yksinkertainen yksittäisen AM sen alkuperäisessä kokoonpanossa julkiseen IP-osoite.

    [Esimerkki tämän menetelmän Tämä malli luo yksinkertaisia Mesos-klusterin yksittäisen perustyyli AM, joka hallitsee VMs AM asteikko-joukon mukaan klusterin koostuvat.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **Kuormituksen tasaamisen AM näytöstä määrittäminen esiintymät** – Jos haluat pitää työn laskentaklusteriin VMs käyttämällä "pyöreän pöydän"-vaihtoehto on, voit määrittää Azure kuormituksen kuormituksen tasaamisen sääntöjen vastaavasti. Voit määrittää keräysputkien vahvistamiseksi sovelluksen suorittamalla ping portit määritetyn protokolla, aikaväli ja pyynnön polulla. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) tukee myös asteikko joukot sekä kehittyneempiä kuormituksen skenaarioita.

    [Tässä on esimerkki, joka luo AM asteikko joukko virtuaalilaitteiksi IIS-verkkopalvelin ja käyttää kuormituksen tasauspalvelun kuormituksen, joka vastaanottaa kunkin AM. Se myös käyttää HTTP-protokolla ping jokaiselle AM tietyn URL-Osoitteesta.](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) (Katso Microsoft.Network/loadBalancers resurssin laji ja networkProfile ja virtualMachineScaleSet extensionProfile)

 - **AM asteikolla määritelty laskentaklusteriin PaaS-klusterin hallinnan käyttöönotto** - joukot joskus kuvataan seuraavan sukupolven Työntekijä-roolin AM asteikko. Se on kelvollinen kuvaus mutta se myös toimii sekava asteikko riskiä ominaisuuksien määrittäminen PaaS v1 työntekijän rooli ominaisuuksilla. Tasolla AM asteikko joukot antaa tosi "Työntekijä rooli" tai työntekijä resurssin siten, että generalized Laske resurssin ympäristö/runtime riippumaton, eli niissä voi mukauttaa ja integroi Azure Resurssienhallinta IaaS osaksi.

    PaaS v1 työntekijän rooli, kun rajoitettu kannalta ympäristö/runtime-tuki (vain Windows platform kuvien) sisältää myös palvelut, kuten VIP swap, määritettäviä päivitysasetukset, runtime/sovelluksen käyttöönotto tiettyjä asetuksia, jotka eivät ole _vielä_ käytettävissä AM skaalata joukot tai muut suurempi tason PaaS palvelut, kuten palvelun kangasta toimitetaan. Tässä yhteydessä muista, että voi tarkastella AM asteikko joukot infrastruktuuria, joka tukee PaaS nimellä. Toisin sanoen PaaS ratkaisuja, kuten palvelun kangasta tai klusterin valvojat kuin Mesos kehittää päälle AM skaalata joukot skaalattava Laske kerrokseen.

    [Esimerkki tämän menetelmän Tämä malli luo yksinkertaisia Mesos-klusterin yksittäisen perustyyli AM, joka hallitsee VMs AM asteikko-joukon mukaan klusterin koostuvat.](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json) [Azure säilö palvelun](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) tulevissa versioissa otetaan käyttöön lisää kompleksiluvun/karkaistu versioissa tässä skenaariossa AM asteikko joukot perusteella.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>AM asteikko määrittäminen suorituskyky ja skaalattavuus ohjeet

- Yli 500 VMs ei luoda useita AM asteikko joukoissa kerrallaan.
- Enintään 20 VMs tiliä ja tallennustilaa suunnitteleminen (paitsi jos asetat _overprovision_ -ominaisuuden arvoksi "false", jolloin voit siirtyä jopa 40).
- Levittäminen tallennustilan tilien nimet mahdollisimman paljon ensimmäinen kirjain.  Esimerkki VMSS mallit [Azure pikaopas malleja](https://github.com/Azure/azure-quickstart-templates/) on esimerkkejä tähän.
- Jos käytät mukautettua VMs, suunnitteleminen enintään 40 VMs AM asteikko määrittäminen yksittäisen tallennustilaa tilillä kohden.  Sinun on valmiiksi kopioitu tallennustilan huomioon ennen aloittamista AM asteikko määrittäminen käyttöönoton kuva. Katso lisätietoja usein kysytyistä Kysymyksistä.
- Suunnittele enintään 4096 VMs VNET kohden.
- Voit luoda VMs määrä on rajoitettu alueen, jossa käyttöönoton core kiintiöön. Voit joutua yhteystiedot niin, että oman Laske kiintiö entistä parempi, vaikka sinulla olisi sydämiä käytettäviksi pilvipalveluihin tai IaaS v1 suuri enimmäismäärä tänään asiakastukeen. Kyselyn kiintiön voit suorittaa seuraavat Azure CLI-komennon: `azure vm list-usage`, ja PowerShell-komentoa: `Get-AzureRmVMUsage` (Jos PowerShell versio 1.0 Käytä alla `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>AM asteikko määrittäminen usein kysytyt kysymykset

**Q.** Kuinka monta VMs sinun on AM asteikko joukon?

**A.** 100, jos käytät platform kuvia, jotka voidaan jakaa tallennustilan useiden eri tilien välillä. Jos käytössäsi on mukautettu kuvia, enintään 40 (Jos _overprovision_ -ominaisuudeksi on määritetty oletusarvoisesti "false"-20), mukautettujen kuvat ovat tällä hetkellä vain yhden tallennustilan tilin.

**Q** mitä resurssien rajoitukset olemassa AM asteikko joukkojen?

**A.** Sinun on oikeutettu enintään 500 VMs luominen useita asteikko-joukot alueittain 10 minuutin ajan. Nykyisen [Azure Tilauspalvelussa rajoitukset /](../azure-subscription-service-limits.md) käyttää.

**Q.** Kuuluvat AM asteikko joukot tietoja levyjen tueta?

**A.** Muiden kuin alkuperäinen versio. Tietojen tallentaminen ovat:

- Azure-tiedostot (jaettu SMB asemat)

- OS asema

- Temp-asema (paikallinen, ei palautettua Azure tallennustilan)

- Tietojen Azure-palvelu (esimerkiksi Azure taulukot, Azure BLOB)

- Ulkoiset tiedot-palvelun (kuten remote DB)

**Q.** Mitä Azure alueiden tukee AM asteikko määrittää?

**A.** Alue, joka tukee Azure Resurssienhallinta tukee AM asteikko joukot.

**Q.** Miten voit luoda AM mittakaava määrittää käyttämällä mukautetun kuvan?

**A.** Jätä vhdContainers-ominaisuuden tyhjäksi, esimerkiksi:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": "https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd"
            },
            "osType": "Windows"
        }
    },


**Q.** Jos pienentäminen AM-asteikko määrittää kapasiteetin 20 15, jossa VMs poistetaan?

**A.** Näennäiskoneiden poistetaan määritetty tasaisesti päivityksen toimialueet ja vika toimialueiden Suurenna käytettävyys asteikko. Suurin tunnuksella VMs poistetaan ensin.

**Q.** Miten se Jos 18 from 15 resursseja on Suurenna sitten?

**A.** Jos kasvatat 18 kapasiteettia, 3 uusi VMs luodaan. Aina, kun kasvaa AM tunniste edellisen suurin arvo (kuten 20, 21, 22). VMs ovat saapuva FDs ja UDs.

**Q.** Käytettäessä useita tiedostotunnisteita AM asteikko joukon voit pakottaa suorittamisen sarjan?

**A.** Ei suoraan, mutta customScript-tunnisteeseen komentosarja voi odottaa toisen tunniste ([esimerkiksi seuraamalla tunniste-loki](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)) suorittamiseen. Lisäohjeita-tunniste järjestyksen löytyvät on blogikirjoituksessa: [Tunniste järjestyksen Azure AM asteikko joukoissa](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**Q.** AM asteikko joukot suoritetaan Azure käytettävyys joukot?

**A.** Kyllä. AM asteikko on määrityksessä 5 FDs ja 5 UDs implisiittinen saatavuus. Ei tarvitse määrittää mitään virtualMachineProfile-kohdassa. Tulevissa versioissa AM asteikko joukot todennäköisesti useita alihallinnat, jotka ulottuvat mutta nyt asteikolla määrittäminen on yksi käytettävyys määrittäminen.
