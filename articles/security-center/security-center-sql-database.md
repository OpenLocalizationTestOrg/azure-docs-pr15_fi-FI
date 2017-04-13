<properties
   pageTitle="Azure Tietoturvakeskuksessa ja Azure SQL-tietokanta-palvelun | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten Tietoturvakeskus voi auttaa sinua suojatun tietokantoja Azure SQL-tietokantaan."
   services="sql-database"
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
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Tietoturvakeskuksessa ja Azure SQL-tietokanta-palvelu

[Azure Tietoturvakeskus](https://azure.microsoft.com/documentation/services/security-center/) auttaa estämään, haku- ja uhkien vastata. Siinä on integroitu suojaus seuranta ja käytäntöjen hallinta Azure-tilauksissa, auttaa tunnistamaan uhkien, jotka muuten huomaamatta ja laaja valikoima security-ratkaisujen mediatyökaluja toimii.

Tässä artikkelissa kerrotaan, miten Tietoturvakeskus voi auttaa sinua suojatun tietokantoja Azure SQL-tietokantaan.

## <a name="why-use-security-center"></a>Tietoturvakeskus käyttötarkoitus

Tietoturvakeskus avulla voit suojata SQL-tietokannan tietojen antamalla näkyvyys palvelimia ja tietokantoja suojaus. Tietoturvakeskus voit tehdä seuraavia toimia:

- Määritä käytännöt SQL-tietokannan salaus ja valvonta.
- Seurata kaikkia tilauksissa SQL-tietokantaan resurssien suojaus.
- Nopeasti ja riskien parantaminen suojauksen ongelmat.
- Integroi [Azure SQL-tietokanta uhkien tunnistus](../sql-database/sql-database-threat-detection-get-started.md)ilmoitukset.

Lisäksi suojaaminen SQL-tietokanta-resursseja myös Tietoturvakeskus suojauksen valvonnan ja hallinnan Azuren näennäiskoneiden, pilvipalveluihin, sovelluksen Services, virtual verkkojen ja muihin. Lisätietoja Tietoturvakeskus [tähän](security-center-intro.md).

## <a name="prerequisites"></a>Edellytykset

Aloita Tietoturvakeskus, jos sinulla on Microsoft Azure-tilausta. Tilaus on käytössä Tietoturvakeskus vapaa taso. Saat lisätietoja Security Center vapaa ja vakio tasoa [Security Center hinnat](https://azure.microsoft.com/pricing/details/security-center/).

Tietoturvakeskus tukee Roolipohjainen käytön. Lisätietoja Roolipohjainen käyttöoikeuksien valvonta (RBAC) Azure-tietokannassa on artikkelissa [Azure Active Directory Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md). Suojauksen Center usein kysytyt kysymykset [käyttöoikeudet käsittelytavan Tietoturvakeskuksessa](security-center-faq.md#how-are-permissions-handled-in-azure-security-center)on tietoja.

## <a name="access-security-center"></a>Access-Tietoturvakeskus

Voit käyttää Tietoturvakeskus [Azure portal](https://azure.microsoft.com/features/azure-portal/). [Kirjaudu sisään-portaaliin](https://portal.azure.com/) ja valitse **Tietoturvakeskus-vaihtoehto**.

![Tietoturvakeskus-vaihtoehto][1]

**Tietoturvakeskus** -sivu avautuu.
![Tietoturvakeskus-sivu][2]

## <a name="set-security-policy"></a>Määritä suojauskäytäntö

Suojaus-käytännöllä määritetään ohjausobjektijoukkoon, on suositeltavaa resurssien määritetyn tilaus tai resurssiryhmä. Tietoturvakeskus voit määrittää jäsenyydelle tilaukset tai resurssiryhmien mukaan yrityksen tarpeiden ja sovellusten tyypin tai suojaustasoa jokaisen tilauksen tiedot.

Voit määrittää käytännön Näytä suositukset SQL tarkistaminen ja SQL läpinäkyvä salauksen (TDE).

- Kun otat **SQL tarkistaminen ja uhkien havaitseminen**, Tietoturvakeskus suosittelee, että valvonnan Azure-tietokannan käytössä yhteensopivuuden takia advanced tunnistaminen ja tutkimuksen varten.
- Kun otat käyttöön **SQL läpinäkyvä salauksen**, Tietoturvakeskus suosittelee, että salaus rest-palvelussa käyttöön Azure SQL-tietokanta, liitetty varmuuskopiot ja tapahtumalokitiedostot.

Määritä suojauskäytäntö valitsemalla Tietoturvakeskus sivu **Policy** -ruutua. Valitse tilaus, johon haluat ottaa käyttöön suojauskäytäntö **suojauskäytäntö** -sivu. Valitse **estävään käytäntöön** ja ottaa **käyttöön** suojauksen suositukset, jota haluat käyttää tätä tilausta.
![Suojauskäytäntö][3]

Lisätietoja on kohdassa [suojauskäytäntöjä](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Suojauksen suositus hallinta

Tietoturvakeskus analysoi säännöllisesti Azure resurssien suojaus-tilaan. Kun Tietoturvakeskus havaitsee mahdollisen tietoturva-aukkoja, Luo suosituksia. Suosituksia opastaa prosessin määrittämiseen tarvittavat ohjausobjektit.

Kun olet määrittänyt suojauskäytäntö, Tietoturvakeskus analysoi tunnistaa mahdolliset heikkouksien resurssien suojaus-tilaan. Suosituksia näkyvät taulukkomuodossa, jossa kunkin rivin edustaa yhtä tietyn suositus. Auttavat ymmärtämään Azure SQL-tietokanta ja kunkin suositus mitä jos käytön käytettävissä suosituksia viitteenä seuraavan taulukon avulla. Suositus valitsemalla Avaa artikkeliin, jossa kerrotaan, miten suositus täytäntöön Tietoturvakeskuksessa.

| Suositus | Kuvaus |
| ----- | ----- |
| [Ota käyttöön SQL-palvelimiin valvonta ja uhkien tunnistus](security-center-enable-auditing-on-sql-servers.md) | Suosittelee ottaminen tarkistaminen ja uhkien tunnistus SQL-tietokanta-palvelimia. (Vain SQL-tietokanta-palvelu. "Ei sisällä Microsoft SQL Server-tietokoneessa oman näennäiskoneiden.) |
| [SQL-tietokantoja valvonta ja uhkien tunnistuksen ottaminen käyttöön](security-center-enable-auditing-on-sql-databases.md) | Suosittelee ottaminen tarkistaminen ja uhkien tunnistus SQL-tietokanta-tietokantoja varten. (Vain SQL-tietokanta-palvelu. "Ei sisällä Microsoft SQL Server-tietokoneessa oman näennäiskoneiden.) |
| [Läpinäkyvä salauksen ottaminen käyttöön](security-center-enable-transparent-data-encryption.md) | Suosittelee käyttöön SQL-tietokantoja salausta. (SQL-tietokanta-palvelu ainoastaan.) |

Voit avata suosituksia Azure-resursseissa valitsemalla Tietoturvakeskus sivu **suositukset** -ruutu. Valitse tiedot näkyviin suositus **suositukset** -sivu. Tässä esimerkissä Valitse **Ota käyttöön valvonta ja uhkien tunnistus SQL-palvelimiin**.

![Suosituksia][4]

Alla kuvatulla Tietoturvakeskus näyttää SQL-palvelimet, jossa tarkistaminen ja uhkien tunnistus ei ole otettu käyttöön. Kun otat tarkistaminen, voit määrittää uhkien tunnistaminen ja sähköpostiasetusten, suojauksen ilmoituksia. Uhkien tunnistus ilmoittaa havaitessaan erheellisiin tietokannan toimintoja, jotka osoittavat mahdollisten suojauksen uhkien tietokantaan. Ilmoitukset näkyvät Tietoturvakeskus Raporttinäkymät-ikkunan.
![Valvonta- ja uhkien tunnistus][5]

Noudattamalla [aloittaminen SQL-tietokannan uhkien tunnistuksen](../sql-database/sql-database-threat-detection-get-started.md) ottaminen käyttöön ja määrittäminen uhkien tunnistaminen ja voit määrittää sähköpostin, joka vastaanottaa suojausvaroitusten tunnistus erheellisiin toiminnan yhteydessä luettelon.

Lisätietoja suositukset-kohdassa [hallinta suojausta koskevia suosituksia](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Suojauksen kunnon valvonta

Kun otat [suojauskäytäntöjä](security-center-policies.md) resurssien lisääminen tilaukseen, Tietoturvakeskus analysoida resurssien tunnistaa mahdolliset heikkouksien suojaus.  Voit tarkastella resurssien suojauksen tilan **resurssin suojauksen kunto** -ruutu. Kun napsautat **tiedot** **resurssin suojauksen kunto** -ruutu, **Tietojen resurssit** -sivu avautuu SQL suosituksia ongelmat, kuten valvonnan ja läpinäkyvä salauksen eivät ole käytössä. Siinä on myös tietokannan Yleiset kuntotietojen suosituksia.
![Resurssin suojaus kunto][6]

Lisätietoja on artikkelissa [Suojaus kunnon valvonta](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Hallinta ja suojausvaroitusten viesteihin vastaaminen

Tietoturvakeskus automaattisesti kerää analysoi ja integroi [Azure SQL-uhkien tunnistus](../sql-database/sql-database-threat-detection-get-started.md), samoin kuin Azure resursseihin tunnistaa reaali uhkien ja vähentää tunnistettujen lokitiedot. Priorisoidut suojausvaroitusten luettelo näkyy Tietoturvakeskus sekä tutkia nopeasti ongelma ja miten riskien parantaminen hyökkäys suosituksia tarvitsemasi tiedot.

Katso ilmoitukset, valitse Tietoturvakeskus sivu **suojausvaroitusten** -ruutua. Valitse ilmoituksen, Lisätietoja tapahtumista, joka on käynnistänyt ilmoituksen ja mitä, jos jokin vaiheet, sinun on tehtävä, riskien parantaminen hyökkäys **suojausvaroitusten** -sivu. Tässä esimerkissä Valitse **mahdolliset SQL lisäämisen**.
![Suojausvaroitusten][7]

Katso alla Tietoturvakeskuksessa on lisätiedot, jotka tarjoavat huomioon mitä hälytyksen, kohde resurssin tarvittaessa lähde-IP-osoite ja suosituksista riskien parantaminen.
![Mahdollinen SQL-lisäämisen][8]

Lisätietoja on artikkelissa [hallinta ja niihin vastaaminen suojausvaroitusten](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Suojauksen Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Tietoturvakeskus suunnittelu ja toimintojen guide](security-center-planning-and-operations-guide.md) - optimoi käyttöä koskevien Tietoturvakeskuksessa on joukko vaiheet ja tehtävät seuraa organisaation suojausvaatimukset ja cloud hallintamallin perusteella.
- [Suojauksen Center tietojen suojauksen](security-center-data-security.md) – Katso, miten Tietoturvakeskus kerää ja käsittelee Azure resurssit, kuten kokoonpanotietoja, metatietoja, tapahtumalokien, kaatumisen vedostiedostot ja lisää tietoja.
- [Suojauksen tapaukset käsittely](security-center-incident.md) - Opettele helpottaa käsittely suojausasetukset tapaukset Tietoturvakeskus suojauksen ilmoitusten ominaisuuksien avulla.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
