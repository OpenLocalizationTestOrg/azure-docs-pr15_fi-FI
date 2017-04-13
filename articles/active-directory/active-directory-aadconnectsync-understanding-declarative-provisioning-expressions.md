<properties
    pageTitle="Azure AD Connect synkronointi: tietoja määritettäviä valmistelu lausekkeiden | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan määritettäviä valmistelu lausekkeet."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect synkronointi: tietoja määritettäviä valmistelu lausekkeet
Azure AD Connect synkronointi perustuu määritettäviä valmistelu ensin käyttöön Forefront käyttäjätietojen hallinta 2010: ssä. Sen avulla voit toteuttaa ilmeen integrointi business logiikan tarvitsematta kirjoittaa käännetty koodi.

Olennainen osa määritettäviä valmistelu on määrite kulkee lausekkeen kieleen. Käytettävä kieli on alijoukkoa Microsoft Visual Basic® for Applications (VBA). Tämä kieli Microsoft Officen ja käyttäjät, joilla on kokemusta VBScript myös tunnistaa sen. Määritettäviä valmistelun Lausekielellä vain funktioilla ja ei ole rakenteellisia kieli. Taulukoissa ei ole menetelmistä tai lauseita. Funktiot ovat sen sijaan ylemmän tason express ohjelma pystysuuntaiseksi.

Lisätietoja on artikkelissa [Tervetuloa Visual Basic for Office 2013: n sovellusten kieliohje](https://msdn.microsoft.com/library/gg264383.aspx).

Määritteet erittäin kirjoitetaan. Funktion hyväksyy vain määritteiden tyyppi on oikea. Kannattaa myös kirjainkoko on merkitsevä. Funktioiden nimet ja määritteiden nimet on oltava asianmukaisesti kannen tai virhe ilmenee virhe.

## <a name="language-definitions-and-identifiers"></a>Kielen määritykset ja tunnisteet

- Funktiot on jokin muu nimi perään argumentit hakasulkeissa: funktionnimi (argumentti 1, N argumentti).
- Määritteet tunnistetaan hakasulkeisiin: [attributeName]
- Parametrien tunnistetaan prosenttimerkki: ParameterName %
- Merkkijonovakiot ovat uhkaavat tarjoukset: esimerkiksi "Contoso" (Huomautus: on käytettävä suorat lainausmerkit "" ja ei kaarevilla lainausmerkeillä "")
- Numeeriset arvot ilmaistuna ilman lainausmerkkejä ja tarkoitus desimaaleja. Heksadesimaali arvot ovat etuliite & h Esimerkiksi 98052 & HFF
- Totuusarvot ilmaistaan kanssa vakioita: TOSI, EPÄTOSI.
- Valmiin vakioita ja literaalien ilmaistaan vain nimen sisältävän: NULL-CRLF, IgnoreThisFlow

### <a name="functions"></a>Funktiot
Määritettäviä valmistelu käyttää monia funktioita käyttöön, voit muuntaa määritteiden arvot. Näitä toimintoja voi olla upotettu, niin yhden funktion tulos on välitetty jokin toinen funktio.

`Function1(Function2(Function3()))`

Toimintojen luettelo löytyy [funktioista](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parametrit
Parametri on määritetty yhdistin tai järjestelmänvalvoja PowerShellin avulla. Parametrien sisältävät yleensä arvot, jotka ovat eri järjestelmästä toiseen, esimerkiksi toimialueen käyttäjän nimi sijaitsee. Nämä parametrit voidaan määrite muodostuvalle.

Active Directory Connectorin kerrotaan seuraavat parametrit synkronoinnin saapuvan liikenteen säännöt:

| Parametrin nimi | Kommentti |
| --- | --- |
| Domain.Netbios | Toimialueen tällä hetkellä tuotavassa, kuten FABRIKAMSALES NetBIOS muoto |
| Domain.FQDN | Toimialueen tällä hetkellä tuotavassa, kuten sales.fabrikam.com FQDN muoto |
| Domain.LDAP | LDAP-muoto toimialueen tällä hetkellä tuotavassa, kuten Ohjauskoneen = myynti-Ohjauskoneen = fabrikam Ohjauskoneen = com |
| Forest.Netbios | Metsää-nimen tällä hetkellä tuotavassa, kuten FABRIKAMCORP NetBIOS muoto |
| Forest.FQDN | Metsää-nimen tällä hetkellä tuotavassa, kuten fabrikam.com FQDN muoto |
| Forest.LDAP | LDAP metsää nimen muoto tällä hetkellä tuotavassa, kuten Ohjauskoneen = fabrikam Ohjauskoneen = com |

Järjestelmä sisältää seuraavat parametri, jonka avulla saat käynnissä yhdistimen tunnus:  
`Connector.ID`

Tässä on esimerkki, joka täyttää metaverse määrite toimialueen, jossa käyttäjä sijaitsee toimialueen netbios nimi:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operaattorit
Käytä seuraavia operaattoreita voidaan käyttää:

- **Vertailu**: <, < = <>, =, >, > =
- **Matematiikan**: +, -, \*, -
- **Merkkijono**: ja (KETJUTA)
- **Loogisen**: & & (ja) || (tai)
- **Arvioinnin tilaus**:)

Operaattorit arvioidaan vasemmalta oikealle ja on sama arvioinnin prioriteetti. Ei \* (kerroin) ei lasketa ennen - (vähennyslasku). 2\*(5 + 3) ei ole sama kuin 2\*5 + 3. Arvioinnin järjestystä, kun vasemmalta oikealle arvioinnin tilaus ei ole sopiva käytetään sulkeet ().

## <a name="multi-valued-attributes"></a>Moniarvoinen määritteet
Funktioiden toimii sekä yksittäisen arvon ja moniarvoisen määritteissä. Moniarvoinen määritteet-funktio toimii kaikki arvot ja samaan toimintoon koskee kaikki arvot.

Esimerkki:  
`Trim([proxyAddresses])`Tee rajaaminen, joka proxyAddress-määritteen arvon.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Jokaisen arvon kanssa @-sign, korvata toimialueeseen @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Etsi SIP-osoite ja poistaa sen arvot.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja on artikkelissa [Tietoja määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning.md)määritys mallin.
- Katso, miten määritettäviä valmistelu on käytetty ulos valmiin ymmärtää [Oletusarvo-määritys](active-directory-aadconnectsync-understanding-default-configuration.md).
- Katso, miten voit muuttaa käytännön avulla [Voit tehdä muutoksia työnkulkuun oletusarvon](active-directory-aadconnectsync-change-the-configuration.md)määritettäviä valmistelu.

**Yleistä aiheita**

- [Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi](active-directory-aadconnectsync-whatis.md)
- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

**Ohjeaiheista**

- [Azure AD Connect synkronointi: Funktiot viittaus](active-directory-aadconnectsync-functions-reference.md)
