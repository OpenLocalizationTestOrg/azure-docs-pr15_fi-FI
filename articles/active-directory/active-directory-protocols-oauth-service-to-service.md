<properties
    pageTitle="Azure AD-palveluiden todennus käyttämällä OAuth2.0 | Microsoft Azure"
    description="Tässä artikkelissa käsitellään HTTP viestien avulla voit toteuttaa palveluiden todennus käyttämällä OAuth2.0 asiakkaan tunnistetiedot myöntäminen työnkulku."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>

# <a name="service-to-service-calls-using-client-credentials"></a>Palvelun puhelujen soittaminen asiakkaan käyttäjätiedot-palvelu

Web-palvelu ( *luottamuksellisia asiakas*) soitettaessa toiseen web-palveluun, sen sijaan, että tekeytyminen käyttäjä todennetaan omassa tunnistetietojen avulla OAuth 2.0 asiakkaan tunnistetiedot myöntäminen Flow sallii. Tässä skenaariossa asiakas on yleensä keskimmäisen web-palvelu, daemon palvelun tai sivustoa.

## <a name="client-credentials-grant-flow-diagram"></a>Asiakkaan käyttäjätiedot myöntää Tietovuokaavion Esimerkki

Seuraavassa kaaviossa tässä artikkelissa kerrotaan, miten asiakkaan käyttäjätiedot myöntää työnkulku toimii Azure Active Directory (Azure AD).

![OAuth2.0 asiakkaan tunnistetiedot myöntäminen työnkulku](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. Asiakassovellus todentaa Azure AD-tunnuksen liittoutumispalvelujen päätepiste ja pyytää access-tunnuksen.
2. Azure AD-tunnuksen liittoutumispalvelujen päätepisteen ongelmat access-tunnuksen.
3. Access-tunnuksen käytetään suojatun resurssin todennusta.
4. Web-sovelluksen palauttaa suojatun resurssin tiedot.

## <a name="register-the-services-in-azure-ad"></a>Rekisteröi Azure AD-palvelut

Rekisteröi puheluja palvelu ja vastaanottamisessa palvelun Azure Active Directory (Azure AD). Lisätietoja on artikkelissa [lisääminen, päivittäminen, ja sovelluksen poistaminen](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Pyydä suojaustunnus

Pyytää käyttöoikeustietue, käytä vuokraajan kielikohtaiset HTTP kirjaa Azure AD-päätepiste.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Suojaustunnuksen palvelun Käyttöoikeuspyyntöä

Palvelun palvelua suojaustunnuksen pyyntö sisältää seuraavat parametrit.

| Parametri | | Kuvaus |
|-----------|------|------------|
| response_type | pakollinen | Määrittää pyydetyt vastaustyyppi. Arvon on oltava **client_credentials**asiakkaan tunnistetiedot Grant-työnkulussa.|
| client_id | pakollinen | Määrittää Azure AD-Asiakastunnus puheluja WWW-palvelun. Etsi puheluja sovelluksen Asiakastunnus Azure hallinta-portaalissa, valitse **Active Directory**, hakemisto, valitse sovellus ja valitse sitten **Määritä**.|
| client_secret | pakollinen |  Kirjoita rekisteröity puheluja WWW-palvelun Azure AD-näppäintä. Luo avain, Azure hallinta-portaalissa, valitse **Active Directory**, hakemisto, sovellus ja valitse sitten **Määritä**. |
| resurssi | pakollinen | Kirjoita sovelluksen tunnus URI vastaanottamisessa WWW-palvelun. Etsi sovellus tunnus-URI Azure hallinta-portaalissa, valitse **Active Directory**, hakemisto, valitse sovellus ja valitse sitten **Määritä**. |

## <a name="example"></a>Esimerkki

Seuraavat HTTP POST pyytää access-tunnuksen https://service.contoso.com/ WWW-palvelun. `client_id` Tunnistaa verkkopalveluun, joka pyytää access-tunnuksen.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Palvelun palvelua suojaustunnuksen vastaus

Success vastauksen sisältää seuraavat parametreilla JSON OAuth 2.0 vastausta.

| Parametri   | Kuvaus |
|-------------|-------------|
|access_token |Pyydetty käyttöoikeustietue. Puheluja WWW-palvelun avulla tunnuksessa todennetaan vastaanottamisessa web-palveluun. |
|access_type  | Ilmaisee tunnuksen tyyppi-arvo. Ainoa, joka tukee Azure AD on **haltijan**. Saat lisätietoja haltijan-tunnukset [OAuth 2.0 luvan Framework: haltijan suojaustunnuksen käyttö (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Kuinka kauan käyttöoikeustietue on voimassa (sekunteina).|
|expires_on   |Kun access-tunniste vanhentuu aika. Päivämäärän esitetään sekunteina-1970-01 – 01T0:0:0Z UTC-ajan päättymisaika asti. Tätä arvoa käytetään välimuistiin tunnusten elinkaaren määrittämiseen. |
|resurssi     | Sovelluksen tunnus URI vastaanottamisessa WWW-palvelun. |

## <a name="example"></a>Esimerkki

Seuraavassa esimerkissä esitetään, access-tunnuksen web-palveluun pyyntö success vastauksen.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Katso myös

* [Azure AD-OAuth 2.0](active-directory-protocols-oauth-code.md)
