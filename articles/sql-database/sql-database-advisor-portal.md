<properties 
   pageTitle="Azure SQL-tietokannan Advisor Azure-portaalissa | Microsoft Azure" 
   description="Voit Azure portaalin Azure SQL-tietokanta-Advisor tarkistaa ja toteuttaa suosituksia, joka voi parantaa nykyisen kyselyn suorituskykyä aiemmin SQL-tietokannat." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor-using-the-azure-portal"></a>SQL-tietokannan Advisor Azure-portaalissa

> [AZURE.SELECTOR]
- [SQL-tietokannan Advisor yleiskatsaus](sql-database-advisor.md)
- [Portal](sql-database-advisor-portal.md)

Voit Azure portaalin Azure SQL-tietokanta-Advisor tarkistaa ja toteuttaa suosituksia, joka voi parantaa nykyisen kyselyn suorituskykyä aiemmin SQL-tietokannat.

## <a name="viewing-recommendations"></a>Suosituksia tarkasteleminen

Suositukset-sivu on kohtaa, johon voit tarkastella yläreunan suositukset perusteella niiden mahdollinen vaikutus suorituskyvyn parantamiseksi. Voit myös tarkastella historiallisten toimintojen tila. Valitse suositus tai tila, niin saat näkyviin tarkempia.

Voit tarkastella ja käyttää suositukset, sinun on Azure [Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md) -käyttöoikeudet. **Lukija** **SQL DB osallistujan** oikeudet ovat Näytä suositukset ja **omistaja**, suorittaa toimintoja, tarvitaan **SQL DB osallistuja** -oikeudet Luo tai poista indeksit ja peruuttaa indeksien luominen.

1. Kirjautuminen [Azure portal](https://portal.azure.com/).
2. Valitse **Lisää palveluja** > **SQL-tietokantoja**, ja valitse tietokanta.
5. Valitse **Suorituskyky suositus** voit tarkastella käytettävissä tietokannasta valittujen koskevia suosituksia.

> [AZURE.NOTE] Saat suosituksia tietokannan on oltava tietoja päiväksi käyttö ja siellä on oltava joitakin tehtävän. On myös on oltava joitakin yhdenmukaisia tehtävän. Advisor SQL-tietokantaan voit helpommin optimoida yhdenmukaisia kyselyn kuviot, kuin se voi satunnaisia Laikukas bursts toiminnan. Jos suositukset eivät ole käytettävissä, **Suorituskyky suositus** sivun annettava selvitys siitä, miksi viestin.

![Suosituksia](./media/sql-database-advisor-portal/recommendations.png)

Tässä on esimerkki "korjaa ongelman rakenne-suositus Azure-portaalissa.

![Korjaa ongelma rakenne](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Suosituksia lajitellaan niiden mahdollisen suorituskyvyn seuraavat neljä luokkiin vaikutus:

| Vaikutus | Kuvaus |
| :--- | :--- |
| Suuri | Suuri vaikutus suosituksia annettava tärkein vaikutus suorituskykyyn. |
| Normaali | Normaali vaikutus suositukset olisi parantaa suorituskykyä, mutta eivät huomattavasti. |
| Pieni | Pieni vaikutus suosituksia annettava ilman kuin parantaa suorituskykyä, mutta parannukset ei ole merkitsevä. 


### <a name="removing-recommendations-from-the-list"></a>Suosituksia poistaminen luettelosta

Jos suosituksia luettelo sisältää kohteet, jotka haluat poistaa luettelossa, voit hylätä suositus:

1. Valitse suositus **suositukset**-luettelosta.
2. Valitse **tiedot** -sivu valitsemalla **Hylkää** .


Halutessasi voit lisätä käytöstä poistetut kohteet takaisin **suositukset** -luettelossa:

1. **Suositukset** -sivu valitsemalla **Näytä poistetaan**.
1. Valitse hylättyjen kohde luettelosta ja tarkastella sen tietoja.
1. Valitse, voit lisätä hakemiston **suosituksia**tärkeimmät luettelo **Kumoa hylkää** .



## <a name="applying-recommendations"></a>Suosituksia käyttäminen

SQL-tietokannan Advisor Saat täydet oikeudet miten suositukset ovat käytössä jollakin seuraavista kolmesta vaihtoehdosta: 

- Käytä yksittäisiä suosituksia yksi kerrallaan.
- Voit käyttää automaattisesti suosituksia advisor ottaminen käyttöön (tällä hetkellä koskee vain indeksi suosituksia).
- Toteuttamisesta suositus manuaalisesti, suorita suositellut T-SQL-komentosarja vastaan tietokannan.

Valitse mikä tahansa suositus tarkastella sen tietoja ja valitse sitten **Näytä komentosarja** Tarkista tarkat tiedot siitä, miten suositus luodaan.

Tietokannan pysyy online aikana advisor koskee suositus--käyttämällä SQL-tietokannan Advisor koskaan kestää tietokannan offline-tilassa.

### <a name="apply-an-individual-recommendation"></a>Käytä yksittäisiä suositus

Voit tarkistaa ja hyväksyä suosituksia yksi kerrallaan.

1. Valitse suositus **suositukset** -sivu.
2. Valitse **tiedot** -sivu valitsemalla **Käytä**.

    ![Käytä suositus](./media/sql-database-advisor-portal/apply.png)

### <a name="enable-automatic-index-management"></a>Ota käyttöön indeksien automaattinen hallinta

Voit määrittää SQL-tietokannan Advisor toteuttamisesta suosituksia automaattisesti. Suosituksia ole käytettävissä, kun ne automaattisesti käyttöön. Kun kaikki indeksi työvaiheita hallitsee palvelu, jos vaikutus suorituskykyyn on negatiivinen suositus palautetaan.

1. Napsauta **automatisoiminen** **suositukset** -sivu:

    ![Advisor asetukset](./media/sql-database-advisor-portal/settings.png)

2. Määritä advisor automaattisesti **luominen** tai **Pudota** indeksit:

    ![Suositellut indeksit](./media/sql-database-advisor-portal/automation.png)


### <a name="manually-run-the-recommended-t-sql-script"></a>Suositeltu T-SQL-komentosarjan suorittaminen manuaalisesti

Valitse mikä tahansa suositus ja valitse sitten **Näytä komentosarja**. Suorittamalla tätä sovelletaan suositus tietokannan vastaan.

*Indeksit, joka suoritetaan manuaalisesti seurataan ja vahvistaa suorituskykyyn vaikuttavia palvelu ei ole* niin ehdotetaan valvoa näiden indeksien vahvistamiseksi ne parantaa suorituskykyä ja muuttaa tai poistaa niitä tarvittaessa luonnin jälkeen. Lisätietoja indeksien luomisesta on artikkelissa [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).


### <a name="canceling-recommendations"></a>Suosituksia peruuttaminen

Suosituksia, jotka ovat **odotetaan**, **tarkistetaan**tai **Success** tila voidaan peruuttaa. Suosituksia **suoritetaan** tila ei voi peruuttaa.

1. Valitse suositus **Säätäminen historia** -alueessa Avaa **suosituksia tiedot** -sivu.
2. Valitse **Peruuta** , jos haluat keskeyttää käyttämistä suositus.



## <a name="monitoring-operations"></a>Toimintojen seuranta

Käyttämällä suositus ei ehkä suoriteta välittömästi. Portaalissa on suositus toimintojen tila. Mahdollisia tiloja indeksin tallennettavien ovat seuraavat:

| Tila | Kuvaus |
| :--- | :--- |
| Odotetaan | Käytä suositus-komento on vastaanotettu ja ajoitetaan suorittamisen. |
| Suorittaminen | Suositus käytetään. |
| Onnistui | Suositus onnistui. |
| Virhe | Virhe käyttöönottoa suositus aikana. Lyhytkestoisia ongelman tai mahdollisesti rakenteen muuttaminen taulukkoon ja komentosarja ei ole enää voimassa. |
| Palautetaan | Suositus on otettu käyttöön, mutta ei performant katsottu ja palautetaan automaattisesti. |
| Palautettu | Suositus on palautettu. |

Valitse prosessin suositus luettelosta Lisätietoja:

![Suositellut indeksit](./media/sql-database-advisor-portal/operations.png)


### <a name="reverting-a-recommendation"></a>Palautetaan suositus

Jos käyttää suositus Advisorin avulla (eli et manuaalisesti suorittanut T-SQL-komentosarja) sen automaattisesti palauttaa sen jos se havaitsee negatiivisen vaikutus suorituskykyyn. Jos haluat palata suositus jostakin syystä voit suorittaa seuraavia:


1. Valitse onnistuneesti käytetään suositus **säädön historia** -alueessa.
2. Valitse **Palauta** **suositus tiedot** -sivu.

![Suositellut indeksit](./media/sql-database-advisor-portal/details.png)


## <a name="monitoring-performance-impact-of-index-recommendations"></a>Suorituskykyä indeksin suosituksia vaikutus seuranta

Kun suosituksia onnistuneesti toteutettu (tällä hetkellä indeksoida toiminnot ja parameterize vain kyselyjen suosituksia) voit valita **Kyselyn tiedot** Avaa [Kyselyn suorituskykyä tiedot](sql-database-query-performance.md) ja katsoa yläreunan kyselyjen suorituskykyä vaikutus suositus tiedot-sivu.

![Näytön suorituskykyyn vaikuttavia](./media/sql-database-advisor-portal/query-insights.png)



## <a name="summary"></a>Yhteenveto

SQL-tietokannan Advisor neuvoo SQL-tietokannan suorituskyvyn parantamiseen. Antamalla T-SQL-komentosarjat yksittäiset ja täysin-automaattinen (tällä hetkellä vain hakemisto), sekä advisor avustaa hyödyllisiä optimointi tietokannan ja parantaa kyselyn suorituskykyä kädessä.



## <a name="next-steps"></a>Seuraavat vaiheet

Valvoa suosituksia ja käyttää niitä tarkentaa suorituskyvyn jatkaa. Tietokannan työmääriä ovat dynaamisia ja muuta jatkuvasti. SQL-tietokantaan advisor säilyvät valvoa ja anna suositukset, voit mahdollisesti parantaa tietokannan suorituskykyä. 

 - Katso [SQL-tietokannan Advisor](sql-database-advisor.md) yleiskatsaus SQL-tietokannan Advisor.
 - Katso lisätietoja tarkasteleminen yläreunan kyselyjen suorituskykyä vaikutus [Kyselyn suorituskykyä tiedot](sql-database-query-performance.md) .

## <a name="additional-resources"></a>Lisäresursseja

- [Kyselyn säilö](https://msdn.microsoft.com/library/dn817826.aspx)
- [INDEKSIN LUOMINEN](https://msdn.microsoft.com/library/ms188783.aspx)
- [Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md)






