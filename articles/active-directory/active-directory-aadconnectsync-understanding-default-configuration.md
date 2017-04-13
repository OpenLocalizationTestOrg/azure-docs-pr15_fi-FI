<properties
    pageTitle="Azure AD Connect synkronointi: tietoja oletusarvon määrittäminen | Microsoft Azure"
    description="Tässä artikkelissa kuvataan Azure AD Connect synkronointi oletusarvo-määritys."
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
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect synkronointi: tietoja oletusarvon määrittäminen
Tässä artikkelissa kerrotaan ruutuun määritys-sääntöjä. Se tiedostot sääntöjä ja miten sääntöjen vaikuttaa määritykset. Se myös esitellään Azure AD Connect synkronointi oletusarvon määrittäminen. Lähinnä lukija ymmärtää, kuinka määritys mallin nimeltä määritettäviä valmistelu toimii tosielämän esimerkki. Tässä artikkelissa oletetaan, että olet jo asentanut ja Määritä Azure AD Connect synkronointi ohjatun asennuksen avulla.

Selvittääksesi määritys-mallin tietoja, lue [Tietoja määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Ruutu säännöt-paikallisen Azure AD
Seuraavat lausekkeet löytyvät ruutuun määritys.

### <a name="user-out-of-box-rules"></a>Käyttäjän ruutuun säännöt
Sääntöjen käyttää myös inetOrgHenkilö objektilaji.

User-objekti on täytettävä seuraavat voidaan synkronoida:

- On oltava sourceAnchor.
- Kun objekti on luotu Azure AD-sourceAnchor ei voi muuttaa. Jos arvo on muutettu paikallisen, objektin lopettaa synkronoinnin, kunnes sourceAnchor muuttuu takaisin edelliseen arvonsa.
- Täytyy olla accountEnabled (userAccountControl)-määritteen kaikille. Paikallisen Active Directory-määrite on aina esitä ja täyttää.

Käyttäjä-objekteja **ei ole** synkronoitu Azure AD:

- `IsPresent([isCriticalSystemObject])`. Varmista monta ruutuun-objektit Active Directoryssa, kuten sisäisen järjestelmänvalvojatilin, ei ole synkronoitu.
- `IsPresent([sAMAccountName]) = False`. Varmista, että käyttäjän objektit, joissa ei ole sAMAccountName-määrite ei ole synkronoitu. Tässä tapauksessa vain käytännössä käydä NT4 päivittänyt toimialue.
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Azure AD Connect synkronointi ja sen aiempien versioiden palvelutilin ei synkronoida.
- Ei synkronoida Exchange-tilien, joka ei toimi Exchange online-tilassa.
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- Objektit, joissa ei toimi Exchange Online ei synkronoida.
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
Tämä bittipeitteen (ja H21C07000) suodattaa seuraaville objekteille:
    - Sähköpostia käyttävä julkiseen kansioon
    - Järjestelmän Puhelunvälittäjän postilaatikko
    - Postilaatikon tietokannan postilaatikon (järjestelmä postilaatikon)
    - Yleinen käyttöoikeusryhmän (ei käytä käyttäjän, mutta ei vanhoja syistä)
    - Muut Universal ryhmän (ei käytä käyttäjän, mutta ei vanhoja syistä)
    - Postilaatikon suunnitelma
    - Etsintäpostilaatikko-toimintoon
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Replikoinnin uhri objekteja ei synkronoida.

Voimassa seuraavat määritesäännöt:

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. SourceAnchor-määritettä ei ole käytetty linkitetyn postilaatikosta. Oletetaan, että jos linkitetyn postilaatikon löydetty todellinen tili on yhdistetty myöhemmin.
- Exchange, jotka liittyvät attribuutit synkronoidaan vain, jos arvo on määrite **mailNickName** .
- Kun useiden puuryhmien määritteet kulutettu seuraavassa järjestyksessä:
    1. Ominaisuudet, jotka liittyvät sisään (esimerkiksi userPrincipalName) on lähettänyt-metsää käytössä tilillä.
    2. Määritteitä, jotka löytyvät Exchange-GAL (Yleinen osoiteluettelo) on lähettänyt-metsää Exchange-postilaatikon kanssa.
    3. Jos postilaatikon ei löydy, valitse seuraavanlaisia määritteitä voi tulla minkä tahansa metsää.
    4. Exchange liittyvät määritteet (sitä ei näy tekniset määritteitä) on lähettänyt metsää-kohtaa, johon `mailNickname ISNOTNULL`.
    5. Jos määritettynä on useita metsien, jotka täyttävät jonkin sääntöjen, yhdistimet (forests) luominen järjestyksen (päivämäärä ja kellonaika) käytetään voit selvittää, mitkä metsää prosentuaalista määritteet.

### <a name="contact-out-of-box-rules"></a>Ota ruutuun säännöt
Yhteyshenkilön objekti on täytettävä seuraavat voidaan synkronoida:

- Yhteyshenkilö on oltava sähköpostikäyttöiset. Se on vahvistettu seuraavien sääntöjen mukaisesti:
    - `IsPresent([proxyAddresses]) = True)`. ProxyAddresses-määritteeseen on täytettävä.
    - Ensisijaisen sähköpostiosoitteen löytyy proxyAddresses-määritteeseen tai sähköposti-määrite. Esiintyminen @ avulla voit tarkistaa, että sisältö on sähköpostiosoite. Yksi kaksi sääntöjen on arvioitava tosi.
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Onko merkinnän "SMTP:" ja jos on, Voinko @ merkkijonon löydy?
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Posti-määritteen täydennetty ja jos se on, voit @ merkkijonon löydy?

Yhteyshenkilön seuraaville objekteille on **synkronoitu Azure AD** :

- `IsPresent([isCriticalSystemObject])`. Varmista yhteyshenkilön objekteja ei ole merkitty kuin kriittinen synkronoidaan. Ei pitäisi olla mikä tahansa oletusasetukset kanssa.
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Nämä objektit ei toimi Exchange online-tilassa.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Replikoinnin uhri objekteja ei synkronoida.

### <a name="group-out-of-box-rules"></a>Ryhmän valinta säännöt
Ryhmän objektin on täytettävä seuraavat voidaan synkronoida:

- On oltava pienempi kuin 50 000 jäsenet. Tämä määrä on paikallisen ryhmän jäsenten määrä.
    - Jos siinä on enemmän jäseniä ennen synkronointi käynnistyy ensimmäisen kerran, ryhmä ei ole synkronoitu.
    - Jos jäsenten määrä kasvaa, kun se on luotu, valitse päättymispäivänä 50 000 jäsenet-lopettaa synkronoinnin, kunnes jäsenyyden määrä on pienempi kuin 50 000 uudelleen.
    - Huomautus: Azure AD myös toimialuerekisteröintipalvelun 50 000 jäsenyyden määrä. Et voi synkronoida jäseniä ryhmiin, vaikka voit muokata tai poistaa tämän säännön.
- Jos ryhmä on **Jakeluryhmä**, valitse se on myös oltava käytössä sähköposti. Katso tämän säännön [yhteyshenkilön ruutuun säännöt](#contact-out-of-box-rules) otetaan käyttöön.

Ryhmä-objekteja **ei ole** synkronoitu Azure AD:

- `IsPresent([isCriticalSystemObject])`. Varmista monta ruutuun-objektit Active Directoryssa, kuten sisäiset Järjestelmänvalvojat-ryhmän, ei ole synkronoitu.
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Aiempien versioiden käyttämän DirSync-ryhmä.
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Rooliryhmän.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Replikoinnin uhri objekteja ei synkronoida.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal valinta säännöt
FSPs liitettyinä "johonkin" (\*) metaverse objektia. Todellisuudessa tämän liity tapahtuu vain käyttäjät ja ryhmät. Määritysten varmistaa, että toimialueiden väliset jäsenyydet ratkennut ja esittää oikein Azure AD.

### <a name="computer-out-of-box-rules"></a>Tietokoneen ruutuun säännöt
Tietokone-objekti on täytettävä synkronoitava seuraavasti:

- `userCertificate ISNOTNULL`. Windows 10-tietokoneet täyttää tämän määritteen. Tietokoneen kaikki objektit tämän määritteen arvon on synkronoitu.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Tietoja muiden valmistajien säännöt-skenaario
Tässä esimerkissä on käytössä-ympäristö, jonka yhden tilin metsää (A), yksi resurssi metsää (R) ja yksi Azure AD-kansio.

![Kuva, jossa on skenaarion kuvaus](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

Tässä määrityksessä oletetaan on otettu käyttöön tilin tilin metsää ja resurssin metsää linkitetyn postilaatikon on ei käytössä-tili.

Tavoitteenamme oletusarvon määrityksissä on:

- Ominaisuudet, jotka liittyvät sisään synkronoituvat metsää käytössä olevan tilin kanssa.
- Määritteitä, jotka löytyvät sitä (Yleinen osoiteluettelo) on synkronoitu metsää postilaatikkoon. Jos postilaatikon ei löydy, muut metsää käytetään.
- Jos linkitetty postilaatikkoa löydy, käytössä linkitetty asiakas on löydy vietävän Azure AD-objektin.

### <a name="synchronization-rule-editor"></a>Synkronoinnin sääntö-editori
Määritystä voi tarkastella ja muuttaa synkronoinnin säännöt Editor (SRE)-työkalulla ja sen pikakuvakkeen löydy aloitusvalikkoon.

![Synkronoinnin säännöt editori-kuvake](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

Se on asennettu Azure AD Connect synkronoinnin SRE on resurssin kit-työkalu. Jos haluat aloittaa sen, on oltava ADSyncAdmins-ryhmän jäsen. Kun se käynnistyy, näet seuraavankaltaiselta:

![Synkronoinnin säännöt saapuva](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Tässä ruudussa näet kaikki synkronoinnin säännöt, jotka on luotu kokoonpanon. Taulukon kullakin rivillä on yksi synkronoinnin sääntö. Valitse sääntötyypeistä vasemmalle kahta erilaista näkyvät: saapuvan ja lähtevän. Saapuva ja lähtevä on metaverse näkymästä. Pääasiassa aiot kohdistus käyttöön tässä yleiskatsauksessa saapuvan liikenteen säännöt. Synkronoinnin todellinen sääntöluetteloa määräytyy olemassa rakenteen AD. Yllä olevassa kuvassa tilin metsää (fabrikamonline.com) ei ole mitään palvelut, kuten Exchange ja Lync- ja synkronointisääntöjä ei ole luotu palvelun. Kuitenkin-resurssin metsää (res.fabrikamonline.com) löydät synkronoinnin säännöt palvelun. Sääntöjä sisältö ei ole havaittu version mukaan. Esimerkiksi Exchange 2013-yhdistelmäympäristössä on useita määritettä kulkee määritetty kuin Exchange 2010 ja 2007: ssä.

### <a name="synchronization-rule"></a>Synkronoinnin sääntö
Synkronoinnin sääntö on joukko määritteitä, kun ehto täyttyy juoksutus kokoonpano-objekti. Sitä käytetään myös kuvataan, miten objektin yhdistimen välilyönnillä liittyvät objektin metaverse **Liity** tai **vastaa**. Synkronoinnin sääntöjen on ensisijainen arvo, joka ilmaisee, miten ne liittyvät toisiinsa. Synkronoinnin sääntö, jolla pienempi numeerinen arvo on suurempi järjestys ja määritettä työnkulku ristiriita laskettavat voittaa ristiriitojen ratkaisu.

Esimerkkinä, tarkista synkronoinnin säännön **-AD – käyttäjän AccountEnabled-**. Merkitse tämä SRE rivi ja valitse sitten **Muokkaa**.

Tämä sääntö on ruutuun säännön, näyttöön tulee varoitus, kun avaat sääntö. Tee haluamasi [muutokset ruudussa sääntöjä](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)niin, että sinulta kysytään, mitä aikeesi ovat. Tässä tapauksessa vain haluat tarkastella. Valitse **ei**.

![Synkronoinnin säännöt varoitus](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Synkronoinnin sääntö on neljä määritysten osaa: kuvaus-suodatin, liity säännöt ja muunnoksia vaikutusalueen määrittäminen.

#### <a name="description"></a>Kuvaus
Ensimmäinen osa on perustiedot, kuten nimi ja kuvaus.

![Kuvaus välilehti synkronoinnissa Sääntöeditori ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Voit myös liittyvien tietojen etsiminen siitä, mitä yhdistetyn järjestelmän tämä sääntö on, jossa objektin toiminto on yhdistetty järjestelmän tyyppi ja metaverse objektilaji. Metaverse objektilaji on aina henkilön riippumatta siitä, kun objektin lähdetyyppi on käyttäjän, inetOrgHenkilö tai yhteyshenkilön. Metaverse objektilaji olisi koskaan muuttua, joten se luodaan yleinen tyyppi. Linkkityyppi voi määrittää liitoksen, StickyJoin tai säännöstä. Tämä asetus yhdessä liittyä säännöt-osassa ja käsitellään myöhemmin.

Näet myös, että sync tätä sääntöä käytetään salasanan synkronointi. Jos käyttäjä on laajuus Synkronoi tämän säännön, salasana on synkronoitu paikallisen-pilveen (olettaen että salasanan synkronointi-ominaisuus on otettu käyttöön).

#### <a name="scoping-filter"></a>Vaikutusalueen määrittäminen suodatin
Vaikutusalueen määrittäminen suodatin-osassa käytetään määrittämistä kun synkronointi sääntö on voimassa. Koska tarkasteltavan synkronoinnin säännön nimi ilmaisee, sitä käytetään vain käytössä käyttäjät-alue on määritetty siten AD-määrite **userAccountControl** ei saa sisältää 2-bittinen määrittäminen. Kun synkronointi-ohjelma etsii käyttäjän AD-toiminto on Synkronoi tämän säännön kun **userAccountControl** on desimaaliarvon 512 (Normaali käyttäjän käytössä). Se ei koske sääntö on **userAccountControl** asettaminen 514 (Normaali käyttäjä ei käytössä).

![Vaikutusalueen määrittäminen synkronointi säännön editori-välilehti ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Tarkasteltavan suodatin on, ryhmät ja lausekkeita, jotka voivat olla ylemmän tason. Kaikkien lauseiden ryhmän sisällä on täytyttävä synkronoinnin sääntöä sovelletaan. Kun useita ryhmiä on määritetty, valitse vähintään yksi ryhmän on täytettävä, jotta sääntöä sovelletaan. Toisin sanoen looginen OR arvioidaan ryhmiä ja loogisten välillä ja arvioidaan ryhmän sisällä. Esimerkki määritysten löytyy lähtevän säännön synkronoinnin **ulos AAD – ryhmään liittyminen**. On useita synkronoinnin suodattimen ryhmissä, esimerkiksi yksi käyttöoikeusryhmät (`securityEnabled EQUAL True`) ja toinen jakeluryhmien (`securityEnabled EQUAL False`).

![Vaikutusalueen määrittäminen synkronointi säännön editori-välilehti ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Tätä sääntöä käytetään, voit määrittää, mitkä ryhmät olisi valmisteltava Azure AD. Jakeluryhmät, jotka on oltava käytössä, jotta voidaan synkronoida Azure AD sähköposti, mutta suojausryhmien sähköpostia ei ole pakollinen.

#### <a name="join-rules"></a>Liittyminen säännöt
Kolmas osa määrittää, kuinka yhdistin tilaa objektien liittyvät objektit metaverse käytetään. Säännön, sinun on tarkastellut aiemmin ei ole tahansa kokoonpanon liittyä sääntöjen niin sijaan aiot katsomalla **-AD – käyttäjän Join-**.

![Liittyminen säännöt-välilehden Synkronoi säännön editorissa ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

Liity säännön sisältö määräytyy vastaavia valittuna ohjatun asennuksen. Saapuvan liikenteen sääntö laskennan alkaa objektin lähde connector-tilaa ja liity sääntöjä kunkin ryhmän arvioidaan järjestyksessä. Jos lähdeobjekti arvioidaan täsmälleen yhden objektin jollakin liity sääntöjä metaverse vastaamaan, objektien liitettyinä. Jos kaikki säännöt arvioitu ja ei ole sama kuvaus-sivulla linkkityyppi käytetään. Jos tämä on määritetty **valmistelu**, uusi objekti luodaan kohde metaverse. Uuden objektin ja metaverse valmistelu kutsutaan myös **projektiin** objektin metaverse.

Liity säännöt käsitellään vain kerran. Kun yhdistimen tilaa-objekti ja metaverse-objekti on liitetty, ne pysyvät liitettyjen, kunhan synkronoinnin säännön alue on edelleen täyttyvät.

Synkronoinnin säännöt laskettaessa laajuus on oltava vain yksi synkronoinnin sääntö ja liity-sääntöjä, jotka on määritetty. Jos useita synkronoinnin sääntöjä liity sääntöjen löytyy yhden objektin, ilmenee virhe. Tästä syystä paras käytäntö on vain yksi synkronoinnin sääntö on määritetty, kun useita synkronoinnin säännöt ovat objektin vaikutusalueen liittää. Azure AD Connect synkronointi ruutuun määrityksessä sääntöjen voi löydy katsomalla nimi ja Etsi sisältävän nimen lopussa **liittyä** word. Synkronoinnin säännöllä ei ole määritetty liity sääntöjä Lisää määrite työnkulut, kun toinen synkronoinnin sääntö liitetty objektit yhteen tai valmisteltu uusi kohde-objekti.

Jos tarkastelet kuvassa, näet säännön yrittää liittyä **objectSID** **msExchMasterAccountSid** (Exchange) ja **msRTCSIP OriginatorSid** (Lync), joka on on odotettavissa tilin resurssi-metsää topologian. Löydät kaikki metsiin saman säännön. Olettaen on, että jokaisen metsää voi olla joko asiakas- tai resurssien metsää. Tämä asetus toimii myös halutessasi tilit, jotka asuvat yksittäisen metsää ja ei tarvitse olla.

#### <a name="transformations"></a>Muunnokset
Muunnoksen osa määrittää kaikkien määrite työnkulkujen, jotka koskevat kohdeobjektin objektit liitettyinä ja laajuus-suodattimen täyttyy. Palaaminen **-AD – käyttäjän AccountEnabled-** synkronointi säännön löydät seuraavat muunnokset:

![Muunnokset välilehti synkronoinnissa Sääntöeditori ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Vie määritysten konteksti-tilin resurssin metsää käyttöönoton, odotetaan löydät käytössä tilin metsää ja käytöstä poistetun tilin resurssin metsää Exchange ja Lync-asetusten kanssa. Tarkasteltavan synkronoinnin sääntö sisältää määritteet-Kirjautuminen vaaditaan ja seuraavanlaisia määritteitä olisi siirtyvät kohteesta metsää ei käytössä. Kaikki nämä määrite työnkulut ovat koonneet yksi synkronoinnin sääntö.

Muunnoksen voi olla eri: Vakio, suorien ja lauseke.

- Tasainen virtaus jatkuu aina englanninkielisissä arvon. Yllä tapauksessa aina määrittää metaverse **Tosi** arvo määrite nimeltä **accountEnabled**.
- Suora työnkulku aina jatkuu kuin kohde-määritteeseen lähde-määritteen arvo-on.
- Kolmas työnkulkutyyppi on lausekkeen ja mahdollistaa kehittyneempiä määrityksiä.

Lausekkeen kieli on Microsoft Office, jolla on kokemusta ihmisiä niin VBA (Visual Basic for Applications)- tai VBScript tunnistaa muoto. Määritteet on kirjoitettava hakasulkeisiin, [attributeName]. Nimiä ja funktioiden nimien kirjainkoko on merkitsevä, mutta synkronoinnin säännöt editorin arvioi lausekkeen ja antaa varoituksen, jos lauseke ei ole kelvollinen. Kaikki lausekkeet ilmaistaan yhdellä rivillä sisäkkäisten funktioiden kanssa. Näyttämään määritys-kieli power näin pwdLastSet, mutta muita kommentit on lisätty työnkulku:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Katso [Tietoja määritettäviä valmistelu lausekkeiden](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) lisätietoja määrite kulkee lausekielellä.

### <a name="precedence"></a>Järjestys
Olet nyt tarkastellut yksittäisiä synkronoinnin sääntöjä, mutta sääntöjä yhteiskäyttö määritykset. Joissakin tapauksissa määrite-arvo on lähettänyt useita synkronoinnin sääntöjen sama kohdemäärite. Tässä tapauksessa voit selvittää, mitkä määrite voittaa käytetään määrite ohittaa. Esimerkkinä Tarkista määrite sourceAnchor. Tämän määritteen on tärkeää määrite voivan Azure AD kirjautuminen. Löydät määrite työnkulun kahdella eri synkronoinnin sääntöjä tämän määritteen **-AD – käyttäjän AccountEnabled-** ja **-AD – käyttäjän yleiset-**. Vuoksi synkronoinnin sääntöjen käsittelyjärjestyksen sourceAnchor määritettä on lähettänyt käytössä tilillä metsää-ensin kun on useita objekteja, jotka on liitetty metaverse-objekti. Jos ei ole käytössä tilejä ja valitse Synkronoi-ohjelma käyttää todellisen kaikki synkronoinnin sääntö **-AD – käyttäjän yleiset-**. Tässä määrityksessä voit varmistaa, että myös tilit, jotka eivät ole käytössä, saat edelleen sourceAnchor.

![Synkronoinnin säännöt saapuva](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Synkronoinnin sääntöjen käsittelyjärjestyksen on määritetty ryhmien ohjatun asennuksen. Kaikki ryhmän sääntöjen olla samaa nimeä, mutta ne ovat yhteydessä eri yhdistetyn kansiot. Asennuksen ohjatussa toiminnossa säännön **-AD – käyttäjän Join-** suurimmat järjestys ja sen iteroivaa over kaikki yhdistetty AD kansioita. Se säilyy sitten sääntöjen tietyssä järjestyksessä seuraava ryhmiin. Ryhmän sisällä säännöt lisätään yhdistimet on lisätty ohjatussa järjestyksessä. Jos toinen yhdistin lisätään ohjatussa toiminnossa, synkronointisääntöjä on tilattava ja uuden yhdysviivan säännöt lisätään viimeksi kunkin ryhmän.

### <a name="putting-it-all-together"></a>Kaikkien tietojen yhdistäminen
On nyt tietää riittävästi tietoja synkronoinnin säännöt voivat ymmärtää, kuinka määritykset toimii eri synkronointi-sääntöjä. Jos tarkastelet käyttäjän ja määritteitä, jotka on lähettänyt metaverse säännöt otetaan käyttöön seuraavassa järjestyksessä:

Nimi | Kommentti
:------------- | :-------------
Valitse AD – käyttäjän liity: stä | Kirjautumistietojen metaverse yhdistimen tilaa objektien sääntö.
Valitse Valitse AD – UserAccount käytössä | Kirjautuminen Azure AD vaaditaan määritteet ja Office 365: ssä. Haluamme seuraavanlaisia määritteitä käytössä-tililtä.
Valitse AD – käyttäjän yhteinen Exchangesta: stä | Löytyi yleisestä osoiteluettelosta määritteet. Tietojen laadun parhaiten metsää, jossa on löytänyt käyttäjän postilaatikon oletetaan.
Valitse AD – käyttäjän yhteinen: stä | Löytyi yleisestä osoiteluettelosta määritteet. Siltä varalta, että postilaatikkoa ei löytynyt, liitettyjen objektin voi edistää määrite-arvo.
Valitse AD – käyttäjän Exchange- | On olemassa vain, jos Exchange on jo olemassa. Jatkuu kaikki infrastruktuurin Exchange määritteet.
Valitse AD – käyttäjän Lync: stä | On olemassa vain, jos Lync on jo olemassa. Jatkuu kaikki infrastruktuurin Lync määritteet.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja on artikkelissa [Tietoja määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning.md)määritys mallin.
- Lisätietoja on artikkelissa [tietoja määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)lausekkeissa lausekielellä.
- Jatka lukemista ruutuun määritysten toiminta [tietoja käyttäjät](active-directory-aadconnectsync-understanding-users-and-contacts.md) ja yhteystiedot
- Katso, miten voit muuttaa käytännön avulla [Voit tehdä muutoksia työnkulkuun oletusarvon](active-directory-aadconnectsync-change-the-configuration.md)määritettäviä valmistelu.

**Yleistä aiheita**

- [Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi](active-directory-aadconnectsync-whatis.md)
- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)
