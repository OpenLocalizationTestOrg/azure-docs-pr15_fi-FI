<properties
   pageTitle="Azure AD Connect: Päivittäminen aiemmasta versiosta | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan eri tavat päivittämiseksi Azure Active Directory yhteyden, mukaan lukien paikallaan tapahtuva päivitys ja kääntymiskulma siirto uusimman version."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Ksi aiemmasta versiosta viimeistään
Tässä ohjeaiheessa kerrotaan eri tavoista avulla voit päivittää Azure AD Connect asennusta uusimman version. On suositeltavaa, että pidät itse nykyisen Azure AD Connect versioiden kanssa. [Siirron iskettävä](#swing-migration) kuvattujen vaiheiden käytetään myös, kun teet merkittäviä määrittäminen, muuttaminen.

Jos haluat päivittää DirSync-kohdassa [päivittää Azure AD-synkronointityökalu (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) .

Muutamalla eri tavoin päivittäminen Azure AD Connect.

Menetelmä | Kuvaus
--- | ---
[Automaattinen päivitys](active-directory-aadconnect-feature-automatic-upgrade.md) | Asiakkaat, joiden pika-asennus tämä on helpoin tapa.
[Paikallaan tapahtuva päivitys](#in-place-upgrade) | Jos sinulla on yhtenäiset, Päivitä-asennuksen käytönaikainen samaan palvelimeen.
[Kääntymiskulma siirto](#swing-migration) | Kaksi-palvelimet-voit valmistella uusien tai määritysten palvelimia ja muuttaa active server, kun olet valmis.

Katso tarvittavia oikeuksia [päivitystä varten tarvittavat käyttöoikeudet](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade).

## <a name="in-place-upgrade"></a>Paikallaan tapahtuva päivitys
Paikallaan tapahtuva päivitys toimii Azure AD-synkronointi tai Azure AD Connect siirtämistä varten. Se ei toimi DirSync tai FIM + Azure AD-yhdistin-ratkaisun.

Tätä menetelmää suositellaan, kun käytössä on yhteen palvelimeen ja pienempi kuin noin 100 000 objekteja. Jos määritettynä on ruutuun synkronoinnin säännöt tehdyt muutokset, koko tuominen ja täyden käyttäjätietojen ilmetä päivityksen jälkeen. Näin varmistat, että uudet asetukset otetaan käyttöön järjestelmän kaikki objektit. Tämä saattaa kestää muutaman tunnin objektien lukumäärän mukaan sync engine alueen. Tavallinen delta-synkronoinnin ajoitus oletusarvoisesti 30 minuutin välein keskeytetään, mutta salasanojen synkronoinnin edelleen. Harkitse tekemään paikallaan tapahtuva päivitys viikonlopun aikana. Jos muutoksia ei ole ruutuun työnkulkuun Azure AD Connect uusien kanssa, Normaali delta tuominen ja synkronointi alkaa sen sijaan.  
![Paikallaan tapahtuva päivitys](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Jos olet tehnyt muutoksia ruutuun synkronoinnin säännöt, nämä asetetaan takaisin oletusarvon kokoonpanon päivitettäessä. Varmista, että kokoonpanosi on käytettävissä päivitykset välillä, varmista, että muutokset tehdään kuvatulla tavalla [parhaat käytännöt muuttaminen oletusarvo-määritys](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

## <a name="swing-migration"></a>Kääntymiskulma siirto
Jos sinulla on monimutkainen käyttöönoton tai hyvin useita objekteja, voi olla hankalaa käytönaikainen päivittäminen live-järjestelmässä. Tämä voi joissakin asiakkaiden kestää useita päiviä ja tänä aikana delta muutoksia käsitellään. Tätä menetelmää käytetään myös silloin, kun aiot tekevät merkittäviä muutoksia kokoonpanosi ja haluat kokeilla näitä siirretty pilveen ennen.

Näissä tilanteissa suositeltava tapa on kääntymiskulma siirron. Tarvitset (vähintään) kaksi palvelimissa, yksi aktiivinen ja yksi väliaikaisen palvelin. Active server (yhtenäiset siniset viivat alla olevassa kuvassa) on vastuussa aktiivinen tuotannon Lataa. Uusien tai määritys on valmis väliaikaisen server (Violetti katkoviivat alla olevassa kuvassa) ja kun kaikki on valmista täysin tämä palvelin on valittavissa. Edellisen active server-nyt vanha versio tai määritysten asennettu, ja tehdä väliaikaisen palvelimeen ja päivittää.

Kahden palvelimen voit käyttää eri versioita. Esimerkiksi aiot poistetaan käytöstä active server käyttää Azure AD-synkronointi ja uuden väliaikaisen palvelimen käyttää Azure AD Connect. Jos kääntymiskulma siirron avulla voit tehdä uuden määrityksen on hyvä samoja versioita on kaksi-palvelimiin.  
![Väliaikaisen server](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Huomautus: On ollut huomattava asiakkaita mieluummin on kolme tai neljä palvelimia tässä tapauksessa. Kun väliaikaisen palvelin on päivitetty, sinulla ei ole varmuuskopion palvelimen, jos [tietojen palauttaminen](active-directory-aadconnectsync-operations.md#disaster-recovery). Kolme tai neljä palvelinten kanssa voidaan valmistaa joukkona ensisijainen/valmius-palvelimien, uudella versiolla, varmistaa, että ovat aina väliaikaisen palvelimen ryhtyä kirjoittamaan.

Siirry Azure AD-synkronointi tai FIM + Azure AD-yhdistin-ratkaisun toimii myös seuraavasti. Nämä vaiheet eivät toimi Dirsync-työkalun, mutta saman kääntymiskulma (kutsutaan myös rinnakkaisia käyttöönoton) siirron menetelmä vaiheisiin DirSync löytyy [päivittäminen Azure Active](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)Directoryn synkronointi (DirSync).

### <a name="swing-migration-steps"></a>Kääntymiskulma siirron vaiheet

1. Jos käytät Azure AD Connect sekä palvelimessa ja kannattaa tehdä vain määritysten muutos, varmista, että active server ja väliaikaisen palvelimen molemmat käyttävät samaa versiota. Joka helpottaa vertailla myöhemmin. Jos olet päivittämässä Azure AD-synkronointi, seuraavissa palvelimissa on eri versioita. Jos päivität Azure AD Connect vanhempaa versiota, se on hyvä aloittaminen saman version kuin kahden palvelimen kanssa, mutta sitä ei tarvita.
2. Jos olet tehnyt joitakin Mukautettu määritys ja väliaikaisen palvelin ei ole se, valitse [Siirrä Mukautettu määritys aktiivisen tietoyhteyksien](#move-custom-configuration-from-active-to-staging-server)noudattamalla.
3. Jos olet päivittämässä Azure AD Connect aiemman version, Päivitä väliaikaisen palvelimen uusimpaan versioon. Jos siirrät Azure AD-synkronointi, asenna Azure AD Connect väliaikaisen palvelimeen.
4. Anna synkronointi-ohjelma suorittaa täysi tuominen ja täyden käyttäjätietojen väliaikaisen palvelimeen.
5. Varmista, että uudet määritykset aiheuta ohjeiden **Vahvista** -kohdassa [Tarkista palvelimen määritysten](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server)odottamattomia muutoksia. Jos jokin ei ole yhtä odotettu, oikea, Suorita tuonti ja synkronoi ja varmista ennen tietojen näyttää hyvältä. Nämä vaiheet löytyvät linkitettyä aihetta.
6. Vaihda väliaikaisen palvelimen active server. Tämä on viimeisessä vaiheessa **vaihtaa active server** vahvistaminen [palvelimen määrityksiä](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Jos olet päivittämässä Azure AD Connect, Päivitä palvelimen nyt väliaikaisen tilassa uusimman version. Hae tiedot ja määritykset päivittää noudattamalla samalla tavalla kuin ennen. Jos olet päivittänyt Azure AD-synkronointi, voit nyt käytöstä ja poistetaan käytöstä vanha palvelimellesi.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Siirrä Mukautettu määritys aktiivisen tietoyhteyksien
Jos olet tehnyt määritysmuutoksia active server, joudut Varmista, että sama tehdyt muutokset tulevat voimaan väliaikaisen palvelimeen.

Olet luonut mukautettuja synkronointi sääntöjä voidaan siirtää PowerShellin avulla. Muut muutokset on otettava käyttöön molemmissa samalla tavalla ja ei voi siirtää.

Varmista asia on määritetty molemmissa palvelimissa samalla tavalla:

- Saman metsien yhteyden.
- Mikä tahansa toimialueen ja OU suodatus.
- Saman valinnaisia ominaisuuksia, kuten salasanan synkronointi ja salasanan takaisinkirjoituksen.

**Siirrä synkronoinnin säännöt**  
Siirrä mukautettu synkronoinnin säännön, toimi seuraavasti:

1. Avaa **Synkronoinnin säännöt editorin** aktiivinen palvelimeen.
2. Valitse mukautetun säännön. Valitse **Vie**. Tämä näyttää Muistio-ikkuna. Tallenna tilapäinen tiedosto PS1-tunniste. Näin PowerShell-komentosarjaa. Kopioi ps1 tiedosto väliaikaisen palvelimeen.  
![Synkronoi säännön vienti](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Yhdistimen GUID-tunnus on eri palvelimella ja on vaihdettava. Saat GUID-tunnus-käynnistää **Synkronoinnin säännöt editorin**, jompikumpi samassa yhdistetyn järjestelmää edustava ruutuun säännöt ja valitse **Vie**. Korvaa GUID-tunnus PS1 tiedostossa GUID-tunnus väliaikaisen palvelimesta.
4. PowerShell-kehotteessa Suorita PS1-tiedosto. Tämä luo mukautettu synkronoinnin säännön palvelimella.
5. Jos sinulla on useita mukautettuja sääntöjä, toista kaikki mukautetut säännöt.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
