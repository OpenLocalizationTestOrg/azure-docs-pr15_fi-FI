<properties
    pageTitle="Azure yksittäisen Kirjaudu ulos SAML protokolla | Microsoft Azure"
    description="Tässä artikkelissa kuvataan yksittäisen Sign-Out SAML-protokollan Azure Active Directoryssa"
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


# <a name="single-sign-out-saml-protocol"></a>Kirjaudu ulos yhden SAML-protokolla

Azure Active Directory (Azure AD) tukee SAML 2.0 web-selaimessa yhden Kirjaudu ulos profiilin. Kirjaudu ulos yksittäiselle toimii oikein, Azure AD on rekisteröitävä sen metatietojen URL-Osoitteen sovelluksen rekisteröinnin yhteydessä. Kirjaudu ulos URL-osoite ja pilvipalvelussa allekirjoitetun avain hakee Azure AD metatiedot. Azure AD saapuvan LogoutRequest allekirjoituksen tarkistaminen allekirjoitetun avaimen avulla, ja LogoutURL käytetään ohjaamaan käyttäjät, kun ne on kirjautunut ulos.

Jos pilvipalvelussa sijaitsevaan ei tue metatietojen päätepisteen, kun sovellus rekisteröidään, kehittäjä Ota yhteyttä Microsoftin asiakaspalveluun Kirjaudu URL-osoite ja allekirjoitetun avain.

Tässä kaaviossa on esitetty Azure AD yksittäisen Kirjaudu ulos prosessin työnkulun.

![Työnkulun yksittäinen uloskirjautuminen](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

Cloud palvelun lähettää `LogoutRequest` viestin Azure AD osoittamassa, että istunnon on lopetettu. Seuraavat ote näkyy otoksen `LogoutRequest` elementti.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

`LogoutRequest` Azure AD lähetetään elementti vaatii määritteet:

- `ID`: Kirjaudu ulos pyynnön Tämä ilmaisee. Arvon `ID` luku eivät saa alkaa. Tyypillinen käytäntö on liittäminen **tunnus** GUID-tunnus string-esitys.

- `Version`: Tämä elementti arvo arvoksi **2.0**. Tämä arvo on pakollinen.

- `IssueInstant`: Tämä on `DateTime` merkkijono järjestämiseen yleisajan (UTC-aika)-arvon ja [vastauksen muoto ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD odottaa tällaista arvon, mutta ei säilytä.

- `Consent`, `Destination`, `NotOnOrAfter` Ja `Reason` määritteet ohitetaan, jos ne sisältyisivät `LogoutRequest` elementti.

### <a name="issuer"></a>Myöntäjä

`Issuer` Osan `LogoutRequest` on oltava täsmälleen samat jokin **ServicePrincipalNames** pilvipalvelussa Azure AD. Tämä on yleensä määritetty **Sovelluksen tunnus URI** , joka on määritetty sovelluksen rekisteröinnin yhteydessä.

### <a name="nameid"></a>NameID

Arvo `NameID` elementti on oltava täsmälleen samat `NameID` käyttäjän, joka on kirjautunut ulos.
## <a name="logoutresponse"></a>LogoutResponse

Azure AD-lähettää `LogoutResponse` saatuaan `LogoutRequest` elementti. Seuraavat ote näkyy otoksen `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure AD-joukot `ID`, `Version` ja `IssueInstant` arvot `LogoutResponse` elementti. Se määrittää myös `InResponseTo` elementin arvo `ID` määritettä `LogoutRequest` , altistettu vastaus.

### <a name="issuer"></a>Myöntäjä

Azure AD määrittää tämän arvon `https://login.microsoftonline.com/<TenantIdGUID>/` missä <TenantIdGUID> Azure AD-vuokraajan vuokraajan ID-tunnusta.

Arvo lasketaan `Issuer` elementti, käyttötarkoitusta **Sovelluksen tunnus URI** arvosta sovelluksen rekisteröinnin yhteydessä.

### <a name="status"></a>Tila

Azure AD-käyttötavat `StatusCode` osan `Status` elementin osoittamaan onnistumisesta tai epäonnistumisesta, kirjaudu ulos. Kun Kirjaudu ulos epäonnistuu, `StatusCode` elementti voi sisältää myös mukautettuja virhesanomia.
