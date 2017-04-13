<properties
 pageTitle="Määrittää Windows RDMA-klusterin MPI sovellukset toimimaan | Microsoft Azure"
 description="Opettele luomaan Windows HPC Pack-klusterin koko H16r, H16mr, A8 tai A9 VMs käyttää Azure RDMA verkon MPI-sovellusten kanssa."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/20/2016"
 ms.author="danlep"/>

# <a name="set-up-a-windows-rdma-cluster-with-hpc-pack-to-run-mpi-applications"></a>Windowsin RDMA klusterin HPC Pack MPI sovelluksia määrittäminen

Määritä Windows RDMA Azure-tietokannassa [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) ja [H - tai Laske paljon A sarjan esiintymät](virtual-machines-windows-a8-a9-a10-a11-specs.md) klusterin rinnakkain viestin kulkeva Interface (MPI)-sovelluksia. Määrittäessäsi RDMA-yhteyttä hyödyntäviin, Windows Server solmujen HPC Pack-klusterin MPI sovellukset kommunikoida tehokkaasti pieni viive Azure-tietokannassa, joka perustuu remote suoraan muistin käytön (RDMA) tekniikka suuren siirtonopeuden verkon kautta.

Jos haluat suorittaa MPI työmääriä Linux VMs, joka käyttää Azure RDMA verkkoa, katso [Linux RDMA klusterin MPI sovelluksia määrittäminen](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="hpc-pack-cluster-deployment-options"></a>HPC Pack klusterin käyttöönottoasetukset
Microsoft HPC Pack on työkalun edellyttäen ilman lisäkustannuksia luominen HPC klusterit paikallisen tai Azure Windows- tai Linux HPC sovellusten suorittamiseen. HPC pakettiin sisältyy runtime-ympäristön Microsoft täytäntöön viestin kulkeva käyttöliittymän Windowsin (MS-MPI). Käytettäessä yhdessä RDMA-yhteyttä hyödyntäviin esiintymät, jossa on tuettu Windows Server-käyttöjärjestelmä HPC Pack on tehokas vaihtoehto Windows MPI-sovelluksia, jotka Azure RDMA verkon käytön. 

Tässä artikkelissa esitellään kaksi skenaariot ja linkit yksityiskohtaiset ohjeet Winodws RDMA klusterin Microsoft HPC Pack määrittäminen. 

* Käyttötilanne 1. Ottaa käyttöön Laske paljon työntekijän rooli esiintymät (PaaS)

* Käyttötilanne 2. Ottaa käyttöön Laske solmujen Laske paljon VMs (IaaS)

Laske paljon esiintymät käytettäväksi Windows Yleiset edellytykset on artikkelissa [tietoja H-sarjan ja Laske paljon A sarjan VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md) .



## <a name="scenario-1-deploy-compute-intensive-worker-role-instances-paas"></a>Käyttötilanne 1. Ottaa käyttöön Laske paljon työntekijän rooli esiintymät (PaaS)

Aiemmin luodusta HPC Pack klusterista lisätä ylimääräisen Laske resursseja Azure työntekijän rooli tilanteissa (Azure solmujen) käynnissä pilvipalvelussa (PaaS). Tätä ominaisuutta kutsutaan myös "burst Azure," HPC Packin tukee koot solualueen työntekijän rooli esiintymät. Kun lisäät Azure solmut, määrittämällä jokin RDMA-yhteyttä hyödyntäviin koot.

Seuraavassa on huomioitavista ja ohjeiden avulla voit RDMA-yhteyttä hyödyntäviin burst Azure esiintymät luodusta (yleensä paikallisen) klusterin. Työntekijän roolin esiintymät lisääminen HPC Pack pään solmu, joka on otettu käyttöön Azure-AM samalla menettelyt avulla.

>[AZURE.NOTE] Katso opetusohjelma, jotta burst Azure HPC Pack- [hybrid klusterin HPC Pack määrittäminen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Huomaa seuraavia ohjeita, jotka liittyvät erityisesti RDMA-yhteyttä hyödyntäviin seikat Azure solmujen.

![Azure Burst][burst]

### <a name="steps"></a>Vaiheet

4. **Ottaa käyttöön ja määrittää pään HPC Pack 2012 R2-solmu**

    Lataa uusimmat HPC Pack asennuspaketin [Microsoft Download Centeristä](https://www.microsoft.com/download/details.aspx?id=49922). Vaatimukset ja Azure purskeen käyttöönoton valmisteleminen ovat artikkelissa [HPC Pack Aloitusoppaan](https://technet.microsoft.com/library/jj884144.aspx) ja [purskeen Azure työntekijä esiintymät ja Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).

5. **Azure tilauksen hallinta-varmenteen määrittäminen**

    Määritä varmenteen suojaamiseen pään solmu ja Azure välinen yhteys. Asetukset ja ohjeita on artikkelissa [tilanteita, joissa HPC Pack Azure hallinta-varmenteen määrittäminen](http://technet.microsoft.com/library/gg481759.aspx). Testi-versioiden HPC Pack asentaa Default Microsoft HPC Azure hallinta varmenteen voit nopeasti ladata Azure-tilaukseen.

6. **Luo uusi pilvipalvelussa ja tallennustilaa tilin**

    Azure perinteinen portaalin avulla voit luoda pilvipalveluun ja tallennustilaa tilin käyttöönoton alueen, jossa RDMA-yhteyttä hyödyntäviin esiintymät ovat käytettävissä.

7. **Azure solmu mallin luominen**

    Käytä-solmu mallin ohjattu luominen HPC klusterin hallinta. Ohjeita on artikkelissa [Azure solmu mallin luominen](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) "Ohjeita voit ottaa käyttöön Azure solmujen kanssa Microsoft HPC Pack".

    Alkuperäinen testeissä Suosittelemme manuaalinen käytettävyys käytännön määrittäminen mallin.

8. **Klusterin solmujen lisääminen**

    Käytä-solmu Ohjattu lisääminen HPC klusterin hallinta. Lisätietoja on artikkelissa [Azure-solmut Lisää HPC Windows-klusterin](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add).

    Muotoilujen solmut kokoa, valitse jokin RDMA-yhteyttä hyödyntäviin esiintymän koot.
    
    >[AZURE.NOTE]Azure-ympäristö, jonka suorittaminen paljon esiintymät, kunkin purskeen HPC Pack tunnistus vähintään 2 RDMA-yhteyttä hyödyntäviin esiintymien (kuten A8) automaattisesti kuin välityspalvelimen solmujen lisäksi voit määrittää Azure työntekijän rooli esiintymät. Välityspalvelimen solmut käyttää sydämiä, joka on kohdistettu tilaus ja maksamaan kuluja sekä Azure työntekijän rooli esiintymät.

9. **Käynnistä (valmisteleminen) solmut ja tuo ne verkossa töiden suorittaminen**

    Valitse solmut ja **Käynnistä** -toimintoa käytetään HPC klusterin hallinta. Kun valmistelu on valmis, valitse solmut ja **Tuoda Online** -toimintoa käytetään HPC klusterin hallinta. Solmut on valmis suorittamaan työt.

10. **Lähettää klusterin työt**

    Klusterin töiden HPC Pack työn lähetyksen työkalujen avulla. Katso [Microsoft HPC Pack: projektin hallinta](http://technet.microsoft.com/library/jj899585.aspx).

11. **Lopeta (deprovision) solmut**

    Kun olet valmis käynnissä olevat työt-solmut offline-tilaan ja **Lopeta** -toimintoa käytetään HPC klusterin hallinta.





## <a name="scenario-2-deploy-compute-nodes-in-compute-intensive-vms-iaas"></a>Käyttötilanne 2. Ottaa käyttöön Laske solmujen Laske paljon VMs (IaaS)

Tässä skenaariossa käyttöönottoa HPC Pack pää-solmu ja klusterin Laske solmujen VMs Active Directory-toimialueen Azure virtual verkossa. HPC Pack sisältää [Azure VMs Käyttöönottoasetukset](virtual-machines-linux-hpcpack-cluster-options.md), kuten automaattisen käyttöönoton komentosarjoja ja Azure pikaopas malleja. Luodaan esimerkiksi alla olevia ohjeita ja huomioon otettavia seikkoja-opas [HPC Pack IaaS käyttöönoton komentosarja](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) avulla voit automatisoida useimmat prosessia.

![Azure VMs klusterin][iaas]



### <a name="steps"></a>Vaiheet

1. **Luo pää klusterin-solmu ja Laske solmu VMs suorittamalla HPC Pack IaaS käyttöönoton komentosarja asiakastietokoneessa**

    Lataa HPC Pack IaaS käyttöönoton komentosarja-paketin [Microsoft Download Centeristä](https://www.microsoft.com/download/details.aspx?id=49922).

    Asiakastietokoneen valmistelu Luo komentosarja-määritystiedosto ja suorittamalla komentosarja, katso [HPC-klusterin HPC Pack IaaS käyttöönoton komentosarjan luominen](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). 
    
    Ottaa käyttöön RDMA-yhteyttä hyödyntäviin Laske solmujen Huomaa seuraavat muita huomioon otettavia seikkoja:
    
    * **Virtual verkko** - määrittää uuden virtual verkon alueessa, joka on saatavilla RDMA-yhteyttä hyödyntäviin esiintymän koon, jota haluat käyttää.

    * **Windows Server-käyttöjärjestelmä** - tukea RDMA yhteys, Määritä Windows Server 2012 R2 tai Windows Server 2012 käyttöjärjestelmän Laske solmun VMs.

    * **Cloud services** - Suosittelemme, että pään solmu yhden pilvipalvelussa ja Laske-solmut eri pilvipalvelussa käyttöönotto.

    * **Päätä solmun koon** - tässä tapauksessa kannattaa koko on vähintään A4 (ylimääräisiä suuri) pään solmun.

    * **HpcVmDrivers tunniste** - käyttöönoton komentosarja asentaa Azure AM agentti ja HpcVmDrivers-tunniste automaattisesti kun otat käyttöön koko A8 tai A9 Laske solmujen Windows Server-käyttöjärjestelmässä. HpcVmDrivers asentaa ohjaimet Laske solmun VMs, jotta he voivat muodostaa yhteyden RDMA verkkoon.

    * **Klusterin verkon määritys** - käyttöönoton komentosarja määrittää automaattisesti HPC Pack-klusterin topologian 5 (kaikki solmut yrityksen verkossa). Tämä topologian tarvitaan kaikki HPC Pack klusterin käyttöönotoissa VMs. Älä muuta klusterin verkkotopologia myöhemmin.

2. **Tuo Laske solmut verkossa töiden suorittaminen**

    Valitse solmut ja **Tuoda Online** -toimintoa käytetään HPC klusterin hallinta. Solmut on valmis suorittamaan työt.

3. **Lähettää klusterin työt**

    Voit lähettää työt pään solmu yhdistäminen tai määrittää paikallisen tietokoneen toiminto. Lisätietoja on artikkelissa [Lähetä työt HPC-klusterin Azure-tietokannassa](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).

4. **Solmut offline-tilaan ja lopettaa (poistaa varauksen) ne**

    Kun olet tehnyt käynnissä olevat työt, ota offline-tilassa solmut HPC klusterin hallinta. Valitse ne Sammuta Azure hallintatyökalut avulla.



## <a name="run-mpi-applications-on-the-cluster"></a>Suorita MPI sovelluksia klusterin

### <a name="example-run-mpipingpong-on-an-hpc-pack-cluster"></a>Esimerkki: Suorita mpipingpong HPC Pack-klusterissa

Vahvista HPC Pack-käyttöönoton RDMA-yhteyttä hyödyntäviin esiintymien klusterin HPC Pack **mpipingpong** -komennon suorittaminen. **mpipingpong** lähettää pisteparin solmujen toistuvasti viive ja mittoja siirtonopeuden laskemiseen ja tilastot RDMA käyttävä sovelluksen verkon tietojen paketit. Tässä esimerkissä näytetään tyypillinen kuvion suorittamisen MPI-työ (eli tässä tapauksessa **mpipingpong**) klusterin **mpiexec** -komennon avulla.

Tässä esimerkissä oletetaan, että olet lisännyt Azure solmujen "purskeen Azure-kokoonpanossa ([Skenaario 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS) in this article). Jos olet asentanut HPC Pack Azure VMs klusterin, tarvitset muokkaamalla komennon syntaksia Määritä eri solmu-ryhmä ja määritä muut ympäristömuuttujat ohjaamaan verkkoliikennettä RDMA verkkoon.


Voit suorittaa mpipingpong klusterin seuraavasti:


1. Pään solmu tai oikein määritetyn asiakastietokone avaa komentorivi-ikkuna.

2. Arvioida viive välisen Azure purskeen käyttöönoton 4 solmujen solmujen, kirjoita seuraava komento lähettää työn mpipingpong toimimaan pieni pakettikoko ja suuri määrä Iteraatioita:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    Komento palauttaa työn, joka lähetetään tunnus.

    Jos olet asentanut käyttöön Azure VMs Määritä solmu-ryhmä, joka sisältää HPC Pack-klusterin Laske yhteen pilvipalvelussa käyttöön VMs solmu ja muokkaaminen **mpiexec** -komento seuraavasti:

    ```
    job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI_DISABLE_SOCK 1 -env MSMPI_PRECONNECT all -env MPICH_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
    ```

3. Kun työ on valmis, voit tarkastella tulos (Tässä tapauksessa tehtävä 1 työn tulosteen), kirjoita seuraava komento

    ```
    task view <JobID>.1
    ```

    Jos &lt; *työn tunnus* &gt; työ, joka on lähetetty ID-tunnusta.

    Tulos sisältää viive tulokset seuraavankaltaiselta.

    ![Ping pong viive][pingpong1]

4. Arvioida siirtonopeuden välisen Azure purskeen solmujen, kirjoita seuraava komento lähettää työn **mpipingpong** toimimaan suuri pakettikoko ja pieneen Iteraatioita:

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    Komento palauttaa työn, joka lähetetään tunnus.

    Muokkaa HPC Pack-klusterin, ottaa käyttöön Azure VMs-komento, kuten vaiheessa 2.

5. Kun työ on valmis, voit tarkastella tulos (Tässä tapauksessa tehtävä 1 työn tulosteen), kirjoita seuraava komento:

    ```
    task view <JobID>.1
    ```

  Tulos sisältää liikenteen tulokset seuraavankaltaiselta.

  ![Ping pong siirtonopeuden][pingpong2]


### <a name="mpi-application-considerations"></a>MPI sovelluksen huomioon otettavia seikkoja


Seuraavassa on käynnissä MPI sovellukset ja HPC Pack Azure-tietokannassa Huomioitavaa. Jotkin koskevat vain ominaisuuksissa Azure solmujen (työntekijän rooli esiintymät lisätty "purskeen Azure-määritys).

* Työntekijän roolin esiintymät pilvipalvelussa ovat säännöllisesti valmistella uudelleen ilman erillistä ilmoitusta Azure (esimerkiksi järjestelmän ylläpidon tai erillisen epäonnistuu tapauksessa). Jos esiintymää on valmistella uudelleen sen ollessa käynnissä MPI-työ, esiintymän menettää sen tiedot ja palauttaa tilaan, kun se on ensin otettu käyttöön, jotka voivat aiheuttaa MPI työ epäonnistuu. Lisää solmujen käyttää yhden MPI työn, ja enää työ suoritetaan, mitä enemmän todennäköistä, että jokin esiintymät voidaan valmistella uudelleen samalla, kun työ on käynnissä. Ota huomioon myös tämän Jos yksittäinen solmu käyttöönoton Sisältöluokitukset tiedostopalvelimeen.


* Suorittamaan MPI töitä Azure-tietokannassa, sinun ei tarvitse käyttää RDMA-yhteyttä hyödyntäviin esiintymät. Voit käyttää minkä tahansa esiintymän koon, joka tue HPC Pack. Kuitenkin RDMA-yhteyttä hyödyntäviin esiintymät suositellaan töitä suhteellisen suurissa MPI, jotka ovat luottamuksellisia verkko, joka yhdistää solmut kaistanleveys ja viive. Jos käytät muita kokoja suorittamaan viive ja kaistanleveyden kirjainkoko MPI töitä, on suositeltavaa, jossa yksi tehtävä käytetään vain muutaman solmuissa pieni töitä.

* Azure-esiintymissä käyttöönotetuissa sovellukset ovat sovellukseen liittyvät Käyttöoikeussopimuksen ehtoja. Valitse minkä tahansa kaupallisen sovelluksen käyttöoikeuksien myöntämistä tai muita rajoituksia käynnissä pilveen toimittajan kanssa. Kaikki valmistajat tarjoavat tukiyhteyksiä käyttöoikeudet.


* Azure esiintymät on edelleen asennuksen paikallisen access-solmut, osakkeet ja palvelimet. Käyttöön Azure solmujen käyttämään paikallista-käyttöoikeuspalvelimen, voit määrittää sivuston sivuston Azure virtual verkkoon.


* Azure esiintymät MPI sovellusten suorittamiseen rekisteröidä MPI kunkin sovelluksen Windowsin palomuuri-esiintymät suorittamalla **hpcfwutil** -komennon. Näin MPI viestintä tapahtuu porttiin, joka on määritetty dynaamisesti palomuurin mukaan.

    >[AZURE.NOTE] Purskeen Azure käyttöönotoissa, voit myös määrittää palomuurin poikkeus-komento, joka suoritetaan automaattisesti kaikki uudet Azure solmut, jotka lisätään yhteyttä klusterin. Kun olet **hpcfwutil** -komennon suorittaminen ja varmista, että sovelluksesi toimii, Lisää komento käynnistyskomentosarjan Azure solmujen. Lisätietoja on artikkelissa [Azure solmujen käynnistyskomentosarjan](https://technet.microsoft.com/library/jj899632.aspx).



* HPC Pack käyttää CCP_MPI_NETMASK klusterin ympäristömuuttuja määrittää hyväksyttävät osoitteet MPI viestintään. Aloita HPC Pack 2012 R2: n, CCP_MPI_NETMASK klusterin ympäristömuuttuja koskee vain MPI viestintä toimialueeseen liittyneet klusterin Laske solmujen välillä (joko paikallisessa tai Azure VMs). Muuttujan ohitetaan solmut Azure määritysten purske lisätään.


* MPI töitä ei voi suorittaa Azure esiintymät, jotka on otettu eri pilvipalveluihin (esimerkiksi-ja Azure ympäristöissä eri solmu malleja tai ottaa käyttöön useita pilvipalveluihin Azure AM Laske solmujen purske) kautta. Jos sinulla on useita Azure solmu-käyttöönotoissa, jotka ovat aloittaminen eri solmu mallit, MPI työ on suoritettava Azure solmujen vain yhden joukon.


* Kun lisäät Azure solmujen yhteyttä klusterin ja tuoda ne verkossa, HPC työn ajoitus-palvelu yrittää välittömästi Aloita työt solmuissa. Vain osa havainnollistamiseen voidaan suorittaa Azure-varmistaa, että voit päivittää tai luo työn malleja, jotka määrittävät, mitä töiden tyypit voidaan suorittaa Azure. Esimerkiksi varmistaaksesi, että työt työmallin mukana toimitettujen suorittaa vain Azure solmujen lisääminen työmallin solmu ryhmät-ominaisuus ja valitse AzureNodes tarvittavat arvona. Jos haluat luoda mukautettuja ryhmiä Azure solmujen, Lisää HpcGroup HPC PowerShell-cmdlet-komennolla.


## <a name="next-steps"></a>Seuraavat vaiheet

* Vaihtoehtona käyttämällä HPC Pack kehittää toimimaan MPI sovellusten suorittaminen solmujen Azure hallitun jakavat Azure erä-palvelussa. Katso [Käytä usean esiintymän tehtävien viestin kulkeva Interface (MPI)-sovelluksia Azure osalta](../batch/batch-mpi.md).

* Jos haluat suorittaa Linux MPI Azure RDMA verkon käyttävät sovellukset-kohdassa [määrittäminen Linux RDMA klusterin MPI sovelluksia](virtual-machines-linux-classic-rdma-cluster.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/burst.png
[iaas]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/iaas.png
[pingpong1]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-classic-hpcpack-rdma-cluster/pingpong2.png