<properties
   pageTitle="Suojauksen yhteyshenkilön tietojen Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten antamaan suojauksen yhteystietoja Azure Tietoturvakeskuksessa."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="provide-security-contact-details-in-azure-security-center"></a>Anna suojauksen yhteystietoja Azure Tietoturvakeskus

Azure Tietoturvakeskuksessa suosittelee, että annat suojauksen yhteystietoja Azure tilauksen, jos omistajuus on vielä vahvistamatta. Näitä tietoja käytetään Microsoft ottaa sinuun yhteyttä Jos Microsoftin Security vastauksen Center (MSRC) löytää asiakastietojen käyttää lainvastaiseen tai luvattoman osapuolen. MSRC suorittaa Azure verkon ja infrastruktuurin seuranta Valitse Suojaus ja niistä uhkien liiketoimintatietojen ja väärinkäyttö valitusten kolmansille osapuolille.

Päivittäinen esiintymä ilmoituksen, ja vain hyvin vakavuus ilmoitukset lähetetään sähköposti-ilmoituksen. Tilauksen koskevat vain voi määrittää sähköpostin asetukset. Resurssin ryhmän tilauksen Peri asetuksia.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **suositukset** -sivu, **Anna suojauksen yhteyshenkilön tiedot**.
![Anna suojauksen yhteyshenkilö][1]

2. **Anna suojauksen yhteyshenkilön tiedot**-sivu avautuu. Valitse yhteyshenkilön tietoja Azure tilaus.
![Yhteyshenkilön tietojen suojaus][2]

3. Toinen **suojauksen yhteyshenkilön tiedot** -sivu avautuu.

  - Kirjoita yhteyshenkilön sähköpostiosoite tai osoitteet pilkuilla erotettuina. Ei ole sähköpostiosoitetta, voit syöttää numeron rajoitukset.
  - Kirjoita yhden suojauksen yhteyshenkilön puhelinnumeron Kansainvälinen numero.
  - Saat tietoja hyvin vakavuus ilmoituksista sähköpostit, **Lähetä minulle sähköpostit tietoja ilmoituksista**toiminnon mahdollinen käyttöönotto.
  - Myöhemmin käytössäsi on vaihtoehto Lähetä sähköposti-ilmoitusten tilauksen omistaja. Tämä asetus on tällä hetkellä harmaana.
  - Valitse **OK** suojaus-yhteystietojen käyttäminen tilauksessa.

## <a name="see-also"></a>Katso myös

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – uusimmista Azure suojaus ja tiedot.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
