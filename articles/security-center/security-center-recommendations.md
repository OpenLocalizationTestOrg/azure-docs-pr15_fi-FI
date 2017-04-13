<properties
   pageTitle="Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tämän asiakirjan käydään läpi kuinka suosituksia Azure Tietoturvakeskuksessa avulla voit suojata Azure resurssit ja pidä suojauskäytäntöjen mukaisesti."
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
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="managing-security-recommendations-in-azure-security-center"></a>Hallinta Azure Tietoturvakeskuksessa suojausta koskevia suosituksia

Tämän asiakirjan esitellään suosituksia käyttämisestä Azure Tietoturvakeskus avulla voit suojata Azure resurssit.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="what-are-security-recommendations"></a>Mitkä ovat suojausta koskevia suosituksia?
Tietoturvakeskus analysoi säännöllisesti Azure resurssien suojaus-tilaan. Kun Tietoturvakeskus havaitsee mahdollisen tietoturva-aukkoja, Luo suosituksia. Suosituksia opastaa prosessi määrittämiseen tarvittavat ohjausobjektit.

## <a name="implementing-security-recommendations"></a>Käyttöönoton suojausta koskevia suosituksia

### <a name="set-recommendations"></a>Määritä suositukset

[Asetus suojauskäytäntöjä Azure Tietoturvakeskuksessa](security-center-policies.md)voit lukea avulla:

- Suojauskäytäntöjen määrittäminen.
- Ota käyttöön tietojen kerääminen.
- Valitse mitä suosituksia Nähdäksesi suojauskäytäntö osana.

Nykyisen käytännön suosituksia center ympärille Järjestelmäpäivitykset, perusaikataulun sääntöjä, antimalware ohjelmat [verkon käyttöoikeusryhmät](../virtual-network/virtual-networks-nsg.md) aliverkosta ja verkkoliittymät, SQL-tietokannan tarkistaminen, SQL-tietokannan läpinäkyvä salauksen ja web-sovelluksen palomuurit.  [Suojauskäytäntöjen määrittäminen](security-center-policies.md) on suositus kunkin asetuksen kuvauksen.

### <a name="monitor-recommendations"></a>Näytön suositukset
Määritettyäsi suojauskäytäntö Tietoturvakeskus analysoi tunnistaa mahdolliset heikkouksien resurssien suojaus-tilaan. **Tietoturvakeskus** sivu **suositukset** -ruutu ilmoittaa, suositukset merkittyä Tietoturvakeskus kokonaismäärän.

![Suositukset-ruutu][1]

Voit tarkastella kunkin suositus:

1. Valitse **suositukset-ruutu** **Tietoturvakeskus** -sivu. **Suositukset** -sivu avautuu.

Suosituksia näkyvät taulukkomuodossa, jossa kunkin rivin edustaa yhtä tietyn suositus. Tämän taulukon sarakkeet:

- **Kuvaus**: Tässä artikkelissa kerrotaan suositus ja mitä sille on tehtävä osoite.
- **Resurssi**: luettelot, joihin tämä suositus pätee resurssit.
- **Tila**: kuvataan suositus nykyinen tila:
    - **Avaa**: suositus ei ole vielä osoitettu.
    - **Meneillään**: suositus käytetään tällä hetkellä resursseja ja mitään toimia ei tarvita itse.
    - **Ratkaistu**: suositus on jo suoritettu (Tässä tapauksessa rivi näkyy harmaana).
- **Vakavuus**: kerrotaan, että tietty suositus vakavuus:
    - **Suuri**: olemassa kuvaava resurssi (esimerkiksi jokin sovellus, AM tai verkon käyttöoikeusryhmän) ja vaatii huomiota.
    - **Normaali**: on haavoittuvuus ja ei-kriittinen tai muita toimia ei tarvita, se poistamista varten tai Viimeistele prosessi.
    - **Pieni**: salliva olisi käsiteltävä mutta ei edellytä kiireisestä aiheesta. (Oletusarvoisesti pienen suositukset eivät ole esittää, mutta voit suodattaa pienen suositukset, jos haluat nähdä niiden.)

Käytä alla olevassa taulukossa viitteenä voi auttaa sinua ymmärtämään käytettävissä suosituksia ja mitä kukin tehdä, jos haluat käyttää sitä.

> [AZURE.NOTE] Haluat ymmärtämään [perinteinen ja resurssien hallinnan käyttöönotto mallien](../azure-classic-rm.md) Azure resurssien.

|Suositus|Kuvaus|
|-----|-----|
|[Tietojen keräämisen tilaukset ottaminen käyttöön](security-center-enable-data-collection.md)|Suosittelee tietojen keräämisen suojauskäytäntö kaikista tilauksistasi ja kaikki näennäiskoneiden (VMs) käyttöönotto-tilausten.|
|[Riskien OS heikkouksien parantaminen](security-center-remediate-os-vulnerabilities.md)|Tasaa OS-määrityksiä ja suositellut määritykset-sääntöjä suosittelee esimerkiksi salli salasanojen tallentamista.|
|[Järjestelmän päivitykset](security-center-apply-system-updates.md)|Suosittelee, että otat puuttuu tietoturvan ja kriittiset päivitykset käyttöön VMs.|
|[Uudelleenkäynnistyksen jälkeen Järjestelmäpäivitykset](security-center-apply-system-updates.md#reboot-after-system-updates)|Suosittelee, että käynnistät Viimeistele järjestelmän päivitysten AM.|
|[Web-sovelluksen palomuurin lisääminen](security-center-add-web-application-firewall.md)|Suosittelee, että asennat web päätepisteet web application palomuuri-(WAF). Voit suojata useita web-sovellusten Tietoturvakeskuksessa lisäämällä aiemmin WAF käyttöönoton näiden sovellusten. WAF laitteiden (luodaan käyttämällä resurssien hallinnan käyttöönottomalli) on erillinen virtual verkon käyttöön. WAF laitteiden (luotu perinteinen käyttöönoton mallin) on rajoitettu verkon käyttöoikeusryhmän avulla. Tuki laajennetaan mukautetun käyttöönottoon WAF laitteen, (perinteinen) myöhemmin. Tietoturvakeskus suosittelee valmistella WAF suojata vastaan kalastelu kohdistamisen web-sovellusten VMs ja sovelluksen palvelun ympäristössä (Ietokannan). Saat lisätietoja Ietokannan on [Sovelluksen palvelun ympäristön ohjeet](../app-service/app-service-app-service-environments-readme.md). |
|[Viimeistele sovellusten suojaus](security-center-add-web-application-firewall.md#finalize-application-protection)|Viimeistele määritys, WAF, liikenne on reititetään WAF laitteen-uudelleen. Tämä suositus seuraavan Suorita tarvittavat asetuksia.|
|[Lisää seuraava luonti palomuuri](security-center-add-next-generation-firewall.md)|Suosittelee, että lisäät seuraava luonti-palomuurin (NGFW) Microsoft-kumppanilta, jotta voit parantaa suojausta suojaukset.|
|[Reitin liikenne NGFW vain](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Suosittelee, että määrität verkon suojauksen ryhmän (NSG) säännöt, jotka saapuvan liikenteen pakottaminen oman AM oman NGFW kautta.|
|[Endpoint Protection asentaminen](security-center-install-endpoint-protection.md)|Suosittelee valmistella antimalware ohjelmien VMs (vain Windows VMs).|
|[Ratkaise Endpoint Protection kunto-ilmoitukset](security-center-resolve-endpoint-protection-health-alerts.md)|Suosittelee ratkaista endpoint protection virheet.|
|[Verkon käyttöoikeusryhmät ottaminen käyttöön aliverkosta ja näennäiskoneiden](security-center-enable-network-security-groups.md)|Suosittelee käyttöön NSGs aliverkosta tai VMs.|
|[Vastakkaisten päätepisteen Internet käyttöoikeuksien rajoittaminen](security-center-restrict-access-through-internet-facing-endpoints.md)|Suosittelee, että määrität NSGs saapuvan liikenteen säännöt.|
|[Ota käyttöön tarkistaminen SQL server](security-center-enable-auditing-on-sql-servers.md)|Suosittelee, että otat seurannan Azure SQL-palvelimet (Azure SQL-palvelua vain; ei ole käytössä oman näennäiskoneiden SQL).|
|[Tietokannan SQL tarkistaminen](security-center-enable-auditing-on-sql-databases.md)|Suosittelee, että otat seurannan Azure SQL-tietokannat (Azure SQL-palvelua vain; ei ole käytössä oman näennäiskoneiden SQL).|
|[Läpinäkyvä salauksen käyttöön SQL-tietokannat](security-center-enable-transparent-data-encryption.md)|Suosittelee, että otat salaus SQL-tietokannat (vain Azure SQL-palvelu).|
|[Ota käyttöön AM agentti](security-center-enable-vm-agent.md)|Voit nähdä VMs vaativat AM-agentti. AM-agentti on oltava asennettuna VMs jotta valmistella korjaustiedoston scanning perusaikataulun scanning ja antimalware ohjelmat. AM-agentti asennetaan oletusarvoisesti VMs, jotka on otettu Azure Marketplacesta. Artikkelissa [AM agentti ja laajennukset – osa 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) on tietoja siitä, miten voit asentaa AM-agentti.|
| [Käytä salauksen](security-center-apply-disk-encryption.md) |Suosittelee salaa AM levyjen Azure salauksen (Windows ja Linux VMs) avulla. Salauksen suositellaan OS sekä tiedot että AM asemat.|
|[Yhteyshenkilön tietojen suojaus](security-center-provide-security-contact-details.md) | Suosittelee, että annat suojauksen yhteystiedot kunkin tilauksistasi. Yhteystiedot on sähköpostin sähköpostiosoitteen ja puhelinnumeron lisääminen. Tietoja käytetään ottaa sinuun yhteyttä Jos suojauksen tiimimme löytää resurssit ovat käsiin. |
| [Päivitä käyttöjärjestelmäversio](security-center-update-os-version.md) | Suosittelee päivittäminen (OS)-käyttöjärjestelmäversio Cloud-palvelun käytettävissä viimeisimmän version OS perheen.  Saat lisätietoja pilvipalveluihin artikkelissa [Cloud Services-palvelujen yleiskatsaus](../cloud-services/cloud-services-choose-me.md). |
| [Haavoittuvuuden arviointi ei ole asennettu](security-center-vulnerability-assessment-recommendations.md) | Suosittelee haavoittuvuuden arviointi-ratkaisun asentaminen oman AM. |
| [Riskien heikkouksien parantaminen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Voit nähdä oman AM asennettuihin haavoittuvuuden arviointi-ratkaisun havaitsemien järjestelmän ja sovelluksen heikkouksien. |

Voit suodattaa ja hylätä suosituksia.

1. Valitse **suodattimen** **suositukset** -sivu. **Suodatin** -sivu avautuu ja voit valita haluat nähdä vakavuus ja tila-arvoja.

    ![Suodattimen suositukset][2]

2. Jos päätät, että suositus ei ole käytettävissä, voit hylätä suositus ja suodattamista näkymän ulos. Voit hylätä suositus kahdella eri tavalla. Yksi tapa on napsauttamalla kohdetta hiiren kakkospainikkeella ja valitse sitten **Hylkää**. Toinen tapa on kohteen päälle, napsauta kolmea pistettä, jotka tulevat näkyviin oikealle ja valitse sitten **Hylkää**. Voit tarkastella poistetut suosituksia valitsemalla **Suodata**ja valitsemalla sitten **Dismissed**.

    ![Hylkää suositus][3]

### <a name="apply-recommendations"></a>Käytä suositukset
Kun olet tarkistanut kaikki suositukset, mitkä yhden sinun on asennettava ensin. On suositeltavaa, että voit käyttää Vakavuusluokitus tärkeimmät parametrin arvioida mitä suosituksia käytetään ensimmäisen kerran.

Suosituksia yllä taulukon Valitse suositus ja käy läpi sitä, miten voit käyttää suositus.

## <a name="see-also"></a>Katso myös
Tässä asiakirjassa on tuotu Tietoturvakeskuksessa suositukset suojaus. Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Suojauksen kunnon seuranta Azure Tietoturvakeskus](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) – Katso, miten voit hallita ja vastata suojausvaroitusten.
- [Kumppaniratkaisuihin ja Azure Tietoturvakeskus seuranta](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure Security-blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen kirjoituksia.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
