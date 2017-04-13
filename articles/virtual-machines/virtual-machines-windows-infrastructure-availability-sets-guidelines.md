<properties
    pageTitle="Käytettävyys määrittää ohjeita | Microsoft Azure"
    description="Lisätietoja käyttöönoton käytettävyys joukot Azure infrastruktuuripalvelut keskeisiä suunnittelu ja käyttöönotto ohjeita."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="availability-sets-guidelines"></a>Käytettävyys määrittää ohjeet

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Tässä artikkelissa keskitytään tietoja tarvittavat suunnittelun vaiheet käytettävyys joukkojen voit varmistaa, että sovellusten pysyy käytettävissä suunniteltu tai suunnittelematon tapahtumien aikana.

## <a name="implementation-guidelines-for-availability-sets"></a>Toteutuksen jälkeisen ohjeet käytettävyys joukot

Päätökset:

- Kuinka monta käytettävyys joukot tarvitsee eri rooleja ja tasojen-sovelluksen infrastruktuurin?

Tehtävät:

- Määritä vaadit sovellusta kunkin tason VMs määrä.
- Määrittää, jos haluat muuttaa vika lukumäärää tai päivitä toimialueet, jota käytetään sovelluksen.
- Pakollinen käytettävyys määrittää nimeämiskäytännön ja mitä ne sijaitsevat VMs. AM voi olla vain yksi käytettävyys määrittäminen. 

## <a name="availability-sets"></a>Käytettävyys joukot

Azure-tietokannassa näennäiskoneiden (VMs) voidaan sijoittaa looginen ryhmittely, kutsutaan käytettävyys määrittäminen. Kun luot VMs käytettävyys asettaa, Azure ympäristö jakaa näiden VMs sijaintia pohjana oleva infrastruktuuri. Olisi suunnitellun ylläpitotapahtuman Azure ympäristö tai pohjana olevan laitteen / infrastruktuuri vika käytettävyys joukot käyttö varmistaa vähintään yksi AM pysyy käynnissä.

Paras käytäntö on sovellusten olisi ei sijaitsevat yksittäisen AM. Käytettävyys-joukon, joka sisältää yksittäisen AM ei yhden suojaa suunniteltu tai suunnittelematon tapahtumien Azure ympäristössä. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) edellyttää vähintään kaksi VMs määritetty sallimaan VMs jakautumisen yli pohjana oleva infrastruktuuri käytettävyyden puitteissa.

Pohjana oleva infrastruktuuri Azure-tietokannassa on jaettu päivittämään toimialueet ja vika toimialueet. Nämä toimialueet on määritetty mukaan mitä isännät Jaa yleisiä päivityksen jakson tai Jaa samalla fyysinen infrastruktuurin kuten power ja verkko. Azure jakaa automaattisesti oman VMs asettaminen toimialueilla säilyttää käytettävyys ja vikasietoa saatavuus kuluessa. Sen mukaan, kokoa ja sovelluksen VMs numeroinnin käytettävyys-joukko voit säätää toimialueet, joita haluat käyttää määrän. Voit lukea lisää [hallinta käytettävyys](virtual-machines-windows-manage-availability.md)ja Päivitä ja vika-toimialueiden käyttöä.

Sovelluksen infrastruktuurin suunnitellessasi sovellusta tasoa, jota käytetään myös kannattaa suunnitella. Ryhmän VMs, jotka vastaavat sama aihe-käyttöön määrittää esimerkiksi määrittää oman edusta virtuaalilaitteiksi IIS saatavuus. Luo erillisen käytettävyys, että taustatietokantaa VMs SQL Serveriä varten. Tavoitteena on varmistaa, että kunkin sovelluksen osan on suojattu käytettävyyden määrittäminen ja vähintään kerran esiintymän aina pysyy käynnissä.

Kuormituksen tasoitusmääritykset voidaan käyttää rinnalla käytettävyys määrittäminen toimimaan ja varmistaa liikenne voidaan reitittää aina suoritettavan sovelluksen kunkin tason eteen. Ilman kuormituksen että VMs saattaa Jatka koko suunnitellut ja suunnittelemattomat ylläpito tapahtumia, mutta käyttäjiesi ei voi ratkaista ne, jos ensisijainen AM ei ole käytettävissä.


## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 