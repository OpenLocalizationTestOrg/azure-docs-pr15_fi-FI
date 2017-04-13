<properties
   pageTitle="Riskien parantaminen OS heikkouksien Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten Azure Tietoturvakeskus suositus **riskien parantaminen OS heikkouksien**täytäntöön."
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

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Riskien OS heikkouksien Azure Tietoturvakeskuksessa parantaminen

Azure Tietoturvakeskus analysoi päivittäin virtuaalikoneen (AM)-käyttöjärjestelmän (OS), joka voi tehdä AM alttiimpi ja sähköpostiosoite näiden heikkouksien määritysmuutoksia suosittelee määrityksiä varten. Saat lisätietoja seurataan tiettyjä määrityksiä [Suositellut määritykset sääntöluetteloa](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Tietoturvakeskus suosittelee ratkaista heikkouksien, kun yhteyttä AM käyttöjärjestelmän määritys ei vastaa suositellut määritykset säännöt.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **riskien parantaminen OS heikkouksien** **suositukset** -sivu.
![Riskien OS heikkouksien parantaminen][1]

    **Riskien parantaminen OS heikkouksien** -sivu avautuu ja näyttää oman VMs OS määritykset, jotka eivät vastaa suositellut määritykset sääntöjen avulla.  Kunkin AM tunnistaa sivu:

   - **Epäonnistui säännöt** --säännöt, jotka AM OS määrittäminen epäonnistui määrä.
   - **VIIMEINEN SKANNATA aika** --päivämäärän ja kellonajan Tietoturvakeskus viimeksi skannattu AM käyttöjärjestelmän määritys.
   - **Tilan** --haavoittuvuuden nykyinen tila:

        - Avaa: Haavoittuvuuden on ei osoitettu vielä
        - Käynnissä: Haavoittuvuuden parhaillaan käytössä, mitään toimia ei tarvita itse
        - Ratkaistu: Haavoittuvuuden jo käsiteltiin (kun ongelma on ratkaistu-merkintä näkyy harmaana)
  - **Vakavuus** --kaikki heikkouksien on määritetty vakavuus pieni, eli haavoittuvuutta olisi käsiteltävä mutta ei edellytä kiireisestä aiheesta.

Valitse AM. Saat kyseisen AM sivu avautuu ja näyttää säännöt, jotka on epäonnistui.
   ![Määritysten säännöt, jotka on epäonnistui][2]

Valitse sääntö. Tässä esimerkissä avulla Valitse **salasana on täytettävä monimutkaisuusvaatimukset**. Sivu avautuu kuvaava epäonnistui sääntö ja niiden vaikutuksesta. Tarkista tiedot ja harkitse käyttöjärjestelmän käyttömahdollisuudet käyttäminen.
  ![Epäonnistuneiden säännön kuvaus][3]

  Tietoturvakeskus käyttää yleisten määritysten luettelointi (CCE) voit määrittää yksilölliset tunnukset määritysten säännöt. Tämä sivu on tarjonnut seuraavat tiedot:

  - NIMI--Säännön nimi
  - VAKAVUUS--CCE vakavuus arvo kriittinen, tärkeitä tai varoitus
  - CCIED--Säännön CCE yksilöllinen tunnus
  - KUVAUS--Säännön kuvaus
  - HAAVOITTUVUUS--Selitys haavoittuvuus tai jos sääntö ei ole käytetty riskin
  - VAIKUTUS--Business vaikutus kun sääntöä käytetään
  - ODOTETUN arvon--Kun Tietoturvakeskus analysoi AM OS kokoonpanosi vastaan säännön arvo
  - SÄÄNNÖN toiminto--Säännön toiminto käyttämä Tietoturvakeskus AM OS kokoonpanosi vastaan säännön analyysin aikana
  - Todellinen arvo--AM OS kokoonpanosi vastaan säännön analyysi palautetaan arvo
  - ARVIOINNIN tuloksen –-analyysin tulos: välittää, epäonnistuvat


## <a name="see-also"></a>Katso myös

Tässä artikkelissa osoittanut ottamisesta käyttöön Tietoturvakeskus suositus "Remediate OS heikkouksien." Voit tarkastella määritysten sääntöryhmän [tähän](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Tietoturvakeskus käyttää CCE (yleisten määritysten luettelointi) voit määrittää yksilölliset tunnukset määritysten säännöt. Lisätietoja on [CCE](http://cce.mitre.org) sivustossa.

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi blogi kirjaa tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
