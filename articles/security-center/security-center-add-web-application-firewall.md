<properties
   pageTitle="Web-sovelluksen palomuurin lisääminen Azure Tietoturvakeskus | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan ottamisesta käyttöön Azure Tietoturvakeskus suositukset **lisääminen web-sovelluksen palomuurin** ja **Viimeistele sovellusten suojaus**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Web-sovelluksen palomuurin lisääminen Azure Tietoturvakeskus

Azure Tietoturvakeskus voi suositella lisääminen web palomuurin (WAF) Microsoft-kumppanilta, jos haluat suojata web-sovellusten. Tämän asiakirjan esitellään, miten voit tehdä tämän kautta.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **suojatun web-sovelluksen käyttäminen web-sovelluksen palomuurin** **suositukset** -sivu.
![Suojatun verkkosovellusta][1]

2. Valitse web-sovelluksen **suojatun web-sovellusten käyttäminen web-sovelluksen palomuuri** -sivu. **Lisää Web-sovelluksen palomuuri** -sivu avautuu.
![Web-sovelluksen palomuurin lisääminen][2]
3. Voit käyttää aiemmin web-sovelluksen palomuurin, jos se on käytettävissä, tai voit luoda uuden. Tässä esimerkissä ei ole olemassa WAFs käytettävissä niin luodaan uusi WAF.

4. Jos haluat luoda uuden WAF, valitse luettelosta integroitu kumppaneita ratkaisu. Tässä esimerkissä on valita **Barracuda Web-sovelluksen palomuuri**.
5. **Barracuda Web-sovelluksen palomuuri** -sivu avautuu antaa kumppanin-ratkaisun tietoja. Valitse **Luo** tiedot-sivu.
![Palomuurin tiedot-sivu][3]

6. **Uusi Web-sovelluksen palomuuri** -sivu avautuu, jossa voit suorittaa **AM** määritysvaiheet ja **WAF**tietoja. Valitse **AM määritystä**.

7. Kirjoita **AM määritys** -sivu, joka suoritetaan WAF virtuaalikoneen asettamasi tarvittavat tiedot.
![AM määritys][4]
8. Palaa **Uusi Web-sovelluksen palomuuri** -sivu ja valitse **WAF tiedot**. Määrität itse WAF **WAF tiedot** -sivu. Vaihe 7: ssä voit määrittää, WAF suorittavat ja vaihe 8 avulla voit valmistella WAF itse virtuaalikoneen.

## <a name="finalize-application-protection"></a>Viimeistele sovellusten suojaus

1. Palaa **suositukset** -sivu. Uuden tapahtuman luotiin, kun olet luonut WAF **Viimeistele sovelluksen**suojauksen. Tämän arvon ilmoittaa, että sinun on todella johtimen Azure Virtual verkoston WAF määrittäminen niin, että se suojaa sovelluksen loppuun.
![Viimeistele sovellusten suojaus][5]

2. Valitse **Viimeistele sovellusten suojaus**. Uusi sivu avautuu. Näet, että on web-sovelluksen on oltava sen liikenne reititetään uudelleen.
3. Valitse verkkosovellus. Sivu tulee näkyviin tutustutaan vaiheet Viimeistellään Internet-sovelluksen palomuurin asetukset. Ohjeiden ja valitse sitten **Rajoita liikenne**. Tietoturvakeskuksessa Tee johdotus ylöspäin.
![Rajoita liikenne][6]

> [AZURE.NOTE] Voit suojata useita web-sovellusten Tietoturvakeskuksessa lisäämällä aiemmin WAF käyttöönoton näiden sovellusten. WAF laitteiden (luodaan käyttämällä resurssien hallinnan käyttöönottomalli) on erillinen virtual verkon käyttöön. WAF laitteiden (luotu perinteinen käyttöönoton mallin) on rajoitettu verkon käyttöoikeusryhmän avulla. Tuki laajennetaan mukautetun käyttöönottoon WAF laitteen, (perinteinen) myöhemmin. Lisätietoja [perinteinen ja resurssien hallinnan käyttöönotto mallien](../azure-classic-rm.md) Azure resurssien.

Lokit-että WAF on nyt täysin integroitu. Tietoturvakeskuksessa voit aloittaa kerääminen ja analysoiminen lokit niin, että se voi tuoda tärkeitä suojausvaroitusten automaattisesti.

## <a name="see-also"></a>Katso myös

Tämän asiakirjan osoittanut ottamisesta käyttöön Tietoturvakeskus suositus "Lisää verkkosovelluksen." Saat lisätietoja web-sovelluksen palomuurin määrittämisestä on seuraavissa artikkeleissa:

- [Web Application palomuurin (WAF) määrittäminen sovelluksen Service-ympäristössä](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi blogi kirjaa tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
