<properties
   pageTitle="SQL Data Warehouse Transact-SQL-kielen elementtien | Microsoft Azure"
   description="Luettelo linkeistä Transact-SQL-kielen-elementit, joita käytetään SQL-tietovarasto viittaus sisällön."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/08/2016"
   ms.author="barbkess;sonyama;kevin"/>

# <a name="language-elements"></a>Kieli-osat

## <a name="core-elements"></a>Core osat

- [syntaksi nimeämiskäytännön](https://msdn.microsoft.com/library/ms177563.aspx)
- [objektin nimeämiskäytäntö säännöt](https://msdn.microsoft.com/library/ms175874.aspx)
- [varatut avainsanat](https://msdn.microsoft.com/library/ms189822.aspx)
- [lajittelut](https://msdn.microsoft.com/library/ff848763.aspx)
- [kommentit](https://msdn.microsoft.com/library/ms181627.aspx)
- [vakiot](https://msdn.microsoft.com/library/ms179899.aspx)
- [tietotyypit](https://msdn.microsoft.com/library/ms187752.aspx)
- [SUORITA](https://msdn.microsoft.com/library/ms188332.aspx)
- [lausekkeet](https://msdn.microsoft.com/library/ms190286.aspx)
- [POISTA](https://msdn.microsoft.com/library/ms173730.aspx)
- [KÄYTTÄJÄTIETOJEN ominaisuuden Vaihtoehtoinen menetelmä](https://msdn.microsoft.com/library/ms186775.aspx)
- [TULOSTA](https://msdn.microsoft.com/library/ms176047.aspx)
- [KÄYTÄ](https://msdn.microsoft.com/library/ms188366.aspx)

## <a name="batches-control-of-flow-and-variables"></a>Erissä Hallintavuo ja muuttujat

- [ALOITA... LOPETA](https://msdn.microsoft.com/library/ms190487.aspx)
- [SIVUNVAIHTO](https://msdn.microsoft.com/library/ms181271.aspx)
- [MÄÄRITELLÄ@local_variable](https://msdn.microsoft.com/library/ms188927.aspx)
- [JOS... MUITA](https://msdn.microsoft.com/library/ms182717.aspx)
- [RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx)
- [SET@local_variable](https://msdn.microsoft.com/library/ms189484.aspx)
- [PALAUTTAA](https://msdn.microsoft.com/library/ee677615.aspx)
- [KOKEILE... TODELLISEN](https://msdn.microsoft.com/library/ms175976.aspx)
- [AIKAA](https://msdn.microsoft.com/library/ms178642.aspx)

## <a name="operators"></a>Operaattorit
- [+ (Lisää)](https://msdn.microsoft.com/library/ms178565.aspx)
- [+ (Merkkijonon yhdistäminen)](https://msdn.microsoft.com/library/ms177561.aspx)
- [-(Negatiiviset)](https://msdn.microsoft.com/library/ms189480.aspx)
- [-(Vähennä)](https://msdn.microsoft.com/library/ms189518.aspx)
- [* (Kertominen)](https://msdn.microsoft.com/library/ms176019.aspx)
- [/ (Jakolasku)](https://msdn.microsoft.com/library/ms175009.aspx)
- [Modulo](https://msdn.microsoft.com/library/ms190279.aspx)

## <a name="wildcard-characters-to-match"></a>Yleismerkkien merkkejä vastaamaan
- [= (Yhtä suuri kuin)](https://msdn.microsoft.com/library/ms175118.aspx)
- [> (Suurempi kuin)](https://msdn.microsoft.com/library/ms178590.aspx)
- [< (Pienempi kuin)](https://msdn.microsoft.com/library/ms179873.aspx)
- [> = (hienoa tai yhtä suuri kuin)](https://msdn.microsoft.com/library/ms181567.aspx)
- [< = (pienempi tai yhtä suuri kuin)](https://msdn.microsoft.com/library/ms174978.aspx)
- [<> (Eri suuri kuin)](https://msdn.microsoft.com/library/ms176020.aspx)
- [! = (Eri suuri kuin)](https://msdn.microsoft.com/library/ms190296.aspx)
- [JA](https://msdn.microsoft.com/library/ms188372.aspx)
- [VÄLILLÄ](https://msdn.microsoft.com/library/ms187922.aspx)
- [ON OLEMASSA](https://msdn.microsoft.com/library/ms188336.aspx)
- [IN](https://msdn.microsoft.com/library/ms177682.aspx)
- [EI [] NULL](https://msdn.microsoft.com/library/ms188795.aspx)
- [KUTEN](https://msdn.microsoft.com/library/ms179859.aspx)
- [EI](https://msdn.microsoft.com/library/ms189455.aspx)
- [TAI](https://msdn.microsoft.com/library/ms188361.aspx)

### <a name="bitwise-operators"></a>Bittitason operaattorit

- [& (Bittitason ja)](https://msdn.microsoft.com/library/ms174965.aspx)
- [| (Bittitason tai)](https://msdn.microsoft.com/library/ms186714.aspx)
- [^ (Bittitaso poissulkeva tai)](https://msdn.microsoft.com/library/ms190277.aspx)
- [~ (Bittitason ei)](https://msdn.microsoft.com/library/ms173468.aspx)
- [^ = (Bittitason poissulkeva tai yhtä suuri kuin)](https://msdn.microsoft.com/library/cc627413.aspx)
- [| = (Bittitason tai on yhtä suuri kuin)](https://msdn.microsoft.com/library/cc627409.aspx)
- [& = (bittitason ja yhtä suuri kuin)](https://msdn.microsoft.com/library/cc627427.aspx)

## <a name="functions"></a>Funktiot

- [@@DATEFIRST](https://msdn.microsoft.com/library/ms187766.aspx)
- [@@ERROR](https://msdn.microsoft.com/library/ms188790.aspx)
- [@@LANGUAGE](https://msdn.microsoft.com/library/ms177557.aspx)
- [@@SPID](https://msdn.microsoft.com/library/ms189535.aspx)
- [@@TRANCOUNT](https://msdn.microsoft.com/library/ms187967.aspx)
- [@@VERSION](https://msdn.microsoft.com/library/ms177512.aspx)
- [ITSEISARVO](https://msdn.microsoft.com/library/ms189800.aspx)
- [ACOS](https://msdn.microsoft.com/library/ms178627.aspx)
- [ASCII](https://msdn.microsoft.com/library/ms177545.aspx)
- [ASIN](https://msdn.microsoft.com/library/ms181581.aspx)
- [ATAN](https://msdn.microsoft.com/library/ms181746.aspx)
- [ATN2](https://msdn.microsoft.com/library/ms173854.aspx)
- [BINARY_CHECKSUM](https://msdn.microsoft.com/library/ms173784.aspx)
- [KIRJAINKOKO](https://msdn.microsoft.com/library/ms181765.aspx)
- [CAST ja Muunna](https://msdn.microsoft.com/library/ms187928.aspx)
- [PYÖRISTÄ.KERR.YLÖS](https://msdn.microsoft.com/library/ms189818.aspx)
- [MERKKI](https://msdn.microsoft.com/library/ms187323.aspx)
- [CHARINDEX-FUNKTIOON](https://msdn.microsoft.com/library/ms186323.aspx)
- [TARKISTUSSUMMA](https://msdn.microsoft.com/library/ms189788.aspx)
- [YHDISTETTYÄ KYSELYÄ](https://msdn.microsoft.com/library/ms190349.aspx)
- [COL_NAME](https://msdn.microsoft.com/library/ms174974.aspx)
- [COLLATIONPROPERTY](https://msdn.microsoft.com/library/ms190305.aspx)
- [KETJUTETTU](https://msdn.microsoft.com/library/hh231515.aspx)
- [COS](https://msdn.microsoft.com/library/ms188919.aspx)
- [COT](https://msdn.microsoft.com/library/ms188921.aspx)
- [LASKE](https://msdn.microsoft.com/library/ms175997.aspx)
- [COUNT_BIG](https://msdn.microsoft.com/library/ms190317.aspx)
- [CUME_DIST](https://msdn.microsoft.com/library/hh231078.aspx)
- [CURRENT_TIMESTAMP](https://msdn.microsoft.com/library/ms188751.aspx)
- [NYKYINEN_KÄYTTÄJÄ](https://msdn.microsoft.com/library/ms176050.aspx)
- [DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx)
- [DATALENGTH](https://msdn.microsoft.com/library/ms173486.aspx)
- [DATEADD](https://msdn.microsoft.com/library/ms186819.aspx)
- [DATEDIFF](https://msdn.microsoft.com/library/ms189794.aspx)
- [DATEFROMPARTS](https://msdn.microsoft.com/library/hh213228.aspx)
- [DATENAME](https://msdn.microsoft.com/library/ms174395.aspx)
- [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx)
- [DATETIME2FROMPARTS](https://msdn.microsoft.com/library/hh213312.aspx)
- [DATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213233.aspx)
- [DATETIMEOFFSETFROMPARTS](https://msdn.microsoft.com/library/hh231077.aspx)
- [PÄIVÄ](https://msdn.microsoft.com/library/ms176052.aspx)
- [DB_ID](https://msdn.microsoft.com/library/ms186274.aspx)
- [DB_NAME](https://msdn.microsoft.com/library/ms189753.aspx)
- [ASTETTA](https://msdn.microsoft.com/library/ms178566.aspx)
- [DENSE_RANK](https://msdn.microsoft.com/library/ms173825.aspx)
- [ERO](https://msdn.microsoft.com/library/ms188753.aspx)
- [KUUKAUSI.LOPPU](https://msdn.microsoft.com/library/hh213020.aspx)
- [ERROR_MESSAGE](https://msdn.microsoft.com/library/ms190358.aspx)
- [ERROR_NUMBER](https://msdn.microsoft.com/library/ms175069.aspx)
- [ERROR_PROCEDURE](https://msdn.microsoft.com/library/ms188398.aspx)
- [ERROR_SEVERITY](https://msdn.microsoft.com/library/ms178567.aspx)
- [ERROR_STATE](https://msdn.microsoft.com/library/ms180031.aspx)
- [EKSPONENTTI](https://msdn.microsoft.com/library/ms179857.aspx)
- [FIRST_VALUE](https://msdn.microsoft.com/library/hh213018.aspx)
- [KERROKSEN](https://msdn.microsoft.com/library/ms178531.aspx)
- [GETDATE](https://msdn.microsoft.com/library/ms188383.aspx)
- [GETUTCDATE](https://msdn.microsoft.com/library/ms178635.aspx)
- [HAS_DBACCESS](https://msdn.microsoft.com/library/ms187718.aspx)
- [HASHBYTES](https://msdn.microsoft.com/library/ms174415.aspx)
- [INDEXPROPERTY](https://msdn.microsoft.com/library/ms187729.aspx)
- [ISDATE](https://msdn.microsoft.com/library/ms187347.aspx)
- [ISNULL](https://msdn.microsoft.com/library/ms184325.aspx)
- [ISNUMERIC](https://msdn.microsoft.com/library/ms186272.aspx)
- [VIIVE](https://msdn.microsoft.com/library/hh231256.aspx)
- [LAST_VALUE](https://msdn.microsoft.com/library/hh231517.aspx)
- [LIIDI](https://msdn.microsoft.com/library/hh213125.aspx)
- [VASEMMALLE](https://msdn.microsoft.com/library/ms177601.aspx)
- [PITUUS](https://msdn.microsoft.com/library/ms190329.aspx)
- [LOKITIEDOSTON](https://msdn.microsoft.com/library/ms190319.aspx)
- [LOG10](https://msdn.microsoft.com/library/ms175121.aspx)
- [PIENEMPI](https://msdn.microsoft.com/library/ms174400.aspx)
- [LTRIM](https://msdn.microsoft.com/library/ms177827.aspx)
- [ENIMMÄISMÄÄRÄ](https://msdn.microsoft.com/library/ms187751.aspx)
- [MIN](https://msdn.microsoft.com/library/ms179916.aspx)
- [KUUKAUSI](https://msdn.microsoft.com/library/ms187813.aspx)
- [NCHAR](https://msdn.microsoft.com/library/ms182673.aspx)
- [NTILE](https://msdn.microsoft.com/library/ms175126.aspx)
- [NULLIF](https://msdn.microsoft.com/library/ms177562.aspx)
- [OBJECT_ID](https://msdn.microsoft.com/library/ms190328.aspx)
- [OBJEKTIN_NIMI](https://msdn.microsoft.com/library/ms186301.aspx)
- [OBJECTPROPERTY](https://msdn.microsoft.com/library/ms176105.aspx)
- [OIBJECTPROPERTYEX](https://msdn.microsoft.com/library/ms188390.aspx)
- [ODBCS skalaarifunktiot](https://msdn.microsoft.com/library/bb630290.aspx)
- [Lause PÄÄLLE](https://msdn.microsoft.com/library/ms189461.aspx)
- [PARSENAME](https://msdn.microsoft.com/library/ms188006.aspx)
- [PATINDEX](https://msdn.microsoft.com/library/ms188395.aspx)
- [PERCENTILE_CONT](https://msdn.microsoft.com/library/hh231473.aspx)
- [PERCENTILE_DISC](https://msdn.microsoft.com/library/hh231327.aspx)
- [PERCENT_RANK](https://msdn.microsoft.com/library/hh213573.aspx)
- [PII](https://msdn.microsoft.com/library/ms189512.aspx)
- [POWER](https://msdn.microsoft.com/library/ms174276.aspx)
- [QUOTENAME](https://msdn.microsoft.com/library/ms176114.aspx)
- [RADIAANIT](https://msdn.microsoft.com/library/ms189742.aspx)
- [SATUNNAISLUKU](https://msdn.microsoft.com/library/ms177610.aspx)
- [JÄRJESTYS](https://msdn.microsoft.com/library/ms176102.aspx)
- [KORVAA](https://msdn.microsoft.com/library/ms186862.aspx)
- [TOISTAA](https://msdn.microsoft.com/library/ms174383.aspx)
- [PERUUTA](https://msdn.microsoft.com/library/ms180040.aspx)
- [OIKEALLE](https://msdn.microsoft.com/library/ms177532.aspx)
- [LUVUN PYÖRISTÄMINEN](https://msdn.microsoft.com/library/ms175003.aspx)
- [SOLUVIITTAUKSESSA](https://msdn.microsoft.com/library/ms186734.aspx)
- [RTRIM](https://msdn.microsoft.com/library/ms178660.aspx)
- [SCHEMA_ID](https://msdn.microsoft.com/library/ms188797.aspx)
- [SCHEMA_NAME](https://msdn.microsoft.com/library/ms175068.aspx)
- [SERVERPROPERTY](https://msdn.microsoft.com/library/ms174396.aspx)
- [SESSION_USER](https://msdn.microsoft.com/library/ms177587.aspx)
- [KIRJAUDU](https://msdn.microsoft.com/library/ms188420.aspx)
- [SIN](https://msdn.microsoft.com/library/ms188377.aspx)
- [SMALLDATETIMEFROMPARTS](https://msdn.microsoft.com/library/hh213396.aspx)
- [SOUNDEX](https://msdn.microsoft.com/library/ms187384.aspx)
- [TILAA](https://msdn.microsoft.com/library/ms187950.aspx)
- [SQL_VARIANT_PROPERTY](https://msdn.microsoft.com/library/ms178550.aspx)
- [NELIÖJUURI](https://msdn.microsoft.com/library/ms176108.aspx)
- [NELIÖ](https://msdn.microsoft.com/library/ms173569.aspx)
- [STATS_DATE](https://msdn.microsoft.com/library/ms190330.aspx)
- [KESKIHAJONTA-FUNKTIO](https://msdn.microsoft.com/library/ms190474.aspx)
- [KESKIHAJONTAP-FUNKTIO](https://msdn.microsoft.com/library/ms176080.aspx)
- [STR](https://msdn.microsoft.com/library/ms189527.aspx)
- [KOHTEIDEN](https://msdn.microsoft.com/library/ms188043.aspx)
- [ALIMERKKIJONO](https://msdn.microsoft.com/library/ms187748.aspx)
- [SUMMA](https://msdn.microsoft.com/library/ms187810.aspx)
- [SUSER_SNAME](https://msdn.microsoft.com/library/ms174427.aspx)
- [SWITCHOFFSET](https://msdn.microsoft.com/library/bb677244.aspx)
- [SYSDATETIME](https://msdn.microsoft.com/library/bb630353.aspx)
- [SYSDATETIMEOFFSET](https://msdn.microsoft.com/library/bb677334.aspx)
- [SYSTEM_USER](https://msdn.microsoft.com/library/ms179930.aspx)
- [SYSUTCDATETIME](https://msdn.microsoft.com/library/bb630387.aspx)
- [TAN](https://msdn.microsoft.com/library/ms190338.aspx)
- [TERTIARY_WEIGHTS](https://msdn.microsoft.com/library/ms186881.aspx)
- [TIMEFROMPARTS](https://msdn.microsoft.com/library/hh213398.aspx)
- [TODATETIMEOFFSET](https://msdn.microsoft.com/library/bb630335.aspx)
- [TYPE_ID](https://msdn.microsoft.com/library/ms181628.aspx)
- [TYPE_NAME](https://msdn.microsoft.com/library/ms189750.aspx)
- [TYPEPROPERTY](https://msdn.microsoft.com/library/ms188419.aspx)
- [UNICODE](https://msdn.microsoft.com/library/ms180059.aspx)
- [ISOT](https://msdn.microsoft.com/library/ms180055.aspx)
- [KÄYTTÄJÄN](https://msdn.microsoft.com/library/ms186738.aspx)
- [KÄYTTÄJÄTUNNUS](https://msdn.microsoft.com/library/ms188014.aspx)
- [VAR](https://msdn.microsoft.com/library/ms186290.aspx)
- [VARP](https://msdn.microsoft.com/library/ms188735.aspx)
- [VUODEN](https://msdn.microsoft.com/library/ms186313.aspx)
- [XACT_STATE](https://msdn.microsoft.com/library/ms189797.aspx)

## <a name="transactions"></a>Tapahtumat

- [tapahtumat](https://msdn.microsoft.com/library/mt204031.aspx)

## <a name="diagnostic-sessions"></a>Diagnostiikan istunnot

- [DIAGNOSTIIKAN ISTUNNON LUOMINEN](https://msdn.microsoft.com/library/mt204029.aspx)

## <a name="procedures"></a>Ohjeita

- [sp_addrolemember](https://msdn.microsoft.com/library/ms187750.aspx)
- [sp_columns](https://msdn.microsoft.com/library/ms176077.aspx)
- [sp_configure](https://msdn.microsoft.com/library/ms188787.aspx)
- [sp_datatype_info_90](https://msdn.microsoft.com/library/mt204014.aspx)
- [sp_droprolemember](https://msdn.microsoft.com/library/ms188369.aspx)
- [sp_execute](https://msdn.microsoft.com/library/ff848746.aspx)
- [sp_executesql](https://msdn.microsoft.com/library/ms188001.aspx)
- [sp_fkeys](https://msdn.microsoft.com/library/ms175090.aspx)
- [sp_pdw_add_network_credentials](https://msdn.microsoft.com/library/mt204011.aspx)
- [sp_pdw_database_encryption](https://msdn.microsoft.com/library/mt219360.aspx)
- [sp_pdw_database_encryption_regenerate_system_keys](https://msdn.microsoft.com/library/mt204033.aspx)
- [sp_pdw_log_user_data_masking](https://msdn.microsoft.com/library/mt204023.aspx)
- [sp_pdw_remove_network_credentials](https://msdn.microsoft.com/library/mt204038.aspx)
- [sp_pkeys](https://msdn.microsoft.com/library/ms189813.aspx)
- [sp_prepare](https://msdn.microsoft.com/library/ff848808.aspx)
- [sp_spaceused](https://msdn.microsoft.com/library/ms188776.aspx)
- [sp_special_columns_100](https://msdn.microsoft.com/library/mt204025.aspx)
- [sp_sproc_columns](https://msdn.microsoft.com/library/ms182705.aspx)
- [sp_statistics](https://msdn.microsoft.com/library/ms173842.aspx)
- [sp_tables](https://msdn.microsoft.com/library/ms186250.aspx)
- [sp_unprepare](https://msdn.microsoft.com/library/ff848735.aspx)



## <a name="set-statements"></a>Määritä lauseet

- [MÄÄRITÄ ANSI_DEFAULTS](https://msdn.microsoft.com/library/ms188340.aspx)
- [MÄÄRITÄ ANSI_NULL_DFLT_OFF](https://msdn.microsoft.com/library/ms187356.aspx)
- [MÄÄRITÄ ANSI_NULL_DFLT_ON](https://msdn.microsoft.com/library/ms187375.aspx)
- [MÄÄRITÄ EI OLE KÄYTÖSSÄ](https://msdn.microsoft.com/library/ms188048.aspx)
- [MÄÄRITÄ ANSI_PADDING](https://msdn.microsoft.com/library/ms187403.aspx)
- [MÄÄRITÄ ANSI_WARNINGS](https://msdn.microsoft.com/library/ms190368.aspx)
- [MÄÄRITÄ ARITHABORT](https://msdn.microsoft.com/library/ms190306.aspx)
- [MÄÄRITÄ ARITHIGNORE](https://msdn.microsoft.com/library/ms184341.aspx)
- [MÄÄRITÄ CONCAT_NULL_YIELDS_NULL](https://msdn.microsoft.com/library/ms176056.aspx)
- [MÄÄRITÄ DATEFIRST](https://msdn.microsoft.com/library/ms181598.aspx)
- [MÄÄRITÄ PÄIVÄMÄÄRÄMUOTO](https://msdn.microsoft.com/library/ms189491.aspx)
- [MÄÄRITÄ FMTONLY](https://msdn.microsoft.com/library/ms173839.aspx)
- [MÄÄRITÄ IMPLICIT_TRANSACITONS](https://msdn.microsoft.com/library/ms187807.aspx)
- [MÄÄRITÄ LOCK_TIMEOUT](https://msdn.microsoft.com/library/ms189470.aspx)
- [MÄÄRITÄ NUMBERIC_ROUNDABORT](https://msdn.microsoft.com/library/ms188791.aspx)
- [MÄÄRITÄ QUOTED_IDENTIFIER](https://msdn.microsoft.com/library/ms174393.aspx)
- [MÄÄRITÄ RIVIMÄÄRÄ](https://msdn.microsoft.com/library/ms188774.aspx)
- [MÄÄRITÄ TEXTSIZE](https://msdn.microsoft.com/library/ms186238.aspx)
- [MÄÄRITÄ TAPAHTUMAN ERISTYSTASON](https://msdn.microsoft.com/library/ms173763.aspx)
- [MÄÄRITÄ XACT_ABORT](https://msdn.microsoft.com/library/ms188792.aspx)


## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja viittaus on artikkelissa [SQL-tietovarasto viittaus yleiskatsaus][].

<!--Image references-->

<!--Article references-->
[SQL-tietovarasto viittaus yleiskatsaus]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->
