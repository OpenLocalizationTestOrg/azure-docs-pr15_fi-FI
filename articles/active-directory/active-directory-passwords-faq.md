<properties
    pageTitle="Usein kysytyt kysymykset: Azure AD-salasanojen hallinta | Microsoft Azure"
    description="Usein kysyttyjä kysymyksiä salasanojen hallinta Azure AD-, mukaan lukien salasanan, rekisteröinnin, raportit ja takaisinkirjoituksen paikallisen Active Directory."
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

# <a name="password-management-frequently-asked-questions"></a>Salasanojen hallinta usein kysytyt kysymykset

> [AZURE.IMPORTANT] **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).

Seuraavassa on joitakin usein kysytyt kysymykset liittyvät salasanojen hallinta asioista.

Jos tiedä, tiedä vastauksen tai tietty ongelma on vastakkaisten apua etsimäsi kysymyksen kanssa, voit lukea alla nähdäksesi, jos Microsoft on kattaa se jo.  Jos on vielä vahvistamatta, älä huoli! Vapaasti Kysy kaikkia kysymyksiä, joita ei ole annettu tähän [Azure AD-keskustelupalstoilla](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) on ja otamme sinulle heti, kun emme voi.

Seuraavissa osissa on jaettu tämä usein kysyttyjä Kysymyksiä:

- [**Kysymyksiä salasanan palauttaminen rekisteröinti**](#password-reset-registration)
- [**Kysymyksiä salasanan palauttaminen**](#password-reset)
- [**Salasanan hallintaraporttien koskeviin kysymyksiin**](#password-management-reports)
- [**Salasanan takaisinkirjoituksen koskeviin kysymyksiin**](#password-writeback)

## <a name="password-reset-registration"></a>Salasanan rekisteröinti
 - **K: Voinko Omat käyttäjät rekisteröityä oman salasanan palauttaminen tiedot?**

 > **A:** Kyllä, kunhan salasanan on otettu käyttöön ja niiden käyttöoikeus, he voivat siirtyä salasanan palauttaminen rekisteröinti-portaalissa osoitteessa http://aka.ms/ssprsetup rekisteröidä niiden todennustiedot käytettävän salasanan. Käyttäjät voivat myös rekisteröityä siirtymällä osoitteessa http://myapps.microsoft.com access-paneeli, valitsemalla profiili-välilehti ja valitsemalla Rekisteröi salasanan asetus. Lisätietoja hankkiminen käyttäjillesi määritetty salasanan lukemalla käyttäjät, jotka on määritetty salasanan hankkiminen.

 - **K: voin määrittää salasanan palauttaminen tietoja aiemmin käyttäjien puolesta?**

 > **A:** Kyllä, voit tehdä DirSync tai PowerShell tai palvelun [Azure hallinta-portaalin](https://manage.windowsazure.com) tai Officen järjestelmänvalvoja-portaalissa. Lisätietoja tämän ominaisuuden käyttöön entistä parempi tietosuoja Azure AD MFA ja salasanan palauttaminen puhelinnumerot ja lukemalla Katso, kuinka tietoja käytetään salasanalla blogimerkinnän nollaa.

 - **K: Voinko tietojen suojauksen kysymyksiä paikallinen synkronointi?**

 > **A:** Tämä ei ole mahdollista tänään, mutta olemme ovat lähitulevaisuudessa.

 - **K: Voinko Omat käyttäjät rekisteröityä tiedot siten, että muut käyttäjät eivät näe näitä tietoja?**

 > **A:** Kyllä, kun käyttäjät rekisteröityä tietojen salasanan palauttaminen rekisteröinti portaalissa saa tallentaa sen yksityisen käyttöoikeuden kenttiin, jotka näkyvät vain Yleiset Järjestelmänvalvojat ja käyttäjän itsestään. Lisätietoja tämän ominaisuuden käyttöön entistä parempi tietosuoja Azure AD MFA ja salasanan palauttaminen puhelinnumerot ja lukemalla Katso, kuinka tietoja käytetään salasanalla blogimerkinnän nollaa.

 - **K: Omat käyttäjät on rekisteröitävä, ennen kuin he voivat käyttää salasanan?**

 > **A:** Ei, jos määrität tarpeeksi todennustiedot heidän puolestaan, käyttäjien ei tarvitse rekisteröidä. Salasanan toimii vain hyvin, kunhan hakemiston vastaaviin kenttiin tallennetut tiedot on muotoiltu oikein. Lue lisää lukemalla lisätietoja siitä, miten tietoja käytetään salasanan.

 - **K: Voinko synkronoida tai määrittää todennus-puhelin, todennus sähköpostin tai vaihtoehtoinen todennus puhelimen kenttiä aiemmin käyttäjien puolesta?**

 > **A:** Tällä hetkellä mutta olemme lähitulevaisuudessa ottaminen käyttöön tätä ominaisuutta.

 - **K: miten rekisteröinti portaalin tietää mitä asetuksia näyttämään aiemmin käyttäjiä?**

 > **A:** Salasanan palauttaminen rekisteröinti portaalin näyttää vain asetukset, että olet ottanut käyttäjien oman kansion määrittäminen välilehti käyttäjän salasanan palauttaminen käytäntöä-osassa. Tämä tarkoittaa, että jos ei ole käytössä, sano tietoturvaan liittyvät kysymykset sitten käyttäjät eivät voi rekisteröidä vaihtoehto.

 - **K:, kun käyttäjä katsotaan rekisteröity?**

 > **A:** Käyttäjä katsotaan rekisteröityjen silloin, kun hän on oltava vähintään N laitteita todennus tiedot määritetty, jossa N on luku, todennus menetelmiä pakollinen-, jotka olet määrittänyt [Azure hallinta-portaalin](https://manage.windowsazure.com). Lisätietoja on artikkelissa mukauttaminen käyttäjän salasanan palauttaminen käytäntöä.


## <a name="password-reset"></a>Salasanan palauttaminen

 - **K: miten kauan vastaanottamaan sähköpostia, Tekstiviesti tai puhelu salasanan?**

 > **A:** Sähköpostin, tekstiviestien ja puhelut olisi saapuvat alle minuutin, ja Normaali tapaus 5 – 20 sekuntia. Jos tämä aikajakson eivät saa ilmoituksen, valitse Roskaposti-kansioosi, numero / sähköpostiosoite, johon yhteyttä on yksi odotat ja todentamistiedot hakemistossa on muotoiltu oikein. Lisätietoja muotoilun puhelinnumerot- ja sähköpostitoimialueitasi osoitteet salasanan käytettäväksi Katso Lue, miten tietoja käytetään salasanan.

 - **K: mitä kieliä tukee salasanan?**

 > **A:** Salasanan Käyttöliittymä, tekstiviestien ja äänipuheluissa ole lokalisoitu saman 40 kielet, joita tuetaan Office 365: ssä. Siinä ovat: arabia, Bulgaria, yksinkertaistetun kiinan, kiina-perinteinen, Kroatia, tšekki, Tanska, hollanti, englanti, Viro, Suomi, ranska, saksa, kreikka, heprea, Hindi, unkari, Indonesia, italia, japani, kazakki, korea, Latvia, Liettua, malaiji (Malesia), norja (Bokmål), puola, portugali (Brasilia), portugali (Portugali), Romania, venäjä, serbia (latinalainen), slovakki, sloveeni, espanja, Ruotsi, Thai, Turkki, Ukraina, ja vietnam.

 - **K: mitä osia salasanan palauttaminen käyttökokemuksesta Hae mukautettu, kun määritän organisaation Omat Directoryn mukauttaminen käyttäjän määrittää välilehden?**

 > **A:** Salasanan palauttaminen portaalin näkyy organisaation logo ja myös mahdollistaa yhteyshenkilön määrittäminen järjestelmänvalvojan linkki osoittamaan mukautettua sähköpostiosoitetta tai URL-osoite. Mistä tahansa sähköpostista, joka lähetetään saa salasanan kutsussa on organisaation logo, (Tässä tapauksessa punainen), värien nimi tekstiosaan sähköposti- ja mukautettu nimi. Esimerkki kaikki yrityskuvaa tukevan elementtien alla. Lisätietoja, lue mukauttaminen salasanan ulkoasu.

  ![][001]

 - **K: Miten voin kerro Omat käyttäjille mistä vaihtamaan salasanansa?**

 > **A:** Voit lähettää käyttäjille https://passwordreset.microsoftonline.com suoraan tai ne napsauttamalla voit pyydät Eikö löytyy koulun tai työpaikan tunnus sisäänkirjautuminen näytön tilin linkki. Voit vapaasti julkaista linkkejä (tai luoda URL-uudelleenohjauksen niihin)-paikkaa, jossa on helposti saatavilla käyttäjille.

 - **K: Voinko käyttää tätä sivua mobiililaitteeseen?**

 > **A:** Kyllä, tämä sivu toimii mobiililaitteissa.

 - **K: eivät tue lukituksen poisto paikallisen active directory-tilit kun käyttäjien salasanojen niiden?**

 > **A:** Kyllä, kun käyttäjä palauttaa yhteystietoluetteloonsa salasana ja salasanan takaisinkirjoituksen on otettu käyttöön Azure AD Connect kaikkien versioiden tai Azure AD-synkronointi 1.0.0485.0222 versiota tai uudempi versio, sitten kyseisen käyttäjän tiliin on automaattisesti lukitus, kun käyttäjä palauttaa salasanansa.

 - **K: miten voit integroida suoraan oma käyttäjän työpöydän kirjautuminen salasanan?**

 > **A:** Tämä ei ole mahdollista tänään. Jos tämä ominaisuus on ehdottoman ja on Azure AD Premium-tilaus, voit asentaa Microsoft käyttäjätietojen hallinta ilman lisäkustannuksia ja paikallisen salasanan palauttaminen ratkaisu siinä ratkaisemaan tämä vaatimus käyttöön.

 - **K: Voinko määrittää eri tietoturvaan liittyvät kysymykset eri kieliversioille?**

 > **A:** Tämä ei ole mahdollista tänään, mutta olemme ovat lähitulevaisuudessa.

 - **K: miten monta kysymyksiä Microsoft määrittää tietoturvaan liittyvät kysymykset todennusta-asetus?**

 > **A:** Voit määrittää enintään 20 mukautetun tietoturvaan liittyvät kysymykset [Azure hallinta-portaalin](https://manage.windowsazure.com).

 - **K: miten kauan tietoturvaan liittyvät kysymykset voidaan?**

 > **A:** Tietoturvaan liittyvät kysymykset voi olla 3-200 merkkiä.

 - **K: miten kauan tietoturvaan liittyvät kysymykset vastauksia ehkä?**

 > **A:** Vastauksia voi olla 3-40 merkin pituinen.

 - **K: on hylätty tietoturvaan liittyvät kysymykset kaksoiskappaleiden vastauksia?**

 > **A:** Kyllä, emme hylkää kaksoiskappaleet vastauksia tietoturvaan liittyvät kysymykset.

 - **K: toukokuu käyttäjän rekisteröidä useamman kuin yhden samaa suojauksen kysymystä?**

 > **A:** Ei, kun käyttäjä rekisteröi tietyn kysymys, hän ei välttämättä rekisteröi kyseisen kysymyksen toisen kerran.

 - **K: onko alaraja tietoturvaan liittyvät kysymykset rekisteröinnin ja palauttaa?**

 > **A:** Kyllä, yksi raja voidaan määrittää rekisteröinti ja toinen Palauta. 3 – 5 tietoturvaan liittyvät kysymykset voidaan tarvita rekisteröinnin ja 3 – 5 voidaan tarvita Palauta.

 - **K: Jos käyttäjä on rekisteröity yli enimmäismäärä on pakollinen vaihtamaan kysymyksiä, miten tietoturvaan liittyvät kysymykset valitaan Palauta aikana?**

 > **A:** N tietoturvaan liittyvät kysymykset valitaan satunnaisesti kysymyksiä käyttäjä on rekisteröity, jossa N on vähimmäismäärä kysymyksiä salasanan vaaditaan kokonaismäärän ulos. Esimerkiksi jos käyttäjällä on 5 tietoturvaan liittyvät kysymykset rekisteröity, mutta vain 3 tarvitaan vaihtamaan, 3 / näiden 5 voidaan valita satunnaisesti ja esitetään käyttäjälle palauttaminen aikaan. Jos käyttäjä saa vastauksia kysymyksiin väärä, valinnan prosessi tapahtuu uudelleen Voit estää kysymyksen toistuvien yritysten eston.

 - **K: Voit estää käyttäjiä yritetään monta kertaa-lyhyen ajan salasanan?**

 > **A:** Kyllä, ovat eri suojausominaisuudet salasanan. Käyttäjät voivat yrittää vain 5 salasanan palauttaminen yritykset tuntia ennen on lukittu 24 tunnin kuluessa. Käyttäjät voivat vain yrittävät Tarkista puhelinnumero 5 luvulla tuntia ennen on lukittu 24 tunnin kuluessa. Käyttäjien yrittää vain yhden todentamismenetelmän 5 luvulla tuntia ennen on lukittu 24 tunnin kuluessa.

 - **K: miten kauan ovat sähköpostin ja SMS kertakäyttöinen tunnuskoodi voimassa?**

 > **A:** Istunnon elinkaaren salasanan varten on 105 minuuttia. Tämä tarkoittaa, että alusta salasanan palauttaminen toiminnon, käyttäjän on 105 minuutin yhteystietoluetteloonsa salasanan. Sähköpostin ja SMS kertakäyttöinen tunnuskoodi ovat virheellisiä, tämän ajanjakson päätyttyä.


## <a name="password-management-reports"></a>Salasanojen hallinta-raportit

 - **K: miten paljon kuluu aikaa tietojen salasanan hallintaraporttien näy?**

 > **A:** Tietojen pitäisi näkyä salasanan hallintaraporttien-5-10 minuutin kuluessa. Se tietyissä tilanteissa, voi kestää tunneiksi näkyvän.

 - **K: Miten voin suodattaa salasanan hallintaraporttien?**

 > **A:** Voit suodattaa salasanan hallintaraporttien napsauttamalla pieni Suurennuslasia erittäin oikealla puolella sarakeotsikot raportin alussa (katso näyttökuva). Jos haluat tehdä tehokkaan suodattaminen, voit ladata raportin excel- ja luo pivot-taulukko.

  ![][002]

 - **K: mikä on tapahtumien enimmäismäärä on tallennettu salasana hallintaraporttien?**

 > **A:** Enintään 1 000 salasanan palauttaminen ja salasanan Palauta rekisteröinti tapahtumat tallennetaan salasanan hallintaraporttien.  Yritämme Laajenna sisällytettävien tapahtumien tämän numeron.

 - **K: kuinka kauas takaisin salasanan hallintaraporttien Siirry?**

 > **A:** Salasanan hallintaraporttien Näytä viimeisten 30 päivän määrä toimintoja. Microsoft tutkii parhaillaan voit tehdä pitkän ajan kuluessa. Nyt, jos haluat arkistoida tiedoista, voit ladata raporttien säännöllisesti ja tallennetaan eri paikkaan.

 - **K: onko enimmäismäärä rivit, jotka voivat olla salasana hallintaraporttien?**

 > **A:** Kyllä, enintään 1 000 riviä saattaa näkyä joko salasanojen hallinta-raportteja, onko ne näkyvät käyttöliittymässä tai ladataan. Microsoft tutkii parhaillaan kasvattaminen rajoitusta.

 - **K: onko Ohjelmointirajapinnan salasanan tai raportointitiedot rekisteröinti?**

 > **A:** Kyllä, Katso seuraavat ohjeissa kerrotaan, miten voit käyttää reporting tietovirta salasanan.  [Lue käyttäminen salasanan palauttaminen raportoinnin tapahtumien ohjelmallisesti](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Salasanan takaisinkirjoituksen
 - **K: miten salasanan takaisinkirjoituksen toimii taustalla?**

 > **A:** Katso [miten salasanan takaisinkirjoituksen toimii](active-directory-passwords-learn-more.md#how-password-writeback-works) yksityiskohtainen selvitys siitä, mitä tapahtuu, kun otat salasanan takaisinkirjoituksen sekä kuinka tietoja kulkee järjestelmässä takaisin paikalliseen ympäristöön. Artikkelissa kerrotaan, miten varmistamme salasanan takaisinkirjoituksen on suojatuissa palvelu toimii kuinka salasana takaisinkirjoituksen [salasanan takaisinkirjoituksen suojausmalli](active-directory-passwords-learn-more.md#password-writeback-security-model) .

 - **K: miten kauan salasanan takaisinkirjoituksen kestää toimii?  Onko synkronointi viive kuten salasana hash synkronoinnin?**

 > **A:** Salasanan takaisinkirjoituksen on pikaviestien. Se on synkronoitu putkijohto, joka toimii olennaisesti eri tavalla kuin salasanojen hash synkronoinnin. Salasanan takaisinkirjoituksen avulla käyttäjät voivat saada palautetta reaaliaikainen niiden salasanan onnistuu tai muuttaa toimintoa. Keskimääräinen aika onnistuneen takaisinkirjoitusta salasanan varten on kohdassa 500 ms.

 - **K: minkä tyyppisiin tileihin salasanan takaisinkirjoitusta käytetään?**

 > **A:** Takaisinkirjoituksen salasanalla työskentelee organisaation ulkopuolisten ja salasanan Hash synkronointi d: n käyttäjille.

 - **K: salasana takaisinkirjoituksen pakottaa toimialueen salasanakäytännöt?**

 > **A:** Kyllä, salasana takaisinkirjoituksen pakottaa salasanan ikä, historia, monimutkaisuus, suodattimet ja paikallisen toimialueen salasanat paikalla voi lisätä muun rajoituksen.

 - **K: onko salasana takaisinkirjoituksen suojatun?  Miten voin varmistaa, voin ei Hae hakkeroitu?**

 > **A:** Salasanan takaisinkirjoituksen on erittäin suojattu. Saat lisätietoja 4 käyttölupiin toteutettu salasanan takaisinkirjoituksen-palvelu, katso kuinka salasana takaisinkirjoituksen works [salasanan takaisinkirjoituksen suojausmalli](active-directory-passwords-learn-more.md#password-writeback-security-model) .




## <a name="links-to-password-reset-documentation"></a>Linkkejä salasanan palauttaminen dokumentaatio
Alla on linkkejä kaikkiin Azure AD-salasanan ohjeissa:

* **Oletko tähän koska sinulla on ongelmia kirjautumisessa?** Jos näin on, [seuraavalla siitä, miten voit muuttaa ja oman salasanan](active-directory-passwords-update-your-own-password.md).
* [**Toiminta**](active-directory-passwords-how-it-works.md) - palveluun ja kuusi eri osat tietoja kunkin onko
* [**Käytön aloittaminen**](active-directory-passwords-getting-started.md) - tietoja, jotta käyttäjät voivat palauttaa ja muuttaa cloud tai paikalliseen salasanansa
* [**Mukauta**](active-directory-passwords-customize.md) - Opi mukauttamaan ulkoasun ja ilmeen ja palvelun organisaation tarpeisiin toiminta
* [**Parhaat käytännöt**](active-directory-passwords-best-practices.md) - Lue, miten voit ottaa nopeasti ja tehokkaasti salasanojen organisaation hallinta
* [**Hae tiedot**](active-directory-passwords-get-insights.md) - tietoja Microsoftin integroitu raportointiominaisuudet
* [**Vianmääritys**](active-directory-passwords-troubleshoot.md) – Opi palvelun nopeasti liittyviä ongelmia
* [**Lisätietoja**](active-directory-passwords-learn-more.md) – Valitse syvä kyselyjä teknisiä tietoja palvelun toiminta


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
