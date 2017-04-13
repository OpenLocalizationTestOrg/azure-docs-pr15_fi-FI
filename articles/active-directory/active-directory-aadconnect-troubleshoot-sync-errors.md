<properties
    pageTitle="Azure AD Connect: Vianmääritys synkronoinnissa | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan vianmääritys Azure AD Connect-synkronoinnin aikana tapahtuneista virheistä."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>Käyttäjätietojen vianmääritys
Virheitä saattaa ilmetä, kun käyttäjätiedot synkronoidaan Windows Server Active Directory (AD DS) Azure Active Directory (Azure AD). Tässä artikkelissa on yleiskatsaus erityyppisiä synkronointivirheitä, jotkin mahdolliset skenaariot, jotka aiheuttavat näiden virheiden ja mahdolliset tapoja virheiden korjaamiseksi. Tämä artikkeli koskee virhe tavallisia, mutta ei voi kattaa kaikki mahdolliset virheet.

 Tässä artikkelissa oletetaan, että lukija tuntee pohjana olevan [suunnitella käsitteitä ja Azure AD-Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Azure AD Connect uusimman version kanssa \(elokuussa 2016 tai uudempi\), synkronointivirheiden raportti on käytettävissä [Azure Portal](https://aka.ms/aadconnecthealth) osana Azure AD yhteyden kunto-synkronointia varten.


Aloitetaan 2016 1 syyskuu [Azure Active Directory kaksoiskappaleet määrite Vikasietoisuudelle](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) ominaisuus otetaan oletusarvoisesti kaikki *uudet* Azure Active Directory-Alihallinnat. Tämä ominaisuus on automaattisesti käytettävissään aiemmin alihallinnat tulevien kuukausien.

Azure AD Connect suorittaa toimintojen 3 tyypit säilyttää synkronoinnissa kansioista: Tuo, synkronointi ja vie. Virheet voivat suorittaa kaikki toiminnot. Tässä artikkelissa keskitytään virheitä pääasiassa Azure AD Vie aikana.

## <a name="errors-during-export-to-azure-ad"></a>Vie Azure AD aikaiset virheet
Seuraavan osan kuvataan erityyppiset synkronointivirheitä, joka voi edetä Azure AD Azure AD-Connectorin avulla voit vientitoiminnon aikana. Tämä yhdistin voidaan tunnistaa nimen muoto, jota "contoso. *onmicrosoft.com*".
Vie Azure AD aikana virheitä ilmaisee, että toiminto \(lisääminen, päivittäminen ja poistaminen jne\) yrittäminen Azure AD Connect \(Sync Engine\) epäonnistui Azure Active Directory.

![Vie virheitä yleiskatsaus](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Tietojen ristiriita-virheet
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Kuvaus
- Kun Azure AD Connect \(sync engine\) ohjaa Azure Active Directory lisättävistä tai päivitettävistä objekteja, Azure AD vastaa saapuvan objektin Azure AD-objektien **immutableId** -määritettä **sourceAnchor** -määritteen avulla. Tämä vastine kutsutaan **Kova vastaa**.
- Kun Azure AD **ei löydä** objekti, joka vastaa **immutableId** määrite saapuvien objektin **sourceAnchor** -määritteen ennen valmistelu uuden objektin, se kuuluu takaisin käyttämään ProxyAddresses ja UserPrincipalName-määritteet vastinetta. Tämä vastine kutsutaan **Pehmeä vastaa**. Pehmeä vastaa on suunniteltu vastaamaan objektien jo Azure AD (jotka ovat Orpotermit Azure AD) on lisätty tai päivitetty synkronoinnissa uudet objektit, jotka esittävät paikallinen saman kohteen (käyttäjät ja ryhmät).
- **InvalidSoftMatch** virhe ilmenee, kun Kova vastine ei löydä **ja** Pehmeä vastaavat toisiaan vastaavat objektin löytää vastaavia objektin, mutta tämä objekti on eri *immutableId* kuin saapuvan objektin *SourceAnchor*, ehdottaminen vastaava objekti on synkronoitu toisen objektin paikallisesta Active Directory-arvosta.

Toisin sanoen järjestyksessä Pehmeä vastaavuuden toimimaan objektin Pehmeä sääntötunnus kanssa ei saa olla Jos *immutableId*arvo. Jos mikä tahansa objekti, jonka *immutableId* kanssa arvo ei ole vaikeaa vastine mutta täyttävän Pehmeä vastine ehdot, toiminto johtaa InvalidSoftMatch synkronoinnin virhe.

Azure Active Directory-rakenne ei salli useita objekteja, joilla on sama arvo määritteet. \(Tämä ei ole kattava luettelo.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- Objektitunnus

>[AZURE.NOTE] [Azure AD-määrite kaksoiskappaleiden määritettä Vikasietoisuudelle](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) -ominaisuus on myös käytössä esiin kuin Azure Active Directory oletustoiminnon.  Tämä pienentää tarkastella Azure AD Connect (sekä muut Synkronoi asiakkaat) synkronointivirheiden määrän tekemällä Azure AD Lisää joustavat kaksoiskappaleet ProxyAddresses ja UserPrincipalName määritteet paikalla olevat AD paikallisissa ympäristöissä tavasta. Tämä ominaisuus ei korjaa monistaminen virheet. Tietoja tarvitsee niin vielä kiinteiksi. Mutta sen avulla uusia objekteja, jotka muuten estetty valmistellaan vuoksi arvojen kaksoiskappaleet Azure AD-valmistelu. Tämä myös pienentää synkronointivirheiden synkronoinnin asiakkaalle luku.
Jos tämä ominaisuus on käytössä vuokraajan, et näe aikana valmistelu uusien objektien InvalidSoftMatch synkronointivirheitä.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Esimerkki tilanteita, joissa InvalidSoftMatch
1. Paikallisessa Active Directory on vähintään kaksi objektit, joissa on sama arvo ProxyAddresses-määritteeseen. Vain yksi on käytön valmistelu Azure AD.
2. Paikallisessa Active Directory on vähintään kaksi objektit, joissa on sama arvo userPrincipalName. Vain yksi on käytön valmistelu Azure AD.
3. Objektin lisättiin käytössä paikallisen Active Directory ProxyAddresses-määritteeseen saman arvon kuin alkuperäisen objektin Azure Active Directoryn. Paikallinen lisätty objekti ei ole käytön valmisteltu Azure Active Directoryn.
4. Objektin lisättiin paikallisesta Active Directory, joilla on sama arvo userPrincipalName-määritteen, Azure Active Directory-tiliä. Objekti ei ole käytön valmisteltu Azure Active Directoryn.
5. Synkronoidun tili on siirretty pois metsää metsää B. Azure AD Connect (sync engine) A on Laske SourceAnchor ObjectGUID-määritteen avulla. Kun metsää, siirrä SourceAnchor arvo on eri. Synkronoiminen aiemmin luodun objektin Azure AD-epäonnistuu (metsää B) uusi objektin.
6. Synkronoidun objektin käytössä vahingossa poistettiin paikallisesta Active Directory- ja uusi objekti on luotu Active Directory-saman kohteen (esimerkiksi käyttäjä) poistamatta Azure Active Directory-tilin. Uuden tilin ei synkronoida aiemmin Azure AD-objektin kanssa.
7. Azure AD Connect on poistanut ja asentaa uudelleen. Eri määrite valittiin uudelleen asennuksen aikana SourceAnchor. Kaikki objektit, jotka oli aiemmin synkronoidut pysäyttää InvalidSoftMatch virheen.

#### <a name="example-case"></a>Esimerkki tapaus:
1. **Teemu Smith** on synkronoitu Azure Active Directory paikallinen Active Directory *contoso.com* -käyttäjän
2. Teemu Smith **UserPrincipalName** on määritetty **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** on Azure AD Connect käytössä-Pekka Smith **objectGUID** avulla lasketaan **SourceAnchor** toimitilojen Active Directory-eli **immutableId** Teemu Smith Azure Active Directoryn varten.
4. Teemu on myös seuraavat arvot **proxyAddresses** -määritteeseen:
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  Uusi käyttäjä, **Teemu Taylorista**lisätään käytössä paikallisen Active Directory.
6. Teemu Taylorista **UserPrincipalName** on määritetty **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** on Azure AD Connect käytössä-Pekka Taylorista **objectGUID** avulla lasketaan **sourceAnchor** toimitilojen Active Directorysta. Teemu Taylorista objekti ei ole vielä on synkronoitu Azure Active Directory.
8. Teemu Taylorista on proxyAddresses-määritteeseen seuraavat arvot
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. Synkronoinnin aikana Azure AD Connect tunnista paikallisesta Active Directory Pekka Taylorista lisäämällä ja pyydä Azure AD saman muuttaa.
10. Azure AD ensin suorittaa Kova vastine. Toisin sanoen etsii Jos objektia immutableId on yhtä kuin "abcdefghijkl0123456789 ==". Kova vastine epäonnistuu, kun ei ole Azure AD-objekti on kyseisen immutableId.
11. Azure AD yrittää sitten Pehmeä-vastine Teemu Taylorista. Toisin sanoen etsii Jos mikä tahansa objekti, jonka proxyAddresses on yhtä suuri kolme arvoa, kutensmtp:bob@contoso.com
12. Azure AD etsii Teemu Smith objektin Pehmeä vastaa hakuehtoja. Tämä objekti on immutableId arvo = "abcdefghijklmnopqrstuv ==". mikä ilmaisee tämä objekti on synkronoitu toiseen objektista paikallisesta Active Directory. Näin ollen Azure AD ei Pehmeä-vastine objektit ja aiheuttaa virheen **InvalidSoftMatch** Synkronoi.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Voit korjata virheen InvalidSoftMatch
Yleisimmät InvalidSoftMatch virheen syynä on kaksi objektit, joissa on eri SourceAnchor \(immutableId\) on sama ProxyAddresses-ja/tai UserPrincipalName-määritteitä, joita käytetään Pehmeä vastine aikana Azure AD-arvo. Jotta voit korjata virheellinen Pehmeä vastaavuus

1.  Määritä kaksoiskappaleet proxyAddresses, userPrincipalName tai muu määritearvo, joka aiheuttaa virheen. Myös selville, mitä kaksi \(vähintään\) objekteja, jotka osallistuvat ristiriita. [Azure AD yhteyden kunto synkronoinnin](https://aka.ms/aadchsyncerrors) luoma raportti auttaa sinua huomaamaan kaksi objektia.
2. Tunnistaa, mikä objekti on edelleen kaksoiskappaleet arvo ja objektissa ei pitäisi.
3. Poista kaksoiskappaleet arvon objektia, jonka ei pitäisi olla arvo. Huomaa, että voit pitäisi tehdä muutoksen missä objektin Orpotermit-kansiossa. Joissakin tapauksissa joudut ehkä poistaa ristiriidan objektit.
4. Jos olet tehnyt muutoksen käytössä paikallisen AD, avulla Azure AD Connect synkronoida muutos.

Huomaa, että Sync virheraportin sisällä Azure AD yhteyden kunto-synkronointia varten on päivitetty 30 minuutin välein ja sisältää uusimmat Synkronointitoiminto virheistä.

>[AZURE.NOTE] ImmutableId, määritelmän, älä muuta objektia elinaika. Jos Azure AD Connect ei määritetty osittain yllä olevasta luettelosta muista käyttötavoista, saattaa lopettaa tilanteessa, jossa Azure AD Connect laskee eri arvo, joka edustaa sama AD-objektin SourceAnchor kohteen (sama käyttäjän/ryhmän/yhteyshenkilön jne), jossa on Azure AD objektin, jota haluat käyttää.

#### <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
- [Kaksoiskappaleiden tai virheelliset attribuutit estävät hakemistosynkronoinnin Office 365: ssä](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Kuvaus
Kun Azure AD yrittää vastaa Pehmeä kaksi objektia, se on mahdollista, että kaksi eri, "objektit tyypin" (esimerkiksi käyttäjä, ryhmä, yhteyshenkilön jne.) on käyttänyt Pehmeä vastine määritteiden samat arvot. Koska kopiointia määritteisiin ei ole sallittua Azure AD-toiminto saattaa johtaa "ObjectTypeMismatch" synkronointivirhe.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Esimerkki tilanteita, joissa ObjectTypeMismatch virhe
- Sähköpostin käytössä käyttöoikeusryhmän luodaan Office 365: ssä. Järjestelmänvalvoja lisää uuden käyttäjän tai yhteyshenkilön paikallinen AD (joka synkronoituu ei ole vielä Azure AD), jotka Office 365-ryhmän ProxyAddresses-määritteeseen saman arvolla.

#### <a name="example-case"></a>Esimerkki kirjainkoko

1. Järjestelmänvalvoja luo uusi sähköposti käytössä käyttöoikeusryhmän Office 365: ssä ALV-osaston ja antaa sähköpostiosoite muodossa tax@contoso.com. Tämä määrittää ryhmän arvo on ProxyAddresses-määritteeseen**smtp:tax@contoso.com**
2. Uusi käyttäjä yhdistää Contoso.com ja paikallinen kanssa kuin proxyAddress käyttäjälle luodaan uusi tili**smtp:tax@contoso.com**
3. Kun Azure AD Connect synkronoidaan uuden käyttäjätilin, se tulee "ObjectTypeMismatch"-virhe.

#### <a name="how-to-fix-objecttypemismatch-error"></a>Voit korjata virheen ObjectTypeMismatch
ObjectTypeMismatch virheen Yleisin syy on kaksi objektien erityyppisiä (käyttäjän, ryhmän, yhteyshenkilön jne.) on sama arvo ProxyAddresses-määritteeseen. Jotta voit vahvistaa ObjectTypeMismatch:

1.  Tunnistaa kaksoiskappaleet proxyAddresses (tai muita) arvo, joka aiheuttaa virheen. Myös selville, mitä kaksi \(vähintään\) objekteja, jotka osallistuvat ristiriita. [Azure AD yhteyden kunto synkronoinnin](https://aka.ms/aadchsyncerrors) luoma raportti auttaa sinua huomaamaan kaksi objektia.
2. Tunnistaa, mikä objekti on edelleen kaksoiskappaleet arvo ja objektissa ei pitäisi.
3. Poista kaksoiskappaleet arvon objektia, jonka ei pitäisi olla arvo. Huomaa, että voit pitäisi tehdä muutoksen missä objektin Orpotermit-kansiossa. Joissakin tapauksissa joudut ehkä poistaa ristiriidan objektit.
4. Jos olet tehnyt muutoksen käytössä paikallisen AD, avulla Azure AD Connect synkronoida muutos. Synkronoi virheraportin sisällä Azure AD yhteyden kunto synkronoinnin päivitetään 30 minuutin välein, ja se sisältää uusimmat Synkronointitoiminto virheistä.


## <a name="duplicate-attributes"></a>Kaksoiskappaleiden määritteet
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Kuvaus
Azure Active Directory-rakenne ei salli useita objekteja, joilla on sama arvo määritteet. Tämä on Azure AD kullekin objektille on pakko on yksilöllinen arvo määritteisiin annetun esiintymän.

- ProxyAddresses
- UserPrincipalName

Jos Azure AD Connect yrittää uuden objektin Lisää tai päivitä yllä määritteiden arvo, joka on jo määritetty toisen objektin Azure Active Directory-objektin, toiminto aiheuttaa synkronointivirheen "AttributeValueMustBeUnique".
#### <a name="possible-scenarios"></a>Mahdolliset skenaariot:
1. Kaksoisarvon määritetään jo synkronoitu objekti, joka menee päällekkäin jonkin toisen synkronoidun objektin kanssa.

#### <a name="example-case"></a>Esimerkki tapaus:
1. **Teemu Smith** on synkronoitu Azure Active Directory paikallinen Active Directory contoso.com-käyttäjän
2. Paikallisesti Risto Smith **UserPrincipalName** on määritetty **bobs@contoso.com**.
3. Teemu on myös seuraavat arvot **proxyAddresses** -määritteeseen:
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. Uusi käyttäjä, **Teemu Taylorista**lisätään käytössä paikallisen Active Directory.
5. Teemu Taylorista **UserPrincipalName** on määritetty **bobt@contoso.com**.
6. **Teemu Taylorista** on **ProxyAddresses** -määritteeseen i seuraavat arvot. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Teemu Taylorista objektin synkronoidaan Azure AD onnistuneesti.
8. Järjestelmänvalvojan päättäneet Päivitä Teemu Taylorista **ProxyAddresses** -määritteeseen seuraava arvo: voin. **smtp:bob@contoso.com**
9. Azure AD yrittää päivittää Teemu Taylorista objektin Azure AD yllä arvona, mutta epäonnistuu nimellä, että ProxyAddresses-arvo on jo määritetty Teemu Sallinen, tuloksena on "AttributeValueMustBeUnique"-virhe.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Voit korjata virheen AttributeValueMustBeUnique
Yleisimmät AttributeValueMustBeUnique virheen syynä on kaksi objektit, joissa on eri SourceAnchor \(immutableId\) on sama arvo, jos kyseessä on ProxyAddresses-ja/tai UserPrincipalName-määrite. Jotta voit korjata virheen AttributeValueMustBeUnique

1.  Määritä kaksoiskappaleet proxyAddresses, userPrincipalName tai muu määritearvo, joka aiheuttaa virheen. Myös selville, mitä kaksi \(vähintään\) objekteja, jotka osallistuvat ristiriita. [Azure AD yhteyden kunto synkronoinnin](https://aka.ms/aadchsyncerrors) luoma raportti auttaa sinua huomaamaan kaksi objektia.
2. Tunnistaa, mikä objekti on edelleen kaksoiskappaleet arvo ja objektissa ei pitäisi.
3. Poista kaksoiskappaleet arvon objektia, jonka ei pitäisi olla arvo. Huomaa, että voit pitäisi tehdä muutoksen missä objektin Orpotermit-kansiossa. Joissakin tapauksissa joudut ehkä poistaa ristiriidan objektit.
4. Jos olet tehnyt muutoksen käytössä paikallisen AD, anna Azure AD Connect synkronoida virheen korjattu muutosta.

#### <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
-[Kaksoiskappaleiden tai virheelliset attribuutit estävät hakemistosynkronoinnin Office 365: ssä](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Tietojen kelpoisuustarkistuksen virheet
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Kuvaus
Azure Active Directory pakottaa itse tietoja ennen kuin sallit kirjoitetaan kansioon tiedot eri rajoituksia. Näin voit varmistaa, että loppukäyttäjät tulee parhaiten mahdollista tarjoaa tiedoista riippuvat sovellukset-ohjelmalla.
#### <a name="scenarios"></a>Skenaariot
a. UserPrincipalName-määritteen on virheellinen tai ei tue merkkejä.
b. UserPrincipalName-määritteen ei noudata valittua muotoa.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Voit korjata virheen IdentityDataValidationFailed

a. Varmista, että userPrincipalName-määritteen on tuettu merkit ja vaadittava muoto.

#### <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
- [Käyttäjien Office 365: een hakemistosynkronoinnin käytön valmistelu]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Kuvaus
Tämä on hyvin tapaukseen, joka johtaa **"DataValidationFailed"** aiheuttaa synkronointivirheen, kun käyttäjän UserPrincipalName liitteen muutetaan yhden liitetyssä toimialueessa toiseen liitetyssä toimialueessa.

#### <a name="scenarios"></a>Skenaariot
Synkronoidun käyttäjän UserPrincipalName-liite on muutettu liitetyt toimialueesta toiseen liitetyssä toimialueessa, paikallinen. Esimerkiksi *UserPrincipalName = bob@contoso.com * on muutettu *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Esimerkki
1. Active Directory UserPrincipalName kanssa uusi käyttäjä saa lisättyjä Teemu Sallinen, Contoso.com-tilibob@contoso.com
2. Teemu siirtää eri osaston Contoso.com kutsutaan Fabrikam.com, ja hänen UserPrincipalName muutetaanbob@fabrikam.com
3. Contoso.com-ja fabrikam.com ovat yhdistettyjen toimialueiden Azure Active Directory-hakemistosta.
4. Teemun userPrincipalName ei Hae päivittää, ja tuloksena on "DataValidationFailed" synkronointivirhe.

#### <a name="how-to-fix"></a>Korjausohje
Jos käyttäjän UserPrincipalName jälkiliite on päivitetty bob@ **contoso.com** , bob@ **fabrikam.com**, jossa **contoso.com** ja **fabrikam.com** ovat **yhdistettyjen toimialueiden**, noudata alapuolella korjaamiseksi synkronointivirheen

1. Päivitä käyttäjän UserPrincipalName Azure AD bob@contoso.com , bob@contoso.onmicrosoft.com. Voit käyttää seuraavaa PowerShell-komentoa PowerShellin Azure AD-moduulin kanssa:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. Salli seuraavan synkronoinnin jakson, yritä synkronointia. Tämän ajan synkronointi onnistuu ja se päivittyy, Teemu UserPrincipalName bob@fabrikam.com odotetulla tavalla.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Kuvaus
Kun määrite ylittää sallitun kokorajoituksen, pituusrajoituksen tai Laske rajan Azure Active Directory-rakenne, Synkronointitoiminto johtaa **LargeObject** tai **ExceededAllowedLength** -synkronointivirheen. Tavallisesti tämä virhe ilmenee määritteet

- userCertificate
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Mahdollisia skenaarioita
1. Teemun userCertificate määrite on tallentaminen liikaa Teemu myönnetyt varmenteet. Nämä voivat sisältää vanhat ja vanhentuneet varmenteet.
2. Määrittää Active Directoryssa Teemun thmubnailPhoto on liian suuri voi synkronoida Azure AD.
3. Aikana automaattisen populaation ProxyAddresses-määritteeseen Active Directory-objektin käytössä määritetty > 500 ProxyAddresses.

### <a name="how-to-fix"></a>Korjausohje

1. Varmista, että aiheuttaa virheen määrite on sallittu rajoissa.

## <a name="related-links"></a>Aiheeseen liittyvät linkit
- [Etsi Active Directory-objekteja Active Directory-hallintakeskus] (https://technet.microsoft.com/library/dd560661.aspx)
- [Kyselyjen Azure Active Directory-objektin käyttämällä PowerShellin Azure Active Directory tekeminen](https://msdn.microsoft.com/library/azure/jj151815.aspx)
