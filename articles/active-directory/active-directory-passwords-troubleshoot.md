<properties
    pageTitle="Vianmääritys: Azure AD-salasanojen hallinta | Microsoft Azure"
    description="Yleisiä vianmääritys ohjeet Azure AD salasanojen hallinta, mukaan lukien Palauta, muuta, takaisinkirjoituksen rekisteröinnin ja mitä tietoja haluat sisällyttää, kun etsit ohjeita."
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
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Salasanojen hallinta vianmääritys

> [AZURE.IMPORTANT] **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).

Jos sinulla on ongelmia salasanojen hallinta, voimme auttaa. Useimmat ongelmat, joita saattaa tulla voidaan ratkaista joitakin yksinkertaisia vianmääritysohjeita, jossa on lisätietoja alla vianmääritys käyttöönoton kanssa:

* [**Jos tarvitset apua sisältämään tiedot**](#information-to-include-when-you-need-help)
* [**Salasanojen hallinta määritysten hallinta-portaalissa Azure liittyviä ongelmia**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Ongelmia salasanan Management raporttien Azure hallinta-portaalissa**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Ongelmia salasanan palauttaminen rekisteröinti Portal**](#troubleshoot-the-password-reset-registration-portal)
* [**Ongelmia salasanan palauttaminen Portal**](#troubleshoot-the-password-reset-portal)
* [**Salasanan takaisinkirjoituksen liittyviä ongelmia**](#troubleshoot-password-writeback)
  - [Salasanan takaisinkirjoituksen tapahtumaloki virhekoodit](#password-writeback-event-log-error-codes)
  - [Salasanan takaisinkirjoituksen connectivity liittyviä ongelmia](#troubleshoot-password-writeback-connectivity)

Jos olet kokeillut vianmäärityksen ohjeita ja ovat edelleen käytössä ilmenee ongelmia, voit kirjata kysymys [Azure AD-keskustelupalstat](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) tai ota yhteyttä tukeen ja Käymme läpi ongelmasi katsaus heti, kun emme voi.

## <a name="information-to-include-when-you-need-help"></a>Jos tarvitset apua sisältämään tiedot

Jos ei ratkaise ongelmaa alla olevia ohjeita kanssa, voit pyytää Microsoftin tukihenkilöt. Kun otat yhteyttä suositellaan sisältää seuraavat tiedot:

 - **Yleinen virhe kuvaus** – mitä tarkka virhesanoma näkyy käyttäjän Katso?  Jos oli virhesanomaa, kuvaavat odottamatonta toimintaa huomannut, yksityiskohtaiset tiedot.
 - **Sivun** – sivun käytitkö, kun näyttöön tuli virheen (sisältää URL-osoite)?
 - **Päivämäärä / aika ja aikavyöhyke** – mikä oli tarkan päivämäärän ja kellonajan näyttöön tuli virheen (sisältää aikavyöhyke)?
 - **Tue koodi** – mikä oli tuki koodi luodaan, kun käyttäjä tuli virheen (tämän, virhetilanteeseen, valitse näytön alareunassa tukevat koodi-linkki ja Lähetä tukihenkilö GUID-tunnus, joka aiheuttaa).
   - Jos käytät sivun alareunassa tuki koodi ilman, painamalla F12 ja SUOJAUSTUNNUS ja CID hakeminen ja kaksi tulosten lähettäminen tukihenkilö.

    ![][001]

 - **Käyttäjätunnus** – mikä oli käyttäjälle, joka tuli virheen (kuten tunnususer@contoso.com)?
 - **Käyttäjän tietoja** – on liitetty käyttäjän, salasana hash synkronoitu, vain cloud?  Käyttäjä on määritetty AAD Premium tai AAD Basic-käyttöoikeutta?
 - **Tapahtumaloki** – Jos käytät salasanaa takaisinkirjoituksen ja virhe on paikallisen infrastruktuuri, ota zip-tapahtumaloki Azure AD Connect-palvelimesta kopio ja Lähetä pyyntö sekä.

Tietojen auttaa meitä Ratkaise ongelma mahdollisimman pian.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Salasanan palauttaminen määritysten Azure hallinta-portaalissa vianmääritys
Jos käytössä ilmenee virhe, kun määrittäminen salasanan, voit ehkä ratkaista ongelman vianmäärityksen ohjeita noudattamalla:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Virhe kirjainkoko</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mitä virhe käyttäjän nähdä?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Ratkaisu</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p><strong>Määritä</strong> -välilehden Azure hallinta-portaalin <strong>Käyttäjän salasanan palauttaminen käytäntöä </strong>-kohdassa ei näy</p>
            </td>
            <td>
              <p><strong>Käyttäjän salasanan palauttaminen käytäntöä </strong>-kohta ei ole näkyvissä Azure hallinta-portaalin <strong>määrittäminen</strong> -välilehden.</p>
            </td>
            <td>
              <p>Näin voi käydä, jos sinulla ei ole määritetty tämän toiminnon järjestelmänvalvojan AAD Premium tai AAD Basic-käyttöoikeutta. </p>
              <p>Tilanteen korjaamiseksi, Määritä AAD Premium tai AAD Basic-käyttöoikeus sisältää järjestelmänvalvojatilin siirtymällä <strong>käyttöoikeudet</strong> -välilehti ja yritä uudelleen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>En näe määritysten asetusten <strong>Käyttäjän salasanan palauttaminen käytäntöä</strong> -osasta, joka on kuvattu ohjeissa.</p>
            </td>
            <td>
              <p><strong>Käyttäjän salasanan palauttaminen käytäntöä </strong>-osa on näkyvissä, mutta vain merkintää, joka näkyy alla on <strong>Käyttäjien käyttöön salasanan</strong> merkintää.</p>
            </td>
            <td>
              <p>Käyttöliittymän muiden tulee näkyviin, kun vaihdat <strong>Käyttäjien käyttöön salasanan</strong> merkintä <strong>Kyllä.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kokoonpanon-vaihtoehto ei ole näkyvissä.</p>
            </td>
            <td>
              <p>Esimerkiksi ei näy <strong>päivinä, ennen kuin käyttäjän täytyy vahvistaa yhteyshenkilön tietonsa</strong> vaihtoehtoa kun voin selata <strong>Käyttäjän salasanan palauttaminen käytäntöä</strong> osan (tai muita esimerkkejä sama ongelma).</p>
            </td>
            <td>
              <p>Käyttöliittymän määrä elementtejä on piilotettu, kunnes ne tarvitaan. Kokeile kaikki sivun asetusten ottaminen käyttöön, jos haluat nähdä.</p>
              <p>Saat lisätietoja kaikki käytettävissä olevat ohjausobjektit <a href="active-directory-passwords-customize.md#password-management-behavior">salasanojen hallinta toiminta</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p><strong>Kirjoita takaisin salasanojen paikallisen</strong> määritysvaihtoehto ei näy</p>
            </td>
            <td>
              <p><strong>Kirjoita takaisin salasanojen paikallisen</strong> -vaihtoehto ei ole näkyvissä Azure hallinta-portaalin <strong>määrittäminen</strong> -välilehdessä</p>
            </td>
            <td>
              <p>Tämä asetus on näkyvissä vain, jos olet ladannut Azure AD Connect ja määrittänyt salasanan takaisinkirjoituksen. Kun olet tehnyt tämän, asetus tulee näkyviin ja avulla voit ottaa käyttöön tai poistaminen käytöstä takaisinkirjoituksen pilvestä.</p>
              <p>Saat lisätietoja siitä, miten voit tehdä tämän <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Käyttöön salasanan takaisinkirjoituksen Azure AD Connect</a> .</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Salasanan hallintaraporttien Azure hallinta-portaalissa vianmääritys
Jos käytössä ilmenee virhe käytettäessä salasana hallintaraporttien, voit ehkä ratkaista ongelman vianmäärityksen ohjeita noudattamalla:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Virhe kirjainkoko</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mitä virhe käyttäjän nähdä?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Ratkaisu</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Salasanan hallinta raportteja ei näy</p>
            </td>
            <td>
              <p><strong>Raportit</strong> -välilehti <strong>Tehtävän Log</strong> -raporttien eivät näy <strong>salasanan tehtävän</strong> ja <strong>salasanan rekisteröinti tehtävän</strong> raportit.</p>
            </td>
            <td>
              <p>Näin voi käydä, jos sinulla ei ole määritetty tämän toiminnon järjestelmänvalvojan AAD Premium tai AAD Basic-käyttöoikeutta. </p>
              <p>Tilanteen korjaamiseksi, Määritä AAD Premium tai AAD Basic-käyttöoikeus sisältää järjestelmänvalvojatilin siirtymällä <strong>käyttöoikeudet</strong> -välilehti ja yritä uudelleen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Käyttäjän merkintöjen näyttäminen useita kertoja</p>
            </td>
            <td>
              <p>Kun käyttäjä rekisteröi vaihtoehtoiseen, matkapuhelin ja tietoturvaan liittyvät kysymykset, ne kaikki näkyvät omille riveilleen yksittäisen rivin asemesta.</p>
            </td>
            <td>
              <p>Tällä hetkellä, kun käyttäjä rekisteröi, ei voi oletetaan, että ne Rekisteröi kaikki rekisteröinti-sivulla. Olemme seurauksena tällä hetkellä Kirjaudu yksittäisiä tieto, joka on rekisteröity erillisessä tapahtuman tietoja.</p>
              <p>Jos haluat koostaa näitä tietoja, voit ladata raportti ja avaa tiedot, kun pivot-taulukko-excel on joustavammin.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Salasanan palauttaminen rekisteröinti portaalissa vianmääritys
Jos käytössä ilmenee virhe, kun rekisteröidään salasanan käyttäjältä, voit ehkä ratkaista ongelman vianmäärityksen ohjeita noudattamalla:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Virhe kirjainkoko</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mitä virhe käyttäjän nähdä?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Ratkaisu</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kansio ei ole otettu käyttöön salasanan palauttaminen</p>
            </td>
            <td>
              <p>Järjestelmänvalvoja ei otettu käyttöön voit käyttää tätä toimintoa.</p>
            </td>
            <td>
              <p>Vaihtaa <strong>Käyttäjien käyttöön salasanan</strong> merkintä <strong>Kyllä</strong> ja valitse sitten <strong>Tallenna</strong> -Azure hallinta-portaalin directory määritys-välilehti. Sinulla on oltava Azure AD Premium- tai Basic tätä toimintoa järjestelmänvalvoja-käyttöoikeus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Käyttäjällä ei ole määritetty AAD Premium tai AAD Basic-käyttöoikeutta</p>
            </td>
            <td>
              <p>Järjestelmänvalvoja ei otettu käyttöön voit käyttää tätä toimintoa.</p>
            </td>
            <td>
              <p>Määritä Azure AD Premium tai Azure AD Basic-käyttöoikeus käyttäjälle Azure hallinta-portaalissa <strong>käyttöoikeudet</strong> -välilehdessä. Sinulla on oltava Azure AD Premium- tai Basic tätä toimintoa järjestelmänvalvoja-käyttöoikeus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Virhe pyyntöä käsiteltäessä</p>
            </td>
            <td>
              <p>Käyttäjä näkee virheilmoituksen siitä, että:</p>
              <p>

              </p>
              <p>Virhe pyyntöä käsiteltäessä </p>
              <p>Kun yrität salasanan.</p>
            </td>
            <td>
              <p>Tämä voi johtua useita ongelmia, mutta yleensä tämä virhe aiheutuu joko palvelun käyttökatkosta tai määritysten ongelman, joka ei voi selvittää. </p>
              <p>Jos näet tämän virheen ja se on vaikuttavat yrityksesi, ota yhteyttä tukeen ja Microsoft auttaa MP. <a href="#information-to-include-when-you-need-help">Jos tarvitset apua sisällytettävien tietojen</a> Nähdäksesi antaa tukihenkilö tukeen nopean näytössä, näkyvissä.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>Salasanan palauttaminen portaalissa vianmääritys
Jos käytössä ilmenee virhe, kun käyttäjän salasanan, voit ehkä ratkaista ongelman vianmäärityksen ohjeita noudattamalla:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Virhe kirjainkoko</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mitä virhe käyttäjän nähdä?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Ratkaisu</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kansio ei ole otettu käyttöön salasanan palauttaminen</p>
            </td>
            <td>
              <p>Tiliä ei ole otettu käyttöön salasanan palauttaminen</p>
              <p>Emme valitettavasti, mutta järjestelmänvalvoja ei ole määrittänyt tilin tämän palvelun kanssa käytettäväksi. </p>
              <p>

              </p>
              <p>Jos haluat, emme voi ottaa yhteyttä organisaation järjestelmänvalvojan salasanan puolestasi.</p>
            </td>
            <td>
              <p>Vaihtaa <strong>Käyttäjien käyttöön salasanan</strong> merkintä <strong>Kyllä</strong> ja valitse sitten <strong>Tallenna</strong> -Azure hallinta-portaalin directory määritys-välilehti. Sinulla on oltava Azure AD Premium- tai Basic tätä toimintoa järjestelmänvalvoja-käyttöoikeus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Käyttäjällä ei ole määritetty AAD Premium tai AAD Basic-käyttöoikeutta</p>
            </td>
            <td>
              <p>Järjestelmänvalvoja tilin salasanan ei vaihdettu automaattisesti, kun emme voi ottaa yhteyttä organisaation järjestelmänvalvoja, jos haluat tehdä se puolestasi.</p>
            </td>
            <td>
              <p>Määritä Azure AD Premium tai Azure AD Basic-käyttöoikeus käyttäjälle Azure hallinta-portaalissa <strong>käyttöoikeudet</strong> -välilehdessä. Sinulla on oltava Azure AD Premium- tai Basic tätä toimintoa järjestelmänvalvoja-käyttöoikeus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Hakemiston on otettu käyttöön salasanan, mutta käyttäjällä on puuttuu tai on ymmenjärjestelmän muotoiltu todennustiedot</p>
            </td>
            <td>
              <p>Tiliä ei ole otettu käyttöön salasanan palauttaminen</p>
              <p>Emme valitettavasti, mutta järjestelmänvalvoja ei ole määrittänyt tilin tämän palvelun kanssa käytettäväksi. </p>
              <p>

              </p>
              <p>Jos haluat, emme voi ottaa yhteyttä organisaation järjestelmänvalvojan salasanan puolestasi.</p>
            </td>
            <td>
              <p>Varmista käyttäjä on muotoiltu oikein yhteyshenkilön tiedot tiedostoa kansiosta, ennen kuin jatkat. Saat lisätietoja siitä, miten voit määrittää todennustiedot hakemistossa niin, että käyttäjillä ei näy tämän virheen <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">tietoja käytetään salasanan</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Hakemiston on otettu käyttöön salasanan, mutta käyttäjällä on vain yksi tieto yhteyshenkilön tiedoston, kun käytäntö määritetään edellyttämään vahvistus vaiheiden</p>
            </td>
            <td>
              <p>Tiliä ei ole otettu käyttöön salasanan palauttaminen</p>
              <p>Emme valitettavasti, mutta järjestelmänvalvoja ei ole määrittänyt tilin tämän palvelun kanssa käytettäväksi. </p>
              <p>

              </p>
              <p>Jos haluat, emme voi ottaa yhteyttä organisaation järjestelmänvalvojan salasanan puolestasi.</p>
            </td>
            <td>
              <p>Varmista, että käyttäjällä on vähintään kaksi oikein määritetyn yhteyshenkilön menetelmiä (esimerkiksi matkapuhelimeen sekä toimiston puhelinnumero), ennen kuin jatkat. Saat lisätietoja siitä, miten voit määrittää todennustiedot hakemistossa niin, että käyttäjillä ei näy tämän virheen <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">tietoja käytetään salasanan</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Hakemiston on otettu käyttöön salasanan, ja käyttäjä on määritetty oikein, mutta käyttäjä ei voi muodostaa yhteyttä </p>
            </td>
            <td>
              <p>Jokin meni vikaan.  Olemme tapahtui odottamaton virhe sinuun yhteyttä.</p>
            </td>
            <td>
              <p>Tämä saattaa johtua tilapäinen palveluvirhe tai väärin yhteyshenkilön tietoja, jotka on voitu ei havaitse oikein. Jos käyttäjä odottaa 10 sekuntia, yritä uudelleen ja "Ota yhteys järjestelmänvalvojaan"-linkki näkyy. Napsauttamalla Yritä uudelleen uudelleen lähettämistä, puhelun olisi napsauttamalla "Ota yhteys järjestelmänvalvojaan" lähettää lomakkeen sähköpostin käyttäjän, salasana tai Yleiset järjestelmänvalvojat (ensisijaisuusjärjestys) pyytää muun käyttäjän tilin salasanan.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Käyttäjä saa koskaan salasanan Tekstiviesti tai puhelu</p>
            </td>
            <td>
              <p>Käyttäjä napsauttaa "teksti minä" tai "Soita minulle" ja vastaanottaa mitään ei koskaan.</p>
            </td>
            <td>
              <p>Tämä saattaa johtua ymmenjärjestelmän muotoiltu puhelinnumero, hakemistossa. Varmista, että puhelinnumero on muodossa "+ ccc xxxyyyzzzzXeeee". Tietoja muotoilusta puhelimen numerot salasanan käytettäväksi kohdassa <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">tietoja käytetään salasanan</a>.</p>
              <p>Jos asetat tiedostotunnistetta kyseisen käyttäjän reititetään, Huomaa, että salasanan ei tue laajennukset, vaikka määrittäisit jokin hakemistossa (ne poistetaan, ennen kutsu on lähetetty). Kokeile numeron ilman laajennusta, tai integroiminen puhelinnumeron lisääminen PBX-tunniste.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Käyttäjä saa ilmoituksen salasanan palauttaminen sähköposti</p>
            </td>
            <td>
              <p>Käyttäjä napsauttaa "me sähköposti- ja vastaanottaa mitään ei koskaan.</p>
            </td>
            <td>
              <p>Yleisin ongelman syy on, että viestin hylännyt roskapostin suodatus. Valitse roskapostin, roskapostia tai Poistetut-kansiossa sähköposti.</p>
              <p>Varmista myös, että tarkistat oikean sähköpostin viestin... useille henkilöille on hyvin samankaltaisia kuin sähköpostiosoitteet ja sinut väärä viesti Saapuneet-kansion tarkistaminen. Jos kumpaakaan näistä vaihtoehdoista, on myös mahdollista hakemistossa sähköpostiosoite on virheellinen, tarkista, varmista, että sähköpostiosoite on oikeaa tiliä ja yritä uudelleen. Lisätietoja sähköpostin muotoilun osoitteet salasanan käytettäväksi Katso <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">tietoja käytetään salasanan</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Palauta Salasanakäytäntö määritän, mutta kun järjestelmänvalvoja-tilin salasanan, käytäntö ei ole käytetty</p>
            </td>
            <td>
              <p>Järjestelmänvalvojien tilien salasanojen niiden artikkelissa salasanan, sähköpostin ja matkapuhelimeen, riippumatta siitä, mitä käytäntö määritetään <strong>Käyttäjän salasanan palauttaminen käytännön</strong> <strong>määrittäminen</strong> -välilehden kohdassa samoja asetuksia.</p>
            </td>
            <td>
              <p><strong>Käyttäjän salasanan palauttaminen käytännön</strong> <strong>määrittäminen</strong> -välilehden kohdassa määritetty asetukset koskevat vain organisaation käyttäjille.</p>
              <p>Microsoft hallitsee ja ohjausobjektien järjestelmänvalvojan salasanan palauttaminen käytännön jotta voit varmistaa parhaan mahdollisen suojaus</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Esti yritetään salasanan liian monta kertaa päivä-käyttäjä</p>
            </td>
            <td>
              <p>Käyttäjä näkee virhe siitä:</p>
              <p>

              </p>
              <p>Käytä jokin muu vaihtoehto.</p>
              <p>Olet yrittänyt liian monta kertaa viimeksi 1 tunti(a)-tilin vahvistaminen. Tietoturvasyistä joutua odottamaan 24 tunti(a), ennen kuin voit yrittää uudelleen. </p>
              <p>Jos haluat, emme voi ottaa yhteyttä organisaation järjestelmänvalvojan salasanan puolestasi.</p>
            </td>
            <td>
              <p>Olemme Toteuta automaattinen rajoitusmekanismin käyttäjien estäminen muodostamasta niiden salasanojen liian monta kertaa-lyhyen ajanjakson. Tämä tapahtuu, kun:</p>
              <ol class="ordered">
                <li>
Käyttäjä yrittää Tarkista puhelinnumero 5 luvulla tunnin.<br\><br\></li>
                <li>
Käyttäjä yrittää käyttää suojauksen kysymyksiä portin 5 luvulla tunnin.<br\><br\></li>
                <li>
Käyttäjä yrittää saman käyttäjätilin salasanan nollaaminen 5 luvulla-tunnin.<br\><br\></li>
              </ol>
              <p>Voit korjata ongelman opasta käyttäjän odottamaan 24 tunnin kuluttua viimeistä yritystä ja käyttäjä voi yhteystietoluetteloonsa salasanan.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Käyttäjä näkee Virhe tarkistettaessa yhteystietoluetteloonsa puhelinnumero</p>
            </td>
            <td>
              <p>Vahvista puhelin käytettävä todennusmenetelmä yritettäessä käyttäjä näkee virhe siitä:</p>
              <p>

              </p>
              <p>Määrittämään numeroon on virheellinen.</p>
            </td>
            <td>
              <p>Tämä virhe ilmenee, kun syöttänyt puhelinnumeron vastaa tiedoston puhelinnumero.</p>
              <p>Varmista, että käyttäjällä on siirtymässä koko puhelinnumero, mukaan lukien alue- ja maa-koodia, kun yrität käyttää puhelimitse välitettävä menetelmän salasanan.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Virhe pyyntöä käsiteltäessä</p>
            </td>
            <td>
              <p>Käyttäjä näkee virheilmoituksen siitä, että:</p>
              <p>

              </p>
              <p>Virhe pyyntöä käsiteltäessä </p>
              <p>Kun yrität salasanan.</p>
            </td>
            <td>
              <p>Tämä voi johtua useita ongelmia, mutta yleensä tämä virhe aiheutuu joko palvelun käyttökatkosta tai määritysten ongelman, joka ei voi selvittää. </p>
              <p>Jos näet tämän virheen ja se on vaikuttavat yrityksesi, ota yhteyttä tukeen ja Microsoft auttaa MP. <a href="#information-to-include-when-you-need-help">Jos tarvitset apua sisällytettävien tietojen</a> Nähdäksesi antaa tukihenkilö tukeen nopean näytössä, näkyvissä.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Salasanan takaisinkirjoituksen vianmääritys
Jos käytössä ilmenee virhe, kun ottaminen käyttöön tai poistaminen käytöstä tai käyttämällä salasanan takaisinkirjoituksen, voit ehkä ratkaista ongelman vianmäärityksen ohjeita noudattamalla:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Virhe kirjainkoko</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Mitä virhe käyttäjän nähdä?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Ratkaisu</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Yleiset onboarding ja käynnistys</p>
            </td>
            <td>
              <p>Salasanan palauttaminen palvelu ei käynnisty paikallinen virheeseen 6800 Azure AD Connect-koneen tapahtumaloki.</p>
              <p>

              </p>
              <p>Synkronoitujen käyttäjien voi palauttaa onboarding, liitetty tai salasana hash jälkeen salasanansa.</p>
            </td>
            <td>
              <p>Kun salasana takaisinkirjoituksen on käytössä, sync engine Soita asiakastietojen onboarding pilvipalveluun takaisinkirjoituksen kirjastossa voit tehdä määritykset (onboarding). Mahdollisesti käynnistettäessä WCF-päätepisteen salasanan takaisinkirjoitusta varten tai onboarding aikana tapahtuneista virheistä aiheuttaa virheitä tapahtumalokiin Azure AD Connect-koneen tapahtumalokiin.</p>
              <p>Uudelleenkäynnistyksen ADSync palvelun-aikana Jos takaisinkirjoituksen on määritetty, WCF-päätepisteen käynnistetään. Kuitenkin päätepisteen käynnistys epäonnistuu, jos on yksinkertaisesti Kirjaa tapahtuman 6800 ja antaa synkronointi palvelu käynnistyy. Tapahtuman esiintyminen tarkoittaa, että salasana-takaisinkirjoituksen päätepisteen ei ole käynnistetty ylöspäin. Tapahtumaloki tiedot tapahtuman (6800) tapahtumaloki tapahtumat sekä luoda PasswordResetService osan osoittaa, miksi päätepisteen ei voitu käynnistää. Tarkista tapahtumaloki virheet ja yritä käynnistää Azure AD Connect uudelleen, jos salasana takaisinkirjoituksen edelleenkään ei ole. Jos ongelma jatkuu, kokeile salasanan takaisinkirjoituksen ottaminen käyttöön ja poistaminen käytöstä.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Kun käyttäjä yrittää salasanan tai lukitus tilin salasanan takaisinkirjoituksen käytössä, toiminto epäonnistuu. Lisäksi näet tapahtuman lisääminen Azure AD Connect tapahtumaloki sisältävä: "synkronointiohjelma palautetaan virhe hr = 800700CE, viestin = tiedostonimi tai tunniste on liian pitkä" Poista lukitus-toiminnon jälkeen.
                            </p>
            </td>
            <td>
              <p>Näin voi käydä, jos olet päivittänyt oli Azure AD Connect tai DirSync vanhemmilla versioilla. Azure AD Connect vanhempien versioiden päivittäminen määrittää 254 merkin (uudempia määrittää salasanan pituus 127 merkkiä) Azure AD-hallinta-agentti tilin salasanan. Tällaisia pitkä salasanat toimivat AD yhdistimen tuonti ja vienti toimille, mutta niitä ei tue Poista lukitus-toiminnon.
                            </p>
            </td>
            <td>
              <p>[Etsi Active Directory-tilin](active-directory-aadconnect-accounts-permissions.md#active-directory-account) Azure AD Connect ja Palauta salasana sisältää enintään 127 merkkiä. Käynnistä-valikosta Avaa **Synkronointipalvelu** . **Yhdistimien** ja Etsi **Active Directory Connectorin**. Valitse se ja valitse **Ominaisuudet**. Siirry sivulle, **tunnistetiedot** ja kirjoita uusi salasana. Valitse **OK** ja sulje sivu.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Virhe määritettäessä takaisinkirjoituksen Azure AD Connect asennuksen aikana.</p>
            </td>
            <td>
              <p>Azure AD Connect asentunut viimeinen vaihe näet virhe-osoittaa, että salasana takaisinkirjoitusta ei voitu määrittää.</p>
              <p>

              </p>
              <p>Azure AD yhteyden sovelluksen tapahtumalokiin sisältää virheen 32009 tekstillä "Virhe käytön auth tunnuksen".</p>
            </td>
            <td>
              <p>Tämä virhe tulee näkyviin kaksi seuraavissa tapauksissa:</p>
              <ul>
                <li class="unordered">
Olet määrittänyt väärän salasanan yleisen järjestelmänvalvojan tilin määritetty Azure AD Connect asentunut alussa.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Yrität käyttää liitetyt käyttäjät määritetty Azure AD Connect asentunut alussa yleisen järjestelmänvalvojan tilillä.<br\><br\></li>
              </ul>
              <p>Voit korjata virheen, varmista, että et ole Azure AD Connect asentunut alussa määritetty liitetyt tilin käyttäminen Yleinen järjestelmänvalvoja ja että määritetty salasana ovat oikein.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Virhe määritettäessä takaisinkirjoituksen Azure AD Connect asennuksen aikana.</p>
            </td>
            <td>
              <p>Azure AD Connect machine-tapahtumaloki sisältää virhe ilmenee, PasswordResetService mukaan 32002.</p>
              <p>

              </p>
              <p>Virhe lukee: "Virhe-yhteyden muodostamisesta ServiceBus tunnuksen toimittaja ei voinut tarjota suojaustunnus..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>Tämä virhe pääkansion syy on, että paikallisen-ympäristössä salasanan palauttaminen palvelu ei ole pysty muodostamaan yhteyttä palvelun bus päätepisteen pilveen. Tämä virhe aiheutuu yleensä tavallisesti estää tietyn portin tai verkko-osoite lähtevän yhteyttä palomuurisäännön.</p>
              <p>

              </p>
              <p>Varmista, että palomuuri sallii Lähtevät yhteydet seuraavat asiat:</p>
              <ul>
                <li class="unordered">
Kaikki liikenne päälle TCP 443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Lähtevät yhteydet <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Kun olet päivittänyt sääntöjen, Azure AD Connect käynnistettävä ja salasanan takaisinkirjoituksen kannattaa aloittaa työskentelyn uudelleen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Salasanan vahvistettu päätepiste paikallinen ei ole käytettävissä</p>
            </td>
            <td>
              <p>Kun olet käsitellyt joillekin aika, liitetty tai salasana hash synkronoitujen käyttäjien voi palauttaa salasanansa.</p>
            </td>
            <td>
              <p>Joissakin harvinaisissa tapauksissa salasanan takaisinkirjoituksen palvelun voi epäonnistua Käynnistä uudelleen, kun Azure AD Connect on aloittanut uudelleen. Tällaisissa tapauksissa, tarkista ensin, näkyykö salasanan takaisinkirjoituksen on käytössä prem. käyttöön Tämän voi tehdä Azure AD Connect ohjatun tai powershell (Katso HowTos osa yllä) avulla. Jos ominaisuus on otettu käyttöön, yritä ottaminen käyttöön tai poistaminen käytöstä uudelleen Käyttöliittymän tai PowerShellin avulla. Katso "Vaihe 2: Ota salasana takaisinkirjoituksen Directory-synkronointi tietokoneeseen &amp; määrittäminen palomuurisäännöt" tähän lisätietoja <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">siitä, miten voit ottaa käyttöön/poistaa käytöstä salasanan takaisinkirjoituksen</a> .</p>
              <p>

              </p>
              <p>Jos tämä ei toimi, poista sovellus kokonaan ja asentaa uudelleen Azure AD Connect.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Käyttöoikeudet-virheet</p>
            </td>
            <td>
              <p>Liitetty tai salasanan hash synkronointi d: n käyttäjät, jotka yritetään palauttaa niiden salasanat-kohdassa virhe, kun olet lähettänyt ilmaisee, että palvelun ongelma salasana.</p>
              <p>

              </p>
              <p>Salasanan palauttaminen toimintojen aikana näkyviin voi tulla hallinta agentti koskeva virhe on estetty käytössä paikallisen tapahtumalokien access.</p>
            </td>
            <td>
              <p>Jos näet nämä virheet oman tapahtumalokiin, Vahvista, että AD MA-tili (joka on määritetty ohjatun määrityksen aikaa) on tarvittavat käyttöoikeudet salasanan takaisinkirjoitusta varten.</p>
              <p>

              </p>
              <p>HUOMAA, että kun nämä oikeudet annetaan se voi viedä enintään 1 tunti oikeutta trickle alaspäin sdprop taustan tehtävästä Palvelinkeskus kautta. </p>
              <p>Salasanan toimimaan-käyttöoikeus on voidaan tehdä suojauksen kuvauksen käyttäjäobjektin, jonka salasanan palautetaan. Nämä oikeudet ilmestyy näkyviin käyttäjäobjekti, kunnes salasanan säilyvät epäonnistua käyttö on estetty.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Virhe määritettäessä salasanan takaisinkirjoituksen Azure AD Connect ohjattu määritystoiminto </p>
            </td>
            <td>
              <p>"Ei voi Etsi MA" Virhe ohjatussa salasanan takaisinkirjoituksen ottaminen käyttöön tai poistaminen käytöstä</p>
            </td>
            <td>
              <p>Julkaisuversio Azure AD Connect luettelot seuraavassa tilanteessa, jossa on tunnettu ohjelmavirhe:</p>
              <ol class="ordered">
                <li>
Voit määrittää Azure AD Connect vuokraajan abc.com (toimialue vahvistettu) käyttämällä creds varten. Tuloksena on AAD yhdistin, jolla on nimi "abc.com – AAD" luomisen.<br\><br\></li>
                <li>
Valitse Muuta AAD creds (joko vanha synkronointi Käyttöliittymän) yhdistimen (Huomaa sitä on samassa alihallinnassa mutta erilaisia toimialuenimeä). <br\><br\></li>
                <li>
Kun yrität salasanan takaisinkirjoituksen ottaa käyttöön/poistaa käytöstä. Ohjattu toiminto rakennettava käyttäen "abc.onmicrosoft.com – AAD" creds, yhdistimen nimi ja välittää salasanan takaisinkirjoituksen cmdlet-komento. Epäonnistuu, koska ei ole luotu tämännimistä yhdistin.<br\><br\></li>
              </ol>
              <p>Tämä on korjattu Microsoftin uusimmat versiot. Jos sinulla on vanhempi muodosta, yksi vaihtoehtoinen on powershell cmdlet-komennon avulla voit ottaa käyttöön/poistaa käytöstä ominaisuus. Katso "Vaihe 2: Ota salasana takaisinkirjoituksen Directory-synkronointi tietokoneeseen &amp; määrittäminen palomuurisäännöt" tähän lisätietoja <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">siitä, miten voit ottaa käyttöön/poistaa käytöstä salasanan takaisinkirjoituksen</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Ei voi palauttaa salasana Erityisryhmiin, kuten Domain Admins käyttäjille tai yrityksen Järjestelmänvalvojat jne.</p>
            </td>
            <td>
              <p>Liitetty tai salasanan hash synkronointi d: n käyttäjät, jotka ovat osa suojatun ryhmiä ja yritetään palauttaa niiden salasanat-kohdassa virhe, kun olet lähettänyt ilmaisee, että palvelun ongelma salasana.</p>
            </td>
            <td>
              <p>Sellaisten käyttäjien Active Directoryn suojattu AdminSDHolder. Katso lisätietoja <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> . </p>
              <p>

              </p>
              <p>Tämä tarkoittaa objektit suojauskuvaukset tarkistetaan säännöllisesti vastaamaan yksi määritetty AdminSDHolder ja palautetaan, jos ne ovat erilaisia. Lisää käyttöoikeuksia, joita tarvitaan salasana takaisinkirjoituksen vuoksi ei trickle tällaisia käyttäjille. Tämä saattaa johtaa salasanan takaisinkirjoitusta ei toimi näiden käyttäjille. Tuloksena on eivät tue hallinta salasanat käyttäjille nämä ryhmissä, koska se katkaisee AD-suojausmalli.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Palauta toimintojen epäonnistuu objektia ei löydy</p>
            </td>
            <td>
              <p>Liitetty tai salasanan hash synkronointi d: n käyttäjät, jotka yritetään palauttaa niiden salasanat-kohdassa virhe, kun olet lähettänyt ilmaisee, että palvelun ongelma salasana.</p>
              <p>

              </p>
              <p>Salasanan palauttaminen toimintojen aikana näkyviin voi tulla virhe tapahtumalokiin virhe "Objekti ei löydy" Azure AD Connect-palvelusta.</p>
            </td>
            <td>
              <p>Tämä virhe yleensä tarkoittaa, että sync engine käyttäjäobjektin AAD connector-tilaa tai linkitetyn ma tai AD yhdistimen tilaa objektia ei löydy. </p>
              <p>

              </p>
              <p>Voit tehdä vianmäärityksen, varmista, että käyttäjä on varmasti synkronoitu-paikallinen AAD kautta Azure AD Connect nykyistä esiintymää ja tarkasta yhdistimen välilyönnit ja ma objekteja kunto. Varmista, että AD CS-objekti on yhdistimen ma objektin "Microsoft.InfromADUserAccountEnabled.xxx" säännön kautta.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Palauta toimintojen epäonnistuu useita vastineita löytyneen eror kanssa</p>
            </td>
            <td>
              <p>Liitetty tai salasanan hash synkronointi d: n käyttäjät, jotka yritetään palauttaa niiden salasanat-kohdassa virhe, kun olet lähettänyt ilmaisee, että palvelun ongelma salasana.</p>
              <p>

              </p>
              <p>Salasanan palauttaminen toimintojen aikana näkyviin voi tulla virhe tapahtumalokiin Azure AD Connect-palvelusta, joka ilmaisee "Löytyi useita maches"-virhe.</p>
            </td>
            <td>
              <p>Tämä ilmaisee, että sync engine havaita ma-objekti on liitetty useita AD CS objektit "Microsoft.InfromADUserAccountEnabled.xxx" kautta. Tämä tarkoittaa, että käyttäjällä on käytössä useampi kuin yksi metsää. </p>
              <p>

              </p>
              <p>Tässä skenaariossa on tällä hetkellä ei tueta salasanan takaisinkirjoituksen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Salasanan toiminnot epäonnistua ja määritys-virhesanoma.</p>
            </td>
            <td>
              <p>Salasanan toiminnot epäonnistua ja määritys-virhesanoma. Sovelluksen tapahtumalokiin sisältää tekstillä virheen Azure AD Connect 6329: 0x8023061f (toiminto epäonnistui, koska tämä hallinta-agentti ei ole käytössä salasanojen synkronoinnin.)</p>
            </td>
            <td>
              <p>Näin tapahtuu, jos Azure AD Connect-määritys on muuttunut Lisää&nbsp;uusi AD-metsää (tai jos haluat poistaa ja Lisää aiemmin metsää) <strong>jälkeen</strong> salasana takaisinkirjoituksen-ominaisuus on jo käytössä. Salasanan toimintojen käyttäjille tällaisten lisätyn metsien epäonnistuu. Voit korjata ongelman, poista käytöstä ja salasanan takaisinkirjoituksen-ominaisuus uudelleen käyttöön, kun metsää määritysmuutoksia on suoritettu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Kirjoittaminen takaisin salasanoja, joita on palauttanut käyttäjien toimii oikein, mutta muut takaisin salasanat muuttaa käyttäjän tai järjestelmänvalvojan käyttäjän salasanan epäonnistuu.</p>
            </td>
            <td>
              <p>Kun yrität salasanan Azure hallinta-portaalin käyttäjän puolesta, näet viestin, jossa sanotaan: "käynnissä paikallisen ympäristön salasanan palauttaminen-palvelu ei tue käyttäjän salasanojen järjestelmänvalvojat. Päivitä Azure AD Connect voit ratkaista tämän uusimman version."</p>
            </td>
            <td>
              <p>Tämä tapahtuu, kun synkronointiohjelma versio ei tue tietyn salasanan takaisinkirjoituksen-toiminnon, jota on käytetty. Versioiden viimeistään 1.0.0419.0911 tukevat kaikki salasanan hallintatoiminnot Azure AD Connect myös salasanan palauttaminen takaisinkirjoituksen, salasana muuta takaisinkirjoituksen ja järjestelmänvalvojan käynnistämä salasanan palauttaminen takaisinkirjoituksen Azure hallinta-portaalista.&nbsp; Dirsync-työkalun myöhemmin kuin 1.0.6862 tukevat vain salasanan palauttaminen takaisinkirjoituksen. Voit ratkaista ongelman, on erittäin suositeltavaa, että asennat uusimman version Azure AD Connect tai Azure Active Directory-yhteyden (Lisätietoja on artikkelissa <a href="active-directory-aadconnect">Hakemistointegrointityökalut</a>) voit ratkaista ongelman ja hyödyntäminen tehokkaasti organisaation takaisinkirjoituksen salasana.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Salasanan takaisinkirjoituksen tapahtumaloki virhekoodit
Tarkasta, että tapahtumaloki tietokoneessa Azure AD Connect on suositeltavaa salasanan takaisinkirjoituksen ongelmien vianmäärityksessä. Tämän tapahtumalokin sisältää tapahtumien kahden lähteen halutut salasanan takaisinkirjoituksen. PasswordResetService lähteen kuvataan Toiminnot ja salasanan takaisinkirjoituksen toimintaan liittyvät ongelmat. ADSync lähteen kuvataan Toiminnot ja salasanojen määrittämisestä AD-ympäristöön liittyvät ongelmat.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Koodi</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Nimeä / viesti</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Lähde</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Kuvaus</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>TURVAAMISTOIMI: MMS(4924) 0x80230619 – "rajoitus estää salasanan kursivoidaan nykyisen määritetylle."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Tapahtuman ilmenee, kun salasana takaisinkirjoituksen palvelu yrittää määrittää salasanan, joka ei täytä salasanan ikä, historia, monimutkaisuus tai suodattaminen vaatimukset toimialueen paikallisen hakemistossa.</p>
              <ul>
                <li class="unordered">
Jos olet salasanan iän ja viimeksi muuttanut salasanan ikkunan ajan kuluessa, voit voi vaihtaa salasanan uudelleen, kunnes ohjausobjekti määritetyn ikä toimialueen. Testausta varten vähintään olisi arvoksi 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jos sinulla on käytössä historia-salasanavaatimukset ja valitse sinun on valittava salasanaa, jolla ei ole käytetty viimeisen N ajat, jos N on salasana historia-asetus. Jos valitset salasanaa, jolla on käytetty viimeisen N ajat, valitse näet virheen tällöin. Testausta varten historia olisi arvoksi 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jos sinulla on monimutkaisuus salasanavaatimukset, ne kaikki pakotetaan suoritettaviksi, kun käyttäjä yrittää muuttaminen tai salasanan.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jos sinulla on käytössä salasanan suodattimet ja käyttäjä valitsee salasanan, joka ei täytä suodatusehtoa, valitse Palauta tai muuttaa toiminto epäonnistuu.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Synkronointiohjelma palautetaan virhe hr = 80230402, viestin = yrittää saada objektin epäonnistui, koska kaksoiskappaleet tapahtumat, joissa on sama ankkurin</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Tapahtuman ilmenee, kun samaa käyttäjätunnus on käytössä useita toimialueita.  Esimerkiksi jos ovat synkronoinnin tilin/resurssin metsien ja on sama käyttäjätunnus esitä ja otettu käyttöön kaikissa, tämä virhe voi ilmetä.  </p>
              <p>Tämä virhe voi ilmetä myös, jos käytössäsi on ei-yksilöivän ankkuri-määrite (kuten alias tai UPN) ja kaksi käyttäjää saman ankkurin määritteen jakaminen.</p>
              <p>Voit ratkaista ongelman, varmista, että sinulla ei ole kaksoiskappaleet käyttäjiä sisällä toimialueen ja, kun käytät yksilöllinen ankkuri-määritteen kullekin käyttäjälle.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman osoittaa paikallisen palvelun havaita salasanan pyyntö liitetty tai salasanan hash synkronointi d: n käyttäjä peräisin pilveen. Tapahtuman on ensimmäinen jokaisen salasanan palauttaminen takaisinkirjoituksen toiminto.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman osoittaa, että käyttäjän valittuna uusi salasana salasana-Palauta-toiminnon aikana, on määrittänyt, että salasana vastaa yrityksen salasana ja salasanan kirjoittaminen onnistui takaisin paikalliseen AD-ympäristöön.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että käyttäjän valittuna salasanan ja salasanan saapunut onnistuneesti paikallisen ympäristön, mutta kun olemme yritti määrittää paikallisen AD-ympäristössä-virhe. Tämä voi johtua seuraavista syistä:</p>
              <ul>
                <li class="unordered">
Käyttäjän salasana ei ikä, historia ja monimutkaisuus vastaa tai suodattaa toimialueen koskevat vaatimukset. Yritä ratkaista täysin uuden salasanan.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Liukuva keskiarvo-palvelutilin ei ole tarvittavat käyttöoikeudet määrittää kyseisen käyttäjätilin uutta salasanaa.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Käyttäjän tili on suojattu-esimerkiksi toimialueen tai yrityksen Järjestelmänvalvojat, joka ei salli seuraavantyyppisten salasanan määrittäminen Toiminnot-ryhmästä.<br\><br\></li>
              </ul>
              <p>Artikkelissa kerrotaan lisää siitä, mitä situtions voi aiheuttaa virheen <a href="#troubleshoot-password-writeback">Vianmääritys salasanan takaisinkirjoituksen</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman tapahtuu, jos otat salasanan takaisinkirjoituksen Azure AD Connect kanssa ja ilmaisee, että olemme käynnistää onboarding organisaation salasanan takaisinkirjoituksen web-palveluun.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee onboarding prosessi onnistui ja salasanan takaisinkirjoituksen ominaisuuksien on valmis käytettäväksi.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että paikallisen palvelun havaita salasanan muutospyyntö liitetty tai salasanan hash synkronointi d: n käyttäjä peräisin pilveen. Tapahtuman on ensimmäinen salasana muuta takaisinkirjoituksen toiminnossa välein.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että käyttäjä on valittuna uusi salasana salasana muuta toiminnon aikana, on määrittänyt, että salasana vastaa yrityksen salasana ja salasanan kirjoittaminen onnistui takaisin paikalliseen AD-ympäristöön.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että käyttäjän valittuna salasanan ja salasanan saapunut onnistuneesti paikallisen ympäristön, mutta kun olemme yritti määrittää paikallisen AD-ympäristössä-virhe. Tämä voi johtua seuraavista syistä:</p>
              <ul>
                <li class="unordered">
Käyttäjän salasana ei ikä, historia ja monimutkaisuus vastaa tai suodattaa toimialueen koskevat vaatimukset. Yritä ratkaista täysin uuden salasanan.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Liukuva keskiarvo-palvelutilin ei ole tarvittavat käyttöoikeudet määrittää kyseisen käyttäjätilin uutta salasanaa.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Käyttäjän tili on suojattu-esimerkiksi toimialueen tai yrityksen Järjestelmänvalvojat, joka ei salli seuraavantyyppisten salasanan määrittäminen Toiminnot-ryhmästä.<br\><br\></li>
              </ul>
              <p>Artikkelissa kerrotaan lisää siitä, missä tilanteissa voi aiheuttaa virheen <a href="#troubleshoot-password-writeback">Vianmääritys salasanan takaisinkirjoituksen</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Paikallisen palvelun havaita salasanan pyyntö liitetty tai salasanan hash synkronointi d: n käyttäjälle järjestelmänvalvojan peräisin käyttäjän puolesta. Tapahtuman on ensimmäinen käynnistämä järjestelmänvalvojan salasanan palauttaminen takaisinkirjoituksen toiminnossa välein.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Järjestelmänvalvoja on valittuna uusi salasana käynnistämä järjestelmänvalvojan salasanan palauttaminen toiminnon aikana, on määrittänyt, että salasana vastaa yrityksen salasana ja salasanan kirjoittaminen onnistui takaisin paikalliseen AD-ympäristöön.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Järjestelmänvalvojan valittuna salasanan käyttäjän puolesta ja salasanan saapunut onnistuneesti paikallisen ympäristön, mutta kun olemme yritti määrittää paikallisen AD-ympäristössä-virhe. Tämä voi johtua seuraavista syistä:</p>
              <ul>
                <li class="unordered">
Käyttäjän salasana ei ikä, historia ja monimutkaisuus vastaa tai suodattaa toimialueen koskevat vaatimukset. Yritä ratkaista täysin uuden salasanan.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Liukuva keskiarvo-palvelutilin ei ole tarvittavat käyttöoikeudet määrittää kyseisen käyttäjätilin uutta salasanaa.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Käyttäjän tili on suojattu-esimerkiksi toimialueen tai yrityksen Järjestelmänvalvojat, joka ei salli seuraavantyyppisten salasanan määrittäminen Toiminnot-ryhmästä.<br\><br\></li>
              </ul>
              <p>Artikkelissa kerrotaan lisää siitä, mitä situtions voi aiheuttaa virheen <a href="#troubleshoot-password-writeback">Vianmääritys salasanan takaisinkirjoituksen</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman tapahtuu, jos poistat salasanan takaisinkirjoituksen Azure AD Connect kanssa ja ilmaisee, että olemme käynnistää offboarding organisaation salasanan takaisinkirjoituksen web-palveluun.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee offboarding prosessi onnistui ja että salasana takaisinkirjoituksen ominaisuus on poistettu onnistuneesti.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman osoittaa offboarding prosessin ei onnistunut. Tämä voi johtua käyttöoikeusvirhe cloud tai paikalliseen Järjestelmänvalvoja-tilin määritys-aikana tai siksi, kun yrität käyttää liitetyt cloud Yleinen järjestelmänvalvoja, kun salasana takaisinkirjoituksen käytöstä. Voit korjata ongelman, tarkista oman järjestelmänvalvojan käyttöoikeudet ja että käytössäsi ei ole mitään liitetty tilin salasanan takaisinkirjoituksen ominaisuuksien määritettäessä.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että salasana takaisinkirjoituksen-palvelu on käynnistetty on valmis hyväksymään salasanan hallinta pyynnöt pilvestä.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että salasana takaisinkirjoituksen-palvelu on pysähtynyt ja että pilvestä pyynnöt salasanan hallinta ei onnistu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että olemme onnistuneesti noutaa määritetty Azure AD Connect asennuksen aikana, jotta voit aloittaa offboarding tai onboarding yleisen järjestelmänvalvojan luvan tunnus.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että olemme on luotu salasana salausavaimen, jota käytetään salaamaan salasanat lähetetään paikallisen ympäristön pilvestä.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman osoittaa Tuntematon virhe salasanan hallinta-toiminnon aikana. Tarkista tapahtuman lisätietoja poikkeuksen-teksti. Jos sinulla on ongelmia, yritä poistaa käytöstä ja ottaminen takaisin käyttöön salasanan takaisinkirjoituksen. Jos tämä ei auta, Sisällytä kopion oman tapahtumaloki sekä määritetty seurantatunnus sisäpiirikaupoista oman tukihenkilö.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee ilmeni virhe yhteyden cloud salasanan palauttaminen palvelun ja yleensä ilmenee, kun paikallisen palvelu ei voi muodostaa salasanan palauttaminen web-palvelu. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman osoittaa, että vuokraajan bus esiintymää muodostamisesta virhe. Tämä voi johtua siitä lähtevät yhteydet estävät paikallisen ympäristön. Tarkista palomuurin varmistaaksesi sallivat TCP 443 päälle ja <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>, ja yritä sitten uudelleen. Jos sinulla on edelleen ongelmia, yritä poistaminen käytöstä ja ottaminen takaisin käyttöön salasanan takaisinkirjoituksen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee web meidän service API välitetään syöte on virheellinen. Yritä uudelleen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että tapahtui virhe salauksen salasanaa, jolla on saapunut pilvestä. Tämä virhe voi johtua salauksen avaimen ristiriita pilvipalvelussa ja paikallisen ympäristön välillä. Jotta voit ratkaista tämän käytöstä ja uudelleen käyttöön salasanan takaisinkirjoituksen paikallisen ympäristön.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Aikana onboarding emme tallenna vuokraajan koskevien tietojen syöttölomake kokoonpanotiedosto paikallisen ympäristön. Tapahtuman osoittaa virhe tämän tiedoston tallentaminen tai, kun palvelun aloittamisesta on oli virhe luettaessa tiedostoa. Voit korjata tämän ongelman, yritä poistamista ja ottaminen uudelleen käyttöön salasanan takaisinkirjoituksen pakottaminen uudelleen kirjoittaminen määritysten tiedoston. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>POISTETTU – tätä tapahtumaa ei ole Azure AD Connect, muodostaa vain hyvin alkuvaiheessa, mitä tue takaisinkirjoituksen Dirsync-työkalun.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Onboarding, aikana lähetämme tietoja pilvestä paikallisen salasanan palauttaminen palvelu. Tiedot on kirjoitettu ladatun tiedoston ennen lähettämistä tallentaa tiedot suojatusti-synkronointi-palveluun. Tapahtuman osoittaa ongelman kirjoittaminen tai muistiin tiedot päivitetään. Voit korjata tämän ongelman, yritä poistamista ja ottaminen uudelleen käyttöön salasanan takaisinkirjoituksen pakottaminen uudelleen kirjoittaminen tähän määritys.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman osoittaa, että virheellisen vastauksen salasanan palauttaminen web-palvelu. Voit korjata tämän ongelman, yritä poistamista ja ottaminen uudelleen käyttöön salasanan takaisinkirjoituksen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että Microsoft ei voitu hakea luvan suojaustunnuksen valitun yleisen järjestelmänvalvojan tilin Azure AD Connect asennuksen aikana. Tämä virhe voi johtua virheelliset käyttäjänimi ja salasana määritetty yleisen järjestelmänvalvojan tililtä tai koska yleisen järjestelmänvalvojan tililtä määritetty on liitetty. Voit korjata tämän ongelman, Suorita uudelleen määritysten oikea käyttäjänimellä ja salasanalla ja varmista järjestelmänvalvoja on hallitun (vain pilvipalveluita tai salasanan synkronointi) tili.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee tapahtui virhe luotaessa salasana salausavain tai salauksen salasanaa, jolla saapuu perusteella. Tämä virhe todennäköisesti osoittaa ympäristön ongelmasta. Tarkista, että tapahtumaloki Saat lisätietoja ja ratkaista ongelman tiedot. Voit myös yrittää käytöstä ja ottaminen takaisin käyttöön salasanan takaisinkirjoituksen palvelun voit ratkaista ongelman.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että paikallisen palvelun ei voitu oikein muodostaa yhteyden onboarding salasanan palauttaminen web-palvelun kanssa. Tämä voi johtua palomuurisäännön tai käytön auth-tunnuksen vuokraajan ongelma. Voit korjata ongelman, että sinulla ei ole estävät Lähtevät yhteydet TCP 443 ja TCP 9350 9354 tai <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>ja AAD järjestelmänvalvojatilin avulla määrän ei äänipuheluita. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>POISTETTU – tätä tapahtumaa ei ole Azure AD Connect, muodostaa vain hyvin alkuvaiheessa, mitä tue takaisinkirjoituksen Dirsync-työkalun.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että paikallisen palvelua ei voitu oikein muodostaa voit aloittaa offboarding salasanan palauttaminen web-palvelun kanssa. Tämä voi johtua palomuurisäännön tai käytön todennus-tunnuksen vuokraajan ongelma. Voit korjata ongelman Varmista, että ovat ei estä lähtevien yhteyksien kautta 443 tai <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>ja että AAD järjestelmänvalvojatilin avulla Palveluksesta poistuneet ei ole liitetty. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että on ollut yrittää muodostaa yhteyttä vuokraajan bus esiintymää. Tavallisesti tämä olisi ei olla merkitystä, mutta jos näet tapahtuman monta kertaa, harkitse tarkistetaan verkkoyhteys palvelun bus erityisesti, jos se on odotusaika tai yhteyttä.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Jotta salasana takaisinkirjoituksen palvelun kunnon valvonta, lähetämme uudelleenaktivoinnin yhteydessä tietojen salasana palauttaa verkkopalvelun 5 minuutin välein. Tapahtuman ilmaisee, että virhe lähetettäessä kunto tiedot takaisin web pilvipalvelussa. Kunto tiedot OII tai henkilökohtaisia tietoja tiedot eivät sisällä, eikä se on laskettuna uudelleenaktivoinnin yhteydessä ja basic palvelun tilastotiedot, jotta voimme tarjota palvelun tilatietojen pilveen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että ollut AD palauttama Tuntematon virhe, tarkista Azure AD Connect palvelimen tapahtumaloki tapahtumien ADSync lähteestä lisätietoja virheestä.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että sen käyttäjän salasanan vaihtaminen tai palauttaminen yritetään ei löytynyt paikallisesta hakemistosta. Näin voi käydä, kun käyttäjä on poistettiin paikallisesti, mutta ei pilvipalvelussa, vai onko synkronoinnin ongelma. Tarkista synkronointi-lokit sekä viimeisen lataustiedot suorittaa muutama synkronointi.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Kun salasanan tai muutospyyntö peräisin pilveen, Käytämme Azure AD Connect asennuksen aikana määritetyn cloud ankkurin ja kysyttävä, kuinka haluat yhdistää pyynnön käyttäjän paikallisen ympäristön. Tapahtuman ilmaisee, että löydetyillä kaksi käyttäjää saman cloud ankkuri-määritteen paikallisen hakemistossa. Tarkista synkronointi-lokit sekä viimeisen lataustiedot suorittaa muutama synkronointi.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että hallinta-agentti palvelutilin ei ole tarvittavat käyttöoikeudet määrittää uuden salasanan kyseiseltä tililtä. Varmista, käyttäjän metsää MA-tili on Palauta ja muuta salasana-käyttöoikeudet metsää kaikki objektit.  Lisätietoja siitä, miten toimia, katso <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">Vaihe 4: Active Directory tarvittavat käyttöoikeudet määrittää</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että olemme yritti, jotka on poistettu käytöstä paikallisen tilin salasanan vaihtaminen tai palauttaminen. Ota tili ja yritä sitten uudelleen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että olemme yritti tilille, jolla on paikallinen lukittu salasanan vaihtaminen tai palauttaminen. Lukitsemisten voi ilmetä, kun käyttäjä on yrittänyt muutoksen tai Palauta salasana liian monta kertaa lyhyen-toimintoa. Lukituksen ja yritä sitten uudelleen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmaisee, että käyttäjän määrittämän virheellisen nykyisen salasanan toiminnon muuttuessa suorittamiseen salasanan. Määritä nykyinen salasana ja yritä uudelleen.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman ilmenee, kun salasana takaisinkirjoituksen palvelu yrittää määrittää salasanan, joka ei täytä salasanan ikä, historia, monimutkaisuus tai suodattaminen vaatimukset toimialueen paikallisen hakemistossa.</p>
              <ul>
                <li class="unordered">
Jos olet salasanan iän ja viimeksi muuttanut salasanan ikkunan ajan kuluessa, voit voi vaihtaa salasanan uudelleen, kunnes ohjausobjekti määritetyn ikä toimialueen. Testausta varten vähintään olisi arvoksi 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jos sinulla on käytössä historia-salasanavaatimukset ja valitse sinun on valittava salasanaa, jolla ei ole käytetty viimeisen N ajat, jos N on salasana historia-asetus. Jos valitset salasanaa, jolla on käytetty viimeisen N ajat, valitse näet virheen tällöin. Testausta varten historia olisi arvoksi 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jos sinulla on monimutkaisuus salasanavaatimukset, ne kaikki pakotetaan suoritettaviksi, kun käyttäjä yrittää muuttaminen tai salasanan.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jos sinulla on käytössä salasanan suodattimet ja käyttäjä valitsee salasanan, joka ei täytä suodatusehtoa, valitse Palauta tai muuttaa toiminto epäonnistuu.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tapahtuman osoittaa ilmeni ongelma-kirjoittaminen, salasanan takaisin paikalliseen hakemistossa vuoksi määritysongelmia Active Directory-hakemistosta. Tarkista Azure AD Connect-koneen tapahtumaloki viestien lisätietoja mitä virhe ADSync-palvelusta. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>POISTETTU – tätä tapahtumaa ei ole Azure AD Connect, muodostaa vain hyvin alkuvaiheessa, mitä tue takaisinkirjoituksen Dirsync-työkalun.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>POISTETTU – tätä tapahtumaa ei ole Azure AD Connect, muodostaa vain hyvin alkuvaiheessa, mitä tue takaisinkirjoituksen Dirsync-työkalun.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>POISTETTU – tätä tapahtumaa ei ole Azure AD Connect, muodostaa vain hyvin alkuvaiheessa, mitä tue takaisinkirjoituksen Dirsync-työkalun.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Salasanan takaisinkirjoituksen yhteyksien vianmääritys

Jos kohtaat palvelun keskeytyksiä Azure AD Connect niin, että salasana takaisinkirjoituksen osa, seuraavassa on joitakin vaiheittaiset ohjeet, voit ratkaista ongelman:

 - [Uudelleenkäynnistys Azure AD yhteyden synkronointi-palvelu](#restart-the-azure-AD-Connect-sync-service)
 - [Poista käytöstä ja ottaminen käyttöön salasanan takaisinkirjoituksen-ominaisuus](#disable-and-re-enable-the-password-writeback-feature)
 - [Azure AD Connect uusimman version asentaminen](#install-the-latest-azure-ad-connect-release)
 - [Salasanan takaisinkirjoituksen vianmääritys](#troubleshoot-password-writeback)

On suositeltavaa, että siinä järjestyksessä, jotta voit palauttaa palvelun nopeita tavalla yllä suorittaa nämä vaiheet.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Uudelleenkäynnistys Azure AD yhteyden synkronointi-palvelu
Azure AD yhteyden synkronointi-palvelun käynnistäminen uudelleen voi auttaa yhteysongelmien tai palveluun muiden lyhytkestoisia ongelmien ratkaisemiseen.

 1. Järjestelmänvalvoja Valitse **Aloita** **Azure AD Connect**-palvelimessa.
 2. Kirjoita hakukenttään **"services.msc"** ja paina **Enter**-näppäintä.
 3. Katso **Microsoft Azure AD Connect** -kohtaa.
 4. Palvelun tapahtuman, valitse **Käynnistä**hiiren kakkospainikkeella ja odota toiminto on valmis.

    ![][002]

Nämä vaiheet muodostaa uudelleen yhteyden pilvipalvelussa ja ratkaista mistään keskeytymisistä saattaa ilmetä.  Jos synkronoi-palvelun uudelleenkäynnistys ei ratkaise ongelmaa, on suositeltavaa yritä seuraavaan vaiheeseen nimellä salasanan takaisinkirjoituksen-toiminnon ottaminen käyttöön ja poistaminen käytöstä.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Poista käytöstä ja ottaminen käyttöön salasanan takaisinkirjoituksen-ominaisuus
Poistaminen käytöstä ja ottaminen takaisin käyttöön salasanan takaisinkirjoituksen-ominaisuuden avulla yhteysongelmien ratkaisemiseen.

 1. Järjestelmänvalvoja Avaa **Ohjattu määritystoiminto Azure AD Connect**.
 2. Valitse **Muodosta yhteys Azure AD** -valintaikkunassa Anna **Azure AD yleisen järjestelmänvalvojan tunnistetiedot**
 3. Valitse **Muodosta yhteys AD DS** -valintaikkunassa Anna **AD-toimialueen palveluista järjestelmänvalvojan tunnistetietoja**.
 4. **Yksilöivä käyttäjät** -valintaikkunassa valitse **Seuraava** .
 5. Valitse **Valinnaiset ominaisuudet** -valintaikkunassa Poista **salasana takaisinkirjoituksen** -valintaruudun valinta.

    ![][003]

 6. Valitse **Seuraava** jäljellä valintaikkunan-sivujen kautta muuttamatta mitään, siirry **Ready to määrittäminen** sivulle.
 7. Varmista, että määrittäminen-sivulla näkyy **salasanan takaisinkirjoituksen vaihtoehto käytöstä** ja valitse sitten vihreä **Määritä** -painiketta voit tallentaa muutokset.
 8. Valitse **Valmis** -valintaikkunassa Poista **Synkronoi nyt** -vaihtoehto ja valitse sitten Sulje ohjattu toiminto **loppuun** .
 9. Avaa **Azure AD Connect ohjattu määritystoiminto**uudelleen.
 10.    **Toista vaiheet 2-8**, paitsi varmistaa **Tarkista, onko salasana takaisinkirjoituksen-asetus** käyttöön **valinnaisia ominaisuuksia** näytön palvelun uudelleen käyttöön.

    ![][004]

Nämä vaiheet muodostaa uudelleen yhteyden Microsoftin pilvipalvelussa ja ratkaista mistään keskeytymisistä saattaa ilmetä.

Jos poistaminen käytöstä ja ottaminen takaisin käyttöön salasanan takaisinkirjoituksen-ominaisuus ei ratkaise ongelmaa, on suositeltavaa, että yrität asentaa uudelleen Azure AD Connect seuraavaan vaiheeseen nimellä.

### <a name="install-the-latest-azure-ad-connect-release"></a>Azure AD Connect uusimman version asentaminen
Azure AD Connect pakettia asennettaessa uudelleen ratkaisee minkä tahansa määritysongelmat, jotka voivat vaikuttaa joko järjestelmänvalvoja muodostaa yhteyttä Microsoftin pilvipalveluihin tai AD paikallisen ympäristön salasanojen hallinta.
On suositeltavaa, suorita tätä vaihetta vasta, kun yrität yläpuolella on kuvattu kaksi ensimmäistä vaihetta.

 1. Lataa Azure AD Connect uusin versio [tästä](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Koska olet jo asentanut Azure AD Connect, sinun on vain käytönaikainen päivittämisestä Azure AD Connect-asennuksen päivittäminen uusimpaan versioon.
 3. Suorita ladattu paketin ja noudata näytön ohjeita Päivitä Azure AD Connect tietokone.  Ei ole muita manuaalinen toimia ei tarvita, paitsi jos olet mukauttanut poissa-ruutuun synkronoinnin säännöt, jolloin kannattaa **varmuuskopioida ne ennen jatkamista Päivitä ja ota ne sen jälkeen, kun olet valmis manuaalisesti uudelleen**.

Nämä vaiheet muodostaa uudelleen yhteyden Microsoftin pilvipalvelussa ja ratkaista mistään keskeytymisistä saattaa ilmetä.

Jos Azure AD Connect palvelimen uusimman version asentaminen ei ratkaise ongelmaa, on suositeltavaa, yritä poistamista ja ottaminen uudelleen käyttöön salasanan takaisinkirjoituksen kuin viimeisessä vaiheessa uusimman Synkronoi QFE asentamisen jälkeen.

Jos tämä ei ratkaise ongelmaa, valitse Suosittelemme voit tarkastella [Vianmääritys salasanan takaisinkirjoituksen](#troubleshoot-password-writeback) ja [Azure AD-salasanan hallinta usein kysytyt kysymykset](active-directory-passwords-faq.md) nähdäksesi, jos ongelmaasi saattaa käsitellään siellä.


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Linkkejä salasanan palauttaminen dokumentaatio
Alla on linkkejä kaikkiin Azure AD-salasanan ohjeissa:

* **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).
* [**Toiminta**](active-directory-passwords-how-it-works.md) - palveluun ja kuusi eri osat tietoja kunkin onko
* [**Aloittaminen**](active-directory-passwords-getting-started.md) – Katso, miten, jotta käyttäjät voivat palauttaa ja muuttaa cloud tai paikalliseen salasanansa
* [**Mukauta**](active-directory-passwords-customize.md) - Opi mukauttamaan ulkoasun ja ilmeen ja palvelun organisaation tarpeisiin toiminta
* [**Parhaat käytännöt**](active-directory-passwords-best-practices.md) - Lue, miten voit ottaa nopeasti ja tehokkaasti salasanojen organisaation hallinta
* [**Hae tiedot**](active-directory-passwords-get-insights.md) - tietoja Microsoftin integroitu raportointiominaisuudet
* [**Usein kysytyt kysymykset**](active-directory-passwords-faq.md) - vastauksia usein kysyttyihin kysymyksiin
* [**Lisätietoja**](active-directory-passwords-learn-more.md) – Valitse syvä kyselyjä teknisiä tietoja palvelun toiminta



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
