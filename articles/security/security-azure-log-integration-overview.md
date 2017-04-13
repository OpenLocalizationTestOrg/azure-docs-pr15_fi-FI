<properties
   pageTitle="Johdanto Microsoft Azure log integrointi | Microsoft Azure"
   description="Lisätietoja Azure log integrointi, tärkeimmistä ominaisuuksista ja miten se toimii."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Johdanto Microsoft Azure log integrointi (ennakkoversio)

Lisätietoja Azure log integrointi, tärkeimmistä ominaisuuksista ja miten se toimii.

## <a name="overview"></a>Yleiskatsaus

Ympäristön (PaaS) ja infrastruktuurin ylläpidettävä Azure palvelu (IaaS) luo suuria määriä tietoa tietoturva. Lokitiedostot sisältää tärkeitä tietoja, joka voi toimittaa liiketoimintatietojen ja tehokas käytännön virheitä, sisäisten ja ulkoisten uhkien, lakisääteisten määräysten ongelmat ja poikkeamia verkon, host ja käyttäjän tiedot.

Azure log-integroinnin avulla voit integroida raaka lokit Azure resurssien paikallisen suojaustietojen ja tapahtuman Management (SIEM) järjestelmien. Azure log integrointi kerää Azure diagnostiikka Windows-laitteessa *(WAD)* näennäiskoneiden sekä diagnostiikka-kumppaniratkaisuihin kuten Web Application palomuurin (WAF). Integrointi on yhdistetty Raporttinäkymät-ikkunan kaikki oman varat paikallisen tai pilvipalvelussa, niin, että voit koostaa, yhdistää, analysoida ja ilmoita suojauksen tapahtumat.

![Azure log-integrointi][1]

## <a name="what-logs-can-i-integrate"></a>Mitä lokeja voi integroida?

Azure tuottaa monipuolisia kirjaaminen Azure jokaisella palvelulla. Lokitiedostot on luokiteltu kaksi päätyyppiä:

- **Ohjausobjektin/hallinnan lokit**, joka tuo näkyvyyttä Azure Resurssienhallinta luominen, Päivitä ja poista-toimintoja. Azure valvontalokien on esimerkki tällaista loki.
- **Tasossa lokeja**, jotka antavat näkyvyys tapahtumat korotettuna osana Azure resurssien käyttö. Tällaista loki ovat Windowsin tapahtumalokiin järjestelmän suojaus- ja sovelluksen kirjaa virtual machine.

Azure log integrointi tukee tällä hetkellä integrointi Azure valvontalokien, virtuaalikoneen lokit ja Azure Tietoturvakeskus ilmoitukset.

Jos sinulla on kysyttävää Azure Log integrointi, Lähetä sähköpostia [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä asiakirjassa on tuotu Azure log integrointiin. Saat lisätietoja Azure log integrointi ja lokit Tuetut tiedostotyypit on seuraavissa artikkeleissa:

- [Microsoft Azure Log integrointi Azure lokit (ennakkoversio)](https://www.microsoft.com/download/details.aspx?id=53324) – tiedot, järjestelmävaatimukset ja asennusohjeet Azure log integroinnista Download Centeristä.
- [Azure log integrointi käytön aloittaminen](security-azure-log-integration-get-started.md) : Tässä opetusohjelmassa esitellään asennettuun Azure log integrointi ja integrointi lokit Azure-tallennustilan, Azure valvontalokien ja Tietoturvakeskus ilmoitukset.
- [Kumppanin määritysvaiheet](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – on blogikirjoituksessa avulla voit määrittää Azure log integrointi kumppaniratkaisuihin Splunk, HP ArcSight ja IBM QRadar käyttöä varten.
- [Azure log integrointi usein kysytyt kysymykset](security-azure-log-integration-faq.md) – tämä usein kysytyt kysymykset vastauksia kysymyksiin Azure log integrointi.
- [Integrointi Tietoturvakeskus varoittaa Azure Kirjaudu integrointi](../security-center/security-center-integrating-alerts-with-log-integration.md) – tässä asiakirjassa kerrotaan, miten synkronoimaan Tietoturvakeskus ilmoitusten sekä virtuaalikoneen suojauksen tapahtumien keräämiä Azure diagnostiikka- ja Azure valvontalokien log analytics tai SIEM ratkaisu.
- [Azure diagnostiikka- ja Azure valvontalokien uudet ominaisuudet](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – tässä blogimerkinnässä kerrotaan Azure valvontalokien ja muita ominaisuuksia, jotta voit käyttää tietoja Azure resurssien toimintoja.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
