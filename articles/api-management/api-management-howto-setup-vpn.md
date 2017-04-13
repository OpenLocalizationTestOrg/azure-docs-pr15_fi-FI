<properties
    pageTitle="VPN-yhteydet Azure API hallinnan määrittäminen"
    description="Opi määrittämään Azure API hallinta ja access web Services-palveluissa kulkee VPN-yhteyden."
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="antonba"/>

# <a name="how-to-setup-vpn-connections-in-azure-api-management"></a>VPN-yhteydet Azure API hallinnan määrittäminen

API hallinta VPN-tuen avulla voit muodostaa API hallinta-yhdyskäytävän Azure-verkkoon virtuaalisia (Klassinen). Näin API hallinta asiakkaat muodostamaan yhteyden niiden Taustajärjestelmä verkkopalvelut, jotka ovat paikallisen tai ovat käytettävissä vain Julkinen Internet suojatusti.

>[AZURE.NOTE] Azure API hallinta toimii perinteinen VNETs. Lisätietoja perinteinen VNET luomisesta on artikkelissa [luominen (perinteinen) käyttämällä Azure-portaalin virtual verkkoon](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Lisätietoja yhdistämisestä perinteinen VNETs ARM VNETS on ohjeaiheessa [yhteyden perinteinen VNets, uusi VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="enable-vpn"> </a>Käyttöön VPN-yhteydet

>VPN-yhteys on käytettävissä vain **Premium** ja **Developer** -tasoa. Jos haluat vaihtaa sitä, Avaa API hallintapalvelun [Azure perinteinen Portal][] ja avaa sitten **Mittakaava** -välilehti. Valitse **Yleiset** -osaan Premium taso ja sitten Tallenna.

Jotta VPN-yhteys, Avaa API hallintapalvelun [Azure perinteinen Portal][] ja **Määritä** -välilehteen. 

VPN-kohdassa Vaihda **VPN-yhteyden** **käyttöön**.

![Määritä API hallinta esiintymä-välilehti][api-management-setup-vpn-configure]

Nyt näet luettelon kaikkien alueiden, jossa API hallintapalvelun valmistelun yhteydessä.

Valitse VPN- ja aliverkon jokaisen alue. Luettelon VPN-yhteydet täytetään virtual verkkojen käytettävissä Azure-tilaukseesi, jotka on määritetty määritetään alueen perusteella.

![Valitse VPN][api-management-setup-vpn-select]

Valitse näytön alareunassa **Tallenna** . Sinulla voi tehdä muita toimia API hallintapalvelun perinteinen Azure-portaalista samalla, kun se päivitetään. Palvelun yhdyskäytävän ne ovat käytettävissä ja runtime puhelut ei vaikuta.

Huomaa, että yhdyskäytävän VIP-osoitteeseen muuttuu joka kerta VPN käytössä vai ei.

## <a name="connect-vpn"> </a>Takana VPN verkkopalvelun muodostaminen

Kun API hallintapalvelun on yhteydessä VPN-verkkopalvelut virtual verkoston käyttäminen on sama asia kuin julkisen palvelujen. Kirjoita paikallisen osoitteen tai WWW-palvelun isäntänimi (Jos DNS-palvelin on määritetty Azure Virtual Network) **WWW-palvelun URL** -kenttään uuden API luotaessa tai muokkaat aiemmin luotua tehtävää.

![Lisää API VPN][api-management-setup-vpn-add-api]

## <a name="required-ports-for-api-management-vpn-support"></a>Tarvittavat portit API hallinta VPN-tuki

Kun API Management-palvelun esiintymän isännöidään VNET, käytetään portit seuraavassa taulukossa. Jos nämä portit on estetty, palvelu ei ehkä toimi oikein. Pysty vähintään yksi seuraavista porttien estetyt on yleisin virheellisen määrityksen ongelman, kun API hallinnan käyttämisestä VNET.

| Portit                      | Suunta        | Protokollan | Tarkoitus                                                          | Lähteen / kohde              |
|------------------------------|------------------|--------------------|------------------------------------------------------------------|-----------------------------------|
| 80, 443                      | Saapuva          | TCP                | Asiakkaan tietoliikenne API hallinta                           | INTERNET / VIRTUAL_NETWORK        |
| 80,443                       | Lähtevän         | TCP                | Hallinnan API Azure säilytys- ja Azure palvelun Bus riippuvuus | VIRTUAL_NETWORK / INTERNET        |
| 1433                         | Lähtevän         | TCP                | SQL-Ohjelmointirajapinnan hallinta riippuvuudet                               | VIRTUAL_NETWORK / INTERNET        |
| 9350, 9351, 9352, 9353, 9354 | Lähtevän         | TCP                | Palvelun Bus riippuvuuksia API hallinta                       | VIRTUAL_NETWORK / INTERNET        |
| 5671                         | Lähtevän         | AMQP               | API hallinta riippuvuuden lokin tapahtuman keskittimeen käytäntö            | VIRTUAL_NETWORK / INTERNET        |
| 6381, 6382, 6383             | Saapuva ja lähtevä liikenne | UDP                | Redis välimuistin riippuvuuksia API hallinta                       | VIRTUAL_NETWORK / VIRTUAL_NETWORK |
| 445                          | Lähtevän         | TCP                | Azure tiedostoresurssin GIT API hallinta-riippuvuus            | VIRTUAL_NETWORK / INTERNET        |

## <a name="custom-dns"> </a>Mukautettu DNS-palvelinasetukset

API hallinta määräytyy Azure palvelujen määrä. Kun API Management-palvelun esiintymän isännöidään VNET, jossa käytetään mukautettuja DNS-palvelimeen, se on oltava pysty ratkaisemaan isäntänimet Azure näistä palveluista. Mukautettujen DNS-määritys noudattamalla [nämä](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) ohjeet.  

## <a name="related-content"> </a>Liittyvät sisältö


* [Luo virtuaalisia verkko sivusto sivusto VPN-yhteyden perinteinen Azure-portaalissa][]
* [Jäljittää API tarkastaminen-toiminnolla voit soittaa Azure API hallinta][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure perinteinen Portal]: https://manage.windowsazure.com/

[Luo virtuaalisia verkko sivusto sivusto VPN-yhteyden perinteinen Azure-portaalissa]: ../vpn-gateway/vpn-gateway-site-to-site-create.md
[Jäljittää API tarkastaminen-toiminnolla voit soittaa Azure API hallinta]: api-management-howto-api-inspector.md
