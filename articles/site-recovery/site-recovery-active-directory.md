<properties
    pageTitle="Suojaa Active Directory- ja DNS-ja Azure palauttaminen | Microsoft Azure"
    description="Tässä artikkelissa kuvataan tietojen palauttaminen ratkaista ottamisesta käyttöön Active Directoryn avulla Azure palauttaminen."
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="08/31/2016"
    ms.author="pratshar"/>

# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Suojaa Active Directory- ja DNS-ja Azure sivuston palauttaminen

Enterprise-sovellusten, kuten SharePoint, Dynamics AX ja SAP määräytyvät sen mukaan, Active Directory- ja DNS-infrastruktuurin toimi oikein. Kun luot tietojen palauttaminen ratkaista sovellusten, on tärkeää muistaa, että haluat suojata ja palauttaa Active Directory- ja DNS ennen muita sovelluksen osia, jotta asioita toimisi oikein, kun tietojen ilmenee.

Sivuston palautus on Azure-palvelu, joka tarjoaa palauttaminen orchestrating replikoinnin automaattisesti ja näennäiskoneiden palauttaminen. Sivuston palauttaminen tukee useita replikoinnin skenaariot johdonmukaisesti suojaaminen ja saumattomasti automaattisesti näennäiskoneiden ja julkisten, yksityinen tai isäntä paveikslėlis sovellukset.

Käytä sivustojen palauttamisen, voit luoda valmis automaattisen tietojen palauttaminen suunnitelman Active Directory. Kun tapahtuu keskeytyksiä, voit aloittaa automaattisesti muutamassa missä tahansa ja lisämääritykset Active Directory muutaman minuutin kuluttua. Jos olet käyttöön Active Directory varten useiden sovelluksista, kuten SharePoint and SAP ensisijainen sivuston ja haluat epäonnistua valmis sivuston kautta, voi epäonnistua Active Directoryn kautta käyttämällä ensin palauttaminen ja sitten hylätty kautta käyttämällä sovelluksen kielikohtaiset palautus suunnitelmien muut sovellukset.

Tässä artikkelissa kerrotaan, miten luot ratkaisun tietojen palauttaminen Active Directory, tietoja suunniteltujen, suunnittelematon ja testaa failovers yhdellä napsautuksella palautus-palvelupaketti, tuettujen määritykset ja edellytykset.  Pitäisi olla Active Directory ja Azure palauttaminen ennen kuin aloitat.

Vaihtoehtoja on kaksi suositellut ympäristön monimutkaisuuden perusteella.

### <a name="option-1"></a>Vaihtoehto 1

Jos sinulla on small useita sovelluksia ja yhden toimialueen ohjauskoneen ja haluat epäonnistua koko sivuston päälle, valitse käyttäminen on suositeltavaa palauttaminen replikoida toimialueen ohjauskoneen toissijainen sivustoon (onko sinun on kaatuvat päälle Azure tai toissijaisen sivustoon). Saman replikoitua virtuaalikoneen voi käyttää liian testi automaattisesti.

### <a name="option-2"></a>Vaihtoehto 2

Jos sinulla on runsaasti sovellukset ja on useampi kuin yksi toimialueen ohjauskoneen ympäristössä tai jos aiot epäonnistua päälle joitain sovelluksia kerrallaan, on suositeltavaa, että lisäksi replikoiminen toimialueen ohjauskoneen virtuaalikoneen palauttaminen kanssa voi järjestää myös muita ohjauskoneen kohde-sivustossa (Azure tai toissijaisen paikallisen palvelinkeskuksen).

>[AZURE.NOTE] Vaikka olet käyttöönoton vaihtoehto-2-testi-automaattisesti, sinun on edelleen replikoida käyttämällä sivuston toimialueen ohjauskoneen tekemisestä. Lue lisätietoja [testaaminen automaattisesti huomioon otettavia seikkoja](#considerations-for-test-failover) .


Seuraavissa osissa kerrotaan sivuston palauttaminen toimialueen ohjauskoneen suojauksen ottaminen käyttöön ja voit määrittää toimialueen ohjauskoneen Azure-tietokannassa.


## <a name="prerequisites"></a>Edellytykset

- Paikallisen ympäristön Active Directory- ja DNS-palvelimen.
- Microsoft Azure tilauksessa Azure sivuston palauttaminen palvelut-säilö.
- Jos sinun on replikoiminen Azure Suorita Azure virtuaalikoneen valmiuden työkalu VMs, jotta ne ovat yhteensopivia Azure VMs ja Azure sivuston palauttaminen palvelut.


## <a name="enable-protection-using-site-recovery"></a>Ota suojaus sivuston palauttaminen


### <a name="protect-the-virtual-machine"></a>Suojaa virtuaalikoneen

Ota käyttöön palauttaminen toimialueen ohjauskoneen/DNS-virtuaalikoneen suojaus. Sivuston palauttaminen asetusten virtuaalikoneen lajin (Hyper-V tai VMware). On suositeltavaa kaatumisen yhdenmukaisia replikoinnin taajuus 15 minuuttia.

###<a name="configure-virtual-machine-network-settings"></a>Virtuaalikoneen verkkoasetusten määrittäminen

Toimialueen ohjauskoneen/DNS-virtuaalikoneen käyttöön määrittää verkoston asetusten palauttaminen niin, että AM kiinnitetään oikea verkon jälkeen automaattisesti. Esimerkiksi jos olet replikoiminen Hyper-V VMs Azure, voit valita AM VMM pilvipalveluun tai alla kuvatulla tavalla verkko-asetusten suojaus-ryhmä

![AM verkkoasetukset](./media/site-recovery-active-directory/VM-Network-Settings.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Suojaa Active Directoryyn Active Directory-toistot

### <a name="site-to-site-protection"></a>Sivusto suojaus

Luo toimialueen ohjauskoneen toissijainen sivustoon ja samaa toimialuetta, joka on käytössä ensisijainen sivustossa, kun muunnat toimialueen ohjauskoneen rooli palvelimen nimi. **Active Directory-sivustot ja -palvelut** -laajennuksen avulla voit määrittää asetukset sivusto-linkin objekti, johon on lisätty sivustot. Asetusten määrittäminen sivuston linkkiä, voit ohjata toistoa ajankohdan kahden tai useamman sivustojen välinen ja kuinka usein. Saat lisätietoja [Ajoituksen sivustojen välinen replikointi](https://technet.microsoft.com/library/cc731862.aspx) .

###<a name="site-to-azure-protection"></a>Sivuston Azure suojaus

[Luo toimialueen ohjauskoneen Azure virtual verkossa](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)ohjeiden mukaan. Kun muunnat toimialueen ohjauskoneen rooli palvelimen Määritä sama toimialuenimi, jota käytetään ensisijainen sivustossa.

Valitse [uudelleen virtual verkon DNS-palvelimeen](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), DNS-palvelimen käyttäminen Azure.

![Azure verkossa](./media/site-recovery-active-directory/azure-network.png)

## <a name="test-failover-considerations"></a>Testaa automaattisesti huomioon otettavia seikkoja

Testaa vikasietotila ilmenee, joka on eristetty tuotannon verkon niin, että ei ole vaikutusta tuotannon työmääriä verkossa.

Useimpien sovellusten vaativat myös toimialueen ohjauskoneen ja toimisi, DNS-palvelimeen, joten sovelluksen epäonnistui päälle, toimialueen ohjauskoneen on luoda eristetyssä verkossa, jota käytetään testin automaattisesti. Helpoin tapa on toimialueen ohjauskoneen/DNS virtuaalikoneen kanssa palauttaminen suojauksen ottaminen käyttöön ja Suorita testi-automaattisesti virtual, että koneen ennen kuin suoritat testi-automaattisesti sovelluksen palautus-palvelupaketin. Näin miten teet näin:

1. Toimialueen ohjauskoneen/DNS virtuaalikoneen sivuston palautus-suojauksen käyttöön.
2. Luo eristetyssä verkossa. Luotu Azure oletusarvoisesti virtual verkon eristetään muiden verkkojen. On suositeltavaa, että verkoston IP-osoitealueita on sama kuin, tuotannon verkossa. Älä ota käyttöön sivuston sivuston connectivity tässä verkossa.
3. Anna DNS-IP-osoite luodaan, odotat DNS virtuaalikoneen saat IP-osoitetta verkossa. Jos olet replikoiminen, Azure, sitten avulla IP-osoite, jota käytetään automaattisesti **Kohde-IP** -asetusta AM ominaisuudet AM. Jos olet replikoiminen toiseen paikallisen sivuston ja käytät DHCP seuraa ohjeita [DNS- ja testaa automaattisesti DHCP](site-recovery-failover.md#prepare-dhcp) : n määrittäminen

>[AZURE.NOTE] Kohdistettu virtual machine aikana testi automaattisesti IP-osoite on sama kuin IP-osoite, se saisit aikana suunniteltu tai suunnittelematon automaattisesti, jos IP-osoite on käytettävissä testi automaattisesti verkossa. Jos se ei ole, virtuaalikoneen vastaanottaa eri IP-osoite, joka on käytettävissä testi automaattisesti verkossa.

4. Suorita testi-automaattisesti sen toimialueen ohjauskoneen virtuaalikoneen eristetyssä verkossa. Toimialueen ohjauskoneen virtuaalikoneen uusimmat käytettävissä sovelluksen yhdenmukaisia palautus kohdan avulla voit tehdä testin vikasietotilaa. 
5. Suorita testi-automaattisesti sovelluksen palautus-palvelupaketteihin.
6. Kun testaus on suoritettu, merkitse testi automaattisesti työn toimialueen ohjauskoneen virtuaalikoneen ja palauttaminen-portaalin **työt** -välilehden palauttaminen suunnitelma "Valmis".

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS- ja toimialueen ohjauskoneen eri tietokoneissa

Jos DNS ei ole sama kuin toimialueen ohjauskoneen virtuaalikoneen tarvitset Luo DNS-AM, Testaa vikasietotilaa. Jos ne ovat samassa AM, voit ohittaa tämän osion.

Voit käyttää ajan tasalla DNS-palvelin ja luo tarvittavat vyöhykkeisiin. Esimerkiksi jos Active Directory-toimialueen contoso.com, voit luoda DNS-vyöhyke nimi contoso.com kanssa. Active Directory vastaavat tapahtumat on päivitettävä DNS-seuraavasti:

1. Varmista, että nämä asetukset ovat paikallaan ennen muiden koneen virtual palautus-suunnitelmassa:

    - Alueen nimen on oltava metsää pääkansion nimen jälkeen.
    - Vyöhykkeen on oltava palautettua tiedostoa.
    - Vyöhykkeen on otettava käyttöön suojattu ja suojaamaton päivitykset.
    - Toimialueen ohjauskoneen virtuaalikoneen tulkintatoiminnon osoitettava DNS virtuaalikoneen IP-osoite.

2. Valitse toimialueen ohjauskoneen virtuaalikoneen varten suorittamalla seuraavan komennon:

    `nltest /dsregdns`

3. Lisää vyöhykkeen DNS-palvelimeen, Salli suojaamaton päivitykset ja tekstin lisääminen sen DNS:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1


## <a name="next-steps"></a>Seuraavat vaiheet

Lue [mitä työmääriä voit suojata?](../site-recovery/site-recovery-workload.md) lisätietoja suojaaminen yrityksen työmääriä Azure palauttaminen kanssa.
