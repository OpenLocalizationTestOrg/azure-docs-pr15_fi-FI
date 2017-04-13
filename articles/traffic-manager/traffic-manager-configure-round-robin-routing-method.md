<properties
   pageTitle="Määritä liikenteen hallinta pyöreä Mikko liikenne reititys-menetelmä | Microsoft Azure"
   description="Tämän artikkelin avulla voit määrittää kuormituksen PYÖRISTÄ-funktiota Mikko liikenteen hallinta päätepisteiden."
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

# <a name="configure-round-robin-routing-method"></a>Määritä PYÖRISTÄ-funktiota Mikko reititys menetelmä

Yleisiä liikenne reititys menetelmä-kaava on antaa samanlaiset päätepisteet, jotka sisältävät pilvipalveluihin ja sivustot- ja liikenne lähettää kullekin pyöreän pöydän tavalla. Seuraavia ohjeita jäsennä määrittäminen liikenteen hallinta, jotta voit suorittaa tämän tyyppistä liikenne reititys menetelmä. Lisätietoja eri liikenne reititys menetelmistä on artikkelissa [liikenteen hallinta liikenne reititys tavoista](traffic-manager-routing-methods.md).

>[AZURE.NOTE] Azure sivustojen on jo pyöreän pöydän kuormituksen toimintoja sivustoissa sisällä palvelinkeskuksen (tunnetaan myös nimellä alue). Voit määrittää pyöreän pöydän liikenne reititys menetelmä sivustot eri palvelinkeskusten liikenteen hallinta.

## <a name="routing-traffic-equally-round-robin-across-a-set-of-endpoints"></a>Liikenne tasaisesti (pyöreä Mikko) reitittämisen päätepisteet joukko:

1. Valitse Azure perinteinen portaalissa vasemmassa ruudussa, voit avata liikenteen hallinta-ruudun **Liikenteen hallinta** -kuvake. Jos et ole vielä luonut liikenteen hallinta profiilin, katso [Hallinta liikenteen hallinta-profiileista](traffic-manager-manage-profiles.md) ohjeet basic liikenteen hallinta profiilin luominen.
2. Azure perinteinen portaalissa liikenteen hallinta-ruudun Etsi liikenteen hallinta profiilia, joka sisältää asetukset, jota haluat muokata, ja valitse sitten profiilin nimen oikealla puolella olevaa nuolta. Profiilin asetukset-sivu avautuu.
3. Valitse profiili-sivulla **päätepisteet** sivun yläreunassa ja varmista, että Palvelupäätepisteet, jotka haluat sisällyttää kokoonpanoa ovat näkyvissä. Voit lisätä tai poistaa päätepisteet ohjeita on kohdassa [Hallinta päätepisteet liikenteen hallinta](traffic-manager-endpoints.md).
4. Valitse profiilisivullasi yläreunasta, kun haluat avata määritys-sivulla **Määritä** .
5. **Liikenne reititys menetelmä asetukset**Varmista, että liikenne reititys menetelmä on **PYÖRISTÄ-funktiota Mikko**. Jos se ei ole, valitse **PYÖRISTÄ-funktiota Mikko** avattavasta luettelosta.
6. Varmista, että **Valvonta-asetukset** on määritetty oikein. Seuranta varmistaa, että offline-tilassa päätepisteet ei lähetetä liikenne. Jotta voit seurata päätepisteet on määritettävä polku ja tiedostonimi. Huomaa, että vinoviivan "/" on suhteellinen polun merkintään ja tarkoittaa, että tiedosto on pääkansio (oletus). Saat lisätietoja valvonnasta [Liikenteen hallinta seuranta](traffic-manager-monitoring.md).
7. Kun olet suorittanut määritysten muutokset, valitse sivun alareunassa **Tallenna** .
8. Testaa muutokset kokoonpanoa. Lisätietoja on artikkelissa [Testaaminen liikenteen hallinta-asetukset](traffic-manager-testing-settings.md).
9. Kun liikenteen hallinta profiilin on määritys ja toimimasta, Muokkaa DNS-tietueen hallitsevan DNS-palvelimeen yrityksen toimialuenimen osoittamaan liikenteen hallinta toimialuenimi. Lisätietoja hallinnasta on [kohdassa yrityksen Internet-toimialueen liikenteen hallinta-toimialueeseen](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Seuraavat vaiheet


[Yrityksen Internet-toimialueen osoittamista toimialueeseen liikenteen hallinta](traffic-manager-point-internet-domain.md)

[Liikenteen hallinta reititys menetelmät](traffic-manager-routing-methods.md)

[Määritä automaattisesti reititys menetelmä](traffic-manager-configure-failover-routing-method.md)

[Määritä suorituskyvyn reititys menetelmä](traffic-manager-configure-performance-routing-method.md)

[Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)

[Liikenteen hallinta - Disable, ota käyttöön tai poistaa seuraavasti:](disable-enable-or-delete-a-profile.md)

[Päätepisteen ottaminen käyttöön tai liikenteen hallinta - poistaminen käytöstä](disable-or-enable-an-endpoint.md)

