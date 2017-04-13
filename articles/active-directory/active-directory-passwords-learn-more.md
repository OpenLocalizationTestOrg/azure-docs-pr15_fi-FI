<properties
    pageTitle="Lue lisää: Azure AD-salasanojen hallinta | Microsoft Azure"
    description="Azure AD salasanojen hallinta, esimerkiksi salasana takaisinkirjoituksen toimintatavan takaisinkirjoituksen suojaaminen salasanalla ja miten salasanan yhteisöportaalin toimivuus tietoja käytetään salasanan aiheistaoffice.com Lisäasetukset."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="learn-more-about-password-management"></a>Lisätietoja salasanojen hallinta

> [AZURE.IMPORTANT] **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).

Jos olet jo asentanut salasanojen hallinta tai on lisätietoja nitty seuraavaksi siirrytään ja miten se toimii ennen kuin otat, tämän osan avulla voit takana palvelun tekninen käsitteitä hyvä yleiskatsaus tekniset pelkän. Käsittelemme seuraavasti:

* [**Salasanan takaisinkirjoituksen yleiskatsaus**](#password-writeback-overview)
  - [Salasana takaisinkirjoituksen toiminta](#how-password-writeback-works)
  - [Skenaariot salasanan takaisinkirjoituksen tuki](#scenarios-supported-for-password-writeback)
  - [Salasanan takaisinkirjoituksen suojausmalli](#password-writeback-security-model)
* [**Miten salasana palautetaan portaalin työn?**](#how-does-the-password-reset-portal-work)
  - [Tietoja käytetään salasanan?](#what-data-is-used-by-password-reset)
  - [Miten salasana palauttaa tietoja käyttäjien](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Salasanan takaisinkirjoituksen yleiskatsaus
Salasanan takaisinkirjoituksen on [Azure Active Directory-yhteyden](active-directory-aadconnect.md) osa, käytössä ja käyttää Azure Active Directory-Premium nykyisen tilaajille. Lisätietoja on artikkelissa [Azure Active Directory-versiot](active-directory-editions.md).

Salasanan takaisinkirjoituksen voit kirjoittaa takaisin salasanat paikallisen Active Directory cloud-asiakasympäristön.  Se obviates sinun ei tarvitse määrittää ja hallita monimutkaisia paikallisen käyttäjien Omatoiminen salasanan palauttaminen ratkaisu ja se on kätevä pilvipohjainen tapa käyttäjiä vaihtamaan salasanansa paikallisen kaikkialla, missä ne ovat.  Lue joillekin salasanan takaisinkirjoituksen ominaisuuksia:

- **Nolla viive palaute.**  Salasanan takaisinkirjoituksen on synkronoitu toiminto.  Käyttäjien ilmoitetaan heti, jos salasana ei täyttänyt käytännön tai ei voi palauttaa tai muuttaa jostakin syystä.
- **Tukee salasanojen AD FS tai federation muita tekniikoita käyttäjille.**  Takaisinkirjoituksen salasanan kanssa, kunhan liitetyt käyttäjätilit synkronoidaan kyselyjä Azure AD-vuokraajan, he voivat hallita niiden paikallista AD salasanat pilvestä.
- **Salasanojen salasanan hash synkronointi käyttäjille, joiden tukee.** Kun salasanan palauttaminen-palvelu tunnistaa synkronoidun käyttäjätilille on käytössä salasanan hash-synkronointia, on vaihdettu tämän tilin paikallisen ja cloud salasanan samanaikaisesti.
- **Tukee salasanojen vaihtaminen access Ohjauspaneelin ja Office 365: ssä.**  Kun liitetty salasanan synkronointi tai käyttäjät ovat peräisin muuttamaan salasanansa vanhentunut tai ei ole vanhentunut, emme vastata viestiin salasanat paikallisen AD-ympäristöön.
- **Kirjoituksen takaisin salasanoja, kun järjestelmänvalvoja palauttaa ne tuodaan tuki** [**Azure hallinta-portaalin**](https://manage.windowsazure.com).  Aina, kun järjestelmänvalvoja palauttaa, käyttäjän salasana [Azure hallinta-portaalin](https://manage.windowsazure.com), jos käyttäjä on liitetty tai salasanan synkronointi, emme määrittää järjestelmänvalvoja valitsee oman paikallisen AD, myös salasana.  Tämä tällä hetkellä ei tueta Office-hallintaportaalissa.
- **Pakottaa paikallisen oman AD salasanakäytännöt.**  Kun käyttäjä nollaa kyseistä salasanaa, emme Varmista, että, että se täyttää paikallisen oman AD-käytäntö, ennen kuin vahvistetaan tähän kansioon.  Tämä sisältää historia, monimutkaisuus, ikä, salasana suodattimet ja muita salasana rajoitukset on määritetty paikallisen AD.
- **Kaikki saapuvat palomuurisäännöt ei tarvita.**  Salasanan takaisinkirjoituksen käyttää Azure palvelun Bus välitys pohjana viestintä-kanavan mikä tarkoittaa, että sinun ei tarvitse avata kaikki saapuvat portit palomuurissa, voit käyttää tätä toimintoa.
- **Ei tueta varten käyttäjätilejä, joka on suojattu ryhmissä paikallisen Active Directoryssa.** Lisätietoja suojatun ryhmät-kohdassa [suojattu tilien ja ryhmien Active Directoryn](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>Salasanan takaisinkirjoituksen toiminta
Salasanan takaisinkirjoituksen on kolme pääosaa:

- Salasanan palauttaminen pilvipalvelussa (tämä on myös integroitu Azure AD salasana Muuta sivujen)
- Vuokraajan kielikohtaiset Azure palvelun Bus välitys
- Paikallinen salasanan päätepiste

Ne sopivat yhteen kuvatulla tavalla-kaavion alla:

  ![][001]

Kun synkronointi liittoutuneille tai salasana hash haluat käyttäjän saadaan Palauta tai muuttaa salasanan pilvipalvelussa, tapahtuu seuraavaa:

1.  Olemme Tarkista tyypin salasanan käyttäjällä on.  Jos näemme salasana hallitaan paikallisesti, valitse varmistamme takaisinkirjoituksen-palvelu on hyvin alkuun.  Jos se on, että avulla käyttäjä voi jatkaa, jos se ei ole, on onko käyttäjälle, joka salasanansa ei voi palauttaa tähän.
2.  Seuraavaksi käyttäjä välittää asianmukaisista yhdyskäytäviä ja Palauta salasana näytön saavuttaa.
3.  Käyttäjä valitsee uusi salasana ja vahvistaa sen.
4.  Yhteydessä valitsemalla Lähetä, emme salaa tekstimuotoisten salasana symmetrisen avaimen, joka on luotu takaisinkirjoituksen asennuksen aikana.
5.  Jälkeen salaaminen salasana on lisätä sen tiedot, jotka eivät mene HTTPS-kanavassa oman vuokraajan tietyn palvelun bus välitys (joka on myös määritetään puolestasi takaisinkirjoituksen määrityksen aikana).  Tämä välitys suojataan satunnaisesti luotu salasana, jonka vain paikallisen version tietää.
6.  Kun viesti saavuttaa palvelun bus-salasanan palauttaminen päätepisteen aktivoituu automaattisesti ja näkee, että se on Palauta odottava pyyntö.
7.  Palvelun sitten etsitään käyttäjän poistettavaan cloud ankkuri-määritteen avulla.  Tälle hakukentälle onnistuu käyttäjäobjekti on oltava AD connector-tilaan, on linkitetty vastaavaan ma objektiin ja se on linkitetty vastaavaan AAD yhdistimen objektiin. Lopuksi, jotta löydät tämän käyttäjätilin synkronointi AD yhdistimen objektin linkittäminen ma on oltava synkronointi säännön `Microsoft.InfromADUserAccountEnabled.xxx` olevaa linkkiä.  Tämä tarvita, sillä kun puhelu on pilvestä, synkronointi-ohjelma käyttää cloudAnchor-määritteen AAD yhdistimen tilaa objektin hakemiseen ja valitse seuraava linkki takaisin ma objektin ja seuraa sitten takaisin AD-objektin linkki. Koska voi olla useita AD objekteja (usean metsää) sama käyttäjä, sync engine riippuvainen `Microsoft.InfromADUserAccountEnabled.xxx` linkin, valitse oikea.
8.  Kun käyttäjätili löytyy, emme yrittää salasanan asianmukainen AD metsää suoraan.
9.  Jos salasanan määrittäminen toiminto onnistuu, emme onko käyttäjälle hänen salasanansa on muutettu ja että se on hauskaa liitteeksi.
10. Jos salasana määritetty toiminto epäonnistuu, syy palauttaa virhearvon käyttäjälle ja ilmoita uudelleen.  Toiminto saattaa epäonnistua, koska palvelu ei ole toiminnassa, koska ne valittu salasana ei täyttänyt organisaation käytäntöjen, koska olemme ei löydy käyttäjän paikallisen AD tai jokin muu luku syistä.  Microsoft on monia tällaisissa tapauksissa tietty viesti ja onko käyttäjälle, mitä käyttäjä voi tehdä ongelman ratkaisemista varten.

### <a name="scenarios-supported-for-password-writeback"></a>Skenaariot salasanan takaisinkirjoituksen tuki
Seuraavassa taulukossa kuvataan, mitä skenaarioita tuetaan versiot, tutustu synkronointiominaisuuksia.  Yleensä on erittäin suositeltavaa, että asennat uusimman version [Azure AD Connect](active-directory-aadconnect.md#install-azure-ad-connect) , jos haluat käyttää salasanan takaisinkirjoituksen.

  ![][002]

### <a name="password-writeback-security-model"></a>Salasanan takaisinkirjoituksen suojausmalli
Salasanan takaisinkirjoituksen on erittäin turvallinen ja tehokkaat palvelu.  Jotta tietosi on suojattu, käyttöön 4-tasoisen suojausmalli, joka on kuvattu alla.

- **Vuokraajan tietyn palvelun bus välitys** – kun olet määrittänyt palvelun kokeiluversion määrittäminen vuokraajan kielikohtaiset palvelun bus välitys, jotka on suojattu, Microsoft ei koskaan pääsy satunnaisesti luotu vahva salasana.
- Vahvan symmetrisen avain, jonka avulla voit salata salasanan kuin ‑toiminto on luodaan **Locked down salaustavalla vahva salasana salausavaimen** – palvelu bus välitys luomisen jälkeen.  Tätä näppäintä sijaitsevat vain yrityksen salainen kaupan pilvipalvelussa, joka on raskaasti lukittu ja seurata samalla tavalla kuin minkä tahansa salasanan hakemistossa.
- **Teollisuuden vakio TLS** – kun salasanan palauttaminen tai muuttaa toiminnon ilmenee pilveen, syy kestää vain teksti-salasanaa ja salaa sitä julkinen avain.  Olemme sitten plop, HTTPS-viestiin, joka lähetetään käyttämällä Microsoftin SSL-varmenteet ja palvelun bus-välitys salatun kanavan kautta.  Kun viesti saapuu palvelun Bus kyselyjä, paikallinen agentti aktivoituu todentaa palvelun Bus käyttämällä vahva salasana, joka oli aiemmin luotu vastataan salatun viestin, purkaa yksityinen avain on luotu ja yritykset avulla voit määrittää salasanan AD DS SetPassword API-Liittymän kautta.  Tämä vaihe on pilveen mitä pystyy voidaan valvoa AD paikallinen salasana käytäntöjen (monimutkaisuus, ikä, historia, suodattimet jne.).
- **Viestin vanhenemiskäytännöistä** – lopuksi jostain syystä viestin sijaitsee palvelun Bus, koska paikallinen palvelu ei ole käytettävissä, jos se aikakatkaisu ja poistaa muutaman minuutin kuluttua, jotta voit parantaa suojausta entisestään.

## <a name="how-does-the-password-reset-portal-work"></a>Miten salasana palautetaan portaalin työn?
Kun käyttäjä siirtyy salasanan portal-työnkulku on poistettu, onko käyttäjätilin voimassa, mitä organisaatiossa, jossa käyttäjät kuuluu, käyttäjän salasana hallinnasta ja riippumatta siitä, onko käyttäjällä käyttöoikeus-toiminnon käyttäminen.  Lue lisätietoja logiikan takana salasanan Palauta sivun ohjeita.

1.  Käyttäjä napsauttaa linkkiä tai siirtyy suoraan nykyiseen [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com)ei voi käyttää.
2.  Käyttäjä lisää käyttäjätunnusta ja välittää captcha.
3.  Azure AD tarkistaa, kuuluuko käyttäjän käyttää tätä ominaisuutta seuraavasti:
    - Tarkistaa, että käyttäjällä on tämä toiminto on käytössä ja määritetty Azure AD-käyttöoikeutta.
        - Jos käyttäjä ei ole tämä toiminto on käytössä tai määritetty käyttöoikeutta, käyttäjää pyydetään yhteystietoluetteloonsa järjestelmänvalvojalta yhteystietoluetteloonsa salasanan.
    - Tarkistaa, että käyttäjällä on oikeus Haasta määritetyt järjestelmänvalvojan käytännön mukaisesti yhteystietoluetteloonsa tilin tiedot.
        - Jos käytäntö edellyttää, että vain yksi, valitse varmistetaan, että käyttäjällä on määritetty vähintään yksi käytössä järjestelmänvalvojan käytäntö haasteita tarvittavat tiedot.
          - Jos käyttäjä ei ole määritetty, käyttäjän on kannata yhteystietoluetteloonsa järjestelmänvalvojalta yhteystietoluetteloonsa salasanan.
        - Jos käytäntö edellyttää, että kaksi haasteisiin, valitse varmistetaan, että käyttäjällä on määritetty vähintään kaksi haasteisiin, järjestelmänvalvojan käytäntö käytössä tarvittavat tiedot.
          - Jos käyttäjä ei ole määritetty ja sitten Microsoft käyttäjä ei kannata yhteystietoluetteloonsa järjestelmänvalvojalta yhteystietoluetteloonsa salasanan.
    - Tarkistaa, onko käyttäjän salasana hallitaan paikallisesti (liitetty tai salasanan hash synkronointi oli).
       - Jos käyttäjän salasana hallitaan paikallisesti takaisinkirjoituksen on otettu käyttöön, käyttäjän sallitaan Jatka todennusta ja hänen salasanansa on vaihdettu.
       - Jos käyttäjän salasana hallitaan paikallisesti takaisinkirjoitusta ei ole otettu käyttöön, käyttäjää pyydetään yhteystietoluetteloonsa järjestelmänvalvojalta yhteystietoluetteloonsa salasanan.
4.  Jos se määritetään, että käyttäjä ei voi onnistuneesti hänen salasanansa on vaihdettu, käyttäjän on ohjattu Palauta vaiheet.

Lisätietoja salasanan takaisinkirjoituksen osoitteessa ottamisesta [käytön aloittaminen: Azure AD salasanojen hallinta](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Tietoja käytetään salasanan?
Seuraavassa taulukossa jäsennykset, mihin ja miten näitä tietoja käytetään salasanan aikana ja on suunniteltu helpottamaan voit päättää, mitä todennusasetukset sopivat organisaation. Tässä taulukossa esitetään myös kaikki muotoilun tapauksissa, jossa kirjoitit tiedot puolesta käyttäjille syötteen poluista, jotka eivät tarkista nämä tiedot koskevat vaatimukset.

> [AZURE.NOTE] Toimiston puhelinnumero ei näy rekisteröinti-portaalissa, koska käyttäjät ovat tällä hetkellä ei voi muokata tämän ominaisuuden hakemistossa.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Yhteydenottotapa nimi</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Azure Active Directory-tiedot-osa</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Käytetään / asetettava mihin?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Muotovaatimuksia</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Toimiston puhelinnumero</p>
            </td>
            <td>
              <p>Puhelinnumero</p>
              <p>esimerkiksi Set-MsolUser - UserPrincipalName JWarner@contoso.com - PhoneNumber "1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Käyttää:</p>
              <p>Salasanan palauttaminen Portal</p>
              <p>Valitse asetettavissa:</p>
              <p>PhoneNumber on asetettava PowerShell-Dirsync-työkalun, Azure hallinta-portaalin ja Office-hallintaportaaliin</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (esimerkiksi 1 1234567890)</p>
              <ul>
                <li class="unordered">
Anna maakoodi<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Anna suuntanumero (tarvittaessa)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
On annettava + edessä maan koodi<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
On oltava maakoodi ja muun luvun välillä on välilyönti<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Tunnisteet eivät ole tuettuja, jos sinulla on määritettyjen tunnisteiden, Microsoft poistaa sen numeron ennen lähettämistä puhelun.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Matkapuhelin</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>TAI</p>
              <p>MobilePhone</p>
              <p>(Todennusta puhelin on käytössä, jos sijaintitietoja ei ole, muuten Tämä kuuluu takaisin Matkapuhelin-kenttään).</p>
              <p>esimerkiksi Set-MsolUser - UserPrincipalName JWarner@contoso.com - MobilePhone "1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Käyttää:</p>
              <p>Salasanan palauttaminen Portal</p>
              <p>Yritysportaalin rekisteröiminen</p>
              <p>Valitse asetettavissa: </p>
              <p>AuthenticationPhone on asetettava salasanan palauttaminen rekisteröinti portal tai MFA rekisteröinti portal.</p>
              <p>MobilePhone on asetettava PowerShell-Dirsync-työkalun, Azure hallinta-portaalin ja Office-hallintaportaaliin</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (esimerkiksi 1 1234567890)</p>
              <ul>
                <li class="unordered">
Anna maakoodi.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Anna suuntanumero (tarvittaessa).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Anna on + edessä maan koodi.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
On oltava maakoodi ja muun luvun välillä on välilyönti.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Tunnisteet eivät ole tuettuja, jos sinulla on määritettyjen tunnisteiden, emme ohita se puhelun yhteydessä.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Vaihtoehtoisen</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>TAI</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Todennusta sähköpostin on käytössä, jos sijaintitietoja ei ole, muuten Tämä kuuluu takaisin vaihtoehtoiseen-kenttään).</p>
              <p>Huomautus: vaihtoehtoiseen-kentässä on määritetty matriisina merkkijonojen hakemistossa.  Käytämme tässä matriisissa on ensimmäisellä rivillä.</p>
              <p>esimerkiksi Set-MsolUser - UserPrincipalName JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>Käyttää:</p>
              <p>Salasanan palauttaminen Portal</p>
              <p>Yritysportaalin rekisteröiminen</p>
              <p>Valitse asetettavissa: </p>
              <p>AuthenticationEmail on asetettava salasanan palauttaminen rekisteröinti portal tai MFA rekisteröinti portal.</p>
              <p>AlternateEmail on asetettava PowerShell-Azure hallinta-portaalin ja Office-hallintaportaaliin</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>tai甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
Sähköpostien pitäisi seurata vakio muotoilu kuin kohden.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Unicode-sähköpostit tuetaan.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Tietoturvaan liittyvät kysymykset ja vastaukset</p>
            </td>
            <td>
              <p>Voit muokata suoraan hakemistossa ei ole käytettävissä.</p>
            </td>
            <td>
              <p>Käyttää:</p>
              <p>Salasanan palauttaminen Portal</p>
              <p>Yritysportaalin rekisteröiminen </p>
              <p>Valitse asetettavissa: </p>
              <p>Voit määrittää tietoturvaan liittyvät kysymykset ainoa tapa on Azure hallinta-portaalin kautta.</p>
              <p>Voit määrittää vastauksia tietoturvaan liittyvät kysymykset tietyn käyttäjän ainoa tapa on palvelun rekisteröinti-portaalissa.</p>
            </td>
            <td>
              <p>Tietoturvaan liittyvät kysymykset on enintään 200 merkkiä ja min 3 merkkiä</p>
              <p>Vastauksia on 40 merkkiä suurin ja pienin 3 merkkiä</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Miten salasana palauttaa tietoja käyttäjien
####<a name="data-settable-via-synchronization"></a>Tietoja asetettavan synkronoinnin kautta
Paikallisen voidaan synkronoida seuraavat kentät:

* Matkapuhelin
* Toimiston puhelinnumero

####<a name="data-settable-with-azure-ad-powershell"></a>Tietoja asetettavan Azure AD-PowerShellin avulla
Seuraavat kentät ovat käytettävissä PowerShellin Azure AD- ja kaavio-Ohjelmointirajapinnan kanssa:

* Vaihtoehtoisen
* Matkapuhelin
* Toimiston puhelinnumero
* Todennus-puhelin
* Todennus-sähköposti

####<a name="data-settable-with-registration-ui-only"></a>Tietoja asetettavan vain rekisteröinnin Käyttöliittymä
Seuraavat kentät ovat vain käytettävissä Vaihtaminen rekisteröinnin Käyttöliittymä (https://aka.ms/ssprsetup) kautta:

* Tietoturvaan liittyvät kysymykset ja vastaukset

####<a name="what-happens-when-a-user-registers"></a>Mitä tapahtuu, kun käyttäjä rekisteröi?
Kun käyttäjä rekisteröi rekisteröintisivu tulee **aina** määrittäminen seuraavat kentät:

* Todennus-puhelin
* Todennus-sähköposti
* Tietoturvaan liittyvät kysymykset ja vastaukset

Jos arvo on säädetty **matkapuhelimella** tai **Vaihtoehtoiseen**, käyttäjät voivat heti käyttää ne nollata salasanoja, vaikka ne eivät ole rekisteröity-palvelun.  Käyttäjien lisäksi kyseiset arvot näkyviin, kun ensimmäisen kerran rekisteröiminen ja muokata niitä, jos ne haluat, että.  Kuitenkin kun rekisteröimään onnistuneesti, näitä arvoja muuttuu pysyväksi **Todennus puhelin** - ja **Todennus sähköposti** -kenttiin tarpeen mukaan.

Tämä voi olla hyötyä tapa salliminen käyttäjille Vaihtaminen samalla kelpoisuus tietoihin rekisteröiminen vaiheet paljon.

####<a name="setting-password-reset-data-with-powershell"></a>Salasanan määrittäminen palauttaa tietoja PowerShellin avulla
Voit määrittää arvot seuraaviin kenttiin Azure AD-PowerShellin avulla.

* Vaihtoehtoisen
* Matkapuhelin
* Toimiston puhelinnumero

Aluksi sinun on [ladattava](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)ja asennettava PowerShellin Azure AD-moduulin.  Kun se on asennettu, voit noudattaa seuraavia ohjeita voit määrittää kunkin kentän.

#####<a name="alternate-email"></a>Vaihtoehtoisen
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Matkapuhelin
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Toimiston puhelinnumero
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Lukuruudun salasanan tiedot PowerShellin avulla
Voit lukea arvot seuraaviin kenttiin Azure AD-PowerShellin avulla.

* Vaihtoehtoisen
* Matkapuhelin
* Toimiston puhelinnumero
* Todennus-puhelin
* Todennus-sähköposti

Aluksi sinun on [ladattava](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule)ja asennettava PowerShellin Azure AD-moduulin.  Kun se on asennettu, voit noudattaa seuraavia ohjeita voit määrittää kunkin kentän.

#####<a name="alternate-email"></a>Vaihtoehtoisen
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Matkapuhelin
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Toimiston puhelinnumero
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Todennus-puhelin
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>Todennus-sähköposti
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Linkkejä salasanan palauttaminen dokumentaatio
Alla on linkkejä kaikkiin Azure AD-salasanan ohjeissa:

* **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).
* [**Toiminta**](active-directory-passwords-how-it-works.md) - palveluun ja kuusi eri osat tietoja kunkin onko
* [**Aloittaminen**](active-directory-passwords-getting-started.md) – Katso, miten, jotta käyttäjät voivat palauttaa ja muuttaa cloud tai paikalliseen salasanansa
* [**Mukauta**](active-directory-passwords-customize.md) - Opi mukauttamaan ulkoasun ja ilmeen ja palvelun organisaation tarpeisiin toiminta
* [**Parhaat käytännöt**](active-directory-passwords-best-practices.md) - Lue, miten voit ottaa nopeasti ja tehokkaasti salasanojen organisaation hallinta
* [**Hae tiedot**](active-directory-passwords-get-insights.md) - tietoja Microsoftin integroitu raportointiominaisuudet
* [**Usein kysytyt kysymykset**](active-directory-passwords-faq.md) - vastauksia usein kysyttyihin kysymyksiin
* [**Vianmääritys**](active-directory-passwords-troubleshoot.md) – Opi palvelun nopeasti liittyviä ongelmia



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
