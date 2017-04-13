<properties
   pageTitle="Määritä suorituskyvyn liikenne reititys-menetelmä | Microsoft Azure"
   description="Tämän artikkelin avulla voit määrittää suorituskyvyn liikenne reititys menetelmä liikenteen hallinta"
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-performance-traffic-routing-method"></a>Määritä suorituskyvyn liikenne reititys-menetelmällä

Reitittää liikenteen pilvipalveluihin ja sivustojen (päätepisteet), jotka sijaitsevat eri palvelinkeskusten yli maapallo (tunnetaan myös nimellä alueet), jotta voit ohjata saapuvan liikenteen päätepisteelle ja alimman viive pyytävä asiakasohjelmassa. Yleensä palvelinkeskuksen ja alimman viive vastaa lähin-maantieteelliset etäisyys. Suorituskyvyn liikenne reititys menetelmä ansiosta voit jakaa pienin viive perusteella, mutta ei voi ottaa huomioon reaaliaikainen muutokset verkon määritysten tai Lataa. Lisätietoja eri liikenne reititys menetelmiä, joka sisältää Azure liikenteen hallinta-kohdassa [liikenteen hallinta liikenne reititys menetelmiä](traffic-manager-routing-methods.md).

## <a name="route-traffic-based-on-lowest-latency-across-a-set-of-endpoints"></a>Reitin liikenne pienin viive joukon päätepisteet perusteella:

1. Valitse Azure perinteinen portaalissa vasemmassa ruudussa, voit avata liikenteen hallinta-ruudun **Liikenteen hallinta** -kuvake. Jos et ole vielä luonut liikenteen hallinta profiilin, katso [Hallinta liikenteen hallinta-profiileista](traffic-manager-manage-profiles.md) vaiheet basic liikenteen hallinta profiilin luominen.
2. Azure perinteinen portaalissa liikenteen hallinta-ruudun Etsi liikenteen hallinta profiilia, joka sisältää asetukset, jota haluat muokata, ja valitse sitten profiilin nimen oikealla puolella olevaa nuolta. Profiilin asetukset-sivu avautuu.
3. Valitse profiili-sivulla **päätepisteet** sivun yläreunassa ja varmista, että Palvelupäätepisteet, jotka haluat sisällyttää kokoonpanoa ovat näkyvissä. Voit lisätä tai poistaa päätepisteet profiilin ohjeita on kohdassa [Hallinta päätepisteet liikenteen hallinta](traffic-manager-endpoints.md).
4. Valitse profiili-sivulla **Määritä** yläreunasta, kun haluat avata määritys-sivulla.
5. **Liikenne reititys asetukset**Varmista, että liikenne reititys menetelmä * *suorituskykyä*. Jos se ei ole, valitse * *suorituskyvyn** avattavassa luettelossa.
6. Varmista, että **Valvonta-asetukset** on määritetty oikein. Seuranta varmistaa, että offline-tilassa päätepisteet ei lähetetä liikenne. Jotta voit seurata päätepisteet on määritettävä polku ja tiedostonimi. Huomaa, että vinoviivan "/" on suhteellinen polun merkintään ja tarkoittaa, että tiedosto on pääkansio (oletus). Saat lisätietoja valvonnasta [Liikenteen hallinta seuranta](traffic-manager-monitoring.md).
7. Kun olet suorittanut määritysten muutokset, valitse sivun alareunassa **Tallenna** .
8. Testaa muutokset kokoonpanoa. Lisätietoja on artikkelissa [Testaaminen liikenteen hallinta-asetukset](traffic-manager-testing-settings.md).
9. Kun liikenteen hallinta profiilin on määritys ja toimimasta, Muokkaa DNS-tietueen hallitsevan DNS-palvelimeen yrityksen toimialuenimen osoittamaan liikenteen hallinta toimialuenimi. Lisätietoja hallinnasta on [kohdassa yrityksen Internet-toimialueen liikenteen hallinta-toimialueeseen](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Seuraavat vaiheet


[Yrityksen Internet-toimialueen osoittamista toimialueeseen liikenteen hallinta](traffic-manager-point-internet-domain.md)

[Liikenteen hallinta reititys menetelmät](traffic-manager-routing-methods.md)

[Määritä automaattisesti reititys menetelmä](traffic-manager-configure-failover-routing-method.md)

[PYÖRISTÄ-funktiota Mikko reititys menetelmä määrittäminen](traffic-manager-configure-round-robin-routing-method.md)

[Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)

[Liikenteen hallinta - Disable, ota käyttöön tai poistaa seuraavasti:](disable-enable-or-delete-a-profile.md)

[Päätepisteen ottaminen käyttöön tai liikenteen hallinta - poistaminen käytöstä](disable-or-enable-an-endpoint.md)

