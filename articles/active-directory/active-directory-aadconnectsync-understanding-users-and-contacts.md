<properties
    pageTitle="Azure AD Connect synkronointi: tietoja käyttäjien ja yhteystietojen | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan käyttäjien ja Azure AD Connect synkronoi yhteystiedot."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD Connect synkronointi: tietoja käyttäjät ja yhteystiedot

On useita eri syitä, miksi on useita Active Directory-puuryhmien ja on useita eri käyttöönoton topologioissa. Yleisiä malleja ovat tilin resurssin käyttöönoton ja sitä sync'ed metsien fuusion & hankinnan jälkeen. Mutta vaikka on täysin mallit, hybrid mallit ovat yleisiin paikan päällä. Azure AD Connect synkronointi oletusarvo-määritys ei oletetaan, että tiettyä mallia, mutta sen mukaan, miten käyttäjän vastaavat valittiin oppaaseen, on noudatettava erilaisia ominaisuuksia.

Tässä artikkelissa käydään läpi miten oletusarvon määrittäminen toimintaa tiettyjä topologioissa. Käydään läpi määritykset ja synkronoinnin säännöt-editorin avulla voidaan tarkastella määritykset.

Sääntöjen muutama yleinen määritykset oletetaan, että:

- Olemme tuominen lähteestä aktiivinen kansioiden järjestys riippumatta lopputulos on aina oltava sama.
- Aktiivinen tili aina osallistuu sisäänkirjautumisongelmien tiedot, kuten **userPrincipalName** ja **sourceAnchor**.
- Käytöstä poistetun tilin osallistuu userPrincipalName ja sourceAnchor, ellei se ole linkitetty postilaatikkoon, jos aktiivinen tili ei löydy.
- Tilin linkitetyn postilaatikkoa ei koskaan käytetään userPrincipalName-ja sourceAnchor. Oletetaan, että aktiivinen tili löytyvät myöhemmin.
- Yhteyshenkilön objektin ehkä valmisteltava Azure AD, yhteyshenkilön tai käyttäjän. Et tiedä todella, kunnes kaikki lähde Active Directory-puuryhmien on käsitelty.

## <a name="contacts"></a>Yhteystiedot

Yhteystiedot, joka edustaa eri metsää käyttäjän on yleisiä fuusion & hankinnan jälkeen missä GALSync ratkaista siltauksen kahden tai useamman Exchange metsien. Yhteyshenkilön objekti on aina liittyminen yhdistimen tilasta metaverse posti-määritteen avulla. Jos näkyvissä on jo yhteyshenkilön objektin tai käyttäjäobjekti, jossa on sama sähköpostiosoite, objektit on yhdistetty toisiinsa. Tämä on määritetty säännön **-AD – yhteyshenkilön Join-**. Säännön, jonka nimi on myös **-AD – yhteyshenkilön Yleiset-** kanssa metaverse-määritteen kulkuun määrite **sourceObjectType** vakion **yhteyshenkilön**kanssa. Tämä sääntö on erittäin pienen ohittaa joten jos käyttäjä objektit on yhdistetty samaan metaverse-objekti ja valitse säännön **--AD – käyttäjän yhteisen** osallistuu tämän määritteen arvo käyttäjän. Tämä sääntö tämän määritteen on yhteyshenkilön, jos käyttäjä ei on liitetty ja arvojen käyttäjän Jos vähintään yksi käyttäjä löysi.

Valmisteluun objektin Azure AD- **ulos AAD – Ota liittyä** lähtevän säännön luo yhteyshenkilön objektia, jos metaverse määrite **sourceObjectType** on määritetty **yhteyttä**. Jos määrite on määritetty **käyttäjälle**, valitse säännön **ulos AAD – käyttäjän liittyä** Luo user-objekti sen sijaan.
Se on mahdollista, että objekti on ylemmän tason yhteystiedosta käyttäjälle enemmän lähde aktiivinen hakemistoja tuotu ja synkronoida.

Esimerkiksi-GALSync topologian olemme etsii yhteystietokohteita kaikille toisen metsää on ensimmäinen metsää tuodessasi. Tämä vaiheen uuden yhteyshenkilön objektien AAD yhdistin. Kun olemme myöhemmin tuominen ja synkronoiminen toisen metsää, syy Etsi reaali käyttäjiä ja liittää aiemmin luotuja metaverse-objekteja. Olemme sitten poistaa yhteyshenkilön objektia AAD ja luoda uuden käyttäjäobjektin sen sijaan.

Jos käytössäsi on verkkotopologia kohtaa, johon käyttäjät ja esittää yhteyshenkilöiksi, varmista, että valitset vastaamaan käyttäjien oppaaseen posti-määrite. Jos valitset toisen vaihtoehdon, sinun on riippuvainen tilaus-määritys. Yhteystietokohteita liitettävää aina posti-määrite, mutta käyttäjäobjekteja vain liittää posti-määritteen Jos tämä vaihtoehto on valittuna oppaaseen. Onnistunut sitten lopputulos kaksi eri objektia saman posti-määritteen metaverse Jos yhteyshenkilön objekti on tuotu ennen käyttäjäobjekti. Vie Azure AD-aikana ilmenee virhe. Tämä ongelma on suunniteltu ominaisuus, ja osoittaa virheellisiä tietoja tai että topologian ei oikein tunnisteta asennuksen aikana.

## <a name="disabled-accounts"></a>Käytöstä poistetut tilit

Käytöstä poistetut tilit synkronoidaan myös Azure AD. Käytöstä poistetut tilit ovat yleisiä edustavan Exchange, kuten neuvotteluhuoneiden resurssit. Poikkeus on käyttäjät, joilla on linkitetty postilaatikkoon; kuten edellä mainittiin ne koskaan valmistella Azure AD tilin.

Olettaen on, että jos käytöstä käyttäjätilille löytyy, sitten olemme ei löydä toisen aktiivinen tili myöhemmin ja objekti on valmisteltu Azure AD userPrincipalName ja sourceAnchor löydy. Siltä varalta, että toinen aktiivinen tili liittää metaverse samaan objektiin, valitse sen userPrincipalName ja sourceAnchor käytetään.

## <a name="changing-sourceanchor"></a>SourceAnchor muuttaminen

Kun objekti on viety Azure AD sitten sitä ei voi muuttaa sourceAnchor enää. Kun objekti on viety metaverse määrite **cloudSourceAnchor** asetuksena on hyväksynyt Azure AD **sourceAnchor** arvo. Jos **sourceAnchor** on muutettu ja vastaa **cloudSourceAnchor**, **AAD – käyttäjän liittyä ulos** säännön palauttaa virheen **sourceAnchor-määrite on muuttunut**. Tässä tapauksessa määritysten tai tietoja on korjattava, niin saman sourceAnchor ei ole metaverse uudelleen ennen objektin voidaan synkronoida uudelleen.

## <a name="additional-resources"></a>Lisäresursseja

* [Azure AD yhteyden synkronointi: Synkronoinnin asetusten mukauttamisen](active-directory-aadconnectsync-whatis.md)
* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)
