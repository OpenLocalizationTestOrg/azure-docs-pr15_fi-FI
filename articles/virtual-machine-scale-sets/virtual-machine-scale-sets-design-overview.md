<properties
    pageTitle="Suunnittelet virtuaalikoneen asteikko määrittää mittakaava | Microsoft Azure"
    description="Lue, miten voit suunnitella virtuaalikoneen asteikko-joukot akselille"
    keywords="Linux virtuaalikoneen virtuaalikoneen asteikko määrittää" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Suunnittelet AM asteikko määrittää asteikko

Tässä ohjeaiheessa kerrotaan tyyliseikat virtuaalikoneen asteikko joukkojen. Lisätietoja virtuaalikoneen asteikko joukot ovat viitata [Virtuaalikoneen asteikko joukot yleiskatsaus](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Tallennustilan

Mittakaava-joukon käyttää tallennustilan tilit VMs OS levyille joukon. On suositeltavaa suhde on 20 VMs tiliä ja tallennustilaa tai vähemmän. On myös suositeltavaa, että voit levittää yli aakkosten alkuun merkit tallennustilan tilin nimi. Yhteystietokorttiisi auttaa levitä kuormituksen eri sisäinen järjestelmiin. Esimerkiksi seuraavat mallin avulla uniqueString Resurssienhallinta mallissa funktiota Luo etuliite hajautusarvot, joka on käyttää kaikkien tallennustilan tilin nimi: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Overprovisioning

"2016-03-30" API-versio alkaen AM asteikko joukot oletusarvoisesti "overprovisioning" VMs. Kun overprovisioning otettu käyttöön, asteikon määrittäminen todella kierrosta määrittäminen Lisää VMs kuin kysyy ja valitse poistaa ylimääräiset VMs, kehrätty viimeksi ylöspäin. Overprovisioning parantaa valmistelu onnistui-kurssit. Ovat ei veloittaa nämä ylimääräiset VMs ja niiden Laske kohti Kiintiörajoitukset.

Kun overprovisioning parantaa valmistelu success korvaukset, se saattaa aiheuttaa sovellus, joka ei ole suunniteltu käsittelemään VMs katoaa ilmoittamatta sekava toiminta. Jos haluat poistaa käytöstä overprovisioning, varmista sinulla seuraava merkkijono mallin: "overprovision": "false". Lisätietoja löytyy [AM asteikko määrittäminen REST API-ohjeissa](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Jos overprovisioning poistetaan käytöstä, saa heti suurempi suhde on VMs tiliä ja tallennustilaa, mutta ei ole suositeltavaa siirtyä yli 40.


## <a name="limits"></a>Rajoitukset
Rakennettu mukautetun kuvan (yksi laadittuihin sinä) asteikon joukon on luotava kaikki OS levyn näennäiskiintolevyjen yhden tallennustilan tilin. Tuloksena suositeltu VMs mukautetun kuvan rakennettu asteikko joukon määrä enimmäismäärä on 20. Jos poistat käytöstä overprovisioning, voit siirtyä jopa 40.

Joukon asteikko-ympäristö kuva rakennettu on tällä hetkellä rajoitettu 100 VMs (Suosittelemme tämän asteikko 5 tallennustilan tilit).

Saat lisätietoja VMs kuin nämä raja-arvot käyttöön, sinun täytyy käyttöönotto asteikko useita ehtojoukkoja, [tämän mallin](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale)mukaisesti.