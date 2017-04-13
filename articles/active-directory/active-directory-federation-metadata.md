<properties
    pageTitle="Azure AD Federation metatietojen | Microsoft Azure"
    description="Tässä artikkelissa kuvataan federation metatietojen asiakirja, joka Azure Active Directory julkaisee palvelut, jotka hyväksyvät Azure Active Directory-tunnukset."
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


# <a name="federation-metadata"></a>Liitetty viestintä metatiedot

Azure Active Directory (Azure AD) julkaisee federation metatietojen asiakirjan Services, joka määrittää hyväksymään suojauksen tunnuksia, jotka Azure AD-ongelmat. Liitetty viestintä metatietojen asiakirja-muoto on kuvattu [Web Services Federation Language (WS liittäminen) versio 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), joka ulottuu [OASIS suojauksen vahvistus Markup Language (SAML) v2.0-metatiedot](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Vuokraajan kielikohtaiset ja vuokraajan riippumattomalla metatietojen päätepisteet

Azure AD julkaisee vuokraajan kielikohtaiset ja vuokraajan riippumattomalla päätepisteet.

Vuokraajan kielikohtaiset päätepisteet on suunniteltu erityisesti vuokraajan. Vuokraajan kielikohtaiset federation metatiedot sisältää tietoja vuokraajan, mukaan lukien vuokraajan kielikohtaiset myöntäjä ja päätepisteen tiedot. Sovellukset, joka rajoittaa yhtä vuokraajan käyttää vuokraajan kielikohtaiset päätepisteet.

Vuokraajan riippumattomalla päätepisteet on yhteiset kaikki Azure AD-vuokraajiin tiedot. Nämä tiedot koskee nykyisessä palvelussa *login.microsoftonline.com* vuokraajiin ja alihallinnat jaetaan. Vuokraajan riippumattomalla päätepisteet suositellaan usean vuokraajan sovellukset, koska niitä ei ole liitetty kaikki tietyn vuokraajan.

## <a name="federation-metadata-endpoints"></a>Liitetty viestintä metatietojen päätepisteet

Azure AD julkaisee federation metatiedot `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

**Vuokraajan kielikohtaiset päätepisteet** `TenantDomainName` voi olla jokin seuraavista:

- Rekisteröity toimialuenimi Azure AD vuokraaja, kuten: `contoso.onmicrosoft.com`.
- Pysyvä vuokraaja tunnus toimialueen, kuten `72f988bf-86f1-41af-91ab-2d7cd011db45`.

**Vuokraajan riippumattomalla päätepisteet** `TenantDomainName` on `common`. Tämän asiakirjan luettelo vain käytettävissä olevia kirjasimia kaikki Azure AD-vuokraajiin, joita isännöidään osoitteessa login.microsoftonline.com Federation metatiedot-osista.

Vuokraajan kielikohtaiset päätepisteen olla esimerkiksi `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Vuokraajan riippumattomalla päätepiste on [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Voit tarkastella federation metatietojen asiakirja kirjoittamalla URL-selaimessa.

## <a name="contents-of-federation-metadata"></a>Sisällön metatietojen federation

Seuraavassa osassa on tietoja, jotka käyttävät tunnusten myöntämä Azure AD-palveluiden tarvitsemia.

### <a name="entity-id"></a>Kohteen tunnus

`EntityDescriptor` Elementti sisältää `EntityID` määrite. Arvo `EntityID` määrite edustaa myöntäjä, eli Suojaustunnussanomapalvelun (STS), joka on myöntänyt tunnuksen. On tärkeää tarkistaa myöntäjä, kun saat tunnusta.

Seuraavat metatiedot näkyy otoksen vuokraajan kielikohtaiset `EntityDescriptor` elementti, jonka `EntityID` elementti.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Voit korvata vuokraajan riippumattomalla päätepisteen vuokraajan tunnus vuokraajan ID-tunnusta, voit luoda vuokraajan kielikohtaiset `EntityID` arvo. Tuloksena oleva arvo on sama kuin tunnuksen myöntäjän. Strategia sallii usean vuokraajan-sovellusta, voit tarkistaa tietyn vuokraajan myöntäjä.

Seuraavat metatiedot näkyy otoksen vuokraajan riippumattomalla `EntityID` elementti. Huomaa, `{tenant}` on literaalimerkit ei paikkamerkin.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Suojaustunnuksen allekirjoitetun varmenteet

Kun palvelu vastaanottaa tunnuksen, joka on myöntänyt Azure AD-vuokraajan-tunnuksen allekirjoitus on tarkistettava allekirjoitetun avainta, joka on julkaistu federation metatietojen asiakirja. Liitetty viestintä-metatiedot on sertifikaatteja, joita alihallinnat käyttää suojaustunnuksen allekirjoittamiseen julkisen osan. Varmenteen raaka tavua näkyvät `KeyDescriptor` elementti. Tunnuksen allekirjoitusvarmenne on allekirjoitukseen vain silloin, kun arvo `use` -määrite on `signing`.

Azure AD julkaisema federation metatietojen asiakirjan voi olla useita allekirjoitetun näppäimiä, kuten milloin Azure AD valmistelee päivittämään allekirjoitusvarmenteen. Kun federation metatietojen asiakirja on useampi kuin yksi varmenne, palvelu, joka on vahvistaminen paikkamerkkejä tukee kaikki varmenteet asiakirjassa.

Seuraavat metatiedot näkyy otoksen `KeyDescriptor` elementti, jonka allekirjoitetun avain.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

`KeyDescriptor` Määritetty rakenne näkyy kahdessa paikassa federation metatietojen asiakirjassa. WS-Federation erityinen-osan ja SAML kielikohtaiset-osan. Varmenteet julkaistu molemmissa osissa on sama.

WS-Federation erityinen-osassa WS Federation metatietojen lukuohjelmaa lukea-varmenteet `RoleDescriptor` elementti `SecurityTokenServiceType` tyyppi.

Seuraavat metatiedot näkyy otoksen `RoleDescriptor` elementti.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

SAML-kohtaisia-osassa WS Federation metatietojen lukuohjelmaa lukea varmenteet- `IDPSSODescriptor` elementti.

Seuraavat metatiedot näkyy otoksen `IDPSSODescriptor` elementti.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Eroja ei ole vuokraajan kielikohtaiset ja vuokraajan riippumattomalla varmenteet-muodossa.

### <a name="ws-federation-endpoint-url"></a>WS Federation päätepisteen URL-osoite

Federation metatiedot sisältää URL-osoite, joka on Azure AD käyttötarkoituksia kertakirjautuminen ja yksi WS Federation protokollan Kirjaudu ulos. Tämä päätepiste tulee näkyviin `PassiveRequestorEndpoint` elementti.

Seuraavat metatiedot näkyy otoksen `PassiveRequestorEndpoint` vuokraajan kielikohtaiset päätepisteen elementti.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Vuokraajan riippumattomalla päätepisteen WS Federation URL-osoite näkyy WS Federation päätepisteen, kuten seuraavassa esimerkissä on esitetty.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>SAML protokolla päätepisteen URL-osoite

Liitetty viestintä-metatiedot sisältävät Azure AD käyttää kertakirjautuminen ja kirjaudu ulos SAML 2.0-protokollan yhden URL-osoite. Nämä päätepisteet näkyvät `IDPSSODescriptor` elementti.

Kirjaudu sisään ja kirjaudu ulos URL-osoitteet näkyvät `SingleSignOnService` ja `SingleLogoutService` osat.

Seuraavat metatiedot näkyy otoksen `PassiveResistorEndpoint` vuokraajan kielikohtaiset päätepisteelle.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Vastaavasti yleisiä SAML 2.0-protokollan päätepisteet Päätepisteet julkaistaan vuokraajan riippumattomalla federation metatiedoissa, kuten seuraavassa esimerkissä on esitetty.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
