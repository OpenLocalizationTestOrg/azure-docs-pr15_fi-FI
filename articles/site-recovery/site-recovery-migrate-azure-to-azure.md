<properties
    pageTitle="Siirtää Azure IaaS näennäiskoneiden Azure alueelta toiseen palauttaminen | Microsoft Azure"
    description="Siirtää Azure IaaS näennäiskoneiden Azure alueelta toiseen Azure palauttaminen avulla."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="raynew"/>

#  <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Siirry Azure IaaS näennäiskoneiden välillä Azure alueet, joiden Azure sivuston palauttaminen

## <a name="overview"></a>Yleiskatsaus

Tervetuloa käyttämään Azure palauttaminen! Käytä tämän artikkelin, jos haluat siirtää Azure VMs Azure alueiden välillä. Ennen kuin aloitat, Huomaa seuraavat seikat:

- Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: Azure Resurssienhallinta ja perinteinen. Azure on myös kaksi portaaleihin – Azure perinteinen portaalin, joka tukee perinteinen käyttöönoton mallia ja Azure-portaalin tukeen käyttöönoton sekä malleille. Siirron perusvaiheet ovat samoja, vaikka olet määrittäminen sivuston palauttaminen resurssien hallinnan tai Classic-ohjelmassa. Kuitenkin Käyttöliittymän ohjeet ja näyttökuvat tämän artikkelin liittyvät Azure portaalin.
- **Nyt voit vain siirtää alueelta toiseen. Voi epäonnistua VMs Azure alueelta toiseen päälle, mutta voit et voi epäonnistua ne takaisin uudelleen.**
- Tässä artikkelissa annettuja ohjeita siirron perustuvat ohjeet replikoiminen fyysinen tietokoneen Azure. Se sisältää vaiheet linkkejä [replikoida VMware VMs tai fyysinen Azure-palvelinten](site-recovery-vmware-to-azure.md), jossa kerrotaan, miten voit toistaa fyysinen server Azure-portaalissa.
- Jos olet määrittämässä palauttaminen perinteinen-portaalissa, noudata [tämän](site-recovery-vmware-to-azure-classic.md)artikkelin tarkat ohjeet. **Sinun on käytettävä enää** tätä [Vanha artikkelissa](site-recovery-vmware-to-azure-classic-legacy.md)annettuja ohjeita.

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prerequisites"></a>Edellytykset

Näin tarvitset tätä käyttöönottoa varten:

- **Määrityspalvelimen**: käytössä Windows Server 2012 R2, joka toimii määrityspalvelimen paikallisen-AM. Asennat muiden sivustojen palauttamisen osien (mukaan lukien prosessi-palvelimen nimen ja perusmuodon kohdepalvelin) tämä AM liian. Lue lisää [skenaarion arkkitehtuuri](site-recovery-vmware-to-azure.md#scenario-architecture) ja [määritykset palvelimen edellytykset](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **IaaS näennäiskoneiden**: VMs, jotka haluat siirtää. Voit siirtää nämä VMs kohteleminen fyysisten laitteiden mukaan.

## <a name="deployment-steps"></a>Käyttöönoton vaiheet

Tässä osassa kuvataan uuden Azure portaalin käyttöönoton ohjeita. Jos tarvitset käyttöönoton seuraavasti palauttaminen perinteinen-portaalissa, katso [Tässä artikkelissa](site-recovery-vmware-to-azure-classic.md).

1. [Luo säilöön](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Ota käyttöön configuration-palvelin](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Kun configuration-palvelin on otettu käyttöön, tarkista, että se viestiä, jonka haluat siirtää VMs kanssa.
4. [Replikoinnin asetusten määrittäminen](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Replikoinnin-käytännön luominen ja liittää määrityspalvelimen.
5. [Mobility-palvelun asentaminen](site-recovery-vmware-to-azure.md#step-6-replication-application). Kunkin AM, jonka haluat suojata on asennettu Mobility-palvelu. Tämä palvelu lähettää tietoja prosessi-palvelimeen. Mobility-palvelun voidaan asentaa manuaalisesti tai miten ja asennu automaattisesti prosessi-palvelin AM suojaus on käytössä. Palomuurisäännöt, jotka haluat siirtää VMs on määritetty sallimaan tämän palvelun push-asennuksen.
6. [Ota käyttöön replikoinnin](site-recovery-vmware-to-azure.md#enable-replication). Ota käyttöön VMs, jotka haluat siirtää toistoa. Voit tutustua IaaS näennäiskoneiden, jonka haluat siirtää Azure näennäiskoneiden yksityinen IP-osoitetta. Osoitteen etsiminen virtuaalikoneen Raporttinäkymät-ikkunan Azure-tietokannassa. Kun otat replikoinnin, voit määrittää tietokoneen tyyppi VMs fyysisten laitteiden nimellä.
7. [Suorita suunnittelematon automaattisesti](site-recovery-failover.md#run-an-unplanned-failover). Kun ensimmäisen replikoinnin on valmis, voit suorittaa suunnittelematon automaattisesti Azure alueelta toiseen. Voit vaihtoehtoisesti palautus Suunnittele ja näyttää suunnittelematon automaattisesti, jos haluat siirtää useita näennäiskoneiden alueiden välillä. [Lue lisää](site-recovery-create-recovery-plans.md) siitä, palautus-palvelupakettia.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja muiden replikoinnin skenaariot [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)
