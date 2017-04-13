<properties
    pageTitle="Azure AD Connect synkronointi: tekninen käsitteitä | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan Azure AD Connect synkronointi tekniset käsitteitä."
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


# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect synkronointi: tekninen käsitteitä
Tässä artikkelissa on ohjeaiheessa [tietoja arkkitehtuuri](active-directory-aadconnectsync-technical-concepts.md)yhteenveto.

Azure AD Connect synkronointi rakentuu tasainen metadirectory synkronointi-ympäristössä.
Seuraavissa osissa esitellä käsitteitä metadirectory synkronointia varten.
Perustuvat MIIS ILM ja FIM-Azure Active Directory-synkronointipalvelut on seuraava ympäristö tietolähteisiin yhdistämiseen, tietolähteiden sekä valmistelun tietojen synkronoiminen ja valmistelun poistaminen käyttäjätiedoista.

![Tekniset käsitteitä](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

Seuraavissa osissa on lisätietoja FIM synkronointipalvelu seuraavat seikat:

- Yhdistin
- Määritteen työnkulku
- Yhdistimen tilaa
- Metaverse
- Valmistelu

## <a name="connector"></a>Yhdistin

Koodimoduuleja, joita käytetään yhdistetyn hakemiston kanssa kommunikoimiseen kutsutaan yhdistimet (aiemmalta nimeltään hallinta tekijöiden (MAs)).

Ne on asennettu tietokoneeseen, jossa Azure AD Connect Synkronoi.
Yhdistimien antaa agentitonta mahdollisuus keskustella käyttämällä Etäjärjestelmä protokollat sen sijaan, että käyttäisit erityinen tekijöiden käyttöönotto. Tämä tarkoittaa pienentyä riski ja käyttöönotto-aikoja, erityisesti silloin, kun tärkeät sovellukset ja järjestelmien käsittely.

Yllä olevassa kuvassa yhdistin on connector-tilaa paremmin, mutta kattaa kaikki viestintä ulkoisen järjestelmän kanssa.

Yhdistimen on vastuussa kaikki tuominen ja vieminen järjestelmän toimintoja ja vapauttaa kehittäjät-hänen nimeään tarvitse lisätä tietoja yhteyden muodostamisesta kunkin järjestelmän grafiikkatiedostomuotoja, kun käytät tietojen muunnokset mukauttamiseen määritettäviä valmistelu.

Tuonnista ja viennistä ilmetä vain, kun salliminen edelleen eristyksen muutoksilta, määrä-järjestelmässä, koska muutoksia ei automaattisesti välittäminen yhdistetty tietolähteeseen. Kehittäjät myös lisäksi luoda omia yhdistimet lähes minkä tahansa tietolähteen yhteyden muodostamisesta.

## <a name="attribute-flow"></a>Määritteen työnkulku

Metaverse on kaikkien liitettyjen jäsenyyksien viereisistä yhdistimen välilyöntejä kootut näkymä. Edellä olevassa kuvassa määrite työnkulku edellisessä nuolenpäitä sekä saapuvan ja lähtevän kulkuun viivoilla. Määritteen on kopioiminen tai muuntaa tietoja järjestelmästä toiseen ja kaikki määritteisiin (saapuvat tai Lähtevät).

Määritteen työnkulku tapahtuu välinen yhdistin välilyönti ja metaverse bi-sijainnin synkronoinnin (täysi tai delta) varten ajoitetut.

Määritteen työnkulku tapahtuu vain silloin, kun nämä synkronoinnin suoritetaan. Määritteen kulkee määritetään synkronoinnin säännöt. Nämä voivat olla saapuva (ISR seuraavassa kuvassa) tai lähtevä (OSR seuraavassa kuvassa).

## <a name="connected-system"></a>Yhdistetyn järjestelmän

Yhdistetyn järjestelmän (eli yhdistetyn hakemiston) on viittaaminen Etäjärjestelmä Azure AD Connect synkronointi on yhdistetty ja lukemiseen ja kirjoittamiseen käyttäjätiedot ja sieltä.

## <a name="connector-space"></a>Yhdistimen tilaa

Kunkin yhdistetyn tietolähteen esitetään lukuna suodatetut alijoukko kohteet ja määritteet connector-tilaan.
Tällöin toimimaan paikallisesti sinun on otettava yhteyttä Etäjärjestelmä, kun synkronoit objektit ilman synkronointipalvelun ja rajoittaa vuorovaikutusta tuontiin ja vie vain.

Kun tietolähteen ja yhdistimen voi tarjota muutosluettelo (delta tuominen), valitse toiminnan tehokkuus kasvaa huomattavasti vain muutokset jälkeen kysely viimeinen jakso vaihdetaan. Yhdistimen tilaa insulates yhdistetyn tietolähteen muutoksilta välittäminen automaattisesti edellyttäen yhdistimen aikatauluun Tuo ja vie. Tämä lisätty vakuutus asiakkaalla lisäkäyttöoikeuksia voidaan hankkia testaus, esikatselun tai vahvistaminen seuraavaan päivitykseen.

## <a name="metaverse"></a>Metaverse

Metaverse on kaikkien liitettyjen jäsenyyksien viereisistä yhdistimen välilyöntejä kootut näkymä.

Käyttäjätietojen on linkitetty yhteen ja myöntäjä on määritetty eri määritteiden – Tuo työnkulku yhdistämismääritykset, keskitetty metaverse objektin alkaa kooste tietoja useista järjestelmistä. Tästä vuosta objektin määrite yhdistämismääritykset suorittaa lähtevä järjestelmien tiedot.

Objektit luodaan, kun tärkeimpien järjestelmän projektien niitä metaverse kyselyjä. Kun kaikki yhteydet on poistettu, metaverse objekti poistetaan.

Metaverse objekteja ei voi muokata suoraan. Objektin kaikissa tiedoissa on lähettänyt määrite työnkulku kautta. Metaverse ylläpitää pysyvä yhdistimien kunkin yhdistimen tilaa. Nämä yhdistimet ei tarvitse ymmärrettävä jokaisen synkronoinnin yhteydessä suoritetaan. Tämä tarkoittaa, että Azure AD Connect synkronointi ei ole Etsi vastaava remote objekti aina. Voit suojata muutoksilta määritteitä, jotka yleensä on vastuussa hajautettuna objektit kallista tekijöiden edellyttää ei tarvitse.

Kun Tutustu uusiin tietolähteisiin, joita saattaa olla aiemmin luotuja objekteja, jotka on tuleeko Azure AD Connect synkronointi käyttää kutsutaan liity säännön prosessin arvioida ehdokkaista, johon haluat muodostaa linkin.
Kun linkki on muodostettu, arvioinnissa toistu ja Normaali määrite työnkulku voi ilmetä remote yhdistetystä tietolähteestä ja metaverse välillä.

## <a name="provisioning"></a>Valmistelu

Kun luotettavalta projektien metaverse uuden yhdistimen tilaa objektin uuden objektiksi voidaan luoda toisen edustava edeltävät yhdistetty tietolähde yhdistin.

Tämä salliminen muodostaa linkin ja määritteen työnkulku voi jatkaa bi-sijainnin.

Aina, kun säännön määrittää, että uusi yhdistimen tilaa objekti on luotava, jonka nimi on valmistelu. Koska tämä toiminto vain tapahtuu yhdistimen tilan, se ei siirtää liitetään yhdistetyn tietolähteeksi kunnes Vie suoritetaan.

## <a name="additional-resources"></a>Lisäresursseja

* [Azure AD yhteyden synkronointi: Synkronoinnin asetusten mukauttamisen](active-directory-aadconnectsync-whatis.md)
* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
