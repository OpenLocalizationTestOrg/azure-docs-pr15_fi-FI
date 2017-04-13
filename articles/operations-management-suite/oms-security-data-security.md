<properties
   pageTitle="Toimintojen hallinta Suite suojaus ja valvonta-ratkaisun tietojen suojauksen | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten tiedot hallitaan ja toimintojen hallinta Suite suojaus ja valvonta-ratkaisun monivuotisilla."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Toimintojen hallinta Suite suojaus ja valvonta ratkaisun tietojen suojaaminen

Auttaa asiakkaita estää, haku- ja uhkien vastata, [Toimintojen hallinta Suite (OMS) suojaus ja valvonta-ratkaisun](operations-management-suite-overview.md) kerää ja käsittelee resurssien tietoja, mukaan lukien:

- Suojauksen tapahtumalokin
- Tapahtuman jäljitys for Windows (tapahtumien seuranta) tapahtumat
- AppLocker valvonnan tapahtumat
- Windowsin palomuuri loki
- Lisäasetukset uhkien Analytics-tapahtumat
- Perusaikataulun arvioinnin tulokset
- Antimalware arvioinnin tulokset
- Päivitys tai korjaus arvioinnin tulokset
- Syslogs virtaa, jotka ovat käytössä erikseen agentti

Olemme Varmista vahva sitoumukset yksityisyyden ja tietosuojan näiden tietojen suojaaminen. Microsoft noudattaa tarkka yhteensopivuus ja suojauksen ohjeita –-koodaus, toimivat palvelu.
Tässä artikkelissa kerrotaan, miten hallitaan ja monivuotisilla OMS suojaus ja valvonta-ratkaisun.

## <a name="data-sources"></a>Tietolähteet

OMS suojaus ja valvonta-ratkaisun analysoida tietoja näennäiskoneiden ja Fyysinen tietokoneissa, joihin OMS-agentti on asennettu. OMS suojaus ja valvonta-ratkaisun voi kerätä kokoonpanotietoja suojauksen tapahtumia, kuten Windowsin tapahtumalokiin, valvontalokien, IIS-lokit ja syslog viestejä. Nämä tiedot ovat: käyttöjärjestelmän tyyppi ja versio, käynnissä prosesseja, tietokoneen nimi ja IP-osoitteet, kirjautunut käyttäjä ja vuokraajan ID-tunnuksellasi.  

## <a name="data-protection"></a>Tietojen suojaaminen

**Tietoja eriytymistä**: tiedot säilytetään loogisesti erillisen palvelun koko kullekin osalle. Kaikki tiedot on merkitty organisaation kohden. Tämä tunnisteita jatkuu koko tietojen elinkaari ja kunkin tasolla palvelun pakotettu. 

**Tietojen käyttö**: suojausta koskevia suosituksia ja tutkia mahdollisia tietoturvauhkia, Microsoftin henkilökunta voi käyttää tietojen kerääminen tai analysoida-palveluissa. Olemme noudattaa [Microsoft Online Services-ehdot](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) ja [Tietosuojatiedot](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), minkä osavaltion, että Microsoft ei käytä asiakastietoja tai mainontaa tai samanlaisia kaupallisiin tarkoituksiin tietoja siitä. Microsoftin henkilöstön voivat käyttää suojausta koskevia suosituksia ja tutkia mahdollisia tietoturvauhkia, tietojen kerääminen tai analysoida-palveluissa. Vain Käytämme asiakastietoja tarvittaessa tarjota Azure palveluja, kuten tarkoituksiin yhteensopiva palveluja tarjoavat. Voit säilyttää oikeudet kaikkiin omilla tiedoillasi.

**Tietojen käyttäminen**: Microsoft käyttää kuvioiden ja uhkien liiketoimintatietojen nähdä yli useita alihallinnat, jotka voit parantaa Microsoftin ehkäisemistä ja havaitsemista valmiudet, Olemme kiellä Microsoftin [Tietosuojatiedot](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx)tietosuojasitoumukset mukaisesti.

> [AZURE.NOTE] Tietojen sijainti määritetään tasolla OMS työtilan työtilan luominen, joka on osa ensimmäisen OMS suojaus ja valvonta määrityksen aikana.

## <a name="see-also"></a>Katso myös

Tässä asiakirjassa opit, kuinka hallitaan ja OMS monivuotisilla. Lisätietoja OMS suojaus ja valvonta ratkaisu on seuraavissa artikkeleissa:

- [Toimintojen hallinta Suite (OMS) yleiskatsaus](operations-management-suite-overview.md)
- [Seuranta ja niihin vastaaminen suojausvaroitusten toimintojen hallinta Suite suojaus ja valvonta ratkaisu](oms-security-responding-alerts.md)
- [Resurssien valvominen toimintojen hallinta Suite suojaus ja valvonta ratkaisu](oms-security-monitoring-resources.md)

