

Sovellukset, jotka on skaalata vika ja Päivitä toimialueilla Laske resurssien ulos ja -toimintojen implisiittisesti saapuva asteikko. Johdanto AM näytöstä joukot viitata viimeisimmät [Azure blogi-ilmoitus](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/).

Tutustu lisätietoja AM asteikko määrittää nämä videot:

 - [Merkitse Russinovich puheen Azure asteikko joukot](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [Virtuaalikoneen asteikko määrittää Guy Bowerman kanssa](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Määrittää luomisen ja ylläpidon AM asteikko

AM asteikko joukot voidaan määrittää ja ottaa käyttöön käyttämällä JSON-mallit ja [REST API](https://msdn.microsoft.com/library/mt589023.aspx) juuri, kuten yksittäisiä Azure Resurssienhallinta VMs. Tämän vuoksi kaikki tavallisia Azure Resurssienhallinta käyttöönotto-menetelmiä voidaan. Saat lisätietoja malleista [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../articles/resource-group-authoring-templates.md).

Esimerkki malleja AM asteikko joukkojen joukko löytyy Azure pikaopas malleja GitHub säilössä tähän:

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) - mallien _vmss_ otsikko ulkoasun.

Näiden mallien lisätietosivut näet painike, joka linkittyy yritysportaalin käyttöönotto-ominaisuus. Ottamaan AM asteikko-joukko napsauttamalla-painiketta ja täytä parametreja, joita tarvitaan portaalissa. Jos et ole varma, onko resurssi tukee kirjaimiksi on turvallisempi Käytä aina pieniksi kirjaimiksi parametriarvot ylemmässä tai molempia sisältäväksi. On myös kätevä videon kuivumisen AM asteikko määrittäminen mallin tähän:

[AM asteikko määrittäminen mallin kuivumisen](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Määrittää skaalaus AM mittakaava, ja

Voit suurentaa tai pienentää näennäiskoneiden AM asteikko joukon määrä, riittää, että _kapasiteetti_ -ominaisuuden muuttaminen ja ota uudelleen malli. Tämä yksinkertaisuuden on helppo kirjoittaa omia mukautettuja skaalauksen kerroksen, jos haluat määrittää Mukautettu mittakaava tapahtumia, jotka eivät tue Azure Automaattinen skaalaus.

Jos ovat mallirakenteeseen muuttaa kapasiteetin mallia, voit määrittää paljon pienempi mallin joka sisältää vain olevat versiot ja päivitetyt kapasiteetti. Tämä esimerkki on mukana: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json).

Käy läpi vaiheet, joka luo mittakaava, joka on skaalattu automaattisesti-kohdassa [Automaattisesti asteikko koneet virtuaalikoneen-asteikko-joukko](../articles/virtual-machines/virtual-machines-windows-vmss-powershell-creating.md)

## <a name="monitoring-your-vm-scale-set"></a>Määritä valvonta AM asteikko

Tällä hetkellä suositellaan [Azure resurssin Explorer](https://resources.azure.com) avulla voit tarkastella AM asteikko joukot. AM asteikko joukot ovat Microsoft.Compute, valitse resurssi, joten tästä sivustosta voit tarkastella niitä laajentamalla on seuraavissa linkeissä:

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## <a name="vm-scale-set-scenarios"></a>AM asteikko määrittäminen skenaariot

Tässä osassa on lueteltu joitakin tyypillinen AM asteikko määrittäminen skenaarioita. Jotkin suurempi tason Azure palveluja (kuten erän, palvelun kangasta ja Azure säilö Service) käyttää tilanteista.

 - **RDP / SSH AM näytöstä määrittäminen esiintymät** - A AM asteikko joukko luodaan sisällä VNET ja yksittäisiä VMs-ei varata julkiseen IP-osoitteet. Tämä on hyvä asian, koska yleensä piilotettavien jakamiseksi erillinen IP-osoitteiden Laske ruudukon tilattomien resurssien kustannukset ja hallinta-katseltavan, ja voit helposti muodostaa yhteyden nämä VMs muista resursseista oman VNET myös julkiseen IP-osoitteet, kuten kuormituksen tasoitusmääritykset tai erillisen näennäiskoneiden on.

 - **Yhteyden muodostaminen NAT sääntöjen avulla VMs** - voit luoda julkisen IP-osoite, liittäminen kuormituksen ja määrittää saapuvan NAT säännöt, jotka IP-osoite porttiin yhdistäminen porttiin AM AM asteikko määrittäminen. Esimerkiksi

    Julkiseen IP-portin 50000 > vmss ->\_0 > portin 22 julkiseen IP - > portin 50001 -> vmss\_1 > portin 22 julkiseen IP - > portin 50002 -> vmss\_2 -> portin 22

    Esimerkki, joka käyttää NAT sääntöjä, joka määrittää yksittäisen julkiseen IP käyttämällä mittakaava AM SSH yhteyden ottaminen käyttöön AM asteikko-joukon luominen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    Tässä on esimerkki suorittaa saman RDP-ja Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **Yhteyden muodostaminen käyttämällä "jumpbox" VMs** - Jos luot AM asteikolla määrittäminen ja yksittäisen AM saman VNET, erillinen AM ja AM asteikko joukon VMs muodostaa keskenään käyttämällä sisäinen IP-osoitteet VNET/aliverkon määrittämä. Jos Luo julkinen IP-osoite ja määritä se erilliseksi AM voit RDP tai SSH erilliseksi AM ja liitä sitten AM asteikko määrittäminen kopioita tietokoneesta. Saatat huomata tässä vaiheessa joukko yksinkertainen AM-asteikko on salliminen turvallisempi kuin yksinkertainen yksittäisen AM sen alkuperäisessä kokoonpanossa julkiseen IP-osoite.

    Esimerkki tämän menetelmän Tämä malli luo yksinkertaisia Mesos-klusterin yksittäisen perustyyli AM, joka hallitsee VMs AM asteikko-joukon mukaan klusterin koostuva: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **PYÖRISTÄ-funktiota Mikko kuormituksen tasaamisen AM näytöstä määrittäminen esiintymät** – Jos haluat pitää työn laskentaklusteriin VMs käyttämällä "pyöreän pöydän"-vaihtoehto on, voit määrittää Azure kuormituksen kanssa kuormituksen tasaamisen säännöt vastaavasti. Voit myös määrittää keräysputkien vahvistamiseksi sovelluksen suorittamalla ping portit määritetyn protokolla, aikaväli ja pyynnön polulla.

    Tässä on esimerkki, joka luo AM asteikko joukko virtuaalilaitteiksi IIS-verkkopalvelin ja käyttää kuormituksen tasauspalvelun kuormituksen, joka vastaanottaa kunkin AM. Se myös käyttää HTTP-protokolla ping jokaiselle AM tietyn URL-Osoitteesta: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) - Microsoft.Network/loadBalancers resurssin laji ja networkProfile ja virtualMachineScaleSet extensionProfile.

 - **AM asteikolla määritelty laskentaklusteriin PaaS-klusterin hallinnan käyttöönotto** - joukot joskus kuvataan seuraavan sukupolven Työntekijä-roolin AM asteikko. Se on kelvollinen kuvaus mutta se myös toimii sekava asteikko riskiä ominaisuuksien määrittäminen PaaS v1 työntekijän rooli ominaisuuksilla. Tasolla AM asteikko joukot antaa tosi "Työntekijä rooli" tai työntekijä resurssin siten, että generalized Laske resurssin ympäristö/runtime riippumaton, eli niissä voi mukauttaa ja integroi Azure Resurssienhallinta IaaS osaksi.

    PaaS v1 työntekijän rooli, kun rajoitettu kannalta ympäristö/runtime-tuki (vain Windows platform kuvien) sisältää myös palvelut, kuten VIP swap, määritettäviä päivitysasetukset, runtime/sovelluksen käyttöönotto tiettyjä asetuksia, jotka eivät ole _vielä_ käytettävissä AM skaalata joukot tai muut suurempi tason PaaS palvelut, kuten palvelun kangasta toimitetaan. Tässä yhteydessä muista, että voi tarkastella AM asteikko joukot infrastruktuuria, joka tukee PaaS nimellä. Toisin sanoen PaaS ratkaisuja, kuten palvelun kangasta tai klusterin valvojat kuin Mesos kehittää päälle AM skaalata joukot skaalattava Laske kerrokseen.

    Tässä on esimerkki Mesos-klusterin joka ottaa käyttöön AM asteikko määrittäminen saman VNET käyttöoikeudet erikseen AM. Erillisestä AM on Mesos perusmuodon ja AM asteikko määrittäminen edustaa toissijaisen solmujen: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json). [Azure säilö palvelun](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) tulevissa versioissa otetaan käyttöön lisää kompleksiluvun/karkaistu versioissa tässä skenaariossa AM asteikko joukot perusteella.

## <a name="vm-scale-set-performance-and-scale-guidance"></a>AM asteikko määrittäminen suorituskyky ja skaalattavuus ohjeet

- Julkinen esikatselun aikana ei luoda yli 500 VMs useita AM asteikko joukoissa kerrallaan.
- Suunnittele enintään 40 VMs tallennustilan tilin kohden.
- Levittäminen tallennustilan tilien nimet mahdollisimman paljon ensimmäinen kirjain.  Esimerkki VMSS mallit [Azure pikaopas malleja](https://github.com/Azure/azure-quickstart-templates/) on esimerkkejä tähän.
- Jos käytät mukautettua VMs, suunnitteleminen enintään 40 VMs AM asteikko määrittäminen yksittäisen tallennustilaa tilillä kohden.  Sinun on valmiiksi kopioitu tallennustilan huomioon ennen aloittamista AM asteikko määrittäminen käyttöönoton kuva. Katso lisätietoja usein kysytyistä Kysymyksistä.
- Suunnittele enintään 2 048 VMs VNET kohden.  Tämä rajoitus kasvaa tulevaisuudessa.
- Voit luoda VMs määrä on rajoitettu Laske core-kiintiön kaikki alueen mukaan. Voit joutua yhteystiedot niin, että oman Laske kiintiö entistä parempi, vaikka sinulla olisi sydämiä käytettäviksi pilvipalveluihin tai IaaS v1 suuri enimmäismäärä tänään asiakastukeen. Kyselyn kiintiön voit suorittaa seuraavat Azure CLI-komennon: _azure AM - käyttö_ja PowerShell-komentoa: _Get-AzureRmVMUsage_ (Jos _Get-AzureVMUsage_käyttämällä PowerShell versiolla 1.0 alla).

## <a name="vm-scale-set-frequently-asked-questions"></a>AM asteikko määrittäminen usein kysytyt kysymykset

**Q.** Kuinka monta VMs sinun on AM asteikko joukon?

**A.** 100, jos käytät platform kuvia, jotka voidaan jakaa tallennustilan useiden eri tilien välillä. Jos käytät mukautettuja kuvia, enintään 40, koska mukautettujen kuvat ovat vain yhden tallennustilan tilin esikatselun aikana.

**Q** mitä resurssien rajoitukset olemassa AM asteikko joukkojen?

**A.** Voit on oikeutettu enintään 500 VMs luominen useita asteikko-joukot alueittain aikana esikatselu. Nykyisen [Azure Tilauspalvelussa rajoitukset /](../articles/azure-subscription-service-limits.md) käyttää.

**Q.** Kuuluvat AM asteikko joukot tietoja levyjen tueta?

**A.** Muiden kuin alkuperäinen versio. Tietojen tallentaminen ovat:

- Azure-tiedostot (jaettu SMB asemat)

- OS asema

- Temp-asema (paikallinen, ei palautettua Azure tallennustilan)

- Ulkoisten tietojen service (kuten remote DB, Azure taulukot, Azure BLOB)

**Q.** Mitä Azure alueiden tukee AM asteikko joukkojen?

**A.** Alue, joka tukee Azure Resurssienhallinta tukee AM asteikko joukot.

**Q.** Miten voit luoda AM asteikolla joukkoja mukautetun kuvan?

**A.** Jätä vhdContainers-ominaisuuden tyhjäksi, esimerkiksi:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**Q.** Jos pienentäminen AM-asteikko määrittää kapasiteetin 20 15, jossa VMs poistetaan?

**A.** Näennäiskoneiden poistetaan määritetty tasaisesti päivityksen toimialueet ja vika toimialueiden Suurenna käytettävyys asteikko.

**Q.** Miten se Jos 18 from 15 resursseja on Suurenna sitten?

**A.** Jos kasvatat 18 VMs indeksi 15 16 17 luodaan. Kummassakin tapauksessa VMs ovat saapuva FDs ja UDs.

**Q.** Käytettäessä useita tiedostotunnisteita AM asteikko joukon voit pakottaa suorittamisen sarjan?

**A.** Ei suoraan, mutta customScript-tunnisteeseen komentosarja voi odottaa, viimeistele toiseen tunniste (esimerkiksi tiedostotunnisteen seuraamalla Kirjaudu - kohdassa [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install\_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)).

**Q.** AM asteikko joukot suoritetaan Azure käytettävyys joukot?

**A.** Kyllä. AM asteikko on implisiittisesti saatavuus 3 FDs ja 5 UDs määrittäminen. Ei tarvitse määrittää mitään virtualMachineProfile-kohdassa. Tulevissa versioissa AM asteikko joukot todennäköisesti useita alihallinnat, jotka ulottuvat mutta nyt asteikolla määrittäminen on yksi käytettävyys määrittäminen.
