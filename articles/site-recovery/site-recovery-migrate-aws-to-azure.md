<properties
    pageTitle="Windows-näennäiskoneiden siirtäminen Azure kanssa palauttaminen Amazon verkkopalvelut | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Windows näennäiskoneiden Amazon Web Services (AWA) Azure Azure palauttaminen avulla voit siirtää."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/22/2016"
    ms.author="raynew"/>

#  <a name="migrate-windows-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Windows näennäiskoneiden-Amazon Web Services (sisältyy AWS) siirtäminen Azure kanssa Azure sivuston palauttaminen

## <a name="overview"></a>Yleiskatsaus

Tervetuloa käyttämään Azure palauttaminen. Tämän artikkelin avulla voit siirtää käynnissä sisältyy AWS, palauttaminen ja Azure Windows-esiintymät. Ennen kuin aloitat, Huomaa seuraavat seikat:

- Azure on kaksi eri käyttöönoton mallien luominen ja käyttäminen resurssit: Azure Resurssienhallinta ja perinteinen. Azure on myös kaksi portaaleihin – Azure perinteinen portaalin, joka tukee perinteinen käyttöönottomalli ja Azure-portaalin tukeen käyttöönoton sekä malleille. Siirron perusvaiheet ovat samoja, vaikka olet määrittäminen sivuston palauttaminen resurssien hallinnan tai Classic-ohjelmassa. Kuitenkin Käyttöliittymän ohjeet ja näyttökuvat tämän artikkelin liittyvät Azure portaalin.
- **Tällä hetkellä voi vain siirtää sisältyy AWS Azure avulla. Voi epäonnistua ja Azure-sisältyy AWS VMs päälle, mutta voit et voi epäonnistua ne takaisin uudelleen. Ei ole jatkuvan replikoinnin.**
- Tässä artikkelissa annettuja ohjeita siirron perustuvat ohjeet replikoiminen fyysinen tietokoneen Azure. Se sisältää vaiheet linkkejä [replikoida VMware VMs tai fyysinen Azure-palvelinten](site-recovery-vmware-to-azure.md), jossa kerrotaan, miten voit toistaa fyysinen server Azure-portaalissa.
- Jos olet määrittämässä palauttaminen perinteinen-portaalissa, noudata [tämän](site-recovery-vmware-to-azure-classic.md)artikkelin yksityiskohtaisia ohjeita. **Sinun on käytettävä enää** tätä [Vanha artikkelissa](site-recovery-vmware-to-azure-classic-legacy.md)annettuja ohjeita.

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus palvelut-keskustelupalsta](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)


## <a name="prerequisites"></a>Edellytykset

Näin tarvitset tätä käyttöönottoa varten

- **Määrityspalvelimen**: käytössä Windows Server 2012 R2, joka toimii määrityspalvelimen paikallisen-AM. Asennat muiden sivustojen palauttamisen osien (mukaan lukien prosessi-palvelimen nimen ja perusmuodon kohdepalvelin) tämä AM liian. Lue lisää [skenaarion arkkitehtuuri](site-recovery-vmware-to-azure.md#scenario-architecture) ja [määritykset palvelimen edellytykset](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **EC2 AM esiintymät**: käyttöjärjestelmässä esiintymät, jotka haluat siirtää.

## <a name="deployment-steps"></a>Käyttöönoton vaiheet

Tässä osassa kuvataan uuden Azure portaalin käyttöönoton ohjeita. Jos tarvitset käyttöönoton seuraavasti palauttaminen perinteinen-portaalissa, katso [Tässä artikkelissa](site-recovery-vmware-to-azure-classic.md).

1. [Luo säilöön](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Ota käyttöön configuration-palvelin](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Kun olet käyttöön configuration-palvelin, Vahvista, että se yhteyden kanssa VMs, jonka haluat siirtää.
4. [Replikoinnin asetusten määrittäminen](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Replikoinnin-käytännön luominen ja liittää määrityspalvelimen.
5. [Mobility-palvelun asentaminen](site-recovery-vmware-to-azure.md#step-6-replication-application). Kunkin AM, jonka haluat suojata on asennettu Mobility-palvelu. Tämä palvelu lähettää tietoja prosessi-palvelimeen. Mobility-palvelun voidaan asentaa manuaalisesti tai miten ja asennu automaattisesti prosessi-palvelin AM suojaus on käytössä. Palomuurisäännöt EC2 esiintymät, jotka haluat siirtää on määritetty sallimaan tämän palvelun push-asennuksen. Käyttöoikeusryhmän EC2 esiintymien pitäisi olla seuraavat säännöt:

    ![palomuurisäännöt](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)

6. [Ota käyttöön replikoinnin](site-recovery-vmware-to-azure.md#enable-replication). Ota käyttöön VMs, jotka haluat siirtää toistoa. Voit tutustua käyttämällä yksityinen IP-osoitteet, johon pääset EC2-konsolin EC2 esiintymät.
7. [Suorita suunnittelematon automaattisesti](site-recovery-failover.md#run-an-unplanned-failover). Kun ensimmäisen replikoinnin on valmis, voit suorittaa suunnittelematon automaattisesti sisältyy AWS-Azure kunkin AM varten. Voit vaihtoehtoisesti palautus Suunnittele ja näyttää suunnittelematon automaattisesti, jos haluat siirtää useita näennäiskoneiden sisältyy AWS Azure. [Lue lisää](site-recovery-create-recovery-plans.md) siitä, palautus-palvelupakettia.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja muiden replikoinnin skenaariot [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)
