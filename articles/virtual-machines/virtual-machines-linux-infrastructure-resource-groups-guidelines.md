<properties
    pageTitle="Resurssin ryhmittelee ohjeet | Microsoft Azure"
    description="Lisätietoja käyttöönoton resurssiryhmät Azure infrastruktuuripalvelut keskeisiä suunnittelu ja käyttöönotto ohjeita."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="azure-resource-group-guidelines"></a>Azure resurssien ryhmä-ohjeet

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Tässä artikkelissa keskitytään tietoja loogisesti muodostetaan ympäristön ja ryhmitellä kaikki komponentit resurssin ryhmissä.


## <a name="implementation-guidelines-for-resource-groups"></a>Toteutuksen jälkeisen ohjeet resurssiryhmät

Päätökset:

- Oletko muodostetaan resurssiryhmät core infrastruktuurin osien vai täydellinen sovelluksen käyttöönottoa?
- Sinun on rajoittamisesta resurssin ryhmiin Roolipohjainen käytön asetusten avulla

Tehtävät:

- Määritä mitä core infrastruktuurin ja varatun resurssiryhmää tarvitset.
- Tarkista Resurssienhallinta malleja yhdenmukaisia, toistettavissa käyttöönotoissa ottamisesta käyttöön.
- Määritä mitä access käyttäjäroolit tarvitset ohjaukseen resurssiryhmien käyttöä.
- Luo resurssiryhmien nimeämiskäytännön mukaisesti. Voit käyttää Azure CLI tai portaalin.


## <a name="resource-groups"></a>Resurssiryhmät

Azure-tietokannassa voit ryhmitellä loogisesti liittyvät resurssit, kuten tallennustilan asiakkaat, virtual verkkojen ja ottaa käyttöön, hallita ja säilyttää ne yhteen kokonaisuutena näennäiskoneiden (VMs). Tämän menetelmän on helppo, voit asentaa sovellukset ja säilyttää kaikki liittyvät resurssit yhdessä hallinta näkökulmasta tai muiden resurssien ryhmän käyttöoikeuden myöntäminen. Resurssin ryhmien monipuolisemman tietoja, voit lukea [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md).

Resurssin ryhmiin avaimen ominaisuus on mahdollisuus muodosta JSON-tiedoston, joka ilmoittaa tallennustilan verkko-ympäristön ja Laske resurssit. Voit myös määrittää Aiheeseen liittyviä mukautettuja komentosarjoja tai käyttää määrityksiä. JSON-mallien avulla luot yhdenmukaisia, toistettavissa käyttöönotoissa sovellusten. Tämän menetelmän avulla voit luoda ympäristössä kehitteillä ja luominen tuotantoympäristössä saman mallin avulla ja päinvastoin. Paremmin ymmärtämään mallien käyttämisestä, lue [mallia vaiheittainen kuvaus](../resource-manager-template-walkthrough.md) , joka opastaa kunkin vaiheen rakennuksen ulos JSON-malli.

Käytettävissä on kaksi eri tapaa niihin ympäristösi resurssin ryhmiin suunniteltaessa:

- Resurssiryhmät kunkin sovelluksen käyttöönottoa, joka yhdistää tallennustilan tilit, virtual verkkojen ja aliverkosta, VMs, ladata tasoitusmääritykset jne.
- Keskitetty resurssin ryhmiin, jotka sisältävät core virtual verkko ja aliverkosta tai tallennustilan tilit. Sovellukset ovat sitten oman resurssin ryhmiin, jotka sisältävät vain VMs kuormituksen tasoitusmääritykset, verkkoliittymät jne.

Kun olet mittakaava, keskitetty virtual verkko-ja aliverkosta resurssiryhmien luomisesta on helppo luoda paikallisen-verkon hybrid yhteysasetukset yhteydet. Vaihtoehtoinen menetelmä on kunkin sovelluksen omia virtual verkosto, joka edellyttää määritys ja ylläpitoa. [Roolipohjainen käytön ohjausobjekteja](../active-directory/role-based-access-control-what-is.md) voi hajautetun resurssiryhmien käyttöoikeuksien hallinta. Tuotannon-sovelluksia voit määrittää käyttäjät, jotka voivat käyttää resursseja tai core infrastruktuurin resurssien voit rajata vain infrastruktuurin insinöörien niiden käyttäminen helpottuisi. Sovelluksen omistajat pystyt käyttämään niiden resurssiryhmä ja ei core Azure infrastruktuurin ympäristön sovelluksen osia. Kun suunnittelet ympäristön, kannattaa ehkä käyttäjät, jotka edellyttävät resursseja ja suunnitella resurssiryhmien vastaavasti. 


## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 