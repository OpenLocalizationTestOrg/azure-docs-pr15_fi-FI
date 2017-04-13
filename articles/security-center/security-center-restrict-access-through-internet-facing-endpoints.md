<properties
   pageTitle="Rajoita Internetiin yhteydessä oleva päätepisteet Azure Tietoturvakeskuksessa käyttöoikeuksien | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten Azure Tietoturvakeskus suositus **Internetiin yhteydessä olevan päätepisteen käyttöoikeuksien rajoittaminen**täytäntöön."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Internetiin yhteydessä oleva päätepisteet Azure Tietoturvakeskuksessa käyttöoikeuksien rajoittaminen

Azure Tietoturvakeskus suosittelee Internetiin yhteydessä oleva päätepisteet käyttöoikeuksien rajoittaminen Jos verkko-käyttöoikeusryhmät (NSGs) on vähintään yksi saapuvan liikenteen säännöt, jotka sallivat käytöltä "kaikki" lähde-IP-osoite. Avaaminen käyttää "mitään" voivat ottaa käyttöön hyökkääjien käyttää resurssit. Tietoturvakeskus suosittelee Muokkaa saapuvien sääntöjen rajoittaaksesi lähteen IP-osoitteet, joiden on todella.

Tämä suositus luodaan kaikki muut kuin portin, jossa on "jokin" tietolähteeksi.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla. Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Internet aukeaman päätepisteen käyttöoikeuksien rajoittaminen** **suositukset-sivu**.
![Vastakkaisten päätepisteen Internet käyttöoikeuksien rajoittaminen][1]

2. **Käyttöoikeuksien rajoittaminen aukeaman päätepisteen Internet**-sivu avautuu. Tämä sivu näyttää liittyvät saapuvan liikenteen säännöt, jotka luovat mahdollisen tietoturvaongelman näennäiskoneiden (VMs). Valitse AM.
![Valitse AM][2]

3. **NSG** -sivu näyttää verkko-käyttöoikeusryhmän tiedot, liittyvät saapuvan liikenteen säännöt ja liittyvän AM. Valitse Jatka muokkaamista saapuvan liikenteen sääntö **Muokkaa saapuvan liikenteen säännöt** .
![Verkko-käyttöoikeusryhmän sivu][3]

4. Valitse saapuvan säännön muokkaaminen **Saapuvan liikenteen säännöt** -sivu. Tässä esimerkissä Valitse **AllowWeb**.
![Saapuvien suojaussäännöt][4]

  Huomaa, että voit myös valita **oletusarvoiset** artikkelissa oletusarvon mukaan kaikki NSGs sääntöjä. Oletusarvoiset ei voi poistaa, mutta ne on varattu alhaisempaa prioriteettia, koska ne voi ohittaa säännöt, jotka luot. Lisätietoja [oletusarvoisen säännöt](../virtual-network/virtual-networks-nsg.md#default-rules).
![Oletusarvoinen säännöt][5]

5. Muokkaa niin, että **lähde** on IP-osoite tai IP-osoitteiden estäminen saapuvien säännön ominaisuuksien **AllowWeb** -sivu. Lisätietoja saapuvien säännön ominaisuudet-kohdassa [NSG säännöt](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Saapuvan liikenteen säännön muokkaaminen][6]

## <a name="see-also"></a>Katso myös

Tässä artikkelissa osoittanut ottamisesta käyttöön Tietoturvakeskus suositus "Internetiin yhteydessä olevan päätepisteen käyttöoikeuksien rajoittaminen." Lisätietoja NSGs ja sääntöjä, on seuraavissa artikkeleissa:

- [Mikä verkon suojauksen ryhmän (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Voit hallita NSGs Azure-portaalissa](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md)– Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Suojausta koskevia suosituksia Azure Tietoturvakeskuksessa hallinta](security-center-recommendations.md)– Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon seuranta Azure Tietoturvakeskus](security-center-monitoring.md)– Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md)--Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md)– Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/)– uusimmista Azure suojaus ja tiedot.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
