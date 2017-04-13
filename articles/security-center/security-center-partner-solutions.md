<properties
   pageTitle="Kumppaniratkaisuihin Azure Tietoturvakeskuksessa hallinta | Microsoft Azure"
   description="Tämän asiakirjan käydään läpi miten Azure Tietoturvakeskus avulla voit näytön yhdellä silmäyksellä kumppaniratkaisuihin kunnon tila integroitu Azure tilauksen."
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

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Kumppaniratkaisuihin ja Azure Tietoturvakeskus seuranta

Tämän asiakirjan esitellään seuraamisesta kumppanin ratkaisujen Azure Tietoturvakeskuksessa kunto-tilaa.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla. Tämä ei ole vaiheittaiset ohjeet.

## <a name="monitoring-partner-solutions"></a>Kumppaniratkaisuihin seuranta

**Tietoturvakeskus** sivu **kumppaniratkaisuihin** -ruudun avulla voit näytön yhdellä silmäyksellä kumppaniratkaisuihin kunnon tila integroitu Azure tilauksen.
![Kumppaniratkaisuihin-ruutu][1]

**Kumppaniratkaisuihin** -ruutu näyttää kumppaniratkaisuihin ja yhteenveto näitä ratkaisuja tila.

Kumppanin ratkaista **tila** voi olla:

- Suojattu (vihreä) – kunto ei ole ongelma.
- Perusasemassa (punainen) - kunto-ongelma, joka edellyttää kiireisestä aiheesta.
- Pysäytetty (Oranssi) raportointi - ratkaisun lakkasi raportoinnin sen kunto.
- Tila tuntematon protection (Oranssi) - ratkaisun kunto on tällä hetkellä vuoksi epäonnistui prosessi uusi resurssi lisätään aiemmin ratkaisu tuntematon.
- Ei raportoitu (harmaa) - ratkaisun ei ilmoitettu mitään vielä ratkaisussa tila voi olla ilmoittamatta, jos se on vain yhteydessä ja edelleen käyttöönotto.

Jos määritettynä on integroitu tilauksen ratkaisuja ei ruudun ilmoitetaan, että ei ole ratkaisuja. **Kumppaniratkaisuihin** -ruutu valitsemalla ansiosta voit avata kumppaniratkaisut suojaus asentamisesta **suositukset** -sivu.
![Ei kumppaniratkaisut][2]

Voit tarkastella kumppaniratkaisuihin kunto seuraavasti:

1. Valitse **kumppaniratkaisuihin** -ruutu. Sivu avautuu ja siinä näkyvät yhteydessä Tietoturvakeskus kumppanin ratkaisujen luettelo.
![Kumppaniratkaisuihin][3]

2. Valitse kumppanin ratkaisu. Tässä esimerkissä avulla Valitse **F5 WAF2** ratkaisu.  Sivu näkyviin, jossa kumppani-ratkaisun ja ratkaisun liittyvät resurssit. Valitse Avaa kumppanin hallintatoimintojen tämän ratkaisun **ratkaisu konsolin** .
![Kumppanin ratkaisun tiedot][4]

3. Siirry takaisin **F5 WAF2** -sivu ja valitse **linkki-sovellus**. **Linkki sovellukset** -sivu avautuu. Tässä voit muodostaa resurssien kumppanin-ratkaisuun.
![Linkki resurssien kumppanin ratkaisu][5]

## <a name="see-also"></a>Katso myös
Tässä asiakirjassa on otettu käyttöön Tietoturvakeskuksessa **Kumppaniratkaisuihin** -ruutuun. Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Suojausta koskevia suosituksia Azure Tietoturvakeskuksessa hallinta](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon seuranta Azure Tietoturvakeskus](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) – Katso, miten voit hallita ja vastata suojausvaroitusten.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – uusimmista Azure suojaus ja tiedot.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
