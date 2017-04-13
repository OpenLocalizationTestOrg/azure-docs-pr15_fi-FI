<properties
   pageTitle="Ota läpinäkyvä salauksen Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan Azure Tietoturvakeskus suositus **Läpinäkyvä salauksen käyttöön**ottamisesta käyttöön."
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

# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Läpinäkyvä salauksen Azure Tietoturvakeskuksessa ottaminen käyttöön

Azure Tietoturvakeskus suosittelee, että otat läpinäkyvä tietojen salaus (TDE) SQL-tietokantoja, jos TDE ei ole jo käytössä. TDE suojaa tietoja ja auttaa vaatimukset täyttävät salaamalla tarvitsematta sovelluksen muutosten tietokannan, liitetty varmuuskopiot ja tapahtuman lokitiedostojen rest-palvelussa. Lisätietoja Lisää on [Läpinäkyvä salauksen Azure SQL-tietokantaan](https://msdn.microsoft.com/library/dn948096).

Tämä suositus pätee vain; Azure SQL-palveluun ei ole käytössä oman näennäiskoneiden SQL.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Ota läpinäkyvä salauksen** **suositukset** -sivu.
![Läpinäkyvä salauksen ottaminen käyttöön][1]

2. **Ota käyttöön läpinäkyvä salauksen SQL-tietokantoja** -sivu avautuu. Valitse SQL-tietokanta TDE ottaminen käyttöön.
![Valitse SQL DB TDE ottaminen käyttöön][2]
3. Valitse **Läpinäkyvä salauksen** -sivu tietojen salaus-kohdasta **edelleen** ja valitse **Tallenna** sivu yläreunan valintanauhassa.
![Ota käyttöön TDE][3]

  Kun TDE on otettu käyttöön valitun SQL-tietokantaan, **salauksen tila** muuttuu **salattu**.    

  ![Salauksen tila][4]

## <a name="see-also"></a>Katso myös

Tässä artikkelissa osoittanut ottamisesta käyttöön Tietoturvakeskus suositus "Käyttöön läpinäkyvä salauksen." Saat lisätietoja SQL TDE on seuraavissa artikkeleissa:

- [Läpinäkyvä salauksen Azure SQL-tietokantaan](https://msdn.microsoft.com/library/dn948096)
- [Aloita kanssa läpinäkyvä tietojen salaus (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – uusimmista Azure suojaus ja tiedot.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
