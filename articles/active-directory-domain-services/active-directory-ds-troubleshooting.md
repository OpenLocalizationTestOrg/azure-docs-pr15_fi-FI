<properties
    pageTitle="Azure Active Directory-toimialuepalveluista: Vianmääritysoppaan | Microsoft Azure"
    description="Azure AD-toimialueen palveluiden vianmääritysoppaan"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD-toimialuepalveluita - Vianmääritysoppaan käyttäminen
Tämä artikkeli tarjoaa vianmääritysohjeita liittyvistä ongelmista määrittäminen ja Azure Active Directory (AD) toimialueen palveluiden hallintaan.


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Et voi ottaa käyttöön Azure AD toimialuepalveluita Azure AD-kansio
Tämän osan avulla voit tehdä vianmäärityksen, kun yrität hakemistossa Azure AD-toimialueen palvelut käyttöön ja sen epäonnistuu tai saa vaihtaa takaisin "Käytöstä".

Valitse vianmäärityksen vaiheet, jotka vastaavat, kohtaat virhesanoman.

|**Virhesanoma**|**Tarkkuus**|
|---|:---|
|*Nimi-contoso100.com on jo käytössä tässä verkossa. Määritä nimi, joka ei ole käytössä.*|[Toimialueen nimiristiriita virtual verkossa](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*Toimialueen palveluiden ei voi ottaa tämän Azure AD-vuokraajan. Palvelu ei ole tarvittavia oikeuksia "Azure AD-toimialueen palvelut Synkronoi"-nimisen sovelluksen. Poistaa "Azure AD-toimialueen palvelut Synkronoi"-sovelluksen ja yritä sitten toimialueen palveluiden Azure AD-vuokraajan käyttöön.*|[Toimialueen palveluiden ei ole tarvittavia oikeuksia Azure AD-toimialueen palvelut Synkronoi-sovellus](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*Toimialueen palveluiden ei voi ottaa tämän Azure AD-vuokraajan. Azure AD-vuokraajan toimialuepalveluita-sovellusta ei ole tarvittavat käyttöoikeudet, jotta toimialuepalveluihin. Poista sovellus-tunniste d87dcbc6-a371-462e-88e3-28ad15ec4e64 sovellukseen ja yritä sitten toimialueen palveluiden Azure AD-vuokraajan käyttöön.*|[Toimialueen palveluiden-sovellusta ei ole määritetty oikein vuokraajan](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*Toimialueen palveluiden ei voi ottaa tämän Azure AD-vuokraajan. Azure AD-vuokraajan Microsoft Azure AD-sovellus on poistettu käytöstä. Ottaa käyttöön sovelluksen tunnus 00000002-0000-0000-c000-000000000000 sovellukseen ja yritä sitten toimialueen palveluiden Azure AD-vuokraajan käyttöön.*|[Microsoft Graph-sovellus on poistettu käytöstä Azure AD-vuokraajan](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>Toimialueen Nimiristiriita
**Virhesanoma:**

*Nimi-contoso100.com on jo käytössä tässä verkossa. Määritä nimi, joka ei ole käytössä.*

**Korjaus:**

Varmista, että sinulla ei ole aiemmin toimialue, joilla on sama toimialuenimi käytettävissä virtual verkossa. Oletetaan esimerkiksi, sinulla on toimialue nimeltä "contoso.com" jo käytettävissä valitun virtual verkossa. Yrität myöhemmin käyttöön Azure AD-toimialueen palveluista sama toimialuenimi (eli contoso.com) virtual verkon hallitun toimialue. Käytössä ilmenee virhe yritettäessä käyttöön Azure AD-toimialueen palveluista.

Tämä virhe on Nimiristiriitojen toimialuenimen virtual verkossa. Tässä tapauksessa sinun käytettävä eri nimellä Azure AD-toimialueen palveluista hallitun toimialueen määrittäminen. Vaihtoehtoisesti voit valmistella varaustiedoista jo olemassa oleva toimialue ja siirry Azure AD-toimialueen palveluiden käyttöön.


### <a name="inadequate-permissions"></a>Riittämätön käyttöoikeudet
**Virhesanoma:**

*Toimialueen palveluiden ei voi ottaa tämän Azure AD-vuokraajan. Palvelu ei ole tarvittavia oikeuksia "Azure AD-toimialueen palvelut Synkronoi"-nimisen sovelluksen. Poistaa "Azure AD-toimialueen palvelut Synkronoi"-sovelluksen ja yritä sitten toimialueen palveluiden Azure AD-vuokraajan käyttöön.*

**Korjaus:**

Tarkista, onko sovelluksen nimellä "Azure AD-toimialueen palvelut synkronointi' Azure AD-kansiossa. Jos sovellus on luotu, poista se ja sitten uudelleen käyttöön Azure AD-toimialueen palveluista.

Suorita seuraavat vaiheet, tarkista, onko sovellus ja poista se, jos sovellus on luotu:

  1. Siirry **Azure perinteinen portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
  2. Valitse vasemmanpuoleisessa ruudussa **Active Directory** -solmu.
  3. Valitse Azure AD-vuokraajan (hakemisto), jonka haluat ottaa käyttöön Azure AD-toimialueen palveluista.
  4. Siirry **sovellukset** -välilehti.
  5. Valitse avattavasta valikosta **yritykseni omistaa sovellukset** -vaihtoehto.
  6. Tarkista sovelluksen nimeltä **Azure AD-toimialueen palvelut Synkronoi**. Jos sovellus on luotu, voit poistaa sen nyt.
  7. Kun olet poistanut sovellus, yritä uudelleen käyttöön Azure AD-toimialueen palveluista.


### <a name="invalid-configuration"></a>Virheellinen määritys
**Virhesanoma:**

*Toimialueen palveluiden ei voi ottaa tämän Azure AD-vuokraajan. Azure AD-vuokraajan toimialuepalveluita-sovellusta ei ole tarvittavat käyttöoikeudet, jotta toimialuepalveluihin. Poista sovellus-tunniste d87dcbc6-a371-462e-88e3-28ad15ec4e64 sovellukseen ja yritä sitten toimialueen palveluiden Azure AD-vuokraajan käyttöön.*

**Korjaus:**

Tarkista, onko sovelluksen nimellä AzureActiveDirectoryDomainControllerServices (ja sovelluksen tunnus d87dcbc6-a371-462e-88e3-28ad15ec4e64) Azure AD-kansiossa. Jos sovellus on luotu, joudut poistaa sen ja ottaa Azure AD-toimialueen palveluista uudelleen.

Käytä seuraavaa PowerShell-komentosarjaa Etsi sovellus ja poista se.

> [AZURE.NOTE] Tämä komentosarja käyttää **PowerShellin Azure AD versio 2** cmdlet-komennot. Täydellisen luettelon kaikki käytettävissä olevat cmdlet-komennot ja lataa moduuli Lue [AzureAD PowerShell-oppaat](https://msdn.microsoft.com/library/azure/mt757189.aspx).

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph käytöstä
**Virhesanoma:**

Toimialueen palveluiden ei voi ottaa tämän Azure AD-vuokraajan. Azure AD-vuokraajan Microsoft Azure AD-sovellus on poistettu käytöstä. Ottaa käyttöön sovelluksen tunnus 00000002-0000-0000-c000-000000000000 sovellukseen ja yritä sitten toimialueen palveluiden Azure AD-vuokraajan käyttöön.

**Korjaus:**

Tarkista, jos olet poistanut sovelluksen 00000002 0000-0000-c000-000000000000-tunnuksella. Sovellus on Microsoft Azure AD-sovellus ja tarjoaa Graph API Azure AD-vuokraajan. Azure AD-toimialueen palveluista on tämän sovelluksen käyttöön Synkronoi Azure AD-vuokraajan hallitun toimialueesi.

Voit korjata tämän virheen, tämän sovelluksen ottaminen käyttöön ja yritä sitten toimialueen palveluiden Azure AD-vuokraajan käyttöön.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Käyttäjät eivät voi kirjautua Azure AD-toimialueen palveluista hallitun toimialueen
Jos yhden tai usean käyttäjän Azure AD-vuokraajan ei voi kirjautua juuri luomasi hallitun toimialueen, suorittaa vianmäärityksen seuraavasti:

- **-Kirjautuminen täydellisen Käyttäjätunnuksen muodossa:** Yritä kirjautua sisään UPN-muodossa (esimerkiksi 'joeuser@contoso.com') sijaan SAMAccountName-muoto ('CONTOSO\joeuser'). SAMAccountName voidaan automaattisesti luoda käyttäjille UPN etuliite on liian pitkä tai on sama kuin toisen käyttäjän hallitun toimialueella. Täydellisen Käyttäjätunnuksen muoto on varmasti Azure AD-vuokraajan yksilöivä.

> [AZURE.NOTE] On suositeltavaa kirjautua Azure AD-toimialueen palveluista hallitun toimialueen UPN-muodossa.

- Varmista, että sinulla on [käytössä salasanojen synkronoinnin](active-directory-ds-getting-started-password-sync.md) mukaisesti käytön aloittaminen-oppaassa kuvatut vaiheet.

- **Ulkoiset tilit:** Varmista, että kyseessä olevaan käyttäjätili ei ole Azure AD-vuokraajan ulkoinen tili. Ulkoiset tilit on esimerkkejä Microsoft-tilit (esimerkiksi 'joe@live.com') tai ulkoisessa käyttäjätilit Azure AD-kansio. Azure AD-toimialueen palveluista ei ole tunnistetietojen esimerkiksi käyttäjätilit, koska nämä käyttäjät voi kirjautua sisään hallitun toimialueeseen.

- **Synkronoitu tilejä:** Jos kyseessä olevaan käyttäjätilit on synkronoitu paikallisesta hakemistosta, varmista seuraavat seikat:
    - Ottaa käyttöön tai päivittää [uusimmat suositellut Azure AD Connect-versiossa](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect).

    - Olet määrittänyt Azure AD Connect [täyden käyttäjätietojen](active-directory-ds-getting-started-password-sync.md)suorittamiseen.

    - Mukaan hakemistossa suuri se saattaa kestää jonkin aikaa varten käyttäjätilejä ja tunnistetietojen hajautusarvot voi käyttää Azure AD-toimialueen palveluihin. Varmista odottaa niin kauan ennen tarkistetaan (sen mukaan, hakemisto - päiväksi muutaman tunnin tai kahden suuren kansioiden kokoa).

    - Jos ongelma toistuu, kun olet varmistanut edelliset vaiheet, Käynnistä Microsoft Azure AD-synkronointi-palvelu uudelleen. Synkronoi tietokoneesta komentorivi Käynnistä ja suorita seuraavat komennot:
      1. nettonykyarvon Lopeta Microsoft Azure AD-synkronointi
      2. nettonykyarvon Käynnistä Microsoft Azure AD-synkronointi

- **Vain pilvipalveluita tilejä**: Jos tarvittavien käyttäjätilillä on vain pilvipalveluita käyttäjätilin, varmista, että käyttäjä on vaihtanut salasanansa, kun Azure AD-toimialueen palveluista. Tässä vaiheessa aiheuttaa tunnistetieto hajautusarvot vaaditaan Azure AD-toimialueen palveluista luodaan.


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Käyttäjät, jotka on poistettu Azure AD-vuokraajan ei poisteta hallitun toimialueen
Azure AD suojaa olet vahingossa käyttäjäobjekteja. Kun poistat käyttäjätilin Azure AD-vuokraajan, vastaava käyttäjäobjektin siirretään roskakoriin. Tämä poistotoiminto on synkronoitu hallitun toimialueen, kun se saa vastaavan käyttäjätilin merkitä ei käytössä. Tämän ominaisuuden avulla voit palauttaa tai peruuttaa käyttäjätili myöhemmin.

Jos haluat poistaa käyttäjätilin täysin hallitun toimialueen, poistaa käyttäjä pysyvästi Azure AD-vuokraajan. Poista-MsolUser PowerShell cmdlet-komennon avulla '-RemoveFromRecycleBin' vaihtoehdon tämän [MSDN-artikkelissa](https://msdn.microsoft.com/library/azure/dn194132.aspx)kuvatulla tavalla.


## <a name="contact-us"></a>Yhteystiedot
Azure Active Directory-toimialueen palveluista tuotetiimin, ota yhteyttä [palautteen tai tukea] (aktiivinen-hakemisto-ds-yhteyshenkilö-us.md).
