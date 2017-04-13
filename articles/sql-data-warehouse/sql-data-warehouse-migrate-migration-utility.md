<properties
   pageTitle="Siirtää: Tietojen varasto siirron apuohjelman | Microsoft Azure"
   description="Siirtää SQL-tietovarasto."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="data-warehouse-migration-utility-preview"></a>Data Warehouse siirron apuohjelman (ennakkoversio)

> [AZURE.SELECTOR]
- [Lataa siirtymisapuohjelma][]

Tietoja varaston siirto-apuohjelma on rakenteen ja tietojen siirtäminen SQL Server- ja Azure SQL-tietokanta Azure SQL-tietovarasto työkalu. Rakenteen siirron aikana työkalu yhdistää automaattisesti vastaavan mallin lähteestä kohteeseen. Kun rakenne on siirretty, työkalujen on asetus, jos haluat siirtää tietoja automaattisesti komentosarjojen kanssa.

Lisäksi rakenteen ja tiedot siirron tämä työkalu kysytään, joka esittää välisiä kohde- ja lähdekentät esiintymät, joka estää virtaviivaistettu siirron yhteensopivuus-raporttien luomiseen.

## <a name="get-started"></a>Käytön aloittaminen
Asennuksen edellytyksenä on BCP komentorivin apuohjelma suorittaa siirron komentosarjat ja Officen, voit tarkastella yhteensopivuusraportti. Jälkeen käynnistettäessä ohjelmatiedosto, joka on ladattu, voit pyydetään hyväksymään vakio Käyttöoikeussopimuksen ennen työkalun asennetaan.

Lisäksi voit suorittaa siirron Utiliy, tarvitset sitä seuraavan käyttöoikeudet, jotka haluat siirtää tietokannan: Luo tietokanta, muuta tahansa TIETOKANTAA tai minkä tahansa NÄKYMÄMÄÄRITYS.

### <a name="launching-the-tool-and-connecting"></a>Työkalu avaamista ja yhdistäminen
Käynnistä työkalu valitsemalla työpöydän kuvakkeesta, joka näkyy viestin Asenna. Yhteydessä avattaessa työkalu, sinua pyydetään yhteydenotto-sivu, jossa voit valita lähde- ja siirtotyökalun kohdetta. Tällä hetkellä tuetaan SQL Server- ja Azure SQL-tietokanta nimellä lähteiden ja SQL-tietovarasto kohteeksi. Kun olet valinnut tämän, sinua pyydetään muodostaa yhteyden lähdepalvelimeen täyttämällä palvelimen nimi ja todennustapa ja valitsemalla sitten "Yhdistä".

Työkalu näyttää jälkeen todennustapa-tietokantoja, jotka ovat Serveriin, jota olet muodostanut yhteyden luettelo. Voit aloittaa siirron valitsemalla tietokanta, johon haluat siirtää ja valitsemalla sitten 'Artikkelista valittuna'.

## <a name="migration-report"></a>Siirtoraportin
Valitsemalla "Tarkista tietokannan yhteensopivuus-työkalu luo raportti, yhteenvetojen tietokannan kaikki objektin yhteensopivuusongelmia siirtämään pyydetty. Joitakin SQL Server-toiminto, joka ei ole SQL-tietovarasto laajempi luettelo löytyy Microsoftin [siirron ohjeissa][]. Kun raportti on luotu osaat Tallenna ja Avaa raportti Excelissä.

Huomioi, että siirto-rakenteen määritellyt 'Object' muutetaan heti siirron kyseisistä tiedoista, jotta Useimmat ongelmat luotaessa. Tarkista muutokset, jotta et halua tehdä lisää muutoksia ennen käyttöä rakenne.

## <a name="migrate-schema"></a>Siirtää rakenne

Kun yhteys on muodostettu, siirtää rakenteen valitseminen luo rakenteen siirron komentosarjan valitut taulukot. Komentosarja-portit taulukon kartat ei ole yhteensopiva tietojen rakennetta kirjoituskentän yhteensopivuus lomakkeisiin ja luo suojausvaltuudet ja rakenteen, jos tämä ilmaistaan käyttäjän siirto-asetukset. Koodi voidaan suorittaa kohdennettujen SQL-tietovarasto esiintymän vastaan, tallennetaan tiedostoon, kopioida Leikepöydän tai muokata myös sitoutuvat ennen siihen jatkotoimia.  

Mainita yläpuolella, siirretään rakenteen tarkistaminen siirron muuttuessa, työkalu on tehnyt varmistamiseksi, että olet ymmärtänyt ne.  

## <a name="migrate-data"></a>Tietojen siirtäminen

Valitsemalla Siirrä tiedot-asetus, voit luoda BCP komentosarjoja, jotka siirtyvät tietojen ensin litteään tiedostoja palvelimeen, ja valitse sitten suoraan SQL-tietovarasto. Suosittelemme tämän prosessin siirtymisen jonkin verran tietojen ja uudelleenyritykset eivät ole valmiita ja virheitä saattaa ilmetä, jos on verkkoyhteys menetyksiä. Jotta voit suorittaa tämän on pystyttävä muodostamaan asennettu BCP komentorivin apuohjelma ja tiedot rakenteen jo on luotu.

Kun olet täyttänyt edellä parametrit rajoittamattoman valitsemalla Suorita siirron ja kaksi pakettien joukko luodaan määritettyyn sijaintiin. Suorita vientitiedosto jotta tietojen tuominen tasainen tiedostojen siirron lähde ja jotta tietojen tuominen SQL-tietovarasto tuontitiedoston.

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet siirtänyt joitakin tietoja, valitse, miten voit [kehittää][].

<!--Image references-->

<!--Article references-->
[siirron dokumentaatio]: sql-data-warehouse-overview-migrate.md
[kehittäminen]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Lataa siirtymisapuohjelma]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
