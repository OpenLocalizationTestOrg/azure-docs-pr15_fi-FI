<properties
   pageTitle="Azure Tietoturvakeskus uhkien liiketoimintatietojen raportin | Microsoft Azure"
   description="Tämän asiakirjan auttaa suojausvaroituksen koskevien lisätietojen hakeminen tutkimuksen aikana Azure Security Center uhkien älykkäät raporttien avulla."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-threat-intelligence-report"></a>Azure Tietoturvakeskus uhkien Liiketoimintatieto-raportti
Tässä asiakirjassa kerrotaan, miten Azure Security Center uhkien älykkäät raportteja voi auttaa sinua lisätietoja uhkien, joka on luotu suojausvaroituksen.

## <a name="what-is-a-threat-intelligence-report"></a>Uhkien Liiketoimintatieto-taulukkoraportin kuvaus
Tietoturvakeskus uhkien tunnistus toimii seuraamalla suojaustiedot Azure resursseja, verkon ja yhdistetyn kumppaniratkaisuihin. Se analysoi nämä tiedot hajautettuna usein tietoja useista lähteistä tunnistavan uhkien. Tämä toimenpide on osa Tietoturvakeskus [tunnistus ominaisuuksia](security-center-detection-capabilities.md). 

Kun Tietoturvakeskus tunnistaa uhkien, se käynnistää on [Suojausvaroitus](security-center-managing-and-responding-alerts.md), jossa on tietty tapahtuma, kuten ehdotukset tarjoamiseksi koskevat yksityiskohtaiset tiedot. Tapauksen vastauksen työryhmiä tutkia ja uhkien riskien parantaminen auttamaan Tietoturvakeskus sisältää uhkien liiketoimintatietojen raportin, joka sisältää tiedot, jotka on havaittu, mukaan lukien tiedot uhkien: 

- Voi saada käyttäjän tunnistetiedot tai yhteydet (Jos nämä tiedot ovat käytettävissä)
- Käytetä kovin laajasti tavoitteet
- Nykyisten ja historiallisten hyökkäyksen Kampanjat (Jos nämä tiedot ovat käytettävissä)
- Käytetä kovin laajasti taktiikoita, työkaluja ja toimia
- Liittyvät ilmaisimet, kuten URL-osoitteet ja tiedostojen hajautusarvot vuotamiseen (IoC)
- Victimology, joka on alan ja maantieteellinen levinneisyys, voit helpottaa selvittämisestä, onko organisaation Azure resurssit ovat vastuullasi
- Rajoituksen ja korjaus

>[AZURE.NOTE] Mitä tahansa raportin tietojen määrä vaihtelee; yksityiskohtainen perustuu haittaohjelmia tehtävän ja levinneisyys.

Tietoturvakeskuksessa on kolmenlaisia uhkien raportit, jotka voivat vaihdella sen mukaan, hyökkäystä. Käytettävissä olevat raportit ovat:

- **Ryhmän käyttöraportti**: tarjoaa kiinnostavat hyökkääjät, tavoitteet ja taktiikoita.
- **Markkinointikampanjan raportin**: ohjeessa on tietoja tietyn hyökkäyksen kampanjat. 
- **Uhkien yhteenvetoraportti**: kattaa kaikki kohteet edellisen kaksi raporteissa.

Tämä tietotyyppi on erittäin hyödyllinen [tapauksen vastauksen](security-center-incident-response.md) aikana ei jatkuvaa tutkimuksen ymmärtää hyökkäystä hänen motivaatioita ja mitä voit tehdä ongelman siirtymiselle pienentämään lähde. 

## <a name="how-to-access-the-threat-intelligence-report"></a>Voit käyttää uhkien Liiketoimintatieto-raportin?

Voit tarkastella nykyistä ilmoituksiasi katsomalla **suojausvaroitusten** -ruutu. Avaa Azure-portaalin ja saat lisätietoja jokaisesta ilmoituksesta seuraavasti:

1. Tietoturvakeskus koontinäytössä näet **suojausvaroitusten** -ruutu.

2. Avaa **suojausvaroitusten** -sivu, jossa on lisätietoja ilmoituksia ruutua ja valitse suojausvaroituksessa, jonka haluat saada lisätietoja tietoja.

    ![Suojausvaroitusten](./media/security-center-threat-report/security-center-threat-report-fig1.png)

3. Tässä tapauksessa **suorittaa epäilyttävistä prosessi** -sivu näyttää ilmoituksen tietoja alla olevassa kuvassa osoitetulla tavalla:

    ![Suojauksen ilmoitusten detais](./media/security-center-threat-report/security-center-threat-report-fig2.png)

4.  Kunkin suojausvaroituksessa käytettävissä tietojen määrä vaihtelee ilmoitus tyypin mukaan. **RAPORTIT** -kentässä on linkin uhkien Liiketoimintatieto-raporttiin. Napsauta sitä ja toisessa selainikkunassa näkyvät PDF-tiedoston.

    ![Tallennustilan valinta](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Täällä voit ladata tämän raportin ja lue lisätietoja tietoturvaongelman, jonka havaittiin PDF ja toimia antamien tietojen mukaan.

## <a name="see-also"></a>Katso myös

Tämän asiakirjan opit, kuinka Azure Security Center uhkien älykkäät raportit voivat auttaa tietoja suojausvaroitusten tutkimuksen aikana. Saat lisätietoja Azure Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskus usein kysytyt kysymykset](security-center-faq.md). Etsi usein kysytyt kysymykset-palvelun avulla.
- [Tapauksen vastauksen virtuaalisten Azure-Tietoturvakeskus](security-center-incident-response.md)
- [Azure Tietoturvakeskus tunnistusominaisuudet](security-center-detection-capabilities.md)
- [Azure Tietoturvakeskus suunnittelu ja toimintojen opas](security-center-planning-and-operations-guide.md). Opettele osat antaa Azure Tietoturvakeskus tyyliseikat ja suunnitteleminen.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md). Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Suojauksen tapahtumaa Azure Tietoturvakeskuksessa käsittely](security-center-incident.md)
- [Azure Security-blogi](http://blogs.msdn.com/b/azuresecurity/). Etsi tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen kirjoituksia.
