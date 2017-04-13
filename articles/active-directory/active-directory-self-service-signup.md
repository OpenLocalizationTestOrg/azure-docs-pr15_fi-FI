<properties
    pageTitle="Omatoimisen kirjautuminen Azure-ominaisuudet | Microsoft Azure"
    description="Yleiskatsaus Omatoiminen kirjautuminen Azure rekisteröitymistä hallinnasta ja miten voit ottaa DNS-toimialuenimen päälle."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>Omatoimisen kirjautuminen Azure-ominaisuudet

Tässä ohjeaiheessa kerrotaan Omatoiminen rekisteröitymistä ja miten voit ottaa DNS-toimialuenimen päälle.  

## <a name="why-use-self-service-signup"></a>Miksi käyttää valintatoiminto Omatoiminen?

- Asiakkaiden siirtyä services he haluavat nopeammin.
- Luo sähköpostipohjainen tarjouksia palvelun.
- Luo sähköpostipohjainen kirjautuminen työnkulut, joissa nopeasti käyttäjät voivat luoda helposti muista työn niiden sähköpostitunnuksen käyttämällä käyttäjätietoja.
- Ei-hallitun Azure kansioita voit myöhemmin tehdä hallitun kansioihin ja käyttää uudelleen muissa palveluissa.

## <a name="terms-and-definitions"></a>Termit ja määritykset

+ **Omatoimisen rekisteröintiä**: Tämä on tapa, jolla käyttäjä kirjautuu pilvipalveluun ja käyttäjätieto luodaan automaattisesti niiden Azure Active Directory (Azure AD) perustuu niiden sähköpostitoimialueelle.
+ **Ei-hallitun Azure directory**: Tämä on kansio, jossa jäsenyyttä on luotu. Ei-hallitun kansio on kansio, jossa on yleinen järjestelmänvalvoja ei ole.
+ **Sähköpostin vahvistettu käyttäjä**: Tämä on Azure AD-käyttäjätilin tyypin. Käyttäjät, jotka on luotu automaattisesti sen jälkeen, kun Omatoiminen tarjouksen rekisteröityminen on nimeltään sähköpostin vahvistettu käyttäjän tunnistetiedot. Sähköpostin vahvistettu käyttäjä on merkitty creationmethod kansion säännöllisesti jäsen = EmailVerified.

## <a name="user-experience"></a>Käyttäjäkokemus

Jos esimerkiksi oletetaan, että käyttäjä, jonka sähköpostiosoite on Dan@BellowsCollege.com vastaanottaa luottamuksellisia tiedostoja sähköpostitse. Tiedostot on suojattu Azure Rights Management (Azure RMS). Mutta Dan's organisaation paljetyyppiset Opiskelijan on ei ole vielä Azure RIGHTS, eikä se on käyttöön Active Directory-RMS. Tässä tapauksessa Dan rekisteröidä ilmainen tilaus hallintapalvelua henkilöille, jotta voit lukea suojattuja tiedostoja.

Jos Dan on ensimmäinen käyttäjä, jolla BellowsCollege.com rekisteröitävä tämän itsepalvelu ojentamassa, valitse ei-hallitun kansio luodaan BellowsCollege.com Azure AD-sähköpostiosoite. Jos tämä tarjoaa tai samanlaisia Omatoiminen tarjoaa rekisteröityminen muiden käyttäjien BellowsCollege.com toimialueesta, heillä on myös sähköposti vahvistettu käyttäjätileihin luodaan ei-hallitun samassa kansiossa Azure-tietokannassa.

## <a name="admin-experience"></a>Järjestelmänvalvoja-toiminto

Kuka omistaa DNS-toimialuenimi ei-hallitun Azure kansion järjestelmänvalvoja voi menee tai Yhdistä hakemiston jälkeen todistamalla omistajuus. Seuraavassa kerrotaan tarkemmin järjestelmänvalvoja-ratkaisun, mutta tässä on yhteenveto:

- Kun ei-hallitun Azure directory kautta, riittää, että Yleinen järjestelmänvalvoja ei-hallitun hakemiston muuttuvat. Tätä kutsutaan joskus sisäinen ottamalla haltuun jollei.
- Ei-hallitun Azure directory yhdistäminen, DNS-toimialuenimi ei-hallitun kansion lisääminen hallitun Azure hakemistossa ja käyttäjien resurssien määritystä on luotu niin käyttäjät voivat jatkaa palvelujen keskeytyksettä. Tätä kutsutaan joskus ulkoisen ottamalla haltuun jollei.

## <a name="what-gets-created-in-azure-active-directory"></a>Mitä luodaan Azure Active Directoryn?

#### <a name="directory"></a>Hakemisto

- Azure Active Directory-hakemistosta toimialueen luodaan ja yksi kansion toimialuetta kohti.
- Azure AD-kansiossa on ei ole yleinen järjestelmänvalvoja.

#### <a name="users"></a>Käyttäjät

- Kullekin käyttäjälle, Rekisteröi-Azure AD-kansio luodaan user-objekti.
- Jokainen objekti on merkitty ulkoiseksi.
- Kunkin käyttäjän annetaan käyttöoikeus, he rekisteröitynyt palveluun.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Miten voin ottaa Omatoiminen Azure AD hakemiston omistan toimialueen?

Voit ottaa Omatoiminen Azure AD hakemiston suorittamalla toimialueen vahvistus. Toimialueen vahvistus osoittaa, että omistat toimialueen DNS-tietueiden luomisen avulla.

Voit tehdä DNS vaihtotarjouksena Azure AD-kansion kahdella tavalla:

- Sisäinen ottamalla haltuun jollei (järjestelmänvalvoja ei-hallitun Azure directory löytää ja haluaa tehdä Hallittu hakemisto)
- Ulkoinen ottamalla haltuun jollei (Lisää uusi toimialue niiden hallittu Azure hakemisto yritystä järjestelmänvalvoja)

Olet ehkä vahvistetaan, että omistat toimialueen, koska sen jälkeen, kun käyttäjä on suorittanut Omatoiminen kirjautuminen tai voit saattaa olla uusi toimialueen lisääminen olemassa Hallittu hakemisto aloittamasi ei-hallitun kansion päälle. Esimerkiksi sinulla ole contoso.com-toimialuetta ja haluat lisätä uuden toimialueen nimi contoso.co.uk tai contoso.uk.

## <a name="what-is-domain-takeover"></a>Mikä on toimialue ottamalla haltuun jollei?  

Tässä osassa kerrotaan, miten voit tarkistaa, että omistat toimialueen

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Mikä on toimialueen vahvistus ja miksi sitä käytetään?

Jotta voit suorittaa toimintoja hakemiston Azure AD edellyttää Vahvista DNS-toimialueen omistajuuden.  Toimialueen vahvistamisen avulla voit ottaa hakemiston ja joko edistää Hallittu hakemisto Omatoiminen hakemiston tai Omatoiminen hakemiston yhdistäminen olemassa Hallittu hakemisto.

## <a name="examples-of-domain-validation"></a>Esimerkkejä toimialueen tarkistuksesta

Voit tehdä DNS vaihtotarjouksena kansion kahdella tavalla:

+ Sisäinen ottamalla haltuun jollei (esimerkiksi järjestelmänvalvoja löytää Omatoiminen, ei-hallitun hakemisto ja haluaa tehdä Hallittu hakemisto)
+ Ulkoinen ottamalla haltuun jollei (esimerkiksi järjestelmänvalvojan yrittää Lisää uusi toimialue Hallittu hakemisto)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Sisäinen ottamalla haltuun jollei - edistää Omatoiminen, ei-hallitun hakemisto on hallittu hakemisto

Kun teet sisäinen ottamalla haltuun jollei, hakemiston saa muuntaa ei-hallitun hakemistosta Hallittu hakemisto. Sinun täytyy suorittaa DNS toimialueen nimen tarkistaminen, joihin luoda MX-tietue- tai TXT-tietue DNS-vyöhyke. Toiminto:

+ Tarkistaa, että omistat toimialueen
+ Tekee Hallittu hakemisto
+ Tekee hakemiston Yleinen järjestelmänvalvoja

Oletetaan, että järjestelmänvalvoja-paljetyyppiset Opiskelijan löytää, että käyttäjät oppilaitoksella on rekisteröinyt Omatoiminen tarjouksista. DNS rekisteröidyn omistajan nimi BellowsCollege.com ja IT-järjestelmänvalvoja voi Vahvista omistajuus Azure DNS-nimi ja valitse menee ei-hallitun hakemiston. Hakemiston tulee Hallittu hakemisto ja IT-järjestelmänvalvoja on määritetty BellowsCollege.com hakemiston yleisen järjestelmänvalvojan roolia.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Ulkoinen ottamalla haltuun jollei - Omatoiminen hakemiston yhdistäminen olemassa Hallittu hakemisto

Ulkoisen ottamalla haltuun jollei, valitse Hallittu hakemisto on jo ja haluat liittyä hallitun kansion kaikki käyttäjät ja ryhmät ei-hallitun hakemistosta sijaan oman kaksi erota kansioita.

Järjestelmänvalvojana Hallittu hakemisto toimialueen lisäämistä ja toimialueen tapahtuu on ei-hallitun kansio, joka liittyy.

Esimerkiksi oletetaan, että olet IT-järjestelmänvalvoja ja jo Hallittu hakemisto Contoso.com-toimialuenimi, joka on rekisteröity organisaation. Huomaat, että organisaation käyttäjät olet suorittanut omatoimisen ylös, tarjoaa sähköposti toimialuenimeä user@contoso.co.uk, eli toisen organisaation omistaa toimialuenimi. Nämä käyttäjät ole tilit ei-hallitun hakemistossa contoso.co.uk varten.

Et halua hallinta kaksi erillistä hakemistojen selaaminen, jotta contoso.co.uk ei-hallitun kansio yhdistäminen olemassa hallittu IT hakemistossa contoso.com varten.

Ulkoinen ottamalla haltuun jollei seuraa samalla DNS vahvistus tavalla kuin sisäinen ottamalla haltuun jollei.  Erotus sitä: käyttäjien ja palvelujen määrittää hallitun IT-kansion uudelleen.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Mitä vaikutuksia suorittamiseen ulkoisen ottamalla haltuun jollei?

Ulkoisen ottamalla haltuun jollei, jossa käyttäjät resurssien määritystä luodaan, jotta käyttäjät voivat edelleen käyttää palveluja keskeytyksettä. Monet sovellukset, kuten RMS henkilöille, käsittele hyvin käyttäjien resurssien määritystä, ja käyttäjät voivat edelleen käyttää palveluja ilman muutosta. Jos jokin sovellus ei käsittele käyttäjien resurssien määritystä tehokkaasti, ulkoiset ottamalla haltuun jollei voi erikseen estetty käyttäjiltä estetään heikko kokemus.

#### <a name="directory-takeover-support-by-service"></a>hakemiston vaihtotarjouksen tuki-palvelu

Seuraavat palvelut tukee tällä hetkellä ottamalla haltuun jollei:

- RMS


Seuraavat palvelut pian tukevat ottamalla haltuun jollei:

- PowerBI

Seuraavat eivät ja vaatii muita hallintatoiminto siirretään käyttäjätietoja ulkoiset ottamalla haltuun jollei jälkeen.

- SharePointin ja OneDrive


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Tietoja vaihtotarjouksena DNS toimialueen nimi

Sinulla on useita vaihtoehtoja siitä, miten voit suorittaa toimialueen vahvistus (ja tee vaihtotarjouksena, jos haluat, että):

1.  Azure hallinta-portaalissa

    Vaihtotarjouksena käynnistetään tekemällä toimialueen lisäämistä.  Jos kansio on jo olemassa toimialueen, sinun on ulkoinen ottamalla haltuun jollei suorittaminen.

    Kirjaudu Azure-portaaliin tunnistetiedoilla.  Siirry aiemmin kansio ja sitten **Lisää toimialue**.

2.  Office 365-palveluun

    Voit Office 365: n [toimialueiden hallinta](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) -sivulla asetukset toimialueet ja DNS-tietueet. Katso [toimialueen vahvistaminen Office 365: ssä](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).

3.  Windows PowerShellin

    Seuraavat vaiheet tarvitaan tarkistus, Windows PowerShellin avulla.

    Vaihe    |   Cmdlet-komennon käyttäminen
    ------- | -------------
    Tunnistetietojen objektin luominen | Get-tunnistetiedot
    Azure AD yhdistäminen | Yhdistä MsolService
    toimialueiden luettelo   | Hae MsolDomain
    Luo hankala  | Hae MsolDomainVerificationDns
    DNS-tietueen luominen   | Tee näin DNS-palvelimeen
    Tarkista haasteellista    | Vahvista MsolEmailVerifiedDomain

Esimerkki:

1. Yhteyden muodostaminen käyttämällä tunnistetietoja, joita on käytetty Omatoiminen tarjoaa vastata Azure AD: tuonti-moduulin MSOnline $msolcred = get-tunnistetiedon Yhdistä-msolservice-tunnistetietojen $msolcred

2. Hae toimialueiden luettelo:

    Hae MsolDomain

3. Suorittamalla Get-MsolDomainVerificationDns cmdlet-komennon hankala luomiseen:

    Hae MsolDomainVerificationDns – toimialuenimi *oma_toimialuenimi* – tilassa DnsTxtRecord

    Esimerkki:

    Hae MsolDomainVerificationDns – toimialuenimi contoso.com – tilassa DnsTxtRecord

4. Kopioi arvo (haasteellista), kaava palauttaa tämän komennon.

    Esimerkki:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. Luo julkinen DNS-nimitilaa DNS txt-tietue, joka sisältää arvon, joka on kopioitu edellisessä vaiheessa.

    Tietueen nimi on ylemmän tason toimialueen nimi, joten jos luot tätä resurssitietue käyttämällä DNS-roolin poistaminen Windows Server, jätä tietueen nimi tyhjäksi ja vain Liitä arvo tekstiruutuun

6. Suorita vahvistamiseksi haasteellista Vahvista MsolDomain cmdlet-komennon:

    Vahvista MsolEmailVerifiedDomain - DomainName *oma_toimialuenimi*

    Esimerkki:

    Vahvista MsolEmailVerifiedDomain - DomainName contoso.com

Onnistuneiden hankala palauttaa kehotteen ilman virhettä.

## <a name="how-do-i-control-self-service-settings"></a>Miten Omatoiminen asetukset määrittää?

Järjestelmänvalvojilla on kaksi Omatoiminen ohjausobjektia tänään. Ne voivat hallita:

- Onko käyttäjät voivat liittyä kansioon sähköpostitse.
- Onko käyttäjillä käyttöoikeus voi itse sovelluksia ja palveluja.


### <a name="how-can-i-control-these-capabilities"></a>Miten voit hallita seuraavia ominaisuuksia?

Järjestelmänvalvoja määrittää nämä Azure AD cmdlet-komento Set-MsolCompanySettings parametreilla nämä ominaisuudet:

+ **AllowEmailVerifiedUsers** määrittää, voiko käyttäjä luoda tai liittyä ei-hallitun hakemisto. Jos parametrin arvoksi $false, ei ole sähköpostin vahvistettu käyttäjät voivat liittyä kansioon.
+ **AllowAdHocSubscriptions** , voivatko käyttäjät voivat suorittaa omatoimisen ylöspäin. Jos parametrin arvoksi $false, ei ole käyttäjät voivat suorittaa Omatoiminen kirjautuminen.


### <a name="how-do-the-controls-work-together"></a>Miten ohjausobjektit toimivat yhdessä?

Nämä kaksi parametria voi käyttää yhdessä määrittämiseen omatoimisen hallita tarkemmin ylöspäin. Esimerkiksi seuraava komento avulla käyttäjät voivat suorittaa omatoimisen, mutta vain jos nämä käyttäjät on jo tili Azure AD (toisin sanoen käyttäjät, joilla on luoda sähköpostin vahvistettu tili ei voi suorittaa omatoimisen ylöspäin):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Seuraava vuokaavio kerrotaan eri yhdistelmät näiden parametrien ja hakemisto ja omatoimisen tuloksena olevat ehdot ylöspäin.

![][1]

Saat lisätietoja ja esimerkkejä siitä, miten voit käyttää näitä parametreja [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx).

## <a name="see-also"></a>Katso myös

-  [Asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md)

-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure Cmdlet-viittaus](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
