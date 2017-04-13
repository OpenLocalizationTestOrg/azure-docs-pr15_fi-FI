<properties
    pageTitle="Azure AD Connect synkronointi: Määritä suodatus | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan Azure AD Connect synkronointi suodatuksen määrittämisestä."
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
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect synkronointi: Määritä suodatus
Voit valvoa suodattaminen, mitkä objektit pitäisi näkyä Azure AD-paikallisesta hakemistosta. Oletusarvon määrittäminen kestää kaikki objektit kaikki määritetyn metsien Domains. Tämä on yleensä suositellut määritykset. Loppukäyttäjät käyttämällä Office 365-toiminnoista, kuten Exchange Onlinen ja Skype for Business-hyötyvät valmis yleisestä osoiteluettelosta, niin he voivat lähettää sähköpostia ja Soita kaikille. Oletus-kokoonpanoa ne saisit tavallisen paikallisen soveltaminen Exchangen, Lyncin tai muiden saman kokemus.

Joissakin tapauksissa se edellyttää tehdä muutoksia työnkulkuun oletusarvon. Seuraavassa on joitakin esimerkkejä:

- Aiot käyttää [usean Azure Active directory-topologian](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory). Valitse edellyttävät suodatin, joka määrittää, mikä objekti synkronoidaanko tiettyyn Azure AD-kansio.
- Koekäytön Suorita Azure-tai Office 365: ssä ja haluat käyttäjien alijoukkoa vain Azure AD. Pieni kokeilla ei ole tärkeä on valmis osoittamaan toimintoja yleisestä osoiteluettelosta.
- Sinulla on useita palvelutilejä ja joita et halua Azure AD-personal muihin tileihin.
- Yhteensopivuuden vuoksi et poista kaikki käyttäjän tilit-ympäristöön. Voit vain poistaa ne käytöstä. Mutta Azure AD-haluat vain aktiiviset asiakkaat on olemassa.

Tässä artikkelissa kerrotaan, miten voit määrittää suodatuksen menetelmistä.

> [AZURE.IMPORTANT]Microsoft ei tue muokkaamista tai toiminnan Azure AD Connect Synkronoi varsinaisesti kuvattu toimien ulkopuolella. Mitään näistä toiminnoista saattaa johtaa epäyhtenäisiä tai ei-tuettu Azure AD Connect synkronoinnin tilan ja tuloksena Microsoft ei anna teknistä tukea tällaisia ominaisuuksissa.

## <a name="basics-and-important-notes"></a>Perustiedot ja tärkeitä muistiinpanoja
Azure AD Connect synkronointiin voit ottaa suodattaminen milloin tahansa. Jos aloittaminen directory-synkronoinnin oletusasetukset ja määritä suodatus-objekteja, jotka on suodatettu, enää synkronoida Azure AD. Tämän muutoksen tuloksena Azure AD-objekteja, jotka on synkronoitu aiemmin, mutta valitse on suodatettu poistetaan Azure AD.

Ennen kuin alat muuttuu suodatus, varmista, että olet [ajoitetun tehtävän käytöstä](#disable-scheduled-task) jotta et vahingossa Vie muutokset, joita et ole vielä vahvistanut on oikein.

Suodattaminen poistaa useita objekteja yhtä aikaa, koska haluat varmistaaksesi, että uusi suodattimet ovat oikein, ennen kuin aloitat vieminen Azure AD muutoksia. Kun olet suorittanut määritysvaiheet, on suositeltavaa noudattaa [vahvistus vaiheet](#apply-and-verify-changes) ennen Vie ja tehdä siihen muutoksia Azure AD.

Suojaamaan poistamasta vahingossa useita objekteja, toiminto [estää vahingossa poistaa](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) on oletusarvoisesti käytössä. Jos poistat useita objekteja, koska suodattaminen (500 oletusarvon mukaan), sinun on noudattamalla tämän artikkelin Salli poistaa Siirry Azure AD.

Jos käytössäsi muodosta ennen marraskuun 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) muutosten tekeminen suodattimen määritys ja käytät salasanojen synkronoinnin, sinun on käynnistettävän koko Synkronoi kaikki salasanoja, kun olet suorittanut määritykset. Lisätietoja salasanan koko synkronointi käynnistämisestä on [käynnistimen koko Synkronoi kaikki salasanat](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Jos olet 1.0.9125 tai uudempia, sitten normaali **täyden käyttäjätietojen** laskee myös jos salasanat synkronoidaan ja ylimääräiset tämä vaihe ei enää tarvita.

Jos **käyttäjä** objektit poistettiin vahingossa suodatus virheen vuoksi Azure AD-voit Luo käyttäjän objektit uudelleen Azure AD poistamalla suodatus määritykset ja synkronoida tekemäsi hakemistoja uudelleen. Tämä toiminto palauttaa Azure AD-käyttäjien roskakorista. Kuitenkin ei voi peruuttaa muiden objektityypit. Esimerkiksi jos poistat vahingossa käyttöoikeusryhmän ja sitä on käytetty Käyttöoikeusluettelon resurssin, ryhmän ja sen käyttöoikeusluettelot ei voi palauttaa.

Azure AD Connect poistaa vain objekteja alueella on kerran pidettäviä. Jos nämä objektit eivät ole laajuus Azure AD-objekteja, jotka on luotu toisessa sync engine, lisäämällä suodattaminen ei poista ne. Esimerkiksi jos aloitetaan DirSync-palvelin ja se luotu koko hakemistosta valmis kopion Azure AD ja olet asentanut uuden Azure AD Connect synkronointi palvelimen rinnakkain ja suodatus on käytössä alusta, se ei poista ylimääräiset Dirsync-työkalun luomien objektien.

Suodatus määritysten säilytetään, kun asennat tai uudemman version Azure AD Connect käyttöön. Se on aina parhaiden käytäntöjen varmistaaksesi, että määritystä ei vahingossa muutettu päivityksen jälkeen uudemman version ennen sen suorittamista ensimmäinen synkronointi käy.

Jos sinulla on useampi kuin yksi metsää, suodatus määritykset, joka on kuvattu tämän artikkelin-arvoksi on käytetty jokaisen metsää (olettaen että haluat, että ne kaikki saman määrityskohde).

### <a name="disable-scheduled-task"></a>Poista käytöstä ajoitettu tehtävä
Valmiin päivitysyrityksen, joka käynnistää synkronoinnin jakson 30 minuutin välein käytöstä, toimi seuraavasti:

1. Siirtyminen PowerShell-kehote
2. Suorita `Set-ADSyncScheduler -SyncCycleEnabled $False` voit poistaa ajoitustoiminnon käytöstä.
3. Tee haluamasi muutokset, kuten kuvattu tämän artikkelin.
4. Suorita `Set-ADSyncScheduler -SyncCycleEnabled $True` ajoituksen uudelleen käyttöön.

**Jos käytät Azure AD Connect-muodosta ennen 1.1.105.0**  
Voit poistaa käytöstä ajoitettu tehtävä, joka käynnistää synkronoinnin jakson 3 tunnin välein, toimimalla seuraavasti:

1. **Tehtävien ajoituksen** käynnistäminen Käynnistä-valikosta.
2. Etsi suoraan **Tehtävien ajoituksen kirjasto**-kohdassa tehtäväluetteloon **Azure AD-synkronointi ajoituksen**hiiren kakkospainikkeella ja valitse **Poista käytöstä**.  
![Tehtävien ajoitus](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Voit nyt tekemällä määritysmuutoksia ja näyttää sync engine manuaalisesti **synkronoinnin palvelun hallinta** -konsolin.

Kun olet suorittanut kaikki suodatus muutokset, älä unohda tulee Edellinen- ja **Ota käyttöön** tehtävän uudelleen.

## <a name="filtering-options"></a>Suodatusasetukset
Seuraavanlaisia suodatus määritys voi suojata Directory-synkronointityökalua:

- [**Ryhmän perusteella**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups): suodatuksen perusteella yhden ryhmän vain voidaan määrittää asennuksen ohjatun asennuksen avulla. Se on edelleen kuulu tämän ohjeaiheen.

- [**Toimialueen**](#domain-based-filtering): tämän asetuksen avulla voit valita mitkä toimialueet, jotka synkronoidaan Azure AD. Se myös avulla voit lisätä ja poistaa sync engine määritykset toimialueet, jos teet muutoksia paikallisen infrastruktuurin Azure AD Connect synkronointi asentamisen jälkeen.

- [**Organisaation yksikkö pohjainen**](#organizational-unitbased-filtering): Tämä suodatusasetuksen avulla voit valita joka organisaatioyksiköiden synkronoida Azure AD. Tämä asetus on valittu organisaatioyksiköiden kaikki objektityypit.

- [**Määritteen-pohjainen**](#attribute-based-filtering): tämän asetuksen avulla voit suodattaa objektit objektien määritearvojen perusteella. Voit myös määrittää eri objektityypit eri suodattimet.

Voit käyttää useita suodatusasetusten samanaikaisesti. Esimerkiksi voit OU-pohjainen suodattaminen objektien sisältämään vain yhden OU ja aikaa määrite perustuva suodatusasetukset suodattaminen haluamasi objektit. Kun käytät usealla suodatustoiminto eri tavalla, suodattimet käyttää looginen ja suodattimet välillä.

## <a name="domain-based-filtering"></a>Toimialueen suodattaminen
Tässä osassa on ohjeita voit määrittää toimialueen suodatin. Jos olet lisännyt tai poistanut toimialueiden oman metsää, kun olet asentanut Azure AD Connect, käytössä on myös päivittää suodatus määritykset.

Voit muuttaa toimialueen suodattaminen ensisijainen tapa on suorittamalla ohjattu ja muuta [toimialueen ja suodatuksen organisaatioyksiköiden](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Ohjattu asennus on automaattinen kaikki kuvattu tämän artikkelin tehtävät.

Tee seuraavat toimet vain, jos et jostain syystä ei voi suorittaa ohjattu asennus.

Toimialueen suodatus määritys koostuu seuraavasti:

- [Valitse toimialueet](#select-domains-to-be-synchronized) , jotka sisällytetään synkronointi.
- Lisätyt ja poistetut kunkin toimialueen osalta Säädä [Suorita profiilit](#update-run-profiles).
- [Käytä ja Tarkista muutokset](#apply-and-verify-changes).

### <a name="select-domains-to-be-synchronized"></a>Valitse toimialueet, kun haluat synkronoida
**Voit määrittää toimialueen suodatin, toimi seuraavasti:**

1. Kirjaudu sisään palvelimeen, joka suorittaa Azure AD Connect synkronoinnin käyttämällä tiliä, joka on **ADSyncAdmins** suojaus-ryhmän jäsen.
2. Käynnistä **Synkronointipalvelu** Käynnistä-valikosta.
3. Valitse **yhdistimiä** ja **Yhdysviivat** -luettelossa, valitse yhdistin sisältötyyppi **Active Directory-toimialueen palveluista**. Valitse **Toiminnot**- **Ominaisuudet**.  
![Yhdistimen ominaisuudet](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Valitse **Määritä kansion osiot**.
5. Valitse **Valitse kansio-osiot** -luettelosta ja poista toimialueet tarpeen mukaan. Varmista, että vain osioita, jonka haluat synkronoida on valittu.  
![Osiot](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
Jos olet muuttanut paikallisen oman AD infrastruktuuri- ja lisätty tai poistettu toimialueiden metsää, valitse **Päivitä** -painiketta saat päivitetyn luettelon. Kun päivität, sinua pyydetään tunnistetietoja. Anna tunnistetiedot lukuoikeus paikallisen Active Directory. Se ei ole käyttäjän, joka on valmiiksi-valintaikkunassa.  
![Tarvitaan päivitys](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Kun olet valmis, Sulje **Ominaisuudet** -valintaikkuna valitsemalla **OK**. Jos toimialueet on poistettu metsää, ajattelevat toimialue on poistettu viesti ponnahdusikkuna ja kyseisen määritys Siivotaan.
7. Säädä [Suorita profiilit](#update-run-profiles)edelleen.

### <a name="update-run-profiles"></a>Suorita päivitys-profiileista
Jos olet päivittänyt toimialueen suodatin, voit myös on päivitettävä Suorita-profiileista.

1. **Yhdysviivat** -luettelossa Varmista, että olet muuttanut edellisessä vaiheessa yhdistin on valittuna. Valitse **Toiminnot**, **Suorita-profiileista määrittäminen**.  
![Yhdistimen Suorita profiilit](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

Voit joutua muuttamaan seuraavat profiilit:

- Täysi tuominen
- Täydellinen synkronointi
- Sama.arvo tuonti
- Sama.arvo-synkronointi
- Vie

Kunkin viisi profiilit-toimia **lisätty** kunkin toimialueen osalta seuraavasti:

1. Valitse Suorita profiili ja valitse sitten **Uusi vaihe**.
2. Valitse saman niminen kirjoittamalla vaiheessa **Vaihe määrittäminen** -sivulla **tyyppi** avattavasta, kuin määritetään profiilissa. Valitse sitten **Seuraava**.  
![Yhdistimen Suorita profiilit](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. Valitse **Connector-kokoonpanon määritys** -sivulla **osio** avattavasta, olet lisännyt toimialueen suodattimen toimialueen nimi.  
![Yhdistimen Suorita profiilit](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. Sulje **Määritä Suorita profiili** -valintaikkuna, valitse **Valmis**.

Kunkin viisi profiilit-toimia **poistaa** kunkin toimialueen osalta seuraavasti:

1. Valitse suorituksen profiilia.
2. Jos **osio** -määritteen **arvon** on GUID-tunnus, valitse Suorita vaihe ja valitse sitten **Poista vaihe**.  
![Yhdistimen Suorita profiilit](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

Tulos on oltava, että kunkin toimialueen, jonka haluat synkronoida lueteltujen vaiheessa kunkin suorituksen profiilia.

Sulje **Suorittaminen-profiileista määrittäminen** -valintaikkuna valitsemalla **OK**.

- Viimeistele määritys- [Käytä ja Tarkista muutokset](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Organisaation yksikkö pohjainen suodattaminen
Ensisijainen tapa muuttaa OU perustuva suodatus on suorittamalla ohjattu ja muuta [toimialueen ja suodatuksen organisaatioyksiköiden](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Ohjattu asennus on automaattinen kaikki kuvattu tämän artikkelin tehtävät.

Tee seuraavat toimet vain, jos et jostain syystä ei voi suorittaa ohjattu asennus.

**Voit määrittää organisaation-yksikön pohjainen suodattaminen, toimi seuraavasti:**

1. Kirjaudu sisään palvelimeen, joka suorittaa Azure AD Connect synkronoinnin käyttämällä tiliä, joka on **ADSyncAdmins** suojaus-ryhmän jäsen.
2. Käynnistä **Synkronointipalvelu** Käynnistä-valikosta.
3. Valitse **yhdistimien** ja **Yhdysviivat** -luettelossa, valitse yhdistin sisältötyyppi **Active Directory-toimialueen palveluista**. Valitse **Toiminnot**- **Ominaisuudet**.  
![Yhdistimen ominaisuudet](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Valitse **Määritä kansio-osiot**, valitse määritettävä toimialue ja valitse sitten **säilöt**.
5. Kun sinulta kysytään, tarjota tunnistetietoja paikallisen Active Directory lukuoikeus. Se ei ole käyttäjän, joka on valmiiksi-valintaikkunassa.
6. **Valitse säilöt** -valintaikkunassa Poista organisaatioyksiköiden, joita et halua synkronointi pilven-kansio ja valitse sitten **OK**.  
![ORGANISAATIOYKSIKKÖ](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - Windows 10-tietokoneet synkronoidaan onnistuneesti Azure AD **tietokoneiden** säilössä pitäisi olla valittuna. Jos toimialueelle liitetyissä tietokoneissa on ohjeaiheessa muiden organisaatioyksiköiden, varmista, että ne ovat valittuina.
  - **ForeignSecurityPrincipals** säilö pitäisi olla valittuna, jos sinulla on useita metsien luottamussuhteet kanssa. Tämä säilö avulla toimialueiden väliset ratkaistava käyttöoikeusryhmän jäsenyyden.
  - **RegisteredDevices** OU pitäisi olla valittuna, jos laitteen takaisinkirjoituksen-ominaisuus on otettu käyttöön. Jos käytät toisen takaisinkirjoituksen-ominaisuutta, kuten ryhmän takaisinkirjoituksen Varmista, paikoista valitaan.
  - Valitse muut OU, jossa käyttäjät, iNetOrgPersons, ryhmät, yhteystiedot ja tietokoneiden sijaitsevat. Kuva kaikki näihin sijaitsevat ManagedObjects OU.
7. Kun olet valmis, Sulje **Ominaisuudet** -valintaikkuna valitsemalla **OK**.
8. Viimeistele määritys- [Käytä ja Tarkista muutokset](#apply-and-verify-changes).

## <a name="attribute-based-filtering"></a>Määritteen perustuva suodattaminen
Varmista, että eivät marraskuun 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) tai uudempi luominen, toimi seuraavasti.

Objektien suodattaminen eniten joustava tapa määritteiden perusteella suodatus on. Voit hallita lähes kaikkiin, kun objektin synkronoidaan Azure AD potenssiin [määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning.md) avulla.

Suodatus voi suojata sekä [saapuvien](#inbound-filtering) metaverse Active Directorysta ja [lähtevien](#outbound-filtering) metaverse-Azure AD. On suositeltavaa Ota suodatus käyttöön saapuvien, sillä se on helpointa säilyttää. Lähtevän suodattaminen kannattaa käyttää vain tarvittaessa liittymään useita metsää objektit, ennen kuin laskennan voidaan hallita.

### <a name="inbound-filtering"></a>Saapuva suodattaminen
Saapuvien perusteella suodatus on käytössä oletusarvon määrittäminen missä Azure AD siirtymällä objektit on oltava metaverse-määritteen cloudFiltered ei ole määritetty arvo on synkronoitava. Tämän määritteen arvo on **Tosi**, jos objekti ei ole synkronoitu. Se ei voi määrittää **False** suunniteltu ominaisuus. Varmista, että muut säännöt on mahdollisuus osallistua arvo, tämän määritteen pitäisi vain kopioitavat arvot **Tosi** tai **NULL** (poissa).

Saapuvien suodattaminen käytät **laajuus** power voit määrittää, mitkä objektit on tai ei voi synkronoida. Tämä on missä vastaamaan oman organisaation vaatimusten muutosten tekeminen. Laajuus-moduuli on **ryhmän** ja **lauseen** määrittämään, jos Synkronoi säännön on oltava alueen. **Ryhmän** sisältää yhden tai usean **lause**. On looginen ja useita lausekkeita ja loogisten tai useita ryhmiä välillä.

Tarkista uutiskirjeistä Esimerkki:  
![Laajuuden](./media/active-directory-aadconnectsync-configure-filtering/scope.png) tämä olisi luetaan **(osasto = IT) tai (osasto = myynti-ja c = US)**.

Ohjeita ja esimerkkejä, joiden käyttäjäobjekti käytetään esimerkkinä, mutta voit käyttää tätä objektia tyypeissä.

Alla olevia malleja järjestys-arvon aloitetaan 500. Tämän arvon varmistaa seuraavat säännöt käsitellään ruutuun sääntöjen (pienet järjestys, suuremmat numeeriset arvot) jälkeen.

#### <a name="negative-filtering-do-not-sync-these"></a>Negatiivinen suodattaminen, "ei synkronoida ne"
Seuraavassa esimerkissä, voit suodattaa pois (ei synkronoida) kaikille käyttäjille, joiden **extensionAttribute15** arvo **NoSync**.

1. Kirjaudu sisään palvelimeen, joka suorittaa Azure AD Connect synkronoinnin käyttämällä tiliä, joka on **ADSyncAdmins** suojaus-ryhmän jäsen.
2. Aloita **Synkronointi säännöt editorin** Käynnistä-valikosta.
3. Varmista, että **saapuvien** on valittuna ja valitse **Lisää uusi sääntö**.
4. Anna säännölle nimi, kuten "*-AD – käyttäjän DoNotSyncFilter-*". Valitse oikea metsää **käyttäjän** **CS objektityyppi**, ja **henkilö** **ma objektilaji**. **Linkkityyppi**Valitse **liittyä** ja kirjoita ohittaa arvo, joka on tällä hetkellä ole käytössä toisessa synkronoinnin sääntö (esimerkiksi 500) ja valitse sitten **Seuraava**.  
![Saapuvien 1 kuvaus](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. **Scoping suodatin** **Lisää**ryhmä, valitse **Lisää lause**ja valitse määrite **ExtensionAttribute15**. Varmista, että operaattori on **yhtä suuri kuin** ja kirjoita arvo **NoSync** arvo-ruutuun. Valitse **Seuraava**.  
![Saapuva 2 laajuus](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. **Liittyminen** sääntöjä jättää tyhjäksi ja valitse sitten **Seuraava**.
7. Valitse **Lisää muunnos** **FlowType-kohteen** , **Vakio**, valitse kohde-määritteen **cloudFiltered** ja kirjoita lähde-ruutuun **Tosi**. Valitse **Lisää** Tallenna sääntö.  
![Saapuva 3 muunnos](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Viimeistele määritys- [Käytä ja Tarkista muutokset](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Suodattaminen, "synkronoida ne vain" positiivinen
Ilmoittaminen positiivinen suodattaminen voi olla hankalaa enemmän, koska sinulla kannattaa myös objekteja, jotka eivät ole ilmeisimmät synkronoitavaksi, kuten neuvotteluhuoneiden.

Positiivinen suodatusasetuksen edellyttää kahta synkronointi säännöt. Yhden (tai useamman) kanssa oikea laajuuden synkronoimaan objektit ja toisen todellisen kaikki synkronointi sääntö, kaikki objektit, joka ei ole vielä määritetty objektina, jotka synkronoidaan suodatin.

Seuraavassa esimerkissä voit synkronoida vain käyttäjän objektien osasto-määrite, joiden on **Myynti**.

1. Kirjaudu sisään palvelimeen, joka suorittaa Azure AD Connect synkronoinnin käyttämällä tiliä, joka on **ADSyncAdmins** suojaus-ryhmän jäsen.
2. Aloita **Synkronointi säännöt editorin** Käynnistä-valikosta.
3. Varmista, että **saapuvien** on valittuna ja valitse **Lisää uusi sääntö**.
4. Anna säännön kuvaava nimi, esimerkiksi "*--AD – käyttäjän myynti synkronoida*". Valitse oikea metsää **käyttäjän** **CS objektityyppi**, ja **henkilö** **ma objektilaji**. **Linkkityyppi**valitsemalla **Liity** ja kirjoita ohittaa arvo, joka on tällä hetkellä ole käytössä toisessa synkronoinnin sääntö (esimerkiksi 501) ja valitse sitten **Seuraava**.  
![Saapuva 4 kuvaus](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. **Scoping suodatin** **Lisää**ryhmä, valitse **Lisää lause**ja valitse määrite **Osasto**. Varmista, että operaattori on **yhtä suuri kuin** ja kirjoita arvoruutuun **Myynti** . Valitse **Seuraava**.  
![Saapuva 5 laajuus](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. **Liittyminen** sääntöjä jättää tyhjäksi ja valitse sitten **Seuraava**.
7. Valitse **Lisää muunnos** **FlowType-kohteen** , **Vakio**, valitse kohde-määritteen **cloudFiltered** ja kirjoita lähde-tekstiruutuun **Epätosi**. Valitse **Lisää** Tallenna sääntö.  
![Saapuva 6 muunnos](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
Tämä on erityisen tapauksen kohtaa, johon voit määrittää cloudFiltered eksplisiittisesti EPÄTOSI.

    Microsoft on nyt todellisen kaikki synkronointi-säännön luominen.

8. Anna säännölle nimi, kuten "*-AD – käyttäjän todellisen all-suodatin-*". Valitse oikea metsää **käyttäjän** **CS objektityyppi**, ja **henkilö** **ma objektilaji**. **Linkkityyppi**Valitse **liittyä** ja käsittelyjärjestyksessä kirjoittamalla arvon, joka on tällä hetkellä ole käytössä toisessa synkronoinnin sääntö (esimerkiksi 600). Sinulla on valittuna järjestys arvo (pienet laskettavat) edellisen synkronoinnin ryhmälle mutta vasemmalle myös tilaa, jotta olemme voit lisätä suodatus synkronointi sääntöjen myöhemmin, kun haluat aloittaa synkronoinnin muita osastot. Valitse **Seuraava**.  
![Saapuva 7 kuvaus](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. **Scoping suodatin** jättää tyhjäksi ja valitse **Seuraava**. Tyhjä suodatin osoittaa sääntöä käytetään kaikissa objekteissa.
10. **Liittyminen** sääntöjä jättää tyhjäksi ja valitse sitten **Seuraava**.
11. Valitse **Lisää muunnos** **FlowType-kohteen** , **Vakio**, valitse kohde-määritteen **cloudFiltered** ja kirjoita lähde-ruutuun **Tosi**. Valitse **Lisää** Tallenna sääntö.  
![Saapuva 3 muunnos](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Viimeistele määritys- [Käytä ja Tarkista muutokset](#apply-and-verify-changes).

Jos haluat, voit luoda sääntöjen kohtaa, johon lisäät enemmän objekteja Microsoftin synkronoinnin ensimmäisen tyyppi.

### <a name="outbound-filtering"></a>Lähtevän suodattaminen
Joissakin tapauksissa on suoritettava suodattaminen vasta, kun objektit ovat liittyneet metaverse. Se voi olla esimerkiksi pakollinen sähköposti-määritteen tarkastella resurssin metsää ja määrittämään, jos objektin synkronoidaan tilin-metsää userPrincipalName-määritteen. Tällöin voit luoda lähtevän säännön suodatuksen.

Tässä esimerkissä muutat suodatus niin vain käyttäjät, jossa sähköposti- ja userPrincipalName pääty @contoso.com synkronoidaan:

1. Kirjaudu sisään palvelimeen, joka suorittaa Azure AD Connect synkronoinnin käyttämällä tiliä, joka on **ADSyncAdmins** suojaus-ryhmän jäsen.
2. Aloita **Synkronointi säännöt editorin** Käynnistä-valikosta.
3. Valitse **Säännöt tyyppi**-kohdasta **Lähtevä**.
4. Etsi säännön, jonka nimi **AAD – käyttäjän liittyä SOAInAD ulos**. Valitse **Muokkaa**.
5. Valitse Ponnahdusvalikossa kohdan vastaa **Kyllä** Jos haluat luoda säännön.
6. Muuta **kuvaus** -sivulla ohittaa käyttämättömät arvon, esimerkiksi 50.
7. Valitse vasemmanpuoleisessa siirtymisruudussa **Scoping suodatin** . Valitse **Lisää lause**, määrite Valitse **Posti**, operaattori Valitse **ENDSWITH**ja arvon tyyppi **@contoso.com**. Valitse **Lisää-lause**, määrite Valitse **userPrincipalName**operaattori Valitse **ENDSWITH**ja arvon tyyppi **@contoso.com**.
8. Valitse **Tallenna**.
9. Viimeistele määritys- [Käytä ja Tarkista muutokset](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Käytä ja muutosten tarkistaminen
Kun olet tehnyt muutokset määritykset, nämä on otettava käyttöön jo ole järjestelmässä objekteihin. Voi myös, että objekteja ei tällä hetkellä Synkronoi-ohjelma käsiteltävä ja synkronoi-ohjelma on lukemaan lähdejärjestelmän uudelleen ja varmista, että sen sisältöä.

Jos olet muuttanut konfigurointi käyttämällä **toimialueen** tai **Organisaatioyksikkö –** suodatus, sinun on suoritettava **täysi tuominen** **Delta synkronoinnin**perään.

Jos olet muuttanut määritysten **määrite** suodatuksen avulla, sinun on suoritettava **täyden käyttäjätietojen**.

Tee seuraavat toimet:

1. Käynnistä **Synkronointipalvelu** Käynnistä-valikosta.
2. **Yhdistimien** ja valitse yhdistin, johon teit määritykset, muuttaa aiemmin **Yhdysviivat** -luettelosta. **Toiminnot**Valitse **Suorita**.  
![Suorita yhdistin](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. Valitse **Suorita profiilit**toiminnon mainittu edellisessä osassa. Jos haluat suorittaa kaksi tehtävää, suorittamalla toinen ensimmäisenä valmistuttua ( **Osavaltio** -sarakkeen on **vapaa** valitun yhdistimen).

Kun synkronointia, kaikki muutokset vaiheistettu viedään. Ennen kuin teet muutoksia itse asiassa Azure AD-haluat varmistaa, että nämä muutokset ovat oikein.

1. Käynnistä cmd kehote ja siirry`%Program Files%\Microsoft Azure AD Sync\bin`
2. Suorita:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Yhdistimen nimi löytyy synkronointipalvelu. Siinä on samankaltainen kuin "contoso.com – AAD" nimi Azure AD varten.
3. Suorita:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. % Temp % nimeltä export.csv, joka voidaan tutkia Microsoft Excel-tiedosto on nyt. Tiedoston nimi sisältää kaikki muutokset, joita voi viedä.
5. Tee tarvittavat muutokset tietojen tai määritysten ja Suorita nämä vaiheet uudelleen (tuominen, synkronoi ja tarkista), kunnes haluamasi muutokset, joita voi viedä ovat oikein.

Kun olet tyytyväinen, vieminen Azure AD muutokset.

1. **Yhdistimien** ja valitse **Yhdysviivat** -luettelosta Azure AD-yhdistin. **Toiminnot**Valitse **Suorita**.
2. Valitse **Suorita profiilit** **Vie**.
3. Jos määritysten muutokset poistaa useita objekteja, valitse näet virhesanoman viennin kun arvo on suurempi kuin määritetty raja-arvon (oletuksena 500). Jos näet tämän virheen, sinun on väliaikaisesti pois käytöstä ominaisuus [estää vahingossa poistaa](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md).

Kun on aika ajoituksen uudelleen käyttöön.

1. **Tehtävien ajoituksen** käynnistäminen Käynnistä-valikosta.
2. Etsi suoraan **Tehtävien ajoituksen kirjasto**-kohdassa tehtäväluetteloon **Azure AD-synkronointi ajoituksen**hiiren kakkospainikkeella ja valitse **Ota käyttöön**.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [Azure AD Connect synkronointi](active-directory-aadconnectsync-whatis.md) -määritys.

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
