<properties
   pageTitle="Seuranta ja niihin vastaaminen suojausvaroitusten toimintojen hallinta Suite suojaus ja valvonta-ratkaisun | Microsoft Azure"
   description="Tämän asiakirjan avulla voit seurata ja vastata suojausvaroitusten käytettävissä OMS suojaus ja valvonta uhkien Liiketoimintatieto-vaihtoehdon avulla."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Seuranta- ja toimintojen hallinta Suite suojaus ja valvonta-ratkaisun suojausvaroitusten vastaaminen

Tämän asiakirjan avulla voit seurata ja vastata suojausvaroitusten käytettävissä OMS suojaus ja valvonta uhkien Liiketoimintatieto-vaihtoehdon avulla.

## <a name="what-is-oms"></a>Mikä on OMS?

Microsoft toimintojen hallinta Suite (OMS) on Microsoft cloud perusteella IT management-ratkaisun, jonka avulla voit hallita ja suojata paikallisen oman ja cloud infrastruktuurin. Lisätietoja OMS Lue artikkeli [Toimintojen hallinta Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Uhkien liiketoimintatietojen

Yrityksen ympäristössä, jossa käyttäjät laaja käyttämään verkon ja yrityksen tietojen yhdistäminen monenlaisia laitteita avulla on välttämätöntä, että voit seurata aktiivisesti resurssien sekä vastata nopeasti suojauksen tapaukset. Tämä on tärkeää suojauksen elinkaari näkökulmasta, koska joitakin cybersecurity uhkien ei saa ottaa ilmoitusten erityisesti tai epäilyttävät toimintoja, jotka voidaan tunnistaa perinteinen tekniset turvaominaisuudet. 

Käytettävissä OMS suojaus ja valvonta **Uhkien Liiketoimintatieto** -asetuksen avulla IT-järjestelmänvalvojien voit tunnistaa tietoturvauhkia vastaan ympäristössä, kuten, tunnistetaan tietyn tietokoneen [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection)kuuluu. Kun hyökkääjät asentaa luvattomasti haittaohjelman, joka yhdistää tämän tietokoneen salaisesti komento ja hallita tietokoneet voi olla solmujen botnet. Voit myös määrittää mahdollisten uhkien tulevat maan alla viestintä kanavat, kuten [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

Jotta voit luoda tämän uhkien liiketoimintatietojen, OMS suojaus ja valvonta käyttää useista lähteistä Microsoft tulevat tiedot. OMS suojaus ja valvonta hyödyntää tunnistaa mahdolliset uhkien vastaan ympäristön tiedoista.

Uhkien Liiketoimintatieto-ruudussa koostuu kolmesta pää asetukset:
- Haittaohjelmien liikenteen palvelinten
- Havaitut uhkien tyypit
- Uhkien liiketoimintatietojen kartta

> [AZURE.NOTE] Yleiskatsaus nämä asetukset Lue [aloittaminen toimintojen hallinta Suite suojaus ja valvonta-ratkaisun](oms-security-getting-started.md).

### <a name="responding-to-security-alerts"></a>Suojausvaroitusten vastaaminen

Seuraavien vaiheittaisten ohjeiden [tapauksen vastauksen suojaus](https://technet.microsoft.com/library/cc512623.aspx) -prosessin on tunnistaa n haavoittuvan vakavuus. Tässä vaiheessa kannattaa suorittaa seuraavia tehtäviä:

- Määrittää, mikä hyökkäystä
- Valitse kohta, hyökkäyksen alkuperän
- Selvitä hyökkäystä käyttötarkoitusta. On suunnattu erityisesti organisaation hankkimaan tiettyjä tietoja hyökkäyksen tai on satunnainen?
- Järjestelmien, joka on käsiin selvittäminen
- Tiedostot, joita on käytetty ja määrittää kyseiset tiedostot suojaustasoa tunnistaa

Voit hyödyntää **Uhkien** liiketoimintatiedot OMS suojaus ja valvonta-ratkaisun auttaa seuraavien toimintojen kanssa. Noudata seuraavia ohjeita voit käyttää tätä **Uhkien Liiketoimintatieto** -asetukset:

1. Valitse **Microsoft toimintojen hallinta Suite** tärkeimmät Raporttinäkymät-ikkunan **Suojaus ja valvonta** -ruutu.

    ![Suojaus ja valvonta](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. **Suojaus ja valvonta** raporttinäkymät-ikkuna tulee näkyviin oikealla **Uhkien Liiketoimintatieto** -ominaisuudet alla kuvatulla tavalla:

    ![Uhkien intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Nämä kolme ruutujen avulla voit nykyisen uhkien yleiskatsaus. **Haittaohjelmien liikenteen Server** osaat selvittäminen, onko tietokoneessa, jossa on seuranta (sisällä tai verkon ulkopuolella), joka lähettää haittaohjelmien liikenne Internet. 

**Detected uhkien tyypit** -ruutu näkyy uhkien, jotka ovat ajan tasalla "tomaatti"luonnosta yhteenveto-ruudussa napsauttamalla Lisätietoja näistä uhkien tulee näkyviin alla:

![Havaitut uhkien tyypit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Voit purkaa lisätietoja kunkin uhkien napsauttamalla sitä. Alla olevassa esimerkissä tarkempia tietoja Botnet:

![Lisätietoja uhkien](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Alkuun, tässä osassa kuvattua nämä tiedot on hyötyä tapauksen vastauksen palvelupyynnön aikana. Se voi olla myös tärkeitä lainmukaisten tutkimuksen aikana kohtaa, johon haluat selvittää hyökkäystä, minkä järjestelmän oli käsiin tai aikajanan. Voit helposti tunnistaa joitakin keskeisiä tietoja hyökkäyksen, kuten raportin: hyökkäyksen, joka on käsiin paikallinen IP tai yhteys istunnon tilan. 

**Uhkien liiketoimintatietojen kartan** avulla voit nykyinen sijainti maapallon ympärille, joissa on haittaohjelmien liikenne. (Saapuvat) ovat oranssi ja punainen (lähtevät) nuolia muuntomallin, joka tunnistaa liikenne, suunta johonkin näistä nuolia napsauttamalla, se näyttää uhkien ja liikenteen suunnan alla kuvatulla tavalla:

![uhkien intel-kartta](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [AZURE.NOTE] Näet osoitus siitä, miten voit käyttää tätä ominaisuutta aikana tapahtuma vastauksen prosessin katselemalla esityksen [opastetun tutkimuksen käyttämällä toimintojen hallinta Suite Mitigate palvelinkeskuksen tietoturvauhkia](https://myignite.microsoft.com/videos/5000) toimitettu Microsoft Ignite.

## <a name="see-also"></a>Katso myös

Tässä asiakirjassa opit suojausvaroitusten vastata **Uhkien Liiketoimintatieto** -asetus OMS suojaus ja valvonta-ratkaisun avulla. Lisätietoja OMS tietoturvasta on seuraavissa artikkeleissa:

- [Toimintojen hallinta Suite (OMS) yleiskatsaus](operations-management-suite-overview.md)
- [Toimintojen hallinta Suite suojaus ja valvonta-ratkaisun käytön aloittaminen](oms-security-getting-started.md)
- [Resurssien valvominen toimintojen hallinta Suite suojaus ja valvonta ratkaisu](oms-security-monitoring-resources.md)
