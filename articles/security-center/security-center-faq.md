<properties
   pageTitle="Usein kysyttyjä kysymyksiä Azure Tietoturvakeskus | Microsoft Azure"
   description="Nämä usein kysytyt kysymykset vastauksia kysymyksiin Azure Tietoturvakeskuksessa."
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
   ms.date="10/27/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-frequently-asked-questions-faq"></a>Usein kysyttyjä kysymyksiä Azure Tietoturvakeskus

Nämä usein kysytyt kysymykset vastauksia kysymyksiin Azure Tietoturvakeskus-palvelu, joka auttaa estämään, haku- ja vastata uhkien näkyvyyden kyselyjä ja hallita Microsoft Azure-resurssien suojaus.

## <a name="general-questions"></a>Yleisiä kysymyksiä

### <a name="what-is-azure-security-center"></a>Mikä on Azure Tietoturvakeskus?
Azure Tietoturvakeskus auttaa estämään, haku- ja vastata uhkien näkyvyyden kyselyjä ja hallita Azure resurssien suojaus. Siinä on integroitu suojaus seuranta ja käytäntöjen hallinta tilausten välillä, auttaa tunnistamaan uhkien, jotka muuten huomaamatta ja laaja valikoima security-ratkaisujen mediatyökaluja toimii.

### <a name="how-do-i-get-azure-security-center"></a>Miten voin hankkia Azure Tietoturvakeskus?
Azure Tietoturvakeskuksessa on käytössä Microsoft Azure-tilaus ja käyttää [Azure portal](https://azure.microsoft.com/features/azure-portal/). ([Kirjautuminen portaaliin](https://portal.azure.com), valitse **Selaa**ja siirry **Tietoturvakeskus**).  

## <a name="billing"></a>Laskutus

### <a name="how-does-billing-work-for-azure-security-center"></a>Miten Azure Tietoturvakeskus laskutuksen työmääriin?
Tietoturvakeskus tarjotaan kaksi tasoa: maksuttomia ja vakio.

Vapaa tason avulla voit määrittää suojauskäytäntöjä ja vastaanottaa suojausvaroitusten tapaukset ja suositukset, joka opastaa määrittäminen tarvittavat ohjausobjektit. Vapaa tason kanssa voit valvoa Azure resurssit ja kumppaniratkaisuihin Azure tilauksen integroitu suojaustilan.

Vakio taso sisältää vapaa taso ominaisuuksia sekä advanced tunnistuksia: uhkien liiketoimintatietojen, käytöspiirteen analyysin tai kaatumisen analysis poikkeavuuksista tunnistus. Vakio taso 90 päivän maksuton kokeiluversio on käytettävissä. Jos haluat päivittää, valitse hinnat taso [suojauskäytäntö](security-center-policies.md#setting-security-policies-for-subscriptions). Katso lisätietoja [Tietoturvakeskus hinnat](security-center-pricing.md) .

## <a name="data-collection"></a>Tietojen kerääminen

Tietoturvakeskus kerää tietoja oman näennäiskoneiden arvioida niiden suojaus-tilaan, anna suojausta koskevia suosituksia ja ilmoittaa uhkien. Kun käytät Tietoturvakeskus, tietojen kerääminen on otettu käyttöön kaikissa näennäiskoneiden-tilaukseesi. Tietojen kerääminen suositellaan, mutta voit voit osallistumiseen liittyvät Tietoturvakeskus käytännön [käytöstä tietojen kerääminen](#how-do-i-disable-data-collection) .

### <a name="how-do-i-disable-data-collection"></a>Miten voin vaimentaa tietojen kerääminen?

Voit estää **tietojen kerääminen** tilauksen suojauskäytäntö kohdassa milloin tahansa. ([Kirjaudu sisään Azure-portaaliin](https://portal.azure.com), valitse **Selaa**, valitse **Tietoturvakeskuksessa**ja valitse **käytännön**.)  Kun valitset tilauksen, uusi sivu avautuu ja mahdollistaa **tietojen kerääminen** käytöstä vaihtoehto. **Poista tekijöiden** -vaihtoehdon agenttien vuoksi poistaminen aiemmin näennäiskoneiden yläreunan valintanauhassa.

> [AZURE.NOTE] Suojauskäytäntöjä voidaan määrittää resurssin ryhmittelytaso ja Azure tilauksen tasolla, mutta sinun on valittava tilauksen tietojen kerääminen käytöstä.

### <a name="how-do-i-enable-data-collection"></a>Miten voin antaa tietojen kerääminen?
Voit ottaa tietojen keräämistä varten Azure tilauksistasi suojauskäytäntö. Jotta tietojen kerääminen, [Kirjaudu sisään Azure-portaaliin](https://portal.azure.com), valitse **Selaa**, valitse **Tietoturvakeskuksessa**ja valitse **käytännön**. **Tietojen kerääminen** asettaminen **-** ja mihin voi kerätä tietoja tallennustilan-tilien määrittäminen (Katso kysymys "[tietojen tallennussijainti?](#where-is-my-data-stored)"). **Tietojen kerääminen** on otettu käyttöön, kun se kerää automaattisesti määritys ja tapahtuman suojaustiedot kaikki tuetut näennäiskoneiden tilauksen.

> [AZURE.NOTE] Suojauskäytäntöjä voidaan määrittää resurssin ryhmittelytaso ja Azure tilauksen tasolla, mutta tietojen kerääminen määrittäminen tapahtuu vain tilauksen tasolla.

### <a name="what-happens-when-data-collection-is-enabled"></a>Mitä tapahtuu, kun tietojen kerääminen on käytössä?
Tietojen kerääminen on käytössä Azure seuranta-agentti ja Azure-suojauksen valvonta-laajennuksen kautta. Azure-suojauksen valvonta-tunniste eri asiaa suojausasetukset etsii ja lähettää sen [Tapahtuman jäljitys for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (tapahtumien seuranta) jäljittää kyselyjä. Lisäksi käyttöjärjestelmän Luo Tapahtumalokimerkinnät.  Azure seuranta-agentti lukee Tapahtumalokimerkinnät ja tapahtumien seuranta jäljittää ja kopioi ne analysis tallennustila-tiliisi.  Tämä on suojauskäytäntö määritetyissä tallennustilan tili. Saat lisätietoja tallennustilan tilin kysymys "[tietojen tallennussijainti?](#where-is-my-data-stored)"

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>Seurantaa agentti tai suojauksen seuranta-tunniste vaikuttaa Omat palvelimesta suorituskykyä?
Agentti ja tunniste siinä käytetään korko.vuosi määrä järjestelmäresursseja ja pitäisi olla vaikutusta suorituskykyyn.

### <a name="where-is-my-data-stored"></a>Jos tiedoissa on tallennettu?
Kunkin alueen, jonka sisältö on näennäiskoneiden Valitse kyseiset näennäiskoneiden kerättyjen tietojen tallennussijainti tallennustilan tilin. Tämä on helppo voit säilyttää tiedot saman maantieteellisen alueen tietosuoja ja tietojen paikallisen tietosuojan varten. Voit valita tilauksen tallennustilan tiliin suojauskäytäntö. ([Kirjaudu sisään Azure-portaaliin](https://portal.azure.com), valitse **Selaa**, valitse **Tietoturvakeskuksessa**ja valitse **käytännön**.) Kun napsautat tilauksen, uusi sivu avautuu. Valitse **Valitse tallennustilan tilejä** , valitse alue.

> [AZURE.NOTE] Suojauskäytäntöjä voidaan määrittää resurssin ryhmittelytaso ja Azure tilauksen tasolla, mutta tallennustilan tilin alueen valitseminen tapahtuu vain tilauksen tasolla.

Lisätietoja Azure tallennustilan ja tallennustilaa tilit-kohdassa [Tallennustilan asiakirjat](https://azure.microsoft.com/documentation/services/storage/) ja [tietoja Azure-tallennustilan tilit](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Azure suojauksen suunnittelu

### <a name="what-is-a-security-policy"></a>Mikä on suojauskäytäntö?
Suojauskäytäntö määrittää ohjausobjektien, jossa on suositeltavaa resurssien määritetyn tilaus tai resurssiryhmä. Azure Tietoturvakeskus voit määrittää käytännöt Azure tilaukset ja resurssiryhmien mukaan yrityksen suojausvaatimukset ja sovellusten tyypin tai suojaustasoa jokaisen tilauksen tiedot.

Esimerkiksi kehitys- tai käytettäviä resursseja voi olla eri kuin käytetyt tuotannon sovellusten suojausvaatimukset. Sovellusten vastaavasti säännellyillä tietoja, kuten PII (henkilötiedot) saattaa edellyttää paremman suojauksen. Käytössä olevien Azure Tietoturvakeskus suojauskäytäntöjä asema suojausta koskevia suosituksia seurantaan ja valvontaan. Lisätietoja suojauskäytäntöjä on artikkelissa [Suojaus kunnon seuranta Azure Tietoturvakeskuksessa](security-center-monitoring.md).

> [AZURE.NOTE] Tilauksen käytännöt ja resurssin Ryhmäkäytäntö tason mahdollisessa resurssin tason ryhmäkäytännön on nähden.

### <a name="who-can-modify-a-security-policy"></a>Ketkä voivat muokata suojauskäytäntö?
Suojauskäytäntöjä määritetään kunkin tilauksen tai resurssiryhmä. Voit muokata suojauskäytäntö tilauksen tasolla-tai resurssien ryhmittelytason, on oltava omistaja tai osallistuja kyseiseen tilaukseen.

Lisätietoja suojauskäytäntö määrittämisestä on artikkelissa [Azure Tietoturvakeskuksessa asetus suojauskäytäntöjä](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Mikä on suositus suojauksen?
Azure Tietoturvakeskus analysoi Azure resurssien suojaus-tilaan. Kun mahdollisesta tietoturva-aukkoja tunnistetaan, suositukset luodaan. Suosituksia opastaa määrittämisestä tarpeen mukaan ohjausobjektin prosessi. Esimerkkejä:

- Antimalware tunnistamista ja Poista haittaohjelmat valmistelu
- [Verkon käyttöoikeusryhmät](../virtual-network/virtual-networks-nsg.md) ja näennäiskoneiden liikenteen hallinta sääntöjen määrittäminen
- Web-sovelluksen palomuurin suojata vastaan kalastelu kohdistamisen web-sovellusten valmistelu
- Käyttöönotto puuttuvat Järjestelmäpäivitykset
- Osoitteiden OS määritykset, jotka eivät vastaa suositeltu perusviivat

Vain suositukset, jotka ovat käytössä suojauskäytäntöjä ovat mukana.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Miten voin nähdä Azure resurssien suojauksen nykytilan?
**Resurssien kunto** -ruutua **Tietoturvakeskus** -sivu näyttää Yleinen suojaus reagoimisessa, näennäiskoneiden, web-sovellusten ja muita resursseja eritellä ympäristön. Kullekin resurssille on ilmaisimen näyttäminen, jos kaikki mahdolliset tietoturva-aukkoja määrittänyt. Valitsemalla resurssien kunto-ruutu näyttää resurssien ja tunnistaa kohtaa, johon kohde huomiota tai ongelmat saattaa olla.

### <a name="what-triggers-a-security-alert"></a>Mitä käynnistää suojausvaroituksen?
Azure Tietoturvakeskus automaattisesti kerää, analysoi ja Varokkeet Azure resursseja, verkon ja kumppaniratkaisuihin, kuten antimalware ja palomuurit lokitiedot. Kun uhkien havaitaan, suojausvaroituksen luodaan. Esimerkkejä tunnistaminen:

- Vaarantuneen näennäiskoneiden yhteydenpito tunnetut haittaohjelmien IP-osoitteet
- Lisäasetukset haittaohjelman havaita käyttämällä Windowsin virheraportointi
- Palvelinten kalastelu näennäiskoneiden vastaan
- Suojausvaroitusten poistaminen integroitu suojaus kumppaniratkaisut esimerkiksi haittaohjelmien tai Web-sovelluksen palomuurit

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Mitä eroa on uhkien havaita ja ilmoituksia käyttöön Microsoft Security Response Center ja Azure Tietoturvakeskus mukaan?
Microsoftin Security vastauksen Center (MSRC) suorittaa Azure verkon ja infrastruktuurin seuranta Valitse Suojaus ja niistä uhkien liiketoimintatietojen ja väärinkäyttö valitusten kolmansille osapuolille. Kun MSRC saa tietoonsa asiakastietoja käyttää lainvastaiseen tai luvattoman osapuolen tai että Azure asiakkaan käyttö ei mukaisia ehtojen hyväksyttävä use-suojauksen tapauksen hallinta ilmoittaa asiakkaalle. Ilmoitus tapahtuu yleensä lähettämällä sähköpostiviestin määritettyä Azure Tietoturvakeskus tai Azure tilauksen omistaja, jos suojauksen yhteyshenkilön ei ole määritetty suojaus yhteystiedot.

Tietoturvakeskuksessa on Azure-palvelu, joka jatkuvasti valvoo asiakkaan Azure-ympäristön ja koskee analytics havaitsemaan mahdollisesti haitallisen tehtävän monien. Nämä tunnistuksia ovat esiin kuin suojausvaroitukset: Tietoturvakeskus Raporttinäkymät-ikkunan.

### <a name="how-are-permissions-handled-in-azure-security-center"></a>Miten käyttöoikeudet käsitellään Azure Tietoturvakeskuksessa?
Azure Tietoturvakeskus tukee Roolipohjainen käytön. Lisätietoja Roolipohjainen käyttöoikeuksien valvonta (RBAC) Azure-tietokannassa on artikkelissa [Azure Active Directory Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md).

Kun käyttäjä avaa Tietoturvakeskus, vain suosituksia ja ilmoituksia, jotka liittyvät resurssit käyttäjällä on pääsy voi nähdä. Tämä tarkoittaa, että käyttäjät näkevät vain, jos käyttäjä on tehtäviin varattujen resurssien omistajan, osallistujan tai lukijan rooli tilaus tai resurssiryhmä, johon resurssi kuuluu liittyviä kohteita.

Jos haluat:

- **Muokkaa suojauskäytäntö**, on oltava omistaja tai osallistuja tilauksen.
- **Käytä suositus**on oltava omistaja tai osallistuja tilauksen.
- **On näkyvyyden kaikissa tilauksistasi suojaus-tilaan**, sinun on oltava omistaja, osallistujan tai Reader (IT-järjestelmänvalvoja, suojaus-ryhmä) jokaisen tilauksen.
- **Näkyvyys resurssien suojauksen tila on**, sinun on oltava resurssiryhmä omistaja osallistujan tai lukijan (DevOps).

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Azure Tietoturvakeskus valvoo Azure resursseja?
Azure Tietoturvakeskus valvoo Azure seuraavissa resursseissa:

- Näennäiskoneiden (VMs) (mukaan lukien [Pilvipalveluihin](../cloud-services/cloud-services-choose-me.md))
- Azure Virtual verkot
- Azure SQL-palvelu
- Kumppaniratkaisuihin integroitu Azure tilauksen, kuten web application palomuuri VMs ja [Sovelluksen Service-ympäristössä](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Näennäiskoneiden

### <a name="what-types-of-virtual-machines-will-be-supported"></a>Tiedostotyypit, näennäiskoneiden on tuettu?
Suojauksen kunnon seuranta- ja suositukset ovat käytettävissä näennäiskoneiden (VMs), jotka on luotu sekä [perinteinen ja resurssien hallinnan käyttöönotto mallit](../azure-classic-rm.md).

Tuetut Windows VMs:

- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2

Tuetut Linux VMs:

- Versioiden Ubuntu 12.04, 14.04, 16.04
- Debian versiot 7, 8
- CentOS versiot 6. \*, 7.*
- Punainen on Enterprise Linux (RHEL)-versiot 6. \*, 7.*
- SUSE Linux Enterprise Server (SLES) versioita 11. \*, 12.*
- Oracle Linux versiot 6. \*, 7.*

Virtuaalilaitteiksi pilvipalvelussa tuetaan myös. Vain cloud services web- ja työntekijä roolit tuotannon paikkojen seurataan käynnissä. Lisätietoja pilvipalvelussa on artikkelissa [Cloud Services-palvelujen yleiskatsaus](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Miksi Azure Tietoturvakeskus ei tunnista antimalware-ratkaisun Azure-AM käytössä?

Azure Tietoturvakeskuksessa on vain näkyvyys antimalware asennettu Azure tunnisteet kautta. Esimerkiksi Tietoturvakeskus ei havaitse antimalware, joka oli asennettu valmiiksi antamasi kuvan tai jos olet asentanut antimalware oman näennäiskoneiden käyttämällä omaa prosesseja (kuten määritysten hallinta-järjestelmät).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Miksi viestin "Puuttuu skannata tiedot" Omat AM varten?

Se voi kestää jonkin aikaa (yleensä pienempi kuin tunnissa) tarkistuksen tietojen täyttäminen kun tietojen kerääminen on otettu käyttöön Azure Tietoturvakeskuksessa. Skannaa ei täytä VMs pysäytetty-tilaan.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Miksi saan sanoman "AM agentti on puuttuu?"

AM-agentti on oltava asennettuna, jotta tietojen kerääminen VMs. AM-agentti asennetaan oletusarvoisesti VMs, jotka on otettu Azure Marketplacesta. Saat lisätietoja muiden VMs AM-agentti asentamisesta on seuraavassa blogimerkinnässä [AM agentti ja laajennukset](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).
