<properties
   pageTitle="PowerShell-yhdistin | Microsoft Azure"
   description="Tässä artikkelissa kuvataan Microsoft Windows PowerShell-yhdistin määrittämisestä."
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

# <a name="windows-powershell-connector-technical-reference"></a>Windows PowerShell-yhdistin tekniset tiedot
Tässä artikkelissa kuvataan Windows PowerShell-yhdistin. Artikkeli koskee seuraavia tuotteita:

- Microsoftin Käyttäjätietojen hallinta 2016 (MIM2016)
- Forefront käyttäjätietojen hallinta 2010 R2 (FIM2010R2)
    -   Käytä korjaustiedoston 4.1.3671.0 tai uudempi [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ja FIM2010R2 yhdistin on ladattavissa [Microsoft Download Centeristä](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Yleistä PowerShell-yhdistin
PowerShell-yhdistin mahdollistaa synkronointipalvelua integroida ulkoiset järjestelmät, jotka tarjoavat Windows PowerShell-pohjainen API. Yhdistimen on siltaa välille puhelun perustuva extensible yhteyksien hallinta-agentti ominaisuuksia 2 (ECMA2) framework ja Windows PowerShell. Lisätietoja ECMA framework Hae [Extensible Connectivity 2.2 hallinnan agentti tiedot](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Edellytykset
Ennen kuin käytät yhdistimen, varmista, että sinulla on synkronointi-palvelimeen seuraavasti:

- Microsoft .NET 4.5.2 Framework tai uudempi versio
- Windows PowerShell 2.0-, 3.0- tai 4.0

Synkronointipalvelu palvelimen suorittamisen-käytäntö on määritetty sallimaan yhdistimen Windows PowerShell-komentosarjojen suorittamisen. Ellei yhdistimen suoritetaan on allekirjoitettu digitaalisesti, komentosarjojen suorittamisen käytännön määrittäminen suorittamalla tämän komennon:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Luo uusi yhdistin
Windows PowerShell-yhdistin luominen synkronointipalvelua, sinun on määritettävä Windows PowerShell-komentosarjojen, jotka suorittavat synkronointipalvelua pyytäjä vaiheet sarjaa. Sen mukaan, voit muodostaa yhteyden tietolähteeseen ja tarvitset toimintoja sinun on otettava käyttöön komentosarjoja vaihtelee. Tässä osassa kuvataan kunkin komentosarjat, jotka voidaan toteuttaa ja kun niitä ei tarvita.

Windows PowerShell-yhdistin on suunniteltu säilyttämään kunkin komentosarjat synkronointipalvelu tietokannan sisällä. Kun se on mahdollista suorittaa komentosarjoja, jotka on tallennettu tiedostojärjestelmässä, on helpompi Lisää kunkin komentosarjan suoraan yhdistimen kokoonpanon leipäteksti.

Luo PowerShell-yhdistin **Synkronointipalvelu** Valitse **Hallinta-agentti** ja **Luo**. Valitse yhdistin **PowerShell (Microsoft)** .

![Yhdistimen luominen](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Yhteys
Lisää parametreja etäjärjestelmässä muodostamisesta. Nämä arvot suojatusti synkronointipalvelu tallentamat ja saataville Windows PowerShell-komentosarjojen suorittamisen yhdistin.

![Yhteys](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Voit määrittää seuraavat Connectivity parametrit:

**Yhteys**

Parametri | Oletusarvo | Tarkoitus
--- | --- | ---
Palvelin | <Blank> | Palvelimen nimi, jonka yhdistimen yhdistäminen.
Toimialueen | <Blank> | Tunnistetietojen tallentamiseen käytettäväksi yhdistimen suoritetaan toimialue.
Käyttäjän | <Blank> | Tallentaa käytettäviksi yhdistimen suoritettaessa tunnistetieto käyttäjänimi.
Salasana | <Blank> | Tunnistetietojen tallentamiseen käytettäväksi yhdistimen suoritetaan salasana.
Connector-tilin tekeytyminen | EPÄTOSI | Kun arvo on TOSI, synkronointipalvelu suoritetaan Windows PowerShell-komentosarjojen annetut valtuudet kontekstissa. Jos mahdollista, on suositeltavaa, että **$Credentials** -parametri on välitetty jokaiselle komentosarjaa käytetään tekeytyminen sijaan. Saat lisätietoja Lisää käyttöoikeuksia, joita tarvitaan tämän asetuksen, [Tekeytyminen lisämäärityksiä](#additional-configuration-for-impersonation).
Käyttäjäprofiilin lataaminen, kun tekeytyminen | EPÄTOSI | Ohjaa Windowsia lataamaan yhdistimen tunnistetiedot käyttäjäprofiili tekeytyminen aikana. Jos tekeytyneillä käyttäjällä on profiili, yhdistimen ei ladata profiili. Saat lisätietoja muita käyttämään parametriä tarvittavat oikeudet [Tekeytyminen lisämäärityksiä](#additional-configuration-for-impersonation).
Kirjautuminen tyyppi, kun tekeytyminen | Ei mitään | Kirjoita tekeytyminen aikana. Saat lisätietoja viitata [dwLogonType] [ dw] ohjeissa.
Allekirjoitetun komentosarjat | EPÄTOSI | Jos arvo on TOSI, Windows PowerShell-yhdistin vahvistaa, että kukin komentosarja on kelvollinen digitaalinen allekirjoitus. Jos arvo on EPÄTOSI, varmista, että synkronointipalvelu server Windows PowerShellin suorittamisen käytäntö on RemoteSigned tai Unrestricted.

**Yleinen moduuli**  
Yhdistimen avulla voit tallentaa jaetun Windows PowerShell-moduulin määrityksessä. Yhdistimen suoritettaessa komentosarjan Windows PowerShell-moduulin puretaan tiedostojärjestelmän niin, että se voidaan tuoda kunkin komentosarjalla.

Tuominen, vieminen ja salasanan synkronointi komentosarjojen yleinen moduuli puretaan yhdistimen MAData kansioon. Rakenteen, vahvistus tai hierarkian osion etsintä-komentosarjojen yleinen moduuli puretaan % TEMP %-kansio. Kummassakin tapauksessa poimitun yleinen moduuli komentosarja nimetään mukaan moduuli-komentosarjan kutsumanimi-asetus.

Lataa moduuli kutsutaan FIMPowerShellConnectorModule.psm1 MAData-kansiosta, käytä seuraavan lauseen:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Lataa moduuli kutsua FIMPowerShellConnectorModule.psm1 % TEMP %-kansio, käytä seuraavan lauseen:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Parametrin vahvistus**  
Vahvistus-komentosarja on valinnainen Windows PowerShell-komentosarja, joka voidaan varmistaa, että yhdistimen määritykset parametreja järjestelmänvalvoja ovat kelvollisia. Olevan palvelimeen, yhteyden tunnistetiedot ja yhteyden parametrit ovat yleisiä käytöt vahvistus-komentosarjan. Vahvistus-komentosarjan kutsutaan jälkeen seuraavat välilehdet ja valintaikkunat muokataan:

- Yhteys
- Yleiset parametrit
- Osion määritys

Vahvistus-komentosarjan saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | Määritys-välilehti tai valintaikkuna, joka on käynnistänyt pyynnön vahvistus.
ConfigParameters | [KeyedCollection] [ keyk] [merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.

Vahvistus-komentosarjan pitäisi palata putkijohto ParameterValidationResult yhtenä objektina.

**Mallin etsiminen**  
Mallin etsiminen komentosarja on pakollinen. Tämä komentosarja palauttaa objektityypit, määritteet ja määritteen rajoituksia, jotka synkronointipalvelua käytetään määritettäessä määrite postinkulun säännöt. Mallin etsiminen komentosarja suoritetaan yhdistimen luonnin aikana ja lisää yhdistimen rakenne. Sitä käytetään myös päivittää rakenne-toiminnon synkronoinnin palvelun hallinta.

Mallin etsiminen komentosarja saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters | [KeyedCollection] [ keyk] [merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.

Komentosarja on palautettava yksittäisen [rakenteen] [ schema] putkijohto objekti. Rakenne-objektin koostuu [SchemaType] [ schemaT] objekteja, jotka edustavat objektityypit (esimerkiksi: käyttäjät ja ryhmät). SchemaType objektissa on kokoelma [SchemaAttribute] [ schemaA] objekteja, jotka edustavat määritteet (esimerkiksi: etunimi, Sukunimi ja postiosoite) tyyppi.

**Lisää parametrit**  
Vakio asetusten lisäksi voit määrittää mukautetun lisämäärityksiä asetuksia, jotka ovat tietyn yhdistimen esiintymään. Nämä parametrit voidaan määrittää yhdistimen-osio, milloin tai Suorita vaihe tasoilla ja pääsee asiaa Windows PowerShell-komentosarjaa. Mukautettujen Hakumääritysten asetusten voidaan tallentaa vain teksti-muodossa synkronointipalvelu tietokannan tai ne voidaan salata. Synkronointipalvelu automaattisesti salaa ja purkaa suojatun asetukset tarvittaessa.

Jos haluat määrittää mukautettuja asetuksia, erota kunkin parametrin nimi pilkku (,).

Mukautettujen Hakumääritysten asetusten komentosarjan käyttöön jälkiliite nimi, jossa on alaviiva ( \_ ) ja laajuus-parametrin (Yleinen, osion tai RunStep). Esimerkiksi käyttää Yleinen tiedostonimi-parametrin, käytä tämän koodikatkelman:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Ominaisuudet
Suunnittelun hallinta-agentti Ominaisuudet-välilehti määrittää toiminta ja yhdistimen toimintoja. Tässä välilehdessä tehdyt valinnat ei voi muokata, kun yhdistin on luotu. Tässä taulukossa luetellaan ominaisuuksien asetukset.

![Ominaisuudet](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

Ominaisuus | Kuvaus |
--- | --- |
[DN-nimi-tyyli][dnstyle] | Ilmaisee, jos yhdistimen tukee erottaa nimet ja, mitä tyyli.
[Vie tyyppi][exportT] | Määrittää objekteja, jotka on esitetty vientikomentosarja. <li>AttributeReplace – on moniarvoinen määritteen koko arvojoukon määrite muuttuessa.</li><li>AttributeUpdate – sisältää vain moniarvoinen määritteeseen deltas määrite muuttuessa.</li><li>MultivaluedReferenceAttributeUpdate - sisältää koko-taustatiedot moniarvoinen määritteet ja vain deltas moniarvoinen viittaus määritteiden.</li><li>ObjectReplace – sisältää kaikki objektin määritteet, kun jokin määritteisiin tehdyt muutokset</li>
[Tietojen normalisointi][DataNorm] | Ohjaa normitettava ankkurin määritteitä, ennen kuin ne on tarkoitettu komentosarjojen synkronointipalvelu.
[Objektin vahvistus][oconf] | Määrittää odotetaan tuo toiminnan synkronointipalvelua. <li>Normaali – oletustoiminnon, odottaa vahvistettava kautta Tuo kaikki viedyn muutokset</li><li>NoDeleteConfirmation – kun objekti on poistettu, ei ei ole luotu odotetaan Tuo.</li><li>NoAddAndDeleteConfirmation – kun objektin luodaan tai poistetaan, ei ei ole luotu odotetaan Tuo.</li>
Käyttää ankkurin DN | Jos DN nimi-tyyli on määritetty LDAP-yhdistin välilyönti ankkuri-määrite on myös DN-nimi.
Useita yhdistimet rinnakkaiset toiminnot | Kun valintaruutu on valittuna, useita Windows PowerShell-yhdistimiä voidaan suorittaa samanaikaisesti.
Osiot | Kun valintaruutu on valittuna, yhdistimen tukee useita osioita ja osion etsiminen.
Hierarkia | Kun valintaruutu on valittuna, yhdistimen tukee LDAP tyylin hierarkkisessa rakenteessa.
Tuo ottaminen käyttöön | Kun valintaruutu on valittuna, yhdistimen tuo tietojen tuonti-komentosarjojen kautta.
Ota käyttöön Delta-tuonti | Kun valintaruutu on valittuna, yhdistimen voi pyytää deltas tuonti-komentosarjoja.
Ota käyttöön vienti | Kun valintaruutu on valittuna, yhdistimen Vie tiedot Vie-komentosarjojen kautta.
Ota käyttöön koko vienti | Kun valintaruutu on valittuna, vie-komentosarjoja tukevat vieminen koko connector-tilaa. Jos haluat käyttää tätä asetusta, ota käyttöön vie myös tarkastettava.
Viittaus ei ole arvoja ensimmäisen Vie kerralla | Kun valintaruutu on valittuna, viittaus määritteet viedään toisen Vie kerralla.
Ota käyttöön objektin uudelleennimeäminen | Kun valintaruutu on valittuna, erottaa nimet voi muokata.
Poista-Lisää korvata | Kun valintaruutu on valittuna, Poista-Lisää toimintojen viedään yhteen korvaa.
Salasanan toimintojen ottaminen käyttöön | Kun valintaruutu on valittuna, salasanan synkronointi komentosarjoja tuetaan.
Ota käyttöön Vie salasanan ensimmäisellä kerralla | Kun valintaruutu on valittuna, salasanojen määrittäminen aikana valmistelu viedään, kun objekti on luotu.

### <a name="global-parameters"></a>Yleiset parametrit
Yleinen Parametrit-välilehti, hallinta agentti suunnittelussa avulla voit määrittää Windows PowerShell-komentosarjojen, jotka suoritetaan yhdistin. Voit myös määrittää mukautettujen Hakumääritysten asetusten yhteys-välilehden määritysten yleinen arvot.

**Osion etsiminen**  
Osio on erillinen nimitilan jaetun rakenteita kuluessa. Active Directoryn jokaisen toimialueen on yksi metsää sisällä osio. Osio on looginen ryhmittely tuoda ja viedä toimintoja. Tuonti ja vienti on osion kontekstin ja kaikki toiminnot tapahtuu tässä yhteydessä. Osioiden katsomaan edustavat LDAP hierarkian. Osion DN-nimi käytetään tuo varmistaa, että kaikki palautetut objektit ovat puitteissa osio. Osion DN-nimi käytetään myös aikana valmistelu kohteesta metaverse connector-kohtaan objektin on oltava yhdistettynä vietäessä osion määrittämiseen.

Osion etsiminen komentosarja saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters  | [KeyedCollection][keyk][merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.

Komentosarja on palautettava joko yksittäinen [osion] [ part] objekti tai osion objektit putkijohto luettelo [T].

**Hierarkian etsiminen**  
Hierarkian etsiminen komentosarja käytetään vain silloin, kun DN nimen tyyli-ominaisuus on LDAP. Komentosarjan avulla voit Selaa ja valitse Aseta säilöjä, pidetään tai loitontaminen vaikutusalueen Tuo ja vie toimintoja. Komentosarjan pitäisi sisältää vain solmut, jotka ovat suoraan lasten komentosarja yhdistettävän pääkansion solmun luettelo.

Hierarkian etsiminen komentosarja saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.
ParentNode | [HierarchyNode][hn] | Pääkansio solmu vallitessa komentosarja palauttaa suoraan lasten hierarkian.

Komentosarja on palaa joko yksittäinen lapsen HierarchyNode-objektin tai alemman tason HierarchyNode objektien luettelon [T] putkijohto.

#### <a name="import"></a>Tuo
Yhdistimiä, jotka tukevat tuontitoimenpiteitä on pantava kolme komentosarjoja.

**Aloita tuonti**  
Aloita tuonti-komentosarja suoritetaan alkuun, Suorita tuonti-vaihe. Tässä vaiheessa voit yhteyden muodostaminen tietolähteen järjestelmän tai tehdä valmistelemaan ennen tietojen tuominen yhdistetyssä järjestelmässä.

Aloita tuonti komentosarja saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Ilmoittaa komentosarja tuonnin suorittaminen (delta tai täysi versio)-osion, hierarkia, vesileima ja odotetun sivukoko tyypistä.
Tyypit | [Rakenne][schema] | Rakenteen yhdistimen välilyönti, joka on tuotu.

Komentosarja on palautettava yksittäisen [OpenImportConnectionResults] [ oicres] objektin putkijohto, esimerkiksi:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Tietojen tuominen**  
Tuo tiedot-komentosarjan kutsutaan yhdistin, kunnes komentosarja osoittaa, että ei ole tietoja, voit tuoda. Windows PowerShell-yhdistin on 9 999 objektien sivun koon. Jos komentosarja palauttaa enintään 9 999 objektien tuontia, sinun on tuettava sivutus. Yhdistimen näyttää, että voit käyttää säilön vesileiman niin, että aina, kun komentosarja tuo tiedot mukautetut tiedot-ominaisuutta kutsutaan, komentosarja jatkaa tuodaan objekteja, mihin se jäi.

Tuo tiedot-komentosarjan saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.
GetImportEntriesRunStep | [ImportRunStep][irs] | Pitää vesileimaa (CustomData), joka voidaan aikana sivutettu tuonti ja delta Tuo.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Ilmoittaa komentosarja tuonnin suorittaminen (delta tai täysi versio)-osion, hierarkia, vesileima ja odotetun sivukoko tyypistä.
Tyypit | [Rakenne][schema] | Rakenteen yhdistimen välilyönti, joka on tuotu.

Tuo tiedot-komentosarja on kirjoitettava luettelon [[CSEntryChange][csec]] putkijohto objekti. Tämä kokoelma koostuu CSEntryChange määritteitä, jotka vastaavat kunkin objektin tuotavassa. Täysi tuonti-asennuksen aikana tämän sivustokokoelman pitäisi olla joukon CSEntryChange objektit, joissa on kaikkien objektien kaikki määritteet. Aikana sama.arvo tuoda CSEntryChange objektin tulisi sisältää joko määrite tason deltas objekteja, voit tuoda tai esityksen objektit, jotka ovat muuttuneet (korvaa tilan).

**Lopeta tuominen**  
Lopeta tuominen komentosarja suoritetaan aina, Suorita tuonti. Tämä komentosarja suorittama uudelleenjärjestäminen tehtäviä tarvittavat (esimerkiksi Sulje yhteydet järjestelmiin) ja vastata virheet.

Lopeta tuo komentosarja saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Ilmoittaa komentosarja tuonnin suorittaminen (delta tai täysi versio)-osion, hierarkia, vesileima ja odotetun sivukoko tyypistä.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | Komentosarja ilmoittaa syy tuonti päättyi.

Komentosarja on palautettava yksittäisen [CloseImportConnectionResults] [ cicres] objektin putkijohto, esimerkiksi:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Vie
Sama kuin yhdistimen tuonti-arkkitehtuuri, yhdistimiä, jotka tukevat Vie käyttöön kolme komentosarjoja.

**Aloita vienti**  
Aloita vientikomentosarja suoritetaan Vie Suorita vaihe alussa. Tässä vaiheessa voit muodostaa yhteyden lähde-järjestelmään ja Järjestä valmistelevat toimenpiteet ennen tietojen vieminen yhdistetyssä järjestelmässä.

Aloita vientikomentosarja saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Ilmoittaa komentosarja Vie suorittaminen (delta tai täysi versio)-osion, hierarkia ja odotetun sivukoon tyypistä.
Tyypit | [Rakenne][schema] | Malli, joka on viety yhdistimen välilyönti.

Komentosarja ei olisi palauttaa tuloksen putkijohto.

**Tietojen vieminen**  
Synkronointipalvelu puhelujen tietojen vieminen komentosarjaa niin monta kertaa kuin on tarpeen käsitellä kaikki odotetaan vienti. Jos connector-tilaa on enemmän kuin yhdistimen sivukoko odotetaan viennin, Vie tiedot komentosarja saattaa voidaan kutsua useita kertoja ja mahdollisesti useita kertoja samaan objektiin.

Vie tiedot komentosarja saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.
CSEntries | Seuraavat: IList[CSEntryChange][csec] | Luettelo yhdistimen tilaa objektien kanssa odottavat viennin käsitellään tämän vaiheen aikana.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Ilmoittaa komentosarja Vie suorittaminen (delta tai täysi versio)-osion, hierarkia ja odotetun sivukoon tyypistä.
Tyypit | [Rakenne][schema] | Malli, joka on viety yhdistimen välilyönti.

Vie tiedot komentosarja on palautettava [PutExportEntriesResults] [ peeres] putkijohto objekti. Tätä objektia ei tarvitse seuraavat tuloksen tiedot jokaisen viedyn yhdistimen, ellei tapahtuu virhe tai muuta ankkuri-määritteeseen. Jos esimerkiksi haluat palauttaa PutExportEntriesResults objektin putkijohto:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Lopeta vienti**  
Aina, suorittaa loppuun Vie-komentosarjan suorittaminen, Vie. Tämä komentosarja suorittama uudelleenjärjestäminen tehtäviä tarvittavat (esimerkiksi Sulje yhteydet järjestelmiin) ja vastata virheet.

Lopeta vientikomentosarja saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Ilmoittaa komentosarja Vie suorittaminen (delta tai täysi versio)-osion, hierarkia ja odotetun sivukoon tyypistä.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | Komentosarja ilmoittaa syy vieminen on päättynyt.

Komentosarja ei olisi palauttaa tuloksen putkijohto.

#### <a name="password-synchronization"></a>Salasanojen synkronoinnin
Windows PowerShell-yhdistimet voidaan salasanan muutokset ja palauttaa kohteen.

Salasana-komentosarjan saa seuraavat parametrit yhdistin:

Nimi | Tietotyyppi | Kuvaus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][merkkijono, [ConfigParameter][cp]] | Yhdistimen määritykset parametrit taulukko.
Tunnistetietojen | [PSCredential][pscred] | Sisältää yhteys-välilehden järjestelmänvalvoja kirjoitettua tunnistetietoja.
Osion | [Osion][part] | Hakemisto-osio, joka CSEntry sijaitsee.
CSEntry | [CSEntry][cse] | Yhdistimen tilaa merkinnän objekti, joka on vastaanotettu salasanan vaihtaminen tai palauttaminen.
OperationType | Merkkijono | Ilmaisee, onko toiminnon palauttaminen (**SetPassword**) tai muuttaa (**salasanan vaihtaminen**).
PasswordOptions | [PasswordOptions][pwdopt] | Liput, jotka määrittävät tarkoitettu salasanan palauttaminen toiminta. Tämä parametri on vain, jos OperationType on **SetPassword**.
Vanha_salasana | Merkkijono | Täytetty objektin vanha salasana salasana-muutokset. Tämä parametri on vain, jos OperationType on **salasanan vaihtaminen**.
Uusisalasana | Merkkijono | Täytetty objektin uusi salasana, jonka komentosarja on määritetty.

Salasanan komentosarja ei odoteta palauttaa tuloksia Windows PowerShell-myyntijakso. Virheen ilmetessä salasanan komentosarja-komentosarja olisi palauttaa yhden ilmoittaa synkronointipalvelu ongelmaa seuraavin poikkeuksin:

- [PasswordPolicyViolationException] [ pwdex1] – ilmenee, jos salasana ei täytä Salasanakäytäntö yhdistetyssä järjestelmässä.
- [PasswordIllFormedException] [ pwdex2] – ilmenee, jos salasana ei ole yhdistetty järjestelmän hyväksyttävät.
- [PasswordExtension] [ pwdex3] – ilmenee muita salasana-komentosarjan virheet.

## <a name="sample-connectors"></a>Esimerkki yhdistimet
Yleiskatsaus käytettävissä otoksen yhdistimet, artikkelissa [Windows PowerShellin yhdistimen otoksen yhdistimen sivustokokoelman][samp].

## <a name="other-notes"></a>Muut muistiinpanot

### <a name="additional-configuration-for-impersonation"></a>Tekeytymistaso lisämäärityksiä
Myönnä käyttäjälle, joka on pelkästään synkronointipalvelu palvelimessa seuraavat oikeudet:

Seuraavissa rekisteriavaimissa lukuoikeudet:

- HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Software\Microsoft\PowerShell
- HKEY_USERS\\[SynchronizationServiceServiceAccountSID] \Environment

Voit selvittää suojauksen tunnus (SUOJAUSTUNNUS) synkronointipalvelu palvelutilin, suorita PowerShell-komennot:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Lukuoikeudet tiedoston järjestelmä-kansioihin:

- %ProgramFiles%\Microsoft forefront tunnistetietojen Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft forefront tunnistetietojen Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft forefront tunnistetietojen Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Korvaa Windows PowerShell-yhdistimen nimi {ConnectorName} paikkamerkin.

## <a name="troubleshooting"></a>Vianmääritys

-   Saat lisätietoja siitä, miten voit ottaa kirjaamisen vianmääritys yhdistin, [Voit ottaa käyttöön tapahtumien seuranta jäljitys yhdistimet](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
