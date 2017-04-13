<properties
    pageTitle="Suojatun avaimen säilö | Microsoft Azure"
    description="Avaimen säilö vaults ja näppäimet ja tietoja hallintaa käyttöoikeuksien hallinta Todennus- ja mallin avaimen säilö ja suojaamisesta avaimen säilö"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/07/2016"
    ms.author="ambapat"/>


# <a name="secure-your-key-vault"></a>Suojatun avaimen säilö

Azure avaimen säilö on pilvipalvelussa, valvontaa salausavaimet ja cloud sovellusten tietoja (kuten varmenteet yhteyden merkkijonoja, salasanojen). Koska nämä tiedot ovat luottamuksellisia ja yrityksen kriittinen haluat suojata access avaimen vaults niin, että vain valtuutetut sovellukset ja käyttäjät voivat käyttää avaimen säilö. Tässä artikkelissa esitellään tärkeimmät säilö access-mallin, kerrotaan todennus- ja ja kerrotaan, miten voit suojata access cloud-sovellusten ja Esimerkki avaimen säilö.

## <a name="overview"></a>Yleiskatsaus

Avaimen säilö pääsy hallitaan kaksi erillistä liittymää: hallinta tasossa ja tietojen taso. Molemmat tasojen ERISNIMI todennus- ja on määritettävä ennen soittajan (käyttäjä tai sovelluksen) voit avata avaimen säilö. Todennus muodostaa, soittajan tunnistetiedot, kun luvan määrittää, mitä toimintoja soittajan on oikeus tehdä.

Todennustavaksi hallinta tasossa ja tietojen tasossa käyttää Azure Active Directory. Luvan hallinta tasossa käyttää Roolipohjainen käyttöoikeuksien valvonta (RBAC) tietojen tasossa käyttää avaimen säilö käyttöoikeuskäytäntö.

Tässä artikkelissa käsiteltäviä aiheita lyhyt yleiskuvaus:

[Todennus käyttämällä Azure Active Directory](#authentication-using-azure-active-direcrory) - tässä osassa kerrotaan, miten soittajan todentaa Azure Active Directory käyttämään avaimen säilö hallinta tasossa ja tietojen tasossa kautta. 

[Hallinnan tasossa ja tietojen taso](#management-plane-and-data-plane) - hallinnan tasossa ja tietojen tasossa ovat käyttöön avaimen säilö käytettävän kahden access-tason. Accessin kunkin tason tukee tiettyjen toimintojen. Tässä osassa kuvataan access-päätepisteet-toimintojen tuetut ja käyttää ohjausobjektin tapa, jolla kunkin tasossa. 

[Hallinnan tasossa käyttöoikeuksien valvonta](#management-plane-access-control) - sisältö tarkastellaan sallitaan hallinta tasolla toimintoja käyttämällä Roolipohjainen käyttöoikeuksien valvonta.

[Tietoja vertailutaso käyttöoikeuksien valvonta](#data-plane-access-control) - tässä osassa kuvataan, miten avaimen säilö käyttöoikeuskäytäntö avulla voit hallita tietojen taso.

[Esimerkki](#example) – tässä esimerkissä kerrotaan, miten määrittäminen oman avaimen säilö sallimaan kolme eri ryhmiä (suojaus-ryhmä, kehittäjät/operaattoreita ja tarkastajille) kehittämiseen, hallita ja seurata sovelluksen Azure tiettyjen tehtävien suorittamiseen käyttöoikeuksien hallinta.


## <a name="authentication-using-azure-active-directory"></a>Todennus Azure Active Directoryn avulla

Kun luot avaimen säilö Azure tilaukseen, on automaattisesti liitetty tilauksen Azure Active Directory-vuokraajan. Tämä kohdevuokraajassa, jotta voit käyttää tätä avaimen säilö on rekisteröitävä kaikki soittajat (käyttäjät ja sovellukset). Sovelluksen tai käyttäjän täytyy todentaa Azure Active Directory käyttämään avaimen säilö. Tämä koskee sekä hallinta tasossa ja tietojen tasossa käyttöoikeuksia. Kummassakin tapauksessa sovellus voi käyttää avaimen säilö kahdella tavalla:

-  **käyttäjän + sovelluksen access** - yleensä tämä koskee sovelluksia, jotka käyttävät avaimen säilö kirjautunut sisään käyttäjän puolesta. Azure PowerShell, Azure-portaalissa on esimerkkejä Accessin. Avulla myöntää käyttöoikeuksia käyttäjille kahdella tavalla: yksi tapa on käyttöoikeuksien myöntäminen käyttäjille, jotta he pääsevät avaimen säilö mistä tahansa sovelluksesta ja muulla tavalla on myöntää käyttöoikeuden avaimen säilö vain, kun he käyttävät tietylle sovellukselle (jota kutsutaan koostetun tunnistetietojen). 
-  **vain sovelluksen access** - sovelluksissa, jotka Suorita daemon palvelut-tausta työt jne. Sovelluksen tunnistetiedot on käyttöoikeudet avaimen säilö.

Sovellusten kummankintyyppisten sovelluksen todentaa Azure Active Directoryn avulla [tuettu todennusmenetelmät](../active-directory/active-directory-authentication-scenarios.md) ja hankkii tunnusta. Todennustapa määräytyy sovelluksen tyyppi. Valitse sovellus käyttää tunnuksessa ja lähettää REST API-pyynnön avaimen säilö. Jos hallinta tasossa access pyynnöt reititetään Azure Resurssienhallinta päätepisteen kautta. Tietoja tasossa käytettäessä sovellusten käytön suoraan avaimen säilö päätepiste. Katso lisätietoja [koko todennus työnkulku](../active-directory/active-directory-protocols-oauth-code.md). 

Resurssinimi sovellus pyytää, jonka tunnus on erilaiset riippuen siitä, onko sovellus käyttää hallinta tasossa tai tietojen taso. Näin ollen resurssinimi on joko hallinta tasossa tai tietojen tasossa päätepiste myöhemmin-Azure käyttöympäristösi mukaan-osassa taulukon mukaisesti.

Käyttöoikeuksien hallinta ja tietojen tasossa yksi yksi tapa ottaa on oma etuja:

- Organisaatiot voivat keskitetysti ohjaa kaikki avaimen vaults organisaation
- Jos käyttäjä jättää ne välittömästi pääse käyttämään kaikki avaimen vaults organisaatiossa
- Organisaatiot voivat mukauttaa todentaminen vakioverkko Azure Active Directory (esimerkiksi ottaminen käyttöön monimenetelmäisen todentamisen suojauksen) asetukset

## <a name="management-plane-and-data-plane"></a>Hallinnan tasossa ja tietojen tasossa

Azure avaimen säilö on Azure-palvelu Azure resurssien hallinnan käyttöönottomalli kautta. Kun luot avaimen säilö, saat virtual säilön sisällä, jossa voit luoda muita objekteja, kuten näppäimet ja varmenteiden tietoja. Sitten voit käyttää omaa avaimen säilö suorittaa tiettyjä hallinta tasossa ja tietojen tasossa käyttäminen. Tasossa hallintaliittymään käytetään hallita oman avaimen säilö itse, kuten luominen, poistaminen, avaimen säilö määritteiden päivittäminen ja määrittäminen access-käytännöt tietojen taso. Tietoja tasossa käyttöliittymän käytetään lisääminen, poistaminen, muokata ja näppäimet, tietoja ja avaimen säilöön tallennettuja varmenteet.

Hallinnan tasossa ja tietojen tasossa liittymät käytetään eri päätepisteet (Katso taulukko) kautta. Toisen sarakkeen taulukossa kuvataan eri Azure ympäristöissä päätepisteen DNS-nimet. Kolmas sarake kuvataan kunkin access tasossa voit suorittaa toimintoja. Jokaisen access-tasossa on myös omassa access-ohjausobjektin järjestelmä: varten tasossa access hallinnassa on määritetty käyttämällä Azure Resource Manager Role-Based Access ohjausobjektin (RBAC), aikana, tietojen tasossa käyttöoikeuksien hallinta on määritetty avaimen säilö access käytännön avulla.

| Access-taso | Access-päätepisteet | Toiminnot | Access-ohjausobjektin järjestelmä|
|--------------|------------------|--------------------|--------|
| Hallinnan tasolla|**Yleistä:**<br> Management.Azure.com:443<br><br> **Azure Kiinan:**<br> Management.chinacloudapi.CN:443<br><br> **Azure US Government:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Saksa:**<br> Management.microsoftazure.de:443 | Luo, lue ja Päivitä/poistaminen avaimen säilö <br> Avaimen säilö access käytäntöjen määrittäminen<br>Määritä tunnisteet avaimen säilö | Azure resurssien hallinnan Roolipohjainen käyttöoikeuksien valvonta (RBAC) |
| Tietoja tasossa | **Yleistä:**<br> &lt;säilö nimi&gt;. vault.azure.net:443<br><br> **Azure Kiinan:**<br> &lt;säilö nimi&gt;. vault.azure.cn:443<br><br> **Azure US Government:**<br> &lt;säilö nimi&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Saksa:**<br> &lt;säilö nimi&gt;. vault.microsoftazure.de:443 | Näppäimet: Salauksen, salata, UnwrapKey, WrapKey, tarkista, kirjaudu, Hae, luettelon, päivittäminen, luominen, tuominen ja poistaminen, varmuuskopiointi, palauttaminen<br><br> Saat tietoja: Hae-luettelosta määrittäminen, poista | Avaimen säilö käyttöoikeuskäytäntö|

Hallinnan tasossa ja tietojen tasossa access ohjausobjektit toimivat. Esimerkiksi jos haluat myöntää sovelluksen oikeuden näppäimillä avaimen säilöön, haluat myöntää avaimen säilö käytäntöjen avulla tietojen taso käyttöoikeudet ja hallinta ei ole tasossa access ei tarvita tämän sovelluksen. Ja, jos haluat, että käyttäjä voi lukea säilö ominaisuudet ja tunnisteita, mutta ei ole oikeutta näppäimet ja varmenteiden tietoja, voit antaa tämän käyttäjän 'lukuoikeus' käyttämällä RBAC ja ei voi käyttää tietoja tasossa tarvitaan.

## <a name="management-plane-access-control"></a>Hallinnan tasossa käyttöoikeuksien valvonta

Hallinnan tasossa koostuu toimintoja, jotka vaikuttavat avaimen säilö itse. Voit esimerkiksi luoda tai poistaa avaimen säilö. Saat vaults luettelo tilauksen. Voit hakea avaimen säilö ominaisuuksia (esimerkiksi tuote-tunnisteet) ja määrittää avaimen säilö access-käytäntöjä, jotka ohjaavat käyttäjät ja sovellukset, joita voi käyttää näppäimet ja tietoja avaimen säilöön. Hallinnassa tasossa access käyttää RBAC. Katso luettelo kaikista avaimen säilö toiminnoista, joita voi käyttää hallinta tasossa taulukon edeltävän osan kautta. 

### <a name="role-based-access-control-rbac"></a>Roolipohjainen käyttöoikeuksien valvonta (RBAC)
Kunkin Azure tilauksella on Azure Active Directory. Käyttäjät, ryhmät ja sovellusten tästä hakemistosta voidaan myöntää pääsyn hallinta Azure tilauksen resurssit, jotka käyttävät Azure resurssien hallinnan käyttöönottomalli. Tällaista käytönvalvonta kutsutaan Roolipohjainen käyttöoikeuksien valvonta (RBAC). Voit hallita tätä käyttöoikeuksia, voit käyttää [Azure portal](https://portal.azure.com/), [Azure CLI Työkalut](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)tai [Azure Resurssienhallinta REST API](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Azure Resurssienhallinta-mallin avulla luoda avaimen säilö resurssin ryhmittely ja ohjausobjektin Accessin tämän avaimen säilö hallinta suuntaisesti Azure Active Directoryn avulla. Voit esimerkiksi myöntää käyttäjille tai ryhmän mahdollista, voit hallita avaimen vaults tietyn resurssiryhmä.

Voit myöntää käyttäjille, ryhmät ja sovellukset tiettyyn alueeseen access määrittämällä RBAC roolit. Esimerkiksi avulla myöntää käyttöoikeuksia käyttäjä voi hallita avaimen vaults määrittää ennalta määritettyjä rooleja 'avaimen säilö avustaja-käyttäjälle osoitteessa tiettyyn alueeseen. Laajuuden olisi tällöin, tilauksen, resurssiryhmä tai vain tiettyjen avaimen säilö. Tilauksen tasolla rooli koskee kaikkia resurssiryhmiä ja kyseisen tilauksen piiriin kuuluvien resurssien. Resurssin ryhmittelytasolla rooli koskee kyseinen resurssiryhmä kaikki resurssit. Tietyn resurssin rooli koskee vain kyseiselle resurssille. Käytössä on useita esimääritettyjä roolia (katso [RBAC: valmiit roolit](../active-directory/role-based-access-built-in-roles.md)), ja jos ennalta määritettyjä rooleja ei vastaa tarpeitasi voit myös määrittää omia rooleja.

>[AZURE.IMPORTANT] Huomaa, että jos käyttäjä on osallistuja-oikeudet (RBAC) avaimen säilö hallinta suuntaisesti she voivat myöntää henkilölle käyttää tietojen suuntaisesti määrittäminen avaimen säilö access-käytäntö, joka ohjaa tietojen taso. Tämän vuoksi kannattaa tiiviisti Määritä, kuka on "osallistuja-oikeudet oman avaimen vaults varmistamiseksi ainoastaan valtuutetut henkilöt voivat käyttää ja hallita avaimen vaults, avaimet, tietoja ja varmenteet.

## <a name="data-plane-access-control"></a>Tietoja tasossa käyttöoikeuksien valvonta

Avaimen säilö tietojen tasossa koostuu toimintoja, jotka vaikuttavat objektit avaimen säilöön, kuten näppäimet, tietoja ja varmenteet.  Tämä vaihtoehto sisältää toimintoja, kuten Luo avain, tuonti, päivitys, luettelon, varmuuskopiointi ja palauttaminen näppäimet-salauksen toimintoja, kuten merkki, tarkista, salata, salauksen rivittää, ja muun tai tunnisteet ja muita määritteitä näppäimet. Vastaavasti, se sisältää tietoja, on määritetty, luettelon, poista.

Tietoja tasossa käytettävyys määrittämällä access-käytännöt avaimen säilö. Käyttäjän, ryhmän tai sovelluksen on oltava osallistuja-oikeudet (RBAC) hallinta tasossa avaimen säilö, jos haluat määrittää, että tärkeimmät säilö access käytäntöjä varten. Käyttäjän, ryhmän tai sovelluksen voit myöntää avaimet tai tietoja tiettyjen toimintojen suorittaminen avaimen säilöön käyttöoikeutta. avaimen säilö tuki enintään 16 access käytännön merkinnät avaimen säilö. Azure Active Directory-käyttöoikeusryhmän luominen ja käyttäjien lisääminen kyseisen ryhmän tietojen tasossa käyttöoikeuden myöntäminen useat käyttäjät voivat avaimen säilö.

### <a name="key-vault-access-policies"></a>avaimen säilö käytön käytännöt

avaimen säilö käytäntöjen myöntää oikeuksia avaimet, tietoja ja varmenteet erikseen. Voit esimerkiksi tarkastella vain näppäimet käyttöoikeuden käyttäjälle, mutta ei tietoja käyttöoikeuksia. Käyttöoikeudet avaimet tai tietoja tai varmenteet ovat kuitenkin säilö tasolla. Toisin sanoen avaimen säilö käyttöoikeuskäytäntö ei tue objektin tason käyttöoikeudet. Voit käyttää [Azure portal](https://portal.azure.com/), [Azure CLI Työkalut](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)tai [avaimen säilö hallinnan REST API](https://msdn.microsoft.com/library/azure/mt620024.aspx) voit määrittää access-käytännöt avaimen säilö.

>[AZURE.IMPORTANT] Huomaa, että tärkeimmät säilö käytäntöjen koskee säilö tasolla. Kun käyttäjä on oikeus luoda ja poistaa näppäimet, hän voi suorittaa kaikki avaimet toimintaan kyseisen avaimen säilöön.

## <a name="example"></a>Esimerkki

Oletetaan, että kehittävät sovellus, joka käyttää varmenteen SSL-Azure tallennustilan tietojen tallentamiseen ja käyttää Kirjaudu toimintojen RSA 2048-bittinen-näppäintä. Oletetaan, että sovellus on käynnissä AM (tai AM-asteikko-määritetty). Voit avaimen säilö avulla voit tallentaa sovelluksen tietoja ja käyttää avaimen säilö, johon käynnistyksen varmenne, jota käytetään sovelluksen Azure Active Directory todentamismenetelmä.

Näin on tässä on yhteenveto kaikista näppäimet ja tietoja voidaan tallentaa avaimen säilöön.

- **SSL-varmenne** - käytetään SSL
- **Tallennustilan avain** - tallennustilan tilin pääsy
- **RSA 2048-bittinen avain** - käytettävän Kirjaudu toiminnot
- **Automaattisen käynnistyksen varmenne** - todennetaan Azure Active Directory, jotta pääset käsiksi avaimen säilö Nouda tallennustilan-näppäintä ja allekirjoittamiseen RSA-näppäimellä.

Nyt täyttävät oletetaan, että henkilöt, jotka ovat hallinta, käyttöönotto ja valvonta tämän sovelluksen. Tässä esimerkissä käytetään kolme roolia.

- **Suojaus-ryhmä** - nämä ovat yleensä IT-henkilöstön Office yhtiön (johtava viranomainen), tai vastaavia, vastuussa tietoja, kuten SSL-varmenteita, allekirjoittamiseen yhteyden merkkijonot tietokantojen-tallennustilan tilin näppäimet RSA avaimet ERISNIMI säilyttämisestä.
- **Kehittäjät/operaattorit** - nämä ovat asiantuntijoille tiedoksi kuka kehittää tämän sovelluksen ja ottaa sen käyttöön Azure-tietokannassa. Yleensä ne eivät kuulu suojaus-ryhmä ja siksi ne olisi ole käyttöoikeutta luottamuksellisia tietoja, kuten SSL-varmenteita, RSA näppäimet, mutta ne ottaa käyttöön sovelluksen pitäisi käyttää niitä.
- **Tarkastajille** – tämä on yleensä henkilöä, erillään kehittäjille ja Yleiset IT-henkilöstön samat. Niiden vastuu on Tarkista oikean käytön ja säilyttäminen varmenteet ja avaimet, ja varmista, että tietojen suojauksen vaatimusten noudattaminen. 

On yksi Lisää rooli, joka on tämän sovelluksen, mutta asiaa tähän mainittu ulkopuolella ja, joka on tilaus (tai resurssiryhmä) järjestelmänvalvoja. Tilauksen järjestelmänvalvoja määrittää ensimmäisen käyttöoikeuksien suojaus-ryhmä. Seuraavassa on oletetaan, että tilaus-järjestelmänvalvoja on myöntänyt suojaus-ryhmän käyttöoikeuden mitä kaikkien resurssien tarvittavat tämän sovelluksen asuvat resurssin ryhmään.

Nyt katsotaan, mitä toimintoja rooleille suorittaa tämän sovelluksen kontekstissa.

- **Suojaus-ryhmä**
    - Luo avaimen vaults
    - Ottaa käyttöön avaimen säilö kirjaaminen
    - Lisää näppäimet/tietoja
    - Luo varmuuskopio näppäimet palauttaminen
    - Myönnä käyttöoikeudet käyttäjille ja sovellukset voivat suorittaa tiettyjä avaimen säilö access-käytännön määrittäminen
    - Siirtää säännöllisesti näppäimet/tietoja
- **Kehittäjät/operaattorit**
    - Käynnistää viittauksia ja SSL varmenteet (thumbprints) tallennustilan avain (salaisuus URI) ja kirjautua key (avain URI) suojaus-ryhmä
    - Kehittää ja ottaa käyttöön sovelluksen, joka käyttää näppäimet ja tietoja ohjelmallisesti
- **Tarkastajille**
    - Tarkista Käyttölokit vahvistamaan oikean avaimen/salaisuus käytön ja tietojen suojauksen vaatimusten noudattaminen

Nyt katsotaan avaimen säilö mitä käyttöoikeudet ovat tarvitsemia rooleille (ja sovelluksen) tekemiseen varatut tehtävät. 

| Käyttäjärooli    | Tasossa hallintaoikeudet | Tietojen taso käyttöoikeudet |
|--------------|------------------------------|------------------------|
|Suojaus-ryhmä|avaimen säilö avustaja|Näppäimet: Voit varmuuskopioida, Luo, poista, Hae, tuominen, luettelon, palauttaminen <br> Tietoja: kaikki|
|Kehittäjät/ylläpitäjä| avaimen säilö käyttöönotto käyttöoikeuksia niin, että ne käyttöön VMs voit noutaa tietoja avaimen säilöstä | Ei mitään |
|Tarkastajille| Ei mitään | Näppäimet: luettelo<br>Tietoja: luettelo|
|Sovelluksen| Ei mitään | Näppäimet: merkki<br>Tietoja: hankkiminen |

>[AZURE.NOTE] Tarkastajille on luettelo näppäimet ja tietoja käyttöoikeuksia, jotta ne Tarkasta määritteet näppäimet ja tietoja, jotka ovat ei ole lähetetty lokit, kuten tunnisteita, aktivointi-ja vanhenemista.

Lisäksi oikeuden avaimen säilö kaikki kolme roolia on linkkejä muihin resursseihin. Jos esimerkiksi haluat, että voit ottaa käyttöön VMs (tai Web Apps jne.) Kehittäjät/operaattoreita on myös näiden resurssityypit 'Osallistuja-käyttöoikeus. Tarkastajille on lukuoikeudet avaimen säilö lokit tallennuspaikkaa tallennustilan-tilille.

Tässä artikkelissa keskitytään käytön avaimen säilö on suojaamisesta, koska on kuvaavat liittyvät ohjeaihe asianmukainen osia vain ja ohittaa tiedot käyttöönotto varmenteet ja avaimet ja tietoja käyttäminen ohjelmallisesti jne. Nämä tiedot jo kuuluvat johonkin toiseen kohtaan. Käyttöönotto avaimen säilöön, VMs sertifikaatteja käsitellään [blogimerkintä](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)ja ei [otoksen koodin](https://www.microsoft.com/download/details.aspx?id=45343) käytettävissä, joka havainnollistaa käyttämisestä käynnistyksen varmenteen todentamiseen Azure AD, jotta pääset käsiksi avaimen säilö.

Useimmat käyttöoikeuksia voi myöntää Azure-portaalissa, mutta hajautetun käyttöoikeuksien myöntämisestä kannattaa käyttää PowerShellin Azure (tai Azure CLI) halutun tuloksen saavuttamiseksi. 

Seuraavat PowerShell-katkelmat oletetaan, että:

- Azure Active Directory-järjestelmänvalvoja on luonut käyttöoikeusryhmät, jotka edustavat kolme roolia eli Contoso Security Team Contoso sovelluksen Devops Contoso App tarkastajille. Järjestelmänvalvoja on lisännyt käyttäjät ryhmiin, ne kuuluvat.

- **ContosoAppRG** on resurssiryhmän, joissa kaikki resurssit sijaitsevat. **contosologstorage** on, johon lokitiedostot on tallennettu. 

- Avaimen säilö **ContosoKeyVault** ja tallennustilaa tilin avaimen säilö lokit **contosologstorage** käytettäviä on oltava samassa Azure sijainnissa


Ensin tilauksen järjestelmänvalvoja määrittää 'avaimen säilö avustaja' ja käyttäjän Access-järjestelmänvalvoja-roolien suojaus-ryhmä. Näin voit hallita niiden käyttöä muita resursseja ja hallita avaimen vaults resurssiryhmän ContosoAppRG suojaus-ryhmä.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Seuraavaa komentosarjaa havainnollistaa, miten suojaus-ryhmän luoda avaimen säilö kirjaaminen ja rooleista ja sovelluksen käyttöoikeuksien määrittäminen-asetuksista. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionToKeys backup,create,delete,get,import,list,restore -PermissionToSecrets all

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devlopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $role

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionToKeys list -PermissionToSecrets list
```

Määritetty mukautettu rooli on käyttäjän tilaukseen, jossa ContosoAppRG resurssiryhmä on luotu. Jos sama mukautettuja rooleja käytetään tulevissa projekteissa, valitse muut tilaukset, sen laajuus voi olla lisätty tilauksia.

Kehittäjät/operaattorit "ottaa käyttöön/toiminto" oikeuden mukautetun roolimääritys on määritetty resurssiryhmän. Näin vain luotu resurssiryhmä 'ContosoAppRG' VMs saavat tietoja (SSL-varmenteen ja automaattisen käynnistyksen varmenteen). VMs, joka keskihajonta/ops työryhmän jäsen luo muita resurssiryhmä ei voi liittyä näitä tietoja, vaikka ne tiesi salainen URI.

Tässä esimerkissä kuvataan yksinkertaisessa skenaariossa. Todellisia tilanteita, joissa voi olla monimutkaisia ja joudut ehkä muuttamaan oman avaimen säilö tarvittavat käyttöoikeudet. Tässä esimerkissä on Oletetaan esimerkiksi, että suojaus antaa avain ja salainen viittaukset (URI ja thumbprints) kehittäjät/operaattorit kyseisen ryhmän on viitattava sovelluksessaan. Näin ollen niitä ei tarvitse myöntää kehittäjät/operaattorit minkä tahansa tietojen tasossa käyttöoikeuksia. Huomaa, että tässä esimerkissä ohjeessa on suojaaminen avaimen säilö. Samalla huomiota olisi kiinnitettävä turvaamaan liian [oman VMs](https://azure.microsoft.com/services/virtual-machines/security/), [tallennustilan tilit](../storage/storage-security-guide.md) ja Azure muita resursseja.

>[AZURE.NOTE] Huomautus: Tässä esimerkissä näytetään, miten tärkeimmät säilö access lukitaan alaspäin tuotannon. Kehittäjät pitäisi olla omia tilaus tai resourcegroup joihin heillä on täydet oikeudet vaults, VMs ja tallennustilaa tilin kohtaa, johon ne kehittää sovellusta.


## <a name="resources"></a>Resurssit

-   [Azure Active Directory Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md)

    Tässä artikkelissa kerrotaan Azure Active Directory Roolipohjainen käyttöoikeuksien valvonta ja miten se toimii.

-   [RBAC: Roolien hallinta](../active-directory/role-based-access-built-in-roles.md)

    Tässä artikkelissa kuvataan kaikki valmiit roolit RBAC käytettävissä.

-   [Tietoja resurssien hallinnan käyttöönotto- ja perinteinen käyttöönotto](../resource-manager-deployment-model.md)

    Tässä artikkelissa kerrotaan Resurssienhallinta käyttöönotto- ja perinteinen käyttöönoton mallit ja kerrotaan käyttämisestä henkilöstöpäälliköt- ja -ryhmät

-    [Roolipohjainen käyttöoikeuksien valvonta Azure PowerShellin hallinta](../active-directory/role-based-access-control-manage-access-powershell.md)

     Tässä artikkelissa kerrotaan, miten voit hallita Roolipohjainen käyttöoikeuksien valvonta Azure PowerShellin avulla

-   [Roolipohjainen käyttöoikeuksien valvonta REST-ohjelmointirajapinnalla hallinta](../active-directory/role-based-access-control-manage-access-rest.md)

    Tässä artikkelissa kerrotaan, miten REST-Ohjelmointirajapinnalla avulla voit hallita RBAC.

-   [Microsoft Azure-Ignite Roolipohjainen käyttöoikeuksien valvonta](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Tämä on kanavan 9 videon linkin 2015 MS Ignite neuvottelusta. Tässä istunnossa ne keskustella hallinta ja raportointiominaisuudet Azure-tietokannassa ja tutkiminen parhaita käytäntöjä ympärille Azure Azure Active Directory-tilauksista käytön suojaamisesta.

-   [Hyväksy OAuth 2.0 ja Azure Active Directoryn web-sovellusten käyttö](../active-directory/active-directory-protocols-oauth-code.md)

    Tässä artikkelissa kuvataan valmis OAuth 2.0-työnkulku todennustapa Azure Active Directory-hakemistosta.

-   [avaimen säilö hallinnan REST API](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    Tämä asiakirja on viittauksen REST API hallittavan avaimen säilö ohjelmallisesti, mukaan lukien avaimen säilö käyttöoikeuskäytäntö.

-   [avaimen säilö REST API](https://msdn.microsoft.com/library/azure/dn903609.aspx)

    Linkki avaimen säilö REST API-oppaat.

-   [Avaimen käyttöoikeuksien valvonta](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)

    Linkki avaimen access ohjausobjektin oppaat.

-   [Salainen käyttöoikeuksien valvonta](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)

    Linkki avaimen access ohjausobjektin oppaat.

-   [Asettaa](https://msdn.microsoft.com/library/mt603625.aspx) ja [poistaa](https://msdn.microsoft.com/library/mt619427.aspx) avaimen säilö käyttöoikeuskäytäntö PowerShellin avulla

    Linkit viittaavat PowerShell cmdlet-komentojen avaimen säilö käyttöoikeuskäytäntö ohjeissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Hakeminen Aloittaminen opetusohjelman järjestelmänvalvoja Katso [Azure avaimen säilö käytön aloittaminen](key-vault-get-started.md).

Saat lisätietoja avaimen säilö, käyttötietojen [Azure avaimen säilö lokiin kirjaaminen](key-vault-logging.md).

Saat lisätietoja näppäimet ja tietoja käyttäminen Azure avaimen säilö [tietoja näppäimet ja tietoja](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Jos sinulla on kysyttävää avaimen säilö, käy [Azure avaimen säilö keskustelupalstat](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
