<properties 
   pageTitle="Mikä on StorSimple? | Microsoft Azure" 
   description="Kuvaa StorSimple tiering, laite, virtual laitteen, ja niiden käyttöön tallennusvälineiden hallinta ja esitellään tärkeimmät StorSimple termit." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="10/05/2016"
   ms.author="v-sharos@microsoft.com"/>

# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000 sarja: hybrid cloud tallennustilan ratkaisu

## <a name="overview"></a>Yleiskatsaus

Tervetuloa käyttämään Microsoft Azure StorSimple integroitu tallennustilan-ratkaisun, joka hallitsee tallennustilan tehtävät paikallisen laitteiden ja Microsoft Azure-pilvitallennustilaa välillä. StorSimple on tehokasta, edullinen ja helposti ohjattavan tallennustilan-alueen verkon (SAN) ratkaisu, joka korjaa useita ongelmia ja yrityksen tallennustilan ja tietojen suojaukseen liittyviä kulut. Se käyttää omia StorSimple 8000 sarjan laitteella, pilvipalveluihin integroituu ja on hallintatyökalut saumaton näkymän kaikki yrityksen tallennustilaa, mukaan lukien pilvitallennustilaa. (Julkaistu Microsoft Azure-sivustossa StorSimple käyttöönotto-tiedot koskevat vain StorSimple 8000 sarjan laitteita. Jos käytät StorSimple 5000/7000 sarjan laitetta, siirry [StorSimple ohjeen](http://onlinehelp.storsimple.com/).)

StorSimple käyttää [tallennustilan tiering](#automatic-storage-tiering) hallitsemaan eri tietovälineestä tallennettuja tietoja. Nykyinen työsarja on tallennetaan paikallisesti tasainen tilan asemista (SSDs), harvoin käytettävät tiedot tallentuvat kiintolevyaseman levyasemien (HDDs) ja arkistointia tiedot on miten pilveen. Lisäksi StorSimple käyttää kopioinnin peruuttaminen ja pakkaus vähentää tallennustilan, jota tiedot siinä käytetään. Jos haluat lisätietoja, siirry [kopioinnin peruuttaminen ja pakkaus](#deduplication-and-compression). Siirry määritelmät muiden avaimen termejä ja käyttää niitä sarjaan StorSimple 8000 käsitteitä [StorSimple sanasto](#storsimple-terminology) on tämän artikkelin lopussa.

StorSimple päivitys 2 saat selville tarvittavat tietomääristä kuin varmistaaksesi, että ensisijainen tiedot ovat varmasti paikallisessa laitteelle ja ei taso pilveen *kiinnitetyt paikallisesti* . Näin voit suorittaa toiminnoista, jotka ovat samalla, kun Käytä pilveen varmuuskopiointi luottamuksellisia cloud viive, kuten SQL- ja virtuaalikoneen toiminnoista, valitse paikallisesti kiinnitetyt asemat. Lisätietoja paikallisesti kiinnitetty tietomääristä artikkelissa [StorSimple hallintapalvelu hallittavan asemat](storsimple-manage-volumes-u2.md). 

Päivityksen 2 avulla voit luoda StorSimple virtual tiedostoja, jotka hyödyntävät pienen viiveet suurempia ja tehokas Azure premium tallennustilan myöntämä. Saat lisätietoja StorSimple premium virtual laitteiden [käyttöönotto ja hallita StorSimple virtual laitteen Azure](storsimple-virtual-device-u2.md). Siirry Azure premium tallennustilan haluat lisätietoja [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](../storage/storage-premium-storage.md).

Lisäksi tallennusvälineiden hallinta-StorSimple tietojen suojaustoimintoja avulla voit tarvittaessa luoda ajoitettu varmuuskopiot ja tallentaa ne paikallisesti tai pilveen. Vaiheittainen tilannevedoksia, mikä tarkoittaa, että ne on luotu ja palauttaa nopeasti muodossa otetaan varmuuskopioita. Cloud tilannevedoksia voi olla hyvin tärkeä tietojen palauttaminen tilanteissa, koska korvaa toissijainen tallennusväline järjestelmien (kuten nauha varmuuskopio) ja voit palauttaa tietoja oman palvelinkeskukseen tai vaihtoehtoinen sivustoihin tarvittaessa.

![Video-kuvake](./media/storsimple-overview/video_icon.png) Katso video, joka Microsoft Azure StorSimple lyhyt esittely.

> [AZURE.VIDEO storsimple-hybrid-cloud-storage-solution]

## <a name="why-use-storsimple"></a>StorSimple käyttötarkoitus

Seuraavassa taulukossa on kuvattu joitakin keskeisiä toimintoja, joka tarjoaa Microsoft Azure StorSimple.

| Toiminto | Etu |
|---------|---------|
|Läpinäkyvä integrointi | Microsoft Azure StorSimple käyttää Linkitä tiedot tallennustilan tilat sisäänkirjautumistoiminnon iSCSI-protokollaa. Tämä varmistaa pilveen, milloin palvelinkeskukseen, tiedot tai sijaitsevia tulee näkyviin säilytettävä yhdessä paikassa.|
|Rajoitetun tallennustilan kustannukset|Microsoft Azure StorSimple Varaa riittävästi paikallinen tai pilvitallennustilaa täyttämään nykyisen ja laajentaa pilvitallennustilaa vain silloin, kun se on tarpeen. Se edelleen vähentää tallennustilaa ja kulujen poistamalla tarpeettomia samat tiedot (kopioinnin peruuttaminen)-versiot ja pakkaus käyttämällä.|
|Vaivaton tallennusvälineiden hallinta|Microsoft Azure StorSimple sisältää järjestelmän hallintatyökalut, joiden avulla voit määrittää ja hallita tallennettuja tietoja paikallisen, etäpalvelimeen ja pilveen. Lisäksi voit hallita varmuuskopiointi ja funktiot palauttaminen Microsoft Management Console (MMC)-laajennus. StorSimple sisältää erillisen, valinnainen-liittymän, joiden avulla voit laajentaa StorSimple hallinta ja tietojen suojauksen palvelujen sisältöön, joka on tallennettu SharePoint-palvelimiin. |
|Parannettu palauttaminen ja vaatimustenmukaisuus|Microsoft Azure StorSimple ei edellytä pidempi palauttaminen aika. Sen sijaan se palauttaa tiedot, se on tarpeen mukaan. Tämä tarkoittaa mahdollisimman vähän häiriöitä jatkaa Normaali toimintoja. Lisäksi voit määrittää käytännöt ja määritä varmuuskopion aikatauluja ja tietojen säilytys.
|Tietoja liikkuvuus|Tietojen lataaminen Microsoft Azure pilvipalveluihin niitä voi käyttää muiden sivustojen palauttaminen ja siirtoa varten. Lisäksi voit StorSimple virtual laitteiden määrittämiseksi näennäiskoneiden (VMs) Microsoft Azure StorSimple. VMs, voit käyttää testi-tai tallennetut tiedot virtual laitteet avulla.|
|Cloud palveluntarjoajia tuki |Ohjelmiston StorSimple 8000 sarjan Päivitä 1 tai uudempi versio tukee Amazon S3 RRS, HP ja OpenStack cloud services sekä Microsoft Azure. (Edelleen tarvitset Microsoft Azure-tallennustilan tilin laitteen hallintatoimintoihin.) Jos haluat lisätietoja, siirry [päivityksen 1.2 uudet ominaisuudet](storsimple-update1-release-notes.md#whats-new-in-update-12).|
|Liiketoiminnan jatkuvuus | Päivitys 1 tai uudempi versio tarjoaa uuden siirtotoiminto, jonka avulla StorSimple 5000 7000 sarjan käyttäjät voivat siirtää tietonsa StorSimple 8000 sarjan-laitteella.|
|Käytettävyys Azure Government-portaalissa | StorSimple päivitys 1 tai uudempi versio on saatavana Azure Government-portaalissa. Lisätietoja on artikkelissa [käyttöönotto paikallisen StorSimple laitteen Government-portaalissa](storsimple-deployment-walkthrough-gov.md).|
|Tietojen suojaaminen ja käytettävyys | StorSimple 8000 sarjan päivitys 1 ja sitä uudemmissa versioissa tukee vyöhykkeen tarpeettomat Storage (ZRS), lisäksi paikallisesti (LRS tarpeettomat tallennustilan) ja Geo tarpeettomat storage (GRS). Lisätietoja [Tässä artikkelissa Azure-redundancy tallennusasetukset](https://azure.microsoft.com/documentation/articles/storage-redundancy/) ZRS lisätietoja.|
|Tärkeiden sovellusten tuki | StorSimple päivitys 2 saat selville kuin paikallisesti kiinnitetyt haluamasi asemat. Tämä ominaisuus varmistaa, että tärkeät sovellukset vaatii tietojen ei tasoisen pilveen. Paikallisesti kiinnitetty asemat eivät ole cloud viiveet suurempia tai yhteysongelmat. Lisätietoja paikallisesti kiinnitetty tietomääristä artikkelissa [StorSimple hallintapalvelu hallittavan asemat](storsimple-manage-volumes-u2.md).|
|Pieni viive ja erinomainen suorituskyky | StorSimple päivitys 2 avulla voit luoda virtuaalisia laitteita, voit hyödyntää erinomainen suorituskyky pieni viive ominaisuuksia Azure premium tallennustilaa. Saat lisätietoja StorSimple premium virtual laitteiden [käyttöönotto ja hallita StorSimple virtual laitteen Azure](storsimple-virtual-device-u2.md).|

![videokuvake](./media/storsimple-overview/video_icon.png) Katso [Tämä video](https://www.youtube.com/watch?v=4MhJT5xrvQw&feature=youtu.be) yleiskatsaus StorSimple 8000 sarjan ominaisuuksiin ja etuihin.

## <a name="storsimple-components"></a>StorSimple osat

Microsoft Azure StorSimple ratkaisu sisältää seuraavat osat:

- **Microsoft Azure StorSimple laitteen** – paikallisen-hybrid tallennustilan taulukko, joka sisältää SSDs ja HDDs-sivustoa tarpeettomat ohjaimet ja automaattinen automaattisesti ominaisuuksia. Tallennustilan tiering, markkinoille tällä hetkellä käytetyt (tai kuuma) paikallisen storage (-laitetta tai paikalliseen palvelimet)-siirrettäessä pilveen vähemmän usein käytettyjen tietojen hallinta ohjaimet.
- **StorSimple virtual laitteen** – tunnetaan myös nimellä StorSimple Virtual laitteen tämä on StorSimple laitteella, joka kopioi arkkitehtuuri ohjelmistoversiota ja fyysinen hybrid muistivälineeseen useimmat ominaisuudet. StorSimple virtual laite toimii Azure virtuaalikoneen yhtä haaraa. Premium virtual laitteet, joka hyödyntää Azure premium tallennustilaa, ovat käytettävissä päivityksen 2 ja uudempi versio.
- **StorSimple hallinta** – perinteinen Azure-portaalin, jolla voit hallita StorSimple laitteen tai StorSimple virtual laitteen yksittäisen web-liittymän tunniste. StorSimple hallinta-palvelun avulla voit luoda ja palveluiden hallinta tarkasteleminen ja laitteiden hallinta, ilmoitusten tarkasteleminen, asemat, ja tarkastella ja hallita varmuuskopion käytännöt ja varmuuskopion luettelon.
- **Windows PowerShell StorSimple** – käyttöliittymä, joiden avulla voit hallita StorSimple laite. Windows PowerShell StorSimple on ominaisuuksia, jotka voidaan rekisteröidä StorSimple laitteen, määrittäminen verkon-liittymän laitteen, asenna päivitykset tietyntyyppisiä, vianmääritys laitteen muodostamalla yhteyden tuki-istunnon ja muuttaa laitteen tila. Voit käyttää Windows PowerShellin StorSimple yhdistämällä serial konsoliin tai käyttämällä Windows PowerShellin Etäpalvelujen.
- **Azure PowerShell StorSimple cmdlet-komennot** – Windows PowerShellin cmdlet-komennot, jotka mahdollistavat palvelun tason ja siirron automatisointiin komentoriviltä kokoelma. Lisätietoja StorSimple Azure PowerShellin cmdlet-komennot Siirry [cmdlet-viittaus](https://msdn.microsoft.com/library/dn920427.aspx).
- **StorSimple tilannevedoksen hallinta** – MMC-laajennus, joka käyttää aseman ryhmät ja Windowsin tilannevedospalvelu sovelluksen yhdenmukaisia varmuuskopiot. Lisäksi voit luoda varmuuskopion aikatauluja ja Kloonaa tai palauttaa tietomääristä StorSimple tilannevedoksen hallinta. 
- **SharePoint-sovittimen StorSimple** – työkalua, joka ulottuu läpinäkyvä Microsoft Azure StorSimple tallennustilan ja tietojen suojauksen SharePoint Server-klustereihin muodostettaessa tallennustilan StorSimple näytettävä ja helpommin hallittaviin SharePointin keskitetty hallinta-portaalista.

Seuraavassa kuvassa on Microsoft Azure StorSimple arkkitehtuuri ja -osien ylätason näkymän.

![StorSimple arkkitehtuuri](./media/storsimple-overview/overview-big-picture.png)

Seuraavissa osissa kuvataan tarkemmin näistä osat sekä kerrotaan, miten ratkaisun järjestää tiedot, varaa tallennustilan ja helpottaa tallennusvälineiden hallinta- ja tietosuojan. Viimeinen osa sisältää määrityksiä tärkeistä termeistä ja käsitteitä, jotka liittyvät StorSimple osat ja niiden hallinta.

## <a name="storsimple-device"></a>StorSimple laite

Microsoft Azure StorSimple laite on paikallisen-hybrid tallennustilan matriisi, joka on ensisijainen tallennustilan ja sen tallennettuja tietoja voidaan käyttää iSCSI. Hallitsee pilvitallennustilaa tietoliikenteen ja varmistaa suojaus ja luottamuksellisuus kaikki tiedot, jotka on tallennettu Microsoft Azure StorSimple-ratkaisussa.

StorSimple laitteen sisältää SSD ja kiintolevyaseman levyasemien HDDs sekä tuki klusteroinnin ja automaattinen automaattisesti. Se sisältää jaetun suoritin jaettua tallennustilan ja kaksi peilattu ohjaimet. Kunkin ohjauskoneen mahdollistaa seuraavat:

- Yhteyden isäntätietokoneen
- Enintään kuusi lähiverkossa (LAN) muodostaa verkkoportit
- Laitteiston seuranta
- Muut pysyvä RAM-muistia (NVRAM), joka säilyttää tiedot, vaikka power keskeyttää
- Klusterin-aware päivittäminen hallittavan ohjelmistopäivitykset automaattisesti klusterin palvelimissa, niin, että päivitysten on vähän tai ei vaikuta palvelun saatavuus
- Klusterin palvelun, joka toimii, kuten taustatietokantaan klusterin tarjoamalla suuren käytettävyyden ja pienentäminen haitalliset tehosteet, joka saattaa ilmetä, jos Kiintolevy tai Suoritettaessa epäonnistuu tai on offline-tilassa

Vain yhden ohjauskoneen on aktiivinen milloin tahansa samanaikaisesti. Jos aktiivinen ohjauskoneen epäonnistuu, toisen ohjaimen aktivoituu automaattisesti. 

Lisätietoja Valitse [StorSimple laitteet ja tila](storsimple-monitor-hardware-status.md).

## <a name="storsimple-virtual-device"></a>StorSimple virtual laite

StorSimple avulla voit luoda virtuaalisia laitteella, joka kopioi arkkitehtuuri ja laitteen fyysinen hybrid tallennustilan ominaisuudet. Azure virtuaalikoneen yhtä haaraa suoritetaan StorSimple virtual laitteen (tunnetaan myös nimellä StorSimple Virtual laitteen). (Virtual laitteen voi luoda vain Azure virtuaalikoneen. Ei voi luoda StorSimple laitteen tai paikallisen palvelimelle.) 

Virtuaalinen laitteessa on seuraavat ominaisuudet:

- Se toimii kuten fyysinen laitteen ja tarjota iSCSI-liittymän näennäiskoneiden pilveen. 
- Voit luoda rajattomasti virtual laitteiden pilveen ja ottaa ne käyttöön ja poistaminen käytöstä tarpeen mukaan. 
- Auttaa simuloida paikallisten ympäristöjen palauttaminen, kehittäminen ja testaa skenaariot ja varmuuskopioiden hakeminen Kohdetason apua. 

Päivitä 2 ja uudemmissa versioissa StorSimple virtual laite on käytettävissä kaksi mallia: 8010 laitteen (aiemmalta nimeltään 1100-malli) ja 8020 laite. 8010 laitteessa on suurin kapasiteetin 30 teratavua. 8020 laitteella, joka hyödyntää Azure premium tallennustila on enintään 64 teratavua kapasiteetti. (Paikallisen tasoa Azure premium tallennustilan tallentaa tiedot-SSD olisi vakio tallennustilan tiedot tallennetaan HDDs.) Huomaa, että sinulla on Azure premium tallennustilan tili, jota käytetään premium-tallennustilan. Lisätietoja premium tallennustilan Siirry [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](../storage/storage-premium-storage.md).

Lisätietoja StorSimple virtual laitteen Siirry [käyttöönotto ja hallita StorSimple virtual laitteen Azure](storsimple-virtual-device-u2.md).

## <a name="storsimple-manager-service"></a>StorSimple hallinta

Microsoft Azure StorSimple on verkkopohjainen käyttöliittymä (StorSimple-hallintapalvelu), jonka avulla voit hallita palvelinkeskuksen ja tallennustilaa cloud keskitetysti. StorSimple hallinta-palvelun avulla voit suorittaa seuraavia tehtäviä:

- Määritä StorSimple laitteiden järjestelmäasetuksia.
- Määritä ja StorSimple laitteet tietoturva-asetusten hallinta.
- Määritä cloud tunnistetiedot ja ominaisuudet.
- Määritä ja hallitse tietomääristä palvelimessa.
- Määritä aseman ryhmät.
- Varmuuskopiointi ja palauttaminen tiedot.
- Valvoa suorituskykyä.
- Tarkista järjestelmäasetuksista ja mahdolliset ongelmat.

Voit käyttää StorSimple hallintapalvelu kaikki hallintatehtävien lukuun ottamatta, jotka edellyttävät järjestelmän ajan, esimerkiksi ensimmäisen määrityskerran ja päivitysten asentaminen alaspäin.

Lisätietoja on Katso [StorSimple hallintapalvelu ammattimainen StorSimple laitteen](storsimple-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>Windows PowerShell StorSimple

Windows PowerShell StorSimple on käyttöliittymä, joiden avulla voit luoda ja hallita Microsoft Azure StorSimple-palvelun ja määrittäminen ja seurata StorSimple laitteet. Windows PowerShell-pohjainen-käyttöliittymä, jossa on erillinen cmdlet-komennot hallinta StorSimple laite on. Windows PowerShell StorSimple sisältää toimintoja, joiden avulla voit:

- Rekisteröi laite.
- Määrittää verkkoliittymän laitteessa.
- Asenna päivitykset tietyntyyppisiä.
- Vianmääritys laitteen muodostamalla yhteyden tuki-istunnon.
- Voit muuttaa laitteen tila.

Voit käyttää Windows PowerShellin StorSimple serial konsolin (Valitse laite suoraan yhdistetty isäntätietokoneen) tai etäyhteyden välityksellä Etäpalvelujen Windows PowerShellin avulla. Huomaa, että jotkin Windows PowerShell StorSimple tehtäviä, kuten alkuperäisen laitteen rekisteröinti voi tehdä vain serial konsolissa. 

Saat lisätietoja Siirry [StorSimple laitteen hallintaan Windows PowerShellin käyttäminen](storsimple-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorSimple cmdlet-komennot

Azure PowerShell StorSimple cmdlet-komennot ovat kokoelma Windows PowerShellin cmdlet-komennot, jotka mahdollistavat palvelun tason ja siirron automatisointiin komentoriviltä. Lisätietoja StorSimple Azure PowerShellin cmdlet-komennot Siirry [cmdlet-viittaus](https://msdn.microsoft.com/library/dn920427.aspx).

## <a name="storsimple-snapshot-manager"></a>StorSimple tilannevedoksen hallinta

StorSimple tilannevedoksen hallinta on Microsoft Management Console (MMC)-laajennus, jonka avulla voit luoda yhtenäisiä paikallinen ajankohta varmuuskopiot ja cloud tiedot. Laajennuksen suorittaa Windows Server-pohjainen isännän. Voit käyttää StorSimple tilannevedoksen hallinta:

- Määrittää, varmuuskopioida ja poistaa asemat.
- Määritä aseman ryhmät, jos haluat varmistaa, varmuuskopioinut tiedot ovat sovelluksen yhdenmukaisia.
- Varmuuskopion käytäntöjen hallinta niin, että tietojen päivityksen yrityskertoja aikataulun varmuuskopioita ja tallennettu määritettyyn sijaintiin (paikallisesti tai pilvipalveluun).
- Palauttaa asemat ja yksittäiset tiedostot.

Varmuuskopioiden välitetyt kuin tilannevedoksia, joka tallentaa vain tehdyt muutokset, koska viimeisen tilannevedoksen on tehty ja vaatia paljon pienempään tilaan kuin koko varmuuskopiot. Voit luoda varmuuskopion aikatauluja tai ottaa heti varmuuskopioiden tarpeen mukaan. Lisäksi voit käyttää StorSimple tilannevedoksen hallinta muodostaa säilytyskäytännöt ohjaavat, kuinka monta tilannevedoksia tallennetaan. Jos haluat myöhemmin palauttaa tietoja varmuuskopion, StorSimple tilannevedoksen hallinta-toiminnon avulla voit valita luettelon paikallisten tai cloud tilannevedoksia. 

Huono sattuessa tai jos haluat palauttaa tietoja muusta syystä, StorSimple tilannevedoksen Manager palauttaa sen vaiheittainen, se on tarpeen mukaan. Tietojen palauttaminen ei edellytä Sammuta koko järjestelmästä palauttaa tiedoston, korvaa laitteiden tai siirtäminen toiseen sivustoon toimintoja.

Jos haluat lisätietoja, siirry [Mikä on StorSimple tilannevedoksen hallinta?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>StorSimple sovittimen SharePoint

Microsoft Azure StorSimple sisältää StorSimple sovittimen SharePoint-valinnainen osa, joka ulottuu läpinäkyvä StorSimple tallennustilan ja tietojen suojauksen suojausominaisuuksia, jotka SharePoint server-klustereihin. Sovittimen toimii Remote Blob Storage (RBS)-palvelu ja SQL Server RBS-toimintoa, voit siirtää BLOB Microsoft Azure StorSimple järjestelmä varmuuskopioida palvelimeen. Microsoft Azure StorSimple tallentaa BLOB-tietojen paikallisesti tai pilvipalvelussa, käyttö perusteella.

SharePointin StorSimple-sovittimen hallitaan SharePointin keskitetty hallinta-portaalissa. Näin ollen SharePoint hallinta säilyy keskitetty ja kaikki tallennustilan ilmeisesti SharePoint-klusterin.

Jos haluat lisätietoja, siirry [StorSimple sovittimen SharePoint](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Tallennustilan hallinta tekniikat
 
Lisäksi erillinen StorSimple laite, virtual laitteen ja muita osia Microsoft Azure StorSimple käyttää seuraavia ohjelmistotekniikoita tiedot nopeasti ja vähentää tallennustilan kulutus:

- [Automaattinen tallennustilan tiering](#automatic-storage-tiering) 
- [Ohuen valmistelu](#thin-provisioning) 
- [Kopioinnin peruuttaminen ja pakkaaminen](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automaattinen tallennustilan tiering

Microsoft Azure StorSimple järjestää automaattisesti tietojen perusteella nykyinen käyttö, ikä ja muiden tietojen suhde looginen tasoa. Tiedot, jotka ovat Aktiivisimmat tallennetaan paikallisesti, kun vähemmän aktiivinen ja passiiviset tiedot siirretään automaattisesti pilveen. Seuraavassa kaaviossa on kuvattu tämän tallennustilan menetelmän.
 
![StorSimple tallennustilan tasoa](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Voit ottaa nopeasti käyttöön StorSimple tallentaa hyvin aktiivinen tiedot (kuuma tiedot) SSD StorSimple-laitteessa. Tietoja, joita käytetään toisinaan tallennetaan (kiinnostunut tiedot)-HDDs laitteen tai sen palvelinkeskuksen-palvelimia. Se siirtyy käytöstä tietojen varmuuskopioidut tiedot, ja tietoja säilytetään arkistointia tai vaatimustenmukaisuus pilvessä varten. 

>[AZURE.NOTE] Päivitä 2 tai uudempi versio voit määrittää aseman kuin paikallisesti kiinnitetty, jolloin tiedot säilyvät paikallisen laitteen ja ei ole tasoisen pilveen. 

StorSimple säätää ja järjestää tietoja ja tallennustilaa palvelujen käyttöä käyttötavat muuttaminen. Esimerkiksi tietoja voi olla pienempi active ajan kuluessa. Kun muuttuu vähitellen vähemmän aktiivinen, se siirretään SSD HDDs ja sitten pilveen. Jos, että samat tiedot aktivoituu uudelleen, se siirretään takaisin tallennustilan laitteen.

Tallennustilan tiering prosessi tapahtuu seuraavasti:

1. Järjestelmänvalvoja määrittää Microsoft Azure cloud tallennustilan tilin.
2. Järjestelmänvalvoja käyttää serial konsolin ja StorSimple hallintapalvelu (käynnissä Azure perinteinen portaalissa) Määritä laitteen ja tiedostonimi, palvelimen luominen asemat ja tietojen suojauksen käytännöt. Paikallisen koneet (esimerkiksi tiedostopalvelimet) käyttää StorSimple laitteen Internet pieni tietokoneen järjestelmän käyttöliittymässä (iSCSI).
3. Aluksi StorSimple tallentaa tiedot nopeasti Suoritettaessa ylätasolla laitteen.
4. Suoritettaessa tason lähestyessä kapasiteetin StorSimple deduplicates ja pakkaa vanhimmasta tietolohkot ja siirtää ne Kiintolevylle taso.
5. Kiintolevy taso tavoista, kapasiteetin StorSimple salaa vanhimmasta tietolohkot ja lähettää niitä turvallisesti Microsoft Azure-tallennustilan tilin HTTPS.
6. Microsoft Azure Luo useita replikoita tietojen sen palvelinkeskuksen ja remote palvelinkeskuksen varmistetaan, että tiedot voidaan palauttaa huono sattuessa. 
7. Kun tiedostopalvelin vaatii pilveen tallennettuja tietoja, StorSimple palauttaa saumattomasti ja tallentaa kopion Suoritettaessa ylätasolla StorSimple laitteen.

### <a name="thin-provisioning"></a>Ohuen valmistelu

Ohuen valmistelu on virtualization tekniikka, jossa on suurempi kuin fyysinen resurssien käytettävissä. Sen sijaan, että varaaminen riittävästi tallennustilan etukäteen StorSimple käyttää ohuen valmistelu varata samalla tarpeeksi tilaa nykyisen ehtojen mukaan. Pilvitallennustilaa joustavasti laatu helpottaa tätä tapaa, koska StorSimple voit suurentaa tai pienentää pilvitallennustilaa täyttämään muuttaminen. 

>[AZURE.NOTE] Paikallisesti kiinnitetty tietomääristä ole levinneet valmisteltu. Vain paikalliseen asemaan varattu tallennustila on valmisteltu kokonaisuudessaan, äänenvoimakkuuden luomisen yhteydessä.

### <a name="deduplication-and-compression"></a>Kopioinnin peruuttaminen ja pakkaaminen

Microsoft Azure StorSimple käyttää kopioinnin peruuttaminen ja tietojen pakkaus vähentää Lisää tallennustilaa.

Kopioinnin peruuttaminen vähentää yleinen poistamalla redundanssin tallennetut tietojoukon tallennetut tiedot. Kun tiedot muuttuvat, StorSimple ohittaa muuttumattomina tiedot ja sieppaa vain muutokset. Lisäksi StorSimple vähentää tallennettujen tietojen tunnistaminen ja poistamalla tarpeettomia tietoja. 

>[AZURE.NOTE] Tietoja paikallisesti kiinnitetty tietomääristä ei deduplicated tai pakata. Kuitenkin paikallisesti kiinnitetty tietomääristä varmuuskopiot deduplicated ja pakata.

## <a name="storsimple-workload-summary"></a>StorSimple kuormituksen yhteenveto

Alla on taulukkomuotoinen yhteenveto tuettujen StorSimple toiminnoista.

| Skenaario                  | Kuormituksen                | Tuettu |  Rajoitukset                                  | Versio              |
|---------------------------|-------------------------|-----------|------------------------------------------------|----------------------|
| Yhteiskäyttö             | Tiedostojen jakaminen            | Kyllä       |                                                | Kaikki versiot         |
| Yhteiskäyttö             | Hajautettu tiedostojen jakaminen| Kyllä       |                                                | Kaikki versiot         |
| Yhteiskäyttö             | SharePoint              | Kyllä, jos *      |Tukee vain paikallisesti kiinnitetty tietomääristä      | Päivitä 2 ja uudempi versio   |
| Arkistointi                  | Yksinkertaisen tiedoston arkistointi   | Kyllä       |                                                | Kaikki versiot         |
| Virtualisointi            | Näennäiskoneiden        | Kyllä, jos *      |Tukee vain paikallisesti kiinnitetty tietomääristä      | Päivitä 2 ja uudempi versio   |
| Tietokannan                  | SQL                     | Kyllä, jos *      |Tukee vain paikallisesti kiinnitetty tietomääristä      | Päivitä 2 ja uudempi versio   |
| Videon valvonta        | Videon valvonta      | Kyllä, jos *       |Kun StorSimple laite on varattu vain tämä työmäärää| Päivitä 2 ja uudempi versio   |
| Varmuuskopiointi                    | Ensisijainen kohde varmuuskopiointi | Kyllä, jos *      |Kun StorSimple laite on varattu vain tämä työmäärää| Päivitä 3 ja uudempi versio |
| Varmuuskopiointi                    | Toissijainen kohteen varmuuskopioinnin | Kyllä, jos *      |Kun StorSimple laite on varattu vain tämä työmäärää| Päivitä 3 ja uudempi versio |

*Kyllä & #42; -Käytetään ratkaisun ohjeita ja rajoituksia.*

StorSimple 8000 sarjan laitteiden ei tue seuraavia toiminnoista. Jos käytössä StorSimple näistä toiminnoista johtaa ei tueta määritys.

-  Lääketieteellinen imaging
-  Exchange
-  VDI
-  Oracle
-  SAP: IN
-  Big datasta
-  Sisällön jakelu
-  SCSI levyltä

Seuraavassa on luettelo StorSimple tuettu infrastruktuurin osista. 

| Skenaario | Kuormituksen      | Tuettu |  Rajoitukset                                 | Versio      |
|----------|---------------|-----------|-----------------------------------------------|--------------|
| Yleiset  | Reitin Express | Kyllä       |                                               |Kaikki versiot |
| Yleiset  | DataCore FC   | Kyllä, jos *       |Tukee DataCore SANsymphony            | Kaikki versiot |
| Yleiset  | DFSR          | Kyllä, jos *      |Tukee vain paikallisesti kiinnitetty tietomääristä     | Kaikki versiot |
| Yleiset  | Indeksointi      | Kyllä, jos *       |Porrastettu levyasemien tuetaan vain metatietojen indeksoinnin (ei tietoja).<br>Paikallisesti kiinnitetty levyasemien valmis indeksoinnin tuetaan.| Kaikki versiot |
| Yleiset  | Anti-virus    | Kyllä, jos *       |Porrastettu levyasemien tuetaan vain tarkistus, valitse Avaa ja sulje.<br> Paikallisesti kiinnitetty levyasemien Perusteellisessa tuetaan.| Kaikki versiot |

*Kyllä & #42; -Käytetään ratkaisun ohjeita ja rajoituksia.*

## <a name="storsimple-terminology"></a>StorSimple sanasto 

Ennen kuin otat Microsoft Azure StorSimple-ratkaisun, on suositeltavaa, että luet seuraavat termit ja määritykset.

### <a name="key-terms-and-definitions"></a>Avaimen termit ja määritykset

| Termin (lyhenne tai lyhenne) | Kuvaus |
| ------------------------------ | ---------------- |
| Accessin ohjausobjektin tietue (ACR)    | Tietue on liitetty asema, joka määrittää, mitkä isännät voit muodostaa yhteyden Microsoft Azure StorSimple laitteen. Määrittäminen perustuu iSCSI koko nimi (IQN) isäntien (sisältyvät ACR), joka muodostaa yhteyden StorSimple laitteen.|
| AES-256                        | 256-bittinen salaus AES (Advanced Standard)-algoritmin salaamiseen tietojen sellaisena kuin se siirtää, ja pilvestä. |
| kohdistus kokoa (Australia)     | Pienin määrä, joka voidaan varata säilyttämään tiedostoa Windowsin levytilaa tiedoston systems. Jos tiedostokoko ei ole klusteri kerrannaisen, ylimääräistä tilaa on käytettävä pitoon (ylöspäin klusteri seuraava kerrannaiseen) tiedoston kadonneen tilaa jakautuminen ja kiintolevyn tuloksena. <br>Suositeltu Australia Azure StorSimple levyasemien on 64 Kilotavua, koska se toimii hyvin kopioinnin peruuttaminen algoritmeista.|
| automaattisen tallennus tiering      | Siirtyvät automaattisesti vähemmän aktiivinen SSD HDDs ja sitten pilveen taso ja sitten hallinnan kaikki tallennustilaa keskitetyn käyttöliittymä ottaminen käyttöön.|
| varmuuskopiotiedoston | Kokoelma varmuuskopioinnin sekä liittyvät yleensä sovelluksen tyyppi, jota käytettiin mukaan. StorSimple hallintapalvelu Käyttöliittymän varmuuskopioinnin luettelo-sivulla näytetään tämän sivustokokoelman.|
| varmuuskopiotiedoston tiedosto             | Tiedosto on luettelo käytettävissä tilannevedoksia tallennettujen Varmuuskopioi tietokanta StorSimple tilannevedoksen hallinta. |
| varmuuskopion käytäntö                   | Mitä tietomääristä varmuuskopioinnin tyypin ja aikataulu, jonka avulla voit luoda varmuuskopiot ennalta määritetyn aikataulun.|
| Binaarinen suurista (BLOB)    | Yhden kohteen tietokannan hallintajärjestelmän tallennettujen binaaritietoja kokoelma. BLOB ovat yleensä kuvat- sekä ääni- ja muut multimedia objektit, mutta joskus binaarinen suoritettavat koodit tallennetaan BLOB.|
| Todennus Handshake Authentication Protocol (CHAP) | Protokolla todennetaan vertaisjärjestelmä yhteyden, salasana tai salaisuus jakamisen peer perusteella. CHAP voi olla yksisuuntainen tai keskinäistä. Yksisuuntainen CHAP, jossa kohde todentaa käynnistäjä. Yhteinen CHAP edellyttää, että kohde todennetaan käynnistäjä ja käynnistäjä todentaa kohde. | 
| Kloonaa                          | Tilavuus kopion. |
|Cloud kuin taso (CaaT)          | Cloud tallennustilan integroitu sisällä tallennustilan arkkitehtuuri taso kuin niin, että kaikki tallennustilan näkyy yksi yritysverkkoa tallennustilan osana.|
| cloud palveluntarjoaja   | Cloud services tietojenkäsittely tarjoaja.|
| cloud tilannevedos                 | Äänenvoimakkuuden tiedot, jotka on tallennettu pilvipalveluun ajankohta kopio. Cloud tilannevedoksen vastaa tilannevedoksen replikoida eri, sähköntuotannosta tallennustilan järjestelmään. Cloud tilannevedoksia ovat erityisen hyödyllisiä tietojen palauttaminen tilanteissa.|
| cloud tallennustilan salausavain   | Salasanan tai käyttää StorSimple laitteen lähettämä laitteen pilveen salatut tiedot avain.|
| klusterin tukeva päivittäminen         | Hallinta ohjelmistopäivitykset automaattisesti klusterin palvelimissa, niin, että päivitysten on vähän tai ei vaikuta palvelun saatavuus.|
| Tietopolkua                       | Kokoelma toiminnallisten yksiköiden, joka suorittaa väliset yhdistettyjen tietojen käsittely.|
| aktivoinnin poistaminen                     | Pysyvä toiminto, joka katkaisee yhteyden StorSimple laitteen ja liittyvän pilvipalvelussa välillä. Cloud tilannevedoksia laitteen säilyvät jälkeen prosessia ja voidaan monistaa tai käyttää palauttaminen.|
| Levyn peilaus                 | Erillisen kiintolevyn looginen levyasemien replikoinnin ohjaa reaaliajassa varmistetaan jatkuva.|
| dynaamisen levyn peilaus         | Loogisen levyasemien dynaamisiksi replikoinnin.|
| dynaamisiksi                  | Levyn aseman muoto, jossa käytetään looginen levyn Manager (LDM) voivat tallentaa ja hallita useita fyysinen levyille tiedot. Dynaamisiksi voi suurentaa antamaan Lisää tilaa.|
| Laajennettu Bunch, levyjen (EBOD) kehys | Toissijainen kehyksen Microsoft Azure StorSimple laitteen, joka sisältää erittäin kiintolevylle levyjen lisätallennustilaa.|
| FAT valmistelu               | Tavalliseen tallennustilan valmistelu mitä tallennustilaan tilaa on varattu perusteella odotettavissa tarpeiden (ja on yleensä lisäksi nykyistä tarvetta). Katso myös *ohuen valmistelu*.|
| kiintolevy (Kiintolevy)          | Asema, joka käyttää pyörivien platters tietojen tallentamiseen.|
| Hybrid pilvitallennustilaa.           | Tallennustilan arkkitehtuuri, joka käyttää paikallisen ja sähköntuotannosta resurssit, kuten pilvitallennustilaa.|
| Internet Small Computer System Interface (iSCSI) | Internet Protocol IP-pohjainen tallennustilan verkko standardin linkittäminen tietojen tallennustilan laitteiden tai tiloja.|
| iSCSI-käynnistäjä                 | Ohjelmisto-osa, joka mahdollistaa ulkoisten iSCSI perustuva tallennustilan verkkoon käyttöjärjestelmässä isäntätietokoneen.|
| iSCSI koko nimi (IQN)      | Yksilöivä nimi, joka määrittää iSCSI-kohteen tai käynnistäjä.|
| iSCSI-kohde                    | Ohjelmiston osa, joka tarjoaa keskitetyn iSCSI levyn alijärjestelmien tallennustilan alueen verkoissa.|
| Live arkistointia                  | Tallennustilan lähestymistapa, jossa arkistointia tiedot ovat käytettävissä (se ei ole tallennettu Työmaan ulkopuolella nauha, kuten) aina. Microsoft Azure StorSimple käyttää live arkistointi.|
|paikallisesti kiinnitetty aseman | asema, joka sijaitsee laitteessa ja ei koskaan tasoisen pilveen. |
| Paikallisen tilannevedoksen                  | Äänenvoimakkuuden tiedot, jotka on tallennettu Microsoft Azure StorSimple laitteeseen ajankohta kopio.|
| Microsoft Azure StorSimple      | Tehokas ratkaisu, joka koostuu palvelinkeskuksen tallennustilan laitteen ja ohjelmisto, jonka avulla IT-organisaatiot voivat hyödyntää pilvitallennustilaa, ikään kuin se olisi palvelinkeskuksen tallennustilan. StorSimple helpottaa tietojen suojaaminen ja tietojen hallinta ja pienentää kustannuksia. Ratkaisu kokoaa ensisijainen tallennustilan, arkistointi tai varmuuskopiointi tietojen palauttaminen (DR) pilvipalveluun saumattomasti kautta. Yhdistämällä SAN säilytys- ja cloud tietojen hallinta yritysluokan ympäristössä StorSimple laitteiden käyttöön nopeutta, yksinkertaisuuden ja luotettavuutta kaikki tallennustilan liittyvät tarpeisiin.|
| Power ja jäähdytys moduuli (PCM)  | Laitteiston osat StorSimple laitteen koostuu power tarvikkeiden ja puhaltimen, joten nimi Power ja jäähdytys moduuli. Laitteen ensisijainen kehyksen on kaksi 764W PCMs EBOD kehys on kaksi 580W PCMs.|
| Ensisijainen kehys               | Tärkeimmät kehyksen StorSimple laitteen, joka sisältää sovelluksen platform-ohjaimet.|
| palautus aika tavoitteen (RTO)   | Aika, joka on käytetty loppuun ennen Liiketoimintaprosessi tai järjestelmän enimmäismäärän palautetaan täysin huono jälkeen.| 
|sarja liitetty SCSI (SAS)       | Minkätyyppinen kiintolevy (Kiintolevy).|
| palvelun tiedot salausavain     | Saataville laitteeseen StorSimple avainta, joka rekisteröi StorSimple hallintapalvelu. StorSimple hallintapalvelu ja laitteen välillä määritystietoja salataan julkisella avaimella, ja sitten voit purkaa vain käyttämällä yksityinen avain laitteessa. Palvelun tiedot salausavaimen avulla palvelun hankkia tämän salauksen yksityinen avain.|
| palvelun rekisteröinti avain        | Avainta, joka auttaa niin, että se näkyy edelleen hallinnan toiminnot Azure perinteinen-portaalissa voit rekisteröidä StorSimple laitteen StorSimple hallintapalvelu.|
| Pieni tietokoneen järjestelmän Interface (SCSI) | Tietokoneiden yhdistäminen ja tietoja siirtämällä fyysisesti joukko.|
| Tasainen tilan asemaa Suoritettaessa         | Levy, joka sisältää liikkuvat osat; esimerkiksi muistitikkuun.|
| tallennustilan tilin                 | Joukko linkitetty annetun cloud-palveluntarjoajan tallennustilan tilin tunnistetiedoilla.| 
| StorSimple sovittimen SharePoint| Microsoft Azure StorSimple osa, joka ulottuu läpinäkyvä SharePoint server-klustereihin StorSimple tallennustilan ja tietojen suojauksen.|
| StorSimple hallinta      | Perinteinen Azure-portaalin, jonka avulla voit hallita Azure StorSimple paikallisen ja virtual laitteet laajennus.|
| StorSimple tilannevedoksen hallinta     | Microsoft Management Console (MMC) laajennuksen varmuuskopiointi ja palauttaminen Microsoft Azure StorSimple toimintojen hallintaan.|
| Ota varmuuskopiointi                     | Toiminto, jonka avulla käyttäjä voi tehdä aseman vuorovaikutteinen varmuuskopion. Manuaalinen varmuuskopiointi aseman verrattuna automaattisen varmuuskopioinnin, määritetty käytännöllä ottaen huomioon on vaihtoehtoinen tapa on.|
| ohuen valmistelu               | Menetelmä optimointi, johon käytettävissä olevan tallennustilan käytetään tallennustilan järjestelmien tehokkuutta. Valitse ohuen valmistelu tallennustilaan on varattu perusteella vähimmäisvaatimus kunkin käyttäjän ajankohtana useiden käyttäjien kesken. Katso myös *fat valmistelu*.|
| tiering | Tietojen perusteella nykyinen käyttö, ikä ja muiden tietojen suhde loogisesti ryhmiteltyjä järjestämisestä. StorSimple järjestää automaattisesti tietojen tasoa.   |
| avauksen ja vaihdon                          | Loogisen tallennustilan alueet asemat muodossa. StorSimple asemat vastaavat liitetty isäntä, mukaan lukien havaitsi iSCSI ja StorSimple laitteen asemat.|
 | avauksen ja vaihdon säilö                | Ryhmittely ja asemat niitä koskevat asetukset. Kaikki asemat StorSimple laitteesi on ryhmitelty aseman säilöjen. Säilön ääniasetusten esimerkiksi tallennustilan tilit, salausasetukset cloud kanssa liittyvät salausavaimet lähetetyt tiedot ja kaistanleveyttä toimille henkilöihin liittyvät pilveen.|
| Äänenvoimakkuus-ryhmä                    | StorSimple tilannevedoksen hallinta aseman ryhmä on määritetty helpottamiseksi varmuuskopion käsittely tietomääristä kokoelma.|
| Tilannevedospalvelu (VSS)| Windows Server käyttöjärjestelmän palvelu, joka helpottaa sovelluksen yhdenmukaisuuden toimittamalla VSS-aware järjestämiseen vaiheittainen tilannevedoksia luonti-sovellusten kanssa. VSS varmistaa, että sovellukset ovat tilapäisesti käytöstä, kun tilannevedoksia otetaan.|
| Windows PowerShell StorSimple | Windows PowerShell-pohjainen käyttöliittymä ja hallita StorSimple laitetta. Säilyttäen joitakin Windows PowerShellin perusominaisuudet liittymän on muita erillinen cmdlet-komennot, jotka ovat suunnattu hallinta StorSimple laitteen.|


## <a name="next-steps"></a>Seuraavat vaiheet

Lue lisää [StorSimple tietoturvasta](storsimple-security.md).




 

 
