<properties
   pageTitle="Yleinen SQL-yhdistin | Microsoft Azure"
   description="Tässä artikkelissa käsitellään määrittäminen Microsoftin yleinen SQL-yhdistin."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-technical-reference"></a>Yleinen SQL Connector tekniset tiedot

Tässä artikkelissa on yleisiä SQL-yhdistin. Artikkeli koskee seuraavia tuotteita:

- Microsoftin Käyttäjätietojen hallinta 2016 (MIM2016)
- Forefront käyttäjätietojen hallinta 2010 R2 (FIM2010R2)
    -   Käytä korjaustiedoston 4.1.3671.0 tai uudempi [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ja FIM2010R2 yhdistin on ladattavissa [Microsoft Download Centeristä](http://go.microsoft.com/fwlink/?LinkId=717495).

Tämä toiminto yhdistin näkyviin [Yleinen SQL-yhdistin vaiheittaiset ohjeet](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) on artikkelissa.

## <a name="overview-of-the-generic-sql-connector"></a>Yleistä yleinen SQL-yhdistin

Yleinen SQL-yhdistin avulla voit integroida synkronointipalvelua tietokannan järjestelmä, joka tarjoaa ODBC-yhteydet.  

Ylätason näkökulmasta yhdistimen nykyinen versio tukee seuraavia ominaisuuksia:

Toiminto | Tuki
--- | ---
Yhdistetty tietolähde | Yhdistin tue kaikkia 64-bittinen ODBC-ohjaimet. Se on testattu kanssa seuraavasti: <li>Microsoft SQL Server- ja SQL Azure-tietokannassa</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 ja 11 g</li><li>MySQL 5.x</li>
Skenaariot   | <li>Objektin elinkaari hallinta</li><li>Salasanojen hallinta</li>
Toiminnot | <li>Täysi tuonti ja Delta tuominen, vieminen</li><li>Vietäväksi: Lisää, poista, päivitys- ja korvaaminen</li><li>Aseta salasana, salasanan vaihtaminen</li>
Rakenne | <li>Dynaaminen etsiminen kohteet ja määritteet</li>

### <a name="prerequisites"></a>Edellytykset
Ennen kuin käytät yhdistimen, varmista, että sinulla on synkronointi-palvelimeen seuraavasti:

- Microsoft .NET 4.5.2 Framework tai uudempi versio
- 64-bittinen ODBC-asiakasohjelman ohjaimet

### <a name="permissions-in-connected-data-source"></a>Yhdistetyn tietolähteen käyttöoikeudet
Voit luoda tai suorittaa tuetut tehtäviä yleinen SQL-yhdistin, sinulla on oltava:

- db_datareader
- db_datawriter

### <a name="ports-and-protocols"></a>Portit ja protokollat
ODBC-ohjain toimimaan vaaditaan porteille tietokannan toimittajan käyttöohjeista.

## <a name="create-a-new-connector"></a>Luo uusi yhdistin
Luo yleinen SQL-yhdistin **Synkronointipalvelu** Valitse **Hallinta-agentti** ja **Luo**. Valitse yhdistin **Yleinen SQL (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Yhteys
Yhdistimen käyttää connectivity ODBC DSN-tiedostoon. Luo käyttämällä **ODBC-tietolähteiden** löydy aloitusvalikkoon **Valvontatyökalut**-kohdassa DSN-tiedosto. Hallintatyökalun Luo **Tiedosto DSN** , jotta se voidaan suorittaa yhdistin.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Yhteyksien näytön on ensimmäinen, kun luot uuden yleisen SQL-yhdistin. Sinun on annettava seuraavat tiedot:

- DSN-tiedostopolku
- Todennus
    - Käyttäjänimi
    - Salasana

Tietokannan pitäisi tue todennusta näistä menetelmistä:

- **Windows-todennuksen**: todentamalla tietokanta käyttää Windows-tunnistetiedot vahvistamiseksi käyttäjä. Käyttäjänimi tai salasana määritetty käytetään tietokannan todentamismenetelmä. Tällä tilillä on oltava tietokannan käyttöoikeudet.
- **SQL-todennusta**: todentamalla käyttäjänimi ja salasana on määritetty jokin Connectivity näytön tietokantayhteyden muodostamisessa tietokannan käyttää. Jos tallennat tiedoston DSN käyttäjän nimi ja salasana, yhteys näytössä tunnistetietoja on ensisijainen.
- **Azure SQL-tietokanta-todennus**: Lisätietoja on artikkelissa [etäyhteyden muodostaminen SQL tietokanta mukaan käyttämällä Azure Active Directory-todennusta](..\sql-database\sql-database-aad-authentication.md).

**DN on ankkurin**: Jos valitset tämän vaihtoehdon, DN käytetään myös ankkuri-määrite. Yksinkertainen käyttöönotto voi käyttää, mutta niitä on myös seuraavat rajoitukset:

-   Yhdistimen tukee vain yhden objektilaji. Tämän vuoksi viittaus haluamasi määritteet vain voi viitata saman objektin laji.

**Vie tyyppi: objektin korvaa**: vietäessä, kun jotkin määritteet ovat muuttuneet, koko objekti, jossa on kaikki määritteet viedään ja korvaa aiemmin luotu objekti.

### <a name="schema-1-detect-object-types"></a>Rakenteen 1 (tunnista objektityypit)
Tällä sivulla aiot määrittää, miten tietokannan eri objektityypit etsiminen suorittaminen yhdistin.

Jokaisen objektityyppi esitetään osioksi ja määrittänyt tarkemmin **määrittäminen osiot ja hierarkioita**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Objektityyppi tunnistus menetelmää**: yhdistimen tukee menetelmien tunnistus objektin tyyppi.

- **Kiinteä arvo**: antaisit objektityypit luettelo CSV-luettelon kanssa. Esimerkki: `User,Group,Department`.  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Taulukon tai näkymän/toimintosarja**: taulukon tai näkymän/tallennettu toimintosarja nimi ja sarakkeen nimi, joka sisältää objektityypit luettelo. Jos käytät tallennetun toimintosarjan, valitse myös säätää parametrit muoto **[nimi]: [Suunta]: [arvo]**. Anna kullekin parametrille erillisenä rivinä (Käytä Ctrl + Enter, kun saat uuden rivin).  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **SQL-kyselyn**: tämän asetuksen avulla voit lähettää SQL-kysely, joka palauttaa yhden sarakkeen yhdistäminen objektin, kuten `SELECT [Column Name] FROM TABLENAME`. Palautetut sarakkeen on oltava merkkijonomuotoisen (varchar).

### <a name="schema-2-detect-attribute-types"></a>Rakenteen 2 (tunnista määritetyypit)
Tällä sivulla aiot määrittää, kuinka nimiä ja tyypit sujuvat tunnistetaan. Kokoonpanoasetusten määrittäminen on lueteltu jokaisen objektityyppi havaittu edelliselle sivulle.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Määritteen tunnistus menetelmää**: yhdistimen tukee seuraavista määritteen tyyppi tunnistus tavoista jokaisen olemassa objektityypin kanssa rakenteen 1-ikkunassa.

- **Taulukon tai näkymän/toimintosarja**: nimetä taulukon tai näkymän/tallennettu toimintosarja, jota käytetään Etsi määritteiden nimet. Jos käytät tallennetun toimintosarjan, valitse myös säätää parametrit muoto **[nimi]: [Suunta]: [arvo]**. Anna kullekin parametrille erillisenä rivinä (Käytä Ctrl + Enter, kun saat uuden rivin). Tunnistamiseen määritteiden nimet moniarvoinen määrite on CSV-taulukoiden tai näkymien. Moniarvoisen tilanteita, joissa ei tue kun pää- ja taulukko on sama sarakkeiden nimet.
- **SQL-kyselyn**: tämän asetuksen avulla voit lähettää SQL-kysely, joka palauttaa yhden sarakkeen kanssa nimiä, kuten `SELECT [Column Name] FROM TABLENAME`. Palautetut sarakkeen on oltava merkkijonomuotoisen (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Rakenteen 3 (Määritä ankkurin ja DN)
Tällä sivulla voit määrittää ankkurin ja kunkin olemassa objektityyppi DN-määrite. Voit valita useita määrite ja tee ankkurin yksilöllinen.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Moniarvoinen ja totuusarvo määritteet eivät näy.
- Sama määrite ei voi käyttää DN ja ankkurin, ellei Connectivity sivun **DN on ankkurin** on valittuna.
- Jos yhteys sivulla **DN on ankkurin** on valittuna, tämä sivu edellyttää DN-määrite. Tämän määritteen voidaan käyttää myös nimellä ankkuri-määrite.
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Rakenteen 4 (Määritä määritteen viittaus ja suunta)
Tällä sivulla voit määrittää määritteen tyyppi, kuten kokonaisluku, binaarinen, tai totuusarvo ja kunkin määritteen suuntaa. Kaikki määritteet-sivun **rakenteen 2** luetellaan moniarvoinen määritteitä.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **Tietotyyppi**: käyttää tällaisia sync engine lyhennettä määritteen. Oletusarvo on käyttää samaa tyyppiä havaita SQL-mallissa, mutta DateTime- ja viitefunktiot eivät ole helposti havaittavissa. Voit joutua määrittämään **DateTime** - tai **viittaus**.
- **Suunta**: Voit määrittää suunnan määritteen tuominen, vieminen tai ImportExport. ImportExport on oletusarvo.
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Huomautus:

- Jos määritetyyppi ei ole havaittavissa yhdistin, se käyttää merkkijono-tietotyyppi.
- **Sisäkkäiset taulukot** voidaan pitää yhden sarakkeen tietokannan taulukoihin. Oracle tallentaa sisäkkäisen taulukon rivit missään tietyssä järjestyksessä. Kun haet sisäkkäistä taulukkoa PL/SQL muuttujaksi, rivit on annettu peräkkäiset alaindeksit alkaa numerosta 1. Joka palauttaa matriisin kaltaisessa access yksittäisiä rivejä.
- Yhdistin ei tueta **VARRYS** .

### <a name="schema-5-define-partition-for-reference-attributes"></a>Rakenteen 5 (Määritä osio viittaus määritteiden)
Tällä sivulla määrittäminen viittaus attribuutit, mitkä (objektityyppi)-osion määrite viittaa.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Jos käytät **DN on kiinteä**, sinun on käytettävä saman objektin laji-viittaat yhden nimellä. Et voi viitata toiseen objektilaji.

### <a name="global-parameters"></a>Yleiset parametrit
Yleiset parametrit-sivua käytetään Delta Tuo, päivämäärä ja aika-muodossa ja salasanan menetelmän.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

Yleinen SQL-yhdistin tukee Delta tuontia seuraavilla tavoilla:

- **Käynnistimen**: Katso [käynnistimien Delta näkymien luomista](https://technet.microsoft.com/library/cc708665.aspx).
- **Vesileiman**: yleinen lähestymistapa, joita voidaan käyttää minkä tahansa muun tietokannan. Vesileima-kysely on valmiiksi tietokannan toimittajan perusteella. Vesileima-sarakkeessa on oltava jokaisen taulukon tai näkymän käytetään. Tässä sarakkeessa on seurattava Lisää ja kuin taulukot ja siihen liitetyt (moniarvoinen tai alikohde) taulukot. Kellonaikoja synkronointipalvelu ja tietokantapalvelimen välillä on synkronoitava. Muussa tapauksessa merkintöjä delta tuominen voi jättää pois.  
Rajoitukset:
    - Vesileiman strategia ei ole poistettu tukiobjekteja.
- **Tilannevedoksen**: (toimii vain Microsoft SQL Server) [luotaessa Delta näkymien tilannevedosten käyttäminen](https://technet.microsoft.com/library/cc720640.aspx)
- **Muutosten jäljityksen**: (toimii vain Microsoft SQL Server) [tietoja muutosten seuranta](https://msdn.microsoft.com/library/bb933875.aspx)  
Rajoitukset:
    - Ankkurin & DN-määritteen on oltava valitun objektin taulukon perusavaimen osa.
    - SQL-kysely ei tueta tuonti ja vienti ja muutosten jäljityksen aikana.

**Lisää parametrit**: Määritä tietokanta palvelimen aikavyöhykettä, joka ilmaisee, jossa tietokantapalvelimen sijaitsee. Tätä arvoa käytetään tukemaan eri muodoissa attribuuteista, päivämäärä ja aika.

Yhdistimen tallentaa aina päivämäärä- ja päivämäärä / aika-UTC-muodossa. Jos haluat muuntaa oikein päivämäärä kellonaika, tietokantapalvelin ja lukumuoto aikavyöhyke on määritettävä. Muoto on ilmaistava .net-muodossa.

Vietäessä jokaisen päivämäärä-aika-määrite on annettava yhdistimen UTC-aika-muodossa.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Salasanan määrittäminen**: yhdistimen tarjoaa salasanan synkronointi ja tukee määrittäminen ja muuta salasana.

Yhdistimen on tukemaan salasanojen synkronoinnin kahdella tavalla:

- **Tallennettu toimintosarja**: Tässä menetelmässä on kaksi tallennettujen toimintosarjojen tukemaan määrittäminen ja muuta salasana. Kirjoita Lisää kaikki parametrit ja muuta salasana-toiminto **Määrittää salasanan SP** ja **Muuta salasana SP** parametria vastaavasti kuin yhdessä seuraavassa esimerkissä.
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Tunniste salasana**: Tämä menetelmä vaatii salasanan tunniste DLL (sinun on antamaan tunniste DLL-nimi, joka on käyttöönoton [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) interface). Salasanan tunniste kokoonpanon on asetettava tunniste-kansiossa, niin, että yhdistimen voi ladata DLL suorituksen aikana.
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Sinun on otettava salasanojen hallinta **Tunniste määrittäminen** -sivulla myös.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Määritä osiot ja hierarkiat
Valitse kaikki objektityypit osiot ja hierarkiat-sivulla. Kunkin objektilaji on oma osio.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Voit ohittaa **yhteyden** tai **Yleiset parametrit** -sivulla määritettyjä arvoja.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Määritä ankkurit
Tämä sivu on vain luku-tilassa, koska ankkurin on jo määritetty. Valitun ankkuri-määrite on liitetään aina objektityyppi, se pysyy yksilöllinen objektin välillä.

![ankkurit](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Suorita vaihe parametrien määrittäminen
Nämä vaiheet on määritetty yhdistimeen Suorita-profiileista. Määritysten tehdä tietojen tuomisen ja viemisen todellinen työmäärä.

### <a name="full-and-delta-import"></a>Koko ja Delta-tuonti
Yleinen SQL Connector tuki kokonaan ja Delta tuonti käyttämällä seuraavista tavoista:

- Taulukko
- Näytä
- Tallennettu toimintosarja
- SQL-kysely

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Taulukon tai näkymän**  
Tuo objektin moniarvoinen määritteet on annettava **Multi-Valued nimi ja taulukkonäkymissä** CSV-taulukon tai näkymän nimi ja vastaaviin liitoksen ehdot **Liity ehto** ylemmän tason taulukkoon.

Esimerkki:, Jonka haluat tuoda työntekijän objekti ja kaikki sen moniarvoinen määritteet. On kaksi taulukkoa, työntekijän (ensisijainen taulukko) ja osaston (moniarvoinen).
Toimi seuraavasti:

- **Taulukon tai näkymän/SP**tyyppi **Työntekijä** .
- Kirjoita osaston **Multi-Valued nimi ja taulukkonäkymissä**.
- Kirjoita liitoksen ehto työntekijän & osaston välissä **Liity ehto**, esimerkiksi `Employee.DEPTID=Department.DepartmentID`.
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Tallennettujen toimintosarjojen**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Jos sinulla on paljon tietoja, on suositeltavaa toteuttamisesta sivutus oman tallennettujen toimintosarjojen kanssa.
- Tallennettu toimintosarja tukemaan sivutus sinun on Käynnistä indeksi-ja End indeksi. Lisätietoja: [sivutus tehokkaasti suuria tietomääriä kautta](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndexja @EndIndex korvataan suorittamisen milloin kyseiselle sivulle enimmäiskoko määrittänyt **Vaihe määrittäminen** -sivulla. Esimerkiksi kun yhdistimen hakee ensimmäisen sivun ja sivun kokoa on määritetty 500, kuten tilanteessa @StartIndex on 1 ja @EndIndex 500. Nämä arvot parantavien yhdistimen hakee seuraavien sivujen ja muuta @StartIndex & @EndIndex arvo.
- Suorita Parametroitu tallennettu toimintosarja toimittavat parametrit `[Name]:[Direction]:[Value]` muodossa. Kirjoita kunkin parametrin omalle rivilleen (Käytä Ctrl + Enter saadaksesi uuden rivin).
- Yleinen SQL-yhdistin tukee myös linkitettyjä palvelimia tuontitoiminto Microsoft SQL Server. Jos tiedot haetaan taulukosta linkitetty Serverissä, taulukon esitettävä muodossa:`[ServerName].[Database].[Schema].[TableName]`
- Yleinen SQL-yhdistin tukee vain objekteja, jotka on samanlainen rakenne (sekä alias nimen ja tietotyypin) välillä Suorita vaiheet tietojen ja rakenteen tunnistus. Jos valitun objektin rakenteen ja Suorita vaihe annetut tiedot on erilainen, SQL-yhdistin ei ei tue tämän tyyppistä skenaarioita.

**SQL-kysely**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Useita tulosjoukon kyselyjen ei tueta.
- SQL-kyselyn tukee sivujakoon ja anna Käynnistä tukemaan sivutus indeksi- ja End indeksi muuttujana.

### <a name="delta-import"></a>Sama.arvo tuonti
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta tuontimääritys edellyttää joitakin Lisää määritysten täysi tuominen verrattuna.

- Jos valitset käynnistimen tai tilannevedoksen hallintatavan delta muutosten jäljittäminen, anna historia-taulukkoa tai tilannevedoksen tietokannan **historia-taulukkoa tai tilannevedoksen tietokannan nimi** -ruutuun.
- Sinun on myös antaa liity ehto historia ja ylemmän tason taulukon välisen esimerkiksi`Employee.ID=History.EmployeeID`
- Jos haluat seurata tapahtuman ylemmän tason taulukon historia-taulukkoa, sinun on määritettävä sarakkeen nimi, joka sisältää toiminnon tiedot (lisääminen, päivittäminen ja poistaminen).
- Jos valitset vesileiman delta muutosten jäljittäminen, anna sarakkeen nimi, joka sisältää toiminnon tiedot **Vesi Merkitse sarakkeen nimi**.
- **Muuta tyyppi-määrite** -sarakkeen vaaditaan Muuta tyyppi. Tämän sarakkeen yhdistää muutoksen, joka esiintyy ensisijainen taulukko tai moniarvoisen taulukon muutostyyppi delta-näkymässä. Tässä sarakkeessa voi olla Modify_Attribute muutostyyppi määritettä tason muuttaminen tai lisääminen, muokkaaminen, tai poista objekti tason Muuta tyyppi-tyypin muuttaminen. Jos se on jokin muu kuin oletusarvo Lisää, Muokkaa, tai poista, voit määrittää kyseiset arvot tämä vaihtoehto.

### <a name="export"></a>Vie
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Yleinen SQL Connector tuki Vie käyttää neljää tukemalla, kuten:

- Taulukko
- Näytä
- Tallennettu toimintosarja
- SQL-kysely

**Taulukon tai näkymän**  
Jos valitset/taulukkonäkymässä-vaihtoehdon, yhdistimen Luo vastaaviin kyselyt tekemään Vie.

**Tallennettujen toimintosarjojen**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Jos valitset tallennettu toimintosarja-vaihtoehdon, vienti edellyttää kolmen eri tallennetut menetelmiä suorittaa lisääminen, päivittäminen ja poistaminen.

- **Lisää SP nimi**: Tämä SP suoritetaan, jos objektin yhdistimen vastaaviin taulukon lisäämistä varten.
- **Päivitä SP nimi**: Tämä SP suoritetaan, jos objektit tulevat yhdistimen päivityksen vastaaviin taulukossa.
- **Poista SP nimi**: Tämä SP suoritetaan, jos objektit tulevat yhdistimen vastaaviin taulukon poistamista varten.
- Määrite valittuna käyttää tallennetun toimintosarjan parametriarvo rakenteesta. Esimerkiksi `EmployeeName: INPUT: @EmployeeName` (EmployeeName valitaan määritelty yhdistimen ja yhdistimen korvaa vastaavan arvon, kun teet vienti)
- Suorittaa Parametroitu tallennetun toimintosarjan, anna parametrien `[Name]:[Direction]:[Value]` muodossa. Kirjoita kunkin parametrin omalle rivilleen (Käytä Ctrl + Enter saadaksesi uuden rivin).

**SQL-kysely**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Jos valitset SQL-kysely-vaihtoehdon, vienti edellyttää kolmen eri kyselyjä suorittaa lisääminen, päivittäminen ja poistaminen.

- **Lisää kyselyn**: Tämä kysely suoritetaan, jos minkä tahansa objektin yhdistimen vastaaviin taulukon lisäämistä varten.
- **Päivitä kyselyn**: Tämä kysely suoritetaan, jos objektit tulevat yhdistimen päivityksen vastaaviin taulukossa.
- **Poistokysely**: Tämä kysely suoritetaan, jos objektit tulevat yhdistimen vastaaviin taulukon poistamista varten.
- Valittu käytetään parametrin arvon kyselyyn, kuten rakenteen määrite`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Vianmääritys

-   Saat lisätietoja siitä, miten voit ottaa kirjaamisen vianmääritys yhdistin, [Voit ottaa käyttöön tapahtumien seuranta jäljitys yhdistimet](http://go.microsoft.com/fwlink/?LinkId=335731).
