<properties
    pageTitle="Määritä liikenteen hallinta reititys menetelmiä | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, miten voit määrittää reititys kolmella eri tavalla liikenteen hallinta"
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
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Määritä liikenteen hallinta reititys menetelmät

Azure liikenteen hallinta sisältää kolme reititys tapaa, jotka määrittävät, kuinka liikenne reititetään käytettävissä Palvelupäätepisteet. Kukin DNS-kysely voit selvittää, mitkä päätepisteen palautetaan vastauksessa DNS vastaanotetut otetaan käyttöön liikenne reititys-menetelmää.

Liikenne reititys kolmella ovat käytettävissä liikenteen hallinta:

- **Prioriteetti:** Valitse "Prioriteetti", kun haluat käyttää ensisijainen palvelupäätepiste ja anna varmuuskopiot, jos ensisijainen ei ole käytettävissä.
- **Painotetun:** Valitse "painotettu", jos haluat jakaa liikenne päätepisteet joukko, joko tasaisesti tai mukaan paksuuksia ja jonka voi määrittää.
- **Suorituskyvyn:** Valitse "Suorituskyvyn", kun päätepisteet ovat eri Maantieteellisten sijaintien ja haluat, että käyttäjät voivat käyttää pienin verkon latenssin kannalta "lähinnä" päätepistettä.

## <a name="configure-priority-routing-method"></a>Määritä prioriteetti reititys menetelmä

Sivuston-tilassa, vaikka Azure sivustojen jo automaattisesti toimintojen tarjoamista sivustot sisällä palvelinkeskuksen (tunnetaan myös nimellä alue). Liikenteen hallinta tarjoaa automaattisesti eri palvelinkeskusten sivustot.

Palvelun automaattisesti yleistä mallia myös liikenne lähettää ensisijainen palveluun ja antaa samanlaiset varmuuskopion palvelut automaattisesti. Seuraavassa kerrotaan, kuinka määrittää tämän Priorisoidut automaattisesti Azure pilvipalveluihin ja verkkosivustot:

1. Valitse Azure perinteinen portaalissa vasemmassa ruudussa, voit avata liikenteen hallinta-ruudun **Liikenteen hallinta** -kuvake.
2. Etsi Azure perinteinen portaalin liikenteen hallinta-ruudussa liikenteen hallinta profiilia, joka sisältää asetukset, jota haluat muokata. Avaa profiilin asetukset-sivun, valitse profiilin nimen oikealla puolella olevaa nuolta.
3. Valitse profiilisivullasi **päätepisteet** sivun yläreunassa. Varmista, että pilvipalveluiden ja sivustoista, jotka haluat sisällyttää kokoonpanoa ovat näkyvissä.
4. Valitse **Määritä** yläreunasta, kun haluat avata määritys-sivulla.
5. **Liikenne reititys asetukset**Varmista, että liikenne reititys menetelmä **automaattisesti**. Jos se ei ole, valitse avattavasta luettelosta **automaattisesti** .
6. **Automaattisesti prioriteetti-luettelossa**muuta oman päätepisteet automaattisesti tilauksen. Kun valitset **automaattisesti** liikenne reititys menetelmä, valitun päätepisteet järjestyksen koskevissa asioissa. Ensisijainen päätepiste on päällimmäisenä. Ylä- ja alanuolien avulla järjestystä tarpeen mukaan. Lisätietoja automaattisesti prioriteetti Windows PowerShellin avulla on artikkelissa [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Varmista, että **Valvonta-asetukset** on määritetty oikein. Seuranta varmistaa, että offline-tilassa päätepisteet ei lähetetä liikenne. Voit valvoa päätepisteet on määritettävä polku ja tiedostonimi. A vinoviiva "/" on suhteellinen polun merkintään ja tarkoittaa, että tiedosto on pääkansio (oletus).
8. Kun olet suorittanut määritysten muutokset, valitse sivun alareunassa **Tallenna** .
9. Testaa muutokset kokoonpanoa.
10. Kun liikenteen hallinta profiilin toimi, Muokkaa DNS-tietueen hallitsevan DNS-palvelimeen yrityksen toimialuenimen osoittamaan liikenteen hallinta toimialuenimi.

## <a name="configure-weighted-routing-method"></a>Painotetun reititys menetelmän määrittäminen

Yleisiä liikenne reititys menetelmä-kaava on antaa samanlaiset päätepisteet, jotka sisältävät pilvipalveluihin ja sivustot- ja liikenne lähettää kullekin pyöreän pöydän tavalla. Seuraavissa vaiheissa kuvataan määrittäminen tällaista liikenne reititys-menetelmää.

>[AZURE.NOTE] Azure sivustojen on jo pyöreän pöydän kuormituksen toimintoja sivustoissa sisällä palvelinkeskuksen (tunnetaan myös nimellä alue). Voit määrittää pyöreän pöydän liikenne reititys menetelmä sivustot eri palvelinkeskusten liikenteen hallinta.

1. Valitse Azure perinteinen portaalissa vasemmassa ruudussa, voit avata liikenteen hallinta-ruudun **Liikenteen hallinta** -kuvake.
2. Etsi liikenteen hallinta-ruudussa liikenteen hallinta profiilia, joka sisältää asetukset, jota haluat muokata. Avaa profiilin asetukset-sivun, valitse profiilin nimen oikealla puolella olevaa nuolta.
3. Valitse profiili-sivulla **päätepisteet** sivun yläreunassa ja varmista, että Palvelupäätepisteet, jotka haluat sisällyttää kokoonpanoa ovat näkyvissä.
4. Valitse profiilisivullasi yläreunasta, kun haluat avata määritys-sivulla **Määritä** .
5. **Liikenne reititys menetelmä asetukset**Varmista, että liikenne reititys menetelmä on **PYÖRISTÄ-funktiota Mikko**. Jos se ei ole, valitse **PYÖRISTÄ-funktiota Mikko** avattavasta luettelosta.
6. Varmista, että **Valvonta-asetukset** on määritetty oikein. Seuranta varmistaa, että offline-tilassa päätepisteet ei lähetetä liikenne. Voit valvoa päätepisteet on määritettävä polku ja tiedostonimi. A vinoviiva "/" on suhteellinen polun merkintään ja tarkoittaa, että tiedosto on pääkansio (oletus).
7. Kun olet suorittanut määritysten muutokset, valitse sivun alareunassa **Tallenna** .
8. Testaa muutokset kokoonpanoa.
9. Kun liikenteen hallinta profiilin toimi, Muokkaa DNS-tietueen hallitsevan DNS-palvelimeen yrityksen toimialuenimen osoittamaan liikenteen hallinta toimialuenimi.

## <a name="configure-performance-traffic-routing-method"></a>Määritä suorituskyvyn liikenne reititys-menetelmällä

Suorituskyvyn liikenne reititys menetelmä mahdollistaa liikenne voidaan ohjata päätepisteelle ja alimman viive asiakkaan verkosta. Yleensä palvelinkeskuksen ja alimman viive on lähin maantieteelliset etäisyys. Reitittää liikenteen tätä menetelmää et voi tili, verkon määritysten reaaliaikainen muutokset tai Lataa.

1. Valitse Azure perinteinen portaalissa vasemmassa ruudussa, voit avata liikenteen hallinta-ruudun **Liikenteen hallinta** -kuvake.
2. Etsi liikenteen hallinta-ruudussa liikenteen hallinta profiilia, joka sisältää asetukset, jota haluat muokata. Avaa profiilin asetukset-sivun, valitse profiilin nimen oikealla puolella olevaa nuolta.
3. Valitse profiili-sivulla **päätepisteet** sivun yläreunassa ja varmista, että Palvelupäätepisteet, jotka haluat sisällyttää kokoonpanoa ovat näkyvissä.
4. Valitse profiili-sivulla **Määritä** yläreunasta, kun haluat avata määritys-sivulla.
5. **Liikenne reititys asetukset**Varmista, että liikenne reititys menetelmä * *suorituskykyä*. Jos se ei ole, valitse * *suorituskyvyn** avattavassa luettelossa.
6. Varmista, että **Valvonta-asetukset** on määritetty oikein. Seuranta varmistaa, että offline-tilassa päätepisteet ei lähetetä liikenne. Voit valvoa päätepisteet on määritettävä polku ja tiedostonimi. A vinoviiva "/" on suhteellinen polun merkintään ja tarkoittaa, että tiedosto on pääkansio (oletus).
7. Kun olet suorittanut määritysten muutokset, valitse sivun alareunassa **Tallenna** .
8. Testaa muutokset kokoonpanoa.
9. Kun liikenteen hallinta profiilin toimi, Muokkaa DNS-tietueen hallitsevan DNS-palvelimeen yrityksen toimialuenimen osoittamaan liikenteen hallinta toimialuenimi.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Liikenteen hallinta-profiileista hallinta](traffic-manager-manage-profiles.md)
* [Liikenteen hallinta reititys menetelmät](traffic-manager-routing-methods.md)
* [Testauksessa liikenteen hallinta-asetukset](traffic-manager-testing-settings.md)
* [Yrityksen Internet-toimialueen osoittamista toimialueeseen liikenteen hallinta](traffic-manager-point-internet-domain.md)
* [Liikenteen hallinta päätepisteet hallinta](traffic-manager-manage-endpoints.md)
* [Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)
