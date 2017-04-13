<properties
   pageTitle="Azure Tietoturvakeskus ja Azure-Virtuaalikoneissa | Microsoft Azure"
   description="Tämän asiakirjan auttaa sinua ymmärtämään, kuinka Azure Tietoturvakeskuksessa voit suojaamiseksi Azuren näennäiskoneiden."
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
   ms.date="10/07/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-and-azure-virtual-machines"></a>Azure Tietoturvakeskus ja Azure-Virtuaalikoneissa

[Azure Tietoturvakeskus](https://azure.microsoft.com/services/security-center/) auttaa estämään, haku- ja uhkien vastata. Siinä on integroitu suojaus seuranta ja käytäntöjen hallinta Azure-tilauksissa, auttaa tunnistamaan uhkien, jotka muuten huomaamatta ja laaja valikoima security-ratkaisujen mediatyökaluja toimii.

Tässä artikkelissa kerrotaan, miten Tietoturvakeskus voi auttaa sinua suojattua yhteyttä Azuren näennäiskoneiden (AM).

## <a name="why-use-security-center"></a>Tietoturvakeskus käyttötarkoitus

Tietoturvakeskus avulla voit suojata virtuaalikoneen tietojen Azure antamalla näkyvyys virtuaalikoneen käyttäjän tietoturva-asetukset. Kun Tietoturvakeskus valvontaa oman VMs, seuraavat ominaisuudet ovat käytettävissä:

- Suositellut määritykset sääntöjen käyttöjärjestelmän (OS) suojausasetukset
- Järjestelmän suojauksen ja tärkeät päivitykset, joita ei ole
- Endpoint protection suositukset
- Levyn salaus vahvistus
- Haavoittuvuuden arviointi ja korjaus
- Uhkien tunnistus

Lisäksi suojaaminen Azure VMs myös Tietoturvakeskus suojauksen valvonnan ja hallinnan pilvipalveluihin, sovelluksen Services tai Virtual verkkoja varten. 

>[AZURE.NOTE] Artikkelissa [Johdanto Azure Tietoturvakeskus](security-center-intro.md) lisätietoja Azure Tietoturvakeskuksessa.

## <a name="prerequisites"></a>Edellytykset

Aloita Azure Tietoturvakeskus, sinun on tiedettävä ja toimi seuraavasti:

- Sinulla on Microsoft Azure-tilausta. Saat lisätietoja Security Center vapaa- ja perusversioilla tasoa [Security Center hinnat](https://azure.microsoft.com/pricing/details/security-center/) .
- Tietoturvakeskus käyttöönoton suunnitteleminen, katso lisätietoja suunnittelu ja toimintojen huomioon otettavia seikkoja [Azure Tietoturvakeskus suunnittelu ja toimintojen opas](security-center-planning-and-operations-guide.md) .
- Lisätietoja käyttöjärjestelmän supportability tiedot, [Azure Tietoturvakeskus usein kysyttyjä kysymyksiä](security-center-faq.md). 

## <a name="set-security-policy"></a>Määritä suojauskäytäntö

Tietojen kerääminen on otettava käyttöön niin, että Azure Tietoturvakeskuksessa voit kerätä suosituksia ja ilmoituksia, jotka on luotu tietojen perusteella määrität suojauskäytäntö. Alla olevassa kuvassa, näet, että **tietojen kerääminen** on jo **käytössä**.

Suojauskäytäntö määrittää ohjausobjektien, jossa on suositeltavaa resurssien määritetyn tilaus tai resurssiryhmä. Ennen kuin otat käyttöön suojauskäytäntö, sinulla on oltava käytössä liittyvä tietojen kerääminen, Tietoturvakeskus kerää tietoja oman näennäiskoneiden arvioida niiden suojaus-tilaan, anna suojausta koskevia suosituksia ja ilmoittaa uhkien. Tietoturvakeskus voit määrittää jäsenyydelle Azure tilaukset tai resurssiryhmien mukaan yrityksen tarpeiden ja sovellusten tyypin tai suojaustasoa jokaisen tilauksen tiedot. 

![Suojauskäytäntö](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

>[AZURE.NOTE] Lisätietoja kunkin **estävään käytäntöön** käytettävissä, katso [määrittäminen suojauskäytäntöjä](security-center-policies.md) artikkelissa.

## <a name="manage-security-recommendations"></a>Hallitse suojausta koskevia suosituksia

Tietoturvakeskus analysoi Azure resurssien suojaus-tilaan. Kun Tietoturvakeskus havaitsee mahdollisen tietoturva-aukkoja, Luo suosituksia. Suosituksia opastaa prosessin määrittämiseen tarvittavat ohjausobjektit.

Määritettyäsi suojauskäytäntö Tietoturvakeskus analysoi tunnistaa mahdolliset heikkouksien resurssien suojaus-tilaan. Suosituksia näkyvät taulukkomuodossa, jossa kunkin rivin edustaa yhtä tietyn suositus. Seuraavassa taulukossa on esimerkkejä suosituksia Azure VMs ja mitä kukin tehdä, jos haluat käyttää sitä. Kun valitset suositus, voit toimitetaan tietoja, joiden avulla voit toteuttaa suositus Tietoturvakeskuksessa.

|Suositus|Kuvaus|
|-----|-----|
|[Tietojen keräämisen tilaukset ottaminen käyttöön](security-center-enable-data-collection.md)|Suosittelee, että otat tietojen keräämisen suojauskäytäntö kaikista tilauksistasi ja kaikki näennäiskoneiden (VMs)-tilausten.|
|[Riskien OS heikkouksien parantaminen](security-center-remediate-os-vulnerabilities.md)|Tasaa OS-määrityksiä ja suositellut määritykset-sääntöjä suosittelee esimerkiksi salli salasanojen tallentamista.|
|[Järjestelmän päivitykset](security-center-apply-system-updates.md)|Suosittelee, että otat puuttuu tietoturvan ja kriittiset päivitykset käyttöön VMs.|
|[Uudelleenkäynnistyksen jälkeen Järjestelmäpäivitykset](security-center-apply-system-updates.md#reboot-after-system-updates)|Suosittelee, että käynnistät Viimeistele järjestelmän päivitysten AM.|
|[Endpoint Protection asentaminen](security-center-install-endpoint-protection.md)|Suosittelee valmistella antimalware ohjelmien VMs (vain Windows VMs).|
|[Ratkaise Endpoint Protection kunto-ilmoitukset](security-center-resolve-endpoint-protection-health-alerts.md)|Suosittelee ratkaista endpoint protection virheet.|
|[Ota käyttöön AM agentti](security-center-enable-vm-agent.md)|Voit nähdä VMs vaativat AM-agentti. AM-agentti on oltava asennettuna VMs jotta valmistella korjaustiedoston scanning perusaikataulun scanning ja antimalware ohjelmat. AM-agentti asennetaan oletusarvoisesti VMs, jotka on otettu Azure Marketplacesta. Artikkelissa [AM agentti ja laajennukset – osa 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) on tietoja siitä, miten voit asentaa AM-agentti.|
| [Käytä salauksen](security-center-apply-disk-encryption.md) |Suosittelee salaa AM levyjen Azure salauksen (Windows ja Linux VMs) avulla. Salauksen suositellaan OS sekä tiedot että AM asemat.|
| [Haavoittuvuuden arviointi ei ole asennettu](security-center-vulnerability-assessment-recommendations.md) | Suosittelee haavoittuvuuden arviointi-ratkaisun asentaminen oman AM. |
| [Riskien heikkouksien parantaminen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Voit nähdä oman AM asennettuihin haavoittuvuuden arviointi-ratkaisun havaitsemien järjestelmän ja sovelluksen heikkouksien. |

>[AZURE.NOTE] Lisätietoja suositukset, katso [hallinta suojausta koskevia suosituksia](security-center-recommendations.md) artikkelissa.

## <a name="monitor-security-health"></a>Suojauksen kunnon valvonta

Kun otat [suojauskäytäntöjä](security-center-policies.md) resurssien lisääminen tilaukseen, Tietoturvakeskus analysoida resurssien tunnistaa mahdolliset heikkouksien suojaus.  Voit tarkastella resurssien sekä mahdolliset ongelmat **resurssin suojauksen kunto** -sivu-suojauksen tilan. Kun napsautat **näennäiskoneiden** **resurssien suojauksen** kunto-ruutu, **näennäiskoneiden** -sivu avautuu oman VMs suosituksia. 

![Suojauksen kunto](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Hallinta ja suojausvaroitusten viesteihin vastaaminen

Tietoturvakeskus automaattisesti kerää analysoi ja integroi lokitiedot Azure resursseja, verkon ja yhdistetyn kumppaniratkaisuihin (kuten palomuurin ja endpoint protection ratkaisuja), tunnistaa reaali uhkien ja vähentää tunnistettujen. Mukaan hyödyntäminen monipuolisen yhteenlaskeminen [tunnistaa](security-center-detection-capabilities.md)Tietoturvakeskuksessa on enää luoda Priorisoidut suojausvaroitusten auttaa ratkaisemaan ongelman ja antaa suosituksia siitä, miten mahdolliset kalastelu riskien parantaminen nopeasti.

![Suojausvaroitusten](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Valitse Suojausvaroitus lisätietoja tapahtuma(t), joka on käynnistänyt ilmoituksen ja mitä, jos jokin vaiheet, sinun on tehtävä, riskien hyökkäys parantaminen. Suojausvaroitusten on ryhmitelty [tyyppi](security-center-alerts-type.md) ja päivämäärän mukaan.


## <a name="see-also"></a>Katso myös

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
