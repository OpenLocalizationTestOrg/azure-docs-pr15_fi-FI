<properties
   pageTitle="Lisää seuraava luonti palomuurin Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan ottamisesta käyttöön Azure Tietoturvakeskus suositukset, **Lisää seuraava luonti-palomuurin** ja **reitin traffice vain NGFW kautta**."
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

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Lisää seuraava luonti-palomuurin Azure Tietoturvakeskus

Azure Tietoturvakeskus voi suositella, Lisää seuraava luonti palomuuri (NGFW) Microsoft-kumppanilta lisäämiseksi oman Suojausluettelo. Tämän asiakirjan esitellään, miten voit tehdä tämän kautta.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Lisää seuraava luonti-palomuurin** **suositukset** -sivu.
![Lisää seuraava luonti palomuuri][1]

2. Valitse **Lisää seuraavan sukupolven palomuuri** -sivu päätepisteen.
![Valitse päätepisteen][2]

3. Toinen **Lisää seuraavan sukupolven palomuuri** -sivu avautuu. Voit käyttää aiemmin ratkaisua, jos se on käytettävissä, tai voit luoda uuden. Tässä esimerkissä ei ole nykyisten ratkaisujen pohjalta käytettävissä niin luodaan uusi NGFW.
![Luo uusi seuraavan sukupolven palomuuri][3]

4. Jos haluat luoda uuden NGFW, valitse luettelosta integroitu kumppaneita ratkaisu. Tässä esimerkissä on valitsee **Valintaruudun kohta**.
![Valitse seuraava luonti palomuurin ratkaisu][4]

5. **Tarkista piste** -sivu avautuu antaa kumppanin-ratkaisun tietoja. Valitse **Luo** tiedot-sivu.
![Palomuurin tiedot-sivu][5]

6. **Luo virtuaalikoneen** -sivu avautuu. Voit kirjoittaa tähän asettamasi virtual machine (AM), joka suoritetaan NGFW tarvittavat tiedot. Ohjeiden ja anna tarvittavat NGFW tiedot. Valitse OK, jolloin.
![Luo virtual machine suorittaa NGFW][6]

## <a name="route-traffic-through-ngfw-only"></a>Reitin liikenne NGFW vain

Palaa **suositukset** -sivu. Uusi tapahtuma on luotu jälkeen voit lisätä NGFW kautta Tietoturvakeskus **reitin liikenne vain NGFW**. Tämä suositus luodaan vain, jos olet asentanut oman NGFW Tietoturvakeskus kautta. Jos sinulla on Internetiin yhteydessä oleva päätepisteet, Tietoturvakeskus kannattaa määrittää verkko-käyttöoikeusryhmän säännöt, jotka saapuvan liikenteen pakottaminen oman AM oman NGFW kautta.

1. Valitse **vain NGFW reitin liikenne** **suositukset-sivu**.
![Reitin liikenne NGFW vain][7]

2. **Reitittää liikenteen – vain NGFW** jossa on luettelo, joka reitittää liikenteen VMs sivu avautuu. Valitse luettelosta AM.
![Valitse AM][8]

3. Saat valitun AM sivu avautuu ja näyttää liittyvän saapuvan liikenteen säännöt. Kuvaus avulla saat lisätietoja mahdollisista vaiheisiin. Valitse Jatka muokkaamista saapuvan liikenteen sääntö **Muokkaa saapuvan liikenteen säännöt** . Se, että **lähde** ei ole määritetty **mitään** , jotka on linkitetty NGFW Internetiin yhteydessä oleva päätepisteiden. Lisätietoja saapuvien säännön ominaisuudet-kohdassa [NSG säännöt](../virtual-network/virtual-networks-nsg.md#nsg-rules).
![Määritä säännöt käyttörajoitukset][9]
![saapuvan liikenteen säännön muokkaaminen][10]

## <a name="see-also"></a>Katso myös

Tämän asiakirjan osoittanut ottamisesta käyttöön Tietoturvakeskus suositus "Lisää seuraavan sukupolven palomuuri." Saat lisätietoja NGFWs ja Check Point kumppanin ratkaisu on seuraavissa artikkeleissa:

- [Seuraavan sukupolven palomuuri](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Tarkista pisteen vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Katso, miten voit määrittää suojauskäytännöt.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi blogi kirjaa tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
