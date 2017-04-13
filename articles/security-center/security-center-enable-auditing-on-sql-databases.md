<properties
   pageTitle="Valvoa SQL-tietokantoja Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tämän asiakirjan avulla voit **valvoa SQL-tietokantoja**Azure Tietoturvakeskus suositus täytäntöön."
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

# <a name="enable-auditing-on-sql-databases-in-azure-security-center"></a>Valvoa Tietoturvakeskuksessa Azure SQL-tietokannat

Azure Tietoturvakeskuksessa suositeltavaa ottaa valvonnasta on kaikki SQL-tietokantoja, jos tarkistaminen ei ole jo käytössä. Seurannan avulla voit säilyttää säädösten noudattamisen, ymmärtää tietokannan toiminta ja Hanki tietoja ristiriidat ja poikkeamia, joka saattaa johtua business koskee tai epäilty suojauksen virheitä.

Kun olet ottanut käyttöön seuranta voit määrittää uhkien tunnistus asetukset ja sähköpostit vastaanottamaan suojausvaroitusten. Uhkien tunnistus havaitsee erheellisiin tietokannan toimintaa, joka osoittaa mahdolliset suojauksen uhkien tietokantaan. Näin voit tunnistaa ja viesteihin vastaaminen mahdollisten uhkien ilmenee.

Tämä suositus pätee vain; Azure SQL-palveluun ei ole käytössä oman näennäiskoneiden SQL.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Ota käyttöön valvonta SQL-tietokantoja** **suositukset** -sivu.  **Ota käyttöön valvonta SQL-tietokantoja** -sivu avautuu.
![Valvoa SQL-tietokannat][1]

2. Valitse SQL-tietokanta seurannan ottaminen käyttöön. **Valvonta- ja uhkien tunnistaminen** -sivu avautuu.
![Valvonta- ja uhkien tunnistus][2]
3. Valitse **valvonta- ja uhkien tunnistus** , sivu **edelleen** **valvonta**-kohdassa.
![Valvonta- ja uhkien tunnistuksen ottaminen käyttöön][3]


5. Noudattamalla [aloittaminen SQL-tietokannan uhkien tunnistuksen](../sql-database/sql-database-threat-detection-get-started.md) ottaminen käyttöön ja määrittäminen uhkien tunnistaminen ja määrittämään sähköpostit, joka vastaanottaa suojausvaroitusten tunnistus erheellisiin toiminnan yhteydessä luettelo.

## <a name="see-also"></a>Katso myös

Tässä artikkelissa osoittanut ottamisesta käyttöön Tietoturvakeskus suositus "Käyttöön valvonnan SQL-tietokantoja." Saat lisätietoja SQL-tietokannan suojaaminen on seuraavissa artikkeleissa:

- [SQL-tietokannan suojaaminen](../sql-database/sql-database-security.md)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – uusimmista Azure suojaus ja tiedot.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]:./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection.png
[3]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
