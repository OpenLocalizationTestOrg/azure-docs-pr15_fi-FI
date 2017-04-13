<properties
    pageTitle="SAML-protokollan Azure kertakirjautuminen | Microsoft Azure"
    description="Tässä artikkelissa kuvataan Azure Active Directoryn yksittäinen merkki-SAML-protokolla"
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

# <a name="single-sign-on-saml-protocol"></a>Yksittäisen Sign-On SAML-protokolla

Tässä artikkelissa käsitellään SAML 2.0 todennus pyynnöt ja vastaukset, jotka Azure Active Directory (Azure AD) tukee, kertakirjautuminen.

Protokollan seuraavassa kuvassa kuvataan Sign järjestyksessä. Pilvipalvelussa (palveluntarjoaja) käyttää HTTP-uudelleenohjaus sidonta välittää `AuthnRequest` (todennus-pyyntö)-osa, kun haluat Azure AD (tunnistetietojen toimittaja). Valitse Azure AD käyttää sitominen kirjaa HTTP-viestin `Response` elementti pilvipalveluun.

![Työnkulun kertakirjautuminen](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Käyttäjien todentamiseen pyytämään cloud services Lähetä `AuthnRequest` elementin Azure AD. Esimerkki SAML 2.0 `AuthnRequest` näyttää tältä:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| TUNNUS | pakollinen | Azure AD käyttää tämän määritteen Täytä `InResponseTo` määrite palautetut vastauksen. Tunnus on ala numero, niin yleisiä strategia kannattaa koskevan merkkijonon, kuten "tunnus" GUID-tunnus string-esitys. Esimerkiksi `id6c1c178c166d486687be4aaf5e482730` on on voimassa. |
| Versio | pakollinen | Tämä on oltava **2.0**.|
| IssueInstant | pakollinen | Tämä on DateTime merkkijono, joka sisältää UTC-aika-arvon ja [vastauksen muoto ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD odottaa tämän tyypin DateTime-arvoksi, mutta ei laske tai -kentän arvoksi. |
| AssertionConsumerServiceUrl | Valinnainen | Jos, tämä on vastattava `RedirectUri` Azure AD cloud-palvelun. |
| ForceAuthn | Valinnainen | Jos, tämä on oltava false. Mikä tahansa arvo aiheuttaa virheen.|
| IsPassive | Valinnainen | Jos, tämä on oltava false. Mikä tahansa arvo aiheuttaa virheen. |  

Kaikki muut `AuthnRequest` määritteet, kuten hyväksyntä, kohteen, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex ja ProviderName ovat **ohitetaan**.

Myös ohittaa Azure AD `Conditions` osan `AuthnRequest`.

### <a name="issuer"></a>Myöntäjä

`Issuer` Osan `AuthnRequest` on oltava täsmälleen samat jokin **ServicePrincipalNames** pilvipalvelussa Azure AD. Tämä on yleensä määritetty **Sovelluksen tunnus URI** , joka on määritetty sovelluksen rekisteröinnin yhteydessä.

Esimerkki SAML ote sisältävän `Issuer` elementti on seuraavanlainen:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Tämä elementti pyytää tietyn nimen tunnus muoto vastauksessa ja on valinnaisia `AuthnRequest` Azure AD lähetetään osat.

Esimerkki `NameIdPolicy` elementti on seuraavanlainen:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Jos `NameIDPolicy` on valmis, voit lisätä sen valinnainen `Format` määrite. `Format` Määrite voi olla vain yksi seuraavista arvoista; mikä tahansa arvo aiheuttaa virheen.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory-ongelmat NameID vaatimus pairwise tunnukseksi.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory-ongelmat sähköpostiosoitteen muoto NameID-ryhmän.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Tämä arvo sallii Azure Active Directory Valitse Varaa-muodossa. Azure Active Directory-ongelmat NameID pairwise tunnukseksi.

Älä sisällytä `SPNameQualifer` määrite. Ohittaa Azure AD `AllowCreate` määrite.

### <a name="requestauthncontext"></a>RequestAuthnContext

`RequestedAuthnContext` Elementti määrittää haluamasi todennustavat. Se on valinnaisia `AuthnRequest` Azure AD lähetetään osat. Azure AD tukee vain yhden `AuthnContextClassRef` arvo: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Vaikutusalueen määrittäminen

`Scoping` Elementti, joka sisältää luettelon tunnistetietojen toimittajat, on valinnaisia `AuthnRequest` Azure AD lähetetään osat.

Jos, älä sisällytä `ProxyCount` määrite, `IDPListOption` tai `RequesterID` elementti, kun niitä ei tueta.

### <a name="signature"></a>Allekirjoitus

Älä sisällytä `Signature` osan `AuthnRequest` elementtejä, kuten Azure AD ei tue kirjautunut käyttöoikeuksien.

### <a name="subject"></a>Aihe

Ohittaa Azure AD `Subject` osan `AuthnRequest` osat.

## <a name="response"></a>Vastaus

Kun pyydetty Sign on suoritettu onnistuneesti, Azure AD kirjaa vastauksen pilvipalveluun. Onnistunut Sign yrityksellä näytteen vastauksen näyttää seuraavanlaiselta:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Vastaus

`Response` Elementti on lupa pyynnön tulos. Azure AD-joukot `ID`, `Version` ja `IssueInstant` arvot `Response` elementti. Myös seuraavien määritteiden määrittää:

- `Destination`: Kun Sign-on suoritettu onnistuneesti, tämä arvo `RedirectUri` palveluntarjoajan (pilvipalvelu).
- `InResponseTo`: Tämä on määritetty `ID` -määrite `AuthnRequest` elementti, joka on aloittanut vastaus.

### <a name="issuer"></a>Myöntäjä

Azure AD-joukot `Issuer` elementin `https://login.microsoftonline.com/<TenantIDGUID>/` missä <TenantIDGUID> Azure AD-vuokraajan vuokraajan ID-tunnusta.

Esimerkki vastauksen myöntäjä osat esimerkiksi näyttää tältä:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Tila

`Status` Elementin välittää onnistumisesta tai epäonnistumisesta, kirjaudu sisään. Se sisältää `StatusCode` -elementin, joka sisältää koodi tai joukko sisäkkäisiä koodit, jotka esittävät pyynnön tilaa. Se sisältää myös `StatusMessage` -elementin, joka sisältää mukautetun virhesanomia, jotka on luotu Sign-prosessin aikana.

<!-- TODO: Add a authentication protocol error reference -->

Seuraavassa on epäonnistunut Sign-yrityksellä SAML vastauksen.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Vahvistus

Lisäksi `ID`, `IssueInstant` ja `Version`, Azure AD määrittää seuraavat osat `Assertion` vastauksen elementti.

#### <a name="issuer"></a>Myöntäjä

Tämä on määritetty `https://sts.windows.net/<TenantIDGUID>/`missä <TenantIDGUID> Azure AD-vuokraajan vuokraajan ID-tunnusta.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Allekirjoitus

Azure AD kirjautuu onnistuneen Sign saatuaan vahvistus. `Signature` Elementti on digitaalinen allekirjoitus, joka pilvipalvelussa avulla voit varmistaa vahvistus aitouden lähde todentaa.

Luo digitaalinen allekirjoitus, Azure AD käyttää allekirjoitetun avain `IDPSSODescriptor` metatietojen sen asiakirjan.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Aihe

Määrittää lyhennys, joka on vahvistus lauseet aihe. Se sisältää `NameID` elementti, joka edustaa todennetun käyttäjän. `NameID` Arvo on kohdistettu tunnus, joka on ohjattu vain palveluntarjoajan, joka on tunnuksen yleisön. Se on jatkuva – se voidaan peruuttaa, mutta ei koskaan uudelleen. Kannattaa myös peittävä, se ei näytä mitään tietoja käyttäjän ja ei voi käyttää tunnisteena määrite kyselyjen.

`Method` -Määrite `SubjectConfirmation` elementti määritetään aina `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Ehdot

Tämä elementti määrittää ehdot, jotka määrittävät SAML vahvistukset hyväksyttävä käyttö.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

`NotBefore` Ja `NotOnOrAfter` määritteet Määritä aikaväli, joka vahvistus on voimassa.

- Arvo `NotBefore` määritettä on yhtä kuin haluat tai hieman (alle sekuntia) viimeistään arvon `IssueInstant` määritettä `Assertion` elementti. Azure AD huomioon ajan itse ja pilvipalvelussa (palveluntarjoajan) väliset erot ja Lisää kaikki puskurin tällä hetkellä.
- Arvo `NotOnOrAfter` -määrite on 70 minuutit arvosta `NotBefore` määrite.

#### <a name="audience"></a>Käyttäjäryhmälle

Sisältää URI-tunnus, joka määrittää yleisö. Azure AD asettaa arvon, tämä elementti arvoon `Issuer` osa `AuthnRequest` , joka käynnistää Sign. Arviointikohde `Audience` arvo, käytä arvoa `App ID URI` , joka on määritetty sovelluksen rekisteröinnin yhteydessä.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Like `Issuer` arvo `Audience` arvon on oltava täsmälleen samat jokin palvelun pääasiallista nimi, joka edustaa Azure AD pilvipalvelussa. Kuitenkin jos arvo `Issuer` osa ei ole URI-arvo `Audience` vastauksen arvo `Issuer` arvon etuliite `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Sisältää saatavat aihe tai käyttäjän tietoja. Seuraavat ote on otoksen `AttributeStatement` elementti. Kolmea pistettä ilmaisee, että osan voivat sisältää useita määritteet ja määritteiden arvot.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Ryhmän nimi** : arvo `Name` määrite (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) on pääasiallista käyttäjänimi todennetun käyttäjän, kuten `testuser@managedtenant.com`.
- **ObjectIdentifier varaa** : arvo `ObjectIdentifier` määrite (`http://schemas.microsoft.com/identity/claims/objectidentifier`) on `ObjectId` , joka edustaa todennetun käyttäjän Azure AD-kansio-objektin. `ObjectId`on muuttumaton, GUID ja käyttää uudelleen todennetun käyttäjän turvallisten tunnus.

#### <a name="authnstatement"></a>AuthnStatement

Tämä elementti vahvistukset vahvistus aihe todennettu tietyn keinoilla tiettynä ajankohtana.

- `AuthnInstant` Määrite määrittää ajan, jolla käyttäjä todennettu Azure AD kanssa.
- `AuthnContext` Elementti määrittää käyttäjä todennetaan todennusta yhteydessä.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
