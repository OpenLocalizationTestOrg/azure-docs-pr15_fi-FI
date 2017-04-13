<properties
  pageTitle="Azure muiden palvelujen kanssa Azure-DNS: n avulla | Microsoft Azure"
  description="Tietoja opit käyttämään Azure DNS Azure muiden nimen selvittäminen"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Azure DNS: n avulla Azure muiden palvelujen kanssa

Azure DNS on isännöityä DNS management ja nimi tarkkuus. Voit luoda julkisen DNS-nimiä, sovellusten ja palveluiden on otettu käyttöön Azure. Azure-palvelun nimi luominen mukautetun toimialueen sujuu yhtä helposti kuin lisääminen tietueen tyyppi on oikea palvelun.

* Dynaamisesti kohdistettu IP-osoitteille sinun on luotava DNS CNAME-tietue, joka yhdistää Azure luotu palvelun DNS-nimen. DNS-standardit estää käytön vyöhykkeen apex CNAME-tietue.
* Nimipalvelimet kohdistettu IP-osoitteille voit luoda DNS A-tietue käyttämällä nimi, esimerkiksi osoitteessa vyöhykkeen apex _pelkistetty_ toimialuenimi.

Seuraavassa taulukossa kuvataan tuetut tietuetyypeistä, joita voi käyttää eri Azure palveluihin. Kuten näet tämän taulukon Azure DNS tukee vain DNS-tietueiden Internetiin yhteydessä oleva verkkoresursseja. Azure DNS ei voi käyttää nimenselvitys sisäisistä, yksityisistä osoitteista.

| Azure-palvelu | Verkkokäyttöliittymä | Kuvaus |
|---------------|-------------------|-------------|
| Sovelluksen yhdyskäytävän | Edustatietokannan julkiseen IP | Voit luoda DNS-A- tai CNAME-tietue. |
| Kuormituksen | Edustatietokannan julkiseen IP | Voit luoda DNS-A- tai CNAME-tietue. Kuormituksen voi olla määritetään dynaamisesti IPv6 julkiseen IP-osoite. Tämän vuoksi sinun on luotava IPv6-osoitteen CNAME-tietue. |
| Liikenteen hallinta | Julkinen nimi | Voit luoda vain CNAME-TIETUE, joka yhdistää liikenteen hallinta profiilin trafficmanager.net nimen. Lisätietoja on artikkelissa [miten liikenteen hallinta toimii](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example). |
| Pilvipalvelussa | Julkiseen IP | Nimipalvelimet kohdistettu IP-osoitteille voit luoda DNS-A-tietue. Dynaamisesti kohdistettu IP-osoitteille sinun on luotava CNAME-tietue, joka yhdistää _cloudapp.net_ nimi. Tämä sääntö koskee VMs luoda perinteinen-portaalissa, koska ne on otettu käyttöön pilvipalveluun nimellä. Lisätietoja on artikkelissa [määrittäminen käyttämään mukautettua toimialuenimeä Cloud Services-palveluissa](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| Sovelluksen-palvelu | Ulkoinen IP | Ulkoinen IP-osoitteille voit luoda DNS-A-tietue. Muussa tapauksessa sinun on luotava CNAME-tietue, joka yhdistää azurewebsites.net nimi. Lisätietoja on artikkelissa [Yhdistä mukautettua toimialuenimeä Azure-sovellukseen](../app-service-web/web-sites-custom-domain-name.md) |
| Resurssienhallinta VMs | Julkiseen IP | Resurssienhallinta VMs voi olla julkiseen IP-osoitteet. AM julkiseen IP-osoite voidaan myös kuormituksen tasauspalvelun takana. Voit luoda julkisen osoitteen DNS-A- tai CNAME-TIETUE tietueen. Mukautettu nimi voidaan ohittaa VIP-kuormituksen. |
| Perinteinen VMs | Julkiseen IP | Perinteinen VMs PowerShell-sovelluksella luodut tai CLI voi määrittää dynaamisen tai staattisen (varattu) virtual osoitteessa. Voit luoda DNS-CNAME- tai tietueen tarpeen mukaan. |
