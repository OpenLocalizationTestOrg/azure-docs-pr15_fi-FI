<properties
    pageTitle="Mikä on liikenteen hallinta | Microsoft Azure"
    description="Tässä artikkelissa auttaa sinua ymmärtämään liikenteen hallinta on ja onko sovelluksen oikean liikenne reititys-vaihtoehto"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="overview-of-traffic-manager"></a>Liikenne hallinnan yleiskatsaus

Microsoft Azure liikenteen hallinta avulla voit hallita käyttäjän liikenteen jakamista eri palvelinkeskusten Palvelupäätepisteet varten. Tuetut liikenteen hallinta päätepisteiden ovat Azure VMs Web Apps- ja cloud services. Voit käyttää myös liikenteen hallinta ulkoinen, Azure päätepisteet kanssa.

Liikenteen hallinta käytetään ohjaamaan asiakkaan pyyntöihin sopivimman päätepisteelle [liikenne reititys menetelmä](traffic-manager-routing-methods.md) ja päätepisteet kunto perusteella toimialueen nimi järjestelmän (DNS). Liikenteen hallinta tarjoaa alueen liikenne reititys tavoista sopivaksi toiseen sovellukseen tarpeet, päätepisteen kunnon [Seuranta](traffic-manager-monitoring.md)ja automaattinen automaattisesti. Liikenteen hallinta on virhe, virhe koko Azure alueen mukaan lukien joustavat.

## <a name="traffic-manager-benefits"></a>Liikenteen hallinta edut

Liikenteen hallinta auttaa sinua:

- **Tärkeiden sovellusten käytettävyyden parantaminen**

    Liikenteen hallinta toimittaa suuren käytettävyyden sovellusten valvonta oman päätepisteet ja antamisen automaattinen automaattisesti, kun seinän päätepiste siirtyy.

- **Paranna vastausajan tehokas sovellusten**

    Azure voit suorittaa pilvipalveluihin tai sivustojen palvelinkeskusten puolilla maailmaa. Liikenteen hallinta parantaa sovelluksen vastausajan ohjaamalla liikenne päätepisteelle asiakkaan kanssa pienin verkon latenssin.

- **Suorittaa ilman käyttökatkot palvelun ylläpito**

    Voit suorittaa sovellustesi ilman käyttökatkot suunniteltu ylläpito-toimintoja. Liikenteen hallinta ohjaa-liikenne paikalliseen vaihtoehtoinen päätepisteet ylläpidon ollessa käynnissä.

- **Paikallisen ja pilvipohjainen sovellusten yhdistäminen**

    Liikenteen hallinta tukee ulkoinen, -Azure päätepisteet käyttöönottamista voidaan käyttää hybrid cloud ja paikallisen käyttöönotoissa, kuten "burst-,-pilvipalveluun," "siirtää--pilvipalveluun," ja "automaattisesti pilven" skenaarioita.

- **Jakaa suuria ja monimutkaisia käyttöönotoissa liikenne**

    Käytä [sisäkkäisiä liikenteen hallinta-profiileista](traffic-manager-nested-profiles.md), liikenne reititys menetelmiä voidaan yhdistää luominen kehittyneitä ja joustavassa tukee suurempia, monimutkaisia käyttöönotoissa tarpeisiin.

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [liikenteen hallinta toiminta](traffic-manager-how-traffic-manager-works.md).

- Lue, miten suuren käytettävyyden-sovellusten käyttäminen [liikenteen hallinta päätepisteen seuranta](traffic-manager-monitoring.md).

- Lisätietoja [liikenne reititys menetelmiä](traffic-manager-routing-methods.md) tuettu liikenteen hallinta.

- [Luo liikenteen hallinta profiilin](traffic-manager-manage-profiles.md).

