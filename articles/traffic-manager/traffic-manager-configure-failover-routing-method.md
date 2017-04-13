<properties
   pageTitle="Määritä liikenteen hallinta automaattisesti liikenne reititys menetelmä | Microsoft Azure"
   description="Tämän artikkelin avulla voit määrittää automaattisesti liikenne reititys menetelmä liikenteen hallinta"
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

# <a name="configure-failover-routing-method"></a>Määritä automaattisesti reititys menetelmä

Usein organisaation haluaa antaa luotettavuutta palveluja. Se tekee tarjoamalla varmuuskopion palveluja siltä varalta, että hänen ensisijaiseen palveluun siirtyy. Palvelun automaattisesti yleistä mallia on identtiset services joukon ja liikenne lähettää ensisijainen-palveluun, säilyttäen varmuuskopion palveluista vähintään määritetyn luettelo. Voit määrittää tämän tyyppistä varmuuskopiointi Azure pilvipalveluihin ja sivustojen noudattamalla seuraavia ohjeita.

Huomaa, että Azure sivustojen on jo automaattisesti liikenne reititys menetelmä toimintoja sivustoissa sisällä palvelinkeskuksen (tunnetaan myös nimellä alueen), riippumatta siitä, sivuston-tilassa. Voit määrittää automaattisesti liikenne reititys menetelmä sivustot eri palvelinkeskusten liikenteen hallinta.

## <a name="to-configure-failover-traffic-routing-method"></a>Voit määrittää automaattisesti liikenne reititys menetelmä seuraavasti:

1. Valitse Azure perinteinen portaalissa vasemmassa ruudussa, voit avata liikenteen hallinta-ruudun **Liikenteen hallinta** -kuvake. Jos et ole vielä luonut liikenteen hallinta profiilin, katso [Hallinta liikenteen hallinta-profiileista](traffic-manager-manage-profiles.md) ohjeet basic liikenteen hallinta profiilin luominen.
2. Azure perinteinen portaalissa liikenteen hallinta-ruudun Etsi liikenteen hallinta profiilia, joka sisältää asetukset, jota haluat muokata, ja valitse sitten profiilin nimen oikealla puolella olevaa nuolta. Profiilin asetukset-sivu avautuu.
3. Valitse profiilisivullasi **päätepisteet** sivun yläosassa ja varmista, molemmat cloud services ja sivustoista (päätepisteet), jotka haluat sisällyttää kokoonpanoa ovat näkyvissä. Voit lisätä tai poistaa päätepisteet ohjeita on kohdassa [Hallinta päätepisteet liikenteen hallinta](traffic-manager-endpoints.md).
4. Valitse profiilisivullasi yläreunasta, kun haluat avata määritys-sivulla **Määritä** .
5. **Liikenne reititys asetukset**Varmista, että liikenne reititys menetelmä **automaattisesti**. Jos se ei ole, valitse avattavasta luettelosta **automaattisesti** .
6. **Automaattisesti prioriteetti-luettelossa**muuta oman päätepisteet automaattisesti tilauksen. Kun valitset **automaattisesti** liikenne reititys menetelmä, valitun päätepisteet järjestyksen koskevissa asioissa. Ensisijainen päätepiste on päällimmäisenä. Ylä- ja alanuolien avulla järjestystä tarpeen mukaan. Lisätietoja automaattisesti prioriteetti Windows PowerShellin avulla on artikkelissa [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Varmista, että **Valvonta-asetukset** on määritetty oikein. Seuranta varmistaa, että offline-tilassa päätepisteet ei lähetetä liikenne. Jotta voit seurata päätepisteet on määritettävä polku ja tiedostonimi. Huomaa, että vinoviivan "/" on suhteellinen polun merkintään ja tarkoittaa, että tiedosto on pääkansio (oletus). Saat lisätietoja valvonnasta [Liikenteen hallinta seuranta](traffic-manager-monitoring.md).
8. Kun olet suorittanut määritysten muutokset, valitse sivun alareunassa **Tallenna** .
9. Testaa muutokset kokoonpanoa. Lisätietoja on kohdassa [Testaus liikenteen hallinta-asetukset](traffic-manager-testing-settings.md) .
10. Kun liikenteen hallinta profiilin on määritys ja toimimasta, Muokkaa DNS-tietueen hallitsevan DNS-palvelimeen yrityksen toimialuenimen osoittamaan liikenteen hallinta toimialuenimi. Lisätietoja hallinnasta on [kohdassa yrityksen Internet-toimialueen liikenteen hallinta-toimialueeseen](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Seuraavat vaiheet

[Yrityksen Internet-toimialueen osoittamista toimialueeseen liikenteen hallinta](traffic-manager-point-internet-domain.md)

[Liikenteen hallinta reititys menetelmät](traffic-manager-routing-methods.md)

[PYÖRISTÄ-funktiota Mikko reititys menetelmä määrittäminen](traffic-manager-configure-round-robin-routing-method.md)

[Määritä suorituskyvyn reititys menetelmä](traffic-manager-configure-performance-routing-method.md)

[Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)

[Liikenteen hallinta - Disable, ota käyttöön tai poistaa seuraavasti:](disable-enable-or-delete-a-profile.md)

[Päätepisteen ottaminen käyttöön tai liikenteen hallinta - poistaminen käytöstä](disable-or-enable-an-endpoint.md)

