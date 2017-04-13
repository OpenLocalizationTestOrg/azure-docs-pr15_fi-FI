<properties
   pageTitle="Azure SQL-tietovarasto tarkistaminen | Microsoft Azure"
   description="Azure SQL-tietovarasto tarkistaminen käytön aloittaminen"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>Azure SQL-tietovarasto tarkistaminen

> [AZURE.SELECTOR]
- [Valvonta](sql-data-warehouse-auditing-overview.md)
- [Uhkien tunnistus](sql-data-warehouse-security-threat-detection.md)

SQL-tietovarasto tarkistaminen antaa tapahtumien tarkastus tietokannan Kirjaudu Azure-tallennustilan tilin tietueeseen. Seurannan avulla voit säilyttää säädösten noudattamisen, ymmärtää tietokannan toiminta ja Hanki tietoja ristiriidat ja poikkeamia, joka saattaa johtua business koskee tai epäilty suojauksen virheitä. SQL-tietovarasto tarkistaminen myös integroitu Microsoft Power BI alirakenteen raportointia ja analysointia varten.

Tarkistustyökalujen ottaminen käyttöön ja helpottaa yhteensopivuusstandardeja ehdokasvaltio mutta ei takaa yhteensopivuuden. Saat lisätietoja Azure ohjelmat, jotka tukevat vaatimusten noudattaminen <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Valvontakeskus</a>.

+ [Tietokannan valvonta perusteet]
+ [Määritä tietokannan tarkistaminen]
+ [Voit analysoida valvontalokien ja raportit]

##<a id="subheading-1"></a>Azure SQL Data Warehouse tietokannan tarkistaminen perusteet


SQL-tietovarasto tietokannan tarkistaminen avulla voit:

- **Säilytä** kirjausketju valitut tapahtumat. Voit määrittää tietokannan toiminnot valvottavat luokkiin.
- **Raportin** tietokannan toiminta. Voit käyttää ennalta määritettyjä raportteja ja Raporttinäkymät-ikkunan avulla pääset alkuun nopeasti tehtävän ja tapahtumaraportointi.
- **Analysoi** -raportit. Löydät epäilyttävistä tapahtumia, epätavallinen tehtävän ja trendien.

Voit määrittää seurannan seuraavat tapahtumaluokat:

**Tavallinen SQL** : n ja **Parametroitu SQL** , johon kerätään valvonta kirjaa luokitellaan  

- **Tietojen käyttäminen**
- **Rakenteen muutokset (DDL)**
- **Tietojen muutokset (Enimmäiskuolleisuusrajaa)**
- **Asiakkaat, rooleja ja käyttöoikeuksia (Tietoyhteyskirjaston)**
- **Tallennettu toimintosarja**, **Kirjaudu sisään** ja **tapahtumien hallinta**.

Tapahtuman kussakin luokassa valvonta **onnistumisen** ja **virheen** toimintojen määritetään erikseen.

Lisätietoja toimet ja tapahtumat, tapahtumista Hae <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Valvonta lokin muodon ohje (doc-tiedoston lataaminen)</a>.

Valvontalokien tallennetaan Azure-tallennustilan tilin. Voit määrittää valvonta-loki säilytysaika.

Valvonta käytäntö voidaan määrittää tietokannan avaamiseksi tai palvelimen oletuskäytäntö. Oletusarvoinen palvelimen valvonta käytännön koskevat kaikki tietokannat palvelimessa, jossa ei ole tietyn ohittavaa tietokannan valvonta käytännön määritetty.

Ennen asetus ylös valvonta valvonta-valintaruutu, jos käytössäsi on ["alatason asiakkaan"](sql-data-warehouse-auditing-downlevel-clients.md).


##<a id="subheading-2"></a>Määritä tietokannan tarkistaminen

1. Käynnistä <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.

2. Siirry SQL-tietovarasto tietokannan määritys-sivu / SQL Server, jota haluat valvoa. Napsauta **asetukset** -painikkeen päälle ja valitse sitten asetukset-sivu ja valitse **valvonta**.

    ![][1]

3. Valvonta määritys-sivu, valitse Poista **Perivät valvonta-asetukset-palvelimesta** -valintaruutu. Voit määrittää tietyn tietokannan asetusten.

    ![][2]

4. Seuraavaksi valvoa napsauttamalla **-** painiketta.

    ![][3]

5. Valitse Avaa valvonta lokit tallennustilan sivu **TALLENNUSTILAN tiedot** valvonta määritys-sivu. Valitse Azure-tallennustilan tilin lokit tallennuspaikka ja säilytysaika. **Vihje:** Eniten irti esimääritettyjä raportteja mallit kaikkien valvottuja tietokantojen saman tallennustilan tilin avulla.

    ![][4]

6. Napsauta Tallenna tallennustilan tiedot kokoonpanon **OK** -painiketta.


7. **Lokiin kirjaaminen tapahtuman mukaan**, valitse Valitse **ONNISTUMISESTA** tai **EPÄONNISTUMISESTA** kirjautua kaikki tapahtumat, tai valitse yksittäisen tapahtuman luokat.


8. Jos olet määrittämässä valvonta tietokantaan, saatat joutua muuta yhteysmerkkijono asiakkaalle, jotta tietojen tarkistaminen kirjataan oikein. Tarkista alatason asiakasyhteydet [Muokata palvelimen FDQN yhteysmerkkijonon](sql-data-warehouse-auditing-downlevel-clients.md) ohjeaiheen.

9. Valitse **OK**.


##<a id="subheading-3">Voit analysoida valvontalokien ja raportit</a>

Valvontalokien sisälly kaupan taulukoiden kokoelma **SQLDBAuditLogs** etuliite Azure-tallennustilan tilin annat asennuksen yhteydessä. Voit tarkastella lokitiedostojen, kuten <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure-tallennustilan Explorer</a>-työkalun avulla.

Valmiiksi määritetyt Raporttinäkymät-ikkunan raporttimalli on käytettävissä <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">ladattavan Excel-laskentataulukon</a> haluat analysoida tietoja nopeasti. Mallin käyttämisestä oman valvontalokien, tarvitset Excel 2013- tai uudempi versio ja Power Query, jonka voit ladata <a href="http://www.microsoft.com/download/details.aspx?id=39379">tähän</a>.

Malli on kuvitteellista mallitiedot ja voit määrittää Power Query tuo oman valvontaloki suoraan Azure-tallennustilan tilin.

Tarkempia ohjeita raporttimallin käsittelemisestä Lue <a href="http://go.microsoft.com/fwlink/?LinkId=506731">How To (tiedoston lataaminen)</a>.

![][5]


##<a id="subheading-4">Tuotannon käyttö käytännöt</a>
Tässä osassa kuvaus viittaa näyttökuvien yläpuolella. <a href="https://portal.azure.com" target="_blank">Azure-portaalin</a> tai <a href= "https://manage.windowsazure.com/" target="_bank">Perinteinen Azure perinteinen Portal</a> voidaan käyttää.


##<a id="subheading-5"></a>Tallennustilan avaimen uudistaminen

Tuotannon, olet todennäköisesti Päivitä tallennustilan avaimien säännöllisin väliajoin. Päivitä kun avaimien on tallennettava uudelleen käytännön. Prosessi on seuraavanlainen:.


1. Siirry **Tallennustilan pikanäppäin** - *ensisijaisen* *Toissijainen* ja **Tallenna**valvonta määritys-sivu (edellä kuvatulla tavalla Määritä valvonta-osiossa).
![][4]
2. Siirry tallennustilan määritys-sivu ja **Luo** *Access-perusavain*.

3. Siirry takaisin valvonta määritys-sivu, siirry **Tallennustilan pikanäppäin** - *Toissijainen* *ensisijainen* ja paina **Tallenna**.

4. Palaa tallennuskiintiön Käyttöliittymä ja **Luo** *Toissijainen pikanäppäin* (kuten seuraava näppäimet päivityksen jakson valmistautumista.

##<a id="subheading-6"></a>Automaatio
On useita PowerShell cmdlet-komentojen avulla voit määrittää tarkistaminen Azure SQL-tietokantaan. Voit käyttää valvonta cmdlet-komennot käytössä on oltava PowerShellin Azure Resurssienhallinta-tilassa.

> [AZURE.NOTE] [Azure Resurssienhallinta](https://msdn.microsoft.com/library/dn654592.aspx) -moduuli on tällä hetkellä esikatselu. Se ei ehkä tarjoa saman hallintaominaisuuksien kuin Azure moduuli.

Kun olet Azure Resurssienhallinta-tilassa, suorita `Get-Command *AzureSql*` käytettävissä cmdlet-komennot-luettelo.


<!--Anchors-->
[Tietokannan valvonta perusteet]: #subheading-1
[Määritä tietokannan tarkistaminen]: #subheading-2
[Voit analysoida valvontalokien ja raportit]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
