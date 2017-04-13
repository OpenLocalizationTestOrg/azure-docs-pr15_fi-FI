<properties
   pageTitle="Vikasietoisuudesta tekniset ohjeet tietovirheitä tai vahingossa hakemista varten | Microsoft Azure"
   description="Artikkeli ymmärtämiseen estämisestä tietovirheitä, ja tietojen tai vahingossa tietojen poisto ja joustavat, erittäin käytettävissä vika varmatoimisempia sovellusten suunnitteleminen suunnittelet palauttaminen"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Azure vikasietoisuudelle tekniset ohjeet: tietovirheitä tai vahingossa palauttaminen

Tehokkaat liiketoimintasuunnitelma jatkuvuuden osa päivityksessä suunnitelma, jos tiedot saa vioittunut tai vahingossa. Seuraavassa on tietoja palauttaminen, kun tiedot on vahingoittunut tai tahaton poistaminen sovellusvirheet tai operaattori virheen vuoksi.

##<a name="virtual-machines"></a>Näennäiskoneiden

Voit suojata oman Azuren näennäiskoneiden (kutsutaan joskus infrastruktuurin nimellä--palvelun VMs) sovellusvirheet tai vahingossa käyttämällä [Azure varmuuskopion](https://azure.microsoft.com/services/backup/). Azure varmuuskopiointi voi luoda, jotka ovat yhdenmukaisia useita AM levyjä varmuuskopiot. Lisäksi varmuuskopiointi säilö voi replikoida eri alueilla antamaan palauttamisesta aluetta menetyksen.

##<a name="storage"></a>Tallennustilan

Huomaa, että Azuren tallennustilaan tarjoaa tietojen vikasietoisuudelle automaattinen replikoiden kautta, tämä ei estä yhteyttä sovelluksen koodin (tai kehittäjät/käyttäjät) teeskentelee tiedot – tahattoman tai tahaton poistaminen ja Päivitä. Tietojen menettämisen tapahtuessa sovelluksen tai käyttäjän virhe tarkkuuden ylläpito vaatii monimutkaisemman tekniikoita, kuten tietojen kopioimista toissijainen tallennuspaikkaan valvontalokin kanssa. Kehittäjät voivat hyödyntää Blob-objektien [tilannevedoksen ominaisuus](https://msdn.microsoft.com/library/azure/ee691971.aspx), jossa voit luoda vain luku-ajankohta tilannevedoksia blob sisällön. Tämä voidaan tietojen tarkkuuden ratkaista perustana Azuren tallennustilaan BLOB-objektit.

###<a name="blob-and-table-storage-backup"></a>Blob-objektien ja taulukon tallennustilan varmuuskopiointi

Vaikka BLOB-objektit ja taulukot ovat erittäin kestävät, ne edustavat aina tiedot nykyisen tilan. Ei-toivottuja muokkaaminen tai poistaminen tietojen palauttamisesta voi vaatia tietojen palauttaminen edelliseen tilaan. Tämä onnistuu tehokkaasta hyödyntää myöntämä Azure tallentamiseen ja säilyttää ajankohta kopiot-ominaisuuksia.

Azure-BLOB-objektit voit suorittaa ajankohta varmuuskopiot [blob tilannevedoksen ominaisuus](https://msdn.microsoft.com/library/ee691971.aspx). Kunkin tilannevedoksen, perittävän vain sisällä blob erot tallentamiseen viimeinen tilannevedoksen tilan sen ollessa tallentamista varten. Tilannevedosten ovat riippumatta siitä, onko ne perustuvat, alkuperäinen Blob-objektien, joten kopiointi toiseen Blob-objektien tai jopa toisen tallennustilan tili on suositeltavaa. Näin varmistat, että varmuuskopiotiedot suojataan oikein vahingossa. Azure-taulukoiden voit määrittää ajankohta kopiot toiseen taulukkoon tai Azure-BLOB-objektit. Yksityiskohtaisia ohjeita ja esimerkkejä suorittamisesta sovellustason varmuuskopioiden taulukoiden ja BLOB löytyvät tähän:

  * [Taulukoiden vastaan sovellusvirheet suojaaminen](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Oman BLOB vastaan sovellusvirheet suojaaminen](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Tietokannan

Azure SQL-tietokanta on useita [Liiketoiminnan jatkuvuus](../sql-database/sql-database-business-continuity.md) (varmuuskopion, Palauta)-asetukset. Tietokantojen voidaan kopioida [Tietokannan kopio](../sql-database/sql-database-copy.md) -toiminnon avulla tai [vieminen](../sql-database/sql-database-export.md) ja [tuominen](https://msdn.microsoft.com/library/hh710052.aspx) SQL Server bacpac tiedoston. Tietokannan kopiota on muulla tavoin yhtenäiset tulokset, mutta ei bacpac (Tuo/Vie-palvelun) kautta. Molempien asetusten suoritetaan jonon-pohjaisten palvelujen tietokeskuksen sisällä, ja niissä ei ole tällä hetkellä aika valmistuminen SLA.

>[AZURE.NOTE]Tietokannan kopio ja tuo/vie asetukset sijoittaa merkittäviä aste kuormituksen lähdetietokannan. Ne voivat aiheuttaa resurssiristiriita tai rajoittaminen tapahtumat.

###<a name="sql-database-backup"></a>SQL-tietokannan varmuuskopiointi

Microsoft Azure SQL-tietokanta, ajankohta varmuuskopiot saavutetaan kopioimalla [Azure SQL-tietokantaan](../sql-database/sql-database-copy.md). Tällä komennolla saman loogisen tietokantapalvelimen tai eri palvelimeen muulla tavoin yhdenmukaista kopion tietokannan luomiseen. Kummassakin tapauksessa tietokannan kopion on täysin ja lähdetietokannan riippumatta. Voit luoda kopioita edustaa ajankohta palautusasetus. Voit palauttaa tietokannan tilan kokonaan nimeämällä ne uuden tietokannan lähde-tietokannan nimi. Vaihtoehtoisesti voit palauttaa tietyn osan tiedoista uusi tietokannasta käyttämällä Transact-SQL-kyselyjä. Lisätietoja SQL-tietokannan artikkelissa [yleistä liiketoiminnan jatkuvuus Azure SQL-tietokantaan](../sql-database/sql-database-business-continuity.md).

###<a name="sql-server-on-virtual-machines-backup"></a>SQL Server-näennäiskoneiden varmuuskopiointi

SQL Server käyttää Azure infrastruktuuriin service-näennäiskoneiden (jota kutsutaan usein IaaS tai IaaS VMs), on kaksi vaihtoehtoa: perinteinen varmuuskopiot ja log toimitus. Perinteinen varmuuskopioiden avulla voit palauttaa tiettyyn kohtaan samanaikaisesti, mutta palauttaminen on hidasta. Perinteinen varmuuskopioista palauttaminen edellyttää alkavat ensimmäisen täyden varmuuskopion ja käyttämällä sitten sen jälkeen tehdyt varmuuskopiot. Toinen vaihtoehto on määritettävä lokia toimitus istunnon luetelmakohtien Lokin varmuuskopioiden palauttaminen (esimerkiksi mukaan kaksi tuntia). Tämä on ikkunan palauttamaan ensisijainen koskevat virheet.

##<a name="other-azure-platform-services"></a>Muut Azure platform-palvelut

Azure ympäristö joidenkin palvelujen tallentavat tietoja käyttäjän hallita tallennustilan asiakas- tai Azure SQL-tietokantaan. Jos asiakas- tai resurssi on poistettu tai vioittunut, tämä saattaa aiheuttaa vakavia virheitä-palvelussa. Tällöin on tärkeää varmuuskopiot, joiden avulla voit luoda nämä resurssit uudelleen, jos ne on poistettu tai vioittunut säilyttää.

Azure-sivustoista ja Azure Mobile-palveluihin voit varmuuskopioida ja säilyttää liittyvät tietokannat. Azure Media-palvelu ja näennäiskoneiden on säilytettävä liittyvän Azure-tallennustilan tilin ja kaikki kyseisen tilin resurssit. Esimerkiksi näennäiskoneiden, voit on varmuuskopioiminen ja Azure-blob-säiliö AM-levyjen hallinta.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Tietovirheitä tai vahingossa Tarkistusluettelot

##<a name="virtual-machines-checklist"></a>Näennäiskoneiden tarkistusluettelo

  1. Tutustu tämän artikkelin näennäiskoneiden-osio.
  2. Varmuuskopiointi ja ylläpitää AM levyjen Azure Backup (tai oman varmuuskopion järjestelmän Azure-blob-säiliö ja Näennäiskiintolevyn tilannevedoksia).

##<a name="storage-checklist"></a>Tallennustilan tarkistusluettelo

  1. Tutustu tämän artikkelin tallennustilan-osio.
  2. Varmuuskopioi tärkeät tallennustilan resurssien säännöllisesti.
  3. Kannattaa käyttää BLOB tilannevedos-ominaisuus.

##<a name="database-checklist"></a>Tietokannan tarkistusluettelo

  1. Tutustu tämän artikkelin tietokanta-osio.
  2. Luo ajankohta varmuuskopiot tietokannan kopioi-komentoa käyttämällä.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>SQL Server-näennäiskoneiden varmuuskopiointi tarkistusluettelo

  1. Tarkista SQL Serverin tämän asiakirjan näennäiskoneiden varmuuskopiointi-osassa.
  2. Perinteinen varmuuskopiointi ja palauttaminen avulla.
  3. Viivästyneet lokin toimitus istunnon luominen

##<a name="web-apps-checklist"></a>Web Apps-tarkistusluettelo

  1. Varmuuskopiointi ja ylläpitää liittyvän tietokannan, jos sellainen on.

##<a name="media-services-checklist"></a>Media-palveluiden tarkistusluettelo

  1. Varmuuskopiointi ja ylläpitää liittyvät tallennustilan resurssit.

##<a name="more-information"></a>Lisätietoja

Katso lisätietoja Varmuuskopiointi ja palauttaminen toiminnot Azure- [tallennustilan, varmuuskopiointi ja palauttaminen](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on kohdistettu [Azure vikasietoisuudesta tekniset ohjeet](./resiliency-technical-guidance.md)sarjaan kuuluvan. Jos etsit vikasietoisuudelle, palauttaminen tai suuren käytettävyyden resurssit-kohdassa Azure vikasietoisuudelle tekniset ohjeet [lisäresursseja](./resiliency-technical-guidance.md#additional-resources).