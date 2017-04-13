<properties
 pageTitle="Luo pää HPC Pack-solmu Azure-AM | Microsoft Azure"
 description="Opettele käyttämään Microsoft HPC Pack pään solmu luominen Azure-AM Azure portaalin ja resurssien hallinnan käyttöönottomalli."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Pään solmu HPC Pack-klusterin luominen Azure-AM ja Marketplace-kuva


Luo pää solmu HPC-klusterin [Microsoft HPC Pack virtuaalikoneen kuva](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) Azure Marketplacesta ja Azure-portaalin avulla. Tämä HPC Pack AM kuva perustuu Windows Server 2012 R2 palvelinkeskuksen HPC Pack 2012 R2: n päivitys 3 valmiiksi asennettuna. Käytä pään solmu kuitti käsite käyttöönoton HPC Pack Azure-tietokannassa. Voit lisätä Laske solmujen sitten Suorita HPC työmääriä klusterin.



>[AZURE.TIP]Otetaan käyttöön koko HPC Pack-klusterin Azure-tietokannassa, joka sisältää pää-solmu ja Laske solmujen, on suositeltavaa, että käytät automaattisesta. [HPC Pack IaaS käyttöönoton komentosarja](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) ja [HPC Pack for Windowsin työmääriä klusterin](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) Resurssienhallinta-malli on asetukset. Saat lisämalleja [Azure HPC Pack klusterin asetukset](virtual-machines-windows-hpcpack-cluster-options.md) . 


## <a name="planning-considerations"></a>Suunnittelussa huomioon otettavia seikkoja

Seuraavassa kuvassa esitetyllä voit ottaa käyttöön Active Directory-toimialueen Azure virtual verkossa HPC Pack pää-solmu.

![HPC Pack pään solmu][headnode]

* **Active Directory-toimialueen** - HPC Pack pään solmu on liitettävä Azure Active Directory-toimialueen ennen aloittamista AM HPC-palvelut. Tässä artikkelissa siitä, joka kertoo käyttöönoton esitetyllä tavalla voit muuntaa AM, voit luoda pään solmun kuin toimialueen ohjauskoneen ennen kuin aloitat HPC-palvelut. Toinen vaihtoehto on erillinen toimialueen ohjauskoneen ja metsää Azure-tietokannassa, johon olet liittynyt pään solmu AM käyttöönotto.

* Määritä tai Azure virtual verkoston luominen **azure virtual verkko** - käytettäessä resurssien hallinnan käyttöönottomalli ottamaan pään solmu. Voit käyttää virtual verkon, jos haluat liittyä pään solmu Active Directory-toimialueen. Lisäksi tarvitset sen myöhemmin, jos haluat lisätä Laske solmu VMs klusterin.

    
## <a name="steps-to-create-the-head-node"></a>Luo pää solmu vaiheet

Seuraavassa on korkean tason ohjeet, joiden avulla voit luoda Azure-AM HPC Pack pään solmun resurssien hallinnan käyttöönottomalli Azure portaalin. 


1. Jos haluat luoda uuden Active Directory-metsää Azure erillisessä toimialueen ohjauskoneen VMs kanssa, yksi vaihtoehto on käyttää [Resurssienhallinta-malli](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/). Yksinkertainen siitä, joka kertoo käyttöönoton on kunnossa ohittaa tämän vaiheen ja määrittää toimialueen ohjauskoneen pään solmu AM itse. Tämä asetus on kuvattu tämän artikkelin.
    
2. Valitse **Luo virtuaalikoneen** [HPC Pack 2012 R2: n Windows Server 2012 R2-sivulla](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) Azure Marketplacesta. 

3. Portaalissa **HPC Pack 2012 R2: n Windows Server 2012 R2-** sivulla Valitse **Resurssi hallinnan** käyttöönotto-malli ja valitse sitten **Luo**.

    ![HPC Pack-kuva][marketplace]

4. Portaalin avulla voit määrittää asetukset ja luo AM. Jos ole ennen käyttänyt Azure, noudata opetusohjelman [luominen Windows-virtual machine Azure-portaalissa](virtual-machines-windows-hero-tutorial.md). Siitä, joka kertoo käyttöönoton voit tavallisesti Hyväksy oletusasetukset tai suositeltuja asetuksia.

    >[AZURE.NOTE]Jos haluat liittyä pään solmu luodun Active Directory-toimialueen Azure-tietokannassa, varmista, että määrität kyseisen toimialueen virtual verkko AM luotaessa.
       
4. Kun olet luonut AM ja AM on käynnissä, [Yhdistäminen AM](virtual-machines-windows-connect-logon.md) Etätyöpöytä. 

5. Liity AM aiemmin toimialue-metsää tai luo toimialueen metsää AM itse.

    * Jos olet luonut AM Azure virtual verkostossayammer aiemmin toimialue-metsää, liity AM metsää vakio palvelimen hallinnan tai Windows PowerShell-työkalujen avulla. Käynnistä.

    * Jos olet luonut uuden virtual verkon (ilman toimialueen aiemmin metsää) AM, poista AM sitten toimialueen ohjauskoneen. Vakio ohjeiden avulla voit asentaa ja määrittää Active Directory-toimialueen palveluista rooli pään solmun. Yksityiskohtaisia ohjeita on kohdassa [asentaminen uusi Windows Server 2012 Active Directory metsää](https://technet.microsoft.com/library/jj574166.aspx).

5. Kun AM on käynnissä, ja se on yhdistetty Active Directory-metsää, Aloita HPC Pack-palveluiden seuraavasti:

    a. Muodosta yhteys pään solmu AM toimialueen tilillä, joka on Järjestelmänvalvojat-ryhmän jäsen. Käytä esimerkiksi järjestelmänvalvojatilin, voit määrittää, pää solmu AM luodessasi.

    b. Käynnistä Windows PowerShell järjestelmänvalvojana pään solmu oletusasetukset, ja kirjoita seuraava komento:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Voi kestää useita minuutteja HPC Pack services alkavan.

    Kirjoita Lisää pään solmu kokoonpanoasetusten määrittäminen `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Seuraavat vaiheet

* Voit nyt käsitellä HPC Pack-klusterin pään solmu. Esimerkiksi HPC klusterin hallinnan käynnistäminen ja suorittaa [Käyttöönoton tehtäväluettelo](https://technet.microsoft.com/library/jj884141.aspx).
* Jos haluat suurentaa klusterin Laske kapasiteetin tarvittaessa, Lisää [Azure purskeen solmujen](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) pilvipalvelussa. 

* Suorita testi työmäärää klusterin. Esimerkki on artikkelissa HPC Pack [Aloitusopas](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png
